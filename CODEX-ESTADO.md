# CODEX-ESTADO

> Ultima resposta processada: **09/11/2026 21:15** · publicado em **09/11/2026 21:19** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 09/11 21:15 · sessao d4c533f18057 · MODO-COMUM-PASSADO-SEM-TURNO · verificador · origem=Code

**Há PROCEDENTES: emissão sem previsão, entrada alternativa de pauta, proteção insuficiente da ordem, teste incompleto do registro e explicação incorreta do fechamento.** As evidências e curas mínimas estão abaixo. Correspondência independente da cópia com `49015265a`, ocorrências reais e resultado da suíte no container: **[nao verificado]**.

1. **QUEM ESCREVE / QUEM LÊ — CONFORME nos campos; PROCEDENTE na explicação de “entregue”.**

| Campo/valor | Quem escreve | Quem lê e significado |
|---|---|---|
| Nova chave/valor de `JUIZES['chamado']` | Declaração em `app/core/juizes.py:293`. | Gerador do MAPA: `bin/gerar_mapa_juizes.py:40`; teste novo: `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:197`. |
| `PAUTA_SEM_TURNO` / prefixo do texto | Constante em `app/chamados/services/fabrica_da_celula.py:276`; escritor específico incorpora ao texto em `app/chamados/services/fabrica_da_celula.py:342`. | `_pautas_cadastro` interpreta o prefixo; juiz e contador usam esse seletor. `app/chamados/services/fabrica_da_celula.py:285`, `app/chamados/services/fabrica_da_celula.py:297`, `app/chamados/services/fabrica_da_celula.py:315`. |
| `Pauta.texto`, âncora e destinatário | A fábrica fornece DP, `ANCORA_COLAB` e `str(cel.colaborador_id)`; a porta comum persiste. É **pessoa**, sem vínculo nem dia na chave. `app/chamados/services/fabrica_da_celula.py:339`, `app/chamados/services/fabrica_da_celula.py:340`; `app/pautas/models.py:30`; `app/pautas/services.py:180`. | O seletor exige cabeça, DP, âncora colab e prefixo; o juiz restringe pela pessoa. `app/chamados/services/fabrica_da_celula.py:284`, `app/chamados/services/fabrica_da_celula.py:297`. |
| `feito_em` | `marcar_feita` e `marcar_feita_pelo_sistema`, com instante `timezone.now()`. `app/pautas/services.py:246`, `app/pautas/services.py:247`, `app/pautas/services.py:271`, `app/pautas/services.py:273`. | `vivas()` exclui feitas; o juiz histórico converte o instante para data local. `app/pautas/models.py:57`; `app/chamados/services/fabrica_da_celula.py:301`, `app/chamados/services/fabrica_da_celula.py:304`. |
| `cancelado_em` | `cancelar`, ato do autor, grava instante. `app/pautas/services.py:288`, `app/pautas/services.py:294`. | `vivas()` exclui canceladas; o juiz aceita cancelamento como fechamento e o prioriza sobre `feito_em`. `app/pautas/models.py:64`; `app/chamados/services/fabrica_da_celula.py:303`. |
| `VFM\|CADASTRO_SEM_TURNO` e carimbo JSON | Vigia lê o contador, imprime número/esperado/dono e grava número e esperado no JSON. `app/chamados/management/commands/vigiar_fio_morto.py:179`, `app/chamados/management/commands/vigiar_fio_morto.py:202`, `app/chamados/management/commands/vigiar_fio_morto.py:223`, `app/chamados/management/commands/vigiar_fio_morto.py:229`. | `bin/fio_estado.py:104` lê o JSON e `bin/fio_estado.py:125` compara censo com esperado; `bin/gerar_estado.py:260` incorpora a apresentação. |

Os leitores genéricos transportam esses campos sem decidir “entregue CADASTRO”: aba de pautas, API de mensageria e listagem da pessoa. A listagem da pessoa inclui histórico; a aba padrão usa vivas. São universos declaradamente diferentes, sem segundo significado para a âncora. `app/pautas/services.py:323`, `app/pautas/services.py:433`; `app/pautas/views.py:40`; `app/api/views_mensageria.py:1329`, `app/api/views_mensageria.py:1334`.

**PROCEDENTE — “o DP já olhou” não decorre do predicado.** O juiz aceita cancelamento pelo autor e não consulta `lido_em`; a docstring afirma que o DP olhou. Cura mínima: descrever “já encaminhada e posteriormente fechada/cancelada”, sem prometer leitura ou resolução pelo DP. Alterar a cobertura do cancelamento exigiria decisão normativa específica. `app/chamados/services/fabrica_da_celula.py:292`, `app/chamados/services/fabrica_da_celula.py:301`; `app/pautas/services.py:288`, `app/pautas/services.py:294`.

**CONFORME — instante versus data:** o armazenamento é de instantes; a cobertura usa `localdate(fechou) >= dia`, sendo `dia=cel.data`. A configuração base é São Paulo; fuso ativo efetivo em produção: **[nao verificado]**. `app/chamados/services/fabrica_da_celula.py:304`, `app/chamados/services/fabrica_da_celula.py:337`; `app/config/settings/base.py:93`, `app/config/settings/base.py:95`.

2. **O selo morde — CONFORME para usos diretos; PROCEDENTE na ordem e no registro.**

| Regra | O que pega | O que escapa |
|---|---|---|
| **a. Marcador/literal fora dos dois sítios** | Nome, import com alias, atributo direto e literal completo. Importar o módulo inteiro e acessar diretamente o atributo **é pego**; outra constante com o mesmo literal completo também. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:111`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:115`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:120`. | `getattr`, texto composto por soma, regex sem o literal completo e SQL parametrizado com texto composto não são resolvidos pelo varredor. Uso atual dessas técnicas para esse marcador: **[nao verificado]**. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:120`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:124`. |
| **b. Seletor fora do juiz/contador** | Chamadas diretas e aliases de `from … import`. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:61`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:146`. | Referência intermediária, como `f = modulo._pautas_cadastro; f()`, e reflexão. Uso atual com essa finalidade: **[nao verificado]**. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:47`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:124`. |
| **c. Escritor fora de `perguntar_pela_celula`** | Chamadas diretas ao escritor específico. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:148`. | Mesmas indireções de função; além disso, o **POST humano já existente** pode produzir o mesmo universo sem chamar o escritor específico. `app/pautas/views.py:76`, `app/pautas/views.py:80`, `app/pautas/views.py:81`; `app/pautas/services.py:180`. |
| **d. Trava → juiz → escrever em `atomic`** | Inversões diretas, ausência ou repetição das chamadas e os casos negativos incluídos. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:174`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:257`. | Não valida argumentos, identidade do `atomic` nem interrupções do fluxo entre os passos. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:179`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:182`. |
| **e. Registro** | Presença do endereço entre os valores da família. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:197`. | Chave errada e segundo registro do mesmo endereço passam. Cura mínima: exigir chave exata e unicidade. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:195`; `app/core/juizes.py:293`. |

**PROCEDENTE — ordem com falsa garantia.** Na execução em memória, os **11 testes novos passaram**, individualmente, com: juiz sem `cel.data`; trava de outro colaborador; `falso.atomic()`; retorno antes do bloco; retorno após a trava; comparação de fechamento invertida; contador fixado em zero. Os pontos permissivos são `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:170`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:179`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:182`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:216`.

Cura mínima: conferir argumentos da trava/juiz, identidade de `transaction.atomic` e sequência executável. Fechamento e contador precisam da proteção comportamental já existente; esses mutantes **não demonstram** que a suíte inteira passaria. `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:55`, `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:80`.

**PROCEDENTE — brecha alcançável hoje pela entrada humana.** Staff autorizado pode enviar duas vezes marcador + DP + mesma âncora colab: a porta comum cria duas cabeças sem a trava/deduplicação específica, enquanto o contador `distinct` continua mostrando uma pessoa. Isso é segunda origem da pauta específica, usando a porta comum; ocorrência real: **[nao verificado]**. Cura mínima: reservar o prefixo ao fluxo específico na entrada comum, declarar essa validação no contrato do selo e testar a recusa pela entrada humana. `app/pautas/views.py:16`, `app/pautas/views.py:76`; `app/pautas/services.py:153`, `app/pautas/services.py:180`; `app/chamados/services/fabrica_da_celula.py:320`.

3. **MESMA PERGUNTA / SEGUNDO JUIZ — CONFORME no chamador atual.**

A chave registrada corresponde ao predicado efetivamente usado: cabeça DP de cadastro da pessoa, viva ou fechada em data local igual/posterior ao dia consultado. O chamador passa exatamente `(cel.colaborador_id, cel.data)`. A assinatura permite `dia=None`, caso em que responde apenas sobre vivas; essa forma não é a chamada da fábrica. `app/core/juizes.py:293`; `app/chamados/services/fabrica_da_celula.py:288`, `app/chamados/services/fabrica_da_celula.py:297`, `app/chamados/services/fabrica_da_celula.py:299`, `app/chamados/services/fabrica_da_celula.py:304`, `app/chamados/services/fabrica_da_celula.py:337`.

O juiz não verifica sozinho “passado sem turno”; essa pré-condição vem do chamador. Logo, o contrato depende desse caminho, atualmente presente. `app/chamados/services/fabrica_da_celula.py:172`, `app/chamados/services/fabrica_da_celula.py:174`.

**Nenhum segundo juiz registrado da mesma entrega foi identificado.** `pergunta_viva`, `dia_sem_trabalho`, `tarefa_do_dia` e `dia_da_pergunta_trancado` têm outras perguntas; o contador mede somente pessoas com pendência viva. `app/core/juizes.py:278`, `app/core/juizes.py:290`, `app/core/juizes.py:291`, `app/core/juizes.py:292`; `app/chamados/services/fabrica_da_celula.py:320`.

4. **OBRIGAÇÕES POSITIVAS — cumprimento e proteção.**

| Obrigação | Código da cópia | Selo novo / cura mínima |
|---|---|---|
| **i. Passado sem turno não pergunta ao colab** | **CONFORME na fábrica**: encaminha cadastro e retorna. **CONFORME no fio mudo quanto ao silêncio**: helper devolve vazio e o ramo pula. **PROCEDENTE em `disputa_emissao`**, explicado abaixo. `app/chamados/services/fabrica_da_celula.py:175`, `app/chamados/services/fabrica_da_celula.py:38`; `app/chamados/management/commands/disparar_perguntas_competencia.py:158`, `app/chamados/management/commands/disparar_perguntas_competencia.py:160`. | O novo selo não testa essa obrigação nos emissores. Falta caso negativo completo para `disputa_emissao` e caso específico do fio mudo; falta também detectar cadastro não entregue quando esses caminhos apenas calam. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:199`; `app/chamados/services/fabrica_da_celula.py:213`. |
| **ii. Uma pendência por colab** | **CONFORME para escritores cooperantes**: `atomic`, trava da pessoa, seleção e escrita; a trava usa `SELECT FOR UPDATE`. Duas conexões reais: **[nao verificado]**. A entrada humana rompe a garantia global descrita no item 2. `app/chamados/services/fabrica_da_celula.py:334`, `app/chamados/services/fabrica_da_celula.py:337`; `app/chamados/models.py:2666`. | Testes anteriores cobrem dois dias e repetição sequencial; não equivalem a dois processos concorrentes. Falta ensaio com conexões distintas e proteção dos argumentos no AST. `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:51`; `app/chamados/tests/test_e4_c20.py:95`, `app/chamados/tests/test_e4_c20.py:105`. |
| **iii. Nome/número/esperado/dono** | **CONFORME**: nome `cadastro_sem_turno`, número de pessoas distintas com pauta viva, esperado `0`, dono `CADASTRO`. Nome aparece também no rótulo VFM/chave JSON; número/esperado/dono aparecem na linha textual. O JSON não transporta dono. `app/chamados/services/fabrica_da_celula.py:320`, `app/chamados/services/fabrica_da_celula.py:321`; `app/chamados/management/commands/vigiar_fio_morto.py:202`, `app/chamados/management/commands/vigiar_fio_morto.py:229`. | O novo selo exige chamada ao seletor, sem conferir os quatro valores nem o consumo pelo vigia. O contrato dos quatro campos já tem teste anterior. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:217`; `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:55`. |
| **iv. Fechada não reabre em laço** | **CONFORME** para dia anterior ou igual à data local do fechamento. Dia posterior permite nova pauta. `app/chamados/services/fabrica_da_celula.py:304`, `app/chamados/services/fabrica_da_celula.py:337`. | Proteção comportamental anterior cobre feita, cancelada, dia coberto e posterior; o AST novo não protege essa semântica. Falta caso na fronteira UTC/data local. `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:58`, `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:67`, `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:80`, `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:90`. |
| **v. Vigia só alarma, nunca julga** | **CONFORME no acompanhamento CADASTRO**: lê, imprime e carimba; esse contador deliberadamente não dispara alarme operacional. O modo `--alarme` de disparar retorna antes da escrita. `app/chamados/management/commands/vigiar_fio_morto.py:179`, `app/chamados/management/commands/vigiar_fio_morto.py:202`; `app/chamados/management/commands/disparar_perguntas_competencia.py:253`. | O novo selo não proíbe julgamento/escrita no vigia; exigir que seu arquivo seja varrido não protege esse papel. O fio mudo escritor está declarado separadamente como exceção de prazo. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:206`; `app/config/crons.py:2044`. |

**PROCEDENTE — caminho de emissão sem previsão.** `_tem_base` aceita batidas; `classificar_falta` pode manter `esperada=[]` e devolver motivo por batida unilateral; `_ata_do_dia` permite continuar sem célula/escala; o agrupador chama a abertura da pergunta. Portanto existe caminho de código que contraria a proibição fornecida, sem demonstrar ocorrência em produção. `app/chamados/services/disputa_emissao.py:814`; `app/escala/servico_jornada.py:396`, `app/escala/servico_jornada.py:444`, `app/escala/servico_jornada.py:446`; `app/chamados/services/disputa_emissao.py:906`, `app/chamados/services/disputa_emissao.py:1234`, `app/chamados/services/disputa_emissao.py:1241`.

Cura mínima: fazer o classificador canônico respeitar a ausência de previsão e encaminhar a pendência pelo fluxo de evento/porta autorizado, com teste do emissor completo. A fila existente chama esse caso de “falta lei”, mas a exceção “sem turno **com batida**” não está no aval fornecido: autorização dessa exceção **[nao verificado]**. `app/docs/BACKLOG.md:1876`; `app/docs/PROMPTS.md:2004`.

Quanto ao **dia posterior ao fechamento**: a implementação e os testes adotam uma nova pendência viva para uma nova ocorrência. Isso é compatível com “uma pendência viva por colab”; autorização literal específica dessa política no aval raspado: **[nao verificado]**. `app/chamados/services/fabrica_da_celula.py:295`; `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:58`.

5. **CLASSES QUE JÁ REPROVARAM OBRA.**

| Classe | Resultado |
|---|---|
| Segundo juiz que duplica registrado | **NÃO identificado para entrega CADASTRO.** `app/core/juizes.py:293`; `app/chamados/services/fabrica_da_celula.py:288`. |
| Segundo escritor fora da porta | **NÃO demonstrado como escritor ORM externo; SIM, segunda origem que contorna a deduplicação específica**, pelo POST. `app/pautas/views.py:76`; `app/pautas/services.py:180`. |
| Relógio fora da borda | **NÃO no novo juiz**: converte instante persistido. **SIM, preexistente no classificador do emissor**, que lê agora internamente. `app/chamados/services/fabrica_da_celula.py:304`; `app/escala/servico_jornada.py:368`. |
| Derivação paralela de fonte única | **SIM para a política de emissão**: a fábrica cala sem previsão; o classificador ainda acusa unilateral sem sequência prevista. Não é segundo juiz de entrega. `app/chamados/services/fabrica_da_celula.py:38`; `app/escala/servico_jornada.py:396`, `app/escala/servico_jornada.py:444`. |
| Selo/RED sem mordida | **SIM, parcialmente**: ordem e registro aceitam os mutantes descritos; usos diretos têm mordida. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:179`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:197`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:224`. |
| Ordem de trava | **NÃO no escritor atual; SIM na proteção incompleta do selo.** `app/chamados/services/fabrica_da_celula.py:334`, `app/chamados/services/fabrica_da_celula.py:337`; `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:182`. |
| Pouso antes do pré-requisito | **[nao verificado]**: cópia e relato não certificam execução/deploy; a exigência está em `CLAUDE.md:374`. | [nao verificado]
| Campo com dois significados | **NÃO identificado nos tipos pessoa/data/instante; SIM na explicação de “entregue = DP olhou”.** `app/chamados/services/fabrica_da_celula.py:292`; `app/pautas/models.py:94`. |

6. **VARREDURA DO FILESYSTEM — pode ler não versionado ou falhar; container efetivo [nao verificado].**

O selo percorre o filesystem, sem inventário Git, incluindo `static`, `staticfiles`, `media` e `logs`. Na montagem da cópia, `staticfiles` vem do host e é ignorado pelo Git; logs/media usam tmpfs. Portanto um `.py` não versionado nesses locais pode entrar na certificação. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:26`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:37`; `.gitignore:12`, `.gitignore:13`; `bin/arvore_do_push.sh:55`, `bin/arvore_do_push.sh:81`.

Leitura sem UTF-8, arquivo ilegível ou Python inválido pode interromper o teste; diretório inacessível pode ser omitido pelo `os.walk` sem `onerror`. Não há tratamento nesses pontos. Falha efetiva no container: **[nao verificado]**. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:37`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:132`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:204`.

A medição local em memória varreu **760 arquivos em aproximadamente 1,63 s**; os 11 testes levaram aproximadamente **1,67 s**. Esse custo local é pequeno; tempo no container e crescimento dos diretórios: **[nao verificado]**. O percurso também enumera arquivos que não serão analisados como Python. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:37`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:199`.

**PROCEDENTES finais:**

