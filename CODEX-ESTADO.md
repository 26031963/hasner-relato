# CODEX-ESTADO

> Ultima resposta processada: **30/10/2026 03:41** · publicado em **30/10/2026 03:41** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 30/10 03:41 · sessao c7555ec23ce1 · - · - · origem=Ronald (tmux)

Não encontrei mudança de resultado no vigia pela extração: mantém universo, filtro pelo C6, memo local por empresa/competência e classificação sem data como `aberta`; a janela continua sendo `janela_atual`, e K6 retorna booleano (`app/chamados/services/vigia_fio.py:404-441`; `app/ponto/janelas.py:60-63`; `app/folha/export.py:569-575`).

Os testes novos têm verificações materiais: [nao verificado]

- R1 exige pergunta viva, nenhuma recusa de reabertura, mesmo chamado renascido e destino vivo (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:149-158`).
- R1b exige recusa `admin_sem_fato_novo`, retorno falso, pergunta sem destino e chamado resolvido (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:162-169`).
- Idempotência exige primeira passagem com destino vivo antes de comparar pergunta, chamados e trilha após a segunda passagem (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:296-308`).
- Custo verifica ausência de chamadas às cercas e teto de seis queries; o novo chokepoint mede `registrar_batida` com órfã sem célula sob teto 31 (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:315-337`; `app/ponto/tests/test_selo_chokepoint_escrita.py:169`; `app/ponto/tests/test_selo_chokepoint_escrita.py:273-283`).

Não identifiquei selo vazio concreto nesses trechos; execução dos testes e medições/RED alegados: [não verificado]. [nao verificado]

## 30/10 03:13 · sessao 96c72512bd21 · ORCAMENTO · verificador · origem=Code

Encontrei os seguintes defeitos e lacunas. Os cenários descrevem comportamento do código; ocorrência em produção: [não verificado].

1. **A expiração ainda pode ser engolida pelo TETO quando a gravação falha.**  
   Entrada: prova do RELATO aprovada, `PAREI: ! CLEAR` há 31 minutos, contador com cinco bloqueios no mesmo HEAD; estado do orçamento legível, mas diretório sem permissão para criar o temporário. `_orc_salva` falha antes de devolver a expiração; `_orcamento_contido` retorna `None`; o caminho antigo incrementa para seis e libera pelo TETO. Quebra **v2(b)**: faltam o `siga` explícito e o contador zerado. Reproduzido em memória.  
   Fontes: `bin/hook_stop_fila1.py:821`, `bin/hook_stop_fila1.py:867`, `bin/hook_stop_fila1.py:1071`, `bin/hook_stop_fila1.py:1078`.

2. **Marco ilegível é tratado como marco novo e permite redisparo.**  
   Estado: ciclo já terminou com `disparo.desfecho="seguiu"` e `marco_ref` conhecido. Sem nenhum clear/compact, a leitura de `ultimo_clear.json` passa a falhar; contexto continua em 60%. `ler_marco` devolve `None`; a diferença de referência apaga o disparo anterior e permite outro `DISPARO`. Quebra **§3: sem redisparo até clear ou compact**. Reproduzido em memória.  
   Fontes: `bin/orcamento_contexto.py:136`, `bin/hook_stop_fila1.py:783`, `bin/hook_stop_fila1.py:800`, `bin/hook_stop_fila1.py:855`.

3. **O desligamento depende de conseguir a trava primeiro.**  
   Entrada: `ORCAMENTO_CONTEXTO=0`, topo CLEAR e trava do orçamento ocupada por mais de cinco segundos. O hook devolve `siga: orçamento ocupado`, embora o instrumento esteja desligado; nem chega à consulta da variável ou ao registro de desligamento. Quebra **§5** e expõe uma precedência errada entre corte e trava. Reproduzido em memória.  
   Fontes: `bin/hook_stop_fila1.py:739`, `bin/hook_stop_fila1.py:748`, `bin/hook_stop_fila1.py:772`.

4. **Fila vazia passa a bloquear quando existe gatilho.**  
   Entrada: todos os itens fechados, topo neutro, nenhum produtor e contexto em 60%. O orçamento devolve PREPARA-CLEAR antes de alcançar a liberação por fila vazia. É regressão objetiva do **caminho antigo de fila vazia**; o contrato não declara expressamente uma exceção ao gatilho para esse caso. O selo cobre fila vazia somente com medida ilegível, sem gatilho. Reproduzido em memória.  
   Fontes: `bin/hook_stop_fila1.py:855`, `bin/hook_stop_fila1.py:1014`, `bin/hook_stop_fila1.py:1039`; `bin/tests/test_orcamento_contexto.sh:145`.

5. **Estado confirmado com append falho perde definitivamente o evento.**  
   Estado: JSON gravável, mas `orcamento.log` sem permissão. `_orc_salva` substitui o estado; `_orc_trilha` engole a falha; a lista de eventos é descartada. Num ATENDIDO/SEGUIU, o próximo Stop já encontra o desfecho consumido e não repõe a linha: o ESTADO subconta permanentemente. Quebra **§4: trilha por disparo/desfecho e contadores pelo log**. O selo verifica apenas a direção inversa, quando o estado não grava.  
   Fontes: `bin/hook_stop_fila1.py:694`, `bin/hook_stop_fila1.py:715`, `bin/hook_stop_fila1.py:723`, `bin/hook_stop_fila1.py:725`; `bin/orcamento_contexto.py:223`; `bin/tests/test_orcamento_contexto.sh:395`.

6. **Um segundo clear apaga a prova do primeiro atendimento.**  
   Sequência: PAREI em t=0; `/clear` em +10 minutos; outro `/clear` em +40; nenhum Stop entre eles. O escritor conserva apenas +40 e o hook registra `SEGUIU`, apesar do atendimento em +10. Quebra a definição de **ATENDIDO do §4**. Classificação reproduzida em memória.  
   Fontes: `bin/marco_de_clear.py:38`, `bin/marco_de_clear.py:47`; `bin/hook_stop_fila1.py:789`, `bin/hook_stop_fila1.py:792`.

7. **A atualização do marco não serializa clear e compact concorrentes.**  
   Intercalação: compact lê o marco antigo; clear publica o novo; compact publica depois com o `clear_hora` antigo. O `replace` atômico não protege a sequência leitura–atualização, e o atendimento pode desaparecer. Quebra **§4** e a conservação prometida pelo escritor. Intercalação reproduzida em memória; existência dessa concorrência no harness: [não verificado].  
   Fontes: `bin/marco_de_clear.py:36`, `bin/marco_de_clear.py:41`, `bin/marco_de_clear.py:43`, `bin/marco_de_clear.py:52`.

8. **Falha ao ler HEAD produz testemunha inválida sem aviso — e o selo aceita.**  
   Entrada: provas do deploy passaram, mas o `git rev-parse HEAD` do append falha. `printf` ainda grava `data hora  0` com sucesso; o aviso não executa. O leitor descarta os três campos restantes: três deploys assim contam zero. Quebra **v2(g), testemunha `hora head rc`**. A regex do selo aceita HEAD vazio por usar `[0-9a-f]*`.   [nao verificado]
   Fontes: `bin/deploy.sh:347`, `bin/deploy.sh:348`; `bin/orcamento_contexto.py:149`; `bin/tests/test_orcamento_contexto.sh:278`, `bin/tests/test_orcamento_contexto.sh:280`.

9. **O RED não prova que reprovação da casca interrompe o deploy.**   [nao verificado]
   Mutação concreta: substituir `falhou` por `echo` na linha 219 do deploy. O teste continua encontrando o `if ! bash`, a guarda e o append esperados; porém o deploy passa a recarregar depois da reprovação. `_julga_deploy` executa fragmentos isolados e verifica a porta por texto, sem provar sua consequência. Lacuna de **v2(g), classe “RED sem mordida”**. A porta do deploy atual funciona; o defeito aqui é sua proteção pelo selo.   [nao verificado]
   Fontes: `bin/deploy.sh:70`, `bin/deploy.sh:219`; `bin/tests/test_orcamento_contexto.sh:265`, `bin/tests/test_orcamento_contexto.sh:284`, `bin/tests/test_orcamento_contexto.sh:289`.

Nas demais áreas solicitadas:

- **Prova do RELATO:** não encontrei bypass introduzido; continua antes da consulta ao orçamento. `bin/hook_stop_fila1.py:1003`, `bin/hook_stop_fila1.py:1011`.
- **PAREI comum e ESPERANDO provado:** não encontrei segundo juiz; reutiliza `_relato_parou` e memoriza uma única avaliação da espera, preservando a prova por avanço de arquivo. `bin/hook_stop_fila1.py:653`, `bin/hook_stop_fila1.py:845`, `bin/hook_stop_fila1.py:847`, `bin/hook_stop_fila1.py:1025`.
- **Falha aberta antiga:** permanece no `except` principal; as exceções próprias do orçamento ficam contidas. A interação defeituosa dessa contenção com a expiração está no achado 1. `bin/hook_stop_fila1.py:863`, `bin/hook_stop_fila1.py:1093`.
- **Relógio, derivação paralela e segundo escritor:** não encontrei defeito adicional: avaliador recebe `agora`, gerador usa `ler`/`linha`, e o orçamento chama os juízes existentes. `bin/orcamento_contexto.py:174`, `bin/orcamento_contexto.py:207`; `bin/gerar_estado.py:274`; `bin/hook_stop_fila1.py:848`.
- **Ordem das travas existentes:** não encontrei aquisição inversa à sequência orçamento → memória da espera; a trava da espera é liberada ao terminar sua avaliação. A ausência de serialização do marco é o achado 7. `bin/hook_stop_fila1.py:743`, `bin/hook_stop_fila1.py:845`, `bin/hook_stop_fila1.py:572`, `bin/hook_stop_fila1.py:602`.
- **Matchers e medição:** o selo vincula os comandos a `compact` e `clear`; o avaliador declara janela/fonte e contém falhas de leitura do transcript. Instalação efetiva no host e correção da fonte externa: [não verificado]. `bin/tests/test_handoff_sessao.sh:60`, `bin/tests/test_handoff_sessao.sh:78`; `bin/orcamento_contexto.py:32`, `bin/orcamento_contexto.py:85`, `bin/orcamento_contexto.py:105`.

