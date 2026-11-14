# CODEX-ESTADO

> Ultima resposta processada: **14/11/2026 10:48** · publicado em **14/11/2026 10:51** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 14/11 10:48 · sessao 3217cb06c88f · FRAC5 · verificador · origem=Code

**NÃO: o diff acrescenta a recusa de frações de 1–4 dias no `clean()`, mas não garante a regra completa em todos os caminhos de gravação.** A edição continua salvando sem esse julgamento; o histórico também o dispensa; e a própria porta aceita um período que já contenha fração curta. (`app/ferias/models.py:387`; `app/ferias/services.py:493`; `app/ferias/services.py:693`; `app/ponto/tests/test_ferias_fracao_demais_5.py:83`)

A comparação com a base abaixo é contrafactual pelo diff fornecido. Executei somente o juiz extraído em memória; execução da suíte Django e execução real contra `f7510b95a`: **[nao verificado]**. Ocorrência desses casos em produção: **[nao verificado]**.

1. **Quem escreve e quem lê**

| Campo/valor | Quem escreve ou monta | Quem lê |
|---|---|---|
| `AgendamentoFerias.dias` | Campo inteiro; criação normal, histórico, edição e aprovação escrevem o valor. (`app/ferias/models.py:320`; `app/ferias/services.py:544`; `app/ferias/services.py:487`; `app/ferias/services.py:680`; `app/ferias/services.py:743`) | Porta carrega os dias das outras frações; aviso carrega os dias gravados. (`app/ferias/models.py:372`; `app/ferias/models.py:378`; `app/ferias/situacao.py:122`) |
| `dias_self` | Variável local calculada por `(data_fim-data_inicio).days+1`. (`app/ferias/models.py:377`) | Entra no conjunto para a exigência de uma fração ≥14; entra sozinho para o mínimo de 5. (`app/ferias/models.py:379`; `app/ferias/models.py:387`) |
| Frações do período | `clean()` consulta agendado/em_gozo/concluido e exclui o próprio PK. O aviso repete essa seleção, com exclusão opcional. (`app/ferias/models.py:368`; `app/ferias/models.py:371`; `app/ferias/situacao.py:115`) | O juiz puro recebe os números; a porta seleciona violações; o aviso apresenta os resultados. (`app/ferias/models.py:265`; `app/ferias/models.py:372`; `app/ferias/models.py:380`; `app/ferias/situacao.py:227`) |
| `FRACAO_MINIMA_DEMAIS=5` | Definição única encontrada. (`app/ferias/models.py:253`) | Comparação no juiz; interpolação dos dois avisos. (`app/ferias/models.py:277`; `app/ferias/situacao.py:249`; `app/ferias/situacao.py:359`) |
| Limites 14 e 3 | Constantes existentes. (`app/ferias/models.py:251`; `app/ferias/models.py:252`) | Comparações no mesmo juiz. (`app/ferias/models.py:273`; `app/ferias/models.py:275`) |

**PROCEDE — o conjunto julgado muda conforme a cláusula.** O caput recebe outras frações mais um marcador `[1]`; a exigência de 14 recebe outras mais a duração atual; o mínimo de 5 recebe somente a duração atual. Portanto, chamar o mesmo juiz não significa aplicar o mesmo julgamento ao período inteiro. (`app/ferias/models.py:372`; `app/ferias/models.py:379`; `app/ferias/models.py:387`)

**PROCEDE — histórico curto permanece aceito expressamente.** `[14,3]` mais uma nova fração de 13 passa pela porta, conforme o próprio teste. O período resultante continua contendo uma fração abaixo de 5. Isso limita a promessa de defesa integral feita no mapa do modelo. (`app/ponto/tests/test_ferias_fracao_demais_5.py:79`; `app/ponto/tests/test_ferias_fracao_demais_5.py:83`; `app/ferias/models.py:15`)

O grep de leitores foi:

```bash
rg -n \
'violacoes_de_fracionamento|FRACAO_MINIMA|FRACOES_MAXIMAS|checar_fracionamento|avisos_de_agendamento|fracionamento|frações|fracoes|menor.*14' \
app --glob '*.py' --glob '*.html' --glob '*.js' \
--glob '!**/migrations/**'
```

Os leitores relevantes encontrados foram:

| Leitor | Resultado |
|---|---|
| Views de criação | Delegam a `checar_fracionamento`, que instancia e chama `clean()`. **NÃO PROCEDE** outro comparador próprio nesse caminho. (`app/ferias/views.py:258`; `app/ferias/services.py:293`) |
| Preview com abono/intake | Delegam à validação existente. (`app/ferias/services.py:309`; `app/core/intake.py:34`; `app/core/intake.py:35`) |
| Aviso do período e da proposta | Chamam o juiz puro. (`app/ferias/situacao.py:227`; `app/ferias/situacao.py:348`) |
| API de mensageria | Delega à situação e aos avisos. (`app/api/views_mensageria.py:791`; `app/api/views_mensageria.py:812`; `app/api/views_mensageria.py:824`) |
| Comando de simulação | Usa `_validar_agendamento`. (`app/ferias/management/commands/listar_ferias_dp.py:107`) |
| Tela principal | Texto corrigido para 3/14/5. (`app/templates/ferias/agendar.html:10`) |
| Formulário inline | **PROCEDE** texto incompleto: continua anunciando apenas 3/14. (`app/templates/ferias/_secao_colab.html:63`) |

**NÃO PROCEDE — segundo comparador numérico de fracionamento encontrado em views/forms/services/crons/API/JS.** Os caminhos encontrados acima delegam; ausência absoluta de variantes dinâmicas ou não reconhecidas pelo grep: **[nao verificado]**. O `min="1"` da interface não constitui sozinho outro juiz, pois a criação delega ao servidor. (`app/templates/ferias/_secao_colab.html:68`; `app/ferias/views.py:259`)

**PROCEDE — aviso incompleto quando duas violações coexistem.** Para `[5,5,5,5]`, o juiz retorna `par1` e `caput`, mas o aviso usa `if par1 / elif caput` e omite o caput. É comportamento preexistente que permanece. (`app/ferias/models.py:273`; `app/ferias/models.py:275`; `app/ferias/situacao.py:228`; `app/ferias/situacao.py:237`)

**PROCEDE — zero desaparece do julgamento.** O filtro `if f` elimina zero antes do mínimo e da contagem: `[14,0] → []` e `[14,5,5,0] → []`. Não demonstra uma gravação real com zero, mas demonstra que o juiz isolado não expressa literalmente “nenhuma fração <5” para toda entrada inteira. (`app/ferias/models.py:271`; `app/ferias/models.py:275`; `app/ferias/models.py:277`)

2. **Edição, cancelamento, status e caminhos de gravação**

**NÃO PROCEDE — `clean()` só funciona para objeto novo.** Ele exclui o próprio PK e julgaria corretamente uma edição explícita para quatro dias, havendo outra fração de 14. O furo está nos chamadores que não executam `clean()`. (`app/ferias/models.py:371`; `app/ferias/models.py:377`; `app/ferias/models.py:387`)

| Caminho | Julga fracionamento antes de gravar? |
|---|---|
| Criação normal/individual/lote | Sim: trava o período, valida e depois cria. (`app/ferias/services.py:531`; `app/ferias/services.py:541`; `app/ferias/services.py:544`) |
| Aprovação de solicitação | Sim: `full_clean()` antes de `save()`. (`app/ferias/services.py:749`; `app/ferias/services.py:750`) |
| Edição regular | **PROCEDE o bypass:** aceita duração ≥1 e chama remarcação; o serviço troca dias/datas e salva sem `clean()`. (`app/ferias/views.py:712`; `app/ferias/views.py:722`; `app/ferias/services.py:680`; `app/ferias/services.py:693`) |
| Correção administrativa | **PROCEDE o bypass, documentado:** dispensa `full_clean()` e usa o mesmo escritor da edição. (`app/ferias/views.py:508`; `app/ferias/views.py:531`; `app/ferias/services.py:702`) |
| Gozo histórico | **PROCEDE o bypass:** valida datas/saldo/abono, mas dispensa fracionamento e salva. Pode admitir `<5`, ausência de 14 ou quarta fração, se as demais guardas permitirem. (`app/ferias/services.py:492`; `app/ferias/services.py:507`; `app/ferias/services.py:510`) |
| Cancelamentos | Não reavaliam as frações restantes; alteram saldo/status. (`app/ferias/services.py:616`; `app/ferias/services.py:620`; `app/ferias/services.py:643`; `app/ferias/services.py:652`) |
| Cron de status | Salva somente status, sem `clean()`. (`app/ferias/services.py:916`; `app/ferias/services.py:920`) |

**PROCEDE — cancelamento pode remover a única fração ≥14.** Cancelar o 14 de `[14,5]` deixa `[5]` no universo considerado pelo juiz; o cancelamento não consulta esse resultado. (`app/ferias/services.py:619`; `app/ferias/services.py:620`; `app/ferias/models.py:370`)

**NÃO PROCEDE — remarcação oficial ressuscita cancelado.** Ela relê o status sob trava e recusa `cancelado`. Reagendar pela criação produz outra linha e desconsidera cancelados na duplicidade. (`app/ferias/services.py:599`; `app/ferias/services.py:600`; `app/ferias/services.py:670`; `app/ferias/models.py:313`; `app/ferias/services.py:534`; `app/ferias/services.py:544`)

**PROCEDE — o status da própria instância não é filtrado pelo `clean()`.** As outras linhas são filtradas, mas `self` entra incondicionalmente. Assim, chamar `clean()` num cancelado também o conta/julga; os serviços de cancelamento normalmente não fazem essa chamada. (`app/ferias/models.py:370`; `app/ferias/models.py:372`; `app/ferias/models.py:379`; `app/ferias/models.py:387`)

**PROCEDE — `save()` não é a porta obrigatória dessa regra.** Ele executa guarda de sobreposição, persistência e espelho; não chama `clean()`. (`app/ferias/models.py:461`)

**NÃO PROCEDE — segundo escritor de produção fora do módulo registrado, no censo encontrado.** O registro permite `ferias/services.py`, e os escritores materiais acima estão nele. Há vários caminhos dentro dessa porta que pulam o juiz. (`app/core/portas.py:489`; `app/ferias/services.py:510`; `app/ferias/services.py:693`)

Grep complementar de escritores:

```bash
rg -n \
'AgendamentoFerias|_AgFer|ferias_agendamentoferias|agendamentos\.(create|update|bulk_create)' \
. --glob '*.py' --glob '!**/test*' --glob '!**/migrations/**'
```

Não apareceu `bulk_create`/`bulk_update`/`QuerySet.update` de produção para esse modelo nesse censo; mecanismos externos ou dinâmicos: **[nao verificado]**. O admin local registra somente `Afastamento`; o teste novo usa `bulk_create` deliberadamente para preparar histórico. (`app/ferias/admin.py:2`; `app/ferias/admin.py:4`; `app/ponto/tests/test_ferias_fracao_demais_5.py:69`; `app/ponto/tests/test_ferias_fracao_demais_5.py:82`)

**PROCEDE — mensagem nova contradiz um caminho real.** A correção admin coleta “este agendamento será recusado”, grava a fração curta e retorna sucesso anexando esse aviso. (`app/ferias/situacao.py:359`; `app/ferias/views.py:517`; `app/ferias/views.py:531`; `app/ferias/views.py:549`; `app/ferias/views.py:553`)

3. **Unidade e abono**

**NÃO PROCEDE — unidade diferente em `dias_self`.** Início e fim são `DateField`; a diferença mais um conta dias civis corridos inclusivos. Não há conversão por turno, escala ou fuso nessa expressão. (`app/ferias/models.py:318`; `app/ferias/models.py:319`; `app/ferias/models.py:377`)

**PROCEDE — a equivalência com `dias` depende do caminho.** `clean()` exige `dias == intervalo inclusivo`; `save()` não exige. O serviço de edição recebe datas e quantidade separadamente, embora as views atuais calculem o fim coerentemente. Divergência efetivamente gravada em produção: **[nao verificado]**. (`app/ferias/models.py:395`; `app/ferias/models.py:461`; `app/ferias/services.py:657`; `app/ferias/services.py:680`; `app/ferias/views.py:718`)

**NÃO PROCEDE — abono aumenta a duração da fração.** O gozo escreve `dias`; a venda incrementa separadamente `PeriodoAquisitivo.dias_vendidos`. O saldo desconta ambos, mas o fim é calculado somente pelos dias de gozo. Portanto, 20 gozados + 10 vendidos constituem fração de 20 nesse código. (`app/ferias/services.py:269`; `app/ferias/services.py:277`; `app/ferias/models.py:220`; `app/ferias/services.py:319`)

4. **Cada asserção morde a base?**

Todas as referências desta tabela são de `app/ponto/tests/test_ferias_fracao_demais_5.py`. “Sim” significa falha contrafactual pelo diff; execução real na base: **[nao verificado]**.

| Linha / asserção | Falharia? | Motivo |
|---|---|---|
| `:20` — `[14,4]` | Sim | Base retornaria `[]`. |
| `:21` — `[25,1]` | Sim | Base retornaria `[]`. |
| `:22` — `[4]` | Sim | Base retornaria somente `['par1']`. |
| `:26` — `[14,5]` | Não | Já retornava `[]`. |
| `:27` — `[20,5,5]` | Não | Já retornava `[]`. |
| `:28` — `[30]` | Não | Já retornava `[]`. |
| `:32` — `[10,10]` | Não | Já retornava `['par1']`. |
| `:33` — `[14,5,5,5]` | Não | Já retornava `['caput']`. |
| `:34` — `[14,3,5,5]` | Sim | Base retornaria somente `['caput']`. |
| `:41` — ausência da comparação literal | Sim | A comparação existia no texto-base. |
| `:42` — presença da chamada ao juiz | Sim | A chamada não existia nessa função-base. |
| `:70`/`:71` — `assertRaises` para quatro dias | Sim | A base aceitaria. |
| `:72` — mensagem contém `5 dias` | Sim, isoladamente | Base não produz essa mensagem; a execução já falharia no `assertRaises`. |
| `:77` — aceita cinco, implicitamente | Não | A base também aceitava. |
| `:83` — aceita treze ao lado de `[14,3]` | Não | A base também aceitava. |
| `:91` — aviso do período contém `5 dias` | Sim | Aviso novo não existia. |
| `:97` — aviso da proposta contém `5 dias` | Sim | Aviso novo não existia. |
| `:106` — remove texto antigo | Sim | Base continha a expressão. |
| `:107` — contém “ao menos uma…” | Sim, isoladamente | Base não continha; normalmente para na `:106`. |
| `:108` — contém “nenhuma…” | Sim, isoladamente | Mesmo motivo. |

**NÃO PROCEDE — conjunto inteiro sem mordida.** Há testes comportamentais que distinguem a cura da base. **PROCEDE — selo estrutural fraco:** buscar substrings não prova exclusividade nem uso efetivo do juiz. (`app/ponto/tests/test_ferias_fracao_demais_5.py:20`; `app/ponto/tests/test_ferias_fracao_demais_5.py:70`; `app/ponto/tests/test_ferias_fracao_demais_5.py:91`; `app/ponto/tests/test_ferias_fracao_demais_5.py:40`)

**PROCEDE — lacunas materiais:** os testes novos não exercitam edição pelo serviço, cancelamento, `save()` como entrada, zero ou aviso simultâneo `par1+caput`; o helper sempre fornece dias/datas coerentes e usa concluído por padrão. (`app/ponto/tests/test_ferias_fracao_demais_5.py:61`; `app/ponto/tests/test_ferias_fracao_demais_5.py:64`; `app/ponto/tests/test_ferias_fracao_demais_5.py:71`; `app/ponto/tests/test_ferias_fracao_demais_5.py:83`; `app/ponto/tests/test_ferias_fracao_demais_5.py:95`)

5. **Classes de reprovação pedidas**

| Classe | Veredito e motivo |
|---|---|
| Segundo juiz duplicando o registrado | **NÃO PROCEDE como introdução deste diff.** O puro já existia e a porta delega. O endereço vivo continua sendo `AgendamentoFerias.clean`, não o puro; o lookup manda consultar esse registro. (`app/ferias/models.py:266`; `app/ferias/models.py:380`; `app/core/juizes.py:271`; `app/docs/JUIZES.md:14`) |
| Segundo escritor fora da porta | **NÃO PROCEDE no censo de produção; PROCEDE gravação sem julgamento dentro da porta.** (`app/core/portas.py:489`; `app/ferias/services.py:693`) |
| Relógio fora da borda | **PROCEDE na árvore preexistente:** avisos têm fallback `date.today()` e cancelamento admin lê a data internamente. **NÃO PROCEDE no juiz novo**, que só recebe números. (`app/ferias/situacao.py:156`; `app/ferias/situacao.py:326`; `app/ferias/services.py:649`; `app/ferias/models.py:265`) |
| Derivação paralela da fonte única | **PROCEDE na montagem duplicada do universo**, sem divergência atual de status: porta e aviso repetem a consulta. **NÃO PROCEDE outro cálculo numérico independente encontrado.** (`app/ferias/models.py:368`; `app/ferias/situacao.py:117`; `app/ferias/situacao.py:227`) |
| Selo/RED sem mordida | **NÃO PROCEDE para o conjunto; PROCEDE a limitação dos testes por substring e a falta da edição real.** (`app/ponto/tests/test_ferias_fracao_demais_5.py:20`; `app/ponto/tests/test_ferias_fracao_demais_5.py:41`; `app/ponto/tests/test_ferias_fracao_demais_5.py:71`) |
| Ordem de trava | **NÃO PROCEDE alteração de ordem pelo diff.** Criação trava PA antes de validar; edição trava AG → PA antes dos efeitos. Deadlock real: **[nao verificado]**. (`app/ferias/services.py:531`; `app/ferias/services.py:541`; `app/ferias/services.py:599`; `app/ferias/services.py:602`; `app/ferias/services.py:688`) |
| Pouso antes do pré-requisito | **NÃO PROCEDE como achado demonstrado.** Sequência efetiva de pouso/deploy e pré-requisitos cumpridos: **[nao verificado]**. | [nao verificado]
| Campo com dois significados | **NÃO PROCEDE confusão de unidade demonstrada; PROCEDE falta de imposição universal da equivalência dias/datas.** Abono permanece separado. (`app/ferias/models.py:377`; `app/ferias/models.py:395`; `app/ferias/models.py:461`; `app/ferias/services.py:277`) |

