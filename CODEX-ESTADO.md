# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 20:26** · publicado em **06/11/2026 20:26** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 20:26 · sessao f234c37c52bf · - · - · origem=Ronald (tmux)

1. **O RED tem mordida por dedução; execução contra `39324b76` é [nao verificado].** A fixture cria colaborador sem escala/batidas (`app/api/tests/test_api_bater_ponto.py:197-211`). Envia S retroativa em 3h sob relógio congelado (`:243-251`); 3h cabe na janela de 48h (`app/api/hora_do_aparelho.py:63,141-146`), escapa da correção ao vivo (`app/api/views_core.py:592-600`) e, sem escala, escapa da correção retroativa (`:611-617`). `_motivo_da_lei` delega à autoridade (`:374-382`); sem escala não há veto noturno nem teto (`app/escala/servico_jornada.py:224-227,366-369`), e o fallback sem batidas/marcos devolve motivo (`:396-426,469-471`). Portanto SSE nasce em `app/api/views_core.py:649-655` e alcança o bloco que o diff anexo mostra chamando `None.isoformat()` no pai. Isso causa `AttributeError`; no cliente de testes pode propagar como erro antes dos asserts, ainda RED. Histórico Git indisponível nesta cópia e execução real no pai [nao verificado].

2. **O patch atinge o import local dos dois gêmeos.** O patch substitui o atributo do módulo durante o POST (`app/api/tests/test_api_bater_ponto.py:243-251`); ambos executam `from ponto.turnos import dia_do_turno_da_batida as _dtb_sse` dentro da função e chamam o alias imediatamente (`app/api/views.py:775-777`; `app/api/views_core.py:775-777`). A chamada anterior possível é detector→resolver, mas depende de entrada órfã preexistente, ausente nessa fixture (`app/chamados/detector_anomalias.py:29-48`; `app/escala/servico_jornada.py:485-488`). Os demais usos na emissão de disputa ocorrem após o bloco API (`app/api/views_core.py:793-795`; `app/chamados/services/disputa_emissao.py:379-384`).

3. **Qual gêmeo depende do settings; a esteira atual resolve `views.py`.**
   - Runner canônico usa `--settings=config.settings.ci` (`bin/suite.sh:102-103`); CI define `ROOT_URLCONF='config.urls_ci'` (`app/config/settings/ci.py:5`); esse URLconf inclui `api.urls` (`app/config/urls_ci.py:18`); `api.urls` importa `views` e liga `/ponto/bater/` (`app/api/urls.py:2,67`).
   - Sob `config.settings_test`, resolve `views_core.py`: ROOT em `app/config/settings_test.py:9`; core primeiro em `app/config/urls_test.py:8-15`; import e rota em `app/api/urls_core.py:5,14`.
   - O comando/settings efetivamente usado no RED anexado é [nao verificado].

4. **O outro gêmeo tem selo AST real, mas não segunda execução comportamental neste teste.** O selo lê funções de topo dos dois arquivos, compara nomes comuns, ignora comentários via AST e tem whitelist vazia (`app/api/tests/test_contract_paridade_kernels.py:12-39`). Conferi exclusivamente em memória com `python3 -B` e AST: `api_bater_ponto AST identical: True`. Isso cobre a igualdade de implementação atual; executar POST contra ambos permanece [nao verificado].

5. **Há reprovação determinística do selo de relógio nesta árvore.** Extraí por AST, sem importar apps/Django nem escrever arquivos, as funções reais de `app/core/tests/test_selo_teste_sem_relogio.py` e executei a varredura: `novos=[]`; `curados=['api/tests/test_api_bater_ponto.py']`. O novo `freeze_time` faz o regex global considerar o arquivo API inteiro congelado (`app/core/tests/test_selo_teste_sem_relogio.py:28-35`), mas ele continua na lista (`app/core/tests/testes_sem_relogio.txt:5`); `test_a_lista_so_encolhe` exige lista de curados vazia (`app/core/tests/test_selo_teste_sem_relogio.py:72-74`). Portanto essa asserção falha com a cópia atual; execução Django completa [nao verificado].

6. **A retirada de A2 da lista satisfaz o selo por texto, não prova congelamento integral.** A2 importa `freeze_time` (`app/chamados/tests/test_a2_data_evento_batida.py:8`), mas só decora um método (`:51-52`). Outros métodos continuam chamando `timezone.now()` e `timezone.localdate()` fora desse congelamento (`:32,49`). O regex devolve zero para o arquivo inteiro ao encontrar qualquer ocorrência (`app/core/tests/test_selo_teste_sem_relogio.py:28-35`). A retirada consta no delta fornecido; o detector real confirmou em memória que A2 retorna zero e já não está na lista.

## 06/11 20:26 · sessao 7995a06ebdd3 · - · - · origem=Ronald (tmux)

**Q2 — outras origens e segurança da remoção**

- Outra fonte sem `entrada_sem_saida`: `detectar_anomalias()` chama `detectar_bloqueio_turno_orfao`, acrescenta `orfao_14h` e escreve contexto sob **`orfao_14h`**, não `entrada_sem_saida`; uma exceção posterior ao append também preserva o motivo e é capturada (`app/chamados/detector_anomalias.py:31-55`). O resultado entra na API em `app/api/views.py:446` / `app/api/views_core.py:433`.
- A fonte E sem S acrescenta o motivo junto de `entrada_sem_saida` (`app/api/views.py:663-675`; `app/api/views_core.py:660-672`).
- **Não achei caminho alcançável para a nova remoção apagar o órfão do detector:** o escritor de `saida_sem_entrada` exige que `orfao_14h` ainda **não** exista; se o detector já o trouxe, não escreve SSE, logo não entra no novo bloco (`app/api/views.py:652-655`, `775-781`; `app/api/views_core.py:649-652`, `775-781`). A proteção por `entrada_sem_saida` é conservadora; E e S são ramos mutuamente exclusivos pelo `tipo` nessas linhas.
- A garantia depende desse invariante local; uma futura fonte que acrescente órfão depois de escrever SSE precisaria preservar sua origem [nao verificado].

**Q3 — consumidores posteriores**

- API: o teste de lista não vazia decide gravar `ultima_batida`, chamar o helper e marcar `em_disputa`; depois `aviso_do_dia` lê a lista e o JSON a devolve (`app/api/views.py:787-797`, `838-848`; mesmos números no core).
- `aviso_do_dia` consulta apenas a presença de `em_ferias` nessa lista (`app/ponto/services/aviso_da_batida.py:69-71`).
- Helper: retorna sem efeitos com lista vazia; cria uma cópia `sorted(set(motivos))`; abre/agrega, gera perguntas, manda push quando nasce ou aparecem motivos novos (`app/chamados/services/disputa_emissao.py:626-657`; reexport em `app/chamados/models.py:3349`).
- Persistência: agrega motivos em disputa existente (`disputa_emissao.py:506-512`), funde contexto (`518-535`); no nascimento grava chamado e disputa com motivos/contexto (`538-561`).
- Push usa o primeiro motivo e admite lista vazia com frase genérica (`disputa_emissao.py:1574-1589`). [nao verificado]
- Chamado de auditoria de desvio leve vem **antes** do novo bloco e usa `ctx_desvio`, `batida`, `tipo`; não usa `motivos_anomalia` nem SSE (`app/api/views.py:735-768`; core `732-768`).
- Auditor de disputas avalia somente motivos efetivamente persistidos e aceita ausência de pergunta quando o contexto não permite texto datado (`app/chamados/management/commands/auditar_disputas.py:85-95`, `188-195`).
- Esses consumidores não impõem que qualquer S sem entrada necessariamente gere motivo: lista vazia é tratada explicitamente pelo fluxo (`views.py:787`, `disputa_emissao.py:626-627`).

**ESMERIL — `saida_sem_entrada.data`**

A afirmação literal “dia do turno em **todos os escritores**” é falsa para o valor transitório: os gêmeos inicialmente escrevem **dia do relógio**, `agora_local.date()`, e depois normalizam pelo `_T1` ou removem o contexto (`app/api/views.py:655-657`, `775-783`; core `652-654`, `775-783`). Não identifiquei leitor dessa data provisória entre a escrita e a normalização; os consumidores aparecem após `787` e o desvio usa contexto próprio (`views.py:729-768`, `787-795`; core `726-768`, `787-795`).

Outros escritores:

- PWA: consulta `_T1` e só escreve SSE com resposta não nula (`app/ponto/services/triagem_batida.py:726-740`).
- Fábrica genérica: `{motivo: {'data': dia.isoformat()}}`; o caminho do julgamento passa `cel.data` (`app/chamados/services/fabrica_da_celula.py:129-139`, `174-176`).
- Pendentes: lê pks, consulta `_T1`, chama classificação e grava `contexto[_motivo]['data']`; `_motivo` pode ser SSE (`app/chamados/services/disputa_emissao.py:277-296`).
- Fallback do gerador: cria contexto **local**, não o persiste diretamente, com dia obtido de `_T1` e confirmado pela grade (`disputa_emissao.py:65-86`, `342-346`).
- Merge/nascimento persistem esses valores sem reinterpretar a data (`disputa_emissao.py:518-560`).

Leitores:

- Texto da pergunta: `contexto[motivo]['data']`, formatação sem deslocar dia (`app/chamados/catalogo/query.py:58-64`).
- Destino/dia da pergunta: contexto como fallback, comparação com `dia_do_fato(ch)` e conversão para `date` (`disputa_emissao.py:191-196`, `230-250`).
- `data_evento`: contexto → parse; sem resposta e pergunta de hora, não cria pergunta (`disputa_emissao.py:364-367`, `407-430`).
- Plantio: prefere `p.data_evento`; fallback em `contexto[p.motivo]['data']` (`app/chamados/services/materializacao.py:341-349`).
- Auditoria: lê a existência e o valor, sem alterar dia (`app/chamados/management/commands/auditar_disputas.py:75-95`).

Detalhe semântico preexistente: API escreve SSE no **contexto**, mas acrescenta **`orfao_14h`** na lista; o gerador itera a lista por motivo, logo esse caso usa o caminho/fallback de `orfao_14h`, não automaticamente a chave SSE (`app/api/views.py:654-655`; `app/chamados/services/disputa_emissao.py:312`, `337-345`, `364`). A pergunta catalogada para órfão pede **saída**; a de SSE pede **entrada** (`app/chamados/catalogo/perguntas.py:48-58`). Isso não é introduzido pelo delta (`views.py:652-657`; delta fornecido).

**Campos diretamente tocados pelo delta**

| Campo | Escritores | Leitores |
|---|---|---|
| `motivos_anomalia` | detector `detector_anomalias.py:26-55`; API anomalias `views.py:482-522`, SSE `654`, E sem S `671`, grave `733`; nova remoção `781` (core correspondentes `474-514`, `651`, `668`, `730`, `781`) | guardas de duplicidade `views.py:652`, `669`; nova remoção `780`; helper/JSON/aviso `787-848` |
| `contexto_anomalia['saida_sem_entrada']` | API `views.py:655-657` / core `652-654`; delta remove `779`, atualiza `.data` `783` | delta `775`; persistência helper `795`; leitores genéricos acima |
| `contexto_anomalia['entrada_sem_saida']` | API `views.py:672-675` / core `669-672` | delta `780`; `_data_furo_da_grade` `disputa_emissao.py:59`, `68-71` |
| `batida` | retorno da porta `views.py:684-698` / core `681` em diante | consulta nova `_dtb_sse` `777`; guarda `699` / core `696`; helper/JSON `789`, `841-844` |
| `_dia_sse` | `_T1`, ambos `777` | comparação `778`, `.isoformat()` `783`; autoridade em `ponto/turnos.py:2917-2926` |

## 06/11 20:26 · sessao 13599b75567f · - · - · origem=Ronald (tmux)

| Classe | Conclusão estática |
|---|---|
| Segundo juiz | Delta pergunta ao juiz existente; `dia_do_turno_da_batida` apenas recorta `dia_das_batidas` por PK, cujo resultado vem de `t.data_turno`, com dia civil apenas para batida que nenhum turno reclama (`app/api/views.py:776`, `app/api/views_core.py:776`, `app/ponto/turnos.py:2917`, `app/ponto/turnos.py:2935`). |
| Segundo escritor fora da porta | Delta modifica estruturas locais; Batida entra por `registrar_batida`, que devolve existente ou executa o `create` antes do retorno (`app/api/views.py:684`, `app/api/views.py:779`, `app/ponto/registro_batida.py:113`, `app/ponto/registro_batida.py:120`, `app/ponto/registro_batida.py:157`). |
| Relógio fora da borda | Bloco novo não consulta relógio: consulta batida e juiz; o placeholder civil já existia e continua antes dele (`app/api/views.py:655`, `app/api/views.py:775`, `app/api/views_core.py:652`, `app/api/views_core.py:775`). |
| Derivação paralela | Juiz de slot consulta `_ts_tipo_do_plantio`; esse plantio chama o ajustador overnight; validação consulta o mesmo slot (`app/chamados/juizes.py:344`, `app/chamados/services/materializacao.py:418`, `app/chamados/services/validacao.py:822`). |
| Selo sem mordida | Selo P13 possui mutantes, mas vigia escrita de `PerguntaDisputa.data_evento`, não semântica de `contexto['saida_sem_entrada']['data']`; execução dos selos [nao verificado] (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:25`, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:144`). |
| Campo com dois significados | Literalmente, **nem todo escritor de SSE.data escreve dia do turno**: primeiro ambos os kernels escrevem dia civil e depois o sobrescrevem com `_T1` ou removem; emissão fica após essa normalização (`app/api/views.py:655`, `app/api/views_core.py:652`, `app/api/views.py:775`, `app/api/views.py:793`). |

Semântica no PWA já nasce normalizada: predicado SSE é calculado antes da escrita, mas contexto só nasce após `registrar_batida` e `_T1 != None` (`app/ponto/services/triagem_batida.py:649`, `app/ponto/services/triagem_batida.py:701`, `app/ponto/services/triagem_batida.py:726`, `app/ponto/services/triagem_batida.py:738`).

Leitores tratam o valor como data da pergunta/turno: catálogo interpola `ctx[motivo].data`; emissão converte essa data e passa a `abrir`; plantio usa `p.data_evento` ou o mesmo contexto e aplica a cadeia overnight uma vez (`app/chamados/catalogo/query.py:58`, `app/chamados/services/disputa_emissao.py:363`, `app/chamados/services/disputa_emissao.py:407`, `app/chamados/services/materializacao.py:343`, `app/chamados/services/materializacao.py:418`).

`DiaDoTurno` é somente `NewType`, portanto não valida procedência semântica em runtime; a porta `abrir` rejeita `None`, enquanto `abrir_conversa` continua gravando dia civil, com perguntas de hora sem dia barradas pelo emissor (`app/core/dia_do_turno.py:13`, `app/chamados/models.py:2413`, `app/chamados/models.py:2419`, `app/chamados/services/disputa_emissao.py:410`).

## 06/11 20:21 · sessao 11bb1a203949 · L-123-MEDIDA-EM-PY · verificador · origem=Code

**Parecer: reprova. Há caminho que altera comportamento e recebe `n/a (medida)`: o selo considera livre qualquer atribuição de topo sem `lambda`, sem restringir o destino nem a expressão executada.** Isso excede “número medido e texto de prova”. (`bin/tests/test_lei_akita.sh:124`, `bin/tests/test_lei_akita.sh:152`, `bin/tests/test_lei_akita.sh:188`)

**1) Caminhos de aceite e recusa**

Contraexemplo concreto: mudar `CADENCIA_TERMOMETRO_DIAS = 7` para `700` passa pela exceção. Essa constante decide vencimento e cor do placar; com `hoje=2026-11-06` e R1 medido em `2026-10-01`, o veredito muda de AMARELO para VERDE, sem editar função. ([placar_estrutural.py:61](/home/ronald/codex_ws/c74fb85e2a25a875ce4ad571926ba2c7be820832/app/core/placar_estrutural.py:61), `app/core/placar_estrutural.py:667`, `app/core/placar_estrutural.py:685`, `bin/tests/test_lei_akita.sh:152`)

Na tabela, “aceita” significa que **o juiz de `padrao` concede a exceção**, considerando satisfeitas as demais condições; o callback remove esses arquivos da lista de negócio. (`bin/tests/test_lei_akita.sh:186`)

