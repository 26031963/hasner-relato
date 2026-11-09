# CODEX-ESTADO

> Ultima resposta processada: **09/11/2026 15:11** · publicado em **09/11/2026 15:11** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 09/11 15:11 · sessao 22223ccca92c · - · - · origem=Ronald (tmux)

Dois procedentes em T1:

1. **Escape do `--toca` por TAB ou newline no nome real.** O inventário emite `t` sem escape em TSV (`bin/codex_pergunta.sh:217`), enquanto o consumidor separa por linha/TAB e usa somente `c[1]` (`bin/codex_pergunta.sh:278-282`). Assim, o arquivo real `app/folha/views.py<TAB>fora` é julgado como `app/folha/views.py` e passa por `--toca app/folha/views.py`, embora seja outro arquivo; reproduzi o rc 0 em memória executando `_TOCA_PY` real com `StringIO`, sem escrever arquivo. A lista de caracteres do `--toca` não protege nomes de arquivos encontrados pelo inventário (`bin/codex_pergunta.sh:323`, `bin/codex_pergunta.sh:193`, `bin/codex_pergunta.sh:217`).

2. **Admissão e fim divergem para arquivos novos irmãos sob pai novo.** `app/novo/a.py` versus `app/novo/b.py` não cruzam na admissão (`bin/codex_pergunta.sh:263-275`). Criar `a.py` produz também `TOCADO app/novo/` (`bin/codex_pergunta.sh:195-196`); o fim aceita esse pai como necessário ao meu arquivo, mas o considera cruzado com o arquivo da outra raia (`bin/codex_pergunta.sh:283-288`). Reproduzi em memória admissão rc 0 e fim rc 3. É falsa recusa por pasta comum, contrária ao critério de arquivo comum (`CLAUDE.md:306-308`).

Demais verificações:

- A primitiva de interpretação é compartilhada: sem `/` final é igualdade exata; com `/` é prefixo por componente; admissão e fim chamam a mesma definição (`bin/codex_pergunta.sh:263-287`).
- `..`, `/./`, `//`, caminho absoluto e raiz diversa de `app/` ou `bin/` são recusados no argumento (`bin/codex_pergunta.sh:323-324`). `app/.` terminal passa na forma, mas não cobre descendentes; não identifiquei escape por isso (`bin/codex_pergunta.sh:264`, `bin/codex_pergunta.sh:323`).
- Symlink tocado é `PROIBIDO`; o walk não segue symlink e usa `lstat` (`bin/codex_pergunta.sh:188-194`, `bin/codex_pergunta.sh:215`). Ausência de escape transitório durante execução: [não verificado].
- O chamador fornece `--toca`; o parser escreve `TOCA[]`, a validação restringe o formato, `_publica_estado` grava `toca=`, `_toca_de` lê esse campo, `_portoes` julga a admissão e o fim relê o estado da outra raia (`bin/codex_pergunta.sh:71`, `bin/codex_pergunta.sh:321-324`, `bin/codex_pergunta.sh:308-312`, `bin/codex_pergunta.sh:291`, `bin/codex_pergunta.sh:295-306`, `bin/codex_pergunta.sh:565-574`).
- Os controles novos cobrem arquivo exato, pasta por componente e pai novo com uma raia; não cobrem TAB/newline nem dois arquivos irmãos sob pai novo (`bin/tests/test_codex_pergunta.sh:562-570`).

## 09/11 14:51 · sessao 8edd85065351 · BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS · sonda · origem=Code

**O anexo permite contar pares candidatos, mas não quantificar disputas distintas que passariam a ficar retidas.** São **112 pares em disputas abertas e 18 em fechadas**, nas classes citadas. [nao verificado — cálculo sobre o anexo]

Não localizei `sonda_par.py` nesta cópia; portanto, não conferi seu conteúdo nem o hash anunciado. Uso `[nao verificado]` nas contagens do anexo e nas somas derivadas delas, sem confirmação independente de PROD. [nao verificado]

1. **Censo por combinação**

“Fechada” abaixo é total menos aberta, condicionado a ambos os dicionários cobrirem o mesmo recorte. [nao verificado]

| Classe rotulada | Pares totais | Em abertas | Em fechadas | Evidência |
|---|---:|---:|---:|---|
| A/A | 3.793 | 1.494 | 2.299 | [nao verificado] |
| A/R | 49 | 43 | 6 | [nao verificado] |
| A/S | 53 | 52 | 1 | [nao verificado] |
| A/T | 1 | 0 | 1 | [nao verificado] |
| A/V | 5 | 2 | 3 | [nao verificado] |
| R/R | 172 | 168 | 4 | [nao verificado] |
| R/S | 18 | 17 | 1 | [nao verificado] |
| S/S | 2.514 | 2.482 | 32 | [nao verificado] |
| S/V | 4 | 4 | 0 | [nao verificado] |
| T/T | 196 | 0 | 196 | [nao verificado] |
| V/V | 18 | 11 | 7 | [nao verificado] |
| **Total** | **6.823** | **4.273** | **2.550** | [nao verificado] |

R/T, R/V, S/T e T/V não aparecem nos dicionários: contribuição zero **na saída apresentada**, sem comprovar zero na projeção correta da matriz. [nao verificado]

Os 6.823 pares correspondem a **13.646 metades**, distribuídas pelos rótulos: **S=5.103, R=411, V=45, A=7.694, T=393**. Das 18.424 metades declaradas, **4.778 ficam fora dessa tabela**; sua distribuição por estado não foi fornecida. A conta dos grupos fecha: `9.900 − 2.756 sem_par − 321 ambíguos = 6.823`. [nao verificado]

**N não é zero conhecido.** A recusa humana registra `ato='recusa'` na trilha e limpa resposta e validação da projeção; sem E1, uma metade recusada pode parecer S. Isso esconde, por exemplo, S/N dentro de S/S e A/N dentro de A/S. (`app/chamados/services/validacao.py:591`; `app/chamados/services/disputa_emissao.py:1590`; exemplos de redistribuição [nao verificado])

2. **Retenção pelo C7 ampliado e legado**

| Classe candidata | Pares em abertas | Pares em fechadas | Evidência |
|---|---:|---:|---|
| A/R | 43 | 6 | [nao verificado] |
| A/S | 52 | 1 | [nao verificado] |
| A/V | 2 | 3 | [nao verificado] |
| A/T | 0 | 1 | [nao verificado] |
| S/V | 4 | 0 | [nao verificado] |
| V/V | 11 | 7 | [nao verificado] |
| **Total** | **112** | **18** | [nao verificado] |

V/R e V/T não acrescentam ocorrências nos dicionários apresentados. O total separado `V_sem_A=47` não deve ser somado aos pares: mistura unidades e pode sobrepor essas ocorrências. [nao verificado]

**Não dá para chamar esses números de 112 disputas novas retidas e 18 disputas legadas.** Os exemplos repetem `disputa_pk`, inclusive entre combinações, e não fornecem a relação completa para deduplicação. [nao verificado — anexo `EXEMPLOS`]

Também falta demonstrar **“que hoje fecham”**: C7 consulta todas as perguntas da disputa, exigindo ausência de resposta sem veredito e de fio vivo; outra pergunta pode já impedir seu fechamento. (`app/chamados/models.py:1850`; `app/chamados/models.py:1861`)

A brecha para V está comprovada no código: C6 considera qualquer pergunta validada morta imediatamente, e `respostas_sem_veredito()` exclui as validadas, mesmo sem aplicação. Isso demonstra a possibilidade de escape, sem quantificar a população afetada. (`app/chamados/juizes.py:1153`; `app/chamados/models.py:1851`)

Os 18 pares fechados são ocorrências candidatas de legado, sem autorização para correção automática do dado pela L-099; a porta `fechar()` também não reabre disputa já fechada. (`CLAUDE.md:176`; `app/chamados/models.py:1887`; quantidade [nao verificado])

3. **`ESPERA_PAR_INTERVALO`**

O anexo informa **`espera_gravada=0`**. A expressão também não apareceu na busca textual desta cópia. A afirmação histórica “v5 nunca pousou” permanece sustentada apenas pelo anexo. [nao verificado] [nao verificado]

Zero gravado não significa zero obrigação pendente: o próprio anexo declara 47 ocorrências de `V_sem_A`. [nao verificado]

4. **Rótulo versus predicado — LEI-AKITA 8**

Sem o script, **não é possível certificar seu predicado nem atribuir-lhe uma inversão específica de precedência**. A lei exige medir pela função real, com rótulo correspondente ao universo contado. (`CLAUDE.md:35`; implementação da sonda [nao verificado])

Os pontos verificáveis para confrontá-lo com `T > A factual > N atual > V > R/S` são:

- **T antes de A:** linhas fora do fato são identificadas pelas vias `duplicata` e `motivo_retratado`; `fora_do_fato()` não depende de carimbo. Classificar pelo timestamp antes dessa identidade pode encobrir T. (`app/chamados/models.py:2401`; `app/chamados/models.py:3193`)
- **A factual:** `materializada_em` sozinho não prova aplicação. `estado_aplicacao_pergunta()` reconhece explicitamente carimbo de recusa sem batida; o juiz de lastro dispõe de verificações de testemunha factual. (`app/chamados/services/materializacao.py:434`; `app/chamados/juizes.py:645`; `app/chamados/juizes.py:789`)
- **N atual:** `recusa_motivo` também guarda recusa técnica; recusa humana histórica não basta para identificar a decisão vigente. A sequência contém respostas e validações e ordena por `ato_em, pk`. (`app/chamados/models.py:2567`; `app/chamados/models.py:3857`; `app/chamados/models.py:3889`)
- **A função atual não entrega a matriz:** exige pergunta tipo hora com texto e retorna estados de aplicação, sem implementar aquela sequência S/R/V/A/N/T. (`app/chamados/services/materializacao.py:430`; `app/chamados/services/materializacao.py:432`)

5. **O que exige outra leitura**

Permanecem indeterminados: classificação correta das 18.424 metades; quantidade de N; identidade dos pares nos grupos ambíguos; classes das metades fora dos pares únicos; sobreposição dos 28 `pendencia_admin`; e correspondência entre os 47 `V_sem_A` e os 45 rótulos V dos pares únicos. [nao verificado]

Para medir **disputas adicionais retidas**, falta uma leitura que associe cada par à disputa, deduplique os IDs, projete os estados com E1 e testemunha factual e compare, na mesma disputa, C7 atual com a obrigação ampliada. O agregado fornecido não contém esse cruzamento. (`app/chamados/models.py:1861`; `app/chamados/models.py:3839`; `app/chamados/juizes.py:645`; disponibilidade desse cruzamento [nao verificado])

## 09/11 14:50 · sessao 35b635788e2f · - · - · origem=Ronald (tmux)

- `estado_aplicacao_pergunta` ainda não implementa a projeção S/R/V/A/N/T: exige `tipo_resposta='hora'` e texto; trata `materializada_em` como aplicada, exceto `paridade_recusada`; pode responder `ja_satisfeita` por batida aprovada ou `bloqueada_paridade`, sem exigir validação. (`app/chamados/services/materializacao.py:432`, `:434`, `:438`, `:447`, `:452`)
- A factual não se prova apenas por `materializada_em`: o juiz de lastro pede testemunha externa, com caminhos para batida ligada, slot esperado e batida não retratada. (`app/chamados/juizes.py:645`, `:732`, `:734`, `:789`)
- T deve prevalecer: as vias fora do fato são `duplicata` e `motivo_retratado`; ambas podem ter carimbo, tornando inadequado classificar A primeiro apenas pelo timestamp. (`app/chamados/models.py:2401`, `:2518`)
- N fica escondido sobretudo em S quando não se lê E1: a recusa grava motivo e chama reabertura com `ato='recusa'`; a reabertura zera resposta, `respondida_em` e `validada_em`, preservando a foto na sequência. (`app/chamados/services/validacao.py:584`, `:591`; `app/chamados/services/disputa_emissao.py:1590`, `:1612`)
- Recusa histórica não basta para N atual: a sequência inclui respostas e validações posteriores e ordena por `ato_em`, `pk`. O campo `recusa_motivo` também recebe recusa técnica do materializador; portanto não identifica sozinho recusa humana vigente. (`app/chamados/models.py:3857`, `:3889`, `:2567`)
- O script `sonda_par.py` e a matriz `matriz_par` não foram encontrados pelo censo de nomes desta cópia; sua lógica exata, hash e correspondência dos rótulos permanecem **[não verificado]**.

## 09/11 14:50 · sessao e86b8c634a3e · - · - · origem=Ronald (tmux)

- C7 atual aceita somente disputa sem respostas sem veredito e sem fio vivo entre **todas** as perguntas; não consulta obrigação do par. [app/chamados/models.py:1856](/home/ronald/codex_ws/3803e66235912bd9ba1a7aa5f20ee94d1dc307f3/app/chamados/models.py:1856)
- “Resposta sem veredito” exige `validada_em is None`, `materializada_em is None`, resposta por timestamp **ou** texto não vazio, e dia não trancado. [app/chamados/models.py:1850](/home/ronald/codex_ws/3803e66235912bd9ba1a7aa5f20ee94d1dc307f3/app/chamados/models.py:1850)
- C6 mata imediatamente toda pergunta validada; assim, a validação sem aplicação pode desaparecer do fio atual. [app/chamados/juizes.py:1152](/home/ronald/codex_ws/3803e66235912bd9ba1a7aa5f20ee94d1dc307f3/app/chamados/juizes.py:1152)
- `fio_vivo` usa `any(pergunta_viva(p))`; basta outra pergunta viva na mesma disputa para ela já estar retida hoje. Logo somar pares de classes candidatas não mede disputas distintas nem incremento de retenção. [app/chamados/juizes.py:1262](/home/ronald/codex_ws/3803e66235912bd9ba1a7aa5f20ee94d1dc307f3/app/chamados/juizes.py:1262), [app/chamados/models.py:1861](/home/ronald/codex_ws/3803e66235912bd9ba1a7aa5f20ee94d1dc307f3/app/chamados/models.py:1861)
- S127 está em `chamados/juizes.py`, não `core/juizes.py`: disputa aberta barra o chamado se vazia ou com fio vivo; não chama C7 inteiro. [app/chamados/juizes.py:2126](/home/ronald/codex_ws/3803e66235912bd9ba1a7aa5f20ee94d1dc307f3/app/chamados/juizes.py:2126)
- `fechar` verifica resposta pendente e perguntas vivas **antes** do `atomic`/travas; não reconsulta C7 sob trava nesta cópia. [app/chamados/models.py:1894](/home/ronald/codex_ws/3803e66235912bd9ba1a7aa5f20ee94d1dc307f3/app/chamados/models.py:1894), [app/chamados/models.py:1913](/home/ronald/codex_ws/3803e66235912bd9ba1a7aa5f20ee94d1dc307f3/app/chamados/models.py:1913)
- Disputa já fechada: repetição idêntica devolve `False`; outra entrada gera erro, sem reabertura nessa porta. [app/chamados/models.py:1887](/home/ronald/codex_ws/3803e66235912bd9ba1a7aa5f20ee94d1dc307f3/app/chamados/models.py:1887)
- `ESPERA_PAR_INTERVALO` não consta da lista de vias do modelo; busca textual em `app`, `bin` e extensões textuais da cópia retornou zero ocorrências. A ausência global não tem linha positiva para citar. [app/chamados/models.py:2533](/home/ronald/codex_ws/3803e66235912bd9ba1a7aa5f20ee94d1dc307f3/app/chamados/models.py:2533); busca global [não verificado].

## 09/11 14:49 · sessao 4353177d1904 · - · - · origem=Ronald (tmux)

Inspeção somente leitura; nenhuma execução nem edição.

| Sítio | Adaptação necessária |
|---|---|
| `chamados/services/fio.py:225`, `:238` | Incluir intervalo no universo das disputas fechadas e consumir a obrigação da projeção canônica; `q_vivas` seguido de C6 elimina V legado. |
| `chamados/services/fio.py:335` | Já chama `estado_aplicacao_pergunta` por reexport do modelo; pode consumir os novos campos diretamente. |
| `chamados/services/acoes_chamado.py:348` | Manter a pergunta cuja projeção indique obrigação do par, além das demais perguntas vivas por C6. |
| `chamados/services/explicador.py:72` | Ramificar intervalo pela projeção antes de consultar `p.estado`. |
| `chamados/services/explicador.py:85` | A precedência da pendência sobre lastro produz W#5: anuncia ausência de plantio antes de perguntar pelo fato. |
| `chamados/services/explicador.py:137` | Substituir recomposição por `disputa.pode_fechar()`. |
| `chamados/services/declaracao_texto.py:221` | Guarda de declaração deve consumir capacidade canônica para intervalo; não importar bloqueio de espera da v5. |
| `chamados/services/declaracao_texto.py:471` | Acrescentar estado/capacidades à projeção, preservando o significado de `aceita`. |
| `chamados/services/declaracao_texto.py:687` | Aplicação textual singular precisa impedimento explícito antes de escrever resposta/validar; proposta de uma hora não autoriza ato de par. |
| `chamados/services/vigia_carimbo.py:106` | Para intervalo, classificar pelo resultado canônico. V permanece dívida; não importar classe de espera que o retire do alarme. |
| `chamados/services/vigia_fio.py:165` | A redução atual é só C6; consumir obrigação canônica para que V legado continue aparecendo no fio. |
| `chamados/juizes.py:1322` | Alimentar a porta do dia com obrigação do par, além de C6. |
| `chamados/juizes.py:1396` | Só acrescentar V em `vivas` é insuficiente: exige presença em `_no_app`, que pode excluir V. Transportar a capacidade/dono canônicos para permitir a obrigação administrativa sem inventar resposta. |
| `chamados/services/raiox.py:68` | Crédito do painel prefiltra `q_vivas`; obrigação V pode desaparecer antes de `verbos`. Migrar esse alimentador também, mantendo uma única projeção. |
| `chamados/services/passivo_validado.py:516` | Encaminhar seleção completa, processadas/aplicadas e operar uma vez por par. |
| `chamados/services/passivo_validado.py:427` | Inventariar/travar disputas, inclusive fechadas, antes do primeiro dia. |
| `chamados/services/passivo_validado.py:545` | Falha geométrica atualmente carimba recusa individual; para intervalo devolver relato conjunto sem fabricar meia decisão. |
| `chamados/services/passivo_validado.py:363` | DRY já evita chamar a unidade; preservar esse contrato. |
| `chamados/management/commands/auditar_invariantes_chamados.py:536`, `:1075` | I6/I18 têm filtros locais que podem esconder dívida por motivo preenchido; intervalos devem consumir classificação canônica onde a pergunta for aplicação/obrigação. |
| `chamados/management/commands/auditar_invariantes_chamados.py:1112` | I19 permanece técnico, sem conversão em estado do par. |
| `templates/chamados/partials/_card_atendimento.html:183` | Frase atual “conferir antes de lançar” deriva ausência de batida da pendência; exibir fato e conferência separadamente. |
| `templates/chamados/partials/modal_fio.html:221` | Badges atuais priorizam pendência/validação individual; ramificar pela projeção do intervalo. |

Atenção ao OL no O330: a irmã A é retirada do grupo em `passivo_validado.py:326`; exigir ambas no grupo de alvos inviabiliza reparação parcial. O inventário deve distinguir irmã A preservada como testemunha de irmã excluída por `FORA_DO_ESCOPO`; esta última deve impedir o conjunto (`passivo_validado.py:329`).

A catraca antiga só detecta comparações com `None`, filtros `isnull` e `if` direto de atributo, portanto não satisfaz resolução de aliases/auxiliares da catraca do par (`chamados/tests/test_contract_leitores_vitalidade.py:294`). Execução, concorrência e efeitos em produção: [nao verificado].

## 09/11 14:49 · sessao edf66e4dad99 · - · - · origem=Ronald (tmux)

