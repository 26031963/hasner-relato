# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 03:40** · publicado em **31/10/2026 03:41** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 03:40 · sessao 6c4eeff007e0 · O261-E-F5C · verificador · origem=Code

A cura separa corretamente **verbo** de **pertencimento à fila**, mas ainda há falhas de custo, garantias textuais excessivas e cobertura incompleta dos testes. (`app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1158`, `app/chamados/services/vigia_fio.py:549`, `app/colaboradores/tests/test_ficha_do_colab.py:110`)

1. **Quem escreve e quem lê o dict**

   | Chave | Escritor | Todos os leitores diretos encontrados |
   |---|---|---|
   | `verbo` | `particionar_verbos(qs_vivos())`, `app/chamados/juizes.py:1158` | Fila, âncora e seleção dos números visíveis: `app/chamados/juizes.py:1224`, `:1228`, `:1232`. |
   | `fila_ids` | Inicialização e inclusão: `app/chamados/juizes.py:1148`, `:1151`; retorno: `:1158` | Porta da pergunta: `app/chamados/juizes.py:1227`; vigia: `app/chamados/services/vigia_fio.py:566`, usado em `:619`. |
   | `fila_cel` | `app/chamados/juizes.py:1149`, `:1153` | Porta da fila: `app/chamados/juizes.py:1223`. |
   | `vivas` | `app/chamados/juizes.py:1154`, `:1157` | Porta: `app/chamados/juizes.py:1225`; vigia: `app/chamados/services/vigia_fio.py:571`, `:576`, `:595`, `:616`. |
   | `colab` | `app/chamados/juizes.py:1159` | Identificação do colaborador para consultar o app: `app/chamados/juizes.py:1227`. |
   | `no_app` | Vazio em `app/chamados/juizes.py:1159`; preenchimento lazy em `:1167` | `_no_app`: `app/chamados/juizes.py:1163`, `:1168`; chamadas pela porta em `:1227` e pelo vigia em `app/chamados/services/vigia_fio.py:580`. |

   A folha e as pautas transportam a alimentação ao juiz; o teste de equivalência também usa alimentação real. (`app/folha/fora_da_folha.py:53`, `:58`, `:138`, `:190`; `app/ponto/services/pautas_dos_discordantes.py:74`, `:76`; `app/chamados/tests/test_o261e_f5b_juiz_porta.py:187`)

   Não encontrei leitor atual desse dict usando `verbo` como conjunto da fila: os dois pontos relevantes agora usam `fila_ids`. Também não encontrei teste construindo manualmente a **alimentação**; os dicts manuais e mocks encontrados representam a **saída da porta**. (`app/chamados/juizes.py:1227`; `app/chamados/services/vigia_fio.py:566`; `app/folha/tests/test_haiku_fora_da_folha.py:101`, `:129`, `:143`; `app/chamados/tests/test_o261e_f5_alcance.py:306`)

   **Sobrou uma referência de teste errada:** `_verbo_do_painel` do teste do vigia continua particionando `qs_na_fila()` e chamando isso de verbo do painel. Seus casos simples não expõem o defeito curado. (`app/chamados/tests/test_o261e_f5_alcance.py:281`, `:285`, `:287`)

2. **Igualdade com painel e badge**

   **Painel padrão, sem filtros, em estado estável: sim, o motor recebe o mesmo universo.** O painel monta a base sem filtros e particiona seus vivos; o juiz particiona `qs_vivos()` diretamente. `select_related`, prefetch, anotações e ordenação não introduzem outro recorte; a partição remove o prefetch antes de ler suas colunas. (`app/chamados/services/painel.py:96`, `:135`, `:198`, `:228`; `app/chamados/juizes.py:1158`; `app/chamados/services/verbos.py:44`)

   **Com filtros, pode divergir.** Contraexemplo deduzido: C normal; D urgente e vivo; pergunta respondida com destino D e pai C. Globalmente, D recebe a pergunta e C fica `decidir`. Filtrando urgência normal, D sai do conjunto e o crédito volta a C, que vira `validar`. O crédito prefere o destino somente quando ele pertence ao conjunto recebido. Ocorrência desse cenário em produção: `[nao verificado]`. (`app/chamados/services/painel.py:117`, `:228`; `app/chamados/services/raiox.py:54`, `:56`, `:140`; `app/chamados/catalogo/natureza.py:48`)

   **Empresas visíveis não restringem o painel padrão:** alimentam os seletores. O vigia restringe os dias a essas empresas, mas a alimentação continua particionando todos os vivos. (`app/chamados/services/painel.py:96`, `:127`, `:436`; `app/chamados/services/vigia_fio.py:534`; `app/chamados/juizes.py:1158`)

   **O badge não garante igualdade no instante atual:** normalmente devolve `PlacarNavbar.badge_chamados`; somente sem lavra usa a partição atual. Em outra exceção, devolve simplesmente `qs_na_fila().count()`. Além disso, mostra uma soma de `validar` e `decidir`, sem verbo individual por chamado. (`app/core/context_processors.py:68`, `:84`, `:88`, `:93`; `app/chamados/services/verbos.py:22`, `:25`)

   `registrado` continua vivo e pode receber crédito, embora não abra porta nem apareça na lista padrão. Silêncio vencido pode abrir porta com verbo `historico`; silêncio vigente fica fora da fila. Verbo `None` pode abrir porta para chamado aberto/em análise de auditoria, produzir fio `chamado` e aparecer no painel como `Registro`. (`app/chamados/catalogo/motor.py:24`, `:29`; `app/chamados/services/painel.py:187`, `:314`; `app/chamados/services/verbos.py:48`, `:125`; `app/chamados/juizes.py:1224`, `:1232`; `app/chamados/catalogo/natureza.py:44`, `:63`)

3. **Pertencimento a `qs_na_fila`**

   Com alimentação real e estado estável, **as duas portas continuam exigindo fila**: `fila_cel` e `fila_ids` nascem da mesma iteração de `qs_na_fila`; a primeira porta usa `fila_cel`, a segunda exige `anc in fila_ids`. Não encontrei passagem estável de âncora fora da fila. (`app/chamados/juizes.py:1150`, `:1153`, `:1223`, `:1227`)

   Uma âncora na fila pode legitimamente não abrir a porta da pergunta: ainda são necessárias pergunta viva, presença no app e resposta não vazia quando respondida. A âncora é o destino; o pai só serve quando não existe destino. (`app/chamados/juizes.py:1155`, `:1227`; `app/chamados/services/fio.py:116`)

   **Há uma fragilidade de consistência:** fila e mapa são lidos separadamente, sem revalidação da alimentação. Se o chamado deixar de ser vivo entre essas leituras, a porta da fila pode receber `None` pelo `.get()`, enquanto a porta da pergunta pode lançar `KeyError` pelo acesso direto. Ocorrência em produção: `[nao verificado]`. (`app/chamados/juizes.py:1150`, `:1158`, `:1222`, `:1224`, `:1228`)

4. **A chave `fila` pode nomear chamado oculto no painel padrão?**

   **Não encontrei esse caso em estado estável e sem filtros.** O juiz exige pertencimento à fila e remove `FORA_DA_LISTA_PADRAO`; o painel exige `COBRANCA` e remove a mesma constante. Paginação pode deixar o chamado fora da página corrente, mas não o elimina do conjunto. (`app/chamados/juizes.py:1150`, `:1232`; `app/chamados/services/painel.py:314`, `:326`, `:340`)

   Silêncio vencido conserva a porta, mas perde o número em `fila`; silêncio vigente sequer abre a porta. Isso está coberto pelo teste correspondente. (`app/chamados/services/painel.py:188`; `app/chamados/services/verbos.py:51`; `app/chamados/tests/test_o261e_f5b_juiz_porta.py:144`)

5. **Checklist das classes de reprovação**

   - **Segundo juiz:** a porta foi registrada e a folha lê seu `fio`. `pergunta_sem_acao_possivel` responde outra pergunta, conforme a ressalva posterior do corte; mantê-la não demonstra duplicação da porta do dia. (`app/core/juizes.py:253`; `app/folha/fora_da_folha.py:67`; `app/chamados/services/raiox.py:151`; `app/docs/BACKLOG.md:778`)
   - **Segundo escritor:** os trechos novos de alimentação/porta escrevem mapas em memória; o command publica pelo carimbador existente. Não identifiquei nova escrita de estado de domínio nesses trechos. (`app/chamados/juizes.py:1148`, `:1167`, `:1233`; `app/chamados/management/commands/vigiar_fio_morto.py:206`)
   - **Relógio fora da borda:** o command captura `agora`, mas passa apenas `hoje`; `qs_na_fila` e `particionar_verbos` continuam lendo relógios próprios. Portanto a rodada não tem um instante único para silêncio/verbos. Os testes congelados não exercitam essa travessia. (`app/chamados/management/commands/vigiar_fio_morto.py:162`, `:167`; `app/chamados/services/painel.py:188`; `app/chamados/services/verbos.py:35`; `app/chamados/tests/test_o261e_f5b_juiz_porta.py:28`)
   - **Derivação paralela:** a tupla de verbos ocultos saiu dos leitores de produção e ficou na constante compartilhada. A permissão do Resolver dia, porém, continua copiada em literal no vigia e no drawer; o teste verifica coincidência textual, sem transformar isso em fonte única. (`app/chamados/services/verbos.py:18`; `app/chamados/services/painel.py:327`; `app/colaboradores/services/ficha.py:160`; `app/chamados/services/vigia_fio.py:184`; `app/colaboradores/services/drawer.py:130`; `app/chamados/tests/test_o261e_f5_alcance.py:357`)
   - **Mordida da cura principal:** os novos casos distinguem a regressão para `particionar_verbos(qs_na_fila())`: nesse mutante C recebe a resposta e deixa de ser `decidir`. O teste da âncora silenciada distingue a troca indevida de `fila_ids` por `verbo`. Essa conclusão é estática; execução Django/RED nesta revisão: `[nao verificado]`. (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:192`, `:213`, `:226`, `:236`)
   - **Selo fraco da ficha:** o mutante `verbo in FORA_DA_LISTA_PADRAO[:1]` passa nas três asserções AST e classifica `historico` como `registro`. Conferi essas expressões em memória: o teste exige presença do nome e ausência do literal, mas não exige uso integral da constante. (`app/colaboradores/services/ficha.py:160`; `app/colaboradores/tests/test_ficha_do_colab.py:108`, `:110`, `:111`, `:112`)
   - **Ordem de trava:** não identifiquei aquisição nova de trava nos trechos acrescentados; o command compartilha a lista e lê sua falha depois dos dois censos. (`app/chamados/juizes.py:1129`; `app/chamados/services/vigia_fio.py:187`; `app/chamados/management/commands/vigiar_fio_morto.py:166`)
   - **Pré-requisito:** o código consumidor e o juiz estão presentes no pouso fornecido, mas a documentação contradiz a ordem: a linha diz “ANTES da F5” e termina “depois da F5”. A sequência real de pousos, suite e opus não está demonstrada pelos anexos: `[nao verificado]`. (`app/docs/BACKLOG.md:777`; `app/docs/CORTES.md:400`)

6. **Custo**

   **Existe multiplicação comprovável no vigia:** para K pares empresa×competência, `censo_alcance` chama K vezes `fora_da_folha`, que alimenta o juiz; depois alimenta novamente. Havendo dias retidos, são **K+1 partições globais**, inclusive alimentações de lotes vazios. (`app/chamados/services/vigia_fio.py:546`, `:549`, `:565`; `app/folha/fora_da_folha.py:138`; `app/chamados/juizes.py:1146`, `:1158`)

   **Também há consultas por pergunta dentro da alimentação:** o carregamento inclui somente `disputa` e `celula`, mas C6 acessa `disputa.colaborador`, empresa e tranca. Fora de request, o memo pertence à instância do colaborador, permitindo repetir consultas. (`app/chamados/juizes.py:1155`, `:1008`, `:888`, `:896`, `:900`, `:919`; `app/ponto/services/fechamento.py:1438`)

   `_no_app` acrescenta consultas por colaborador; as perguntas do app carregam apenas `disputa`, enquanto o juiz acessa também `celula`, abrindo outro caminho de N+1. (`app/chamados/juizes.py:1163`, `:757`; `app/chamados/services/pergunta_no_app.py:42`)

   Não identifiquei chamada de produto por célula/request; os caminhos encontrados alimentam lotes em vigia, lavrador e pautas. Tempo e quantidade efetiva de consultas em produção: `[nao verificado]`. (`app/chamados/services/vigia_fio.py:565`; `app/folha/management/commands/lavrar_retrato_folha.py:54`; `app/ponto/services/pautas_dos_discordantes.py:74`; `app/folha/views_retrato.py:8`)

7. **Textos que o código não garante**

   - **“Consultas por lote, nunca por dia”** e **“UMA vez por rodada”** excedem as garantias, pelos acessos de C6 e pelas alimentações indiretas acima. (`app/chamados/juizes.py:1130`, `:888`; `app/chamados/services/vigia_fio.py:514`, `:549`; `app/folha/fora_da_folha.py:138`)
   - **“Mesmo universo do painel e do badge”** precisa distinguir painel sem filtros, painel filtrado e badge lavrado. (`app/chamados/juizes.py:1136`, `:1206`; `app/chamados/services/vigia_fio.py:173`; `app/core/context_processors.py:68`)
   - **“Pergunta sem crédito deixa a âncora em decidir”** não vale universalmente: auditoria pode dar `None`, geofence dá `validar`, outra pergunta pode fornecer crédito e silêncio pode sobrepor `historico`. (`app/chamados/juizes.py:1211`; `app/chamados/catalogo/natureza.py:44`, `:46`, `:48`; `app/chamados/services/verbos.py:51`)
   - **`DESTRAVA['admin']` promete resposta na mesa de disputa**, mas um chamado `gps_fora_geofence` da célula pode produzir `validar`/fio `admin` sem pergunta. O texto não acompanha esse contrato mais amplo. (`app/folha/fora_da_folha.py:41`; `app/chamados/catalogo/natureza.py:46`, `:59`; `app/chamados/juizes.py:1175`, `:1224`)
   - **“Ninguém está esperando ninguém”** também é excessivo: sem essas duas portas ainda pode haver decisão pendente, ausência aberta ou mesa do admin. (`app/folha/fora_da_folha.py:46`; `app/chamados/juizes.py:1214`; `app/chamados/services/vigia_fio.py:288`, `:291`, `:295`)
   - Restaram descrições antigas: grupo C como “chamado vivo sem pergunta viva” e folha julgando diretamente por `.viva`; o leitor atual recebe o fio da porta. (`app/folha/fora_da_folha.py:10`, `:67`; `app/chamados/services/vigia_fio.py:129`)

## 31/10 03:39 · sessao e426ca6c50df · - · - · origem=Ronald (tmux)

Custo confirmado por leitura estática:

- **Partição global repetida:** cada `alimentar_portas` particiona `qs_vivos()` inteiro, mesmo com lote vazio (`app/chamados/juizes.py:1146`, `:1158`). `censo_alcance` chama `fora_da_folha` por empresa×competência (`app/chamados/services/vigia_fio.py:546`), cada chamada alimenta (`app/folha/fora_da_folha.py:138`), e depois alimenta novamente (`app/chamados/services/vigia_fio.py:565`). São K+1 partições quando há dias retidos, sendo K os pares empresa×competência.
- **N+1 dentro da alimentação:** carrega apenas `disputa` e `celula` (`app/chamados/juizes.py:1155`), mas C6 acessa `disputa.colaborador` (`:888`) e `colab.empresa` (`:896`). Fora de request, o memo pertence à instância do colaborador (`:919`), podendo repetir também `periodo_trancado` (`:900`; `app/ponto/services/fechamento.py:1438`).
- **Consultas posteriores:** `_no_app` faz consulta do colaborador e das perguntas por colaborador (`app/chamados/juizes.py:1163`; `app/chamados/services/pergunta_no_app.py:42`). Estas últimas carregam só `disputa`, enquanto C6 acessa `celula` (`app/chamados/juizes.py:757`), introduzindo consultas por pergunta.
- **Não identifiquei chamador de produção por célula ou request** na busca dos símbolos; caminhos encontrados são vigia, lavrador e pautas. A tela declara ler o retrato gravado (`app/folha/views_retrato.py:8`); lavrador chama por empresa×competência (`app/folha/management/commands/lavrar_retrato_folha.py:54`, `:65`; `app/folha/retrato_folha.py:1157`); pautas alimenta por empresa (`app/ponto/services/pautas_dos_discordantes.py:66`, `:74`). Tempo e contagem observados em produção: `[nao verificado]`.

Textos/docstrings que excedem a garantia:

- **“Consultas por lote, nunca por dia”** (`app/chamados/juizes.py:1130`) não descreve os acessos de C6 e `_no_app` acima.
- **“Alimentado UMA vez por rodada”** (`app/chamados/services/vigia_fio.py:514`) ignora as alimentações indiretas por empresa×competência (`:549`; `app/folha/fora_da_folha.py:138`).
- **`DESTRAVA['admin']` promete validar resposta na disputa** (`app/folha/fora_da_folha.py:41`), mas `gps_fora_geofence` recebe `validar` independentemente de pergunta (`app/chamados/catalogo/natureza.py:29`, `:46`, `:59`); chamado desse módulo ligado à célula abre porta e produz fio `admin` (`app/chamados/juizes.py:1223`, `:1175`).
- **Pergunta sem crédito não garante âncora em `decidir`**, como afirma `app/chamados/juizes.py:1211`: auditoria pode resultar em `None`, geofence em `validar`, outra pergunta creditada em `cobrar`/`validar`, e silêncio em `historico` (`app/chamados/catalogo/natureza.py:44`; `app/chamados/services/verbos.py:47`).
- **`DESTRAVA['nada']` diz “ninguém está esperando ninguém”** (`app/folha/fora_da_folha.py:46`), embora ausência de porta ainda possa corresponder a decisão pendente, ausência aberta ou mesa do admin (`app/chamados/services/vigia_fio.py:288`, `:609`). O próprio juiz exclui essas outras entradas do conceito de porta (`app/chamados/juizes.py:1214`).
- **Prosa residual desatualizada:** grupo C ainda é descrito como “chamado vivo sem pergunta viva” (`app/folha/fora_da_folha.py:10`); vigia ainda diz que folha julga via `.viva` (`app/chamados/services/vigia_fio.py:129`), mas o leitor atual lê `porta['fio']` (`app/folha/fora_da_folha.py:67`).

## 31/10 03:38 · sessao df5e7c1b8947 · - · - · origem=Ronald (tmux)

Conclusões dos itens 2 e 4:

- **Sem filtros e no mesmo estado/instante, o motor é igual:** juiz usa `particionar_verbos(qs_vivos())`; painel usa `particionar_verbos(qs_vivos(_base=montar_qs_filtrado(...)))`. Sem filtros, as bases coincidem. `select_related`, `prefetch_related`, ordenação e anotações não acrescentam filtro; a partição remove prefetch e lê colunas. (`app/chamados/juizes.py:1158`; `app/chamados/services/painel.py:96-155,198,228`; `app/chamados/services/verbos.py:34-46`)

- **Filtros podem mudar o verbo do mesmo chamado:** C normal na fila; D urgente vivo; pergunta respondida com destino D e disputa.chamado=C. Global: C=`decidir`, D=`validar`. Com `urgencia=normal`, D sai do universo e o crédito volta para C, que vira `validar`. É consequência dos filtros antes da partição e da preferência pelo destino somente se pertencer ao conjunto recebido. O caso é deduzido do código; ocorrência em produção `[nao verificado]`. (`app/chamados/services/painel.py:117-130,228`; `app/chamados/services/raiox.py:54-56,133-149`; `app/chamados/catalogo/natureza.py:48-64`)

- **Empresas visíveis não filtram a lista padrão nem seu universo de verbos:** servem aos dropdowns. Empresa inativa/de diagnóstico pode entrar no universo global. O vigia, por sua vez, recorta seu universo de dias a empresas visíveis, sem limitar a partição global do juiz. (`app/colaboradores/queries.py:78-87`; `app/chamados/services/painel.py:96-132,436,542`; `app/chamados/services/vigia_fio.py:535-539`; `app/chamados/juizes.py:1158`)

- **O badge normalmente não particiona ao vivo:** devolve `PlacarNavbar.badge_chamados`; só sem lavra usa a partição atual, e em outra exceção devolve a quantidade em `qs_na_fila`. A lavra vem do painel sem filtros e soma apenas `validar`+`decidir`. Portanto igualdade temporal com juiz/painel não é garantida. (`app/core/context_processors.py:68-93,110-114`; `app/core/management/commands/lavrar_badge_navbar.py:39-41,72-74,91-93`; `app/chamados/services/verbos.py:22-27`)

- **Registrado/silêncio:** `registrado` permanece no universo vivo e pode receber crédito, mas sai tanto da porta quanto da lista padrão; silêncio futuro sai da fila, passado entra na fila mas recebe `historico`, depois removido de `fila` e da lista padrão. (`app/chamados/catalogo/motor.py:24-29`; `app/chamados/services/painel.py:155,186-188,314,326-329`; `app/chamados/services/verbos.py:48-51`; `app/chamados/juizes.py:1232`)

- **Verbo `None` não é ausência de porta:** chamado de auditoria aberto/em análise entra na fila, pode aparecer em `fila`, e produz fio `chamado`. Painel mostra `Registro`; resumo transforma `None` em `registro`; badge não o conta como admin. Isso é coerente com a implementação, mas não autoriza tratar todos os verbos de porta como ações. (`app/chamados/catalogo/natureza.py:18-23,44-45,63`; `app/chamados/juizes.py:1224,1232-1234`; `app/chamados/services/painel.py:381-386,472-484`; `app/chamados/services/verbos.py:125-135,22-27`)

- **Item 4:** em estado estável, não encontrei número de `fila` eliminado pelas regras da lista padrão sem filtros. `fila` exige cobrança e remove os mesmos verbos ocultos; painel exige cobrança e remove a mesma constante. Paginação e grupos inicialmente recolhidos podem impedir exibição imediata, sem eliminar o chamado do conjunto. Concorrência entre consultas/requests não tem snapshot garantido. (`app/chamados/juizes.py:1150-1158,1223,1232`; `app/chamados/services/painel.py:314,326-329,340-341,496-497`; `app/templates/chamados/partials/_lista_resumo.html:57-61`)

Falhas de prova/texto:

- O auxiliar de teste do vigia **continua chamando a fila de universo do painel**; seus casos simples não expõem a divergência curada. (`app/chamados/tests/test_o261e_f5_alcance.py:281-299`)
- Os novos testes do juiz comparam outro chamado ao mesmo motor global, sem exercitar `montar_contexto_painel` nem o badge real. Mordem a regressão para `qs_na_fila`, mas não provam a promessa ampla de tela/badge. (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:87-92,192-224`)
- A afirmação “pergunta viva que o crédito não vê deixa o verbo da âncora em decidir” é excessiva: auditoria dá `None`; geofence dá `validar`; silêncio passado pode sobrepor `historico`; outras perguntas creditadas à âncora também alteram o verbo. (`app/chamados/juizes.py:1211`; `app/chamados/catalogo/natureza.py:44-51`; `app/chamados/services/verbos.py:48-51`)
- As docstrings que dizem “mesmo universo do painel e badge” precisam restringir painel sem filtros e distinguir o badge lavrado do fallback vivo. (`app/chamados/juizes.py:1135-1139,1206-1208`; `app/chamados/services/vigia_fio.py:172-173`; `app/core/context_processors.py:68-93`)

