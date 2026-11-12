# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 20:01** · publicado em **12/11/2026 20:03** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 20:01 · sessao ab67b3003d37 · TRANCA-VIVAS-08 · verificador · origem=Code

**REPROVA — bloqueante: o acumulador ainda pode perder competências sem contrariar os oráculos do arquivo.** A troca `tot[k] += linha[k]` → `tot[k] = linha[k]` sobrevive porque os casos medem uma única competência. (`app/chamados/services/competencia_trancada.py:355`; `app/chamados/tests/test_tranca_vivas_08.py:36`; `app/chamados/tests/test_tranca_vivas_08.py:244`)

1. **A resgatada percorre as entradas reais de `medir()`.** A fixture cria o chamado pela porta, fornece `data_turno`, grava `resgatado_em = fechado_em + 5 minutos` e escreve `q.chamado_destino`. O leitor calcula `competencia_do_chamado`, exige `resgatado_em > fechado_em` e consulta exatamente `q.chamado_destino_id`. O resgate é montado por `update`; o ato de resgatar pela porta não é exercitado nesse teste. (`app/chamados/tests/test_tranca_vivas_08.py:226`; `app/chamados/tests/test_tranca_vivas_08.py:229`; `app/chamados/tests/test_tranca_vivas_08.py:231`; `app/chamados/services/competencia_trancada.py:105`; `app/chamados/services/competencia_trancada.py:117`; `app/chamados/services/competencia_trancada.py:132`)

   A criação resolve a célula pelo dia informado, e a competência usa `dia_do_fato`. O mock não desloca essa competência por perguntas vivas: esse percurso consulta `disputa__chamado_id`, enquanto a fixture escreve somente `chamado_destino`; a disputa nasce sem chamado-pai. (`app/chamados/models.py:1067`; `app/chamados/models.py:1235`; `app/chamados/juizes.py:2359`; `app/chamados/juizes.py:2343`; `app/chamados/tests/test_c7_red_minimos.py:108`)

2. **O mock altera a vida, mas não torna a agregação vácua.** `dia_da_pergunta_trancado=False` remove o degrau que mataria a pergunta pela tranca. Não substitui a formação de `resgatados`, os carimbos ou a FK. O oráculo exige explicitamente `([q.pk], [], [])` nas três parcelas e união **1**, tanto no total quanto na linha. Portanto, prova a agregação **condicionada à vida simulada**; não prova que o resgate real produz essa vida. (`app/chamados/juizes.py:1405`; `app/chamados/services/competencia_trancada.py:117`; `app/chamados/services/competencia_trancada.py:132`; `app/chamados/tests/test_tranca_vivas_08.py:239`; `app/chamados/tests/test_tranca_vivas_08.py:241`; `app/chamados/tests/test_tranca_vivas_08.py:247`)

3. **A perda da resgatada está curada; permanecem sobreviventes de agregação.**

   | Local | Resultado contra os oráculos |
   |---|---|
   | União, linha 348 | Retirar resgatadas é mordido pelo novo MORDE; trocar união por concatenação continua mordido pelo caso de sobreposição. (`app/chamados/tests/test_tranca_vivas_08.py:249`; `app/chamados/tests/test_tranca_vivas_08.py:203`) |
   | Cardinalidade da união, linha 351 | O antigo mutante `len(_com_o_admin) - len(m['vivas_resgatadas'])` agora devolve **0** onde linha e total exigem **1**. (`app/chamados/services/competencia_trancada.py:351`; `app/chamados/tests/test_tranca_vivas_08.py:244`; `app/chamados/tests/test_tranca_vivas_08.py:247`) |
   | Parcelas por competência, linha 353 | `len(m[k])` → `min(1, len(m[k]))` sobrevive: parcelas verificadas são zero ou um. Com duas resgatadas, publicaria parcela **1**, embora a união fosse **2**. (`app/chamados/services/competencia_trancada.py:353`; `app/chamados/tests/test_tranca_vivas_08.py:186`; `app/chamados/tests/test_tranca_vivas_08.py:244`) |
   | Acumulador, linha 355 | `+=` → `=` sobrevive. Com duas competências contendo uma resgatada cada, o original totaliza **2** e o mutante conserva apenas **1**. (`app/chamados/services/competencia_trancada.py:355`; `app/chamados/tests/test_tranca_vivas_08.py:36`; `app/chamados/tests/test_tranca_vivas_08.py:246`) |

   Os dois sobreviventes foram conferidos em memória sobre o corpo real de `medir_frota`, com entradas sintéticas; preservam os resultados dos cenários do arquivo, inclusive os MORDE. Execução Django/ORM e sobrevivência na suíte completa: **[nao verificado]**. (`app/chamados/services/competencia_trancada.py:331`; `app/chamados/tests/test_tranca_vivas_08.py:206`; `app/chamados/tests/test_tranca_vivas_08.py:251`)