| Caso | Resultado pelo código |
|---|---|
| Dict com chamada: `R = {'n': fechar()}` | **Aceita.** `Assign` sem lambda; chamadas não são vetadas. (`bin/tests/test_lei_akita.sh:152`) |
| Reatribuir nome usado por função | **Aceita.** Não examina dependências entre atribuição e função. (`bin/tests/test_lei_akita.sh:152`) |
| Monkeypatch: `motor.fechar = outra` | **Aceita.** Não restringe os alvos da atribuição. Chamada solta `fechar()` seria recusada. (`bin/tests/test_lei_akita.sh:151`, `bin/tests/test_lei_akita.sh:152`) |
| Anotação executável: `x: fechar() = 1` | **Aceita.** `AnnAssign` também é livre sem lambda. (`bin/tests/test_lei_akita.sh:152`) |
| Merge | **Depende das linhas restantes.** Compara com o primeiro pai e elimina caminhos iguais a outro pai já publicado. Nas diferenças restantes, atribuições perigosas passam; função/import/classe alterados recusam. (`bin/tests/test_lei_akita.sh:164`, `bin/tests/test_lei_akita.sh:236`, `bin/tests/test_lei_akita.sh:247`) |
| Rename | **Recusa normalmente:** `--no-renames` conserva origem e destino; origem ausente no ALVO não declara medida. Pode aceitar uma exclusão histórica de dados se a origem tiver sido restaurada e declarada no ALVO posterior. (`bin/tests/test_lei_akita.sh:139`, `bin/tests/test_lei_akita.sh:164`, `bin/tests/test_lei_akita.sh:238`) |
| Arquivo novo que se declara medida | **Aceita se todas as linhas forem livres**, inclusive atribuições com chamadas. Novo com função/import/classe recusa. Commit raiz sem primeiro pai recusa. (`bin/tests/test_lei_akita.sh:160`, `bin/tests/test_lei_akita.sh:169`, `bin/tests/test_lei_akita.sh:174`) |
| Declaração reatribuída depois | **Aceita.** `PAPEL_DO_MODULO='medida'` seguido de `PAPEL_DO_MODULO='negocio'` satisfaz o `any`; não verifica o último valor. (`bin/tests/test_lei_akita.sh:141`) |
| Decorador | **Recusa.** O intervalo preso começa no primeiro decorador. (`bin/tests/test_lei_akita.sh:149`, `bin/tests/test_lei_akita.sh:153`) |
| Linha de continuação | **Segue o nó AST inteiro:** atribuição com chamada aceita; continuação de import/def/class recusa. (`bin/tests/test_lei_akita.sh:150`, `bin/tests/test_lei_akita.sh:152`) |
| Hunk `-U0` com contagem zero | **Ignora corretamente esse lado**, conferindo o outro. A âncora não é tratada como linha modificada; isso permite criação e exclusão quando o lado existente é livre. (`bin/tests/test_lei_akita.sh:171`, `bin/tests/test_lei_akita.sh:174`) |
| Docstring que não é a primeira | **Recusa** a string solta após outro nó: `eh_doc` exige `i == 0`. Comentários anteriores à docstring inicial não são nós AST. (`bin/tests/test_lei_akita.sh:148`, `bin/tests/test_lei_akita.sh:151`) |
| Arquivo deletado | **Recusa se continua ausente no ALVO.** Se restaurado no ALVO, uma exclusão histórica composta somente por linhas livres pode passar; excluir função continua recusado pelo lado antigo. (`bin/tests/test_lei_akita.sh:131`, `bin/tests/test_lei_akita.sh:139`, `bin/tests/test_lei_akita.sh:174`) |
| Comentário/branco dentro de função/classe | **Recusa.** `preso` prevalece sobre `soltas`. (`bin/tests/test_lei_akita.sh:154`) |

Há outra abertura: **diff bem-sucedido sem nenhum cabeçalho `@@` chega a `True`**, pois não existe exigência de encontrar hunks. A produção desse cenário pelo Git real nesta cópia é **[não verificado]**. (`bin/tests/test_lei_akita.sh:171`, `bin/tests/test_lei_akita.sh:176`)

Também passam alterações de `estado`, `papel`, constantes e estrutura de `RESULTADOS`/`ETAPAS`, embora determinem contagens, classificação e percentual publicados. (`bin/tests/test_lei_akita.sh:152`, `app/core/placar_estrutural.py:643`, `app/core/placar_estrutural.py:699`, `app/core/espelho_verdade.py:205`, `app/core/espelho_verdade.py:213`)

**2) Declaração no ALVO**

**Abre classificação retroativa.** Um commit anterior que mudou uma atribuição de negócio pode passar quando um commit posterior acrescenta a declaração no ALVO. Retirá-la no ALVO também pode reprovar medidas anteriores. Os intervalos continuam sendo analisados em pai/`h`; a autorização vem de outra revisão. (`bin/tests/test_lei_akita.sh:140`, `bin/tests/test_lei_akita.sh:158`, `bin/tests/test_lei_akita.sh:168`, `bin/tests/test_lei_akita.sh:255`)

O MORDE exige explicitamente esse comportamento ao julgar `so_numero` depois de retirar a declaração no ALVO. Falta o sentido inverso: ALVO concedendo declaração a um `h` que não a possuía. (`bin/tests/test_lei_akita.sh:349`)

**3) O MORDE morde quais cláusulas?**

Ele chama as funções reais; o shell reprova qualquer `MORDE_FALHOU`, mesmo havendo `MORDE_OK` ao final. (`bin/tests/test_lei_akita.sh:347`, `bin/tests/test_lei_akita.sh:473`, `bin/tests/test_lei_akita.sh:488`)

| Caso | Mutante que distingue por inspeção |
|---|---|
| `so_numero`, `comentario`, `doc` | Matam **aceite desligado**. (`bin/tests/test_lei_akita.sh:330`, `bin/tests/test_lei_akita.sh:341`) |
| `na_def` | Mata **todas as linhas livres** ou liberação de corpo de função. (`bin/tests/test_lei_akita.sh:333`, `bin/tests/test_lei_akita.sh:342`) |
| `com_import` | Mata liberação de import. (`bin/tests/test_lei_akita.sh:334`, `bin/tests/test_lei_akita.sh:342`) |
| `com_lambda` | Mata retirada do veto a lambda. (`bin/tests/test_lei_akita.sh:335`, `bin/tests/test_lei_akita.sh:342`) |
| `sem_decl` | Mata dispensa da declaração. (`bin/tests/test_lei_akita.sh:336`, `bin/tests/test_lei_akita.sh:343`) |
| `mais_fech` | Mata ignorar o segundo arquivo de negócio. (`bin/tests/test_lei_akita.sh:337`, `bin/tests/test_lei_akita.sh:343`) |
| Número com `n/a (instrumento)` | Mata extensão da exceção ao outro rótulo. (`bin/tests/test_lei_akita.sh:344`) |
| ALVO sem declaração | Mata leitura da declaração exclusivamente em `h`. (`bin/tests/test_lei_akita.sh:349`) |

**Não cobre cada cláusula.** Faltam casos de chamada em atribuição, monkeypatch, reatribuição funcional, declaração anulada depois, classe/decorador, docstring posterior, arquivo novo/deletado, retroatividade e ausência de hunks. Os casos específicos de medida também não cobrem merge/rename nem erro Git. (`bin/tests/test_lei_akita.sh:330`, `bin/tests/test_lei_akita.sh:340`, `bin/tests/test_lei_akita.sh:349`)

Falta um negativo que dependa **somente do lado antigo**: `na_def` é preso dos dois lados; import e lambda já recusam pelo novo. A inserção de comentário exercita contagem zero, mas não demonstra todas as suas bordas. (`bin/tests/test_lei_akita.sh:331`, `bin/tests/test_lei_akita.sh:333`)

A integração com o universo tampouco é exercitada nesses casos: os commits têm mensagem `"c"`, e `julgar` recebe mensagem e arquivos montados manualmente. (`bin/tests/test_lei_akita.sh:325`, `bin/tests/test_lei_akita.sh:340`, `bin/tests/test_lei_akita.sh:347`)

A promessa “erro Git = `PADRAO_ERRO`” não vale literalmente para todas as novas operações: `show` com erro vira AST ausente; erro de `rev-parse` vira `False`; o novo `diff` emite `PADRAO_ERRO`. (`bin/tests/test_lei_akita.sh:131`, `bin/tests/test_lei_akita.sh:160`, `bin/tests/test_lei_akita.sh:165`)

Execução integral, três commits verdes e execução dos dois mutantes vermelhos: **[não verificado]**. [nao verificado]

**4) Leitores, escritores e quebra pelo nome novo**

| Símbolo | Quem escreve/define | Quem lê/chama |
|---|---|---|
| `PAPEL_DO_MODULO` | Declarações reais nos dois módulos; fixture e retirada no MORDE. (`app/core/placar_estrutural.py:38`, `app/core/espelho_verdade.py:30`, `bin/tests/test_lei_akita.sh:314`, `bin/tests/test_lei_akita.sh:338`) | `declara_medida`, por AST no ALVO. Aceita a presença sintática, sem avaliar o valor final executado. (`bin/tests/test_lei_akita.sh:139`) |
| `medida_do_commit` | Definida no selo. (`bin/tests/test_lei_akita.sh:156`) | Universo real e MORDE; resultado consumido pelo callback de `_julgar_linha`. (`bin/tests/test_lei_akita.sh:255`, `bin/tests/test_lei_akita.sh:347`, `bin/tests/test_lei_akita.sh:351`, `bin/tests/test_lei_akita.sh:188`) |

**O nome adicional não quebra os leitores identificados:** `gerar_estado` executa os módulos e acessa atributos explícitos; os testes citados acessam `RESULTADOS`, `ETAPAS`, funções e constantes específicas. (`bin/gerar_estado.py:75`, `bin/gerar_estado.py:80`, `bin/gerar_estado.py:142`, `bin/gerar_estado.py:145`, `app/core/tests/test_r1_termometro_uma_medicao.py:40`, `app/core/tests/test_r1_termometro_uma_medicao.py:44`, `app/core/tests/test_l114_placar_papel_e_termometro.py:30`)

Contrato que enumere exatamente todos os nomes globais desses módulos e quebra na suíte completa: **[não verificado]**. [nao verificado]

Régua e pre-push consomem o resultado do selo: a régua percorre `test_*.sh`; o pre-push passa o SHA empurrado. (`bin/regua.sh:138`, `bin/pre-push.sh:38`)

**5) Classes de reprovação**

- **Segundo juiz / derivação paralela:** a alteração mantém `julgar` e acrescenta um predicado de elegibilidade no mesmo fluxo. Não demonstra duplicação de autoridade registrada de cálculo, dinheiro ou batida. (`bin/tests/test_lei_akita.sh:177`, `bin/tests/test_lei_akita.sh:255`, `app/core/juizes.py:7`)
- **Selo sem mordida:** há mordida efetiva para os casos plantados, mas ela deixa sobreviver os contraexemplos de atribuição executável e de controle do placar. (`bin/tests/test_lei_akita.sh:340`, `bin/tests/test_lei_akita.sh:152`)
- **ESMERIL-SEMÂNTICA: reprova.** Nos módulos, “medida” promete número medido e prova; no selo, significa qualquer `Assign`/`AnnAssign` sem lambda. O mesmo rótulo autoriza mudanças de decisão e efeitos executáveis. (`app/core/placar_estrutural.py:33`, `app/core/espelho_verdade.py:25`, `bin/tests/test_lei_akita.sh:152`)

## 06/11 20:21 · sessao 15727da88a0e · V2-BUGS-DO-MOTOR · sonda · origem=Code

Não executei Django nem as sondas; resultados de produção: [não verificado]. [nao verificado]

O universo exige `batidas_apuraveis`, remoção de duplicatas, pareamento, eventual substituição pela ata e filtro pelo dia da jornada. Os scripts percorrem o fechamento real em leitura. (app/ponto/services/fechamento.py:623; app/ponto/motor_calculo_v2.py:530, 531, 2351, 2429, 2458)

**Script 1.** Separa adicional, HE e chamadas auxiliares dos intervalos. As somas são dos retornos reais das chamadas, antes do desconto de intervalos e da conversão para hora reduzida. O replay do calculador usa os argumentos do fechamento e aparece separado; seu ramo começa em 10/2026. (app/ponto/motor_calculo_v2.py:983–987; app/ponto/services/fechamento.py:1249–1260)

