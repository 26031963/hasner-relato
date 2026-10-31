# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 02:30** · publicado em **31/10/2026 02:32** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 02:30 · sessao 54ab8bc9f7f1 · - · - · origem=Ronald (tmux)

Resultados conferidos por leitura e probes Python `-B` em memória; não executei o selo, que cria temporários e cópias (`bin/tests/test_disjuntor.sh:37-49`, `:612-621`).

1. **B foi ampliado, mas permanece incompleto.** Os prompts novos do selo são reconhecidos (`bin/tests/test_disjuntor.sh:345-351`), porém:
   - `Revisão\nFINAL da obra **OBRA-X**` e `FINAL revisão da obra OBRA-X` dão `_eh_final=False`: a expressão não atravessa quebra de linha e só admite `final` antes de `review`, não antes de `revisão` (`bin/disjuntor.py:88`, `:463-465`). `_veredito_e_obra` retorna `(None,None)` e esses agentes não entram nem em `b3_sem_obra` (`:495-497`, `:520-523`).
   - `LEITURA FINAL da obra obra-x` dá obra `None`: os padrões usam `re.I`, mas o `fullmatch` posterior não usa (`bin/disjuntor.py:84-85`, `:473-474`). Entra no contador sem obra, sem incrementar a obra pretendida (`:497`, `:520-523`).
   - `LEITURA FINAL da obra O261 (C)` produz `O261 (C)`; `LEITURA FINAL da obra **O261** (C)` produz `O261`. A cauda exige whitespace imediatamente depois do ID; markdown separa as chaves (`bin/disjuntor.py:83-85`, `:481-482`).
   - `obra ABC` era aceito pela expressão anterior fornecida no diff; agora exige hífen ou letras+algarismos (`bin/disjuntor.py:81`). Uso desses formatos em produção: [não verificado].

2. **Testes por ausência de sinal.** `_passa` exige stdout vazio e nenhuma `FALHA` nova; não exige comprovação de que a invocação foi reconhecida (`bin/tests/test_disjuntor.sh:91-94`).
   - B3 caso14 passa também se aquele prompt final não for reconhecido; caso15 fornece a contraparte positiva (`:349-351`). Caso16 exige numericamente um agente sem obra (`:353-356`).
   - Codex25 passa também com parser retornando `[]`; confirmei isso aplicando somente o mutante `A-sem-continuacao` em memória (`:488`, `:725`). O mesmo mutante também retorna `[]` para o comando26, mas **26 o mata** porque exige `ANEXO-ENXUTO` (`:489`). Codex29–32 também verificam autorização por ausência; 24,26–28 verificam negativas concretas (`:487-495`).
   - Trilha21,23,25,28 são verificações de ausência (`:544`, `:554`, `:556`, `:561`). Há contrapartes positivas: 2/3 exigem linhas com formato completo;22/27 exigem negativas;24 executa o checador;26 exige ALVO;29 exige a falha específica da trava (`:542-562`). Particularmente25 aceita qualquer saída do Stop sem o nome do checador, inclusive vazio por outra falha (`:556`).

3. **Mutantes novos.**
   - `C-alvo-cru` troca a ferramenta pelo alvo, alterando simultaneamente formato e privacidade (`bin/tests/test_disjuntor.sh:666`; `bin/disjuntor.py:189`). Já cai no caso2, que exige `Agent general-purpose`; não isola as verificações de vazamento/ATOS posteriores (`bin/tests/test_disjuntor.sh:542`, `:544`, `:553-557`).
   - `C-sem-trava-de-saida` tem isolamento melhor: ferramenta `FEITO` deve manter a negativa CHAVE, não acrescentar linha e produzir o `ValueError` específico (`bin/tests/test_disjuntor.sh:560-562`, `:667`).
   - Não encontrei, por inspeção, mutante novo necessariamente sobrevivente; execução integral dos mutantes: [não verificado].

4. **Escritores/leitores B3.**
   - `b3_sem_obra`: produtor lógico `_veredito_e_obra`, que emite `''` (`bin/disjuntor.py:497`); coleta em `varrer_b3` (`:520-521`); escritor da lista `_fn` (`:527-530`); persistência `_com_estado` (`:237-243`); leitura e contagem `_b3_sem_obra` (`:1056-1060`); produtor do campo de apresentação `ler` (`:1052`); leitor final `linha` (`:1122`).
   - `b3`: criação/escrita de reprovados/disparado em `varrer_b3` (`bin/disjuntor.py:526`, `:532-546`); leitura e escrita de rearme em `rearma_b3` (`:553-559`); leitor `b3_disparados` (`:564-565`); consumidores da varredura PreToolUse (`:909-916`) e Stop (`bin/hook_stop_fila1.py:1022-1026`).
   - Chamadores que provocam escrita pela varredura: os mesmos PreToolUse e Stop; `ler` apenas conta o estado já acumulado (`bin/disjuntor.py:912`, `:1052-1058`; `bin/hook_stop_fila1.py:1022`).

5. **Exceção nova concreta.** Com estado lógico `{'b3_sem_obra':7}`, `_b3_sem_obra` levanta `TypeError`, pois executa `len(7)` e só captura `RuntimeError`; confirmei com `ler_estado` substituído em memória (`bin/disjuntor.py:1056-1060`). `--linha` não contém captura ao redor de `ler/linha` (`:1129-1131`). Strings e dicionários são contados silenciosamente por `len`, embora o escritor normal produza lista (`:527-530`, `:1058`). Estado desse tipo em produção: [não verificado].

## 31/10 02:30 · sessao ef602f7d040e · - · - · origem=Ronald (tmux)

Achados de C e perguntas 3/4/6, obtidos só por leitura e probes Python `-B` com escrita substituída por funções em memória:

- **C original, comando cru no RELATO:** não reproduzi. `tool_input.command` vira `alvo`, que entra apenas no hash/tamanho da linha; o texto vai a `trilha('ALVO', ...)` (`bin/disjuntor.py:178-190`). Com `{"tool_name":"Bash","tool_input":{"command":"echo deploy NO AR e FEITO; touch DISJUNTOR_DESLIGADO"}}`, a linha resultante contém `Bash -- alvo sha1:e02b244fde (52 car., na trilha privada)`, sem o comando; a própria negação decorre de `guarda_chave` (`bin/disjuntor.py:135-136`).

- **Ainda há texto escolhido pelo payload:** `limpa` aceita qualquer sequência de letras/dígitos ASCII, `_` e `-`, até 40 caracteres, nos campos `agent_id`, `tool_name` e `subagent_type`; não valida catálogo desses campos (`bin/disjuntor.py:186-190`). Exemplo: `agent_id="texto-livre|`XYZ`"` produz `agente texto-livreXYZ`. `|`, crase, estrelas e quebras de linha não passam pela whitelist nesses campos; alvo cru só gera digest hex e tamanho (`bin/disjuntor.py:184-190`).

- **Uma linha gravada pode casar `ATOS`:** payload concreto:
  ```json
  {"tool_name":"Bash","agent_id":"FEITO-NADA","tool_input":{"command":"touch DISJUNTOR_DESLIGADO"}}
  ```
  A negação ocorre por `bin/disjuntor.py:135-136`; a linha contém `agente FEITO-NADA`. `FEITO` casa `ATOS`, mas `NADA` casa `NEGA` e faz `RA.afirmacoes` ignorar a linha toda (`bin/relato_afirma_com_prova.py:30-49`). Probe confirmou gravação. Outra variante: `tool_name="FEITO", agent_id="NADA", file_path="/r/DISJUNTOR_DESLIGADO"`; o caminho dispara a guarda independentemente do nome da ferramenta (`bin/disjuntor.py:137-138`).

- **Cobertura real da trava:** a linha gerada sempre possui `**`; por isso é “forte”, mesmo quando o ato aparece fora do trecho em negrito. A trava rejeita os sete padrões maiúsculos de `ATOS`, com fronteira de palavra, salvo presença de negação em qualquer lugar da linha. Também aceita prosa sem `**`/título, outras caixas (`feito`), palavras fora de `ATOS`, formas embutidas em identificador (`SEM_EFEITO`), e linhas com `PROVA:` na própria linha ou nas seis seguintes (`bin/relato_afirma_com_prova.py:30-50`). Para a chamada nova `RA.afirmacoes(ln)`, só existe uma linha, sem vizinhas; `PROVA:` não é gerável pelos campos limpos (`bin/disjuntor.py:186-195`).

- **Payload `FEITO` sem negação:** `{"tool_name":"FEITO","tool_input":{"file_path":"/r/DISJUNTOR_DESLIGADO"}}` provoca `ValueError` na trava; não entra no RELATO, mas deixa `ALVO` e `FALHA relato_negacao`. Probe confirmou. A exceção é capturada e a negativa já foi emitida antes de tentar o relato (`bin/disjuntor.py:194-204`, `bin/disjuntor.py:947-950`).

**Escritores/leitores pedidos:**

| Valor | Escritor | Leitor |
|---|---|---|
| Evento `ALVO` | Preparado em `bin/disjuntor.py:185`; append físico em `bin/disjuntor.py:103-109` | Leitor explícito localizado: teste `bin/tests/test_disjuntor.sh:557`. `ler` lê o arquivo, mas filtra e descarta `ALVO`, pois só percorre os eventos de `bin/disjuntor.py:998-1005`; consumidor funcional de `ALVO` nos cinco arquivos: não encontrei. |
| Linha do RELATO | Construída em `bin/disjuntor.py:187-190`, validada em `:194`, delegada em `:199-201`; escritor físico `app/core/esteira_vigia.py:859-884` | RA lê o arquivo em `bin/relato_afirma_com_prova.py:86-96`; Stop invoca o checador em `bin/hook_stop_fila1.py:1148-1155`, bloqueia por retorno não limpo em `:1172-1177`; testes em `bin/tests/test_disjuntor.sh:542-562`. O escritor também lê o RELATO anterior em `app/core/esteira_vigia.py:860` e `:104-107`. |
| `d['b3_sem_obra']` de `linha` | `_b3_sem_obra` calcula em `bin/disjuntor.py:1056-1060`; `ler` atribui no dicionário em `:1052` | `linha` formata em `bin/disjuntor.py:1122`; chamada de apresentação `:1130`; teste `bin/tests/test_disjuntor.sh:355`. |

**Efeitos colaterais e exceções:**

- Novo `sys.path.insert(0, ...)` permanece no processo e acrescenta mais uma entrada a cada `relato_negacao`; probe confirmou crescimento de um elemento por chamada (`bin/disjuntor.py:192-193`). O import de RA não chama `main` nem escreve a base: `main` está protegido por `if __name__ == '__main__'` (`bin/relato_afirma_com_prova.py:123-124`); a escrita da base ocorre só dentro de `main`, em `:100-110`. Eventual criação de bytecode pelo interpretador em execução normal: [não verificado].
- `ALVO` acrescenta uma escrita para cada negativa antes da validação do RELATO (`bin/disjuntor.py:185-195`). A trilha normaliza espaços e limita **todo o detalhe a 300 caracteres**, logo não guarda o alvo inteiro apesar de o SHA1 e o tamanho serem calculados sobre ele (`bin/disjuntor.py:108-109`, `:184-190`).
- A designação “privada” não impõe proteção própria: a trilha usa `_abre` (`bin/disjuntor.py:107`), que delega a `orcamento_contexto.abre` (`:97-100`); esta pede modo `0o644` na criação (`bin/orcamento_contexto.py:73-74`). Permissões/umask efetivos de produção: [não verificado].
- Falha de `trilha('ALVO')` é engolida e não impede a linha de dizer “na trilha privada” (`bin/disjuntor.py:103-111`, `:185-190`). Não há confirmação de persistência nem referência garantida ao texto.
- **Exceção nova não tratada em `--linha`:** estado JSON válido `{"b3_sem_obra":1}` passa pela validação que só exige dicionário (`bin/disjuntor.py:264-266`), mas produz `TypeError: object of type 'int' has no len()` em `_b3_sem_obra`; só `RuntimeError` é capturado (`:1056-1060`). O erro sobe por `ler` e por `main --linha`, que não contém `try` (`:1052`, `:1129-1131`). Probe em memória confirmou.
- As exceções comuns introduzidas no caminho de `relato_negacao` são contidas pelo `except Exception`, gerando falha de registro; a negação da ferramenta continua emitida (`bin/disjuntor.py:203-204`, `:947-952`).

## 31/10 02:30 · sessao 3c700e287ce2 · - · - · origem=Ronald (tmux)

- **A original foi corrigido:** `cd /x && (CODEX_OBRA=X CODEX_PAPEL=verificador bin/codex_pergunta.sh --anexo d.diff h q r)` retorna uma chamada em `_codex_cmds`; parênteses agora separam trechos e o selo exige negativa por orçamento nesse exemplo. `bin/disjuntor.py:754`, `bin/disjuntor.py:794`, `bin/disjuntor.py:817`, `bin/tests/test_disjuntor.sh:487`.

- **Regressão de reconhecimento, com redirecionamento colado:**
  ```bash
  bin/codex_pergunta.sh</dev/null --anexo app/docs/LEIS.md h q r
  ```
  Bash invoca o wrapper com cinco argumentos; `_codex_cmds` retorna `[]`, porque depois de `.sh` agora exige espaço ou fim. Assim não aplica ANEXO-ENXUTO nem B4-CODEX. A regex antiga fornecida aceitava o limite de palavra antes de `<`. `bin/disjuntor.py:751`, `bin/disjuntor.py:841`, `bin/disjuntor.py:845`, `bin/disjuntor.py:849`, `bin/disjuntor.py:869`; regex antiga: `delta.diff` do usuário [não verificado em arquivo local].

- **Outra regressão causada pelo novo separador:**
  ```bash
  cat <<<EOF
  bin/codex_pergunta.sh --anexo app/docs/LEIS.md h q r
  ```
  Bash executa a chamada após o `cat`; `_codex_cmds` retorna `[]`. O primeiro `<` de `<<<` não entra no ramo heredoc, mas a iteração seguinte reconhece `<<EOF` dentro do here-string; como não encontra terminador, salta ao fim e descarta a chamada real. `bin/disjuntor.py:787`, `bin/disjuntor.py:788`, `bin/disjuntor.py:790`, `bin/disjuntor.py:797`, `bin/disjuntor.py:799`, `bin/disjuntor.py:800`.

- **Falso positivo novo — só define uma função:**
  ```bash
  f() { bin/codex_pergunta.sh --anexo app/docs/LEIS.md h q r; }
  ```
  `_codex_cmds` retorna uma chamada com `volta=meio` e o anexo; `codex_decide` nega com ANEXO-ENXUTO, embora Bash apenas defina `f`. Sem `--anexo`, orçamento excedido produz B4-CODEX. O parser divide `()` e elimina `{`, perdendo o contexto de definição. `bin/disjuntor.py:753`, `bin/disjuntor.py:754`, `bin/disjuntor.py:794`, `bin/disjuntor.py:817`, `bin/disjuntor.py:823`, `bin/disjuntor.py:849`, `bin/disjuntor.py:869`.

- **Falso positivo de heredoc:** em `cat <<EOF`, uma linha contendo tabulação + `EOF` ou `EOF ` não encerra o heredoc de Bash, mas o scanner a aceita como terminador. Uma linha posterior do corpo que mencione o wrapper passa a ser interpretada como chamada. `bin/disjuntor.py:799`.

Os exemplos foram conferidos com importação `python3 -B`, orçamento substituído em memória e uma função Bash também em memória no lugar do wrapper; nenhum arquivo foi criado. A suíte completa não foi executada [não verificado]. [nao verificado]

## 31/10 02:29 · sessao 273b3485f29d · O261-E-F5C · verificador · origem=Code

**Bloqueia:** a nova `fila` ainda pode nomear chamado oculto no painel padrão; as frases negam ações que o juiz não exclui; e o drawer mantém “chamado N” literal. (`app/chamados/services/painel.py:188`, `:329`; `app/folha/fora_da_folha.py:36`, `:43`, `:200`; `app/folha/retrato_folha.py:1069`.)

