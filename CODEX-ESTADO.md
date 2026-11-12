# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 10:59** · publicado em **12/11/2026 10:59** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 10:59 · sessao 327b56f5872e · PUSH-SEM-REGRAVAR · verificador · origem=Code

1. **ok — a regra (c) saiu inteira do julgamento.** Cada commit é julgado pela própria mensagem e arquivos; depois disso, só a exceção nominal pode apagar sua falha. Não restaram `primeira_linha`, `pouso_de`, `julgar_pouso` ou `POR_POUSO` no selo. O tratamento de **hunk próprio do merge** permanece, mas não dispensa os commits dos pais ([test_lei_akita.sh:438–467](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/tests/test_lei_akita.sh:438)).

2. **ok — exceção restrita ao SHA inteiro solicitado.** A lista contém somente `c2162edda843b4adea87885762cdfc451c01167a`, com o motivo literal do aval ([test_lei_akita.sh:369–372](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/tests/test_lei_akita.sh:369)). A condição exige simultaneamente falha e pertinência exata: `if d and h in EXCECAO_NOMINAL`. Não compara prefixos; o abreviamento acontece apenas no registro da saída. Outro SHA, inclusive merge, não entra. Duplicatas saem antes desse ramo, como duplicatas ([test_lei_akita.sh:442–464](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/tests/test_lei_akita.sh:442)).

3. **ok funcional; FILA na cobertura da integração.** A catraca rejeita dois itens, outro SHA, zero itens e motivo vazio ou composto de espaços ([test_lei_akita.sh:373–378](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/tests/test_lei_akita.sh:373)). Roda **antes de chamar o enumerador**, embora depois da leitura do vocabulário ([test_lei_akita.sh:129](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/tests/test_lei_akita.sh:129), [test_lei_akita.sh:932–935](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/tests/test_lei_akita.sh:932)).

   O erro interrompe o Python sem `PADRAO_FIM`; o shell chama `_falha` e termina com **rc 1**, não apenas impressão. Conferi essa ligação isoladamente, em memória: os cinco casos inválidos deram rc 1; a lista real deu rc 0 ([test_lei_akita.sh:26](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/tests/test_lei_akita.sh:26), [test_lei_akita.sh:943–961](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/tests/test_lei_akita.sh:943)).

   **FILA:** o MORDE prova os quatro retornos de rejeição, mas não testa retirar a chamada produtiva da catraca. Esse mutante não seria acusado pelos casos novos ([test_lei_akita.sh:913–918](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/tests/test_lei_akita.sh:913), [test_lei_akita.sh:932–934](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/tests/test_lei_akita.sh:932)).

4. **ok por inspeção para restaurar integralmente (c); FILA para volta+ff literal.** O caso `juiz unico` cria passo inválido na raia e merge válido, mas exige que o passo continue vermelho: restaurar (c) retiraria esse passo das falhas e dispararia `MORDE_FALHOU` ([test_lei_akita.sh:896–911](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/tests/test_lei_akita.sh:896)).

   Os casos de merge inventado e `n/a` são **vácuos para matar essa restauração**, pois o ramo antigo já não dispensava nesses cenários; a condição antiga está apenas no anexo fornecido **[nao verificado]**. O teste novo constrói pouso `raia→base` com `--no-ff`, não `main→raia` seguido de ff ([test_lei_akita.sh:904–907](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/tests/test_lei_akita.sh:904)). Execução integral do mutante: **[nao verificado]**.

5. **ok — escritores e leitores identificados.**

   | Valor | Quem escreve | Quem lê |
   |---|---|---|
   | `EXCECAO_NOMINAL` | `bin/tests/test_lei_akita.sh:369–371` | `:462–463`, `:914–915`, `:932` |
   | `EXCECAO_CATRACA` | `bin/tests/test_lei_akita.sh:372` | `:373–375`, `:913` |
   | `POR_EXCECAO` | `bin/tests/test_lei_akita.sh:399`, `:463` | `:909–911`, `:940–941` |
   | `PADRAO_EXCECAO` | `bin/tests/test_lei_akita.sh:941` | apresentação `:949`; contagem no OK `:958` |
   | Frase final de OK | `bin/tests/test_lei_akita.sh:958` | chamadas externas abertas usam rc: `bin/pre-push.sh:38`, `bin/regua.sh:140–143` |

   No censo literal da cópia, não apareceu leitor órfão de `PADRAO_POUSO`; “pelo pouso” fora do selo aparece como comentário de outra obra ([test_esmeril_celula_censo.sh:10](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/tests/test_esmeril_celula_censo.sh:10)). O pre-push chama o selo antes do atalho do arsenal ([pre-push.sh:38–43](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/pre-push.sh:38)). Hook efetivamente instalado e leitores dinâmicos: **[nao verificado]**.

6. **FILA — mordida incompleta da ligação produtiva da catraca**, conforme item 3 ([test_lei_akita.sh:913–918](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/tests/test_lei_akita.sh:913)). **ok no delta** quanto a derivação paralela: produção e MORDE chamam a mesma função; não há segundo predicado ([test_lei_akita.sh:914–918](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/tests/test_lei_akita.sh:914), [test_lei_akita.sh:932](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/tests/test_lei_akita.sh:932)). Não identifiquei campo com dois significados nem leitura de relógio nos trechos novos ([test_lei_akita.sh:369–378](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/tests/test_lei_akita.sh:369), [test_lei_akita.sh:940–958](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/tests/test_lei_akita.sh:940)).

**Pouso: [nao verificado].** Resultado v6 com **0 FALHA no main real**, selo completo, MORDE completo e identidade Git do hash informado: **[nao verificado]**. [nao verificado]

## 12/11 10:57 · sessao 395d7244505f · - · - · origem=Ronald (tmux)

TELA é `app/ponto/views.py::espelho` → `services.espelho.espelho_do_colab` (`app/ponto/views.py:157-159`), depois `montar_semanas` (`app/ponto/views.py:183-184`), renderizando `ponto/espelho.html` (`app/ponto/views.py:192-197`).

| Entrada | Juiz consultado | Operação | Persistido | Retorno UI | Prova existente |
|---|---|---|---|---|---|
| Dia normal de trabalho, faixa reconhecida, ata/status lavrado | `grade_da_celula` (`app/ponto/services/espelho.py:522`); `fatos_do_periodo` (`app/ponto/services/espelho.py:653`); D4 `status_dos_dias_lavrados` (`app/ponto/services/espelho.py:170`) | Transporta ata/marcos; monta previsto e realizado (`app/ponto/services/espelho.py:693-699`); aplica palavra (`app/ponto/services/espelho.py:1301`); lê status/rotulo (`app/ponto/services/espelho.py:174`) | Nada nessas operações; muta dicionários em memória (`app/ponto/services/espelho.py:773`, `app/ponto/services/espelho.py:340`, `app/ponto/services/espelho.py:174`) | Faixa usa status e `status_rotulo` no title (`app/templates/ponto/espelho.html:151`); cartões mostram previsto/marcações (`app/templates/ponto/espelho.html:218-246`) | `app/ponto/tests/test_caracterizacao_espelho.py::test_contrato_context` confere HTTP 200, D4, turnos e batidas (`:53-55`, `:84-90`); `app/ponto/tests/test_e4_esp_espelho_le_o_d4.py::test_RED_testemunha_le_status_e_rotulo_do_D4_em_todo_dia` (`:64-79`). |
| Ausência cujo tipo não tem faixa; dia passado sem batida e sem status lavrado | A2 `fatos_do_periodo(...recusas=...)` (`app/ponto/services/espelho.py:653-654`); D4 (`app/ponto/status_dia.py:311-318`) | Transporta `erro.rotulo` para `_recusa_do_dia` (`app/ponto/services/espelho.py:880`); aplica `veredito_dia=''`, `palavra_dia=rotulo`, cor vermelha (`app/ponto/services/espelho.py:339-341`); D4 devolve `sem_certidao` com rótulo da recusa (`app/ponto/status_dia.py:317-318`) | Nada; alterações de dicionário (`app/ponto/services/espelho.py:340`, `app/ponto/status_dia.py:318`) | O texto **não é impresso como `palavra_dia` pelo template ponto/espelho**; chega no `title="{{ d.status_rotulo }}"` da faixa (`app/templates/ponto/espelho.html:151`). Com ausência, card mostra chip próprio de ausência (`app/templates/ponto/espelho.html:191`) | `app/ponto/tests/test_lastro_c10_sem_banco.py::test_C_RED_rotulo_vem_da_recusa_por_dia_sem_concluir_precedencia` (`:269-279`), recorte AST (`:236-267`); `app/ponto/tests/test_lastro_c11_sem_banco.py::test_RED_espelho_montar_semanas_dia_passado_sem_batida_sem_lavra` (`:123-136`). Não provam o handler UI completo. |
| Mesmo tipo sem faixa, **com batida**, ou com status antigo lavrado | Mesmo A2 da palavra (`app/ponto/services/espelho.py:653-654`), mas D4 passa por retornos anteriores (`app/ponto/status_dia.py:283-298`) | `palavra_dia` pode receber recusa; D4 **não consulta recusas** se turno vivo, andamento, status lavrado ou batida sem lavra (`app/ponto/status_dia.py:283-298`) | Nada nessas operações (`app/ponto/services/espelho.py:340`, `app/ponto/status_dia.py:284-297`) | Turno vivo: `aberto`; status lavrado: preserva status/rotulo; batida encerrada sem lavra: `sem_certidao` genérico (`app/ponto/status_dia.py:283-298`). Template lê esse `status_rotulo`, não `palavra_dia` (`app/templates/ponto/espelho.html:151`) | Nenhuma prova específica sem-faixa+batida aberta. Teste geral status lavrado com batida: `app/ponto/tests/test_e4_esp_espelho_le_o_d4.py::test_RED_celula_lavrada_irregular_com_batida_na_tela_e_irregular` (`:46-54`). |
| Batida órfã na competência, com célula/ata | `grade_da_celula` (`app/ponto/services/espelho.py:522`), `_orfas_da_ata` (`app/escala/services/leitor_celula.py:180`), `montar_realizado_grade` (`app/ponto/services/espelho.py:698`) | Leitor transporta `ata.orfas` ou `['?']*n_orfas` (`app/escala/services/leitor_celula.py:187-190`, `:420`); adaptador devolve cópia de `gd.orfas` (`app/escala/utils.py:1113`); escreve `orfas_grade` no dict (`app/ponto/services/espelho.py:853`) | Nada; `orfas_grade` é memória (`app/ponto/services/espelho.py:853`) | Se `realizado_grade` tiver conteúdo, órfãs aparecem como `• HH:MM` (`app/templates/ponto/espelho.html:231-242`); sem realizado, imprime batidas cruas (`:243-246`) | Transporte da ata: `app/escala/tests/test_t41_leitor_celula.py::test_orfas_chegam_com_hora_real_nao_interrogacao` (`:47-71`). Prova ponta a ponta UI órfã: nenhuma entre os arquivos abertos. |
| Batida órfã sem célula/marcos | `montar_realizado_grade` (`app/escala/utils.py:1081`) | Guarda `if not gd or not marcos` devolve `[],[]` (`app/escala/utils.py:1087-1088`); TELA grava `orfas_grade=[]` em memória (`app/ponto/services/espelho.py:698`, `:853`) | Nada | Batidas cruas, sem marcação de órfã nesse template (`app/templates/ponto/espelho.html:243-246`) | `app/ponto/tests/test_lastro_c11_sem_banco.py::test_PDF_igual_tela_orfa_sem_celula_dia_valido_coletor_inteiro` (`:380-389`): construção chama atribuição real da tela por AST (`:306-319`), mas `espelho_do_colab` é mock (`:325-326`). |
| Dia relavrado `ausencia→folga`, com batida | `grade_da_celula` lê `ata.tipo_dia` (`app/escala/services/leitor_celula.py:397-398`); A2 separado (`app/ponto/services/espelho.py:653`); D4 separado (`app/ponto/services/espelho.py:170`) | Define `dia.folga` pelo `gd.tipo_dia=='folga'` (`app/ponto/services/espelho.py:688`, `:779`); mantém batidas (`:775`); transporta cobertura atual para palavra (`:882`), que tem precedência sobre folga no tradutor (`app/ponto/services/dia_decidido.py:363-372`) | Nada no leitor; lavra é anterior | Card pode mostrar badge `Folga` (`app/templates/ponto/espelho.html:171`) e batidas (`:231-246`); faixa lê D4, podendo conservar status lavrado (`app/ponto/status_dia.py:290-293`). Não deduzir “Folga” na palavra só por `ata.tipo_dia`: ela usa A2/cobertura (`app/ponto/services/espelho.py:879-882`, `app/ponto/services/dia_decidido.py:363-372`) | Nenhuma prova aberta da sequência real relavra+UI; teste D4 geral inclui folga na janela, mas não relavra (`app/ponto/tests/test_e4_esp_espelho_le_o_d4.py:72-79`). |

Fontes e escritores dos campos:

- `ata.tipo_dia`, `ata.orfas`, `ata.n_orfas`: compositor `ata_do_dia` escreve no dict (`app/ponto/services/cartorio.py:729-736`); leitor transporta tipo e órfãs (`app/escala/services/leitor_celula.py:397-420`).
- `ata.ausencia_tipo/id/label`: compositor copia da grade (`app/ponto/services/cartorio.py:763-768`); leitor copia para grade lida (`app/escala/services/leitor_celula.py:436-438`).
- `orfas_grade`, `realizado_grade`, `folga`: montador TELA escreve (`app/ponto/services/espelho.py:779-781`, `:853`); template lê (`app/templates/ponto/espelho.html:171`, `:231-242`).
- `_recusa_do_dia`: montador escreve de A2 (`app/ponto/services/espelho.py:880`); `aplicar_palavra_do_dia` lê e escreve palavra/veredito/cor (`app/ponto/services/espelho.py:339-341`).
- `status/status_rotulo`: `montar_semanas` escreve da saída D4 (`app/ponto/services/espelho.py:170-174`); faixa UI lê (`app/templates/ponto/espelho.html:151`). Escritor persistente de `CelulaDia.status_dia/status_rotulo` não aberto por este explorer [nao verificado].

Nuance essencial dos três sítios: `TipoSemFaixa` no caminho normal/pre-apuração dentro do `try` pode cair em `except Exception`, zerar grade e seguir (`app/ponto/services/espelho.py:533-574`), alcançando posteriormente A2+recusa (`:653-654`) e `palavra_dia` (`:880`, `:339-341`, `:1301`). Portanto “rótulo não chega” nesses dois sítios é demonstrável para **a grade daquele caminho**, mas não autoriza afirmar que o dicionário final jamais recebe palavra. No fallback `AtaAgregadaSemMarcos`, a chamada a `_pela_grade` dentro do handler (`:563`) pode propagar, sem alcançar a montagem posterior; handlers irmãos não cobrem exceção levantada dentro desse handler (`:540-574`).

Nos testes c10-c13 abertos, não existe execução desses três caminhos inteiros: o teste de palavra executa somente bloco AST posterior à grade (`app/ponto/tests/test_lastro_c10_sem_banco.py:236-267`); o teste c13 de recusa chama `_marcar_ausencias` diretamente (`app/ponto/tests/test_lastro_c13_sem_banco.py:110-128`).

