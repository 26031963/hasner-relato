# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 19:55** · publicado em **12/11/2026 19:57** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 19:55 · sessao 67844783dc07 · TRANCA-VIVAS-08 · censo · origem=Code

Numeração dos anexos transcritos: primeira linha de cada corpo = 1, incluindo linhas vazias.

1. **(b) Codex viu; a triagem descartou — agregado com significado indevido e cobertura insuficiente.**  
   **Pergunta:** “Consumidor da mensageria […] muda de significado?” e “campo com dois significados”; também perguntou sobre “selo/RED sem mordida”. (`preverif.q:17,19-20`)  
   **Resposta:** identificou que o CLI publica a união como `decidir_pelo_dp`, enquanto a mensageria descreve “resposta do colaborador sem veredito”; apontou ainda: “Falta […] um caso nominal de `vivas_resgatadas` e da união sem duplicatas”. (`preverif.r:1,116`)  
   **Motivos escritos na triagem:** rótulo descartado para FILA porque a “descricao ja incompleta antes da obra; consumidor fora do compose”; cobertura descartada porque “resgate defendido por test_competencia_trancada.py:164/:177; mudar teste agora invalida a arvore certificada”. (`triagem.txt:2,6`)   [nao verificado]
   A execução específica do “mutante soma passa” consta do laudo, mas sua demonstração não foi fornecida: **[não verificado]**. (`opus_final.md:2`)

2. **(b) Codex viu; a triagem descartou — `resgatado_em` também decorre de resposta do colaborador.**  
   **Pergunta:** “As exclusoes (resgatado, tocado por admin, conversa) usam o discriminador existente ou reimplementam regra?”. (`preverif.q:15-16`)  
   **Resposta:** “`resgatado_em` não significa exclusivamente ‘resgatado pelo admin’”, pois a porta aceita resgate por resposta do colaborador. (`preverif.r:3`)  
   **Motivo escrito na triagem:** “discriminador existente, sem juiz novo”. (`triagem.txt:3`)  
   O código confirma o caminho por resposta e o docstring restrito ao admin; o “em prod 0” permanece **[não verificado]**. (`app/chamados/services/materializacao.py:724-727`; `app/chamados/services/competencia_trancada.py:75`; `opus_final.md:3`)

3. **(b) Codex viu; a triagem descartou — placar não lê o residual.**  
   **Pergunta:** “liste TODOS os escritores e leitores no archive, inclusive fora do diff”, abrangendo o contador `vivas_apesar_da_tranca`. (`preverif.q:8-9`)  
   **Resposta:** “O placar antigo **não lê o residual novo**: extrai somente a soma `chamados + perguntas + disputas`”. (`preverif.r:98`)  
   **Motivo escrito na triagem:** “FILA (instrumento, pouso proprio)”. (`triagem.txt:5`) [nao verificado]

4. **(a) Codex não apontou o achado documental, com o tema presente — assinatura do ato em JUIZES.md.**  
   **Pergunta:** anexou expressamente `app/docs/JUIZES.md` e perguntou se a mudança distingue “via de sistema” de “ato humano legitimo”. (`preverif.q:3,12-14`)  
   **Resposta:** reconheceu o predicado “aceita `via='admin'` **ou presença** de `acoes`”, mas tratou o registro apenas como existente: “C20 e sua relação com C6/tarefa já constam do registro e lookup”. Não apontou nesses trechos a necessidade de atualizar sua descrição. (`preverif.r:15,108`)  
   Classifico como omissão de tema presente: o registro mantém “usuario humano, nunca o sistema”, enquanto o filtro exige a assinatura do ato; essa atualização documental é precisamente o achado do opus. (`app/docs/JUIZES.md:88`; `app/chamados/juizes.py:1298`; `opus_final.md:5`)

5. **(c) Fora do alcance — prova de produção pós-cura.**  
   **Pergunta:** pediu o universo do contador e o censo de leitores. (`preverif.q:8-9,15-16`)  
   **Resposta:** identificou os leitores C20 em tarefa do dia e reconciliador, mas declarou: “A medição pós-cura da frota, universo não vazio, residual zero e publicação no RELATO: **[não verificado]**”. Portanto, conhecer os leitores não demonstrou seu impacto em produção. (`preverif.r:35-36,120`)  
   **Triagem:** “fora do alcance do Codex (frota/sonda_frota.r)”. As lacunas específicas sobre banco, quatro alvos e `chamados=207` são relatadas pelo opus; seus resultados não estão demonstrados no dado fornecido: **[não verificado]**. (`triagem.txt:7`; `opus_final.md:6`)

6. **(c) Fora do alcance quanto aos usuários de produção; os três atos já foram reconhecidos.**  
   **Pergunta:** perguntou sobre alcance em ato humano legítimo e leitura do histórico antigo. (`preverif.q:13-14`)  
   **Resposta:** enumerou os três fundamentos — cobrar, Resolver dia e desarquivar — e ressalvou: “Que todos os atos humanos antigos possuíam essas assinaturas: **[não verificado]**”. (`preverif.r:24,26`)  
   O código confirma o conjunto de três atos. Já as identidades/naturezas dos usuários 657/653 aparecem apenas na referência do opus à `sonda_v4.r`, cujo conteúdo não foi fornecido: **[não verificado]**. (`app/chamados/juizes.py:1242-1250`; `opus_final.md:7`)

**Contagem por classe:** **(a) 1; (b) 3; (c) 2; (e) 0; (d) 0. Total: 6 achados.** (`preverif.q:3,8-20`; `preverif.r:1,3,15,24,26,35-36,98,108,116,120`; `triagem.txt:2-7`; `opus_final.md:2-7`)

## 12/11 19:55 · sessao b923d43079e6 · E3-FORMA-SQL · sonda · origem=Code

Não executei novamente `sonda_espacos.py`: o script não foi localizado na cópia e não há acesso a banco/logs. Execução nova e identidade Git do HEAD: [nao verificado]. Abaixo está a transcrição integral do dado fornecido, incluindo o anexo; sua execução não foi verificada independentemente. [nao verificado] [nao verificado]

```text
sonda_leitura: sonda_espacos.py [71d433320633ce76] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

q_vivas_total=14548  q_vivas_com_carimbo_e_so_espacos=0
carimbo__texto                                7976  amostra=[53332, 4938, 4944, 5000, 52249, 54324, 36412, 53521, 16369, 36272]
carimbo__vazio                                9626  amostra=[48306, 48314, 4880, 4882, 4884, 48318, 4954, 7897, 5022, 5243]
sem_carimbo__vazio                           31556  amostra=[46957, 43188, 43875, 53869, 37786, 40504, 41545, 49486, 40745, 39156]
universo                                     49158  amostra=[]
total_erros=0
== anexo: e3_forma_sql_do_juiz.txt ==
aval Ronald: E3-FORMA-SQL-DO-JUIZ (12/11 19:1x). UNIDADE: PerguntaDisputa. Nenhuma lei nova, nenhum juiz novo; responde a PERGUNTA DE DESENHO E3 porta A (1)(2).
CONFERIDO: o juiz existe -- chamados/juizes.py:1515 respondida_sem_texto, com texto_vazio = (t or '').strip() (:1510); a forma SQL inline em models.py:3349-3350 (q_vivas) NAO e o mesmo predicado: so testa '' e NULL, e resposta so de espacos passa como viva. Derivacao paralela divergente = BUG no caminho (LEI-AKITA 6).
(1) A forma Q nasce como a FACE SQL do juiz respondida_sem_texto, no mesmo modulo, com o mesmo teste de vazio (trim); q_vivas:3349 e os 9 sitios `respondida_em__isnull` leem ela. Selo de equivalencia por propriedade: juiz Python == Q sobre o mesmo universo, incluindo '', '   ', NULL, texto; mutante (tirar o trim) = vermelho. Nao e juiz novo: e a mesma pergunta, declarada uma vez com duas faces (molde qs_perguntas_absolviveis). [nao verificado]
(2) Catraca pela forma de USO, nunca por lista: respondida_em em contexto booleano (isnull, is None, truthiness, filtro) = "respondeu?" e passa pelo juiz/face SQL; respondida_em lido como VALOR (instante, ordenacao, trilha) fica fora. Os 4 sitios (juizes.py:348, verbos.py:342, veredito_lavrado.py:207, backfill/DecisaoPergunta) sao julgados por essa regra no AST; o que for booleano migra.
(3) Os 17 que movem passivo: medida na sombra antes, como ja manda o aval E3-10 item 3; o 8.622 declara a UNIDADE e o recorte contra os 142 do E3-10. App do colab muda ou supra_juiz retrata = PARA e publica a lista.
LEI-AKITA: origem=chamados/juizes.py::respondida_sem_texto (face SQL), testemunha=o juiz, RED=resposta so de espacos viva no SQL e vazia no Python, quem-mais-le=9 sitios SQL + q_vivas + 4 de instante, juizes novos=0, padrao=Single Source of Truth
Segue a fila; PAREI so em lei ou !
```

Na aplicação, os escritores explícitos encontrados são estes:

| Escritor | Onde escreve e persiste | Pode gravar somente espaços com `respondida_em` preenchido? |
|---|---|---|
| `responder_pergunta_disputa_core` | Atribui texto/carimbo em [disputa_emissao.py:1467](app/chamados/services/disputa_emissao.py:1467); salva em [disputa_emissao.py:1508](app/chamados/services/disputa_emissao.py:1508). | **Não para a entrada recebida.** Faz `resposta = (resposta or '').strip()` e rejeita vazio **antes da atribuição e do save**, em [disputa_emissao.py:1432](app/chamados/services/disputa_emissao.py:1432). |
| `responder_automaticamente` | Atribui `texto` e `instante` em [disputa_emissao.py:1652](app/chamados/services/disputa_emissao.py:1652); salva ambos em [disputa_emissao.py:1655](app/chamados/services/disputa_emissao.py:1655). | **Sim, a função permite**, recebendo `texto='   '` e instante preenchido, quando a guarda permite prosseguir. O `strip()` de [disputa_emissao.py:1647](app/chamados/services/disputa_emissao.py:1647) verifica a **resposta antiga**; o novo `texto` é gravado sem trim nem rejeição de vazio. |
| `reabrir_para_correcao` | Define `resposta_colab=''` e `respondida_em=None` em [disputa_emissao.py:1593](app/chamados/services/disputa_emissao.py:1593); persiste por `descarimbar_em_lote` ou `qs.update` em [disputa_emissao.py:1630](app/chamados/services/disputa_emissao.py:1630). | **Não.** Escreve vazio literal e remove o carimbo; não depende de trim. |

O chamador operacional localizado de `responder_automaticamente` passa `_TEXTO = '[auto] Foto recebida em upload separado.'`, definido em [foto_recebida.py:25](app/api/foto_recebida.py:25), com `agora` obtido em [foto_recebida.py:42](app/api/foto_recebida.py:42), na chamada de [foto_recebida.py:48](app/api/foto_recebida.py:48). Essa chamada fornece texto não vazio; atribuir a ela uma ocorrência real de resposta só de espaços é [nao verificado].

Há também caminhos genéricos de persistência e o escritor da sombra, relevantes para não tratar a proteção do core como garantia de toda escrita:

| Caminho | Escrita e tratamento de espaços |
|---|---|
| Nascimento por `abrir` / `abrir_conversa` → `_nascer` | Recebem `**campos` em [models.py:2777](app/chamados/models.py:2777) e [models.py:2795](app/chamados/models.py:2795); `_nascer` os passa ao construtor e salva em [models.py:3015](app/chamados/models.py:3015). Não há trim nesse trecho: um par fornecido como `resposta_colab='   ', respondida_em=instante` chega bruto ao save. Uso operacional desse par por essa entrada: [nao verificado]. |
| `PerguntaDisputa.save` | Em [models.py:3414](app/chamados/models.py:3414), verifica identidade na inclusão e chama `super().save()` em [models.py:3424](app/chamados/models.py:3424), sem normalizar a resposta. Portanto, esse método não barra o par espaços/carimbo recebido em memória. |
| `descarimbar_em_lote` | Aceita `**outros` em [models.py:3253](app/chamados/models.py:3253) e os repassa ao `update` em [models.py:3265](app/chamados/models.py:3265), sem trim. A interface permite fornecer resposta/carimbo por esses argumentos; a reabertura efetiva fornece `''`/`None`, conforme [disputa_emissao.py:1593](app/chamados/services/disputa_emissao.py:1593). Uso operacional para fornecer espaços/carimbo: [nao verificado]. |
| Máscara da sombra | O inventário inclui `PerguntaDisputa.resposta_colab` em [sombra_regras_pessoais.py:85](bin/sombra_regras_pessoais.py:85). O executor atribui por `setattr` em [sombra_mascara_pessoal.py:57](bin/sombra_mascara_pessoal.py:57) e grava por `bulk_update` em [sombra_mascara_pessoal.py:62](bin/sombra_mascara_pessoal.py:62) e [sombra_mascara_pessoal.py:64](bin/sombra_mascara_pessoal.py:64), com banco exigido como `sombra` em [sombra_mascara_pessoal.py:10](bin/sombra_mascara_pessoal.py:10). Não faz trim, mas transforma espaços em `… <hash>`, conforme [sombra_regras_pessoais.py:29](bin/sombra_regras_pessoais.py:29); não produz resposta só de espaços. |

Os testes também fazem escrita direta: há um `update(resposta_colab='   ')` explícito em [test_o261_resposta_vazia.py:152](app/chamados/tests/test_o261_resposta_vazia.py:152), após preencher `respondida_em` em [test_o261_resposta_vazia.py:150](app/chamados/tests/test_o261_resposta_vazia.py:150). Isso monta um cenário de teste; execução desse cenário nesta análise: [nao verificado].

`carimbar_resposta_pela_trilha` não escreve `resposta_colab`: rejeita resposta cujo `strip()` fique vazio em [disputa_emissao.py:1670](app/chamados/services/disputa_emissao.py:1670) e salva exclusivamente `respondida_em` em [disputa_emissao.py:1676](app/chamados/services/disputa_emissao.py:1676).

A proteção demonstrada é anterior ao save no **core**; ela não existe para o texto novo no **automático**, nem no `save` genérico. [disputa_emissao.py:1432](app/chamados/services/disputa_emissao.py:1432), [disputa_emissao.py:1652](app/chamados/services/disputa_emissao.py:1652), [models.py:3414](app/chamados/models.py:3414). Qual escritor originou registros reais, ou se algum já gravou somente espaços em produção, permanece [nao verificado].

## 12/11 19:54 · sessao be142c9f6191 · - · - · origem=Ronald (tmux)

Escritores indiretos e caminhos auditados:

| Caminho | Evidência | Espaços + carimbo |
|---|---|---|
| `PerguntaDisputa._nascer` recebe `**campos`, constrói e persiste | `app/chamados/models.py:2826`, `:3015`, `:3016`, `:3018` | Pode receber `resposta_colab='   '` e `respondida_em=instante`; não faz trim. As portas `abrir`/`abrir_conversa` encaminham os campos em `:2791`/`:2803`. Os chamadores operacionais localizados não passam esse par: `app/chamados/services/disputa_emissao.py:439`, `:444`, `:837`, `:1371`; `app/chamados/services/declaracao_texto.py:376`, `:666`; `app/chamados/services/acoes_disputa.py:263`. Execução disso em produção: [não verificado]. |
| Máscara da sombra | `bin/sombra_regras_pessoais.py:85` inventaria o campo; `bin/sombra_mascara_pessoal.py:57` atribui e `:62`/`:64` fazem `bulk_update` | Restrita ao banco `sombra` por `:10`; não usa trim, mas transforma espaços em texto com hash, conforme `bin/sombra_regras_pessoais.py:29`–`:33`, portanto não produz texto só de espaços. |
| `PerguntaDisputa.save` | `app/chamados/models.py:3414`–`:3425` | Sem normalização/trim global: apenas identidade na inclusão e `super().save` dentro de transação. |
| `DecisaoPergunta.registrar` | `app/chamados/models.py:3954`–`:3956` | Grava decisões e acorda células; não reescreve `PerguntaDisputa.resposta_colab`. |

Entradas do escritor canônico:

- O próprio core faz trim e rejeita vazio antes de atribuir/gravar: `app/chamados/services/disputa_emissao.py:1432`–`:1434`; atribui o par em `:1467`–`:1468`; salva em `:1508`.
- Web faz trim antes da chamada: `app/chamados/views.py:1265`, `:1283`.
- API faz trim e rejeita vazio antes da chamada: `app/api/views.py:2614`–`:2620`.
- Correção administrativa faz trim e exige HH:MM antes da chamada: `app/chamados/services/acoes_disputa.py:334`–`:337`, `:371`.
- Declaração de hora faz trim antes de chamar: `app/chamados/services/declaracao_texto.py:319`, `:419`.
- Aplicação de declaração chama com hora extraída/formada como HH:MM: `app/chamados/services/declaracao_texto.py:33`–`:39`, `:611`–`:612`, `:693`.

A porta automática aceita texto novo sem trim: `app/chamados/services/disputa_emissao.py:1652`–`:1655`. Seu único chamador operacional localizado passa uma constante com texto, definida em `app/api/foto_recebida.py:25` e passada em `:48`. Portanto a capacidade da função de gravar espaços é demonstrável pelo corpo; entrada de espaços pelo fluxo de foto atual não é sustentada por esse chamador. (`app/chamados/services/disputa_emissao.py:1652`–`:1655`; `app/api/foto_recebida.py:25`, `:48`.)

`carimbar_resposta_pela_trilha` não escreve o campo de texto e recusa texto só de espaços antes de salvar o carimbo: `app/chamados/services/disputa_emissao.py:1670`, `:1674`–`:1676`.

