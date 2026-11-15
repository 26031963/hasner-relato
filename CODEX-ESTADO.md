# CODEX-ESTADO

> Ultima resposta processada: **15/11/2026 17:24** · publicado em **15/11/2026 17:24** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 15/11 17:24 · sessao 30c7b5361d1d · TRIAGEM-DA-TRAVA · verificador · origem=Code

**AVAL-DEVOLVIDO — ITEM B.** O item A permanece independente e registrado como vigente. [app/docs/RELATO.md:9208](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/app/docs/RELATO.md:9208)

1. **NAO PROCEDE — as premissas não conferem integralmente.**

   `_GUARDA_MOTIVO` está exatamente em **:236**, mas essa linha declara a regex; a conferência ocorre em **:255**, com recusa rc 2 em **:262**. Hoje verifica a forma do motivo, sem consultar aprovação ou hash. `--dir` existe e escolhe o diretório montado para executar os testes; não certifica sua identidade. [bin/suite.sh:236](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/suite.sh:236), [bin/suite.sh:255](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/suite.sh:255), [bin/suite.sh:136](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/suite.sh:136), [bin/suite.sh:284](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/suite.sh:284)

   O selo existe, contém recusas comportamentais e exige que o mutante `aceita_sem_motivo` seja detectado. Entretanto, seu positivo atual aceita pre-opus sem fornecer prova de aprovação. Execução do selo nesta revisão: **[nao verificado]**. [bin/tests/test_suite_motivo.sh:46](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/tests/test_suite_motivo.sh:46), [bin/tests/test_suite_motivo.sh:61](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/tests/test_suite_motivo.sh:61), [bin/tests/test_suite_motivo.sh:156](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/tests/test_suite_motivo.sh:156)

   **A premissa dos três chamadores está errada:** `volta_intermediaria.sh` chama `suite.sh --dir … --volta …`, atravessando a guarda. Somente `regua.sh` e `pre-push.sh`, entre esses três, executam pela trava própria fora dela. [bin/volta_intermediaria.sh:32](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/volta_intermediaria.sh:32), [bin/regua.sh:165](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/regua.sh:165), [bin/pre-push.sh:142](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/pre-push.sh:142)

   **Já existe escritor de lastro por hash:** `<resposta>.id` registra obra, papel, hash, PID, fim e SHA da resposta. Pode ser lido sem escritor novo, mas comprova identidade e sucesso da chamada, não “zero achados procedentes”. `.rc` é escrito somente para construtor; a triagem existente distingue `procedente|descartado`, sem certificado específico de aprovação limpa. Existência de artefato concreto aprovado em produção: **[nao verificado]**. [bin/codex_pergunta.sh:464](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/codex_pergunta.sh:464), [bin/codex_pergunta.sh:274](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/codex_pergunta.sh:274), [bin/disjuntor.py:869](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/disjuntor.py:869)

2. **PROCEDE — há colisão com uma saída vigente do CODEX.**

   O item 5(d) manda subir ao opus **após mais de três voltas Codex, sem esperar**; o item 5(c) exige suite inteira verde antes desse opus. A guarda literal que exige aprovação prévia para liberar a suite não contempla essa saída: pode impedir o requisito necessário ao encaminhamento obrigatório. [CLAUDE.md:377](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/CLAUDE.md:377)

   Também é preciso preservar a triagem: achados descartados com motivo seguem anexados ao opus, que decide. Exigir uma resposta textual de “APROVA” do Codex seria mais restritivo que essa regra. [CLAUDE.md:375](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/CLAUDE.md:375)

   Motivo obrigatório e chamadas próprias da régua/pre-push são compatíveis com a seção 3 e com o contrato registrado de TRAVA-MENOS-FILA-COM-SELO. O literal original desse aval em `CORTES.md`: **[nao verificado]**; o conteúdo verificável está nas referências seguintes. [CLAUDE.md:101](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/CLAUDE.md:101), [app/docs/RELATO.md:9059](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/app/docs/RELATO.md:9059)

   Contrato de cinco linhas antes da construção confere. “UMA vez” conserva a ressalva vigente: **mudou a forma do contrato, exige nova chamada**. A consolidação CODEX da 7b prevalece sobre blocos antigos da PISTA. [app/docs/PISTA.md:170](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/app/docs/PISTA.md:170), [app/docs/PISTA.md:10](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/app/docs/PISTA.md:10)

3. **PROCEDE — há condições que não fecham no literal.**

   **Delta:** é permitido nas voltas intermediárias. Uma revisão do delta pode produzir lastro para o **novo commit**, passado ao wrapper; a prova do hash anterior não passa automaticamente a valer para ele. Portanto, delta e identidade por hash são compatíveis, mas a herança de aprovação entre hashes não está demonstrada. [CLAUDE.md:366](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/CLAUDE.md:366), [bin/codex_pergunta.sh:79](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/codex_pergunta.sh:79), [bin/codex_pergunta.sh:467](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/codex_pergunta.sh:467)

   **Identidade da árvore:** o sidecar identifica um commit; `--dir` monta conteúdo de um diretório. O fluxo atual não confere que os bytes montados correspondem àquele commit, nem revalida isso depois da espera pela trava. “Hash de `--dir`” precisa fechar essa relação; igualdade efetiva durante a execução: **[nao verificado]**. [bin/suite.sh:284](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/suite.sh:284), [bin/suite.sh:317](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/suite.sh:317)

   **Substituto com rc 75:** a regra permite sonnet com motivo, mas o wrapper sai por limite antes de criar o `.id`. Protocolo existente que grave aprovação desse substituto vinculada ao hash: **[nao verificado]**. O rc 75 da trava significa outra coisa: a vez não chegou. [CLAUDE.md:360](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/CLAUDE.md:360), [bin/codex_pergunta.sh:451](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/codex_pergunta.sh:451), [bin/trava_teste.sh:18](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/trava_teste.sh:18)

   **Contadores:** falta fonte histórica completa demonstrada. A suite remove seu rc temporário; a trava sobrescreve dono/início por “livre”; a volta intermediária grava resultado e duração, sem estado simultâneo da segunda pista. A régua possui carimbo próprio com resultado e horário, mas não cobre todas as chamadas de `suite.sh`. O histórico geral continua registrado como obra separada aberta. Valores dos dois contadores nas 24 horas: **[nao verificado]**. [bin/suite.sh:367](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/suite.sh:367), [bin/trava_teste.sh:40](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/trava_teste.sh:40), [bin/volta_intermediaria.sh:45](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/volta_intermediaria.sh:45), [bin/regua.sh:196](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/regua.sh:196), [app/docs/BACKLOG.md:2581](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/app/docs/BACKLOG.md:2581)

   **Sem `--dir`:** `pre-opus <OBRA>` é uso canônico para a árvore viva, mas B só nomeia “hash de `--dir`”. Deixar passar abre esse caminho; exigir `--dir` muda o comando canônico e esbarra na recusa atual de passar a viva explicitamente. Se for literalmente apenas `"pre-opus"`, sem obra, já retorna rc 2. [CLAUDE.md:96](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/CLAUDE.md:96), [bin/suite.sh:289](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/suite.sh:289), [bin/suite.sh:298](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/suite.sh:298), [bin/tests/test_suite_motivo.sh:48](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/tests/test_suite_motivo.sh:48)

4. **NAO PROCEDE — não há ato fora do declarado demonstrado no escopo B.**

   O registro limita B a guarda, selo e contadores, em instrumento com pouso próprio; não encomenda alteração de exportada, dinheiro ou regra de negócio. Esses atos continuam sujeitos ao `!`, e config do Code permanece proibida. Respeito efetivo aos limites de config, trava e motivo `pouso` pela implementação futura: **[nao verificado]**. [app/docs/BACKLOG.md:1954](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/app/docs/BACKLOG.md:1954), [CLAUDE.md:265](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/CLAUDE.md:265), [CLAUDE.md:350](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/CLAUDE.md:350)

## 15/11 17:22 · sessao f0dfa0f0ad86 · - · - · origem=Ronald (tmux)

- **Há escritor existente de identidade por hash**, mas existência de aprovação concreta em produção é **[nao verificado]**. O wrapper resolve o argumento como commit completo (`bin/codex_pergunta.sh:79`), arquiva esse commit (`bin/codex_pergunta.sh:311`) e, se o processo retorna 0 e produz resposta nova, grava `<resposta>.id` com obra, papel, hash completo, pid, fim e SHA da resposta (`bin/codex_pergunta.sh:464`). Esse sidecar já pode ser lido sem novo escritor; não constitui sozinho aprovação sem achados: o wrapper só testa sucesso do processo e presença da resposta (`bin/codex_pergunta.sh:428`, `bin/codex_pergunta.sh:466`).

- **`.rc` não é prova atual do verificador.** O arquivo `<resposta>.rc` só é escrito para `PAPEL=construtor` (`bin/codex_pergunta.sh:274`). `.r` é convenção de nome do arquivo de resposta fornecido pelo chamador; o wrapper aceita `RESPOSTA="$3"` (`bin/codex_pergunta.sh:67`).

- **Existe outro escritor registrado, mas não de aprovação sem achados:** `disjuntor.py --triagem` aceita sidecar de verificador, confere SHA e grava a triagem (`bin/disjuntor.py:892`, `bin/disjuntor.py:910`). Seus vereditos são `procedente|descartado` (`bin/disjuntor.py:139`); “procedente” significa achado procedente, não resposta “0 procedentes”, conforme CODEX item 5(b) (`CLAUDE.md:375`).

- **rc75 não deixa aprovação substituta nesse protocolo.** O wrapper registra limite e sai 75 antes da criação do sidecar (`bin/codex_pergunta.sh:451`, `bin/codex_pergunta.sh:459`, `bin/codex_pergunta.sh:464`). A lei permite leitura sonnet com rc75/falha e motivo no RELATO (`CLAUDE.md:360`; `app/docs/PISTA.md:94`), mas escritor que transforme a decisão desse substituto em aprovação verificável por hash é **[nao verificado]**.

- **Delta não contradiz necessariamente hash igual.** A pergunta de volta intermediária pode conter só delta (`CLAUDE.md:366`); passando o novo commit ao wrapper, o sidecar identifica o novo hash (`bin/codex_pergunta.sh:79`, `bin/codex_pergunta.sh:467`). Reutilizar prova de hash anterior para árvore modificada não é sustentado por esse mecanismo (`bin/codex_pergunta.sh:467`).

- **“Hash de `--dir`” precisa de definição.** Hoje `--dir` é apenas caminho (`bin/suite.sh:136`), montado como diretório (`bin/suite.sh:284`). Não há nesse caminho conferência HEAD/limpeza entre diretório e commit; a porta `--montagem` monta assets/cache, sem esse vínculo (`bin/arvore_do_push.sh:28`). Cópia archive pode nem ter `.git`, pois nasce por `git archive` (`bin/codex_pergunta.sh:315`). Sem `--dir`, roda a árvore viva por default (`bin/suite.sh:23`, `bin/suite.sh:297`); o literal “hash de --dir” não define esse caso **[nao verificado]**.

- **Fonte histórica suficiente para os dois contadores não foi demonstrada.** A suite executa testes sem gravar evento persistente de seleção/hash/data (`bin/suite.sh:317`); o rc temporário é removido (`bin/suite.sh:338`, `bin/suite.sh:367`). A trava grava dono+pid e início, sobrescrevendo com “livre” na saída; não guarda histórico nem pista viva (`bin/trava_teste.sh:23`, `bin/trava_teste.sh:40`). A volta intermediária grava OUT.log/.fim e duração, mas não estado da segunda pista (`bin/volta_intermediaria.sh:32`, `bin/volta_intermediaria.sh:42`). O ESTADO lê atualmente chamadas/limites Codex (`bin/gerar_estado.py:267`; `bin/codex_limite_estado.py:49`), não esses eventos de suite.

- **Segunda pista é externa ao repo.** O contrato registrado diz “servidor do GLPI; nenhum arquivo do repo”, com estado consultado por `ssh pista estado` (`app/docs/BACKLOG.md:2039`). Há fotografias textuais de 10:22 e 11:3x (`app/docs/RELATO.md:161`, `app/docs/RELATO.md:88`); não provam estado simultâneo a cada volta nem fornecem histórico completo dos contadores. Valores efetivos nas 24h são **[nao verificado]**.

## 15/11 17:22 · sessao f1505d748fb8 · E4C-F1V3 · leitura · origem=Code

**D1+J1 apontam uma candidata a cura: C12/desligamento. Os três vermelhos de limbo de C6/C11 continuam vermelhos, pois suas próprias asserções exigem texto sem instante vivo. O verde final de C12 não foi verificado por execução.** (`chamados/tests/test_e4b_c12_orm.py:179`; `chamados/tests/test_e4b_c6_orm.py:82`; `chamados/tests/test_e4b_c11_orm.py:122`.)

Todos os caminhos abaixo são relativos a `app/`. Não achei `.git` nesta árvore; `git diff 151b0666a..d36ecbddd` falhou com “not a git repository”. Procurei também arquivos de diff e fotografias históricas. A fotografia encontrada identifica outro BASE, `97bbaa60`, e não substitui o main solicitado. Portanto, **a lista completa dos sítios alterados por c2..c9 e sua comparação literal com main ficam [nao verificado]**. (`chamados/tests/e4c_f1v5_base_juizes.txt:1`.)

**(a) Escritores e leitores encontrados na cópia.**

Os escritores abaixo permitem rastrear os valores consumidos pelos sítios seguintes; a presença e o comportamento desses escritores no main são [nao verificado].

| Escritor na cópia | Texto e instante escritos |
|---|---|
| `responder_pergunta_disputa_core` | Escreve `resposta_colab=resposta` e `respondida_em=_agora`; registra RESPOSTA com os mesmos texto e instante. (`chamados/services/disputa_emissao.py:1481`; `chamados/services/disputa_emissao.py:1525`.) |
| Escrita de resposta por texto | Escreve `resposta_colab=texto`, `respondida_em=instante` e a RESPOSTA correspondente. (`chamados/services/disputa_emissao.py:1673`; `chamados/services/disputa_emissao.py:1677`.) |
| Recuperação pela trilha | Preenche `respondida_em=instante`; registra RESPOSTA usando o texto já existente na projeção. (`chamados/services/disputa_emissao.py:1695`; `chamados/services/disputa_emissao.py:1698`.) |
| `PerguntaDisputa.carimbar` | Com `resposta` solicitado e texto preenchido, preenche o instante ausente; com `'validar'`, também preenche validação. (`chamados/models.py:3260`.) |
| `reabrir_para_correcao` | Registra a foto anterior e zera texto e instante da projeção. (`chamados/services/disputa_emissao.py:1609`; `chamados/services/disputa_emissao.py:1632`.) |
| Ressuscitação | Registra REABERTURA com texto/foto anteriores; depois zera `respondida_em` e validação, conservando o texto. (`chamados/models.py:1655`; `chamados/models.py:1660`.) |
| Conservação do legado | Converte projeção com instante sem linha correspondente em payload RESPOSTA, copiando texto e instante. (`chamados/models.py:4121`.) |
| `DecisaoPergunta.registrar` | Persiste os payloads recebidos por `bulk_create`; `ato_em` e `ato_texto` são campos próprios da decisão. (`chamados/models.py:4063`; `chamados/models.py:4065`; `chamados/models.py:4143`.) |

Na tabela seguinte, “texto” significa texto após `strip()`. “Não classifica” significa que o sítio transporta ou interpreta valores, sem decidir sozinho que a pergunta está respondida. A condição de cada sítio **no main** permanece [nao verificado], pela ausência do diff.