Exploração concluída, sem edições nem execução de Python/Django.

| Ponto | Evidência e consequência |
|---|---|
| C7 atual | `chamados/models.py:1856`: apenas respostas sem veredito e `fio_vivo`; falta consumir obrigação do par. |
| `fechar` | Capturar `pelo_juiz = motivo is None` antes de `chamados/models.py:1905`; reler C7 após `trava_na_ordem`, em `chamados/models.py:1918`, preservando exceções nomeadas. |
| Fotos append-only | `reabrir_para_correcao`, `chamados/services/disputa_emissao.py:1571`, registra RECUSA/REABERTURA com foto antes de limpar resposta e validação; não escreve pendência/motivo. |
| Sequência | `DecisaoPergunta`, `chamados/models.py:3857`; somente RECUSA/REABERTURA aceitam foto em `chamados/models.py:3867`; porta de criação em `chamados/models.py:3943`. |
| Fixture | A fixture anexada usa campos existentes; manter `pk=960`, `nome='col960'`, relógio congelado, CNPJ sintético. CPF `[cpf]` normaliza para vazio: `core/identifiers.py:25` e `colaboradores/models.py:417`. |
| Ambiguidade | Duplicatas de mesmo marco só cabem no acervo anterior ao horizonte do índice: `chamados/models.py:3483`. Não inventar `instancia_marco`: `chamados/models.py:3439`. |
| S127 antigo | Dublês e expectativa precisam migrar para C7 em `chamados/tests/test_disputa_s127.py:17` e `chamados/tests/test_pergunta_viva_no_guarda.py:24`. |
| Teste legado afetado | `chamados/tests/test_fio_intervalo.py:115` espera materializar duas aceitas sem fornecer escala/DNA. Preservar o caso e explicitar abstinência ou fornecer identidade comprovável conforme a lei. |
| Selo de escritores | `chamados/tests/test_porta_validacao_pergunta.py:79` enumera atos com escrita exatos; novo `save` em helper diferente pode exigir ajuste fundamentado do selo. |
| Concorrência | Molde real com duas conexões em `chamados/tests/test_e1_identidade_corrida.py:22`; exige `TransactionTestCase`, porque `TestCase` mantém fixture em transação não visível às threads. |
| Molde de mutantes | Arquivo `e3_mutantes` sob `app/`: [nao verificado — busca não encontrou]. Nenhum caminho externo foi consultado. |

Primeiro aceite singular preservado em outros testes existentes: [nao verificado — busca não encontrou]. Execução, compatibilidade das fixtures e mortes de mutantes: [nao verificado]. [nao verificado]

## 09/11 14:49 · sessao 3e47de222653 · - · - · origem=Ronald (tmux)

Leitura concluída; nenhuma edição nem execução de Python/manage.py.

| Entrada | Adaptação necessária |
|---|---|
| Validar detalhe/modal | Casca `acao_validar_pergunta` já traduz `ok=False`; pode encaminhar pedido explicitamente conjunto à porta do par, mantendo singular recusado. `chamados/services/acoes_disputa.py:19`, `:46`. |
| Recusar | Acrescentar guarda `not r['ok']`: hoje estados novos acabam em toast de sucesso. `chamados/services/acoes_disputa.py:82`, `:87`. |
| Corrigir | Desviar intervalo antes de qualquer nota/resposta: escreve nota em `:359`, responde em `:371` e só depois valida em `:379`. Singular deve sair antes de `:352`. `chamados/services/acoes_disputa.py:345`. |
| Reabrir questionário | `_corrig(d)` pode selecionar uma irmã só; formar e validar universo de pares antes de `_reabrir(qs)`. `chamados/services/acoes_disputa.py:96`, `:103`. |
| Inline | Universo atual vem de C6, portanto pode esconder V legado; laço chama porta singular. Precisa agrupar uma vez por identidade, mantendo universo autorizado. `chamados/views_cobrar.py:169`, `:208`. |
| Lote JSON/tela/chat | Convergem em `validar_lote_classe_a`; correção central alcança JSON. `chamados/views_cobrar.py:247`, `:261`. |
| Comando classe A | `--limite` corta perguntas, podendo separar irmãs; APPLY chama singular. Selection incompleta precisa impedir conjunto. `chamados/management/commands/validar_classe_a.py:69`, `:100`. |
| Fio | Novas ações precisam entrar na porteira contextual e preservar política de disputa fechada. `chamados/services/fio.py:558`, `:573`, `:600`. |
| Detalhe | Gates usam `chamado_que_lista` da pergunta âncora; serviço precisa conferir ambas no escopo autorizado. `chamados/views.py:501`, `:518`, `:536`. |

Templates e resposta:

- `_pergunta_card.html` tem formas singulares em `:76`, `:102`, `:125`; modal em `:264`, `:280`, `:291`, `:302`. Um ramo exclusivo de intervalo com include conjunto evita oferecer meia validação.
- O fragmento htmx atual recarrega apenas a âncora com `{perg, pode_agir}`; precisa renderizar conjunto ou atualizar irmã por OOB. `chamados/views.py:399`, `:404`.
- O cartão do colaborador afirma “antes de lançar” só pela pendência, sem perguntar pelo lastro. `templates/chamados/partials/_card_atendimento.html:183`.
- O modal dá prioridade à pendência sobre aplicação. `templates/chamados/partials/modal_fio.html:221`, `:226`.

Casos antigos a inspecionar para reescrever expectativas, sem apagar: `chamados/tests/test_o306_marco_do_motivo_nao_e_do_vizinho.py:137`, `:151`, `:159`, `:176`, `:185`; `chamados/tests/test_s139.py:26`; `chamados/tests/test_marco_por_motivo.py:59`.

Comportamento executado, cobertura completa de portas e efeitos concorrentes: [nao verificado].

## 09/11 14:46 · sessao 5a6bf4616dff · BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS · leitura · origem=Code

**Matriz contra o MAIN informado.** `FALTA` significa comportamento a construir; não significa comportamento já comprovado. Os testes propostos abaixo não foram executados. **[nao verificado: implementação e execução futuras]** [nao verificado]

Há uma diferença de base que precisa ficar explícita: no MAIN, `materializacao.py:1433` é um retorno, e `declaracao_texto.py:485` seleciona marcos aceitos. A escrita de `ESPERA_PAR_INTERVALO` está no **diff v5 fornecido**, não nesses pontos do MAIN. O materializador atual opera uma pergunta, e a projeção atual publica apenas `aceita`. (`app/chamados/services/materializacao.py:1158`, `app/chamados/services/materializacao.py:1433`, `app/chamados/services/declaracao_texto.py:471`, `app/chamados/services/declaracao_texto.py:485`; ausência do símbolo no censo: **[nao verificado: ausência sem endereço próprio]**)

**1. Estados e ESMERIL-SEMÂNTICA**

Os seis nomes solicitados precisam ser uma **projeção derivada**: a propriedade existente só distingue pendente, respondida, validada e resolvida. Não existe hoje uma enumeração exclusiva com os seis estados. (`app/chamados/models.py:3041`)

| Código | Estado da metade | Fonte existente no MAIN | Contrato da leitura nova |
|---|---|---|---|
| S | Sem resposta | `resposta_colab`, `respondida_em`; reabertura limpa ambos. (`app/chamados/models.py:2455`, `app/chamados/services/disputa_emissao.py:1590`) | Ausência de declaração atual; não confundir com recusa humana histórica. **FALTA** |
| R | Respondida | A porta grava resposta e instante. (`app/chamados/services/disputa_emissao.py:1464`) | Declaração presente, sem aceite/aplicação atual. **FALTA** |
| V | Validada | `validada_em/por`, escritos por `_carimbar_validacao`, com decisão E1. (`app/chamados/services/validacao.py:127`, `app/chamados/services/validacao.py:139`) | Aceite sem aplicação comprovada: dívida legada, nunca “aguardando” persistido. **FALTA** |
| A | Aplicada | Carimbo `materializada_em/por/via` pela porta; testemunha julgada por `_pergunta_materializada_de_fato`. (`app/chamados/models.py:3103`, `app/chamados/juizes.py:645`) | Para intervalo, exige resolução factual da metade; carimbo de identidade/retratação não prova batida. **FALTA** |
| N | Recusada | Recusa humana escreve motivo e chama reabertura com `ato='recusa'`; a projeção perde resposta/validação. (`app/chamados/services/validacao.py:584`, `app/chamados/services/validacao.py:593`, `app/chamados/services/disputa_emissao.py:1590`) | Ler a sequência de decisões e respostas posteriores; `recusa_motivo != ''` não identifica recusa atual. **FALTA** |
| T | Retratada/fora do fato | `via_resolucao`, `fora_do_fato()`; inclui duplicata e motivo retratado. (`app/chamados/models.py:2401`, `app/chamados/models.py:3201`) | Terminalidade de identidade, separada de aplicação. Não ressuscitar a linha. (`app/chamados/models.py:3180`, `app/chamados/models.py:3206`) |

**Ordem da projeção proposta:** primeiro identidade fora do fato; depois aplicação factual; depois decisão atual de recusa; depois aceite; depois resposta/ausência. Preservar os indicadores originais e a trilha como evidência, sem apagá-los para fabricar exclusividade. **FALTA — [nao verificado: desenho]**

| Campo | Mais de um significado hoje | Consequência obrigatória |
|---|---|---|
| `validada_em` | Registra aceite e também mata imediatamente a pergunta no C6, mesmo sem aplicação. (`app/chamados/services/validacao.py:134`, `app/chamados/juizes.py:1153`) | Não usar morte no C6 como prova de aplicação do par. **FALTA** |
| `materializada_em` | Carimbo de aplicação e de resolução por identidade, inclusive retratação. (`app/chamados/models.py:3103`, `app/chamados/models.py:3185`) | Não apresentar toda materializada como batida aplicada. **FALTA** |
| `recusa_motivo` | Recusa materializadora, recusa humana, escape e descrição de identidade. (`app/chamados/services/materializacao.py:1102`, `app/chamados/services/validacao.py:584`, `app/chamados/services/disputa_emissao.py:1494`, `app/chamados/models.py:3188`) | Não usá-lo como enumeração do par. Motivo do ato conjunto vai à decisão/trilha. **FALTA** |
| `pendencia_admin` | O comentário diz “recusou plantar”; o rótulo diz “plantada pelo motivo”. O escritor pode gravar pendência e continuar até `registrar_batida`. (`app/chamados/models.py:2555`, `app/chamados/models.py:2562`, `app/chamados/services/materializacao.py:850`, `app/chamados/services/materializacao.py:927`) | Conferência técnica e existência de batida devem ser apresentadas separadamente. **FALTA** |
| `respondida_em` / resposta | Timestamp sozinho não garante conteúdo; a aplicação exige texto não vazio. (`app/chamados/services/materializacao.py:829`) | Incoerência entre texto e instante deve gerar insumo inconsistente, sem aceite/aplicação automática. **FALTA** |
| Fora do fato | Também recebe carimbo e pode ser reconhecida pelo juiz como resolvida sem testemunha de batida. (`app/chamados/models.py:3185`, `app/chamados/juizes.py:670`) | Preservar resolução de identidade sem chamá-la de intervalo aplicado. **FALTA** |

O W semântico já tem base no MAIN: o explicador afirma “NAO plantou” diante da pendência antes de perguntar pelo lastro, embora o ramo possa ter plantado. (`app/chamados/services/explicador.py:85`, `app/chamados/services/explicador.py:87`, `app/chamados/services/explicador.py:93`, `app/chamados/services/materializacao.py:850`, `app/chamados/services/materializacao.py:927`)

**2. Autoridades da matriz**

| Pergunta | Autoridade existente | Por que não resolve sozinha o par / destino |
|---|---|---|
| Quais são as duas perguntas da mesma pausa? | Motivos → S/hii e E/hfi; pausas e pares canônicos do DNA. (`app/chamados/juizes.py:885`, `app/escala/models.py:1166`, `app/escala/models.py:1197`) | Adaptador `identificar_par_intervalo`: **FALTA no MAIN**. Os motivos não carregam índice da pausa. (`app/chamados/services/declaracao_texto.py:48`) |
| Qual o estado de aplicação? | `estado_aplicacao_pergunta`. (`app/chamados/services/materializacao.py:425`) | Hoje avalia uma pergunta e sua vizinha. Deve delegar intervalo à avaliação única do conjunto. Extração do juiz derivado do par: **FALTA** |
| A testemunha factual existe? | `_pergunta_materializada_de_fato`. (`app/chamados/juizes.py:645`) | Reutilizar sua autoridade de lastro, distinguindo as vias de identidade; não duplicar procura de batida nos leitores. (`app/chamados/juizes.py:670`, `app/chamados/juizes.py:732`) |
| A pergunta está viva? | C6, `pergunta_viva`. (`app/core/juizes.py:266`) | Não responde aplicação do par: `validada_em` já devolve falso. (`app/chamados/juizes.py:1153`) |
| A disputa pode fechar? | C7, `DisputaSupervisao.pode_fechar`. (`app/core/juizes.py:267`, `app/chamados/models.py:1856`) | Ampliar a autoridade existente para consumir obrigações derivadas do par. **FALTA** |
| O chamado pode encerrar com essa disputa? | S127, `disputa_viva_do_chamado`. (`app/chamados/juizes.py:2126`) | Hoje recompõe pelo `fio_vivo`; deve consumir C7 inteiro, conservando a exceção de disputa vazia. (`app/chamados/juizes.py:2137`) |
| O vão entre batidas foi intervalo? | `intervalos_do_turno`. (`app/core/juizes.py:202`) | Julga batidas, não identifica duas declarações nem decide aceite administrativo. Não serve como substituto da identidade das perguntas. (`app/core/juizes.py:202`, `app/chamados/services/declaracao_texto.py:48`) |

O juiz derivado proposto é a ampliação da autoridade de aplicação existente, com identidade alimentada pelas fontes acima; telas, C7 e vigias consomem sua resposta. Não haverá um juiz de espera em cada consumidor. **FALTA — [nao verificado: desenho]**

| Identidade | Contrato |
|---|---|
| Par único | Mesma disputa e dia do turno, duas metades inequívocas, marcos pertencentes à mesma pausa declarada. **FALTA**; fontes: `app/chamados/models.py:2424`, `app/chamados/models.py:2431`, `app/escala/models.py:1197` |
| Ambíguo | Multiplicidade de candidatas ou duas pausas sem índice que prove a associação: abstenção explícita. Não escolher primeiro PK nem aplicar avulsa. **FALTA**; lacuna existente: `app/chamados/services/declaracao_texto.py:48` |
| Sem par | Pergunta de intervalo sem contraparte identificável: informar a falta da identidade; nenhuma aplicação individual. **FALTA**; unidade atual individual: `app/chamados/services/materializacao.py:1158` |
| Contraparte retratada | A identidade ativa pode deixar de formar par, mas o aceite residual da metade ainda no fato continua sendo obrigação de fechamento. **FALTA**; risco atual: `app/chamados/juizes.py:1153`, `app/chamados/models.py:1851` |
| Índice inventado | Não preencher `instancia_marco` arbitrariamente para resolver ambiguidade: o modelo exige a derivação do motivo. (`app/chamados/models.py:3436`, `app/chamados/models.py:3441`) |

**3. Contratos de operação, persistência e fechamento usados na matriz**

Todas as operações seguintes são **FALTA como unidade PAR**. As referências identificam as portas existentes a reutilizar.

| Código | Operação e persistência | Tela e prova |
|---|---|---|
| O0 | **Nenhuma operação administrativa do par.** Zero alteração de validação, materialização, pendência ou motivo para representar espera. A resposta continua pela porta existente. (`app/chamados/services/disputa_emissao.py:1464`) | “Aguardando a outra metade”, derivado das declarações; indicar qual falta. Teste `test_espera_nao_escreve`: fotografar campos, decisões, notas e eventos antes/depois. **FALTA** |
| OV | **Validar par.** Duas declarações presentes; travar/reler ambas antes do primeiro carimbo. Validar, registrar/reutilizar as duas batidas pela porta, julgar conjunto e lastro, carimbar ambas e só então reconciliar. Qualquer falha desfaz o ato inteiro. Portas: `app/chamados/services/validacao.py:127`, `app/ponto/registro_batida.py:94`, `app/chamados/models.py:3103` | “Intervalo aplicado” somente após as duas testemunhas. `test_validar_par_atomico`: nenhuma observação intermediária contém só uma aplicação. **FALTA** |
| ON | **Recusar par.** Motivo obrigatório, foto anterior e decisões `RECUSA` das duas metades no mesmo ato; reabertura pela porta. Motivo conjunto em `ato_motivo`/trilha, sem transformá-lo em estado de `recusa_motivo`. (`app/chamados/services/disputa_emissao.py:1590`, `app/chamados/services/disputa_emissao.py:1612`, `app/chamados/models.py:3876`) | “Par recusado; corrigir as declarações”. Não anuncia aplicação nem quitação automática. `test_recusar_par_preserva_fotos`: duas projeções devolvidas, histórico intacto. **FALTA** |
| OC | **Corrigir par.** Ato explícito sobre o conjunto; preserva decisões anteriores e usa as portas de resposta, reabertura e aplicação pertinentes. Não preenche hora ausente por inferência. (`app/chamados/services/disputa_emissao.py:1464`, `app/chamados/services/disputa_emissao.py:1590`, `app/chamados/models.py:3914`) | Mostra as duas metades e o impedimento concreto. `test_corrigir_par_sem_inferencia`: nenhum horário sintético e nenhuma alteração avulsa. **FALTA** |
| OL | **Corrigir aplicação parcial legada.** Rejulgar o conjunto sob a mesma porta; preservar/reutilizar testemunha válida, sem duplicar a metade existente. Dedup e reuso já existem. (`app/ponto/registro_batida.py:113`, `app/chamados/services/materializacao.py:859`) | “Aplicação parcial; corrigir o par”. Não oferecer “validar a que falta”. `test_legado_parcial_reusa_testemunha`: testemunha anterior intacta, resultado conjunto comprovado. **FALTA** |
| OT | **Corrigir obrigação cuja irmã saiu do fato.** Não ressuscitar T. Se o fato continuar cobrado, a identidade deve ser recomposta pelas portas existentes; se deixou de existir, sua resolução deve alcançar a obrigação residual com trilha. (`app/chamados/models.py:3193`, `app/chamados/models.py:3206`) | “Contraparte fora do fato; revisar o par”. `test_irma_retratada_nao_apaga_divida`: aceite residual continua retendo até resolução explícita. **FALTA** |
| OI | **Nenhuma no replay de par aplicado.** Mesmo estado, nenhuma batida ou decisão adicional. Uma revisão com conteúdo diferente é outro ato explícito. Imutabilidade: `app/chamados/models.py:3903` | “Intervalo aplicado”. `test_replay_par_aplicado`: fotografias iguais e zero duplicata. **FALTA** |

| Fechamento | MAIN | Contrato da construção |
|---|---|---|
| C7 | Exige ausência de respostas sem veredito e de perguntas vivas. (`app/chamados/models.py:1861`) | **FALTA:** acrescentar a retenção das obrigações do par, inclusive aceite sem aplicação, independentemente de `pendencia_admin` e `recusa_motivo`. |
| `Disputa.fechar` | Examina respostas/vivas antes do `atomic`; trava depois. (`app/chamados/models.py:1894`, `app/chamados/models.py:1903`, `app/chamados/models.py:1918`) | **FALTA:** no percurso normal, reler C7 sob a trava antes da escrita de fechamento. Preservar as portas nomeadas de exceção e seus contratos. (`app/chamados/models.py:1884`) |
| S127 | Retém disputa vazia ou `fio_vivo`. (`app/chamados/juizes.py:2138`) | **FALTA:** disputa vazia ou C7 retendo. Nenhuma recomposição local do estado do par. |
| `transicionar('fechado'/'resolvido')` | Ambos passam por S127; a guarda adicional de FURO é exclusiva de `resolvido`. (`app/chamados/models.py:148`, `app/chamados/models.py:181`) | **FALTA:** ambos precisam parar pela retenção S127/C7 do par, antes de `_aplicar`. (`app/chamados/models.py:199`) |
| Liberação | C7 continua autoridade da disputa inteira. (`app/chamados/models.py:1856`) | “Liberar” abaixo significa apenas que **este par** não retém; outras perguntas, competência e guardas existentes continuam sendo julgadas. **FALTA** |