```python
import logging
import re
import sys
from collections import defaultdict
from datetime import datetime, time, timedelta

from django.db import connection, transaction
from django.utils import timezone

from colaboradores.models import Colaborador, Empresa
from ponto import motor_calculo_v2 as motor
from ponto.calculador import regras
from ponto.calculador.alimentacao import (
    insumos_do_calculador, rubricas_por_dia,
)
from ponto.janelas import janela_fechamento
from ponto.services.fechamento import (
    recalcular_fechamento_mes, universo_da_competencia,
)

FONTE = (
    "app/ponto/motor_calculo_v2.py:82,110,112,191,441,983;"
    "app/ponto/services/fechamento.py:373,623,745,1253"
)

def emitir(*args):
    print(*args, "|", FONTE)

# Redação apenas na saída de erros; nenhum nome/CPF é impresso.
privados = [
    str(valor)
    for linha in Colaborador.objects.values_list("nome", "cpf")
    for valor in linha if valor
]
privados.sort(key=len, reverse=True)

def repr_seguro(exc):
    texto = repr(exc)
    for valor in privados:
        texto = texto.replace(valor, "[removido]")
    return re.sub(r"\d{3}\.?\d{3}\.?\d{3}-?\d{2}", "[removido]", texto)

def bloquear_escrita(execute, sql, params, many, context):
    comando = sql.lstrip().upper()
    permitidos = (
        "SELECT", "SHOW", "SET ", "SAVEPOINT ",
        "RELEASE SAVEPOINT ", "ROLLBACK TO SAVEPOINT ",
    )
    if not comando.startswith(permitidos):
        raise RuntimeError("SQL de escrita bloqueado pela sonda")
    return execute(sql, params, many, context)

class Reverter(BaseException):
    pass

COD_FECHAMENTO = recalcular_fechamento_mes.__code__
COD_MINUTOS = motor.minutos_noturnos.__code__
COD_INTERVALOS = motor.minutos_noturnos_dos_intervalos.__code__
COD_PRINCIPAL = motor.MotorBase.calcular_noturnas.__code__
COD_HE = motor.he_noturna.__code__
COD_CALCULADOR = regras.noturnas_do_dia.__code__
COD_MES = motor.MotorBase.calcular_mes.__code__

def excesso_22_24(inicio, fim, prorrogacao):
    # Mesmas condições e mesmas bordas civis de minutos_noturnos:
    # app/ponto/motor_calculo_v2.py:89-117.
    if inicio >= fim or fim - inicio > timedelta(hours=26):
        return 0.0
    total = 0.0
    dia = inicio.date()
    while dia <= fim.date():
        zero = motor.TZ_LOCAL.localize(datetime.combine(dia, time()))
        cinco = motor.TZ_LOCAL.localize(datetime.combine(dia, time(5)))
        vinte_duas = motor.TZ_LOCAL.localize(
            datetime.combine(dia, time(22))
        )
        proximo_zero = motor.TZ_LOCAL.localize(
            datetime.combine(dia + timedelta(days=1), time())
        )
        if prorrogacao and max(inicio, zero) < cinco and fim > cinco:
            a = max(inicio, vinte_duas)
            b = min(fim, proximo_zero)
            if a < b:
                total += (b - a).total_seconds() / 60.0
        dia += timedelta(days=1)
    return total

def periodos_finais(locais):
    resultado = locais["resultado"]
    objetos = list(resultado.periodos) + list(resultado.periodos_ft)
    finais = {id(p): ("motor", p) for p in objetos}

    # O escritor só incorpora os dias-alvo do motor órfão:
    # app/ponto/services/fechamento.py:829-833.
    if "_r_orfa" in locais:
        r = locais["_r_orfa"]
        alvo = locais["_alvo"]
        for p in list(r.periodos) + list(r.periodos_ft):
            if timezone.localtime(p.entrada).date() in alvo:
                finais[id(p)] = ("motor_orfao", p)
    return finais

def sondar(colab, mes, empresa_id):
    ctx = {
        "locais": None, "chamadas": [], "erros": [],
        "fase": "motor", "resultado": None,
    }

    def periodo_do_motor(frame):
        periodo = None
        parcela_r3 = False
        f = frame
        while f is not None:
            candidato = f.f_locals.get("periodo")
            if periodo is None and isinstance(
                candidato, motor.PeriodoCalculo
            ):
                periodo = candidato
            # A parcela R3 é calculada em objeto temporário e creditada
            # no primeiro aberto ordenado: motor_calculo_v2.py:2553-2560.
            if f.f_code is COD_MES and 2517 <= f.f_lineno <= 2564:
                abertos = f.f_locals.get("_abs")
                if abertos:
                    periodo = min(abertos, key=lambda p: p.entrada)
                    parcela_r3 = True
            f = f.f_back
        if periodo is None:
            raise RuntimeError("Chamada noturna sem período identificável")
        return periodo, parcela_r3

    def periodo_do_calculador(dono):
        dados = dono.f_locals
        if dono.f_code is COD_HE:
            entrada, saida = dados["entrada"], dados["saida"]
        else:
            entrada, saida = dados["_e"], dados["_s"]

        dia = None
        f = dono
        while f is not None:
            if f.f_code is rubricas_por_dia.__code__:
                dia = f.f_locals.get("k")
                break
            f = f.f_back

        resultado = ctx["resultado"]
        mapa = getattr(resultado, "dia_da_jornada", {}) or {}
        candidatos = {}
        for p in list(resultado.periodos) + list(resultado.periodos_ft):
            e = getattr(p, "efetivo_entrada", None)
            s = getattr(p, "efetivo_saida", None)
            if (
                e is not None and s is not None
                and e <= entrada <= saida <= s
                and (dia is None or mapa.get(id(p)) == dia)
            ):
                candidatos[id(p)] = p
        if len(candidatos) != 1:
            raise RuntimeError(
                "Recorte do calculador sem associação unívoca ao turno: "
                "%d candidatos" % len(candidatos)
            )
        return next(iter(candidatos.values()))

    def perfil(frame, evento, retorno):
        if evento != "return":
            return
        try:
            if frame.f_code is COD_FECHAMENTO:
                ctx["locais"] = dict(frame.f_locals)
                return
            if frame.f_code is not COD_MINUTOS:
                return

            auxiliar = frame.f_back.f_code is COD_INTERVALOS
            dono = frame.f_back
            while dono is not None and dono.f_code not in (
                COD_PRINCIPAL, COD_HE, COD_CALCULADOR
            ):
                dono = dono.f_back
            if dono is None:
                raise RuntimeError("Chamador noturno não catalogado")

            if ctx["fase"] == "motor":
                p, r3 = periodo_do_motor(frame.f_back)
                canal = (
                    "motor/adicional"
                    if dono.f_code is COD_PRINCIPAL else "motor/HE"
                )
                if r3:
                    canal += "/parcela_R3"
            else:
                p = periodo_do_calculador(dono)
                canal = (
                    "replay_calculador/HE"
                    if dono.f_code is COD_HE
                    else "replay_calculador/adicional"
                )
            if auxiliar:
                canal += "/auxiliar_intervalos"

            dados = frame.f_locals
            ctx["chamadas"].append((
                canal, p, dados["inicio"], dados["fim"],
                dados["prorrogacao"], float(retorno),
            ))
        except Exception as exc:
            ctx["erros"].append(exc)

    def rastrear(frame, evento, argumento):
        if frame.f_code not in (
            COD_FECHAMENTO, motor._periodos_pelo_marco.__code__
        ):
            return None
        frame.f_trace_lines = False
        if evento == "exception" and isinstance(argumento[1], Exception):
            if not isinstance(argumento[1], StopIteration):
                ctx["erros"].append(argumento[1])
        return rastrear

    anterior_perfil = sys.getprofile()
    anterior_trace = sys.gettrace()
    try:
        try:
            with transaction.atomic():
                sys.setprofile(perfil)
                sys.settrace(rastrear)
                retorno = recalcular_fechamento_mes(
                    mes, 2026, empresa_id=empresa_id,
                    colaborador_ids=[colab.pk],
                    somente_leitura=True,
                    lavrar_dias_pagos=False,
                    retornar_objeto=True,
                )
                if colab.pk not in retorno or ctx["locais"] is None:
                    raise RuntimeError("Fechamento não devolveu o colaborador")

                locais = ctx["locais"]
                ctx["resultado"] = locais["resultado"]

                # Replay apenas do ramo que o fechamento possui:
                # app/ponto/services/fechamento.py:1249-1260.
                if mes >= 10 and not ctx["erros"]:
                    ctx["fase"] = "calculador"
                    insumos = insumos_do_calculador(
                        colab, locais["resultado"],
                        locais["_data_ini"], locais["_data_fim"],
                        celulas=locais["_cels_lote"],
                    )
                    rubricas_por_dia(
                        colab, locais["resultado"],
                        locais["_data_ini"], locais["_data_fim"],
                        insumos=insumos,
                        celulas=locais["_cels_lote"],
                        certa_cache=locais["_certa_cache"],
                        tipo_escala=locais["tipo_escala"],
                        escala_colaborador=locais["esc"],
                        feriados=locais["feriados"],
                        batidas_locais=[
                            timezone.localtime(b.timestamp)
                            for b in locais["batidas"]
                        ],
                        dias_com_ausencia_inteira=locais["_dias_aus_inteira"],
                    )
                raise Reverter()
        except Reverter:
            pass
    finally:
        sys.setprofile(anterior_perfil)
        sys.settrace(anterior_trace)

    if ctx["erros"]:
        raise ctx["erros"][0]
    return periodos_finais(ctx["locais"]), ctx["chamadas"]

def novo_resumo():
    return {"turnos": set(), "colabs": set(), "chamadas": 0,
            "minutos": 0.0, "excesso": 0.0}

desabilitado = logging.root.manager.disable
logging.disable(10**9)
try:
    with connection.execute_wrapper(bloquear_escrita):
        for mes in (9, 10, 11):
            erros = []
            universo = 0
            origens = defaultdict(int)
            canais = defaultdict(novo_resumo)
            casos = defaultdict(novo_resumo)
            tentados = 0

            for empresa_id in (2, 3, 4):
                try:
                    empresa = Empresa.objects.get(pk=empresa_id)
                    ini, fim = janela_fechamento(mes, 2026, empresa)
                    emitir(
                        "JANELA", "%02d/2026" % mes,
                        "empresa", empresa_id, ini.isoformat(), fim.isoformat(),
                        "corte_cadastrado", empresa.dia_inicio_competencia,
                    )
                    colabs = list(
                        universo_da_competencia(mes, 2026, empresa_id)
                        .order_by("pk")
                    )
                except Exception as exc:
                    erros.append(repr_seguro(exc))
                    continue

                for colab in colabs:
                    tentados += 1
                    try:
                        finais, chamadas = sondar(colab, mes, empresa_id)
                        # Só incorpora um colaborador depois de validar tudo.
                        preparados = []
                        for canal, p, a, b, prorroga, minutos in chamadas:
                            if id(p) not in finais:
                                continue  # período que não entrou na apuração
                            origem = finais[id(p)][0]
                            canal = origem + "/" + canal
                            longo = b - a > timedelta(hours=26)
                            excesso = excesso_22_24(a, b, prorroga)
                            preparados.append((
                                canal, (colab.pk, id(p)),
                                minutos, longo, excesso,
                            ))

                        universo += len(finais)
                        for origem, p in finais.values():
                            origens[origem] += 1

                        for canal, chave, minutos, longo, excesso in preparados:
                            r = canais[canal]
                            r["turnos"].add(chave)
                            r["colabs"].add(colab.pk)
                            r["chamadas"] += 1
                            for caso, ocorre in (("a", longo), ("b", excesso > 0)):
                                if not ocorre:
                                    continue
                                r = casos[(canal, caso)]
                                r["turnos"].add(chave)
                                r["colabs"].add(colab.pk)
                                r["chamadas"] += 1
                                r["minutos"] += minutos
                                r["excesso"] += excesso
                    except Exception as exc:
                        erros.append(("col%d" % colab.pk, repr_seguro(exc)))

            validade = "INVALIDO" if erros else "VALIDO"
            emitir(
                "COMPETENCIA", "%02d/2026" % mes, validade,
                "colabs_tentados", tentados,
                "UNIVERSO_TURNOS", universo,
                "por_origem", dict(origens),
            )
            # Os mesmos turnos podem aparecer em mais de um canal.
            # Auxiliares e parcelas R3 não criam turnos no universo.
            for canal in sorted(canais):
                r = canais[canal]
                emitir(
                    "CANAL", canal,
                    "turnos_avaliados", len(r["turnos"]),
                    "chamadas_avaliadas", r["chamadas"],
                )
                for caso in ("a", "b"):
                    r = casos[(canal, caso)]
                    emitir(
                        "CASO", caso, canal,
                        "n_turnos", len(r["turnos"]),
                        "n_colabs", len(r["colabs"]),
                        "colabs", ["col%d" % pk for pk in sorted(r["colabs"])],
                        "n_chamadas", r["chamadas"],
                        "soma_retornos_min", r["minutos"],
                        "soma_excesso_22_24_min", r["excesso"],
                    )
            if not canais:
                emitir("SEM_CANAIS_OBSERVADOS", validade)
            if mes == 9:
                emitir(
                    "REPLAY_CALCULADOR",
                    "NAO_APLICAVEL_AO_RAMO_DO_FECHAMENTO_09",
                )
            emitir("ERROS", len(erros), "primeiro_repr", erros[0] if erros else None)
finally:
    logging.disable(desabilitado)
```

Funções reais chamadas/observadas pelo Script 1:

- `universo_da_competencia`: app/ponto/services/fechamento.py:373; `recalcular_fechamento_mes`: :439; `apurar_por_fatia`: :132.
- `janela_fechamento`: app/ponto/janelas.py:54; `corte_da_empresa`: :36; `periodo_apuracao`: :10.
- `batidas_apuraveis`: app/ponto/turnos.py:2844; `parear_turnos`: :1190.
- `turnos_via_autoridade`: app/ponto/motor_calculo_v2.py:453; `_periodos_via_autoridade`: :509; `_periodos_pelo_marco`: :534; `MotorBase.calcular_mes`: :2265.
- `minutos_noturnos`: app/ponto/motor_calculo_v2.py:82; `minutos_noturnos_dos_intervalos`: :121; `calcular_noturnas`: :969; `he_noturna`: :409; `_inicio_do_trecho_extra`: :378.
- `insumos_do_calculador`: app/ponto/calculador/alimentacao.py:305; `rubricas_por_dia`: :353; `noturnas_do_dia`: app/ponto/calculador/regras.py:66.

Os chamadores diretos são `calcular_noturnas`, `he_noturna`, `minutos_noturnos_dos_intervalos` e `noturnas_do_dia`. O primeiro recebe pontas ajustadas; HE recebe a cauda calculada; os intervalos usam prefixos; o calculador pode fornecer segmentos separados pelas pausas. `MotorPlantao` delega ao cálculo do 12x36. (app/ponto/motor_calculo_v2.py:983, 440–443, 191–196, 2759, 3510–3511; app/ponto/calculador/regras.py:85; app/ponto/calculador/alimentacao.py:128–158)

**Script 2.** Usa a distribuição real de `lavrar`, interrompendo-a antes da primeira escrita. Também envolve cada apuração em `transaction.atomic()` com exceção final para rollback e bloqueia SQL de escrita. Imprime o censo pelo juiz de turno e, separadamente, pelos períodos abertos finais do motor. Essas duas representações podem diferir. (app/ponto/services/dia_pago.py:135, 333–358; app/ponto/motor_calculo_v2.py:867–882)