| Sítio leitor na cópia | Fonte e predicado efetivo |
|---|---|
| `atos_pergunta.leitura` | RESPOSTA fornece `ato_texto` e `ato_em`; RECUSA/REABERTURA limpam ambos; DEVOLUÇÃO limpa somente validação. Não exige texto para transportar o instante. (`chamados/services/atos_pergunta.py:46`; `chamados/services/atos_pergunta.py:52`; `chamados/services/atos_pergunta.py:56`.) |
| `_leitura_conservada`, objeto sem relação/PK | Transporta diretamente texto e instante projetados, recortando instante futuro. Não classifica. (`chamados/juizes.py:296`; `chamados/juizes.py:307`.) |
| `_leitura_conservada`, objeto persistido | Combina atos reais e conservação R/V; pode conservar texto legado **sem instante**, sem fabricar decisão. (`chamados/juizes.py:321`; `chamados/juizes.py:336`; `chamados/juizes.py:352`.) |
| `_leitura_da_pergunta` | Obtém os atos por carga, prefetch ou relação e entrega à conservação. Não acrescenta predicado de respondida. (`chamados/juizes.py:362`; `chamados/juizes.py:375`.) |
| `_slot_esperado`, `hora` | Usa texto dos atos pelo embrulho; não exige o par completo, embora impeça resposta posterior ao horizonte de fornecer slot. (`chamados/juizes.py:500`; `chamados/juizes.py:525`.) |
| `_slot_esperado`, `data_hora` | Interpreta diretamente `atos.resposta`; não exige instante de resposta. (`chamados/juizes.py:512`.) |
| `_resposta_concorda_com_a_luz` | Lê **texto** da leitura dos atos para comparar a hora. (`chamados/juizes.py:1511`.) |
| `projecao_temporal_da_pergunta` | Transporta instante e texto da leitura, além de validação/materialização/via. Não classifica. (`chamados/juizes.py:1539`; `chamados/juizes.py:1550`.) |
| `_pergunta_viva_calc`, degrau da resposta | Aplica **só texto**; o instante retornado em `respondida` não participa desse `if`. (`chamados/juizes.py:1554`; `chamados/juizes.py:1572`.) |
| `respondida_sem_texto` | Aplica **instante E ausência de texto**. É detector do estado ilegal, não o predicado positivo D1. (`chamados/juizes.py:1683`.) |
| `q_respondida_com_texto` | RESPOSTA corrente com texto, acrescida da conservação; esta inclui legado **com texto e sem instante**, quando não há ato terminal. Portanto, não implementa D1 estrito. (`chamados/juizes.py:1715`; `chamados/juizes.py:1737`; `chamados/juizes.py:1759`.) |
| `PerguntaDisputa.q_vivas` | Consome aquela face SQL como ramo da resposta; continua sendo prefiltro de vitalidade. (`chamados/models.py:3500`; `chamados/models.py:3515`.) |
| `perguntas_corrigiveis` | Aplica **instante OU texto**, mais a proteção de lastro/vitalidade. (`chamados/juizes.py:1921`.) |
| `perguntas_respondidas_a_validar` | Filtra pela face SQL anterior e pelo C6; ordena pelo instante da leitura. Assim, admite o legado sem instante conservado pela face SQL. (`chamados/juizes.py:1929`; `chamados/juizes.py:1932`.) |
| `declarou_hora` | Interpreta **só texto** dos atos, excluindo escapes e descartados. (`chamados/juizes.py:1958`.) |
| `delta_da_resposta` | Interpreta **só texto** dos atos para calcular delta. (`chamados/juizes.py:2105`.) |
| `classificar_resposta_validacao` | Em escolha, interpreta **só texto**; em hora, delega ao delta. (`chamados/juizes.py:2209`; `chamados/juizes.py:2216`.) |
| `motivo_implausivel_validacao` | Interpreta **só texto** dos atos para comparar com o marco. (`chamados/juizes.py:2232`.) |
| `validar_pergunta` | Seu gate inicial exige **só `respondida_em` projetado**. Não achei teste de texto nesse gate. (`chamados/services/validacao.py:418`.) |
| `_item_validar`, mensageria | Transporta **instante dos atos**, mas ainda transporta **texto da projeção**; aceita instante nulo. (`api/views_mensageria.py:564`; `api/views_mensageria.py:574`; `api/views_mensageria.py:576`.) |
| Cargas de leitura | Transportam `ato_em`/`ato_texto`, conservações e snapshots; não definem respondida. (`chamados/services/carga_pergunta_viva.py:80`; `chamados/services/carga_pergunta_viva.py:88`; `chamados/services/carga_pergunta_viva.py:258`; `chamados/services/carga_pergunta_viva.py:281`.) |
| Fio, app e relavra | Alimentam a leitura dos atos antes de consultar o juiz; não criam outro predicado positivo nesse carregamento. (`chamados/services/fio.py:255`; `chamados/services/pergunta_no_app.py:66`; `chamados/services/relavra_via.py:235`.) |

Há ainda leitores relevantes para a interpretação de D1: a partição do app usa **só instante projetado**, enquanto `sem_veredito_entre` usa **instante OU texto projetados**. Não posso afirmar que esses sítios pertençam ao diff c2..c9. (`chamados/services/pergunta_no_app.py:87`; `chamados/models.py:1952`; pertença ao diff [nao verificado].)

**(b) Veredito por teste dos anexos.** [nao verificado]

Os estados vermelhos/erros são os informados nos anexos. As setas abaixo são previsões por inspeção, com execução D1+J1 **[nao verificado]**. “?” significa que não há evidência para determinar o resultado posterior.

| Teste | c9 → D1+J1 | Motivo e asserção | [nao verificado]
|---|---|---|
| `test_v8_R11_um_instante_ate_o_app`, máquina 2026-07-25 | erro → ? | Fixture sem resposta; D1 não aponta mudança desse ramo. Não achei traceback no dado fornecido. Asserções: `chamados/tests/test_e3_portas_tarefa.py:634`; fixture: `chamados/tests/e3_v8_apoio.py:167`. |
| Mesmo teste, máquina 2027-01-21 | erro → ? | Mesmo limite de evidência e mesmas asserções. (`chamados/tests/test_e3_portas_tarefa.py:634`; `chamados/tests/test_e3_portas_tarefa.py:636`.) |
| `test_e1_v8…test_BUG113_resposta_nova_reabre_a_MESMA_ausencia_e_reabsorve` | vermelho → vermelho previsto | Herda o teste de ausência; a resposta nova escreve o par completo e `ATESTADO` não deriva slot. (`chamados/tests/test_e1_v8.py:118`; `chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:163`; `chamados/services/disputa_emissao.py:1481`; `chamados/services/materializacao.py:359`.) |
| `test_o261_e1_ausencia_rejeitada_devolve…test_BUG113_resposta_nova_reabre_a_MESMA_ausencia_e_reabsorve` | vermelho → vermelho previsto | Mesma cadeia; não identifiquei cura por D1/J1 nas verificações de reabertura e reabsorção. (`chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:165`; `chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:169`.) |
| `test_determinismo_juiz_antes_e_depois_do_ato_recusado` | vermelho → vermelho previsto | Fixture com par completo; D1 mantém sua condição verdadeira. Asserções: `chamados/tests/test_pvmuda_carimbo_em_voo.py:128`; fixture: `chamados/tests/test_pvmuda_carimbo_em_voo.py:64`. |
| `test_endpoint_do_app_filtra_por_q_vivas` | **vermelho → vermelho** | O teste exige exatamente um filtro de compreensão; a cópia possui também o filtro de carregamento por tranca. D1/J1 não o removem. (`chamados/tests/test_contract_pergunta_viva.py:82`; `chamados/services/pergunta_no_app.py:65`; `chamados/services/pergunta_no_app.py:67`.) |
| `test_idempotente_validar_2x_a_recusada_mesmo_estado_uma_trilha` | vermelho → vermelho previsto | Par completo; DEVOLUÇÃO preserva resposta e instante, alterando validação. (`chamados/tests/test_o261_recusa_fala.py:67`; `chamados/tests/test_o261_recusa_fala.py:158`; `chamados/services/atos_pergunta.py:56`.) |
| `test_irmas_que_gravaram_ficam_como_estao` | vermelho → vermelho previsto | Mesmo cenário com par completo; nenhuma cura específica identificada nas verificações das irmãs. (`chamados/tests/test_o261_recusa_fala.py:67`; `chamados/tests/test_o261_recusa_fala.py:124`; `chamados/tests/test_o261_recusa_fala.py:133`.) |
| `test_propriedades_sob_intercalacao` | vermelho → ? | A máquina de estados pode responder novamente escrevendo só projeção após recusa. Sem o traço reduzido do Hypothesis, não se pode assumir igualdade entre ato e projeção para J1. (`chamados/tests/test_pvmuda_carimbo_em_voo.py:237`; `chamados/tests/test_pvmuda_carimbo_em_voo.py:243`; asserções: `chamados/tests/test_pvmuda_carimbo_em_voo.py:276`; `chamados/tests/test_pvmuda_carimbo_em_voo.py:289`.) |
| `test_recusa_da_porta_do_ponto_na_porta_nova_nao_reabre_a_disputa_e_deixa_trilha` | vermelho → vermelho previsto | Fixture com par completo; materializador substituído por mock de recusa. D1/J1 não corrigem essa devolução. (`chamados/tests/test_fio_le_o_juiz.py:167`; `chamados/tests/test_fio_le_o_juiz.py:251`; `chamados/tests/test_fio_le_o_juiz.py:257`.) |
| C11 `test_RED_A6_beneficio_rejeitado_limbo` | **vermelho → vermelho** | `limbo=True` chega à preparação que exige texto sem instante vivo; D1 falha antes do ato. (`chamados/tests/test_e4b_c11_orm.py:122`; `chamados/tests/test_e4b_c6_orm.py:82`.) |
| `test_RED_alimentacao_e_no_app_contam_a_tranca_uma_vez` | vermelho → vermelho previsto | Fixture já tem texto e instante; a asserção cobra contagem de tranca, sem cura identificada por D1/J1. (`chamados/tests/test_n1_perguntas_do_app.py:29`; `chamados/tests/test_n1_perguntas_do_app.py:153`; `chamados/tests/test_n1_perguntas_do_app.py:157`.) |
| C12 `test_RED_desligamento` | **vermelho → verde candidato** | D1 permite ao S128 carimbar o limbo; a segunda cascata pode satisfazer `admin_decidiu`. (`chamados/services/disputa_emissao.py:1987`; `chamados/services/disputa_emissao.py:2007`; `chamados/tests/test_e4b_c12_orm.py:171`; `chamados/tests/test_e4b_c12_orm.py:179`.) |
| C11 `test_RED_limbo_misto_fecha_mortas_orfa_e_entrega_motivo_proprio` | **vermelho → vermelho** | Método herdado de C6; D1 contradiz a própria precondição. (`chamados/tests/test_e4b_c11_orm.py:27`; `chamados/tests/test_e4b_c6_orm.py:82`.) |
| C6 `test_RED_limbo_misto_fecha_mortas_orfa_e_entrega_motivo_proprio` | **vermelho → vermelho** | Mesmo conflito; também permanece incompatível a expectativa posterior de conservar o limbo sem carimbo. (`chamados/tests/test_e4b_c6_orm.py:82`; `chamados/tests/test_e4b_c6_orm.py:116`.) |
| `test_RED_recusa_por_paridade_devolve_ao_admin_com_o_motivo` | vermelho → vermelho previsto | Par completo; DEVOLUÇÃO não remove a resposta. D1/J1 não mostram cura para as verificações da recusa. (`chamados/tests/test_o261_recusa_fala.py:67`; `chamados/tests/test_o261_recusa_fala.py:95`; `chamados/services/atos_pergunta.py:56`.) |
| `test_RED_segunda_tentativa_e_o_estado_de_prod` | vermelho → vermelho previsto | Fixture completa; nenhuma mudança identificada no estado/trilha exigidos pela segunda tentativa. (`chamados/tests/test_pvmuda_carimbo_em_voo.py:64`; `chamados/tests/test_pvmuda_carimbo_em_voo.py:113`; `chamados/tests/test_pvmuda_carimbo_em_voo.py:118`.) |
| `test_RED_validar_recusada_por_paridade_nao_fecha_a_disputa` | vermelho → vermelho previsto | Mesmo par completo; D1 não muda sua classificação. (`chamados/tests/test_pvmuda_carimbo_em_voo.py:64`; `chamados/tests/test_pvmuda_carimbo_em_voo.py:87`; `chamados/tests/test_pvmuda_carimbo_em_voo.py:93`.) |

A candidata C12 depende desta cadeia concreta: o limbo tem texto sem instante; a primeira cascata ainda encontra a proteção `instante OU texto`; D1 deixa o limbo morrer pelo C6 e o S128 chamar `carimbar(..., resposta='validar')`; essa porta preenche resposta/validação; na segunda cascata, a disputa pode fechar por desligamento. Isso explica a possível cura da asserção final, sem certificar o teste inteiro. (`chamados/tests/test_e4b_c12_orm.py:58`; `chamados/models.py:1954`; `colaboradores/services/desligamento.py:165`; `chamados/services/disputa_emissao.py:2007`; `chamados/models.py:3260`; `chamados/tests/test_e4b_c12_orm.py:179`.)

| Teste de ponto/core | c9 → D1+J1 | Motivo e asserção | [nao verificado]
|---|---|---|
| `test_calendario` | vermelho → ? | Cobra medição SQL; fixture já tem o par completo. (`core/tests/test_selo_performance.py:322`; `core/tests/test_selo_performance.py:344`; `core/tests/test_selo_performance.py:406`.) |
| `test_chamados_modal` | vermelho → ? | Mesma limitação: não há nova contagem nem diagnóstico da falha. (`core/tests/test_selo_performance.py:344`; `core/tests/test_selo_performance.py:380`.) |
| `test_pendencias_disputa` | vermelho → ? | Mesma limitação de medição. (`core/tests/test_selo_performance.py:344`; `core/tests/test_selo_performance.py:357`.) |
| `test_MORDE_o_mmd_commitado_bate_com_o_codigo` | vermelho → ? | Exige igualdade entre geração e artefato; o anexo não mostra a divergência. (`core/tests/test_selo_diagrama_do_codigo.py:46`.) |
| `test_MORDE_pendente_curado_sai_da_lista` | **vermelho → vermelho** | A impressão registrada não existe literalmente na chamada atual, que acrescentou `agora=agora`; D1/J1 não alteram isso. (`core/juizes.py:878`; `colaboradores/services/drawer.py:121`; `core/tests/test_contract_juiz_tela.py:38`.) |
| `test_MORDE_uma_aresta_a_mais_no_mmd_derruba_o_selo` | vermelho → ? | Sem traceback, não se sabe se falhou a detecção da adulteração ou a restauração. (`core/tests/test_selo_diagrama_do_codigo.py:93`; `core/tests/test_selo_diagrama_do_codigo.py:98`.) |
| `test_registrar_batida_com_1_furo_vivo_que_ela_fecha` | vermelho → ? | Cobra fechamento, callbacks e consultas; o cenário não insere pergunta explicitamente. (`ponto/tests/test_selo_chokepoint_escrita.py:416`; `ponto/tests/test_selo_chokepoint_escrita.py:437`; `ponto/tests/test_selo_chokepoint_escrita.py:289`; `ponto/tests/test_selo_chokepoint_escrita.py:293`.) |
| `test_registrar_batida_com_3_furos_vivos_que_ela_fecha` | vermelho → ? | Mesmo cenário com três chamados; sem cura semântica demonstrada. (`ponto/tests/test_selo_chokepoint_escrita.py:416`; `ponto/tests/test_selo_chokepoint_escrita.py:442`; `ponto/tests/test_selo_chokepoint_escrita.py:293`.) |
| `test_registrar_batida_com_3_furos_vivos_que_ficam` | vermelho → ? | Cobra permanência dos vivos e custo da escrita; sem medição contrafactual. (`ponto/tests/test_selo_chokepoint_escrita.py:456`; `ponto/tests/test_selo_chokepoint_escrita.py:477`; `ponto/tests/test_selo_chokepoint_escrita.py:293`.) |
| `test_ruff_zero` | vermelho → ? | Cobra Ruff na árvore inteira; não achei os diagnósticos no dado fornecido. (`core/tests/test_contract_esmeril.py:23`; `core/tests/test_contract_esmeril.py:24`.) |