**4. Matriz 6×6 reduzida**

A tabela contém as **21 classes por simetria de estados**, cobrindo as 36 posições. A simetria não troca os papéis temporais: saída continua S/hii e volta E/hfi. (`app/chamados/juizes.py:885`)

Não há escritor do par no MAIN que permita declarar combinações entre metades impossíveis: validar, recusar e retratar operam individualmente. A existência dessas portas não prova a ocorrência de cada combinação em produção. (`app/chamados/services/validacao.py:435`, `app/chamados/services/validacao.py:593`, `app/chamados/models.py:2062`; ocorrência populacional: **[nao verificado]**)

**Todas as células de operação/tela/fechamento/prova abaixo são FALTA para o PAR**, com os contratos O acima. O juiz é a ampliação de `estado_aplicacao_pergunta` descrita na seção 2. (`app/chamados/services/materializacao.py:425`)

| Combinação, inclusive transposta | Evidência de leitura/independência no MAIN | Operação → persistência | Tela derivada | Fechamento do par | Teste sugerido + asserção | [nao verificado]
|---|---|---|---|---|---|
| S/S | Respostas e instantes. `disputa_emissao.py:1590` | O0 | Aguardando as duas respostas | Reter enquanto cobrado | `test_SS`: zero aceite, batida ou espera gravada |
| S/R | Resposta individual. `disputa_emissao.py:1464` | O0 | Aguardando a metade S | Reter | `test_SR`: declarar uma não valida nenhuma |
| S/V | Aceite individual. `validacao.py:435` | OC para devolver/corrigir legado; sem validação parcial | Aceite legado sem par pronto | Reter, mesmo com motivo técnico | `test_SV`: C7 falso e zero aplicação |
| S/A | Aplicação individual. `materializacao.py:1220` | OL | Aplicação parcial legada | Reter até resolver conjunto | `test_SA`: não completar silenciosamente |
| S/N | Recusa limpa resposta. `validacao.py:593` | O0; nova declaração pela porta | Aguardando respostas/correção | Reter enquanto cobrado | `test_SN`: recusa histórica não vira resposta |
| S/T | Fora do fato é terminal. `models.py:3201` | Nenhuma aplicação; identidade incompleta | Contraparte fora do fato | Reter cobrança remanescente | `test_ST`: sem fallback avulso |
| R/R | Validação atual por pergunta. `validacao.py:435` | OV / ON / OC | Par pronto para decisão | Reter até ato completo | `test_RR`: um comando decide ambas |
| R/V | Aceite mata só a validada. `juizes.py:1153` | OV / ON / OC sobre ambas | Par com aceite legado | Reter até aplicação/decisão conjunta | `test_RV`: não preservar meia validação nova |
| R/A | Aplicação individual. `materializacao.py:1220` | OL | Aplicação parcial; revisar conjunto | Reter | `test_RA`: reuso não duplica primeira |
| R/N | Recusa e resposta são atos distintos. `models.py:3857` | OC; OV só após duas declarações atuais | Corrigir as declarações | Reter | `test_RN`: não recuperar hora antiga como atual |
| R/T | Identidade terminal. `models.py:3180` | Nenhuma aplicação; recompor identidade primeiro | Sem par ativo completo | Reter resposta/cobrança remanescente | `test_RT`: R não aplica avulsa |
| V/V | C6 mata ambas pelo aceite. `juizes.py:1153` | OV / ON / OC para legado | Aceites sem aplicação | **Reter**; não são quitação | `test_VV`: C7 falso sem duas aplicações |
| V/A | Aceite e aplicação são independentes. `models.py:3043` | OL | Aplicação parcial legada | Reter | `test_VA`: metade aplicada não quita outra | [nao verificado]
| V/N | Recusa da outra não desfaz aceite. `validacao.py:593` | OC sobre conjunto | Aceite residual e declaração recusada | Reter | `test_VN`: motivo de recusa não oculta V |
| V/T | V mata no C6; T também pode morrer por identidade. `juizes.py:1153`, `juizes.py:1164` | OT | Aceite sem aplicação; irmã fora do fato | **Reter — RED W#3** | `test_VT_pendencia`: C7/S127/fechar retêm com motivo não-ESPERA |
| A/A | Carimbo e testemunha. `models.py:3103`, `juizes.py:645` | OI; OC somente para outro ato explícito | Intervalo aplicado | Não retém por aplicação | `test_AA`: replay idêntico, zero nova linha/batida | [nao verificado]
| A/N | Recusa não é reversão de batida. `validacao.py:573` | OL | Aplicação parcial e recusa | Reter até revisão conjunta | `test_AN`: recusar não apaga chão |
| A/T | Retratação não prova aplicação da irmã. `models.py:3185` | OT / OL | Conjunto sem resolução factual integral | Reter obrigação residual | `test_AT`: sem ressurreição de T nem quitação presumida |
| N/N | Recusa devolve perguntas. `disputa_emissao.py:1590` | Nenhuma no replay; aguardar novas declarações | Par recusado, aguardando correção | Recusa não quita cobrança | `test_NN`: repetir não duplica RECUSA |
| N/T | Decisão e identidade distintas. `models.py:3857`, `models.py:3201` | Nenhuma aplicação; revisar identidade/cobrança | Recusa e contraparte fora do fato | Reter cobrança remanescente | `test_NT`: nenhuma hora histórica aplicada | [nao verificado]
| T/T | Ambas fora do fato. `models.py:3201` | Nenhuma | Fato retirado; histórico preservado | Não retém por essas identidades | `test_TT`: C7 conserva regras das demais perguntas |

Nas referências abreviadas da matriz, `models.py`, `juizes.py` e os serviços pertencem a `app/chamados/`.

| Situação adicional | Juiz/operação | Persistência, tela, fechamento e prova |
|---|---|---|
| **AMBÍGUO** | Identificador e avaliação do par: **FALTA**. Não selecionar candidatas arbitrariamente. Lacuna documentada em `app/chamados/services/declaracao_texto.py:48` | Zero aplicação/aceite do pedido singular; tela explica ambiguidade; obrigações existentes continuam retidas. `test_ambiguo_nao_escolhe_pk`: quatro candidatas intactas. **FALTA** |
| **Intervalo SEM PAR** | Identificador: **FALTA**. A aplicação avulsa atual não serve. (`app/chamados/services/materializacao.py:1158`) | Recusa falante: falta contraparte identificada; não dizer “colada”. `test_sem_par_nao_aplica_avulsa`: zero escrita e impedimento explícito. **FALTA** |
| **Resposta/instante incoerentes ou carimbo sem lastro** | Fontes de resposta e lastro existentes. (`app/chamados/services/materializacao.py:829`, `app/chamados/juizes.py:645`) | Não forçar enquadramento saudável nos seis rótulos; devolver impedimento e dívida real. `test_insumo_incoerente`: não aplicar/fechar por ignorância. **FALTA** |

| “Impossível” | Escritor que impede |
|---|---|
| V persistida por **novo** aceite sem aplicação integral | **FALTA:** porta do par deve manter validação e aplicação no mesmo `atomic`; falha reverte ambas. A porta atual 

[truncado em 20000 de 51628 caracteres]

## 09/11 14:40 · sessao 685913bd30a3 · - · - · origem=Ronald (tmux)

Censo realizado por AST em memória, sem importar Django: resolução de `ImportFrom.asname`, `Import.asname`, atributos e atribuições simples; 25 chamadas executáveis de validar/recusar/corrigir/materializar/reparar em runtime. Reflexão, nomes montados dinamicamente e chamadas externas à cópia: **[nao verificado]**.

| Entrada MAIN | Comportamento hoje | Destino no desenho do par |
|---|---|---|
| Detalhe: `app/chamados/views.py:501`, `:518`, `:536` | Despacha validar, recusar e corrigir uma pergunta para as cascas. | Operar par inteiro com identidade comprovada; pedido singular recebe recusa falante antes de escrita. **FALTA**. |
| Fio/modal: `app/chamados/services/fio.py:609`, `:638`, `:639` | Mesmas três cascas; `:609` passa `sem_reabrir` quando disputa estava fechada. | Mesma porta do par e mesmo tratamento de disputa fechada; preservar política de reabertura. **FALTA**. |
| Cascas: `app/chamados/services/acoes_disputa.py:19`, `:80`, `:379` | Chamam `validar_pergunta`, `recusar_pergunta`; correção responde uma pergunta e depois valida. | Formulário coleta ambas as horas e motivo; serviço decide o conjunto. Correção atual não serve como atomic do par: primeiro responde em `:371`, depois valida em `:379`. |
| Aplicar declaração de texto: `app/chamados/views.py:480` → `app/chamados/services/declaracao_texto.py:690` | Abre/acha pergunta, eventualmente grava resposta em `:687`, valida uma em `:690`. | Uma proposta de uma hora não autoriza validar meia pausa: apresentar par e exigir dados completos ou recusa falante. **FALTA**. |
| Validar inline: `app/chamados/views_cobrar.py:208` | Laço de perguntas vivas respondidas, selecionadas em `:169`; uma chamada por item. | Agrupar por identidade; processar cada par uma vez; nunca completar seleção com irmã fora do universo autorizado. **FALTA**. |
| Lote JSON: `app/chamados/views_cobrar.py:261` | Chama `validar_lote_classe_a`. | Mesmo agrupamento por par. **FALTA**. |
| Lote da tela: `app/chamados/views_cobrar.py:381` | Chama o mesmo lote com refs do POST. | Contagem de pares e perguntas separada; seleção incompleta recebe motivo. **FALTA**. |
| Lote do chat/IA: `app/core/views_ia.py:169` | Chama o mesmo lote após gate de permissão em `:165`. | Mesma porta do par; proposta precisa identificar o conjunto. **FALTA**. |
| Escritor do lote: `app/chamados/services/validacao.py:746` | Reclassifica cada pergunta em `:735`; altera `request.POST['pergunta_id']` em `:745`; conta pela validação em `:753`. | Elegibilidade e decisão por conjunto; uma irmã fora da classe/seleção impede aplicar ambas. **FALTA**. |
| Comando assinado: `app/chamados/management/commands/validar_classe_a.py:100` | Chama validação por pergunta; exige usuário em `:90`, DRY retorna em `:85`. | DRY lista pares/ambíguos/sem par; APPLY exige par completo e assinado. **FALTA**. |
| Regularizar detalhe: `app/chamados/services/acoes_disputa.py:139` | Chama materialização granular da disputa. | Processar unidades par sem validar respostas novas implicitamente; legado validado segue matriz de correção. **FALTA**. |
| Regularizar fio: `app/chamados/services/fio.py:670` | Mesmo materializador, importado por reexport de `chamados.models` em `:663`. | Mesmo destino. **FALTA**. |
| Validação individual: `app/chamados/services/validacao.py:463`, `:469` | Primeiro materializa a pergunta atual; depois varre a disputa. | Ramificação de intervalo deve impedir ação singular antes de `_carimbar_validacao`, que hoje ocorre em `:435`; ato conjunto substitui ambos caminhos para intervalo. **FALTA**. |
| Varredura: `app/chamados/services/materializacao.py:1376` | Seleciona respondidas+validadas+sem materialização em `:1369`, aplica individualmente. | Identificar, travar e processar par uma vez; nunca cair em aplicação avulsa por identidade ausente/ambígua. **FALTA**. |
| O330: `app/chamados/services/passivo_validado.py:516` | Dentro do atomic do dia (`:511`), chama unidade individual; mede geometria depois em `:536`. | Par completo dentro do universo do reparador, mantendo atomic do dia/competência, lastro e geometria. Não puxar irmã excluída. **FALTA**. |
| Comando O317/O330: `app/chamados/management/commands/materializar_passivo_validado.py:101`, `:110` | Chama `reparar`; DRY não chama unidade, conforme `app/chamados/services/passivo_validado.py:359`. | Preservar DRY sem escrita e escopo do reparador; relatório passa a contar unidades par. **FALTA**. |
| API de foto: `app/api/foto_recebida.py:51`; chamadas de fachada em `app/api/views.py:886`, `app/api/views_core.py:876` | Universo nominal é `foto_ausente` (`foto_recebida.py:61`, `:73`); ramo `:79` usa todas as perguntas quando `d.motivos` fica sem outros motivos (`:70`). | Não é endpoint nominal de intervalo. Porta central deve recusar meia pausa caso pergunta de intervalo alcance esse universo; foto não pode fornecer horas sintéticas. Entrada efetiva de intervalo por essa inconsistência: **[nao verificado]**. |
| Cron de validação do admin | `validar_classe_a` está fora do pipeline: `app/config/crons.py:1335`; texto diz ato assinado, nunca rotina em `:1336`. | Nenhum cron novo que valide par; rotina só observa estados derivados. |
| Cron do reparador | `materializar_passivo_validado` também fora do pipeline: `app/config/crons.py:825`; ato sob aval em `:826`. | Mantém-se ato, sem automatizar validação. |
| Observadores/fechamento | `auditar_invariantes_chamados`: `app/config/crons.py:542`; `fechar_becos_disputa --alarme`: `:663`; vigia fio: `:1713`; vigia carimbo: `:1751`. | Leem juiz derivado; beco já consulta C7 em `app/chamados/services/becos.py:33`, `:56`. |

**Atomic e portas existentes reaproveitáveis**

| Propriedade | MAIN |
|---|---|
| Validação e tentativa de aplicação no mesmo atomic | `app/chamados/services/validacao.py:425`; trava em `:431`; carimbo em `:435`; aplicação em `:463`. |
| Atomic da aplicação individual e proteção de reentrada | `app/chamados/services/materializacao.py:1220`. Para o par precisa cobrir as duas perguntas antes da primeira escrita: **FALTA**. |
| Lastro após plantio | `app/chamados/services/materializacao.py:1233`; reprovação levanta dentro do atomic em `:1239`. |
| Carimbo pela porta C6 | `app/chamados/services/materializacao.py:1277`. |
| Batida pela porta única | `app/chamados/services/materializacao.py:927`; flags `autoridade_humana=False` em `:929`, `fato_de_chao=False` em `:930`. |
| Vínculo da batida à pergunta | `app/chamados/services/materializacao.py:948`, por `anotar`. |
| Reconciliação hoje acontece após uma metade | `app/chamados/services/materializacao.py:974`; deve passar para depois das duas escritas e carimbos. **FALTA**. |
| Dedup existente | `app/ponto/registro_batida.py:106`, `:113`: mesmo tipo ±60s inclusivos; retorna existente em `:115`. |
| Guardas de competência e colisão continuam na porta | `app/ponto/registro_batida.py:132`, `:145`, `:150`. |
| O330 já julga geometria do conjunto do dia | `app/chamados/services/passivo_validado.py:536`, `:537`; não resolve a recusa individual anterior de cada metade em `:516`. |

**Trilha: detalhe necessário para não prometer uma linha física impossível pelo modelo existente**

| Contrato | Fonte |
|---|---|
| Decisão existente pertence a uma pergunta, não a um par | FK obrigatória em `app/chamados/models.py:3871`; atos em `:3857`. Uma entidade/linha própria do par: **FALTA**. |
| Validação grava uma linha E1 por pergunta | `app/chamados/services/validacao.py:139`. Validar duas metades preservando esse contrato gera duas linhas E1, uma por metade. |
| Recusa grava uma linha por pergunta, com foto anterior | `app/chamados/services/disputa_emissao.py:1612`; comentário explícito “UMA linha por ato, recusa, nunca reabertura + outra” em `:1595`. |
| Recusa hoje limpa resposta e validação | `app/chamados/services/disputa_emissao.py:1590`, update em `:1629`. Portanto “recusada” atual não é resposta mantida mais flag; a foto mora na decisão. |
| Imutabilidade | `app/chamados/models.py:3903`, `:3908`; porta de append `:3943`. |
| Preservar legado antes de substituir projeção | `app/chamados/models.py:3914`; validação chama em `app/chamados/services/validacao.py:133`. |
| Idempotência | Prova deve exigir repetição com zero linhas E1 adicionais, uma linha de cada ato por metade; se requisito exigir “uma linha do par”, uma ocorrência consolidada contendo ambos IDs pode ser desenhada, mas vínculo/porta do ato do par são **FALTA**. Existência desse recurso: **[nao verificado]**. |

A API pública de validar/recusar **intervalo** dedicada não apareceu nesse censo; `api/foto_recebida.py:51` é o único chamador direto de `validar_pergunta` sob `app/api` encontrado por AST. Portas externas, reflexão e execução real permanecem **[nao verificado]**.

## 09/11 14:40 · sessao 80dc16e3dafe · - · - · origem=Ronald (tmux)

| Achado | Evidência MAIN |
|---|---|
| **FALTA escritor de espera no MAIN.** Não há símbolo `ESPERA_PAR_INTERVALO` nem `aguarda_outra_metade_intervalo` no censo AST dos fontes Python runtime; a atribuição indicada no enunciado pertence ao diff v5 fornecido. Ausência por censo: **[nao verificado: não há arquivo:linha para símbolo inexistente]**. | MAIN tem `_materializar_uma` individual em `app/chamados/services/materializacao.py:1158`; projeção apenas `aceita` em `app/chamados/services/declaracao_texto.py:471`. |
| **Não apagar a pendência técnica existente.** O campo significa conferência de tipo; seu próprio rótulo reconhece batida plantada pelo motivo. | `app/chamados/models.py:2561`; `app/chamados/models.py:2564`. |
| **ESMERIL já necessário no MAIN:** explicador/cartão tratam pendência como ausência de plantio, enquanto escritor pode persistir pendência e continuar plantando. | `app/chamados/services/explicador.py:85`; `app/chamados/services/explicador.py:87`; `app/templates/chamados/partials/_card_atendimento.html:183`; `app/chamados/services/materializacao.py:850`; `app/chamados/services/materializacao.py:927`. |

Censo AST executado em memória: `app/**/*.py`, excluindo `tests`, `migrations`, `__pycache__`; reconheceu `Import`/`ImportFrom` com apelidos, atribuições simples de nomes, atributos Store/Load, argumentos ORM, `getattr`/`setattr` e chamadas às portas de recusa/identidade. Reflexão, código gerado, atribuição dinâmica e módulos fora desse universo: **[nao verificado]**.

| Escritor semântico MAIN | Atribuição/porta | Significado atual; destino no desenho |
|---|---|---|
| `_materializar_resposta_hora` | `app/chamados/services/materializacao.py:850` | Grava juntos `pendencia_admin` e `recusa_motivo` técnicos; preservar somente esse sentido. A escrita não significa espera. `app/chamados/models.py:2561`. |
| `_carimbar_recusa` | `app/chamados/services/materializacao.py:1102` | Recusa materializadora; colisão usa identidade em `:1099`; só em mudança anexa ocorrência em `:1107`. Não receber ESPERA no novo desenho. |
| `recusar_pergunta` | `app/chamados/services/validacao.py:584` | Recusa humana; reabre por `reabrir_para_correcao(... ato='recusa')` em `:593`. Intervalo deve migrar para ato conjunto; fora do escopo permanece. |
| `responder_pergunta_disputa_core` | `app/chamados/services/disputa_emissao.py:1494` | Escape `FOLGA_CONTESTA`/`OUTRO_MOTIVO`, não espera; decisão humana exigida em `:1483`. |
| `retratar_duplicata` | `app/chamados/services/fio.py:1350` | Descrição da identidade duplicada; delega gravação ao modelo. Não converter em estado do par. |
| `carimbar_identidade` — porta física compartilhada | `app/chamados/models.py:3188` | Grava motivo opcional no mesmo save do carimbo; preservar contratos de identidade. |