4. **A API morde troca entre os quatro campos.** Os valores **7, 3, 2, 4** são distintos, e cada chave é comparada com seu próprio valor; qualquer troca de pares falha. Conferi as seis trocas sobre o dicionário real da view. O residual também é certificado, com **5**. A projeção interna de `por_competencia` não recebe essa prova: o falso usa lista vazia. (`app/api/tests/test_api_mensageria_chamados_em_competencia_trancada.py:57`; `app/api/tests/test_api_mensageria_chamados_em_competencia_trancada.py:64`; `app/api/views_mensageria.py:1573`)

5. **Quem escreve e quem lê os valores publicados:** [nao verificado]

   | Campo | Escritor do valor | Leitor |
   |---|---|---|
   | `decidir_pelo_dp` | `medir()` escreve IDs; agregador escreve contagens. (`app/chamados/services/competencia_trancada.py:139`; `app/chamados/services/competencia_trancada.py:353`) | União e pauta do DP. (`app/chamados/services/competencia_trancada.py:348`; `app/chamados/services/competencia_trancada.py:246`) |
   | `vivas_resgatadas` | `medir()` escreve IDs; agregador conta. (`app/chamados/services/competencia_trancada.py:149`; `app/chamados/services/competencia_trancada.py:353`) | União. (`app/chamados/services/competencia_trancada.py:348`) |
   | `vivas_tocadas_pelo_admin` | `medir()` escreve IDs; agregador conta. (`app/chamados/services/competencia_trancada.py:158`; `app/chamados/services/competencia_trancada.py:353`) | União. (`app/chamados/services/competencia_trancada.py:348`) |
   | `ficam_com_o_admin` | Cardinalidade por competência e acumulação total. (`app/chamados/services/competencia_trancada.py:351`; `app/chamados/services/competencia_trancada.py:355`) | CLI e API. (`app/chamados/management/commands/chamados_em_competencia_trancada.py:22`; `app/api/views_mensageria.py:1573`) |
   | `vivas_apesar_da_tranca` | Classificação residual, cardinalidade e total. (`app/chamados/services/competencia_trancada.py:158`; `app/chamados/services/competencia_trancada.py:352`; `app/chamados/services/competencia_trancada.py:355`) | CLI e API. (`app/chamados/management/commands/chamados_em_competencia_trancada.py:27`; `app/api/views_mensageria.py:1577`) |

   As três parcelas também são lidas pela CLI e pela API. O cliente da mensageria recebe o JSON inteiro; a ferramenta acrescenta `cliente` e repassa o payload. (`app/chamados/management/commands/chamados_em_competencia_trancada.py:24`; `app/api/views_mensageria.py:1574`; `mensageria/nucleo/core_client.py:241`; `mensageria/nucleo/ferramentas.py:2038`)

Para fechar os sobreviventes demonstrados, faltam oráculos com **duas competências positivas** e **mais de um ID numa parcela**. Os atuais conferem uma competência e parcelas zero/um. (`app/chamados/tests/test_tranca_vivas_08.py:36`; `app/chamados/tests/test_tranca_vivas_08.py:186`; `app/chamados/tests/test_tranca_vivas_08.py:244`)

## 12/11 20:01 · sessao c24cf91fbb4c · BUG-CADASTRO-NAO-ALCANCA-O-FUTURO · construtor · origem=Code

C5e escrita. O [relato completo](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-193752/app/docs/RELATO-BUG-CADASTRO-C5E.txt:6) contém funções alteradas, chamadores, origem/leitores de `hoje` e comandos.

- **164 testes sem banco passaram**; RED da meia-noite falhou contra o corpo da base. [Evidências](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-193752/app/docs/RELATO-BUG-CADASTRO-C5E.txt:107). [nao verificado]
- **38 mutantes morderam por asserção**, incluindo leitura nova e `desfazer_apaga_admin`. [Comandos e resultados](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-193752/app/docs/RELATO-BUG-CADASTRO-C5E.txt:162).
- Catraca final: **31**, abaixo do teto herdado 36; sítios remanescentes enumerados. [Censo](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-193752/app/docs/RELATO-BUG-CADASTRO-C5E.txt:61).

Suíte ORM: **[nao verificado]**, sem Django disponível e com acesso a banco vetado. Sem commit, pela proibição inicial. Certificação permanece com Code/suite e opus final. [nao verificado]

## 12/11 20:00 · sessao 1286233fe78e · - · - · origem=Ronald (tmux)

Há sobreviventes concretos nos oráculos do arquivo:

| Local | Mutante | Por que passa | Contracaso |
|---|---|---|---|
| `app/chamados/services/competencia_trancada.py:355` | `tot[k] += linha[k]` → `tot[k] = linha[k]` | A fixture cria uma competência; os testes leem a linha `[0]` e o respectivo total. (`app/chamados/tests/test_tranca_vivas_08.py:36`, `:186`, `:188`, `:244`, `:246`) | Duas competências com uma resgatada cada: original **2**, mutante **1**. |
| `app/chamados/services/competencia_trancada.py:353` | `len(m[k])` → `min(1, len(m[k]))` | As parcelas verificadas têm cardinalidade zero ou um; o MORDE da soma altera somente a união. (`app/chamados/tests/test_tranca_vivas_08.py:186`, `:206`, `:244`) | Uma competência com duas resgatadas: união **2** nos dois; parcela original **2**, mutante **1**. |

Confirmei executando em memória o corpo real de `medir_frota`, extraído por AST, substituindo apenas `PeriodoFechado` e `medir`: ambos os mutantes preservaram os dicionários completos nos regimes residual, sobreposição, resgatada exclusiva, MORDE soma e MORDE sem resgatadas. A origem da função é `app/chamados/services/competencia_trancada.py:331`; execução Django/ORM e sobrevivência na suíte completa: **[nao verificado]**.

Na linha da cardinalidade da união, teto **1** morre porque o MORDE soma exige **2**; teto **2** sobreviveria por leitura aos valores examinados, mas é um exemplo menos natural que a perda do acumulador. (`app/chamados/services/competencia_trancada.py:351`; `app/chamados/tests/test_tranca_vivas_08.py:211`)

## 12/11 19:59 · sessao b87e237b6753 · - · - · origem=Ronald (tmux)

A fixture percorre as entradas reais que `medir()` lê: cria o chamado por `ChamadoColaborador.criar`, grava `resgatado_em` cinco minutos depois de `fechado_em` e aponta `q.chamado_destino` para esse chamado. `medir()` calcula a competência do chamado, exige a desigualdade estrita entre os carimbos e reconhece a pergunta pela FK de destino. (`app/chamados/tests/test_tranca_vivas_08.py:226`; `app/chamados/tests/test_tranca_vivas_08.py:229`; `app/chamados/tests/test_tranca_vivas_08.py:231`; `app/chamados/services/competencia_trancada.py:105`; `app/chamados/services/competencia_trancada.py:117`; `app/chamados/services/competencia_trancada.py:132`)

O chamado ganha a célula de `data_turno` na porta de criação; o juiz usa o dia dessa célula e a janela da empresa. A base criou essa célula para `2026-07-10`, dentro da competência testada. (`app/chamados/models.py:1067`; `app/chamados/models.py:1235`; `app/chamados/models.py:1241`; `app/chamados/juizes.py:98`; `app/chamados/juizes.py:2359`; `app/chamados/juizes.py:2372`; `app/chamados/tests/test_tranca_vivas_08.py:32`; `app/chamados/tests/test_tranca_vivas_08.py:38`)

O mock altera a leitura de vida: remove a morte pelo degrau da tranca em `_pergunta_viva_calc`; por isso o teste prova a agregação **condicionada a uma pergunta viva**, sem provar que o ato real de resgate produz essa vida. Contudo, não fabrica `resgatados`, não troca a comparação dos carimbos e não substitui a FK lida por `medir()`. O oráculo exige expressamente o PK apenas em `vivas_resgatadas`, seguido de total e linha iguais a um; portanto a união não fica vazia nem vacuamente verde. (`app/chamados/tests/test_tranca_vivas_08.py:239`; `app/chamados/juizes.py:1405`; `app/chamados/services/competencia_trancada.py:117`; `app/chamados/services/competencia_trancada.py:132`; `app/chamados/tests/test_tranca_vivas_08.py:241`; `app/chamados/tests/test_tranca_vivas_08.py:244`; `app/chamados/tests/test_tranca_vivas_08.py:247`)

O mock pode alcançar também `pode_fechar()` através de `fio_vivo`, mudando a lista de disputas encerráveis; isso não substitui os campos da união, cujas listas já foram classificadas antes desse laço. (`app/chamados/models.py:1849`; `app/chamados/models.py:1854`; `app/chamados/services/competencia_trancada.py:144`; `app/chamados/services/competencia_trancada.py:149`; `app/chamados/services/competencia_trancada.py:174`)

A competência deste chamado não ganha um atalho pelo mock: a pesquisa de perguntas vivas de `competencia_do_chamado()` filtra `disputa__chamado_id`, enquanto a fixture só escreve `chamado_destino`; a disputa da base é criada sem chamado-pai. (`app/chamados/juizes.py:2343`; `app/chamados/juizes.py:2365`; `app/chamados/tests/test_tranca_vivas_08.py:231`; `app/chamados/tests/test_c7_red_minimos.py:106`)

Execução ORM, resultado da suíte e efeito em produção: **[nao verificado]**. [nao verificado]

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

_4417 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