Para os demais testes C6/C11/C12, **não identifiquei mudança semântica causada por D1/J1**: as preparações comuns não têm resposta, e C12 acrescenta limbo especificamente no desligamento. O estado c9 dos testes ausentes dos anexos e seu resultado posterior continuam **[nao verificado]**; ausência da lista de falhas não prova verde. (`chamados/tests/test_e4b_c6_orm.py:71`; `chamados/tests/test_e4b_c11_orm.py:99`; `chamados/tests/test_e4b_c12_orm.py:136`.)

A tabela completa esses testes individualmente. “?” nas duas posições indica estado não fornecido, sem alteração prevista: [nao verificado]

| Teste — C6 e também sua execução herdada em C11 | c9 → D1+J1 | Asserção pertinente | [nao verificado]
|---|---|---|
| `test_RED_exportada_mista_fecha_mortas_orfa_e_entrega_pauta` | ? → ? | Carimbos e pauta: `chamados/tests/test_e4b_c6_orm.py:118`; `chamados/tests/test_e4b_c6_orm.py:128`. |
| `test_RED_fio_tambem_entrega_pauta_no_fechamento` | ? → ? | Mesmo corpo: `chamados/tests/test_e4b_c6_orm.py:113`; `chamados/tests/test_e4b_c6_orm.py:128`. |
| `test_RED_passivo_ja_encerrado_nao_ganha_pauta` | ? → ? | `chamados/tests/test_e4b_c6_orm.py:174`. |
| `test_RED_c7_conversa_sem_competencia` | ? → ? | `chamados/tests/test_e4b_c6_orm.py:190`. |
| `test_RED_c7_trava_antes_exists_mesma_ancora` | ? → ? | `chamados/tests/test_e4b_c6_orm.py:216`. |
| `test_RED_c7_texto_cabe_com_limbo_longo` | ? → ? | Mede texto de pauta, sem classificar resposta: `chamados/tests/test_e4b_c6_orm.py:238`; `chamados/tests/test_e4b_c6_orm.py:241`. |
| `test_RED_c8_todas_ancoras_antes_de_escrever` | ? → ? | Encaminha ao selo SQL: `chamados/tests/test_e4b_c6_orm.py:246`; asserção: `chamados/tests/test_e4b_c6_orm.py:301`. |
| `test_RED_c9_contador_dp_antes_das_ancoras` | ? → ? | Encaminha ao selo SQL: `chamados/tests/test_e4b_c6_orm.py:249`; asserção: `chamados/tests/test_e4b_c6_orm.py:301`. |
| `test_RED_c9_lote_vazio_nao_toca_contador` | ? → ? | `chamados/tests/test_e4b_c6_orm.py:264`; `chamados/tests/test_e4b_c6_orm.py:265`. |
| `test_RED_c7_recusa_rollback_nao_entrega` | ? → ? | `chamados/tests/test_e4b_c6_orm.py:321`; `chamados/tests/test_e4b_c6_orm.py:322`. |

A execução duplicada em C11 decorre da herança da classe C6. (`chamados/tests/test_e4b_c11_orm.py:27`.)

| Demais testes próprios de C11 | c9 → D1+J1 | Asserção/caminho pertinente | [nao verificado]
|---|---|---|
| `test_RED_A1_excecao` | ? → ? | Usa cenário sem limbo e verificações comuns: `chamados/tests/test_e4b_c11_orm.py:104`; `chamados/tests/test_e4b_c11_orm.py:101`; `chamados/tests/test_e4b_c6_orm.py:128`. |
| `test_RED_A2_agenda` | ? → ? | `chamados/tests/test_e4b_c11_orm.py:107`; chamada da agenda: `chamados/tests/test_e4b_c11_orm.py:94`. |
| `test_RED_A3_escala` | ? → ? | `chamados/tests/test_e4b_c11_orm.py:110`; verificações comuns: `chamados/tests/test_e4b_c6_orm.py:128`. |
| `test_RED_A4_autorizar_geofence` | ? → ? | `chamados/tests/test_e4b_c11_orm.py:113`; registro: `chamados/tests/test_e4b_c11_orm.py:92`. |
| `test_RED_A5_recusar_geofence` | ? → ? | `chamados/tests/test_e4b_c11_orm.py:116`; retratação: `c

[truncado em 20000 de 26345 caracteres]

## 15/11 17:22 · sessao a0d55dc0e9f7 · - · - · origem=Ronald (tmux)

Conclusões para consolidar, todas só da cópia:

- **Pergunta 2 — PROCEDE, com colisão/omissão específica:** a guarda literal exige aprovação para o hash, mas CODEX item 5 admite achado descartado com motivo e anexo ao opus, e manda subir ao opus após mais de três voltas sem esperar. O B não declara como esses caminhos continuam atravessando a guarda. Evidência: `CLAUDE.md:375-379`. A exigência de suite inteira verde antes do opus continua: `CLAUDE.md:377-378`, `app/docs/PISTA.md:113-114`.
- **Sem colisão demonstrada** com motivo obrigatório, porta canônica, prioridade de instrumento ou pouso próprio: `CLAUDE.md:95-105`, `CLAUDE.md:290-295`, `CLAUDE.md:325-328`. O próprio registro B conserva redefinição, teto 2 livre, revisão e contrato antes da construção: `app/docs/BACKLOG.md:1954`.
- **Contrato não é autorização de chamada única para sempre:** contrato de cinco linhas e revisão prévia conferem; se a forma mudar, exige nova chamada. `CLAUDE.md:394`, `app/docs/PISTA.md:170-176`. O literal “UMA vez antes” precisa ser lido junto dessa condição.
- **Delta:** voltas do meio podem verificar só o delta (`CLAUDE.md:366-368`); pre-verificador recebe diff do hash da cópia (`CLAUDE.md:372-373`). Isso permite revisar delta, mas não define mecanismo que transfira prova do hash antigo ao novo [nao verificado].
- **rc 75:** substituição da leitura por sonnet/falha do Codex é prevista com motivo (`CLAUDE.md:358-362`, `app/docs/PISTA.md:91-96`). Essa regra não define certificado persistente de aprovação por hash que a guarda poderia ler [nao verificado]. Não confundir com rc 75 da trava: ali é “a vez não chegou” (`CLAUDE.md:103-105`).
- **A permanece independente:** está registrado vigente, enquanto B está só registrado para revisão antes de construir: `app/docs/RELATO.md:9208`, `app/docs/BACKLOG.md:1954`.
- **TRAVA-MENOS-FILA-COM-SELO não consta nominalmente em `app/docs/CORTES.md` nesta cópia** [nao verificado quanto ao literal original]. Seu conteúdo verificável está em `CLAUDE.md:101-105`, `app/docs/RELATO.md:9057-9063` e `app/docs/PROMPTS.md:2087`. O contrato original declara regua/pre-push fora dessa porta (`app/docs/RELATO.md:9059`) e separa histórico/ESTADO por cruzarem trava+gerador (`app/docs/RELATO.md:9063`, `app/docs/BACKLOG.md:2581`).
- **Pergunta 4 — NAO PROCEDE no escopo declarado:** instrumento com pouso próprio é permitido (`CLAUDE.md:325-328`); config do Code é proibida (`CLAUDE.md:350-352`), e exportada/escala/vínculo/atalho pedem `!` (`CLAUDE.md:265-268`). Nenhum desses atos é necessário pelo escopo B registrado (`app/docs/BACKLOG.md:1954`). Implementação futura respeitando o escopo: [nao verificado].
- Para precedência normativa, a consolidação 7b é mais nova e prevalece sobre blocos antigos de PISTA (`app/docs/PISTA.md:10-12`).

## 15/11 17:22 · sessao b835b1f65803 · - · - · origem=Ronald (tmux)

- **Premissa da guarda: PROCEDE com correção.** `_GUARDA_MOTIVO` existe exatamente em `bin/suite.sh:236`, mas essa linha só declara a regex; a decisão é `bin/suite.sh:255` e a recusa rc 2 é `bin/suite.sh:262`. A regex admite `pre-opus <OBRA>` e `pouso <OBRA>`; não consulta prova nem hash. (`bin/suite.sh:236`, `bin/suite.sh:255`)

- **`--dir` existe e escolhe a árvore: PROCEDE.** Parse em `bin/suite.sh:136`, atribuição em `bin/suite.sh:137`; descoberta da seleção efetiva em `${DIR:-$RAIZ}/app` em `bin/suite.sh:238`; montagem da cópia em `bin/suite.sh:284`–`296`. A árvore viva passada explicitamente como `--dir` é recusada pela proteção de limpeza; sem `--dir`, usa `$RAIZ/app`. (`bin/suite.sh:289`, `bin/suite.sh:290`, `bin/suite.sh:298`)

- **Selo existente que morde: PROCEDE para a guarda atual de motivo.** O selo executa a suite real numa árvore falsa e detecta se chegou à trava, recusando também custo anterior à recusa; exige rc 2 sem motivo e aceita `pre-opus FIO-E0-V4-E3`. (`bin/tests/test_suite_motivo.sh:3`, `bin/tests/test_suite_motivo.sh:37`, `bin/tests/test_suite_motivo.sh:41`, `bin/tests/test_suite_motivo.sh:46`, `bin/tests/test_suite_motivo.sh:61`) Há mutantes explícitos, inclusive `aceita_sem_motivo`, e o juiz precisa reprovar cada mutante. (`bin/tests/test_suite_motivo.sh:156`, `bin/tests/test_suite_motivo.sh:162`, `bin/tests/test_suite_motivo.sh:164`) **Não prova hoje aprovação prévia por hash**: o positivo atual passa sem fornecer tal prova. (`bin/tests/test_suite_motivo.sh:61`)

- **“Os três chamam por caminho próprio, fora da guarda”: NAO PROCEDE.** `volta_intermediaria.sh` chama a própria suite com `--dir` e `--volta`, atravessando a guarda; se o helper produzir seleção inteira, a guarda atual também a alcança. (`bin/volta_intermediaria.sh:32`, `bin/suite.sh:164`, `bin/suite.sh:255`) `regua.sh` e `pre-push.sh` usam diretamente `trava_teste.sh` e `manage.py test`, fora dessa guarda. (`bin/regua.sh:165`, `bin/regua.sh:168`, `bin/pre-push.sh:142`, `bin/pre-push.sh:186`)

- **Lacuna literal sem `--dir`: PROCEDE.** `--motivo "pre-opus <OBRA>"` sem `--dir` é uso documentado para árvore viva, e hoje usa `$RAIZ/app`; o item B citado só especifica prova para “hash de `--dir`”, deixando esse caminho sem condição explícita de prova. (`bin/suite.sh:23`, `bin/suite.sh:129`, `bin/suite.sh:298`; literal do anexo, sem arquivo:linha disponível [nao verificado]) Se a pergunta quer dizer literalmente `--motivo "pre-opus"` **sem nome de obra**, já recusa rc 2, independentemente de `--dir`. (`bin/suite.sh:236`, `bin/suite.sh:255`, `bin/tests/test_suite_motivo.sh:48`)

- **Momento da prova:** colocar a guarda junto à atual significa verificar antes de env-file, montagem e espera pela trava; a trava só é pedida posteriormente. (`bin/suite.sh:255`, `bin/suite.sh:276`, `bin/suite.sh:284`, `bin/suite.sh:317`) A árvore viva permanece um bind mount de `$RAIZ/app`; uma prova anterior à espera, sozinha, não comprova que esse conteúdo permaneceu igual até a execução. (`bin/suite.sh:298`, `bin/suite.sh:317`, `bin/suite.sh:322`; ausência de mutação durante a espera [nao verificado])

- **Escopo material do código atual:** a porta executa testes em container com settings CI; `REGUA_DB`, quando existente, é apenas repassado. (`bin/suite.sh:305`, `bin/suite.sh:306`, `bin/suite.sh:319`, `bin/suite.sh:324`, `bin/suite.sh:325`) Nenhuma implementação do item B foi fornecida para provar ausência futura de alterações em exportada, dinheiro, config do Code, trava ou pouso [nao verificado]. O literal do anexo proíbe essas três últimas mudanças, mas o cumprimento futuro não é provado pelo código atual [nao verificado].

Inspeção estática apenas; não executei o selo, pois ele cria, copia e remove arquivos temporários. (`bin/tests/test_suite_motivo.sh:10`, `bin/tests/test_suite_motivo.sh:11`, `bin/tests/test_suite_motivo.sh:15`, `bin/tests/test_suite_motivo.sh:16`)

## 15/11 17:18 · sessao d2bf99cfb25b · - · - · origem=Ronald (tmux)

Leitura concluída sem execução; não há evidência suficiente para certificar nenhuma cura dos vermelhos informados. Previsões abaixo são [nao verificado], exceto a permanência da falha estática demonstrada pelo código.

| Teste | Previsão D1+J1 | Referências | [nao verificado]
|---|---|---|
| BUG113, nas duas classes | Sem cura identificada | Asserções `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:163`, `:165`, `:169`, `:170`; herança `app/chamados/tests/test_e1_v8.py:118`. Escritor grava o par em `app/chamados/services/disputa_emissao.py:1481`; escape não deriva slot em `app/chamados/services/materializacao.py:359`. |
| Recusa por paridade | Sem cura identificada | Asserções `app/chamados/tests/test_o261_recusa_fala.py:95`; fixture com par completo `:67`. Devolução preserva resposta em `app/chamados/services/validacao.py:213`, `:230`; leitura preserva texto/instante em `app/chamados/services/atos_pergunta.py:56`. |
| Irmãs que gravaram | Sem cura identificada | `app/chamados/tests/test_o261_recusa_fala.py:116`, `:124`, `:126`, `:133`; mesma fixture com par completo `:67`. |
| Idempotência da recusada | Sem cura identificada | `app/chamados/tests/test_o261_recusa_fala.py:158`, `:159`, `:161`, `:162`; mesmo fluxo de devolução acima. |
| Carimbo em voo: validar recusada | Sem cura identificada | Fixture completa `app/chamados/tests/test_pvmuda_carimbo_em_voo.py:64`; asserções `:87`, `:90`, `:92`, `:93`. |
| Carimbo em voo: segunda tentativa | Sem cura identificada | `app/chamados/tests/test_pvmuda_carimbo_em_voo.py:105`, `:113`, `:114`, `:118`. |
| Carimbo em voo: determinismo | Sem cura identificada | `app/chamados/tests/test_pvmuda_carimbo_em_voo.py:128`, `:130`. |
| Propriedades sob intercalação | Indeterminado | Asserções `app/chamados/tests/test_pvmuda_carimbo_em_voo.py:276`, `:281`, `:289`; a regra de nova resposta escreve somente projeção em `:243`, após possível recusa `:237`. Assim, igualdade entre texto dos atos e projeção não está garantida em todas as trajetórias. |
| Endpoint filtra por q_vivas | **Permanece vermelho** | Teste exige exatamente um filtro de list comprehension em `app/chamados/tests/test_contract_pergunta_viva.py:78`; código possui filtro adicional em `app/chamados/services/pergunta_no_app.py:65`, além de `:67`. D1/J1 não removem esse filtro. |
| Recusa da porta nova | Sem cura identificada | Fixture completa `app/chamados/tests/test_fio_le_o_juiz.py:167`; materializador substituído por mock em `:251`; asserções `:257`, `:258`, `:259`, `:260`. |
| Alimentação e _no_app contam tranca | Sem cura identificada | Fixture completa `app/chamados/tests/test_n1_perguntas_do_app.py:29`; asserções `:153`, `:154`, `:156`, `:157`; contador mede `periodo_trancado` em `:65`. |
| R11, duas máquinas | Indeterminado | Asserções `app/chamados/tests/test_e3_portas_tarefa.py:634`, `:636`; fixture sem resposta em `app/chamados/tests/e3_v8_apoio.py:167`; corpos e mocks carregados em `:189`. Não achei traceback no dado fornecido; causa específica e estado posterior [nao verificado]. |

Nos fluxos determinísticos de paridade, D1 acrescentaria condição verdadeira ao par completo das fixtures; J1 recebe o mesmo texto conservado. A conservação nasce em `app/chamados/models.py:4123`; devolução altera validação, preservando resposta, em `app/chamados/services/atos_pergunta.py:56`. Isso não demonstra cura dos vermelhos.

Ressalva textual: a cópia testa **somente instante** em `app/chamados/services/validacao.py:418`, enquanto o C6 testa **somente texto** em `app/chamados/juizes.py:1572`. A afirmação sobre o predicado literal do main não pode ser comprovada por essas linhas da cópia [nao verificado].

O artefato `app/chamados/tests/e4c_f1v5_base_juizes.txt:1` identifica uma fotografia do BASE `97bbaa60`, não do main solicitado; portanto, não serve como substituto do diff contra `151b0666a` (`:2`, `:3`).

## 15/11 17:18 · sessao 66db28c118f0 · - · - · origem=Ronald (tmux)

**Embrulho e leitores**

- A única instanciação produtiva encontrada de `_PerguntaParaPlantio` é `_slot_esperado`, que passa `atos.resposta` à classe e entrega o objeto a `_ts_tipo_do_plantio` (`app/chamados/juizes.py:525`). A classe só substitui `resposta_colab`; todos os demais atributos são delegados à pergunta (`app/chamados/juizes.py:384-391`). Portanto, não expõe atributo público exclusivo exigido por outro leitor (`app/chamados/juizes.py:388-391`).
- Leitores indiretos desse objeto: `_ts_tipo_do_plantio` lê `chamado_destino`, `motivo`, `data_evento`, `pk` e `resposta_colab`; `_tipo_pelo_marco`, chamado pelo primeiro, lê `motivo`, `resposta_colab`, `data_evento` e `pk` (`app/chamados/services/materializacao.py:342-359`, `:390`, `:412-421`, `:275-289`, `:308`). Esses atributos existem na pergunta original; a única diferença é o valor de `resposta_colab` substituído pelo embrulho (`app/chamados/juizes.py:389-391`).
- Ocorrências adicionais encontradas na busca global são suporte/testes/documentos: carregamento no ambiente recortado (`app/chamados/tests/e3_c8b_apoio.py:128-129`); exigência da presença do símbolo (`app/chamados/tests/test_e4c_f1v2_harnesses.py:55-56`, `:105`); mutante que remove o símbolo (`app/chamados/tests/e4c_f1v2_mutantes.py:61-65`); teste que instancia a classe e exige texto substituído, pergunta original intacta e ausência do campo em `vars` do embrulho (`app/chamados/tests/test_e4c_f1v7_regressoes.py:55-58`); relatos históricos (`app/chamados/tests/e4c_f1v7_entrega.txt:9`, `app/chamados/tests/e4c_f1v7_red.txt:38`).

**J1: fonte realmente usada**

- Materializador de hora chama `_ts_tipo_do_plantio` com a pergunta original e lê `p.resposta_colab`; o slot passa o embrulho com texto dos atos (`app/chamados/services/materializacao.py:852`; `:359`, `:412`; `app/chamados/juizes.py:525`).
- Existe outra divergência: slot de `data_hora` lê `atos.resposta`, enquanto o materializador de `data_hora` lê `p.resposta_colab` (`app/chamados/juizes.py:512-519`; `app/chamados/services/materializacao.py:1001-1008`). Assim, **só retirar o embrulho na linha 525 não basta para cumprir a expressão “mesma fonte que o materializador” para todos os tipos** (`app/chamados/juizes.py:513`; `app/chamados/services/materializacao.py:1001`).
- Texto sem instante pode continuar alimentando cálculo de slot sem ser classificado como “respondida”; esse caso está explicitamente testado sem fabricar instante (`app/chamados/tests/test_e4c_f1v6_matriz.py:143-146`). Portanto, esse leitor não demonstra contradição lógica com D1 (`app/chamados/tests/test_e4c_f1v6_matriz.py:144-146`). Há, entretanto, testes adicionais da obra que exigem texto sem instante em filas chamadas “respondidas a validar”, potencialmente incompatíveis com D1 aplicado a essas filas (`app/chamados/tests/test_e4c_f1v6_orm.py:54-58`).

**Ponto/core: tabela para consolidação**

“Vermelho” vem do anexo fornecido; onde não há transição demonstrável, mantenho resultado contrafactual `[nao verificado]`.

| Teste | c9 → D1+J1 | Fundamentação | [nao verificado]
|---|---|---|
| `test_calendario` | vermelho → `[nao verificado]`; sem cura demonstrada | Mede contagem SQL da rota e compara teto; as perguntas da fixture já têm texto e instante (`app/core/tests/test_selo_performance.py:402-406`, `:333-350`, `:317-322`). |
| `test_chamados_modal` | vermelho → `[nao verificado]`; sem cura demonstrada | Mesma medição; fixture já satisfaz D1 (`app/core/tests/test_selo_performance.py:377-380`, `:333-350`, `:317-322`). |
| `test_pendencias_disputa` | vermelho → `[nao verificado]`; sem cura demonstrada | Mesma medição; fixture já satisfaz D1 (`app/core/tests/test_selo_performance.py:354-357`, `:333-350`, `:317-322`). |
| `test_MORDE_pendente_curado_sai_da_lista` | vermelho → vermelho, se somente D1/J1 | Causa estática concreta encontrada: registro exige chamada completa numa linha; código agora acrescenta `agora=agora` e fecha chamada na linha seguinte, faltando a impressão literal (`app/core/juizes.py:878`; `app/colaboradores/services/drawer.py:121-122`; asserção `app/core/tests/test_contract_juiz_tela.py:35-38`). |
| `test_MORDE_o_mmd_commitado_bate_com_o_codigo` | vermelho → `[nao verificado]` | Exige igualdade entre artefatos e geração; D1/J1 não especificam atualização desses artefatos nem foi fornecida a divergência (`app/core/tests/test_selo_diagrama_do_codigo.py:44-47`; `app/core/management/commands/gerar_diagrama.py:921-930`). |
| `test_MORDE_uma_aresta_a_mais_no_mmd_derruba_o_selo` | vermelho → `[nao verificado]` | Duas asserções: arquivo adulterado retorna 1 e arquivo restaurado retorna 0. Sem traceback não se sabe qual falhou (`app/core/tests/test_selo_diagrama_do_codigo.py:90-98`). O próprio teste escreve arquivo, portanto não deve ser executado nesta tarefa (`app/core/tests/test_selo_diagrama_do_codigo.py:90-96`). |
| `test_registrar_batida_com_1_furo_vivo_que_ela_fecha` | vermelho → `[nao verificado]`; sem cura semântica demonstrada | Fixture cria célula/chamados de furo; verifica fechamento, callbacks e consultas, sem inserir PerguntaDisputa nesse cenário (`app/ponto/tests/test_selo_chokepoint_escrita.py:413-433`, `:435-438`, `:287-298`). |
| `test_registrar_batida_com_3_furos_vivos_que_ela_fecha` | vermelho → `[nao verificado]`; sem cura semântica demonstrada | Mesmo cenário com três chamados; verifica três fechamentos e custos (`app/ponto/tests/test_selo_chokepoint_escrita.py:416-433`, `:440-443`, `:287-298`). |
| `test_registrar_batida_com_3_furos_vivos_que_ficam` | vermelho → `[nao verificado]`; sem cura semântica demonstrada | Fixture cria célula/chamados cobrando saída e registra entrada; verifica vivos e custos (`app/ponto/tests/test_selo_chokepoint_escrita.py:453-468`, `:475-478`, `:287-298`). |
| `test_ruff_zero` | vermelho → `[nao verificado]` | Asserta código de saída do Ruff sobre a árvore inteira; anexo não fornece diagnósticos que permitam atribuir a falha a D1/J1 (`app/core/tests/test_contract_esmeril.py:22-24`). |

As medições de consultas ou callbacks não podem ser substituídas pela constatação de que o predicado mudou: os testes cobram valores medidos e o anexo só fornece nomes das falhas (`app/core/tests/test_selo_performance.py:338-350`; `app/ponto/tests/test_selo_chokepoint_escrita.py:287-298`; diagnóstico concreto das falhas: `[nao verificado]`).

## 15/11 17:17 · sessao 86555b64bf04 · - · - · origem=Ronald (tmux)

Resultados por inspeção; execução com D1+J1: [nao verificado].

| Teste | Estado informado → previsão D1+J1 | Fundamentação | [nao verificado]
|---|---|---|
| C6 `test_RED_limbo_misto_fecha_mortas_orfa_e_entrega_motivo_proprio` | vermelho → vermelho | A própria preparação exige texto **sem instante** vivo em `app/chamados/tests/test_e4b_c6_orm.py:82`; D1 torna essa pergunta morta pela célula `ok` (`app/chamados/juizes.py:1618`). Falharia antes do ato. |
| C11 mesmo teste herdado | vermelho → vermelho | Herança em `app/chamados/tests/test_e4b_c11_orm.py:27`; mesmo assert C6 `:82`. |
| C11 `test_RED_A6_beneficio_rejeitado_limbo` | vermelho → vermelho | Aciona `limbo=True` (`app/chamados/tests/test_e4b_c11_orm.py:122`), chama `_provar` (`:101`), chega ao mesmo assert C6 `:82`. |
| C12 `test_RED_desligamento` | vermelho → verde previsto [nao verificado] | Limbo sem instante em `app/chamados/tests/test_e4b_c12_orm.py:58`; D1 permite que S128 o carimbe; segunda cascata consegue carimbar a muda `admin_decidiu`, satisfazendo `:179`. |
| Demais testes C6/C11/C12 | sem mudança lógica identificada; estados de execução [nao verificado] | Preparação padrão sem texto (`app/chamados/tests/test_e4b_c6_orm.py:71`); somente desligamento acrescenta limbo em C12 (`app/chamados/tests/test_e4b_c12_orm.py:136`). Não criam divergência texto do ato/projeção para J1. |

Cadeia C12: a primeira tentativa continua retida pelo juiz próprio `respostas_sem_veredito`, cujo predicado é instante **OU** texto (`app/chamados/models.py:1952`), consultado em `app/colaboradores/services/desligamento.py:165`. Entretanto, no fechamento do chamado, D1 torna o limbo morto; S128 deixa de pulá-lo (`app/chamados/services/disputa_emissao.py:1987`), chama `carimbar(... resposta='validar')` (`:2007`), que preenche resposta/validação (`app/chamados/models.py:3260`). Na segunda execução (`app/chamados/tests/test_e4b_c12_orm.py:171`), a disputa deixa de ter a pendência e a porta de desligamento carimba a muda (`app/chamados/models.py:2029`).

No subteste desligamento **não exportado**, nenhuma mudança identificada: a pergunta viva não exportada continua impedindo o fechamento e a varredura do limbo; a expectativa é chamado aberto (`app/chamados/tests/test_e4b_c12_orm.py:166`).

Contradição explícita dos **testes** com D1: assert C6 `:82`, expectativas de conservar o limbo sem carimbo em `:116`, e os três testes que os utilizam. Isso não demonstra necessidade técnica de texto sem instante no plantio. O predicado distinto de proteção `respostas_sem_veredito` permanece OR na cópia (`app/chamados/models.py:1954`).

Cobertura: C6 tem 11 métodos de teste; C11 acrescenta 13 e herda os 11 C6 (`app/chamados/tests/test_e4b_c11_orm.py:27`); C12 tem 10, incluindo sete subcenários em `test_nao_exportada_zero_pauta_por_gatilho` (`app/chamados/tests/test_e4b_c12_orm.py:209`). Nenhum verde→vermelho adicional foi identificado [nao verificado].

## 15/11 17:17 · sessao f0c6835096f7 · E4C-F1V3 · sonda · origem=Code

Não executei o envelope: ele cria/reescreve arquivos e acrescenta uma trilha, contrariando a regra de somente leitura (bin/sonda_leitura.sh:132, bin/sonda_leitura.sh:134, bin/sonda_leitura.sh:216, bin/sonda_leitura.sh:250).

O universo fornecido não é vazio: **898 perguntas com ato RESPOSTA; 50.283 perguntas no total**. [nao verificado]

| Classe | ABE | EXP |
|---|---:|---:|
| TEXTO_DIFERE | 24 [nao verificado] | Não consta [nao verificado] |
| RESPONDIDA_SEM_ATO | 4695 [nao verificado] | 12248 [nao verificado] |
| ATO_SEM_RESPONDIDA_EM | 24 [nao verificado] | Não consta [nao verificado] |

Transcrição integral da saída fornecida; todo o bloco é **[nao verificado]**, sem nova execução:

```text
sonda_leitura: sonda_ato_x_projecao.py [2f7516615f14db9b] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

