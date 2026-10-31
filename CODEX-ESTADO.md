# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 17:58** · publicado em **31/10/2026 17:59** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 17:58 · sessao b1f133212c08 · PADRAO-DE-CIMA · verificador · origem=Code

**Pré-veredito: REPROVA.** Há caminhos que liberam por falta de sinal, uma liberação anterior que pode valer na primeira observação e uma corrida entre estado e escopo. **Certeza alta sobre os fluxos; ocorrência em produção [não verificado].** (`bin/disjuntor.py:1423`, `bin/disjuntor.py:1477`, `bin/disjuntor.py:1374`, `bin/disjuntor.py:1401`, `bin/disjuntor.py:1421`)

Fiz leitura estática e sondas isoladas em memória, com I/O substituído. Resultado das suítes completas: **[não verificado]**; executá-las criaria e apagaria arquivos, contrariando sua restrição. (`bin/tests/test_padrao_de_cima.sh:29`, `bin/tests/test_padrao_de_cima.sh:36`, `bin/tests/test_lei_akita.sh:168`)

**1. Quem escreve e quem lê**

Certeza **alta** nesta tabela; autoria humana efetiva e execução em produção ficam **[não verificado]**.

| Campo/arquivo | Quem escreve | Quem lê |
|---|---|---|
| **RELATO — topo** | Escrita manual é reconhecida pelo código; o diff não escreve `DESENHO` nem `PADRAO-DE-CIMA`. A porta de alarmes preserva o topo. (`app/core/esteira_vigia.py:829`, `app/core/esteira_vigia.py:839`, `app/core/esteira_vigia.py:872`) | Recorte central em `_bloco_do_topo`; PAREI/ESPERANDO por `_linha_do_topo`; B3 por `_topo_consistente`; PDC por `_topo_de` sobre os bytes do corpo. (`bin/hook_stop_fila1.py:400`, `bin/hook_stop_fila1.py:415`, `bin/disjuntor.py:676`, `bin/disjuntor.py:1359`) |
| **RELATO — corpo e negativas** | `relato_negacao` chama `esteira_vigia.no_relato`, que toma flock do diretório e publica por `replace`. **Também existem escritores fora dessa porta:** `gerar_cortes` e `deploy_agendado`. (`bin/disjuntor.py:234`, `bin/disjuntor.py:236`, `app/core/esteira_vigia.py:848`, `app/core/esteira_vigia.py:884`, `bin/gerar_cortes.py:136`, `bin/deploy_agendado.sh:44`) | PDC varre o arquivo inteiro; fora do diff, Stop verifica prova, handoff lê o bloco inicial e publicação lê o RELATO. (`bin/disjuntor.py:1351`, `bin/disjuntor.py:1365`, `bin/hook_stop_fila1.py:1176`, `bin/handoff_sessao.sh:92`, `bin/relato.sh:56`) |
| **BACKLOG — bloco OBRAS** | Bloco declarado manual; `gerar_backlog` preserva-o e regrava o documento; `backlog_estado` altera a célula de estado diretamente. (`bin/gerar_backlog.py:103`, `bin/gerar_backlog.py:119`, `bin/gerar_backlog.py:140`, `bin/backlog_estado.py:58`, `bin/backlog_estado.py:59`) | Selo PDC, Stop/fila, handoff e gerador do resumo. **O resolvedor PDC não consulta BACKLOG.** (`bin/tests/test_padrao_de_cima.sh:328`, `bin/hook_stop_fila1.py:173`, `bin/handoff_sessao.sh:54`, `bin/gerar_backlog_topo.py:12`, `bin/disjuntor.py:1282`) |
| **`PROGRAMAS`** | Cadastro literal no código, separado do BACKLOG. (`bin/disjuntor.py:1110`) | `_PdcResolvedor.entidades`, somente sem entidade; selo verifica as chaves contra OBRAS. (`bin/disjuntor.py:1282`, `bin/tests/test_padrao_de_cima.sh:333`) |
| **GLOSSARIO / PLANO-PADROES** | Estes leitores não os alteram; padrão novo exige corte. Autor efetivo de cada alteração: **[não verificado]**. (`bin/padrao_vocab.py:34`, `CLAUDE.md:70`) | `padrao_vocab.vocabulario`: duas primeiras colunas do GLOSSARIO, negritos do NORTE e itens dos ADRs. (`bin/padrao_vocab.py:41`, `bin/padrao_vocab.py:48`, `bin/padrao_vocab.py:53`) |
| **Vocabulário em memória** | Produzido por `padrao_vocab.vocabulario`, normalizado por `limpo`. (`bin/padrao_vocab.py:21`, `bin/padrao_vocab.py:57`) | LEI-AKITA e PDC. (`bin/tests/test_lei_akita.sh:89`, `bin/tests/test_lei_akita.sh:110`, `bin/disjuntor.py:1327`, `bin/disjuntor.py:1315`) |
| **Estado PDC:** `eventos`, `visto_em`, `entidades`, `nomeados`, `libera_vistas`, `disparadas` | `decide_pdc`, persistido por `_com_estado` sob flock. (`bin/disjuntor.py:1360`, `bin/disjuntor.py:1373`, `bin/disjuntor.py:1396`, `bin/disjuntor.py:1401`, `bin/disjuntor.py:1405`, `bin/disjuntor.py:260`, `bin/disjuntor.py:281`) | Janela/agrupamento, `estado_pdc` e painel. (`bin/disjuntor.py:1287`, `bin/disjuntor.py:1292`, `bin/disjuntor.py:1458`, `bin/disjuntor.py:1537`) |
| **Escopo:** `em`, `disparadas`, `obras_com_evento` | `_pdc_grava_escopo`, depois da transição do estado. (`bin/disjuntor.py:1421`, `bin/disjuntor.py:1432`) | Fallback de `estado_pdc`. A decisão consulta `disparadas`; não valida frescor de `em` nem usa `obras_com_evento` nesse ramo. (`bin/disjuntor.py:1446`, `bin/disjuntor.py:1463`) |
| **`disjuntor.log` / painel** | `trilha`; PDC anuncia disparo/liberação após tentativa de persistência. (`bin/disjuntor.py:135`, `bin/disjuntor.py:1413`) | `ler` conta eventos; `_pdc_painel` lê o estado; `linha` apresenta os contadores. (`bin/disjuntor.py:2055`, `bin/disjuntor.py:2077`, `bin/disjuntor.py:1537`, `bin/disjuntor.py:2189`) |
| **`DISJUNTOR_DESLIGADO`** | Escritor autorizado: terminal do Ronald; Code recebe guarda contra ferramentas que nomeiem a chave. Fixtures também a criam. (`bin/disjuntor_settings.json:3`, `bin/disjuntor.py:167`, `bin/disjuntor.py:171`, `bin/tests/test_padrao_de_cima.sh:313`) | `desligado`, por `lexists`, inclusive symlink; usado nas portas B3/PDC, PreToolUse e Stop. (`bin/disjuntor.py:153`, `bin/disjuntor.py:1070`, `bin/disjuntor.py:1504`, `bin/disjuntor.py:1979`, `bin/hook_stop_fila1.py:1039`) |
| **B3: estado e triagem** | `decide_b3`, `rearma_b3` e `triagem`, pela trava comum do estado. (`bin/disjuntor.py:985`, `bin/disjuntor.py:1058`, `bin/disjuntor.py:838`, `bin/disjuntor.py:844`) | `julga_b3` → `estado_b3` → `b3_parada`/`_nega_b3`; Stop chama `julga_b3`. (`bin/disjuntor.py:1006`, `bin/disjuntor.py:1015`, `bin/disjuntor.py:1072`, `bin/disjuntor.py:1889`, `bin/hook_stop_fila1.py:1048`) |

Os hooks configurados em `.claude/` apontam para caminhos absolutos da árvore viva. Que a sessão de produção executava exatamente este hash: **[não verificado]**. (`.claude/settings.json:9`, `.claude/settings.json:68`)

**2. O vocabulário tem uma fonte?**

**Uma implementação da regra, alimentada por dois documentos.** Os dois consumidores chamam `padrao_vocab.vocabulario`; a extração saiu do heredoc do LEI-AKITA. **Certeza alta.** (`bin/padrao_vocab.py:34`, `bin/tests/test_lei_akita.sh:89`, `bin/disjuntor.py:1327`)

**Não é garantida a mesma lista concreta:** LEI-AKITA lê os documentos de `ALVO` por `git show`; PDC lê o disco da raiz. A igualdade depende desses conteúdos coincidirem. **Certeza alta; coincidência em produção [não verificado].** (`bin/tests/test_lei_akita.sh:75`, `bin/tests/test_lei_akita.sh:77`, `bin/disjuntor.py:1325`)

Há regex próprias para **formatos diferentes**: campo LEI-AKITA e quatro campos da liberação PDC. Ambas delegam a validade do termo ao vocabulário compartilhado. Há ainda termos esperados no MORDE e o conjunto `B3_LEIS`, que julga o desenho B3, não o universo de `padrao=`. **Certeza alta.** (`bin/tests/test_lei_akita.sh:99`, `bin/disjuntor.py:1130`, `bin/tests/test_lei_akita.sh:110`, `bin/disjuntor.py:1315`, `bin/tests/test_lei_akita.sh:163`, `bin/disjuntor.py:87`)

Outra implementação equivalente escondida em toda a árvore: **[não verificado]**; não foi localizada na busca textual.

**3. Falhas e limites**

| Situação | Comportamento e certeza |
|---|---|
| **GLOSSARIO ausente** | **Alta:** LEI-AKITA recebe `VocabIlegivel` e falha pela ausência de conclusão do juiz. PDC não libera nem consome a candidata. Porém o vocabulário só é consultado quando há linha candidata no topo: ausência do documento não provoca recusa global. (`bin/padrao_vocab.py:46`, `bin/tests/test_lei_akita.sh:90`, `bin/tests/test_lei_akita.sh:298`, `bin/disjuntor.py:1384`, `bin/disjuntor.py:1388`, `bin/disjuntor.py:1390`) |
| **RELATO legível, sem topo útil** | **Alta:** o recorte pode retornar `[]`; isso não libera entidade disparada. Um arquivo vazio também não fornece novos eventos. Não existe validação separada de “topo obrigatório”. (`bin/hook_stop_fila1.py:427`, `bin/hook_stop_fila1.py:443`, `bin/disjuntor.py:1365`, `bin/disjuntor.py:1383`) |
| **RELATO ilegível com estado legível** | **Alta, confirmado em memória:** `decide_pdc` preenche `nao_sei`, mas `estado_pdc` ignora-o. Sem disparada conhecida para a obra, retorna **LIVRE**. Com estado vazio, o portão fica verde sem ter lido a fonte. (`bin/disjuntor.py:1423`, `bin/disjuntor.py:1474`, `bin/disjuntor.py:1477`) |
| **BACKLOG perde uma chave cadastrada** | **Alta:** runtime permanece igual, pois usa o dict literal; o selo reprova a divergência. Se a obra faltar no próprio dict e não resolver modelo, recebe entidade vazia e não agrupa. (`bin/disjuntor.py:1110`, `bin/disjuntor.py:1282`, `bin/disjuntor.py:1284`, `bin/tests/test_padrao_de_cima.sh:333`) |
| **Codex rc 75** | **Alta:** limite identificado registra `limite.log`, orienta seguir por sonnet e retorna 75, sem sidecar válido desta chamada. A continuação efetiva da obra é **[não verificado]**; o wrapper apenas emite a orientação. (`bin/codex_pergunta.sh:186`, `bin/codex_pergunta.sh:191`, `bin/codex_pergunta.sh:192`, `bin/codex_pergunta.sh:194`, `bin/codex_pergunta.sh:201`) |
| **Disjuntor desligado** | **Alta:** portas B3/PDC retornam zero; PreToolUse e Stop pulam os disjuntores. A guarda contra alterar a chave continua ativa. (`bin/disjuntor.py:1070`, `bin/disjuntor.py:1504`, `bin/disjuntor.py:1978`, `bin/disjuntor.py:1979`, `bin/hook_stop_fila1.py:1039`) |
| **Falha do censo/resolvedor** | **Alta, confirmado em memória:** exceção vira `entidades=[]`, persistida no evento. Restaurar o censo não reclassifica o mesmo SHA, porque só eventos novos são resolvidos. Pode permanecer verde por ausência de entidade. (`bin/disjuntor.py:1366`, `bin/disjuntor.py:1369`, `bin/disjuntor.py:1373`, `bin/disjuntor.py:1298`) |
| **Falha ao gravar liberação** | **Alta:** a liberação é recolocada no resultado como disparada; nesta chamada continua recusada. (`bin/disjuntor.py:1418`, `bin/disjuntor.py:1419`) |
| **Estado ilegível** | **Alta:** usa escopo legível; fora dele retorna LIVRE. Sem escopo, recusa obras com evento; sem RELATO legível, recusa todas. (`bin/disjuntor.py:1461`, `bin/disjuntor.py:1465`, `bin/disjuntor.py:1471`) |

Mais dois bloqueios:

- **Liberação anterior aceita na primeira observação — alta, confirmado em memória.** Encontrando pela primeira vez dois desenhos e uma liberação válida já no topo, o ciclo dispara e libera imediatamente. O mtime é descartado; não existe fotografia das linhas anteriores ao disparo como no B3. O selo cobre somente liberação consumida numa observação anterior com um desenho. (`bin/disjuntor.py:1351`, `bin/disjuntor.py:1374`, `bin/disjuntor.py:1396`, `bin/disjuntor.py:1401`, `bin/tests/test_padrao_de_cima.sh:195`, `bin/tests/test_padrao_de_cima.sh:197`)
- **Escopo pode regredir — alta, inferência de concorrência.** A gravação ocorre após liberar a trava do estado. A decide primeiro e pausa; B decide depois e publica escopo novo; A publica o antigo por cima. Na próxima falha do estado, uma obra omitida pelo escopo antigo recebe LIVRE. `os.replace` evita arquivo parcial, mas não essa inversão temporal. (`bin/disjuntor.py:295`, `bin/disjuntor.py:1416`, `bin/disjuntor.py:1421`, `bin/disjuntor.py:1437`, `bin/disjuntor.py:1465`)

**4. Os selos mordem?**

**Há mordida comportamental, com lacunas.** Execução integral e sobrevivência experimental dos mutantes abaixo: **[não verificado]**; os graus indicados são por inspeção.

| Selo | Mordida existente | Mutante simples que pode escapar |
|---|---|---|
| **`test_padrao_de_cima.sh`** | **Alta:** portas reais, liberações, redisparo, janela, entidades distintas, desligamento e controles sem mutação. Apagar o gate do wrapper tem mutante declarado. (`bin/tests/test_padrao_de_cima.sh:110`, `bin/tests/test_padrao_de_cima.sh:179`, `bin/tests/test_padrao_de_cima.sh:285`, `bin/tests/test_padrao_de_cima.sh:377`, `bin/tests/test_padrao_de_cima.sh:388`) | **Alta por inspeção:** remover somente `O261-E-CELULA-20-UM-DONO` do cadastro, ou trocar seu destino. O selo exige apenas chaves existentes e dict não vazio; os cenários de programa usam outras duas obras. (`bin/disjuntor.py:1113`, `bin/tests/test_padrao_de_cima.sh:157`, `bin/tests/test_padrao_de_cima.sh:333`) |
| **`test_lei_akita.sh`** | **Alta:** lista vazia, inversão da aceitação e retirada da validação são atingidas pelos termos obrigatórios e casos positivos/negativos. (`bin/tests/test_lei_akita.sh:163`, `bin/tests/test_lei_akita.sh:181`, `bin/tests/test_lei_akita.sh:203`) | **Alta por inspeção:** substituir o `raise VocabIlegivel` do GLOSSARIO por `pass`. Os cenários fornecem as fontes; não exercitam sua ausência. O mutante permitiria vocabulário parcial quando o GLOSSARIO faltasse. (`bin/padrao_vocab.py:45`, `bin/padrao_vocab.py:46`, `bin/tests/test_lei_akita.sh:172`, `bin/tests/test_padrao_de_cima.sh:74`) |
| **`test_disjuntor.sh`** | **Alta:** verifica leituras dentro das transições e agora exige duas tomadas da trava no verificador final, B3 e PDC. (`bin/tests/test_disjuntor.sh:845`, `bin/tests/test_disjuntor.sh:879`, `bin/tests/test_disjuntor.sh:882`) | **Alta por inspeção, considerando este selo isoladamente:** apagar a consulta ao vocabulário em `_pdc_libera`. Seus acréscimos não exercitam liberação PDC; o selo específico é quem possui esse mutante. (`bin/disjuntor.py:1315`, `bin/tests/test_disjuntor.sh:432`, `bin/tests/test_disjuntor.sh:882`, `bin/tests/test_padrao_de_cima.sh:396`) |

Há duas mordidas contaminadas:

- **Alta:** `B3-foto-parcial-engolida` injeta `H._bloco_do_topo()` onde `H` não está definido; os imports são locais a outras funções. A morte pode ser `NameError`, aceito pelo harness como qualquer falha. (`bin/tests/test_disjuntor.sh:1663`, `bin/tests/test_disjuntor.sh:1667`, `bin/disjuntor.py:676`, `bin/disjuntor.py:688`, `bin/disjuntor.py:702`, `bin/tests/test_disjuntor.sh:1491`)
- **Alta:** retirar a guarda “entidade já disparada” leva a `disp[ent]` inexistente no cenário antecipado; o harness também aceita esse erro como morte. Isso não prova a semântica temporal pretendida. (`bin/tests/test_padrao_de_cima.sh:195`, `bin/tests/test_padrao_de_cima.sh:397`, `bin/disjuntor.py:1402`, `bin/tests/test_padrao_de_cima.sh:375`)

**5. BUG 6**

**Curado na porta defeituosa do wrapper. Certeza alta.** `b3_parada` testa a chave antes de consultar estado; o teste específico remove exatamente essa guarda no mutante. (`bin/disjuntor.py:1070`, `bin/tests/test_padrao_de_cima.sh:319`, `bin/tests/test_padrao_de_cima.sh:385`)

Os outros caminhos produtivos encontrados também têm guarda: PreToolUse antes de `pretool`, e Stop antes de `julga_b3`. **Não encontrei outro chamador produtivo desprotegido.** As funções internas `decide_b3`/`estado_b3` continuam sem guarda própria; uma chamada direta não herda o desligamento. **Certeza alta nos caminhos citados; ausência absoluta de outro chamador [não verificado].** (`bin/disjuntor.py:1979`, `bin/disjuntor.py:1930`, `bin/disjuntor.py:1944`, `bin/hook_stop_fila1.py:1039`, `bin/hook_stop_fila1.py:1048`, `bin/disjuntor.py:903`, `bin/disjuntor.py:1015`)

**6. Classes históricas procuradas**

| Classe | Resultado e grau de certeza |
|---|---|
| **Segundo juiz duplicando registrado** | Reuso do censo e do juiz do vocabulário, confirmado. `_so_modelos` acrescenta filtro de herança; `censo.modelos` coleta classes mais amplamente. Duplicação de juiz de negócio não demonstrada; ausência absoluta **[não verificado]**. (`bin/disjuntor.py:1197`, `bin/disjuntor.py:1216`, `bin/disjuntor.py:1242`, `app/core/censo_escritas.py:90`, `app/core/censo_escritas.py:109`) |
| **Segundo escritor fora da porta** | **Encontrado, alta:** `gerar_cortes` e `deploy_agendado` regravam RELATO fora do flock comum; risco de sobrescrever atualização concorrente. Resíduo já declarado no código. (`bin/gerar_cortes.py:117`, `bin/gerar_cortes.py:136`, `bin/deploy_agendado.sh:37`, `bin/deploy_agendado.sh:44`, `app/core/esteira_vigia.py:839`) |
| **Relógio fora da borda** | **Encontrado, alta:** hooks injetam `agora`, mas CLIs deixam `decide_pdc` e `decide_b3` chamar `time.time()` internamente. Trilha também lê relógio próprio. (`bin/disjuntor.py:1977`, `bin/disjuntor.py:1506`, `bin/disjuntor.py:1344`, `bin/disjuntor.py:903`, `bin/disjuntor.py:140`) |
| **TOCTOU / fotos diferentes** | **RELATO corpo/topo curado, alta:** recorte dos mesmos bytes validados. **Modelos ainda expostos, alta condicional:** censo lê `models`, filtro relê sem fotografia comum. O painel também abre a trilha quatro vezes, podendo misturar instantes. (`bin/disjuntor.py:709`, `bin/disjuntor.py:713`, `bin/disjuntor.py:1359`, `app/core/censo_escritas.py:105`, `bin/disjuntor.py:1232`, `bin/disjuntor.py:2055`, `bin/disjuntor.py:2058`) |
| **Selo sem mordida** | **Encontradas lacunas e mortes por exceção, alta**, detalhadas no item 4. Os selos estão ligados à régua por glob; LEI-AKITA também ao pre-push. Execução efetiva **[não verificado]**. (`bin/tests/test_disjuntor.sh:1667`, `bin/tests/test_padrao_de_cima.sh:375`, `bin/regua.sh:138`, `bin/pre-push.sh:38`) |
| **Ordem de trava** | **Alta:** wrapper toma slot → resposta → estado; triagem toma resposta compartilhada → estado. Não identifiquei inversão nesses percursos. O defeito concreto é publicar escopo fora da trava. (`bin/codex_pergunta.sh:145`, `bin/codex_pergunta.sh:163`, `bin/codex_pergunta.sh:165`, `bin/disjuntor.py:812`, `bin/disjuntor.py:844`, `bin/disjuntor.py:1421`) |
| **Fallback** | **Encontrado, alta:** escopo antigo pode liberar; RELATO ilegível perde `nao_sei`; falha de entidade vira vazio persistente. O fallback por programa, por sua vez, é explicitamente cadastrado. (`bin/disjuntor.py:1465`, `bin/disjuntor.py:1423`, `bin/disjuntor.py:1477`, `bin/disjuntor.py:1370`, `bin/disjuntor.py:1282`) |
| **argv com dado sensível** | **Alta quanto à forma:** novos gates passam somente `$OBRA`; pergunta e anexos chegam ao Codex pelo stdin. Porém `CODEX_OBRA` só é validado como não vazio, sem raspagem. Dado sensível efetivamente presente nele: **[não verificado]**. (`bin/codex_pergunta.sh:44`, `bin/codex_pergunta.sh:45`, `bin/codex_pergunta.sh:165`, `bin/codex_pergunta.sh:168`, `bin/codex_pergunta.sh:178`) |

## 31/10 17:56 · sessao 5c5a8d6e50e5 · V3-RETOMA-PERIODOS · verificador · origem=Code