```python
import inspect
import logging
import re
import sys
from collections import defaultdict
from datetime import date

from django.db import connection, transaction
from django.utils import timezone

from colaboradores.models import Colaborador, Empresa
from ponto import motor_calculo_v2 as motor
from ponto.janelas import janela_fechamento
from ponto.services.dia_pago import lavrar
from ponto.services.fechamento import (
    recalcular_fechamento_mes, universo_da_competencia,
)

FONTE = (
    "app/ponto/services/fechamento.py:439,623,1185;"
    "app/ponto/motor_calculo_v2.py:867,2495,2558;"
    "app/ponto/services/dia_pago.py:135,343,357"
)

def emitir(*args):
    print(*args, "|", FONTE)

privados = [
    str(valor)
    for linha in Colaborador.objects.values_list("nome", "cpf")
    for valor in linha if valor
]
privados.sort(key=len, reverse=True)

def repr_seguro(exc):
    texto = repr(exc)
    for valor in privados:
        texto = texto.replace(valor, "[removido]")
    return re.sub(r"\d{3}\.?\d{3}\.?\d{3}-?\d{2}", "[removido]", texto)

def bloquear_escrita(execute, sql, params, many, context):
    comando = sql.lstrip().upper()
    if not comando.startswith((
        "SELECT", "SHOW", "SET ", "SAVEPOINT ",
        "RELEASE SAVEPOINT ", "ROLLBACK TO SAVEPOINT ",
    )):
        raise RuntimeError("SQL de escrita bloqueado pela sonda")
    return execute(sql, params, many, context)

class Capturado(BaseException):
    pass

class Reverter(BaseException):
    pass

COD_FECHAMENTO = recalcular_fechamento_mes.__code__
COD_JUIZ = motor.MotorBase._turnos_do_juiz.__code__
COD_LAVRAR = lavrar.__code__

# Descobre a barreira no fonte carregado; falha se não for unívoca.
fonte_lavrar, primeira_linha = inspect.getsourcelines(lavrar)
barreiras = [
    primeira_linha + indice
    for indice, linha in enumerate(fonte_lavrar)
    if "DiaPago.objects.filter(" in linha and ".delete()" in linha
]
if len(barreiras) != 1:
    raise RuntimeError("ERROS 1: barreira de escrita de lavrar não identificada")
LINHA_ANTES_DA_ESCRITA = barreiras[0]

def sondar(colab, mes, empresa_id):
    ctx = {
        "locais": None, "turnos": {}, "linhas": None,
        "erros": [],
    }

    def perfil(frame, evento, retorno):
        if evento != "return":
            return
        try:
            if frame.f_code is COD_FECHAMENTO:
                ctx["locais"] = dict(frame.f_locals)
            elif frame.f_code is COD_JUIZ and retorno is not None:
                ini = frame.f_locals["data_ini"]
                fim = frame.f_locals["data_fim"]
                for turno in retorno:
                    if turno.data_turno is None or not (
                        i

[truncado em 20000 de 32436 caracteres]

## 06/11 20:20 · sessao 3487bdb015de · - · - · origem=Ronald (tmux)

Considere abaixo “aceita” como `julgar` devolvendo `None` para `padrao=n/a (medida)`, com o arquivo no conjunto de negócio e sem outro bloqueador no commit. A exceção remove arquivos cujo callback `medida(a)` retorna verdadeiro. (`bin/tests/test_lei_akita.sh:186-191`)

| Caso | Resultado e fundamento |
|---|---|
| Dict com chamada, `R={'n': fechar()}` | **Aceita**: qualquer `Assign`/`AnnAssign` de topo sem `Lambda` é livre; não examina chamadas nem efeitos. (`bin/tests/test_lei_akita.sh:152-155`) |
| Reatribuir global usado por função | **Aceita**, inclusive `FATOR=2`, sem alterar a função: não analisa dependências dos leitores. (`bin/tests/test_lei_akita.sh:152-155`) |
| Monkeypatch de topo, `motor.fechar=outro` | **Aceita**: alvos de atribuição não são restringidos a nomes de dados. (`bin/tests/test_lei_akita.sh:152-155`) |
| Anotação executável, `x: fechar()=1` | **Aceita**: `AnnAssign` inteiro é livre, exceto se houver `Lambda`. (`bin/tests/test_lei_akita.sh:152-155`) |
| Chamada solta `fechar()` | **Recusa**: `Expr(Call)` entra em `preso`. (`bin/tests/test_lei_akita.sh:151-155,174-175`) |
| Função/classe/import mexido | **Recusa** se houver linha desse nó efetivamente alterada: seu intervalo fica preso. (`bin/tests/test_lei_akita.sh:148-155,174-175`) |
| Declaração seguida de `PAPEL_DO_MODULO='negocio'` | **Aceita**: basta existir *algum* `Assign` literal de topo igual a `'medida'`; não verifica a última atribuição nem o valor efetivo. (`bin/tests/test_lei_akita.sh:139-144`) |
| Decorador | **Recusa**: intervalo preso da função começa no menor `lineno` entre função e decoradores. (`bin/tests/test_lei_akita.sh:149-155`) |
| Continuação de atribuição com chamada | **Aceita**: todo o intervalo até `end_lineno` fica livre. (`bin/tests/test_lei_akita.sh:150-155`) |
| Continuação de import/def/class | **Recusa**: todo o intervalo fica preso. (`bin/tests/test_lei_akita.sh:148-155`) |
| Docstring inicial | **Aceita**; comentários anteriores não contam como elementos de `t.body`. (`bin/tests/test_lei_akita.sh:148-153`) |
| String que não é o primeiro nó do módulo | **Recusa**: `eh_doc` exige `i == 0`. (`bin/tests/test_lei_akita.sh:151-155`) |
| Comentário/branco dentro de def/class | **Recusa**: `soltas` perde para `preso`. (`bin/tests/test_lei_akita.sh:154-155`) |
| Hunk `-U0` com contagem zero | **Ignora corretamente esse lado**: `if n` impede cobrar a linha âncora; o outro lado continua sendo conferido. Não é, sozinho, um bypass. (`bin/tests/test_lei_akita.sh:171-175`) |
| Arquivo novo que se declara medida | **Aceita se todo o arquivo novo for livre**, inclusive atribuições com chamadas/monkeypatch; lado pai ausente é ignorado por `-0,0`. Novo arquivo com def/import/classe é recusado. Commit raiz sem primeiro pai é recusado. (`bin/tests/test_lei_akita.sh:158-175`) |
| Arquivo deletado e ainda ausente no ALVO | **Recusa** antes do diff: não existe declaração no ALVO. (`bin/tests/test_lei_akita.sh:131-144,158-159`) |
| Deleção histórica, arquivo restaurado no ALVO | **Pode aceitar**: declaração restaurada satisfaz a guarda; exclusão de arquivo composto somente por linhas livres passa, pois `+0,0` ignora AST ausente em `h`. Excluir função/import/classe continua recusado pelo lado pai. (`bin/tests/test_lei_akita.sh:139-175`) |
| Rename | `--no-renames` conserva origem e destino na lista; origem ausente no ALVO normalmente faz **recusar**. Se origem for restaurada no ALVO e ambos os lados forem compostos de linhas livres, pode **aceitar**. (`bin/tests/test_lei_akita.sh:139-175,238-242,255`) |
| Merge | Usa diff contra primeiro pai. Depois remove caminhos iguais a qualquer outro pai já publicado; diferenças inéditas restantes passam pelo mesmo callback vulnerável a atribuições executáveis. Não há aceitação geral de merges, mas tampouco proteção adicional dos RHS. (`bin/tests/test_lei_akita.sh:236-255`) |
| Diff bem-sucedido sem nenhum `@@` | **Aceita** depois da declaração e da existência do pai, pois o laço não roda e chega a `return True`. Não exige ao menos um hunk nem rejeita saída “Binary files differ”; alteração apenas de modo também não tem linhas para cobrar. (`bin/tests/test_lei_akita.sh:158-176`) |

A leitura no ALVO abre **classificação retroativa**: adicionar hoje a declaração a um arquivo de negócio faz commits anteriores de atribuições desse arquivo passarem como medida; tirar hoje a declaração faz commits anteriores de medida serem recusados. A remoção em `h` também pode passar se o ALVO posterior restaurou a declaração. Não migra a análise das linhas: os dois ASTs ainda são pai e `h`, mas a autorização vem do ALVO. (`bin/tests/test_lei_akita.sh:139-144,156-175,255`)

Confirmei em memória as classificações AST e, com `subprocess.run` simulado, os casos novo só atribuições=True; novo com def=False; deleção ausente no ALVO=False; deleção restaurada=True; diff sem hunk=True. Isso valida a implementação da função, **não** a produção real dessas saídas pelo Git nesta cópia sem `.git`. (`bin/tests/test_lei_akita.sh:139-176`; execução Git real desses cenários: [não verificado])

## 06/11 20:20 · sessao 42b265fa899f · - · - · origem=Ronald (tmux)

Achados para consolidação:

- **“Medida” está semanticamente alargada:** os módulos dizem “NUMERO MEDIDO e texto de PROVA” (`app/core/placar_estrutural.py:33`, `app/core/espelho_verdade.py:25`), mas o selo libera qualquer `Assign`/`AnnAssign` sem lambda (`bin/tests/test_lei_akita.sh:152`). Isso inclui `CADENCIA_TERMOMETRO_DIAS = 7` (`app/core/placar_estrutural.py:61`), usada para decidir vencimento (`app/core/placar_estrutural.py:667`) e cor/veredito (`app/core/placar_estrutural.py:685`). Trocar a cadência recebe a exceção apesar de alterar decisão do leitor, sem tocar `def`. (`bin/tests/test_lei_akita.sh:188`)

- **Outros dados liberados também decidem:** `RESULTADOS` alimenta estado efetivo e papel (`app/core/placar_estrutural.py:643`), e a linha pública conta estados usando constantes globais (`app/core/placar_estrutural.py:699`). No espelho, `ETAPAS` e `FEITO` determinam feitos e percentual (`app/core/espelho_verdade.py:205`, `app/core/espelho_verdade.py:210`, `app/core/espelho_verdade.py:213`). O selo não distingue esses controles de número/prova. (`bin/tests/test_lei_akita.sh:152`)

- **Escritores de `PAPEL_DO_MODULO`:** declarações reais em `app/core/placar_estrutural.py:38` e `app/core/espelho_verdade.py:30`; fixture do MORDE escreve o literal em `bin/tests/test_lei_akita.sh:314` e produz sua retirada em `bin/tests/test_lei_akita.sh:338`. **Leitor semântico:** `declara_medida`, por AST no `ALVO`, em `bin/tests/test_lei_akita.sh:139`. Não lê o valor executado: basta existir algum `Assign` correspondente no corpo. (`bin/tests/test_lei_akita.sh:141`)

- **Escritor de `medida_do_commit`:** definição em `bin/tests/test_lei_akita.sh:156`; **leitores/chamadores:** universo real em `bin/tests/test_lei_akita.sh:255`; casos MORDE em `bin/tests/test_lei_akita.sh:347`; retirada da declaração no ALVO em `bin/tests/test_lei_akita.sh:351`. A exceção é aplicada por `_julgar_linha` ao callback `medida`, em `bin/tests/test_lei_akita.sh:188`.

- **O nome novo, isoladamente, não interfere nos leitores identificados:** `gerar_estado` executa os módulos e escolhe atributos explícitos, sem enumerar nomes globais (`bin/gerar_estado.py:75`, `bin/gerar_estado.py:80`, `bin/gerar_estado.py:142`, `bin/gerar_estado.py:145`). Core testes escolhem `RESULTADOS`, `ETAPAS`, funções e constantes explícitas (`app/core/tests/test_r1_termometro_uma_medicao.py:40`, `app/core/tests/test_r1_termometro_uma_medicao.py:44`, `app/core/tests/test_selo_r6_frase_do_juiz.py:16`, `app/core/tests/test_l114_placar_papel_e_termometro.py:30`). O selo de afirmações lê só `mod.RESULTADOS` (`bin/tests/test_placar_nao_afirma_fora_do_ar.sh:80`); handoff usa regex específica da frase dos contratos (`bin/handoff_sessao.sh:178`). Contrato que enumere exatamente o conjunto de nomes globais desses dois módulos não foi encontrado; ausência absoluta e execução integral dos testes: **[não verificado]**.

- **`regua` e pre-push são consumidores indiretos do resultado do selo:** a régua percorre `bin/tests/test_*.sh` (`bin/regua.sh:138`) e o pre-push chama especificamente o selo com o SHA empurrado (`bin/pre-push.sh:38`); nenhum desses trechos interpreta `PAPEL_DO_MODULO`. (`bin/regua.sh:140`, `bin/pre-push.sh:38`)

- **Juiz duplicado/derivação paralela:** a mudança reaproveita `_julgar_linha` e o universo existente, adicionando callback e predicado de elegibilidade (`bin/tests/test_lei_akita.sh:177`, `bin/tests/test_lei_akita.sh:255`). Não demonstra uma segunda autoridade de cálculo/dinheiro/batida. O registro vivo define autoridades por pergunta (`app/core/juizes.py:3`, `app/core/juizes.py:7`), enquanto o anexo de consulta declara sua própria natureza de lookup (`app/docs/JUIZES.md:7`). A reprovação demonstrável é a divergência entre a promessa de “medida” e a sintaxe aceita, acima. (`bin/tests/test_lei_akita.sh:124`, `bin/tests/test_lei_akita.sh:152`)

## 06/11 20:19 · sessao edb5b413dba1 · - · - · origem=Ronald (tmux)

O MORDE reutiliza o juiz real `julgar` e `medida_do_commit`; o portão rejeita qualquer `MORDE_FALHOU`, embora `MORDE_OK` seja impresso ao final independentemente das falhas. Portanto a impressão de `MORDE_OK` isoladamente não prova sucesso. (`bin/tests/test_lei_akita.sh:347`, `bin/tests/test_lei_akita.sh:473`, `bin/tests/test_lei_akita.sh:488`)

| Caso | Mutante que o caso distingue, por inspeção |
|---|---|
| `so_numero`, `comentario`, `doc` verdes | Aceite desligado, ou recusar toda atribuição/comentário/docstring. (`bin/tests/test_lei_akita.sh:330`, `bin/tests/test_lei_akita.sh:341`) |
| `na_def` vermelho | Liberar corpo de função; também mata “todas as linhas livres”. (`bin/tests/test_lei_akita.sh:333`, `bin/tests/test_lei_akita.sh:342`) |
| `com_import` vermelho | Liberar import de topo. (`bin/tests/test_lei_akita.sh:334`, `bin/tests/test_lei_akita.sh:342`) |
| `com_lambda` vermelho | Remover veto a lambda dentro de atribuição. (`bin/tests/test_lei_akita.sh:335`, `bin/tests/test_lei_akita.sh:342`) |
| `sem_decl` vermelho | Dispensar declaração. (`bin/tests/test_lei_akita.sh:336`, `bin/tests/test_lei_akita.sh:343`) |
| `mais_fech` vermelho | Ignorar outro arquivo de negócio ou examinar só o primeiro arquivo. (`bin/tests/test_lei_akita.sh:337`, `bin/tests/test_lei_akita.sh:343`) |
| Número com `n/a (instrumento)` vermelho | Estender a exceção ao outro rótulo. (`bin/tests/test_lei_akita.sh:344`) |
| ALVO sem declaração, julgando `so_numero`, vermelho | Ler declaração em `h` em vez de ALVO. (`bin/tests/test_lei_akita.sh:349`) |

São distinções deduzidas do código; a execução dos dois mutantes alegados, seus resultados vermelhos e os três commits verdes permanecem **[não verificado]**. O caminho `logs/akmed/red_vivo.log` citado no anexo não está disponível nesta cópia **[não verificado]**.

O bloco não cobre individualmente classe, decorador, docstring não inicial, chamada em atribuição, `AnnAssign`, reatribuição de nome funcional, monkeypatch, declaração anulada depois, novo arquivo, exclusão, rename, merge de medida, erro git nem concessão retroativa de declaração pelo ALVO. Seu catálogo completo contém somente os nove casos e a checagem adicional descritos acima. (`bin/tests/test_lei_akita.sh:330`, `bin/tests/test_lei_akita.sh:340`, `bin/tests/test_lei_akita.sh:349`)

Há uma atribuição multilinha positiva (`PL`/`so_numero`) e inserção de comentário; falta caso negativo que dependa exclusivamente do lado antigo do diff. `na_def` tem linhas presas nos dois lados, enquanto import e lambda são negativos pelo lado novo. (`bin/tests/test_lei_akita.sh:314`, `bin/tests/test_lei_akita.sh:330`, `bin/tests/test_lei_akita.sh:331`, `bin/tests/test_lei_akita.sh:333`)

Os casos de medida não testam a integração com a enumeração do universo: os commits recebem mensagem `"c"` e o teste chama diretamente `julgar` com mensagem e lista de arquivos construídas manualmente. (`bin/tests/test_lei_akita.sh:325`, `bin/tests/test_lei_akita.sh:340`, `bin/tests/test_lei_akita.sh:347`)

A cláusula “erro de git = PADRAO_ERRO” não é cumprida literalmente por todas as novas operações: falha de `git show` vira AST ausente; qualquer falha de `rev-parse h^1` vira `False`; apenas o novo `git diff` levanta explicitamente `PADRAO_ERRO`. Esses dois primeiros caminhos normalmente recusam a exceção, mas não emitem a classificação de erro prometida. (`bin/tests/test_lei_akita.sh:131`, `bin/tests/test_lei_akita.sh:160`, `bin/tests/test_lei_akita.sh:164`)

## 06/11 20:16 · sessao 3375283c7246 · - · - · origem=Ronald (tmux)

Chamadores diretos de produção de `minutos_noturnos`:

- `MotorBase.calcular_noturnas`, definição `app/ponto/motor_calculo_v2.py:969`, chamada `:983`: recebe `[e,s]` após `_janela_do_par`, aplicado em `:2759`, `:2886`, `:3211`. Os escritores chamam essa função em `:2792`, `:2896`, `:3247`.
- `he_noturna`, definição `app/ponto/motor_calculo_v2.py:409`, chamada `:441`: recebe `[_inicio_do_trecho_extra(...), saida]`, calculado em `:440`. `_aplicar_he_noturna` passa `periodo.entrada/saida` em `:1015`.
- `minutos_noturnos_dos_intervalos`, definição `app/ponto/motor_calculo_v2.py:121`: mede prefixos `[entrada,b]` e `[entrada,a]` em `:191–192`, além do total `[entrada,saida]` em `:196`.
- `noturnas_do_dia`, definição `app/ponto/calculador/regras.py:66`, chamada `:85`: recebe cada par fornecido pelo calculador. O caminho de produção pode fornecer segmentos cortados pelos intervalos: `app/ponto/calculador/alimentacao.py:128`, `:133`, `:137`, `:158`; a função `noturno_no_par` fornece o par inteiro: `app/ponto/calculador/periodo.py:488`, `:510`.

`MotorPlantao` usa a conta de `Motor12x36.calcular_periodo`, por delegação explícita em `app/ponto/motor_calculo_v2.py:3510–3511`; portanto não constitui outro chamador direto.

O universo real exige mais que `batidas_apuraveis + parear_turnos`: `calcular_mes` remove duplicatas em `app/ponto/motor_calculo_v2.py:2351`, monta períodos em `:2354`, e `_periodos_via_autoridade` aplica `periodos_de_turnos(turnos_via_autoridade(...))` em `:530`, seguido de `_periodos_pelo_marco` em `:531`. O filtro por dia da jornada antecede a chamada de `calcular_periodo`: `:2403`, `:2429–2432`, `:2458`.

Recomendação de instrumentação: `sys.setprofile` observando os objetos `__code__` reais permite coletar os argumentos e retornos de `minutos_noturnos`, sem reconstruir os recortes e sem alterar módulos. Contar separadamente os canais principal, HE e prefixos de intervalos; prefixos não são novos turnos. A HE pode ser chamada novamente sobre o último período após agregação diária/semanal: `app/ponto/motor_calculo_v2.py:3147`, `:3178`, `:3399`.

Para o excesso de (b), reproduzir apenas a geometria da sobreposição, após verificar o limite de 26h de `app/ponto/motor_calculo_v2.py:94`. Para cada dia civil com a condição de prorrogação de `:110`, somar:

```python
max(0.0, (min(fim, meia_noite_seguinte)
          - max(inicio, inicio_do_dia, hora_22)).total_seconds() / 60)
```

As duas parcelas que produzem a duplicidade são explicitamente somadas em `app/ponto/motor_calculo_v2.py:112–116`.

O fechamento com `somente_leitura=True` e `lavrar_dias_pagos=False` não percorre o calculador. O chamador do calculador fica no ramo de lavratura em `app/ponto/services/fechamento.py:1253`, `:1255`, condicionado a competência ≥10/2026 em `:1249`. O calculador de HE chama a mesma `he_noturna` em `app/ponto/calculador/regras.py:832`, usando `he_pontas` formadas com as pontas efetivas em `app/ponto/calculador/alimentacao.py:83`, `:101–106`. Portanto medir somente o fechamento em leitura não autoriza imprimir zero para os chamadores do calculador.

## 06/11 20:16 · sessao 5f84fc60e33a · - · - · origem=Ronald (tmux)

Desenho recomendado para o SCRIPT 2:

1. Chamar `recalcular_fechamento_mes(mes, 2026, empresa_id=empresa, colaborador_ids=[pk], somente_leitura=True, lavrar_dias_pagos=False, retornar_objeto=True)`. O universo é `universo_da_competencia`; a flag `lavrar_dias_pagos=True` altera o comportamento da guarda de competências exportadas, portanto prejudica a equivalência do universo. `app/ponto/services/fechamento.py:373`, `app/ponto/services/fechamento.py:530`, `app/ponto/services/fechamento.py:545`.

2. Capturar `dict(frame.f_locals)` apenas no evento `return` de `recalcular_fechamento_mes.__code__`, mediante `sys.setprofile`. Isso entrega o `resultado` composto e os mapas usados pelo escritor de dias: `_ft_certa`, `_ft_feriado`, `_ft_sem_escala`, `_orfaos_por_dia`, `_prev_dia_m`, `_real_dia_m`. As entradas efetivas da lavratura estão em `app/ponto/services/fechamento.py:1185`.

3. Para obter valores diários sem copiar aritmética, chamar a função **real** `dia_pago.lavrar` sobre uma fachada temporária de `ponto.models.DiaPago`. A fachada instancia objetos reais não salvos; somente `.objects.filter(...).delete()` e `.objects.bulk_create(...)` são substituídos por operações em memória. A função real monta todos os números antes dessas duas operações. `app/ponto/services/dia_pago.py:135`, `app/ponto/services/dia_pago.py:173`, `app/ponto/services/dia_pago.py:267`, `app/ponto/services/dia_pago.py:283`, `app/ponto/services/dia_pago.py:333`, `app/ponto/services/dia_pago.py:357`.

```python
from unittest.mock import patch
import ponto.models as modelos
from ponto.services.dia_pago import lavrar

RealDiaPago = modelos.DiaPago
capturadas = []