- Emissão sem previsão ainda alcançável em `disputa_emissao`; corrigir o predicado canônico e proteger o caminho completo. `app/escala/servico_jornada.py:444`; `app/chamados/services/disputa_emissao.py:1241`.
- Entrada humana produz o marcador sem deduplicação específica; reservar essa espécie ao fluxo autorizado. `app/pautas/views.py:76`; `app/pautas/services.py:180`.
- Selo de ordem aceita trava/juiz/atomic incorretos e fluxo interrompido; validar identidade, argumentos e sequência executável. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:179`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:182`.
- Teste de registro aceita pergunta errada e duplicação; exigir chave exata e unicidade. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:197`.
- Explicação “DP já olhou” excede o predicado; corrigir a documentação do fechamento/cancelamento. `app/chamados/services/fabrica_da_celula.py:292`; `app/pautas/services.py:288`.

## 09/11 21:13 · sessao d6f821d9f1e7 · - · - · origem=Ronald (tmux)

**PROCEDENTE — ordem selada apenas pela forma.** Em memória, os 11 testes novos ficaram verdes para cada mutante: juiz sem `cel.data`, trava de outro colaborador, `falso.atomic()`, retorno antes do bloco, retorno após a trava, fechamento `>=` trocado por `<=` e contador fixado em zero. `_ordem_ok` não confere argumentos ou identidade das funções e ignora instruções intermediárias que interrompem o fluxo. Cura mínima: conferir argumentos, vínculo ao `transaction.atomic` e sequência executável; proteger fechamento/contador com testes semânticos. (`app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:170`, `:179`, `:182`, `:216`; `app/chamados/services/fabrica_da_celula.py:304`, `:320`, `:335`, `:337`)

**PROCEDENTE — registro incompleto.** Chave errada mantendo o valor e segundo registro com o mesmo valor passam no teste de registro. Cura mínima: exigir a chave exata e unicidade do endereço nessa família. (`app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:195`; `app/core/juizes.py:293`)

**CONFORME, com limites — o selo morde usos diretos.** Nome/import alias/atributo direto/literal completo são detectados; importar módulo e chamar seu atributo também é detectado, assim como outra constante contendo o mesmo literal. Entretanto, `getattr`, referência intermediária `f = modulo._pautas_cadastro; f()`, equivalente do escritor, `Q` com prefixo composto por `+`, regex sem literal completo e SQL com parâmetro composto escapam. Esses exemplos são mutantes hipotéticos, sem ocorrência encontrada no código atual. (`app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:47`, `:61`, `:111`, `:120`, `:124`, `:138`)

**Caminho atual alcançável — entrada humana pode produzir o marcador.** POST de staff aceita texto, destinatário e âncora arbitrários e chama a porta genérica sem deduplicação específica/trava por colaborador. Entrada adequada pode criar outra pauta CADASTRO sem passar por `pauta_cadastro_sem_turno`; ocorrência real é `[nao verificado]`. Isso continua usando a porta genérica de Pauta, portanto não comprova escritor ORM fora dela. (`app/config/urls.py:36`; `app/pautas/urls.py:9`; `app/pautas/views.py:16`, `:72`, `:76`; `app/pautas/services.py:153`, `:180`)

**Varredura — pode incluir arquivo não versionado.** Percorre filesystem, sem consultar Git; `staticfiles` do host é montado no container como diretório real, embora ignorado no Git. Logs/media ficam em tmpfs. `.py` inválido, ilegível ou sem UTF-8 pode falhar em leitura/parse; diretório inacessível pode ser omitido pelo `os.walk` sem `onerror`. Falha efetiva nesse container: `[nao verificado]`. Cura mínima: universo de fontes versionadas/materializado para o selo, distinguindo arquivos gerados. (`app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:26`, `:36`, `:132`, `:204`; `.gitignore:13`; `bin/arvore_do_push.sh:55`, `:81`)

**Custo local:** varredura de 760 arquivos sem erros/violações em aproximadamente 1,63 s; os 11 testes em aproximadamente 1,67 s, usando `python3 -B`, AST e `unittest.TestCase` em memória. Tempo/resultado no container Django: `[nao verificado]`. O método que determina esse custo está em `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:199`.

**Limite importante:** os mutantes passaram apenas pelo selo novo. Testes funcionais anteriores já cobrem contador, fechamento e dia posterior; não se pode concluir que a suíte inteira os aceitaria. (`app/chamados/tests/test_e4a_pauta_cadastro_contador.py:55`, `:61`, `:76`, `:84`, `:88`)

## 09/11 21:12 · sessao 57bbec0b6ee6 · - · - · origem=Ronald (tmux)

- **Juiz e registro — CONFORME no caminho efetivo.** A chave nova de `JUIZES['chamado']` aponta para `pauta_cadastro_existente(colaborador_id, dia=None)`; o único chamador de produção específico passa `(cel.colaborador_id, cel.data)`. O universo é cabeça, departamento DP, âncora de pessoa e prefixo do marcador. Não há outro juiz de entrega CADASTRO registrado; `dia_sem_trabalho`, `tarefa_do_dia`, `pergunta_viva` e `dia_da_pergunta_trancado` respondem outras perguntas. (`app/core/juizes.py:278`, `app/core/juizes.py:290`, `app/core/juizes.py:291`, `app/core/juizes.py:292`, `app/core/juizes.py:293`; `app/chamados/services/fabrica_da_celula.py:279`, `app/chamados/services/fabrica_da_celula.py:288`, `app/chamados/services/fabrica_da_celula.py:337`)

- **Escrita/leitura dos campos.** O marcador nasce como constante em `:276`, é consumido pelo filtro em `:285` e incorporado ao texto em `:342`. O escritor específico assina TI, destina a DP, usa `ANCORA_COLAB` e `str(cel.colaborador_id)`, sem vínculo ou data na chave. A porta genérica grava esses campos em `Pauta.objects.create`; o seletor e juiz leem os mesmos valores; contador deduplica por `ancora_id`. (`app/chamados/services/fabrica_da_celula.py:276`, `app/chamados/services/fabrica_da_celula.py:284`, `app/chamados/services/fabrica_da_celula.py:297`, `app/chamados/services/fabrica_da_celula.py:320`, `app/chamados/services/fabrica_da_celula.py:339`; `app/pautas/models.py:30`; `app/pautas/services.py:180`)

- **Viva/fechada e datas — CONFORME com ressalva semântica.** Viva vem da autoridade `PautaQuerySet.vivas`: não feita e não cancelada. Fechada cobre `timezone.localdate(cancelado_em or feito_em) >= dia`; sem `dia`, só viva cobre. `feito_em` é escrito por `marcar_feita` e `marcar_feita_pelo_sistema`; `cancelado_em` por `cancelar`; os três usam instantes `timezone.now()`. O juiz converte esses instantes a data local, não compara uma `date` com um instante. Fuso base São Paulo e `USE_TZ=True`; fuso ativo real do processo: `[nao verificado]`. (`app/pautas/models.py:57`, `app/pautas/models.py:64`; `app/pautas/services.py:246`, `app/pautas/services.py:247`, `app/pautas/services.py:271`, `app/pautas/services.py:272`, `app/pautas/services.py:294`; `app/chamados/services/fabrica_da_celula.py:299`, `app/chamados/services/fabrica_da_celula.py:303`, `app/chamados/services/fabrica_da_celula.py:304`; `app/config/settings/base.py:93`, `app/config/settings/base.py:95`)

- **Ressalva “DP já olhou”: não demonstrada.** A docstring diz que fechamento significa “o DP já olhou”, porém cancelamento é ato do autor e dispensa leitura; `pauta_cadastro_existente` não lê `lido_em`. Isso não muda a definição operacional declarada de “entregue”, mas não autoriza afirmar que DP leu/resolveu. A cura mínima é corrigir a explicação, se “entregue” significa encaminhada; mudar o predicado exige decidir se cancelamento deveria continuar cobrindo. (`app/chamados/services/fabrica_da_celula.py:292`, `app/chamados/services/fabrica_da_celula.py:301`, `app/pautas/models.py:94`, `app/pautas/services.py:288`, `app/pautas/services.py:294`)

- **Uma pendência pelo escritor específico — CONFORME estático; concorrência real `[nao verificado]`.** O escritor abre `atomic`, trava a pessoa, consulta o juiz e escreve. `trava_na_ordem` executa `SELECT FOR UPDATE` da linha de `Colaborador`; portanto dois escritores cooperantes do mesmo colab serializam antes da seleção. Não há chave por dia/vínculo. (`app/chamados/services/fabrica_da_celula.py:334`, `app/chamados/services/fabrica_da_celula.py:335`, `app/chamados/services/fabrica_da_celula.py:337`, `app/chamados/services/fabrica_da_celula.py:339`; `app/chamados/models.py:2666`)

- **Brecha alcançável hoje: segunda origem pelo POST genérico — PROCEDENTE se o contrato “UMA por colab” incluir todas as entradas do produto.** Staff autorizado pode postar duas vezes o mesmo texto com marcador, `ancora_tipo=colab`, mesmo id e DP: rota existe, view recebe tudo do POST, service grava sem reservar prefixo, travar pessoa ou deduplicar. Isso cria duas cabeças vivas; contador por `distinct` continua mostrando 1. Não é segundo escritor ORM fora da porta, mas entrada alternativa que contorna o escritor específico. O selo AST não vê valores vindos do POST. Ocorrência em produção `[nao verificado]`. Cura mínima: reservar o prefixo à entrada específica na porta comum e acrescentar teste negativo da entrada humana; não basta procurar mais tokens em fonte. (`app/config/urls.py:36`; `app/pautas/urls.py:9`; `app/pautas/views.py:16`, `app/pautas/views.py:73`, `app/pautas/views.py:76`, `app/pautas/views.py:80`, `app/pautas/views.py:81`; `app/pautas/services.py:153`, `app/pautas/services.py:180`; `app/chamados/services/fabrica_da_celula.py:320`; `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:120`)

- **Contador: os quatro campos existem.** `nome='cadastro_sem_turno'`, número de `ancora_id` distintos vivos, `esperado=0`, `dono='CADASTRO'`. O vigia chama o contador e imprime nome no rótulo `VFM|CADASTRO_SEM_TURNO`, número, esperado e dono. JSON contém chave/número em `censo` e esperado em `esperado`, mas não transporta dono; o dono continua na função e linha textual. `bin/fio_estado.py` lê o JSON e compara genericamente esperado versus censo; `gerar_estado.py` usa essa apresentação. (`app/chamados/services/fabrica_da_celula.py:309`, `app/chamados/services/fabrica_da_celula.py:320`, `app/chamados/services/fabrica_da_celula.py:321`; `app/chamados/management/commands/vigiar_fio_morto.py:98`, `app/chamados/management/commands/vigiar_fio_morto.py:179`, `app/chamados/management/commands/vigiar_fio_morto.py:202`, `app/chamados/management/commands/vigiar_fio_morto.py:223`, `app/chamados/management/commands/vigiar_fio_morto.py:229`; `bin/fio_estado.py:104`, `bin/fio_estado.py:125`, `bin/fio_estado.py:191`; `bin/gerar_estado.py:260`)

- **Vigia do contador — CONFORME como leitor.** `vigiar_fio_morto` só chama contador/imprime/carimba; CADASTRO fica fora de `ALARMA_SEMPRE` e `ALARMA_EM_0`, portanto nem dispara `SystemExit` por essa pendência. O leitor do ESTADO mostra divergência “fora do esperado”. O aval fornecido exige contador, não exige interromper a corrente por CADASTRO. (`app/chamados/management/commands/vigiar_fio_morto.py:83`, `app/chamados/management/commands/vigiar_fio_morto.py:84`, `app/chamados/management/commands/vigiar_fio_morto.py:179`, `app/chamados/management/commands/vigiar_fio_morto.py:181`, `app/chamados/management/commands/vigiar_fio_morto.py:202`, `app/chamados/management/commands/vigiar_fio_morto.py:239`; `bin/fio_estado.py:191`)

- **Fechamento/dia novo — implementação e testes existentes.** Viva cobre qualquer dia; fechada cobre até a data local inclusiva. Dia posterior cria nova pauta, mantendo só uma viva pelo escritor cooperante. Isso é coerente com nova ocorrência de cadastro faltante; autorização literal específica dessa reemissão no aval raspado `[nao verificado]`; a decisão está expressa na docstring e no selo comportamental anterior. (`app/chamados/services/fabrica_da_celula.py:292`, `app/chamados/services/fabrica_da_celula.py:295`, `app/chamados/services/fabrica_da_celula.py:304`; `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:58`, `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:67`, `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:80`, `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:90`)

- **Novo selo não protege todas as obrigações positivas.** Ele exige sítios, contagens de chamadas e forma sequencial; não fixa conteúdo dos filtros, número/dono/esperado, comparação de fechamento/data local, argumentos corretos da trava/juiz ou emissão em todos os emissores. Existem testes comportamentais anteriores para dois dias/uma pauta, quatro campos, feita/cancelada, dia posterior e dia antigo; o teste isolado de ordem/duas rodadas usa dublês, não duas conexões concorrentes. Cura mínima: preservar esses testes e acrescentar concorrência real e fronteira local de meia-noite, além de identidade/argumentos do AST. (`app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:174`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:199`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:216`; `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:51`, `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:55`, `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:58`, `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:80`; `app/chamados/tests/test_e4_c20.py:65`, `app/chamados/tests/test_e4_c20.py:96`, `app/chamados/tests/test_e4_c20.py:105`)

- **Leitores genéricos não são outro juiz.** A aba seleciona por destinatário e viva, serializa texto/âncora/estado; o copiloto exibe `p.texto`; API mensageria lista recebidas DP e entrega texto/âncora; `pautas_do_colab` lista pautas de pessoa/dia sem interpretar o marcador. (`app/pautas/services.py:308`, `app/pautas/services.py:322`, `app/pautas/services.py:433`; `app/pautas/views.py:40`, `app/pautas/views.py:43`; `app/templates/core/_copiloto.html:405`; `app/api/views_mensageria.py:1324`, `app/api/views_mensageria.py:1329`, `app/api/views_mensageria.py:1334`)

Não executei banco, log ou testes que escrevem arquivos. Resultado de execução da suíte e correspondência independente com o commit de produção `[nao verificado]`. [nao verificado]

## 09/11 21:11 · sessao a1ada130ce61 · - · - · origem=Ronald (tmux)

**PROCEDENTE — obrigação (i), fora da fábrica.** `disputa_emissao` pode perguntar por dia passado sem previsto: `_tem_base` aceita batidas (`app/chamados/services/disputa_emissao.py:814`); `classificar_falta` deixa a sequência prevista vazia sem escala (`app/escala/servico_jornada.py:396`) e mesmo assim acusa batida unilateral (`app/escala/servico_jornada.py:444`, `app/escala/servico_jornada.py:446`). `_ata_do_dia` autoriza continuar sem célula e sem escala (`app/chamados/services/disputa_emissao.py:899`, `app/chamados/services/disputa_emissao.py:906`); o agrupador chega à abertura (`app/chamados/services/disputa_emissao.py:1232`, `app/chamados/services/disputa_emissao.py:1241`). A porta verifica identidade/costura, mas chega à inserção sem guarda de previsão (`app/chamados/models.py:2789`, `app/chamados/models.py:2896`, `app/chamados/models.py:3017`). Ocorrência real em produção: **[nao verificado]**.

Correção mínima: fazer o juiz canônico abster-se quando não há sequência prevista, proteger o caminho completo de emissão com caso negativo e encaminhar CADASTRO pela porta/evento autorizado. O novo selo só verifica nomes/sítios e forma do escritor, portanto não detecta essa emissão (`app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:138`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:199`).

**CONFORME — fio mudo não pergunta nesse caso.** Ele passa `vencidos_no_instante` a `motivos_pela_ata` e pula quando não há motivo (`app/chamados/management/commands/disparar_perguntas_competencia.py:158`, `app/chamados/management/commands/disparar_perguntas_competencia.py:160`); o helper devolve vazio para ausência de previsto (`app/chamados/services/fabrica_da_celula.py:38`). Esse ramo não encaminha ao CADASTRO; a entrega depende do evento da fábrica (`app/chamados/services/fabrica_da_celula.py:175`). O teste existente de lote sem turno monta apenas célula, sem chamado mudo, portanto não cobre especificamente esse ramo (`app/chamados/tests/test_fabrica_pela_ata.py:30`, `app/chamados/tests/test_fabrica_pela_ata.py:63`).

**CONFORME — vigia versus emissor declarado.** O ramo célula conta (`app/chamados/management/commands/disparar_perguntas_competencia.py:112`); `--alarme` retorna antes de escrever (`app/chamados/management/commands/disparar_perguntas_competencia.py:253`). O fio mudo escreve com `--apply`, mas está explicitamente declarado como exceção de prazo, não vigia (`app/config/crons.py:2044`, `app/chamados/management/commands/disparar_perguntas_competencia.py:282`). Não classificar essa escrita, isoladamente, como violação.

**CONFORME — contador CADASTRO não alarma deliberadamente.** O vigia lê o contador (`app/chamados/management/commands/vigiar_fio_morto.py:179`), publica número/esperado/dono e “não alarma” (`app/chamados/management/commands/vigiar_fio_morto.py:202`); CADASTRO está fora dos conjuntos de alarme (`app/chamados/management/commands/vigiar_fio_morto.py:83`, `app/chamados/management/commands/vigiar_fio_morto.py:84`). O aval fornecido exige contador, sem exigir alarme específico por cadastro **[nao verificado: anexo transcrito sem arquivo local]**. O selo novo não protege essa ligação ao vigia (`app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:206` apenas exige que o arquivo seja varrido).

**Relógio:** no juiz novo, `localdate(fechou)` converte timestamp persistido, sem ler agora (`app/chamados/services/fabrica_da_celula.py:304`). No universo ampliado, há leitura de relógio dentro de `classificar_falta` (`app/escala/servico_jornada.py:368`) e fallback em `cobra_marcos_da_ata` (`app/chamados/services/disputa_emissao.py:744`); nenhuma dessas leituras é protegida pelo selo novo (`app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:124`).

**Selos existentes relevantes, sem executar banco:** deduplicação sequencial e quatro campos do contador (`app/chamados/tests/test_e4a_pauta_cadastro_contador.py:51`); fechada não cala dia posterior (`app/chamados/tests/test_e4a_pauta_cadastro_contador.py:58`); fechada cobre dias anteriores e iguais (`app/chamados/tests/test_e4a_pauta_cadastro_contador.py:80`); trava antes de consulta com stubs, duas chamadas sequenciais (`app/chamados/tests/test_e4_c20.py:64`). Esses casos não demonstram dois processos concorrentes (`app/chamados/tests/test_e4_c20.py:105`, `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:52`).

## 09/11 21:08 · sessao 0784c8db88ae · MODO-COMUM-PASSADO-SEM-TURNO · construtor · origem=Code

Arquivos entregues:

- [test_juiz_pauta_cadastro_sem_turno.py](/home/ronald/wt-codex-modo-comum-passado-sem-turno-210159/app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:194): selo AST, registro, anti-vacuidade e casos MORDE para a–d.
- [JUIZES.md](/home/ronald/wt-codex-modo-comum-passado-sem-turno-210159/app/docs/JUIZES.md:86): linha com juiz, escritor, contador e selo.

Do rascunho, corrigi:

- Varredura inclui Python em `static/media/logs`; exclui testes, migrations, dependências e links simbólicos (`chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:26`).
- Só a definição da constante recebe exceção no módulo; imports, aliases, atributos e literais são fiscalizados (`chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:96`).
- Escopos aninhados e métodos não herdam autorização; docstrings são ignoradas (`chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:71`).
- Ordem exige trava, guarda do juiz e escrita no mesmo bloco `atomic`, rejeitando duplicações e chamadas em blocos sem execução (`chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:155`).

Comando executado para base e dois mutantes reais, restaurados em `finally`. Como Django não estava disponível, o import de `SimpleTestCase` foi substituído em memória por `unittest.TestCase`; execução com Django: [nao verificado].

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B - <<'PY'
import ast
import io
import pathlib
import unittest

path = pathlib.Path('chamados/tests/test_juiz_pauta_cadastro_sem_turno.py')
tree = ast.parse(path.read_text(), str(path))
for node in tree.body:
    if isinstance(node, ast.ImportFrom) and node.module == 'django.test':
        node.module = 'unittest'
        node.names = [ast.alias(name='TestCase', asname='SimpleTestCase')]