## 14/11 10:48 · sessao 149f75f30723 · [nome]-ANDAM-JUNTO · verificador · origem=Code

**O delta fecha parte dos problemas do leitor e das provas do selo; os três achados de conteúdo continuam abertos.** A validação semântica também permanece incompleta: aceita portões sem identificação e `fecha BO` sem fonte (`bin/placar_grade.py:55`, `bin/placar_grade.py:60`, `app/docs/GRADE-MESTRA.md:119`, `app/docs/GRADE-MESTRA.md:272`, `app/docs/GRADE-MESTRA.md:853`).

**1. Volta 1: cura e caso que morde**

| Achado | Veredito na volta 2 | Cura/preservação e selo |
|---|---|---|
| Tabelas de oito/dez colunas sem deslocamento | **PROCEDE, preservado.** Esquema vem do cabeçalho; largura incompatível é rejeitada. | Leitor: `bin/placar_grade.py:76`, `bin/placar_grade.py:87`. Caso novo `RJ-LARGA`, com nove células: `bin/tests/test_placar_grade.sh:138`; rejeição exigida em `:150`. |
| Células semanticamente inválidas entram | **PROCEDE, parcialmente fechado.** `REGISTRADAxyz` foi corrigido nas dez colunas; nas oito continua reconhecimento por prefixo. `lei`, `arquivo congelado`, `colisao com` sem identificação e número sem fonte continuam aceitos. | Cura: `bin/placar_grade.py:97`; caso que morde: `bin/tests/test_placar_grade.sh:151`. Pendências: `bin/placar_grade.py:94`, `bin/placar_grade.py:55`, `bin/placar_grade.py:60`. |
| Soma dos valores informados por estado | **PROCEDE, preservado.** Soma declarações; não demonstra BO distintos nem ausência de sobreposição. | Soma: `bin/placar_grade.py:145`, `bin/placar_grade.py:149`; assert exato: `bin/tests/test_placar_grade.sh:117`. Correspondência com BO reais: **[nao verificado]**. |
| Complemento não aumenta obras | **PROCEDE, preservado.** Complementos ficam separados, participam da soma e recebem `(compl.)`. | Separação: `bin/placar_grade.py:114`; soma: `:145`; apresentação: `:264`. Mordida: total em `bin/tests/test_placar_grade.sh:113` e linha completa em `:121`. |
| `?` entra no denominador, não nas medidas | **PROCEDE, preservado.** Zero também é medido. | `bin/placar_grade.py:53`, `bin/placar_grade.py:145`, `bin/placar_grade.py:150`. Soma exige quatro medidas entre seis linhas; zero aparece explicitamente: `bin/tests/test_placar_grade.sh:116`, `:117`, `:122`. |
| Asserts existentes têm mordida | **PROCEDE; lacunas apontadas foram cobertas.** Há agora casos para `abc`, zero, largura, estado inválido, complemento inválido e sufixo. | `bin/tests/test_placar_grade.sh:121`, `:122`, `:149`, `:150`, `:151`, `:152`. O mutante que apaga a medida do complemento continua exigindo vermelho em `:160`, `:161`, `:162`. |
| DECLARADO-X-GRAVADO omite impedimento de lei | **PROCEDE, aberto.** Continua “colisao”, enquanto o RELATO retira a obra até corte de cinco questões. | Sem cura: `app/docs/GRADE-MESTRA.md:272`, `app/docs/RELATO.md:7`, `app/docs/RELATO.md:24`. Nenhum caso específico no selo apresentado: `bin/tests/test_placar_grade.sh:132`, `:165`. |
| E3 registra colisão consigo mesma | **PROCEDE, aberto.** “A propria obra em voo” continua no portão; a regra compara arquivos entre raias. | Sem cura: `app/docs/GRADE-MESTRA.md:119`, `CLAUDE.md:309`, `CLAUDE.md:310`. Nenhuma prova dessa relação no selo: `bin/tests/test_placar_grade.sh:132`, `:165`. |
| O232 aponta JUSTIFICATIVA como colisão em voo | **PROCEDE a falta de sustentação, aberto.** A referência continua; JUSTIFICATIVA está INTERROMPIDA na própria GRADE. | `app/docs/GRADE-MESTRA.md:853`, `app/docs/GRADE-MESTRA.md:269`. Colisão atual com agente vivo: **[nao verificado]**. |

Reproduzi em memória os asserts relevantes e mutantes: rejeitar zero, aceitar `abc`, aceitar `REGISTRADAxyz`, aceitar complemento inválido e retirar `(compl.)` derrubam verificações; retirar a guarda de largura provoca exceção, também detectável pelo selo (`bin/tests/test_placar_grade.sh:112`, `:121`, `:122`, `:146`, `:149`, `:151`, `:152`). Execução integral do shell: **[nao verificado]**; ele cria e remove temporários (`bin/tests/test_placar_grade.sh:21`, `:26`).

**2. Escritores, leitores e significado dos números**

| Objeto | Quem escreve | Quem lê |
|---|---|---|
| Coluna `resolve` | Escritor efetivo: **[nao verificado]**. A GRADE atribui origem à leitura Codex e determina manutenção por marco (`app/docs/GRADE-MESTRA.md:4`, `:5`). | `ler()` monta o campo pelo cabeçalho; `resolve_preenchido()` conta sua presença (`bin/placar_grade.py:39`, `:90`, `:121`). |
| `logs/GRADE-MESTRA/resolve/quadro.md` | Escritor de produção: **[nao verificado]**. O selo escreve apenas quadros de fixture (`bin/tests/test_placar_grade.sh:125`, `:128`). | `sem_obra()` escolhe caminho explícito/env/padrão, lê o arquivo e extrai `sem obra: N` (`bin/placar_grade.py:132`, `:134`, `:137`). |
| Linhas derivadas | `--cabecalho` escreve o bloco da GRADE; `--placar` escreve o documento derivado (`bin/placar_grade.py:195`, `:205`, `:260`, `:270`). | `--conferir` compara o bloco completo; o selo verifica as linhas novas (`bin/placar_grade.py:221`, `bin/tests/test_placar_grade.sh:123`, `:127`, `:159`). |

**NAO PROCEDE “456 digitado no cálculo”.** O denominador é `len(lidas)`; complementos ficam separados. A cópia registra `11/456`, e o selo também exige `1/5` na fixture, contrariando uma constante 456 (`bin/placar_grade.py:114`, `:122`, `app/docs/GRADE-MESTRA.md:28`, `bin/tests/test_placar_grade.sh:123`).

**PROCEDE exclusão incompleta de marcadores:** somente vazio, `?` e `-` são excluídos, após remoção dos espaços das extremidades. `n/a` e `[nao verificado]` seriam contados como preenchidos (`bin/placar_grade.py:48`, `:121`). Esses marcadores não aparecem na coluna desta cópia; inflação atual causada por eles: **NAO PROCEDE**, conforme os valores distintos abaixo (`app/docs/GRADE-MESTRA.md:35`, `bin/placar_grade.py:90`).

Valores distintos de `resolve`, incluindo obras e complementos:

| Valor literal | Exemplo |
|---|---|
| `-` | `app/docs/GRADE-MESTRA.md:37` |
| `HJ-074` | `app/docs/GRADE-MESTRA.md:127` |
| `HJ-092` | `app/docs/GRADE-MESTRA.md:128` |
| `casos 01, 02, 05, 06, 08` | `app/docs/GRADE-MESTRA.md:170` |
| `caso 03` | `app/docs/GRADE-MESTRA.md:267` |
| `HJ-090; HJ-091` | `app/docs/GRADE-MESTRA.md:268` |
| `HJ-093` | `app/docs/GRADE-MESTRA.md:269` |
| `HJ-091` | `app/docs/GRADE-MESTRA.md:288` |
| `casos 01–10` | `app/docs/GRADE-MESTRA.md:289` |
| `caso 01–14` | `app/docs/GRADE-MESTRA.md:431` |
| `caso 10` | `app/docs/GRADE-MESTRA.md:505` |
| `caso 13–14` | `app/docs/GRADE-MESTRA.md:686` |
| `HJ-095` | `app/docs/GRADE-MESTRA.md:687` |
| `caso 07` | `app/docs/GRADE-MESTRA.md:705` |
| `caso 09` | `app/docs/GRADE-MESTRA.md:1052` |
| `caso 02,03,05,07,08,09,10,11,12` | `app/docs/GRADE-MESTRA.md:1059` |
| `HJ-090` | `app/docs/GRADE-MESTRA.md:1351` |

O numerador mede **células de obras preenchidas**, sem deduplicar referências nem contar casos individualmente: `HJ-090; HJ-091` vale uma célula; `HJ-092` aparece em duas obras. Complementos preenchidos ficam fora desse indicador (`bin/placar_grade.py:121`, `app/docs/GRADE-MESTRA.md:268`, `:128`, `:469`, `bin/placar_grade.py:114`).

**3. Determinismo e consumidores**

**PROCEDE: `--conferir` continua independente de `logs/`.** Seu caminho lê a GRADE e deriva `cabecalho()`; `sem_obra()` só entra no PLACAR. A conferência da cópia retornou verde com 456 obras, inclusive variando `PLACAR_QUADRO_RESOLVE` (`bin/placar_grade.py:209`, `:218`, `:221`, `:181`, `:260`, `app/docs/GRADE-MESTRA.md:22`, `:24`, `:28`).

O **PLACAR completo** depende agora do quadro/env; já continha horário e commit. Portanto essa independência vale para `--conferir`, não para todo o documento produzido (`bin/placar_grade.py:132`, `:241`, `:260`).

**NAO PROCEDE quebra nos consumidores nomeados:**

| Consumidor | Leitura relevante |
|---|---|
| `relato.sh` | Gera com o leitor atualizado e transporta GRADE/PLACAR inteiros (`bin/relato.sh:101`, `:119`, `:120`). |
| Raspador | Lê texto inteiro, mascara e escreve; não interpreta esses contadores (`bin/raspar_publico.py:28`, `:31`, `:40`). |
| `hook_stop_fila1` | Lê BACKLOG e RELATO (`bin/hook_stop_fila1.py:33`, `:34`, `:186`, `:472`). |
| Pre-push | Lê referências Git, carimbo e labels da régua (`bin/pre-push.sh:8`, `:53`, `:184`; `app/core/hooks/pre-push.sh:22`, `:80`). |

O selo está ligado ao loop da régua; **não é chamado diretamente pelo pre-push**, cujo comentário explicita essa diferença (`bin/regua.sh:138`, `:140`, `:143`, `bin/pre-push.sh:14`, `:15`). Outros consumidores externos: **[nao verificado]**.

**4. Classes pedidas**

| Classe | Veredito |
|---|---|
| **Selo sem mordida** | **NAO PROCEDE** para as lacunas antigas agora cobertas. **PROCEDE uma lacuna residual:** retirar vazio da lista de exclusão de `resolve` passa pelas fixtures, pois elas exercitam referência, `?` e `-`, sem célula vazia (`bin/placar_grade.py:121`, `bin/tests/test_placar_grade.sh:90`, `:92`, `:123`, `:159`). |
| **Número sem fonte** | **PROCEDE em `fecha BO`:** a fonte é opcional no parser, embora exigida na descrição da GRADE; a fixture usa `2` sem fonte (`bin/placar_grade.py:55`, `app/docs/GRADE-MESTRA.md:8`, `bin/tests/test_placar_grade.sh:92`). **NAO PROCEDE como constante inventada em `sem obra`:** lê o quadro ou publica “nao medido”; validade do censo real: **[nao verificado]** (`bin/placar_grade.py:134`, `:137`, `:139`). |
| **Campo com dois significados** | **PROCEDE em `portao` da E3:** “a própria obra está em voo” foi registrado como colisão entre raias (`app/docs/GRADE-MESTRA.md:119`, `CLAUDE.md:309`). **NAO PROCEDE dupla semântica demonstrada em `resolve`:** o indicador implementado mede presença textual; interpretá-lo como quantidade de casos resolvidos não tem sustentação nesse cálculo (`bin/placar_grade.py:119`, `:121`, `app/docs/GRADE-MESTRA.md:268`). |

## 14/11 10:47 · sessao 54e270c3155d · COERENCIA-DAS-LEIS · censo · origem=Code

app/chamados/catalogo/motor.py::VIVOS ; o chamado esta vivo? (app/core/juizes.py:107) ; REGISTRADO-NAO-E-VIVO-DESVIO conserva registrado em VIVOS (app/docs/CORTES.md:328) ; vivo corresponde a aberto, em_analise ou registrado (app/chamados/catalogo/motor.py:24) ; nao ; nenhum achado independente buscando status_local, status_local__in e tuplas aberto/em_analise/registrado, consumidores conferidos usam VIVOS (app/ponto/services/cartorio.py:1384, app/chamados/services/historico.py:180) ; -

app/chamados/catalogo/motor.py::decidir ; pode transitar de um estado a outro? (app/core/juizes.py:108) ; nenhum texto sobre a matriz, endereco documentado em app/docs/JUIZES.md:37 ; nascimento aceita EMITIR/REGISTRAR, transicao humana consulta matriz e perfil, cancelado/superado sao terminais absolutos, eventos do sistema consultam mapa e combinacoes ausentes recusam (app/chamados/catalogo/motor.py:130, app/chamados/catalogo/motor.py:142, app/chamados/catalogo/motor.py:153, app/chamados/catalogo/motor.py:158) ; nao ; nenhum achado independente buscando TRANSICOES_PERMITIDAS, TRANSICAO_HUMANA, pode_transicionar e decidir, modelo deriva a matriz e delega a decisao (app/chamados/models.py:117, app/chamados/models.py:124) ; -

app/chamados/catalogo/premissa.py::premissa_morta ; a premissa do chamado morreu, podendo fechar? (app/core/juizes.py:109) ; L-125 para cluster (app/docs/LEIS.md:146), O169-DECIDIDO-PELA-LEI (app/docs/CORTES.md:430), RESPOSTAS-2910-MANHA aponta a celula para premissa de turno (app/docs/CORTES.md:357) ; True somente para classe reconhecida cujo fato sustentador foi coletado e e falso, classe desconhecida ou fato ausente conservam a premissa viva (app/chamados/catalogo/premissa.py:124, app/chamados/catalogo/premissa.py:128, app/chamados/catalogo/premissa.py:134, app/chamados/catalogo/premissa.py:150) ; nao ; _absolvido_pela_celula responde pela absolvição do chamado de turno usando veredito nao acusador e ausencia de marcos pendentes, autoridade expressamente nomeada pelo corte (app/chamados/reconciliador.py:1019, app/chamados/reconciliador.py:1028, app/chamados/reconciliador.py:1030, app/docs/CORTES.md:357) ; -

app/chamados/utils.py::pode_agir_chamado ; quem pode agir no chamado? (app/core/juizes.py:110) ; nenhum texto especifico em LEIS/CORTES, endereco documentado em app/docs/JUIZES.md:49 ; usa departamento do dono do modulo ou area da solicitacao, recusa anonimo, aceita gestor geral, departamento desconhecido ou usuario sem competencias declaradas, demais precisam da competencia ou perfil correspondente (app/chamados/utils.py:39, app/chamados/utils.py:42, app/chamados/utils.py:55, app/chamados/utils.py:57, app/chamados/utils.py:60, app/chamados/utils.py:65, app/chamados/utils.py:72) ; nao ; nenhum achado contextual independente buscando pode_agir_chamado, departamento_do_chamado, tem_perfil e departamento, consumidores conferidos consultam o juiz (app/chamados/views.py:1015, app/chamados/services/acoes_disputa.py:187) ; -

app/chamados/catalogo/modulos.py::data_do_chamado ; qual o dia do chamado? (app/core/juizes.py:111) ; K1-DIA-DO-CHAMADO declara chave_data=data_turno (app/docs/CORTES.md:177), lookup proibe leitura crua dessa chave (app/docs/JUIZES.md:62) ; devolve o valor da chave_data declarada no catalogo atual ou aposentado, sem modulo ou chave declarada devolve None (app/chamados/catalogo/modulos.py:222, app/chamados/catalogo/modulos.py:225) ; nao ; dias_do_fato determina o dia pela celula e depois por data_turno cru, sem consultar data_do_chamado, o singular tambem acrescenta fallback cru (app/chamados/juizes.py:184, app/chamados/juizes.py:187, app/chamados/juizes.py:118) ; -

app/chamados/juizes.py::pergunta_viva ; a pergunta da disputa esta viva? (app/core/juizes.py:112) ; L-051 (app/docs/LEIS.md:92), L-061 (app/docs/LEIS.md:95), FIO-MORTO-E-ESTRUTURAL (app/docs/CORTES.md:253), leitura da tranca (app/docs/CORTES.md:272), absolvicao pelo marco respondido por batida (app/docs/CORTES.md:413) ; delega ao calculo com memo, validacao, tranca e determinadas vias encerram, resposta preenchida permanece viva salvo decisao administrativa ou concordancia resposta/luz, depois consulta identidade, marco, celula, falta e testemunha da materializacao (app/chamados/juizes.py:894, app/chamados/juizes.py:1401, app/chamados/juizes.py:1405, app/chamados/juizes.py:1412, app/chamados/juizes.py:1417, app/chamados/juizes.py:1429, app/chamados/juizes.py:1438, app/chamados/juizes.py:1444, app/chamados/juizes.py:1450, app/chamados/juizes.py:1462, app/chamados/juizes.py:1476, app/chamados/juizes.py:1483) ; nao ; nenhum achado exato independente buscando pergunta_viva, viva, q_vivas, validada_em, materializada_em e estado, property delega e q_vivas declara somente pre-filtro por contencao (app/chamados/models.py:3392, app/chamados/models.py:3398, app/chamados/models.py:3431) ; -

app/chamados/models.py::DisputaSupervisao.pode_fechar ; a disputa pode fechar? (app/core/juizes.py:113) ; JUIZ-PODE-FECHAR-NASCE exige fio vazio e nenhuma resposta sem veredito (app/docs/CORTES.md:272) ; fechamento ordinario exige simultaneamente respostas_sem_veredito vazia e fio_vivo falso (app/chamados/models.py:1905, app/chamados/models.py:1909) ; nao ; relatorios_disputa.classificar declara PRONTA_FECHAR apenas pela ausencia de perguntas vivas, sem consultar respostas_sem_veredito (app/chamados/relatorios_disputa.py:18, app/chamados/relatorios_disputa.py:20) ; -