UNIVERSO perguntas_com_ato_RESPOSTA 898 perguntas_total 50283
CONT ('ATO_SEM_RESPONDIDA_EM', 'ABE') 24
CONT ('ATO_SEM_RESPONDIDA_EM', 'ABE', 'emp2 06/2026') 1
CONT ('ATO_SEM_RESPONDIDA_EM', 'ABE', 'emp2 10/2026') 2
CONT ('ATO_SEM_RESPONDIDA_EM', 'ABE', 'emp2 11/2026') 13
CONT ('ATO_SEM_RESPONDIDA_EM', 'ABE', 'emp3 10/2026') 2
CONT ('ATO_SEM_RESPONDIDA_EM', 'ABE', 'emp3 11/2026') 6
CONT ('RESPONDIDA_SEM_ATO', 'ABE') 4695
CONT ('RESPONDIDA_SEM_ATO', 'ABE', 'emp1 08/2026') 2
CONT ('RESPONDIDA_SEM_ATO', 'ABE', 'emp1 10/2026') 38
CONT ('RESPONDIDA_SEM_ATO', 'ABE', 'emp1 11/2026') 4
CONT ('RESPONDIDA_SEM_ATO', 'ABE', 'emp2 05/2026') 6
CONT ('RESPONDIDA_SEM_ATO', 'ABE', 'emp2 06/2026') 1
CONT ('RESPONDIDA_SEM_ATO', 'ABE', 'emp2 07/2026') 139
CONT ('RESPONDIDA_SEM_ATO', 'ABE', 'emp2 10/2026') 1889
CONT ('RESPONDIDA_SEM_ATO', 'ABE', 'emp2 11/2026') 468
CONT ('RESPONDIDA_SEM_ATO', 'ABE', 'emp29 07/2026') 1
CONT ('RESPONDIDA_SEM_ATO', 'ABE', 'emp3 05/2026') 2
CONT ('RESPONDIDA_SEM_ATO', 'ABE', 'emp3 06/2026') 21
CONT ('RESPONDIDA_SEM_ATO', 'ABE', 'emp3 08/2026') 1252
CONT ('RESPONDIDA_SEM_ATO', 'ABE', 'emp3 10/2026') 290
CONT ('RESPONDIDA_SEM_ATO', 'ABE', 'emp3 11/2026') 156
CONT ('RESPONDIDA_SEM_ATO', 'ABE', 'emp4 05/2026') 7
CONT ('RESPONDIDA_SEM_ATO', 'ABE', 'emp4 08/2026') 270
CONT ('RESPONDIDA_SEM_ATO', 'ABE', 'emp4 10/2026') 96
CONT ('RESPONDIDA_SEM_ATO', 'ABE', 'emp4 11/2026') 53
CONT ('RESPONDIDA_SEM_ATO', 'EXP') 12248
CONT ('RESPONDIDA_SEM_ATO', 'EXP', 'emp2 08/2026') 7909
CONT ('RESPONDIDA_SEM_ATO', 'EXP', 'emp2 09/2026') 2334
CONT ('RESPONDIDA_SEM_ATO', 'EXP', 'emp3 07/2026') 907
CONT ('RESPONDIDA_SEM_ATO', 'EXP', 'emp3 09/2026') 473
CONT ('RESPONDIDA_SEM_ATO', 'EXP', 'emp4 06/2026') 235
CONT ('RESPONDIDA_SEM_ATO', 'EXP', 'emp4 07/2026') 327
CONT ('RESPONDIDA_SEM_ATO', 'EXP', 'emp4 09/2026') 63
CONT ('TEXTO_DIFERE', 'ABE') 24
CONT ('TEXTO_DIFERE', 'ABE', 'emp2 06/2026') 1
CONT ('TEXTO_DIFERE', 'ABE', 'emp2 10/2026') 2
CONT ('TEXTO_DIFERE', 'ABE', 'emp2 11/2026') 13
CONT ('TEXTO_DIFERE', 'ABE', 'emp3 10/2026') 2
CONT ('TEXTO_DIFERE', 'ABE', 'emp3 11/2026') 6
PKS TEXTO_DIFERE 24 38887 39180 42244 46751 48773 51830 52502 52698 53491 53644 53782 53900 54260 54385 55295 55313 55641 56287 56608 56718 56883 56923 57211 57214
PKS RESPONDIDA_SEM_ATO 16943 6 7 8 118 119 120 158 174 175 176 180 181 183 184 185 186 189 190 193 194 200 202 203 207 208 263 264 265 266 267 268 272 287 289 292 293 294 295 301 302 305 307 308 309 310 311 313 320 321 322 323 324 325 326 327 328 329 330 331 334 335 337 345 348 349 350 351 354 355 357 358 359 363 364 365 366 369 370 373 374 375 381 390 398 400 401 405 406 407 412 415 418 419 420 424 427 430 435 439 445 448 454 455 456 457 458 459 461 463 466 470 473 476 482 483 484 488 490 495 496 499 504 505 510 511 513 514 516 518 519 521 524 525 526 528 531 534 540 545 547 550 551 553 557 560 562 563 564 565 566 567 569 570 576 577 578 612 613 614 615 616 626 627 671 672 673 674 675 676 677 678 690 691 692 700 701 703 704 705 706 707 709 710 711 712 713 714 715 716 717 719 720 721 723 725 726 727 728 729 730
PKS ATO_SEM_RESPONDIDA_EM 24 38887 39180 42244 46751 48773 51830 52502 52698 53491 53644 53782 53900 54260 54385 55295 55313 55641 56287 56608 56718 56883 56923 57211 57214
```

## 15/11 16:06 · sessao 68f26a1eb8a9 · L2-BACKFILL · verificador · origem=Code

**AVAL-DEVOLVIDO: REEMISSAO-1600, item 3 — perguntas 1 e 3; ressalva de comprovação na 4.** (`avais_1600.txt:4`; `CLAUDE.md:37`)

Nos anexos, conto linhas desde a primeira linha de cada bloco fornecido. [nao verificado]

1. **PROCEDE — o molde citado não fornece o DRY de produção somente leitura.**

   O comando tem DRY default, `--aplicar` e PAREI com lista de exportadas. Porém, **sem `--aplicar`, chama `aplicar()` dentro de uma transação e depois faz rollback**. Portanto, esse DRY tenta executar o caminho escritor; rollback não o transforma em leitura. O envelope configura transações somente leitura. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:425`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:478`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:578`; `bin/sonda_leitura.sh:142`)

   Assim, “item novo no molde” não basta para executar “DRY em prod, leitura pelo envelope”: falta explicitar o caminho L2 que somente seleciona e mede. Sua implementação permanece **[nao verificado]**. O molde também deixa exportadas fora do apply; não oferece passagem específica para ensaiar escrita nelas na sombra. (`avais_1600.txt:4`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:253`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:263`)

   **A porta comporta a linha pedida:** `RETRATACAO` existe; `ato_motivo` é texto sem `choices`, portanto aceita `'obsoleta'`; `ato_em` recebe o datetime fornecido, sem restrição temporal nessa porta. A chamada correta é `registrar([dict(...)])`. (`app/chamados/models.py:4001`; `app/chamados/models.py:4016`; `app/chamados/models.py:4019`; `app/chamados/models.py:4086`; `app/chamados/models.py:4096`)

   “Primeira vaga depois da c14” é condição futura, não prova de vaga disponível; disponibilidade efetiva **[nao verificado]**. Antes de abrir, permanece obrigatório conferir o teto. (`avais_1600.txt:4`; `CLAUDE.md:291`)