`git show 366e07d4` falhou: a cópia não contém repositório Git. Portanto, diff exato, conteúdo de `dc3ba57f`, RED executado e sequência dos commits: **[nao verificado]**. Os achados abaixo são do código disponível; ocorrência e quantidade em produção: **[nao verificado]**.

1. **Quem escreve e quem lê**

| Valor | Escritor/origem | Leitores |
|---|---|---|
| `porta['fila']` | `alimentar_portas` monta `fila_cel` usando `qs_na_fila`; o juiz seleciona a célula e escreve a tupla ordenada. (`app/chamados/juizes.py:1144`, `:1148`, `:1216`, `:1224`.) | `o_que_segura` lê e interpola os números na frase. (`app/folha/fora_da_folha.py:199`, `:200`, `:202`.) |
| `GRUPOS['C']` | Literal do módulo. (`app/folha/fora_da_folha.py:36`.) | `retrato_folha.ajuda` lê a prosa pelo mapeamento fio→letra; vira `title` e chega ao template da barra. (`app/folha/retrato_folha.py:71`, `:177`, `:196`, `:825`; `app/templates/core/_barra_gestao.html:67`; inclusão em `app/templates/folha/_retrato_barra.html:122`.) `fora_da_folha` também percorre `GRUPOS`, mas somente suas chaves. (`app/folha/fora_da_folha.py:139`.) |
| `DESTRAVA['chamado']` | Literal do módulo. (`app/folha/fora_da_folha.py:43`.) | `o_que_segura` lê e personaliza; `retrato_folha.o_que_fazer` lê literalmente pelo fio gravado. (`app/folha/fora_da_folha.py:197`, `:200`; `app/folha/retrato_folha.py:1058`, `:1069`.) A view fornece o bloco e o template imprime `r.valor`. (`app/colaboradores/views.py:984`; `app/templates/folha/_retrato_o_que_fazer.html:33`, `:37`.) |

Os testes consumidores são: [nao verificado]

- **Dict inteiro do juiz:** expectativas com `fila` nas duas rotas, e igualdade singular/lote. (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:138`, `:145`, `:163`.)
- **Teste novo:** injeta `fila` por mock e verifica a frase personalizada. (`app/folha/tests/test_haiku_fora_da_folha.py:125`, `:127`, `:133`, `:136`, `:139`.)
- **Prosa de `GRUPOS`:** ausência de substring no teste novo; propagação para tooltips; comparação das chaves. (`app/folha/tests/test_haiku_fora_da_folha.py:122`; `app/folha/tests/test_retrato_folha_quadros.py:154`, `:1276`, `:1285`.)
- **`DESTRAVA` literal:** igualdade do drawer, inclusive `chamado`, e restrição aos valores dos mapas. (`app/folha/tests/test_retrato_folha_dono_sao_tres.py:114`, `:119`; `app/folha/tests/test_haiku_fora_da_folha.py:484`; `app/folha/tests/test_retrato_folha_drawer.py:224`, `:229`.) Os testes de `colab` também consomem o mapa. (`app/folha/tests/test_retrato_folha_drawer.py:187`; `app/colaboradores/tests/test_retrato_no_drawer.py:210`.)

O **vigia** consome datas da folha e `verbos`/`rota` do juiz, sem ler a nova `fila` ou essas frases nesses caminhos. (`app/chamados/services/vigia_fio.py:549`, `:550`, `:596`, `:598`, `:601`.) **Pautas** consome o fio por `_alimentar`/`_fio_do_dia`, também sem essas frases. (`app/ponto/services/pautas_dos_discordantes.py:60`, `:74`, `:76`.)

2. **As frases não são verdadeiras para todo `fio='chamado'` — bloqueia**

A garantia efetiva de `fio_pelos_verbos` é: nenhum valor do mapa é `cobrar` ou `validar`. Qualquer outro valor, inclusive `historico` e `None`, resulta em `chamado`. Isso não garante ausência de pergunta pendente nem de resposta aguardando validação. (`app/chamados/juizes.py:1170`, `:1177`, `:1181`.)

**Contraexemplo permitido pelo código, nas duas rotas:** chamado aberto com `silenciado_ate` vencido e pergunta viva pendente.

- `qs_na_fila` admite silêncio vencido: exclui somente `silenciado_ate > agora`. (`app/chamados/services/painel.py:186`, `:188`.)
- A partição substitui o verbo original por `historico` quando o silêncio venceu. Assim, pode encobrir tanto `cobrar` quanto `validar`. (`app/chamados/services/verbos.py:44`, `:45`, `:48`.)
- O app continua selecionando perguntas vivas, sem filtro de silêncio; pergunta viva sem resposta é `PENDENTE`. (`app/chamados/services/pergunta_no_app.py:39`, `:42`, `:44`; `app/chamados/models.py:2207`, `:2208`.)
- Com o chamado ligado à célula, o juiz devolve `porta_fila`; com somente a pergunta ligada à célula e a âncora fora dela, devolve `porta_pergunta`. Em ambos, `historico` permite `fio='chamado'`. (`app/chamados/juizes.py:1216`, `:1218`, `:1220`, `:1224`, `:1225`.)

Logo, “nenhuma pede resposta do COLAB nem validação” pode ser falsa nas duas rotas. (`app/folha/fora_da_folha.py:36`, `:43`; cadeia acima.)

**Também há chamado nomeado invisível no painel padrão.** Na `porta_fila`, esse chamado histórico permanece em `fila`, recebe número na frase, mas o painel padrão exclui IDs com verbo `historico`. A chave nova separa chamados da célula de âncoras externas; não prova visibilidade. (`app/chamados/juizes.py:1224`; `app/folha/fora_da_folha.py:199`, `:201`; `app/chamados/services/painel.py:322`, `:327`, `:329`.)

3. **Drawer versus `o_que_segura`: incoerente — bloqueia**

`o_que_segura` troca o trecho genérico por números reais de `fila` ou somente pela pergunta no app. O drawer entrega o literal completo, incluindo **“chamado N na fila, ou…”**. (`app/folha/fora_da_folha.py:200`, `:202`; `app/folha/retrato_folha.py:1069`; `app/templates/folha/_retrato_o_que_fazer.html:37`.)

O retrato grava o fio em `dono`, mas sua linha não carrega `fila` nem `rota` para realizar essa escolha. (`app/folha/retrato_folha.py:1177`, `:1186`, `:1188`.) Além disso, o contrato já declara “N literal na tela” um bug de rótulo. (`app/docs/CORTES.md:353`.)

Os testes atuais preservam essa incoerência ao exigir igualdade com o literal, inclusive para `chamado`. (`app/folha/tests/test_retrato_folha_dono_sao_tres.py:119`; `app/folha/tests/test_haiku_fora_da_folha.py:484`.)

4. **Checklist das classes**

| Classe | Veredito |
|---|---|
| Segundo juiz duplicando registrado | **Sem bloqueio identificado nos blocos examinados:** porta registrada, escada no juiz e folha lendo seu fio. (`app/core/juizes.py:253`; `app/chamados/juizes.py:1175`; `app/folha/fora_da_folha.py:68`.) |
| Segundo escritor fora da porta | **Sem bloqueio identificado nesses blocos:** montam mapas, texto e retorno, sem escrita persistente. (`app/chamados/juizes.py:1215`, `:1224`; `app/folha/fora_da_folha.py:191`, `:207`.) |
| Relógio fora da borda | Há leituras transitivas em `qs_na_fila` e `particionar_verbos`; introdução ou alteração pelo commit **[nao verificado]**. (`app/chamados/services/painel.py:188`; `app/chamados/services/verbos.py:32`.) |
| Derivação paralela de fonte única | A alimentação consulta as fontes existentes; o defeito comprovado é tratar pertencimento a `qs_na_fila` como garantia de exibição. **Bloqueia pela conclusão indevida**, descrita no item 2. (`app/chamados/juizes.py:1140`, `:1153`; `app/chamados/services/painel.py:329`.) |
| Selo/RED sem mordida | O teste novo mocka o juiz e verifica apenas `GRUPOS['C']` e a saída de `o_que_segura`; deixa passar o drawer e não exercita silêncio vencido. **Insuficiente como selo dessas garantias.** (`app/folha/tests/test_haiku_fora_da_folha.py:121`, `:125`, `:131`, `:139`.) |
| Mordida sobre `dc3ba57f` | **[nao verificado]**: não há árvore anterior nem resultado de execução disponível. O nome “MORDE” e as asserções não evidenciam o RED exigido. (`app/folha/tests/test_haiku_fora_da_folha.py:114`; `CLAUDE.md:28`.) |
| Ordem de trava / pouso antes do pré-requisito | Corte de nascimento e teste de `registrado` fora da fila existem. (`app/docs/CORTES.md:400`; `app/chamados/tests/test_o261e_f5b_juiz_porta.py:177`, `:184`.) Cumprimento temporal, suíte verde e pouso: **[nao verificado]**; suíte antes do opus é requisito explícito. (`CLAUDE.md:600`.) |

## 31/10 02:28 · sessao 41c42f95852f · - · - · origem=Ronald (tmux)

- **RED sobre `dc3ba57f`: [nao verificado].** O teste contém casos com juiz inteiramente mockado, não uma execução da árvore anterior: `app/folha/tests/test_haiku_fora_da_folha.py:125-139`. A obrigação exige vermelho evidenciado: `CLAUDE.md:28-29`, `CLAUDE.md:348-353`. **Bloqueia certificação do RED**, sem demonstrar sozinho defeito da cura.

- **Cobertura incompleta do novo teste:** verifica `GRUPOS['C']` por substring e a saída personalizada de `o_que_segura`; não verifica `DESTRAVA['chamado']` literal, drawer ou produção real da chave `fila`: `app/folha/tests/test_haiku_fora_da_folha.py:121-139`. Pode passar deixando o drawer incoerente. **Bloqueia se usado como selo dessa coerência.**

- **Juiz duplicado/fonte paralela:** não encontrei nos blocos examinados. O juiz está registrado em `app/core/juizes.py:253`; a escada continua em `app/chamados/juizes.py:1170-1181`; folha lê o fio em `app/folha/fora_da_folha.py:60-68` e a fila em `app/folha/fora_da_folha.py:195-202`. Alimentação consulta a fonte existente em `app/chamados/juizes.py:1140-1154`. **Sem bloqueio identificado nesses blocos.**

- **Segundo escritor:** os blocos de apresentação apenas montam retorno e texto: `app/folha/fora_da_folha.py:191-207`; o juiz monta retorno em `app/chamados/juizes.py:1215-1225`. **Sem escrita persistente identificada nesses blocos.**

- **Relógio:** a dependência `particionar_verbos` lê `timezone.now()` e deriva histórico: `app/chamados/services/verbos.py:32-48`. Sua introdução pelo diff é **[nao verificado]**. **Não atribuir como regressão sem histórico.**

- **Pré-requisito `registrado` fora da fila:** existe teste explícito consultando `COBRANCA` e `qs_na_fila`: `app/chamados/tests/test_o261e_f5b_juiz_porta.py:177-184`. Resultado executado **[nao verificado]**.

- **Ordem de trava/pouso:** nascimento autorizado consta em `app/docs/CORTES.md:400`; suite verde antes do opus é exigida em `CLAUDE.md:600-602`. Execução de RED → cura → GREEN → suite e cumprimento do pouso **[nao verificado]**.

## 31/10 02:28 · sessao 145bbd00c71a · - · - · origem=Ronald (tmux)

Bloqueia: `fila` nova continua podendo nomear chamado invisível ao admin.

- `qs_na_fila` aceita chamado aberto/em análise com `silenciado_ate` passado: `app/chamados/services/painel.py:186`.
- A partição muda qualquer verbo não nulo desse chamado para `historico`: `app/chamados/services/verbos.py:43`.
- `fio_pelos_verbos` devolve `chamado` para `historico`: `app/chamados/juizes.py:1175`.
- `alimentar_portas` inclui esse chamado em `fila_cel`: `app/chamados/juizes.py:1144`.
- O juiz devolve seu número em `fila`: `app/chamados/juizes.py:1224`.
- `o_que_segura` escreve “chamado N na fila”: `app/folha/fora_da_folha.py:199`.
- O painel padrão exclui justamente `historico`: `app/chamados/services/painel.py:322`.

Também bloqueia a afirmação universal das frases: sob uma pergunta viva e pendente, cujo chamado âncora tem silêncio vencido, a rota pode ser `porta_pergunta`, verbo `historico` e fio `chamado`; o app continua mostrando a pergunta, pois não filtra silêncio nem verbo histórico. Logo “nenhuma pede resposta do COLAB” não decorre da garantia do juiz. A pergunta viva sem `respondida_em` é pendente. Fontes: `app/chamados/juizes.py:1216`, `app/chamados/services/pergunta_no_app.py:39`, `app/chamados/models.py:2207`, `app/folha/fora_da_folha.py:36`, `app/folha/fora_da_folha.py:43`.

Outro caso que excede a garantia é chamado de módulo de auditoria gravado aberto: o queryset aceita pelo status, mas a natureza produz verbo `None`; o fio também será `chamado`, embora “decidir” não seja o verbo desse chamado. Fontes: `app/chamados/services/painel.py:187`, `app/chamados/catalogo/natureza.py:18`, `app/chamados/catalogo/natureza.py:44`, `app/chamados/catalogo/natureza.py:63`, `app/chamados/juizes.py:1181`.

Censo da chave `fila`:

| Papel | Local |
|---|---|
| Escritor de produção | `app/chamados/juizes.py:1224` |
| Origem do valor | `app/chamados/juizes.py:1216`, alimentada em `app/chamados/juizes.py:1146` |
| Leitor de produção | `app/folha/fora_da_folha.py:199`; interpolação em `app/folha/fora_da_folha.py:200` |
| Teste compara dict inteiro, porta_fila | `app/chamados/tests/test_o261e_f5b_juiz_porta.py:138` |
| Teste compara dict inteiro, porta_pergunta | `app/chamados/tests/test_o261e_f5b_juiz_porta.py:145` |
| Teste compara retorno inteiro singular/lote | `app/chamados/tests/test_o261e_f5b_juiz_porta.py:163` |
| Teste novo injeta `fila` falsa | `app/folha/tests/test_haiku_fora_da_folha.py:125` |
| Teste novo verifica nomes | `app/folha/tests/test_haiku_fora_da_folha.py:136` |
| Vigia lê somente `verbos`/`rota` | `app/chamados/services/vigia_fio.py:596` |

As combinações acima são contraexemplos permitidos pelo código; existência delas em produção [nao verificado]. O caminho citado nas docstrings como `chamados/painel.py` corresponde na cópia ao serviço `app/chamados/services/painel.py:196`.

## 31/10 02:28 · sessao 919d5ab1e0d0 · - · - · origem=Ronald (tmux)

**Bloqueia: drawer mantém o “N” literal e diverge de `o_que_segura`.** A fonte contém `chamado N na fila, ou a pergunta viva...` (`app/folha/fora_da_folha.py:43`); `o_que_segura` escolhe a rota e substitui o trecho pelos IDs de `fila` ou pela pergunta (`app/folha/fora_da_folha.py:198–202`). O drawer lê `DESTRAVA` literalmente pelo fio gravado (`app/folha/retrato_folha.py:1057–1069`), e o template exibe esse valor (`app/templates/folha/_retrato_o_que_fazer.html:33–38`). O contrato declara “N literal na tela” um bug de rótulo (`app/docs/CORTES.md:353`). Ocorrência em produção: [nao verificado].

O lavrador grava o fio em `dono`, sem `fila` nem `rota` (`app/folha/retrato_folha.py:1177–1188`); a view passa essa linha a `o_que_fazer` (`app/colaboradores/views.py:980–985`). Os testes perpetuam o texto literal, inclusive para `chamado` (`app/folha/tests/test_retrato_folha_dono_sao_tres.py:114–120`; `app/folha/tests/test_haiku_fora_da_folha.py:484`).

| Valor | Escritor | Leitores |
|---|---|---|
| `fila` | Retorno de `porta_alcancavel_do_dia`; `_fila` vem do mapa alimentado por `qs_na_fila` (`app/chamados/juizes.py:1144–1147`, `1216`, `1224`) | Leitor funcional direto: `o_que_segura` (`app/folha/fora_da_folha.py:199–202`). Testes de dict inteiro: `app/chamados/tests/test_o261e_f5b_juiz_porta.py:138–139`, `145–146`. Mock do teste novo: `app/folha/tests/test_haiku_fora_da_folha.py:125–139`. |
| `GRUPOS['C']` | Literal (`app/folha/fora_da_folha.py:36`) | Tooltip `ajuda` (`app/folha/retrato_folha.py:70–72`), produzido como `title` (`825`) e exibido no partial da barra (`app/templates/core/_barra_gestao.html:67`; inclusão em `app/templates/folha/_retrato_barra.html:122`). `fora_da_folha` lê apenas as chaves (`app/folha/fora_da_folha.py:139`). |
| `DESTRAVA['chamado']` | Literal (`app/folha/fora_da_folha.py:43–44`) | Ferramenta (`197–202`), drawer (`app/folha/retrato_folha.py:1069`) e template acima. Testes: `app/folha/tests/test_retrato_folha_drawer.py:224–229`, `app/folha/tests/test_retrato_folha_dono_sao_tres.py:119`, `app/folha/tests/test_haiku_fora_da_folha.py:484`. |

Outros leitores de `GRUPOS`: testes de chaves (`app/folha/tests/test_retrato_folha_quadros.py:154`), de propagação da prosa (`1276`, `1285`) e teste novo (`app/folha/tests/test_haiku_fora_da_folha.py:122–123`).

O vigia lê datas do universo da folha e `verbos`/`rota` do juiz; não consome a prosa nem a nova `fila` nesses caminhos (`app/chamados/services/vigia_fio.py:549–551`, `596–601`). Pautas lê o fio por `_alimentar`/`_fio_do_dia`, sem consumir a prosa (`app/ponto/services/pautas_dos_discordantes.py:60`, `74–77`).

## 31/10 00:53 · sessao 5bb56cea7746 · O261-E-F1-CLASSE-4 · verificador · origem=Code

1. **Os dois laços explícitos obedecem ao C8, mas isso não garante a ordem de todos os fechamentos do dia.** O imediato ordena antes de chamar `reconciliar()`; o adiado também. Havendo um não protestante com fio, o delta envia todos os não protestantes juntos para os adiados. (`app/ponto/services/cartorio.py:1217`, `app/ponto/services/cartorio.py:1225`, `app/ponto/services/cartorio.py:756`)

   `sobrevivente_por_ultimo` põe por último o escolhido por `cobranca_viva_do_dia`, que busca o mais novo por `-criado_em, -pk`; preserva a ordem dos demais. (`app/chamados/juizes.py:1627`, `app/chamados/juizes.py:1645`)

   **Sim, existem fechamentos fora dessa ordenação.** Antes do laço adiado, E2 pode fechar uma disputa, cujo signal reconcilia o chamado-pai sem passar pelo ordenador. Além disso, os adiados continuam em `vivos`: o ramo `FIO-DUP` pode reconciliar um deles ainda no primeiro corpo, também sem C8. (`app/ponto/services/cartorio.py:753`, `app/chamados/reconciliador.py:635`, `app/chamados/signals.py:42`, `app/ponto/services/cartorio.py:1329`)

2. **Em trabalho, o delta também desloca o sem fio para o lote adiado**, desde que ambos tenham os próprios marcos acesos: ambos não protestam e basta um deles estar em `_com_fio`. O sem fio deixa de tentar fechar em `_porm` antes da lavra e passa normalmente a tentar depois da lavra/E2, **antes da segunda execução do corpo**; o com fio já seguia esse caminho. (`app/ponto/services/cartorio.py:1155`, `app/ponto/services/cartorio.py:1218`, `app/ponto/services/cartorio.py:1225`, `app/ponto/services/cartorio.py:753`, `app/ponto/services/cartorio.py:766`)

   O delta não acrescenta veto: marcos acesos regularizam a premissa e o gate aceita essa regularização. A tentativa no laço pode ser um no-op se E2/signal já fechou o chamado. A ressalva é o sem fio composto de perguntas duplicatas mortas: ele ainda pode fechar pelo `FIO-DUP` no primeiro corpo; sem perguntas, esse bloco o pula. (`app/chamados/models.py:1229`, `app/chamados/models.py:699`, `app/chamados/signals.py:42`, `app/ponto/services/cartorio.py:1325`, `app/ponto/services/cartorio.py:1329`)

   Nos testes conferidos, não encontrei exigência de que esse par misto feche em etapas distintas: o teste do evento verifica fechamento no mesmo evento; o v5 usa um único adiado e verifica a ordem das datas; o teste de ressuscita verifica resultado e contadores com um chamado. Ausência dessa dependência em toda a suíte: **[não verificado]**. (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:390`, `app/chamados/tests/test_o261e_f1_codex_v5.py:169`, `app/chamados/tests/test_o261e_f1_codex_v5.py:182`, `app/ponto/tests/test_o261_cobrado_le_o_fio.py:409`)