## 12/11 10:57 · sessao 3f3e9341170f · - · - · origem=Ronald (tmux)

**ok funcional; FILA de cobertura.**

- **Catraca efetiva:** `catraca_excecao` rejeita 2 itens, outro SHA, zero itens e motivo vazio (`bin/tests/test_lei_akita.sh:373-378`). Confirmei esses retornos executando apenas a função extraída em memória. O fluxo real chama a catraca **antes de enumerar** (`:932-935`; enumeração em `:422`). O erro interrompe Python antes de `PADRAO_FIM` (`:934`, `:943`); o shell registra `_falha` por marcador ausente (`:950`), incrementa `falhas` (`:26`) e termina com **rc 1** (`:957-961`).

- **MORDE da catraca:** cobre os quatro argumentos inválidos e a lista real, chamando a mesma função (`:913-918`). **FILA:** testa somente o retorno da função; remover a chamada produtiva de `:932-934` não é detectado por esses casos. Portanto não há mordida específica da integração catraca→rc do selo (`:913-918`, `:932-935`).

- **Mutante que devolve (c):** por leitura, o caso com `padrao_merge='juiz unico'` mata a restauração literal da regra antiga: cria `passo` inválido fora da primeira linha (`:901-903`), cria `fp` inválido no tronco (`:904-906`), pousa com merge válido (`:907`) e exige ambos vermelhos (`:908-911`). O antigo ramo dispensaria `passo`; isso imprime `MORDE_FALHOU`, rejeitado pelo shell e convertido em rc 1 (`:954-955`, `:961`). Execução do mutante real: **[nao verificado]**, pois o MORDE cria arquivos (`:714-725`).

- **Vacuidade:** os casos de merge com termo inventado e `n/a` (`:896`) **não matam o mutante (c)**: a regra antiga já recusava dispensar o passo nesses cenários — conforme delta v5 fornecido **[nao verificado]**. Continuam controles úteis da permanência das falhas (`:909`).

- **Volta+ff literal:** **FILA:** o novo teste chamado `volta/pouso` constrói pouso `raia→base` com `--no-ff`, após voltar ao tronco (`:904-907`); não constrói `main→raia` seguido de ff do main. A mordida da restauração integral de (c) vem do pouso válido (`:896-911`); regressão específica de volta+ff não está demonstrada nesse bloco. [nao verificado]

## 12/11 10:57 · sessao 926af4d1aeec · - · - · origem=Ronald (tmux)

- **ok — leitores externos por rc.** `bin/pre-push.sh:38` chama o selo com `LEI_AKITA_HEAD="$PUSH_SHA"` e aborta com rc 1; essa chamada antecede o atalho de arsenal (`bin/pre-push.sh:43`). `bin/regua.sh:138` enumera os selos, captura saída e aborta pelo rc (`bin/regua.sh:140–143`). A régua tem atalho anterior que pode pular a pasta (`bin/regua.sh:84–94`); o pre-push continua chamando diretamente o selo (`bin/pre-push.sh:38`).

- **ok — escritores/leitores internos.** `EXCECAO_NOMINAL` nasce em `bin/tests/test_lei_akita.sh:369–371`; é lida na pertinência e no motivo (`:462–463`), no MORDE (`:914–915`) e na catraca real (`:932`). `EXCECAO_CATRACA` nasce em `:372`, alimenta o argumento padrão da catraca (`:373`) e o SHA dos casos MORDE (`:913`). `POR_EXCECAO` é declarada global e zerada em `:398–399`, recebe apenas falha nominal em `:462–464`, é lida pelo MORDE em `:909–911` e pela emissão em `:940–941`.

- **ok — emissão e apresentação.** O Python escreve `PADRAO_EXCECAO` com SHA abreviado apenas na saída e motivo em `bin/tests/test_lei_akita.sh:941`; o Bash lê e apresenta em `:949`, e conta essas linhas na frase final em `:958`. A frase final termina com quantidade de exceções e `catraca 1` (`:958`); nenhum leitor de frase foi identificado nas chamadas abertas (`bin/pre-push.sh:38`; `bin/regua.sh:140–143`).

- **ok no censo literal da cópia — sem leitor órfão identificado.** A busca por `PADRAO_POUSO`, `POR_POUSO`, `pouso_de`, `julgar_pouso` e `primeira_linha` em `bin`, hooks e pastas ocultas não encontrou referências. Não há referência externa aos novos nomes no censo literal. A ocorrência externa de “pelo pouso” é comentário sobre outra obra (`bin/tests/test_esmeril_celula_censo.sh:10`). Leitores construídos dinamicamente ou instalação real: **[nao verificado]**.

- **FILA/limite de instalação.** O instalador define fonte/instalação em `bin/hooks.sh:9–11` e gera wrapper que faz `exec bash .../bin/pre-push.sh` em `:49–56`; o hook instalado não foi aberto: **[nao verificado]**. Existe uma cópia histórica separada em `app/core/hooks/pre-push.sh`, cujo fluxo não chama AKITA (`app/core/hooks/pre-push.sh:5–28`); não há evidência de ela ser o hook instalado **[nao verificado]**.

## 12/11 10:57 · sessao 1662bd412cf1 · - · - · origem=Ronald (tmux)

Rastreio somente leitura; testes abaixo foram abertos, não executados [nao verificado]. [nao verificado]

**A correção da referência do aval:** `cartorio.py:763-768` não faz `ausencia→folga`: transporta `ausencia_tipo`, `ausencia_id`, `ausencia_label` para a ata. A atribuição de `tipo_dia='folga'` ocorre em `_marcar_ausencias`: faixa A2 `folga`/`feriado` e cobertura integral; parcial continua `ausencia` (`app/escala/utils.py:1238-1254`; `app/ponto/services/cartorio.py:763-768`).

**Caminhos de `simular_folha`:**

| Caminho | Passa pela relavra? | Referência |
|---|---|---|
| Comando sem opções | Não: lê export/fechamentos já gravados | `app/folha/management/commands/simular_folha.py:120-134`; `_retidos` lê gravados em `:76-80` |
| `--lido` | Não: chama fechamento em leitura; a grade usada pelo fechamento lê atas existentes | `app/folha/management/commands/simular_folha.py:124-134`; `app/ponto/services/fechamento.py:1014-1015`; `app/folha/export.py:329-338`; `app/escala/services/leitor_celula.py:378-398` |
| `--recalcular` | Não relavra CelulaDia: recalcula fechamento, lê a mesma grade lavrada; persiste FechamentoMensal e DiaPago | `app/folha/management/commands/simular_folha.py:121-123`; `app/ponto/services/fechamento.py:1014-1015`, `:1142-1149`, `:1196-1206`; `app/ponto/services/dia_pago.py:359-360` |
| `--comparar` | Não: lê JSON e compara | `app/folha/management/commands/simular_folha.py:243-251` |
| Shell `foto`/`par`, sem `--antes` | Não: shell chama `--recalcular`; `par` faz as duas fotos | `bin/simular_folha.sh:73`, `:97-99` |
| Shell `foto --antes ...` | **Pode**, conforme comando fornecido: executa `tenant_command $ANTES` antes do recalculo | `bin/simular_folha.sh:65-74` |
| Shell `par --antes ...` | **Pode**, somente na foto **depois**, conforme comando fornecido | `bin/simular_folha.sh:94-99` |

Um caminho concreto de relavra é `--antes "processar_cartorio --empresa N --apply --forcar"`: flags abertas em `app/ponto/management/commands/processar_cartorio.py:31-34`, dias passados da empresa selecionados em `:54-67`, chamada a `julgar_colab` em `:71`. O cartório monta grade prevista viva em `app/ponto/services/cartorio.py:1163-1172`, consulta/transporta a ata nova em `:1432-1436` e lavra em `:1846-1870`. Isso descreve alcance do código; execução concreta desse caminho no c13 [nao verificado]. `--antes` genérico sozinho não prova que relavra ocorreu (`bin/simular_folha.sh:72-74`).

Mesmo invocar o cartório sem forçar pode conservar a ata velha se impressão estiver em dia: ramo normal `app/ponto/services/cartorio.py:1405-1411`, ramo sem adesão `:1256-1262`. O teste existente demonstra preservação de `{'tipo_dia':'ausencia'}` sem fato novo e mudança para `folga` com fato novo: `app/ponto/tests/test_lastro_c5_sem_banco.py::LastroC5::test_P5_ata_antiga_e_FOTO_sem_bump_ate_fato_novo` (classe exata `LastroC5` [nao verificado]; método/linhas verificados em `:692-712`).

**Escritores e leitores:**

- Grade escreve `dia.tipo_dia`, `ausencia_tipo/id/label`, `cobertura_parcial`: `app/escala/utils.py:1243-1254`. Cartório copia tipo e métricas/orfas para `_ata`: `app/ponto/services/cartorio.py:729-753`; copia metadados de ausência em `:766-768`.
- Persistência passa por `CelulaDia.lavrar_veredito` → porta: `app/escala/models.py:1819-1828`; porta atribui `cel.ata` e devolve campos em `app/ponto/portas/celula.py:1032-1047`; cartório usa `save=False` e `carimbar` em `app/ponto/services/cartorio.py:1847-1870`; `carimbar` salva campos lavrados em `app/ponto/portas/celula.py:1065-1072`.
- Leitor `grade_da_celula` prefere `ata.tipo_dia`; só sem esse valor usa `cel.trabalha`: `app/escala/services/leitor_celula.py:397-398`. Transporta orfas/métricas em `:418-428`, ausência em `:433-442`.
- Dinheiro conserva ausência quando existe `ausencia_tipo` mesmo se rótulo é folga: `app/ponto/services/dia_pago.py:392-406`. Teste sintético da cadeia grade→ata→leitor→mapas: `app/ponto/tests/test_lastro_c5_sem_banco.py::test_B1_grade_ata_leitor_folha_preserva_tipo_link_e_dinheiro`, `:342-360`; cobre realizados 0/180/600, não `simular_folha`.
- `trabalha` tem escritores gerar/editar na porta: criação `app/ponto/portas/celula.py:851-857`, edição em `:609`; a lavra acima altera ata/veredito/status e não atribui `trabalha` (`:1032-1038`).

**Os dois leitores de folga com batida:**

1. `folha.causas.classificar_causa` usa `tipo_dia=='folga' and orfas` em `app/folha/causas.py:100`. Só chega ali depois das saídas antecipadas para falta de previsão, classe da régua, ausência de furo, adesão zero e sem âncora (`:58-99`); retorna `dias_trocados` em `:104-106`, sem conferir `ausencia_tipo` (`:100-106`). Prova existente: `app/folha/tests/test_causas.py::CausasTest::test_dias_trocados_nao_vira_falta`, `:81-85`, com folga sintética/orfas e trabalho com missing; nenhuma relavra nesse teste (`:81-85`).

2. `tripwire_celulas.medir_ata_fantasma` pré-filtra `trabalha=False` ou NULL (`app/ponto/management/commands/tripwire_celulas.py:338-340`), então exige ata `folga`, sem lâmpadas (`:342`) e conta `n_celulas>0` (`:344-349`). Não pergunta se houve batida/orfas nesse julgamento (`:341-349`). **Célula `trabalha=True` relavrada com ata folga fica fora do queryset**, embora o leitor `grade_da_celula` reconheça o tipo da ata (`:338-342`; `app/escala/services/leitor_celula.py:397-398`). Provas existentes: `app/ponto/tests/test_tripwire_ata_fantasma.py::TripwireEnxergaFolgaIndeterminadaTest::test_GOLDEN_folga_com_trabalha_NONE_e_contada`, `:56-65`, e `::test_folga_com_trabalha_FALSE_segue_contada`, `:67-71`; as fixtures têm `n_orfas=0` (`:47-54`), não fecham relavra com batida.

Testes de simulação abertos não demonstram esse caso: `app/folha/tests/test_o48_passo4_simular_folha_lido.py` prepara jornada/folga e vereditos em `:87-112`, testa escolha da autoridade em `::test_MORDE_a_foto_gravada_nao_manda_na_linha_do_TXT_quando_lido` (`:159-189`), gravado em `::test_MORDE_sem_lido_a_foto_segue_o_gravado` (`:191-215`) e ausência de escrita de fechamento/DiaPago em `::test_MORDE_lido_nao_escreve_nada` (`:231-245`). Ausência→folga dentro de `simular_folha` nessas provas: nenhuma (`:87-112`, `:159-245`).

## 12/11 10:57 · sessao bd44bcd67409 · - · - · origem=Ronald (tmux)

PDF — achados para consolidação, apenas leitura; testes não executados [nao verificado]. [nao verificado]

| Entrada | Juiz consultado | Operação | Persistido | Retorno | Prova existente |
|---|---|---|---|---|---|
| Ausência conhecida com faixa, aprovada | `faixa_de`, `app/ponto/precedencia.py:105`; A2 em `app/escala/utils.py:1212`; composição comum em `app/ponto/services/espelho.py:343` | Coletor chama tela e recorta seus dias (`app/relatorios/pdf_espelho.py:479`, `:563`); reaplica palavra comum (`:598`) | Nenhuma escrita de ata/cadastro pelo coletor; muta os objetos dos dias (`:567`, `:571`, `:600`). Emissão auditável grava registro, conforme ressalva abaixo | `palavra_dia` na coluna Motivo (`app/relatorios/pdf_espelho.py:885`) | `app/relatorios/tests/test_rotulo_ausencia_pdf.py::test_05_tipo_novo_criado_pela_porta_rotula_sozinho` (`:52`), com código `reciclagem`, cadastro pela porta e ausência aprovada |
| Ausência de tipo sem faixa | A2 captura recusa por data quando recebe `recusas`, `app/ponto/precedencia.py:419`; grade recebe recipiente em `app/relatorios/pdf_espelho.py:474` | Grade escreve `recusa`, `tipo_dia=None` e preserva geometria (`app/escala/utils.py:1218`); PDF transporta recusa da **grade própria**, `app/relatorios/pdf_espelho.py:567`; compositor a transforma em palavra vermelha (`app/ponto/services/espelho.py:339`) | Nada de domínio; campos transitórios em memória (`app/relatorios/pdf_espelho.py:567`, `:600`) | `"Tipo sem faixa: <codigo>"`, veredito vazio (`app/ponto/services/espelho.py:340`), desenhado em `app/relatorios/pdf_espelho.py:885` | `app/ponto/tests/test_lastro_c13_sem_banco.py::test_PDF_le_rotulo_recusado_do_contrato_sem_concluir_precedencia` (`:154`); teste DB existente `app/relatorios/tests/test_rotulo_ausencia_pdf.py::test_09_legado_sem_faixa_rotula_so_o_dia_recusado` (`:85`) |
| Batida órfã; caso sem célula na tela | PDF consulta `grade_espelho_janela`, `app/relatorios/pdf_espelho.py:474`; tela chama `montar_realizado_grade`, `app/ponto/services/espelho.py:698` | PDF lê `grade_pdf.dias[].orfas`, indexados por data (`app/relatorios/pdf_espelho.py:475`, `:480`), une horários (`:481`), escreve `batida.orfa` (`:571`) e exclui entrada espúria de turno aberto (`:517`) | Nada de domínio; atributo transitório `batida.orfa` (`app/relatorios/pdf_espelho.py:571`) | Órfã continua impressa, cinza com asterisco (`app/relatorios/pdf_espelho.py:838`, `:847`); seus alertas de turno são omitidos (`:926`) | `app/ponto/tests/test_lastro_c11_sem_banco.py::test_PDF_igual_tela_orfa_sem_celula_dia_valido_coletor_inteiro` (`:380`) exige `orfas_grade=[]` na tela (`:383`) e `batida.orfa=True` (`:384`); o nome do teste não demonstra igualdade |
| Dia relavrado de ausência para folga, com batida | Faixa A2 traduzida em `tipo_dia='folga'` para cobertura integral (`app/escala/utils.py:1243`); leitura dos dias da tela em `app/relatorios/pdf_espelho.py:563` | PDF usa dias da tela, mas recalcula classificação transitória da órfã pela própria grade (`app/relatorios/pdf_espelho.py:567`); mostra palavra e pago da linha (`:864`, `:885`) | Coletor não relavra; cartório transporta `ausencia_tipo/id/label` à ata (`app/ponto/services/cartorio.py:766`) | Não é possível afirmar só de `ata.tipo_dia='folga'` que a palavra será “Folga”: compositor prioriza cobertura (`app/ponto/services/dia_decidido.py:363`), depois reconhece fatos A2 `folga/folga_da_escala` (`:371`). Resultado concreto sem mais entradas [nao verificado] | **Nenhuma prova aberta da sequência relavra → PDF**. Teste próximo `app/relatorios/tests/test_pdf_linha_do_dia_e_o_pago.py::test_MORDE_folga_trabalhada_NAO_sai_como_zero_no_papel` (`:100`) fabrica `DiaPago` (`:113`); ele mesmo declara que testa derivação, não lavratura (`:109`) |

