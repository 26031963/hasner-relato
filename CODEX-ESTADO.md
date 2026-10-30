# CODEX-ESTADO

> Ultima resposta processada: **30/10/2026 19:18** · publicado em **30/10/2026 19:20** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 30/10 19:18 · sessao 143cd6819825 · O261-V-FASE · sonda · origem=Code

- Vínculos ativos: `12x36 = 337`; `24x48` não consta na saída. [nao verificado]
- Detector `fora_de_fase`: `n=10`, `universo=337`. [nao verificado]
- Achados por ciclo: `{'12x36': 10}`. [nao verificado]
- Achados `24x48`: **0**; `colab_ids []`. [nao verificado]
- Todos os `colab_ids`: `[300, 366, 451, 696, 788, 978, 979, 982, 986, 998]`. [nao verificado]
- A saída fornecida não exibe mensagem de erro. [nao verificado]

## 30/10 19:15 · sessao 507013464189 · E1-BATERIA · red · origem=Code

Criado somente [test_e1_lote2_status_do_dia.py](/home/ronald/codex_ws/red_e54eae6ed8c4_20261030_190743_37155/app/ponto/tests/test_e1_lote2_status_do_dia.py): **20 casos, sendo 15 executáveis e 5 skips de lei**. Sintaxe validada em memória, sem criar bytecode. Suite Django não executada; RED ainda não evidenciado. [nao verificado]

| ID | Status esperado | Retenção esperada | Regra citada |
|---|---|---|---|
| S01 | `ok` | Não retém | CLT arts. 58 e 74 §2 |
| S02 | SKIP — pendente | Pendente | L-039, L-110 |
| S03 | `previsto` | Não retém | ADR D4; CLAUDE §6 |
| S04 | `aberto` | Não retém | ADR D4; CLAUDE §6 |
| S05 | `folga` | Não retém | CLT art. 67; L-024 |
| S06 | `folga_trabalhada` | Não retém | CLT art. 71 §1; L-024 |
| S07 | `ausencia` | Não retém | Lei 605/49 art. 6 §§1–2; L-089 |
| S08 | `falta` | Não retém | L-039; Lei 605/49 art. 6 |
| S09 | `folga` | Não retém | Lei 605/49 art. 8 |
| S10 | `folga_trabalhada` | Não retém | Lei 605/49 art. 9 |
| S11 | `irregular` | Não retém | L-033, L-032; Portaria 671 |
| S12 | SKIP — pendente | Pendente | ADR D4; L-033 |
| S13 | `irregular`, acoplamento em SKIP | Pendente | L-084; CLT art. 58 §1 |
| S14 | SKIP — premissa divergente | Pendente | L-084 |
| S15 | `ok` | Não retém | CLT art. 59-A; ADR D3 |
| S16 | `folga` | Não retém | CLT art. 59-A; ADR D3 |
| S17 | `folga` | Não retém | CLT art. 59-A; L-024 |
| S18 | SKIP — mapeamento pendente | Não retém o abono; caso suspenso | ADR: decisão humana prevalece; L-039 |
| S19 | `ok` | Não retém | CLT art. 62; ADR D3 |
| S20 | `sem_dados` | Não retém | L-112; ADR D3; CLT art. 4 |

Perguntas registradas nos skips:

- **S02:** sem decisão humana, usar `irregular`, `escala_suspeita` ou autorizar exceção à L-039 para `falta`? Qual motivo de retenção?
- **S12:** após encerrar, entrada única continua `aberto` ou vira `irregular`? Retém por qual motivo?
- **S13:** atraso registrado gera `irregular` sem retenção, com retenção ou `ok` com desconto apenas na rubrica?
- **S14:** as pontas diferem **exatamente 180 minutos**. Manter os horários, corrigir o cenário para superar 180 ou alterar expressamente o limite?
- **S18:** admin resolve como `justificado` com abono: prevalece `ausencia`, `folga` ou uma exceção explícita para `ok`?

## 30/10 19:12 · sessao 1c0187623ab2 · MEDIDA-PROGRAMA · verificador · origem=Code

**O desenho permite chegar a zero sem eliminar todas as derivações do dia:** já há um classificador de produção que escapa do vocabulário proposto, e VALOR continua restrito aos sete arquivos previamente enumerados. (`app/relatorios/furos.py:55`, `app/relatorios/furos.py:69`, `app/ponto/tests/test_s3_placar_exercicio.py:34`, `app/ponto/tests/test_s3_placar_exercicio.py:305`)

**1. Onde a definição verificável falha**

- **Falso negativo concreto: `relatorios/furos.py::classificar_dia`.** Calcula o estado pela presença e quantidade de marcos `missing`, retornando `sem_marcos`, `furo`, `parcial` ou `ok`. Só `ok` pertence ao vocabulário contratado; portanto não alcança duas palavras. A resposta alimenta os dias apresentados e a contagem de furos. O censo anterior já o identifica como “classificar_dia próprio”, classe C. (`app/relatorios/furos.py:63`, `app/relatorios/furos.py:69`, `app/relatorios/furos.py:70`, `app/relatorios/furos.py:72`, `app/relatorios/furos.py:74`, `app/relatorios/furos.py:75`, `app/relatorios/furos.py:160`, `app/relatorios/furos.py:164`, `app/docs/CENSO_NARNIA.md:66`)

- **A indireção não fica necessariamente coberta pela tabela ou função chamada.** `estado_do_dia` escolhe entre `_VER_ESTADO` e o classificador acima, sem produzir os literais procurados. A tabela fica no módulo; o classificador também escapa. Assim, neste caso existente, nenhum elo garante a detecção prometida. (`app/relatorios/furos.py:24`, `app/relatorios/furos.py:45`, `app/relatorios/furos.py:46`, `app/relatorios/furos.py:47`, `app/relatorios/furos.py:52`)

- **Há outros vocabulários paralelos já existentes.** O painel operacional escolhe `geofence/normal/turno_aberto/encerrado/ausente/folga`, publica o status e monta contadores; só `folga` entra no vocabulário contratado. O registro já reconhece esses contadores como derivação própria. (`app/core/services/painel_op.py:155`, `app/core/services/painel_op.py:160`, `app/core/services/painel_op.py:166`, `app/core/services/painel_op.py:169`, `app/core/services/painel_op.py:172`, `app/core/services/painel_op.py:175`, `app/core/services/painel_op.py:190`, `app/core/services/painel_op.py:200`, `app/core/juizes.py:705`)

- **A catraca por função não detecta necessariamente um sítio novo dentro de função declarada.** `api_espelho_v2` já contém decisões separadas para certidão da grade, dia com batidas e dia sem batidas. Todas viram uma identidade `arquivo::funcao`; acrescentar outra decisão ali pode preservar o mesmo conjunto de identidades. (`app/api/views.py:1514`, `app/api/views.py:1570`, `app/api/views.py:1644`, `app/api/views.py:1650`, `app/api/views.py:1653`)

- **Falso positivo do predicado bruto:** `campos_da_frase` produz `folga/falta` sob escolha, mas preenche o **tipo da intenção**, não o status do dia. Deve ser exclusão por homônimo. Sem o `NAO_E_LEITOR` concreto, afirmar que sobreviveria ao filtro final é [nao verificado]. (`app/core/intake.py:219`, `app/core/intake.py:228`, `app/core/intake.py:232`)

- **VALOR mede uma lista fechada, não descobre leitores novos.** Tanto o teste principal quanto o teste “MONTADOR é UM SÓ” percorrem `LEITORES`. Um arquivo novo fora dessa lista não entra automaticamente. (`app/ponto/tests/test_s3_placar_exercicio.py:305`, `app/ponto/tests/test_s3_placar_exercicio.py:346`, `app/ponto/tests/test_s3_placar_exercicio.py:350`)

- **O detector reutilizado tem uma cegueira sintática real:** não atravessa `BoolOp` no caminho de `getattr`; a forma `(getattr(p, 'minutos_extra_50', 0) or 0) / 60` existe na cópia. Isso demonstra uma limitação do detector, embora esteja no montador autorizado e não prove leitor indevido. (`app/ponto/tests/test_s3_placar_exercicio.py:205`, `app/ponto/services/espelho.py:113`, `app/ponto/services/espelho.py:114`, `app/ponto/tests/test_s3_placar_exercicio.py:58`)

**2. O que já responde à mesma pergunta**

Para **derivação própria de dinheiro**, já existem o detector, o universo e a cobrança de zero da S3. O contrato acerta ao propor transferi-los e manter uma implementação compartilhada; criar outro detector duplicaria essa pergunta. (`app/ponto/tests/test_s3_placar_exercicio.py:14`, `app/ponto/tests/test_s3_placar_exercicio.py:183`, `app/ponto/tests/test_s3_placar_exercicio.py:303`)

Para **sítios de tela que respondem por conta própria**, já existem `PENDENTES['tela']`, `fora_de_autoridade('tela')` e sua publicação como `leitores_narnia`. Esse inventário deve ser reutilizado como registro da dívida; ele não equivale ao novo censo AST, pois conta declarações e verifica impressões textuais. (`app/core/juizes.py:656`, `app/core/juizes.py:690`, `app/core/juizes.py:1415`, `bin/placar_code.sh:484`, `bin/placar_code.sh:490`, `app/core/tests/test_contract_juiz_tela.py:34`)

Há ainda uma imprecisão no cruzamento proposto: `_TT6` pergunta **“o colaborador entra na folha (e por que não)?”**, com autoridade `classificar_export`; “status e contagem” é o motivo de uma entrada específica do calendário. Não é o significado geral de `_TT6`. (`app/core/juizes.py:665`, `app/core/juizes.py:678`, `app/core/juizes.py:809`)

**3. Quem define e quem consome os nomes**

| Nome | Definição/escrita atual | Leituras atuais |
|---|---|---|
| `ROTULO_STATUS` | Dicionário em `app/colaboradores/services/calendario.py:44`. | Calendário: `app/colaboradores/services/calendario.py:901`; minha escala: `app/escala/views.py:36`, `app/escala/views.py:55`; selos: `app/ponto/tests/test_contract_espelho_sem_emoji.py:220`, `app/colaboradores/tests/test_calendario_um_juiz.py:70`. |
| `deriva_dinheiro` | Função em `app/ponto/tests/test_s3_placar_exercicio.py:183`. | Critério b: `app/ponto/tests/test_s3_placar_exercicio.py:306`; casos sintéticos: `app/ponto/tests/test_s3_placar_exercicio.py:312`; montador: `app/ponto/tests/test_s3_placar_exercicio.py:346`, `app/ponto/tests/test_s3_placar_exercicio.py:350`. |
| `LEITORES` | Tupla em `app/ponto/tests/test_s3_placar_exercicio.py:34`. | Critério a: `app/ponto/tests/test_s3_placar_exercicio.py:247`; critério b: `app/ponto/tests/test_s3_placar_exercicio.py:305`; segundo montador: `app/ponto/tests/test_s3_placar_exercicio.py:350`. |
| `DINHEIRO` / `NAO_E_MOTOR` | `app/ponto/tests/test_s3_placar_exercicio.py:163` e `app/ponto/tests/test_s3_placar_exercicio.py:171`. | Via `getattr`: `app/ponto/tests/test_s3_placar_exercicio.py:203`; via atributo, somente `DINHEIRO`: `app/ponto/tests/test_s3_placar_exercicio.py:213`. |
| `gerar_estado.py` / ESTADO | `gerar` incorpora os placares e escreve o destino em `bin/gerar_estado.py:346`, `bin/gerar_estado.py:349`. | Entrada CLI: `bin/gerar_estado.py:355`; chamada pelo relato: `bin/relato.sh:38`; ESTADO entra na publicação: `bin/relato.sh:102`, `bin/relato.sh:115`. |

Mover `LEITORES` exige preservar também seus consumidores do critério **a** e do montador; a dependência não está restrita ao critério **b**. (`app/ponto/tests/test_s3_placar_exercicio.py:247`, `app/ponto/tests/test_s3_placar_exercicio.py:350`)

**4. Carga no host e falhas no ESTADO**

- **Importar os módulos atuais diretamente exige Django.** O selo importa `SimpleTestCase`; o calendário importa Django e modelos. A extração AST do vocabulário e a transferência completa do detector evitam essas dependências; o funcionamento da implementação futura é [nao verificado]. (`app/ponto/tests/test_s3_placar_exercicio.py:30`, `app/colaboradores/services/calendario.py:22`, `app/colaboradores/services/calendario.py:28`)

- **Carregar por `spec_from_file_location` não resolve imports do projeto.** O gerador acrescenta `raiz/bin` ao caminho e carrega o placar por arquivo. O próprio placar registra o precedente de `ModuleNotFoundError` ao importar `core` nesse contexto. (`bin/gerar_estado.py:75`, `bin/gerar_estado.py:77`, `bin/gerar_estado.py:198`, `app/core/placar_estrutural.py:457`, `app/core/placar_estrutural.py:462`)

- **Copiar o molde não garante “ESTADO nunca cai”.** `_placar_estrutural` protege carga e chamadas, mas acessa campos obrigatórios durante a formatação fora do `try`. Outros leitores também são importados e chamados sem proteção. O novo bloco precisa abranger importação, leitura e composição; se a geração falhar, o relato aborta o ciclo. (`bin/gerar_estado.py:74`, `bin/gerar_estado.py:86`, `bin/gerar_estado.py:101`, `bin/gerar_estado.py:215`, `bin/gerar_estado.py:260`, `bin/relato.sh:38`)

- **A publicação existente depende de banco e rede.** Antes de gerar o ESTADO, `relato.sh` exige a extração de nomes, que consulta `Colaborador.objects` via Django/container; depois publica com `git push`. “Sem banco/rede” pode descrever o censo isolado, não essa cadeia. (`bin/relato.sh:32`, `bin/relato.sh:38`, `bin/relato_nomes.sh:11`, `bin/relato_nomes.sh:18`, `bin/relato_nomes.sh:22`, `bin/relato.sh:126`)

O timer declara cinco minutos e o serviço aponta a árvore operacional; timer ativo, execução nessa árvore e duração inferior a dois segundos são [nao verificado]. (`bin/hasner-relato.timer:6`, `bin/hasner-relato.service:14`, `bin/hasner-relato.service:15`)