class Memoria:
    def filter(self, **kwargs):
        return self

    def delete(self):
        return 0, {}

    def bulk_create(self, objetos):
        capturadas.extend(objetos)
        return objetos

class DiaPagoEmMemoria:
    TIPO_DIA = RealDiaPago.TIPO_DIA
    TIPO_AJUSTE = RealDiaPago.TIPO_AJUSTE
    objects = Memoria()

    def __new__(cls, *args, **kwargs):
        return RealDiaPago(*args, **kwargs)

# L = snapshot dos locals de recalcular_fechamento_mes,
# rodado exclusivamente para este colaborador.
r = L["resultado"]
with patch.object(modelos, "DiaPago", DiaPagoEmMemoria):
    lavrar(
        colab, mes, 2026,
        periodos=r.periodos,
        ft_certa=L["_ft_certa"],
        ft_feriado=L["_ft_feriado"],
        ft_sem_escala=L["_ft_sem_escala"],
        orfaos_por_dia=L["_orfaos_por_dia"],
        previsto_por_dia=L["_prev_dia_m"],
        realizado_por_dia=L["_real_dia_m"],
        dia_da_jornada=getattr(r, "dia_da_jornada", None),
        versao="motor",
    )
```

4. Detectar aberto pela saída **real** de `MotorBase._turnos_do_juiz`, capturada no evento `return` pelo objeto de código. O retorno contém `Turno(data_turno, entrada, saida, batidas, aberto, cross_meianoite)`; filtrar `entrada is not None`, `saida is None`, `aberto`, e `data_ini <= data_turno <= data_fim` dos argumentos desse frame. A geometria desta porta responde pelo dia e pelo intervalo; os períodos e minutos continuam sendo os do motor. `app/ponto/turnos.py:41`, `app/ponto/motor_calculo_v2.py:867`, `app/ponto/motor_calculo_v2.py:882`, `app/ponto/motor_calculo_v2.py:2346`.

5. Como informação adicional, comparar com `p.turno_aberto` nos períodos finais, agrupados por `resultado.dia_da_jornada[id(p)]`. O mapa preserva os mesmos objetos na composição de fatias. `app/ponto/services/fechamento.py:292`, `app/ponto/services/fechamento.py:302`.

O código já admite crédito de horas em período ainda marcado aberto, a partir dos pares fechados existentes no turno; isso impede interpretar automaticamente `aberto E horas>0` como pagamento de um dia cheio. A conclusão para col882 e para a frota depende da execução da sonda, ainda [não verificado]. `app/ponto/motor_calculo_v2.py:2495`, `app/ponto/motor_calculo_v2.py:2514`, `app/ponto/motor_calculo_v2.py:2558`.

Erros internos do fechamento podem ser capturados e consumidos pelo próprio serviço; retorno ausente para o colaborador deve virar erro, nunca zero. A grade também pode ficar sem resposta (`minutos_* = None`), e os mapas vazios não devem ser apresentados como minutos zero. `app/ponto/services/fechamento.py:1071`, `app/ponto/services/fechamento.py:1078`, `app/ponto/services/fechamento.py:1287`.

## 06/11 20:11 · sessao f922a2f8576e · PUSH-PRESO-14 · verificador · origem=Code

**Meu veredito: os três commits R1 publicam ou corrigem medições; o quarto implementa idempotência pela chave do fato e reutiliza autoridades existentes.** Evidências abaixo usam `hash:arquivo:linha` com a numeração nova dos diffs anexados. (`36c90132:app/core/placar_estrutural.py:106–120`; `6ed6adbe:app/core/placar_estrutural.py:106–119`; `a16637ca:app/core/placar_estrutural.py:108–118`; `5a3f8d0e:app/pautas/management/commands/pauta_pago_x_devido_exportada.py:130–139,190–213`)

1. **`36c90132` — nenhum termo arquitetural é realizado pelo diff mostrado.** Ele substitui números e provas, troca `fonte_do_medido` e remove afirmações sobre destinos que a nova medição não responde; os estados dos itens permanecem iguais. (`36c90132:app/core/placar_estrutural.py:106–120`; `36c90132:app/core/espelho_verdade.py:168–180`)

   Dois candidatos seriam **Single Source of Truth / fonte única** e **Architectural Invariants + Fitness Functions**, existentes no vocabulário. Porém, atualizar referências para a mesma medição não implementa uma autoridade única; publicar o resultado da SOMA também não implementa sua verificação. Neste diff, ambos permanecem conteúdo do placar. (`app/docs/GLOSSARIO.md:11,14`; `36c90132:app/core/placar_estrutural.py:106–120`)

2. **`6ed6adbe` — nenhum termo arquitetural é realizado pelo diff mostrado.** Ele reorganiza provas para apresentar números primeiro, restringe afirmações ao universo medido e corrige a atribuição da conferência da SOMA para a sonda. (`6ed6adbe:app/core/espelho_verdade.py:49,82,168–180`; `6ed6adbe:app/core/placar_estrutural.py:106–119`)

   Os candidatos seriam novamente **fonte única** e **fitness function / selo**. O diff corrige a descrição das fontes e verificações, mas não altera o mecanismo que impõe uma fonte única nem acrescenta a verificação executável; as mudanças de teste narradas na mensagem não aparecem no diff anexado. (`app/docs/GLOSSARIO.md:11`; `app/docs/PLANO-PADROES.md:48`; `6ed6adbe:app/core/placar_estrutural.py:106–119`; `6ed6adbe:app/core/espelho_verdade.py:49,82,168–180`)

3. **`a16637ca` — nenhum termo arquitetural é realizado pelo diff mostrado.** Ele compacta números e ressalvas nas provas, corrige `SOMA.fonte` e documenta a partição `pk % 6`; não implementa essa partição. A compactação tem efeito concreto na publicação porque o leitor recorta a prova em 150 caracteres. (`a16637ca:app/core/espelho_verdade.py:49,82,168–180`; `a16637ca:app/core/placar_estrutural.py:108–118`; `bin/gerar_estado.py:164`)

   Dois candidatos seriam **fitness function / selo** e **Shadow mode / Parallel run — sombra**. O diff apenas relata a conferência e a execução em sombra; não acrescenta um selo nem um caminho de execução paralela. (`app/docs/PLANO-PADROES.md:48`; `app/docs/GLOSSARIO.md:19`; `a16637ca:app/core/placar_estrutural.py:108–118`)

4. **`5a3f8d0e` — realiza principalmente `idempotencia pela chave do fato` / `dedup do fato`.** A chave contém marca, colaborador e competência; a busca acrescenta a âncora da empresa/competência e considera cabeças não canceladas, inclusive feitas. Dentro da transação, trava o colaborador, consulta novamente a existência e só cria quando ausente. Isso implementa o mecanismo, além de declará-lo. (`app/docs/GLOSSARIO.md:27`; `5a3f8d0e:app/pautas/management/commands/pauta_pago_x_devido_exportada.py:53–54,173–174,190–213,240–243`)

   O segundo candidato, **single source of truth / um juiz por pergunta**, também é realizado no consumo das autoridades: PAGO vem de `valores_do_gravado`, DEVIDO do recalculador existente em modo leitura, DIFERE de `_diferente` e ENTREGUE de `competencia_entregue`. Para escolher um único rótulo principal, escolheria **`padrao=idempotencia pela chave do fato`**, pela implementação explícita de deduplicação serializada. (`app/docs/PLANO-PADROES.md:46–47`; `5a3f8d0e:app/pautas/management/commands/pauta_pago_x_devido_exportada.py:109–139,190–213`)

Quanto ao comportamento:

- **Os três R1 alteram informação e apresentação do placar, sem modificar cálculo financeiro ou registro de batidas.** No primeiro, trocar a fonte também pode mudar a data reconhecida e, consequentemente, o veredito de cadência do termômetro; portanto há efeito no placar além da prosa. (`36c90132:app/core/placar_estrutural.py:111`; `6ed6adbe:app/core/espelho_verdade.py:49`; `a16637ca:app/core/espelho_verdade.py:168`; `bin/gerar_estado.py:47–56`; `app/core/placar_estrutural.py:676–684`)
- **O quarto não é só medida:** por padrão é leitura, mas `--escrever` cria pautas DP e `--reverter` cancela pautas. Ele consulta o cálculo financeiro em `somente_leitura=True`, sem habilitar a lavratura de `DiaPago`; não introduz alteração de regra de dinheiro nem escrita de batidas. (`5a3f8d0e:app/pautas/management/commands/pauta_pago_x_devido_exportada.py:135,161–188,207–217,259–283`; `app/ponto/services/fechamento.py:439–442,1112–1128,1175`)
- Execução efetiva desses caminhos e efeitos observados em produção: **[não verificado]**.

A recusa mecânica dos três primeiros decorre da classificação por caminho: `app/`, fora de testes e documentos, é “negócio” e recusa `n/a`. Isso não demonstra que realizem um padrão; a própria L-123 exige conferir o rótulo contra o diff. (`bin/tests/test_lei_akita.sh:122–135`; `app/docs/PLANO-PADROES.md:53,58`)

## 06/11 20:07 · sessao 631ab37d7510 · BUG-PLANTIO-NOTURNO-MAIS-1 · verificador · origem=Code

**As três curas fecham os caminhos locais apontados, com ressalvas:** (a) acrescenta um caso de push sem pergunta; (b) evita a exceção, mas não garante recuperação posterior; (c) cobre inserção recente, enquanto reaproveitamento conserva a criação antiga. `app/chamados/services/disputa_emissao.py:411`, `app/chamados/services/disputa_emissao.py:640`, `app/ponto/services/triagem_batida.py:730`, `app/chamados/services/materializacao.py:922`.

RED real, execução Django contra o pai e ocorrência em produção: **[nao verificado]**. A avaliação dos testes abaixo é estática, usando o delta fornecido e os caminhos citados. [nao verificado]

**1. Curas e alcance de (a)**

- **(a) Fecha o fallback defeituoso:** sem dia, uma pergunta `hora` agora para antes de `abrir_conversa`; perguntas de outros tipos continuam podendo nascer como conversa. `app/chamados/services/disputa_emissao.py:410`, `app/chamados/services/disputa_emissao.py:417`.
- **(b) Fecha `None.isoformat()` na triagem:** `_T1` é consultado uma vez e a disputa só nasce quando há dia. `app/ponto/services/triagem_batida.py:726`, `app/ponto/services/triagem_batida.py:739`.
- **(c) Fecha a exclusão da inserção retroativa recente:** troca o filtro temporal, preservando os demais critérios e a consulta ao mesmo juiz. Não achei juiz ou escritor novo nessa alteração. `app/ponto/management/commands/vigia_plantio_overnight.py:73`, `app/ponto/management/commands/vigia_plantio_overnight.py:76`, `app/ponto/management/commands/vigia_plantio_overnight.py:87`.

**Caminho legítimo “nasce conversa-hora e outro escritor preenche o dia depois”: nao achei.** A conversa já recebe `localdate()` no nascimento; `reapontar_resolvedora` só escreve a data quando ela está vazia. Portanto, esse escritor posterior não corrigiria o “hoje” daquela conversa. `app/chamados/models.py:2426`, `app/chamados/services/fio.py:835`.

Os quatro motivos `hora` do catálogo são `orfao_14h`, `saida_sem_entrada`, `intervalo_saida` e `intervalo_volta`. Os intervalos sem dia já eram recusados antes da guarda nova; o caso demonstrável atingido por ela é órfão/SSE com texto produzido, mas sem data válida após os fallbacks. `app/chamados/catalogo/perguntas.py:48`, `app/chamados/catalogo/perguntas.py:56`, `app/chamados/catalogo/perguntas.py:124`, `app/chamados/catalogo/perguntas.py:132`, `app/chamados/catalogo/query.py:66`, `app/chamados/services/disputa_emissao.py:387`.

**A acusação não desaparece deixando somente log:** motivos e contexto são persistidos antes da geração. A recusa específica, entretanto, fica apenas no `logger.info`, sem pergunta ou pendência própria. `app/chamados/services/disputa_emissao.py:532`, `app/chamados/services/disputa_emissao.py:557`, `app/chamados/services/disputa_emissao.py:632`, `app/chamados/services/disputa_emissao.py:415`.

**Efeito adicional concreto:** após recusar a única pergunta, `criar_ou_atualizar_disputa` ainda pode mandar “Toque para responder”. A decisão de enviar push ignora o resultado do gerador. Esse desacoplamento já existia; a cura acrescenta um caso que o alcança. `app/chamados/services/disputa_emissao.py:640`, `app/chamados/services/disputa_emissao.py:652`, `app/chamados/services/disputa_emissao.py:656`, `app/chamados/services/disputa_emissao.py:1577`.

Chamadores diretos encontrados fora dos testes: [nao verificado]

| Chamador | O que espera |
|---|---|
| `criar_ou_atualizar_disputa` | Geração como efeito; ignora a lista retornada e devolve a disputa. `app/chamados/services/disputa_emissao.py:640`, `app/chamados/services/disputa_emissao.py:652`, `app/chamados/services/disputa_emissao.py:658`. |
| `perguntar_entradas_pendentes` | Injeta dia obtido da batida via `_T1`, acumula a lista e salva contexto se ela não estiver vazia. `app/chamados/services/disputa_emissao.py:282`, `app/chamados/services/disputa_emissao.py:292`, `app/chamados/services/disputa_emissao.py:294`. |
| Fábrica `perguntar` | Fornece dia explícito; chama criação/atualização e novamente o gerador; ignora a lista. `app/chamados/services/fabrica_da_celula.py:139`, `app/chamados/services/fabrica_da_celula.py:141`. |
| `smoke_chamados` | Fornece data quando exigida e espera uma pergunta com o tipo do catálogo. `app/chamados/management/commands/smoke_chamados.py:96`, `app/chamados/management/commands/smoke_chamados.py:102`, `app/chamados/management/commands/smoke_chamados.py:104`. |

O contrato documentado é “lista das perguntas criadas”, mas a implementação também acrescenta pergunta reaproveitada, com `_criada=False`; isso antecede o delta. `app/chamados/services/disputa_emissao.py:304`, `app/chamados/services/disputa_emissao.py:449`, `app/chamados/models.py:2497`.

**2. SSE sem disputa: há recuperação?**

**Recuperador específico dessa tentativa/batida retratada: nao achei.** `registrar_batida` pode devolver a mesma chave retratada sem inserir nem salvar; `_T1` exclui retratadas. A guarda então pula a disputa e a triagem pode chegar à indicação de sucesso, sem registrar pendência própria nesse ramo. `app/ponto/registro_batida.py:120`, `app/ponto/turnos.py:2905`, `app/ponto/turnos.py:2925`, `app/ponto/services/triagem_batida.py:730`, `app/ponto/services/triagem_batida.py:757`.

**Há recuperação condicional pelo dia:** o código declara `vigia_de_hora --apply` a cada dez minutos e cartório diário. O julgamento pode chamar a fábrica, que emite perguntas pela célula. Isso não equivale a reprocessar aquela tentativa. `app/config/crons.py:367`, `app/config/crons.py:616`, `app/ponto/services/cartorio.py:981`, `app/ponto/services/cartorio.py:997`.

Não há garantia universal: o vigia seleciona células `trabalha=True`; a fábrica pode recusar por situação, veredito ou parcial, e só pergunta motivos ainda ausentes. Portanto, **pode permanecer sem pergunta; “sempre invisível” também não está demonstrado**. `app/ponto/services/vigia_de_hora.py:21`, `app/chamados/services/fabrica_da_celula.py:91`, `app/chamados/services/fabrica_da_celula.py:99`, `app/chamados/services/fabrica_da_celula.py:102`, `app/chamados/services/fabrica_da_celula.py:126`. Recuperação efetiva em produção: **[nao verificado]**.

A cura é local à triagem: API e clone ainda chamam `.isoformat()` sem essa guarda. `app/api/views.py:782`, `app/api/views_core.py:782`.

**3. Quem escreve `Batida.criado_em`; reabertura e reaproveitamento**

O escritor normal é `auto_now_add=True`, na inserção da linha. A porta cria por `Batida.objects.create`; `timestamp` é um campo separado. `app/ponto/models.py:23`, `app/ponto/models.py:79`, `app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:157`.

**Reescritor Python específico de `Batida.criado_em`: nao achei.** A porta de anotação não aceita esse campo e salva somente os campos solicitados. `app/ponto/registro_batida.py:203`, `app/ponto/registro_batida.py:241`, `app/ponto/registro_batida.py:254`.

Há restauração genérica de schema por dump: ela restaura os valores arquivados, sem passar pelo `auto_now_add` ou renovar a criação para o horário do restore. Uso efetivo em produção: **[nao verificado]**. `scripts/backup_tenant.sh:10`, `scripts/restore_tenant.sh:8`.

**Batida nova tem criação do novo plantio; reaproveitada conserva a criação original.** O materializador procura existente antes de criar e, no reaproveitamento, pode trocar apenas `pergunta_origem`. A reabertura do questionário limpa resposta/validação, sem renovar a batida. `app/chamados/services/materializacao.py:835`, `app/chamados/services/materializacao.py:847`, `app/chamados/services/materializacao.py:903`, `app/chamados/services/materializacao.py:922`, `app/chamados/services/disputa_emissao.py:1434`.

Assim, uma `disputa_s84_retro` antiga revinculada hoje pode ficar fora da janela; uma `app` reaproveitada permanece fora pelo filtro de origem. Esse limite impede interpretar o contador como cobertura de **toda aplicação recente**. `app/ponto/management/commands/vigia_plantio_overnight.py:16`, `app/ponto/management/commands/vigia_plantio_overnight.py:76`, `app/chamados/services/materializacao.py:924`.

**4. Os testes mordem o pai?** [nao verificado]

As conclusões desta tabela são previsões estáticas contra os trechos anteriores mostrados no delta; execução no pai: **[nao verificado]**.

| Teste | Resultado sem a cura correspondente | [nao verificado]
|---|---|
| `PerguntaDeHoraSemDiaDoTurnoTest.test_MORDE_orfao...` | **FALHA.** Data malformada produz texto, mas não dia; sem a guarda, nasce conversa datada de hoje e viola `assertFalse`. `app/chamados/tests/test_plantio_noturno_mais1.py:203`, `app/chamados/tests/test_plantio_noturno_mais1.py:206`, `app/chamados/tests/test_plantio_noturno_mais1.py:207`, `app/chamados/catalogo/query.py:66`, `app/chamados/models.py:2426`. |
| `test_contra_exemplo_escolha_sem_dia_segue_conversa` | **PASSA mesmo sem a cura.** O motivo é `escolha`; a conversa já era o resultado anterior. `app/chamados/tests/test_plantio_noturno_mais1.py:210`, `app/chamados/tests/test_plantio_noturno_mais1.py:214`, `app/chamados/catalogo/perguntas.py:84`. |
| `VigiaContaPeloPlantioNaoPeloInstanteTest.test_MORDE...` | **FALHA.** Em 01/12, a saída com instante de 23/10 fica fora do corte de trinta dias do pai; o novo `criado_em` de 01/12 a inclui. `app/chamados/tests/test_plantio_noturno_mais1.py:30`, `app/chamados/tests/test_plantio_noturno_mais1.py:32`, `app/chamados/tests/test_plantio_noturno_mais1.py:192`, `app/chamados/tests/test_plantio_noturno_mais1.py:195`, `app/ponto/management/commands/vigia_plantio_overnight.py:76`. |
| Caso congelado de `test_a2` | **PASSA no pai com esse congelamento.** Não é RED das curas: o motivo é `escolha` e a ausência da linha já levava à conversa. O congelamento elimina a contradição que ocorreria em 12/08/2026 entre `!=12/08` e `==localdate()`. `app/chamados/tests/test_a2_data_evento_batida.py:51`, `app/chamados/tests/test_a2_data_evento_batida.py:56`, `app/chamados/tests/test_a2_data_evento_batida.py:59`, `app/chamados/tests/test_a2_data_evento_batida.py:60`, `app/chamados/services/disputa_emissao.py:383`. |

O teste de hora mata precisamente o fallback antigo, mas só proíbe nascimento **hoje**, não qualquer nascimento indevido. O teste anterior do vigia continua aceitando qualquer delta `>=1`. `app/chamados/tests/test_plantio_noturno_mais1.py:207`, `app/chamados/tests/test_plantio_noturno_mais1.py:178`.

**5. Escritores, leitores e ESMERIL-SEMANTICA**

| Campo | Quem escreve/fornece | Quem lê |
|---|---|---|
| `PerguntaDisputa.data_evento` | Gravação efetiva em `_nascer`; preenchimento de pergunta sem dia em `reapontar_resolvedora`. Gerador, agrupador e declarações fornecem o valor; conversa fornece `localdate()`. `app/chamados/models.py:2498`, `app/chamados/services/fio.py:837`, `app/chamados/services/disputa_emissao.py:430`, `app/chamados/services/disputa_emissao.py:1029`, `app/chamados/services/declaracao_texto.py:327`, `app/chamados/models.py:2426`. | Plantio/slot como dia do turno; dedup/fábrica como identidade do dia; juízes/reconciliador como dia do fato; folha/calendário como agrupamento; API como apresentação. `app/chamados/services/materializacao.py:344`, `app/chamados/juizes.py:319`, `app/chamados/models.py:2396`, `app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/reconciliador.py:1080`, `app/folha/previa.py:67`, `app/colaboradores/services/calendario.py:477`, `app/api/views.py:1174`. |
| `contexto['saida_sem_entrada']` | Triagem fornece dia de `_T1`; APIs substituem a data civil transitória por `_T1`; fábrica fornece dia da célula; agregador persiste/mescla. `app/ponto/services/triagem_batida.py:739`, `app/api/views.py:782`, `app/api/views_core.py:782`, `app/chamados/services/fabrica_da_celula.py:139`, `app/chamados/services/disputa_emissao.py:529`, `app/chamados/services/disputa_emissao.py:560`. | Texto, data da pergunta, seleção de chamado e fallback de plantio/completude. `app/chamados/catalogo/query.py:58`, `app/chamados/services/disputa_emissao.py:364`, `app/chamados/services/disputa_emissao.py:196`, `app/chamados/services/materializacao.py:343`, `app/chamados/juizes.py:365`. |
| `Batida.criado_em` | Inserção automática; restore recupera valor arquivado. `app/ponto/models.py:79`, `scripts/restore_tenant.sh:8`. | Vigia como criação recente; detector de flush como chegada ao servidor; contador de sequência como corte de criação; cache/cartório como marco de fato novo. `app/ponto/management/commands/vigia_plantio_overnight.py:76`, `app/ponto/turnos.py:355`, `app/ponto/turnos.py:366`, `app/ponto/turnos.py:2384`, `app/chamados/veredito_lavrado.py:138`, `app/ponto/services/cartorio.py:353`. |

**Divergências encontradas:**

- **Permanece a conhecida conversa × turno:** nascimento civil da conversa e leitores de identidade/célula sem discriminador persistido. A cura elimina o caminho `hora` do gerador, não a colisão geral já declarada fora do escopo. `app/chamados/models.py:2426`, `app/chamados/models.py:2453`, `app/chamados/services/fabrica_da_celula.py:118`.
- **Criação × reaplicação:** `criado_em` significa nascimento da linha; o vigia o chama de plantio recente, embora a aplicação possa reaproveitar linha antiga. `app/ponto/models.py:79`, `app/ponto/management/commands/vigia_plantio_overnight.py:67`, `app/chamados/services/materializacao.py:835`.
- **Leitor atual de `saida_sem_entrada.data` especificamente como dia civil da saída: nao achei.** Texto pergunta entrada naquele dia, e plantio usa dia do turno. Permanece a diferença de chave nas APIs: motivo `orfao_14h`, contexto `saida_sem_entrada`. `app/chamados/catalogo/perguntas.py:57`, `app/chamados/juizes.py:319`, `app/api/views.py:654`, `app/api/views.py:655`.

**6. Classes que já reprovaram obra**

| Classe | Resultado nesta volta |
|---|---|
| Segundo juiz | **Novo no delta: nao achei.** Triagem e vigia consultam `_T1`/`dia_das_batidas`. `app/ponto/services/triagem_batida.py:729`, `app/ponto/management/commands/vigia_plantio_overnight.py:87`. |
| Segundo escritor fora da porta | **Efetivo novo: nao achei.** Escritas encontradas continuam no nascimento e costura. `app/chamados/models.py:2498`, `app/chamados/services/fio.py:837`. |
| Relógio fora da borda | **Permanece no helper do vigia**, sem instante recebido do chamador; a troca do filtro não o criou. `app/ponto/management/commands/vigia_plantio_overnight.py:76`. |
| Derivação paralela | **Não acrescentada pelo delta; permanece na fila:** parser de contexto no gerador/agrupador e contexto fraco. `app/chamados/services/disputa_emissao.py:367`, `app/chamados/services/disputa_emissao.py:1107`, `app/chamados/data_evento.py:27`, `app/chamados/juizes.py:162`. |
| Selo/RED sem mordida | **Os novos casos principais de (a) e (c) têm mordida estática.** Permanecem as lacunas do selo para `**dict` e `setattr` por variável; o caso congelado é estabilização, não RED comportamental do delta. `app/chamados/tests/test_plantio_noturno_mais1.py:195`, `app/chamados/tests/test_plantio_noturno_mais1.py:207`, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:86`, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:91`, `app/chamados/tests/test_a2_data_evento_batida.py:51`. |
| Campo com dois significados | **Persistem conversa/turno e o limite criação/reaplicação descritos acima.** O delta não resolve o discriminador nem registra um instante próprio de reaproveitamento. `app/chamados/models.py:2450`, `app/chamados/models.py:2498`, `app/chamados/services/materializacao.py:924`. |

## 06/11 20:05 · sessao 8a7c282839f5 · ESMERIL-SEM-INSTANTE · verificador · origem=Code

**Não encontrei bloqueante novo no delta fornecido; os três achados procedentes foram corrigidos.** (`app/ponto/tests/test_composicao_de_fatias_e_total.py:281`, `:333`; `app/ponto/tests/test_contract_no_batida_date.py:72`)

1. **As três curas conferem.**

   - **Teste 3:** agora exige exatamente `[utc[0], utc[1], utc[3]]` em A. Revertendo a conversão para `b.timestamp.date()`, A recebe `[utc[2], utc[4]]`: o novo assert falha mesmo isolado da igualdade anterior. Se o filtro entregar `[]`, também falha; se ambos os resultados forem `{}`, ocorre `KeyError` em `A.pk`, portanto não passa silenciosamente. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:325`, `:330`, `:333`; `app/ponto/services/fechamento.py:204`, `:254`, `:261`)
   - **Gravador:** deixou de ordenar e registra a sequência recebida pelo motor. Está curado o mascaramento introduzido pelo próprio capturador. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:279`, `:281`)
   - **ALLOWLIST:** a justificativa agora aponta precisamente para `_com_batida`, que ainda contém `timezone.localtime(b.timestamp).date()` e é reconhecido pelo visitante. (`app/ponto/tests/test_contract_no_batida_date.py:72`, `:208`; `app/ponto/services/fechamento.py:793`)

2. **Não há relógio solto, import morto novo ou dependência indevida de ordem nas alterações fornecidas.** Os instantes são literais, o fuso fica sob `override`, e a ordem esperada vem da lista entregue explicitamente pelo teste. As compreensões da porta preservam essa sequência; não dependem da ordem de conjuntos ou de empates no banco. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:245`, `:325`, `:327`, `:328`, `:333`; `app/ponto/services/fechamento.py:204`, `:253`, `:260`, `:262`)

   **Limite da cobertura:** o gravador foi corrigido, mas o teste ainda não prova preservação de uma entrada arbitrariamente embaralhada: os três sobreviventes já estão em ordem cronológica. Uma ordenação indevida na porta ainda poderia passar neste caso. Isso não invalida a cura do capturador. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:281`, `:325`, `:326`, `:333`)

3. **Não mantenho os descartes como bloqueantes desta fatia.** A restrição para naive existe, mas os três caminhos encontrados carregam listas pelo ORM, com `DateTimeField` e `USE_TZ=True`; não há origem naive demonstrada nesses caminhos. Isso sustenta o contrato aware sem provar os valores efetivamente recebidos em produção **[nao verificado]**. (`app/ponto/services/fechamento.py:633`; `app/ponto/services/espelho.py:1022`; `app/relatorios/pdf_espelho.py:272`; `app/ponto/models.py:23`; `app/config/settings/base.py:95`; `app/ponto/turnos.py:44`)

   `None` continua inválido: a conversão devolve `None`, que falha na comparação com datas; não há tratamento novo que prometa aceitá-lo. (`app/ponto/turnos.py:46`; `app/ponto/services/fechamento.py:254`)

4. **Nenhum bloqueante novo identificado.** O residual de saída após meia-noite sob células alternadas permanece: o filtro ainda pode excluir a saída pelo dia civil de outro vínculo. A correção desse residual não é demonstrada por estes asserts. (`app/ponto/services/fechamento.py:255`, `:261`; `app/ponto/tests/test_composicao_de_fatias_e_total.py:333`)

**Validação:** em memória, com dependências substituídas, os três testes passaram na cópia e falharam nos mutantes `.date()` e filtro vazio; o assert novo também falhou isoladamente. Os quatro testes do contrato passaram isolados. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:291`, `:306`, `:316`, `:333`; `app/ponto/tests/test_contract_no_batida_date.py:236`)