2. **NAO PROCEDE — existe “senão” geral.**

   Condição literal que não fecha significa **PAROU com o número**. Isso cobre ausência de vaga, cruzamento de arquivo e resultado insuficiente das provas. Não é necessário repetir um “senão” em cada cláusula. (`CLAUDE.md:37`; `avais_1600.txt:8`)

3. **PROCEDE — há cruzamento concreto com a cópia RELAVRA-11.**

   O comando já diverge nela: o main consulta `competencia_entregue`, enquanto a cópia consulta `competencia_ja_paga`, incluindo o mesmo trecho de classificação usado pelo DRY. Logo, a condição “não cruza arquivo […] nem a cópia da RELAVRA-11” **não fecha agora**; não há autorização para ignorá-la. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:453`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:471`; `/home/ronald/wt-relavra11-113556/app/chamados/management/commands/backfill_decisoes_pergunta.py:456`; `/home/ronald/wt-relavra11-113556/app/chamados/management/commands/backfill_decisoes_pergunta.py:474`; `avais_1600.txt:4`; `app/docs/PISTA.md:44`; `CLAUDE.md:37`)

   Nos diffs R3/F1 examinados, comando e testes diretos `*e1*` não apareceram; a ausência não tem listagem persistida com arquivo:linha: **[nao verificado]**. Há teste relacionado na F1: a catraca examina explicitamente `backfill_decisoes_pergunta::linhas_a_criar`. (`/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/tests/test_e3_catraca_respondedores.py:68`; `/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/tests/test_e3_catraca_respondedores.py:81`) [nao verificado]

   Não há colisão demonstrada com escritor único ao usar `registrar`. A exceção ao COMPLEMENTO-3 é nominal. L-127 continua sendo vocabulário; a ordem Backfill → leitores tem suporte documental no ADR, que qualifica seu contexto como redação do Code. (`app/chamados/models.py:3989`; `avais_1600.txt:4`; `app/docs/LEIS.md:148`; `app/docs/PLANO-PADROES.md:81`; `app/docs/PLANO-PADROES.md:86`)

4. **NAO PROCEDE quanto às referências principais; exclusão universal e universo permanecem não comprovados.**

   - `_retratada_com_trilha` exige **carimbo + log correlacionado por ação, modelo e PK**. “O mesmo filtro” precisa preservar os três campos do log. (`app/chamados/juizes.py:291`; `app/chamados/juizes.py:294`)
   - O caminho completo é `chamados/services/carga_pergunta_viva.py`. A linha 111 lê **LogAuditoria**, não DP. Isso não demonstra um leitor já migrado para atos. (`app/chamados/services/carga_pergunta_viva.py:110`)
   - `remover_motivo` grava **RETRATACAO**, com motivo formatado `"<evento>: motivo <motivo> removido (via antes: <via>)"`, diferente de `'obsoleta'`, e preserva o carimbo existente. (`app/chamados/models.py:2120`; `app/chamados/models.py:2123`; `app/chamados/models.py:2125`)
   - O selecionador exclui a pergunta **depois de persistida a DP obsoleta**, sustentando idempotência sequencial. `registrar` não deduplica nem há constraint de unicidade desse ato; execução e proteção concorrente L2 são **[nao verificado]**. (`avais_1600.txt:4`; `app/chamados/models.py:4033`; `app/chamados/models.py:4096`)
   - O seletor **não exclui expressamente quem passou por `remover_motivo`**. Se também houver log obsoleta anterior e faltar DP obsoleta, seus critérios continuam satisfeitos. A existência dessa interseção em produção é **[nao verificado]**. (`avais_1600.txt:4`; `app/chamados/models.py:2120`; `app/chamados/juizes.py:294`)
   - `disputa_emissao.py:1494–1495` realmente zera validação e autor. (`app/chamados/services/disputa_emissao.py:1494`)
   - **52/39/13 não são medida primária comprovada aqui**; o aval manda substituí-los pelo DRY futuro. Contagens atuais e efeitos efetivos em FM/TXT são **[nao verificado]**. Registrar DP efetivamente aciona o acordamento de células. (`avais_1600.txt:4`; `app/chamados/models.py:4097`)

## 15/11 16:04 · sessao a43f858a2c0b · - · - · origem=Ronald (tmux)

**Q2 — NAO PROCEDE:** existe “senão” geral: condição literal que não fecha implica PAROU com o número; a abertura também exige conferir teto de dois construtores. (`CLAUDE.md:37`; `CLAUDE.md:291`)

**Q3 — PROCEDE quanto ao cruzamento com a cópia RELAVRA-11.** O comando diverge do main: a cópia usa `competencia_ja_paga`; o main usa `competencia_entregue`. Logo, a condição “não cruza arquivo […] nem a cópia RELAVRA-11” não está satisfeita agora. Isso exige parar essa abertura; o aval não autoriza ignorar a condição. (`/home/ronald/wt-relavra11-113556/app/chamados/management/commands/backfill_decisoes_pergunta.py:456`; mesma cópia `:474`, `:501`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:453`, `:471`, `:498`; `CLAUDE.md:37`; `app/docs/PISTA.md:44`)

Outras verificações:

- A porta `DecisaoPergunta.registrar` respeita o escritor único requerido pelo selo existente; ele recusa construção e escrita ORM fora de `models.py`. (`app/chamados/tests/test_e1_entradas.py:185`; `:196`; `:217`)
- `test_e4_backfill.py` está alterado nos diffs R3/F1, mas testa **outro comando**, `backfill_fio_morto_e4b`. Não deve ser apresentado como teste direto de `backfill_decisoes_pergunta`. (`/home/ronald/codex_ws/e08db44a694aa355db6abcb54ee5290419168934/app/chamados/tests/test_e4_backfill.py:31`; `:36`)
- F1 altera um arquivo de catraca que também examina `backfill_decisoes_pergunta::linhas_a_criar`; esse arquivo relacionado não está livre de sobreposição caso L2 precise editá-lo. (`/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/tests/test_e3_catraca_respondedores.py:65`; `:68`; `:81`)
- Ausência do comando e dos testes diretos `*e1*` nos diffs R3/F1 foi conferida por `git diff --name-only`; não há artefato persistido com arquivo:linha dessa listagem: **[nao verificado]** segundo a exigência estrita de citação. [nao verificado]
- A ordem Expand → Backfill → leitores → Contract tem suporte documental; o próprio ADR declara que seu contexto é redação do Code. O literal original de `aval_expand_contract.txt` não foi acessado: **[nao verificado]**, além da transcrição fornecida na revisão anterior. (`app/docs/PLANO-PADROES.md:81`; `:86`; `app/docs/BACKLOG.md:1968`)
- L-127 continua sendo vocabulário; não é, isoladamente, a fonte da ordem. (`app/docs/LEIS.md:148`)
- “Primeira vaga depois c14” é condição futura; sua disponibilidade efetiva permanece **[nao verificado]**. Há “senão” suficiente para não abrir quando faltar vaga ou houver cruzamento. (`CLAUDE.md:37`; `:291`; `app/docs/PISTA.md:44`)

## 15/11 16:04 · sessao 9848e5608028 · - · - · origem=Ronald (tmux)

Achados do comando e da porta:

- **PROCEDE quanto ao DRY literal de produção:** o molde atual chama `aplicar()` mesmo sem `--aplicar`, e só desfaz a transação depois. Há escrita tentada e travas; não é execução somente leitura. A porta do envelope usa role sem privilégios de escrita. É necessário definir um DRY L2 realmente leitor, separado desse fluxo; isso não está explicitado no aval. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:578-584`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:217`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:324`; `bin/sonda_leitura.sh:11-16`; `bin/sonda_leitura.sh:142`)