app/chamados/models.py::ChamadoColaborador.abrir ; o chamado e duplicado, do mesmo fato? (app/core/juizes.py:114) ; L-007 exige escritor unico e trilha (app/docs/LEIS.md:65), lookup aponta abrir como autoridade do duplicado (app/docs/JUIZES.md:62), nenhum texto especifico da chave em LEIS/CORTES ; sob trava do colaborador busca colaborador mais lookup, reutiliza existente e tenta renascer encerrado, aceita filho agrupado sob pai vivo, senao consulta o motor para nascer (app/chamados/models.py:949, app/chamados/models.py:953, app/chamados/models.py:968, app/chamados/models.py:976, app/chamados/models.py:982) ; nao ; abertura faz dedup proprio por colaborador, modulo, VIVOS, assunto, descricao, prazo e autor, retornando igual ou chamando criar, auditor tambem agrupa duplicatas por colaborador/modulo/dia (app/chamados/services/abertura.py:165, app/chamados/services/abertura.py:170, app/chamados/services/abertura.py:172, app/chamados/management/commands/auditar_invariantes_chamados.py:625, app/chamados/management/commands/auditar_invariantes_chamados.py:626) ; -

app/chamados/catalogo/motor.py::prazo_estourou ; o prazo SLA do chamado estourou? (app/core/juizes.py:115) ; nenhum texto especifico da comparacao em LEIS/CORTES, endereco documentado em app/docs/JUIZES.md:63 ; True quando prazo_sla existe e prazo_sla <= agora, sem prazo retorna False (app/chamados/catalogo/motor.py:56) ; nao ; sla.vencido calcula referencia DP/criacao mais horas por urgencia e compara com <, cobranca._cobranca_vencida usa prazo_sla ou criacao mais 30 minutos e compara com < (app/chamados/services/sla.py:30, app/chamados/services/sla.py:39, app/chamados/services/cobranca.py:253, app/chamados/services/cobranca.py:254) ; -

app/escala/services/regeneracao.py::cobranca_orfa ; a cobranca ficou orfa depois da regeneracao? (app/core/juizes.py:116) ; L-020 sobre reescrita denunciada (app/docs/LEIS.md:71), resposta especifica documentada em app/docs/JUIZES.md:41 ; sem marcos_faltantes ou dia retorna None, senao procura reescrita posterior a criacao, combinando regenerada_em/editada_em e consultando janela historica somente na ausencia de regenerada_em (app/escala/services/regeneracao.py:149, app/escala/services/regeneracao.py:154, app/escala/services/regeneracao.py:119, app/escala/services/regeneracao.py:122, app/escala/services/regeneracao.py:124) ; nao ; nenhum achado independente buscando cobranca_orfa, dia_reescrito_apos, regenerada_em, editada_em e criado_em, consumidores conferidos delegam (app/ponto/management/commands/gerar_celulas.py:257, app/ponto/management/commands/supra_juiz.py:275) ; -

app/chamados/services/silencio_ausencia.py::reconciliar_por_ausencia ; a ausencia do dia suspende, fecha ou levanta a cobranca viva? (app/core/juizes.py:117) ; L-061 (app/docs/LEIS.md:95), L-062 (app/docs/LEIS.md:96), corte S4-PENDENTE-[nome]-COBRANCA transcrito no modulo (app/chamados/services/silencio_ausencia.py:1) ; ausencia aberta silencia ou renova sem sobrepor silencio vivo alheio, aprovada fecha somente com premissa morta e celula nao acusadora, demais casos levantam apenas o marcador proprio (app/chamados/services/silencio_ausencia.py:78, app/chamados/services/silencio_ausencia.py:81, app/chamados/services/silencio_ausencia.py:94, app/chamados/services/silencio_ausencia.py:42, app/chamados/services/silencio_ausencia.py:113) ; nao ; auditor refaz ausencia aberta, marcador e silencio alheio, e acusa cobranca com ausencia aprovada sem a guarda da lampada (app/chamados/management/commands/auditar_invariantes_chamados.py:643, app/chamados/management/commands/auditar_invariantes_chamados.py:646, app/chamados/management/commands/auditar_invariantes_chamados.py:648, app/chamados/management/commands/auditar_invariantes_chamados.py:660) ; -

app/chamados/services/disputa_emissao.py::motivo_fora_da_fabrica ; a fabrica de perguntas alcanca o chamado e, se nao, por que? (app/core/juizes.py:119) ; L-061 (app/docs/LEIS.md:95), FIO-MORTO-E-ESTRUTURAL (app/docs/CORTES.md:253), DUAS-RESPOSTAS item 2 destina nunca_bateu sem adesao a C5 (app/docs/CORTES.md:433) ; retorna causa para disputa, encerrado, nunca_bateu sem adesao, pergunta materializada com celula ausente ou nao acusadora, virada resolvida ou categoria fora do mapa, retorna None para marcos nomeados passados ou categoria mapeada (app/chamados/services/disputa_emissao.py:753, app/chamados/services/disputa_emissao.py:758, app/chamados/services/disputa_emissao.py:760, app/chamados/services/disputa_emissao.py:772, app/chamados/services/disputa_emissao.py:783, app/chamados/services/disputa_emissao.py:785) ; nao ; nenhum achado independente buscando motivo_fora_da_fabrica, chamado_orfao_e_regularizavel, pergunta_do_chamado_ja_encerrada e categoria_fora_do_mapa, consumidores delegam (app/chamados/services/disputa_emissao.py:796, app/chamados/services/pergunta_no_app.py:133) ; -

app/chamados/services/pergunta_no_app.py::retencao ; por que o chamado vivo de furo nao tem pergunta no app? (app/core/juizes.py:120) ; L-061 (app/docs/LEIS.md:95), FIO-MORTO-E-ESTRUTURAL (app/docs/CORTES.md:253) ; sem dia retorna sem_dia, hoje/futuro ou presente no app retorna None, depois verifica fabrica, ata, parcial na lista administrativa, cobertura, pergunta viva em outro chamado, marco ja perguntado e fabrica nao rodada (app/chamados/services/pergunta_no_app.py:127, app/chamados/services/pergunta_no_app.py:129, app/chamados/services/pergunta_no_app.py:133, app/chamados/services/pergunta_no_app.py:136, app/chamados/services/pergunta_no_app.py:144, app/chamados/services/pergunta_no_app.py:146, app/chamados/services/pergunta_no_app.py:149, app/chamados/services/pergunta_no_app.py:151, app/chamados/services/pergunta_no_app.py:153) ; nao ; nenhum achado independente buscando retencao, fabrica_nao_rodou, pergunta_encerrada_marco_apagado e parcial_na_lista_do_admin, consumidores delegam (app/chamados/services/pergunta_no_app.py:201, app/chamados/services/pergunta_no_app.py:223) ; -

app/chamados/juizes.py::competencia_do_chamado ; qual a competencia do chamado? (app/core/juizes.py:122) ; L-026 (app/docs/LEIS.md:77), L-090 (app/docs/LEIS.md:123), K8-COMPETENCIA-NAO-E-MES-CIVIL (app/docs/CORTES.md:216) ; escolhe o fato mais recente entre dia_do_fato, ausencia e perguntas vivas, determina a competencia por janela_atual com a empresa, retorna None sem fato datado (app/chamados/juizes.py:2360, app/chamados/juizes.py:2363, app/chamados/juizes.py:2366, app/chamados/juizes.py:2368, app/chamados/juizes.py:2370, app/chamados/juizes.py:2373) ; nao ; arquivar_competencia_encerrada escolhe dia do fato ou criacao conforme escopo e calcula a competencia aritmeticamente, sem consultar competencia_do_chamado (app/chamados/management/commands/arquivar_competencia_encerrada.py:111, app/chamados/management/commands/arquivar_competencia_encerrada.py:115, app/chamados/management/commands/arquivar_competencia_encerrada.py:177, app/chamados/management/commands/arquivar_competencia_encerrada.py:233) ; -

app/chamados/juizes.py::fio_vivo ; alguma pergunta desta colecao esta viva? (app/core/juizes.py:125) ; L-002 (app/docs/LEIS.md:60), FIO-MORTO-E-ESTRUTURAL declara a autoridade da reducao da colecao (app/docs/CORTES.md:253) ; retorna any(pergunta_viva(p) for p in perguntas), colecao vazia retorna False (app/chamados/juizes.py:1513) ; nao ; Command._disputa_viva faz a reducao any(p.viva for p in d.perguntas.all()) fora do juiz, embora p.viva delegue individualmente a C6 (app/chamados/management/commands/arquivar_competencia_encerrada.py:158, app/chamados/models.py:3431) ; -

app/ponto/services/furos_vetados.py::motivo_do_veto ; este dia veta nascimento ou reabertura automatica de chamado de turno? (app/core/juizes.py:131) ; L-093 (app/docs/LEIS.md:111), VETO-EC-NONE e JUIZ-MOTIVO-DO-VETO (app/docs/CORTES.md:403) ; vinculo/template ausente ou regua degenerada retorna VETO_DEGENERADA, regua suspeita retorna VETO_REGUA, demais retornam None (app/ponto/services/furos_vetados.py:40, app/ponto/services/furos_vetados.py:42, app/ponto/services/furos_vetados.py:44) ; nao ; detectar_ausencias e reconciliar_grade recompõem os dois vetos fora do juiz, copias tambem declaradas no registro (app/ponto/management/commands/detectar_ausencias.py:601, app/ponto/management/commands/detectar_ausencias.py:605, app/ponto/management/commands/reconciliar_grade.py:495, app/ponto/management/commands/reconciliar_grade.py:499, app/core/juizes.py:129) ; -

app/chamados/juizes.py::porta_alcancavel_do_dia ; por onde alguem chega a este dia, com o verbo do painel? (app/core/juizes.py:135) ; L-002 (app/docs/LEIS.md:60), LAUDO-3010-JUIZ-DA-ACAO-DO-ADMIN (app/docs/CORTES.md:404), O261-E-F5-DESENHO (app/docs/CORTES.md:409) ; soma chamados da celula na fila e ancoras na fila de perguntas vivas no app sem resposta vazia, devolve rota, fila visivel, verbos e fio, ou None sem porta (app/chamados/juizes.py:1657, app/chamados/juizes.py:1661, app/chamados/juizes.py:1663, app/chamados/juizes.py:1667) ; nao ; nenhum achado da mesma pergunta buscando porta_alcancavel_do_dia, alimentar_portas, fila_cel, FIO_DO_VERBO e projecoes rota/fila/verbos/fio, _portas_da_tarefa restringe a portas de acoes exigiveis e declara nao reconstituir C17 (app/chamados/services/tarefa_do_dia.py:541, app/chamados/services/tarefa_do_dia.py:546) ; -

app/chamados/juizes.py::dia_sem_trabalho ; a cobranca cala porque a celula lavrada diz que o dia nao e de trabalho? (app/core/juizes.py:149) ; L-061 (app/docs/LEIS.md:95), L-125 (app/docs/LEIS.md:146), JUIZ-DIA-SEM-TRABALHO (app/docs/CORTES.md:416), excecao de ausencia parcial documentada em app/docs/JUIZES.md:67 ; True para trabalha is False ou ata.tipo_dia preenchido diferente de trabalho, na ausencia parcial somente trabalha is False cala, trabalha=None sozinho nao cala (app/chamados/juizes.py:438, app/chamados/juizes.py:454, app/chamados/juizes.py:456) ; nao ; cartorio._protesta deriva parcialmente a resposta usando trabalha is not False e marcos nao acesos, excecao expressamente declarada nos textos (app/ponto/services/cartorio.py:1458, app/ponto/services/cartorio.py:1459, app/docs/CORTES.md:416, app/docs/JUIZES.md:75) ; -

app/chamados/services/tarefa_do_dia.py::tarefa_do_dia ; quais acoes ainda sao exigiveis por instancia de marco, de quem e por quais portas? (app/core/juizes.py:155) ; L-002 (app/docs/LEIS.md:60), L-061 (app/docs/LEIS.md:95), FIO-E0-V4 (app/docs/CORTES.md:426), LEI-TRANCA-E2-JUIZ (app/docs/CORTES.md:427) ; exige agora e carga no mesmo instante, combina grade, lampadas, teto, tranca, ultima decisao e efeitos para produzir acoes com dono e porta, expondo nao_sei quando indeterminado (app/chamados/services/tarefa_do_dia.py:116, app/chamados/services/tarefa_do_dia.py:164, app/chamados/services/tarefa_do_dia.py:204, app/chamados/services/tarefa_do_dia.py:306, app/chamados/services/tarefa_do_dia.py:562, app/chamados/services/tarefa_do_dia.py:566) ; nao ; vigia_fio.dono_fora_do_fio decide separadamente se a pendencia espera decisao do dia, ausencia ou cadastro, respondedor antigo nomeado para substituicao na E3 (app/chamados/services/vigia_fio.py:306, app/chamados/services/vigia_fio.py:309, app/chamados/services/vigia_fio.py:311, app/chamados/services/vigia_fio.py:313, app/docs/CORTES.md:426) ; -

app/chamados/juizes.py::dia_da_pergunta_trancado ; o dia da pergunta esta arquivado por competencia trancada sem toque posterior do admin? (app/core/juizes.py:161) ; L-051 (app/docs/LEIS.md:92), L-090 (app/docs/LEIS.md:123), LEI-TRANCA-E2-JUIZ (app/docs/CORTES.md:427), assinatura humana do toque documentada em app/docs/JUIZES.md:88 ; consulta dias_trancados pela competencia da empresa e responde True somente sem toque administrativo posterior, conversa nao tem dia de turno a trancar (app/chamados/juizes.py:338, app/chamados/juizes.py:991, app/chamados/juizes.py:1062, app/chamados/juizes.py:1078) ; nao ; nenhum achado independente buscando dia_da_pergunta_trancado, dias_trancados, periodo_trancado, TOQUE_COBRAR_DIA e _admin_tocou, consumidores conferidos delegam (app/chamados/reconciliador.py:668, app/chamados/services/disputa_emissao.py:1970, app/chamados/services/tarefa_do_dia.py:422) ; -

app/chamados/services/fabrica_da_celula.py::pauta_cadastro_existente ; o dia passado sem turno previsto ja esta entregue ao DP como pendencia de cadastro? (app/core/juizes.py:297) ; MODO-COMUM-PASSADO-SEM-TURNO e LEI-PASSADO-SEM-MARCOS (app/docs/CORTES.md:428) ; True quando existe cabeca DP viva do marcador para o colaborador ou pauta fechada por feito/cancelado em data local igual ou posterior ao dia consultado, dia=None consulta somente viva (app/chamados/services/fabrica_da_celula.py:308, app/chamados/services/fabrica_da_celula.py:327, app/chamados/services/fabrica_da_celula.py:329, app/chamados/services/fabrica_da_celula.py:331, app/chamados/services/fabrica_da_celula.py:334) ; nao ; nenhum achado independente buscando PAUTA_SEM_TURNO, CADASTRO: PASSADO SEM TURNO PREVISTO e pauta_cadastro_existente, o escritor pergunta ao juiz (app/chamados/services/fabrica_da_celula.py:367) ; -

app/chamados/models.py::DisputaSupervisao.respostas_sem_veredito ; quais respostas do colaborador na disputa esperam veredito? (app/core/juizes.py:298) ; O168-JUIZ-DO-VEREDITO (app/docs/CORTES.md:449), tranca por leitura L-051 (app/docs/LEIS.md:92, app/chamados/models.py:1896) ; lista perguntas sem validada_em nem materializada_em, com respondida_em ou texto preenchido apos strip, excluindo dia trancado salvo ignorar_tranca=True (app/chamados/models.py:1886, app/chamados/models.py:1898, app/chamados/models.py:1902) ; nao ; auditor deriva I21 por respondida_em preenchido, validada/materializada nulas e texto diferente de vazio, restringindo a pais/destinos mortos sem chamar o juiz (app/chamados/management/commands/auditar_invariantes_chamados.py:1572, app/chamados/management/commands/auditar_invariantes_chamados.py:1575, app/chamados/management/commands/auditar_invariantes_chamados.py:1576) ; -

app/ponto/supra_juiz.py::pode_executar_csf ; o chamado tem resposta sem veredito que impede encerramento pelo sistema? (app/core/juizes.py:299) ; O168-JUIZ-CSF com predicado antigo expressamente superado (app/docs/CORTES.md:448), O168-JUIZ-DO-VEREDITO (app/docs/CORTES.md:449) ; True permite encerrar somente sem respostas pendentes de veredito nas disputas de origem e nas perguntas que resolvem o chamado, consulta sem_veredito_entre com ignorar_tranca=True nas duas metades (app/ponto/supra_juiz.py:567, app/ponto/supra_juiz.py:573, app/ponto/supra_juiz.py:574, app/ponto/supra_juiz.py:588) ; nao ; 

[truncado em 20000 de 20531 caracteres]

## 14/11 10:47 · sessao dc5489172e29 · CAL09 · verificador · origem=Code

**PROCEDE a lacuna do teste de “duas cascas” e a ambiguidade entre dia civil e dia do turno. NÃO PROCEDE atribuir ao novo gate uma mudança na classificação das batidas:** ele passa a consultar a lista que o próprio laço já desenha. (`app/colaboradores/tests/test_cal09_dia_so_com_saidas.py:54`; `app/colaboradores/services/calendario.py:224`; `app/colaboradores/services/calendario.py:244`; `app/templates/colaboradores/partials/_calendario_grade.html:122`; `app/templates/colaboradores/partials/_calendario_grade.html:125`)