Suíte Django real, correspondência Git com o hash/base e integralidade do delta: **[nao verificado]**. [nao verificado]

## 06/11 20:05 · sessao 2d4a03cfa6b6 · - · - · origem=Ronald (tmux)

**(b) fecha a exceção na triagem, mas não garante uma pergunta posterior.** O retorno de `registrar_batida` pode ser a mesma chave já retratada, sem criação ou `save`; `_T1` não inclui retratadas e devolve `None`; a nova guarda evita `.isoformat()` nesse retorno. Referências: `app/ponto/registro_batida.py:116`, `app/ponto/registro_batida.py:120`, `app/ponto/turnos.py:2905`, `app/ponto/turnos.py:2917`, `app/ponto/turnos.py:2925`, `app/ponto/services/triagem_batida.py:726`, `app/ponto/services/triagem_batida.py:730`.

**Outro emissor que retome especificamente essa tentativa/batida retratada: nao achei.** O retorno antecipado de mesma chave não dispara os sinais de criação, e a triagem segue até informar sucesso; a nova guarda não grava pendência nem emite log próprio. Referências: `app/ponto/registro_batida.py:120`, `app/ponto/services/triagem_batida.py:730`, `app/ponto/services/triagem_batida.py:757`.

Há emissores independentes capazes de perguntar **pelo dia**, sob condições: vigia de hora declarado a cada dez minutos e cartório diário; o cartório chama a fábrica depois do julgamento, e a fábrica exige célula elegível, veredito acusador e motivo ainda sem pergunta. Portanto, não cabe afirmar “fica sempre invisível”, nem “o cron garante recuperação”. Referências: `app/config/crons.py:367`, `app/config/crons.py:616`, `app/ponto/services/cartorio.py:981`, `app/ponto/services/cartorio.py:997`, `app/chamados/services/fabrica_da_celula.py:91`, `app/chamados/services/fabrica_da_celula.py:99`, `app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/services/fabrica_da_celula.py:164`.

Limites concretos: `julgar_celula` recusa célula inexistente; vigia de hora seleciona `trabalha=True`; a fábrica também pode calar por parcial/lista; o lote `disparar_perguntas_competencia` apenas conta o caminho da célula e só emite no caminho de chamado vivo já existente com célula. Referências: `app/ponto/services/cartorio.py:1976`, `app/ponto/services/vigia_de_hora.py:21`, `app/chamados/services/fabrica_da_celula.py:102`, `app/chamados/management/commands/disparar_perguntas_competencia.py:86`, `app/chamados/management/commands/disparar_perguntas_competencia.py:141`, `app/chamados/management/commands/disparar_perguntas_competencia.py:158`. Recuperação efetiva em produção: **[nao verificado]**.

**Censo `DisputaSupervisao.contexto['saida_sem_entrada']`:**

| Papel | Evidência |
|---|---|
| PWA fornece `data` de `_T1` e hora da tentativa | `app/ponto/services/triagem_batida.py:734`, `app/ponto/services/triagem_batida.py:739` |
| API e clone montam data civil transitória e a substituem por `_T1` antes de persistir | `app/api/views.py:655`, `app/api/views.py:780`, `app/api/views_core.py:652`, `app/api/views_core.py:780` |
| Fábrica fornece data explícita da célula, pelo motivo | `app/chamados/services/fabrica_da_celula.py:139` |
| Agregador persiste/mescla o contexto | `app/chamados/services/disputa_emissao.py:518`, `app/chamados/services/disputa_emissao.py:532`, `app/chamados/services/disputa_emissao.py:560` |
| Texto lê `data` para perguntar entrada naquele dia | `app/chamados/catalogo/query.py:58`, `app/chamados/catalogo/perguntas.py:57` |
| Gerador lê `data` como dia da pergunta datada | `app/chamados/services/disputa_emissao.py:364` |
| Seleção de chamado compara `data` com dia do fato | `app/chamados/services/disputa_emissao.py:191`, `app/chamados/services/disputa_emissao.py:231` |
| Materializador usa como fallback do dia do turno | `app/chamados/services/materializacao.py:343`, `app/chamados/services/materializacao.py:346` |
| Juiz de completude usa como fallback do turno | `app/chamados/juizes.py:363`, `app/chamados/juizes.py:365`, `app/chamados/juizes.py:372` |