## 31/10 03:38 · sessao c5fa694cf9ae · - · - · origem=Ronald (tmux)

Contrato de `alimentar_portas` — escritor inicial único: `app/chamados/juizes.py:1148` a `:1159`; leitores por chave:

| Chave | Escrita | Leitura direta |
|---|---|---|
| `verbo` | `particionar_verbos(qs_vivos())['mapa']`, `juizes.py:1158` | `porta_alcancavel_do_dia`: `.get()` na fila, `:1224`; índice na âncora, `:1228`; filtro do número visível, `:1232` |
| `fila_ids` | inicializa/adiciona em `juizes.py:1148`, `:1151`; retorna `:1158` | pertinência da âncora, `juizes.py:1227`; `censo_alcance`, `vigia_fio.py:566`, usado para véu em `:619` |
| `fila_cel` | `juizes.py:1149`, `:1153`, `:1158` | `porta_alcancavel_do_dia`, `juizes.py:1223` |
| `vivas` | `juizes.py:1154` a `:1158` | porta, `juizes.py:1225`; vigia, `vigia_fio.py:571`, com usos em `:576`, `:595`, `:616` a `:645` |
| `colab` | `juizes.py:1159` | porta consulta colaborador da célula em `juizes.py:1227` |
| `no_app` | inicial vazio, `juizes.py:1159`; cache lazy `_no_app`, `:1167` | `_no_app`, `juizes.py:1163`, `:1168`; chamados em porta `:1227` e vigia `vigia_fio.py:580` |

Passadores do contrato, sem acesso direto a suas chaves: `folha/fora_da_folha.py:53`, `:58`, `:138`, `:190`; `ponto/services/pautas_dos_discordantes.py:74`, `:76`; teste de equivalência de lote em `chamados/tests/test_o261e_f5b_juiz_porta.py:187`, `:188`, `:189`. A chamada avulsa do juiz cria alimentação em `chamados/juizes.py:1222`.

Não encontrei leitor atual deste dict tomando as chaves de `verbo` como conjunto da fila: o antigo caso do vigia agora lê `fila_ids` (`app/chamados/services/vigia_fio.py:566`), e a porta usa `fila_ids` (`app/chamados/juizes.py:1227`). Não encontrei dict manual de alimentação em testes; os dicts manuais e mocks são **saída da porta**: `app/folha/tests/test_haiku_fora_da_folha.py:101`, `:129`, `:143`; `app/chamados/tests/test_o261e_f5_alcance.py:306`. O teste do lote usa o alimentador real (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:187`).

Há helper antigo que ainda chama o universo errado e se apresenta como painel: `app/chamados/tests/test_o261e_f5_alcance.py:281` a `:285` particiona `qs_na_fila()`; seus usos estão em `:293`, `:298`. O helper F5B foi corrigido para `qs_vivos()` (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:87` a `:92`). Isso não é leitor de `alimentar_portas`, mas deixa uma referência de teste enganosa.

Pertinência da porta, com alimentação real e estado estável:

- Fila e âncora continuam provenientes de `qs_na_fila`: `fila_cel` e `fila_ids` nascem da mesma iteração (`app/chamados/juizes.py:1150` a `:1153`); a fila lê `fila_cel` (`:1223`), a pergunta exige `anc in fila_ids` (`:1227`). A âncora é destino, com fallback para chamado da disputa (`app/chamados/services/fio.py:116` a `:117`).
- Estar na fila é condição necessária da âncora, não suficiente da pergunta: ainda exige pergunta viva, resposta não vazia se respondida e presença no app (`app/chamados/juizes.py:1155` a `:1157`, `:1227`). Os testes cobrem resposta vazia e fora do app (`app/chamados/tests/test_o261e_f5_alcance.py:145`, `:194`).
- Silêncio vigente fica fora da fila; silêncio vencido pode estar nela, pois o predicado exclui somente `silenciado_ate > now` (`app/chamados/services/painel.py:186` a `:188`). O vencido conserva porta, mas perde número em `fila` se o verbo for `historico` (`app/chamados/juizes.py:1232`; teste `app/chamados/tests/test_o261e_f5b_juiz_porta.py:147`).
- `registrado` está em `VIVOS`, mas não em `COBRANCA`, portanto não entra em `qs_na_fila` (`app/chamados/catalogo/motor.py:24`, `:29`; `app/chamados/services/painel.py:187`), coberto em `app/chamados/tests/test_o261e_f5b_juiz_porta.py:250` a `:257`.

Ressalva de consistência: fila e mapa de verbos são consultas separadas (`app/chamados/juizes.py:1150`, `:1158`), e `alimentacao` fornecida não é revalidada (`:1222`). Se um chamado deixar `VIVOS` entre essas consultas, a fila pode produzir verbo `None` pelo `.get()` (`:1224`), enquanto a mesma situação na porta da pergunta pode lançar `KeyError` pelo índice (`:1228`); ocorrência em produção `[nao verificado]`.

## 31/10 03:37 · sessao fa4f12636b7a · O261-E-F1C4 · verificador · origem=Code

**Não aprovaria o delta:** a nova guarda da ressurreição elimina uma recuperação legítima prevista pelas portas existentes; além disso, protege apenas o chamador cartório, deixando outro caminho capaz de desfazer `celula_absolveu`. São conclusões estáticas; ocorrência em produção [não verificado]. (`app/ponto/services/cartorio.py:1355`; `app/chamados/models.py:1232`; `app/chamados/models.py:1389`; `app/chamados/models.py:1453`)

1. **Tranca: mesma autoridade. Isenção da resposta: regra copiada.**

O E2 chama **a mesma função** `dia_da_pergunta_trancado(p)` usada no trecho indicado do emissor. Essa função determina a competência por `janela_atual`, consulta `periodo_trancado` e considera o toque humano posterior. `periodo_trancado` é a autoridade registrada em `_K7`; o filtro novo não reproduz sua lógica. (`app/chamados/reconciliador.py:579`; `app/chamados/reconciliador.py:617`; `app/chamados/services/disputa_emissao.py:1541`; `app/chamados/services/disputa_emissao.py:1551`; `app/chamados/juizes.py:891`; `app/chamados/juizes.py:907`; `app/core/juizes.py:283`)

Já a isenção do `y` é **uma segunda expressão da mesma regra**: o laço testa texto com `any((p.resposta_colab or '').strip()...)`; a compreensão repete isso para `y`. Não divergem hoje, mas não consultam um predicado compartilhado. Portanto, o comentário “a MESMA isenção” descreve equivalência atual, não unicidade de implementação. (`app/ponto/services/cartorio.py:1328`; `app/ponto/services/cartorio.py:1344`; `app/ponto/services/cartorio.py:1346`)

2. **O novo `_protesta(x)` tem contraexemplo de regressão.**

O problema é a unidade consultada: `_protesta` usa **a célula do dia em julgamento**, enquanto a porta de ressurreição examina **o marco de cada pergunta**. Essas datas podem legitimamente diferir numa jornada que cruza a meia-noite. ([cartorio.py:1156](/home/ronald/codex_ws/7b76efa7d2dcaaa47c339314360f3867852d8dd0/app/ponto/services/cartorio.py:1156); `app/chamados/models.py:1403`; `app/chamados/models.py:1410`; `app/chamados/juizes.py:1719`)

Contraexemplo permitido pelo código:

- Há um `turno_aberto` em D+1, dia de folga, cobrando a véspera D ainda furada. A premissa preserva explicitamente `turno_aberto*` nessa situação: não regulariza automaticamente pela folga e consulta a véspera. (`app/chamados/models.py:1232`; `app/chamados/juizes.py:424`)
- Sua resolvedora pertence a D, tem o marco apagado, está materializada e validada, sem texto, pela via `completude_turno`. Esse estado é usado pelos próprios testes de ressurreição; o C6 inicialmente a considera morta por `validada_em`. (`app/chamados/tests/test_o261_ressuscita_respeita_o_marco.py:93`; `app/chamados/tests/test_o261_ressuscita_respeita_o_marco.py:104`; `app/chamados/juizes.py:1006`)
- Sendo o único chamado da cobrança, ele permanece vivo: a premissa segue furada e não há outro sobrevivente para justificar dedup. (`app/chamados/models.py:699`; `app/chamados/models.py:709`; `app/chamados/models.py:728`)
- Antes da guarda acrescentada, a porta limpava os carimbos dessa pergunta; com a célula de D acusando, ela voltava a viver. Agora `_protesta=False`, porque **D+1 é folga**, e o `continue` impede essa recuperação. ([cartorio.py:1355](/home/ronald/codex_ws/7b76efa7d2dcaaa47c339314360f3867852d8dd0/app/ponto/services/cartorio.py:1355); `app/chamados/models.py:1453`; `app/chamados/juizes.py:1056`)

Logo, “não protesta neste dia” não basta para concluir “nenhuma pergunta deste chamado deve ressuscitar”. O próprio coletor da premissa já distingue o caso da véspera; a guarda nova perde essa distinção. (`app/ponto/services/cartorio.py:1157`; `app/chamados/models.py:1232`; `app/chamados/juizes.py:424`)