## 30/10 19:10 · sessao a64a4e6e6de1 · - · - · origem=Ronald (tmux)

Achados principais de STATUS:

- **Falso negativo de status do dia já existente:** `relatorios/furos.py::classificar_dia` calcula `sem_marcos/furo/parcial/ok` por quantidade de `missing`; apenas `ok` pertence ao vocabulário contratado, portanto não atinge duas palavras e escapa. A função é consumida por `estado_do_dia`, cuja resposta chega aos dias e à contagem de furos. Evidência: `app/relatorios/furos.py:55`, `:63`, `:69`, `:70`, `:72`, `:74`, `:75`, `:45`, `:160`, `:164`. O próprio censo anterior chama esse trecho de “classificar_dia próprio”, classe C: `app/docs/CENSO_NARNIA.md:66`.

- **A promessa “a tabela/função chamada é que tem de cair” não fecha:** `_VER_ESTADO` é tabela de módulo, expressamente fora do contador; `estado_do_dia` escolhe entre a tabela e `classificar_dia` sem produzir os literais buscados; e `classificar_dia` escapa pelo vocabulário acima. Evidência: `app/relatorios/furos.py:24`, `:45`, `:46`, `:47`, `:52`, `:55`.

- **Outros vocabulários paralelos já presentes:** `montar_painel_operacional` produz `geofence/normal/turno_aberto/encerrado/ausente/folga`, publica o status e faz seis contadores; somente `folga` pertence ao vocabulário do contrato, portanto escapa. Evidência: `app/core/services/painel_op.py:155`, `:160`, `:166`, `:169`, `:172`, `:175`, `:190`, `:200`. O registro já reconhece os seis contadores como classe C: `app/core/juizes.py:705`. `painel_tv` produz `fora/normal/geofence/aberto`, novamente com somente uma palavra reconhecida: `app/core/views.py:807`, `:816`, `:818`, `:828`, `:835`.

- **Falso positivo bruto, resolvível pela exclusão prometida:** `campos_da_frase` produz `folga/falta` com escolha, mas está preenchendo o tipo de uma intenção de escrita, não o status do dia. Deve entrar como homônimo no `NAO_E_LEITOR`; sem a lista concreta não se pode afirmar que sobreviverá ao filtro final. Evidência: `app/core/intake.py:219`, `:228`, `:232`, `:236`.

- **Derivações reais que a regra encontra:** `_classificar`, agregação `celstat_map` de `contexto_calendario`, `minha_escala`, `api_espelho_v2` e `_status_dia`; não encontrei motivo para chamar esses achados de falso positivo. Evidência: `app/colaboradores/services/calendario.py:125`, `:136`, `:509`, `:511`; `app/escala/views.py:49`, `:55`; `app/api/views.py:1514`, `:1570`, `:1644`, `:1650`; `app/ponto/services/espelho.py:152`, `:158`, `:171`.

- **Granularidade:** `api_espelho_v2` contém derivação de certidão da grade, status de dias com batidas e status de dias sem batidas, mas contará uma função. Assim, uma nova derivação dentro dessa função já declarada pode preservar o mesmo conjunto de identidades. Evidência: `app/api/views.py:1512`, `:1514`, `:1570`, `:1636`, `:1644`, `:1650`, `:1653`, `:1656`.

- **PDF:** a palavra é lida em `app/relatorios/pdf_espelho.py:919`; os dias vêm do espelho em `:592`, `:598`; o compositor comum é reaplicado em `:632`. Isso não prova ausência de toda decisão de estado no PDF: há seleção local de datas de turno aberto em `:613` e supressão da etiqueta `turno aberto` por orfandade em `:956`, `:960`, `:966`. São formas que escapam do contador de literais, mas não justificam afirmar um novo status codificado sem definir melhor a pergunta.

- **Reuso:** o contador existente `fora_de_autoridade('tela')` mede os sítios declarados que ainda respondem por conta própria, e seu nome publicado é `leitores_narnia`: `app/core/juizes.py:655`, `:658`, `:1415`, `:1417`. Não é medição AST dos literais. O tripwire AST específico do PDF resolve aliases, congela dívida e cobra crescimento/entrada podre, mas mede chamadas de cálculo, outra pergunta: `app/relatorios/tests/test_pdf_nao_calcula.py:33`, `:47`, `:62`, `:95`, `:107`.

A execução ad hoc estrita do critério encontrou oito candidatos brutos; nenhum teste `test*.py` fora de `tests/` satisfaz hoje o critério completo [não verificado por arquivo:linha — resultado de execução somente leitura]. [nao verificado]

## 30/10 19:09 · sessao a9a0edaf0a67 · - · - · origem=Ronald (tmux)

Achados de host/ESTADO, somente leitura:

- **A publicação não é independente de banco/Django/rede.** `relato.sh` exige a lista de nomes antes de gerar o ESTADO e sai em erro se ela falhar (`bin/relato.sh:32`, `bin/relato.sh:38`). O extrator consulta `Colaborador.objects` pelo `manage.py` no container e recusa ausência de resultado (`bin/relato_nomes.sh:10`, `bin/relato_nomes.sh:18`, `bin/relato_nomes.sh:22`). Depois ainda publica via `git push` (`bin/relato.sh:126`). A independência prometida só pode ser do **censo isolado**, não dessa cadeia.

- **`spec_from_file_location` não torna dependências importáveis nem remove Django.** O gerador só acrescenta `raiz/bin` ao `sys.path` (`bin/gerar_estado.py:198`) e carrega o módulo puro por caminho (`bin/gerar_estado.py:75`). Importar diretamente o selo S3 puxa Django (`app/ponto/tests/test_s3_placar_exercicio.py:30`); importar calendário puxa Django e modelos (`app/colaboradores/services/calendario.py:22`, `app/colaboradores/services/calendario.py:28`). A extração AST e transferência das funções puras resolve esse obstáculo se forem completas; a implementação futura é [não verificado]. O próprio placar registra o precedente de `ModuleNotFoundError` ao importar `core` nesse carregamento (`app/core/placar_estrutural.py:457`).

- **O molde `_placar_estrutural` não garante sozinho “ESTADO nunca cai”.** A carga e as chamadas ficam no `try` (`bin/gerar_estado.py:74`), mas a renderização acessa campos obrigatórios fora dele (`bin/gerar_estado.py:101`, `bin/gerar_estado.py:115`). Leitores `ler/linha` do corpo de `gerar` também são importados e chamados sem proteção (`bin/gerar_estado.py:215`, `bin/gerar_estado.py:260`, `bin/gerar_estado.py:265`). O novo bloco precisa proteger **importação, leitura e composição**, não apenas a chamada a `censo`; a implementação futura é [não verificado]. Falha do gerador aborta o ciclo (`bin/relato.sh:38`).

- **Fiagem é mais que criar o leitor:** o ESTADO usa uma string com `%s` e uma tupla posicional (`bin/gerar_estado.py:278`, `bin/gerar_estado.py:332`); incluir seção sem atualizar ambos quebra a geração. O arquivo final é gravado por `gerar` (`bin/gerar_estado.py:349`), chamado pelo CLI (`bin/gerar_estado.py:355`) e por `relato.sh` (`bin/relato.sh:38`). Hoje os placares são incorporados em `bin/gerar_estado.py:346`; a integração proposta é [não verificado].

- **Cadência declarada versus execução:** o arquivo de timer programa cinco minutos (`bin/hasner-relato.timer:6`), e o serviço aponta `/home/ronald/saas-hasner/bin/relato.sh` (`bin/hasner-relato.service:15`). Timer instalado/ativo, árvore efetivamente lida, duração inferior a dois segundos e quantidade de arquivos no host são [não verificado].

Precisão adicional dos nomes do contrato:

- `_TT6` não se chama “status/contagem”; pergunta **“o colaborador entra na folha (e por que não)?”** (`app/core/juizes.py:665`) e aponta `folha/export.py::classificar_export` (`app/core/juizes.py:678`).
- `_TT3` pergunta o julgamento do dia (`app/core/juizes.py:662`) e aponta `classificar_falta` (`app/core/juizes.py:675`).
- O contador existente `fora_de_autoridade('tela')` é `len(PENDENTES)` (`app/core/juizes.py:1415`), cuja família cobre leitores de número/estado em sentido mais amplo (`app/core/juizes.py:656`). Seu selo verifica impressão textual presente e tamanho da lista (`app/core/tests/test_contract_juiz_tela.py:34`, `app/core/tests/test_contract_juiz_tela.py:56`); portanto não é o mesmo censo AST proposto.

## 30/10 19:09 · sessao a5775eb684a5 · - · - · origem=Ronald (tmux)

Reuso obrigatório e mapa dos nomes:

- A pergunta VALOR já é o critério (b) do selo S3: “0 leitor com derivação própria de dinheiro”, definido como aritmética sobre atributos de período (`app/ponto/tests/test_s3_placar_exercicio.py:14`), implementado em `deriva_dinheiro` (`:183`) e cobrado em `test_b_nenhum_leitor_tem_derivacao_propria_de_dinheiro` (`:303`). O selo antigo declara expressamente que já não é o placar (`app/ponto/tests/test_s3_leitor_nao_chama_motor.py:4`). Reutilizar o detector e a lista é coerente; outro detector para essa mesma pergunta duplicaria o existente (`app/ponto/tests/test_s3_placar_exercicio.py:303`).

- Escritor de `LEITORES`: atribuição com os sete arquivos em `app/ponto/tests/test_s3_placar_exercicio.py:34`. Leitores: critério (a), `:247`; critério (b), `:305`; fiscalização do segundo montador, `:350`.

- Escritor de `DINHEIRO`: `app/ponto/tests/test_s3_placar_exercicio.py:163`. Leituras: reconhecimento de `getattr`, `:203`; reconhecimento de atributo, `:213`.

- Escritor de `NAO_E_MOTOR`: `app/ponto/tests/test_s3_placar_exercicio.py:171`. Leitura: filtro de `getattr`, `:203`. Não há esse filtro na via `ast.Attribute`, `:213`; atualmente as duas listas têm nomes distintos (`:163`, `:171`).

- Escritor de `deriva_dinheiro`: definição em `app/ponto/tests/test_s3_placar_exercicio.py:183`. Leitores: teste principal `:306`; casos sintéticos `:315`, `:317`, `:319`, `:325`, `:330`, `:358`, `:359`; teste do montador `:346` e `:350`.

Limitações concretas:

- A derivação do VALOR é medida só nos sete caminhos fixos; o teste não descobre novos leitores em `app/` (`app/ponto/tests/test_s3_placar_exercicio.py:305`). O teste chamado “o MONTADOR é UM SÓ” também só confronta o montador previamente nomeado com esses sete arquivos (`:346`, `:350`). Assim, acrescentar outro arquivo de produção não o inclui automaticamente no critério VALOR (`:34`, `:305`).

- O detector reutilizado é cego à forma real `(getattr(p, 'minutos_extra_50', 0) or 0) / 60`: a subida não atravessa `ast.BoolOp` (`app/ponto/tests/test_s3_placar_exercicio.py:205`), e a forma existe em `app/ponto/services/espelho.py:113` e `:114`. O caso que “morde” usa `getattr` direto, sem `or 0` (`app/ponto/tests/test_s3_placar_exercicio.py:324`). Isso prova limitação do detector, embora esses sítios pertençam ao montador excluído, não demonstre sozinho leitor indevido (`app/ponto/tests/test_s3_placar_exercicio.py:58`).

- Outro atributo real convertido em horas, `minutos_noturnos_relogio`, aparece em `app/ponto/services/espelho.py:103`, mas não pertence a `DINHEIRO` (`app/ponto/tests/test_s3_placar_exercicio.py:163`).

- `horas_do_dia` faz derivação do dia sem lavratura (`periodo.minutos_trabalhados / 60`) em `app/ponto/services/espelho.py:136`; a API chama esse helper e publica seu número (`app/api/views.py:1569`, `:1584`). O contrato precisa explicitar a classe desse helper: o código o considera núcleo (`app/api/views.py:1562`), e o selo exclui o arquivo inteiro como montador (`app/ponto/tests/test_s3_placar_exercicio.py:58`). Seria incorreto afirmar sem essa distinção que a API ainda calcula diretamente (`app/api/views.py:1569`).

Carga sem Django:

- Importar o selo inteiro exige Django em nível de módulo (`app/ponto/tests/test_s3_placar_exercicio.py:30`). O detector propriamente dito utiliza AST (`:190`), portanto extrair sua implementação para módulo puro evita essa dependência; importar o arquivo atual diretamente no host sem Django falha nesse import (`:30`).

- Se também transferirem `_raiz` sem adaptar sua semântica, ela sobe exatamente três diretórios a partir de `__file__`, posição própria do arquivo em `ponto/tests/` (`app/ponto/tests/test_s3_placar_exercicio.py:175`); `_fonte` usa esse resultado para resolver os sete caminhos (`:179`). O contrato lista somente detector e constantes para mudança, portanto esse erro é condicional à implementação futura [não verificado].

Verificação local: extraindo apenas AST, constantes e a função, sem importar Django nem escrever arquivos, os sete arquivos retornaram lista vazia; esse resultado corresponde ao domínio fixo do teste, não prova ausência no restante de produção (`app/ponto/tests/test_s3_placar_exercicio.py:305`).

## 30/10 19:07 · sessao 82d9cde7b5f5 · E1-BATERIA · red · origem=Code

Criado apenas [test_e1_lote1_valor_do_dia.py](/home/ronald/codex_ws/red_e54eae6ed8c4_20261030_190156_3863743/app/ponto/tests/test_e1_lote1_valor_do_dia.py), com 20 casos, fixture próprio e porta CCT real.

Sintaxe e estrutura validadas em memória, sem criar `.pyc`. Motor e suíte Django não executados; o RED ainda não foi evidenciado por execução. [nao verificado]

Valores em minutos; **N** indica noturno relógio/legal.