**Leitor atual desse `data` especificamente como dia civil da saída: nao achei.** A conversão de overnight usa dia do turno e marcos; o recuo antigo por `hora_tentativa` está removido. Referências: `app/chamados/juizes.py:301`, `app/chamados/juizes.py:312`, `app/chamados/juizes.py:319`, `app/chamados/services/materializacao.py:419`.

Duas ressalvas preexistentes: API/clone acrescentam motivo `orfao_14h`, mas gravam a chave de contexto `saida_sem_entrada`; ambos continuam chamando `.isoformat()` sem guarda de `None`. Não são mudanças do delta fornecido. Referências: `app/api/views.py:654`, `app/api/views.py:655`, `app/api/views.py:782`, `app/api/views_core.py:651`, `app/api/views_core.py:652`, `app/api/views_core.py:782`.

## 06/11 20:04 · sessao be24a03c3f1c · - · - · origem=Ronald (tmux)

**Cura (c): fecha a omissão de inserção retroativa recente; não cobre toda reaplicação recente de pergunta.** O filtro passa a usar `criado_em`, preservando os demais critérios, e consulta o mesmo juiz `dia_das_batidas`; não achei segundo juiz nem escritor novo nessa mudança. Referências: `app/ponto/management/commands/vigia_plantio_overnight.py:73`, `app/ponto/management/commands/vigia_plantio_overnight.py:76`, `app/ponto/management/commands/vigia_plantio_overnight.py:87`.

**Escrita de `Batida.criado_em`:**

- É `DateTimeField(auto_now_add=True)`: registra nascimento da linha, enquanto `timestamp` registra instante da marcação. A porta insere por `Batida.objects.create`. Referências: `app/ponto/models.py:23`, `app/ponto/models.py:79`, `app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:157`.
- Reescritor Python específico por atribuição/update/SQL/cópia/restauração de `Batida.criado_em`: **nao achei**. A porta de anotação não aceita esse campo e salva somente os campos pedidos. Referências: `app/ponto/registro_batida.py:203`, `app/ponto/registro_batida.py:241`, `app/ponto/registro_batida.py:254`.
- Existe carga sintética em lote, restrita a banco de ensaio, construída com os dicionários recebidos. Referências: `app/ponto/registro_batida.py:316`, `app/ponto/registro_batida.py:326`, `app/ponto/registro_batida.py:331`, `app/ponto/registro_batida.py:335`.
- Há restauração genérica de schema: backup por `pg_dump`, restauração por `pg_restore --clean --if-exists`. Esse caminho restaura o valor do dump, sem passar pelo `auto_now_add`; não carimba a hora do restore. Uso efetivo e valores restaurados em produção: **[nao verificado]**. Referências: `scripts/backup_tenant.sh:10`, `scripts/restore_tenant.sh:8`.

**Reabertura/reaproveitamento:**

- Materialização procura batida existente antes de criar; aceita tanto real quanto de disputa. Se reaproveita, apenas pode mudar `pergunta_origem`, mantendo `criado_em` e `origem`. Referências: `app/chamados/juizes.py:230`, `app/chamados/juizes.py:238`, `app/chamados/services/materializacao.py:835`, `app/chamados/services/materializacao.py:847`, `app/chamados/services/materializacao.py:922`, `app/ponto/registro_batida.py:254`.
- A própria porta também pode devolver linha antiga pelo dedup ±60s ou pela mesma chave, sem inserir. Referências: `app/ponto/registro_batida.py:107`, `app/ponto/registro_batida.py:113`, `app/ponto/registro_batida.py:120`.
- Portanto, **batida realmente criada na nova aplicação tem `criado_em` desse plantio; batida reaproveitada mantém o nascimento original**. Uma linha `disputa_s84_retro` antiga revinculada hoje pode permanecer fora da janela; uma batida `app` reaproveitada continua fora pelo filtro de origem. Isso é limitação do significado “plantio recente”, não regressão criada pela troca do filtro. Referências: `app/ponto/models.py:79`, `app/chamados/services/materializacao.py:903`, `app/chamados/services/materializacao.py:922`, `app/ponto/management/commands/vigia_plantio_overnight.py:16`, `app/ponto/management/commands/vigia_plantio_overnight.py:76`.
- A reabertura do questionário limpa resposta/validação; não renova nascimento da batida. Referências: `app/chamados/services/acoes_disputa.py:102`, `app/chamados/services/disputa_emissao.py:1434`, `app/chamados/services/disputa_emissao.py:1454`.

**Teste `VigiaContaPeloPlantioNaoPeloInstanteTest`: FALHA sem a cura, por análise estática.** Cria a saída em 01/12, com `timestamp` 23/10, e exige seu PK em janela de 30 dias. No pai, o filtro `timestamp >= 01/11` fornecido no delta a exclui e produz `[]`; no delta, `criado_em` 01/12 a inclui. Referências: `app/chamados/tests/test_plantio_noturno_mais1.py:30`, `app/chamados/tests/test_plantio_noturno_mais1.py:32`, `app/chamados/tests/test_plantio_noturno_mais1.py:192`, `app/chamados/tests/test_plantio_noturno_mais1.py:195`, `app/ponto/management/commands/vigia_plantio_overnight.py:76`; código exato do pai: **[nao verificado por Git; disponível apenas no delta fornecido]**. Execução Django/RED real: **[nao verificado]**.

**Leitores de `Batida.criado_em` encontrados:**

| Leitor | Significado |
|---|---|
| Vigia novo | Nascimento recente da linha tomado como plantio recente. `app/ponto/management/commands/vigia_plantio_overnight.py:76`. |
| `_marca_ecos_flush` | Hora de chegada ao servidor versus instante do evento, para identificar flush offline. `app/ponto/turnos.py:355`, `app/ponto/turnos.py:362`, `app/ponto/turnos.py:366`. |
| `sequencia_relogio_x_jornada` | Universo de linhas criadas desde corte. `app/ponto/turnos.py:2378`, `app/ponto/turnos.py:2384`. |
| `marcos_do_lote` | Invalidação de cache por nascimento de linha; retração tem relógio separado. `app/chamados/veredito_lavrado.py:137`, `app/chamados/veredito_lavrado.py:140`. |
| Cartório | Fato novo desde julgamento: nascimento ou retração de batida na janela. `app/ponto/services/cartorio.py:350`, `app/ponto/services/cartorio.py:352`. |

**Classes regressivas pertinentes:** segundo juiz e segundo escritor novo, **nao achei**; relógio dentro do helper permanece (`timezone.now()`), e o delta apenas troca o campo filtrado. O teste novo tem mordida contra essa troca; o teste anterior continua exigindo só `delta >= 1`. Referências: `app/ponto/management/commands/vigia_plantio_overnight.py:76`, `app/ponto/management/commands/vigia_plantio_overnight.py:87`, `app/chamados/tests/test_plantio_noturno_mais1.py:178`, `app/chamados/tests/test_plantio_noturno_mais1.py:195`. A divergência semântica relevante é **nascimento da linha versus última aplicação/revinculação**, demonstrada pelo reaproveitamento sem atualização do carimbo. Referências: `app/ponto/models.py:79`, `app/chamados/services/materializacao.py:835`, `app/chamados/services/materializacao.py:922`.

## 06/11 20:04 · sessao 7d8006827f3a · - · - · origem=Ronald (tmux)

**Cura (a): fecha o caminho de conversa-hora; caminho legítimo de preencher o dia depois: nao achei.** Todas as perguntas de catálogo `hora` são `orfao_14h`, `saida_sem_entrada`, `intervalo_saida` e `intervalo_volta`; todas pedem `{data}`. Os dois intervalos sem dia já são recusados antes da cura; a guarda nova efetivamente alcança os motivos órfão/SSE que ainda chegam sem dia após os fallbacks. Referências: `app/chamados/catalogo/perguntas.py:48`, `:56`, `:124`, `:132`; `app/chamados/services/disputa_emissao.py:338`, `:387`, `:411`.

O caminho demonstrável é data malformada: o formatador aceita o texto bruto, o parser não produz dia, falta batida válida e falta chamado datado. Antes abriria conversa; agora registra `info` e continua. Referências: `app/chamados/catalogo/query.py:62`, `:66`; `app/chamados/services/disputa_emissao.py:364`, `:379`, `:399`, `:410`, `:415`.

A conversa anterior **já nascia com `localdate()`**; o redatador encontrado só escreve `data_evento` quando a pergunta não tem data. Logo o caminho “nasce conversa, depois outro escritor põe o dia correto” não se sustentava nesse código. Referências: `app/chamados/models.py:2426`; `app/chamados/services/fio.py:835`, `:837`. Outros escritores efetivos encontrados por busca: nao achei além do nascimento e dessa porta; fluxo dinâmico não coberto pela busca: **[nao verificado]**.

Não some toda a acusação: a criação/agregação grava motivos e contexto **antes** de gerar perguntas, além da lavra de abertura da disputa. A recusa específica não gera registro de pergunta. Referências: `app/chamados/services/disputa_emissao.py:511`, `:532`, `:535`, `:557`, `:567`, `:640`, `:415`.

**Efeito novo concreto a registrar:** o chamador ignora a lista retornada e continua enviando push “Toque para responder” mesmo se o gerador recusou a única pergunta. Isso já era possível por outras recusas, mas a cura acrescenta este caso. Referências: `app/chamados/services/disputa_emissao.py:640`, `:641`, `:652`, `:656`, `:1575`, `:1580`. Ocorrência/impacto em produção: **[nao verificado]**.

**Chamadores diretos encontrados fora de testes:** [nao verificado]

| Chamador | Expectativa |
|---|---|
| `criar_ou_atualizar_disputa` | Chama no nascimento e na agregação; ignora retorno e retorna disputa, com push separado. `app/chamados/services/disputa_emissao.py:640`, `:652`, `:658`. |
| `perguntar_entradas_pendentes` | Calcula dia via batida/`_T1`, injeta contexto e soma lista retornada; só salva contexto se recebeu perguntas. `app/chamados/services/disputa_emissao.py:282`, `:292`, `:294`, `:295`. |
| Fábrica `perguntar` | Fornece dia explícito, chama criar/atualizar e chama gerador novamente; ignora lista, retorna número de carimbos desfeitos. `app/chamados/services/fabrica_da_celula.py:129`, `:139`, `:141`, `:142`. |
| Comando `smoke_chamados` | Fornece data válida quando template exige, espera exatamente uma pergunta por motivo e tipo igual ao catálogo. `app/chamados/management/commands/smoke_chamados.py:90`, `:96`, `:102`, `:104`, `:109`. |

O gerador documenta “lista das perguntas criadas”, mas adiciona `p` mesmo quando `_criada=False`, comportamento anterior ao delta. Isso explica que lista não é prova estrita de criação. Referências: `app/chamados/services/disputa_emissao.py:304`, `:423`, `:449`; `app/chamados/models.py:2497`.

**Mordida dos testes — inferência estática, execução Django/RED no pai: [nao verificado].** [nao verificado]

- `PerguntaDeHoraSemDiaDoTurnoTest.test_MORDE...`: **falharia sem a cura (a)**. A fixture fornece motivo `orfao_14h`, data malformada, sem vínculo; o caminho anterior abriria conversa com o dia congelado e violaria `assertFalse(...data_evento=localdate()).exists()`. Referências: `app/chamados/tests/test_plantio_noturno_mais1.py:198`, `:203`, `:205`, `:206`, `:207`; `app/chamados/catalogo/query.py:66`; `app/chamados/models.py:2426`; guarda nova `app/chamados/services/disputa_emissao.py:411`.
- `test_contra_exemplo_escolha_sem_dia_segue_conversa`: **passaria sem a cura**. O catálogo dá `escolha`, e o fallback conversa anterior é precisamente o comportamento exigido. Referências: `app/chamados/tests/test_plantio_noturno_mais1.py:210`, `:213`, `:214`; `app/chamados/catalogo/perguntas.py:82`, `:84`; `app/chamados/models.py:2426`.
- Caso congelado `test_a2...test_MORDE_batida_sem_linha_nao_inventa_dia_pelo_timestamp`: **passaria na árvore pai em dias locais diferentes de 12/08/2026**; o delta só fixa o relógio em outubro. Não é RED das curas (a)-(c), pois motivo é `escolha` e ausência da linha já levava à conversa. Sem congelamento, no dia 12/08/2026 os asserts `!=12/08` e `==localdate()` eram incompatíveis. Referências: `app/chamados/tests/test_a2_data_evento_batida.py:51`, `:55`, `:56`, `:59`, `:60`; `app/chamados/catalogo/perguntas.py:84`; `app/chamados/services/disputa_emissao.py:379`, `:383`, `:417`.

Limite do teste de hora: ele proíbe somente nascimento **na data de hoje**, não qualquer nascimento errado em outra data; é suficiente para matar precisamente o fallback antigo. Referências: `app/chamados/tests/test_plantio_noturno_mais1.py:207`, `:208`; `app/chamados/models.py:2426`.

## 06/11 20:03 · sessao 65cdc82cca83 · - · - · origem=Ronald (tmux)

- **Sem relógio solto/import morto novos no delta:** as alterações executáveis só capturam timestamps sem `sorted` e acrescentam o assert; todos os instantes do caso 3 são literais aware e o fuso é fixado pelo `override`. O delta não altera imports. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:243`, `:280`, `:281`, `:324`, `:325`, `:327`, `:333`; diff fornecido [nao verificado] quanto à integralidade Git)
- **A ordem exigida é sustentada pelo chamador do teste:** `_rodar` recebe explicitamente a lista `utc` nessa sequência e a porta usa compreensões que preservam a ordem recebida. O assert espera os sobreviventes `[utc[0], utc[1], utc[3]]`. Não depende da ordem de `set`/dict nem de empate do banco. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:325`, `:328`, `:333`; `app/ponto/services/fechamento.py:204`, `:253`, `:260`, `:262`)
- **Ressalva não bloqueante:** retirar `sorted` cura o mascaramento no gravador, mas este novo assert ainda não detecta uma ordenação cronológica introduzida na porta: os três sobreviventes já estão em ordem crescente. O elemento final fora de ordem é excluído pelo filtro. Portanto, “gravador não ordena” está curado; “teste prova preservação de ordem para entrada arbitrária” não está demonstrado. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:281`, `:325`, `:326`, `:333`; `app/ponto/services/fechamento.py:254`)
- **Censo de produção:** os três chamadores encontrados carregam listas por `batidas_apuraveis`, cujo retorno usa `order_by('timestamp')`. O motor deduplica sequencialmente pela batida anterior, justificando preservar a ordem. (`app/ponto/services/fechamento.py:633`, `:754`; `app/ponto/services/espelho.py:1022`, `:1164`; `app/relatorios/pdf_espelho.py:272`, `:419`; `app/ponto/turnos.py:2871`; `app/ponto/motor_calculo_v2.py:905`)
- **Naive não sustentado como bloqueante:** a nova porta chama `data_local`, documentado para aware; a origem estática dos três caminhos é ORM, com `DateTimeField` sem `null=True` e `USE_TZ=True`. Valores realmente devolvidos em produção: **[nao verificado]**. Nada no delta reabre essa interface. (`app/ponto/services/fechamento.py:203`, `:204`; `app/ponto/turnos.py:43`, `:46`; `app/ponto/models.py:23`; `app/config/settings/base.py:95`)
- **Comentário da ALLOWLIST agora confere:** a ofensa restante é `_com_batida = {timezone.localtime(b.timestamp).date() ...}`. (`app/ponto/tests/test_contract_no_batida_date.py:72`; `app/ponto/services/fechamento.py:793`)
- **Novo bloqueante identificado: nenhum dentro deste delta**, limitado às três alterações fornecidas; comparação integral do commit/base permanece **[nao verificado]**. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:281`, `:333`; `app/ponto/tests/test_contract_no_batida_date.py:72`)

## 06/11 20:02 · sessao 99c2d369a999 · H12H06-B · sonda · origem=Code

Não executei a sonda: o invólucro cria arquivos e grava log, contrariando a restrição de somente leitura (`bin/sonda_leitura.sh:216`, `bin/sonda_leitura.sh:217`, `bin/sonda_leitura.sh:250`). Segue a transcrição integral do dado fornecido, sem nova execução: [nao verificado]