O censo direto de produção é:

| Operação | Chamadores |
|---|---|
| `ressuscitar_perguntas()` | Cartório, `app/ponto/services/cartorio.py:1365`; `ChamadoColaborador.reabrir`, `app/chamados/models.py:1389`. |
| `perguntas_a_ressuscitar()` | Comando de reabrir órfãs, para detectar irmãs que voltariam junto: `app/chamados/management/commands/reabrir_orfas_enterradas.py:106`. |
| Universo compartilhado | Wrapper de leitura e escritor consultam `_universo_da_ressuscitacao`: `app/chamados/models.py:1398`; `app/chamados/models.py:1444`. |

A entrada `reabrir()` recebe pedidos de renascimento, desarquivamento, adesão, disputa administrativa, competência, resposta visível e comandos de reparação; todos podem alcançar a ressurreição quando suas guardas permitem. (`app/chamados/models.py:978`; `app/chamados/views_arquivar.py:68`; `app/chamados/services/adesao.py:92`; `app/chamados/services/acoes_disputa.py:266`; `app/chamados/services/competencia_trancada.py:258`; `app/chamados/services/disputa_emissao.py:1606`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:161`; `app/ponto/management/commands/reconciliar_grade.py:522`; `app/ponto/management/commands/detectar_ausencias.py:301`; `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:49`)

**Existe caminho residual de apagar o carimbo:** `reabrir → ressuscitar_perguntas`. O universo da porta exige materialização, resposta nula/vazia e marco não comprovadamente aceso; não exclui `celula_absolveu` nem exige C6 viva. A escrita apaga a via e os carimbos. (`app/chamados/models.py:1389`; `app/chamados/models.py:1403`; `app/chamados/models.py:1410`; `app/chamados/models.py:1453`)

Um contraexemplo é um container `disputa_supervisao_manual`, fora do reconhecimento de chamado de furo, com resolvedora sem texto, célula `concorde` definitiva e motivo sem marco próprio. O E2 carimba; uma reabertura aceita apaga; o próximo E2 carimba novamente. A guarda de premissa da reabertura é condicionada a `e_chamado_de_furo`, portanto não cobre todo container. Isso demonstra a possibilidade de vai-e-volta do carimbo; repetição automática na mesma madrugada [não verificado]. (`app/chamados/catalogo/modulos.py:146`; `app/chamados/models.py:1192`; `app/chamados/models.py:1296`; `app/chamados/juizes.py:842`; `app/chamados/juizes.py:1068`; `app/chamados/reconciliador.py:623`; `app/chamados/models.py:1380`; `app/chamados/models.py:1453`)

Os outros desfazedores têm restrições diferentes: `desfazer_carimbo_sem_lastro` exige C6 viva; competência e fantasma restringem a via; resposta restringe a `VIAS_SEM_LASTRO`; devolução de absorção seleciona `ausencia_dia`/`declaracao`. Eles não demonstram esse mesmo ciclo de `celula_absolveu` sob célula definitiva estável. (`app/chamados/services/relavra_via.py:227`; `app/chamados/models.py:2544`; `app/chamados/models.py:2514`; `app/chamados/models.py:2534`; `app/chamados/services/materializacao.py:578`; `app/chamados/services/materializacao.py:628`)

3. **Quem escreve e quem lê.**

`celula_absolveu` é valor de `PerguntaDisputa.via_resolucao`. “Retratada”, nesse percurso, designa uma operação; não é o nome do campo gravado pelo delta. (`app/chamados/models.py:2212`; `app/chamados/models.py:2262`; `app/chamados/reconciliador.py:505`)

| Campo/estado | Quem escreve | Quem lê |
|---|---|---|
| Pergunta: `via_resolucao`, `materializada_em`, `materializada_por`, na absolvição | E2 carimba e salva: `app/chamados/reconciliador.py:621`. | C6 e juiz de lastro: `app/chamados/juizes.py:603`, `app/chamados/juizes.py:1044`, `app/chamados/juizes.py:1050`, `app/chamados/juizes.py:1080`; tradução em `estado`: `app/chamados/models.py:2439`. |
| Pergunta: carimbos desfeitos na ressurreição | A porta limpa `materializada_em`, `validada_em`, `validada_por`, `respondida_em` e `via_resolucao`: `app/chamados/models.py:1453`. | Universo da porta: `app/chamados/models.py:1403`; C6: `app/chamados/juizes.py:1006`, `app/chamados/juizes.py:1020`, `app/chamados/juizes.py:1080`. |
| Pergunta: `resposta_colab`/`respondida_em` | Escritor da resposta: `app/chamados/services/disputa_emissao.py:1247`; correção limpa resposta/carimbos: `app/chamados/services/disputa_emissao.py:1358`. | Isenções do cartório: `app/ponto/services/cartorio.py:1328`, `app/ponto/services/cartorio.py:1346`; porta de ressurreição: `app/chamados/models.py:1406`; C6: `app/chamados/juizes.py:1020`. |
| Chamado: `status_local`, `resolvido_em`, `via_encerramento` | Núcleo `_gravar_estado`: `app/chamados/models.py:222`, `app/chamados/models.py:237`, `app/chamados/models.py:279`. `reconciliar` e `reabrir` delegam: `app/chamados/models.py:618`, `app/chamados/models.py:1380`. | Cartório: `app/ponto/services/cartorio.py:1323`, `app/ponto/services/cartorio.py:1342`; signals: `app/chamados/signals.py:33`, `app/chamados/signals.py:81`; auditor: `app/chamados/management/commands/auditar_invariantes_chamados.py:571`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1066`. |
| Disputa: `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento` | E2 pede `fechar`: `app/chamados/reconciliador.py:639`; porta grava: `app/chamados/models.py:1809`; ressurreição pede reabertura: `app/chamados/models.py:1479`, `app/chamados/models.py:2035`. | E2: `app/chamados/reconciliador.py:627`; receiver que tenta fechar o pai: `app/chamados/signals.py:28`; ressurreição: `app/chamados/models.py:1479`. |
| `out['fio_nao_protesta']` | Incrementado por chamado que alcança a guarda: `app/ponto/services/cartorio.py:1322`, `app/ponto/services/cartorio.py:1362`. | Só a leitura do próprio incremento foi localizada; consumidor específico [não verificado]. `julgar_celula` devolve o dicionário, mas o resumo do comando omite a chave: `app/ponto/services/cartorio.py:1668`; `app/ponto/management/commands/processar_cartorio.py:66`. |

Não há novo escritor de status fora da porta nesse delta: `y.reconciliar()` converge no núcleo existente. O E2 também já está declarado como porta do carimbo de absolvição. Isso **não torna `via_resolucao` um campo de escritor único**: o próprio registro declara essa dívida. (`app/ponto/services/cartorio.py:1348`; `app/chamados/models.py:618`; `app/core/portas.py:306`; `app/core/portas.py:352`)

4. **Checklist das classes de reprovação.**

| Classe | Resultado |
|---|---|
| Segundo juiz duplicando juiz registrado | **Não no filtro da tranca:** ambos consultam a mesma função e `_K7`. (`app/chamados/reconciliador.py:617`; `app/chamados/services/disputa_emissao.py:1551`; `app/core/juizes.py:283`) |
| Segundo escritor fora da porta | **Não acrescentado para status.** Há, porém, política nova em apenas um chamador da ressurreição, enquanto o outro continua apagando o mesmo estado. (`app/chamados/models.py:618`; `app/ponto/services/cartorio.py:1355`; `app/chamados/models.py:1389`) |
| Relógio fora da borda | **Permanece no sítio tocado:** E2 usa `_tz.now()` internamente; a função não recebe instante. É anterior a este delta, mas contraria a regra fornecida. (`app/chamados/reconciliador.py:551`; `app/chamados/reconciliador.py:621`; `app/docs/CORTES.md:369`) |
| Derivação paralela de fonte única | **Sim:** isenção de resposta repetida; e `_protesta` usado como critério global de ressurreição perde a distinção da véspera preservada pela premissa. (`app/ponto/services/cartorio.py:1328`; `app/ponto/services/cartorio.py:1346`; `app/ponto/services/cartorio.py:1157`; `app/chamados/models.py:1232`) |
| Selo/RED sem mordida suficiente | **Cobertura incompleta.** Os casos FIO-DUP citados criam perguntas sem resposta, portanto não exercitam a isenção nova do `y`. A flexibilização do prefixo preserva as verificações de sobrevivente e trilha. (`app/ponto/tests/test_fechar3_troca_escala_gate.py:283`; `app/ponto/tests/test_fechar3_troca_escala_gate.py:298`; `app/ponto/tests/test_fechar3_troca_escala_gate.py:302`; `app/ponto/tests/test_fechar3_troca_escala_gate.py:319`) |
| Ordem de trava | Inversão nova **não demonstrada**. O fechamento continua pelo `atomic` do gate e pela ordem documentada chamado → pergunta. Ausência de deadlock em execução [não verificado]. (`app/chamados/models.py:614`; `app/chamados/models.py:261`) |
| Pouso antes do pré-requisito | RED/GREEN, suite e pouso deste hash **[não verificado]**. Os pré-requisitos estão escritos, mas o anexo não comprova seu cumprimento. (`CLAUDE.md:28`; `CLAUDE.md:542`) | [nao verificado]

O teste novo da tranca verifica corretamente **não carimbar**. Ele não prova “destrancar devolve viva”: sua célula é `concorde`, definitiva e com todas as lâmpadas acesas; mesmo sem tranca, o C6 continua considerando essa pergunta morta. (`app/chamados/tests/test_e2_pergunta_so_ao_c6.py:80`; `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:89`; `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:92`; `app/chamados/juizes.py:1068`)

## 31/10 03:34 · sessao 95afd20c65d1 · - · - · origem=Ronald (tmux)

Mapa de campos e portas:

| Estado/campo | Quem escreve | Quem lê |
|---|---|---|
| `celula_absolveu` | É valor de `PerguntaDisputa.via_resolucao`, definido em `app/chamados/models.py:2212`; E2 grava junto com `materializada_em` e `materializada_por` em `app/chamados/reconciliador.py:621`–625. | C6 lê via/carimbo em `app/chamados/juizes.py:1015`, `:1044`, `:1050`, `:1080`; lastro consulta `VIAS_SEM_TESTEMUNHA` em `app/chamados/juizes.py:603`, cujo conjunto contém essa via em `app/chamados/models.py:2875`. |
| Ressuscitação da pergunta | `ressuscitar_perguntas` limpa `materializada_em`, `validada_em`, `validada_por`, `respondida_em` e `via_resolucao` em `app/chamados/models.py:1453`; reabre a disputa em `:1479`. | Universo da própria porta lê materialização, texto e lâmpada em `app/chamados/models.py:1403`; C6 lê os carimbos em `app/chamados/juizes.py:1020`, `:1044`, `:1050`, `:1080`; propriedade `estado` traduz o juiz em `app/chamados/models.py:2432`. |
| Chamado: `status_local`, `resolvido_em`, `via_encerramento` | Núcleo `_gravar_estado`: status em `app/chamados/models.py:222`, fechamento em `:237`–243 e persistência em `:279`. `reconciliar` delega em `:618`; reabrir delega em `:1380`. | Delta lê status em `app/ponto/services/cartorio.py:1323`, `:1342` e chama a porta em `:1348`. Signals leem em `app/chamados/signals.py:33`, `:70`, `:81`; auditor lê fechamento em `app/chamados/management/commands/auditar_invariantes_chamados.py:571` e via em `:1066`. |
| Disputa: fechamento | E2 pede fechamento pela porta em `app/chamados/reconciliador.py:639`; porta grava `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento` em `app/chamados/models.py:1809`. | Juiz `pode_fechar` em `app/chamados/models.py:1737`; receiver lê fechamento e pede fechamento do chamado-pai em `app/chamados/signals.py:28`–45; ressuscitação lê disputa fechada em `app/chamados/models.py:1479`. |
| `out['fio_nao_protesta']` | Incremento em `app/ponto/services/cartorio.py:1362`. | A única ocorrência nominal na cópia é o próprio incremento; sem consumidor específico localizado. O resultado completo é devolvido em `app/ponto/services/cartorio.py:1668`, mas o resumo do comando não inclui a chave em `app/ponto/management/commands/processar_cartorio.py:66`–68. |

`retratada` não é campo nem via da pergunta no modelo: as vias declaradas estão em `app/chamados/models.py:2227`–2244. Retratação de chamado é operação `_retratar`, delegando a `reconciliar`, resolução de perguntas e nota em `app/chamados/reconciliador.py:505`–542; deduplicação de pergunta usa `VIA_DUPLICATA` em `app/chamados/services/fio.py:878`–897.

O delta não introduz segundo escritor de status fora da porta: chama `y.reconciliar()` em `app/ponto/services/cartorio.py:1348`, que converge no núcleo em `app/chamados/models.py:618`. E2 também já é porta registrada para o carimbo de absolvição em `app/core/portas.py:306`–310. Contudo, **o campo `via_resolucao` não tem escritor único**: a dívida existente é explicitamente reconhecida em `app/core/portas.py:352`–356; as portas de pergunta autorizadas aparecem em `:388`–395.

Relógio interno nos sítios alcançados: E2 em `app/chamados/reconciliador.py:621`, núcleo de chamado em `app/chamados/models.py:238`, fechamento da disputa em `:1809`; essas leituras não foram acrescentadas por este delta, conforme anexo fornecido [não verificado para execução/produção].

## 31/10 03:34 · sessao ea87990a7bc2 · - · - · origem=Ronald (tmux)

Censo de chamadas de produção na cópia:

| Porta | Chamador direto |
|---|---|
| `ressuscitar_perguntas()` | `ChamadoColaborador.reabrir`, após gravar o estado reaberto: `app/chamados/models.py:1380`, `app/chamados/models.py:1389`. |
| `ressuscitar_perguntas()` | HX-CARTORIO-RESSUSCITA: `app/ponto/services/cartorio.py:1365`. |
| `perguntas_a_ressuscitar()` | Guarda que impede reabrir irmãs fora do escopo no comando: `app/chamados/management/commands/reabrir_orfas_enterradas.py:106`. |
| Universo compartilhado | O wrapper de leitura e a porta usam `_universo_da_ressuscitacao`: `app/chamados/models.py:1398`, `app/chamados/models.py:1444`. |

O universo da porta seleciona pergunta materializada, resposta nula/vazia e marco que não seja comprovadamente aceso; **não consulta C6, veredito, status do chamado nem via de resolução**. Portanto inclui `celula_absolveu` com marco `False` ou `None`: `app/chamados/models.py:1403`, `app/chamados/models.py:1410`. A escrita apaga `materializada_em`, `validada_em`, `validada_por`, `respondida_em` e `via_resolucao`: `app/chamados/models.py:1453`.

**Existe caminho residual de desfazer `celula_absolveu` fora do novo `_protesta`: `reabrir → ressuscitar_perguntas`.** O novo filtro protege apenas o chamador cartório: `app/ponto/services/cartorio.py:1355`; a outra chamada permanece em `app/chamados/models.py:1389`.

Contraexemplo estático, sem afirmar ocorrência em produção:

1. Chamado de container, por exemplo `disputa_supervisao_manual`, com categoria/subcategoria vazias, resolvido pelo sistema, sem irmão nem agrupamento; esses módulos são containers e estão fora de `CALADOS_POR_ISENCAO`: `app/chamados/catalogo/modulos.py:144`, `app/chamados/catalogo/modulos.py:232`. A guarda de premissa só se aplica a `e_chamado_de_furo`, cujo reconhecimento depende do módulo/categoria/subcategoria: `app/chamados/models.py:1192`, `app/chamados/models.py:1296`.
2. Uma resolvedora sem texto aponta para célula `concorde` definitiva e tem motivo sem marco próprio ou marco ausente/apagado. C6 mata pela célula definitiva: `app/chamados/juizes.py:1068`. E2 carimba `celula_absolveu`: `app/chamados/reconciliador.py:611`, `app/chamados/reconciliador.py:623`.
3. Uma reabertura aceita grava o novo estado e chama a porta: `app/chamados/models.py:1377`, `app/chamados/models.py:1389`. Um chamador real é a reabertura administrativa de disputa: `app/chamados/services/acoes_disputa.py:265`.
4. O marco `False`/`None` deixa a resolvedora no universo, a porta limpa o carimbo e a passada seguinte do E2 volta a carimbá-la: `app/chamados/models.py:1410`, `app/chamados/models.py:1453`, `app/chamados/reconciliador.py:623`. Existência desse conjunto em produção: **[não verificado]**.

Não generalizar esse contraexemplo a todo chamado de furo: a reabertura desses passa por `premissa_regularizada`, que lê a célula atual, decisão administrativa, dia sem trabalho, marcos acesos/inexistentes e `_celula_furada`; uma premissa regularizada recusa a reabertura: `app/chamados/models.py:1228`, `app/chamados/models.py:1249`, `app/chamados/models.py:1296`.

Chamadores indiretos de `reabrir()` que podem chegar à porta, sujeitos às respectivas guardas:

- Renascimento por premissa: `app/chamados/models.py:978`; seus chamadores são o dedup de abertura e resposta tardia: `app/chamados/models.py:881`, `app/chamados/services/resposta_tardia.py:45`.
- Desarquivar tranca: `app/chamados/views_arquivar.py:68`.
- Adesão: `app/chamados/services/adesao.py:92`.
- Reabrir disputa administrativa: `app/chamados/services/acoes_disputa.py:266`.
- Reabrir competência: `app/chamados/services/competencia_trancada.py:258`.
- HX-RESPOSTA-VISIVEL: `app/chamados/services/disputa_emissao.py:1606`.
- Reabrir órfãs enterradas: `app/chamados/management/commands/reabrir_orfas_enterradas.py:161`.
- Reconciliar grade: `app/ponto/management/commands/reconciliar_grade.py:522`.
- Detector de ausência, ramo de âncora: `app/ponto/management/commands/detectar_ausencias.py:301`.
- Reabrir chamados de ausência aberta: `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:49`.

Outros desfazedores não demonstram esse mesmo ciclo em estado estável:

- `desfazer_carimbo_sem_lastro` relê a pergunta travada e só limpa se C6 a considera viva; C6 morta por célula definitiva impede a limpeza: `app/chamados/services/relavra_via.py:221`, `app/chamados/services/relavra_via.py:227`.
- `reabrir_por_competencia` só desfaz `competencia_trancada`: `app/chamados/models.py:2544`.
- `reabrir_fantasma` só desfaz `fantasma`, e seu comando exige marco apagado e C6 viva: `app/chamados/models.py:2514`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:77`.
- `reabrir_por_resposta` só desfaz `VIAS_SEM_LASTRO`: `app/chamados/models.py:2534`.

Os testes da porta cobrem marco aceso versus apagado, ausência de célula, idempotência e reabertura da disputa; esses casos não são um teste de `E2 → reabrir → E2` com célula definitiva que absolve: `app/chamados/tests/test_o261_ressuscita_respeita_o_marco.py:104`, `app/chamados/tests/test_o261_ressuscita_respeita_o_marco.py:147`, `app/chamados/tests/test_o261_ressuscita_respeita_o_marco.py:167`, `app/ponto/tests/test_hx_cartorio_ressuscita.py:85`.

## 31/10 03:34 · sessao 0ac2dc173d1d · - · - · origem=Ronald (tmux)

Achado de regressão por análise estática: o novo guarda confunde **dia do chamado** com **dia/marco da pergunta**. `_protesta` lê a célula da volta do cartório e retorna falso em qualquer folga (`app/ponto/services/cartorio.py:1156-1157`), enquanto o universo da ressurreição lê cada pergunta e seu marco (`app/chamados/models.py:1403-1410`).

Contraexemplo válido pelas portas existentes:

- Um `turno_aberto` está ancorado em D+1, folga; a véspera D tem saída apagada. A própria premissa preserva essa cobrança: exclui `turno_aberto*` da regularização automática por folga (`app/chamados/models.py:1232`) e consulta a véspera (`app/chamados/juizes.py:424-428`). A resolvedora em D pode legitimamente resolver o chamado em D+1 (`app/chamados/juizes.py:1719-1725`).
- A pergunta está materializada e validada, sem resposta escrita, por `celula_absolveu` ou `completude_turno`; assim seu fio está morto por `validada_em` (`app/chamados/juizes.py:1006-1007`). Esse é um estado explicitamente coberto pelos testes existentes de ressurreição correta (`app/chamados/tests/test_o261_ressuscita_respeita_o_marco.py:93-117`, `app/ponto/tests/test_o261_cobrado_le_o_fio.py:288-291`, `app/ponto/tests/test_o261_cobrado_le_o_fio.py:315-327`).
- Sozinho, o chamado permanece vivo: premissa furada não permite reconciliar; o próprio sobrevivente C8 não fecha por dedup (`app/chamados/models.py:699-731`).
- Antes, o laço chegava a `ressuscitar_perguntas`, cuja porta limpa os carimbos e devolve a saída apagada à fila (`app/chamados/models.py:1453-1454`). Agora encontra `_protesta=False` e sai antes (`app/ponto/services/cartorio.py:1355-1365`).

Portanto há chamado corretamente ressuscitável que perde a recuperação automática; ocorrência e quantidade em produção: [não verificado]. O comentário que justifica o guarda pressupõe “célula que não acusa” sobre a pergunta, mas o predicado consulta a célula da volta do chamado (`app/ponto/services/cartorio.py:1156-1157`, `app/ponto/services/cartorio.py:1358-1361`).

Sobre a isenção do `y`: é **a mesma expressão lógica copiada**, não uma chamada à mesma autoridade. O laço verifica `any((p.resposta_colab or '').strip()...)` em `app/ponto/services/cartorio.py:1328`; a compreensão repete o teste em `app/ponto/services/cartorio.py:1346-1347`. Não encontrei divergência atual entre essas duas expressões; a duplicação permite evolução independente nesses dois sítios (`app/ponto/services/cartorio.py:1328`, `app/ponto/services/cartorio.py:1346-1347`).

O teste flexibilizado de `[FIO-DUP]` para `[FIO-DUP` continua verificando sobrevivente, terminal e trilha de dedup; portanto essa edição, isoladamente, não torna o teste vazio (`app/ponto/tests/test_fechar3_troca_escala_gate.py:302-307`). Ela não testa a proteção de resposta do `y`: suas duas perguntas são criadas sem texto (`app/ponto/tests/test_fechar3_troca_escala_gate.py:283-289`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:298-299`). O outro caso FIO-DUP-C8 também cria a pergunta do velho sem texto (`app/ponto/tests/test_fechar3_troca_escala_gate.py:319-320`).

O contador novo é incrementado pelo número de **chamados** que atingiram o guarda, não por perguntas (`app/ponto/services/cartorio.py:1322`, `app/ponto/services/cartorio.py:1362`). Seu único uso nominal encontrado foi a própria leitura/incremento; leitor externo ou selo dedicado: [não verificado].

## 31/10 03:26 · sessao 62aed3dacb80 · FIO-ESTADO-LE-O-ALCANCE · verificador · origem=Code

**O escritor F5 já existe; a manchete e o selo ainda seguem o contador antigo.** O comando grava `censo.fio_morto_alcance` e `alcance`, mas o ESTADO continua imprimindo `fio_morto`. O próprio teste F5 caracteriza **antigo=0 e alcance=1** — portanto os números não são intercambiáveis. (`app/chamados/management/commands/vigiar_fio_morto.py:216-218`; `bin/fio_estado.py:101-104`; `app/chamados/tests/test_o261e_f5_alcance.py:430-434`.)

O anexo não contém uma medição produtiva: valores atuais, último recarimbo e resultado publicado em produção são **[nao verificado]**. [nao verificado]

| Campo | Quem escreve | Quem lê atualmente |
|---|---|---|
| `censo.fio_morto_alcance` | `censo_alcance()` inicializa em zero e incrementa os dias sem porta; o comando copia para `censo`. (`app/chamados/services/vigia_fio.py:539`, `:649`; `app/chamados/management/commands/vigiar_fio_morto.py:217`.) | O comparador genérico o consulta **somente se estiver em `esperado`**. Não integra o retorno nominal nem a manchete. (`bin/fio_estado.py:72-92`, `:101-104`.) |
| `alcance.alcance_cego` | Calculado como `not dias`; persistido no bloco inteiro. (`app/chamados/services/vigia_fio.py:553`; `app/chamados/management/commands/vigiar_fio_morto.py:218`.) | `linhas_alcance()` lê o resultado em memória para a saída do comando; o leitor host ignora esse bloco. (`app/chamados/services/vigia_fio.py:662-664`; `bin/fio_estado.py:69-92`.) |
| `alcance.alcance_universo` | `len(dias)`, dos dias retidos por empresa visível e competência aberta; persistido em `alcance`. (`app/chamados/services/vigia_fio.py:534-552`; `app/chamados/management/commands/vigiar_fio_morto.py:218`.) | Usado na saída textual do comando, mas ignorado pelo leitor host. (`app/chamados/services/vigia_fio.py:659-662`; `bin/fio_estado.py:69-92`.) |
| `esperado` | O comando declara os esperados antigos, acrescenta `fio_morto_alcance=0` e grava uma cópia. (`app/chamados/management/commands/vigiar_fio_morto.py:87-95`, `:211`.) | `ler()` compara todas as chaves presentes com `censo`; `linha()` ainda seleciona o esperado antigo para a manchete. (`bin/fio_estado.py:70-73`, `:103`.) |
| `medido_em` | O relógio é capturado **antes dos censos** e serializado em horário local no carimbo. É instante de início, não de conclusão. (`app/chamados/management/commands/vigiar_fio_morto.py:162-168`, `:207`.) | `ler()` calcula idade; `linha()` mostra data, idade e aviso de velho. (`bin/fio_estado.py:74-84`, `:112-117`.) |
| `censo.fio_morto` antigo | `censo()` parte dos chamados vivos com célula, inicializa e incrementa o contador; o comando preserva `d` no carimbo. (`app/chamados/services/vigia_fio.py:317`, `:340`, `:417`; `app/chamados/management/commands/vigiar_fio_morto.py:216`.) | Ainda determina a manchete e participa da comparação com `esperado`. O alarme também consulta o contador em memória. (`bin/fio_estado.py:72-73`, `:86`, `:101-104`; `app/chamados/management/commands/vigiar_fio_morto.py:82`, `:170-171`.) |

Os pontos de falha são estes:

1. **Carimbo anterior ao F5 pode parecer bom sem medir alcance.** Sem `alcance` e sem o novo esperado, o leitor aceita o arquivo como `medido=True`, compara apenas as chaves antigas e pode mostrar `fio morto: 0` sem divergências. O selo chama exatamente essa combinação de “VERDE”; não existe uma validação de versão/esquema. (`bin/fio_estado.py:69-92`, `:101-104`; `bin/tests/test_fio_estado.sh:40-47`, `:56-60`.)

2. **Universo vazio produz zero, mas significa cegueira.** O produtor retorna `fio_morto_alcance=0`, `alcance_universo=0` e `alcance_cego=True`. Hoje o host ignora os dois últimos campos; olha somente `censo.cego`, que pertence ao universo antigo. Assim, igualdade `0=0` pode coexistir com ausência de universo de alcance, sem aviso correspondente. (`app/chamados/services/vigia_fio.py:538-555`; `bin/fio_estado.py:72-73`, `:91`, `:109-111`.)

   O contrato precisa também fechar **subchaves ausentes ou incoerentes**: só exigir o bloco `alcance` e rejeitar `alcance_cego=True` não garante que exista `alcance_universo>0`. O escritor fornece ambas; o leitor atual não verifica nenhuma. (`app/chamados/services/vigia_fio.py:552-555`; `bin/fio_estado.py:69-92`.)

3. **Contador ausente/`None` não suprime a manchete numérica.** Se seu esperado existir, `None != 0` aparece em `FORA DO ESPERADO`, mas a manchete continua mostrando o número antigo. Se faltar também `esperado.fio_morto_alcance`, nem essa divergência será calculada. (`bin/fio_estado.py:72-73`, `:101-108`.)

   **`esperado` ausente vira `{}`**, deixando a comparação vazia. Portanto a presença do esperado do novo contador também precisa ser condição de medição válida; ausência não pode virar aprovação por falta de comparação. (`bin/fio_estado.py:70-73`.)

4. **Recorte é protegido pelo escritor, não pelo conteúdo validado pelo leitor.** `--empresa` vira uma lista e só `empresas is None` permite carimbar; recortes declaram “NAO carimbado”. Há teste específico dessa proteção. Portanto o comando atual não produz normalmente um carimbo de recorte nesse destino. (`app/chamados/management/commands/vigiar_fio_morto.py:152-156`, `:205`, `:223-225`; `app/chamados/tests/test_o261_vigia_fio.py:801-814`.)

   Se um arquivo recortado chegar ao caminho por outra origem, o leitor não valida escopo/proveniência e pode publicar seu número como frota. A ocorrência disso em produção é **[nao verificado]**. (`bin/fio_estado.py:63-92`.)

5. **A proteção temporal não cobre data ausente ou inválida.** Nesses casos, a idade fica desconhecida, `velho=False` e a manchete permanece numérica. Não há fallback efetivo para `hoje`. Com data válida antiga, aparece a advertência de velho, mas o número continua na manchete; a idade ainda é arredondada antes da comparação com 26 horas. (`bin/fio_estado.py:74-84`, `:101-117`.)

6. **“Exceção de leitura = ILEGIVEL” não está implementado.** Ausência, erro de abertura/permissão e JSON inválido convergem para `medido=False` e **NUNCA MEDIDO**. JSON de formato errado pode lançar `AttributeError`; `medido_em` não textual pode lançar `TypeError`, pois a captura posterior aceita apenas `ValueError`. Essas falhas podem interromper o gerador antes da publicação. (`bin/fio_estado.py:63-80`, `:97-100`; `bin/gerar_estado.py:260-261`, `:364`.)

   Exceção no produtor antes da gravação também pode deixar o carimbo anterior intacto e ainda dentro das 26 horas. Já uma rodada que apenas **alarma** grava antes de sair: esse caso preserva a medição. (`app/chamados/management/commands/vigiar_fio_morto.py:167-189`, `:205-228`; `bin/fio_estado.py:74-84`.)

7. **O selo atual não prova a mudança proposta.** Suas fixtures não contêm alcance; os casos numéricos verificam `medido`, `velho` e nomes de divergências, sem conferir a manchete. O caso CEGO só procura essa palavra e permite conservar o número. Um mutante que restitua apenas a manchete antiga não seria detectado por essas verificações. (`bin/tests/test_fio_estado.sh:40-51`, `:56-91`, `:104-107`.)

   A fixture decisiva deve ter **antigo=0/alcance=N** e conferir a manchete publicada; o caso inverso distingue o novo resultado da regressão antiga. A primeira divergência já está caracterizada nos testes F5. (`bin/fio_estado.py:101-108`; `app/chamados/tests/test_o261e_f5_alcance.py:430-434`.)

Sobre outro leitor: o ESTADO delega a `ler()`/`linha()`, e os modos JSON e `--linhas` usam o mesmo `ler()`. Não identifiquei outro leitor operacional do arquivo; consumidores externos ou construídos inteiramente por caminhos dinâmicos são **[nao verificado]**. (`bin/gerar_estado.py:260-261`; `bin/fio_estado.py:64-65`, `:123-129`.)

**Existe, porém, uma regra operacional diferente no alarme**, que mede ao vivo e não lê o carimbo: cobra `cego` e passivos antigos, deixando `alcance_cego` e `fio_morto_alcance` fora do alarme. Logo um ESTADO corrigido poderá acusar alcance cego ou passivo enquanto o comando não alarma por esses campos — divergência atualmente explícita e testada. (`app/chamados/management/commands/vigiar_fio_morto.py:81-95`, `:167-171`, `:227-228`; `app/chamados/tests/test_o261e_f5_alcance.py:454-455`.)

## 31/10 03:25 · sessao c8b173220787 · - · - · origem=Ronald (tmux)

Leitores localizados e diferenças:

- **ESTADO usa uma leitura única:** `bin/gerar_estado.py:260-261` importa e chama `fio_estado.ler` e `linha`; o acesso ao arquivo acontece em `bin/fio_estado.py:64-65`. O CLI JSON e `--linhas` passam pelo mesmo `ler` (`bin/fio_estado.py:125-129`).
- **Não localizei outro leitor operacional do arquivo na cópia**, após buscas incluindo ocultos/ignorados, variantes do nome e construções de caminho; a existência de consumidores externos ou caminhos inteiramente dinâmicos é **[nao verificado]**. Evidência positiva dos consumidores identificados: `bin/gerar_estado.py:260-261`, `bin/fio_estado.py:64-65`.
- **O command possui regra diferente, mas não lê o carimbo:** calcula `d` e `alc` diretamente (`app/chamados/management/commands/vigiar_fio_morto.py:167-168`), calcula `gritou` somente sobre `d` (`:170-171`) e escreve o carimbo posteriormente (`:205-221`). O selo F5 exige explicitamente `fio_morto_alcance` fora de `ALARMA_EM_0` (`app/chamados/tests/test_o261e_f5_alcance.py:454-455`).

Falhas no leitor atual:

- `alcance`, `alcance_cego` e `alcance_universo` não são extraídos; o retorno contém somente campos do censo antigo (`bin/fio_estado.py:69-92`). `fio_morto_alcance` entra apenas indiretamente na comparação genérica de `esperado` (`:72-73`).
- A manchete imprime **`censo.fio_morto`**, com seu esperado antigo (`bin/fio_estado.py:101-104`). O aviso CEGO lê **`censo.cego`**, não `alcance.alcance_cego` (`:91`, `:109-111`).
- Carimbo anterior ao F5 pode produzir `medido=True`, zero antigo e nenhuma divergência: não se exige `alcance`; percorrem-se somente chaves existentes em `esperado` (`bin/fio_estado.py:69-73`, `:82-92`, `:101-104`). As próprias fixtures atuais representam esse formato e chamam zero antigo de VERDE (`bin/tests/test_fio_estado.sh:40-47`, `:56-60`).
- Se faltar `esperado`, ele vira `{}` e nenhuma divergência é calculada (`bin/fio_estado.py:70-73`). Se faltar só `esperado.fio_morto_alcance`, a comparação tampouco cobra esse campo (`:72-73`). Se faltar o contador e seu esperado existir, `None != 0` entra em `fora_do_esperado`, mas a manchete ainda imprime o número antigo (`:72-73`, `:101-108`).
- `medido_em` ausente/inválido não invalida a medição: idade fica `None`, `velho=False` e o número permanece, acompanhado de “idade nao medivel” (`bin/fio_estado.py:74-84`, `:112-117`). A idade é arredondada antes de comparar com 26 h, podendo atrasar ligeiramente a marcação de velho (`:79`, `:84`).
- Arquivo ausente, erro de I/O e JSON inválido caem todos no mesmo retorno `medido=False` e texto **NUNCA MEDIDO**, sem linha ILEGIVEL (`bin/fio_estado.py:63-68`, `:97-100`). Erros de forma/tipo após `json.load` podem escapar: `.get`/`.items` fora do `try`, ou `fromisoformat` com valor não textual, pois o segundo `except` captura apenas `ValueError` (`:69-80`).

Sobre o selo:

- O selo atual verifica zero/N antigo, idade, ausência, limiar variável, caminho e `censo.cego`; não testa alcance, universo vazio, recorte no conteúdo, esperado ausente, JSON inválido nem mutante da manchete nova (`bin/tests/test_fio_estado.sh:36-109`).
- Mesmo hoje, o caso CEGO só exige a palavra na saída; não exige supressão do número (`bin/tests/test_fio_estado.sh:104-107`), e o compositor mantém o número antes do aviso (`bin/fio_estado.py:101-111`).
- Há leitores diretos **de fixtures nos testes Django**, não consumidores alternativos de produção: `app/chamados/tests/test_o261_vigia_fio.py:775`, `:830`; `app/chamados/tests/test_o261e_f5_alcance.py:259`, `:275`, `:449`; `app/chamados/tests/test_o261_resposta_vazia.py:157-159`.

## 31/10 03:25 · sessao 6a1beaaa659b · - · - · origem=Ronald (tmux)

O selo atual aceita precisamente o carimbo sem `alcance` que o contrato quer reprovar: fixtures não têm as chaves F5 e o caso 1 exige resultado “verde” (`bin/tests/test_fio_estado.sh:40-47`, `bin/tests/test_fio_estado.sh:56-60`).

- A manchete continua em `censo.fio_morto`; nenhum campo de `alcance` integra o retorno de `ler` (`bin/fio_estado.py:69-92`, `bin/fio_estado.py:101-104`).
- `esperado` ausente vira `{}`; nenhuma comparação acontece, embora `medido=True` e a manchete possa apresentar `None` (`bin/fio_estado.py:70-73`, `bin/fio_estado.py:82-86`, `bin/fio_estado.py:101-104`).
- `medido_em` ausente/inválido mantém idade desconhecida e `velho=False`; não impede número na manchete (`bin/fio_estado.py:74-84`, `bin/fio_estado.py:101-104`). Carimbo antigo válido recebe advertência, mas conserva a manchete numérica (`bin/fio_estado.py:112-117`).
- `cego` legado só acrescenta uma nota; não muda a manchete nem `fora_do_esperado`, e `alcance_cego` é ignorado (`bin/fio_estado.py:72-73`, `bin/fio_estado.py:91`, `bin/fio_estado.py:109-111`).
- Arquivo ausente, falha de abertura e JSON inválido recebem igualmente “NUNCA MEDIDO”; não há saída “ILEGIVEL” (`bin/fio_estado.py:63-68`, `bin/fio_estado.py:97-100`).
- JSON válido de formato errado pode levantar `AttributeError`; `medido_em` não textual pode levantar `TypeError`, fora das capturas existentes (`bin/fio_estado.py:69-80`). A chamada do gerador não protege esta seção, podendo interromper a publicação antes da escrita final (`bin/gerar_estado.py:260-261`, `bin/gerar_estado.py:364`).

O selo valida principalmente `medido`, `velho` e nomes de divergências; não verifica a manchete nos casos numéricos. Só procura textos para ausência e cegueira; assim, um mutante que devolva a manchete ao contador legado pode passar (`bin/tests/test_fio_estado.sh:51`, `bin/tests/test_fio_estado.sh:56-91`, `bin/tests/test_fio_estado.sh:104-107`). O comentário sobre mutações não corresponde a um mecanismo de mutação implementado nesse script (`bin/gerar_estado.py:259`, `bin/tests/test_fio_estado.sh:19-109`).

Os testes F5 já documentam a diferença essencial: legado zero com alcance um; universo vazio cego; carimbo com ambos os locais do contador novo e esperado zero. Esses testes não exercitam o leitor host (`app/chamados/tests/test_o261e_f5_alcance.py:425-434`, `app/chamados/tests/test_o261e_f5_alcance.py:443-455`).

Não executei os testes, pois criam e apagam arquivos temporários (`bin/tests/test_fio_estado.sh:32-34`, `bin/tests/test_fio_estado.sh:48`, `bin/tests/test_fio_estado.sh:75`). Resultado efetivo de execução: [nao verificado].

## 31/10 03:25 · sessao b569565a3261 · - · - · origem=Ronald (tmux)

Escritor e origem dos campos:

| Campo | Quem produz | Quem persiste |
|---|---|---|
| `censo.fio_morto_alcance` | `censo_alcance()` inicializa em 0 e incrementa para cada dia sem porta após as exclusões de hoje/futuro e dos baldes legais (`app/chamados/services/vigia_fio.py:539`, `:603`, `:609`, `:649`). | `Command.handle()` copia `alc['fio_morto_alcance']` para o bloco `censo` (`app/chamados/management/commands/vigiar_fio_morto.py:216-217`). |
| `alcance.alcance_cego` | `not dias`, portanto universo vazio significa cegueira (`app/chamados/services/vigia_fio.py:553`). | Bloco `alcance: alc` inteiro (`app/chamados/management/commands/vigiar_fio_morto.py:218`). |
| `alcance.alcance_universo` | `len(dias)`; `dias` deduplica `(colaborador, data)` dos dias retidos por empresa visível × competência aberta (`app/chamados/services/vigia_fio.py:534-552`). | Mesmo bloco inteiro (`app/chamados/management/commands/vigiar_fio_morto.py:218`). |
| `esperado` | `ESPERADO` contém antigo `fio_morto=0`; `ESPERADO_DO_CARIMBO` acrescenta `fio_morto_alcance=0` (`app/chamados/management/commands/vigiar_fio_morto.py:87-95`). | Cópia de `ESPERADO_DO_CARIMBO` (`app/chamados/management/commands/vigiar_fio_morto.py:211`). |
| `medido_em` | `agora=timezone.now()` antes de executar os dois censos; não é timestamp de conclusão (`app/chamados/management/commands/vigiar_fio_morto.py:162-168`). | `timezone.localtime(agora).isoformat()` (`app/chamados/management/commands/vigiar_fio_morto.py:207`). |
| `censo.fio_morto` antigo | `censo()` inicializa 0; universo parte de chamado vivo com célula; incrementa `fio_morto` após examinar lâmpada, pergunta, teto temporal e dono fora do fio (`app/chamados/services/vigia_fio.py:317`, `:340`, `:359-417`). | `dict(d, ...)` preserva o contador antigo (`app/chamados/management/commands/vigiar_fio_morto.py:216`). |

Persistência e exceções:

- Caminho padrão do escritor: `settings.VIGIA_FIO_CARIMBO` se configurado; senão `<BASE_DIR>/logs/fio_morto.json` (`app/chamados/management/commands/vigiar_fio_morto.py:98`, `:101-111`). O leitor host fixa `<raiz>/app/logs/fio_morto.json`, sem ler essa configuração (`bin/fio_estado.py:55`, `:62`).
- `_carimbar()` grava `.novo` e faz `os.replace()`, sem capturar erro de escrita (`app/chamados/management/commands/vigiar_fio_morto.py:126-133`). Exceção em censos/consultas anteriores impede atingir a gravação; a ordem está em `:167-189` antes de `:205-221`. Consequentemente, uma rodada falha pode deixar o último carimbo legível, ainda fresco para o leitor; não há registro da falha embutido no carimbo antigo (`app/chamados/management/commands/vigiar_fio_morto.py:167-221`; `bin/fio_estado.py:63-84`).
- `--empresa` não carimba: transforma a opção em lista de empresas, e grava só se `empresas is None`; até opção com IDs sem correspondência gera `[]`, portanto cai no “NAO carimbado” (`app/chamados/management/commands/vigiar_fio_morto.py:152-156`, `:205`, `:223-225`). Recorte persistido por essa implementação não é caso normal; arquivo de fixture/manual poderia não ter marcador de recorte, pois o leitor não valida proveniência (`bin/fio_estado.py:63-92`).
- Universo novo vazio retorna `fio_morto_alcance=0`, `alcance_universo=0`, `alcance_cego=True`; o command copia inclusive esse zero ao censo. Assim, comparar apenas contador × esperado produz igualdade 0=0 sem medição não vazia (`app/chamados/services/vigia_fio.py:538-555`; `app/chamados/management/commands/vigiar_fio_morto.py:211`, `:216-218`). O contrato precisa fazer cegueira/universo prevalecerem sobre essa igualdade.
- Falha da lista Cadastro × Realidade é capturada e registrada: mantém `_dias=None`, `dia_do_admin()` retorna falso e conta dia sem resposta, sem retirar o dia do contador (`app/chamados/services/vigia_fio.py:203-225`). Os dois censos compartilham a mesma lista; contador de falhas é copiado após ambos (`app/chamados/management/commands/vigiar_fio_morto.py:164-169`). Logo essa exceção é uma medição degradada que ainda pode produzir carimbo; não equivale à exceção de leitura do JSON (`app/chamados/services/vigia_fio.py:217-225`; `app/chamados/management/commands/vigiar_fio_morto.py:205-221`).

Consumos internos relevantes:

- O command lê `alc['fio_morto_alcance']` para o censo (`app/chamados/management/commands/vigiar_fio_morto.py:217`); `linhas_alcance()` lê contador, universo e cegueira para saída textual (`app/chamados/services/vigia_fio.py:659-664`).
- O alarme do command continua com `ALARMA_SEMPRE=('dias_futuros_no_universo','cego')` e passivos antigos; não inclui `alcance_cego` nem `fio_morto_alcance` (`app/chamados/management/commands/vigiar_fio_morto.py:81-95`, `:170-171`, `:227-228`). Portanto um painel corrigido pode acusar alcance cego/passivo enquanto o cron não alarma por esses mesmos campos, por desenho explícito atual (`:92-94`).
- O leitor atual não expõe `alcance`, `alcance_cego`, `alcance_universo` nem o novo contador em seu retorno; apenas o detecta incidentalmente se estiver listado em `esperado`, pelo comparador genérico (`bin/fio_estado.py:69-92`). Manchete continua no antigo `fio_morto` (`bin/fio_estado.py:101-104`).
- Não há dado produtivo medido no anexo: o que aconteceu em produção é `[nao verificado]`; a descrição acima refere-se aos caminhos possíveis no código.

## 31/10 03:10 · sessao f6d5809d61e6 · O261-E-F1-CLASSE-4 · verificador · origem=Code

**Reprovo o diff:** há fechamento de fatos distintos por dedup, sobrevivência dependente da sequência de eventos, carimbo indevido pela tranca e teste que não distingue a cura de sua reversão. (`app/ponto/services/cartorio.py:1341`, `app/chamados/models.py:709`, `app/chamados/reconciliador.py:611`, `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:63`)

1. **O gate não garante “um vivo”: há contraexemplos com zero e com dois.**

   **Zero depois, um antes:** considere A antigo, `intervalo_pendente`, cobrando intervalo apagado; B novo, `batida_ausente`, cobrando somente S apagada. Ambos sem perguntas ancoradas, num dia abaixo do piso de emissão, sem toque do admin. O código admite essas cobranças separadas por marcos e bloqueia a reemissão automática abaixo do piso. (`app/chamados/catalogo/modulos.py:105`, `app/chamados/catalogo/modulos.py:111`, `app/chamados/models.py:1234`, `app/ponto/services/cartorio.py:1398`, `app/ponto/services/cartorio.py:1460`)

   | Evento | Antes do diff | Depois do diff |
   |---|---|---|
   | Fechamento da disputa de B dispara Defesa 1 | B fecha apoiado em A. | B recusa porque ele é o C8. |
   | Fechamento da disputa de A dispara Defesa 1 | A sozinho recusa. | A fecha apoiado em B. |
   | Apenas S acende; cartório reconcilia por marco | A continua cobrando intervalo. | B fecha; o intervalo continua apagado, mas ficam zero vivos. |

   A sequência decorre do signal chamando a mesma porta, da troca de `excluir=self` pela proteção do próprio C8 e do fechamento independente quando os marcos do sobrevivente acendem. Abaixo do piso, o cartório não recompõe a cobrança apagada. É um contraexemplo de fluxo; ocorrência em produção: **[não verificado]**. (`app/chamados/signals.py:42`, `app/chamados/models.py:704`, `app/chamados/models.py:709`, `app/chamados/models.py:1236`, `app/ponto/services/cartorio.py:1225`, `app/ponto/services/cartorio.py:1406`)

   **Dois depois, um antes:** A antigo e B novo cobram D; B não tem perguntas; A ancora pergunta viva de D−1, admitida pela costura de jornada noturna. O gate protege B pelo C8 de D. A tenta fechar, mas sua pergunta procura uma resolvedora em D−1, onde não há candidata, e também recusa. Antes, B sem pergunta fechava apoiado em A. (`app/chamados/models.py:369`, `app/chamados/models.py:371`, `app/chamados/models.py:373`, `app/chamados/models.py:709`, `app/chamados/juizes.py:1626`, `app/chamados/juizes.py:1719`)

   A extensão nova não alcança esse caso: o cartório pula A por fio vivo e B por ausência de perguntas. Portanto, proteger o eleito e conseguir transferir as perguntas são pré-requisitos diferentes; o primeiro não garante o segundo. (`app/ponto/services/cartorio.py:1325`, `app/ponto/services/cartorio.py:1326`, `app/chamados/models.py:373`)

2. **`[FIO-DUP-C8]` amplia o fechamento para outro turno e para chamado respondido.**

   O filtro novo de `y` exige apenas outro objeto, estado vivo e módulo em `CALADOS_POR_ISENCAO`; não compara `entrada_timestamp` nem aplica a proteção de resposta usada para `x`. (`app/ponto/services/cartorio.py:1328`, `app/ponto/services/cartorio.py:1341`)

   - **Outro turno do mesmo dia:** passa pela seleção por dia e pelo gate de dedup. A reata também aceita imediatamente datas iguais. Isso contraria a identidade explicitada pelo juiz de turno: dois turnos no mesmo dia são dois fatos. (`app/chamados/juizes.py:1625`, `app/chamados/models.py:709`, `app/chamados/juizes.py:1710`, `app/chamados/juizes.py:1647`)
   - **Chamado com resposta:** `y` pode fechar mesmo que sua própria passagem pelo laço externo fosse poupada pelo texto do colaborador. A pergunta viva pode ser reapontada para `x`; o efeito demonstrável é fechamento e mudança de âncora, sem prova de perda do texto. (`app/ponto/services/cartorio.py:1328`, `app/ponto/services/cartorio.py:1343`, `app/chamados/models.py:416`, `app/chamados/juizes.py:1020`)
   - **Outro módulo:** a extensão alcança `batida_ausente`, `turno_aberto` e `intervalo_pendente`. Diferença de módulo, isoladamente, não demonstra erro: já existe teste autorizando dedup entre `batida_ausente` e `turno_aberto`. (`app/chamados/catalogo/furo_modulos.py:15`, `app/chamados/catalogo/modulos.py:235`, `app/chamados/tests/test_fechar3_retorno_do_gate.py:85`)

3. **Há mudança indireta fora de `CALADOS_POR_ISENCAO`, pela retirada da ordenação.**

   A cobertura agora percorre os mais novos primeiro. Considere A antigo, `entrada_bloqueada_turno_aberto`, categoria `turno_aberto`, datado por contexto/célula e com premissa viva; B novo, `batida_ausente`, cujo único marco já acendeu. Ambos passam pelo filtro de premissa da cobertura. Antes, o helper colocava B por último: A fechava apoiado em B, depois B fechava pelo marco. Agora B fecha primeiro e A perde a cobrança de apoio, permanecendo vivo. (`app/ponto/services/cobertura.py:225`, `app/ponto/services/cobertura.py:309`, `app/chamados/catalogo/premissa.py:48`, `app/chamados/models.py:1197`, `app/chamados/models.py:1236`)

   Portanto, “fora de CALADOS nada muda” vale apenas para a comparação isolada do próprio chamado com a resposta do C8; não vale para o estado que ele encontra depois dos outros fechamentos. Existência desse legado datado em produção: **[não verificado]**. (`app/chamados/models.py:707`, `app/chamados/models.py:709`, `app/ponto/services/cobertura.py:317`)

4. **E2 transforma silêncio pela tranca em absolvição pela célula.**

   Com célula não acusadora, marco pendente e marco perguntado apagado, o pré-teste anterior impedia o carimbo. Agora o C6 pode devolver `False` exclusivamente porque o dia está trancado; E2 grava `materializada_em` e `celula_absolveu`, embora a tranca expressamente não constitua absolvição. (`app/chamados/reconciliador.py:584`, `app/chamados/reconciliador.py:611`, `app/chamados/reconciliador.py:615`, `app/chamados/juizes.py:1010`, `app/chamados/juizes.py:1012`)

   O escritor irmão já distingue essas causas e proíbe carimbar pergunta morta pela leitura da tranca. Destrancar só limpa a via `competencia_trancada`; não limpa `celula_absolveu`. O materializador, por sua vez, exige `materializada_em=NULL`. Assim, o carimbo indevido pode sobreviver à reabertura e excluir uma resposta da materialização. (`app/chamados/services/disputa_emissao.py:1549`, `app/chamados/models.py:2541`, `app/chamados/models.py:2534`, `app/chamados/services/materializacao.py:1286`)

   **Resposta à pergunta 4:** no universo atual selecionado por E2, não identifiquei outro delta funcional normal além da tranca. Validação e materialização prévias estão excluídas; marco aceso já passava pelo pré-teste removido; pendência sem marco aceso e sem tranca continua viva no C6. (`app/chamados/reconciliador.py:584`, `app/chamados/juizes.py:1005`, `app/chamados/juizes.py:1032`, `app/chamados/juizes.py:1068`, `app/chamados/juizes.py:1080`)

5. **Os testes mordem parcialmente; a reversão exata da cura E2 permanece indistinguível.** [nao verificado]

   | Mutante | Resultado dedutível pela leitura |
   |---|---|
   | Restaurar exatamente o pré-teste removido de E2 | Preserva os dois resultados esperados: saída acesa passa; apagada continua sem carimbo. Os testes não distinguem a reversão. (`app/chamados/tests/test_e2_pergunta_so_ao_c6.py:26`, `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:63`, `app/chamados/juizes.py:1068`) |
   | Restaurar o teto antigo, sem exceção para marco aceso | Contraria o primeiro teste. Esse é o mutante que o teste distingue. (`app/chamados/tests/test_e2_pergunta_so_ao_c6.py:63`) |
   | Restaurar `excluir=self` no gate | Contraria a sobrevivência do novo e a trilha exigidas no teste do signal. (`app/ponto/tests/test_fechar3_troca_escala_gate.py:249`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:255`) |
   | Remover `[FIO-DUP-C8]` | Contraria fechamento e reaponte exigidos no terceiro teste novo. (`app/ponto/tests/test_fechar3_troca_escala_gate.py:313`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:323`) |
   | Restaurar somente a ordenação, mantendo o gate novo | As expectativas novas não distinguem essa restauração. (`app/chamados/models.py:709`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:251`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:303`) |

   Os testes da extensão usam mesmo módulo, mesmos marcos e perguntas sem resposta. Não cobrem outro turno, resposta escrita nem a sequência temporal de desaparecimento da cobrança. Os testes E2 também omitem `data_evento`, deixando a tranca fora do caso. (`app/ponto/tests/test_fechar3_troca_escala_gate.py:272`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:283`, `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:58`, `app/chamados/juizes.py:887`)

O censo dos chamadores de `.reconciliar()` resulta nos efeitos abaixo; “sem mudança direta” refere-se ao caminho descrito, preservados seus insumos. (`app/chamados/models.py:645`, `app/chamados/models.py:688`, `app/chamados/models.py:699`)

| Leitor/chamador | O que passa a observar |
|---|---|
| `app/chamados/signals.py:42` | Pode receber recusa para o novo antes fechável; a nota Defesa 1 depende do retorno. |
| `app/chamados/services/materializacao.py:539` e `:711` | Uma chamada descarta o retorno; a outra o propaga. O novo gate pode manter o destino vivo. |
| `app/ponto/management/commands/detectar_ausencias.py:158` e `:816` | LIMBO normalmente não chega ao C8; os fechamentos de cobrança condicionam suas notas ao retorno. |
| `app/chamados/services/limbo_folgas.py:101` e `:243` | Pai cadastral normalmente permanece igual; filho agrupado sob pai vivo passa antes do C8. (`app/chamados/models.py:699`) |
| `app/ponto/services/cobertura.py:122` e `:317` | Mudam retorno, recusados/explicados e, pela ordem removida, o apoio disponível aos seguintes. |
| `app/ponto/services/veredito_celula.py:174` e `:183` | Fecha perguntas antes do lote; decisão administrativa na célula regulariza antes do C8. (`app/chamados/models.py:1230`) |
| `app/chamados/services/adesao.py:115` e `:150` | Agrupamento vivo e pai cadastral normalmente evitam a comparação alterada. (`app/chamados/models.py:699`, `app/chamados/models.py:1192`) |
| `app/ponto/services/lastro.py:306` e `:317` | Marco efetivamente satisfeito regulariza antes do C8; pode encerrar o sobrevivente que sustentou outros fechamentos. (`app/chamados/models.py:1236`) |
| `app/ponto/services/triagem_batida.py:580` e `:588` | Mantém proteção de outro turno aberto e seleção por módulo; recusa do novo gate impede os efeitos posteriores. |
| `app/ponto/services/ausencia.py:678` | Pode observar outros apoios vivos após a mudança de sequência; o módulo externo entra na guarda pela categoria `ausencia`. (`app/ponto/services/ausencia.py:653`, `app/chamados/models.py:1197`) |
| `app/chamados/services/feriado_reavaliar.py:47` | Espuriedade reconhecida regulariza antes do C8; nota/trilha continuam condicionadas ao fechamento. (`app/chamados/models.py:1252`) |
| `app/chamados/services/silencio_ausencia.py:93` | Aprovação e pré-filtro permanecem; se chegar à comparação alterada, a recusa impede o fechamento. |
| `app/ponto/services/cluster_espurio.py:80` | Classe emitida fora da guarda de furo: sem mudança direta pelo C8. (`app/ponto/services/cluster_espurio.py:89`, `app/chamados/models.py:1192`) |
| `app/ponto/services/entrada_sincronizada.py:28` | Exige marcos acesos antes da porta; normalmente regulariza antes do C8. (`app/chamados/models.py:1236`) |
| `app/chamados/management/commands/auditar_chamados_orfaos.py:67` | Pode trocar fechamento/OK por recusa; nota depende do retorno. |
| `app/colaboradores/management/commands/marcar_feriado_do_posto.py:126` | Premissa espúria reconhecida passa antes do C8; nota e contador dependem do retorno. (`app/chamados/models.py:1252`) |
| `app/colaboradores/services/pos_vinculo.py:36` | Pais cadastrais normalmente ficam fora da guarda alterada; retorno governa nota/contador. |
| `app/ponto/management/commands/curar_residuo_bug115.py:151` | Continua subordinado ao gate; nova recusa impede nota e contagem de encerramento. |
| `app/ponto/management/commands/detectar_intervalo_ausente.py:128` | Premissa específica morta não garante dia regularizado; a proteção do eleito pode mudar fechamento e contador, inclusive no dry-run. |
| `app/chamados/reconciliador.py:520` | Retenções continuam anteriores ao gate; fechamento, justificativa e resolução das perguntas dependem de sua aprovação. |
| `app/ponto/services/cartorio.py:1225`, `:1259` e `:1341` | Perde a ordenação e ganha tentativas de fechamento de outros vivos, inclusive os poupados pelo laço externo. |

Quanto aos campos e autoridades pedidos:

- **`status_local`:** a escrita continua em `_gravar_estado`; o diff altera decisões e sequência que chegam ao escritor, com efeitos no signal resolvedor e no rejulgamento do cartório. (`app/chamados/models.py:618`, `app/chamados/models.py:222`, `app/chamados/models.py:279`, `app/chamados/signals.py:76`, `app/chamados/signals.py:165`)
- **`_f1_dedup`:** mantém escritor e consumidor. A tentativa protegida deixa de preenchê-lo; a trilha continua posterior à aprovação da pergunta viva. (`app/chamados/models.py:717`, `app/chamados/models.py:645`, `app/chamados/models.py:648`, `app/chamados/models.py:654`)
- **`chamado_destino`:** muda qual sobrevivente recebe a pergunta; a escrita continua pela porta `reapontar_resolvedora`, com `resolvedora_valida` e relavra. (`app/chamados/models.py:371`, `app/chamados/models.py:418`, `app/chamados/services/fio.py:837`, `app/chamados/services/fio.py:845`)
- **`materializada_em`/`via_resolucao`:** E2 ganha o alcance indevido da tranca; o resolvedor por chamado mantém sua exclusão explícita. (`app/chamados/reconciliador.py:615`, `app/chamados/services/disputa_emissao.py:1551`)
- **C6/C8:** suas funções não recebem alteração de corpo neste diff; mudam os argumentos ao C8 e os estados que suas consultas encontram. O C6 responde vitalidade, incluindo silêncio pela tranca, não a causa “célula absolveu”. (`app/chamados/models.py:709`, `app/chamados/juizes.py:1625`, `app/chamados/juizes.py:1010`, `app/chamados/reconciliador.py:617`)
- **Cadastro:** C6 está registrado como `pergunta_viva`; C8 está registrado como `ChamadoColaborador.abrir`, e a documentação confirma essa autoridade. Chamar `cobranca_viva_do_dia` de “C8” não demonstra, por si, conformidade com esse registro. (`app/core/juizes.py:238`, `app/core/juizes.py:240`, `app/docs/JUIZES.md:54`)

Execução das suítes/mutantes, incidência dos contraexemplos em produção e resultado após uma agenda real completa de crons: **[não verificado]**.

## 31/10 03:08 · sessao 51114a07024e · - · - · origem=Ronald (tmux)

Dois contraexemplos condicionais pelo código; ocorrência em produção [não verificado]:

1. **Zero vivos depois, um antes**: A velho `intervalo_pendente` cobra intervalo apagado; B novo `batida_ausente` cobra S apagada; dia abaixo do piso, sem perguntas. Fechar disputa de B: antes B fechava apoiado A; agora B recusa por ser C8. Fechar disputa de A: antes A sozinho recusava; agora A fecha apoiado B. Quando somente S acende, B fecha por premissa regularizada; intervalo continua apagado e nenhuma cobrança renasce abaixo do piso. Referências: `app/chamados/signals.py:42`, `app/chamados/models.py:699`, `app/chamados/models.py:709`, `app/chamados/models.py:1236`, `app/ponto/services/cartorio.py:1398`, `app/ponto/services/cartorio.py:1460`. Precisa explicitar ausência de outros chamados/perguntas e de toque que atravesse piso.

2. **Dois vivos depois, um antes**: A velho e B novo cobram D; B não tem perguntas; A ancora pergunta viva de D−1, costura legítima por turno noturno. Agora B recusa por ser C8; A tenta fechar, mas sua pergunta procura C8 em D−1, sem candidato, e segura A. Antes B fechava apoiado A, restando A. Referências: `app/chamados/models.py:369`, `app/chamados/models.py:375`, `app/chamados/models.py:710`, `app/chamados/juizes.py:1626`, `app/chamados/juizes.py:1719`. Cartório pula A com fio vivo e B sem perguntas: `app/ponto/services/cartorio.py:1325`.

**Módulo externo também pode mudar indiretamente**: retirada da ordenação em cobertura faz B novo com marco já atendido fechar antes de A externo; A perde sua cobrança de apoio. Exemplo externo aceito pelo código: `entrada_bloqueada_turno_aberto`, categoria `turno_aberto`, ligado a D por célula/contexto. Ambos passam o filtro da cobertura. Antes A fechava em B; agora permanece. Referências: `app/ponto/services/cobertura.py:225`, `app/ponto/services/cobertura.py:309`, `app/chamados/catalogo/premissa.py:48`, `app/chamados/models.py:1197`, `app/ponto/views.py:297`. Existência desse legado datado em produção [não verificado].

Mapa completo dos chamadores foi enviado nas mensagens anteriores. `_f1_dedup` mantém escritor e consumidor: preenchimento em `app/chamados/models.py:717`, limpeza/leitura em `app/chamados/models.py:645`, emissão somente após aprovação da pergunta em `app/chamados/models.py:649`. O diff muda quais tentativas chegam ao preenchimento; nenhum escritor adicional de `status_local` foi identificado no diff, que continua chegando a `app/chamados/models.py:227` e `app/chamados/models.py:279`.

## 31/10 03:05 · sessao 4b8ade7fe326 · - · - · origem=Ronald (tmux)

Achado principal: a extensão `[FIO-DUP-C8]` trata todo `y` vivo do lote e em `CALADOS_POR_ISENCAO` como duplicata; não compara `entrada_timestamp`, não exige mesmo módulo nem repete a proteção de `resposta_colab` aplicada a `x`. (app/ponto/services/cartorio.py:1328; app/ponto/services/cartorio.py:1341)

- **Outro turno do mesmo dia:** caminho demonstrável pelo código. `vivos` agrupa pelo dia declarado do chamado, sem chave de turno; C8 escolhe pelo dia e antiguidade, também sem chave de turno. A extensão chama `y.reconciliar()`, cujo gate admite dedup sustentado por `x`. Se houver pergunta viva, `resolvedora_valida` aceita imediatamente quando as datas são iguais; não verifica as entradas distintas. Isso conflita com a identidade registrada em `irmao_vivo_do_turno`: dois turnos no mesmo dia são dois fatos. (app/ponto/services/cartorio.py:85; app/ponto/services/cartorio.py:94; app/chamados/juizes.py:1625; app/chamados/models.py:709; app/chamados/models.py:371; app/chamados/juizes.py:1710; app/chamados/juizes.py:1647)

- **Chamado com resposta:** a proteção de texto só vale para `x`; `y` respondido pode fechar pela extensão. Entretanto, não demonstrar isso como “resposta enterrada”: a pergunta viva pode ser reapontada para `x`, preservando seu texto e sua vitalidade. O efeito comprovável é fechar o chamado respondido e mudar sua âncora, contornando a exclusão explícita do laço externo. (app/ponto/services/cartorio.py:1328; app/ponto/services/cartorio.py:1343; app/chamados/models.py:371; app/chamados/models.py:416; app/chamados/juizes.py:1020)

- **Outro módulo:** alcança `batida_ausente`, `turno_aberto` e `intervalo_pendente`, pela interseção entre a lista do cartório e CALADOS. Só a diferença de módulo não prova fechamento indevido: já há testes autorizando dedup entre `batida_ausente` e `turno_aberto`. (app/chamados/catalogo/furo_modulos.py:15; app/chamados/catalogo/modulos.py:235; app/ponto/services/cartorio.py:1343; app/chamados/tests/test_fechar3_retorno_do_gate.py:85)

**Mordida dos testes, por leitura estática:** [nao verificado]

- Reverter o gate para `excluir=self` contraria a expectativa do novo teste com pergunta do mais novo e signal do E2: ele exige o novo vivo e trilha de fechamento do velho sustentada nele. (app/ponto/tests/test_fechar3_troca_escala_gate.py:242; app/ponto/tests/test_fechar3_troca_escala_gate.py:249; app/ponto/tests/test_fechar3_troca_escala_gate.py:255)
- Remover a extensão deixa o velho com pergunta viva fora da reconciliação FIO-DUP normal, contrariando a expectativa de fechamento/reaponte do terceiro teste. (app/ponto/services/cartorio.py:1326; app/ponto/tests/test_fechar3_troca_escala_gate.py:313; app/ponto/tests/test_fechar3_troca_escala_gate.py:323)
- Os testes novos da extensão usam o mesmo módulo, os mesmos marcos e perguntas sem resposta; não exercitam outro turno, outro módulo nem resposta escrita. Retirar filtros ou ampliar indevidamente a extensão nesses eixos pode continuar verde. (app/ponto/tests/test_fechar3_troca_escala_gate.py:272; app/ponto/tests/test_fechar3_troca_escala_gate.py:283; app/ponto/tests/test_fechar3_troca_escala_gate.py:318)
- Restaurar somente a ordenação pelo helper, mantendo o gate atual, não é distinguido pelas expectativas de resultado: o próprio gate já protege a identidade escolhida pelo C8. (app/chamados/models.py:709; app/ponto/tests/test_fechar3_troca_escala_gate.py:251; app/ponto/tests/test_fechar3_troca_escala_gate.py:303)

Execução dos testes/mutantes e ocorrência desses casos em produção: **[não verificado]**. [nao verificado]

## 31/10 03:05 · sessao 46ca8e8ed8eb · - · - · origem=Ronald (tmux)

**E2 falha ao transformar tranca reversível em `celula_absolveu`.** Para pergunta sem carimbo/validação, célula não acusadora com marco pendente e marco perguntado apagado, o pré-teste removido impedia o carimbo. Agora `dia_da_pergunta_trancado()` faz o C6 devolver `False` antes do teto, e E2 grava `materializada_em` e `VIA_CELULA_ABSOLVEU`. O escritor irmão preserva expressamente a recusa de carimbar por tranca. (app/chamados/reconciliador.py:584, app/chamados/reconciliador.py:611, app/chamados/juizes.py:1010, app/chamados/services/disputa_emissao.py:1549)

O efeito persiste no carimbo: `reabrir_por_competencia()` só limpa `VIA_COMPETENCIA_TRANCADA`; `reabrir_por_resposta()` só limpa `VIAS_SEM_LASTRO`, que não inclui `celula_absolveu`; o materializador exige `materializada_em=NULL`. Assim, uma resposta posteriormente viva pode continuar excluída da seleção de materialização. (app/chamados/models.py:2541, app/chamados/models.py:2534, app/chamados/models.py:2893, app/chamados/services/materializacao.py:1286)

**Os dois testes E2 deixam verde o mutante que desfaz exatamente esta cura.** Reintroduzir `if marcos_pendentes(p.celula) and fato_do_marco_no_espelho(p) is not True: continue` preserva ambos resultados: saída acesa passa pelo pré-teste e C6 mata; saída apagada é preservada por ambos. Eles mordem apenas a guarda mais antiga que retinha qualquer pergunta com marco pendente. Os testes também omitem `data_evento`, portanto não exercitam a tranca. (app/chamados/tests/test_e2_pergunta_so_ao_c6.py:26, app/chamados/tests/test_e2_pergunta_so_ao_c6.py:58, app/chamados/tests/test_e2_pergunta_so_ao_c6.py:63, app/chamados/juizes.py:887, app/chamados/juizes.py:1067)

**Pergunta específica 4:** por inspeção de fluxo, no universo selecionado por E2, não há outro delta normal além da tranca. `validada_em` e `materializada_em` nulos excluem as demais mortes antecipadas do C6; resposta concordante exige marco aceso, que já passava pela guarda antiga; pendente sem marco aceso e sem tranca acaba vivo. (app/chamados/reconciliador.py:584, app/chamados/juizes.py:969, app/chamados/juizes.py:1005, app/chamados/juizes.py:1044, app/chamados/juizes.py:1068)

**Escritores e leitores:**

- `retratar_perguntas_absolvidas_por_celula` muda os carimbos apenas no caso acima; `resolver_perguntas_por_chamado_resolvido` não foi alterado e mantém proteção explícita da tranca no ramo com destino. O fechamento do chamado pode dispará-lo pelo signal. (app/chamados/reconciliador.py:615, app/chamados/services/disputa_emissao.py:1536, app/chamados/services/disputa_emissao.py:1551, app/chamados/signals.py:76)
- `chamado_destino` continua passando por `reapontar_resolvedora`, que consulta `resolvedora_valida`, protege o fio, grava a FK e relavra o veredito. A reata no gate usa essa porta. O diff pode mudar **qual** chamado chega a essa reata ao mudar o sobrevivente, mas não cria escritor adicional da FK. (app/chamados/services/fio.py:829, app/chamados/services/fio.py:837, app/chamados/services/fio.py:845, app/chamados/models.py:413)
- O plano do gate procura sobrevivente por pergunta, ainda com `excluir=self`, e exige `resolvedora_valida`. Este juiz aceita mesmo dia e possui exceção para jornada atravessando meia-noite; não distingue dois turnos no mesmo dia quando as datas coincidem. (app/chamados/models.py:367, app/chamados/models.py:371, app/chamados/juizes.py:1710, app/chamados/juizes.py:1719)
- O C6 não mudou neste diff; mudou sua autoridade sobre a escrita de E2. Seus retornos continuam incluindo “morta pela tranca”, que não significa “célula absolveu”. (app/chamados/juizes.py:1005, app/chamados/reconciliador.py:611)

**Registro:** o C6 registrado é `pergunta_viva`; o C8 registrado é `ChamadoColaborador.abrir`, não `cobranca_viva_do_dia`. A tabela documental confirma “duplicado = abrir”. Portanto a denominação “C8” usada pelo diff para o sobrevivente não coincide com o registro atual. (app/core/juizes.py:238, app/core/juizes.py:240, app/docs/JUIZES.md:54)

Não executei testes ou mutantes; o resultado de execução e a ocorrência desses estados em produção são [não verificado]. [nao verificado]

## 31/10 03:01 · sessao 0b50d4b4a7dc · O261-E-F1-CLASSE-4 · sonda · origem=Code

**(1) tipo_dia_por_trabalha**

| trabalha | ata.tipo_dia | Contagem | Referência |
|---|---|---:|---|
| True | 'trabalho' | 46093 | [nao verificado] |
| False | 'folga' | 22128 | [nao verificado] |
| True | None | 10409 | [nao verificado] |
| False | None | 5382 | [nao verificado] |
| True | 'ausencia' | 2904 | [nao verificado] |
| False | 'ausencia' | 1609 | [nao verificado] |
| True | 'falta' | 375 | [nao verificado] |
| True | 'folga' | 1885 | [nao verificado] |
| None | 'trabalho' | 52007 | [nao verificado] |
| False | 'trabalho' | 154 | [nao verificado] |
| None | 'folga' | 222 | [nao verificado] |
| None | 'ausencia' | 78 | [nao verificado] |
| None | None | 66 | [nao verificado] |

**(2) universo_chamados_vivos_com_celula: 2964** [nao verificado]

**(3) divergem**

| Chave (trabalha, tipo_dia, em CALADOS_POR_ISENCAO, modulo) | Contagem | Chamado de exemplo | Referência |
|---|---:|---:|---|
| (None, 'trabalho', False, 'ancora_limbo') | 1 | 12982 | [nao verificado] |
| (True, 'ausencia', False, 'batida_fora_escala_leve') | 1 | 17793 | [nao verificado] |
| (True, 'ausencia', False, 'colaborador_em_ferias') | 1 | 28767 | [nao verificado] |
| (True, 'ausencia', False, 'desligamento_revisao') | 10 | 35918 | [nao verificado] |
| (True, 'ausencia', False, 'disputa_supervisao') | 3 | 20782 | [nao verificado] |
| (True, 'ausencia', False, 'sem_previsao_escala') | 5 | 35217 | [nao verificado] |
| (True, 'falta', False, 'desligamento_revisao') | 4 | 35915 | [nao verificado] |
| (True, 'falta', False, 'disputa_supervisao') | 1 | 15402 | [nao verificado] |
| (True, 'folga', False, 'desligamento_revisao') | 1 | 16004 | [nao verificado] |
| (True, 'folga', False, 'sem_previsao_escala') | 1 | 32564 | [nao verificado] |
| (True, 'folga', True, 'batida_ausente') | 10 | 34867 | [nao verificado] |

## 31/10 02:37 · sessao 6a09fec44f3d · O261-E-F5C · verificador · origem=Code

**FINAL: BLOQUEIA.** O diff mantém divergência entre painel e juiz, promete uma ação que o fio não garante e acrescenta um teste que falha antes das asserções pretendidas (`app/chamados/services/painel.py:188`, `app/chamados/services/painel.py:326`, `app/folha/fora_da_folha.py:43`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:147`).

1. **Escritores e leitores**

| Valor | Quem escreve | Quem lê |
|---|---|---|
| `porta['fila']` | Tupla ordenada produzida pelo juiz em `app/chamados/juizes.py:1226`, devolvida em `app/chamados/juizes.py:1227`. A entrada anterior, `fila_cel`, vem de `alimentar_portas`, em `app/chamados/juizes.py:1148`. | `o_que_segura`, em `app/folha/fora_da_folha.py:196`; teste direto em `app/chamados/tests/test_o261e_f5b_juiz_porta.py:158`; comparações do dict inteiro em `app/chamados/tests/test_o261e_f5b_juiz_porta.py:138`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:164` e `app/chamados/tests/test_o261e_f5b_juiz_porta.py:182`. |
| `FORA_DA_LISTA_PADRAO` | Declaração em `app/chamados/services/verbos.py:18`. | Painel em `app/chamados/services/painel.py:327`; juiz em `app/chamados/juizes.py:1226`. |
| `GRUPOS['C']` | Texto em `app/folha/fora_da_folha.py:36`. | Tooltip via `retrato_folha.ajuda`, em `app/folha/retrato_folha.py:70`; enumeração das chaves em `app/folha/fora_da_folha.py:139`; testes em `app/folha/tests/test_haiku_fora_da_folha.py:123`, `app/folha/tests/test_retrato_folha_quadros.py:1276` e `app/folha/tests/test_retrato_folha_quadros.py:1285`. |
| `DESTRAVA['chamado']` | Texto em `app/folha/fora_da_folha.py:43`. | Ferramenta em `app/folha/fora_da_folha.py:197`; drawer em `app/folha/retrato_folha.py:1069`; template exibe `r.valor` em `app/templates/folha/_retrato_o_que_fazer.html:37`. Testes em `app/folha/tests/test_haiku_fora_da_folha.py:137`, `app/folha/tests/test_haiku_fora_da_folha.py:483`, `app/folha/tests/test_retrato_folha_dono_sao_tres.py:119` e `app/folha/tests/test_retrato_folha_drawer.py:226`. |
| `o_que_segura()['dias'][*]['chamado']` | Cópia de `porta['fila']` para lista em `app/folha/fora_da_folha.py:196`, incluída no retorno em `app/folha/fora_da_folha.py:199`. | Asserção do teste novo em `app/folha/tests/test_haiku_fora_da_folha.py:138`. Consumidor externo em produção: **[nao verificado]**. |

O vigia lê `verbos` e `rota`, **não a nova `fila`**. Assim, uma porta com `fila=()` e verbo `historico` continua sendo contada como porta alcançável (`app/chamados/services/vigia_fio.py:596`, `app/chamados/services/vigia_fio.py:598`, `app/chamados/services/vigia_fio.py:601`).

As pautas dos discordantes leem indiretamente o juiz por `_fio_do_dia`, comparando apenas com `FIO_SEM_PORTA`; não consomem os números de `chamado` (`app/ponto/services/pautas_dos_discordantes.py:59`, `app/ponto/services/pautas_dos_discordantes.py:74`, `app/ponto/services/pautas_dos_discordantes.py:76`).

Os dois asserts diretos do dict inteiro foram atualizados. A comparação lote versus individual compara duas respostas atuais; o mock antigo do vigia continua sem `fila`, mas seu consumidor acessa somente `rota/verbos` (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:138`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:164`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:182`, `app/chamados/tests/test_o261e_f5_alcance.py:307`, `app/chamados/services/vigia_fio.py:598`).

