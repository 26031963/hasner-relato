# CODEX-ESTADO

> Ultima resposta processada: **17/11/2026 10:13** · publicado em **17/11/2026 10:13** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 17/11 10:13 · sessao 436fe2ea11f4 · - · - · origem=Ronald (tmux)

Reprodução **somente em memória**, dos casos Python extraídos do selo, com E/S e PIDs simulados: original passou; M1–M31 produziram `FALHA:`, sem `EXCECAO:` nem exceção escapada. Isso verifica a lógica dos casos; execução integral do shell, permissões/PIDs reais e ocorrência em produção: [nao verificado]. Os casos e o executor correspondem a `bin/tests/test_codex_ociosidade.sh:29`, `bin/tests/test_codex_ociosidade.sh:215`, `bin/tests/test_codex_ociosidade.sh:235`.

| Mutantes | Resultado/classificação e asserção |
|---|---|
| M1 | Propriedade: chamada viva passa a ACUSA; comparação de estado. (`bin/tests/test_codex_ociosidade.sh:63`, `bin/tests/test_codex_ociosidade.sh:243`) |
| M2 | Propriedade: um ABERTO antigo deixa de acusar. (`bin/tests/test_codex_ociosidade.sh:61`, `bin/tests/test_codex_ociosidade.sh:244`) |
| M3 | Propriedade: 30min00s e 10min passam a acusar. (`bin/tests/test_codex_ociosidade.sh:84`, `bin/tests/test_codex_ociosidade.sh:86`, `bin/tests/test_codex_ociosidade.sh:245`) |
| M4 | Propriedade: PID morto/ilegível passa a vivo; também elimina consulta de PID e viola contador. (`bin/tests/test_codex_ociosidade.sh:66`, `bin/tests/test_codex_ociosidade.sh:149`, `bin/tests/test_codex_ociosidade.sh:214`, `bin/tests/test_codex_ociosidade.sh:246`) |
| M5–M7 | Propriedade: ABERTO sem hora, fila ausente e log ausente deixam SEM DADO. (`bin/tests/test_codex_ociosidade.sh:76`, `bin/tests/test_codex_ociosidade.sh:80`, `bin/tests/test_codex_ociosidade.sh:81`, `bin/tests/test_codex_ociosidade.sh:247`) |
| M8 | Propriedade: PID morto publicado “em curso”. (`bin/tests/test_codex_ociosidade.sh:68`, `bin/tests/test_codex_ociosidade.sh:250`) |
| M9–M10 | Propriedade: fila malformada/diretório/ilegível passa a `ok`. (`bin/tests/test_codex_ociosidade.sh:114`, `bin/tests/test_codex_ociosidade.sh:124`, `bin/tests/test_codex_ociosidade.sh:127`, `bin/tests/test_codex_ociosidade.sh:251`) |
| M11–M14 | Propriedade: log ilegível/corrompido ou PID desconhecido deixa SEM DADO. (`bin/tests/test_codex_ociosidade.sh:129`, `bin/tests/test_codex_ociosidade.sh:146`, `bin/tests/test_codex_ociosidade.sh:149`, `bin/tests/test_codex_ociosidade.sh:155`, `bin/tests/test_codex_ociosidade.sh:253`) |
| M15 | Propriedade: 30min59s e 30min00,5s deixam de acusar. (`bin/tests/test_codex_ociosidade.sh:85`, `bin/tests/test_codex_ociosidade.sh:190`, `bin/tests/test_codex_ociosidade.sh:257`) |
| M16, M18 | Propriedade: contagens incluem borda inferior ou excluem `agora`. (`bin/tests/test_codex_ociosidade.sh:98`, `bin/tests/test_codex_ociosidade.sh:100`, `bin/tests/test_codex_ociosidade.sh:258`, `bin/tests/test_codex_ociosidade.sh:260`) |
| M17 | Propriedade: chamada futura deixa SEM DADO. (`bin/tests/test_codex_ociosidade.sh:103`, `bin/tests/test_codex_ociosidade.sh:259`) |
| M19 | Propriedade: situação sintética entra em `rc`. (`bin/tests/test_codex_ociosidade.sh:74`, `bin/tests/test_codex_ociosidade.sh:261`) |
| **M20** | **Somente texto do motivo:** auxiliares continuam SEM DADO, agora “chamadas.log nao lido”. Morre nas exigências literais de “ilegivel: PermissionError”. (`bin/tests/test_codex_ociosidade.sh:138`, `bin/tests/test_codex_ociosidade.sh:139`, `bin/tests/test_codex_ociosidade.sh:262`; `bin/codex_limite_estado.py:447`, `bin/codex_limite_estado.py:501`) |
| **M21** | **Somente texto/classe:** `sem_dado_chamadas='ilegivel: None'` continua verdadeiro, e a guarda apaga `{}`/`[]`; foto relê e permanece SEM DADO. Morre apenas nas mesmas exigências literais. **Equivalente quanto à propriedade SEM DADO, não quanto a todos os campos/saída.** (`bin/codex_limite_estado.py:97`, `bin/codex_limite_estado.py:98`, `bin/codex_limite_estado.py:99`, `bin/codex_limite_estado.py:273`; `bin/tests/test_codex_ociosidade.sh:138`, `bin/tests/test_codex_ociosidade.sh:139`, `bin/tests/test_codex_ociosidade.sh:263`) |
| M22 | Propriedade: estado vira `ok` diante do RuntimeError esperado. A morte é comparação de estado; há injeção explícita de exceção com `excecao=True`, não exceção escapada certificada como morte. (`bin/tests/test_codex_ociosidade.sh:117`, `bin/tests/test_codex_ociosidade.sh:120`, `bin/tests/test_codex_ociosidade.sh:54`, `bin/tests/test_codex_ociosidade.sh:264`) |
| M23 | Propriedade: limite ilegível passa a zero publicado. (`bin/tests/test_codex_ociosidade.sh:164`, `bin/tests/test_codex_ociosidade.sh:265`) |
| M24 | Propriedade: inválidas de limite ficam zero, em lugar de dois. (`bin/tests/test_codex_ociosidade.sh:170`, `bin/tests/test_codex_ociosidade.sh:266`) |
| M25 | Propriedade: erro semanal vira ausência sem classe/SEM DADO. (`bin/tests/test_codex_ociosidade.sh:175`, `bin/tests/test_codex_ociosidade.sh:200`, `bin/tests/test_codex_ociosidade.sh:267`) |
| M26 | Propriedade: log ausente publica papéis zerados e últimas vazias. (`bin/tests/test_codex_ociosidade.sh:182`, `bin/tests/test_codex_ociosidade.sh:183`, `bin/tests/test_codex_ociosidade.sh:268`) |
| M27 | Propriedade: `fim` inválido encerra a chamada e remove SEM DADO. (`bin/tests/test_codex_ociosidade.sh:185`, `bin/tests/test_codex_ociosidade.sh:188`, `bin/tests/test_codex_ociosidade.sh:269`) |
| M28 | Propriedade: 30min00,5s deixa de acusar. (`bin/tests/test_codex_ociosidade.sh:190`, `bin/tests/test_codex_ociosidade.sh:270`) |
| M29 | Propriedade: consulta de PID duplica, contador recebe 2 em vez de 1. O sobrevivente adicional da v2 agora morre. (`bin/tests/test_codex_ociosidade.sh:207`, `bin/tests/test_codex_ociosidade.sh:214`, `bin/tests/test_codex_ociosidade.sh:271`) |
| M30 | Propriedade de validação: `erro_semanal=None` em vez de ValueError. **O `if/elif` evita executar `linha(d20)` quando isso ocorre**, então a conversão posterior `float('lixo')` não é exercitada nesse mutante. (`bin/tests/test_codex_ociosidade.sh:200`, `bin/tests/test_codex_ociosidade.sh:201`, `bin/tests/test_codex_ociosidade.sh:272`; `bin/codex_limite_estado.py:467`) |
| **M31** | **Somente classe da exceção:** remoção da validação troca ValueError explícito por AttributeError em `pl.get`; `ler()` captura ambos e mantém SEM DADO. A comparação exige exatamente ValueError. **Equivalente quanto à propriedade SEM DADO.** (`bin/codex_limite_estado.py:89`, `bin/codex_limite_estado.py:338`, `bin/codex_limite_estado.py:340`; `bin/tests/test_codex_ociosidade.sh:200`, `bin/tests/test_codex_ociosidade.sh:273`) |

O executor agora rejeita `Traceback`/`EXCECAO:` antes de aceitar `FALHA:`, e os captures corrigidos encaminham exceções para `excecoes`; a falha anterior de certificar escapes foi corrigida nesses pontos. O executor ainda não distingue divergência de propriedade de divergência apenas textual, como M20/M21/M31. (`bin/tests/test_codex_ociosidade.sh:52`, `bin/tests/test_codex_ociosidade.sh:135`, `bin/tests/test_codex_ociosidade.sh:166`, `bin/tests/test_codex_ociosidade.sh:177`, `bin/tests/test_codex_ociosidade.sh:236`, `bin/tests/test_codex_ociosidade.sh:238`)

O relaxamento adicional `b>=1`→`b>=0` continua equivalente sob os invariantes: zero ABERTO implica idade `None`, barrada por `s is not None`; M2 testa `b>1`, uma mutação diferente. O caso “fila é diretório” agora alcança `open`, portanto a vacuidade antiga foi corrigida. (`bin/codex_limite_estado.py:225`, `bin/codex_limite_estado.py:259`, `bin/codex_limite_estado.py:304`; `bin/tests/test_codex_ociosidade.sh:113`, `bin/tests/test_codex_ociosidade.sh:244`)

## 17/11 10:13 · sessao 6fee9ce867f3 · RESPOSTAS-1711-1005 · verificador · origem=Code

**AVAL-DEVOLVIDO: item 3**, pela premissa de encaminhamento atual e pela expressão ampla “ausência registrada”. O corte que atribui CADASTRO pode resolver a escolha aberta, mas esse destino ainda precisa chegar ao juiz e à lista. `app/docs/LEIS.md:122`; `app/docs/RELATO.md:352`; `app/ponto/management/commands/e6_oraculo.py:53`; `app/escala/services/cadastro_realidade.py:728`.

**Item 4 CONFERE para a opção A do conflito texto corrente versus foto de REABERTURA/RECUSA.** Isso não significa que todo consumidor ignore `ato_texto`: a tarefa já lê esse campo quando o último ato é RESPOSTA. `app/docs/LEIS.md:151`; `app/chamados/services/disputa_emissao.py:1632`; `app/chamados/services/tarefa_do_dia.py:159`.

**Item 3 — as quatro perguntas**

1. **As premissas conferem? Parcialmente.** L-099 define ESTRUTURA/CADASTRO/BATIDA, reserva construção a ESTRUTURA e destina CADASTRO/BATIDA à lista do admin. Ela não atribuía, sozinha, CADASTRO a FATO_EM_AUSENCIA; essa escolha permanecia aberta. `app/docs/LEIS.md:122`; `app/docs/RELATO.md:352`.

   O juiz existente é `e6_oraculo.dono_da_divergencia`, carregado por `InsumosDoDono.dono(data)`. Sua assinatura não recebe FATO_EM_AUSENCIA; seus ramos ainda podem devolver BATIDA ou ESTRUTURA. Portanto, o novo dono não está realizado nesse juiz. `app/ponto/management/commands/e6_oraculo.py:53`; `app/ponto/management/commands/e6_oraculo.py:98`; `app/ponto/management/commands/e6_oraculo.py:182`.

   **Existe sinal na lista Cadastro x Realidade, mas não o caminho geral prometido:** A9 tem rótulo “ausência aprovada com batida”, dono literal `juiz`, exige `ausencia_id` e pelo menos duas batidas. Entra agregado, sem data/dono diário, e recebe a ação **“Com o sistema: em cura (nao e do cadastro)”**, sem URL. `app/ponto/services/esmeril_espelho.py:23`; `app/ponto/services/esmeril_espelho.py:133`; `app/escala/services/cadastro_realidade.py:923`; `app/escala/services/cadastro_realidade.py:728`.

   A rota reconhecida pelo vigia é **Cadastro x Realidade → `dias_na_lista` → `ListaDoAdmin.dia_do_admin` → `fio_do_admin`**. Ela exige assinatura datada e dono CADASTRO/BATIDA; o A9 agregado não cumpre essas condições. `app/escala/services/cadastro_realidade.py:1047`; `app/chamados/services/vigia_fio.py:200`; `app/chamados/services/vigia_fio.py:261`.

   Há outra exposição administrativa, restrita: a tarefa oferece VALIDAR/ADMIN quando o dia não é exportado, está sem trabalho, tem veredito `fato_em_ausencia` e nenhuma pergunta viva C6. Isso não constitui a rota geral à lista com dono L-099. `app/chamados/services/tarefa_do_dia.py:352`.

2. **Colide com corte vigente?** Atribuir CADASTRO resolve a escolha aberta e é compatível com L-099; não autoriza ampliar a classe FATO_EM_AUSENCIA. Parcial sem período continua CADASTRO com código próprio; parcial fora do período não entra nesse FATO. `app/docs/LEIS.md:122`; `app/docs/RELATO.md:352`; `app/docs/CORTES.md:386`; `app/ponto/supra_juiz.py:395`; `app/ponto/supra_juiz.py:403`.

3. **Alguma condição não fecha no literal? Sim.** “Ausência registrada” abrange mais que a classificação atual: o juiz distingue cobertura aprovada, período parcial e fato do dia; coexistência de registro e batida não basta. O brief deve referir-se à **classe lavrada pelo juiz**, preservando essas distinções. `app/ponto/supra_juiz.py:392`; `app/ponto/supra_juiz.py:395`; `app/ponto/supra_juiz.py:413`; `app/ponto/turnos.py:2523`.

   O bloqueio por cruzamento pode permanecer como condição do aval; o cruzamento autenticado atual com B2+delta/F2 não é demonstrável pelas listas disponíveis no dado. [nao verificado]

4. **Toca exportada, dinheiro ou `!` além do declarado?** O item manda registrar o corte e manter a obra sem abrir; não concede execução de ajustes de dados. [nao verificado: literal do anexo]

   As ações mencionadas têm efeito financeiro: ajustar ausência pode mudar insumo da folha e acordar projeções; “descartar batida” deve significar **retratar**, preservando o registro e retirando minutos da apuração. `app/ponto/services/ausencia.py:989`; `app/ponto/services/ausencia.py:1123`; `app/ponto/registro_batida.py:289`; `app/ponto/services/retratar_humano.py:134`.

   Não cabe presumir exportadas genericamente protegidas: L-113 alterou a restrição anterior, e o evento pode recalcular competência exportada. Isso não transforma o registro do item 3 em autorização de correção ou ato de frota. `app/docs/LEIS.md:155`; `app/ponto/services/fechamento.py:1790`; `app/docs/LEIS.md:62`.