## 12/11 19:54 · sessao 2ca5f70d8909 · - · - · origem=Ronald (tmux)

Escritores operacionais explícitos encontrados:

| Função | Escrita e persistência | Só espaços com `respondida_em` preenchido? |
|---|---|---|
| `responder_pergunta_disputa_core` | `app/chamados/services/disputa_emissao.py:1467` atribui texto; `:1468` instante; `:1508` salva ambos. | **Não pelo argumento recebido**: normaliza em `:1432` com `(resposta or '').strip()` e rejeita vazio em `:1433-1434`, antes da atribuição/save. |
| `responder_automaticamente` | Mesmo arquivo `:1652` atribui `texto` bruto; `:1653` instante; `:1655` salva ambos. | **Sim na função**, caso receba `texto='   '` e instante não nulo: não normaliza nem valida o argumento. O `strip()` de `:1647` examina **o texto antigo**, apenas impedindo sobrescrever resposta já existente. |
| `reabrir_para_correcao` | Mesmo arquivo `:1593` prepara `resposta_colab=''` e `respondida_em=None`; persiste por `:1632` ou por `:1630` → `app/chamados/models.py:3265-3267`. | **Não**: grava vazio e remove instante; não depende de trim. |

O único chamador operacional encontrado de `responder_automaticamente` é `app/api/foto_recebida.py:48`: fornece a constante não vazia `_TEXTO = '[auto] Foto recebida em upload separado.'` (`:25`) e `agora = _tz.now()` (`:42`). Portanto, possibilidade na função está demonstrada; geração de espaços por essa chamada atual não está demonstrada, pois o argumento é fixo.

Também há portas genéricas que podem persistir o campo:

- `PerguntaDisputa._nascer`: constrói com `**campos` em `app/chamados/models.py:3015-3016`, salva em `:3018`; recebe esses argumentos através de `abrir` (`:2777`, `:2790-2791`) ou `abrir_conversa` (`:2795`, `:2802-2803`). Não faz trim do campo; `save()` também não o faz (`:3414-3424`). Poderia receber texto de espaços e instante preenchido por esses kwargs. As chamadas operacionais examinadas não passam os campos de resposta: `disputa_emissao.py:439-452`, `:837-846`, `:1371-1378`; `declaracao_texto.py:376-377`, `:666-667`; `acoes_disputa.py:263-268`. O `**kw` de `disputa_emissao.py:845` contém apenas `chamados_relacionados_ids`, preparado em `:834-836`.
- `PerguntaDisputa.descarimbar_em_lote`: `app/chamados/models.py:3253`, `:3265-3267` repassa `**outros` a `update`, sem trim. A chamada que passa `resposta_colab` é a reabertura acima, com `''`/`None` (`disputa_emissao.py:1593`, `:1630`); a outra chamada passa apenas `validada_em`, `validada_por`, `respondida_em` (`models.py:1567`).
- Máscara da **sombra**: inventário `bin/sombra_regras_pessoais.py:85` inclui a coluna; executor `bin/sombra_mascara_pessoal.py:44-64` transforma valores, atribui por `setattr` (`:57`) e persiste por `bulk_update` (`:62`, `:64`). Exige banco chamado `sombra` (`:10`). Não faz trim, mas espaços viram marcador com hash conforme `bin/sombra_regras_pessoais.py:29-33`; não escreve `respondida_em`.

`carimbar_resposta_pela_trilha` **não escreve `resposta_colab`**: só preenche instante em `disputa_emissao.py:1674-1676`; rejeita texto só de espaços antes disso em `:1670-1671`. `PerguntaDisputa.carimbar` também só adiciona instante com texto não vazio, usando `texto_vazio` em `models.py:3131-3134`, sem reescrever o texto.

## 12/11 19:52 · sessao 31a083aa2e1d · TRANCA-VIVAS-08 · verificador · origem=Code

**REPROVA — bloqueante 1: a cobertura ainda permite retirar `vivas_resgatadas` da união sem contrariar os oráculos examinados.** O caso novo mantém essa parcela em zero; falta um caso com resgatada exclusiva conferindo o total e a linha por competência. (`app/chamados/tests/test_tranca_vivas_08.py:186`; `app/chamados/services/competencia_trancada.py:351`)

1. **BLOQUEIA 1: rótulos curados; prova da união ainda incompleta.**

   | Campo | Quem escreve o valor | Quem lê |
   |---|---|---|
   | `decidir_pelo_dp` | `medir()` inclui IDs não resgatados vindos de `respostas_sem_veredito(ignorar_tranca=True)`. (`app/chamados/services/competencia_trancada.py:126`, `:137`) | Pauta do DP, comando de encerramento e agregador. (`app/chamados/services/competencia_trancada.py:246`; `app/chamados/management/commands/encerrar_por_competencia_trancada.py:38`; `app/chamados/services/competencia_trancada.py:353`) |
   | `vivas_resgatadas` | `medir()` inclui perguntas vivas ligadas a chamados resgatados após a tranca. (`app/chamados/services/competencia_trancada.py:117`, `:132`, `:149`) | `medir_frota()` incorpora à união e conta separadamente. (`app/chamados/services/competencia_trancada.py:348`, `:353`) |
   | `vivas_tocadas_pelo_admin` | `medir()` classifica perguntas vivas usando o helper real de toque. (`app/chamados/services/competencia_trancada.py:155`, `:158`) | `medir_frota()` incorpora à união e conta separadamente. (`app/chamados/services/competencia_trancada.py:348`, `:353`) |
   | `ficam_com_o_admin` | `medir_frota()` calcula a cardinalidade da união por competência e acumula os totais. (`app/chamados/services/competencia_trancada.py:348`, `:351`, `:355`) | CLI, API e mensageria, pelo percurso abaixo. (`app/chamados/management/commands/chamados_em_competencia_trancada.py:22`; `app/api/views_mensageria.py:1573`; `mensageria/nucleo/ferramentas.py:2038`) |

   A CLI agora imprime a união como `ficam_com_o_admin=` e publica as três contagens separadas. A API repassa os quatro valores; `core_client` lê JSON, e a ferramenta descreve corretamente a união e devolve o payload. (`app/chamados/management/commands/chamados_em_competencia_trancada.py:22`; `app/api/views_mensageria.py:1573`; `mensageria/nucleo/core_client.py:239`; `mensageria/nucleo/ferramentas.py:2032`)

   O outro comando, `encerrar_por_competencia_trancada`, imprime `decidir_pelo_dp` como quantidade e IDs da lista de respostas; não imprime a união sob esse nome. (`app/chamados/management/commands/encerrar_por_competencia_trancada.py:36`; `app/chamados/services/competencia_trancada.py:139`)

   **Não encontrei leitor da CLI alterada que dependesse de `decidir_pelo_dp=` como união:** o placar extrai exclusivamente `^chamados_em_competencia_trancada=`; o teste antigo que procura `decidir_pelo_dp=1 ids=...` chama o comando de **encerramento**; o teste novo exige a contagem separada. (`bin/placar_code.sh:287`; `app/chamados/tests/test_competencia_trancada.py:54`, `:149`; `app/chamados/tests/test_tranca_vivas_08.py:199`)

   API e mensageria têm testes de contagem geral/repasse, mas suas asserções não certificam os quatro campos. (`app/api/tests/test_api_mensageria_chamados_em_competencia_trancada.py:39`; `mensageria/nucleo/tests/test_ferramenta_chamados_em_competencia_trancada.py:23`)

   Permanecem descrições históricas: o relato de 17/09 registra a semântica antiga do residual e sua soma em `ficam_com_o_admin`; TICKETS registra a regra antiga de escrever a via na pergunta. São registros datados, enquanto o fonte atual declara a retirada dessa escrita. (`app/docs/RELATO-ARQUIVO.md:17319`, `:17323`; `app/docs/TICKETS.md:1180`; `app/chamados/services/competencia_trancada.py:133`)

   Consumidores externos, usos dinâmicos em templates e leitores não identificados pelo censo textual: **[nao verificado]**.

2. **Fixture real, mordida da soma e mutante sobrevivente.**

   Por leitura, a fixture monta a **mesma pergunta** nas duas listas pelo caminho real: cria pergunta sem vínculo com a célula, grava resposta sem validação, lavra a célula via admin e posiciona a trilha após a tranca. `medir()` consulta `respostas_sem_veredito`, `pergunta_viva` e o helper de toque; a asserção exige expressamente o mesmo PK nas duas listas. (`app/chamados/tests/test_c7_red_minimos.py:110`; `app/chamados/tests/test_tranca_vivas_08.py:175`, `:177`, `:49`, `:182`; `app/chamados/services/competencia_trancada.py:126`, `:144`, `:155`) Execução dessa fixture com ORM: **[nao verificado]**.

   Conferi a agregação em memória, extraindo o corpo real de `medir_frota` e substituindo apenas as entradas: a âncora da união ocorre uma vez; DP `[id]`, resgatadas `[]`, tocadas `[id]` produzem **1**, enquanto a troca por concatenação produz **2**. Isso corresponde à transformação e ao resultado exigidos pelo teste. (`app/chamados/services/competencia_trancada.py:348`, `:351`; `app/chamados/tests/test_tranca_vivas_08.py:206`, `:211`, `:221`)

   Os exemplos simples precisam desta distinção:

   - União → somente DP ou somente tocadas mantém o resultado comportamental `1`, mas remove a âncora e é barrado por `fonte.count(velho) == 1`. (`app/chamados/tests/test_tranca_vivas_08.py:186`, `:221`)
   - Cardinalidade → `len()` de DP ou tocadas preserva a âncora, mas devolve `1` quando o teste da soma exige `2`. (`app/chamados/tests/test_tranca_vivas_08.py:211`; `app/chamados/services/competencia_trancada.py:351`)

   **O bloqueante é outro mutante:** substituir, na saída, `len(_com_o_admin)` por `len(_com_o_admin) - len(m['vivas_resgatadas'])`. Ele preserva a âncora, o vazio `0`, a sobreposição `1` e a soma mutada `2`; porém uma resgatada exclusiva passa de **1 para 0**. Conferi esses resultados em memória. Os oráculos examinados deixam passar a perda de uma categoria expressamente integrante da união. (`app/chamados/services/competencia_trancada.py:336`, `:348`, `:351`; `app/chamados/tests/test_tranca_vivas_08.py:163`, `:186`, `:189`, `:198`, `:211`)

   Os testes antigos de resgate verificam desarquivamento e vida da pergunta; o teste antigo do placar confere apenas o contador geral. Não fecham essa lacuna da união. (`app/chamados/tests/test_competencia_trancada.py:164`, `:177`, `:219`) Sobrevivência na suíte Django completa: **[nao verificado]**.

3. **FILA e JUIZES.md.**

   O item 2 continua materialmente presente: resposta do colaborador também carimba `resgatado_em`, embora o docstring da exclusão ainda diga “RESGATADO pelo admin”. (`app/chamados/services/materializacao.py:724`; `app/chamados/services/arquivo.py:205`; `app/chamados/services/competencia_trancada.py:75`)

   O item 3 permanece: o placar lê o contador geral e não extrai `vivas_apesar_da_tranca`. (`bin/placar_code.sh:287`) Registro em arquivo da cópia do encaminhamento dos itens 2, 3 e 5 para FILA, assim como medição pós-cura da frota e das quatro perguntas: **[nao verificado]**.

   **O item 4 está coerente:** JUIZES.md descreve `via='admin'` ou presença de `acoes`; o filtro exige exatamente essa assinatura em um `dict`, além de usuário não nulo, diferente do sistema, e timestamp estritamente posterior à tranca. `acoes=[]` conta por presença da chave e tem caso explícito. (`app/docs/JUIZES.md:88`; `app/chamados/juizes.py:1260`, `:1298`, `:1302`; `app/ponto/services/veredito_celula.py:76`; `app/chamados/tests/test_tranca_vivas_08.py:107`)

4. **Checklist do delta.**

   - **Segundo juiz / derivação paralela:** não identificados nos caminhos examinados; o contador consulta as autoridades existentes, e API/CLI leem `medir_frota`. (`app/core/juizes.py:292`; `app/chamados/services/competencia_trancada.py:144`, `:155`; `app/api/views_mensageria.py:1563`; `app/chamados/management/commands/chamados_em_competencia_trancada.py:14`)
   - **Segundo escritor fora da porta:** não identificado nos trechos novos de produção; são agregação, projeção e impressão. (`app/chamados/services/competencia_trancada.py:341`; `app/api/views_mensageria.py:1573`; `app/chamados/management/commands/chamados_em_competencia_trancada.py:22`)
   - **Relógio fora da borda:** não identificado; o filtro recebe `fechado_em`, e a fixture usa relógio congelado. (`app/chamados/juizes.py:1070`, `:1302`; `app/chamados/tests/test_tranca_vivas_08.py:30`, `:49`)
   - **Selo sem mordida:** há mordida contra soma, mas proteção incompleta contra perda de categoria — **bloqueante 1 acima**. (`app/chamados/tests/test_tranca_vivas_08.py:187`, `:211`)
   - **Campo com dois significados:** a publicação agora distingue união e parcelas; permanece a dívida já descrita sobre a autoria do resgate. (`app/chamados/management/commands/chamados_em_competencia_trancada.py:22`; `mensageria/nucleo/ferramentas.py:2032`; `app/chamados/services/materializacao.py:725`)

Suíte completa, histórico RED/GREEN, integridade do hash e efeito em produção: **[nao verificado]**. [nao verificado]

## 12/11 19:51 · sessao c5e6ab20fd86 · - · - · origem=Ronald (tmux)

A fixture é consistente por leitura e percorre `medir()` real: cria uma pergunta sem `celula`, grava resposta sem validação/materialização e lavra o dia via admin; o log vai para cinco minutos após a tranca. A lista DP vem de `respostas_sem_veredito(ignorar_tranca=True)` e a mesma pergunta viva entra em tocadas pelo helper real. A asserção exige explicitamente o mesmo PK nas duas listas. (`app/chamados/tests/test_tranca_vivas_08.py:34`, `:43`, `:175`, `:182`; `app/chamados/tests/test_c7_red_minimos.py:110`; `app/chamados/models.py:1843`; `app/chamados/services/competencia_trancada.py:123`, `:137`, `:144`, `:155`.) Execução ORM: [nao verificado].

Executei somente a agregação extraída por AST de `medir_frota`, em memória, substituindo import de `PeriodoFechado` e retorno de `medir`. A âncora ocorre uma vez; DP `[1]`, resgatadas `[]`, tocadas `[1]` dão união `1` e soma `2`. Isso corresponde à âncora e ao oráculo escritos no teste. (`app/chamados/services/competencia_trancada.py:348`, `:351`; `app/chamados/tests/test_tranca_vivas_08.py:206`, `:211`, `:221`.)

Os exemplos simples **não sobrevivem à classe inteira**:

- União → só DP ou só tocadas: passam o caso comportamental `1`, mas removem a âncora e falham em `fonte.count(velho) == 1`. (`app/chamados/tests/test_tranca_vivas_08.py:182`, `:221`.)
- `len(_com_o_admin)` → `len(m['decidir_pelo_dp'])`: preserva a âncora, mas o MORDE passa a devolver `1` quando exige `2`. (`app/chamados/services/competencia_trancada.py:351`; `app/chamados/tests/test_tranca_vivas_08.py:211`.)

Há, porém, um mutante ligado ao requisito que escapa aos oráculos examinados: substituir `len(_com_o_admin)` por `len(_com_o_admin) - len(m['vivas_resgatadas'])`. Mantém a âncora, preserva overlap `1`, vazio `0` e soma mutada `2`; portanto preserva as asserções da classe. Numa resgatada exclusiva, produz união `0` quando a fonte real produz `1`, contrariando a categoria declarada. (`app/chamados/services/competencia_trancada.py:336`, `:348`, `:351`; `app/chamados/tests/test_tranca_vivas_08.py:163`, `:186`, `:189`, `:198`, `:209`, `:211`.) Sobrevivência da suíte Django completa: [nao verificado].

Os testes antigos citados de resgate verificam desarquivamento, vida e segunda passada; não conferem a união de `medir_frota`. O teste antigo do placar confere apenas `chamados_em_competencia_trancada`. (`app/chamados/tests/test_competencia_trancada.py:164`, `:177`, `:219`.)

## 12/11 19:51 · sessao ea33eaacf0ca · - · - · origem=Ronald (tmux)

Censo semântico concluído por leitura:

| Campo | Quem escreve | Quem lê/publica |
|---|---|---|
| `decidir_pelo_dp` | `medir()` inclui ids não resgatados vindos de `respostas_sem_veredito(ignorar_tranca=True)` — `app/chamados/services/competencia_trancada.py:126`, `:137-139` | `_pauta_do_dp` usa ids de respostas — `:246-249`; `encerrar` chama pauta — `:232-233`; comando de encerramento imprime quantidade e ids — `app/chamados/management/commands/encerrar_por_competencia_trancada.py:36-39`; contador calcula cardinalidade — `competencia_trancada.py:353`. |
| `vivas_resgatadas` | `medir()` inclui pergunta viva ligada a chamado resgatado depois da tranca — `competencia_trancada.py:117-118`, `:132`, `:145-150` | `medir_frota()` usa na união e conta separadamente — `:348`, `:353`. |
| `vivas_tocadas_pelo_admin` | `medir()` consulta `_admin_tocou_os_dias_depois` e classifica pergunta viva — `competencia_trancada.py:155-158` | `medir_frota()` usa na união e conta separadamente — `:348`, `:353`. |
| `ficam_com_o_admin` | `medir_frota()` calcula cardinalidade da união e soma por competência — `competencia_trancada.py:348-356` | comando imprime união nominal e separadas — `app/chamados/management/commands/chamados_em_competencia_trancada.py:16-25`; API repassa — `app/api/views_mensageria.py:1573-1578`; cliente GET JSON — `mensageria/nucleo/core_client.py:239-241`; ferramenta descreve união corretamente e devolve payload — `mensageria/nucleo/ferramentas.py:2032-2040`. |