**Veredito: o diff corrige os casos apresentados, mas não fecha a classe do aval.** Continua existindo pausa reconstruída pelas pontas; a origem na célula não é exigida; o censo deixa passar reconstruções simples; e há contraexemplo com HE100 noturna maior que a HE100 total. **Certeza alta.** (`app/ponto/calculador/nucleo.py:149`, `app/ponto/calculador/alimentacao.py:79`, `app/ponto/tests/test_v3_periodos_da_celula.py:218`, `app/ponto/motor_calculo_v2.py:391`)

Executei cálculos e mutantes em memória, carregando funções da cópia por AST. Execução da suíte Django, integração com banco, impacto em produção e comprovação criptográfica do hash: **[nao verificado]**. [nao verificado]

**1. Autoridade, escritores e leitores — certeza alta**

A autoridade registrada para **“que períodos e que intervalo teve este dia?”** é `ponto/juiz_batida.py::periodos_do_dia`. Quanto vale o par é outra pergunta, atribuída a `MotorBase.calcular_periodo`. O próprio JUIZES.md manda prevalecer o registro vivo. (`app/core/juizes.py:1143`, `app/core/juizes.py:1144`, `app/core/juizes.py:1145`, `app/docs/JUIZES.md:14`)

O v3 lê **o mesmo resultado do motor que o espelho lê**. Isso comprova compartilhamento da fonte intermediária, mas não comprova que todos os períodos vieram da célula: o motor conserva geometria alternativa quando não consegue usar o juiz da ata. (`app/ponto/services/espelho.py:432`, `app/ponto/calculador/alimentacao.py:79`, `app/ponto/motor_calculo_v2.py:480`, `app/ponto/motor_calculo_v2.py:523`, `app/ponto/motor_calculo_v2.py:582`)

| Campo/fluxo | Quem escreve | Quem lê |
|---|---|---|
| `resultado.periodos` e `periodos_ft` | Motor publica os períodos calculados. `app/ponto/motor_calculo_v2.py:2396`, `app/ponto/motor_calculo_v2.py:2430` | Espelho: `app/ponto/services/espelho.py:432`; leitor comum: `app/ponto/calculador/nucleo.py:393`; alimentação: `app/ponto/calculador/alimentacao.py:79`; releitura: `app/ponto/calculador/periodo.py:607` |
| `efetivo_entrada`, `efetivo_saida`, `intervalos_do_motor` | Carimbo: `app/ponto/motor_calculo_v2.py:1778`; **também há escrita direta de `efetivo_saida`**: `app/ponto/motor_calculo_v2.py:1421` | Porta: `app/ponto/calculador/alimentacao.py:86`, `app/ponto/calculador/alimentacao.py:107`; releitura: `app/ponto/calculador/periodo.py:742`, `app/ponto/calculador/periodo.py:752` |
| `dia_da_jornada` | Motor expõe/mescla o mapa. `app/ponto/motor_calculo_v2.py:1011`, `app/ponto/motor_calculo_v2.py:2897` | `app/ponto/calculador/alimentacao.py:76`, `app/ponto/calculador/periodo.py:594` |
| `pares`: segmentos somáveis | Porta recorta conforme intervalos e `intra_descontada`. `app/ponto/calculador/alimentacao.py:134`, `app/ponto/calculador/alimentacao.py:140`, `app/ponto/calculador/alimentacao.py:164` | Núcleo chamado em `app/ponto/calculador/alimentacao.py:478`; soma em `app/ponto/calculador/nucleo.py:147` |
| `pares_por_dia`: períodos inteiros | `app/ponto/calculador/alimentacao.py:165` | Porta transmite em `app/ponto/calculador/alimentacao.py:560`; noturnas em `app/ponto/calculador/regras.py:885` |
| `intervalos_por_dia` | `app/ponto/calculador/alimentacao.py:166` | `app/ponto/calculador/alimentacao.py:482`, `app/ponto/calculador/alimentacao.py:562`; desconto em `app/ponto/calculador/regras.py:86` |
| `he_pontas_por_dia`: entrada, saída, HE50/100 antigas, intervalos, teto | Criação em `app/ponto/calculador/alimentacao.py:115`; teto pela soma dos segmentos em `app/ponto/calculador/alimentacao.py:169` | Transmissão em `app/ponto/calculador/alimentacao.py:561`; distribuição em `app/ponto/calculador/regras.py:858`; HE antiga serve ao diagnóstico em `app/ponto/calculador/regras.py:880` |
| `pausa_min` | **Gap entre segmentos**, em `app/ponto/calculador/nucleo.py:149`, `app/ponto/calculador/nucleo.py:152` | Porta transmite em `app/ponto/calculador/alimentacao.py:550`; intra e limite de HE usam em `app/ponto/calculador/regras.py:749`, `app/ponto/calculador/regras.py:760`, `app/ponto/calculador/regras.py:767` |
| `fora_por_dia` | Soma de `minutos_fora_janela`. `app/ponto/calculador/alimentacao.py:96` | Pontualidade recebe em `app/ponto/calculador/alimentacao.py:548` |
| Abertos e contadores | `app/ponto/calculador/alimentacao.py:90`, `app/ponto/calculador/alimentacao.py:117`, `app/ponto/calculador/alimentacao.py:163` | Retornam nos insumos em `app/ponto/calculador/alimentacao.py:325`; não constituem bloqueio das rubricas em `app/ponto/calculador/alimentacao.py:478` |
| Split HE50/100 noturna | Função comum chamada por período em `app/ponto/calculador/regras.py:864`; valores escritos em `app/ponto/calculador/regras.py:874`, `app/ponto/calculador/regras.py:876` | Fechamento entrega as rubricas à lavra em `app/ponto/services/fechamento.py:1256`; lavrador percorre em `app/ponto/services/dia_pago.py:320` |
| Transbordo e saldo | Toma, desconta e registra sobra em `app/ponto/calculador/regras.py:861`, `app/ponto/calculador/regras.py:869`, `app/ponto/calculador/regras.py:872` | Cada parcela alimenta `he_noturna` em `app/ponto/calculador/regras.py:864`; bloqueio específico por `he_sem_periodo_min`: **[nao verificado]** |

Portanto: **o v3 lê o resultado compartilhado com o espelho; não exige a autoridade da célula em todos os caminhos.** `periodos_do_resultado` exige atributos existentes e diferentes de `None`, mas aceita listas vazias e não verifica proveniência. (`app/ponto/calculador/nucleo.py:393`, `app/ponto/calculador/nucleo.py:400`)

**2. Ramos restantes pelas pontas — certeza alta**

- **Produção: pausa entre segmentos.** O núcleo calcula `entrada_atual − saída_anterior`, inclusive entre períodos distintos. Para 00–01 + 06–08, reproduzi **180 minutos trabalhados, 300 de pausa e envelope 480**, embora os períodos não tenham intervalos julgados. Essa pausa chega à intra. (`app/ponto/calculador/nucleo.py:149`, `app/ponto/calculador/nucleo.py:152`, `app/ponto/calculador/alimentacao.py:478`, `app/ponto/calculador/alimentacao.py:550`)
- **Ramo cru, quando `pares=None`.** Há agrupamento pelo gap da última batida, vão desde a primeira, cortes próprios, data da primeira batida, envelope última−primeira, pareamento sequencial e pausa entre pontas. A chamada de produção examinada passa lista e não entra nesse ramo. (`app/ponto/calculador/nucleo.py:188`, `app/ponto/calculador/nucleo.py:215`, `app/ponto/calculador/nucleo.py:237`, `app/ponto/calculador/nucleo.py:239`, `app/ponto/calculador/nucleo.py:245`, `app/ponto/calculador/nucleo.py:253`, `app/ponto/calculador/alimentacao.py:478`)
- **Decisão pelo meio do gap.** O ramo cru consulta envelopes pelas datas das pontas e decide se o vão é interno pelo ponto médio. (`app/ponto/calculador/nucleo.py:180`, `app/ponto/calculador/nucleo.py:183`)
- **Releitura em `periodo.py`.** Recorta o par pelos intervalos julgados e volta a obter pausa/envelope pelo núcleo. Aqui a geometria recebida é lavrada, mas a pausa continua recuperada pelos segmentos. (`app/ponto/calculador/periodo.py:304`, `app/ponto/calculador/periodo.py:355`, `app/ponto/calculador/periodo.py:357`)
- **Intervalo existente pode desaparecer de `pausa_min`.** Se `intra_descontada=0`, a porta mantém um segmento inteiro, mesmo havendo intervalo julgado. O noturno recebe o intervalo; a pausa escalar não o recebe. (`app/ponto/calculador/alimentacao.py:134`, `app/ponto/calculador/alimentacao.py:136`, `app/ponto/calculador/alimentacao.py:144`, `app/ponto/calculador/alimentacao.py:550`, `app/ponto/calculador/alimentacao.py:562`)

Não encontrei envelope executável por `min/max` de timestamps nos cinco módulos; `max(saidas)` aparece como relato de implementação anterior. A ausência além da cópia examinada é **[nao verificado]**. (`app/ponto/calculador/alimentacao.py:261`)

**3. O censo morde? Parcialmente — certeza alta**

Ele reconhece três grafias: vazio literal em keyword `intervalos`, subscript duplo diretamente nas keywords `entrada/saida` e vizinho `X[i+1][j]`. Não acompanha variáveis nem a semântica da reconstrução. (`app/ponto/tests/test_v3_periodos_da_celula.py:189`, `app/ponto/tests/test_v3_periodos_da_celula.py:195`, `app/ponto/tests/test_v3_periodos_da_celula.py:218`)

Executei o censo: os cinco arquivos retornam vazio. Estes mutantes também retornam vazio: [nao verificado]

```python
vazio = ()
he(entrada=p.e, saida=p.s, intervalos=vazio)

e, s = pts[0][0], pts[-1][1]
he(entrada=e, saida=s, intervalos=ivs)

he(entrada=min(ts), saida=max(ts), intervalos=ivs)
```

São falsos negativos decorrentes dos próprios predicados; a pausa existente com variável “anterior” já é uma testemunha concreta. A busca também usa `glob('*.py')`, sem recursão. (`app/ponto/tests/test_v3_periodos_da_celula.py:220`, `app/ponto/tests/test_v3_periodos_da_celula.py:222`, `app/ponto/tests/test_v3_periodos_da_celula.py:235`, `app/ponto/calculador/nucleo.py:149`)

**4. Regra, asserts e contraexemplos — certeza alta nos cálculos**

Os números do aval conferem **nas unidades declaradas pelos testes**: split em horas de relógio; hora reduzida é tratamento separado do adicional noturno. Validação jurídica externa da CLT/CCT: **[nao verificado]**. (`app/ponto/calculador/regras.py:88`, `app/ponto/calculador/regras.py:876`, `app/ponto/tests/test_v3_periodos_da_celula.py:12`)

| Caso | Resultado pela regra fornecida | Asserts |
|---|---|---|
| Feriado 21–05, pausa 23–00 | Trabalho 7h; noturno 22–23 + 00–05 = **6h HE100 noturna** | Conferem: `app/ponto/tests/test_v3_periodos_da_celula.py:111`, `app/ponto/tests/test_v3_periodos_da_celula.py:113` |
| Períodos 00–01 + 06–08 | **3h HE100**, das quais **1h noturna** | Conferem: `app/ponto/tests/test_v3_periodos_da_celula.py:169`, `app/ponto/tests/test_v3_periodos_da_celula.py:170` |
| 22–07, pausa 04h30–05h30, prorrogação e hora de 60min | **8h noturnas** | Confere: `app/ponto/tests/test_v3_periodos_da_celula.py:132` |

**Contraexemplos reproduzidos:**

- **Período único cruzando meia-noite, pausa inteiramente após 05h:** 22–07, pausa 05h30–06h30, prorrogação ativa, feriado em dobra, 480 trabalhados. `do_dia` retorna **HE100=8h e HE100 noturna=9h**. O período ganha prorrogação, mas a pausa é julgada isoladamente, começa após 05h e desconta zero. Viola a propriedade parte≤todo. (`app/ponto/motor_calculo_v2.py:110`, `app/ponto/motor_calculo_v2.py:145`, `app/ponto/motor_calculo_v2.py:391`, `app/ponto/calculador/regras.py:864`)
- **Pausa antes da janela:** 20–05, pausa 20h30–21h30, sem prorrogação: retorna **7h noturnas**, corretamente sem descontar a pausa diurna. (`app/ponto/motor_calculo_v2.py:99`, `app/ponto/motor_calculo_v2.py:143`)
- **HE somente na cauda prorrogada:** período 22–07, sem pausa, 120 minutos extras: `he_noturna` retorna **zero noturno**, porque recalcula a janela sobre 05–07, perdendo o contexto do período original. É outro limite da função compartilhada, preexistente ao diff. (`app/ponto/motor_calculo_v2.py:390`, `app/ponto/motor_calculo_v2.py:391`)
- **Sem períodos:** `pares=[]`, `he_pontas=[]`, 180 trabalhados em feriado retorna **HE100=3h, split=0h e `he_sem_periodo_min=180`**, sem recusar a decisão. Com `he_pontas=None`, declara ausência do split. (`app/ponto/calculador/regras.py:627`, `app/ponto/calculador/regras.py:845`, `app/ponto/calculador/regras.py:872`, `app/ponto/calculador/regras.py:876`)

Além disso, ausência de período **lavrado na célula** pode continuar como período geométrico do motor; o comportamento não satisfaz o “PAREI” literal do anexo `aval.txt:1754`. (`app/ponto/motor_calculo_v2.py:520`, `app/ponto/motor_calculo_v2.py:526`, `app/ponto/motor_calculo_v2.py:563`)

**5. Transbordo — implementado, mas teste não prova a ordem** [nao verificado]

Sim: ordena por **saída**, percorre em reverso, toma até o teto, chama o split com os intervalos próprios e desconta do restante. **Certeza alta.** (`app/ponto/calculador/regras.py:854`, `app/ponto/calculador/regras.py:858`, `app/ponto/calculador/regras.py:861`, `app/ponto/calculador/regras.py:864`, `app/ponto/calculador/regras.py:869`)

O C2 consome todos os 180 minutos disponíveis. Trocar `reversed(_pts)` por `_pts` preserva seu resultado: os dois períodos acabam preenchidos integralmente. Logo, esse mutante passa nos asserts do C2 e não é detectado pelo censo. **Certeza alta.** (`app/ponto/tests/test_v3_periodos_da_celula.py:158`, `app/ponto/tests/test_v3_periodos_da_celula.py:161`, `app/ponto/tests/test_v3_periodos_da_celula.py:170`, `app/ponto/tests/test_v3_periodos_da_celula.py:218`)

Um caso discriminante usa os mesmos períodos, mas apenas 120 minutos de HE: reversa ocupa 06–08 e dá zero noturno; direta ocupa 00–01 primeiro e dá 60 minutos noturnos. **Certeza alta por aplicação do algoritmo; execução desse caso: [nao verificado].** (`app/ponto/calculador/regras.py:858`, `app/ponto/calculador/regras.py:861`)

**6. Assinaturas e chamadores — certeza alta na análise estática**

O diff **não muda assinaturas formais** dessas funções; muda o contrato dos dados: `he_pontas` passa de quatro para seis componentes, `pares_por_dia` passa a conter períodos e surge a chave obrigatória `intervalos_por_dia`. Em `periodo.py`, muda apenas prosa. (`app/ponto/calculador/alimentacao.py:35`, `app/ponto/calculador/alimentacao.py:364`, `app/ponto/calculador/alimentacao.py:165`, `app/ponto/calculador/alimentacao.py:482`, `app/ponto/calculador/regras.py:572`, `app/ponto/calculador/regras.py:858`, `app/ponto/calculador/periodo.py:98`)

| API | Chamadores relevantes |
|---|---|
| `segmentos_do_motor` | Porta: `app/ponto/calculador/alimentacao.py:325`; teste: `app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:157` |
| `rubricas_por_dia` | Fechamento: `app/ponto/services/fechamento.py:1239`; instrumento: `app/ponto/management/commands/diff_calculador.py:417`. Ambos montam insumos pela porta: `app/ponto/services/fechamento.py:1237`, `app/ponto/management/commands/diff_calculador.py:312` |
| `do_dia` em produção | `app/ponto/calculador/alimentacao.py:549` |
| Funções de `periodo.py` | Chamadas internas em `app/ponto/calculador/periodo.py:742`, `app/ponto/calculador/periodo.py:752`, `app/ponto/calculador/periodo.py:784`, `app/ponto/calculador/periodo.py:820`; bateria em `app/ponto/tests/test_o280_bateria_da_lei.py:3310` |

Não encontrei chamador fornecendo a tupla antiga: os testes explícitos foram migrados para seis componentes. Chamadores externos à cópia e compatibilidade integrada: **[nao verificado]**. (`app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:304`, `app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:41`, `app/ponto/tests/test_v3_periodos_da_celula.py:158`)

**7. Classes que já reprovaram obras**

| Classe | Resultado e certeza |
|---|---|
| **Segundo juiz duplicando registrado** | **Presente na classe da pergunta, alta:** o registro atribui períodos/intervalo ao juiz da batida, enquanto o núcleo decide pausas por gaps e mantém pareamento cru próprio. Não é autoridade nova registrada pelo diff; é derivação sobrevivente. `app/core/juizes.py:1144`, `app/ponto/calculador/nucleo.py:152`, `app/ponto/calculador/nucleo.py:245` |
| **Segundo escritor fora da porta** | **Presente, preexistente, alta:** `_carimbar_segmento` declara exclusividade, mas o bloqueio escreve `efetivo_saida` diretamente. Há também escrita de rubricas noturnas pela releitura fora de `_escrever_noturnas`; o selo desse escritor examina apenas o módulo do motor. `app/ponto/motor_calculo_v2.py:1773`, `app/ponto/motor_calculo_v2.py:1421`, `app/ponto/calculador/periodo.py:753`, `app/ponto/tests/test_noturno_sitio_unico.py:100` |
| **Relógio lido fora da borda** | Não identifiquei leitura da hora atual nos cinco módulos. Os `localtime` examinados recebem timestamp explícito. **Certeza alta nesse escopo**; sistema inteiro: **[nao verificado]**. `app/ponto/calculador/alimentacao.py:126`, `app/ponto/calculador/alimentacao.py:281` |
| **Derivação paralela de fonte única** | **Presente, alta:** pausa reconstruída; dois recortadores; preservação de geometria quando a ata não responde. `app/ponto/calculador/nucleo.py:149`, `app/ponto/calculador/alimentacao.py:140`, `app/ponto/calculador/periodo.py:304`, `app/ponto/motor_calculo_v2.py:582` |
| **Selo/RED sem mordida** | **Presente parcialmente, alta:** o censo pega suas três grafias, mas deixa passar os mutantes demonstrados; C2 não distingue ordem direta/reversa; propriedade parte≤todo cobre C2 e não a pausa após 05h. `app/ponto/tests/test_v3_periodos_da_celula.py:218`, `app/ponto/tests/test_v3_periodos_da_celula.py:161`, `app/ponto/tests/test_v3_periodos_da_celula.py:174` |
| **Ordem de trava** | Não demonstrada como defeito do diff. Ordem efetiva de locks, commit, deploy e aplicação em produção: **[nao verificado]**. | [nao verificado]
| **Pouso antes do pré-requisito** | **Pré-requisito não garantido, alta:** origem na célula não é exigida; sem dia/carimbo só conta e segue. O fechamento ainda captura falha da lavra e continua. Pouso efetivamente ocorrido: **[nao verificado]**. `app/ponto/motor_calculo_v2.py:526`, `app/ponto/calculador/alimentacao.py:82`, `app/ponto/calculador/alimentacao.py:94`, `app/ponto/services/fechamento.py:1265` |
| **Fallback** | **Presente, alta:** geometria sobrevive sem célula/veredito utilizável; template do chamador sobrevive à falha do vínculo; intervalo ausente vira vazio; falha ao decidir trabalho vira `True`. `app/ponto/motor_calculo_v2.py:526`, `app/ponto/motor_calculo_v2.py:582`, `app/ponto/calculador/alimentacao.py:520`, `app/ponto/calculador/alimentacao.py:531`, `app/ponto/calculador/alimentacao.py:107`, `app/ponto/calculador/alimentacao.py:475` |

## 31/10 17:55 · sessao 11dd9efae639 · - · - · origem=Ronald (tmux)

Certeza **alta** = leitura direta do código; efeitos de concorrência são **inferências estáticas**; execução dos selos e estado real de produção **[não verificado]**.

- **Vocabulário:** há uma regra única em `bin/padrao_vocab.py:34`, alimentada por **dois documentos**, `GLOSSARIO.md` e `PLANO-PADROES.md` (`bin/padrao_vocab.py:10`, `bin/padrao_vocab.py:41`, `bin/padrao_vocab.py:48`). O selo usa essa função (`bin/tests/test_lei_akita.sh:85`, `bin/tests/test_lei_akita.sh:89`); o disjuntor também (`bin/disjuntor.py:1322`, `bin/disjuntor.py:1327`). **Não é garantida a mesma lista:** o selo lê `git show ALVO:arquivo` (`bin/tests/test_lei_akita.sh:75`, `bin/tests/test_lei_akita.sh:77`); o disjuntor lê os arquivos do disco da raiz (`bin/disjuntor.py:1325`). Igualdade dos conteúdos em produção **[não verificado]**.
- **Cópias/regex:** existe um subconjunto literal de termos esperados no MORDE (`bin/tests/test_lei_akita.sh:163`) e fixtures próprias no selo PDC (`bin/tests/test_padrao_de_cima.sh:74`, `bin/tests/test_padrao_de_cima.sh:79`); não são outro produtor do vocabulário. O selo tem parser próprio do campo da mensagem (`bin/tests/test_lei_akita.sh:99`); o PDC tem parser próprio dos quatro campos do RELATO (`bin/disjuntor.py:1130`, `bin/disjuntor.py:1311`), mas ambos delegam a comparação do termo à regra compartilhada (`bin/tests/test_lei_akita.sh:110`, `bin/disjuntor.py:1315`). Não localizei outra extração de GLOSSARIO/PLANO na busca textual; ausência absoluta de implementação equivalente **[não verificado]**.
- **Quem escreve os documentos:** a regra autoriza padrão novo só por corte e proposta do Code no topo (`CLAUDE.md:65`, `CLAUDE.md:70`, `app/docs/PLANO-PADROES.md:49`). Os leitores novos não escrevem esses documentos (`bin/padrao_vocab.py:34`, `bin/disjuntor.py:1324`, `bin/tests/test_lei_akita.sh:76`). Identidade de quem efetivamente escreveu cada linha dos documentos **[não verificado]**.

**RELATO — escritores e leitores**