**Item 4 — as quatro perguntas**

1. **As premissas conferem? Sim, no conflito da opção A.** Reabrir/recusar guarda a resposta retirada em `ato_texto` e limpa o par corrente. O histórico de respostas anteriores lê essas fotos. `app/chamados/services/disputa_emissao.py:1609`; `app/chamados/services/disputa_emissao.py:1632`; `app/chamados/services/disputa_emissao.py:1648`; `app/chamados/models.py:3667`.

   Os leitores comprovados do texto corrente são:

   | Leitor | Evidência |
   |---|---|
   | Juiz `pergunta_respondida` e C6 | `app/chamados/juizes.py:1552`; `app/chamados/juizes.py:1389`; `app/chamados/juizes.py:1423` |
   | Validação e materialização de batida | `app/chamados/services/validacao.py:1094`; `app/chamados/services/materializacao.py:625`; `app/chamados/services/materializacao.py:678` |
   | Calendário e prévia da folha | `app/colaboradores/services/calendario.py:494`; `app/folha/previa.py:142` |
   | Raio X, API e apresentação | `app/chamados/services/raiox.py:138`; `app/api/views.py:2561`; `app/chamados/models.py:3686` |
   | Guarda do fechamento por SILENCIO | `app/chamados/models.py:1923`; `app/chamados/models.py:1968` |

   **SILENCIO** consulta respostas sem veredito pelo par corrente e registra o ato copiando `resposta_colab`; não recupera o texto retirado de REABERTURA/RECUSA. Na tarefa, último ato SILENCIO retorna nenhuma ação. `app/chamados/models.py:1923`; `app/chamados/models.py:2005`; `app/chamados/services/tarefa_do_dia.py:176`.

   **Exceção à leitura universal:** a tarefa consulta `ult.ato_texto` no ramo RESPOSTA; em REABERTURA/RECUSA consulta o tipo do ato. Logo, “texto fica histórico” não pode virar proibição de toda leitura operacional de `ato_texto`. `app/chamados/services/tarefa_do_dia.py:159`; `app/chamados/services/tarefa_do_dia.py:163`.

2. **Colide com corte vigente?** A opção A conserva a L-130: reabertura/ressuscitação limpa a resposta inteira e mantém o texto anterior na trilha. Continua vigente o corte F8 anterior: validação humana legada não é revogada por RESPOSTA sintética posterior, com nenhuma batida validada alterada. `app/docs/LEIS.md:151`; `app/docs/CORTES.md:488`.

3. **Alguma condição não fecha no literal?** A escolha documental fecha nesse recorte; a construção futura continua devendo a medida de neutralidade exigida pelo corte anterior. `app/docs/CORTES.md:488`.

   O raspado declara 226 divergências, distribuídas em 214 REABERTURAS e 12 RECUSAS, mas não comprova o DIFF real nem neutralidade das batidas; as linhas `COL` somam 191, divergindo do total declarado. [nao verificado]

4. **Toca exportada, dinheiro ou `!` além do declarado?** Conservar a fonte corrente e registrar a escolha não exige substituir texto, plantar batida ou recalcular folha. A futura implementação não recebe certificado de dinheiro zero: o materializador usa esse texto para produzir fatos, e o corte anterior mantém a parada se o DIFF mudar o julgamento em exportada. `app/chamados/services/materializacao.py:678`; `app/docs/CORTES.md:488`.