Não identifiquei consumidor da CLI que ainda interprete `decidir_pelo_dp=` como união: o placar extrai exclusivamente `^chamados_em_competencia_trancada=` (`bin/placar_code.sh:287-289`); o teste antigo procura `decidir_pelo_dp=1 ids=...` (`app/chamados/tests/test_competencia_trancada.py:149`), mas seu helper chama o **outro comando**, `encerrar_por_competencia_trancada` (`:52-55`); o teste novo procura a lista separada (`app/chamados/tests/test_tranca_vivas_08.py:199`). Consumidores externos à cópia: [nao verificado].

Há textos/fontes antigos preservados: `RELATO-ARQUIVO.md:17319-17323` registra o BO de 17/09 com `vivas_apesar_da_tranca` significando resgatadas e soma em `ficam_com_o_admin`; `:17387` registra o contador antigo. `TICKETS.md:1180` registra a regra FEITO 15/09, inclusive a escrita antiga na pergunta, enquanto o fonte atual declara sua retirada pela C7 (`competencia_trancada.py:133-135`).

O snapshot `app/chamados/tests/e4b_c12_base.json:6` contém a implementação antiga, inclusive `ficam_com_o_admin=len(dp)+len(vivas_apesar_da_tranca)`. É base de teste carregada em `e4b_c12_verificar.py:18`; o modo `base` reenxerta funções escritoras listadas (`:132-153`), e o sítio dessa família é o encerramento (`e4b_c12_sitios.json:68-72`). Não constitui parser vivo da linha alterada nem substituição de `medir_frota()` nesse modo (`e4b_c12_verificar.py:140-153`).

Testes da API e mensageria verificam contagem geral/repasse, sem certificar os quatro novos campos (`app/api/tests/test_api_mensageria_chamados_em_competencia_trancada.py:39-48`; `mensageria/nucleo/tests/test_ferramenta_chamados_em_competencia_trancada.py:23-28`). Execução desses testes: [nao verificado].

## 12/11 19:50 · sessao d5aabf8b9f26 · - · - · origem=Ronald (tmux)

JUIZES.md está coerente com o filtro real: a linha 88 descreve a assinatura `via='admin'` ou presença de `acoes`; o código exige exatamente um `dict` com uma dessas condições (`app/docs/JUIZES.md:88`; `app/chamados/juizes.py:1298`; constante em `app/ponto/services/veredito_celula.py:76`). Ainda exige usuário não nulo, diferente do sistema, e ato estritamente posterior à tranca (`app/chamados/juizes.py:1260`; `app/chamados/juizes.py:1302`).

`acoes=[]` conta por presença da chave, coerentemente com o escritor real e com teste explícito; não se exige lista preenchida (`app/ponto/services/veredito_celula.py:358`; `app/chamados/tests/test_tranca_vivas_08.py:107`; `app/chamados/tests/test_tranca_vivas_08.py:109`). `acoes=None` também satisfaria o predicado, mas produção desse valor pelo escritor examinado é [nao verificado].

Os escritores reais confirmam a distinção: a porta da célula grava `via` no evento; o despachante humano chama a mesma porta com `via='admin'` e depois registra `acoes` (`app/ponto/portas/celula.py:1042`; `app/ponto/portas/celula.py:1044`; `app/ponto/services/veredito_celula.py:352`; `app/ponto/services/veredito_celula.py:356`). O ator pode vir do request inclusive na lavra automática; o código de observabilidade documenta e implementa essa herança (`app/core/observ.py:100`; `app/core/observ.py:108`; `app/core/observ.py:115`).

No checklist:

- **Segundo juiz:** não identificado no caminho examinado. C20 está registrado; o avulso delega ao lote, e o contador lê `pergunta_viva` e o mesmo helper de toque usado pelo lote (`app/core/juizes.py:292`; `app/chamados/juizes.py:991`; `app/chamados/juizes.py:1076`; `app/chamados/services/competencia_trancada.py:144`; `app/chamados/services/competencia_trancada.py:155`).
- **Segundo escritor:** os trechos novos de produção apenas agregam dicionários, projetam JSON e imprimem contagens; não acrescentam escrita persistente (`app/chamados/services/competencia_trancada.py:341`; `app/chamados/services/competencia_trancada.py:355`; `app/api/views_mensageria.py:1563`; `app/chamados/management/commands/chamados_em_competencia_trancada.py:22`). A chamada do teste continua passando pela porta canônica da célula (`app/chamados/tests/test_tranca_vivas_08.py:177`; `app/escala/models.py:1826`).
- **Relógio fora da borda:** não identificado nesses trechos. O filtro compara com `fechado_em`; a fixture congela `AGORA` e posiciona a trilha em `fechado_em + 5min` (`app/chamados/juizes.py:1070`; `app/chamados/juizes.py:1302`; `app/chamados/tests/test_tranca_vivas_08.py:30`; `app/chamados/tests/test_tranca_vivas_08.py:49`).
- **Derivação paralela:** não identificada nesse contador: os juízos são consultados nas autoridades existentes; união/contagens ficam em `medir_frota`, e API/CLI leem sua saída (`app/chamados/services/competencia_trancada.py:144`; `app/chamados/services/competencia_trancada.py:155`; `app/chamados/services/competencia_trancada.py:348`; `app/api/views_mensageria.py:1563`; `app/chamados/management/commands/chamados_em_competencia_trancada.py:14`).
- **Campo com dois significados:** a união agora recebe o nome próprio e os componentes são expostos separadamente (`app/chamados/services/competencia_trancada.py:336`; `app/chamados/services/competencia_trancada.py:353`; `app/api/views_mensageria.py:1573`; `app/chamados/management/commands/chamados_em_competencia_trancada.py:22`).

A dívida de `resgatado_em` permanece: a descrição “RESGATADO pelo admin” é mais estreita que a origem real, pois resposta do colaborador também carimba o resgate (`app/chamados/services/competencia_trancada.py:75`; `app/chamados/services/arquivo.py:180`; `app/chamados/services/materializacao.py:725`). Isso corresponde ao item 2 já encaminhado para FILA no dado do usuário; registro desse encaminhamento em arquivo da cópia é [nao verificado].

Execução da suite, histórico RED, resultados de produção pós-cura e integridade do hash do archive: [nao verificado]. [nao verificado]

## 12/11 19:45 · sessao 041d9a2e35d3 · TIPO-NA-FONTE · sonda · origem=Code

A tentativa `bash bin/sonda_leitura.sh sonda_estado_c.py` terminou na guarda de arquivo inexistente, antes das escritas do wrapper ([bin/sonda_leitura.sh:71](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/bin/sonda_leitura.sh:71)). Saída integral:

```text
sonda_leitura: sonda_estado_c.py nao existe
```

Não houve nova medição de produção. A identidade Git do HEAD da cópia permanece [nao verificado].

Abaixo, reprodução integral do dado fornecido; execução e conteúdo do arquivo original da sonda [nao verificado]:

```text
sonda_leitura: sonda_estado_c.py [5eafc1a4f533ae11] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

lavradas                     135068  amostra=[]
orfas_hhmm                     3415  amostra=[104313, 143296, 7908, 137769, 117714, 123044, 24044, 28707, 113924, 113931]
orfas_vazia                  131653  amostra=[137148, 123129, 111993, 109909, 22590, 138874, 22594, 28678, 138878, 33849]
total_erros=0
== anexo: complemento_tipo_item3_matriz.txt ==
aval Ronald: COMPLEMENTO TIPO-ITEM3-MATRIZ (12/11 19:5x) -- texto integral no prompt da sessao 12/11 ~19:5x; resumo operativo: EXPAND so (escritor ISO com fuso, todo leitor de producao pelo acessor incl. cartorio:837/:1424, 5 asserts + selo :959-960 pelo acessor, fixture ganha a chave); plantio NAO muda (registrar chave dna.lampadas[].instante com fuso); AtaVigiada vira MutableMapping injetada automaticamente sob settings ci (mutantes update, |=, del); nao_medidos SAI do diff, (c) chave ausente/n_orfas inconsistente = ESTADO ILEGAL com tripwire, sonda de frota de (c) ANTES (>0 = PARA e publica); achado 6: igual no HEAD 0a747193e -> REGISTRAR (TURNO-G5), se o diff mudou cura. PROIBIDO big bang em pousos separados, None=zero, fallback, tocar plantio. LEI-AKITA: origem=escala/services/ata.py (acessor+gravar_orfas), testemunha=o acessor, RED=contraexemplos 1-5 da preverif v2, quem-mais-le=tabela item 8 da preverif, juizes novos=0, padrao=Single Writer Principle
```

**O código permite persistir ata sem `orfas` ou com contador divergente.** O construtor habitual entrega ambas corretamente, mas a porta não valida o contrato; a operação de precedência também pode criar uma ata sem essas chaves ([cartorio.py:742](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/services/cartorio.py:742), [cartorio.py:745](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/services/cartorio.py:745), [portas/celula.py:995](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/portas/celula.py:995), [portas/celula.py:1083](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/portas/celula.py:1083)).

| Escritor operacional | O que escreve e quais estados permite |
|---|---|
| `ata_do_dia` | Constrói `n_orfas = len(dia.get('orfas') or [])` e `orfas = [str(h) …]`. Para as listas produzidas pela grade, inclui ambas e mantém `n_orfas == len(orfas)` ([cartorio.py:742](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/services/cartorio.py:742), [cartorio.py:745](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/services/cartorio.py:745), [utils.py:1811](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/escala/utils.py:1811)). |
| `lavrar_veredito` | Recebe qualquer `dict(ata)`, preservando somente a chave alheia `precedencia`. Atribui a ata nos **dois ramos**, em `:1006` e `:1033`. Não exige `orfas`, `n_orfas` ou igualdade entre eles; portanto aceita, por exemplo, `{}` ou `{'orfas': [], 'n_orfas': 1}` ([portas/celula.py:104](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/portas/celula.py:104), [:991](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/portas/celula.py:991), [:1006](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/portas/celula.py:1006), [:1033](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/portas/celula.py:1033)). |
| `carimbar_precedencia` | Copia `cel.ata or {}`, acrescenta `precedencia` e persiste o JSON inteiro. Pode criar `{'precedencia': …}` sobre `None`/`{}`, sem `orfas` nem `n_orfas`; também conserva eventual divergência anterior ([portas/celula.py:1083](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/portas/celula.py:1083), [:1087](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/portas/celula.py:1087)). |
| `carimbar` → `salvar_e_emitir` | Persiste a ata previamente atribuída pela lavra com `save=False`, quando `ata` vem em `lavrados`; não recalcula as duas chaves ([portas/celula.py:1070](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/portas/celula.py:1070), [:905](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/portas/celula.py:905)). |

O método do modelo apenas delega à porta, e o campo é `JSONField(null=True, blank=True)` ([models.py:1826](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/escala/models.py:1826), [models.py:1778](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/escala/models.py:1778)).

No cartório normal, a ata vem de `ata_do_dia`, passa à lavra e é salva pelo carimbo ([cartorio.py:1851](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/services/cartorio.py:1851), [:1853](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/services/cartorio.py:1853), [:1873](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/services/cartorio.py:1873)). No ramo nunca-bateu, `_dia_nb` ausente resulta em `ata=None`, que não substitui a ata existente; depois, o caminho unitário ainda pode carimbar precedência ([cartorio.py:1251](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/services/cartorio.py:1251), [:1293](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/services/cartorio.py:1293), [portas/celula.py:991](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/portas/celula.py:991), [cartorio.py:2048](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/services/cartorio.py:2048)).

Os comandos de backfill/regeneração chegam aos mesmos escritores:

| Entrada | Caminho |
|---|---|
| `processar_cartorio --apply --forcar` | `--forcar` declara backfill de ata; chama `julgar_colab` ([processar_cartorio.py:29](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/management/commands/processar_cartorio.py:29), [:65](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/management/commands/processar_cartorio.py:65)). |
| `celula_veredito_velho` | Chama `julgar_colab(apply_=True, forcar=True)`; sem aplicação, desfaz a transação ([celula_veredito_velho.py:144](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/management/commands/celula_veredito_velho.py:144), [:156](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/management/commands/celula_veredito_velho.py:156)). |
| `rejulgar_ausencia_em_folga` | Chama `julgar_celula(..., forcar=True)` ([rejulgar_ausencia_em_folga.py:78](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/management/commands/rejulgar_ausencia_em_folga.py:78)). |
| `curar_residuo_bug115` | Pode gerar a célula e depois relavra pelo cartório ([curar_residuo_bug115.py:135](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/management/commands/curar_residuo_bug115.py:135), [:143](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/management/commands/curar_residuo_bug115.py:143)). |
| `bin/rejulgar_o118_col221.py` | Chama `julgar_colab(..., forcar=True)` ([rejulgar_o118_col221.py:24](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/bin/rejulgar_o118_col221.py:24)). |
| `aplicar_col900` | Regenera ou reverte pela porta ([aplicar_col900.py:232](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/management/commands/aplicar_col900.py:232), [:184](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/management/commands/aplicar_col900.py:184)). |
| `corrigir_escala_retroativa` | Chama regeneração e reconciliação ([corrigir_escala_retroativa.py:230](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/colaboradores/management/commands/corrigir_escala_retroativa.py:230), [:237](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/colaboradores/management/commands/corrigir_escala_retroativa.py:237)). |
| `diff_col900` | Exercita a regeneração em ensaio com transação desfeita ([diff_col900.py:69](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/management/commands/diff_col900.py:69), [:77](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/management/commands/diff_col900.py:77)). |

A regeneração/reversão alcança `julgar_celula(..., forcar=True)` pelo processamento posterior da alteração; não constrói outro par `orfas/n_orfas` ([portas/celula.py:434](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/portas/celula.py:434), [:734](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/portas/celula.py:734), [:811](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/portas/celula.py:811)).

Há ainda estas distinções:

- Geração e regeneração podem criar célula com `ata=NULL`; isso difere de um dicionário lavrado sem `orfas` ([portas/celula.py:259](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/portas/celula.py:259), [:854](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/portas/celula.py:854), [models.py:1778](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/escala/models.py:1778)).
- `backfill_b5_insumos` lê a ata e grava apenas os insumos; seu `bulk_update` não inclui `ata` ([backfill_b5_insumos.py:73](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/management/commands/backfill_b5_insumos.py:73), [portas/celula.py:1108](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/portas/celula.py:1108)).
- A migração da ata apenas adiciona o campo anulável ([0037_hx_ata_celula.py:13](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/escala/migrations/0037_hx_ata_celula.py:13)).
- As outras atribuições operacionais a `.ata` são cópias em memória: projeção do julgamento e montagem da grade; esta última contém `orfas` sem `n_orfas`, sem persistência nesse fluxo ([portas/celula.py:943](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/portas/celula.py:943), [utils.py:1886](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/escala/utils.py:1886), [:1895](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/escala/utils.py:1895)).

**Não é possível atribuir escritor/data a classes `c_*`: nenhuma foi apresentada no dado.** Também não foram fornecidos valores de `n_orfas`, presença explícita da chave por registro ou datas de gravação; o significado de `total_erros=0` depende do código ausente da sonda. [nao verificado]

As duas categorias positivas fornecidas, `orfas_hhmm=3415` e `orfas_vazia=131653`, são compatíveis com o construtor atual, que grava strings e lista vazia respectivamente; isso não demonstra qual escritor gravou cada linha nem quando ([cartorio.py:742](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/services/cartorio.py:742), [:745](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/services/cartorio.py:745)). Autoria, data e associação a qualquer `c_*`: [nao verificado].

## 12/11 19:44 · sessao f7c1b2d23ac2 · TIPO-NA-FONTE · construtor · origem=Code

Parei antes de editar: o complemento exige **“sonda de frota de (c) ANTES (>0 = PARA e publica)”**. O resultado dessa sonda não consta do dado fornecido: **[nao verificado]**. Falta de resultado não permite assumir zero.

A inspeção confirmou:

- Ausência/inconsistência ainda retorna `None`: [ata.py:119](escala/services/ata.py:119).
- Os REDs em memória reproduziram os escapes de `update`, `|=` e `del` da classe atual: [ata.py:46](escala/services/ata.py:46).
- Os leitores crus permanecem: [cartorio.py:837](ponto/services/cartorio.py:837) e [cartorio.py:1424](ponto/services/cartorio.py:1424).
- Achado 6: **pre-existente, DECIDIDO PELA LEI TURNO-G5, obra própria**. A comparação com `0a747193e` confirmou a consulta civil e a atribuição por turno: [vigia_pareador_celula.py:114](ponto/services/vigia_pareador_celula.py:114), [utils.py:1763](escala/utils.py:1763).
- Próxima chave permanece pendente: `dna.lampadas[].instante` nasce sem fuso em [gerar_celulas.py:69](ponto/management/commands/gerar_celulas.py:69).

Envie o resultado raspado da sonda, com **total examinado e quantidade de estados (c)**. A cura e sua validação aguardam essa condição obrigatória.

## 12/11 19:43 · sessao 770ef8f2cd1b · - · - · origem=Ronald (tmux)