| Chamador runtime de `_carimbar_recusa`, inclusive apelido/import | Causa |
|---|---|
| `app/chamados/services/materializacao.py:836` | Sem resposta textual humana. |
| `app/chamados/services/materializacao.py:913` | Guarda individual de paridade sem marco aceso. |
| `app/chamados/services/materializacao.py:1213` | Data em disputa. |
| `app/chamados/services/materializacao.py:1297` | Erro da unidade individual, no tratamento final. |
| `app/chamados/services/passivo_validado.py:545` | Reprovação da geometria do grupo diário; import resolvido em `app/chamados/services/passivo_validado.py:506`. |

| Leitor atual / limite real | Referências MAIN | O que passa a ler — **proposta, FALTA implementação** |
|---|---|---|
| Fio: abertas carregam todas; fechadas filtram C6. Injeta estado de aplicação por pergunta. | `app/chamados/services/fio.py:221`; `:238`; `:335`. | Juiz derivado do par para visibilidade/ação de intervalo; C6 continua autoridade individual. |
| Detalhe filtra apenas C6 com apelido `_viva`. | `app/chamados/services/acoes_chamado.py:347`; `:348`. | Resultado do juiz do par conserva obrigações conjuntas ainda não aplicadas. |
| Explicador: estado individual; pendência precede lastro e afirma “NAO plantou”. Fechamento recomposto por `not _fio_vivo(ps)`. | `app/chamados/services/explicador.py:72`; `:85`; `:93`; `:136`; `:137`. | Estado e dono derivados do par; existência de batida pelo juiz do lastro; fechamento por C7 inteiro. |
| Modal lê diretamente pendência e motivo. | `app/templates/chamados/partials/modal_fio.html:221`; `:225`. | Projeção do juiz para espera/ações; motivo técnico continua detalhe, sem decidir espera. |
| Cartão usa pendência para dizer conferir “antes de lançar” e motivo como fallback. | `app/templates/chamados/partials/_card_atendimento.html:183`; `:186`. | Projeção derivada separa espera, dívida real, aplicação comprovada e conferência técnica. |
| Declaração: resposta validada impede nova declaração; projeção `aceita` representa capacidade de declarar aquele marco. | `app/chamados/services/declaracao_texto.py:210`; `:221`; `:279`; `:372`; `:471`; `:662`. | Capacidade de responder e capacidade de operar o par em propriedades distintas; não redefinir `aceita` como validação/aplicação. |
| I6 | `app/chamados/management/commands/auditar_invariantes_chamados.py:535`. | Continua sua pergunta de recusa muda; juiz do par somente quando precisar reconhecer espera derivada. |
| I18: validada humana, sem materialização, sem motivo. | `app/chamados/management/commands/auditar_invariantes_chamados.py:1074`. | Subclassificar obrigação do par pelo juiz, preservando dívida comprovada; não esconder validada sem aplicação. |
| I19: pendência administrativa sem motivo. | `app/chamados/management/commands/auditar_invariantes_chamados.py:1111`; `:1112`. | **Permanece técnico**, sem introduzir espera ou nova cópia de sua consulta. |
| Vigia carimbo: universo validado; primeiro lastro, depois slot e plantio. | `app/chamados/services/vigia_carimbo.py:97`; `:106`; `:110`; `:113`. | Juiz derivado classifica espera/obrigação do par sem retirar dívida aplicada falsamente do alarme. Universo de respostas não validadas ainda não entra nesse vigia. |
| Passivo lê classificação do vigia, não rederiva lastro. Guarda foto dos campos para reversão. | `app/chamados/services/passivo_validado.py:64`; `:324`; `:329`. | Unidade inteira dentro do universo autorizado; espera só por leitura do juiz. |
| Vigia fio consome classes da porta e C6. | `app/chamados/services/vigia_fio.py:148`; `:152`; `:165`. | Classes do juiz/porta, com partição exaustiva; nenhuma fórmula local de espera. |
| `flip_auto` lê texto de recusa de paridade, **não espera**. | `app/ponto/services/flip_auto.py:128`; `:132`. | Conservar fila real de paridade; juiz do par deve impedir que espera entre como candidata a flip. Não transformar texto de espera em alvo de correção. |
| `o_que_segura` não lê pendência/motivo diretamente; lê exportação, dias retidos e porta do dia. Esta é alimentada por C6. | `app/folha/fora_da_folha.py:215`; `:217`; `:221`; `:225`; `:65`; `app/chamados/juizes.py:1322`. | Alterar alimentação da autoridade da porta para incluir obrigação do par; `o_que_segura` continua consumidor dessa autoridade. |
| Reconciliador já pergunta C7. | `app/chamados/reconciliador.py:411`; `:611`; `:1170`. | C7 ampliado consome juiz derivado do par. |
| Becos já pergunta C7. | `app/chamados/services/becos.py:33`; `:56`. | Mesmo C7 ampliado. |
| Crons vigias carimbo/fio, sem apply. | `app/config/crons.py:2213`; `:2226`; `app/chamados/management/commands/vigiar_carimbo_sem_registro.py:34`. | Consomem classificadores migrados; não gravam espera. |
| Cron de becos é `--alarme`, porta eventual continua em `becos`. | `app/config/crons.py:663`; `app/chamados/management/commands/fechar_becos_disputa.py:39`; `:57`. | C7 único, sem regra de par no command. |
| O330 não é cron: command expressamente fora da rotina. | `app/config/crons.py:825`; `:830`; `app/chamados/management/commands/materializar_passivo_validado.py:68`. | Ato explícito que opera par inteiro ou recusa universo incompleto. |

Leitores adicionais de motivo: devolução lê motivo para tirar validação e registrar ato (`app/chamados/services/validacao.py:188`, `:192`, `:205`, `:225`); foto/auditoria lê em `app/chamados/services/validacao.py:147`, `:582`, `:604`. Devem receber o desfecho do ato conjunto, mantendo motivo técnico/humano como evidência; espera não pode acionar devolução. **FALTA implementação**.

Catraca sugerida: resolver aliases e rastrear usos dos seis campos para impedir **dedução de espera** fora do juiz; também impedir gravação de código/texto de espera por `Assign`, `setattr`, `update`, `bulk_update`, `save`, `carimbar_identidade` e `_carimbar_recusa`. Não proibir globalmente leituras de motivo/pendência: I19, auditoria e flip têm perguntas legítimas próprias nas referências acima. **[nao verificado: selo ainda inexistente]**.

## 09/11 14:39 · sessao a96c313f8c14 · SELOS-CANON-I3 · verificador · origem=Code

**REPROVADO.** Permanecem carimbo duplicável sob flock, abertura Agent sem fotografia e divergência entre B3 e placar. (`bin/disjuntor.py:939`, `bin/disjuntor.py:967`, `bin/disjuntor.py:2187`, `bin/disjuntor.py:2608`)

Nas tabelas, **PROCEDENTE** significa defeito remanescente; **NAO PROCEDE**, defeito indicado corrigido no recorte citado. Execução dos RED/GREEN, totais de mutantes mortos, condição dos hooks instalados e ocorrências em produção: **[nao verificado]**.

**1. A1…A11**

| Item | Veredito | Evidência e contraexemplo concreto |
|---|---|---|
| **A1 — identidade e persistência** | **PROCEDENTE: classe incompleta.** | A colisão original foi corrigida: UUID nasce uma vez, segue ao censo e ao `ini`, e o casamento exige obra/pid/hash/tent. Entretanto, `CODEX_LOGD=$R/logs/alternativo` é permitido: o `ini` vai para lá, enquanto a fotografia procura `logs/codex/chamadas.log` da raiz viva. A chamada pode executar e terminar SEM-FOTO. Além disso, falha do `printf` não impede `INI=1` nem a execução do Codex. (`bin/codex_pergunta.sh:92`, `bin/codex_pergunta.sh:200`, `bin/codex_pergunta.sh:240`, `bin/codex_pergunta.sh:300`, `bin/codex_pergunta.sh:408`, `bin/codex_limite_estado.py:53`, `bin/disjuntor.py:1026`) |
| **A1 — retry, rc 75, sonda e vaga** | **NAO PROCEDE vaga sem UUID na variável.** | Geração e validação precedem a aquisição da vaga. Pausa retorna 75 antes dela; sonda é recusada no construtor; limite retorna 75 depois da chamada. Os dois sítios de execução não introduzem retry interno. Isso não garante a persistência do `ini`, conforme acima. (`bin/codex_pergunta.sh:92`, `bin/codex_pergunta.sh:94`, `bin/codex_pergunta.sh:193`, `bin/codex_pergunta.sh:197`, `bin/codex_pergunta.sh:246`, `bin/codex_pergunta.sh:408`, `bin/codex_pergunta.sh:415`, `bin/codex_pergunta.sh:449`) |
| **A2 — desempate** | **NAO PROCEDE.** | Para duas fotos no mesmo segundo, N=5 seguida de N=3, a posição da linha decide; N não participa da ordenação. A consulta escolhe a última tupla. (`bin/disjuntor.py:1010`, `bin/disjuntor.py:1018`, `bin/disjuntor.py:1031`, `bin/disjuntor.py:1039`) |
| **A3 — carimbo concorrente** | **PROCEDENTE.** | P1 captura borda 10:00:00 e atrasa; P2 captura 10:00:01, obtém a trava primeiro e grava 10:00:01. P1 obtém depois, mas a releitura descarta esse carimbo por `t > agora`; grava outro, 10:00:00. Dois carimbos nascem mesmo com exclusão correta. (`bin/disjuntor.py:939`, `bin/disjuntor.py:961`, `bin/disjuntor.py:967`, `bin/disjuntor.py:969`) |
| **A4 — obra canônica** | **PROCEDENTE quanto à unificação; empréstimo de foto corrigido.** | B3 preserva `O9 (C)`, mas porta e consulta recusam essa identidade. `_obra_canonica` impede usar a fotografia de `O9`; não oferece uma representação comum para abrir/fotografar/consultar o final qualificado. O teste de cauda certifica a negativa, não a compatibilidade entre as pontas. (`bin/disjuntor.py:659`, `bin/disjuntor.py:923`, `bin/disjuntor.py:1127`, `bin/disjuntor.py:2100`, `bin/disjuntor.py:2801`, `bin/tests/test_etapas_no_brief.sh:381`) |
| **A5 — evento/memo/placar** | **PROCEDENTE: nova divergência.** | A escrita estrita agora precede o memo. Porém: grava PROVADAS; o `os.replace` do estado falha; a obra reabre com N=5; a próxima chamada reconfere o mesmo `aid` e grava METADE. B3 incorpora METADE, enquanto o placar conserva o primeiro PROVADAS. Não há recuperação da decisão já publicada. (`bin/disjuntor.py:404`, `bin/disjuntor.py:1123`, `bin/disjuntor.py:1128`, `bin/disjuntor.py:1158`, `bin/disjuntor.py:1164`, `bin/disjuntor.py:1320`, `bin/disjuntor.py:2608`) |
| **A6 — DESLIGADO preserva sujeição** | **PROCEDENTE: caminho de falha permanece aberto.** | Com chave desligada e carimbo ocupado, Agent é liberado, registra apenas FALHA e pula DESLIGADO. Retirada a obra da fila antes do B3, essa FALHA não prova sujeição; o final recebe FORA. (`bin/disjuntor.py:907`, `bin/disjuntor.py:1103`, `bin/disjuntor.py:1130`, `bin/disjuntor.py:2313`, `bin/disjuntor.py:2315`, `bin/disjuntor.py:2334`) |
| **A7 — carimbo antes de FORA** | **NAO PROCEDE a diferença original.** | Agent e wrapper agora carimbam antes do pertencimento. Compartilham, entretanto, o defeito de unicidade de A3. (`bin/disjuntor.py:2094`, `bin/disjuntor.py:2097`, `bin/disjuntor.py:2754`, `bin/disjuntor.py:2775`) |
| **A8 — relógio exclusivamente na borda** | **PROCEDENTE.** | Fotos recebem `agora`, mas negativas novas de ETAPAS passam por `trilha()`, que omite o instante e provoca `datetime.now()` interno. Essas negativas são depois consumidas para decidir sujeição histórica. Portanto não é apenas escrita diagnóstica sem efeito. (`bin/disjuntor.py:219`, `bin/disjuntor.py:233`, `bin/disjuntor.py:908`, `bin/disjuntor.py:1104`, `bin/disjuntor.py:2338`, `bin/disjuntor.py:2785`) |
| **A9 — pré-vaga isentando** | **NAO PROCEDE o defeito original.** | SNAPSHOT/APROVADO do wrapper sem `ini` deixou de provar primeira abertura: a trilha aceita apenas Agent nesse reconhecedor; wrapper depende do `ini` construtor. `_primeira_abertura` não exige casamento nem `tent`, porém; o efeito legado está na decisão 1 abaixo. (`bin/disjuntor.py:904`, `bin/disjuntor.py:1086`, `bin/disjuntor.py:1087`) |
| **A10 — cobertura da classe** | **PROCEDENTE: incompleta.** | O positivo Agent→foto→B3→consulta existe. Mas o teste de exceção só exige stdout vazio: `_censo_na_excecao` descarta `oks=[]`, e a chamada conforme é liberada sem publicar SNAPSHOT/APROVADO. O teste concorrente também aceita uma linha mesmo que outros participantes falhem: não verifica seus rc individualmente. (`bin/tests/test_etapas_no_brief.sh:470`, `bin/tests/test_etapas_no_brief.sh:476`, `bin/tests/test_etapas_no_brief.sh:532`, `bin/tests/test_etapas_no_brief.sh:535`, `bin/tests/test_etapas_no_brief.sh:546`, `bin/disjuntor.py:2187`, `bin/disjuntor.py:2365`) |
| **A11 — reancoragem** | **NAO PROCEDE.** | O mutante está ancorado no pertencimento do wrapper, em `if dentro is False:`. A fixture contrapõe célula qualificada a pertencimento pela célula crua e inclui o positivo com brief conforme. Morte executada: **[nao verificado]**. (`bin/tests/test_censo_antes_da_vaga.sh:755`, `bin/disjuntor.py:2779`, `bin/tests/test_censo_antes_da_vaga.sh:537`, `bin/tests/test_censo_antes_da_vaga.sh:543`) |

Há ainda dois contraexemplos de fotografia fora dos casos cobertos:

| Item | Veredito | Contraexemplo |
|---|---|---|
| **Identidade cortada na trilha** | **PROCEDENTE.** | Obra `OBRA-` seguida de 205 letras `A` passa a validação, mas o detalhe SNAPSHOT ultrapassa 300 caracteres e o corte alcança o UUID. A porta retorna sucesso; o parser não reconhece a fotografia truncada. (`bin/disjuntor.py:183`, `bin/disjuntor.py:225`, `bin/disjuntor.py:896`, `bin/disjuntor.py:898`, `bin/disjuntor.py:923`, `bin/disjuntor.py:2128`, `bin/disjuntor.py:2133`) |
| **N e hash de conteúdos diferentes** | **PROCEDENTE.** | CENSO, ETAPAS e SHA abrem o brief separadamente. Uma edição entre a leitura de N=3 e o hash do conteúdo N=5 permite persistir `sha(N5):N3`. Ocorrência real: **[nao verificado]**. (`bin/disjuntor.py:1888`, `bin/disjuntor.py:1945`, `bin/disjuntor.py:2116`, `bin/disjuntor.py:2119`, `bin/disjuntor.py:2120`, `bin/disjuntor.py:1186`, `bin/disjuntor.py:2128`) |

**2. As cinco decisões abertas**

| Decisão | Veredito | Efeito concreto |
|---|---|---|
| **1 — legado sem tent** | **PROCEDENTE: não se limita a inflação por 24 h.** | Exemplo: carimbo I3 às 12h; abertura legítima pela v2 às 13h, com fotografia e `ini` sem tent; atualização para v3 às 14h. A fotografia antiga desaparece do parser; o `ini` posterior ao carimbo impede isenção; um final completo recebe SEM-FOTO e reprovação. A janela de 24 h pertence somente ao contador, não ao B3. (`bin/disjuntor.py:896`, `bin/disjuntor.py:898`, `bin/disjuntor.py:990`, `bin/disjuntor.py:1086`, `bin/disjuntor.py:1135`, `bin/disjuntor.py:1143`, `bin/disjuntor.py:2565`) |
| **1 — isenção herdada** | **PROCEDENTE quanto à duração ilimitada.** | Um `ini` anterior ao carimbo continua sendo o mínimo histórico e permite SEM-SNAPSHOT em finais futuros sem foto, inclusive após reabertura DESLIGADA. Não expira após 24 h. Se uma isenção vitalícia por identidade da obra foi autorizada: **[nao verificado]**. (`bin/disjuntor.py:1086`, `bin/disjuntor.py:1089`, `bin/disjuntor.py:1135`, `bin/disjuntor.py:1138`) |
| **2 — DESLIGADO implica sujeita** | **PROCEDENTE quanto ao rótulo; não demonstra isenção indevida por si.** | A regra adota sujeição mesmo sem comprovar fila 1, mas a mensagem e o contador apresentam SEM-FOTO como “obra da fila 1”. Uma obra externa aberta DESLIGADA pode receber esse rótulo. Sujeição conservadora e pertencimento comprovado são fatos diferentes. (`bin/disjuntor.py:907`, `bin/disjuntor.py:1104`, `bin/disjuntor.py:1140`, `bin/disjuntor.py:2640`) |
| **3 — carimbo falho com chave desligada** | **PROCEDENTE.** | Wrapper recusa; Agent passa sem DESLIGADO. Além da assimetria, perde-se a evidência histórica necessária a A6, permitindo FORA após retirada da fila. (`bin/disjuntor.py:2754`, `bin/disjuntor.py:2756`, `bin/disjuntor.py:2789`, `bin/disjuntor.py:2313`, `bin/disjuntor.py:2315`, `bin/disjuntor.py:1130`) |
| **4 — relógios declarados antigos** | **PROCEDENTE: continuam alcançáveis pelo I3.** | `trilha()` continua lendo relógio internamente e recebe negativas novas. `--b3-parada` também alcança `decide_b3` sem instante, acionando seu fallback. Ser herdado não remove esses caminhos da classe examinada. (`bin/disjuntor.py:233`, `bin/disjuntor.py:1298`, `bin/disjuntor.py:1421`, `bin/disjuntor.py:1474`, `bin/disjuntor.py:2785`, `bin/disjuntor.py:2855`; `CLAUDE.md:371`) |
| **5 — flock no próprio disjuntor.log** | **NAO PROCEDE bloqueio da leitura direta; há espera indireta.** | O trecho citado do hook faz `stat`/comparação de inode, sem flock nesse arquivo. Porém Stop→B3→conferência pode disputar a trava do carimbo por 20 tentativas de 50 ms e terminar NAO SEI. (`bin/hook_stop_fila1.py:666`, `bin/hook_stop_fila1.py:679`, `bin/hook_stop_fila1.py:1204`, `bin/disjuntor.py:1126`, `bin/disjuntor.py:911`, `bin/disjuntor.py:964`) |

Logo, **B3 pode reprovar incorretamente na transição v2→v3**. Também pode herdar indefinidamente uma isenção antiga; qualificá-la como incorreta depende do aval sobre duração dessa isenção, que está **[nao verificado]**. (`bin/disjuntor.py:898`, `bin/disjuntor.py:1089`, `bin/disjuntor.py:1135`, `bin/disjuntor.py:1143`)