- O molde **tem** DRY por default, flag `--aplicar` e saída PAREI com lista de PKs exportadas. Entretanto, o apply atual aplica grupos abertos e deixa inteiros de fora os grupos exportados; não para necessariamente o ato inteiro. O teste prende precisamente esse comportamento. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:425-427`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:478-482`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:620-628`; `app/chamados/tests/test_e1_v13d_exportada_fora.py:164-175`)

- **Ensaio exportado na sombra:** o veto existente não contém exceção por ambiente; exclui exportadas antes de planejar/escrever. Portanto, simplesmente acrescentar item no mesmo molde não fornece o caminho para ensaiar escrita nas exportadas. Esse caminho depende de implementação adicional **[nao verificado]**, e o literal não distingue como o ensaio atravessa `exportada = PAREI`. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:253-265`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:302-309`; anexo `avais_1600.txt:4`)

- `registrar` **aceita** `ato=RETRATACAO`, `ato_motivo='obsoleta'` e `ato_em` datetime fornecido. `RETRATACAO` é ato existente; motivo é CharField sem choices; data não tem mecanismo automático nem validação temporal nessa porta. As constraints são de absorção e foto, não restringem esse motivo ou o instante histórico. “Arbitrário” não dispensa o tipo datetime válido. (`app/chamados/models.py:4001`; `app/chamados/models.py:4009`; `app/chamados/models.py:4016`; `app/chamados/models.py:4019`; `app/chamados/models.py:4033-4043`; `app/chamados/models.py:4086-4096`)

- A porta **não deduplica**: faz `bulk_create`. A idempotência de uma segunda execução sequencial pode nascer do antijoin proposto, mas sua implementação/execução L2 e proteção contra concorrência ficam **[nao verificado]**. (`app/chamados/models.py:4094-4098`; anexo `avais_1600.txt:4`)

- Cada chamada que registra DP acorda as células encontradas; emite uma por célula da chamada e pula pergunta sem célula resolvível. `emitir_evento_celula` cria efetivamente `EventoCelula`. Não dá para prometer zero efeito em fechamento/TXT a partir só dessa porta. (`app/chamados/models.py:4097-4098`; `app/chamados/models.py:3314-3331`; `app/ponto/portas/celula.py:945-954`)

- A marca em `LogAuditoria` cabe no schema, que tem ação, objeto, antes/depois e ator. A convenção concreta e a escrita L2 ainda são **[nao verificado]**. Atenção: a API `evento` escreve Log dentro de atomic, mas isso por si não prova que a convenção L2 está implementada. (`app/core/models.py:296-307`; `app/core/observ.py:133-137`)

- Chamar `registrar` respeita o escritor único de DP. Existe catraca que permite escrita do backfill apenas na função de módulo `backfill_decisoes_pergunta.aplicar`; uma nova função escritora em outro ponto quebrará esse selo, salvo alteração deliberada correspondente. (`app/chamados/models.py:3989-3992`; `app/chamados/tests/test_e1_entradas.py:185-222`; `app/chamados/tests/test_e1_v7_catracas.py:253-288`)

- `remover_motivo` grava **RETRATACAO**, mas o motivo é formatado `"<evento>: motivo <motivo> removido (via antes: <via>)"` e não o literal `'obsoleta'`; depois carimba identidade com `VIA_MOTIVO_RETRATADO`, mantendo carimbo anterior quando existe. Isso confirma o MORDE sobre o valor gravado, porém não prova exclusão de uma pergunta que também tenha um log obsoleta anterior. (`app/chamados/models.py:2118-2126`)

## 15/11 16:03 · sessao 7d408a8b92b7 · - · - · origem=Ronald (tmux)

- `_retratada_com_trilha` existe e exige `materializada_em` não nulo, mais `LogAuditoria` com **ação, modelo e objeto**: `acao='retratar_pergunta_obsoleta'`, `modelo='PerguntaDisputa'`, `objeto_id=p.pk`. A expressão resumida do aval precisa preservar a correlação modelo/PK para equivaler ao filtro. (`app/chamados/juizes.py:285`; `app/chamados/juizes.py:291`; `app/chamados/juizes.py:294`)
- `carga_pergunta_viva.py:111` está em **services**. Lê `LogAuditoria`, não `DecisaoPergunta`, para as PKs de `perguntas`; `retratada()` reaproveita `_retratada_com_trilha` com a trilha carregada. (`app/chamados/services/carga_pergunta_viva.py:110`; `app/chamados/services/carga_pergunta_viva.py:192`)
- `remover_motivo` grava `ato=RETRATACAO`, instante atual `_agora_ret`, e motivo textual `'%s: motivo %s removido (via antes: %s)'[:240]`; portanto **não grava motivo igual a `'obsoleta'`**. Depois carimba a via `VIA_MOTIVO_RETRATADO`, preservando carimbo existente. (`app/chamados/models.py:2088`; `app/chamados/models.py:2120`; `app/chamados/models.py:2123`; `app/chamados/models.py:2125`; `app/chamados/models.py:3265`)
- Isso **não prova exclusão universal da pergunta de `remover_motivo`**: o selecionador proposto não exclui essa via nem outras retrações; uma pergunta que também possua log obsoleta e careça da DP obsoleta satisfaz seus critérios. A existência concreta dessa interseção em produção fica **[nao verificado]**. A porta `remover_motivo` por si só não fornece esse log. (`app/chamados/models.py:2088`; `app/chamados/models.py:2118`; `app/chamados/models.py:2120`; `app/chamados/juizes.py:294`)
- A fórmula é idempotente **para rodadas sequenciais concluídas**, pois a linha criada passa a satisfazer a exclusão. Não há garantia concorrente nessa porta: `registrar` executa `bulk_create` sem deduplicação, e os constraints existentes regulam fotos/absorção, sem unicidade do ato obsoleta. Implementação e ensaio L2 permanecem **[nao verificado]**. (`app/chamados/models.py:4033`; `app/chamados/models.py:4086`; `app/chamados/models.py:4094`)
- O zeramento de validação está corretamente em `1494–1495`; `1480–1481` conserva decisões anteriores e atribui resposta. (`app/chamados/services/disputa_emissao.py:1480`; `app/chamados/services/disputa_emissao.py:1494`)

## 15/11 15:59 · sessao 1c1728d29871 · TELA-DIZ-O-QUE-O-SERVIDOR-DISSE · verificador · origem=Code

1. **PROCEDE**, no ramo `r.ok`: `ok` mantém o sucesso; `ja_registrada`, `ignorada_intervalo` e outro/ausente/JSON ilegível recebem exatamente os textos previstos. Todos usam o mesmo overlay com `inset:0` e retorno ao dashboard agendado em 3000 ms. Não há cálculo de próximo tipo nem exibição de texto recebido do servidor nesse tratamento. `app/templates/ponto/bater_ponto_v2.html:150`, `app/templates/ponto/bater_ponto_v2.html:156`, `app/templates/ponto/bater_ponto_v2.html:168`.

   Ressalva de alcance: respostas redirecionadas continuam saindo antes de `r.ok`, pelo caminho preexistente. `app/templates/ponto/bater_ponto_v2.html:364`.

2. **NÃO PROCEDE** a falha nos casos examinados: JSON ilegível é contido pelo `try` interno; sucesso exige `d.status === 'ok'`. O `catch` externo continua envolvendo também a apresentação, mas nenhum dos corpos examinados demonstra exceção escapando até ele. `app/templates/ponto/bater_ponto_v2.html:170`, `app/templates/ponto/bater_ponto_v2.html:171`, `app/templates/ponto/bater_ponto_v2.html:365`, `app/templates/ponto/bater_ponto_v2.html:369`.

   **PROCEDE**, pelo diff fornecido, que os ramos não-2xx e offline foram preservados; correspondem aos caminhos atuais em `app/templates/ponto/bater_ponto_v2.html:366`, `app/templates/ponto/bater_ponto_v2.html:370`, `app/templates/ponto/bater_ponto_v2.html:382`. Comparação independente com o commit base: **[não verificado]**.

3. **PROCEDE** o contrato lido pelo cliente:

   | Corpo/valor escrito | Servidor core | Servidor UI / serviço |
   |---|---|---|
   | Deduplicação: `status='ja_registrada'` | `app/api/views_core.py:521` | `app/api/views.py:535` |
   | Colisão: `status='ignorada_intervalo'` | `app/api/views_core.py:673` | `app/api/views.py:682` |
   | Reutilização: corpo produzido pelo serviço, `status='ja_registrada'` | `app/api/views_core.py:684` | `app/api/views.py:693`; `app/ponto/services/resposta_batida.py:21` |
   | Criação: `status='ok'` | `app/api/views_core.py:807` | `app/api/views.py:813` |
   | `aviso`: frase ou string vazia, somente no `ok` | `app/api/views_core.py:805`, `app/api/views_core.py:817` | `app/api/views.py:811`, `app/api/views.py:823`; produtor: `app/ponto/services/aviso_da_batida.py:55` |

   O cliente decodifica o corpo e lê somente `d.status`; não lê `aviso`, `mensagem`, `sucesso` ou `tipo` da resposta. `v2Aviso` é uma função de apresentação de textos locais, não um leitor de `d.aviso`. `app/templates/ponto/bater_ponto_v2.html:163`, `app/templates/ponto/bater_ponto_v2.html:170`, `app/templates/ponto/bater_ponto_v2.html:172`.

   **NÃO PROCEDE** haver outro status nos retornos 2xx dessas funções: os quatro caminhos acima produzem apenas os três valores. `app/api/views_core.py:521`, `app/api/views_core.py:673`, `app/api/views_core.py:686`, `app/api/views_core.py:807`; `app/ponto/services/resposta_batida.py:22`.

   Não há dupla semântica no campo lido: `r.status` é HTTP; `d.status` é resultado da batida. Existe ambiguidade preexistente em `sucesso=True`, inclusive para `ignorada_intervalo`, mas a cura não usa esse campo. `app/templates/ponto/bater_ponto_v2.html:170`, `app/templates/ponto/bater_ponto_v2.html:367`; `app/api/views_core.py:680`, `app/api/views_core.py:814`.

4. **PROCEDE** que o RED morde o texto visível nas duas cascas: elas são enumeradas explicitamente, renderizadas pelo helper, e a sonda lê `innerText` do elemento no centro da janela. `app/ponto/tests/test_camera_alternativa_previa.py:38`, `app/ponto/tests/test_camera_alternativa_previa.py:144`; `app/ponto/tests/test_tela_diz_o_que_o_servidor_disse.py:94`.

   **NÃO PROCEDE** passar com texto vazio: há guarda positiva para `ok`, igualdade com textos não vazios nos demais e exigência de um POST/zero registros offline. `app/ponto/tests/test_tela_diz_o_que_o_servidor_disse.py:136`, `app/ponto/tests/test_tela_diz_o_que_o_servidor_disse.py:143`, `app/ponto/tests/test_tela_diz_o_que_o_servidor_disse.py:151`.

   **PROCEDE**, por análise, que ambos os mutantes morreriam: sucesso incondicional viola a proibição de “êxito”; JSON sem proteção alcança o registro offline e viola sua contagem zero. `app/ponto/tests/test_tela_diz_o_que_o_servidor_disse.py:65`, `app/ponto/tests/test_tela_diz_o_que_o_servidor_disse.py:136`, `app/ponto/tests/test_tela_diz_o_que_o_servidor_disse.py:150`; `app/templates/ponto/bater_ponto_v2.html:371`. Execuções e quantidades de falhas relatadas: **[não verificado]**.

   Limites: callback vazio de retorno passaria, porque o teste apenas conta e descarta timers de 3000 ms; “tela cheia” verifica somente `position:fixed`. O código atual contém destino e geometria corretos. `app/ponto/tests/test_tela_diz_o_que_o_servidor_disse.py:69`, `app/ponto/tests/test_tela_diz_o_que_o_servidor_disse.py:96`; `app/templates/ponto/bater_ponto_v2.html:153`, `app/templates/ponto/bater_ponto_v2.html:156`.

5. **NÃO PROCEDE**, no tratamento novo, introdução de segundo juiz, segundo escritor de batida, relógio de negócio ou derivação paralela: ele traduz o status recebido em apresentação. O selo tem mordida para essa tradução e percorre ambas as cascas. `app/templates/ponto/bater_ponto_v2.html:168`; `app/ponto/tests/test_tela_diz_o_que_o_servidor_disse.py:140`, `app/ponto/tests/test_tela_diz_o_que_o_servidor_disse.py:145`.

   Smoke automatizado não comprova o clique humano: a sonda chama `v2Confirmar` diretamente; a exigência de clique permanece distinta. `app/ponto/tests/test_tela_diz_o_que_o_servidor_disse.py:92`; `CLAUDE.md:202`. Smoke iPhone/Android, RED/GREEN e suites efetivamente executados: **[não verificado]**. A aprovação aqui é da pré-verificação do código; esses resultados não ficam certificados.

6. **NÃO PROCEDE** consumidor omitido no censo estático de `templates/`: **N=1**, a própria v2. O outro consumidor encontrado é o sincronizador excluído pelo aval: ele decodifica JSON, ignora `data.status` e remove a pendência por `resp.ok`. `app/templates/ponto/bater_ponto_v2.html:360`; `app/static/pwa.js:153`, `app/static/pwa.js:158`, `app/static/pwa.js:162`.

   A tela antiga envia à própria rota e consome HTML; o service worker não intercepta POST. `app/templates/ponto/bater_ponto.html:237`, `app/templates/ponto/bater_ponto.html:533`, `app/templates/ponto/bater_ponto.html:539`; `app/static/sw.js:36`. Inclusão efetiva da linha proposta no BACKLOG: **[não verificado]**.

VEREDITO: APROVA

## 15/11 15:58 · sessao 62f3c873c59d · - · - · origem=Ronald (tmux)

6. **NÃO PROCEDE** a hipótese de outro consumidor omitido em `templates/`: o censo estático por endpoint, nome de rota, nomes das funções e valores de status encontrou **N=1**, a v2; envio em `app/templates/ponto/bater_ponto_v2.html:360`, tratamento em `:365`, leitura em `:170`.

- Fora de `templates/`, o consumidor encontrado é `app/static/pwa.js:153`: decide pelo HTTP em `:158`, lê JSON em `:159`, mas ignora `data.status` e remove a pendência em `:162`. Sua interface é o indicador agregado em `:194`; a exclusão do sincronizador está expressa no aval fornecido, anexo `reemissao_1215.txt`, item A [não verificado quanto a arquivo físico].
- A tela antiga não consome essa resposta: seu formulário não define `action` (`app/templates/ponto/bater_ponto.html:237`) e o fetch envia à própria localização (`:533`), consumindo HTML (`:539`). A rota web é separada: `app/ponto/urls.py:11`, com processamento em `app/ponto/views.py:271` e `:352`.
- `sw.js` não constitui outro consumidor desse POST: retorna antes de interceptar métodos diferentes de GET (`app/static/sw.js:35`).
- Rotas API confirmadas: `app/api/urls.py:67` aponta para `views`; `app/api/urls_core.py:14` aponta para `views_core` por importação em `:5`; inclusão dos roteadores em `app/config/urls.py:37` e `app/config/urls_core.py:6`.
- O template é classe A por exceção nominal: `app/docs/CORTES.md:411` e `:413`. O arquivo raspado `leis_sitios.txt` fornece numeração diferente; nesta cópia, os endereços vivos são 411/413.
- A linha `PWA-SYNC-NAO-DIZ-STATUS` aparece como proposta no anexo; sua efetiva inclusão no BACKLOG não foi demonstrada [não verificado].
- Smoke manual iPhone/Android permanece pendente segundo o próprio anexo `entrega.md`; execução real desses cliques [não verificado].

## 15/11 15:58 · sessao 3146cc6c831d · - · - · origem=Ronald (tmux)

Pergunta 3: **PROCEDE** o contrato usado pelo cliente; **NÃO PROCEDE** haver outro `status` 2xx emitido por essas funções. Os retornos são:

| Resultado | `api/views_core.py` | `api/views.py` |
|---|---|---|
| Deduplicação: `ja_registrada` | 521–531 | 535–545 |
| Colisão: `ignorada_intervalo` | 673–683 | 682–692 |
| Reutilização: `ja_registrada` | 684–686 | 693–695 |
| Criação: `ok` | 807–818 | 813–824 |

O corpo reutilizado vem de `app/ponto/services/resposta_batida.py:21`, com `status='ja_registrada'` em `:22`; o serviço identifica criação pela marca `criada_neste_toque` em `:14`, escrita nas criações em `app/ponto/registro_batida.py:155` e `:161`.

O cliente lê o corpo por `r.json()` e exclusivamente seu `d.status`; compara estritamente os três valores e usa textos locais. `aviso`, `mensagem`, `tipo` e `sucesso` do JSON não participam dessa decisão. `app/templates/ponto/bater_ponto_v2.html:170`, `:171`, `:172`, `:173`, `:174`.

`aviso` é uma string com frase ou `''`, produzida por `aviso_do_dia`; só aparece no retorno `ok`. `app/ponto/services/aviso_da_batida.py:55`, `:69`, `:79`, `:90`; `app/api/views_core.py:805`, `:817`; `app/api/views.py:811`, `:823`. O payload de reutilização não contém essa chave. `app/ponto/services/resposta_batida.py:21`.

A função cliente `v2Aviso` apresenta textos locais e não lê o campo JSON `aviso`. `app/templates/ponto/bater_ponto_v2.html:163`, `:172`. O aviso inicial da página é outro canal: contexto `aviso_do_dia`, preenchido em `app/ponto/services/triagem_batida.py:764`, `:765` e exibido em `app/templates/ponto/bater_ponto_v2.html:55`, `:56`.

Não aparece dupla semântica nova nos campos lidos: `r.ok` trata HTTP e `d.status` trata resultado da batida. `app/templates/ponto/bater_ponto_v2.html:365`, `:170`. Existe uma ambiguidade preexistente em **`sucesso=True`**, inclusive quando nada novo foi registrado, mas o diff não lê essa chave. `app/api/views_core.py:680`, `:814`; `app/api/views.py:689`, `:820`; `app/templates/ponto/bater_ponto_v2.html:170`.

As duas rotas de código existem: UI inclui `api.urls`, que aponta para `views.api_bater_ponto`; kernel inclui `api.urls_core`, que aponta para `views_core.api_bater_ponto`. `app/config/urls.py:37`; `app/api/urls.py:2`, `:67`; `app/config/urls_core.py:6`; `app/api/urls_core.py:5`, `:14`. Qual delas atende produção: [não verificado].

Correção pontual da entrega: o literal `ignorada_intervalo` está em `app/api/views_core.py:674`; `:675` contém `id`.

## 15/11 15:58 · sessao cd9b16af6fec · - · - · origem=Ronald (tmux)

O RED morde os comportamentos pedidos nas duas cascas: o mock responde 200 com os seis corpos (`app/ponto/tests/test_tela_diz_o_que_o_servidor_disse.py:51`), chama a função real de envio (`:92`), mede `innerText` do elemento no centro da janela (`:94`) e exige um POST, elemento fixo, um agendamento de 3000 ms e nenhuma escrita offline (`:136`). As duas cascas são explícitas (`app/ponto/tests/test_camera_alternativa_previa.py:38`), carregadas pelo loader com o conteúdo real de cada raiz (`:144`).

Não passa vazio: a sonda ausente e erros falham (`app/ponto/tests/test_tela_diz_o_que_o_servidor_disse.py:132`); o caso `ok` exige o texto positivo (`:143`); cada não-ok exige igualdade com texto não vazio (`:151`). JS ausente também falharia pelo POST zero (`:136`).

Os mutantes descritos morreriam por análise: sucesso incondicional exibe “êxito” e viola `:150`; JSON sem `try` rejeita no caso ilegível (`:65`), alcança o catch externo que registra offline (`app/templates/ponto/bater_ponto_v2.html:369`), violando `:136`. Quantidades de falhas relatadas e execuções efetivas: [não verificado].

Limites reais da prova:

- O retorno não é executado: todo timer de 3000 ms é interceptado e descartado (`app/ponto/tests/test_tela_diz_o_que_o_servidor_disse.py:67`). Um callback vazio passaria; o destino e o retorno correto estão demonstrados pelo código atual (`app/templates/ponto/bater_ponto_v2.html:156`).
- “Tela cheia” mede apenas `position:fixed` (`app/ponto/tests/test_tela_diz_o_que_o_servidor_disse.py:96`); geometria integral está no código atual, `inset:0` (`app/templates/ponto/bater_ponto_v2.html:153`).
- A chamada é direta a `v2Confirmar`, sem clique (`app/ponto/tests/test_tela_diz_o_que_o_servidor_disse.py:92`); o Chromium usa configuração desktop padrão (`app/core/tests/test_smoke_chromium.py:104`). Isso não comprova os smokes humanos iPhone/Android: [não verificado].
- A exigência de duas cascas está satisfeita no teste (`app/ponto/tests/test_tela_diz_o_que_o_servidor_disse.py:141`, `:146`; `CLAUDE.md:203`). Smoke de clique continua uma prova distinta (`CLAUDE.md:202`).

## 15/11 15:40 · sessao 3347d475b5e6 · E3-RESTO-R3 · verificador · origem=Code

1. **F1 — Alta: `--apply` fecha pauta de colaborador cuja apuração falhou.** Se `_ins.cxr()` lança exceção, `apurar` omite o colaborador sem registrá-lo em `sem_leitura`. Sua empresa/competência permanece no recorte; `sincronizar` interpreta a ausência como saída do alvo e fecha a pauta, afirmando que os dias ganharam fio ou veredito novo. Caso sintético reproduzido em memória: col7, empresa1, competência10/2026, pauta viva e falha do espelho → `apurado=[]`, `sem_leitura=[]`, um fechamento. A cura protege outras empresas/competências, mas ainda permite fechar trabalho não apurado. (`app/ponto/management/commands/pautas_discordantes.py:103`, `app/ponto/management/commands/pautas_discordantes.py:126`, `app/ponto/services/pautas_dos_discordantes.py:194`, `app/ponto/services/pautas_dos_discordantes.py:211`.)

   O teste do comando substitui `apurar` por `Mock(return_value=[])`; portanto não exercita essa falha. (`app/chamados/tests/test_e3_r3_c11_sem_banco.py:142`.)

2. **F2 — Média: retrato legado mascara alarme positivo conhecido.** Basta uma linha bloqueada sem `dias_sem_leitura` para o total virar `None`, inclusive quando outra linha já informou dias problemáticos. O comando trata `None` como não alarmante. Caso sintético reproduzido: uma linha nova com um dia sem leitura → contador `1`, saída `2`; acrescentando uma linha legada → contador `None`, saída `0`, mesmo com `--alarme --alarme-passivo`. (`app/folha/vigia_retido_calado.py:57`, `app/folha/vigia_retido_calado.py:63`, `app/folha/management/commands/vigiar_retido_calado.py:38`.)

   Os testes cobrem positivo e legado separadamente; não cobrem positivo misturado com legado. (`app/chamados/tests/test_e3_r3_c6_sem_banco.py:251`, `app/chamados/tests/test_e3_r3_c6_sem_banco.py:258`.)

3. **F3 — Média: a cura do relógio não alcançou os chamadores reais da tela.** `o_que_fazer` aceita `hoje`, mas seu chamador de produção não o fornece; `desenho_do_led` também chama o leitor sem ele. Filtro, LED e pílulas continuam chegando a `timezone.localdate()` separadamente. Caso concreto: uma resposta atravessa meia-noite com linha `ninguem`, `datas=['2026-11-15']`; ela pode entrar pelo filtro **Hoje** e depois receber LED/barra **Sem ação disponível**. (`app/colaboradores/views.py:986`, `app/folha/retrato_folha.py:460`, `app/folha/retrato_folha.py:553`, `app/folha/retrato_folha.py:650`, `app/folha/retrato_folha.py:830`.)

   O teste c11 prova somente o caminho com `hoje` explicitamente fornecido, não a integração desses chamadores. (`app/chamados/tests/test_e3_r3_c11_sem_banco.py:230`.)

4. **F4 — Média: o novo destino “Resolver dia” perde a casca necessária à navegação do calendário.** A Folha abre diretamente `calendario_ponto` no drawer. A resposta é o partial cujas setas e modos apontam para `#calendario-container`; esse elemento pertence ao painel anterior, substituído por `corpo.innerHTML = html`, e não é recriado pelo calendário. O mês inicial abre, mas os controles ficam sem seu destino HTMX. (`app/folha/retrato_folha.py:1042`, `app/templates/folha/_retrato_o_que_fazer.html:73`, `app/colaboradores/views.py:1965`, `app/templates/core/_drawer_generico.html:26`, `app/templates/colaboradores/calendario_partial.html:38`, `app/templates/colaboradores/partials/painel_colaborador.html:183`.)

   Execução desse percurso em navegador: **[nao verificado]**.