namespace = {'__file__': str(path.resolve()), '__name__': 'selo_direto'}
exec(compile(ast.fix_missing_locations(tree), str(path), 'exec'), namespace)
classe = namespace['JuizPautaCadastroSemTurnoTest']
fabrica = pathlib.Path(namespace['FABRICA'])
original = fabrica.read_bytes()
fonte = original.decode()
fn = next(n for n in ast.parse(fonte).body
          if isinstance(n, ast.FunctionDef)
          and n.name == namespace['ESCRITOR'])
bloco = next(n for n in fn.body if isinstance(n, ast.With))
guarda, escrita = bloco.body[1:3]
assert isinstance(guarda, ast.If) and isinstance(escrita, ast.Expr)
linhas = fonte.splitlines(keepends=True)
mutante = ''.join(
    linhas[:guarda.lineno - 1]
    + linhas[escrita.lineno - 1:escrita.end_lineno]
    + linhas[guarda.lineno - 1:guarda.end_lineno]
    + linhas[escrita.end_lineno:])
ast.parse(mutante)
leitor = pathlib.Path('core/views.py')
leitor_original = leitor.read_bytes()
intruso = (
    b'\n\ndef _mutante_cadastro_sem_turno():\n'
    b'    from chamados.services.fabrica_da_celula import PAUTA_SEM_TURNO\n'
    b'    return Pauta.objects.filter(texto__startswith=PAUTA_SEM_TURNO)\n')

for nome, arquivo, alterado in (
    ('BASE', fabrica, original),
    ('M1_ESCRITA_ANTES_DO_JUIZ', fabrica, mutante.encode()),
    ('M2_SEGUNDO_LEITOR', leitor, leitor_original + intruso),
):
    antes = arquivo.read_bytes()
    try:
        if alterado != antes:
            arquivo.write_bytes(alterado)
        suite = unittest.defaultTestLoader.loadTestsFromTestCase(classe) [nao verificado]
        saida = io.StringIO()
        result = unittest.TextTestRunner(stream=saida).run(suite) [nao verificado]
        print(f'=== {nome} ===', flush=True)
        print(saida.getvalue(), end='', flush=True)
        print(f'RC={int(not result.wasSuccessful())}', flush=True)
        if nome == 'BASE':
            assert result.wasSuccessful()
        else:
            assert len(result.failures) == 1 and not result.errors
    finally:
        if arquivo.read_bytes() != antes:
            arquivo.write_bytes(antes)
    assert arquivo.read_bytes() == antes
assert fabrica.read_bytes() == original
assert leitor.read_bytes() == leitor_original
print('RESTAURACAO: fabrica e core/views.py identicos aos bytes originais')
PY
```

Saída relevante dos testes de `chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:199` e `:220`:

```text
=== BASE ===
Ran 11 tests in 1.702s
OK
RC=0

=== M1_ESCRITA_ANTES_DO_JUIZ ===
AssertionError: False is not true : chamados/services/fabrica_da_celula.py::pauta_cadastro_sem_turno: esperado trava -> juiz -> escrever em atomic
Ran 11 tests in 1.695s
FAILED (failures=1)
RC=1