| Caso | Esperados | Regra citada | Origem |
|---|---|---|---|
| C01 | trab 660; N 360/360; extra 0; feriado 0 | CCT 38-d/10; CLT 73 §2 e 59-A | wt-v3 CCT-1 |
| C02 | trab 660; N 480/548,57; extra 0 | Aval 25/09/O211; CLT 73; Súmula 60 II | wt-v3 CCT-2 |
| C03 | trab 660; N 480/548,57; extra 0 | Adendo 05/10; piso de lacuna; CLT 73 | wt-v3 CCT-3, primeira metade |
| C04 | trab 660; N 480/548,57; extra 0 | Adendo 05/10; empate específico não herda | wt-v3 CCT-3, segunda metade |
| C05 | trab 420; N 360/411,43; extra 0; régua `legais` | CLT 73; Súmula 60 II; parecer 29/07 | wt-v3 CCT-4 |
| C06 | **SKIP:** N 360/411,43 ou 240/274,29 | CLT 73 §5; Súmula 60 II | wt-v3 CCT-4b |
| C07 | trab 660; HE feriado 660 | CLT 59-A; parecer 08/09; BUG 91 | wt-v3 CCT-5, primeira ponta |
| C08 | trab 660; HE feriado 0; extra 0 | CLT 59-A; parecer 08/09; BUG 91 | wt-v3 CCT-5, segunda ponta |
| C09 | trab 660; N 360/411,43; intervalo descontado 60; extra 0 | CLT 59-A, 73, 71 §2 e 4 | Novo |
| C10 | trab 660; N 360/411,43; intervalo descontado 60; extra e HE feriado 0 | CLT 59-A, 73 e 71 §2 | Novo |
| C11 | trab 360; N 300/342,86; extra 0 | CLT 73 §1/§2; Súmula 60 | wt-v3 F2 |
| C12 | trab 480; extra, HE100 e HE feriado 480; HE50 0 | Lei 605/49 art.9; Súmula 146 | wt-v3 F10 |
| C13 | trab 540; extra e HE50 60; HE100 0 | CLT 58 caput e 59 §1 | Novo |
| C14 | trab 450; atraso 30; antecipada e extra 0 | CLT 58 §1; Súmula 366 | Novo |
| C15 | atraso, antecipada e extra 0 | CLT 58 §1: tolerância 5/10 | Novo |
| C16 | **SKIP:** intervalo indenizado 30 ou 60 | CLT 71 §4; pergunta V3-BATERIA | Novo |
| C17 | trab 540; intervalo descontado 0 e indenizado 60; extra e HE50 60; HE100 0 | CLT 71 e 59 §1; L-031 | Novo |
| C18 | trab 690; extra e HE50 30; HE100 0 | CLT 59-A, 59 §1 e 71 §2 | Novo |
| C19 | FT 1, com 660 min; HE feriado e extra 0 | Súmula 146; corte 19/09 | wt-v3 F3 |
| C20 | trab 450; antecipada 30; atraso e extra 0 | CLT 58 §1; Súmula 366 | Novo |

As fontes da régua também são afirmadas nos casos CCT executáveis e em C09.

Dúvidas preservadas sem escolher resposta: **C06**, possibilidade de a CCT negar prorrogação noturna fora do 12x36; **C16**, indenização de 30 minutos suprimidos ou 60 integrais. A cl.10 tem lastro local descrito como interpretação; C01 segue o corte fornecido para o 12x36.

## 30/10 19:03 · sessao 0d8d2229d690 · O261-E-CELULA-20 · verificador · origem=Code

**Veredito: fechamento parcial.** A cura da instância velha fecha o caso descrito; permanecem falhas no selo e na nova deduplicação de auditoria. A guarda acrescentada a `abrir` também não cobre todo o pós-estado. ([models.py:206](app/chamados/models.py:206), [contrato:22](app/chamados/tests/test_contract_status_nucleo.py:22), [models.py:2327](app/chamados/models.py:2327), [models.py:2334](app/chamados/models.py:2334))

1. **Cada cura fecha o item declarado?**

| Item | Verificação |
|---|---|
| **1 — instância velha** | **Fecha o cenário sequencial.** Os quatro gates recarregam antes da decisão; o tripwire considera memória **ou banco** antes da mutação. ([models.py:595](app/chamados/models.py:595), [:618](app/chamados/models.py:618), [:676](app/chamados/models.py:676), [:1425](app/chamados/models.py:1425), [:1442](app/chamados/models.py:1442), [:206](app/chamados/models.py:206)) |
| **2 — dedup costura em terminal** | **Fecha o caso simples do `test_m`:** consulta a guarda, preserva o destino recusado e retorna `(ex, False)`. **Parcial para a classe**, pelo escape de célula abaixo. ([models.py:2321](app/chamados/models.py:2321), [:2347](app/chamados/models.py:2347), [teste:269](app/chamados/tests/test_o261e_f1_porta_encerramento.py:269)) |
| **7 — comentário de atomicidade** | **Fecha a correção textual:** agora distingue exceção propagada da falha do pai engolida pelo receiver. ([models.py:1735](app/chamados/models.py:1735), [signals.py:46](app/chamados/signals.py:46)) |
| **8 — dois estados no desligamento** | Continua sendo efeito possível da ordem declarada: disputa fecha, receiver reconcilia o pai, depois a cascata chama `encerrar`. Não é uma cura comportamental deste delta. ([desligamento.py:142](app/colaboradores/services/desligamento.py:142), [signals.py:42](app/chamados/signals.py:42), [desligamento.py:155](app/colaboradores/services/desligamento.py:155)) |
| **9 — `antes` da reata** | **Fecha:** captura o destino real antes da porta e acrescenta `ancora`; o teste confere `None` no caso pelo container. ([models.py:376](app/chamados/models.py:376), [:383](app/chamados/models.py:383), [teste:297](app/chamados/tests/test_o261e_f1_porta_encerramento.py:297)) |
| **Selo** | Corrige a âncora textual única e contém os três mutantes declarados, mas a consolidação enfraquece a anticegueira. ([selo:158](app/chamados/tests/test_o261e_f1_selo_encerramento.py:158), [contrato:22](app/chamados/tests/test_contract_status_nucleo.py:22)) |

2. **Caminhos, chamadores e mordida**

- **PROCEDE — regressão nova da anticegueira.** O contrato exige somente `bool(achados)`. Remover a escrita do núcleo deixa os dois swaps permitidos suficientes para passar; o tripwire também aceita, pois retorna ao encontrar a guarda sem exigir uma escrita posterior. Assim, perdeu-se a exigência específica de encontrar `NUCLEO`, presente no teste removido do patch. ([contrato:22](app/chamados/tests/test_contract_status_nucleo.py:22), [selo:87](app/chamados/tests/test_o261e_f1_selo_encerramento.py:87), [:115](app/chamados/tests/test_o261e_f1_selo_encerramento.py:115), [models.py:304](app/chamados/models.py:304), [:314](app/chamados/models.py:314))

- **PROCEDE — cura parcial do tripwire.** `if (...) and False:` passa porque não é `ast.Constant`; uma escrita antecipada dentro de outro `if` também passa porque só se rejeita `Assign` diretamente no corpo. Remover `or _anterior_no_banco in VIVOS` igualmente passa: a nova proteção do banco não está presa por esse selo. São limites do verificador, sem demonstração de ocorrência na produção. ([selo:107](app/chamados/tests/test_o261e_f1_selo_encerramento.py:107), [models.py:208](app/chamados/models.py:208))

- **Escape preexistente que a cura 2 deixa aberto:** a guarda julga com a célula antiga; depois `abrir` pode substituí-la. Se a antiga absolve e a nova acusa, a costura em terminal pode ser admitida antes de a pergunta tornar-se viva. O C6 distingue exatamente esses dois vereditos. Existência desse estado em produção: **[não verificado]**. ([models.py:2321](app/chamados/models.py:2321), [:2334](app/chamados/models.py:2334), [juizes.py:1054](app/chamados/juizes.py:1054))

- **PROCEDE — perda nova de auditoria:** recusa `A→M`, costura válida `A→B`, nova recusa `B→M`: a segunda recusa encontra a primeira no histórico e não registra o novo `antes=B`. A identidade pesquisada contém pergunta e destino pedido, sem origem ou ciclo. Repetir a mesma entrada deve deduplicar; isso não exige apagar a foto de um fato posterior distinto. ([models.py:2327](app/chamados/models.py:2327), [:2333](app/chamados/models.py:2333), [CLAUDE.md:299](CLAUDE.md:299), [CLAUDE.md:32](CLAUDE.md:32))

- **Chamadores:** a assinatura e o retorno de `abrir` permanecem compatíveis com os seis emissores encontrados. Porém, a nova leitura `.exists()` da auditoria está fora do best-effort de `evento`: erro nessa consulta propaga pelo ramo de recusa. Ocorrência desse erro: **[não verificado]**. ([models.py:2275](app/chamados/models.py:2275), [:2347](app/chamados/models.py:2347), [disputa_emissao.py:389](app/chamados/services/disputa_emissao.py:389), [:770](app/chamados/services/disputa_emissao.py:770), [:1160](app/chamados/services/disputa_emissao.py:1160), [acoes_disputa.py:259](app/chamados/services/acoes_disputa.py:259), [declaracao_texto.py:322](app/chamados/services/declaracao_texto.py:322), [:611](app/chamados/services/declaracao_texto.py:611), [observ.py:105](app/core/observ.py:105))

Não identifiquei segundo escritor novo nesses trechos: o helper recarrega e a persistência continua no núcleo; nascimento/identidade e reapontamento de destino já são portas declaradas. Nascimento com destino terminal continua fora da guarda, expressamente reservado à F2. A leitura do banco também não elimina a janela entre consulta e escrita. ([models.py:618](app/chamados/models.py:618), [:252](app/chamados/models.py:252), [contrato de escritores:60](app/core/tests/test_contract_escritores_estado.py:60), [fio.py:748](app/chamados/services/fio.py:748), [models.py:206](app/chamados/models.py:206))

3. **Quem escreve e quem lê a auditoria**

| Registro | Escritor | Leitores encontrados |
|---|---|---|
| `costura_recusada` | `PerguntaDisputa.abrir` monta ação, motivo e payload; `core.observ.evento` efetua o `create`. ([models.py:2332](app/chamados/models.py:2332), [observ.py:134](app/core/observ.py:134)) | O próprio `abrir` lê para deduplicar; `test_m` lê para contar uma linha. ([models.py:2328](app/chamados/models.py:2328), [teste:286](app/chamados/tests/test_o261e_f1_porta_encerramento.py:286)) |
| `pergunta_reatada` | `_gate_pergunta_viva` monta o novo payload; `evento` persiste. ([models.py:380](app/chamados/models.py:380), [observ.py:136](app/core/observ.py:136)) | `test_n` lê e confere o novo `valor_antes` e a pergunta em `valor_depois`. ([teste:296](app/chamados/tests/test_o261e_f1_porta_encerramento.py:296)) |

A tela genérica de auditoria consulta ambos os registros, mas renderiza descrição e metadados: **não exibe `valor_antes`/`valor_depois`**. Outro consumidor funcional do novo payload: **[não verificado]**. ([views.py:705](app/core/views.py:705), [log_auditoria.html:55](app/templates/core/log_auditoria.html:55))

Execução Django dos REDs/GREENs e confirmação em produção: **[não verificado]**.

## 30/10 19:02 · sessao ac639396587e · - · - · origem=Ronald (tmux)

- **Cura 2 fecha o caso simples:** o dedup consulta `_costura_nao_mata_o_fio`, preserva o destino anterior quando recusado e mantém retorno `(ex, False)`; E4-c continua levantando para divergência de turno. `app/chamados/models.py:2311`, `app/chamados/models.py:2313`, `app/chamados/models.py:2321`, `app/chamados/models.py:2347`.

- **Cura 2 não cobre todo pós-estado:** a guarda roda antes da troca de `celula`. C6 pode considerar a célula antiga absolvente e permitir destino terminal; a troca subsequente para célula acusadora torna a pergunta viva nesse terminal. Prova pura em memória com os corpos AST reais de `abrir`, `_costura_nao_mata_o_fio` e `_pergunta_viva_calc`: `before_viva=False`, `created=False`, `after_viva=True`, destino `resolvido`. É escape preexistente que a cura não fecha; existência desse dado em produção **[não verificado]**. `app/chamados/models.py:2321`, `app/chamados/models.py:2334`, `app/chamados/models.py:2346`, `app/chamados/juizes.py:1054`, `app/chamados/juizes.py:1056`, `app/chamados/juizes.py:1064`.

- **Novo dedup de auditoria abafa fatos posteriores distintos:** sua identidade é somente ação/modelo/pergunta/destino pedido, pesquisada em todo histórico; não inclui origem nem ciclo. Depois de uma recusa A→M, uma costura válida A→B e nova recusa B→M, a segunda foto de origem não é escrita. Prova pura em memória com `abrir` real: duas recusas, uma linha, única foto `antes=10`, origem atual `11`. Isso é perda de auditoria da tentativa posterior; contrato 2 só exige fundir repetição da mesma entrada, e L-007 exige antes/depois de ação crítica. `app/chamados/models.py:2327`, `app/chamados/models.py:2328`, `app/chamados/models.py:2333`, `CLAUDE.md:299`, `app/docs/LEIS.md:65`.

- **Nova dependência de leitura de auditoria:** o `.exists()` de `LogAuditoria` está fora do best-effort de `evento`; falha nessa leitura propaga, embora o `create` da trilha seja engolido. Ocorrência de falha em produção **[não verificado]**. `app/chamados/models.py:2328`, `app/core/observ.py:105`, `app/core/observ.py:133`, `app/core/observ.py:139`.

- **Seis emissores, assinatura preservada:** `gerar_perguntas_disputa` consome `(p,_criada)` e só reata órfã criada; `_criar_pergunta_se_couber` ignora o retorno e declara `True`; âncora de limbo e reabertura administrativa ignoram o retorno; as duas declarações consomem `(perg,_)`. Não há exigência de novo tipo de retorno nesses chamadores. `app/chamados/services/disputa_emissao.py:389`, `app/chamados/services/disputa_emissao.py:404`, `app/chamados/services/disputa_emissao.py:770`, `app/chamados/services/disputa_emissao.py:780`, `app/chamados/services/disputa_emissao.py:1160`, `app/chamados/services/acoes_disputa.py:259`, `app/chamados/services/declaracao_texto.py:322`, `app/chamados/services/declaracao_texto.py:611`, `app/chamados/models.py:2347`.