**3. Classes já reprovadoras, selos e semântica**

| Classe/item | Veredito | Evidência e contraexemplo |
|---|---|---|
| **Segundo juiz duplicando registrado** | **NAO PROCEDE no N auditado.** | B3 e consulta usam `_fotografia_efetiva`. `n_v` confirma o verificador; `n_rel` reconhece medidas no RELATO; o cruzamento pelo mínimo é explícito. Não demonstrei duplicação de autoridade registrada. (`bin/disjuntor.py:1128`, `bin/disjuntor.py:1146`, `bin/disjuntor.py:1149`, `bin/disjuntor.py:2805`) |
| **Segundo escritor fora da porta** | **NAO PROCEDE nos sítios auditados.** | Append converge em `trilha_linhas`; estado converge em `_com_estado`; `ini/fim` convergem no wrapper. Os defeitos encontrados são de persistência, recuperação e correspondência, apesar dessa centralização. (`bin/disjuntor.py:225`, `bin/disjuntor.py:233`, `bin/disjuntor.py:404`, `bin/codex_pergunta.sh:266`, `bin/codex_pergunta.sh:299`) |
| **Derivação paralela / testemunha divergente** | **PROCEDENTE em A5.** | Após evento persistido e estado perdido, B3 usa a reconferência, mas ESTADO usa o primeiro evento. Exemplo: B3 METADE com faltam=5; placar PROVADAS 1/1. Viola a mesma autoridade e contador correspondente ao universo. (`bin/disjuntor.py:1158`, `bin/disjuntor.py:1320`, `bin/disjuntor.py:2608`; `CLAUDE.md:20`, `CLAUDE.md:35`) |
| **Deduplicação antes do teto temporal** | **PROCEDENTE.** | Primeira linha futura PROVADAS com `aid=x`, seguida de linha presente METADE com o mesmo aid: a futura marca `vistos`, depois é excluída; a presente também é excluída. Resultado: nenhum final contado. (`bin/disjuntor.py:2608`, `bin/disjuntor.py:2610`, `bin/disjuntor.py:2611`) |
| **Ordem de trava** | **NAO PROCEDE inversão demonstrada.** | B3 faz estado→carimbo. O carimbo libera seu `with` antes de retornar e não adquire estado dentro dele. Há contenção e falha possíveis; não encontrei ciclo reverso nesses caminhos. (`bin/disjuntor.py:398`, `bin/disjuntor.py:1320`, `bin/disjuntor.py:958`, `bin/disjuntor.py:974`) |
| **Mutante `abertura-sem-hash`** | **PROCEDENTE: causa diferente da anunciada.** | Troca somente o hash do lado `ini` por vazio, mantendo igualdade integral. Assim perde casamentos válidos A/K/L; não permite o casamento ilegal de J. Na fixture, “sem” passa de 7 para 9 por falsos negativos. (`bin/tests/test_censo_antes_da_vaga.sh:726`, `bin/disjuntor.py:989`, `bin/disjuntor.py:990`, `bin/tests/test_censo_antes_da_vaga.sh:474`, `bin/tests/test_censo_antes_da_vaga.sh:483`, `bin/tests/test_censo_antes_da_vaga.sh:488`) |
| **Mutante `obra-fora-da-forma-passa`** | **PROCEDENTE: não produz a abertura anunciada.** | Remover `nc == 'forma'` deixa o `if nc` seguinte negando por ETAPAS. A fixture cai porque mudou a mensagem “fora da forma do censo”, enquanto a abertura continua negada. (`bin/tests/test_censo_antes_da_vaga.sh:747`, `bin/disjuntor.py:2104`, `bin/tests/test_censo_antes_da_vaga.sh:304`, `bin/tests/test_censo_antes_da_vaga.sh:306`) |
| **Mutante `t2-carimbo-a-cada-final`** | **PROCEDENTE: causa contaminada.** | Faz o leitor ignorar todos os carimbos, inclusive na leitura de volta. Após append, ocorre `RuntimeError`; não isola somente repetição de carimbo em finais normalmente conferidos. (`bin/tests/test_etapas_no_brief.sh:641`, `bin/disjuntor.py:971`, `bin/disjuntor.py:973`) |
| **Guarda exit 2 de `#G:tentativa`** | **PROCEDENTE: sem mordida própria.** | Os mutantes alteram o `ini` ou geram UUID fixo válido. Nenhum remove a recusa diante de UUID vazio/inválido. O catálogo cobra a tag, não cada guarda associada a ela. (`bin/codex_pergunta.sh:93`, `bin/codex_pergunta.sh:94`, `bin/tests/test_codex_pergunta.sh:560`, `bin/tests/test_codex_pergunta.sh:561`, `bin/tests/test_codex_pergunta.sh:563`) |
| **Harness “mensagem específica”** | **PROCEDENTE quanto à declaração generalizada.** | ETAPAS exige rc≠0 e nome do caso; censo vizinho aceita qualquer rc≠0; wrapper procura qualquer FALHA emitida pelo caso. Nem mesmo a mensagem específica garante a causa interna correta. (`bin/tests/test_etapas_no_brief.sh:588`, `bin/tests/test_censo_antes_da_vaga.sh:687`, `bin/tests/test_codex_pergunta.sh:579`, `bin/tests/test_codex_pergunta.sh:580`) |
| **Pouso antes do pré-requisito** | **PROCEDENTE como caminho ainda existente.** | Bash principal sem `agent_id`, sem Agent e sem `codex_pergunta` retorna antes do B3. A obra acrescenta conferência a esse B3, mas não torna universal sua execução antes de comandos de pouso. (`bin/disjuntor.py:2244`, `bin/disjuntor.py:2272`) |

| Campo/conceito | Semântica efetiva e problema |
|---|---|
| **`tent` / “tentativa”** | UUID identifica invocação; o contador chama “tentativa” ao `ini` construtor e ainda soma Agent DESLIGADO. O `fim` continua ligado por obra/hash/pid, sem UUID: duas invocações com PID reciclado podem compartilhar o resultado exibido por `ultimas_chamadas`. (`bin/codex_pergunta.sh:92`, `bin/codex_pergunta.sh:266`, `bin/codex_limite_estado.py:64`, `bin/codex_limite_estado.py:89`, `bin/disjuntor.py:2653`) |
| **Obra canônica** | Porta/consulta exigem ID sem cauda; B3 preserva cauda significativa. Há recusa segura de empréstimo, mas não identidade universal compatível. (`bin/disjuntor.py:659`, `bin/disjuntor.py:923`, `bin/disjuntor.py:1128`, `bin/disjuntor.py:2801`) |
| **Instante** | Mistura instante da borda com instante interno de publicação; ambos são persistidos com precisão de segundos. A igualdade no mesmo segundo também impede provar `primeira_abertura < instalação`, mesmo que a ordem real tenha sido essa. (`bin/disjuntor.py:219`, `bin/disjuntor.py:220`, `bin/disjuntor.py:1135`) |
| **Sujeita / DESLIGADO** | Sujeita significa fila viva, pertencimento indeterminado ou histórico de julgamento/DESLIGADO. Não significa necessariamente fila 1 comprovada, embora SEM-FOTO seja apresentado assim. (`bin/disjuntor.py:1097`, `bin/disjuntor.py:1099`, `bin/disjuntor.py:1104`, `bin/disjuntor.py:2640`) |
| **“Pousos”** | Mede finais APROVADOS conferidos pelo B3, não merges/deploys realizados; o próprio texto explicita esse recorte. (`bin/disjuntor.py:2616`, `bin/disjuntor.py:2637`) |

**4. Escritores e leitores**

`disjuntor.log` e a trilha ETAPAS/CENSO são o mesmo arquivo físico; fotografia é um registro dentro dele. (`bin/disjuntor.py:142`, `bin/disjuntor.py:2128`, `bin/disjuntor.py:2133`)

| Arquivo/campo | Quem escreve | Quem lê |
|---|---|---|
| **`chamadas.log` — hora, obra, papel, hash12, pid, ini, pergunta, tent** | Wrapper: `bin/codex_pergunta.sh:299`, `bin/codex_pergunta.sh:300` | Parser: `bin/codex_limite_estado.py:56`, `bin/codex_limite_estado.py:64`, `bin/codex_limite_estado.py:66`; fotografia: `bin/disjuntor.py:1026`; primeira abertura: `bin/disjuntor.py:1086`; censo: `bin/disjuntor.py:2561` |
| **`chamadas.log` — fim, sessão, rc, duração** | `bin/codex_pergunta.sh:266`, `bin/codex_pergunta.sh:267` | `bin/codex_limite_estado.py:68`; últimas chamadas: `bin/codex_limite_estado.py:89`; origem Code: `bin/codex_limite_estado.py:97`, `bin/codex_limite_estado.py:98`; publicação: `bin/gerar_codex_estado.py:176` |
| **Trilha — instante/evento/detalhe** | Append: `bin/disjuntor.py:225`; adaptador: `bin/disjuntor.py:233` | Leitores específicos abaixo; hook também compara inode para excluir prova de espera: `bin/hook_stop_fila1.py:666`, `bin/hook_stop_fila1.py:679` |
| **INSTALADO** | `bin/disjuntor.py:969` | `bin/disjuntor.py:936`, `bin/disjuntor.py:939`; usado na isenção: `bin/disjuntor.py:1135` |
| **SNAPSHOT — obra/N/via/pid/hash/tent** | Montagem: `bin/disjuntor.py:2128`; CLI: `bin/disjuntor.py:2133`; Agent: `bin/disjuntor.py:2137`, `bin/disjuntor.py:2232` | Parser/ingestão: `bin/disjuntor.py:898`, `bin/disjuntor.py:1011`; casamento: `bin/disjuntor.py:1026`; N efetivo: `bin/disjuntor.py:1038`; B3: `bin/disjuntor.py:1128`; consulta: `bin/disjuntor.py:2805` |
| **SNAPSHOT — briefs, sha12 e N por brief** | SHA: `bin/disjuntor.py:1186`; associação: `bin/disjuntor.py:2124`; serialização: `bin/disjuntor.py:2129` | O leitor da fotografia consome o N agregado e a via; não reconfere a lista de hashes dos briefs: `bin/disjuntor.py:1014`, `bin/disjuntor.py:1018` |
| **CENSO APROVADO/FORA/DESLIGADO** | `bin/disjuntor.py:2130`, `bin/disjuntor.py:2133`, `bin/disjuntor.py:2316`, `bin/disjuntor.py:2759`, `bin/disjuntor.py:2780` | Abertura Agent: `bin/disjuntor.py:904`, `bin/disjuntor.py:1079`; sujeição: `bin/disjuntor.py:1103`; medidor: `bin/disjuntor.py:2543`, `bin/disjuntor.py:2565` |
| **NEGOU/FALHA/ALVO** | `bin/disjuntor.py:309`, `bin/disjuntor.py:2313`, `bin/disjuntor.py:2338`, `bin/disjuntor.py:2785` | Negativa para sujeição: `bin/disjuntor.py:908`; censo: `bin/disjuntor.py:2537`; ETAPAS: `bin/disjuntor.py:2580`, `bin/disjuntor.py:2597` |
| **ETAPAS finais — obra/N/verificador/relato/faltam/aid** | Montagem: `bin/disjuntor.py:1131`, `bin/disjuntor.py:1136`, `bin/disjuntor.py:1140`, `bin/disjuntor.py:1152`; publicação: `bin/disjuntor.py:1158` | Placar: `bin/disjuntor.py:2597`; deduplicação por aid: `bin/disjuntor.py:2606`; apresentação: `bin/disjuntor.py:2630` |
| **`disjuntor_estado.json` — memo/reprovações/metade** | Memo: `bin/disjuntor.py:1164`; incorporação B3: `bin/disjuntor.py:1326`, `bin/disjuntor.py:1328`; persistência: `bin/disjuntor.py:403`, `bin/disjuntor.py:404` | Estado: `bin/disjuntor.py:424`; memo: `bin/disjuntor.py:1123`; decisão: `bin/disjuntor.py:1337` |
| **Brief — CENSO/ETAPAS/K** | Autor real: **[nao verificado]**; fixture: `bin/tests/test_etapas_no_brief.sh:64` | Coletor: `bin/disjuntor.py:1998`; validadores: `bin/disjuntor.py:2116`, `bin/disjuntor.py:2119`; SHA: `bin/disjuntor.py:2120` |
| **BACKLOG — fila/

[truncado em 20000 de 21190 caracteres]

## 09/11 14:39 · sessao 5572ed6b010b · - · - · origem=Ronald (tmux)

| Tema | Achado no MAIN | Consequência para matriz |
|---|---|---|
| Estados existentes | `estado` distingue pendente/respondida/validada/resolvida; não distingue recusada/retratada/aplicação real. `app/chamados/models.py:3034` | Os seis estados pedidos exigem projeção derivada; juiz desse conjunto: **FALTA**. |
| Sem resposta | Campos `respondida_em`, `resposta_colab`. `app/chamados/models.py:2455` | Exigir ambos coerentes; reabertura limpa os dois. `app/chamados/services/disputa_emissao.py:1590` |
| Respondida | Resposta não vazia e instante; escritor grava juntos. `app/chamados/services/disputa_emissao.py:1464` | Não inferir aceite pela resposta. `app/chamados/services/validacao.py:435` |
| Validada | `validada_em/por`, com linha `VALIDACAO`. `app/chamados/services/validacao.py:127` | Hoje mata C6 mesmo sem aplicação. `app/chamados/juizes.py:1153` |
| Aplicada | `materializada_em/por/via`, carimbo pela porta. `app/chamados/models.py:3103` | Carimbo isolado não prova batida; `_pergunta_materializada_de_fato` exige testemunha, mas também absolve identidade. `app/chamados/juizes.py:645`, `app/chamados/juizes.py:670` |
| Recusada | Recusa humana escreve motivo e reabre, apagando resposta/validação. `app/chamados/services/validacao.py:584`, `app/chamados/services/validacao.py:593` | Hoje a property mostra pendente; distinguir pela sequência de atos `RECUSA` e respostas posteriores. `app/chamados/models.py:3045`, `app/chamados/models.py:3857` |
| Retratada | Via `motivo_retratado`; `fora_do_fato` inclui também duplicata. `app/chamados/models.py:2401`, `app/chamados/models.py:3201` | Não confundir ausência do fato com aplicação do intervalo: identidade também grava `materializada_em`. `app/chamados/models.py:3185` |
| Histórico aplicado→retratado | Remoção preserva carimbo e acrescenta `RETRATACAO`. `app/chamados/models.py:2064` | A/T são rótulos atuais exclusivos por precedência; histórico pode conter ambos. `app/chamados/models.py:2069` |
| Terminalidade | `carimbar`, `carimbar_identidade` e `descarimbar` impedem retorno de fora do fato. `app/chamados/models.py:3120`, `app/chamados/models.py:3180`, `app/chamados/models.py:3206` | Corrigir não pode ressuscitar a pergunta retratada; se fato voltar, nasce pergunta nova. `app/chamados/models.py:2520` |

| Juiz/porta | MAIN | Limite |
|---|---|---|
| C6 | Validação mata no primeiro degrau. `app/chamados/juizes.py:1153` | Não responde se par foi aplicado. `app/chamados/juizes.py:1153` |
| C7 | `not respostas_sem_veredito() and not fio_vivo(...)`. `app/chamados/models.py:1861` | V/V e V/T podem liberar sem batida; ambas saem de `respostas_sem_veredito`. `app/chamados/models.py:1851`, `app/chamados/juizes.py:1164` |
| S127 | Usa `fio_vivo`, preservando disputa vazia. `app/chamados/juizes.py:2138` | Não consome C7 inteiro no MAIN. `app/chamados/juizes.py:2138` |
| Transição chamado | Consulta S127 para estados encerrados antes de `_aplicar`. `app/chamados/models.py:148`, `app/chamados/models.py:199` | Guarda adicional de FURO é exclusiva de `resolvido`. `app/chamados/models.py:181` |
| `Disputa.fechar` | Confere respostas e vivas antes do atomic; trava em seguida. `app/chamados/models.py:1894`, `app/chamados/models.py:1903`, `app/chamados/models.py:1918` | Reconsulta C7 sob trava: **FALTA**. |
| Travas | Ordem Colaborador→Disputa→Chamado→Pergunta pela porta comum. `app/chamados/models.py:2637` | Reutilizar para travar as duas metades antes da primeira escrita. **[nao verificado: desenho]** |
| Identidade de par | Motivos mapeiam S/hii e E/hfi. `app/chamados/juizes.py:885` | Identificador de par: **FALTA**; os motivos não carregam índice da pausa. `app/chamados/services/declaracao_texto.py:48` |
| DNA | `pausas_do_dia` lê DNA congelado e todos pares canônicos. `app/escala/models.py:1166`, `app/escala/models.py:1197` | Pode alimentar adaptador sem inventar identidade adicional. **[nao verificado: desenho]** |
| Índice artificial | Modelo exige `instancia_marco` derivada do motivo. `app/chamados/models.py:3436` | Inventar identificador de pausa nesse campo é recusado. `app/chamados/models.py:3441` |

**Não declarar combinações entre metades impossíveis no MAIN:** validação, recusa e retratação operam individualmente; não existe escritor do par que imponha igualdade dos estados. `app/chamados/services/validacao.py:435`, `app/chamados/services/validacao.py:593`, `app/chamados/models.py:2062`

| Combinações reduzidas | Leitura para o desenho novo |
|---|---|
| NN, NR, NC, NV | Aguardar respostas/correções; nenhuma validação parcial; zero escrita de espera. **[nao verificado: desenho]** |
| RR, RV, VV | Se identidade única e duas respostas válidas, oferecer validar/recusar/corrigir o par; VV é legado transitório, não novo repouso. **[nao verificado: desenho]** |
| RC, VC, CC | Par em correção; não aplicar enquanto faltar declaração; decisão conjunta. **[nao verificado: desenho]** |
| NA, RA, VA, CA | Aplicação parcial legada: reter fechamento; corrigir o conjunto preservando chão e trilha; não completar silenciosamente uma metade. **[nao verificado: desenho]** |
| AA | Idempotência: nenhuma nova batida/validação/trilha; fechamento condicionado às demais perguntas. **[nao verificado: desenho]** |
| NT, RT, CT | Identidade atual incompleta: nenhuma aplicação avulsa; recusa falante para recompor identidade. **[nao verificado: desenho]** |
| VT, AT | Reter aceite/aplicação pendente, independentemente de motivo/pendência técnica; corrigir sem reviver T. **[nao verificado: desenho]** |
| TT | Fora do fato; não confundir com intervalo aplicado; preservam-se decisões anteriores. `app/chamados/models.py:3201`, `app/chamados/models.py:3821` |
| AMBÍGUO / SEM PAR | Abstenção explícita, nenhuma escolha por primeiro PK; porta individual não pode servir de fallback. **[nao verificado: desenho]** |

A simetria acima cobre 21 classes das 36 posições; não torna saída e volta temporalmente intercambiáveis: os tipos são S/E e os marcos hii/hfi. `app/chamados/juizes.py:885`

| Esmeril semântico | Evidência |
|---|---|
| `validada_em` = aceite e morte no C6 | `app/chamados/services/validacao.py:134`, `app/chamados/juizes.py:1153` |
| `materializada_em` = aplicação, identidade, retratação, resolução sem declaração | `app/chamados/models.py:3103`, `app/chamados/models.py:3185` |
| `recusa_motivo` = recusa humana, escape, motivo de identidade, recusa técnica | `app/chamados/services/validacao.py:584`, `app/chamados/services/disputa_emissao.py:1494`, `app/chamados/models.py:3188` |
| `pendencia_admin` = descrição comenta “recusou plantar”, choice declara “plantada pelo motivo” | `app/chamados/models.py:2555`, `app/chamados/models.py:2562` |
| Resposta anterior não deve sumir na recusa/correção | Projeção limpa, foto vai à sequência imutável. `app/chamados/services/disputa_emissao.py:1590`, `app/chamados/services/disputa_emissao.py:1612` |