**Quem escreve e quem lê os campos**

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `resposta_colab` | Resposta humana e automática pelas portas de `disputa_emissao`; reabertura/recusa e ressuscitação zeram pela porta de descarimbo. `app/chamados/services/disputa_emissao.py:1481`; `app/chamados/services/disputa_emissao.py:1674`; `app/chamados/services/disputa_emissao.py:1648`; `app/chamados/models.py:1629`. | Juiz/C6, validação, materializador, calendário, prévia, API, apresentação e guarda do SILENCIO, nos sítios relacionados acima. |
| `ato_texto` | Porta única `DecisaoPergunta.registrar`; recebe resposta, reabertura/recusa, validação, devolução, SILENCIO, absorção e backfill. `app/chamados/models.py:4110`; `app/chamados/services/disputa_emissao.py:1525`; `app/chamados/services/disputa_emissao.py:1632`; `app/chamados/services/validacao.py:175`; `app/chamados/services/validacao.py:281`; `app/chamados/models.py:2005`; `app/chamados/services/materializacao.py:1047`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:326`. | Histórico de respostas anteriores; tarefa no ramo último ato RESPOSTA. `app/chamados/models.py:3675`; `app/chamados/services/tarefa_do_dia.py:160`. |
| Ausência/status/período | Colaborador/API e admin chamam criação/edição; transição normal usa `mudar_estado`, decisão/período usa `decidir`. Férias derivadas têm escrita própria de status na mesma família de serviço. `app/api/views.py:1812`; `app/ponto/views.py:2948`; `app/ponto/services/ausencia.py:955`; `app/ponto/services/ausencia.py:1088`; `app/ponto/services/ausencia.py:1101`; `app/ponto/services/ausencia.py:1496`. | Cobertura: folha somente aprovada, cobrança não rejeitada; suprajuiz, tarefa, vigia e fechamento consomem esses insumos. `app/ponto/turnos.py:2523`; `app/ponto/supra_juiz.py:395`; `app/chamados/services/tarefa_do_dia.py:178`; `app/chamados/services/vigia_fio.py:256`; `app/ponto/services/fechamento.py:638`. |
| Batida | API e materializador chamam `registrar_batida`; correção usa `anotar`; retratação humana autorizada chama `retratar_batida`, que preserva a linha. `app/api/views_core.py:645`; `app/chamados/services/materializacao.py:1570`; `app/ponto/registro_batida.py:255`; `app/ponto/services/retratar_humano.py:163`; `app/ponto/registro_batida.py:287`. | `batidas_apuraveis` exclui retratadas; juiz de dono, fechamento e export consomem a apuração; A9 conta batidas para sinalização. `app/ponto/turnos.py:2868`; `app/ponto/management/commands/e6_oraculo.py:150`; `app/ponto/services/fechamento.py:632`; `app/folha/export.py:499`; `app/ponto/services/esmeril_espelho.py:133`. |

VEREDITO: item 3 PROCEDE; item 4 CONFERE -- falta a rota diária CADASTRO/admin prometida no item 3 (app/escala/services/cadastro_realidade.py:728; app/escala/services/cadastro_realidade.py:1047); opção A conserva texto corrente e foto histórica de REABERTURA/RECUSA (app/docs/LEIS.md:151; app/chamados/services/disputa_emissao.py:1632).

## 17/11 10:13 · sessao 689b1f17ac1e · - · - · origem=Ronald (tmux)

A composição ainda pode cair por entrada de sessão; confirmei `linha(ler())` somente em memória, sem escrever arquivos. O guard de `ler()` protege a leitura semanal, mas não a formatação posterior; `used_percent=10**400` é aceito como `int`, `erro_semanal` fica `None`, e `float(...)` levanta `OverflowError` na montagem da frase. A chamada do gerador não tem guarda local; `relato.sh` propaga saída 1. (`bin/codex_limite_estado.py:87`, `bin/codex_limite_estado.py:348`, `bin/codex_limite_estado.py:467`, `bin/codex_limite_estado.py:472`, `bin/gerar_estado.py:270`, `bin/relato.sh:38`)

Reprodução usada: E/S substituída por `StringIO`, sessão `{"payload":{"rate_limits":{"primary":{"used_percent":10**400,"window_minutes":10080}}}}` — com o inteiro escrito por extenso no JSON —, chamadas concluídas e fila vazia. Resultado: `ler erro_semanal=None ociosidade=ok`; `linha(ler()) => OverflowError int too large to convert to float`. Isso decorre diretamente da validação que admite qualquer inteiro e da conversão posterior. (`bin/codex_limite_estado.py:348`, `bin/codex_limite_estado.py:467`)

Outros casos de esquema inválido permanecem tratados como ausência de informação:

- `rate_limits=false`, `rate_limits=0` ou `rate_limits=[]` viram `{}` pelo `or {}` e não recebem `erro_semanal`; o resultado é `[sem used_percent]`. (`bin/codex_limite_estado.py:340`, `bin/codex_limite_estado.py:352`, `bin/codex_limite_estado.py:468`)
- `primary=[1]` vira `primary=None`, também sem SEM DADO pela fonte; confirmei em memória. (`bin/codex_limite_estado.py:345`, `bin/codex_limite_estado.py:350`, `bin/codex_limite_estado.py:468`)
- Uma última linha contendo `rate_limits` com JSON sintaticamente inválido é ignorada; se não há outra candidata, retorna `None` e publica `[sem rate_limits nas sessoes]`, sem classificar a corrupção. (`bin/codex_limite_estado.py:328`, `bin/codex_limite_estado.py:334`, `bin/codex_limite_estado.py:353`, `bin/codex_limite_estado.py:460`)

A validação do `fim` foi ampliada para exigir `rc`/`dur` inteiros, mas ainda descarta o horário do fim e aceita PID inválido quando existe fim da mesma chave: `_em_curso` retorna `False` antes de consultar `_pid_vivo`. Em memória, `ini` às 09:50 com PID `abc` e `fim` às 08:00 da mesma chave produziu `descartadas=0`, `em_curso=0`, `estado=ok` e últimas `('0','terminou')` com fila vazia. Portanto, não há validação de cronologia ou da identidade do PID de chamadas concluídas. (`bin/codex_limite_estado.py:136`, `bin/codex_limite_estado.py:143`, `bin/codex_limite_estado.py:147`, `bin/codex_limite_estado.py:213`, `bin/codex_limite_estado.py:304`)

Os exemplos específicos de v2 com `payload=[1]` e `used_percent="lixo"` estão corrigidos: o leitor levanta `ValueError`, e `ler()` converte para SEM DADO antes da frase. O reparo não cobre todos os valores numéricos nem todas as estruturas inválidas. (`bin/codex_limite_estado.py:338`, `bin/codex_limite_estado.py:348`, `bin/codex_limite_estado.py:89`, `bin/codex_limite_estado.py:458`)

Execução integral do selo e ocorrência desses defeitos em produção: [nao verificado].

## 17/11 10:12 · sessao 39528a968e59 · - · - · origem=Ronald (tmux)

Persistem dois achados de v2: relógio próprio na contagem por papel do disjuntor e borda inferior inclusiva no censo. O produtor real da fila continua [nao verificado]. (`bin/disjuntor.py:2036`, `bin/codex_limite_estado.py:167`, `bin/disjuntor.py:2132`, `bin/tests/test_codex_ociosidade.sh:40`)

| Campo | Quem escreve | Quem lê / contrato |
|---|---|---|
| `ini`: hora, obra, papel, hash, PID, pergunta | Wrapper grava `date` sem offset, `OBRA`, `PAPEL`, hash de 12 caracteres, `$$`, pergunta raspada. O registro antecede a espera por slot. (`bin/codex_pergunta.sh:295`, `bin/codex_pergunta.sh:308`, `bin/codex_pergunta.sh:364`) | `_registro` conserva hora, obra, papel, PID e chave `(obra,hash,pid)`; pergunta descartada. Papéis conta inícios; últimas e ociosidade compartilham `_foto`. (`bin/codex_limite_estado.py:143`, `bin/codex_limite_estado.py:145`, `bin/codex_limite_estado.py:94`, `bin/codex_limite_estado.py:105`) |
| `fim`: hora, obra, papel, hash, PID, sessão, rc, duração | `_saida`, pelo trap EXIT após `INI=1`; `rc=$?`, sessão ou `-`, duração em segundos por diferença de epoch desde `T0C`. (`bin/codex_pergunta.sh:265`, `bin/codex_pergunta.sh:269`, `bin/codex_pergunta.sh:272`, `bin/codex_pergunta.sh:282`) | Valida timestamp e sintaxe inteira de rc/duração; conserva sessão, rc e duração como texto. Timestamp e papel do fim são descartados. `_em_curso` usa apenas a existência da chave. Não compara fim com início nem duração com timestamps. (`bin/codex_limite_estado.py:131`, `bin/codex_limite_estado.py:137`, `bin/codex_limite_estado.py:147`, `bin/codex_limite_estado.py:213`) |
| `fila.md`: identificador, estado, data/hora | Escritor real e procedimento de abertura: [nao verificado]. Fixture escreve o arquivo. (`bin/tests/test_codex_ociosidade.sh:39`) | `ler_fila_reserva` aceita os quatro estados; conta ABERTO e exige carimbo em minutos; calcula idade em segundos de ponto flutuante, sem truncamento. Ociosidade consome quantidade/idade/erro. (`bin/codex_limite_estado.py:53`, `bin/codex_limite_estado.py:240`, `bin/codex_limite_estado.py:251`, `bin/codex_limite_estado.py:295`) |
| `limite.log`: hora, obra, mensagem | Wrapper escreve quando RC diferente de 0/124 e mensagem corresponde ao padrão de limite/indisponibilidade. (`bin/codex_pergunta.sh:451`, `bin/codex_pergunta.sh:453`, `bin/codex_pergunta.sh:456`) | `ler` exige três campos e timestamp válido/não futuro, mas não valida obra/mensagem. Ausência significa zero por contrato declarado. (`bin/codex_limite_estado.py:6`, `bin/codex_limite_estado.py:73`, `bin/codex_limite_estado.py:78`) |
| Tupla `(hora,obra,papel,rc,situacao)` | `ultimas_chamadas` escreve cinco campos; rc é texto numérico ou `None`, situação é campo separado. (`bin/codex_limite_estado.py:185`, `bin/codex_limite_estado.py:187`) | `_ultimas_txt` desempacota cinco campos e apresenta rc quando existe; os testes verificam índices 3 e 4. (`bin/codex_limite_estado.py:505`, `bin/tests/test_codex_ociosidade.sh:68`, `bin/tests/test_codex_ociosidade.sh:72`) |

Outros consumidores relêem chamadas pelo adaptador que elimina diagnósticos de entradas inválidas/futuras: procedência de sessões, tokens por papel e censo. Procedência alimenta `gerar_codex_estado`; tokens alimentam o disjuntor. (`bin/codex_limite_estado.py:151`, `bin/codex_limite_estado.py:154`, `bin/codex_limite_estado.py:312`, `bin/codex_limite_estado.py:418`, `bin/disjuntor.py:2126`, `bin/gerar_codex_estado.py:176`, `bin/disjuntor.py:2028`)

O módulo compartilha `(agora−24h,agora]` para limite, papéis e contexto horário. O censo usa `[agora−86400s,agora]` e converte timestamps locais para epoch; portanto as bordas não são uniformes. O disjuntor também consulta papéis sem transmitir seu relógio. (`bin/codex_limite_estado.py:59`, `bin/codex_limite_estado.py:83`, `bin/codex_limite_estado.py:172`, `bin/codex_limite_estado.py:291`, `bin/disjuntor.py:2085`, `bin/disjuntor.py:2132`, `bin/disjuntor.py:2036`)

Os produtores usam hora local sem offset e o parser cria datetime ingênuo; igualdade efetiva entre os fusos dos processos: [nao verificado]. (`bin/codex_pergunta.sh:272`, `bin/codex_pergunta.sh:308`, `bin/codex_pergunta.sh:456`, `bin/codex_limite_estado.py:63`, `bin/codex_limite_estado.py:131`)

Há erro semântico de entrada que ainda pode virar `ok`: um `fim` da mesma chave anterior ao `ini` passa pela validação sintática, encerra a chamada e não gera diagnóstico; com fila sem ABERTO a decisão retorna `ok`. Também PID inválido associado a fim não é examinado, porque o predicado retorna antes da consulta de PID. Ocorrência em produção: [nao verificado]. (`bin/codex_limite_estado.py:137`, `bin/codex_limite_estado.py:147`, `bin/codex_limite_estado.py:213`, `bin/codex_limite_estado.py:304`)

## 17/11 10:12 · sessao 2691ab6f2cfa · PORTA-DO-EXPORT-SEM-BECO · leitura · origem=Code

**(b)** A justificativa literal é **“SELO DE PORTA, nao de pipeline”**: conferir tela == PDF == fechamento == TXT antes do export. Agendar mediria competências que ninguém exportará naquele dia e “encher[ia] log de verde”; o comando também reúne os dois contadores que antes existiam soltos. (`app/config/crons.py:887-897`)

Os documentos sustentam o seguinte:

- **RELATO-ARQUIVO:** registra que o contrato exigia cada comando em `CRONS` ou declarado com motivo em `FORA_DE_PIPELINE`; o autor escreveu “Declarei: selo de PORTA, nao de pipeline”, repetindo a justificativa contra agendamento. É uma decisão de implementação documentada; aval nominal específico para essa classificação: **[nao verificado]**. (`app/docs/RELATO-ARQUIVO.md:5014-5019`)
- **BACKLOG:** registra `SELO-TXT` fechado em 28/09, igualdade dos quatro leitores, selo verde nos 205 daquele universo e “Export so depois disto, com o `!`”. Essa linha não determina periodicidade diária. (`app/docs/BACKLOG.md:1203`)
- **CORTES:** declara registrar cortes/avais de Ronald; ordem específica para manter esse selo fora do pipeline: **[nao verificado]**. Os cortes sobre “carimbo da porta cega” tratam do carimbo de aprovação e de conta vazia/turno aberto. (`app/docs/CORTES.md:5-7`, `app/docs/CORTES.md:315`, `app/docs/CORTES.md:317`)
- **LAPIDES:** declara guardar histórias copiadas do `CLAUDE.md`; decisão específica sobre o agendamento deste selo: **[nao verificado]**. (`app/docs/LAPIDES.md:7-21`)

“Roda no ATO” é a formulação de `crons.py`; o caminho executável atual exige medição anterior pelo comando e **somente confere** durante o POST. (`app/config/crons.py:889-896`, `app/folha/views.py:124-133`) Git log e autoria histórica além desses registros: **[nao verificado]**.

**(c)** Nesta cópia, a cadeia operacional é:

- **Escreve:** `selo_leitores_no_mesmo_numero.Command.handle` chama `medir` e depois `lavrar` para cada empresa ativa. `lavrar` grava `MetricaSnapshot` com `CHAVE='porta_export_leitores'`; grava inclusive resultado negativo ou divergente, não apenas verde. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:33`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86-95`, `app/folha/porta_export.py:18`, `app/folha/porta_export.py:768-796`)
- **Lê:** `conferir` busca o snapshot de hoje e verifica competência e valor; `gerar_dominio_txt` consome sua mensagem e recusa antes de montar o TXT. Só valor zero passa essa porta. (`app/folha/porta_export.py:800-821`, `app/folha/porta_export.py:865-867`, `app/folha/views.py:128-133`)

O censo funcional de produção é **um escritor (`lavrar`) e um leitor (`conferir`)**, com comando e view como respectivos chamadores. O helper `lavrar_verde_para_teste` também chama `lavrar`, mas se declara exclusivo de testes e fica fora desse censo. (`app/folha/porta_export.py:768`, `app/folha/porta_export.py:800`, `app/folha/porta_export.py:870-881`)

A identidade persistida é **chave + escopo + empresa + dia**; mês/ano ficam somente nos `defaults.detalhe`. A restrição única do modelo confirma essa identidade sem competência. (`app/folha/porta_export.py:772-776`, `app/inteligencia/models.py:102-105`)

- **Mesma empresa, mesmo dia:** carimbar A e depois B atualiza a mesma linha, substituindo valor e detalhe pelos de B. Pedir A passa a receber recusa de competência incompatível; pedir B ainda depende de valor zero. Empresas diferentes têm chaves separadas. (`app/folha/porta_export.py:772-776`, `app/folha/porta_export.py:815-821`)
- **Mesma empresa, dias diferentes:** são linhas distintas. Se A foi carimbada ontem e B hoje, A de ontem permanece no histórico, mas não libera export hoje; havendo B hoje, pedir A recebe incompatibilidade. Sem qualquer linha hoje, recebe “NAO FOI MEDIDA HOJE”. (`app/inteligencia/models.py:102-105`, `app/folha/porta_export.py:772-776`, `app/folha/porta_export.py:807-817`)

**(d)** A string literal, concatenada, da ausência de carimbo é esta; os `%s` recebem razão social e comando. (`app/folha/porta_export.py:812-813`)

```text
A PORTA DO EXPORT NAO FOI MEDIDA HOJE para %s. O TXT nao sai sem a prova de que a tela, o PDF, o fechamento e o TXT estao no mesmo numero. Rode: %s
```

O segundo `%s` contém este comando literal formatado com mês e ano pedidos. (`app/folha/porta_export.py:809-810`)

```text
docker exec saas_core python manage.py tenant_command selo_leitores_no_mesmo_numero --schema=juliani --mes %02d --ano %s
```

O botão mostra **`Gerar TXT`** e faz POST para `folha:gerar_txt`; a view passa a recusa intacta como `msg=_barra`, com `ok=False`, redirecionando para `folha:exportar`. O helper guarda a mensagem de erro, a view de destino renderiza `folha/exportar.html`, e o template a imprime literalmente por `{{ m }}`. (`app/templates/folha/index.html:63`, `app/templates/folha/index.html:107-109`, `app/folha/views.py:128-133`, `app/core/respostas.py:50-51`, `app/core/respostas.py:68-71`, `app/folha/views.py:92`, `app/templates/folha/exportar.html:10`) Razão social e mensagem efetivamente vistas em produção: **[nao verificado]**.

**(e)** O pré-desenho usa a infraestrutura declarada: `saas_ui`, `gthread`, `timeout=30`, com orientação “trabalho longo vai pra job”. Os ~100 s citados na porta são uma medida histórica documentada; tempo atual da competência 11 por empresa: **[nao verificado]**. A própria view ressalva que `timeout=30` não implica matar toda requisição longa aos 30 s. (`CLAUDE.md:89`, `app/gunicorn.conf.py:19-22`, `app/folha/porta_export.py:7-12`, `app/folha/views.py:182-195`)

| Alternativa | Quem escreveria | O que o admin veria | Risco inferido do código |
|---|---|---|---|
| Medir no clique, dentro do request | O request chamaria `medir → lavrar`, antes de `conferir`. (`app/folha/porta_export.py:768`, `app/folha/porta_export.py:800`) | Espera pela medição; depois, continuidade do TXT ou recusa. (`app/folha/views.py:129-134`) | Trabalho longo ocupa atendimento da UI; duração e resultado real de timeout **[nao verificado]**. (`CLAUDE.md:89`, `app/gunicorn.conf.py:19-22`) |
| Cron diário | O agendador acionaria o comando existente, que chama `lavrar`. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86-92`) | Conferência do carimbo pronto; falta, outra competência ou falha continuam recusando. (`app/folha/porta_export.py:807-821`) | Medição sem export previsto; outra competência pode substituir a primeira; mudanças posteriores no mesmo dia não invalidam automaticamente a foto. (`app/config/crons.py:894-895`, `app/folha/porta_export.py:772-776`, `app/folha/porta_export.py:807-818`) |
| Cron + nova medição no clique | Dois gatilhos chamariam o mesmo `lavrar`. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:92`, `app/folha/porta_export.py:768`) | Espera pela nova medição e recebe seu resultado. (`app/folha/views.py:129-134`) | Repete trabalho e mantém o custo no request; ambos disputam a mesma chave diária por empresa. (`app/folha/porta_export.py:7-12`, `app/folha/porta_export.py:772-776`) |

PREPARA: perguntas de lei 0; escritores do carimbo 1; leitores 1

## 17/11 10:12 · sessao 07165033cd08 · - · - · origem=Ronald (tmux)

Item 4: **CONFERE**, entendido no recorte da opção A: conservar o texto corrente para os leitores existentes e manter o texto retirado por REABERTURA/RECUSA na trilha; esse comportamento já corresponde à L-130. `app/docs/LEIS.md:151`; `app/chamados/services/disputa_emissao.py:1609`; `app/chamados/services/disputa_emissao.py:1632`; `app/chamados/services/disputa_emissao.py:1648`.

1. **Premissas e leitores.** `resposta_colab` é o texto lido pelo juiz `pergunta_respondida`, que exige instante E texto após trim; C6 usa a projeção corrente temporal. `app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1552`; `app/chamados/juizes.py:1389`; `app/chamados/juizes.py:1423`.

   Outros leitores comprovados do texto corrente:

   - Calendário, último recurso: consulta texto e C6. `app/colaboradores/services/calendario.py:494`; `app/colaboradores/services/calendario.py:504`; `app/colaboradores/services/calendario.py:506`.
   - Prévia, `_material`: decide pela presença do texto. `app/folha/previa.py:139`; `app/folha/previa.py:142`.
   - Materializador: texto para interpretar/plantar hora. `app/chamados/services/materializacao.py:625`; `app/chamados/services/materializacao.py:678`; `app/chamados/services/materializacao.py:718`.
   - Validação/classificação: texto para escape, confirmação e hora. `app/chamados/services/validacao.py:1094`; `app/chamados/services/validacao.py:1105`.
   - Raio X: par corrente submetido ao juiz. `app/chamados/services/raiox.py:138`; `app/chamados/services/raiox.py:147`.
   - API e apresentação: campo corrente e seu display. `app/api/views.py:1163`; `app/api/views.py:2561`; `app/chamados/models.py:3686`; `app/chamados/models.py:3710`.
   - SILENCIO: a guarda de respostas sem veredito lê o par corrente; o fechamento grava uma decisão copiando esse texto. `app/chamados/models.py:1910`; `app/chamados/models.py:1923`; `app/chamados/models.py:1968`; `app/chamados/models.py:2005`.

   **Exceção que deve ficar explícita:** não vale afirmar que *todo* leitor atual ignora `ato_texto`. A tarefa usa `ult.ato_texto` no ramo RESPOSTA; nos ramos REABERTURA/RECUSA usa o tipo do ato, e SILENCIO retorna nenhuma ação sem ler texto. `app/chamados/services/tarefa_do_dia.py:159`; `app/chamados/services/tarefa_do_dia.py:160`; `app/chamados/services/tarefa_do_dia.py:163`; `app/chamados/services/tarefa_do_dia.py:176`. O raspado atribui as 226 divergências a 214 REABERTURAS e 12 RECUSAS, portanto não demonstra divergências no ramo RESPOSTA. [nao verificado]

2. **Colisão.** Não identifiquei colisão da opção A nesse recorte: L-130 exige o texto anterior na trilha e o par inteiro zerado; L-002 exige autoridade compartilhada. `app/docs/LEIS.md:151`; `app/docs/LEIS.md:60`. Permanece o corte F8 anterior: validação humana legada segue válida apesar de RESPOSTA sintética posterior, com medida de neutralidade e nenhuma batida validada alterada. `app/docs/CORTES.md:488`.

3. **Literal/condições.** A afirmação “`ato_texto` fica histórico” precisa ser lida como texto retirado no recorte P2, não como proibição universal de leitura do campo: a tarefa já o lê. `app/chamados/models.py:4020`; `app/chamados/models.py:3667`; `app/chamados/services/tarefa_do_dia.py:160`. O raspado não comprova DIFF real da F8, neutralidade das batidas, nem a população integral: totais `NUMERO` e soma das linhas `COL` divergem. [nao verificado]

4. **Exportada/dinheiro/!.** Registrar a opção A não requer trocar texto, materializar batidas ou regerar folha; os escritores mostrados só explicam o comportamento existente. `app/chamados/services/disputa_emissao.py:1609`; `app/chamados/services/disputa_emissao.py:1632`. Não é prova de dinheiro zero numa futura construção: o materializador deriva plantio do texto, e o corte anterior exige parar se a classificação exportada mudar. `app/chamados/services/materializacao.py:678`; `app/chamados/services/materializacao.py:718`; `app/docs/CORTES.md:488`. Eventual efeito produtivo atual é [nao verificado].

**Mapa de escrita/leitura dos dois campos:**

| Campo | Escritores | Leitores |
|---|---|---|
| `resposta_colab` | Resposta humana: `disputa_emissao.py:1481`, persistida em `:1522`; resposta automática `:1674`, persistida em `:1677`; reabertura/recusa fornece vazio em `:1609` e delega em `:1648` à porta `models.py:3362`/`:3391`; ressuscitar também zera o par `models.py:1630`; nascimento aceita campos pela porta `models.py:2930` com guarda em `:2949`. | Juiz/C6, calendário, prévia, materializador, validação, Raio X, API, display e guarda do SILENCIO, conforme referências acima. |
| `ato_texto` | Porta única `DecisaoPergunta.registrar`, `models.py:4110`/`:4120`; recebe linhas da resposta `disputa_emissao.py:1525`, reabertura/recusa `:1632`, automática `:1678`, recuperação pela trilha `:1699`; validação `validacao.py:175`, devolução `:281`/`:356`; SILENCIO `models.py:2005`, retratação `:2137`, absorção `materializacao.py:1047`, decisão do dia `ponto/services/veredito_celula.py:197`, backfill `backfill_decisoes_pergunta.py:326`. | Histórico `models.py:3667`/`:3675`; tarefa, último ato RESPOSTA, `tarefa_do_dia.py:159`/`:160`. |

Não há alteração direta de `ato_texto`: decisões existentes recusam reescrita, exclusão e atualização em lote. `app/chamados/models.py:4002`; `app/chamados/models.py:4070`; `app/chamados/models.py:4075`.

## 17/11 10:12 · sessao 2c2cbc136325 · - · - · origem=Ronald (tmux)

Mapa verificado para os campos do item 3:

| Campo | Quem escreve | Quem lê |
|---|---|---|
| Ausência: tipo, datas, minutos | Colaborador pela API chama `criar_ausencia`; admin cria e edita pela mesma família de portas. `app/api/views.py:1812`; `app/ponto/views.py:2682`; `app/ponto/views.py:2948`; escrita em `app/ponto/services/ausencia.py:483` e `app/ponto/services/ausencia.py:648`. | Autoridades `ausencias_do_periodo`/`ausencia_cobre` leem intervalo e efeito; fechamento consome o queryset da folha. `app/ponto/turnos.py:2535`; `app/ponto/turnos.py:2550`; `app/ponto/services/fechamento.py:638`. |
| Ausência: status | Nasce de `estado_inicial`, salvo em `criar_ausencia`; transição normal por `mudar_estado`; decisão DP passa por `decidir`. `app/ponto/services/ausencia.py:475`; `app/ponto/services/ausencia.py:486`; `app/ponto/services/ausencia.py:955`; `app/ponto/services/ausencia.py:1088`. **Exceção real:** cancelamento de férias escreve `status='rejeitada'` diretamente, com trilha dentro de atomic. `app/ponto/services/ausencia.py:1496`; `app/ponto/services/ausencia.py:1499`. | `_pelo_efeito`: folha só aprovada, cobrança qualquer não rejeitada. `app/ponto/turnos.py:2523`. Supra juiz recebe `cobertura_status` e considera aprovada em `app/ponto/supra_juiz.py:268`; fechamento pede `para='folha'` em `app/ponto/services/fechamento.py:639`. |
| Ausência: hora_ini/hora_fim | DP pela mesma porta `decidir`, inclusive edição da aprovada; trilha antes/depois. `app/ponto/views.py:2822`; `app/ponto/services/ausencia.py:1099`; `app/ponto/services/ausencia.py:1108`. | Juiz de período e supra juiz distinguem sem período, fora, dentro/indeterminado. `app/ponto/turnos.py:2634`; `app/ponto/supra_juiz.py:395`; `app/ponto/supra_juiz.py:403`; `app/ponto/supra_juiz.py:413`. |
| Batida: nascimento | Porta `registrar_batida`; API de chão e materializador institucional são chamadores. `app/ponto/registro_batida.py:118`; `app/api/views_core.py:645`; `app/chamados/services/materializacao.py:1455`; `app/chamados/services/materializacao.py:1570`. | Universo apurável por `batidas_apuraveis`, apenas `retratada_em NULL`; fechamento e export consomem essa autoridade. `app/ponto/turnos.py:2868`; `app/ponto/turnos.py:2888`; `app/ponto/services/fechamento.py:632`; `app/folha/export.py:499`. |
| Batida: correção/retratação | `anotar` grava campos existentes; `retratar_batida` grava `aprovada=False` e `retratada_em`; admin chama `retratar_humano.retratar`, exigindo permissão e motivo ativo. `app/ponto/registro_batida.py:255`; `app/ponto/registro_batida.py:274`; `app/ponto/registro_batida.py:287`; `app/ponto/services/retratar_humano.py:163`. | Apuração exclui retratada; exibição de marcação conserva cru por contrato do próprio leitor. `app/ponto/turnos.py:2871`. |

Pontos materiais para a pergunta 4:

- “Descartar a batida” precisa significar **retratar**, preservando o registro: a porta declara “batida não se apaga”. `app/ponto/registro_batida.py:289`.
- A ação humana pode mudar dinheiro: a própria permissão explica que retratar “tira minuto da apuração”; retratação agenda reprocessamento do fechamento. `app/ponto/services/retratar_humano.py:134`; `app/ponto/registro_batida.py:297`; `app/ponto/services/fechamento.py:1809`.
- Ajustar ausência também muda insumo da folha e invalida projeções dos intervalos anterior e novo; não é ação financeiramente neutra. `app/ponto/services/ausencia.py:154`; `app/ponto/services/ausencia.py:669`; `app/ponto/services/ausencia.py:1123`.
- Não afirmar proteção genérica de toda exportada: evento pode recalcular exportada; a guarda concreta vigente é 09/2026 nas empresas que exportaram. `app/ponto/services/fechamento.py:1790`; `app/ponto/services/fechamento.py:36`; `app/ponto/services/fechamento.py:558`.
- Criação institucional de batida tem guarda de competência aprovada/exportada; fato de chão não é barrado. `app/ponto/registro_batida.py:147`; `app/ponto/registro_batida.py:156`; `app/ponto/janelas.py:75`. Essa guarda não é chamada pelo corpo de retratação humana inspecionado. `app/ponto/services/retratar_humano.py:150`.

Condição literal do FATO_EM_AUSENCIA é mais estreita que “ausência registrada”: cobertura **aprovada**; parcial sem período produz outro código CADASTRO; parcial fora não produz FATO_EM_AUSENCIA; há ramo com batida sem par e realizado zero. `app/ponto/supra_juiz.py:268`; `app/ponto/supra_juiz.py:395`; `app/ponto/supra_juiz.py:403`; `app/ponto/supra_juiz.py:456`.

## 17/11 10:11 · sessao c31d5ab43975 · RESSUSCITAR-DEVOLVE-MEDIDA · sonda · origem=Code

O modo `--sonda` executa o envelope antes de fornecer o dado raspado ao Codex (`bin/codex_pergunta.sh:329`, `bin/codex_pergunta.sh:334`). Segue a saída recebida, sem nova execução [nao verificado]:

```text
sonda_leitura: sonda.py [5cd5c1baa70789e6] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