- **Nascimento com destino/pai terminal continua sem essa guarda, expressamente fora de F1:** o `create` permanece direto; portanto não é caminho novo aberto pelo delta anexado. `app/chamados/models.py:2348`, `app/chamados/services/fio.py:748`.

- **Não nasceu segundo escritor de status neste trecho:** o delta do dedup escreve destino/célula/veredito da pergunta; as duas portas de costura já existiam, e o próprio gate ainda efetua a atribuição. A exclusividade literal reivindicada por `reapontar_resolvedora` já divergia desse dedup preexistente. `app/chamados/models.py:2323`, `app/chamados/models.py:2335`, `app/chamados/models.py:2346`, `app/chamados/services/fio.py:772`, `app/chamados/services/fio.py:828`.

## 30/10 19:01 · sessao 87d817a9ef88 · - · - · origem=Ronald (tmux)

O selo **fecha os três mutantes declarados**, mas permanece parcial e a consolidação introduz uma regressão de anticegueira. [app/chamados/tests/test_o261e_f1_selo_encerramento.py:158](app/chamados/tests/test_o261e_f1_selo_encerramento.py:158), [app/chamados/tests/test_contract_status_nucleo.py:22](app/chamados/tests/test_contract_status_nucleo.py:22)

- **NOVO — anticegueira enfraquecida.** O teste removido no anexo exigia encontrar especificamente `NUCLEO`; agora basta `achados` não vazio. Retirar apenas `models.py:213`, ou trocar por `self.__dict__['status_local'] = novo`, deixa as escritas permitidas de `_vivas_ancoradas` e satisfaz `assertTrue(achados)` e `fora_do_nucleo=[]`. O tripwire também retorna verdadeiro porque encontra a guarda antes de exigir qualquer escrita posterior. [app/chamados/tests/test_contract_status_nucleo.py:22](app/chamados/tests/test_contract_status_nucleo.py:22), [app/chamados/tests/test_o261e_f1_selo_encerramento.py:86](app/chamados/tests/test_o261e_f1_selo_encerramento.py:86), [app/chamados/tests/test_o261e_f1_selo_encerramento.py:115](app/chamados/tests/test_o261e_f1_selo_encerramento.py:115), [app/chamados/models.py:304](app/chamados/models.py:304), [app/chamados/models.py:314](app/chamados/models.py:314)

- **Cura parcial — guarda morta ainda aceita.** `if (...) and False:` não é `ast.Constant`; contém chamada e `raise` e passa. Uma escrita antecipada dentro de `if novo: self.status_local = novo` também passa, pois o detector só rejeita `Assign` diretamente no corpo. São limites estruturais que permanecem, não novo caminho observado na produção. [app/chamados/tests/test_o261e_f1_selo_encerramento.py:107](app/chamados/tests/test_o261e_f1_selo_encerramento.py:107)

- **Nova leitura do banco sem selo próprio.** Substituir a condição atual por `if anterior in VIVOS and novo in TERMINAIS:` mantém chamada e `raise`, satisfazendo o selo apesar de retirar a proteção contra instância velha. [app/chamados/models.py:206](app/chamados/models.py:206), [app/chamados/tests/test_o261e_f1_selo_encerramento.py:111](app/chamados/tests/test_o261e_f1_selo_encerramento.py:111)

- **Consolidação resolve a falsa infração do swap**, mas sua exceção continua baseada apenas em arquivo/nome da função: não verifica classe nem que `_vivas_ancoradas` nunca salva. No código atual essa função consulta, troca em memória e restaura no `finally`; não há `save()` nela. [app/chamados/tests/test_contract_status_nucleo.py:16](app/chamados/tests/test_contract_status_nucleo.py:16), [app/chamados/tests/test_o261e_f1_selo_encerramento.py:27](app/chamados/tests/test_o261e_f1_selo_encerramento.py:27), [app/chamados/tests/test_o261e_f1_selo_encerramento.py:86](app/chamados/tests/test_o261e_f1_selo_encerramento.py:86), [app/chamados/models.py:298](app/chamados/models.py:298)

Executei os seis testes do selo por AST em memória, substituindo somente a importação de `SimpleTestCase` por `unittest.TestCase`; todos passaram. A varredura encontrou somente o escritor e os dois swaps permitidos, e os mutantes acima reproduziram os escapes. Não executei Django nem acessei banco; esses resultados locais não foram registrados em arquivo. [não verificado] [nao verificado]

## 30/10 19:01 · sessao 318dd251f59b · - · - · origem=Ronald (tmux)

Item 1 fecha no cenário sequencial descrito: o helper compara o status persistido e recarrega os campos de estado; os quatro gates o chamam antes da decisão do motor. Não cria segundo escritor: a persistência continua em `_gravar_estado`. (`app/chamados/models.py:618-621`, `:595`, `:676`, `:1425`, `:1442`, `:213`, `:252`)

O tripwire passa a considerar tanto a memória quanto o banco, antes de alterar a instância; uma instância terminal sobre registro vivo já não evita a checagem das perguntas ancoradas. (`app/chamados/models.py:206-213`)

Não encontrei regressão concreta nos caminhos examinados: `cancelar`, `encerrar` e `encerrar_por_competencia_trancada` continuam consultando a guarda antes do escritor, com retorno `False` na recusa. (`app/chamados/models.py:683-686`, `:1431-1434`, `:1449-1454`)