A função que a tela chama é `montar_realizado_grade` (`app/ponto/services/espelho.py:698`), definida em `app/escala/utils.py:1081`: sem dia ou marcos retorna duas listas vazias (`:1087`); caso contrário, devolve `gd['orfas']` (`:1113`). O resultado é publicado em `dia['orfas_grade']` (`app/ponto/services/espelho.py:853`).

O PDF **já chama** `espelho_do_colab` (`app/relatorios/pdf_espelho.py:479`). Portanto, poderia ler desse retorno `{d['data']: set(d.get('orfas_grade') or []) ...}`, exatamente a troca introduzida pelo mutante `orfas_tela` (`app/ponto/tests/test_lastro_c13_sem_banco.py:45`). Desapareceriam chamada e índice da grade própria (`app/relatorios/pdf_espelho.py:473`); mudaria a fonte do mapa (`:480`). Porém, `:567` também depende de `_dias_grade` para a recusa: ao remover a grade, esse transporte precisa ler `_recusa_do_dia` que a tela publica (`app/ponto/services/espelho.py:880`), preservando a palavra comum (`:339`). Apagar apenas `:474-476` deixa referência pendente em `:567`.

A igualdade do selo é fraca: coletor seleciona os mesmos objetos de `_tela['dias']` (`app/relatorios/pdf_espelho.py:563`), depois os muta (`:567`, `:571`, `:600`); a fixture devolve `dias` ao chamador (`app/ponto/tests/test_lastro_c11_sem_banco.py:378`) e compara essa lista ao resultado (`:382`). Já a divergência explícita de fontes é demonstrada pelas asserções `:383-384`.

“Persistido = nada” deve ser limitado ao domínio/coletor: a emissão mensal chama `gerar_pdf_auditavel` (`app/relatorios/pdf_espelho.py:1065`), que cria `RegistroPDFAuditavel` (`app/relatorios/pdf_base.py:152`) com hash, filtros, autor, identificação do colaborador e dados de origem (`:153-165`). A emissão informacional usa buffer em memória e retorna bytes (`app/relatorios/pdf_espelho.py:1018`, `:1033`).

A união só por HH:MM existe (`app/relatorios/pdf_espelho.py:481`); `_entrada_espuria` compara apenas horário (`:178`), e esse predicado controla a contagem de turnos abertos (`:517`). Assim, a estrutura permite colisão entre datas; incidência real no dado raspado [nao verificado].

## 12/11 10:49 · sessao c53e9ca51217 · LASTRO-FAIXA-SEM-DEFAULT · censo · origem=Code

1. **BLOQUEIA-1 — selo PDF/tela e igualdade sobre lista compartilhada**
   - **Classe:** [nao verificado]. O Codex viu; descarte pela triagem não está documentado. (`preverif_c13.r:29`; `postmortem_anexo_triagem.txt:2`)
   - **Pergunta — preverif_c13.q:11-12:**
     > relatorios/pdf_espelho.py: o PDF le SO o contrato de render (grade_espelho_janela) -- nenhuma regra propria de dia, faixa,
     > orfa ou turno aberto (LEI-AKITA 2)?
   - **Resposta — preverif_c13.r:29:**
     > Contra-exemplo já fixado pelo teste alterado: tela tem `orfas_grade=[]`, enquanto PDF marca a batida como órfã e conta zero aberto. A igualdade `pdf['dias']==dias` compara a lista compartilhada que o próprio coletor modifica. [nao verificado]
   - **Triagem — postmortem_anexo_triagem.txt:2:**
     > B1 duas fontes de orfa (PDF grade_espelho_janela recusas={} x tela orfas_grade): PROCEDENTE
   - **Motivo literal de descarte:** nenhum.

2. **BLOQUEIA-2 — `_pela_grade` sem recusas**
   - **Classe:** [nao verificado]. O Codex viu o ramo de ata agregada; descarte pela triagem não está documentado. (`preverif_c13.r:33`; `postmortem_anexo_triagem.txt:4`)
   - **Pergunta — preverif_c13.q:12:**
     > A tela e o PDF mostram o mesmo rotulo 'Tipo sem faixa: \<codigo>' para o mesmo dia?
   - **Resposta — preverif_c13.r:33:**
     > **BLOQUEIA — rótulo não garantido no ramo de ata agregada.** O handler chama `_pela_grade()` sem `recusas`; uma ausência aprovada `fantasma_xyz` levanta `TipoSemFaixa` antes do transporte nomeado.
   - **Resposta específica sobre o `except` da pré-apuração:** nenhum.
   - **Triagem — postmortem_anexo_triagem.txt:4:**
     > B3 _pela_grade sem recusas no ramo de ata agregada (espelho.py:495,563): PROCEDENTE por leitura; ocorrencia em prod nao medida.
   - **Motivo literal de descarte:** nenhum.

3. **Item 2 — RED e mutantes c13 somente em prosa**
   - **Classe:** **(c)** — evidência de execução/RED. (`CLAUDE.md:386`; `opus_final_c13.txt:12-13`)
   - **Pergunta — preverif_c13.q:6:**
     > Caso H -- o harness novo ainda morde (mutante que desfaz a cura do produto fica vermelho)?
   - **Resposta — preverif_c13.r:53:**
     > os mutantes executados ficaram vermelhos.
   - **Trecho da resposta que fornece saída de falha em `c13.log`/`construtor_c13.r`:** nenhum.

4. **Item 8 — relavra `ausencia` → `folga` e leitores não fechados**
   - **Classe:** **(a)** para a lacuna de código sobre relavra e leitores; efeito financeiro efetivo em produção: [nao verificado]. (`preverif_c5.q:5`; `preverif_c13.r:47`; `app/folha/causas.py:100`)
   - **Pergunta — preverif_c5.q:5:**
     > Algum leitor de dinheiro (dia_pago, fechamento, folha/export, prontidao) muda de VALOR para algum tipo/dia? Liste o caso concreto (cadastro + batidas) se mudar. O que o diff afirma preservar e de fato preservado em todo caminho (com snapshot e sem snapshot, ata gravada antes desta versao sem as chaves novas)?
   - **Pergunta — preverif_c13.q:15:**
     > Dinheiro: algum caminho de folha/simular_folha le o que mudou?
   - **Resposta — preverif_c13.r:47:**
     > **Nada de caminho demonstrado recebendo o novo `None`:** `simular_folha` passa pelo fechamento; fechamento lê `grade_do_fechamento`, cuja fonte é `grade_da_celula`.
   - **Trecho que identifica a lacuna da relavra, `_folga_bat`/`dias_trocados` ou `tripwire_celulas`:** nenhum.

5. **FILA — união HH:MM entre datas suprime turno aberto**
   - **Classe:** [nao verificado]. O Codex viu; a triagem registrou procedência e herança, sem descarte literal. (`preverif_c13.r:31`; `postmortem_anexo_triagem.txt:3`)
   - **Pergunta — preverif_c13.q:11-12:**
     > relatorios/pdf_espelho.py: o PDF le SO o contrato de render (grade_espelho_janela) -- nenhuma regra propria de dia, faixa,
     > orfa ou turno aberto (LEI-AKITA 2)?
   - **Resposta — preverif_c13.r:31:**
     > **BLOQUEIA — regra própria de turno aberto permanece.** PDF une órfãs de todas as datas por HH:MM e usa esse conjunto para excluir abertos.
   - **Triagem — postmortem_anexo_triagem.txt:3:**
     > B2 uniao HH:MM entre datas (pdf_espelho.py:174 _entrada_espuria, :480 _orfas_hhmm, :517): PROCEDENTE, HERDADO (c10 ja unia); bug no caminho (LEI-AKITA 6).
   - **Motivo literal de descarte:** nenhum.

6. **FILA — fechamento relança `TipoSemFaixa` e interrompe a empresa**
   - **Classe:** [nao verificado]. O Codex viu; não há linha de triagem correspondente no anexo. (`preverif_c10.r:47`; descarte: [nao verificado])
   - **Pergunta — preverif_c10.q:7:**
     > fechamento deixa TipoSemFaixa subir: quem chama o fechamento (cron, view, export) e o que acontece com a competencia quando sobe -- trava o fechamento inteiro da empresa? Isso e o comportamento que o aval pede (item 4/6)?
   - **Resposta — preverif_c10.r:47:**
     > As três capturas alteradas relançam `TipoSemFaixa`. Se ela ocorrer, interrompe o restante da chamada — empresa inteira ou subconjunto escolhido —, sem representar uma tranca formal da competência
   - **Resposta — preverif_c10.r:49:**
     > Não há atomicidade de lote: A pode estar salvo, B pode ganhar linha `aberto` antes da falha, e C não ser processado.
   - **Triagem:** nenhum.
   - **Motivo literal de descarte:** nenhum.

7. **FILA — `efeito_vigente` inclui inativos**
   - **Classe:** **(a)** quanto à inclusão de inativos; efeito zero na frota: [nao verificado]. (`preverif_c5.q:5`; `app/ponto/catalogo/ausencias.py:299`)
   - **Pergunta — preverif_c5.q:5:**
     > Algum leitor de dinheiro (dia_pago, fechamento, folha/export, prontidao) muda de VALOR para algum tipo/dia?
   - **Resposta que identifica a inclusão de inativos como o achado final:** nenhum.
   - **Resposta anterior, sobre o comportamento inverso — preverif_c5.r:28:**
     > **BLOQUEIA — a ata nova perde valores quando o tipo sai do catálogo ativo.**

8. **FILA — `folga_da_escala` recarimba precedência e é exposta pela API**
   - **Classe:** **(a)**. (`preverif_c5.q:4`; `app/ponto/precedencia.py:431`; `app/ponto/services/cartorio.py:1999`; `app/ponto/services/dia_do_colab.py:90`)
   - **Pergunta — preverif_c5.q:4:**
     > QUEM ESCREVE e QUEM LE, inclusive fora do diff; todos supoem o MESMO significado (unidade, dia do turno x dia civil, inclusivo/exclusivo, folga humana x folga da escala)?
   - **Resposta sobre o marcador — preverif_c5.r:10:**
     > A2 escreve o código vencedor; c5 distingue folga automática por `folga_da_escala`.
   - **Trecho que identifica o efeito de recarimbo preguiçoso e sua exposição pela API:** nenhum.

9. **FILA — `orfa_classe.py` sem chamador produtivo**
   - **Classe:** **(e) NAO PERGUNTADO** — pergunta específica sobre ausência de chamadores: nenhum. (`opus_final_c13.txt:22`; `CLAUDE.md:386`)
   - **Pergunta:** nenhum.
   - **Resposta que identifica zero chamador:** nenhum.
   - **Resposta sobre alcance, sem identificar o órfão — preverif_c13.r:23:**
     > Alcance desses leitores pelo canal novo do PDF: **[nao verificado]**.

10. **Suíte c13 — RC do processo não registrado** [nao verificado]
    - **Classe:** **(c)** — evidência de execução/suíte. (`CLAUDE.md:386`; `opus_final_c13.txt:17`) [nao verificado]
    - **Pergunta específica sobre registro do RC da suíte c13:** nenhum. [nao verificado]
    - **Resposta — preverif_c13.r:53:**
      > Suite integral, HTTP/ORM e PDF binário: **[nao verificado]**. [nao verificado]

| Achado | Classe | Volta |
|---|---|---|
| BLOQUEIA-1 | [nao verificado]: viu; descarte não documentado | c13 |
| BLOQUEIA-2 | [nao verificado]: viu; descarte não documentado | c13 |
| RED c13 somente em prosa | (c) | c13 |
| Item 8: relavra e leitores | (a) | c5/c13 |
| FILA: união HH:MM | [nao verificado]: viu; descarte não documentado | c13 |
| FILA: fechamento interrompe empresa | [nao verificado]: viu; triagem ausente | c10 |
| FILA: efeito inclui inativos | (a) | c5 |
| FILA: recarimbo/API | (a) | c5 |
| FILA: `orfa_classe` sem chamador | (e) | nenhuma |
| Suíte: RC não registrado | (c) | c13 | [nao verificado]

## 12/11 10:47 · sessao e39f8978844e · - · - · origem=Ronald (tmux)