1. **Escritores e produtores — NÃO PROCEDE produtor real sem `batidas` nos caminhos encontrados.**

   O escritor do dict real é `contexto_calendario`:

   | Campo | Construção e escrita |
   |---|---|
   | `batidas` | Achata as batidas dos turnos atribuídos à célula; exclui `retratada_em` preenchido; escreve a chave. (`app/colaboradores/services/calendario.py:610`, `:611`, `:621`, `:729`) |
   | `entradas` | Subconjunto de `batidas_dia` com `tipo == 'E'`; escreve a chave. (`app/colaboradores/services/calendario.py:658`, `:740`) |
   | `retratadas` | Consulta separada com `retratada_em__isnull=False`, intervalo de instantes com fim exclusivo, agrupamento por data civil local; escreve a chave. (`app/colaboradores/services/calendario.py:239`, `:241`, `:244`, `:640`, `:730`) |

   Os três caminhos encontrados entregam esses mesmos dias:

   | Caminho | Alimentação → inclusão |
   |---|---|
   | Calendário do perfil | `app/colaboradores/views.py:1949` → render em `:1965` → `app/templates/colaboradores/calendario_partial.html:62` |
   | Drawer | `app/colaboradores/services/drawer.py:121` → `cal` em `:153` → `app/templates/colaboradores/partials/_drawer_diagnostico.html:35` |
   | Relatório de furos | `app/relatorios/furos.py:160` → `app/templates/relatorios/furos.html:42` |

   Portanto, **o significado é igual entre esses caminhos: marcação viva dos turnos atribuídos ao dia**, excluindo retratadas; não significa necessariamente marcação ocorrida naquele dia civil. (`app/colaboradores/services/calendario.py:224`; `app/colaboradores/services/calendario.py:621`)

   O `batidas` superior do drawer não é outro produtor do dict diário: a grade recebe explicitamente `cal.dias`. (`app/colaboradores/services/drawer.py:147`; `app/templates/colaboradores/partials/_drawer_diagnostico.html:35`)

2. **Leitor órfão de `entradas` — NÃO PROCEDE.**

   A chave continua escrita e o teste de caracterização ainda a lê. Não foi encontrado leitor executável do **campo diário** em templates, views ou JS; as ocorrências restantes na grade são comentários. (`app/colaboradores/services/calendario.py:740`; `app/colaboradores/tests/test_calendario_caracterizacao.py:28`; `app/templates/colaboradores/partials/_calendario_grade.html:115`; `app/templates/colaboradores/partials/_calendario_grade.html:119`)

   A **variável local** `entradas` permanece necessária: alimenta presença geográfica, entrada principal e `houve_trabalho`. As fixtures que montam a chave não são leitores. (`app/colaboradores/services/calendario.py:693`; `app/colaboradores/services/calendario.py:696`; `app/colaboradores/services/calendario.py:715`; `app/colaboradores/tests/test_cal09_dia_so_com_saidas.py:32`; `app/colaboradores/tests/test_ui_pilulas_quebram_linha.py:56`; `app/colaboradores/tests/test_e4cal_l1_r7_grade_le_o_rotulo.py:24`)

3. **Dia civil × dia do turno — PROCEDE a distinção; NÃO PROCEDE como regressão de atribuição introduzida pelo diff.**

   `batidas` pode conter marcação de outro dia civil: o produtor agrupa por `t.data_turno`, e um turno fechado pode atravessar meia-noite. Até uma saída sem entrada pode ser atribuída à véspera pelo juiz. (`app/colaboradores/services/calendario.py:223`; `app/colaboradores/services/calendario.py:224`; `app/ponto/turnos.py:293`; `app/ponto/turnos.py:298`; `app/ponto/turnos.py:322`)

   Assim, a saída da manhã seguinte pode aparecer na célula do turno anterior. Isso segue a autoridade registrada para o pertencimento da batida; o novo gate não recalcula esse pertencimento. (`app/core/juizes.py:208`; `app/colaboradores/services/calendario.py:224`; `app/templates/colaboradores/partials/_calendario_grade.html:122`)

   **PROCEDE a ressalva ao argumento da cura:** `batidas`/`entradas` usam dia do turno, enquanto `retratadas` usa dia civil local. O comentário novo “toda marcação viva do dia” não explicita essa diferença. Uma cauda normalmente pareada já acompanha sua entrada na mesma célula; o caso novo demonstrado pelo teste é uma lista contendo somente saída. (`app/colaboradores/services/calendario.py:224`; `app/colaboradores/services/calendario.py:244`; `app/colaboradores/services/calendario.py:611`; `app/colaboradores/services/calendario.py:658`; `app/templates/colaboradores/partials/_calendario_grade.html:120`; `app/colaboradores/tests/test_cal09_dia_so_com_saidas.py:42`)

   Ocorrências concretas dessa distribuição em produção: **[não verificado]**.

4. **Mordida e grafo — a mordida lógica PROCEDE; a prova completa das cascas NÃO PROCEDE.**

   Para a fixture de saída, `batidas=[S]`, `entradas=[]` e `retratadas=[]`. A guarda antiga do diff anexado fecha o bloco; o teste exige `◀ 07:06`. Portanto, **transplantado para o código anterior, esse teste tem mordida lógica**. Execução efetiva em `f7510b95a`, GREEN atual e RED anterior à cura: **[não verificado]**. (`app/colaboradores/tests/test_cal09_dia_so_com_saidas.py:28`; `app/colaboradores/tests/test_cal09_dia_so_com_saidas.py:41`; `app/templates/colaboradores/partials/_calendario_grade.html:125`)

   **PROCEDE a lacuna do selo:** ele procura uma substring nas fontes de dois templates; não percorre o grafo nem renderiza essas raízes com seus contextos. Omite a terceira inclusão, `relatorios/furos.html`. (`app/colaboradores/tests/test_cal09_dia_so_com_saidas.py:54`; `app/colaboradores/tests/test_cal09_dia_so_com_saidas.py:60`; `app/templates/relatorios/furos.html:42`)

   Também não prova uma casca de acesso do colaborador: o calendário é servido pela view do gestor, e a página de perfil estende `base.html`. Já existe teste anterior que percorre includes/extends, contempla o relatório e cobra ausência da grade sob `base_app.html`. (`app/colaboradores/views.py:1939`; `app/colaboradores/views.py:1940`; `app/templates/colaboradores/detalhe.html:1`; `app/colaboradores/tests/test_calendario_mostra_o_carro.py:224`; `app/colaboradores/tests/test_calendario_mostra_o_carro.py:247`; `app/colaboradores/tests/test_calendario_mostra_o_carro.py:251`)

As classes pedidas, consolidadas no escopo deste diff:

| Classe | Veredito e motivo |
|---|---|
| Segundo juiz duplicando registrado | **NÃO PROCEDE.** O gate lê a lista existente; `Resolver dia` continua lendo seu campo próprio, com autoridade registrada. (`app/templates/colaboradores/partials/_calendario_grade.html:122`, `:129`; `app/core/juizes.py:739`) |
| Segundo escritor fora da porta | **NÃO PROCEDE.** A mudança de produção é uma leitura no template; o dict acrescentado pertence à fixture. (`app/templates/colaboradores/partials/_calendario_grade.html:122`; `app/colaboradores/tests/test_cal09_dia_so_com_saidas.py:28`) |
| Relógio fora da borda | **NÃO PROCEDE como introdução do diff.** O gate não lê relógio; as leituras existentes ficam na montagem do contexto. (`app/templates/colaboradores/partials/_calendario_grade.html:122`; `app/colaboradores/services/calendario.py:163`, `:250`) |
| Derivação paralela de fonte única | **NÃO PROCEDE.** A condição consulta exatamente a lista iterada, sem nova regra de pareamento. (`app/templates/colaboradores/partials/_calendario_grade.html:122`, `:125`) |
| Selo/RED sem mordida | **NÃO PROCEDE para a saída isolada; PROCEDE a insuficiência da alegação de grafo completo.** (`app/colaboradores/tests/test_cal09_dia_so_com_saidas.py:41`, `:54`; `app/templates/relatorios/furos.html:42`) |
| Ordem de trava | **NÃO PROCEDE no diff.** A alteração executável é uma condição de template; os testes constroem objetos locais em `SimpleTestCase`. (`app/templates/colaboradores/partials/_calendario_grade.html:122`; `app/colaboradores/tests/test_cal09_dia_so_com_saidas.py:24`, `:36`) |
| Pouso antes do pré-requisito | **NÃO PROCEDE como acusação comprovada.** Cronologia de suíte, aprovação e pouso: **[não verificado]**. | [nao verificado]
| Campo com dois significados | **PROCEDE a ambiguidade preexistente do eixo “dia”:** vivas por turno, retratadas por civil; o comentário novo não distingue. (`app/colaboradores/services/calendario.py:224`, `:244`; `app/templates/colaboradores/partials/_calendario_grade.html:120`) |

## 14/11 10:46 · sessao f287baced451 · COERENCIA-DAS-LEIS · censo · origem=Code

app/ponto/precedencia.py::fatos_do_dia ; F2 — o feriado suprime o trabalho do dia? ; nenhum L-NNN específico localizado [nao verificado], corte E2-EMISSORES-COMPLEMENTO manda consultar este juiz em app/docs/CORTES.md:360 ; delega a fatos_do_periodo, preserva trabalho previsto quando o vínculo não transforma feriado em folga, caso contrário inclui feriado na precedência (app/ponto/precedencia.py:256, app/ponto/precedencia.py:376, app/ponto/precedencia.py:382), feriado_vira_folga exige ciclo 6x1/5x2/semanal e trabalha_em_feriado falso (app/escala/models.py:32, app/escala/models.py:1390, app/escala/models.py:1392) ; sim: texto apresenta trabalha_em_feriado como exceção (app/docs/JUIZES.md:94), código também preserva plantão 12x36 com essa flag falsa (app/ponto/precedencia.py:373, app/ponto/precedencia.py:377) ; nenhum achado independente confirmado, procurados trabalha_em_feriado, feriado_vira_folga e vetos de feriado [nao verificado], o helper é chamado pelo juiz (app/ponto/precedencia.py:377), o veto do posto declara outra pergunta contratual (app/ponto/management/commands/detectar_ausencias.py:619) ; -

app/ponto/motor_calculo_v2.py::CICLOS_FERIADO_SIMPLES ; F3 — feriado trabalhado paga dobra ou hora simples? ; L-086 em app/docs/LEIS.md:118, cuja própria linha registra a revogação da proibição de dobrar feriado previsto, corte corretivo transcrito em app/ponto/motor_calculo_v2.py:917, entrada correspondente em CORTES.md [nao verificado] ; somente 12x36 pertence ao conjunto de hora simples (app/ponto/motor_calculo_v2.py:3623), fábrica resolve os demais ciclos como dobra (app/ponto/motor_calculo_v2.py:3694), admite parâmetro explícito por cláusula cadastrada (app/ponto/motor_calculo_v2.py:3633), dobra exige feriado_em_dobra e feriado no dia (app/ponto/motor_calculo_v2.py:934) ; sim: comentário ainda afirma que plantão previsto nunca dobra (app/ponto/precedencia.py:375), corte corretivo exclui feriado dessa proibição (app/ponto/motor_calculo_v2.py:917), a revogação já está reconhecida em app/docs/LEIS.md:118 ; achado: fechamento seleciona períodos de feriado e escreve suas horas integralmente em HE100 feriado por regra própria de classificação (app/ponto/services/fechamento.py:949, app/ponto/services/fechamento.py:954, app/ponto/services/fechamento.py:955), sítio ausente dos PENDENTES declarados (app/core/juizes.py:475) ; -

SEM_JUIZ ; F4 — que horas do turno que cruza a meia-noite contam como feriado? ; L-085 atribui toda jornada ao dia de início, inclusive feriado e horas após meia-noite (app/docs/LEIS.md:117), entrada correspondente em CORTES.md [nao verificado] ; Motor12x36 julga feriado pela data local da entrada efetiva de cada período (app/ponto/motor_calculo_v2.py:2759, app/ponto/motor_calculo_v2.py:2809), Comercial agrupa pelo dia do turno e aplica feriado nesse dia (app/ponto/motor_calculo_v2.py:3325, app/ponto/motor_calculo_v2.py:3326, app/ponto/motor_calculo_v2.py:3350), TurnoPartido faz equivalente (app/ponto/motor_calculo_v2.py:2942, app/ponto/motor_calculo_v2.py:3092) ; sim: jornada inteira pertence ao início (app/docs/LEIS.md:117), mas Motor12x36 consulta a data de cada período (app/ponto/motor_calculo_v2.py:2809), divergência também descrita em app/docs/LEIS.md:175 ; achado: fechamento decide feriado pela data civil de cada entrada para classificar horas como HE100 feriado (app/ponto/services/fechamento.py:947, app/ponto/services/fechamento.py:949, app/ponto/services/fechamento.py:955), sítio ausente dos PENDENTES declarados (app/core/juizes.py:475) ; -

app/ponto/janelas.py::janela_atual ; F5 — a que competência, corte a corte, pertence o dia? ; L-026 em app/docs/LEIS.md:77, L-090 em app/docs/LEIS.md:123, corte K8-COMPETENCIA-NAO-E-MES-CIVIL em app/docs/CORTES.md:216 ; usa corte da empresa limitado a 2–28 (app/ponto/janelas.py:51, app/ponto/janelas.py:63), intervalo começa no corte e termina no corte menos um, avançando a competência quando data.day atinge o corte (app/ponto/janelas.py:12, app/ponto/janelas.py:27), fallback executado é 20 para empresa ausente ou campo falso (app/ponto/janelas.py:50) ; sim: docstring estabelece default 21 (app/ponto/janelas.py:37), implementação usa 20 (app/ponto/janelas.py:50) ; achado: _jornada_que_mente escolhe mês/ano civil de hoje e corte 21 literal (app/escala/services/cadastro_realidade.py:224, app/escala/services/cadastro_realidade.py:225), fora dos três sítios F5 declarados (app/core/juizes.py:488, app/core/juizes.py:489, app/core/juizes.py:490) ; -

app/ponto/janelas.py::competencia_fechada ; F6 — a competência do dia está fechada, pode escrever? ; L-026 em app/docs/LEIS.md:77, L-090 em app/docs/LEIS.md:123 e corte K8 em app/docs/CORTES.md:216 fixam competência da data, predicado específico aprovado/exportado nesses textos [nao verificado] ; converte timestamp em dia local, resolve competência pela empresa e consulta FechamentoMensal do colaborador (app/ponto/janelas.py:84, app/ponto/janelas.py:85, app/ponto/janelas.py:86), aprovado/exportado devolvem impedimento, demais casos devolvem vazio (app/ponto/janelas.py:87, app/ponto/janelas.py:88, app/ponto/janelas.py:89) ; nao, predicado escrito no registro coincide com código (app/core/juizes.py:80, app/ponto/janelas.py:87) ; nenhum achado, procurados competencia_fechada, consultas FechamentoMensal e condições aprovado/exportado fora de testes/migrations [nao verificado], consumidores conferidos consultam o juiz (app/ponto/registro_batida.py:135, app/ponto/services/flip_batida.py:22, app/escala/management/commands/lavrar_foto_folgas.py:95), tranca por empresa é pergunta distinta (app/core/juizes.py:76) ; -

app/ponto/catalogo/justificativas.py::prazo_estourou ; F7 — qual o prazo do DP para responder à justificativa? ; nenhum texto específico de duração localizado em LEIS.md/CORTES.md [nao verificado] ; esqueci_entrada/esqueci_saida têm 1 dia, batida_errada/batida_duplicada 2, hora_extra/banco_horas/folga_trabalhada/troca_turno 3, outro 7, demais tipos 3 (app/ponto/catalogo/justificativas.py:22, app/ponto/catalogo/justificativas.py:23, app/ponto/catalogo/justificativas.py:24, app/ponto/catalogo/justificativas.py:26, app/ponto/catalogo/justificativas.py:31), estoura quando dias corridos locais excedem o prazo, mas ignora hoje recebido e consulta o relógio (app/ponto/catalogo/justificativas.py:36, app/ponto/catalogo/justificativas.py:41) ; sim: teste escrito exige estouro ao avançar hoje informado (app/ponto/tests/test_prazo_justificativa_pelo_juiz.py:81), código ignora esse argumento (app/ponto/catalogo/justificativas.py:36) ; nenhum achado, procurados PRAZO_DP, prazo_dp_dias, dias_em_aberto, prazo_estourou e limites fixos de justificativas [nao verificado], tela consulta juiz (app/ponto/views.py:1734), backlog fixo de sete dias já está declarado como PENDENTE (app/ponto/views.py:1747, app/core/juizes.py:491) ; -

app/chamados/catalogo/motor.py::PRAZO_ARQUIVO_DIAS ; F8 — qual o prazo de arquivo/silêncio do chamado? ; nenhum texto em LEIS.md/CORTES.md fixando a duração localizado [nao verificado], corte QUARENTENA-DO-COLAB-POR-PERIODO preserva arquivamento por chamado com prazo sem numerá-lo (app/docs/CORTES.md:348) ; padrão de 30 dias (app/chamados/catalogo/motor.py:84), vencimento calculado como agora mais essa constante (app/chamados/services/arquivo.py:117, app/chamados/services/arquivo.py:118), horizontes explícitos de cinco/dez anos são exceções separadas (app/chamados/services/arquivo.py:112) ; nao, duração do juiz e texto de tela coincidem (app/chamados/catalogo/motor.py:84, app/templates/chamados/partials/_lista_chamados.html:116) ; achado: leitor de tela responde Arquivar por 30 dias por literal próprio (app/templates/chamados/partials/_lista_chamados.html:116), fora dos PENDENTES declarados (app/core/juizes.py:475) ; -

SEM_JUIZ ; F9 — o que é dia útil? ; nenhum texto específico localizado em LEIS.md/CORTES.md [nao verificado] ; VT/VA contam segunda a sexta excluindo feriados, sem município consideram somente nacionais e não consultam escala (app/colaboradores/models.py:996, app/colaboradores/models.py:997, app/colaboradores/models.py:1005), escalonamento aceita segunda a sexta das 08h às 18h sem consultar feriado (app/ponto/management/commands/escalonar_chamados_supervisao.py:25, app/ponto/management/commands/escalonar_chamados_supervisao.py:26) ; sim: registro descreve seg-sex sem considerar feriado (app/core/juizes.py:353), texto de VT/VA exige excluir feriado (app/colaboradores/models.py:994) ; achado: dias_uteis_do_mes define o calendário comercial de VT/VA por conta própria (app/colaboradores/models.py:1005), fora do único PENDENTE F9, que é o escalonamento (app/core/juizes.py:492) ; feriado em segunda a sexta deve ser excluído como em VT/VA (app/colaboradores/models.py:994), ou admitido na janela comercial como no escalonamento (app/ponto/management/commands/escalonar_chamados_supervisao.py:25, app/ponto/management/commands/escalonar_chamados_supervisao.py:26)?

Contagem das linhas acima: perguntas lidas == 8, sendo seis com juiz e duas SEM_JUIZ (app/core/juizes.py:301, app/core/juizes.py:350). Coluna 5 sim: 6. Coluna 6 achado: 5. Coluna 7 preenchida: 1. Alcance em produção: [nao verificado].

## 14/11 10:46 · sessao b0057b754654 · - · - · origem=Ronald (tmux)