2. **Painel padrão versus `fila`: não são equivalentes — bloqueia**

O filtro final por verbo usa a mesma constante, mas o universo anterior difere: o juiz começa por `qs_na_fila`, que elimina qualquer silêncio futuro; a lista padrão elimina os verbos `arquivado/historico` depois da partição (`app/chamados/juizes.py:1144`, `app/chamados/services/painel.py:188`, `app/chamados/services/painel.py:326`).

Contraexemplo de código: chamado **da célula**, status `aberto`, módulo `gps_ausente`, silêncio futuro:

- O módulo é auditoria e produz verbo `None`; o silêncio só substitui verbos diferentes de `None` (`app/chamados/catalogo/natureza.py:18`, `app/chamados/catalogo/natureza.py:44`, `app/chamados/catalogo/natureza.py:63`, `app/chamados/services/verbos.py:48`).
- O painel padrão mostra: `aberto` pertence a `COBRANCA`, e `None` não pertence à constante de exclusão (`app/chamados/catalogo/motor.py:29`, `app/chamados/services/painel.py:314`, `app/chamados/services/painel.py:327`, `app/chamados/services/verbos.py:18`).
- O juiz exclui antes da partição, pelo silêncio futuro (`app/chamados/services/painel.py:188`, `app/chamados/juizes.py:1144`).