UNIVERSO 2174
CLASSES {'entregue': 1116, 'aberta': 1058} erros 0
ABERTAS_COLABS_DISTINTOS 64
MAIOR_POR_PESSOA 100 col883
TOP5 [('col883', 100), ('col697', 82), ('col510', 55), ('col855', 50), ('col925', 46)]
SOMA_CONFERE True
FIM_ITEM7_OK
```

Leitura: bate numericamente com as 2174 candidatas informadas das 09:04; diferença 0. Identidade das candidatas [nao verificado]. `sem_dia=0` e `desconhecido=0` inferidos do fechamento 1116 + 1058 = 2174; contadores não explicitados na saída [nao verificado].

ITEM7: universo 2174; entregue 1116; aberta 1058; sem_dia 0; desconhecido 0; colabs abertas 64; maior por pessoa 100 [nao verificado]

## 17/11 10:11 · sessao 1f02adf6c2a6 · - · - · origem=Ronald (tmux)

**Item 3: sugiro PROCEDE (AVAL-DEVOLVIDO), pela premissa de encaminhamento atual; o corte de dono CADASTRO em si cabe na lei.** L-099 distingue ESTRUTURA/CADASTRO/BATIDA e manda apenas ESTRUTURA à construção; não fixa FATO_EM_AUSENCIA como CADASTRO. Essa escolha continuava aberta no registro. `app/docs/LEIS.md:122`; `app/docs/RELATO.md:352`; `app/docs/RELATO.md:422`.

1. **Premissas e caminho atual.** O juiz `dono_da_divergencia` não recebe ausência/FATO_EM_AUSENCIA e ainda pode devolver BATIDA por turno aberto/paridade ou ESTRUTURA pelo residual; `InsumosDoDono.dono(data)` também não monta tal insumo. Portanto o novo corte ainda precisa chegar ao juiz existente. `app/ponto/management/commands/e6_oraculo.py:53`; `app/ponto/management/commands/e6_oraculo.py:98`; `app/ponto/management/commands/e6_oraculo.py:182`.

   Há sinal A9 na lista Cadastro x Realidade: rótulo “ausencia aprovada com batida no dia”, dono literal “juiz”; sua seleção exige `CelulaDia.ausencia_id` e pelo menos duas batidas. A lista o apresenta agregado, sem `data` ou `dono_do_dia`, e a ação cai no padrão **“Com o sistema: em cura (nao e do cadastro)”**, sem URL. Isso não comprova o caminho prometido de cada dia ao admin para corrigir ausência ou retratar batida. `app/ponto/services/esmeril_espelho.py:23`; `app/ponto/services/esmeril_espelho.py:133`; `app/escala/services/cadastro_realidade.py:923`; `app/escala/services/cadastro_realidade.py:728`.

   O leitor do vigia consulta exatamente a lista Cadastro x Realidade; `dias_na_lista` exige assinatura datada e o balde `fio_do_admin` só aceita dono BATIDA/CADASTRO nessa lista. A9 não satisfaz essas condições. `app/chamados/services/vigia_fio.py:189`; `app/chamados/services/vigia_fio.py:200`; `app/chamados/services/vigia_fio.py:261`; `app/escala/services/cadastro_realidade.py:1047`.

   Existe outro caminho limitado: a tarefa expõe `VALIDAR/ADMIN` para dia **não exportado**, sem trabalho, veredito `fato_em_ausencia` e nenhuma pergunta viva C6. Isso não é atribuição do dono L-099 nem ida geral à lista Cadastro x Realidade; a composição posterior ainda exige alcançabilidade da ação administrativa. `app/chamados/services/tarefa_do_dia.py:357`; `app/chamados/services/tarefa_do_dia.py:654`.

2. **Colisão com cortes.** Dar CADASTRO a essa classe resolve escolha aberta e respeita a distinção de L-099; não permite redefinir o que é FATO_EM_AUSENCIA. Ausência parcial sem período é outra classe, nunca FATO; batida fora do período não é FATO; hora ilegível sem hora conhecida dentro mantém FATO por corte posterior. `app/docs/LEIS.md:122`; `app/docs/RELATO.md:352`; `app/docs/CORTES.md:386`; `app/docs/CORTES.md:401`; `app/ponto/supra_juiz.py:395`; `app/ponto/supra_juiz.py:403`; `app/ponto/supra_juiz.py:413`.

3. **Literal que não fecha.** “Batida em dia com ausência registrada” é mais amplo que o código: a classificação exige cobertura **aprovada**, condições do dia e tratamento do período parcial; ausência aberta pode suspender cobrança sem ser cobertura financeira aprovada. A expressão deve nomear a classe lavrada pelo juiz, não toda coexistência de registro e batida. `app/ponto/supra_juiz.py:392`; `app/ponto/supra_juiz.py:395`; `app/ponto/supra_juiz.py:450`; `app/ponto/turnos.py:2523`.

   A condição de esperar cruzamentos permanece válida como condição do anexo; a existência atual das interseções autenticadas B2+delta/F2 depende das listas não disponíveis. [nao verificado]

4. **Exportada/dinheiro/!.** Registrar dono e brief não exige corrigir dados; a correção futura por humano muda insumo de cálculo. A porta de ausência acorda projeção quando muda insumo da folha; retratar batida tira minuto da apuração e exige usuário/permissão. Não converter o corte em execução desses atos ou autorização em exportada: L-092 exige conservar gravado/Pauta DP. `app/ponto/services/ausencia.py:989`; `app/ponto/services/ausencia.py:1112`; `app/ponto/services/retratar_humano.py:131`; `app/docs/LEIS.md:125`.

**Campos ausência/status e batida, para a consolidação:**

- **Ausência/status — escritores:** `criar_ausencia`; edição de tipo/data por `editar_ausencia_registro`; status por `mudar_estado`, que valida transição e grava; `decidir` usa essa porta e grava período/carimbos no mesmo ato. Views admin chamam essas portas para aprovação, edição do período da aprovada e cancelamento. `app/ponto/services/ausencia.py:262`; `app/ponto/services/ausencia.py:614`; `app/ponto/services/ausencia.py:955`; `app/ponto/services/ausencia.py:975`; `app/ponto/services/ausencia.py:1087`; `app/ponto/services/ausencia.py:1101`; `app/ponto/views.py:2788`; `app/ponto/views.py:2811`; `app/ponto/views.py:2886`.
- **Ausência/status — leitores:** cobertura para folha filtra `aprovada`, cobrança usa não rejeitada; suprajuiz lê cobertura/período; tarefa lê status da absorção e cobertura; vigia distingue ausência aberta. `app/ponto/turnos.py:2523`; `app/ponto/supra_juiz.py:395`; `app/chamados/services/tarefa_do_dia.py:178`; `app/chamados/services/tarefa_do_dia.py:309`; `app/chamados/services/vigia_fio.py:256`.
- **Batida — retratação:** `retratar_humano.retratar` verifica usuário, permissão e motivo cadastrado, depois chama `registro_batida.retratar_batida`; esta grava `aprovada=False` e `retratada_em`, preservando o registro. `app/ponto/services/retratar_humano.py:122`; `app/ponto/services/retratar_humano.py:150`; `app/ponto/services/retratar_humano.py:179`; `app/ponto/registro_batida.py:287`.
- **Batida — leitores relevantes ao item:** `InsumosDoDono` lê `batidas_apuraveis` e turnos; esmeril conta batidas por dia para A9; suprajuiz lê realizado/órfãs e cobertura para FATO. `app/ponto/management/commands/e6_oraculo.py:150`; `app/ponto/management/commands/e6_oraculo.py:156`; `app/ponto/services/esmeril_espelho.py:120`; `app/ponto/services/esmeril_espelho.py:133`; `app/ponto/supra_juiz.py:392`.

## 17/11 10:10 · sessao 0f381d53517e · - · - · origem=Ronald (tmux)

- **Motivo declarado no código:** “SELO DE PORTA, nao de pipeline (28/09)”; antecede export com tela == PDF == fechamento == TXT e zero divergência no universo TXT. O comentário diz que roda no ato/manual antes, e que agendar mediria competência que ninguém exportará naquele dia, enchendo log de verde. Também registra que os dois contadores anteriores existiam soltos e ninguém os rodava juntos. `app/config/crons.py:887-897`.
- **RELATO confirma como a classificação nasceu:** o contrato `test_todo_command_tem_casa` exigiu casa em `CRONS` ou declaração motivada em `FORA_DE_PIPELINE`; a declaração foi “selo de PORTA, nao de pipeline”, com a mesma justificativa contra agendamento. Isso é relato da decisão de implementação, não prova de aval nominal de Ronald. `app/docs/RELATO-ARQUIVO.md:5014-5019`; aval nominal para essa classificação: **[não verificado]**.
- **Origem funcional no RELATO:** os dois contadores já existiam e não eram rodados juntos; foi criado um comando com uma pergunta/uma resposta. O registro de 28/09 publica o selo verde nos 205 do TXT, sem allowlist. `app/docs/RELATO-ARQUIVO.md:4988-5008`.
- **BACKLOG registra o resultado, não uma proibição geral de cron:** `SELO-TXT`, fechado em 28/09, exige igualdade dos quatro leitores no universo TXT e registra selo verde nos 205; “Export so depois disto, com o `!`”. `app/docs/BACKLOG.md:1203`.
- **CORTES:** não localizei entrada específica que mande manter esse selo em `FORA_DE_PIPELINE` ou proíba cron diário; tal autoria/mandato é **[não verificado]**. O arquivo se declara registro dos cortes/avais de Ronald e é gerado de CORTES.json. `app/docs/CORTES.md:1-7`. As entradas sobre “carimbo da porta cega” são outra decisão: conta vazia versus turno aberto no ato de aprovação, não a periodicidade do selo de leitores. `app/docs/CORTES.md:315`, `app/docs/CORTES.md:317`.
- **LAPIDES:** não localizei lápide específica sobre a escolha `FORA_DE_PIPELINE`; relação direta com essa decisão: **[não verificado]**. O arquivo declara conter histórias copiadas do CLAUDE, e não um catálogo completo de decisões. `app/docs/LAPIDES.md:7-21`. A lápide 43 fala de selos de host que ninguém chamava; não demonstra a razão deste selo de export. `app/docs/LAPIDES.md:501-506`.
- **Limite importante:** “roda no ATO” é a intenção registrada em crons/RELATO; se a view realmente mede/lavra durante o request deve ser respondido pelo censo de chamadas, não inferido desse comentário. `app/config/crons.py:889-896`; `app/docs/RELATO-ARQUIVO.md:5016-5019`.
- Git log/autoria histórica do sítio: **[não verificado]**, conforme indisponibilidade fornecida.

## 17/11 10:10 · sessao df5b7b1a54c1 · - · - · origem=Ronald (tmux)

(d) Texto literal de ausência do carimbo, concatenando as strings de `app/folha/porta_export.py:812-813`:

```text
A PORTA DO EXPORT NAO FOI MEDIDA HOJE para %s. O TXT nao sai sem a prova de que a tela, o PDF, o fechamento e o TXT estao no mesmo numero. Rode: %s
```

Os `%s` são `empresa.razao_social` e `_cmd`; `_cmd` é construído em `app/folha/porta_export.py:809-810`:

```text
docker exec saas_core python manage.py tenant_command selo_leitores_no_mesmo_numero --schema=juliani --mes %02d --ano %s
```

A cadeia de tela é:

- Botão literal `Gerar TXT` em `app/templates/folha/index.html:109`, dentro do form POST para `folha:gerar_txt` em `app/templates/folha/index.html:63`; rota para `gerar_dominio_txt` em `app/folha/urls.py:14`.
- A view chama `conferir(empresa, mes, ano)` e passa o retorno intacto como `msg=_barra`, com `ok=False` e redirect para `folha:exportar`: `app/folha/views.py:128-133`.
- `ok=False` vira tipo `erro`: `app/core/respostas.py:50-51`; em POST normal, registra Django message e redireciona: `app/core/respostas.py:68-71`.
- O destino renderiza `folha/exportar.html`: `app/folha/views.py:92`; o template exibe a mensagem por `{{ m }}`: `app/templates/folha/exportar.html:10`.
- Caso receba `HX-Request: true`, o helper devolve 204 e `HX-Trigger` com `showToast`, mesmo `msg`: `app/core/respostas.py:63-66`. O form de `Gerar TXT` citado usa POST HTML normal: `app/templates/folha/index.html:63`.

(e) Evidência para o pré-desenho:

- `CLAUDE.md:89`: `saas_ui = todas as telas (gthread, timeout=30: trabalho longo vai pra job)`.
- Config literal: `worker_class = "gthread"`, `threads = 4`, `timeout = 30`, `graceful_timeout = 30`: `app/gunicorn.conf.py:19-22`.
- A justificativa histórica no comentário da view é medir cinco perguntas custar dois espelhos completos por colaborador, `~100 s para os 205 do TXT`, considerado inaceitável no POST; command mede/lavra e view confere: `app/folha/views.py:124-127`. Isso não mede competência 11, tempo atual ou empresas [não verificado].
- O código não demonstra que todo request gthread com mais de 30 s morre obrigatoriamente; esse resultado literal é [não verificado]. Pode apresentar risco de request longo/ocupação de threads e conflito com a orientação operacional citada: `CLAUDE.md:89`; `app/gunicorn.conf.py:19-22`.

## 17/11 09:50 · sessao 1e4b3d0bf14d · CONTADOR-CODEX-OCIOSO · verificador · origem=Code

**A volta 2 ainda reprova:** há reparos efetivos, mas entradas de sessão continuam podendo derrubar o ESTADO, o disjuntor mantém outro relógio/borda e quatro mutantes recebem o selo após exceções convertidas em `FALHA:`. (`bin/codex_limite_estado.py:325`, `bin/codex_limite_estado.py:448`, `bin/gerar_estado.py:270`, `bin/disjuntor.py:2036`, `bin/disjuntor.py:2132`, `bin/tests/test_codex_ociosidade.sh:200`)

Conferi por leitura e simulação dos casos em memória, com arquivos e PIDs simulados. Execução integral do selo no host e ocorrência dos defeitos em produção: [nao verificado].

**1) Os achados de v1.r foram resolvidos?**

| Achado de v1.r | Situação no código atual |
|---|---|
| Segundo juiz em `ultimas_chamadas` | **Resolvido na composição principal.** `ler()` compartilha `_foto`; ela calcula `_em_curso`, e últimas chamadas lê o resultado. (`bin/codex_limite_estado.py:93`, `bin/codex_limite_estado.py:94`, `bin/codex_limite_estado.py:153`, `bin/codex_limite_estado.py:176`) |
| Garantia de carimbo quando nasce ABERTO | **Não demonstrada.** A fixture escreve a fila; produtor real e procedimento de abertura: [nao verificado]. (`bin/tests/test_codex_ociosidade.sh:39`) |
| Truncamento de 30min59s | **Resolvido para segundos inteiros:** 1859 supera 1800, com caso explícito. Ainda há truncamento subsegundo: 30min00,500s vira 1800 e não acusa. (`bin/codex_limite_estado.py:51`, `bin/codex_limite_estado.py:243`, `bin/codex_limite_estado.py:296`, `bin/tests/test_codex_ociosidade.sh:84`) |
| Janelas de 24h divergentes | **Parcial.** Limite, papéis e contexto horário usam `_na_janela`; o censo do disjuntor mantém a borda inferior inclusiva. (`bin/codex_limite_estado.py:59`, `bin/codex_limite_estado.py:83`, `bin/codex_limite_estado.py:164`, `bin/codex_limite_estado.py:283`, `bin/disjuntor.py:2132`) |
| Linhas curtas, hora inválida e tipo desconhecido ignorados | **Resolvido para essas classes na ociosidade:** são contabilizados e produzem SEM DADO. A validação dos demais campos continua incompleta. (`bin/codex_limite_estado.py:125`, `bin/codex_limite_estado.py:129`, `bin/codex_limite_estado.py:139`, `bin/codex_limite_estado.py:275`) |
| PID inválido ou erro do SO tratado como morto | **Resolvido para chamadas sem fim:** retorna `None`, que torna a foto SEM DADO. (`bin/codex_limite_estado.py:188`, `bin/codex_limite_estado.py:198`, `bin/codex_limite_estado.py:279`) |
| `chamadas.log` sem permissão derruba `ler()` | **Resolvido nesse caminho:** a guarda envolve foto, papéis e últimas chamadas. A segurança do caminho completo continua incompleta por causa das sessões. (`bin/codex_limite_estado.py:92`, `bin/codex_limite_estado.py:95`, `bin/codex_limite_estado.py:89`, `bin/codex_limite_estado.py:325`) |
| Executor certifica exceção como morte | **Parcial:** rejeita traceback e `EXCECAO:`, mas os próprios casos convertem exceções em `FALHA:`, aceitas pelo executor. (`bin/tests/test_codex_ociosidade.sh:51`, `bin/tests/test_codex_ociosidade.sh:134`, `bin/tests/test_codex_ociosidade.sh:200`, `bin/tests/test_codex_ociosidade.sh:202`) |
| M2 dependia da exceção `None > 30`; M11 dependia do motivo | **Corrigidos nos mutantes atuais:** M2 muda o requisito para mais de um ABERTO; M11 transforma erro em registro vazio, alterando o estado esperado. (`bin/tests/test_codex_ociosidade.sh:208`, `bin/tests/test_codex_ociosidade.sh:217`, `bin/tests/test_codex_ociosidade.sh:60`, `bin/tests/test_codex_ociosidade.sh:128`) |
| Diretório era caso vacuamente “ausente”; faltavam bordas | **Corrigidos:** fila diretório chega ao `open`; existem casos de 30min59s e início igual a `agora`. (`bin/codex_limite_estado.py:214`, `bin/codex_limite_estado.py:217`, `bin/tests/test_codex_ociosidade.sh:84`, `bin/tests/test_codex_ociosidade.sh:93`, `bin/tests/test_codex_ociosidade.sh:113`) |
| `rc` continha código ou situação sintética | **Separação implementada:** tupla com cinco campos e consumidor adaptado. Entretanto, o código não valida que o `rc` recebido seja numérico. (`bin/codex_limite_estado.py:139`, `bin/codex_limite_estado.py:178`, `bin/codex_limite_estado.py:179`, `bin/codex_limite_estado.py:486`) |

**2) Quem escreve, quem lê e quais significados compartilham?**

| Campo/fonte | Quem escreve | Quem lê e significado efetivo |
|---|---|---|
| `chamadas.log/ini`: hora, obra, papel, hash, PID, pergunta | Wrapper: hora sem offset, hash com 12 caracteres, PID `$$`, pergunta raspada; grava antes da espera por slot. (`bin/codex_pergunta.sh:295`, `bin/codex_pergunta.sh:308`, `bin/codex_pergunta.sh:364`) | `_registro` interpreta hora, obra, papel e chave `(obra,hash,pid)`; não conserva a pergunta. Portanto, o início inclui espera do wrapper, sem comprovar execução do Codex. (`bin/codex_limite_estado.py:124`, `bin/codex_limite_estado.py:135`, `bin/codex_limite_estado.py:137`, `bin/disjuntor.py:2073`) |
| `chamadas.log/fim`: hora, chave, papel, sessão, rc, duração | `_saida`, pelo trap EXIT após `INI=1`; duração em segundos pela diferença de epoch. (`bin/codex_pergunta.sh:265`, `bin/codex_pergunta.sh:269`, `bin/codex_pergunta.sh:272`, `bin/codex_pergunta.sh:273`, `bin/codex_pergunta.sh:282`) | Parser valida hora/tamanho, mas conserva somente sessão, rc e duração como texto. `_em_curso` usa existência da chave; últimas usa rc; procedência e tokens usam sessão. A hora do fim é descartada, sem conferência de fim posterior ao início. (`bin/codex_limite_estado.py:129`, `bin/codex_limite_estado.py:139`, `bin/codex_limite_estado.py:175`, `bin/codex_limite_estado.py:205`, `bin/codex_limite_estado.py:306`, `bin/codex_limite_estado.py:403`) |
| `fila.md`: estado e carimbo | Escrita demonstrada na fixture; escritor real: [nao verificado]. (`bin/tests/test_codex_ociosidade.sh:39`) | `ler_fila_reserva` exige estado conhecido e data+hora para ABERTO; produz quantidade, idade em segundos inteiros e erro. Ociosidade consome os três. (`bin/codex_limite_estado.py:227`, `bin/codex_limite_estado.py:235`, `bin/codex_limite_estado.py:243`, `bin/codex_limite_estado.py:251`, `bin/codex_limite_estado.py:287`) |
| `limite.log`: hora, obra, mensagem | Wrapper grava uma linha quando reconhece limite/indisponibilidade. (`bin/codex_pergunta.sh:451`, `bin/codex_pergunta.sh:452`, `bin/codex_pergunta.sh:456`) | `ler()` exige três campos e hora válida; conta linhas, sem usar obra/mensagem para validar a classificação. Publica total, janela, última hora e inválidas. (`bin/codex_limite_estado.py:73`, `bin/codex_limite_estado.py:75`, `bin/codex_limite_estado.py:81`, `bin/codex_limite_estado.py:97`) |
| Registro/foto internos | `_registro` produz existência, inícios, fins, descartadas e futuras; `_foto` acrescenta `em_curso=True/False/None`. (`bin/codex_limite_estado.py:105`, `bin/codex_limite_estado.py:139`, `bin/codex_limite_estado.py:153`) | `ler()` compartilha o registro com papéis, últimas e ociosidade; esta consome os contadores de erro e o valor trivalente. (`bin/codex_limite_estado.py:94`, `bin/codex_limite_estado.py:101`, `bin/codex_limite_estado.py:273`, `bin/codex_limite_estado.py:280`) |
| Tupla `(hora,obra,papel,rc,situacao)` | `ultimas_chamadas`: `rc=None` sem fim; situação em campo próprio. (`bin/codex_limite_estado.py:177`, `bin/codex_limite_estado.py:179`) | `_ultimas_txt` desempacota cinco campos; testes verificam situação no índice 4 e rc no 3. (`bin/codex_limite_estado.py:486`, `bin/tests/test_codex_ociosidade.sh:67`, `bin/tests/test_codex_ociosidade.sh:71`) |

Outros consumidores de chamadas são procedência no painel, tokens por papel e censo do disjuntor; `_linhas_chamadas` entrega somente inícios/fins, eliminando os contadores de entradas descartadas. (`bin/gerar_codex_estado.py:176`, `bin/codex_limite_estado.py:399`, `bin/disjuntor.py:2126`, `bin/codex_limite_estado.py:145`, `bin/codex_limite_estado.py:146`)

**Não há contrato temporal uniforme entre todos:** o módulo usa `(agora−24h,agora]`, mas o censo usa `[agora−24h,agora]`. O disjuntor também chama a contagem por papel sem transmitir seu `agora`. (`bin/codex_limite_estado.py:59`, `bin/disjuntor.py:2132`, `bin/disjuntor.py:2036`, `bin/codex_limite_estado.py:159`)

Os produtores gravam hora sem offset; o leitor usa `strptime` ingênuo e relógio local. Igualdade efetiva dos fusos dos processos: [nao verificado]. (`bin/codex_pergunta.sh:272`, `bin/codex_pergunta.sh:308`, `bin/codex_pergunta.sh:456`, `bin/codex_limite_estado.py:63`, `bin/codex_limite_estado.py:124`)

**3) Ainda existe entrada virando zero/ok ou leitura derrubando o ESTADO? Sim.**

- **`fim` malformado ainda vira conclusão válida.** Na simulação, `rc=lixo` e `dur=lixo` foram aceitos, encerraram a chave e produziram `em_curso=0`, `estado=ok` com fila sem ABERTO; últimas publicou `('lixo','terminou')`. O parser não valida esses campos. (`bin/codex_limite_estado.py:129`, `bin/codex_limite_estado.py:139`, `bin/codex_limite_estado.py:205`, `bin/codex_limite_estado.py:273`, `bin/codex_limite_estado.py:296`)
- **JSON válido com estrutura errada derruba a leitura semanal.** Por exemplo, `{"payload":[1],"rate_limits":true}` produz `AttributeError` no acesso a `.get`. O tratamento externo dessa leitura captura somente `OSError`; `gerar_estado.py` não protege a chamada. (`bin/codex_limite_estado.py:325`, `bin/codex_limite_estado.py:89`, `bin/gerar_estado.py:270`)
- **`used_percent="lixo"` derruba a composição.** O leitor conserva o valor e `_semanal_txt` chama `float`, produzindo `ValueError` fora das guardas de leitura. (`bin/codex_limite_estado.py:331`, `bin/codex_limite_estado.py:448`, `bin/codex_limite_estado.py:453`, `bin/gerar_estado.py:270`)
- **Log ausente ainda aparece como zero nas seções auxiliares:** papéis zerados e últimas vazias; a ociosidade corretamente permanece SEM DADO. (`bin/codex_limite_estado.py:115`, `bin/codex_limite_estado.py:161`, `bin/codex_limite_estado.py:174`, `bin/codex_limite_estado.py:270`)

A queda do gerador continua propagando saída 1 para `relato.sh`. (`bin/relato.sh:38`)

**4) Cada mutante morre pela propriedade?**

Na reprodução dos casos Python em memória, o original passou e os 25 mutantes divergiram. Isso não comprova a execução integral do shell nem permissões/PIDs reais: [nao verificado]. [nao verificado]

| Mutante | Divergência observada e asserção correspondente |
|---|---|
| M1 | Chamada viva passa a ACUSA. (`bin/tests/test_codex_ociosidade.sh:62`, `bin/tests/test_codex_ociosidade.sh:207`) |
| M2 | Um ABERTO antigo deixa de acusar. (`bin/tests/test_codex_ociosidade.sh:60`, `bin/tests/test_codex_ociosidade.sh:208`) |
| M3 | 30min00s e 10min passam a acusar. (`bin/tests/test_codex_ociosidade.sh:83`, `bin/tests/test_codex_ociosidade.sh:85`, `bin/tests/test_codex_ociosidade.sh:209`) |
| M4, M8 | PID morto conta como vivo ou recebe situação “em curso”. (`bin/tests/test_codex_ociosidade.sh:65`, `bin/tests/test_codex_ociosidade.sh:67`, `bin/tests/test_codex_ociosidade.sh:210`, `bin/tests/test_codex_ociosidade.sh:214`) |
| M5, M6, M7 | Sem carimbo, fila ausente ou log ausente passam a `ok`. (`bin/tests/test_codex_ociosidade.sh:75`, `bin/tests/test_codex_ociosidade.sh:79`, `bin/tests/test_codex_ociosidade.sh:80`, `bin/tests/test_codex_ociosidade.sh:211`) |
| M9, M10 | Fila malformada, diretório ou sem permissão passa a `ok`. (`bin/tests/test_codex_ociosidade.sh:113`, `bin/tests/test_codex_ociosidade.sh:123`, `bin/tests/test_codex_ociosidade.sh:126`, `bin/tests/test_codex_ociosidade.sh:215`, `bin/tests/test_codex_ociosidade.sh:216`) |
| M11, M12, M13, M14 | Log ilegível/corrompido ou PID desconhecido passa de SEM DADO para ACUSA. (`bin/tests/test_codex_ociosidade.sh:128`, `bin/tests/test_codex_ociosidade.sh:145`, `bin/tests/test_codex_ociosidade.sh:148`, `bin/tests/test_codex_ociosidade.sh:154`, `bin/tests/test_codex_ociosidade.sh:217`) |
| M15 | 30min59s deixa de acusar. (`bin/tests/test_codex_ociosidade.sh:84`, `bin/tests/test_codex_ociosidade.sh:221`) |
| M16, M18 | Contagens incluem a borda inferior ou excluem `agora`. (`bin/tests/test_codex_ociosidade.sh:97`, `bin/tests/test_codex_ociosidade.sh:99`, `bin/tests/test_codex_ociosidade.sh:222`, `bin/tests/test_codex_ociosidade.sh:224`) |
| M17 | Log futuro deixa de produzir SEM DADO. (`bin/tests/test_codex_ociosidade.sh:102`, `bin/tests/test_codex_ociosidade.sh:223`) |
| M19 | Situação sintética entra em rc. (`bin/tests/test_codex_ociosidade.sh:72`, `bin/tests/test_codex_ociosidade.sh:225`) |
| M21 | Papéis/últimas publicam zero ou ausência de registro após erro. (`bin/tests/test_codex_ociosidade.sh:137`, `bin/tests/test_codex_ociosidade.sh:139`, `bin/tests/test_codex_ociosidade.sh:227`) |
| M24 | Contador de linhas inválidas fica zero em vez de dois. (`bin/tests/test_codex_ociosidade.sh:169`, `bin/tests/test_codex_ociosidade.sh:230`) |
| **M20, M22, M23, M25** | **Morrem exclusivamente por exceção convertida em `FALHA: ... LEVANTOU`.** (`bin/tests/test_codex_ociosidade.sh:51`, `bin/tests/test_codex_ociosidade.sh:134`, `bin/tests/test_codex_ociosidade.sh:165`, `bin/tests/test_codex_ociosidade.sh:176`) |

Os quatro últimos verificam uma regressão útil de “nunca levanta”, mas **não satisfazem o critério pedido de morte sem exceção**. O executor não reconhece essa origem porque rejeita apenas traceback/`EXCECAO:` e depois aceita qualquer `FALHA:`. Nenhum dos outros mutantes depende somente de mudança no motivo. (`bin/tests/test_codex_ociosidade.sh:200`, `bin/tests/test_codex_ociosidade.sh:202`, `bin/tests/test_codex_ociosidade.sh:53`, `bin/tests/test_codex_ociosidade.sh:169`)

Há duas ressalvas adicionais:

- **M8 não prova unicidade.** Um mutante adicional que troca a leitura de `em_curso` por nova chamada a `_em_curso(c,reg['fins'])` sobreviveu aos casos em memória: reintroduz a consulta duplicada, mas conserva as respostas com PIDs estáveis. M8 atual introduz situação errada, sem medir quantidade de avaliações. (`bin/codex_limite_estado.py:153`, `bin/codex_limite_estado.py:176`, `bin/codex_limite_estado.py:207`, `bin/tests/test_codex_ociosidade.sh:214`)
- **O relaxamento `b>=1` para `b>=0` é equivalente sob os invariantes atuais:** zero ABERTO implica idade `None`, barrada por `s is not None`. Esse mutante adicional sobreviveu em memória; M2 atual usa `b>1`, uma alteração diferente. (`bin/codex_limite_estado.py:232`, `bin/codex_limite_estado.py:251`, `bin/codex_limite_estado.py:296`, `bin/tests/test_codex_ociosidade.sh:208`)

**5) Classes solicitadas**

- **Segundo juiz:** removido da composição atual; o selo ainda deixa sobreviver sua reintrodução correta. (`bin/codex_limite_estado.py:153`, `bin/codex_limite_estado.py:176`, `bin/tests/test_codex_ociosidade.sh:214`)
- **Relógio fora da borda:** permanece na chamada do disjuntor sem `agora`. (`bin/disjuntor.py:2036`, `bin/codex_limite_estado.py:159`)
- **Derivação paralela:** permanece na janela inclusiva do censo. (`bin/disjuntor.py:2132`, `bin/codex_limite_estado.py:59`)
- **Campo com dois significados:** separação estrutural corrigida; falta validar rc para impedir texto inválido de entrar nesse campo. (`bin/codex_limite_estado.py:139`, `bin/codex_limite_estado.py:179`, `bin/codex_limite_estado.py:486`)
- **Selo sem mordida:** parcialmente corrigido; persistem a certificação das quatro exceções e a falta de detecção da avaliação duplicada. (`bin/tests/test_codex_ociosidade.sh:51`, `bin/tests/test_codex_ociosidade.sh:134`, `bin/tests/test_codex_ociosidade.sh:200`, `bin/tests/test_codex_ociosidade.sh:214`)

VEREDITO: REPROVA -- entrada inválida ainda vira zero/ok, sessões podem derrubar o ESTADO e o selo aceita mortes por exceção (bin/codex_limite_estado.py:139, bin/codex_limite_estado.py:325, bin/codex_limite_estado.py:448; bin/gerar_estado.py:270; bin/tests/test_codex_ociosidade.sh:202).

## 17/11 09:48 · sessao 77c456e611f7 · - · - · origem=Ronald (tmux)

Reprodução em memória: baseline verde; todos os 25 mutantes atuais geram `FALHA`, sem traceback escapado. Execução integral do selo: [nao verificado]. [nao verificado]

- M1–M19 e M24 apresentam divergências em estado, contagem, idade ou campos; M21 detecta publicação de zero/ausência de SEM DADO. Nenhum desses depende somente do texto do motivo. (`bin/tests/test_codex_ociosidade.sh:53`, `bin/tests/test_codex_ociosidade.sh:67`, `bin/tests/test_codex_ociosidade.sh:137`, `bin/tests/test_codex_ociosidade.sh:169`)
- M20/M22/M23/M25 morrem exclusivamente porque exceções são convertidas pelo teste em `FALHA: ... LEVANTOU`. Isso verifica a propriedade “nunca levanta”, mas o executor aceita mortes por exceção, contrariando seu critério declarado. (`bin/tests/test_codex_ociosidade.sh:51`, `bin/tests/test_codex_ociosidade.sh:134`, `bin/tests/test_codex_ociosidade.sh:165`, `bin/tests/test_codex_ociosidade.sh:176`, `bin/tests/test_codex_ociosidade.sh:200`)
- A antiga vacuidade “fila é diretório” foi corrigida: `exists` permite chegar ao `open`, cujo `IsADirectoryError` produz SEM DADO. (`bin/codex_limite_estado.py:214`, `bin/codex_limite_estado.py:217`, `bin/codex_limite_estado.py:219`; `bin/tests/test_codex_ociosidade.sh:113`)
- As bordas 30min59s e início exatamente em `agora` agora têm comparações explícitas. (`bin/tests/test_codex_ociosidade.sh:84`, `bin/tests/test_codex_ociosidade.sh:93`, `bin/tests/test_codex_ociosidade.sh:97`)

Dois mutantes adicionais sobreviveram a todos os casos em memória:

1. Trocar `v = c['em_curso']` por `v = _em_curso(c, reg['fins'])`: reintroduz avaliação duplicada, mas os PIDs estáveis não revelam a consulta adicional. M8 testa rótulo errado, não unicidade. (`bin/codex_limite_estado.py:153`, `bin/codex_limite_estado.py:176`, `bin/codex_limite_estado.py:207`; `bin/tests/test_codex_ociosidade.sh:214`)
2. Trocar `b >= 1` por `b >= 0`: equivalente sob os invariantes atuais, pois `b=0` implica idade `None`, barrada por `s is not None`. M2 atual usa `b > 1`, que é diferente desse relaxamento. (`bin/codex_limite_estado.py:232`, `bin/codex_limite_estado.py:243`, `bin/codex_limite_estado.py:251`, `bin/codex_limite_estado.py:296`; `bin/tests/test_codex_ociosidade.sh:208`)

## 17/11 09:47 · sessao a37b28740f37 · - · - · origem=Ronald (tmux)

Achados confirmados no código e por simulação somente em memória:

- **O caminho completo ainda pode derrubar o ESTADO.** JSON de sessão válido com `payload: [1]`, `rate_limits: [1]` ou raiz `["rate_limits"]` produz `AttributeError`; `ler_semanal()` captura somente `ValueError` do parsing, enquanto `ler()` captura somente `OSError` dessa leitura. A chamada em `gerar_estado.py` não protege leitura nem composição. (`bin/codex_limite_estado.py:325`, `bin/codex_limite_estado.py:326`, `bin/codex_limite_estado.py:328`, `bin/codex_limite_estado.py:87`, `bin/gerar_estado.py:270`)
- **Também há queda na composição.** `used_percent: "lixo"` passa pelo leitor e produz `ValueError` em `float(...)`; não há guarda na composição da frase. (`bin/codex_limite_estado.py:331`, `bin/codex_limite_estado.py:448`, `bin/codex_limite_estado.py:453`, `bin/gerar_estado.py:270`)
- **Entrada malformada ainda pode virar zero/ok.** Uma linha `fim` com `rc=lixo` e `dur=lixo`, mas quantidade suficiente de campos, é aceita sem incrementar descartadas. Ela encerra a chamada pelo simples casamento da chave; com fila legível sem ABERTO, obtém-se `em_curso=0`, `estado=ok`, além de `rc=lixo` publicado em últimas chamadas. Reprodução em memória confirmou esses valores. (`bin/codex_limite_estado.py:129`, `bin/codex_limite_estado.py:139`, `bin/codex_limite_estado.py:205`, `bin/codex_limite_estado.py:273`, `bin/codex_limite_estado.py:296`, `bin/codex_limite_estado.py:178`)
- **Ausência do log ainda se apresenta como zeros nas seções auxiliares.** `_registro()` devolve registro vazio, `ler_chamadas()` devolve todos os papéis zero e `ultimas_chamadas()` devolve lista vazia; a ociosidade, corretamente, continua SEM DADO. (`bin/codex_limite_estado.py:115`, `bin/codex_limite_estado.py:161`, `bin/codex_limite_estado.py:174`, `bin/codex_limite_estado.py:270`)

Reparos de v1 efetivos:

- O segundo juiz de últimas chamadas foi removido no caminho `ler()`: a mesma foto alimenta papéis, últimas e ociosidade; `_em_curso` é carimbado e seu valor é consumido por últimas. (`bin/codex_limite_estado.py:93`, `bin/codex_limite_estado.py:153`, `bin/codex_limite_estado.py:176`)
- A janela do módulo passou a uma função única `(agora−24h, agora]`, usada por limite, papéis e contexto por hora. (`bin/codex_limite_estado.py:59`, `bin/codex_limite_estado.py:83`, `bin/codex_limite_estado.py:164`, `bin/codex_limite_estado.py:283`)
- `30min59s` agora acusa; a idade está em segundos, comparada a 1800. (`bin/codex_limite_estado.py:243`, `bin/codex_limite_estado.py:296`)
- `rc` e `situacao` agora ocupam posições distintas e o consumidor foi adaptado, embora `rc` de entrada não seja validado. (`bin/codex_limite_estado.py:178`, `bin/codex_limite_estado.py:179`, `bin/codex_limite_estado.py:486`, `bin/codex_limite_estado.py:139`)

Classes ainda presentes:

- **Relógio fora da borda:** o disjuntor chama `ler_chamadas(raiz)` sem transmitir seu `agora`; a função captura outro relógio. (`bin/disjuntor.py:2036`, `bin/codex_limite_estado.py:159`)
- **Derivação paralela:** o censo mantém janela inclusiva nas duas bordas, diferente da função única do módulo, e usa `_linhas_chamadas()`, que descarta os contadores de linhas inválidas/futuras do registro. (`bin/disjuntor.py:2132`, `bin/codex_limite_estado.py:59`, `bin/codex_limite_estado.py:145`, `bin/codex_limite_estado.py:146`)

Ocorrência desses casos em produção: [nao verificado].

## 17/11 09:46 · sessao 0a302ad15775 · - · - · origem=Ronald (tmux)

- **`chamadas.log/ini`**: `bin/codex_pergunta.sh:308` grava hora local sem offset, obra, papel, hash de 12 caracteres, PID `$$`, tipo `ini` e pergunta raspada; a pergunta é reduzida a uma linha em `bin/codex_pergunta.sh:295`. O registro precede a espera por slot em `bin/codex_pergunta.sh:364`; portanto, conta tentativa/espera do wrapper, sem provar execução do Codex (`bin/disjuntor.py:2073`). `_registro` lê hora/obra/papel/hash/PID/tipo, ignora conteúdo da pergunta e usa `(obra,hash,pid)` como chave (`bin/codex_limite_estado.py:124`, `bin/codex_limite_estado.py:128`, `bin/codex_limite_estado.py:135`, `bin/codex_limite_estado.py:137`).

- **`chamadas.log/fim`**: `_saida`, via trap EXIT, grava hora, mesma chave, papel, sessão ou `-`, código de saída e duração em segundos de epoch (`bin/codex_pergunta.sh:265`, `bin/codex_pergunta.sh:269`, `bin/codex_pergunta.sh:272`, `bin/codex_pergunta.sh:282`). O parser valida timestamp e quantidade de campos, mas **descarta a hora do fim** e guarda sessão/rc/duração como strings sem validação de domínio (`bin/codex_limite_estado.py:124`, `bin/codex_limite_estado.py:129`, `bin/codex_limite_estado.py:139`). `_em_curso` considera qualquer `fim` da chave suficiente para encerrar, sem testar se ocorreu depois do `ini` (`bin/codex_limite_estado.py:205`).

- **Consumidores de `ini/fim`**: contagem por papel (`bin/codex_limite_estado.py:157`), últimas chamadas (`bin/codex_limite_estado.py:169`), ociosidade (`bin/codex_limite_estado.py:273`), origem de sessões (`bin/codex_limite_estado.py:304`, `bin/gerar_codex_estado.py:176`), tokens por papel (`bin/codex_limite_estado.py:399`) e censo do disjuntor (`bin/disjuntor.py:2126`). Não encontrei consumidor da duração além de seu armazenamento; consumidor externo: [nao verificado].

- **`fila.md`**: escritor demonstrado somente na fixture, que grava o argumento `fila` (`bin/tests/test_codex_ociosidade.sh:39`). Produtor real, responsável pela transição para ABERTO e garantia do carimbo no nascimento: [nao verificado]. O leitor exige prefixo de estado conhecido e carimbo `AAAA-MM-DD HH:MM` para ABERTO, calcula idade em segundos inteiros e devolve quantidade/idade/erro (`bin/codex_limite_estado.py:53`, `bin/codex_limite_estado.py:54`, `bin/codex_limite_estado.py:227`, `bin/codex_limite_estado.py:235`, `bin/codex_limite_estado.py:243`, `bin/codex_limite_estado.py:251`). Ociosidade consome esses resultados (`bin/codex_limite_estado.py:287`).

- **`limite.log`**: wrapper escreve hora, obra e primeira mensagem de erro reconhecida como limite/indisponibilidade, com tabs/newlines removidos da mensagem (`bin/codex_pergunta.sh:451`, `bin/codex_pergunta.sh:452`, `bin/codex_pergunta.sh:454`, `bin/codex_pergunta.sh:456`). `ler()` lê os três campos, valida somente hora/quantidade mínima, conta linhas e publica total/janela/última hora; não usa obra nem mensagem para classificar o evento (`bin/codex_limite_estado.py:73`, `bin/codex_limite_estado.py:75`, `bin/codex_limite_estado.py:81`, `bin/codex_limite_estado.py:97`). A fonte de escrita pode ser alterada por `CODEX_LOGD`, enquanto o leitor usa `<raiz>/logs/codex` (`bin/codex_pergunta.sh:101`, `bin/codex_limite_estado.py:45`, `bin/codex_limite_estado.py:64`); configuração real alinhada: [nao verificado].

- **Tupla `ultimas_chamadas`**: escritor agora produz `(hora,obra,papel,rc,situacao)`, com `rc=None` sem fim e situação separada (`bin/codex_limite_estado.py:175`, `bin/codex_limite_estado.py:177`, `bin/codex_limite_estado.py:179`). `ler()` fornece a mesma `_foto` para as últimas e a contagem, eliminando a duplicação do predicado nessa composição (`bin/codex_limite_estado.py:93`, `bin/codex_limite_estado.py:94`, `bin/codex_limite_estado.py:101`, `bin/codex_limite_estado.py:153`). O renderizador desempacota cinco campos corretamente (`bin/codex_limite_estado.py:486`); testes consultam a situação no índice 4 e rc no índice 3 (`bin/tests/test_codex_ociosidade.sh:67`, `bin/tests/test_codex_ociosidade.sh:71`). Entretanto, rc inválido do log continua sendo aceito e publicado, inclusive texto sintético se vier na entrada (`bin/codex_limite_estado.py:139`, `bin/codex_limite_estado.py:178`, `bin/codex_limite_estado.py:486`).

- **Bordas ainda diferentes entre consumidores**: módulo usa `(agora−24h,agora]` para limite, papéis e contexto horário (`bin/codex_limite_estado.py:59`, `bin/codex_limite_estado.py:83`, `bin/codex_limite_estado.py:164`, `bin/codex_limite_estado.py:283`), mas censo do disjuntor mantém ambas as bordas inclusivas (`bin/disjuntor.py:2132`). Além disso, disjuntor chama `ler_chamadas(raiz)` sem passar seu relógio (`bin/disjuntor.py:2036`), fazendo o leitor capturar outro `now` (`bin/codex_limite_estado.py:159`). Portanto, as classes **derivação paralela** e **relógio fora da borda** permanecem demonstradas nesses consumidores (`bin/disjuntor.py:2036`, `bin/disjuntor.py:2132`).

- **Fuso e precisão**: gravações de hora usam `date` sem offset, leitores usam `strptime` ingênuo e `datetime.now()` local (`bin/codex_pergunta.sh:272`, `bin/codex_pergunta.sh:308`, `bin/codex_pergunta.sh:456`, `bin/codex_limite_estado.py:63`, `bin/codex_limite_estado.py:124`, `bin/codex_limite_estado.py:235`). Igualdade efetiva entre os ambientes produtores/leitores: [nao verificado]. A correção resolve 30min59s, mas ainda trunca frações de segundo: `30min00.500s` vira 1800 e não acusa, embora a comparação final seja `s > 1800` (`bin/codex_limite_estado.py:51`, `bin/codex_limite_estado.py:243`, `bin/codex_limite_estado.py:296`).

## 17/11 09:32 · sessao daeb5df7e465 · E4C-F2 · verificador · origem=Code

1. **[nota] O mutador preserva exatamente a âncora atual.** A conferência AST em memória mostrou apenas a retirada de `q_nao_validada_pelo_ato()`: permanecem o OR da âncora, `pk__in`, relacionamentos e ordenação. Não há atributo com `'validad'` nos argumentos atuais da âncora; ali aparece `self.pk` (`app/chamados/models.py:355–359`; `app/chamados/tests/test_cortes_f2_f9_m13.py:28–43`).

   Para a forma antiga documentada, `validada_em__isnull=True`, a regra também remove somente esse keyword (`app/chamados/tests/test_cortes_f2_f9_m13.py:5`, `:39`). Aplicação ao código exato de **708162b15**: **[nao verificado]**; a sondagem dessa forma sintética não comprova a revisão histórica.

2. **[fila] AncoradasCustoTest distingue validação só no ATO, mas deixa sobreviver uma dependência indevida da projeção.** Agora exige excluir `so_no_ato`, mesmo com `validada_em=None` (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:487–494`). Contudo, este mutante no filtro externo ainda satisfaz custo e M13, por leitura das fixtures (`app/chamados/models.py:357`; `app/chamados/tests/test_cortes_f2_f9_m13.py:49–64`):

   ```python
   PerguntaDisputa.objects.filter(
       PerguntaDisputa.q_nao_validada_pelo_ato() & Q(validada_em__isnull=True),
       pk__in=_ancora,
   )
   ```

   Falta a divergência inversa: carimbo residual, VALIDACAO em T1 e RECUSA em T2; a pergunta deveria entrar, mas esse mutante a excluiria. O caso existe contra leitura/face, sem chamar `_ancoradas` (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:51–55`, `:105–109`). O seletor M13 ainda retiraria um único argumento composto e produziria a mordida esperada por PK (`app/chamados/tests/test_cortes_f2_f9_m13.py:33–43`, `:54–56`). Execução desse mutante: **[nao verificado]**.

3. **[fila] Há também um mutante sobrevivente da âncora:** reduzir o OR a `Q(chamado_destino_id=self.pk)`. Todas as perguntas desses dois testes usam `destino=ch`, deixando sem cobertura o ramo “sem destino, pelo chamado da disputa” (`app/chamados/models.py:355–356`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:484–488`; `app/chamados/tests/test_cortes_f2_f9_m13.py:49–62`). Esse ramo é exigido pelo juiz da âncora (`app/chamados/services/fio.py:100–102`, `:117`). Sobrevivência deduzida por leitura; execução **[nao verificado]**.