| Achado | Classe sugerida | Trechos literais / limites |
|---|---|---|
| RED c13 apenas em prosa, sem saída de falha | **(c)** | `.q`: preverif_c13.q: “Caso H -- o harness novo ainda morde (mutante que desfaz a cura do produto fica vermelho)?” `.r`: preverif_c13.r: “os mutantes executados ficaram vermelhos.” Isso afirma execução, mas não fornece o log cuja ausência o opus apontou. Prova documental de execução c13: **[nao verificado]**. |
| Item 8: relavra muda `ausencia`→`folga`; leitores financeiros não fechados | **(a)** para a lacuna de código | `.q`: preverif_c5.q: “O que o diff afirma preservar e de fato preservado em todo caminho (com snapshot e sem snapshot, ata gravada antes desta versao sem as chaves novas)?”; preverif_c13.q: “Dinheiro: algum caminho de folha/simular_folha le o que mudou?” `.r` que encontre relavra, `_folga_bat` ou tripwire: **nenhum**. A resposta c13 restringiu-se a: “**Nada de caminho demonstrado recebendo o novo `None`:** `simular_folha` passa pelo fechamento; fechamento lê `grade_do_fechamento`, cuja fonte é `grade_da_celula`.” O leitor `folga`+órfã existe efetivamente em `app/folha/causas.py:100`. Resultado financeiro de produção: **[nao verificado]**. |
| FILA: `efeito_vigente` inclui inativos | **(a)** defensável; não confundir com achado anterior inverso | `.q`: preverif_c5.q: “Algum leitor de dinheiro (dia_pago, fechamento, folha/export, prontidao) muda de VALOR para algum tipo/dia?” `.r` que aponte a inclusão atual de inativos como mudança: **nenhum**. A c5.r encontrou o inverso: “**BLOQUEIA — a ata nova perde valores quando o tipo sai do catálogo ativo.**” Portanto isso não é citação de que o Codex viu o achado final. A inclusão atual está em `app/ponto/catalogo/ausencias.py:299`, com universo implementado em `:249`. Efeito zero na frota: somente dado do opus, **[nao verificado]**. |
| FILA: `folga`→`folga_da_escala` recarimba precedência preguiçosamente e API expõe | **(a)** | `.q`: preverif_c5.q: “Para cada campo/valor que o diff le ou grava (faixa, tipo_do_dia, tipo_dia, ausencia_tipo, ausencia_id, ausencia_label, mapas abono/previsto/realizado por dia, totais do fechamento, prontidao, TipoSemFaixa, tipos_ativos_sem_faixa): QUEM ESCREVE e QUEM LE, inclusive fora do diff; todos supoem o MESMO significado [...]?” `.r` que identifique esse efeito de recarimbo: **nenhum**. A c5.r reconheceu somente a origem/consumidores: “A2 escreve o código vencedor; c5 distingue folga automática por `folga_da_escala`.” O valor atual nasce em `app/ponto/precedencia.py:431`; a API de diagnóstico expõe em `app/ponto/services/dia_do_colab.py:90`; o carimbo por chamada A2 está em `app/ponto/services/cartorio.py:1997`. |
| FILA: `orfa_classe.py` sem chamador produtivo | **(e)** | `.q` que peça censo de chamadores dessa função ou órfãos dessa obra: **nenhum**. `.r` que encontre zero chamador: **nenhum**. C13.r discutiu interpretação latente de `None` e declarou alcance **[nao verificado]**, não encontrou ausência de chamadores. A busca atual encontra definição em `app/escala/orfa_classe.py:53` e uso nos testes em `app/escala/tests/test_orfa_classe.py:20`; nenhum uso produtivo localizado na busca `app/{folha,escala,ponto,relatorios}`. |
| Suite: rc do processo não registrado | **(c)** | `.q` específica sobre registro do rc da suite c13: **nenhum**. `.r`: preverif_c13.r: “Suite integral, HTTP/ORM e PDF binário: **[nao verificado]**.” O dado da execução é externo à inspeção de código; rc efetivo: **[nao verificado]**. | [nao verificado]

Não há material suficiente sobre opus anteriores para sustentar **(d)** nestes itens; atribuí-la exigiria inferência **[nao verificado]**.

## 12/11 10:46 · sessao fe2690a1a345 · - · - · origem=Ronald (tmux)

**BLOQUEIA-1 — classe não determinável entre as cinco sem inferir descarte.** O Codex viu; a triagem o considerou procedente. Descarte: nenhum. [nao verificado]

- **preverif_c13.q, item 3:** “relatorios/pdf_espelho.py: o PDF le SO o contrato de render (grade_espelho_janela) -- nenhuma regra propria de dia, faixa,
   orfa ou turno aberto (LEI-AKITA 2)? A tela e o PDF mostram o mesmo rotulo 'Tipo sem faixa: <codigo>' para o mesmo dia?”
- **preverif_c13.r, item 3:** “**BLOQUEIA — duas fontes para a mesma órfã.** PDF reconstrói pelo contrato; tela lê a célula na competência. Contra-exemplo já fixado pelo teste alterado: tela tem `orfas_grade=[]`, enquanto PDF marca a batida como órfã e conta zero aberto. A igualdade `pdf['dias']==dias` compara a lista compartilhada que o próprio coletor modifica.” [nao verificado]
- **preverif_c13.r, item 4:** “**BLOQUEIA — selo insuficiente para igualdade:** o teste aceita explicitamente respostas diferentes para a órfã; portanto verde não certifica ‘cartão=espelho’.” [nao verificado]
- **Triagem literal:** “B1 duas fontes de orfa (PDF grade_espelho_janela recusas={} x tela orfas_grade): PROCEDENTE -- o c13 TROCOU a fonte (c10 lia _tela['dias'].orfas_grade; c13 le _grade_pdf.orfas) para fazer R6-R9 passarem; test_lastro_c11_sem_banco.py:383-384 aceita respostas diferentes. Pergunta de desenho ao opus: quem e a fonte da orfa no PDF quando a celula responde sem_celula.”
- **Motivo literal de descarte:** nenhum. [nao verificado]

**BLOQUEIA-2 — mesma insuficiência classificatória.** Visto e considerado procedente; descarte não documentado. [nao verificado]

- **preverif_c13.q, item 3:** “A tela e o PDF mostram o mesmo rotulo 'Tipo sem faixa: <codigo>' para o mesmo dia?”
- **preverif_c13.r, item 3:** “**BLOQUEIA — rótulo não garantido no ramo de ata agregada.** O handler chama `_pela_grade()` sem `recusas`; uma ausência aprovada `fantasma_xyz` levanta `TipoSemFaixa` antes do transporte nomeado. PDF também passa por essa tela depois de obter sua grade. Contra-exemplo: ata agregada que exige o builder + ausência aprovada sem faixa.”
- **Triagem literal:** “B3 _pela_grade sem recusas no ramo de ata agregada (espelho.py:495,563): PROCEDENTE por leitura; ocorrencia em prod nao medida.”
- **Motivo literal de descarte:** nenhum. [nao verificado]

**FILA união HH:MM — mesma insuficiência classificatória.** Visto; triagem o qualifica como herdado, mas não o descarta. [nao verificado]

- **preverif_c13.q, item 3:** “relatorios/pdf_espelho.py: o PDF le SO o contrato de render (grade_espelho_janela) -- nenhuma regra propria de dia, faixa,
   orfa ou turno aberto (LEI-AKITA 2)?”
- **preverif_c13.r, item 3:** “**BLOQUEIA — regra própria de turno aberto permanece.** PDF une órfãs de todas as datas por HH:MM e usa esse conjunto para excluir abertos. Contra-exemplo executado em memória: órfã em **10/11 07:02** elimina um aberto legítimo de **11/11 07:02**, mesmo sem órfã nesse segundo dia; a tela conta esse aberto.”
- **Triagem literal:** “B2 uniao HH:MM entre datas (pdf_espelho.py:174 _entrada_espuria, :480 _orfas_hhmm, :517): PROCEDENTE, HERDADO (c10 ja unia); bug no caminho (LEI-AKITA 6).”
- **Motivo literal de descarte:** nenhum. [nao verificado]

**FILA fechamento aborta empresa — classe não determinável sem inventar triagem.** Perguntado e visto na c10; nenhuma linha de triagem correspondente no anexo. [nao verificado]

- **preverif_c10.q, item 4:** “fechamento deixa TipoSemFaixa subir: quem chama o fechamento (cron, view, export) e o que acontece com a competencia quando sobe -- trava o fechamento inteiro da empresa? Isso e o comportamento que o aval pede (item 4/6)?”
- **preverif_c10.r, item 4:** “As três capturas alteradas relançam `TipoSemFaixa`. Se ela ocorrer, interrompe o restante da chamada — empresa inteira ou subconjunto escolhido —, sem representar uma tranca formal da competência”
- **preverif_c10.r, item 4:** “Não há atomicidade de lote: A pode estar salvo, B pode ganhar linha `aberto` antes da falha, e C não ser processado.”
- **Triagem:** nenhum. [nao verificado]
- **Motivo literal de descarte:** nenhum. [nao verificado]

O único encaminhamento geral literal é: “DESTINO: MANHA-1211 item 3 = UMA volta de cura (foi a c13) e UM opus final; CODEX item 5(d) >3 voltas sobe ao opus. Vai ao opus final com esta triagem + preverif_c13.r como anexo, depois da suite verde e do par simular_folha.” Isso prova encaminhamento, não descarte. [nao verificado] [nao verificado]

## 12/11 10:43 · sessao f87651c142b3 · ABSOLVIDA-PELA-FOLGA · sonda · origem=Code

1. Transcrição integral do JSON fornecido; execução e conteúdo em produção sem verificação independente. [nao verificado]

```json
{
  "contagem": {
    "coerente_ou_sem_turno_aberto": 860,
    "exportada": 27,
    "materializadas": 931,
    "nao_exportada": 39,
    "universo_base": 931
  },
  "foras": {
    "ambigua": 4,
    "sem_fonte": 1
  },
  "soma_conferida": 931,
  "casos": [
    {"col": "col110", "comp": "09/2026", "data_evento": "2026-09-18", "dia_do_turno": "2026-09-17", "emp": 3, "grupo": "exportada", "pk": 32655},
    {"col": "col219", "comp": "09/2026", "data_evento": "2026-08-22", "dia_do_turno": "2026-08-21", "emp": 2, "grupo": "exportada", "pk": 15779},
    {"col": "col241", "comp": "09/2026", "data_evento": "2026-09-20", "dia_do_turno": "2026-09-19", "emp": 2, "grupo": "exportada", "pk": 33624},
    {"col": "col272", "comp": "09/2026", "data_evento": "2026-08-30", "dia_do_turno": "2026-08-29", "emp": 2, "grupo": "exportada", "pk": 22275},
    {"col": "col296", "comp": "08/2026", "data_evento": "2026-07-22", "dia_do_turno": "2026-07-21", "emp": 2, "grupo": "exportada", "pk": 37081},
    {"col": "col331", "comp": "09/2026", "data_evento": "2026-09-14", "dia_do_turno": "2026-09-13", "emp": 2, "grupo": "exportada", "pk": 26344},
    {"col": "col331", "comp": "09/2026", "data_evento": "2026-09-16", "dia_do_turno": "2026-09-15", "emp": 2, "grupo": "exportada", "pk": 26717},
    {"col": "col332", "comp": "09/2026", "data_evento": "2026-09-13", "dia_do_turno": "2026-09-12", "emp": 2, "grupo": "exportada", "pk": 26113},
    {"col": "col344", "comp": "08/2026", "data_evento": "2026-08-12", "dia_do_turno": "2026-08-11", "emp": 2, "grupo": "exportada", "pk": 9192},
    {"col": "col425", "comp": "09/2026", "data_evento": "2026-08-29", "dia_do_turno": "2026-08-28", "emp": 2, "grupo": "exportada", "pk": 21229},
    {"col": "col475", "comp": "09/2026", "data_evento": "2026-09-08", "dia_do_turno": "2026-09-07", "emp": 2, "grupo": "exportada", "pk": 25832},
    {"col": "col478", "comp": "09/2026", "data_evento": "2026-08-24", "dia_do_turno": "2026-08-23", "emp": 2, "grupo": "exportada", "pk": 16271},
    {"col": "col499", "comp": "08/2026", "data_evento": "2026-07-24", "dia_do_turno": "2026-07-23", "emp": 2, "grupo": "exportada", "pk": 23261},
    {"col": "col499", "comp": "09/2026", "data_evento": "2026-09-08", "dia_do_turno": "2026-09-07", "emp": 2, "grupo": "exportada", "pk": 25187},
    {"col": "col499", "comp": "09/2026", "data_evento": "2026-09-10", "dia_do_turno": "2026-09-09", "emp": 2, "grupo": "exportada", "pk": 25561},
    {"col": "col612", "comp": "09/2026", "data_evento": "2026-09-13", "dia_do_turno": "2026-09-12", "emp": 2, "grupo": "exportada", "pk": 26178},
    {"col": "col634", "comp": "08/2026", "data_evento": "2026-08-08", "dia_do_turno": "2026-08-07", "emp": 2, "grupo": "exportada", "pk": 25262},
    {"col": "col649", "comp": "09/2026", "data_evento": "2026-08-26", "dia_do_turno": "2026-08-25", "emp": 2, "grupo": "exportada", "pk": 20900},
    {"col": "col67", "comp": "09/2026", "data_evento": "2026-09-04", "dia_do_turno": "2026-09-03", "emp": 3, "grupo": "exportada", "pk": 23025},
    {"col": "col673", "comp": "09/2026", "data_evento": "2026-09-14", "dia_do_turno": "2026-09-13", "emp": 2, "grupo": "exportada", "pk": 26346},
    {"col": "col72", "comp": "09/2026", "data_evento": "2026-08-23", "dia_do_turno": "2026-08-22", "emp": 3, "grupo": "exportada", "pk": 15980},
    {"col": "col747", "comp": "09/2026", "data_evento": "2026-08-31", "dia_do_turno": "2026-08-30", "emp": 3, "grupo": "exportada", "pk": 22514},
    {"col": "col820", "comp": "09/2026", "data_evento": "2026-08-31", "dia_do_turno": "2026-08-30", "emp": 2, "grupo": "exportada", "pk": 22518},
    {"col": "col820", "comp": "09/2026", "data_evento": "2026-09-10", "dia_do_turno": "2026-09-09", "emp": 2, "grupo": "exportada", "pk": 25567},
    {"col": "col865", "comp": "08/2026", "data_evento": "2026-08-01", "dia_do_turno": "2026-07-31", "emp": 2, "grupo": "exportada", "pk": 35287},
    {"col": "col888", "comp": "09/2026", "data_evento": "2026-09-06", "dia_do_turno": "2026-09-05", "emp": 2, "grupo": "exportada", "pk": 23344},
    {"col": "col891", "comp": "08/2026", "data_evento": "2026-08-15", "dia_do_turno": "2026-08-14", "emp": 2, "grupo": "exportada", "pk": 10104},
    {"col": "col1011", "comp": "11/2026", "data_evento": "2026-11-02", "dia_do_turno": "2026-11-01", "emp": 2, "grupo": "nao_exportada", "pk": 54345},
    {"col": "col110", "comp": "08/2026", "data_evento": "2026-07-28", "dia_do_turno": "2026-07-27", "emp": 3, "grupo": "nao_exportada", "pk": 3533},
    {"col": "col121", "comp": "10/2026", "data_evento": "2026-10-01", "dia_do_turno": "2026-09-30", "emp": 3, "grupo": "nao_exportada", "pk": 38608},
    {"col": "col127", "comp": "08/2026", "data_evento": "2026-08-12", "dia_do_turno": "2026-08-11", "emp": 3, "grupo": "nao_exportada", "pk": 9189},
    {"col": "col134", "comp": "08/2026", "data_evento": "2026-07-29", "dia_do_turno": "2026-07-28", "emp": 3, "grupo": "nao_exportada", "pk": 3686},
    {"col": "col134", "comp": "08/2026", "data_evento": "2026-08-04", "dia_do_turno": "2026-08-03", "emp": 3, "grupo": "nao_exportada", "pk": 5715},
    {"col": "col134", "comp": "08/2026", "data_evento": "2026-08-08", "dia_do_turno": "2026-08-07", "emp": 3, "grupo": "nao_exportada", "pk": 7642},
    {"col": "col165", "comp": "08/2026", "data_evento": "2026-08-05", "dia_do_turno": "2026-08-04", "emp": 3, "grupo": "nao_exportada", "pk": 6055},
    {"col": "col165", "comp": "10/2026", "data_evento": "2026-09-22", "dia_do_turno": "2026-09-21", "emp": 3, "grupo": "nao_exportada", "pk": 34753},
    {"col": "col331", "comp": "10/2026", "data_evento": "2026-10-04", "dia_do_turno": "2026-10-03", "emp": 2, "grupo": "nao_exportada", "pk": 40234},
    {"col": "col332", "comp": "11/2026", "data_evento": "2026-11-04", "dia_do_turno": "2026-11-03", "emp": 2, "grupo": "nao_exportada", "pk": 53721},
    {"col": "col335", "comp": "11/2026", "data_evento": "2026-11-01", "dia_do_turno": "2026-10-31", "emp": 2, "grupo": "nao_exportada", "pk": 53269},
    {"col": "col340", "comp": "10/2026", "data_evento": "2026-10-09", "dia_do_turno": "2026-10-08", "emp": 2, "grupo": "nao_exportada", "pk": 42053},
    {"col": "col347", "comp": "10/2026", "data_evento": "2026-10-01", "dia_do_turno": "2026-09-30", "emp": 2, "grupo": "nao_exportada", "pk": 39002},
    {"col": "col396", "comp": "10/2026", "data_evento": "2026-09-29", "dia_do_turno": "2026-09-28", "emp": 2, "grupo": "nao_exportada", "pk": 38296},
    {"col": "col407", "comp": "11/2026", "data_evento": "2026-11-03", "dia_do_turno": "2026-11-02", "emp": 2, "grupo": "nao_exportada", "pk": 53628},
    {"col": "col416", "comp": "11/2026", "data_evento": "2026-11-03", "dia_do_turno": "2026-11-02", "emp": 2, "grupo": "nao_exportada", "pk": 53376},
    {"col": "col451", "comp": "10/2026", "data_evento": "2026-10-16", "dia_do_turno": "2026-10-15", "emp": 2, "grupo": "nao_exportada", "pk": 44515},
    {"col": "col467", "comp": "10/2026", "data_evento": "2026-10-12", "dia_do_turno": "2026-10-11", "emp": 2, "grupo": "nao_exportada", "pk": 42809},
    {"col": "col490", "comp": "11/2026", "data_evento": "2026-10-29", "dia_do_turno": "2026-10-28", "emp": 2, "grupo": "nao_exportada", "pk": 51402},
    {"col": "col496", "comp": "11/2026", "data_evento": "2026-10-23", "dia_do_turno": "2026-10-22", "emp": 2, "grupo": "nao_exportada", "pk": 49135},
    {"col": "col496", "comp": "10/2026", "data_evento": "2026-09-29", "dia_do_turno": "2026-09-28", "emp": 2, "grupo": "nao_exportada", "pk": 55068},
    {"col": "col516", "comp": "11/2026", "data_evento": "2026-10-21", "dia_do_turno": "2026-10-20", "emp": 2, "grupo": "nao_exportada", "pk": 46463},
    {"col": "col551", "comp": "11/2026", "data_evento": "2026-10-28", "dia_do_turno": "2026-10-27", "emp": 2, "grupo": "nao_exportada", "pk": 51001},
    {"col": "col599", "comp": "10/2026", "data_evento": "2026-10-19", "dia_do_turno": "2026-10-18", "emp": 2, "grupo": "nao_exportada", "pk": 45740},
    {"col": "col612", "comp": "11/2026", "data_evento": "2026-11-06", "dia_do_turno": "2026-11-05", "emp": 2, "grupo": "nao_exportada", "pk": 54473},
    {"col": "col72", "comp": "08/2026", "data_evento": "2026-08-11", "dia_do_turno": "2026-08-10", "emp": 3, "grupo": "nao_exportada", "pk": 23252},
    {"col": "col841", "comp": "10/2026", "data_evento": "2026-10-01", "dia_do_turno": "2026-09-30", "emp": 2, "grupo": "nao_exportada", "pk": 38600},
    {"col": "col863", "comp": "10/2026", "data_evento": "2026-09-25", "dia_do_turno": "2026-09-24", "emp": 2, "grupo": "nao_exportada", "pk": 36061},
    {"col": "col876", "comp": "11/2026", "data_evento": "2026-11-01", "dia_do_turno": "2026-10-31", "emp": 3, "grupo": "nao_exportada", "pk": 52434},
    {"col": "col879", "comp": "11/2026", "data_evento": "2026-11-05", "dia_do_turno": "2026-11-04", "emp": 2, "grupo": "nao_exportada", "pk": 54449},
    {"col": "col888", "comp": "11/2026", "data_evento": "2026-10-29", "dia_do_turno": "2026-10-28", "emp": 2, "grupo": "nao_exportada", "pk": 51404},
    {"col": "col938", "comp": "10/2026", "data_evento": "2026-09-24", "dia_do_turno": "2026-09-23", "emp": 2, "grupo": "nao_exportada", "pk": 36109},
    {"col": "col94", "comp": "08/2026", "data_evento": "2026-08-14", "dia_do_turno": "2026-08-13", "emp": 3, "grupo": "nao_exportada", "pk": 9782},
    {"col": "col960", "comp": "10/2026", "data_evento": "2026-10-01", "dia_do_turno": "2026-09-30", "emp": 2, "grupo": "nao_exportada", "pk": 39409},
    {"col": "col976", "comp": "11/2026", "data_evento": "2026-10-27", "dia_do_turno": "2026-10-26", "emp": 2, "grupo": "nao_exportada", "pk": 50668},
    {"col": "col992", "comp": "11/2026", "data_evento": "2026-11-09", "dia_do_turno": "2026-11-08", "emp": 2, "grupo": "nao_exportada", "pk": 56105},
    {"col": "col999", "comp": "11/2026", "data_evento": "2026-10-26", "dia_do_turno": "2026-10-25", "emp": 2, "grupo": "nao_exportada", "pk": 50394},
    {"col": "col999", "comp": "11/2026", "data_evento": "2026-10-29", "dia_do_turno": "2026-10-28", "emp": 2, "grupo": "nao_exportada", "pk": 51407}
  ],
  "caso_51407": [
    {"col": "col999", "comp": "11/2026", "data_evento": "2026-10-29", "dia_do_turno": "2026-10-28", "emp": 2, "grupo": "nao_exportada", "pk": 51407}
  ]
}
```