| Provas existentes aproveitáveis | Local |
|---|---|
| Estado validado não vivo | `app/chamados/tests/test_pergunta_estado.py:39` |
| Estado materializado sem resposta | `app/chamados/tests/test_pergunta_estado.py:50` |
| Resposta e instante escritos juntos por AST | `app/chamados/tests/test_contract_par_resposta.py:88` |
| S127 disputa vazia / viva / resolvida | `app/chamados/tests/test_disputa_s127.py:46`, `app/chamados/tests/test_disputa_s127.py:51`, `app/chamados/tests/test_disputa_s127.py:56` |
| C7 autoridade registrada | `app/chamados/tests/test_disputa_fim.py:22` |
| Decisões imutáveis | `app/chamados/models.py:3826` |
| Porta da sequência | `app/chamados/models.py:3942` |

Execução de testes, ocorrência populacional e dados de produção além do anexo: **[nao verificado]**. [nao verificado]

## 09/11 14:36 · sessao 10d859872129 · - · - · origem=Ronald (tmux)

| Item | Veredito e contraexemplo |
|---|---|
| A1 — identidade normal | **NAO PROCEDE** colisão original: UUID nasce uma vez, valida antes da vaga, segue ao censo e ao ini; casamento exige obra/pid/hash/tent. (`bin/codex_pergunta.sh:92`, `bin/codex_pergunta.sh:94`, `bin/codex_pergunta.sh:240`, `bin/codex_pergunta.sh:246`, `bin/codex_pergunta.sh:300`, `bin/disjuntor.py:989`, `bin/disjuntor.py:990`) |
| A1 — persistência na classe | **PROCEDENTE.** `CODEX_LOGD=$R/logs/alternativo` é permitido; o censo grava na raiz viva, mas o ini vai ao LOGD alternativo, enquanto B3 procura `logs/codex/chamadas.log` fixo. Chamada real com UUID nunca efetiva fotografia; pode virar SEM-FOTO. (`bin/codex_pergunta.sh:88`, `bin/codex_pergunta.sh:200`, `bin/codex_pergunta.sh:240`, `bin/codex_pergunta.sh:300`, `bin/codex_limite_estado.py:22`, `bin/codex_limite_estado.py:53`, `bin/disjuntor.py:1026`, `bin/disjuntor.py:1143`) |
| A1 — ini impossível de gravar | **PROCEDENTE.** `printf` falha e `INI=1` executa mesmo assim; não existe `set -e`. Ex.: chamadas.log regular sem permissão de escrita, demais saídas graváveis. O wrapper pode chamar Codex sem ini persistido. (`bin/codex_pergunta.sh:53`, `bin/codex_pergunta.sh:299`, `bin/codex_pergunta.sh:300`, `bin/codex_pergunta.sh:408`) |
| A1 — truncamento | **PROCEDENTE.** Obra de 210 caracteres (`OBRA-` + 205 letras A) passa a forma e cabe no nome da worktree; o detalhe SNAPSHOT ultrapassa 300 caracteres. O escritor corta o UUID, `_SNAP_RE` deixa de reconhecer a foto, mas a porta retorna sucesso. Demonstrado usando AST extraída em memória, sem arquivos. (`bin/disjuntor.py:183`, `bin/disjuntor.py:923`, `bin/disjuntor.py:925`, `bin/disjuntor.py:225`, `bin/disjuntor.py:899`, `bin/disjuntor.py:2128`, `bin/disjuntor.py:2133`, `bin/disjuntor.py:2138`; nome: `bin/codex_pergunta.sh:214`) |
| Retry/rc75/sonda | **NAO PROCEDE** vaga sem UUID na variável: geração/validação precedem todo ramo. Pausa sai antes da vaga; limite sai depois da chamada; sonda é recusada no construtor. Não há retry interno entre as duas chamadas únicas do Codex. Persistência tem as falhas acima. (`bin/codex_pergunta.sh:92`, `bin/codex_pergunta.sh:193`, `bin/codex_pergunta.sh:197`, `bin/codex_pergunta.sh:408`, `bin/codex_pergunta.sh:415`, `bin/codex_pergunta.sh:449`, `bin/codex_pergunta.sh:462`) |
| A2 | **NAO PROCEDE** desempate por N: índice da linha é preservado; ordenação usa `(t,i)` e consulta escolhe a última. (`bin/disjuntor.py:1010`, `bin/disjuntor.py:1018`, `bin/disjuntor.py:1020`, `bin/disjuntor.py:1031`, `bin/disjuntor.py:1039`) |
| A4 | **Cura segura parcial.** CLI exige inteiro; B3 não empresta foto da identidade nua. Porém B3 preserva `O9 (C)`, enquanto a porta/consulta recusam essa identidade: não existe representação comum que permita fotografar a obra qualificada. (`bin/disjuntor.py:659`, `bin/disjuntor.py:923`, `bin/disjuntor.py:1127`, `bin/disjuntor.py:1128`, `bin/disjuntor.py:2100`, `bin/disjuntor.py:2750`, `bin/disjuntor.py:2801`) |
| A5 — ordem original | **NAO PROCEDE** perda original evento após memo: escrita estrita precede memo. (`bin/disjuntor.py:1158`, `bin/disjuntor.py:1164`) |
| A5 — repetição incoerente | **PROCEDENTE.** Primeiro evento PROVADAS grava; `os.replace` do estado falha. RELATO perde uma medida antes da próxima chamada. Sem memo persistido, o mesmo aid vira METADE, incorporado pelo B3; contador mantém PROVADAS porque escolhe primeiro evento. (`bin/disjuntor.py:404`, `bin/disjuntor.py:411`, `bin/disjuntor.py:1123`, `bin/disjuntor.py:1146`, `bin/disjuntor.py:1158`, `bin/disjuntor.py:1320`, `bin/disjuntor.py:1328`, `bin/disjuntor.py:2608`) |
| A5 — futuro | **PROCEDENTE.** Linha futura PROVADAS aid=a1 seguida de linha presente METADE aid=a1 resulta em `finais=0`, `metade=[]`: dedup ocorre antes do filtro temporal. Demonstrado pela função real extraída por AST com StringIO. (`bin/disjuntor.py:2605`, `bin/disjuntor.py:2608`, `bin/disjuntor.py:2610`, `bin/disjuntor.py:2611`) |
| Fotografia de conteúdo | **PROCEDENTE.** CENSO, ETAPAS e SHA leem arquivo separadamente. Troca do brief entre N3 e hash do conteúdo N5 produz `sha(N5):N3`; não existe revalidação comum. Ocorrência real **[nao verificado]**. (`bin/disjuntor.py:1888`, `bin/disjuntor.py:1945`, `bin/disjuntor.py:2116`, `bin/disjuntor.py:2119`, `bin/disjuntor.py:2120`, `bin/disjuntor.py:1186`, `bin/disjuntor.py:2128`) |

Lei relevante: contador deve corresponder ao universo e leitura deve consumir a mesma autoridade; estado tem escritor canônico. (`CLAUDE.md:20`, `CLAUDE.md:32`, `CLAUDE.md:35`)

| Arquivo/campo | Escritor | Leitor |
|---|---|---|
| chamadas.log ini: hora/obra/papel/hash/pid/pergunta/tent | `bin/codex_pergunta.sh:299`, `bin/codex_pergunta.sh:300` | Parser `bin/codex_limite_estado.py:56`, `bin/codex_limite_estado.py:64`, `bin/codex_limite_estado.py:66`; foto `bin/disjuntor.py:1026`; primeira abertura `bin/disjuntor.py:1086`; censo `bin/disjuntor.py:2565` |
| chamadas.log fim: sessão/rc/duração | `bin/codex_pergunta.sh:266`, `bin/codex_pergunta.sh:267` | `bin/codex_limite_estado.py:68`; últimas chamadas ainda casam por obra/hash/pid, ignorando tent: `bin/codex_limite_estado.py:89`; origem Code: `bin/codex_limite_estado.py:97`, `bin/codex_limite_estado.py:98` |
| disjuntor.log instante/evento/detalhe | Único append `bin/disjuntor.py:225`; adaptador `bin/disjuntor.py:233` | Leitores específicos abaixo |
| SNAPSHOT obra/N/via/pid/hash/tent/briefs | Montagem `bin/disjuntor.py:2128`; CLI `bin/disjuntor.py:2133`; Agent `bin/disjuntor.py:2137`, `bin/disjuntor.py:2232` | Parser `bin/disjuntor.py:899`; ingestão `bin/disjuntor.py:1011`; casamento `bin/disjuntor.py:1026`; N único B3 `bin/disjuntor.py:1128`, CLI `bin/disjuntor.py:2805` |
| ETAPAS finais + aid | `bin/disjuntor.py:1152`, `bin/disjuntor.py:1158` | `bin/disjuntor.py:2597`, `bin/disjuntor.py:2606` |
| disjuntor_estado.json memo/filas | Memo `bin/disjuntor.py:1164`; B3 `bin/disjuntor.py:1326`, `bin/disjuntor.py:1328`; gravação única `bin/disjuntor.py:398`, `bin/disjuntor.py:404` | Memo `bin/disjuntor.py:1123`; estado `bin/disjuntor.py:424` |
| RELATO e RELATO-ARQUIVO medidas K | Autoria real **[nao verificado]** | Helper `bin/disjuntor.py:1042`; contador `bin/disjuntor.py:1055`; cruzamento B3 `bin/disjuntor.py:1146`, `bin/disjuntor.py:1149`; verificador humano/modelo `.claude/agents/verificador.md:18` |
| ETAPAS n/N do verificador | Instrução `.claude/agents/verificador.md:18`; execução **[nao verificado]** | `bin/disjuntor.py:1147`, `bin/disjuntor.py:1148` |
| ESTADO linha exibida | Composição `bin/disjuntor.py:2734`; publicação `bin/gerar_estado.py:281` | Gerador consome `ler/linha`: `bin/gerar_estado.py:280` |

Segundo escritor introduzido: **NAO PROCEDE** nos sítios auditados; append, estado e N convergem nas funções citadas. Duplicação de juiz **registrado** não demonstrada; N tem autoridade compartilhada, enquanto `n_v` e `n_rel` são duas evidências cruzadas explicitamente pelo mínimo. (`bin/disjuntor.py:225`, `bin/disjuntor.py:404`, `bin/disjuntor.py:1128`, `bin/disjuntor.py:2805`, `bin/disjuntor.py:1149`)

## 09/11 14:36 · sessao 87856602ee3a · - · - · origem=Ronald (tmux)

| Item | Conclusão | Evidência / contraexemplo |
|---|---|---|
| A10 — positivo Agent normal | **NAO PROCEDE a ausência original desse positivo.** | Abre Agent, exige PROVADAS pelo B3 e consulta `--etapas`: `bin/tests/test_etapas_no_brief.sh:529`, `:532`, `:535`; mutante remove ingestão Agent em `:698`. Execução/morte efetivas **[nao verificado]**. |
| A10 — classe de exceção | **PROCEDENTE.** | O teste injeta `RuntimeError` e exige somente stdout vazio: `bin/tests/test_etapas_no_brief.sh:540`, `:546`. Entretanto `_censo_na_excecao` passa `oks=[]` descartável, retorna `None`, e o hook libera: `bin/disjuntor.py:2187`, `:2190`, `:2365`. Uma abertura conforme na exceção passa sem SNAPSHOT/APROVADO; o teste fica verde sem provar a fotografia. |
| A10 — concorrência | **PROCEDENTE quanto à força da prova.** | O teste inicia oito processos, dorme 0,6 s e só conta linhas: `bin/tests/test_etapas_no_brief.sh:470`, `:473`, `:475`. Não há barreira confirmando que todos leram antes da soltura nem verificação dos oito rc. Um carimbo com outros processos falhando ainda satisfaz `:476`. |
| A11 | **NAO PROCEDE.** | Âncora de `bin/tests/test_censo_antes_da_vaga.sh:755` casa uma vez em `bin/disjuntor.py:2779`. Troca pertencimento canônico por célula crua. A fixture qualificada exige negativa e depois aprovação com brief conforme: `bin/tests/test_censo_antes_da_vaga.sh:537`, `:543`, `:545`. Morte executada **[nao verificado]**. |
| Guarda `#G:tentativa`, exit 2 | **PROCEDENTE: falta mordida própria.** | Guarda em `bin/codex_pergunta.sh:93`. Os mutantes alteram ini ou geram UUID fixo **válido**: `bin/tests/test_codex_pergunta.sh:560`, `:561`. Nenhum fornece vazio/UUID inválido removendo a recusa. Catálogo só cobra presença da tag, não cada guarda: `:563`. |
| Mutante `abertura-sem-hash` | **PROCEDENTE: morte por propriedade diferente.** | `bin/tests/test_censo_antes_da_vaga.sh:726` substitui o hash do **lado ini** por `''`; o confronto continua sendo igualdade integral (`bin/disjuntor.py:989`, `:990`). Portanto rejeita casamento válido, em vez de permitir hash divergente. A válido: fixture `:474`, `:481`; J divergente: `:476`, `:483`. O contador “sem” vira 9, em vez de 7, pelos matches válidos perdidos (`:488`), não pelo casamento ilegal de J. |
| Mutante `obra-fora-da-forma-passa` | **PROCEDENTE: não produz a abertura anunciada.** | Remove só o ramo `nc == 'forma'`: `bin/tests/test_censo_antes_da_vaga.sh:747`. Continua negando no `if nc`, agora por ETAPAS: `bin/disjuntor.py:2104`. Caso `O9 (C)` exige também texto “fora da forma do censo”: `bin/tests/test_censo_antes_da_vaga.sh:304`, `:306`. Morre pela mensagem diferente, enquanto a abertura continua negada. |
| Mutante `t2-carimbo-a-cada-final` | **PROCEDENTE: causa contaminada pela releitura.** | Mutação faz `_instalado_lido` ignorar todo carimbo (`bin/tests/test_etapas_no_brief.sh:641`). A releitura após append permanece sem resultado, causando `RuntimeError` em `bin/disjuntor.py:973`. O caso conta carimbos em `bin/tests/test_etapas_no_brief.sh:353`, mas a sequência já negou os finais; não isola “finais normalmente conferidos criam carimbos repetidos”. Execução **[nao verificado]**. |
| Harness ETAPAS | **NAO PROCEDE a alegação de aceitar qualquer rc.** | Exige rc≠0 e nome do caso em `bin/tests/test_etapas_no_brief.sh:588`. Isso não garante a causa interna correta, como o mutante anterior mostra. |
| Harness censo vizinho | **PROCEDENTE.** | Aceita qualquer rc≠0 em `bin/tests/test_censo_antes_da_vaga.sh:687`. |
| Harness wrapper | **PROCEDENTE quanto à declaração genérica “rc≠0 e mensagem específica”.** | Não verifica rc nem mensagem específica; procura qualquer `FALHA` emitida por `c_$caso`: `bin/tests/test_codex_pergunta.sh:579`, `:580`. |
| RED/GREEN e mortes executadas | **[nao verificado].** | A cópia não contém `.git`; os arquivos de RED/GREEN descritos na entrega não foram disponibilizados como artefatos independentes. Não usei o relato do construtor como comprovação. |

A exigência de RED evidenciado está em `CLAUDE.md:28`; a exigência de contraexemplo além do golden, em `CLAUDE.md:29`; contador e rótulo devem corresponder ao universo real, em `CLAUDE.md:35`.

## 09/11 14:36 · sessao c9cc3705ce2d · - · - · origem=Ronald (tmux)

| Item | Conclusão | Evidência e contraexemplo |
|---|---|---|
| A3 — unicidade concorrente | **PROCEDENTE: classe permanece aberta.** | `_instalado_lido` ignora carimbos posteriores ao instante particular do chamador (`bin/disjuntor.py:939`); essa filtragem é repetida sob o flock (`bin/disjuntor.py:967`). P1 lê borda 10:00:00, atrasa; P2 lê 10:00:01, adquire primeiro e carimba 10:00:01; P1 adquire depois, ignora esse carimbo e apenda 10:00:00 (`bin/disjuntor.py:969`). Ficam duas linhas mesmo com exclusão correta. |
| A3 — leitura do hook bloqueada pelo flock | **NAO PROCEDE para a leitura direta.** | O trecho citado do hook faz `stat` e comparação de inode, sem adquirir flock no disjuntor.log (`bin/hook_stop_fila1.py:659`, `bin/hook_stop_fila1.py:666`, `bin/hook_stop_fila1.py:679`). O helper de abertura também não adquire flock (`bin/orcamento_contexto.py:73`). |
| A3 — hook pode esperar indiretamente | **PROCEDENTE, com teto nominal de aproximadamente 1 s.** | Stop chama `julga_b3` (`bin/hook_stop_fila1.py:1204`); B3 detém a trava de estado (`bin/disjuntor.py:383`) ao chamar `_confere_etapas` (`bin/disjuntor.py:1320`), que pode disputar a trava do carimbo (`bin/disjuntor.py:1126`, `bin/disjuntor.py:961`). Há 20 tentativas e sleeps de 50 ms (`bin/disjuntor.py:911`, `bin/disjuntor.py:964`). |
| Ordem de travas | **NAO PROCEDE inversão demonstrada.** | B3 adquire estado→carimbo; o carimbo não chama escritor do estado e libera seu `with` antes de retornar (`bin/disjuntor.py:398`, `bin/disjuntor.py:958`, `bin/disjuntor.py:974`). Não demonstrado ciclo reverso neste código. |
| A6 — DESLIGADO | **PROCEDENTE: preservação incompleta.** | Com carimbo ocupado e chave desligada, Agent abre, grava somente FALHA e pula DESLIGADO (`bin/disjuntor.py:2313`, `bin/disjuntor.py:2315`, `bin/disjuntor.py:2334`). Retire a obra da fila antes do próximo B3: `_sujeita` não reconhece essa FALHA (`bin/disjuntor.py:907`, `bin/disjuntor.py:1103`), e o final recebe FORA (`bin/disjuntor.py:1130`). |
| A7 — carimbo antes de dispensar FORA | **NAO PROCEDE a diferença original.** | Agent carimba antes do pertencimento (`bin/disjuntor.py:2094`, `bin/disjuntor.py:2097`); wrapper também (`bin/disjuntor.py:2754`, `bin/disjuntor.py:2775`). |
| A8 — todo caminho novo usa borda | **PROCEDENTE: não fecha a classe.** | Negativas ETAPAS seguem por `trilha()` no wrapper e PreTool (`bin/disjuntor.py:2785`, `bin/disjuntor.py:2338`); esse helper omite `agora` (`bin/disjuntor.py:233`), provocando `datetime.now()` interno (`bin/disjuntor.py:219`). Essas negativas são insumo histórico de sujeição (`bin/disjuntor.py:908`, `bin/disjuntor.py:1104`). FALHA nova de DESLIGADO também usa esse caminho (`bin/disjuntor.py:2313`). Isso fere a classe explicitamente enumerada em `CLAUDE.md:371`. |
| A8 — dois caminhos declarados antigos | **Existem; antiguidade pelo diff fornecido, execução histórica [nao verificado].** | `trilha()` omite instante (`bin/disjuntor.py:233`); `decide_b3` lê relógio se recebe `None` (`bin/disjuntor.py:1298`). Há chamador real que omite: `--b3-parada`→`b3_parada`→`estado_b3` (`bin/disjuntor.py:2855`, `bin/disjuntor.py:1474`, `bin/disjuntor.py:1421`). Stop e PreTool passam instante (`bin/hook_stop_fila1.py:1198`, `bin/hook_stop_fila1.py:1204`, `bin/disjuntor.py:2202`). |
| A9 — wrapper pré-vaga isentando sem ini | **NAO PROCEDE o defeito original.** | Parser de primeira abertura aceita apenas Agent na trilha (`bin/disjuntor.py:904`); wrapper precisa de ini construtor (`bin/disjuntor.py:1086`). |
| Nova falha no ramo de exceção | **PROCEDENTE: abre sem fotografia.** | `_censo_na_excecao` fornece lista temporária `oks=[]` à porta e a descarta (`bin/disjuntor.py:2187`); nenhum escritor publica essa foto nesse ramo. Sendo conforme, retorna `None` e `main_pretool` libera a ferramenta (`bin/disjuntor.py:2190`, `bin/disjuntor.py:2344`, `bin/disjuntor.py:2365`). O novo teste exige apenas stdout vazio, sem conferir fotografia (`bin/tests/test_etapas_no_brief.sh:546`). |