4. **[nota] No delta fornecido, não muda lógica produtiva além da docstring, nem se deslocam linhas de produção.** A substituição ocupa a mesma linha (`app/chamados/models.py:346`). O c12 inclui linhas na identidade, mas exclui arquivos de testes, onde ocorreram os acréscimos (`app/chamados/tests/e4b_c12_ast.py:17–27`, `:32–39`, `:222–223`). Executei, sem banco e sem escrita de bytecode, os dois testes do censo e corpo nominal: **2 passaram** (`app/chamados/tests/test_e4b_c12_selo.py:22`, `:34`). Suíte completa e efeito em produção: **[nao verificado]**.

## 17/11 09:31 · sessao db4eb04e80f0 · - · - · origem=Ronald (tmux)

[ fila ] Confirmado por leitura: o mutante abaixo em `app/chamados/models.py:357` passa nos dois testes normais:

```python
PerguntaDisputa.objects.filter(
    PerguntaDisputa.q_nao_validada_pelo_ato() & Q(validada_em__isnull=True),
    pk__in=_ancora,
)
```

As pendentes têm projeção nula; as validadas já saem pela face. Referências: `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:485-494`; `app/chamados/tests/test_cortes_f2_f9_m13.py:50-55,60-64`.

Com o seletor M13, `_incluir_validadas` remove o argumento posicional composto inteiro ao encontrar `q_nao_validada_pelo_ato`; conta uma retirada e conserva `pk__in`. Assim, continua produzindo a mordida esperada por PK. Referências: `app/chamados/tests/test_cortes_f2_f9_m13.py:33-43,54-56`.