Escritores persistentes em services/portas/models:

- `app/ponto/services/cartorio.py:730` (`ata_do_dia`) é o construtor dos dois valores: `n_orfas = len(dia.get('orfas') or [])` em `:742`, `orfas = [str(h) ...]` em `:745`. Com a lista de entrada produzida pela grade, sempre inclui ambas as chaves e mantém os comprimentos iguais; a grade materializa listas em `app/escala/utils.py:1811` e ordena em `:1837-1838`.
- O cartório normal monta a ata em `app/ponto/services/cartorio.py:1851`, passa à lavra em `:1853`, e persiste pelo carimbo em `:1873-1874`.
- O ramo nunca-bateu monta ata completa apenas quando `_dia_nb` existe; caso contrário passa `None` (`app/ponto/services/cartorio.py:1251`, `:1292-1293`). `ata=None` não altera a ata na porta (`app/ponto/portas/celula.py:991`). O carimbo final ocorre em `app/ponto/services/cartorio.py:1314-1316`.
- `app/escala/models.py:1819-1828` só delega para a porta; o campo é JSON livre, nullable (`:1778`), sem constraint sobre essas chaves em `Meta` (`:1830-1838`).
- Escritor efetivo `lavrar_veredito`: copia qualquer `dict(ata)` sem validar as chaves ou a igualdade (`app/ponto/portas/celula.py:991-1001`), preservando apenas `precedencia` (`:104`, `:997-999`). Atribui em `:1006` ou `:1033`, persiste em `:1008` ou `:1040`, usando `salvar_e_emitir` (`:899-907`). Portanto pode persistir uma ata recebida sem `orfas`, sem `n_orfas` ou com contagem divergente; os chamadores produtivos do cartório localizados usam o construtor acima (`app/ponto/services/cartorio.py:1251`, `:1851`).
- Outro escritor efetivo do JSON inteiro: `carimbar_precedencia` copia `cel.ata or {}`, acrescenta `precedencia`, atribui e salva (`app/ponto/portas/celula.py:1083-1087`). Pode criar uma ata apenas com `precedencia` quando a anterior era `None`/`{}`; também conserva uma eventual inconsistência preexistente, pois não lê essas duas chaves (`:1080-1087`). É chamado por `aplicar_precedencia_na_celula` (`app/ponto/services/cartorio.py:1986`, `:2000`) após `julgar_colab` no caminho unitário (`:2040-2048`).

Transformadores que preservam `orfas` e `n_orfas`: `dispensar_por_ausencia` modifica lâmpadas e `n_missing` (`app/ponto/services/cartorio.py:802-813`); `_ata_ate_o_teto` retorna cópia com lâmpadas e `n_missing` (`:907-917`).

Atribuições apenas em memória: `celula_como_o_julgamento_deixa` cria cópia e atribui `dict(ata)` (`app/ponto/portas/celula.py:932-948`); o montador cria cópia contendo `orfas` e sem `n_orfas`, entregue a `turnos_do_colab`, sem save nesse fluxo (`app/escala/utils.py:1886-1898`).

Datas presentes no código são comentários de desenho, sem demonstração da data de escrita das linhas de produção: ata identificada como HX-ATA-CELULA em 29/08 (`app/escala/models.py:1774`); lista identificada como T4.2 (`app/ponto/services/cartorio.py:743-745`); precedência identificada como A2 de 09/09 (`app/ponto/portas/celula.py:1078`); preservação dessa chave identificada como O261 de 18/10 (`:85-104`). Autoria/data de cada célula do dado raspado: [nao verificado].

## 12/11 19:43 · sessao 79016e5a536d · - · - · origem=Ronald (tmux)

Escritores e caminhos encontrados, somente leitura:

- `lavrar_veredito` é o persistidor da ata recebida: converte com `dict(ata)` sem validar `orfas`/`n_orfas`, preserva somente `precedencia`, atribui em dois ramos. Portanto pode receber e gravar dicionário sem `orfas` ou com contador inconsistente. `app/ponto/portas/celula.py:991`, `:995`, `:997`, `:1006`, `:1033`, `:1040`.
- `carimbar_precedencia` também escreve a ata inteira: parte de `dict(cel.ata or {})`, acrescenta somente `precedencia`, salva. Pode criar ata sem `orfas` quando a anterior é vazia; preserva inconsistência prévia sem repará-la. `app/ponto/portas/celula.py:1083`, `:1084`, `:1085`, `:1087`. Chamador de produção: `app/ponto/services/cartorio.py:2000`.
- A persistência efetiva passa por `cel.save(update_fields=campos)`, sem validação dessas chaves nesta função. `app/ponto/portas/celula.py:899`, `:905`.

Entradas de comandos/scripts que relavram pelo cartório, sem montar escritor alternativo de `orfas`:

| Entrada | Fluxo demonstrado |
|---|---|
| `processar_cartorio --apply [--forcar]` | `--forcar` é explicitamente backfill de ata em `app/ponto/management/commands/processar_cartorio.py:29`; chama `julgar_colab` em `:65`. |
| `celula_veredito_velho` | Intercepta e delega à mesma porta em `app/ponto/management/commands/celula_veredito_velho.py:121`; chama `julgar_colab(apply_=True, forcar=True)` em `:144`; sem `--aplicar`, desfaz a transação em `:156`. |
| `rejulgar_ausencia_em_folga --apply` | Gate em `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:70`; `julgar_celula(..., forcar=True)` em `:78`. |
| `curar_residuo_bug115` | Pode criar célula em `app/ponto/management/commands/curar_residuo_bug115.py:135`; relavra pelo cartório em `:143`. |
| `bin/rejulgar_o118_col221.py` | Recorta col221 desde 2026-09-21 em `:17`, `:20`, `:21`; chama `julgar_colab(..., apply_=APLICAR, forcar=True)` em `:24`. |
| `aplicar_col900` | Regenera pela porta em `app/ponto/management/commands/aplicar_col900.py:232`; reversão em `:184`. |
| `corrigir_escala_retroativa` | Chama regeneração em `app/colaboradores/management/commands/corrigir_escala_retroativa.py:230`; depois reconcilia em `:237`. |

Regeneração não monta diretamente ata: altera DNA/campos em `app/ponto/portas/celula.py:272` e `:276`; chama `_o_chao_do_dia_mudou` em `:354`, que relavra dias elegíveis por `julgar_celula(..., apply_=True, forcar=True)` em `:434`. `regenerar_celulas_dia` delega à mesma regeneração em `:734`; reversão altera DNA em `:786` e `:794` e chama o mesmo rabo em `:811`.

`backfill_b5_insumos` **não escreve ata**: lê em `app/ponto/management/commands/backfill_b5_insumos.py:73`, chama `carimbar_insumos_em_lote` em `:81`/`:84`, cujo `bulk_update` inclui somente ausência, entrada, saída e `insumos_em`. `app/ponto/portas/celula.py:1108`.

A migração de ata apenas adiciona JSONField anulável, sem validação de estrutura e sem backfill de conteúdo. `app/escala/migrations/0037_hx_ata_celula.py:13`, `:16`. A data **do cabeçalho gerado** é 2026-08-29; isso não demonstra data de implantação/execução. `app/escala/migrations/0037_hx_ata_celula.py:1`; implantação [nao verificado].

Criações de células pela geração/regeneração omitem `ata`, podendo nascer com `NULL`; isso é diferente de uma ata lavrada sem a chave. `app/ponto/portas/celula.py:259`, `:854`; campo anulável `app/escala/models.py:1778`.

Atribuições em `celula_como_o_julgamento_deixa` e no builder aparecem em `app/ponto/portas/celula.py:945` e `app/escala/utils.py:1887`; avaliar com o outro explorador que são objetos/copias de leitura, não persistidores, antes de listá-las como escritores de banco.

## 12/11 19:42 · sessao 4957a50984d0 · - · - · origem=Ronald (tmux)

Não localizei `sonda_estado_c.py`, o hash `5eafc1a4f533ae11` nem o anexo `TIPO-ITEM3-MATRIZ` na cópia por busca de nomes e conteúdo. Ausência comprovável por arquivo:linha é impossível; marcar `[nao verificado]`.

A porta `bin/sonda_leitura.sh` exige arquivo existente antes de prosseguir (`:71`). Executá-la com uma sonda existente violaria “não crie, edite nem apague arquivo”: reescreve credencial (`:126-134`), cria e remove temporários (`:216-218`), acrescenta trilha (`:248-250`) e usa banco/Django (`:235-240`). Também ajusta roles/GRANTs/rede (`:140-143`, `:157-181`, `:186-193`).

O texto fornecido não contém nenhuma contagem `c_*`, definição dessas classes, datas por célula nem identidade do escritor. Não permite atribuir uma classe positiva a escritor/data. `[nao verificado]`

Referências temporais no HEAD que não comprovam autoria dos dados fornecidos:

- `app/escala/tests/test_t41_leitor_celula.py:50-53` e `:75-76`: comentários sobre atas anteriores à T4.2 e medição em 31/08.
- `app/ponto/tests/test_o118_orfa_que_acendeu.py:2-16`: comentário sobre problema entre 22/09 e 01/10.
- `app/escala/models.py:1774-1778`: documentação inicial da ata em 29/08, listando `n_orfas` sem mencionar `orfas`.

Fábrica atual inclui ambos os campos, com contagem e lista derivados de `dia.get('orfas') or []`: `app/ponto/services/cartorio.py:742-745`.

## 12/11 19:32 · sessao 1d6600f7c1f6 · TRANCA-VIVAS-08 · verificador · origem=Code

1. **Classe: campo com significado impreciso — ressalva, sem bloqueante novo demonstrado.** `ficam_com_o_admin` agora é a união, sem duplicatas, de respostas destinadas ao DP, resgatadas vivas e vivas por toque do admin. Portanto pode aumentar mesmo sem resposta do colaborador. O CLI continua publicando esse agregado como `decidir_pelo_dp`, e a ferramenta da mensageria ainda descreve “resposta do colaborador sem veredito”. Essa descrição já era incompleta para as resgatadas; a obra amplia a diferença para as tocadas. (`app/chamados/services/competencia_trancada.py:343`; `app/chamados/management/commands/chamados_em_competencia_trancada.py:20`; `mensageria/nucleo/ferramentas.py:2031`; `app/chamados/tests/test_tranca_vivas_08.py:131`; `app/chamados/tests/test_c7_red_minimos.py:110`)

   Outra precisão necessária: `resgatado_em` não significa exclusivamente “resgatado pelo admin”. A porta também aceita resgate por resposta do colaborador e conserva o **primeiro** carimbo. O contador reutiliza esse discriminador existente; não verifica novamente quem realizou o resgate. (`app/chamados/services/arquivo.py:180`; `app/chamados/services/arquivo.py:190`; `app/chamados/services/materializacao.py:724`; `app/chamados/services/competencia_trancada.py:117`)

2. **Classe: censo de escritores e leitores de `veredito_celula`.** Os escritores produtivos encontrados são estes; nenhum deles produz ato humano sem `via` e sem `acoes`. (`app/ponto/portas/celula.py:1027`; `app/ponto/portas/celula.py:1042`; `app/ponto/services/veredito_celula.py:356`)

   | Papel | Escritor/leitor e significado |
   |---|---|
   | Escritor físico da auditoria | `evento` grava `acao`, usuário e `valor_depois`; `registrar_log` delega a ele. (`app/core/observ.py:134`; `app/core/auditoria.py:22`) |
   | Lavra da porta | Tanto atualização apenas de status quanto lavra completa gravam `valor_depois` com `via`, `veredito`, `protestos` e `status_dia`. (`app/ponto/portas/celula.py:1027`; `app/ponto/portas/celula.py:1042`) |
   | Despachante Resolver dia | Grava `veredito`, `acoes` e `erros`; também solicita lavra pela via `admin`. (`app/ponto/services/veredito_celula.py:352`; `app/ponto/services/veredito_celula.py:356`) |
   | Chamadores humanos | A view Resolver dia e declarar falta chegam ao mesmo despachante. (`app/ponto/views.py:3339`; `app/ponto/services/declarar_dia.py:213`) |
   | Chamadores de sistema | O cartório chama a porta com `via='cartorio'` nos três caminhos encontrados. (`app/ponto/services/cartorio.py:934`; `app/ponto/services/cartorio.py:1292`; `app/ponto/services/cartorio.py:1853`) |
   | Usuário herdado | Quando `ator` não vem preenchido, `evento` pode usar o usuário autenticado da request. Logo, usuário preenchido sozinho não demonstra ato humano. (`app/core/observ.py:108`; `app/core/observ.py:113`) |
   | Leitor do toque | Lê objeto, instante e JSON; aceita `via='admin'` **ou presença** de `acoes`. (`app/chamados/juizes.py:1295`; `app/chamados/juizes.py:1298`) |
   | Outro leitor específico | `decisao_do_dia` seleciona células atualmente com via `admin`, depois lê todas as linhas dessa ação para obter o último usuário/ator. **Não filtra a via do JSON do log.** (`app/ponto/services/veredito_celula.py:110`; `app/ponto/services/veredito_celula.py:115`; `app/ponto/services/veredito_celula.py:122`) |
   | Consumidores dessa decisão | Calendário e API do espelho. (`app/colaboradores/services/calendario.py:415`; `app/api/views.py:1410`) |
   | Leitores genéricos | Tela de auditoria, foto E4B, contagem de escritas dos crons e censo de portas; esses caminhos não aplicam o novo predicado humano sobre `via`/`acoes`. (`app/core/views.py:705`; `app/chamados/services/foto_e4b.py:322`; `app/chamados/services/vigia_loop.py:86`; `app/core/portas_da_tela.py:152`) |

   Nos testes, escrevem logs sintéticos os helpers E2 e SNC2 e o teste novo; `test_signal_ancora` fornece a tupla simulada. Também leem essas linhas os testes de porta, status lavrado e folga sem adesão. (`app/chamados/tests/test_e2_tarefa_do_dia.py:1068`; `app/chamados/tests/test_snc2_toque_em_lote.py:59`; `app/chamados/tests/test_tranca_vivas_08.py:90`; `app/chamados/tests/test_tranca_vivas_08.py:105`; `app/escala/tests/test_signal_ancora.py:247`; `app/ponto/tests/test_porta_celula.py:90`; `app/ponto/tests/test_porta_celula.py:149`; `app/ponto/tests/test_e3_status_lavrado.py:257`; `app/ponto/tests/test_x1_folga_sem_adesao.py:305`)

3. **Classe: mudança de comportamento do juiz — alcance exato.** Comparado ao trecho anterior fornecido, o novo filtro remove do conjunto de toques as linhas cujo JSON não seja dicionário ou não contenha nenhuma das assinaturas aceitas. Isso inclui lavra `cartorio`, JSON vazio/nulo e qualquer outra via sem `acoes`; não é uma exclusão codificada apenas para a palavra `cartorio`. Atualmente a porta admite somente `cartorio` e `admin`. (`app/chamados/juizes.py:1298`; `app/escala/models.py:1769`)

   `dia_da_pergunta_trancado` muda de **False para True** quando havia tranca viva e o único fundamento para destrancar eram essas linhas agora rejeitadas: nenhum cobrar válido, nenhum Resolver dia aceito e nenhum desarquivar válido posterior permanece. `pergunta_viva` pode então mudar de **True para False**, porque a tranca é examinada antes da resposta e dos demais fundamentos de vida. O filtro não acrescenta fundamento novo para ressuscitar pergunta. (`app/chamados/juizes.py:1069`; `app/chamados/juizes.py:1076`; `app/chamados/juizes.py:1275`; `app/chamados/juizes.py:1302`; `app/chamados/juizes.py:1325`; `app/chamados/juizes.py:1401`; `app/chamados/juizes.py:1405`)

   Os caminhos humanos atuais encontrados permanecem reconhecidos pelas duas assinaturas. Histórico sem `via`, mas com `acoes`, também permanece reconhecido — inclusive `acoes=[]` ou `acoes=None`, pois se testa a existência da chave. Histórico sem ambas passa a ser ignorado, independentemente da natureza real do ato. Que todos os atos humanos antigos possuíam essas assinaturas: **[não verificado]**. (`app/ponto/services/veredito_celula.py:352`; `app/ponto/services/veredito_celula.py:358`; `app/chamados/juizes.py:1298`)