Para os demais resultados possíveis: `validar/cobrar/decidir` sem silêncio entram nos dois; `arquivado` sai nos dois; `historico` sai da lista e da nova `fila`, embora permaneça em `verbos`; `None` sem silêncio futuro entra nos dois, quando o status é de cobrança (`app/chamados/catalogo/natureza.py:57`, `app/chamados/services/verbos.py:48`, `app/chamados/services/painel.py:314`, `app/chamados/services/painel.py:327`, `app/chamados/juizes.py:1218`, `app/chamados/juizes.py:1226`).

No mesmo estado e instante, sem filtros extras/paginação e comparando chamados da célula, `fila` é subconjunto da lista padrão; não há caso inverso nessa composição de filtros. A exclusão de âncoras de outra célula é adicional e intencional (`app/chamados/services/painel.py:188`, `app/chamados/services/painel.py:314`, `app/chamados/services/painel.py:327`, `app/chamados/juizes.py:1217`, `app/chamados/juizes.py:1226`). Ocorrência desses contraexemplos em produção: **[nao verificado]**.

3. **Universalidade das frases**

**`GRUPOS['C']`: compatível com a escada, sem bloqueio isolado.** Nas duas rotas, `chamado` significa que os verbos da porta não contêm `cobrar` nem `validar`. Isso não garante visibilidade na lista padrão (`app/chamados/juizes.py:1175`, `app/chamados/juizes.py:1227`, `app/folha/fora_da_folha.py:36`).