O caso ausente é a **divergência inversa**: projeção `validada_em=T1`, VALIDACAO em T1 e RECUSA em T2. A pergunta deve entrar em `_ancoradas`, mas o mutante a exclui pelo carimbo residual. Esse caso existe somente contra leitura e face: `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:105-109,51-55`.

Adversarial mínimo: repetir esse caso com `destino=ch` e exigir seu PK em `ch._ancoradas()`. Execução [nao verificado].

## 17/11 09:30 · sessao db4eb04e80f0 · - · - · origem=Ronald (tmux)

1. [nota] **Sim, o teste agora distingue validação só no ato.** `so_no_ato` nasce com projeção vazia, recebe RESPOSTA e VALIDACAO, continua com `validada_em=None` e deve sair de `_ancoradas` junto com a validada alinhada. Referências: `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:487-494`; `app/chamados/tests/cortes_f2_f9_apoio.py:66-74`.

2. [fila] **A âncora ainda tem mutante sobrevivente nos dois testes.** Em `app/chamados/models.py:355-356`, substituir o filtro por:

   ```python
   _ancora = PerguntaDisputa.objects.filter(
       Q(chamado_destino_id=self.pk)).values('pk')
   ```

   preserva a subquery, o `pk__in`, a face e uma query (`app/chamados/models.py:357-359`). Todas as perguntas desses testes têm `destino=ch`, logo o mutante conserva seus resultados esperados: `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:484-494`; `app/chamados/tests/test_cortes_f2_f9_m13.py:49-55,59-64`. Entretanto, perde perguntas sem destino ancoradas pelo chamado da disputa, contrariando `app/chamados/services/fio.py:100-102,116-117`. Sobrevivência deduzida por leitura; execução [nao verificado].

   Para fechar essa lacuna, incluir pergunta sem destino com `disputa.chamado=ch`, além de pergunta destinada a outro chamado. A fixture atual cria a disputa sem preencher `chamado`: `app/chamados/tests/cortes_f2_f9_apoio.py:58-64`.