4. **Classe: autoridade única — escritores dos retornos e leitores da tranca.** Não há campo persistido novo nesses retornos: os produtores são as funções abaixo, com caches locais/de consulta já existentes. (`app/chamados/juizes.py:902`; `app/chamados/juizes.py:1149`; `app/chamados/juizes.py:1232`)

   | Retorno | Produtor e leitores produtivos diretos |
   |---|---|
   | `_toque_humano_nos_dias`: conjunto de índices | Produzido em `app/chamados/juizes.py:1262` e retornado em `app/chamados/juizes.py:1327`; consumido por `_admin_tocou_os_dias_depois` em `app/chamados/juizes.py:1229`. |
   | `_admin_tocou_os_dias_depois`: lista de booleanos | Produzida em `app/chamados/juizes.py:1210`; lida pelo avulso em `app/chamados/juizes.py:1195`, pelo lote da tranca em `app/chamados/juizes.py:1077` e pelo contador em `app/chamados/services/competencia_trancada.py:155`. |
   | `_admin_tocou_o_dia_depois`: booleano | Delega ao lote em `app/chamados/juizes.py:1195`; leitor em `app/chamados/juizes.py:1076`. |
   | `dias_trancados`: lista de booleanos | Produzida em `app/chamados/juizes.py:1023` e preenchida em `app/chamados/juizes.py:1079`; leitores: avulso `app/chamados/juizes.py:991`, consulta `app/chamados/juizes.py:1149` e tarefa do dia `app/chamados/services/tarefa_do_dia.py:422`. |
   | `dia_da_pergunta_trancado`: booleano | Lê consulta ou lote de um em `app/chamados/juizes.py:987` e `app/chamados/juizes.py:991`; leitores: C6 `app/chamados/juizes.py:1405`, respostas sem veredito `app/chamados/models.py:1847`, reconciliador `app/chamados/reconciliador.py:668` e `app/chamados/reconciliador.py:748`, emissão `app/chamados/services/disputa_emissao.py:1970`. |
   | `pergunta_viva`: booleano | Corpo em `app/chamados/juizes.py:1399`; fachada/memo em `app/chamados/juizes.py:862`. O modelo reexporta a autoridade e a property `viva` delega a ela. (`app/chamados/models.py:4084`; `app/chamados/models.py:3376`) |

   Os leitores de teste diretamente relacionados à tranca incluem E2, SNC2, Signal Âncora, E1-v9, pergunta somente ao C6, chamado do dia e o teste novo. (`app/chamados/tests/test_e2_tarefa_do_dia.py:1135`; `app/chamados/tests/test_e2_tarefa_do_dia.py:1194`; `app/chamados/tests/test_snc2_toque_em_lote.py:228`; `app/escala/tests/test_signal_ancora.py:267`; `app/chamados/tests/test_e1_v9.py:430`; `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:94`; `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:264`; `app/chamados/tests/test_tranca_vivas_08.py:73`)

5. **Classe: quem mais lê C6 — censo das chamadas produtivas diretas.** Além da fachada/property acima, encontrei estes consumidores; os aliases importados de `chamados.models` também chegam à mesma autoridade. (`app/chamados/models.py:4084`)

   | Arquivo | Linhas das chamadas |
   |---|---|
   | `app/chamados/juizes.py` | 1496, 1501, 1513, 1583, 1684, 2346 |
   | `app/chamados/models.py` | 368, 1510, 1897, 3045, 3140, 3376 |
   | `app/chamados/reconciliador.py` | 399, 662, 759, 1087, 1595 |
   | `app/chamados/views.py` | 1242, 1257, 1417 |
   | `app/chamados/relatorios_disputa.py` | 18 |
   | `app/chamados/services/fio.py` | 238, 606, 782, 961, 2129, 2134 |
   | `app/chamados/services/disputa_emissao.py` | 1821, 1966, 2048 |
   | `app/chamados/services/competencia_trancada.py` | 144, 167 |
   | `app/chamados/services/pergunta_no_app.py` | 58, 170 |
   | `app/chamados/services/vigia_fio.py` | 755, 813 |
   | `app/chamados/services/relavra_via.py` | 242 |
   | `app/chamados/services/acoes_chamado.py` | 355 |
   | `app/chamados/services/tarefa_do_dia.py` | 491 |
   | `app/chamados/management/commands/auditar_invariantes_chamados.py` | 202, 540, 858, 894, 1653, 1710, 1769, 1782 |
   | `app/chamados/management/commands/disparar_perguntas_competencia.py` | 109 |
   | `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py` | 449 |
   | `app/chamados/management/commands/reabrir_orfas_enterradas.py` | 83 |
   | `app/api/views.py` | 2608 |
   | `app/colaboradores/services/calendario.py` | 476 |
   | `app/ponto/services/dia_do_colab.py` | 71 |

   Leitores diretos adicionais de teste, por módulo, foram encontrados nos seguintes sítios; a lista identifica módulos consumidores, não certifica sua execução. (`app/chamados/tests/test_c7_red_minimos.py:124`)

   - `app/colaboradores/tests/test_calendario_ultimo_recurso_le_o_c6.py:56`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:367`; `app/ponto/tests/test_bug114_fechador_de_dois_dias.py:239`; `app/ponto/tests/test_hx_cartorio_ressuscita.py:56`; `app/ponto/tests/test_cobparc_marco_no_periodo.py:386`; `app/ponto/tests/test_o261_cobrado_le_o_fio.py:134`; `app/ponto/tests/test_cods_do_ato.py:124`; `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:84`; `app/escala/tests/test_f3_beco1_folga_na_lista.py:80`.
   - `app/chamados/tests/test_e2_tarefa_do_dia.py:1135`; `app/chamados/tests/test_contract_pergunta_viva.py:56`; `app/chamados/tests/test_e4b_c4_orm.py:473`; `app/chamados/tests/test_orfa_sem_candidato.py:70`; `app/chamados/tests/test_selo_juizes_orfaos.py:73`; `app/chamados/tests/test_o261_lavra_le_a_lampada.py:138`; `app/chamados/tests/test_pergunta_no_app.py:85`; `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:186`; `app/chamados/tests/test_o44_tranca_nao_barra_o_toque.py:177`; `app/chamados/tests/test_e1_v9.py:171`.
   - `app/chamados/tests/test_limbo_nao_fecha_com_filho_no_defeito.py:85`; `app/chamados/tests/test_selo_i8b_resolvedora_cruzada.py:83`; `app/chamados/tests/test_e4a_relogio_injetado.py:34`; `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:98`; `app/chamados/tests/test_viva_delega_ao_juiz.py:42`; `app/chamados/tests/test_tranca_de_verdade.py:52`; `app/chamados/tests/test_reabrir_orfas_enterradas.py:103`; `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:95`; `app/chamados/tests/test_o261e_f1_porta_encerramento.py:75`; `app/chamados/tests/test_invariantes_disputa.py:145`.
   - `app/chamados/tests/test_c7_red_minimos.py:124`; `app/chamados/tests/test_pvmuda_carimbo_em_voo.py:76`; `app/chamados/tests/test_e4b_c11_orm.py:145`; `app/chamados/tests/test_o261_vigia_fio.py:134`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:668`; `app/chamados/tests/test_e4b_c12_orm.py:61`; `app/chamados/tests/test_o261e_f5b_juiz_porta.py:106`; `app/chamados/tests/test_e4a2_nunca_bateu.py:158`; `app/chamados/tests/test_resposta_nao_e_fantasma.py:86`; `app/chamados/tests/test_bug117_marco_no_espelho.py:128`.
   - `app/chamados/tests/test_t34_furo_sob_limbo.py:55`; `app/chamados/tests/test_pergunta_viva.py:18`; `app/chamados/tests/test_competencia_trancada.py:127`; `app/chamados/tests/test_selo_i6_ouve_a_lei.py:114`; `app/chamados/tests/test_e4b_c6_orm.py:80`; `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:150`; `app/chamados/tests/test_e1_v6_horizonte.py:538`; `app/chamados/tests/test_o261_pergunta_viva_teto.py:55`; `app/chamados/tests/test_selo_i8_fio_solto.py:95`; `app/chamados/tests/test_hx_tipo_pelo_marco.py:161`.
   - `app/chamados/tests/test_o261e_f2_fila.py:246`; `app/chamados/tests/test_porta_veredito_pergunta.py:427`; `app/chamados/tests/test_adesao_por_colab.py:61`; `app/chamados/tests/test_migrar_data_evento_dia_do_turno.py:104`; `app/chamados/tests/test_reconciliador_c1_escopo.py:48`; `app/chamados/tests/test_selo_celula_absolveu.py:72`; `app/chamados/tests/test_o261_resposta_vazia.py:113`; `app/chamados/tests/test_e1_entradas.py:152`; `app/chamados/tests/test_selo_memo_pergunta_viva.py:86`; `app/chamados/tests/test_o261_recusa_fala.py:105`.
   - `app/chamados/tests/test_selo_emissor_fio_mudo.py:269`; `app/chamados/tests/test_snc2_toque_em_lote.py:90`; `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:98`; `app/chamados/tests/test_reabrir_pendente_pelo_juiz.py:67`; `app/chamados/tests/test_orfas_pelo_evento.py:102`; `app/chamados/tests/test_jv3_reconciliar_vira_vigia.py:151`; `app/chamados/tests/test_b2d_resposta_visivel.py:99`. O teste novo também chega ao C6 pelo helper herdado. (`app/chamados/tests/test_tranca_vivas_08.py:86`; `app/chamados/tests/test_c7_red_minimos.py:124`)

6. **Classe: contador versus universo — conforme no recorte examinado.** `medir()` não mede todas as perguntas globais: mede as da empresa e da janela do PF. Para perguntas de fato, esse recorte coincide com o do C20: mesmo `data_evento`, mesma empresa e janelas da mesma autoridade. As duas pontas da janela são inclusivas. Conversas saem pelo discriminador existente; o contador consulta efetivamente `pergunta_viva`, sem reproduzir seus degraus. (`app/chamados/services/competencia_trancada.py:99`; `app/chamados/services/competencia_trancada.py:119`; `app/chamados/services/competencia_trancada.py:141`; `app/chamados/services/competencia_trancada.py:144`; `app/chamados/juizes.py:334`; `app/chamados/juizes.py:1062`; `app/ponto/janelas.py:54`; `app/ponto/janelas.py:60`)

   A classificação é: conversa fora do universo de fatos; fato vivo resgatado em lista própria; demais fatos vivos separados pelo mesmo juiz do toque usado pelo C20. Portanto **residual não significa total de vivas**: significa vivas sem as exclusões declaradas. As exclusões não criam um segundo juiz de vitalidade. (`app/chamados/services/competencia_trancada.py:141`; `app/chamados/services/competencia_trancada.py:144`; `app/chamados/services/competencia_trancada.py:145`; `app/chamados/services/competencia_trancada.py:155`; `app/chamados/services/competencia_trancada.py:158`; `app/chamados/juizes.py:1077`)

   | Campo/lista | Escritor e leitores encontrados |
   |---|---|
   | Todas as listas | Inicializadas por `medir`. (`app/chamados/services/competencia_trancada.py:89`) |
   | `fora_conversa` | Acrescida em `medir`; lida nominalmente nos testes novos. Sem consumidor produtivo nominal adicional encontrado. (`app/chamados/services/competencia_trancada.py:142`; `app/chamados/tests/test_tranca_vivas_08.py:151`) |
   | `vivas_resgatadas` | Acrescida em `medir`; lida na união de `medir_frota` e na exclusão do teste de conversa. (`app/chamados/services/competencia_trancada.py:149`; `app/chamados/services/competencia_trancada.py:343`; `app/chamados/tests/test_tranca_vivas_08.py:154`) |
   | `vivas_tocadas_pelo_admin` | Acrescida em `medir`; lida por `medir_frota` e testes novos. (`app/chamados/services/competencia_trancada.py:158`; `app/chamados/services/competencia_trancada.py:343`; `app/chamados/tests/test_tranca_vivas_08.py:137`; `app/chamados/tests/test_tranca_vivas_08.py:143`) |
   | `vivas_apesar_da_tranca` | Acrescida em `medir`; convertida em quantidade e somada por `medir_frota`; lida pelo CLI, API e testes novos/mutante. (`app/chamados/services/competencia_trancada.py:158`; `app/chamados/services/competencia_trancada.py:347`; `app/chamados/services/competencia_trancada.py:348`; `app/chamados/management/commands/chamados_em_competencia_trancada.py:22`; `app/api/views_mensageria.py:1574`; `app/chamados/tests/test_tranca_vivas_08.py:129`; `app/chamados/tests/test_tranca_vivas_08.py:162`; `app/chamados/tests/test_tranca_vivas_08.py:193`) |
   | `perguntas_lidas_trancadas` | Acrescida quando o C6 devolve falso; lida pelos testes N1 e novo. O nome não demonstra que a tranca foi a causa específica da morte. (`app/chamados/services/competencia_trancada.py:151`; `app/chamados/services/competencia_trancada.py:152`; `app/chamados/tests/test_n1_perguntas_do_app.py:229`; `app/chamados/tests/test_tranca_vivas_08.py:144`) |
   | `decidir_pelo_dp` | Acrescida antes da separação das conversas, preservando a seleção de escrita; consumida pela pauta, agregado da frota e comando de encerramento. (`app/chamados/services/competencia_trancada.py:137`; `app/chamados/services/competencia_trancada.py:139`; `app/chamados/services/competencia_trancada.py:232`; `app/chamados/services/competencia_trancada.py:246`; `app/chamados/services/competencia_trancada.py:343`; `app/chamados/management/commands/encerrar_por_competencia_trancada.py:38`) |

   Outros leitores do dicionário de `medir` são `encerrar` e `disparar_no_ato`; ambos continuam dirigidos pelas listas de ação e sua quantidade, sem transformar as novas exclusões em escrita. (`app/chamados/services/competencia_trancada.py:190`; `app/chamados/services/competencia_trancada.py:206`; `app/chamados/services/competencia_trancada.py:221`; `app/chamados/services/competencia_trancada.py:303`)

7. **Classe: semântica temporal — não encontrei mudança de unidade, fuso ou fronteira.** A comparação continua usando o instante de criação da **trilha**, estritamente posterior a `fechado_em`; igualdade não conta. Não se compara o dia civil da request nem `veredito_em`. Os campos são timestamps, e a configuração usa timezone habilitado em São Paulo. (`app/chamados/juizes.py:1260`; `app/chamados/juizes.py:1302`; `app/core/models.py:307`; `app/ponto/models.py:926`; `app/config/settings/base.py:93`; `app/config/settings/base.py:95`)

   O dia da pergunta continua sendo o dia do turno para fatos; conversa devolve `None`. A tarefa do dia documenta expressamente que sua tranca é a atual, não uma reconstrução da tranca no instante `agora`. A obra não introduz outra leitura de relógio nesse filtro. (`app/chamados/juizes.py:334`; `app/chamados/juizes.py:1291`; `app/chamados/services/tarefa_do_dia.py:359`)

8. **Classe: consumidor da mensageria/observabilidade.** O campo novo é escrito no JSON exclusivamente com o total recebido de `medir_frota`; o cliente HTTP transporta o JSON, a ferramenta acrescenta `cliente`, e o montador incorpora o bloco quando solicitado. Não há derivação paralela nesse caminho. (`app/api/views_mensageria.py:1563`; `app/api/views_mensageria.py:1574`; `mensageria/nucleo/core_client.py:239`; `mensageria/nucleo/ferramentas.py:2035`; `mensageria/nucleo/ferramentas.py:2037`; `mensageria/nucleo/ferramentas.py:2059`; `mensageria/nucleo/ferramentas.py:419`)

   O placar antigo **não lê o residual novo

[truncado em 20000 de 25920 caracteres]

## 12/11 19:28 · sessao a0a9efc3b6f7 · - · - · origem=Ronald (tmux)

1. **Universo do contador coincide com o recorte do C20 para perguntas de fato.** `medir()` seleciona empresa e `data_evento` entre início/fim inclusivos; elimina conversa pelo discriminador `_dia_de_turno_da_pergunta` e pergunta a `pergunta_viva` para cada fato. O C20 usa o mesmo dia, empresa e janela de competência. Não encontrei regra paralela de competência introduzida. (`app/chamados/services/competencia_trancada.py:99`, `:119`, `:131`, `:141`, `:144`; `app/chamados/juizes.py:334`, `:991`, `:1062`; `app/ponto/janelas.py:54`, `:60`)

2. **Exclusões reutilizam fontes existentes.** Conversa: `_dia_de_turno_da_pergunta`; toque: `_admin_tocou_os_dias_depois`, também usado pelo C20; resgatada: mantém o discriminador armazenado `resgatado_em > fechado_em`, aplicado aos chamados vivos cuja competência o C14 reconhece. O primeiro carimbo de resgate é escrito por `arquivo.levantar`; tanto desarquivar humano quanto resposta do colaborador podem solicitar esse carimbo — “resgatada pelo admin” é descrição mais estreita que o campo existente. (`app/chamados/services/competencia_trancada.py:101`, `:105`, `:117`, `:132`, `:141`, `:155`; `app/chamados/juizes.py:1076`; `app/chamados/services/arquivo.py:180`, `:190`, `:205`; `app/chamados/views_arquivar.py:77`; `app/chamados/services/materializacao.py:724`)

3. **Censo dos escritores/leitores dos campos novos e alterados.** `medir` inicializa todas as listas; escreve `fora_conversa`, `vivas_resgatadas`, `perguntas_lidas_trancadas`, `vivas_tocadas_pelo_admin` e `vivas_apesar_da_tranca`. `medir_frota` lê as três listas de vivas, calcula `ficam_com_o_admin` por união de IDs, calcula o residual por tamanho e soma por PF com `reaberto_em=NULL`. `fora_conversa` não tem consumidor produtivo nominal adicional encontrado; `perguntas_lidas_trancadas` é verificada pelo teste N1 e pelos testes novos. (`app/chamados/services/competencia_trancada.py:89`, `:142`, `:149`, `:152`, `:158`, `:341`, `:343`, `:347`, `:348`; `app/chamados/tests/test_n1_perguntas_do_app.py:229`; `app/chamados/tests/test_tranca_vivas_08.py:129`, `:137`, `:144`, `:151`, `:154`, `:162`, `:190`)

4. **Outros consumidores de `medir`.** `encerrar` recebe todo o dicionário, mas só usa listas de escrita e `decidir_pelo_dp`; `disparar_no_ato` usa tranca e quantidade de ações. O comando de encerramento imprime listas de ação/exclusões antigas, sem ler nominalmente as novas listas. Não encontrei outro escritor produtivo das novas listas. (`app/chamados/services/competencia_trancada.py:190`, `:192`, `:206`, `:221`, `:232`, `:246`, `:303`; `app/chamados/management/commands/encerrar_por_competencia_trancada.py:29`, `:36`)