5. **F5 — Baixa: residual `decidir` aparece como “Validar no fio”.** A tarefa conserva `acao=None` e depois promove o fio residual para `admin`. Sem ação publicada, a apresentação usa o fallback de `admin`, rotulado “Validar no fio”. Caso: fila `{101: 'decidir'}`, sem ação de instância. O destino continua abrindo o fio, mas a orientação nomeia outra ação. (`app/chamados/services/tarefa_do_dia.py:692`, `app/chamados/services/tarefa_do_dia.py:696`, `app/folha/retrato_folha.py:980`, `app/folha/retrato_folha.py:1055`.)

6. **F6 — Baixa: permaneceram afirmações documentais falsas.**

   - “QUATRO fios” e “baldes QUATRO” contradizem os cinco valores atuais. (`app/folha/retrato_folha.py:164`, `app/folha/fora_da_folha.py:39`, `app/folha/retrato_folha.py:521`, `app/folha/retrato_folha.py:190`.)
   - A promessa de marcar também os dias `nao_sei` não se realiza por data: o template marca apenas `dias_sem_porta`. (`app/folha/fora_da_folha.py:96`, `app/templates/folha/_retrato_o_que_fazer.html:46`.)
   - Os comentários dos crons ainda descrevem leituras de `VIVOS/respondida_em` e `fio_da_linha`; os caminhos atuais consomem a porta da tarefa e `dono_da_linha`. Os papéis declarados continuam coerentes. (`app/config/crons.py:2289`, `app/config/crons.py:2261`, `app/chamados/services/furo_sem_canal.py:98`, `app/folha/vigia_retido_calado.py:55`.)

Os casos acima são demonstrações pelo código e por execução em memória; sua ocorrência em produção é **[nao verificado]**.

Para **A**, a conferência das curas ficou assim:

| PROCEDE | Cura no sítio-fonte e mordida |
|---|---|
| **2/7 — LU-01** | **Real.** A escada contém `decidir → admin`; a publicação residual a consulta preservando rota/fila. O esperado alterado possui lei explícita. Mutante removendo a entrada morre. Resta F5 na apresentação. (`app/docs/CORTES.md:433`, `app/chamados/juizes.py:1653`, `app/chamados/services/tarefa_do_dia.py:696`, `app/chamados/tests/e3_r3_c11_mutantes.py:19`.) |
| **1a — data do Resolver dia** | **Real.** O dia atravessa projeção, Folha e href; o calendário consome ano/mês. Os quatro mutantes retirando os transportes e a querystring morrem. A integração da casca tem F4. (`app/chamados/services/tarefa_do_dia.py:714`, `app/folha/fora_da_folha.py:212`, `app/folha/retrato_folha.py:1047`, `app/colaboradores/services/calendario.py:194`, `app/chamados/tests/e3_r3_c11_mutantes.py:21`.) |
| **1b — marca dos dias sem porta** | **Real.** O template lê o campo; o mutante retirando a condição morre. (`app/templates/folha/_retrato_o_que_fazer.html:46`, `app/chamados/tests/e3_r3_c11_mutantes.py:29`.) |
| **3 — recorte das pautas** | **Parcial.** O filtro protege empresa/competência externa e `empresas=[]` não expande para a frota; ambos possuem mordida. Permanece F1. (`app/ponto/services/pautas_dos_discordantes.py:73`, `app/ponto/services/pautas_dos_discordantes.py:190`, `app/chamados/tests/e3_r3_c11_mutantes.py:31`.) |
| **4 — relógio** | **Parcial.** `censo` exige instante compatível com `hoje`; o reatador transmite o mesmo instante às duas consultas. Esses mutantes morrem. A transmissão até a tela permanece incompleta: F3. (`app/chamados/services/vigia_fio.py:273`, `app/chamados/services/vigia_fio.py:283`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:59`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:113`, `app/chamados/tests/e3_r3_c11_mutantes.py:37`.) |
| **5 — alarme sem leitura** | **Parcial.** O contador entrou em `ALARMA_EM_0` e retirar essa entrada mata o teste. Permanece F2 na formação do total. (`app/folha/management/commands/vigiar_retido_calado.py:17`, `app/chamados/tests/e3_r3_c11_mutantes.py:50`.) |
| **8 — cor** | **Implementada na paleta.** `neutro/#374151` é distinto dos outros sete tokens; fundo/borda são `#f9fafb/#e5e7eb`. Mutantes de colisão e alteração do hex morrem. O `neutro` de `STATUS_DO_APP` pertence a outro vocabulário, sem compartilhar esse lookup de cor. (`app/folha/retrato_folha.py:315`, `app/folha/retrato_folha.py:323`, `app/templates/core/_barra_gestao.html:67`, `app/ponto/status_dia.py:84`, `app/chamados/tests/e3_r3_c11_mutantes.py:53`.) |
| **9 — docstrings e editor da ausência** | **Os sítios apontados foram alterados.** O link solicita `somente_decisao=1`, a view desliga o editor dependente de `salvarAusencia`, e os dois mutantes morrem. Os testes documentais verificam texto; não demonstram comportamento por si. Restam F6. (`app/folha/retrato_folha.py:1051`, `app/ponto/views.py:2090`, `app/templates/ponto/partials/painel_ausencia.html:145`, `app/chamados/tests/test_e3_r3_c11_sem_banco.py:299`, `app/chamados/tests/e3_r3_c11_mutantes.py:63`.) |

Não encontrei cura feita **somente no teste** nesses itens; encontrei curas incompletas nos pontos 3, 4 e 5, conforme F1–F3 e os sítios acima. [nao verificado]

Para **B**:

- **6 — descarte sustentado.** A R3 reancora um pendente, sem acrescentar autoridade ao registro. As frases de nascimento de C19/C20 constam no dado fornecido da principal, em `app/docs/CORTES.md:418` e `app/docs/CORTES.md:419`; execução da trava no pouso: **[nao verificado]**. (`app/core/juizes.py:153`, `app/core/juizes.py:160`, `app/core/juizes.py:878`.)
- **3b — descarte sustentado quanto às entidades da obra.** `get_or_create` pode escrever `User`, não Chamado/Pauta/CelulaDia/DiaPago/FechamentoMensal. A existência garantida do usuário na frota é **[nao verificado]**; a docstring que promete `DoesNotExist` contradiz a implementação. (`app/core/utils.py:15`, `app/core/utils.py:18`.)
- **10 — descarte sustentado como obrigação anterior ao pouso.** A medida M1 está exigida antes do pouso; não há prova fornecida de seu cumprimento no hash final nem de pouso prematuro: **[nao verificado]**. (`app/chamados/tests/e3_r3_c6_entrega.txt:86`, `app/chamados/tests/e3_r3_c6_entrega.txt:94`.)

No **C1**, o rastreio dos campos e valores relevantes do diff, incluindo fontes e consumidores externos, é:

| Campos/valores | Quem escreve ou publica | Quem lê; significado |
|---|---|---|
| Identidade da pergunta, `data_evento`, motivo, instância | `app/chamados/models.py:2903`; `app/chamados/services/fio.py:829` | `app/chamados/services/tarefa_do_dia.py:242`, `app/chamados/services/tarefa_do_dia.py:404`: **dia do turno**, não instante nem dia civil da resposta. |
| Decisão, `ato_em`, texto, absorção | `app/chamados/models.py:4086` | `app/chamados/services/tarefa_do_dia.py:419`, `app/chamados/services/tarefa_do_dia.py:140`: última decisão com `ato_em <= agora`. |
| Ata, veredito, protestos, status do dia | Porta `app/ponto/portas/celula.py:1032` | Leitor `app/escala/services/leitor_celula.py:350`; tarefa `app/chamados/services/tarefa_do_dia.py:188`; censos `app/chamados/services/vigia_fio.py:332`. |
| Lâmpada `veredito_lavrado` e vigência | `app/chamados/veredito_lavrado.py:254` | `app/chamados/services/tarefa_do_dia.py:431`: leitura com `esta_stale`, sem novo julgamento. |
| Efeito da batida, `pergunta_origem`, `criado_em` | Campos do modelo em `app/ponto/models.py:40`, `app/ponto/models.py:79` | `app/chamados/services/tarefa_do_dia.py:440`: universo apurável, existência limitada por `criado_em <= agora`. |
| Ausência, status e id publicado | `app/ponto/services/ausencia.py:179`, `app/ponto/services/ausencia.py:872` | `app/chamados/services/tarefa_do_dia.py:176`, `app/chamados/services/tarefa_do_dia.py:307`; id vira destino em `app/folha/retrato_folha.py:1038`. |
| Chamado, estado, âncora, fila e verbo | Estado pela porta `app/chamados/models.py:447`; âncora por `app/chamados/services/fio.py:97`; alimentação `app/chamados/juizes.py:1562` | Tarefa `app/chamados/services/tarefa_do_dia.py:444`, `app/chamados/services/tarefa_do_dia.py:625`; terminais vêm do catálogo, não de lista nova. |
| `agora`, `hoje`, competência, `[ini,fim]` | Bordas `app/chamados/management/commands/vigiar_fio_morto.py:171`, `app/folha/management/commands/lavrar_retrato_folha.py:49`; janelas consultadas em `app/chamados/services/furo_sem_canal.py:66` | Carga inclusiva em `app/chamados/services/tarefa_do_dia.py:570`; decisão por instante e apresentação por dia local. Divergência: F3. |
| Oferta `resolver_dia_oferecido`, inclusive `None` | `app/colaboradores/services/calendario.py:799`, plural `app/colaboradores/services/calendario.py:125` | `app/chamados/services/tarefa_do_dia.py:611`, `app/chamados/services/tarefa_do_dia.py:632`: desconhecimento preservado em `nao_sei`. |
| `rota`, `fila`, `verbos`, `fio`, `acao`, `ausencia`, `dia_do_turno` | `app/chamados/services/tarefa_do_dia.py:698`, projeção `app/chamados/services/tarefa_do_dia.py:702` | Folha `app/folha/fora_da_folha.py:71`; furo `app/chamados/services/furo_sem_canal.py:98`; Vigia `app/chamados/services/vigia_fio.py:594`; beco `app/escala/services/cadastro_realidade.py:472`; pauta `app/ponto/services/pautas_dos_discordantes.py:85`. |
| Grupo, ações, datas, `dias_sem_porta`, `dias_sem_leitura` | `app/folha/fora_da_folha.py:202` | Copiados ao retrato em `app/folha/retrato_folha.py:1230`; marca no template `app/templates/folha/_retrato_o_que_fazer.html:46`; contagem em `app/folha/vigia_retido_calado.py:57`. |
| JSON do retrato: status, motivo, dono, tipo, saldo, dias, datas, ações, informativo | Montagem `app/folha/retrato_folha.py:1223`; persistência `app/folha/management/commands/lavrar_retrato_folha.py:78` | Tela `app/folha/views_retrato.py:196`, drawer `app/colaboradores/views.py:984`, Vigia `app/folha/vigia_retido_calado.py:53`. `dono` gravado contém **fio**; o balde visual vem de `dono_da_linha`. (`app/folha/retrato_folha.py:435`.) |
| Contadores de furo, rotas e `nao_sei` | `app/chamados/services/furo_sem_canal.py:108` | Command `app/chamados/management/commands/lavrar_furo_sem_canal.py:34`; snapshot em `app/chamados/management/commands/lavrar_furo_sem_canal.py:50`. |
| Contadores do Vigia e linha de nove posições | `app/chamados/services/vigia_fio.py:311`, `app/chamados/services/vigia_fio.py:521`; formato em `app/chamados/services/vigia_fio.py:494` | Command e carimbo `app/chamados/management/commands/vigiar_fio_morto.py:180`. `alcance_nao_sei` conta dias independentemente da rota. (`app/chamados/services/vigia_fio.py:599`.) |
| `calados`, `roteados`, `dias_sem_leitura_sistema` | `app/folha/vigia_retido_calado.py:49` | Alarme `app/folha/management/commands/vigiar_retido_calado.py:37`: os dois primeiros contam pessoa por retrato/competência; o último conta dia-colab. Problema de agregação: F2. (`app/folha/vigia_retido_calado.py:7`.) |
| Pauta: texto/chave, âncora, destinatário, `feito_em`; recorte e retenções transitórias | Seleção `app/ponto/services/pautas_dos_discordantes.py:147`; escrita `app/pautas/services.py:153`; fechamento `app/pautas/services.py:256` | Sincronização `app/ponto/services/pautas_dos_discordantes.py:189`; âncora significa `empresa:AAAA-MM`, conforme `app/pautas/models.py:84`. Problema: F1. |
| Beco: M1, `pai_limbo`, perguntas, data e detalhe | `app/escala/services/cadastro_realidade.py:502`, `app/escala/services/cadastro_realidade.py:515` | Reatador `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:63`: lista vazia de perguntas não provoca reata nem criação de pai. |
| Hrefs, `somente_decisao`, `pode_editar`; token, hex, fundo e borda | `app/folha/retrato_folha.py:1033`, `app/folha/retrato_folha.py:315` | View `app/ponto/views.py:2090`; templates `app/templates/ponto/partials/painel_ausencia.html:145`, `app/templates/core/_barra_gestao.html:67`. Problemas de apresentação: F4/F5. |