A soma das categorias fornecidas fecha: **860 + 27 + 39 + 4 + 1 = 931**; são 66 casos listados, sendo 27 exportados e 39 não exportados. `materializadas=931` descreve o universo e não é parcela adicional; `caso_51407` repete um membro de `casos`. [nao verificado — conferência do dado fornecido]

2. **`materializada_em` pode estar preenchido sem resposta porque essa via carimba uma resolução pela célula.** O escritor funcional é `retratar_perguntas_absolvidas_por_celula`, que chama `p.carimbar(VIA_CELULA_ABSOLVEU, _user)` sem argumento de resposta. [app/chamados/reconciliador.py:596](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/reconciliador.py:596)

A atribuição literal é `self.materializada_em = quando`, em `_escrever_carimbo`; `carimbar` chama essa função e persiste com `self.save(update_fields=campos)`, após consultar o juiz e recusar o carimbo se a pergunta continuar viva. [app/chamados/models.py:3100](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/models.py:3100) [app/chamados/models.py:3130](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/models.py:3130) [app/chamados/models.py:3140](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/models.py:3140) [app/chamados/models.py:3153](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/models.py:3153)

`respondida_em` só é preenchido quando há opção de resposta **e** texto não vazio; essa chamada não pede isso. Portanto, os dois campos não precisam nascer juntos. [app/chamados/models.py:3104](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/models.py:3104) [app/chamados/models.py:3131](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/models.py:3131)

Nesta via, “materializada” significa **carimbada por resolução da célula**: `celula_absolveu` integra `VIAS_SEM_TESTEMUNHA`, e o código declara que a testemunha é a célula julgada, sem batida a exigir. O `help_text` do campo, que ainda menciona “batida criada + chamado fechado”, é mais estreito que esse comportamento. [app/chamados/models.py:3741](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/models.py:3741) [app/chamados/models.py:3747](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/models.py:3747) [app/chamados/models.py:3396](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/models.py:3396)

Isso não acrescenta ao predicado do aval uma exigência de resposta ou de batida plantada. [nao verificado — interpretação do aval fornecido] Explica, porém, o desvio de `classificar()`: ele usa apenas o carimbo para escolher `_fonte_materializada`, que exige batida plantada viva e lança `sem_fonte` quando ela falta. [app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:147](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:147) [app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:151](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:151) [app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:68](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:68)

O caminho histórico exato que gravou cada uma das 931 perguntas permanece [nao verificado].

3. **Sim: `_fonte_viva` contém condições mais restritivas que a divergência de dia descrita no aval.** [app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:108](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:108) [nao verificado — comparação com o aval fornecido]

- **Aberto no próprio dia:** se qualquer candidato aberto tem `data_turno == data_evento`, retorna `None` imediatamente. Assim, outro aberto em dia diferente pode nem ser examinado. A ocorrência dessa coexistência nas 931 é [nao verificado]. [app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:109](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:109)
- **Saída prevista:** ignora turnos com `data_turno >= data_evento` e só aceita os anteriores cuja saída prevista seja exatamente `data_evento`. Um aberto com dia divergente e previsão diferente pode terminar em `None`. [app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:113](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:113) [app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:118](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:118) [app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:125](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:125)
- **Exceções distintas:** sem correspondência e com candidato sem marcos, lança `sem_fonte`; com múltiplas correspondências de saída prevista, lança `ambigua`. [app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:120](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:120) [app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:123](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:123)

O contador que **potencialmente esconderia os descartes por `None` é `coerente_ou_sem_turno_aberto=860`**. O vínculo exato entre retorno e contador é [nao verificado], pois a implementação de `sonda_medida3.py` não foi localizada na cópia. Quantos desses 860 satisfazem o predicado do aval é [nao verificado]; portanto, os 66 listados não demonstram exaustividade da medida. [nao verificado]

## 12/11 10:41 · sessao 8f661e04ae0f · - · - · origem=Ronald (tmux)

- O escritor funcional é `retratar_perguntas_absolvidas_por_celula`: percorre as perguntas absolvíveis, obtém `get_user_sistema()` e chama `p.carimbar(PerguntaDisputa.VIA_CELULA_ABSOLVEU, _user)`, **sem pedir escrita de resposta**. `app/chamados/reconciliador.py:550`, `:585`, `:588`, `:596`.
- A porta `carimbar` tem `resposta=None`; escolhe `quando` ou `timezone.now()`, chama `_escrever_carimbo`, consulta `pergunta_viva` e só salva se a pergunta resultar morta. A atribuição literal de `materializada_em` está em `_escrever_carimbo`; a persistência está em `self.save(update_fields=campos)`. `app/chamados/models.py:3104`, `:3125`, `:3130`, `:3100`, `:3140`, `:3144`, `:3153`.
- `respondida_em` só recebe instante dentro de `if resposta and not texto_vazio(self.resposta_colab)`. Como o chamador desta via não passa `resposta`, é coerente haver `materializada_em` preenchido com `respondida_em` nulo. `app/chamados/models.py:3131`, `:3132`, `:3133`; `app/chamados/reconciliador.py:596`.
- A seleção exige carimbo e validação ainda nulos, célula vinculada, julgada e com veredito que não acusa; depois o gerador consulta o juiz real e exclui dia trancado. `app/chamados/reconciliador.py:628`, `:630`, `:631`, `:632`, `:638`, `:640`, `:642`, `:662`, `:668`.
- O juiz pode dar a pergunta por morta por célula com veredito que não acusa e sem marcos pendentes; isso não exige resposta nem plantio de batida. `app/chamados/juizes.py:1438`, `:1440`, `:1451`, `:1452`, `:1453`.
- `celula_absolveu` pertence explicitamente a `VIAS_SEM_TESTEMUNHA`: o comentário identifica a célula julgada como testemunha e explica que não há batida a exigir num dia absolvido. Portanto, nesta via, “materializada” é carimbo de processamento/resolução pela célula, e não prova de declaração do colaborador ou batida plantada. `app/chamados/models.py:3731`, `:3741`, `:3742`, `:3743`, `:3747`.
- O `help_text` do campo ainda diz “batida criada + chamado fechado”, descrição mais estreita que o comportamento executável acima. `app/chamados/models.py:3394`, `:3396`.
- Isso não altera o predicado solicitado no aval; invalida usar `materializada_em != NULL` como sinônimo de “existe batida plantada”. O comando existente faz essa divisão em `classificar`, mandando qualquer carimbada para `_fonte_materializada`, que reprova ausência de batida plantada viva. `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:147`, `:151`, `:152`, `:68`, `:69`, `:72`.
- O caminho histórico exato que carimbou cada uma das 931 perguntas, sua data e seu autor no banco não foi comprovado nesta inspeção. [nao verificado]

## 12/11 10:41 · sessao 9913f3b3b51d · - · - · origem=Ronald (tmux)

- `classificar()` considera “materializada” apenas pela presença de `materializada_em` e escolhe `_fonte_materializada`; essa função exige batida plantada viva e lança `sem_fonte` quando ela falta. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:147`, `:151`, `:65`, `:68`, `:72`)

- `_fonte_viva` começa com turnos que têm entrada, não têm saída e possuem `data_turno`. Se **qualquer** candidato tem `data_turno == data_evento`, retorna `None` antes de procurar um candidato de outro dia. Logo, se coexistirem um aberto no próprio dia e outro aberto cujo dia diverge, esse segundo não será avaliado nessa chamada; a existência dessa configuração nas 931 é [nao verificado]. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:108`, `:109`, `:110`)

- Há outros cortes: ignora candidatos com `data_turno >= data_evento`; dos anteriores, aceita somente aqueles cuja saída prevista é igual a `data_evento`. A previsão vem do último marco de `marcos_turno`, convertido em data local. Se não encontrar correspondência e todos os candidatos examinados tiverem marcos, retorna `None`. Esses critérios são mais restritivos que simplesmente exigir dia do turno diferente e turno ainda sem saída. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:113`, `:115`, `:118`, `:122`, `:125`, `:82`, `:90`, `:91`)

- Se não houver correspondência, mas existir candidato anterior sem marcos, lança `sem_fonte`; se houver mais de uma correspondência, lança `ambigua`. Portanto, os cortes com retorno `None` devem ser distinguidos dessas exceções. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:116`, `:120`, `:123`)

- **Contador potencialmente ocultador na v3: `coerente_ou_sem_turno_aberto` (860 no JSON fornecido).** A associação exata entre os retornos `None` e esse contador é [nao verificado]: `sonda_medida3.py` e o texto do contador não foram encontrados na cópia; o usuário forneceu somente a saída da sonda. A quantidade das 860 que satisfaria o predicado do aval também é [nao verificado].

- O juiz efetivo consulta `dia_das_batidas` para a batida por PK; a implementação atribui a cada batida reclamada o `data_turno` do turno e usa a data local para as demais. (`app/ponto/turnos.py:2927`, `:2935`, `:2945`, `:2953`, `:2955`)