- **Escritor de produção da coluna `resolve`: [nao verificado].** A GRADE atribui sua origem a leitura de Codex e manda atualizar a linha no mesmo commit do marco; não identifica automação de preenchimento (`app/docs/GRADE-MESTRA.md:4`, `app/docs/GRADE-MESTRA.md:5`). `--cabecalho` substitui apenas o bloco derivado, preservando as tabelas (`bin/placar_grade.py:192`, `bin/placar_grade.py:198`, `bin/placar_grade.py:205`).
- **Escritor de produção de `quadro.md`: [nao verificado].** Os escritores demonstrados são as fixtures de quadro com `sem obra: 7` e quadro sem esse campo (`bin/tests/test_placar_grade.sh:125`, `bin/tests/test_placar_grade.sh:128`).
- O leitor da coluna é `ler`, pelo cabeçalho e montagem do dicionário; `resolve_preenchido` calcula a presença textual (`bin/placar_grade.py:39`, `bin/placar_grade.py:90`, `bin/placar_grade.py:121`). O leitor do quadro é `sem_obra`, que consulta caminho explícito/env/padrão, lê o texto e extrai `sem obra: N` (`bin/placar_grade.py:132`, `bin/placar_grade.py:134`, `bin/placar_grade.py:137`).
- O denominador é `len(lidas)`, não constante `456`; complemento segue lista separada (`bin/placar_grade.py:114`, `bin/placar_grade.py:122`). A linha persistida declara `11/456` (`app/docs/GRADE-MESTRA.md:28`).

Valores distintos de `resolve`, contando tabelas de obras **e** complementos:

| Valor | Exemplo |
|---|---|
| `-` | `app/docs/GRADE-MESTRA.md:37` |
| `HJ-074` | `app/docs/GRADE-MESTRA.md:127` |
| `HJ-092` | `app/docs/GRADE-MESTRA.md:128` |
| `casos 01, 02, 05, 06, 08` | `app/docs/GRADE-MESTRA.md:170` |
| `caso 03` | `app/docs/GRADE-MESTRA.md:267` |
| `HJ-090; HJ-091` | `app/docs/GRADE-MESTRA.md:268` |
| `HJ-093` | `app/docs/GRADE-MESTRA.md:269` |
| `HJ-091` | `app/docs/GRADE-MESTRA.md:288` |
| `casos 01–10` | `app/docs/GRADE-MESTRA.md:289` |
| `caso 01–14` | `app/docs/GRADE-MESTRA.md:431` |
| `caso 10` | `app/docs/GRADE-MESTRA.md:505` |
| `caso 13–14` | `app/docs/GRADE-MESTRA.md:686` |
| `HJ-095` | `app/docs/GRADE-MESTRA.md:687` |
| `caso 07` | `app/docs/GRADE-MESTRA.md:705` |
| `caso 09` | `app/docs/GRADE-MESTRA.md:1052` |
| `caso 02,03,05,07,08,09,10,11,12` | `app/docs/GRADE-MESTRA.md:1059` |
| `HJ-090` | `app/docs/GRADE-MESTRA.md:1351` |

`preenchido` só exclui `''`, `?`, `-`; aceitaria `n/a` e `[nao verificado]` (`bin/placar_grade.py:121`). Esses dois marcadores não aparecem nos valores da coluna acima; aparecem como linguagem documental em outros contextos, o que não prova uso na coluna (`app/docs/RELATO.md:1406`, `app/docs/RELATO.md:86`).

Os três problemas de conteúdo seguem na cópia:

| Achado | Veredito e prova |
|---|---|
| DECLARADO-X-GRAVADO omite impedimento de lei | **PROCEDE, aberto:** portão continua colisão; RELATO retira a obra até corte e enumera cinco questões (`app/docs/GRADE-MESTRA.md:272`, `app/docs/RELATO.md:7`, `app/docs/RELATO.md:24`). |
| FIO-E0-V4-E3-LEITORES colide consigo mesma | **PROCEDE, aberto:** texto diz “a própria obra em voo”; regra exige comparação de arquivo com outra raia (`app/docs/GRADE-MESTRA.md:119`, `CLAUDE.md:309`, `CLAUDE.md:310`). |
| O232 usa JUSTIFICATIVA como colisão em voo | **PROCEDE falta de sustentação, aberto:** portão continua JUSTIFICATIVA; sua linha está INTERROMPIDA (`app/docs/GRADE-MESTRA.md:853`, `app/docs/GRADE-MESTRA.md:269`). Colisão com agente efetivamente vivo: **[nao verificado]**. |

Não há prova de dois significados diferentes na coluna `resolve`: seus valores são referências HJ/casos, e o cálculo novo só mede presença textual (`app/docs/GRADE-MESTRA.md:127`, `app/docs/GRADE-MESTRA.md:170`, `bin/placar_grade.py:121`). Há coexistência de motivos de impedimento no campo `portao`, prevista no próprio contrato; o defeito demonstrado é preencher esse campo com motivo que não sustenta a situação documental, conforme os três achados acima (`app/docs/GRADE-MESTRA.md:8`, `app/docs/GRADE-MESTRA.md:119`, `app/docs/GRADE-MESTRA.md:272`, `app/docs/GRADE-MESTRA.md:853`).

## 14/11 10:46 · sessao d0e842e7b48f · - · - · origem=Ronald (tmux)

Leitores e unidade, só por leitura estática:

- **PROCEDE — tela paralela incompleta.** O texto corrigido está em `app/templates/ferias/agendar.html:10`, mas o formulário inline ainda declara somente “até 3 frações, uma ≥14 dias”, omitindo mínimo 5, em `app/templates/ferias/_secao_colab.html:63`. O input inline aceita `min="1"` em `app/templates/ferias/_secao_colab.html:68`; isso é permissividade da interface, não segundo juiz, pois o POST dessa forma vai a `agendar_ferias` (`app/templates/ferias/_secao_colab.html:60`) e a validação server delega ao juiz (`app/ferias/views.py:258`, `app/ferias/services.py:293`).

- **PROCEDE — aviso agora mente em caminho real.** `avisos_de_agendamento` diz que a fração `<5` “será recusada” (`app/ferias/situacao.py:359`), mas a correção administrativa usa esse mesmo aviso (`app/ferias/views.py:516`) e grava as novas datas/dias sem `clean()` (`app/ferias/services.py:680`, `app/ferias/services.py:693`). A própria view documenta a dispensa do art.134 nessa correção (`app/ferias/views.py:508`).

- **NÃO PROCEDE — segundo comparador novo de fracionamento fora do juiz.** Os locais executáveis encontrados delegam: `_validar_agendamento` → `checar_fracionamento` → `AgendamentoFerias.clean` (`app/ferias/views.py:258`, `app/ferias/services.py:293`); aviso do período → puro (`app/ferias/situacao.py:227`); aviso hipotético → puro (`app/ferias/situacao.py:348`); preview de abono → `_validar_agendamento` (`app/ferias/services.py:309`); API → avisos/situação (`app/api/views_mensageria.py:791`, `app/api/views_mensageria.py:812`, `app/api/views_mensageria.py:824`); intake declara esses escritor/preview (`app/core/intake.py:34`); comando de simulação usa `_validar_agendamento` (`app/ferias/management/commands/listar_ferias_dp.py:107`). Ausência absoluta de variantes não reconhecidas lexicalmente: **[não verificado]**.

- **Registro: endereço vivo ainda é a porta.** `core/juizes.py` registra `AgendamentoFerias.clean`, não `violacoes_de_fracionamento` (`app/core/juizes.py:271`). O lookup diz explicitamente que o registro vivo manda (`app/docs/JUIZES.md:10`, `app/docs/JUIZES.md:14`). A função pura já é descrita como AUS-ESPELHO-1 de 19/09 (`app/ferias/models.py:266`); portanto esse diff não introduziu uma segunda função de julgamento, embora a descrição “juiz puro registrado” não corresponda ao endereço atual (`app/core/juizes.py:271`).

- **NÃO PROCEDE — unidade divergente em `dias_self`.** Os campos são `DateField` (`app/ferias/models.py:318`), e `dias_self=(fim-início).days+1`, dias civis corridos inclusivos (`app/ferias/models.py:377`); o mesmo `clean` exige igualdade do campo `dias` com esse intervalo (`app/ferias/models.py:395`). A prévia constrói `fim=início+(dias-1)` (`app/ferias/services.py:294`), o histórico faz a inversa (`app/ferias/services.py:487`), e JS calcula fim com `(n-1)` dias e retorno com `n` dias em UTC (`app/templates/ferias/_secao_colab.html:196`).

- **PROCEDE — integridade dessa equivalência depende do caminho.** O `save()` só chama guarda de colisão e sincronização de ausências (`app/ferias/models.py:461`); portanto não impõe a igualdade `dias`/intervalo verificada por `clean()` (`app/ferias/models.py:395`). A edição aceita separadamente `nova_ini`, `nova_fim`, `novos_dias` no escritor (`app/ferias/services.py:657`) e grava os três (`app/ferias/services.py:680`). A view atual calcula fim por dias (`app/ferias/views.py:507`); divergência efetivamente gravada em produção: **[não verificado]**.

- **NÃO PROCEDE — abono contado como fração.** `dias` da fração é gozo; o saldo desconta gozo e vendidos separadamente (`app/ferias/models.py:220`). O preview de abono compara `dias+dias_vendidos` ao saldo, mas mantém fim calculado só por `dias` (`app/ferias/services.py:315`, `app/ferias/services.py:319`). A porta cria o agendamento de gozo e depois só incrementa `PeriodoAquisitivo.dias_vendidos` (`app/ferias/services.py:269`, `app/ferias/services.py:275`). A venda isolada só altera período (`app/ferias/services.py:897`). Frações são apenas os `dias` dos agendamentos em estados agendado/em_gozo/concluido (`app/ferias/situacao.py:115`).

- **Duplicação de insumo, sem divergência atual de estados:** a porta carrega `dias` de agendados/em_gozo/concluidos (`app/ferias/models.py:368`); `_fracoes_do_periodo` repete a seleção (`app/ferias/situacao.py:117`). A porta exclui o próprio PK (`app/ferias/models.py:371`); o aviso oferece exclusão opcional (`app/ferias/situacao.py:120`). O juiz recebe esses números; não reconstrói datas de frações antigas (`app/ferias/models.py:271`).

Grep aplicado à árvore: [nao verificado]

```bash
rg -n 'violacoes_de_fracionamento|FRACAO_MINIMA_DEMAIS|FRACAO_MINIMA|FRACOES_MAXIMAS|checar_fracionamento|checar_ferias_com_abono|avisos_de_agendamento|_fracoes_do_periodo|14 dias|14d|ate 3 fra|até 3 fra|menor.*14|[Ff]racionamento' \
  --glob '*.py' --glob '*.js' --glob '*.html' \
  --glob '!**/tests/**' --glob '!**/test*' --glob '!**/migrations/**' .
```

Os hits pertinentes estão listados acima; hits de “14 dias” sem relação com férias foram descartados pelo contexto dos respectivos arquivos **[não verificado: exaustividade semântica além do grep]**.

## 14/11 10:46 · sessao a81d86ae6c85 · - · - · origem=Ronald (tmux)

Análise contrafactual estática pelo diff fornecido; execução real contra f7510b95a: [não verificado].

| Asserção no teste | Falharia na base? | Motivo | [nao verificado]
|---|---|---|
| `:20`, `[14,4]` | SIM | Base retornaria `[]`; exige `['par1_demais']`. |
| `:21`, `[25,1]` | SIM | Base retornaria `[]`. |
| `:22`, `[4]` | SIM | Base retornaria apenas `['par1']`. |
| `:26`, `[14,5]` | NÃO | Base já retornava `[]`. |
| `:27`, `[20,5,5]` | NÃO | Base já retornava `[]`. |
| `:28`, `[30]` | NÃO | Base já retornava `[]`. |
| `:32`, `[10,10]` | NÃO | Base já retornava `['par1']`. |
| `:33`, `[14,5,5,5]` | NÃO | Base já retornava `['caput']`. |
| `:34`, `[14,3,5,5]` | SIM | Base retornaria apenas `['caput']`. |
| `:41`, ausência da comparação literal | SIM | Base continha exatamente `int(dias) < FRACAO_MINIMA`. |
| `:42`, presença da chamada literal | SIM | Base não continha chamada ao juiz nessa função. |
| `:70–71`, `assertRaises` | SIM | Base aceitaria quatro dias ao lado de quatorze. |
| `:72`, mensagem contém `5 dias` | SIM, isoladamente | Base não produz essa mensagem; normalmente fica inalcançável depois da falha do `assertRaises`. |
| `:77`, aceitação de cinco | NÃO | Ausência implícita de exceção já ocorria. |
| `:83`, aceitação de treze junto ao histórico `[14,3]` | NÃO | Aceitação já ocorria. |
| `:91`, aviso do período contém `5 dias` | SIM | Base não emitia o novo aviso. |
| `:97`, aviso da proposta contém `5 dias` | SIM | Base só emitia o texto referente aos quatorze. |
| `:106`, ausência do texto antigo | SIM | Texto antigo continha a substring. |
| `:107`, presença de “ao menos uma…” | SIM, isoladamente | Texto antigo não continha a substring; execução normalmente para na `:106`. |
| `:108`, presença de “nenhuma…” | SIM, isoladamente | Mesmo motivo. |

Todas as linhas acima pertencem a `app/ponto/tests/test_ferias_fracao_demais_5.py`; o contrafactual usa as remoções do diff anexo e o comportamento atual em `app/ferias/models.py:271`, `app/ferias/situacao.py:348`, `app/templates/ferias/agendar.html:10`.

- **Selo/RED sem mordida — NÃO PROCEDE para o conjunto; PROCEDE como limitação dos selos sintáticos.** Há regressões comportamentais que falhariam na base (`test_ferias_fracao_demais_5.py:20`, `:70`, `:91`, `:97`). As verificações por substring não provam exclusividade do juiz: a presença de uma chamada, inclusive em comentário, satisfaz `:42`; mudança de espaços pode contornar `:41`. (`app/ponto/tests/test_ferias_fracao_demais_5.py:40–42`)

- **Cobertura da porta real — PROCEDE a lacuna.** Os testes chamam `clean()` em objetos novos, usando `bulk_create` só para preparar histórico; não exercitam `save()` nem os serviços reais de edição. (`app/ponto/tests/test_ferias_fracao_demais_5.py:63`, `:69`, `:71`, `:77`, `:83`) A edição muda dias e grava sem `clean()` (`app/ferias/services.py:680`, `:693`); `save()` só executa guarda de sobreposição e espelho (`app/ferias/models.py:461–466`).

- **Status não vivo — PROCEDE a lacuna.** O helper usa exclusivamente `status='concluido'` por padrão; nenhum teste passa `cancelado` ou exercita transição de status. (`app/ponto/tests/test_ferias_fracao_demais_5.py:61–65`)

- **Zero e contagem — PROCEDE a lacuna.** Nenhuma entrada dos testes contém zero; o juiz elimina valores falsos antes de verificar mínimo e quantidade, logo `[14,0] → []` e `[14,0,5,5] → []`. (`app/ferias/models.py:271–278`; entradas em `app/ponto/tests/test_ferias_fracao_demais_5.py:20–34`)

- **Caput junto com par1 — PROCEDE a lacuna.** Nenhum teste verifica aviso simultâneo para `[5,5,5,5]`; o juiz retorna ambas as violações, mas o `elif` suprime o aviso do caput. (`app/ferias/models.py:273–276`; `app/ferias/situacao.py:228`, `:237`; testes em `app/ponto/tests/test_ferias_fracao_demais_5.py:32–34`, `:85–97`)

- **Coerência dias/datas — PROCEDE a lacuna.** O helper sempre cria datas e `dias` coerentes, impedindo que estes testes detectem divergência entre as duas fontes. (`app/ponto/tests/test_ferias_fracao_demais_5.py:63–64`; guarda existente em `app/ferias/models.py:395–398`)

## 14/11 10:46 · sessao 11c8bef045f8 · - · - · origem=Ronald (tmux)

Achados de escritores, todos por inspeção estática:

- **PROCEDE — edição regular contorna o juiz.** A view exige apenas `novos_dias >= 1`, calcula fim inclusivo e chama `remarcar_agendamento` (`app/ferias/views.py:712`, `:718`, `:722`). Essa função delega a `_trocar_janela` (`app/ferias/services.py:705`, `:709`), que muda datas/dias e salva sem `clean`/`full_clean` (`:680`, `:693`). `AgendamentoFerias.save` chama somente guarda de sobreposição, persistência e espelho (`app/ferias/models.py:461`). Assim uma edição para 1–4 dias pode persistir apesar do diff, desde que saldo, status e colisão permitam (`app/ferias/services.py:670`, `:675`, `:693`).

- **PROCEDE — correção admin também contorna o juiz, explicitamente.** A view exige somente duração positiva e documenta que retirou `full_clean` para permitir correções (`app/ferias/views.py:503`, `:508`). Chama `corrigir_datas_agendamento` (`:531`), que usa o mesmo `_trocar_janela` sem `clean` (`app/ferias/services.py:700`, `:702`, `:693`).

- **PROCEDE — mensagem nova diverge da escrita admin.** O aviso novo promete “este agendamento sera recusado” (`app/ferias/situacao.py:358`); a correção admin coleta esse aviso (`app/ferias/views.py:516`), salva (`:531`) e retorna sucesso anexando os avisos (`:549`, `:552`, `:554`). Logo pode devolver “Datas corrigidas…” e “sera recusado” depois de já gravar uma fração curta (`app/ferias/views.py:549`, `:553`).

- **PROCEDE — criação histórica pula art.134 inteiro.** `registrar_gozo_historico` monta dias por diferença inclusiva (`app/ferias/services.py:484`, `:487`), documenta que valida só coerência de datas, sem fracionamento (`:492`), confere datas/saldo/abono (`:495`, `:501`, `:507`) e salva diretamente (`:510`). Portanto pode criar `<5`, quarta fração ou conjunto sem 14, conforme saldo e ausência de colisão (`app/ferias/services.py:492`, `:510`; `app/ferias/models.py:461`).

- **PROCEDE — cancelamento pode desfazer “uma >=14”.** DP e admin devolvem saldo e mudam status para cancelado sem reavaliar as frações restantes (`app/ferias/services.py:615`, `:616`, `:619`, `:620`; `:642`, `:643`, `:651`, `:652`). Exemplo deduzido: cancelar a fração 14 de `[14,5]` deixa `[5]` para o conjunto vivo consultado pelo juiz (`app/ferias/models.py:368`, `:370`).