## 30/10 03:12 · sessao 229461fbb71f · - · - · origem=Ronald (tmux)

Achados confirmados:

1. **Marco ilegível rearma ciclo já encerrado.** Com `disparo.desfecho="seguiu"` e `marco_ref` existente, uma falha ao ler `ultimo_clear.json` vira `None`; o hook interpreta isso como mudança de marco, apaga o disparo e pode emitir outro `DISPARO` pelos mesmos 60%. Reproduzi em memória. Quebra §3, “não redispara até próximo marco de clear ou compact”. (`bin/orcamento_contexto.py:125-137`; `bin/hook_stop_fila1.py:781-800`, `855-858`.)

2. **Dois clears entre Stops classificam atendimento correto como expiração.** PAREI em t=0; `/clear` em +10 min; outro `/clear` em +40 min; próximo Stop depois disso. O escritor conserva somente +40; o hook registra `SEGUIU`, embora o clear de +10 tenha atendido dentro da janela. Reproduzi a classificação pela função real com mocks em memória. Quebra §4, definição de ATENDIDO. (`bin/marco_de_clear.py:38-47`; `bin/hook_stop_fila1.py:789-796`.)

3. **Concorrência clear/compact pode perder o clear atendido.** Compact lê o marco antigo; clear publica o novo; compact publica depois, trazendo `clear_hora` antigo. `os.replace` protege a substituição, mas não serializa a leitura e atualização. Reproduzi esse interleaving com as duas chamadas reais e armazenamento em memória. Quebra §4 e a conservação declarada pelo próprio escritor. Concorrência real em produção: [não verificado]. (`bin/marco_de_clear.py:33-52`.)

4. **Falha no append perde definitivamente eventos dos contadores.** Estado gravável e `orcamento.log` sem permissão: `_orc_salva` persiste DISPARO/desfecho, `_orc_trilha` engole o erro, e os eventos são descartados. O próximo Stop lê o evento como consumido, enquanto o ESTADO continua contando somente o log. Quebra §4; o selo cobre apenas o sentido contrário, estado que falha antes da trilha. (`bin/hook_stop_fila1.py:687-695`, `708-725`; `bin/orcamento_contexto.py:217-234`; `bin/tests/test_orcamento_contexto.sh:395-399`.)

Áreas lidas sem defeito adicional identificado:

- Relógio da medição recebido por argumento; leituras de relógio nas bordas Stop, CLI e gerador. (`bin/orcamento_contexto.py:174-178`, `207`, `257`; `bin/hook_stop_fila1.py:763`; `bin/gerar_estado.py:276`.)
- Último assistente fora de sidechain, soma dos três campos, ausência de usage/modelo desconhecido e contenção das falhas de leitura. (`bin/orcamento_contexto.py:62-106`.)
- Gerador usa `ler`/`linha` do avaliador; não introduz segundo juiz nem segundo escritor do orçamento. (`bin/gerar_estado.py:271-276`; `bin/orcamento_contexto.py:207-250`.)
- Janela tem número e fonte declarados; correção da fonte externa e autenticidade histórica da fixture: [não verificado]. (`bin/orcamento_contexto.py:32-43`; `bin/tests/fixtures/orcamento_transcript.jsonl:1-8`.)

Observação de menor força: `contar_marcos` conta também linhas `rc=1` ou `rc=erro`; confirmei em memória que duas dessas linhas mais uma `rc=0` resultam em três marcos. Entretanto, o escritor fornecido publica literalmente `0`; a presença dessas linhas na testemunha de produção é [não verificado]. (`bin/orcamento_contexto.py:149-165`; `bin/deploy.sh:347`.)

## 30/10 03:11 · sessao 86cb32d779ac · - · - · origem=Ronald (tmux)

Achados nas áreas delegadas:

1. **O selo do deploy não prova que reprovação da casca interrompe o deploy.** `_julga_deploy` executa somente o bloco de append e a guarda isolados; para a porta da casca, exige apenas a presença textual do `if ! bash ...`. Mutação concreta: trocar `falhou` por `echo` em `bin/deploy.sh:219` mantém esses checks, embora passe a recarregar e registrar marco após reprovação. Quebra a prova exigida por v2(g), classe “RED sem mordida”. No deploy atual, a porta funciona: `falhou` termina com `exit 1`. (`bin/tests/test_orcamento_contexto.sh:265`, `bin/tests/test_orcamento_contexto.sh:280`, `bin/tests/test_orcamento_contexto.sh:284`, `bin/tests/test_orcamento_contexto.sh:289`; `bin/deploy.sh:70`, `bin/deploy.sh:218`.)

2. **Falha na leitura do HEAD produz testemunha inválida e subcontagem silenciosa.** Se o `git rev-parse HEAD` do append falhar, `printf` ainda escreve `data hora  0` e retorna sucesso; o aviso não aparece. O leitor descarta essa linha porque possui somente três campos. Três deploys provados nessas condições contam como zero e não disparam (c), contrariando a testemunha `hora head rc` de v2(g). O selo aceita o defeito: sua regex permite HEAD vazio (`[0-9a-f]*`) e executa o fragmento numa raiz temporária sem repositório. Ocorrência no host: [não verificado]. (`bin/deploy.sh:347`; `bin/orcamento_contexto.py:149`; `bin/tests/test_orcamento_contexto.sh:278`, `bin/tests/test_orcamento_contexto.sh:280`.)

3. **O marco tem corrida de atualização.** Compact lê o marco anterior; clear grava marco novo; compact substitui o arquivo com o `clear_hora` antigo que havia lido. Isso perde a conservação do clear e pode perder o ATENDIDO exigido pelo contrato 4. A escrita é atômica, mas a sequência leitura–modificação–substituição não tem trava. Concorrência efetiva de SessionStart no host: [não verificado]. (`bin/marco_de_clear.py:39`, `bin/marco_de_clear.py:43`, `bin/marco_de_clear.py:52`.)

Sem defeito encontrado na guarda atual do append: distingue normal/cópia de agendado, exclui casca pulada e fica depois das provas de casca e rotas. (`bin/deploy.sh:218`, `bin/deploy.sh:321`, `bin/deploy.sh:324`, `bin/deploy.sh:345`.)

O selo vincula os comandos exatos aos matchers `compact` e `clear`; instalação efetiva no host permanece [não verificado], pois depende de `.claude/settings.json`, deliberadamente externo ao versionamento. (`bin/tests/test_handoff_sessao.sh:15`, `bin/tests/test_handoff_sessao.sh:60`, `bin/tests/test_handoff_sessao.sh:70`, `bin/tests/test_handoff_sessao.sh:75`, `bin/tests/test_handoff_sessao.sh:78`; `.gitignore:66`.)

## 30/10 03:09 · sessao 183f01bf09fa · O261-C · verificador · origem=Code

1. **2º juiz / derivação paralela de fonte única:** o HIST tocado ainda decide o dia exclusivamente por `contexto_json__data_turno`, enquanto `dia_do_fato` prioriza `celula.data`. Um encerrado com célula do dia e JSON sem essa chave é descartado antes de `motivos_atendidos`. Para pergunta morta de intervalo, c1 e c2 recusam, deixando-a órfã. O filtro é anterior, mas está no sítio tocado. (`app/chamados/reconciliador.py:1452`, `app/chamados/reconciliador.py:1454`, `app/chamados/juizes.py:88`, `app/chamados/reconciliador.py:363`, `app/chamados/reconciliador.py:764`, `app/chamados/reconciliador.py:727`)