- As linhas de negativa saem de `relato_negacao` (`bin/disjuntor.py:198`), por `no_relato` (`bin/disjuntor.py:234`, `bin/disjuntor.py:236`). A porta toma flock do diretório (`app/core/esteira_vigia.py:844`, `app/core/esteira_vigia.py:848`) e grava temporário com replace (`app/core/esteira_vigia.py:879`, `app/core/esteira_vigia.py:884`). A inserção precede a primeira seção e preserva o topo (`app/core/esteira_vigia.py:869`, `app/core/esteira_vigia.py:872`).
- **Há escritores fora dessa porta**, explicitamente declarados como resíduo (`app/core/esteira_vigia.py:839`): `gerar_cortes` lê e regrava o arquivo inteiro (`bin/gerar_cortes.py:117`, `bin/gerar_cortes.py:136`); `deploy_agendado` também (`bin/deploy_agendado.sh:37`, `bin/deploy_agendado.sh:44`). **Inferência alta:** podem sobrescrever atualização concorrente da porta, pois não tomam seu flock nesses trechos.
- A regra de duas linhas úteis é `hook_stop_fila1._bloco_do_topo` (`bin/hook_stop_fila1.py:415`, `bin/hook_stop_fila1.py:429`, `bin/hook_stop_fila1.py:439`). PAREI e ESPERANDO usam `_linha_do_topo` (`bin/hook_stop_fila1.py:400`, `bin/hook_stop_fila1.py:407`, `bin/hook_stop_fila1.py:1104`). O B3 delega o recorte (`bin/disjuntor.py:676`, `bin/disjuntor.py:690`); o PDC usa corpo e topo dos mesmos bytes (`bin/disjuntor.py:1351`, `bin/disjuntor.py:1359`).
- Outros leitores: checador de prova no Stop (`bin/hook_stop_fila1.py:1176`); handoff lê todo o bloco até `##` com regra própria para PAREI/lei/! (`bin/handoff_sessao.sh:92`, `bin/handoff_sessao.sh:102`, `bin/handoff_sessao.sh:108`); publicação lê RELATO e BACKLOG (`bin/relato.sh:56`, `bin/relato.sh:104`, `bin/relato.sh:111`).
- Os hooks da cópia apontam para **caminhos absolutos da árvore viva**, Stop e PreToolUse (`.claude/settings.json:9`, `.claude/settings.json:68`); executar esta cópia não prova que aqueles hooks executem o hash auditado **[não verificado]**.

**BACKLOG / PROGRAMAS**

- `PROGRAMAS` é cadastro manual no código (`bin/disjuntor.py:1110`), consultado somente quando nenhuma entidade foi resolvida (`bin/disjuntor.py:1282`, `bin/disjuntor.py:1283`). O runtime PDC **não consulta BACKLOG** nesse caminho.
- O selo lê BACKLOG e valida só que cada chave do cadastro aparece e que o dict não está vazio (`bin/tests/test_padrao_de_cima.sh:328`, `bin/tests/test_padrao_de_cima.sh:333`, `bin/tests/test_padrao_de_cima.sh:334`). Não valida os **valores** obra→programa nem se toda obra que deveria estar cadastrada está presente.
- **Certeza alta:** apagar a linha do BACKLOG não muda o cadastro carregado no runtime (`bin/disjuntor.py:1110`, `bin/disjuntor.py:1282`); o selo reprova aquela divergência (`bin/tests/test_padrao_de_cima.sh:333`). Uma obra fora do cadastro e sem modelo resulta em `[]`, portanto não agrupa (`bin/disjuntor.py:1280`, `bin/disjuntor.py:1284`).
- BACKLOG é escrito pelo gerador, que preserva o bloco OBRAS escrito à mão (`bin/gerar_backlog.py:103`, `bin/gerar_backlog.py:111`, `bin/gerar_backlog.py:119`) e publica por replace (`bin/gerar_backlog.py:138`, `bin/gerar_backlog.py:140`). Há outro escritor de célula de estado (`bin/backlog_estado.py:31`, `bin/backlog_estado.py:58`, `bin/backlog_estado.py:59`).
- Leitores fora do diff: Stop, por `_backlog_texto`, lista de paradas e travas (`bin/hook_stop_fila1.py:170`, `bin/hook_stop_fila1.py:317`, `bin/hook_stop_fila1.py:351`); resumo publicado por `gerar_backlog_topo` (`bin/gerar_backlog_topo.py:12`, `bin/gerar_backlog_topo.py:16`).

**Chave**

- O leitor central é `desligado`, usando `lexists`, inclusive symlink (`bin/disjuntor.py:151`, `bin/disjuntor.py:153`). O escritor autorizado é o terminal do Ronald segundo configuração (`bin/disjuntor_settings.json:3`); o PreToolUse tenta impedir escrita pelo Code (`bin/disjuntor.py:167`, `bin/disjuntor.py:171`). Existência atual da chave **[não verificado]**.
- Stop testa a chave antes de chamar B3 (`bin/hook_stop_fila1.py:1039`, `bin/hook_stop_fila1.py:1048`). Fixtures escrevem a chave nos selos (`bin/tests/test_padrao_de_cima.sh:303`, `bin/tests/test_padrao_de_cima.sh:313`).

**Riscos adicionais para consolidar**

1. **TOCTOU nos modelos — inferência alta:** o censo lê models (`bin/disjuntor.py:1216`, `app/core/censo_escritas.py:105`), `_so_modelos` relê os mesmos arquivos (`bin/disjuntor.py:1218`, `bin/disjuntor.py:1232`), e o arquivo citado pode ser lido depois (`bin/disjuntor.py:1258`). Não há uma foto comum equivalente à do RELATO.
2. **TOCTOU no vocabulário — inferência alta:** GLOSSARIO e PLANO são lidos sequencialmente sem snapshot conjunto (`bin/padrao_vocab.py:41`, `bin/padrao_vocab.py:48`).
3. **Liberação prévia na primeira observação — certeza alta sobre comportamento:** se a primeira leitura já contém dois desenhos e uma linha válida de liberação, o mesmo ciclo dispara e libera (`bin/disjuntor.py:1374`, `bin/disjuntor.py:1381`, `bin/disjuntor.py:1397`, `bin/disjuntor.py:1401`). O mtime foi descartado (`bin/disjuntor.py:1351`). O teste “liberação antes” cobre apenas uma chamada prévia com um desenho, consumindo a linha antes do segundo (`bin/tests/test_padrao_de_cima.sh:195`, `bin/tests/test_padrao_de_cima.sh:198`).
4. **Falha de resolver entidade vira ausência de sinal persistida:** qualquer exceção vira `ents=[]` e o sha fica registrado (`bin/disjuntor.py:1369`, `bin/disjuntor.py:1373`); só sha novo é resolvido (`bin/disjuntor.py:1366`). **Inferência alta:** reparar o censo não reclassifica o evento já gravado, mantendo verde para evento sem entidade.
5. **Juiz duplicado registrado:** não identifiquei duplicação de autoridade de negócio; o PDC chama o censo existente (`bin/disjuntor.py:1197`, `bin/disjuntor.py:1216`). A filtragem de subclasse é regra nova de reconhecimento (`bin/disjuntor.py:1222`, `bin/disjuntor.py:1242`), enquanto `censo_escritas.modelos` declara coletar toda classe de topo com bases (`app/core/censo_escritas.py:90`, `app/core/censo_escritas.py:109`). Ausência absoluta de juiz equivalente registrado **[não verificado]**.

## 31/10 17:55 · sessao 5bfc595274a8 · - · - · origem=Ronald (tmux)

Achados para consolidação, todos por leitura estática; ocorrência em produção [não verificado].

| Item | Resultado e certeza | Evidência |
|---|---|---|
| **Verde com RELATO ilegível** | **Alta.** `decide_pdc` informa `nao_sei`, mas `estado_pdc` ignora esse atributo e retorna LIVRE quando a obra não aparece nas disparadas; estado válido vazio + RELATO ilegível libera. | `bin/disjuntor.py:1423`, `bin/disjuntor.py:1474`, `bin/disjuntor.py:1477` |
| **Escopo pode regredir** | **Alta, inferência de concorrência.** A trava do estado é liberada antes da gravação do escopo. A decide primeiro e pausa; B decide depois e publica escopo novo; A sobrescreve com escopo antigo. Se o estado ficar ilegível, a obra omitida pelo escopo antigo fica LIVRE. | `bin/disjuntor.py:293`, `bin/disjuntor.py:295`, `bin/disjuntor.py:1416`, `bin/disjuntor.py:1421`, `bin/disjuntor.py:1461`, `bin/disjuntor.py:1465` |
| **LIBERA anterior sem observação intermediária** | **Alta.** Primeira decisão com dois desenhos e LIBERA já no topo dispara e libera na mesma transição. Não há fotografia/mtime como no B3. O selo cobre LIBERA previamente consumida com um desenho; não cobre esse primeiro encontro. | `bin/disjuntor.py:1374`, `bin/disjuntor.py:1381`, `bin/disjuntor.py:1396`, `bin/disjuntor.py:1401`, `bin/tests/test_padrao_de_cima.sh:195`, `bin/tests/test_padrao_de_cima.sh:197` |
| **Falha do censo vira entidade vazia permanente** | **Alta.** Exceção produz `ents=[]`, grava o evento e não tenta resolver novamente porque SHA já existe. Não agrupa e pode liberar por falta de entidade. | `bin/disjuntor.py:1366`, `bin/disjuntor.py:1369`, `bin/disjuntor.py:1373`, `bin/disjuntor.py:1298` |
| **BUG 6** | **Alta.** Curado na porta `b3_parada`. Outros chamadores de produção encontrados passam pelo desligamento do PreToolUse ou do Stop. As funções internas de julgamento continuam sem guarda própria, mas não encontrei outro chamador produtivo desprotegido. | `bin/disjuntor.py:1070`, `bin/disjuntor.py:1979`, `bin/hook_stop_fila1.py:1039`, `bin/hook_stop_fila1.py:1048`, `bin/disjuntor.py:1889`, `bin/disjuntor.py:1930`, `bin/disjuntor.py:1944` |
| **RELATO corpo/topo** | **Alta.** Mesma foto no PDC. As duas leituras físicas são intencionais e comparadas; o topo recorta os bytes aceitos. | `bin/disjuntor.py:709`, `bin/disjuntor.py:713`, `bin/disjuntor.py:1351`, `bin/disjuntor.py:1359` |
| **TOCTOU dos modelos** | **Alta, condicional a alteração concorrente.** O censo lê os modelos e `_so_modelos` relê os mesmos arquivos sem foto comum. Pode combinar campos/classes da versão A com bases da versão B. | `app/core/censo_escritas.py:105`, `bin/disjuntor.py:1216`, `bin/disjuntor.py:1218`, `bin/disjuntor.py:1232` |
| **Relógio fora da borda** | **Alta.** Hooks fornecem `agora`; caminhos CLI deixam as decisões consultarem `time.time()` internamente. Trilha também consulta relógio próprio. | `bin/disjuntor.py:1977`, `bin/disjuntor.py:1949`, `bin/disjuntor.py:1506`, `bin/disjuntor.py:1344`, `bin/disjuntor.py:903`, `bin/disjuntor.py:140` |
| **Ordem das travas** | **Alta.** Wrapper: slot → resposta → estado. Triagem: resposta compartilhada → estado. Não encontrei inversão nesse percurso; escopo fora da trava é o problema concreto. | `bin/codex_pergunta.sh:145`, `bin/codex_pergunta.sh:163`, `bin/codex_pergunta.sh:165`, `bin/disjuntor.py:812`, `bin/disjuntor.py:844`, `bin/disjuntor.py:260` |
| **Codex rc 75** | **Alta.** Identifica limite/indisponibilidade em erro, registra `limite.log`, manda seguir por sonnet e retorna 75. Não emite sidecar válido dessa chamada; automatização efetiva da continuação [não verificado]. | `bin/codex_pergunta.sh:186`, `bin/codex_pergunta.sh:191`, `bin/codex_pergunta.sh:192`, `bin/codex_pergunta.sh:194`, `bin/codex_pergunta.sh:201` |
| **argv sensível** | **Alta quanto à forma.** Novas chamadas passam somente obra em argv; pergunta/anexos seguem pelo stdin. `CODEX_OBRA` só é validado como não vazio, portanto conteúdo sensível nesse identificador não é impedido; presença efetiva [não verificado]. | `bin/codex_pergunta.sh:44`, `bin/codex_pergunta.sh:45`, `bin/codex_pergunta.sh:165`, `bin/codex_pergunta.sh:168`, `bin/codex_pergunta.sh:178` |

GLOSSÁRIO ausente impede LIBERA quando o topo tenta usá-lo; sem candidata a LIBERA, o vocabulário nem é consultado, portanto ausência não é recusa global. RELATO legível vazio produz topo `[]`, não erro: não libera disparadas, mas também não cria eventos. BACKLOG não é lido pelo resolvedor produtivo: ele usa `PROGRAMAS` literal; o selo só verifica presença das chaves. **Certeza alta.** `bin/disjuntor.py:1384`, `bin/disjuntor.py:1388`, `bin/disjuntor.py:1390`, `bin/hook_stop_fila1.py:443`, `bin/disjuntor.py:1110`, `bin/disjuntor.py:1282`, `bin/tests/test_padrao_de_cima.sh:321`.

Os hooks configurados apontam para a árvore viva absoluta; execução/configuração efetiva de produção [não verificado]. `.claude/settings.json:9`, `.claude/settings.json:68`.

## 31/10 17:55 · sessao 2358df259c1e · - · - · origem=Ronald (tmux)

Certeza **alta por leitura estática**; não rodei os selos porque criam árvores temporárias e arquivos (`bin/tests/test_padrao_de_cima.sh:29`, `bin/tests/test_padrao_de_cima.sh:36`, `bin/tests/test_lei_akita.sh:168`).

- **PADRAO-DE-CIMA tem mordida útil:** exige recusas e liberações pelas portas reais; possui controles sem mutação e mutantes para gate, agrupamento, vocabulário, janela e desligamento (`bin/tests/test_padrao_de_cima.sh:110`, `bin/tests/test_padrao_de_cima.sh:285`, `bin/tests/test_padrao_de_cima.sh:377`, `bin/tests/test_padrao_de_cima.sh:380`).
- **PROGRAMAS é selo parcial:** verifica somente chaves existentes no BACKLOG e lista não vazia; não verifica valores nem completude (`bin/tests/test_padrao_de_cima.sh:332`). Remover a chave `O261-E-CELULA-20-UM-DONO`, ou trocar somente seu destino, escapa por inspeção: os cenários comportamentais usam apenas duas E4 (`bin/disjuntor.py:1113`, `bin/tests/test_padrao_de_cima.sh:157`).
- **Atomicidade do escopo não está demonstrada:** caso verifica conteúdo final; não observa escrita concorrente. Substituir `os.replace` por escrita direta pode passar (`bin/tests/test_padrao_de_cima.sh:118`, `bin/disjuntor.py:1437`). Certeza **média sobre sobrevivência sem executar**.
- **Faltam casos de ausência:** fixtures sempre fornecem GLOSSARIO/PLANO; RELATO ausente só é testado com estado já corrompido (`bin/tests/test_padrao_de_cima.sh:74`, `bin/tests/test_padrao_de_cima.sh:250`, `bin/tests/test_padrao_de_cima.sh:257`). Isso deixa sem cobertura o descarte de `d.nao_sei` com estado legível (`bin/disjuntor.py:1423`, `bin/disjuntor.py:1474`).
- **Mutante inválido no test_disjuntor:** `B3-foto-parcial-engolida` injeta `H._bloco_do_topo()` numa função sem `H` local/global; os imports existentes são locais a outras funções (`bin/tests/test_disjuntor.sh:1663`, `bin/disjuntor.py:669`, `bin/disjuntor.py:688`, `bin/disjuntor.py:702`). Reprodução isolada em memória produziu `NameError`; harness conta qualquer falha como morte (`bin/tests/test_disjuntor.sh:1491`). Certeza **alta, confirmada em memória**.
- **Outro mutante morre por exceção:** remover a guarda de entidade não disparada conduz a `disp[ent]` inexistente no primeiro cenário de LIBERA antecipada (`bin/tests/test_padrao_de_cima.sh:195`, `bin/tests/test_padrao_de_cima.sh:397`, `bin/disjuntor.py:1402`). Harness também aceita esse crash como mordida (`bin/tests/test_padrao_de_cima.sh:375`). Certeza **alta por fluxo estático**.
- **LEI-AKITA morde lista vazia e inversão do predicado:** exige termos conhecidos, casos positivos e negativos e presença do marcador final (`bin/tests/test_lei_akita.sh:163`, `bin/tests/test_lei_akita.sh:181`, `bin/tests/test_lei_akita.sh:298`, `bin/tests/test_lei_akita.sh:302`). Porém um fallback em `VocabIlegivel` não é exercitado pelos casos: nenhuma fonte é removida; trocar a exceção por vocabulário parcial poderia sobreviver em condições saudáveis (`bin/tests/test_lei_akita.sh:87`, `bin/tests/test_lei_akita.sh:172`).
- **Não são selos órfãos no código:** a régua percorre `tests/test_*.sh`; LEI-AKITA tem chamada explícita no pre-push (`bin/regua.sh:138`, `bin/pre-push.sh:38`). Execução efetiva em produção: **[não verificado]**.

## 31/10 17:54 · sessao 7bc4e88cb9e1 · - · - · origem=Ronald (tmux)

**Achados certos, por leitura estática e execução em memória de funções da cópia:**

- **Sobra pausa reconstruída pelas pontas no caminho de produção.** `minutos_do_oraculo` recebe os segmentos em `app/ponto/calculador/alimentacao.py:478`; calcula pausa entre fim anterior e próxima entrada em `app/ponto/calculador/nucleo.py:149`; essa pausa alimenta as rubricas em `app/ponto/calculador/alimentacao.py:550`. Para períodos 00–01 + 06–08, executei a função real: 180 minutos trabalhados, **300 minutos de pausa** e envelope 480; esses 300 minutos não vêm de intervalos julgados. Certeza alta; origens: `app/ponto/calculador/nucleo.py:152`, `app/ponto/calculador/nucleo.py:153`.

- **Existe também o ramo completo de reconstrução por batidas cruas.** Quando `pares is None`, o núcleo identifica turnos pelo gap da última batida (`app/ponto/calculador/nucleo.py:188`), pelo vão desde a primeira (`app/ponto/calculador/nucleo.py:215`), aplica cortes próprios (`app/ponto/calculador/nucleo.py:216`), toma a data da primeira (`app/ponto/calculador/nucleo.py:237`), envelope última menos primeira (`app/ponto/calculador/nucleo.py:239`), pareia sequencialmente (`app/ponto/calculador/nucleo.py:245`) e calcula pausas entre pontas (`app/ponto/calculador/nucleo.py:253`). A porta de produção examinada passa lista, portanto esse ramo não é acionado por essa chamada específica (`app/ponto/calculador/alimentacao.py:478`). Certeza alta.

- **O ramo cru ainda decide se um gap pertence ao envelope pelo ponto médio e pelas datas de suas pontas.** `app/ponto/calculador/nucleo.py:180`, `app/ponto/calculador/nucleo.py:183`. Certeza alta.

- **`periodo.py` também volta a calcular pausa/envelope pelos segmentos.** Recorta o período com intervalos julgados (`app/ponto/calculador/periodo.py:304`), chama o núcleo (`app/ponto/calculador/periodo.py:355`) e lê a pausa e o envelope calculados (`app/ponto/calculador/periodo.py:357`, `app/ponto/calculador/periodo.py:358`). Diferentemente do ramo cru, recebe a geometria lavrada; ainda há cálculo da pausa pelos segmentos. Certeza alta.

- **A pausa muda conforme o desconto do motor.** A porta só recorta intervalos quando `intra_descontada > 0` (`app/ponto/calculador/alimentacao.py:134`, `app/ponto/calculador/alimentacao.py:136`). Com intervalo julgado presente e desconto zero, o núcleo recebe um segmento inteiro, portanto não vê a pausa, enquanto `do_dia` recebe os intervalos julgados separadamente (`app/ponto/calculador/alimentacao.py:144`, `app/ponto/calculador/alimentacao.py:550`, `app/ponto/calculador/alimentacao.py:562`). Certeza alta sobre o fluxo.

**Censo AST: morde três grafias, não a classe do defeito.**

Executei as funções AST do próprio teste em memória, sem importar Django: os cinco módulos retornam conjunto vazio. Os seguintes mutantes também retornam conjunto vazio, por limitações explícitas de `app/ponto/tests/test_v3_periodos_da_celula.py:189`, `app/ponto/tests/test_v3_periodos_da_celula.py:195`, `app/ponto/tests/test_v3_periodos_da_celula.py:220`, `app/ponto/tests/test_v3_periodos_da_celula.py:222`:

```python
vazio = ()
he(entrada=p.e, saida=p.s, intervalos=vazio)
```

```python
e = pts[0][0]
s = pts[-1][1]
he(entrada=e, saida=s, intervalos=ivs)
```

```python
he(entrada=min(ts), saida=max(ts), intervalos=ivs)
```

```python
he(pts[0][0], pts[-1][1], ivs)
```

A pausa calculada com variável `anterior` também passa; o próprio código existente constitui testemunha desse falso negativo (`app/ponto/calculador/nucleo.py:149`, `app/ponto/calculador/nucleo.py:152`). Além disso, o universo usa `glob('*.py')`, sem recursão (`app/ponto/tests/test_v3_periodos_da_celula.py:235`). Certeza alta.

**Classes do item 7:**

- **Derivação paralela/fonte única:** ocorrência concreta na pausa da produção acima; AST verde apesar dela (`app/ponto/calculador/nucleo.py:152`, `app/ponto/calculador/alimentacao.py:550`, `app/ponto/tests/test_v3_periodos_da_celula.py:220`). Certeza alta.
- **Fallback:** mantém template/cadastro/folga do chamador quando o juiz não responde ou levanta (`app/ponto/calculador/alimentacao.py:520`, `app/ponto/calculador/alimentacao.py:523`, `app/ponto/calculador/alimentacao.py:531`); ausência de vínculo e exceção também viram `dia_de_trabalho=True` (`app/ponto/calculador/alimentacao.py:470`, `app/ponto/calculador/alimentacao.py:475`). Certeza alta.
- **Ausência de intervalo vira vazio:** `getattr(..., ()) or ()` em `app/ponto/calculador/alimentacao.py:107`. Não distingue “motor declarou nenhum intervalo” de “campo ausente”. Certeza alta.
- **Pouso antes do pré-requisito/PAREI:** período sem dia ou sem carimbo apenas incrementa contador e segue (`app/ponto/calculador/alimentacao.py:82`, `app/ponto/calculador/alimentacao.py:87`). Já ausência dos campos de períodos efetivamente levanta `ResultadoSemPeriodos` (`app/ponto/calculador/nucleo.py:393`). A equivalência disso com “período que a célula não guarda” precisa ser estabelecida pelo pai com a fonte da célula; [nao verificado].
- **Segundo escritor:** em `calculador`, não encontrei escrita de `efetivo_entrada`, `efetivo_saida` ou `intervalos_do_motor`; há escrita de rubricas nos objetos pela releitura (`app/ponto/calculador/periodo.py:743`, `app/ponto/calculador/periodo.py:753`, `app/ponto/calculador/periodo.py:839`). A irregularidade como “fora da porta” depende de seu contrato; [nao verificado].
- **Relógio fora da borda/ordem de trava:** não encontrei chamadas de relógio atual, transação ou lock nos cinco módulos; as chamadas `localtime` recebem instante explícito (`app/ponto/calculador/alimentacao.py:126`, `app/ponto/calculador/alimentacao.py:281`). Ordem de travas dos chamadores externos: [nao verificado].
- **Segundo juiz registrado:** a pausa reconstruída está provada, mas seu enquadramento contra pergunta específica registrada em `core/juizes.py` depende da inspeção do registro; [nao verificado].