Decisão aberta 1 tem efeito no B3, além da inflação do contador:

- **Falsa reprovação da fotografia antiga:** uma abertura wrapper legítima da v2, posterior ao carimbo I3 já existente, tem SNAPSHOT e ini sem tent. O novo `_SNAP_RE` exige tent (`bin/disjuntor.py:896`, `bin/disjuntor.py:898`), portanto descarta a fotografia. Como o ini é posterior ao carimbo, o final completo recebe SEM-FOTO (`bin/disjuntor.py:1086`, `bin/disjuntor.py:1135`, `bin/disjuntor.py:1143`).
- **Isenção não expira em 24 horas:** `_primeira_abertura` varre todos os ini, sem corte de janela (`bin/disjuntor.py:1086`, `bin/disjuntor.py:1089`). Ini anterior ao carimbo permite SEM-SNAPSHOT para final futuro sem fotografia (`bin/disjuntor.py:1135`). Uma reabertura DESLIGADA posterior também pode herdar essa isenção. Se isenção vitalícia por identidade da obra foi autorizada: **[nao verificado]**.
- **Mesmo segundo perde ordem real:** ini efetivamente anterior ao carimbo, mas persistido no mesmo segundo, falha no `< inst` e recebe SEM-FOTO (`bin/disjuntor.py:1135`, `bin/disjuntor.py:1143`). Persistência reduz instante a segundos (`bin/disjuntor.py:220`).
- O contador legado fica “sem censo” porque casamento exige tent (`bin/disjuntor.py:990`) e falta de casamento soma `sem` (`bin/disjuntor.py:2566`). A limitação de 24 h existe somente nesse medidor (`bin/disjuntor.py:2520`, `bin/disjuntor.py:2565`).

O selo concorrente não garante o cenário entre segundos nem sincroniza os oito leitores: usa relógios individuais, sleep fixo e conta linhas, descartando stderr e sem conferir rc individual (`bin/tests/test_etapas_no_brief.sh:471`, `bin/tests/test_etapas_no_brief.sh:473`, `bin/tests/test_etapas_no_brief.sh:474`, `bin/tests/test_etapas_no_brief.sh:475`). RED/GREEN executados e ocorrência dos contraexemplos em produção: **[nao verificado]**.

## 09/11 14:13 · sessao c07d957d350d · BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS · verificador · origem=Code

As curas específicas de 4, 7a e 7b estão na árvore. Porém, há um **contracaso W no cruzamento entre pendência preservada e irmã retratada**: a metade aceita deixa de ser reconhecida como espera, e C7 permite fechar sem aplicação (`app/chamados/juizes.py:2137`; `app/chamados/services/materializacao.py:1434`, `:112`; `app/chamados/models.py:1863`, `:1953`). Ocorrência em produção e execução desse contracaso: **[nao verificado]**.

| Item | Veredito | Classe | Evidência e contraexemplo concreto |
|---|---|---|---|
| **0 — triagem 4** | **NAO PROCEDE que continue aberto** | W corrigido | S127 agora consulta C7, preservando disputa vazia. O teste exige a recusa específica na transição real, antes da escrita (`app/chamados/juizes.py:2137`; `app/chamados/tests/test_bug_par_intervalo.py:468`). |
| **0 — triagem 7a** | **NAO PROCEDE que continue aberto, nos dois defeitos apontados** | W corrigido | Limpeza exclusiva de ESPERA e preservação do motivo com pendência estão implementadas. O cruzamento restante está no item 3 abaixo (`app/chamados/services/materializacao.py:1376`, `:1434`). |
| **0 — triagem 7b** | **NAO PROCEDE que falte o rótulo** | F | M38 está explicitamente rotulado estrutural; a diferença cobrada é chamada de método (`app/chamados/tests/test_bug_par_intervalo.py:815`, `:842`). |
| **1 — delegação S127** | **NAO PROCEDE duplicação restante em S127** | F | Consome C7 inteiro. Em snapshot estável, C7 não pode liberar quando o antigo `fio_vivo` travava, pois incorpora `not fio_vivo` (`app/chamados/juizes.py:2137`; `app/chamados/models.py:1863`). |
| **2 — recusa residual após aplicação** | **PROCEDENTE** | F | Falha conjunta grava recusa nas duas; reaceitar primeiro uma grava ESPERA nessa; reaceitar a segunda aplica ambas, mas conserva nela a recusa anterior (`app/chamados/services/materializacao.py:1392`, `:1435`, `:1376`). |
| **3 — pendência impede reconhecer espera** | **PROCEDENTE** | **W** | Pendência impede gravar ESPERA; motivo técnico impede o juiz de reconhecer espera. Retratada a irmã, C7 libera fechamento apesar da metade aceita sem aplicação (`app/chamados/services/materializacao.py:1434`, `:112`; `app/chamados/models.py:1863`, `:1953`). |
| **3 — limpeza posterior da pendência** | **NAO PROCEDE como caminho localizado** | F | O escritor encontrado grava valor não vazio; não localizei limpador runtime. Limpeza externa/dinâmica: **[nao verificado]** (`app/chamados/services/materializacao.py:937`). |
| **4 — outra recomposição de I19** | **NAO PROCEDE no censo da cópia** | F | Consulta única compartilhada pelo comando e contraprova; I18 e I6 são outras perguntas, com filtros diferentes (`app/chamados/management/commands/auditar_invariantes_chamados.py:549`, `:1118`, `:1081`, `:536`; `app/chamados/tests/test_bug_par_intervalo.py:503`). |
| **5 — campos com significados incompatíveis** | **PROCEDENTE** | W/F | A incompatibilidade interfere na retenção do fechamento acima; também produz apresentação “não plantou” quando o escritor efetivamente plantou pelo motivo (`app/chamados/services/materializacao.py:476`, `:1014`; `app/chamados/services/explicador.py:93`). |
| **6 — M38 estrutural** | **PROCEDENTE; rótulo verdadeiro** | F | Com três campos vazios, `descarimbar` retorna sem alterar estado ou salvar. M38 pula essa chamada; o teste diferencia pelas chamadas, não pelo resultado funcional (`app/chamados/models.py:3218`; `app/chamados/tests/mutantes_bug_par_intervalo.py:131`; `app/chamados/tests/test_bug_par_intervalo.py:842`). |

O **contracaso W** não precisa tomar a fixture como dado: os escritores permitem esta sequência condicional; sua ocorrência em produção permanece **[nao verificado]**.

1. Uma saída ainda avulsa não encontra tipo pelo marco. A derivação produz pendência e o ramo grava `pendencia_admin` junto do motivo (`app/chamados/services/materializacao.py:79`, `:388`, `:476`, `:937`).
2. Uma recusa posterior, por exemplo competência aprovada, retorna `None`; a saída normal do `atomic` preserva a pendência, enquanto a validação é devolvida. Isso dispensa competência exportada (`app/ponto/janelas.py:87`; `app/chamados/services/materializacao.py:1022`, `:1482`, `:1514`; `app/chamados/services/validacao.py:202`).
3. Nasce a irmã pela emissão; revalidar a primeira com a irmã ainda sem aceite conserva sua validação, pendência e recusa não-ESPERA, sem aplicar a batida (`app/chamados/services/disputa_emissao.py:834`; `app/chamados/services/materializacao.py:1434`; `app/chamados/services/validacao.py:473`).
4. A irmã sai do fato por retratação. A primeira morre no C6 por estar validada; a irmã morre pela identidade. A primeira também fica fora de `respostas_sem_veredito` e do juiz de espera (`app/chamados/models.py:2078`; `app/chamados/juizes.py:1153`, `:1164`; `app/chamados/models.py:1851`; `app/chamados/services/materializacao.py:112`).
5. **C7 libera.** `Disputa.fechar` passa pela consulta sob trava e salva o fechamento. `transicionar('fechado')` também passa: a guarda adicional busca somente perguntas não validadas, e a guarda de FURO é exclusiva de `resolvido` (`app/chamados/models.py:1863`, `:1927`, `:1953`, `:344`, `:181`, `:199`).

A diferença introduzida pela preservação é a **perda de retenção em C7**: gravar ESPERA conservaria a retenção depois da saída da irmã; preservar motivo técnico faz o juiz rejeitá-la. Não atribuo à troca S127 uma nova permissividade lógica: sua composição ficou mais restritiva (`app/chamados/services/materializacao.py:116`, `:112`, `:1434`; `app/chamados/models.py:1863`).

Os sete testes V5 têm a seguinte discriminação, por leitura das asserções; mortes executadas dos mutantes: **[nao verificado]**. [nao verificado]

| Teste em `app/chamados/tests/test_bug_par_intervalo.py` | Passaria sem a cura correspondente? | Onde morde |
|---|---|---|
| Fechado retido, `:472` | **Não** | Exige mensagem específica de S127 em `:468`; não basta qualquer bloqueio. M45 substitui justamente essa delegação (`app/chamados/tests/mutantes_bug_par_intervalo.py:157`). |
| Resolvido retido, `:475` | **Não** | A eventual recusa posterior por FURO não satisfaz a mensagem exigida em `:468` (`app/chamados/models.py:192`). M48: `app/chamados/tests/mutantes_bug_par_intervalo.py:167`. |
| Disputa vazia, `:478` | **Sim** | Controle de preservação: retorno da disputa e transição bloqueada em `:482`, `:484`. |
| Todas mortas, `:488` | **Sim** | Controle positivo: `None` em `:496` e status persistido em `:499`. |
| Pendência anterior, `:506` | **Não** | Sobrescrita antiga falha imediatamente na comparação do motivo em `:517`, antes de depender do I19. |
| Pendência nascida no plantio, `:527` | **Não** | Observa os campos produzidos no ramo e compara depois da aplicação em `:542`, `:552`. |
| Limpeza exclusiva, `:556` | **Não** | Limpeza ampla falha em `:565`; nunca limpar falha em `:566`. |

Esses testes cobrem separadamente pendência preservada e irmã retratada com ESPERA; não combinam pendência preservada com irmã retratada (`app/chamados/tests/test_bug_par_intervalo.py:459`, `:506`).

O censo AST, considerando importações com apelido e atribuições simples, encontrou estes quatro sítios de chamada executável; reflexão arbitrária e nomes montados dinamicamente: **[nao verificado]**.

| Chamador de `disputa_viva_do_chamado` | Uso do retorno |
|---|---|
| `app/chamados/models.py:154`, apelido `_disp_viva` importado em `:153` | Disputa retornada provoca `ValueError`; ausência permite prosseguir até `_aplicar` (`:155`, `:199`). `Chamado.fechar` chega por `transicionar` (`:454`). |
| `app/chamados/tests/test_disputa_s127.py:45` | Devolve resultado ao teste e cobra uma consulta C7 em `:47`. |
| `app/chamados/tests/test_bug_par_intervalo.py:482` | Cobra retorno da disputa vazia. |
| `app/chamados/tests/test_bug_par_intervalo.py:496` | Cobra `None`, seguido da transição real em `:497`. |

Inspeção de fonte e alvos de mutação não são chamadas ao juiz (`app/chamados/tests/test_pergunta_viva_no_guarda.py:37`; `app/chamados/tests/mutantes_bug_par_intervalo.py:157`, `:167`). Os caminhos automáticos localizados consultam C7 diretamente, inclusive reconciliador, materializador e beco (`app/chamados/reconciliador.py:411`, `:1170`; `app/chamados/services/materializacao.py:1534`; `app/chamados/services/becos.py:56`).

**C7 não é inteiramente puro nem barato por construção.** Carrega perguntas duas vezes; para cada pergunta elegível à espera, o identificador consulta irmãs e escala, formando padrão N+1. Contagem e latência reais: **[nao verificado]** (`app/chamados/models.py:1862`, `:1850`; `app/chamados/services/materializacao.py:114`, `:70`, `:87`). A cadeia C6 pode ler `date.today()`, já alcançável pelo `fio_vivo` antigo (`app/chamados/juizes.py:1217`, `:575`, `:1262`). Há memos em instância e um efeito persistente condicional: a leitura de toque humano chama `get_user_sistema`, implementado com `get_or_create`; ausência do usuário em produção: **[nao verificado]** (`app/chamados/juizes.py:862`, `:1087`; `app/core/utils.py:18`). Esses pontos são **F**, sem escrita errada ou efeito financeiro demonstrado pelo dado fornecido **[nao verificado]**.

Os valores de `recusa_motivo` não formam enumeração fechada: o campo aceita texto livre, e os escritores abaixo permitem resíduos anteriores à aplicação (`app/chamados/models.py:2577`).

| Valor/origem | Escritor e elegibilidade |
|---|---|
| Motivo técnico de tipo não conferido | Produzido na derivação e gravado com pendência (`app/chamados/services/materializacao.py:479`, `:937`). Deve sobreviver enquanto representar essa pendência; apagá-lo viola I19 (`app/chamados/management/commands/auditar_invariantes_chamados.py:549`). |
| Recusa de paridade, data, competência ou falha conjunta | `_carimbar_recusa` grava o texto; produtores incluem guarda de paridade, data e falha do par (`app/chamados/services/materializacao.py:1190`, `:1000`, `:1324`, `:1392`). |
| `Recusada por ...` | Recusa humana grava; resposta posterior normal não limpa automaticamente esse campo (`app/chamados/services/validacao.py:605`; `app/chamados/services/disputa_emissao.py:1483`, `:1505`). |
| `Colaborador respondeu ... exige decisao...` | Escape grava; resposta normal posterior pode preservar o texto (`app/chamados/services/disputa_emissao.py:1494`, `:1483`). |
| Descrição de duplicata | Porta de identidade grava, mas essa linha fica fora do universo do par (`app/chamados/services/fio.py:1350`; `app/chamados/services/materializacao.py:72`). |
| ESPERA | Envelope usa o escritor existente; aplicação limpa exclusivamente esse valor (`app/chamados/services/materializacao.py:1435`, `:1376`). |