=== M2_SEGUNDO_LEITOR ===
'core/views.py:1310::_mutante_cadastro_sem_turno le/escreve o marcador por conta propria'
'core/views.py:1311::_mutante_cadastro_sem_turno le/escreve o marcador por conta propria'
Ran 11 tests in 1.713s
FAILED (failures=1)
RC=1
RESTAURACAO: fabrica e core/views.py identicos aos bytes originais
```

Ruff nos dois arquivos entregues (`chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:1`; `docs/JUIZES.md:86`):

```bash
/tmp/claude-1001/-home-ronald-saas-hasner-app/707e509b-29e4-4438-9018-16733980c39a/scratchpad/ruffpkg/bin/ruff check --no-cache --config ruff.toml chamados/tests/test_juiz_pauta_cadastro_sem_turno.py docs/JUIZES.md
```

```text
All checks passed!
```

## 09/11 21:00 · sessao c358ee63e42c · MODO-COMUM-PASSADO-SEM-TURNO · censo · origem=Code

Quem responde diretamente é **`pauta_cadastro_existente(colaborador_id, dia)`**: procura uma cabeça de Pauta com o marcador, destinada ao DP e ancorada no colaborador; aceita uma viva ou uma fechada cuja data local seja igual ou posterior a `dia`. O contador responde outra pergunta: quantos colaboradores têm essa pendência **viva**. (`app/chamados/services/fabrica_da_celula.py:284`, `app/chamados/services/fabrica_da_celula.py:297`, `app/chamados/services/fabrica_da_celula.py:301`, `app/chamados/services/fabrica_da_celula.py:320`)

**1. Censo das consultas e varredura AST**

Foram encontrados estes **seis sítios concretos de consulta por âncora de colaborador**:

| Sítio | Seleção | Responde à pergunta investigada? |
|---|---|---|
| `app/chamados/services/fabrica_da_celula.py:284` | `ANCORA_COLAB`, DP, cabeça, prefixo `PAUTA_SEM_TURNO`. | Seleciona exatamente o universo investigado. |
| `app/pautas/services.py:435` | `Q(ancora_tipo='colab', ...)`, unido às pautas de dias da pessoa, em `:436`. | Listagem geral; não interpreta o marcador. |
| `app/ponto/services/afastado_avisa.py:62` | `'colab'`, supervisão, cabeça viva, prefixo `TEXTO[:40]`. | Outro assunto. |
| `app/ponto/services/ausencia.py:1039` | `'colab'`, DP, viva, prefixo `[A4/SEM-DOCUMENTO]`, definido em `:1038`. | Outro assunto. |
| `app/ponto/services/ausencia.py:1229` | `'colab'`, DP, viva, prefixo `[A1-FERIAS/AG#%s]`, definido em `:1228`. | Outro assunto. |
| `app/ponto/services/pautas_dos_deslogados.py:65` | `ANCORA='colab'`, `DEPARTAMENTO='supervisao'`, marcador `[APP-401]`, definidos em `:26–28`. | Outro assunto. |

No universo específico do marcador, todas estas operações consultam ou derivam o queryset:

| Arquivo:linha | Operação |
|---|---|
| `app/chamados/services/fabrica_da_celula.py:283` | Escolhe `Pauta.objects.vivas()` ou `Pauta.objects.all()`. |
| `app/chamados/services/fabrica_da_celula.py:284` | Aplica âncora, destinatário, cabeça e marcador. |
| `app/chamados/services/fabrica_da_celula.py:297` | Filtra colaborador e executa `exists()` nas vivas. |
| `app/chamados/services/fabrica_da_celula.py:301` | Filtra colaborador e lê os fechamentos no histórico. |
| `app/chamados/services/fabrica_da_celula.py:315` | Obtém as vivas para o contador. |
| `app/chamados/services/fabrica_da_celula.py:319` | Restringe pelas pessoas das empresas informadas. |
| `app/chamados/services/fabrica_da_celula.py:320` | Conta `ancora_id` distintos. |

Há ainda uma consulta genérica capaz de receber `'colab'`: `pendentes_por_ancora(tipo, ident)`. Ela conta pautas vivas de qualquer assunto, sem selecionar DP ou marcador; chamador efetivo com `'colab'`: **[nao verificado]**. (`app/pautas/services.py:334`, `app/pautas/services.py:336`)

O script abaixo foi executado em memória com `python3 -B`. Ele procura candidatos por AST, resolve constantes simples e apelidos importados, inclui `Q(...)`, escritas e âncoras não resolvidas para revisão manual. Reflexão, SQL externo e valores montados exclusivamente em execução: **[nao verificado]**.

```python
import ast
from pathlib import Path
mark = 'CADASTRO: PASSADO SEM TURNO PREVISTO'
count = errors = 0
for p in sorted(Path('.').rglob('*.py')):
    if set(p.parts) & {'tests','.venv','venv','node_modules','.git','__pycache__'}:
        continue
    if p.name.startswith('test_') or p.name in {'tests.py','conftest.py'}:
        continue
    count += 1
    try:
        t = ast.parse(p.read_text())
    except (SyntaxError,UnicodeError) as e:
        errors += 1
        print('ERRO',p,e)
        continue
    env = {'ANCORA_COLAB':'colab','PAUTA_SEM_TURNO':mark}
    for n in ast.walk(t):
        if isinstance(n,ast.ImportFrom):
            for a in n.names:
                if a.name in env: env[a.asname or a.name] = env[a.name]
                if a.name == 'Pauta' and a.asname:
                    print(f'ALIAS {p}:{n.lineno} {a.asname}=Pauta')
    def val(n):
        if isinstance(n,ast.Constant): return n.value
        if isinstance(n,ast.Name): return env.get(n.id)
        if isinstance(n,ast.Attribute): return env.get(n.attr)
    for _ in range(3):
        for n in ast.walk(t):
            if isinstance(n,ast.Assign) and val(n.value) is not None:
                for dst in n.targets:
                    if isinstance(dst,ast.Name): env[dst.id] = val(n.value)
    for n in sorted(ast.walk(t),key=lambda n:getattr(n,'lineno',0)):
        if not isinstance(n,ast.Call): continue
        f = ast.unparse(n.func)
        hit = any((k.arg=='ancora_tipo' and val(k.value)=='colab') or
                  (k.arg=='texto__startswith' and isinstance(val(k.value),str)
                   and val(k.value).startswith(mark)) for k in n.keywords)
        dynamic = any(k.arg=='ancora_tipo' and val(k.value) is None for k in n.keywords)
        query = isinstance(n.func,ast.Attribute) and n.func.attr in {
            'filter','exclude','get','get_or_create','update_or_create','create'}
        if hit or (dynamic and query):
            args = ', '.join(k.arg+'='+ast.unparse(k.value) for k in n.keywords
                             if k.arg in {'ancora_tipo','texto__startswith'})
            print(f'{"ALVO" if hit else "DINAMICO"} {p}:{n.lineno} {f}({args})')
print(f'ARQUIVOS={count} ERROS={errors}')
```

Saída:

```text
DINAMICO app/chamados/management/commands/reabrir_orfas_enterradas.py:201 Pauta.objects.filter(ancora_tipo=ANCORA_COMPETENCIA, texto__startswith=MARCADOR)
DINAMICO app/chamados/services/fabrica_da_celula.py:233 Pauta.objects.filter(ancora_tipo=ANCORA_DIA, texto__startswith=PAUTA_LIMBO_EXPORTADA)
ALVO app/chamados/services/fabrica_da_celula.py:284 q.filter(ancora_tipo=ANCORA_COLAB, texto__startswith=PAUTA_SEM_TURNO)
ALVO app/chamados/services/fabrica_da_celula.py:339 escrever(ancora_tipo=ANCORA_COLAB)
DINAMICO app/chamados/services/passivo_validado.py:163 Pauta.objects.filter(ancora_tipo=ANCORA_COMPETENCIA, texto__startswith=cabeca)
DINAMICO app/chamados/services/passivo_validado.py:255 Pauta.objects.filter(ancora_tipo=ANCORA_COMPETENCIA, texto__startswith=cabeca)
DINAMICO app/pautas/management/commands/pauta_pago_x_devido_exportada.py:242 Pauta.objects.nao_canceladas().filter(ancora_tipo=ancora_tipo, texto__startswith=chave + ' ')
DINAMICO app/pautas/services.py:180 Pauta.objects.create(ancora_tipo=ancora_tipo)
DINAMICO app/pautas/services.py:336 Pauta.objects.filter(ancora_tipo=tipo)
ALVO app/pautas/services.py:436 Q(ancora_tipo='colab')
ALVO app/ponto/services/afastado_avisa.py:62 Pauta.objects.filter(ancora_tipo='colab', texto__startswith=TEXTO[:40])
ALVO app/ponto/services/afastado_avisa.py:74 escrever(ancora_tipo='colab')
ALVO app/ponto/services/ausencia.py:1039 Pauta.objects.filter(ancora_tipo='colab', texto__startswith=marcador)
ALVO app/ponto/services/ausencia.py:1055 escrever(ancora_tipo='colab')
ALVO app/ponto/services/ausencia.py:1229 Pauta.objects.filter(ancora_tipo='colab', texto__startswith=marcador)
ALVO app/ponto/services/ausencia.py:1245 escrever(ancora_tipo='colab')
ALVO app/ponto/services/pautas_dos_deslogados.py:65 Pauta.objects.vivas().filter(ancora_tipo=ANCORA, texto__startswith=MARCADOR)
ALVO app/ponto/services/pautas_dos_deslogados.py:77 escrever(ancora_tipo=ANCORA)
ARQUIVOS=1197 ERROS=0
```

Os candidatos `DINAMICO` restantes foram separados assim:

- `ANCORA_COMPETENCIA` e `ANCORA_DIA` correspondem a outros tipos de âncora. (`app/pautas/models.py:33`, `app/pautas/models.py:34`)
- Os três chamadores de `_existente(...)` passam `ANCORA_COMPETENCIA`. (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:183`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:202`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:222`)
- `Pauta.objects.create(...)` é a implementação da porta de escrita, não uma consulta de entrega. (`app/pautas/services.py:153`, `app/pautas/services.py:180`)
- `pendentes_por_ancora(...)` permanece genérica, conforme descrito acima. (`app/pautas/services.py:336`)

**2. Quem escreve e quem lê o marcador**

**Escritor específico encontrado: um.** `pauta_cadastro_sem_turno` chama `pautas.services.escrever`, assinando por TI, destinando ao DP e usando o colaborador como âncora. O texto começa com `PAUTA_SEM_TURNO` e registra `cel.data` como primeiro dia observado. (`app/chamados/services/fabrica_da_celula.py:333`, `app/chamados/services/fabrica_da_celula.py:339`, `app/chamados/services/fabrica_da_celula.py:341`)

Seu único chamador de produção encontrado é `perguntar_pela_celula`, no ramo `venc is None`. Antes da deduplicação, o escritor abre transação e adquire a trava do colaborador. (`app/chamados/services/fabrica_da_celula.py:174`, `app/chamados/services/fabrica_da_celula.py:175`, `app/chamados/services/fabrica_da_celula.py:334`, `app/chamados/services/fabrica_da_celula.py:335`)

O endpoint humano também aceita texto e âncora arbitrários do POST, mas não contém uma referência específica ao marcador. Uso dele para escrever esse marcador: **[nao verificado]**. (`app/pautas/views.py:76`, `app/pautas/views.py:80`, `app/pautas/views.py:81`)

| Leitor | O que efetivamente lê |
|---|---|
| `app/chamados/services/fabrica_da_celula.py:284` | Interpreta o prefixo no banco. |
| `app/chamados/services/fabrica_da_celula.py:297` e `:301` | Usa esse universo para responder “já entregue?”. |
| `app/chamados/services/fabrica_da_celula.py:315` e `:320` | Conta colaboradores com pauta viva desse universo. |
| `app/chamados/management/commands/vigiar_fio_morto.py:179` | Chama o contador; imprime em `:202` e inclui no carimbo em `:229`. |
| `bin/fio_estado.py:104` | Lê o carimbo; compara genericamente `censo` e `esperado` em `:125`, exibindo divergências em `:191`. |
| `bin/gerar_estado.py:260` | Incorpora essa apresentação ao relatório. |

O vigia publica esperado zero, mas esse contador fica fora dos conjuntos que disparam seu alarme. (`app/chamados/management/commands/vigiar_fio_morto.py:83`, `app/chamados/management/commands/vigiar_fio_morto.py:98`, `app/chamados/management/commands/vigiar_fio_morto.py:181`, `app/chamados/management/commands/vigiar_fio_morto.py:202`)

Há também leitores **genéricos**, que transportam ou exibem o texto sem interpretar o marcador:

- Aba DP: seleção por departamento e pautas vivas; serialização de `p.texto`; apresentação no copiloto. (`app/pautas/services.py:308`, `app/pautas/services.py:322`, `app/pautas/views.py:40`, `app/templates/core/_copiloto.html:405`)
- API de mensageria: aba padrão DP, lista recebidas e entrega o texto. O cliente e a ferramenta de mensageria repassam esses itens. (`app/api/views_mensageria.py:1324`, `app/api/views_mensageria.py:1329`, `app/api/views_mensageria.py:1334`, `mensageria/nucleo/core_client.py:431`, `mensageria/nucleo/ferramentas.py:2241`, `mensageria/nucleo/ferramentas.py:2248`)
- Detalhe do colaborador: existe template que mostra estado e texto truncado. A alimentação efetiva do contexto `pautas_do_colab` nessa tela ficou **[nao verificado]**. (`app/templates/colaboradores/detalhe.html:113`, `app/templates/colaboradores/detalhe.html:118`, `app/templates/colaboradores/detalhe.html:119`)

**3. Decisões fora de `perguntar_pela_celula`**

A decisão explícita de **encaminhar esse caso ao CADASTRO** está em `perguntar_pela_celula`: exige passado, colaborador ativo, dia não dispensado por `dia_sem_trabalho`; quando `vencidos_no_instante` devolve `None`, escreve a pauta e retorna sem perguntas. (`app/chamados/services/fabrica_da_celula.py:172`, `app/chamados/services/fabrica_da_celula.py:174`, `app/chamados/services/fabrica_da_celula.py:175`)

O significado de `None` vem da ausência simultânea de marcos vencidos e pendentes em uma célula passada. A ausência de marcos também pode resultar de vínculo/tipo inexistente ou de horários de início/fim ausentes. (`app/chamados/services/fabrica_da_celula.py:26`, `app/chamados/services/fabrica_da_celula.py:27`, `app/ponto/services/cartorio.py:856`, `app/escala/servico_jornada.py:72`)

| Componente | Caminho confirmado |
|---|---|
| **`disparar_perguntas_competencia` — célula** | Lê `motivos_que_o_evento_deixou`; este chama `motivos_a_perguntar`. Com `vencidos=None`, `motivos_pela_ata` retorna vazio. Não verifica nessa passagem se a pauta CADASTRO existe. (`app/chamados/management/commands/disparar_perguntas_competencia.py:112`, `app/chamados/services/fabrica_da_celula.py:213`, `app/chamados/services/fabrica_da_celula.py:112`, `app/chamados/services/fabrica_da_celula.py:38`) |
| **Mesmo comando — fio mudo** | Bypassa `perguntar_pela_celula`: chama diretamente os helpers `vencidos_no_instante` e `motivos_pela_ata`; sem motivo, pula. O escritor posterior chama `perguntar` diretamente. Portanto cala esse caso por delegação aos helpers, sem encaminhá-lo ao CADASTRO nesse ramo. (`app/chamados/management/commands/disparar_perguntas_competencia.py:158`, `app/chamados/management/commands/disparar_perguntas_competencia.py:160`, `app/chamados/management/commands/disparar_perguntas_competencia.py:282`) |
| **`reconciliador` — consumidor de eventos** | Delega a `perguntar_por_eventos`, que relê as células e chama `perguntar_pela_celula`. (`app/chamados/reconciliador.py:1386`, `app/chamados/reconciliador.py:1387`, `app/chamados/services/fabrica_da_celula.py:270`, `app/chamados/services/fabrica_da_celula.py:272`) |
| **`reconciliador.veredito_orfa`** | Usa `dia_encerrado_motivo`. Sem previsto, não afirma encerramento e devolve `None`; isso conserva a pergunta, sem significar entrega ao DP. (`app/chamados/reconciliador.py:1090`, `app/chamados/reconciliador.py:1097`, `app/chamados/reconciliador.py:1104`, `app/ponto/turnos.py:2123`) |
| **`reconciliador.lavrar_sem_turno_aberto`** | Trata ausência de turno **aberto**; ainda exige que `pergunta_viva` seja falsa e carimba pela porta. Não consulta a pauta CADASTRO. (`app/chamados/reconciliador.py:371`, `app/chamados/reconciliador.py:397`, `app/chamados/reconciliador.py:405`) |
| **`fio`** | A falta de previsão deixa `esperada_prevista=''` para apresentação. A disponibilidade de ação lê o predicado de `raiox`; esses ramos não comprovam entrega ao CADASTRO. (`app/chamados/services/fio.py:325`, `app/chamados/services/fio.py:331`, `app/chamados/services/fio.py:279`, `app/chamados/services/fio.py:283`) |
| **`signals`** | Na volta a VIVOS emite evento da célula; na mudança do chamado também chama o julgamento do cartório. O cartório chama `perguntar_pela_celula`. (`app/chamados/signals.py:201`, `app/chamados/signals.py:207`, `app/chamados/signals.py:216`, `app/ponto/services/cartorio.py:1008`) |

**`disputa_emissao` tem decisões próprias de emissão, sem essa guarda CADASTRO.** `_tem_base` aceita batidas como base; `_ata_do_dia` permite continuar sem célula quando não existe escala vigente; os agrupadores usam `_classificar_falta` e `_criar_pergunta_se_couber` diretamente. (`app/chamados/services/disputa_emissao.py:814`, `app/chamados/services/disputa_emissao.py:899`, `app/chamados/services/disputa_emissao.py:906`, `app/chamados/services/disputa_emissao.py:1197`, `app/chamados/services/disputa_emissao.py:1210`, `app/chamados/services/disputa_emissao.py:1232`)

Essa classificação pode devolver motivo usando as batidas mesmo sem sequência prevista. Portanto, o código lido não sustenta que **todo emissor** aplica “passado sem turno → CADASTRO”. Ocorrência concreta desse desvio com dados de produção: **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:802`, `app/escala/servico_jornada.py:444`, `app/escala/servico_jornada.py:446`, `app/escala/servico_jornada.py:469`)

**4. Chamadores, significado de `dia` e fechamento**

| Função | Todos os chamadores de produção encontrados |
|---|---|
| `_pautas_cadastro` | `app/chamados/services/fabrica_da_celula.py:297`, `app/chamados/services/fabrica_da_celula.py:301`, `app/chamados/services/fabrica_da_celula.py:315` |
| `pauta_cadastro_existente` | `app/chamados/services/fabrica_da_celula.py:337` |
| `contador_cadastro_sem_turno` | `app/chamados/management/commands/vigiar_fio_morto.py:179` |
| `pauta_cadastro_sem_turno` | `app/chamados/services/fabrica_da_celula.py:175` |

O único chamador de `pauta_cadastro_existente` passa **`cel.data`**. É a data da célula que ancora os marcos; o encadeamento pode avançar os marcos posteriores para o dia seguinte. Assim, `dia` não muda para a data civil da saída noturna. (`app/chamados/services/fabrica_da_celula.py:337`, `app/chamados/services/fabrica_da_celula.py:26`, `app/escala/servico_jornada.py:81`, `app/escala/servico_jornada.py:82`)

O fechamento é interpretado assim:

- **Viva:** `feito_em IS NULL` **e** `cancelado_em IS NULL`, pelo queryset do modelo. (`app/pautas/models.py:57`, `app/pautas/models.py:64`)
- **Entregue com `dia`:** qualquer cabeça viva basta; senão, percorre o histórico e aceita `timezone.localdate(cancelado_em or feito_em) >= dia`. Se ambos estiverem preenchidos, prioriza cancelamento; não calcula o maior instante. (`app/chamados/services/fabrica_da_celula.py:297`, `app/chamados/services/fabrica_da_celula.py:301`, `app/chamados/services/fabrica_da_celula.py:303`, `app/chamados/services/fabrica_da_celula.py:304`)
- **Sem `dia`:** apenas pauta viva responde verdadeiro. O chamador encontrado passa `dia` explicitamente. (`app/chamados/services/fabrica_da_celula.py:299`, `app/chamados/services/fabrica_da_celula.py:337`)
- **Contador:** não recebe dia e conta somente colaboradores distintos com pauta viva. Por isso, contador zero não implica que um dia antigo deixou de estar “entregue”. (`app/chamados/services/fabrica_da_celula.py:309`, `app/chamados/services/fabrica_da_celula.py:315`, `app/chamados/services/fabrica_da_celula.py:320`, `app/chamados/services/fabrica_da_celula.py:304`)

`hoje` e a data do fechamento usam `timezone.localdate`, sem fuso explícito nessas chamadas. A configuração base declara `America/Sao_Paulo` e `USE_TZ=True`; o fuso efetivamente ativo no processo de produção ficou **[nao verificado]**. (`app/chamados/services/fabrica_da_celula.py:161`, `app/chamados/services/fabrica_da_celula.py:304`, `app/config/settings/base.py:93`, `app/config/settings/base.py:95`)

Há uma diferença entre a explicação e a garantia operacional: o predicado considera **cancelamento** suficiente para entrega, embora cancelar seja ato do autor, e não exija `lido_em`. Logo, verdadeiro não comprova que alguém do DP leu ou resolveu o cadastro. (`app/chamados/services/fabrica_da_celula.py:303`, `app/chamados/services/fabrica_da_celula.py:304`, `app/pautas/services.py:288`, `app/pautas/services.py:294`)

**Número final medido: 1 sítio de produção responde semanticamente “este dia já está entregue ao DP como pendência CADASTRO?” — `pauta_cadastro_existente`.** Ele contém duas consultas, viva e histórica, sobre um único construtor específico do marcador. O censo mais amplo encontrou seis consultas concretas por colaborador, discriminadas acima. (`app/chamados/services/fabrica_da_celula.py:288`, `app/chamados/services/fabrica_da_celula.py:297`, `app/chamados/services/fabrica_da_celula.py:301`, `app/chamados/services/fabrica_da_celula.py:284`, `app/pautas/services.py:435`, `app/ponto/services/afastado_avisa.py:62`, `app/ponto/services/ausencia.py:1039`, `app/ponto/services/ausencia.py:1229`, `app/ponto/services/pautas_dos_deslogados.py:65`)

## 09/11 20:57 · sessao 6838b687ee95 · - · - · origem=Ronald (tmux)

Censo independente, somente leitura:

| Consulta concreta por âncora de colaborador | Filtro e finalidade |
|---|---|
| `app/chamados/services/fabrica_da_celula.py:284` | `ANCORA_COLAB`, DP, cabeça, marcador `PAUTA_SEM_TURNO`; o marcador é definido em `:276`. |
| `app/pautas/services.py:435` | `Q(ancora_tipo='colab', ...)` em `:436`, unido a âncoras de dia; lista geral de pautas do colaborador. |
| `app/ponto/services/afastado_avisa.py:62` | Literal `'colab'`, supervisão, cabeça, feita/cancelada nulas, prefixo `TEXTO[:40]` (`:63–65`). |
| `app/ponto/services/ausencia.py:1039` | Literal `'colab'`, DP, feita/cancelada nulas; marcador `[A4/SEM-DOCUMENTO]` definido em `:1038`. |
| `app/ponto/services/ausencia.py:1229` | Literal `'colab'`, DP, feita/cancelada nulas; marcador `[A1-FERIAS/AG#%s]` definido em `:1228`. |
| `app/ponto/services/pautas_dos_deslogados.py:65` | Alias `ANCORA='colab'` (`:28`), `DEPARTAMENTO='supervisao'` (`:27`), marcador `[APP-401]` (`:26`). |

São **6 sítios concretos de consulta por colaborador**; somente o primeiro filtra o marcador objeto do censo. Os demais possuem marcadores/finalidades diferentes ou listagem geral, conforme as linhas acima.

A consulta do marcador propaga-se para operações em `app/chamados/services/fabrica_da_celula.py:297` (viva por colaborador), `:301` (histórico por colaborador), `:315` (contador), `:319` (restrição por empresas), `:320` (contagem distinta).

Há duas consultas de âncora parametrizada: `app/pautas/services.py:336`, em `pendentes_por_ancora(tipo, ident)`, e `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:242`, em `_existente(Pauta, ancora_tipo, ...)`. Os três chamadores desta última passam `ANCORA_COMPETENCIA` em `:183`, `:202`, `:222`. Chamador de `pendentes_por_ancora` não encontrado na varredura textual do repositório; uso dinâmico: [nao verificado].

O endpoint genérico aceita texto/âncora do POST em `app/pautas/views.py:76–82`; portanto pode transportar conteúdo arbitrário, mas não contém referência ao marcador. A leitura genérica da aba entrega `p.texto` em `app/pautas/views.py:40`; a lista vem de `sv.listar_por_aba` em `:33`.

O template do colaborador exibe texto e estado em `app/templates/colaboradores/detalhe.html:118–119`, usando contexto `pautas_do_colab` em `:113–115`. Não localizei produtor desse contexto fora da definição do service `app/pautas/services.py:433`; funcionamento desse painel em produção: [nao verificado].

A AST independente percorreu todos os `.py` do repositório, excluindo diretórios `tests` e nomes `test_*.py`. Encontrou as seis consultas acima e cinco chamadas de escrita com âncora colab: fábrica `:339`, afastado `:74`, ausência `:1055` e `:1245`, deslogados `:77`. Somente a fábrica escreve o marcador alvo (`app/chamados/services/fabrica_da_celula.py:341–342`). Nenhum import com alias do modelo `Pauta` ou `ANCORA_COLAB` apareceu nessa varredura; resolução por reflexão/import dinâmico: [nao verificado].

Para o número final, sugiro distinguir **6 sítios físicos de consulta concreta por colaborador**, **1 construtor de queryset específico do marcador** (`app/chamados/services/fabrica_da_celula.py:279`) e **1 predicado que responde semanticamente “este dia já foi entregue ao DP?”** (`app/chamados/services/fabrica_da_celula.py:288`), composto por consulta viva e consulta histórica (`:297`, `:301`).

## 09/11 20:57 · sessao 72b02e642af7 · - · - · origem=Ronald (tmux)

Chamadores de produção encontrados por AST em todos os `*.py`, excluindo diretórios `tests`:

```text
app/chamados/management/commands/vigiar_fio_morto.py:179: contador_cadastro_sem_turno(empresas)
app/chamados/services/fabrica_da_celula.py:315: _pautas_cadastro()
app/chamados/services/fabrica_da_celula.py:337: pauta_cadastro_existente(cel.colaborador_id, cel.data)
app/chamados/services/fabrica_da_celula.py:175: pauta_cadastro_sem_turno(cel)
app/chamados/services/fabrica_da_celula.py:297: _pautas_cadastro()
app/chamados/services/fabrica_da_celula.py:301: _pautas_cadastro(vivas=False)
```

O script executado, sem escrita:

```python
import ast, pathlib
names = {
    'pauta_cadastro_existente', '_pautas_cadastro',
    'contador_cadastro_sem_turno', 'pauta_cadastro_sem_turno',
}
for p in sorted(pathlib.Path('.').rglob('*.py')):
    if 'tests' in p.parts:
        continue
    try:
        t = ast.parse(p.read_text())
    except (SyntaxError, UnicodeError):
        continue
    aliases = {
        a.asname or a.name: a.name
        for n in ast.walk(t) if isinstance(n, ast.ImportFrom)
        for a in n.names if a.name in names
    }
    for n in ast.walk(t):
        if not isinstance(n, ast.Call):
            continue
        f = n.func
        name = (
            aliases.get(f.id, f.id) if isinstance(f, ast.Name)
            else f.attr if isinstance(f, ast.Attribute) else ''
        )
        if name in names:
            print(f'{p}:{n.lineno}: {ast.unparse(n)}')
```

- `pauta_cadastro_existente` recebe `dia` somente de `cel.data`; seu único chamador é `pauta_cadastro_sem_turno`. Não há chamador de produção omitindo `dia` na saída acima. A definição permite omiti-lo e, nesse caso, consulta somente pautas vivas: `app/chamados/services/fabrica_da_celula.py:337`, `app/chamados/services/fabrica_da_celula.py:299`.
- O escritor recebe a célula de `perguntar_pela_celula`, depois da guarda `cel.data < hoje`, ativo, dia que não é sem trabalho, e `vencidos_no_instante(...) is None`: `app/chamados/services/fabrica_da_celula.py:172`, `app/chamados/services/fabrica_da_celula.py:175`.
- Portanto, o `dia` entregue à deduplicação é a **data da célula**, usada como âncora do turno; não é a data civil da saída de turno noturno. O teto recebe `cel.data`, e `marcos_turno` encadeia os marcos a partir desse dia, avançando ao dia seguinte quando necessário: `app/chamados/services/fabrica_da_celula.py:26`, `app/ponto/services/cartorio.py:841`, `app/escala/servico_jornada.py:62`, `app/escala/servico_jornada.py:81`.
- `hoje` padrão é `timezone.localdate(agora)`; a data do fechamento também é extraída com `timezone.localdate(fechou)`. A configuração base declara `America/Sao_Paulo` e `USE_TZ=True`; a função não fornece um fuso explícito a `localdate`: `app/chamados/services/fabrica_da_celula.py:160`, `app/chamados/services/fabrica_da_celula.py:304`, `app/config/settings/base.py:93`, `app/config/settings/base.py:95`.
- Viva significa **feito nulo e cancelado nulo**, pelo queryset do modelo. “Entregue” aceita viva independentemente de `dia`, ou histórica com `cancelado_em or feito_em` cuja data local seja `>= dia`; quando ambos existem, a expressão prioriza cancelamento. `lido_em` não entra nesse juízo: `app/pautas/models.py:57`, `app/pautas/models.py:64`, `app/chamados/services/fabrica_da_celula.py:297`, `app/chamados/services/fabrica_da_celula.py:303`.
- O contador tem significado diferente: conta colaboradores distintos com pauta **viva**, sem parâmetro `dia` e sem incluir fechadas que ainda respondem “entregue” para dias antigos. Logo, contador zero não implica ausência de entrega ao DP: `app/chamados/services/fabrica_da_celula.py:315`, `app/chamados/services/fabrica_da_celula.py:320`, `app/chamados/services/fabrica_da_celula.py:304`.
- O contador pode recortar colaboradores pelas empresas informadas; não altera a semântica de fechamento: `app/chamados/services/fabrica_da_celula.py:316`.
- O vigia lê o contador, imprime `VFM|CADASTRO_SEM_TURNO`, e grava número e esperado no carimbo. Esse contador não integra os conjuntos usados para disparar alarme: `app/chamados/management/commands/vigiar_fio_morto.py:179`, `app/chamados/management/commands/vigiar_fio_morto.py:202`, `app/chamados/management/commands/vigiar_fio_morto.py:98`, `app/chamados/management/commands/vigiar_fio_morto.py:229`, `app/chamados/management/commands/vigiar_fio_morto.py:83`.
- O relatório instrumental lê o carimbo e compara genericamente todas as chaves de `esperado`; assim pode mostrar `cadastro_sem_turno` como fora do esperado sem ler o marcador no banco. `gerar_estado` incorpora essa apresentação: `bin/fio_estado.py:104`, `bin/fio_estado.py:125`, `bin/fio_estado.py:191`, `bin/gerar_estado.py:260`.
- Tela e API exibem o texto por caminhos genéricos, sem interpretar esse marcador: listagem da aba DP em `app/pautas/services.py:304`, `app/pautas/services.py:322`; serialização em `app/pautas/views.py:40`; apresentação em `app/templates/core/_copiloto.html:405`; API mensageria com aba padrão DP em `app/api/views_mensageria.py:1324`, `app/api/views_mensageria.py:1329`, `app/api/views_mensageria.py:1334`.
- Existe template de detalhe do colaborador que mostra texto e estado, além do helper `pautas_do_colab` que consulta âncora literal `'colab'`. Não encontrei chamador Python desse helper; a alimentação efetiva dessa seção fica **[nao verificado]**: `app/templates/colaboradores/detalhe.html:118`, `app/templates/colaboradores/detalhe.html:119`, `app/pautas/services.py:433`, `app/pautas/services.py:436`.

## 09/11 20:56 · sessao 780379ccd126 · - · - · origem=Ronald (tmux)

Item 3 — decisões e delegações confirmadas:

| Sítio | Comportamento |
|---|---|
| `app/chamados/services/fabrica_da_celula.py:172` | A guarda explícita reúne passado, colaborador ativo, dia que não é sem trabalho; consulta `vencidos_no_instante` e, se `None`, chama `pauta_cadastro_sem_turno` e retorna sem perguntas (`:173-176`). |
| `app/chamados/management/commands/disparar_perguntas_competencia.py:158` | O caminho **fio mudo** chama diretamente `motivos_pela_ata(... vencidos=vencidos_no_instante(...))`; sem motivo, pula (`:160-161`). Não passa por `perguntar_pela_celula`, mas delega o significado de ausência de previsto aos helpers da fábrica: `app/chamados/services/fabrica_da_celula.py:27` e `:38`. |
| `app/chamados/management/commands/disparar_perguntas_competencia.py:271` | O escritor do fio mudo chama `fabrica_da_celula.perguntar` diretamente (`:282-283`); essa função cria/atualiza disputa e gera perguntas sem a guarda CADASTRO de `perguntar_pela_celula`: `app/chamados/services/fabrica_da_celula.py:141`. |
| `app/chamados/management/commands/disparar_perguntas_competencia.py:112` | O caminho **célula** é leitor delegado: chama `motivos_que_o_evento_deixou`; este chama `motivos_a_perguntar`, não escreve CADASTRO: `app/chamados/services/fabrica_da_celula.py:213`. |
| `app/chamados/reconciliador.py:1386` | O consumidor delega a `perguntar_por_eventos`, que relê as células atuais e chama `perguntar_pela_celula`: `app/chamados/services/fabrica_da_celula.py:270`. |
| `app/chamados/reconciliador.py:1090` | `veredito_orfa` consulta `dia_encerrado_motivo`; sem previsto/turno aberto sem escala, retorna `None`, mantendo a pergunta viva, sem enviá-la à Pauta (`:1097-1104`). É decisão diferente de “já entregue ao DP”. |
| `app/chamados/reconciliador.py:371` | `lavrar_sem_turno_aberto` consulta ausência de **turno aberto**, depois exige que `pergunta_viva` seja falsa (`:397-399`) e carimba pela porta (`:405`). Não confundir turno aberto com turno previsto; não é encaminhamento CADASTRO. |
| `app/chamados/signals.py:201` | A volta de chamado a VIVOS emite evento de célula (`:204-207`); a mudança de chamado também julga a célula pelo cartório (`:208-216`). São delegações, sem decisão própria sobre o marcador. |
| `app/chamados/services/fio.py:325` | A ausência de marco previsto aqui apenas deixa `esperada_prevista=''` para apresentação (`:327-333`); não é prova de encaminhamento CADASTRO. |
| `app/chamados/services/fio.py:1306` | O veto de competência do acervo delega a `competencia_do_dia_entregue` (`:1313-1316`); é outro significado de “entregue”, sem relação com a Pauta de falta de turno. |

Atenção ao caminho antigo de `disputa_emissao`: ele não implementa uma regra geral “passado sem previsto → CADASTRO”. `_tem_base` considera batida suficiente (`app/chamados/services/disputa_emissao.py:813`); `_ata_do_dia` permite continuar sem célula quando não há escala vigente (`:899-907`); os laços usam `_classificar_falta` e `_criar_pergunta_se_couber` diretamente (`:1197-1216`, `:1232-1247`). `_classificar_falta` delega ao serviço de jornada (`:802-803`), cujo fallback sem marcos previstos pode devolver motivo de pergunta (`app/escala/servico_jornada.py:438`, `:469-471`). Ocorrência efetiva dessa combinação em produção: [nao verificado].

Não encontrei nesses cinco módulos outra escrita do marcador nem outra consulta que responda “já existe Pauta CADASTRO deste dia”; a evidência positiva da decisão de envio permanece `app/chamados/services/fabrica_da_celula.py:174`. Exaustividade dessa negativa por AST deve ser consolidada pelo agente principal [nao verificado].

## 09/11 20:04 · sessao 2390bdb8a8f2 · FIO-E0-V4-E4-ESCRITORES · censo · origem=Code

Nos anexos, numerei as linhas desde a primeira linha de cada conteúdo, incluindo linhas vazias. Classifico o **achado específico**, distinguindo-o de outros problemas na mesma função.

| achado | classe | trecho .q | trecho .r | linha da triagem | 1 linha de motivo |
|---|---|---|---|---|---|
| **P1 — consulta desnecessária e justificativa C8** | **(e) NÃO PERGUNTADO** | “cada escritor tocado, chamado 2x, produz o mesmo estado, 1 linha de trilha e 0 duplicata?” (`preverif_e4a1.q:41`) | “dedup conserva a linha e só salva reuso com alterações” (`preverif_e4a1.r:162`) | F2 discute “re-emite EventoCelula” e “outbox at-least-once”, sem tratar da consulta de captura (`triagem_preverif_e4a1.txt:24–26`). | Faltou perguntar sobre custo SQL do reuso **sem alteração** e validade da justificativa C8; idempotência de estado não verifica consulta dispensável (`preverif_e4a1.q:41`; `app/chamados/models.py:2958,2993–3003`). |
| **P2 — ramo por `perguntar` continua sem `hoje`** | **(a) tema presente; ramo específico não visto** | “D9: o C6 […] le date.today() / relogio no caminho que este diff toca?” (`preverif_e4a1.q:14–15`) | “Há também o caminho fábrica → desfazer carimbo → `pergunta_viva` → janela, igualmente sem instante injetado” (`preverif_e4a1.r:33`) | “PROCEDENTE […] -> CURADO”; descreve a passagem direta de `hoje`, sem identificar o segundo ramo (`triagem_preverif_e4a1.txt:8–12`). | A resposta viu D9, mas não enumerou o caminho por `perguntar`; a triagem aceitou e curou parcialmente, portanto **não houve descarte desse achado** (`app/chamados/services/fabrica_da_celula.py:140–142,178,188`). |
| **P3 — ausência de contador/vigia** | **(d) tema abordado de forma insuficiente; nenhum dos dois viu** | “dia passado sem marcos previstos NAO gera pergunta por data. Algum caminho […] ainda gera pergunta para esse dia?” (`preverif_e4a1.q:36–37`) | “Pauta sem turno […] **CONFORME:** […] inclui pautas já feitas/canceladas” (`preverif_e4a1.r:165`) | E3 descarta outro problema: “ha batida de entrada, nao e o 'dia passado sem nada'” (`triagem_preverif_e4a1.txt:17–21`). | PASSADO-SEM-MARCOS foi perguntado pelo lado da emissão proibida; faltou explicitar a obrigação positiva do contador e o ciclo após fechar/cancelar a Pauta (`app/docs/PROMPTS.md:2004`; `app/chamados/services/fabrica_da_celula.py:277–304`). |
| **P4 — RED sobrescrito pelo GREEN** | **(c) fora do alcance: evidência de execução posterior** | “selo/RED sem mordida […] algum teste passa com o codigo produtivo revertido” (`preverif_e4a1.q:28–29`) | “CONFORME para os sete mutantes declarados” (`preverif_e4a1.r:107`) | A cura posterior anuncia “RED/GREEN: ponto/tests/test_porta_celula_dia.py” (`triagem_preverif_e4a1.txt:3–7`). | O achado é a perda do registro RED da cura posterior, relatada no anexo; não é o defeito de reversão que o Codex identificou (`opus_final_e4a1_1.txt:24–26`; `triagem_preverif_e4a1.txt:3–7`). |
| **P5 — “o caso comum”** | **(c) fora do alcance temporal da leitura** | “Copia lida: o commit 6a1fc54f7”; “julgue o codigo contra os avais e leis anexos” (`preverif_e4a1.q:2–4`) | “A porta física da sequência continua sendo `registrar`” (`preverif_e4a1.r:54`) | F1 descarta duplicação por replay, não vocabulário (`triagem_preverif_e4a1.txt:22–23`); as curas seguem “sem 2a volta Codex” (`:27`). | A frase foi acrescentada na cura posterior de `registrar([])`; ela não existia no corpo lido em 6a, portanto não cabe atribuir sua ausência àquela resposta (`6a1fc54f7/app/chamados/models.py:3939–3948`; `app/chamados/models.py:3943–3946`). |

A comparação temporal dos snapshots disponíveis mostra:

- **P1:** a consulta já estava em **6a1fc54f7**, exatamente antes de `upd = []`; permaneceu igual em 629f2d244. Portanto, **não nasceu nas curas posteriores**. O aumento dos tetos e a justificativa C8 são posteriores (`6a1fc54f7/app/chamados/models.py:2958–2959`; [models.py:2958](/home/ronald/codex_ws/629f2d244fa0f0c527e56a2697b4c633b263b729/app/chamados/models.py:2958); `6a1fc54f7/app/ponto/tests/test_selo_chokepoint_escrita.py:180,187`; `app/ponto/tests/test_selo_chokepoint_escrita.py:180,187`). Sua introdução especificamente no diff **f2962ab8d..6a1fc54f7**, em vez de já existir na base, permanece **[nao verificado]**: a base e o patch não estavam disponíveis nesta consulta.
- **P2:** o ramo `perguntar_pela_celula → perguntar → desfazer_carimbo_sem_lastro` já existia sem propagação de `hoje`; a cura posterior acrescentou `hoje=hoje` somente à chamada direta. O furo permaneceu, enquanto a cura parcial e seu selo nasceram depois (`6a1fc54f7/app/chamados/services/fabrica_da_celula.py:128–137,180–182`; [fabrica_da_celula.py:178](/home/ronald/codex_ws/629f2d244fa0f0c527e56a2697b4c633b263b729/app/chamados/services/fabrica_da_celula.py:178); `app/chamados/services/fabrica_da_celula.py:188`; `app/chamados/tests/test_e4a_relogio_injetado.py:61–63`).
- **P4:** o bloco de rejulgamento e os testes correspondentes foram acrescentados depois de 6a; a sobrescrita do log é sustentada pelo dado raspado, sem confirmação independente de execução (`6a1fc54f7/app/ponto/portas/celula.py:807–809`; [celula.py:809](/home/ronald/codex_ws/629f2d244fa0f0c527e56a2697b4c633b263b729/app/ponto/portas/celula.py:809); `app/ponto/tests/test_porta_celula_dia.py:257–282`; `opus_final_e4a1_1.txt:24`). Há divergência documental: o `RELATO.md:8` desta cópia contém “E4a 17:09”, não a citação RED atribuída a ele pelo opus ([RELATO.md:8](/home/ronald/codex_ws/629f2d244fa0f0c527e56a2697b4c633b263b729/app/docs/RELATO.md:8); `opus_final_e4a1_1.txt:24`).

**Contagem da tabela: (a) 1; (b) 0; (c) 2; (d) 1; (e) 1. Total: 5.**

## 09/11 19:59 · sessao 81d39397ed64 · - · - · origem=Ronald (tmux)

P3: recomendo **(d)**. O tema PASSADO-SEM-MARCOS foi perguntado em E, e a resposta examinou a Pauta, seus escritores e a deduplicação; não examinou contador/vigia nem o ciclo após fechamento/cancelamento. O código confirma que a existência de qualquer Pauta, inclusive feita/cancelada, impede nova criação (`app/chamados/services/fabrica_da_celula.py:278`; `app/chamados/services/fabrica_da_celula.py:298`). A exigência “uma por colab, com contador” está registrada em `app/docs/PROMPTS.md:2004`.

Trechos relevantes dos anexos:

- `.q`, E: “dia passado sem marcos previstos NAO gera pergunta por data”.
- `.r`, C: “Deduplicação por colaborador, departamento, cabeça e prefixo”.
- `.r`, F: “Pauta sem turno | **CONFORME:** trava, relê existência e só então escreve; inclui pautas já feitas/canceladas”.
- Triagem E3: “DESCARTADO (fila)” refere-se a entradas órfãs com Batida real; **não corresponde à falta do contador**, portanto não sustenta classe (b). Numeração física dos anexos: [nao verificado].

P5: recomendo **(e)**. Faltou pedir fiscalização textual dos termos da LEI-AKITA 1 no diff; a referência genérica às leis não explicita essa conferência. A frase está em `app/chamados/models.py:3944`; a regra textual está em `CLAUDE.md:17–19`. A resposta e a triagem fornecidas não apresentam achado sobre essa frase; numeração física dessa ausência nos anexos: [nao verificado].

Evidências adicionais:

- SILENCIAR exige prazo, tripwire e item de fila: `CLAUDE.md:194`.
- Escopo literal do aval: `CLAUDE.md:37`.
- A Pauta é a única ação nesse ramo: `app/chamados/services/fabrica_da_celula.py:300–304`.
- Busca por `PAUTA_SEM_TURNO` encontrou somente fábrica e testes: `app/chamados/services/fabrica_da_celula.py:274`; `app/chamados/tests/test_e4_c20.py:88`; `app/chamados/tests/test_orfas_pelo_evento.py:288`.

## 09/11 19:59 · sessao f390a20b0f2a · - · - · origem=Ronald (tmux)

P2: o ramo defeituoso já existia na árvore da preverificação: `/home/ronald/codex_ws/6a1fc54f7d428f08aa5e5716ccd5427189718907/app/chamados/services/fabrica_da_celula.py:128–137,180–182`. A comparação dos snapshots mostra que a cura posterior acrescentou `hoje=hoje` apenas à chamada direta atual de `desfazer_carimbo_sem_lastro`, em `app/chamados/services/fabrica_da_celula.py:178`; o ramo por `perguntar` permanece sem propagação em `:133–142,188`.

O selo D9 atual evita esse ramo: `_coleta` retorna `[]` em `app/chamados/tests/test_e4a_relogio_injetado.py:61–63`, e a chamada só ocorre quando `ms` tem conteúdo, em `app/chamados/services/fabrica_da_celula.py:186–188`.

P4: todo o bloco de cura em `app/ponto/portas/celula.py:809–821` e os testes em `app/ponto/tests/test_porta_celula_dia.py:257–282` foram acrescentados após o snapshot 6a1fc54f7. A sobrescrita do RED depende do dado raspado; sua ocorrência não é demonstrável pelo código [nao verificado].

Há divergência documental: o `app/docs/RELATO.md:8` desta cópia contém o registro “E4a 17:09”, sem a citação RED indicada pelo opus; a citação atribuída a essa linha só consta no anexo [nao verificado].

Para a classificação, P4 cabe em **(c)**. P2 não cabe literalmente em **(b)**: a triagem declarou D9 “PROCEDENTE → CURADO”, e o descarte sobre `lavrar` refere-se a outro ramo (`triagem_preverif_e4a1.txt:8–16`). Para usar **(a)**, explicite que a omissão é o ramo `perguntar`, embora a resposta tenha identificado D9 genericamente (`preverif_e4a1.r`, A3/D9; localização numérica [nao verificado]).

## 09/11 18:30 · sessao 57a6d3dbea05 · FIO-E0-V4-E4-ESCRITORES · verificador · origem=Code

**PROCEDENTE — bloqueia**, pelos cinco achados consolidados ao final: relógio no C6, reversão sem relavra, emissão residual para passado sem turno e duas falhas de idempotência nos escritores tocados (`app/chamados/juizes.py:575`; `app/ponto/portas/celula.py:785`; `app/chamados/services/disputa_emissao.py:288`; `app/chamados/models.py:3945`; `app/chamados/services/fio.py:925`).

A comparação Git integral com `f2962ab8d` ficou **[nao verificado]**: esta cópia não contém `.git`. O julgamento abaixo usa o patch produtivo fornecido e os arquivos locais; execução com banco, concorrência e ocorrência dos contracasos em produção permanecem **[nao verificado]**.

**A1 — CONFORME: A03, A17 e A18 conservam a projeção.**

| Caso | Informação depois do patch | Escrita e leitura |
|---|---|---|
| A03, `reabrir_admin` | Continua nos campos da `DisputaSupervisao`: fechamento, resolução e contador de reaberturas. Não recebeu DP próprio. | Escrita em `app/chamados/models.py:2149–2161`; chamada e leitura do contador em `app/chamados/services/acoes_disputa.py:255–259,298`. |
| A17, pendência técnica | Continua em `pendencia_admin` e `recusa_motivo`. | Escrita em `app/chamados/services/materializacao.py:850–851`; leitura em `app/chamados/services/explicador.py:85–90` e `app/templates/chamados/partials/modal_fio.html:221–225`. |
| A18, recusa técnica | Continua em `recusa_motivo`; colisão também pode receber carimbo de identidade. | Escrita em `app/chamados/services/materializacao.py:1099–1107`; leitura em `app/templates/chamados/partials/_card_atendimento.html:186` e `app/chamados/services/explicador.py:85–90`. |

O catálogo de atos não fornece substitutos próprios para esses três casos; seus leitores continuam dependentes da projeção (`app/chamados/models.py:3853–3863`). Não encontrei perda dessas informações causada pelos hunks fornecidos; diff integral **[nao verificado]**.

**A2 / D8 — CONFORME quanto à preservação da visibilidade existente.**

As telas e os leitores de apresentação continuam usando `recusa_motivo`:

- **Modal administrativo:** leitura direta sob resposta/pendência, incluindo advertência (`app/templates/chamados/partials/modal_fio.html:216–225`).
- **Atendimento do colaborador:** cartão que apresenta o motivo após as alternativas por `via_resolucao`; incluído nas duas listas do atendimento (`app/templates/chamados/partials/_card_atendimento.html:176–186`; `app/templates/chamados/meu_atendimento.html:127,144`).
- **Explicação do fio:** lê pendência e motivo, é montada pelo fio e renderizada pelo modal (`app/chamados/services/explicador.py:85–90`; `app/chamados/services/fio.py:302–314`; `app/templates/chamados/partials/modal_fio.html:433–442`).

A preservação é relativa ao comportamento anterior: a exibição direta no modal tem condicionais, e o cartão dá preferência às vias. Portanto, “toda recusa aparece incondicionalmente” não está demonstrado (`app/templates/chamados/partials/modal_fio.html:221–225`; `app/templates/chamados/partials/_card_atendimento.html:176–186`).

Outros leitores conservados: validação/devolução, reversão do passivo, auditoria e interpretação de identidade de batida (`app/chamados/services/validacao.py:147,188–208`; `app/chamados/services/passivo_validado.py:66`; `app/chamados/management/commands/auditar_invariantes_chamados.py:536,1074–1075,1111–1112`; `app/ponto/services/flip_auto.py:132`).

**A3 / D9 — PROCEDENTE, bloqueia: o instante não chega ao C6.**

O caminho tocado é:

`reapontar_resolvedora` → `lavrar` → `_pergunta_materializada_de_fato` → `_dentro_da_janela_viva` → `date.today()` (`app/chamados/services/fio.py:926`; `app/chamados/veredito_lavrado.py:262–264`; `app/chamados/juizes.py:674,764,575`).

Há também o caminho fábrica → desfazer carimbo → `pergunta_viva` → janela, igualmente sem instante injetado (`app/chamados/services/fabrica_da_celula.py:172`; `app/chamados/services/relavra_via.py:242`; `app/chamados/juizes.py:1387`).

`lavrar` não aceita `agora` e consulta `timezone.now()` **depois** do juiz; esse instante apenas carimba o resultado já calculado (`app/chamados/veredito_lavrado.py:254,262–273`).

**Correção mínima:** propagar o instante da borda pelos caminhos tocados até o juiz da janela, derivando dele o dia local, conforme a conversão já existente em `_hoje` (`app/chamados/veredito_lavrado.py:95–104`).

**B — CONFORME no patch produtivo fornecido: não há migração de leitores da projeção para DP.**

O nascimento acrescenta atos preservando os campos recebidos; ressuscitação e reapontamento acrescentam acordares. A preservação de R/V continua lendo a própria projeção (`app/chamados/models.py:3010–3015,1563–1568,3910–3936`; `app/chamados/services/fio.py:924–929`).

O leitor fora do Python permanece:

- **Antes/depois:** modal lê `data_conferida_em` e `data_conferida_por`, exibindo nome ou username (`app/templates/chamados/partials/modal_fio.html:169–170`).
- **Escritor mantido:** `conferir_data_do_fato` grava ambos os campos (`app/chamados/services/validacao.py:664–666`).

A retirada do fallback de classificação na fábrica está em `motivos_pela_ata`; sua conformidade com PASSADO-SEM-MARCOS é julgada no item E (`app/chamados/services/fabrica_da_celula.py:32–46`).

O PRONTO integral continua pendente: o placar puro retornou **19/20**, e o cron permanece em `JUIZES_POR_VARREDURA`. Isso é compatível com a divisão E4a-1/E4a-2 informada, mas não comprova entrega de E4a completa (`app/core/contratos_estruturais.py:261–270,1063`; `app/config/crons.py:2044–2048`).

**C — Escritores, leitores e significado dos campos.**

A porta física da sequência continua sendo `registrar`; seus campos e restrições estão no modelo (`app/chamados/models.py:3835–3897,3939–3948`).

| Ato DP | Quem solicita a escrita |
|---|---|
| Resposta | `app/chamados/services/disputa_emissao.py:1508–1509,1653–1654,1674–1675` |
| Validação | `app/chamados/services/validacao.py:139–141` |
| Devolução | `app/chamados/services/validacao.py:205–208` |
| Reabertura / recusa humana | `app/chamados/services/disputa_emissao.py:1612–1625`; `app/chamados/services/validacao.py:593–594` |
| Silêncio | `app/chamados/models.py:1924–1926`; `app/ponto/services/veredito_celula.py:194–196`; legado em `app/chamados/management/commands/backfill_decisoes_pergunta.py:324–326` |
| Retratação | `app/chamados/models.py:2056–2060` |
| Absorção | `app/chamados/services/materializacao.py:563–566` |
| Preservação de R/V sem ato correspondente | `app/chamados/models.py:3910–3936`; usada nos hunks em `app/chamados/models.py:1563–1564,3015` |

Os leitores da sequência são o histórico, seu carregamento pelo fio, a tarefa do dia e o backfill (`app/chamados/models.py:3504–3512`; `app/chamados/services/fio.py:231–232`; `app/chamados/services/tarefa_do_dia.py:337–347`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:99–101,315`).

| Projeção / valor | Escritores persistentes | Leitores relevantes |
|---|---|---|
| R: `resposta_colab`, `respondida_em` | Resposta, automática e trilha em `app/chamados/services/disputa_emissao.py:1464–1508,1649–1654,1671–1675`; limpeza em `:1627–1629`; complemento do carimbo em `app/chamados/models.py:3127–3129`; ressuscitação em `:1566`; nascimento em `:3010–3015`. | Estado/C6 em `app/chamados/models.py:3037–3044` e `app/chamados/juizes.py:1339–1351`; materializador em `app/chamados/services/materializacao.py:432,468`; histórico em `app/chamados/services/historico.py:98–109`; preservação DP em `app/chamados/models.py:3920–3930`. |
| V: `validada_em/por` | Validação/devolução em `app/chamados/services/validacao.py:134–141,199–208`; resposta nova limpa em `app/chamados/services/disputa_emissao.py:1477–1479`; reabertura em `:1627–1629`; carimbo com `resposta='validar'` em `app/chamados/models.py:3130–3133`; ressuscitação em `:1566`. | C6 em `app/chamados/juizes.py:1323`; fila em `app/chamados/models.py:3344–3355`; procedência em `app/ponto/services/procedencia_batida.py:71–74`; preservação DP em `app/chamados/models.py:3931–3935`. |
| `materializada_em/por`, `via_resolucao` | Atribuição do carimbo em `app/chamados/models.py:3093–3097`; portas em `:3099,3164,3199`; lote em `:3260–3262`; troca de via em `:3271–3272` e `app/chamados/services/relavra_via.py:125–128`; nascimento em `app/chamados/models.py:3010–3013`. | C6 em `app/chamados/juizes.py:660–687,1334–1404`; reconciliação em `app/chamados/reconciliador.py:366,1049,1504`; materialização em `app/chamados/services/materializacao.py:434–438,625–632,1093`; apresentação/cache em `app/chamados/services/raiox.py:175–187`; procedência em `app/ponto/services/procedencia_batida.py:71–74`. |
| `pendencia_admin`, `recusa_motivo` | `app/chamados/services/materializacao.py:850–851,1099–1103`; `app/chamados/services/validacao.py:584–590`; escape em `app/chamados/services/disputa_emissao.py:1494–1505`; identidade em `app/chamados/models.py:3183–3185`, chamada pela duplicata em `app/chamados/services/fio.py:1355`. | Telas e leitores discriminados em A2; interpretação textual adicional em `app/ponto/services/flip_auto.py:132`. |
| `data_conferida_em/por` | `app/chamados/services/validacao.py:664–666`. | Gate em `app/chamados/services/validacao.py:662`; exibição em `app/templates/chamados/partials/modal_fio.html:169–170`. |
| `veredito_lavrado`, `julgado_em`, `janela_expira_em` | Lavra em `app/chamados/veredito_lavrado.py:254–275`; gravação em lote em `:240–250`; restauração em memória em `:284–289`. Os hunks salvam a lavra em `app/chamados/models.py:3000–3002` e `app/chamados/services/fio.py:926–928`. | Validade em `app/chamados/veredito_lavrado.py:107–116,213–234`; tarefa em `app/chamados/services/tarefa_do_dia.py:349–352`; raio X em `app/chamados/services/raiox.py:34–35,135–136,184–185`. |
| Identidade: colaborador, disputa, destino, célula e dia | Nascimento/reuso em `app/chamados/models.py:2845–2896,2958–3002,3010–3013`; reapontamento/rechaveamento em `app/chamados/services/fio.py:879,925–929,1038`. | Domínio do fato em `app/chamados/models.py:2697–2755`; C6 em `app/chamados/juizes.py:1336–1392`; coleta do evento em `app/chamados/models.py:3221–3244`. |
| DNA e `trabalha` | Produtor `app/ponto/management/commands/gerar_celulas.py:73–104`; gravações da porta em `app/ponto/portas/celula.py:243–244,259–273,600–604,786–795,850–871`. | Marcos em `app/escala/models.py:1117–1133`; teto em `app/ponto/services/cartorio.py:831–859`; C6 em `app/chamados/juizes.py:634–642,1369–1392`; fábrica em `app/chamados/services/fabrica_da_celula.py:87–107`. |
| `acordar_celulas` / `EventoCelula` | Coleta e emissão em `app/chamados/models.py:3221–3244`; INSERT único em `app/ponto/portas/celula.py:883–892`; novos chamadores em `app/chamados/models.py:1568,3003,3946`, `app/chamados/services/fio.py:929`, `app/chamados/signals.py:207` e `app/ponto/portas/celula.py:796`. | Consumidor em `app/chamados/reconciliador.py:1343–1401`; célula atual por colaborador/data em `app/chamados/services/fabrica_da_celula.py:260–264`; semeadura em `app/chamados/reconciliador.py:1289–1311`. |
| Pauta de cadastro sem turno | Porta em `app/chamados/services/fabrica_da_celula.py:279–298`, delegando a `app/pautas/services.py:153–186`. | Deduplicação por colaborador, departamento, cabeça e prefixo em `app/chamados/services/fabrica_da_celula.py:271–276,292–293`. |

Outros leitores de R/V/C encontrados na cópia, inclusive fora dos hunks:

- **API e telas:** `app/api/views.py:1188–1202,2584–2600,2643,2660`; `app/api/views_mensageria.py:567–570`; `app/templates/chamados/partials/_pergunta_card.html:20–42`; `app/templates/chamados/partials/_card_atendimento.html:94–100,135–186,384–385`; `app/templates/chamados/partials/_lote_preview.html:24`.
- **Apresentação e indicadores:** `app/chamados/views.py:1280,1416`; `app/chamados/views_cobrar.py:169,176,291`; `app/chamados/services/painel.py:67,262–273`; `app/chamados/services/verbos.py:240–246,311–321`; `app/chamados/templatetags/chamados_tags.py:128`; `app/chamados/relatorios_disputa.py:21`.
- **Fluxos auxiliares:** `app/chamados/canal.py:65`; `app/chamados/services/cobranca.py:229`; `app/chamados/services/furo_sem_canal.py:81`; `app/chamados/services/vigia_carimbo.py:98`; `app/chamados/services/vigia_fio.py:675,783`; `app/chamados/services/pergunta_no_app.py:70`; `app/chamados/services/declaracao_texto.py:227,417–419,687`; `app/chamados/services/becos.py:30`; `app/chamados/services/acoes_disputa.py:95,130,345,353`.
- **Outros domínios e comandos:** `app/ponto/supra_juiz.py:545,548`; `app/ponto/services/cartorio.py:1601`; `app/ponto/services/dia_do_colab.py:70`; `app/colaboradores/services/calendario.py:474`; `app/colaboradores/services/ficha.py:171`; `app/folha/previa.py:142`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1572–1575,1602–1603,1655,1766,1778`; `app/ponto/management/commands/reconciliar_grade.py:461`.

A exaustividade diante de acessos dinâmicos adicionais permanece **[nao verificado]**.

**C — Semântica: PROCEDENTE nos desacordos temporal e de época já identificados.**

- DP ordena por `ato_em, pk`; a tarefa considera o ato existente inclusivamente em `ato_em <= agora`. Resposta e validação preservam o instante da projeção (`app/chamados/models.py:3885,3927–3935`; `app/chamados/services/tarefa_do_dia.py:337–347`).
- A célula representa **dia do turno**; a batida resolve esse dia pelo pareamento. Conversa conserva a exceção explícita de dia civil (`app/ponto/turnos.py:2927–2943`; `app/chamados/services/fio.py:1612,1626–1627`).
- O teto vence inclusivamente quando `marco + 30 minutos <= agora`; o evento registra a data da célula e o instante automático de criação (`app/ponto/services/cartorio.py:828,858–859`; `app/ponto/portas/celula.py:892`; `app/escala/models.py:1857–1863`).
- A configuração usa São Paulo e datas conscientes de fuso, mas o C6 ainda usa a data do processo. A equivalência do fuso efetivo do processo em produção está **[nao verificado]** (`app/config/settings/base.py:93–95`; `app/chamados/juizes.py:575`).
- Na reversão, DNA atual e ata/veredito anteriores podem ser lidos juntos: desacordo comprovado pelo código, detalhado em E (`app/ponto/portas/celula.py:786–796`; `app/chamados/juizes.py:1369–1385`).

**D — Classes que já reprovaram.**

| Classe | Julgamento |
|---|---|
| Segundo juiz duplicando autoridade | **CONFORME nos hunks:** fábrica delega ao C6 existente; o registro conserva sua autoridade (`app/chamados/services/fabrica_da_celula.py:172`; `app/chamados/services/relavra_via.py:242`; `app/core/juizes.py:278`). |
| Segundo escritor fora da porta | **CONFORME nos hunks:** DP continua no `registrar`; evento continua no emissor único. As portas da pergunta estão declaradas (`app/chamados/models.py:3945`; `app/ponto/portas/celula.py:892`; `app/core/portas.py:393–400`). |
| Relógio fora da borda | **PROCEDENTE:** D9 acima (`app/chamados/juizes.py:575`). |
| Derivação paralela de fonte única | **CONFORME quanto às autoridades reutilizadas; PROCEDENTE quanto ao caminho de emissão que escapa da guarda**, detalhado em E (`app/ponto/services/cartorio.py:831–859`; `app/chamados/services/disputa_emissao.py:288–294`). |
| Selo/RED sem mordida | **CONFORME para os sete mutantes declarados; cobertura insuficiente para afirmar correção do consumo de DNA** (`app/chamados/tests/e4_verificar.py:16–45`; `app/chamados/tests/test_e4_expand.py:284–293`). |
| Ordem de trava | **CONFORME nos novos pontos de aquisição:** pauta trava antes de selecionar/escrever; ressuscitação usa a ordem central. Prova concorrente global **[nao verificado]** (`app/chamados/services/fabrica_da_celula.py:289–294`; `app/chamados/models.py:1562,2629–2693`). |
| Pouso antes do pré-requisito | **CONFORME quanto à ausência de migração de leitores no patch; PRONTO integral não demonstrado**, com cron ainda escritor (`app/chamados/models.py:3010–3015`; `app/config/crons.py:2044–2048`). |
| Campo com dois significados | `recusa_motivo` já reúne conteúdos de donos diferentes, explicitamente declarados. O parser textual persiste; não foi introduzido pelo patch fornecido (`app/core/portas.py:385–392`; `app/ponto/services/flip_auto.py:132`). |

Nos testes sem banco, foram obtidos **29 verdes** e **RC=1 nos sete mutantes**. Eles leem e alteram em memória o fonte aplicado (`app/chamados/tests/e4_verificar.py:14–45,58–97`; `app/chamados/tests/e4_catracas.py:8–9`; `app/chamados/tests/e4_apoio.py:8–16`).

Retirando em memória os hunks executáveis fornecidos, **17 testes passam, 8 falham e 4 dão erro**. Os sobreviventes são oito testes de portas anteriores, sintaxe, um efeito canônico, seis controles E3 e uma borda E2; os doze testes de eventos/C20 detectam a retirada (`app/chamados/tests/test_e4_expand.py:127–245,254–370`; `app/chamados/tests/test_e4_efeitos.py:8–21`; `app/chamados/tests/e4_verificar.py:14,48–55`; `app/chamados/tests/test_e4_c20.py:20–174`). Reversão integral ao commit exato: **[nao verificado]**.

**E1 — PROCEDENTE, bloqueia: reversão emite evento, mas falta relavra anterior ao consumo.**

A reversão restaura DNA/trabalho e emite o evento no mesmo atomic; não atualiza ata/veredito. O consumidor segue para C6 e fábrica sem julgar previamente o DNA restaurado (`app/ponto/portas/celula.py:785–796`; `app/chamados/reconciliador.py:1364–1387`).

C6 pode retornar pelo veredito antigo antes de consultar a classificação do dia. A fábrica também pode recusar emissão pelo veredito antigo, embora o teto tenha sido calculado com o DNA atual (`app/chamados/juizes.py:1369–1392`; `app/chamados/services/fabrica_da_celula.py:87–107`; `app/ponto/services/cartorio.py:840–844`).

Contracaso executado em memória com corpos reais: restaurar DNA de trabalho sobre célula com `veredito='concorde'` conservou esse veredito, emitiu um evento e produziu C6=False; substituir o veredito por `furo` mudou C6=True. Ocorrência em produção **[nao verificado]** (`app/ponto/portas/celula.py:740–796`; `app/chamados/juizes.py:1322–1392`).

**Correção mínima:** no consumidor, julgar a célula restaurada pelo cartório antes dos leitores de C6/ata, reutilizando o juiz já chamado pela regeneração (`app/ponto/portas/celula.py:422–440`).

**E2 — CONFORME: falha ordinária do INSERT na volta a VIVOS.**

O savepoint está **dentro** de `engolir`: a exceção sai do atomic antes de ser absorvida. O observador incrementa contador e emite ERROR com contexto e causa (`app/chamados/signals.py:205–207`; `app/core/observ.py:73–91`).

Assim, esse caminho prevê perda do evento com rastro, preservando a transição externa. O teste modela justamente rollback, continuidade do cartório e registro da falha; comprovação PostgreSQL permanece **[nao verificado]** (`app/chamados/tests/test_e4_c20.py:117–174`).

A cobertura do novo acordar é limitada aos módulos de `MODULOS_DE_TURNO`; essa tupla não inclui `batida_ausente`. A abrangência de todos os renascimentos do FIO MUDO não está demonstrada (`app/chamados/signals.py:200–202`; `app/chamados/reconciliador.py:336–337`).

**E3 — PROCEDENTE, bloqueia: passado sem marcos ainda pode gerar pergunta.**

O caminho direto e o FIO MUDO do cron respeitam o novo significado de `None` (`app/chamados/services/fabrica_da_celula.py:27–41,164–168`; `app/chamados/management/commands/disparar_perguntas_competencia.py:157–161`).

Entretanto, o próprio caminho C20 alcança:

`perguntar` → `criar_ou_atualizar_disputa` → `perguntar_entradas_pendentes` → `classificar_falta` → `gerar_perguntas_disputa` → `abrir` (`app/chamados/services/fabrica_da_celula.py:138`; `app/chamados/services/disputa_emissao.py:682–685,282–294,443–452`).

O merge conserva `entradas_pendentes` anteriores; o classificador ainda devolve motivo sem marcos esperados. Portanto, o evento de um dia pode emitir pergunta para outro dia passado sem turno (`app/chamados/services/disputa_emissao.py:548–559`; `app/escala/servico_jornada.py:469–471`).

Também permanece o repasse por data, cujo gate admite ausência de escala e cuja porta auxiliar verifica ausência/dedup sem exigir turno previsto (`app/chamados/services/disputa_emissao.py:905–907,

[truncado em 20000 de 24374 caracteres]

## 09/11 18:27 · sessao bcec07067663 · - · - · origem=Ronald (tmux)

**E — Reversão de DNA: PROCEDENTE, bloqueia.** O evento é emitido no mesmo atomic da restauração, mas a restauração conserva ata/veredito anteriores (`app/ponto/portas/celula.py:785-796`). O consumidor chama C6 e fábrica sem relavrar a célula (`app/chamados/reconciliador.py:1364-1387`). C6 retorna pelo veredito antigo antes de consultar a classificação do dia (`app/chamados/juizes.py:1369-1392`). A fábrica combina teto do DNA atual com ata persistida (`app/ponto/services/cartorio.py:840-844`; `app/chamados/services/fabrica_da_celula.py:43-49`).

Contracaso executado somente em memória, por extração AST das funções reais: restaurar DNA de trabalho sobre célula antes em folga deixou `trabalha=True`, `veredito='concorde'`, um evento e C6=False; simular a lavra faltante com `veredito='furo'` mudou C6=True. Isso demonstra a dependência do veredito antigo; ocorrência em produção [nao verificado]. Fontes executadas: `app/ponto/portas/celula.py:740`; `app/chamados/juizes.py:1322`.

Correção mínima: o consumidor deve acionar o cartório para julgar o DNA restaurado antes das operações que leem C6/ata; reutilizar o juiz existente, como já faz o rabo compartilhado da regeneração (`app/ponto/portas/celula.py:422-440`).

**E — Falha do evento na volta a VIVOS: CONFORME no caminho ordinário.** O savepoint fica dentro de `engolir`, permitindo seu rollback antes de engolir a exceção (`app/chamados/signals.py:205-207`). A falha deixa contador e ERROR com contexto/causa (`app/core/observ.py:73-91`). Não há perda sem rastro nesse caminho; comprovação com transação PostgreSQL [nao verificado]. O teste existente modela a transação com stub (`app/chamados/tests/test_e4_c20.py:117-174`).

**E — PASSADO-SEM-MARCOS: PROCEDENTE, bloqueia.** O caminho principal da fábrica e o FIO MUDO do cron respeitam a nova guarda (`app/chamados/services/fabrica_da_celula.py:38-41`; `app/chamados/management/commands/disparar_perguntas_competencia.py:157-161`). Entretanto, o próprio C20 alcança um desvio:

`perguntar` → `criar_ou_atualizar_disputa` → `perguntar_entradas_pendentes` → `classificar_falta` → `gerar_perguntas_disputa` → `PerguntaDisputa.abrir` (`app/chamados/services/fabrica_da_celula.py:138`; `app/chamados/services/disputa_emissao.py:682-685`; `app/chamados/services/disputa_emissao.py:282-294`; `app/chamados/services/disputa_emissao.py:443-452`).

O contexto antigo pode conservar `entradas_pendentes` quando a fábrica atualiza apenas `data` (`app/chamados/services/disputa_emissao.py:548-559`). Sem escala ou marcos esperados, `classificar_falta` ainda retorna `orfao_14h` (`app/escala/servico_jornada.py:469-471`). Assim, evento de um dia com marcos pode emitir pergunta para outro dia passado sem turno previsto, contrariando `app/docs/PROMPTS.md:2004`.

Também permanece o repasse por data: `_classificar_falta` seguido de `_criar_pergunta_se_couber`; `_ata_do_dia` permite continuar sem escala (`app/chamados/services/disputa_emissao.py:1234-1244`; `app/chamados/services/disputa_emissao.py:905-907`). A porta auxiliar verifica ausência e dedup, sem exigir turno previsto (`app/chamados/services/disputa_emissao.py:827-842`).

Contracaso executado em memória: `classificar_falta` real, sem escala/marcos/batidas, retornou `('S','orfao_14h','SAIU')`; `perguntar_entradas_pendentes` real chamou a geração para o dia passado derivado da batida simulada. Ocorrência desse contexto em produção [nao verificado]. Fontes: `app/escala/servico_jornada.py:270`; `app/chamados/services/disputa_emissao.py:257`.

Correção mínima: aplicar a guarda compartilhada de passado sem turno nos caminhos de emissão datada, incluindo entradas pendentes e repasse, encaminhando à pauta de cadastro; preservar a classificação global usada por outros leitores (`app/chamados/services/fabrica_da_celula.py:164-168`; `app/chamados/services/fabrica_da_celula.py:279-298`).

**E — FABRICA-POR-EVENTO: cobertura parcial; zeragem [nao verificado].** Os emissores são cartório e consumidor (`app/ponto/services/cartorio.py:1006-1008`; `app/chamados/reconciliador.py:1386-1387`). O cron continua apenas contando esse universo, enquanto seu ramo FIO MUDO escreve (`app/chamados/management/commands/disparar_perguntas_competencia.py:109-113`; `app/chamados/management/commands/disparar_perguntas_competencia.py:270-295`). C20 examina somente datas presentes no lote de eventos; não recupera automaticamente todo estoque sem evento (`app/chamados/services/fabrica_da_celula.py:260-264`). A semeadura existente cobre chamados vivos e órfãs específicas, sem varrer todas as células sem pergunta (`app/chamados/reconciliador.py:1289-1311`). Portanto, o código não garante estancar a contagem; resultado no ar [nao verificado].

**F — Reapontamento: PROCEDENTE contra a exigência literal de mesmo estado 2×.** Não existe no-op para destino/dia efetivamente iguais: o caminho sempre lavra, salva e acorda a origem (`app/chamados/services/fio.py:866`; `app/chamados/services/fio.py:925-929`). A lavra sempre avança `julgado_em` (`app/chamados/veredito_lavrado.py:270-273`). Além disso, o save acorda a célula atual e a chamada explícita acorda a anterior, que podem coincidir (`app/chamados/signals.py:251-257`; `app/chamados/services/fio.py:929`). Duplicatas na fila são admitidas pelo desenho; não equivalem automaticamente a duplicação de efeito/DP (`app/escala/models.py:1848-1850`).

Correção mínima: retornar sem escrita quando destino e identidade efetivos não mudarem; emitir os acordares necessários por célula distinta no ato (`app/chamados/services/fio.py:924-929`; `app/chamados/models.py:3234-3244`).

**F — Reversão sequencial: CONFORME quanto ao estado e evento.** Limpar `dna_anterior` faz a segunda chamada retornar False e não emitir novamente (`app/ponto/portas/celula.py:777-778`; `app/ponto/portas/celula.py:787`). Confirmado no contracaso em memória: primeira=True, segunda=False, um evento. Concorrência e uma trilha efetivamente persistida [nao verificado]; a leitura antecede o atomic e não usa trava de linha (`app/ponto/portas/celula.py:776`; `app/ponto/portas/celula.py:785`).

**C — Célula/DNA e evento, mapa para consolidação.**

| Elemento | Escritores | Leitores / significado |
|---|---|---|
| DNA | Produtor `app/ponto/management/commands/gerar_celulas.py:73`; atribuições na porta em `app/ponto/portas/celula.py:243`, `:259`, `:272`, `:600`, `:786`, `:850` | Marcos congelados em `app/escala/models.py:1117`; teto em `app/ponto/services/cartorio.py:840`; C6 em `app/chamados/juizes.py:634` e `:1369` |
| `EventoCelula` | INSERT único `app/ponto/portas/celula.py:892`; acordares por pergunta `app/chamados/models.py:3227` | Consumidor `app/chamados/reconciliador.py:1315`; fábrica relê célula atual por colaborador/data em `app/chamados/services/fabrica_da_celula.py:262` |
| Dia | Célula declara dia do turno em `app/ponto/turnos.py:2939`; batida resolve pelo pareamento em `app/ponto/turnos.py:2927`; atribuição única da pergunta em `app/chamados/services/fio.py:790` | Conversa conserva exceção explícita de dia civil em `app/chamados/services/fio.py:1612` e `:1626`; evento grava a data da célula em `app/ponto/portas/celula.py:892` |
| Tempo | Instante do evento automático em `app/escala/models.py:1860`; consumidor captura agora em `app/chamados/reconciliador.py:1337` | Fábrica recebe esse instante e deriva dia local em `app/chamados/services/fabrica_da_celula.py:264`; teto por marco +30 min em `app/ponto/services/cartorio.py:828` |

Procedentes encontrados: reversão sem relavra anterior ao consumo; desvio de emissão para passado sem turno; reapontamento sem idempotência de estado (`app/ponto/portas/celula.py:785`; `app/chamados/services/disputa_emissao.py:288`; `app/chamados/services/fio.py:925`).

## 09/11 18:25 · sessao c723709f6c87 · - · - · origem=Ronald (tmux)

Complemento C — escritores R/V/C verificados por AST e leitura dos sítios, sem importar a aplicação:

| Campos | Escritores persistentes |
|---|---|
| `resposta_colab/respondida_em` | `app/chamados/services/disputa_emissao.py:1464-1465`, save `:1505`; resposta automática `:1649-1652`; pela trilha `:1671-1673`; reabertura limpa via dict `:1590` e update `:1627-1629`; nascimento recebe campos em `app/chamados/models.py:3008-3015`; `carimbar` completa resposta em `:3127-3129`; ressuscitação zera em `:1566`. |
| `validada_em/por` | `app/chamados/services/validacao.py:134-137`, devolução `:199-202`; resposta nova limpa em `app/chamados/services/disputa_emissao.py:1477-1479`; reabertura `:1590`, `:1627-1629`; `carimbar(resposta='validar')` em `app/chamados/models.py:3130-3133`; ressuscitação em `:1566`; nascimento `:3008-3015`. |
| `pendencia_admin` | `app/chamados/services/materializacao.py:850-851`. |
| `recusa_motivo` | `app/chamados/services/materializacao.py:850-851`, `:1102-1103`; via identidade em `app/chamados/models.py:3183-3185`, chamada pela colisão em `app/chamados/services/materializacao.py:1099-1100` e duplicata em `app/chamados/services/fio.py:1355`; recusa humana `app/chamados/services/validacao.py:584-590`; escape em `app/chamados/services/disputa_emissao.py:1494-1505`. |
| `data_conferida_em/por` | `app/chamados/services/validacao.py:664-666`. |

As atribuições em `app/chamados/models.py:1507-1508` pertencem a cópia de julgamento (`:1494`), e `:3159-3161` desfazem memória; não são escritores persistentes adicionais.

Leitores de produção agrupados:

- API: `app/api/views.py:1188-1190`, `:1202`, `:2584-2586`, `:2600`, `:2643`, `:2660`; mensageria `app/api/views_mensageria.py:567-570`.
- Templates: `app/templates/chamados/partials/modal_fio.html:169-170`, `:184`, `:216-227`; `_pergunta_card.html:20-32`, `:42`; `_card_atendimento.html:94-100`, `:135-155`, `:176-186`, `:384-385`; `_lote_preview.html:24`.
- Juiz/estado: `app/chamados/juizes.py:347`, `:827-828`, `:1300`, `:1323`, `:1339`, `:1447`, `:1592-1600`, `:1628`, `:1775`, `:1879-1901`, `:1949-1953`; `app/chamados/models.py:1843-1845`, `:3037-3044`, `:3344-3355`, `:3494`, `:3517-3539`, `:3920-3934`.
- Reconciliação/emissão: `app/chamados/reconciliador.py:364`, `:630`, `:1049`, `:1082`; `app/chamados/services/disputa_emissao.py:1467-1468`, `:1616-1618`, `:1644`, `:1667`, `:1710`, `:1853`, `:1914`, `:1926`, `:1931`, `:1943`.
- Materialização/validação: `app/chamados/services/materializacao.py:278`, `:356`, `:407`, `:432`, `:468`, `:627`, `:668-670`, `:829`, `:995`, `:1092`, `:1193`, `:1370-1371`; `app/chamados/services/validacao.py:139-147`, `:188-208`, `:236`, `:393-396`, `:573-582`, `:662`, `:732`, `:753`, `:851-866`, `:924`.
- Tela/fio: `app/chamados/views.py:1280`, `:1416`; `app/chamados/views_cobrar.py:169`, `:176`, `:291`; `app/chamados/services/fio.py:340`, `:660`, `:1227`, `:1239`; `app/chamados/services/explicador.py:32`, `:79-90`; `app/chamados/templatetags/chamados_tags.py:128`; `app/chamados/relatorios_disputa.py:21`.
- Histórico/indicadores: `app/chamados/services/historico.py:98-109`; `app/chamados/services/painel.py:67`, `:262-273`; `app/chamados/services/verbos.py:240-246`, `:311-321`; `app/chamados/veredito_lavrado.py:206-209`.
- Outros caminhos: `app/chamados/canal.py:65`; `app/chamados/services/cobranca.py:229`; `furo_sem_canal.py:81`; `vigia_carimbo.py:98`; `vigia_fio.py:675`, `:783`; `pergunta_no_app.py:70`; `declaracao_texto.py:227`, `:417-419`, `:687`; `becos.py:30`; `acoes_disputa.py:95`, `:130`, `:345`, `:353`; `raiox.py:138`; `passivo_validado.py:66`.
- Fora de chamados: `app/ponto/supra_juiz.py:545`, `:548`; `app/ponto/services/cartorio.py:1601`; `dia_do_colab.py:70`; `procedencia_batida.py:71-74`; `flip_auto.py:130-132`; `veredito_celula.py:196`; `app/colaboradores/services/calendario.py:474`; `ficha.py:171`; `app/folha/previa.py:142`.
- Comandos/sondas: `app/chamados/management/commands/auditar_invariantes_chamados.py:536`, `:1075-1076`, `:1105-1112`, `:1572-1575`, `:1602-1603`, `:1655`, `:1766`, `:1778`; `curar_perguntas_em_limbo.py:32`, `:46`, `:69`; `encerrar_perguntas_com_fato.py:32`; `validar_classe_a.py:80`; `reabrir_orfas_enterradas.py:68`; `backfill_decisoes_pergunta.py:56`, `:102`, `:326`; `dedup_perguntas_fato.py:75`; `app/ponto/management/commands/reconciliar_grade.py:461`; `bin/censo_folha.py:28-29`; `bin/sombra_regras_pessoais.py:85`.

Censo acima combina atributos AST, kwargs e leituras conferidas; exaustividade diante de acesso dinâmico adicional: [nao verificado].

`recusa_motivo` tem múltiplos conteúdos preexistentes e explicitamente declarados como donos diferentes em `app/core/portas.py:385-392`; além da apresentação, `app/ponto/services/flip_auto.py:132` interpreta estrutura textual para extrair uma batida. Esse risco é legado, sem alteração desses escritores/leitores no patch fornecido.

Para F: o reenvio da mesma resposta sempre muda `respondida_em` (`app/chamados/services/disputa_emissao.py:1459-1465`) e acrescenta DP (`:1508`); o próprio contrato declara essa não idempotência em `app/core/contratos_estruturais.py:563-564`. `DecisaoPergunta.registrar`, agora tocado, continua fazendo `bulk_create` incondicional (`app/chamados/models.py:3942-3945`), sem unicidade do ato nas constraints (`:3886-3897`).

## 09/11 18:25 · sessao 5d56fd863bda · - · - · origem=Ronald (tmux)

D9 — **PROCEDENTE bloqueante**. O caminho tocado continua lendo relógio dentro do juiz: `fio.reapontar_resolvedora` chama `_lavrar` (`app/chamados/services/fio.py:926`); `lavrar` chama `_pergunta_materializada_de_fato` (`app/chamados/veredito_lavrado.py:262`); este alcança `_dentro_da_janela_viva` (`app/chamados/juizes.py:674`, `app/chamados/juizes.py:764`), que executa `date.today()` (`app/chamados/juizes.py:575`). `lavrar` não recebe instante e só lê `timezone.now()` depois de consultar o juiz (`app/chamados/veredito_lavrado.py:254`, `app/chamados/veredito_lavrado.py:271`). Correção mínima: propagar `agora` desde a borda nesses caminhos, inclusive ao juiz da janela; conservar a semântica de dia local (`app/chamados/veredito_lavrado.py:98`).

Também há caminho pelo C6: a fábrica chama `desfazer_carimbo_sem_lastro` (`app/chamados/services/fabrica_da_celula.py:172`), que chama `pergunta_viva` (`app/chamados/services/relavra_via.py:242`); seu cálculo consulta a janela sem instante (`app/chamados/juizes.py:1387`).

D, selos/RED — **CONFORME quanto aos sete mutantes declarados**. Executei o runner sem escrita de bytecode: 29 testes verdes; os sete mutantes produziram RC=1. Eles alteram em memória o texto lido do código aplicado (`app/chamados/tests/e4_verificar.py:16`, `app/chamados/tests/e4_verificar.py:58`, `app/chamados/tests/e4_catracas.py:8`); os corpos são compilados desse texto (`app/chamados/tests/e4_apoio.py:8`). A reversão é descartada em memória (`app/chamados/tests/e4_verificar.py:69`).

Reversão em memória dos **hunks executáveis** dos seis arquivos do anexo: 29 testes, **17 passam, 8 falham, 4 dão erro**. As docstrings novas da fábrica foram mantidas; identidade integral com o commit f2962ab8d fica **[nao verificado]**, pois não há repositório Git nesta cópia. Os 17 sobreviventes são:

- Oito testes de portas fora dos hunks anexados, A07/A09/A10/A11/A12/A13-A08/A14/A15 (`app/chamados/tests/test_e4_expand.py:127`, `app/chamados/tests/test_e4_expand.py:223`).
- Um de sintaxe (`app/chamados/tests/test_e4_expand.py:241`).
- Um de estado canônico do chamado (`app/chamados/tests/test_e4_efeitos.py:8`).
- Seis controles E3/C17 incluídos no runner (`app/chamados/tests/e4_verificar.py:14`).
- Um controle E2 de bordas, que testa `esta_stale`, não a injeção no C6 (`app/chamados/tests/test_e2_tarefa_do_dia.py:1409`).

Os 12 testes novos de eventos/C20 detectaram a retirada dos hunks; portanto os 17 sobreviventes não demonstram, por si, selo novo sem mordida (`app/chamados/tests/test_e4_expand.py:254`, `app/chamados/tests/test_e4_c20.py:20`). Integração ORM, concorrência e falha real PostgreSQL: **[nao verificado]**; o próprio selo declara instrumentação sem banco (`app/chamados/tests/test_e4_expand.py:1`, `app/chamados/tests/test_e4_expand_orm.py:1`).

F, `DecisaoPergunta.registrar` — **PROCEDENTE pela exigência literal de replay da porta**. A porta faz `bulk_create` incondicional e emite evento em toda chamada (`app/chamados/models.py:3942`); não há identidade única do ato nas constraints, apenas checks de formato (`app/chamados/models.py:3884`). Executando o corpo extraído por AST duas vezes com a mesma linha e ORM instrumentado: duas linhas e dois eventos. O teste H2 cobre uma chamada de sucesso e outra de falha, sem replay (`app/chamados/tests/test_e4_expand.py:359`, `app/chamados/tests/test_e4_expand.py:368`). Correção mínima: identidade idempotente do ato, deduplicação sob trava/constraint correspondente, evento condicionado à criação. Evitar deduplicar indiscriminadamente texto: a sequência admite atos posteriores distintos (`app/chamados/models.py:3817`).

D, juiz/escritor paralelo — não identifiquei novo clone do C6 nos hunks: a fábrica delega a porta existente e esta consulta `pergunta_viva` (`app/chamados/services/fabrica_da_celula.py:172`, `app/chamados/services/relavra_via.py:242`); o registro mantém C6 na mesma autoridade (`app/core/juizes.py:278`). A escrita de DP continua na porta `registrar` (`app/chamados/models.py:3945`); censo completo dos demais escritores fica com a consolidação **[nao verificado]**.

PRONTO E4a integral — **não atingido**: módulo puro retorna `contratos_estruturais: 19/20 verdes`; a única célula não verde é chamado/juiz, dependente das exceções de varredura (`app/core/contratos_estruturais.py:261`, `app/core/contratos_estruturais.py:1063`). `disparar_perguntas_competencia` segue em `JUIZES_POR_VARREDURA` pelo fio mudo (`app/config/crons.py:2044`). Isso é compatível com a divisão E4a-1/E4a-2 fornecida no anexo; não deve ser apresentado como 20/20 (`app/core/contratos_estruturais.py:264`).

## 09/11 18:23 · sessao c723709f6c87 · - · - · origem=Ronald (tmux)

A1 — CONFORME: os três casos continuam na projeção; não foi criado fato equivalente nem migrado leitor no patch fornecido. A03 escreve `fechada_em/por`, `resolucao`, `motivo_fechamento`, `reaberturas_admin` e eventualmente `motivos` em `app/chamados/models.py:2149-2161`; a casca continua chamando essa porta em `app/chamados/services/acoes_disputa.py:255-259`, lê o contador em `:298`. A17 escreve conjuntamente `pendencia_admin` e `recusa_motivo` em `app/chamados/services/materializacao.py:850-851`; A18 escreve `recusa_motivo` em `:1102-1103`, ou pela porta de identidade em `:1099-1100` quando a colisão encerra por fato ocupado. O chamado recebe ocorrência em `:1105-1107`, mas isso não substitui a projeção como fonte dos leitores. O catálogo DP não contém atos próprios para esses três casos (`app/chamados/models.py:3853-3863`).

A2/D8 — CONFORME quanto à preservação antes/depois: os leitores continuam lendo a projeção. Leituras diretas de tela encontradas:

- Modal administrativo: `app/templates/chamados/partials/modal_fio.html:225`, sob `respondida_em` e `pendencia_admin` (`:216-221`); preserva advertência em `:222-225`.
- Atendimento do colaborador: `app/templates/chamados/partials/_card_atendimento.html:186`, com preferência anterior por `via_resolucao`; incluído por `app/templates/chamados/meu_atendimento.html:127` e `:144`.
- Explicador: `app/chamados/services/explicador.py:85-90` lê `pendencia_admin/recusa_motivo`; o fio o monta em `app/chamados/services/fio.py:302-314`. O modal renderiza seus itens em `app/templates/chamados/partials/modal_fio.html:433-442`.

Não afirmar visibilidade incondicional de toda recusa: o modal exige pendência para imprimir diretamente o campo (`app/templates/chamados/partials/modal_fio.html:221-225`), e o cartão só o mostra depois de esgotar as vias (`app/templates/chamados/partials/_card_atendimento.html:186`). Essas condições não foram alteradas pelo patch fornecido [nao verificado quanto ao diff completo ausente].

Outros leitores de `recusa_motivo`: `app/chamados/services/validacao.py:147`, `:188-208`; `app/chamados/services/passivo_validado.py:66`; `app/chamados/management/commands/auditar_invariantes_chamados.py:536`, `:1074-1075`, `:1111-1112`; `app/ponto/services/flip_auto.py:132` extrai identidade de batida pelo texto. Os campos de formulário em `modal_fio.html:60`, `:294` e `_pergunta_card.html:114` são entradas da recusa humana, não leituras da projeção.

B/modal — CONFORME: segue lendo `data_conferida_em/por`, nome completo ou username, em `app/templates/chamados/partials/modal_fio.html:169-170`; segue sendo escrito por `app/chamados/services/validacao.py:664-666`. O patch fornecido não altera esses sítios. A leitura do dia continua comparando a data da pergunta e a antiga do chamado (`app/templates/chamados/partials/modal_fio.html:161-167`).

C — sequência DP:

| Ato/campo | Escritor/chamador da porta | Leitor |
|---|---|---|
| Porta física DP | `app/chamados/models.py:3939-3948` | — |
| `resposta` | `app/chamados/services/disputa_emissao.py:1508-1509`, `:1653-1654`, `:1674-1675` | histórico `app/chamados/models.py:3504-3512`; tarefa `app/chamados/services/tarefa_do_dia.py:337-347` |
| `validacao` | `app/chamados/services/validacao.py:139-141` | tarefa `app/chamados/services/tarefa_do_dia.py:337-347` |
| `devolucao` | `app/chamados/services/validacao.py:205-208` | tarefa `app/chamados/services/tarefa_do_dia.py:337-347` |
| `reabertura/recusa` | `app/chamados/services/disputa_emissao.py:1612-1625`; recusa humana chama em `app/chamados/services/validacao.py:593-594` | histórico `app/chamados/models.py:3509-3512`; modal `app/templates/chamados/partials/modal_fio.html:204-211` |
| `silencio` | `app/chamados/models.py:1924-1926`; legado `app/chamados/management/commands/backfill_decisoes_pergunta.py:324-326` | tarefa `app/chamados/services/tarefa_do_dia.py:337-347`; seleção legado `app/chamados/management/commands/backfill_decisoes_pergunta.py:99-101`, `:315` |
| `retratacao` | `app/chamados/models.py:2056-2060` | tarefa `app/chamados/services/tarefa_do_dia.py:339-342` |
| `absorcao` | `app/chamados/services/materializacao.py:563-566` | carga com ausência `app/chamados/services/tarefa_do_dia.py:337-338` |
| Preservação R/V sem linha | `app/chamados/models.py:3910-3936`; nascimento `:3015`; ressuscitação `:1563-1564` | mesma função lê projeção; tarefa a usa em `app/chamados/services/tarefa_do_dia.py:343-347` |

O instante de resposta é o mesmo na projeção e DP (`app/chamados/services/disputa_emissao.py:1459-1465`, `:1508`); idem validação (`app/chamados/services/validacao.py:134-141`), silêncio (`app/chamados/models.py:1925`) e absorção (`app/chamados/services/materializacao.py:561-566`). A tarefa limita atos inclusivamente por `ato_em <= agora` (`app/chamados/services/tarefa_do_dia.py:339`); a ordem total DP é `ato_em, pk` (`app/chamados/models.py:3885`).

R/V/materialização mantêm leitura antiga: estado em `app/chamados/models.py:3037-3044`; carimbo físico único `_escrever_carimbo` em `:3093-3097`; `carimbar` em `:3120-3148`; volta avulsa em `:3200-3208`; volta em lote em `:3260-3262`; campos que acordam mantêm R/V/C em `:3216-3218`.

Observação útil sobre “campo com dois significados”: `recusa_motivo` já contém recusa técnica (`app/chamados/services/materializacao.py:1102`), advertência de tipo (`:850`), recusa humana (`app/chamados/services/validacao.py:584`), explicação de escape (`app/chamados/services/disputa_emissao.py:1494-1496`) e identidade da duplicata (`app/chamados/services/fio.py:1355`). O patch fornecido não cria nem altera esses usos; classificar como nova infração E4a seria incorreto [nao verificado quanto a exigência de sanar o legado].

Procedentes encontrados nesta subverificação: nenhum comprovado.

## 09/11 18:18 · sessao 87e4233cdf18 · FIO-E0-V4-E4-ESCRITORES · construtor · origem=Code

Entrega em [E4A_V3_ENTREGA.txt](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-180711/app/docs/E4A_V3_ENTREGA.txt:1), com parque integral, mapa de campos e comando ORM.

29 testes puros passaram; os sete mutantes morderam. [Resultados e comandos](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-180711/app/docs/E4A_V3_TESTES.txt:1). [nao verificado]

Não encontrei bypass no disparar; nenhuma cura produtiva foi necessária. [Análise](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-180711/app/docs/E4A_V3_ENTREGA.txt:90).

Resultados ORM e certificação final: [nao verificado].

## 09/11 18:11 · sessao 06071fff5ff7 · - · - · origem=Ronald (tmux)

Não encontrei caminho do `disparar` que ainda pergunte por DATA sem previsão fora da fábrica. Não recomendo alteração produtiva.

O universo célula lê `motivos_que_o_evento_deixou` em `chamados/management/commands/disparar_perguntas_competencia.py:112`, que passa por `motivos_a_perguntar` em `chamados/services/fabrica_da_celula.py:205`, calcula o teto em `:87` e entrega esse teto a `motivos_pela_ata` em `:107`. O universo FIO MUDO também passa explicitamente o teto em `chamados/management/commands/disparar_perguntas_competencia.py:158`. `vencidos=None` retorna `[]` em `chamados/services/fabrica_da_celula.py:38`. Só os motivos selecionados chegam ao escritor em `chamados/management/commands/disparar_perguntas_competencia.py:270`.

Ajustes identificados por leitura, sem executar ORM:

- `test_fabrica_pela_ata.py::FabricaPelaAta.test_05_sem_turno_previsto_vai_ao_cadastro_sem_pergunta` exige uma saída `CADASTRO-SEM-TURNO` ausente no comando HEAD; retirar essa exigência futura, conservando zero perguntas. `chamados/tests/test_fabrica_pela_ata.py:63`.
- `test_selo_emissor_fio_mudo.py::EmissorPedeDoChamadoMudoTest` tem seis casos afetados: `test_MORDE_o_emissor_ACHA_o_chamado_mudo`, `test_MORDE_desligado_e_barrado_e_NOMEADO`, `test_MORDE_competencia_exportada_VOLTA_a_ser_pedida`, `test_MORDE_pergunta_MORTA_do_MESMO_FATO_e_piso_F1t`, `test_MORDE_o_cron_leva_os_dois_caminhos_e_diz_por_porta`, `test_o_dry_nao_grava_nada`. As fixtures não têm vínculo nem DNA; o helper injeta ata, mas não previsão. `chamados/tests/test_selo_emissor_fio_mudo.py:36`, `:62`.
- No mesmo módulo, `FioMudoNaoSobreviveAUmCicloTest.test_MORDE_o_chamado_mudo_nao_sobrevive_a_um_ciclo` usa a mesma forma sem vínculo. `chamados/tests/test_selo_emissor_fio_mudo.py:256`, `:291`. Ajuste menor: dar previsão explícita no mock `marcos_vencidos` dentro de `_lei_e_ata`, acompanhado de comentário da LEI-PASSADO-SEM-MARCOS. Acrescentar vínculo antes de plantar o Chamado pode fazer o signal emitir antes do exercício; essa consequência precisa ser considerada. `chamados/signals.py:210`.
- `test_o261_julgamento_desfaz_carimbo.py::_Cenario._cenario` tem DNA, mas nenhum vínculo. Sem EC, `marcos_com_teto` retorna `[]`; a guarda nova manda a Pauta antes da coleta de carimbos. `chamados/tests/test_o261_julgamento_desfaz_carimbo.py:53`, `ponto/services/cartorio.py:856`, `chamados/services/fabrica_da_celula.py:164`.
- Quatro casos O261 inferidos vermelhos: `JulgamentoDesfazCarimboSemLastroTest.test_MORDE_o_julgamento_desfaz_o_carimbo_da_pergunta_viva`, `.test_julgar_duas_vezes_e_idempotente`, `VigiaMaterializadaVivaTest.test_MORDE_o_vigia_conta_e_zera_depois_do_julgamento`, `.test_MORDE_exportada_conta_no_balde_do_DP_e_o_julgamento_nao_tem_fronteira`. `chamados/tests/test_o261_julgamento_desfaz_carimbo.py:88`, `:122`, `:208`, `:221`.
- O cenário O261 também alimenta `test_e1_v6_horizonte.py::FabricaLeOFatoTest.test_RED_pergunta_retratada_nao_cala_o_fato_que_volta_na_celula`, cuja expectativa passa de `['saida_sem_entrada']` a `[]`. `chamados/tests/test_e1_v6_horizonte.py:459`, `:467`. Acrescentar vínculo ao cenário compartilhado conserva a intenção desses testes sob a lei; existe `dar_vinculo` em `chamados/tests/_vinculo_do_dia.py:18`.

Nos outros apps com referências explícitas, não identifiquei expectativa alterada: F3 tem vínculo/DNA (`escala/tests/test_f3_beco1_cadastro_entrou.py:54`, `:72`); `test_cods_do_ato` e `test_classificar_falta_escala` substituem a fábrica (`ponto/tests/test_cods_do_ato.py:67`, `:95`; `ponto/tests/test_classificar_falta_escala.py:24`). Exaustividade sobre caminhos indiretos de todos os signals/cartório da árvore: [nao verificado]. Resultados ORM: [nao verificado].

## 09/11 18:10 · sessao 85fcb794b0e7 · - · - · origem=Ronald (tmux)

Inventário dos campos/valores do diff produtivo `f2962ab8d → 46d313581`:

| Campo/valor | QUEM ESCREVE | QUEM LÊ |
|---|---|---|
| DP `pergunta/pergunta_id`, `ato`, `ato_em`, `ato_por/id`, `ato_texto`, `ato_motivo` | `DecisaoPergunta.registrar`: `chamados/models.py:3945`; novas assinaturas de nascimento: `:3015`; conversão da projeção em linhas RESPOSTA/VALIDACAO: `:3929`, `:3933` | Correspondência para preservar legado: `chamados/models.py:3923`; histórico: `:3504`; tarefa por sequência/horizonte: `chamados/services/tarefa_do_dia.py:131`, `:337` |
| DP fotos `respondida_original_em`, `validada_original_em`, `validada_original_por_id` e vínculos de absorção | Portas existentes, gravadas pelo mesmo executor `chamados/models.py:3945`; campos e restrições: `:3873`, `:3894` | Histórico distingue atos com foto: `chamados/models.py:3509`; tarefa lê ato final: `chamados/services/tarefa_do_dia.py:136`; constraints: `chamados/models.py:3894` |
| Pergunta `respondida_em/resposta_colab` | Porta de resposta existente: `chamados/services/disputa_emissao.py:1464`; nascimento recebe `**campos`: `chamados/models.py:3010`; ressuscitação limpa instante via `:1566` | Nova conversão para DP: `chamados/models.py:3920`, `:3929`; C6 mantém projeção: `chamados/juizes.py:1339`; app: `chamados/services/pergunta_no_app.py:70` |
| Pergunta `validada_em/validada_por/id` | Validação existente: `chamados/services/validacao.py:134`; resposta invalida: `chamados/services/disputa_emissao.py:1477`; ressuscitação: `chamados/models.py:1566` | Conversão para DP: `chamados/models.py:3931`; C6: `chamados/juizes.py:1323` |
| Pergunta `materializada_em/materializada_por/id/via_resolucao` | Porta de carimbo: `chamados/models.py:3093`; limpeza por lote: `:3260`; ressuscitação usa a porta: `:1566` | C6: `chamados/juizes.py:1334`, `:1360`, `:1366`; fábrica busca candidatas com carimbo: `chamados/services/fabrica_da_celula.py:136` |
| Pergunta `pk`, `disputa_id`, `disputa__colaborador_id`, `celula/id`, `data_evento`, `chamado_destino/id`, `conversa`, `motivo` | Nascimento conserva campos: `chamados/models.py:3010`; reuso: `:2962`, `:2978`; reapontamento: `chamados/services/fio.py:925`; data canônica: `chamados/services/fio.py:790` | Captura de origem do evento: `chamados/models.py:3224`; reuso: `:2958`; ressuscitação: `:1565`; reapontamento: `chamados/services/fio.py:924`; fábrica/dedup: `chamados/services/fabrica_da_celula.py:113`, `:116` |
| Pergunta `veredito_lavrado/julgado_em/janela_expira_em` | Lavrador existente: `chamados/veredito_lavrado.py:270`; reuso salva os mesmos campos: `chamados/models.py:3001`; fio: `chamados/services/fio.py:927` | Validade temporal: `chamados/veredito_lavrado.py:228`; tarefa mantém seu leitor: `chamados/services/tarefa_do_dia.py:351` |
| Chamado `status_local`, `_b5_status_antes`, `modulo_origem`, `celula_id`, `pk`, `colaborador` | Estado pela porta existente: `chamados/models.py:228`; snapshot do status persistido: `chamados/signals.py:184` | Novo evento de volta a VIVOS: `chamados/signals.py:191`, `:201`; VIVOS segue catálogo: `chamados/catalogo/motor.py:24`; julgamento seguinte: `chamados/signals.py:214` |
| EventoCelula `celula_id/colaborador_id/data` | Escritor único: `ponto/portas/celula.py:892`; novos emissores: `chamados/models.py:1568`, `:3003`, `:3946`; `chamados/services/fio.py:929`; `chamados/signals.py:207`; reversão: `ponto/portas/celula.py:796` | Consumidor agrupa por colaborador e IDs: `chamados/reconciliador.py:1346`, `:1350`, `:1352`; fábrica lê datas do lote e relê célula atual: `chamados/services/fabrica_da_celula.py:260`, `:262` |
| EventoCelula `processado_em/tentativas/erro` | Consumidor existente, depois da nova fábrica: `chamados/reconciliador.py:1395`, `:1401` | Seleção pendente: `chamados/reconciliador.py:1343`, `:1393`; retenção: `:1408` |
| CelulaDia `pk/data/colaborador_id`, `trabalha`, `ata`, `veredito`, `julgada_em`; Colaborador `situacao='ativo'` | Portas existentes da célula/cartório: `ponto/portas/celula.py:900`, `:1002`; cadastro define situação: `colaboradores/models.py:323` | Fábrica mantém leitura da célula: `chamados/services/fabrica_da_celula.py:80`, `:88`, `:91`; novo encaminhamento CADASTRO: `:164`; consumidor relê atual por colaborador/data: `:262` |
| Retorno `vencidos=None`, `set()`, pares `(tipo, HH:MM)` | Adaptador do teto: `chamados/services/fabrica_da_celula.py:26`, `:28`; a autoridade continua `cartorio.marcos_vencidos` | `None`/vazio deixam de autorizar pergunta: `chamados/services/fabrica_da_celula.py:38`; `None` passado encaminha CADASTRO: `:166`; pares limitam motivos: `:49` |
| Pauta `ancora_tipo=ANCORA_COLAB`, `ancora_id`, `destinatario_departamento='dp'`, `pai=None`, marcador `CADASTRO: PASSADO SEM TURNO PREVISTO`, `autor_user` sistema `ti`, `remetente_informado='ti'`, `texto` | Novo chamador: `chamados/services/fabrica_da_celula.py:294`; escritor único efetivo: `pautas/services.py:180` | Guarda histórica por colaborador/cabeça/DP/marcador: `chamados/services/fabrica_da_celula.py:274`; `ANCORA_COLAB` é importado, sem literal inventado: `:273` |
| Celula `dna/dna_anterior/trabalha/escala_geradora_id/regenerada_em/regeneracoes` | Reversão existente conserva sua escrita: `ponto/portas/celula.py:786`, `:794`; diff acrescenta somente evento `:796` | Própria reversão lê DNA anterior: `ponto/portas/celula.py:777`, `:783`; leitor de reescrita: `escala/services/regeneracao.py:84`, `:104`; fábrica continua lendo ata/célula, `chamados/services/fabrica_da_celula.py:91`, `:107` |

O diff produtivo não altera escritores de cron, registro de juízes, campos de conferência, pendência administrativa ou recusa técnica; os novos testes podem compará-los, mas isso não acrescenta escrita produtiva. Evidência: `git diff f2962ab8d 46d313581 -- chamados/models.py chamados/reconciliador.py chamados/services/fabrica_da_celula.py chamados/services/fio.py chamados/signals.py ponto/portas/celula.py`; cada bloco acima referencia os fontes reais.

Comando ORM principal e vizinhos diretamente relacionados às funções alteradas, para a sessão executar, sem execução aqui:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test --settings=config.settings_test \
chamados.tests.test_e4_expand_orm \
chamados.tests.test_e1_entradas \
chamados.tests.test_e1_volta2 \
chamados.tests.test_e1_v12 \
chamados.tests.test_e1_v13_cadeia \
chamados.tests.test_e1_v15 \
chamados.tests.test_e1_v15b \
chamados.tests.test_e1_v15d_um_ponto_de_escrita \
chamados.tests.test_e1_v15f \
chamados.tests.test_e1_v15g_portas \
chamados.tests.test_e1_v5_ordem_trava \
chamados.tests.test_e1_v5_portas_backfill \
chamados.tests.test_e1_v6_horizonte \
chamados.tests.test_e1_v9 \
chamados.tests.test_fabrica_pela_ata \
chamados.tests.test_fabrica_por_evento \
chamados.tests.test_f1c4_3_emissores \
chamados.tests.test_cobparc5_parcial_sem_periodo_so_lista \
chamados.tests.test_pergunta_do_dia_em_curso \
chamados.tests.test_o261_julgamento_desfaz_carimbo \
chamados.tests.test_jv4a_disparar_vigia_da_celula \
chamados.tests.test_c20_juiz3_outbox \
chamados.tests.test_jv3_reconciliar_vira_vigia \
chamados.tests.test_orfas_pelo_evento \
chamados.tests.test_orfas_nao_morre_na_guarda_da_porta \
chamados.tests.test_porta_reapontamento_pergunta \
chamados.tests.test_o261e_f1_codex_v5 \
chamados.tests.test_o261e_f1_porta_encerramento \
chamados.tests.test_reconciliar_pergunta_a_premissa \
chamados.tests.test_e2_celula_regenerada_reavalia \
ponto.tests.test_porta_celula_dia \
ponto.tests.test_hx_cartorio_ressuscita \
ponto.tests.test_d3_impressao_cobre_insumos \
ponto.tests.test_c3_ordem_dos_receivers_da_batida \
ponto.tests.test_lastro_parte2_fecho_no_evento \
ponto.tests.test_lastro_parte3_fecho_na_pergunta \
ponto.tests.test_o_outbox_celula \
ponto.tests.test_bug114_fechador_de_dois_dias \
ponto.tests.test_bug115_lampada_do_marco \
ponto.tests.test_orfas_vigia
```

Evidência de vizinhança: imports da fábrica em `chamados/tests/test_fabrica_por_evento.py:35`, `test_f1c4_3_emissores.py:76`, `test_o261_julgamento_desfaz_carimbo.py:82`; consumidor em `test_c20_juiz3_outbox.py:241`, `test_jv3_reconciliar_vira_vigia.py:141`, `ponto/tests/test_o_outbox_celula.py:165`; fio em `chamados/tests/test_porta_reapontamento_pergunta.py:271`; signal em `chamados/tests/test_reconciliar_pergunta_a_premissa.py:269` e `ponto/tests/test_d3_impressao_cobre_insumos.py:369`; reversão em `ponto/tests/test_porta_celula_dia.py:184`; ressuscitação em `chamados/tests/test_e1_v13_cadeia.py:140`.

Todos os resultados ORM permanecem `[nao verificado]`.

_3233 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