## 31/10 17:54 · sessao 8d16ad74d24b · - · - · origem=Ronald (tmux)

**Autoridade e origem — certeza alta.** `JUIZES.md` é lookup e manda consultar o registro vivo (`app/docs/JUIZES.md:7`, `app/docs/JUIZES.md:14`). Quem responde “que períodos e que intervalo teve este dia?” é `ponto/juiz_batida.py::periodos_do_dia`; quem responde quanto vale o PAR é `MotorBase.calcular_periodo` (`app/core/juizes.py:1143`, `app/core/juizes.py:1144`, `app/core/juizes.py:1145`). `periodos_do_resultado` é leitor dos objetos publicados pelo motor, e `_carimbar_segmento` copia a geometria efetivamente usada; nenhum deles substitui o juiz do dia (`app/ponto/calculador/nucleo.py:393`, `app/ponto/calculador/nucleo.py:400`, `app/ponto/motor_calculo_v2.py:1772`, `app/ponto/motor_calculo_v2.py:1778`).

**O v3 lê a mesma fonte do espelho, mas a proveniência da célula é condicional — certeza alta.** Espelho percorre `resultado_v2.periodos + periodos_ft` (`app/ponto/services/espelho.py:432`). A porta v3 percorre ambos via `periodos_do_resultado`, lendo carimbo efetivo e intervalos (`app/ponto/calculador/alimentacao.py:79`, `app/ponto/calculador/alimentacao.py:86`, `app/ponto/calculador/alimentacao.py:107`). O motor começa com geometria e só a substitui pelo veredito da célula quando as guardas permitem (`app/ponto/motor_calculo_v2.py:480`, `app/ponto/motor_calculo_v2.py:566`, `app/ponto/motor_calculo_v2.py:635`, `app/ponto/motor_calculo_v2.py:638`).

**A condição “período que a célula não guarda = PAREI” não está garantida por essa cadeia — certeza alta sobre o código; aplicação jurídica do aval cabe ao principal.** Sem células alimentadas, retorna o trio geométrico; turno partido também retorna esse trio; sem célula para o dia, juiz levantando, ata desalinhada/aberta ou luz sem batida, conserva a geometria anterior (`app/ponto/motor_calculo_v2.py:509`, `app/ponto/motor_calculo_v2.py:520`, `app/ponto/motor_calculo_v2.py:523`, `app/ponto/motor_calculo_v2.py:526`, `app/ponto/motor_calculo_v2.py:563`, `app/ponto/motor_calculo_v2.py:567`, `app/ponto/motor_calculo_v2.py:582`, `app/ponto/motor_calculo_v2.py:591`). A porta só distingue atributo de resultado ausente/`None` de lista vazia; não verifica origem na célula (`app/ponto/calculador/nucleo.py:393`, `app/ponto/calculador/nucleo.py:397`). Sem mapa de jornada ou sem carimbo fechado, conta e pula em vez de parar (`app/ponto/calculador/alimentacao.py:81`, `app/ponto/calculador/alimentacao.py:94`).

| Campo/fluxo | Quem escreve | Quem lê |
|---|---|---|
| `resultado.periodos` / `periodos_ft` | Criação por `calcular_periodo`, publicação nas listas (`app/ponto/motor_calculo_v2.py:2396`, `app/ponto/motor_calculo_v2.py:2430`, `app/ponto/motor_calculo_v2.py:2432`) | Espelho (`app/ponto/services/espelho.py:432`); porta única (`app/ponto/calculador/nucleo.py:400`); v3 alimentação (`app/ponto/calculador/alimentacao.py:79`); releitura (`app/ponto/calculador/periodo.py:607`) |
| `efetivo_entrada`, `efetivo_saida`, `intervalos_do_motor` | Carimbo (`app/ponto/motor_calculo_v2.py:1778`, `app/ponto/motor_calculo_v2.py:1779`, `app/ponto/motor_calculo_v2.py:1780`); **também `efetivo_saida` no bloqueio** (`app/ponto/motor_calculo_v2.py:1421`) | Porta (`app/ponto/calculador/alimentacao.py:86`, `app/ponto/calculador/alimentacao.py:107`); releitura (`app/ponto/calculador/periodo.py:645`, `app/ponto/calculador/periodo.py:742`, `app/ponto/calculador/periodo.py:752`); motor HE noturna lê intervalos (`app/ponto/motor_calculo_v2.py:956`) |
| `dia_da_jornada` | Exposição/mescla do mapa do motor (`app/ponto/motor_calculo_v2.py:1011`, `app/ponto/motor_calculo_v2.py:1013`, `app/ponto/motor_calculo_v2.py:2897`) | Porta (`app/ponto/calculador/alimentacao.py:76`, `app/ponto/calculador/alimentacao.py:81`); releitura (`app/ponto/calculador/periodo.py:594`) |
| `pares` segmentos | Porta (`app/ponto/calculador/alimentacao.py:140`, `app/ponto/calculador/alimentacao.py:144`, `app/ponto/calculador/alimentacao.py:164`) | Núcleo, chamado pela porta (`app/ponto/calculador/alimentacao.py:478`); soma (`app/ponto/calculador/nucleo.py:147`) |
| `pares_por_dia` períodos | Porta (`app/ponto/calculador/alimentacao.py:165`) | `rubricas_por_dia` (`app/ponto/calculador/alimentacao.py:481`, `app/ponto/calculador/alimentacao.py:560`); noturnas (`app/ponto/calculador/regras.py:885`, `app/ponto/calculador/regras.py:887`) |
| `intervalos_por_dia` | Porta (`app/ponto/calculador/alimentacao.py:166`) | Porta de rubricas (`app/ponto/calculador/alimentacao.py:482`, `app/ponto/calculador/alimentacao.py:562`); cálculo noturno (`app/ponto/calculador/regras.py:887`, `app/ponto/calculador/regras.py:86`) |
| `he_pontas_por_dia`, agora 6 componentes | Porta cria geometria+intervalos e atualiza teto pela soma dos segmentos (`app/ponto/calculador/alimentacao.py:115`, `app/ponto/calculador/alimentacao.py:168`, `app/ponto/calculador/alimentacao.py:169`) | Porta (`app/ponto/calculador/alimentacao.py:488`, `app/ponto/calculador/alimentacao.py:561`); split (`app/ponto/calculador/regras.py:854`, `app/ponto/calculador/regras.py:858`) |
| `pausa_min` | **Continua derivada do gap entre segmentos**, em `Veredito.pausas` (`app/ponto/calculador/nucleo.py:149`, `app/ponto/calculador/nucleo.py:152`) | Porta transmite (`app/ponto/calculador/alimentacao.py:550`); regras lê na base do piso (`app/ponto/calculador/regras.py:749`) |
| HE100 noturna e HE50 noturna | `he_noturna` emprestada do motor é chamada por período; valores do dia escritos pelo v3 (`app/ponto/calculador/regras.py:846`, `app/ponto/calculador/regras.py:864`, `app/ponto/calculador/regras.py:874`, `app/ponto/calculador/regras.py:876`) | Retorno de rubricas armazenado em `out[k]` (`app/ponto/calculador/alimentacao.py:549`); demais consumidores completos: [nao verificado] |
| Transbordo | Ordenação pela saída, iteração reversa, teto e decremento (`app/ponto/calculador/regras.py:854`, `app/ponto/calculador/regras.py:858`, `app/ponto/calculador/regras.py:861`, `app/ponto/calculador/regras.py:869`) | Cada parcela vai a `he_noturna` com intervalos próprios (`app/ponto/calculador/regras.py:864`, `app/ponto/calculador/regras.py:866`) |
| `he_sem_periodo_min` | Detalhe recebe saldo que não coube (`app/ponto/calculador/regras.py:872`) | Consumidor específico desse detalhe: [nao verificado] |

**Segundo escritor — certeza alta, preexistente e fora do diff.** `_carimbar_segmento` se declara escritor único dos três campos (`app/ponto/motor_calculo_v2.py:1773`), porém `_dia_sem_previsto_fora_da_janela` atribui diretamente `efetivo_saida = efetivo_entrada` (`app/ponto/motor_calculo_v2.py:1420`, `app/ponto/motor_calculo_v2.py:1421`), chamado após o cálculo do período (`app/ponto/motor_calculo_v2.py:2399`). É exceção de bloqueio explicitamente documentada; não prova, por si, erro monetário novo (`app/ponto/motor_calculo_v2.py:1415`).

**Derivação paralela/pausa — certeza alta.** `periodo.py` possui outro recortador de períodos, `segmentos_do_par`, além da porta; depois recupera pausa pelos gaps do núcleo (`app/ponto/calculador/periodo.py:290`, `app/ponto/calculador/periodo.py:303`, `app/ponto/calculador/periodo.py:355`, `app/ponto/calculador/periodo.py:357`). A releitura reescreve minutos trabalhados, intra e noturnas nos próprios objetos (`app/ponto/calculador/periodo.py:743`, `app/ponto/calculador/periodo.py:744`, `app/ponto/calculador/periodo.py:753`, `app/ponto/calculador/periodo.py:754`).

**Fallback — certeza alta.** Além da cadeia geométrica acima, se o vínculo do dia não existe ou levanta, `rubricas_por_dia` conserva o template declarado pelo chamador (`app/ponto/calculador/alimentacao.py:520`, `app/ponto/calculador/alimentacao.py:523`, `app/ponto/calculador/alimentacao.py:531`, `app/ponto/calculador/alimentacao.py:538`). O comentário chamar isso de “não é fallback” não altera o fluxo observado (`app/ponto/calculador/alimentacao.py:517`).

## 31/10 17:54 · sessao 48385a33de2a · - · - · origem=Ronald (tmux)

**5. Transbordo — certeza alta.** O código ordena períodos pela saída em `app/ponto/calculador/regras.py:854`, percorre `reversed(_pts)` em `:858`, limita cada fatia pelo teto em `:861`, chama `he_noturna` com entrada, saída e intervalos daquele período em `:864` e desconta a fatia do restante em `:869`. A sobra sem período fica em `he_sem_periodo_min`, `:872`.

**O teste C2 não prova a ordem — certeza alta.** Ele distribui 180 minutos em períodos com capacidade total exatamente 180; tanto ordem direta quanto reversa consomem os dois por inteiro e produzem 1 hora noturna. Retirar `reversed` deixa os asserts iguais (`app/ponto/tests/test_v3_periodos_da_celula.py:158`, `:161`, `:169`, `:170`). Contraexemplo discriminante: mesmos períodos 00–01 e 06–08, mas HE total de 120 minutos: ordem reversa ocupa 06–08, com zero noturno; direta ocupa 00–01 e mais uma hora diurna, com 60 minutos noturnos (`app/ponto/calculador/regras.py:858`, `:861`, `:864`). Execução desse contraexemplo: [nao verificado].

**6. Assinaturas/chamadores — certeza alta na análise estática.** O diff fornecido não altera assinaturas formais: `segmentos_do_motor` continua com `resultado` (`app/ponto/calculador/alimentacao.py:35`); `rubricas_por_dia`, com os mesmos parâmetros (`:364`); `do_dia`, idem (`app/ponto/calculador/regras.py:572`); em `periodo.py`, o diff só altera comentários (`app/ponto/calculador/periodo.py:98`, `:505`). A mudança incompatível é **o contrato dos dados**: nova chave obrigatória `intervalos_por_dia` (`app/ponto/calculador/alimentacao.py:482`), `pares_por_dia` contendo períodos (`:165`) e `he_pontas` passando de quatro para seis elementos, desempacotados em `app/ponto/calculador/regras.py:858`.

Chamadores encontrados:

| API | Chamadores |
|---|---|
| `segmentos_do_motor` | Porta `app/ponto/calculador/alimentacao.py:325`; teste `app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:157`. |
| `rubricas_por_dia` em produção/instrumento | `app/ponto/services/fechamento.py:1239`, com insumos criados em `:1237`; `app/ponto/management/commands/diff_calculador.py:417`, com insumos criados em `:312`. |
| `rubricas_por_dia` em testes | `test_s5b_vinculo_do_dia_efeito.py:112`; `test_he100_dia_sem_previsto.py:272`; `test_v3_dobra_do_ciclo_do_dia.py:94`, `:178`; `test_v3_dobra_noturna_e_cct_do_dia.py:172`, `:361`; `test_o280_bateria_da_lei.py:2776`, `:2861`; `test_v3_periodos_da_celula.py:97`, `:101`; `test_lote_de_celulas_nao_troca_de_colaborador.py:121`, todos sob `app/ponto/tests/`. | [nao verificado]
| `regras.do_dia` em produção | `app/ponto/calculador/alimentacao.py:549`. Os demais usos encontrados são testes; entradas omitidas continuam opcionais e geram `SEM_ENTRADA`, conforme `app/ponto/calculador/regras.py:583`, `:610`, `:627`. |
| `periodo.py` | `reler` tem chamadas na bateria O280, por exemplo `app/ponto/tests/test_o280_bateria_da_lei.py:3310`, `:3712`, `:4051`, `:4382`, `:5181`. Chamadas internas: `periodo_do_dia` em `app/ponto/calculador/periodo.py:742`, `noturno_no_par` em `:752`, `pontualidade_no_dia` em `:784`, `excesso_do_dia` em `:820`. |

**Não encontrei chamada com a tupla antiga de quatro elementos — certeza alta no código estático pesquisado.** As chamadas explícitas com `he_pontas` são a porta (`app/ponto/calculador/alimentacao.py:561`) e testes: `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:308`, cujas tuplas têm seis elementos em `:304`; `test_v3_dobra_noturna_e_cct_do_dia.py:41`, `:59`, `:102`, `:143`; `test_v3_periodos_da_celula.py:164`, cujas tuplas estão em `:158`. Chamadores externos à cópia: [nao verificado].

Outro leitor de `pares_por_dia`: o comando atribui esse mapa em `app/ponto/management/commands/diff_calculador.py:318`; sua chamada de rubricas usa integralmente os novos insumos em `:418`, sem remontar o contrato antigo. Compatibilidade executada com Django/DB: [nao verificado].

## 31/10 17:44 · sessao 7e00fa505aa6 · TRAVA-COBERTURA · leitura · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 31/10 17:42 · sessao bc4f2203bbdb · - · - · origem=Ronald (tmux)

Censo de reentrância: nos caminhos versionados examinados, nenhum ancestral identificado segura `/tmp/juliani_db_test.lock` antes do disparo; os ancestrais prendem arquivos distintos, e a trava global protege apenas o comando filho. Ancestrais externos e processos de produção são **[não verificado]**. (`bin/trava_teste.sh:35`; `bin/trava_teste.sh:42`)

| Chamador | Cadeia e lock ancestral identificado |
|---|---|
| `suite` | `suite → exec trava → docker`; não adquire flock antes do `exec`. (`bin/suite.sh:96`) |
| `regua` | `regua → trava → docker`; o próprio script segura `/tmp/regua.lock`, distinto do global. (`bin/regua.sh:16`; `bin/regua.sh:97`; `bin/regua.sh:165`) |
| `pre-push` | `git push → hook wrapper → bin/pre-push → _teste_irmao → trava`; prende `/tmp/regua.lock`, ou aceita `REGUA_TRAVA_HERDADA=1`. (`bin/hooks.sh:56`; `bin/pre-push.sh:80`; `bin/pre-push.sh:86`; `bin/pre-push.sh:142`) |
| `vigia_arvore` | Prende `/tmp/vigia_arvore.lock`; chama sequencialmente `suite → trava`, depois outra trava na confirmação viva. (`bin/vigia_arvore.sh:24`; `bin/vigia_arvore.sh:83`; `bin/vigia_arvore.sh:106`; `bin/vigia_arvore.sh:131`) |
| `regua_relogio` | Chama diretamente `trava → docker`; seu corpo anterior prepara diretório e env-file, sem flock próprio. (`bin/regua_relogio.sh:31`; `bin/regua_relogio.sh:46`; `bin/regua_relogio.sh:51`) |
| `molde_fatia` | `esteira → rodar → teste → trava`; ancestral segura `/tmp/esteira___FATIA__.lock`, distinto. (`bin/molde_fatia/esteira.sh:7`; `bin/molde_fatia/esteira.sh:15`; `bin/molde_fatia/rodar.sh:40`) |
| `molde_fatia MAPA` | Mesmo ancestral; MAPA ainda executa docker diretamente e termina antes da função `teste`. (`bin/molde_fatia/rodar.sh:28`; `bin/molde_fatia/rodar.sh:33`) |
| `molde_autocura` | `esteira → rodar → teste → docker`; ancestral segura `/tmp/esteira___FATIA__.lock`; a cadeia só começa após `rodar` retornar. (`bin/molde_autocura/esteira.sh:6`; `bin/molde_autocura/esteira.sh:14`; `bin/molde_autocura/esteira.sh:23`; `bin/molde_autocura/rodar.sh:20`) |
| `isolamento` | `cadeia → regua → trava → docker`, retorna; então `cadeia → isolar_ou_parar → _iso_roda`. A cadeia ainda segura `/tmp/regua.lock`, mas a trava do teste anterior já retornou. (`bin/molde_fatia/cadeia.sh:14`; `bin/molde_fatia/cadeia.sh:51`; `bin/molde_fatia/cadeia.sh:55`; `bin/isolamento.sh:45`) |
| `placar_code` | `_cred` abre subshell, gera env-file e dispara docker diretamente. Não há aquisição global nesse sítio. (`bin/placar_code.sh:121`; `bin/placar_code.sh:122`) |
| `esteira_vigia` | `vigia_esteira → Python → passada → _isolar/_rodar_testes`, sob `/tmp/vigia_esteira.lock`, distinto. (`bin/vigia_esteira.sh:9`; `bin/vigia_esteira.sh:11`; `app/core/esteira_vigia.py:1473`; `app/core/esteira_vigia.py:740`) |
| Python pelo integrador | `integrador → integrador_lote.rodar → _suite → regua → trava`; integrador segura `/tmp/integrador.lock`, os locks da árvore/raia e `/tmp/regua.lock`; chamadas Python adicionais acontecem fora do comando protegido da régua. (`bin/integrador.sh:9`; `bin/integrador.sh:32`; `bin/integrador.sh:51`; `bin/integrador.sh:52`; `app/core/integrador_lote.py:639`; `app/core/integrador_lote.py:375`) |

Há **sete sítios** chamadores de `_rodar_testes`: quatro internos (`_isolar`, `auto_revert`, `curar_por_relogio`, `autocura`) e três no integrador (`_suite`, `_isolamento`, `_conferir_quarentena`). (`app/core/esteira_vigia.py:740`; `app/core/esteira_vigia.py:1269`; `app/core/esteira_vigia.py:1319`; `app/core/esteira_vigia.py:1406`; `app/core/integrador_lote.py:373`; `app/core/integrador_lote.py:403`; `app/core/integrador_lote.py:769`)

Perda de rc e contraexemplos:

- `suite` usa `exec`, preservando o resultado da trava; `_teste_irmao` guarda `_rc`, distingue 75 e devolve `_rc`. (`bin/suite.sh:96`; `bin/pre-push.sh:141`; `bin/pre-push.sh:149`; `bin/pre-push.sh:157`)
- `regua` recebe diretamente o status, mas o descarta e decide por texto `Ran`/`OK`; um 75 vira “a suite nem rodou” e rc final 1. (`bin/regua.sh:165`; `bin/regua.sh:170`; `bin/regua.sh:178`; `bin/regua.sh:205`)
- `regua_relogio` continua após o pipeline e reinicia `RC=0`; ausência de teste pode terminar OK se a lista conhecida estiver vazia. (`bin/regua_relogio.sh:51`; `bin/regua_relogio.sh:58`; `bin/regua_relogio.sh:67`; `bin/regua_relogio.sh:77`)
- `regua_calendario` ignora o rc dos três docker e compara apenas nomes vermelhos; três falhas de infraestrutura podem terminar OK com lista vazia. (`bin/regua_calendario.sh:43`; `bin/regua_calendario.sh:48`; `bin/regua_calendario.sh:55`; `bin/regua_calendario.sh:59`)
- `vigia_arvore` troca ausência de `Ran` por rc 1; nas confirmações decide pelo log, sem preservar 75. (`bin/vigia_arvore.sh:88`; `bin/vigia_arvore.sh:90`; `bin/vigia_arvore.sh:106`; `bin/vigia_arvore.sh:136`)
- Moldes descartam os rc de RED/GREEN e decidem pelo texto; o molde de autocura termina com `echo`, podendo retornar 0 após falha do teste. (`bin/molde_fatia/rodar.sh:46`; `bin/molde_fatia/rodar.sh:48`; `bin/molde_autocura/rodar.sh:24`; `bin/molde_autocura/rodar.sh:27`)
- `_iso_roda` devolve o resultado do grep; falha de infraestrutura pode ser atribuída à árvore por `isolar_ou_parar`. (`bin/isolamento.sh:31`; `bin/isolamento.sh:34`; `bin/isolamento.sh:45`; `bin/isolamento.sh:49`)
- `placar_code` não guarda falha de `teste_envfile` nem do docker/pipeline; saída vazia vira `_cred=0`. (`bin/placar_code.sh:121`; `bin/placar_code.sh:127`; `bin/placar_code.sh:128`)
- `_rodar_testes` ignora `returncode`, devolvendo apenas presença de `OK`; após migração, 75 também se tornaria `False`, indistinto de vermelho. `_isolar` grava “arvore” e `_isolamento` devolve verdadeiro nessa situação. (`app/core/esteira_vigia.py:760`; `app/core/esteira_vigia.py:762`; `app/core/esteira_vigia.py:741`; `app/core/integrador_lote.py:403`)
- `_conferir_quarentena` ignora até esse booleano: log sem `FAIL/ERROR` libera todos os testes da lista, mesmo que nada tenha rodado. (`app/core/integrador_lote.py:769`; `app/core/integrador_lote.py:771`; `app/core/integrador_lote.py:772`)
- `_suite` ignora rc da régua e escolhe o arquivo `/tmp/regua_*` mais recente; falha antes de produzir log novo pode ler uma execução anterior. (`app/core/integrador_lote.py:375`; `app/core/integrador_lote.py:376`; `app/core/integrador_lote.py:379`)
- Existe cópia adicional de pre-push em `app/core/hooks`, fora do universo `bin/**/*.sh + Python` do contrato: docker direto em `_teste_irmao`, com duas chamadas de testes. Sua ativação é **[não verificado]**; o wrapper oficial gerado aponta a `bin/pre-push.sh`. (`app/core/hooks/pre-push.sh:54`; `app/core/hooks/pre-push.sh:82`; `app/core/hooks/pre-push.sh:87`; `bin/hooks.sh:56`)