```text
sonda_leitura: pre.py [f35cf407c2faf784] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

HASH_FM {"05": [615, "e29ff62650eebac5"], "06": [704, "9400c1a360842d79"], "07": [629, "2edced101a39a6c3"], "08": [618, "22459b830139a009"], "09": [607, "6e69a4d396e9f407"], "10": [624, "1cc01147a16e856b"], "11": [579, "d5a3a67bf065738d"], "12": [87, "afb9cb5ee9e2741c"]}
EXP {"pk": 2, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "80f417a64296887d8c1f58d580947e40aae78af2c21945c14485c0a96f9814c7", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 3, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "98edd7f6e5d72d68a427efcd40cf83e08814313ab7d10d8c858acea3711ebff2", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 4, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "98edd7f6e5d72d68a427efcd40cf83e08814313ab7d10d8c858acea3711ebff2", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 5, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "98edd7f6e5d72d68a427efcd40cf83e08814313ab7d10d8c858acea3711ebff2", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 6, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "98edd7f6e5d72d68a427efcd40cf83e08814313ab7d10d8c858acea3711ebff2", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 7, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "98edd7f6e5d72d68a427efcd40cf83e08814313ab7d10d8c858acea3711ebff2", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 8, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "71570435dbbe5246f59b19986230ad265eece33da6015177bf5b806b75d45e6b", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 9, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "1f2b81f241a8f8cd344a4cd211911f20ab4cdd4ae25d65b416072bff7df2ac02", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 10, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "3c51aae49a29ab84f6b2c21885f796beca6038b483394138c44715743fa95a18", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 11, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "3c51aae49a29ab84f6b2c21885f796beca6038b483394138c44715743fa95a18", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 12, "empresa_id": 4, "mes": 7, "ano": 2026, "hash_sha256": "68d299ff806aceb36e414b75fd18e6ea9043d556b351645072448cf024af010c", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 13, "empresa_id": 4, "mes": 7, "ano": 2026, "hash_sha256": "491f6fff352cc5102cd0d0a5f24f004edc433e792d114563a4e77b529a216c2f", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 14, "empresa_id": 4, "mes": 7, "ano": 2026, "hash_sha256": "491f6fff352cc5102cd0d0a5f24f004edc433e792d114563a4e77b529a216c2f", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 15, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "1897e08736e104ef3d9eb81917bb26c2849f60a21e6f3141bfc2b35acdc08f63", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 16, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "1897e08736e104ef3d9eb81917bb26c2849f60a21e6f3141bfc2b35acdc08f63", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 17, "empresa_id": 3, "mes": 7, "ano": 2026, "hash_sha256": "7d54b0e6602bf4ea8bf9615009cb4e470398f96a9e66b8623dfc03ec4874524e", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 18, "empresa_id": 2, "mes": 8, "ano": 2026, "hash_sha256": "790d8cd7ea0c631c1b76cb462bc9ebadc163787ffd7e854b1d5b9ea0b0d73f01", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 19, "empresa_id": 2, "mes": 8, "ano": 2026, "hash_sha256": "2b3ac743a5782a5122fc89fb917405d9e9529d141f8bb6d81e0c61610b043dd0", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 20, "empresa_id": 2, "mes": 9, "ano": 2026, "hash_sha256": "8a63e3baabc501af7d63aa3845c6a9221d7f18ac091b5f7038650a23aaa59dbc", "invalidada_em": "2026-09-30 20:07:06.058663+00:00", "substituida_por_id": 23}
EXP {"pk": 21, "empresa_id": 3, "mes": 9, "ano": 2026, "hash_sha256": "6e0f1048428293078bc82c21aa8756630446c394ef2c2a5b16cbb1efbb6626d8", "invalidada_em": "2026-09-30 20:07:07.142119+00:00", "substituida_por_id": 24}
EXP {"pk": 22, "empresa_id": 4, "mes": 9, "ano": 2026, "hash_sha256": "cb7f00fe59f84e505fce92aab2769242aec6671b688d7c514311518345c29773", "invalidada_em": "2026-09-30 20:07:07.385295+00:00", "substituida_por_id": 25}
EXP {"pk": 23, "empresa_id": 2, "mes": 9, "ano": 2026, "hash_sha256": "0e3e008b49b1ff73a89556a24e495f0e05f566cad644d8354f224300bbc5a703", "invalidada_em": "2026-10-01 14:57:23.407593+00:00", "substituida_por_id": 26}
EXP {"pk": 24, "empresa_id": 3, "mes": 9, "ano": 2026, "hash_sha256": "5c503b95f9f9cd35541db29a45a6447f026e272b9eedf27b346cda26b92810a6", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 25, "empresa_id": 4, "mes": 9, "ano": 2026, "hash_sha256": "84c78cd0871f5f5251db047a6d12054930f91c05a39ec46d6669fddc1d97a73b", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 26, "empresa_id": 2, "mes": 9, "ano": 2026, "hash_sha256": "0ae5da67364cabfb5dcadde9481ec75d63b702ca95956627adbbb37079664d53", "invalidada_em": "2026-10-01 22:44:56.762182+00:00", "substituida_por_id": 27}
EXP {"pk": 27, "empresa_id": 2, "mes": 9, "ano": 2026, "hash_sha256": "361d0f9685f86d3a51d49b8f5194df469db8e4818c9cae0e9bd9f1461f9af0cc", "invalidada_em": null, "substituida_por_id": null}
AUS5198 {"pk": 5198, "data": "2026-10-24", "minutos": 420, "tipo": "falta"}
REG_PEND {"n": 0, "itens": []}
REG_PEND_FROTA 2
DISP_ABERTAS {"n": 23}
HASH_DP 9 11501 54c6419c61520d6a
HASH_DP 10 21418 9d68332a36ef63eb
ERROS {}
```

REG_PEND n=0, itens=[]; DISP_ABERTAS n=23. [nao verificado]  
HASH_FM: 09=[607,"6e69a4d396e9f407"]; 10=[624,"1cc01147a16e856b"]. [nao verificado]  
HASH_DP: 9=11501/54c6419c61520d6a; 10=21418/9d68332a36ef63eb. [nao verificado]  
AUS5198: 420 minutos. [nao verificado]  
ERROS={}; nenhuma exceção aparece no dado fornecido; ocorrência fora dele: [nao verificado].

## 06/11 20:01 · sessao 1988589b9388 · - · - · origem=Ronald (tmux)

(a) Fecha o nascimento indevido de conversa `hora`: a guarda recusa apenas quando as duas fontes finais de dia são `None`; antes, esse caminho caía em `abrir_conversa`, que grava `localdate()`. Referências: `app/chamados/services/disputa_emissao.py:410`, `app/chamados/services/disputa_emissao.py:411`, `app/chamados/services/disputa_emissao.py:417`, `app/chamados/models.py:2426`.

**Caminho legítimo “nasce conversa hora e recebe dia depois”: nao achei.** A conversa já nasce com data preenchida; o escritor posterior `reapontar_resolvedora` só atribui data quando a pergunta está sem data. Referências: `app/chamados/models.py:2426`, `app/chamados/services/fio.py:835`, `app/chamados/services/fio.py:837`.

Os quatro motivos `hora` são `orfao_14h`, `saida_sem_entrada`, `intervalo_saida` e `intervalo_volta`. Os intervalos sem dia já retornam antes da nova guarda; seu fluxo não era o fallback de conversa. Referências: `app/chamados/catalogo/perguntas.py:48`, `app/chamados/catalogo/perguntas.py:56`, `app/chamados/catalogo/perguntas.py:124`, `app/chamados/catalogo/perguntas.py:132`, `app/chamados/services/disputa_emissao.py:387`.

**Ressalva concreta:** o novo caminho pode deixar disputa sem pergunta e enviar push mandando responder: o chamador ignora a lista retornada e envia push após gerar; a mensagem promete “Toque para responder”. Uma disputa sem perguntas também trava o encerramento do chamado. Referências: `app/chamados/services/disputa_emissao.py:640`, `app/chamados/services/disputa_emissao.py:641`, `app/chamados/services/disputa_emissao.py:652`, `app/chamados/services/disputa_emissao.py:656`, `app/chamados/services/disputa_emissao.py:1575`, `app/chamados/juizes.py:2084`.

**Não desaparece todo rastro além do log:** motivos/contexto já foram persistidos na disputa antes da tentativa de geração. O ramo novo, porém, não persiste um ato de recusa da pergunta: apenas `logger.info` e `continue`. Uma chamada posterior com contexto corrigido tenta gerar novamente, mesmo com motivo já existente. Referências: `app/chamados/services/disputa_emissao.py:557`, `app/chamados/services/disputa_emissao.py:511`, `app/chamados/services/disputa_emissao.py:529`, `app/chamados/services/disputa_emissao.py:415`, `app/chamados/services/disputa_emissao.py:652`.

Chamadores diretos encontrados fora dos testes: [nao verificado]

| Chamador | Expectativa observável |
|---|---|
| `perguntar_entradas_pendentes` | Injeta dia obtido por `_T1`, acumula a lista retornada e salva contexto somente se a lista não estiver vazia. `app/chamados/services/disputa_emissao.py:282`, `app/chamados/services/disputa_emissao.py:292`, `app/chamados/services/disputa_emissao.py:294`, `app/chamados/services/disputa_emissao.py:295`. |
| `criar_ou_atualizar_disputa`, nascimento e atualização | Ignora lista, envia push por nascimento/motivo novo e retorna disputa. `app/chamados/services/disputa_emissao.py:640`, `app/chamados/services/disputa_emissao.py:642`, `app/chamados/services/disputa_emissao.py:652`, `app/chamados/services/disputa_emissao.py:656`, `app/chamados/services/disputa_emissao.py:658`. |
| `fabrica_da_celula.perguntar` | Fornece dia explícito, ignora a lista e retorna quantidade de carimbos desfeitos. `app/chamados/services/fabrica_da_celula.py:139`, `app/chamados/services/fabrica_da_celula.py:141`, `app/chamados/services/fabrica_da_celula.py:142`. |
| `smoke_chamados._bloco_a_motivos` | Fornece data válida para templates datados e exige exatamente uma pergunta por motivo. `app/chamados/management/commands/smoke_chamados.py:96`, `app/chamados/management/commands/smoke_chamados.py:102`, `app/chamados/management/commands/smoke_chamados.py:104`. |

`models.py` apenas reexporta a função; a declaração de hora usa sua própria porta e documenta que não chama o gerador. Referências: `app/chamados/models.py:3350`, `app/chamados/services/disputa_emissao.py:585`.

**Testes, por análise contrafactual do diff; execução no pai [nao verificado]:** [nao verificado]

- `PerguntaDeHoraSemDiaDoTurnoTest.test_MORDE_orfao_sem_dia_nao_nasce_como_conversa_de_hoje` **falha no pai**: data malformada produz texto, o parser de dia falha, não há batida nem chamado para fornecer outra data, e o antigo fallback cria conversa em `localdate()`, contrariando `assertFalse`. Referências: `app/chamados/tests/test_plantio_noturno_mais1.py:203`, `app/chamados/tests/test_plantio_noturno_mais1.py:206`, `app/chamados/tests/test_plantio_noturno_mais1.py:207`, `app/chamados/catalogo/query.py:62`, `app/chamados/services/disputa_emissao.py:364`, `app/chamados/services/disputa_emissao.py:238`, `app/chamados/models.py:2426`.
- Seu contraexemplo `escolha` **passa também no pai**: esse tipo não é atingido pela nova guarda. Referências: `app/chamados/tests/test_plantio_noturno_mais1.py:210`, `app/chamados/catalogo/perguntas.py:82`, `app/chamados/services/disputa_emissao.py:411`.
- O caso congelado de `test_a2` **passaria no pai com o mesmo congelamento transplantado**: usa motivo `escolha`, cujo comportamento não mudou. Sem congelamento, passaria nos dias diferentes de 12/08/2026; em 12/08, os dois asserts são incompatíveis. Congelar cura instabilidade do teste, não produz RED de implementação. Referências: `app/chamados/tests/test_a2_data_evento_batida.py:51`, `app/chamados/tests/test_a2_data_evento_batida.py:57`, `app/chamados/tests/test_a2_data_evento_batida.py:59`, `app/chamados/tests/test_a2_data_evento_batida.py:60`, `app/chamados/catalogo/perguntas.py:84`.

O teste novo de `hora` rejeita somente pergunta com data de hoje; não demonstra ausência de pergunta em qualquer outra data. Referência: `app/chamados/tests/test_plantio_noturno_mais1.py:207`.

**Escritores encontrados de `PerguntaDisputa.data_evento`:** nascimento real em `_nascer`; atribuição posterior e desfazimento em memória em `reapontar_resolvedora`; `_Proxy.data_evento` é insumo transitório de validação. Outro escritor real: **nao achei**. Referências: `app/chamados/models.py:2498`, `app/chamados/services/fio.py:837`, `app/chamados/services/fio.py:846`, `app/chamados/models.py:2444`.

**ESMERIL:** permanece a divergência já declarada fora do escopo: conversa grava dia da pergunta, enquanto `do_fato` e fábrica usam a mesma data como identidade de fato/célula, sem discriminador. A guarda nova elimina a conversa `hora` deste emissor, mas não resolve essa colisão geral. Referências: `app/chamados/models.py:2422`, `app/chamados/models.py:2396`, `app/chamados/models.py:2453`, `app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/services/disputa_emissao.py:411`.

## 06/11 20:01 · sessao 0ca929e8dcf8 · - · - · origem=Ronald (tmux)

**(c) fecha a omissão para batidas novas; não transforma `criado_em` em data da última materialização.** O filtro agora usa nascimento da linha, enquanto a comparação continua pelo juiz `_T1`. Referências: `app/ponto/management/commands/vigia_plantio_overnight.py:76`, `app/ponto/management/commands/vigia_plantio_overnight.py:87`.

- **Escritor normal:** `Batida.criado_em` é `auto_now_add=True`; as criações passam por `Batida.objects.create`. A porta de anotação não permite alterar esse campo. Referências: `app/ponto/models.py:79`, `app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:157`, `app/ponto/registro_batida.py:203`, `app/ponto/registro_batida.py:241`.
- **Reescritor dedicado do campo em produção: nao achei.** A restauração de tenant usa `pg_restore`; a sombra incremental copia `ponto_batida` com `SELECT *` e repõe as colunas por `COPY`. Por essas operações, o valor é preservado do registro original, não recalculado pela hora da cópia — inferência do SQL. Referências: `scripts/restore_tenant.sh:8`, `scripts/backup_tenant.sh:10`, `bin/sombra_incremental.sh:34`, `bin/sombra_incremental.sh:57`, `bin/sombra_incremental.sh:62`. Conteúdo real dos dumps e restaurações executadas: **[nao verificado]**.
- **Plantio depois de reabertura:** quando cria outra linha, recebe o nascimento novo; quando reaproveita linha existente, conserva o nascimento antigo. O materializador primeiro busca batida no marco e somente cria se não encontrou; depois pode apenas trocar `pergunta_origem`. Referências: `app/chamados/services/materializacao.py:835`, `app/chamados/services/materializacao.py:847`, `app/chamados/services/materializacao.py:903`, `app/chamados/services/materializacao.py:922`, `app/chamados/services/materializacao.py:924`.
- **Outro reaproveitamento:** `registrar_batida` devolve linha existente pelo mesmo tipo na janela ou pela mesma chave colaborador/instante, inclusive retratada; não altera `criado_em`. Referências: `app/ponto/registro_batida.py:113`, `app/ponto/registro_batida.py:120`.
- **Restauração de retratação:** a porta `desfazer` só grava `aprovada=True` e `retratada_em=None`; conserva `criado_em`. Referência: `app/ponto/services/retratar_humano.py:207`.

**Limite semântico:** “plantada recentemente” coincide com `criado_em` para criação nova; não coincide com reaproveitamento/revinculação recente de linha antiga. Assim, uma linha antiga de `origem='disputa_s84_retro'` vinculada hoje a outra pergunta continua fora da janela por criação. Referências: `app/ponto/management/commands/vigia_plantio_overnight.py:67`, `app/ponto/management/commands/vigia_plantio_overnight.py:73`, `app/ponto/management/commands/vigia_plantio_overnight.py:76`, `app/chamados/services/materializacao.py:924`. Isso não demonstra nova regressão: se seu `timestamp` também era antigo, o filtro do pai já a omitia — comparação com o delta fornecido; execução no pai **[nao verificado]**.

**Teste do vigia: morde por análise contrafactual.** Cria a linha em 01/12, com instante em 23/10; na janela de 30 dias, o filtro anterior por `timestamp` exclui a única candidata e o resultado seria `[]`, contrariando `[b.pk]`. O teste cobre criação nova, não reaproveitamento. Referências: `app/chamados/tests/test_plantio_noturno_mais1.py:30`, `app/chamados/tests/test_plantio_noturno_mais1.py:32`, `app/chamados/tests/test_plantio_noturno_mais1.py:192`, `app/chamados/tests/test_plantio_noturno_mais1.py:193`, `app/chamados/tests/test_plantio_noturno_mais1.py:195`. RED executado: **[nao verificado]**.

**Demais leitores de `Batida.criado_em` encontrados:** o pareador usa chegada para distinguir eco de flush; o contador de sequência seleciona batidas criadas desde um marco; o cache de veredito usa o máximo de criação e trata retratação separadamente. Esses usos conservam o significado de criação/chegada. Referências: `app/ponto/turnos.py:355`, `app/ponto/turnos.py:366`, `app/ponto/turnos.py:368`, `app/ponto/turnos.py:2384`, `app/chamados/veredito_lavrado.py:137`, `app/chamados/veredito_lavrado.py:140`.

_2513 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