5. **Consumidor mensageria: transporte integral, sem derivação nova.** API lê `medir_frota` e publica o novo residual; `core_client` obtém o JSON, `ferramentas.chamados_em_competencia_trancada` acrescenta `cliente` e devolve o dicionário integral; `competencia_trancada_da_pergunta` o inclui quando a pergunta pede competência trancada; o montador de contexto incorpora esse bloco. Portanto o campo novo chega a esse consumidor sem filtro próprio. (`app/api/views_mensageria.py:1562`, `:1574`; `app/api/urls.py:36`; `mensageria/nucleo/core_client.py:239`; `mensageria/nucleo/ferramentas.py:2035`, `:2037`, `:2051`, `:2059`, `:419`)

6. **Classe: campo/rótulo com significado impreciso — dívida anterior ampliada, não bloqueante novo isolado.** O agregado `ficam_com_o_admin` passa a incluir também perguntas vivas por toque, mesmo sem resposta, e elimina repetição de IDs. A ferramenta ainda o descreve como “resposta do colaborador sem veredito”; o CLI imprime esse agregado sob `decidir_pelo_dp`. Contudo o diff anexado demonstra que ambos já recebiam resgatadas vivas antes, portanto a imprecisão não nasceu nesta obra. O caso novo sem resposta está explícito nas fixtures. (`app/chamados/services/competencia_trancada.py:335`, `:343`; `mensageria/nucleo/ferramentas.py:2031`; `app/chamados/management/commands/chamados_em_competencia_trancada.py:20`; `app/chamados/tests/test_tranca_vivas_08.py:131`; `app/chamados/tests/test_c7_red_minimos.py:110`)

7. **Classe: observabilidade — placar antigo não acompanha o residual.** CLI imprime residual próprio, mas `bin/placar_code.sh` extrai apenas `chamados_em_competencia_trancada`, a soma antiga de chamados/perguntas/disputas. Assim o placar pode mostrar zero mesmo com residual positivo; a API e a ferramenta mensageria recebem o residual separadamente. Não encontrei no aval fornecido exigência explícita de migrar esse placar, portanto não elevaria a bloqueante só por isso. (`app/chamados/management/commands/chamados_em_competencia_trancada.py:22`, `:25`; `bin/placar_code.sh:287`; `app/api/views_mensageria.py:1574`; `mensageria/nucleo/ferramentas.py:2037`)

8. **Testes antigos de resgate não dependem da semântica antiga do residual.** Defendem que desarquivar devolve chamado/pergunta e que a passada seguinte respeita o admin; não leem `vivas_apesar_da_tranca`. O teste do placar continua cobrando só o agregado antigo. O teste novo cobre residual positivo, toque excluído e conversa separada, mas falta caso novo nominal para `vivas_resgatadas`/união sem duplicata; execução e GREEN permanecem [não verificado]. (`app/chamados/tests/test_competencia_trancada.py:164`, `:177`, `:201`, `:219`; `app/chamados/tests/test_tranca_vivas_08.py:123`, `:131`, `:146`, `:158`)

**Neste subescopo, nenhum bloqueante novo comprovado; aprovação final depende dos demais eixos da revisão e da evidência de testes [não verificado].** [nao verificado]

## 12/11 19:27 · sessao 6c1e9266396f · - · - · origem=Ronald (tmux)

1. **Escritores atuais: nenhum ato humano de produção encontrado sem `via` e sem `acoes`.** A porta escreve as duas variantes do log com `via`: atualização só de status (`app/ponto/portas/celula.py:1027`) e lavra completa (`app/ponto/portas/celula.py:1042`). O despachante escreve `acoes` e `erros` (`app/ponto/services/veredito_celula.py:356`). Ambos chegam ao escritor físico único `evento`, diretamente ou por `registrar_log` (`app/core/auditoria.py:22`; `app/core/observ.py:134`). O método do modelo apenas delega (`app/escala/models.py:1826`).

2. **Chamadores de produção da porta:** despachante humano com `via='admin'` (`app/ponto/services/veredito_celula.py:352`); cartório com `via='cartorio'` em completamento de status, ramo nunca bateu e julgamento normal (`app/ponto/services/cartorio.py:934`; `app/ponto/services/cartorio.py:1292`; `app/ponto/services/cartorio.py:1853`). `ator=None` pode herdar o usuário autenticado da request, explicando por que presença de usuário não distingue o ato (`app/core/observ.py:108`; `app/core/observ.py:113`; `app/core/observ.py:118`). O middleware instala/resetta esse contexto (`app/core/middleware.py:76`).

3. **Ato humano atual preservado.** A view Resolver dia chama o despachante (`app/ponto/views.py:3339`); declarar falta chama o mesmo despachante (`app/ponto/services/declarar_dia.py:213`). As assinaturas desses caminhos continuam aceitas pelo novo filtro (`app/chamados/juizes.py:1298`). Declarar folga escreve folga e fecha o fio, sem produzir esta ação diretamente (`app/ponto/services/declarar_dia.py:191`; `app/ponto/services/declarar_dia.py:205`).

4. **Histórico: aceitação e limite preciso.** Log antigo sem `via`, mas com chave `acoes`, continua contando; `acoes=[]` ou `acoes=None` também contam porque o filtro testa presença da chave. Log nulo, não dicionário ou dicionário sem `via='admin'` e sem `acoes` passa a ser ignorado (`app/chamados/juizes.py:1298`). Que **todos os atos humanos históricos** traziam uma dessas assinaturas é **[não verificado]**: o escritor presente demonstra o formato atual, não versões anteriores (`app/ponto/services/veredito_celula.py:356`). O teste de linha sem assinatura codifica rejeição; não comprova a natureza do acervo (`app/chamados/tests/test_tranca_vivas_08.py:88`).

5. **Leitores específicos da ação.**
   - Juiz do toque: lê objeto, instante e JSON; admite assinatura humana e exige instante estritamente posterior ao fechamento (`app/chamados/juizes.py:1295`; `app/chamados/juizes.py:1302`).
   - `decisao_do_dia`: filtra a **via atual da célula**, não a via do JSON do log (`app/ponto/services/veredito_celula.py:110`). Em seguida lê todas as linhas dessa ação para obter o último usuário/ator (`app/ponto/services/veredito_celula.py:115`). Portanto a frase do brief “já filtra via admin” precisa dessa precisão. A pergunta dele é quais dias o admin decidiu, e não se estão destrancados (`app/ponto/services/veredito_celula.py:85`).
   - Consumidores de `decisao_do_dia`: calendário (`app/colaboradores/services/calendario.py:415`) e espelho API (`app/api/views.py:1410`).

6. **Leitores genéricos capazes de incluir essas linhas:** tela de auditoria, com seleção livre de ação (`app/core/views.py:705`; `app/core/views.py:709`), cuja tabela exibe metadados/descrição e não interpreta `via`/`acoes` (`app/templates/core/log_auditoria.html:56`; `app/templates/core/log_auditoria.html:73`); foto E4B, por modelo/objeto, preservando auditoria (`app/chamados/services/foto_e4b.py:311`; `app/chamados/services/foto_e4b.py:322`); contador de escritas por cron, indiferente à ação/JSON (`app/chamados/services/vigia_loop.py:86`). O censo de portas também pode contar ações detectadas como exclusivas, sem interpretar esse JSON (`app/core/portas_da_tela.py:149`; `app/core/portas_da_tela.py:152`).

7. **Escritores/leitores de teste além da produção:** helpers adaptados escrevem logs sintéticos (`app/chamados/tests/test_e2_tarefa_do_dia.py:1068`; `app/chamados/tests/test_snc2_toque_em_lote.py:59`); teste novo cria linha sem assinatura e chama `registrar_log` com `acoes` (`app/chamados/tests/test_tranca_vivas_08.py:90`; `app/chamados/tests/test_tranca_vivas_08.py:105`). O novo teste também lê/ajusta timestamps e confere `via`/usuário (`app/chamados/tests/test_tranca_vivas_08.py:43`; `app/chamados/tests/test_tranca_vivas_08.py:66`). Leitores de contagem de lavras existentes: porta (`app/ponto/tests/test_porta_celula.py:90`; `app/ponto/tests/test_porta_celula.py:149`), status lavrado (`app/ponto/tests/test_e3_status_lavrado.py:257`) e folga sem adesão (`app/ponto/tests/test_x1_folga_sem_adesao.py:305`). `test_signal_ancora` alimenta tuplas simuladas desse log (`app/escala/tests/test_signal_ancora.py:244`).

8. **Adaptações preservam as assertivas observadas.** E2 continua exigindo ação `perguntar/sistema`, agora com JSON equivalente ao escritor real (`app/chamados/tests/test_e2_tarefa_do_dia.py:1109`; `app/chamados/tests/test_e2_tarefa_do_dia.py:1111`). SNC2 conserva a matriz esperada de vivos/mortos, incluindo outro colaborador e data antes da tranca (`app/chamados/tests/test_snc2_toque_em_lote.py:126`; `app/chamados/tests/test_snc2_toque_em_lote.py:136`). Signal âncora conserva `{0}` para dois itens com `desde` diferentes e os testes que matam `>`→`>=` (`app/escala/tests/test_signal_ancora.py:258`; `app/escala/tests/test_signal_ancora.py:269`). Não encontrei afrouxamento dessas asserções nesses trechos.

9. **Sem defeito corrente comprovado no filtro desta subtarefa.** A ação possui dois formatos documentáveis, ambos preservados conforme seu escritor; o filtro passa a distinguir suas assinaturas (`app/ponto/portas/celula.py:1044`; `app/ponto/services/veredito_celula.py:358`; `app/chamados/juizes.py:1298`). A completude histórica permanece **[não verificado]**. Execução de testes e produção também **[não verificado]**.

## 12/11 19:27 · sessao 2fd4fb45de82 · - · - · origem=Ronald (tmux)

1. **Adaptações preservam as assertivas anteriores.** Em `test_e2_tarefa_do_dia`, a fixture passa a reproduzir o payload real com `acoes`; continuam as verificações de tranca antes do toque e tarefa após Resolver dia, por colaborador (`app/chamados/tests/test_e2_tarefa_do_dia.py:1099-1111`; escritor real: `app/ponto/services/veredito_celula.py:356-358`). Em `test_snc2_toque_em_lote`, permanece o mapa exato de nove respostas e a igualdade lote/avulso; ambos os logs, inclusive o de outro colaborador, recebem a assinatura (`app/chamados/tests/test_snc2_toque_em_lote.py:118-140`). Em `test_signal_ancora`, permanece `{0}` para dois itens com instantes diferentes, as exclusões de usuário e o SQL estritamente `>`; os mutantes `>=` continuam nas três etapas (`app/escala/tests/test_signal_ancora.py:237-278`).

2. **O teste da lavra/cartório tem mordida efetiva no código descrito.** A fixture chama a porta real sob `REQUISICAO_DA_TRILHA` humana e confere usuário herdado e `via='cartorio'`; depois exige dia trancado e pergunta morta (`app/chamados/tests/test_tranca_vivas_08.py:56-86`). O mutante remove precisamente o filtro no fonte real e exige que a mesma fixture passe a destrancar (`app/chamados/tests/test_tranca_vivas_08.py:169-186`). Há contraexemplos separados para lavra admin, `registrar_log` com `acoes` e ato anterior à tranca (`app/chamados/tests/test_tranca_vivas_08.py:95-118`).

3. **O teste do contador é artificial, mas testa a ligação ao juiz e o residual.** Ele substitui exclusivamente `dia_da_pergunta_trancado`, confere que `pergunta_viva` ficou verdadeira e chama `medir` real, exigindo o ID no residual; a frota também é chamada realmente (`app/chamados/tests/test_tranca_vivas_08.py:123-129`; `app/chamados/tests/test_tranca_vivas_08.py:158-163`). A mordida transforma o destino do ID no corpo real de `medir` e demonstra residual vazio no mutante (`app/chamados/tests/test_tranca_vivas_08.py:188-195`). **Limite:** o mutante não restaura literalmente o antigo ramo resgatado, apesar do nome; força toda viva à exclusão admin (`app/chamados/tests/test_tranca_vivas_08.py:190-191`).

4. **Cobertura incompleta declarada:** o arquivo novo verifica conversa, toque admin, cartório e soma da frota, mas não contém caso de pergunta resgatada em `vivas_resgatadas` (`app/chamados/tests/test_tranca_vivas_08.py:121-163`). Cobertura dessa exclusão em outros testes: [não verificado].

5. **Relógio e travas:** os testes novos congelam o tempo e deslocam explicitamente a trilha relativamente a `fechado_em`; não introduzem leitura nova de relógio (`app/chamados/tests/test_tranca_vivas_08.py:30-49`). O trecho alterado do juiz continua sendo consulta de logs e classificação em memória, sem aquisição de trava (`app/chamados/juizes.py:1291-1303`); o trecho alterado de `medir` apenas consulta autoridades e preenche listas (`app/chamados/services/competencia_trancada.py:131-158`). Ordem global do serviço Resolver dia permanece colaborador primeiro, antes dos demais atos (`app/ponto/services/veredito_celula.py:224-227`).

6. **Prova executada:** RED anterior à cura, GREEN, suite inteira, cronologia dos testes e prova de frota pós-cura: [não verificado]. A declaração “escritos antes da cura” aparece apenas no docstring (`app/chamados/tests/test_tranca_vivas_08.py:10`).

**Conclusão deste recorte:** nenhum bloqueante próprio demonstrado nas três adaptações ou na mordida dos testes; execução e resultado permanecem [não verificado]. [nao verificado]

## 12/11 19:26 · sessao ecf65b15b5ad · TIPO-NA-FONTE · verificador · origem=Code

1. **BLOQUEANTE — regressão de comportamento / fuso no plantio.** O novo `_plantio_preenche_furo_na_data` passa `instante_marco` e `instante_luz` ao propositor, mas esses valores vêm sem fuso: `montar_lampadas` produz ISO local *naive*, e `periodos_do_dia` o reconstrói assim. O propositor rejeita esse valor com `ValueError('marca exige fuso')`; o materializador captura a exceção e devolve `False`. (`app/chamados/services/materializacao.py:182`; `app/chamados/services/materializacao.py:189`; `app/ponto/management/commands/gerar_celulas.py:69`; `app/ponto/juiz_batida.py:237`; `app/ponto/propositor.py:25`; `app/chamados/services/materializacao.py:194`)

   Contraexemplo: entrada acesa às 08:03, saída 17:00 apagada, `orfas=[]`, plantio às 17:04. A atribuição anterior com HH:MM aceita; o novo caminho encontra a exceção acima. Isso atinge contagem **conhecida e zero**, independentemente de legado desconhecido. O teste existente exige precisamente a aceitação desse plantio. (`app/chamados/tests/test_hx_plantio_celula.py:30`; `app/chamados/tests/test_hx_plantio_celula.py:34`; `app/chamados/tests/test_hx_plantio_celula.py:35`; `app/ponto/propositor.py:269`)

2. **BLOQUEANTE — migração incompleta dos testes existentes.** Permaneceram cinco assertivas incompatíveis com os produtores novos, que entregam `datetime` na grade e ISO na ata. (`app/escala/utils.py:622`; `app/escala/utils.py:663`; `app/escala/services/ata.py:150`)

   | Expectativa antiga remanescente | Sítio |
   |---|---|
   | `'13:41'` dentro das órfãs da grade | `app/escala/tests/test_ata_x_turno_entrada.py:142` |
   | Grade igual a `['15:00']` | `app/escala/tests/test_saida_valida_cross_meia_noite.py:102` |
   | Grade igual a `['14:03']`, em arquivo parcialmente adaptado | `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:135` |
   | Ata igual a `['03:10', '22:50']` | `app/ponto/tests/test_bateria_lavratura.py:637` |
   | Ata igual a `['08:00', '17:00']` | `app/ponto/tests/test_bateria_lavratura.py:1057` |

   Há também fixture sem a chave `orfas` chamando `ata_do_dia`; a nova leitura obrigatória `dia['orfas']` produz `KeyError`, onde antes o diff fornecido usava `.get(... ) or []`. (`app/chamados/tests/test_emis_ausencia_parcial_cobra.py:60`; `app/chamados/tests/test_emis_ausencia_parcial_cobra.py:68`; `app/chamados/tests/test_emis_ausencia_parcial_cobra.py:72`; `app/ponto/services/cartorio.py:769`; versão-base local [nao verificado])

3. **BLOQUEANTE — selo existente perdeu mordida.** A bateria continua comparando `lampada['luz']`, em HH:MM, com `ata['orfas']`, agora em ISO. A interseção fica vazia mesmo se a mesma batida estiver simultaneamente acesa e órfã; esse teste passa sem defender sua invariante. (`app/ponto/tests/test_bateria_lavratura.py:959`; `app/ponto/tests/test_bateria_lavratura.py:960`; `app/escala/services/ata.py:150`)

   Os testes semelhantes foram adaptados em outros arquivos, mas essa bateria ficou de fora. (`app/ponto/tests/test_o118_orfa_que_acendeu.py:167`; `app/ponto/tests/test_lampada_acende_pela_batida.py:84`)