- **NÃO PROCEDE — reagendar ressuscita cancelado pelo escritor oficial.** `_travar_agendamento` relê status sob trava e recusa os estados informados (`app/ferias/services.py:599`, `:600`). Remarcação usa `RECUSA_ESCRITA`, que contém cancelado (`:670`; `app/ferias/models.py:311`, `:313`). Agendar novamente cria outra linha, excluindo cancelados da duplicidade (`app/ferias/services.py:532`, `:534`, `:544`); não altera a linha cancelada (`:544`).

- **PROCEDE — `clean` não distingue status da própria fração.** O filtro de status se aplica às *outras* linhas, excluindo `self.pk`, mas a própria duração entra incondicionalmente (`app/ferias/models.py:368`, `:371`, `:377`, `:379`, `:387`). Se chamado explicitamente num cancelado, julga também esse cancelado; `save` normalmente não o chama (`:461`).

- **NÃO PROCEDE — segundo escritor de produção fora da porta registrada, no censo por grep.** O registro permite escrita de `AgendamentoFerias` em `ferias/services.py` (`app/core/portas.py:489`). As construções/escritas de produção encontradas estão nesse módulo: histórico `:484`/`:510`, criação normal `:544`, cancelamentos `:620`/`:652`, edição `:693`, aprovação `:739`/`:750`, confirmação `:854`/`:872`/`:876`, cron `:920`. Isso **não** elimina o achado de bypass de `clean`, pois a porta de serviços não exige esse método em todos esses caminhos (`app/ferias/models.py:461`; `app/ferias/services.py:693`).

- **Criação normal e aprovação leem o juiz.** `executar_agendamento` trava PA antes de `_validar_agendamento` e só depois cria (`app/ferias/services.py:531`, `:541`, `:544`); a validação delega a `checar_fracionamento` (`app/ferias/views.py:257`, `:259`), que monta objeto e chama `clean` (`app/ferias/services.py:293`, `:294`). Aprovação chama `full_clean` antes de salvar (`:749`, `:750`). Chat/IA usa o escritor registrado, sem escrita própria (`app/core/views_ia.py:684`).

- **Admin/import/update/bulk.** O admin local registra apenas `Afastamento` (`app/ferias/admin.py:2`, `:4`). Grep de produção não encontrou `AgendamentoFerias.objects.bulk_create/bulk_update/update/update_or_create`, tabela SQL `ferias_agendamentoferias` ou registro admin de AG; cobertura de mecanismos externos/dinâmicos e importações executadas fora da cópia: **[não verificado]**. O teste do diff usa `bulk_create` para injetar histórico (`app/ponto/tests/test_ferias_fracao_demais_5.py:70`, `:83`, `:90`).

- **Cron escreve sem `clean`, mas só status.** Command lê data na borda e chama serviço (`app/ferias/management/commands/sincronizar_status_ferias.py:27`, `:49`); serviço trava AG, recalcula pelo juiz de status e salva apenas status (`app/ferias/services.py:916`, `:917`, `:920`).

- **Ordem de trava: NÃO PROCEDE como alteração introduzida pelo diff.** Criação trava PA (`app/ferias/services.py:531`); edição/cancelamento travam AG → PA (`:599`, `:602`), antes da escrita/ausências (`:688`, `:693`). O diff não altera esses pontos. Deadlock real em produção: **[não verificado]**.

- **Relógio fora da borda: PROCEDE na árvore preexistente, sem relação com predicado novo.** Cancelamentos chamam `periodo.aplicar_status()` sem dia (`app/ferias/services.py:617`, `:644`), admin carimba `date.today()` dentro do serviço (`:649`), resposta carimba `timezone.now()` dentro do serviço (`:875`), venda chama `aplicar_status()` sem dia (`:898`). `PeriodoAquisitivo.aplicar_status` recebe dia opcional e o passa ao juiz (`app/ferias/models.py:207`, `:215`). O juiz puro de fracionamento não precisa de relógio (`app/ferias/models.py:265`).

Grep utilizado: `rg -n --glob '*.py' --glob '!**/test*' --glob '!**/migrations/**' '\b(AgendamentoFerias|_AgFer)\b|agendamentos\.(create|update|bulk_create)' .`, complementado por `ferias_agendamentoferias|agendamentoferias|agendamentos\.bulk_|AgendamentoFerias\.objects\.(bulk_create|bulk_update|update|update_or_create)|admin\.site\.register.*Agendamento`; resultados materiais citados acima. Ausência de acesso a banco/log e ausência de execução de testes: **[não verificado]**. [nao verificado]

## 14/11 10:45 · sessao 818416232d8a · - · - · origem=Ronald (tmux)

- **Selo/RED sem mordida — NÃO PROCEDE quanto à lógica do caso 09.** A fixture põe `batidas=[S]`, `entradas=[]`, `retratadas=[]`; o teste exige `◀ 07:06`, que a guarda antiga do anexo impede de renderizar. A nova guarda abre o laço já existente. (`app/colaboradores/tests/test_cal09_dia_so_com_saidas.py:22`, `:28`, `:41`; `app/templates/colaboradores/partials/_calendario_grade.html:122`, `:125`). Execução real no pai `f7510b95a` e RED cronologicamente anterior à cura: **[não verificado]**. O ambiente não oferece `django.template`, e a cópia não contém metadados Git **[não verificado]**.

- **“O teste das duas cascas prova o grafo real completo” — NÃO PROCEDE; lacuna do selo PROCEDE.** Ele apenas verifica se o caminho aparece no texto-fonte de dois templates; não renderiza as raízes, valida contextos nem percorre includes. (`app/colaboradores/tests/test_cal09_dia_so_com_saidas.py:54`). Há terceira inclusão direta, fora da lista: `relatorios/furos.html`, com `dias=card.cal.dias` e `only`. (`app/templates/relatorios/furos.html:42`). As outras duas inclusões efetivas estão em `app/templates/colaboradores/calendario_partial.html:62` e `app/templates/colaboradores/partials/_drawer_diagnostico.html:35`.

- **Segundo juiz duplicado — NÃO PROCEDE no diff.** A alteração troca a condição de visibilidade pela própria lista que o laço desenha; não redefine status, geometria ou oferta de Resolver dia. (`app/templates/colaboradores/partials/_calendario_grade.html:122`, `:125`, `:129`). Resolver dia continua lendo `dia.resolver_dia_oferecido`, cuja autoridade registrada é `calendario.py::resolver_dia_oferecido`. (`app/core/juizes.py:725`, `:739`).

- **Segundo escritor fora da porta — NÃO PROCEDE no diff.** A mudança de execução é leitura de `dia.batidas`; a escrita adicionada é apenas construção da fixture de teste. (`app/templates/colaboradores/partials/_calendario_grade.html:122`; `app/colaboradores/tests/test_cal09_dia_so_com_saidas.py:28`). A lei exige escritor canônico de **estado**, não proíbe fixtures locais. (`CLAUDE.md:32`).

- **Relógio fora da borda — NÃO PROCEDE como introdução do diff.** A guarda nova não consulta relógio. (`app/templates/colaboradores/partials/_calendario_grade.html:122`). As leituras existentes de `timezone.now()` ficam na montagem do contexto, em `app/colaboradores/services/calendario.py:163` e `:250`; sua simples existência não demonstra leitura fora da borda. Julgamento completo dessa fronteira: **[não verificado]**.

- **Derivação paralela da fonte única — NÃO PROCEDE no diff.** O bloco passa a consultar diretamente a lista que já desenhava, sem adicionar filtragem ou cálculo paralelo. (`app/templates/colaboradores/partials/_calendario_grade.html:122`, `:125`). Isso corresponde à lei “testemunha lê”. (`CLAUDE.md:20`).

- **Ordem de trava — NÃO PROCEDE no diff.** A alteração executável é uma condição de template, e os testes usam `SimpleTestCase` com objetos locais; não foi acrescentada aquisição de trava. (`app/templates/colaboradores/partials/_calendario_grade.html:122`; `app/colaboradores/tests/test_cal09_dia_so_com_saidas.py:24`, `:36`).

- **Pouso antes do pré-requisito — [não verificado].** A lei exige suíte verde antes do opus e proíbe pular o opus final; o material fornecido não demonstra cronologia de suíte, opus ou pouso. (`CLAUDE.md:377`). Não há base para afirmar infração histórica nem cumprimento. **[não verificado]** [nao verificado]

- **Cobertura adicional não demonstrada.** O selo cobre saída isolada, entrada e ausência de marcação, mas não inclui retratada isolada, saída órfã, turno atravessando meia-noite, fuso ou contexto real de `relatorios/furos.html`. (`app/colaboradores/tests/test_cal09_dia_so_com_saidas.py:41`, `:45`, `:49`, `:54`; `app/templates/relatorios/furos.html:42`). Isso limita a prova, sem retirar a mordida lógica específica da saída isolada. (`app/colaboradores/tests/test_cal09_dia_so_com_saidas.py:41`).

## 14/11 10:45 · sessao 198cbbbb991a · - · - · origem=Ronald (tmux)

- **NAO PROCEDE — produtor sem `batidas` ou significado diferente entre rotas reais.** Os três caminhos convergem para `contexto_calendario`: perfil/admin em `app/colaboradores/views.py:1948` e `:1949`, render em `:1965`; drawer em `app/colaboradores/services/drawer.py:120` e `:121`, entregue como `cal` em `:153`; auditoria de espelhos em `app/relatorios/furos.py:160`. Os includes leem os mesmos dias em `app/templates/colaboradores/calendario_partial.html:62`, `app/templates/colaboradores/partials/_drawer_diagnostico.html:35` e `app/templates/relatorios/furos.html:42`.

- **Escritor único do dict real:** `batidas` em `app/colaboradores/services/calendario.py:729`; `retratadas` em `:730`; `entradas` em `:740`. A lista viva nasce do achatamento dos turnos em `:610` e `:611`, filtrada por `retratada_em is None` em `:621`; entradas são o subconjunto `tipo == 'E'` em `:658`. Retratadas são consultadas separadamente em `:239`, agrupadas por data local em `:244` e selecionadas para a célula em `:640`. O `batidas` de `app/colaboradores/services/drawer.py:147` é contexto superior; a grade recebe `cal.dias`, conforme `app/templates/colaboradores/partials/_drawer_diagnostico.html:35`.

- **PROCEDE — ambiguidade “dia” civil × turno no argumento da cura.** `batidas` e `entradas` pertencem ao **dia do turno**, pois a carga é agrupada por `t.data_turno` em `app/colaboradores/services/calendario.py:223` e `:224`; `retratadas` pertence ao **dia civil local**, conforme `:244`. O comentário novo chama `batidas` de “toda marcação viva do dia” sem explicitar o eixo em `app/templates/colaboradores/partials/_calendario_grade.html:120`. A distinção entre as listas já existe no produtor; a mudança do gate não altera essa classificação (`app/colaboradores/services/calendario.py:224`, `:244`, `:621`).

- **NAO PROCEDE como regressão de atribuição de dia causada pelo gate.** A autoridade registrada para o dono da batida é `turnos_do_colab`, em `app/core/juizes.py:208`. O turno pode conter marcação de outro dia civil (`app/ponto/turnos.py:322` e `:324`), e inclusive uma saída órfã pode pertencer à véspera (`:293` e `:298`). O produtor preserva essa decisão (`app/colaboradores/services/calendario.py:224`); o gate novo apenas permite desenhar a lista já atribuída (`app/templates/colaboradores/partials/_calendario_grade.html:122` e `:125`). Portanto, “saída na manhã seguinte aparece na célula do turno anterior” é possível e segue o eixo usado pelo produtor; ocorrência em produção: **[não verificado]**.

- **PROCEDE — terceira raiz omitida pelo teste das duas cascas.** Além das duas raízes enumeradas em `app/colaboradores/tests/test_cal09_dia_so_com_saidas.py:57` e `:58`, `app/templates/relatorios/furos.html:42` inclui a grade com `only`, alimentada pelo mesmo serviço em `app/relatorios/furos.py:160`. A view renderiza essa raiz em `app/relatorios/views.py:1566`. O teste novo, portanto, não prova a completude do grafo real (`app/colaboradores/tests/test_cal09_dia_so_com_saidas.py:57` e `:60`).

- **NAO PROCEDE — novo juiz ou nova derivação de turnos introduzidos pela troca.** O gate lê a mesma lista que o laço desenha (`app/templates/colaboradores/partials/_calendario_grade.html:122` e `:125`); a geometria continua vindo da autoridade registrada (`app/core/juizes.py:208`, `app/colaboradores/services/calendario.py:220` e `:224`). `resolver_dia_oferecido` permanece uma pergunta distinta registrada em `app/core/juizes.py:739` e lida em `app/templates/colaboradores/partials/_calendario_grade.html:129`.

Nenhuma execução contra banco, logs ou código-base histórico foi realizada nesta exploração; resultados de execução e mordida efetivamente observada em `f7510b95a`: **[não verificado]**.

## 14/11 10:45 · sessao a67d27a06dde · - · - · origem=Ronald (tmux)

**NAO PROCEDE quebra de formato nos consumidores nomeados:**

| Caminho | Evidência |
|---|---|
| `relato.sh` | Gera PLACAR com o leitor atualizado e transporta GRADE/PLACAR completos, sem interpretar as linhas novas (`bin/relato.sh:101`, `bin/relato.sh:119`, `bin/relato.sh:120`, `bin/relato.sh:127`, `bin/relato.sh:137`). |
| Raspador | Lê texto inteiro, aplica máscara e escreve o resultado; não interpreta colunas ou contadores da GRADE (`bin/raspar_publico.py:28`, `bin/raspar_publico.py:31`, `bin/raspar_publico.py:40`). |
| Hook Stop | Seus documentos são BACKLOG/RELATO; lê BACKLOG inteiro e recorta topo do RELATO (`bin/hook_stop_fila1.py:33`, `bin/hook_stop_fila1.py:34`, `bin/hook_stop_fila1.py:186`, `bin/hook_stop_fila1.py:472`). |
| Pre-push | Lê stdin do Git, carimbo da régua e labels dos testes; os formatos alterados da GRADE/PLACAR não são interpretados nesses caminhos (`bin/pre-push.sh:8`, `bin/pre-push.sh:53`, `bin/pre-push.sh:184`). |
| Segunda cópia pre-push | Também usa carimbo e labels, sem interpretar GRADE/PLACAR (`app/core/hooks/pre-push.sh:22`, `app/core/hooks/pre-push.sh:80`). |

O selo GRADE é chamado pelo loop `test_*.sh` da régua, que barra retorno diferente de zero; o próprio selo confere a GRADE viva (`bin/regua.sh:138`, `bin/regua.sh:140`, `bin/regua.sh:143`, `bin/tests/test_placar_grade.sh:164`, `bin/tests/test_placar_grade.sh:165`).

**Não atribuir essa execução diretamente ao pre-push:** o código declara que o loop geral de selos não roda nele; as chamadas explícitas são outros selos (`bin/pre-push.sh:14`, `bin/pre-push.sh:16`, `bin/pre-push.sh:19`, `bin/pre-push.sh:38`).

O instalador escreve wrapper que delega ao `bin/pre-push.sh`; instalação efetiva em produção: **[nao verificado]** (`bin/hooks.sh:56`).

`pos_push` atualiza exclusivamente o derivado de TICKETS nesse caminho (`bin/pos_push.sh:19`, `bin/pos_push.sh:20`, `bin/pos_push.sh:29`).

Outros consumidores externos: **[nao verificado]**.

## 14/11 10:45 · sessao cae6ffed01ad · - · - · origem=Ronald (tmux)

**PROCEDE — teste das “duas cascas” não cobre o grafo real.** O teste apenas procura uma substring em duas fontes previamente escolhidas; não renderiza essas raízes nem percorre includes/extends: `app/colaboradores/tests/test_cal09_dia_so_com_saidas.py:54-60`. Há **terceiro include direto** em `app/templates/relatorios/furos.html:42`, com `only`, dentro de página que estende `base.html` (`:1`).

Os três caminhos de produção encontrados são:

- `calendario_ponto` chama `contexto_calendario` e renderiza `calendario_partial`: `app/colaboradores/views.py:1948-1949`, `:1965`; include da grade em `app/templates/colaboradores/calendario_partial.html:62`.
- Drawer chama `contexto_drawer`: `app/colaboradores/views.py:2945-2947`; este chama `contexto_calendario` (`app/colaboradores/services/drawer.py:120-121`) e entrega `cal` (`:153`); include em `app/templates/colaboradores/partials/_drawer_diagnostico.html:33-35`. Quando não há escala posicionável, `cal=None` e a guarda já impede a grade: `app/colaboradores/services/drawer.py:100-104`, template `:33`.
- Relatório recebe `dataset_furos`: `app/relatorios/views.py:1541`; cada card recebe `cal=contexto_calendario(...)`: `app/relatorios/furos.py:160-164`; render do relatório em `app/relatorios/views.py:1566`; include em `app/templates/relatorios/furos.html:42`.

**PROCEDE — descrição “admin e perfil do colab” é enganosa se significar duas cascas de acesso.** A view do calendário declara gestor e exige `ver_cadastro`: `app/colaboradores/views.py:1939-1942`. O teste não verifica hospedeiros, permissões ou casca do colaborador: `app/colaboradores/tests/test_cal09_dia_so_com_saidas.py:54-60`. Há teste anterior que percorre includes/extends e exige `relatorios/furos.html` sob `base.html`, além de ausência sob `base_app.html`: `app/colaboradores/tests/test_calendario_mostra_o_carro.py:224-252`. Outra catraca anterior registra e verifica os três includers e o hospedeiro `colaboradores/detalhe.html`: `app/colaboradores/tests/test_chip_ausencia_duas_linhas.py:442-480`.

**NÃO PROCEDE — leitor órfão de `dia['entradas']`.** A chave continua escrita em `app/colaboradores/services/calendario.py:740`; o teste de caracterização continua lendo-a em `app/colaboradores/tests/test_calendario_caracterizacao.py:28`. No partial, as ocorrências restantes de `entradas` são comentários, não leitura executável: `app/templates/colaboradores/partials/_calendario_grade.html:115`, `:119`. Não encontrei outro leitor do campo em templates, views ou JS [não verificado por execução].