3. **Sim, a fixture produz o velho com fio, inclusive pela leitura de `pergunta_viva`.** Ela cria a pergunta sem informar `celula`, cuja FK aceita nulo. Em folga, `classificar_falta` retorna sem falta; porém C6 continua até `materializada_em is None` e retorna `True`. `fio_vivo` consulta esse C6, exatamente como `_com_fio`. (`app/ponto/tests/test_fechar3_troca_escala_gate.py:194`, `app/chamados/models.py:2150`, `app/escala/servico_jornada.py:303`, `app/chamados/juizes.py:1074`, `app/chamados/juizes.py:1080`, `app/chamados/juizes.py:1115`, `app/ponto/services/cartorio.py:1094`)

   Essa pergunta **não é absolvida pelo E2**, que exige célula associada. Com o delta, o velho tenta primeiro, fecha por dedup e reata a pergunta ao novo. (`app/chamados/reconciliador.py:588`, `app/chamados/juizes.py:1651`, `app/chamados/models.py:702`, `app/chamados/models.py:418`)

   **Pelo contrafactual do delta fornecido, as asserções finais ficam vermelhas sem a alteração de `_adiar`:** o novo sem fio fecharia imediatamente apoiado no velho; depois o velho ficaria sem irmão, com a premissa da véspera ainda viva, e teria o fechamento recusado. (`app/ponto/services/cartorio.py:1225`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:175`, `app/chamados/juizes.py:424`, `app/chamados/models.py:702`, `app/chamados/models.py:725`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:200`)