**`DESTRAVA['chamado']`: não universal — bloqueia.** A escada também devolve `chamado` para `None` e `historico`; portanto não garante a ação `decidir` (`app/chamados/juizes.py:1175`, `app/chamados/catalogo/natureza.py:63`, `app/chamados/services/verbos.py:51`, `app/folha/fora_da_folha.py:43`).

Contraexemplo: chamado aberto da célula, silêncio vencido, nenhuma pergunta. Ele passa por `qs_na_fila`, vira `historico`, produz `porta_fila`, fio `chamado` e `fila=()`. A frase manda abrir um chamado da fila ou uma pergunta no app, embora nesse caso não haja nenhum desses alvos visíveis nomeados (`app/chamados/services/painel.py:188`, `app/chamados/services/verbos.py:51`, `app/chamados/juizes.py:1217`, `app/chamados/juizes.py:1226`, `app/folha/fora_da_folha.py:43`).

Na `porta_pergunta`, a existência da pergunta também não obriga o verbo a ser `decidir`: o juiz aceita o verbo da âncora, e a natureza de auditoria pode produzir `None` mesmo com pergunta (`app/chamados/juizes.py:1221`, `app/chamados/juizes.py:1222`, `app/chamados/catalogo/natureza.py:44`, `app/chamados/catalogo/natureza.py:63`).