É necessário distinguir o **campo exportado** da **variável local**: `entradas` ainda é derivada em `app/colaboradores/services/calendario.py:658` e consumida para presença geo/entrada principal (`:693-696`) e `houve_trabalho` (`:715`). Os testes `test_cal09_dia_so_com_saidas.py:32`, `test_ui_pilulas_quebram_linha.py:56` e `test_e4cal_l1_r7_grade_le_o_rotulo.py:24` escrevem fixtures, não leem o campo.

## 14/11 10:44 · sessao a56832ce33d6 · - · - · origem=Ronald (tmux)

F2 — Juiz declarado em app/core/juizes.py:302. A resposta executável é: feriado entra na precedência salvo dia previsto cujo vínculo não transforma feriado em folga, em app/ponto/precedencia.py:376-383; a função pública delega ao período em app/ponto/precedencia.py:256. O predicado permite supressão apenas em 6x1, 5x2 e semanal sem trabalha_em_feriado, em app/escala/models.py:32 e app/escala/models.py:1390-1395. O texto “suprime SALVO trabalha_em_feriado” está em app/docs/JUIZES.md:93-94 e app/ponto/precedencia.py:13-17; é menos completo que o código atual, que também preserva 12x36 com flag falsa, conforme app/ponto/precedencia.py:371-377. Não encontrei L-NNN que fixe diretamente esta regra nas ocorrências de feriado de LEIS.md; corte operacional manda consultar fatos_do_dia em app/docs/CORTES.md:360. Candidato externo: app/ponto/management/commands/detectar_ausencias.py:629-630 veta também por cobra_no_dia, cujo critério próprio é posto opera_em_feriado=False e feriado local em app/colaboradores/services/feriado_do_posto.py:70-74. Ressalva importante para decidir a coluna 6: o próprio sítio declara que “posto fecha” é outro fato contratual, mesmo quando o vínculo trabalha no feriado, em app/ponto/management/commands/detectar_ausencias.py:619-622; eu não o classificaria automaticamente como clone da natureza do dia. O helper feriado_vira_folga não é independente: o juiz o chama expressamente em app/ponto/precedencia.py:377.

F3 — Juiz declarado em app/core/juizes.py:303. Regra executável: somente 12x36 integra CICLOS_FERIADO_SIMPLES, em app/ponto/motor_calculo_v2.py:3623; a fábrica aplica ciclo não pertencente à lista como feriado_em_dobra, em app/ponto/motor_calculo_v2.py:3693-3694. Há exceção por cláusula cadastrada documentada em app/ponto/motor_calculo_v2.py:3633-3635 e aplicada em app/core/regua_cct.py:477-478, mas este último sítio já está entre os PENDENTES em app/core/juizes.py:486. A dobra exige flag e feriado em app/ponto/motor_calculo_v2.py:934. Conflito textual inequívoco: L-086 começa afirmando que feriado previsto nunca dobra, em app/docs/LEIS.md:118, enquanto o corte corretivo escrito no juiz diz que feriado não entra nessa proibição e segue o cadastro do ciclo, em app/ponto/motor_calculo_v2.py:917-920. A própria linha app/docs/LEIS.md:118 registra a revogação parcial; não apresentar esse par como lei ainda sem decisão. Outra contradição textual persistente: app/ponto/precedencia.py:375 diz “plantão previsto ... nunca dobra”, enquanto app/ponto/motor_calculo_v2.py:934 não consulta previsão. Candidato externo não declarado: app/ponto/services/fechamento.py:947-955 classifica períodos por feriado_vira_folga e acrescenta os minutos trabalhados a HE100 feriado; ele não está nos sítios de app/core/juizes.py:476-492. Deve ser descrito como regra própria de promoção da rubrica; o comentário explica que o adicional já era 100% e muda a rubrica, em app/ponto/services/fechamento.py:936-942. Não contar app/ponto/calculador/regras.py:302-310 como cópia: importa a lista do juiz.

F4 — SEM_JUIZ em app/core/juizes.py:351-352. L-085 manda atribuir toda jornada ao dia de início, inclusive feriado e cauda após meia-noite, em app/docs/LEIS.md:117. A prática é heterogênea: Motor12x36.calcular_periodo usa a data local da entrada efetiva de cada período, em app/ponto/motor_calculo_v2.py:2759-2760, app/ponto/motor_calculo_v2.py:2796 e app/ponto/motor_calculo_v2.py:2809; MotorComercial usa o mapa do dia da jornada em app/ponto/motor_calculo_v2.py:3325-3327 e julga a dobra por esse dia em app/ponto/motor_calculo_v2.py:3350; MotorTurnoPartido faz equivalente em app/ponto/motor_calculo_v2.py:2942 e app/ponto/motor_calculo_v2.py:3092. Conflito: app/docs/LEIS.md:117 contra data civil de cada período em app/ponto/motor_calculo_v2.py:2809; a auditoria documental já descreve exatamente essa divergência em app/docs/LEIS.md:175. Sítio externo não incluído nos PENDENTES: app/ponto/services/fechamento.py:947-955 decide feriado por data civil de cada entrada para transferir horas a HE100 feriado. Não inferir produção atingida: [não verificado]. Para coluna 7, não achei duas candidatas legais independentes e atuais: há uma lei escrita e implementações divergentes, em app/docs/LEIS.md:117 e app/ponto/motor_calculo_v2.py:2809.

## 14/11 10:43 · sessao f6c292934bd4 · - · - · origem=Ronald (tmux)

Retiro o conflito C6 baseado em app/docs/CORTES.md:342: app/docs/CORTES.md:451 o substitui expressamente. C6 fica com coluna 5 = nao e coluna 7 = -.

O corte novo governa o ato de desligamento. A divergência comprovada está no escritor: app/colaboradores/services/desligamento.py:165 preserva disputa com resposta sem veredito, e app/chamados/models.py:1942 recusa fechá-la; para as demais, a porta ainda grava admin_decidiu, app/chamados/models.py:1974 e app/chamados/models.py:1986. Isso contraria “tudo morre, com ou sem resposta” e “via própria, nunca admin_decidiu”, app/docs/CORTES.md:451.

Não atribuo essa divergência ao predicado C6 nem ao fechamento ordinário C7: o corte determina a transformação pelo ato específico, enquanto os juízes leem o estado recebido, app/chamados/juizes.py:1400 e app/chamados/models.py:1909.

Contagem final do lote 1–8: conflitos na coluna 5 = 0, achados na coluna 6 = 4, perguntas na coluna 7 = 0. Mantém-se C3 externo autorizado como achado, app/chamados/reconciliador.py:1019, sem conflito de resultado demonstrado.

## 14/11 10:43 · sessao ddb4065f7d55 · - · - · origem=Ronald (tmux)

F5 — app/ponto/janelas.py::janela_atual ; a que competencia (corte a corte) pertence o dia? ; L-026 app/docs/LEIS.md:77, L-090 app/docs/LEIS.md:123, K8-COMPETENCIA-NAO-E-MES-CIVIL app/docs/CORTES.md:216 ; janela_atual chama periodo_apuracao_atual com corte_da_empresa, corte limitado a 2–28, inicio no corte do mes anterior e fim no corte-1, competencia avanca quando data.day >= corte (app/ponto/janelas.py:63, app/ponto/janelas.py:26, app/ponto/janelas.py:27, app/ponto/janelas.py:15, app/ponto/janelas.py:16), fallback efetivo 20 para empresa ausente/campo falso (app/ponto/janelas.py:50) ; sim: default 21 escrito em CLAUDE.md:82 e app/ponto/janelas.py:37 contradiz fallback 20 executado em app/ponto/janelas.py:50 ; achado: _jornada_que_mente escolhe periodo_apuracao(hoje.month, hoje.year,21), com corte proprio e mes civil, fora dos PENDENTES F5 (app/escala/services/cadastro_realidade.py:225, app/core/juizes.py:488, app/core/juizes.py:489, app/core/juizes.py:490), achado ja descrito em app/docs/LEIS.md:189 ; -

F6 — app/ponto/janelas.py::competencia_fechada ; a competencia do dia esta fechada (pode escrever)? ; L-026 app/docs/LEIS.md:77, L-090 app/docs/LEIS.md:123 e corte K8 app/docs/CORTES.md:216 respondem competencia da data, predicado aprovado/exportado nao localizado nesses textos [nao verificado] ; data local do timestamp, janela_atual pela empresa, busca FechamentoMensal do colaborador no mes/ano do fim da janela, recusa quando status aprovado/exportado e devolve vazio nos demais casos (app/ponto/janelas.py:84, app/ponto/janelas.py:85, app/ponto/janelas.py:86, app/ponto/janelas.py:87, app/ponto/janelas.py:89) ; nao ; nenhum achado: procurados competencia_fechada, consultas FechamentoMensal e testes de status aprovado/exportado nos .py de app fora tests/migrations, consumidores confirmados chamam juiz em app/ponto/registro_batida.py:135, app/ponto/services/flip_batida.py:22 e app/escala/management/commands/lavrar_foto_folgas.py:95, fronteira com tranca da EMPRESA explicita em app/core/juizes.py:76 ; -

F9 — SEM_JUIZ ; o que e dia util? ; nenhum texto especifico encontrado em LEIS/CORTES [nao verificado] ; VT/VA contam segunda a sexta excluindo feriados obtidos de feriados_de, sem municipio somente nacionais, sem ler escala (app/colaboradores/models.py:996, app/colaboradores/models.py:997, app/colaboradores/models.py:1003, app/colaboradores/models.py:1005), escalonamento considera segunda a sexta entre 08h e 18h sem consultar feriado (app/ponto/management/commands/escalonar_chamados_supervisao.py:24, app/ponto/management/commands/escalonar_chamados_supervisao.py:26) ; sim: descricao seg-sex sem feriado em app/core/juizes.py:353 contradiz exclusao explicita de feriados em app/colaboradores/models.py:1005 ; achado: dias_uteis_do_mes define por conta propria o dia util de VT/VA em app/colaboradores/models.py:1005, consumido para custo por app/colaboradores/models.py:825 e app/colaboradores/models.py:826, esse sitio nao esta no unico PENDENTE F9 declarado em app/core/juizes.py:492 ; -

Observacoes para consolidacao:

- Coluna 7 de F5 ficou “-” por leitura estrita: encontrei apenas default 21 como texto de regra e default 20 no codigo, sem segundo corte/prosa normativo que sustente 20. Se “candidata escrita” incluir implementacao, o par existe em app/ponto/janelas.py:37 e app/ponto/janelas.py:50.
- F9 coluna 5 descreve conflito entre registro e implementacao de sitio SEM_JUIZ. Se o criterio literal limitar texto x codigo apenas ao juiz declarado, marque “nao” e mantenha a divergencia somente na resposta vigente. As duas implementacoes comerciais efetivamente diferem em feriado (app/colaboradores/models.py:1005, app/ponto/management/commands/escalonar_chamados_supervisao.py:26).
- JUIZES.md ainda diz F6 SEM juiz, mas isso e divergencia do registro, nao candidata de regra de negocio (app/docs/JUIZES.md:96, app/docs/JUIZES.md:97, app/core/juizes.py:305).

## 14/11 10:43 · sessao 8d296e4010c4 · - · - · origem=Ronald (tmux)

F7:
- Coluna 3: nenhum texto específico de prazo em LEIS.md/CORTES.md encontrado pela busca de prazo, justificativa, SLA e PRAZO-JUSTIFICATIVA [não verificado]. JUIZES.md:97 diz “prazo do DP ... SEM juiz”, contradizendo o registro app/core/juizes.py:307 e o próprio juiz app/ponto/catalogo/justificativas.py:15.
- Coluna 4: esqueci_entrada/esqueci_saida 1 dia, batida_errada/batida_duplicada 2 dias, hora_extra/banco_horas/folga_trabalhada/troca_turno 3 dias, outro 7 dias, demais tipos 3 dias (app/ponto/catalogo/justificativas.py:22, app/ponto/catalogo/justificativas.py:23, app/ponto/catalogo/justificativas.py:24, app/ponto/catalogo/justificativas.py:26, app/ponto/catalogo/justificativas.py:31). Dias corridos desde a data local de criação, estoura somente quando dias > prazo; usa timezone.localdate() e ignora o argumento hoje (app/ponto/catalogo/justificativas.py:36, app/ponto/catalogo/justificativas.py:41).
- Coluna 5: sim documental quanto à existência de juiz: app/docs/JUIZES.md:97 (“SEM juiz”) x app/ponto/catalogo/justificativas.py:15 (“juiz único”). Não achei dois prazos vigentes contraditórios além do backlog já declarado [não verificado].
- Coluna 6: nenhum achado fora dos PENDENTES, procurando PRAZO_DP, prazo_dp_dias, dias_em_aberto, prazo_estourou e limites fixos de justificativas em Python/templates/JS [não verificado]. A Central chama o juiz em app/ponto/views.py:1729, app/ponto/views.py:1734; o cron também em app/ponto/management/commands/alertar_justificativas_sla.py:10, app/ponto/management/commands/alertar_justificativas_sla.py:20. O backlog fixo de 7 dias está em app/ponto/views.py:1747 e já declarado em app/core/juizes.py:491.
- Coluna 7: “-”. As antigas duas regras (1/2/3/7 x 7 fixos) aparecem como história expressamente resolvida em app/ponto/catalogo/justificativas.py:3 e app/ponto/catalogo/justificativas.py:6.

F8:
- Coluna 3: nenhum texto específico com o número 30 em LEIS.md/CORTES.md encontrado [não verificado]. CORTES.md:348, item 4, preserva “arquivamento por chamado com prazo”, sem definir duração. O mandato 05/08 com 30 dias está escrito no código, app/chamados/catalogo/motor.py:84.
- Coluna 4: 30 dias, app/chamados/catalogo/motor.py:84; a data padrão é agora + essa constante, app/chamados/services/arquivo.py:117 e app/chamados/services/arquivo.py:118.
- Coluna 5: não quanto ao número: constante 30 em app/chamados/catalogo/motor.py:84 e texto de tela 30 em app/templates/chamados/partials/_lista_chamados.html:116 coincidem. Os horizontes excepcionais de 10/5 anos estão expressamente separados do prazo padrão em app/chamados/services/arquivo.py:112 e app/chamados/services/arquivo.py:114.
- Coluna 6: achado: o leitor de tela responde por literal “Arquivar por 30 dias?” em app/templates/chamados/partials/_lista_chamados.html:116, sem consultar o juiz; outro texto independente “Silenciado por idade (30d)” em app/chamados/views_arquivar.py:35. Nenhum dos dois consta dos PENDENTES de feriado/prazo (app/core/juizes.py:475, app/core/juizes.py:492). São textos próprios, embora o valor atual coincida com o juiz (app/chamados/catalogo/motor.py:84). A escrita efetiva da view consulta arquivo.prazo_padrao(), app/chamados/views_arquivar.py:36.
- Coluna 7: “-”. As duas fontes acima dizem o mesmo número, app/chamados/catalogo/motor.py:84 e app/templates/chamados/partials/_lista_chamados.html:116.

Cuidado: app/chamados/management/commands/arquivar_prescritos.py:32 tem default de idade 30 e app/chamados/management/commands/arquivar_prescritos.py:48 calcula o corte para selecionar quem arquivar; a duração do silêncio consulta o juiz em app/chamados/management/commands/arquivar_prescritos.py:84. Não classificaria esse corte de idade como duplicação da duração F8.

## 14/11 10:43 · sessao 14514d383bd7 · - · - · origem=Ronald (tmux)

app/chamados/catalogo/motor.py::prazo_estourou ; o prazo (SLA) do chamado estourou? (app/core/juizes.py:115) ; nenhum texto específico em LEIS/CORTES sobre a comparação do prazo, endereço documentado em app/docs/JUIZES.md:63 ; prazo presente e prazo_sla <= agora, sem prazo False (app/chamados/catalogo/motor.py:56) ; nao ; sla.vencido calcula referência DP/criação + horas por urgência e compara < (app/chamados/services/sla.py:30, app/chamados/services/sla.py:39), cobranca._cobranca_vencida usa prazo ou criação+30min e < (app/chamados/services/cobranca.py:253, app/chamados/services/cobranca.py:254) ; -

app/escala/services/regeneracao.py::cobranca_orfa ; a cobranca ficou orfa depois da regeneracao? (app/core/juizes.py:116) ; L-020 app/docs/LEIS.md:71, texto específico da órfã app/docs/JUIZES.md:41 ; sem marcos_faltantes ou dia retorna None, senão instante de reescrita posterior à criação, combinando regenerada_em/editada_em e janela histórica na ausência de regenerada_em (app/escala/services/regeneracao.py:149, app/escala/services/regeneracao.py:154, app/escala/services/regeneracao.py:119, app/escala/services/regeneracao.py:122) ; nao ; nenhum achado, procurados cobranca_orfa, dia_reescrito_apos, regenerada_em, editada_em, criado_em, consumidores conferidos delegam em app/ponto/management/commands/gerar_celulas.py:257 e app/ponto/management/commands/supra_juiz.py:275 ; -

app/chamados/services/silencio_ausencia.py::reconciliar_por_ausencia ; a ausencia do dia suspende, fecha ou levanta a cobranca viva? (app/core/juizes.py:117) ; L-061 app/docs/LEIS.md:95, L-062 app/docs/LEIS.md:96, corte S4 registrado no próprio módulo app/chamados/services/silencio_ausencia.py:1 ; aberta silencia/renova sem sobrepor silêncio vivo alheio, aprovada fecha só com premissa morta e célula não acusadora, demais levantam somente marcador próprio (app/chamados/services/silencio_ausencia.py:78, app/chamados/services/silencio_ausencia.py:94, app/chamados/services/silencio_ausencia.py:113, app/chamados/services/silencio_ausencia.py:42) ; nao ; auditor refaz ausência aberta, marcador e outro silêncio (app/chamados/management/commands/auditar_invariantes_chamados.py:642, app/chamados/management/commands/auditar_invariantes_chamados.py:646, app/chamados/management/commands/auditar_invariantes_chamados.py:648) e acusa aprovada sem conferir lâmpada (app/chamados/management/commands/auditar_invariantes_chamados.py:660) ; -

