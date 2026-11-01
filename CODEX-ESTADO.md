# CODEX-ESTADO

> Ultima resposta processada: **01/11/2026 11:44** · publicado em **01/11/2026 11:49** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 01/11 11:44 · sessao 271244391423 · CONTENCAO-QUE-MORDE-V2 · verificador · origem=Code

**REPROVADO na pré-verificação.** Há uma corrida que permite autorizar o sétimo agente, dois mutantes novos que sobrevivem e duas âncoras antigas quebradas pelo diff. (`bin/disjuntor.py:1584`, `bin/disjuntor.py:1586`, `bin/tests/test_disjuntor.sh:2242`, `bin/tests/test_disjuntor.sh:2247`, `bin/tests/test_disjuntor.sh:2215`, `bin/tests/test_disjuntor.sh:2237`)

Resultado integral das suítes: **[não verificado]**. Elas criam e apagam arquivos, contrariando a regra desta chamada; a verificação ficou em leitura e simulações em memória. (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:51`, `bin/tests/test_hora_da_secao_nao_e_futura.sh:17`)

**1. Escritores e leitores.** O inventário dos acessos operacionais encontrados é:

| Estado/campo | Quem escreve | Quem lê |
|---|---|---|
| `reservas[*]` | `porta_agent` cria; a própria porta remove por resultado; `poda_reservas` remove por sessão. (`bin/disjuntor.py:1596`, `bin/disjuntor.py:1584`, `bin/disjuntor.py:1637`) | Porta verifica identidade, resultados e quantidade; poda examina as reservas. (`bin/disjuntor.py:1566`, `bin/disjuntor.py:1584`, `bin/disjuntor.py:1586`, `bin/disjuntor.py:1627`) |
| `reservas[*].sessao` | Vem do `session_id` do payload. (`bin/disjuntor.py:1554`, `bin/disjuntor.py:1596`) | Poda compara com a sessão recebida; selo confere `"S1"`. (`bin/disjuntor.py:1631`, `bin/tests/test_disjuntor.sh:1814`) |
| `reservas[*].agente` | Vem do `agent_id` **do chamador**, podendo ficar vazio no principal. (`bin/disjuntor.py:1596`) | Não participa da decisão de poda; o selo só verifica existência da chave, não seu valor. (`bin/disjuntor.py:1631`, `bin/tests/test_disjuntor.sh:1814`) |
| `b1[agent]` | `b1_do_agente` grava `checado`, `disparado` e `motivo`. (`bin/disjuntor.py:576`) | A mesma função lê antes e dentro da trava; `b1_disparados` alimenta o Stop. (`bin/disjuntor.py:564`, `bin/disjuntor.py:573`, `bin/disjuntor.py:585`, `bin/hook_stop_fila1.py:1054`) |
| `b3[obra].voltas` | `_voltas` inicializa/completa; reserva Codex incrementa `codex`; porta incrementa `agent`; `reabre` incrementa `extra`. (`bin/disjuntor.py:1453`, `bin/disjuntor.py:1457`, `bin/disjuntor.py:1486`, `bin/disjuntor.py:1594`, `bin/disjuntor.py:1667`) | Gates de teto/idempotência, mensagem de negativa e painel. (`bin/disjuntor.py:1482`, `bin/disjuntor.py:1484`, `bin/disjuntor.py:1592`, `bin/disjuntor.py:1464`, `bin/disjuntor.py:1723`) |
| `autorizados` | Porta acrescenta e conserva os últimos 500 identificadores. (`bin/disjuntor.py:1597`) | Porta aceita repetição de identificador já autorizado. (`bin/disjuntor.py:1567`) |
| `b4.negou_desde` | `pretool` grava no primeiro disparo; remove todo `b4` quando a medida deixa de negar. (`bin/disjuntor.py:2152`, `bin/disjuntor.py:2147`) | `pretool` consulta antes da limpeza e dentro do callback. (`bin/disjuntor.py:2143`, `bin/disjuntor.py:2151`) |
| `disjuntor_estado.json` | Escritor operacional físico: `_com_estado`, por temporário e `os.replace`, sob flock. (`bin/disjuntor.py:290`, `bin/disjuntor.py:312`) | `_com_estado` e `ler_estado`; o Stop também consulta metadados para excluir arquivos do próprio instrumento como prova de trabalho. (`bin/disjuntor.py:301`, `bin/disjuntor.py:336`, `bin/hook_stop_fila1.py:585`, `bin/hook_stop_fila1.py:590`) |
| `ultimo_clear.json` | `marco_de_clear.main`, com **trava própria**, escreve temporário e troca atomicamente. (`bin/marco_de_clear.py:55`, `bin/marco_de_clear.py:75`) | O escritor lê o marco anterior; `orcamento_contexto.ler_marco` atende avaliação, painel e Stop. (`bin/marco_de_clear.py:67`, `bin/orcamento_contexto.py:156`, `bin/orcamento_contexto.py:329`, `bin/orcamento_contexto.py:361`, `bin/hook_stop_fila1.py:862`) |

**Há escritores fora de `_com_estado` na cópia:** o escritor de `ultimo_clear.json`, por desenho, e escritores de fixtures dos dois arquivos. Exemplos do estado: inicialização, corrupção, substituição direta e restauração de cópia. (`bin/marco_de_clear.py:78`, `bin/tests/test_disjuntor.sh:52`, `bin/tests/test_disjuntor.sh:494`, `bin/tests/test_disjuntor.sh:1154`, `bin/tests/test_disjuntor.sh:1476`, `bin/tests/test_disjuntor.sh:1776`)

As fixtures também leem o estado diretamente e escrevem/manipulam `ultimo_clear.json` fora de sua porta operacional. (`bin/tests/test_disjuntor.sh:1151`, `bin/tests/test_disjuntor.sh:1483`, `bin/tests/test_disjuntor.sh:1814`, `bin/tests/test_orcamento_contexto.sh:93`, `bin/tests/test_orcamento_contexto.sh:384`, `bin/tests/test_orcamento_contexto.sh:473`, `bin/tests/test_orcamento_contexto.sh:488`)

Não localizei segundo escritor operacional desses arquivos em `app/` ou `.claude/`; completude contra acessos indiretos/dinâmicos **[não verificado]**. Os chamadores explícitos incluem a configuração dos hooks, o wrapper e o painel. (`.claude/settings.json:44`, `.claude/settings.json:57`, `.claude/settings.json:68`, `bin/codex_pergunta.sh:168`, `bin/gerar_estado.py:280`)

**2. O que permanece sob `_com_estado`.** Nas rotas examinadas, transcript, arquivos de subagente, glob e git foram retirados dos callbacks indicados; **“sob a trava só ler/gravar estado” continua falso**, porque permanecem outros arquivos e outra trava. (`bin/disjuntor.py:569`, `bin/disjuntor.py:1473`, `bin/disjuntor.py:1561`, `bin/disjuntor.py:1663`, `bin/disjuntor.py:1010`, `bin/disjuntor.py:1889`)

| Caminho | Operação que ainda acontece sob a trava |
|---|---|
| `porta_agent` | Lê `triagem.log` por `_b3_revalida` e `_cura_falta`. Foto, semente e ancestria são anteriores à trava. (`bin/disjuntor.py:1570`, `bin/disjuntor.py:1575`, `bin/disjuntor.py:983`, `bin/disjuntor.py:1347`, `bin/disjuntor.py:930`, `bin/disjuntor.py:1561`) |
| `b1_do_agente` | Usa a medida pronta, mas escreve anúncio de disparo na trilha. (`bin/disjuntor.py:576`, `bin/disjuntor.py:578`, `bin/disjuntor.py:145`) |
| `reserva_volta_codex` | Relê triagem por `_b3_revalida`; semente já veio pronta. (`bin/disjuntor.py:1473`, `bin/disjuntor.py:1476`, `bin/disjuntor.py:983`) |
| `reabre` | Callback manipula somente o estado e a semente pronta. (`bin/disjuntor.py:1663`, `bin/disjuntor.py:1665`) |
| `decide_b3` | Lê RELATO inteiro repetidamente, triagem, arquivos citados no DESENHO e, no legado, `disjuntor.log`; também escreve anúncios. (`bin/disjuntor.py:1010`, `bin/disjuntor.py:774`, `bin/disjuntor.py:1011`, `bin/disjuntor.py:1059`, `bin/disjuntor.py:690`, `bin/disjuntor.py:1065`, `bin/disjuntor.py:1217`, `bin/disjuntor.py:1086`) |
| `pretool` | Callback B4 grava estado e anúncio; a limpeza de B4 só altera estado. (`bin/disjuntor.py:2147`, `bin/disjuntor.py:2149`, `bin/disjuntor.py:2153`) |
| Stop | Herda B3; B2 escreve disparo; B4 abre e tenta flock compartilhado de `logs/sombra.lock` enquanto segura o estado. (`bin/hook_stop_fila1.py:1044`, `bin/hook_stop_fila1.py:1046`, `bin/hook_stop_fila1.py:1048`, `bin/disjuntor.py:1873`, `bin/disjuntor.py:1889`, `bin/disjuntor.py:356`, `bin/disjuntor.py:360`) |

**3. Estado ausente, caminho por caminho.** A tabela considera o disjuntor ligado; a chave válida desliga deliberadamente PreToolUse e a porta do wrapper. (`bin/disjuntor.py:2172`, `bin/disjuntor.py:1180`)

| Caminho | Consequência |
|---|---|
| Bash de subagente com transcript válido | `ler_estado` levanta; `pretool` converte em B1 “não sei = nega”, indicando entrega por `SubagentHandback`. (`bin/disjuntor.py:564`, `bin/disjuntor.py:2115`, `bin/disjuntor.py:2117`) |
| Bash de subagente sem transcript/id válido | B1 retorna antes de consultar estado; ausência não fecha esse caminho. DIFF/apply também suspende B1. (`bin/disjuntor.py:555`, `bin/disjuntor.py:559`, `bin/disjuntor.py:2113`) |
| Agent | Final reconhecido pode negar antes pelo B3. Nos demais, a ausência capturada no B4 segue até a porta, cuja exceção vira `PORTA-NAO-SEI`; fallback externo também nega Agent. (`bin/disjuntor.py:2138`, `bin/disjuntor.py:2143`, `bin/disjuntor.py:2176`, `bin/disjuntor.py:2196`) |
| Wrapper Codex verificador | B3 retorna `NAO_SEI`/rc 4; wrapper termina rc 3 antes de executar Codex. **Não chega à reserva nesse percurso.** Outros papéis não consultam essa porta B3. (`bin/disjuntor.py:1182`, `bin/disjuntor.py:1186`, `bin/disjuntor.py:1192`, `bin/codex_pergunta.sh:167`, `bin/codex_pergunta.sh:177`) |
| `reserva_volta_codex` direta | Captura a exceção e retorna rc 4, recusada. (`bin/disjuntor.py:1491`) |
| `--reabre` | Exceção sem tratamento em `reabre`/CLI: não reabre e termina com erro Python. (`bin/disjuntor.py:1669`, `bin/disjuntor.py:2429`, `bin/disjuntor.py:2450`) |
| `--triagem` | Sidecar/resposta inválidos recusam antes; com eles válidos, ausência vira rc 4 “triagem não gravada”. `--cura` também retorna 4. (`bin/disjuntor.py:891`, `bin/disjuntor.py:896`, `bin/disjuntor.py:914`, `bin/disjuntor.py:1388`) |
| Stop | Falhas B4/B2 viram `None` e seguem a política habitual. No ramo de recados, o aviso B3-NAO-SEI é montado, mas a leitura seguinte de B1 levanta e **descarta todos os recados**. (`bin/hook_stop_fila1.py:1043`, `bin/hook_stop_fila1.py:1052`, `bin/hook_stop_fila1.py:1054`, `bin/hook_stop_fila1.py:1064`) |
| Painel | Voltas mostram “não sei”; contador de final sem obra também mostra “não sei”. Outras medidas continuam vindas de suas fontes. (`bin/disjuntor.py:1720`, `bin/disjuntor.py:2326`, `bin/disjuntor.py:2397`, `bin/gerar_estado.py:281`) |
| `--cert-suite` | Não depende desse estado; pode certificar pelos hashes e log. (`bin/disjuntor.py:1273`, `bin/disjuntor.py:1294`, `bin/disjuntor.py:2424`) |
| `marco_de_clear.py` | Grava o marco; depois a poda falha, tenta registrar FALHA e termina rc 0 sem stdout. (`bin/marco_de_clear.py:78`, `bin/marco_de_clear.py:81`, `bin/marco_de_clear.py:31`, `bin/marco_de_clear.py:34`, `bin/marco_de_clear.py:84`) |

Nenhuma dessas rotas recria o JSON quando ele está ausente **na leitura**: a exceção antecede callback e gravação; `_com_estado` ainda pode criar diretório e `.lock`. (`bin/disjuntor.py:289`, `bin/disjuntor.py:303`, `bin/disjuntor.py:309`, `bin/disjuntor.py:312`)

O Stop pode liberar pelas condições habituais, inclusive PAREI e teto, sem impor fechamento pelo estado ausente. Portanto a política “ausente nega” não é universal entre os caminhos. (`bin/hook_stop_fila1.py:1226`, `bin/hook_stop_fila1.py:1284`, `bin/hook_stop_fila1.py:1064`)

Não há bloqueio total das ferramentas do principal: ferramentas sem `agent_id`, fora de Agent/Codex, saem antes do I/O; `SubagentHandback` fica fora do B1. A instalação/recuperação real pelo Ronald não foi fornecida **[não verificado]**. (`bin/disjuntor.py:2109`, `bin/disjuntor.py:2113`, `bin/disjuntor.py:275`)

**4. Medidas anteriores à trava: há janela de autorização indevida.** Este cenário foi reproduzido em memória, controlando a interlevação dos callbacks; ocorrência em produção **[não verificado]**. O mecanismo está na foto anterior à trava combinada com reservas que outros callbacks podem apagar. (`bin/disjuntor.py:1561`, `bin/disjuntor.py:1578`, `bin/disjuntor.py:1584`)

1. Existem cinco agentes de fundo vivos. A fotografa cinco e fica suspensa antes da trava. (`bin/disjuntor.py:1561`)
2. B autoriza e lança o sexto, gravando sua reserva; chega o resultado de lançamento. (`bin/disjuntor.py:1586`, `bin/disjuntor.py:1596`, `bin/disjuntor.py:1644`)
3. C fotografa seis, remove a reserva B por encontrar seu resultado e nega outra abertura por seis vivos. A remoção fica gravada mesmo com a negativa. (`bin/disjuntor.py:1584`, `bin/disjuntor.py:1587`, `bin/disjuntor.py:309`, `bin/disjuntor.py:315`)
4. A entra depois: sua foto ainda contém cinco, mas a reserva B já desapareceu. Soma cinco com zero e autoriza o **sétimo**. (`bin/disjuntor.py:1578`, `bin/disjuntor.py:1586`, `bin/disjuntor.py:1596`)

Isso contradiz diretamente o comentário “nunca a menos”: a própria foto antiga não apaga a reserva nova, mas **outro chamador a apaga**. (`bin/disjuntor.py:1582`, `bin/disjuntor.py:1584`)

Também há janelas nos outros insumos:

- **B1 nega quando medida nova liberaria:** mede ausência de arquivo recente; o agente grava arquivo antes da trava; callback persiste o disparo antigo. Medida posterior encontraria “arquivo novo”. (`bin/disjuntor.py:513`, `bin/disjuntor.py:521`, `bin/disjuntor.py:569`, `bin/disjuntor.py:576`)
- **B1 libera quando medida nova negaria:** mede ausência de tokens; tokens chegam antes da trava, mantendo as demais condições; callback persiste `False` antigo. Não há revalidação da medida. (`bin/disjuntor.py:511`, `bin/disjuntor.py:569`, `bin/disjuntor.py:576`)
- Um disparo concorrente já gravado é preservado, mas `checado` pode regredir quando uma medida antiga sobrescreve outra mais recente ainda não disparada. (`bin/disjuntor.py:574`, `bin/disjuntor.py:576`)
- **Ancestria:** cura com hash ainda não medido, acrescentada após `_ancestria`, causa negativa conservadora, mesmo se ancestral; a triagem é relida, mas o mapa não é atualizado. (`bin/disjuntor.py:1328`, `bin/disjuntor.py:1336`, `bin/disjuntor.py:1347`, `bin/disjuntor.py:1356`)
- **Semente:** material histórico surgido depois do glob pode ficar fora da inicialização/complementação única; contadores já existentes não são simplesmente substituídos pela semente velha. Ocorrência dessa chegada tardia no harness **[não verificado]**. (`bin/disjuntor.py:1423`, `bin/disjuntor.py:1438`, `bin/disjuntor.py:1452`, `bin/disjuntor.py:1454`)

**5. Poda e payload.** O payload **real** de SessionStart conter `session_id` é **[não verificado]**. A cópia mostra comandos configurados, leitura do stdin e payloads fabricados nos testes; isso não prova o formato efetivamente entregue pelo harness. (`.claude/settings.json:34`, `.claude/settings.json:44`, `.claude/settings.json:57`, `bin/marco_de_clear.py:44`, `bin/tests/test_disjuntor.sh:1817`, `bin/tests/test_disjuntor.sh:1822`)

Com sessão vazia:

- `clear` remove tudo. Cenário concreto possível: S1 ainda possui reservas de chamadas síncronas vivas e S2 executa clear sem payload; suas reservas desaparecem da conta global. Com sessão preenchida, também se remove toda outra sessão sem verificar se continua viva. Ocorrência simultânea real **[não verificado]**. (`bin/disjuntor.py:1630`, `bin/disjuntor.py:1632`, `bin/disjuntor.py:1586`)
- `compact` conserva tudo. Se ficaram seis reservas órfãs de S1 e o transcript atual não contém seus resultados, elas continuam impedindo Agent; não há expiração temporal nesse caminho. (`bin/disjuntor.py:1634`, `bin/disjuntor.py:1584`, `bin/disjuntor.py:1587`)

Quem cria é `porta_agent`; quem apaga entradas é a própria porta por resultado ou `poda_reservas`, chamada pelo marco depois de gravá-lo. `agente` não orienta nenhuma dessas remoções; a poda usa sessão/origem, sem TTL. (`bin/disjuntor.py:1596`, `bin/disjuntor.py:1584`, `bin/disjuntor.py:1630`, `bin/disjuntor.py:1637`, `bin/marco_de_clear.py:81`)

**6. P1: regex, corte e relógio.**

| Entrada | Resultado do regex |
|---|---|
| `- HH:MM`, `- HH:Mx` | Casa. (`bin/tests/test_hora_da_secao_nao_e_futura.sh:39`) |
| As mesmas após `**` | Casa. (`bin/tests/test_hora_da_secao_nao_e_futura.sh:39`) |
| `- 12:30h de trabalho` | Casa como `12:30`: não exige separação da unidade. (`bin/tests/test_hora_da_secao_nao_e_futura.sh:39`) |
| `- 08:00-12:00` | Casa como `08:00`: não distingue intervalo de instante. (`bin/tests/test_hora_da_secao_nao_e_futura.sh:39`) |
| `- 01/11` | Não casa: falta o `:` exigido. (`bin/tests/test_hora_da_secao_nao_e_futura.sh:39`) |

As quatro formas literais pedidas são cobertas. Duração/intervalo também podem ser acusados como instante; o bloqueio final do regex só exclui dígito ou `/`. Minuto exato é respeitado e `x` vira início da dezena. (`bin/tests/test_hora_da_secao_nao_e_futura.sh:39`, `bin/tests/test_hora_da_secao_nao_e_futura.sh:65`)

O dia mais próximo entre ontem/hoje/amanhã introduz uma exceção ao pedido absoluto: hora mais de aproximadamente 12 horas adiante pode virar passado. A limitação está declarada no próprio código. (`bin/tests/test_hora_da_secao_nao_e_futura.sh:35`, `bin/tests/test_hora_da_secao_nao_e_futura.sh:69`)

`DESDE_LINHA` implementa um corte fixo em **01/11/2026 11:23**, pela referência temporal calculada. Ser exatamente o instante do commit de nascimento é **[não verificado]**, pois os metadados desse commit não foram fornecidos. (`bin/tests/test_hora_da_secao_nao_e_futura.sh:34`, `bin/tests/test_hora_da_secao_nao_e_futura.sh:38`, `bin/tests/test_hora_da_secao_nao_e_futura.sh:67`)

Para linha não commitada, funciona **se** o blame entregar author-time de agora, como o comentário pressupõe; esse comportamento não foi exercitado nesta cópia **[não verificado]**. O juiz não identifica explicitamente o commit zero para distinguir esse caso. (`bin/tests/test_hora_da_secao_nao_e_futura.sh:25`, `bin/tests/test_hora_da_secao_nao_e_futura.sh:43`, `bin/tests/test_hora_da_secao_nao_e_futura.sh:50`, `bin/tests/test_hora_da_secao_nao_e_futura.sh:66`)

Para author-time futuro, `min(quando, agora)` **muda o contrato literal**: author-time 13h, agora 12h e lista 12h30 resulta vermelho, embora seja anterior ao author-time. É uma contenção adicional contra relógio futuro, não equivalência à comparação exclusivamente pelo blame. (`bin/tests/test_hora_da_secao_nao_e_futura.sh:66`, `bin/tests/test_hora_da_secao_nao_e_futura.sh:70`)

**7. Mordida dos casos e mutantes.**

- **`v4-poda-tudo` sobrevive.** No clear do teste só existem reservas S1; apagar tudo satisfaz a expectativa. No compact só existem reservas S2; preservar tudo também satisfaz. Faltam sessões misturadas nas duas operações. (`bin/tests/test_disjuntor.sh:1813`, `bin/tests/test_disjuntor.sh:1817`, `bin/tests/test_disjuntor.sh:1821`, `bin/tests/test_disjuntor.sh:1822`, `bin/tests/test_disjuntor.sh:2242`, `bin/disjuntor.py:1630`)
- **`v4-ausente-vazio-leitura` sobrevive.** Fazer `ler_estado` retornar `{}` não libera os casos: Agent ainda falha no `_com_estado` intacto; B1 avança à medida e também falha nele, produzindo a negativa esperada. O teste não isola o contrato do leitor sem trava. (`bin/tests/test_disjuntor.sh:1831`, `bin/tests/test_disjuntor.sh:1834`, `bin/tests/test_disjuntor.sh:2247`, `bin/disjuntor.py:304`, `bin/disjuntor.py:564`, `bin/disjuntor.py:581`, `bin/disjuntor.py:1602`)
- **`i3-sem-cura` não chega ao comportamento mutante:** sua âncora tem zero ocorrências porque a chamada ganhou `anc`. (`bin/tests/test_disjuntor.sh:2237`, `bin/disjuntor.py:1575`)
- **`i11-sem-poda` também não chega:** `del res[k]` passou a ocorrer duas vezes. O executor exige exatamente uma ocorrência e acusa âncora inválida. Isso é falha da montagem do mutante, não morte pela costura retirada. (`bin/tests/test_disjuntor.sh:2215`, `bin/disjuntor.py:1585`, `bin/disjuntor.py:1637`, `bin/tests/test_disjuntor.sh:1900`)
- **`v4_trava_curta` aceita ausência de operação útil.** `_lento` exige apenas trava disponível e ausência de exceção; não exige que tenha lido/medido nem verifica o resultado da porta/B1. Também usa espera de 0,8s sem sincronização que confirme a entrada no trecho lento. Uma negativa precoce ou agendamento diferente pode escapar ao que se pretendia medir. (`bin/tests/test_disjuntor.sh:1856`, `bin/tests/test_disjuntor.sh:1864`, `bin/tests/test_disjuntor.sh:1872`)
- Os dois mutantes que recolocam leitura/medida sob a trava atingem a contenção pretendida **quando** a interlevação temporal fabricada ocorre; não cobrem as corridas da fotografia antiga descritas na pergunta 4. (`bin/tests/test_disjuntor.sh:2248`, `bin/tests/test_disjuntor.sh:2251`, `bin/tests/test_disjuntor.sh:1865`)
- **Caso 1b:** exige rc 0 para passado e rc 1 para futuro, exercitando comum/negrito com `x`; não testa futuro com minuto exato. Na checagem do mutante, porém, aceita qualquer rc diferente de 1, inclusive erro de execução: o esperado deveria ser especificamente 0. O mutante concreto usa substituição válida com âncora única. (`bin/tests/test_hora_da_secao_nao_e_futura.sh:95`, `bin/tests/test_hora_da_secao_nao_e_futura.sh:103`, `bin/tests/test_hora_da_secao_nao_e_futura.sh:104`, `bin/tests/test_hora_da_secao_nao_e_f

[truncado em 20000 de 23251 caracteres]

## 01/11 11:38 · sessao d0f934b2dfa2 · - · - · origem=Ronald (tmux)

Achado principal confirmado por simulação em memória: a foto antiga permite o 7º Agent. A fotografa 5 vivos; B reserva e lança o sexto; C fotografa 6, poda a reserva B e nega; A finalmente entra, soma sua foto de 5 com reservas vazias e autoriza outro. A foto não é versionada contra as podas concorrentes. Isso contradiz o comentário “nunca a menos”. Reprodução construída, ocorrência em produção [não verificado]. (`bin/disjuntor.py:1561`, `bin/disjuntor.py:1578`, `bin/disjuntor.py:1584`, `bin/disjuntor.py:1586`, `bin/disjuntor.py:1596`)

Q2: não encontrei transcript/subagente/glob/git/processo externo nos callbacks das rotas citadas. Porém “sob a trava só estado” é falso: há triagem, RELATO, arquivos citados pelo desenho, trilha de disparos e outra trava. (`bin/disjuntor.py:930`, `bin/disjuntor.py:1010`, `bin/disjuntor.py:1059`, `bin/disjuntor.py:690`, `bin/disjuntor.py:1217`, `bin/disjuntor.py:1086`, `bin/disjuntor.py:1889`)

- `porta_agent`: `_b3_revalida` e `_cura_falta` leem triagem sob trava; foto, semente e git ficam antes. (`bin/disjuntor.py:1561`, `bin/disjuntor.py:1562`, `bin/disjuntor.py:1563`, `bin/disjuntor.py:1570`, `bin/disjuntor.py:1575`)
- `b1_do_agente`: medida fora; sob trava escreve disparo na trilha. (`bin/disjuntor.py:569`, `bin/disjuntor.py:578`, `bin/disjuntor.py:145`)
- `reserva_volta_codex`: semente fora, triagem dentro. (`bin/disjuntor.py:1473`, `bin/disjuntor.py:1476`, `bin/disjuntor.py:983`)
- `reabre`: somente estado e semente previamente medida no callback. (`bin/disjuntor.py:1663`, `bin/disjuntor.py:1666`)
- `decide_b3`: RELATO inteiro, triagem, arquivos referenciados, trilha antiga, segunda fotografia do RELATO e anúncios. Nenhum git na validação do desenho. (`bin/disjuntor.py:1010`, `bin/disjuntor.py:1011`, `bin/disjuntor.py:1059`, `bin/disjuntor.py:1065`, `bin/disjuntor.py:1080`, `bin/disjuntor.py:1094`)
- `pretool`: callbacks B4 leem/gravam estado e escrevem trilha. (`bin/disjuntor.py:2147`, `bin/disjuntor.py:2149`, `bin/disjuntor.py:2153`)
- Stop: herda os caminhos B3; `b4_stop` abre/testa `logs/sombra.lock` sob a trava do estado; `b2` anuncia disparo sob ela. (`bin/hook_stop_fila1.py:1044`, `bin/hook_stop_fila1.py:1046`, `bin/hook_stop_fila1.py:1048`, `bin/disjuntor.py:1889`, `bin/disjuntor.py:356`, `bin/disjuntor.py:360`, `bin/disjuntor.py:1873`)

Q4, demais janelas:

- B1 pode disparar indevidamente: mede ausência de arquivo recente, agente escreve arquivo antes da trava, callback grava a medida antiga e torna o disparo persistente. Medir depois da escrita retornaria “arquivo novo”. (`bin/disjuntor.py:513`, `bin/disjuntor.py:521`, `bin/disjuntor.py:569`, `bin/disjuntor.py:574`, `bin/disjuntor.py:576`)
- B1 pode autorizar indevidamente: medida encontra zero tokens; transcript recebe tokens antes da trava; callback grava `False` antigo. (`bin/disjuntor.py:511`, `bin/disjuntor.py:569`, `bin/disjuntor.py:576`)
- `checado` pode regredir: callback antigo sobrescreve uma medida negativa mais recente sem comparar `checado`. Disparo já gravado por concorrente é preservado. (`bin/disjuntor.py:574`, `bin/disjuntor.py:576`)
- Cura nova após a fotografia de ancestria nega conservadoramente, mesmo se a cura for ancestral; a triagem é relida sob trava e o hash novo falta no mapa. (`bin/disjuntor.py:1328`, `bin/disjuntor.py:1336`, `bin/disjuntor.py:1347`, `bin/disjuntor.py:1356`, `bin/disjuntor.py:1358`)
- Semente antiga é usada sem revarrer; ela só inicializa `voltas` ou completa `sem_transcript` uma vez. Material histórico que apareça depois da medição pode ficar fora da semente; ocorrência concreta no harness [não verificado]. (`bin/disjuntor.py:1423`, `bin/disjuntor.py:1438`, `bin/disjuntor.py:1452`, `bin/disjuntor.py:1454`)

Q5:

- Payload **real** de SessionStart com `session_id`: [não verificado]. Há configuração dos comandos e leitura do campo; os payloads do selo são fabricados. (`.claude/settings.json:34`, `.claude/settings.json:44`, `.claude/settings.json:57`, `bin/marco_de_clear.py:44`, `bin/marco_de_clear.py:62`, `bin/tests/test_disjuntor.sh:1817`, `bin/tests/test_disjuntor.sh:1822`)
- Sessão vazia + clear apaga tudo. Caso de código: clear sem payload em S2 remove reservas síncronas ainda ativas de S1 concorrente, permitindo subcontagem. Mesmo com sessão preenchida, poda remove toda outra sessão sem verificar se continua viva. (`bin/disjuntor.py:1630`, `bin/disjuntor.py:1632`, `bin/disjuntor.py:1633`)
- Sessão vazia + compact conserva tudo. Caso de código: seis reservas órfãs de outra sessão permanecem e não têm resultados no transcript novo; a porta continua negando por seis vivos. (`bin/disjuntor.py:1634`, `bin/disjuntor.py:1584`, `bin/disjuntor.py:1586`)
- Quem grava: `porta_agent`; quem remove: poda por resultados em `porta_agent`, ou `poda_reservas`, chamada por `marco_de_clear`. Não apareceu outro apagador de `reservas` em código executável pesquisado em `bin/`, `app/`, `.claude/`. (`bin/disjuntor.py:1596`, `bin/disjuntor.py:1584`, `bin/disjuntor.py:1637`, `bin/marco_de_clear.py:81`, `bin/marco_de_clear.py:31`)
- `agente` guarda o `agent_id` do chamador; pode ficar vazio para o agente principal. Esse campo não participa da poda, que compara somente sessão. (`bin/disjuntor.py:1596`, `bin/disjuntor.py:1631`)
- Não há TTL na poda: campo `t` é gravado, mas a seleção usa sessão/origem. (`bin/disjuntor.py:1596`, `bin/disjuntor.py:1630`, `bin/disjuntor.py:1632`)

## 01/11 11:37 · sessao 7b68d8bd016d · - · - · origem=Ronald (tmux)

6. P1

- O regex casa as quatro formas pedidas: `- HH:MM`, `- HH:Mx`, `- **HH:MM`, `- **HH:Mx`; também casa os prefixos de `- 12:30h de trabalho` e `- 08:00-12:00`, pois só proíbe dígito ou `/` imediatamente após o minuto. `- 01/11` não casa. Portanto duração/intervalo podem ser julgados como instante, produzindo falso positivo conforme a referência. `bin/tests/test_hora_da_secao_nao_e_futura.sh:39,61-71`.
- Minuto exato é respeitado; `x` vira início da dezena. Horas/minutos inválidos são ignorados, embora contem como linhas vistas. `bin/tests/test_hora_da_secao_nao_e_futura.sh:63-67`.
- A data é inferida pelo dia mais próximo entre ontem/hoje/amanhã; futuro acima de 12h pode ser interpretado como passado, limitação expressamente declarada. Empate exato favorece ontem por ordem do gerador. `bin/tests/test_hora_da_secao_nao_e_futura.sh:35-37,68-69`.
- Para linha não commitada, o código confia no `author-time` produzido pelo blame e aplica `min(quando, agora)`: funciona se esse tempo for “agora”, como o comentário pressupõe; esse comportamento real do Git não foi medido nesta cópia. `bin/tests/test_hora_da_secao_nao_e_futura.sh:25-26,43-44,50,66`; [não verificado].
- Para commit com author-time futuro, o juiz compara com agora, **não com o author-time pedido literalmente**: author-time 13h, agora 12h, lista 12h30 ⇒ vermelho; pela referência literal de 13h seria passado. `bin/tests/test_hora_da_secao_nao_e_futura.sh:66-70`.
- O corte é um instante fixo, 01/11/2026 11h23, aplicado à referência já limitada por agora. Isso implementa corte temporal por author-time; se corresponde ao commit de nascimento não é comprovável pelo anexo sem metadados de commit. `bin/tests/test_hora_da_secao_nao_e_futura.sh:34,38,66-67`; [não verificado].

7. Testes/mutantes [nao verificado]

**Dois mutantes novos sobrevivem por construção:**

- `v4-poda-tudo` troca `if sessao` por `if False`. No teste, clear S2 encontra exclusivamente reservas S1, então apagar tudo dá o resultado esperado; compact S2 encontra exclusivamente reservas S2, então preservar tudo também dá o resultado esperado. Faltam reservas da sessão viva durante clear e reservas estrangeiras durante compact. `bin/tests/test_disjuntor.sh:1813-1824,2242-2244`; `bin/disjuntor.py:1630-1637`. Confirmei a equivalência executando somente a função extraída em memória, com estado em dict e escritor substituído por callback sem arquivo.
- `v4-ausente-vazio-leitura` faz `ler_estado` retornar `{}`, mas o teste ainda obtém as mesmas negativas pelo `_com_estado` intacto: Agent chega à porta; B1 avança à medida e chega à trava; ambos encontram ausência e levantam o mesmo RuntimeError. O arquivo continua ausente. Portanto o caso não isola a leitura sem trava. `bin/tests/test_disjuntor.sh:1830-1836,2247`; `bin/disjuntor.py:303-304,564-581,1602`.

**Âncoras quebradas, não mortes funcionais:**

- `i3-sem-cura` procura a chamada antiga sem `anc`; a atual contém `, anc`, portanto a âncora tem zero ocorrências. `bin/tests/test_disjuntor.sh:2237`; `bin/disjuntor.py:1575`.
- `i11-sem-poda` procura `            del res[k]`, agora presente duas vezes, na poda por resultado e na poda SessionStart. `bin/tests/test_disjuntor.sh:2215`; `bin/disjuntor.py:1585,1637`.
- O executor exige exatamente uma ocorrência e acusa “âncora sumiu”; nesses dois casos o comportamento mutante nem é testado. `bin/tests/test_disjuntor.sh:1897-1904`.

**Ausência de sinal:**

- `_lento` exige somente que outra chamada consiga a trava e que a thread não lance exceção; não exige leitura efetiva, autorização ou resultado B1 esperado. Um ramo que devolva negativa cedo ou deixe de medir pode passar. Os mutantes declarados de mover leitura para dentro da trava exercitam contenção, mas o caso não comprova integralmente a operação útil. `bin/tests/test_disjuntor.sh:1856-1872,2248-2252`.
- O caso 1b tem verificações positivas de rc para passado e futuro e testa versões comum/negrito com `x`; não testa futuro com minuto exato. `bin/tests/test_hora_da_secao_nao_e_futura.sh:95-108`.
- A verificação do mutante P1 apenas rejeita rc=1; aceita rc=2/3/etc., permitindo que falha de execução pareça comprovar desligamento do ramo. O mutante concreto gerado é uma substituição válida e única, mas o oráculo deveria exigir rc=0. `bin/tests/test_hora_da_secao_nao_e_futura.sh:112-121`.
- `v4_poda` usa autorização com stdout vazio **e nenhuma FALHA nova**, melhor que ausência simples de saída; porém não verifica o valor de `agente`, apenas a existência da chave. `bin/tests/test_disjuntor.sh:104-106,1814,1820-1821`.

Não executei as suítes: elas criam/apagam arquivos temporários, contrariando a regra da chamada. `bin/tests/test_disjuntor.sh:39-41,50-52`; `bin/tests/test_hora_da_secao_nao_e_futura.sh:17-19,81-86`.

## 01/11 11:37 · sessao 44283db1341c · - · - · origem=Ronald (tmux)

**Q3 — ausência de `disjuntor_estado.json`:**

| Caminho | Resultado |
|---|---|
| PreTool Bash de subagente, transcript válido | `ler_estado` levanta; `pretool` transforma em B1 “não sei = nega” e pede `SubagentHandback` (`bin/disjuntor.py:564`, `:2114-2120`). |
| Bash subagente sem transcript/id válido | Retorna `False` antes de ler o estado; passa por B1. DIFF/apply também suspende B1 (`bin/disjuntor.py:555-560`, `:2112-2113`). |
| PreTool Agent | Leitura de B4 captura ausência; depois porta levanta e `main_pretool` nega `PORTA-NAO-SEI`. Exceção anterior também tem fallback que nega Agent (`bin/disjuntor.py:2142-2145`, `:2174-2178`, `:2196-2204`). Final reconhecido pode negar antes via B3 (`:2135-2140`, `:1112-1128`). |
| Wrapper Codex verificador | `estado_b3` recebe `NAO_SEI`; `b3_parada` retorna 4; wrapper converte em saída 3 antes de executar Codex. Nesse caminho a reserva sequer é chamada (`bin/disjuntor.py:1182-1192`; `bin/codex_pergunta.sh:167-178`). Chave desligada pula a porta (`bin/disjuntor.py:1180-1181`). Outros papéis não passam pelo B3 (`bin/codex_pergunta.sh:167-169`). |
| `reserva_volta_codex` direta | Captura exceção da trava/leitura; retorna `(4, recusada/não sei)` (`bin/disjuntor.py:1490-1495`). |
| `--reabre` | `_com_estado` levanta sem catch em `reabre` nem em `main`: traceback/saída Python não zero; não reabre (`bin/disjuntor.py:1663-1671`, `:2428-2430`, `:2449-2450`). |
| `--triagem` | Se sidecar/resposta já são inválidos, recusa antes; se válidos, ausência vira rc 4 “triagem não gravada”. Cura igualmente rc 4 (`bin/disjuntor.py:891-897`, `:913-916`, `:1387-1390`). |
| Stop hook | B4/B2 falham e viram `None`, mantendo caminho normal. Em `recados`, B3 gera aviso `NAO_SEI`, mas `b1_disparados` levanta logo depois e **descarta o aviso inteiro**, retornando `None` (`bin/hook_stop_fila1.py:1043-1064`; `bin/disjuntor.py:584-585`). Segue fila, PAREI, espera e teto habituais; não fecha pelo estado ausente (`bin/hook_stop_fila1.py:1205-1227`, `:1284-1304`). |
| Painel | `_voltas_txt` retorna “não sei”; `_b3_sem_obra` retorna `None`, mostrado “não sei”. Demais medidas de trilhas continuam independentes (`bin/disjuntor.py:1718-1727`, `:2324-2330`, `:2393-2398`). `gerar_estado` chama esse leitor (`bin/gerar_estado.py:279-283`). |
| `--cert-suite` | Não lê nem depende desse estado; pode certificar normalmente quando log/hashes atendem condições (`bin/disjuntor.py:1273-1301`, `:2424-2427`). |
| `marco_de_clear.py` | Grava `ultimo_clear.json`; depois poda falha pela ausência, registra FALHA quando consegue e termina rc 0, sem stdout. Não recria o estado (`bin/marco_de_clear.py:60-84`, `:30-37`). |

Nenhuma dessas rotas recria silenciosamente `disjuntor_estado.json`: ausência levanta antes de executar callback e antes do `json.dump/os.replace`; podem criar diretório e `.lock` (`bin/disjuntor.py:289-315`). A aplicação principal continua com ferramentas: sem `agent_id`, fora de Agent/Codex, retorna antes de I/O; `SubagentHandback` é excluído do B1 (`bin/disjuntor.py:2109-2113`). Recuperação automática do arquivo não existe nas rotas auditadas (`bin/disjuntor.py:303-304`, `:338-339`); instalação real pelo Ronald é apenas comentário, sem evidência fornecida [não verificado].

**Q1 — complemento de inventário:**

- Escritor operacional do arquivo de estado: `_com_estado`, `json.dump` + `os.replace`, com trava própria (`bin/disjuntor.py:290-315`). Leitor genérico fora da trava: `ler_estado` (`:332-344`). Há **escritores de fixtures fora da porta**, por exemplo `bin/tests/test_disjuntor.sh:52`, `:362`, `:494`, `:498`, `:836`, `:1439`, `:1453`, `:1470`, `:1476`, `:1480`, `:1774`; não são segundos escritores operacionais.
- `ultimo_clear.json`: escritor operacional separado `marco_de_clear.main`, com trava própria e leitura do anterior (`bin/marco_de_clear.py:55-78`). Leitor público `orcamento_contexto.ler_marco` (`bin/orcamento_contexto.py:152-164`), usado por `avaliar` (`:329`), painel OC (`:361`) e Stop (`bin/hook_stop_fila1.py:862`). Fixtures também escrevem: `bin/tests/test_orcamento_contexto.sh:84-94`, `:384`, `:473`, `:486`.
- `.claude/settings.json` apenas conecta SessionStart compact/clear e PreTool (`.claude/settings.json:44`, `:57`, `:68`); não fornece amostra do payload real do harness [não verificado].
- Para `reservas`, `autorizados`, `b1`, `voltas`, `b4.negou_desde`, a busca em código de `bin/`, `.claude/`, `app/` só encontrou acessos pertinentes em `disjuntor.py` e a asserção de dono do selo (`bin/tests/test_disjuntor.sh:1814`); completude contra acessos indiretos/dinâmicos [não verificado].
- `reservas[*].agente` é escrito, mas não usado para decidir poda; a poda compara somente `sessao` (`bin/disjuntor.py:1596`, `:1630-1631`).

## 01/11 10:53 · sessao 377e14165497 · GRADE-FOLGA-JUIZ · verificador · origem=Code

**O delta fecha um parâmetro sem fornecedor encontrado; não muda o retorno dos chamadores encontrados. O teste morde a restauração explícita do parâmetro, mas permite sua volta por `**kwargs`.** (`app/ponto/precedencia.py:259`, `app/ponto/precedencia.py:282`, `app/ponto/tests/test_grade_folga_juiz_sitios.py:181`)

1. **Quem escreve e quem lê os nomes tocados**

   | Nome | Escrita/definição → leitura |
   |---|---|
   | `folgas_lancadas`, parâmetro removido | Deixou de existir na assinatura; o nome permanece como variável local. (`app/ponto/precedencia.py:259`, `app/ponto/precedencia.py:282`) |
   | `folgas_lancadas`, variável local | Única atribuição na função: resultado do carregador; única leitura: `.get(data)`. (`app/ponto/precedencia.py:282`, `app/ponto/precedencia.py:342`) |
   | `folgas_lancadas_do_periodo` | Lê `FolgaDia` por pessoa e intervalo, ordena por `pk`, monta listas por data; fornece o mapa à atribuição local. (`app/ponto/precedencia.py:194`, `app/ponto/precedencia.py:206`, `app/ponto/precedencia.py:208`, `app/ponto/precedencia.py:282`) |
   | `celulas_geradoras` | Continua parâmetro independente: carga quando `None`, leitura da célula e fornecimento ao juiz do vínculo. (`app/ponto/precedencia.py:260`, `app/ponto/precedencia.py:315`, `app/ponto/precedencia.py:349`, `app/ponto/precedencia.py:351`) |
   | `SimpleTestCase` / `TestCase` | Importados; o primeiro passa a ser base da classe nova, o segundo continua base de `_Base`. (`app/ponto/tests/test_grade_folga_juiz_sitios.py:29`, `app/ponto/tests/test_grade_folga_juiz_sitios.py:38`, `app/ponto/tests/test_grade_folga_juiz_sitios.py:174`) |
   | Classe/método novos, `self`, `inspect`, `fatos_do_periodo` | Classe e método definidos; o método recebe `self`, importa `inspect` e a função, e os lê exclusivamente na asserção da assinatura. (`app/ponto/tests/test_grade_folga_juiz_sitios.py:174`, `app/ponto/tests/test_grade_folga_juiz_sitios.py:178`, `app/ponto/tests/test_grade_folga_juiz_sitios.py:179`, `app/ponto/tests/test_grade_folga_juiz_sitios.py:180`, `app/ponto/tests/test_grade_folga_juiz_sitios.py:181`) |

   Os quatro pontos de chamada de produção omitem `folgas_lancadas`:

   - Casca `fatos_do_dia`: só três posicionais. (`app/ponto/precedencia.py:256`)
   - Calendário: passa `lugar`, `feriados`, `vinculos`, `folgas_foto`; esta última continua distinta do argumento removido. (`app/colaboradores/services/calendario.py:604`, `app/colaboradores/services/calendario.py:605`, `app/colaboradores/services/calendario.py:606`)
   - Espelho: importa pelo alias `_fatos_periodo` e passa apenas `feriados` como opcional. (`app/ponto/services/espelho.py:657`, `app/ponto/services/espelho.py:659`)
   - Folha, `dias_com_furo`: só três posicionais. (`app/folha/services/sem_furo.py:25`)

   Os oito pontos de chamada em testes também omitem o removido: calendário; quatro testes pela célula, dos quais somente um passa `celulas_geradoras`; três testes de vínculo encerrado. (`app/colaboradores/tests/test_calendario_um_juiz.py:176`, `app/ponto/tests/test_precedencia_pela_celula.py:77`, `app/ponto/tests/test_precedencia_pela_celula.py:90`, `app/ponto/tests/test_precedencia_pela_celula.py:106`, `app/ponto/tests/test_precedencia_pela_celula.py:120`, `app/ponto/tests/test_precedencia_pela_celula.py:121`, `app/ponto/tests/test_precedencia_vinculo_encerrado.py:78`, `app/ponto/tests/test_precedencia_vinculo_encerrado.py:86`, `app/ponto/tests/test_precedencia_vinculo_encerrado.py:138`)

   Nenhuma dessas chamadas usa `**kwargs` ou expansão de dict. O command relacionado chama diretamente carregador e seletor, sem chamar `fatos_do_periodo`. (`app/ponto/management/commands/rejulgar_ausencia_em_folga.py:51`, `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:64`, `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:65`) Não encontrei outro chamador em commands, template tags ou dicts; chamadas construídas dinamicamente que escapem ao rastreio estático: **[não verificado]**.

2. **Valor publicado** [nao verificado]

   **Não muda por este delta nos chamadores encontrados, mantendo iguais entradas e fontes.** Todos omitem o argumento; pelo delta fornecido, antes isso acionava a carga por `None`, e agora a mesma carga é incondicional. O mapa segue para seleção diária, inclusão de folga nos fatos presentes e publicação de `tipo_do_dia`, `faixa`, `suprimidos` e `fatos.folga_lancada`. (`app/ponto/precedencia.py:282`, `app/ponto/precedencia.py:342`, `app/ponto/precedencia.py:358`, `app/ponto/precedencia.py:380`, `app/ponto/precedencia.py:398`, `app/ponto/precedencia.py:408`)

   Uma chamada que ainda fornecesse `folgas_lancadas=...`, inclusive `None` ou via dict expandido, passaria a receber `TypeError`; não há essa chamada entre as encontradas. (`app/ponto/precedencia.py:259`, `app/ponto/precedencia.py:260`) Valores efetivos de produção: **[não verificado]**.

3. **Mordida do teste** [nao verificado]

   **Sim:** restaurar qualquer parâmetro explícito chamado `folgas_lancadas` deixa a asserção vermelha, mesmo que o corpo ignore seu valor. **Há escape:** aceitar `**kwargs` e ler `kwargs['folgas_lancadas']`; a assinatura expõe `kwargs`, e o nome procurado continua ausente. O teste não chama a função nem verifica a origem do mapa. (`app/ponto/tests/test_grade_folga_juiz_sitios.py:178`, `app/ponto/tests/test_grade_folga_juiz_sitios.py:181`)

   Executei o método extraído da árvore em memória, com módulos substitutos: assinatura atual **verde**, parâmetro explícito **vermelho**, variante com `**kwargs` **verde**, aceitando e devolvendo o mapa externo. A prova é isolada da aplicação; execução da suíte Django: **[não verificado]**. (`app/ponto/tests/test_grade_folga_juiz_sitios.py:181`)

4. **Classes**

   - **2º juiz / derivação paralela:** o delta fecha a entrada alternativa e mantém o carregador e o seletor existentes. (`app/ponto/precedencia.py:282`, `app/ponto/precedencia.py:358`)
   - **2º escritor / relógio fora da borda:** não introduzidos pelo delta; a mudança é atribuição local a partir de leitura, sem nova escrita persistente ou consulta ao relógio. (`app/ponto/precedencia.py:206`, `app/ponto/precedencia.py:282`)
   - **Selo sem mordida:** possui mordida nominal comprovada, mas cobertura incompleta para impedir folgas externas por qualquer argumento. (`app/ponto/tests/test_grade_folga_juiz_sitios.py:181`)
   - **Função/parâmetro sem chamador:** remove o parâmetro sem fornecedor encontrado; a função continua chamada em produção. (`app/ponto/precedencia.py:259`, `app/ponto/precedencia.py:256`, `app/colaboradores/services/calendario.py:604`, `app/ponto/services/espelho.py:659`, `app/folha/services/sem_furo.py:25`)

## 01/11 10:52 · sessao 072a205776a1 · - · - · origem=Ronald (tmux)

Censo estático: **12 chamadas**, nenhuma fornece `folgas_lancadas` nem usa `**kwargs` nessa chamada:

| Natureza | Arquivo:linha | Argumentos opcionais |
|---|---|---|
| Produção, casca diária | `app/ponto/precedencia.py:256` | nenhum |
| Produção, calendário | `app/colaboradores/services/calendario.py:604` | `lugar`, `feriados`, `vinculos`, `folgas_foto` (`:605–606`) |
| Produção, espelho; alias `_fatos_periodo` | `app/ponto/services/espelho.py:657`, `:659` | `feriados` |
| Produção, folha | `app/folha/services/sem_furo.py:25` | nenhum |
| Teste | `app/colaboradores/tests/test_calendario_um_juiz.py:176` | nenhum |
| Testes | `app/ponto/tests/test_precedencia_pela_celula.py:77`, `:90`, `:106` | nenhum |
| Teste | `app/ponto/tests/test_precedencia_pela_celula.py:120` | `celulas_geradoras` em `:121` |
| Testes | `app/ponto/tests/test_precedencia_vinculo_encerrado.py:78`, `:86`, `:138` | nenhum |

A busca textual integral e AST dos `.py` não encontraram chamador adicional por command, template tag, referência em dict ou `getattr`; chamadas construídas fora da cópia permanecem **[não verificado]**. O command relacionado usa diretamente `folgas_lancadas_do_periodo` e `folga_lancada_do_dia`, sem `fatos_do_periodo` (`app/ponto/management/commands/rejulgar_ausencia_em_folga.py:51`, `:64–65`).

**Valor publicado:** nenhum chamador encontrado muda por este delta: todos omitiam o parâmetro e já seguiam o ramo `None` do diff fornecido, que chama o mesmo carregador agora incondicional em `app/ponto/precedencia.py:282`. A variável local tem sua leitura em `:342`, segue ao juiz em `:358`, entra nos fatos presentes em `:380–381` e no retorno em `:398–408`. É conclusão estática; valores efetivos de produção **[não verificado]**.

A única ocorrência executável do nome/chave literal removido fora dessa variável local é a fiscalização da assinatura no teste novo (`app/ponto/tests/test_grade_folga_juiz_sitios.py:181`).

## 01/11 10:52 · sessao 0aefb91249f8 · - · - · origem=Ronald (tmux)

`folgas_lancadas` tem um único escritor local, a atribuição da carga em `app/ponto/precedencia.py:282`, e um leitor, `.get(data)`, em `app/ponto/precedencia.py:342`. A carga lê `FolgaDia` por colaborador e intervalo, ordena por `pk` e monta `{data: [FolgaDia...]}` (`app/ponto/precedencia.py:204`, `app/ponto/precedencia.py:206`, `app/ponto/precedencia.py:208`).

O fluxo permanece `_folgas_candidatas` → `folga_lancada_do_dia` → predicado existente (`app/ponto/precedencia.py:342`, `app/ponto/precedencia.py:358`, `app/ponto/precedencia.py:217`, `app/ponto/precedencia.py:235`). O resultado participa da faixa `folga`, da ordenação de precedência e da publicação de `tipo_do_dia`, `faixa`, `suprimidos` e `fatos.folga_lancada` (`app/ponto/precedencia.py:380`, `app/ponto/precedencia.py:392`, `app/ponto/precedencia.py:398`, `app/ponto/precedencia.py:408`).

Para os chamadores de produção lidos — calendário, espelho por alias, sem_furo e casca diária — o argumento é omitido; portanto, com mesmas fontes e entradas, a carga que antes ocorria por `None` continua sendo a mesma (`app/colaboradores/services/calendario.py:604`, `app/ponto/services/espelho.py:657`, `app/ponto/services/espelho.py:659`, `app/folha/services/sem_furo.py:25`, `app/ponto/precedencia.py:256`, `app/ponto/precedencia.py:282`; condição anterior: delta fornecido). Mudança efetiva de valores em produção: [não verificado].

O delta fecha uma entrada alternativa e usa o carregador existente; não acrescenta segundo juiz, escritor persistente, relógio ou derivação paralela (`app/ponto/precedencia.py:259`, `app/ponto/precedencia.py:282`, `app/ponto/precedencia.py:412`). A função permanece chamada; a porta removida é o parâmetro (`app/ponto/precedencia.py:256`, `app/ponto/precedencia.py:259`). A frase antiga “o calendario passa True” não corresponde à chamada atual e não serve como prova de chamador vivo (`app/ponto/precedencia.py:229`, `app/colaboradores/services/calendario.py:604`).

## 01/11 10:52 · sessao 1c055287e6b5 · - · - · origem=Ronald (tmux)

O teste morde a volta **explícita** de `folgas_lancadas`: compara esse nome com `inspect.signature(...).parameters`; qualquer parâmetro explícito com esse nome reprova, mesmo sem uso. (app/ponto/tests/test_grade_folga_juiz_sitios.py:181)

Há escape: acrescentar `**kwargs` e recuperar `kwargs.get('folgas_lancadas')`. A assinatura contém `kwargs`, portanto a asserção continua verde e o argumento volta a ser aceito e usado. É limitação direta da asserção exclusivamente nominal; ela não chama a função nem verifica seu retorno/fonte. (app/ponto/tests/test_grade_folga_juiz_sitios.py:178–181)

Demonstração em ASTs executadas somente em memória, usando a assinatura real: atual → verde e `TypeError`; parâmetro explícito → vermelho; `**kwargs` com leitura da chave → verde e devolve o mapa recebido. A execução do teste Django completo permanece [não verificado]. A assinatura atual não possui `**kwargs`. (app/ponto/precedencia.py:259–260)

Mapa dos nomes novos do teste: `SimpleTestCase` entra pelo import e é lido como base da nova classe; `inspect` é importado localmente e lido na asserção; o import local de `fatos_do_periodo` também é lido nela. (app/ponto/tests/test_grade_folga_juiz_sitios.py:29; app/ponto/tests/test_grade_folga_juiz_sitios.py:174; app/ponto/tests/test_grade_folga_juiz_sitios.py:179–181)

Classificação: **selo com mordida limitada**, não selo totalmente inerte; barra a restauração literal, mas não sela a impossibilidade comportamental de receber folgas externas. Atualmente o mapa nasce obrigatoriamente da função interna e depois é lido por dia. (app/ponto/tests/test_grade_folga_juiz_sitios.py:181; app/ponto/precedencia.py:282; app/ponto/precedencia.py:342)

## 01/11 10:45 · sessao dcd3c2f8c7bb · GRADE-FOLGA-JUIZ · verificador · origem=Code

O `eh_trabalho` publicado preserva a lógica do main no diff fornecido; o `status` pode mudar porque passou a receber as folgas lançadas da pessoa inteira. Há um resíduo fora da ALLOWLIST — C5 — e há testes chamados `MORDE` que já passariam no main. (`app/colaboradores/services/calendario.py:604`, `app/colaboradores/services/calendario.py:772`, `app/colaboradores/services/calendario.py:890`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1532`; `app/escala/tests/test_grade_folga_vale_em_toda_competencia.py:74`)

A comparação com o main abaixo usa o diff integral fornecido. A identidade Git da cópia, execução de RED/GREEN, medidas de produção, reversão e ensaio desta árvore permanecem **[não verificado]**.

1. **Escritores, leitores e contratos tocados**

O contrato novo do carregamento é **`{data: [FolgaDia, ...]}`**, ordenado por `pk`; o retorno público de `fatos_do_periodo` continua sendo **`{data: fatos}`**. A lista de candidatas não é publicada na chave `folga_lancada`: essa chave continua sendo `bool`, ou `None` no ramo sem jurisdição. (`app/ponto/precedencia.py:206`, `app/ponto/precedencia.py:208`, `app/ponto/precedencia.py:386`, `app/ponto/precedencia.py:407`)

| Campo/estrutura | Quem escreve | Quem lê |
|---|---|---|
| Candidatas `{data: [FolgaDia]}` | `folgas_lancadas_do_periodo`, consultando pessoa e intervalo e agrupando todas as linhas: `app/ponto/precedencia.py:206`, `app/ponto/precedencia.py:208`. | Precedência: `app/ponto/precedencia.py:341`; grade: `app/escala/utils.py:1124`; veto: `app/ponto/precedencia.py:504`; declaração: `app/ponto/services/declarar_dia.py:74`; comando: `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:64`. |
| `FolgaDia.data`, vínculo e origem | A porta declarada é `escala/folgas.py`: `app/core/portas.py:466`. Materialização do calendário: `app/escala/folgas.py:173`; manual: `app/escala/folgas.py:259`, `app/escala/folgas.py:284`; reparentamento da FK: `app/escala/folgas.py:435`. | Carregador lê pessoa/data/`pk`: `app/ponto/precedencia.py:206`; predicado lê `calendario_origem_id`, `escala_colaborador_id` e `ec_do_dia.pk`: `app/ponto/precedencia.py:237`, `app/ponto/precedencia.py:239`. Demais leitores constam no item 4. |
| Resultado `FolgaDia \| None` do juiz | `folga_lancada_do_dia` escolhe a primeira candidata aceita: `app/ponto/precedencia.py:216`. | `fatos_do_periodo`: `app/ponto/precedencia.py:357`; overlay: `app/escala/utils.py:1124`; veto: `app/ponto/precedencia.py:505`; declaração: `app/ponto/services/declarar_dia.py:74`; comando: `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:65`. |
| `fatos[data]['tipo_do_dia']` | Sem jurisdição, `None`; caso contrário, o tipo do primeiro fato pela precedência: `app/ponto/precedencia.py:386`, `app/ponto/precedencia.py:391`, `app/ponto/precedencia.py:398`. | Calendário: `app/colaboradores/services/calendario.py:796`; tradução da palavra: `app/ponto/services/dia_decidido.py:370`; cartório: `app/ponto/services/cartorio.py:1699`; diagnóstico: `app/ponto/services/dia_do_colab.py:86`; sem-furo: `app/folha/services/sem_furo.py:26`; faltas de hoje: `app/ponto/services/faltas_de_hoje.py:61`; deslogados: `app/ponto/services/deslogados.py:166`; classificador de folga humana: `app/ponto/precedencia.py:191`. |
| `fatos[data]['faixa']` | `None` ou faixa vencedora: `app/ponto/precedencia.py:386`, `app/ponto/precedencia.py:392`, `app/ponto/precedencia.py:399`. | Calendário: `app/colaboradores/services/calendario.py:795`; folga humana: `app/ponto/precedencia.py:187`; cartório: `app/ponto/services/cartorio.py:1700`; reavaliação de chamados: `app/chamados/services/feriado_reavaliar.py:36`; semana do posto: `app/escala/services/semana_do_posto.py:177`; abriu/não bateu: `app/ponto/services/abriu_nao_bateu.py:123`; detector: `app/ponto/management/commands/detectar_ausencias.py:616`; diagnóstico: `app/ponto/services/dia_do_colab.py:130`. |
| `fatos[data]['fatos']['folga_lancada']` | `None` ou `bool(folga_lancada)`: `app/ponto/precedencia.py:388`, `app/ponto/precedencia.py:407`. | Os três leitores literais encontrados: `app/ponto/precedencia.py:190`, `app/colaboradores/services/calendario.py:803`, `app/ferias/situacao.py:97`. |
| `rotulo` | `app/ponto/precedencia.py:386`, `app/ponto/precedencia.py:400`. | Cartório: `app/ponto/services/cartorio.py:1701`; diagnóstico: `app/ponto/services/dia_do_colab.py:85`, `app/ponto/services/dia_do_colab.py:117`. |
| `suprimidos` | Lista das faixas perdedoras, sem repetição: `app/ponto/precedencia.py:393`, `app/ponto/precedencia.py:401`. | Texto da supressão: `app/ponto/precedencia.py:417`; cartório: `app/ponto/services/cartorio.py:1702`; diagnóstico: `app/ponto/services/dia_do_colab.py:87`. |
| `era_dia_previsto` | Célula, quando responde; senão cálculo do vínculo; exceção vira `None`: `app/ponto/precedencia.py:364`, `app/ponto/precedencia.py:367`, `app/ponto/precedencia.py:402`. | Reavaliação: `app/chamados/services/feriado_reavaliar.py:34`; cartório: `app/ponto/services/cartorio.py:1703`; diagnóstico: `app/ponto/services/dia_do_colab.py:84`; repouso para férias: `app/ferias/situacao.py:99`. |
| `fatos.ausencias`, `fatos.feriado` | Tipos de ausência encontrados e feriado encontrado, mesmo que suprimidos: `app/ponto/precedencia.py:403`, `app/ponto/precedencia.py:406`. | Diagnóstico: `app/ponto/services/dia_do_colab.py:88`; repouso lê feriado: `app/ferias/situacao.py:97`. |
| `fatos.feriado_so_no_pagamento` | Calculado em `app/ponto/precedencia.py:372`, usado internamente em `app/ponto/precedencia.py:377`, publicado em `app/ponto/precedencia.py:408`. | Não achei leitor nominal externo dessa chave na busca pelos `.py` de produção; consumidor dinâmico: **[não verificado]**. |
| `ata.precedencia` | O cartório seleciona `tipo_do_dia`, `faixa`, `rotulo`, `suprimidos`, `era_dia_previsto` e `texto`: `app/ponto/services/cartorio.py:1698`. A porta grava a chave: `app/ponto/portas/celula.py:819`. | A chave `folga_lancada` **não integra essa seleção persistida**: `app/ponto/services/cartorio.py:1698`. |
| `dia.tipo_dia`, `dia.celulas`, `dia.orfas` da grade | Builders-base: `app/escala/utils.py:727`, `app/escala/utils.py:752`, `app/escala/utils.py:1751`. Overlay agora escreve `folga`, preserva comprimento com `[None] * n` e limpa órfãs: `app/escala/utils.py:1125`. Ausências posteriores também podem sobrescrever: `app/escala/utils.py:1178`, `app/escala/utils.py:1209`. | Leitores externos detalhados abaixo; transformação para ata em `app/ponto/services/cartorio.py:582`. |
| `eh_trabalho` diário do calendário | Inicializado em `None`, recebe `ec.eh_dia_trabalho(...)`, exceção retorna a `None`; publicado sem substituição: `app/colaboradores/services/calendario.py:755`, `app/colaboradores/services/calendario.py:772`, `app/colaboradores/services/calendario.py:780`, `app/colaboradores/services/calendario.py:890`. | `_classificar`: `app/colaboradores/services/calendario.py:111`; `resolver_dia_oferecido`: `app/colaboradores/services/calendario.py:189`. |
| `status` diário do calendário | `_classificar` produz; contexto publica: `app/colaboradores/services/calendario.py:801`, `app/colaboradores/services/calendario.py:885`. | Porta curta: `app/colaboradores/services/calendario.py:213`; contagem: `app/colaboradores/services/calendario.py:224`; oferta de resolver: `app/colaboradores/services/calendario.py:190`; resumo: `app/colaboradores/services/calendario.py:936`; template: `app/templates/colaboradores/partials/_calendario_grade.html:69`. |
| Conjunto `folgas={(cid,data)}` do comando | Carregador pessoal + juiz + vínculo do dia: `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:62`. | Cruzamento com ausências: `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:67`; pertinência: `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:34`. |
| Tuplas de ausência `(colaborador_id,tipo,data,data_fim)` | Consulta materializada de aprovadas: `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:56`. | Seleção de pessoas: `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:63`; expansão e cruzamento: `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:28`. |
| `REGRA_VERSOES` | Declaração acrescenta `(3,'2026-10')`: `app/ponto/services/cartorio.py:111`. | Validação no import: `app/ponto/services/cartorio.py:185`; `versao_para`: `app/ponto/services/cartorio.py:209`, `app/ponto/services/cartorio.py:214`; versão chega à impressão por `app/ponto/services/cartorio.py:965`, `app/ponto/services/cartorio.py:1170`. |
| Pergunta nova de `JUIZES` e contagens documentais | Registro: `app/core/juizes.py:192`; espelhos: `app/docs/JUIZES.md:84`, `app/docs/MAPA-JUIZES.md:52`, `app/docs/MAPA.md:107`, `app/docs/ARQUITETURA.mmd:318`. | Gerador lê perguntas/quantidades/endereço do registro: `app/core/management/commands/gerar_diagrama.py:344`, `app/core/management/commands/gerar_diagrama.py:347`, `app/core/management/commands/gerar_diagrama.py:752`. |

Os consumidores externos da grade alcançados pelos três campos alterados incluem os seguintes sítios; a persistência continua passando por `ata_do_dia` e `lavrar_veredito`. (`app/ponto/services/cartorio.py:577`, `app/ponto/services/cartorio.py:1582`)

| Chave | Leitores fora do overlay |
|---|---|
| `tipo_dia` | `app/escala/orfa_classe.py:65`; `app/escala/services/furos_diarios.py:39`; `app/ponto/status_dia.py:146`; `app/ponto/supra_juiz.py:222`; `app/ponto/worklist.py:19`; `app/ponto/propositor.py:36`; `app/ponto/services/veredito_celula.py:221`; `app/ponto/services/dia_pago.py:386`; `app/ponto/services/espelho.py:690`; `app/ponto/services/simular_dia.py:12`; `app/ponto/services/bordas_realizado.py:74`; `app/ponto/services/fechamento.py:1011`; `app/chamados/services/materializacao.py:225`; `app/chamados/services/declaracao_texto.py:90`; `app/chamados/services/disputa_emissao.py:68`; `app/folha/causas.py:40`; `app/folha/export.py:1071`; `app/relatorios/furos.py:147`; `app/inteligencia/calculadores.py:46`; `app/ponto/management/commands/e6_piso_autopsia.py:13`; `app/ponto/management/commands/mapa_divergencia.py:131`. |
| `celulas` | `app/escala/services/furos_diarios.py:13`; `app/escala/orfa_classe.py:37`; `app/escala/services/leitor_celula.py:256`; `app/ponto/status_dia.py:143`; `app/ponto/propositor.py:38`; `app/ponto/supra_juiz.py:230`; `app/ponto/services/cartorio.py:582`; `app/ponto/services/espelho.py:696`; `app/ponto/services/simular_dia.py:14`; `app/ponto/services/veredito_celula.py:250`; `app/ponto/turnos.py:2667`; `app/folha/causas.py:24`; `app/relatorios/furos.py:149`; `app/relatorios/pdf_espelho.py:551`; `app/inteligencia/calculadores.py:51`; `app/api/views.py:1506`; `app/colaboradores/services/calendario.py:477`; `app/chamados/services/declaracao_texto.py:97`; `app/ponto/management/commands/detectar_par_relampago.py:141`; `app/ponto/management/commands/reconciliar_grade.py:152`; `app/ponto/management/commands/mapa_divergencia.py:120`; `app/ponto/management/commands/propor_reorganizacao.py:92`. |
| `orfas` | `app/escala/orfa_classe.py:58`; `app/escala/services/furos_diarios.py:16`; `app/ponto/status_dia.py:145`; `app/ponto/propositor.py:39`; `app/ponto/supra_juiz.py:373`; `app/ponto/services/cartorio.py:583`; `app/ponto/services/simular_dia.py:12`; `app/ponto/services/bordas_realizado.py:49`; `app/ponto/turnos.py:2674`; `app/folha/causas.py:43`; `app/relatorios/pdf_espelho.py:473`; `app/colaboradores/services/calendario.py:459`; `app/ponto/management/commands/detectar_par_relampago.py:140`. |

Depois da lavra, há leitores de `ata.tipo_dia` em `chamados/juizes`, no leitor da célula e no cartão; isso faz a mudança alcançar a premissa da cobrança, além da apresentação da grade. (`app/chamados/juizes.py:408`; `app/escala/services/leitor_celula.py:345`; `app/relatorios/cartao_pela_celula.py:288`)

O parâmetro **`folgas_lancadas` permanece, mas perdeu o último fornecedor encontrado**. Seu contrato efetivo passou de valor único por data para iterável de candidatas por data: o `.get(data)` alimenta diretamente o laço do juiz. Um antigo `{data: True}` ou `{data: FolgaDia}` não atende esse contrato. (`app/ponto/precedencia.py:260`, `app/ponto/precedencia.py:341`, `app/ponto/precedencia.py:216`)

Todos os chamadores encontrados de `fatos_do_periodo`, incluindo o alias, são:

| Produção | Testes | [nao verificado]
|---|---|
| `fatos_do_dia`: `app/ponto/precedencia.py:256` | `app/colaboradores/tests/test_calendario_um_juiz.py:176` |
| Calendário: `app/colaboradores/services/calendario.py:604` | `app/ponto/tests/test_precedencia_pela_celula.py:77`, `app/ponto/tests/test_precedencia_pela_celula.py:90`, `app/ponto/tests/test_precedencia_pela_celula.py:106`, `app/ponto/tests/test_precedencia_pela_celula.py:120` |
| Espelho, alias `_fatos_periodo`: `app/ponto/services/espelho.py:659` | `app/ponto/tests/test_precedencia_vinculo_encerrado.py:78`, `app/ponto/tests/test_precedencia_vinculo_encerrado.py:86`, `app/ponto/tests/test_precedencia_vinculo_encerrado.py:138` |
| Sem-furo: `app/folha/services/sem_furo.py:25` | Nenhum desses fornece `folgas_lancadas`; a alimentação explícita do teste em `app/ponto/tests/test_precedencia_pela_celula.py:120` é `celulas_geradoras`. |

Não achei outro fornecedor em busca textual de `folgas_lancadas=` e em varredura AST dos chamadores nos `.py` da cópia. Chamadas por reflexão: **[não verificado]**. A docstring que ainda diz “o calendário passa `True`” está vencida diante da chamada atual. (`app/ponto/precedencia.py:229`; `app/colaboradores/services/calendario.py:604`)

As funções de produção criadas/alteradas têm os seguintes chamadores:

| Função | Chamadores |
|---|---|
| `folgas_lancadas_do_periodo` | `app/ponto/precedencia.py:281`, `app/ponto/precedencia.py:501`, `app/escala/utils.py:1119`, `app/ponto/services/declarar_dia.py:74`, `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:64`. |
| `folga_lancada_do_dia` | `app/ponto/precedencia.py:357`, `app/ponto/precedencia.py:505`, `app/escala/utils.py:1124`, `app/ponto/services/declarar_dia.py:74`, `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:65`. |
| `folga_lancada_vale_no_dia` | Um chamador de produção: `app/ponto/precedencia.py:217`. |
| `_aplicar_folgas_foto` | Builder single: `app/escala/utils.py:770`; por turno: `app/escala/utils.py:1794`. |
| `montar_grade_prevista` | Legacy: `app/escala/utils.py:995`; ele conserva somente os dias do segmento julgado para esse vínculo: `app/escala/utils.py:999`. |
| `montar_grade_prevista_periodo_por_turno` | Dispatcher: `app/escala/utils.py:2036`; comando direto: `app/ponto/management/commands/detectar_par_relampago.py:136`. |
| `_folga_do_dia` | `o_dia_ja_e_folga`: `app/ponto/services/declarar_dia.py:95`; recusa: `app/ponto/services/declarar_dia.py:112`. |
| `o_dia_ja_e_folga` | Aceite/idempotência e tratamento após recusa: **`app/chamados/services/validacao.py:261`, `app/chamados/services/validacao.py:316`**. |
| `_recusa_do_dia` | `checar_declaracao`: `app/ponto/services/declarar_dia.py:147`. |
| `veto_de_lancamento` | `app/ferias/views.py:253`; `app/ferias/models.py:428`; `app/escala/folgas.py:258`, `app/escala/folgas.py:283`; `app/ponto/services/ausencia.py:380`; `app/ponto/services/declarar_dia.py:120`. |
| `contexto_calendario` | `app/colaboradores/views.py:1946`; `app/colaboradores/services/drawer.py:121`; `app/relatorios/furos.py:184`; portas curtas e contagem em `app/colaboradores/services/calendario.py:198`, `app/colaboradores/services/calendario.py:210`, `app/colaboradores/services/calendario.py:221`. |
| `Command.handle` | Entrada do comando: `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:45`; chamada por nome exercitada em `app/ponto/tests/test_grade_folga_juiz_sitios.py:122`. |
| `_folga_avulsa`, apagada | Não há definição remanescente; há lápide em `app/ponto/precedencia.py:161` e teste de ausência em `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:187`. |

Na catraca, os dados novos são conjuntos de triplas `(arquivo, função, classe_do_achado)`. `REL`, `CAMPO_ORIGEM` e `JUIZ` alimentam o analisador; `ALLOWLIST` é comparada com os achados reais nos dois sentidos. `_cadeia` escreve os passos, `_getattr_literal` reconhece acesso literal, `_le_a_origem` reconhece leitura de origem, `achados_da_fonte` produz as triplas e `achados` percorre arquivos. Seus leitores são o próprio percurso e os testes. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:28`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:35`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:54`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:62`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:72`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:123`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:176`)

Os testes escrevem fixtures de cadastro, vínculos, células/DNA, folgas, ausências, chamados, batidas, fechamento e exportação; essas escritas estão nos respectivos preparadores e casos, sem escritor novo de produção no diff. (`app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:28`; `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:41`; `app/escala/tests/test_grade_folga_vale_em_toda_competencia.py:38`; `app/ponto/tests/test_grade_folga_juiz_sitios.py:41`; `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:45`)

2. **`eh_trabalho` e diferenças de `status`**

**Sim: a lógica que publica `eh_trabalho` é a do main, segundo o diff dado.** O vínculo ativo continua sendo carregado no mesmo sítio; `_cels_mes` e `_folgas_mes` continuam alimentando `ec.eh_dia_trabalho`; a guarda de vigência, o tratamento da exceção e a publicação permanecem iguais. O método chamado continua lendo `cel.trabalha` antes do cálculo aritmético. (`app/colaboradores/services/calendario.py:336`, `app/colaboradores/services/calendario.py:395`, `app/colaboradores/services/calendario.py:593`, `app/colaboradores/services/calendario.py:769`, `app/colaboradores/services/calendario.py:772`, `app/colaboradores/services/calendario.py:890`; `app/escala/models.py:1071`)

A linha causal do diff para o calendário é a chamada em **`app/colaboradores/services/calendario.py:604`**: sai a alimentação `{d: True}` do vínculo ativo. O carregador padrão passa a trazer a pessoa inteira, e o juiz examina todas as candidatas contra o vínculo do dia. (`app/ponto/precedencia.py:281`, `app/ponto/precedencia.py:350`, `app/ponto/precedencia.py:357`)

| Situação | Diferença possível em relação ao main |
|---|---|
| Manual presa ao vínculo velho, omitida pela foto do ativo | Passa a ser fato de folga; no caso sem batida do teste, `falta → folga`. (`app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:54`) |
| Calendário do ativo, mas célula gerada pelo velho | Deixa de ser tratado como manual válida; no caso do teste, `folga → falta`. (`app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:96`) |
| Calendário de outro vínculo, sem candidata válida | Continua inválido; o caso de preservação espera `falta`. (`app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:61`) |
| Inválida de menor `pk` antes de outra válida | O carregador anterior podia esconder a válida; agora o juiz percorre todas. (`app/ponto/precedencia.py:208`, `app/ponto/precedencia.py:216`; `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:85`) |
| Calendário válido do vínculo do dia, ausente da foto do ativo | Agora pode ser encontrado pelo universo da pessoa e aceito pela igualdade de vínculo. (`app/ponto/precedencia.py:206`, `app/ponto/precedencia.py:239`) |

Essas alterações **não implicam sempre** `falta ↔ folga`: ausência vencedora sem batida retorna antes; feriado tem tratamento próprio; turno aberto continua `aberto`; com batida e folga humana, o ramo é `folga_trabalhada`; sem batida, a folga humana força o caminho de folga. (`app/colaboradores/services/calendario.py:109`, `app/colaboradores/services/calendario.py:112`, `app/colaboradores/service

[truncado em 20000 de 35641 caracteres]

## 01/11 10:40 · sessao 069db40031e4 · - · - · origem=Ronald (tmux)

Avaliação estática; testes não executados e RED/GREEN observado em execução permanece [não verificado]. [nao verificado]

| Arquivo/teste (linha da definição) | Sem a cura do diff | [nao verificado]
|---|---|
| `app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:54` | Morde: main perde folga manual do velho; mudança do carregamento está em `app/colaboradores/services/calendario.py:604`. |
| Mesmo arquivo `:61` | Já passa main; par de preservação, conforme a própria caracterização `:10`. |
| Mesmo arquivo `:96` | Morde: foto do ativo confundia calendário dele com manual no dia da geradora velha; fixture `:84`, `:89`, `:98`. |
| `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:70` | Morde por turno; já passa legacy, distinção documentada `:21`. |
| Mesmo arquivo `:75` | Já passa por turno; morde legacy, distinção documentada `:22`. |
| Mesmo arquivo `:80` | Já passa ambos: calendário do próprio vínculo; fixture/as débeis `:81`, `:82`. |
| Mesmo arquivo `:85` | Morde ambos pela precedência: calendário inválido de menor pk escondia o válido; caracterização `:16`, asserção `:89`, carregador corrigido `app/ponto/precedencia.py:208`. |
| Mesmo arquivo `:91` | Morde ambos: manual fora da janela, caso `:92`, `:93`; universo agora pessoal `app/escala/utils.py:1119`. |
| `app/escala/tests/test_grade_folga_vale_em_toda_competencia.py:69` | Premissa já passa main; só verifica versões `:71`, `:72`. |
| Mesmo arquivo `:74`, `:79`, `:84` | **Já passam main nos dois builders.** Velho termina em 31/07 (`:47`), janela começa em 13/08 (`:25`, `:66`); main por turno só aplicava overlay dos vínculos da janela, cujo conjunto continua sendo formado em `app/escala/utils.py:1268`. Legacy segmenta pelo vínculo de cada dia em `app/escala/utils.py:975`, `:985`, `:995`. Simulação usa janela de um dia em `app/ponto/services/simular_dia.py:7`. São úteis contra retorno de gating, mas não RED da cura contra este main. |
| Mesmo arquivo `:90` | Morde ambos: manual do velho fora da janela deve ser aceita; assert `:93`. |
| `app/ponto/tests/test_grade_folga_juiz_sitios.py:79` | Morde veto antigo; caso/asserts `:80`, `:81`, `:82`. |
| Mesmo arquivo `:84` | Par já passava main; manual continua vetando `:86`, `:87`. |
| Mesmo arquivo `:93` | Morde declarador antigo; manual do velho precisa aparecer `:96`, `:97`. |
| Mesmo arquivo `:99`, `:105` | Pares já passam main; calendário alheio recusado como fato e próprio continua gerido por calendário `:102`, `:109`. |
| Mesmo arquivo `:125` | Morde universo antigo do comando; espera zero `:129`. |
| Mesmo arquivo `:131` | Par já passava main; espera um `:135`. |
| Mesmo arquivo `:159`, `:166` | Já passam main; própria documentação registra que gate e juiz já concordavam `:141`, `:142`, `:143`. |
| `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:95` | Premissa já passa main `:97`, `:99`. |
| Mesmo arquivo `:101` | Morde por turno: ata precisa mudar para trabalho `:103`. |
| Mesmo arquivo `:106` | Morde por turno pela assert adicional da ata `:114`; preservação FM/TXT sozinha não separa cura do main `:115`, `:117`, `:118`, `:119`. |
| Mesmo arquivo `:121` | Já passa main; protege horizonte e não comprova cura. Assert final `:128`. |
| Mesmo arquivo `:130` | Morde por turno pela resposta explícita trabalho `:136`; evita igualdade das duas testemunhas erradas. |
| `app/ponto/tests/test_o219_versao_da_regra.py:67`, `:78` | Mordem retirada do bump: tupla exige v3 `:76`; leitor exige 3 na competência 10 `:89`, `:90`. |

Os casos da primeira bateria de grade são herdados pelos dois builders em `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:97` e `:102`; os da segunda também, em `app/escala/tests/test_grade_folga_vale_em_toda_competencia.py:98` e `:103`. Os testes do cartório não fixam a flag; default é True em `app/config/settings/base.py:165`; com False, o calendário alheio já era ignorado no main legacy por segmentação `app/escala/utils.py:985`, `:995`.

Na catraca:

- `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:176` morde produção antiga mediante achados fora da allowlist `:178`; `:182` morde `_folga_avulsa` sobrevivente `:187`.
- Testes `:189`, `:194`, `:200`, `:223`, `:230`, `:236`, `:245`, `:262` são testes do analisador/percurso com fontes sintéticas; passam sem migrar os leitores de produção, e protegem o scanner, conforme cada fonte/assert em `:190`, `:195`, `:201`, `:226`, `:231`, `:238`, `:251`, `:263`. [nao verificado]
- `:204` e `:215` inspecionam funções novas `:212`, `:220`: main puro falharia por atributo inexistente; mantendo funções novas e restaurando leitores antigos, esses dois ainda passam. A costura de produção é cobrada por `:176` e `:182`.
- Não executei o teste de percurso, pois cria arquivos temporários em `:256`, `:257`, `:258`, `:259`; execução [não verificado]. [nao verificado]

Em `app/ponto/tests/test_rejulgar_ausencia_em_folga.py:48` só mudou o mock do helper. Os três casos `:58`, `:64`, `:70` recebem universo já mockado `:52` e julgamento mockado `:53`; não verificam esta regra. Transplantar o helper atual ao main deixa a consulta FolgaDia antiga sem mock, podendo produzir erro incidental de acesso a banco; isso não seria RED da regra `:47`, `:48`, `:49`.

Não acusar fixture sem carimbo no teste `sem_forcar`: as batidas são criadas em `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:62`; seu signal chama julgamento em `app/ponto/signals.py:131`, `:135`, `:93`. Depois só a ata é sobreposta em `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:74`. Contudo o teste não afirma explicitamente impressão/carimbo em dia; pulo real exige ambos e veredito em `app/ponto/services/cartorio.py:1176`, `:1180`. Estado concreto da fixture e execução permanecem [não verificado].

Para 3(f), nenhum ato de pouso é comprovado pelos anexos [não verificado]. Os pré-requisitos existem: bump é ato de frota e exige ensaio `--apply` antes do pouso em `app/ponto/services/cartorio.py:173`, `:175`; sombra no main atual, reversão e frota antes do opus em `app/docs/PROMPTS.md:1758`; reconstrução de projeções não exportadas pela porta em `app/docs/LEIS.md:142`. Fase conflitante sai do escopo em `app/docs/PROMPTS.md:1781`; publicação de `eh_trabalho` sai em `:1783`.

## 01/11 10:38 · sessao af94c54b7920 · - · - · origem=Ronald (tmux)

Resultado estático dos sítios delegados; execução dos testes e pré-requisitos de produção: [não verificado]. [nao verificado]

| Função/objeto | Escritor e leitores de produção |
|---|---|
| `_aplicar_folgas_foto` | Implementação em `app/escala/utils.py:1103`; chamada pelo builder single em `:770` e pelo builder por turno em `:1794`. O single é chamado pelo legacy em `:995`; o dispatcher escolhe os builders em `:2036` e `:2038`. |
| Contrato novo do overlay | Recebe pessoa e `esc_do_dia`; carrega pela pessoa em `app/escala/utils.py:1119`, julga em `:1124`, lê comprimento de `celulas` em `:1125`, escreve `tipo_dia`, `celulas`, `orfas` em `:1126`, `:1127`, `:1128`. |
| `_folga_do_dia` | Implementação delega ao carregador e juiz em `app/ponto/services/declarar_dia.py:73`; leitores locais em `:95` e `:112`. |
| `o_dia_ja_e_folga` | Implementação em `app/ponto/services/declarar_dia.py:78`; leitores externos em `app/chamados/services/validacao.py:261` e `:316`. |
| `_recusa_do_dia` | Implementação em `app/ponto/services/declarar_dia.py:98`; leitura por `checar_declaracao` em `:147`; leitura da origem em `:115` escolhe frase após o juiz aceitar a candidata em `:112`. |
| Universo `folgas` do command | Escrito em `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:62`, carregado em `:64`, julgado em `:65`, acrescentado em `:66`, lido pelo cruzamento em `:67`. O cruzamento lê tuplas das ausências em `:28`, delega expansão ao juiz em `:33` e testa pertinência em `:34`. |
| `REGRA_VERSOES` | Escrito em `app/ponto/services/cartorio.py:111`; lido pelo validador de import em `:185` e pelo leitor em `:209` e `:214`. |
| Versão na impressão | `versao_para` chamada em `app/ponto/services/cartorio.py:965`; resultados passam a `impressao_insumos` em `:1017` e `:1170`; a impressão acrescenta `regra:` em `:370`; carimbo controla o pulo em `:1018`–`:1024` e `:1176`–`:1182`. |

Os escritores-base dos três campos da grade são os builders em `app/escala/utils.py:727`, `:752`, `:1751`, `:1757`. O overlay alterado já existia e segue escrevendo o dict transitório em `:1126`; ausência e isenção continuam posteriores à folga em `:771`, `:772`, `:1797`, `:1799`. A tradução para ata ocorre em `app/ponto/services/cartorio.py:585`, `:589`, `:592`; a lavra continua pela porta `lavrar_veredito` em `:1061` e `:1582`.

Leitores diretos dos campos da grade localizados por `rg`, excluindo tests/migrations:

- `tipo_dia`: `app/escala/utils.py:852`, `:1018`, `:1178`; `app/escala/orfa_classe.py:65`; `app/escala/services/furos_diarios.py:39`; `app/ponto/status_dia.py:146`; `app/ponto/supra_juiz.py:222`; `app/ponto/worklist.py:19`; `app/ponto/propositor.py:36`, `:210`; `app/ponto/services/veredito_celula.py:221`; `app/ponto/services/dia_pago.py:386`, `:393`, `:398`; `app/ponto/services/cartorio.py:585`, `:1015`; `app/ponto/services/espelho.py:690`; `app/ponto/services/simular_dia.py:12`; `app/ponto/services/bordas_realizado.py:74`; `app/ponto/services/fechamento.py:1011`; `app/chamados/services/materializacao.py:225`; `app/chamados/services/declaracao_texto.py:90`; `app/chamados/services/disputa_emissao.py:68`; `app/folha/causas.py:40`, `:41`, `:100`; `app/folha/export.py:1071`, `:1075`, `:1080`; `app/relatorios/furos.py:147`, `:151`, `:218`, `:237`; `app/inteligencia/calculadores.py:46`, `:141`; `app/ponto/management/commands/e6_piso_autopsia.py:13`; `app/ponto/management/commands/mapa_divergencia.py:131`, `:143`, `:155`, `:183`, `:188`, `:194`.
- `celulas`: `app/escala/utils.py:809`, `:1016`, `:1061`, `:1179`, `:1209`, `:1774`, `:1841`, `:1916`; `app/escala/orfa_classe.py:37`; `app/escala/services/furos_diarios.py:13`; `app/escala/services/leitor_celula.py:256`; `app/ponto/status_dia.py:143`; `app/ponto/propositor.py:38`, `:212`; `app/ponto/supra_juiz.py:230`, `:371`, `:439`, `:497`; `app/ponto/services/cartorio.py:582`, `:716`, `:1189`; `app/ponto/services/espelho.py:372`, `:696`; `app/ponto/services/simular_dia.py:14`; `app/ponto/services/veredito_celula.py:250`, `:288`; `app/ponto/turnos.py:2667`; `app/folha/causas.py:24`, `:58`; `app/relatorios/furos.py:149`, `:220`, `:242`; `app/relatorios/pdf_espelho.py:551`; `app/inteligencia/calculadores.py:51`, `:146`; `app/api/views.py:1506`; `app/colaboradores/services/calendario.py:464`, `:477`; `app/chamados/services/declaracao_texto.py:97`; `app/ponto/management/commands/detectar_par_relampago.py:141`; `app/ponto/management/commands/reconciliar_grade.py:152`; `app/ponto/management/commands/mapa_divergencia.py:120`, `:141`, `:174`; `app/ponto/management/commands/propor_reorganizacao.py:92`.
- `orfas`: `app/escala/utils.py:1085`, `:1843`, `:1844`, `:1917`, `:1922`; `app/escala/orfa_classe.py:58`; `app/escala/services/furos_diarios.py:16`; `app/ponto/status_dia.py:145`; `app/ponto/propositor.py:39`; `app/ponto/supra_juiz.py:373`, `:490`; `app/ponto/services/cartorio.py:583`, `:589`, `:592`, `:1191`; `app/ponto/services/simular_dia.py:12`; `app/ponto/services/bordas_realizado.py:49`; `app/ponto/turnos.py:2674`; `app/folha/causas.py:43`, `:100`; `app/relatorios/pdf_espelho.py:473`, `:548`; `app/colaboradores/services/calendario.py:459`, `:460`; `app/ponto/management/commands/detectar_par_relampago.py:140`.

Leitores posteriores da ata incluem `app/chamados/juizes.py:408` (`tipo_dia`), `:459` (`orfas`), `app/escala/services/leitor_celula.py:158` (`orfas`), `:345` (`tipo_dia`), `app/chamados/services/materializacao.py:166`, `app/ponto/services/esmeril_espelho.py:125`, `app/ponto/services/cluster_espurio.py:30`, `app/ponto/juiz_batida.py:161`, `app/relatorios/cartao_pela_celula.py:288`, `app/ponto/management/commands/tripwire_celulas.py:342`.

Classes:

- **(b) Não encontrei segundo escritor persistente introduzido nesses sítios**: o overlay escreve dict em memória (`app/escala/utils.py:1126`), a lavra permanece pela porta (`app/ponto/services/cartorio.py:1582`), o command chama `julgar_celula` (`app/ponto/management/commands/rejulgar_ausencia_em_folga.py:78`), declarar folga chama `lancar_folga` (`app/ponto/services/declarar_dia.py:189`).
- **(c) Nenhum relógio novo nesses trechos**: o `localdate` do command está na borda (`app/ponto/management/commands/rejulgar_ausencia_em_folga.py:52`) e não foi adicionado pelo diff fornecido. Há relógios preexistentes internos nos builders single/legacy (`app/escala/utils.py:658`, `:964`).
- **(d) O overlay e `_folga_do_dia` delegam, sem reescrever o predicado** (`app/escala/utils.py:1124`; `app/ponto/services/declarar_dia.py:74`). A origem em `_recusa_do_dia` escolhe a frase após a seleção do juiz (`app/ponto/services/declarar_dia.py:112`, `:115`).
- **(f) Há pré-requisito explícito de ensaio com `--apply` antes do pouso da versão 3** (`app/ponto/services/cartorio.py:174`); cumprimento não comprovado pelo dado fornecido [não verificado]. O corte inclui fase conflitante no mesmo pouso (`app/docs/CORTES.md:416`), enquanto a dispensa posterior está declarada na documentação/teste (`app/docs/JUIZES.md:91`; `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:148`); a existência do aval externo não está comprovada [não verificado].
- **(g) Nenhuma dessas funções tem zero chamador de produção**: sítios de chamada na tabela acima; `Command.handle` é entrada do Django (`app/ponto/management/commands/rejulgar_ausencia_em_folga.py:45`).

## 01/11 10:38 · sessao 1dbfb8d26830 · - · - · origem=Ronald (tmux)

Leitores literais da chave `folga_lancada` em produção encontrados:

| Leitor | Fonte | Monta universo próprio? | ALLOWLIST |
|---|---|---|---|
| `folga_humana_ou_feriado` | `app/ponto/precedencia.py:190` | Não; lê dict da precedência | Não consta; não é consulta própria |
| `contexto_calendario` | `app/colaboradores/services/calendario.py:803` | Não; lê `_fatos` | Não consta; não é consulta própria |
| `_dia_de_descanso` | `app/ferias/situacao.py:94`, `app/ferias/situacao.py:97` | Não; chama `fatos_do_dia` | Não consta; não é consulta própria |

Universos/leituras de FolgaDia por data ainda fora do novo carregador+juiz:

| Sítio | Leitura | ALLOWLIST |
|---|---|---|
| `folgas_do_periodo` | `app/escala/alimentacao.py:66` | Sim, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:140` |
| `defeito_folga_nao_definida` | `app/escala/models.py:1220` | Sim, catraca `:158` |
| `eh_dia_trabalho_calculado` | `app/escala/models.py:1257`, `:1281`, `:1327`, `:1330` | Sim, catraca `:160`; pendência GRADE-FOLGA-GERADOR |
| `fase_conflitante.medir` | `app/escala/services/fase_conflitante.py:74`, `:80` | Sim, catraca `:142`; fila separada explicitada `:143` |
| `divergencia_foto_x_calendario` | `app/escala/folgas.py:101` | Sim, catraca `:147`, `:148` |
| `gerar_foto_folgas` | `app/escala/folgas.py:165`, `:169` | Sim, catraca `:149`, `:150` |
| `lancar_folga` | `app/escala/folgas.py:254`, `:259` | Sim, catraca `:151`, `:152` |
| `_foto_do_dia` | `app/escala/folgas.py:274` | Sim, catraca `:145`, `:146` |
| `reparentar_folga` | `app/escala/folgas.py:405`, `:408`, `:418` | Sim, catraca `:153`, `:154` |
| `reparentar_folgas_do_colaborador` | `app/escala/folgas.py:453`, `:457` | Sim, catraca `:155` |
| command `reparentar_folgas` | `app/escala/management/commands/reparentar_folgas.py:28`, `:31` | Sim, catraca `:156` |
| `_grid_ctx` | `app/escala/views_plano_folgas.py:60`, `:63`, `:70` | Sim, catraca `:162` |
| `plano_folgas_toggle` | `app/escala/views_plano_folgas.py:112`, `:113` | Sim, catraca `:163`, `:164` |
| `template_do_vinculo` | `app/ponto/services/diagnostico_escala.py:102` | Sim, catraca `:167` |
| `escalas_em_limbo` | `app/relatorios/furos.py:112` | Sim, catraca `:168` |
| Auditor C5 | `app/chamados/management/commands/auditar_invariantes_chamados.py:1532` | **Não**, ALLOWLIST completa em catraca `:139`–`:169` |

C5 é o resíduo relevante: percorre todas as FolgaDia pelo alias `_FD`, usa cobertura do vínculo e `vinculo_que_cobre` para formar pares (`app/chamados/management/commands/auditar_invariantes_chamados.py:1532`, `:1534`, `:1535`), cruza células (`:1537`), acusa `trabalha=True` (`:1542`) e exclui competências exportadas (`:1546`). Não distingue calendário de manual nem pergunta ao juiz nesse bloco; a própria catraca declara esse escape e a fila GRADE-FOLGA-C5 como incompleto (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:19`).

`_recusa_do_dia` lê a origem somente da folga já aceita pelo juiz, para escolher a frase (`app/ponto/services/declarar_dia.py:112`, `:115`); tem ALLOWLIST específica (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:165`).

Extraí e executei somente o analisador AST da catraca em memória, com `python3 -B`, sem importar Django e sem rodar testes: encontrou 22 entradas, nenhuma fora da ALLOWLIST e nenhuma obsoleta. A lógica que explica a cegueira diante de C5 exige métodos `filter/exclude/get/get_or_create`, pessoa e data (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:103`, `:105`, `:106`); C5 usa `select_related` e filtra em Python (`app/chamados/management/commands/auditar_invariantes_chamados.py:1533`).

Não achei outros sítios por busca textual de `folga_lancada`, `FolgaDia`, `folgas_dia` e `folgas_geradas` em toda a cópia, separando produção de tests/migrations; inexistência absoluta por reflexão/dados dinâmicos: [não verificado].

Há uma derivação inline preexistente da classificação “folga humana”: calendário `app/colaboradores/services/calendario.py:803` repete a expressão de `app/ponto/precedencia.py:187`, `:190`, `:191`; a própria precedência documenta a migração pendente do calendário (`app/ponto/precedencia.py:183`). Ela não nasceu no diff dado.

## 01/11 04:57 · sessao eb235f80a993 · V3-DOBRA-SEM-NOTURNA · verificador · origem=Code

1. **NADA — a prosa corrigida confere no caso declarado de pausa idêntica duplicada.** O caminho da ata pode preservar o período geométrico e acrescentar outro; a coleta repete a pausa, o recuo ignora a repetição e o desconto noturno une intervalos sobrepostos. Referências: `app/ponto/motor_calculo_v2.py:685`, `:688`, `:397`, `:404`, `:169`; `app/ponto/calculador/alimentacao.py:107`.

2. **PROCEDENTE documental — “a MESMA tupla” generaliza além do código.** A porta entrega a coleção agregada do dia; o motor entrega somente os intervalos daquele período. O teste exige três pausas distintas nessa coleção diária. Melhor redação: **“intervalos da mesma fonte, agregados por dia”**. Referências: `app/ponto/calculador/alimentacao.py:107`, `:527`; `app/ponto/motor_calculo_v2.py:1006`; `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:173`.

   **NADA quanto a impacto funcional adicional omitido demonstrado:** os leitores alcançados são os declarados — split da HE e noturnas nos ramos normal e folga. Referências: `app/ponto/calculador/regras.py:828`, `:842`, `:714`.

3. **NADA — novo defeito funcional demonstrado nesta volta.** A pausa idêntica duplicada recebe os tratamentos descritos no docstring; isso sustenta esse caso específico. Referências: `app/ponto/motor_calculo_v2.py:397`, `:404`, `:169`.

**DUVIDA remanescente:** equivalência com pausas distintas em envelopes sobrepostos efetivamente produzidos pelo motor: [nao verificado]. Execução integral Django, logs RED/mutante e ocorrência em produção: [nao verificado].

## 01/11 04:56 · sessao 98899c1c6f1e · - · - · origem=Ronald (tmux)

**NADA — impacto funcional omitido demonstrado:** o argumento novo alcança os leitores declarados: split da HE (`app/ponto/calculador/regras.py:828`) e noturnas normal/folga (`app/ponto/calculador/regras.py:842`, `:714`). Os limiares informados conferem: duração efetiva bruta >360 para 12x36 e >240 para comercial; turno partido não desconta intra do período (`app/ponto/motor_calculo_v2.py:2749`, `:2755`, `:3201`, `:3214`, `:2881`).

**PROCEDENTE, apenas precisão documental — “a MESMA tupla” não é identidade literal.** A porta concatena pausas de todos os períodos do dia, depois cria uma tupla; o motor entrega somente `periodo.intervalos_do_motor` (`app/ponto/calculador/alimentacao.py:107`, `:527`; `app/ponto/motor_calculo_v2.py:1006`). O teste exige três pausas distintas nessa coleção (`app/ponto/tests/test_v3_dobra_split_noturno_porta.py:165`, `:173`). Formulação precisa: “intervalos da mesma fonte, agregados por dia”. Equivalência geral com pausas distintas em envelopes sobrepostos efetivamente produzidos: **[nao verificado]**.

O escritor produtivo localizado é `_carimbar_segmento` (`app/ponto/motor_calculo_v2.py:1830`). Logs RED, mutante, impacto de frota e estado de produção: **[nao verificado]**.

## 01/11 04:56 · sessao cd8660f33972 · - · - · origem=Ronald (tmux)

**1. NADA — a correção da prosa está coerente com o código, no caso declarado de pausa idêntica duplicada.** O motor recorta cada intervalo à janela efetiva antes de gravá-lo nos três ramos; a coleta concatena as listas; o recuo ignora a repetição após alcançar seu início; e o desconto noturno une intervalos sobrepostos. Referências: `app/ponto/motor_calculo_v2.py:1874`, `:1875`, `:2752`, `:2753`, `:2879`, `:2880`, `:3212`, `:3213`; `app/ponto/calculador/alimentacao.py:107`; `app/ponto/motor_calculo_v2.py:397`, `:404`, `:169`, `:170`.

O caminho citado conserva períodos cujo mapa aponta outro dia e acrescenta o período da ata; o dia de jornada depois é novamente consultado pelo instante da entrada. Isso sustenta a possibilidade descrita de duplicação no mesmo dia de jornada. Referências: `app/ponto/motor_calculo_v2.py:685`, `:686`, `:688`, `:2391`.

**3. NADA — nenhum novo defeito demonstrado nessa correção do docstring.** A caracterização “fila/zona inviolável” é decisão de escopo fornecida pelo usuário; sua formalização na cópia: [nao verificado].

## 01/11 04:32 · sessao 4345dfc785b1 · V3-DOBRA-SEM-NOTURNA · verificador · origem=Code

**1. NADA — não localizei defeito procedente no teste novo.** Ele exige exatamente as três pausas, incluindo `periodos_ft`, e cinco segmentos; perder o segundo período ou o FT viola a asserção. HE zerada não torna a coleta vazia: ela ocorre fora da condição de HE positiva. Referências: anexo `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:157`, `:164`, `:168`, `:170`; anexo `app/ponto/calculador/alimentacao.py:105`, `:107`.

As datas são explícitas. Existe leitura indireta preexistente de `date.today()`, mas os fixtures N6/N12 usam uma pausa fixa, sem variação semanal que altere estes resultados. Referências: `app/ponto/tests/test_o280_bateria_da_lei.py:1186`, `:1200`, `:1086`; anexo `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:44`; `app/ponto/juiz_batida.py:125`, `:128`, `:135`.

**2. PROCEDENTE — existe caminho para envelopes sobrepostos no mesmo dia de jornada.** Portanto, a afirmação de disjunção no docstring novo é incorreta como garantia geral. Referências: anexo `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:139`; `app/ponto/motor_calculo_v2.py:684`, `:688`.

Contraprova sintética, reproduzida com corpos reais das funções em memória:

- DNA de **01/09: 22–07**; célula/DNA de **02/09: 00–07**, pausa **03–04**; batidas de 02/09: **00E, 03S, 04E, 07S**. O pareamento atribui esse turno a **01/09**, pela janela da véspera. Referências: `app/ponto/turnos.py:299`, `:306`; `app/ponto/motor_calculo_v2.py:887`, `:889`.
- A ata de 02/09 responde **00–07**, com pausa **03–04**. O filtro preserva o período geométrico, pois seu mapa diz 01/09, e acrescenta outro **00–07**. Ambos recebem dia de jornada **01/09**. Referências: `app/ponto/juiz_batida.py:259`, `:260`, `:285`; `app/ponto/motor_calculo_v2.py:685`, `:688`, `:2391`.
- Com janela de HE desativada, as pontas efetivas permanecem iguais às cruas: os envelopes efetivos também coincidem. Referências: `app/ponto/motor_calculo_v2.py:1680`, `:1722`, `:1828`, `:1829`.

**3. NADA — novo erro funcional demonstrado no diff completo.** A contraprova acima duplica também a mesma pausa: o recuo ignora a repetição e o desconto noturno une intervalos sobrepostos. Ela refuta o descarte por disjunção, mas não demonstra contaminação nova do split por A. Referências: `app/ponto/motor_calculo_v2.py:397`, `:404`, `:169`.

**DUVIDA — equivalência com pausas diferentes em envelopes sobrepostos efetivamente produzidos pelo motor:** [nao verificado]. Execução integral Django da contraprova e da árvore proposta, e ocorrência em produção: [nao verificado].

## 01/11 04:31 · sessao 112211ccb2b1 · - · - · origem=Ronald (tmux)

**PROCEDENTE — existe caminho de períodos sobrepostos do mesmo dia de jornada.** A premissa global de disjunção é falsa: `_periodos_pelo_marco` remove pelo dia do mapa e acrescenta o envelope da célula sem exigir que a entrada pertença a esse dia. Referências: `app/ponto/motor_calculo_v2.py:684`, `:685`, `:688`.

Contraprova sintética reproduzida em memória, somente leitura, com corpos reais extraídos por AST:

- DNA de 01/09: **22–07**; DNA/célula de 02/09: **00–07**, pausa **03–04**.
- Batidas de 02/09: **00E, 03S, 04E, 07S**.
- `parear_turnos` gera **00–07 de 02/09 atribuído a 01/09**: a entrada cabe na jornada da véspera. Referências: `app/ponto/turnos.py:299`, `:300`, `:306`.
- `_dia_do_turno` registra a entrada 00 como pertencente a 01/09. Referências: `app/ponto/motor_calculo_v2.py:884`, `:887`, `:889`.
- O juiz da célula 02/09 responde **00–07**, pausa **03–04**. Referências: `app/ponto/juiz_batida.py:174`, `:259`, `:260`, `:282`, `:285`.
- O filtro para 02/09 preserva o período geométrico mapeado para 01/09 e acrescenta uma cópia **00–07**. A execução devolveu **duas tuplas idênticas**, ambas atribuídas a **01/09**. Referências: `app/ponto/motor_calculo_v2.py:684`, `:685`, `:688`, `:722`, `:2391`.

Com janela de HE desativada, efetivos permanecem iguais às pontas cruas e são carimbados nos períodos; portanto os envelopes efetivos também se sobrepõem. Referências: `app/ponto/motor_calculo_v2.py:1680`, `:1681`, `:1722`, `:1723`, `:1828`, `:1829`.

**Limite da prova:** executei corpos reais de `parear_turnos`, `periodos_de_turnos`, `periodos_do_dia`, `montar_lampadas`, `_dia_do_turno` e `_periodos_pelo_marco`; Django não está instalado, então timezone foi substituído por normalização UTC−03 e a entidade CelulaDia teve constantes e método de ordenação extraídos do próprio código. Execução integral de `calcular_mes` no Django: **[nao verificado]**. Ocorrência em produção: **[nao verificado]**. [nao verificado]

**NADA demonstrado como regressão funcional nova de A nessa contraprova:** as cópias têm a mesma pausa; o recuo ignora a duplicata e o desconto noturno une intervalos sobrepostos. Referências: `app/ponto/motor_calculo_v2.py:397`, `:404`, `:169`. Ela refuta a justificativa de disjunção, mas não prova que A altere indevidamente o split nesse caso. Efeito com pausas distintas em envelopes sobrepostos realmente produzidos pelo motor: **[nao verificado]**.

## 01/11 04:30 · sessao 8f8c0ea15497 · - · - · origem=Ronald (tmux)

**NADA — defeito procedente nos testes revisados.** O teste novo usa três objetos distintos, duas listas e o mesmo dia explícito; exige exatamente os três intervalos e cinco segmentos, portanto não é vacuidade nem deixa sobreviver a coleta apenas do primeiro período. HE zerada não impede a coleta, que fica fora da condição de HE positiva. Referências: **A:** `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:153`, `:164`, `:168`, `:170`; **A:** `app/ponto/calculador/alimentacao.py:105`; `app/ponto/calculador/nucleo.py:393`, `:400`.

**NADA — relógio capaz de mudar esses resultados localizado.** Há, sim, leitura indireta preexistente de `date.today()`: `rubricas_por_dia` chama `intervalo_cadastrado`, que consulta `pausas_do_dia(hoje)`. Entretanto, N6/N12 possuem uma pausa fixa, sem override semanal; o retorno relevante termina nos campos fixos `hii/hfi`. Referências: `app/ponto/calculador/alimentacao.py:395`, `:485`; `app/ponto/juiz_batida.py:125`, `:128`, `:135`; `app/escala/models.py:304`; **A:** `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:44`; `app/ponto/tests/test_o280_bateria_da_lei.py:1086`.

**Limite do teste unitário:** p3, com envelope de três horas e desconto de quinze minutos, não reproduz o desconto dos ramos atuais 12x36/comercial, cujo portão exige duração maior que seis horas; isso não invalida sua asserção sobre transporte dos campos recebidos. Não usá-lo como prova de que o motor produz essa fixture completa. Referências: **A:** `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:164`; `app/ponto/motor_calculo_v2.py:2755`, `:2763`, `:3216`.

Os casos integrados exigem período único, intervalo presente e resultados numéricos não nulos; N12 exige também par único não cortado. Referências: **A:** `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:72`, `:75`, `:125`, `:127`, `:130`.

Execução Django dos anexos nesta revisão: **[nao verificado]**.

## 01/11 04:30 · sessao b60f6e19db67 · - · - · origem=Ronald (tmux)

**NADA novo procedente no delta executável**, além das questões já triadas e da geometria de envelopes sob investigação. A coleta acrescentada alimenta auxiliares que recortam os intervalos ao envelope; nas noturnas, também unem sobreposições antes de medir. Referências: **A:** `app/ponto/calculador/alimentacao.py:107`, `:527`; `app/ponto/motor_calculo_v2.py:164`, `:169`, `:397`, `:399`.

A troca de `return` por `elif` preserva a prioridade da dobra sobre o excedente e libera os blocos existentes de split, noturnas e pontualidade. Não identifiquei efeito colateral novo nessa sequência. Referências: `app/ponto/calculador/regras.py:779`, `:795`, `:820`, `:840`, `:851`.

Execução integral da árvore proposta: **[nao verificado]**.

## 01/11 04:27 · sessao ea3df59c7a57 · V3-DOBRA-SEM-NOTURNA · verificador · origem=Code

**PROCEDENTE — falta cobertura para intervalos de vários períodos:** o mutante “usar só o primeiro período” sobrevive aos cenários novos, que usam um único período com pausa. **DUVIDA — a agregação por dia não prova equivalência com os intervalos próprios de cada período quando envelopes se sobrepõem.** Referências: anexo `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:72`, `:125`; anexo `app/ponto/calculador/alimentacao.py:107`; `app/ponto/calculador/regras.py:823`.

Nas referências abaixo, **A:** indica linhas do arquivo resultante apresentado no anexo; sem esse prefixo, são linhas atuais da cópia. A cópia ainda passa `intervalos=()`; a cura anterior da dobra já está nela. Referências: `app/ponto/calculador/alimentacao.py:514`; `app/ponto/calculador/regras.py:789`.

**1. Escritores e leitores**

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `PeriodoCalculo.intervalos_do_motor` | Default `()` no dataclass; atribuição de produção em `_carimbar_segmento`, que copia os pares para tupla. `app/ponto/motor_calculo_v2.py:247`, `:1830`. | Dentro do motor: `_aplicar_he_noturna`. Fora: corte dos segmentos, recálculo do período e noturno por par; A acrescenta o transporte por dia. `app/ponto/motor_calculo_v2.py:1006`; `app/ponto/calculador/alimentacao.py:121`; `app/ponto/calculador/periodo.py:741`, `:751`; **A:** `app/ponto/calculador/alimentacao.py:107`. |
| `insumos['intervalos_por_dia']` | `segmentos_do_motor` inicializa o mapa e acumula os intervalos dos períodos fechados pela chave `_dia`. **A:** `app/ponto/calculador/alimentacao.py:65`, `:107`. | `insumos_do_calculador` repassa o dict; `rubricas_por_dia` exige a chave e entrega uma tupla ao julgamento daquele dia. `app/ponto/calculador/alimentacao.py:302`, `:305`; **A:** `app/ponto/calculador/alimentacao.py:457`, `:527`. |
| Parâmetro `do_dia(intervalos=...)` | Default `()` na assinatura; A passa o conteúdo do mapa. `app/ponto/calculador/regras.py:574`; **A:** `app/ponto/calculador/alimentacao.py:527`. | Exatamente três pontos: noturnas da folga, split da HE e noturnas do ramo normal. `app/ponto/calculador/regras.py:714`, `:829`, `:842`. |

Os três motores recortam os intervalos à janela efetiva antes do carimbo: 12x36, partido e comercial. Referências: `app/ponto/motor_calculo_v2.py:2752`, `:2879`, `:3212`.

`calcular_noturnas` **não lê diretamente o atributo**: recebe o argumento local também usado no carimbo, por `_escrever_noturnas`. Referências: `app/ponto/motor_calculo_v2.py:991`, `:2780`, `:2884`, `:3235`.

Há escritores de fixture em `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:51`, `app/ponto/tests/test_s5b_vinculo_do_dia_efeito.py:48` e `app/ponto/tests/test_lote_de_celulas_nao_troca_de_colaborador.py:44`. Asserções existentes leem o campo em `app/ponto/tests/test_noturno_desconta_intervalo.py:150`, `app/ponto/tests/test_o272_intra_fora_da_janela.py:91`, `:102`, `:118` e `app/ponto/tests/test_s5b_porta_unica.py:145`; o teste novo acrescenta leituras em **A:** `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:73`, `:126`.

**2. Efeito em cada leitor; desconto duplo e formato**

| Leitor | Par cortado na pausa | Par não cortado |
|---|---|---|
| Noturnas da folga | Fica igual quando os segmentos excluem toda a pausa: a interseção é vazia. `app/ponto/calculador/regras.py:714`; `app/ponto/motor_calculo_v2.py:164`, `:167`. | Pode diminuir pela parcela noturna da pausa. `app/ponto/calculador/regras.py:85`, `:86`. |
| Noturnas do ramo normal, incluindo dobra | Mesmo comportamento da folga; a dobra segue até esse bloco após a cura anterior. `app/ponto/calculador/regras.py:795`, `:842`. | Pode diminuir pela mesma subtração. `app/ponto/calculador/regras.py:85`, `:86`. |
| Split HE50/HE100 | Pode mudar: `he_pontas` mantém o envelope, independentemente dos cortes em `pares`. `app/ponto/calculador/alimentacao.py:96`; `app/ponto/calculador/regras.py:823`. | Também pode mudar: intervalos alteram o recuo da cauda extra e o desconto dentro dela. `app/ponto/motor_calculo_v2.py:440`, `:442`. |

**PROCEDENTE — o split pode diminuir, permanecer igual ou aumentar.** Em execução isolada dos corpos das funções, com datas sintéticas em UTC−03:

- Envelope 21–07, HE100=540, pausa02–03: **540 → 480 minutos** de HE100 noturna. Referências: `app/ponto/motor_calculo_v2.py:440`, `:442`.
- Envelope 22–07, HE100=120, pausa05:30–06:30: **0 → 120 minutos**. O recuo passa das05 para04 e passa a satisfazer a condição de prorrogação. Referências: `app/ponto/motor_calculo_v2.py:395`, `:405`, `:110`.

**Não identifiquei desconto duplo no universo de segmentos que excluem a pausa.** As noturnas partem da duração noturna de cada segmento, não de `minutos_trabalhados`; o intervalo recortado ao segmento mede zero. Referências: `app/ponto/calculador/regras.py:85`, `:86`; `app/ponto/motor_calculo_v2.py:164`, `:167`.

No split, os minutos de HE determinam **quanto trabalho** recuar; depois se desconta a pausa do envelope encontrado. Essas duas operações não retiram duas vezes a pausa da mesma grandeza. Referências: `app/ponto/motor_calculo_v2.py:392`, `:400`, `:403`, `:440`, `:442`.

Pontualidade **não lê esse parâmetro**: recebe `pontualidade` e `intervalo_marcos`. Trabalhadas, intra, dobra e HE total usam `trab`, `pausa_min` e os demais argumentos próprios; portanto A não muda essas contas por meio de `intervalos`. Referências: `app/ponto/calculador/regras.py:725`, `:740`, `:760`, `:776`, `:852`.

Nenhum dos três leitores exige intervalos recortados ao **dia civil**. O formato consumido é uma sequência de pares de datetimes; os auxiliares localizam as pontas, ordenam e intersectam com o recorte da chamada. O desconto noturno ainda une intervalos sobrepostos antes de medir. Referências: `app/ponto/motor_calculo_v2.py:162`, `:164`, `:169`, `:395`, `:399`.

**3. É o mesmo insumo usado pelo motor no mesmo cálculo?**

Para um período fechado, A transporta os mesmos pares de datetimes carimbados pelo motor; ao agrupar vários períodos, passa uma lista agregada do dia, não o vínculo individual período→intervalos. Referências: `app/ponto/motor_calculo_v2.py:1830`; **A:** `app/ponto/calculador/alimentacao.py:107`, `:527`.

Há quatro distinções relevantes:

- **PROCEDENTE — pontas diferentes no split, preexistentes a A.** O motor usa `periodo.entrada/saida`; a porta publica `efetivo_entrada/efetivo_saida`. Logo, mesmo conteúdo de intervalos não garante o mesmo resultado. Referências: `app/ponto/motor_calculo_v2.py:1003`; `app/ponto/calculador/alimentacao.py:73`, `:96`. Contraprova sintética: HE100=390, pausa02–03, pontas cruas21–07 produzem390 minutos; pontas efetivas21–04:30 produzem330, pela mesma função. Referências: `app/ponto/motor_calculo_v2.py:440`, `:442`. Ocorrência desse exemplo em produção: **[nao verificado]**.

- **DUVIDA — intervalos alheios dentro do envelope.** Se períodos agrupados no mesmo dia tiverem envelopes disjuntos, o recorte elimina as pausas alheias. Se houver sobreposição, o auxiliar não distingue a origem da pausa. Referências: **A:** `app/ponto/calculador/alimentacao.py:107`; `app/ponto/calculador/regras.py:829`; `app/ponto/motor_calculo_v2.py:397`, `:399`. Contraprova sintética: envelope21–07, HE100=540, pausa própria02–03 dá480; acrescentar pausa alheia04–05 dá420 minutos. Referências: `app/ponto/motor_calculo_v2.py:395`, `:440`, `:442`. Existência dessa geometria no dado fornecido: **[nao verificado]**.

- **`periodos_ft` entram no transporte.** A porta percorre `periodos_do_resultado`, que reúne `periodos` e `periodos_ft`; a chave vem de `dia_da_jornada[id(p)]`, sem redatar pela data civil da pausa. Período sem mapa é contado e descartado. Referências: `app/ponto/calculador/nucleo.py:393`, `:400`; `app/ponto/calculador/alimentacao.py:63`, `:66`, `:68`, `:69`. Assim, períodos com entradas civis diferentes e o mesmo dia de jornada alimentam a mesma lista. Referências: `app/ponto/calculador/alimentacao.py:68`; **A:** `app/ponto/calculador/alimentacao.py:107`.

- **Turno aberto não publica esses intervalos.** Os motores retornam antes do carimbo/noturnas; a porta publica o aberto e continua antes da coleta nova. Referências: `app/ponto/motor_calculo_v2.py:2740`, `:2867`, `:3192`; `app/ponto/calculador/alimentacao.py:74`, `:82`. O crédito de pares fechados dentro de aberto copia trabalhado/fora da janela, sem copiar intervalos ou noturnas. Referências: `app/ponto/motor_calculo_v2.py:2541`, `:2547`.

O adicional normal do motor usa pontas efetivas, mas conta o **par inteiro**; a porta pode contar segmentos. Essa diferença permanece: pausa04:30–05:30 resulta em480 minutos por par e390 por segmentos, pois o segmento iniciado05:30 perde a prorrogação. É limitação anterior, não desconto duplo introduzido por A. Referências: `app/ponto/motor_calculo_v2.py:110`; `app/ponto/tests/test_o280_bateria_da_lei.py:4162`, `:4168`.

**4. Os testes mordem?** [nao verificado]

**Contra `intervalos=()`: sim, pelas asserções e pelo cálculo isolado.** O RED exige8h de HE100 noturna, enquanto a chamada sem intervalo produz9h; o gêmeo exige diferença de1h. O caso N12 inteiro exige300 minutos noturnos de relógio, enquanto sem intervalo seriam360. Referências: **A:** `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:75`, `:89`, `:130`; `app/ponto/motor_calculo_v2.py:440`, `:442`; `app/ponto/calculador/regras.py:85`.

**PROCEDENTE — contra “intervalos só do primeiro período”: não.** Os casos com pausa explicitamente têm um período; a propriedade reutiliza esses cenários. Falta uma chamada com pelo menos dois períodos e pausas distintas para distinguir esse mutante. Referências: **A:** `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:72`, `:92`, `:125`.

A propriedade “não descontar duas vezes” **alcança o universo cortado comum**: N6 tem base12x36, envelope600 minutos e pausa02–03; o motor desconta intra e a porta corta quando `intra_descontada>0`. Ela compara as noturnas da porta com as do motor, com/sem pausa e com/sem feriado. Referências: **A:** `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:44`, `:68`, `:92`; `app/ponto/motor_calculo_v2.py:2755`, `:2763`; `app/ponto/calculador/alimentacao.py:117`, `:121`.

Essa propriedade não afirma explicitamente que saíram dois segmentos, nem cobre pausa atravessando05h, múltiplos períodos ou mistura com FT. A igualdade genérica porta=motor já tem a contraprova480/390 acima. Referências: **A:** `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:92`, `:97`; `app/ponto/tests/test_o280_bateria_da_lei.py:4168`.

Execução integral dos testes novos, mutantes pela porta Django e suíte da árvore proposta: **[nao verificado]**. [nao verificado]

**5. Checklist das classes**

| Classe | Resultado nesta obra |
|---|---|
| Segundo juiz duplicando registrado | **Ausente no incremento A:** o transporte alimenta os auxiliares existentes do motor. A autoridade de cálculo está indicada em `app/docs/JUIZES.md:30`; delegações em `app/ponto/calculador/regras.py:63`, `:821`, `:829`. |
| Segundo escritor fora da porta | **Ausente para `intervalos_do_motor`:** A acrescenta leitura; a atribuição continua em `_carimbar_segmento`. O mapa novo é montado na porta. `app/ponto/motor_calculo_v2.py:1830`; **A:** `app/ponto/calculador/alimentacao.py:107`. |
| Relógio lido fora da borda | **Ausente no incremento A:** as linhas novas transportam datetimes já recebidos; não consultam “agora”. **A:** `app/ponto/calculador/alimentacao.py:107`, `:527`. |
| Derivação paralela de fonte única | **Ausente no incremento A:** copia os intervalos e chama a mesma implementação de split/desconto. **A:** `app/ponto/calculador/alimentacao.py:107`; `app/ponto/calculador/regras.py:829`; `app/ponto/motor_calculo_v2.py:442`. |
| Selo/RED sem mordida | **Presente parcialmente — PROCEDENTE:** há mordida contra vazio, mas falta contra coleta incompleta de períodos. **A:** `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:75`, `:92`, `:125`. |
| Ordem de trava | **Ausente no incremento A:** as alterações executáveis mostradas montam/consultam mapas e passam argumentos, sem acrescentar aquisição de trava. **A:** `app/ponto/calculador/alimentacao.py:65`, `:107`, `:457`, `:527`. |
| Pouso antes do pré-requisito | **DUVIDA:** o código-base contém a cura anterior exigida para alcançar split/noturnas após dobra. `app/ponto/calculador/regras.py:789`, `:795`. Evidência de suíte verde, leitura final e ordem real de pouso: **[nao verificado]**; a exigência está em `CLAUDE.md:557`. |

**6. Compatibilidade dos consumidores do dict**

**Não encontrei quebra nos chamadores concretos da cópia.** Os dois de produção obtêm os insumos pela porta antes de chamar `rubricas_por_dia`: fechamento e comando de diff. Referências: `app/ponto/services/fechamento.py:1237`, `:1240`; `app/ponto/management/commands/diff_calculador.py:312`, `:313`, `:418`.

Os chamadores de teste também usam `insumos_do_calculador`, inclusive os que fabricam períodos: `app/ponto/tests/test_s5b_vinculo_do_dia_efeito.py:111`, `:113`; `app/ponto/tests/test_lote_de_celulas_nao_troca_de_colaborador.py:120`, `:122`; `app/ponto/tests/test_he100_dia_sem_previsto.py:271`, `:272`; `app/ponto/tests/test_o280_bateria_da_lei.py:2775`, `:2776`, `:2859`, `:2861`.

Os consumidores encontrados acessam campos específicos; não apareceu serialização ou comparação estrita do dict inteiro nesses caminhos. Referências: `app/ponto/management/commands/diff_calculador.py:317`, `:318`, `:319`, `:320`, `:322`, `:323`; `app/ponto/tests/test_he100_dia_sem_previsto.py:296`.

**DUVIDA — o contrato passa a exigir a chave nova.** Um chamador manual antigo sem ela receberia `KeyError`, pois A usa indexação obrigatória; tal chamador não foi localizado na cópia. Referência: **A:** `app/ponto/calculador/alimentacao.py:457`. Consumidores externos à cópia: **[nao verificado]**.

_1377 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
