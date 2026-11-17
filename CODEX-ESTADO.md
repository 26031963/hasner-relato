# CODEX-ESTADO

> Ultima resposta processada: **17/11/2026 05:38** · publicado em **17/11/2026 05:38** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 17/11 05:38 · sessao 948a32929507 · - · - · origem=Ronald (tmux)

Numeração contada desde a primeira linha de cada anexo, incluindo vazias.

| Anexo | Linha | Trecho literal |
|---|---:|---|
| verif_v1.q | 10 | “(2) sobreposicao com turno previsto ou ausencia barra;” |
| verif_v1.q | 11 | “tipos de escala diferentes ok; (4) qualquer intervalo, d1 = d2 permitido, competencia nao exportada; (5) carta = Ausencia” |
| verif_v1.q | 12 | “tipo permuta, aprovada, dia exato, ligada ao id da permuta; (6) compensacao, nao folga trabalhada; sem DSR na semana barra.” |
| verif_v1.q | 16 | “1. Para cada regra (1)-(6) e cada item do contrato: o codigo a cumpre, cumpre em parte, ou nao cumpre? Contra-exemplo” |
| verif_v1.q | 18 | “2. Existe caminho em que algo saia da casa (push, julgamento gravado, recalculo de dinheiro, chamado, log externo) ANTES” | [nao verificado]
| verif_v1.q | 21 | “3. Ordem de travas: permuta x permuta cruzada, permuta x trocar_plantao, permuta x regenerar_celulas_vinculo/” |
| verif_v1.q | 25 | “LogAuditoria.valor_depois permuta_id/cartas, Ausencia.status/documento/data/data_fim): arquivo:linha de cada” |
| verif_v1.q | 27 | “significado (dia civil x dia do turno, inclusivo x exclusivo, fuso)?” |
| verif_v1.q | 29 | “(a interjornada e o DSR locais sao juiz novo?);” |
| verif_v1.q | 30 | “selo sem mordida (algum teste” | [nao verificado]
| verif_v1.q | 31 | “passaria com o codigo da base ou com a regra removida?); ordem de trava; campo com dois significados.” |
| verif_v1.q | 32 | “6. A idempotencia ('igual') pode dar falso positivo (celulas editadas por OUTRA operacao com o mesmo estado, trilha de” |
| verif_v2.q | 14 | “2. A trava nova cria ORDEM INVERTIDA com algum caminho que ja trava CelulaDia, Ausencia, Chamado ou Pergunta ANTES de” |
| verif_v2.q | 21 | “5. Os testes novos do delta mordem (passariam com o codigo do hash b85315b2b)? Algum e vacuo?” |
| verif_v3.q | 11 | “1. TABELA DE ORDEM DE TRAVA. Para cada caminho -- permutar_plantoes, trocar_plantao isolada, editar_dia direto,” |
| verif_v3.q | 17 | “2. CELULA AUSENTE: quando uma das celulas nao existe (fora do horizonte gerado), ha ciclo entre a permuta/troca e” |
| verif_v3.q | 18 | “editar_dia direto ou a geracao/regeneracao criando a MESMA celula? Compare com o codigo do hash (sem o delta): o delta” |
| verif_v3.q | 21 | “4. Os testes novos/reescritos falhariam com o codigo da volta 2 (colaborador travado antes das cartas; guarda de dono” | [nao verificado]

Respostas relevantes:

- **verif_v1.r:12:** “**proíbe expressamente a igualdade**.”
- **verif_v1.r:13:** “**Não exige tipo nem aprovação e não grava vínculo na Ausencia.**”
- **verif_v1.r:60:** “Cobertura e sobreposição usam intervalo fechado; a permuta confere posição e fim igual ao início ou nulo.”
- **verif_v1.r:71:** “A exclusão só existe na guarda da própria permuta.”
- **verif_v1.r:73:** “`data_fim` é **inclusiva**; nulo significa aberto somente para determinados tipos.”
- **verif_v1.r:79:** “As guardas locais **são decisões novas**, com piso de11h e semana segunda–domingo. Registro/corte específico delas: **não achei** [nao verificado].”
- **verif_v1.r:93:** “Entretanto, não compara DNA, geradora, autor, carimbo, pedido, papéis ou se a última edição ainda pertence àquela permuta.”
- **verif_v3.r:1:** “A alegada ‘ordem da casa’ não corresponde a todas as transações existentes.” Atenção: as aspas no original são tipográficas duplas: `“ordem da casa”`.
- **verif_v3.r:69:** linha da tabela sobre ordem invertida: “**Continua globalmente:** outra ausência, desligamento, INSS, Chamado e transações de várias etapas têm C antes de algum X posterior.”
- **verif_v3.r:70:** “**Continua sem demonstrar concorrência ou célula ausente.**”
- **verif_v3.r:77–78:** testes de ordem na permuta e troca “**Falharia**” contra v2. [nao verificado]
- **verif_v3.r:84:** “o helper reconhece a presença do SQL `FOR UPDATE`, não a quantidade de linhas retornadas; uma consulta vazia também acrescentaria ‘celula’ à ordem.” Original usa aspas duplas tipográficas em `“celula”`.
- **verif_v3.r:91:** “**Não rejeitada não significa aprovada.**”
- **verif_v3.r:103:** “`igual` compara origem/trabalha e existência de trilha com as cartas; não compara DNA, geradora ou última edição, e retorna antes das barreiras atuais.”
- **verif_v3.r:104:** “**Corrida de criação também pode resultar em erro de unicidade**, mesmo sem deadlock: os criadores não recuperam a célula concorrente, e troca/permuta capturam apenas suas exceções de recusa.”
- **verif_v3.r:105:** “**Tipo/aprovação das cartas e mesmo dia continuam pendentes.**”

Os pontos `verif_v1.r:60`, `:73` e `verif_v3.r:104` coincidem com as referências fornecidas pelo opus.

## 17/11 05:37 · sessao bddc04b98043 · - · - · origem=Ronald (tmux)

Numeração de `entrega.md` contando linhas vazias, com `# O10-D...` como linha 1; texto total: 180 linhas.

| Tema | Linhas e trecho literal |
|---|---|
| `d1=d2` | `entrega.md:5`: `1. **AVAL item 2 literal NAO cumprido: "d1 = d2 aceito".** PERGUNTA DE DESENHO. A porta RECUSA d1 = d2 com frase` |
| Motivo para adiamento de `d1=d2` | `entrega.md:8`: `os marcos sao os do template do PROPRIO vinculo. Com d1 = d2 os dois ja trabalham no dia; "trocar o turno" so` — complementa `:9-14`; `:14`: `muda). A recusa NAO tem selo nem mutante de proposito, para nao travar o oposto do contrato.` |
| Tipo/status | `entrega.md:15`: `2. **Regra (5) em parte: carta "tipo permuta, aprovada".** Nao existe `tipo='permuta'` em `ponto/models.py`/` |
| Qualquer ausência | `entrega.md:16`: `` `ponto/catalogo/ausencias.py`; a porta aceita qualquer `Ausencia` com `documento`, do dia exato, da celula certa,`` |
| Motivo tipo/status | `entrega.md:18`: ``conferido: exigir `aprovada` com o tipo de hoje (ex. `declaracao`, que e ABONA em``; `:20`: `pelo consumidor da ausencia o dia em que a pessoa trabalha -- efeito de dinheiro errado. Precisa do tipo`; `:21`: `` `permuta` sem efeito no catalogo (migration + catalogo congelado), que e fatia propria. Pergunta: corte do tipo.`` |
| Interjornada/DSR sem juiz | `entrega.md:22`: ``3. **Interjornada (1) e DSR (6) sem juiz registrado** em `core/juizes.py`: a porta compoe guardas LOCAIS sobre juizes``; `:23`: ``registrados (`vinculo_do_dia`, `eh_dia_trabalho`, `marcos_turno`). Nao registrei juiz (PROIBIDO autoridade nova).``; `:24`: `Pergunta de lei: confirmar com o DP (o aval ja pede homologacao de (1), (3), (6)).` |
| Ordem global adiada | `entrega.md:29`: `5. **PERGUNTA DE DESENHO -- ORDEM GLOBAL DE TRAVA (Codex v3, procedente, NAO curavel nesta obra).** A casa NAO tem` |
| Motivo estrutural | `entrega.md:39`: `as barreiras liam vizinho e desligado sem trava (Codex v1, procedente). Opcoes, todas fora desta obra: (a) corte da`; `:40`: `ORDEM DA CASA (ex. C sempre primeiro) migrando editar_dia/regeneracao/cartorio -- obra ESTRUTURA propria;` |
| Selos/M24/M25 congelam escolha | `entrega.md:55`: ``**Selo congela a escolha:** `test_TRAVA_carta_celula_COLABORADOR_*` e os mutantes M24/M25 cravam X antes de C; se o``; `:56`: `corte for (a) C primeiro, esses selos INVERTEM (mesmo motivo de d1 = d2 nao ter selo). E o comentario` |
| Comentários/docstring | `entrega.md:57`: `` `ORDEM DA CASA` em `celula.py:744` e `:1003` e a docstring `:969` (linhas de bd590178e) dizem mais do que o vivo: e a ordem dos caminhos``; `:58`: `X -> C, nao uma ordem universal -- ajustar o texto no pouso (nao mexi para nao descertificar a suite).` |
| Dois significados | `entrega.md:65`: `8. **Carta = Ausencia tem DOIS significados (Codex v1 (4)/(5), procedente, nao curado: e o item 2).** A carta`; restante `:66-68`. |
| Status pendente | `entrega.md:70`: ``9. **"Nao rejeitada" != "aprovada"** (Codex v3 item 5): a porta exclui so carta `rejeitada`; carta pendente vale.`` |
| Triagem tipo/status e mesmo dia | `entrega.md:150`: `isolada aceitava rejeitada. Nao curados (TOPO): d1 = d2 (1), tipo/aprovacao da carta (2, 8), regeneracao sem trava` |
| Idempotência descartada | `entrega.md:153`: ``comparado por CONJUNTO (A/B -> B/A passava). Notas ao TOPO: 6 e 9. Descartado com motivo: idempotencia `igual` nao``; `:154`: ``compara DNA (o DNA e funcao de `trabalha` + vinculo pelo produtor unico P13; repetir com cadastro mudado e``; `:155`: `regeneracao, nao permuta) -- vai ao opus.` |
| IntegrityError reconhecido/adiado | `entrega.md:157`: `e o A<->C da mesma carta, e que os testes novos discriminam a v2 (3 de 4). PROCEDENTE e NAO curavel aqui: a casa nao`; `:158`: `tem ordem global X/C (TOPO 5), celula ausente (TOPO 5), corrida de criacao pode dar IntegrityError de unicidade em vez`; `:159`: `de deadlock (mesmo desenho, TOPO 7). Repetidos: barreiras na troca isolada (6), idempotencia (descarte acima),` | [nao verificado]
| RED módulo inexistente | `entrega.md:95`: `- Volta 1: arquivo de teste novo (RED = modulo inexistente na base fbc5c76ce).` |
| Placar contaminado por ERROR | `entrega.md:104`: ``MORDEM**, MEDIDA POR CAMADA: 21 contra os testes da volta 3 (`placar3.txt`, com o defeito de fixture do teste do``; `:105`: ``dono que dava ERROR em todo run); M19/M26 contra test.v4 (`placar4.txt`); M08/M18/M24/M25 contra o codigo e os testes`` | [nao verificado]
| Motivo não rerodar mutantes | `entrega.md:106`: ``FINAIS (`placar6.txt`). Os outros 20 (so no placar3) nao foram rerodados sobre v5 -- mutar `celula.py` depois do veredito descertificaria a arvore (o delta v3->v5 so muda o texto da regra 4 e`` |
| Sombra se declara atomic+raise | `entrega.md:113`: ``## DIFF NA SOMBRA (atomic+raise; `o10d_permuta_diff.py`, log `sombra_diff_final.log`; codigo da volta 1)`` |
| Dois colaboradores no FM futuro | `entrega.md:116`: `(uma por colab, {d1,d2}); FechamentoMensal movido: (128,12/2026), (136,12/2026), nenhum em competencia exportada.` |
| FM intacto alegado | `entrega.md:121`: `- (2c) competencia exportada 21/08-20/09: `recusada` com 4 frases LAVRADA; FM intacto True; frota `editada` +0.` |
| Drenagem manual mencionada | `entrega.md:128`: ``linhas: `on_commit registrados` 1 -> 3 e `drenados` 5 -> 7. Os +2 sao os sentinelas `_TravaDoColaborador` `` |
| Alegação dos efeitos | `entrega.md:131`: `` `permutada`, 4 celulas, frota +4, trilhas 1/2/4, rabo 0 dentro / 2 no commit, FM movidos so na competencia aberta,`` | [nao verificado]

Não achados específicos na entrega:

- Bug da aceitação de **range aberto** como carta e exclusão indevida da guarda: **não achado**; `:65-68` tratam do conflito geral dos significados.
- Pergunta sobre **dia cedido ter de ser plantão**: **não achado**; `:119` apenas relata a recusa `"nao tem plantao previsto 14/11"`.
- `M06/M09` morrerem por exceção: **não achado**.
- Literal `GREEN_v3 errors=1`: **não achado**; defeito geral de ERROR em todo run consta de `:104-105`.
- MD5 de somente dois colaboradores: **não achado**.
- Aviso explícito de que a drenagem aconteceu **dentro do atomic**: **não achado**; `:113` dá contexto atomic+raise e `:128` menciona drenagem, sem essa crítica.

A remissão de `entrega.md:159` para “TOPO 7” é textual, mas o TOPO 7 em `:61-64` trata de “ambos ativos”, não de `IntegrityError`.

## 17/11 05:37 · sessao 7d588557d67a · - · - · origem=Ronald (tmux)

Auditoria recomendada:

- **Item 1: (a).** A pergunta cobria ausência, dia exato e semântica de `data_fim` (`verif_v1.q:10–12`, `:24–27`). A resposta descreveu intervalo fechado/nulo aberto, mas não identificou o contraexemplo composto de aceitar afastamento como carta e excluí-lo da barreira (`verif_v1.r:60`, `:73`).
- **Item 2: (b).** Tipo/aprovação e `d1=d2` foram apontados e conscientemente adiados (`verif_v1.r`, seção 1; `entrega.md`, TOPO 1, 2, 8, 9).
- **Item 3:** comentários/docstring de ordem universal **(b)**: a abertura de `verif_v3.r` contesta expressamente a “ordem da casa”; TOPO 5 admite o exagero e adia sua correção. Para o defeito específico de **selos congelarem escolha sem aval**, não há crítica literal na resposta: **(a)** é mais rigoroso que atribuir reconhecimento explícito ao Codex. A resposta identifica o comportamento dos testes, mas não esse defeito (`verif_v3.r`, seção 4). Resultados particulares de M24/M25 ficam **(c)**. [nao verificado]
- **Item 4:** IntegrityError **(b)**; interjornada/DSR como decisões sem juiz **(b)**; exigência adicional de plantão no dia cedido **(e)**. Para esta última, trecho específico nas perguntas/respostas: **não achado**. O construtor conhecia o predicado, pois o implementou e justificou (`app/ponto/portas/celula.py:905–911`) e registrou sua recusa na sombra (`entrega.md`, seção DIFF NA SOMBRA, caso 2a). Portanto não cabe **(d)**.
- **Item 5: (b).** DNA/geradora ignorados foram vistos; houve descarte expresso por P13 (`verif_v1.r`, seção 6; `verif_v3.r`, seção 6; `entrega.md`, TRIAGEM CODEX v2).
- **Itens 6–7: (c).** Os defeitos concretos dependem de RED, execuções de mutantes e instrumento/medição da sombra fornecidos posteriormente (`opus_final.md:7–8`, contando título como linha 1).

Não encontrei fundamento para **(d)**: nos casos ausentes da crítica do Codex, existe reconhecimento do construtor ou o material está fora do alcance declarado. [nao verificado]

## 17/11 03:42 · sessao 170bd6b657ed · O10-D-PERMUTA-RECIPROCA · verificador · origem=Code

**Parecer: não aprovar como fechamento integral.** O delta corrige a inversão local contra edição/regeneração de células existentes, mas mantém ciclos com células ausentes e com caminhos que começam pelo Colaborador. A alegada “ordem da casa” não corresponde a todas as transações existentes. (`delta_v3.patch:+744`; `delta_v3.patch:+1000`; `app/colaboradores/services/desligamento.py:128`; `app/ponto/services/cadastro_futuro.py:31`.)

Referências `app/...:N` apontam à cópia original; `delta_v3.patch:+N`, à numeração de destino do anexo aplicado mentalmente. Execução de testes, reprodução concorrente, DDL efetivo e ocorrência em produção: **[nao verificado]**. [nao verificado]

**1. Tabela de ordem de trava**

Notação: **A** = Ausencia; **X** = CelulaDia; **C** = Colaborador; **D/H/P** = Disputa/Chamado/Pergunta. A autoridade ordena C e depois D/H/P por PK; ordenar uma chamada não ordena todas as aquisições da transação. (`app/chamados/models.py:2757`; `app/chamados/models.py:2771`; `app/chamados/models.py:2780`; `app/chamados/models.py:2793`; `app/chamados/models.py:2797`.)

| Caminho | Sequência efetiva |
|---|---|
| `permutar_plantoes`, com delta | **A por PK → X existentes por colaborador/data → C por PK → trocas internas → criação/edição de X → H**, quando o marcador encontra cobrança elegível. As trocas internas retomam travas já adquiridas; X ausente só nasce depois de C. (`delta_v3.patch:+1000`; `delta_v3.patch:+1013`; `app/ponto/portas/celula.py:1018`; `app/ponto/portas/celula.py:645`; `app/ponto/portas/celula.py:672`.) |
| `trocar_plantao` isolada, com delta | **A por PK → X existentes por colaborador/data → C por PK → criação/edição dos dois X → H**, condicional ao marcador. (`delta_v3.patch:+734`; `delta_v3.patch:+752`; `delta_v3.patch:+756`; `app/ponto/portas/celula.py:751`; `app/ponto/portas/celula.py:672`.) |
| `editar_dia` direto | **X existente por `FOR UPDATE`, ou INSERT de X ausente → C → H**, estes dois últimos somente se houver cobrança órfã elegível. O rabo fica depois do atomic próprio; atomic externo pode conservá-lo na mesma transação. (`app/ponto/portas/celula.py:637`; `app/ponto/portas/celula.py:645`; `app/ponto/portas/celula.py:672`; `app/ponto/portas/celula.py:676`; `app/chamados/models.py:487`.) |
| `regenerar_celulas_vinculo` isolada | Leitura inicial **sem trava → INSERT/UPDATE dos X em ordem de data → C → H**, se o marcador for elegível. O marcador recebe dias **reescritos**, não os meramente criados. O rabo fica depois do atomic próprio. (`app/ponto/portas/celula.py:226`; `app/ponto/portas/celula.py:227`; `app/ponto/portas/celula.py:294`; `app/ponto/portas/celula.py:311`; `app/ponto/portas/celula.py:328`; `app/ponto/portas/celula.py:393`.) |
| Command `gerar_celulas` | Um atomic envolve **regeneração de hoje por colaborador crescente → geração do período**. Pode conservar **X de A → C de A → X de B → C de B → mais X da geração**. Portanto não é simplesmente “todos os X antes de todos os C”. (`app/ponto/management/commands/gerar_celulas.py:319`; `app/ponto/management/commands/gerar_celulas.py:323`; `app/ponto/management/commands/gerar_celulas.py:345`; `app/ponto/management/commands/gerar_celulas.py:349`.) |
| `gerar_celulas_periodo` | Leitura sem trava → **INSERT de X ausente / UPDATE de DNA antigo nulo**. Atomic próprio e pedido explícito de C: **não achei**. Se chamado pelo command, herda as travas anteriores dele. (`app/ponto/portas/celula.py:1201`; `app/ponto/portas/celula.py:1218`; `app/ponto/portas/celula.py:1230`; `app/ponto/management/commands/gerar_celulas.py:345`.) |
| `decidir(Ausencia)` | Atomic → **UPDATE de A → post_save síncrono → cartório**. O cartório pode fazer **C/H → UPDATE de X** durante emissão; também pode escrever X antes de uma reconciliação que pede C. Não tem uma sequência única A→X→C. (`app/ponto/services/ausencia.py:1079`; `app/ponto/services/ausencia.py:1088`; `app/ponto/services/ausencia.py:988`; `app/ponto/signals.py:147`; `app/ponto/services/cartorio.py:1791`; `app/ponto/services/cartorio.py:1873`; `app/ponto/services/cartorio.py:1007`.) |
| `mudar_estado` direto | Não abre atomic próprio. **A → cartório → demais travas na mesma transação** exige atomic externo. O consumidor `on_commit` é outro momento, distinto do cartório síncrono. (`app/ponto/services/ausencia.py:955`; `app/ponto/services/ausencia.py:988`; `app/ponto/signals.py:147`; `app/ponto/signals.py:272`.) |
| Desligamento | **C → UPDATE de vínculo → X futuros gerados por data**, conservados no atomic do desligamento. (`app/colaboradores/services/desligamento.py:128`; `app/colaboradores/services/desligamento.py:133`; `app/colaboradores/services/desligamento.py:148`; `app/colaboradores/services/vinculo.py:641`; `app/colaboradores/services/vinculo.py:662`; `app/ponto/services/cadastro_futuro.py:31`.) |
| Lançamento INSS | **C → criação/reabertura de A → post_save/cartório → X**, dentro do atomic da view. (`app/ponto/views.py:2544`; `app/ponto/views.py:2546`; `app/ponto/views.py:2547`; `app/ponto/views.py:2564`.) |
| Escrita/transição de Chamado | **C → D/H/P conforme a operação → cartório síncrono → X**, quando a mudança de estado exige julgamento. (`app/chamados/models.py:290`; `app/chamados/models.py:487`; `app/chamados/signals.py:189`; `app/chamados/signals.py:218`; `app/ponto/portas/celula.py:1267`.) |
| `regenerar_futuras_do_cadastro` | **X futuros existentes por data → regeneração de cada dia → C/H**, se o marcador alcançar cobrança elegível. (`app/ponto/services/cadastro_futuro.py:30`; `app/ponto/services/cadastro_futuro.py:36`; `app/ponto/services/cadastro_futuro.py:44`; `app/ponto/portas/celula.py:328`.) |
| Foto futura de folgas | **X futuros → FolgaDia → regeneração futura**, conservando as travas de X. (`app/escala/folgas.py:620`; `app/escala/folgas.py:621`; `app/escala/folgas.py:639`; `app/escala/folgas.py:640`.) |
| Restauração de snapshot | **EscalaColaborador por PK → X que referenciam vínculos removidos → DELETE/UPDATE dos vínculos → regeneração futura**. O chamador ainda pode escrever nota/transição de Chamado no atomic externo. (`app/colaboradores/services/vinculo.py:1017`; `app/colaboradores/services/vinculo.py:1020`; `app/colaboradores/services/vinculo.py:1025`; `app/colaboradores/services/vinculo.py:1035`; `app/chamados/services/acoes_chamado.py:142`; `app/chamados/services/acoes_chamado.py:153`.) |
| Salvamento de template | Atomic envolvendo várias regenerações: pode acumular **X/C de A antes de pedir X de B**, como o command. (`app/escala/services/cadastro_tipo.py:140`; `app/escala/services/cadastro_tipo.py:323`; `app/escala/services/cadastro_tipo.py:339`; `app/escala/services/cadastro_tipo.py:353`.) |
| Cancelamento de férias | **A por PK → saves → cartório síncrono**, com os sufixos condicionais já descritos. (`app/ponto/services/ausencia.py:1484`; `app/ponto/services/ausencia.py:1488`; `app/ponto/services/ausencia.py:1498`.) |
| Reversão de regeneração / lavra / carimbo | UPDATE de X; reversão tem rabo depois do atomic próprio. Lavra e carimbo herdam as travas do chamador; `salvar_e_emitir` não impõe ordem anterior de C. (`app/ponto/portas/celula.py:1130`; `app/ponto/portas/celula.py:1140`; `app/ponto/portas/celula.py:1162`; `app/ponto/portas/celula.py:1266`.) |

**Pares envolvendo permuta/troca**, consideradas suas invocações isoladas, sem travas prévias do chamador:

| Par | Resultado com o delta |
|---|---|
| Permuta×permuta; permuta×troca; troca×troca | Com conjuntos estáveis e apenas essas portas, ciclo: **não achei**. Os prefixos são ordenados e a criação ocorre depois da trava comum de C. Isso não demonstra segurança perante os outros caminhos. (`delta_v3.patch:+752`; `delta_v3.patch:+756`; `delta_v3.patch:+1000`; `delta_v3.patch:+1013`.) |
| Permuta/troca×edição direta, X existente | A inversão simples da v2 fecha: ambas chegam a X antes de C. Ciclo nesse prefixo isolado: **não achei**. X ausente muda o resultado, abaixo. (`app/ponto/portas/celula.py:638`; `app/ponto/portas/celula.py:672`; `delta_v3.patch:+752`; `delta_v3.patch:+1013`.) |
| Permuta/troca×regeneração isolada, todos os X existentes | No prefixo de um colaborador, as células são adquiridas antes de C e por data; ciclo local: **não achei**. Atomic externo, criação e acumulação de colaboradores precisam da análise separada abaixo. (`app/ponto/portas/celula.py:228`; `app/ponto/portas/celula.py:311`; `app/ponto/portas/celula.py:328`.) |
| Permuta/troca×command ou salvamento de template | **Ciclo possível:** G reescreve A/hoje e segura C_A; P segura A/futuro e B/hoje e espera C_A; G avança e espera B/hoje. O bloqueio de P ocorre antes das barreiras. (`app/ponto/management/commands/gerar_celulas.py:321`; `app/ponto/management/commands/gerar_celulas.py:345`; `app/ponto/portas/celula.py:328`; `delta_v3.patch:+1010`; `delta_v3.patch:+1013`.) |
| Permuta/troca×decisão da **mesma carta** | O ciclo simples A↔C da v2 fica serializado pela carta antes de X/C. Outros recursos anteriores do chamador: **[nao verificado]**. (`delta_v3.patch:+735`; `delta_v3.patch:+1008`; `app/ponto/services/ausencia.py:988`.) |
| Permuta/troca×decisão de **outra ausência** do mesmo colaborador | **Ciclo possível:** P segura X; decisão de outra A emite cobrança e segura C; P espera C; decisão espera X para carimbar. (`delta_v3.patch:+1013`; `app/ponto/services/cartorio.py:1791`; `app/chamados/models.py:952`; `app/ponto/services/cartorio.py:1873`.) |
| Permuta/troca×desligamento | **Ciclo possível:** P segura X futuro gerado; desligamento segura C; desligamento espera X; P espera C. Não exige célula ausente nem marcador elegível na permuta. (`delta_v3.patch:+1010`; `delta_v3.patch:+1013`; `app/colaboradores/services/desligamento.py:133`; `app/colaboradores/services/desligamento.py:148`; `app/ponto/services/cadastro_futuro.py:31`.) |
| Permuta/troca×INSS ou transição de Chamado com cartório | **Ciclo possível pelo mesmo X↔C:** o outro caminho pode possuir C antes de pedir X. Na reabertura de A também pode haver A↔C se for uma carta já retida pela troca. (`app/ponto/views.py:2546`; `app/ponto/views.py:2547`; `app/chamados/models.py:290`; `app/chamados/signals.py:218`; `delta_v3.patch:+756`.) |
| Permuta/troca×escrita somente de C | Sem pedido posterior de A/X, ciclo: **não achei**; há espera unilateral. (`app/colaboradores/services/colaborador.py:131`; `app/colaboradores/services/colaborador.py:136`; `delta_v3.patch:+1013`.) |
| Permuta/troca×lavra/carimbo somente de X | Sem pedido posterior de C ou outro recurso retido pela permuta, ciclo: **não achei**. Se o chamador já detém C, é o caso anterior de X↔C. (`app/ponto/portas/celula.py:1266`; `app/chamados/signals.py:218`.) |

Para os demais pares, o mesmo cruzamento está presente em **edição ou regeneração×desligamento/Chamado/decisão emissora**: E/R segura X e pede C; o outro segura C e pede X. Em **edição/regeneração×command/template**, o command pode reter C de uma etapa e pedir X que E/R retém. São ciclos condicionados aos marcadores/emissões indicados, não observações de produção. (`app/ponto/portas/celula.py:672`; `app/ponto/portas/celula.py:328`; `app/ponto/management/commands/gerar_celulas.py:321`; `app/chamados/models.py:952`; `app/ponto/services/cadastro_futuro.py:31`.)

Entre duas edições diretas de uma célula cada, duas regenerações isoladas do mesmo colaborador em ordem de data, ou duas operações que adquirem somente C: ciclo concreto **não achei**. Para as demais combinações dos caminhos compostos do inventário, **não achei demonstração completa de ausência de ciclos**; os atomics externos impedem concluir segurança apenas pela ordem de uma função interna. (`app/ponto/portas/celula.py:638`; `app/ponto/portas/celula.py:228`; `app/ponto/portas/celula.py:393`; `app/chamados/models.py:2771`.)

**2. Célula ausente: o delta não fecha o problema global**

`SELECT FOR UPDATE` das existentes não reserva a chave da célula ausente; os três criadores usam `create`, e o modelo declara unicidade por colaborador/data. A trava de C só serializa os escritores que a adquirem **antes** de criar. (`app/ponto/portas/celula.py:638`; `app/ponto/portas/celula.py:645`; `app/ponto/portas/celula.py:294`; `app/ponto/portas/celula.py:1218`; `app/escala/models.py:1833`; `delta_v3.patch:+1013`.)

| Par e interleaving concreto | Comparação com o hash |
|---|---|
| **P/T×edição:** P consulta X ausente; E insere X; P trava C; E encontra cobrança órfã e espera C; P tenta inserir X e espera a transação de E pela unicidade. | **Piora esse interleaving.** No hash, sem aquisição anterior de C pelo marcador de outra célula, P esperaria X enquanto E obteria C e terminaria. O delta passa a exigir C antes dessa criação. (`app/ponto/portas/celula.py:645`; `app/ponto/portas/celula.py:672`; `app/chamados/models.py:487`; `delta_v3.patch:+1013`; `app/escala/models.py:1833`.) |
| **P/T×regeneração:** R insere X e reescreve outro dia Y; P não vê X, trava C e tenta criar X; R chega ao marcador de Y e espera C. | **Piora esse interleaving**, quando o hash ainda não tinha adquirido C por edição anterior. A criação de X, sozinha, não entra em `_reescritos`; Y é necessário para esse exemplo pelo marcador. (`app/ponto/portas/celula.py:294`; `app/ponto/portas/celula.py:317`; `app/ponto/portas/celula.py:328`; `delta_v3.patch:+1013`.) |
| **P/T×geração/regeneração em atomic com vários X:** G insere X; P pretrava outro X existente Y e depois pede/cria X; G tenta atualizar Y. | **Empata quanto à existência desse ciclo:** a base já pretravava Y antes da criação de X. Acrescentar C não elimina X↔Y. No gerador de período, a atualização possível é o enriquecimento de DNA antigo nulo. (`app/ponto/portas/celula.py:984`; `app/ponto/portas/celula.py:1218`; `app/ponto/portas/celula.py:1230`; `app/ponto/portas/celula.py:311`.) |
| **Permuta×permuta / permuta×troca**, criando células em etapas diferentes | **Melhora:** a trava comum de C agora antecede todas as criações dessas portas. Na base, a permuta compunha dois pares sucessivos e podia criar o segundo par depois de outra operação ter criado parte dele. (`app/ponto/portas/celula.py:1018`; `app/ponto/portas/celula.py:751`; `delta_v3.patch:+756`; `delta_v3.patch:+1013`.) |
| **Troca isolada×troca isolada** | No caminho simples, ciclo original por criação em ordem oposta: **não achei**; a base já ordenava os dois lados por colaborador/data. Portanto não atribuo ao delta uma correção demonstrada desse par. (`app/ponto/portas/celula.py:751`.) |

Há ainda uma hipótese independente do marcador: G insere X; P trava C e espera o INSERT de G pela unicidade; G precisa validar a FK de X para C e espera P. O modelo declara FK e unicidade, mas **DDL efetivo e momento dessa validação: [nao verificado]**. Não afirmo que o INSERT sempre peça essa trava imediatamente. Sob essa condição, o delta piora o par simples contra qualquer criador; a v2 já tinha o mesmo risco. (`app/escala/models.py:1681`; `app/escala/models.py:1833`; `delta_v3.patch:+756`; `delta_v3.patch:+1013`.)

O hash também podia chegar a C por um marcador anterior; portanto “piora” acima significa **ampliar os interleavings que ciclam**, não afirmar que a base era livre de deadlocks. (`app/ponto/portas/celula.py:672`; `app/ponto/portas/celula.py:1018`.)

**3. Situação dos itens 1, 2, 5 e 7 da volta 2**

| Item | Veredito |
|---|---|
| **1 — validações e criação concorrente** | Mantém as correções de releitura do desligado, quatro documentos e rejeição. Conserva serialização entre permutas/trocas. **Não fecha** a criação ausente contra edição/geração/regeneração. A troca isolada continua sem barreiras de descanso. (`delta_v3.patch:+1013`; `delta_v3.patch:+1027`; `delta_v3.patch:+757`; `app/ponto/portas/celula.py:753`; `app/ponto/portas/celula.py:902`.) |
| **2 — ordem invertida** | **Fecha o contraexemplo local com X existente** de edição/regeneração e o ciclo simples com decisão da mesma carta. **Continua globalmente:** outra ausência, desligamento, INSS, Chamado e transações de várias etapas têm C antes de algum X posterior. Contra caminhos C→X, a v3 perde a serialização que C primeiro proporcionava na v2. (`delta_v3.patch:+752`; `delta_v3.patch:+1013`; `app/ponto/services/cartorio.py:1791`; `app/colaboradores/services/desligamento.py:148`; `app/ponto/management/commands/gerar_celulas.py:345`.) |
| **5 — testes** | Melhora a discriminação da ordem v3 e acrescenta o caso A/B→B/A. **Continua sem demonstrar concorrência ou célula ausente.** A fragilidade de contar callbacks brutos também permanece: a trava registra sentinelas `on_commit`. (`delta_v3.patch:+333`; `delta_v3.patch:+380`; `app/chamados/services/trava_do_colaborador.py:47`; `app/ponto/tests/test_o10d_permuta_reciproca.py:305`.) |
| **7 — outros defeitos** | **Fecha a comparação de donos por conjunto.** Permanecem tipo/aprovação sem exigência, cartas como fatos de ausência, leitura antiga da regeneração, recusa de mesmo dia e idempotência insuficiente. (`delta_v3.patch:+742`; `app/ponto/portas/celula.py:227`; `app/ponto/portas/celula.py:897`; `app/ponto/portas/celula.py:975`; `app/ponto/portas/celula.py:1003`.) |

**4. Os testes distinguem a volta 2?** [nao verificado]

| Teste novo/reescrito | Contra o código da volta 2 | [nao verificado]
|---|---|
| Ordem carta→célula→Colaborador na permuta | **Falharia:** a v2 começava por Colaborador. (`delta_v3.patch:+352`; `delta_v3.patch:+359`.) |
| Mesma ordem na troca isolada | **Falharia**, pelo mesmo motivo. (`delta_v3.patch:+352`; `delta_v3.patch:+371`.) |
| Donos A/B→B/A | **Falharia:** conjuntos iguais não detectam papéis invertidos. (`delta_v3.patch:+380`; `delta_v3.patch:+742`.) |
| Rejeitada na troca isolada | **Passaria:** a rejeição já era verificada na v2. É regressão adicional, não discriminador da v3. (`delta_v3.patch:+757`; `delta_v3.patch:+396`.) |
| Desligado no banco, rejeitada na permuta, quatro documentos antes da primeira troca | **Passariam:** são correções acumuladas da v2. (`delta_v3.patch:+219`; `delta_v3.patch:+256`; `delta_v3.patch:+266`.) |
| Rollback com segunda recusa forjada | **Passaria:** o atomic e a conversão da segunda recusa já existiam. O teste executa a primeira troca real e exige trilha antes de forjar a segunda recusa. (`delta_v3.patch:+290`; `app/ponto/portas/celula.py:972`; `app/ponto/portas/celula.py:1019`.) |

**Não passam vazios por falta de células na fixture:** a base gera as células de setembro até dezembro, e os testes de ordem usam novembro. Contudo, o helper reconhece a presença do SQL `FOR UPDATE`, não a quantidade de linhas retornadas; uma consulta vazia também acrescentaria “celula” à ordem. Teste concorrente com segunda conexão/célula ausente: **não achei no delta**. (`app/ponto/tests/test_o10b_editar_dia_troca.py:91`; `app/ponto/tests/test_o10b_editar_dia_troca.py:93`; `app/ponto/tests/test_o10d_permuta_reciproca.py:48`; `delta_v3.patch:+333`; `delta_v3.patch:+352`.)

**5. Quem escreve, quem lê e qual significado**

| Campo/valor | Escritores encontrados | Leitores e divergência |
|---|---|---|
| `Ausencia.colaborador` | A criação atribui o dono e salva. Escritor de produção que **transfira** uma ausência existente: **não achei**; o editor normal não altera esse campo. (`app/ponto/services/ausencia.py:484`; `app/ponto/services/ausencia.py:562`; `app/ponto/services/ausencia.py:648`.) | Troca lê dono como papel cedente/substituto; cobertura e sobreposição leem como pessoa ausente. O par ordenado corrige a identidade dos papéis recebidos. (`delta_v3.patch:+734`; `delta_v3.patch:+742`; `app/ponto/turnos.py:2543`; `app/ponto/turnos.py:2839`.) |
| `Ausencia.status` | Estado inicial na criação; `mudar_estado`; decisão; cancelamento de férias grava rejeitada. (`app/ponto/services/ausencia.py:475`; `app/ponto/services/ausencia.py:486`; `app/ponto/services/ausencia.py:976`; `app/ponto/services/ausencia.py:1496`.) | Delta exclui só rejeitada; folha exige aprovada; cobrança/sobreposição consideram não rejeitadas. **Não rejeitada não significa aprovada.** (`delta_v3.patch:+757`; `delta_v3.patch:+

[truncado em 20000 de 24725 caracteres]

## 17/11 03:37 · sessao 466bb7bb03cb · - · - · origem=Ronald (tmux)

Achado adicional decisivo: o delta v3 mantém inversão contra **desligamento**, mesmo com células existentes. `executar_desligamento` segura Colaborador pelo `salvar_inteiro`, fecha vínculos e trava células futuras na mesma transação. A permuta/troca agora segura essas células antes de pedir Colaborador. Interleaving: P segura célula futura; D segura Colaborador; D espera célula; P espera Colaborador. (`app/colaboradores/services/desligamento.py:128`, `app/colaboradores/services/desligamento.py:133`, `app/colaboradores/services/desligamento.py:148`; `app/colaboradores/services/colaborador.py:90`, `app/colaboradores/services/colaborador.py:158`; `app/colaboradores/services/vinculo.py:640`, `app/colaboradores/services/vinculo.py:662`; `app/ponto/services/cadastro_futuro.py:30`, `app/ponto/services/cadastro_futuro.py:31`; `delta_v3.patch:+997`, `delta_v3.patch:+1009`.)

O command também compõe outra ordem: regenera A/hoje, podendo reter Colaborador A pelo marcador; depois regenera B/hoje. P pode segurar A/futuro e B/hoje, esperando Colaborador A, enquanto G espera B/hoje. Bloqueia antes das barreiras da permuta. O mesmo padrão existe no salvamento de template em atomic envolvendo vários colaboradores. (`app/ponto/management/commands/gerar_celulas.py:319`, `app/ponto/management/commands/gerar_celulas.py:323`, `app/ponto/management/commands/gerar_celulas.py:345`; `app/ponto/portas/celula.py:311`, `app/ponto/portas/celula.py:328`; `app/escala/services/cadastro_tipo.py:140`, `app/escala/services/cadastro_tipo.py:323`, `app/escala/services/cadastro_tipo.py:339`, `app/escala/services/cadastro_tipo.py:353`.)

Inventário adicional de caminhos:

| Caminho | Sequência observada |
|---|---|
| `regenerar_futuras_do_cadastro` | Células futuras geradas por data → regeneração de cada dia → Colaborador/Chamado se marcador elegível. (`app/ponto/services/cadastro_futuro.py:30`, `app/ponto/services/cadastro_futuro.py:36`, `app/ponto/services/cadastro_futuro.py:44`; `app/ponto/portas/celula.py:328`.) |
| `sincronizar_foto_futura_do_calendario` | Células futuras → FolgaDia → serviço anterior. (`app/escala/folgas.py:620`, `app/escala/folgas.py:621`, `app/escala/folgas.py:639`, `app/escala/folgas.py:640`.) |
| `reverter_para_snapshot` | EscalaColaborador por PK → células referenciando vínculos removidos → DELETE/UPDATE vínculos → futuras. No chamador, nota/transição do Chamado ainda dentro do atomic externo. (`app/colaboradores/services/vinculo.py:1017`, `app/colaboradores/services/vinculo.py:1020`, `app/colaboradores/services/vinculo.py:1025`, `app/colaboradores/services/vinculo.py:1035`; `app/chamados/services/acoes_chamado.py:142`, `app/chamados/services/acoes_chamado.py:149`, `app/chamados/services/acoes_chamado.py:153`, `app/chamados/services/acoes_chamado.py:155`.) |
| `reverter_regeneracao_dia` | UPDATE célula sem releitura sob trava → evento; rabo após atomic próprio, ainda sob transação externa quando houver. (`app/ponto/portas/celula.py:1121`, `app/ponto/portas/celula.py:1130`, `app/ponto/portas/celula.py:1140`, `app/ponto/portas/celula.py:1162`.) |
| `salvar_e_emitir` | UPDATE célula → INSERT EventoCelula no atomic. (`app/ponto/portas/celula.py:1266`, `app/ponto/portas/celula.py:1267`, `app/ponto/portas/celula.py:1269`.) |
| Lançamento INSS | Colaborador → criação/reabertura Ausencia → post_save/cartório → atualização situação, mesmo atomic. (`app/ponto/views.py:2544`, `app/ponto/views.py:2546`, `app/ponto/views.py:2547`, `app/ponto/views.py:2564`.) |

Para célula ausente há também ciclo **condicionado à FK efetiva**: G insere X; P não vê INSERT não confirmado, trava Colaborador e tenta inserir X, esperando a UNIQUE; G valida FK no INSERT/commit e pede chave compartilhada no Colaborador, esperando P. Modelo declara FK e UNIQUE, backend declarado é PostgreSQL; DDL vivo e momento efetivo da validação **[nao verificado]**. Não afirmar que todo INSERT trava Colaborador imediatamente. (`app/escala/models.py:1681`, `app/escala/models.py:1833`; `app/ponto/portas/celula.py:645`, `app/ponto/portas/celula.py:294`, `app/ponto/portas/celula.py:1218`; `app/config/settings/prod.py:11`.)

Entre apenas duas permutas/trocas, com conjuntos de donos/cartas/células estáveis, **não achei** ciclo novo: prefixos ordenados e trava comum antecedem criação; isso não permite concluir ordem global segura diante dos caminhos acima. (`delta_v3.patch:+749`, `delta_v3.patch:+1003`; `app/chamados/models.py:2771`.)

## 17/11 03:36 · sessao 90e72400e96b · - · - · origem=Ronald (tmux)

Achados para consolidação, todos por leitura; concorrência executada/produção: **[nao verificado]**.

| Caminho | Ordem efetiva e limites |
|---|---|
| `decidir` | `atomic` → UPDATE Ausencia → `post_save` síncrono → cartório. (`app/ponto/services/ausencia.py:1079`, `app/ponto/services/ausencia.py:1088`, `app/ponto/services/ausencia.py:988`; `app/ponto/signals.py:147`, `app/ponto/signals.py:93`.) |
| `mudar_estado` direto | Não abre atomic próprio: sobreposição da trava de Ausencia com as seguintes exige atomic externo. (`app/ponto/services/ausencia.py:955`, `app/ponto/services/ausencia.py:988`, `app/ponto/services/ausencia.py:1002`.) |
| Cartório, ramo emissor | Pode adquirir Colaborador/Chamado antes de escrever CelulaDia: emissão em `cartorio.py:1791` → `detectar_ausencias.py:694` → `models.py:949`, `models.py:952`; carimbo posterior em `cartorio.py:1873` → `celula.py:1505`, `celula.py:1267`. |
| Cartório, outros ramos | Sem adesão, escreve célula e retorna, sem emissão. Já a primeira passada pode escrever célula e a reconciliação seguinte pedir Colaborador. Portanto não há ordem universal “Ausencia→célula→Colaborador”. (`app/ponto/services/cartorio.py:1292`, `app/ponto/services/cartorio.py:1314`, `app/ponto/services/cartorio.py:1318`, `app/ponto/services/cartorio.py:1007`, `app/ponto/services/cartorio.py:1011`.) |
| Consumidor de Ausencia | É `on_commit`; suas travas não pertencem ao atomic original. Não confundir com o cartório síncrono. (`app/ponto/signals.py:250`, `app/ponto/signals.py:272`.) |
| INSS pela view | Colaborador → criação/reabertura de Ausencia → cartório/célula → situação `afastado`, no mesmo atomic. (`app/ponto/views.py:2544`, `app/ponto/views.py:2546`, `app/ponto/views.py:2547`, `app/ponto/views.py:2564`; `app/colaboradores/services/colaborador.py:90`, `app/colaboradores/services/colaborador.py:81`.) |
| Desligamento | Colaborador → UPDATE vínculo → células futuras geradas por data, no mesmo atomic. (`app/colaboradores/services/desligamento.py:128`, `app/colaboradores/services/desligamento.py:133`, `app/colaboradores/services/desligamento.py:148`; `app/colaboradores/services/vinculo.py:862`, `app/colaboradores/services/vinculo.py:641`, `app/colaboradores/services/vinculo.py:662`; `app/ponto/services/cadastro_futuro.py:30`, `app/ponto/services/cadastro_futuro.py:31`, `app/ponto/services/cadastro_futuro.py:36`.) |
| Cadastro futuro | Trava células existentes por data e chama regeneração mantendo as travas. (`app/ponto/services/cadastro_futuro.py:30`, `app/ponto/services/cadastro_futuro.py:36`, `app/ponto/services/cadastro_futuro.py:44`.) |
| Cancelamento de férias | Ausencias por PK → saves síncronos → cartório, dentro do atomic; pode ter os mesmos sufixos anteriores. (`app/ponto/services/ausencia.py:1484`, `app/ponto/services/ausencia.py:1488`, `app/ponto/services/ausencia.py:1498`.) |
| Backfill origem férias | AgendamentoFerias (+ período pelo `select_related`) → Ausencias por PK → UPDATE origem; Colaborador explícito posterior não achei. (`app/ponto/services/ausencia.py:1671`, `app/ponto/services/ausencia.py:1673`, `app/ponto/services/ausencia.py:1679`, `app/ponto/services/ausencia.py:1696`.) |

**Ciclo preservado pela volta 3:** permuta/troca segura célula X, antes da nova trava de Colaborador; uma decisão de **outra Ausencia** do mesmo colaborador consegue segurar Colaborador durante emissão, depois pede X para carimbar; a permuta pede Colaborador: ciclo X↔Colaborador. Se for exatamente a mesma carta, o pedido inicial de carta da permuta a faz esperar antes de possuir X; esse interleaving simples fica serializado. (`delta_v3.patch:+754`, `delta_v3.patch:+1013`; `app/ponto/services/cartorio.py:1791`, `app/ponto/services/cartorio.py:1873`; `app/chamados/models.py:952`.)

**Outro ciclo concreto:** P segura célula futura gerada X e pede Colaborador; desligamento segura Colaborador e pede X pelo suplemento futuro. A ordem célula→Colaborador já era alcançável na base pelo marcador de órfãs; o delta antecipa o ponto dessa espera, sem eliminar o ciclo. (`delta_v3.patch:+1013`; `app/ponto/portas/celula.py:672`; `app/colaboradores/services/desligamento.py:133`, `app/colaboradores/services/desligamento.py:148`; `app/ponto/services/cadastro_futuro.py:31`.)

| Campo | Escritores e leitores |
|---|---|
| `Ausencia.colaborador` | Criação escreve em `ausencia.py:484`, persiste em `ausencia.py:562`; edição normal altera tipo/data/minutos, sem dono em `ausencia.py:648`, `ausencia.py:659`. **Escritor de produção que transfere dono: não achei.** Leitores de cobertura e sobreposição usam esse FK como pessoa ausente: `turnos.py:2543`, `turnos.py:2839`. |
| `Ausencia.documento` | Criação: `ausencia.py:492`; anexar: `ausencia.py:1017`, `ausencia.py:1019`; reabrir: `ausencia.py:1307`, `ausencia.py:1309`. Modelo define comprovante, preenchido só no primeiro dia do intervalo: `models.py:777`, `models.py:780`. Leitores: estado inicial `ausencia.py:938`; gate de aprovação `ausencia.py:754`, `ausencia.py:774`; aviso `ausencia.py:796`, `ausencia.py:813`. |
| `Ausencia.status` | Criação deriva estado: `ausencia.py:475`, `ausencia.py:486`; transição: `ausencia.py:976`, `ausencia.py:988`; cancelamento férias: `ausencia.py:1496`, `ausencia.py:1498`. Folha exige aprovada, cobrança aceita todas não rejeitadas: `turnos.py:2528`, `turnos.py:2531`; sobreposição exclui rejeitadas: `turnos.py:2839`. |
| `Colaborador.situacao` | Portas guardadas: `colaborador.py:126`, `colaborador.py:140`, `colaborador.py:152`, `colaborador.py:164`; desligar: `desligamento.py:129`, `desligamento.py:133`; reativar: `desligamento.py:242`, `desligamento.py:247`; INSS: `ponto/views.py:2564`. `esta_desligado` significa somente estado `desligado`; agendamento futuro e afastamento não satisfazem: `desligamento.py:23`, `desligamento.py:29`, `desligamento.py:32`. Choices incluem ativo, afastado, férias, desligado: `colaboradores/models.py:295`. |
| `CelulaDia` no cartório | Cartório lê célula existente: `cartorio.py:2027`; futura/inexistente retorna: `cartorio.py:2025`, `cartorio.py:2028`; escreve veredito/ata/carimbos pela porta: `cartorio.py:1853`, `cartorio.py:1873`, `celula.py:1505`, `celula.py:1267`. |

**Divergência semântica que continua:** não rejeitada não significa aprovada; documento presente normalmente coloca a linha em `aguardando_decisao`. As cartas aceitas na permuta continuam sendo fatos de ausência para cobrança e, quando aprovadas, para folha. Não desligado também não significa ativo. (`app/ponto/services/ausencia.py:938`, `app/ponto/services/ausencia.py:912`; `app/ponto/turnos.py:2528`, `app/ponto/turnos.py:2531`; `app/colaboradores/services/desligamento.py:32`; `app/colaboradores/models.py:295`.)

## 17/11 03:35 · sessao 4aabe714545d · - · - · origem=Ronald (tmux)

Auditoria dos testes por leitura; execução e concorrência real: **[nao verificado]**. [nao verificado]

| Teste do delta acumulado | Contra a volta 2 | [nao verificado]
|---|---|
| Ordem na permuta e na troca isolada | **Falhariam:** exigem `carta → celula → colaborador`; a v2 começava por Colaborador. (`delta_v3.patch`, hunk `+331`, métodos `test_TRAVA_carta_celula_COLABORADOR_*`; descrição da v2 no anexo `verif_v2.r`, item 2.) |
| Donos A/B → B/A | **Falharia:** a comparação por conjunto aceita a inversão; a troca continua e edita os papéis relidos. (`delta_v3.patch`, hunk `+331`, método `test_TROCA_carta_que_TROCOU_de_dono_A_B_para_B_A_e_recusada`; `app/ponto/portas/celula.py:736`, `app/ponto/portas/celula.py:751`, `app/ponto/portas/celula.py:753`.) |
| Rejeitada na troca isolada | **Passaria:** a v2 já verificava rejeição antes de chamar `editar_dia`. (`verif_v2.r`, item 1, linha “Carta rejeitada”; `delta_v3.patch`, hunk `+331`, método `test_TROCA_isolada_recusa_carta_REJEITADA`.) |
| Instância velha/desligado, rejeitada na permuta e quatro documentos antes da primeira troca | **Passariam:** são correções acumuladas da v2, não discriminadores da v3. (`verif_v2.r`, itens 1 e 5; `delta_v3.patch`, hunks `+216` e `+253`.) |
| Rollback com segunda recusa forjada | **Passaria:** preserva o mesmo atomic e conversão de recusa da base; executa a primeira troca real e exige trilha antes da segunda chamada. (`app/ponto/portas/celula.py:972`, `app/ponto/portas/celula.py:1018`, `app/ponto/portas/celula.py:1020`; `delta_v3.patch`, hunk `+290`.) |

**Os testes de ordem não são vazios por falta de células na fixture.** A base gera CED/SUB/CTL/C61 de 01/09 até 31/12; os testes novos usam 03/11 e 04/11. A composição da fixture chama essa base, e `_cel` usa `.get`, portanto a ausência de uma célula também derrubaria as fotografias dos testes que as usam. (`app/ponto/tests/test_o10b_editar_dia_troca.py:91`, `app/ponto/tests/test_o10b_editar_dia_troca.py:93`, `app/ponto/tests/test_o10b_editar_dia_troca.py:103`; `app/ponto/tests/test_o10d_permuta_reciproca.py:48`, `app/ponto/tests/test_o10d_permuta_reciproca.py:89`.)

**O helper continua insuficiente para provar proteção de célula ausente.** Ele reconhece a instrução `SELECT … FOR UPDATE` pelo SQL e coleta nomes distintos; não verifica quantas linhas foram travadas. Um SELECT que devolva zero linhas também satisfaz essa coleta. Não há segunda conexão ou interleaving concorrente nesses testes. (`delta_v3.patch`, hunk `+331`, métodos `_ordem_das_travas` e `_confere_ordem_da_casa`; concorrência executada: **[nao verificado]**.) [nao verificado]

Outros defeitos que permanecem:

- A idempotência da permuta compara apenas `origem`, `trabalha` e existência de trilha com as mesmas cartas; não compara DNA, vínculo gerador ou autoria da última edição, e retorna antes das barreiras. (`app/ponto/portas/celula.py:1003`, `app/ponto/portas/celula.py:1005`, `app/ponto/portas/celula.py:1006`, `app/ponto/portas/celula.py:1010`.)
- A troca isolada continua sem chamar as barreiras de interjornada/DSR da permuta. (`app/ponto/portas/celula.py:751`, `app/ponto/portas/celula.py:753`; barreiras em `app/ponto/portas/celula.py:902`, `app/ponto/portas/celula.py:927`.)
- Tipo específico e aprovação das cartas continuam sem exigência; a fixture positiva usa `declaracao/aguardando_decisao`. (`app/ponto/tests/test_o10b_editar_dia_troca.py:106`, `app/ponto/tests/test_o10b_editar_dia_troca.py:108`; o próprio contrato do teste declara a lacuna em `app/ponto/tests/test_o10d_permuta_reciproca.py:12`.)
- A carta permanece também um fato de ausência: a guarda exclui seus IDs, mas a grade consulta cobertura aprovada e pode apagar os marcos de ausência integral. (`app/ponto/portas/celula.py:897`; `app/escala/utils.py:1188`, `app/escala/utils.py:1220`, `app/escala/utils.py:1251`.)
- `d1 == d2` continua recusado apesar de o contrato documentado dizer permitido. (`app/ponto/portas/celula.py:975`; `app/ponto/tests/test_o10d_permuta_reciproca.py:259`.)

## 17/11 03:26 · sessao 919afaf4b8c8 · O10-D-PERMUTA-RECIPROCA · verificador · origem=Code

**Parecer: não aprovar como fechamento integral.** O delta corrige validações locais, mas introduz conflito de ordem com caminhos existentes: a permuta passa a fazer **Colaborador → célula**, enquanto edição e regeneração podem fazer **célula → Colaborador**. (`delta_v2.patch:+998`; `app/ponto/portas/celula.py:638`, `app/ponto/portas/celula.py:672`, `app/chamados/models.py:487`.)

Referências ao código são da cópia original; `delta_v2.patch:+N` indica a numeração de destino do anexo. Execução de testes, reprodução concorrente e ocorrência em produção: **[nao verificado]**. [nao verificado]

**1. O que fecha da volta 1**

| Achado | Parecer | Fundamento e limite |
|---|---|---|
| Deadlock na criação de célula ausente | **Fecha em parte.** | Entre permutas/trocas que compartilham colaboradores, ambas passam a adquirir os mesmos Colaborador por PK antes das cartas/células; isso elimina o interleaving original de criação cruzada. Entretanto, permanece possibilidade de deadlock contra edição/regeneração, detalhada abaixo. (`delta_v2.patch:+738`, `delta_v2.patch:+998`; `app/chamados/models.py:2771`; `app/ponto/portas/celula.py:672`, `app/ponto/portas/celula.py:328`.) |
| TOCTOU de interjornada/DSR entre permutas/trocas | **Fecha a janela concorrente entre essas portas; fecha em parte a garantia de descanso.** | A trava comum serializa essas operações, mas `trocar_plantao` continua escrevendo sem chamar `_barreiras_da_permuta`. Assim, uma troca executada depois da permuta pode produzir descanso inválido. (`delta_v2.patch:+738`, `delta_v2.patch:+998`; `app/ponto/portas/celula.py:751`, `app/ponto/portas/celula.py:753`, `app/ponto/portas/celula.py:1010`.) |
| `esta_desligado` sobre instância velha | **Fecha para a validação da permuta.** | Os dois colaboradores são relidos depois da trava; a guarda recebe essas instâncias. Os escritores guardados de situação também usam a trava de Colaborador. Isso não transforma “não desligado” em “ativo”. (`delta_v2.patch:+1000`; `app/ponto/portas/celula.py:1010`; `app/colaboradores/services/colaborador.py:80`, `app/colaboradores/services/colaborador.py:130`; `app/colaboradores/services/desligamento.py:32`.) |
| Documento conferido somente na vez do par | **Fecha.** | A permuta verifica os quatro documentos antes de alcançar qualquer chamada interna de troca, inclusive antes do retorno `igual`. É preenchimento do campo; existência/conteúdo do arquivo: **[nao verificado]**. (`delta_v2.patch:+1020`; `app/ponto/portas/celula.py:998`, `app/ponto/portas/celula.py:1018`.) |
| Carta rejeitada | **Fecha a aceitação de `status='rejeitada'` nessas portas.** | A permuta verifica as quatro; a troca isolada verifica seu par, sobre cartas relidas e travadas. Exigir aprovação ou tipo específico continua ausente. (`delta_v2.patch:+749`, `delta_v2.patch:+1021`; `app/ponto/portas/celula.py:734`, `app/ponto/portas/celula.py:987`.) |

Contraexemplo para a garantia de descanso: A, 24x48 07–07, cede 09/11 e recebe 10/11 numa permuta válida. Depois, uma troca isolada faz A assumir 11/11. A termina o primeiro turno em 11/11 às 07h e começa o seguinte nesse mesmo instante; a troca não consulta interjornada. O equivalente para DSR é receber sábado numa permuta e domingo numa troca posterior. São contraexemplos deduzidos dos caminhos; execução: **[nao verificado]**. (`app/ponto/portas/celula.py:818`, `app/ponto/portas/celula.py:915`, `app/ponto/portas/celula.py:931`, `app/ponto/portas/celula.py:753`; `app/ponto/tests/test_o10d_permuta_reciproca.py:48`.)

**2. Ordem invertida**

**Sim, há caminho concreto no código.** A ordem nova é **Colaborador → Ausencia → CelulaDia**. (`delta_v2.patch:+738`, `delta_v2.patch:+998`; `app/ponto/portas/celula.py:734`, `app/ponto/portas/celula.py:982`, `app/ponto/portas/celula.py:984`.)

- **Edição direta: CelulaDia → Colaborador → Chamado.** `editar_dia` trava a célula e, ainda no atomic, chama `_marcar_cobrancas_orfas`; este chama `renovar_contexto`, que pede a trava de Colaborador e Chamado antes do save. (`app/ponto/portas/celula.py:638`, `app/ponto/portas/celula.py:672`; `app/ponto/management/commands/gerar_celulas.py:264`; `app/chamados/services/contexto.py:103`; `app/chamados/models.py:487`.)

  Interleaving: E segura a célula; P segura seu Colaborador e espera a célula; E encontra cobrança órfã elegível e espera Colaborador. Há ciclo de espera. Exige cobrança que atravesse as guardas do marcador; reprodução: **[nao verificado]**. (`app/ponto/management/commands/gerar_celulas.py:257`, `app/ponto/management/commands/gerar_celulas.py:261`; referências anteriores.)

- **Regeneração: CelulaDia → Colaborador → Chamado.** O UPDATE da célula ocorre antes do mesmo marcador. O command gerador também alcança esse caminho dentro de seu atomic, pela regeneração das células de hoje. (`app/ponto/portas/celula.py:311`, `app/ponto/portas/celula.py:328`; `app/ponto/management/commands/gerar_celulas.py:323`, `app/ponto/management/commands/gerar_celulas.py:345`, `app/ponto/management/commands/gerar_celulas.py:347`.)

- **Porta de ausência: Ausencia → Colaborador.** `decidir` abre atomic e chega ao save de Ausencia. Seu `post_save` chama o cartório sincronamente; havendo emissão de cobrança, a abertura de Chamado pede Colaborador. Portanto uma decisão pode segurar a carta e esperar Colaborador enquanto a permuta segura Colaborador e espera essa carta. Exemplo deduzido: rejeição de atestado passado deixa furo que exige emissão. Execução: **[nao verificado]**. (`app/ponto/services/ausencia.py:1079`, `app/ponto/services/ausencia.py:1088`, `app/ponto/services/ausencia.py:988`; `app/ponto/signals.py:147`, `app/ponto/signals.py:93`; `app/ponto/services/cartorio.py:1791`; `app/ponto/management/commands/detectar_ausencias.py:694`; `app/chamados/models.py:952`.)

- **`gerar_celulas_periodo` isoladamente:** pedido explícito posterior de Colaborador **não achei**. Há leitura sem trava, INSERT e atualização de DNA; o caminho composto do command tem a inversão da regeneração acima. (`app/ponto/portas/celula.py:1201`, `app/ponto/portas/celula.py:1218`, `app/ponto/portas/celula.py:1230`; `app/ponto/management/commands/gerar_celulas.py:347`.)

- **`abertas_do_colaborador`:** dentro da autoridade, a sequência continua Colaborador → Disputa → Chamado → Pergunta. Não achei inversão causada apenas por essa flag; ela tampouco verifica as travas que o chamador já adquiriu. (`app/chamados/models.py:2771`, `app/chamados/models.py:2780`, `app/chamados/models.py:2793`, `app/chamados/models.py:2797`.)

**3. Reentrância e precondições**

**Sim, para as mesmas linhas na mesma transação.** A autoridade refaz o `SELECT FOR UPDATE`; o registro evita adicionar sentinelas duplicadas. Na composição normal, a permuta já possui os dois Colaborador quando as duas trocas internas os pedem novamente. (`app/chamados/models.py:2771`, `app/chamados/models.py:2773`; `app/chamados/services/trava_do_colaborador.py:42`; `delta_v2.patch:+738`, `delta_v2.patch:+998`.)

O delta cumpre as precondições indicadas: as chamadas ficam dentro dos atomics existentes e passam listas, tipo aceito pelo primeiro argumento. A restrição de `abertas_do_colaborador=True` a no máximo um colaborador não se aplica, pois o delta não passa essa flag. (`app/ponto/portas/celula.py:729`, `app/ponto/portas/celula.py:972`; `app/chamados/models.py:2742`, `app/chamados/models.py:2763`, `app/chamados/models.py:2765`; `delta_v2.patch:+738`, `delta_v2.patch:+998`.)

**Reentrância não garante ordem por transação:** a função ordena os IDs daquela chamada; não impede acrescentar outro Colaborador depois de já possuir células, Chamados ou Perguntas. Chamador de produção envolvendo a permuta dessa maneira: **não achei [nao verificado]**. (`app/chamados/models.py:2757`, `app/chamados/models.py:2763`, `app/chamados/models.py:2771`.)

**4. Catraças**

**Não achei catraca que fique vermelha pelas adições mostradas; execução: [nao verificado].**

- A catraca de travas proíbe `select_for_update` das quatro entidades fora da autoridade. O delta chama a autoridade, sem criar outro escritor direto de trava de Colaborador. (`app/chamados/tests/test_e1_v7_catracas.py:85`, `app/chamados/tests/test_e1_v7_catracas.py:116`, `app/chamados/tests/test_e1_v7_catracas.py:131`; `delta_v2.patch:+738`, `delta_v2.patch:+998`.)
- A16 exclui imports dentro de funções; os novos imports de `chamados.models` são locais. (`app/core/tests/test_contract_direcao_a16.py:88`, `app/core/tests/test_contract_direcao_a16.py:95`, `app/core/tests/test_contract_direcao_a16.py:123`; `delta_v2.patch:+736`, `delta_v2.patch:+996`.)
- O selo Disputa→Colaborador examina travas diretas dentro da função; não demonstra a ordem dos caminhos indiretos encontrados acima. (`app/chamados/tests/test_selo_ordem_de_trava_disputa_colaborador.py:90`, `app/chamados/tests/test_selo_ordem_de_trava_disputa_colaborador.py:110`.)

Lista AST fechada de **quem pode chamar** `trava_na_ordem`: **não achei [nao verificado]**.

**5. Os testes mordem?** [nao verificado]

Por leitura, **os cinco testes novos falhariam no hash**; execução efetiva: **[nao verificado]**. [nao verificado]

| Teste novo | Por que distingue a base | [nao verificado]
|---|---|
| Desligado no banco, instância velha | A base pergunta à instância recebida. (`delta_v2.patch:+219`; `app/ponto/portas/celula.py:863`.) |
| Carta rejeitada | A base não verifica rejeição e alcança a troca mockada, contrariando a recusa exigida. (`delta_v2.patch:+256`; `app/ponto/portas/celula.py:994`, `app/ponto/portas/celula.py:1018`.) |
| Documento antes da primeira troca | A base só confere documento dentro da troca, que o teste substitui. (`delta_v2.patch:+266`; `app/ponto/portas/celula.py:741`, `app/ponto/portas/celula.py:1018`.) |
| Primeira trava na permuta | Na base, a primeira trava é de Ausencia. (`delta_v2.patch:+331`; `app/ponto/portas/celula.py:982`.) |
| Primeira trava na troca isolada | Na base, a primeira trava também é de Ausencia. (`delta_v2.patch:+331`; `app/ponto/portas/celula.py:734`.) |

O **rollback reescrito passaria na base**, pois o atomic e a conversão da segunda recusa já existem. **Não é vazio:** executa a primeira troca real, exige sua trilha e exige duas chamadas. (`delta_v2.patch:+290`; `app/ponto/portas/celula.py:972`, `app/ponto/portas/celula.py:1019`.)

Os testes de SQL comprovam a primeira instrução de trava, mas não demonstram concorrência ou proteção de célula ausente: a fixture gera as células previamente. Testes novos com segunda conexão, mudança de dono da carta ou rejeição na troca isolada: **não achei no delta [nao verificado]**. (`app/ponto/tests/test_o10b_editar_dia_troca.py:91`; `app/ponto/tests/test_o10d_permuta_reciproca.py:70`; `delta_v2.patch:+331`.)

Há uma fragilidade adicional no teste antigo de callbacks: ele conta a fila bruta, mas a trava registra sentinelas `on_commit`. A fixture pode já trazer os colaboradores registrados pela geração de matrícula; portanto não concluo que surgirão exatamente três callbacks nesse teste. O repo já possui contador que exclui esses sentinelas. (`app/ponto/tests/test_o10d_permuta_reciproca.py:305`; `app/chamados/services/trava_do_colaborador.py:47`; `app/colaboradores/models.py:436`; `app/chamados/tests/test_e1_v13_ordem.py:8`; `app/ponto/tests/test_selo_chokepoint_escrita.py:258`.)

**6. Quem escreve, quem lê e os significados**

| Campo | Escritores encontrados | Leitores e significado |
|---|---|---|
| `Colaborador.situacao` | Portas `gravar`, `salvar_campos`, `salvar_inteiro`, `atualizar_em_lote`; desligamento atribui `desligado`, reativação atribui `ativo`. (`app/colaboradores/services/colaborador.py:126`, `app/colaboradores/services/colaborador.py:140`, `app/colaboradores/services/colaborador.py:152`, `app/colaboradores/services/colaborador.py:164`; `app/colaboradores/services/desligamento.py:129`, `app/colaboradores/services/desligamento.py:242`.) | A permuta lê `esta_desligado`, que reconhece somente esse estado. Afastado não é desligado; desligamento futuro agendado ainda não altera a situação. (`app/ponto/portas/celula.py:863`; `app/colaboradores/services/desligamento.py:29`, `app/colaboradores/services/desligamento.py:32`.) |
| `Ausencia.status` | Criação escolhe estado inicial; `mudar_estado` persiste transições; `decidir` aceita aprovação/rejeição. Cancelamento de férias também grava `rejeitada`. (`app/ponto/services/ausencia.py:475`, `app/ponto/services/ausencia.py:486`, `app/ponto/services/ausencia.py:976`, `app/ponto/services/ausencia.py:1057`, `app/ponto/services/ausencia.py:1496`.) | Folha exige aprovada; cobrança e sobreposição excluem rejeitada. (`app/ponto/turnos.py:2528`, `app/ponto/turnos.py:2531`, `app/ponto/turnos.py:2839`.) |

Não achei significado incompatível de **desligado** nesses caminhos. Porém **não desligado ≠ ativo**: existem `afastado` e `ferias`, e essa diferença continua na guarda. (`app/colaboradores/services/desligamento.py:30`; `app/colaboradores/models.py:295`; `app/ponto/portas/celula.py:863`.)

**Não rejeitada ≠ aprovada:** documento presente normalmente produz `aguardando_decisao`, ainda aberto. Além disso, rejeitada também representa férias canceladas; não permite concluir falsidade do documento. (`app/ponto/services/ausencia.py:912`, `app/ponto/services/ausencia.py:938`, `app/ponto/services/ausencia.py:1496`.)

Permanece o conflito das **cartas como comprovantes e fatos de ausência**: a permuta exclui seus IDs da própria guarda, mas a grade continua consultando cobertura aprovada; uma carta integral aprovada pode transformar o dia assumido em ausência e apagar seus marcos. (`app/ponto/portas/celula.py:897`; `app/escala/utils.py:1188`, `app/escala/utils.py:1220`, `app/escala/utils.py:1251`.)

**7. Outros defeitos e limites**

- **A guarda de mudança de colaborador é incompleta.** Compara conjuntos: se as duas cartas passam de donos A/B para B/A, o conjunto permanece `{A,B}` e a troca executa os papéis invertidos. As travas ainda cobrem os dois donos, mas a promessa de recusar carta cujo dono mudou não é cumprida. Escritor de produção que altere `Ausencia.colaborador`: **não achei [nao verificado]**; a falha lógica está na comparação. (`delta_v2.patch:+737`, `delta_v2.patch:+747`; `app/ponto/portas/celula.py:751`, `app/ponto/portas/celula.py:753`; `app/ponto/services/ausencia.py:648`.)

- **Tipo e aprovação continuam sem exigência.** O caso positivo usa `declaracao/aguardando_decisao`; excluir rejeitada não resolve esse achado da volta 1. (`app/ponto/tests/test_o10b_editar_dia_troca.py:106`; `app/ponto/tests/test_o10d_permuta_reciproca.py:130`; `delta_v2.patch:+1021`.)

- **A regeneração ainda pode sobrescrever com leitura antiga.** Ela carrega células sem trava e decide pela origem dessa instância; a nova trava da permuta não é adquirida antes dessa leitura/escrita. (`app/ponto/portas/celula.py:227`, `app/ponto/portas/celula.py:298`, `app/ponto/portas/celula.py:311`.)

- **Persistem `d1=d2` recusado e idempotência insuficiente.** O delta não altera a recusa, nem faz `igual` comparar DNA ou conferir que a última edição ainda pertence à permuta; esse retorno continua anterior à guarda de desligamento. (`app/ponto/portas/celula.py:975`, `app/ponto/portas/celula.py:1003`, `app/ponto/portas/celula.py:1005`, `app/ponto/portas/celula.py:1010`.)

## 17/11 03:23 · sessao e39e9a130d99 · - · - · origem=Ronald (tmux)

**4. Catraças: não achei uma que fique vermelha pelas mudanças mostradas; execução efetiva: [nao verificado].**

- A catraca de travas proíbe `select_for_update` de Colaborador/Disputa/Chamado/Pergunta fora da autoridade; não proíbe chamar a autoridade. O delta adiciona chamadas a `PerguntaDisputa.trava_na_ordem`, sem novo `Colaborador.select_for_update`. (`app/chamados/tests/test_e1_v7_catracas.py:71`, `app/chamados/tests/test_e1_v7_catracas.py:85`, `app/chamados/tests/test_e1_v7_catracas.py:116`, `app/chamados/tests/test_e1_v7_catracas.py:131`; `delta_v2.patch`, hunks `@@ -730` e `@@ -976`)
- A16 considera imports de topo e exclui os que estão dentro de funções; os imports novos de `chamados.models` são locais. Portanto essas adições não infringem essa catraca. (`app/core/tests/test_contract_direcao_a16.py:88`, `app/core/tests/test_contract_direcao_a16.py:95`, `app/core/tests/test_contract_direcao_a16.py:123`; `delta_v2.patch`, hunks `@@ -730` e `@@ -976`)
- O selo Disputa→Colaborador examina `select_for_update` direto na mesma função e não segue chamadas indiretas; as adições não introduzem esse padrão. (`app/chamados/tests/test_selo_ordem_de_trava_disputa_colaborador.py:12`, `app/chamados/tests/test_selo_ordem_de_trava_disputa_colaborador.py:20`, `app/chamados/tests/test_selo_ordem_de_trava_disputa_colaborador.py:110`)
- Catraça AST que fixe uma lista de **quem pode chamar** `trava_na_ordem`: **não achei** [nao verificado].

**5. Mordida dos testes, por leitura; execução: [nao verificado].** [nao verificado]

| Teste do delta | Passaria no hash? | Evidência | [nao verificado]
|---|---|---|
| `test_R3_desligado_le_o_BANCO_e_nao_a_instancia` | **Não**, por leitura: o hash consulta `esta_desligado` sobre a instância recebida. | `app/ponto/portas/celula.py:863`; delta `@@ -216` |
| `test_R5_carta_REJEITADA_nao_e_carta` | **Não**: o hash não verifica rejeição e alcança o mock de troca; falta a recusa anterior exigida. | `app/ponto/portas/celula.py:994`, `app/ponto/portas/celula.py:1018`, `app/ponto/portas/celula.py:1042`; delta `@@ -246` |
| `test_R5_carta_sem_documento_barra_ANTES_da_1a_escrita` | **Não**: a conferência do hash está na troca, que o teste substitui; a permuta não recusa antes dela. | `app/ponto/portas/celula.py:741`, `app/ponto/portas/celula.py:1018`; delta `@@ -246` |
| Primeira trava de Colaborador na permuta | **Não**: a primeira trava explícita do hash é Ausencia. | `app/ponto/portas/celula.py:982`; delta `@@ -291` |
| Primeira trava de Colaborador na troca isolada | **Não**: a primeira trava explícita do hash é Ausencia. | `app/ponto/portas/celula.py:734`; delta `@@ -291` |
| Rollback reescrito, segunda recusa forjada | **Sim, por leitura**: a rede de rollback já existe no hash. Não é vazio: executa a primeira troca real, exige trilha e exige duas chamadas. | `app/ponto/portas/celula.py:972`, `app/ponto/portas/celula.py:1019`; delta `@@ -265` |

Os testes novos de SQL provam a posição da primeira instrução, mas **não demonstram ausência de deadlock nem proteção de células ausentes**: a fixture gera as células previamente e os testes mostrados usam uma única execução, sem segunda conexão concorrente. (`app/ponto/tests/test_o10b_editar_dia_troca.py:91`, `app/ponto/tests/test_o10d_permuta_reciproca.py:70`; delta `@@ -291`)

Testes novos de concorrência interjornada/DSR, de carta cujo colaborador mudou e de rejeição na troca isolada: **não achei no delta** [nao verificado]. [nao verificado]

**Cuidado com a contagem de callbacks:** `trava_na_ordem` registra um sentinela `on_commit` por colaborador ainda não registrado; logo callback não equivale necessariamente a rabo. (`app/chamados/models.py:2773`, `app/chamados/services/trava_do_colaborador.py:37`, `app/chamados/services/trava_do_colaborador.py:47`)

Não afirmaria que o teste antigo `len(cbs)==1` passa a falhar com três callbacks: sua fixture cria colaboradores sem matrícula, e cada criação trava o colaborador anterior para gerar matrícula; assim os colaboradores usados podem estar registrados desde o atomic da classe. Essa contaminação de fixture já é documentada por outro selo. (`app/ponto/tests/test_o10d_permuta_reciproca.py:305`, `app/ponto/tests/test_o10b_editar_dia_troca.py:95`, `app/colaboradores/models.py:427`, `app/colaboradores/models.py:436`, `app/chamados/tests/test_e1_v13_ordem.py:8`)

Já existe no repo uma contagem que exclui explicitamente esses sentinelas. (`app/ponto/tests/test_selo_chokepoint_escrita.py:258`)

## 17/11 03:22 · sessao f763a2ac856e · - · - · origem=Ronald (tmux)

**Q2 — há ordem invertida introduzida contra caminhos existentes.**

1. **`editar_dia` direto:** `CelulaDia → Colaborador → Chamado`. A célula é travada em `app/ponto/portas/celula.py:638`; `_marcar_cobrancas_orfas` roda ainda dentro do atomic em `app/ponto/portas/celula.py:672`; o marcador chama `renovar_contexto` em `app/ponto/management/commands/gerar_celulas.py:264`; esta chama `trava_a_ordem_para_escrever` antes do save em `app/chamados/services/contexto.py:103`; a porta pede Colaborador e Chamado em `app/chamados/models.py:487`. O delta faz a ordem contrária, **Colaborador → carta → célula** (`delta_v2.patch`, hunk `@@ -976,9 +989,16 @@`). Contraexemplo: T1 segura célula e chega ao marcador; T2 segura Colaborador e espera essa célula; T1 espera Colaborador. Execução real: [nao verificado].

2. **Regeneração:** mesma inversão, por trava implícita do UPDATE: `CelulaDia → Colaborador → Chamado`. A regeneração salva a célula em `app/ponto/portas/celula.py:311` e chama o mesmo marcador em `app/ponto/portas/celula.py:328`. Exige célula efetivamente reescrita e cobrança elegível pelo marcador (`app/ponto/management/commands/gerar_celulas.py:257`, `app/ponto/management/commands/gerar_celulas.py:261`). O cron gerador alcança isso dentro de seu atomic: `app/ponto/management/commands/gerar_celulas.py:345` → `_regenerar_hoje_existentes` em `:347` → regeneração em `:323`.

3. **Portas de ausência:** caminho possível **Ausencia → Colaborador**. `decidir` abre atomic (`app/ponto/services/ausencia.py:1079`), chama `mudar_estado` (`:1088`), que salva Ausencia (`:988`). O post_save síncrono chama o cartório (`app/ponto/signals.py:143`, `:147`, `:93`); o cartório pode emitir chamado (`app/ponto/services/cartorio.py:1791`, `:1829`), cuja abertura toma Colaborador (`app/ponto/management/commands/detectar_ausencias.py:694`, `app/chamados/models.py:952`). Exemplo deduzido: rejeitar atestado num dia passado e deixar furo que exige emissão; delta segura Colaborador e espera essa Ausencia, enquanto a decisão segura Ausencia e pede Colaborador. Ocorrência/execução: [nao verificado]. **Não confundir com o consumidor de ausência**, que é agendado em `on_commit` (`app/ponto/signals.py:272`).

4. **Gerador puro:** pedido explícito posterior de Colaborador dentro de `gerar_celulas_periodo`: **não achei**. Ali há leitura sem trava (`app/ponto/portas/celula.py:1201`), INSERT (`:1218`) e UPDATE de DNA (`:1230`). Inserções poderiam envolver travas de FK no PostgreSQL, mas modo/instante efetivos e interleaving nesta cópia: [nao verificado].

5. **`abertas_do_colaborador`:** a própria função segue `Colaborador → Disputa → Chamado → Pergunta` (`app/chamados/models.py:2771`, `:2780`, `:2793`, `:2797`). Isso não torna segura uma chamada feita depois de adquirir outra linha: ela não examina a ordem das travas já adquiridas pelo chamador (`app/chamados/models.py:2763`).

**Q3 — reentrante para as mesmas linhas, com ressalvas.**

- O primeiro argumento **aceita lista**, além de instância e PK (`app/chamados/models.py:2763`). As listas ordenadas do delta são compatíveis (`delta_v2.patch`, hunks `@@ -730,6 +730,12 @@` e `@@ -976,9 +989,16 @@`).
- As duas chamadas estão em atomic: troca em `app/ponto/portas/celula.py:729`; permuta em `:972`. A função exige transação aberta por contrato (`app/chamados/models.py:2742`).
- A função executa novamente o SELECT FOR UPDATE (`app/chamados/models.py:2771`); o registro de colaboradores evita sentinelas duplicadas (`app/chamados/services/trava_do_colaborador.py:42`). Repetir as mesmas linhas na mesma transação não introduz nova espera; concorrência executada: [nao verificado].
- **Reentrância não autoriza ampliar o conjunto depois de travar cartas/células, nem pedir novo colaborador fora da ordem global.** A função ordena somente o conjunto daquela chamada e não verifica quais linhas o atomic já segura (`app/chamados/models.py:2757`, `:2763`, `:2771`).
- `abertas_do_colaborador=True` com dois colaboradores é expressamente proibido por assert (`app/chamados/models.py:2765`); o delta não passa essa flag, portanto não viola essa precondição (`delta_v2.patch`, hunks citados).
- A função não verifica existência das linhas antes de registrar os IDs solicitados: registra `_cids`, não os PKs efetivamente retornados (`app/chamados/models.py:2771`, `:2773`). Exploração concreta por argumentos apagados/inválidos do delta: [nao verificado].

## 17/11 03:22 · sessao 6c89e3ebe7ea · - · - · origem=Ronald (tmux)

**Pergunta 6 — escritores, leitores e significados**

| Campo | Quem escreve | Quem lê / significado |
|---|---|---|
| `Colaborador.situacao` | Porta material: `gravar`, `salvar_campos`, `salvar_inteiro`, `atualizar_em_lote`; quando levam `situacao`, travam e guardam a linha. (`app/colaboradores/services/colaborador.py:126`, `:140`, `:152`, `:164`, `:174`) Ato de desligamento escreve `desligado`; reativação escreve `ativo` pela porta específica. (`app/colaboradores/services/desligamento.py:128`, `:241`) | `esta_desligado` reconhece **somente** `situacao == 'desligado'`; afastado não é desligado e desligamento futuro agendado mantém situação. (`app/colaboradores/services/desligamento.py:23`, `:29`, `:32`, `:107`) A permuta usa esse juiz. (`app/ponto/portas/celula.py:854`, `:863`) |
| `Ausencia.status` | Criação escolhe estado inicial; com documento nasce `aguardando_decisao`. (`app/ponto/services/ausencia.py:473`, `:486`, `:938`) Transições passam por `mudar_estado`; decisão DP recebe `aprovada/rejeitada`. (`app/ponto/services/ausencia.py:955`, `:976`, `:1057`, `:1088`) Há ainda escritor explícito de rejeição de férias canceladas, fora de `mudar_estado`. (`app/ponto/services/ausencia.py:1468`, `:1496`) | Folha exige `aprovada`; cobrança e sobreposição excluem `rejeitada`. (`app/ponto/turnos.py:2523`, `:2528`, `:2531`, `:2839`) Rejeitada permanece histórica, sai da mesa; pode ser reaberta. (`app/ponto/turnos.py:3140`; `app/ponto/services/ausencia.py:1249`, `:1281`) |

**Conflitos que persistem:**

- **Não desligado não significa ativo.** Existem também `afastado` e `ferias`; a permuta continua aceitando essas situações se as demais barreiras passarem. A API de batida, por exemplo, recusa `ferias`. (`app/colaboradores/models.py:295`; `app/ponto/portas/celula.py:861`; `app/api/views.py:403`)
- O valor `afastado` do cadastro não é autoridade atual para afastamento: a apresentação deriva afastamento da ausência, com precedência de desligado. (`app/colaboradores/services/situacao_exibida.py:23`, `:56`, `:62`)
- **Não rejeitada não significa aprovada.** `aguardando_decisao` é estado aberto, com decisão ainda devida pelo DP; o delta permite esse estado. (`app/ponto/services/ausencia.py:912`, `:938`; `app/ponto/models.py:702`; `delta_v2.patch`, hunk `@@ -995,6 +1015,19 @@`)
- O significado de `rejeitada` é compatível com a exclusão nova; porém inclui também cancelamento administrativo de férias, sem concluir que o documento seja falso ou inválido. (`app/ponto/services/ausencia.py:1468`, `:1496`)
- Permanece a dupla função das cartas: comprovantes na permuta e fatos de ausência nos demais leitores. A própria guarda exclui seus IDs, mas a grade consulta cobertura aprovada e transforma o dia em ausência; se integral, apaga marcos. (`app/ponto/portas/celula.py:889`, `:897`; `app/escala/utils.py:1188`, `:1220`, `:1251`)

**Outro defeito específico do delta: guarda de mudança de dono incompleta.**

A troca compara o **conjunto** dos dois colaboradores. Se a carta cedente inicialmente apontava A e a substituta B, mas agora apontam respectivamente B e A, `{A,B}` continua igual e a recusa não acontece. A operação passa a executar os papéis invertidos, embora a promessa seja recusar carta cujo colaborador mudou. (`delta_v2.patch`, hunk `@@ -738,6 +744,12 @@`; execução dos papéis em `app/ponto/portas/celula.py:751`, `:753`, `:764`)

Não achei porta de produção que altere `Ausencia.colaborador`: a edição examinada recebe e escreve tipo, data, duração e minutos. Portanto a falha lógica da comparação está demonstrada; ocorrência ou alcançabilidade por escritor de produção: **[nao verificado]**. (`app/ponto/services/ausencia.py:614`, `:648`, `:659`)

## 17/11 03:15 · sessao 57163e223b89 · O10-D-PERMUTA-RECIPROCA · verificador · origem=Code

**Parecer: não aprovar como cumprimento integral do aval.** Há recusa explícita de `d1 = d2`, cartas sem validação de tipo/aprovação e validações de descanso sobre estado não travado. (`app/ponto/portas/celula.py:975`, `app/ponto/portas/celula.py:741`, `app/ponto/portas/celula.py:897`, `app/ponto/portas/celula.py:905`)

Revisão somente por leitura; testes e concorrência executados: [nao verificado]. Identidade Git do hash, sem repositório Git disponível na cópia: [nao verificado]. Os contraexemplos abaixo são cadastros hipotéticos deduzidos do código; ocorrência em produção: [nao verificado]. [nao verificado]

**1. Regras e contrato**

| Exigência | Parecer | Evidência e contraexemplo |
|---|---|---|
| **(1) Interjornada de 11h nos dois colaboradores** | **Cumpre em parte.** Calcula anterior/seguinte para ambos, mas consulta vizinhos sem travá-los. | Duas permutas concorrentes podem deixar A trabalhando **10/11 07h → 11/11 07h** e **11/11 07h → 12/11 07h**, embora cada validação isolada encontre 24h de descanso. Cadastro e sequência detalhados na resposta 3. (`app/ponto/portas/celula.py:880`, `app/ponto/portas/celula.py:905`, `app/ponto/portas/celula.py:915`, `app/ponto/portas/celula.py:922`, `app/ponto/portas/celula.py:984`) |
| **(2) Sobreposição com turno previsto ou ausência barra** | **Cumpre em parte.** Consulta ambos, excluindo as quatro cartas; não estabiliza outras ausências nem os dados de escala. | A cede 03/11 e recebe 04/11; após a consulta de ausência de A04, outra transação insere atestado aprovado em A04. A permuta continua: somente as quatro cartas e quatro células existentes foram travadas. (`app/ponto/portas/celula.py:891`, `app/ponto/portas/celula.py:897`, `app/ponto/portas/celula.py:982`, `app/ponto/turnos.py:2839`) |
| **(3) Mesma empresa, ambos ativos, vínculos nas duas datas; escalas diferentes permitidas** | **Cumpre em parte.** Empresa e vínculos são consultados; “ativo” foi reduzido a “não desligado”. | No cadastro CED03/SUB04 do teste, mudar SUB para `situacao='afastado'`, mantendo vínculos e sem ausência sobreposta, não aciona essa guarda. `esta_desligado` só reconhece `desligado`; também lê a instância recebida, que pode estar velha. Escalas diferentes têm caso positivo. (`app/colaboradores/models.py:295`, `app/colaboradores/services/desligamento.py:32`, `app/ponto/portas/celula.py:863`, `app/ponto/portas/celula.py:1007`, `app/ponto/tests/test_o10d_permuta_reciproca.py:162`) |
| **(4) Qualquer intervalo, `d1 = d2`, competência não exportada** | **Cumpre em parte.** Não impõe distância máxima entre datas e consulta a lavra; **proíbe expressamente a igualdade**. | Dois colaboradores com plantões distintos em **03/11/2026**, ambos vigentes e competência aberta: `permutar_plantoes(A,03/11,B,03/11)` devolve `recusada` antes de examinar as cartas. A consulta de exportação também tem corrida concorrente, descrita na resposta 3. (`app/ponto/portas/celula.py:975`, `app/ponto/portas/celula.py:873`) |
| **(5) Quatro Ausencia tipo permuta, aprovadas, dia exato, ligadas ao id** | **Cumpre em parte.** Exige quatro linhas distintas nas quatro posições, dia exato e documento preenchido; liga seus IDs à auditoria. **Não exige tipo nem aprovação e não grava vínculo na Ausencia.** | O próprio caso de sucesso CED03/SUB04 usa `tipo='declaracao'`, `status='aguardando_decisao'`. Também não há rejeição de carta `rejeitada` na porta. (`app/ponto/portas/celula.py:987`, `app/ponto/portas/celula.py:995`, `app/ponto/portas/celula.py:741`, `app/ponto/portas/celula.py:1030`, `app/ponto/tests/test_o10b_editar_dia_troca.py:106`, `app/ponto/tests/test_o10d_permuta_reciproca.py:129`) |
| **(6) Compensação, não folga trabalhada; sem DSR na semana barra** | **Cumpre em parte.** Cedido vira `False`, assumido vira trabalho normal do próprio template; verifica sete datas da semana. A verificação não é protegida contra concorrência e as cartas continuam tendo efeito de ausência. | A5x2, folgas sábado/domingo: uma permuta recebe **14/11**, outra recebe **15/11**. Cada uma vê uma folga restante; juntas deixam A trabalhando **09–15/11 inteiro**. Cadastro completo na resposta 3. (`app/ponto/portas/celula.py:708`, `app/ponto/portas/celula.py:929`, `app/ponto/portas/celula.py:931`, `app/escala/utils.py:1220`) |

Quanto ao contrato:

- **Rollback ao receber `recusada`: cumpre para as escritas de banco realizadas pela troca/permuta.** A troca lança `_TrocaRecusada` dentro do `atomic`; a permuta transforma a recusa de qualquer troca em `_PermutaRecusada` dentro do atomic maior. Não achei escrita de banco sobrevivente nesses retornos. (`app/ponto/portas/celula.py:729`, `app/ponto/portas/celula.py:754`, `app/ponto/portas/celula.py:776`, `app/ponto/portas/celula.py:972`, `app/ponto/portas/celula.py:1019`, `app/ponto/portas/celula.py:1036`)
- **Ordenar as quatro células antes de travar: cumpre em parte.** Ordena as **existentes** por colaborador/data; células ausentes não ficam protegidas e são criadas pela ordem dos dois pares, permitindo inversão. (`app/ponto/portas/celula.py:984`, `app/ponto/portas/celula.py:1018`, `app/ponto/portas/celula.py:645`)
- **Aceitar `d1 = d2`: não cumpre.** (`app/ponto/portas/celula.py:975`)

**2. Algo sai antes do commit? Recusa deixa escrita?** [nao verificado]

**Permuta e troca isolada:** o rabo normal fica em `on_commit`, registrado dentro do atomic correspondente. Isso corrige a emissão pelo primeiro lado antes da recusa do segundo. A marcação de cobranças órfãs continua dentro da transação e é desfeita com ela. (`app/ponto/portas/celula.py:668`, `app/ponto/portas/celula.py:700`, `app/ponto/portas/celula.py:773`, `app/ponto/portas/celula.py:1034`)

**`editar_dia` direto continua permitindo efeito externo antes do commit de um chamador.** Ele sai apenas do seu atomic interno e chama imediatamente `_o_chao_do_dia_mudou`. Se houver atomic externo, julgamento e abertura de chamado acontecem antes desse commit externo; o emissor pode chamar `webpush` diretamente. (`app/ponto/portas/celula.py:637`, `app/ponto/portas/celula.py:676`, `app/ponto/portas/celula.py:473`, `app/ponto/services/cartorio.py:2040`, `app/ponto/management/commands/detectar_ausencias.py:777`, `app/chamados/push_utils.py:40`)

Contraexemplo: atomic externo → `editar_dia` num dia passado com falta de batidas → julgamento abre chamado e envia push → exceção posterior desfaz o atomic externo. O push não acompanha o rollback; existência de assinatura/configuração para efetivamente enviá-lo: [nao verificado]. (`app/ponto/portas/celula.py:676`, `app/ponto/portas/celula.py:473`, `app/ponto/management/commands/detectar_ausencias.py:779`, `app/chamados/push_utils.py:40`)

**O view que chama `editar_dia`: não achei.** Chamador de produção direto dessa função fora da própria porta: não achei [nao verificado].

Dinheiro e rejunção/falta espúria têm `on_commit` próprios; não achei execução desses efeitos antes do commit pelos caminhos examinados. (`app/ponto/services/fechamento.py:1798`, `app/ponto/services/fechamento.py:1827`, `app/ponto/portas/celula.py:529`, `app/ponto/portas/celula.py:541`)

**A promessa literal incluindo logs não vale.** Falha de auditoria incrementa memória e chama `logging.error` imediatamente; falha ao identificar a lavra também registra exceção antes da recusa. Destino externo efetivo desses logs: [nao verificado]. (`app/core/observ.py:73`, `app/core/observ.py:79`, `app/core/observ.py:105`, `app/ponto/portas/celula.py:623`)

**3. Travas, deadlock, TOCTOU e lavra**

- **Permuta × permuta cruzada, com todas as células existentes:** não achei inversão na sequência explícita: cartas por PK, depois células por colaborador/data. **Permuta × troca isolada**, nas mesmas condições, também segue essa ordem relativa. Isso não protege os demais estados consultados. (`app/ponto/portas/celula.py:982`, `app/ponto/portas/celula.py:984`, `app/ponto/portas/celula.py:734`, `app/ponto/portas/celula.py:751`)
- **Com células ausentes, há interleaving que deadlocka.** A12x36 trabalha03/11 e B12x36 trabalha04/11, ambos19–07, vínculos vigentes e células03/04 ainda inexistentes. P=`permuta(A03,B04)` e Q=`permuta(B04,A03)`, com cartas distintas: P cria A03/B03; Q cria A04/B04; P espera A04 e Q espera A03. As criações encontram a unicidade colaborador/data, mas não tiveram trava anterior dessas linhas. Execução do interleaving: [nao verificado]. (`app/ponto/portas/celula.py:984`, `app/ponto/portas/celula.py:1018`, `app/ponto/portas/celula.py:645`, `app/escala/models.py:1833`)
- **Permuta × troca isolada também pode inverter nas ausentes:** P cria A03/B03; uma troca distinta A04→B03 cria A04 e espera B03; P então espera A04. Execução: [nao verificado]. (`app/ponto/portas/celula.py:751`, `app/ponto/portas/celula.py:1018`, `app/ponto/portas/celula.py:645`)
- **TOCTOU de interjornada:** A24x48,07–07, âncora03/11, trabalha09 e12. P: A cede09 a B6x1 com folga segunda e recebe10. Q: A cede12 a C6x1 com folga quinta e recebe11. Cada operação vê 24h de descanso de A; B/C transferem sua folga para o dia cedido. As células travadas são disjuntas, mas o resultado são os dois plantões consecutivos de A10/A11, descanso **0h**. Execução: [nao verificado]. (`app/ponto/portas/celula.py:984`, `app/ponto/portas/celula.py:905`, `app/ponto/portas/celula.py:915`, `app/ponto/portas/celula.py:922`)
- **TOCTOU de DSR:** A5x2,08–17, folgas sábado/domingo; B/C5x2,08–17, folgas segunda/terça. P: A cede03/11 e recebe14/11 de B. Q: A cede24/11 e recebe15/11 de C. B conserva02/11 de folga; C conserva23/11. Cada validação de A encontra uma folga em14/15; juntas eliminam ambas. Execução: [nao verificado]. (`app/escala/models.py:852`, `app/ponto/portas/celula.py:929`, `app/ponto/portas/celula.py:984`)
- **Permuta × regeneração: escrita baseada em leitura antiga.** R lê A03 como gerada; P edita e commita A03; R usa a origem antiga e salva `trabalha/dna`. Como esse save não inclui `origem/editada_*`, pode restar célula **editada**, com carimbo da permuta e conteúdo da regeneração. Não achei inversão direta de células nesse par quando todas existem; achei esta sobrescrita. (`app/ponto/portas/celula.py:227`, `app/ponto/portas/celula.py:298`, `app/ponto/portas/celula.py:307`, `app/ponto/portas/celula.py:311`)
- **Permuta × geração:** o gerador também carrega sem trava. Numa célula passada inicialmente gerada com `dna=None`, pode enriquecer o DNA usando `trabalha` antigo depois da edição concorrente, deixando `trabalha=False` com DNA de trabalho. Nas ausentes existe disputa de INSERT. (`app/ponto/portas/celula.py:1201`, `app/ponto/portas/celula.py:1217`, `app/ponto/portas/celula.py:1228`)

**Lavra depois das travas?** Na permuta que chega à validação, **sim**, depois da pré-trava das células existentes. Em `editar_dia` direto e troca isolada, a guarda vem **antes** da trava daquela célula; na regeneração, antes do atomic. Nenhuma dessas consultas trava exportação/holerite, portanto uma lavra concorrente pode nascer depois da consulta. O retorno `igual` da permuta antecede a guarda. (`app/ponto/portas/celula.py:984`, `app/ponto/portas/celula.py:1010`, `app/ponto/portas/celula.py:614`, `app/ponto/portas/celula.py:638`, `app/ponto/portas/celula.py:222`, `app/ponto/portas/celula.py:1003`, `app/folha/models.py:182`)

**4. Quem escreve, quem lê e significados**

| Campo | Escritor | Leitores fora do diff e consequência |
|---|---|---|
| `CelulaDia.trabalha` | `editar_dia`; geração/regeneração continuam na mesma porta. (`app/ponto/portas/celula.py:645`, `app/ponto/portas/celula.py:648`, `app/ponto/portas/celula.py:294`, `app/ponto/portas/celula.py:307`) | Dia previsto lê a célula; precedência e cobrança passam a reconhecer folga/trabalho; previsto da grade muda. (`app/escala/models.py:1095`, `app/ponto/precedencia.py:348`, `app/chamados/juizes.py:459`, `app/escala/services/leitor_celula.py:398`, `app/escala/services/leitor_celula.py:412`) |
| `CelulaDia.dna` | `montar_dna` produz; `editar_dia` persiste. (`app/ponto/management/commands/gerar_celulas.py:73`, `app/ponto/portas/celula.py:636`, `app/ponto/portas/celula.py:649`) | Marcos, pausas e extra congelada são lidos pelo vínculo; marcos aware pelo serviço de jornada; julgamento e impressão pelo cartório; enquadramento da batida pelo juiz. (`app/escala/models.py:1124`, `app/escala/models.py:1189`, `app/escala/models.py:1235`, `app/escala/servico_jornada.py:76`, `app/ponto/services/cartorio.py:1398`, `app/ponto/services/cartorio.py:1437`, `app/ponto/juiz_batida.py:158`) |
| `origem` | Edição grava `editada`; geração grava `gerada`. (`app/ponto/portas/celula.py:651`, `app/ponto/portas/celula.py:295`) | Exclui de regeneração/geração e do vigia de cadastro; impede restauração que removeria a geradora; aparece na apresentação. (`app/ponto/portas/celula.py:298`, `app/ponto/portas/celula.py:1223`, `app/ponto/services/vigia_cadastro_futuro.py:49`, `app/colaboradores/services/vinculo.py:1027`, `app/colaboradores/services/vinculo.py:1103`, `app/ponto/services/dia_do_colab.py:91`) |
| `editada_em` | `editar_dia`. (`app/ponto/portas/celula.py:652`) | Vira fato novo para cartório, invalidação de veredito de pergunta, censo de fase e cobrança órfã; apresentação usa `localtime`. (`app/ponto/services/cartorio.py:393`, `app/chamados/veredito_lavrado.py:191`, `app/escala/services/fase_12x36.py:188`, `app/escala/services/regeneracao.py:119`, `app/ponto/services/dia_do_colab.py:93`) |
| `editada_por` | `editar_dia`. (`app/ponto/portas/celula.py:653`) | Leitor específico de produção fora do diff: **não achei** [nao verificado]. O FK permite `SET_NULL`. (`app/escala/models.py:1733`) |
| `LogAuditoria.valor_depois`, `permuta_id`, `cartas` | Porta monta os valores; `core.observ.evento` faz o INSERT. (`app/ponto/portas/celula.py:664`, `app/ponto/portas/celula.py:770`, `app/ponto/portas/celula.py:1030`, `app/core/observ.py:134`) | `cartas` é lido pela idempotência da própria permuta. Leitor específico de `permuta_id/cartas` fora do diff: **não achei** [nao verificado]. (`app/ponto/portas/celula.py:1005`) |
| `Ausencia.status` | Criação/transição da porta de ausência; permuta não o altera. (`app/ponto/services/ausencia.py:486`, `app/ponto/services/ausencia.py:976`, `app/ponto/portas/celula.py:1018`) | Folha aceita apenas aprovada; cobrança aceita não rejeitada; sobreposição aceita não rejeitada. (`app/ponto/turnos.py:2528`, `app/ponto/turnos.py:2530`, `app/ponto/turnos.py:2839`) |
| `Ausencia.documento` | Criação/anexo da porta de ausência. (`app/ponto/services/ausencia.py:493`, `app/ponto/services/ausencia.py:1017`) | Troca verifica somente preenchimento; disponibilidade real do arquivo: [nao verificado]. (`app/ponto/portas/celula.py:741`) |
| `Ausencia.data/data_fim` | Criação/edição da ausência; `save` deriva fim/duração. (`app/ponto/services/ausencia.py:484`, `app/ponto/services/ausencia.py:649`, `app/ponto/models.py:904`, `app/ponto/models.py:917`) | Cobertura e sobreposição usam intervalo fechado; a permuta confere posição e fim igual ao início ou nulo. (`app/ponto/turnos.py:2552`, `app/ponto/turnos.py:2844`, `app/ponto/portas/celula.py:987`, `app/ponto/portas/celula.py:995`) |

Outros leitores diretos encontrados, além dos anteriores:

| Comportamento afetado | Leitores |
|---|---|
| Cobrança, perguntas, falta e troca de enquadramento | `app/escala/servico_jornada.py:310`; `app/chamados/juizes.py:952`; `app/chamados/juizes.py:1352`; `app/ponto/marcos.py:155`; `app/ponto/services/flip_auto.py:208`; `app/ponto/services/passivo_s84.py:96`; `app/ponto/supra_juiz.py:209`; `app/chamados/services/fabrica_da_celula.py:228`. |
| Grade, veredito, disponibilidade e exportação | `app/escala/utils.py:46`; `app/escala/utils.py:97`; `app/escala/utils.py:130`; `app/escala/utils.py:166`; `app/escala/utils.py:1893`; `app/ponto/services/veredito_celula.py:31`; `app/folha/export.py:1295`. |
| Pauta, apresentação, diagnóstico e vigias | `app/pautas/services.py:424`; `app/ponto/services/dia_do_colab.py:46`; `app/ponto/services/dia_do_colab.py:123`; `app/relatorios/dia_da_frota.py:47`; `app/ponto/services/diagnostico_escala.py:153`; `app/ponto/services/esmeril_espelho.py:71`; `app/ponto/services/esmeril_espelho.py:116`; `app/ponto/services/vigia_entrada.py:126`; `app/ponto/services/faltas_de_hoje.py:43`; `app/chamados/services/adesao.py:35`. |
| Instrumentos e comandos que também observam os campos | `app/ponto/management/commands/detectar_intervalo_ausente.py:53`; `app/ponto/management/commands/backfill_b5_insumos.py:73`; `app/ponto/management/commands/vigia_b5_ausencia.py:79`; `app/ponto/management/commands/tripwire_celulas.py:22`; `app/ponto/management/commands/tripwire_celulas.py:99`; `app/ponto/management/commands/censo_fase_12x36.py:392`; `app/chamados/management/commands/auditar_invariantes_chamados.py:377`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1447`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1546`. |

**Conflito de significado comprovável:** as cartas são tratadas como comprovante pela permuta, mas continuam sendo fatos de ausência para os outros leitores. A exclusão só existe na guarda da própria permuta. Com cartas aprovadas integrais nas datas assumidas, a grade pode transformar trabalho em `ausencia` e apagar seus marcos, embora a célula esteja `trabalha=True`. (`app/ponto/portas/celula.py:897`, `app/ponto/turnos.py:2528`, `app/escala/utils.py:1188`, `app/escala/utils.py:1220`, `app/escala/utils.py:1251`)

Quanto às datas: `data_fim` é **inclusiva**; nulo significa aberto somente para determinados tipos. Marcos de turno são ancorados no dia inicial e encadeados com avanço quando o seguinte é `<=` ao anterior, usando timezone corrente; ausência parcial noturna também pertence ao dia inicial do turno. Não achei a permuta convertendo essas datas para outro referencial. Fuso efetivamente ativo em produção: [nao verificado]. (`app/ponto/turnos.py:2552`, `app/ponto/turnos.py:2553`, `app/escala/servico_jornada.py:69`, `app/escala/servico_jornada.py:89`, `app/ponto/models.py:765`)

**5. Classes que já reprovaram obra**

| Classe | Resultado da procura |
|---|---|
| **Segundo juiz que duplica registrado** | **Não achei duplicata registrada de interjornada ou descanso semanal previsto** [nao verificado]. As guardas locais **são decisões novas**, com piso de11h e semana segunda–domingo. Registro/corte específico delas: **não achei** [nao verificado]. O DSR financeiro do motor é outra pergunta. (`app/ponto/portas/celula.py:800`, `app/ponto/portas/celula.py:915`, `app/ponto/portas/celula.py:929`, `app/ponto/motor_calculo_v2.py:2565`) |
| **Segundo escritor de CelulaDia fora da porta** | **Não achei introduzido pelo diff.** A composição chega a `editar_dia`; a auditoria material continua em `core.observ`. (`app/ponto/portas/celula.py:1019`, `app/ponto/portas/celula.py:753`, `app/ponto/portas/celula.py:654`, `app/core/observ.py:134`) |
| **Relógio fora da borda** | **Presente no caminho:** edição lê `timezone.now`; rabo lê `timezone.localdate` quando executa. O diff passa a executar esse rabo no momento do commit, sem transportar o instante do ato. (`app/ponto/portas/celula.py:643`, `app/ponto/portas/celula.py:465`, `app/ponto/portas/celula.py:698`) |
| **Derivação paralela de fonte única** | `_turno_pos_permuta` delega horários a `marcos_turno`; **não achei clone dos horários do template**. Porém valida uma representação diferente das lâmpadas congeladas: `marcos_turno` encadeia datas, enquanto `montar_lampadas` desloca somente horas `< hi`. (`app/ponto/portas/celula.py:818`, `app/escala/servico_jornada.py:90`, `app/ponto/management/commands/gerar_celulas.py:67`) |
| **Selo sem mordida** | Há lacunas concretas: sucesso aceita cartas erradas/não aprovadas; `d1=d2` foi deixado sem selo; teste de trava examina SQL de uma única execução; testes do rabo usam mock. (`app/ponto/tests/test_o10b_editar_dia_troca.py:106`, `app/ponto/tests/test_o10d_permuta_reciproca.py:259`, `app/ponto/tests/test_o10d_permuta_reciproca.py:273`, `app/ponto/tests/test_o

[truncado em 20000 de 24707 caracteres]

## 17/11 03:09 · sessao 1c0230761cb3 · - · - · origem=Ronald (tmux)

Q3 — conclusões por inspeção; concorrência executada e incidência em produção: [nao verificado].

- **Permuta × permuta cruzada, com as quatro células existentes:** a ordem explícita é cartas por PK e células por `(colaborador_id,data)`; não achei inversão direta nessa sequência. As reentradas em `trocar_plantao` usam cartas já travadas pela própria permuta. `app/ponto/portas/celula.py:979-986`, `:1018-1020`, `:733-735`, `:751-753`.

- **Células ausentes: há interleaving que deadlocka.** Cadastro A12x36 trabalha03/11, B12x36 trabalha04/11; ambas19–07; vínculos vigentes e sem células03/04 ainda. P=`permutar(A,03,B,04)` e Q=`permutar(B,04,A,03)`, com cartas distintas. Ambas passam a pré-trava vazia; P insere A03,B03, Q insere A04,B04; no segundo par P espera A04 e Q espera A03. O código trava apenas linhas existentes, compõe pares na ordem dos argumentos e cria células posteriormente; unicidade por colaborador/data transforma esses INSERTs em disputa. `app/ponto/portas/celula.py:984-986`, `:1018-1020`, `:645-646`; `app/escala/models.py:1833`. Execução desse interleaving: [nao verificado].

- **Permuta × troca isolada, células ausentes:** mesmo risco. P acima insere A03,B03; T, com cartas distintas, troca A04→B03, inserindo A04 antes de esperar B03; P espera A04 no segundo par. A troca ordena seus dois lados, mas a permuta não preserva essa ordem de criação nas quatro células ausentes. `app/ponto/portas/celula.py:751-753`, `:984-986`, `:1018-1020`, `:645-646`. Execução: [nao verificado].

- **Interjornada valida estado não travado.** A24x48 07–07, âncora03/11, trabalha09 e12. P: A cede09 a B5/6x1 com folga segunda, B cede10 a A. Q: A cede12 a C6x1 com folga quinta, C cede11 a A. P e Q validam paralelamente: cada uma vê24h entre seu turno assumido e o turno original vizinho; suas células travadas são disjuntas. Depois de ambas, A trabalha10/11 07→11/11 07 e11/11 07→12/11 07: descanso0h. A pré-trava alcança só as quatro células da operação; vizinhos±2 são leituras comuns. `app/ponto/portas/celula.py:984-986`, `:905-925`. Execução: [nao verificado].

- **DSR também tem TOCTOU.** A5x2 08–17 folgas sábado/domingo; B e C5x2 08–17 folgas segunda/terça. P: A cede03/11, B cede14/11; Q: A cede24/11, C cede15/11. Cada validação vê uma folga restante no fim de semana14/15; juntas preenchem as duas, deixando A trabalhar09–15 inteiro. B conserva segunda02 e C segunda23 como descanso. Datas/fontes do exemplo são cadastro hipotético permitido pelo modelo. `app/escala/models.py:852-861`; `app/ponto/portas/celula.py:929-931`, `:984-986`. Execução: [nao verificado].

- **Regenerar × permuta: sobrescrita por snapshot antigo.** A regeneração lê células sem trava; decide `origem=='gerada'` no objeto carregado e salva `trabalha/dna` depois. Interleaving: R lê A03 gerada; P edita e commita A03 como folga; R salva sua leitura antiga. Como `origem` não está em `update_fields`, sobra `origem='editada'` e carimbo da permuta, mas `trabalha/dna` voltam ao cálculo da regeneração. `app/ponto/portas/celula.py:226-227`, `:298`, `:307-312`. Não achei inversão direta das travas de célula neste par quando todas existem: regeneração percorre datas crescentes e permuta trava por colaborador/data. `:228-229`, `:320`, `:984-986`. Execução: [nao verificado].

- **Gerar × permuta:** o gerador carrega células sem trava; se carregar A03 passada com `origem='gerada',dna=None`, e a permuta editar antes do enriquecimento, o gerador pode gravar o DNA calculado usando o `trabalha` antigo, deixando célula editada com `trabalha=False` e marcos de trabalho. Nas ausentes há corrida de INSERT/unicidade. `app/ponto/portas/celula.py:1200-1202`, `:1217-1230`; `app/escala/models.py:1833`. Execução: [nao verificado].

- **Guarda de lavra:** na permuta que chega a escrever, sim, é consultada depois da pré-trava das células; na edição direta/troca isolada, a guarda de cada edição vem antes da trava daquela célula. Regeneração consulta antes do atomic. Nenhum desses pontos trava a exportação/holerite, portanto não fecha corrida com lavra concorrente; `ExportacaoDominio.registrar` insere sem protocolo comum de trava de células. `app/ponto/portas/celula.py:984-986`, `:1010`, `:872-875`, `:613-638`, `:222-227`; `app/folha/models.py:174-187`; `app/ponto/management/commands/gerar_celulas.py:170-185`.

- **Selo insuficiente para concorrência:** o teste confere texto/posição de SQL numa chamada; não abre duas transações nem cobre ausência de células ou alteração de vizinhos. `app/ponto/tests/test_o10d_permuta_reciproca.py:273-291`.

## 17/11 03:08 · sessao c608e03f6016 · - · - · origem=Ronald (tmux)

Achados para consolidar (todos por leitura estática; ocorrência em produção [nao verificado]):

1. **Carta usada como documento também continua sendo ausência com efeito no dia assumido.** O fixture cria `tipo='declaracao', status='aguardando_decisao'`, e O10D reutiliza esse helper: `app/ponto/tests/test_o10b_editar_dia_troca.py:106`, `app/ponto/tests/test_o10d_permuta_reciproca.py:43`. A permuta exclui essas quatro linhas apenas da consulta de sobreposição: `app/ponto/portas/celula.py:897`. Os demais leitores não recebem essa exclusão:
   - Cobrança considera toda ausência não rejeitada; folha só aprovada: `app/ponto/turnos.py:2523`.
   - Cobertura usa data única ou intervalo inclusivo: `app/ponto/turnos.py:2550`.
   - Grade de folha busca cobertura aprovada: `app/escala/utils.py:1168`, `app/escala/utils.py:1188`; muda o dia para `ausencia`: `app/escala/utils.py:1210`, `app/escala/utils.py:1220`; ausência integral apaga marcos da grade: `app/escala/utils.py:1251`.
   - Precedência faz ausência ganhar de jornada e tipo desconhecido cair em atestado: `app/ponto/precedencia.py:46`, `app/ponto/precedencia.py:76`, `app/ponto/precedencia.py:379`, `app/ponto/precedencia.py:396`.
   - Contraexemplo: sucesso G01 (CED 03/11, SUB 04/11), com as mesmas cartas aprovadas de declaração integral: as células assumidas ficam `trabalha=True`, porém a grade as apresenta como ausência. Cadastro/data do caso: `app/ponto/tests/test_o10d_permuta_reciproca.py:129`; interpretação da ausência: referências acima. Não executado [nao verificado].

2. **Interjornada e descanso semanal locais são perguntas novas, não achei duplicata de juiz já registrado.** `core/juizes.py` não contém interjornada nem juiz de descanso semanal previsto [nao verificado: resultado negativo de busca]. DSR do motor é direito financeiro perdido por faltas, pergunta diferente: `app/ponto/motor_calculo_v2.py:2565`, `app/ponto/motor_calculo_v2.py:2593`, `app/ponto/motor_calculo_v2.py:2605`. Guardas novas estão em `app/ponto/portas/celula.py:915`, `app/ponto/portas/celula.py:929`; registro delas não achei [nao verificado].

3. **Mapa dos campos, leitores fora do diff:**
   - `trabalha`: fonte lida por `EscalaColaborador.eh_dia_trabalho`: `app/escala/models.py:1094`; precedência: `app/ponto/precedencia.py:347`, `app/ponto/precedencia.py:357`; cala cobrança com `False`: `app/chamados/juizes.py:443`, `app/chamados/juizes.py:459`; leitura lavrada e previsto: `app/escala/services/leitor_celula.py:397`, `app/escala/services/leitor_celula.py:411`; cartório protesta apenas se não False: `app/ponto/services/cartorio.py:1459`.
   - `dna`: marcos congelados: `app/escala/models.py:1124`; encadeamento aware de marcos: `app/escala/servico_jornada.py:76`, `app/escala/servico_jornada.py:89`; impressão e ata do cartório: `app/ponto/services/cartorio.py:1398`, `app/ponto/services/cartorio.py:1437`; grade alimenta juiz de turno com DNA: `app/escala/utils.py:1892`.
   - `origem='editada'`: bloqueia remoção de geradora ao restaurar snapshot: `app/colaboradores/services/vinculo.py:1025`; bloqueia desfazer ajuste que removeria vínculo novo: `app/colaboradores/services/vinculo.py:1103`; apresentação do dia: `app/ponto/services/dia_do_colab.py:91`. Regeneração e geração também a respeitam dentro do mesmo arquivo da porta: `app/ponto/portas/celula.py:120`, `app/ponto/portas/celula.py:1223`.
   - `editada_em`: fato novo para cartório: `app/ponto/services/cartorio.py:393`; invalida respostas lavradas: `app/chamados/veredito_lavrado.py:191`; reabre janela de censo de fase: `app/escala/services/fase_12x36.py:188`; carimbo de reescrita para cobrança órfã: `app/escala/services/regeneracao.py:72`, `app/escala/services/regeneracao.py:101`, `app/escala/services/regeneracao.py:119`, `app/escala/services/regeneracao.py:154`; apresentação localtime: `app/ponto/services/dia_do_colab.py:93`.
   - `editada_por`: escritor `editar_dia`: `app/ponto/portas/celula.py:646`, `app/ponto/portas/celula.py:653`; leitor de produção específico fora do diff não achei [nao verificado]. Modelo tem FK `SET_NULL`: `app/escala/models.py:1733`.
   - `LogAuditoria.valor_depois`: escritor material é `core.observ.evento`, CREATE: `app/core/observ.py:134`; adaptador `evento_kw`: `app/core/observ.py:143`. `permuta_id`/`cartas` são escritos pela porta: `app/ponto/portas/celula.py:1030`; leitor específico de `cartas` é só idempotência da própria permuta: `app/ponto/portas/celula.py:1005`. Leitor de `permuta_id` fora do diff não achei [nao verificado].
   - `Ausencia.status/data/data_fim/documento`: criação pela porta de ausência monta kwargs e valida/salva: `app/ponto/services/ausencia.py:483`, `app/ponto/services/ausencia.py:529`; edição escreve datas: `app/ponto/services/ausencia.py:649`; transição escreve status: `app/ponto/services/ausencia.py:976`; anexo escreve documento: `app/ponto/services/ausencia.py:1017`. `Ausencia.save` deriva fim inclusivo e dias: `app/ponto/models.py:904`, `app/ponto/models.py:917`; carimba aprovação/rejeição: `app/ponto/models.py:882`. Permuta não escreve esses campos, apenas os lê: `app/ponto/portas/celula.py:987`, `app/ponto/portas/celula.py:995`, `app/ponto/portas/celula.py:1019` (troca checa documento).

4. **Semântica temporal:**
   - `CelulaDia.data`/`Ausencia.data` são `DateField`, não instante: `app/escala/models.py:1685`, `app/ponto/models.py:750`.
   - Turno ancora em `dia`, avança a data quando marco seguinte `<=` anterior e usa timezone corrente com `make_aware`: `app/escala/servico_jornada.py:69`, `app/escala/servico_jornada.py:89`.
   - Ausência cobre data final inclusiva: `app/ponto/turnos.py:2552`, `app/ponto/turnos.py:2844`; NULL é aberto só para tipos específicos: `app/ponto/turnos.py:2553`. Help text do campo diz NULL indeterminado genericamente: `app/ponto/models.py:751` — divergência de descrição preexistente.
   - Parcial noturna pertence ao dia inicial do turno: `app/ponto/models.py:765`, `app/ponto/services/ausencia.py:1040`; bordas parciais inclusivas: `app/escala/utils.py:1239`.

5. **Possível derivação divergente de duração:** `_turno_pos_permuta` usa `marcos_turno`, que só encadeia quatro marcos: `app/escala/servico_jornada.py:76`, `app/escala/servico_jornada.py:82`; produtor de DNA já congela dois intervalos e lâmpadas: `app/ponto/management/commands/gerar_celulas.py:90`, `app/ponto/management/commands/gerar_celulas.py:101`; ordem canônica inclui os dois pares: `app/escala/models.py:1652`, `app/escala/models.py:1665`. Divergência concreta com cadastro aceito [nao verificado]; pede inspeção das restrições de pausa antes de elevar a defeito confirmado.

6. **A guarda de competência não duplica juiz registrado:** `_competencias_exportadas` enumera candidatas e chama `competencia_fechada_para_celula`: `app/ponto/management/commands/gerar_celulas.py:155`, `app/ponto/management/commands/gerar_celulas.py:161`. Essa autoridade está registrada K9: `app/core/juizes.py:346`, `app/core/juizes.py:353`.

## 17/11 03:07 · sessao 845411659453 · - · - · origem=Ronald (tmux)

Achados da pergunta 2, por inspeção estática:

- **Permuta e troca isolada:** o rabo é agendado dentro do `atomic` e executado por `transaction.on_commit`: `app/ponto/portas/celula.py:681-700`, `:773-775`, `:1034-1035`. A recusa vira exceção dentro do `atomic`, capturada fora dele; desfaz células, trilhas e callbacks: `:729-781`, `:972-1041`. **Não achei escrita de banco sobrevivente à devolução `'recusada'` nesses caminhos.** Execução real [nao verificado].
- **Editar direto sob transação externa continua vulnerável:** `editar_dia` encerra seu `atomic` interno e chama `_o_chao_do_dia_mudou` imediatamente (`app/ponto/portas/celula.py:637-677`). Sob `transaction.atomic()` do chamador, isso ocorre antes do commit externo. O rabo chama `julgar_celula(...apply_=True, forcar=True)` (`:465-473`), capaz de gravar julgamento/chamado e mandar push antes desse commit.
- Cadeia concreta do push: `app/ponto/services/cartorio.py:2011-2042` chama o corpo do julgamento; `:1776-1793` e `:1828-1831` chamam `_abrir_chamado`; `app/ponto/management/commands/detectar_ausencias.py:777-784` chama `push_supervisao`; `app/chamados/push_utils.py:89-111` encaminha para `_enviar_push_usuario`, que chama `webpush` diretamente em `:38-48`. Contraprova conceitual: `atomic externo → editar_dia(dia passado de trabalho sem batidas) → push → exceção/rollback externo`. Subscription/configuração e ocorrência em produção [nao verificado].
- **Dinheiro e reconciliação:** o recalculo já agenda `on_commit` em `app/ponto/services/fechamento.py:1798-1827`; rejunção/falta espúria também em `app/ponto/portas/celula.py:529-541`. Não achei dinheiro antes do commit por esses caminhos.
- **Marcação de cobranças órfãs:** ocorre antes do commit (`app/ponto/portas/celula.py:668-672` → `app/ponto/management/commands/gerar_celulas.py:250-265` → `app/chamados/services/contexto.py:97-105`), mas escreve contexto no mesmo `atomic`. O receiver do cartório retorna se o status não mudou (`app/chamados/signals.py:189-195`); não achei emissão de push nessa marcação.
- **Log fora do rollback:** falha de auditoria chama `registrar_engolido`, incrementa memória e executa `logging.error` imediatamente (`app/core/observ.py:73-82`, `:105-139`). A guarda de edição também registra `logger.exception` antes de devolver `'barrada'` (`app/ponto/portas/celula.py:618-628`), podendo a troca externa acabar `'recusada'`. Portanto a garantia literal de “nenhum log antes do commit” não vale. Há handlers de console/arquivo em `app/config/settings/saas.py:155-164`; uso desses settings e destino externo efetivo [nao verificado].
- **View pedido:** não achei view que chame `editar_dia` nesta cópia. Busca global por `editar_dia`, `trocar_plantao` e imports da porta encontrou apenas a própria porta, testes, comentários e documentação; existência do view [nao verificado]. Não assumir `ATOMIC_REQUESTS`: também não achei configuração dele na cópia. [nao verificado]

## 17/11 03:05 · sessao d89a88cca37a · E4C-F1-B2 · verificador · origem=Code

1. **Fila anterior resolvida: o mutante agora é discriminado.** O segundo alvo recebe `'\u00a0 '` e a asserção exige esse valor cru em `valor_antes`; substituir a fotografia por `resposta_colab=''` produz divergência para esse alvo. (app/chamados/tests/test_f1nova_b2_leitores.py:435–447; app/chamados/services/validacao.py:434–435)

   **Sim, a troca mantém a elegibilidade no cenário da fixture:** `respondida_sem_texto` considera esses brancos vazios; a face SQL também os inclui. A alteração mantém falso `pergunta_respondida`, preservando o caminho do C6 usado pela fixture viva. (app/chamados/juizes.py:1526–1552,1560–1572,1423; app/chamados/tests/test_f1nova_b2_leitores.py:240–244) A porta passa **ambas** as metades de destino, `(None, '')`, à recusa; esse par não satisfaz nenhuma das duas ilegalidades e pode ser gravado. (app/chamados/services/validacao.py:440; app/chamados/models.py:3381–3386; app/chamados/services/fio.py:1617–1625)

2. **Sem achado bloqueante de contrato na fixture.** O SQL contorna a validação de `save`, como o arquivo declara expressamente para fabricar legado ilegal. (app/chamados/tests/test_f1nova_b2_leitores.py:11–12,50–55,435) Entretanto, os contratos encontrados **excluem `tests/`**: escritor do par, texto com carimbo e escritores de estado. Não sustentam acusar esse delta de violar um contrato de escritores de teste. (app/chamados/tests/test_e3_porta_A_escritor_do_par.py:166–173; app/chamados/tests/test_contract_texto_com_carimbo.py:148–153; app/core/tests/test_contract_escritores_estado.py:186–195) Outro tripwire que inclua testes: **[não verificado]**.

3. **Nenhuma das três classes demonstrada no delta.** O selo passa a distinguir o mutante indicado; a escrita acrescentada pertence à fixture de legado; e preservar o texto cru na auditoria enquanto o juiz aplica `strip()` não introduz dois significados concorrentes para “respondida”. (app/chamados/tests/test_f1nova_b2_leitores.py:430–447,12; app/chamados/services/validacao.py:434–435; app/chamados/juizes.py:1526–1552)

Execução integral do teste Django/SQL e confirmação de HEAD: **[não verificado]**. [nao verificado]

## 17/11 03:05 · sessao f8906de56ab1 · - · - · origem=Ronald (tmux)

Q2: o delta escreve diretamente por `update` na fixture (`app/chamados/tests/test_f1nova_b2_leitores.py:435`), porém não localizei contrato que proíba isso em testes [não verificado]. O próprio arquivo declara que fixtures do par ilegal simulam legado por SQL (`app/chamados/tests/test_f1nova_b2_leitores.py:12`) e já fornece `_legado` com `update(**par)` fora das portas (`app/chamados/tests/test_f1nova_b2_leitores.py:50`).

Os contratos encontrados excluem testes explicitamente: [nao verificado]

- Escritor único do par: `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:168`.
- Texto e carimbo na mesma escrita: `app/chamados/tests/test_contract_texto_com_carimbo.py:152`.
- Censo de escritores de estado: `app/core/tests/test_contract_escritores_estado.py:191`.
- Censo geral: `app/core/censo_escritas.py:54`.

Logo, não há suporte nesses contratos para classificar a fixture como “segundo escritor de produção fora da porta”; o novo sítio fica em teste (`app/chamados/tests/test_f1nova_b2_leitores.py:430`). Nenhum achado bloqueante de contrato demonstrado por essa alteração; existência de outro selo que inclua `tests/` permanece [não verificado].

## 17/11 02:57 · sessao cdb003f546de · E4C-F1-B2 · verificador · origem=Code

**Não encontrei bloqueador funcional demonstrado no delta apresentado.** A delegação elimina a cópia do predicado, e os novos testes cobrem a ausência de trilha e a guarda de contagem que o laudo apontava. Restam filas de cobertura e de medição dos efeitos indiretos. (`app/chamados/juizes.py:1591`; `app/chamados/tests/test_f1nova_b2_leitores.py:430`; `app/chamados/tests/test_f1nova_b2_leitores.py:452`; `app/chamados/tests/test_f1nova_b2_leitores.py:469`.)

A cópia não contém `.git`: identidade do HEAD e integralidade do diff **[nao verificado]**. Executei somente leitura e classificação AST em memória; execução dos testes Django, mutantes com banco e suite **[nao verificado]**. [nao verificado]

**1. Escritores, leitores e significado dos campos — sem bloqueio demonstrado; há distinções importantes.**

| Campo/valor | Quem escreve | Quem lê e o significado usado |
|---|---|---|
| Log L130: `usuario`, `valor_antes`, `valor_depois` | A porta monta a fotografia crua anterior, o estado posterior e o autor; `evento` converte `ator` em `usuario` e grava os JSONs. (`app/chamados/services/validacao.py:434`; `app/chamados/services/validacao.py:449`; `app/core/observ.py:118`; `app/core/observ.py:134`.) | Os testes interpretam essa ação especificamente. Auditoria e fotografia E4B alcançam a linha genericamente; não encontrei consumidor de negócio específico para seus JSONs. (`app/chamados/tests/test_f1nova_b2_leitores.py:438`; `app/core/views.py:705`; `app/chamados/services/foto_e4b.py:311`.) |
| `respondida_em` / `resposta_colab` | Resposta humana pelo core; resposta automática; recuperação do instante pela trilha; `carimbar`, quando existe texto; zeragem pela porta do lote, usada por reabertura, ressuscitação e devolução L130. (`app/chamados/services/disputa_emissao.py:1481`; `app/chamados/services/disputa_emissao.py:1674`; `app/chamados/services/disputa_emissao.py:1696`; `app/chamados/models.py:3251`; `app/chamados/models.py:3402`; `app/chamados/models.py:1639`; `app/chamados/services/validacao.py:440`.) | Juiz Python, faces SQL, property e estado; vitalidade; retenção; fila de validação; validar/recusar; app. Todos esses caminhos usam o par ou delegam ao juiz. (`app/chamados/juizes.py:1526`; `app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1565`; `app/chamados/models.py:3675`; `app/chamados/models.py:3164`; `app/chamados/juizes.py:1423`; `app/chamados/models.py:1933`; `app/chamados/juizes.py:1763`; `app/chamados/services/validacao.py:682`; `app/chamados/services/validacao.py:902`; `app/api/views.py:2578`.) |
| `validada_em` | Atribuição positiva em `_carimbar_validacao` e na opção `resposta='validar'` de `carimbar`. Desfazem: nova resposta, devoluções da validação/paridade, reabertura e ressuscitação. (`app/chamados/services/validacao.py:168`; `app/chamados/models.py:3255`; `app/chamados/services/disputa_emissao.py:1494`; `app/chamados/services/validacao.py:273`; `app/chamados/services/validacao.py:349`; `app/chamados/services/disputa_emissao.py:1609`; `app/chamados/models.py:1639`.) | Estado, vitalidade, retenção, materializador, portas de validação e seleção do estoque. É assinatura/processamento da validação, não substituto de “respondida”. (`app/chamados/models.py:3161`; `app/chamados/juizes.py:1405`; `app/chamados/models.py:1934`; `app/chamados/services/materializacao.py:2056`; `app/chamados/services/validacao.py:685`; `app/chamados/services/validacao.py:392`.) |
| `materializada_em` / `via_resolucao` | Carimbo em `_escrever_carimbo`, pelas portas `carimbar`, `carimbar_identidade` e `descarimbar`; lote em `descarimbar_em_lote`. Há duas portas próprias que mudam somente a via: `encerrar_por_fato_no_espelho` e `relavrar`, declaradas no selo. (`app/chamados/models.py:3218`; `app/chamados/models.py:3224`; `app/chamados/models.py:3289`; `app/chamados/models.py:3324`; `app/chamados/models.py:3402`; `app/chamados/models.py:3413`; `app/chamados/services/relavra_via.py:125`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:51`.) | Estado, C6, espelho SQL, retenção, materialização, estoque e leitores do lastro. Um carimbo não prova sozinho que nasceu batida: existem vias de identidade e resolução sem batida. (`app/chamados/models.py:3161`; `app/chamados/juizes.py:1416`; `app/chamados/juizes.py:1489`; `app/chamados/models.py:3494`; `app/chamados/models.py:1934`; `app/chamados/services/materializacao.py:2067`; `app/chamados/services/validacao.py:394`.) |
| `DecisaoPergunta` | Escrita física por `registrar`. Produtores: nascimento/ressuscitação/fechamento/retratação no modelo; resposta/reabertura/automática/recuperação pela trilha; validação/devoluções/L130; absorção; silêncio do veredito; backfills. (`app/chamados/models.py:4134`; `app/chamados/models.py:1621`; `app/chamados/models.py:2015`; `app/chamados/models.py:2147`; `app/chamados/models.py:3139`; `app/chamados/services/disputa_emissao.py:1525`; `app/chamados/services/disputa_emissao.py:1631`; `app/chamados/services/disputa_emissao.py:1678`; `app/chamados/services/disputa_emissao.py:1699`; `app/chamados/services/validacao.py:173`; `app/chamados/services/validacao.py:279`; `app/chamados/services/validacao.py:354`; `app/chamados/services/validacao.py:439`; `app/chamados/services/materializacao.py:1048`; `app/ponto/services/veredito_celula.py:195`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:324`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:141`.) | Histórico de respostas, tarefa do dia, fio, rótulo de devolução BUG-PAR, backfill e fotografia E4B. (`app/chamados/models.py:3681`; `app/chamados/services/tarefa_do_dia.py:158`; `app/chamados/services/fio.py:263`; `app/chamados/services/materializacao.py:275`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:98`; `app/chamados/services/foto_e4b.py:280`.) |

**Não é correto dizer que toda leitura desses campos pergunta a mesma coisa.** A API pode serializar o instante; a lavra calcula sua maior data; o histórico preserva um carimbo legado mesmo com texto vazio. Esses usos tratam instante e histórico, enquanto `pergunta_respondida` decide validade da resposta. (`app/api/views.py:2563`; `app/chamados/veredito_lavrado.py:206`; `app/chamados/models.py:4112`; `app/chamados/juizes.py:1552`.)

Para decidir “respondida”, o vazio é `(texto or '').strip()`: NULL, `''` e somente brancos são equivalentes, inclusive NBSP. O SQL constrói os brancos a partir de `str.isspace()`. Já a trilha L130 conserva o texto anterior cru; `DecisaoPergunta` normaliza texto falso para `''`, mas conserva brancos. Isso corresponde a preservação histórica, não a outro juiz de resposta. (`app/chamados/juizes.py:1528`; `app/chamados/juizes.py:1560`; `app/chamados/services/validacao.py:434`; `app/chamados/models.py:4115`.)

A porta de integridade recusa os dois meios-estados; o `save` verifica o par que ficará gravado, e a porta do lote também verifica as metades recebidas. Existe uma corrida concorrente expressamente declarada para saves fora da trava canônica; não é introduzida por este delta. (`app/chamados/services/fio.py:1607`; `app/chamados/models.py:3581`; `app/chamados/models.py:3591`; `app/chamados/models.py:3381`.)

**2. Três testes novos — corrigem os dois bloqueios de teste do laudo, por leitura.** [nao verificado]

| Mutação | Resultado esperado nos novos testes | [nao verificado]
|---|---|
| **(a) Apagar `evento(...)`** | Falha a exigência de exatamente uma linha por pergunta; se `_log` ficar indefinido, ocorre exceção antes. (`app/chamados/tests/test_f1nova_b2_leitores.py:438`; `app/chamados/services/validacao.py:449`.) |
| **(b) Apagar `if _log is None: raise`** | Falha o `assertRaises(TrilhaNaoGravada)` da porta direta. (`app/chamados/tests/test_f1nova_b2_leitores.py:459`; `app/chamados/services/validacao.py:455`.) |
| **(c) Apagar a guarda de contagem** | O segundo alvo falha na trilha, o comando captura e continua com uma devolvida; sem a guarda, não levanta o `CommandError` exigido e conserva a primeira alteração. (`app/chamados/management/commands/estoque_l130_instante_sem_texto.py:103`; `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:111`; `app/chamados/tests/test_f1nova_b2_leitores.py:469`.) |
| **(d) Gravar ator/antes/depois diferentes dos esperados** | As igualdades exatas falham para os dois alvos da fixture. (`app/chamados/tests/test_f1nova_b2_leitores.py:441`.) |

O patch **alcança a chamada real**: a porta importa `evento` localmente durante cada chamada, dentro do contexto do patch. O interceptador captura previamente a função real e só devolve `None` para a ação e os PKs escolhidos. (`app/chamados/services/validacao.py:423`; `app/chamados/tests/test_f1nova_b2_leitores.py:421`; `app/chamados/tests/test_f1nova_b2_leitores.py:459`; `app/chamados/tests/test_f1nova_b2_leitores.py:476`.)

**Fila — mutantes plausíveis sobreviventes:** substituir o ANTES por `resposta_colab=''` ou `via_resolucao=''` passa nos três testes novos: ambos começam nesses defaults, e a fixture só altera o instante. Também não se confere a descrição do evento. A implementação atual preserva corretamente os valores crus; a lacuna é de cobertura para brancos/NBSP e via anterior não vazia. (`app/chamados/models.py:2562`; `app/chamados/models.py:2676`; `app/chamados/tests/test_f1nova_b2_leitores.py:234`; `app/chamados/tests/test_f1nova_b2_leitores.py:438`; `app/chamados/services/validacao.py:434`; `app/chamados/services/validacao.py:450`.)

**3. `q_nao_respondida` — mesmo conjunto para todas as combinações perguntadas.**

O ramo retirado é exatamente o retorno de `q_respondida_sem_texto`; a troca é substituição por chamada, sem nova negação nem mudança de tratamento de NULL. (`app/chamados/juizes.py:1572`; `app/chamados/juizes.py:1591`.)

| `respondida_em` | Texto NULL | `''` | Só brancos, inclusive NBSP | Texto após trim |
|---|---:|---:|---:|---:|
| NULL | inclui | inclui | inclui | inclui |
| Presente | inclui | inclui | inclui | exclui |

A tabela decorre dos mesmos ramos antes/depois; NULL no texto é caso lógico previsto pela face, embora o campo atual não aceite NULL. (`app/chamados/juizes.py:1568`; `app/chamados/juizes.py:1572`; `app/chamados/juizes.py:1591`; `app/chamados/models.py:2562`.)

Os **sete chamadores de produção** encontrados usam a face em `filter()`:

- Canal: `app/chamados/canal.py:66`.
- Universo da ressuscitação: `app/chamados/models.py:1533`.
- Reabrir disputa pela view: `app/chamados/views.py:1418`.
- Validar inline: `app/chamados/views_cobrar.py:178`.
- Auditor C2: `app/chamados/management/commands/auditar_invariantes_chamados.py:1607`.
- Ação de reabertura: `app/chamados/services/acoes_disputa.py:98`.
- Recobrança: `app/chamados/services/disputa_emissao.py:1735`.

Ressuscitação e auditor encadeiam `exclude` sobre outros campos; nesses sítios não há inspeção de `Q.children`, `~q_nao_respondida()` ou dependência de `annotate`. (`app/chamados/models.py:1533`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1607`.)

**4. Coletor da catraca — nenhum RESPONDEDOR atual escondido em outro arquivo; limite futuro em fila.**

Reconstruindo em memória o coletor anterior fornecido no anexo, o conjunto anterior tem estes **24 membros**; o posterior é esse conjunto mais as três faces SQL abaixo. Sementes e fechamento transitivo são calculados em `app/chamados/tests/test_e3_catraca_respondedores.py:116`.

```text
_dias_das_perguntas_vivas
_pergunta_materializada_de_fato
_pergunta_viva_calc
_resposta_concorda_com_a_luz
_slot_esperado
alimentar_portas
classificar_resposta_validacao
competencia_do_chamado
declarou_hora
delta_da_resposta
disputa_viva_do_chamado
fio_vivo
motivo_implausivel_validacao
pergunta_respondida
pergunta_sem_lastro_ou_viva
pergunta_viva
perguntas_corrigiveis
perguntas_respondidas_a_validar
perguntas_vivas_de
porta_alcancavel_do_dia
projecao_temporal_da_pergunta
respondida_sem_texto
texto_sem_carimbo
texto_vazio
```

| Novo membro | O que faz com o texto |
|---|---|
| `q_respondida_sem_texto` | Exige instante e texto NULL ou composto somente de brancos. (`app/chamados/juizes.py:1572`.) |
| `q_respondida_com_texto` | Exige instante e algum caractere fora dos brancos. (`app/chamados/juizes.py:1580`.) |
| `q_nao_respondida` | Inclui ausência de instante ou delega ao primeiro predicado. (`app/chamados/juizes.py:1591`.) |

Executei o classificador real em memória: respondedores atuais `[]`; reconstrução anterior também `[]`. Mantendo o código atual e trocando somente o coletor, a única classificação alterada é `q_nao_respondida`, de RESPONDEDOR para PAR; nenhum outro arquivo muda de papel. (`app/chamados/tests/test_e3_catraca_respondedores.py:422`; `app/chamados/tests/test_e3_catraca_respondedores.py:385`.)

**Fila — a ampliação admite falso PAR futuro.** O coletor aceita qualquer kwarg cujo primeiro segmento seja `resposta_colab`, sem exigir consulta, e atravessa corpos diferidos por `ast.walk`. Confirmei em memória que uma função acrescentada ao juiz contendo apenas `qs.update(resposta_colab="x")` faz um consumidor `p.respondida_em is not None and função(qs)` virar PAR. Esse membro não existe na árvore atual. (`app/chamados/tests/test_e3_catraca_respondedores.py:124`; `app/chamados/tests/test_e3_catraca_respondedores.py:131`; `app/chamados/tests/test_e3_catraca_respondedores.py:151`; `app/chamados/tests/test_e3_catraca_respondedores.py:390`.)

**5. Retração, sonda e EXPORTADA — docstring corrigida; medição incompleta em fila.**

A docstring bate com o código: universo de crédito, `ignorar_tranca=True`, validação/materialização ausentes e resposta pelo juiz instante **E** texto. (`app/chamados/juizes.py:2104`; `app/chamados/juizes.py:2115`; `app/chamados/models.py:1933`.)

As diferenças CH fornecidas são compatíveis com “15 chamados, 113 perguntas a menos”: as remoções listadas somam 113, sem adição. A docstring registra essa medida; reprodução em produção **[nao verificado]**. (`app/chamados/juizes.py:2107`.)

O anexo mostra perda da retenção nos dois destinos, encerramento forçado permitido nos dois, `_retratar=True` somente no CH23072 e liberação do predicado CSF nos dois. São resultados do dado fornecido; execução independente **[nao verificado]**.

| Leitor/caminho | Efeito da mudança e limite da sonda |
|---|---|
| `retencao_da_retratacao` → `_retratar` | Perder irmãs remove uma retenção; ainda existe a retenção de justificativa alheia durante aprovação e o gate de `reconciliar`. (`app/chamados/reconciliador.py:448`; `app/chamados/reconciliador.py:453`; `app/chamados/reconciliador.py:520`.) |
| `encerrar_com_retencao` | Usa o encerramento forçado, distinto de reconciliar: não passa pelos mesmos gates de premissa/conversa. Isso permite resultados diferentes; a causa específica do `False` do CH33525 é **[nao verificado]**. (`app/chamados/reconciliador.py:498`; `app/chamados/models.py:1686`; `app/chamados/models.py:683`.) |
| Justificativa | Aprovação pode retratar a origem; ajuste retroativo percorre a cascata e força encerramentos com retenção. Enumerar justificativas relacionadas não demonstra executar essas portas nem o contexto de aprovação. (`app/ponto/services/justificativa.py:41`; `app/ponto/services/justificativa.py:46`; `app/ponto/services/justificativa.py:393`; `app/chamados/reconciliador.py:429`.) |
| Materialização `_fecha` | Sem irmãs, tenta reconciliar destino e clones. Execução desse ramo não aparece demonstrada no anexo **[nao verificado]**. (`app/chamados/services/materializacao.py:1246`; `app/chamados/services/materializacao.py:1258`.) |
| HX do pai | Pai terminal em disputa aberta deixa de ser reaberto por meias-respostas. Imprimir pais e listas não demonstra executar a reabertura. (`app/chamados/services/disputa_emissao.py:2025`; `app/chamados/services/disputa_emissao.py:2031`.) |
| CSF e gate do pai | CSF reúne perguntas da origem **e** do destino, com a mesma opção de ignorar tranca. O gate do pai lê essa lista. A sonda cobre o predicado, não o fechamento completo. (`app/ponto/supra_juiz.py:573`; `app/ponto/supra_juiz.py:588`; `app/chamados/models.py:716`.) |
| Auditor I12 | Com TM fechado e chamado vivo, irmãs levam ao piso I12r; sem elas, o chamado entra na violação I12. Esse leitor não foi medido na saída fornecida **[nao verificado]**. (`app/chamados/management/commands/auditar_invariantes_chamados.py:980`; `app/chamados/management/commands/auditar_invariantes_chamados.py:986`.) |

**Há alcance automático estrutural em EXPORTADA; execução concreta hoje não está demonstrada.** O consumidor de eventos está configurado a cada minuto e chama retratação por célula e por premissa sobre vivos do colaborador, sem cerca de competência nesses laços. O gate adicional de pergunta viva exclui expressamente competências entregues de sua proteção. Uma retratação admitida escreve chamado, pode rejeitar justificativa e aciona resolução de perguntas. (`app/config/crons.py:394`; `app/chamados/reconciliador.py:1367`; `app/chamados/reconciliador.py:1375`; `app/chamados/reconciliador.py:998`; `app/chamados/reconciliador.py:1456`; `app/chamados/models.py:364`; `app/chamados/reconciliador.py:524`.)

Isso **não prova que os dois CH anexados sejam retratados por esse cron hoje**: faltam evento pendente e aprovação dos demais juízos; a retratação por célula exige veredito que absolve, enquanto o anexo mostra `cobrado`. Execução desses casos pelo consumidor **[nao verificado]**. (`app/chamados/reconciliador.py:1346`; `app/chamados/reconciliador.py:1025`.)

O cron CSF executável usa janela corrente e ainda exige lavra/código/gates antes da retratação; os dois dias de setembro estão fora da janela informada no anexo. Instalação/execução efetiva dos crons **[nao verificado]**. (`app/config/crons.py:599`; `app/ponto/management/commands/supra_juiz.py:85`; `app/ponto/management/commands/supra_juiz.py:249`.)

A devolução direta do estoque continua recusando competência entregue, e seu comando permanece declarado como ato único. Não confundir essa proteção com uma cerca geral dos consumidores indiretos. (`app/chamados/services/validacao.py:400`; `app/config/crons.py:791`.)

**6. Classes de reprovação — resultado neste recorte.**

| Classe | Resultado |
|---|---|
| Segundo juiz / derivação paralela | A delegação remove a duplicação do ramo SQL; retenção e CSF leem o corpo compartilhado. Sem nova duplicação demonstrada. (`app/chamados/juizes.py:1591`; `app/chamados/juizes.py:2117`; `app/ponto/supra_juiz.py:573`.) |
| Segundo escritor fora da porta | L130 zera pela porta do lote e registra histórico pela porta da sequência. As duas escritas próprias de via estão declaradas no selo. (`app/chamados/services/validacao.py:439`; `app/chamados/services/validacao.py:440`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:51`.) |
| Relógio fora da borda | No comando, `hoje` nasce uma vez e segue até a porta; o segundo relógio serve ao arquivo de reversão. Sem nova violação demonstrada nesse caminho. (`app/chamados/management/commands/estoque_l130_instante_sem_texto.py:36`; `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:62`; `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:105`.) |
| Selo/RED sem mordida | Os bloqueios anteriores ganharam asserções relevantes; permanecem **em fila** os mutantes de fotografia constante e o falso PAR futuro descritos acima. (`app/chamados/tests/test_f1nova_b2_leitores.py:441`; `app/chamados/tests/test_f1nova_b2_leitores.py:460`; `app/chamados/tests/test_f1nova_b2_leitores.py:477`; `app/chamados/tests/test_e3_catraca_respondedores.py:124`.) |
| Ordem de trava | Lote trava a união antes das portas e confere FKs/elegibilidade; porta usa a ordem canônica. Sem inversão nova demonstrada no caminho do comando. (`app/chamados/management/commands/estoque_l130_instante_sem_texto.py:78`; `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:91`; `app/chamados/services/validacao.py:429`.) |
| Campo com dois significados | Histórico de carimbo legado e resposta válida são usos distintos explic

[truncado em 20000 de 20360 caracteres]

## 17/11 02:53 · sessao 732e39fefeed · - · - · origem=Ronald (tmux)

**Q2 — correções do laudo têm mordida por leitura do fluxo; execução dos testes/mutantes não verificada.** [nao verificado]

| Mutação | O que falha | Fonte |
|---|---|---|
| Remover `evento(...)` | Teste exige exatamente um log por pergunta, com ação/modelo/id; se restar `_log` indefinido, falha antes por exceção. | `app/chamados/tests/test_f1nova_b2_leitores.py:438-445`; `app/chamados/services/validacao.py:449-455` |
| Remover `if _log is None: raise` | Teste direto exige `TrilhaNaoGravada` quando evento devolve `None`. Sem a guarda, retorna `True` e deixa o ato. | `app/chamados/tests/test_f1nova_b2_leitores.py:459-467`; `app/chamados/services/validacao.py:455-458` |
| Remover guarda de contagem | O segundo alvo falha na trilha; comando captura a exceção e continua, ficando `devolvida=1`. Sem `:111-113`, o primeiro alvo fica devolvido e o comando não levanta o `CommandError` exigido. | `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:103-115`; `app/chamados/tests/test_f1nova_b2_leitores.py:469-483` |
| Ator/antes/depois errados | Igualdade exata dos três é exigida para ambos os alvos da fixture. | `app/chamados/tests/test_f1nova_b2_leitores.py:441-445` |

O patch **alcança a chamada real**: a porta faz import local de `core.observ.evento` a cada chamada (`app/chamados/services/validacao.py:423`), dentro dos contextos do patch (`app/chamados/tests/test_f1nova_b2_leitores.py:459`, `:476`). `_evento_falha_para` captura o evento real antes do patch e intercepta apenas ação + pk selecionados (`app/chamados/tests/test_f1nova_b2_leitores.py:421-427`).

**Fila — mutantes plausíveis sobreviventes nos três testes novos.** Trocar o ANTES por `resposta_colab=''` e/ou `via_resolucao=''` passa, pois ambos os campos são default `''` (`app/chamados/models.py:2562`, `:2676-2678`) e a fixture não os varia (`app/chamados/tests/test_f1nova_b2_leitores.py:234-237`). O primeiro perderia brancos/NBSP históricos; o segundo perderia via histórica não vazia. A implementação atual preserva ambos crus (`app/chamados/services/validacao.py:434-435`). Alterar/remover `descricao` também não é conferido pelos três testes (`app/chamados/tests/test_f1nova_b2_leitores.py:438-445`; evento em `app/chamados/services/validacao.py:450-451`). Isso é limitação de cobertura, sem defeito atual demonstrado.

**Atomicidade, trava e relógio — sem nova violação demonstrada no caminho do comando.** Comando abre atomic do lote, trava união inteira e confere FKs/filtro antes de chamar portas (`app/chamados/management/commands/estoque_l130_instante_sem_texto.py:73-105`); porta abre savepoint/atomic e usa trava canônica (`app/chamados/services/validacao.py:428-430`). A implementação da trava ordena Colaborador, Disputa, Chamado, Pergunta (`app/chamados/models.py:2781`, `:2790`, `:2803`, `:2807`). `hoje` nasce na borda e é repassado ao predicado/porta (`app/chamados/management/commands/estoque_l130_instante_sem_texto.py:36`, `:42`, `:99`, `:105`); a porta não chama relógio diretamente (`app/chamados/services/validacao.py:408-458`). A data do arquivo de reversão usa segundo relógio para nome/metadado, não para elegibilidade (`app/chamados/management/commands/estoque_l130_instante_sem_texto.py:62-67`).

**Escritores/leitores do log específico.** Porta monta ANTES e DEPOIS e passa autor (`app/chamados/services/validacao.py:434-454`); escritor genérico mapeia `ator` para `usuario`, `antes/depois` para JSON (`app/core/observ.py:118`, `:134-137`). O evento usa savepoint próprio e retorna `None` na falha best-effort (`app/core/observ.py:105`, `:133-139`), justificando a guarda fail-closed da porta (`app/chamados/services/validacao.py:455-457`). Pela busca literal, consumidores específicos dessa ação são os testes (`app/chamados/tests/test_f1nova_b2_leitores.py:415`, `:438-450`, `:465`, `:483`); consumidor de negócio dinâmico adicional [não verificado]. Leitores genéricos alcançam a linha: auditoria filtra por ação/modelo/usuário (`app/core/views.py:705-714`), mas template só mostra usuário/ação/descrição, sem os JSONs (`app/templates/core/log_auditoria.html:55-74`); foto E4B captura toda auditoria dos sujeitos (`app/chamados/services/foto_e4b.py:311-323`).

**DecisaoPergunta — histórico de projeção, não selo de resposta válida L-130.** A porta grava o anterior pela sequência antes de zerar (`app/chamados/services/validacao.py:436-440`). O carregador histórico aceita qualquer `respondida_em != None`, inclusive texto vazio, e normaliza texto falso para `''` (`app/chamados/models.py:4105-4119`). Isso corresponde ao propósito de preservar projeção legada (`app/chamados/models.py:4095-4104`). `registrar` é porta única, com registros imutáveis (`app/chamados/models.py:4124-4137`, `:4084-4090`). O leitor da tarefa verifica o texto da linha RESPOSTA e devolve ação de responder se vazio (`app/chamados/services/tarefa_do_dia.py:159-162`), coerente com L-130. A trilha histórica não é prova de `pergunta_respondida`.

Leitores adicionais: coleção de decisões no fio (`app/chamados/services/fio.py:251-264`), tarefa ordenada por ato/instante (`app/chamados/services/tarefa_do_dia.py:421-430`), foto E4B (`app/chamados/services/foto_e4b.py:280-281`), rótulo BUG-PAR que só procura DEVOLUCAO (`app/chamados/services/materializacao.py:275-276`), histórico de respostas (`app/chamados/models.py:3688-3697`; delegado em `app/chamados/services/disputa_emissao.py:2119-2123`). Neste último, a única RESPOSTA vazia fica como “vigente” da sequência até aparecer outra resposta ou ato com foto, portanto não aparece imediatamente na lista de anteriores; a porta L130 deliberadamente não cria REABERTURA/DEVOLUCAO (`app/chamados/services/validacao.py:417-419`).

## 17/11 02:53 · sessao 2703bf549fd7 · - · - · origem=Ronald (tmux)

Q3/Q4: não encontrei bloqueio atual; a delegação preserva o predicado e o coletor não encobre outro respondedor nesta cópia. (`app/chamados/juizes.py:1572`, `app/chamados/juizes.py:1591`, `app/chamados/tests/test_e3_catraca_respondedores.py:116`)

- A expressão anterior era `A | (B & (C | D))`; `q_respondida_sem_texto()` devolve exatamente `B & (C | D)`. Logo permanece: instante NULL inclui qualquer texto; instante presente inclui NULL, `''` e somente brancos, inclusive NBSP, e exclui texto não vazio após trim. (`app/chamados/juizes.py:1528`, `app/chamados/juizes.py:1560`, `app/chamados/juizes.py:1572`, `app/chamados/juizes.py:1591`)
- São sete chamadores de produção, todos usam `filter()`: canal:66; models:1533; views:1418; views_cobrar:178; auditar_invariantes_chamados:1607; acoes_disputa:98; disputa_emissao:1735. `models` e auditor encadeiam `exclude` sobre outros campos; nenhum inspeciona a estrutura do Q, aplica `~` nessa face ou depende de `annotate`. (`app/chamados/canal.py:66`, `app/chamados/models.py:1533`, `app/chamados/views.py:1418`, `app/chamados/views_cobrar.py:178`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1607`, `app/chamados/services/acoes_disputa.py:98`, `app/chamados/services/disputa_emissao.py:1735`)
- Antes eram 24 predicados; depois, os mesmos 24 mais as três faces SQL: `q_respondida_sem_texto`, `q_respondida_com_texto`, `q_nao_respondida`. As três decidem pela resposta; nenhum novo membro é um uso de `update`/`values` sem decisão pelo texto. (`app/chamados/tests/test_e3_catraca_respondedores.py:122`, `app/chamados/juizes.py:1572`, `app/chamados/juizes.py:1580`, `app/chamados/juizes.py:1591`)

Conjunto anterior completo, obtido pelo coletor antigo reconstruído em memória:

```text
_dias_das_perguntas_vivas
_pergunta_materializada_de_fato
_pergunta_viva_calc
_resposta_concorda_com_a_luz
_slot_esperado
alimentar_portas
classificar_resposta_validacao
competencia_do_chamado
declarou_hora
delta_da_resposta
disputa_viva_do_chamado
fio_vivo
motivo_implausivel_validacao
pergunta_respondida
pergunta_sem_lastro_ou_viva
pergunta_viva
perguntas_corrigiveis
perguntas_respondidas_a_validar
perguntas_vivas_de
porta_alcancavel_do_dia
projecao_temporal_da_pergunta
respondida_sem_texto
texto_sem_carimbo
texto_vazio
```

O conjunto posterior é esse conjunto unido às três faces SQL acima. O cálculo das sementes e do fecho está em `app/chamados/tests/test_e3_catraca_respondedores.py:116-136`.

Executei o classificador real, com `python3 -B` e `PYTHONDONTWRITEBYTECODE=1`, sobre `production_sources()` e `templates_da_arvore()`, sem Django, banco ou arquivos criados. Resultado: antes e depois, respondedores vivos vazios; mantendo o código HEAD e trocando apenas o coletor, a única mudança é `juizes.q_nao_respondida:1591`, de RESPONDEDOR para PAR. Nenhum outro arquivo muda de papel. (`app/chamados/tests/test_e3_pendentes_app.py:96`, `app/chamados/tests/test_e3_catraca_respondedores.py:403`, `app/chamados/tests/test_e3_catraca_respondedores.py:422`)

**Fila — limite futuro:** o coletor aceita qualquer keyword cujo primeiro segmento seja `resposta_colab`, inclusive `resposta_colab=` em escrita, e usa `ast.walk`, que atravessa corpos diferidos. Assim, uma futura função de escrita pode entrar indevidamente no conjunto; isso não ocorre entre os novos membros atuais. (`app/chamados/tests/test_e3_catraca_respondedores.py:124`, `app/chamados/tests/test_e3_catraca_respondedores.py:131`, `app/chamados/tests/test_e3_catraca_respondedores.py:144`)

_6319 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