app/chamados/services/disputa_emissao.py::motivo_fora_da_fabrica ; a fabrica de perguntas alcanca o chamado e se nao por que? (app/core/juizes.py:119) ; L-061 app/docs/LEIS.md:95, FIO-MORTO-E-ESTRUTURAL app/docs/CORTES.md:253, DUAS-RESPOSTAS item 2 app/docs/CORTES.md:433 ; causas para disputa, encerrado, nunca_bateu, pergunta materializada com célula não acusadora, virada resolvida ou categoria fora do mapa, None para marcos nomeados passados ou categoria mapeada (app/chamados/services/disputa_emissao.py:753, app/chamados/services/disputa_emissao.py:758, app/chamados/services/disputa_emissao.py:760, app/chamados/services/disputa_emissao.py:772, app/chamados/services/disputa_emissao.py:783, app/chamados/services/disputa_emissao.py:785) ; nao ; nenhum achado, procurados motivo_fora_da_fabrica, chamado_orfao_e_regularizavel, pergunta_do_chamado_ja_encerrada, categoria_fora_do_mapa, consumidores delegam em app/chamados/services/disputa_emissao.py:796 e app/chamados/services/pergunta_no_app.py:133 ; -

app/chamados/services/pergunta_no_app.py::retencao ; por que chamado vivo de furo nao tem pergunta no app? (app/core/juizes.py:120) ; L-061 app/docs/LEIS.md:95, FIO-MORTO-E-ESTRUTURAL app/docs/CORTES.md:253 ; sem dia retorna sem_dia, hoje/futuro ou presente no app retorna None, depois testa fábrica, ata, parcial na lista, cobertura, pergunta viva em outro chamado, marco já perguntado e fábrica não rodada (app/chamados/services/pergunta_no_app.py:127, app/chamados/services/pergunta_no_app.py:129, app/chamados/services/pergunta_no_app.py:133, app/chamados/services/pergunta_no_app.py:136, app/chamados/services/pergunta_no_app.py:144, app/chamados/services/pergunta_no_app.py:149, app/chamados/services/pergunta_no_app.py:151) ; nao ; nenhum achado, procurados retencao, fabrica_nao_rodou, pergunta_encerrada_marco_apagado, parcial_na_lista_do_admin, consumidores delegam em app/chamados/services/pergunta_no_app.py:201 e app/chamados/services/pergunta_no_app.py:223 ; -

app/chamados/juizes.py::competencia_do_chamado ; qual a competencia do chamado? (app/core/juizes.py:122) ; L-026 app/docs/LEIS.md:77, K8-COMPETENCIA-NAO-E-MES-CIVIL app/docs/CORTES.md:216, DECIDIR-642 documentado em app/core/juizes.py:121 ; fato mais recente entre dia_do_fato, ausência e perguntas vivas, competência por janela_atual(dia,empresa), None sem fato datado (app/chamados/juizes.py:2360, app/chamados/juizes.py:2363, app/chamados/juizes.py:2366, app/chamados/juizes.py:2368, app/chamados/juizes.py:2370, app/chamados/juizes.py:2373) ; nao ; arquivar_competencia_encerrada escolhe dia_do_fato ou criado_em e calcula competência aritmeticamente (app/chamados/management/commands/arquivar_competencia_encerrada.py:111, app/chamados/management/commands/arquivar_competencia_encerrada.py:115, app/chamados/management/commands/arquivar_competencia_encerrada.py:177, app/chamados/management/commands/arquivar_competencia_encerrada.py:233) ; -

app/chamados/juizes.py::fio_vivo ; alguma pergunta desta colecao esta viva? (app/core/juizes.py:125) ; L-002 app/docs/LEIS.md:60, FIO-MORTO-E-ESTRUTURAL app/docs/CORTES.md:64 ; any(pergunta_viva(p) for p in perguntas), vazio False (app/chamados/juizes.py:1513) ; nao ; Command._disputa_viva reduz any(p.viva for p in d.perguntas.all()) fora do juiz (app/chamados/management/commands/arquivar_competencia_encerrada.py:158), p.viva delega C6 (app/chamados/models.py:3431) ; -

app/ponto/services/furos_vetados.py::motivo_do_veto ; este dia veta nascimento ou reabertura automatica de chamado de turno? (app/core/juizes.py:131) ; L-093 app/docs/LEIS.md:111, VETO-EC-NONE + JUIZ-MOTIVO-DO-VETO app/docs/CORTES.md:403 ; vínculo/template ausente ou régua degenerada retorna VETO_DEGENERADA, suspeita VETO_REGUA, demais None (app/ponto/services/furos_vetados.py:40, app/ponto/services/furos_vetados.py:42, app/ponto/services/furos_vetados.py:44) ; nao ; detectar_ausencias reconstrói os dois vetos (app/ponto/management/commands/detectar_ausencias.py:601, app/ponto/management/commands/detectar_ausencias.py:605), reconciliar_grade também (app/ponto/management/commands/reconciliar_grade.py:495, app/ponto/management/commands/reconciliar_grade.py:499), cópias declaradas em app/core/juizes.py:129 ; -

Contagem deste recorte: 8 juízes lidos, 0 conflitos conclusivos, 5 linhas com achado externo, 0 perguntas de lei. Referências dos oito: app/core/juizes.py:285 a app/core/juizes.py:292.

Leituras integrais sem truncamento concluídas: app/docs/JUIZES.md:1 até 127, CLAUDE.md:1 até 460, app/docs/DOSSIES.md:1 até 366 e trecho coletivo de app/docs/CORTES.md:181 até 280. LEIS e demais trechos de CORTES dependem da consolidação das outras leituras.

## 14/11 10:43 · sessao f6c292934bd4 · - · - · origem=Ronald (tmux)

Leitura adicional concluída: CLAUDE.md inteiro, app/docs/DOSSIES.md inteiro, app/docs/JUIZES.md inteiro e app/docs/LEIS.md inteiro com todas as colunas. CORTES.md:1–180 integral, mais conteúdo normativo até :380 antes da redistribuição.

Correções às oito linhas anteriores:

- C3, coluna 3: acrescentar RESPOSTAS-2910-MANHA, app/docs/CORTES.md:357, que nomeia a pergunta “a premissa do chamado de turno morreu?” e aponta a célula por `_absolvido_pela_celula`.
- C3, coluna 6: substituir “nenhum achado” por app/chamados/reconciliador.py::_absolvido_pela_celula, que absolve por veredito não acusador e ausência de marcos pendentes, app/chamados/reconciliador.py:1028 e app/chamados/reconciliador.py:1030. O gate também o consulta, app/chamados/models.py:1162.
- C6, coluna 5: há conflito literal específico: app/docs/CORTES.md:342 afirma que a resposta do colaborador “só perde” para decisão administrativa na célula, enquanto app/chamados/juizes.py:1425 e app/chamados/juizes.py:1429 encerram também por concordância entre resposta e luz sem decisão administrativa. A alteração de 25/10 está escrita em app/chamados/juizes.py:1333, anterior ao corte de 28/10.
- C6, coluna 7, caso os textos normativos nas docstrings contem como candidatas escritas: “A resposta só perde para decisão administrativa na célula, app/docs/CORTES.md:342, ou também encerra por concordância resposta/luz, app/chamados/juizes.py:1333?”

Demais linhas mantidas. Contagem revista deste lote: 8 lidos, 1 conflito literal, 4 com achado na coluna 6, 1 candidata a pergunta na coluna 7, conforme evidências acima.

## 14/11 10:43 · sessao bda1b8e9067f · COERENCIA-DAS-LEIS · censo · origem=Code

app/escala/models.py::EscalaColaborador.eh_dia_trabalho ; o dia e de trabalho para o vinculo? (app/core/juizes.py:184) ; L-020, celula soberana (app/docs/LEIS.md:71), L-022, paridade da foto e None sem foto (app/docs/LEIS.md:73), L-023, foto complementa ou substitui o ciclo (app/docs/LEIS.md:74) ; le trabalha da celula e, sem celula, delega ao calculado (app/escala/models.py:1094), que usa a ancora no 12x36 sem foto do mes (app/escala/models.py:1289) ; sim: sem foto alguma responde None (app/docs/LEIS.md:73) x sem foto do mes, havendo ancora, responde pela paridade (app/escala/models.py:1289) ; gerar_batidas_smoke_motor.py::_trabalha_no_dia decide por ancora, dias da semana e retorno padrao True (app/ponto/management/commands/gerar_batidas_smoke_motor.py:138), execucao em producao [nao verificado] ; sem celula nem foto, responde None (app/docs/LEIS.md:73) ou calcula pela ancora (app/escala/models.py:1289)?

app/escala/utils.py::minutos_previstos_do_dia ; quantos minutos o dia preve? (app/core/juizes.py:185) ; L-024, grade como fonte unica de previsto (app/docs/LEIS.md:75), corte JUIZ-ESCALA-NASCE reafirma essa fonte (app/docs/CORTES.md:224) ; zero sem vinculo, tipo ou trabalho confirmado (app/escala/utils.py:2256), calcula duracao com virada (app/escala/utils.py:2272), desconta pausas e, sem elas, ainda desconta hii/hfi devolvidos pelos marcos (app/escala/utils.py:2305) ; sim: F1 exige 480 descontando pausa do template quando a celula existe com DNA.marcos nulo (app/escala/utils.py:2298) x R4 exige nao descontar pausa inexistente naquele dia, produzindo 540 (app/escala/utils.py:2300) ; montar_grade_prevista_periodo_por_turno soma segmentos E para S por conta propria (app/escala/utils.py:1635) e publica minutos_previstos (app/escala/utils.py:1809) ; celula existente com DNA.marcos nulo desconta pausa do template, 480 (app/escala/utils.py:2298), ou nao desconta, 540 (app/escala/utils.py:2300)?

app/ponto/precedencia.py::fatos_do_dia ; qual fato vence o dia? (app/core/juizes.py:186) ; corte E2-EMISSORES-COMPLEMENTO manda emissores consultarem fatos_do_dia (app/docs/CORTES.md:360), L-089 limita efeito de ausencia ao dia previsto (app/docs/LEIS.md:121), ordem completa em LEIS/CORTES: nenhum texto localizado [nao verificado] ; delega ao periodo de um dia (app/ponto/precedencia.py:256), ordena ferias, afastamento, atestado, feriado, folga, trabalho (app/ponto/precedencia.py:46), mantendo feriado apenas no pagamento quando o plantao previsto nao vira folga (app/ponto/precedencia.py:376) ; sim: folga vence feriado (app/docs/JUIZES.md:107) x feriado vence folga (app/ponto/precedencia.py:46) ; _marcar_ausencias decide ausencia contra folga por predicado proprio (app/escala/utils.py:1210) e sobrescreve tipo_dia como falta ou ausencia (app/escala/utils.py:1214, app/escala/utils.py:1220) ; coexistindo folga e feriado, vence folga (app/docs/JUIZES.md:107) ou feriado (app/ponto/precedencia.py:46)?

app/core/feriados.py::eh_feriado ; o dia e feriado para o colaborador? (app/core/juizes.py:188) ; nenhum texto especifico localizado em LEIS/CORTES [nao verificado] ; consulta feriados_de (app/core/feriados.py:105), cujo alcance aceita nacional sempre, estadual pela UF e municipal pela UF e nome normalizado (app/core/feriados.py:60, app/core/feriados.py:65), sem municipio considera somente nacional (app/core/feriados.py:102) ; nao ; eh_feriado_do_posto decide sozinho por nacional ou igualdade do nome da cidade, sem verificar UF nem delegar ao juiz (app/colaboradores/services/feriado_do_posto.py:55, app/colaboradores/services/feriado_do_posto.py:59) ; -

app/ponto/turnos.py::ausencia_cobre ; uma ausencia cobre o dia para a folha? (app/core/juizes.py:190) ; L-089 limita o efeito financeiro ao dia previsto (app/docs/LEIS.md:121), texto especifico da cobertura geometrica em LEIS/CORTES: nenhum texto localizado [nao verificado] ; para folha exige aprovada (app/ponto/turnos.py:2516), cobre data inicial, intervalo fechado inclusivo ou intervalo aberto somente para TIPOS_EM_ABERTO, escolhendo maior data e maior pk (app/ponto/turnos.py:2554) ; nao ; ausencias_na_janela rederiva status aprovada e intersecao temporal no periodo (app/folha/export.py:87), alimentando minutos de falta (app/folha/export.py:140), embora a expansao dos dias delegue a dias_da_ausencia (app/folha/export.py:69) ; -

app/ponto/turnos.py::ausencia_cobre ; uma ausencia suspende a cobranca do dia? (app/core/juizes.py:191) ; nenhum texto especifico localizado em LEIS/CORTES [nao verificado] ; para cobranca exclui rejeitada (app/ponto/turnos.py:2518), aplica data inicial, intervalo fechado inclusivo ou aberto somente para TIPOS_EM_ABERTO, escolhendo maior data e maior pk (app/ponto/turnos.py:2554) ; nao ; fatos_do_periodo filtra nao rejeitadas e intersecao por conta propria (app/ponto/precedencia.py:273), reavalia a cobertura de cada dia diretamente (app/ponto/precedencia.py:336) e incorpora essas ausencias ao julgamento (app/ponto/precedencia.py:380) ; -

app/escala/servico_jornada.py::classificar_falta ; que marco do dia faltou, furo? (app/core/juizes.py:192) ; L-020, DNA congelado (app/docs/LEIS.md:71), L-073, teto por instante (app/docs/LEIS.md:100), L-087, papel da batida pela ata (app/docs/LEIS.md:119), L-088, furo como marco nao cumprido (app/docs/LEIS.md:120) ; cala em folga, ausencia inteira, noturno completo e antes do fim previsto (app/escala/servico_jornada.py:310, app/escala/servico_jornada.py:352, app/escala/servico_jornada.py:364, app/escala/servico_jornada.py:388), aplica veto posicional e depois decide entrada ou saida faltante por presenca e contagem de tipos E/S (app/escala/servico_jornada.py:457, app/escala/servico_jornada.py:465, app/escala/servico_jornada.py:480) ; sim: papel E/S vem da ata, tipo gravado nao decide (app/docs/LEIS.md:119) x furo decidido contando b.tipo (app/escala/servico_jornada.py:480) ; _match_marcos deriva quais marcos ficam missing por alinhamento proprio, fora de classificar_falta (app/escala/utils.py:613, app/escala/utils.py:618, app/escala/utils.py:656, app/escala/utils.py:659) ; o marco faltante vem do papel da ata contra o DNA (app/docs/LEIS.md:119) ou da contagem do tipo gravado (app/escala/servico_jornada.py:480)?

app/ponto/turnos.py::realizado_do_dia ; quantos minutos o dia realizou? (app/core/juizes.py:194) ; L-102, sem turno pareado recebe palavra sem numero (app/docs/LEIS.md:128), L-103, fechamento aplica zero declarado ao realizado sem numero (app/docs/LEIS.md:129), L-115, borda posicional depois de pausa fechada (app/docs/LEIS.md:151) ; carrega turnos e intervalo e delega ao corpo puro (app/ponto/turnos.py:779, app/ponto/turnos.py:795), sem turno retorna minutos None (app/ponto/turnos.py:721), com turnos soma bordas fechadas menos pausas batidas ou janela fixa declarada, incluindo excecao posicional (app/ponto/turnos.py:725, app/ponto/turnos.py:735, app/ponto/turnos.py:756, app/ponto/turnos.py:770) ; sim, divergencia historica expressamente superada: numero da ata rotulado (app/docs/DOSSIES.md:142) x sem numero, com alteracao expressa do texto anterior (app/docs/LEIS.md:128) ; minutos_do_oraculo possui corte e soma independentes dos pares por dia (app/ponto/calculador/nucleo.py:53, app/ponto/calculador/nucleo.py:247), independencia declarada para comparacao com minutos_realizados (app/config/crons.py:1055) ; -

app/ponto/status_dia.py::status_do_dia_lavrado ; qual o status do dia? (app/core/juizes.py:199) ; L-039, falta decidida (app/docs/LEIS.md:87), L-061, consumidor le a lampada (app/docs/LEIS.md:95), corte E3-STATUS-JUIZ, status lavrado ou andamento antes do encerramento (app/docs/CORTES.md:415) ; avulso delega ao lote (app/ponto/status_dia.py:218), andamento retorna aberto (app/ponto/status_dia.py:280), encerrado com lavra retorna a coluna (app/ponto/status_dia.py:287), com batida sem lavra retorna sem_certidao (app/ponto/status_dia.py:292), demais ramos consultam precedencia para falta, ausencia, folga, previsto, sem_certidao ou sem_dados (app/ponto/status_dia.py:308) ; nao ; nenhum achado nas buscas status_dia, status_do_dia, atribuicoes de status e retornos irregular, folga_trabalhada e sem_certidao, calendario, app e espelho delegam (app/colaboradores/services/calendario.py:606, app/api/views.py:1483, app/ponto/services/espelho.py:170), exaustividade alem dessas buscas [nao verificado] ; -

app/ponto/precedencia.py::folga_lancada_do_dia ; este dia tem folga lancada que vale para o colaborador? (app/core/juizes.py:204) ; corte GRADE-FOLGA-JUIZ, calendario pertence ao vinculo e folga manual pertence a pessoa (app/docs/CORTES.md:417), L-NNN especifica: nenhum texto localizado [nao verificado] ; retorna primeira candidata aceita (app/ponto/precedencia.py:216), None nao vale, folga manual vale sempre e folga de calendario exige o mesmo vinculo do dia (app/ponto/precedencia.py:235, app/ponto/precedencia.py:237, app/ponto/precedencia.py:239) ; nao ; C5 de auditar_invariantes_chamados aceita FolgaDia quando o vinculo original ou outro vinculo cobre a data, sem distinguir calendario_origem (app/chamados/management/commands/auditar_invariantes_chamados.py:1536), comportamento reconhecido como mesma classe fora da catraca (app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:19) ; -

Contagem: juizes lidos = 10/10 (app/core/juizes.py:183), PENDENTES da familia vazio (app/core/juizes.py:386). Coluna 5 = sim em 5 linhas, incluindo 1 divergencia historica expressamente superada (app/docs/LEIS.md:73, app/escala/utils.py:2297, app/docs/JUIZES.md:107, app/docs/LEIS.md:119, app/docs/LEIS.md:128). Coluna 6 = achado em 9 linhas, sem achado para status nas buscas descritas, com delegacao conferida em app/ponto/services/espelho.py:170. Coluna 7 preenchida em 4 linhas (app/docs/LEIS.md:73, app/escala/utils.py:2297, app/docs/JUIZES.md:107, app/docs/LEIS.md:119).

_5100 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