Os demais itens de **C**:

- **C2 — nenhum segundo juiz novo encontrado.** Os leitores principais consomem a projeção; `TERMINAIS` vem do catálogo. `dono_fora_do_fio` subdivide somente dias sem rota, e C17 permanece no instrumento de sombra. (`app/chamados/services/tarefa_do_dia.py:447`, `app/chamados/services/vigia_fio.py:601`, `app/chamados/services/vigia_fio.py:615`, `app/core/juizes.py:136`.)
- **C3 — nenhum segundo escritor novo dessas cinco entidades encontrado; há F1 na decisão de fechar Pauta.** Pautas usam os serviços existentes; reata usa a porta do fio; os commands de pauta, reata e snapshot exigem `--apply`. Não apareceu escrita nova de CelulaDia, DiaPago ou FechamentoMensal no diff fornecido. (`app/ponto/services/pautas_dos_discordantes.py:201`, `app/chamados/services/limbo_folgas.py:245`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:90`, `app/chamados/management/commands/lavrar_furo_sem_canal.py:42`.)
- **C4 — F3.** A injeção funciona no caminho testado, mas os leitores reais ainda recorrem ao relógio interno. (`app/chamados/tests/test_e3_r3_c11_sem_banco.py:239`, `app/folha/retrato_folha.py:460`.)
- **C5 — papéis coerentes; comentários envelhecidos em F6.** Snapshot é `lavra`; os dois comandos de contagem são `vigia`. (`app/config/crons.py:2291`, `app/config/crons.py:2236`, `app/config/crons.py:2262`.)
- **C6 — nenhum registro novo na R3.** A alteração é reancoragem do pendente TT3; C19/C20 são os registros anteriores. (`app/core/juizes.py:878`, `app/core/juizes.py:153`, `app/core/juizes.py:160`.)
- **C7 — não encontrei enfraquecimento na troca residual ou em `3272→3273`.** C12 testa preservação de rota/fila/ação; a mudança de dono tem LU-01. A linha real está em 3273, a igualdade da catraca permanece e o mutante voltando a 3272 morre. Comparação histórica integral dos corpos B: **[nao verificado]**. (`app/chamados/tests/test_e3_r3_c12_sem_banco.py:12`, `app/docs/CORTES.md:433`, `app/ponto/views.py:3273`, `app/core/tests/test_medida_turno_do_par.py:553`, `app/chamados/tests/e3_r3_c12_mutantes.py:32`.)
- **C8 — F4/F5.** Os números da barra são lidos de `it.numero`; a nova marca lê o campo gravado, sem contar dias no template. Permanecem textos técnicos “Datas (ISO)” e “Sem porta”. Renderização nas duas cascas e estilo computado: **[nao verificado]**. (`app/templates/core/_barra_gestao.html:67`, `app/templates/folha/_retrato_o_que_fazer.html:43`, `app/templates/folha/_retrato_o_que_fazer.html:46`.)
- **C9 — F6.** As correções documentais apontadas na triagem existem, mas não eliminaram as contradições enumeradas. (`app/chamados/services/furo_sem_canal.py:19`, `app/chamados/management/commands/lavrar_furo_sem_canal.py:6`, `app/folha/retrato_folha.py:521`.)
- **C10 — nenhuma inversão nova de trava encontrada.** O reatador grava a reversão antes da escrita, trava Colaborador antes de Chamado e relê a fila sob a trava. Cumprimento da medida M1 final e ordem efetiva do pouso: **[nao verificado]**. (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:99`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:112`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:113`, `app/chamados/tests/e3_r3_c6_entrega.txt:94`.)

Reexecutei os **314 testes locais**, os **23 mutantes c11** e os **9 mutantes c12**: controles verdes e mortes por asserção. Isso sustenta as curas específicas, sem cobrir os casos F1–F4. Suite integral, ORM e renderização permanecem **[nao verificado]**. (`app/chamados/tests/e3_r3_c12_verificar.py:68`, `app/chamados/tests/e3_r3_c11_mutantes.py:70`, `app/chamados/tests/e3_r3_c12_mutantes.py:84`, `app/chamados/tests/e3_r3_c12_entrega.txt:34`.)

**VEREDITO: REPROVA.**

## 15/11 15:34 · sessao 778c10d24aae · TELA-DIZ-O-QUE-O-SERVIDOR-DISSE · verificador · origem=Code

Conto as linhas dos anexos dentro de cada bloco, incluindo vazias, como na revisão anterior (`revisao_anterior.r:5`).

1. **NAO PROCEDE — não identifiquei condição inexequível no literal.**

   Entre os construtores identificados no dado, **F1 + C seriam dois**: C é opus, e a trava de um Codex construtor é específica do Codex. BUG-PAR está registrada esperando R3; se sua volta abrir antes, C espera entregar. A conferência efetiva do teto na abertura permanece **[nao verificado]** (`CLAUDE.md:291-292,353-356`; `queima_util_1530.txt:2,6`; `relato_topo_1530.txt:3-5`).

   Cópia nova do HEAD e deploy de UI com `--sem-migrate` são procedimentos previstos. A flag recusa migrations pendentes; ausência concreta de pendências **[nao verificado]** (`CLAUDE.md:39-40,309-313`; `bin/deploy.sh:205-219`).

   As duas cascas podem ser exercitadas pelos testes existentes. RED/verde de C e smoke humano cumpridos **[nao verificado]** (`app/ponto/tests/test_l030_templates_renderizados.py:15-30`; `queima_util_1530.txt:6`).

2. **NAO PROCEDE — os “senão” estão definidos.**

   Sem vaga, não abre; BUG-PAR aberta tem precedência; cota acabando determina commit parcial e RELATO. Status desconhecido, ausente ou JSON ilegível têm mensagem definida; sítios fora de `templates/` vão ao BACKLOG. Condição literal falsa encontra também o PAROU geral (`queima_util_1530.txt:6-8`; `reemissao_1215.txt:4-5`; `CLAUDE.md:37-38`). [nao verificado]

3. **NAO PROCEDE quanto a colisão demonstrada; cruzamento integral dos diffs [nao verificado].**

   C levanta nominalmente a proibição que motivou a devolução anterior de A. A exceção agora alcança construção dessa obra (`queima_util_1530.txt:6`; `revisao_anterior.r:29-33`).

   A v2 continua **classe A nominal**, com escada inteira: “tela” não dispensa preverificação nem suite inteira verde antes do opus final. O novo literal não revoga essas obrigações (`app/docs/CORTES.md:409,411,413`; `CLAUDE.md:372-381`).

   C exige expressamente smoke nas **DUAS cascas**, preservando BUG 73. iPhone e Android, sozinhos, não comprovam as árvores `base.html` e `base_app.html`; cliques efetivos **[nao verificado]** (`queima_util_1530.txt:6`; `CLAUDE.md:202-203`; `app/docs/LEIS.md:98-99`).

   **Não certifico ausência de arquivo comum** entre C e R3/F1/BUG-PAR: os três diffs `main...obra`, especialmente F1, permanecem **[nao verificado]**. O cruzamento registrado entre BUG-PAR e R3/F1 identifica outros arquivos e não resolve essa pergunta sobre C (`relato_topo_1530.txt:3`). A regra exige conferir arquivo comum, inclusive testes (`CLAUDE.md:309-313`; `complemento2_queima.txt:4`). [nao verificado]

4. **NAO PROCEDE quanto às referências materiais conferidas; referências sem comprovação seguem [nao verificado].**

   **Sim:** no main, `r.ok` chama diretamente `v2Sucesso(tipo,false)`, agora na **linha 348**. JSON só é lido depois, no ramo não 2xx. O êxito está em `:155`, e o retorno ao dashboard em 3 segundos em `:157` (`app/templates/ponto/bater_ponto_v2.html:150-157,343-350`).

   Os dois handlers devolvem os três status em `Response(...)` sem código HTTP alternativo:

   | Status 200 | `app/api/views_core.py` | `app/api/views.py` |
   |---|---|---|
   | `ok` | `:807-818` | `:813-824` |
   | `ja_registrada` | `:521-531` | `:535-545` |
   | `ignorada_intervalo` | `:673-683` | `:682-692` |

   **Não encontrei quarto valor de status 2xx.** Existe outro caminho de retorno 200 por reutilização, mas ele repete `ja_registrada` (`app/api/views_core.py:684-686`; `app/api/views.py:693-695`; `app/ponto/services/resposta_batida.py:14-31`).

   O sincronizador está em **`app/static/pwa.js`**: faz esse POST, lê JSON, mas não consulta `data.status`; remove a pendência por `resp.ok`. Seu indicador é agregado, sem anúncio individual (`app/static/pwa.js:153-165,182-201`).

   No censo desta cópia, **N=1 consumidor direto em `templates/`: a própria v2; zero outros encontrados**. A tela antiga recebe HTML de sua própria rota, distinta dessa API (`app/templates/ponto/bater_ponto_v2.html:343-350`; `app/templates/ponto/bater_ponto.html:533-539`; `app/ponto/urls.py:11-12`; `app/api/urls.py:67`).

   Existência física dos arquivos de logs citados pelos avais, diffs completos das obras e cumprimento dos portões operacionais permanecem **[nao verificado]**. Portanto, estes pareceres não certificam que **ABRE** já esteja satisfeito (`queima_util_1530.txt:3,6`; `CLAUDE.md:37-38`).

## 15/11 15:34 · sessao 70c4e6b3dc35 · - · - · origem=Ronald (tmux)

Achado confirmado, gravidade baixa: **residual `decidir` é apresentado como “Validar no fio”**. `_portas_da_tarefa` calcula `acao=None` antes de mudar o fio residual para `admin` (`app/chamados/services/tarefa_do_dia.py:692-699`); `portas_da_linha` usa então o fallback `PORTA_DO_FIO['admin']`, rotulado “Validar no fio” (`app/folha/retrato_folha.py:980`, `:1055-1064`). Caso sintético reproduzido: fila `{101: 'decidir'}` sem ação exigível publica fio `admin`, ação `None`, e esse botão. O destino ainda abre o fio; problema de orientação da ação.

**LU-01 está curada na produção**, não apenas no teste: a escada contém `('decidir', 'admin')` e a publicação residual a consulta (`app/chamados/juizes.py:1653-1669`; `app/chamados/services/tarefa_do_dia.py:696-699`). Mutante removendo essa entrada morreu por asserção; fonte do mutante e expectativa: `app/chamados/tests/e3_r3_c11_mutantes.py:19-20`, `app/chamados/tests/test_e3_r3_c11_sem_banco.py:263-268`.

**Dia no link também possui mordida real**: executei os quatro mutantes retirando a data da projeção, Folha, haiku e href; todos morreram com controles verdes. Sítios: `app/chamados/tests/e3_r3_c11_mutantes.py:21-28`; asserções: `app/chamados/tests/test_e3_r3_c11_sem_banco.py:27-56`.

**C12 preserva explicitamente rota/fila e ação**, e a mudança de esperado residual possui referência à LU-01: `app/chamados/tests/test_e3_r3_c12_sem_banco.py:12-34`; classificação A/B em `app/chamados/tests/e3_r3_c12_classificacao.txt:8-46`. Executei os dois módulos puros `test_e3_r3_c12_sem_banco` e `test_e3_degrau_dono_e_porta`: 77 testes passaram. Execução Django/ORM continua [nao verificado].

Mapa principal:

| Campo | Fonte / escritor | Leitura/publicação |
|---|---|---|
| `data_evento`, identidade | `app/chamados/models.py:2903`; `app/chamados/services/fio.py:829` | Dia do turno em `app/chamados/services/tarefa_do_dia.py:242-263`, `:404-410` |
| Decisão, `ato_em`, texto, absorção | `app/chamados/models.py:4014-4027`, `:4086` | Corte `<=agora`: `app/chamados/services/tarefa_do_dia.py:419-424`; julgamento `:140-185` |
| `veredito_lavrado` | `app/chamados/veredito_lavrado.py:254-276` | Vigência por `esta_stale`: `app/chamados/services/tarefa_do_dia.py:431-434` |
| Âncora / terminal | `app/chamados/services/fio.py:97-117`; catálogo `app/chamados/catalogo/motor.py:24-31` | `app/chamados/services/tarefa_do_dia.py:444-451` |
| Fila e verbo | `app/chamados/juizes.py:1562-1592` lê painel/partição | Publicação em `app/chamados/services/tarefa_do_dia.py:625-699` |
| Oferta Resolver dia | `app/colaboradores/services/calendario.py:126-176` | `app/chamados/services/tarefa_do_dia.py:603-619`; data projetada `:712-715` |

Leitores da publicação: `app/folha/fora_da_folha.py:78-89`, `:199-231`, `:264-283`; `app/chamados/services/furo_sem_canal.py:94-105`; `app/chamados/services/vigia_fio.py:357-374`, `:594-605`; `app/escala/services/cadastro_realidade.py:468-474`; `app/ponto/services/pautas_dos_discordantes.py:84-98`.

Não encontrei novo escritor persistente nem nova lista literal de status nos dois arquivos examinados: a carga usa `TERMINAIS` do catálogo e publica dicionários em memória (`app/chamados/services/tarefa_do_dia.py:447-451`, `:698-699`). Relógio entra por `agora`, validado na porta (`app/chamados/services/tarefa_do_dia.py:723-733`).

Candidato temporal reproduzido, **não recomendado como regressão da R3**: uma resposta posterior a `agora` já exclui a pergunta dos pendentes porque a partição lê `respondida_em` atual (`app/chamados/services/pergunta_no_app.py:76-85`), antes do C6 (`app/chamados/services/tarefa_do_dia.py:527-539`). Reproduzi fio `nada` para uma ação RESPONDER histórica. Esses trechos não aparecem alterados no diff fornecido; atribuição à obra [nao verificado].

_5701 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