2. **Selo/RED:** há uma incoerência no teste novo de idempotência. A segunda chamada direta exige igualdade de todos os campos, mas percorre novamente o c2 e `reapontar_resolvedora`, que sempre relavra e renova `julgado_em`. Sem congelamento externo, a asserção deve falhar nesse campo; não demonstra o contrato anunciado. (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:263`, `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:264`, `app/chamados/services/disputa_emissao.py:209`, `app/chamados/reconciliador.py:1487`, `app/chamados/services/fio.py:798`, `app/chamados/veredito_lavrado.py:228`)

- **2º escritor fora da porta: nada.** A costura permanece em `reapontar_resolvedora`. (`app/chamados/reconciliador.py:871`, `app/chamados/reconciliador.py:1507`)
- **Relógio fora da borda: nada nos trechos novos.** (`app/chamados/reconciliador.py:703`, `app/chamados/reconciliador.py:721`)
- **Ordem de trava: nada.** A emissão continua pelo `abrir`, com trava de Colaborador antes do dedup. (`app/chamados/reconciliador.py:831`, `app/chamados/models.py:666`, `app/chamados/models.py:669`)
- **Pouso antes do pré-requisito: nada demonstrável.** O RELATO declara B no ar e passivo aplicado; a confirmação externa desses registros está [não verificado]. (`app/docs/CORTES.md:399`, `app/docs/RELATO.md:4`, `app/docs/RELATO.md:7`)

Resultado efetivo da suíte: [não verificado]; os achados acima são de leitura estática. [nao verificado]

## 30/10 03:08 · sessao aaca92fde8a5 · - · - · origem=Ronald (tmux)

- **Relógio fora da borda: nada novo.** Os helpers novos consultam janela, K6, C7 e C6 sem ler `now()`/`today()` diretamente (`app/chamados/reconciliador.py:703`, `app/chamados/reconciliador.py:710`, `app/chamados/reconciliador.py:724`).

- **Selo/RED: problema concreto no teste novo de idempotência.** `test_reatar_duas_vezes_e_idempotente` chama a porta diretamente pela segunda vez e exige igualdade de **todos** os campos da pergunta (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:263`, `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:264`). A segunda passagem exclui o próprio chamado em `_derivar_chamado_destino`, pois ele já tem pergunta não materializada (`app/chamados/services/disputa_emissao.py:209`); segue por HIST vazio ao c2 (`app/chamados/reconciliador.py:1477`, `app/chamados/reconciliador.py:1487`), reutiliza o chamado vivo (`app/chamados/reconciliador.py:793`, `app/chamados/models.py:670`) e reaponta pela porta (`app/chamados/reconciliador.py:871`). Essa porta sempre relavra e salva (`app/chamados/services/fio.py:797`), renovando `julgado_em` (`app/chamados/veredito_lavrado.py:228`). Portanto, o teste novo exige uma idempotência que o caminho atual não oferece; a igualdade da linha falha por esse timestamp. A escrita redundante de produção é anterior e não constitui achado independente no diff; o sítio novo problemático é a asserção do teste (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:264`).

Execução da suíte e resultado efetivo: [não verificado]. [nao verificado]

## 30/10 03:07 · sessao fe94d09b3594 · - · - · origem=Ronald (tmux)

Achado na classe **2º juiz / derivação paralela do dia**: o HIST tocado ainda seleciona encerrados exclusivamente por `contexto_json__data_turno`, descartando chamado cujo dia está na célula, embora `dia_do_fato` priorize justamente `celula.data`. O ramo vivo já consulta esse juiz. (`app/chamados/reconciliador.py:1452`, `app/chamados/reconciliador.py:1454`, `app/chamados/juizes.py:88`, `app/chamados/services/disputa_emissao.py:213`)

Contraexemplo: pergunta morta de intervalo, competência aberta, encerrado com célula do dia e contexto sem `data_turno`. O HIST não encontra o candidato; c1 recusa intervalo; c2 recusa pergunta morta, mantendo-a órfã. Ausência dessa chave em chamados históricos é reconhecida no próprio código. (`app/chamados/reconciliador.py:1454`, `app/chamados/reconciliador.py:363`, `app/chamados/reconciliador.py:764`, `app/chamados/reconciliador.py:727`, `app/chamados/juizes.py:1713`, `app/chamados/catalogo/modulos.py:19`)

O filtro já existia, mas está em sítio tocado pelo diff fornecido; o escopo registrado inclui sítios tocados. (`app/docs/CORTES.md:385`; diff fornecido [não verificado por Git])

Nas chamadas novas a `motivos_atendidos`, `pergunta_viva`, `competencia_entregue` e `dia_da_pergunta_trancado`: **nada adicional** de juiz duplicado; elas delegam às autoridades existentes. (`app/chamados/reconciliador.py:713`, `app/chamados/reconciliador.py:716`, `app/chamados/reconciliador.py:727`, `app/chamados/reconciliador.py:1470`)

## 30/10 03:07 · sessao d5f4610bd274 · - · - · origem=Ronald (tmux)

- **2º escritor fora da porta: nada.** O diff encaminha os novos casos ao c2 e ao HIST; ambos continuam usando `reapontar_resolvedora`, cuja escrita ocorre na porta. (`app/chamados/reconciliador.py:764`, `app/chamados/reconciliador.py:1470`, `app/chamados/reconciliador.py:871`, `app/chamados/reconciliador.py:1507`, `app/chamados/services/fio.py:792`).
- **Ordem de trava: nada.** A emissão continua por `ChamadoColaborador.abrir`, que toma a trava de Colaborador antes do dedup; nenhum novo bloco de trava foi introduzido nos trechos examinados. (`app/chamados/reconciliador.py:831`, `app/chamados/models.py:666`, `app/chamados/models.py:669`, `app/chamados/reconciliador.py:685`, `app/chamados/reconciliador.py:1459`).
- **Pouso antes do pré-requisito: nada demonstrável.** O corte exige B antes de C; o RELATO declara B no ar, passivo aplicado e C remedido após B. Os logs citados não estão comprovados nesta leitura. (`app/docs/CORTES.md:399`, `app/docs/RELATO.md:3`, `app/docs/RELATO.md:4`, `app/docs/RELATO.md:7`; execução externa: [não verificado]).

## 30/10 03:03 · sessao c6d879dd914a · ORCAMENTO · verificador · origem=Code

Encontrei os seguintes contraexemplos no código. Ocorrência desses cenários em produção: [nao verificado].

1. **A expiração pode ser engolida pelo TETO quando a trava está ocupada.** Cenário: CLEAR ativo há 31 minutos, contador do HEAD atual em `blocks=5`, trava do orçamento ocupada por mais de cinco segundos. O orçamento retorna `None`; o caminho antigo incrementa para seis e libera pelo TETO, sem o `siga` explícito nem zerar o contador. Quebra **v2(b)**. — [hook_stop_fila1.py:724](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:724), [hook_stop_fila1.py:1040](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:1040), [hook_stop_fila1.py:1056](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:1056).

2. **Transcript truncado pode disparar sozinho com usage antigo.** Cenário: penúltima linha é um assistant com 600000 tokens; a última começa `{"type":"assistant","message":{"usage":` e está truncada; marco recente, nenhum gatilho temporal ou de deploy. O parser ignora a linha ilegível e retorna os 60% anteriores. Quebra **§1(a)**, último turno, e **§3/v2(e)**, medida ilegível deve virar “não sei”. — [orcamento_contexto.py:76](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/orcamento_contexto.py:76).

3. **O gatilho de deploy pode disparar com apenas dois deploys posteriores ao clear.** Cenário: deploy às `10:00:00.100`, clear às `10:00:00.900`, depois mais dois deploys. O histórico elimina frações e o contador compara com `int(desde)`: inclui o deploy anterior e conta três. Quebra **§1(c)/§4**, disparo somente no limiar. O selo exige essa inclusão, portanto preserva a sobrecontagem. — [deploy.sh:347](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/deploy.sh:347), [orcamento_contexto.py:147](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/orcamento_contexto.py:147), [test_orcamento_contexto.sh:325](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/tests/test_orcamento_contexto.sh:325).

4. **Duas leituras do marco permitem liberar um CLEAR velho depois de um clear novo.** Cenário: disparo ativo e topo CLEAR; `SessionStart clear` substitui o marco entre a leitura do ciclo pelo hook e a releitura pelo avaliador. A decisão usa o ciclo antigo, mas as medidas usam o marco novo; pode retornar `libera` e salvar essa combinação. O escritor do marco não participa da trava do orçamento. Quebra o encerramento/rearme pelo marco novo: é inconsistência de snapshot e sincronização. — [hook_stop_fila1.py:753](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:753), [hook_stop_fila1.py:770](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:770), [orcamento_contexto.py:159](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/orcamento_contexto.py:159), [marco_de_clear.py:40](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/marco_de_clear.py:40).

5. **O flock não torna trilha e estado uma transação.** Cenário: append de `DISPARO` funciona, mas `os.replace` do estado falha; o próximo Stop relê o estado anterior e apenda outro `DISPARO` no mesmo ciclo. A mesma ordem permite duplicar desfechos e inflar os contadores publicados. Quebra **§3**, sem redisparo, e **§4**, uma linha por evento/desfecho. — [hook_stop_fila1.py:704](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:704), [hook_stop_fila1.py:765](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:765), [hook_stop_fila1.py:826](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:826).

6. **`ATENDIDO` pode ser contado sem prova dos 30 minutos.** Cenário: disparo em `t=1000`, nenhum Stop observou o topo CLEAR, e clear em `t=10000`. Sem `clear_parei_desde`, a condição de atraso não vale e o código registra `ATENDIDO`, apesar dos 150 minutos decorridos. Quebra a definição do contador em **§4**: não existe evidência da janela exigida. — [hook_stop_fila1.py:758](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:758), [hook_stop_fila1.py:764](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:764).

7. **Um clear atendido pode desaparecer antes de ser contado.** Cenário: CLEAR liberado, `/clear` dentro do prazo, depois `compact` antes do próximo Stop. O segundo marco sobrescreve o primeiro; o hook vê apenas `compact` e registra `RESET`, perdendo o `ATENDIDO`. Quebra **§4**: o desfecho depende de um evento que não foi conservado. — [marco_de_clear.py:40](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/marco_de_clear.py:40), [hook_stop_fila1.py:753](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:753), [hook_stop_fila1.py:767](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:767).

8. **O selo do deploy aceita histórico sem linha e prova não executada.** Seu juiz verifica posições textuais e executa somente a guarda. Aceita trocar o `printf` por `:` mantendo o redirecionamento; também aceita tornar a prova de casca inalcançável com `if false`. Os mutantes existentes alteram apenas a guarda. É **selo sem mordida** para **v2(g)/§4**: não prova a escrita efetiva nem sua dependência da casca OK. — [test_orcamento_contexto.sh:251](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/tests/test_orcamento_contexto.sh:251), [test_orcamento_contexto.sh:266](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/tests/test_orcamento_contexto.sh:266), [test_orcamento_contexto.sh:272](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/tests/test_orcamento_contexto.sh:272).

9. **O selo do handoff aceita `clear` sem carregar o handoff.** Cenário: matcher `clear` contém somente `python3 .../marco_de_clear.py clear`. A validação passa: exige `cat HANDOFF-SESSAO.md` apenas em `compact`. Falha contra **v2(h)**, na exigência de vincular o handoff aos matchers `compact` **e** `clear`. Configuração realmente instalada: [nao verificado]. — [test_handoff_sessao.sh:69](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/tests/test_handoff_sessao.sh:69), [test_handoff_sessao.sh:72](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/tests/test_handoff_sessao.sh:72).

10. **Não há a função pura prometida; o avaliador ainda lê o relógio internamente.** Cenário: a CLI chama `avaliar` sem `agora`; com os mesmos arquivos e argumentos, duas chamadas em lados diferentes das seis horas produzem decisões diferentes por uma leitura interna do relógio. É a classe **relógio fora da borda**, contra o desenho “função pura + CLI”. O caminho Stop passa `agora` explicitamente e evita essa leitura específica. — [orcamento_contexto.py:157](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/orcamento_contexto.py:157), [orcamento_contexto.py:238](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/orcamento_contexto.py:238), [hook_stop_fila1.py:770](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:770).

Nas demais áreas solicitadas:

- **RELATO, PAREI comum e ESPERANDO:** não achei quebra funcional nesses caminhos. A prova do RELATO continua primeiro; os juízes existentes são reutilizados; o memo evita consumir duas vezes a prova de mtime. — [hook_stop_fila1.py:972](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:972), [hook_stop_fila1.py:814](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:814), [hook_stop_fila1.py:653](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:653).
- **Fila vazia:** sem resposta própria do orçamento, continua liberando. Com gatilho, PREPARA-CLEAR antecede essa saída; isso muda o caminho antigo, mas §2 manda disparar sem declarar exceção para fila vazia. — [hook_stop_fila1.py:983](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:983), [hook_stop_fila1.py:1008](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:1008).
- **Falha aberta:** não achei exceção própria do orçamento escapando para o `try` principal; a contenção devolve o caminho antigo. O fail-open principal permanece. — [hook_stop_fila1.py:832](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:832), [hook_stop_fila1.py:1062](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:1062).
- **Segundo juiz/escritor e ordem das travas:** não achei juiz paralelo da fila/PAREI, escritor externo do TETO ou inversão ORC↔ESPERA. Há, porém, a corrida com o escritor do marco descrita no achado 4. — [hook_stop_fila1.py:816](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:816), [hook_stop_fila1.py:841](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:841), [hook_stop_fila1.py:572](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/hook_stop_fila1.py:572).
- **Deploy e selos na régua:** não achei falha na guarda atual do append após as provas, nem selo novo órfão. As falhas estão na precisão da testemunha e na força do selo, conforme achados 3 e 8. — [deploy.sh:218](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/deploy.sh:218), [deploy.sh:324](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/deploy.sh:324), [deploy.sh:345](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/deploy.sh:345), [regua.sh:138](/home/ronald/codex_ws/a7b63585b58cc1853932bd2b24950a0a67cf94f5/bin/regua.sh:138).

## 30/10 03:00 · sessao 8c639f7673c9 · - · - · origem=Ronald (tmux)

Achados nos selos, provados em memória, sem escrever arquivos:

1. **O selo aceita `/clear` sem carregar o handoff.** O `cat HANDOFF-SESSAO.md` só é exigido para `compact`; para `clear`, basta o comando de escrever o marco. Cenário: `SessionStart.clear` contém apenas `python3 .../marco_de_clear.py clear`. A validação passa, mas a sessão nova não recebe o handoff. Quebra v2(h), considerando sua exigência de vincular o comando aos matchers `compact` **e** `clear`. A configuração efetivamente instalada é [nao verificado]. — `bin/tests/test_handoff_sessao.sh:69`, `bin/tests/test_handoff_sessao.sh:72`.

2. **O selo do deploy aceita prova de casca inalcançável e histórico sem linha.** Seu juiz verifica posição textual das referências e executa apenas a guarda do append. Duas mutações em memória foram aceitas: [nao verificado]
   - Embrulhar a prova de casca de `bin/deploy.sh:214` em `if false; then ... fi`.
   - Trocar o `printf` de `bin/deploy.sh:347` por `:` mantendo `>> historico.log`: nenhuma linha é apendada.
   
   Os mutantes existentes só alteram a guarda. Logo o selo não prova a propriedade anunciada de v2(g)/item 4: testemunha escrita somente com casca OK e que efetivamente permita contar deploys. — `bin/tests/test_orcamento_contexto.sh:251`, `bin/tests/test_orcamento_contexto.sh:254`, `bin/tests/test_orcamento_contexto.sh:263`, `bin/tests/test_orcamento_contexto.sh:272`.

3. **O novo selo não cobre cruzamentos importantes com os caminhos antigos.** A fixture tem um único item; os casos de PAREI comum não incluem outro item andável, nem RELATO sem prova com gatilho/CLEAR ativo. O selo antigo do PAREI chama somente `_relato_parou()`, portanto não cobre a integração nova em `main`. Isso é lacuna de regressão de v2(i), não prova isolada de defeito funcional nesses caminhos. — `bin/tests/test_orcamento_contexto.sh:39`, `bin/tests/test_orcamento_contexto.sh:214`, `bin/tests/test_hook_parei_com_andavel.sh:36`, `bin/tests/test_relato_prova_no_stop.sh:20`.

4. **A validação nova do SessionStart ignora o tipo do hook.** Executei o trecho real de validação com `type="prompt"` e as strings de comando exigidas: retornou zero. `_cmds` só recolhe `command`; portanto não garante que os comandos de marco/handoff serão executados como comandos. Quebra a força probatória exigida em v2(h); ocorrência na configuração real é [nao verificado]. — `bin/tests/test_handoff_sessao.sh:60`, `bin/tests/test_handoff_sessao.sh:74`.

Áreas lidas sem defeito encontrado neste recorte:

- Os novos selos entram no loop existente da régua; não são selos órfãos. — `bin/regua.sh:138`.
- A prova do RELATO permanece antes da consulta ao orçamento. — `bin/hook_stop_fila1.py:972`, `bin/hook_stop_fila1.py:980`.
- O selo novo verifica motivos na trilha e reinicia a árvore de logs entre cenários principais. — `bin/tests/test_orcamento_contexto.sh:88`, `bin/tests/test_orcamento_contexto.sh:118`, `bin/tests/test_orcamento_contexto.sh:165`.
- Instalação real de `SessionStart`/`Stop` não é demonstrada pela cópia: o próprio selo declara que consulta `.claude/settings.json` vivo, fora do git. — `bin/tests/test_handoff_sessao.sh:15`; [nao verificado].

## 30/10 03:00 · sessao 88dff749c612 · - · - · origem=Ronald (tmux)

Achados com prova em memória, sem escrever arquivos:

1. **Transcript parcialmente ilegível herda medida antiga e pode disparar sozinho.** `bin/orcamento_contexto.py:76-78` ignora a última linha corrompida e volta à anterior. Cenário: penúltima linha assistant com 600000 tokens; última linha `{"type":"assistant","message":{"usage":` truncada; marco recente e menos de três deploys. `medir_contexto()` devolveu `pct=60`, em vez de “não sei”. Quebra §3 e v2(e), além da exigência de medir somente o último turno. O selo só usa lixo completo/último assistant válido sem usage (`bin/tests/test_orcamento_contexto.sh:137`, `bin/tests/test_orcamento_contexto.sh:276`).

2. **Arredondamento conta deploy anterior ao clear.** `bin/orcamento_contexto.py:147` compara com `int(desde)`; o escritor elimina frações em `bin/deploy.sh:347`. Cenário: um deploy às 10:00:00.100, clear às 10:00:00.900, mais dois deploys posteriores. Conta três, embora só dois sejam posteriores ao marco. Prova em memória confirmou a inclusão de timestamps anteriores no mesmo segundo. Quebra §1(c)/§4: gatilho não pode disparar abaixo do limiar. O selo exige precisamente essa inclusão, `bin/tests/test_orcamento_contexto.sh:325-330`, portanto protege a sobrecontagem.

3. **Dois snapshots do marco permitem liberar CLEAR antigo depois de um clear novo.** `bin/hook_stop_fila1.py:753` lê o marco para controlar o ciclo; `bin/hook_stop_fila1.py:770` chama avaliação que relê o marco em `bin/orcamento_contexto.py:159`. `bin/marco_de_clear.py:40` pode substituir o marco entre essas leituras, sem a trava do orçamento. Cenário: disparo ativo, topo CLEAR antigo, SessionStart clear entre leituras. Prova com mocks retornou `('libera', 'PAREI ! CLEAR (PREPARA-CLEAR)')`, salvou `marco_ref` antigo e `base_epoch` novo, sem ATENDIDO. Quebra encerramento/rearme por marco novo e permite reaproveitar PAREI anterior. É problema de snapshot/trava, não segundo escritor.

4. **Clear atendido pode desaparecer antes de ser contado.** `bin/marco_de_clear.py:40` sobrescreve o único marco; o reconhecimento de ATENDIDO só acontece no próximo Stop, `bin/hook_stop_fila1.py:753-768`. Cenário: CLEAR liberado, `/clear` dentro dos 30 minutos, depois compact antes do próximo Stop. O Stop vê somente compact e escreve RESET, perdendo ATENDIDO obrigatório no §4. O dado intermediário não está em log de eventos. Cenário demonstrado por leitura do fluxo; ocorrência em produção [nao verificado].

Áreas lidas sem outro defeito encontrado:

- Guarda e posição do append: casca falha encerra em `bin/deploy.sh:218-219`; rotas falhas encerram em `bin/deploy.sh:324`; append exclui casca pulada/reload agendado em `bin/deploy.sh:345`.
- Não encontrei segundo escritor do marco/histórico nos arquivos consultados: escritores em `bin/marco_de_clear.py:37-40` e `bin/deploy.sh:347`.
- Relógio é capturado explicitamente pelo hook (`bin/hook_stop_fila1.py:736`) e passado ao avaliador (`bin/hook_stop_fila1.py:770`). Há desconformidade literal com “função pura + CLI”: `avaliar()` ainda lê relógio internamente quando omitido (`bin/orcamento_contexto.py:157`), como ocorre na CLI (`bin/orcamento_contexto.py:238`); também lê arquivos internamente (`bin/orcamento_contexto.py:158-167`). Eu classificaria isso abaixo dos defeitos comportamentais.
- A janela possui número e fonte declarados (`bin/orcamento_contexto.py:32-43`); existência/conteúdo real dessa fonte fora da cópia [nao verificado].

## 30/10 03:00 · sessao ef3651d874f9 · - · - · origem=Ronald (tmux)

Achados no hook, com prova em memória onde indicado:

1. **`ATENDIDO` sem provar atendimento nos 30 min.** Em `bin/hook_stop_fila1.py:759`, a verificação do prazo exige `clear_parei_desde`; ausente esse campo, qualquer marco `clear` cai em `ATENDIDO` em `bin/hook_stop_fila1.py:764`. Cenário reproduzido só em memória: DISPARO em `1000`, nenhum PAREI observado, clear em `10000` → registra ATENDIDO, apesar de decorridos 150 minutos. Quebra a definição do contador na cláusula 4 (“marco de clear novo dentro dos 30 min”); o código só valida esse prazo quando houve PAREI observado. `bin/hook_stop_fila1.py:758`.

2. **Contenção da trava permite ao TETO engolir a expiração.** Cenário: CLEAR ativo há 31 min, TETO do HEAD atual com `blocks=5`, outro processo mantém ORC lock por mais de cinco segundos. `_orcamento` retorna `None` em `bin/hook_stop_fila1.py:724`; o main segue sem executar o `siga` de expiração nem zerar o contador em `bin/hook_stop_fila1.py:983`; o PAREI genérico recusa CLEAR em `bin/hook_stop_fila1.py:898`; o contador cresce para seis em `bin/hook_stop_fila1.py:1040`, liberando por TETO em `bin/hook_stop_fila1.py:1056`. Quebra v2(b). O próprio trecho protegido pode demorar mais de cinco segundos, pois chama o juiz de trabalho em curso em `bin/hook_stop_fila1.py:817`, cujo subprocesso tem timeout de dez segundos em `bin/hook_stop_fila1.py:386`.

3. **Disparo na trilha antes de persistir estado pode repetir no mesmo ciclo.** DISPARO é apendado em `bin/hook_stop_fila1.py:826`, e somente depois o estado é salvo em `bin/hook_stop_fila1.py:827`. Cenário: append funciona, mas `mkstemp`/`os.replace` falha em `bin/hook_stop_fila1.py:701`; a fronteira contém a exceção e segue o caminho antigo em `bin/hook_stop_fila1.py:836`; no próximo Stop, o estado ainda não contém o disparo, então novo DISPARO é apendado. Quebra “sem redisparo” da cláusula 3 e “uma linha por disparo” da cláusula 4; o flock serializa, mas não torna log+estado uma transação. `bin/hook_stop_fila1.py:710`.

Área ambígua que convém não apresentar como achado categórico: a expiração só existe enquanto o topo continua CLEAR, em `bin/hook_stop_fila1.py:775`. Reproduzi em memória CLEAR observado há 50 min, ref igual, topo trocado para ESPERANDO: nenhuma linha SEGUIU, nenhum desfecho; retorna None em `bin/hook_stop_fila1.py:798`. A cláusula pode ser interpretada como “30 min mantendo esse topo”; nessa interpretação não há defeito demonstrado. [nao verificado]

Áreas lidas sem defeito encontrado:

- Prova do RELATO continua anterior a todas as liberações, inclusive CLEAR: `bin/hook_stop_fila1.py:972`.
- PAREI comum conserva seu juiz e precedência após a nova transição: `bin/hook_stop_fila1.py:989`.
- ESPERANDO comprovado adia o disparo; memo impede que `arquivo=` seja julgado duas vezes no mesmo Stop: `bin/hook_stop_fila1.py:653`, `bin/hook_stop_fila1.py:814`, `bin/hook_stop_fila1.py:994`.
- Exceção própria do orçamento é contida sem subir ao fail-open principal: `bin/hook_stop_fila1.py:832`; o fail-open antigo continua em `bin/hook_stop_fila1.py:1062`.
- Não achei segundo juiz da fila/PAREI/trabalho em curso: as funções existentes são reutilizadas em `bin/hook_stop_fila1.py:816`.
- Não achei segundo escritor externo do contador TETO: `_zera_teto` está dentro do mesmo hook proprietário em `bin/hook_stop_fila1.py:841`.
- Não achei inversão entre ORC lock e ESPERA lock: orçamento toma ORC antes de consultar ESPERANDO em `bin/hook_stop_fila1.py:719`; ESPERA usa aquisição não bloqueante em `bin/hook_stop_fila1.py:572` e fecha antes de retornar em `bin/hook_stop_fila1.py:603`.
- Fila vazia libera pelo caminho antigo em `bin/hook_stop_fila1.py:1008` quando não há resposta própria do orçamento; com gatilho, há um bloqueio PREPARA antes disso em `bin/hook_stop_fila1.py:983`, mas o contrato 2 manda disparar e não fornece exceção para fila vazia. Não considero isso, isoladamente, defeito contratual. `bin/hook_stop_fila1.py:828`.

## 30/10 03:00 · sessao 7a97d71c3c87 · O261-C · verificador · origem=Code

Restam **dois defeitos de código**: a tranca ainda tem uma brecha com data nula, e o memo pode devolver K6 velho. Ocorrência atual na frota: **[não verificado]**. (`app/chamados/reconciliador.py:1451`, `app/chamados/reconciliador.py:702`)

1. **Tranca consultada antes do pré-requisito da data.** A porta aceita `data_evento=NULL` e obtém o dia pelo contexto, mas consulta `dia_da_pergunta_trancado(p)` antes de colocar esse dia na pergunta. C7 retorna `False` quando a data está nula. (`app/chamados/models.py:1856`, `app/chamados/reconciliador.py:1424`, `app/chamados/reconciliador.py:1451`, `app/chamados/juizes.py:887`)

   Para intervalo **morto por `validada_em`**, com único `batida_ausente` encerrado desse dia, o HIST novo casa pelo juiz e chega à costura; ela só então data e salva a pergunta, inclusive no dia trancado. Antes, a igualdade não casava esse intervalo. É uma regressão latente da proteção da tranca, sem contestar o R8 já corrigido para perguntas datadas. (`app/chamados/juizes.py:1006`, `app/chamados/reconciliador.py:1463`, `app/chamados/reconciliador.py:1466`, `app/chamados/reconciliador.py:1500`, `app/chamados/services/fio.py:788`, `app/chamados/services/fio.py:799`)

   **R8 não cobre isso:** o cenário sempre grava `data_evento=dia`. (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:96`, `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:183`)

2. **Sim, `_o261c_entregue` pode ficar velho entre perguntas na mesma instância.** No cron, a instância recebida pelo `iterator()` atravessa todo o laço de perguntas daquele colaborador; no evento, cada motivo passa novamente `disputa.colaborador`. O memo dura esse processamento, não apenas uma pergunta. (`app/chamados/management/commands/reconciliar_chamados.py:51`, `app/chamados/reconciliador.py:1380`, `app/chamados/services/disputa_emissao.py:292`, `app/chamados/services/disputa_emissao.py:409`)

   A chave contém somente o dia, sem invalidação. Registrar ou invalidar um export altera a fonte consultada por K6; uma consulta posterior do mesmo dia continua devolvendo o bool guardado. O lock de `abrir` sobre `Colaborador` não é compartilhado por `ExportacaoDominio.registrar`. (`app/chamados/reconciliador.py:702`, `app/folha/models.py:162`, `app/folha/models.py:182`, `app/folha/export.py:570`, `app/chamados/models.py:669`)

   Assim, um export concluído entre duas perguntas pode deixar a segunda entrar no alcance novo com o `False` anterior; o c2 não revalida a entrega adiante. O inverso também conserva indevidamente o alcance fechado após invalidação. Intercalação real em produção: **[não verificado]**. (`app/chamados/reconciliador.py:703`, `app/chamados/reconciliador.py:755`, `app/chamados/reconciliador.py:788`, `app/folha/export.py:572`)

Por classe:

| Classe | Resultado |
|---|---|
| Segundo juiz registrado | **Nada novo**; consulta os juízes existentes. `app/chamados/reconciliador.py:704`, `:717`, `:1452`, `:1463`. |
| Segundo escritor fora da porta | **Nada**; conserva `abrir` e `reapontar_resolvedora`. `app/chamados/reconciliador.py:822`, `:862`, `:1500`. |
| Relógio fora da borda | **Nada novo** no diff. `app/chamados/reconciliador.py:697`, `:1431`. |
| Derivação paralela de fonte única | **Nenhuma fórmula nova**; permanece o defeito de atualização do memo descrito no achado 2. `app/chamados/reconciliador.py:706`. |
| Selo/RED sem mordida | Faltam data nula no R8 e mudança de K6 entre consultas. `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:96`, `:197`, `:225`. RED/GREEN: **[não verificado]**. |
| Ordem de trava | **Achado 1**; nenhuma inversão nova de aquisição de lock identificada. `app/chamados/reconciliador.py:1451`, `app/chamados/models.py:666`. |
| Pouso antes do pré-requisito | **Nada comprovado**. O RELATO registra B no ar; a remedida exigida para C permanece **[não verificado]**. `app/docs/RELATO.md:3`, `app/docs/CORTES.md:399`. |

## 30/10 02:59 · sessao a88b4c09a206 · - · - · origem=Ronald (tmux)

Achado: **regressão latente na ordem do pré-requisito da tranca**. A porta aceita `data_evento=NULL` e deriva o dia do contexto, mas consulta C7 antes de datar a pergunta; C7 retorna `False` quando `data_evento` é `None`. (`app/chamados/reconciliador.py:1424`, `app/chamados/reconciliador.py:1451`, `app/chamados/juizes.py:887`)

Reprodução por fluxo estático: intervalo morto por `validada_em`, contexto apontando para dia trancado, competência não entregue, único `batida_ausente` encerrado do dia. C6 retorna morto; o novo HIST casa intervalo e chama a costura, que data e salva a pergunta. A costura só confere correspondência do dia; a lavra não veta tranca. (`app/chamados/juizes.py:1006`, `app/chamados/reconciliador.py:1463`, `app/chamados/reconciliador.py:1500`, `app/chamados/services/fio.py:788`, `app/chamados/juizes.py:1721`, `app/chamados/veredito_lavrado.py:211`)

**Lacuna no R8:** o cenário comum sempre preenche `data_evento=dia`; assim, o teste cobre a pergunta já datada, sem alcançar o fallback do contexto. O modelo permite `NULL`, e testes antigos exercitam perguntas sem data com contexto datado. (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:94`, `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:183`, `app/chamados/models.py:1856`, `app/chamados/tests/test_reconciliar_pergunta_orfa.py:35`, `app/chamados/tests/test_reconciliar_pergunta_orfa.py:94`)

A lavra não impede a escrita: até exceção do juiz vira `indeterminado`, seguida de atribuição dos campos e retorno à costura que salva. (`app/chamados/veredito_lavrado.py:218`, `app/chamados/veredito_lavrado.py:227`, `app/chamados/services/fio.py:798`)

Ocorrência atual na frota: **[não verificado]**. A documentação local só registra zero perguntas sem data em 03/10. (`app/chamados/services/fio.py:780`)

RED/GREEN executado desta revisão: **[não verificado]**; não executei banco nem comandos de escrita. **[não verificado]** [nao verificado]

## 30/10 02:58 · sessao fe2132c9a81c · - · - · origem=Ronald (tmux)

Achei um defeito demonstrável no memo: ele sobrevive à pergunta e não invalida quando K6 muda. A chave é somente `dia`; a segunda consulta na mesma instância retorna o bool anterior sem consultar `competencia_entregue` novamente (`app/chamados/reconciliador.py:702-708`).

- **Cron:** cada colaborador chega pelo `iterator()`, porém a mesma instância percorre todas as suas perguntas; portanto o memo dura a passagem do colaborador, não uma pergunta (`app/chamados/management/commands/reconciliar_chamados.py:51-56`; `app/chamados/reconciliador.py:1380-1383`).
- **Evento:** o laço percorre motivos e passa sempre `disputa.colaborador` para reatar; o `atomic` local delimita savepoint, sem reler/invalidar o memo (`app/chamados/services/disputa_emissao.py:292`; `app/chamados/services/disputa_emissao.py:404-410`).
- **Mudança da fonte:** registrar cria `ExportacaoDominio`; invalidar altera `invalidada_em`; K6 consulta essas linhas e depende da vigência atual (`app/folha/models.py:154-188`; `app/folha/export.py:569-575`).
- **Não há serialização comum com o export:** o lock do nascimento é sobre `Colaborador`, enquanto `ExportacaoDominio.registrar` apenas consulta e cria ED; a view chega diretamente a essa porta (`app/chamados/models.py:666-669`; `app/folha/models.py:174-188`; `app/folha/views.py:295-300`).
- **Consequência:** depois de cachear aberta, um export concluído entre duas consultas do mesmo dia não impede a segunda pergunta de entrar no alcance novo. O c2 não possui outra guarda de export; isso é deliberado para o comportamento anterior, mas deixa a cerca nova dependente do memo velho (`app/chamados/reconciliador.py:703-708`; `app/chamados/reconciliador.py:754-755`; `app/chamados/reconciliador.py:788-795`). O inverso, invalidar o TXT depois de cachear entregue, mantém o alcance novo indevidamente fechado (`app/folha/models.py:154-162`; `app/folha/export.py:572`; `app/chamados/reconciliador.py:703-708`).

Executei somente o corpo real da função extraído por AST, em memória, com `python3 -B` e fonte K6 falsa: resultado `primeira=False; K6 atualizado=True; segunda=False; chamadas_K6=1`. Não houve escrita de arquivo nem acesso ao banco nesse ensaio; ele confirma a retenção descrita nas linhas 702–708. Ocorrência em produção ou ensaio concorrente com banco: **[não verificado]**. [nao verificado]

Os testes R4 fixam o estado exportado antes da primeira consulta; a idempotência repete a mesma pergunta sem alterar o export. Não cobrem a mudança K6 entre perguntas na mesma instância (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:197-209`; `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:225-247`).

Não encontrei export serial chamado pelo próprio laço; o caminho comprovado é a mudança externa/concorrente do registro enquanto a instância continua em uso (`app/chamados/reconciliador.py:1380-1386`; `app/chamados/services/disputa_emissao.py:389-416`; `app/folha/views.py:295-300`).

## 30/10 02:58 · sessao e0fd15b18b59 · - · - · origem=Ronald (tmux)

- **Segundo juiz: nada encontrado.** O alcance consulta `janela_atual` e `competencia_entregue`; vitalidade/tranca e correspondência histórica usam os juízes existentes. (`app/chamados/reconciliador.py:704-718`, `:1451-1464`; `app/chamados/juizes.py:1589-1600`)
- **Segundo escritor fora da porta: nada encontrado.** O c2 conserva `ChamadoColaborador.abrir` e `reapontar_resolvedora`; o HIST conserva `reapontar_resolvedora` e S128. (`app/chamados/reconciliador.py:822`, `:862`, `:1500-1506`)
- **Relógio fora da borda: nada novo encontrado.** O diff usa o dia recebido; data, juiz e lavra continuam na ordem da porta. (`app/chamados/reconciliador.py:698-707`, `:1431-1435`; `app/chamados/services/fio.py:788-800`)
- **Derivação paralela de fonte única: nada novo encontrado.** O HIST novo lê `motivos_atendidos`, e o c2 alargado lê a constante canônica do grupo DIA. Os dois chamadores de produção restringem o universo a esse grupo. (`app/chamados/reconciliador.py:748-755`, `:1342`, `:1360-1365`, `:1463`; `app/chamados/services/disputa_emissao.py:404-409`)
- **Ordem de trava: nenhuma inversão nova identificada.** O diff não introduz aquisição de trava; o nascimento continua pela porta que trava o colaborador. (`app/chamados/reconciliador.py:822`; `app/chamados/models.py:669`)
- **Pouso antes do pré-requisito: [não verificado].** O corte exige B antes de C, remedida pela mesma sonda, lista nomeada antes do código e parada se superar 343 ou surgir classe nova. O RELATO ainda descreve a obra como em curso e remete às sondas; esses logs não estão disponíveis nesta cópia. (`app/docs/CORTES.md:399`; `app/docs/RELATO.md:3`, `:18`, `:41`; [não verificado] quanto às provas externas)

## 30/10 02:53 · sessao b57be1a9a722 · ORCAMENTO · verificador · origem=Code

Eu reprovaria o diff: a expiração ainda pode liberar pelo PAREI genérico, e uma medida antiga pode disparar CLEAR quando o último turno não tem `usage`. — `bin/hook_stop_fila1.py:761`, `bin/hook_stop_fila1.py:941`, `bin/orcamento_contexto.py:82`.

As cláusulas abaixo estão no contrato reproduzido em `app/docs/RELATO.md:22`, `app/docs/RELATO.md:24`, `app/docs/RELATO.md:25` e `app/docs/RELATO.md:27`. Os cenários são contraexemplos do código; ocorrência deles em produção é [nao verificado].

1. **CLEAR expirado volta a liberar pelo PAREI genérico.**  
   Estado: `disparo.desfecho="seguiu"`, apenas `ITEM-VIVO` andável, topo `PAREI: ! CLEAR -- ITEM-VIVO encerrado | espera Ronald`. O orçamento retorna `None`; o juiz genérico encontra o ID e libera. Também acontece sem disparo ou depois de um clear novo. A transição própria apenas deixa de liberar pela sua porta; não impede a reinterpretação posterior. Quebra item 3 e v2(a)/(b). — `bin/hook_stop_fila1.py:757`, `bin/hook_stop_fila1.py:761`, `bin/hook_stop_fila1.py:884`, `bin/hook_stop_fila1.py:941`.

2. **Último assistente sem `usage` herda medida anterior.**  
   Entrada: penúltimo assistente com 600.000 tokens; último assistente sem `usage`; marco recente, zero deploys. O leitor pula o último e retorna os 60% anteriores, disparando sozinho por (a). `usage={}` também é pulado. Deveria responder “nao sei”. Quebra itens 1(a)/3; o selo de ilegibilidade não contém essa sequência. — `bin/orcamento_contexto.py:82`, `bin/orcamento_contexto.py:87`, `bin/orcamento_contexto.py:93`, `bin/tests/test_orcamento_contexto.sh:137`.

3. **PENDENTE desaparece antes do primeiro Stop sem produtor.**  
   Primeiro Stop: ESPERANDO comprovado, contexto 600.000/1M; registra PENDENTE. Segundo Stop, mesmo marco: produtor terminou, contexto 599.999/1M. O ramo `not gatilho` apaga a pendência antes de verificar a espera; PREPARA-CLEAR não entra. Quebra v2(d): a pendência registrada deveria entrar no primeiro Stop sem produtor. — `bin/hook_stop_fila1.py:766`, `bin/hook_stop_fila1.py:767`, `bin/hook_stop_fila1.py:773`, `bin/hook_stop_fila1.py:778`.

4. **Trabalho em voo reconhecível pelo caminho antigo não impede PREPARA-CLEAR.**  
   Estado: topo neutro, contexto no limiar, `manage.py test` ou `git push` rodando. O orçamento verifica somente o ESPERANDO declarado e dispara; `_tem_trabalho_em_curso()` seria consultado muito depois. Assim, a proteção cobre o ESPERANDO formal, mas não todo trabalho que o próprio hook já identifica. Contraria “nada em voo é precondição, não efeito”, v2(d). — `bin/hook_stop_fila1.py:392`, `bin/hook_stop_fila1.py:395`, `bin/hook_stop_fila1.py:773`, `bin/hook_stop_fila1.py:781`, `bin/hook_stop_fila1.py:987`.

5. **Sem trava, dois Stops podem registrar dois DISPARO no mesmo ciclo.**  
   Interleaving: A e B leem o mesmo estado sem `disparo`; ambos medem gatilho; ambos apendam DISPARO e salvam. `os.replace` protege cada substituição, mas não a operação completa de ler, decidir, registrar e gravar. Quebra “sem redisparo” e a trilha de um disparo por ciclo. Stops concorrentes no host são [nao verificado]. — `bin/hook_stop_fila1.py:708`, `bin/hook_stop_fila1.py:698`, `bin/hook_stop_fila1.py:781`, `bin/hook_stop_fila1.py:783`.

6. **Clear tardio recebe ATENDIDO.**  
   Cenário: CLEAR liberado em T0; nenhum Stop intermediário; `/clear` em T0+40min. No próximo Stop, a mudança do marco registra ATENDIDO sem comparar sua hora com `clear_parei_desde`. A regra de expiração no próximo Stop não torna esse clear “dentro dos 30 minutos”. Quebra a definição do contador no item 4. — `bin/hook_stop_fila1.py:725`, `bin/hook_stop_fila1.py:728`, `bin/hook_stop_fila1.py:729`, `bin/hook_stop_fila1.py:747`.

7. **A forma literal é reconhecida por prefixo.**  
   Com disparo ativo, `PAREI: ! CLEARANCE | espera Ronald` libera como CLEAR. O mesmo vale para `CLEAR-QUALQUER`, porque o reconhecimento é `startswith`. Quebra a forma literal exigida por v2(a). — `bin/hook_stop_fila1.py:677`, `bin/hook_stop_fila1.py:738`, `bin/hook_stop_fila1.py:746`.

8. **ESTADO perde os números quando falta o marco.**  
   Cenário: sem `ultimo_clear.json`, transcript iniciado há sete horas, três deploys posteriores. O avaliador calcula horas/marcos pelo transcript, mas o hook persiste somente contexto. O leitor do ESTADO retorna `None` para horas/marcos e imprime apenas a explicação do fallback. Quebra o formato mensurável `Xh, M marcos` do item 4. — `bin/orcamento_contexto.py:160`, `bin/orcamento_contexto.py:162`, `bin/hook_stop_fila1.py:735`, `bin/orcamento_contexto.py:203`, `bin/orcamento_contexto.py:219`.

9. **Precisões incompatíveis subcontam deploy posterior ao clear.**   [nao verificado]
   Cenário: marco às `12:00:00.100`; append de deploy provado às `12:00:00.900`; mais dois deploys posteriores. O marco preserva fração; o histórico trunca para segundos; o primeiro deploy é comparado como `12:00:00.000` e descartado. Conta dois em vez de três. Quebra item 1(c)/v2(g); o selo separa marco e deploys por minutos. — `bin/marco_de_clear.py:34`, `bin/deploy.sh:347`, `bin/orcamento_contexto.py:141`, `bin/orcamento_contexto.py:144`, `bin/tests/test_orcamento_contexto.sh:130`.

10. **Os selos novos têm aceitação por texto sem provar o comportamento.**
   
    - **Deploy:** inverter apenas `SEM_PROVA_DE_CASCA != 1` para `= 1` mantém a aprovação de `_julga_deploy`: ele exige os nomes das variáveis, não a condição correta. Passaria a contar casca pulada e excluir deploy provado. O mutante oficial elimina a guarda inteira, portanto não morde essa inversão. Quebra a prova exigida em v2(g). — `bin/deploy.sh:345`, `bin/tests/test_orcamento_contexto.sh:252`, `bin/tests/test_orcamento_contexto.sh:253`, `bin/tests/test_orcamento_contexto.sh:257`.
    - **Matchers:** comandos `echo HANDOFF-SESSAO.md`, `echo marco_de_clear.py compact` e `echo marco_de_clear.py clear`, nos respectivos matchers, satisfazem as condições novas sem carregar handoff nem escrever marco. Logo, o selo aceita uma instalação inoperante; não prova v2(h). A configuração efetivamente instalada é [nao verificado]. — `bin/tests/test_handoff_sessao.sh:60`, `bin/tests/test_handoff_sessao.sh:64`, `bin/tests/test_handoff_sessao.sh:67`.

Nas demais áreas examinadas:

- **Prova do RELATO:** não achei passagem nova que a pule; vem antes do orçamento. — `bin/hook_stop_fila1.py:924`, `bin/hook_stop_fila1.py:932`.
- **PAREI comum e ESPERANDO comprovado:** não achei regressão adicional nos casos estáveis; reutilizam os juízes existentes, e a memória compartilhada evita consumir duas vezes o avanço do arquivo. Permanecem os defeitos 1, 3 e 4 acima. — `bin/hook_stop_fila1.py:660`, `bin/hook_stop_fila1.py:774`, `bin/hook_stop_fila1.py:946`.
- **TETO:** na primeira expiração reconhecida, o `siga` explícito precede o TETO e pede seu zeramento; o defeito está na liberação genérica posterior. — `bin/hook_stop_fila1.py:754`, `bin/hook_stop_fila1.py:935`, `bin/hook_stop_fila1.py:937`.
- **Fila vazia e falha aberta:** sem gatilho, a fila vazia conserva sua saída; exceção do orçamento é contida e devolve ao fluxo antigo. — `bin/hook_stop_fila1.py:766`, `bin/hook_stop_fila1.py:789`, `bin/hook_stop_fila1.py:960`, `bin/hook_stop_fila1.py:1014`.
- **Segundo juiz, segundo escritor e fonte paralela:** não achei duplicação adicional nesses papéis: PAREI/ESPERANDO são consultados; ESTADO só lê; marco tem o escritor declarado; a contagem usa o histórico escolhido. A perda do fallback está no achado 8. — `bin/hook_stop_fila1.py:774`, `bin/gerar_estado.py:271`, `bin/marco_de_clear.py:40`, `bin/orcamento_contexto.py:130`.
- **Relógio e ordem de trava:** no Stop, o instante é capturado e passado ao avaliador. Não achei inversão nova de travas; achei ausência de serialização no estado novo, descrita no achado 5. — `bin/hook_stop_fila1.py:706`, `bin/hook_stop_fila1.py:734`, `bin/hook_stop_fila1.py:572`, `bin/hook_stop_fila1.py:602`.
- **Append atual e entrada na régua:** não achei falha adicional no posicionamento do append: fica depois das provas e exclui casca pulada/higiene; o selo novo entra no laço existente. — `bin/deploy.sh:218`, `bin/deploy.sh:324`, `bin/deploy.sh:331`, `bin/deploy.sh:345`, `bin/regua.sh:138`.

## 30/10 02:52 · sessao b89a5b99fff0 · O261-C · verificador · origem=Code

Encontrei **duas falhas de comportamento no HIST ampliado**, além de lacunas nos selos e custo novo no evento. Ambas decorrem de alcançar ramos preexistentes do S128 que não preservam as guardas do primeiro ramo (`app/chamados/reconciliador.py:1450,1493`; `app/chamados/services/disputa_emissao.py:1551,1558,1634`).

1. **Ordem de trava: o HIST novo escreve em pergunta silenciada pela tranca.**  
   Caso: intervalo órfão, competência **não entregue, mas trancada**, histórico `batida_ausente` resolvido por outra via, disputa sem pai. O C6 responde MORTA pela tranca; isso permite entrar no HIST ampliado. A costura escreve antes de consultar a proteção específica da tranca (`app/chamados/juizes.py:1010`; `app/chamados/reconciliador.py:1438,1450,1487`).

   O primeiro ramo do S128 respeita a tranca, mas o ramo posterior inclui todas as disputas abertas do colaborador, aceita pai ausente e carimba essa mesma pergunta como `chamado_encerrado`, sem consultar a tranca. Portanto, a guarda anterior é ultrapassada. Essa via também não é desfeita por `reabrir_por_competencia`, que aceita apenas `competencia_trancada` (`app/chamados/services/disputa_emissao.py:1551,1622,1628,1634,1644,1647`; `app/chamados/models.py:2213`).

   Antes, intervalo não casava com o mapa e era recusado pelo c1/c2; a entrada nova é o casamento por `motivos_atendidos`. Contraria o silêncio por leitura documentado pelo próprio juiz: “nada se escreve” (`app/chamados/models.py:2466,2478`; `app/chamados/reconciliador.py:363,748,1450`; `app/chamados/juizes.py:879`).

2. **MORTA com resposta: o HIST novo pode alcançar um carimbo que enterra a declaração.**  
   Caso: intervalo MORTO por decisão da célula, resposta preenchida e `respondida_em` presente, `validada_em` nulo, sem batida que prove a declaração, disputa sem pai. O primeiro ramo do S128 preserva essa resposta; o ramo sem credor a encontra novamente e grava `materializada_em`, sem repetir a proteção (`app/chamados/services/disputa_emissao.py:1558,1602,1634,1644,1659`).

   O HIST ampliado passa a levar esse intervalo ao S128. O efeito desarma o materializador que deveria consumir a resposta — exatamente o risco documentado na guarda existente (`app/chamados/reconciliador.py:1450,1493`; `app/chamados/services/disputa_emissao.py:1553`).

   **Limite do achado:** na chamada direta de reatar/reconciliar órfãs há essa mudança; no cron completo, uma célula absolvente pode já ter provocado o carimbo no retratador anterior. Não atribuo esse efeito inteiro ao diff no caminho completo (`app/chamados/reconciliador.py:584,617,1301,1302`).

3. **Selo/RED sem mordida: competência por janela não está distinguida de mês civil.**  
   Os testes usam somente 06/10 e 10/09, com empresa sem corte personalizado. Nessas datas, trocar a competência da janela por `dia.month/dia.year` não distingue os cenários, embora produza resposta errada depois do corte. A implementação atual está correta; o selo não protege essa exigência (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:43,44,52,85`; `app/ponto/janelas.py:27`; `app/chamados/reconciliador.py:702`).

   A idempotência também verifica apenas três campos e contagens. Ela aceita nova lavra/save que altere `julgado_em`; portanto, não demonstra literalmente “mesmo estado” nem ausência de UPDATE (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:200,205,207`; `app/chamados/services/fio.py:798`; `app/chamados/veredito_lavrado.py:228`).

   Os casos MORTOS novos nascem por `validada_em`; não exercitam tranca nem resposta do colaborador. O único HIST morto testado é `intervalo_volta` (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:90,92,159`).

4. **Custo novo no evento: consulta de exportação duplicada por intervalo vivo.**  
   Reatar consulta a entrega em `:1436`; chegando ao c2, o intervalo consulta novamente a mesma competência por `:749 → :709 → :703`. Cada chamada de `competencia_entregue` executa um SELECT; o memo de `pergunta_viva` não cobre essa consulta (`app/chamados/reconciliador.py:703,709,749,1436`; `app/folha/export.py:570`; `app/chamados/juizes.py:791`).

   Esse custo entra na emissão síncrona e na reconciliação acionada pela batida. Estouro efetivo dos tetos: **[nao verificado]** (`app/chamados/services/disputa_emissao.py:404,409`; `app/ponto/signals.py:206`; `app/chamados/reconciliador.py:1302`).

Nas demais classes:

- **2º juiz que duplica juiz registrado: nada.** Os trechos novos consultam as autoridades existentes (`app/chamados/reconciliador.py:700,701,711,1402,1450`).
- **2º escritor fora da porta: nada.** Emissão por `abrir`; costuras por `reapontar_resolvedora` (`app/chamados/reconciliador.py:816,856,1487,1499`).
- **Relógio novo fora da borda: nada.** Os helpers recebem o dia e consultam sua competência (`app/chamados/reconciliador.py:685,698,702,1427`).
- **Derivação paralela de fonte única: nada na implementação.** A lacuna está no selo da janela, descrita acima (`app/chamados/reconciliador.py:700,702,703`).
- **Ordem de locks SQL: nada demonstrado.** A falha encontrada é a precedência da guarda de tranca sobre a escrita (`app/chamados/reconciliador.py:1487,1493`; `app/chamados/services/disputa_emissao.py:1551`).
- **Pouso antes do pré-requisito: [nao verificado].** O RELATO declara B no ar e passivo aplicado, mas não validei os artefatos nem a ancestralidade dos commits (`app/docs/RELATO.md:3,4,7`). A exigência de B antes e remedida posterior consta do corte (`app/docs/CORTES.md:399`).

Os chamadores de produção encontrados foram o laço de órfãs e a emissão de pergunta recém-criada; c2 é chamado pela própria porta de reatar. O cron permanece em `*/15`, usando o universo de `perguntas_orfas_reatavies` (`app/chamados/reconciliador.py:1374,1377,1467`; `app/chamados/services/disputa_emissao.py:404`; `app/chamados/management/commands/reconciliar_chamados.py:36`; `app/config/crons.py:376`).

Exercitei os dois desvios do S128 com seu corpo extraído em memória e dependências simuladas: ambos alcançaram o carimbo posterior. Integração Django, suíte e mutantes completos: **[nao verificado]**. Nenhum arquivo foi criado, editado ou apagado **[nao verificado por auditoria externa]**. [nao verificado]

## 30/10 02:52 · sessao ba59fdf4e883 · - · - · origem=Ronald (tmux)

Achados confirmados por leitura e chamadas com `open`, gravações e trilha substituídos por objetos em memória:

1. **CLEAR volta a liberar pela porta genérica após expirar.** Estado `disparo.desfecho="seguiu"`, único item andável `ITEM-VIVO`, topo `PAREI: ! CLEAR -- ITEM-VIVO encerrado | espera Ronald`: `_orcamento` retorna `None`; `_relato_parou()` aceita a mesma linha pelo id e o `main` libera. Também ocorre sem disparo ou após um clear novo. Quebra v2(a)/(b) e item 3: a transição própria não impede que sua forma seja reinterpretada como PAREI comum. Referências: `bin/hook_stop_fila1.py:757`, `bin/hook_stop_fila1.py:761`, `bin/hook_stop_fila1.py:818`, `bin/hook_stop_fila1.py:884`, `bin/hook_stop_fila1.py:941`.

2. **Clear tardio conta como ATENDIDO.** Disparo e CLEAR já reconhecidos; passam 65 minutos sem Stop; chega `/clear`; próximo Stop vê marco novo. O código registra ATENDIDO sem comparar a hora do marco com `clear_parei_desde`. Quebra item 4, que exige clear dentro de 30 minutos. Referências: `bin/hook_stop_fila1.py:725`, `bin/hook_stop_fila1.py:728`, `bin/hook_stop_fila1.py:729`.

3. **Gatilho pendente é perdido.** Primeiro Stop: produtor comprovado e contexto 600000/1M, registrando PENDENTE. Próximo Stop, mesmo marco: produtor terminou e contexto 599999/1M. O ramo `not gatilho` apaga a pendência antes de verificar o produtor e não entra PREPARA-CLEAR. Quebra v2(d), primeiro Stop sem produtor. Referências: `bin/hook_stop_fila1.py:766`, `bin/hook_stop_fila1.py:767`, `bin/hook_stop_fila1.py:773`, `bin/hook_stop_fila1.py:778`.

4. **Forma aceita por prefixo.** Com disparo ativo, `PAREI: ! CLEARANCE | espera Ronald` recebe `('libera', ...)`, pois o reconhecimento usa `startswith`. Quebra forma literal v2(a). Referências: `bin/hook_stop_fila1.py:677`, `bin/hook_stop_fila1.py:738`, `bin/hook_stop_fila1.py:746`.

5. **Estado não serializado permite dois disparos no mesmo ciclo.** Dois Stops leem o mesmo estado sem `disparo`, antes de qualquer gravação. Ambos medem gatilho, apendam DISPARO e salvam. Reproduzido com duas threads sincronizadas após a leitura: dois resultados PREPARA-CLEAR, dois eventos DISPARO, dois estados salvos. `os.replace` torna cada escrita atômica, mas não protege ler–decidir–registrar–gravar. Quebra “sem redisparo” e trilha de um disparo por ciclo; ocorrência real de Stops concorrentes em produção [nao verificado]. Referências: `bin/hook_stop_fila1.py:699`, `bin/hook_stop_fila1.py:706`, `bin/hook_stop_fila1.py:781`, `bin/hook_stop_fila1.py:783`, `bin/hook_stop_fila1.py:784`.

6. **Trabalho vivo já detectável pelo caminho antigo não impede a entrada PREPARA-CLEAR.** Topo neutro, contexto no limiar, suite ou push em curso: `_esperando_uma_vez` não encontra declaração ESPERANDO e o código dispara; `_tem_trabalho_em_curso` só seria consultado depois. Reproduzido com essa função respondendo `True`: PREPARA-CLEAR imediato. Confronta “nada em voo é precondição, não efeito” de v2(d); o caso formal `ESPERANDO` comprovado é tratado. Referências: `bin/hook_stop_fila1.py:373`, `bin/hook_stop_fila1.py:773`, `bin/hook_stop_fila1.py:781`, `bin/hook_stop_fila1.py:785`, `bin/hook_stop_fila1.py:987`.

Áreas lidas sem defeito adicional encontrado:

- **Prova do RELATO:** permanece antes da consulta ao orçamento e de qualquer nova liberação. `bin/hook_stop_fila1.py:924`, `bin/hook_stop_fila1.py:932`.
- **ESPERANDO formal comprovado:** a consulta compartilhada evita duplicar a leitura que consome avanço do arquivo; sem disparo ativo, orçamento registra pendência e devolve ao caminho antigo. `bin/hook_stop_fila1.py:653`, `bin/hook_stop_fila1.py:773`, `bin/hook_stop_fila1.py:946`.
- **Expiração isolada versus TETO:** naquele Stop, a resposta explícita ocorre antes do TETO e solicita zerá-lo. O defeito é a liberação genérica no Stop seguinte, descrita acima. `bin/hook_stop_fila1.py:750`, `bin/hook_stop_fila1.py:935`, `bin/hook_stop_fila1.py:937`.
- **Falha aberta:** exceção do orçamento é contida e devolve ao fluxo antigo; o handler externo permanece. `bin/hook_stop_fila1.py:789`, `bin/hook_stop_fila1.py:1014`.
- **Juiz/porta:** o código novo consulta os juízes existentes de PAREI e ESPERANDO; não encontrei um segundo predicado paralelo para esses dois vereditos. `bin/hook_stop_fila1.py:774`, `bin/hook_stop_fila1.py:660`.
- **Ordem de travas:** não encontrei inversão nova; o problema concreto é ausência de serialização do estado do orçamento. A trava preexistente de ESPERANDO é não bloqueante e encerra antes da gravação do orçamento. `bin/hook_stop_fila1.py:572`, `bin/hook_stop_fila1.py:602`, `bin/hook_stop_fila1.py:779`.

## 30/10 02:51 · sessao e49c9886afaa · - · - · origem=Ronald (tmux)

Achados confirmados:

1. **Último turno sem `usage` reutiliza medida velha.** `bin/orcamento_contexto.py:82` e `bin/orcamento_contexto.py:87` continuam a busca ao encontrar o último assistente sem `usage` ou com soma zero. Cenário: penúltimo assistente com 600000 tokens; último sem `usage`; marco recente e zero deploys. Retorna 60% e dispara pelo turno anterior. Confirmei com `BytesIO` e mocks, sem escrita. Viola último turno e “sem usage → nao sei” (`app/docs/RELATO.md:22`, `app/docs/RELATO.md:24`). Os casos ilegíveis não incluem essa sequência (`bin/tests/test_orcamento_contexto.sh:137`).

2. **ESTADO sem marco não publica horas e marcos medidos pelo fallback.** O avaliador calcula ambos desde o início do transcript (`bin/orcamento_contexto.py:160`, `bin/orcamento_contexto.py:162`), mas o hook guarda somente contexto (`bin/hook_stop_fila1.py:735`). O leitor do ESTADO devolve horas/marcos `None` sem marco (`bin/orcamento_contexto.py:203`), e imprime apenas uma explicação (`bin/orcamento_contexto.py:219`). Cenário: marco ausente, transcript iniciado há sete horas, três deploys posteriores. O gatilho funciona, mas a linha não oferece os números exigidos pelo item 4 (`app/docs/RELATO.md:25`).

3. **Deploy posterior ao clear pode ser descartado pela precisão incompatível.** Marco guarda epoch fracionário (`bin/marco_de_clear.py:33`); histórico guarda segundos inteiros (`bin/deploy.sh:347`); contagem compara o segundo reconstruído diretamente com a base fracionária (`bin/orcamento_contexto.py:141`, `bin/orcamento_contexto.py:144`). Cenário: clear `12:00:00.100`, append de deploy provado `12:00:00.900`, depois mais dois deploys. Conta dois, embora três sejam posteriores ao marco; quebra item 1(c)/v2(g) (`app/docs/RELATO.md:22`, `app/docs/RELATO.md:27`). O selo usa separação de minutos e não cobre a fronteira (`bin/tests/test_orcamento_contexto.sh:130`).

4. **ATENDIDO aceita clear tardio.** Mudança de marco registra ATENDIDO sem comparar sua hora com o prazo (`bin/hook_stop_fila1.py:725`, `bin/hook_stop_fila1.py:729`), antes da checagem de expiração (`bin/hook_stop_fila1.py:747`). Cenário: CLEAR liberado em T0; nenhum Stop intermediário; clear em T0+40min; próximo Stop registra ATENDIDO. Viola “atendido = marco novo dentro dos 30min” (`app/docs/RELATO.md:25`).

Possível falha adicional do selo: exige impressão do HANDOFF somente para `compact` (`bin/tests/test_handoff_sessao.sh:64`), enquanto para `clear` exige apenas o escritor do marco (`bin/tests/test_handoff_sessao.sh:66`). Portanto uma configuração `clear` que não carrega HANDOFF passa nessa inspeção, apesar do texto de v2(h) mencionar ambos os matchers (`app/docs/RELATO.md:27`). Configuração efetivamente instalada: [nao verificado].

A janela tem número e fonte textual declarados (`bin/orcamento_contexto.py:32`); a fonte externa mencionada e a correção dos tamanhos são [nao verificado]. O escritor do marco é único nos arquivos examinados e usa substituição atômica (`bin/marco_de_clear.py:37`, `bin/marco_de_clear.py:40`); não achei segundo escritor nessa área.

_340 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
