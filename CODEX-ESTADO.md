# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 14:31** · publicado em **31/10/2026 14:36** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 14:31 · sessao c7f1ca21315f · REPROVA-E-DESENHO-CODEX · verificador · origem=Code

**REPROVADO: o contrato não está fechado.** Há disparo com volta já DESCARTADA, chamadas que escapam do bloqueio e rearme que pode ressuscitar contagem anterior. (`bin/disjuntor.py:839`, `bin/disjuntor.py:1280`, `bin/disjuntor.py:912`)

A análise abaixo usa o diff fornecido e a cópia dos arquivos. Correspondência da cópia com o commit, ocorrência em produção e resultado da execução do selo/mutantes: **[nao verificado]**. A suíte escreve e apaga arquivos, portanto sua execução contrariaria a restrição de somente leitura. (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:41`, `bin/tests/test_disjuntor.sh:51`)

| Item | Cumpre? | Evidência |
|---|---|---|
| **1. Três chamadas distintas, da mesma obra, PROCEDENTES; N achados = uma volta** | **Parcialmente.** O percurso nominal separa por obra, deduplica identidades e dispara em três. A identidade e sua conservação têm falhas descritas abaixo. | `bin/disjuntor.py:773`, `bin/disjuntor.py:795`, `bin/disjuntor.py:839`, `bin/disjuntor.py:850` |
| **2. Não triada ou DESCARTADA não conta** | **Não.** O filtro da fonte considera o último veredito, mas a contagem persistida nunca retira uma volta reclassificada como DESCARTADO. | `bin/disjuntor.py:801`, `bin/disjuntor.py:844`, `bin/disjuntor.py:849` |
| **3. Negar próximo opus final e Codex verificador; liberar pelo mesmo topo; outros papéis passam** | **Parcialmente.** As duas negativas e a liberação compartilhada existem, mas dependem de reconhecimento textual incompleto e falham abertas em exceções. Há também negativa indevida de outro papel. | `bin/disjuntor.py:868`, `bin/disjuntor.py:1280`, `bin/disjuntor.py:1290`, `bin/disjuntor.py:1329` |
| **4. Escritor único e identidade do wrapper, somente verificador** | **Parcialmente.** A porta `--triagem` recusa ausência de sidecar e papel diferente, mas só confere campos preenchidos; não comprova que a resposta existe nem que aquele sidecar pertence à chamada atual. O append não tem trava. | `bin/disjuntor.py:752`, `bin/disjuntor.py:759`, `bin/disjuntor.py:771`, `bin/disjuntor.py:778`, `bin/disjuntor.py:1515` |
| **5. Liberação/rearme zeram; voltas liberadas não retornam** | **Parcialmente.** Funciona para identidades já incorporadas ao JSON. O rearme não incorpora triagens pendentes antes de zerar, e a liberação pode usar uma fotografia obsoleta da triagem. | `bin/disjuntor.py:821`, `bin/disjuntor.py:842`, `bin/disjuntor.py:874`, `bin/disjuntor.py:912` |

Os caminhos de falha identificados são estes; os cenários descrevem consequências do código, não ocorrências comprovadas em produção. (`bin/disjuntor.py:762`, `bin/disjuntor.py:804`, `bin/disjuntor.py:1263`)

1. **DESCARTADO continua na contagem: disparo e negação indevidos.** A é PROCEDENTE; uma decisão guarda A. Depois A vira DESCARTADO, e B/C são triadas PROCEDENTES. A decisão seguinte conserva A e acrescenta B/C: dispara com três, embora somente duas continuem PROCEDENTES. `_codex_procedentes` recalcula a fonte corretamente; a falha está na união cumulativa de `r['codex']`. (`bin/disjuntor.py:801`, `bin/disjuntor.py:839`, `bin/disjuntor.py:845`, `bin/disjuntor.py:849`)

2. **Codex verificador passa com a obra parada por diferença entre shell real e parser.** Com B3 de OBRA-C disparado, `env CODEX_OBRA=OBRA-C CODEX_PAPEL=verificador bin/codex_pergunta.sh h q r` não produz chamada reconhecida pelo parser. Também escapam ambiente previamente exportado, valores obtidos por expansão de variáveis e execução dentro de `bash -c` ou outro script. O wrapper lê o ambiente efetivo e executa o Codex sem consultar B3. Parte desses limites já está declarada no código, mas continua incompatível com a garantia absoluta do contrato. (`bin/disjuntor.py:1117`, `bin/disjuntor.py:1120`, `bin/disjuntor.py:1196`, `bin/disjuntor.py:1280`, `bin/codex_pergunta.sh:44`, `bin/codex_pergunta.sh:157`)

3. **O parser pode negar outro papel ou consultar a obra errada.** `CODEX_PAPEL=verificador CODEX_PAPEL=leitura ...` é reconhecido como verificador, embora o wrapper receba leitura; a ordem inversa libera o verificador. O mesmo problema afeta atribuições repetidas de obra. O regex também encontra `CODEX_PAPEL=verificador` dentro do valor de outra variável. Além disso, `CODEX_OBRA="O9 (C)"` vira `O9`: pode liberar a obra parada `O9 (C)` ou negar essa chamada por um B3 de `O9`. A identidade com cauda é suportada pelo reconhecedor do opus. (`bin/disjuntor.py:525`, `bin/disjuntor.py:1196`, `bin/disjuntor.py:1198`, `bin/disjuntor.py:1280`, `bin/codex_pergunta.sh:44`)

4. **Um sidecar basta, mesmo sem resposta; a origem não é comprovada.** Se existir apenas `R.id` com os cinco campos preenchidos e papel verificador, `--triagem R procedente` aceita: não abre R. Um sidecar fabricado ou alterado também passa. Alterar a identidade entre duas triagens permite representar uma única chamada como duas voltas. O leitor do log, por sua vez, aceita qualquer linha com quatro campos e veredito permitido, sem conferir seu lastro. Portanto existe a porta escritora pretendida, mas a exclusividade e a procedência não são garantidas por essas validações. (`bin/disjuntor.py:755`, `bin/disjuntor.py:759`, `bin/disjuntor.py:768`, `bin/disjuntor.py:773`, `bin/disjuntor.py:794`)

5. **Reutilizar o arquivo de resposta perde ou troca o lastro da chamada.** Duas chamadas bem-sucedidas para R deixam somente o último `R.id`; uma triagem atrasada da primeira passa a usar a identidade da segunda. Outro cenário: R tinha sidecar de verificador; uma chamada de papel leitura sobrescreve R e termina com erro; o sidecar anterior permanece, permitindo triar a resposta nova como verificador. Se RC=0 ocorrer sem resposta nova, mas R antigo existir, o wrapper grava identidade nova para o arquivo antigo. Falhar a escrita do sidecar também não invalida previamente a identidade velha. (`bin/codex_pergunta.sh:40`, `bin/codex_pergunta.sh:158`, `bin/codex_pergunta.sh:178`, `bin/codex_pergunta.sh:180`, `bin/disjuntor.py:768`, `bin/disjuntor.py:771`)

6. **A identidade pode fundir chamadas distintas.** Ela usa hash truncado em 12 caracteres, PID e término com precisão de segundos. Se duas chamadas coincidirem nesses campos — por exemplo, PID reciclado com relógio recuado — serão uma única volta; se a chave coincidir com uma liberada/rearmada, a chamada nova será ignorada. Ocorrência dessa colisão em produção: **[nao verificado]**. (`bin/codex_pergunta.sh:179`, `bin/disjuntor.py:773`, `bin/disjuntor.py:795`, `bin/disjuntor.py:842`)

7. **Triagem gravada fora da trava e decisão com fonte obsoleta.** O teste de idempotência e o append não compartilham trava: dois processos podem verificar ausência e anexar duas linhas iguais. **Isso, isoladamente, não conta duas voltas**, pois o leitor reduz por chave e o acumulador verifica pertença. Contudo, `decide_b3` lê as triagens antes de adquirir a trava do JSON: uma reclassificação ou terceira triagem concluída nesse intervalo fica fora da decisão; uma liberação também pode deixar de consumir voltas já PROCEDENTES quando efetivamente grava. (`bin/disjuntor.py:774`, `bin/disjuntor.py:778`, `bin/disjuntor.py:795`, `bin/disjuntor.py:821`, `bin/disjuntor.py:844`, `bin/disjuntor.py:897`)

8. **Rearme bem-sucedido pode redisparar imediatamente com triagens anteriores.** A já está no JSON; B/C/D já estão PROCEDENTES no log, mas ainda não foram varridas. O rearme arquiva somente A e retorna sucesso. Na decisão seguinte B/C/D entram e disparam, sem nenhuma volta nova após o rearme. A função não lê a triagem antes de zerar. (`bin/disjuntor.py:908`, `bin/disjuntor.py:912`, `bin/disjuntor.py:913`, `bin/disjuntor.py:925`, `bin/disjuntor.py:839`)

9. **Erro de leitura pode abrir ambas as portas, inclusive com disparo persistido.** `triagem.log` ilegível ou transformado em diretório levanta antes de consultar o estado; somente `FileNotFoundError` é tratado pelo leitor. JSON inválido, falha ao abrir a trava ou trava ocupada também levantam. O PreToolUse captura a exceção e libera a ferramenta; o Stop captura e omite o recado. Já log ausente retorna vazio, e linhas malformadas são ignoradas: se o JSON ainda não acumulou as voltas, a falta de fonte vira ausência de contagem. (`bin/disjuntor.py:788`, `bin/disjuntor.py:790`, `bin/disjuntor.py:794`, `bin/disjuntor.py:821`, `bin/disjuntor.py:248`, `bin/disjuntor.py:257`, `bin/disjuntor.py:263`, `bin/disjuntor.py:1329`, `bin/hook_stop_fila1.py:1056`)

10. **A leitura final do opus também pode escapar pelo texto do prompt.** `LEITURA FINAL REPROVA-E-DESENHO-CODEX` reconhece “final”, mas não a obra, pois falta uma das âncoras esperadas. `Parecer final da obra REPROVA-E-DESENHO-CODEX` não reconhece “final” pelo vocabulário fechado. Incluir no histórico do prompt a frase `nao e a leitura final` desativa o reconhecimento de todo o prompt. Nessas formas a chamada não consulta B3. São limites preexistentes, mas afetam o item 3 pedido. (`bin/disjuntor.py:103`, `bin/disjuntor.py:111`, `bin/disjuntor.py:113`, `bin/disjuntor.py:500`, `bin/disjuntor.py:511`, `bin/disjuntor.py:1290`)

11. **O ESTADO pode mostrar zeros/“nenhuma” sem ter conseguido ler.** `ler` começa com contadores vazios e ignora `OSError` das trilhas; `linha` imprime B3=0, negativas=0 e liberações=0. Assim, trilha ausente/ilegível pode produzir aparência de normalidade mesmo com obra disparada no JSON; esse painel não lista os B3 ativos. Falha de leitura das chamadas Codex vira `None`, depois `{}`, depois “nenhuma”. O campo `b3_sem_obra`, especificamente, preserva “nao sei” quando o estado é ilegível. (`bin/disjuntor.py:1373`, `bin/disjuntor.py:1405`, `bin/disjuntor.py:1425`, `bin/disjuntor.py:1435`, `bin/disjuntor.py:1439`, `bin/disjuntor.py:1476`, `bin/disjuntor.py:1479`, `bin/disjuntor.py:1493`)

Há ainda uma diferença temporal relevante: a terceira triagem apenas anexa ao log; o disparo acontece na próxima decisão. Um DESENHO escrito entre esses dois momentos entra na fotografia do disparo e não libera. Isso não atende à leitura literal de “dispara quando houver três”; segue, porém, o protocolo existente de exigir linha posterior ao disparo observado. O contrato precisa escolher esse instante para eliminar a ambiguidade. (`bin/disjuntor.py:778`, `bin/disjuntor.py:825`, `bin/disjuntor.py:850`, `bin/disjuntor.py:859`, `bin/disjuntor.py:870`, `bin/disjuntor.py:883`)

**Não há fundamento para acusar escrita do JSON B3 fora da trava:** a gravação e os anúncios acontecem dentro de `_com_estado`, após `flock`. O estado da triagem e sua leitura antecipada são os pontos fora dessa proteção. A liberação/rearme das identidades já observadas também limpa as listas e impede sua reincorporação. (`bin/disjuntor.py:252`, `bin/disjuntor.py:273`, `bin/disjuntor.py:284`, `bin/disjuntor.py:287`, `bin/disjuntor.py:778`, `bin/disjuntor.py:821`, `bin/disjuntor.py:842`, `bin/disjuntor.py:876`, `bin/disjuntor.py:912`)

O selo e seus mutantes têm esta cobertura:

| Item | O que exercitam | O que fica sem caso que morde |
|---|---|---|
| **1** | Obras separadas; duas passam/três param; repetir a mesma triagem grava uma linha. Mutantes `sem-codex`, `limiar-2`, `por-achado`. (`bin/tests/test_disjuntor.sh:1147`, `bin/tests/test_disjuntor.sh:1150`, `bin/tests/test_disjuntor.sh:1153`, `bin/tests/test_disjuntor.sh:1156`, `bin/tests/test_disjuntor.sh:1249`, `bin/tests/test_disjuntor.sh:1251`, `bin/tests/test_disjuntor.sh:1259`) | Wrapper real, conservação/colisão da identidade e reutilização da resposta. A fixture escreve somente `achado`; `por-achado` muda a identidade a cada **triagem**, não testa resposta com vários achados. (`bin/tests/test_disjuntor.sh:1129`, `bin/tests/test_disjuntor.sh:1130`, `bin/tests/test_disjuntor.sh:1259`) |
| **2** | Descartadas desde o início e não triadas; mutante que inclui descartadas. (`bin/tests/test_disjuntor.sh:1140`, `bin/tests/test_disjuntor.sh:1144`, `bin/tests/test_disjuntor.sh:1250`) | **PROCEDENTE → decisão → DESCARTADO antes do disparo.** A reclassificação testada ocorre somente depois da liberação. (`bin/tests/test_disjuntor.sh:1169`) |
| **3** | Negativa de opus final e Codex, permissão de leitura, recado Stop e liberação DESENHO; mutante remove a negativa Codex. (`bin/tests/test_disjuntor.sh:1156`, `bin/tests/test_disjuntor.sh:1157`, `bin/tests/test_disjuntor.sh:1158`, `bin/tests/test_disjuntor.sh:1160`, `bin/tests/test_disjuntor.sh:1163`, `bin/tests/test_disjuntor.sh:1252`) | Ambiente real, sintaxes alternativas, falsas negativas/positivas do parser e falhas de leitura. Codex tampouco é a primeira borda após a terceira triagem: `_parada` já dispara antes. (`bin/tests/test_disjuntor.sh:1133`, `bin/tests/test_disjuntor.sh:1156`, `bin/tests/test_disjuntor.sh:1157`) |
| **4** | Recusa sidecar ausente, papel leitura e veredito inválido; mutante `sem-sidecar`. (`bin/tests/test_disjuntor.sh:1136`, `bin/tests/test_disjuntor.sh:1137`, `bin/tests/test_disjuntor.sh:1138`, `bin/tests/test_disjuntor.sh:1253`) | Existência/vínculo da resposta, procedência do sidecar, escritor exclusivo e concorrência. O próprio teste fabrica `.id`; a cópia dos mutantes inclui apenas `bin/*.py`. (`bin/tests/test_disjuntor.sh:1130`, `bin/tests/test_disjuntor.sh:1184`) |
| **5** | Liberação DESENHO, quarta volta isolada e reclassificação de volta já liberada; mutante deixa `codex` sem limpar na liberação. (`bin/tests/test_disjuntor.sh:1163`, `bin/tests/test_disjuntor.sh:1166`, `bin/tests/test_disjuntor.sh:1169`, `bin/tests/test_disjuntor.sh:1258`) | **Rearme Codex.** Os casos antigos rearmam reprovações opus. Retirar Codex da união de `rearmado`, mantendo a limpeza imediata, não é exercitado pelo caso novo. Também falta rearme com triagens anteriores ainda não varridas. (`bin/tests/test_disjuntor.sh:398`, `bin/tests/test_disjuntor.sh:1081`, `bin/tests/test_disjuntor.sh:1124`, `bin/tests/test_disjuntor.sh:1172`, `bin/disjuntor.py:912`) |

Os testes antigos acrescentam cobertura compartilhada de ERRO-BOBO e de anúncio sob a trava; não substituem os casos ausentes de triagem e rearme Codex. (`bin/tests/test_disjuntor.sh:386`, `bin/tests/test_disjuntor.sh:1063`, `bin/tests/test_disjuntor.sh:1081`)

O próprio selo ainda aceita ausência de sinal como sucesso em vários negativos: `_parada` procura apenas texto B3, sem verificar nova FALHA. Uma exceção que silencie a decisão pode passar nesses pontos, embora `_passa` tenha uma checagem mais forte. Portanto, **o item 5, no ramo de rearme Codex, é o que fica inteiramente sem caso específico que morde; os demais têm cobertura parcial e as lacunas acima.** (`bin/tests/test_disjuntor.sh:98`, `bin/tests/test_disjuntor.sh:1134`, `bin/tests/test_disjuntor.sh:1145`, `bin/tests/test_disjuntor.sh:1148`, `bin/tests/test_disjuntor.sh:1167`, `bin/tests/test_disjuntor.sh:1170`)

## 31/10 14:29 · sessao b13e3e31e25d · - · - · origem=Ronald (tmux)

Auditoria somente leitura dos itens 1, 2, 4 e 5:

- **Contagem nominal correta:** há limiar 3, isolamento por obra, deduplicação por identidade e arquivo com N achados conta uma identidade; achados da resposta nem são percorridos. A fonte considera apenas o último veredito PROCEDENTE. `bin/disjuntor.py:76`, `bin/disjuntor.py:773`, `bin/disjuntor.py:795`, `bin/disjuntor.py:801`, `bin/disjuntor.py:839`, `bin/disjuntor.py:844`, `bin/disjuntor.py:850`.

- **Defeito certo: DESCARTADO continua contando depois de observado PROCEDENTE.** A fonte é recalculada, mas `r['codex']` recebe apenas união; nenhuma identidade deixa a lista por reclassificação. Cenário: A PROCEDENTE → decisão guarda A; A vira DESCARTADO; B e C PROCEDENTE → decisão dispara com `[A,B,C]`, embora só B e C sejam procedentes. Reprodução feita somente em memória com mocks; o resultado decorre de `bin/disjuntor.py:801`, `bin/disjuntor.py:839`, `bin/disjuntor.py:845`, `bin/disjuntor.py:849`.

- **Defeito certo: rearme não zera as triagens já existentes ainda não observadas.** `rearma_b3` arquiva apenas as listas existentes no JSON e não consulta a triagem. Cenário: obra já existe com lista vazia; D/E/F estão PROCEDENTE no log; rearme retorna sucesso; próxima decisão incorpora D/E/F e dispara imediatamente. Reprodução em memória confirmou. `bin/disjuntor.py:908`, `bin/disjuntor.py:912`, `bin/disjuntor.py:913`, `bin/disjuntor.py:925`, `bin/disjuntor.py:839`.

- **Liberação/rearme dos itens efetivamente observados estão corretos:** movem as identidades para conjuntos de exclusão e limpam ambas as listas. Reclassificar uma identidade já liberada não a ressuscita. `bin/disjuntor.py:842`, `bin/disjuntor.py:874`, `bin/disjuntor.py:876`, `bin/disjuntor.py:912`.

- **Estado da triagem fora da trava:** o check de idempotência e o append não usam flock; a leitura para decidir ocorre antes da trava do JSON. Dois processos podem anexar a mesma triagem duas vezes, embora isso **não** conte duas voltas porque o leitor reduz por identidade. Uma decisão pode usar veredito obsoleto, ou uma liberação deixar de consumir voltas procedentes anexadas entre a leitura e a transição. `bin/disjuntor.py:774`, `bin/disjuntor.py:778`, `bin/disjuntor.py:795`, `bin/disjuntor.py:821`, `bin/disjuntor.py:897`. O JSON B3 é efetivamente escrito sob flock; não encontrei outra gravação desse JSON fora da trava nos arquivos examinados. `bin/disjuntor.py:248`, `bin/disjuntor.py:252`, `bin/disjuntor.py:267`, `bin/disjuntor.py:273`.

- **Identidade insuficientemente vinculada à resposta:** `_le_id` verifica somente cinco campos não vazios; não verifica existência da resposta, conteúdo, hash, mtime, proveniência do wrapper ou correspondência com a chamada. Só o sidecar basta para triagem. `bin/disjuntor.py:752`, `bin/disjuntor.py:755`, `bin/disjuntor.py:759`, `bin/disjuntor.py:768`.

- **Sidecar obsoleto permite triar resposta de outro papel:** o wrapper aceita reutilizar RESPOSTA e só substitui `.id` no sucesso; não invalida identidade anterior no começo nem no erro. Cenário: R.id antigo de verificador; nova chamada `leitura` escreve R e termina com erro; a triagem aceita a resposta nova como verificador antigo. `bin/codex_pergunta.sh:40`, `bin/codex_pergunta.sh:158`, `bin/codex_pergunta.sh:178`, `bin/disjuntor.py:771`.

- **Reutilização/sobrescrita perde ou atribui indevidamente identidade:** duas chamadas bem-sucedidas para o mesmo caminho deixam só o último `.id`, impedindo triagem posterior da anterior; RC0 sem resposta nova, mas com R antigo existente, gera nova identidade para conteúdo antigo; falha ao abrir `.id` para gravar pode preservar identidade anterior e deduplicar a nova chamada como antiga. `bin/codex_pergunta.sh:158`, `bin/codex_pergunta.sh:178`, `bin/codex_pergunta.sh:180`, `bin/disjuntor.py:773`, `bin/disjuntor.py:774`.

- **Colisão estrutural de identidade subconta chamadas:** usa somente hash12 + PID + término com precisão de segundos. Com PID reciclado e mesmo segundo — ou relógio recuado — duas chamadas distintas podem fundir-se; também podem coincidir com identidade já liberada/rearmada e nunca contar. Ocorrência real: [nao verificado]. `bin/codex_pergunta.sh:179`, `bin/disjuntor.py:773`, `bin/disjuntor.py:842`.

- **Fonte ausente ou linhas corrompidas viram ausência de voltas:** log inexistente retorna `{}`; linhas fora do formato são ignoradas, sem representar “não sei”. Se não houver identidades previamente acumuladas, uma decisão permite prosseguir mesmo sem fonte suficiente para concluir zero. `bin/disjuntor.py:788`, `bin/disjuntor.py:790`, `bin/disjuntor.py:794`, `bin/disjuntor.py:801`.

Sobre descartadas anteriores à liberação: elas nunca integraram a contagem e não entram em `liberado`; reclassificá-las depois pode fazê-las contar. Isso é demonstrável no código, mas considerar toda volta anterior automaticamente consumida exige interpretação além de “voltas já liberadas não voltam a contar”. `bin/disjuntor.py:801`, `bin/disjuntor.py:874`, `bin/disjuntor.py:842`.

## 31/10 14:29 · sessao f6faf85cf3dc · - · - · origem=Ronald (tmux)

Achados de bloqueio e leitura, por inspeção da cópia; ocorrência em produção [nao verificado].

1. **Codex pode passar com B3 disparado.** O bloqueio depende exclusivamente da obra e papel extraídos de atribuições literais anteriores ao comando; o wrapper lê o ambiente efetivo e executa sem consultar B3. Cenários que o parser confirmou em memória:
   - `env CODEX_OBRA=OBRA-C CODEX_PAPEL=verificador bin/codex_pergunta.sh h q r` → nenhuma chamada reconhecida.
   - `export CODEX_OBRA=OBRA-C CODEX_PAPEL=verificador; bin/codex_pergunta.sh h q r` → papel `?`, obra vazia.
   - `CODEX_OBRA="$OBRA" CODEX_PAPEL="$PAPEL" ...` → expansão não resolvida.
   - `bash -c "CODEX_OBRA=OBRA-C CODEX_PAPEL=verificador ..."` → nenhuma chamada reconhecida.
   - Execução por outro script ou fora do PreToolUse também não encontra guarda dentro do wrapper. Fontes: `bin/disjuntor.py:1120`, `bin/disjuntor.py:1186`, `bin/disjuntor.py:1280`, `bin/codex_pergunta.sh:44`, `bin/codex_pergunta.sh:157`. Alguns limites já são expressamente declarados como resíduos em `bin/disjuntor.py:1117`.

2. **Negação indevida ou ausência de negação por primeira atribuição.** `CODEX_PAPEL=verificador CODEX_PAPEL=leitura` é lido como verificador e negado, embora o wrapper receba leitura; ordem inversa libera verificador. O mesmo ocorre com `CODEX_OBRA=OBRA-C CODEX_OBRA=OBRA-LIVRE` e ordem inversa. Além disso, `OUTRA="CODEX_PAPEL=verificador" CODEX_PAPEL=leitura` é lido como verificador. Fontes: `bin/disjuntor.py:1195`, `bin/disjuntor.py:1196`, `bin/disjuntor.py:1198`, `bin/disjuntor.py:1280`, `bin/codex_pergunta.sh:44`.

3. **Obra com espaços é truncada.** `CODEX_OBRA="O9 (C)" CODEX_PAPEL=verificador ...` vira obra `O9`, embora wrapper preserve `O9 (C)`; pode liberar obra parada `O9 (C)` ou negar indevidamente devido a B3 de `O9`. Essa identidade com cauda é explicitamente suportada pelo leitor de obras do opus. Fontes: `bin/disjuntor.py:1198`, `bin/disjuntor.py:1280`, `bin/disjuntor.py:525`, `bin/codex_pergunta.sh:44`.

4. **Falha de leitura abre ambas as portas.** `decide_b3` lê a triagem antes do estado; `triagem.log` como diretório ou sem permissão levanta, pois só ausência é capturada. Isso libera Codex e opus mesmo se JSON já disser `disparado=true`. Estado JSON inválido, trava ocupada ou falha de abrir a trava também levantam e encontram o mesmo caminho que libera a ferramenta. Fontes: `bin/disjuntor.py:821`, `bin/disjuntor.py:788`, `bin/disjuntor.py:790`, `bin/disjuntor.py:248`, `bin/disjuntor.py:257`, `bin/disjuntor.py:263`, `bin/disjuntor.py:1329`. No Stop, a falha apenas omite o recado B3: `bin/hook_stop_fila1.py:1048`, `bin/hook_stop_fila1.py:1056`.

5. **Opus: reconhecimento textual preexistente deixa passar leitura final.** `LEITURA FINAL REPROVA-E-DESENHO-CODEX` é final mas não identifica obra; `Parecer final da obra REPROVA-E-DESENHO-CODEX` não é reconhecido como final; adicionar ao histórico `nao e a leitura final` invalida o reconhecimento de todo o prompt. Portanto B3 não é consultado nesses casos. Fontes: `bin/disjuntor.py:103`, `bin/disjuntor.py:111`, `bin/disjuntor.py:113`, `bin/disjuntor.py:498`, `bin/disjuntor.py:503`, `bin/disjuntor.py:1290`. Outro `subagent_type` também passa pelo B3, mas isso coincide com a exclusão contratual de outros papéis; classificar uma leitura como verificador apesar de outro tipo seria interpretação [nao verificado]. Fonte: `bin/disjuntor.py:1290`.

6. **Leitura produz zeros/“nenhuma” sem dado.** `ler` inicializa disparos, negativas e liberações em zero, ignora `OSError` das trilhas e `linha` apresenta esses zeros; não consulta obras B3 paradas, apenas `b3_sem_obra`. Se a trilha está ausente/ilegível, pode exibir B3=0 mesmo com estado disparado. Falha de `ler_chamadas` produz `None`, convertido para `{}` e depois “nenhuma”. Fontes: `bin/disjuntor.py:1373`, `bin/disjuntor.py:1405`, `bin/disjuntor.py:1425`, `bin/disjuntor.py:1435`, `bin/disjuntor.py:1476`, `bin/disjuntor.py:1479`, `bin/disjuntor.py:1493`, `bin/disjuntor.py:1501`. Não confundir com estado ilegível para `b3_sem_obra`: esse campo preserva “nao sei”. Fonte: `bin/disjuntor.py:1439`.

7. **Não encontrei escrita do JSON B3 fora da trava.** A única escrita desse JSON está em `_com_estado`, depois do `flock` e antes do fechamento do descritor. A triagem é apendada fora dessa trava e sua fotografia é lida antes de adquiri-la; são problemas distintos. Fontes: `bin/disjuntor.py:252`, `bin/disjuntor.py:270`, `bin/disjuntor.py:287`, `bin/disjuntor.py:778`, `bin/disjuntor.py:821`.

8. **Ressalva explícita de operação:** a chave `DISJUNTOR_DESLIGADO` suspende o PreToolUse e o recado Stop. É uma exceção preexistente declarada, não uma regressão escondida do diff. Fontes: `bin/disjuntor.py:34`, `bin/disjuntor.py:1320`, `bin/hook_stop_fila1.py:1039`.

9. **Janela entre terceira triagem e disparo persistido:** `triagem` só apenda; se DESENHO válido for escrito após a terceira triagem e antes da primeira decisão B3, a decisão dispara, fotografa essa linha como anterior e nega. Pelo contrato literal de disparar ao alcançar três, é atraso de disparo com negação adicional; pelo protocolo preexistente que exige linha posterior ao disparo observado, é comportamento esperado. Fontes: `bin/disjuntor.py:778`, `bin/disjuntor.py:825`, `bin/disjuntor.py:850`, `bin/disjuntor.py:859`, `bin/disjuntor.py:870`, `bin/disjuntor.py:883`.

O caso `b3_codex` usa apenas atribuições literais, obras sem espaços e sidecars fabricados; seus mutantes não exercitam wrapper real, variantes de ambiente, atribuições repetidas, falhas de leitura nem saída de estado sem dados. Fontes: `bin/tests/test_disjuntor.sh:1128`, `bin/tests/test_disjuntor.sh:1133`, `bin/tests/test_disjuntor.sh:1157`, `bin/tests/test_disjuntor.sh:1249`.

## 31/10 14:28 · sessao de83cf872bd2 · - · - · origem=Ronald (tmux)

O selo cobre percursos básicos, mas não fecha os cinco itens; os sete mutantes Codex alteram apenas `disjuntor.py`, e o teste fabrica resposta e identidade sem executar o wrapper. (`bin/tests/test_disjuntor.sh:1128`, `bin/tests/test_disjuntor.sh:1130`, `bin/tests/test_disjuntor.sh:1249`, `bin/tests/test_disjuntor.sh:1260`)

| Item | Cobertura efetiva | Lacuna |
|---|---|---|
| 1 — três chamadas distintas da mesma obra | Isolamento entre obras, duas passam/três bloqueiam, repetição da mesma triagem produz uma linha. Mutantes `sem-codex`, `limiar-2` e `por-achado`. (`bin/tests/test_disjuntor.sh:1147`, `bin/tests/test_disjuntor.sh:1150`, `bin/tests/test_disjuntor.sh:1153`, `bin/tests/test_disjuntor.sh:1156`, `bin/tests/test_disjuntor.sh:1249`, `bin/tests/test_disjuntor.sh:1251`, `bin/tests/test_disjuntor.sh:1259`) | Não testa identidade produzida pelo wrapper, reutilização do caminho da resposta, colisões ou chamadas concorrentes; `_rcx` sempre fabrica identidades com pids diferentes e resposta contendo apenas `achado`. (`bin/tests/test_disjuntor.sh:1128`, `bin/tests/test_disjuntor.sh:1130`, `bin/tests/test_disjuntor.sh:1150`, `bin/tests/test_disjuntor.sh:1155`) |
| 2 — não triada/descartada não conta | Três descartadas iniciais, três não triadas; mutante que conta descartadas. (`bin/tests/test_disjuntor.sh:1140`, `bin/tests/test_disjuntor.sh:1144`, `bin/tests/test_disjuntor.sh:1250`) | Falta `PROCEDENTE → DESCARTADO` antes do disparo: o acumulador apenas acrescenta e não reconcilia a retirada, permitindo disparo com duas procedentes atuais. O único caso de reclassificação ocorre depois da liberação. (`bin/disjuntor.py:839`, `bin/disjuntor.py:845`, `bin/disjuntor.py:849`, `bin/tests/test_disjuntor.sh:1169`) |
| 3 — bloqueios e liberação | Bloqueia Agent final, Codex verificador e Stop; permite papel leitura; libera com DESENHO. Mutante remove negativa Codex. (`bin/tests/test_disjuntor.sh:1156`, `bin/tests/test_disjuntor.sh:1157`, `bin/tests/test_disjuntor.sh:1158`, `bin/tests/test_disjuntor.sh:1160`, `bin/tests/test_disjuntor.sh:1163`, `bin/tests/test_disjuntor.sh:1252`) | B3 Codex usa apenas comando canônico com variáveis literais; testes de outras sintaxes pertencem ao caso de orçamento `codex`, sem obra parada por B3. Não testa Codex como primeira borda após a terceira triagem: `_parada` já provoca o disparo antes. (`bin/tests/test_disjuntor.sh:1133`, `bin/tests/test_disjuntor.sh:1156`, `bin/tests/test_disjuntor.sh:1157`, `bin/tests/test_disjuntor.sh:562`, `bin/tests/test_disjuntor.sh:569`) |
| 4 — escritor e identidade | Recusa sidecar ausente, papel leitura e veredito inválido; mutante `sem-sidecar`. (`bin/tests/test_disjuntor.sh:1136`, `bin/tests/test_disjuntor.sh:1137`, `bin/tests/test_disjuntor.sh:1138`, `bin/tests/test_disjuntor.sh:1253`) | Não verifica origem real do sidecar, existência/conteúdo da resposta, escrita exclusiva nem concorrência; a própria fixture fabrica o lastro e `_mutante` copia só arquivos Python. (`bin/tests/test_disjuntor.sh:1129`, `bin/tests/test_disjuntor.sh:1130`, `bin/tests/test_disjuntor.sh:1184`) |
| 5 — zerar e não ressuscitar | Liberação DESENHO, quarta volta isolada e reclassificação de volta liberada; mutante que deixa toda a lista Codex na liberação. (`bin/tests/test_disjuntor.sh:1163`, `bin/tests/test_disjuntor.sh:1166`, `bin/tests/test_disjuntor.sh:1169`, `bin/tests/test_disjuntor.sh:1258`) | **Rearme de Codex não tem caso que morde.** O rearme antigo usa reprovações Opus; o caso de atomicidade também usa `reprova`. Retirar Codex da união de `rearmado`, mantendo a limpeza imediata, deixaria as voltas voltarem na decisão seguinte sem ser exercitado pelo selo Codex. (`bin/tests/test_disjuntor.sh:398`, `bin/tests/test_disjuntor.sh:1081`, `bin/tests/test_disjuntor.sh:1124`, `bin/tests/test_disjuntor.sh:1172`, `bin/disjuntor.py:912`, `bin/disjuntor.py:913`, `bin/disjuntor.py:842`) |

Os testes antigos acrescentam cobertura compartilhada de ERRO-BOBO, validade do topo, gravação e trava; seus estados são alimentados por reprovações Opus, portanto não substituem casos sobre a triagem Codex concorrente ou seu rearme. (`bin/tests/test_disjuntor.sh:386`, `bin/tests/test_disjuntor.sh:398`, `bin/tests/test_disjuntor.sh:779`, `bin/tests/test_disjuntor.sh:834`, `bin/tests/test_disjuntor.sh:1063`)

Há também falso positivo no próprio selo: `_parada` só procura texto B3, e vários negativos aceitam qualquer saída sem esse texto; uma falha que silencie o bloqueio pode passar nesses pontos. `_passa` verifica ausência de nova FALHA, mas não envolve esses negativos. (`bin/tests/test_disjuntor.sh:98`, `bin/tests/test_disjuntor.sh:100`, `bin/tests/test_disjuntor.sh:1134`, `bin/tests/test_disjuntor.sh:1145`, `bin/tests/test_disjuntor.sh:1148`, `bin/tests/test_disjuntor.sh:1167`, `bin/tests/test_disjuntor.sh:1170`)

Resultado da execução da suíte e mortes efetivamente observadas dos mutantes: [nao verificado]. [nao verificado]

## 31/10 14:05 · sessao 9f9ab0ecebe5 · LEI-AKITA-UNIVERSO-DO-PUSH · verificador · origem=Code

**Aval: VERMELHO.** A volta corrige a travessia das raias e a comparação dos merges, mas ainda permite omitir o julgamento de curas e pode julgar um HEAD diferente do commit enviado. (`bin/tests/test_lei_akita.sh:135`, `bin/tests/test_lei_akita.sh:139`, `bin/tests/test_lei_akita.sh:144`; `bin/pre-push.sh:22`, `bin/pre-push.sh:36`)

Comparei integralmente os dois arquivos com o snapshot do pai disponível. Autenticidade dos SHAs pelo Git: **[nao verificado]**, pois esta cópia não contém `.git`. O MORDE integral não foi executado: ele cria arquivos temporários; executei somente seus 13 casos unitários e contraprovas em memória. (`bin/tests/test_lei_akita.sh:153`, `bin/tests/test_lei_akita.sh:168`, `bin/tests/test_lei_akita.sh:180`, `bin/tests/test_lei_akita.sh:184`)

1. **Universo efetivamente julgado**

   A enumeração corresponde a:

   `ancestrais(HEAD) − ancestrais(origin/main) − ancestrais(primeiro pai do corte)`

   O último desconto só existe quando o corte tem pai. Depois dessa enumeração, somente commits com linha iniciada por `LEI-AKITA:` chegam ao juiz. (`bin/tests/test_lei_akita.sh:127`, `bin/tests/test_lei_akita.sh:132`, `bin/tests/test_lei_akita.sh:135`, `bin/tests/test_lei_akita.sh:139`)

   | Situação | Comportamento |
   |---|---|
   | HEAD igual a origin/main | Enumera zero e julga zero. Pode ficar VERDE se as demais verificações passarem; esse vazio é correto para o contrato. (`bin/tests/test_lei_akita.sh:135`, `bin/tests/test_lei_akita.sh:136`, `bin/tests/test_lei_akita.sh:243`) |
   | Commits locais à frente | Enumera todos os alcançáveis exclusivamente por HEAD, respeitando o piso; pula os sem linha LEI-AKITA. (`bin/tests/test_lei_akita.sh:132`, `bin/tests/test_lei_akita.sh:135`, `bin/tests/test_lei_akita.sh:139`) |
   | Merge de raia, dois pais | Inclui o merge e os commits exclusivos da raia: saiu `--first-parent`. Cada um passa pelo mesmo filtro de mensagem. (`bin/tests/test_lei_akita.sh:130`, `bin/tests/test_lei_akita.sh:135`, `bin/tests/test_lei_akita.sh:137`) |
   | origin/main ausente | `rev-list` falha; vira `PADRAO_ERRO` e falta `PADRAO_FIM`, portanto VERMELHO. (`bin/tests/test_lei_akita.sh:121`, `bin/tests/test_lei_akita.sh:135`, `bin/tests/test_lei_akita.sh:237`) |
   | Repositório raso | Rejeita antes de procurar o corte ou enumerar commits. (`bin/tests/test_lei_akita.sh:125`, `bin/tests/test_lei_akita.sh:126`) |
   | Corte ausente | Rejeita explicitamente; deixou de interpretar ausência como universo vazio. (`bin/tests/test_lei_akita.sh:127`, `bin/tests/test_lei_akita.sh:128`, `bin/tests/test_lei_akita.sh:129`) |

   “Publicado” significa aqui **alcançável pela referência local `origin/main`**. Não há consulta ao servidor nessa função; referência desatualizada pode fazer julgar história já publicada. Estado real dessa referência: **[nao verificado]**. (`bin/tests/test_lei_akita.sh:118`, `bin/tests/test_lei_akita.sh:135`)

2. **Lista de arquivos**

   | Commit | Lista usada | Aval | [nao verificado]
   |---|---|---|
   | Comum | `git diff --name-only h^1 h` | Representa a alteração contra seu pai. (`bin/tests/test_lei_akita.sh:143`, `bin/tests/test_lei_akita.sh:144`) |
   | Merge | O mesmo diff, contra o primeiro pai | Representa o que o pouso acrescenta à linha receptora, incluindo resoluções; corrige o vazio que `show` podia devolver. (`bin/tests/test_lei_akita.sh:142`, `bin/tests/test_lei_akita.sh:144`) |
   | Raiz | `git show --name-only --format= h` | Busca os arquivos introduzidos pela raiz. (`bin/tests/test_lei_akita.sh:143`, `bin/tests/test_lei_akita.sh:144`) |

   Para caminhos comuns, a escolha corresponde ao que cada commit traz. Entretanto, a saída é textual, sem `-z`, e é passada por `splitlines()`; negócio depende de o texto começar literalmente com `app/`. Um caminho recebido entre aspas, como `"app/ponto/a\303\247ao.py"`, passa como não negócio e libera `n/a`. A falha do classificador foi reproduzida em memória; reprodução da emissão desse caminho pelo Git real: **[nao verificado]**. (`bin/tests/test_lei_akita.sh:92`, `bin/tests/test_lei_akita.sh:144`, `bin/tests/test_lei_akita.sh:145`)

3. **Vocabulário aceito**

   São **55 termos normalizados**. O selo ignora caixa, normaliza espaços e remove crases, asteriscos e conteúdo entre parênteses. Além deles, aceita exatamente `n/a (medida)` e `n/a (instrumento)` quando não identifica arquivo de negócio. (`bin/tests/test_lei_akita.sh:61`, `bin/tests/test_lei_akita.sh:83`, `bin/tests/test_lei_akita.sh:90`, `bin/tests/test_lei_akita.sh:99`)

   | Fonte | Termos aceitos |
   |---|---|
   | `app/docs/GLOSSARIO.md:11` | single source of truth; aggregate root; celula soberana; fonte unica |
   | `app/docs/GLOSSARIO.md:12` | single writer principle; juiz unico; escritor unico |
   | `app/docs/GLOSSARIO.md:13` | sinal derivado do estado; reconciliacao level-triggered; lampada |
   | `app/docs/GLOSSARIO.md:14` | architectural invariants + fitness functions; contratos estruturais |
   | `app/docs/GLOSSARIO.md:15` | architecture conformance test; selo |
   | `app/docs/GLOSSARIO.md:16` | event-driven architecture; crons virando evento |
   | `app/docs/GLOSSARIO.md:17` | transactional outbox; o-outbox |
   | `app/docs/GLOSSARIO.md:18` | strangler fig pattern; construir ao lado e trocar inteira |
   | `app/docs/GLOSSARIO.md:19` | shadow mode; parallel run; sombra |
   | `app/docs/GLOSSARIO.md:20` | merge queue; comboio; pouso |
   | `app/docs/GLOSSARIO.md:21` | backfill; reprocessing; rejulgar o passivo |
   | `app/docs/GLOSSARIO.md:22` | cqrs com projecoes reconstruiveis; espelho; diapago; fechamento; retrato e pilulas |
   | `app/docs/GLOSSARIO.md:23` | duplicated business logic; logic drift; derivacao paralela |
   | `app/docs/GLOSSARIO.md:24` | bitemporal data; vinculo do dia x vinculo de hoje |
   | `app/docs/GLOSSARIO.md:25` | immutable event log + compensating event; batida so retratada |
   | `app/docs/GLOSSARIO.md:26` | adr; leis; cortes |
   | `app/docs/GLOSSARIO.md:27` | idempotencia pela chave do fato; dedup do fato |
   | `app/docs/PLANO-PADROES.md:46`, `app/docs/PLANO-PADROES.md:47`, `app/docs/PLANO-PADROES.md:48` — adicionais | um juiz por pergunta; single writer; evento em vez de varredura; log imutavel; outbox; bitemporal; projecao reconstruivel; cqrs; fitness function |

   **Não coincide integralmente com a redação da lei.** A enumeração acompanha o NORTE e as duas primeiras colunas do glossário, mas a CLAUDE admite termo existente nos dois arquivos, e a própria L-123 repete essa redação ampla. (`app/docs/PLANO-PADROES.md:48`, `app/docs/PLANO-PADROES.md:49`, `app/docs/PLANO-PADROES.md:53`; `CLAUDE.md:68`)

   Recusas comprovadas na função real:

   - `Architecture Decision Record`: aparece explicitamente como padrão conhecido e também no glossário; é perdido pela remoção dos parênteses e pela restrição ao NORTE. (`app/docs/PLANO-PADROES.md:41`; `app/docs/GLOSSARIO.md:26`; `bin/tests/test_lei_akita.sh:78`, `bin/tests/test_lei_akita.sh:84`)
   - `projection rebuild` e `read model replay`: aparecem como padrões conhecidos no plano, mas ficam fora do trecho extraído. (`app/docs/PLANO-PADROES.md:33`; `bin/tests/test_lei_akita.sh:78`, `bin/tests/test_lei_akita.sh:104`)

   No sentido contrário, aceita `padrao=juiz unico (Outro Padrao Totalmente Novo)`: elimina o complemento antes da comparação. Esse valor integral não é um dos termos enumerados. (`bin/tests/test_lei_akita.sh:62`, `bin/tests/test_lei_akita.sh:104`)

4. **Caminhos de VERDE sem o julgamento devido** [nao verificado]

   - **Cura sem linha LEI-AKITA:** é pulada. Uma mensagem contendo somente `cura` e `padrao=Outro Padrao Totalmente Novo` não produz falha. As verificações anteriores cobram a menção no molde, não a presença em cada commit. Isso deixa descoberta a exigência de que nenhuma cura entre sem padrão. (`bin/tests/test_lei_akita.sh:45`, `bin/tests/test_lei_akita.sh:46`, `bin/tests/test_lei_akita.sh:139`; `CLAUDE.md:66`)
   - **Campo falso ou duplicado:** `xpadrao=juiz unico` é aceito; `padrao=Padrao Inventado Do Nada, padrao=juiz unico` também. A regex não exige fronteira de campo e só usa a última ocorrência. Ambas as contraprovas passaram na função real. (`bin/tests/test_lei_akita.sh:95`, `bin/tests/test_lei_akita.sh:98`)
   - **Ref enviada diferente de HEAD:** o selo pode julgar universo vazio de HEAD enquanto outra ref enviada contém commit ruim. A seleção do SHA enviado acontece depois do selo. (`bin/tests/test_lei_akita.sh:135`; `bin/pre-push.sh:22`, `bin/pre-push.sh:36`)
   - **Régua com carimbo:** retorna “JA VERDE” antes de executar a pasta de selos; sua condição compara arquivos Python/HTML, sem identidade das mensagens ou do universo Git. (`bin/regua.sh:33`, `bin/regua.sh:84`, `bin/regua.sh:93`, `bin/regua.sh:138`)

5. **Os MORDE mordem, mas deixam mutantes sobreviver**

   Os 13 casos unitários exercitam a função real e distinguem termo inventado, substring, ausência/vazio, `n/a` em negócio e duas linhas com uma inválida. O cenário Git procura quatro commits ruins, incluindo raia e merge, excluindo o publicado. (`bin/tests/test_lei_akita.sh:153`, `bin/tests/test_lei_akita.sh:168`, `bin/tests/test_lei_akita.sh:187`, `bin/tests/test_lei_akita.sh:200`)

   Mutantes concretos:

   | Mutante | Por que sobrevive |
   |---|---|
   | Trocar `_limpo(termo) not in VOCAB` por `_limpo(termo) in {'padrao inventado do nada', 'a', 'writer principle'}` | Passou nos 13 casos executados em memória e aceitou `Outro Padrao Totalmente Novo`. O cenário Git também usa somente aquele inventado literal; os demais controles permanecem iguais. (`bin/tests/test_lei_akita.sh:104`, `bin/tests/test_lei_akita.sh:154`, `bin/tests/test_lei_akita.sh:160`, `bin/tests/test_lei_akita.sh:189`, `bin/tests/test_lei_akita.sh:195`) |
   | Na ausência do corte, usar `corte=['HEAD']` | O teste “sem corte” também não tem `origin/main`; falharia por essa segunda ausência e passaria, porque aceita qualquer `PADRAO_ERRO`. (`bin/tests/test_lei_akita.sh:128`, `bin/tests/test_lei_akita.sh:135`, `bin/tests/test_lei_akita.sh:191`, `bin/tests/test_lei_akita.sh:218`, `bin/tests/test_lei_akita.sh:223`) |
   | Remover `*pisos` do `rev-list` | O corte plantado é raiz, logo seu piso já é vazio. Não testa a não retroatividade com corte não raiz. (`bin/tests/test_lei_akita.sh:132`, `bin/tests/test_lei_akita.sh:135`, `bin/tests/test_lei_akita.sh:182`) |
   | Comparar merges com o segundo pai | Ambos os lados plantados alteram negócio; o merge com `n/a` continuaria vermelho contra qualquer pai. (`bin/tests/test_lei_akita.sh:144`, `bin/tests/test_lei_akita.sh:188`, `bin/tests/test_lei_akita.sh:194`, `bin/tests/test_lei_akita.sh:197`) |
   | Rejeitar qualquer universo vazio | Não existe caso saudável `HEAD==origin/main`; o cenário principal exige quatro ruins. Esse mutante bloquearia o main sincronizado sem reprovar o MORDE. (`bin/tests/test_lei_akita.sh:135`, `bin/tests/test_lei_akita.sh:200`) |

   Sobrevivência integral desses mutantes com Git real: **[nao verificado]**; além do primeiro teste unitário executado, a conclusão acima é por inspeção dos caminhos. [nao verificado]

6. **Chamadores e bloqueio de push legítimo** [nao verificado]

   A régua executa todos os `bin/tests/test_*.sh` e aborta no primeiro vermelho. A chamada nova no pre-push ocorre antes do atalho do arsenal, fechando o escape “régua já verde” para o push comum de HEAD. `SKIP_TESTS=1` continua desligando tudo. (`bin/regua.sh:138`, `bin/regua.sh:140`, `bin/regua.sh:143`; `bin/pre-push.sh:5`, `bin/pre-push.sh:22`, `bin/pre-push.sh:65`)

   **Sim, pode bloquear push legítimo:** exclusão de branch e envio de outra ref válida passam pelo julgamento de HEAD antes de o hook reconhecer o alvo. Um commit inválido apenas no HEAD basta para barrá-los. O seletor existente já distingue exclusão e SHA enviado, mas é chamado depois. (`bin/pre-push.sh:22`, `bin/pre-push.sh:36`, `bin/pre-push.sh:37`; `app/core/arvore_do_push.py:23`, `app/core/arvore_do_push.py:34`, `app/core/arvore_do_push.py:43`)

   Há também raiz fixa: o wrapper delega ao pre-push do repositório corrente, mas a chamada do selo aponta para `~/saas-hasner`, e o selo usa `/home/ronald/saas-hasner` por padrão. Em clone/worktree, pode avaliar outra árvore. Instalação real do hook: **[nao verificado]**. (`bin/hooks.sh:56`; `bin/pre-push.sh:22`; `bin/tests/test_lei_akita.sh:17`)

7. **Outros defeitos e limites**

   - O vocabulário e as verificações estruturais vêm dos arquivos no disco, enquanto mensagens e caminhos vêm dos commits. Uma alteração documental ainda não commitada pode mudar o vocabulário que aprova o push. (`bin/tests/test_lei_akita.sh:28`, `bin/tests/test_lei_akita.sh:70`, `bin/tests/test_lei_akita.sh:77`, `bin/tests/test_lei_akita.sh:138`)
   - O preparo dos repositórios MORDE descarta o código de retorno dos comandos Git. Erros de preparação podem ser confundidos com o erro que o caso queria provar; o teste sem corte exemplifica essa falta de isolamento. (`bin/tests/test_lei_akita.sh:176`, `bin/tests/test_lei_akita.sh:178`, `bin/tests/test_lei_akita.sh:218`, `bin/tests/test_lei_akita.sh:224`)
   - O selo confere nome e classificação de caminhos; não confere se o diff realiza o padrão declarado. Essa obrigação permanece explicitamente com o verificador humano, portanto um VERDE mecânico não a certifica. (`bin/tests/test_lei_akita.sh:104`, `bin/tests/test_lei_akita.sh:145`; `CLAUDE.md:70`; `app/docs/PLANO-PADROES.md:58`)

## 31/10 14:04 · sessao d089eab7c55b · CLAUDE-MD-RACIONAL · verificador · origem=Code

**As três correções recuperam o alcance anterior; o PRONTO numérico confere.** A literalidade é do núcleo operativo, não uma reprodução integral dos parágrafos antigos. Fontes comparadas abaixo.

1. **As três cláusulas**

   **(4) Moratória L-117 — mesmo alcance.**

   Antes:

   > Ate `contratos_estruturais` = **20/20** (juiz `core/contratos_estruturais.py::total()`, hoje 15/20), o Code **nao numera nem propoe lei nova por iniciativa propria**: lei nova so nasce de CORTE LITERAL dele.

   `a7b66d14:CLAUDE.md:644–646`.

   Volta 8:

   > Ate 20/20 o Code nao numera nem propoe lei nova por iniciativa propria: lei nova so nasce de CORTE LITERAL dele (L-117).

   `2f5e38c6:CLAUDE.md:455`.

   Foram recuperados tanto **“lei nova por iniciativa propria”** quanto a exceção **“CORTE LITERAL dele”**. O encaminhamento da pergunta com números e continuidade da fila permanece no mesmo item; “Ate 20/20” preserva o término da moratória. O contador e seu juiz continuam identificados no arquivo. `a7b66d14:CLAUDE.md:646–647`; `2f5e38c6:CLAUDE.md:453–457`, `:259–263`.

   **(5) AUTONOMIA-DE-CODIGO, item 2 — mesmo alcance.**

   Antes:

   > **qual raia ou agente pega qual obra**, e quando uma obra registrada entra como COMPLEMENTO de outra;

   `a7b66d14:CLAUDE.md:481–482`.

   Volta 8:

   > qual raia ou agente pega qual obra e quando uma obra registrada entra como COMPLEMENTO de outra;

   `2f5e38c6:CLAUDE.md:461–462`.

   Recupera **obra registrada como complemento de outra**, mantendo decisão do Code, registro em uma linha e dispensa de aval. O destino de achados continua sendo cláusula separada. `a7b66d14:CLAUDE.md:479`, `:487`; `2f5e38c6:CLAUDE.md:460–462`.

   **(7a) CODEX-PRE-VERIFICADOR — mesmo alcance geral.**

   Antes:

   > antes de declarar pronto, Codex `CODEX_PAPEL=verificador` (diff + leis dos sitios + classes que ja reprovaram)

   `a7b66d14:CLAUDE.md:507`.

   Volta 8:

   > PRE-VERIFICADOR. (a) TODO construtor, ANTES de declarar pronto, `CODEX_PAPEL=verificador`

   `2f5e38c6:CLAUDE.md:543`.

   A redação anterior não limitava a obrigação a cura de negócio; o texto apontado por ela também dizia **“O construtor, ANTES de declarar pronto”**. A volta 8 elimina a restrição introduzida pelo pai, sem criar nova categoria de sujeitos. `a7b66d14:CLAUDE.md:507`; `a7b66d14:app/docs/PISTA.md:100–101`; `034b0e38:CLAUDE.md:543`.

2. **Os cortes dos cabeçalhos não retiram regra, condição ou ponteiro exclusivo.**

   - **PLACAR:** saiu somente “resposta a uma pergunta de LEI que eu levantei”. Essa informação histórica não se repete, mas não condicionava a regra. Permanecem autor/data, exercício, dois verdes, L-095 e `lap-18`. `034b0e38:CLAUDE.md:566`; `2f5e38c6:CLAUDE.md:566–570`.
   - **“historia em LAPIDES”:** saíram apenas essas palavras dos cabeçalhos de PARADA, RAIAS e DINHEIRO. Os ponteiros específicos permanecem respectivamente em `:443`, `:482` e `:574`. `034b0e38:CLAUDE.md:440`, `:479`, `:573`; `2f5e38c6:CLAUDE.md:443`, `:482`, `:574`.

3. **PRONTO conferido em bytes, sem normalizar o texto.**

   | Medida | Resultado |
   |---|---:|
   | 7b, do início do título até antes de `## 8.` | **20.955 B ≤ 21.000 B** |
   | Arquivo inteiro | **57.756 B ≤ 62.000 B** |

   Fontes medidas: `2f5e38c6:CLAUDE.md:436–627` e `2f5e38c6:CLAUDE.md:1–631`.

   Os ponteiros `LAPIDES.md#lap-N` preservam **69 ocorrências e 68 destinos distintos**, com mesmas multiplicidades nas três versões. Do pai à volta 8, também preservam a sequência; frente a `a7b66d14`, houve reordenação. A duplicata é `lap-63`. `a7b66d14:CLAUDE.md:1–672`; `034b0e38:CLAUDE.md:1–631`; `2f5e38c6:CLAUDE.md:1–631`, especialmente `:231`, `:235`.

4. **Outros defeitos**

   Não identifiquei outro defeito material no delta: ele se limita às três correções e aos quatro cortes de cabeçalho examinados, preservando o restante do texto do pai. `034b0e38:CLAUDE.md:440`, `:455`, `:461–462`, `:479`, `:543`, `:566`, `:573`; mesmas posições em `2f5e38c6:CLAUDE.md`.

   A execução do selo completo e de seus quatro MORDE fica **[nao verificado]**; o script cria e apaga arquivos temporários, contrariando a restrição desta auditoria. `bin/tests/test_claude_md_regras_preservadas.sh:89–114`.

## 31/10 14:04 · sessao 21b77f1d4341 · - · - · origem=Ronald (tmux)

Achados por inspeção estática; MORDE integral não executado porque cria diretórios e arquivos (`bin/tests/test_lei_akita.sh:180`, `bin/tests/test_lei_akita.sh:181`, `bin/tests/test_lei_akita.sh:184`).

1. **O teste “sem corte” não isola a falta do corte.** `plantar(..., marcador=False)` não cria `origin/main`; essa referência só é criada no outro cenário. O teste aceita qualquer `SystemExit` começando por `PADRAO_ERRO`. Mutante sobrevivente: substituir a rejeição da ausência do corte por `if not corte: corte=['HEAD']`; o teste continuaria passando por erro de `origin/main` na enumeração. Em repositório com `origin/main`, permitiria escolher arbitrariamente `HEAD` como corte. (`bin/tests/test_lei_akita.sh:128`, `bin/tests/test_lei_akita.sh:135`, `bin/tests/test_lei_akita.sh:175`, `bin/tests/test_lei_akita.sh:191`, `bin/tests/test_lei_akita.sh:218`, `bin/tests/test_lei_akita.sh:223`.)

2. **O piso não é testado.** Mutante sobrevivente: remover `*pisos` da chamada de `rev-list`. O único cenário saudável cria o corte na raiz, portanto `pisos=[]`; além disso, a raiz já é ancestral do publicado. Não distingue preservar a não retroatividade de julgá-la incorretamente. (`bin/tests/test_lei_akita.sh:132`, `bin/tests/test_lei_akita.sh:135`, `bin/tests/test_lei_akita.sh:179`, `bin/tests/test_lei_akita.sh:182`, `bin/tests/test_lei_akita.sh:191`.)

3. **O primeiro pai do merge não é comprovado pelo MORDE.** Mutante sobrevivente: usar o segundo pai somente nos merges. Ambos os lados plantados alteram arquivos de negócio; comparar com qualquer pai deixa o merge `n/a (instrumento)` vermelho. Para distinguir os pais, precisa de cenário assimétrico, por exemplo primeiro lado com negócio e raia apenas com documentação. (`bin/tests/test_lei_akita.sh:143`, `bin/tests/test_lei_akita.sh:144`, `bin/tests/test_lei_akita.sh:187`, `bin/tests/test_lei_akita.sh:194`, `bin/tests/test_lei_akita.sh:197`.)

4. **A lista do commit raiz não é testada.** Mutante sobrevivente: substituir o ramo `else git('show',...)` por `else ''`. Nenhuma raiz é julgada no cenário de quatro novos: o corte raiz está atrás de `origin/main`. Com raiz de negócio efetivamente no universo, esse mutante aceitaria `n/a`. (`bin/tests/test_lei_akita.sh:144`, `bin/tests/test_lei_akita.sh:182`, `bin/tests/test_lei_akita.sh:191`, `bin/tests/test_lei_akita.sh:198`.)

5. **Não há cenário saudável de universo vazio.** Mutante sobrevivente: inserir, após a enumeração, `if not hs: raise SystemExit('PADRAO_ERRO vazio')`. Ele bloquearia `HEAD==origin/main`, mas o cenário positivo exige exatamente quatro novos ruins; os outros cenários esperam erros. (`bin/tests/test_lei_akita.sh:135`, `bin/tests/test_lei_akita.sh:198`, `bin/tests/test_lei_akita.sh:200`, `bin/tests/test_lei_akita.sh:205`, `bin/tests/test_lei_akita.sh:212`, `bin/tests/test_lei_akita.sh:220`.)

6. **Fragilidade adicional da lista de caminhos:** a enumeração usa texto sem `-z` e depois `splitlines()`, enquanto negócio depende de `startswith('app/')`. Um caminho que o Git imprima entre aspas pode escapar dessa classificação; renomeação para fora de `app/` também merece caso próprio, pois não há `--no-renames` ou leitura explícita de origem/destino. (`bin/tests/test_lei_akita.sh:92`, `bin/tests/test_lei_akita.sh:144`, `bin/tests/test_lei_akita.sh:145`.) Reprodução com Git real desses dois casos: **[nao verificado]**.

## 31/10 14:02 · sessao 8191a99f63f2 · - · - · origem=Ronald (tmux)

- **A chamada nova fecha o atalho do arsenal.** `bin/pre-push.sh:22` executa o selo antes de `SKIP_ARSENAL=1` (`bin/pre-push.sh:65–69`), enquanto a régua percorre os selos de host apenas depois do retorno antecipado “JA VERDE” (`bin/regua.sh:84–94`, `138–145`). `SKIP_TESTS=1` continua pulando tudo (`bin/pre-push.sh:5`).

- **Pode bloquear push legítimo e julgar outro commit.** A chamada acontece antes de calcular `PUSH_SHA` e reconhecer push só de exclusão (`bin/pre-push.sh:22`, `36–38`). O selo enumera `HEAD` de `R`, independentemente do SHA/ref enviado (`bin/tests/test_lei_akita.sh:17`, `135`). Assim, exclusão de branch ou push de outra ref válida pode ser barrado por commit ruim apenas no HEAD; inversamente, HEAD igual a origin/main deixa vazio o universo desse selo mesmo que a ref empurrada contenha commit ruim (`bin/pre-push.sh:22`, `36`; `bin/tests/test_lei_akita.sh:135–140`). O próprio seletor existente distingue SHA enviado de HEAD e exclusões (`app/core/arvore_do_push.py:8–10`, `19–43`).

- **Raiz fixa agrava clones/worktrees.** O wrapper instalado executa o pre-push da raiz do repositório corrente (`bin/hooks.sh:49–56`), mas a chamada nova aponta para `~/saas-hasner` e o selo usa `/home/ronald/saas-hasner` por padrão (`bin/pre-push.sh:22`; `bin/tests/test_lei_akita.sh:17`). Sem configuração explícita, pode julgar a árvore principal em vez do clone/worktree cujo push disparou o hook (mesmas referências).

- **Chamadores identificados:** pre-push direto (`bin/pre-push.sh:22`); pasta de host pela régua, executando cada `test_*.sh` com Bash e abortando no primeiro vermelho (`bin/regua.sh:138–145`); documentação do molde confirma esse vínculo (`bin/molde_fatia/LEIAME:48–49`). O instalador canônico usa `bin/pre-push.sh` como fonte (`bin/hooks.sh:9–11`); o instalador legado dentro de app também aponta para essa fonte (`app/core/hooks/hooks.sh:9–11`). Hook realmente instalado: [nao verificado].

- **Cache da régua permanece inadequado para selo de histórico:** retorno antecipado compara somente a impressão digital de arquivos Python/HTML (`bin/regua.sh:25–35`, `84–94`), sem comparar mensagens/HEAD/origin/main; portanto uma mudança só no histórico não força o julgamento dos commits pela pasta de host (mesmas referências). A nova chamada cobre esse problema no pre-push comum, antes do atalho do arsenal (`bin/pre-push.sh:22`, `65–69`).

- O pai está disponível em `../4e519aea9b9be16f1ad219c6470d0415bf928e2f`; comparação das versões de `bin/pre-push.sh` mostra apenas as três linhas atuais `20–22`. A identidade Git desse snapshot não foi validada: [nao verificado]. `logs/claude_md_racional/akita_v2.r` não está na árvore disponível: [nao verificado].

## 31/10 14:02 · sessao f63197d45ae2 · - · - · origem=Ronald (tmux)

As três alterações recuperam o sentido das cláusulas selecionadas. A literalidade é do núcleo operativo, com remoção de Markdown/quebras; não houve restauração integral dos parágrafos antigos. Fontes abaixo.

1. **Moratória L-117**
   
   Original, `a7b66d14:CLAUDE.md:644–647`:
   > **(L-117)** Ate `contratos_estruturais` = **20/20** (juiz `core/contratos_estruturais.py::total()`, hoje 15/20), o Code **nao numera nem propoe lei nova por iniciativa propria**: lei nova so nasce de CORTE LITERAL dele. Caso que parece pedir lei vai ao TOPO do RELATO como PERGUNTA com numeros e a fila SEGUE (PAREI-DE-LEI-NAO-DEVOLVE-TURNO). Ela se extingue sozinha em 20/20.

   Atual, `2f5e38c6:CLAUDE.md:453–457`:
   > 3. PERGUNTA DE LEI NAO DEVOLVE TURNO. Falta lei, predicado novo de juiz ou duas candidatas: linha `lei: <a que falta>` no TOPO do RELATO COM NUMEROS, e a esteira SEGUE o proximo item da ORDEM VIVA que nao depende dela; a resposta entra quando vier. Ate 20/20 o Code nao numera nem propoe lei nova por iniciativa propria: lei nova so nasce de CORTE LITERAL dele (L-117). Se uma lei do desenho ESCRITA ja responde [...]

   A volta 8 restaura literalmente, sem o negrito/quebra, “nao numera nem propoe lei nova por iniciativa propria: lei nova so nasce de CORTE LITERAL dele”. Recupera a distinção que o pai perdera ao dizer apenas “nao numera nem propoe lei” (`034b0e38:CLAUDE.md:455`). O encaminhamento da pergunta está no próprio item 3 atual. O referente de 20/20 continua identificável no arquivo: juiz e placar em `2f5e38c6:CLAUDE.md:259–263`, término do estrutural em `:271–272`. **Ressalva de literalidade integral:** a redação atual não repete na moratória o nome do contador, o juiz, o antigo estado 15/20 e a frase explícita de autoextinção; “Ate 20/20” preserva o termo final. Não identifiquei estreitamento/alargamento da proibição restaurada nessas fontes.

2. **AUTONOMIA-DE-CODIGO, item 2**

   Original, `a7b66d14:CLAUDE.md:479–482`:
   > **O Code DECIDE sozinho, registra em UMA linha no RELATO e segue, sem aval**, em quatro coisas: [...] 2. **qual raia ou agente pega qual obra**, e quando uma obra registrada entra como COMPLEMENTO de outra;

   Atual, `2f5e38c6:CLAUDE.md:460–462`:
   > 5. DECISAO TECNICA E DO CODE, em UMA linha no RELATO, sem aval: [...] qual raia ou agente pega qual obra e quando uma obra registrada entra como COMPLEMENTO de outra; [...]

   Recupera exatamente sujeito e relação: **obra registrada** → **COMPLEMENTO de outra**. O pai dizia “quando achado entra como COMPLEMENTO” (`034b0e38:CLAUDE.md:461–462`), confundindo essa delegação com o destino de achados, que já tem enunciado próprio e permanece na sequência (`2f5e38c6:CLAUDE.md:462`). Sem mudança de alcance identificada.

3. **CODEX-PRE-VERIFICADOR**

   Original, `a7b66d14:CLAUDE.md:507`:
   > CODEX-PRE-VERIFICADOR e SUITE-ANTES-DO-OPUS: antes de declarar pronto, Codex `CODEX_PAPEL=verificador` (diff + leis dos sitios + classes que ja reprovaram); procedente corrige e repete (>3 voltas sobe); so entao suite INTEIRA verde, depois UM opus final; medida por obra no RELATO. (PISTA.md#codex-pre-verificador) [nao verificado]

   Atual, `2f5e38c6:CLAUDE.md:543–549`:
   > 5. PRE-VERIFICADOR. (a) TODO construtor, ANTES de declarar pronto, `CODEX_PAPEL=verificador` com: diff do hash da copia; LEIS dos sitios tocados [...] (c) Suite INTEIRA da arvore da obra VERDE, depois UM opus final [...] [nao verificado]

   A cláusula original não restringe a “cura de negocio”; essa restrição aparece no pai (`034b0e38:CLAUDE.md:543`) e é removida na volta 8. “TODO construtor” explicita o alcance geral já contido no comando antigo “antes de declarar pronto”. **Não é reprodução literal do enunciado antigo**, pois “TODO construtor” não constava da linha 507; é restauração semântica do alcance, não ampliação demonstrável por esses textos. O ponteiro `PISTA.md#codex-pre-verificador` permanece em `2f5e38c6:CLAUDE.md:518`.

Não encontrei defeito material adicional nas três substituições acima; eventuais diferenças nas outras partes das consolidações não foram objeto desta subtarefa. [não verificado]

## 31/10 14:02 · sessao e868ae83e323 · - · - · origem=Ronald (tmux)

As remoções desta volta não retiram regra, condição ou ponteiro específico:

- **PARADA E AUTONOMIA:** sai somente `; historia em LAPIDES` (`034b0e38:CLAUDE.md:440`). Continuam os caminhos dos instrumentos (`2f5e38c6:CLAUDE.md:440`), o selo e as seis âncoras `lap-45/46/17/16/11/49` (`2f5e38c6:CLAUDE.md:442-443`).
- **RAIAS E POUSO:** sai somente `; historia em LAPIDES` (`034b0e38:CLAUDE.md:479`). Continuam os nomes das regras (`2f5e38c6:CLAUDE.md:477-479`), os ponteiros PISTA (`2f5e38c6:CLAUDE.md:481`) e `lap-52/15/53` (`2f5e38c6:CLAUDE.md:482`).
- **DINHEIRO:** sai somente `; historia em LAPIDES` (`034b0e38:CLAUDE.md:573`). Continuam as referências L-092/L-110 (`2f5e38c6:CLAUDE.md:573`) e `lap-55/59/08/09/10/48/12` (`2f5e38c6:CLAUDE.md:574`).
- **PLACAR:** sai somente `, resposta a uma pergunta de LEI que eu levantei` (`034b0e38:CLAUDE.md:566`). Essa informação histórica não reaparece literalmente, mas não enuncia condição ou regra. Autor e data permanecem (`2f5e38c6:CLAUDE.md:566`); exercício, dois verdes, preservação da L-095 e `lap-18` permanecem (`2f5e38c6:CLAUDE.md:567-570`).

A obrigação geral de mover história sem apagar, mantendo ponteiro e regra, também permanece (`2f5e38c6:CLAUDE.md:614-615`).

Extração por `LAPIDES\.md#lap-\d+`: **69 ocorrências, 68 destinos únicos**, com sequência integral idêntica em `a7b66d14`, `034b0e38` e `2f5e38c6`; a duplicata é `lap-63` (`2f5e38c6:CLAUDE.md:231` e `2f5e38c6:CLAUDE.md:235`). Nenhum defeito adicional identificado nas remoções examinadas.

## 31/10 14:02 · sessao cebde2888d4a · - · - · origem=Ronald (tmux)

A função real, extraída e executada apenas em memória, produz **55 termos normalizados**, além das duas exceções `n/a`; remove parênteses, asteriscos e crases, converte para minúsculas e normaliza espaços. (`bin/tests/test_lei_akita.sh:61–90`)

Lista exata, agrupada pela origem:

| Origem | Termos normalizados aceitos |
|---|---|
| `app/docs/GLOSSARIO.md:11` | single source of truth; aggregate root; celula soberana; fonte unica |
| `app/docs/GLOSSARIO.md:12` | single writer principle; juiz unico; escritor unico |
| `app/docs/GLOSSARIO.md:13` | sinal derivado do estado; reconciliacao level-triggered; lampada |
| `app/docs/GLOSSARIO.md:14` | architectural invariants + fitness functions; contratos estruturais |
| `app/docs/GLOSSARIO.md:15` | architecture conformance test; selo |
| `app/docs/GLOSSARIO.md:16` | event-driven architecture; crons virando evento |
| `app/docs/GLOSSARIO.md:17` | transactional outbox; o-outbox |
| `app/docs/GLOSSARIO.md:18` | strangler fig pattern; construir ao lado e trocar inteira |
| `app/docs/GLOSSARIO.md:19` | shadow mode; parallel run; sombra |
| `app/docs/GLOSSARIO.md:20` | merge queue; comboio; pouso |
| `app/docs/GLOSSARIO.md:21` | backfill; reprocessing; rejulgar o passivo |
| `app/docs/GLOSSARIO.md:22` | cqrs com projecoes reconstruiveis; espelho; diapago; fechamento; retrato e pilulas |
| `app/docs/GLOSSARIO.md:23` | duplicated business logic; logic drift; derivacao paralela |
| `app/docs/GLOSSARIO.md:24` | bitemporal data; vinculo do dia x vinculo de hoje |
| `app/docs/GLOSSARIO.md:25` | immutable event log + compensating event; batida so retratada |
| `app/docs/GLOSSARIO.md:26` | adr; leis; cortes |
| `app/docs/GLOSSARIO.md:27` | idempotencia pela chave do fato; dedup do fato |
| `app/docs/PLANO-PADROES.md:46–48` | um juiz por pergunta; single writer; evento em vez de varredura; log imutavel; outbox; bitemporal; projecao reconstruivel; cqrs; fitness function |

`n/a (medida)` e `n/a (instrumento)` são aceitos exatamente nessa grafia quando nenhum caminho classificado como negócio aparece; a classificação é textual: começa com `app/`, não contém `/tests/` e não começa com `app/docs/`. (`bin/tests/test_lei_akita.sh:90–103`)

**Divergência documental:** a implementação acompanha a enumeração restrita do NORTE e as duas primeiras colunas do glossário; porém `CLAUDE.md:68` admite termo existente em qualquer dos dois arquivos, e `PLANO-PADROES.md:53` diz “termo do GLOSSARIO ou deste arquivo”. Pela redação ampla, há falsos vermelhos comprovados: `projection rebuild` e `read model replay` aparecem como padrões conhecidos (`app/docs/PLANO-PADROES.md:33`), `Architecture Decision Record` aparece como padrão conhecido (`app/docs/PLANO-PADROES.md:41`) e no próprio glossário (`app/docs/GLOSSARIO.md:26`), mas todos são recusados. A causa é limitar o plano ao trecho NORTE e remover todo conteúdo entre parênteses antes da extração. (`bin/tests/test_lei_akita.sh:77–87`, `bin/tests/test_lei_akita.sh:104–105`)

**Falsos verdes comprovados na função real:**

- `padrao=Padrao Inventado Do Nada, padrao=juiz unico` retorna aprovação: escolhe somente o último campo, mesmo com duas declarações explícitas de padrão. O MORDE verifica duas linhas, mas não duas declarações na mesma linha. (`bin/tests/test_lei_akita.sh:94–98`, `bin/tests/test_lei_akita.sh:162`)
- `padrao=juiz unico (Padrao Inventado Do Nada)` retorna aprovação porque todo texto entre parênteses desaparece; o campo completo inexiste na lista documental. (`bin/tests/test_lei_akita.sh:61–63`, `bin/tests/test_lei_akita.sh:104`)
- `padrao=sinal derivado do estado, reconciliacao level-triggered`, copiando a célula técnica do glossário sem seu comentário parentético, retorna “linha LEI-AKITA sem padrao=”: a regex não consegue consumir vírgula de valor. As duas partes isoladas são aceitas. (`app/docs/GLOSSARIO.md:13`, `bin/tests/test_lei_akita.sh:85`, `bin/tests/test_lei_akita.sh:95–97`)

Não há aceite de substrings como `a` ou `Writer Principle`: a comparação é por pertinência do termo normalizado ao conjunto; os dois exemplos têm MORDE negativo explícito. (`bin/tests/test_lei_akita.sh:104`, `bin/tests/test_lei_akita.sh:160–161`)

## 31/10 13:55 · sessao 8b0c4c13d6aa · LEI-AKITA-UNIVERSO-DO-PUSH · verificador · origem=Code

**Aval: REPROVADO.** A volta 2 fecha o erro comum de Git virar universo vazio, mas o portão ainda pode deixar passar commits novos sem julgamento: a régua reutiliza verde anterior, o pre-push não chama Akita e os merges perdem parte do universo e dos arquivos. (`bin/tests/test_lei_akita.sh:93–109`; `bin/regua.sh:84–94`; `bin/pre-push.sh:13–19`)

1. **Universo efetivamente julgado**

O código seleciona a cadeia de primeiros pais de `HEAD`, exclui o alcançável por `origin/main` e pelo pai do corte. Portanto, aplica restrições adicionais ao contrato literal `origin/main..HEAD`. (`bin/tests/test_lei_akita.sh:97–105`)

| Situação | Comportamento |
|---|---|
| `HEAD == origin/main` | Seleção vazia legítima; imprime zero julgados e termina a etapa PADRAO normalmente. As outras verificações ainda podem reprovar. (`bin/tests/test_lei_akita.sh:105–106,159–167`) |
| Commits locais à frente, história linear posterior ao corte | Percorre todos os selecionados, lê mensagem e arquivos, chama `julgar`. Commit sem linha `LEI-AKITA:` é contado, mas aceito sem validar padrão. (`bin/tests/test_lei_akita.sh:74–77,105–112`) |
| Merge de raia, dois pais, à frente | Julga o merge e sua cadeia de primeiros pais; omite os commits exclusivos do segundo pai, inclusive novos posteriores à lei. (`bin/tests/test_lei_akita.sh:100–105`) |
| `origin/main` ausente ou quebrada | Encontrado o corte, erro de `rev-list` gera `PADRAO_ERRO`; falta o marcador final e o shell reprova. **Exceção:** sem corte, retorna antes de consultar `origin/main` e aceita `PADRAO_SEM_CORTE`. (`bin/tests/test_lei_akita.sh:93–105,157–167`) |
| Repositório raso | Não verifica completude. O histórico truncado pode mudar o corte aparente e reduzir a enumeração sem erro de Git; também pode fazer a leitura de arquivos tratar a fronteira como raiz. (`bin/tests/test_lei_akita.sh:97–109`) |
| `HEAD` destacado numa worktree | Com `LEI_AKITA_R` apontando à worktree, funciona sem nome de branch. Sem essa variável, julga `/home/ronald/saas-hasner`, independentemente da cópia executada. (`bin/tests/test_lei_akita.sh:17,93,105,164`) |

A exclusão dos publicados acontece por referência, sem dispensa nominal para os dois hashes; eles ficam documentados na lápide. (`bin/tests/test_lei_akita.sh:105`; `app/docs/LAPIDES.md:781–786`)

2. **Há caminhos de VERDE sem julgamento devido** [nao verificado]

- **Merge:** `--first-parent` omite um commit novo ruim da raia se o merge tiver mensagem aceitável. Além disso, `git show --name-only` sem comparação explícita contra o primeiro pai pode retornar zero arquivos num merge sem conflito; nesse caso, `n/a (instrumento)` passa mesmo quando o pouso introduz código de negócio. Conferi essa diferença com objetos existentes: a chamada usada pelo selo retornou vazio, enquanto a comparação com o primeiro pai listou arquivos de negócio. (`bin/tests/test_lei_akita.sh:83–85,105–110`)

- **Raso:** exercitei a função real com objetos existentes e uma fronteira rasa fornecida por pipe, sem criar arquivos. Com as mesmas revisões, o histórico completo produziu 71 julgados e duas falhas; truncado, produziu um julgado e nenhuma falha. O código não exige histórico completo antes de aceitar esse resultado. (`bin/tests/test_lei_akita.sh:97–113`; padrões ruins documentados em `app/docs/LAPIDES.md:785–786`)

- **Sem corte:** `corte=[]` vira `PADRAO_SEM_CORTE`, explicitamente aceito como sucesso. Não existe distinção entre ausência histórica legítima da regra e impossibilidade de estabelecer o universo exigido pelo portão. (`bin/tests/test_lei_akita.sh:97–99,157–167`)

- **Repositório errado:** executar a cópia da worktree sem `LEI_AKITA_R` pode conferir o principal e deixar o commit da worktree sem julgamento. (`bin/tests/test_lei_akita.sh:17,93,156,164`)

Quanto ao **rc**, a correção funciona no caminho comum: falhas de enumeração, leitura da mensagem ou leitura dos arquivos abortam antes de `PADRAO_FIM`, e o shell acusa a ausência desse marcador. O rc do Python continua descartado, mas esses erros já não viram verde apenas por terem stdout vazio. (`bin/tests/test_lei_akita.sh:93–95,108–109,162–167`)

O `grep -c` sem correspondência retorna zero na saída; seu rc não interrompe o script, que não usa `errexit`. Nesse trecho, isso representa corretamente zero falhas, e não explica os falsos verdes acima. (`bin/tests/test_lei_akita.sh:16,165–167`)

3. **O que os MORDE detectam**

Exercitei os seis casos de `julgar` e as seis mutações abaixo apenas em memória: os originais deram os resultados esperados e cada mutação foi detectada pelo caso indicado. (`bin/tests/test_lei_akita.sh:116–125`)

| Caso | Mutante que faz o caso deixar de satisfazer sua expectativa |
|---|---|
| Padrão inventado | Remover a rejeição de termo ausente de `TEXTO`. (`bin/tests/test_lei_akita.sh:88–89,116`) |
| `n/a (medida)` em negócio | Permitir esse valor mesmo com arquivos de negócio. (`bin/tests/test_lei_akita.sh:83–85,117`) |
| Sem `padrao=` | Retornar `None` quando não encontrar o campo. (`bin/tests/test_lei_akita.sh:79–81,118`) |
| `Single Writer Principle` válido | Rejeitar todo termo, inclusive o existente. (`bin/tests/test_lei_akita.sh:88–90,119`; `app/docs/GLOSSARIO.md:12`) |
| `n/a (instrumento)` em `bin/tests` | Rejeitar instrumento mesmo fora de negócio. (`bin/tests/test_lei_akita.sh:83–85,120`) |
| `padrao=` mencionado no RED | Trocar `ms[-1]` por `ms[0]`, passando a extrair `no texto`. (`bin/tests/test_lei_akita.sh:79–82,121`) |

Os dois casos de universo têm estas mordidas por construção; execução integral deles **[não verificado]**, pois a fixture cria arquivos e repositório temporário. (`bin/tests/test_lei_akita.sh:128–142`)

| Caso | Mutantes visados |
|---|---|
| Publicado ruim e dois novos ruins | Retirar `^origin/main`; retornar universo vazio; julgar somente HEAD. A comparação exige exatamente os dois novos. (`bin/tests/test_lei_akita.sh:138–146`) |
| Ausência de `origin/main` | Voltar a ignorar rc e transformar o erro de enumeração em lista vazia. (`bin/tests/test_lei_akita.sh:147–153`) |

**Sobrevivem aos MORDE atuais:** retirar `--first-parent`, retirar o piso, liberar `n/a (instrumento)` em negócio e liberar `n/a` sem qualificador. A fixture é linear, tem corte na raiz e os negativos de `n/a` cobrem somente `medida`. (`bin/tests/test_lei_akita.sh:116–121,133–145`)

4. **Chamadores e efeito da mudança**

A pasta de selos de host é executada pela régua, que propaga falha por `exit 1`. Quando efetivamente chamado, Akita passa a excluir os publicados, mas conserva os filtros de primeiro pai e piso. (`bin/regua.sh:138–145`; `bin/tests/test_lei_akita.sh:97–110`)

**O pre-push canônico não chama Akita.** Ele declara que o loop de host não roda ali e chama explicitamente outros selos de push; depois executa as suítes Django. O wrapper gerado delega para esse arquivo. Hook efetivamente instalado **[não verificado]**. (`bin/pre-push.sh:13–19,182–195`; `bin/hooks.sh:51–56`)

Há um caminho normal que perde justamente o commit novo: [nao verificado]

- A cadeia exige ausência de commits locais e roda a régua antes do commit. (`bin/molde_fatia/cadeia.sh:13,51`)
- Cria o commit e chama novamente a régua. (`bin/molde_fatia/cadeia.sh:68–81`)
- A régua pode retornar “JA VERDE” comparando apenas arquivos Python/HTML e status, antes dos selos; criar o commit não altera esse conteúdo. (`bin/regua.sh:25–35,84–94,138`)
- A cadeia aceita o stamp anterior e faz push; Akita não é chamado pelo pre-push. **Logo, um `padrao=Padrao Inventado Do Nada` novo pode sair sem avaliação Akita.** Reprodução operacional completa **[não verificado]**; o caminho está nos chamadores citados. (`bin/molde_fatia/cadeia.sh:72–85`; `bin/pre-push.sh:13–19`)

5. **Outros defeitos**

- **Campo vazio ou fragmento passa:** confirmei em memória que `padrao= `, `padrao=a` e `padrao=Single Writer` retornam sucesso. O código remove espaços e procura substring no documento inteiro; a string vazia também pertence a qualquer texto. Isso excede os termos e aliases autorizados. (`bin/tests/test_lei_akita.sh:63–69,79–89`; `app/docs/PLANO-PADROES.md:46–50`)

- **Só a última linha é julgada:** uma linha inválida pode ser encoberta por outra válida posterior, apesar do comentário declarar “toda linha”. (`bin/tests/test_lei_akita.sh:48,75–79`)

- **“Julgados” superestima a validação:** conta todos os hashes enumerados, inclusive os que `julgar` ignora por ausência de `LEI-AKITA:`. (`bin/tests/test_lei_akita.sh:74–77,105–106,174`)

- **Stamp antigo sobrevive à falha de host:** esse caminho da régua sai com erro sem invalidar o stamp; a cadeia usa `|| true` e consulta apenas `REGUA_STATUS=OK`. Assim, mesmo uma execução vermelha pode ser seguida da aceitação de um stamp anterior. (`bin/regua.sh:138–143`; `bin/molde_fatia/cadeia.sh:70–82`)

## 31/10 13:52 · sessao 08f446a69283 · - · - · origem=Ronald (tmux)

Leitura do código e execução isolada de `julgar` em memória confirmaram os seis resultados esperados; o MORDE completo não foi executado porque cria arquivos temporários (`bin/tests/test_lei_akita.sh:128-140`).

| Caso MORDE | Mutante que o caso mata |
|---|---|
| Termo inventado (`:116`) | Remover a rejeição de termo ausente do catálogo (`:88-89`). |
| `n/a (medida)` em negócio (`:117`) | Permitir esse `n/a` com arquivos de negócio ou fazer `negocio()` retornar sempre vazio (`:71-72`, `:83-85`). |
| Falta de `padrao=` (`:118`) | Retornar `None` quando a expressão regular não encontra o campo (`:79-81`). |
| `Single Writer Principle` válido (`:119`) | Rejeitar qualquer termo ou inverter a condição do catálogo (`:88-90`); o termo está no catálogo (`app/docs/GLOSSARIO.md:12`). |
| `n/a (instrumento)` em `bin/tests` (`:120`) | Proibir todo `n/a` ou rejeitar instrumento mesmo fora de negócio (`:83-85`). |
| `padrao=` mencionado no RED (`:121`) | Usar `ms[0]` em vez de `ms[-1]` (`:79-82`): confirmado em memória, extrai `no texto` e produz falha. |
| Dois novos ruins e publicado ruim (`:138-146`) | Julgar somente HEAD, retornar universo vazio ou deixar de excluir `origin/main` (`:105-112`). | [nao verificado]
| Sem `origin/main` (`:147-153`) | Ignorar `returncode` e transformar erro do `rev-list` em stdout vazio (`:93-96`). |

Todas as referências `:linha` acima pertencem a `bin/tests/test_lei_akita.sh`.

Mutantes sobreviventes: retirar `--first-parent`; retirar o piso; permitir `n/a (instrumento)` em negócio; aceitar `n/a` sem qualificador. A fixture tem corte na raiz, história linear e nenhum negativo de instrumento em negócio ou `n/a` genérico (`bin/tests/test_lei_akita.sh:116-121`, `bin/tests/test_lei_akita.sh:133-145`).

Falso verde comprovado: `padrao=a`, `padrao=x` e `padrao=padrao` retornam `None`. O juiz busca substring em todo o texto dos documentos, enquanto o contrato documental delimita termos técnicos, aliases e termos expressamente listados; nenhum MORDE cobre essa diferença (`bin/tests/test_lei_akita.sh:63-69`, `bin/tests/test_lei_akita.sh:88-90`, `app/docs/PLANO-PADROES.md:46-50`).

A correção do rc do Git fecha o erro comum: subprocesso malsucedido aborta antes do marcador final, e o shell acusa ausência de resposta (`bin/tests/test_lei_akita.sh:93-95`, `bin/tests/test_lei_akita.sh:162-167`). Porém, corte ausente retorna antes de validar `origin/main`; `PADRAO_SEM_CORTE` é explicitamente aceito como sucesso e não tem fixture MORDE (`bin/tests/test_lei_akita.sh:97-99`, `bin/tests/test_lei_akita.sh:157-158`, `bin/tests/test_lei_akita.sh:167`).

## 31/10 13:51 · sessao 893cbcb3fe18 · - · - · origem=Ronald (tmux)

1. **A régua chama; pre-push não chama.** `bin/regua.sh:138-145` executa `bash` para cada `tests/test_*.sh` e aborta com `exit 1` se o selo falhar. `bin/pre-push.sh:13-19` declara que esse loop não roda no pre-push e chama explicitamente apenas os selos `lei_protege_sitio` e `achado_por`; o restante do pre-push testa suites de Django (`bin/pre-push.sh:155-195`). A versão antiga também não chama Akita (`app/core/hooks/pre-push.sh:6-89`). O instalador canônico usa `bin/pre-push.sh` (`bin/hooks.sh:9-11`) e produz wrapper que delega a ele (`bin/hooks.sh:51-56`). Hook efetivamente instalado: **[não verificado]**.

2. **Buraco crítico: o selo pode perder justamente o commit novo no fluxo normal.** A cadeia exige ausência de commits locais (`bin/molde_fatia/cadeia.sh:13`), copia a cura (`bin/molde_fatia/cadeia.sh:36-40`) e roda a régua antes de criar o commit (`bin/molde_fatia/cadeia.sh:51`); nesse ponto o Akita julga o HEAD antigo. Depois cria o commit com `msg_commit.txt` (`bin/molde_fatia/cadeia.sh:68`) e chama novamente a régua (`bin/molde_fatia/cadeia.sh:70-82`). Porém a régua retorna “JA VERDE” pelo fingerprint de arquivos Python/HTML e pelo status, antes dos selos de host (`bin/regua.sh:25-35`, `bin/regua.sh:84-94`, `bin/regua.sh:138-145`). Criar o commit não altera esses arquivos: o novo `padrao=Padrao Inventado Do Nada` pode nunca ser julgado. A cadeia aceita o stamp antigo e faz push (`bin/molde_fatia/cadeia.sh:72`, `bin/molde_fatia/cadeia.sh:82-85`); o pre-push não chama Akita (`bin/pre-push.sh:13-19`). Mesmo executar as suites novamente não cobre o juiz Python embutido nesse selo de host (`bin/pre-push.sh:182-195`, `bin/tests/test_lei_akita.sh:58-163`). Reprodução operacional: **[não verificado]**; caminho deduzido diretamente dos chamadores citados.

3. **O cache não identifica o universo de commits.** A régua calcula hash de árvore (`bin/regua.sh:45`), mas seu atalho só compara `FP` e `REGUA_STATUS` (`bin/regua.sh:84-94`). O pre-push compara árvore/fingerprint/status (`bin/pre-push.sh:61-66`), sem comparar mensagens ou `origin/main..HEAD`. Portanto amend apenas da mensagem, commit vazio ou merge de mesma árvore preservam a identidade usada pelo cache e podem conservar o verde anterior sem nova avaliação Akita (`bin/regua.sh:84-94`, `bin/pre-push.sh:61-66`). Isso é particularmente incompatível com um selo cujo universo é obtido de `HEAD` e `origin/main` (`bin/tests/test_lei_akita.sh:105-110`).

4. **Worktree não é escolhido automaticamente pelo local de execução.** O selo fixa `R=${LEI_AKITA_R:-/home/ronald/saas-hasner}` (`bin/tests/test_lei_akita.sh:17`), entrega esse `R` ao Python (`bin/tests/test_lei_akita.sh:164`, `bin/tests/test_lei_akita.sh:169`) e executa Git com `-C repo` (`bin/tests/test_lei_akita.sh:93`, `bin/tests/test_lei_akita.sh:156`). Sem override, executar a cópia numa worktree julga o repositório principal. A régua não fornece `LEI_AKITA_R` ao chamar o selo (`bin/regua.sh:138-140`) e também fixa APP/STAMP no principal (`bin/regua.sh:14-15`). Com `LEI_AKITA_R` explicitamente apontado à worktree, HEAD destacado é compatível com o código: o juiz usa `HEAD`, sem exigir nome de branch (`bin/tests/test_lei_akita.sh:105`).

5. **Efeito do universo novo:** nas execuções efetivas da pasta de selos, a régua passa a receber julgamento apenas dos commits selecionados em `HEAD ^origin/main`, adicionalmente restringidos pelo primeiro pai e pelo piso (`bin/tests/test_lei_akita.sh:97-110`); falhas do selo continuam abortando a régua antes da suite (`bin/regua.sh:138-145`). Isso não transforma o selo em guarda de todo push, porque o pre-push continua sem chamá-lo e a régua pode sair antes do loop (`bin/pre-push.sh:13-19`, `bin/regua.sh:84-94`).

## 31/10 13:44 · sessao f9f744f07fd0 · REPROVA-E-DESENHO · verificador · origem=Code

**REPROVADO.** Há caminho que libera uma linha anterior ao disparo e falhas na validação das linhas de liberação (`bin/hook_stop_fila1.py:437`, `bin/disjuntor.py:635`, `bin/disjuntor.py:731`, `bin/disjuntor.py:609`, `bin/disjuntor.py:616`).

1. **A decisão sobre o estado está numa única seção sob trava.** Acumular, fotografar, disparar e liberar acontecem dentro de `_fn`, executada por uma única `_com_estado`; o `flock` envolve o callback e a gravação (`bin/disjuntor.py:244`, `bin/disjuntor.py:259`, `bin/disjuntor.py:693`, `bin/disjuntor.py:744`). Os dois hooks chamam `decide_b3`, sem reconhecedores concorrentes de liberação (`bin/disjuntor.py:1136`, `bin/hook_stop_fila1.py:1044`). A varredura dos transcripts acontece antes da trava, mas a acumulação acontece dentro dela (`bin/disjuntor.py:690`, `bin/disjuntor.py:702`). Não encontrei divergência de regra entre os chamadores; permanecem as falhas de fotografia abaixo (`bin/disjuntor.py:729`, `bin/disjuntor.py:1136`, `bin/hook_stop_fila1.py:1044`).

2. **Sim, uma linha anterior ao disparo pode liberar.**

   **Leitura parcialmente ilegível:** suponha primeira linha neutra e segunda linha com `ERRO-BOBO` já existente. Se a leitura falhar antes da segunda linha, `_bloco_do_topo` devolve a primeira linha; `_topo_consistente` aceita esse resultado como fotografia completa. Restaurar a leitura e executar `touch` permite que a segunda linha antiga libere, pois seu SHA ficou fora da fotografia (`bin/hook_stop_fila1.py:438`, `bin/disjuntor.py:635`, `bin/disjuntor.py:695`, `bin/disjuntor.py:731`, `bin/disjuntor.py:733`). Esse caminho foi reproduzido em memória; o teste existente exige preservar a leitura parcial, mas não testa sua consequência na fotografia do disparo (`bin/tests/test_disjuntor.sh:746`, `bin/tests/test_disjuntor.sh:747`).

   **Janela fotografia → marcação:** um escritor pode colocar a linha depois da fotografia e antes de `disparado=True`. Ela já estará presente quando essa marcação ocorrer, ficará fora da fotografia e poderá satisfazer o mtime, comparado ao relógio capturado antes de toda a operação (`bin/disjuntor.py:689`, `bin/disjuntor.py:694`, `bin/disjuntor.py:712`, `bin/disjuntor.py:733`).

   Com fotografia completa, `touch`, reescrita preservando a linha e mtime igual não vencem o bloqueio por SHA. Ilegibilidade **total** também recebe fotografia tardia, sem liberar naquela observação (`bin/disjuntor.py:724`, `bin/disjuntor.py:726`, `bin/disjuntor.py:731`). Mtime igual é aceito para uma candidata fora da fotografia; isso está explicitamente testado (`bin/disjuntor.py:733`, `bin/tests/test_disjuntor.sh:875`).

3. **Não encontrei evento formal `DISJUNTOR`/`LIBERA` B3 anunciado sem gravação concluída.** O sucesso só é marcado depois de `os.replace`; a publicação depende dele, e uma liberação não gravada volta ao conjunto bloqueado (`bin/disjuntor.py:265`, `bin/disjuntor.py:274`, `bin/disjuntor.py:745`, `bin/disjuntor.py:751`).

   Há uma distinção: `NEGOU DISJUNTOR B3...` pode aparecer sem persistência, porque a negativa daquela chamada continua valendo. Isso não é contado como evento formal de disparo (`bin/disjuntor.py:749`, `bin/disjuntor.py:1136`, `bin/disjuntor.py:1168`, `bin/disjuntor.py:1226`).

4. **A validação ainda aceita entradas indevidas.**

| Entrada | Resultado e causa |
|---|---|
| `ERRO-BOBO OBRA-X: ___` | Aceita motivo só com pontuação: `\w` inclui underscore (`bin/disjuntor.py:616`). |
| Link para arquivo **dentro** da raiz | Aceito: `realpath` resolve o link antes de `_abre`; o `O_NOFOLLOW` recebe o destino resolvido (`bin/disjuntor.py:576`, `bin/disjuntor.py:581`, `bin/orcamento_contexto.py:74`). |
| `bin/disjuntor.py:1.5` ou `:1xyz` | Aceito validando apenas o prefixo `:1`; falta delimitação do número (`bin/disjuntor.py:77`, `bin/disjuntor.py:609`). |
| Referência válida seguida de `/etc/passwd:1` | Aceita a linha inteira: a referência externa, sem extensão, é ignorada pela regex (`bin/disjuntor.py:77`, `bin/disjuntor.py:609`, `bin/disjuntor.py:610`). |
| Referência válida seguida de `bin/disjuntor.py:-9` | A referência negativa também é ignorada, em vez de invalidar a linha (`bin/disjuntor.py:77`, `bin/disjuntor.py:610`). |

Para referências reconhecidas integralmente, caminho absoluto, resolução para fora da raiz, linha zero e linha além do fim são recusados; a última linha sem quebra é contada corretamente (`bin/disjuntor.py:573`, `bin/disjuntor.py:577`, `bin/disjuntor.py:586`, `bin/disjuntor.py:588`).

5. **Mutantes: nenhuma âncora ausente ou repetida entre os 129 declarados; nenhum sobrevivente identificado.** A conferência individual encontrou uma ocorrência de cada âncora, conforme exige o próprio mecanismo (`bin/tests/test_disjuntor.sh:937`; declarações a partir de `bin/tests/test_disjuntor.sh:945`).

   Os 27 mutantes associados a `b3_topo` e os cinco de `b3_atomico` caíram na execução dos respectivos casos em memória, com controles passando; os casos exercitam reconhecimento, validação, trava, mtime e releitura após mudança (`bin/tests/test_disjuntor.sh:682`, `bin/tests/test_disjuntor.sh:728`, `bin/tests/test_disjuntor.sh:853`, `bin/tests/test_disjuntor.sh:875`, `bin/tests/test_disjuntor.sh:884`).

   Para os outros 97, há caso correspondente que os derruba por inspeção estática; **queda integrada desses 97: [nao verificado]**. A suíte shell não foi executada porque cria, modifica e apaga arquivos (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:51`, `bin/tests/test_disjuntor.sh:930`, `bin/tests/test_disjuntor.sh:939`).

6. **Nenhuma chamada residual encontrada em `bin/` ou `app/`.** As seis funções removidas são `linha_libera_b3`, `libera_b3`, `_topo_shas`, `varrer_b3`, `_topo_e_mtime` e `b3_disparados` — definições no pai em [disjuntor.py:586](/home/ronald/codex_ws/5f3a10ab4813c993574f25bbad16da662abe60c1/bin/disjuntor.py:586), [592](/home/ronald/codex_ws/5f3a10ab4813c993574f25bbad16da662abe60c1/bin/disjuntor.py:592), [638](/home/ronald/codex_ws/5f3a10ab4813c993574f25bbad16da662abe60c1/bin/disjuntor.py:638), [666](/home/ronald/codex_ws/5f3a10ab4813c993574f25bbad16da662abe60c1/bin/disjuntor.py:666), [733](/home/ronald/codex_ws/5f3a10ab4813c993574f25bbad16da662abe60c1/bin/disjuntor.py:733) e [744](/home/ronald/codex_ws/5f3a10ab4813c993574f25bbad16da662abe60c1/bin/disjuntor.py:744). O único resultado textual restante do grep foi a documentação antiga citando `linha_libera_b3`; os chamadores produtivos usam `decide_b3` (`app/docs/RELATO.md:11`, `bin/disjuntor.py:1136`, `bin/hook_stop_fila1.py:1044`).

7. **Outros defeitos/limites:**

   A observação dita consistente não detecta reescrita no mesmo inode, com mesmo tamanho e mtime preservado: a chave compara somente esses três atributos. Portanto, o segundo `stat` não garante que o conteúdo permaneceu igual durante a leitura (`bin/disjuntor.py:627`, `bin/disjuntor.py:635`).

   Os eventos são publicados depois de soltar a trava; duas transições concorrentes podem aparecer em ordem invertida na trilha, embora ambas tenham sido gravadas (`bin/disjuntor.py:277`, `bin/disjuntor.py:744`, `bin/disjuntor.py:747`).

   O contrato no RELATO ficou desatualizado: ainda nomeia a função removida e exige motivo de 20 caracteres, enquanto o novo reconhecedor aceita um caractere de palavra (`app/docs/RELATO.md:11`, `app/docs/RELATO.md:12`, `bin/disjuntor.py:616`).

## 31/10 13:43 · sessao 01543f2c5b50 · - · - · origem=Ronald (tmux)

Auditoria de mutantes em só leitura concluída. **Nenhum dos 129 mutantes tem âncora ausente ou repetida; todos os 129 códigos mutados compilam.** Extraí as chamadas via Bash substituindo `_mutante` apenas por `printf`, contei as ocorrências e usei `compile()` em memória; o critério exigido pelo próprio script é uma ocorrência exata (`bin/tests/test_disjuntor.sh:934-939`). A lista dos 129 está em `bin/tests/test_disjuntor.sh:945-1128`.

**Nenhum sobrevivente identificado.** Executei em memória os 27 mutantes de `b3_topo` e os cinco de `b3_atomico`, com controle passando. Os demais 97 têm caso que os derruba por inspeção estática; a execução integrada deles está **[nao verificado]**. Não executei a suíte shell porque ela cria, altera e apaga arquivos (`bin/tests/test_disjuntor.sh:39-56`, `bin/tests/test_disjuntor.sh:930-939`).

Mapa das verificações em memória:

| Mutante | Assert que caiu |
|---|---|
| B3-sem-lei | 17 — `bin/tests/test_disjuntor.sh:698` |
| B3-sem-lei-juiz | 2 e outros — `bin/tests/test_disjuntor.sh:684` |
| B3-sem-lei-escritor-topo | 31 — `bin/tests/test_disjuntor.sh:708` |
| B3-sem-lei-celula | 19/32 — `bin/tests/test_disjuntor.sh:700`, `bin/tests/test_disjuntor.sh:709` |
| B3-sem-lei-lampada | 20/33 — `bin/tests/test_disjuntor.sh:701`, `bin/tests/test_disjuntor.sh:710` |
| B3-sem-arquivo-linha | 18/62 — `bin/tests/test_disjuntor.sh:699`, `bin/tests/test_disjuntor.sh:721` |
| B3-acento-conta | 19 — `bin/tests/test_disjuntor.sh:700` |
| B3-bobo-qualquer / B3-bobo-pontuacao | 21 — `bin/tests/test_disjuntor.sh:704` |
| B3-sitio-nao-confere | 34/35/36/38/39/60/61 — `bin/tests/test_disjuntor.sh:712-720` |
| B3-sitio-so-o-1o | 61 — `bin/tests/test_disjuntor.sh:720` |
| B3-linha-zero | 36 — `bin/tests/test_disjuntor.sh:714` |
| B3-linha-alem-do-fim | 38/39 — `bin/tests/test_disjuntor.sh:716-718` |
| B3-ultima-sem-quebra | 39 — `bin/tests/test_disjuntor.sh:717-718` |
| B3-fora-da-raiz | 60 — `bin/tests/test_disjuntor.sh:719` |
| B3-desenho-no-meio | 24 — `bin/tests/test_disjuntor.sh:722` |
| B3-id-substring | 3 e outros — `bin/tests/test_disjuntor.sh:685` |
| B3-obra-fora-do-cabecalho | 25/27 — `bin/tests/test_disjuntor.sh:723`, `bin/tests/test_disjuntor.sh:725` |
| B3-pergunta-sem-texto | 26 — `bin/tests/test_disjuntor.sh:724` |
| B3-topo-sem-lookbehind | 7 e controles — `bin/tests/test_disjuntor.sh:689`, `bin/tests/test_disjuntor.sh:728-732` |
| B3-topo-sem-sublinhado | 13 — `bin/tests/test_disjuntor.sh:693` |
| B3-topo-sem-re-I | 14 e controles — `bin/tests/test_disjuntor.sh:694`, `bin/tests/test_disjuntor.sh:728-732` |
| B3-topo-sem-validacao | controles 42/43/49/52/59 — `bin/tests/test_disjuntor.sh:728-732` |
| B3-identidade-sem-hifen | 15 — `bin/tests/test_disjuntor.sh:695` |
| B3-identidade-sem-upper | 16 — `bin/tests/test_disjuntor.sh:696` |
| B3-topo-parcial-vira-none | 10/11 — `bin/tests/test_disjuntor.sh:747-748` |
| B3-topo-sem-cauda | 2 e outros — `bin/tests/test_disjuntor.sh:684` |
| B3-sem-mtime | c5 — `bin/tests/test_disjuntor.sh:879-882` |
| B3-mtime-estrito | c4 — `bin/tests/test_disjuntor.sh:875-878` |
| B3-topo-fora-da-trava / B3-duas-travas | c1/c2 — `bin/tests/test_disjuntor.sh:853-867` |
| B3-observacao-sem-restat | c6 — `bin/tests/test_disjuntor.sh:884-901` |

Para consolidar os outros B3 por inspeção:

- `B3-limiar-3`, `B3-sem-lei-escritor`, `B3-foto-ilegivel-vira-vazia`, `B3-sem-foto-tardia`, `B3-foto-ilegivel-calada`, `B3-anuncia-antes-de-gravar`, `B3-gravou-mente`, `B3-libera-sem-gravar`, `B3-migra-sobrescreve`: casos próprios em `b3_limiar` (`bin/tests/test_disjuntor.sh:763-800`).
- `B3-stop-cala`: presença do recado no Stop (`bin/tests/test_disjuntor.sh:344-348`).
- `P1-b3-so-no-stop`: reprovação sem Stop anterior (`bin/tests/test_disjuntor.sh:401-403`).
- `P2-rearme-ignorado`: verificador passa após rearme e Stop (`bin/tests/test_disjuntor.sh:398-400`).
- `B3-liberado-volta`: reprovações já consumidas permanecem liberadas (`bin/tests/test_disjuntor.sh:379-381`).
- `B3-sitio-nao-confere-integrado`: arquivo inexistente mantém bloqueio (`bin/tests/test_disjuntor.sh:364`).
- `B3-so-a-1a-candidata`: primeira candidata consumida não esconde ERRO-BOBO seguinte (`bin/tests/test_disjuntor.sh:387-389`).
- `B3-topo-so-linha-1`: DESENHO na segunda linha útil libera (`bin/tests/test_disjuntor.sh:373-381`).
- `B3-sem-consumo`: reescrever linha consumida mantém bloqueio (`bin/tests/test_disjuntor.sh:382-385`).
- `B3-sem-topo-no-disparo`: linha anterior ao disparo seguida de touch mantém bloqueio (`bin/tests/test_disjuntor.sh:390-393`).
- `B3-sem-migracao`: reprovações antigas arquivadas não disparam (`bin/tests/test_disjuntor.sh:328-335`).
- `B3-topo-branco-encerra`: branco entre as duas linhas úteis (`bin/tests/test_disjuntor.sh:373-379`).
- `B3-topo-bloco-inteiro`: DESENHO na terceira linha útil não libera (`bin/tests/test_disjuntor.sh:367`).
- `B-sem-contador`: final sem obra aparece no contador (`bin/tests/test_disjuntor.sh:427-431`).
- Demais reconhecedores de final/obra/markdown/cauda: exemplos positivos e negativos (`bin/tests/test_disjuntor.sh:409-426`).

Limite material: `b3_atomico` mede a entrada no callback de `_com_estado`, não observa diretamente `fcntl.flock`; portanto comprova uma chamada e leitura dentro do callback, enquanto a aquisição efetiva da trava depende da implementação de `_com_estado` (`bin/tests/test_disjuntor.sh:829-850`, `bin/disjuntor.py:230-281`).

## 31/10 13:42 · sessao 42bd65f14083 · - · - · origem=Ronald (tmux)

Achados das perguntas 1–3:

1. A transição do estado está unificada: `_fn` lê o topo, acumula, dispara e libera sob uma chamada de `_com_estado`; os dois hooks usam `decide_b3`. A varredura dos transcripts ocorre antes da trava, sem modificar o estado. (`bin/disjuntor.py:690`, `bin/disjuntor.py:693`, `bin/disjuntor.py:744`, `bin/disjuntor.py:1136`, `bin/hook_stop_fila1.py:1044`)

2. **Há dois caminhos confirmados em memória que liberam linha anterior ao disparo:**

   - **Leitura parcialmente ilegível:** `_bloco_do_topo` devolve a primeira linha já lida quando ocorre `OSError` antes da segunda; `_topo_consistente` aceita essa leitura como completa. Se a segunda linha já continha `ERRO-BOBO`, a fotografia não a guarda. Restaurar a leitura e executar `touch` permite que ela libere. Reproduzido com iterador que falha antes da segunda linha útil: primeira decisão `{'OBRA-X': 1}`, segunda `{}`, eventos `DISJUNTOR`, `LIBERA`. (`bin/hook_stop_fila1.py:437`, `bin/disjuntor.py:635`, `bin/disjuntor.py:695`, `bin/disjuntor.py:731`, `bin/disjuntor.py:733`)
   - **Janela fotografia → disparo:** depois que `_topo_consistente` retorna, um escritor pode colocar a linha no RELATO antes da atribuição `disparado=True`. Ela não estará na fotografia e seu mtime será posterior ao `agora` capturado na borda. A próxima chamada libera. Reproduzido inserindo a linha no início de `_migra_limiar`, com a obra ainda não disparada. (`bin/disjuntor.py:689`, `bin/disjuntor.py:694`, `bin/disjuntor.py:696`, `bin/disjuntor.py:712`, `bin/disjuntor.py:733`)

   Com leitura completa e sem concorrência, `touch`, reescrita preservando a linha e mtime igual não vencem a fotografia: o SHA antigo é recusado antes da comparação temporal. Ilegibilidade total também recebe fotografia tardia sem liberação naquela chamada. (`bin/disjuntor.py:724`, `bin/disjuntor.py:726`, `bin/disjuntor.py:731`)

3. Não encontrei evento formal `DISJUNTOR`/`LIBERA` B3 anunciado sem gravação concluída: `gravou=True` só sucede `os.replace`; o anúncio depende desse resultado, e liberação não persistida volta ao conjunto bloqueado. (`bin/disjuntor.py:265`, `bin/disjuntor.py:274`, `bin/disjuntor.py:745`, `bin/disjuntor.py:751`)

   Uma negativa `NEGOU DISJUNTOR B3 ...` pode aparecer mesmo sem persistência, por decisão explícita de manter a negativa daquela chamada; o contador distingue `NEGOU` de `DISJUNTOR`. A publicação dos eventos ocorre depois de soltar a trava, permitindo ordem histórica invertida sob concorrência, embora ambos tenham sido gravados em algum instante. (`bin/disjuntor.py:277`, `bin/disjuntor.py:747`, `bin/disjuntor.py:749`, `bin/disjuntor.py:1136`, `bin/disjuntor.py:1168`, `bin/disjuntor.py:1226`)

## 31/10 13:42 · sessao eda389dede8f · CLAUDE-MD-RACIONAL · verificador · origem=Code

**1) Nenhuma das dez linhas removidas ficou sem correspondente no 7b atual**, considerando o bloco completo. (`CLAUDE.md:481`, `CLAUDE.md:483`, `CLAUDE.md:518`, `CLAUDE.md:519`, `CLAUDE.md:567`, `CLAUDE.md:568`, `CLAUDE.md:570`, `CLAUDE.md:575`, `CLAUDE.md:576`)

| Linha removida, identificada pelo conteúdo | Onde ficou |
|---|---|
| Cinco âncoras RAIAS/POUSO | Todas mantidas, apenas concatenadas: `CLAUDE.md:481`. | [nao verificado]
| Contador de raias (5), selo no pre-push, fila 2 depois de CELULA-TURNO-FECHA **e** O145, conduta provisória, HANDOFF HEAD/origin/status | Preservados em `CLAUDE.md:483`; continuação do HANDOFF em `CLAUDE.md:484`. | [nao verificado]
| Quatro primeiras âncoras CODEX | Todas mantidas, concatenadas: `CLAUDE.md:518`. |
| Seis demais âncoras CODEX | Todas mantidas, concatenadas: `CLAUDE.md:519`. |
| S3 EXERCÍCIO, não CAMINHO; zero com os dois verdes | Preservado; “dois” virou “2”: `CLAUDE.md:567`. |
| Corte 23/09: sem fechamento, mostrar número do motor rotulado; morre “quem CHAMA o motor” | Corte em `CLAUDE.md:570`; objeto “numero de motor” e rótulo em `CLAUDE.md:568`. |
| L-095 não cede; histórico lap-18 | Ambos em `CLAUDE.md:570`. |
| L-009/L-082: item 1 retira `!` somente na competência corrente | Redação equivalente pela negativa: “fora do item 1 (competencia CORRENTE) o `!` segue”: `CLAUDE.md:575`. |
| Paga, vínculo/escala retroativo, atalho; sem critério escrito vale L-009; introdução do DIFF | Casos em `CLAUDE.md:575`; cláusula residual e DIFF em `CLAUDE.md:576`. |
| DIFF ANTECIPADO; backup/renome do TXT pelo admin | Ambos em `CLAUDE.md:576`. |

**2) Os resultados foram transcritos, mas a referência “(2)” perdeu precisão.** NOTURNO conserva exatamente `(a) 1,5 / (b) 2,3 / (b) parcial 4,6`; no aval original, “item (2)” significa classificação pelo Codex. No bloco atual, o item 2 significa DESPACHO; a classificação está no item 6. Portanto, não certifico equivalência inequívoca da linha inteira sem explicitar o destino dessa referência. (`app/docs/PROMPTS.md:1671`; `CLAUDE.md:559`; `CLAUDE.md:528`; `CLAUDE.md:553`; `app/docs/PISTA.md:124`)

Há também uma ressalva de vigência: o aval de **10:4x substitui expressamente o de 10:3x**, passando NOTURNO 1,5 para **(e)**, com falha do Codex zero. A linha adicionada reproduz o resultado histórico de 10:3x, mas não informa essa substituição. (`app/docs/PROMPTS.md:1672`; `CLAUDE.md:559`)

**3) O corte do PLACAR está preservado em relação ao texto removido do anexo.** Na leitura conjunta, continuam: sem fechamento → MOSTRAR número do motor com `'sem apuracao ainda'`; eliminação de “quem CHAMA o motor”; L-095 intacta. A frase isolada ficou menos explícita, mas seu referente permanece no bloco. (`CLAUDE.md:568`; `CLAUDE.md:570`)

O cotejo direto com **`41b4d89d:CLAUDE.md`**, além do texto apresentado no anexo, é **[nao verificado]**.

**4) A frase conserva a cláusula residual de AVAL-DE-CRITERIO; sozinha, não expressa a regra inteira.** O registro L-082 também determina “sem critério escrito → L-009”. As condições restantes aparecem no item 2 de DINHEIRO: `!` de critério, somente campos-alvo, demais campos de todos os colaboradores zerados contra o GRAVADO, total na faixa aprovada e violação → PAREI com tabela. (`CLAUDE.md:576`; `app/docs/LEIS.md:109`; `CLAUDE.md:586–588`)

No conjunto atual, essas condições estão sob competência exportada; a não exportada tem a preaprovação condicionada do item 1. A equivalência integral de redação e escopo com **41b4d89d** permanece **[nao verificado]**. (`CLAUDE.md:577–588`)

**5) Bytes medidos com `wc -c`:**

| Recorte | Bytes |
|---|---:|
| 7b: título incluído, até imediatamente antes de `## 8.`, incluindo a linha vazia final | **20.997** (`CLAUDE.md:436–627`) |
| CLAUDE.md inteiro | **57.798** (`CLAUDE.md:1–631`) |

## 31/10 13:42 · sessao 342dfc199d5b · - · - · origem=Ronald (tmux)

Q4 tem falhas concretas:

- **Motivo só pontuação passa:** `ERRO-BOBO OBRA-X: _` e `...: ___` são aceitos, pois `\w` inclui underscore. Reproduzi em memória, sem escrita. `...` e `!!!` são recusados. Fonte: `bin/disjuntor.py:615-617`; testes cobrem apenas `...` como pontuação em `bin/tests/test_disjuntor.sh:704`.
- **Link interno passa:** `_sitio_existe` resolve `realpath` antes de abrir; se `link.py` aponta para um arquivo dentro da raiz, `_abre` recebe o destino e aceita. O `O_NOFOLLOW` deixa de detectar o link original. Confirmei por simulação de `realpath` em memória. Fontes: `bin/disjuntor.py:575-581`, `bin/orcamento_contexto.py:74`.
- **Referência fora da raiz pode ser ignorada:** a linha `DESENHO OBRA-X: lei=juiz unico pergunta=quem? bin/disjuntor.py:1 /etc/passwd:1` é aceita; a regex exige extensão, portanto só valida `bin/disjuntor.py:1`. Também passa referência válida seguida de `bin/disjuntor.py:-9`, cuja segunda referência não casa. Fontes: `bin/disjuntor.py:77`, `bin/disjuntor.py:608-612`. Contradiz “todo arquivo:linha” em `bin/disjuntor.py:596`; teste correspondente só usa referências com extensão em `bin/tests/test_disjuntor.sh:720`.
- **Número malformado passa:** `bin/disjuntor.py:1.5` e `bin/disjuntor.py:1xyz` são aceitos, validando apenas o prefixo `:1`. Reproduzido em memória. Fontes: `bin/disjuntor.py:77`, `bin/disjuntor.py:609-610`.
- Para referências reconhecidas integralmente, absoluto, resolução para fora da raiz, linha zero e além do fim são recusados; última linha sem quebra é contada. Fontes: `bin/disjuntor.py:573-588`. Casos existentes: `bin/tests/test_disjuntor.sh:714-719`.

Q6:

- AST do pai contra o novo identifica seis funções removidas: `linha_libera_b3`, `libera_b3`, `_topo_shas`, `varrer_b3`, `_topo_e_mtime`, `b3_disparados`; nenhuma removida de `hook_stop_fila1.py`. Evidência de remoção via diff/AST, sem arquivo persistido: `[nao verificado]` para citar diretamente uma linha inexistente no novo.
- `rg -n -uuu` em `bin/` e `app/` não encontrou chamadas residuais aos seis nomes; o único resultado foi documentação antiga mencionando `linha_libera_b3` em `app/docs/RELATO.md:11`.
- Os dois chamadores produtivos agora usam `decide_b3`: `bin/disjuntor.py:1136`, `bin/hook_stop_fila1.py:1044`.

## 31/10 13:42 · sessao eabc6a4aa035 · - · - · origem=Ronald (tmux)

Questão 2: a lista de resultados foi copiada de `app/docs/PROMPTS.md:1671` para `CLAUDE.md:559`: NOTURNO `(a) 1,5 / (b) 2,3 / (b) parcial 4,6`, V-FASE refeita pelo `(2)`. Entretanto, a referência perdeu precisão: no aval original `(2)` é classificação pelo Codex; no bloco consolidado o item 2 é DESPACHO (`CLAUDE.md:528`), e a classificação está no item 6 (`CLAUDE.md:553`). A PISTA ainda tem o item `(2) CLASSIFICACAO` original (`app/docs/PISTA.md:124`). Portanto: conteúdo classificatório transcrito, referência cruzada sem equivalência inequívoca (`CLAUDE.md:559`; `app/docs/PROMPTS.md:1671`; `app/docs/PISTA.md:124`).

Há ainda conflito com o aval seguinte: 10:4x substitui expressamente o de 10:3x e classifica NOTURNO 1,5 como `(e)`, falha do Codex zero (`app/docs/PROMPTS.md:1672`). A própria consolidação distingue `(a)` tema presente de `(e)` não perguntado (`CLAUDE.md:556`; `CLAUDE.md:557`). Assim, a linha 559 pode registrar o estado histórico de 10:3x, mas não reproduz a classificação posterior vigente; sem explicitar a substituição, pode reintroduzir a atribuição antiga (`CLAUDE.md:559`; `app/docs/PROMPTS.md:1672`). A PISTA confirma que sua versão é a de 10:4x, que substitui 10:3x (`app/docs/PISTA.md:120`).

Questão 4: a frase isolada não contém toda a AVAL-DE-CRITERIO; é a cláusula residual “sem critério escrito → L-009” (`CLAUDE.md:576`; `app/docs/LEIS.md:109`). As condições materiais continuam no item 2 de DINHEIRO: `!` de critério, somente campos-alvo, zero nos demais campos/colaboradores, medição contra o GRAVADO, total na faixa, violação → PAREI com tabela (`CLAUDE.md:586`; `CLAUDE.md:587`; `CLAUDE.md:588`). A L-082 registra exatamente esses critérios e o residual sem critério (`app/docs/LEIS.md:109`).

O conjunto atual mantém essa estrutura para competência exportada (`CLAUDE.md:582`; `CLAUDE.md:586`), com exceção de competência não exportada no item 1 (`CLAUDE.md:577`) e referência explícita aos casos em que o `!` segue (`CLAUDE.md:575`). Comparação literal e de escopo com `41b4d89d:CLAUDE.md`: [nao verificado], pois esse conteúdo histórico não foi fornecido nem encontrado como arquivo nesta cópia.

_1086 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