## 17/11 09:30 · sessao 3b3e67c2f1be · - · - · origem=Ronald (tmux)

3. **[nota] No delta fornecido, a única mudança produtiva é o texto da docstring**, na mesma linha; o corpo de `_ancoradas` permanece abaixo dela, sem deslocamento (`app/chamados/models.py:343-359`, especialmente `:346`). Comportamento produtivo adicional ao delta fornecido: [nao verificado].

4. **[nota] O selo c12 por linha não foi afetado por este delta.** Ele inclui o número da linha na identidade do `CORPO` (`app/chamados/tests/e4b_c12_ast.py:17-27,222-223`); conferência AST somente em memória encontrou todas as nove chamadas nas linhas cadastradas: 205, 302, 462, 667, 847, 865, 1501, 1705 e 1725 (`app/chamados/tests/e4b_c12_ast.py:19-27`). Os acréscimos nos dois testes não entram no universo produtivo: a pasta `tests` é excluída (`app/chamados/tests/e4b_c12_ast.py:32-39`). O mecanismo dos sítios também resolve a linha atual pelo AST, usando `linha_base` apenas para a fonte-base (`app/chamados/tests/e4b_c12_verificar.py:84-92,106-115`); o sítio E01 continua sendo a chamada de `fechar` (`app/chamados/tests/e4b_c12_sitios.json:370-376`; `app/chamados/models.py:464-467`).