4. **BLOQUEANTE — catraca não intercepta escritas comuns.** `AtaVigiada` intercepta atribuição individual, mas herda `dict.update`, `dict.__ior__` e `dict.__delitem__`. Depois de uma leitura legítima pelo acessor, `a.update(orfas=...)`, `a |= {'orfas': ...}` e `del a['orfas']` deixam `acessos_crus_ata=0` e satisfazem `exigir_catraca`. Isso permite um segundo escritor passar pelo selo. (`app/escala/services/ata.py:46`; `app/escala/services/ata.py:77`; `app/escala/services/ata.py:114`; `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:17`)

   A fiscalização também depende da injeção explícita de `AtaVigiada`; os dirigidos não tornam automaticamente vigiada toda `CelulaDia.ata` usada pela suite Django. Os mutantes existentes cobrem get, subscript, cópia, dict, pop e setdefault, mas deixam essas escritas herdadas de fora. (`app/escala/tests/test_selo_tipo_na_fonte_orfas.py:25`; `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:43`; `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:55`; `app/escala/tests/test_tipo_na_fonte_orfas_v2.py:117`)

5. **BLOQUEANTE — leitor fora da porta / `None` tratado como zero / derivação paralela.** `cartorio.insumos_da_ata` continua lendo `n_orfas` diretamente com `or 0`. Sem lâmpadas, tanto `{'orfas': ['08:00']}` quanto `{'orfas': None}` devolvem `(False, False)`, embora o acessor responda, respectivamente, quantidade `1` e `None`. O próprio comentário do ramo distingue ausência de batida de órfã sem papel atribuído. (`app/ponto/services/cartorio.py:831`; `app/ponto/services/cartorio.py:835`; `app/ponto/services/cartorio.py:837`; `app/escala/services/ata.py:25`; `app/escala/services/ata.py:32`)

   Outro leitor fora do diff ainda deriva quantidade usando `len(dia.get('orfas') or [])`: o cálculo de `n_bat` no cartório. Hoje ele recebe a grade construída naquele julgamento, mas conserva uma segunda regra de contagem que transforma `None` em zero. (`app/ponto/services/cartorio.py:1414`; `app/ponto/services/cartorio.py:1422`; `app/ponto/services/cartorio.py:1424`)

   No archive há ainda leitura crua de `ata.orfas` por truthiness no script de snapshot. (`bin/snapshot_o118_col221.py:32`)

6. **BLOQUEANTE — ESMERIL-SEMANTICA: dia do turno versus dia civil no vigia.** As órfãs encontradas pelo match são guardadas no dia do turno; apenas as sobras ainda não consumidas passam pelo agrupamento civil. O vigia continua buscando a órfã exclusivamente na célula do dia civil da borda. (`app/escala/utils.py:1706`; `app/escala/utils.py:1763`; `app/escala/utils.py:1816`; `app/escala/utils.py:1825`; `app/escala/utils.py:1839`; `app/ponto/services/vigia_pareador_celula.py:114`)

   Contraexemplo: ata D contém a órfã de D+1 às 06:29 e a última lâmpada apagada; ata D+1 contém `orfas=[]`; o turno fechado de D termina naquele instante. O vigia consulta a lista vazia de D+1, perde o caso e pode devolver `cego=0`, pois seu universo continua preenchido. Ocorrência dessa configuração em produção: **[nao verificado]**. (`app/ponto/services/vigia_pareador_celula.py:102`; `app/ponto/services/vigia_pareador_celula.py:114`; `app/ponto/services/vigia_pareador_celula.py:118`; `app/ponto/services/vigia_pareador_celula.py:120`)

   O PDF tem risco equivalente: marca pela ata da linha apresentada, enquanto uma batida não reclamada por turno pode aparecer no dia civil. A integração desse contraexemplo no PDF permanece **[nao verificado]**; o calendário passou a procurar na união dos instantes. (`app/ponto/turnos.py:2962`; `app/ponto/services/espelho.py:575`; `app/relatorios/pdf_espelho.py:644`; `app/colaboradores/services/calendario.py:623`)

7. **BLOQUEANTE — `nao_medidos` desaparece em consumidores operacionais.** O esmeril publica `nao_medidos` na frota, mas a API da frota seleciona somente quatro campos e o descarta; o command também imprime contadores sem publicar a parte não medida. Assim, esses leitores não permitem distinguir “nenhuma ocorrência medida” de cobertura incompleta de A5. (`app/ponto/services/esmeril_espelho.py:235`; `app/ponto/services/esmeril_espelho.py:245`; `app/api/views_mensageria.py:1790`; `app/ponto/management/commands/esmeril_espelho.py:21`; `app/ponto/management/commands/esmeril_espelho.py:23`)

   A apuração de furos igualmente acrescenta `nao_medidos` ao retorno, mas seu command continua imprimindo apenas empresa, janela, colaboradores e dias. (`app/escala/services/furos_diarios.py:150`; `app/escala/management/commands/apurar_furos_diarios.py:13`; `app/escala/management/commands/apurar_furos_diarios.py:14`)

8. **Censo — escritores, leitores e contratos localizados.** A contagem e a identidade temporal são contratos diferentes: lista HH:MM conserva quantidade conhecida, mas não produz instantes; lista ausente/inválida/incompleta produz `None` na leitura temporal. A quantidade também fica `None` quando inválida ou inconsistente. (`app/escala/services/ata.py:20`; `app/escala/services/ata.py:27`; `app/escala/services/ata.py:29`; `app/escala/services/ata.py:119`; `app/escala/services/ata.py:130`)

   | Campo/API | Escritores e leitores de produção/bin localizados |
   |---|---|
   | JSON `ata.orfas` | Leitor no acessor: `app/escala/services/ata.py:17`; escritor: `app/escala/services/ata.py:151`; leitor cru externo: `bin/snapshot_o118_col221.py:32`. |
   | JSON `ata.n_orfas` | Escritor: `app/escala/services/ata.py:152`; leitores: `app/escala/services/ata.py:25`, `app/escala/services/ata.py:125`, `app/ponto/services/cartorio.py:837`. |
   | `gravar_orfas` | Chamado pela lavra e pela ata sintética da grade: `app/ponto/services/cartorio.py:769`; `app/escala/utils.py:1903`. |
   | Retorno `contar_orfas` | `app/folha/causas.py:44`; `app/escala/services/furos_diarios.py:24`; `app/escala/services/leitor_celula.py:191`; `app/ponto/status_dia.py:188`; `app/ponto/management/commands/detectar_par_relampago.py:143`; `app/ponto/supra_juiz.py:380`; `app/ponto/services/bordas_realizado.py:50`. |
   | Retorno `contagem_orfas_da_ata` | `app/chamados/juizes.py:511`; `app/escala/services/leitor_celula.py:421`. |
   | Retorno `orfas_da_ata` | `app/chamados/services/materializacao.py:179`; `app/escala/services/leitor_celula.py:421`; `app/ponto/juiz_batida.py:162`; `app/ponto/management/commands/tripwire_celulas.py:235`; `app/ponto/services/esmeril_espelho.py:133`; `app/ponto/services/vigia_pareador_celula.py:77`; `app/ponto/services/cluster_espurio.py:30`. |
   | `tem_batida` dos fatos | Escrito em `app/ponto/status_dia.py:192`; transportado pelo cartório em `app/ponto/services/cartorio.py:933`, `app/ponto/services/cartorio.py:1294`, `app/ponto/services/cartorio.py:1855`; delegado pelo modelo em `app/escala/models.py:1827`; consumido pela porta em `app/ponto/portas/celula.py:963` e pela regra em `app/ponto/status_dia.py:139`. |
   | `AtaVigiada` | Estado escrito em `app/escala/services/ata.py:54`, `app/escala/services/ata.py:63`, `app/escala/services/ata.py:66`; fiscalização em `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:18` e `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:20`; injeções em `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:31` e `app/escala/tests/test_tipo_na_fonte_orfas_v2.py:117`, `app/escala/tests/test_tipo_na_fonte_orfas_v2.py:209`, `app/escala/tests/test_tipo_na_fonte_orfas_v2.py:240`, `app/escala/tests/test_tipo_na_fonte_orfas_v2.py:287`. |

   As projeções em memória também têm escritores: os dois builders, os overlays que zeram órfãs, o leitor da célula e o retorno do juiz de batida. Não são escritas adicionais no JSON persistido, mas pertencem ao contrato mudado. (`app/escala/utils.py:763`; `app/escala/utils.py:789`; `app/escala/utils.py:800`; `app/escala/utils.py:964`; `app/escala/utils.py:968`; `app/escala/utils.py:1009`; `app/escala/utils.py:1036`; `app/escala/utils.py:1045`; `app/escala/utils.py:1157`; `app/escala/utils.py:1222`; `app/escala/utils.py:1257`; `app/escala/utils.py:1262`; `app/escala/utils.py:1727`; `app/escala/utils.py:1738`; `app/escala/utils.py:1807`; `app/escala/utils.py:1816`; `app/escala/utils.py:1837`; `app/escala/utils.py:1839`; `app/escala/utils.py:2045`; `app/escala/services/leitor_celula.py:391`; `app/escala/services/leitor_celula.py:421`; `app/ponto/juiz_batida.py:165`)

   Seus leitores diretos localizados incluem adaptador de realizado, calendário, classificador de órfãs, propositor, cartório, simulador, supra, cobertura parcial e PDF. Os usos de truthiness que apenas perguntam se uma lista **já tipada** está vazia não são, por si, leitura do formato antigo; os contraexemplos são os achados 2, 3 e 5. (`app/escala/utils.py:1089`; `app/escala/utils.py:1114`; `app/colaboradores/services/calendario.py:380`; `app/escala/orfa_classe.py:54`; `app/ponto/propositor.py:36`; `app/ponto/propositor.py:81`; `app/ponto/services/cartorio.py:736`; `app/ponto/services/cartorio.py:1424`; `app/ponto/services/simular_dia.py:12`; `app/ponto/supra_juiz.py:516`; `app/ponto/turnos.py:2667`; `app/relatorios/pdf_espelho.py:204`)

   `nao_medidos` tem unidades distintas por resultado, que não devem ser somadas indiscriminadamente. (`app/escala/services/furos_diarios.py:53`; `app/ponto/services/esmeril_espelho.py:35`; `app/relatorios/pdf_espelho.py:218`)

   | Resultado | Unidade/escritor | Leitor/destino |
   |---|---|---|
   | Status | Indicador de quantidade de órfãs desconhecida naquele dia | `app/ponto/status_dia.py:195`; aceito, mas não usado diretamente pela decisão em `app/ponto/status_dia.py:114`. |
   | Furos | Datas não medidas por colaborador; agregado em dias-colaborador | `app/escala/services/furos_diarios.py:53`; `app/escala/services/furos_diarios.py:85`; `app/escala/services/furos_diarios.py:134`; preservação nas exclusões em `app/escala/services/furos_diarios.py:144` e `app/colaboradores/services/pos_vinculo.py:86`. |
   | Causa da folha | Dias úteis com quantidade desconhecida | `app/folha/causas.py:46`; persistido com a causa em `app/ponto/services/fechamento.py:1080`; retrato lê somente a palavra da causa em `app/folha/retrato_folha.py:1116`. |
   | Bordas | Dias passados sem turno cuja presença não foi medida | `app/ponto/services/bordas_realizado.py:85`; command publica todos os nomes em `app/ponto/management/commands/bordas_realizado.py:26`. |
   | Score geométrico | Dias da grade sem quantidade conhecida | `app/ponto/management/commands/detectar_par_relampago.py:145`; bloqueia melhora/perfeição em `app/ponto/management/commands/detectar_par_relampago.py:162` e `app/ponto/management/commands/detectar_par_relampago.py:275`. |
   | Tripwire | Células elegíveis sem identidade temporal de órfãs | `app/ponto/management/commands/tripwire_celulas.py:237`; publicado em `app/ponto/management/commands/tripwire_celulas.py:363`. |
   | Esmeril | Dias não medidos por assinatura A5 | `app/ponto/services/esmeril_espelho.py:135`; agregado/publicado em `app/ponto/services/esmeril_espelho.py:215` e `app/ponto/services/esmeril_espelho.py:245`; perdas nos leitores: achado 7. |
   | PDF | Indicador por dia e quantidade de períodos abertos não medidos | `app/relatorios/pdf_espelho.py:184`; `app/relatorios/pdf_espelho.py:218`; observação por dia em `app/relatorios/pdf_espelho.py:995`. |

   Há homônimos que não são a lista JSON: `FuroDiario.orfas` é inteiro, `contexto_json.orfas` recebe ISO, e as “órfãs” do reconciliador/diagrama são perguntas. (`app/escala/models.py:1883`; `app/ponto/portas/celula.py:1132`; `app/ponto/services/cluster_espurio.py:212`; `app/chamados/reconciliador.py:1340`; `app/core/management/commands/gerar_diagrama.py:783`)

   **Completude de todos os aliases, acessos dinâmicos e sítios de fixtures no archive inteiro: [nao verificado].** O inventário acima não deve ser convertido em declaração de zero global; o próprio instrumento declara cobertura restrita aos caminhos executados. (`app/escala/services/ata.py:47`)

9. **Comportamento dos quatro julgamentos — diferenças em relação ao diff-base fornecido.** A disponibilidade local do objeto Git `0a747193e` é **[nao verificado]**; a comparação abaixo usa os trechos anteriores presentes no anexo.

   | Julgamento | Entradas em que muda | Classe |
   |---|---|---|
   | `_celula_furada` | Sem lâmpada explicitamente apagada, quantidade ausente/inválida/inconsistente passa a devolver `None`; lista HH:MM sem `n_orfas` continua conhecida por comprimento, e quantidade positiva conhecida conserva a consulta à régua. | **(a)** mudanças quando a contagem resulta desconhecida. (`app/chamados/juizes.py:508`; `app/chamados/juizes.py:511`; `app/chamados/juizes.py:520`; `app/escala/services/ata.py:32`) |
   | `fatos_do_status` / `status_da_lavra` | Sem casada nem realizado positivo, quantidade desconhecida produz `tem_batida=None` e `sem_certidao`, antes dos ramos falta/ausência/folga/isento. Na API direta, `orfas=None,n_orfas=2` passa a provar presença. | **(a) e (b)** na API; no pipeline antigo a projeção por quantidade já podia preservar essa presença. (`app/ponto/status_dia.py:139`; `app/ponto/status_dia.py:188`; `app/ponto/status_dia.py:192`; `app/escala/services/leitor_celula.py:421`) |
   | `classificar_causa` | Quantidade positiva conhecida sem instantes passa a integrar `com_orfa`; desconhecimento pode trocar a causa por `indeterminado`. A causa conhecida da régua permanece prioritária quando há previsão. | **(a) e (b)** na API direta. (`app/folha/causas.py:44`; `app/folha/causas.py:64`; `app/folha/causas.py:68`; `app/folha/causas.py:75`; `app/folha/causas.py:78`) |
   | `classificar_dia` | `orfas=None,n_orfas>0` passa a provar fato; com chave presente e `None`, o fallback por batidas civis é suprimido. Isso também muda entrada de quantidade **conhecida zero**: `orfas=None,n_orfas=0`, previsto positivo, realizado zero e batidas civis positivas. | **(a) e (b)** na API direta. (`app/ponto/supra_juiz.py:379`; `app/ponto/supra_juiz.py:381`; `app/ponto/supra_juiz.py:518`; `app/ponto/supra_juiz.py:526`) |

   **Supra e produção:** não demonstrei ata corrente válida, gravada pelo cartório anterior com lista e quantidade consistentes, que troque os códigos nesses dois blocos: o novo leitor conserva sua quantidade e positividade. Existência de entrada real divergente em produção e identidade global da retratação: **[nao verificado]**. (`app/escala/services/ata.py:20`; `app/escala/services/leitor_celula.py:421`; `app/ponto/supra_juiz.py:381`; `app/ponto/supra_juiz.py:519`)

   O command do supra usa esse classificador para placar, exemplos e alarme; a retratação lê `cobranca_indevida` e protestos **lavrados**. Portanto uma diferença da chamada direta não prova, isoladamente, diferença na retratação. (`app/ponto/management/commands/supra_juiz.py:223`; `app/ponto/management/commands/supra_juiz.py:239`; `app/ponto/management/commands/supra_juiz.py:246`)

10. **Vocabulário de status — `sem_certidao` já é reconhecido.** Ele está no vocabulário do núcleo e nas escolhas do modelo; o app traduz para `neutro`, o calendário conta e oferece decisão, o espelho tem estilo próprio e o relatório de furos cobra apenas `irregular`. (`app/ponto/status_dia.py:45`; `app/escala/models.py:1813`; `app/ponto/status_dia.py:84`; `app/colaboradores/services/calendario.py:94`; `app/colaboradores/services/calendario.py:791`; `app/templates/ponto/espelho.html:122`; `app/relatorios/furos.py:128`)

   Os consumidores do status lavrado localizados são calendário, espelho, relatório de furos, API e painel. Um status realmente desconhecido pode atravessar D4 se possuir rótulo gravado; sem rótulo, o lookup falha. O adaptador do app também lança `KeyError` para palavra fora do vocabulário. (`app/colaboradores/services/calendario.py:606`; `app/ponto/services/espelho.py:170`; `app/relatorios/furos.py:123`; `app/api/views.py:1483`; `app/core/services/painel_op.py:336`; `app/ponto/status_dia.py:292`; `app/ponto/status_dia.py:294`; `app/ponto/status_dia.py:91`)