4. **Sim, por leitura, `[RECONCILIA-MARCO] ` é o caminho nas duas ordens da fixture anterior.** Em `trabalha=False`, nenhum protesta; sem fio, seguem para `_porm`, ordenado pelo C8. O velho fecha por dedup apoiado no novo, produzindo a linha nesse bloco. O B5.3b depende dos códigos de cobrança, cujo insumo agora recebe zero chamados protestantes. (`app/ponto/tests/test_fechar3_troca_escala_gate.py:134`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:137`, `app/ponto/services/cartorio.py:1155`, `app/ponto/services/cartorio.py:1159`, `app/ponto/services/cartorio.py:1225`, `app/ponto/services/cartorio.py:1231`, `app/ponto/services/cartorio.py:1252`, `app/ponto/supra_juiz.py:441`)

Execução dos testes e da mutação: **[não verificado]**. [nao verificado]

## 31/10 00:53 · sessao 7ab06b1c856c · - · - · origem=Ronald (tmux)

- Em trabalho, `marcos_acesos=True` faz ambos não protestarem; havendo um com fio, `_adiar` recebe ambos. O sem fio deixa de tentar `_porm` no primeiro corpo e normalmente tenta fechar após lavra/E2, ainda antes do segundo corpo; o com fio já seguia esse caminho. `app/ponto/services/cartorio.py:1155`, `app/ponto/services/cartorio.py:1217`, `app/ponto/services/cartorio.py:1225`, `app/ponto/services/cartorio.py:744`, `app/ponto/services/cartorio.py:753`, `app/ponto/services/cartorio.py:756`, `app/ponto/services/cartorio.py:766`.

- O delta não acrescenta veto ao fechamento: marcos acesos regularizam a premissa, e o gate aceita a premissa regularizada. A tentativa pode resultar em no-op se E2/signal já fechou o chamado. `app/chamados/models.py:1229`, `app/chamados/models.py:699`, `app/chamados/reconciliador.py:638`, `app/chamados/signals.py:42`, `app/chamados/models.py:615`.

- Ressalva concreta à ordem: adiados continuam em `vivos`; se o sem fio possui somente perguntas `VIA_DUPLICATA`, `FIO-DUP` tenta reconciliá-lo ainda no primeiro corpo, sem C8. Sem perguntas, esse bloco pula o chamado. `app/ponto/services/cartorio.py:1220`, `app/ponto/services/cartorio.py:1321`, `app/ponto/services/cartorio.py:1325`, `app/ponto/services/cartorio.py:1329`.

- Nos testes conferidos, nenhum exige que o par misto trabalho/com fio/sem fio feche em corpos distintos: o teste do evento verifica fechamento no mesmo evento e ausência de terminal com pergunta viva; o v5 usa um único adiado e verifica ordem das datas; o teste de ressuscita usa um chamado e verifica resultado/contadores, pelo caminho B5.3b. `app/chamados/tests/test_o261e_f1_porta_encerramento.py:358`, `app/chamados/tests/test_o261e_f1_porta_encerramento.py:390`, `app/chamados/tests/test_o261e_f1_codex_v5.py:169`, `app/chamados/tests/test_o261e_f1_codex_v5.py:182`, `app/ponto/tests/test_o261_cobrado_le_o_fio.py:393`, `app/ponto/tests/test_o261_cobrado_le_o_fio.py:416`. Ausência dessa dependência em toda a suíte: [não verificado].

## 31/10 00:52 · sessao 5b464716b57a · - · - · origem=Ronald (tmux)

**3. Sim, por leitura estática a fixture alcança o lote misto e distingue o delta.**

- O velho recebe a pergunta via `objects.create(... chamado_destino=velho, data_evento=DIA)`, sem `celula`; a FK permite nulo. A associação automática à célula pertence a `abrir()`, que a fixture não chama. (`app/ponto/tests/test_fechar3_troca_escala_gate.py:193`, `app/chamados/models.py:2150`, `app/chamados/models.py:2363`)
- `trabalha=False` faz `classificar_falta()` retornar `(None, None, None)`, mas isso apenas impede o retorno antecipado `True` naquele degrau de C6. Como a pergunta está sem célula e sem materialização, C6 chega a `materializada_em is None` e retorna `True`. Portanto ela está viva mesmo no dia de folga. (`app/escala/servico_jornada.py:303`, `app/chamados/juizes.py:1054`, `app/chamados/juizes.py:1074`, `app/chamados/juizes.py:1080`)
- `fio_vivo` consulta `pergunta_viva`; `_com_fio` consulta `fio_vivo` nas resolvedoras do chamado. Assim, o velho entra em `_com_fio`, enquanto o novo não recebeu pergunta. (`app/chamados/juizes.py:1115`, `app/ponto/services/cartorio.py:1094`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:194`)
- O E2 não absolve essa pergunta da fixture: seu filtro exige `celula__isnull=False`. O fechamento do velho pode ocorrer pela exceção de dedup com reata da pergunta ao novo. (`app/chamados/reconciliador.py:588`, `app/chamados/models.py:702`, `app/chamados/models.py:371`, `app/chamados/models.py:418`)
- Com o delta, ambos vão ao lote adiado e C8 põe o novo por último. Sem a alteração de `_adiar` da volta 1, o novo sem fio seria reconciliado imediatamente sozinho, apoiado no velho; depois o velho ficaria sem irmão e seria recusado pela premissa ainda viva da véspera. As expectativas finais ficariam invertidas: velho vivo e novo resolvido. (`app/ponto/services/cartorio.py:1217`, `app/ponto/services/cartorio.py:1225`, `app/ponto/services/cartorio.py:756`, `app/chamados/juizes.py:1651`, `app/chamados/models.py:702`, `app/chamados/models.py:725`, `app/chamados/juizes.py:424`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:175`)

Resultado efetivo de execução do teste ou da mutação: **[não verificado]**. [nao verificado]

## 31/10 00:46 · sessao 9bae963174b4 · O261-E-F1-CLASSE-4 · verificador · origem=Code

O diff `HEAD × HEAD~1` ficou **[nao verificado]**: `git show HEAD` falhou porque a cópia entregue não contém repositório Git. Abaixo estão conclusões sobre o código atual; atribuí-las à introdução neste commit seria **[nao verificado]**. Ocorrência e quantidade em produção também são **[nao verificado]**. [nao verificado]

1. **Quem escreve e quem lê**

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `CelulaDia.trabalha` | Porta da célula: regeneração, reversão e geração (`app/ponto/portas/celula.py:231`, `:260`, `:532`, `:612`). | Juiz `EscalaColaborador.eh_dia_trabalho` (`app/escala/models.py:1072`); cartório (`app/ponto/services/cartorio.py:1156`); premissa dos chamados (`app/chamados/juizes.py:389`); leitor da grade (`app/escala/services/leitor_celula.py:344`). |
| `dna['marcos']` | Conteúdo produzido por `montar_dna`, somente quando `trabalha is True` (`app/ponto/management/commands/gerar_celulas.py:79`, `:83`, `:100`); persistido pelas operações da porta (`app/ponto/portas/celula.py:260`, `:530`, `:614`). | Juiz dos marcos do vínculo (`app/escala/models.py:1099`); produtor das lâmpadas (`app/ponto/services/cartorio.py:434`); juiz do marco perguntado (`app/chamados/juizes.py:849`). |
| `ata['lampadas']` | Produzida por `lampadas_do_dia`/`ata_do_dia`; teto aplicado por `_ata_ate_o_teto` (`app/ponto/services/cartorio.py:418`, `:555`, `:703`). Persistência por `lavrar_veredito` e `carimbar` (`app/ponto/portas/celula.py:690`, `:728`). | Lastro: acesos, inexistentes, apagados e pendentes (`app/ponto/services/lastro.py:89`, `:115`, `:128`, `:148`); pergunta (`app/chamados/juizes.py:866`); grade (`app/escala/services/leitor_celula.py:354`); folha (`app/folha/services/sem_furo.py:22`). |
| Veredito `cobrado` | Escolhido pelo cartório quando há fio que ainda protesta (`app/ponto/services/cartorio.py:1195`); persistido pela porta (`app/ponto/portas/celula.py:692`). | Pertence a `VEREDITOS_ACUSAM` (`app/escala/models.py:1735`); lido pelo C6 (`app/chamados/juizes.py:1056`), E2 (`app/chamados/reconciliador.py:598`), fábrica (`app/chamados/services/fabrica_da_celula.py:86`) e retenção da folha (`app/folha/export.py:1215`). |
| Pergunta materializada/retratada | Neste caminho, E2 escreve `materializada_em`, autor e `VIA_CELULA_ABSOLVEU` (`app/chamados/reconciliador.py:620`). Há outros escritores efetivos: materializador, emissão, dedup e portas do modelo (`app/chamados/services/materializacao.py:509`; `app/chamados/services/disputa_emissao.py:1562`; `app/chamados/services/fio.py:894`; `app/chamados/models.py:2456`). | C6 lê carimbo, via, resposta e célula (`app/chamados/juizes.py:1020`, `:1044`, `:1050`, `:1080`); `estado` delega a ele (`app/chamados/models.py:2435`). Desfazer carimbo também consulta C6 (`app/chamados/services/relavra_via.py:227`). |
| Estado de `ChamadoColaborador` | Transições persistem por `_gravar_estado` (`app/chamados/models.py:222`, `:279`); nascimentos têm portas próprias (`app/chamados/models.py:1039`). | Motor decide transições (`app/chamados/models.py:637`); cartório seleciona vivos (`app/ponto/services/cartorio.py:1089`); C8 escolhe cobrança sobrevivente (`app/chamados/juizes.py:1625`); C6 também considera o estado do destino (`app/chamados/models.py:313`). |

O modelo desses caminhos chama-se **`PerguntaDisputa`**, com estado `PENDENTE`; E2 “retrata” pelo carimbo e pela via de resolução (`app/chamados/models.py:2142`, `:2310`; `app/chamados/reconciliador.py:620`).

**Sim, há leitores com critérios diferentes:**

- O cartório exclui do protesto `trabalha=False` ou cobrança com todos os próprios marcos acesos; `dia_sem_trabalho` considera também `trabalha=None` e qualquer `ata.tipo_dia` diferente de trabalho. Os critérios divergem (`app/ponto/services/cartorio.py:1156`; `app/chamados/juizes.py:389`).
- Os comandos `supra_juiz` e `emitir_furo_retroativo` continuam entregando **todos os chamados vivos** ao classificador, enquanto o cartório entrega `_protestam`. Portanto, a mesma classificação recebe insumos diferentes (`app/ponto/management/commands/supra_juiz.py:207`; `app/ponto/management/commands/emitir_furo_retroativo.py:100`; `app/ponto/services/cartorio.py:1159`).
- `q_vivas` mantém toda célula provisória como candidata, sem a exceção pelo próprio marco aceso. Isso é uma seleção mais larga, permitida pelo contrato explícito de **continência**, com julgamento posterior; isoladamente, não é defeito (`app/chamados/models.py:2577`, `:2556`).
- A absolvição de **chamados de turno** ainda exige ausência de qualquer marco pendente; E2 agora individualiza a pergunta. São universos diferentes: dia do chamado versus marco perguntado (`app/chamados/reconciliador.py:983`, `:610`).

2. **Onde o desenho falha ou permanece incompleto**

**A partição entre imediatos e adiados quebra a escolha do sobrevivente C8.** Considere dois `turno_aberto` no dia de folga, com véspera furada: o mais velho tem pergunta viva sob o veredito anterior acusador; o mais novo não tem fio. Esse módulo pertence ao universo lido pelo cartório, e C6 pode manter a pergunta viva pelo veredito anterior (`app/chamados/catalogo/furo_modulos.py:15`; `app/chamados/juizes.py:1054`).

O caminho fica assim:

- `_com_fio` é calculado antes da nova lavra; o velho vai para `_adiar`, o novo para `_porm` (`app/ponto/services/cartorio.py:1094`, `:1214`, `:1222`).
- Cada subconjunto tem um elemento; o ordenador retorna sem consultar C8. O novo fecha por dedup apoiado no velho ainda vivo (`app/chamados/juizes.py:1645`; `app/chamados/models.py:702`).
- Depois da lavra e de E2, o velho é tentado sozinho. Para `turno_aberto`, a folga não regulariza diretamente: a premissa consulta a véspera furada. Sem outro vivo, o fechamento é recusado (`app/ponto/services/cartorio.py:753`, `:756`; `app/chamados/models.py:1225`; `app/chamados/juizes.py:424`; `app/chamados/models.py:721`).

**Resultado do caminho: novo fechado, velho vivo**, contrariando C8, que escolhe o mais novo por `-criado_em, -pk`. Ordenar separadamente os subconjuntos não ordena o lote inteiro. Reprodução com banco: **[nao verificado]** (`app/chamados/juizes.py:1627`; `app/ponto/services/cartorio.py:1222`, `:756`).

**Há leitura obsoleta no memo do C6.** A chave não inclui ata nem DNA, embora a decisão dependa deles. Confirmei com funções extraídas em memória: pergunta provisória inicialmente viva; depois, alterando só a ata para acender seu marco, a mesma instância continua retornando `True`, enquanto `_pergunta_viva_calc` retorna `False`. Reprodução integrada com banco: **[nao verificado]** (`app/chamados/juizes.py:767`, `:793`, `:849`, `:866`, `:1068`).

**O relógio não está inteiramente na borda recebida.** `julgar_celula(agora=...)` calcula `hoje` pelo relógio real e compara o fim do turno com outro `now()`, antes de encaminhar o `agora` fornecido. Assim, o instante injetado não governa todas as guardas (`app/ponto/services/cartorio.py:1597`, `:1618`, `:1621`).

**Não constatei segundo escritor persistente novo nesses caminhos.** O cartório continua chamando a porta da célula; a sobreposição de ata em `celula_do_dia_agora` é somente em memória. E2 já é escritor sancionado no censo de `materializada_em`; esse campo possui escritores plurais declarados (`app/ponto/services/cartorio.py:1470`, `:1485`; `app/chamados/juizes.py:382`; `app/core/tests/test_contract_escritores_estado.py:83`).

**Há risco de ordem inversa entre duas travas de chamados.** `reconciliar` trava o próprio chamado; a reata escolhe outro vivo e a porta de costura trava esse destino. Duas transações A→B e B→A podem solicitar as linhas em ordem inversa; não há ordenação conjunta por PK nesse caminho. Deadlock reproduzido: **[nao verificado]** (`app/chamados/models.py:672`, `:371`, `:418`; `app/chamados/services/fio.py:762`).

**Idempotência não é uniforme.** O fechamento sequencial relê o banco e o motor impede novo `RECONCILIAR` sobre resolvido; a recusa de fechamento deduplica a trilha. Porém `reabrir` recusado por premissa regularizada ou admin sem fato novo escreve `reabertura_recusada` novamente em cada chamada: mesmo estado, várias linhas (`app/chamados/models.py:636`, `:378`, `:1335`, `:1341`; `app/chamados/catalogo/motor.py:111`).

**Existe caminho de vai-e-volta por assimetria dia/turno.** O fechamento por dedup aceita outro vivo do **dia**; a reabertura procura irmão do **turno**, exigindo `entrada_timestamp`. O teste dos dois chamados em folga omite essa chave. Com véspera furada e vínculo sem veto, o fechado por sistema pode reabrir sem fato novo e voltar a fechar por dedup; a exigência de fato novo é específica do fechamento administrativo. Reprodução integrada: **[nao verificado]** (`app/chamados/models.py:702`, `:1283`, `:1294`, `:1373`; `app/chamados/juizes.py:1674`; `app/ponto/tests/test_fechar3_troca_escala_gate.py:131`, `:145`).

3. **Chamado legítimo fechado e pergunta provisória sem batida**

**O gate atual permite fechar chamado legítimo em ausência parcial.** A grade mantém `tipo_dia='ausencia'`, marca cobertura parcial e conserva as células faltantes; o classificador reconhece isso como dia de cobrança. Entretanto, `dia_sem_trabalho` responde verdadeiro para esse tipo, e `premissa_regularizada` aceita a resposta **antes de consultar o marco apagado** (`app/escala/utils.py:1180`, `:1190`, `:1195`; `app/ponto/supra_juiz.py:279`; `app/chamados/juizes.py:389`; `app/chamados/models.py:1225`).

Logo, se `reconciliar()` for chamado para esse furo e não houver pergunta viva que retenha o fechamento, ele pode ser resolvido mesmo com marco realmente faltante; a mesma premissa depois impede reabrir. É defeito do gate atual, sem prova de introdução pelo HEAD ou ocorrência no dado de produção: **[nao verificado]** (`app/chamados/models.py:646`, `:649`, `:618`, `:1289`).

**Nos ramos `_adiar`/`_porm`, trabalho verdadeiro com próprio marco apagado ou pendente continua protestando.** `marcos_acesos` não retorna verdadeiro nesse caso, portanto esses conjuntos não o selecionam somente pela mudança examinada (`app/ponto/services/lastro.py:175`, `:179`; `app/ponto/services/cartorio.py:1156`, `:1214`, `:1222`).

**Pergunta provisória cujo próprio marco continua sem batida permanece protegida na nova condição.** Lâmpada `acesa=None`/`False` não prova o fato; C6 e E2 exigem literalmente `is True` para dispensar o teto. E2 ainda consulta `pergunta_viva` antes de carimbar (`app/chamados/juizes.py:869`, `:1068`; `app/chamados/reconciliador.py:610`, `:616`).

4. **Ordem, conjunto reconciliado e testes existentes** [nao verificado]

**Há um teste existente cuja expectativa de caminho precisa mudar:** ele exige `[RECONCILIA] `, mas os chamados de folga agora entram em `_porm`, que registra `[RECONCILIA-MARCO]`. As expectativas de estado e sobrevivente desse fixture continuam pertinentes (`app/ponto/tests/test_fechar3_troca_escala_gate.py:153`, `:154`, `:158`; `app/ponto/services/cartorio.py:1222`, `:1228`).

**Esse teste não cobre a falha da partição:** cria ambos sem perguntas. Falta a variante “velho com fio, novo sem fio”, que percorre os dois subconjuntos e pode preservar o velho; isso pede cobertura adicional e correção da ordem global, não trocar a expectativa para aceitar o sobrevivente errado (`app/ponto/tests/test_fechar3_troca_escala_gate.py:142`; `app/ponto/services/cartorio.py:1214`, `:1222`, `:756`; `app/chamados/juizes.py:1627`).

**O teste da segunda passada continua compatível:** a lista é ordenada por data antes do corpo; seu chamado falso único não provoca consulta C8, pois o ordenador retorna para lote menor que dois (`app/ponto/services/cartorio.py:766`; `app/chamados/tests/test_o261e_f1_codex_v5.py:169`, `:183`; `app/chamados/juizes.py:1645`).

**Os testes antigos de provisória sem batida devem continuar exigindo pergunta viva e nenhum carimbo.** Eles usam justamente o próprio marco `acesa=None, pendente=True`, que não satisfaz a exceção nova (`app/chamados/tests/test_o261_pergunta_viva_teto.py:54`; `app/chamados/tests/test_o261_e2_absolvicao_exige_fato_encerrado.py:27`, `:64`). Resultado da suíte completa: **[nao verificado]**.

## 31/10 00:45 · sessao 6a0aa9eab820 · - · - · origem=Ronald (tmux)

A partição quebra a escolha do sobrevivente C8 no cenário proposto, por leitura do caminho; execução com banco **[não verificado]**.

1. `turno_aberto` pertence a `FURO_MODULES` (`app/chamados/catalogo/furo_modulos.py:15`) e ao universo do gate (`app/chamados/catalogo/modulos.py:237`, `app/chamados/models.py:1187`).
2. A pergunta do velho pode estar viva com `trabalha=False` e veredito anterior acusador: C6 retorna True pelo veredito, sem consultar `trabalha` (`app/chamados/juizes.py:1054`). `_com_fio` é calculado antes da nova lavra (`app/ponto/services/cartorio.py:1094`).
3. Na folga, ambos deixam de protestar (`app/ponto/services/cartorio.py:1155`). O velho com fio vai para `_adiar` (`app/ponto/services/cartorio.py:1214`); o novo sem fio vai para `_porm` (`app/ponto/services/cartorio.py:1222`).
4. Cada subconjunto contém um elemento: `sobrevivente_por_ultimo` retorna sem consultar C8 (`app/chamados/juizes.py:1645`). O novo fecha por dedup, apoiado no velho ainda vivo (`app/chamados/models.py:702`).
5. A lavra grava o veredito da folga (`app/ponto/services/cartorio.py:1470`); E2 pode então carimbar a pergunta do velho, pois a célula não acusa e não tem pendentes (`app/chamados/reconciliador.py:584`, `app/chamados/reconciliador.py:610`, `app/chamados/juizes.py:1068`).
6. O laço adiado tenta fechar o velho sozinho (`app/ponto/services/cartorio.py:755`). Para `turno_aberto`, folga não regulariza diretamente a premissa (`app/chamados/models.py:1225`); ela consulta a véspera furada (`app/chamados/juizes.py:424`). Sem outro vivo, o gate recusa (`app/chamados/models.py:702`, `app/chamados/models.py:721`).

**Resultado: novo fechado, velho vivo**, contrariando o C8, que escolhe o mais novo por `-criado_em, -pk` (`app/chamados/juizes.py:1627`). O teste existente de sobrevivência cria ambos sem perguntas e não cobre a partição por fio (`app/ponto/tests/test_fechar3_troca_escala_gate.py:142`).

## 31/10 00:44 · sessao edb37687a75c · - · - · origem=Ronald (tmux)

Achados conferidos, somente por leitura:

- **Chamado legítimo pode fechar por ausência parcial.** A grade conserva `tipo_dia='ausencia'`, `cobertura_parcial=True` e os marcos faltantes em `app/escala/utils.py:1180` e `app/escala/utils.py:1184`; o suprajuiz considera essa combinação dia de cobrança em `app/ponto/supra_juiz.py:274`. Entretanto, `dia_sem_trabalho` responde True para qualquer `ata.tipo_dia != 'trabalho'`, mesmo com `cel.trabalha=True` (`app/chamados/juizes.py:386`). `premissa_regularizada` aceita essa resposta antes de consultar as lâmpadas (`app/chamados/models.py:1220`). Assim, um chamado de furo sem pergunta viva ancorada pode passar pelo gate e ser reconciliado (`app/chamados/models.py:646`, `app/chamados/models.py:649`, `app/chamados/models.py:618`); depois a mesma premissa impede sua reabertura (`app/chamados/models.py:1289`). A ata não conserva `cobertura_parcial` (`app/ponto/services/cartorio.py:557`). Introdução desse defeito no HEAD: [não verificado].

- **Provisória sem a batida continua protegida no novo E2.** O carimbo exige ausência de pendentes ou prova positiva do próprio marco; depois ainda consulta C6 (`app/chamados/reconciliador.py:608`, `app/chamados/reconciliador.py:616`). `fato_do_marco_no_espelho` exige o marco do DNA, lâmpadas correspondentes e todas acesas (`app/chamados/juizes.py:842`, `app/chamados/juizes.py:863`, `app/chamados/juizes.py:869`). Uma lâmpada própria com `acesa=None, pendente=True` não satisfaz isso. Os testes de provisória sem batida seguem coerentes (`app/chamados/tests/test_o261_pergunta_viva_teto.py:52`, `app/chamados/tests/test_o261_e2_absolvicao_exige_fato_encerrado.py:62`).

- **A irmã dos chamados conserva o teto global.** `_absolvido_pela_celula` continua recusando qualquer célula com marcos pendentes (`app/chamados/reconciliador.py:981`). O novo E2 individualiza a pergunta, enquanto essa irmã examina o dia inteiro (`app/chamados/reconciliador.py:608`, `app/chamados/reconciliador.py:983`).

- **Não nasceu escritor novo de estado de chamado nesse caminho.** `_retratar` chama `ch.reconciliar` (`app/chamados/reconciliador.py:524`); `reconciliar` usa o motor e `_gravar_estado` (`app/chamados/models.py:614`). A escrita persistida de `status_local` está em `app/chamados/models.py:222`, e a trilha precede o save no mesmo atomic (`app/chamados/models.py:274`). Os nascimentos são exceções próprias (`app/chamados/models.py:890`, `app/chamados/models.py:1039`).

- **E2 já é escritor sancionado de materialização.** A gravação continua em `app/chamados/reconciliador.py:620`; o censo declara esse arquivo em `app/core/tests/test_contract_escritores_estado.py:83`. Há pluralidade de escritores preexistente; não é correto dizer que todo `materializada_em` tem uma porta única global (`app/core/tests/test_contract_escritores_estado.py:83`).

- **Idempotência sequencial do fechamento:** a segunda chamada relê o banco e o motor recusa novo RECONCILIAR sobre resolvido (`app/chamados/models.py:636`, `app/chamados/catalogo/motor.py:111`). A recusa de fechamento também deduplica trilha (`app/chamados/models.py:378`). **Contraexemplo atual ao contrato “2×, uma linha”**: `reabrir` recusado por admin sem fato novo ou premissa regularizada grava `reabertura_recusada` sem dedup a cada chamada (`app/chamados/models.py:1335`, `app/chamados/models.py:1341`). Introdução no HEAD: [não verificado].

- **Risco estático de ciclo entre duas travas de chamados:** reconciliar trava primeiro o próprio chamado (`app/chamados/models.py:669`), escolhe outro vivo para receber a pergunta (`app/chamados/models.py:371`) e a costura trava esse destino (`app/chamados/services/fio.py:758`, `app/chamados/services/fio.py:837`). C8 aceita o outro excluindo o próprio (`app/chamados/juizes.py:1625`). Duas transações A→B e B→A podem portanto pedir as duas linhas em ordem inversa; reprodução de deadlock: [não verificado]. Os testes atuais conferem trava própria antes do plano e trava do destino isoladamente (`app/chamados/tests/test_o261e_f1_codex_v5.py:25`, `app/chamados/tests/test_o261e_f1_codex_v5.py:65`).

- **SQL com regra global própria:** `q_vivas` ainda usa qualquer lâmpada pendente para manter candidata (`app/chamados/models.py:2575`), enquanto C6 passou a absolver pelo próprio marco (`app/chamados/juizes.py:1068`). Isso amplia candidatos mortos, porém o contrato documentado é continência, com julgamento posterior, e não igualdade (`app/chamados/models.py:2556`). A property `.viva` delega ao C6 (`app/chamados/models.py:2597`).

- **Ordenação:** os adiados passam agora por `sobrevivente_por_ultimo` (`app/ponto/services/cartorio.py:754`); o fechamento imediato também (`app/ponto/services/cartorio.py:1221`). Esse ordenador consulta C8 e põe o sobrevivente por último, preservando a ordem dos demais (`app/chamados/juizes.py:1644`). A segunda passada continua ordenada por data (`app/ponto/services/cartorio.py:766`), como exige o teste existente (`app/chamados/tests/test_o261e_f1_codex_v5.py:164`).

## 31/10 00:44 · sessao 6a0aa9eab820 · - · - · origem=Ronald (tmux)

Conferências do recorte:

- **Um teste existente precisa mudar de caminho/log:** `test_MORDE_rec_dois_turno_aberto_em_folga_com_vespera_furada_sobrevive_o_mais_novo` exige `[RECONCILIA] ` em `app/ponto/tests/test_fechar3_troca_escala_gate.py:153`. Com `trabalha=False`, `_protesta` devolve False (`app/ponto/services/cartorio.py:1155`), o lote passa por `_porm` (`app/ponto/services/cartorio.py:1222`) e escreve `[RECONCILIA-MARCO]` (`app/ponto/services/cartorio.py:1228`). `_protestam` vazio não gera a cobrança indevida que acionaria `_rec` (`app/ponto/services/cartorio.py:1157`, `app/ponto/services/cartorio.py:1249`). A expectativa de sobrevivência do mais novo continua compatível com `sobrevivente_por_ultimo` (`app/chamados/juizes.py:1648`) e com o gate de dedup (`app/chamados/models.py:702`).

- **O teste de adiados não exige mock novo:** seu lote contém um único chamado falso (`app/chamados/tests/test_o261e_f1_codex_v5.py:169`); `sobrevivente_por_ultimo` devolve lotes com menos de dois elementos antes de consultar ORM (`app/chamados/juizes.py:1645`). A ordenação da segunda passada continua protegida pelo `sorted` (`app/ponto/services/cartorio.py:766`) e pelo teste (`app/chamados/tests/test_o261e_f1_codex_v5.py:183`).

- **Os testes antigos do teto continuam válidos:** exercitam justamente o marco perguntado com `acesa=None, pendente=True` (`app/chamados/tests/test_o261_e2_absolvicao_exige_fato_encerrado.py:27`), que não satisfaz `fato_do_marco_no_espelho` (`app/chamados/juizes.py:869`). Portanto continuam esperando ausência de carimbo (`app/chamados/tests/test_o261_e2_absolvicao_exige_fato_encerrado.py:64`) e pergunta viva (`app/chamados/tests/test_o261_pergunta_viva_teto.py:54`).

- **Leitor que não migrou literalmente, mas tem contrato mais largo:** `q_vivas` mantém a proteção de toda célula provisória, sem a exceção pelo marco aceso (`app/chamados/models.py:2575`). Entretanto seu contrato é prefiltragem generosa por continência; o consumidor deve consultar o juiz para resposta exata (`app/chamados/models.py:2556`, `app/chamados/models.py:2594`). Isso isoladamente não prova divergência indevida.

- **Memo incompleto:** a decisão nova depende de `cel.ata` e `cel.dna` (`app/chamados/juizes.py:1068`, `app/chamados/juizes.py:849`, `app/chamados/juizes.py:866`), mas `_chave_memo_pv` não contém esses campos (`app/chamados/juizes.py:767`). Alterar somente a ata, mantendo o veredito, permite devolver a resposta cacheada (`app/chamados/juizes.py:793`). O teste que promete invalidar por qualquer campo lido não cobre ata/DNA (`app/chamados/tests/test_selo_memo_pergunta_viva.py:117`). Não prova loop persistente: E2 consulta novas instâncias (`app/chamados/reconciliador.py:584`).

- **Vai-e-volta possível por assimetria preexistente:** fechamento por dedup usa outro chamado vivo do **dia** (`app/chamados/models.py:702`); reabertura verifica irmão do **turno** (`app/chamados/models.py:1294`), que exige `entrada_timestamp` (`app/chamados/juizes.py:1674`). O fixture dos dois `turno_aberto` em folga omite essa chave (`app/ponto/tests/test_fechar3_troca_escala_gate.py:145`), mantém a véspera furada (`app/ponto/tests/test_fechar3_troca_escala_gate.py:131`) e fecha o velho por dedup (`app/ponto/tests/test_fechar3_troca_escala_gate.py:154`). A premissa do turno em folga continua consultando a véspera (`app/chamados/models.py:1225`, `app/chamados/juizes.py:424`). Com vínculo sem veto, o velho pode reabrir: exigência de fato novo só vale para encerramento administrativo (`app/chamados/models.py:1282`); caminho permitido grava reabertura (`app/chamados/models.py:1373`). Rejulgamento pode fechá-lo por dedup novamente. Execução com banco: **[não verificado]**.

- **Nomenclatura:** o modelo destes caminhos é `PerguntaDisputa` (`app/chamados/models.py:2142`), com estado `PENDENTE` (`app/chamados/models.py:2310`); a existência de um modelo chamado `PerguntaPendente` não foi encontrada no censo textual da cópia **[não verificado]**.

- **Outra regra de “sem trabalho” já existente:** `dia_sem_trabalho` considera `trabalha is not True` ou ata com tipo diferente de trabalho (`app/chamados/juizes.py:386`), enquanto o predicado novo só dispensa por `trabalha is False` ou marcos acesos (`app/ponto/services/cartorio.py:1155`).

## 31/10 00:43 · sessao 96bb4a47aa74 · - · - · origem=Ronald (tmux)

Censo e consequências conferidas:

- **`CelulaDia.trabalha` e `dna`**: escritos na porta por `regenerar_celulas_vinculo` (`app/ponto/portas/celula.py:231`, `:243`, `:260`), `reverter_regeneracao_dia` (`:530`) e `gerar_celulas_periodo` (`:590`). O produtor do conteúdo é `montar_dna`, que só põe marcos quando `trabalha is True` (`app/ponto/management/commands/gerar_celulas.py:73`, `:79`, `:100`). Leitores canônicos: `eh_dia_trabalho` devolve `cel.trabalha` (`app/escala/models.py:1048`, `:1072`); `marcos_do_dia` lê DNA e, com marcos vazios/nulos, recorre ao template (`:1099`, `:1108`). Outros leitores relevantes: cartório (`app/ponto/services/cartorio.py:1156`), pergunta por marco (`app/chamados/juizes.py:849`), leitura da grade soberana (`app/escala/services/leitor_celula.py:344`).

- **`ata['lampadas']`**: conteúdo produzido por `lampadas_do_dia`/`ata_do_dia`, usando DNA e casamento da grade (`app/ponto/services/cartorio.py:418`, `:434`, `:555`); teto transforma somente apagadas ainda pendentes em `acesa=None, pendente=True` (`:696`, `:703`). Persistência passa por `lavrar_veredito` e `carimbar` (`:1468`, `:1470`, `:1485`; `app/ponto/portas/celula.py:647`, `:690`, `:728`). `carimbar_precedencia` altera outra chave da mesma ata (`app/ponto/portas/celula.py:732`, `:740`). `celula_do_dia_agora` sobrepõe a ata apenas no objeto em memória (`app/chamados/juizes.py:379`, `:382`); a grade também faz uma ata transitória para alimentar o juiz, chamando o produtor existente (`app/escala/utils.py:1827`, `:1832`). Não são segundos escritores persistentes nesses sítios.

- **Leitores das lâmpadas**: lastro da cobrança (`app/ponto/services/lastro.py:82`, `:153`), marcos inexistentes (`:98`), apagados (`:125`) e pendentes (`:138`); fato da pergunta (`app/chamados/juizes.py:831`, `:866`); concordância resposta/luz (`:957`, `:981`); grade soberana (`app/escala/services/leitor_celula.py:331`, `:354`); folha prévia (`app/folha/previa.py:84`); sem furo (`app/folha/services/sem_furo.py:22`); horário de entrada (`app/ponto/services/vigia_entrada.py:80`, `:123`).

- **`cobrado`**: decisão explícita do cartório depende agora do fio e `_protesta` (`app/ponto/services/cartorio.py:1195`, `:1196`); a porta valida e persiste (`app/ponto/portas/celula.py:678`, `:692`). É acusador (`app/escala/models.py:1735`). Leitores: pergunta viva (`app/chamados/juizes.py:1054`), concordância da resposta (`:1002`), SQL de perguntas (`app/chamados/models.py:2579`), fábrica (`app/chamados/services/fabrica_da_celula.py:86`), emissão (`app/chamados/services/disputa_emissao.py:705`), retenção da folha (`app/folha/export.py:1215`).

**Divergências reais entre leitores:**

1. Cartório usa `_protesta = cel.trabalha is not False and marcos_acesos(...) is not True` (`app/ponto/services/cartorio.py:1155`). `dia_sem_trabalho` entende “sem trabalho” como `trabalha is not True` **ou** ata de tipo diferente de trabalho (`app/chamados/juizes.py:386`). Divergem para `trabalha=None` e `trabalha=True` com ata não trabalho. O leitor da grade também prioriza `ata.tipo_dia` sobre o campo (`app/escala/services/leitor_celula.py:344`). Consequência operacional desses casos no dado fornecido: **[não verificado]**.

2. O comando supra e o emissor retroativo continuam passando **todos os vivos** ao classificador (`app/ponto/management/commands/supra_juiz.py:195`, `:207`; `app/ponto/management/commands/emitir_furo_retroativo.py:93`, `:100`), enquanto cartório passa `_protestam` (`app/ponto/services/cartorio.py:1157`, `:1159`). Portanto podem gerar `COBRANCA_SEM_FURO`/`COBRANCA_EM_DIA_SEM_TRABALHO` quando o cartório exclui a cobrança desse insumo; os predicados desses códigos estão em `app/ponto/supra_juiz.py:418` e `:441`. O supra ainda executa retratação com `--executar` (`app/ponto/management/commands/supra_juiz.py:243`, `:248`; trava desligada em `:41`), sujeito a conversa e cobrança órfã (`:225`, `:239`). **Vai-e-volta concreto provocado por isso: [não verificado]**.

3. `q_vivas` preserva toda célula provisória na seleção, mesmo quando o fato da pergunta acendeu (`app/chamados/models.py:2577`); juiz agora absolve esse caso (`app/chamados/juizes.py:1068`). Isso é seleção mais ampla, compatível com o contrato declarado de continência e filtro posterior (`app/chamados/models.py:2556`).

**Legítimos e provisórios:**

- Para `trabalha=True` com marco nomeado realmente apagado/pendente, `marcos_acesos` devolve `False` (`app/ponto/services/lastro.py:175`, `:179`); `_protesta=True` e o chamado não entra nos novos conjuntos `_adiar` e `_porm` (`app/ponto/services/cartorio.py:1156`, `:1214`, `:1222`). Fechamento novo indevido desse legítimo por esses ramos: não demonstrado.
- Pergunta provisória sem a batida tem fato `False`/`None`, nunca `True` (`app/chamados/juizes.py:863`, `:869`); a exceção nova exige literalmente `is True` (`:1068`). Os testes existentes preservam o caso sem batida (`app/chamados/tests/test_o261_pergunta_viva_teto.py:52`, `:62`; `app/chamados/tests/test_o261_e2_absolvicao_exige_fato_encerrado.py:62`).

**Relógio e idempotência:**

- `julgar_colab` recebe `hoje/agora`, e `marcos_vencidos` compara com esse instante (`app/ponto/services/cartorio.py:709`, `:680`). Contudo a unidade `julgar_celula(agora=...)` ainda consulta relógio real para `hoje` e guarda de fim (`:1597`, `:1618`), antes de passar o `agora` fornecido (`:1621`). O corpo também chama `afastado_na_batida(c)` sem data (`:1056`), que delega com `dia=None` (`app/ponto/services/afastado_avisa.py:38`). Não são novidades demonstráveis de HEAD sem diff.
- Porta da célula evita nova gravação/evento com mesma ata/veredito/protestos (`app/ponto/portas/celula.py:687`) e mesmo carimbo (`:717`). Prova global de “duas chamadas = uma linha de trilha”: **[não verificado]**.

A cópia não contém `.git`; o diff exato HEAD/HEAD~1 permaneceu **[não verificado]**. Não executei suite nem acessei banco/log. [nao verificado]

## 31/10 00:21 · sessao 93fd0110e578 · O261-E-F5B · sonda · origem=Code

Transcrição do dado raspado fornecido, sem conferência em PROD. [nao verificado]

| Contador | Dias | Referência |
|---|---:|---|
| universo | 1844 | [nao verificado] |
| porta_fila | 810 | [nao verificado] |
| porta_pergunta | 169 | [nao verificado] |
| de_hoje_pra_frente | 0 | [nao verificado] |
| fio_morto_alcance | 617 | [nao verificado] |
| decisao_do_dia_pendente | 222 | [nao verificado] |
| aguarda_decisao_de_ausencia | 23 | [nao verificado] |
| escala_nao_descreve_o_dia | 2 | [nao verificado] |
| fio_do_admin | 1 | [nao verificado] |
| porta_exige_o_admin | 171 | [nao verificado] |

Os quatro baldes fora do fio estão definidos em [vigia_fio.py:502](/home/ronald/codex_ws/68821bbd7d83c65b55dde18bc3733e2ba5df9aa0/app/chamados/services/vigia_fio.py:502).

| rota | papel | Dias | Referência |
|---|---|---:|---|
| aguarda_decisao_de_ausencia | Aguardando decisao do DP | 14 | [nao verificado] |
| aguarda_decisao_de_ausencia | Aguardando documento do colaborador | 9 | [nao verificado] |
| decisao_do_dia_pendente | regularizar_batida | 222 | [nao verificado] |
| escala_nao_descreve_o_dia | fora da lista do admin | 2 | [nao verificado] |
| fio_do_admin | admin-batida | 1 | [nao verificado] |
| porta_fila | cobrar | 673 | [nao verificado] |
| porta_fila | cobrar,decidir | 18 | [nao verificado] |
| porta_fila | cobrar,validar | 1 | [nao verificado] |
| porta_fila | decidir | 65 | [nao verificado] |
| porta_fila | decidir,validar | 3 | [nao verificado] |
| porta_fila | validar | 50 | [nao verificado] |
| porta_pergunta | cobrar | 127 | [nao verificado] |
| porta_pergunta | cobrar,validar | 1 | [nao verificado] |
| porta_pergunta | historico | 8 | [nao verificado] |
| porta_pergunta | validar | 33 | [nao verificado] |
| sem_porta | nenhuma porta | 617 | [nao verificado] |
| **Soma** | | **1844** | [nao verificado] |

**A soma bate: 1844 = universo 1844; diferença 0.** [nao verificado]

`810 + 169 + 0 + 617 + 222 + 23 + 2 + 1 = 1844`. [nao verificado]

| Papel | Dias na tabela fornecida | Referência |
|---|---:|---|
| admin-cadastro | 0 | [nao verificado] |
| admin-batida | 1 | [nao verificado] |
| fora da lista do admin | 2 | [nao verificado] |
| lista sem resposta | 0 | [nao verificado] |

## 31/10 00:21 · sessao 540e573b89ba · O261-E-F5B · verificador · origem=Code

Há achados no código atual. A atribuição exata aos três commits e o resultado dos testes no pai ficam **[nao verificado]**: a cópia não contém `.git`. Não consultei banco nem logs; casos abaixo são contraprovas construídas a partir do código. [nao verificado]

1. **Falhas encontradas e classes pedidas**

- **Falha da lista não chega ao contador.** No ramo de folga contestada validada, `dono_fora_do_fio` consulta `papel_do_dia` e retorna antes de `dia_do_admin`. Se a lista falha, o papel vira `"lista sem resposta"`, mas somente `dia_do_admin` registra o dia em `_sem_resposta`. Caso concreto: único dia retido, sem porta, com `FOLGA_CONTESTA` validada e lista indisponível; o alcance publica a falha, porém o comando carimba `fio_do_admin_sem_resposta=0`. Isso contradiz o contrato de contar os dias consultados sem resposta. Referências: `app/chamados/services/vigia_fio.py:192`, `:224`, `:231`, `:242`, `:292-295`; `app/chamados/management/commands/vigiar_fio_morto.py:169`.

- **A porta pode declarar alcançável um chamado ocultado pelo painel padrão.** Um chamado aberto, com célula e `silenciado_ate` vencido, entra em `qs_na_fila`; a partição atribui `historico`; o painel padrão exclui esse verbo. O juiz aceita a entrada e devolve `porta_fila`, `fio='chamado'`. A fórmula literal do corte é seguida, mas sua equivalência com uma porta visível no painel falha no código atual. Referências: `app/chamados/services/painel.py:186-188`, `:326-329`; `app/chamados/services/verbos.py:45-48`; `app/chamados/juizes.py:1213`, `:1220`. A introdução dessa inconsistência nestes commits é **[nao verificado]**.

- **Fonte única dos donos ainda duplicada.** O vigia declara `DONOS_DA_MESA_DO_ADMIN=('BATIDA','CADASTRO')`; pautas declara `DONOS_DE_PAUTA=('CADASTRO','BATIDA')`. Ambos implementam a separação L-099 entre trabalho do admin e estrutura. Hoje os conjuntos coincidem; são duas declarações independentes. Uma alteração em apenas uma delas permite retirar um dia do fio morto sem a mesma classificação na emissão de pautas. Referências: `app/chamados/services/vigia_fio.py:168`, `:226`, `:272-275`; `app/ponto/services/pautas_dos_discordantes.py:41`, `:136`; `CLAUDE.md:311-316`.

- **O selo de unicidade não alcança essa duplicação.** O teste “predicado existe UMA vez” inspeciona exclusivamente o AST de `vigia_fio.py`; portanto passa com a declaração paralela de pautas presente. Prova unicidade local, não fonte única entre os consumidores. Referências: `app/chamados/tests/test_o261e_f5_alcance.py:235-246`; `app/ponto/services/pautas_dos_discordantes.py:41`.

- **Há relógio fora da borda na fonte consumida.** O comando captura `agora/hoje` uma vez e passa a data à lista. Entretanto `cadastro_realidade.lista` chama `_jornada_que_mente` sem essa data; o helper usa `date.today()` e corte literal 21. Caso: rodada com `hoje` injetado, ou atravessando meia-noite, pode montar C2 para outra janela e alterar a presença do colaborador usada na causa `d`. Referências: `app/chamados/management/commands/vigiar_fio_morto.py:158-168`; `app/chamados/services/vigia_fio.py:214-216`, `:630`; `app/escala/services/cadastro_realidade.py:186-187`, `:518`. É problema da fonte atual; introdução no diff **[nao verificado]**.

- **Restaram leitores com afirmações que a nova saída não garante.** `linhas()` continua afirmando “o dono é o CADASTRO” para todo `escala_nao_descreve_o_dia`, embora o papel agora possa ser `admin-batida` ou `fora da lista do admin`. Além disso, `DESTRAVA['chamado']` afirma “não há pergunta aberta”, mas o juiz admite pergunta viva sem crédito, com verbo `decidir` e fio `chamado`. Casos concretos: o cenário BATIDA já usado pelo teste de rótulo; uma pergunta viva excluída do crédito por materialização de fato. Referências: `app/chamados/services/vigia_fio.py:830-832`; `app/chamados/tests/test_o261e_f5_alcance.py:334-338`; `app/folha/fora_da_folha.py:43`; `app/chamados/juizes.py:1200-1202`, `:1175-1181`.

Nas demais classes:

| Classe | Resultado |
|---|---|
| Segundo juiz duplicando `porta_alcancavel_do_dia` | **Sem achado** nos leitores examinados: folha e alcance chamam o registrado. `app/folha/fora_da_folha.py:54-67`; `app/chamados/services/vigia_fio.py:595`; `app/core/juizes.py:253`. |
| Segundo escritor introduzido pela mudança | **Sem achado** nos caminhos examinados; alimentação e juiz consultam as fontes. Há múltiplos sítios anteriores de escrita dos carimbos, discriminados abaixo. `app/chamados/juizes.py:1129-1163`, `:1208-1220`. |
| Tabela verbo→fio na folha | **Sem achado**: está no juiz; a folha lê `porta['fio']`. `app/chamados/juizes.py:1170-1181`; `app/folha/fora_da_folha.py:65-67`. |
| Ordem das decisões do alcance | **Sem achado**: porta antes de hoje/futuro, depois os baldes de lei. `app/chamados/services/vigia_fio.py:595-609`. |
| Pouso antes dos pré-requisitos | **[nao verificado]**: sem histórico, execução da suíte ou evidência de pouso. | [nao verificado]
| Teto escondido no alcance | **Sem achado**: contador sobe antes do corte; `sem_porta` sempre entra; demais omissões são contadas. `app/chamados/services/vigia_fio.py:583-589`. |

2. **Quem escreve e quem lê**

O quadro registra escritores e leitores localizados na cadeia auditada e em consumidores externos a ela; exaustividade de todos os usos do repositório **[nao verificado]**. Quais arquivos efetivamente estão fora do diff também é **[nao verificado]**, pela ausência do histórico.

| Campo | Escritores localizados | Leitores localizados |
|---|---|---|
| `ChamadoColaborador.status_local` | Transição em `app/chamados/models.py:185-193`; nascimento em `:699-704`, `:834-851`. | Fila em `app/chamados/services/painel.py:155`, `:187`; modal em `app/chamados/services/fio.py:385-399`; sinais em `app/chamados/signals.py:161-180`; API em `app/api/views.py:1047`, `:1145`; calendário em `app/colaboradores/services/calendario.py:557`. O novo juiz recebe o resultado da fila em `app/chamados/juizes.py:1144-1148`. |
| `silenciado_ate` | Arquivar/desarquivar em `app/chamados/services/arquivo.py:166-171`, `:201-208`. | Fila em `app/chamados/services/painel.py:188`; verbo em `app/chamados/services/verbos.py:42-48`; propriedade em `app/chamados/models.py:304-307`; sinal em `app/chamados/signals.py:237-242`; contador em `app/chamados/services/fila.py:122-127`; pesquisa em `app/chamados/views_arquivar.py:156-157`. |
| `ChamadoColaborador.celula` | Nascimento em `app/chamados/models.py:716-722`, `:829-831`; ligação posterior em `app/chamados/services/vinculos.py:92-99`. | Dia do fato e alimentação em `app/chamados/juizes.py:89-90`, `:1146-1148`; reconciliação em `app/chamados/models.py:866-868`; modal pelo dia do juiz em `app/chamados/services/fio.py:365-374`. |
| `respondida_em` / `resposta_colab` | Resposta em `app/chamados/services/disputa_emissao.py:1247-1266`; reabertura em `:1358-1377`; resposta automática/reparo em `:1392-1410`; ressuscitação em `app/chamados/models.py:1258-1259`; complementação dos carimbos em `app/chamados/services/materializacao.py:511-529`, `:1190-1198`, e `app/chamados/services/disputa_emissao.py:1570-1577`. | C6 em `app/chamados/juizes.py:1020-1032`; resposta vazia em `:1126`; SQL em `app/chamados/models.py:2342-2355`; créditos em `app/chamados/services/raiox.py:133-149`; API em `app/api/views.py:1173-1175`, `:2632-2634`; modal em `app/templates/chamados/partials/modal_fio.html:210`; prévia em `app/folha/previa.py:142`; cartório em `app/ponto/services/cartorio.py:1263`; ficha em `app/colaboradores/services/ficha.py:171`. |
| `validada_em` | Validação/devolução em `app/chamados/services/validacao.py:127-133`, `:180-190`; nova resposta desfaz em `app/chamados/services/disputa_emissao.py:1250-1266`; reabertura em `:1358-1377`; ressuscitação em `app/chamados/models.py:1258`; também carimbada por `app/chamados/services/materializacao.py:514-529`, `:1194-1198`, e `app/chamados/services/disputa_emissao.py:1574-1577`. | C6 em `app/chamados/juizes.py:1006-1007`; estado/SQL em `app/chamados/models.py:2204-2211`, `:2353`; ações em `app/chamados/services/acoes_disputa.py:341`; procedência em `app/ponto/services/procedencia_batida.py:74`; placar em `app/chamados/services/painel.py:66-67`. |
| `materializada_em` | Portas do modelo em `app/chamados/models.py:2228`, `:2242`, `:2267`, `:2282`, `:2302`, `:2312`; ressuscitação em `:1258`; reconciliador em `app/chamados/reconciliador.py:402`, `:617`; fio em `app/chamados/services/fio.py:846`; relavra em `app/chamados/services/relavra_via.py:231`; materialização em `app/chamados/services/materializacao.py:509`, `:858`, `:1043`, `:1149`, `:1339`; emissão em `app/chamados/services/disputa_emissao.py:1562`, `:1644`. | C6 em `app/chamados/juizes.py:1015`, `:1044`, `:1050-1052`, `:1076-1083`; clique morto/crédito em `app/chamados/services/raiox.py:133-134`, `:173-185`; estado em `app/chamados/models.py:2206-2209`; procedência em `app/ponto/services/procedencia_batida.py:74`; validação em `app/chamados/services/validacao.py:182`; placar em `app/chamados/services/painel.py:67`. |
| `chamado_destino` | Nascimento/reuso em `app/chamados/models.py:2160-2182`; reapontamento em `app/chamados/services/fio.py:785-800`. | Âncora em `app/chamados/services/fio.py:117`; crédito em `app/chamados/services/raiox.py:135-147`; API em `app/api/views.py:1046`, `:2626`, `:2642-2644`; materialização em `app/chamados/services/materializacao.py:939`, `:1021`, `:1169-1172`; app em `app/chamados/services/pergunta_no_app.py:116`, `:139-142`; validação em `app/chamados/services/validacao.py:788`; tranca em `app/chamados/services/competencia_trancada.py:115`. |

Assim, a descrição de `validar_pergunta` como “escritor único de `validada_em`” não descreve todos os sítios atuais: materialização e emissão também atribuem esse carimbo. Isso é constatável no código; ser uma mudança destes commits é **[nao verificado]**. Referências: `app/docs/JUIZES.md:38-39`; `app/chamados/services/materializacao.py:514-529`; `app/chamados/services/disputa_emissao.py:1574-1577`.

| Valor derivado | Quem produz | Quem lê |
|---|---|---|
| Mapa de `particionar_verbos` | Crédito + natureza + silêncio em `app/chamados/services/verbos.py:31-59`. | Alimentação do juiz em `app/chamados/juizes.py:1153`; painel em `app/chamados/services/painel.py:230`, `:320`, `:326`, `:381`, `:472`; lente em `app/chamados/services/verbos.py:174`; inteligência em `app/chamados/services/na_fila.py:19-25`; ficha em `app/colaboradores/services/ficha.py:158`; mensageria em `app/api/views_mensageria.py:395`, `:453`, `:1143`; cobrar/arquivar em `app/chamados/views_cobrar.py:78`, `app/chamados/views_arquivar.py:116`. Navbar lê contagens em `app/core/context_processors.py:89-90`. |
| Retorno da porta: `rota`, `verbos`, `fio` | Juiz em `app/chamados/juizes.py:1212-1220`; escada em `:1170-1181`. | Folha lê `fio` em `app/folha/fora_da_folha.py:67`; `o_que_segura` lê IDs de `verbos` em `:193`; alcance lê rota/verbos em `app/chamados/services/vigia_fio.py:595-600`. |
| `FIO_SEM_PORTA` | Declaração em `app/chamados/juizes.py:1172`. | Folha em `app/folha/fora_da_folha.py:65-67`; seleção de pautas em `app/ponto/services/pautas_dos_discordantes.py:59`, `:76`. |
| Fio de `_fio_do_dia` | Leitura da porta em `app/folha/fora_da_folha.py:59-67`. | Grupos/contadores em `:148-166`; detalhe em `:191-198`; pautas em `app/ponto/services/pautas_dos_discordantes.py:76`. |
| Papel de `dono_fora_do_fio` | Permissão de Resolver dia, estado de ausência ou papel da lista em `app/chamados/services/vigia_fio.py:287-295`. | Alcance em `:608-612`; agregação/lista em `:583-587`; relatório em `:672-676`. O censo antigo chama sem papel em `:398`. |
| `ListaDoAdmin.dia_na_lista` | Mapa carregado por `lista` + `dias_na_lista` em `app/chamados/services/vigia_fio.py:214-215`, consultado em `:228-234`. | `papel_do_dia` em `:240`; causas do alcance em `:614`, `:630`, `:634`. |
| `ListaDoAdmin.papel_do_dia` | Dono lavrado formatado ou sentinela em `app/chamados/services/vigia_fio.py:240-243`. | Ramos escala/admin em `:293`, `:295`. |

**Há impacto indireto em outros leitores.** Grupo e fio alimentam `montar_linhas`, que grava `dono` no retrato; a lavra persiste esse resultado e a tela lê o retrato gravado. Portanto uma classificação diferente chega aos contadores, PDF e raio-X após nova lavra, sem exigir mudança nesses consumidores. Referências: `app/folha/retrato_folha.py:1156-1188`; `app/folha/management/commands/lavrar_retrato_folha.py:65-76`; `app/folha/views_retrato.py:196`, `:318`, `:331`, `:337`. A diferença efetiva de valores contra o pai é **[nao verificado]**.

3. **Chamado `registrado` com célula**

Sozinho, sem outra porta por pergunta, fica em **fio `nada`**. `registrado` pertence a `VIVOS`, mas não a `COBRANCA`; a fila o exclui, o juiz retorna `None` e a folha usa `FIO_SEM_PORTA`. Referências: `app/chamados/catalogo/motor.py:24-29`; `app/chamados/services/painel.py:187`; `app/chamados/juizes.py:1218-1219`; `app/folha/fora_da_folha.py:65-67`.

Ele continua acessível pelo drill `?status=registrado` e pelo modal que carrega todos os chamados do colaborador. O modal pode oferecer resposta conforme permissão; geofence registrado pode ser puxado para análise e decidido. Referências: `app/chamados/services/painel.py:304-305`; `app/chamados/services/fio.py:174-176`; `app/templates/chamados/partials/modal_fio.html:643-650`; `app/chamados/services/acoes_chamado.py:229-233`; `app/chamados/services/fio.py:527-540`.

**Sem achado contra a definição literal do corte nesse caso:** o juiz exclui expressamente acesso manual por modal/URL do conceito de porta adotado. Referência: `app/chamados/juizes.py:1203-1205`.

4. **`pergunta_sem_acao_possivel`: outra pergunta**

Ela responde: **“esta pergunta já foi materializada de fato, tornando o clique inútil?”** Exige `materializada_em`; lê o veredito lavrado e, sem ele, consulta `_pergunta_materializada_de_fato`. Não pergunta por fila, âncora ou alcance do dia. Referência: `app/chamados/services/raiox.py:151-185`.

O chamador direto localizado em produção é `montar_chamados_fio`: combina pergunta viva com ausência de clique morto para definir `tem_pergunta_viva`. A partição usa a versão do crédito em SQL, não chama diretamente essa função. Referências: `app/chamados/services/fio.py:275-281`; `app/chamados/services/raiox.py:133-149`; `app/chamados/services/verbos.py:31`.

**Sem achado de segundo juiz da alcançabilidade aqui.** Fazer essa função delegar à porta confundiria materialização da pergunta com existência de acesso ao dia, perguntas distintas nos contratos atuais. Referências: `app/chamados/services/raiox.py:152-162`; `app/chamados/juizes.py:1187-1202`.

5. **Fonte dos rótulos**

Tanto `escala_nao_descreve_o_dia` quanto `fio_do_admin`, quando solicitados com papel, consultam `ListaDoAdmin.papel_do_dia`. A classe carrega a mesma `cadastro_realidade.lista` e extrai os dias por `dias_na_lista`; o papel é `admin-<dono lavrado>`, ou informa ausência/falha da lista. Referências: `app/chamados/services/vigia_fio.py:214-215`, `:236-243`, `:293-295`.

O dono diário vem de `dono_do_dia` das assinaturas datadas, incluindo C9; sua lavra recebe o dono do oráculo e o persiste em snapshot. Referências: `app/escala/services/cadastro_realidade.py:595-604`, `:655-670`; `app/ponto/services/teto_na_lista.py:86`, `:110-116`.

**Há conjunto paralelo de donos**, conforme o achado do item 1: vigia e pautas repetem BATIDA/CADASTRO. Também permanece o texto agregado fixo de CADASTRO, embora o papel detalhado siga a lista. Referências: `app/chamados/services/vigia_fio.py:168`, `:830-832`; `app/ponto/services/pautas_dos_discordantes.py:41`.

6. **Mordida dos testes** [nao verificado]

Não executei a suíte Django, que depende de banco **[nao verificado]**. O vermelho efetivo no pai e a identificação exata de novos/alterados são **[nao verificado]**. Abaixo está a avaliação por leitura; ausência de símbolo no pai produziria erro de importação, não prova semântica da cura. [nao verificado]

| Teste | Mordida e contrafactual | [nao verificado]
|---|---|
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:108` | Exige nenhuma porta no cenário p#34375. Sem juiz no pai: falha mecânica; RED semântico isolado não demonstrado. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:112` | **Morde:** exige folha `nada`; ficaria vermelho contra folha que transforma pergunta viva respondida sob terminal em `admin`. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:117` | **Morde pela folha:** vigia deve dizer causa `c` e folha `nada`; igualdade não basta, há resultados explícitos. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:123` | **Morde:** mantém portas A/B, créditos corretos e `cobrar` vencendo no fio. Pai sem juiz: apenas erro mecânico. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:136` | Caracterização positiva de chamado sem pergunta; não demonstra necessidade da cura anterior. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:141` | Caracterização positiva de resposta→validar/admin; folha antiga que já desse admin passaria nessa parte. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:147` | **Morde:** registrado deve dar `nada`; vermelho contra folha que o aceita por estar em `VIVOS`. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:153` | Propriedade lote=avulso; detecta divergência entre alimentações, não o defeito antigo por si. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:165` | **Morde ordem da escada** e tratamento de `None`; ausência do helper no pai seria erro mecânico. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:175` | Pré-condição: registrado fora da fila. Deve continuar verde onde essa regra já existia. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:185` | Morde falta de registro; não testa alcançabilidade. |
| `app/folha/tests/test_haiku_fora_da_folha.py:93` | **Forte:** injeta `fio='xyz'` sob cobrar; tradução paralela devolveria colab. |
| `app/folha/tests/test_haiku_fora_da_folha.py:114` | Morde tabela literal de verbos na folha; não exclui toda derivação possível por outros mecanismos. |
| `app/folha/tests/test_haiku_fora_da_folha.py:160` | Morde retorno a `VIVOS`/`.viva`; parte da conferência de fontes é apenas presença textual. |

Para o arquivo de alcance, cujo subconjunto alterado não pode ser determinado sem o diff:

| Teste | O que morde; vermelho no pai | [nao verificado]
|---|---|
| `app/chamados/tests/test_o261e_f5_alcance.py:110` | Universo positivo, dia morto e partição. Pai **[nao verificado]**. |
| `app/chamados/tests/test_o261e_f5_alcance.py:119` | Porta da fila retira dia do morto. Pai **[nao verificado]**. |
| `app/chamados/tests/test_o261e_f5_alcance.py:126` | Pergunta no app com âncora na fila abre porta. Pai **[nao verificado]**. |
| `app/chamados/tests/test_o261e_f5_alcance.py:134` | Silêncio vigente vira causa `a`; não cobre silêncio vencido. Pai **[nao verificado]**. |
| `app/chamados/tests/test_o261e_f5_alcance.py:141` | Registrado sem porta vira causa `a`. Pai **[nao verificado]**. |
| `app/chamados/tests/test_o261e_f5_alcance.py:146` | Respondida vazia vira causa `b`. Pai **[nao verificado]**. |
| `app/chamados/tests/test_o261e_f5_alcance.py:154` | Respondida sob terminal vira causa `c`. Pai **[nao verificado]**. |
| `app/chamados/tests/test_o261e_f5_alcance.py:161` | Pendente sob terminal recebe detalhe próprio. Pai **[nao verificado]**. |
| `app/chamados/tests/test_o261e_f5_alcance.py:173` | Colaborador listado, dia ausente: causa `d`. Pai **[nao verificado]**. |
| `app/chamados/tests/test_o261e_f5_alcance.py:179` | BATIDA sai para admin; ESTRUTURA permanece morto. Pai **[nao verificado]**. |
| `app/chamados/tests/test_o261e_f5_alcance.py:190` | Âncora não basta sem pergunta no app. Pai **[nao verificado]**. |
| `app/chamados/tests/test_o261e_f5_alcance.py:198` | Falha da lista não apaga dia morto; não cobre folga validada. Pai **[nao verificado]**. |
| `app/chamados/tests/test_o261e_f5_alcance.py:204` | Resolver dia vira balde próprio. Pai **[nao verificado]*

[truncado em 20000 de 23682 caracteres]

## 31/10 00:19 · sessao 655f21619021 · - · - · origem=Ronald (tmux)

Achado confirmado: falha da lista no novo ramo `escala_nao_descreve_o_dia` não chega ao contador. Esse ramo retorna antes de `dia_do_admin` e usa `papel_do_dia` (`app/chamados/services/vigia_fio.py:293`); `dia_na_lista` devolve `None` na falha sem registrar o dia (`app/chamados/services/vigia_fio.py:231`), e o rótulo vira `lista sem resposta` (`app/chamados/services/vigia_fio.py:242`). O único registro em `_sem_resposta` fica no ramo pulado (`app/chamados/services/vigia_fio.py:224`). Caso: folga contestada validada, lista indisponível, único dia da rodada; imprime falta da lista, mas o command grava `fio_do_admin_sem_resposta=0` (`app/chamados/management/commands/vigiar_fio_morto.py:169`). Contraria o contrato de contar dias distintos que perguntaram e não receberam resposta (`app/chamados/services/vigia_fio.py:192`). Reproduzido somente em memória com a classe extraída por AST: `papel_do_dia(...) → 'lista sem resposta'; sem_resposta → 0`; a reprodução não escreveu arquivos [nao verificado por arquivo:linha].

O teste novo de rótulo cobre CADASTRO, BATIDA e lista vazia; falta cobrir lista indisponível (`app/chamados/tests/test_o261e_f5_alcance.py:327`). Os testes de falha existentes usam dia sem a folga validada e percorrem `dia_do_admin`, portanto não pegam a regressão (`app/chamados/tests/test_o261e_f5_alcance.py:223`, `app/chamados/tests/test_o261e_f5_alcance.py:248`, `app/chamados/tests/test_o261e_f5_alcance.py:266`).

Outro achado: o teste chamado “predicado existe UMA vez” só analisa o AST de `vigia_fio.py` (`app/chamados/tests/test_o261e_f5_alcance.py:239`). Ele passa com o mesmo conjunto declarado em `DONOS_DA_MESA_DO_ADMIN` e `DONOS_DE_PAUTA` (`app/chamados/services/vigia_fio.py:168`, `app/ponto/services/pautas_dos_discordantes.py:41`); ambos dizem aplicar a separação CADASTRO/BATIDA versus ESTRUTURA da L-099 (`app/chamados/services/vigia_fio.py:273`, `app/ponto/services/pautas_dos_discordantes.py:17`). O selo prova unicidade local; não a fonte única entre leitores.

Inventário dos testes específicos do juiz — são **11**, corrigindo minha contagem inicial. Resultado histórico no pai e lista exata dos alterados: [nao verificado], porque a cópia não contém histórico Git. Abaixo, a mordida verificável pela leitura e o contrafactual semântico; erro por símbolo inexistente não conta como RED semântico. [nao verificado]

| Arquivo:linha | Teste / mordida | [nao verificado]
|---|---|
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:108` | Ricardo, juiz retorna nenhuma porta. Se juiz não existe no pai, falha por importação; sozinho não demonstra RED semântico. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:112` | Ricardo, folha deve dizer `nada`. RED semântico contra folha que toma pergunta viva respondida por fio admin; essa divergência antiga é descrita no próprio caso (`:8`), mas pai executado [nao verificado]. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:117` | Mesmo caso, folha e vigia concordam. A metade do vigia já corresponde ao comportamento antigo descrito em `:10`; quem deve ficar vermelho é a folha (`:120`). |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:123` | A sem pergunta + B com pergunta: exige ambos os verbos reais, `decidir` e `cobrar`, e fio `colab`. Morde perda de uma porta e crédito recalculado para o chamado errado. No pai, eventual importação ausente deve ser separada dessa mordida. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:136` | Só chamado da fila: fio `chamado`. Caracterização positiva; não prova que a correção é necessária. Novo símbolo ausente daria falha mecânica. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:141` | Resposta sob âncora na fila: `validar`/`admin`. Caracterização positiva; uma folha antiga que já desse admin também passaria na parte comportamental. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:147` | `registrado` com célula: nenhuma porta e folha `nada`. Morde folha que usa VIVOS e classifica o registrado como chamado. Histórico concreto [nao verificado]. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:153` | Alimentação avulsa e lote iguais. Morde divergência entre as duas vias; é propriedade da implementação nova, não RED semântico do defeito anterior. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:165` | Escada no juiz: cobrar vence validar, validar vence decidir, outro/None dá chamado. Morde ordem errada; ausência de `fio_pelos_verbos` no pai daria só erro de importação. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:175` | Registrado fora de COBRANCA e da fila. É condição/caracterização; deve ficar verde numa versão anterior que já tenha essa definição (`app/chamados/catalogo/motor.py:29`). Pai [nao verificado]. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:185` | Registro em `core.JUIZES`. Morde falta de registro; não testa a regra de alcançabilidade. |

Testes da folha diretamente ligados à troca: [nao verificado]

| Arquivo:linha | Mordida / contrafactual |
|---|---|
| `app/folha/tests/test_haiku_fora_da_folha.py:93` | Forte: injeta fio `xyz` sob verbo cobrar; uma tradução paralela devolveria colab. Também testa None e número de consultas ao juiz (`:101`). Vermelho semântico contra leitor que ainda traduz verbo; pai concreto [nao verificado]. |
| `app/folha/tests/test_haiku_fora_da_folha.py:114` | AST proíbe literais cobrar/validar/decidir e atributo `_FIO_DO_VERBO`. Morde a tabela literal anterior descrita no contexto; não impede toda derivação possível por outros mecanismos. Pai concreto [nao verificado]. |
| `app/folha/tests/test_haiku_fora_da_folha.py:160` | Exige import do juiz e proíbe import VIVOS, atributo `.viva` e certos status textuais. Morde retorno ao predicado próprio antigo. `motivos_retencao_celula` e `classificar_export` são conferidos por presença textual, inclusive em comentários/docstrings (`:174`). Pai concreto [nao verificado]. |

Inventário completo do arquivo de alcance, para consolidar apenas o que efetivamente estiver no diff. Em **todos**, vermelho no pai permanece [nao verificado]; não há como distinguir novos de preexistentes nesta cópia.

| `app/chamados/tests/test_o261e_f5_alcance.py:linha` | O que efetivamente morde |
|---|---|
| `:110` | Universo positivo, dia sem porta conta um, partição e competência. |
| `:119` | Mesmo dia com chamado na fila sai do contador morto. |
| `:126` | Pergunta no app sob âncora na fila é porta. |
| `:134` | Silenciado não é porta e recebe causa a. |
| `:141` | Registrado não é porta e recebe causa a. |
| `:146` | Respondida com texto vazio não é porta, causa b. |
| `:154` | Respondida sob destino terminal recebe causa c. |
| `:161` | Pendente sob terminal recebe detalhe nomeado. |
| `:173` | Colaborador presente na lista sem o dia recebe causa d. |
| `:179` | BATIDA na lista sai para admin; ESTRUTURA permanece morto. |
| `:190` | Âncora na fila não basta quando pergunta fora do app. |
| `:198` | Falha da lista não apaga o dia morto; não contém folga validada. |
| `:204` | Resolver dia oferecido vira decisão pendente. |
| `:212` | Censo e alcance compartilham classificação e uma carga da lista. |
| `:223` | Mesma falha nos dois universos conta uma vez; não contém folga validada. |
| `:235` | Unicidade do predicado/carregador apenas dentro de vigia_fio; não morde cópia em pautas. |
| `:248` | Command lê a falha depois dos dois universos; não contém folga validada. |
| `:266` | Command compartilha mesma instância entre os dois censos. |
| `:287` | Papel alterna cobrar→validar quando a pergunta recebe resposta; consulta partição real. |
| `:301` | Forte: juiz injetado com verbo xyz deve produzir rótulo xyz; morde tabela própria do vigia. |
| `:310` | Rótulos sem porta, Resolver dia, hoje/futuro e donos CADASTRO/BATIDA. |
| `:327` | Rótulo escala segue dono da lista e acusa dia ausente; falta exceção da lista. |
| `:340` | A constante de permissão aparece em chamada textual do drawer. Morde mudança literal divergente; não testa a permissão em execução. |
| `:347` | Rótulo de ausência segue STATUS_CHOICES para dois estados. |
| `:357` | Limites 0/1/500 mantêm soma e omitidos; sem porta nunca some. |
| `:374` | Âncora registrada fora da fila também é véu. |
| `:381` | Dia de hoje sem porta é balde próprio e mantém soma. |
| `:387` | Porta da fila vence teto de hoje. |
| `:393` | Porta da pergunta vence teto de hoje. |
| `:399` | Validada não entra nas causas de pergunta viva. |
| `:408` | Dia presente sem dono não vira CADASTRO por presunção. |
| `:413` | Universo vazio marca cego. |
| `:418` | Caracteriza diferença de universo entre censo antigo e alcance. |
| `:424` | Competência entregue sai do universo por autoridade existente. |
| `:431` | Command carimba contador e esperado zero; explicitamente mantém alcance fora do alarme enquanto houver passivo (`:443`). |

Outros itens pedidos na minha subauditoria: não identifiquei teto oculto no contador novo de alcance; contador é incrementado antes do corte e omitidos são declarados (`app/chamados/services/vigia_fio.py:583`). A ordem porta antes de hoje e baldes está no código (`app/chamados/services/vigia_fio.py:595`, `app/chamados/services/vigia_fio.py:602`, `app/chamados/services/vigia_fio.py:608`) e tem testes nas linhas `:387` e `:393` acima. Pouso efetivo, suite verde e pré-requisito cumprido em execução: [nao verificado].

## 31/10 00:19 · sessao f43bd3a6ba89 · - · - · origem=Ronald (tmux)

Achados e censo, somente sobre a cópia atual; quais linhas pertencem exatamente ao diff e o comportamento no pai são [nao verificado], pois a cópia não contém `.git`.

1. **Falha da lista escapa ao contador no novo rótulo de escala.** `dono_fora_do_fio` retorna no ramo de folga validada, chamando apenas `papel_do_dia`, antes de chegar a `dia_do_admin` (`app/chamados/services/vigia_fio.py:292`, `:293`, `:294`). `papel_do_dia` recebe `None` de `dia_na_lista` e retorna `"lista sem resposta"` sem registrar o dia; quem adiciona a `_sem_resposta` é exclusivamente `dia_do_admin` (`app/chamados/services/vigia_fio.py:228`, `:231`, `:240`, `:242`, `:223`, `:224`).
   
   Caso concreto: único dia retido, sem porta, com FOLGA_CONTESTA validada; a lista lança exceção. O alcance escreve `escala_nao_descreve_o_dia|lista sem resposta`, mas `lista.sem_resposta` continua 0 e o comando carimba `fio_do_admin_sem_resposta=0` (`app/chamados/services/vigia_fio.py:608`, `:612`; `app/chamados/management/commands/vigiar_fio_morto.py:169`, `:216`). Isso contraria o contrato de contar dias que perguntaram e não tiveram resposta (`app/chamados/services/vigia_fio.py:190`, `:192`, `:606`).
   
   Verificação isolada sem escrita: compilei por AST a classe real, simulei lista já carregada sem resposta (`_carregada=True`, `_dias=None`); `papel_do_dia` retornou `"lista sem resposta"` com contador 0, enquanto `dia_do_admin` elevou o mesmo contador a 1. A cadeia do ramo de folga é estática nos endereços acima. O teste de rótulo não cobre falha; cobre apenas CADASTRO/BATIDA/fora da lista (`app/chamados/tests/test_o261e_f5_alcance.py:327`, `:332`, `:334`, `:337`).

2. **Conjunto L-099 declarado em paralelo, ainda sem divergência de conteúdo.** O vigia declara `DONOS_DA_MESA_DO_ADMIN=('BATIDA','CADASTRO')`; pautas declara `DONOS_DE_PAUTA=('CADASTRO','BATIDA')` (`app/chamados/services/vigia_fio.py:168`; `app/ponto/services/pautas_dos_discordantes.py:41`). Os consumidores são o predicado de retirada de fio morto e o alvo de emissão de pauta, respectivamente (`app/chamados/services/vigia_fio.py:226`; `app/ponto/services/pautas_dos_discordantes.py:136`). Ambos afirmam realizar a mesma separação L-099 CADASTRO/BATIDA para lista do admin (`app/chamados/services/vigia_fio.py:272`; `app/ponto/services/pautas_dos_discordantes.py:17`; `app/docs/LEIS.md:122`).
   
   O selo “predicado existe UMA vez” só lê AST de `vigia_fio`, logo não encontra a cópia de pautas; uma alteração futura em apenas um conjunto passa por esse selo (`app/chamados/tests/test_o261e_f5_alcance.py:238`, `:239`, `:240`, `:245`). É duplicação comprovada; divergência atual entre os conjuntos: sem achado pelos valores citados.

3. **Relógio fora da borda na fonte consumida.** O comando captura uma única borda temporal e passa `hoje` à lista (`app/chamados/management/commands/vigiar_fio_morto.py:158`, `:162`, `:166`; `app/chamados/services/vigia_fio.py:214`). Porém `cadastro_realidade.lista` chama `_jornada_que_mente(empresas)` sem essa data; o helper lê `date.today()` e fixa corte 21 (`app/escala/services/cadastro_realidade.py:518`, `:186`, `:187`).
   
   Caso: rodada com `hoje` injetado ou atravessando meia-noite pode montar C2 de outra janela. C2 altera presença do colaborador na lista, presença capturada em `_colabs`, que decide a causa `d` do alcance quando o dia está fora da lista (`app/escala/services/cadastro_realidade.py:215`; `app/chamados/services/vigia_fio.py:216`, `:248`, `:630`). Trata-se de problema preexistente na fonte; introdução nesses três commits [nao verificado].

4. **Outro leitor ainda publica CADASTRO fixo para escala.** O rótulo de papel do alcance segue a lista, inclusive BATIDA e ausência da lista (`app/chamados/services/vigia_fio.py:293`; `app/chamados/tests/test_o261e_f5_alcance.py:333`, `:335`, `:338`). Entretanto `linhas()` continua publicando “o dono é o CADASTRO, vai para a lista do admin” para todo o balde; o comando imprime esse texto (`app/chamados/services/vigia_fio.py:830`, `:831`; `app/chamados/management/commands/vigiar_fio_morto.py:173`). No cenário BATIDA ou fora da lista já existente no teste, coexistem o papel correto do alcance e a declaração fixa no relatório; avaliar como falha do leitor remanescente, pois o nome técnico do campo e o texto descritivo são distintos nos endereços citados.

Censo de fontes e leitores:

| Valor | Escritor/fonte | Leitores |
|---|---|---|
| Dono de cada dia da lista | `teto_na_lista.medir` recebe `_ins.dono(d)` e grava em `dono`; `lavrar` persiste o detalhe em snapshot (`app/ponto/services/teto_na_lista.py:86`, `:110`, `:113`, `:116`) | `_teto_do_dia` copia para `dono_do_dia`; a lista usa para o dono e a ação; `dias_na_lista` monta o mapa por dia (`app/escala/services/cadastro_realidade.py:310`, `:313`, `:598`, `:599`, `:670`) |
| `ListaDoAdmin._dias/_colabs` | `_carregar`: lista real e `dias_na_lista`, mais presença dos colaboradores (`app/chamados/services/vigia_fio.py:214`, `:215`, `:216`) | `dia_do_admin`, `dia_na_lista`, `colab_na_lista` (`app/chamados/services/vigia_fio.py:226`, `:234`, `:248`) |
| `dia_na_lista` | Consulta o mapa carregado, retorna presença/dono ou falha (`app/chamados/services/vigia_fio.py:228`, `:234`) | `papel_do_dia` e classificação das causas do alcance (`app/chamados/services/vigia_fio.py:240`, `:614`, `:630`, `:634`) |
| `papel_do_dia` | `"admin-"+dono.lower()` ou sentinelas fora da lista/sem resposta (`app/chamados/services/vigia_fio.py:240`, `:242`, `:243`) | Ramos `escala_nao_descreve_o_dia` e `fio_do_admin` de `dono_fora_do_fio` (`app/chamados/services/vigia_fio.py:293`, `:295`) |
| Papel de `dono_fora_do_fio` | Resolver dia: constante da permissão; ausência: `Ausencia.STATUS_CHOICES`; escala/admin: lista (`app/chamados/services/vigia_fio.py:287`, `:288`, `:289`, `:291`, `:293`, `:295`) | `censo` chama sem papel; alcance chama com papel e grava em lista e contador por rota/papel (`app/chamados/services/vigia_fio.py:398`, `:608`, `:612`, `:584`, `:587`) |
| Rótulos agregados do alcance | Contador por rota/papel (`app/chamados/services/vigia_fio.py:585`) | `linhas_alcance`, stdout do comando e detalhe do carimbo (`app/chamados/services/vigia_fio.py:672`, `:675`; `app/chamados/management/commands/vigiar_fio_morto.py:175`, `:218`) |

A tela e o PDF Cadastro x Realidade também leem a mesma `lista`, com empresas visíveis; não passam por `ListaDoAdmin` (`app/escala/views.py:487`, `:490`, `:492`, `:495`). Sua fonte compartilhada pode mudar o rótulo consumido pelo vigia sem mudar os leitores; quais desses leitores estão fora do diff [nao verificado].

Quanto a teto escondido: **sem achado no alcance**. Todo dia `sem_porta` entra na lista mesmo com `limite=0`; os demais cortes incrementam `alcance_dias_omitidos`; o contador por rota/papel sobe antes do corte (`app/chamados/services/vigia_fio.py:584`, `:585`, `:586`, `:589`). Os testes cobram soma do universo e omissão declarada (`app/chamados/tests/test_o261e_f5_alcance.py:364`, `:365`, `:366`).

## 31/10 00:19 · sessao 134634b540d5 · - · - · origem=Ronald (tmux)

| Campo | Escritores encontrados | Leitores relevantes |
|---|---|---|
| `ChamadoColaborador.status_local` | Transição: `app/chamados/models.py:185-193`; nascimento: `:699-704`, `:834-851`. | Fila: `app/chamados/services/painel.py:155`, `:186-188`; verbo/modal: `app/chamados/services/fio.py:385-399`; sinais/cartório: `app/chamados/signals.py:161-180`; API: `app/api/views.py:1047`, `:1145`; calendário: `app/colaboradores/services/calendario.py:557`; folha por nova alimentação: `app/chamados/juizes.py:1144-1148`. |
| `silenciado_ate` | `app/chamados/services/arquivo.py:166-171`, `:201-208`. | Fila: `app/chamados/services/painel.py:188`; partição: `app/chamados/services/verbos.py:42-48`; predicado: `app/chamados/models.py:304-307`; sinal: `app/chamados/signals.py:237-242`; contador: `app/chamados/services/fila.py:122-127`; pesquisa: `app/chamados/views_arquivar.py:156-157`. |
| `ChamadoColaborador.celula` | Nascimento: `app/chamados/models.py:716-722`, `:829-831`; ligação posterior: `app/chamados/services/vinculos.py:92-99`. | Dia do fato: `app/chamados/juizes.py:89-90`; alimentação da porta: `:1146-1148`; reconciliação: `app/chamados/models.py:866-868`; modal usa dia do juiz: `app/chamados/services/fio.py:365-374`. |
| `respondida_em`, `resposta_colab` | Resposta: `app/chamados/services/disputa_emissao.py:1247-1266`; automática: `:1392-1394`; reparo pela trilha: `:1407-1410`; reabertura: `:1358-1377`; ressuscitação zera carimbo: `app/chamados/models.py:1258-1259`; materialização também completa carimbo: `app/chamados/services/materializacao.py:511-529`, `:1190-1198`; fechamento automático: `app/chamados/services/disputa_emissao.py:1570-1575`, `:1655-1657`. | Juiz C6: `app/chamados/juizes.py:1020-1032`; filtro SQL: `app/chamados/models.py:2342-2355`; resposta vazia: `app/chamados/juizes.py:1126`; créditos: `app/chamados/services/raiox.py:133-149`; API: `app/api/views.py:1173-1175`, `:2632-2634`; modal: `app/templates/chamados/partials/modal_fio.html:210`; prévia folha: `app/folha/previa.py:142`; cartório: `app/ponto/services/cartorio.py:1263`; ficha: `app/colaboradores/services/ficha.py:171`. |
| `validada_em` | Assinatura/devolução: `app/chamados/services/validacao.py:127-133`, `:180-190`; nova resposta desfaz: `app/chamados/services/disputa_emissao.py:1250-1266`; reabertura: `:1358-1377`; ressuscitação: `app/chamados/models.py:1258`; materializadores também carimbam: `app/chamados/services/materializacao.py:514-529`, `:1194-1198`; fechamento automático: `app/chamados/services/disputa_emissao.py:1574-1575`. | C6 começa por ela: `app/chamados/juizes.py:1006-1007`; estado: `app/chamados/models.py:2204-2211`; SQL de vivas: `:2353`; ações: `app/chamados/services/acoes_disputa.py:341`; procedência: `app/ponto/services/procedencia_batida.py:74`; placar/ETag: `app/chamados/services/painel.py:66-67`. |
| `materializada_em` | Modelo: `app/chamados/models.py:2228`, `:2242`, `:2267`, `:2282`, `:2302`, `:2312`; ressuscitação `:1258`; reconciliação: `app/chamados/reconciliador.py:402`, `:617`; fio: `app/chamados/services/fio.py:846`; relavra: `app/chamados/services/relavra_via.py:231`; materializador: `app/chamados/services/materializacao.py:509`, `:858`, `:1043`, `:1149`, `:1339`; emissão: `app/chamados/services/disputa_emissao.py:1562`, `:1644`. | C6: `app/chamados/juizes.py:1015`, `:1044`, `:1050-1052`, `:1076-1083`; clique morto/crédito: `app/chamados/services/raiox.py:133-134`, `:173-185`; estado: `app/chamados/models.py:2206-2209`; procedência: `app/ponto/services/procedencia_batida.py:74`; validação: `app/chamados/services/validacao.py:182`; placar: `app/chamados/services/painel.py:67`. |
| `chamado_destino` | Nascimento/reuso: `app/chamados/models.py:2160-2182`; reapontamento: `app/chamados/services/fio.py:785-800`. | Âncora do fio: `app/chamados/services/fio.py:80-81`, `:117`; créditos: `app/chamados/services/raiox.py:135-147`; API: `app/api/views.py:1046`, `:2626`, `:2642-2644`; materialização: `app/chamados/services/materializacao.py:939`, `:1021`, `:1169-1172`; pergunta no app: `app/chamados/services/pergunta_no_app.py:116`, `:139-142`; validação: `app/chamados/services/validacao.py:788`; competência trancada: `app/chamados/services/competencia_trancada.py:115`. |

**Registrado com célula:** sozinho, sem outra porta por pergunta, fica em `nada`: `registrado` pertence a `VIVOS`, porém não a `COBRANCA` (`app/chamados/catalogo/motor.py:24-29`); `qs_na_fila` filtra `COBRANCA` (`app/chamados/services/painel.py:186-188`); alimentação só inclui esses chamados (`app/chamados/juizes.py:1144-1148`); porta vazia retorna `None` (`:1218-1219`); folha converte ausência de porta em `FIO_SEM_PORTA` (`app/folha/fora_da_folha.py:65-67`).

Há ação manual: o drill `?status=registrado` aceita status explícito (`app/chamados/services/painel.py:304-305`); modal admin carrega todos os chamados do colaborador (`app/chamados/services/fio.py:174-176`); caixa de resposta aparece para registrado, conforme permissão (`app/templates/chamados/partials/modal_fio.html:643-650`); geofence registrado pode ser puxado para análise e decidido (`app/chamados/services/acoes_chamado.py:229-233`, `app/chamados/services/fio.py:527-540`). O juiz exclui explicitamente acesso manual pelo modal de seu conceito de porta (`app/chamados/juizes.py:1203-1205`): portanto isso não prova, sozinho, descumprimento do corte.

**Achado candidato — porta da fila com verbo `historico`:** chamado aberto, célula acusadora e `silenciado_ate` passado entra em `qs_na_fila` (`app/chamados/services/painel.py:186-188`), mas a partição atribui `historico` (`app/chamados/services/verbos.py:45-48`), e a lista padrão o exclui (`app/chamados/services/painel.py:326-329`). A nova porta aceita esse chamado e devolve `fio='chamado'` (`app/chamados/juizes.py:1213`, `:1175-1181`, `:1220`). Assim o dia pode ser chamado de alcançável pela fila embora o painel padrão não mostre o cartão; acesso manual segue possível (`app/chamados/services/fio.py:174-176`). A inconsistência fila/partição já existe no código; sua introdução neste diff está [nao verificado].

**Leitores indiretos cujo resultado pode mudar sem alteração própria:** `_fio_do_dia` alimenta grupo, `dias_por_fio` e `por_fio` (`app/folha/fora_da_folha.py:148-167`); `montar_linhas` lê esse grupo e grava o fio em `dono` (`app/folha/retrato_folha.py:1156-1188`); command chama essa função e persiste o retrato (`app/folha/management/commands/lavrar_retrato_folha.py:65-76`); UI lê retrato gravado (`app/folha/views_retrato.py:196`), e contadores usam comprimento das linhas (`app/folha/retrato_folha.py:114-121`). Portanto o efeito chega após nova lavra também a barra, PDF e raio-X (`app/folha/views_retrato.py:318`, `:331`, `:337`).

**`o_que_segura['chamado']`:** entrega IDs do mapa de verbos da porta (`app/folha/fora_da_folha.py:193-198`), incluindo eventual âncora de pergunta em outro chamado, pois o juiz soma `chamado_que_lista(p)` à porta (`app/chamados/juizes.py:1214-1217`). O valor anterior ao diff está [nao verificado].

**Rótulo possivelmente inexato:** `DESTRAVA['chamado']` afirma “não há pergunta aberta” (`app/folha/fora_da_folha.py:43`), mas o juiz documenta pergunta viva sem crédito mantendo verbo `decidir`, portanto fio `chamado` (`app/chamados/juizes.py:1200-1202`, `:1175-1181`). Esse caso não permite concluir ausência de pergunta a partir do fio.

_847 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
