# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 13:33** · publicado em **12/11/2026 13:34** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 13:33 · sessao 3ddb216d4271 · AUDITORIA-DE-TRAVAS · leitura · origem=Code

**O contrato v2 está errado como está: troca a fonte do topo, mas não especifica a troca de todos os juízes da fila; além disso, sua fórmula de andáveis e seu selo contradizem o RED que o aval manda manter.** (`contrato_item3_v2.txt:3`, `:6`; `aval_respostas_1211_1230.txt:4`; `bin/tests/test_hook_andavel_e_a_ordem_viva.sh:59`, `:72`)

Nas referências aos dois anexos, numerei as linhas do **texto fornecido**, começando pelo título como linha 1. Não são referências a arquivos localizados nesta cópia. A análise abaixo trata o BACKLOG como evidência documental; pousos e medidas de produção nele declarados não foram confirmados em banco ou log. [nao verificado]

**1. Censo**

Na coluna “efeito”, confronto cada leitor com a determinação de retirar marcador e coluna da condição de fonte. (`aval_respostas_1211_1230.txt:4`; `contrato_item3_v2.txt:2`)

| Sítio | Pergunta ou escrita atual | Efeito da fonte nova |
|---|---|---|
| `bin/gerar_backlog.py:110`, `:119`, `:127` | Lê e republica a janela antiga desde o marcador até `OBRAS:FIM`, incluindo a prosa intermediária. | Precisa distinguir conteúdo manual preservado de publicação derivada; copiar essa janela integralmente pode recolocar marcador/lista antigos. |
| `bin/gerar_backlog.py:131`, `:138` | Exige presença dos textos do marcador e de `OBRAS:INICIO`; escreve BACKLOG por `.novo` e `os.replace`. | A guarda de presença não certifica igualdade com a fonte estruturada. |
| `bin/hook_stop_fila1.py:149`, `:171` | `_item_fechado` decide pela coluna de estado ou tachado na descrição; `_item_parado` acrescenta congelamento, fila 2, espera, PAREI e portão. | Esses predicados precisam deixar de decidir pertencimento e andabilidade da ORDEM. |
| `bin/hook_stop_fila1.py:192`, `:209`, `:225` | `_item_da_ordem_viva`: qual é o topo? Extrai marcador, procura o id no bloco OBRAS e veta pela coluna. | Trocar apenas a extração do marcador deixa o veto da coluna funcionando como segunda fonte. |
| `bin/hook_stop_fila1.py:231`, `:250`, `:256` | `_itens_andaveis` enumera todos os não parados de OBRAS; `_ha_item_aberto` decide fila vazia por essa lista. | Ambos precisam migrar; mudar somente `_andaveis_fora_do_em_curso` não resolve o vazio. |
| `bin/hook_stop_fila1.py:270`, `:276` | Quais e quantos andam além do topo? O recado manda buscar o próximo no adendo. | A contagem muda com a exclusão do ruído; a orientação ainda levaria o humano à prosa se permanecer. |
| `bin/hook_stop_fila1.py:290`, `:312` | Diagnostica marcador quebrado e oferece o item em curso aos demais leitores. | Diagnóstico precisa passar a “fonte quebrada”; o topo só permanece igual se o primeiro id estruturado for o mesmo. |
| `bin/hook_stop_fila1.py:323`, `:344` | `_ids_que_nao_andam`: quais ids estão parados pela coluna? | Continua juiz ativo: essa lista veta o PAREI em `:1298`. |
| `bin/hook_stop_fila1.py:349`, `:378`, `:380` | `travas_da_fila`: quais obras não fechadas têm palavra de trava na coluna? | Não pode continuar sustentando a conclusão de “fila inteira travada” pela prosa. |
| `bin/hook_stop_fila1.py:1232`, `:1245` | `_relato_parou`: PAREI do em curso devolve turno somente sem outro andável. | A resposta muda quando se retiram linhas abertas fora da ORDEM. |
| `bin/hook_stop_fila1.py:1248`, `:1298`, `:1310` | `_parei_do_em_curso`: reconhece `!`, rejeita ids parados pela coluna e compara com o topo. | Também precisa migrar o veto por coluna, não apenas a obtenção do topo. |
| `bin/hook_stop_fila1.py:1391`, `:1400` | Sem topo válido, decide entre fila vazia e marcador quebrado consultando OBRAS. | Precisa distinguir fonte válida vazia de fonte inválida usando a nova fonte. |
| `bin/handoff_sessao.sh:54`, `:64`, `:71`, `:120` | Publica topo, em curso, trava do item e devolução do turno, perguntando ao hook. | Herda a migração do juiz; rótulos e respostas sobre PAREI também mudam. |
| `bin/handoff_sessao.sh:163`, `:171`, `:209`, `:216` | Perguntas distintas: avais de hoje não cumpridos e linhas abertas da raia20, pelo fechamento da coluna. | Podem continuar como informação documental, mas não como exclusão de pousadas ou definição da ORDEM. |
| `bin/handoff_sessao.sh:290`, `:294`, `:299`, `:314` | Sem próximo, mostra lista travada e afirma que o turno se devolve; descreve próximo como primeiro aberto de OBRAS ou manda conferir a ordem à mão. | Precisa mudar: topo inválido não significa fila inteiramente travada; os textos ainda encaminham à autoridade antiga. |
| `bin/disjuntor.py:1671`, `:1682` | `_fila1_viva`: quais obras exigem censo? Obtém todos os andáveis de OBRAS. | Universo muda para o estruturado; linhas abertas fora dele deixam de integrar essa lista. |
| `bin/disjuntor.py:1720`, `:1736` | `_pertence_fila1`: esta obra pertence à fila? Usa token e nomeação inteira, com recusa de ambiguidades. | Contrato precisa definir identidade exata dos ids, inclusive qualificações e aliases. |
| `bin/disjuntor.py:1746`, `:1755`, `:2355` | Aplica ou dispensa o censo conforme pertencimento; CLI usa a mesma pergunta. | Herda a mudança de universo. “Não muda de resposta” não vale para itens retirados da lista. |
| `bin/codex_pergunta.sh:228` | Chama o censo antes de obter a vaga. | Leitor indireto; deve continuar delegando ao disjuntor. |
| `bin/backlog_estado.py:56`, `:57`, `:59`, `:62` | Escreve estado e compara próximo/ids parados antes e depois. | Escrever coluna precisa deixar de mudar a ORDEM. Hoje o script **escreve e informa**, apesar da docstring dizer que para quando muda o veredito (`:8`, `:66`). |
| `bin/interrompidas_estado.py:89`, `:93`, `:105`, `:121`, `:125` | Lê INTERROMPIDA/FUNDIDA; pergunta topo ao hook; considera retorno por topo, fusão ou fechamento. | Topo migra indiretamente; fechamento documental continua pela coluna, se mantido. Não pode definir a exclusão estruturada de pousadas. |
| `bin/gerar_estado.py:236`, `:241` | Publica interrompidas e topo do medidor. | Leitor indireto; diagnóstico de marcador em `:243` precisa acompanhar a fonte nova. |
| `bin/alarme_sessao_ociosa.py:89`, `:96`, `:113` | **Parser próprio:** conta linhas de `## OBRAS`, excluindo ☑/FEITO/FECHAD na linha inteira. | Continua segundo juiz se não migrar: essa contagem decide fila vazia e disparo (`:40`, `:148`). |
| `bin/placar_code.sh:368` | Publica o alarme de sessão. | Herda o leitor anterior. |
| `bin/fabricante_alvo.py:238`, `:251`, `:273`, `:279` | **Parser próprio:** escolhe O/F por texto e coluna — dinheiro, só registrar, Ronald/aval/corte/smoke, livre e fechado. | Continua lendo prosa e escolhendo “item da ORDEM” se não migrar. O contrato precisa delimitar essa pergunta. |
| `bin/fabricante_alvo.py:315`, `:320`; `bin/fabricante.sh:129` | Tenta livres e depois ORDEM escalonada; fabricante chama o seletor. | Leitores indiretos do seletor anterior. |
| `bin/gerar_backlog_topo.py:16`, `:26`, `:30`, `:39` | Produz resumo de O-números abertos, por frases e ordenação numérica. | É pergunta diferente da ORDEM VIVA; não pode passar a ser consumido como fila 1. |
| `bin/relato.sh:35`, `:115`, `:116` | Gera o resumo e publica BACKLOG completo. | Transporta a publicação derivada. Seu comentário ainda declara que a ORDEM se lê na tabela completa (`:7`). |
| `.claude/settings.json:9`, `:20`, `:29`, `:40`, `:53`, `:68` | Stop chama hook; PreCompact chama handoff; SessionStart lê handoff; PreToolUse chama disjuntor. | São entradas/leitores indiretos. Apontam para `/home/ronald/saas-hasner`; execução desta cópia nesse caminho é [nao verificado]. |

Os selos também fazem parte da migração:

| Selo em `bin/tests/` | Pergunta/fixture atual e efeito |
|---|---|
| `test_hook_andavel_e_a_ordem_viva.sh:34`, `:51`, `:73` | Escreve marcador, ordem em prosa, estados, POS-20 e ruído; exige liberar com restante travado e bloquear com restante aberto. Precisa representar os dois casos na fonte estruturada. |
| `test_hook_parei_com_andavel.sh:25`, `:42`, `:56` | Alterna restante aberto/travado pela coluna para exigir bloquear/liberar. |
| `test_hook_parei_trava_o_item_em_curso.sh:20`, `:40`, `:53` | Distingue PAREI do topo de PAREI de outro item; fixtures precisam migrar. |
| `test_hook_parei_casa_id_inteiro.sh:21`, `:42`, `:57` | Escreve marcador e congeladas; preserva a pergunta de identidade inteira, mudando a fonte do congelamento. |
| `test_hook_le_o_marcador.sh:39`, `:56`, `:74`, `:92` | Exige autoridade/falhas do marcador e vazio pela tabela. Essas expectativas precisam virar autoridade/falhas/vazio da fonte estruturada. |
| `test_hook_estado_e_a_coluna.sh:41`, `:51`, `:59`, `:80` | Exige estado/tachado como veto do topo e examina BACKLOG real. Essa expectativa sobre a ORDEM conflita com o aval novo. |
| `test_hook_nao_cobra_congelado.sh:12`, `:111`, `:115`, `:135`, `:168` | Lê o predicado de coluna, valida marcador real e escreve fixtures. Exigir marcador como fonte deixa de valer. |
| `test_interrompidas_estado.sh:37`, `:60`, `:67`, `:82` | Testa topo/fusão/fechamento e ausência de escrita. Migrar topo, mantendo a pergunta específica de interrupção. |
| `test_censo_antes_da_vaga.sh:36`, `:112`, `:125`, `:568`, `:694` | Testa pertencimento pela tabela, fechada fora, fonte ausente e ambiguidades; inclui mutante dependente dessa leitura. Fixtures, expectativas e mutante precisam migrar. |
| `test_handoff_sessao.sh:124`, `:130`, `:151` | Proíbe parser próprio do marcador/OBRAS e usa fixture de tabela. A guarda precisa alcançar a fonte nova; a fixture precisa fornecê-la. |
| `test_fabricante_le_a_ordem.sh:25`, `:34`, `:65` | Recalcula quem espera Ronald pela coluna e confronta o fabricante; continua leitor da fonte antiga se mantido. |
| `test_fabricante_backlog.sh:22`, `:35`, `:39` | Exige livres/expansão escalonada e reconhece ORDEM pelo texto. Depende da delimitação do fabricante. |
| `test_alarme_vigia_a_sessao.sh:62` | Exige textualmente que a fila venha de BACKLOG; essa guarda precisa acompanhar a troca. |
| `test_orcamento_contexto.sh:40`, `:165`, `:451` | Cria marcador e altera coluna para simular fila vazia. Migrar a representação do vazio. |
| `test_disjuntor.sh:59` | Cria marcador/tabela para hook e disjuntor. Precisa da fonte estruturada na fixture. |
| `test_hook_nao_para_por_processo.sh:36`, `:65` | Simula fila viva/vazia com marcador e fechado; preservar respostas com fixtures novas. |
| `test_hook_parei_so_lei.sh:36`, `:48`, `:68` | Simula lei, técnico, `!` e só registro pela tabela; migrar a fonte dessas condições. |
| `test_hook_teto_nao_conta_espera.sh:40`, `:69` | Fixture de marcador para testar teto/espera. Muda a fixture, não a pergunta do teto. |
| `test_hook_esperando_com_prova.sh:40`, `:65`, `:79` | Fixture para prova de espera e `siga`; precisa da nova fonte. |
| `test_hook_stop_saida_valida.sh:20`, `:45` | Fixture para schema e silêncio; precisa da nova fonte. |
| `test_relato_prova_no_stop.sh:16`, `:23` | Fixture para chegar à guarda de prova; precisa da nova fonte. |

Em `app/`, os outros leitores encontrados de BACKLOG perguntam **existência do item** e **padrão declarado da obra**, não ordem/andabilidade: `app/core/tests/test_marca_cron15.py:89` e `app/chamados/tests/e4b_c4_verificar.py:209`. A mesma distinção vale para `bin/tests/test_prompt_virou_item.sh:25`.

Há declarações documentais da autoridade antiga que precisam receber a emenda do instrumento: marcador manual em `app/docs/LEIS.md:140`, `app/docs/CORTES.json:736` e `app/docs/BACKLOG.md:1574`. O handoff salvo ainda publica `MANHA-0511`, enquanto o marcador atual publica `ORDEM-POS-E4A2`; não serve como prova da ordem atual. (`app/docs/HANDOFF-SESSAO.md:19`; `app/docs/BACKLOG.md:8`)

**2. Cobertura literal do aval**

| Exigência | Veredito sobre v2 |
|---|---|
| Fonte única estruturada em arquivo próprio | **Cobre a forma:** três seções no mesmo arquivo, com um escritor declarado. (`contrato_item3_v2.txt:2`) |
| `gerar_backlog` publica; hook lê | **Cobre a intenção.** “Renderiza no BACKLOG, sem escrever nele” precisa identificar que o arquivo intocável é o `.ids`; literalmente, o antecedente fica ambíguo. (`contrato_item3_v2.txt:2`) |
| Marcador e coluna deixam de ser fonte | **Não fecha a implementação:** nomeia duas funções, mas deixa sem especificação os demais caminhos do censo acima. (`contrato_item3_v2.txt:3`; `bin/hook_stop_fila1.py:225`, `:267`, `:344`, `:380`, `:1298`) |
| Universo = fila 1 não pousada | **Não resolve a semântica:** transforma todos os restantes de ORDEM em andáveis. O RED exige distinguir restante travado de restante aberto. (`contrato_item3_v2.txt:3`; `bin/tests/test_hook_andavel_e_a_ordem_viva.sh:51`, `:73`) |
| Exclusões POS-20 e pousadas, pelo id | **Representa as seções**, mas falta a semente com identidade resolvida e a regra operacional de exclusão. A interdição de repetição entre seções cobre conflito, não resolve aliases. (`contrato_item3_v2.txt:2`, `:5`; `app/docs/BACKLOG.md:2031`, `:1965`) |
| RED ca320b69f permanece | **Diz que permanece, mas contradiz seu comportamento.** (`contrato_item3_v2.txt:6`; `bin/tests/test_hook_andavel_e_a_ordem_viva.sh:59`, `:72`) |

O defeito central é concreto: **ORDEM com três ids, dois restantes andáveis e PAREI do em curso liberando** não preserva a regra guardada. Com outro andável, o RED exige **não devolver o turno**. Não é questão de confirmar uma hipótese do Code: essa cláusula do contrato está errada. (`contrato_item3_v2.txt:6`; `bin/tests/test_hook_andavel_e_a_ordem_viva.sh:75`; `bin/hook_stop_fila1.py:1245`)

Além disso, uma lista apenas de ids não registra a diferença entre os dois estados que o RED exercita. O contrato precisa explicar essa diferença dentro da fonte estruturada, sem reintroduzir coluna, marcador ou adendo como juiz. A representação dessa condição está ausente do v2. (`contrato_item3_v2.txt:2`, `:3`; `bin/tests/test_hook_andavel_e_a_ordem_viva.sh:51`, `:73`)

Também faltam: vazio válido; repetição **dentro** da mesma seção; gramática/seções inválidas; identidade canônica; e separação entre falha esperada da fonte e exceção que alcança a falha aberta. O v2 enumera ausência/ilegibilidade/repetição entre seções/id sem linha, mas não esses casos. (`contrato_item3_v2.txt:5`; `bin/disjuntor.py:1736`; `bin/hook_stop_fila1.py:1460`)

A promessa de handoff/disjuntor “não mudarem de resposta” sobra como garantia geral: a correção pretende justamente mudar pertencimento/contagem e, por consequência, devolução do turno e exigência de censo. (`contrato_item3_v2.txt:4`; `bin/disjuntor.py:1682`, `:1755`; `bin/hook_stop_fila1.py:1245`)

**Sim, sobrariam leitores de prosa** se a migração seguir apenas os nomes explícitos do §1: veto de PAREI, lista travada, alarme e fabricante; os recados também continuariam mandando o humano ao adendo. (`bin/hook_stop_fila1.py:1298`, `:380`, `:285`; `bin/alarme_sessao_ociosa.py:113`; `bin/fabricante_alvo.py:273`; `bin/handoff_sessao.sh:314`)

**3. Riscos do publicador e ordem de migração** [nao verificado]

- **Reintroduzir saída velha:** a preservação começa no marcador antigo. Acrescentar publicação nova sem retirar a antiga dessa janela pode duplicar marcador/lista; o hook atual usa a primeira ocorrência encontrada. (`bin/gerar_backlog.py:119`, `:127`; `bin/hook_stop_fila1.py:209`)
- **Perder adendos:** preservar apenas `OBRAS:INICIO` em diante descartaria a prosa entre o marcador e o início do bloco, atualmente incluída na janela. (`app/docs/BACKLOG.md:8`, `:712`; `bin/gerar_backlog.py:119`)
- **Tripwire inadequado:** remover o marcador pode parar a geração; conservar qualquer ocorrência textual satisfaz uma guarda que não verifica valor nem derivação. (`bin/gerar_backlog.py:131`)
- **Perder a cauda manual atual:** OBRAS termina em `:2038`, mas META-18 e outras linhas estão em `:2392` em diante. O gerador preserva apenas a janela terminada em OBRAS:FIM e recompõe o restante. (`app/docs/BACKLOG.md:2038`, `:2392`, `:2401`; `bin/gerar_backlog.py:100`, `:119`, `:138`)
- **Declarar falsa fila vazia:** ausência/erro da fonte não pode virar lista vazia nem escapar pelo tratamento genérico que libera o hook. (`contrato_item3_v2.txt:5`; `bin/hook_stop_fila1.py:1400`, `:1460`)
- **Fixture antiga impedir ou falsear a migração:** os selos de marcador/coluna precisam mudar junto com os leitores, preservando as propriedades do RED. (`bin/tests/test_hook_le_o_marcador.sh:56`; `bin/tests/test_hook_estado_e_a_coluna.sh:51`; `bin/tests/test_hook_andavel_e_a_ordem_viva.sh:59`)