Uma recusa histórica sem pendência pode, portanto, ficar obsoleta após sucesso; **não encontrei escrita posterior errada causada apenas por esse resíduo** nos leitores pedidos. Devolução ignora materializadas; validação usa erros do ato; vigia e passivo reconhecem lastro; flip exige a via `paridade_recusada`, diferente da aplicação normal; I18 exige não materializada (`app/chamados/services/validacao.py:190`, `:491`, `:511`; `app/chamados/services/vigia_carimbo.py:112`; `app/chamados/services/passivo_validado.py:329`; `app/ponto/services/flip_auto.py:128`; `app/chamados/services/materializacao.py:1283`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1081`). **F** para esse resíduo isolado; efeito adicional em produção: **[nao verificado]**.

Na apresentação, a metade técnica validada some da lista do detalhe porque não é C6 viva nem espera; o fio de disputa aberta carrega todas e mostra pendência/motivo no modal (`app/chamados/services/acoes_chamado.py:349`; `app/chamados/juizes.py:1153`; `app/chamados/services/fio.py:221`, `:225`; `app/templates/chamados/partials/modal_fio.html:222`). O explicador afirma “NAO plantou” antes de consultar lastro, embora o escritor possa plantar e conservar pendência; o cartão também diz conferir “antes de lançar”. Essa divergência é **F** (`app/chamados/services/explicador.py:93`, `:101`; `app/chamados/services/materializacao.py:1014`; `app/templates/chamados/partials/_card_atendimento.html:183`).

`o_que_segura` consulta exportação, dias retidos e a porta do dia; não lê diretamente pendência/motivo. A alimentação dessa porta usa C6, que exclui a metade validada. A frase resultante depende das demais portas daquele dia e é **[nao verificado]** para o dado fornecido (`app/folha/fora_da_folha.py:215`, `:221`, `:226`; `app/chamados/juizes.py:1322`, `:1153`).

Quanto às demais classes estruturais, as batidas continuam pela porta, os carimbos pela porta do modelo e as travas pelo adquirente comum em ordem Colaborador → Disputa → Chamado → Pergunta; não identifiquei outro escritor introduzido fora dessas portas nos caminhos examinados. Prova concorrente com duas conexões: **[nao verificado]** (`app/chamados/services/materializacao.py:1014`, `:1283`, `:134`; `app/chamados/models.py:2683`, `:2692`, `:2705`, `:2709`). A identidade e os instantes continuam consumindo suas fontes existentes, sem nova derivação paralela nesses trechos (`app/chamados/services/materializacao.py:87`, `:88`, `:1349`).

M38 satisfaz exatamente a alternativa estrutural solicitada: o rótulo existe, e o executável confirma a limitação. As asserções de estado/trilha desse cenário não constituem contracaso comportamental à guarda antiga (`app/chamados/tests/test_bug_par_intervalo.py:815`, `:842`, `:864`; `app/chamados/models.py:3218`).

## 09/11 14:11 · sessao d1463be4513a · - · - · origem=Ronald (tmux)

**Cruzamento PROCEDENTE, classe W, demonstrável estaticamente; ocorrência em produção [nao verificado].**

O estado `validada_em != None`, `materializada_em == None`, `pendencia_admin != ''`, `recusa_motivo` técnico pode nascer por portas reais:

1. Antes de existir a irmã, saída responde `12:00`; destino não tem marco do motivo, e escala 19–07 com pausa 01–02 deixa todos os marcos a mais de 240 minutos da resposta. `_tipo_pelo_marco` não acha tipo pela tolerância (`app/chamados/services/materializacao.py:374`–`:388`); `_ts_tipo_do_plantio` anexa `PEND_TIPO_NAO_CONFERIDO`, mas continua derivação (`:476`, `:493`).
2. O ramo grava os dois campos (`app/chamados/services/materializacao.py:937`–`:938`). Com competência **aprovada, sem precisar ser exportada**, `registrar_batida` devolve recusa (`app/ponto/janelas.py:87`, `app/ponto/registro_batida.py:139`).
3. O ramo retorna `None` (`app/chamados/services/materializacao.py:1022`–`:1027`); `_materializar_avulsa` retorna de dentro do `atomic` sem levantar (`:1482`–`:1483`), preservando a pendência. O `finally` grava o motivo da recusa (`:1514`–`:1515`); competência não é `COLISAO`, logo não ganha carimbo materializado (`:1181`, `:1189`). A validação é devolvida pela porta (`app/chamados/services/validacao.py:470`, `:202`–`:205`).
4. Nasce a irmã e o admin revalida a primeira com confirmação de implausibilidade. Com par incompleto, V5 pula o carimbo de espera por haver pendência (`app/chamados/services/materializacao.py:1434`); o ramo de par da validação não chama `devolver_validacao_recusada` (`app/chamados/services/validacao.py:473`–`:479`). Ficam aceite, pendência e recusa não-ESPERA, sem plantio.
5. A irmã sai por `remover_motivo`, que carimba identidade de retratação (`app/chamados/models.py:2071`–`:2079`). A primeira morre no degrau `validada_em` (`app/chamados/juizes.py:1153`); a irmã morre pela identidade (`:1164`). `respostas_sem_veredito` exclui ambas (`app/chamados/models.py:1851`), e o juiz de espera exclui a primeira por sua recusa técnica (`app/chamados/services/materializacao.py:112`). **C7 passa a devolver True** (`app/chamados/models.py:1863`).

O fechamento humano real passa S127 e chega à escrita: `transicionar('fechado')` chama S127 (`app/chamados/models.py:154`), a guarda adicional busca somente perguntas **não validadas** (`:344`), e aplica em `:199`, `:450`, salvando estado em `:296`. A primeira técnica não entra; a irmã retratada não está viva.

Há ainda efeito real de escrita da **disputa**: `Disputa.fechar` consulta C7 sob trava (`app/chamados/models.py:1927`) e salva fechamento em `:1953`. Se o pai estiver terminal, o beco classifica por C7 (`app/chamados/services/becos.py:31`–`:33`) e chama `d.fechar` em `:56`–`:57`; saves de pergunta agendam essa entrada em `app/chamados/signals.py:225`.

**Comparação com ac3/v4:** a mudança V5 de `if q.validada_em` para `if q.validada_em and not q.pendencia_admin` introduz a perda de retenção de C7: antes a primeira ganhava ESPERA, que continuava retendo após retratação da irmã (`app/chamados/services/materializacao.py:116`); agora preserva motivo técnico, que o mesmo juiz rejeita em `:112`. O S127 antigo já permitia o fechamento **humano** nesse cenário, pois ignorava toda espera; portanto esse ato humano não deve ser apresentado como regressão nova da troca S127. A liberação nova comprovável é **C7 permitir fechamento da disputa**, e V5 também deixa passar o mesmo cruzamento no S127 atual (`app/chamados/juizes.py:2137`).

Os testes existentes cobrem separadamente preservação técnica (`app/chamados/tests/test_bug_par_intervalo.py:508`) e irmã retratada com ESPERA (`:459`–`:465`); não combinam as condições. Seu GREEN não demonstra que esse cruzamento esteja fechado.

## 09/11 14:10 · sessao f88b05a1eeb9 · - · - · origem=Ronald (tmux)

Itens 2/3/5 — provas consolidadas:

| Ponto | Veredito/classe | Prova e contraexemplo |
|---|---|---|
| Recusa técnica deve sobreviver à aplicação | NÃO PROCEDE apagar indiscriminadamente, F | `_ts_tipo_do_plantio` sem marco acrescenta `tipo_nao_conferido` e texto dizendo **“batida plantada ... pelo MOTIVO”** (`app/chamados/services/materializacao.py:476`); ramo hora grava ambos (`:937`), pode plantar (`:1014`) e o par limpa exclusivamente ESPERA (`:1376`). Limpar motivo técnico deixaria `pendencia_admin` sem motivo, precisamente consulta I19 (`app/chamados/management/commands/auditar_invariantes_chamados.py:549`). |
| Recusa não técnica pode sobrar num par aplicado | PROCEDENTE, F | Falha escreve recusa nas duas e devolve validação (`materializacao.py:1392`). Após corrigir a causa, primeiro reaceite grava ESPERA só na primeira (`:1434`); segundo reaceite aplica ambas e limpa apenas ESPERA (`:1376`). A segunda conserva “intervalo recusado em conjunto”, embora aplicada. Ocorrência no dado prod: [nao verificado]. | [nao verificado]
| Leitores provocam escrita errada por essa recusa residual | NÃO PROCEDE demonstrado, F | Devolução retorna antes de ler motivo quando materializada (`app/chamados/services/validacao.py:190`); validação usa erros do ato (`:491`, `:511`) e retorna sucesso (`:538`). Vigia dá `com_registro` pelo lastro (`app/chamados/services/vigia_carimbo.py:112`); reparador pula essa classe (`app/chamados/services/passivo_validado.py:329`). Flip exige `via_resolucao=VIA_PARIDADE_RECUSADA` (`app/ponto/services/flip_auto.py:128`), enquanto aplicação usa a via pelo fato/declarou (`materializacao.py:1276`, `:1285`). I18 exige não materializada (`auditar_invariantes_chamados.py:1081`), I19 exige pendência (`:549`). |
| Pendência anterior tira metade da apresentação de espera | PROCEDENTE, F; há desdobramento W abaixo | Espera não é gravada quando há pendência (`materializacao.py:1434`) e juiz rejeita motivo técnico (`:112`). Detalhe inclui somente C6 viva ou espera (`app/chamados/services/acoes_chamado.py:349`), enquanto C6 mata validada (`app/chamados/juizes.py:1153`): a metade validada com pendência fica fora dessa lista. Fio aberto carrega todas as perguntas (`app/chamados/services/fio.py:221`, `:225`), portanto nela permanece visível com badge “Pendencia do admin” e motivo (`app/templates/chamados/partials/modal_fio.html:222`). |
| Pendência pode ser limpa depois sem regravar espera | NÃO PROCEDE na cópia, F | Único escritor runtime literal encontrado grava valor não vazio em `materializacao.py:937`; não há limpeza runtime localizada. Limpeza por SQL externo/dinâmico ou dado prod: [nao verificado]. |
| Semântica “não plantou” versus “plantada sem tipo confirmado” | PROCEDENTE, F | Escritor planta pelo motivo e acende pendência (`materializacao.py:476`, `:937`, `:1014`); display do modelo diz “plantada pelo motivo” (`app/chamados/models.py:2571`). Explicador prioriza pendência e afirma “sistema NAO plantou” antes de consultar lastro (`app/chamados/services/explicador.py:93`, `:101`); cartão diz conferir “antes de lançar” (`app/templates/chamados/partials/_card_atendimento.html:183`). Isso pode ocorrer inclusive após sucesso do par (`materializacao.py:1376`). |
| Pendência + irmã retratada libera fechamento com metade não aplicada | **PROCEDENTE condicional pelo código, W** | Há rota runtime abaixo; não foi executada e ocorrência em prod é [nao verificado]. | [nao verificado]

Contraexemplo W, sem depender de fixture:

1. Pergunta `intervalo_saida` está avulsa, sem irmã (`materializacao.py:79`); sem tipo identificável, ramo gera pendência (`:467`, `:476`) e grava `pendencia_admin`+motivo (`:937`).
2. Vizinha S posterior aciona paridade (`:962`); marco não aceso leva a `_carimbar_recusa` e `return None` (`:987`, `:1000`). `_materializar_avulsa` retorna normalmente do atomic (`:1482`), preservando pendência; validação devolve `validada_em` a nulo (`validacao.py:470`, `:202`).
3. Irmã passa a existir pela porta de emissão (`app/chamados/services/disputa_emissao.py:829`, `:834`, `:878`). Reaceitar a saída com irmã sem aceite chega a `materializacao.py:1434`, conserva pendência/recusa e deixa saída validada sem materialização.
4. Remoção do motivo da irmã retrata-a pela porta (`app/chamados/models.py:2071`, `:2078`); sobrando o motivo da saída, retorna sem fechar (`:2083`).
5. A saída morre no C6 por estar validada (`juizes.py:1153`); irmã retratada morre por identidade (`:1164`). Saída não entra em respostas sem veredito (`models.py:1851`), nem em espera por ter motivo técnico (`materializacao.py:112`). Logo C7 libera (`models.py:1863`).
6. S127 libera (`juizes.py:2137`); `transicionar('fechado')` supera guarda C6 ancorada (`models.py:165`), evita guarda de FURO que só vale para `resolvido` (`:181`) e chama `_aplicar` (`:199`). A guarda final também só retém C6 viva (`:224`), antes de gravar estado (`:228`). **Fecha chamado com metade aceita e não aplicada**. Na versão anterior do envelope, o primeiro reaceite sobrescreveria o motivo por ESPERA; após retratar irmã, fallback `materializacao.py:116` reteria C7. [nao verificado]

Escritores de `recusa_motivo` e valores alcançáveis:

- Motivo técnico: `materializacao.py:937`, derivado em `:479`.
- Espera, recusas de paridade/data/competência e falha conjunta: `_carimbar_recusa` em `:1190`; produtores em `:1000`, `:1312`, `:1392`, `:1417`, `:1435`, `:1515`.
- Recusa humana “Recusada por ...”: `validacao.py:605`; reabertura não limpa motivo (`disputa_emissao.py:1590`, `:1629`), resposta normal também não o limpa (`:1483`, `:1505`).
- Escapes “Colaborador respondeu ... exige decisao”: `disputa_emissao.py:1494`; resposta normal posterior pode preservar esse texto (`:1483`).
- Duplicata escreve descrição apontando canônica (`fio.py:1350`), mas linha fica fora do fato e é excluída da identidade do par (`materializacao.py:72`).
- Porta do modelo grava motivo fornecido (`models.py:3196`); limpeza do par está em `materializacao.py:1377`.

`o_que_segura` não lê diretamente esses campos: usa exportação e dias retidos (`app/folha/fora_da_folha.py:215`), lê `porta_alcancavel_do_dia` (`:65`, `:221`) e traduz fio por DESTRAVA (`:226`). Alimentação da porta considera C6 (`app/chamados/juizes.py:1322`), não espera do par; metade validada não participa desse conjunto (`:1153`). Resultado concreto do dado prod nessa ferramenta: [nao verificado].

## 09/11 14:08 · sessao d1463be4513a · - · - · origem=Ronald (tmux)

**Item 1 — NÃO PROCEDE que S127 ainda duplique C7; PROCEDENTE custo/leitura indireta de relógio e efeito colateral condicional, classe F.** S127 consulta C7 e preserva exceção vazia em `app/chamados/juizes.py:2134` e `:2137`; C7 incorpora `respostas_sem_veredito`, `fio_vivo` e espera em `app/chamados/models.py:1862`.

Censo AST de todos os `.py`, com importações `as` e propagação de atribuição simples de função:

| Sítio executável | Consumo do retorno |
|---|---|
| `app/chamados/models.py:153`, chamada `:154`, alias `_disp_viva` | `transicionar` levanta `ValueError` se recebeu disputa (`:155`); sem bloqueio chega a `_aplicar` em `:199`, que chama `_gravar_estado` em `:450`; escreve estado em `:228` e salva no corpo seguinte. |
| `app/chamados/tests/test_disputa_s127.py:45` | Guarda resultado e o devolve após cobrar consulta C7 em `:47`. |
| `app/chamados/tests/test_bug_par_intervalo.py:482` | Cobra disputa sem perguntas retornada. |
| `app/chamados/tests/test_bug_par_intervalo.py:496` | Cobra `None` quando todas morreram sem resposta pendente; depois transiciona realmente em `:497`. |

Referências não executáveis à função: `inspect.getsource` em `app/chamados/tests/test_pergunta_viva_no_guarda.py:37`; nomes dos alvos mutantes em `app/chamados/tests/mutantes_bug_par_intervalo.py:157` e `:167`; referência histórica em docstring de `app/chamados/tests/test_o261_fio_tem_uma_porta.py:25`. Acessos construídos dinamicamente por strings e reflexão arbitrária: **[nao verificado]**.

**Comparação de liberação:** em um mesmo snapshot, o novo bloqueio é mais restritivo: C7 só é verdadeiro com `not fio_vivo(perguntas)`, além das demais condições (`app/chamados/models.py:1863`). Portanto não há caso lógico estável em que C7 libere com o S127 antigo travando; concorrência entre consultas distintas: **[nao verificado]**. O contrário existe: saída validada em espera e volta retratada fazem ambas morrerem pelo C6, mas C7 retém a espera; cenário e asserções em `app/chamados/tests/test_bug_par_intervalo.py:459`, `:463`, `:465`. Isso alcança escrita por `transicionar('fechado'/'resolvido')`, bloqueada pela mensagem específica de S127 em `:468`; os wrappers estão em `:472` e `:475`.

**Disputa vazia continua bloqueando:** curto-circuito `not disputa.perguntas.exists()` em `app/chamados/juizes.py:2137`; teste chama a transição real e verifica estado/trilha intactos em `app/chamados/tests/test_bug_par_intervalo.py:480`, `:484`, `:486`. Este contracaso passaria também com a regra antiga; protege preservação, não demonstra a cura.

**Custo:** há consulta adicional `exists()` em `app/chamados/juizes.py:2137`, e C7 lê perguntas duas vezes, uma por `list(self.perguntas.all())` e outra em `respostas_sem_veredito()` (`app/chamados/models.py:1862`, `:1850`). Além disso, para cada pergunta elegível, a espera chama identificador (`app/chamados/services/materializacao.py:114`), que consulta irmãs em `:70` e escala em `:87`; o carregador consulta célula em `app/escala/servico_jornada.py:46`, e `pausas_do_dia` pode buscar novamente em `app/escala/models.py:1187`. Existe padrão N+1; quantidade real de queries/latência em produção: **[nao verificado]**.

**Efeitos colaterais:** C7 não contém `save/update`, mas sua cadeia não é inteiramente pura: C6 grava memo na instância em `app/chamados/juizes.py:862`; tranca grava memo em `:996` e `:1066`. `respostas_sem_veredito` pode alcançar `dia_da_pergunta_trancado` (`app/chamados/models.py:1854`), `_admin_tocou_o_dia_depois` (`app/chamados/juizes.py:971`), `_toque_humano_no_dia` (`:1064`) e `get_user_sistema` (`:1087`), cujo código é `User.objects.get_or_create` em `app/core/utils.py:18`. Pode criar usuário ausente; ausência desse usuário em produção e escrita errada: **[nao verificado]**. Em falhas engolidas, há contador em memória e log em `app/core/observ.py:75` e `:79`.

**Relógio:** C7 pode alcançar o C6 e `_dentro_da_janela_viva` (`app/chamados/juizes.py:1217`), que chama `date.today()` em `:575`. A cadeia já existia no S127 antigo por `fio_vivo`; não é leitura nova demonstrada da volta 5 (`app/chamados/juizes.py:1262`). Não afirmar “zero relógio”.

**Cron:** não apareceu chamada direta de cron à função no censo AST; os sítios automáticos relevantes consultam C7 diretamente em `app/chamados/reconciliador.py:411`, `:611`, `:1170` e `app/chamados/services/materializacao.py:672`, `:1534`. O único chamador de produção de S127 é `transicionar`; `Chamado.fechar` delega a ele em `app/chamados/models.py:454`. Escrita automática que passa por `_gravar_estado` sem `transicionar` não ganha automaticamente esse bloqueio S127, pois o gate mostrado é local a `transicionar` (`app/chamados/models.py:148`).

**Item 5 — juiz duplicado:** a recomposição antiga foi eliminada de S127 em favor de C7 (`app/chamados/juizes.py:2137`); fio continua reduzindo exclusivamente por C6 em `:1262`. Não há novo escritor de status nesta cura: o gate lança erro e a escrita continua em `_aplicar`/`_gravar_estado` (`app/chamados/models.py:155`, `:199`, `:450`). O risco de custo e relógio acima é **F**; ocorrência concreta de estado errado ou efeito exportada/dinheiro por essa troca: **[nao verificado]**.

## 09/11 14:08 · sessao 724a79eb995d · - · - · origem=Ronald (tmux)

| Item | Veredito | Classe | Evidência e contracaso |
|---|---|---|---|
| 0 / triagem 4 | **NAO PROCEDE que continue aberto.** | W corrigido | S127 consulta `not disputa.pode_fechar()` e preserva vazio em `app/chamados/juizes.py:2137`; `transicionar` aborta antes de `_aplicar` em `app/chamados/models.py:154` e `:199`. Contracaso discriminante: metade validada + irmã retratada, C6 sem vivas e C7 falso, em `app/chamados/tests/test_bug_par_intervalo.py:459`. |
| 0 / triagem 7a | **NAO PROCEDE que continue aberto.** | W corrigido | Limpa somente espera em `app/chamados/services/materializacao.py:1376`; não sobrescreve motivo com pendência em `:1434`. Motivo técnico nasce conjuntamente com pendência em `:937`. |
| 0 / triagem 7b | **NAO PROCEDE que falte o rótulo.** | F | O próprio teste rotula M38 estrutural em `app/chamados/tests/test_bug_par_intervalo.py:815`; a limitação é confirmada pelas operações executáveis, abaixo. |
| 4 | **NAO PROCEDE derivação paralela de I19 encontrada.** | F | Consulta única `app/chamados/management/commands/auditar_invariantes_chamados.py:549`, consumida pelo comando em `:1118` e contraprova em `app/chamados/tests/test_bug_par_intervalo.py:503`. Censo AST de palavras-chave `pendencia_admin` em chamadas Python encontrou apenas essa consulta e a fixture `:513`; acessos dinâmicos [nao verificado]. |
| 6 | **PROCEDENTE: M38 segue estrutural e o rótulo é verdadeiro.** | F | Fixture exige três campos vazios em `app/chamados/tests/test_bug_par_intervalo.py:827`; nessa condição `descarimbar` retorna antes de alterar campos, cache ou salvar, em `app/chamados/models.py:3218`. M38 acrescenta o curto-circuito pelo carimbo em `app/chamados/tests/mutantes_bug_par_intervalo.py:134`. A diferença cobrada é a lista de chamadas em `app/chamados/tests/test_bug_par_intervalo.py:842`, repetida em `:859`; asserções de estado/trilha não distinguem as implementações nesse cenário. |

Discriminação dos sete testes V5, inferida do código; mortes executadas dos mutantes **[nao verificado]**: [nao verificado]

| Teste em `app/chamados/tests/test_bug_par_intervalo.py` | Passaria sem a cura correspondente? | Asserção discriminante |
|---|---|---|
| `:472`, fechado retido | **Não.** | `assertRaisesRegex` em `:468`: S127 antigo libera o caminho sem vivas; novo aborta com mensagem de disputa. Guarda real em `app/chamados/models.py:155`. M45 em `app/chamados/tests/mutantes_bug_par_intervalo.py:157`. |
| `:475`, resolvido retido | **Não.** | Mesma asserção `:468`; eventual recusa posterior por FURO (`app/chamados/models.py:181`, `:192`) não satisfaz o texto exigido. M48 em `app/chamados/tests/mutantes_bug_par_intervalo.py:167`. |
| `:478`, disputa vazia | **Sim**, relativamente à troca S127. É controle de preservação. | C7 verdadeiro em `:481`, S127 retorna disputa em `:482`, transição bloqueada em `:484`. |
| `:488`, todas mortas | **Sim**, relativamente à troca S127. É controle positivo de liberação. | Retorno `None` em `:496` e status persistido fechado em `:499`. |
| `:506`, pendência anterior | **Não.** | A sobrescrita antiga falha imediatamente na comparação do motivo em `:517`; portanto não depende apenas de I19. Preservação após aplicação também cobrada em `:523`. M47 em `app/chamados/tests/mutantes_bug_par_intervalo.py:164`. |
| `:527`, pendência nascida no plantio | **Não.** | Captura os dois campos reais produzidos durante o plantio em `:542`, comprova origem em `:549`/`:550`, compara após aplicação em `:552`. Limpeza ampla apaga o motivo e falha nessa comparação. M46 em `app/chamados/tests/mutantes_bug_par_intervalo.py:161`. |
| `:556`, limpeza exclusiva | **Não** com limpeza ampla antiga; também não com limpeza desativada. | Limpeza ampla falha em `:565` (outro motivo preservado); nunca limpar falha em `:566` (espera vazia). M49 cobre a segunda mutação em `app/chamados/tests/mutantes_bug_par_intervalo.py:171`. |

O selo estrutural S127 exige chamada a `pode_fechar`, proíbe recomposição nominal de C6/espera e confere a cadeia C7→C6 em `app/chamados/tests/test_pergunta_viva_no_guarda.py:39`. O teste com dublê cobra efetivamente uma chamada C7 em `app/chamados/tests/test_disputa_s127.py:47`; os casos funcionais V5 acima sustentam o comportamento real em `app/chamados/tests/test_bug_par_intervalo.py:469`.

_3179 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