## 12/11 10:39 · sessao aa1c00a286c9 · ABSOLVIDA-PELA-FOLGA · sonda · origem=Code

1. Transcrição integral do JSON fornecido; resultado de produção sem verificação independente. [nao verificado]

```json
{"caso_51407": [{"em_classificar": true, "no_universo": true}], "casos": [], "contagem": {"linhas_classificar": 931, "materializadas_no_universo": 931, "sem_dia_novo": 931, "universo_base": 931}, "foras_sem_dia": {"sem_fonte": 931}}
```

2. **`classificar` não é um medidor geral do predicado do aval.** Ela seleciona a fonte conforme `materializada_em`: preenchida → `_fonte_materializada`; ausente → `_fonte_viva`. `via_resolucao=celula_absolveu` e `respondida_em` nula não alteram essa escolha. [classificar:147](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:147)

Para `orfao_14h` **viva**, `_fonte_viva` procura turnos com entrada, sem saída e com `data_turno`. Ela:

- Devolve `None` imediatamente se **qualquer** candidato tem `data_turno == data_evento`, considerando a pergunta coerente. [fonte_viva:108](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:108)
- Entre os candidatos anteriores a `data_evento`, aceita somente aqueles cuja saída prevista cai exatamente em `data_evento`; essa previsão vem do último marco da escala. [fonte_viva:112](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:112), [saida_esperada:82](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:82)
- Marca `fora=ambigua` se houver mais de um encaixe; sem encaixe, marca `fora=sem_fonte` se algum candidato examinado não tiver marcos. Sem encaixe e sem essa falta de marcos, devolve `None`, inclusive quando existem turnos abertos que não satisfazem o encaixe exigido. Um único encaixe devolve sua entrada e seu turno. [fonte_viva:120](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:120)

Para **materializada**, a fonte exigida é uma batida viva vinculada por `Batida.pergunta_origem`, não a entrada aberta encontrada por `_fonte_viva`. Sem essa batida, `celula_absolveu` cai em `sem_fonte`; múltiplas batidas geram `ambigua`; uma batida ausente dos turnos gera `sem_fonte`, e presente em múltiplos turnos gera `ambigua`. [classificar:138](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:138), [fonte_materializada:65](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:65)

Obtida a fonte, `novo` só é preenchido depois de o juiz devolver um dia não nulo, diferente de `data_evento` e igual ao `data_turno` encontrado. Juiz sem dia gera `sem_fonte`; divergência entre juiz e turno gera `ambigua`, ambos ainda sem `novo`. Depois de preencher `novo`, `orfao_14h` materializada recebe `fora_do_aval`; para quem passar dessa condição, fato já existente no destino gera `colisao`. Portanto, **`novo` preenchido não significa ausência de `fora`**. [classificar:160](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:160)

Precisão sobre `None`: quem o devolve é `_fonte_viva`; `classificar` responde com uma lista e omite essa pergunta por `continue`. Também omite materializada cujo turno não tenha saída ou cuja saída não caia em `data_evento`, e qualquer pergunta cujo dia devolvido pelo juiz já coincida com o gravado. [classificar:154](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:154), [classificar:176](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:176)

**Sim, o predicado amplo do aval pode sofrer descarte silencioso:** uma materializada com fonte encontrada, turno ainda aberto e dia diferente é omitida antes de consultar o juiz, porque `t.saida is None`. Nas vivas, uma entrada aberta com dia diferente também pode ser omitida quando sua saída prevista não coincide com `data_evento`; existir outro turno aberto no próprio `data_evento` provoca o retorno antecipado de “coerente”. Essas condições são mais restritivas que apenas “entrada aberta cujo dia difere do gravado”. [classificar:158](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:158), [fonte_viva:108](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:108)

3. **O bloqueio equivalente nesta saída é `sem_dia_novo=931`, explicado por `foras_sem_dia.sem_fonte=931`.** Os mesmos 931 constam como materializados e como linhas retornadas; portanto, o agregado fornecido não mostra perda de perguntas entre universo e retorno, mas mostra todas sem dia novo. [nao verificado]

Como materializadas seguem `_fonte_materializada`, chamar a função real **não garante chamar o caminho que procura entrada aberta**; a exigência de batida plantada pode barrar a medição antes disso. [classificar:151](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:151), [fonte_materializada:68](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:68)

Assim, `casos: []` **não demonstra zero casos do aval**. O JSON só confirma, para #51407, presença no universo e no retorno; não fornece seu dia novo, sua entrada aberta nem separação entre exportadas e não exportadas. O número que satisfaz efetivamente o predicado do aval permanece [nao verificado].

## 12/11 10:38 · sessao 5cbda7b3d038 · - · - · origem=Ronald (tmux)

Entradas já alcançam a relavra comum: lançar [escala/folgas.py:337], alterar/remover [:380], lote [:418], reparentar [:523]. A relavra suplementa futuras em [:298]. Assim, testes comportamentais de amanhã devem ficar GREEN no HEAD; RED no HEAD desses comportamentos é [nao verificado]. A deficiência apontada é de evidência individual. [nao verificado]

Para ampliar a suíte pura: [nao verificado]

- Carregar `_foto_do_dia`, `lancar_folga`, `alterar_folga`, `aplicar_folgas_em_lote`, `reparentar_folga` e **a relavra real**, usando `_dias_dos_meses_que_sairam=lambda *a: []`; o teste atual da relavra já usa esse preparo. [ponto/tests/test_cadastro_futuro_sem_banco.py:584] [nao verificado]
- Reutilizar `calendario()`: seu `eh_dia_trabalho_calculado` consulta as fotos do vínculo, permitindo verificar que amanhã muda de trabalho para folga. [ponto/tests/test_cadastro_futuro_sem_banco.py:480]
- Acrescentar `Query.get_or_create`, necessário para lançamento. [escala/folgas.py:335]
- Usar foto com `save/delete`, `pk`, `tipo` e `calendario_origem=None`; `Foto` atual fornece somente a propriedade do calendário. [ponto/tests/test_cadastro_futuro_sem_banco.py:176]
- Dublar `ponto.precedencia.veto_de_lancamento` e `core.models.LogAuditoria`; a porta de reparentamento exige também `vinculo_do_dia_carregado`, retornando o vínculo novo pela autoridade previamente carregada. [escala/folgas.py:328] [escala/folgas.py:479] [escala/folgas.py:493]

Casos individuais recomendados:

1. `lancar_folga(novo, AMANHA, 'F')`: começar com amanhã coerente, trabalhando, nomeando `novo`; verificar foto criada e célula não trabalhando. [escala/folgas.py:335]
2. `alterar_folga(novo, AMANHA, 'F2')`: foto manual já existente e célula com DNA anterior incoerente; verificar tipo e DNA atualizados. [escala/folgas.py:364]
3. Remoção: foto manual existente, célula coerente como folga; `alterar_folga(..., '')` remove foto e amanhã volta a trabalhar. [escala/folgas.py:353]
4. `aplicar_folgas_em_lote(novo, [(AMANHA, 'F')], hoje=HOJE)`: conferir lista gravada, foto e célula. [escala/folgas.py:405]
5. Reparentar: foto manual de amanhã no vínculo velho encerrado hoje; célula de amanhã já nomeia `novo`; conferir retorno `migrada`, FK da foto e célula. [escala/folgas.py:487] [escala/folgas.py:515]
6. Aplicar calendário: carregar função real da view sem decoradores; publicar serviços reais preparados em `sys.modules`; fornecer `request.method='POST'` e `POST['tipo_escala_id']`; dublar somente `JsonResponse`, `get_object_or_404` e relógio. [escala/views.py:376] [escala/views.py:388]

O mutante comum apropriado é envolver a chamada futura da relavra em `if ec.data_inicio <= _hoje_teste`. O mutador atual apenas remove essa chamada porque `relavrar_celulas_da_foto` não integra seu conjunto de guardas; precisa receber essa variante. [ponto/tests/test_cadastro_futuro_sem_banco.py:87] [escala/folgas.py:298]

Aplicar calendário tem redundância legítima: troca a FK com sincronização futura, materializa fotos que também relavram e chama relavra explícita. Mutar somente a chamada explícita pode sobreviver graças aos outros caminhos; o teste deve executar tudo e aplicar a guarda aos transportes futuros comuns. [escala/views.py:409] [escala/views.py:411] [escala/views.py:416] [colaboradores/services/vinculo.py:454] [escala/folgas.py:215] [nao verificado]

## 12/11 10:38 · sessao b118550ea971 · - · - · origem=Ronald (tmux)

Item 4(c) está bloqueado pelo limite de escrita: a regeneração fora da vigência conserva `escala_geradora` e só salva se `trabalha` ainda não for falso ([celula.py:238](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-090745/app/ponto/portas/celula.py:238)). A juíza devolve `None` para a geradora encerrada ([alimentacao.py:282](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-090745/app/escala/alimentacao.py:282)), portanto o contador permanece positivo corretamente ([vigia_cadastro_futuro.py:36](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-090745/app/ponto/services/vigia_cadastro_futuro.py:36)). Nenhuma operação existente permite limpar o carimbo: a reversão também só atribui geradora quando o identificador restaurado é verdadeiro ([celula.py:789](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-090745/app/ponto/portas/celula.py:789)). Resolver sem editar esse arquivo exigiria outro escritor ou alterar artificialmente julgamento/contador.

Item 5 admite solução na casca do comando: dentro do atomic existente, regenerar somente células geradas existentes de hoje, chamando a porta atual com `recalculo=False` e `estender_horizonte=False`; a chamada ficaria junto ao gerador ([gerar_celulas.py:310](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-090745/app/ponto/management/commands/gerar_celulas.py:310)). Assim o cron mantém a execução pelo comando, sem editar sua configuração. A seleção deve preservar o universo ativo do gerador ([celula.py:830](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-090745/app/ponto/portas/celula.py:830)); usar a geradora carimbada julgada pode divergir do comportamento anterior em sobreposições. Equivalência completa desse caminho alternativo: [nao verificado].

## 12/11 10:36 · sessao 0d7a98f2dd10 · ABSOLVIDA-PELA-FOLGA · sonda · origem=Code

**931 no universo; 931 em `sem_batida_origem`; listas de casos vazias.** Transcrição do JSON fornecido, sem conferência em produção. [nao verificado]

```json
{"caso_51407": [], "casos": [], "contagem": {"sem_batida_origem": 931, "universo_base": 931}}
```

- **Exportada × não exportada:** os grupos não constam da saída; não podem ser transcritos como zero. Nenhuma tupla `(colNNN, #pk, data_evento, dia do turno, competência)` foi retornada. [nao verificado]
- **#51407/col999 não aparece.** A saída não identifica seu contador individual. Se integrou `universo_base`, os totais são compatíveis com exclusão em `sem_batida_origem`; confirmar isso exige o script ou o diagnóstico individual. [nao verificado]

1. **A saída não comprova a medida do aval.** O conteúdo de `sonda_medida1.py` não está no texto recebido nem foi localizado nesta cópia; seus filtros de `via_resolucao`, ausência de resposta e ordem dos contadores permanecem [nao verificado].

- **A FK `PerguntaDisputa.batida_origem` não é a fonte garantida de `orfao_14h`.** Esse motivo pertence à família MARCO; a FK identifica episódios BATIDA, e seu preenchimento automático retorna `None` para MARCO. Exigir essa FK descarta perguntas sem avaliar o turno que elas cobram. — `app/chamados/catalogo/motivos.py:34`; `app/chamados/models.py:2437`; `app/chamados/models.py:3462`.
- **O argumento homônimo `resolver_data_evento(..., batida_origem=...)` representa, sim, a entrada aberta de `orfao_14h`.** Ele consulta `dia_do_turno_da_batida`; isso não significa que aquela entrada esteja gravada na FK da pergunta. — `app/escala/servico_jornada.py:495`; `app/escala/servico_jornada.py:499`; `app/escala/servico_jornada.py:509`.
- **Buscar o turno por `pk` em `t.batidas` usa a mesma associação do juiz.** Porém, o juiz do dia monta um mapa por PK e atribui o dia local às batidas não reclamadas por turno; exigir exatamente um turno acrescenta uma restrição. A equivalência também depende da janela utilizada pela sonda. — `app/ponto/turnos.py:2933`; `app/ponto/turnos.py:2952`; `app/ponto/turnos.py:2955`. Janela e restrições efetivas da sonda: [nao verificado].
- **A migração existente não exige essa FK para encontrar a entrada aberta:** procura candidato único com entrada, sem saída e último marco previsto no dia gravado, retornando `t.entrada`; também exclui explicitamente `orfao_14h` materializada. — `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:108`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:115`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:126`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:168`.
- **A separação deve consultar `marcos_da_competencia(...)[“exportada”]`.** O campo `marco` pode indicar uma etapa posterior, como paga; a competência da data segue a janela da empresa. — `app/ponto/services/fechamento.py:1537`; `app/ponto/services/fechamento.py:1543`; `app/ponto/services/dia_pago.py:716`.

2. **Os números e as listas são exatamente os transcritos acima; o total real do predicado continua indeterminado.** [nao verificado]

3. **O contador capaz de esconder casos é `sem_batida_origem`: 931/931, ou 100% do universo reportado.** [nao verificado] A exclusão por FK nula não demonstra ausência do predicado, pois essa família não recebe a FK automaticamente. — `app/chamados/models.py:3462`.

- **`juiz_sem_dia`, `ambiguo` e `com_saida` não foram publicados:** tamanho e participação nas exclusões individuais são [nao verificado]. [nao verificado]
- **`juiz_sem_dia` e `ambiguo` representam falta de determinação, não prova de inexistência do caso:** o juiz pode devolver `None`, e a busca da fonte pode encontrar múltiplos candidatos. — `app/ponto/turnos.py:2936`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:120`.
- **`com_saida` excluiria corretamente apenas quando fosse a saída do turno certo da fonte**, pois o predicado exige turno ainda sem saída. O código distingue candidatos por entrada e ausência de saída; se a sonda aplicou isso ao turno correto permanece [nao verificado]. — `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:108`.

## 12/11 10:35 · sessao 5f24455a0385 · - · - · origem=Ronald (tmux)

- `marcos_da_competencia` separa exportada pelo booleano `['exportada']`, delegado a `competencia_entregue(empresa_id, mes, ano)`. Comparar `['marco'] == 'exportada'` perderia competências cujo marco mais adiantado é `trancada` ou `paga`. — `app/ponto/services/fechamento.py:1537`, `app/ponto/services/fechamento.py:1543`
- Entregue significa existência de `ExportacaoDominio` vigente com `invalidada_em is None`; existência de qualquer exportação ou `FechamentoMensal.status='exportado'` não substitui esse juiz. — `app/folha/export.py:588`, `app/folha/export.py:591`, `app/ponto/services/fechamento.py:1533`
- Competência do fato: `competencia_do_fato(colaborador, dia)` chama `competencias_do_fato`; a derivação usa `janela_atual(dia, empresa)` e toma mês/ano do fim da janela. — `app/ponto/services/fechamento.py:1602`, `app/ponto/services/fechamento.py:1599`, `app/ponto/services/dia_pago.py:716`
- `competencia_do_dia_entregue` já compõe janela da empresa e juiz de entrega; `None` significa ausência de dia, não comprovação de não exportada. — `app/chamados/services/vigia_fio.py:822`, `app/chamados/services/vigia_fio.py:834`, `app/chamados/services/vigia_fio.py:838`
- Datas 28/10 e 29/10 pertencem ambas à competência 11 do mesmo ano para qualquer corte permitido neste código: corte máximo 28; dia a partir do corte avança o mês. Isso não informa se a competência foi exportada. — `app/ponto/janelas.py:26`, `app/ponto/janelas.py:27`, `app/ponto/janelas.py:32`
- Conteúdo de `sonda_medida1.py`, seu cálculo da competência e seu uso efetivo de `marcos_da_competencia`: [não verificado].