4. **Drawer: igualdade da frase, incoerência da ação — bloqueia**

Para o mesmo fio, ferramenta e drawer usam literalmente o mesmo `DESTRAVA`; essa parte está coerente (`app/folha/fora_da_folha.py:197`, `app/folha/retrato_folha.py:183`, `app/folha/retrato_folha.py:1069`).

Porém o botão de `chamado` continua **“Validar no fio”**, enquanto a nova frase manda **decidir** e afirma ausência de validar. O botão chega ao template com esse rótulo (`app/folha/retrato_folha.py:977`, `app/folha/retrato_folha.py:1024`, `app/templates/folha/_retrato_o_que_fazer.html:62`, `app/folha/fora_da_folha.py:43`).

O drawer também não recebe o novo dado numérico: o retrato grava fio, datas e outros eixos, e a view lê essa linha gravada; não chama `o_que_segura` nesse fluxo (`app/folha/retrato_folha.py:1177`, `app/folha/retrato_folha.py:1186`, `app/colaboradores/views.py:980`, `app/colaboradores/views.py:984`). Paridade temporal entre retrato e ferramenta em produção: **[nao verificado]**.

5. **Checklist e mordida dos testes** [nao verificado]

| Item | Veredito |
|---|---|
| Segundo juiz registrado | Não identificado no diff: a porta já está registrada, a escada permanece nela e a folha lê seu resultado (`app/core/juizes.py:253`, `app/chamados/juizes.py:1175`, `app/folha/fora_da_folha.py:68`). |
| Segundo escritor fora da porta | Não identificado nas alterações produtivas: os novos valores são retornos de leitura, sem nova persistência (`app/chamados/juizes.py:1227`, `app/folha/fora_da_folha.py:199`). |
| Relógio fora da borda | Nenhuma leitura nova no diff; a cadeia existente lê instantes internos separados em `qs_na_fila` e `particionar_verbos`. Não sustenta promessa de fotografia temporal única (`app/chamados/services/painel.py:188`, `app/chamados/services/verbos.py:35`, `app/chamados/juizes.py:1153`). |
| Derivação paralela | A constante é compartilhada, mas isso não unifica os filtros anteriores; permanece a divergência demonstrada para `None` com silêncio futuro (`app/chamados/services/painel.py:188`, `app/chamados/services/painel.py:327`, `app/chamados/juizes.py:1226`). **Bloqueia a equivalência.** |
| Ordem de trava | Ordem efetivamente executada nesta obra: **[nao verificado]**. |
| Pouso antes do pré-requisito | Sequência real de suite/verificação/pouso: **[nao verificado]**. A regra exige suite inteira verde antes do opus final (`CLAUDE.md:600`). | [nao verificado]