O teste novo cobre diretamente apenas a instância velha em `encerrar`; execução dos testes e cobertura dinâmica dos outros gates e do tripwire com instância velha: [não verificado]. (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:260-267`)

A leitura não torna o fechamento atômico perante concorrência: consulta, guarda e `save` permanecem sem trava de linha; isso é limite remanescente, não prova de ocorrência. (`app/chamados/models.py:206-212`, `:247-252`, `:618-621`)

## 30/10 19:00 · sessao 61de8c571a69 · prova_red_escrita · red · origem=Code

1. Criar `/home/ronald/saas-hasner/INTRUSO_RED_3759812.txt`: código 1.
   ```text
   /bin/bash: line 1: /home/ronald/saas-hasner/INTRUSO_RED_3759812.txt: Read-only file system
   ```

2. Criar `/tmp/INTRUSO_RED_3759812.txt`: código 1.
   ```text
   /bin/bash: line 1: /tmp/INTRUSO_RED_3759812.txt: Read-only file system
   ```

3. Acrescentar `# INTRUSO_RED_3759812` a `app/ponto/models.py`: código 0, saída vazia. A alteração foi permitida.

4. Criar `app/ponto/tests/test_intruso_red_3759812.py`: código 0, saída vazia. Arquivo criado com:
   ```python
   def test_x():
       assert False
   ```

## 30/10 18:57 · sessao 481142c712d0 · O261-E-CELULA-20 · verificador · origem=Code

**VERMELHO para F1:** a guarda pode ser pulada por instância desatualizada; há outro reaponte de pergunta existente que não recebe a proteção; o contrato antigo e o novo teste anti-vacuidade falham. (`app/chamados/models.py:206`, `app/chamados/models.py:356`, `app/chamados/models.py:2300`, `app/chamados/tests/test_contract_status_nucleo.py:28`, `app/chamados/tests/test_o261e_f1_selo_encerramento.py:161`)

Executei somente o código puro dos selos em memória. Execução da suíte Django, contagens reais de queries, incidência em produção e integridade dos dados exportados: **[nao verificado]**. [nao verificado]

**(1) Escritores, leitores e valores**

O censo abaixo agrupa portas e consumidores por função. Nascimento e acervo têm caminhos próprios; o selo novo exclui testes e migrações. (`app/chamados/models.py:854`, `app/chamados/models.py:970`, `app/chamados/tests/test_o261e_f1_selo_encerramento.py:92`)

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `ChamadoColaborador.status_local` | Persistência pelo núcleo `_gravar_estado`; nascimento por `abrir`/`criar`. A novidade também troca e restaura o campo **em memória**. (`app/chamados/models.py:211`, `app/chamados/models.py:250`, `app/chamados/models.py:854`, `app/chamados/models.py:970`, `app/chamados/models.py:302`, `app/chamados/models.py:312`) | Motor das portas; C6; C8; receivers; fila/painel; fio; API e relatórios. (`app/chamados/models.py:118`, `app/chamados/models.py:596`, `app/chamados/juizes.py:700`, `app/chamados/juizes.py:1621`, `app/chamados/signals.py:33`, `app/chamados/signals.py:81`, `app/chamados/services/painel.py:155`, `app/chamados/services/fio.py:385`, `app/api/views.py:1047`, `app/relatorios/pdf_chamados.py:116`) |
| `resolvido_em` | O núcleo carimba ao terminar e limpa ao voltar a vivo; preserva o carimbo numa passagem entre encerrados. (`app/chamados/models.py:226`, `app/chamados/models.py:227`, `app/chamados/models.py:231`) | Guarda de reabertura, indicadores, arquivo, detalhe e vigia de terminal sem carimbo. (`app/chamados/models.py:1214`, `app/chamados/services/painel.py:243`, `app/chamados/views_arquivar.py:157`, `app/templates/chamados/detalhe_local.html:88`, `app/chamados/management/commands/auditar_invariantes_chamados.py:571`) |
| `via_encerramento` | O núcleo grava a via da porta ou limpa no vivo. As correspondências são `RECONCILIAR→sistema`, `ENCERRAR→cascade`, `CANCELAR→cancelado`, humana→`admin`; tranca passa sua via própria. (`app/chamados/models.py:228`, `app/chamados/models.py:232`, `app/chamados/catalogo/motor.py:95`, `app/chamados/models.py:1439`) | Renascimento recusa `cascade`; tranca usa a via para reabrir; S128 não age sobre `competencia_trancada`; arquivo e auditoria também leem. (`app/chamados/models.py:890`, `app/chamados/services/competencia_trancada.py:252`, `app/chamados/services/disputa_emissao.py:1531`, `app/chamados/views_arquivar.py:64`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1067`) |
| `urgencia` | Ritual `_rebaixar_urgencia_em_memoria` escreve `normal`, preservando a original; núcleo restaura; `rebaixar`, normalizador do receiver e `escalar_urgencia` persistem pelos respectivos caminhos. (`app/chamados/models.py:234`, `app/chamados/models.py:406`, `app/chamados/models.py:408`, `app/chamados/models.py:420`, `app/chamados/models.py:555`, `app/chamados/models.py:472`) | SLA, ordenação/contadores, verbos, API e apresentação. (`app/chamados/services/sla.py:30`, `app/chamados/services/painel.py:102`, `app/chamados/services/painel.py:223`, `app/chamados/services/verbos.py:194`, `app/api/views.py:1150`, `app/templates/chamados/partials/_lista_chamados.html:68`) |
| `PerguntaDisputa.chamado_destino` | **Dois caminhos persistentes:** `fio.reapontar_resolvedora` e o dedup de `PerguntaDisputa.abrir`; este último também cria perguntas. Os helpers novos fazem substituições temporárias para julgamento. (`app/chamados/services/fio.py:828`, `app/chamados/services/fio.py:830`, `app/chamados/models.py:2300`, `app/chamados/models.py:2314`, `app/chamados/models.py:2316`, `app/chamados/models.py:308`, `app/chamados/services/fio.py:758`) | Âncora do fio; crédito da mesa/app; C6; irmãs respondidas; busca de disputa; tranca e materialização. (`app/chamados/services/fio.py:116`, `app/chamados/services/raiox.py:60`, `app/api/views.py:1046`, `app/chamados/juizes.py:699`, `app/chamados/juizes.py:1474`, `app/chamados/services/fio.py:135`, `app/chamados/services/competencia_trancada.py:136`, `app/chamados/services/disputa_emissao.py:1537`) |
| `PerguntaDisputa.materializada_em` | Portas do model carimbam/desfazem admin, tranca, fantasma e reaberturas; ressuscitação também limpa. Há escritores no materializador, reconciliador, emissão, dedup e relavra. (`app/chamados/models.py:2363`, `app/chamados/models.py:2377`, `app/chamados/models.py:2402`, `app/chamados/models.py:2417`, `app/chamados/models.py:2437`, `app/chamados/models.py:2447`, `app/chamados/models.py:1377`, `app/chamados/services/materializacao.py:509`, `app/chamados/services/materializacao.py:1043`, `app/chamados/reconciliador.py:402`, `app/chamados/reconciliador.py:617`, `app/chamados/services/disputa_emissao.py:1562`, `app/chamados/services/fio.py:876`, `app/chamados/services/relavra_via.py:231`) | C6/materializada de fato, estado da pergunta, respostas sem veredito, fila de materialização, raio-X e invalidação da lavra. (`app/chamados/juizes.py:603`, `app/chamados/juizes.py:1076`, `app/chamados/models.py:2341`, `app/chamados/models.py:1654`, `app/chamados/services/materializacao.py:1289`, `app/chamados/services/raiox.py:133`, `app/chamados/veredito_lavrado.py:163`) |
| `PerguntaDisputa.via_resolucao` | Acompanha os carimbos acima: admin grava `admin_decidiu`; tranca, fantasma, declaração/ausência, sem-turno, célula absolveu e duplicata têm seus escritores. Relavra pode trocar a via; reaberturas limpam. (`app/chamados/models.py:2362`, `app/chamados/models.py:2376`, `app/chamados/models.py:2401`, `app/chamados/services/materializacao.py:526`, `app/chamados/reconciliador.py:406`, `app/chamados/reconciliador.py:619`, `app/chamados/services/fio.py:875`, `app/chamados/services/relavra_via.py:123`, `app/chamados/models.py:2446`) | C6 lê precedência de tranca/identidade; materializada de fato lê vias sem testemunha; relavra seleciona vias derivadas; UI apresenta a via. (`app/chamados/juizes.py:1015`, `app/chamados/juizes.py:1044`, `app/chamados/juizes.py:603`, `app/chamados/services/relavra_via.py:116`, `app/templates/chamados/partials/_card_atendimento.html:186`) |
| `DisputaSupervisao.fechada_em` | `fechar` carimba; `reabrir_admin` e `reabrir_sistema` limpam. (`app/chamados/models.py:1730`, `app/chamados/models.py:1734`, `app/chamados/models.py:1932`, `app/chamados/models.py:1956`) | Receivers do pai/regularização/materialização; API/fio; validação; reconciliador; cascata; tranca e crons. (`app/chamados/signals.py:28`, `app/chamados/signals.py:106`, `app/chamados/signals.py:130`, `app/api/views.py:1030`, `app/chamados/services/fio.py:594`, `app/chamados/services/validacao.py:199`, `app/chamados/reconciliador.py:635`, `app/colaboradores/services/desligamento.py:139`, `app/chamados/services/competencia_trancada.py:145`, `app/ponto/management/commands/alertar_disputas_pendentes.py:34`) |
| Log `fechamento_recusado` | `_registrar_recusa`, através de `evento`; payload distingue pergunta viva de premissa do dia. (`app/chamados/models.py:347`, `app/chamados/models.py:364`, `app/chamados/models.py:654`) | O próprio dedup lê a última recusa e procura mudança posterior de estado; auditoria genérica permite consulta. (`app/chamados/models.py:342`, `app/chamados/models.py:345`, `app/core/views.py:705`, `app/core/views.py:710`) |
| Log `pergunta_reatada` | `_gate_pergunta_viva`, depois de cada reaponte. (`app/chamados/models.py:374`, `app/chamados/models.py:377`) | Consulta genérica de auditoria; o histórico do chamado não inclui essa ação, pois filtra apenas `estado_chamado`. (`app/core/views.py:705`, `app/chamados/services/historico.py:44`, `app/chamados/services/historico.py:86`) |
| Log `estado_chamado` | `_gravar_estado`, antes do save e no mesmo atomic da escrita. (`app/chamados/models.py:245`, `app/chamados/models.py:246`, `app/chamados/models.py:250`) | Dedup da recusa, histórico, vigia de loops e vigia trilha×banco. (`app/chamados/models.py:345`, `app/chamados/services/historico.py:86`, `app/chamados/services/vigia_loop.py:29`, `app/chamados/services/vigia_loop.py:114`) |

Há escrita histórica de status/via na migration 0039; ela fica fora do selo novo. Aplicação dessa migration nos ambientes: **[nao verificado]**. (`app/chamados/migrations/0039_a3_via_backfill.py:24`, `app/chamados/migrations/0039_a3_via_backfill.py:33`, `app/chamados/tests/test_o261e_f1_selo_encerramento.py:92`)

**(2) Onde falha contra a árvore**

1. **Bypass da guarda por instância desatualizada, sem precisar de concorrência simultânea.** Guarde uma instância `resolvido`; reabra o mesmo registro por outra instância; acrescente uma pergunta viva; chame `encerrar()` na primeira. O motor recebe o status antigo, `_gate_pergunta_viva` retorna cedo e o tripwire também não roda. Só `reconciliar()` relê o status persistido. (`app/chamados/catalogo/motor.py:102`, `app/chamados/models.py:1412`, `app/chamados/models.py:356`, `app/chamados/models.py:206`, `app/chamados/models.py:592`)

   Reproduzi esse desvio com os métodos extraídos e persistência simulada em memória: `encerrar()` devolve `True` sem consultar `_vivas_ancoradas`. Reprodução com ORM/banco: **[nao verificado]**. O teste novo exige que terminal→terminal passe com pergunta ancorada, portanto não cobre o caso em que apenas a instância está terminal. (`app/chamados/models.py:356`, `app/chamados/models.py:206`, `app/chamados/tests/test_o261e_f1_porta_encerramento.py:205`)

2. **A nova costura protege apenas um dos escritores existentes.** `PerguntaDisputa.abrir()` encontra `ex`, verifica E4-c, troca `ex.chamado_destino`, relavra e salva. Esse ramo pode reapontar uma pergunta existente para terminal sem `_costura_nao_mata_o_fio`; não cabe na exceção documentada como “nascimento para F2”. (`app/chamados/models.py:2292`, `app/chamados/models.py:2297`, `app/chamados/models.py:2300`, `app/chamados/models.py:2314`, `app/chamados/services/fio.py:748`, `app/chamados/services/fio.py:823`)

3. **Âncora e crédito não significam a mesma coisa na árvore.** O filtro novo cai no pai somente se o destino for `NULL`, reproduzindo `chamado_que_lista`. Porém mesa e app creditam ao pai também quando o destino está morto; o juiz das respostas pendentes lê esse mesmo crédito. (`app/chamados/models.py:297`, `app/chamados/services/fio.py:117`, `app/chamados/services/raiox.py:61`, `app/chamados/services/raiox.py:64`, `app/api/views.py:1047`, `app/chamados/juizes.py:1474`)

   Consequência estática: pai vivo de `disputa_supervisao`, destino terminal e pergunta muda sem materialização podem perder o último chamado visível quando o pai reconcilia: a guarda não coleta essa pergunta, e C6 continua dando-a por viva. É uma divergência preexistente de consumidores que F1 não cobre; ocorrência no dado de produção: **[nao verificado]**. (`app/chamados/models.py:297`, `app/chamados/models.py:601`, `app/chamados/models.py:621`, `app/chamados/juizes.py:1076`, `app/api/views.py:1047`)

4. **Há chamadores que presumem sucesso de `encerrar()`.** A troca de vínculo ignora a recusa e cria outro aceite; regularização externa já grava materialização/nota, ignora a recusa e retorna `materializada=True`. Isso permite concluir o ato mantendo o chamado anterior vivo; não prova erro na materialização em si. (`app/colaboradores/services/vinculo.py:387`, `app/colaboradores/services/vinculo.py:393`, `app/chamados/services/regularizacao_ext.py:197`, `app/chamados/services/regularizacao_ext.py:205`, `app/chamados/services/regularizacao_ext.py:207`)

5. **A nova exceção humana alcança ações com efeitos anteriores.** Geofence cria advertência e nota antes de `transicionar`; a recusa por viva ancorada deixa esses efeitos anteriores à transição, e a guarda de repetição só impede chamado já encerrado. Agenda e escala têm atomic local, mas deixam o `ValueError` propagar pela casca. (`app/chamados/services/acoes_chamado.py:176`, `app/chamados/services/acoes_chamado.py:193`, `app/chamados/services/acoes_chamado.py:218`, `app/chamados/services/acoes_chamado.py:222`, `app/chamados/models.py:159`, `app/chamados/services/acoes_chamado.py:102`, `app/chamados/services/acoes_chamado.py:149`, `app/chamados/views.py:461`, `app/chamados/views.py:464`)

   Já a resposta rápida do fio captura `ValueError`; os receivers principais tratam recusa ou isolam a falha. (`app/chamados/services/fio.py:716`, `app/chamados/signals.py:42`, `app/chamados/signals.py:46`, `app/chamados/signals.py:91`)

6. **Reaponte e status têm atomic comum, mas não exclusão concorrente.** As quatro portas de sistema envolvem plano/reaponte/estado no atomic; a seleção de perguntas e sobrevivente é feita sem trava. Portanto há rollback conjunto para exceção propagada, mas a estabilidade do plano até o save não está assegurada pelo código. Concorrência efetiva: **[nao verificado]**. (`app/chamados/models.py:569`, `app/chamados/models.py:670`, `app/chamados/models.py:1417`, `app/chamados/models.py:1434`, `app/chamados/models.py:296`, `app/chamados/juizes.py:1621`)

7. **A mudança de ordem de `Disputa.fechar` tem efeito útil e limite transacional.** As perguntas agora recebem `admin_decidiu` antes do save da disputa, permitindo ao receiver ler as mudas já encerradas. Mas o comentário “falha leva os dois” não vale para falha do pai capturada pelo receiver: pergunta e disputa podem confirmar com pai ainda vivo. (`app/chamados/models.py:1720`, `app/chamados/models.py:1724`, `app/chamados/models.py:1734`, `app/chamados/signals.py:41`, `app/chamados/signals.py:46`)

   A ordem passa a incluir pergunta→disputa→chamado; existe caminho chamado→pergunta nos receivers do chamado. Atomic não resolve essa inversão de aquisição de locks; deadlock observado: **[nao verificado]**. (`app/chamados/models.py:1724`, `app/chamados/models.py:1734`, `app/chamados/signals.py:42`, `app/chamados/models.py:250`, `app/chamados/signals.py:92`, `app/chamados/services/disputa_emissao.py:1583`)

8. **Desligamento muda a trilha e mantém uma frase falsa nos casos recusados.** O pai pode primeiro virar `resolvido/sistema` pelo signal e depois `fechado/cascade` no laço; `resolvido` pertence a `NAO_FECHADOS`, o carimbo é preservado e a via é regravada. São duas transições distintas. (`app/colaboradores/services/desligamento.py:142`, `app/colaboradores/services/desligamento.py:150`, `app/chamados/catalogo/motor.py:32`, `app/chamados/models.py:226`, `app/chamados/models.py:228`, `app/chamados/models.py:246`)

   O laço pula a nota quando `encerrar()` recusa, mas o chamado de auditoria ainda afirma genericamente que a cascata fechou chamados/disputas abertas. (`app/colaboradores/services/desligamento.py:155`, `app/colaboradores/services/desligamento.py:170`)

9. **O antes da reata pode mentir.** Para pergunta ancorada pelo container, `chamado_destino` era `NULL`; a trilha grava `antes={'chamado_destino': self.pk}`. O payload deveria descrever o campo real ou nomear explicitamente a âncora. (`app/chamados/models.py:297`, `app/chamados/models.py:380`)

   As três ações usam `evento`, que é best-effort: falha da auditoria pode ser engolida. Assim, atomic não garante que toda recusa/reata tenha uma linha de trilha. (`app/chamados/models.py:347`, `app/chamados/models.py:377`, `app/chamados/models.py:246`, `app/core/observ.py:105`, `app/core/observ.py:133`, `app/core/observ.py:139`)

O **pós-estado** delega a vitalidade ao C6 registrado, mas permite que o próprio encerramento seja parte da prova: pergunta carimbada sem slot pode morrer porque o destino passa a `ENCERRADOS`. O teste novo exige esse comportamento. `cancelado/superado` não pertencem a `ENCERRADOS`, portanto não produzem necessariamente a mesma absolvição. Não é equivalente a “toda viva no pré-estado reata ou recusa”. (`app/core/juizes.py:238`, `app/chamados/models.py:302`, `app/chamados/juizes.py:699`, `app/chamados/juizes.py:715`, `app/chamados/tests/test_o261e_f1_porta_encerramento.py:166`, `app/chamados/catalogo/motor.py:30`)

No caminho quente, a batida julga a célula e também chama o reconciliador; o cartório chama `reconciliar()` nos chamados resolvidos pela lâmpada. O diff acrescenta atomic, uma coleta no plano e outra no tripwire quando fecha; cada coleta cria novos objetos/memo, e o plano consulta C8 por pergunta viva. Mesmo sem perguntas, as duas buscas de âncoras continuam presentes no fechamento permitido. (`app/ponto/signals.py:135`, `app/ponto/signals.py:215`, `app/ponto/services/cartorio.py:1163`, `app/chamados/models.py:569`, `app/chamados/models.py:601`, `app/chamados/models.py:207`, `app/chamados/models.py:296`, `app/chamados/models.py:300`, `app/chamados/models.py:329`)

Os tetos existentes continuam fixados, inclusive nos cenários de batida que fecham chamados. **Delta real, latência e quais tetos estouram: [nao verificado]**. (`app/ponto/tests/test_selo_chokepoint_escrita.py:112`, `app/ponto/tests/test_selo_chokepoint_escrita.py:114`, `app/ponto/tests/test_selo_chokepoint_escrita.py:157`, `app/ponto/tests/test_selo_chokepoint_escrita.py:218`)

Nos laços examinados, cartório, retratação, tranca e auditoria de órfãos conferem o retorno; os descartes de `reconciliar()` em detecção de ausências/materialização já são declarados como operações sem nota ou contador posterior. (`app/ponto/services/cartorio.py:1175`, `app/chamados/reconciliador.py:525`, `app/chamados/services/competencia_trancada.py:177`, `app/chamados/management/commands/auditar_chamados_orfaos.py:67`, `app/chamados/tests/test_selo_reconciliar_le_o_retorno.py:22`)

**O selo AST não cumpre toda a promessa literal.**

- O teste anti-vacuidade **falha antes de testar o mutante**: espera uma ocorrência textual, mas há duas, no núcleo e no plano. (`app/chamados/tests/test_o261e_f1_selo_encerramento.py:161`, `app/chamados/models.py:207`, `app/chamados/models.py:322`)
- O verificador do tripwire aceita `if False` e substituição do `raise` por `print`: só compara números de linha. Confirmei esses mutantes em memória. (`app/chamados/tests/test_o261e_f1_selo_encerramento.py:106`, `app/chamados/tests/test_o261e_f1_selo_encerramento.py:110`)
- O detector não acompanha dicionários/nomes indiretos: `qs.update(**dados)` e `update_or_create(defaults=dados)` escapam; construtor com **PK existente** seguido de save também escapa da premissa de “nascimento”. (`app/chamados/tests/test_o261e_f1_selo_encerramento.py:69`, `app/chamados/tests/test_o261e_f1_selo_encerramento.py:71`, `app/chamados/tests/test_o261e_f1_selo_encerramento.py:140`)
- A exceção libera `_vivas_ancoradas` inteira pelo nome, sem verificar ausência de save nem restauração no `finally`; `__dict__` e SQL direto também não são formas reconhecidas. Isso descreve furos do selo, não evidência de uso atual desses mutantes em produção. (`app/chamados/tests/test_o261e_f1_selo_encerramento.py:27`, `app/chamados/tests/test_o261e_f1_selo_encerramento.py:55`, `app/chamados/tests/test_o261e_f1_selo_encerramento.py:86`)

Há também um **teste existente que pode passar sem andar seu caminho**: `test_gravar_estado_cruzando_a_fronteira` cria perguntas respondidas, chama `reconciliar()` sem conferir retorno e verifica apenas teto. Com a nova recusa, pode ficar barato justamente porque não cruzou a fronteira. (`app/ponto/tests/test_selo_chokepoint_escrita.py:240`, `app/ponto/tests/test_selo_chokepoint_escrita.py:292`, `app/chamados/models.py:602`)

O teste de exportada apenas simula `competencia_entregue=True` e verifica o status final; não demonstra que os registros exportados permaneceram intocados. (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:113`)

**(3) Testes vermelhos** [nao verificado]

| Teste |

[truncado em 20000 de 22863 caracteres]

## 30/10 18:57 · sessao 5ee53deaea38 · E1-BATERIA · verificador · origem=Code

A escrita de RED em cópia própria já está autorizada; exige contrato anterior ao uso e revisão de cada teste pelo Code (`CLAUDE.md:626`). Os pontos de falha são: [nao verificado]

1. **O predicado permite alterar e apagar testes existentes.** O item 3 só recusa alterações cujo caminho **não** case `app/<app>/tests/test_*.py`. Portanto, modificar ou apagar um teste existente nesse caminho passa pelo predicado, enquanto o item 4 exige rc 5 para modificação. Consequência: implementar literalmente a regra viola o próprio selo. É preciso exigir que todos os arquivos anteriores permaneçam intactos e aceitar somente acréscimos permitidos. Implementação: [nao verificado] — anexo, itens 3–4. [nao verificado]

2. **Manifesto SHA256 não define toda a barreira prometida.** Faltam regras explícitas para tipos de entrada, links simbólicos, permissões, normalização de caminhos e localização protegida do manifesto. Comparação final também não detecta escrita seguida de restauração. A prova atual usa `find . -type f`, que deixa links simbólicos fora do inventário (`bin/tests/prova_codex_pergunta_real.sh:11`). Consequência: copiar essa técnica não demonstra “nenhuma escrita fora do permitido”; demonstra apenas parte das diferenças finais. Barreira futura: [nao verificado].

3. **A saída atual do CLI não serve diretamente como publicação do patch.** O wrapper passa `-o "$RESPOSTA"` ao Codex antes de conhecer o resultado (`bin/codex_pergunta.sh:157`, `bin/codex_pergunta.sh:159`). Consequência: copiar esse fluxo para `<saida.patch>` permite produzir saída antes das verificações e antes de decidir por rc 5/6/7/75/124. O contrato precisa definir geração privada, validação, publicação final e tratamento de destino preexistente. Implementação: [nao verificado].

4. **Falta definir como arquivos novos viram patch.** A cópia atual é extraída por `git archive | tar`, sem inicialização de repositório (`bin/codex_pergunta.sh:95`). O contrato não especifica a geração do diff. Consequência: presumir um checkout com índice ou usar uma estratégia que omita arquivos novos pode entregar patch vazio ou incompleto. Método futuro: [nao verificado] — anexo, itens 2–3.

5. **“Mesmo gate” só soma as portas se elas usarem exatamente os mesmos locks e teto.** Hoje `LOGD` depende do diretório do script e aceita override; `CODEX_TETO` também aceita override sem validação (`bin/codex_pergunta.sh:47`, `bin/codex_pergunta.sh:51`). Os locks são abertos nesse diretório (`bin/codex_pergunta.sh:138`, `bin/codex_pergunta.sh:144`). Consequência: diretórios diferentes criam filas independentes; teto maior permite ultrapassar seis. Além disso, a medição descrita foi de seis chamadas curtas de leitura, não de escrita com compilação (`bin/codex_pergunta.sh:8`). Capacidade dessa nova carga: [nao verificado].

6. **O timeout atual não inclui a espera por vaga.** Há `flock` bloqueante no gate e laço de espera pelos slots; `timeout` só começa depois (`bin/codex_pergunta.sh:140`, `bin/codex_pergunta.sh:142`, `bin/codex_pergunta.sh:157`). Consequência: se o desenho copiar esse mecanismo, uma chamada poderá ultrapassar 1800 segundos sem retornar 124. O contrato precisa distinguir prazo do Codex e prazo total da chamada. Implementação futura: [nao verificado].

7. **A regra sobre agente esperando ficou condicional indevidamente.** A seção 7b atribui à sessão principal o acompanhamento de log, rc ou sinal; o agente entrega comando e caminho e termina (`CLAUDE.md:556`). O contrato admite agente construtor como chamador, mas só manda entregar e sair quando ele “não pode esperar”. Consequência: deixa aberta a espera do agente que a regra proíbe. Implementação: [nao verificado] — anexo, itens 1–2.

8. **Compilar e contar arquivos não prova RED.** A suíte executa `manage.py test` com labels determinadas (`bin/suite.sh:59`, `bin/suite.sh:102`; lista em `bin/regua.sh:23`). Um arquivo sintaticamente válido pode não conter teste descobrível, pertencer a app fora dessas labels ou já passar no código atual. Consequência: o rc 0 descrito não comprova reprodução do defeito. Descoberta e vermelho pelo motivo esperado continuam [nao verificado] — anexo, item 3.

9. **Falta explicitar o caminho de execução após aplicar o patch.** A suíte usa a cópia somente com `--dir`; sem isso, usa a árvore viva (`bin/suite.sh:72`, `bin/suite.sh:80`). Para cópias, consulta a porta de montagem, que fornece caches/logs/media e verifica staticfiles (`bin/suite.sh:77`; `bin/arvore_do_push.sh:50`, `bin/arvore_do_push.sh:80`). Consequência: execução improvisada pode testar a árvore errada ou produzir vermelho de infraestrutura. Procedimento futuro: [nao verificado].

10. **O ESTADO continuará dizendo “nunca escreve” para uma resposta RED.** O cabeçalho é fixo (`bin/gerar_codex_estado.py:141`). O leitor seleciona respostas finais sem condicioná-las à aprovação do patch ou à revisão; a associação de procedência também descarta rc e duração (`bin/gerar_codex_estado.py:97`, `bin/gerar_codex_estado.py:153`; `bin/codex_limite_estado.py:96`). Consequência: uma final RED pode aparecer sob descrição falsa e sem indicar que o wrapper recusou seu artefato.

11. **Codex falso não prova as garantias reais do sandbox e do limite.** Os testes atuais separam estrutura de prova real (`bin/tests/test_codex_pergunta.sh:2`); a prova real tenta escrever na cópia, na viva e em `/tmp` (`bin/tests/prova_codex_pergunta_real.sh:17`). O selo de limite declara não ter exemplo real do erro (`bin/tests/test_codex_limite.sh:4`). Consequência: os casos falsos propostos podem validar o recusador de alterações, mas isolamento efetivo de `/tmp`/`$TMPDIR`, rede e classificação de indisponibilidade permanecem [nao verificado].

Dois pontos estão coerentes: `red` realmente será contado como `outro`, preservando o papel literal nas últimas chamadas e na procedência (`bin/codex_limite_estado.py:78`, `bin/codex_limite_estado.py:88`, `bin/codex_limite_estado.py:96`); o novo selo, com o nome proposto, será descoberto automaticamente pela régua (`bin/regua.sh:138`).

## 30/10 18:56 · sessao 4e7a231d3a51 · - · - · origem=Ronald (tmux)

- **Regra de autorização contraditória.** No anexo, item 3, alteração/apagamento só é recusado quando o caminho **não** casa `app/<app>/tests/test_*.py`; modificar ou apagar teste existente fica permitido. O item 4 exige rc 5 para modificar existente. Consequência: implementar literalmente o item 3 viola o próprio selo. Implementação futura [nao verificado]. [nao verificado]

- **Manifesto de conteúdo não basta para a garantia declarada.** O contrato não define inventário de tipos, links, modos nem normalização dos caminhos; comparação final tampouco identifica escrita seguida de restauração. A prova atual usa `find . -type f`, portanto também deixa links simbólicos fora do inventário (`bin/tests/prova_codex_pergunta_real.sh:11`). Consequência: copiar essa técnica não demonstra “nenhuma escrita fora do permitido”; proteção efetiva futura [nao verificado].

- **Archive exige uma estratégia explícita para produzir o patch.** O wrapper materializa arquivos por `git archive | tar`, sem criar repositório na cópia (`bin/codex_pergunta.sh:95`). O contrato não diz como transforma arquivos novos em patch. Consequência: usar um `git diff` comum nessa cópia não é uma solução definida, e arquivos novos precisam ser incluídos deliberadamente; método futuro [nao verificado].

- **Saída do Codex não pode ser publicada diretamente como patch.** O wrapper atual passa o destino ao CLI antes de conhecer o rc (`bin/codex_pergunta.sh:157`, `bin/codex_pergunta.sh:158`, `bin/codex_pergunta.sh:159`). Consequência: copiar esse fluxo para `<saida.patch>` permite saída antes das validações; o contrato exige publicar apenas depois de rc 0. Estágio privado, tratamento de destino preexistente e publicação final [nao verificado].

- **Selo falso prova o validador, não o isolamento real.** Os testes atuais distinguem estrutura da chamada real (`bin/tests/test_codex_pergunta.sh:2`, `bin/tests/test_codex_pergunta.sh:3`); a prova real tenta escrever na cópia, na viva e em `/tmp` (`bin/tests/prova_codex_pergunta_real.sh:17`, `bin/tests/prova_codex_pergunta_real.sh:26`). Consequência: os casos falsos propostos não demonstram funcionamento das exclusões de `/tmp`/`$TMPDIR` nem isolamento do host; essas garantias permanecem [nao verificado].

## 30/10 18:55 · sessao 10615df8b65a · - · - · origem=Ronald (tmux)

- **Espera de agente ficou condicional indevidamente.** A regra veda usar agente para esperar e atribui polling de `rc`, log ou sinal à sessão principal; agente entrega comando+caminho e termina (`CLAUDE.md:556-559`). O contrato permite que “o chamador”, inclusive agente construtor, leia o resultado quando terminar e só entregue comando se “não pode esperar” (anexo, item 2; implementação [nao verificado]). Consequência: deixa autorizada uma espera que a regra proíbe.

- **Compilar e contar arquivos não estabelece RED nem descoberta pela suíte.** A suíte executa `manage.py test` com as labels selecionadas (`bin/suite.sh:59`, `bin/suite.sh:102-103`), cuja fonte contém lista determinada de apps (`bin/regua.sh:23`). Arquivo permitido com sintaxe válida pode não conter teste descobrível, pode pertencer a app fora das labels ou pode passar contra o código atual; o contrato não exige execução/vermelho anterior (anexo, item 3; comportamento futuro [nao verificado]). Consequência: `rc 0` pode entregar um “teste novo” que nunca roda ou não demonstra o defeito.

- **Cópia por archive não basta para rodar a suíte corretamente.** O caminho existente é `suite.sh --dir <copia>`, que obtém montagem por `arvore_do_push.sh` (`bin/suite.sh:72-79`); sem `--dir` a suíte usa a árvore viva (`bin/suite.sh:80-82`). A porta fornece tmpfs de caches/logs/media (`bin/arvore_do_push.sh:80-81`) e recusa staticfiles vazio (`bin/arvore_do_push.sh:50-53`). O contrato não descreve validação RED por esse caminho (anexo, itens 3–4; implementação [nao verificado]). Consequência: execução improvisada pode testar árvore errada ou produzir vermelho de infraestrutura.

- **Selo falso não prova a barreira real de escrita.** Os selos existentes distinguem estrutura de prova real (`bin/tests/test_codex_pergunta.sh:2-3`); a prova real tenta escrever em cópia, viva e `/tmp`, comparando estado antes/depois (`bin/tests/prova_codex_pergunta_real.sh:15-27`). O contrato prevê apenas Codex falso (anexo, item 4; proteção efetiva do sandbox [nao verificado]). Consequência: os casos de manifesto podem passar sem demonstrar isolamento real de `/tmp`, `$TMPDIR` e host.

Dois pontos que **não** constituem conflito por si:

- O corte novo autoriza expressamente escrita de RED em cópia própria e revisão por teste; as proibições anteriores devem ser lidas com essa exceção (`CLAUDE.md:626-628`). [nao verificado]
- O novo selo `bin/tests/test_codex_escreve_red.sh` será descoberto automaticamente pela régua, se criado nesse nome; não exige lista adicional (`bin/regua.sh:138-145`). A execução futura permanece [nao verificado].

## 30/10 18:55 · sessao 893874dc535d · - · - · origem=Ronald (tmux)

- **ESTADO continuará afirmando “nunca escreve” para respostas da porta RED.** O cabeçalho é fixo em `bin/gerar_codex_estado.py:141`; o leitor aceita todas as respostas finais, sem filtrar papel, rc ou revisão (`bin/gerar_codex_estado.py:80`, `bin/gerar_codex_estado.py:97`, `bin/gerar_codex_estado.py:143`). Consequência: respostas RED entram sob descrição falsa; esse relatório tampouco distingue RED aprovado de RED recusado.

- **Timeout não abrange a fila, se reproduzir o mecanismo atual.** O wrapper espera no gate e nos slots antes de iniciar `timeout` (`bin/codex_pergunta.sh:139`, `bin/codex_pergunta.sh:142`, `bin/codex_pergunta.sh:157`). Consequência: uma chamada pode levar mais de 1800 s sem rc124; aplicação desse comportamento ao script novo é [nao verificado].

- **Possível perda de procedência em limite, se copiar o fluxo atual.** O wrapper retorna rc75 antes de extrair `thread_id` (`bin/codex_pergunta.sh:167`, `bin/codex_pergunta.sh:171`, `bin/codex_pergunta.sh:175`). O leitor exclui sessões `-` (`bin/codex_limite_estado.py:96`) e classifica finais sem correspondência como Ronald/tmux (`bin/gerar_codex_estado.py:153`). Consequência: sessão do Code pode aparecer como Ronald; ocorrência e reprodução no script novo são [nao verificado].

- **Tratamento real de limite segue sem prova.** O selo declara não haver exemplo real (`bin/tests/test_codex_limite.sh:4`); cobre mensagens falsas de stderr/evento e falha genérica (`bin/tests/test_codex_limite.sh:18`, `bin/tests/test_codex_limite.sh:31`, `bin/tests/test_codex_limite.sh:37`). Consequência: garantia de rc75 em erro real de uso/indisponibilidade permanece [nao verificado].

- **`red` contado como `outro` funciona conforme declarado; não é falha.** O vocabulário contém quatro papéis (`bin/codex_limite_estado.py:23`) e desconhecidos viram `outro` (`bin/codex_limite_estado.py:78`); o papel literal permanece nas últimas chamadas e na procedência (`bin/codex_limite_estado.py:88`, `bin/codex_limite_estado.py:96`).

## 30/10 18:51 · sessao 06895aaa58a5 · - · - · origem=Ronald (tmux)

Relatório somente por leitura estática; reprodução em banco, ocorrência em produção e tempo medido: [nao verificado].

1. Dois conflitos concretos com escritores/leitores existentes

- `PerguntaDisputa.abrir()` reaponta pergunta **existente**, não só cria: busca `ex`, valida apenas E4-c, atribui destino, relavra e salva; não chama a nova proteção de costura. Portanto a F1 protege `fio.reapontar_resolvedora()`, mas deixa o reaponte pelo dedup costurar viva em terminal. A exceção documentada no novo helper só menciona “nascimento”, insuficiente para este ramo existente (`app/chamados/models.py:2292`, `app/chamados/models.py:2295`, `app/chamados/models.py:2300`, `app/chamados/models.py:2314`; proteção: `app/chamados/services/fio.py:748`, `app/chamados/services/fio.py:821`). O contrato já declara os dois arquivos como escritores de destino (`app/core/tests/test_contract_escritores_estado.py:60`).

- A âncora nova usa destino absoluto, caindo no pai **somente quando destino é NULL**; os leitores vivos da mesa e do app caem no pai também quando destino está terminal (`app/chamados/models.py:296`; `app/chamados/services/raiox.py:54`, `app/chamados/services/raiox.py:142`; `app/api/views.py:1043`). O juiz de respostas pendentes já lê a mesma regra de crédito vivo (`app/chamados/juizes.py:1474`). Contraexemplo estático: pai vivo de módulo `disputa_supervisao`, destino terminal, pergunta muda sem materialização. A pergunta é creditada ao pai por API/raiox, mas `_vivas_ancoradas` não a coleta; `reconciliar()` pode fechar o pai, e C6 continua dando a pergunta por viva porque sem materialização retorna True (`app/chamados/models.py:296`, `app/chamados/models.py:601`, `app/chamados/models.py:607`, `app/chamados/models.py:574`; `app/chamados/juizes.py:1076`). Ocorrência desse arranjo em produção: [nao verificado].

- Outra passagem sem guarda: instância antiga `resolvido`, banco reaberto por outra instância e pergunta viva criada/reatada, seguida de `.encerrar()` na instância antiga. `encerrar` decide pela instância, gate e tripwire só atuam quando o estado em memória pertence a `VIVOS`; assim grava `fechado` sobre o banco vivo sem consultar a pergunta (`app/chamados/models.py:1412`, `app/chamados/models.py:356`, `app/chamados/models.py:201`, `app/chamados/models.py:206`, `app/chamados/models.py:250`). A leitura fresca já existe em `reconciliar`, porém não em `encerrar` (`app/chamados/models.py:592`). Reprodução executada: [nao verificado].

2. Censo de escritores

| Campo | Escritores persistentes de produção | Escrita em memória / nascimento / acervo |
|---|---|---|
| `ChamadoColaborador.status_local` | Núcleo `_gravar_estado`, atribui e salva (`app/chamados/models.py:211`, `app/chamados/models.py:250`). | `_vivas_ancoradas` troca/devolve em memória (`app/chamados/models.py:302`, `app/chamados/models.py:312`); `criar` nasce por create (`app/chamados/models.py:970`); migration 0039 atualiza acervo e reverse (`app/chamados/migrations/0039_a3_via_backfill.py:24`, `app/chamados/migrations/0039_a3_via_backfill.py:33`). |
| `resolvido_em` | Núcleo carimba terminal ou limpa vivo (`app/chamados/models.py:227`, `app/chamados/models.py:231`). | Nascimento pode receber campos via `**campos` (`app/chamados/models.py:970`). |
| `via_encerramento` | Núcleo grava via ou limpa (`app/chamados/models.py:228`, `app/chamados/models.py:232`). | Migration backfill/reverse (`app/chamados/migrations/0039_a3_via_backfill.py:24`, `app/chamados/migrations/0039_a3_via_backfill.py:25`, `app/chamados/migrations/0039_a3_via_backfill.py:28`, `app/chamados/migrations/0039_a3_via_backfill.py:34`). |
| `urgencia` | Ritual rebaixa (`app/chamados/models.py:408`), núcleo restaura (`app/chamados/models.py:234`), `rebaixar` salva (`app/chamados/models.py:415`), `_rebaixar_sem_post_save` persiste via update dinâmico (`app/chamados/models.py:550`, `app/chamados/models.py:555`), `escalar_urgencia` atribui e salva (`app/chamados/models.py:472`). | Nascimento aceita após validação (`app/chamados/models.py:967`, `app/chamados/models.py:970`). |
| `PerguntaDisputa.chamado_destino` | `abrir` no dedup (`app/chamados/models.py:2300`, `app/chamados/models.py:2314`) e nascimento (`app/chamados/models.py:2316`); `fio.reapontar_resolvedora` (`app/chamados/services/fio.py:828`). | Avaliação temporária no novo gate (`app/chamados/models.py:308`); novo helper de costura troca/devolve (`app/chamados/services/fio.py:758`, `app/chamados/services/fio.py:762`). |
| `materializada_em` + `via_resolucao` | Portas do model: admin, tranca, fantasma, reaberturas e ressuscitação (`app/chamados/models.py:2362`, `app/chamados/models.py:2376`, `app/chamados/models.py:2401`, `app/chamados/models.py:2416`, `app/chamados/models.py:2436`, `app/chamados/models.py:2446`, `app/chamados/models.py:1377`). Reconciliador carimba sem-turno e célula absolveu (`app/chamados/reconciliador.py:402`, `app/chamados/reconciliador.py:617`). Materializador carimba declaração/ausência, recusa paridade/colisão e aprovação (`app/chamados/services/materializacao.py:509`, `app/chamados/services/materializacao.py:526`, `app/chamados/services/materializacao.py:858`, `app/chamados/services/materializacao.py:1043`, `app/chamados/services/materializacao.py:1149`, `app/chamados/services/materializacao.py:1177`, `app/chamados/services/materializacao.py:1339`). Dedup retrata duplicata (`app/chamados/services/fio.py:875`). Emissão resolve por chamado/fato e desfaz absorção (`app/chamados/services/disputa_emissao.py:1562`, `app/chamados/services/disputa_emissao.py:1644`, `app/chamados/services/disputa_emissao.py:1360`). Relavra troca via/desfaz carimbo (`app/chamados/services/relavra_via.py:123`, `app/chamados/services/relavra_via.py:231`). | `encerrar_por_fato_no_espelho` só troca via (`app/chamados/models.py:2389`). `regularizacao_ext.py:198` é campo homônimo de **RegularizacaoExterna**, não pergunta (`app/chamados/services/regularizacao_ext.py:197`). |
| `DisputaSupervisao.fechada_em` | Fechamento em `fechar` (`app/chamados/models.py:1730`, `app/chamados/models.py:1734`); limpeza por `reabrir_admin` e `reabrir_sistema` (`app/chamados/models.py:1932`, `app/chamados/models.py:1956`). | Default NULL (`app/chamados/models.py:1557`). |
| `LogAuditoria` ações pedidas | Persistência comum por `evento` (`app/core/observ.py:133`). `estado_chamado` nasce no núcleo (`app/chamados/models.py:246`); `fechamento_recusado` em `_registrar_recusa` (`app/chamados/models.py:347`); `pergunta_reatada` em `_gate_pergunta_viva` (`app/chamados/models.py:377`). | `evento` é best-effort e pode retornar sem linha após engolir erro (`app/core/observ.py:105`, `app/core/observ.py:139`). |

3. Censo agrupado de leitores que interferem na fatia

- `status_local`: motor das portas (`app/chamados/models.py:118`, `app/chamados/models.py:596`, `app/chamados/models.py:664`, `app/chamados/models.py:1412`); pergunta materializada/C6 (`app/chamados/juizes.py:700`, `app/chamados/juizes.py:774`); C8 e escolha de cobrança (`app/chamados/juizes.py:1621`, `app/chamados/juizes.py:1675`); receivers de estado (`app/chamados/signals.py:33`, `app/chamados/signals.py:70`, `app/chamados/signals.py:81`, `app/chamados/signals.py:169`, `app/chamados/signals.py:230`); fila, painel e partição (`app/chamados/services/painel.py:155`, `app/chamados/services/painel.py:187`, `app/chamados/services/verbos.py:227`, `app/chamados/services/desenlace.py:48`); fio e próximo card (`app/chamados/services/fio.py:385`, `app/chamados/services/fio.py:399`, `app/chamados/services/fio.py:428`); API (`app/api/views.py:1047`, `app/api/views.py:1145`, `app/api/views.py:1155`, `app/api/views_mensageria.py:417`); relatórios (`app/relatorios/views.py:745`, `app/relatorios/pdf_chamados.py:116`); apresentação (`app/templates/chamados/partials/_lista_chamados.html:33`, `app/templates/chamados/partials/_card_atendimento.html:19`, `app/templates/chamados/partials/modal_fio.html:493`, `app/templates/chamados/detalhe_local.html:279`); auditoria de invariantes (`app/chamados/management/commands/auditar_invariantes_chamados.py:573`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1067`).

- `resolvido_em`: núcleo preserva terminal→terminal (`app/chamados/models.py:226`); reabertura administrativa mede fato novo desde carimbo (`app/chamados/models.py:1214`, `app/chamados/models.py:1218`); fila/indicadores contam encerramentos (`app/chamados/services/fila.py:109`, `app/chamados/services/painel.py:243`, `app/chamados/services/painel.py:247`, `app/chamados/services/verbos.py:234`); invalidação de painel usa máximo (`app/chamados/services/painel.py:65`); arquivo ordena (`app/chamados/views_arquivar.py:157`); detalhe e relatório exibem (`app/templates/chamados/detalhe.html:56`, `app/templates/chamados/detalhe_local.html:88`, `app/templates/relatorios/historico_chamados.html:127`); vigia terminal sem carimbo (`app/chamados/management/commands/auditar_invariantes_chamados.py:571`).

- `via_encerramento`: nascimento/revival não reabre cascade (`app/chamados/models.py:890`, `app/chamados/services/adesao.py:90`); guarda administrativa (`app/chamados/models.py:1214`); tranca reabre por esta via (`app/chamados/services/competencia_trancada.py:252`); emissão não toca trancada (`app/chamados/services/disputa_emissao.py:1531`); arquivo/pauta (`app/chamados/views_arquivar.py:64`, `app/chamados/views_arquivar.py:156`); diagnóstico de cobertura de furos (`app/ponto/services/furos_sem_cobranca.py:69`); invariantes exigem via (`app/chamados/management/commands/auditar_invariantes_chamados.py:1067`).

- `urgencia`: SLA (`app/chamados/services/sla.py:30`, `app/chamados/services/painel.py:375`), ordenação/filtro/contadores (`app/chamados/services/painel.py:102`, `app/chamados/services/painel.py:118`, `app/chamados/services/painel.py:223`), verbos (`app/chamados/services/verbos.py:194`), API (`app/api/views.py:1150`, `app/api/views_mensageria.py:419`), templates (`app/templates/chamados/fio_colaborador.html:165`, `app/templates/chamados/partials/tabela_chamados.html:30`, `app/templates/chamados/partials/_lista_chamados.html:68`).

- `chamado_destino`: âncora absoluta (`app/chamados/services/fio.py:116`), crédito vivo da mesa (`app/chamados/services/raiox.py:60`, `app/chamados/services/raiox.py:142`), crédito do app (`app/api/views.py:1046`), serialização de pergunta (`app/api/views.py:2626`, `app/api/views.py:2642`), juiz da materialização (`app/chamados/juizes.py:699`), juiz das irmãs (`app/chamados/juizes.py:1474`), derivação do marco (`app/chamados/juizes.py:1303`, `app/chamados/juizes.py:1430`), busca de disputa (`app/chamados/services/fio.py:135`), tranca (`app/chamados/services/competencia_trancada.py:136`), materialização de resolvidas (`app/chamados/services/disputa_emissao.py:1537`), filas/pergunta no app (`app/chamados/services/pergunta_no_app.py:116`, `app/chamados/services/pergunta_no_app.py:139`).

- `materializada_em`/`via_resolucao`: C6 usa carimbo e vias para tranca, identidade, lâmpada, ausência de materialização e fato (`app/chamados/juizes.py:1015`, `app/chamados/juizes.py:1044`, `app/chamados/juizes.py:1050`, `app/chamados/juizes.py:1076`, `app/chamados/juizes.py:1083`); juiz de materializada usa vias (`app/chamados/juizes.py:603`); estado exibido da pergunta (`app/chamados/models.py:2341`); `respostas_sem_veredito` exclui materializadas (`app/chamados/models.py:1654`); materializador seleciona não materializadas (`app/chamados/services/materializacao.py:417`, `app/chamados/services/materializacao.py:1289`); relavra seleciona carimbos desfazíveis (`app/chamados/services/relavra_via.py:179`); raiox esconde materializada comprovada por lavra (`app/chamados/services/raiox.py:133`); cache agregado usa timestamp (`app/chamados/veredito_lavrado.py:163`); vigia I8 usa carimbos (`app/chamados/management/commands/auditar_invariantes_chamados.py:954`); UI relata via (`app/templates/chamados/partials/_card_atendimento.html:186`). O leitor de fila de flip de paridade é filtro, não escritor (`app/ponto/services/flip_auto.py:129`).

- `fechada_em`: receivers fecham pai/materializam/retomam regularização (`app/chamados/signals.py:28`, `app/chamados/signals.py:106`, `app/chamados/signals.py:130`); API limita disputas abertas (`app/api/views.py:1008`, `app/api/views.py:1030`, `app/api/views.py:2746`); fio resolve e impede operação em fechada (`app/chamados/services/fio.py:131`, `app/chamados/services/fio.py:594`, `app/chamados/services/fio.py:627`); validação pode reabrir (`app/chamados/services/validacao.py:199`); reconciliador/materializador fecham somente aberta (`app/chamados/reconciliador.py:413`, `app/chamados/reconciliador.py:635`, `app/chamados/services/materializacao.py:542`, `app/chamados/services/materializacao.py:1233`); cascata e tranca (`app/colaboradores/services/desligamento.py:139`, `app/chamados/services/competencia_trancada.py:145`); crons de disputa/becos (`app/chamados/management/commands/auditar_disputas.py:87`, `app/chamados/management/commands/fechar_becos_disputa.py:32`, `app/ponto/management/commands/alertar_disputas_pendentes.py:34`); vigia viva em disputa fechada (`app/chamados/services/vigia_fio.py:388`); cadastro/situação (`app/colaboradores/services/drawer.py:70`, `app/colaboradores/services/situacional.py:117`, `app/colaboradores/painel_cell.py:32`); tela (`app/templates/chamados/partials/modal_fio.html:372`).

- Logs: `fechamento_recusado` e `estado_chamado` são lidos pelo dedup da recusa (`app/chamados/models.py:342`, `app/chamados/models.py:345`). `estado_chamado` alimenta história e dois vigias (`app/chamados/services/historico.py:44`, `app/chamados/services/historico.py:86`, `app/chamados/services/vigia_loop.py:29`, `app/chamados/services/vigia_loop.py:114`). `pergunta_reatada` não apresentou leitor especializado na busca nominal da árvore; o leitor genérico de auditoria aceita todas as ações e filtro literal (`app/core/views.py:705`, `app/core/views.py:710`). História do chamado só inclui `estado_chamado`, logo não mostra a nova reata ou recusa (`app/chamados/services/historico.py:44`, `app/chamados/services/historico.py:86`). As ações livres já são admitidas pelo armazenamento, embora as três não pertençam ao menu estático `ACOES` (`app/core/models.py:286`, `app/core/models.py:296`; menu: `app/core/views.py:738`).

4. Limites adicionais do censo

- Migrações 0039 escrevem terminal fora da porta como acervo; o selo novo exclui migrations, mas a instrução da migration é executável em instalação/migração, e não há evidência de seu estado de aplicação nesta sonda (`app/chamados/migrations/0039_a3_via_backfill.py:24`; aplicação: [nao verificado]).
- O arquivo `app/chamados/admin.py` está vazio na cópia inspecionada; nenhum leitor/escritor explícito desses campos ali foi encontrado [nao verificado quanto a comportamento de registro genérico externo].
- Testes/fixtures escrevem estados deliberadamente por create/update; não devem ser contados como escritores de produção; o próprio novo selo declara a exclusão (`app/chamados/tests/test_o261e_f1_selo_encerramento.py:6`).
- Novas ações de auditoria usam a infraestrutura best-effort existente; “recusa com uma linha garantida” não é garantia transacional dura porque `evento()` engole falha e permite continuidade (`app/core/observ.py:98`, `app/core/observ.py:105`, `app/core/observ.py:133`).

## 30/10 18:51 · sessao 95a24d27d2ce · - · - · origem=Ronald (tmux)

Achados estáticos; execução de testes e ocorrência em produção [nao verificado]. [nao verificado]

1. **Bypass por instância velha.** Uma instância carregada como `resolvido` pode sobreviver à reabertura feita por outra instância (`resolvido -> em_analise` permitido em `app/chamados/catalogo/motor.py:102`). Depois de nascer pergunta viva, `velha.encerrar()` decide pelo estado velho (`app/chamados/models.py:1412`), pula a guarda (`app/chamados/models.py:356`) e pula o tripwire (`app/chamados/models.py:206`), gravando `fechado`. Só `reconciliar` recarrega os campos do banco (`app/chamados/models.py:592`). O teste novo explicitamente permite terminal→terminal com pergunta ancorada (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:205`).

2. **Ações humanas deixam efeitos antes da nova recusa.** Autorizar geofence pode criar advertência (`app/chamados/services/acoes_chamado.py:193`), anexar nota (`app/chamados/services/acoes_chamado.py:218`) e então receber `ValueError` da nova guarda no `transicionar` (`app/chamados/services/acoes_chamado.py:222`; `app/chamados/models.py:159`). Não há atomic abrangendo esse método; a recusa deixa chamado vivo e permite repetir a advertência (`app/chamados/services/acoes_chamado.py:176`). Agenda e escala têm atomic, mas a exceção propaga sem tratamento nas ações e na casca (`app/chamados/services/acoes_chamado.py:102`, `app/chamados/services/acoes_chamado.py:140`; `app/chamados/views.py:459`).

3. **Dois chamadores ignoram a recusa de `encerrar`.** Troca de vínculo segue criando novo aceite depois de `_ch_old.encerrar()` sem conferir retorno (`app/colaboradores/services/vinculo.py:384`, `app/colaboradores/services/vinculo.py:393`). Regularização externa já grava estado materializado e nota, ignora `encerrar()` e retorna `materializada=True` (`app/chamados/services/regularizacao_ext.py:197`, `app/chamados/services/regularizacao_ext.py:205`, `app/chamados/services/regularizacao_ext.py:207`). Isso não prova que os efeitos de domínio sejam incorretos, mas invalida a expectativa de que o chamado necessariamente saiu da fila nessas chamadas (`app/colaboradores/services/vinculo.py:374`; `app/chamados/models.py:1418`).

4. **Atomicidade de `Disputa.fechar` tem limite.** A nova ordem escreve pergunta, disputa e depois chamado pelo signal (`app/chamados/models.py:1722`, `app/chamados/models.py:1734`; `app/chamados/signals.py:42`). Uma falha do fechamento do pai é capturada pelo receiver; portanto não desfaz necessariamente pergunta e disputa, apesar do comentário “falha leva os dois” (`app/chamados/signals.py:46`; `app/chamados/models.py:1720`). Há ordem inversa em outro caminho: save do chamado dispara resolução que escreve perguntas (`app/chamados/models.py:250`; `app/chamados/signals.py:92`; `app/chamados/services/disputa_emissao.py:1583`). Deadlock efetivo [nao verificado].

5. **Desligamento muda a trilha intermediária.** A disputa fecha primeiro e seu signal pode produzir `resolvido/sistema`; depois o laço inclui `resolvido` e produz `fechado/cascade` (`app/colaboradores/services/desligamento.py:139`, `app/colaboradores/services/desligamento.py:150`; `app/chamados/catalogo/motor.py:32`, `app/chamados/catalogo/motor.py:95`). O segundo ato preserva `resolvido_em`, regrava a via e deixa outra linha `estado_chamado` (`app/chamados/models.py:226`, `app/chamados/models.py:228`, `app/chamados/models.py:246`). A descrição do chamado de auditoria continua afirmando que a cascata fechou chamados/disputas mesmo havendo recusas (`app/colaboradores/services/desligamento.py:155`, `app/colaboradores/services/desligamento.py:170`).

Testes existentes previsivelmente vermelhos, sem execução: [nao verificado]

- `LacoHxBug31Test.test_resposta_creditada_ao_pai_SEGURA_a_retratacao`, `test_MORDE_validada_libera_o_pai`, `test_retida_duas_vezes_UMA_linha_de_trilha`, `test_retida_MORDE_pendencia_nova_grava_de_novo`: encerram destino com resposta viva, ignoram o novo `False` e pressupõem destino morto (`app/chamados/tests/test_laco_hx_x_bug31.py:107`, `:129`, `:140`, `:153`). Destino vivo deixa de creditar a resposta ao pai (`app/chamados/juizes.py:1474`), então a retenção esperada não acontece e a retratação do pai prossegue (`app/chamados/reconciliador.py:520`).

- `RespostaVisivelTest.test_resposta_pendente_mantem_pai_visivel` e `test_resposta_pendente_tem_precedencia_sobre_encerrar`: pai encerra porque a âncora é o destino; destino agora recusa encerrar, permanecendo vivo (`app/chamados/tests/test_b2d_resposta_visivel.py:57`, `:104`; `app/chamados/models.py:298`, `app/chamados/models.py:1418`). O HX não reabre o pai, porque o mesmo juiz não lhe credita resposta enquanto o destino vive (`app/chamados/services/disputa_emissao.py:1598`; `app/chamados/juizes.py:1474`).

- Candidatos por custo: `test_gravar_estado_sem_pergunta`, `test_gate_da_premissa_fecha_com_dia_e_celula`, `test_gate_da_premissa_recusa_com_dia_e_celula`, `test_registrar_batida_com_1_furo_vivo_que_ela_fecha`, `test_registrar_batida_com_3_furos_vivos_que_ela_fecha` mantêm tetos anteriores (`app/ponto/tests/test_selo_chokepoint_escrita.py:112`, `:114`, `:115`, `:157`; testes em `:286`, `:319`, `:325`, `:360`, `:365`). O novo atomic e as consultas de `_vivas_ancoradas` acrescentam trabalho nesses caminhos (`app/chamados/models.py:569`, `app/chamados/models.py:601`, `app/chamados/models.py:207`). Contagens exatas e quais excedem o teto [nao verificado].

Os signals de disputa e de resolução já tratam `False` ou isolam exceções; os crons principais conferem o retorno antes de contar/anotar (`app/chamados/signals.py:42`, `app/chamados/signals.py:91`; `app/chamados/services/competencia_trancada.py:177`; `app/ponto/services/cartorio.py:1163`, `:1175`, `:1196`). Os descartes em `detectar_ausencias` e materialização são explicitamente autorizados pelo selo existente, sem nota/contador posterior (`app/chamados/tests/test_selo_reconciliar_le_o_retorno.py:22`).

## 30/10 18:51 · sessao ec3bc7bfd574 · - · - · origem=Ronald (tmux)

Achado confirmado por execução em memória, sem Django/banco: `NucleoUnicoContract.test_um_escritor_de_status_local` fica vermelho. O selo antigo só libera a atribuição literal `self.status_local = novo`; a nova restauração `self.status_local = _antes` entra em `ofensores`. Referências: `app/chamados/tests/test_contract_status_nucleo.py:7-9,23-28`; `app/chamados/models.py:312`.

O selo AST novo passa na árvore atual, mas deixa passar os seguintes mutantes, comprovados executando suas funções com Python stdlib em memória:

- `d={'status_local':'fechado'}; qs.update(**d)` e `update_or_create(defaults=d)`: ele só reconhece dicionário literal no próprio argumento. `app/chamados/tests/test_o261e_f1_selo_encerramento.py:44-45,67-73`.
- `C(pk=1,status_local='fechado').save(update_fields=['status_local'])`: todo construtor é ignorado, sem comprovar nascimento. `app/chamados/tests/test_o261e_f1_selo_encerramento.py:61-80,140-147`.
- `ch.__dict__['status_local']='fechado'; ch.save()` e SQL direto: formas ausentes do detector. `app/chamados/tests/test_o261e_f1_selo_encerramento.py:55-80`.
- Um `save()` acrescentado dentro de `_vivas_ancoradas`: a exceção libera a função inteira pelo nome; não verifica ausência de persistência nem restauração no `finally`. `app/chamados/tests/test_o261e_f1_selo_encerramento.py:27-28,86-87`.
- Trocar a condição do tripwire por `if False:` ou trocar seu `raise` por `print`: o checker ainda retorna `True`, pois compara somente números de linha; o mutante atual só remove uma chamada textual. `app/chamados/tests/test_o261e_f1_selo_encerramento.py:99-110,159-163`; guarda efetiva em `app/chamados/models.py:206-210`.

Não encontrei esses bypass hipotéticos usados atualmente: o scanner novo encontrou apenas as atribuições em `app/chamados/models.py:211,302,312` e nenhuma fora de suas permissões. A execução Django dos testes permanece [nao verificado].

Regressões funcionais previstas por leitura:

- `RespostaVisivelTest.test_resposta_pendente_mantem_pai_visivel` e `test_resposta_pendente_tem_precedencia_sobre_encerrar`: o teste fecha primeiro o pai e depois tenta fechar o destino com resposta viva. Agora o destino recusa; o crédito permanece no destino vivo, e o resolver não reabre o pai. As expectativas de pai vivo tendem a falhar. `app/chamados/tests/test_b2d_resposta_visivel.py:57-67,104-115`; `app/chamados/models.py:356-370,1415-1420`; `app/chamados/services/disputa_emissao.py:1596-1606`; `app/chamados/juizes.py:1471-1476`.
- `LacoHxBug31Test.test_resposta_creditada_ao_pai_SEGURA_a_retratacao`, `test_MORDE_validada_libera_o_pai`, `test_retida_duas_vezes_UMA_linha_de_trilha` e `test_retida_MORDE_pendencia_nova_grava_de_novo`: dependem de `dest.encerrar()` produzir destino morto com resposta pendente. A nova recusa conserva destino vivo, impedindo o crédito ao pai e a retenção esperada. `app/chamados/tests/test_laco_hx_x_bug31.py:107-118,129-164`; `app/chamados/juizes.py:1471-1476`; `app/chamados/reconciliador.py:448-451`. Execução e eventuais efeitos de fixtures/dedup: [nao verificado].

Coberturas ausentes nos testes novos: rollback após reapontar, falha na segunda pergunta de um plano, concorrência, `encerrar_por_competencia_trancada`, reata por `cancelar`/`encerrar` e estabilidade dos dados exportados; os casos presentes estão em `app/chamados/tests/test_o261e_f1_porta_encerramento.py:81-254`. O teste de competência entregue apenas simula o juiz de exportação e verifica o status final; não verifica integridade dos campos de pergunta/exportada. `app/chamados/tests/test_o261e_f1_porta_encerramento.py:113-118`.

_722 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