## 12/11 10:35 · sessao 3e1d1c1a78a4 · - · - · origem=Ronald (tmux)

- `orfao_14h` pertence à família MARCO; sua identidade é pessoa + dia do turno + motivo + instância. A coluna `PerguntaDisputa.batida_origem` identifica episódios da família BATIDA, portanto não é uma fonte obrigatória nem garantida para `orfao_14h`. `app/chamados/catalogo/motivos.py:15`, `app/chamados/catalogo/motivos.py:34`, `app/chamados/models.py:2437`.
- O preenchimento automático da FK devolve `None` para motivos fora da família BATIDA. Logo excluir `orfao_14h` por FK nula exclui linhas por algo esperado no modelo, sem verificar se têm turno aberto no dia errado. `app/chamados/models.py:3453`, `app/chamados/models.py:3461`.
- O acervo ficou com FK nula deliberadamente: o contexto da disputa muda e não permite reconstruir retrospectivamente o episódio antigo. `app/chamados/migrations/0052_pergunta_batida_origem.py:1`.
- O argumento homônimo `resolver_data_evento(..., batida_origem=...)` significa a entrada aberta de `orfao_14h`; ele chama `dia_do_turno_da_batida`. Esse argumento não equivale à FK da pergunta. `app/escala/servico_jornada.py:495`, `app/escala/servico_jornada.py:499`, `app/escala/servico_jornada.py:506`.
- Um emissor encontra a entrada aberta por `entradas_pendentes[].pk` e passa a batida a esse argumento. A lista contém `_t.entrada.pk` de turnos abertos. `app/chamados/detector_anomalias.py:35`, `app/chamados/detector_anomalias.py:43`, `app/chamados/detector_anomalias.py:47`, `app/ponto/utils_turno_orfao.py:30`, `app/ponto/utils_turno_orfao.py:38`.
- Outro emissor encontra a entrada com `_achar_batida_origem(chamado)`, que consulta `turnos_do_colab`, seleciona turnos abertos com entrada anterior à criação do chamado e retorna a entrada candidata; o objeto é usado em `resolver_data_evento`, sem ser passado à criação da pergunta como FK. `app/chamados/services/materializacao.py:110`, `app/chamados/services/materializacao.py:124`, `app/chamados/services/materializacao.py:125`, `app/chamados/services/materializacao.py:140`, `app/chamados/services/disputa_emissao.py:1187`, `app/chamados/services/disputa_emissao.py:1205`, `app/chamados/services/disputa_emissao.py:1213`.
- A migração já identifica fonte de `orfao_14h` viva por turno com entrada e sem saída cujo último marco previsto cai em `data_evento`, exigindo candidato único, e retorna `t.entrada`; ela não depende de `p.batida_origem`. `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:82`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:108`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:115`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:120`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:126`.
- Para materializada, a migração usa batida viva com `pergunta_origem_id=p.pk`, encontra o turno por `pk` em `t.batidas` e exige exatamente um turno; após obter o dia pelo juiz, exclui explicitamente `orfao_14h` materializada como `fora_do_aval`. `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:65`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:76`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:138`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:160`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:168`.
- Não foi possível auditar filtros e ordem dos contadores da sonda: seu conteúdo não está no dado recebido nem foi localizado na cópia. [não verificado]

## 12/11 10:25 · sessao 9a2b122ced99 · TRIAGEM-OFFLINE-RED-HEADLESS · construtor · origem=Code

Criado [test_sw_offline_headless_red.py](/home/ronald/wt-codex-triagem-offline-red-headless-101137/app/core/tests/test_sw_offline_headless_red.py:1), somente em `core/tests/`. Não executei testes, Chromium ou Ruff; resultados abaixo são expectativas **[nao verificado]**. [nao verificado]

O harness original usa `file://`, incompatível com este registro de SW; o ajuste ficou no teste novo: servidor HTTP em localhost e CDP para clique real e corte de rede nas abas e no worker. [Harness original](/home/ronald/wt-codex-triagem-offline-red-headless-101137/app/core/tests/test_smoke_chromium.py:115), [ajuste](/home/ronald/wt-codex-triagem-offline-red-headless-101137/app/core/tests/test_sw_offline_headless_red.py:195), [offline](/home/ronald/wt-codex-triagem-offline-red-headless-101137/app/core/tests/test_sw_offline_headless_red.py:464). [nao verificado]

A garantia da **FONTE** vem da leitura de `SW`, do redirecionamento da raiz da view real e da comparação do corpo HTTP com fonte + build. O mutante altera apenas uma cópia temporária. [Leitura e isolamento](/home/ronald/wt-codex-triagem-offline-red-headless-101137/app/core/tests/test_sw_offline_headless_red.py:226), [comparação pelo navegador](/home/ronald/wt-codex-triagem-offline-red-headless-101137/app/core/tests/test_sw_offline_headless_red.py:437).

Comandos para a sessão, a partir de `app/`:

```bash
MUTANTE_SW= PYTHONDONTWRITEBYTECODE=1 python manage.py test core.tests.test_sw_offline_headless_red --settings=config.settings_test --parallel=1 -v 2
```

Esperado: controle verde; offline RED com `PASSO 3 OFFLINE C1: clique Bater ponto deve entregar documento /ponto/bater-v2/` e `PASSO 4 OFFLINE C1: reload da home deve entregar documento nao-branco`. O reload continua sendo tentado após a falha do clique. [Asserções](/home/ronald/wt-codex-triagem-offline-red-headless-101137/app/core/tests/test_sw_offline_headless_red.py:490). **[nao verificado]** [nao verificado]

```bash
MUTANTE_SW= PYTHONDONTWRITEBYTECODE=1 python manage.py test core.tests.test_sw_offline_headless_red.SWOfflineHeadlessRedTest.test_controle_com_rede_home_e_bater --settings=config.settings_test --parallel=1 -v 2
```

Esperado: verde com rede. [Controle](/home/ronald/wt-codex-triagem-offline-red-headless-101137/app/core/tests/test_sw_offline_headless_red.py:443). **[nao verificado]** [nao verificado]

```bash
MUTANTE_SW=navega_guarda PYTHONDONTWRITEBYTECODE=1 python manage.py test core.tests.test_sw_offline_headless_red --settings=config.settings_test --parallel=1 -v 2
```

Esperado: controle e offline verdes; o mutante aguarda `cache.put` no ramo `navigate`. [Trecho do mutante](/home/ronald/wt-codex-triagem-offline-red-headless-101137/app/core/tests/test_sw_offline_headless_red.py:70). **[nao verificado]**

```bash
PYTHONDONTWRITEBYTECODE=1 python -m ruff check core/tests/test_sw_offline_headless_red.py
```

Escritores e leitores das dependências:

| Dado | Quem escreve/define | Quem lê/consome |
|---|---|---|
| Registro do SW | `static/pwa.js:14` | Inicialização em `static/pwa.js:324`; teste em `core/tests/test_sw_offline_headless_red.py:354` |
| Scope `/` | `static/pwa.js:14`; `core/views.py:574` | `static/sw.js:23`; teste em `core/tests/test_sw_offline_headless_red.py:374` |
| Build/cache | `core/views.py:555`, `:572`; `static/sw.js:1` | `static/sw.js:12`, `:20`, `:74`; teste em `core/tests/test_sw_offline_headless_red.py:440` |
| Home efetiva | `core/urls.py:17`; `core/views.py:270` | Teste em `core/tests/test_sw_offline_headless_red.py:408` |
| Link Bater ponto | `templates/core/dashboard_colaborador_v2.html:31`; `ponto/urls.py:12` | Clique em `core/tests/test_sw_offline_headless_red.py:381`; render em `ponto/views.py:356`; elemento em `templates/ponto/bater_ponto_v2.html:109` |

Os dois modos de sessão são exercitados, mas essas rotas herdam literalmente `base.html`; cobertura de `base_app.html` não é demonstrada aqui. (`templates/core/dashboard_colaborador_v2.html:1`; `templates/ponto/bater_ponto_v2.html:1`; `core/tests/test_sw_offline_headless_red.py:397`.)

## 12/11 10:24 · sessao 08ce782c30e6 · BUG-CADASTRO-NAO-ALCANCA-O-FUTURO · verificador · origem=Code

**BLOQUEIA — o admin de template passa a alcançar o passado, há portas sem atomicidade entre cadastro e cura, e a suíte em memória contém um erro antes do julgamento do feriado.** (`app/escala/services/cadastro_tipo.py:337`, `app/escala/services/cadastro_tipo.py:373`, `app/colaboradores/services/vinculo.py:326`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:599`)

Identidade do hash/base por Git, estado atual de produção e execução da suíte Django: [nao verificado]. As comparações com `087fe758a` abaixo usam o diff fornecido, sem certificar sua completude por Git: [nao verificado].

1. Para as portas do censo, o transporte comum seleciona células **existentes, geradas, estritamente futuras, dentro da janela corrente do gerador**, e chama a mesma porta com `[dia,dia]`, sem estender horizonte. (`app/ponto/services/cadastro_futuro.py:18`, `app/ponto/services/cadastro_futuro.py:22`, `app/ponto/services/cadastro_futuro.py:30`)

Na tabela, **G** significa mutante que recoloca `data_inicio <= hoje`; **M** significa mutante que remove a chamada da cura. Ambos modificam em memória o corpo lido da produção; somente cinco funções recebem G. (`app/ponto/tests/test_cadastro_futuro_sem_banco.py:25`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:73`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:87`)

| Porta do censo | Sítio que alcança o futuro | RED e achado |
|---|---|---|
| Escala/vínculo — `executar_vinculo` | `app/colaboradores/services/vinculo.py:326` | G morto pelo teste `app/ponto/tests/test_cadastro_futuro_sem_banco.py:339`. **BLOQUEIA:** cura fora do atomic do cadastro, detalhada no item 4. |
| Flag `trabalha_em_feriado` | `app/colaboradores/services/vinculo.py:878` | G morto em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:352`. **BLOQUEIA:** save e cura separados. |
| Município do posto | `app/colaboradores/services/vinculo.py:928` | G morto em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:356`. **BLOQUEIA:** save e cura separados. |
| Template — `salvar_tipo` | `app/escala/services/cadastro_tipo.py:361` | G morto em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:361`. **BLOQUEIA:** universo limitado a `ativa=True`, em `app/escala/services/cadastro_tipo.py:334`. |
| Template pelo admin | `app/escala/admin.py:12`, `app/escala/services/cadastro_tipo.py:373` | Teste de amanhã em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:392`. **BLOQUEIA:** passou a regenerar também o passado, em `app/escala/services/cadastro_tipo.py:348`. |
| Signal de vigência | `app/escala/signals.py:77` | M morto em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:576`; G específico não demonstrado pelo mutador. |
| Signal de âncora | `app/escala/signals.py:128` | M morto em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:569`; G específico não demonstrado pelo mutador. |
| Foto mensal de folgas | `app/escala/folgas.py:215`, `app/escala/folgas.py:298` | M da relavra morto em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:584`; reconciliação mensal cobrada em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:612`. |
| Lançar folga | `app/escala/folgas.py:337` → relavra comum | **BLOQUEIA por evidência incompleta:** sem RED individual “amanhã + mutante” demonstrado; o novo teste chama diretamente a relavra, em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:586`. |
| Alterar/remover folga | `app/escala/folgas.py:380` → relavra comum | **BLOQUEIA por evidência incompleta:** teste anterior cobra idempotência, não a guarda futura por essa entrada: `app/escala/tests/test_bug136_plano_em_lote_relavra.py:105`. |
| Folgas em lote | `app/escala/folgas.py:418` → relavra comum | **BLOQUEIA por evidência incompleta:** teste anterior usa o início da competência; não demonstra o mutante desta obra: `app/escala/tests/test_bug136_plano_em_lote_relavra.py:68`. |
| Reparentar folga | `app/escala/folgas.py:523` → relavra comum | **BLOQUEIA por evidência incompleta:** há RED anterior de rejulgamento mensal, sem mordida futura desta obra demonstrada: `app/escala/tests/test_f3_beco1_cadastro_entrou.py:201`. |
| Aplicar calendário por tipo | `app/escala/views.py:409`, `app/escala/views.py:416` | **BLOQUEIA por evidência incompleta:** teste anterior cobra FK/foto, sem conferir célula futura sob mutante: `app/escala/tests/test_folga_aplicar.py:39`. |
| Editor de calendário | `app/escala/folgas.py:598` | M morto em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:509`. |
| Trocar calendário do vínculo | `app/colaboradores/services/vinculo.py:454` | M morto em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:497`. |
| Cadastrar/editar/importar feriado | Saves de formulário/importação atravessam o signal: `app/core/views_config.py:68`, `app/core/views_config.py:81`, `app/core/views_config.py:141`, `app/core/signals.py:29` | M do signal morto em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:429`. **BLOQUEIA:** prova específica de obedecer ao silêncio da juíza dá ERROR antes da chamada: `app/ponto/tests/test_cadastro_futuro_sem_banco.py:599`. |
| Excluir feriado | `app/core/signals.py:35` | M morto em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:435`. |
| Fechar vigência por `update` | `app/colaboradores/services/vinculo.py:639` | M morto em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:366`. **BLOQUEIA:** encerramento sem sucessor deixa contador positivo, detalhado no item 5. |
| Recusa/restauração de snapshot | `app/colaboradores/services/vinculo.py:997` | M morto em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:448`. |
| Correção retroativa com piso amanhã | `app/colaboradores/management/commands/corrigir_escala_retroativa.py:243` | G morto em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:517`. |
| ESCALA-AUTO — aplicar/desfazer | Chamada direta à mesma porta, ainda com horizonte: `app/escala/services/escala_auto_executor.py:104`, `app/escala/services/escala_auto_executor.py:143` | **FILA:** comportamento anterior mantido; sem RED desta obra por mutante demonstrado. Os testes existentes conferem cadastro/passado: `app/escala/tests/test_escala_auto_executor.py:66`, `app/escala/tests/test_escala_auto_executor.py:117`. |

**BLOQUEIA — template não cobre todo vínculo que responde pelo futuro.** Contra-exemplo: hoje 12/11; vínculo inativo, mas com fim em 20/11; célula gerada de 13/11 nomeando esse vínculo. A juíza aceita a geradora dentro da vigência, mas a propagação do template a exclui por `ativa=True`; mudar 08:00 para 09:00 deixa o DNA antigo. (`app/escala/alimentacao.py:282`, `app/escala/services/cadastro_tipo.py:334`)

**FILA — demais entradas encontradas pelo censo:** troca de `Colaborador.posto` não muda o posto declarado no vínculo; `Posto.opera_em_feriado` define o padrão de vínculos novos; reclassificação grava apenas `tipo_base`. Não há suplemento nesses caminhos; não ficou comprovada mudança de célula vigente exigida por eles. (`app/colaboradores/services/colaborador.py:47`, `app/escala/models.py:905`, `app/colaboradores/services/estrutura.py:80`, `app/escala/services/cadastro_tipo.py:134`)

**FILA — portas técnicas adicionais:** correção de um dia continua sem horizonte; regeneração em exportada mantém horizonte apenas para vínculo ativo; cura BUG115 chama o gerador diretamente. Não ganharam RED específico desta obra demonstrado. (`app/ponto/portas/celula.py:734`, `app/ponto/portas/regen_exportada.py:174`, `app/ponto/management/commands/curar_residuo_bug115.py:135`)

2. Nos contra-exemplos do aval:

- **Nada — suplemento não cria célula fora da janela nem preenche lacunas futuras:** ele percorre somente datas existentes e desliga o horizonte. O teste isolado “início além da janela” passa; trocar para horizonte habilitado é morto pelos testes que detectam novas células entre 14 e 20/11. (`app/ponto/services/cadastro_futuro.py:22`, `app/ponto/services/cadastro_futuro.py:31`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:374`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:386`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:584`)
- **FILA — a garantia não vale universalmente para AUTO:** `_ate` retorna pelo menos o início, mesmo sem célula; início além da janela cria aquela célula. É comportamento anterior preservado, não introduzido pelo suplemento. (`app/escala/services/escala_auto_executor.py:52`, `app/escala/services/escala_auto_executor.py:104`, `app/ponto/portas/celula.py:259`)
- **Nada — `origem='editada'` fica fora da cura explícita:** seleção e escritor a preservam; snapshot e desfazer agora recusam apagar sua geradora. (`app/ponto/services/cadastro_futuro.py:23`, `app/ponto/portas/celula.py:263`, `app/colaboradores/services/vinculo.py:990`, `app/colaboradores/services/vinculo.py:1062`)
- **BLOQUEIA — admin de template amplia o passado:** o hook novo chama `_propagar_regeneracao_template`, que começa na competência anterior e regenera até hoje. Contra-exemplo: célula gerada de ontem de vínculo ativo; editar horário pelo admin agora reescreve seu DNA, embora essa entrada antes apenas salvasse o template no diff fornecido. (`app/escala/admin.py:12`, `app/escala/services/cadastro_tipo.py:337`, `app/escala/services/cadastro_tipo.py:348`, `app/escala/services/cadastro_tipo.py:373`)
- **BLOQUEIA — “só futuro” não limita todos os efeitos de escrita:** `fechar_vigencia` ganhou regeneração com recálculo padrão; a cauda chama o fechamento da competência inteira e este grava fechamento/dias pagos. Encerrar hoje, tocando amanhã, pode relavrar dias anteriores da mesma competência por esse caminho novo. Movimento efetivo de valores em produção: [nao verificado]. (`app/colaboradores/services/vinculo.py:639`, `app/ponto/portas/celula.py:445`, `app/ponto/portas/celula.py:458`, `app/ponto/services/fechamento.py:1810`, `app/ponto/services/fechamento.py:1144`, `app/ponto/services/fechamento.py:1201`)
- **Nada — cron continua cadastrado às 05:50, cria faltantes e deixou de reescrever existentes de hoje/futuro; preserva editadas e ainda enriquece DNA histórico nulo.** Não há desligamento nesse caminho. (`app/config/crons.py:426`, `app/ponto/portas/celula.py:853`, `app/ponto/portas/celula.py:859`, `app/ponto/portas/celula.py:864`, `app/ponto/portas/celula.py:870`)
- **BLOQUEIA — a retirada alcançou também hoje:** feriado cadastrado para hoje é excluído pelo suplemento; o cron novo também pula a célula existente de hoje. Contra-exemplo: célula de 12/11 `trabalha=True`; salvar feriado de 12/11 não a cura por esses dois caminhos, enquanto o ramo removido do cron a reescrevia. (`app/ponto/services/cadastro_futuro.py:44`, `app/ponto/portas/celula.py:861`, `app/ponto/portas/celula.py:870`)