Não executei a suíte c12; resultado integral de execução: [nao verificado]. [nao verificado]

## 17/11 09:27 · sessao 4ee5805a2bd9 · EXPORT-UM-JUIZ-DO-CODIGO · censo · origem=Code

**A divergência confere:** o export aceita qualquer código não vazio após `strip()`, enquanto o layout exige matrícula numérica em dez posições; qualquer erro de layout interrompe o TXT inteiro. `app/folha/export.py:840`, `app/folha/export.py:872`, `app/folha/export.py:273`, `app/folha/export.py:277`, `app/folha/views.py:275`.

**1. Escritores**

Contagem por sítio funcional que persiste o campo: **8 = duas atribuições explícitas + quatro regravações integrais + duas inicializações**. As portas comuns estão discriminadas abaixo, sem contá-las novamente.

| # | Sítio | O que grava |
|---|---|---|
| 1 | Admissão | `codigo_folha.strip()`, somente se não vazio; atribui e salva. `app/colaboradores/views.py:1131`, `app/colaboradores/views.py:1232`, `app/colaboradores/views.py:1237` |
| 2 | Edição | Mesmo tratamento; enviar vazio **preserva o código anterior**. `app/colaboradores/views.py:1353`, `app/colaboradores/views.py:1355`, `app/colaboradores/views.py:1365` |
| 3 | Vincular posto | `save()` integral regrava também o código carregado. `app/colaboradores/views.py:345`, `app/colaboradores/services/colaborador.py:160` |
| 4 | Transferir posto | Mesma regravação integral. `app/colaboradores/views.py:1418`, `app/colaboradores/services/colaborador.py:160` |
| 5 | Desligamento | Mesma regravação integral. `app/colaboradores/services/desligamento.py:133`, `app/colaboradores/services/colaborador.py:160` |
| 6 | Reativação | Regrava o objeto inteiro pela porta própria. `app/colaboradores/services/desligamento.py:247`, `app/colaboradores/services/colaborador.py:200` |
| 7 | Comando `smoke_chamados` | Cria colaborador de fixture sem código explícito, recebendo `default=''`. `app/chamados/management/commands/smoke_chamados.py:78`, `app/colaboradores/models.py:312` |
| 8 | Migration `0026` | Adiciona o campo com `default=''`; é `AddField`, não migration de dados com `RunPython`. `app/colaboradores/migrations/0026_colaborador_codigo_dominio.py:13` |

As persistências comuns são `salvar_campos`, `salvar_inteiro`, `salvar_reativacao`, `criar_colaborador` e, finalmente, `Colaborador.save()`. Não validam a forma do código. `app/colaboradores/services/colaborador.py:140`, `app/colaboradores/services/colaborador.py:152`, `app/colaboradores/services/colaborador.py:184`, `app/colaboradores/services/colaborador.py:204`, `app/colaboradores/models.py:415`.

As portas genéricas `gravar` e `atualizar_em_lote` também permitem esse campo na operação `cadastro`; a existência de outro chamador dinâmico que lhes entregue o código é **[nao verificado]**. `app/colaboradores/services/colaborador.py:46`, `app/colaboradores/services/colaborador.py:126`, `app/colaboradores/services/colaborador.py:164`.

O Django admin **não é escritor**: `ColaboradorAdmin` herda `SoLeitura`, que nega inclusão, alteração e exclusão. Os aliases usados para reexibir formulários recusados alteram apenas memória. `app/colaboradores/admin.py:48`, `app/core/admin.py:59`, `app/core/admin.py:62`, `app/core/admin.py:65`, `app/colaboradores/views.py:1117`, `app/colaboradores/views.py:1282`, `app/core/reexibir.py:21`.

Importador especializado do código ou outra migration de dados que o preencha: **[nao verificado]**.

**2. Leitores e ESMERIL-SEMÂNTICA**

Contagem: **20 sítios funcionais**, incluindo leitura do valor, projeção, matrícula serializada e admin; consumidores apenas de status/motivo ficam separados.

| # | Leitor | Suposição |
|---|---|---|
| 1 | `classificar_export` | Vazio após `strip()` fica fora; texto não vazio supera esse crivo. `app/folha/export.py:840`, `app/folha/export.py:872` |
| 2 | `montar_linhas_export` | Aplica `strip().zfill(10)`; não exige número nem trunca. `app/folha/export.py:728` |
| 3 | Pendências cadastrais | `None`, vazio e só espaços significam ausente. `app/colaboradores/services/cadastro.py:50`, `app/colaboradores/services/cadastro.py:82` |
| 4 | API `_cadastro_incompleto` | Somente `NULL` ou `''` significam ausente; espaços passam. `app/api/views_mensageria.py:1281` |
| 5 | Tela de edição | Exibe texto; vazio aparece vazio; JS exige preenchimento. `app/templates/colaboradores/editar.html:62`, `app/templates/colaboradores/editar.html:298` |
| 6 | Diferença entre exportações | Chave global `strip().zfill(10)`, sem recorte de empresa; colisão sobrescreve. `app/folha/haiku_export.py:115`, `app/folha/haiku_export.py:117` |
| 7 | Vigia do TXT | Ignora vazio após `strip()`; usa `zfill(10)` para associar motivo. `app/folha/services/vigia_txt.py:114` |
| 8 | Tradutor do PDF | Exige matrícula numérica no registro de 43 caracteres; resolve nome por empresa e código preenchido com zeros. `app/folha/pdf_export.py:112`, `app/folha/pdf_export.py:147` |
| 9 | PDF de validação | Código preenchido com zeros vira chave para ID; colisão sobrescreve. `app/folha/validacao_pdf.py:72` |
| 10 | Montagem da prévia | Copia código do juiz; vazio vira `''`. `app/folha/previa.py:100` |
| 11 | PDF de retidos | Exibição textual; vazio vira `'-'`. `app/folha/retidos_pdf.py:93` |
| 12 | Preview do export | Exibição textual escapada; vazio vira `'-'`. `app/folha/views.py:401` |
| 13 | Prévia de propostas | Código cru no CSV; escapado no HTML; sem validação própria. `app/folha/views.py:736`, `app/folha/views.py:742`, `app/folha/views.py:787`, `app/folha/views.py:811` |
| 14 | Retrato da folha | Copia para a chave `codigo`; vazio vira `''`. `app/folha/retrato_folha.py:1239` |
| 15 | Match configurável de holerite | Igualdade exata, com empresa quando há CNPJ; exige exatamente um candidato. `app/templates/holerite/upload_lote_v2.html:46`, `app/holerite/motor.py:218`, `app/holerite/motor.py:225`, `app/holerite/motor.py:298` |
| 16 | `montar_linha_10` | Converte para texto e aplica `zfill(10)`; não valida. `app/folha/export.py:36` |
| 17 | `partes_da_linha` | Reconhece matrícula numérica em dez posições de registro com 43 caracteres. `app/folha/export.py:247` |
| 18 | `validar_layout_dominio` | Exige comprimento 43 e matrícula `.isdigit()`. `app/folha/export.py:273`, `app/folha/export.py:277` |
| 19 | Agregador do export | Exige registro de 43 caracteres; usa a fatia da matrícula como chave sem exigir que seja numérica. `app/folha/export.py:313`, `app/folha/export.py:319` |
| 20 | Django admin | Exibe o campo como somente leitura, derivado dos metadados do modelo. `app/colaboradores/admin.py:48`, `app/core/admin.py:50`, `app/core/admin.py:68` |

O raio-x da folha, os relatórios de pendências e “quem cobrar”, e o tique do fechamento **herdam o julgamento**, sem verificar novamente o formato do código. `app/folha/views.py:589`, `app/relatorios/views_pend.py:49`, `app/relatorios/views_pend.py:58`, `app/relatorios/views.py:1660`, `app/ponto/services/fechamento_lista.py:29`.

Os selos de cartão/TXT e PDF/espelho selecionam quem o juiz declarou `entra`; o selo integrado delega à porta, cujo universo também usa esse status. `app/relatorios/management/commands/cartao_x_txt_divergentes.py:53`, `app/relatorios/management/commands/pdf_x_espelho_divergentes.py:58`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:33`, `app/folha/porta_export.py:469`.

**ESMERIL-SEMÂNTICA não fecha:** há texto de até vinte caracteres no modelo, presença de texto no export, matrícula numérica de dez posições no layout e conceitos diferentes de vazio na API e no cadastro. `app/colaboradores/models.py:311`, `app/folha/export.py:872`, `app/folha/export.py:277`, `app/api/views_mensageria.py:1281`, `app/colaboradores/services/cadastro.py:83`.

**3. Validação no cadastro e no modelo**

Os dois inputs são `type="text"`, sem `pattern` nem `maxlength`; os scripts os tornam obrigatórios. Não exigem número ou dez dígitos. `app/templates/colaboradores/admitir.html:58`, `app/templates/colaboradores/admitir.html:204`, `app/templates/colaboradores/editar.html:62`, `app/templates/colaboradores/editar.html:298`.

No servidor, obrigatórios são nome/CPF, com dispensa do CPF na edição; código ausente vira pendência. A lista antiga que contém `codigo_folha` deixou de ser o juiz da gravação. `app/colaboradores/services/cadastro.py:22`, `app/colaboradores/services/cadastro.py:53`, `app/colaboradores/services/cadastro.py:29`, `app/colaboradores/views.py:3225`.

A guarda genérica de tamanho **não cobre o alias normal da tela**: admissão omite o código do dicionário examinado; edição entrega `codigo_folha`, enquanto o helper procura o nome exato `codigo_dominio`. `app/colaboradores/views.py:1180`, `app/colaboradores/views.py:1298`, `app/core/validacao.py:49`.

O modelo declara `CharField(max_length=20, blank=True, default='')`, sem validator específico ou unicidade. Seu `save()` não valida o código. `app/colaboradores/models.py:311`, `app/colaboradores/models.py:400`, `app/colaboradores/models.py:415`.

**4. Exigência do layout e predicado atual**

`montar_linha_10` preenche a matrícula à esquerda até dez posições; código maior permanece maior. O validador exige registro de 43 caracteres, constante `11` e campos numéricos, incluindo matrícula nas posições `[2:12]`. `app/folha/export.py:36`, `app/folha/export.py:45`, `app/folha/export.py:273`, `app/folha/export.py:275`, `app/folha/export.py:277`.

O predicado exato de `sem_codigo_dominio` é:

```python
item['codigo_dominio'] = (colab.codigo_dominio or '').strip()
if not item['codigo_dominio']:
    # status='fora', motivo='sem_codigo_dominio'
```

Fontes: `app/folha/export.py:840`, `app/folha/export.py:872`. Esse ramo vem depois de rescisão; não pergunta número, tamanho ou duplicidade. `app/folha/export.py:866`, `app/folha/export.py:872`.

**Correção da premissa do anexo:** o layout não verifica repetição nem vazio original. Vazio montado diretamente vira dez zeros; repetição não é comparada entre colaboradores. `app/folha/export.py:36`, `app/folha/export.py:266`. Que ambos devam ser recusados é **[nao verificado]**.

**5. Juízes registrados e predicados vivos**

**Nenhum juiz específico registrado responde “código do Domínio válido?”** O registro aponta `classificar_export` para aprovação/entrada no TXT e para a leitura de tela; o documento aponta o mesmo juiz de entrada. `app/core/juizes.py:329`, `app/core/juizes.py:330`, `app/core/juizes.py:743`, `app/docs/JUIZES.md:118`.

Há **quatro implementações vivas de juízo sobre entrada/forma do código no TXT**, contando sítios, não expressões distintas:

- Presença após `strip()`: `app/folha/export.py:840`, `app/folha/export.py:872`.
- Matrícula numérica/largura no layout: `app/folha/export.py:273`, `app/folha/export.py:277`.
- Matrícula numérica/largura no parser comum: `app/folha/export.py:247`.
- Matrícula numérica/largura no parser do PDF: `app/folha/pdf_export.py:112`.

As verificações de completude cadastral, SQL e `required` do navegador são adicionais, discriminadas acima, fora dessa contagem de juízos de entrada/forma do TXT. `app/colaboradores/services/cadastro.py:83`, `app/api/views_mensageria.py:1281`, `app/templates/colaboradores/admitir.html:204`, `app/templates/colaboradores/editar.html:298`.

**6. Perguntas de lei**

**lei 0 para o PREPARA solicitado:** coerência e consumidor lendo a autoridade já têm lei escrita; juiz único já consta como padrão estrutural. `app/docs/LEIS.md:122`, `app/docs/LEIS.md:95`, `app/docs/CORTES.md:378`.

Política nova para duplicidade, novo motivo de retenção ou nascimento de nova autoridade não ficam decididos por este censo; sua autorização específica é **[nao verificado]**. Regra nova não pode ser deduzida apenas do código existente, e autoridade nova exige frase de nascimento no CORTES. `app/docs/LEIS.md:136`, `app/docs/LEIS.md:69`.

CENSO: escritores 8, leitores 20, predicados vivos 4, lei 0

_6456 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