O teste **`test_MORDE_a_frase...` morde a redação anterior fornecida no diff**: suas substrings proibidas rejeitam “sem pergunta viva” e “chamado N”. Contudo, os dois mocks usam somente `decidir`; não exercitam a porta real, `None`, `historico` ou o botão do drawer. Portanto não demonstram a universalidade da nova frase (`app/folha/tests/test_haiku_fora_da_folha.py:123`, `app/folha/tests/test_haiku_fora_da_folha.py:126`, `app/folha/tests/test_haiku_fora_da_folha.py:133`, `app/folha/tests/test_haiku_fora_da_folha.py:137`).

O teste **`test_MORDE_silenciado...` não oferece RED útil como escrito — bloqueia**: importa `Chamado`, inexistente no módulo; o modelo e o helper usam `ChamadoColaborador`. Isso causa `ImportError` antes dos subtestes, também no pai descrito pelo diff (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:147`, `app/chamados/models.py:21`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:62`).

Mesmo corrigindo esse import, **2099 continua quebrando na versão curada**: o silêncio futuro elimina o chamado antes da partição; sem pergunta, o juiz retorna `None`, mas o teste indexa `porta['verbos']` (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:150`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:156`, `app/chamados/services/painel.py:188`, `app/chamados/juizes.py:1223`). O ramo de 2020 poderia distinguir a ausência de `fila` no pai, mas está inacessível pelo erro anterior (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:147`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:158`).

**RED/GREEN observado por suite Django contra `dc3ba57f` e contra esta obra: [nao verificado].**

## 31/10 02:36 · sessao f790b02d836e · - · - · origem=Ronald (tmux)

**Bloqueia: o teste novo de silenciamento não certifica a cura.** [nao verificado]

- `test_MORDE_silenciado_na_celula_nao_e_numero_da_fila` importa `Chamado`, símbolo inexistente no módulo; o modelo é `ChamadoColaborador`. Portanto termina em `ImportError` antes dos subtestes, tanto na cópia quanto no pai reconstruído pelo diff. Referências: `app/chamados/tests/test_o261e_f5b_juiz_porta.py:147`, `app/chamados/models.py:21`, helper correto em `app/chamados/tests/test_o261e_f5b_juiz_porta.py:62`.
- Corrigido somente esse import, o caso de 2099 continua errado: `qs_na_fila` exclui o silenciamento futuro antes de alimentar o juiz; sem perguntas, este retorna `None`, e `porta['verbos']` produz `TypeError`. Referências: `app/chamados/services/painel.py:188`, `app/chamados/juizes.py:1144`, `app/chamados/juizes.py:1146`, `app/chamados/juizes.py:1153`, `app/chamados/juizes.py:1223`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:150`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:156`.
- O caso de 2020, depois dessa correção, alcança `porta_fila`, `historico`, `fila=()`, `fio=chamado` na cópia; o pai não devolvia `fila`, de modo que a leitura nova encontra `KeyError`, não comparação comportamental com número indevido. Referências: `app/chamados/juizes.py:1225`, `app/chamados/juizes.py:1227`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:149`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:158`; ausência da chave no pai consta do diff anexo `[nao verificado]` quanto ao checkout histórico.
- O teste novo da frase detecta os textos antigos: `sem pergunta viva`, `nao ha pergunta` e `chamado N` violam suas asserções. Porém a primeira falha textual impede chegar aos mocks naquele código antigo; isso demonstra mordida textual, sem demonstrar por si a correção universal da frase nova. Referências: `app/folha/tests/test_haiku_fora_da_folha.py:123`, `app/folha/tests/test_haiku_fora_da_folha.py:125`, `app/folha/tests/test_haiku_fora_da_folha.py:126`; textos antigos no diff anexo `[nao verificado]` quanto ao checkout histórico.

**Demais itens do checklist:**

- **Segundo juiz registrado: não identifiquei criação pelo diff.** A autoridade já está registrada, a escada continua no juiz e a folha lê sua saída: `app/core/juizes.py:132`, `app/core/juizes.py:253`, `app/chamados/juizes.py:1170`, `app/chamados/juizes.py:1175`, `app/folha/fora_da_folha.py:55`, `app/folha/fora_da_folha.py:68`.
- **Segundo escritor fora da porta: não identifiquei escrita persistente nova.** As alterações produzem valores de retorno e textos de apresentação: `app/chamados/juizes.py:1227`, `app/folha/fora_da_folha.py:196`, `app/folha/fora_da_folha.py:198`.
- **Relógio: nenhuma leitura nova introduzida pelo diff.** A cadeia herda leituras independentes em `app/chamados/services/painel.py:188` e `app/chamados/services/verbos.py:35`; o queryset da fila é consumido para células e novamente para a partição em `app/chamados/juizes.py:1146` e `app/chamados/juizes.py:1153`. Logo não há instante único recebido pela borda nessa cadeia; não atribuir sua introdução à F5C.
- **Fonte única: a constante é compartilhada, mas isso não prova identidade dos universos.** O juiz chega à constante depois do filtro adicional de `qs_na_fila`: `app/chamados/services/verbos.py:18`, `app/chamados/services/painel.py:188`, `app/chamados/services/painel.py:327`, `app/chamados/juizes.py:1226`.
- **Ordem de trava e pouso antes de pré-requisito: execução desta obra `[nao verificado]`.** A norma exige suite inteira verde antes do opus final (`CLAUDE.md:600`) e impede o Codex de aprovar sozinho o pouso (`CLAUDE.md:599`). A dependência documental é F5B antes de F5, mas a mesma linha traz estado “registrada (depois da F5)”; isso é inconsistência documental, não prova da sequência executada: `app/docs/BACKLOG.md:777`. Sem prova da sequência, não acusar pouso indevido; também não certificá-lo `[nao verificado]`.

Verifiquei o import e os resultados 2020/2099 com AST e execução das funções reais isoladas, em memória, com dependências substituídas. Não rodei Django, suite, banco ou logs; resultados integrados e RED empírico em `dc3ba57f` permanecem `[nao verificado]`.

## 31/10 02:36 · sessao 2c86673a353d · - · - · origem=Ronald (tmux)

Achados para consolidar:

1. **DESTRAVA['chamado'] não é universal — bloqueia.** O juiz devolve `chamado` para qualquer conjunto sem `cobrar` nem `validar`, incluindo `None` e `historico`; não garante `decidir` (`app/chamados/juizes.py:1170`, `app/chamados/juizes.py:1175`, `app/chamados/catalogo/natureza.py:63`, `app/chamados/services/verbos.py:48`). Porém a frase manda abrir “o chamado da fila ou a pergunta viva no app” **e decidir** (`app/folha/fora_da_folha.py:43`). Contraexemplo de código: chamado da célula, aberto, silêncio vencido, sem pergunta: entra em `qs_na_fila`, recebe `historico`, mantém porta/fio `chamado`, mas devolve `fila=()` (`app/chamados/services/painel.py:186`, `app/chamados/services/verbos.py:51`, `app/chamados/juizes.py:1217`, `app/chamados/juizes.py:1226`). Não há chamado visível nem pergunta no app nesse caso; ocorrência em produção [nao verificado].

2. **GRUPOS['C'] é compatível com a escada, nas duas rotas — não bloqueia isoladamente.** “Tem porta aberta” vale quando o juiz retorna dict; fio `chamado` significa ausência de `cobrar` e `validar`, independentemente da rota (`app/folha/fora_da_folha.py:36`, `app/chamados/juizes.py:1175`, `app/chamados/juizes.py:1223`). Isso descreve os verbos; não garante que alguma porta esteja visível na lista padrão (`app/chamados/juizes.py:1226`).

3. **Drawer: frase igual; ação do link incoerente — bloqueia a alegação de coerência completa.** A ferramenta usa `DESTRAVA[_f]` e o drawer lê o mesmo mapa por `FIO_DO_CARTORIO`; portanto a frase para o mesmo fio é literalmente igual (`app/folha/fora_da_folha.py:197`, `app/folha/retrato_folha.py:183`, `app/folha/retrato_folha.py:1069`). Mas o fio `chamado` oferece **“Validar no fio”**, enquanto a nova frase diz **“decidir”** e afirma que o painel não põe a porta em validar (`app/folha/retrato_folha.py:977`, `app/folha/fora_da_folha.py:43`). Esse link é materializado por `portas_da_linha` e renderizado no template (`app/folha/retrato_folha.py:1015`, `app/folha/retrato_folha.py:1024`, `app/templates/folha/_retrato_o_que_fazer.html:60`).

4. **Drawer não recebe o novo dado `chamado`.** Seu retrato grava `dono` derivado do grupo, datas e contagens; não grava números de chamados (`app/folha/retrato_folha.py:1177`, `app/folha/retrato_folha.py:1186`). A view lê a linha gravada e chama `o_que_fazer`/`portas_da_linha`; não chama `o_que_segura` (`app/colaboradores/views.py:980`, `app/colaboradores/views.py:984`). Logo a igualdade demonstrada é de frase para o mesmo fio, não de dados atuais ou números apresentados (`app/folha/fora_da_folha.py:196`, `app/folha/retrato_folha.py:1069`). Coerência temporal em produção [nao verificado].

Censo de escritores/leitores:

| Valor | Escritor | Leitores |
|---|---|---|
| `GRUPOS['C']` | `app/folha/fora_da_folha.py:36` | `retrato_folha.ajuda`, pelo mapa fio→letra (`app/folha/retrato_folha.py:70`, `app/folha/retrato_folha.py:177`); chaves de grupos em `fora_da_folha` (`app/folha/fora_da_folha.py:139`); testes (`app/folha/tests/test_haiku_fora_da_folha.py:123`, `app/folha/tests/test_retrato_folha_quadros.py:1276`, `app/folha/tests/test_retrato_folha_quadros.py:1285`). |
| `DESTRAVA['chamado']` | `app/folha/fora_da_folha.py:43` | `o_que_segura` (`app/folha/fora_da_folha.py:197`), drawer (`app/folha/retrato_folha.py:1069`), template via `retrato_fazer` (`app/templates/folha/_retrato_o_que_fazer.html:33`, `app/templates/folha/_retrato_o_que_fazer.html:37`), testes (`app/folha/tests/test_haiku_fora_da_folha.py:137`, `app/folha/tests/test_haiku_fora_da_folha.py:483`, `app/folha/tests/test_retrato_folha_dono_sao_tres.py:119`, `app/folha/tests/test_retrato_folha_drawer.py:226`). |
| `o_que_segura()['dias'][*]['chamado']` | Cópia de `porta['fila']` para lista (`app/folha/fora_da_folha.py:196`, `app/folha/fora_da_folha.py:199`) | Teste novo (`app/folha/tests/test_haiku_fora_da_folha.py:138`). Consumidor produtivo externo ou em mensageria [nao verificado]. |
| `porta['fila']` | `app/chamados/juizes.py:1226`, `app/chamados/juizes.py:1227` | `o_que_segura` (`app/folha/fora_da_folha.py:196`); testes de dict inteiro (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:138`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:164`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:182`) e teste novo (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:158`). |

Vigia e pautas continuam leitores **indiretos** da porta: o vigia lê `verbos` e `rota`, sem usar `fila` (`app/chamados/services/vigia_fio.py:596`, `app/chamados/services/vigia_fio.py:598`, `app/chamados/services/vigia_fio.py:601`); pautas dos discordantes leem `_fio_do_dia` e comparam com `FIO_SEM_PORTA`, sem usar o dado `chamado` (`app/ponto/services/pautas_dos_discordantes.py:59`, `app/ponto/services/pautas_dos_discordantes.py:74`).

O teste novo de frase verifica substrings proibidas e dois mocks com verbo `decidir`; não cobre `None`, `historico` nem a porta real e não chama o drawer (`app/folha/tests/test_haiku_fora_da_folha.py:123`, `app/folha/tests/test_haiku_fora_da_folha.py:126`, `app/folha/tests/test_haiku_fora_da_folha.py:133`, `app/folha/tests/test_haiku_fora_da_folha.py:137`). Ele pode reprovar a frase antiga, mas não prova a universalidade da nova (`app/folha/tests/test_haiku_fora_da_folha.py:124`).

_870 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