3. Escritores, leitores e significados conferidos:

| Campo/valor | Quem escreve ou produz | Quem lê |
|---|---|---|
| `escala_geradora` | Regeneração, edição humana, reversão e criação do cron; há também `SET_NULL` implícito ao excluir vínculo. (`app/ponto/portas/celula.py:259`, `app/ponto/portas/celula.py:272`, `app/ponto/portas/celula.py:600`, `app/ponto/portas/celula.py:791`, `app/ponto/portas/celula.py:854`, `app/escala/models.py:1693`) | Juíza, carregadores, feriado e vigia. (`app/escala/alimentacao.py:270`, `app/escala/alimentacao.py:356`, `app/ponto/services/cadastro_futuro.py:53`, `app/ponto/services/vigia_cadastro_futuro.py:28`) |
| `dna` | Produtor `montar_dna`; persistência pela porta nas operações acima e no enriquecimento histórico. (`app/ponto/management/commands/gerar_celulas.py:73`, `app/ponto/portas/celula.py:256`, `app/ponto/portas/celula.py:865`) | Marcos, pausas e extra do vínculo; grade; cartório; juízes; tripwire; vigia. (`app/escala/models.py:1124`, `app/escala/models.py:1189`, `app/escala/models.py:1235`, `app/escala/services/leitor_celula.py:398`, `app/ponto/services/cartorio.py:1251`, `app/chamados/juizes.py:948`, `app/ponto/management/commands/tripwire_celulas.py:30`, `app/ponto/services/vigia_cadastro_futuro.py:40`) |
| `trabalha` | Cálculo cadastral alimenta regeneração/cron; edição e reversão também escrevem. (`app/ponto/portas/celula.py:255`, `app/ponto/portas/celula.py:272`, `app/ponto/portas/celula.py:600`, `app/ponto/portas/celula.py:788`, `app/ponto/portas/celula.py:856`) | Método do vínculo, semana do posto, precedência, juízes e suprajuiz. (`app/escala/models.py:1095`, `app/escala/services/semana_do_posto.py:85`, `app/ponto/precedencia.py:348`, `app/chamados/juizes.py:455`, `app/ponto/supra_juiz.py:62`) |
| `origem` | Criação grava `gerada`; edição humana grava `editada`. (`app/ponto/portas/celula.py:260`, `app/ponto/portas/celula.py:603`, `app/ponto/portas/celula.py:856`) | Guardas da regeneração, reversão, cron, suplemento e exclusões protegidas. (`app/ponto/portas/celula.py:238`, `app/ponto/portas/celula.py:263`, `app/ponto/portas/celula.py:777`, `app/ponto/portas/celula.py:859`, `app/ponto/services/cadastro_futuro.py:23`, `app/colaboradores/services/vinculo.py:990`) |
| `regenerada_em`, `regeneracoes`, `dna_anterior` | Regeneração e reversão; reversão consome e limpa o DNA anterior. (`app/ponto/portas/celula.py:273`, `app/ponto/portas/celula.py:787`, `app/ponto/portas/celula.py:792`) | Cobrança órfã, ponte do dia, cartório, invalidadores e comandos de correção/medição. (`app/escala/services/regeneracao.py:104`, `app/escala/services/regeneracao.py:119`, `app/ponto/services/dia_do_colab.py:94`, `app/ponto/services/cartorio.py:393`, `app/chamados/veredito_lavrado.py:193`, `app/escala/services/fase_12x36.py:189`, `app/ponto/management/commands/aplicar_col900.py:173`, `bin/r5_idempotencia_frota.py:57`) |
| `FolgaDia`, `calendario_origem` | Foto automática grava calendário; lançamento manual deixa origem nula; reparentamento troca o vínculo. (`app/escala/folgas.py:194`, `app/escala/folgas.py:335`, `app/escala/folgas.py:360`, `app/escala/folgas.py:515`) | Reconciliação distingue manual/calendário; gerador lê foto do vínculo; precedência/grade aceitam manual da pessoa e calendário do vínculo do dia. (`app/escala/folgas.py:180`, `app/escala/folgas.py:187`, `app/escala/models.py:1286`, `app/escala/models.py:1356`, `app/ponto/precedencia.py:237`) |
| `vinculo_do_dia` | Resposta produzida pela autoridade existente; carregadores apenas montam alimentação. (`app/escala/alimentacao.py:228`, `app/escala/alimentacao.py:358`) | Feriado, vigia, reparentamento, calendário, grade e fechamento. (`app/ponto/services/cadastro_futuro.py:54`, `app/ponto/services/vigia_cadastro_futuro.py:31`, `app/escala/folgas.py:493`, `app/colaboradores/services/calendario.py:389`, `app/escala/services/leitor_celula.py:411`, `app/ponto/services/fechamento.py:191`) |
| `hoje` | Cron usa `date.today`; serviços usam `timezone.localdate`; porta também possui leituras internas. (`app/ponto/management/commands/gerar_celulas.py:304`, `app/ponto/services/cadastro_futuro.py:16`, `app/escala/folgas.py:280`, `app/ponto/portas/celula.py:186`, `app/ponto/portas/celula.py:296`) | Seleção futura, horizonte, julgamento e vigia. (`app/ponto/services/cadastro_futuro.py:18`, `app/ponto/portas/celula.py:190`, `app/ponto/portas/celula.py:431`, `app/ponto/services/vigia_cadastro_futuro.py:20`) |

**Nada — divergência nova de limites inclusivos:** início e fim são inclusivos nas duas autoridades; lâmpadas noturnas deslocam instantes para D+1, mantendo a chave do turno; feriado é consultado pela data recebida do dia de escala. (`app/escala/alimentacao.py:214`, `app/escala/alimentacao.py:282`, `app/ponto/management/commands/gerar_celulas.py:67`, `app/escala/models.py:1395`)

**FILA — calendário nulo tem documentação ambígua:** a porta descreve `None` como padrão do template, mas o sincronizador lê a FK literal e o interpretador trata `None` como calendário vazio; a view resolve o padrão explicitamente. (`app/colaboradores/services/vinculo.py:441`, `app/escala/folgas.py:625`, `app/escala/folgas.py:52`, `app/colaboradores/views.py:236`)

**BLOQUEIA — nem todos usam o mesmo significado de cobertura:** geradora carimbada inativa sem fim é aceita por `vinculo_do_dia`, mas rejeitada por `vinculo_cobre_a_data` e pela regeneração. Esse desacordo atinge diretamente o contador novo. (`app/escala/alimentacao.py:196`, `app/escala/alimentacao.py:214`, `app/escala/alimentacao.py:282`, `app/ponto/portas/celula.py:146`)

4. Nas classes de reprovação:

- **Nada — segundo juiz novo no feriado/vigia:** ambos perguntam à autoridade registrada; o desacordo de cobertura acima é entre autoridades existentes. (`app/ponto/services/cadastro_futuro.py:54`, `app/ponto/services/vigia_cadastro_futuro.py:31`, `app/core/juizes.py:1304`, `app/core/juizes.py:1305`)
- **Nada — derivação paralela nova de previsto/GRADE:** vigia compara trabalho/DNA pelos mesmos produtores usados pelo escritor; não calcula minutos previstos por uma regra nova. (`app/ponto/services/vigia_cadastro_futuro.py:39`, `app/ponto/portas/celula.py:255`)
- **FILA — segundo escritor implícito permanece:** excluir vínculo aciona `SET_NULL` fora da porta. As duas exclusões explícitas protegidas agora recusam referências editadas; referências geradas históricas continuam sujeitas ao comportamento anterior. (`app/escala/models.py:1693`, `app/colaboradores/services/vinculo.py:988`, `app/colaboradores/services/vinculo.py:992`, `app/colaboradores/services/vinculo.py:1064`)
- **FILA — reescrita sem denúncia permanece:** fora da vigência, o escritor altera apenas `trabalha/dna`, sem os três carimbos. Contra-exemplo: encerrar hoje uma geradora de amanhã sem sucessor apaga os marcos sem registrar essa regeneração nos campos que os leitores consultam; dívida anterior já documentada. (`app/ponto/portas/celula.py:238`, `app/ponto/portas/celula.py:245`, `app/docs/LEIS.md:163`, `app/escala/services/regeneracao.py:119`)
- **FILA — fallbacks anteriores permanecem:** trabalho sem célula; marcos com DNA/marcos nulos mesmo havendo célula; pausas/extra sem célula; vínculo pela vigência quando não há geradora. Nada novo nesses fallbacks no diff fornecido. (`app/escala/models.py:1096`, `app/escala/models.py:1133`, `app/escala/models.py:1208`, `app/escala/models.py:1239`, `app/escala/alimentacao.py:285`)
- **FILA — relógio fora da borda:** os novos serviços possuem `localdate` próprio; rejulgamento e cauda releem o relógio apesar de receberem dias derivados de `hoje`. Django usa São Paulo; fuso do SO em produção: [nao verificado]. Contra-exemplo condicional: SO em UTC já está em 13/11, enquanto São Paulo ainda está em 12/11. (`app/ponto/services/cadastro_futuro.py:16`, `app/ponto/services/cadastro_futuro.py:42`, `app/escala/folgas.py:616`, `app/ponto/portas/celula.py:296`, `app/ponto/portas/celula.py:426`, `app/config/settings/base.py:93`)
- **BLOQUEIA — cura e cadastro não compartilham atomicidade em todas as portas:** vínculo sai do atomic antes da cura e engole falha; template salva antes da propagação e também engole falha; flag de feriado, município e formulário de feriado salvam antes da cura sem atomic abrangente próprio. Contra-exemplo: falha na regeneração após o save deixa cadastro novo e célula antiga, com sucesso possível nos caminhos que engolem a exceção. (`app/colaboradores/services/vinculo.py:188`, `app/colaboradores/services/vinculo.py:299`, `app/colaboradores/services/vinculo.py:331`, `app/escala/services/cadastro_tipo.py:306`, `app/escala/services/cadastro_tipo.py:322`, `app/colaboradores/services/vinculo.py:862`, `app/colaboradores/services/estrutura.py:68`, `app/core/views_config.py:68`)
- **Nada — nas portas com atomic abrangente, a escrita futura é síncrona:** calendário do vínculo, fechamento, snapshot, folgas e admin abrangem a cura. `on_commit` agenda efeitos posteriores, não a escrita futura do DNA. (`app/colaboradores/services/vinculo.py:450`, `app/colaboradores/services/vinculo.py:623`, `app/colaboradores/services/vinculo.py:984`, `app/escala/folgas.py:332`, `app/escala/services/cadastro_tipo.py:369`, `app/ponto/services/cadastro_futuro.py:21`, `app/ponto/portas/celula.py:502`)
- **BLOQUEIA — ordem inversa de travas:** snapshot trava célula C e depois exclui vínculo E; fechamento atualiza E e 

[truncado em 20000 de 24767 caracteres]

_4174 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