11. **Testes adaptados e demais classes de reprovação.** As três adaptações perguntadas preservam suas defesas principais: COBPARC mantém previsto/furado/órfãs e acrescenta o indicador; sobra mantém os contrastes e acrescenta identidade com data/segundos; propositor conserva os resultados de tipo incoerente, composto e coerente. A troca de `['?']` por `None` acompanha o contrato novo, com rejeição do legado ainda testada no acessor. (`app/escala/tests/test_cobparc_rotulo_furos_diarios.py:34`; `app/escala/tests/test_cobparc_rotulo_furos_diarios.py:46`; `app/escala/tests/test_sobra_na_borda_nao_vira_intervalo.py:324`; `app/escala/tests/test_sobra_na_borda_nao_vira_intervalo.py:361`; `app/escala/tests/test_sobra_na_borda_nao_vira_intervalo.py:456`; `app/ponto/tests/test_propositor.py:165`; `app/ponto/tests/test_propositor.py:170`; `app/ponto/tests/test_propositor.py:176`; `app/ponto/tests/test_propositor.py:198`; `app/escala/tests/test_tipo_na_fonte_orfas.py:25`)

   Os novos testes não são apenas mocks de saída: extraem funções reais, atravessam grade→lavra→leitor e executam mutantes em memória. Entretanto, isolam 

[truncado em 20000 de 22291 caracteres]

## 12/11 19:22 · sessao 515104c557cc · TIPO-NA-FONTE · verificador · origem=Code

**REPROVA — o ciclo foi corrigido, mas retorno misto continua sem lacuna; esse bloqueante permanece.** (`bin/leitores_de_chave.py:707`, `bin/leitores_de_chave.py:237`, `bin/leitores_de_chave.py:60`)

1. **Bloqueante: retorno misto foi documentado, não corrigido.** `retorna_json()` aceita a função ao encontrar **algum** retorno JSON; depois, uma chamada à função aceita recebe proveniência JSON sem avaliar o ramo escolhido. A própria v5 declara “retorno misto […] semeia: SEM lacuna”. (`bin/leitores_de_chave.py:237`, `bin/leitores_de_chave.py:243`, `bin/leitores_de_chave.py:405`, `bin/leitores_de_chave.py:60`)

   Contraexemplo executado em memória, com `f` declarada em RETURNS:

   ```python
   def f(c, b):
       if b:
           return c.ata
       return 42

   def sonda(c):
       return f(c, False)["k"]
   ```

   A ferramenta reconhece `sonda:k`, sem lacuna RETURNS ou de apelido. Há evidência de um ramo JSON, mas nenhuma prova de que essa chamada devolva JSON. O vocabulário correto da saída é **evento reconhecido**, não “acesso provado”; isso não satisfaz a correção específica JSON-ou-lacuna solicitada. (`bin/leitores_de_chave.py:237`, `bin/leitores_de_chave.py:405`, `bin/leitores_de_chave.py:7`, `bin/leitores_de_chave.py:29`)

2. **Contraexemplos adicionais: ciclos fechados; apelidos conservadores nos ensaios.** Resultados obtidos em memória chamando `_verifica_returns()` e `File.scan()`, sem gravar arquivos; os mecanismos responsáveis estão nas referências de cada linha. (`bin/leitores_de_chave.py:702`, `bin/leitores_de_chave.py:771`)

   | Contraexemplo | Resultado |
   |---|---|
   | `f → g → h → f`, sem base JSON | Três lacunas RETURNS; nenhuma chave da sonda reconhecida. (`bin/leitores_de_chave.py:707`, `bin/leitores_de_chave.py:725`) |
   | Recursão direta `return f()` | Uma lacuna RETURNS; nenhuma chave da sonda reconhecida. (`bin/leitores_de_chave.py:707`, `bin/leitores_de_chave.py:725`) |
   | Ciclo de três funções retornando por variáveis intermediárias | Três lacunas RETURNS; nenhuma chave da sonda reconhecida. (`bin/leitores_de_chave.py:336`, `bin/leitores_de_chave.py:725`) |
   | JSON em um ramo, `42` no outro | Sonda reconhecida, sem lacuna: bloqueante do achado 1. (`bin/leitores_de_chave.py:243`, `bin/leitores_de_chave.py:60`) |
   | JSON em um ramo, queda implícita para `None` | Mesmo resultado: sonda reconhecida sem lacuna. (`bin/leitores_de_chave.py:240`, `bin/leitores_de_chave.py:245`) |
   | `a=c.ata; b=a; return b` | Sonda reconhecida com cadeia sintática de proveniência. (`bin/leitores_de_chave.py:340`, `bin/leitores_de_chave.py:346`, `bin/leitores_de_chave.py:357`) |
   | `a=42; return a` | Uma lacuna RETURNS; sonda não reconhecida. (`bin/leitores_de_chave.py:340`, `bin/leitores_de_chave.py:421`, `bin/leitores_de_chave.py:725`) |
   | `apelido=f; return apelido(c)` | Função intermediária cai em lacuna RETURNS; não ganhou suporte novo para alias de função. (`bin/leitores_de_chave.py:320`, `bin/leitores_de_chave.py:410`, `bin/leitores_de_chave.py:725`) |

3. **O selo morde os defeitos comportamentais principais, mas não todas as alterações documentais.** A tabela separa mordida demonstrada por ensaio em memória de ausência de asserção constatada no shell. (`bin/tests/test_leitores_de_chave.sh:100`, `bin/tests/test_leitores_de_chave.sh:125`, `bin/tests/test_leitores_de_chave.sh:229`)

   | Verificação | Mutante que a derruba |
   |---|---|
   | `(j)`: preservar acessos anteriores | Escolher o último binding do arquivo, ignorando a posição do acesso: perde `k_antes` e `k_laco_vazio`. (`bin/leitores_de_chave.py:346`, `bin/tests/test_leitores_de_chave.sh:233`) |
   | `(j)`: limitar sementes de laço | Aplicar a semente ao escopo inteiro: aparecem os caminhos proibidos. (`bin/leitores_de_chave.py:362`, `bin/tests/test_leitores_de_chave.sh:234`) |
   | `(g)/(j)`: declarar reatribuição | Retirar a emissão de lacuna `apelido`. (`bin/leitores_de_chave.py:344`, `bin/tests/test_leitores_de_chave.sh:194`, `bin/tests/test_leitores_de_chave.sh:235`) |
   | `(k)`: `return 42` | Fazer `_returns()` devolver sempre o conjunto declarado: a sonda continua reconhecida indevidamente. (`bin/leitores_de_chave.py:108`, `bin/tests/test_leitores_de_chave.sh:108`) |
   | `(l)`: ciclo sem base | Restaurar maior ponto fixo descendente: o ciclo se sustenta, desaparecem as duas lacunas e a sonda permanece reconhecida. (`bin/leitores_de_chave.py:707`, `bin/tests/test_leitores_de_chave.sh:128`, `bin/tests/test_leitores_de_chave.sh:129`) |
   | Bloco JSON `dimensao` | Remover uma chave do bloco; o teste compara o esquema, não os valores. (`bin/tests/test_leitores_de_chave.sh:69`, `bin/tests/test_leitores_de_chave.sh:72`) |
   | Nova ressalva do stderr | **Nenhum:** apagar somente a frase “NAO acessos reais” preserva as asserções existentes. É lacuna de cobertura, não bloqueante independente. (`bin/leitores_de_chave.py:836`, `bin/tests/test_leitores_de_chave.sh:61`, `bin/tests/test_leitores_de_chave.sh:246`) |

   **(k) também passa com maior ponto fixo descendente**: esse algoritmo consegue eliminar `return 42`; quem distingue especificamente o ciclo é `(l)`. Isso foi ensaiado em memória; reversão exata para `a0f3b84f6`: **[nao verificado]**. (`bin/tests/test_leitores_de_chave.sh:101`, `bin/tests/test_leitores_de_chave.sh:121`, `bin/tests/test_leitores_de_chave.sh:128`)

   O comentário “só a proveniência RETURNS liga x ao JSON” é excessivo: existe resolução local alternativa. O controle `K0` impede que isso torne `(k)` vazio; `(l)` isolado não tem controle equivalente de ausência prévia da lacuna. (`bin/tests/test_leitores_de_chave.sh:96`, `bin/leitores_de_chave.py:409`, `bin/tests/test_leitores_de_chave.sh:100`, `bin/tests/test_leitores_de_chave.sh:127`)

4. **Não encontrei promessa de completude nem catraca alimentada pela ferramenta nos textos abertos.** Docstring admite falsos positivos e construções sem lacuna; stderr rejeita completude; a linha final do selo explicita eventos reconhecidos, não acessos reais. (`bin/leitores_de_chave.py:5`, `bin/leitores_de_chave.py:51`, `bin/tests/test_leitores_de_chave.sh:273`)

   `N_REAL` e `N_DNA` vêm da ferramenta, mas recebem apenas validação de formato, rc e positividade; não há teto ou comparação histórica de catraca. (`bin/tests/test_leitores_de_chave.sh:57`, `bin/tests/test_leitores_de_chave.sh:60`, `bin/tests/test_leitores_de_chave.sh:253`, `bin/tests/test_leitores_de_chave.sh:256`)

5. **Não encontrei fallback calado após falha da análise na CLI.** Raiz ausente, parse inválido e declaração sem sítio retornam rc 2 antes da emissão numérica. O fallback interno de `_returns()` para declarados existe, mas o caminho normal inicializa `RET_EF` vazio antes da análise; não é recuperação silenciosa de erro nesse caminho. (`bin/leitores_de_chave.py:810`, `bin/leitores_de_chave.py:818`, `bin/leitores_de_chave.py:823`, `bin/leitores_de_chave.py:108`, `bin/leitores_de_chave.py:707`, `bin/leitores_de_chave.py:766`)

6. **Leitores e classes anteriores.** O consumidor direto encontrado na cópia é o selo, que lê stdout, stderr, rc e JSON; a régua executa esse selo e consome seu rc. Não encontrei nesse circuito rederivação paralela nem interpretação da dimensão como acessos reais. (`bin/tests/test_leitores_de_chave.sh:39`, `bin/tests/test_leitores_de_chave.sh:43`, `bin/regua.sh:138`, `bin/regua.sh:140`, `bin/tests/test_leitores_de_chave.sh:273`)

   `disjuntor.py:1565` valida referências textuais de CENSO, sem consumir o JSON ou a contagem da ferramenta. “Cota inferior” está delimitada como eventos reconhecidos; `ESCRITOR` inclui explicitamente montagem de cópia, portanto não prova persistência. (`bin/disjuntor.py:1565`, `bin/leitores_de_chave.py:7`, `bin/leitores_de_chave.py:61`)

   Mensagens de commit, delta exato contra v4, leitores externos/briefs e execução integral do selo: **[nao verificado]**. O selo integral cria e remove temporários, incompatível com a restrição desta verificação. (`bin/tests/test_leitores_de_chave.sh:33`, `bin/tests/test_leitores_de_chave.sh:35`)

## 12/11 19:22 · sessao 5e4f295d8fcb · - · - · origem=Ronald (tmux)

Achados confirmados:

1. **Leitor cru fora do acessor**: `bin/snapshot_o118_col221.py:32` lê `CelulaDia.ata['orfas']` por `.get()` e truthiness. A catraca cobre só objetos `AtaVigiada` injetados nos testes, não esse script (`app/escala/services/ata.py:46`, `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:25`).

2. **Quantidade com duas interpretações / None vira ausência**: `insumos_da_ata` continua lendo `n_orfas` cru com `or 0` (`app/ponto/services/cartorio.py:837`). Execução isolada da função real:
   - `{'orfas':['08:00'],'lampadas':[]}`: acessor conta `1`; `insumos_da_ata` devolve `(False,False)`.
   - `{'orfas':None,'lampadas':[]}`: acessor conta `None`; `insumos_da_ata` devolve `(False,False)`.
   
   Isso contradiz a explicação do próprio ramo, que prevê `(None,None)` quando houve órfã sem papel atribuído (`app/ponto/services/cartorio.py:835`, `app/escala/services/ata.py:20`).

3. **Derivação paralela da quantidade**: cartório ainda calcula `n_bat` usando `len(dia.get('orfas') or [])`, fora do acessor (`app/ponto/services/cartorio.py:1422`, `app/ponto/services/cartorio.py:1424`). O caminho lê grade construída naquele julgamento, mas continua tendo regra própria de contagem e colapsando `None` em zero (`app/ponto/services/cartorio.py:1414`, `app/ponto/services/cartorio.py:1424`).

4. **ESMERIL-SEMANTICA: vigia procura dia civil, produtor também guarda pelo turno**: `_match_marcos` devolve órfãs do turno, guardadas em `orfas_por_data[d]` e transportadas para a linha D (`app/escala/utils.py:1706`, `app/escala/utils.py:1763`, `app/escala/utils.py:1816`). Somente restos não consumidos passam pelo agrupamento civil (`app/escala/utils.py:1819`, `app/escala/utils.py:1825`, `app/escala/utils.py:1839`). O vigia procura a órfã exclusivamente na célula do dia civil da borda (`app/ponto/services/vigia_pareador_celula.py:114`).

   Contraexemplo executado sem banco: ata D tem órfã ISO de D+1 06:29 e última lâmpada apagada; ata D+1 tem `orfas=[]`; turno fechado D termina exatamente nesse instante. Função real `censo` retorna `casos=[]`, universo completo com um turno posicionado, `cego=0`. O lookup que perde o fato e o falso verde estão em `app/ponto/services/vigia_pareador_celula.py:114` e `app/ponto/services/vigia_pareador_celula.py:120`. Ocorrência dessa entrada em produção [nao verificado].

5. **Possível mesma mistura no PDF**: batida não reclamada por turno é apresentada no dia civil (`app/ponto/turnos.py:2962`, `app/ponto/services/espelho.py:575`). PDF marca órfãs consultando só a lista da ata correspondente à linha (`app/relatorios/pdf_espelho.py:644`). Uma órfã noturna armazenada na ata D, mas apresentada em D+1, perde a marca se D+1 tiver lista vazia; integração dessa entrada [nao verificado]. O calendário passou a consultar a união dos instantes justamente para permitir dono diferente da linha (`app/colaboradores/services/calendario.py:622`, `app/colaboradores/services/calendario.py:628`).

6. **Suite ficou com expectativas antigas incompatíveis**: [nao verificado]
   - JSON novo comparado a `['03:10','22:50']` e `['08:00','17:00']`: `app/ponto/tests/test_bateria_lavratura.py:637`, `app/ponto/tests/test_bateria_lavratura.py:1057`.
   - Lista de datetimes comparada a `'13:41'`: `app/escala/tests/test_ata_x_turno_entrada.py:142`.
   - Lista de datetimes comparada a `['15:00']`: `app/escala/tests/test_saida_valida_cross_meia_noite.py:102`.
   - Arquivo tocado ainda compara lista de datetimes a `['14:03']`: `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:135`.
   
   Escritores novos entregam datetimes na grade e ISO no JSON (`app/escala/utils.py:622`, `app/escala/utils.py:663`, `app/escala/services/ata.py:150`). Execução Django [nao verificado].

Inventário de APIs em produção:

- `contar_orfas`: definição `app/escala/services/ata.py:20`; consumidores `app/folha/causas.py:44`, `app/escala/services/furos_diarios.py:24`, `app/escala/services/leitor_celula.py:191`, `app/ponto/status_dia.py:188`, `app/ponto/management/commands/detectar_par_relampago.py:143`, `app/ponto/supra_juiz.py:380`, `app/ponto/services/bordas_realizado.py:50`.
- `contagem_orfas_da_ata`: definição `app/escala/services/leitor_celula.py:188`; consumidores `app/chamados/juizes.py:511`, `app/escala/services/leitor_celula.py:421`.
- `orfas_da_ata`: definição `app/escala/services/leitor_celula.py:182`; consumidores `app/chamados/services/materializacao.py:179`, `app/escala/services/leitor_celula.py:421`, `app/ponto/juiz_batida.py:162`, `app/ponto/management/commands/tripwire_celulas.py:235`, `app/ponto/services/esmeril_espelho.py:133`, `app/ponto/services/vigia_pareador_celula.py:77`, `app/ponto/services/cluster_espurio.py:30`.
- JSON `orfas`: leitura no acessor `app/escala/services/ata.py:17`; escrita `app/escala/services/ata.py:151`; exceção externa `bin/snapshot_o118_col221.py:32`.
- JSON `n_orfas`: leituras `app/escala/services/ata.py:25`, `app/escala/services/ata.py:125`, `app/ponto/services/cartorio.py:837`; escrita `app/escala/services/ata.py:152`.
- Chamadores de `gravar_orfas`: `app/ponto/services/cartorio.py:769`, `app/escala/utils.py:1903`.
- `AtaVigiada`: escritores internos dos contadores `app/escala/services/ata.py:54`, `app/escala/services/ata.py:63`, `app/escala/services/ata.py:66`; leitores da fiscalização `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:18`, `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:20`; injeções adicionais `app/escala/tests/test_tipo_na_fonte_orfas_v2.py:117`, `app/escala/tests/test_tipo_na_fonte_orfas_v2.py:209`, `app/escala/tests/test_tipo_na_fonte_orfas_v2.py:240`, `app/escala/tests/test_tipo_na_fonte_orfas_v2.py:287`.
- Homônimos: `FuroDiario.orfas` é inteiro, não lista JSON (`app/escala/models.py:1883`, `app/ponto/portas/celula.py:1132`); `contexto_json.orfas` recebe ISO de órfãs (`app/ponto/services/cluster_espurio.py:212`); contagem de perguntas órfãs do reconciliador é outra unidade (`app/chamados/reconciliador.py:1340`, `app/chamados/reconciliador.py:1377`); perguntas sem juiz do diagrama são outro conceito (`app/core/management/commands/gerar_diagrama.py:783`).

Busca literal archive inteiro por `censo_orfas` não encontrou referências; chamadores dinâmicos [nao verificado]. Inventários detalhados de todos os sítios literais dos testes e projeções foram enviados nas mensagens anteriores; completude por análise de aliases/dinâmicos [nao verificado]. [nao verificado]

_4413 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