Acoplamentos de testes ao texto/argv: [nao verificado]

- `test_arvore_vermelha` exige literais `desfazer_copia`, `exec bash "$script"`, `git archive HEAD app`, `SNAP=/tmp/vigia_arvore_commit_`, `sozinhos` e `alarme_arvore`; extrai o **primeiro** `grep -oP` de isolamento e executa só esse grep em log sintético. Não executa os scripts nem cria cadeia aninhada de trava. (`app/core/tests/test_arvore_vermelha.py:39`; `app/core/tests/test_arvore_vermelha.py:53`; `app/core/tests/test_arvore_vermelha.py:58`; `app/core/tests/test_arvore_vermelha.py:61`; `app/core/tests/test_arvore_vermelha.py:67`; `app/core/tests/test_arvore_vermelha.py:72`)
- `test_suite_por_lote` analisa o texto do molde com regex de linhas contendo `LABELS` e `regua.sh`/`$LABELS`; não executa a suite do integrador. (`app/core/tests/test_suite_por_lote.py:81`; `app/core/tests/test_suite_por_lote.py:86`; `app/core/integrador_lote.py:203`)
- `test_prepush_veredito_e_o_rc` recorta a função por `^_teste_irmao()`/`^}`, exige o literal `return "$_rc"` e proíbe `| tail -12; }`. (`bin/tests/test_prepush_veredito_e_o_rc.sh:61`; `bin/tests/test_prepush_veredito_e_o_rc.sh:64`; `bin/tests/test_prepush_veredito_e_o_rc.sh:68`)
- `test_prepush_testa_o_commit` exige o argv textual `-v "$ARVORE_PUSH/app":/app`, proíbe a montagem da árvore viva e cobra nomes dos auxiliares. (`bin/tests/test_prepush_testa_o_commit.sh:63`; `bin/tests/test_prepush_testa_o_commit.sh:65`; `bin/tests/test_prepush_testa_o_commit.sh:69`; `bin/tests/test_prepush_testa_o_commit.sh:71`)
- `test_recursos_sourced` junta continuações e exige `TESTE_DOCKER` ou `--cpuset-cpus` em cada linha contendo `docker run` e `manage.py test`. (`bin/tests/test_recursos_sourced.sh:38`; `bin/tests/test_recursos_sourced.sh:42`)
- `test_segredo_fora_do_argv` busca `-e DB_PASSWORD=` só em `bin`, exige os literais `teste_envfile()` e `umask 177`, além do modo 600 do arquivo existente. (`bin/tests/test_segredo_fora_do_argv.sh:24`; `bin/tests/test_segredo_fora_do_argv.sh:39`; `bin/tests/test_segredo_fora_do_argv.sh:42`; `bin/tests/test_segredo_fora_do_argv.sh:48`)
- `test_fonte_unica_antes_do_uso` depende de regex de `source ... recursos.sh` e posição da chamada das funções; aceita mesma linha, como no placar; varre só scripts diretos de `bin` e `molde_fatia`. (`bin/tests/test_fonte_unica_antes_do_uso.sh:38`; `bin/tests/test_fonte_unica_antes_do_uso.sh:41`; `bin/tests/test_fonte_unica_antes_do_uso.sh:47`; `bin/tests/test_fonte_unica_antes_do_uso.sh:54`)

`gate_cobranca` não dispara teste: cria `SONDA` única no host, mas copia para nome fixo `/tmp/gate_sonda.py` **dentro de `saas_core`**, e usa `manage.py shell`/`tenant_command`; esse temporário não fica no `logs/` do env-file. Falha de coleta vira zero pelo `_int`, portanto dado ausente pode parecer medição zero. (`bin/gate_cobranca.sh:18`; `bin/gate_cobranca.sh:36`; `bin/gate_cobranca.sh:37`; `bin/gate_cobranca.sh:40`; `bin/gate_cobranca.sh:54`)

## 31/10 17:42 · sessao 36dbb69fb8ed · E4-7-SAIDA-III · verificador · origem=Code

**A volta 2 ainda não fecha a verificação:** A1 corrige o noturno do teste, mas admite outro caso de data divergente; A2 reduz consultas, porém mantém N+1 nos encerrados; A3 e A4 têm a cura pedida no código. (`app/core/services/painel_op.py:97`, `app/core/services/painel_op.py:202`, `app/core/services/painel_op.py:214`; `app/templates/core/_painel_card.html:69`)

RED/GREEN da suíte Django, número efetivamente medido pela sonda, ocorrência dos contraexemplos em produção e ordem histórica de pouso: **[não verificado]**. [nao verificado]

1) **Quem escreve e quem lê.**

Os campos abaixo são escritos no contexto em memória do painel; `dia` lê o estado persistido pelo D4. O escritor persistente de `status_dia/status_rotulo` continua sendo a porta de lavratura. (`app/core/services/painel_op.py:225`; `app/ponto/status_dia.py:171`; `app/ponto/portas/celula.py:701`, `app/ponto/portas/celula.py:709`)

| Campo/valor | Quem escreve ou fornece | Quem lê |
|---|---|---|
| `status=normal/geofence/turno_aberto` | O juiz fornece `_tm`; o painel escolhe os rótulos usando alerta geo e fim previsto +30 minutos. **Os quatro rótulos não vêm prontos do juiz.** (`app/core/services/painel_op.py:168`, `app/core/services/painel_op.py:172`) | Filtro, contadores, agregado geo, ordenação, avatar, badge e filtro JS via atributo. (`app/core/services/painel_op.py:202`, `app/core/services/painel_op.py:244`, `app/core/services/painel_op.py:288`, `app/core/services/painel_op.py:296`; `app/templates/core/_painel_card.html:6`, `app/templates/core/_painel_card.html:54`, `app/templates/core/_painel_card.html:66`) |
| `status=encerrado` | Painel: existe E recente e não existe `_tm` vivo. Não exige encontrar um turno efetivamente fechado. (`app/core/services/painel_op.py:193`) | Além dos leitores anteriores, abre o caminho de data/D4. (`app/core/services/painel_op.py:214`; `app/templates/core/_painel_card.html:67`) |
| `status=None` | Painel: sem turno vivo e sem E selecionada. (`app/core/services/painel_op.py:196`) | Avatar/badge usam `previsao`; Designar exige também `previsto_agora`. (`app/templates/core/_painel_card.html:54`, `app/templates/core/_painel_card.html:66`, `app/templates/core/_painel_card.html:69`) |
| `previsao=folga/escala_suspeita/previsto/None` | `_previsao_de_hoje`: pergunta `EC.eh_dia_trabalho`; ausência de EC/tipo produz `None`. (`app/core/services/painel_op.py:27`; `app/escala/models.py:1048`) | Avatar e badge; testes dos dois primeiros casos. (`app/templates/core/_painel_card.html:54`, `app/templates/core/_painel_card.html:66`; `app/core/tests/test_e4_7iii_painel_e_presenca.py:94`, `app/core/tests/test_e4_7iii_painel_e_presenca.py:103`) |
| `dia=None` ou `{status,rotulo,fonte}` | Inicialização local; D4 somente após encontrar `_d_turno` e obter encerramento verdadeiro. (`app/core/services/painel_op.py:212`) | Tooltip e texto `enc. · status` do card. (`app/templates/core/_painel_card.html:66`) |
| `data_turno` | `_tm.data_turno`; sem vivo, `_d_turno`; sem mapeamento, **hoje**. (`app/core/services/painel_op.py:230`) | Testes de ancoragem e novo A1. Leitor produtivo adicional encontrado: **[não verificado]**. (`app/core/tests/test_painel_op_turno_aberto_ancora.py:50`; `app/core/tests/test_painel_op_turno_pelo_juiz.py:80`; `app/core/tests/test_e4_7iii_painel_e_presenca.py:263`) |
| `filtro_status` | GET da página/lote; serviço transforma palavra fora de `statuses` em `todos`. (`app/core/views.py:609`, `app/core/views.py:627`; `app/core/services/painel_op.py:64`) | Recorte das linhas e exclusão de postos vazios; a view ainda publica no contexto o GET bruto. (`app/core/services/painel_op.py:202`, `app/core/services/painel_op.py:284`; `app/core/views.py:669`) |
| `statuses` | Lista fixa dos quatro valores, rótulos e cores. (`app/core/services/painel_op.py:56`) | Validação do filtro, retorno e contexto da view; consumidor atual no HTML: **[não verificado]**. (`app/core/services/painel_op.py:64`, `app/core/services/painel_op.py:314`; `app/core/views.py:639`, `app/core/views.py:672`) |
| `_turno_vivo` | `turnos_abertos_de`, descartando respostas sem turno/entrada. (`app/core/services/painel_op.py:97`) | `_em_turno_ids`, presença/cobertura, duração, status e data da linha. (`app/core/services/painel_op.py:101`, `app/core/services/painel_op.py:168`, `app/core/services/painel_op.py:176`, `app/core/services/painel_op.py:230`, `app/core/services/painel_op.py:286`) |
| `contadores` | Quatro somas sobre **linhas já filtradas**; linhas com `status=None` não entram nessas somas. (`app/core/services/painel_op.py:202`, `app/core/services/painel_op.py:244`) | View e stats da página/fragmento. (`app/core/views.py:637`, `app/core/views.py:670`; `app/templates/core/_painel_stats.html:32`; `app/templates/core/_painel_fragment.html:2`) |
| `previsto_agora` | Composição local de `eh_dia_trabalho` e `marcos_turno`, para ontem/hoje, usando EC ativa. (`app/core/services/painel_op.py:109`, `app/core/services/painel_op.py:144`) | Cobertura do posto e condição de Designar. (`app/core/services/painel_op.py:290`; `app/templates/core/_painel_card.html:69`) |

A entrada selecionada é a última E apurável por timestamp, com limite inferior de 24 horas e **sem limite superior**; ela também fornece horário, ID, coordenadas e entrada usada na classificação geo. (`app/core/services/painel_op.py:78`, `app/core/services/painel_op.py:83`, `app/core/services/painel_op.py:223`, `app/core/services/painel_op.py:228`; `app/ponto/turnos.py:2844`)

| Atributo/ação do card | Escritor | Leitor |
|---|---|---|
| `data-search` | Nome do posto/empresa. (`app/templates/core/_painel_card.html:3`) | Busca por `dataset.search`. (`app/templates/core/painel_operacional.html:198`) |
| `data-cobertura` | Cobertura calculada do posto. (`app/templates/core/_painel_card.html:4`) | Filtro Descobertos. (`app/templates/core/painel_operacional.html:176`) |
| `data-tem-geofence` | `p.geofence > 0` → `1/0`. (`app/templates/core/_painel_card.html:5`) | Filtro Geofence. (`app/templates/core/painel_operacional.html:177`) |
| `data-tem-aberto` | Concatena um `1` por colaborador aberto. (`app/templates/core/_painel_card.html:6`) | Filtro Aberto exige exatamente `'1'`. (`app/templates/core/painel_operacional.html:178`) |
| `data-tem-ausente` | Removido; o card atual publica apenas os atributos anteriores. (`app/templates/core/_painel_card.html:3`) | O filtro atual conserva Descobertos/Geofence/Aberto. (`app/templates/core/painel_operacional.html:173`) |
| Designar | `previsto_agora and not status`; publica titular ID/nome, posto ID/nome e empresa ID. (`app/templates/core/_painel_card.html:69`) | `abrirSubstModal` copia os cinco atributos para `estado`. (`app/templates/core/painel_operacional.html:280`) |

O modal envia titular/posto/janela ao GET de disponíveis e titular/substituto/posto/escala/janela ao POST de criação; o POST delega à porta `designar_cobertura`. `empresa_id` não integra esse POST, e o botão não transporta `data_turno`. (`app/templates/core/painel_operacional.html:343`, `app/templates/core/painel_operacional.html:410`; `app/ponto/views.py:3027`, `app/ponto/views.py:3065`; `app/templates/core/_painel_card.html:69`)

As portas chamadas pelo diff têm estes produtores e leitores externos:

| Porta | Quem produz a resposta | Outros leitores diretos encontrados |
|---|---|---|
| `turnos_abertos_de` | Carregador plural chama o mesmo `_turno_aberto_puro` do singular. (`app/ponto/turnos.py:2266`, `app/ponto/turnos.py:2333`) | Serviço situacional. (`app/colaboradores/services/situacional.py:169`) |
| `dia_das_batidas` | Dias dos turnos recebidos; batida não reclamada recebe dia civil. (`app/ponto/turnos.py:2883`) | API e espelho. (`app/api/views.py:1446`, `app/api/views.py:2090`; `app/ponto/services/espelho.py:462`) |
| `dias_encerrados` | Carrega células/vínculos e chama `_dia_encerrado_motivo_puro` por dia. (`app/ponto/turnos.py:2129`) | Calendário, cartório, D4, emissão retroativa e suprajuiz. (`app/colaboradores/services/calendario.py:697`; `app/ponto/services/cartorio.py:910`; `app/ponto/status_dia.py:176`; `app/ponto/management/commands/emitir_furo_retroativo.py:62`; `app/ponto/management/commands/supra_juiz.py:108`) |
| `status_do_dia_lavrado` | Lê célula e pergunta encerramento; devolve lavra ou resposta temporal. (`app/ponto/status_dia.py:157`) | Chamador produtivo direto encontrado nesta cópia: painel. (`app/core/services/painel_op.py:220`) |
| `turnos_do_colab` | Carrega universo vivo, células, vínculos/ata; pareia e recorta por `data_turno`. (`app/ponto/turnos.py:1848`, `app/ponto/turnos.py:1973`) | Calendário, espelho, drawer, API, PDF, núcleo, escala e caminhos de chamados/apuração listados abaixo. (`app/colaboradores/services/calendario.py:301`; `app/ponto/services/espelho.py:625`; `app/colaboradores/services/drawer.py:45`; `app/api/views.py:1449`; `app/relatorios/pdf_espelho.py:213`; `app/ponto/nucleo.py:47`) |

O restante do censo direto de `turnos_do_colab` inclui:

- Chamados: detector, juízes, modelo, emissão e materialização. (`app/chamados/detector_anomalias.py:155`; `app/chamados/juizes.py:369`, `app/chamados/juizes.py:1727`; `app/chamados/models.py:1260`; `app/chamados/services/disputa_emissao.py:644`; `app/chamados/services/materializacao.py:109`, `app/chamados/services/materializacao.py:204`)
- Escala: defesa, escala automática, impacto de troca, jornada e grade. (`app/escala/regua_defesa.py:358`; `app/escala/services/escala_auto.py:87`; `app/escala/services/impacto_troca.py:18`; `app/escala/servico_jornada.py:250`, `app/escala/servico_jornada.py:392`; `app/escala/utils.py:1524`, `app/escala/utils.py:1838`)
- Ponto: bordas, esmeril, passivo, regularização, teto, triagem, vigia, leitura de turno e views. (`app/ponto/services/bordas_realizado.py:66`; `app/ponto/services/esmeril_espelho.py:98`; `app/ponto/services/passivo_s84.py:82`; `app/ponto/services/regularizacao.py:61`; `app/ponto/services/teto_na_lista.py:68`; `app/ponto/services/triagem_batida.py:552`; `app/ponto/services/vigia_pareador_celula.py:97`; `app/ponto/turno_leitura.py:89`, `app/ponto/turno_leitura.py:95`; `app/ponto/views.py:377`, `app/ponto/views.py:1586`, `app/ponto/views.py:3167`)
- Comandos: censos, ausências, pares relâmpago, DNA, oráculo, turno retroativo, abandono e vigias. (`app/ponto/management/commands/censo_fase_12x36.py:180`; `app/ponto/management/commands/detectar_ausencias.py:336`; `app/ponto/management/commands/detectar_par_relampago.py:101`; `app/ponto/management/commands/dna_x_batida_real.py:69`; `app/ponto/management/commands/e6_oraculo.py:147`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88`; `app/ponto/management/commands/marcar_abandono_provisorio.py:46`; `app/ponto/management/commands/verificar_nucleo.py:29`; `app/ponto/management/commands/vigia_plantio_overnight.py:38`; `app/relatorios/management/commands/vinculo_do_dia_divergentes.py:124`)
- No próprio módulo: realizado, encerramento, par completo, mapa de dias e batidas do dia-turno. (`app/ponto/turnos.py:778`, `app/ponto/turnos.py:2079`, `app/ponto/turnos.py:2132`, `app/ponto/turnos.py:2152`, `app/ponto/turnos.py:2882`, `app/ponto/turnos.py:2917`)

`ALLOWLIST` e `LOOKUP_ALLOWLIST` são escritas como conjuntos no módulo do contrato. A primeira alimenta os testes de novos `.date()` e entradas podres; a segunda alimenta os testes equivalentes de lookups ORM. Retirar o painel da primeira é coerente com a mudança sintática; mantê-lo na segunda corresponde aos lookups de geo/atividade ainda existentes. Isso **não verifica a semântica da data retornada indiretamente** por `dia_das_batidas`. (`app/ponto/tests/test_contract_no_batida_date.py:10`, `app/ponto/tests/test_contract_no_batida_date.py:98`, `app/ponto/tests/test_contract_no_batida_date.py:233`, `app/ponto/tests/test_contract_no_batida_date.py:247`, `app/ponto/tests/test_contract_no_batida_date.py:266`; `app/core/services/painel_op.py:104`, `app/core/services/painel_op.py:217`, `app/core/services/painel_op.py:274`)

Fora da tela, o registro vivo mantém pendentes os contadores, badge/atributo de aberto e filtro JS. O host lê o mesmo medidor por `bin/leitor_dia_estado.py`; o gerador de estado consome esse resultado. (`app/core/juizes.py:715`, `app/core/juizes.py:727`; `bin/leitor_dia_estado.py:25`, `bin/leitor_dia_estado.py:35`; `bin/gerar_estado.py:284`)

2) **A1..A4: cura e contraexemplos.**

| Item | Resultado da inspeção |
|---|---|
| **A1** | Cura a fixture 19–07, pausa 00–01, consulta 08h: agora busca o dia do turno e o teste exige esse dia. **Não fecha toda a classe**, conforme contraexemplo abaixo. (`app/core/services/painel_op.py:215`; `app/core/tests/test_e4_7iii_painel_e_presenca.py:241`, `app/core/tests/test_e4_7iii_painel_e_presenca.py:263`) |
| **A2** | Cura o custo do D4 para linhas excluídas pelo filtro e troca o turno vivo por lote. Para linhas encerradas preservadas, continuam chamadas individuais e recargas internas. Número medido exigido na triagem: **[não verificado]**. (`app/core/services/painel_op.py:97`, `app/core/services/painel_op.py:202`, `app/core/services/painel_op.py:214`; `app/ponto/status_dia.py:171`) |
| **A3** | Cura a cobertura parcial: o botão deixou de exigir posto descoberto. O teste tem dois previstos, um presente e exige botão apenas no outro. Saída antecipada continua barrada por `status=encerrado`, como já reconhecido na fila fornecida. (`app/templates/core/_painel_card.html:69`; `app/core/tests/test_e4_7iii_painel_e_presenca.py:335`, `app/core/tests/test_e4_7iii_painel_e_presenca.py:361`; `app/core/services/painel_op.py:193`) |
| **A4** | Cura `ausente/folga/lixo` na porta compartilhada por página e lote. O teste preserva também o filtro válido `encerrado`. Há quebra herdada do refresh para filtros válidos, detalhada no item 5. (`app/core/services/painel_op.py:64`; `app/core/views.py:607`, `app/core/views.py:632`; `app/core/tests/test_e4_7iii_painel_e_presenca.py:371`) |

**Contraexemplo construído de A1, somente com batidas passadas:** colaborador ativo, vínculo diurno 07–19 com intervalo 12–13; agora **17/09/2026 às 20h local**; E viva em **14/09 às 07h** e E viva em **16/09 às 20h com `is_intervalo=True`**, nenhuma outra batida. A flag existe no modelo; o pareador a absorve no turno corrente, e a guarda temporal desse trecho exige `not _iv`. Assim, o turno de 14/09 contém as duas batidas. (`app/ponto/models.py:26`; `app/ponto/turnos.py:1324`, `app/ponto/turnos.py:1347`, `app/ponto/turnos.py:1369`, `app/ponto/turnos.py:1495`)

Nesse caso:

- O plural lê 15–17/09, não recebe a E inicial e ignora a E intervalada isolada. O painel encontra a E de 16/09 nas 24 horas e a chama `encerrado`. (`app/ponto/turnos.py:2323`, `app/ponto/turnos.py:1369`; `app/core/services/painel_op.py:78`, `app/core/services/painel_op.py:83`, `app/core/services/painel_op.py:193`)
- `turnos_do_colab(15,17)` lê a E antiga pelo pad, mas elimina o turno de 14/09 pelo recorte final. `dia_das_batidas` então usa o dia civil **16/09**, que vira `data_turno` da linha. (`app/ponto/turnos.py:1857`, `app/ponto/turnos.py:1973`, `app/ponto/turnos.py:2889`; `app/core/services/painel_op.py:217`, `app/core/services/painel_op.py:230`)
- O espelho mensal atribui essa mesma E ao turno de **14/09**; o calendário mensal também a recebe nesse turno. Portanto, há divergência concreta de pertencimento/data entre as janelas. Ocorrência em produção: **[não verificado]**. (`app/ponto/services/espelho.py:462`; `app/ponto/turnos.py:2884`; `app/colaboradores/services/calendario.py:301`)

Para A2, o próprio cenário do teste — passar de três para seis colaboradores E07/S19, consulta 20h, filtro `todos` — percorre o bloco D4 mais vezes. `turnos_do_colab` consulta batidas/células/vínculos; `dias_encerrados` recarrega células/vínculos; existindo célula, D4 consulta-a e recalcula turnos/encerramento. O custo residual cresce por linha; seu coeficiente efetivo permanece **[não verificado]**. (`app/core/tests/test_e4_7iii_painel_e_presenca.py:269`, `app/core/tests/test_e4_7iii_painel_e_presenca.py:275`; `app/ponto/turnos.py:1858`, `app/ponto/turnos.py:2129`; `app/ponto/status_dia.py:171`)

Os novos testes plantam `status_dia` diretamente e não comparam painel, espelho e calendário para o mesmo caso. Os outros dois leitores ainda possuem classificadores próprios; igualdade entre suas palavras e o D4 não foi demonstrada por esses testes. (`app/core/tests/test_e4_7iii_painel_e_presenca.py:76`, `app/core/tests/test_e4_7iii_painel_e_presenca.py:260`; `app/ponto/services/espelho.py:152`; `app/colaboradores/services/calendario.py:93`)

3) **Universos e janelas não são integralmente iguais.**

| Caminho | Universo/janela efetivos |
|---|---|
| Entrada/candidatos do painel | Vivas desde `agora−24h`, sem teto superior; colaboradores ativos do recorte empresarial. (`app/core/services/painel_op.py:70`, `app/core/services/painel_op.py:78`, `app/core/services/painel_op.py:83`, `app/core/services/painel_op.py:94`) |
| Turno vivo singular/plural | Vivas por data civil de hoje−2 até hoje; mesmo corpo puro. O painel restringe antes os candidatos a 24h. (`app/ponto/turnos.py:2190`, `app/ponto/turnos.py:2323`, `app/ponto/turnos.py:2333`; `app/core/services/painel_op.py:94`) |
| Turnos para D4 do painel | Dias de turno `[hoje−2,hoje]`; consulta de batidas recebe pad `[hoje−4,hoje+1]`, mas o resultado volta a ser recortado por `data_turno`. (`app/core/services/painel_op.py:215`; `app/ponto/turnos.py:1845`, `app/ponto/turnos.py:1973`) |
| Mapa e portão do painel | Recebem corretamente a **mesma lista** e a mesma janela anterior. (`app/core/services/painel_op.py:216`) |
| D4 interno | Recarrega turnos e encerramento para `[data,data]`, não reutiliza a lista do painel. (`app/ponto/status_dia.py:175`) |
| Espelho/calendário | Usam o período apresentado, mensal ou personalizado. O calendário passa seus próprios turnos à juíza da mesma janela. (`app/ponto/services/espelho.py:462`, `app/ponto/services/espelho.py:625`; `app/colaboradores/services/calendario.py:285`, `app/colaboradores/services/calendario.py:301`, `app/colaboradores/services/calendario.py:697`) |

A identidade do filtro `retratada_em IS NULL` foi preservada nesses carregadores; isso não torna suas janelas equivalentes. O próprio contrato de alimentação avisa que a resposta do pareamento depende da janela. (`app/ponto/turnos.py:1858`, `app/ponto/turnos.py:2111`, `app/ponto/turnos.py:2324`, `app/ponto/turnos.py:2890`)

**Não: dois dias não cobrem universalmente todo turno cuja E recente vira “encerrado”.** O caso intervalado acima refuta a afirmação, porque a autoridade de apresentação dá dia civil à batida cujo turno foi excluído da lista recebida. (`app/ponto/turnos.py:1976`, `app/ponto/turnos.py:2884`, `app/ponto/turnos.py:2893`)

Há ainda um recorte herdado que perde presença normal: **24x48 07–07, E ontem às 07h, sem S, consulta hoje às 08h**. O juiz pode mantê-la viva até 11h, mas a E já tem 25 horas e fica fora dos candidatos do painel. A troca para plural não corrige isso. (`app/ponto/selecao_periodo.py:164`; `app/ponto/turnos.py:1987`, `app/ponto/turnos.py:2005`; `app/core/services/painel_op.py:78`, `app/core/services/painel_op.py:94`)

Também não há garantia para E futura: uma E depois de amanhã passa pelo limite inferior, fica fora da janela do plural/mapa e pode resultar em `encerrado`, `dia=None`, `data_turno=hoje`. Existência desse dado em produção: **[não verificado]**. (`app/core/services/painel_op.py:83`, `app/core/services/painel_op.py:193`, `app/core/services/painel_op.py:230`; `app/ponto/turnos.py:2844`)

4) **Classes de reprovação e mordida dos testes.** [nao verificado]

- **Segundo juiz:** a troca singular→plural não cria um: ambos chegam ao mesmo corpo. O dia do turno e o D4 já têm autoridades registradas. Entretanto, `dia_das_batidas` responde também à apresentação de batidas não reclamadas; usar sua resposta como identidade de turno é justamente a fragilidade de A1. (`app/ponto/turnos.py:2266`, `app/ponto/turnos.py:2333`, `app/ponto/t

[truncado em 20000 de 28318 caracteres]

## 31/10 17:42 · sessao 834d7c0b4f9f · - · - · origem=Ronald (tmux)

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 31/10 17:41 · sessao a39845de0124 · - · - · origem=Ronald (tmux)

1. **Leitores/escritores identificados**

- A própria trava abre/trunca `/tmp/juliani_db_test.lock`, obtém o flock, escreve `<ESTEIRA_QUEM ou basename>:<pid> desde <data>` em `.quem` e escreve `livre desde <data>` no `EXIT` (`bin/trava_teste.sh:21`, `bin/trava_teste.sh:23`, `bin/trava_teste.sh:35-41`).
- `--quem` só testa se `.quem` tem conteúdo e o imprime inteiro; sem conteúdo imprime `livre`. Não consulta flock, PID nem Docker (`bin/trava_teste.sh:28`).
- A mensagem de timeout também lê `.quem` inteiro, usando `?` quando o `cat` falha (`bin/trava_teste.sh:37`).
- **O handoff não lê `--quem`.** Consulta diretamente os locks de teste e sombra com `flock -n "$_arq" true`; imprime “trava nunca criada”, “LIVRE” ou “TOMADA”, sem nome do detentor ou contêiner (`bin/handoff_sessao.sh:223-235`).
- Os hooks PreCompact chamam o handoff, e SessionStart `compact`/`clear` imprimem o Markdown gerado; portanto consomem indiretamente a classificação do flock, sem ler `.quem` (`.claude/settings.json:20`, `.claude/settings.json:29`, `.claude/settings.json:40`, `.claude/settings.json:53`).
- As sessões recebem orientação documental para consultar `--quem`; a documentação atual explica que `livre` responde pelo chamador e não garante ausência de suíte (`CLAUDE.md:203-205`). [nao verificado]
- O selo da trava lê o **código** para substituir a linha literal `TRAVA=/tmp/juliani_db_test.lock`, exercitando uma cópia com lock isolado; não lê o lock global (`bin/tests/test_trava_teste.sh:101-114`).
- Outros leitores/escritores executáveis em Python/hooks de `.quem` ou consumidores programáticos de `--quem`: **[não verificado]**; o censo textual não encontrou além dos sítios acima.

2. **Falhas concretas relevantes**

- O contrato atribui ao handoff a leitura de `--quem`, mas ele usa flock diretamente; corrigir somente `--quem` deixa o handoff chamando um recurso “LIVRE” mesmo com contêiner órfão. O caminho que publica LIVRE termina no sucesso do flock, sem Docker (`bin/handoff_sessao.sh:230-233`).
- O selo atual mede por **arquivo**: percorre somente `bin/*.sh`, ignora qualquer arquivo cujo caminho case `*test_*`, aceita a mera presença de `trava_teste.sh` em qualquer ponto e concede isenção por presença de `REGUA_DB`, salvo dois padrões literais de suíte completa (`bin/tests/test_trava_teste.sh:40-63`).
- Contraexemplo existente: o molde é julgado separadamente apenas pela presença textual de `trava_teste.sh`; um comando protegido pode aprovar o arquivo inteiro e esconder outro comando sem trava (`bin/tests/test_trava_teste.sh:70-71`).
- Contraexemplo sintético demonstrável pela lógica: um arquivo com `python manage.py test core`, `REGUA_DB` somente em outra linha e sem trava é dispensado; um arquivo com dois testes, dos quais só um usa a trava, também passa (`bin/tests/test_trava_teste.sh:54-62`).
- `--quem` pode publicar informação lateral antiga: não verifica que o PID escrito continua vivo nem que o flock esteja preso; sem `.quem`, responde livre independentemente do flock (`bin/trava_teste.sh:28`, `bin/trava_teste.sh:40-41`).
- Não há atualmente injeção de label, inspeção de órfãos nem parada de contêiner no corpo da trava; ela simplesmente executa `"$@"` após gravar `.quem` (`bin/trava_teste.sh:35-42`).

3. **Testes/selos presos ao texto ou forma** [nao verificado]

- O selo da trava depende da linha **exata** `TRAVA=/tmp/juliani_db_test.lock`; qualquer mudança de forma que impeça o `sed` aciona aborto antes dos testes de concorrência (`bin/tests/test_trava_teste.sh:103-114`).
- O mesmo selo exige `trava_teste.sh` textual no molde, usa os padrões literais `test api chamados` e `test ${ONLY:-$LABELS}`, e só prova serialização/timeout com comandos `bash`, `sleep` e `true`, sem Docker (`bin/tests/test_trava_teste.sh:59-71`, `bin/tests/test_trava_teste.sh:119-137`).
- O selo do handoff exige literalmente `flock -n`, `juliani_db_test.lock` e `sombra.lock`, proibindo o nome de `pgrep`; migrar o handoff integralmente para `--quem` exige adaptar esse selo (`bin/tests/test_handoff_sessao.sh:115-118`).
- O selo de `suite.sh` exige as substrings `trava_teste.sh`, `manage.py test`, uma regex específica de `LABELS=$(grep -oP ...regua.sh`, e ausência de `DB_PASSWORD` (`bin/tests/test_suite_sh.sh:50-58`).
- O selo de segredo exige literalmente `teste_envfile()` e `umask 177`; uma implementação equivalente com outra forma reprova (`bin/tests/test_segredo_fora_do_argv.sh:39-43`).
- Django exige em `isolamento.sh` as strings `desfazer_copia` e `exec bash "$script"` (`app/core/tests/test_arvore_vermelha.py:51-54`).
- O teste Django extrai a **primeira ocorrência** de `grep -oP '...'` do código de isolamento e executa a regex extraída contra fixture; adicionar antes outro `grep -oP` pode quebrar esse teste sem alterar a extração real dos IDs (`app/core/tests/test_arvore_vermelha.py:56-63`).
- Django exige em `vigia_arvore.sh` as strings `git archive HEAD app`, `SNAP=/tmp/vigia_arvore_commit_`, `sozinhos` e `alarme_arvore` (`app/core/tests/test_arvore_vermelha.py:65-73`).
- O selo de recursos junta continuações e identifica comandos pelas strings `docker run` e `manage.py test`, exigindo `TESTE_DOCKER` ou `--cpuset-cpus`; esconder o comando atrás de função muda o alcance desse censo (`bin/tests/test_recursos_sourced.sh:38-43`).
- O selo de cpuset procura contêineres cujo `.Path` + `.Args` contenham literalmente `manage.py test`; uma forma como `python manage.py` com subcomando montado por shell pode escapar da detecção (`bin/tests/test_cpuset_de_teste.sh:41-50`).
- O selo de ordem da fonte identifica `source` e chamadas das funções por regex de linha; aceita fonte e uso na mesma linha, como no placar (`bin/tests/test_fonte_unica_antes_do_uso.sh:38-48`).
- O teste Django do molde exige que `integrador_lote.roda_suite_completa()` retorne lista vazia ao ler `bin/molde_fatia/rodar.sh`; alterações de invocação precisam preservar a semântica reconhecida por esse juiz (`app/core/tests/test_suite_por_lote.py:81-87`).

## 31/10 17:39 · sessao 092d76fd4836 · - · - · origem=Ronald (tmux)

A1 cura o noturno comum do RED: a entrada de retorno 01h agora é datada pelo turno, e o mesmo conjunto de turnos alimenta `dia_das_batidas` e `dias_encerrados`. Entretanto, isso não garante equivalência para qualquer cadastro/batida: o painel usa `[hoje-2, hoje]`, o D4 recarrega `[data,data]`, e o espelho/calendário usam seu período apresentado. O próprio carregador declara que o pareamento depende da janela. (`app/core/services/painel_op.py:214-220`; `app/ponto/status_dia.py:175-176`; `app/ponto/services/espelho.py:462`; `app/ponto/services/espelho.py:625`; `app/colaboradores/services/calendario.py:301`; `app/colaboradores/services/calendario.py:697`; `app/ponto/turnos.py:2111-2114`.)

Contraexemplo passado construído: agora **17/09/2026 20h local**, vínculo ativo diurno **07–19**, intervalo **12–13**; batidas vivas **14/09 07h E** e **16/09 20h E com `is_intervalo=True`**, sem outras batidas. A flag é permitida pelo modelo e consumida antes das regras de borda; a guarda temporal exige `not _iv`, portanto não barra essa batida já marcada. O pareador produz turno aberto de **14/09**, contendo ambas as batidas. Esse resultado puro foi reproduzido em memória com `python3 -B`, sem arquivo ou banco; ocorrência em produção **[não verificado]**. (`app/ponto/models.py:26`; `app/ponto/turnos.py:1324`; `app/ponto/turnos.py:1347-1374`; `app/ponto/turnos.py:1495-1496`; `app/ponto/turnos.py:316-321`.)

Nesse cenário, o plural lê batidas somente de **15 a 17/09**: não recebe a entrada inicial; a entrada intervalada isolada é ignorada, por não haver turno corrente. O painel, porém, encontra a E de 16/09 nas últimas 24h e declara **encerrado**. Sua consulta de turnos lê a entrada antiga pelo pad, mas elimina o turno de 14/09 porque `data_turno` fica fora de `[15,17]`; `dia_das_batidas` atribui então a batida ao dia civil **16/09**. O espelho mensal atribui essa mesma batida a **14/09**, e o calendário mensal a recebe no turno de 14/09. Logo, **a janela de dois dias não cobre todo turno que uma entrada recente pode integrar**. (`app/ponto/turnos.py:2323-2326`; `app/ponto/turnos.py:1369-1374`; `app/core/services/painel_op.py:78`; `app/core/services/painel_op.py:83-85`; `app/core/services/painel_op.py:193-195`; `app/ponto/turnos.py:1857-1861`; `app/ponto/turnos.py:1973-1976`; `app/ponto/turnos.py:2884-2893`; `app/core/services/painel_op.py:217`; `app/core/services/painel_op.py:230`; `app/ponto/services/espelho.py:462`; `app/colaboradores/services/calendario.py:301-302`.)

Outro contraexemplo, **herdado**, do recorte dos candidatos: escala **24x48 07–07**, entrada ontem às 07h sem saída, consulta hoje às 08h, nenhuma outra batida. O juiz direto mantém aberta viva até **11h**, mas o corte de 24h do painel elimina colaborador e entrada, produzindo ausência de presença na linha e na cobertura. A troca singular→plural não cura esse recorte. (`app/ponto/turnos.py:1987-1994`; `app/ponto/turnos.py:2005-2011`; `app/core/services/painel_op.py:78`; `app/core/services/painel_op.py:83-99`; `app/core/services/painel_op.py:167-200`.)

O painel também não limita as entradas por `agora`: passa somente `ini`, sem `fim`; candidatos seguem o mesmo corte sem teto superior. Uma E futura depois de amanhã pode ser selecionada como entrada “encerrada”, ficar sem mapeamento na janela e receber `data_turno=hoje`. Existência desse dado em produção **[não verificado]**. (`app/core/services/painel_op.py:83-85`; `app/core/services/painel_op.py:94-96`; `app/ponto/turnos.py:2844-2852`; `app/core/services/painel_op.py:193-220`; `app/core/services/painel_op.py:230`.)

Censo das portas, por chamadas diretas de produção encontradas:

- **`status_do_dia_lavrado`**: painel, exclusivamente nesse censo. (`app/core/services/painel_op.py:220`.)
- **`turnos_abertos_de` de `ponto`**: painel e situacional; o homônimo de chamados é outra função. (`app/core/services/painel_op.py:98`; `app/colaboradores/services/situacional.py:169`; `app/chamados/reconciliador.py:82`.)
- **`dias_encerrados`**: calendário, painel, cartório, D4, emissão retroativa e comando suprajuiz. (`app/colaboradores/services/calendario.py:697`; `app/core/services/painel_op.py:218`; `app/ponto/services/cartorio.py:910`; `app/ponto/status_dia.py:176`; `app/ponto/management/commands/emitir_furo_retroativo.py:62`; `app/ponto/management/commands/supra_juiz.py:108`.)
- **`dia_das_batidas`**: API, painel e espelho. O PDF deixou de perguntar diretamente e desenha a resposta da tela. (`app/api/views.py:1446`; `app/api/views.py:2090`; `app/core/services/painel_op.py:217`; `app/ponto/services/espelho.py:462`; `app/relatorios/pdf_espelho.py:529-533`.)
- **`turnos_do_colab`, leitores principais**: API; juízes/modelo/detector/materialização de chamados; calendário e drawer; painel; escala automática, impacto de troca, serviço de jornada e grade; núcleo, espelho, regularização, triagem, verificadores e views; PDF e comandos. (`app/api/views.py:1449`; `app/chamados/juizes.py:369`; `app/chamados/juizes.py:1727`; `app/chamados/models.py:1260`; `app/chamados/detector_anomalias.py:155`; `app/chamados/services/materializacao.py:109`; `app/chamados/services/materializacao.py:204`; `app/chamados/services/disputa_emissao.py:644`; `app/colaboradores/services/calendario.py:301`; `app/colaboradores/services/drawer.py:45`; `app/core/services/painel_op.py:216`; `app/escala/services/escala_auto.py:87`; `app/escala/services/impacto_troca.py:18`; `app/escala/servico_jornada.py:250`; `app/escala/servico_jornada.py:392`; `app/escala/utils.py:1524`; `app/escala/utils.py:1838`; `app/ponto/nucleo.py:47`; `app/ponto/services/espelho.py:625`; `app/ponto/services/regularizacao.py:61`; `app/ponto/services/triagem_batida.py:552`; `app/ponto/views.py:377`; `app/ponto/views.py:1586`; `app/ponto/views.py:3167`; `app/relatorios/pdf_espelho.py:213`; `app/relatorios/pdf_espelho.py:447`.)
- **`bin/`**: referências de cadastro/censo, sem chamada direta dessas cinco portas encontrada no censo AST; os sítios textuais relevantes são estes. (`bin/esmeril_turno_censo.py:63`; `bin/esmeril_vinculo_censo.py:45`.)

O caminho plural usa o mesmo corpo puro do singular e o mesmo recorte civil de três dias; não nasce um juiz novo nessa troca. O painel restringe previamente seus candidatos às últimas 24h, enquanto o serviço situacional passa sua lista de colaboradores. (`app/ponto/turnos.py:2176-2195`; `app/ponto/turnos.py:2266-2276`; `app/ponto/turnos.py:2330-2334`; `app/core/services/painel_op.py:94-98`; `app/colaboradores/services/situacional.py:169`.)

## 31/10 17:38 · sessao 2e4fc01188d4 · - · - · origem=Ronald (tmux)

Auditoria estática; testes não executados, resultados RED/GREEN e contagens reais [não verificado]. Os testes do medidor criam diretórios e arquivos temporários (`app/core/tests/test_e4_7iii_painel_e_presenca.py:182-195`); o selo shell de juiz também pode gravar a base e cria temporário (`bin/tests/test_juiz_novo_tem_corte.sh:31-34,47-60`).

**A2 não elimina N+1:** filtro vem antes do D4 (`app/core/services/painel_op.py:202-220`) e turno vivo usa plural (`:93-100`), mas, para cada encerrado preservado, há `turnos_do_colab`, `dia_das_batidas`, `dias_encerrados` e potencial D4 (`:214-220`). `turnos_do_colab` consulta batidas/células/escalas (`app/ponto/turnos.py:1858-1886`); `dias_encerrados` recarrega células/escalas mesmo recebendo turnos (`:2129-2132`); D4 consulta célula e recarrega turnos/dias numa janela singleton (`app/ponto/status_dia.py:171-176`). A quantidade medida não está no dado entregue [não verificado].

**Mordida individual dos 16 testes**, por inspeção das asserções — execução [não verificado]: [nao verificado]

| Teste (sufixo) | O que derruba; limite | [nao verificado]
|---|---|
| `dia_de_trabalho_antes...` | Retorno de `ausente`, perda de `previsto`, posto descoberto antes de começar: asserções diretas (`app/core/tests/test_e4_7iii_painel_e_presenca.py:88-98`). |
| `folga_de_hoje...` | Retorno de `status='folga'` ou perda de previsão folga (`:100-104`). |
| `contadores_e_filtros...` | Reintrodução das chaves removidas no retorno do serviço; não renderiza filtro/card (`:106-114`). |
| `encerrado_de_turno_antes...` | Exposição do D4 antes do fim previsto; isoladamente passa se todo D4 for removido, por exigir `None` (`:120-131`). |
| `retratada_unica...` | Volta da consulta crua da atividade: espiona o argumento `teve_atividade`; par vivo impede apagar todas (`:133-157`). |
| `dia_encerrado_mostra...` | Remover D4 ou devolver status/fonte diferente; contrapartida positiva ao teste anterior (`:163-172`). | [nao verificado]
| `painel_e_PRESENCA...` | Remover categorias/exclusões ou permitir novas derivações na cópia mínima; não valida semântica de presença (`:198-209`). |
| `presenca_que_volta...ausente` | Muta literal no código copiado e exige retorno à catraca; verifica detector, não execução do painel (`:211-215`). |
| `presenca_que_volta...folga` | Idem, literal folga (`:217-221`). |
| `previsao_que_diz...` | Idem, previsto→ok (`:223-227`). |
| `noturno_com_pausa...` | Retorno à data civil da volta da pausa e perda do D4 do turno: exige data D, `ok`, `lavrado` (`:241-266`). |
| `filtro_que_exclui...` | Reintrodução de custo por encerrado antes do filtro ou de juiz singular por candidato: compara 3/6 encerrados, filtro normal (`:299-305`). |
| `turno_vivo_em_lote...` | Custo adicional por linha fora das quatro funções mockadas; deixa explicitamente todo D4 sem fiscalização de crescimento (`:292-297,307-318`). |
| **`MEDIDA_residuo...`** | **Não é tripwire da cura: só exige `n6>=n3`. Remover todo D4 permite `n6==n3` e passa; crescimento arbitrariamente maior também passa** (`:320-329`). |
| `dois_previstos_um_presente...` | Reintroduzir condição cobertura descoberto tira botão do ausente parcial; par presente impede botão indiscriminado (`:335-362`). |
| `status_ausente_ou_folga...` | Remover normalização faz conjuntos diferentes; caso encerrado preserva filtro válido. Não testa GET/contexto/HTMX (`:371-384`). |

**ALLOWLIST / LOOKUP_ALLOWLIST.** Escritor das duas é o próprio módulo de teste, via literais (`app/ponto/tests/test_contract_no_batida_date.py:10-72,98-154`). Leitores: detector novos `.date()` (`:233-236`), novos lookups (`:238-245`), entradas podres de cada lista (`:247-278`). Remover painel da primeira é coerente com a troca de `data_turno` para juiz (`app/core/services/painel_op.py:217,230`); manter na segunda corresponde aos lookups efetivos de geofence e atividade (`:104,274`), com categoria residente painel hoje (`app/ponto/tests/test_contract_no_batida_date.py:126-129`). O detector `.date()` segue referências sintáticas a timestamp, wrappers e variáveis locais (`:179-206`); não é prova semântica de todo caminho indireto.

**Autoridades e duplicação.** Registro vivo manda dia da batida para `turnos_do_colab`, turno aberto para `turno_aberto_de`, dia encerrado para `dia_encerrado`, status para D4 (`app/core/juizes.py:173,177,180,204`). Plural declara carregar dados para o mesmo `_turno_aberto_puro` e o chama, sem novo predicado (`app/ponto/turnos.py:2266-2275,2330-2334`). `dia_das_batidas` adapta turnos recebidos e também atribui dia civil à batida não reclamada — vale examinar esse segundo ramo no universo do painel (`:2855-2878`). O histórico de sua declaração indevida como segundo juiz é documentado no selo, sem provar erro novo desta chamada (`bin/tests/test_juiz_novo_tem_corte.sh:6-11`).

**Medidor não garante fonte única.** Categoria só testa palavras permitidas, não de qual autoridade derivaram (`app/core/medida_leitor_do_dia.py:366-375`). A exclusão de painel permite presença com seus literais e a de previsão permite três palavras (`:269-307`); uma segunda derivação com as mesmas palavras passa por esse predicado. O instrumento declara cegueira para indireção, booleanos, templates/JS e decisões que reutilizam quantidade de literais (`:334-343`). Previsão ternária continua em dois lugares (`app/escala/views.py:54`; `app/core/services/painel_op.py:35`), ambas perguntando `EC.eh_dia_trabalho` (`:40`; `:34`), sem novo juiz de aritmética demonstrado por esse ternário.

**Escritor/relógio/trava.** Mudanças produtivas do painel constroem dicionários e retornam contexto; não introduzem gravação de estado ou bloqueio transacional (`app/core/services/painel_op.py:225-241,294-315`). Relógio é capturado uma vez na borda do serviço (`:53-54`) e passado ao plural/encerramento/D4 (`:98,219-220`); os defaults internos `timezone.now()` não são usados por essas chamadas explícitas (`app/ponto/turnos.py:2124-2125,2295-2296`; `app/ponto/status_dia.py:169-170`). A view de minha escala conserva `datetime.date.today()` (`app/escala/views.py:26`), e não há caso UTC/local cruzando dia nos freezes novos (`app/core/tests/test_e4_7iii_painel_e_presenca.py:85,117,160,238,269,332,365`).

Pré-requisitos estão presentes na cópia: D4 (`app/ponto/status_dia.py:157`), plural (`app/ponto/turnos.py:2266`), categorias previsão/presença (`app/core/medida_leitor_do_dia.py:266-274`) e respectivas exclusões (`:297-307`). Ordem histórica de pouso, RED antes de cura e passagem das travas [não verificado]; a cópia não possui metadados Git utilizáveis [não verificado].

**Leitores adicionais das guardas:** suíte consome o medidor em `app/core/tests/test_medida_leitor_do_dia.py:29`, verifica novos/sumidos/sítios/exclusões em `:39-55,73-83`; host carrega o mesmo módulo e publica sua lista e alarmes em `bin/leitor_dia_estado.py:25-35,46-69`; selo host exige medidor único e testa catraca/plantas (`bin/tests/test_leitor_dia_estado.sh:25-43,45-71`). Core integra labels da suíte (`bin/regua.sh:23`).

## 31/10 17:37 · sessao fe7ba70ebfc3 · - · - · origem=Ronald (tmux)

A3 e A4 têm cura no caminho pedido: Designar usa `previsto_agora and not status`, sem depender da cobertura; filtro desconhecido vira `todos` antes de recortar linhas. A execução dos testes está [não verificado]. (`app/templates/core/_painel_card.html:69`; `app/core/services/painel_op.py:64`; `app/core/services/painel_op.py:202`)

Mapa de escritores/leitores:

| Campo | Escritor | Leitores |
|---|---|---|
| `status` | Ramos `normal/geofence/turno_aberto/encerrado/None`, `painel_op.py:172–200`; publicação `:232` | Filtro `:202`, contadores `:245–248`, agregado geofence `:288`, ordenação `:296`; card `:6`, `:36`, `:54`, `:66–69` |
| `previsao` | `_previsao_de_hoje`, `painel_op.py:27–35`; publicação `:239` | Avatar e badge, `_painel_card.html:54`, `:66–67`; novos testes `test_e4_7iii_painel_e_presenca.py:95`, `:104` | [nao verificado]
| `dia` | `None`, depois D4 sob portão, `painel_op.py:212–220`; publicação `:240` | Tooltip, texto do badge, `_painel_card.html:66–67`; testes `test_e4_7iii_painel_e_presenca.py:131`, `:170–172`, `:264–266` | [nao verificado]
| `data_turno` | Turno vivo, depois mapa do juiz, fallback hoje, `painel_op.py:230` | Testes `test_painel_op_turno_aberto_ancora.py:50`, `test_painel_op_turno_pelo_juiz.py:80`, novo `test_e4_7iii_painel_e_presenca.py:263`; consumidor de produção fora do serviço não identificado [não verificado] | [nao verificado]
| `filtro_status` | GET página `core/views.py:627`; GET lote `:609`; normalização serviço `painel_op.py:64–65` | Serviço `:202`, `:284`; contexto da página mantém valor bruto `core/views.py:669`; lotes preservam GET em `_qs_sem_offset_pn`, `:580–585` |
| `statuses` | `painel_op.py:56–61` | Validação `:64`, retorno `:314`; view `core/views.py:639`, contexto `:672`; uso no template atual não identificado [não verificado] |
| `contadores` | `painel_op.py:244–249` | `core/views.py:637`, `:670`; `_painel_stats.html:32–35`, incluído pela página `painel_operacional.html:122` e fragmento `_painel_fragment.html:2–3` |

Os caminhos de produção que chamam o serviço são a página e o lote; o lote reconstrói o painel inteiro e só fatia os postos depois. (`app/core/views.py:607`; `app/core/views.py:632`; `app/core/views.py:610–615`)

Os `data-*` do card:

- `data-search` recebe posto/empresa; busca lê `dataset.search`. (`app/templates/core/_painel_card.html:3`; `app/templates/core/painel_operacional.html:198`)
- `data-cobertura`, `data-tem-geofence`, `data-tem-aberto` recebem os agregados/status; filtro JS lê os três. (`app/templates/core/_painel_card.html:4–6`; `app/templates/core/painel_operacional.html:176–182`)
- Designar publica titular ID/nome, posto ID/nome e empresa ID; JS copia para `estado`. (`app/templates/core/_painel_card.html:69`; `app/templates/core/painel_operacional.html:280–285`)
- GET de disponíveis envia titular, posto e janela; endpoint lê esses parâmetros. (`app/templates/core/painel_operacional.html:343`; `app/ponto/views.py:3065–3068`)
- POST envia titular, substituto, posto, escala e janela; view delega à porta `designar_cobertura`. Empresa ID não integra esse POST. (`app/templates/core/painel_operacional.html:410–421`; `app/ponto/views.py:3027–3038`)
- O botão não transporta `data_turno`; a janela nasce do relógio do navegador. (`app/templates/core/_painel_card.html:69`; `app/templates/core/painel_operacional.html:293–301`)

Resíduos concretos fora do diff:

1. **Filtro URL válido desaparece no refresh:** página aceita `?status=encerrado`, mas `hx-get` periódico inclui apenas `fragment` e empresa; após atualização, o serviço recebe `todos`. (`app/core/views.py:627`; `app/templates/core/painel_operacional.html:147`; `app/core/views.py:627`)
2. **Dois abertos no mesmo posto somem do filtro Aberto:** template produz `"11"` e JS exige exatamente `"1"`. (`app/templates/core/_painel_card.html:6`; `app/templates/core/painel_operacional.html:178`)
3. **Lotes chegam fora do filtro/busca ativo:** reaplicação acontece somente quando `afterSwap.target.id === 'pop-body'`; sentinela dos lotes usa `hx-target="this"`. (`app/templates/core/painel_operacional.html:161–166`; `app/templates/core/_painel_lote.html:7–8`)
4. **Janela de Designar mistura data UTC e hora local:** `toISOString().slice(0,10)` fornece a data e `toTimeString()` a hora. Exemplo deduzido: abrir às 22h de 31/10 em UTC−3 gera início padrão 23h com data UTC 01/11, deslocando um dia; ocorrência real [não verificado]. (`app/templates/core/painel_operacional.html:294–301`)
5. **Contexto não reflete a normalização A4:** o serviço usa `todos`, enquanto `filtro_status` no contexto e `qs_lotes` continuam com o parâmetro bruto; não há leitor atual identificado que torne isso quebra visual [não verificado]. (`app/core/services/painel_op.py:64–65`; `app/core/views.py:669`; `app/core/views.py:580–585`)
6. **Selo antigo de crescimento não ganhou encerrados:** fixture de 45 e mais 100 cria colaboradores/escalas, sem batidas; portanto não exercita D4 por linha. (`app/core/tests/test_selo_lotes_painel.py:41–53`; `app/core/tests/test_selo_lotes_painel.py:95–106`)

Instrumentos fora do diff:

- Registro vivo mantém pendências dos quatro contadores, badge de aberto, `data-tem-aberto` e filtro JS; não foram declaradas curadas. (`app/core/juizes.py:715–716`; `:727–732`)
- `bin/leitor_dia_estado.py` carrega o mesmo medidor e chama `censo`; `bin/gerar_estado.py` usa seu resultado. (`bin/leitor_dia_estado.py:25–35`; `bin/gerar_estado.py:284–291`)
- `bin/sitios_por_pergunta.py` lê `core/juizes.py` por AST para produzir o inventário. (`bin/sitios_por_pergunta.py:22`; `:32–43`)
- O esmeril de batida não inclui `painel_op.py` na superfície de apuração, portanto não constitui proteção dessa leitura no painel. (`bin/esmeril_batida_censo.py:79–87`)
- O tripwire de contagem de pendência inclui o construtor do painel entre os leitores obrigados a usar `qs_na_fila`. (`app/core/tests/test_tripwire_contagem_de_pendencia.py:44–58`)

ALLOWLIST/LOOKUP_ALLOWLIST:

- A primeira controla `.date()` sobre timestamp via visitante AST; novos ofensores e entradas podres têm testes distintos. A retirada de `painel_op.py` é coerente com a troca por `dia_das_batidas`. (`app/ponto/tests/test_contract_no_batida_date.py:43–45`; `:187–217`; `:233–236`; `:247–264`)
- A segunda controla lookups ORM `timestamp__date`; painel permanece residente para “hoje”, compatível com os lookups geofence/atividade ainda existentes. (`app/ponto/tests/test_contract_no_batida_date.py:126–129`; `:157–175`; `app/core/services/painel_op.py:104`; `:274`)
- Novos lookups e entradas podres têm asserções próprias; não foi ampliada a exceção do painel no diff fornecido. (`app/ponto/tests/test_contract_no_batida_date.py:238–245`; `:266–278`)

## 31/10 17:09 · sessao 45436fe12f28 · E4-7-SAIDA-III · verificador · origem=Code

Encontrei três problemas relevantes: **D4 consultado pela data errada no noturno**, **N+1 novo nas linhas encerradas** e **perda do botão “Designar” em cobertura parcial**. As causas estão em `app/core/services/painel_op.py:194`, `app/core/services/painel_op.py:195` e `app/templates/core/_painel_card.html:69`.

Não há casos individuais de produção no material fornecido; incidência e impacto em produção são **[nao verificado]**. Os contraexemplos abaixo são deduções do código, sem execução contra banco **[nao verificado quanto à reprodução]**.

**1. Quem escreve e quem lê**

| Campo/valor | Quem escreve ou fornece | Quem lê |
|---|---|---|
| `status`: `normal`, `geofence`, `turno_aberto`, `encerrado`, `None` | O próprio painel escolhe as palavras: presença pelo juiz, alerta geo e comparação com fim previsto; `encerrado` significa entrada encontrada sem turno vivo. `app/core/services/painel_op.py:85`, `app/core/services/painel_op.py:158`, `app/core/services/painel_op.py:168`, `app/core/services/painel_op.py:181`. | Filtro, contadores, agregação e ordenação: `app/core/services/painel_op.py:198`, `app/core/services/painel_op.py:224`, `app/core/services/painel_op.py:273`, `app/core/services/painel_op.py:281`. Card, avatar, badge, indicador aberto e Designar: `app/templates/core/_painel_card.html:6`, `app/templates/core/_painel_card.html:36`, `app/templates/core/_painel_card.html:54`, `app/templates/core/_painel_card.html:67`, `app/templates/core/_painel_card.html:69`. |
| `previsao`: `folga`, `escala_suspeita`, `previsto`, `None` | `_previsao_de_hoje` traduz `EC.eh_dia_trabalho`; este lê `CelulaDia.trabalha`, com fallback aritmético. `app/core/services/painel_op.py:27`, `app/core/services/painel_op.py:218`, `app/escala/models.py:1066`. | Avatar e fallback do badge; testes novos. `app/templates/core/_painel_card.html:54`, `app/templates/core/_painel_card.html:67`, `app/core/tests/test_e4_7iii_painel_e_presenca.py:95`. |
| `dia` | Painel inicializa `None`, pergunta encerramento e chama D4. D4 lê a célula; o escritor persistente é a porta de lavra. `app/core/services/painel_op.py:192`, `app/ponto/status_dia.py:171`, `app/ponto/portas/celula.py:705`, `app/ponto/portas/celula.py:709`. | Card lê `rotulo` e `status`; teste também verifica `fonte`. `app/templates/core/_painel_card.html:66`, `app/templates/core/_painel_card.html:67`, `app/core/tests/test_e4_7iii_painel_e_presenca.py:171`. |
| `contadores` | Quatro somas sobre as linhas **já filtradas**. `app/core/services/painel_op.py:198`, `app/core/services/painel_op.py:223`. | View repassa; stats lê quatro chaves; teste verifica a remoção das antigas. `app/core/views.py:637`, `app/core/views.py:670`, `app/templates/core/_painel_stats.html:32`, `app/core/tests/test_e4_7iii_painel_e_presenca.py:109`. |
| `statuses` | Lista com quatro opções. `app/core/services/painel_op.py:230`. | View lê e repassa ao contexto. `app/core/views.py:639`, `app/core/views.py:672`. Consumidor final adicional **[nao verificado]**. |
| `ausentes` do posto | Removido; o dicionário atual não publica essa chave. `app/core/services/painel_op.py:279`. | Card atual usa `em_turno` e `geofence`; teste exige ausência da chave. `app/templates/core/_painel_card.html:12`, `app/core/tests/test_e4_7iii_painel_e_presenca.py:112`. |
| `bateu_hoje_ids` | `batidas_apuraveis(None)` filtradas pela data; a autoridade exclui retratadas. `app/core/services/painel_op.py:258`, `app/ponto/turnos.py:2845`. | `_teve_ativ`, depois `classificar_cobertura`. `app/core/services/painel_op.py:276`. |
| `PALAVRAS_DA_PRESENCA` | Constante com as quatro palavras. `app/core/medida_leitor_do_dia.py:272`. | `PALAVRAS_DA_CATEGORIA`, predicado do censo e teste novo. `app/core/medida_leitor_do_dia.py:274`, `app/core/medida_leitor_do_dia.py:367`, `app/core/tests/test_e4_7iii_painel_e_presenca.py:207`. |
| `NAO_E_LEITOR` | Registro de exclusões; inclui as duas funções do painel. `app/core/medida_leitor_do_dia.py:275`, `app/core/medida_leitor_do_dia.py:302`. | Censo aplica categoria/motivo e procura exclusões mortas; selo verifica categorias e sobreposição. `app/core/medida_leitor_do_dia.py:366`, `app/core/medida_leitor_do_dia.py:403`, `app/core/tests/test_medida_leitor_do_dia.py:73`. |
| `TETO_NAO_E_LEITOR` | Constante `10`. `app/core/medida_leitor_do_dia.py:309`. | Selo exige igualdade com o tamanho da lista. `app/core/tests/test_medida_leitor_do_dia.py:75`. |
| `DECLARADOS` | Cinco funções: duas do calendário, API, espelho administrativo e furos. `app/core/medida_leitor_do_dia.py:316`. | Censo calcula novos/sumidos/sítios alterados; selo cobra os resultados. `app/core/medida_leitor_do_dia.py:395`, `app/core/tests/test_medida_leitor_do_dia.py:39`, `app/core/tests/test_medida_leitor_do_dia.py:50`. |

Página, fragmento e lote passam pelo mesmo construtor; grade e lote incluem o mesmo card. `app/core/views.py:607`, `app/core/views.py:632`, `app/core/views.py:679`, `app/templates/core/_painel_grid.html:2`, `app/templates/core/_painel_lote.html:7`.

Fora do diff, testes continuam lendo presença, entrada e âncora: `app/core/tests/test_painel_op_turno_pelo_juiz.py:75`, `app/core/tests/test_painel_op_turno_aberto_ancora.py:49`, `app/core/tests/test_e4_7_painel_entrada_retratada.py:80`.

O publicador do ESTADO carrega **o mesmo medidor**, publica contagem, lista, alarmes e limitações; não calcula outro censo. A S3 também importa seu detector de dinheiro. `bin/leitor_dia_estado.py:32`, `bin/leitor_dia_estado.py:46`, `bin/gerar_estado.py:290`, `app/ponto/tests/test_s3_placar_exercicio.py:38`.

`core/juizes.py` não consome o payload: mantém pendências de contadores, badges e filtros por impressões do código. A obra alterou descrições, sem retirar essas pendências. `app/core/juizes.py:715`, `app/core/juizes.py:727`, `app/core/juizes.py:731`.

**2. Leitores que ainda esperam o contrato antigo**

Nos consumidores atuais rastreados, não encontrei expectativa residual de `status='ausente'`, `status='folga'` ou `p.ausentes`: o badge consulta `previsao`, os stats usam quatro contadores e o JS filtra descoberto/geofence/aberto. `app/templates/core/_painel_card.html:67`, `app/templates/core/_painel_stats.html:32`, `app/templates/core/painel_operacional.html:173`.

**Permanece uma incompatibilidade nas URLs:** ambas as views aceitam `?status=ausente` ou `?status=folga` e passam o valor sem normalização. O serviço elimina todas as linhas por igualdade e depois omite os postos vazios. Favoritos ou integrações reais usando essas URLs são **[nao verificado]**. `app/core/views.py:607`, `app/core/views.py:626`, `app/core/services/painel_op.py:198`, `app/core/services/painel_op.py:269`.

Não identifiquei API consumidora desse construtor. A API do espelho continua com classificação própria, declarada separadamente no medidor; consumidor externo do payload do painel é **[nao verificado]**. `app/core/medida_leitor_do_dia.py:323`.

**3. Classes de reprovação**

- **Segundo julgamento da data do turno:** a nova leitura do D4 usa a data civil da última entrada, enquanto o juiz registrado para pertencimento da batida é `turnos_do_colab`. No turno fechado, o painel também publica essa data civil em `data_turno`. `app/core/services/painel_op.py:194`, `app/core/services/painel_op.py:209`, `app/core/juizes.py:177`.

- **Escolha de vínculo fora da autoridade:** o painel seleciona por `ativa=True`, sem consultar vínculo do dia ou cobertura da data. Isso alimenta previsão, cobertura e fim previsto, embora essas autoridades estejam registradas. É caminho herdado, agora também alimentando o campo novo. `app/core/services/painel_op.py:97`, `app/core/services/painel_op.py:132`, `app/core/services/painel_op.py:218`, `app/core/juizes.py:1255`.

- **Previsão duplicada:** helper e `minha_escala` repetem a tradução True/False/None. Ambos perguntam trabalho à EC; portanto não há segunda aritmética de escala nesse ternário, mas existem duas traduções e escolhas independentes do vínculo. `app/core/services/painel_op.py:34`, `app/escala/views.py:25`, `app/escala/views.py:40`, `app/escala/views.py:54`.

- **Segundo escritor persistente:** não identifiquei novo escritor no caminho alterado. O painel monta dicionários e D4 consulta dados. O `.update(status_dia=...)` novo pertence à fixture, não ao serviço. `app/core/services/painel_op.py:192`, `app/ponto/status_dia.py:171`, `app/core/tests/test_e4_7iii_painel_e_presenca.py:76`.

- **Relógio:** a cadeia nova recebe o mesmo `agora` capturado pelo construtor; não encontrei leitura independente nova nesses chamados. Permanecem relógios do navegador e `date.today()` em `minha_escala`; discrepância por fuso do processo é **[nao verificado]**. `app/core/services/painel_op.py:53`, `app/core/services/painel_op.py:195`, `app/templates/core/painel_operacional.html:155`, `app/templates/core/painel_operacional.html:294`, `app/escala/views.py:26`.

- **N+1 novo, confirmado pela estrutura:** cada linha encerrada chama `dias_encerrados` sem alimentação; quando passa, D4 consulta célula, recarrega turnos e chama novamente `dias_encerrados`. Isso acontece antes do filtro de status. Quantidade e latência reais são **[nao verificado]**. `app/core/services/painel_op.py:193`, `app/core/services/painel_op.py:198`, `app/ponto/turnos.py:2129`, `app/ponto/status_dia.py:171`.

- **Selo de custo não cobre esse ramo:** as fixtures de crescimento criam vínculos sem batidas, portanto não exercitam linhas encerradas. Já existia outro N+1, chamando presença por colaborador, apesar da porta plural disponível. `app/core/tests/test_selo_lotes_painel.py:41`, `app/core/tests/test_selo_lotes_painel.py:95`, `app/core/services/painel_op.py:85`, `app/ponto/turnos.py:2266`.

Os REDs novos cobram as trocas literais `encerrado→ausente`, `encerrado→folga` e `previsto→ok`. Contudo, o medidor não verifica a origem dos campos: só examina certas produções de literais reconhecidos. `app/core/tests/test_e4_7iii_painel_e_presenca.py:211`, `app/core/medida_leitor_do_dia.py:194`, `app/core/medida_leitor_do_dia.py:200`.

Executei o censo somente em memória: **5 = status 5 + valor 0**, sem alarmes. Também mantiveram esse resultado duas alterações exclusivamente em memória: publicar `status` por `{'resultado': 'falta'}['resultado']` e substituir D4 por `{'status': 'falta', 'fonte': 'paralelo'}`. A limitação decorre do tratamento de dicionários/indireção; isso demonstra limite do medidor, não aprovação desses mutantes pela suíte completa. `app/core/medida_leitor_do_dia.py:200`, `app/core/medida_leitor_do_dia.py:335`, `app/core/medida_leitor_do_dia.py:368`, `app/core/medida_leitor_do_dia.py:389`.

**4. Contraexemplos concretos**

Todos abaixo são cenários sintéticos inferidos do código; reprodução executada e ocorrência em produção são **[nao verificado]**.

| Cadastro + batidas + hora | Diferença resultante |
|---|---|
| Ativo, 12x36 **19–07**, intervalo **00–01**, âncora 17/09; E17/09 19h, S18/09 00h, E18/09 01h, S18/09 07h; consulta em **18/09 08h**; D4 de 17/09 lavrado `ok`. | Painel pega a E de retorno, consulta encerramento de **18/09** e omite o D4 de **17/09**. A janela consultada termina em 19/09 07h; calendário agrupa o turno em 17/09. `app/core/services/painel_op.py:72`, `app/core/services/painel_op.py:194`, `app/escala/servico_jornada.py:61`, `app/ponto/turnos.py:2102`, `app/colaboradores/services/calendario.py:301`. |
| Ativo, EC 12x36 **07–19**, início **18/09**, âncora 17/09; sem célula nem batidas; consulta **17/09 09h**. | Painel anuncia `previsto` e conta previsão agora, embora o vínculo ainda não cubra a data. Calendário aplica a guarda de vigência e fica sem previsão. `app/core/services/painel_op.py:34`, `app/core/services/painel_op.py:139`, `app/escala/models.py:1260`, `app/colaboradores/services/calendario.py:766`. |
| Ativo, **07–19**, única E17/09 07h, sem S; consulta **17/09 23h01**. | Presença expira após fim+4h: painel diz `encerrado`. Calendário ainda lê `t.aberto` e diz `aberto`. Assim, `encerrado` não prova saída registrada. `app/ponto/turnos.py:1987`, `app/ponto/turnos.py:2008`, `app/core/services/painel_op.py:181`, `app/colaboradores/services/calendario.py:811`, `app/colaboradores/services/calendario.py:144`. |

A diferença de âncora noturna não está coberta pelo teste existente: ele termina sem a S final; os testes novos de D4 são diurnos. `app/core/tests/test_painel_op_turno_aberto_ancora.py:41`, `app/core/tests/test_e4_7iii_painel_e_presenca.py:120`, `app/core/tests/test_e4_7iii_painel_e_presenca.py:163`.

**5. O que quebra ou permanece quebrado fora do diff**

- **Designar em cobertura parcial — regressão:** dois previstos 07–19, às09 um presente e outro sem batida. O juiz retorna `parcial`, o painel converte para `coberto` e o botão exige `descoberto`; desaparece a ação para o titular sem presença. `app/ponto/selecao_periodo.py:246`, `app/core/services/painel_op.py:261`, `app/templates/core/_painel_card.html:69`.

- **Designar após saída antecipada:** E07/S08, consulta às09, jornada até19. O titular continua previsto e o posto pode estar descoberto, mas `status='encerrado'` impede o botão por `not c.status`. `app/core/services/painel_op.py:181`, `app/core/services/painel_op.py:217`, `app/templates/core/_painel_card.html:69`.

- **Filtro “Aberto” perde postos com dois abertos:** o template gera `data-tem-aberto="11"`; JS aceita somente `'1'`. `app/templates/core/_painel_card.html:6`, `app/templates/core/painel_operacional.html:178`.

- **Lotes novos escapam do filtro/busca:** a reaplicação só ocorre quando o alvo do swap é `pop-body`; sentinelas de lote substituem a si mesmas. `app/templates/core/painel_operacional.html:161`, `app/templates/core/_painel_lote.html:8`.

- **KPI “sem escala” mistura universos:** denominador conta todos os postos ativos; numeradores vêm do painel filtrado; o residual inclui encerrados e postos fora da empresa selecionada. `app/core/views.py:648`, `app/core/views.py:651`, `app/templates/core/_painel_stats.html:24`.

- **O dashboard ainda deriva ausentes separadamente:** `ausentes_hoje` não é consumidor da chave removida; continua sendo outra classificação por trabalho e ausência de entrada. A TV também mantém atividade por batidas cruas e previsão por hora do template. `app/core/views.py:388`, `app/core/views.py:404`, `app/core/views.py:792`, `app/core/views.py:848`.

- **Selo host inconsistente por construção:** a cópia mínima não inclui `ponto/status_dia.py`, agora obrigatório para carregar o vocabulário. Ela produz indisponibilidade, e depois a expressão aritmética recebe `NB=NAO`. É dedução estática; execução do script **[nao verificado]**. `bin/tests/test_leitor_dia_estado.sh:46`, `app/core/medida_leitor_do_dia.py:162`, `bin/leitor_dia_estado.py:35`, `bin/tests/test_leitor_dia_estado.sh:59`.

A suíte não foi executada: suas fixtures criam registros e arquivos temporários, incompatíveis com a restrição desta verificação. Resultado da suíte e REDs executados são **[nao verificado]**. `app/core/tests/test_e4_7iii_painel_e_presenca.py:45`, `app/core/tests/test_e4_7iii_painel_e_presenca.py:183`.

## 31/10 17:07 · sessao 9ff881fc009d · - · - · origem=Ronald (tmux)

Achados por leitura estática; execução, incidência e impacto em produção: **[nao verificado]**.

1. **P1 — D4 consultado no dia errado após intervalo noturno.** `entradas_hoje` conserva a última E cronológica, inclusive retorno de intervalo; a nova consulta usa `localdate(entrada.timestamp)` como dia. Não pergunta ao juiz o `data_turno` do turno fechado. O próprio campo `data_turno` repete esse fallback quando não existe turno vivo. (`app/core/services/painel_op.py:72-75,193-196,209`; ordenação da fonte: `app/ponto/turnos.py:2852`.)

   Contraexemplo: ativo, 12x36 19–07, intervalo 00–01, âncora 17/09; E17/09 19h, S18/09 00h, E18/09 01h, S18/09 07h; agora 18/09 08h; célula17 lavrada `ok`. O painel escolhe E18/09 01h e consulta encerramento de **18/09**, cuja janela noturna termina em **19/09 07h**; `dia=None`. O calendário agrupa esse turno em **17/09**. Portanto o painel omite o D4 do turno encerrado e apresenta entrada/data ancoradas no retorno do intervalo. É inferência estática, não reprodução executada. (`app/core/services/painel_op.py:155,181-196,207-209`; `app/escala/servico_jornada.py:61-93`; `app/ponto/turnos.py:2102-2105`; `app/colaboradores/services/calendario.py:297-302`.)

   A geometria dessa fixture já aparece no teste de âncora noturna, mas ele termina sem S final; os novos testes D4 usam somente 07–19. (`app/core/tests/test_painel_op_turno_aberto_ancora.py:3-6,33-42`; `app/core/tests/test_e4_7iii_painel_e_presenca.py:120-131,163-172`.)

2. **P1/P2 — N+1 novo por linha encerrada, com recarga duplicada.** Cada linha `encerrado` chama `dias_encerrados` sem células/turnos alimentados. Esse carregador consulta células, vínculos e turnos; se verdadeiro, o painel chama D4, que consulta a célula, recarrega turnos e chama novamente `dias_encerrados`. O custo cresce com o número de linhas encerradas, inclusive antes do filtro de status. Quantidade e latência reais: [nao verificado]. (`app/core/services/painel_op.py:193-199`; `app/ponto/turnos.py:2129-2133`; `app/ponto/status_dia.py:171-176`.)

   Já havia N+1 de presença: `turno_aberto_de` dentro de laço, embora exista `turnos_abertos_de` com carregamento plural. (`app/core/services/painel_op.py:85-86`; `app/ponto/turnos.py:2266-2274`.)

3. **P2 — previsão/cobertura continuam escolhendo vínculo por `ativa=True`, fora da autoridade registrada.** O painel monta um dict de vínculos ativos e usa diretamente esse vínculo em `_previsao_de_hoje`, `_previsto_agora` e no fim da janela. A autoridade cadastrada para vínculo do dia é `vinculo_do_dia`, que respeita geradora e vigência; para cobertura da data, `vinculo_cobre_a_data`. (`app/core/services/painel_op.py:97-102,132-147,152,176-178,218`; `app/core/juizes.py:1255-1256`; `app/escala/alimentacao.py:173-184,237-254`.)

   Contraexemplo: hoje 17/09 09h; ativo com EC 12x36 07–19, `ativa=True`, `data_inicio=18/09`, âncora17; sem célula nem batidas. Painel produz `previsao='previsto'` e `previsto_agora=True`; calendário guarda a vigência, obtém `eh_trabalho=None` e cai em `sem_dados`. Inferência estática: a aritmética da EC não testa `data_inicio`. (`app/core/services/painel_op.py:27-35,132-147`; `app/escala/models.py:1260-1263`; `app/colaboradores/services/calendario.py:766-778,118-143`.)

4. **Discrepância legada concreta: turno expirado sem S.** Ativo, 07–19, única E17/09 07h, agora17/09 23h01. O juiz de presença expira a aberta após fim+4h; painel passa a `encerrado`, embora não haja S. O calendário ainda usa `any(t.aberto)` e pinta `aberto`. O novo D4 pode aparecer nessa linha após o encerramento pelo relógio. Inferência estática; ocorrência real [nao verificado]. (`app/ponto/turnos.py:1987-2011,2097-2105`; `app/core/services/painel_op.py:181-196`; `app/colaboradores/services/calendario.py:144-145,811`.)

5. **Derivação paralela de previsão existe como tradução duplicada, não como nova aritmética de escala.** `_previsao_de_hoje` e `minha_escala` repetem o mesmo ternário True/False/None → previsto/folga/escala_suspeita; ambas delegam trabalho à EC. A duplicação é da tradução e da escolha/alimentação do vínculo. (`app/core/services/painel_op.py:27-35`; `app/escala/views.py:25,40,54`; `app/escala/models.py:1048-1073`.)

6. **Relógio do painel:** a nova cadeia usa um `agora` capturado no construtor e passado ao juiz de presença, ao encerramento e ao D4; não encontrei nova leitura independente nesses caminhos efetivamente chamados. `minha_escala` continua usando `datetime.date.today()`, diferente da data local derivada do instante no painel; discrepância depende do fuso do processo [nao verificado]. (`app/core/services/painel_op.py:53-54,86,195-196`; `app/ponto/status_dia.py:169-170`; `app/ponto/turnos.py:2124-2125,2261-2263`; `app/escala/views.py:26`.)

7. **Segundo escritor:** o construtor publicado não grava D4, célula ou batida; o novo caminho chama leitores. Os testes plantam `status_dia` diretamente por `.update()`, mas isso está no código de teste. Não atribuir ao serviço um segundo escritor por causa da fixture. (`app/core/services/painel_op.py:190-196`; `app/ponto/status_dia.py:171-185`; `app/core/tests/test_e4_7iii_painel_e_presenca.py:75-82`.)

8. **Selos que deixam passar a âncora errada:** o contrato de batida por data permite todo o arquivo `painel_op.py`, com comentário sobre transferência do legado; a nova derivação `_d_enc` fica dentro dessa permissão por arquivo. Os novos testes não incluem turno fechado com retorno de intervalo após meia-noite nem teto de consultas. (`app/ponto/tests/test_contract_no_batida_date.py:32-43`; `app/core/services/painel_op.py:194`; `app/core/tests/test_e4_7iii_painel_e_presenca.py:85-172`.)

## 31/10 17:06 · sessao c60283801c54 · - · - · origem=Ronald (tmux)

Mapa do medidor e dos selos:

- `PALAVRAS_DA_PRESENCA` é definido no módulo e lido por `PALAVRAS_DA_CATEGORIA`, pelo predicado de `censo` e pelo teste novo; contém normal/geofence/turno_aberto/encerrado. O detector, porém, só entrega palavras existentes no vocabulário geral; normal/geofence ficam fora dele por desenho. (`app/core/medida_leitor_do_dia.py:169`, `:194`, `:270-274`, `:367-368`; `app/core/tests/test_e4_7iii_painel_e_presenca.py:207`)
- `NAO_E_LEITOR` é definido em `app/core/medida_leitor_do_dia.py:275`; as duas novas entradas estão em `:302-307`. `censo` consulta categoria/motivo e calcula exclusões mortas (`:366-370`, `:403`); o selo valida categoria/motivo/tamanho e ausência de sobreposição com DECLARADOS (`app/core/tests/test_medida_leitor_do_dia.py:73-83`).
- `TETO_NAO_E_LEITOR=10` é definido em `app/core/medida_leitor_do_dia.py:309` e comparado ao tamanho exato da lista em `app/core/tests/test_medida_leitor_do_dia.py:75`.
- `DECLARADOS` mantém cinco funções: duas do calendário, API do espelho, espelho admin e relatório de furos. É definido em `app/core/medida_leitor_do_dia.py:316-332`; `censo` lê motivos, conta declarados e calcula novos/sumidos/sítios alterados (`:377`, `:395-401`). O selo cobra esses resultados (`app/core/tests/test_medida_leitor_do_dia.py:39-55`, `:69-71`).
- `bin/leitor_dia_estado.py` carrega o mesmo módulo e chama `censo`, sem detector paralelo (`:25`, `:32-35`). Publica contagem/lista/alarmes/pontos cegos (`:46-69`); `bin/gerar_estado.py` importa `ler`/`linha` (`:290-291`). A S3 importa o detector de dinheiro do mesmo módulo (`app/ponto/tests/test_s3_placar_exercicio.py:38`).
- `core/juizes.py` mantém painel, badges e filtro como pendências: contadores em `:715-716`, ausentes_hoje em `:717-718`, badge aberto e atributo de filtro em `:727-730`, JS em `:731-732`. A obra atualiza os motivos, não declara esses sítios curados. O juiz de turno é registrado em `:180` e `:683`; status do dia em `:173`; também constam de `app/docs/JUIZES.md:20` e `:74-75`.

Validação executada somente em memória, com `python3 -B`: o censo desta cópia retornou 5 = status 5 + valor 0, sem novos, sumidos, sítios alterados, exclusões mortas ou ilegíveis. Os campos provêm de `app/core/medida_leitor_do_dia.py:389-408`; o resultado de execução não foi gravado em arquivo [nao verificado].

Mordida real e limite:

- Os testes novos realmente plantam `encerrado → ausente`, `encerrado → folga` e `previsto → ok`, exigindo aparecimento em novos e incremento de contagem. (`app/core/tests/test_e4_7iii_painel_e_presenca.py:211-227`)
- O predicado **não verifica todos os valores de status nem a origem do dia**: considera apenas literais previamente reconhecidos pelo detector; valores dentro de dict não entram, assim como palavras fora do vocabulário. (`app/core/medida_leitor_do_dia.py:194`, `:200-212`, `:368`)
- Contra-mutações executadas somente em memória mantiveram 5 e todos os alarmes vazios:
  1. `painel_op.py:174`: `status = 'normal'` substituído por `status = {'resultado': 'falta'}['resultado']`.
  2. `painel_op.py:196`: chamada ao D4 substituída por `dia = {'status': 'falta', 'fonte': 'paralelo'}`.
  3. Mesmo sítio substituído por `dia = 'normal' if colab.id else 'geofence'`.
  
  Isso prova limite do medidor, não aprovação pela suíte completa; os resultados em memória não possuem arquivo de registro [nao verificado]. A causa está em `app/core/medida_leitor_do_dia.py:194-212`, e a limitação de indireção/templates/vocabulário é publicada em `:334-343`.

Quebra concreta fora do diff:

- O selo do host constrói cópia mínima com leitor, medidor e calendário, mas não copia `app/ponto/status_dia.py`. (`bin/tests/test_leitor_dia_estado.sh:46-50`)
- O medidor exige esse arquivo para obter o vocabulário antes do censo. (`app/core/medida_leitor_do_dia.py:124`, `:162-163`, `:352`)
- Portanto a cópia mínima retorna `medido=False`, e `campo` produz `NAO|...`; depois o teste avalia `$((NB + 1))` com NB=NAO sob `set -u`, falhando antes de demonstrar a planta +1. (`bin/leitor_dia_estado.py:35-37`; `bin/tests/test_leitor_dia_estado.sh:13`, `:33-36`, `:50`, `:59`)
- Essa falha é dedução estática; o script não foi executado porque cria e apaga arquivos temporários. (`bin/tests/test_leitor_dia_estado.sh:46-56`)

## 31/10 17:06 · sessao 41d28737325d · - · - · origem=Ronald (tmux)

Achados prioritários:

1. **Regressão no “Designar” em cobertura parcial.** Com dois colaboradores previstos 07–19, às 09h um tem E07 e o outro não bateu: o ausente recebe `status=None`, `previsto_agora=True`; cobertura retorna `parcial`, mas painel converte para `coberto`, e o botão novo exige `descoberto`. Portanto desaparece o caminho de designação para o segundo titular. Fontes: `app/core/services/painel_op.py:184`, `:217`, `:261`, `:275`; `app/ponto/selecao_periodo.py:244`; `app/templates/core/_painel_card.html:69`. A condição anterior, `status == 'ausente'`, está no diff fornecido [nao verificado quanto a execução].

2. **Encerramento antecipado também impede designação.** Titular 07–19 com E07/S08, às09: recebe `encerrado`; continua previsto, posto pode ficar descoberto, mas `not c.status` é falso. Fontes: `app/core/services/painel_op.py:181`, `:193`, `:217`; `app/templates/core/_painel_card.html:69`.

3. **URLs antigas continuam aceitas e agora esvaziam o painel.** Ambas as views passam `?status=ausente`/`folga` sem validação ao serviço; igualdade com o status novo não encontra ninguém, depois os postos vazios são omitidos. Fontes: `app/core/views.py:607`, `:626`, `:632`; `app/core/services/painel_op.py:198`, `:269`. Existência de favoritos/integração usando essas URLs: [nao verificado].

4. **Selo de N+1 não exercita o ramo novo.** A fixture de 45 colaboradores e os 100 acrescentados possuem vínculos, sem batidas; a comparação de contagem nunca testa colaboradores `encerrado`, cujo ramo chama os dois serviços individualmente. Fontes: `app/core/tests/test_selo_lotes_painel.py:41`, `:95`, `:111`; `app/core/services/painel_op.py:193`.

Mapa dos leitores:

| Campo | Escritor | Leitores encontrados |
|---|---|---|
| `status` | Serviço escolhe as quatro palavras ou `None`: `app/core/services/painel_op.py:168`, `:181`, `:188`, `:211` | Filtro/contadores/geofence/ordenação no próprio serviço: `:198`, `:224`, `:273`, `:281`; avatar, badge, aberto e botão: `app/templates/core/_painel_card.html:6`, `:36`, `:54`, `:66`, `:69` |
| `previsao` | `_previsao_de_hoje`, chamada ao criar linha: `app/core/services/painel_op.py:27`, `:218` | Avatar e fallback textual do badge: `app/templates/core/_painel_card.html:54`, `:66`, `:67` |
| `dia` | Condição encerrado + juiz + D4: `app/core/services/painel_op.py:192` | Título `rotulo` e texto `status`: `app/templates/core/_painel_card.html:66`, `:67` |
| `contadores` | Quatro somas: `app/core/services/painel_op.py:223` | View repassa: `app/core/views.py:637`, `:670`; template lê quatro: `app/templates/core/_painel_stats.html:32` |
| `statuses` | Lista de quatro: `app/core/services/painel_op.py:230` | View lê/repassa: `app/core/views.py:639`, `:672`. Leitor final atual não encontrado [nao verificado] |
| `ausentes` do posto | Removido no diff; dicionário atual não possui: `app/core/services/painel_op.py:279` | Card atual não pede a chave; cabeçalho usa em_turno/geofence: `app/templates/core/_painel_card.html:12`, `:34`, `:35` |
| `bateu_hoje_ids` | Apuráveis por data: `app/core/services/painel_op.py:258` | `_teve_ativ`, depois `classificar_cobertura`: `:276`, `:277` |

As duas entradas de produção encontradas são página/fragmento e lote; ambas usam o mesmo serviço. Página repassa linhas, postos, contadores e statuses; lote utiliza postos. Fontes: `app/core/views.py:607`, `:610`, `:632`, `:635`, `:637`, `:639`, `:679`. Os dois templates de grade/lote incluem o mesmo card: `app/templates/core/_painel_grid.html:2`, `app/templates/core/_painel_lote.html:7`. Rotas HTML: `app/core/urls.py:29`, `:30`; consumidor API do serviço não demonstrado [nao verificado].

**Não encontrei leitor atual do painel ainda esperando `ausente`/`folga` em `status` ou `ausentes`** nos consumidores acima: badge passa a consultar `previsao`; filtros JS só conhecem descoberto/geofence/turno_aberto; stats leem as quatro chaves. Fontes: `app/templates/core/_painel_card.html:67`; `app/templates/core/painel_operacional.html:173`; `app/templates/core/_painel_stats.html:32`.

Testes externos ao diff consomem `status=normal`, `encerrado`, `turno_aberto`, `entrada`, `data_turno`, `tempo_turno`, cobertura e chamados; não esperam explicitamente as palavras removidas. Fontes: `app/core/tests/test_painel_op_turno_pelo_juiz.py:75`, `:78`, `:84`; `app/core/tests/test_painel_op_turno_aberto_ancora.py:49`; `app/core/tests/test_e4_7_painel_entrada_retratada.py:80`, `:82`; `app/core/tests/test_painel_previsto_celula.py:55`; `app/core/tests/test_registrado_nao_e_pendencia.py:67`.

Fora do diff, permanecem:

- **Filtro “Aberto” perde posto com dois turnos abertos:** card concatena `11`, JS aceita apenas string `'1'`. Fontes: `app/templates/core/_painel_card.html:6`; `app/templates/core/painel_operacional.html:178`. Registro anterior do mesmo bug: `app/docs/CENSO_NARNIA.md:225`.
- **Filtros/busca não são reaplicados aos lotes:** handler `afterSwap` só trata alvo `pop-body`; sentinelas substituem `this` por novos cards. Fontes: `app/templates/core/painel_operacional.html:161`; `app/templates/core/_painel_lote.html:8`.
- **KPI “sem escala” inclui encerrados e ignora empresa no denominador:** total conta todos os postos ativos, enquanto numeradores vêm do serviço filtrado; residual é rotulado sem escala. Fontes: `app/core/views.py:648`, `:651`; `app/templates/core/_painel_stats.html:24`.
- **TV continua outra derivação:** usa batidas cruas para atividade e previsão por hora do template, sem fase/DNA; não consome o serviço operacional. Fontes: `app/core/views.py:792`, `:848`. Os status TV são `fora`, `normal`, `geofence`, `aberto`, consumidos por JS próprio. Fontes: `app/core/views.py:807`, `:816`, `:828`; `app/templates/core/painel_tv.html:161`, `:177`.
- **Relógio do cliente permanece em duas funções:** exibição e período default de designação. Fontes: `app/templates/core/painel_operacional.html:155`, `:294`.
- **Registro vivo ainda declara pendências de derivação no serviço/card/JS:** contadores, KPIs, badges e filtros continuam em `PENDENTES['tela']`. Fontes: `app/core/juizes.py:715`, `:719`, `:727`, `:729`, `:731`.

Não executei testes: os testes relevantes criam registros e os testes do medidor também criam arquivos temporários, incompatíveis com somente leitura. Fontes: `app/core/tests/test_selo_lotes_painel.py:32`, `app/core/tests/test_medida_leitor_do_dia.py:98`. Resultado de execução: [nao verificado].

_1172 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