A sequência recomendada, sem dois juízes ativos, é:

1. **Corrigir o contrato antes do código:** resolver universo versus andabilidade, cláusula do PAREI, identidade dos ids e preservação da cauda. São incompatibilidades já demonstradas, não decisões para o código adivinhar. (`contrato_item3_v2.txt:3`, `:6`; `app/docs/BACKLOG.md:2038`, `:2401`)
2. **Preparar uma única alteração completa:** fonte semeada, leitor/validador compartilhado, todos os consumidores do censo, publicador, fixtures e emenda documental. Nenhum consumidor deve consultar a fonte antiga como fallback. (`aval_respostas_1211_1230.txt:4`; `contrato_item3_v2.txt:3`, `:7`) [nao verificado]
3. **No publicador, separar explicitamente preservação e geração:** conservar os trechos manuais necessários; substituir a publicação antiga; se houver marcador, gerar exatamente um a partir da fonte. Verificar igualdade da publicação e estabilidade de duas gerações, além da preservação manual. (`bin/gerar_backlog.py:119`, `:131`; `contrato_item3_v2.txt:6`)
4. **Ativar o conjunto no mesmo corte**, sem leitores antigos em execução atravessando a troca; confirmar o caminho efetivamente chamado pela configuração. Um commit único não demonstra sozinho qual cópia está em execução. (`contrato_item3_v2.txt:3`; `.claude/settings.json:9`, `:68`)
5. **Reverter o conjunto junto**, sem deixar seletor de compatibilidade marcador/arquivo. (`contrato_item3_v2.txt:7`)

Recontando **em memória**, pela lógica desta cópia: são **942** itens em `_itens_andaveis` e **941 além do em curso**. Portanto o “antes 942 / depois len(ORDEM)−1” do contrato compara total com restantes; precisa usar a mesma medida nos dois lados. O número pós-migração ainda é [nao verificado], pois depende da semente. (`bin/hook_stop_fila1.py:231`, `:270`; `app/docs/BACKLOG.md:8`, `:1758`; `contrato_item3_v2.txt:6`)

**4. Lista inicial e ambiguidades**

**Não há uma lista linear única que possa ser transcrita como semente sem resolver contradições.** O marcador aponta uma ordem que recebeu substituições posteriores, inclusive duas raias. (`app/docs/BACKLOG.md:8`, `:1758`, `:1759`, `:1880`, `:2401`)

Estas são as sequências literais relevantes, na ordem escrita em cada fonte:

| Fonte | Sequência nomeada |
|---|---|
| Adendo acima do marcador, histórico | O-OUTBOX → CONTRATO-20-E2-REBAIXA → funções de lastro/órfãs → disparar_perguntas_competencia → PENDENCIA-SEM-JUIZ-3/SILENCIO-VENCIDO-VOLTA → JUSTIFICATIVA-PAGA-O-PERIODO. Mistura ids e funções. (`app/docs/BACKLOG.md:7`) |
| **ORDEM-POS-E4A2**, apontada pelo marcador | BO-0611-05 → BO-DISPUTA-SEM-PERGUNTA → E4b → E4c → E4d → BUG-PAR-FROTA-1110 → DISPUTA-ZUMBI-1110 → O169/O168/LASTRO. (`app/docs/BACKLOG.md:8`, `:1758`) |
| **PLANO-JANELA-POS-RESET** | AUDITORIA-DE-TRAVAS primeiro; depois O169-FECHADA/O168/LASTRO → RAIZ F0/F1, F2=BO-DISPUTA, F3/F4 absorvendo ZUMBI/PAR → E4b/E4c/E4d → MAPA-DA-FILA-BO. Substitui a ordem anterior do E4b em diante. (`app/docs/BACKLOG.md:1759`) |
| **ORDEM-ATE-O-ZERO** | c21 BO → E3 inteira → E4b inteira → E4c → E4d → O169/O168/LASTRO; AUDITORIA permanece antes. (`app/docs/BACKLOG.md:1880`) |
| **META-18-DUAS-RAIAS** | Série E + RAIZ F1–F7 + O169/O168/LASTRO; muda posição da RAIZ/item 5 e inicia raia 2 com O169-FECHADA v5. Não fornece intercalação linear completa. (`app/docs/BACKLOG.md:2401`) |
| **RAIA-2-BO-VISIVEL** | Após O169 v5: BO-ROTULO-ENCERRADO → TRIAGEM-OFFLINE-SEM-CAMERA → LOTE-VALIDAR-REGRESSAO-0311. (`app/docs/BACKLOG.md:1903`) |
| Inserção posterior na raia 2 | APP-MENSAGENS-MSG-0 depois de TRIAGEM-OFFLINE e antes de O168/LASTRO; posição relativa ao LOTE não explicitada. (`app/docs/BACKLOG.md

[truncado em 20000 de 23679 caracteres]

## 12/11 13:30 · sessao 7bbc60e1c1e5 · - · - · origem=Ronald (tmux)

Censo de `bin/tests/` e `app/`, só leitura; não executei selos, pois eles criam fixtures (`bin/tests/test_hook_andavel_e_a_ordem_viva.sh:20`, `:42`).

**Ponto que invalida o v2 como está:** o RED mantido exige duas respostas: `ITEM-A` na ordem, mas esperando `!`, não impede liberar o PAREI do em curso; o mesmo `ITEM-A` aberto impede. O contrato “andáveis = IDs de ORDEM menos em curso” perde essa distinção se ORDEM significa o universo “fila 1 não pousada”. O selo mostra literalmente os dois estados em `bin/tests/test_hook_andavel_e_a_ordem_viva.sh:51`, `:60`, `:66`, `:73`, `:75`, `:78`. Contar 2 restantes e simultaneamente exigir que o PAREI do em curso libere também contradiz o comportamento guardado por `bin/tests/test_hook_parei_com_andavel.sh:45`, `:55`. A especificação precisa resolver a distinção pertença/andabilidade sem voltar a usar prosa/coluna como fonte; como fazê-lo não está definido pelo anexo [nao verificado].

| Sítio | O que pergunta / efeito da migração |
|---|---|
| `bin/tests/test_hook_andavel_e_a_ordem_viva.sh:34`, `:51`, `:73` | Escreve marcador, ordem em prosa, estados, POS-20 e 30 ruídos; pergunta ao hook sobre topo/andáveis/PAREI. Precisa fixture estruturada preservando ambos os lados do RED. |
| `bin/tests/test_hook_parei_com_andavel.sh:25`, `:42`, `:46`, `:56` | Escreve marcador/tabela; usa coluna para alternar restante livre/`!` e exigir bloquear/liberar. Muda se o v2 só contar IDs. |
| `bin/tests/test_hook_parei_trava_o_item_em_curso.sh:20`, `:40`, `:53`, `:61` | Marcador O261, outro item; PAREI de outro item não libera; PAREI do topo libera quando outro está em `!`. Fixture/alteração de coluna precisam migrar. |
| `bin/tests/test_hook_parei_casa_id_inteiro.sh:21`, `:42`, `:57`, `:64` | Escreve marcador; classifica O1/O14 congeladas e O142 vivo para testar ID inteiro e PAREI. Fixture/fonte de congelamento precisam migrar. |
| `bin/tests/test_hook_le_o_marcador.sh:39`, `:56`, `:64`, `:74`, `:81`, `:88` | Exige autoridade do marcador e suas falhas; fila vazia é decidida pela tabela. Precisa virar autoridade/falhas do `.ids`, incluindo vazio explícito. |
| `bin/tests/test_hook_estado_e_a_coluna.sh:41`, `:51`, `:59`, `:80`, `:94` | Exige que estado/tachado definam topo parado; varre BACKLOG real com padrões do hook. A expectativa sobre ordem conflita com coluna deixar de ser fonte. |
| `bin/tests/test_hook_nao_cobra_congelado.sh:12`, `:25`, `:32`, `:111`, `:115`, `:135`, `:168` | Raspa `_NAO_ANDA` do código, valida coluna; pergunta ao hook pelo marcador real e sintético; exige falha sem marcador. Precisa rever tanto autoridade quanto fixtures. |
| `bin/tests/test_interrompidas_estado.sh:37`, `:44`, `:60`, `:67`, `:82` | Escreve marcador e estado, testa topo/fundida/fechada e ausência de escrita. Deve migrar topo para `.ids`, preservar a pergunta específica de interrupção. |
| `bin/tests/test_censo_antes_da_vaga.sh:36`, `:112`, `:125`, `:131`, `:568`, `:579`, `:694` | Escreve marcador e estados; exige pertencer à fila por tabela, exclusão fechada, “não sei” sem BACKLOG/bloco, ambiguidade de IDs; mutante depende da leitura de BACKLOG. Migração alcança fixtures, asserts e mutante. |
| `bin/tests/test_handoff_sessao.sh:124`, `:125`, `:130`, `:151` | Guarda exige importar hook e proíbe parser próprio OBRAS/marcador. A guarda permanece útil, mas precisa cobrir eventual parser próprio `.ids`; a fixture só tabela não fornece a nova fonte. |
| `bin/tests/test_fabricante_le_a_ordem.sh:25`, `:30`, `:34`, `:65`, `:70` | Lê estados reais para calcular quem espera Ronald e confronta fabricante; escreve casos espera/livre. Continua leitor de coluna se não migrar. |
| `bin/tests/test_fabricante_backlog.sh:22`, `:29`, `:35`, `:39` | Consome fabricante, exige itens livres e expansão `escalonar=True`, reconhece ordem pelo texto “ORDEM”. Deve rever se pertence ao escopo fila 1; essa delimitação não aparece no contrato [nao verificado]. |
| `bin/tests/test_alarme_vigia_a_sessao.sh:62` | Guarda textual exige que alarme use `docs/BACKLOG.md`; pode exigir autoridade antiga depois da migração. |
| `bin/tests/test_orcamento_contexto.sh:40`, `:165`, `:451` | Escreve marcador/tabela e muda coluna para simular fila vazia; precisa representar vazio no `.ids`. |
| `bin/tests/test_disjuntor.sh:59` | Escreve marcador/tabela ITEM-VIVO para exercitar hook/disjuntor; precisa `.ids` na fixture. |
| `bin/tests/test_hook_nao_para_por_processo.sh:36`, `:47`, `:65`, `:74` | Escreve marcador para fila viva e só fechado para vazia; bloqueio/liberação devem preservar resposta com nova fixture. |
| `bin/tests/test_hook_parei_so_lei.sh:36`, `:40`, `:48`, `:59`, `:68` | Escreve marcador; testa PAREI técnico/lei/`!` e só-registro por coluna. Precisa preservar perguntas com fonte estruturada. |
| `bin/tests/test_hook_teto_nao_conta_espera.sh:40`, `:52`, `:69` | Escreve marcador/tabela para exercitar teto com/sem espera. Fonte muda; resposta pretendida não. |
| `bin/tests/test_hook_esperando_com_prova.sh:40`, `:65`, `:67`, `:79` | Escreve marcador/tabela e exige liberação com prova ou `siga: ITEM-VIVO` sem prova. Fonte muda; resposta pretendida não. |
| `bin/tests/test_hook_stop_saida_valida.sh:20`, `:27`, `:45` | Escreve marcador/tabela; testa schema de bloqueio e silêncio com PAREI. Fonte muda; resposta pretendida não. |
| `bin/tests/test_relato_prova_no_stop.sh:16`, `:23`, `:28` | Escreve marcador/tabela para chegar ao guard de prova. Fonte muda; checagem de prova não deveria mudar. |

Outros leitores reais de BACKLOG encontrados em `app/` e `bin/tests/` não respondem à pergunta auditada: existência do item (`app/core/tests/test_marca_cron15.py:89`, `bin/tests/test_prompt_virou_item.sh:25`) e texto do padrão da obra (`app/chamados/tests/e4b_c4_verificar.py:209`). Não classificá-los como leitores da ORDEM VIVA.

Documentos que ainda declaram a autoridade antiga e precisam reconciliar no marco: `app/docs/LEIS.md:140` diz marcador escrito à mão; `app/docs/CORTES.json:736` idem; `app/docs/BACKLOG.md:707` chama marcador de primeiro aberto único; `app/docs/BACKLOG.md:1574` diz que medidor lê marcador e ele continua manual. `app/docs/HANDOFF-SESSAO.md:19` publica topo “MANHA-0511”, enquanto o marcador real está em “ORDEM-POS-E4A2” (`app/docs/BACKLOG.md:8`); é saída envelhecida, não prova da ordem atual.

## 12/11 13:30 · sessao a3d3702630bf · - · - · origem=Ronald (tmux)

Censo `bin/` e `.claude/`, exceto testes: [nao verificado]

| Sítio | Pergunta/ato atual | Efeito da fonte nova |
|---|---|---|
| `bin/gerar_backlog.py:110-127` | Lê e recoloca janela manual começando no marcador, incluindo toda prosa entre marcador e OBRAS. | Tem de separar saída derivada da janela preservada; senão recoloca marcador/lista velhos e pode duplicar saída. |
| `bin/gerar_backlog.py:129-140` | Tripwire exige que marcador e OBRAS presentes antes continuem no resultado; escreve BACKLOG por `.novo` + replace. | Se remover marcador, tripwire impede geração; se conservar como derivado, presença sozinha não verifica valor/fonte nova. |
| `bin/hook_stop_fila1.py:149-180` | `_item_fechado`: FECHADO/NO AR na coluna estado ou tachado na descrição; `_item_parado`: fechado ou regex de CONGELADA/FILA 2/espera/PAREI/portão. | Precisam deixar de julgar pertinência/andabilidade por estado; podem permanecer para perguntas documentais distintas. |
| `bin/hook_stop_fila1.py:192-228` | `_item_da_ordem_viva`: extrai marcador, exige linha correspondente em OBRAS, veta `_item_parado`. | Nova origem do topo; manter veto de coluna contradiz “coluna deixa de ser fonte”. |
| `bin/hook_stop_fila1.py:231-267` | `_itens_andaveis`: todos os ids não parados de OBRAS, ordem física; `_ha_item_aberto`: bool dessa lista. | Devem passar à fonte estruturada. Contrato só mudar `_andaveis_fora...` não basta: vazio continua julgado pela tabela. |
| `bin/hook_stop_fila1.py:270-287` | Conta andáveis além do topo; recado manda humano pegar próximo na “ORDEM VIVA (adendo do BACKLOG)”. | Lista deve vir da fonte única; recado ainda encaminharia leitura humana da prosa se não mudar. |
| `bin/hook_stop_fila1.py:290-322` | Diagnóstico de marcador quebrado; `_proximo_da_fila` delega topo. | Diagnóstico muda para fonte quebrada; topo pode conservar resposta se primeiro id for o mesmo. |
| `bin/hook_stop_fila1.py:323-346` | `_ids_que_nao_andam`: lista por estado de OBRAS. | Continua segundo juiz se não migrar; é usada para rejeitar PAREI em `bin/hook_stop_fila1.py:1298-1302`. |
| `bin/hook_stop_fila1.py:349-383` | `travas_da_fila`: varre OBRAS, exclui fechadas, mostra palavra de trava encontrada na coluna. | Se usada como “fila inteira travada”, continua juiz por prosa; contrato não a menciona. |
| `bin/hook_stop_fila1.py:1232-1245` | `_relato_parou`: PAREI do topo + nenhum outro andável. | Resposta muda quando tabela tem ruído fora da ORDEM. |
| `bin/hook_stop_fila1.py:1248-1313` | `_parei_do_em_curso`: PAREI `!`, elimina parados por coluna, compara id com topo. | Precisa retirar veto antigo por coluna e usar a mesma identidade estruturada. |
| `bin/hook_stop_fila1.py:1391-1403` | Main escolhe topo; sem topo consulta tabela para liberar “fila vazia” ou reclamar marcador. | Deve validar fonte antes de distinguir fila vazia/quebrada; manter tabela permite resposta divergente. |
| `bin/handoff_sessao.sh:54-78` | Lê BACKLOG, pergunta topo e próximo ao hook, publica ambos. | Herda fonte nova se API migrar; rótulo “MARCADOR” fica velho. |
| `bin/handoff_sessao.sh:120-123` | Pergunta “trava item?” e “devolve turno?” ao hook. | Herda mudanças do censo de andáveis/PAREI. |
| `bin/handoff_sessao.sh:163-174` | Para avais de hoje, procura linha BACKLOG e chama `_item_fechado` para dizer não cumprido. | Pergunta diferente de ordem, mas continua lendo estado/prosa de fechamento. Não pode servir de exclusão de pousadas da ORDEM. |
| `bin/handoff_sessao.sh:209-219` | Lista linhas abertas da raia20 usando `_item_fechado`. | Pergunta documental distinta; continua lendo coluna, portanto declarar exceção de escopo. |
| `bin/handoff_sessao.sh:290-299` | Sem `_proximo_da_fila`, publica `travas_da_fila` e afirma “nenhum item livre: turno se devolve”. | Perigoso manter: topo inválido não implica fila vazia nem inteira travada. |
| `bin/handoff_sessao.sh:309-316` | Publica próximo e diz “1o ABERTO do bloco OBRAS”; na ausência manda conferir ORDEM VIVA à mão. | Texto precisa migrar; preservá-lo manda humano ler a prosa velha. |
| `bin/disjuntor.py:1671-1682` | `_fila1_viva`: lê BACKLOG, exige marcador OBRAS, retorna `_itens_andaveis`. | Tem de ler universo estruturado; tabela aberta inteira deixa de exigir censo. Promessa contratual “não muda resposta” é falsa para itens abertos fora da ORDEM. |
| `bin/disjuntor.py:1720-1743` | Pertinência por token da célula e coincidência inteira nos dois sentidos, com ambiguidades. | Deve definir identidade do novo arquivo: ids exatos ou token qualificado? Contrato não resolve. |
| `bin/disjuntor.py:1746-1759` | Censo exigido só para obra pertencente à fila viva. | Herda troca do universo. |
| `bin/disjuntor.py:2355-2364` | CLI publica FORA ou aplica mesmo censo. | Herda troca; fora da lista nova muda resposta. |
| `bin/codex_pergunta.sh:228-234` | Invoca `disjuntor.py --censo` antes da vaga, usando raiz viva. | Leitor indireto da pertinência; não duplicar parser. |
| `bin/backlog_estado.py:56-68` | Antes/depois de escrever estado, consulta próximo e ids que não andam; imprime veredito alterado. | Deve deixar de sugerir que escrever coluna muda a ORDEM; hoje o código escreve e só informa a mudança, apesar da docstring prometer “PARA” em `bin/backlog_estado.py:8-10`. |
| `bin/interrompidas_estado.py:89-125` | Lê coluna INTERROMPIDA/FUNDIDA e topo importado; diz “voltou” quando id/fusão coincide com topo ou `_item_fechado`. | Topo migra indiretamente; fechamento documental permanece por coluna. Não usar este fechamento como exclusão estruturada. |
| `bin/gerar_estado.py:236-250` | Publica medição de interrompidas e topo importado. | Leitor indireto; texto “sem marcador” em `:243` precisa mudar. |
| `bin/alarme_sessao_ociosa.py:89-116` | **Leitor próprio esquecido:** conta itens em `## OBRAS`, excluindo ☑/FEITO/FECHAD da linha inteira. | Precisa migrar. Hoje decide fila vazia/disparo em `:40-45`, chamado por `:148-150`; mantendo-o, haverá dois juízes. |
| `bin/placar_code.sh:368-369` | Publica saída do alarme. | Leitor indireto; herda mudança. |
| `bin/fabricante_alvo.py:238-283` | **Leitor próprio esquecido:** lê linhas O/F, usa texto/estado (dinheiro, só registrar, Ronald/aval/corte/smoke/suspensão, livre, no ar/feito/fechado), escolhe itens da “ORDEM escalonada”. | Continua lendo prosa se não migrar; definição do universo do fabricante precisa ficar explícita. |
| `bin/fabricante_alvo.py:315-320` | Tenta itens livres, depois ORDEM escalonada. | Herda seletor anterior. |
| `bin/fabricante.sh:129` | Invoca seletor de alvo. | Leitor indireto. |
| `bin/gerar_backlog_topo.py:16-40` | Lista O-números “abertos”, decide FEITO pela descrição/onde e estado por frases; ordena numericamente. | É resumo distinto, não fila1/ORDEM; se permanecer, rotulá-lo e não consumi-lo como fonte de fila. |
| `bin/relato.sh:35`, `bin/relato.sh:115-121` | Gera resumo e publica BACKLOG inteiro. | Transporta saída derivada; não decide topo. Comentário em `bin/relato.sh:7-8` ainda afirma que ORDEM só se lê na tabela completa. |
| `.claude/settings.json:9` | Stop chama hook. | Herda fonte nova; configuração aponta caminho absoluto `/home/ronald/saas-hasner`, portanto execução dessa cópia em produção é [nao verificado]. |
| `.claude/settings.json:20`, `:29`, `:40`, `:53`, `:68` | PreCompact chama handoff; SessionStart lê handoff; PreToolUse chama disjuntor. | Leitores indiretos dos juízes acima. |

Contradições contratuais centrais:

- Mudar só duas funções deixa cinco caminhos ativos lendo tabela/estado: `_itens_andaveis`, `_ha_item_aberto`, `_ids_que_nao_andam`, `travas_da_fila` e veto `_item_parado` dentro do topo; referências em `bin/hook_stop_fila1.py:225`, `:250`, `:267`, `:344`, `:378-382`, `:1298-1302`, `:1400`.
- “Handoff e disjuntor mudam só fonte, não resposta” não pode ser literal: o bug consiste justamente em contar abertos fora da ORDEM, e disjuntor usa essa lista em `bin/disjuntor.py:1682`; PAREI depende dela em `bin/hook_stop_fila1.py:1245`.
- Não basta parar o parser de marcador: orientação ao humano ainda manda ler adendo/prosa em `bin/hook_stop_fila1.py:285-287` e `bin/handoff_sessao.sh:314-316`.
- Tripwire atual só verifica presença; necessidade de verificar igualdade da publicação derivada ao `.ids` e impedir prosa velha duplicada decorre de `bin/gerar_backlog.py:119-137`.

Não executei scripts auditados: possuem escritas explícitas (`bin/gerar_backlog.py:138-140`, `bin/backlog_estado.py:59`, `bin/hook_stop_fila1.py:51-54`, `bin/relato.sh:121-131`).

## 12/11 13:30 · sessao e4ace4e8c489 · - · - · origem=Ronald (tmux)

Não há lista linear única que eu possa transcrever como “fila 1 não pousada atual” sem resolver contradições e aliases por conta própria. O marcador aponta `ORDEM-POS-E4A2`; a própria cópia registra várias substituições posteriores dessa ordem. (`app/docs/BACKLOG.md:8`, `app/docs/BACKLOG.md:1758`, `app/docs/BACKLOG.md:1759`, `app/docs/BACKLOG.md:1880`, `app/docs/BACKLOG.md:2401`)

**Listas literais por camada, preservando a ordem escrita:**

| Fonte | Sequência literal |
|---|---|
| Topo, ainda anterior ao marcador | `O-OUTBOX` → `CONTRATO-20-E2-REBAIXA` → `fechar_cobranca_com_lastro` e `reconciliar_perguntas_orfas` → `disparar_perguntas_competencia` → `PENDENCIA-SEM-JUIZ-3` e `SILENCIO-VENCIDO-VOLTA` → `JUSTIFICATIVA-PAGA-O-PERIODO`; mistura ids e nomes de funções. (`app/docs/BACKLOG.md:7`) |
| Item apontado pelo marcador | `BO-0611-05` → `BO-DISPUTA-SEM-PERGUNTA` → E4b BACKFILL → E4c LEITORES → E4d CONTRACT → `BUG-PAR-FROTA-1110` → `DISPUTA-ZUMBI-1110` → O169/O168/LASTRO. (`app/docs/BACKLOG.md:1758`) |
| Substituição do E4b em diante, vigente a partir de 11/11 18h segundo o texto | Primeiro `AUDITORIA-DE-TRAVAS`; depois (a) `O169-FECHADA`, O168, LASTRO; (b) `RAIZ-DA-DISPUTA` F0/F1, F2=BO-DISPUTA, F3/F4 absorvem DISPUTA-ZUMBI e BUG-PAR; (c) E4b/E4c/E4d; (d) itens do `MAPA-DA-FILA-BO`. (`app/docs/BACKLOG.md:1759`) |
| Substituição do item (3) anterior | c21 BO → E3 inteira → E4b inteira → E4c → E4d → O169/O168/LASTRO; AUDITORIA-DE-TRAVAS permanece antes. (`app/docs/BACKLOG.md:1880`) |
| Emenda posterior | Série E + RAIZ F1–F7 + O169/O168/LASTRO; duas raias, com raia 2 iniciando em `O169-FECHADA` v5. Diz substituir posição da RAIZ e item 5, mas não dá nessa linha intercalação linear de todas as etapas. (`app/docs/BACKLOG.md:2401`) |
| Raia 2 subsequente | Depois do pouso O169 v5: `BO-ROTULO-ENCERRADO` → `TRIAGEM-OFFLINE-SEM-CAMERA` → `LOTE-VALIDAR-REGRESSAO-0311`. (`app/docs/BACKLOG.md:1903`) |
| Inserção na raia 2 | `APP-MENSAGENS-MSG-0` depois de TRIAGEM-OFFLINE e antes de O168/LASTRO; posição relativa ao LOTE-VALIDAR não explicitada. (`app/docs/BACKLOG.md:1913`) |
| Inserção na RAIZ | `RAIZ-DA-DISPUTA-F0-CASO-7279` à frente da F1 e de HJ-067/HJ-073. (`app/docs/BACKLOG.md:2406`) |
| Lista fechada do 20 | O168, LASTRO, E3-LEITORES, E4B, E4C, E4D; a linha diz “lista fechada”, não declara que esta enumeração é ordem de execução. O169 é expressamente “pousada”. (`app/docs/BACKLOG.md:2031`) |

**Exclusões/pousos com prova textual, sem inferir por nome ou prefixo:**

- `FIO-E0-V4-E4B`: a própria linha diz **POUSOU 12/11 08:46**, commit `087fe758a`; permanece no rol fechado do 20 sob alias E4B. Portanto não pode ser carregada como “não pousada” só porque aparece nesse rol. (`app/docs/BACKLOG.md:1891`, `app/docs/BACKLOG.md:2031`)
- `C20-O169`: **POUSADA 05/11**, FECHADA. O169 é dita pousada no rol do 20. (`app/docs/BACKLOG.md:1796`, `app/docs/BACKLOG.md:2031`)
- **Ambiguidade O169:** `O169-FECHADA` diz “já POUSADO 05/11 pela C20-O169”, mas separa delta do contador e ainda descreve B3/volta 5; não autoriza excluir automaticamente esse delta pelo pouso de `C20-O169`. (`app/docs/BACKLOG.md:1965`)
- `FIO-E0-V4-E4A-2`: NO AR em 10/11, merge `613a51498`. (`app/docs/BACKLOG.md:1890`)
- `FIO-E0-V4-E2-FUNCAO`: NO AR em 09/11, comboio com “E3”; isso não prova o pouso da E3-LEITORES posterior, cuja linha continua ABERTA/em obra. (`app/docs/BACKLOG.md:1869`, `app/docs/BACKLOG.md:1870`)
- `O-OUTBOX` e `CONTRATO-20-LASTRO-E-ORFAS`, ainda citados na lista do topo, estão respectivamente NO AR e FECHADA em suas linhas. (`app/docs/BACKLOG.md:733`, `app/docs/BACKLOG.md:735`)

**Ids explicitamente classificados POS-20 nas suas linhas:**

| id | Linha |
|---|---:|
| TRIAGEM-FILA-OPUS-V6 | `app/docs/BACKLOG.md:1812` |
| E4B-FILA-OPUS-C14 | `app/docs/BACKLOG.md:1893` |
| CORTES-JSON-DEFASADO | `app/docs/BACKLOG.md:2030` |
| CICLO-C-CEL-TEMPLATE | `app/docs/BACKLOG.md:2032` |
| FAIXA-NO-CADASTRO | `app/docs/BACKLOG.md:2033` |
| EFEITO-SEM-FALLBACK-CATALOGO | `app/docs/BACKLOG.md:2034` |
| LASTRO-FILA-PREVERIF-C11 | `app/docs/BACKLOG.md:2035` |
| BO-ROTULO-REABRIR-SO-NO-DETALHE | `app/docs/BACKLOG.md:2036` |
| PROPORCAO-FILA-OPUS-V11 | `app/docs/BACKLOG.md:2037` |
| AUDITORIA-DE-TRAVAS: MAPA-20-DERIVADO | `app/docs/BACKLOG.md:2399` |
| UI-PAINEL-FILA-OPUS-V5 | `app/docs/BACKLOG.md:2402` |
| BUG-CADASTRO-NAO-ALCANCA-O-FUTURO | `app/docs/BACKLOG.md:2403` |
| UI-ESCALA-REQUADRO | `app/docs/BACKLOG.md:2404` |
| UI-ESCALA-REQUADRO-ESPELHO | `app/docs/BACKLOG.md:2405` |

Não classificar `PROPORCAO-POR-RISCO` como POS-20 por substring: sua linha diz FECHADA NO AR e destina **achado novo** à AUDITORIA-DE-TRAVAS POS-20. Tampouco classificar `CONGELA-O-20` por sua frase de regra para **linha nova**. (`app/docs/BACKLOG.md:1917`, `app/docs/BACKLOG.md:2031`)

**Problemas concretos para semear o `.ids`:**

- E4c/E4d são etapas escritas dentro de `FIO-E0-V4-E4-ESCRITORES`; a linha não cria ids próprios para cada etapa. Exigir “id de ORDEM com linha OBRAS” impede usar literalmente `E4C`/`E4D` sem esclarecer a identidade. (`app/docs/BACKLOG.md:1882`, `app/docs/BACKLOG.md:2031`; ausência de linha própria: [nao verificado] como prova negativa completa)
- O168 tem linha `O168` e outra `C20-O168`; LASTRO tem `LASTRO-FOLGA-FORA-DO-CATALOGO` e `LASTRO-FAIXA-SEM-DEFAULT`, esta explicitamente “obra LASTRO”. A migração não pode escolher esses aliases silenciosamente. (`app/docs/BACKLOG.md:1398`, `app/docs/BACKLOG.md:2004`, `app/docs/BACKLOG.md:1810`, `app/docs/BACKLOG.md:1811`)
- `BUG-PAR-FROTA-1110` e `DISPUTA-ZUMBI-1110` são complementos absorvidos por outras obras/etapas; incluí-los como três obras autônomas pode duplicar o mesmo trabalho. (`app/docs/BACKLOG.md:1900`, `app/docs/BACKLOG.md:1908`, `app/docs/BACKLOG.md:1759`)
- A prosa histórica afirma simultaneamente “marcador autoridade”, “ordem é a das linhas” e “hook lê este bloco”; esses registros pertencem a momentos diferentes e não entregam uma regra atual única para ordenar a semente. (`app/docs/BACKLOG.md:689`, `app/docs/BACKLOG.md:699`, `app/docs/BACKLOG.md:722`)

## 12/11 13:26 · sessao 0fb37705e777 · - · - · origem=Ronald (tmux)

Sugestão de selo AST [nao verificado: implementação]:

- Propagar dependências por `Assign`, `AnnAssign`, `For` e comprehensions, separando escopos de funções.
- Marcar `%H:%M`, `[:5]` e `[-5:]` como identidade horária; reconhecer `orfas_grade` como ponte externa.
- Inspecionar `Compare`, elementos de `Set`/`SetComp`, chaves de `Dict`/`DictComp` e argumentos de `set(...)`.
- Exigir data no próprio operando horário. A chave externa de um dict não data seu set interno: esse é precisamente o problema de `relatorios/pdf_espelho.py:475`.
- Propagar o elemento da compreensão, sem confundi-lo com suas condições. A condição `b.orfa` identifica contexto sensível, mas não acrescenta data.
- Exibição isolada passa: listas de marcações e composição de texto não são comparações ou chaves (`relatorios/pdf_espelho.py:840`, `relatorios/pdf_espelho.py:844`, `relatorios/pdf_espelho.py:866`).

Os três mutantes devem restaurar, separadamente, a seleção por hora em `relatorios/pdf_espelho.py:479`, o conjunto/filtro sem data em `relatorios/pdf_espelho.py:675` e a supressão por hora em `relatorios/pdf_espelho.py:919`.

Cuidado com `.date()` de batidas: o selo anterior o rejeita explicitamente em `relatorios/tests/test_pdf_sem_regra_propria.py:158`. Formatação contendo data e hora evita essa nova ocorrência. Para S2, o produtor já fornece `%d/%m %H:%M`; inferência de ano seria informação adicional (`ponto/adaptador_motor.py:35`, `ponto/adaptador_motor.py:42`).

## 12/11 13:25 · sessao d01f0456916e · DECLARADO-X-GRAVADO · leitura · origem=Code

**O aval procede, com limite: a hora aplicável já é calculada, mas só sai embutida em texto. O destaque automático da divergência não cabe integralmente na fila “templates/static/testes”, mantendo Python intocado.** (`app/chamados/services/materializacao.py:431-465`).

**Contrato e chamadas**

- Retorna `None` se `tipo_resposta != 'hora'` ou resposta vazia após `strip`; nos demais retornos, `dict` com exatamente duas chaves, ambas `str`: `estado` e `detalhe`. Estados: `aplicada`, `recusada_paridade`, `invalida`, `ja_satisfeita`, `bloqueada_paridade`, `aplicavel`. Não entrega hora, data, tipo ou divergência como campos próprios. (`app/chamados/services/materializacao.py:438-465`).
- `aplicavel.detalhe` contém “validar vai registrar batida E/S HH:MM”. Leitura e escrita usam `_ts_tipo_do_plantio`; nele, a S139 pode substituir a resposta pelo marco do intervalo. (`app/chamados/services/materializacao.py:450`, `:463-465`, `:852`, `:409-416`).
- O consumidor encontrado anexa `pergunta.estado_aplicacao` no montador do fio; exceção também vira `None`. Esse dado chega ao fio completo, modal e re-render parcial. `meu_atendimento`, do colaborador, também utiliza o montador. (`app/chamados/services/fio.py:335-338`; `app/chamados/views.py:677-703`, `:889-931`, `:559-569`; `app/chamados/services/fio.py:446-465`).

| Tela / view | Hoje | Proposta e alcance |
|---|---|---|
| **Modal do fio** — `app/chamados/views.py:869`, `:927-931` | Mostra resposta; desenha apenas paridade/reuso, omitindo `aplicavel`. O alerta ainda promete gravar “este horário”. (`app/templates/chamados/partials/modal_fio.html:219-246`, `:258-263`). | Exibir literalmente `estado_aplicacao.detalhe` já cabe em template. Para “Resposta X → será gravado Y”, com destaque condicional, falta projeção estruturada do mesmo juiz; isso exigiria Python em `app/chamados/services/materializacao.py:431-465`, fora da fila UI. |
| **Fio completo** — `app/chamados/views.py:659`, `:677-703` | Recebe o estado, mas o template não oferece cartão de validação de pergunta: apresenta thread e resposta livre/status. (`app/templates/chamados/fio_colaborador.html:234-275`). | Não contar como validação visível já existente. Acrescentar validação/prévia aqui seria ampliar a tela; o contexto já contém as perguntas preparadas. (`app/chamados/services/fio.py:253-258`, `:319-338`). |
| **Central** — `app/chamados/views.py:726-743` | Lista detalhada valida inline, com confirmação genérica; o endpoint percorre perguntas respondidas. Não há estado de aplicação na linha. O resumo padrão abre o fio. (`app/templates/chamados/partials/_lista_chamados.html:82`, `:143-167`; `app/chamados/views_cobrar.py:145-169`; `app/chamados/services/painel.py:385-405`; `app/templates/chamados/partials/_lista_resumo.html:53-60`). | Prévia por **cada pergunta**, antes de validar. Exige alimentar contexto em `app/chamados/services/painel.py` ou uma prévia em `app/chamados/views_cobrar.py`: ambos fora de templates/static. Alternativa só de navegação: encaminhar ao modal com a prévia. |
| **Detalhe local / cartão HTMX** — `app/chamados/views.py:549-551`, `:398-405` | Exibe resposta, evidência, previsto/alerta e botão Validar; promete gravar “este horário”, sem estado de aplicação. (`app/templates/chamados/detalhe_local.html:310`; `app/templates/chamados/partials/_pergunta_card.html:29-54`, `:68-97`). | Alimentar estado no GET **e** no fragmento após ação; desenhar a mesma projeção do modal e corrigir a promessa. Exige `app/chamados/services/acoes_chamado.py:334-419` e `app/chamados/views.py:398-405`, fora de templates/static. |
| **Lote / drawer** — `app/chamados/views_cobrar.py:353-386` | Tabela mostra Resposta / Previsto / Dif.; o produtor fornece classificação, sem estado de aplicação. (`app/templates/chamados/partials/_lote_preview.html:23-26`; `app/chamados/services/validacao.py:921-924`). | Coluna “Será gravado”, estado e destaque por pergunta. Exige alimentação em `app/chamados/views_cobrar.py:361-368`; alterar o produtor em `services/validacao.py` também tocaria serviço de núcleo. “Previsto” atual não deve ser reaproveitado como resultado do plantio. |
| **Lote / copiloto** — `app/chamados/views_cobrar.py:275-302` | Até 200 referências, mas apenas 10 exemplos; JS mostra resposta/previsto e valida todas as referências. (`app/chamados/views_cobrar.py:270-293`; `app/templates/core/_copiloto.html:236-250`). | Encaminhar à prévia completa ou fornecer projeção de todas as referências. Exige Python em `views_cobrar.py`; alterar `_copiloto.html` alcança include da base. (`app/templates/base.html:432`). |

Há ainda “Aplicar hora e validar” para declaração textual no modal; essa porta também chega a `validar_pergunta` e precisa da mesma honestidade na promessa de gravação. (`app/templates/chamados/partials/modal_fio.html:130-143`; `app/chamados/services/declaracao_texto.py:696`).

**Riscos da proposta**

- **Previsão parcial:** o estado exige batida aprovada para anunciar reuso; a escrita reusa qualquer existente. A escrita também tem bloqueio por data divergente que não aparece nesse estado. Portanto `aplicavel` não garante o desfecho final. (`app/chamados/services/materializacao.py:453-465`, `:865-877`, `:1197-1204`).
- **N+1/cache:** o fio consulta estado por pergunta; ele busca grade, batida existente e vizinhas. `estado_aplicacao_pergunta` não recebe `carga`; `CargaDaVida` não elimina automaticamente esse custo. Proposta: alimentar o mesmo juiz por lote, sem cache global entre requests. (`app/chamados/services/fio.py:319-338`; `app/chamados/services/materializacao.py:283-285`, `:431-459`, `:95-104`; `app/chamados/services/carga_pergunta_viva.py:152-158`; `app/chamados/juizes.py:879-889`).
- **Verificação de tela:** cobrir hora igual/divergente, reuso, bloqueio, `None`, re-render e múltiplas perguntas; bases/includes/JS exigem smoke nas duas cascas ou prova pelo grafo, conforme a regra local. (`CLAUDE.md:202-203`; `app/chamados/services/materializacao.py:438-465`).
- **Produção além do anexo, custos medidos nesta cópia e resultado de smoke/testes:** [nao verificado]. [nao verificado]

## 12/11 13:25 · sessao ec41dabd9e8a · - · - · origem=Ronald (tmux)

Censo A conferido, sem escrita.

| Sítio | Entrada → saída | Caso protegido/teste | Lei/corte | [nao verificado]
|---|---|---|---|
| `_ts_tipo_do_plantio`: hora S139 | `resposta_colab` → hora de `_marco_faltante` para **todo** motivo `intervalo_saida`/`intervalo_volta` quando há marco; o corpo não testa `intervalo_livre`, “fixo” ou distância. `app/chamados/services/materializacao.py:409-419` | Volta informada `02:00`, marco `03:00`: exige `E@03:00`, impede `E@02:00`. `app/chamados/tests/test_s139.py:14-18`; `app/chamados/tests/test_s139.py:40-56` | **SEM LEI específica localizada** nos três documentos pesquisados [nao verificado]. L-032 trata intervalo no envelope, não autoriza substituir declaração; L-099 exige coerência com cadastro. `app/docs/LEIS.md:80`; `app/docs/LEIS.md:122` |
| `_tipo_pelo_marco` | Tipo inicialmente pelo motivo (`E` para três motivos, `S` nos demais) → tipo do marco cobrado por motivo; fallback ATA, marco mais próximo em até 240min. Havendo divergência, prevalece snapshot cobrado. `app/chamados/services/materializacao.py:344`; `app/chamados/services/materializacao.py:246-300`; `app/chamados/services/materializacao.py:390-407` | `orfao_14h` + resposta `19:01` gerava `S`, apesar de marco de entrada; teste exige `E`; sentido inverso também testado. `app/chamados/tests/test_hx_tipo_pelo_marco.py:86-119` | Corte Ronald 31/08 declarado no código e teste; documento nos três índices: **SEM LEI específica localizada** [nao verificado]. `app/chamados/services/materializacao.py:247-262`; `app/chamados/tests/test_hx_tipo_pelo_marco.py:16-26` |
| Tipo sem marco | Ausência de marco identificável → mantém tipo pelo motivo e grava `pendencia_admin=tipo_nao_conferido`, junto de `recusa_motivo`. `app/chamados/services/materializacao.py:392-407`; `app/chamados/services/materializacao.py:851-857` | Sem régua ou marco distante: ainda planta pelo motivo, com pendência explícita. `app/chamados/tests/test_hx_tipo_pelo_marco.py:121-142` | Corte caminho C 31/08 declarado no corpo; documento nos três índices: **SEM LEI específica localizada** [nao verificado]. `app/chamados/services/materializacao.py:393-405` |
| Data do plantio + overnight | `data_evento`, fallback contexto do motivo, + hora fonte → instante local; pode mudar de D para D+1 quando cabe no envelope da cadeia de marcos do vínculo do dia, ±90min; sem marcos fica em D. `app/chamados/services/materializacao.py:345-350`; `app/chamados/services/materializacao.py:418-426`; `app/chamados/juizes.py:296-331` | Turno partido S12 fica D; intervalo noturno volta00 vai D+1; sem marcos entrada não recua D−1. `app/chamados/tests/test_s3f4_data_e_ancora_pela_autoridade.py:59-91` | L-085 sustenta **dia do turno = início**; envelope ±90 e derivação específica têm cortes 15/09 documentados em código/teste, sem corte específico encontrado nos três documentos [nao verificado]. `app/docs/LEIS.md:117`; `app/chamados/juizes.py:297-309` |
| Reuso S137 | Procura o primeiro timestamp do mesmo colaborador/tipo dentro de ±20min do instante derivado; preserva timestamp/tipo da batida encontrada e escreve `pergunta_origem`. `app/chamados/juizes.py:235-247`; `app/chamados/services/materializacao.py:865-876`; `app/chamados/services/materializacao.py:952-956` | Evitar S falsa em D06:46 quando a saída real do turno já existe em D+1 06:46; teste exige contagem inalterada. `app/chamados/tests/test_materializar_overnight.py:1-4`; `app/chamados/tests/test_materializar_overnight.py:48-71` | **SEM LEI específica localizada** para ±20min [nao verificado]; propósito declarado no próprio juiz. `app/chamados/juizes.py:236-238` |
| Deduplicação final da porta | Mesmo tipo vivo até ±60s → retorna existente; mesma chave `(colaborador,timestamp)` → retorna existente inclusive outro tipo/retratada. Não altera seus valores. `app/ponto/registro_batida.py:106-122` | Reenvio da mesma saída já reclassificada como entrada causava constraint/500; caso registrado no comentário e teste. `app/ponto/registro_batida.py:116-122`; `app/ponto/tests/test_bater_mesma_chave_idempotente.py:1-6` | Corte 14/09 declarado no corpo; **SEM LEI específica localizada** nos três documentos [nao verificado]. `app/ponto/registro_batida.py:116-119` |
| Ramo `data_hora` | Aceita data + HH:MM[:SS], mas só utiliza HH e MM; segundos são descartados; sem componente de hora usa `00:00`; tipo sempre `E`. `app/chamados/services/materializacao.py:1001-1014` | Teste fundador da truncagem/default: [nao verificado]. O teste encontrado protege colisão da âncora E com S real próxima. `app/chamados/tests/test_chokepoint_ancora.py:8-11`; `app/chamados/tests/test_chokepoint_ancora.py:24-44` | **SEM LEI específica localizada** para truncar segundos ou completar `00:00` [nao verificado]. |

O teste O306 **mantém S139**: `13:05` vira `13:00` quando o marco pertence à própria pergunta; impede usar marco de motivo vizinho. `app/chamados/tests/test_o306_marco_do_motivo_nao_e_do_vizinho.py:136-153`

Os sítios efetivos de criação a partir de `PerguntaDisputa` são os dois ramos do materializador, que chamam `registrar_batida`; o timestamp/tipo recebido é gravado pela porta sem outra transformação no create. `app/chamados/services/materializacao.py:933-940`; `app/chamados/services/materializacao.py:1013-1025`; `app/ponto/registro_batida.py:152-159`

Validação individual, lote de perguntas, regularização de disputa e reparo do passivo convergem para essa unidade, sem segundo derivador nesses caminhos. `app/chamados/services/acoes_disputa.py:13-22`; `app/chamados/services/validacao.py:461-469`; `app/chamados/services/validacao.py:735-746`; `app/chamados/services/acoes_disputa.py:118-139`; `app/chamados/services/passivo_validado.py:514-516`

`RegularizacaoExterna` é outra entrada: itens lançados pela supervisão e confirmados pelo colaborador, sem leitura de `PerguntaDisputa.resposta_colab`; para item `intervalo=True`, ordena as duas horas e atribui S à menor/E à maior. Este é sítio de alteração do **tipo associado aos campos**, que merece constar no limite do censo, mas não pertence ao universo das perguntas. `app/chamados/services/regularizacao_ext.py:1-19`; `app/chamados/services/regularizacao_ext.py:52-55`; `app/chamados/services/regularizacao_ext.py:168-179`; teste `app/chamados/tests/test_reg_intervalo.py:11-14`; `app/chamados/tests/test_reg_intervalo.py:28-45`

A correção externa escreve diretamente `alvo.tipo` e `alvo.timestamp`, a partir de `item.hora_saida` prioritária, senão `hora_entrada`, e `item.data`; não executa ajuste overnight nesse ramo. `app/chamados/services/regularizacao_ext.py:143-156`

Para TRIAGEM, existe regra real: `_carimbada_como_miolo` reconhece `is_intervalo`, `_intra_dur` ou `_intra_ata`; as duas últimas são marcas em memória do pareamento. `intervalos_do_turno` lê pares marcados e aplica piso de 15min. `app/ponto/turnos.py:932-940`; `app/ponto/turnos.py:502-546`

`Batida.is_intervalo` é campo persistido explícito de saída/volta do intervalo; `Batida.origem` é o campo do canal de origem, default `app`. `app/ponto/models.py:26`; `app/ponto/models.py:45`

## 12/11 13:25 · sessao 4a40a12f21d6 · - · - · origem=Ronald (tmux)

Contratos encontrados para D, sem execução em Django ou produção. [nao verificado]

- Universo: `respondida_em__isnull=False`, motivos pedidos e `tipo_resposta='hora'`. **Não usar `p.estado == RESPONDIDA`**, porque a propriedade devolve `VALIDADA`/`RESOLVIDA` quando há `validada_em`; isso excluiria os plantios do universo. (`app/chamados/models.py:2443-2450`; `app/chamados/models.py:3034-3049`)
- Relação real: `Batida.pergunta_origem`, reverso `PerguntaDisputa.batidas_plantadas`; `PerguntaDisputa.batida_origem` identifica outro tipo de episódio e não a batida produzida pela resposta. (`app/ponto/models.py:40-44`; `app/chamados/models.py:2437-2442`)
- Origem: campo `Batida.origem`; aparelho é `'app'`; plantio hora atual passa literal `'disputa_s84_retro'`. (`app/ponto/models.py:13-20`; `app/ponto/models.py:45`; `app/chamados/services/materializacao.py:933-939`)
- **Origem disputa + relação não prova que aquela pergunta criou a batida**: S137 procura/reusa qualquer existente antes da criação, inclusive de disputa; a ligação depois é sobrescrita para a pergunta atual. Não há campo persistido “criada nesta pergunta” ou “reuso”. Logo, classificação histórica exata nova/reuso exige deixar ambiguidades em `OUTRO`, ou declarar explicitamente que “nova” é o critério operacional por origem, com sua limitação. (`app/chamados/services/materializacao.py:863-877`; `app/chamados/services/materializacao.py:952-956`)
- Há segundo dedup na porta: mesmo tipo dentro de 60 segundos e mesma chave exata podem devolver existente, sem informar um booleano `created`. (`app/ponto/registro_batida.py:94-122`)
- A trilha `materializar_batida` roda tanto para nova quanto reuso; o JSON grava o `ts` derivado, não necessariamente `nova_batida.timestamp`, e fixa origem disputa mesmo quando reusou app. **Não usar esse JSON como fato da batida persistida nem indicador de criação.** (`app/chamados/services/materializacao.py:947-978`)
- `Batida.criado_em` existe e serve como instante de criação da nova batida; `timestamp` é instante marcado. (`app/ponto/models.py:23`; `app/ponto/models.py:79`)

**Desfechos e precedência**

- Recusa paridade terminal: `p.via_resolucao == PerguntaDisputa.VIA_PARIDADE_RECUSADA`; a própria leitura `estado_aplicacao_pergunta` usa isso. (`app/chamados/models.py:2475`; `app/chamados/services/materializacao.py:440-447`; `app/chamados/services/materializacao.py:930`)
- Existe recusa paridade sem essa via: quando marco está apagado, chama `_carimbar_recusa`, grava `recusa_motivo` e fica sem materializar. O catálogo real pode classificá-la com `traduzir_recusa(p.recusa_motivo)[0] == 'PARIDADE'`. (`app/chamados/services/materializacao.py:906-922`; `app/chamados/services/materializacao.py:1089-1109`; `app/chamados/catalogo/recusa.py:26`; `app/chamados/catalogo/recusa.py:77`)
- `pendencia_admin` não significa ausência de plantio: pode ser gravado por tipo não conferido e a execução continuar criando batida. Portanto, nova/reuso devem preceder essa categoria no censo exclusivo, e sobreposição merece contador separado. (`app/chamados/services/materializacao.py:390-405`; `app/chamados/services/materializacao.py:851-859`; `app/chamados/services/materializacao.py:933-939`)
- `_carimbar_recusa` normalmente grava só `recusa_motivo`, sem `pendencia_admin`; apenas código `'COLISAO'` carimba a via `fato_ocupado`. (`app/chamados/services/materializacao.py:1094-1109`)
- Leitor existente dos rótulos de administração: `explicar_pergunta`: `aguarda_validacao` quando estado RESPONDIDA, depois `pendencia_admin`, depois juiz factual; falha de plantio validada vira `validada_sem_gravar`. Pode usá-lo como definição explícita de pendente administrativo, lembrando que sua precedência não é a precedência solicitada pelo censo exclusivo. (`app/chamados/services/explicador.py:68-104`)
- `nao_validada`: campo `validada_em is None`; outras vias/carimbos sem batida e sem pendência devem ficar `OUTRO`, sem adivinhar. (`app/chamados/models.py:2450`; `app/chamados/models.py:2469-2543`)

**Data e DNA histórico**

- Data da pergunta: `p.data_evento`; fallback real é `date.fromisoformat((p.disputa.contexto or {})[p.motivo]['data'])`. (`app/chamados/services/materializacao.py:345-353`)
- A data não é a data civil da resposta: `_ajustar_saida_overnight` trata `data_evento` como **dia do turno**, lê cadeia prevista, tenta dia do turno e seguinte dentro do envelope de 90 minutos; fora dele permanece no dia do turno. (`app/chamados/juizes.py:296-331`)
- Encadeamento real: `marcos_turno` lê `ec.marcos_do_dia`, sequência hi→hii→hfi→hf, avançando um dia quando próximo marco `<=` anterior. (`app/escala/servico_jornada.py:69-94`)
- Para datar resposta sem passar por S139, pode montar `ts` da resposta em `data_evento`, chamar somente `_ajustar_saida_overnight` alimentado com `carga.vinculo(...)` e `carga.celulas` contendo cópia da célula com o DNA histórico; assim chama o juiz real com alimentação histórica. A função recebe essa alimentação sem precisar do resto de `CargaDaVida`. (`app/chamados/juizes.py:316-321`; `app/escala/servico_jornada.py:75-77`; `app/escala/models.py:1117-1132`)
- Campos exatos: `CelulaDia.regenerada_em`, `.regeneracoes`, `.dna_anterior`, `.dna`; DNA contém `vinculo_id`, `tipo_escala_id`, `marcos` e `lampadas`. (`app/escala/models.py:1692-1729`; `app/ponto/management/commands/gerar_celulas.py:95-104`)
- Seleção exigida pelo usuário é DNA anterior se `regenerada_em > b.criado_em`, DNA atual caso contrário. Mas **dna_anterior só guarda imediatamente anterior**: com várias regenerações, não garante versão vigente no plantio; imprimir contador de ambiguidade e não chamar essa versão histórica comprovada. (`app/escala/models.py:1714-1729`; `app/ponto/portas/celula.py:271-277`)
- `gerada_em` é `auto_now_add`, não acompanha regeneração; `regenerada_em` é última regeneração, não edição humana. (`app/escala/models.py:1702-1721`)
- Não cair no template atual quando DNA histórico/marcos estiver ausente: `ec.marcos_do_dia` tem esse fallback; ausência deve sair como não verificada na sonda histórica. (`app/escala/models.py:1098-1103`; `app/escala/models.py:1124-1133`)
- Marco oposto: saída→`hfi`, volta→`hii`, conforme pares e tipos da célula. (`app/escala/models.py:1652-1656`)

**Vínculo e TRIAGEM**

- `escala_vigente(colaborador, dia)` é carregador da autoridade pura `escala.alimentacao.vinculo_do_dia`, não outra regra de escolha. Célula nomeia geradora; vigência continua sendo conferida. (`app/escala/servico_jornada.py:15-66`; `app/escala/alimentacao.py:269-286`)
- Há regra existente para batida de intervalo: `_carimbada_como_miolo(b)` considera `is_intervalo`, `_intra_dur` e `_intra_ata`. **Não precisa dizer que inexiste e usar primeira/última.** (`app/ponto/turnos.py:932-940`)
- As marcas em memória vêm de `turnos_do_colab(colab,d,d)` e seu pareador; itere `t.batidas`, filtre `_carimbada_como_miolo`, `origem == 'app'`, e use dono de `t.data_turno` via `escala_vigente` para confirmar mesmo vínculo. A função já exclui retratadas e considera virada do dia. (`app/ponto/turnos.py:1850-1866`; `app/ponto/turnos.py:1915-1925`; `app/ponto/turnos.py:1935-1939`; `app/ponto/turnos.py:1960`; `app/ponto/turnos.py:1992-1995`)
- `intervalos_do_turno` é outra pergunta: retorna pares com piso de 15 minutos, enquanto `_carimbada_como_miolo` responde geometria. Para comparar proximidade a batidas de intervalo, a segunda é mais direta. (`app/ponto/turnos.py:502-546`; `app/ponto/turnos.py:932-940`)

**Carga semanal**

- Se contar perguntas validadas no estado atual, use `p.validada_em` e semana ISO local desde `2026-09-21`, explicitando uma validação atual por pergunta. (`app/chamados/models.py:2450`)
- Se “validações” significar todos os atos históricos, existe `DecisaoPergunta` com `ato='validacao'`, `ato_em`, `pergunta`; a pergunta guarda projeção, e linha é o fato. Misturar os dois universos muda a unidade. (`app/chamados/models.py:3840-3856`; `app/chamados/models.py:3858`; `app/chamados/models.py:3872-3876`)
- Top escala precisa dizer se é pk de **vínculo** (`dna['vinculo_id']`/EscalaColaborador.pk) ou **modelo** (`dna['tipo_escala_id']`/TipoEscala.pk); os dois existem no mesmo DNA. (`app/ponto/management/commands/gerar_celulas.py:97-98`)

## 12/11 13:25 · sessao f80726ddfd1e · - · - · origem=Ronald (tmux)

B — telas/rotas de validação:

| Superfície | View/rota e template | O que mostra | Fonte |
|---|---|---|---|
| Modal do fio | `app/chamados/urls.py:39`; `app/chamados/views.py:869`, `:927`, `:951-954`; `app/templates/chamados/partials/modal_fio.html:219`, `:258-284` | Resposta bruta e, no alerta, resposta versus previsto; não desenha o detalhe `aplicavel` que contém a hora futura. O detalhe exibido é só `bloqueada_paridade` ou `ja_satisfeita`. | `estado_aplicacao_pergunta` é calculado em `app/chamados/services/fio.py:335-338`; filtrado no template em `app/templates/chamados/partials/modal_fio.html:230-246`; hora futura existe em `app/chamados/services/materializacao.py:463-465`. |
| Detalhe local/cartão individual | `app/chamados/urls.py:14`; `app/chamados/views.py:483-501`, `:549-551`; `app/templates/chamados/detalhe_local.html:307-310`; `app/templates/chamados/partials/_pergunta_card.html:31`, `:43-54`, `:67-97` | `resposta_display`, resposta versus previsto no alerta, evidência física; não exibe uma previsão derivada do plantio. | Usa `leitura_da_resposta` → `classificar_resposta_validacao`/`frase_da_resposta`, em `app/chamados/templatetags/chamados_tags.py:144-152`; montagem da evidência em `app/chamados/services/acoes_chamado.py:361-386`. O cartão não referencia `estado_aplicacao`; seus campos de exibição/validação estão em `app/templates/chamados/partials/_pergunta_card.html:29-98`. |
| Central, botão inline ✓ | `app/chamados/urls.py:33`; `app/chamados/views_cobrar.py:145-169`, `:201-214`; `app/templates/chamados/partials/_lista_chamados.html:79-82` | O botão/linha não apresenta hora respondida nem hora futura antes de validar. | Seleciona perguntas por `perguntas_vivas_de`, chama `acao_validar_pergunta`; não consulta `estado_aplicacao_pergunta`. `app/chamados/views_cobrar.py:168-169`, `:201-208`. |
| Central, drawer “Validar em lote” | `app/templates/chamados/painel_gestao.html:14`; `app/chamados/urls.py:36-37`; `app/chamados/views_cobrar.py:353-368`, `:372-386`; `app/templates/chamados/partials/_lote_preview.html:21-26` | Resposta, “Previsto” e diferença; não a hora derivada que será gravada. | `fila_validar_classificada`/`classe_do_lote`; `esperada` vem de `delta_da_resposta`, enquanto `_slot_esperado` só serve à busca de conflito. `app/chamados/services/validacao.py:864-877`. |
| Copiloto, proposta/validação de lote | `app/chamados/urls.py:34`, `:38`; `app/chamados/views_cobrar.py:232-261`, `:275-302`; `app/templates/core/_copiloto.html:236-250` | Amostra de resposta e “previsto”; não previsão do plantio. | `fila_validar_classificada`, campos `resposta_colab`, `detalhe.esperada`, `delta_min`. `app/chamados/views_cobrar.py:287-293`. |
| Modal, “Aplicar HH:MM e validar” de declaração em texto | `app/templates/chamados/partials/modal_fio.html:126-143`; `app/chamados/services/declaracao_texto.py:641-648`, `:687-696` | Hora extraída, tipo, dia e marco previsto; não deriva a hora efetiva pelo materializador. | Proposta de `propostas_do_chamado`; grava resposta pela porta canônica e chama `validar_pergunta`. `app/chamados/services/declaracao_texto.py:649-652`, `:691-696`; propostas preparadas em `app/chamados/services/fio.py:291-299`. |

A rota do fio completo aceita POST pelo mesmo `processar_post_fio`, incluindo `validar_pergunta`; entretanto seu HTML apresenta descrição, followups e caixa de resposta, sem os cartões/botões de validação presentes no modal. A ausência foi conferida por busca no arquivo inteiro; os blocos renderizados são `app/templates/chamados/fio_colaborador.html:229-260`; delegação da rota em `app/chamados/views.py:667-677`; validação comum em `app/chamados/services/fio.py:585-609`.

“Regularizar disputa” também aparece no detalhe/modal, depois do conjunto de respostas, como operação de fechamento/materialização, sem uma nova previsão individual por pergunta: `app/templates/chamados/detalhe_local.html:314-326`; `app/templates/chamados/partials/modal_fio.html:353-359`; entradas em `app/chamados/views.py:467-475` e `app/chamados/services/fio.py:646-656`.

C — trilha:

- `_log_auditoria_materializacao` envia ação, modelo `Batida`, PK, descrição, JSON `depois` e ator a `core.observ.evento`; seu contrato é best effort. `app/chamados/services/materializacao.py:731-737`.
- Na resposta `hora`, o JSON contém `batida_pk`, `colaborador_id`, `timestamp`, `tipo`, `origem`, `disputa_pk`, `chamado_pai_pk`, `chamado_destino_pk`, `chamados_relacionados_ids`, `pergunta_pk`, `resposta_colab`. A descrição ainda contém nome do colaborador, timestamp e referências de disputa/chamados. `app/chamados/services/materializacao.py:957-978`.
- Na âncora `data_hora`, contém `batida_pk`, `colaborador_id`, `timestamp`, `tipo`, `origem`, `disputa_pk`, `chamado_destino_pk`, `pergunta_pk`, `resposta_colab`; descrição contém nome, timestamp e referências. `app/chamados/services/materializacao.py:1045-1063`.
- Esses payloads não registram código da regra aplicada, motivo da substituição nem marco usado; registram resposta e timestamp derivado. `app/chamados/services/materializacao.py:966-978`, `:1053-1063`.
- Cuidado: o log `materializar_batida` também roda no reuso, declara “criada”, fixa `origem='disputa_s84_retro'` e usa `timestamp=ts` derivado; não garante que esses sejam origem/timestamp da Batida reusada. O reuso não altera timestamp: apenas chega à anotação de `pergunta_origem`. `app/chamados/services/materializacao.py:865-877`, `:952-978`.
- `anotar` grava os campos pedidos e trilha antes/depois automática; no vínculo da pergunta são o antigo/novo `pergunta_origem`, ação `vincular_pergunta_origem`, descrição com PKs e ator. Não recebe regra/marco nesse chamado. É idempotente, sem nova linha se o campo já coincide. `app/ponto/registro_batida.py:235-263`; `app/chamados/services/materializacao.py:952-956`.
- O registro final inclui `usuario`, `acao`, `modelo`, `objeto_id`, `descricao`, `valor_antes`, `valor_depois`, `ip` e eventual `ator` do processo. `app/core/observ.py:118-123`, `:133-137`; adaptação de `anotar` em `app/ponto/registro_batida.py:225-232` e `app/core/auditoria.py:17-24`.

## 12/11 13:24 · sessao 6a8cc614e150 · - · - · origem=Ronald (tmux)

Achados em leitura:

- **Aval confirmado com limite:** `aplicavel` já traz “validar vai registrar batida E/S HH:MM” em `app/chamados/services/materializacao.py:463-465`. O horário vem de `_ts_tipo_do_plantio` (`:450`), também usado pela escrita (`:852`); S139 substitui resposta pelo marco de intervalo (`:409-416`).
- **Contrato real:** `dict[str,str]` com exatamente `estado` e `detalhe`, ou `None` para tipo diferente de `hora` ou resposta vazia (`materializacao.py:438-465`). Estados: `recusada_paridade`, `aplicada`, `invalida`, `ja_satisfeita`, `bloqueada_paridade`, `aplicavel` (`:444-465`). Não fornece `hora`, `timestamp`, `data`, `tipo` ou `divergiu` estruturados (`:431-465`). [nao verificado]
- **Consequência da fila 2:** exibir `detalhe` de `aplicavel` cabe em template; destacar diferença sem interpretar texto/recalcular exige projeção Python adicional, portanto fora de templates/static, em `app/chamados/services/materializacao.py:431` e alimentação de contextos. O atual contrato não permite comparação segura da hora (`:463-465`).
- **Quem chama e entrega:** único consumidor de produção encontrado é `montar_chamados_fio`, anexando `p.estado_aplicacao` (`app/chamados/services/fio.py:335-338`). Fio chama e entrega `chamados` (`app/chamados/views.py:677,697-703`); modal chama e entrega (`:889,927-931`); re-render parcial também (`app/chamados/services/fio.py:437-446,461-465`). `meu_atendimento` também usa o montador, embora seja tela do colaborador (`views.py:559-569`).
- **Modal omite apenas o aplicável:** os dois ramos mostram `bloqueada_paridade` e `ja_satisfeita`, sem `aplicavel` (`app/templates/chamados/partials/modal_fio.html:230-246`).

Riscos comprováveis na cópia:

- **N+1:** montador chama estado por pergunta (`fio.py:319-338`); estado consulta batida existente e vizinhas (`materializacao.py:453-459`), vizinhas fazem até duas queries (`:95-104`), tipo consulta grade (`:283-285`), overnight consulta vínculo (`app/chamados/juizes.py:317-321`). `select_related` de perguntas não elimina essas consultas (`fio.py:225-232`).
- **CargaDaVida não alimenta esse estado:** função não aceita `carga` e chama auxiliares avulsos (`materializacao.py:431,450-459`). CargaDaVida pertence à consulta do C6 (`carga_pergunta_viva.py:69-83`), memoiza grade por colaborador/dia (`:152-158`) e tem batida na janela alimentada (`:188-190`); seu uso atual é `pergunta_viva(...carga=...)` (`tarefa_do_dia.py:488-491`). Não é cache pronto para colar no estado.
- **Cache:** memo C6 vive na instância/request; com carga não lê nem altera memo avulso (`juizes.py:872-889`). Evitar cache global/entre requests (`:879-880`).
- **Promessa ainda parcial:** leitura só chama “já satisfeita” para batida aprovada (`materializacao.py:453-454`), escrita reusa qualquer existente (`:865-877`); gate `DATA_EM_DISPUTA` existe na escrita (`:1197-1204`) e não no estado (`:431-465`). Portanto estado aplicável não garante desfecho final.
- **Smoke:** regra explícita para bases/includes/JS e clique nas duas cascas (`CLAUDE.md:202-203`); piso existente explica árvores separadas (`app/core/tests/test_smoke_chromium.py:18-21`). Selo pode provar exclusão de uma casca pelo grafo de includes (`CLAUDE.md:203`).
- **S139 protege caso inverso:** teste documenta volta 03:00 digitada 02:00, slot oposto (`app/chamados/tests/test_s139.py:14-18`) e verifica plantio 03:00 (`:49-56`). Não executei testes, pois produzem arquivos/banco e a autorização é só leitura.

## 12/11 13:24 · sessao abbee9299297 · - · - · origem=Ronald (tmux)

Mapa adicional ao fio, somente código:

| Tela | Hoje | Proposta e alcance |
|---|---|---|
| **Central, lista detalhada** | `painel_gestao` monta contexto por `montar_contexto_painel` e renderiza lista: `app/chamados/views.py:726-743`. Botão valida todas as respondidas pendentes do chamado: `app/templates/chamados/partials/_lista_chamados.html:82`; confirmação só diz “Validar as respostas pendentes deste chamado?”: `:143-167`; endpoint percorre perguntas e chama autoridade: `app/chamados/views_cobrar.py:145-169,201-213`. Nenhum horário de aplicação no contexto de linha: `app/chamados/services/painel.py:385-405`. | Prévia por pergunta lendo `estado_aplicacao_pergunta`, antes do POST. Exige contexto/endpoint em Python, fora de templates/static: `views_cobrar.py` e/ou `services/painel.py`; templates/JS não têm o dado hoje nas linhas citadas. Alternativa estritamente UI: encaminhar ao fio antes de validar, deixando a prévia existente do fio carregar o juiz. |
| **Central, resumo padrão** | Padrão selecionado em `app/chamados/views.py:736-741`; não valida diretamente, abre fio em `app/templates/chamados/partials/_lista_resumo.html:53-60`. | Cobertura pela alteração do fio. |
| **Lote, drawer** | GET `lote_preview`: `app/chamados/views_cobrar.py:353-368`; POST `validar_lote_tela`: `:372-386`. Tabela exibe resposta, horário previsto e diferença: `app/templates/chamados/partials/_lote_preview.html:23-26`. Itens são classe/detalhe, sem estado de aplicação: `app/chamados/services/validacao.py:921-924`. | Acrescentar coluna “Será gravado”, estado e destaque usando o mesmo juiz por pergunta. Exige `views_cobrar.py` fora de templates/static (ou alterar produtor `services/validacao.py`, núcleo); “Previsto” atual não é previsão de gravação. |
| **Lote, copiloto IA** | GET `proposta_lote` traz até **200 refs** mas amostra de **10** com resposta/previsto: `app/chamados/views_cobrar.py:270-302`. JS mostra amostra e valida todas as refs: `app/templates/core/_copiloto.html:236-250`. | A amostra não cobre cada pergunta a validar. Usar drawer completo ou enviar estado para todas as refs; exige Python em `views_cobrar.py`, fora da raia UI. |
| **Detalhe local / cartão HTMX** | GET `app/chamados/views.py:549-551` → produtor `app/chamados/services/acoes_chamado.py:334-419` → include `app/templates/chamados/detalhe_local.html:310`. Mostra resposta e evidência: `_pergunta_card.html:29-39`, além de resposta/previsto no alerta: `:52-54`; valida: `:92-97`. Pós-ação renderiza cartão só com `perg`/`pode_agir`: `app/chamados/views.py:398-405`. | Anexar estado no GET **e** no fragmento após ação; exige `services/acoes_chamado.py` e `views.py`, fora de templates/static. Revisar promessa atual “este horário será gravado” em `_pergunta_card.html:54,68`, que não lê esse estado. |

Porta adicional no mesmo modal: “Aplicar hora e validar”, `app/templates/chamados/partials/modal_fio.html:140-143`, tratada por `app/chamados/views.py:478-481`; também chama `validar_pergunta` em `app/chamados/services/declaracao_texto.py:696`. Ela aplica declaração textual, portanto precisa delimitação de escopo em relação à unidade já respondida do aval.

Busca de chamadas produtivas achou ainda API de foto, mas sem tela de intervalo comprovada: `app/api/foto_recebida.py:51` [nao verificado quanto à aplicabilidade a intervalo].

## 12/11 13:24 · sessao 9f580ff5149c · AUDITORIA-DE-TRAVAS · verificador · origem=Code

1. **Tetos corretos:** `1630+299=1929`, `594+63=657`, `891+288=1179`. O comentário identifica corretamente as últimas 20 por papel e as 20 agrupadas de leitura/censo/sonda; nenhuma troca de número ou janela encontrada. (`medida_6_8.r:9–11`; `bin/codex_pergunta.sh:85–95`)

2. **Mordem por inspeção:** `c_positivo` exige 1929 na terceira linha e `codex` na quarta; `teto_construtor` troca precisamente 1929→1930. `c_timeout_zero` exige recusa de `0600`, inclusive ausência de chamada ao Codex; seu mutante remove somente o reconhecimento de zero inicial. (`bin/tests/test_codex_pergunta.sh:139–143,289–294,493,595,597`)
   
   Trocar somente **657 ou 1179 também é detectado**, pelos casos básicos, embora não haja mutantes numéricos dedicados aos dois. (`bin/tests/test_codex_pergunta.sh:463,474,480,593–598`)
   
   Lacuna plausível: trocar `-k 30` por `-k 300`; as verificações novas ignoram a segunda linha do argv, e o Codex falso termina imediatamente. Sobrevivência por inspeção; execução **[não verificado]**. (`bin/codex_pergunta.sh:414`; `bin/tests/test_codex_pergunta.sh:111,293–294`)

3. **Escritores/leitores:** o wrapper escreve `TETO_S` em `:95`, lê-o em `:96,98`; escreve `TIMEOUT` em `:96,98`, lê-o na validação `:97–98`, no `flock -w` `:389` e nos dois `timeout` `:414,421`. `CODEX_TIMEOUT` é entrada de ambiente lida em `:96`; os testes a fornecem em `bin/tests/test_codex_limite.sh:24` e `bin/tests/test_codex_pergunta.sh:461,468,478,492–493`.
   
   Os valores informados dos lançadores — 1800/2400/3600/5400 — **não provocam recusa**: acima do teto são cortados; abaixo são preservados. Conferência direta dos lançadores: **[não verificado]**. (`opus_v2_reprova.txt:1`; `bin/codex_pergunta.sh:97–98`) A chamada real de prova usa o default, sem definir essa variável. (`bin/tests/prova_codex_pergunta_real.sh:85`)

4. **Classes:** segundo juiz/escritor ou derivação paralela do teto: não encontrados; decisão concentrada na tabela e normalização do wrapper. (`bin/codex_pergunta.sh:95–98`) Relógio envolve efetivamente `codex`, mas preparação e espera por slots ficam fora dele. (`bin/codex_pergunta.sh:361–389,414–422`) `TIMEOUT` atende dois orçamentos sequenciais — trava e execução — explicitamente documentados; não representa duração total. (`bin/codex_pergunta.sh:92–93,389,414,421`) Selo sem mordida: os dois achados específicos foram corrigidos por inspeção, com a lacuna adicional acima. Execução dos selos: **[não verificado]**. (`bin/tests/test_codex_pergunta.sh:293–294,493,595,597`)

## 12/11 13:17 · sessao f268da51b608 · S139-RESPEITA-A-RESPOSTA · sonda · origem=Code

Referências `DADO:N` contam as linhas tabuladas do bloco fornecido, começando em `S R_COLUNAS` como linha 1.

1. **TOTAL**

| Medida | Número | Fonte |
|---|---:|---|
| perguntas | 152 | DADO:317 (`S TOTAL`) |
| relações | 152 | DADO:317 |
| hora_diferente | 62 | DADO:317 |
| acima30 | 31 | DADO:317 |
| delta_nv | 0 | DADO:317 |
| COLABS | 52 | DADO:318 |
| SEM_DIA | 0 | DADO:320 |
| FORA_RECORTE_CONTEXTUAL | 0 | DADO:321 |
| TOTAL_ERROS | 0 | DADO:322 |

Nenhum erro reportado torna os números pedidos impossíveis de afirmar; há campos de snapshot não verificados, discriminados abaixo. (DADO:322; DADO:15–137, filtrando `delta_relogio_min > 30`.)

2. **Das 31 perguntas acima30**

| Marco | DENTRO dos 90 min | FORA | NÃO_VERIFICADO |
|---|---:|---:|---:|
| Cadastrado | **23** | **8** | **0** |
| Snapshot | **7** | **1** | **23** |

Fontes da contagem pelo cadastro:

- **23 dentro:** DADO:15–17, 32–34, 42–43, 56, 59, 71–72, 82, 101–103, 105–109, 115, 130.
- **8 fora:** DADO:51, 54, 57, 113, 122, 128–129, 137.
- **0 não verificado:** todas essas 31 linhas têm classificação cadastral preenchida. (Mesmas linhas.)

Pelo snapshot, as **7 dentro** são as pks **49876, 50079, 51892, 52250, 53443, 53847 e 54585**; a **1 fora** é a pk **54584**; as outras **23** têm `NAO_VERIFICADO`. (DADO:33–34, 71, 82, 103, 115, 129–130; demais linhas acima30 enumeradas acima.)

**Retratadas: 0** entre as 31 perguntas acima30. (DADO:15–137, mesmas 31 linhas filtradas.)

3. **Colaboradores e dia-colab distintos, considerando somente acima30**

| Classificação pelo cadastro | Colabs | Dia-colab | Fonte |
|---|---:|---:|---|
| Ao menos uma pergunta dentro | **9** | **15** | DADO:214, 216, 229, 248–249, 251, 254–255, 282, 290, 312–316 |
| Somente perguntas fora | **2** | **6** | DADO:169, 183; 230–232, 256, 283, 289 |

Os **2 colaboradores somente fora** são **col301 e col707**. **col873 e col909** têm perguntas acima30 dentro e fora em dias diferentes: entram nos 9 colaboradores com alguma dentro, enquanto seus dias somente fora entram nos 6 dia-colab. (DADO:169, 183, 282–283, 289–290.)

4. **Pergunta pk 50833**

Sim: **col707**, batida pk **152915**, dia de turno **27/10/2026**; resposta **11:10**, batida local **2026-10-27T16:00:00-03:00**, marco cadastrado **16:00**, delta **290 minutos**, `dentro90_cadastro=0`: **FORA**. (DADO:51.)

5. **Distribuição do delta entre acima30**

Faixas sem sobreposição, incluindo o limite superior:

| Faixa em minutos | Perguntas | Fonte |
|---|---:|---|
| 30 < delta ≤ 60 | **22** | Linhas dentro do cadastro listadas no item 2, exceto DADO:82 |
| 60 < delta ≤ 120 | **3** | DADO:82 — 63 min; DADO:54, 57 — 120 min |
| 120 < delta ≤ 240 | **5** | DADO:113, 122, 128–129, 137 — 180 min |
| delta > 240 | **1** | DADO:51 — 290 min |

## 12/11 13:15 · sessao 50160a1d8a3a · - · - · origem=Ronald (tmux)

Conferência dos itens 2, 3 e 5:

- **Cadastro:** 23 DENTRO, 8 FORA, 0 NÃO_VERIFICADO. DENTRO sustentado pelas linhas R com pergunta_pk 49126, 49246, 49247, 49875, 49876, 50079, 50503, 50504, 51108, 51132, 51892, 51923, 52250, 53440, 53442, 53443, 53446, 53449, 53450, 53453, 53454, 53847, 54585. FORA sustentado pelas linhas R com pergunta_pk 50833, 51086, 51111, 53641, 54017, 54583, 54584, 55094.
- **Snapshot:** 7 DENTRO — linhas R com pergunta_pk 49876, 50079, 51892, 52250, 53443, 53847, 54585; 1 FORA — linha R com pergunta_pk 54584; 23 NÃO_VERIFICADO — demais linhas acima30 enumeradas no primeiro item.
- **Retratadas:** 0 nas 31 linhas acima30 enumeradas; todas têm retratada=0.

**Universos distintos entre perguntas acima30:**

| Cadastro | Colabs | Dia-colab |
|---|---:|---:|
| Ao menos uma DENTRO | 9 | 15 |
| Somente FORA | 2 | 6 |

Suporte para DENTRO, por linhas R acima30 e respectivas linhas S COLAB_DIA:

- col040: 2026-11-04.
- col055: 2026-10-25.
- col263: 2026-10-31.
- col502: 2026-10-24 e 2026-10-28.
- col516: 2026-10-22.
- col569: 2026-10-23 e 2026-10-30.
- col873: 2026-10-26.
- col909: 2026-11-05.
- col993: 2026-10-24, 2026-10-26, 2026-10-28, 2026-10-30 e 2026-11-01.

Suporte para SOMENTE FORA por colaborador: col707 (R 50833) e col301 (R 53641, 54017, 55094). col873 e col909 têm acima30 DENTRO em outros dias, portanto não entram como colaboradores “só fora” (R 50503, 50504, 54585).

Os 6 dia-colab somente FORA são col707/2026-10-27; col873/2026-10-28; col301/2026-11-03, 2026-11-04 e 2026-11-07; col909/2026-11-03 (linhas R 50833, 51086, 51111, 53641, 54017, 55094, 54583, 54584).

**Faixas sem sobreposição, com limite superior incluído:**

| Faixa | Perguntas | Suporte |
|---|---:|---|
| 30 < delta ≤ 60 | 22 | As linhas DENTRO acima, exceto R 52250 |
| 60 < delta ≤ 120 | 3 | R 52250=63; R 51086 e 51111=120 |
| 120 < delta ≤ 240 | 5 | R 53641, 54017, 54583, 54584, 55094=180 |
| delta > 240 | 1 | R 50833=290 |

## 12/11 13:10 · sessao e93ae91a7d8b · S139-RESPEITA-A-RESPOSTA · censo · origem=Code

**A. A S139 troca a hora antes da escrita; os leitores do slot herdam essa troca.** O trecho executável começa com `resposta_colab` e, para `intervalo_saida`/`intervalo_volta`, substitui-a pela hora de `_marco_faltante`, quando encontrada. Não há nesse trecho uma condição adicional sobre “intervalo fixo” nem uma verificação do envelope. (`app/chamados/services/materializacao.py:409-426`)

As referências `:342` e `:712` indicadas na pergunta são de **`chamados/juizes.py`**, onde estão os comentários sobre essa derivação. (`app/chamados/juizes.py:342`; `app/chamados/juizes.py:712`)

| Papel | Quem faz | Evidência |
|---|---|---|
| Resolve a hora e o tipo | `_ts_tipo_do_plantio` | `app/chamados/services/materializacao.py:333-428` |
| Busca o marco cobrado | `_marco_faltante` → `entrada_de_marco`: motivo primeiro; fallback por tipo somente no formato legado | `app/chamados/juizes.py:523-580` |
| Solicita o plantio | `_materializar_resposta_hora`, depois de derivar e procurar reuso | `app/chamados/services/materializacao.py:852-877` |
| Grava `Batida.timestamp` | `registrar_batida` → `Batida.objects.create` | `app/chamados/services/materializacao.py:933-940`; `app/ponto/registro_batida.py:157-159` |
| Grava a relação pergunta→batida | `anotar(... pergunta_origem=p)`, tanto para nova quanto para reusada | `app/chamados/services/materializacao.py:947-956` |
| Grava resposta e instante derivado na trilha | `_log_auditoria_materializacao`, ação `materializar_batida` | `app/chamados/services/materializacao.py:957-978` |

O campo persistido é **`Batida.pergunta_origem`**, reverso **`PerguntaDisputa.batidas_plantadas`**; não confundir com `PerguntaDisputa.batida_origem`, que identifica a batida sobre a qual outra família de perguntas pergunta. (`app/ponto/models.py:40-44`; `app/chamados/models.py:2437-2442`)

Os leitores diretos da derivação são estes:

| Leitor | Consequência |
|---|---|
| `estado_aplicacao_pergunta` | Calcula aplicação, reuso, paridade e horário anunciado na interface pelo instante derivado. (`app/chamados/services/materializacao.py:450-465`) |
| `_slot_esperado` | **Hoje chama o mesmo derivador**; o docstring “Reproduz” é anterior ao corpo que delega. (`app/chamados/juizes.py:342`; `app/chamados/juizes.py:369-374`) |
| `_pergunta_materializada_de_fato` | Confere as batidas vinculadas contra esse slot; a conferência aceita a janela de ±20 minutos. (`app/chamados/juizes.py:752-761`; `app/chamados/juizes.py:250-279`) |
| `CargaDaVida` | Prepara/cacheia os slots usados pelo juiz. (`app/chamados/services/carga_pergunta_viva.py:113-126`) |
| `_explicar_validada` | Busca batida perto do slot e escreve “validada como” usando sua hora. (`app/chamados/services/explicador.py:29-42`; `app/chamados/services/explicador.py:58-60`) |
| `classe_do_lote` | Confere conflito perto do slot que o plantio usaria. (`app/chamados/services/validacao.py:869-876`) |
| `vigia_carimbo.classificar` | Classifica slot não derivável, ausência de plantio ou plantio fora do slot. (`app/chamados/services/vigia_carimbo.py:105-113`) |

O juiz de materialização também alimenta o veredito lavrado, o raio-X, o explicador e a conferência posterior ao plantio. (`app/chamados/veredito_lavrado.py:263`; `app/chamados/services/raiox.py:187`; `app/chamados/services/explicador.py:93`; `app/chamados/services/materializacao.py:1239`)

**Outra cópia executável da substituição resposta→marco não foi localizada neste censo; exaustividade sobre todo consumidor indireto de `timestamp`: [nao verificado].** Os caminhos acima compartilham o derivador. (`app/chamados/services/materializacao.py:450`; `app/chamados/services/materializacao.py:852`; `app/chamados/juizes.py:372`)

No esmeril semântico:

- `resposta_colab` conserva a declaração; a substituição acontece na variável `_hora_fonte`, não na resposta persistida. A trilha grava os dois valores separadamente. (`app/chamados/services/materializacao.py:412-419`; `app/chamados/services/materializacao.py:969-977`)
- `Batida.timestamp` recebe o instante materializado, que nesse ramo pode ser o marco. O pareador consome esse campo como instante da batida. (`app/ponto/registro_batida.py:157-159`; `app/ponto/turnos.py:1217`; `app/ponto/turnos.py:1357`)
- Há textos que chamam esse slot de “hora respondida” ou “slot da resposta”, embora ele possa conter a hora do marco. (`app/chamados/juizes.py:663-667`; `app/chamados/services/explicador.py:31`; `app/chamados/services/vigia_carimbo.py:17-20`)
- `delta_da_resposta` e `motivo_implausivel_validacao` preservam a distinção: leem resposta e marco separadamente. (`app/chamados/juizes.py:1846-1868`; `app/chamados/juizes.py:1972-1983`)
- A reconciliação da grade reconhece a dependência: evita trocar o marco sob resposta viva já dada porque a materialização lê `marcos_faltantes`. (`app/ponto/management/commands/reconciliar_grade.py:437-440`; `app/ponto/management/commands/reconciliar_grade.py:460-462`)

**B. O caso protegido era uma volta de intervalo declarada no horário da saída do intervalo.** O teste fundador descreve volta esperada às `03:00`, resposta `02:00`, e exige impedir `E@02:00` no slot da saída. A fixture valida essa resposta e exige `E@03:00`, expressamente diferente de `02:00`. (`app/chamados/tests/test_s139.py:15-18`; `app/chamados/tests/test_s139.py:40-56`)

“Marco oposto” significa, nesse caso, jornada `19→07`, intervalo `02:00/S→03:00/E`: uma `E` próxima das `02:00` ocupa temporalmente o marco `S`. Outro teste reproduz a figura com `E@02:05` e exige a classificação `batida_retro_invertida`. (`app/chamados/tests/test_auditar_disputas_padroes.py:117-150`)

O dano diretamente demonstrado é **tipo `E` no horário do marco `S`**. Consequência financeira ou quebra específica de pareamento pela retirada da S139: **[nao verificado]**.

A O306 manteve explicitamente a S139: resposta `13:05` continua virando `13:00` quando esse é o marco do próprio motivo. Corrigiu outra falha: receber o marco de um motivo vizinho, deixando o marco perguntado vazio. (`app/chamados/tests/test_o306_marco_do_motivo_nao_e_do_vizinho.py:146-153`; `app/chamados/juizes.py:555-575`)

Commit, autoria, data e corte específico do nascimento da S139 por `git log/blame`: **[nao verificado]**. As consultas Git falharam porque esta cópia não foi reconhecida como repositório. **[nao verificado]** A L-032 e a L-099 locais não identificam o nascimento da S139; estabelecem, respectivamente, reconhecimento do intervalo dentro do envelope e coerência com o cadastro existente. (`app/docs/LEIS.md:80`; `app/docs/LEIS.md:122`)

**C. Os 90 minutos estão dentro de `parear_turnos`; não existe uma função única pronta com a assinatura perguntada.** O literal aparece no predicado interno `_absorvivel`, na guarda de continuidade e na absorção por marco. (`app/ponto/turnos.py:1251`; `app/ponto/turnos.py:1352`; `app/ponto/turnos.py:1358`)

A função reutilizável é **`_dist_marco(ts, marcos)`**: devolve a menor distância local em minutos, considerando dia anterior, mesmo dia e seguinte. Não recebe colaborador, não resolve o marco e não retorna um booleano. (`app/ponto/turnos.py:326-340`)

A composição disponível é:

| Pergunta | Código existente |
|---|---|
| Qual vínculo responde pelo colaborador/dia? | `escala_vigente` → `vinculo_do_dia`. (`app/escala/servico_jornada.py:15-66`) |
| Quais os marcos cadastrados desse dia? | `ec.marcos_do_dia`: DNA congelado, com fallback para template. (`app/escala/models.py:1098-1133`) |
| Qual marco corresponde ao motivo? | `MARCO_DA_PERGUNTA`: saída→`hii`, volta→`hfi`. (`app/chamados/juizes.py:922-926`) |
| Está no raio de 90 minutos desse marco? | `_dist_marco(...) <= 90`. (`app/ponto/turnos.py:326-340`; `app/ponto/turnos.py:1251`) |

**Estar nesse raio não prova absorção pelo pareador.** A absorção também compara a proximidade das bordas e depende do turno corrente e de seu teto de continuidade. O pareador ainda refina marcos pelo DNA na data civil local da batida. (`app/ponto/turnos.py:1229-1251`; `app/ponto/turnos.py:1349-1370`)

`intervalo_cadastrado` responde a duração cadastrada; não é a função de pertencimento ao envelope. (`app/ponto/juiz_batida.py:106-107`)

**D. Script para colar no shell Django, dentro do envelope de leitura.** Ele usa a **relação persistida**, incluindo reuso, sem consultar a trilha. Essa relação é escrita nos dois caminhos; logo, sua contagem não significa “batidas novas criadas”. Também pode ser reassociada a outra pergunta pelo escritor. (`app/chamados/services/materializacao.py:865-876`; `app/chamados/services/materializacao.py:952-956`)

O script imprime separadamente:

- raio do marco **cadastrado** do colaborador/dia;
- raio do marco **cobrado no snapshot** do chamado;
- absorção completa pelo pareador como `NAO_VERIFICADO`, pelas dependências descritas em C. (`app/escala/models.py:1098-1133`; `app/chamados/juizes.py:523-580`; `app/ponto/turnos.py:1349-1370`)

A diferença é a menor distância entre horas locais, inclusive pela meia-noite, calculada por `_dist_marco`; não atribui uma data inventada à resposta `HH:MM`. (`app/ponto/turnos.py:326-340`; `app/chamados/models.py:2448`)

`S total` conta perguntas distintas; `relacoes` conta batidas vinculadas. Havendo várias batidas para uma pergunta, `acima30` significa que pelo menos uma ultrapassou 30 minutos. Isso fica explícito no código abaixo. Execução Django e números resultantes: **[nao verificado]**.

```python
from collections import defaultdict, Counter
from datetime import date, datetime, time
from django.db import connection
from django.db.models import Q
from django.utils import timezone
from chamados.juizes import MARCO_DA_PERGUNTA, _marco_faltante
from escala.servico_jornada import escala_vigente
from ponto.models import Batida
from ponto.turnos import _dist_marco

INICIO = date(2026, 10, 21)
# Raio existente: ponto/turnos.py:1251,1358.
RAIO_MIN = 90
TOTAL_ERROS = 0
cache = {}
grupos = defaultdict(lambda: defaultdict(set))
links_por_pergunta = Counter()
fora_recorte = set()
sem_dia = set()

def emitir(*campos):
    print("\t".join(str(x) for x in campos))

def erro(etapa, pk, exc):
    global TOTAL_ERROS
    TOTAL_ERROS += 1
    # Nunca imprimir mensagem da excecao, objetos ou resposta bruta.
    emitir("S", "ERRO", etapa, pk, type(exc).__name__)

def tentar(etapa, pk, func):
    try:
        return func()
    except Exception as exc:
        erro(etapa, pk, exc)
        return None

def apenas_select(execute, sql, params, many, context):
    if not str(sql).lstrip().upper().startswith("SELECT"):
        raise RuntimeError("SQL_nao_SELECT")
    return execute(sql, params, many, context)

def hora(texto):
    hh, mm = texto.strip().split(":")
    return time(int(hh), int(mm))

def marco_cadastro(cid, dia, chave):
    ec = escala_vigente(cid, dia)
    if ec is None:
        return None
    # Contrato: escala/models.py:1098-1133.
    return dict(zip(("hi", "hf", "hii", "hfi"),
                    ec.marcos_do_dia(dia)))[chave]

def raio(resp, marco, dia):
    if resp is None or marco is None:
        return None
    ts = timezone.make_aware(datetime.combine(dia, resp))
    return _dist_marco(ts, [marco]) <= RAIO_MIN

def preparar(p):
    if p.pk in cache:
        return cache[p.pk]
    dia = p.data_evento
    if dia is None:
        texto = ((p.disputa.contexto or {}).get(p.motivo) or {}).get("data")
        dia = tentar("dia_contexto", p.pk, lambda: date.fromisoformat(texto))
    if dia is None:
        sem_dia.add(p.pk)
        cache[p.pk] = None
        return None
    if dia < INICIO:
        fora_recorte.add(p.pk)
        cache[p.pk] = None
        return None
    cid = p.disputa.colaborador_id
    tipo, chave = MARCO_DA_PERGUNTA[p.motivo]
    resp = tentar("hora_resposta", p.pk, lambda: hora(p.resposta_colab))
    mc = tentar("marco_cadastro", p.pk,
                lambda: marco_cadastro(cid, dia, chave))
    def snapshot():
        texto = _marco_faltante(p.chamado_destino, p.motivo, tipo)
        return hora(texto) if texto else None
    ms = tentar("marco_snapshot", p.pk, snapshot)
    dc = tentar("raio_cadastro", p.pk, lambda: raio(resp, mc, dia))
    ds = tentar("raio_snapshot", p.pk, lambda: raio(resp, ms, dia))
    cache[p.pk] = (cid, dia, resp, mc, ms, dc, ds)
    return cache[p.pk]

def fmt_hora(h):
    return h.strftime("%H:%M") if h is not None else "NAO_VERIFICADO"

def flag(v):
    return "NAO_VERIFICADO" if v is None else int(v)

def registrar(chave, p, b, delta, dc, ds):
    g = grupos[chave]
    g["total"].add(p.pk)
    g["relacoes"].add(b.pk)
    if delta is None:
        g["delta_nv"].add(p.pk)
    else:
        if delta > 0:
            g["hora_diferente"].add(p.pk)
        if delta > 30:
            g["acima30"].add(p.pk)
    g["raio_cadastro_nv" if dc is None else
      "dentro90_cadastro" if dc else "fora90_cadastro"].add(p.pk)
    g["raio_snapshot_nv" if ds is None else
      "dentro90_snapshot" if ds else "fora90_snapshot"].add(p.pk)

emitir("S", "R_COLUNAS", "col", "pergunta_pk", "batida_pk", "dia_turno",
       "hora_respondida", "batida_local", "delta_relogio_min",
       "marco_cadastro", "dentro90_cadastro", "marco_snapshot",
       "dentro90_snapshot", "retratada", "absorcao_pareador")
try:
    with connection.execute_wrapper(apenas_select):
        qs = (Batida.objects.filter(
            pergunta_origem__motivo__in=("intervalo_saida", "intervalo_volta"),
            pergunta_origem__tipo_resposta="hora",
            pergunta_origem__respondida_em__isnull=False,
            pergunta_origem__validada_em__isnull=False)
            .filter(Q(pergunta_origem__data_evento__gte=INICIO) |
                    Q(pergunta_origem__data_evento__isnull=True))
            .select_related("pergunta_origem__disputa",
                            "pergunta_origem__chamado_destino")
            .order_by("pergunta_origem_id", "pk"))
        for b in qs.iterator(chunk_size=1000):
            try:
                p = b.pergunta_origem
                dados = preparar(p)
                if dados is None:
                    continue
                cid, dia, resp, mc, ms, dc, ds = dados
                if b.colaborador_id != cid:
                    raise ValueError("colaborador_divergente")
                links_por_pergunta[p.pk] += 1
                local = timezone.localtime(b.timestamp)
                delta = (tentar("delta_batida_resposta", p.pk,
                                lambda: _dist_marco(b.timestamp, [resp]))
                         if resp is not None else None)
                emitir("R", f"col{cid:03d}", p.pk, b.pk, dia.isoformat(),
                       fmt_hora(resp), local.isoformat(),
                       "NAO_VERIFICADO" if delta is None else f"{delta:.3f}",
                       fmt_hora(mc), flag(dc), fmt_hora(ms), flag(ds),
                       int(b.retratada_em is not None), "NAO_VERIFICADO")
                for chave in (("TOTAL",), ("COLAB", f"col{cid:03d}"),
                              ("COLAB_DIA", f"col{cid:03d}", dia.isoformat())):
                    registrar(chave, p, b, delta, dc, ds)
            except Exception as exc:
                erro("item", b.pk, exc)
except Exception as exc:
    erro("consulta", 0, exc)

metricas = ("total", "relacoes", "hora_diferente", "acima30", "delta_nv",
            "dentro90_cadastro", "fora90_cadastro", "raio_cadastro_nv",
            "dentro90_snapshot", "fora90_snapshot", "raio_snapshot_nv")
emitir("S", "RESUMO_COLUNAS", *metricas)
if ("TOTAL",) not in grupos:
    grupos[("TOTAL",)]
for chave in sorted(grupos):
    emitir("S", *chave, *(len(grupos[chave][m]) for m in metricas))
emitir("S", "COLABS", sum(k[0] == "COLAB" for k in grupos))
emitir("S", "PERGUNTAS_COM_MULTIPLAS_BATIDAS",
       sum(n > 1 for n in links_por_pergunta.values()))
emitir("S", "SEM_DIA", len(sem_dia))
emitir("S", "FORA_RECORTE_CONTEXTUAL", len(fora_recorte))
emitir("S", "TOTAL_ERROS", TOTAL_ERROS)
```

Os números `506/1282`, `376`, `133` e `59` ficam como informação do anexo, sem confirmação independente: **[nao verificado]**. Quantos dos 59 ultrapassam 30 minutos e quantos estão no raio cadastrado: **[nao verificado]**. O anexo também não fornece os marcos cadastrados necessários para confirmar que o caso 50833 é o contraexemplo “dentro do envelope”: **[nao verificado]**.

## 12/11 13:05 · sessao 529fdf18fac9 · - · - · origem=Ronald (tmux)

C — o número e o juiz real:

- A L-032 aponta o envelope de 90 minutos em `app/docs/LEIS.md:80`.
- O número **90 não é constante compartilhada**: aparece literalmente dentro de `parear_turnos`, no predicado interno `_absorvivel` (`app/ponto/turnos.py:1251`), na guarda do teto (`app/ponto/turnos.py:1352`) e na absorção por marco (`app/ponto/turnos.py:1358`).
- A função reutilizável real é `_dist_marco(ts, marcos)`: calcula a menor distância em minutos entre o instante local e cada hora de marco, considerando dia anterior, mesmo dia e seguinte. Retorna `None` sem marco; **não recebe colaborador nem dia da jornada e não responde um booleano** (`app/ponto/turnos.py:326`–`340`).
- O predicado completo de absorção exige distância ao intervalo `<=90` e intervalo tão próximo quanto a borda do turno; no laço exige também turno corrente e respeita o teto de continuidade. Portanto, estar no raio de X não prova que a resposta seria absorvida como intervalo numa jornada real (`app/ponto/turnos.py:1246`–`1251`; `app/ponto/turnos.py:1349`–`1370`).
- **Não há função única pronta** `(colab, dia, marco X, resposta) -> dentro_envelope`. Essa pergunta é composta por resolução de vínculo/marcos, distância e guardas internas do pareador (`app/escala/servico_jornada.py:15`–`66`; `app/ponto/turnos.py:1229`–`1251`; `app/ponto/turnos.py:1349`–`1370`).

Fontes de marco:

- Vínculo de um colaborador/dia: `escala_vigente`, que carrega células/vínculos e chama `vinculo_do_dia` (`app/escala/servico_jornada.py:15`–`66`).
- Marcos desse vínculo/dia: `ec.marcos_do_dia(dia)`, DNA congelado da célula com fallback para template (`app/escala/models.py:1098`–`1133`).
- Marcos efetivamente usados pelo pareador: template por `marcos_do_template` (`app/ponto/turnos.py:1504`–`1514`), refinado por DNA usando a **data civil local da batida** em `_ivm_de`/`_bm_de` (`app/ponto/turnos.py:1229`–`1244`). Com segundo intervalo, o chamador deixa de passar mapa DNA para preservar o segundo par do template (`app/ponto/turnos.py:1984`–`1989`).
- `intervalo_cadastrado` responde **duração cadastrada**, não pertencimento ao envelope (`app/ponto/juiz_batida.py:106`–`146`).

D — recomendação para a sonda:

Use o marco X **cobrado na pergunta**, pelo leitor puro `entrada_de_marco(p.chamado_destino, p.motivo, tipo=None)`; ele resolve por motivo no snapshot `marcos_faltantes`. A casca `_marco_faltante(ch,motivo,tipo)` devolve só a hora e admite fallback por tipo apenas no formato legado (`app/chamados/juizes.py:523`–`580`). Teste o raio chamando a função real:

```python
dist = _dist_marco(ts_resposta, [hora_marco])
dentro_raio_marco_cobrado = None if dist is None else dist <= 90
```

Esse resultado precisa ser descrito como **“raio de 90 minutos do marco cobrado no snapshot”**, e não “classificação plena do pareador”; o último resultado depende de contexto e guardas acima (`app/ponto/turnos.py:1246`–`1251`; `app/ponto/turnos.py:1349`–`1370`).

Não usar `grade_da_celula` para identificar `hii`/`hfi` apenas pelo tipo: ela entrega `{'tipo','hora'}` das lâmpadas, sem chave de marco ou motivo (`app/escala/services/leitor_celula.py:148`; `app/escala/services/leitor_celula.py:413`–`418`). Identificação só por E/S é precisamente a ambiguidade entrada/volta e intervalo/saída descrita em `_marco_faltante` (`app/chamados/juizes.py:526`–`531`). A grade faz leituras no corpo, mas sua régua é a ATA, uma fonte diferente do DNA/template usado no pareador (`app/escala/services/leitor_celula.py:361`–`362`; `app/escala/services/leitor_celula.py:413`–`418`).

## 12/11 13:05 · sessao 5af3ff980af8 · - · - · origem=Ronald (tmux)

Item A — achados da cópia, somente leitura:

- **Correção de caminho:** as referências `:342` e `:712` pertencem a `app/chamados/juizes.py`; em `app/core/juizes.py:342` está `SEM_JUIZ` e em `app/core/juizes.py:712` está `_TT4`. (`app/chamados/juizes.py:341`, `app/chamados/juizes.py:712`, `app/core/juizes.py:342`, `app/core/juizes.py:712`)

- **Derivador da hora:** `_ts_tipo_do_plantio` começa com `p.resposta_colab`; para os dois motivos de intervalo, pede `_marco_faltante` e, havendo hora, substitui a resposta pelo marco. Combina a hora com o dia e aplica overnight. O comentário “horário FIXO” não corresponde a uma condição explícita sobre `intervalo_modo` nesse trecho: o discriminador executável é motivo de intervalo + marco encontrado. (`app/chamados/services/materializacao.py:409`, `app/chamados/services/materializacao.py:413`, `app/chamados/services/materializacao.py:419`, `app/chamados/services/materializacao.py:426`)

- **Fonte do marco:** `_marco_faltante` delega a `entrada_de_marco`, que lê `ch.contexto_json['marcos_faltantes']`, casa primeiro por motivo; havendo algum motivo na lista, não cai no tipo de outro marco; somente lista inteiramente legada admite fallback por tipo. (`app/chamados/juizes.py:523`, `app/chamados/juizes.py:535`, `app/chamados/juizes.py:549`, `app/chamados/juizes.py:551`, `app/chamados/juizes.py:576`, `app/chamados/juizes.py:578`)

- **Quem escreve:** `_materializar_resposta_hora` chama o derivador e consulta reuso em ±20 minutos. Sem reuso, chama `registrar_batida(colaborador, ts, tipo, 'disputa_s84_retro', ...)`; a porta efetua `Batida.objects.create(timestamp=timestamp, ...)`. (`app/chamados/services/materializacao.py:852`, `app/chamados/services/materializacao.py:865`, `app/chamados/services/materializacao.py:933`, `app/ponto/registro_batida.py:157`)

- **Relação persistida:** nova ou reusada, recebe `pergunta_origem=p` via `anotar`; o FK reverso é `batidas_plantadas`. Portanto, esse vínculo inclui reuso e sozinho não prova que uma batida nova nasceu. A trilha `materializar_batida` também é emitida após os dois caminhos e grava `batida_pk`, timestamp pretendido, `pergunta_pk` e `resposta_colab`. (`app/chamados/services/materializacao.py:871`, `app/chamados/services/materializacao.py:952`, `app/chamados/services/materializacao.py:954`, `app/chamados/services/materializacao.py:957`, `app/chamados/services/materializacao.py:967`, `app/chamados/services/materializacao.py:976`, `app/ponto/models.py:40`)

- **Não localizei segunda cópia executável dessa substituição** no inventário de chamadas: `_ts_tipo_do_plantio` serve o plantio, o estado da UI e `_slot_esperado`. Este último **hoje delega** ao mesmo derivador; o docstring “Reproduz” ficou, mas o corpo declara expressamente que não reproduz mais. (`app/chamados/services/materializacao.py:450`, `app/chamados/services/materializacao.py:852`, `app/chamados/juizes.py:342`, `app/chamados/juizes.py:369`, `app/chamados/juizes.py:372`)

Leitores diretos desse slot, portanto herdando a hora do marco:

| Leitor | Uso | Evidência |
|---|---|---|
| `estado_aplicacao_pergunta` | Estado do modal; reuso, paridade e promessa de horário a registrar | `app/chamados/services/materializacao.py:450`, `app/chamados/services/materializacao.py:453`, `app/chamados/services/materializacao.py:458`, `app/chamados/services/materializacao.py:463` |
| `_pergunta_materializada_de_fato` | Compara batidas ligadas à hora do slot; fallback de acervo busca a mesma janela | `app/chamados/juizes.py:752`, `app/chamados/juizes.py:755`, `app/chamados/juizes.py:760`, `app/chamados/juizes.py:812` |
| `_plantio_na_hora_do_slot` | Batida ligada deve estar em ±20 minutos do slot; para hora, aceita dia do turno ou seguinte | `app/chamados/juizes.py:267`, `app/chamados/juizes.py:276`, `app/chamados/juizes.py:278` |
| `CargaDaVida` | Cache de slots e preparação das testemunhas do C6 | `app/chamados/services/carga_pergunta_viva.py:113`, `app/chamados/services/carga_pergunta_viva.py:119`, `app/chamados/services/carga_pergunta_viva.py:126` |
| `_explicar_validada` | Procura batida em ±20 minutos e diz “validada como …” usando a hora derivada | `app/chamados/services/explicador.py:29`, `app/chamados/services/explicador.py:34`, `app/chamados/services/explicador.py:58`, `app/chamados/services/explicador.py:60` |
| `classe_do_lote` | Confere conflito próximo do slot de plantio | `app/chamados/services/validacao.py:869`, `app/chamados/services/validacao.py:872`, `app/chamados/services/validacao.py:875` |
| `vigia_carimbo.classificar` | Distingue slot não derivável / plantio fora do slot / sem plantio | `app/chamados/services/vigia_carimbo.py:105`, `app/chamados/services/vigia_carimbo.py:110`, `app/chamados/services/vigia_carimbo.py:113` |

**Leitores transitivos do juiz de materialização:** veredito lavrado, raiox, explicador e fechamento/materialização voltam a esse juiz. (`app/chamados/veredito_lavrado.py:263`, `app/chamados/services/raiox.py:187`, `app/chamados/services/explicador.py:93`, `app/chamados/services/materializacao.py:1239`, `app/chamados/services/materializacao.py:1433`, `app/chamados/juizes.py:1473`)

**Esmeril semântica:** o campo bruto `resposta_colab` permanece a fala; `Batida.timestamp` pode conter o marco, e o slot esperado também passa a conter o marco. Assim, não é uma mistura dentro de `resposta_colab`; é uma divergência entre declaração preservada e instante materializado. O próprio código guarda ambos na trilha. (`app/chamados/services/materializacao.py:412`, `app/chamados/services/materializacao.py:416`, `app/chamados/services/materializacao.py:969`, `app/chamados/services/materializacao.py:977`)

Há frases que chamam o slot de “hora respondida” embora o cálculo possa usar o marco: docstring do juiz de materialização, explicador e descrição do vigia. Isso sustenta o descompasso textual e operacional; uma intenção adicional de seus autores é [nao verificado]. (`app/chamados/juizes.py:663`, `app/chamados/juizes.py:667`, `app/chamados/services/explicador.py:31`, `app/chamados/services/vigia_carimbo.py:17`)

**Leitores que preservam a distinção:** `delta_da_resposta` lê a resposta bruta separadamente do marco; `motivo_implausivel_validacao` faz o mesmo. Ambos procuram marco no chamado e depois na grade, sem substituir a resposta. O fio também apresenta `esperada_prevista` separadamente e calcula o estado da aplicação via derivador. (`app/chamados/juizes.py:1846`, `app/chamados/juizes.py:1850`, `app/chamados/juizes.py:1863`, `app/chamados/juizes.py:1972`, `app/chamados/juizes.py:1977`, `app/chamados/services/fio.py:325`, `app/chamados/services/fio.py:331`, `app/chamados/services/fio.py:336`)

**Testes da mesma política:** `test_s139.py` exige resposta `02:00` virar `E@03:00`, rejeitando expressamente `02:00`. O teste posterior O306 mantém S139: resposta `13:05` deve produzir `13:00` quando é o marco do próprio motivo. (`app/chamados/tests/test_s139.py:40`, `app/chamados/tests/test_s139.py:45`, `app/chamados/tests/test_s139.py:51`, `app/chamados/tests/test_s139.py:53`, `app/chamados/tests/test_s139.py:55`, `app/chamados/tests/test_o306_marco_do_motivo_nao_e_do_vizinho.py:146`, `app/chamados/tests/test_o306_marco_do_motivo_nao_e_do_vizinho.py:151`, `app/chamados/tests/test_o306_marco_do_motivo_nao_e_do_vizinho.py:153`)

**Outro consumidor explícito do caráter mutável do marco:** reconciliar grade evita reescrever a cobrança enquanto houver fio vivo respondido, porque a materialização lê `marcos_faltantes`; comentário declara que trocar o marco sob resposta já dada a orfanaria. (`app/ponto/management/commands/reconciliar_grade.py:437`, `app/ponto/management/commands/reconciliar_grade.py:460`)

## 12/11 13:05 · sessao 46494a3f7c2a · - · - · origem=Ronald (tmux)

B — O caso protegido está declarado em [app/chamados/tests/test_s139.py:15](/home/ronald/codex_ws/096ace9d0f9fe1a939f12e1f303bcb41ddb91e3a/app/chamados/tests/test_s139.py:15): volta esperada às 03:00; resposta livre 02:00; impedir uma `E@02:00` no horário da saída do intervalo. A fixture contém marco `03:00/E`, resposta `02:00` e validação; o teste exige batida `E`, diferente de `02:00`, exatamente `03:00` — [app/chamados/tests/test_s139.py:40](/home/ronald/codex_ws/096ace9d0f9fe1a939f12e1f303bcb41ddb91e3a/app/chamados/tests/test_s139.py:40), [app/chamados/tests/test_s139.py:46](/home/ronald/codex_ws/096ace9d0f9fe1a939f12e1f303bcb41ddb91e3a/app/chamados/tests/test_s139.py:46), [app/chamados/tests/test_s139.py:49](/home/ronald/codex_ws/096ace9d0f9fe1a939f12e1f303bcb41ddb91e3a/app/chamados/tests/test_s139.py:49).

“Cair no slot oposto” significa concretamente: jornada `19→07`, intervalo `02:00/S→03:00/E`; a batida `E@02:00` coincide com o marco `S`. O teste do auditor descreve esse caso e comprova detecção como `batida_retro_invertida` criando `E@02:05`; o detector anterior, limitado às bordas da jornada, não o encontrava — [app/chamados/tests/test_auditar_disputas_padroes.py:117](/home/ronald/codex_ws/096ace9d0f9fe1a939f12e1f303b41ddb91e3a/app/chamados/tests/test_auditar_disputas_padroes.py:117), [app/chamados/tests/test_auditar_disputas_padroes.py:140](/home/ronald/codex_ws/096ace9d0f9fe1a939f12e1f303bcb41ddb91e3a/app/chamados/tests/test_auditar_disputas_padroes.py:140). Consequência específica em pareamento, folha ou dinheiro causada pela retirada da S139: [nao verificado].

A S139 hoje não testa distância nem envelope: substitui a resposta pelo `_marco_faltante` quando há motivo de intervalo e marco disponível — [app/chamados/services/materializacao.py:409](/home/ronald/codex_ws/096ace9d0f9fe1a939f12e1f303bcb41ddb91e3a/app/chamados/services/materializacao.py:409).

A O306 preservou expressamente a regra para o marco do próprio motivo: resposta `13:05` continua produzindo `13:00` — [app/chamados/tests/test_o306_marco_do_motivo_nao_e_do_vizinho.py:146](/home/ronald/codex_ws/096ace9d0f9fe1a939f12e1f303bcb41ddb91e3a/app/chamados/tests/test_o306_marco_do_motivo_nao_e_do_vizinho.py:146). Corrigiu outra falha: fallback por tipo entregava marco de outro motivo, produzindo `08:00` para pergunta de `13:00`; o marco das 13:00 continuava vazio, a célula seguia `furo` e havia nova declaração — [app/chamados/juizes.py:555](/home/ronald/codex_ws/096ace9d0f9fe1a939f12e1f303bcb41ddb91e3a/app/chamados/juizes.py:555).

Commit, data, autoria e corte/lei específicos do nascimento da S139 por `git log/blame`: [nao verificado]. Raiz e `app/` não foram reconhecidas como repositório Git nas tentativas de consulta; nenhum arquivo foi criado. [nao verificado] [nao verificado]

A L-032 local exige reconhecer intervalo dentro do envelope apesar da diferença de horário, mas sua linha não cita S139 — [app/docs/LEIS.md:80](/home/ronald/codex_ws/096ace9d0f9fe1a939f12e1f303bcb41ddb91e3a/app/docs/LEIS.md:80). A L-099 exige coerência com o cadastro e encaminha divergências de cadastro/batida ao admin; tampouco cita a origem da S139 — [app/docs/LEIS.md:122](/home/ronald/codex_ws/096ace9d0f9fe1a939f12e1f303bcb41ddb91e3a/app/docs/LEIS.md:122).

## 12/11 12:57 · sessao 9d7727ee3e34 · AUDITORIA-DE-TRAVAS · verificador · origem=Code

**Parecer: atendimento parcial.** O cap foi implementado, mas permanecem a falta de recusa específica por ausência de entrada na tabela e a diferença entre teto da execução e prazo total do wrapper. (`bin/codex_pergunta.sh:89`, `:358`, `:383`, `:408`)

1. **Contrato, aval e furos apontados**

| Ponto | Resultado |
|---|---|
| Tetos e rebaixamento | Confere: construtor **1930**, verificador **894**, leitura/censo/sonda **1191**; override maior reduz `TIMEOUT` e emite “teto do papel”. (`contrato_item6.txt:3–4`; `bin/codex_pergunta.sh:89–92`) |
| Default sem override | Resolvido no código: variável **ausente ou vazia** usa o teto do papel. Isso altera o default anterior de 900 e escolhe uma das alternativas que o contrato deixara abertas. Aval específico dessa escolha: **[não verificado]**. (`diff_c8c3420f1.txt`, hunk `@@ -80,9 +80,16 @@`; `contrato_item6.txt:3–4`; `bin/codex_pergunta.sh:90`) |
| Domínio do override | Agora aceita somente `[1-9][0-9]*`: recusa zero, negativos, frações, sufixos e até `0600`. É uma restrição adicional não definida pelo contrato; “inteiro positivo” não explica a rejeição de zeros iniciais. (`contrato_item6.txt:4`; `bin/codex_pergunta.sh:91`) |
| Inteiro enorme | Passa pela guarda textual, mas pode exceder a faixa da comparação `-le`: produz diagnóstico do Bash antes da linha de rebaixamento. O cap continua aplicado, porém a promessa de **uma linha** no stderr não fica garantida nesse domínio. (`bin/codex_pergunta.sh:91–92`; `contrato_item6.txt:4`) |
| Mensagem do `flock -w` | Usa corretamente o `TIMEOUT` já limitado. Continua dizendo “ocupada há Ns” sem medir idade da ocupação e dá esse diagnóstico para qualquer falha do `flock`; esse comportamento foi preservado. (`bin/codex_pergunta.sh:383`; `diff_c8c3420f1.txt`, alterações do wrapper) |
| Papel sem entrada | **Nenhum dos cinco papéis atuais fica descoberto.** Entretanto, a tabela usa `*) TETO_S=1191`: acrescentar um papel somente à lista de aceitos concede default silencioso, sem exigir medida/entrada própria. A recusa contratada por ausência de medida não está materializada na tabela. (`contrato_item6.txt:4`; `bin/codex_pergunta.sh:84`, `:89`) |

Portanto, não certifico “o contrato e o aval, e só eles”: o núcleo do cap está entregue, mas o diff também fixa default e domínio antes indefinidos, enquanto mantém o fallback genérico da tabela. (`contrato_item6.txt:3–4`; `bin/codex_pergunta.sh:89–92`)

O `900` de `encadeado.sh` continua sendo outro parâmetro: espera por carimbos, com rc 2; sua preservação corresponde ao aval. (`aval_respostas_1211_1230.txt:3`; `bin/encadeado.sh:71`, `:84–96`)

2. **ESMERIL-SEMÂNTICA: quem escreve e quem lê**

O censo textual de `bin/`, incluindo `bin/tests/`, resulta nos seguintes escritores e leitores; produtores ou leitores dinâmicos fora dos sítios identificados são **[não verificado]**. (`bin/codex_pergunta.sh:90–92`; referências individuais abaixo)

| Variável | Quem escreve | Quem lê |
|---|---|---|
| `CODEX_TIMEOUT` | `20` no selo de limite; `20` no selo do disjuntor; `unset`, `3600`, `600` e `abc` no selo do wrapper. (`bin/tests/test_codex_limite.sh:24`; `bin/tests/test_disjuntor.sh:1213`; `bin/tests/test_codex_pergunta.sh:121`, `:458`, `:462`, `:467`) | O wrapper importa o ambiente para `TIMEOUT`. O selo encaminha esse ambiente pelo `_roda`. (`bin/codex_pergunta.sh:90`; `bin/tests/test_codex_pergunta.sh:132–137`) |
| `TIMEOUT` | O wrapper atribui override/default e depois aplica o cap. O mutante remove a atribuição do cap na cópia de teste. (`bin/codex_pergunta.sh:90`, `:92`; `bin/tests/test_codex_pergunta.sh:567`, `:583–585`) | Guarda de domínio; comparação e diagnóstico; espera e mensagem da trava; execução construtora; execução dos demais papéis. (`bin/codex_pergunta.sh:91`, `:92`, `:383`, `:408`, `:415`) |
| Referências textuais | O teste do disjuntor manipula literalmente o comando contendo `$TIMEOUT`; não calcula outro prazo. (`bin/tests/test_disjuntor.sh:1826`) | O mutador do selo lê o fonte e substitui a atribuição marcada. (`bin/tests/test_codex_pergunta.sh:567`, `:577–586`) |

Há dois usos documentados relevantes fora de `bin/`: **censo com 2400**, que nesta implementação seria reduzido a **1191**, e **construtor com 1800**, que permanece abaixo de 1930. São registros históricos, não prova de chamadores atualmente ativos nem da necessidade desses prazos. (`app/docs/RELATO.md:7756`, `:7782`, `:425`; `bin/codex_pergunta.sh:89`, `:92`)

`CODEX_TIMEOUT` **não recebe o valor limitado de volta**: pode continuar sendo `3600` no ambiente enquanto `TIMEOUT=1191` governa o wrapper. Um descendente que leia o primeiro como prazo efetivo: **[não verificado]**. (`bin/codex_pergunta.sh:90`, `:92`)

A unidade dos consumidores é segundos, mas há **dois orçamentos sucessivos**: a trava da resposta pode esperar um teto inteiro; depois o processo recebe outro teto inteiro. O construtor mantém ainda `-k 30`. Logo, 1930 segundos não é prazo total do lançador. (`bin/codex_pergunta.sh:6`, `:383`, `:408`, `:415`)

Antes disso, o gate bloqueia sem prazo e os slots repetem esperas de cinco segundos sem limite acumulado. A duração registrada começa em `T0C`, inclui preparação posterior, filas, trava e encerramento; não mede exclusivamente o intervalo sob `timeout`. (`bin/codex_pergunta.sh:259–267`, `:298–325`, `:358–370`, `:427`)

“Estourou = 124” também precisa desse escopo: a espera da resposta sai **3**, e o inventário construtor pode substituir inclusive um retorno 124 por **3**. O contador existente por papel conta `ini`, qualquer rc; não é o contador contratado de rc 124 em 24 horas. Meta `≤1` por papel: **[não verificado]**. (`contrato_item6.txt:4–5`; `bin/codex_pergunta.sh:383`, `:418`, `:427–440`; `bin/codex_limite_estado.py:71–79`)

3. **Casos novos, mutantes e fake timeout**

**Os dois mutantes têm mordida demonstrável pela leitura do código; execução efetiva permanece [não verificado].** O primeiro deixa `3600` no argumento, contrariando `1191`; o segundo deixa `abc` prosseguir até o cap e a chamada do Codex, contrariando rc 2 e ausência de captura exigidos por `_recusa`. (`bin/codex_pergunta.sh:91–92`; `bin/tests/test_codex_pergunta.sh:140–143`, `:458–460`, `:467`, `:567–568`)

`c_teto_papel` verifica três comportamentos: leitura `3600→1191`, verificador `600→600` e verificador sem override `→894`. `c_timeout_int` cobre somente `abc`. Não cobre os demais limites do domínio nem todos os tetos/defaults por papel. (`bin/tests/test_codex_pergunta.sh:455–467`)

**O fake desta cópia encaminha o valor ao timeout real:** grava `"$@"` e executa `/usr/bin/timeout "$@"`, sem transformação. Cada subcaso apaga previamente a captura, evitando argumento velho. (`bin/tests/test_codex_pergunta.sh:115–118`, `:458`, `:462`, `:464`)

A cobertura, porém, tem limites concretos:

- Apenas o primeiro subcaso exige rc 0; os de `600` e `894` verificam somente o argumento capturado. A mensagem é buscada em stdout+stderr mesclados, sem exigir stderr nem exatamente uma linha. (`bin/tests/test_codex_pergunta.sh:136`, `:459–465`)
- O Codex falso termina imediatamente: os casos não provam expiração, rc 124, escalada `-k 30`, nem aplicação do teto ao `flock`. (`bin/tests/test_codex_pergunta.sh:111`, `:455–467`; `bin/codex_pergunta.sh:383`, `:408`)
- Existe um falso verde possível além dos dois mutantes: separar `timeout "$TIMEOUT" true; codex ...` manteria a primeira linha capturada correta, deixando o Codex fora do relógio. Os casos novos não verificam qual comando recebe o timeout. Execução desse mutante adicional: **[não verificado]**. (`bin/tests/test_codex_pergunta.sh:456`, `:460`, `:463`, `:465`; chamada atual correta em `bin/codex_pergunta.sh:415`)

O selo integral não foi executado porque cria, altera e apaga arquivos, inclusive a cópia usada pelos mutantes. Resultado do selo e tabela de mutantes nesta auditoria: **[não verificado]**. (`bin/tests/test_codex_pergunta.sh:18–19`, `:74–75`, `:585`, `:596`)

4. **Classes de reprovação**

| Classe | Constatação neste delta |
|---|---|
| Segundo juiz | Não identificado para o teto: a decisão permanece no wrapper. (`bin/codex_pergunta.sh:89–92`) |
| Segundo escritor fora da porta | Não identificado: default e cap escrevem a variável no mesmo wrapper; a escrita dos mutantes ocorre no cenário de teste. (`bin/codex_pergunta.sh:90`, `:92`; `bin/tests/test_codex_pergunta.sh:585`) |
| Relógio fora da borda | **Presente e preservado:** fila sem prazo global, espera da resposta separada da execução, preparação e inventário fora do timeout do Codex. (`bin/codex_pergunta.sh:358–370`, `:383`, `:408`, `:427`) |
| Derivação paralela de fonte única | Não aparece uma segunda tabela operacional de tetos. Entretanto, o comentário afirma janela de 20 “DO PAPEL”; p95 individual de leitura/censo/sonda continua **[não verificado]**, pois o anexo relata janela agrupada. (`bin/codex_pergunta.sh:85–89`; `contrato.r`, seção 3) |
| Selo/RED sem mordida | Os dois mutantes têm mordida estática; a cobertura não certifica toda a semântica temporal. (`bin/tests/test_codex_pergunta.sh:455–467`, `:567–594`) |
| Ordem de trava | Preservada: vaga construtora → gate/slot → resposta → porta B3. Não há inversão introduzida no diff fornecido. (`bin/codex_pergunta.sh:247`, `:358`, `:363`, `:383–385`) |
| Campo com dois significados | **Presente, inclusive previsto no contrato:** `TIMEOUT` governa espera da trava e duração do processo. A falha semântica é tratá-lo como prazo global ou equipará-lo a `duracao_s`. (`contrato_item6.txt:3`; `bin/codex_pergunta.sh:267`, `:383`, `:408`, `:415`) |

## 12/11 12:54 · sessao ede65d4988a8 · - · - · origem=Ronald (tmux)

- **Implementação principal confere:** 1930/894/1191, default no teto, cap com diagnóstico e compartilhamento do valor entre `flock` e execução; escolhas estão em `bin/codex_pergunta.sh:89–92`, leitores em `:383`, `:408`, `:415`. Exatidão dos p95 individuais e eficácia futura: [não verificado].
- **Default resolvido pelo código:** ausência ou vazio vira teto, pois usa `${CODEX_TIMEOUT:-$TETO_S}`. É uma escolha entre as alternativas antes apontadas, não explicitada no contrato fornecido (`bin/codex_pergunta.sh:90`; `contrato_item6.txt:3–4`).
- **Domínio novo:** aceita apenas decimal sem zero inicial; rejeita `0`, `0600`, negativos, frações e sufixos. A recusa de `0600`, inclusive, excede a descrição “inteiro positivo”. Essa restrição não aparece no contrato (`bin/codex_pergunta.sh:91`; `contrato_item6.txt:4`).
- **Papel sem entrada permanece um furo estrutural:** desconhecido atual sai 2 pela lista, mas a tabela tem `*)1191`. Acrescentar papel somente à lista produz teto silencioso; inexiste recusa específica por falta de entrada (`bin/codex_pergunta.sh:84`, `:89`; `contrato_item6.txt:4`).
- **Mensagem do flock usa o valor efetivo:** `${TIMEOUT}s` acompanha o cap. Porém “ocupada há” não mede idade da ocupação, e qualquer falha de `flock` recebe esse diagnóstico; comportamento preexistente (`bin/codex_pergunta.sh:383`).
- **Relógio fora da borda e campo com dois significados permanecem:** gate e repetição de slots sem limite acumulado; depois a resposta pode consumir um teto inteiro e a execução recebe outro teto inteiro. Logo 1930 segundos não limita duração total do wrapper (`bin/codex_pergunta.sh:358–370`, `:383`, `:408`, `:415`).
- **124 não cobre todo estouro/resultado:** trava sai 3; inventário construtor pode substituir inclusive um 124 por 3. São comportamentos preservados, não introduzidos pelo diff (`bin/codex_pergunta.sh:383`, `:427–440`).
- **Métrica tem borda diferente:** `duracao_s` começa em `T0C`, inclui preparação posterior e filas, e termina na saída; não é apenas duração sob `timeout` (`bin/codex_pergunta.sh:259–267`, `:298–325`, `:358–415`).
- **Fonte histórica não prova p95 individual:** comentário afirma últimas 20 “DO PAPEL”, mas o anexo `contrato.r` informa 20 chamadas do grupo leitura/censo/sonda. Atribuir p95 individual aos três segue [não verificado] (`bin/codex_pergunta.sh:85–88`; `contrato.r`, seção 3).
- **Outras classes:** o diff não demonstra segundo juiz, segundo escritor ou inversão de travas: concentra decisão temporal em `:89–92` e mantém consumidores/ordem existentes; ocorrência no restante do sistema [não verificado] (`bin/codex_pergunta.sh:89–92`, `:355–415`).

_4260 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
