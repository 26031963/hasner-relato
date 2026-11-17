# CODEX-ESTADO

> Ultima resposta processada: **17/11/2026 11:50** · publicado em **17/11/2026 11:50** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 17/11 11:50 · sessao c7e55a092905 · - · - · origem=Ronald (tmux)

Os quatro montadores de RESPOSTA encontrados declaram o campo: humana `False`; automática, carimbo pela trilha e foto da projeção `True`. A persistência permanece em `registrar`, via `bulk_create(cls(**ln))`. (`app/chamados/services/disputa_emissao.py:1525`, `app/chamados/services/disputa_emissao.py:1679`, `app/chamados/services/disputa_emissao.py:1701`, `app/chamados/models.py:4236`, `app/chamados/models.py:4313`)

Os backfills não acrescentam montador independente de RESPOSTA: `backfill_decisoes_pergunta.linhas_a_criar` consulta a mesma fábrica; sua escrita própria é SILENCIO. `backfill_fio_morto_e4b` registra o resultado da fábrica. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:67`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:324`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:141`)

**O tripwire não cobre todos os formatos pedidos.** Ele inspeciona somente chamadas cujo nome é literalmente `dict`, com `ato` explícito como atributo `RESPOSTA` ou constante `'resposta'`. Não vê `DecisaoPergunta(...)`, `objects.create(...)`, dicionário literal passado a `registrar`, `bulk_create([D(...)])`, montagem posterior por atribuição ou valor indireto de `ato`. A prova executada em memória devolveu `[]` para os quatro primeiros formatos. (`app/chamados/tests/test_f2_resposta_mantem_validacao.py:23`, `app/chamados/tests/test_f2_resposta_mantem_validacao.py:35`)

Não localizei escritor concreto de produção omitindo o campo fora desses quatro, nem fixture de produção de `DecisaoPergunta`; isso é resultado do censo da cópia, não garantia sobre fontes externas. [nao verificado]

O campo tem significado operacional único: RESPOSTA conserva ou desfaz a validação. Leitura Python e face SQL consultam o campo. O leitor decisório restante encontrado de `ato_motivo` restringe-se a DEVOLUCAO BUG-PAR, não RESPOSTA. (`app/chamados/models.py:4266`, `app/chamados/models.py:4284`, `app/chamados/models.py:3532`, `app/chamados/services/materializacao.py:275`)

A migração conserva a regra no domínio permitido: `''` permanece `False`; espaço e demais textos não vazios tornam-se `True`, sem trim. `NULL` não pertence ao esquema: o campo é não anulável desde a origem e continua assim. Portanto não há base para afirmar conservação sobre estoque hipotético com `NULL`; sua existência seria [nao verificado]. (`app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:12`, `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:25`, `app/chamados/migrations/0050_resposta_anterior.py:24`, `app/chamados/migrations/0051_decisao_pergunta.py:48`, `app/chamados/models.py:4147`)

A sequência declarada da 0055 é AddField → RunPython → SET CONSTRAINTS ALL IMMEDIATE → AddConstraint, repetindo o mecanismo usado pela 0051 antes de ALTER TABLE posterior. Aplicação efetiva no PostgreSQL permanece [nao verificado]. (`app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:22`, `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:27`, `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:29`, `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:30`, `app/chamados/migrations/0051_decisao_pergunta.py:101`)

O teste semântico novo tem mordida por construção: motivo preenchido com campo falso deve desfazer; motivo vazio com campo verdadeiro deve conservar. O teste da constraint usa VALIDACAO com campo verdadeiro e exige `IntegrityError`. Execução desses testes com banco: [nao verificado]. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:136`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:154`)

## 17/11 11:49 · sessao c2475fbabde5 · CORTE-FOLHA-ESPELHO · leitura · origem=Code

**O delta principal é retirar “disputa aberta” da decisão:** o corte passa a exigir pergunta viva por C6, dia dentro da competência e ação humana publicada pela tarefa do dia. Uma disputa fechada pode conter pergunta viva; uma aberta pode não conter nenhuma elegível. (`app/chamados/juizes.py:1403`; `app/docs/CORTES.md:409`; aplicação do complemento: [nao verificado])

**Sobre o erro 56712:** o caminho de leitura identificado alcança `tarefa → carga_da_vida → _slot_esperado → _ts_tipo_do_plantio → _tipo_pelo_marco`. A divergência registra `ERROR` e incrementa contador em memória; esse ponto não demonstra escrita na pergunta ou no banco. A ocorrência individual, a frequência nas últimas 24 horas e eventual erro devolvido ao usuário permanecem [nao verificado]. (`app/chamados/services/tarefa_do_dia.py:534`; `app/chamados/services/carga_pergunta_viva.py:119`; `app/chamados/juizes.py:375`; `app/chamados/services/materializacao.py:568`; `app/core/observ.py:73`)

**(i) Censo — somente o que muda**

| Leitores do pd.r | Delta |
|---|---|
| PDF auditável e suas cascas: emissão, lote, job, avulso por competência e anexação | Passam a consumir a resposta composta por competência. Hoje o bloqueio ainda pergunta apenas pela disputa aberta. (`app/relatorios/pdf_espelho.py:1085`; `app/relatorios/services.py:237`; `app/relatorios/views.py:1003`; `app/relatorios/views.py:1127`; mudança: [nao verificado]) |
| Classificador, montador/TXT e leitores indiretos da classificação enumerados no pd.r | Herdam a unidade nova pelo classificador; não recebem filtros próprios de pergunta. Hoje o ramo impeditivo consulta a célula. (`app/folha/export.py:716`; `app/folha/export.py:968`; mudança: [nao verificado]) |
| `dias_que_seguram`, `fora_da_folha`, `o_que_segura` | Precisam receber perguntas/dias elegíveis e ações da composição. Hoje selecionam apenas `furo_espelho` e buscam seus dias no retentor da célula. (`app/folha/fora_da_folha.py:121`; `app/folha/fora_da_folha.py:156`; `app/folha/fora_da_folha.py:203`; mudança: [nao verificado]) |
| Tarefa do dia/C19 | Entra como autoridade de ação e alcance, sem ganhar regra própria de retenção. Publica ações por instância e também portas residuais do dia. (`app/chamados/services/tarefa_do_dia.py:42`; `app/chamados/services/tarefa_do_dia.py:627`) |
| Retrato e tela do admin | O lavrador precisa gravar decisão, perguntas/dias e ações recebidos; a tela continua lendo o gravado. Hoje um motivo novo cairia no dono `admin`, com dias possivelmente ausentes e ações vazias. (`app/folha/retrato_folha.py:374`; `app/folha/retrato_folha.py:1214`; `app/folha/retrato_folha.py:1232`; `app/folha/retrato_folha.py:1249`; `app/folha/views_retrato.py:194`; mudança: [nao verificado]) |
| Prévia e cobertura da proposta | Precisam acompanhar a resposta composta: hoje a prévia consulta perguntas datadas, mas a cobertura da retenção compara dias da célula. (`app/folha/previa.py:64`; `app/folha/previa.py:169`; mudança: [nao verificado]) |
| Vigia de alcance | Herda o universo novo por `dias_que_seguram`; não deve medir apenas o antigo conjunto de furos. (`app/chamados/services/vigia_fio.py:510`; `app/chamados/services/vigia_fio.py:537`; mudança: [nao verificado]) |

**Não há migração geral do helper público.** Gates de cadastro/benefício, LEDs globais, drawer, cobranças, regularização externa, seleção/travamento/fechamento de recipientes e verbos continuam respondendo suas perguntas operacionais. HTML/API do espelho hoje apresentam existência global de disputa; isso não pode ser apresentado como impedimento desta competência sem consumir a composição. (`app/chamados/services/disputa_emissao.py:34`; `app/colaboradores/mixins.py:300`; `app/colaboradores/services/situacional.py:117`; `app/colaboradores/services/drawer.py:71`; `app/chamados/services/regularizacao_ext.py:59`; `app/ponto/views.py:142`; `app/api/views.py:1670`; alcance proposto: [nao verificado])

Badges e pendências do app conservam sua partição; o complemento não autoriza aproveitar esta obra para migrar a B2. Aprovação conserva a pergunta separada de `travas_aprovacao`; vigia de pausa conserva sua medição de L-033. (`app/chamados/services/pergunta_no_app.py:76`; `app/ponto/services/fechamento_lista.py:100`; `app/folha/services/vigia_pausa.py:125`; limite B2: [nao verificado — complemento, item 8])

**(ii) Onde mora a resposta única**

**Não encontrei o predicado completo já composto nos caminhos examinados.** C6 responde vida, C19 responde tarefa e K2 responde entrada no TXT; o registro os mantém separados. A inexistência absoluta fora desses caminhos permanece [nao verificado]. (`app/core/juizes.py:291`; `app/core/juizes.py:304`; `app/core/juizes.py:330`)

O endereço mais próximo é `tarefa_do_dia`, com `carga_da_tarefa(colaboradores, ini, fim, agora)`: a carga já seleciona perguntas factuais na janela inclusiva e consulta C6 em parte dos caminhos. Entretanto, tarefas normais também são produzidas pela sequência de decisões sem exigir C6 naquele ramo; pode haver validação administrativa sem pergunta viva. Portanto, C19 **não é hoje a resposta completa**. (`app/chamados/services/tarefa_do_dia.py:405`; `app/chamados/services/tarefa_do_dia.py:319`; `app/chamados/services/tarefa_do_dia.py:357`; `app/chamados/services/tarefa_do_dia.py:534`)

O pré-desenho deve declarar **uma composição única para o corte**, consumida por K2 e pelo PDF, reaproveitando C6, C19, janela e exceções existentes. **Essa composição seria juiz novo**; não é correto anunciá-la como função já existente nem alterar silenciosamente o helper global. Endereço proposto: módulo de corte compartilhado na fronteira da folha, ainda sem implementação ou registro. (`app/core/juizes.py:291`; `app/core/juizes.py:304`; `app/core/juizes.py:330`; proposta: [nao verificado])

A composição precisa preservar estes cuidados:

- O dia factual da pergunta já tem leitura própria: conversa não fornece dia do turno; a janela respeita o corte da empresa. (`app/chamados/juizes.py:338`; `app/ponto/janelas.py:54`)
- Não exigir `fechada_em IS NULL`: C6 não usa essa guarda e existe pergunta respondida viva em recipiente encerrado. (`app/chamados/juizes.py:1403`; `app/docs/CORTES.md:409`)
- Não usar `bool(porta)`: desconhecimento pode publicar envelope com `rota=None`. Tampouco `nao_sei=True` apaga uma ação que esteja publicada. (`app/chamados/services/tarefa_do_dia.py:719`)
- Não exigir ação no topo para reconhecer colaborador respondedor: `porta_app` pode ter `acao=None`; o responder aparece nas instâncias. (`app/chamados/services/tarefa_do_dia.py:651`; `app/chamados/services/tarefa_do_dia.py:679`; `app/chamados/services/tarefa_do_dia.py:701`)
- Conservar pergunta, dia, ação, responsável, porta e desconhecimento como evidências; o complemento não estabelece uma exigência adicional de igualdade de `pergunta_id` para todo verbo administrativo do dia. (`app/chamados/services/tarefa_do_dia.py:648`; `app/chamados/services/tarefa_do_dia.py:684`; contrato futuro: [nao verificado])

**(iii) Matriz**

Leio o “só segura” literalmente para o ramo espelho/disputa: um NÃO não pode voltar a ser FORA apenas pela mesma célula acusadora ou ausente, sem pergunta elegível e ação. Esses caminhos laterais existem hoje. Outras exclusões da folha continuam independentes. (`app/folha/export.py:1270`; `app/folha/export.py:1293`; `app/folha/export.py:1295`; `app/folha/export.py:959`; `app/folha/export.py:974`; interpretação: [nao verificado — complemento, itens 1–2])

Na tabela, cada resultado representa a sequência **export → PDF → retrato → tela do admin**:

- **R:** FORA pelo corte → informacional → grava perguntas/dias e ações impeditivos → mostra quem pode agir. [nao verificado — pré-desenho]
- **N:** sem impedimento por essa unidade → auditável permitido quanto a esse bloqueio → sem retenção atribuída à pergunta → sem pendência impeditiva desta folha. Não garante ENTRA contra outras exclusões. [nao verificado — pré-desenho]
- **E:** N, com incidente para nossa lista, dono ESTRUTURA/RAIZ-DA-DISPUTA; não vira tarefa do admin. [nao verificado — complemento, item 2]

| Estado da pergunta | Dia | Sem exceção L-033 | Só intervalo, L-033 comprovada |
|---|---|---|---|
| Viva com ação | Dentro | **R**. [nao verificado] | **N**; pergunta pode continuar viva. (`app/docs/LEIS.md:81`; aplicação PDF: [nao verificado]) |
| Viva com ação | Fora | **N**. [nao verificado] | **N**. [nao verificado] |
| Viva com ação aparente | Sem dia | **N**; ação sem dia identificado não demonstra tarefa elegível na janela. [nao verificado] | Não demonstrável sem identificar o dia; permanece **N**. [nao verificado] |
| Viva sem ação | Dentro | **E**. [nao verificado] | **E**, sem retenção. [nao verificado] |
| Viva sem ação | Fora | **E**. [nao verificado] | **E**, sem retenção. [nao verificado] |
| Viva sem ação | Sem dia | **E**. [nao verificado] | L-033 não demonstrável; **E**. [nao verificado] |
| Morta | Dentro | **N**. [nao verificado] | **N**. [nao verificado] |
| Morta | Fora | **N**. [nao verificado] | **N**. [nao verificado] |
| Morta | Sem dia | **N**. [nao verificado] | L-033 não demonstrável; **N**. [nao verificado] |

Pergunta morta normalmente é histórico; se restar recipiente sem ação, o recipiente residual vai para ESTRUTURA. Uma pessoa com pergunta elegível e outro beco continua retida pela primeira; o beco segue separado para nossa lista. [nao verificado — aplicação do complemento, itens 1–2]

L-033 exige protestos exatamente `{'FURO_PARCIAL'}`, sem desalinhamento, turno aberto ou desconhecimento, e somente lâmpadas de pausa apagadas. Motivo misto ou ponta faltante não recebe a exceção. (`app/folha/export.py:1150`; `app/folha/export.py:1155`; `app/folha/export.py:1173`)

O limite das competências entregues do pd.r permanece; o complemento não autoriza aplicar retroativamente o corte, tocar a porta do export ou produção. [nao verificado — pd.r fornecido; complemento, item 8]

**(iv) REDs — especificações, sem execução [nao verificado]**

Cada linha deve conferir a cadeia inteira da matriz, incluindo retrato e apresentação do responsável. [nao verificado — pré-desenho]

| Linha | Caso que morde → expectativa |
|---|---|
| Viva/ação/dentro | Disputa **fechada**, pergunta respondida viva, admin pode validar → R; controle com colaborador respondedor e `acao=None` no topo também retém. (`app/docs/CORTES.md:409`; `app/chamados/services/tarefa_do_dia.py:701`; expectativa: [nao verificado]) |
| Viva/ação/fora | Pergunta morta dentro e única viva acionável fora → N; detecta uso da menor data da disputa. (`app/chamados/juizes.py:85`; expectativa: [nao verificado]) |
| Viva/ação/sem dia | Fio aparenta ação, mas pergunta não tem dia factual → N; abertura/criação não substitui dia. (`app/chamados/juizes.py:338`; expectativa: [nao verificado]) |
| Viva/sem ação/dentro | Célula acusa, C6 viva, envelope apenas `nao_sei` e `rota=None` → E; detecta retenção residual e falso alcance. (`app/folha/export.py:1293`; `app/chamados/services/tarefa_do_dia.py:722`; expectativa: [nao verificado]) |
| Viva/sem ação/fora | Recipiente aberto, pergunta fora, ninguém age → E; PDF não bloqueia globalmente. (`app/relatorios/pdf_espelho.py:1085`; expectativa: [nao verificado]) |
| Viva/sem ação/sem dia | Pergunta sem dia e sem tarefa publicada → E; controle sem pergunta também não retém. (`app/chamados/services/tarefa_do_dia.py:408`; `app/relatorios/tests/test_relatorios.py:84`; expectativa: [nao verificado]) |
| Morta/dentro | Pergunta validada, célula ainda acusa, nenhuma outra elegível → N; detecta célula reintroduzindo o veto. (`app/chamados/juizes.py:1405`; `app/folha/export.py:1293`; expectativa: [nao verificado]) |
| Morta/fora | Disputa aberta somente com perguntas mortas fora → N, sem bloqueio auditável. (`app/chamados/juizes.py:1405`; `app/relatorios/pdf_espelho.py:1085`; expectativa: [nao verificado]) |
| Morta/sem dia | Pergunta validada sem dia em recipiente aberto → N; recipiente residual sem ação segue para ESTRUTURA. (`app/chamados/juizes.py:1405`; expectativa: [nao verificado]) |
| L-033 | Viva acionável dentro, entrada/saída presentes e só pausa faltando → N; controles com ponta faltante ou protesto adicional não ganham exceção. (`app/folha/export.py:1173`; expectativa compartilhada com PDF: [nao verificado]) |

Os controles transversais precisam incluir limites `ini/fim`, cortes distintos por empresa, várias perguntas/recipientes, passagem de viva para morta no mesmo relógio e coexistência de ação com desconhecimento. Isso morde `.first()`, resumo do recipiente e leituras em instantes diferentes. (`app/ponto/janelas.py:10`; `app/chamados/services/disputa_emissao.py:40`; `app/chamados/services/tarefa_do_dia.py:721`; `app/chamados/services/tarefa_do_dia.py:741`; expectativas: [nao verificado])

**(v) As três decisões de leigo**

1. **Sem descobrir o dia, a pessoa fica impedida por essa pergunta?** O complemento responde: não, pois exige dia dentro da janela; sem ação, vai para ESTRUTURA. [nao verificado — complemento, itens 1–2]

2. **Um problema deste mês já resolvido continua segurando porque a disputa ficou aberta por outro mês?** O complemento responde: não se C6 disser morta; só conta a pergunta viva, dentro e acionável. (`app/chamados/juizes.py:1403`; aplicação: [nao verificado])

3. **Passe livre e desligados continuam protegidos?** O complemento não responde nominalmente, mas a lei existente já responde: o retentor exclui ambos, e desligado não retém em nenhum dia da competência. Não sobra decisão nova nessa pergunta. (`app/folha/export.py:1197`; `app/folha/export.py:1207`; `app/core/constants.py:13`)

Não identifiquei nova pergunta de lei necessária neste delta; falta declarar e construir a composição, não decidir novamente vida, tarefa, janela ou essas proteções. (`app/core/juizes.py:291`; `app/core/juizes.py:304`; `app/ponto/janelas.py:54`; avaliação do delta: [nao verificado])

A classificação individual dos 29 e dos dois desconhecidos, a remedição dos 107 e sua distribuição por empresa/ação continuam [nao verificado]: o dado fornecido contém os totais mencionados no aval, sem as linhas individuais necessárias.

perguntas de lei: 0

## 17/11 11:48 · sessao d93020e59613 · CORTE-FOLHA-ESPELHO · verificador · origem=Code

**AVAL-DEVOLVIDO nos itens 1 e 2.** O complemento resolve a unidade e nomeia o C6, mas ainda deixa abertos o dia irresolúvel, a composição da L-033 e o significado operacional de “quem age”. (`app/chamados/juizes.py:338`; `app/chamados/juizes.py:1482`; `app/folha/export.py:1173`; `app/chamados/services/tarefa_do_dia.py:637`)

As referências aos anexos recebidos no enunciado, sem arquivo:linha consultável, ficam marcadas **[nao verificado]**.

No **item 1**, a premissa confere parcialmente:

1. **As premissas conferem no código?**

   “Pergunta viva pelo C6” tem autoridade existente: `pergunta_viva`, registrada em `core/juizes.py`; o wrapper memoriza e o corpo `_pergunta_viva_calc` julga. Não equivale a disputa aberta, pergunta sem resposta ou simples ausência de validação. O C6 considera validação, tranca, vias de encerramento, resposta, célula e testemunho da materialização; uma resposta pode conservar a pergunta viva para julgamento administrativo. (`app/core/juizes.py:291`; `app/chamados/juizes.py:866`; `app/chamados/juizes.py:1403`; `app/chamados/juizes.py:1423`)

   “De supervisão” corresponde à entidade `DisputaSupervisao`, ligada à pergunta por FK. O nascimento usual também cria chamado de categoria/módulo `disputa_supervisao`, área `supervisao`; não é inferência pelo papel de quem age. (`app/chamados/models.py:1774`; `app/chamados/models.py:2521`; `app/chamados/services/disputa_emissao.py:582`; `app/chamados/services/disputa_emissao.py:591`)

   Para pergunta **de fato**, `data_evento` é o dia do turno. Para **conversa**, o mesmo campo recebe o dia civil da conversa, que não é competência de fato. O helper usado pelo C6/tranca devolve `None` para conversa e `data_evento` para fato, sem alternativa pela célula. A tarefa também seleciona somente `conversa=False`, pela janela de `data_evento`. (`app/chamados/models.py:2888`; `app/chamados/models.py:2900`; `app/chamados/juizes.py:338`; `app/chamados/services/tarefa_do_dia.py:405`)

   Portanto, a fonte já existente para alinhar pergunta e tarefa é **dia do turno em `data_evento`, discriminando conversa**. Usar `celula.data` como alternativa muda o universo: essa alternativa existe no resolvedor do chamado-pai, mas não na seleção da tarefa. (`app/chamados/juizes.py:81`; `app/chamados/juizes.py:83`; `app/chamados/services/tarefa_do_dia.py:408`)

2. **Colide com aval ou lei vigente?**

   A unificação dos leitores concorda com testemunha lendo a mesma autoridade. Entretanto, **viva + dentro da janela + alguém age ainda não basta para SIM**: a L-033 mantém pagável o furo somente de intervalo, com pergunta aberta. A composição precisa chamar a isenção existente, sem deixar o novo bloqueio contorná-la. (`CLAUDE.md:20`; `app/docs/LEIS.md:81`; `app/folha/export.py:1158`; `app/folha/export.py:1293`)

   C6, C19, C7 e respostas sem veredito têm perguntas diferentes e autoridades registradas. Nenhum deles, individualmente, responde ao novo predicado combinado da folha; seu endereço e composição ainda precisam aparecer no pré-desenho. Não cabe declará-lo como autoridade existente de predicado e universo idênticos. (`app/core/juizes.py:291`; `app/core/juizes.py:292`; `app/core/juizes.py:304`; `app/core/juizes.py:308`; `CLAUDE.md:279`)

   O complemento não redefine “competência aberta” do aval anterior [nao verificado]. K6 significa existência de TXT vigente não invalidado; o marco `aberta` significa ausência dos demais marcos. São recortes distintos, portanto a pendência anterior de alcance permanece. (`app/folha/export.py:591`; `app/ponto/services/fechamento.py:1523`; `app/ponto/services/fechamento.py:1543`)

3. **Alguma condição não fecha no literal?**

   Dos quatro pontos da revisão anterior:

   | Ponto | Resultado da conferência |
   |---|---|
   | Vários fatos dentro e fora | **Fecha pela unidade pergunta**, desde que cada candidata seja examinada individualmente. Reutilizar o dia do pai reabre o defeito: ele devolve o menor dia. (`app/chamados/juizes.py:75`; `app/chamados/juizes.py:85`) |
   | Quais perguntas contam | **Fecha quanto à vida:** C6 é explícito no complemento [nao verificado]. Ainda falta discriminar fato/conversa e conservar expressamente o filtro de disputa aberta: C6 e `fechada_em` são condições distintas, e existe fluxo que reconhece pergunta viva em disputa fechada. (`app/chamados/juizes.py:338`; `app/chamados/models.py:1888`; `app/chamados/services/validacao.py:253`) |
   | Pergunta sem dia resolvível | **Segue aberto.** Os campos admitem NULL e C6 pode devolver viva sem dia nem célula. As portas atuais exigirem dia no nascimento não elimina essa possibilidade do modelo/acervo. (`app/chamados/models.py:2519`; `app/chamados/models.py:2528`; `app/chamados/models.py:2892`; `app/chamados/juizes.py:1482`) |
   | Composição da L-033 | **Segue aberta.** A isenção existe, mas não é consequência de consultar C6 ou tarefa. (`app/folha/export.py:1173`; `app/chamados/services/tarefa_do_dia.py:733`) |

   Pergunta sem dia não pode ser automaticamente classificada como “fora da janela” nem como “ninguém age”: falta insumo para consultar a tarefa daquele dia. O destino desses casos não está determinado nos itens 1 e 2 fornecidos [nao verificado]; a tarefa exige um dia dentro da carga. (`app/chamados/services/tarefa_do_dia.py:572`; `app/chamados/services/tarefa_do_dia.py:733`)

4. **Toca exportada, dinheiro ou `!` fora do declarado?**

   Implementado, o corte altera quem produz eventos financeiros: o montador omite quem não foi classificado como `entra`. Esse é o efeito pretendido, mas não autoriza alterar cálculo, escala, vínculo ou gravado exportado. (`app/folha/export.py:716`; `app/folha/export.py:726`; `CLAUDE.md:265`; `CLAUDE.md:411`)

   Também não basta proteger escritas: mudar o helper global mudaria leitores atuais sem janela, inclusive o PDF. O pré-desenho precisa preservar o alcance histórico e definir o limite de competência antes da troca. (`app/chamados/services/disputa_emissao.py:40`; `app/relatorios/pdf_espelho.py:1085`; `app/folha/export.py:591`)

No **item 2**, a direção “sem beco” é compatível com separar responsabilidade estrutural, mas o literal ainda não determina uma resposta única:

1. **As premissas conferem no código?**

   A tarefa responde por **colaborador + dia do turno + instância de marco**. Devolve `pergunta_id`, razões e ações `{acao,de,porta}`; depois publica `fio`, `rota`, `fila`, `verbos`, `acao` e `nao_sei`. O `fio` é uma seleção de quem age a seguir; as instâncias preservam os demais donos. (`app/chamados/services/tarefa_do_dia.py:42`; `app/chamados/services/tarefa_do_dia.py:46`; `app/chamados/services/tarefa_do_dia.py:54`; `app/chamados/services/tarefa_do_dia.py:733`)

   **Não existem exatamente os três pares citados.** Há `perguntar`, `responder`, `validar`, `materializar`, `rejulgar`, `decidir_ausencia`, `entregar_documento`, além de `resolver_dia` e `decidir` residual. Os donos das ações incluem `sistema`, `colaborador` e `admin`; a projeção também distingue `chamado`, `nada` e `nao_sei`. (`app/chamados/services/tarefa_do_dia.py:68`; `app/chamados/services/tarefa_do_dia.py:72`; `app/chamados/services/tarefa_do_dia.py:669`; `app/chamados/services/tarefa_do_dia.py:684`; `app/chamados/services/tarefa_do_dia.py:698`)

   Ação exigível não garante porta alcançável: resposta depende de pendência publicada no app; validação depende da âncora; sistema tem regras próprias de alcance. Portanto, contar somente `acoes` ou somente o nome do dono não reproduz a porta publicada. (`app/chamados/services/tarefa_do_dia.py:651`; `app/chamados/services/tarefa_do_dia.py:654`; `app/chamados/services/tarefa_do_dia.py:660`)

2. **Colide com aval ou lei vigente?**

   Encaminhar defeito comprovado do sistema à ESTRUTURA concorda com L-099. **L-007**, porém, trata de escritor canônico, tripwire e trilha; sozinha não demonstra que toda ausência de ação seja defeito estrutural. A tarefa também encerra exigências legitimamente por isenção, tranca ou marco ainda não vencido. (`CLAUDE.md:179`; `app/docs/LEIS.md:65`; `app/chamados/services/tarefa_do_dia.py:214`; `app/chamados/services/tarefa_do_dia.py:221`; `app/chamados/services/tarefa_do_dia.py:234`)

   O encaminhamento precisa compor com o retrato: o corte registrado anteriormente agrupava Chamado/Ninguém em Admin, e o código atual atribui `admin` a motivo diferente de `furo_espelho`. O novo destino estrutural exige integração explícita para não reaparecer automaticamente na lista administrativa. (`app/docs/CORTES.md:332`; `app/folha/retrato_folha.py:374`)

   L-033 e K6 continuam necessários: ter ação não revoga a isenção de intervalo nem define competência aberta. (`app/docs/LEIS.md:81`; `app/folha/export.py:591`; `app/ponto/services/fechamento.py:1543`)

3. **Alguma condição não fecha no literal?**

   Permanecem estas escolhas:

   - **Ação de qual pergunta?** A publicação agrega o dia inteiro e pode acrescentar `resolver_dia` sem `pergunta_id`. Falta dizer se basta ação de qualquer assunto daquele dia ou se ela precisa resolver a pergunta candidata. (`app/chamados/services/tarefa_do_dia.py:637`; `app/chamados/services/tarefa_do_dia.py:640`; `app/chamados/services/tarefa_do_dia.py:684`)
   - **Lista fechada ou exemplos?** O destino de `entregar_documento`, ações de sistema e demais verbos não está definido pelo parêntese do complemento [nao verificado]. Esses valores existem na tarefa. (`app/chamados/services/tarefa_do_dia.py:68`; `app/chamados/services/tarefa_do_dia.py:185`)
   - **Ausência conhecida versus desconhecimento.** Célula ausente produz `nao_sei`, não uma negativa de ação. Desconhecimento pode coexistir com ação administrativa; não se pode transformar todo `nao_sei` em “ninguém age” nem descartar sua ação. (`app/chamados/services/tarefa_do_dia.py:230`; `app/chamados/services/tarefa_do_dia.py:274`; `app/chamados/services/tarefa_do_dia.py:336`; `app/chamados/services/tarefa_do_dia.py:719`)
   - **Viva, na janela, sem ação:** o item propõe retirar essa retenção [nao verificado], mas falta dizer se retira somente o impedimento por pergunta ou também a retenção independente pela célula. Hoje célula ausente ou acusadora pode reter sem consultar tarefa; preservar esse ramo não garante liberar a pessoa. (`app/folha/export.py:1270`; `app/folha/export.py:1273`; `app/folha/export.py:1293`)
   - **Destino concreto:** RAIZ-DA-DISPUTA existe como obra ESTRUTURA, com esqueleto indicado, e sua F1 já recebe o desenho da disputa sem pergunta. Isso sustenta o encaixe documental. Arquivo receptor, responsável e forma de entrada da “lista nossa” e deste novo complemento ao brief permanecem [nao verificado]. (`app/docs/BACKLOG.md:2055`; `app/docs/CORTES.md:483`)

   A ocorrência e quantidade dessas combinações nos grupos citados pelo anexo não podem ser atribuídas individualmente com o dado fornecido **[nao verificado]**.

4. **Toca exportada, dinheiro ou `!` fora do declarado?**

   Liberar pessoas antes retidas altera o conjunto de eventos enviados e pode alterar a aprovação automática associada à geração do TXT. O pré-desenho deve declarar esse impacto, preservando o gravado exportado e os demais crivos. (`app/folha/export.py:726`; `app/folha/views.py:244`; `app/folha/views.py:246`; `CLAUDE.md:411`)

   “Não retém por esta pergunta” também não garante entrada no TXT: permanecem rescisão, código Domínio, férias com batida, cadastro e homologação. Tampouco garante PDF auditável, pois o lote tenta o informacional após qualquer exceção da tentativa auditável. (`app/folha/export.py:866`; `app/folha/export.py:872`; `app/folha/export.py:877`; `app/folha/export.py:959`; `app/folha/export.py:974`; `app/relatorios/services.py:244`)

Para os dois itens, os campos e valores determinantes têm estes escritores e leitores:

| Campo/valor | QUEM ESCREVE ou deriva | QUEM LÊ |
|---|---|---|
| Disputa: `colaborador`, `motivos`, `contexto`, `chamado` | Emissão cria; agregação atualiza motivos/contexto; há nascimentos por órfãos e âncora. (`app/chamados/services/disputa_emissao.py:550`; `app/chamados/services/disputa_emissao.py:576`; `app/chamados/services/disputa_emissao.py:601`; `app/chamados/services/disputa_emissao.py:1137`; `app/chamados/services/disputa_emissao.py:1372`) | Identidade da pergunta, C6 e resolução do chamado. (`app/chamados/models.py:3597`; `app/chamados/juizes.py:1474`; `app/chamados/juizes.py:75`) |
| Disputa: `fechada_em` / aberta | `fechar`; reaberturas admin/sistema zeram. (`app/chamados/models.py:2013`; `app/chamados/models.py:2230`; `app/chamados/models.py:2254`) | Property aberta, bloqueador global e partição do app. (`app/chamados/models.py:1888`; `app/chamados/services/disputa_emissao.py:40`; `app/chamados/services/pergunta_no_app.py:81`) |
| Pergunta: `disputa`, `conversa`, `motivo`, texto | Portas fato/conversa e `_nascer`; emissor fornece texto e motivo. (`app/chamados/models.py:2894`; `app/chamados/models.py:2906`; `app/chamados/models.py:3124`; `app/chamados/services/disputa_emissao.py:448`) | Helper do dia, seleção da tarefa e posição do marco. (`app/chamados/juizes.py:342`; `app/chamados/services/tarefa_do_dia.py:405`; `app/chamados/services/tarefa_do_dia.py:130`) |
| Pergunta: `data_evento` | Atribuição única `gravar_dia_do_fato`; nascimento, costura e re-chaveamento persistem. (`app/chamados/services/fio.py:837`; `app/chamados/models.py:3126`; `app/chamados/services/fio.py:926`; `app/chamados/services/fio.py:1090`) | C6/tranca pelo helper; carga da tarefa pela janela. (`app/chamados/juizes.py:338`; `app/chamados/juizes.py:995`; `app/chamados/services/tarefa_do_dia.py:408`) |
| Pergunta: `celula`, `colaborador`, `instancia_marco`, `batida_origem` | Nascimento deriva identidade; reuso/re-chaveamento podem reapontar célula. (`app/chamados/models.py:3592`; `app/chamados/models.py:3616`; `app/chamados/models.py:3108`; `app/chamados/services/fio.py:1085`) | C6, juiz do turno e posicionamento na tarefa. (`app/chamados/juizes.py:1418`; `app/chamados/services/fio.py:1701`; `app/chamados/services/tarefa_do_dia.py:132`) |
| `resposta_colab`, `respondida_em` | Resposta humana/automática, reparo pela trilha; carimbo pode preencher instante; ressuscitação limpa. (`app/chamados/services/disputa_emissao.py:1481`; `app/chamados/services/disputa_emissao.py:1674`; `app/chamados/services/disputa_emissao.py:1696`; `app/chamados/models.py:3240`; `app/chamados/models.py:1555`) | C21, projeção temporal/C6 e pendência do app. (`app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1389`; `app/chamados/juizes.py:1423`; `app/chamados/services/pergunta_no_app.py:81`) |
| `validada_em` | Validação carimba; devolução, resposta nova e ressuscitação limpam; carimbo também pode preencher. (`app/chamados/services/validacao.py:168`; `app/chamados/services/validacao.py:273`; `app/chamados/services/disputa_emissao.py:1494`; `app/chamados/models.py:1554`; `app/chamados/models.py:3245`) | C6 e juiz de respostas sem veredito. (`app/chamados/juizes.py:1405`; `app/chamados/models.py:1923`) |
| `materializada_em`, `via_resolucao` | `_escrever_carimbo`, pelas portas carimbar/identidade/descarimbar e lote. (`app/chamados/models.py:3183`; `app/chamados/models.py:3207`; `app/chamados/models.py:3362`) | C6 e exclusões da tarefa. (`app/chamados/juizes.py:1416`; `app/chamados/juizes.py:1482`; `app/chamados/services/tarefa_do_dia.py:255`) |
| Célula: dia/DNA; `ata`, `veredito`, `veredito_via`, `protestos`; lâmpadas | Portas geram/editam célula; `lavrar_veredito` escreve julgamento/ata; leitor projeta `acesa_lavrada`. (`app/ponto/portas/celula.py:638`; `app/ponto/portas/celula.py:863`; `app/ponto/portas/celula.py:1032`; `app/escala/services/leitor_celula.py:137`) | C6, tarefa e isenção L-033. (`app/chamados/juizes.py:1454`; `app/chamados/services/tarefa_do_dia.py:108`; `app/folha/export.py:1173`) |
| Empresa: `dia_inicio_competencia`; mês/ano e janela | Tela fornece cadastro às portas de empresa; janela é derivada pelo juiz. Mês/ano são parâmetros da consulta. (`app/colaboradores/views.py:515`; `app/colaboradores/views.py:542`; `app/colaboradores/services/estrutura.py:13`; `app/ponto/janelas.py:54`) | Export, tarefa e lote PDF. (`app/folha/export.py:1306`; `app/chamados/services/tarefa_do_dia.py:408`; `app/relatorios/services.py:214`) |
| `agora`; tranca do dia | Relógio entra pela borda; portas trancam/reabrem período; C20 deriva tranca com toque admin. (`app/chamados/services/tarefa_do_dia.py:737`; `app/ponto/services/fechamento.py:1400`; `app/ponto/services/fechamento.py:1432`; `app/chamados/juizes.py:978`) | Projeção temporal C6, corte dos atos e tarefa. (`app/chamados/juizes.py:1391`; `app/chamados/services/tarefa_do_dia.py:423`; `app/chamados/services/tarefa_do_dia.py:459`) |
| Decisão: `pergunta`, `ato`, `ato_em`, `ato_texto`, `absorcao_ausencia` | Porta única `DecisaoPergunta.registrar`; resposta, validação/devolução e absorção fornecem atos. (`app/chamados/models.py:4110`; `app/chamados/services/disputa_emissao.py:1525`; `app/chamados/services/validacao.py:173`; `app/chamados/services/validacao.py:279`; `app/chamados/services/materializacao.py:1045`) | Carga ordena por instante/PK e tarefa lê a última decisão aplicável. (`app/chamados/services/tarefa_do_dia.py:421`; `app/chamados/services/tarefa_do_dia.py:158`) |
| Pergunta: `veredito_lavrado`, `julgado_em`, `janela_expira_em` | `lavrar` e gravador de lote; portas também conservam lavra no mesmo ato. (`app/chamados/veredito_lavrado.py:254`; `app/chamados/veredito_lavrado.py:240`; `app/chamados/services/fio.py:1089`) | Carga verifica vigência e tarefa consulta efeito. (`app/chamados/services/tarefa_do_dia.py:432`; `app/chamados/services/tarefa_do_dia.py:168`) |
| Ausência: `status`, período/cobertura | Portas `mudar_estado`, `decidir` e `_aplicar_periodo`. (`app/ponto/services/ausencia.py:955`; `app/ponto/services/ausencia.py:1051`; `app/ponto/services/ausencia.py:1057`) | Tarefa distingue documento, decisão admin e aprovação. (`app/chamados/services/tarefa_do_dia.py:179`; `app/chamados/services/tarefa_do_dia.py:242`; `app/chamados/services/tarefa_do_dia.py:309`) |
| Efeito: batida ligada por `pergunta_origem` | Materialização vincula pela porta `anotar`. (`app/chamados/services/materializacao.py:1509`) | Carga lê batidas apuráveis existentes até `agora`; tarefa distingue efeito e lâmpada. (`app/chamados/services/tarefa_do_dia.py:442`; `app/chamados/services/tarefa_do_dia.py:173`) |
| Chamado: `status_local`, silêncio; fila e verbos derivados | `_gravar_estado`; portas de arquivo; `alimentar_fila` deriva mapas pelo painel. (`app/chamados/models.py:201`; `app/chamados/services/arquivo.py:166`; `app/chamados/services/arquivo.py:202`; `app/chamados/juizes.py:1583`) | Publicação de alcance e dono da tarefa. (`app/chamados/services/tarefa_do_dia.py:637`; `app/chamados/services/tarefa_do_dia.py:665`) |
| `pendentes_app` | Partição do app cruzada com C6 pela carga. (`app/chamados/services/pergunta_no_app.py:76`; `app/chamados/services/tarefa_do_dia.py:529`; `app/chamados/services/tarefa_do_dia.py:539`) | Publicação de `porta_app`. (`app/chamados/services/tarefa_do_dia.py:651`) |
| Tarefa e porta: instâncias, ações, razões, desconhecimento, rota, fio, fila | Derivação pela tarefa e `_portas_da_tarefa`; `porta_da_tarefa` projeta. (`app/chamados/services/tarefa_do_dia.py:190`; `app/chamados/services/tarefa_do_dia.py:708`; `app/chamados/services/tarefa_do_dia.py:712`) | Folha, Vigia e furo sem canal. (`app/folha/fora_da_folha.py:70`; `app/chamados/services/vigia_fio.py:357`; `app/chamados/services/furo_sem_canal.py:94`) |
| K6: tripla, vigência, invalidação; ENTREGUE | `ExportacaoDominio.registrar`/`invalidar`; vigência e ENTREGUE são derivados. (`app/folha/models.py:174`; `app/folha/models.py:158`; `app/folha/models.py:224`; `app/folha/export.py:591`) | Marcos da competência e tarefa. (`app/ponto/services/fechamento.py:1537`; `app/chamados/services/tarefa_do_dia.py:479`) |
| Nova resposta SIM/NÃO; lista ESTRUTURA | Endereço, derivador e escritor/destinatário concreto da lista ainda

[truncado em 20000 de 20634 caracteres]

## 17/11 11:47 · sessao 3da0e9f6bd91 · - · - · origem=Ronald (tmux)

Conclusões para consolidar, só da cópia e do anexo:

- A unidade nova **não exige disputa aberta**: C6 lê validação, tranca, resolução, resposta, célula e fato; não lê `disputa.fechada_em`. Logo uma pergunta viva com ação dentro de disputa fechada precisa entrar na avaliação; não conservar o filtro A do pd.r. (`app/chamados/juizes.py:1403`, `app/chamados/juizes.py:1423`, `app/chamados/juizes.py:1482`) Existe corte literal de pergunta respondida que continua viva, “só Validar”, mesmo com disputa encerrada. (`app/docs/CORTES.md:409`)
- **Não basta acrescentar uma retenção à classificação atual.** O ramo atual retém por célula ausente/acusadora sem exigir pergunta ou ação; manter esse ramo como impedimento independente permitiria reter exatamente os becos que o complemento manda liberar. (`app/folha/export.py:1270`, `app/folha/export.py:1293`, `app/folha/export.py:1295`; interpretação do complemento: [nao verificado])
- “Sem impedimento pela pergunta” precisa preservar as **outras exclusões** — rescisão, código, férias com batida, cadastro zero, catálogo —, sem significar ENTRA garantido. (`app/folha/export.py:866`, `app/folha/export.py:872`, `app/folha/export.py:877`, `app/folha/export.py:959`, `app/folha/export.py:974`)

A matriz nova tem uma única combinação afirmativa: **C6 viva + ação da própria pergunta + dia dentro + não L-033**; as demais não retêm por essa unidade. Isso é expectativa do complemento, ainda não implementação. [nao verificado]

| Estado | Dentro, não L-033 | Fora, não L-033 | Sem dia | L-033 comprovada |
|---|---|---|---|---|
| Viva com ação | Retém / PDF informacional | Não retém / auditável permitido | Não retém / auditável permitido | Não retém / auditável permitido |
| Viva sem ação | Não retém; ESTRUTURA | Não retém; ESTRUTURA | Não retém; ESTRUTURA | Não retém; ESTRUTURA |
| Morta | Não retém por essa pergunta | Não retém por essa pergunta | Não retém por essa pergunta | Não retém por essa pergunta |

Todas as expectativas da tabela: [nao verificado — complemento fornecido]. Retrato/tela devem registrar a mesma decisão, evidências e dono; sem ação não pode virar pendência do admin. [nao verificado — complemento fornecido]

A exceção L-033 exige protestos **exatamente** `{'FURO_PARCIAL'}` e resposta do juiz dos períodos sem desalinhamento, turno aberto ou desconhecimento, com alguma lâmpada apagada e todas de pausa. “Sem dia” não permite afirmar L-033; a retenção já é NÃO pela falta da condição temporal, sem inventar exceção. (`app/folha/export.py:1150`, `app/folha/export.py:1155`, `app/folha/export.py:1173`)

REDs que mordem a mudança, especificações sem execução: [nao verificado]

| Linha | Caso que morde |
|---|---|
| Viva com ação / dentro | Disputa **fechada**, pergunta respondida viva, admin pode validar; célula não retinha antes: deve reter; controle com mesma pergunta fora libera. (`app/chamados/juizes.py:1423`; cenário/expectativa: [nao verificado]) |
| Viva com ação / fora | Primeira pergunta dentro já morta; única viva acionável fora: não reter nem herdar `min(data)` da disputa. (`app/chamados/juizes.py:85`; expectativa: [nao verificado]) |
| Viva com ação / sem dia | Disputa aberta, tarefa aparenta ação, dia ausente: não transformar abertura/criação em dia dentro. (`app/chamados/models.py:1820`, `app/chamados/models.py:2899`; expectativa: [nao verificado]) |
| Viva sem ação / dentro | Célula acusadora e pergunta C6 viva, tarefa sem verbo: liberar impedimento e registrar ESTRUTURA; detecta retenção residual da célula. (`app/folha/export.py:1293`; expectativa: [nao verificado]) |
| Viva sem ação / fora | Disputa aberta fora, nenhum verbo: não bloquear PDF e não mandar para admin. (`app/relatorios/pdf_espelho.py:1085`; expectativa: [nao verificado]) |
| Viva sem ação / sem dia | Recipiente aberto sem pergunta datada/acionável: não bloqueia PDF; lista ESTRUTURA. O teste atual cria disputa sem pergunta e exige `ValueError`; agora essa expectativa é superada pelo complemento. (`app/relatorios/tests/test_relatorios.py:84`; expectativa: [nao verificado]) |
| Morta / dentro | Célula ainda acusa, mas pergunta validada/morta e nenhum outro credor elegível: não reter por célula solta. (`app/chamados/juizes.py:1405`, `app/folha/export.py:1293`; expectativa: [nao verificado]) |
| Morta / fora | Disputa aberta só com perguntas mortas fora: auditável permitido; abertura não substitui C6. (`app/relatorios/pdf_espelho.py:1085`; expectativa: [nao verificado]) |
| Morta / sem dia | Pergunta validada sem dia em recipiente aberto: C6 morta prevalece, sem bloqueio PDF. (`app/chamados/juizes.py:1405`; expectativa: [nao verificado]) |
| L-033 / viva acionável dentro | Entrada/saída batidas, apenas pausa faltando: pergunta continua viva, não retém nem torna PDF informacional; controle com ponta faltante ou segundo protesto não ganha exceção. (`app/docs/LEIS.md:81`, `app/folha/export.py:1173`; expectativa PDF: [nao verificado]) |

Sobre as três perguntas leigas do pd.r:

1. **Sem saber o dia, a pessoa fica impedida?** Complemento responde: não por esta unidade, pois exige dia dentro. Sem ação, dono ESTRUTURA; sem dia com ação não autoriza inventar data. [nao verificado — complemento fornecido]
2. **Problema deste mês já resolvido continua impedindo porque a disputa segue aberta por outro mês?** Complemento responde: não se C6 disser morta; só a viva acionável dentro conta. Recipiente fechado também não prova morte. (`app/chamados/juizes.py:1403`; aplicação do complemento: [nao verificado])
3. **Passe livre e desligados continuam protegidos?** Complemento não responde nominalmente, mas a lei anterior já responde: o retentor retorna vazio para isento ou desligado, com literal “desligado NÃO retém … NENHUM dia”. Preservar essa proteção não requer nova pergunta de lei. (`app/folha/export.py:1197`, `app/folha/export.py:1207`; `app/core/constants.py:13`; `app/docs/CORTES.md:441`)

Não identifiquei pergunta de lei nova necessária nesta parte; dúvidas de “como correlacionar pergunta e verbo” são contrato da autoridade/implementação, não autorização para inventar critério. [nao verificado]

## 17/11 11:47 · sessao a732081df2ae · - · - · origem=Ronald (tmux)

**Autoridade:** não encontrei função existente que responda integralmente “há pergunta viva C6 desta pessoa, dentro da competência, com ação humana publicada pela tarefa?”. O registro separa C6, C19 e K2; portanto dar endereço único a esse predicado completo seria **juiz novo de composição**, reaproveitando essas autoridades. A busca realizada não prova inexistência absoluta fora dos caminhos examinados. (`app/core/juizes.py:291`; `app/core/juizes.py:304`; `app/core/juizes.py:330`; inexistência absoluta: [nao verificado])

O endereço existente mais próximo é `chamados/services/tarefa_do_dia.py::tarefa_do_dia`, com `carga_da_tarefa(colaboradores, ini, fim, agora)`: a carga seleciona perguntas factuais, `conversa=False`, por `data_evento` inclusivo na janela, e consulta C6 para as exposições especiais e para as pendentes do app. Contudo, a tarefa normal de marco também pode nascer da sequência de decisões sem consultar C6 nesse ramo. (`app/chamados/services/tarefa_do_dia.py:405`; `app/chamados/services/tarefa_do_dia.py:485`; `app/chamados/services/tarefa_do_dia.py:534`; `app/chamados/services/tarefa_do_dia.py:319`; `app/chamados/services/tarefa_do_dia.py:334`)

**Não serve `bool(porta_da_tarefa(...))`:** desconhecimento publica envelope com `rota=None`; a tarefa também oferece validar célula sem pergunta viva, Resolver dia ou verbo residual de fila. Logo é preciso ler a ação humana publicada e manter a exigência independente de pergunta viva C6 na janela. Não criar novo juiz para “quem age”: essa resposta já pertence a C19. (`app/chamados/services/tarefa_do_dia.py:357`; `app/chamados/services/tarefa_do_dia.py:665`; `app/chamados/services/tarefa_do_dia.py:684`; `app/chamados/services/tarefa_do_dia.py:698`; `app/chamados/services/tarefa_do_dia.py:712`; `app/core/juizes.py:159`)

O literal fornecido não exige explicitamente que a ação esteja na instância com o mesmo `pergunta_id`; exige que a tarefa do dia dê ação. Introduzir essa exigência adicional seria interpretação além do complemento. [nao verificado — complemento fornecido]

**Leitura com efeito de observabilidade:** a tarefa chama `carga_da_vida`, que antecipa `_slot_esperado`; este chama `_ts_tipo_do_plantio`, que chama `_tipo_pelo_marco`. A divergência entre marco cobrado e ata chama `registrar_engolido('chamados.materializacao', 'tipo_marco_divergente', ...)`. (`app/chamados/services/tarefa_do_dia.py:534`; `app/chamados/services/carga_pergunta_viva.py:117`; `app/chamados/juizes.py:375`; `app/chamados/services/materializacao.py:656`; `app/chamados/services/materializacao.py:568`)

Nesse ponto, o efeito comprovado é incremento de contador em memória e `logging.error`, não escrita em `PerguntaDisputa` ou chamada ao escritor de `LogAuditoria`; o escritor deste último é outra função, `evento`. A pergunta 56712, ocorrências nas últimas 24 horas e devolução de erro em telas/app continuam [nao verificado]. (`app/core/observ.py:73`; `app/core/observ.py:94`; `app/core/observ.py:134`)

**Decisão antiga 3:** já há lei literal independente do complemento: desligado não retém em **nenhum dia da competência**; o retentor retorna vazio para desligado ou isento, e C19 retorna sem tarefa para isento. Portanto não cabe reapresentar genericamente essa proteção como lei ausente. A interação de eventual novo predicado deve preservar esses guardas. (`app/folha/export.py:1197`; `app/folha/export.py:1207`; `app/chamados/services/tarefa_do_dia.py:214`; `app/core/constants.py:13`)

## 17/11 11:46 · sessao 81ab028b7e9d · - · - · origem=Ronald (tmux)

- **Entram como leitores da evidência da retenção:** `fora_da_folha.dias_que_seguram`, `fora_da_folha`, `o_que_segura`, `retrato_folha.montar_linhas` e, por herança, tela/admin, barra, LED, drawer e PDF do retrato. Hoje o universo enriquecido é exclusivamente `motivo == 'furo_espelho'`; uma nova retenção por pergunta com célula limpa ficaria sem esse enriquecimento. (`app/folha/fora_da_folha.py:156`; `app/folha/fora_da_folha.py:165`; `app/folha/fora_da_folha.py:203`; `app/folha/retrato_folha.py:1214`; `app/folha/views_retrato.py:194`; `app/folha/views_retrato.py:200`)
- **Tarefa do dia fornece ação/alcance, mas seu resumo não certifica a pergunta:** a tarefa conserva `pergunta_id` e ações nas instâncias; o resumo pode obter dono por outras portas do dia ou pelo residual “decidir”, e pode publicar `resolver_dia` com `pergunta_id=None`. A composição precisa conservar o vínculo entre pergunta elegível e testemunho da ação, evitando usar só `fio` do dia como prova. (`app/chamados/services/tarefa_do_dia.py:651`; `app/chamados/services/tarefa_do_dia.py:665`; `app/chamados/services/tarefa_do_dia.py:686`; `app/chamados/services/tarefa_do_dia.py:690`; `app/chamados/services/tarefa_do_dia.py:707`)
- **RED que morde colaborador respondedor:** `porta_app` pode ter `acao=None` no topo; a ação `responder` está na instância. Portanto `porta.get('acao')` excluiria o respondedor, embora tenha ação alcançável. (`app/chamados/services/tarefa_do_dia.py:651`; `app/chamados/services/tarefa_do_dia.py:679`; `app/chamados/services/tarefa_do_dia.py:701`; `app/chamados/services/tarefa_do_dia.py:709`)
- **RED que morde desconhecimento:** `porta_da_tarefa` pode devolver envelope com `rota=None` porque `nao_sei=True`; `bool(porta)` não prova ação disponível. Também pode haver `nao_sei=True` com rota válida, portanto desconhecimento por si só não apaga uma ação publicada. (`app/chamados/services/tarefa_do_dia.py:719`; `app/chamados/services/tarefa_do_dia.py:721`; `app/chamados/services/tarefa_do_dia.py:722`)
- **RED que morde retrato:** acrescentar apenas motivo novo ao export produziria dono `admin`, tipo `sem_causa`, dias/datas desconhecidos e ações vazias. O retrato precisa gravar as evidências recebidas da composição: perguntas/dias que efetivamente retêm e suas ações publicadas, sem refazer C6 ou a tarefa. (`app/folha/retrato_folha.py:374`; `app/folha/retrato_folha.py:388`; `app/folha/retrato_folha.py:1232`; `app/folha/retrato_folha.py:1248`; `app/folha/retrato_folha.py:1249`)
- **Sem ação vai à estrutura no complemento; hoje ainda ocupa baldes da UI:** a cópia conserva `sem_acao_disponivel` e `sem_leitura_sistema`; o agrupamento misto conserva dias sem porta junto da pessoa com ação. Assim, a projeção futura precisa separar os incidentes estruturais do impedimento acionável, inclusive no misto. (`app/folha/retrato_folha.py:191`; `app/folha/retrato_folha.py:194`; `app/folha/fora_da_folha.py:97`; `app/folha/fora_da_folha.py:238`; destino novo: [nao verificado — complemento fornecido, item 2])
- **Tela continua testemunha do gravado:** lê `RetratoFolha`, usa `linhas_do_retrato` e conta o recorte pelas datas gravadas; o filtro de período altera dias/saldo, não a presença da pessoa na lista. Não cabe acrescentar nela consulta independente a disputa/pergunta. (`app/folha/views_retrato.py:194`; `app/folha/views_retrato.py:200`; `app/folha/views_retrato.py:225`; `app/folha/views_retrato.py:230`; `app/folha/retrato_folha.py:1327`)
- **Mais um consumidor indireto do universo:** `vigia_fio.censo_alcance` lê `dias_que_seguram` e deduplica por colaborador/dia; passa a acompanhar o universo novo se continuar medindo quem segura a folha. (`app/chamados/services/vigia_fio.py:510`; `app/chamados/services/vigia_fio.py:537`; `app/chamados/services/vigia_fio.py:541`; `app/chamados/services/vigia_fio.py:544`)
- **Prévia precisa acompanhar a consequência, sem virar autoridade:** lê classificação e perguntas datadas, mas `_proposta_cobre_retencao` só compara dias retidos pela célula; pode prometer liberação mesmo restando pergunta elegível se não consumir a resposta composta. (`app/folha/previa.py:43`; `app/folha/previa.py:64`; `app/folha/previa.py:169`; `app/folha/previa.py:173`)
- **Permanecem perguntas distintas:** lista de fechamento lê ENTRA e, separadamente, `travas_aprovacao`; nova retenção não autoriza deduzir novo veto de aprovação. O vigia de pausa continua perguntando pelo retentor da célula para medir L-033. (`app/ponto/services/fechamento_lista.py:73`; `app/ponto/services/fechamento_lista.py:100`; `app/ponto/services/fechamento_lista.py:102`; `app/folha/services/vigia_pausa.py:125`)

## 17/11 11:45 · sessao 3da05f32583e · PORTA-EXPORT · verificador · origem=Code

**Não aprovo ainda:** há caminhos que ocultam FALHOU, carimbam hoje uma medição iniciada ontem e publicam VERDE após lavrar um carimbo vermelho na mesma rodada. O último problema já existia na agregação anterior e permanece. (`app/folha/porta_export.py:1083`, `app/folha/porta_export.py:318`, `app/folha/porta_export.py:774`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:150`.)

Banco, logs, execução integrada e cron implantado: **[nao verificado]**.

1. **Leitores e escritores**

   | Linha/campo | Quem escreve | Quem lê |
   |---|---|---|
   | Carimbo `porta_export_leitores:AAAA-MM` | `lavrar`, usando `chave_do_carimbo`: `app/folha/porta_export.py:775`, `app/folha/porta_export.py:920`. Chamador produtivo encontrado: command em `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:135`. | `estado_da_porta`, pela mesma fábrica: `app/folha/porta_export.py:1078`. |
   | Carimbo `valor` | `lavrar`: falhas, ou `-1` quando não mediu: `app/folha/porta_export.py:777`. | Classificação da porta: `app/folha/porta_export.py:1085`; magnitude da mensagem: `app/folha/porta_export.py:863`. |
   | Carimbo `detalhe` | `lavrar` monta os campos: `app/folha/porta_export.py:779`. | Lista de divergências em `conferir`: `app/folha/porta_export.py:818`; motivo em `_campos_do_texto`: `app/folha/porta_export.py:1095`. |
   | Pedido `porta_export_pedido:AAAA-MM` | `pedir` cria; `assumir` atualiza/cria: `app/folha/porta_export.py:1009`, `app/folha/porta_export.py:1029`. Fábrica: `app/folha/porta_export.py:925`. | `_linha_do_pedido`: `app/folha/porta_export.py:977`; usado pelo retry e pela porta: `app/folha/porta_export.py:1014`, `app/folha/porta_export.py:1077`. |
   | Pedido `detalhe['estado']` | PEDIDO em `pedir`: `app/folha/porta_export.py:1005`; EM_CURSO em `assumir`: `app/folha/porta_export.py:1032`; demais transições por `_mudar_estado`: `app/folha/porta_export.py:988`. `encerrar`, `vigiar` e falha do disparo chegam por essa função: `app/folha/porta_export.py:1040`, `app/folha/porta_export.py:1054`, `app/folha/porta_export.py:1061`, `app/folha/porta_export.py:1141`. | CAS, retry, vigia e decisão da porta: `app/folha/porta_export.py:990`, `app/folha/porta_export.py:1017`, `app/folha/porta_export.py:1051`, `app/folha/porta_export.py:1080`. |
   | Pedido `pedido_em`, demais timestamps, `motivo`, `mes/ano/origem` | Criação, assunção e transições: `app/folha/porta_export.py:1005`, `app/folha/porta_export.py:1032`, `app/folha/porta_export.py:987`. | Vigia lê `pedido_em`: `app/folha/porta_export.py:1057`; apresentação lê `motivo`: `app/folha/porta_export.py:1097`. Não encontrei consumidor funcional dos demais metadados. |
   | Pedido `valor` | Sempre `None` nesses escritores: `app/folha/porta_export.py:1011`, `app/folha/porta_export.py:1031`. | A decisão usa seu estado, não seu valor: `app/folha/porta_export.py:1080`. |
   | `data_ref` | Dia consultado separadamente por `lavrar`, `pedir` e `assumir`: `app/folha/porta_export.py:774`, `app/folha/porta_export.py:1004`, `app/folha/porta_export.py:1028`. | Filtro do pedido e do carimbo: `app/folha/porta_export.py:978`, `app/folha/porta_export.py:1079`. |
   | `data_ini` | Mesmo dia da gravação: `app/folha/porta_export.py:778`, `app/folha/porta_export.py:1010`, `app/folha/porta_export.py:1031`. | Não participa da decisão da porta: `app/folha/porta_export.py:1077`. |
   | `calculado_em` | `auto_now` no modelo e atualização explícita nas transições: `app/inteligencia/models.py:96`, `app/folha/porta_export.py:991`. | Não participa da validade do carimbo ou do prazo do pedido: `app/folha/porta_export.py:1057`, `app/folha/porta_export.py:1078`. |

   Existe leitor genérico `serie(chave, ...)`, sem chamada identificada para essas chaves, e apresentação genérica pelo `__str__`: `app/inteligencia/metricas.py:62`, `app/inteligencia/models.py:113`. O escritor genérico exige chave registrada; não encontrei registro dessas chaves nele: `app/inteligencia/metricas.py:39`.

   **Não encontrei leitor ativo da chave antiga sem competência.** `CHAVE` permanece como prefixo da fábrica; escritor e leitor usam a fábrica. A chave cabe no campo existente e sua unicidade inclui escopo, empresa e dia. (`app/folha/porta_export.py:18`, `app/folha/porta_export.py:920`, `app/folha/porta_export.py:776`, `app/folha/porta_export.py:1078`, `app/inteligencia/models.py:87`, `app/inteligencia/models.py:103`.)

   Nos testes há escritor direto de fixture; o helper compartilhado delega a `lavrar`. Isso não constitui segundo escritor produtivo. (`app/folha/tests/test_porta_do_export.py:40`, `app/folha/porta_export.py:875`.)

2. **Liberação da porta**

   Com os valores produzidos por `medir/lavrar`, não encontrei liberação sem uma linha zero da chave, empresa e `data_ref` consultados. `conferir` libera somente PRONTA. (`app/folha/porta_export.py:764`, `app/folha/porta_export.py:1078`, `app/folha/porta_export.py:1085`, `app/folha/porta_export.py:813`.)

   **Mas FALHOU pode ser encoberto pelo verde anterior:** verde de hoje → nova execução do cron/manual → exceção → pedido FALHOU → PRONTA pelo carimbo anterior. FALHOU só determina FALHA quando não existe carimbo. Reproduzi esse ramo em memória: `conferir` devolveu `''`. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:132`, `app/folha/porta_export.py:1083`, `app/folha/porta_export.py:1090`, `app/folha/porta_export.py:813`.)

   **Há também fragilidade no “de hoje”:** `medir` congela seu instante inicial; `lavrar` consulta o dia novamente ao terminar. Uma medição iniciada antes da meia-noite pode ganhar carimbo de hoje usando teto temporal de ontem. (`app/folha/porta_export.py:318`, `app/folha/porta_export.py:338`, `app/folha/porta_export.py:774`.)

3. **Request e concorrência**

   O request chama `abrir_a_porta`, que pede e dispara o mesmo command por `Popen`; esse caminho não chama `medir/lavrar`. A recusa ocorre antes da montagem do TXT. O cálculo já existente para montar o arquivo continua depois da porta. (`app/folha/views.py:130`, `app/folha/views.py:135`, `app/folha/porta_export.py:1110`, `app/folha/porta_export.py:1138`, `app/folha/views.py:210`.)

   Para **a chamada principal por alvo**, dois processos usando a mesma chave e o mesmo banco não entram simultaneamente em `medir`: a trava antecede assunção, medição, lavra e encerramento; quem perde pula o alvo. Dois cliques também disputam a linha única do pedido. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:122`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:127`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:130`, `app/inteligencia/models.py:103`, `app/folha/porta_export.py:1008`.)

   **A exclusão não cobre toda submedição:** `medir(A)` chama `cartao_x_txt_divergentes`, que percorre todas as empresas ativas. Clique em A e cron medindo B podem medir simultaneamente as mesmas empresas nesse subcontador, apesar das travas distintas. (`app/folha/porta_export.py:667`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:51`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:63`.)

   A identidade efetiva do banco/schema entre `saas_ui` e `saas_core` é **[nao verificado]**. O código usa o schema da conexão, sem incorporar nome de container na chave. (`app/folha/porta_export.py:934`, `app/folha/porta_export.py:1128`.)

4. **Estados, vigia e colisões**

   Para processo morto, não encontrei prisão permanente demonstrável: EM_CURSO sem trava vira FALHOU; PEDIDO sem trava expira após 300 segundos, quando consultado. No retry, `pedido_em` é recomposto dinamicamente; não falta timestamp. (`app/folha/porta_export.py:900`, `app/folha/porta_export.py:1054`, `app/folha/porta_export.py:1060`, `app/folha/porta_export.py:1019`, `app/folha/porta_export.py:989`.)

   **Há corrida no vigia:** consulta a trava e depois altera a linha condicionado apenas ao texto do estado. Entre essas operações, outra execução pode assumir a mesma linha como EM_CURSO; o vigia antigo pode marcar FALHOU sobre a execução nova. Se o CAS perde, também não recarrega o objeto, podendo responder EM_CONFERENCIA com estado desatualizado. (`app/folha/porta_export.py:1051`, `app/folha/porta_export.py:1054`, `app/folha/porta_export.py:990`, `app/folha/porta_export.py:992`, `app/folha/porta_export.py:1029`.)

   A consulta de `pg_locks` está coerente com dois int4 válidos: banco corrente, `classid` convertido para unsigned, `objid=k2` positivo e `objsubid=2`. Comportamento contra PostgreSQL em execução: **[nao verificado]**. (`app/folha/porta_export.py:969`, `app/folha/porta_export.py:971`.)

   **A chave tem colisões e limite de faixa:**

   - Mesma empresa/mês em 2000 e 2100 colidem por `ano % 100`; ambos os anos passam pelo sanitizador da view. (`app/folha/porta_export.py:937`, `app/core/sanitiza.py:35`.)
   - CRC32 não garante separação de schemas. Exemplo calculado em memória: `tenant_8r7val5qgvx7` e `tenant_j01vp43zm44n` produzem `3328595997`. Existência desses schemas em produção: **[nao verificado]**. (`app/folha/porta_export.py:935`.)
   - Empresa `214749`, competência 11/2026, produz `k2=2147492611`, fora de int4, sem validação antes do SQL. Existência desse ID em produção: **[nao verificado]**. (`app/folha/porta_export.py:937`, `app/folha/porta_export.py:952`.)

5. **Classes históricas e mordida do selo**

   Não identifiquei juiz duplicado nem segundo escritor produtivo do carimbo. Competência corrente continua delegada a `janela_atual`; a medição continua consultando `classificar_export`, registrado para entrada no TXT. (`app/ponto/services/gestao_he.py:428`, `app/core/juizes.py:345`, `app/folha/porta_export.py:469`, `app/core/juizes.py:330`, `app/folha/porta_export.py:775`.)

   Estão presentes **relógio fora de uma borda comum**, **dia da medição confundido com dia da lavra** e **uso do verde anterior encobrindo falha posterior**. No rollout, o relógio ainda é consultado por empresa. (`app/folha/porta_export.py:318`, `app/folha/porta_export.py:774`, `app/folha/porta_export.py:1083`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:48`, `app/ponto/services/gestao_he.py:427`.)

   **O selo AST morde literais, mas não garante toda mensagem de template.** Reproduzi em memória: detecta `'docker exec ...'` e `'Rode: ...'`; deixa passar `'docker ' + 'exec ...'` e `'Ro' + 'de:'`. Ele examina constantes individuais, não o texto montado. (`app/folha/tests/test_porta_sem_beco.py:371`, `app/folha/tests/test_porta_sem_beco.py:372`.)

   A varredura cobre Python dos apps enumerados, incluindo porta e view, mas exclui HTML, JavaScript e commands. Há fontes reais de texto nesses locais excluídos. (`app/folha/tests/test_porta_sem_beco.py:384`, `app/folha/tests/test_porta_sem_beco.py:388`, `app/folha/tests/test_porta_sem_beco.py:390`, `app/templates/folha/exportar.html:12`, `app/static/pwa.js:357`.)

   Além disso, exceção do command pode virar `motivo`, ser interpolada e chegar às mensagens da tela; o selo não acompanha esse fluxo dinâmico. Vazamento real de texto proibido por esse caminho: **[nao verificado]**. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:132`, `app/folha/porta_export.py:1040`, `app/folha/porta_export.py:1097`, `app/folha/views.py:135`, `app/core/respostas.py:69`, `app/templates/folha/exportar.html:10`.)

6. **Recorte, soma e SystemExit**

   Para empresa positiva, o command carimba apenas os alvos filtrados; rollout seleciona ativas em rollout. **`--empresa 0`, porém, seleciona todas as ativas**, porque o filtro depende de truthiness; `--rollout --empresa 0` também escapa da rejeição de argumentos incompatíveis. A submedição global continua fora desse recorte. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:45`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:49`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:53`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:135`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:51`.)

   A soma entre competências distintas conta cada `(mes, ano)` uma vez. **Dentro da mesma competência, a última leitura sobrescreve a anterior.** Reproduzi em memória o `handle`: próprias zero, contador global 4 para A e depois 0 para B → carimbos 4 e 0 → saída final SELO VERDE. A identidade por empresa passa nos dois casos. Esse risco preexistente permanece. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:135`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:150`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:189`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:226`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:236`.)

   Os testes de soma fixam uma competência; o teste rollout contempla uma empresa. Não demonstram duas competências na mesma execução. (`app/relatorios/tests/test_selo_leitores_no_mesmo_numero.py:136`, `app/folha/tests/test_porta_sem_beco.py:224`.)

   `SystemExit` dentro de `medir` escapa de `except Exception`, mas o `finally` solta a trava e permite recuperação pelo vigia. As saídas explícitas posteriores à medição ocorrem depois de CONCLUIDO; portanto não prendem permanentemente a empresa já terminada. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:131`, `app/folha/porta_export.py:956`, `app/folha/porta_export.py:1054`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:136`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:139`.)

## 17/11 11:45 · sessao 4278c4537fa3 · - · - · origem=Ronald (tmux)

Item 2 tem premissa parcial e condições ainda abertas:

- A tarefa responde por **colaborador, dia do turno e instância de marco**; não por uma única pergunta. As instâncias incluem `pergunta_id`, mas a publicação agrega ações e portas do dia inteiro, inclusive `resolver_dia` sem pergunta associada. Falta dizer se basta ação de qualquer assunto daquele dia ou se a ação precisa resolver a pergunta candidata. (`app/chamados/services/tarefa_do_dia.py:733`; `app/chamados/services/tarefa_do_dia.py:284`; `app/chamados/services/tarefa_do_dia.py:637`; `app/chamados/services/tarefa_do_dia.py:640`; `app/chamados/services/tarefa_do_dia.py:684`)
- Não existem apenas os três verbos declarados no complemento: há `perguntar`, `responder`, `validar`, `materializar`, `rejulgar`, `decidir_ausencia`, `entregar_documento`; os donos incluem `sistema`, além de colaborador e admin. A publicação também acrescenta `resolver_dia` e conserva `decidir` residual. (`app/chamados/services/tarefa_do_dia.py:68`; `app/chamados/services/tarefa_do_dia.py:72`; `app/chamados/services/tarefa_do_dia.py:684`; `app/chamados/services/tarefa_do_dia.py:698`)
- Uma ação exigível na instância não garante porta humana alcançável: colaborador precisa constar dos pendentes no app; validação exige âncora utilizável; sistema tem regras próprias de alcance. (`app/chamados/services/tarefa_do_dia.py:651`; `app/chamados/services/tarefa_do_dia.py:654`; `app/chamados/services/tarefa_do_dia.py:660`)
- A tarefa seleciona perguntas `conversa=False` por **`data_evento` dentro da janela**, sem fallback para `celula.data`. Dentro do marco, usa a pergunta mais recente da identidade, após exclusões por via/retratação. Não enumera todas as perguntas C6 vivas como tarefas independentes. (`app/chamados/services/tarefa_do_dia.py:405`; `app/chamados/services/tarefa_do_dia.py:255`; `app/chamados/services/tarefa_do_dia.py:262`; `app/chamados/services/tarefa_do_dia.py:319`; `app/chamados/services/tarefa_do_dia.py:332`)
- Célula ausente produz `nao_sei=True`, razão `sem_celula`; ausência de lâmpadas e passivo sem linha também podem produzir desconhecimento. Isso não significa “sei que ninguém age”. `nao_sei` pode coexistir com ação e porta admin. (`app/chamados/services/tarefa_do_dia.py:230`; `app/chamados/services/tarefa_do_dia.py:240`; `app/chamados/services/tarefa_do_dia.py:154`; `app/chamados/services/tarefa_do_dia.py:274`; `app/chamados/services/tarefa_do_dia.py:336`; `app/chamados/services/tarefa_do_dia.py:719`)
- `porta_da_tarefa=None` significa ausência conhecida de porta; desconhecimento publica envelope com `rota=None`. O leitor precisa consultar `rota` e `nao_sei`, preservando a distinção. (`app/chamados/services/tarefa_do_dia.py:712`; `app/chamados/services/tarefa_do_dia.py:722`; `app/folha/fora_da_folha.py:86`)
- Há saídas sem tarefa por razões declaradas: fora do vínculo, isenção, competência trancada ou marco ainda não vencido. A ausência de tarefa, isoladamente, não demonstra defeito estrutural. A ocorrência concreta dessas combinações com pergunta C6 viva exige medição [nao verificado]. (`app/chamados/services/tarefa_do_dia.py:77`; `app/chamados/services/tarefa_do_dia.py:211`; `app/chamados/services/tarefa_do_dia.py:214`; `app/chamados/services/tarefa_do_dia.py:221`; `app/chamados/services/tarefa_do_dia.py:234`)
- RAIZ-DA-DISPUTA tem destino documental: fila ESTRUTURA e esqueleto indicado no BACKLOG; o corte BO-DISPUTA-DESENHO-RAIZ encaminha matriz/laudo à F1, sem construir. “Lista nossa” e o endereço concreto do novo brief não foram identificados [nao verificado]. (`app/docs/BACKLOG.md:2055`; `app/docs/CORTES.md:483`)
- L-007 trata de escritor canônico e rastreabilidade; a atribuição ESTRUTURA/CADASTRO/BATIDA está explicitamente em L-099. Portanto, a referência a L-007 não basta para provar que todo caso sem ação é ESTRUTURA. (`app/docs/LEIS.md:65`; `app/docs/LEIS.md:122`)

| Insumo/valor | Quem escreve/deriva | Quem lê |
|---|---|---|
| `DecisaoPergunta.pergunta`, `ato`, `ato_em`, `ato_texto`, `absorcao_ausencia` | Porta única `registrar`, grava novas linhas. Chamadores: resposta, validação/devolução e absorção. (`app/chamados/models.py:4110`; `app/chamados/models.py:4120`; `app/chamados/services/disputa_emissao.py:1525`; `app/chamados/services/validacao.py:173`; `app/chamados/services/validacao.py:279`; `app/chamados/services/materializacao.py:1045`) | Carga ordena por `ato_em,pk`, limita a `agora`; `_pela_decisao` lê a última linha. (`app/chamados/services/tarefa_do_dia.py:421`; `app/chamados/services/tarefa_do_dia.py:423`; `app/chamados/services/tarefa_do_dia.py:158`) |
| `Ausencia.status` | `mudar_estado`, escritor único; `decidir` é a porta do ato DP. (`app/ponto/services/ausencia.py:955`; `app/ponto/services/ausencia.py:976`; `app/ponto/services/ausencia.py:1057`) | Escolha entre documento do colaborador, decisão admin e encerramento pela aprovação. (`app/chamados/services/tarefa_do_dia.py:179`; `app/chamados/services/tarefa_do_dia.py:309`) |
| Célula `ata`, `veredito`, `protestos`; lâmpada derivada | `lavrar_veredito` atribui/persiste os campos; leitor da célula transporta `acesa` como `acesa_lavrada`. (`app/ponto/portas/celula.py:1032`; `app/ponto/portas/celula.py:1091`; `app/ponto/portas/celula.py:1098`; `app/escala/services/leitor_celula.py:114`; `app/escala/services/leitor_celula.py:137`) | Carga lê `grade_da_celula`; tarefa extrai lâmpadas e instâncias. (`app/chamados/services/tarefa_do_dia.py:544`; `app/chamados/services/tarefa_do_dia.py:108`; `app/chamados/services/tarefa_do_dia.py:125`) |
| Pergunta `veredito_lavrado`, `julgado_em`, `janela_expira_em` | `veredito_lavrado.lavrar`; grava após consultar juiz de materialização. (`app/chamados/veredito_lavrado.py:254`; `app/chamados/veredito_lavrado.py:270`; `app/chamados/veredito_lavrado.py:275`) | Carga aceita só sim/não ainda vigentes; decisão usa essa leitura. (`app/chamados/services/tarefa_do_dia.py:432`; `app/chamados/services/tarefa_do_dia.py:435`; `app/chamados/services/tarefa_do_dia.py:168`) |
| Chamado `status_local`, `silenciado_ate` | `_gravar_estado`; portas de arquivo atribuem e limpam silêncio. (`app/chamados/models.py:201`; `app/chamados/models.py:228`; `app/chamados/services/arquivo.py:166`; `app/chamados/services/arquivo.py:202`) | `qs_vivos`, `qs_na_fila`, partição de verbos. (`app/chamados/services/painel.py:155`; `app/chamados/services/painel.py:188`; `app/chamados/services/verbos.py:44`) |
| `verbo`, `fila_ids`, `fila_cel`, `fila_dia` | Projeções de `particionar_verbos` e `alimentar_fila`; não são novo estado persistido. (`app/chamados/services/verbos.py:47`; `app/chamados/juizes.py:1594`; `app/chamados/juizes.py:1597`; `app/chamados/juizes.py:1608`; `app/chamados/juizes.py:1613`) | Publicação de portas da tarefa. (`app/chamados/services/tarefa_do_dia.py:637`; `app/chamados/services/tarefa_do_dia.py:650`; `app/chamados/services/tarefa_do_dia.py:665`) |
| `pendentes_app` | `particao_do_app`: resposta sem instante e disputa aberta; carga cruza com C6 no mesmo relógio. (`app/chamados/services/pergunta_no_app.py:76`; `app/chamados/services/pergunta_no_app.py:81`; `app/chamados/services/tarefa_do_dia.py:529`; `app/chamados/services/tarefa_do_dia.py:539`) | Publicação da porta do colaborador. (`app/chamados/services/tarefa_do_dia.py:651`) |
| Resultado tarefa: `instancias`, `pergunta_id`, `acoes.{acao,de,porta}`, `nao_sei`, `razao` | `_tarefa_pura` e `_pela_decisao`, derivados; contrato declara zero escritores. (`app/chamados/services/tarefa_do_dia.py:135`; `app/chamados/services/tarefa_do_dia.py:190`; `app/chamados/services/tarefa_do_dia.py:284`; `app/chamados/services/tarefa_do_dia.py:733`) | `_portas_da_tarefa` agrega ações. (`app/chamados/services/tarefa_do_dia.py:637`) |
| Porta: `rota`, `fila`, `verbos`, `fio`, `acao`, `ausencia`, `nao_sei` | `_portas_da_tarefa` publica; `porta_da_tarefa` projeta. (`app/chamados/services/tarefa_do_dia.py:708`; `app/chamados/services/tarefa_do_dia.py:712`) | Folha, Vigia, furo sem canal e beco de cadastro. (`app/folha/fora_da_folha.py:70`; `app/folha/fora_da_folha.py:217`; `app/chamados/services/vigia_fio.py:357`; `app/chamados/services/furo_sem_canal.py:94`; `app/escala/services/cadastro_realidade.py:466`) |

O novo impedimento também precisa preservar o dono no retrato: hoje qualquer motivo diferente de `furo_espelho` recebe `admin`, e motivo sem mapeamento vira `sem_causa`. (`app/folha/retrato_folha.py:374`; `app/folha/retrato_folha.py:388`)

## 17/11 11:44 · sessao 85be606504fd · - · - · origem=Ronald (tmux)

Achados para os itens 1 e 2, somente pelo código:

- **C6 é `chamados.juizes.pergunta_viva`, não `q_vivas`, disputa aberta ou pergunta não validada isoladamente.** Registro em `app/core/juizes.py:291`; wrapper e memo em `app/chamados/juizes.py:866`; lei em `app/chamados/juizes.py:1403`. `q_vivas` é pré-filtro deliberadamente mais amplo; valor exato exige C6 ou `.viva`. (`app/chamados/models.py:3464`; `app/chamados/models.py:3470`; `app/chamados/models.py:3502`; `app/chamados/models.py:3512`)

- **Vida não exige dia nem célula.** Após os degraus anteriores, `materializada_em is None` devolve `True` mesmo com `data_evento=None` e `celula=None`. Ambos os campos aceitam NULL. Logo “pergunta viva sem dia” é admitida pelo código; não é contradição impossível. (`app/chamados/juizes.py:1482`; `app/chamados/models.py:2519`; `app/chamados/models.py:2528`)

- C6 mata validada, dia trancado e vias terminais; usa resposta com instante **e texto**, carimbo, veredito/lâmpadas da célula, falta e testemunho do fato. Resposta pode conservar vida para o admin, portanto viva não significa apenas “colaborador ainda deve responder”. (`app/chamados/juizes.py:1404`; `app/chamados/juizes.py:1409`; `app/chamados/juizes.py:1416`; `app/chamados/juizes.py:1423`; `app/chamados/juizes.py:1435`; `app/chamados/juizes.py:1454`; `app/chamados/juizes.py:1470`; `app/chamados/juizes.py:1489`)

- **“Dia da pergunta” exige discriminar fato/conversa.** `data_evento` é dia do turno para a porta `abrir`; `abrir_conversa` grava dia civil da conversa, que não tem turno nem competência de fato. O helper usado pelo C6/tranca retorna `None` para conversa e `data_evento` para fato, sem fallback à célula. (`app/chamados/models.py:2888`; `app/chamados/models.py:2900`; `app/chamados/models.py:2906`; `app/chamados/juizes.py:338`; `app/chamados/juizes.py:342`; `app/chamados/juizes.py:995`)

- **O resolvedor do chamado-pai é inadequado ao novo quantificador por pergunta.** Ele descarta conversa, prefere `data_evento`, usa `celula.data` como fallback e devolve o mínimo de todos os fatos. (`app/chamados/juizes.py:75`; `app/chamados/juizes.py:79`; `app/chamados/juizes.py:81`; `app/chamados/juizes.py:85`). Trocar unidade para pergunta fecha a ambiguidade de múltiplos fatos se medir cada pergunta individualmente; utilizar aquele mínimo reabre o erro.

- **Supervisão é a entidade `DisputaSupervisao`, relacionada à pergunta por FK; não apenas um rótulo de quem age.** O modelo reúne pessoa, motivos, contexto e chamado-pai; seu nascimento usual abre chamado de categoria/módulo `disputa_supervisao`, área `supervisao`. (`app/chamados/models.py:1774`; `app/chamados/models.py:1800`; `app/chamados/models.py:1813`; `app/chamados/models.py:2521`; `app/chamados/services/disputa_emissao.py:582`; `app/chamados/services/disputa_emissao.py:591`; `app/chamados/services/disputa_emissao.py:601`)

- **C6 não é filtro de `fechada_em`.** O corpo calcula a vida da pergunta sem exigir disputa aberta; `aberta` é outra pergunta, `fechada_em is None`. A existência possível de pergunta viva em disputa fechada está reconhecida e é reparada por reabertura no fluxo de devolução da validação. (`app/chamados/juizes.py:1403`; `app/chamados/models.py:1888`; `app/chamados/services/validacao.py:253`; `app/chamados/services/validacao.py:283`). O aval precisa conservar explicitamente o filtro anterior “disputa aberta” ou declarar sua retirada; o texto do complemento diz que o resto segue, segundo anexo fornecido [nao verificado].

- **C7 continua distinto:** disputa pode fechar se não há resposta sem veredito e não há fio vivo por C6. O juiz de retenção da folha não deve substituir C6, C7 ou tarefa. (`app/chamados/models.py:1928`; `app/chamados/models.py:1934`; `app/chamados/juizes.py:1510`; `app/chamados/juizes.py:1519`)

Escritores/leitores relevantes:

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `DisputaSupervisao.colaborador`, `motivos`, `contexto`, `chamado` | Emissão usual cria em `app/chamados/services/disputa_emissao.py:601`; órfãos `:1137`; âncora `:1372`; agregação de motivos/contexto `:550` e `:576`. | Identidade da pergunta lê pessoa da disputa em `app/chamados/models.py:3597`; C6 lê pessoa em `app/chamados/juizes.py:1474`; resolvedor do chamado lê perguntas em `app/chamados/juizes.py:75`. |
| `fechada_em` / aberta | `fechar` grava em `app/chamados/models.py:2013`; `reabrir_admin` zera `:2230`; `reabrir_sistema` zera `:2254`. | Property aberta `app/chamados/models.py:1888`; C6 não o usa em seu corpo `app/chamados/juizes.py:1403`. |
| `data_evento` | Atribuição única `gravar_dia_do_fato`, `app/chamados/services/fio.py:837` e `:847`; nascimento persiste `app/chamados/models.py:3126`; costura `app/chamados/services/fio.py:926` e `:974`; re-chaveamento `:1084` e `:1090`. O dia vem do juiz de turno `app/chamados/services/fio.py:1698`. | C6/tranca pelo helper `app/chamados/juizes.py:338`; resolvedor do pai `:81`; juiz do turno da linha `app/chamados/services/fio.py:1698`. |
| `conversa` | Portas `abrir`/`abrir_conversa`, `app/chamados/models.py:2894` e `:2906`; inserção `:3124`. | Helper do dia `app/chamados/juizes.py:342`; resolvedor do pai `:79`; juiz do turno `app/chamados/services/fio.py:1699`. |
| `celula` | Nascimento `app/chamados/models.py:3124`; re-chaveamento `app/chamados/services/fio.py:1085`; reuso pode reapontar a identidade conforme `app/chamados/models.py:3108`. | C6 `app/chamados/juizes.py:1418` e `:1454`; fallback do pai `:83`. |
| `colaborador`, `instancia_marco`, `batida_origem` da pergunta | Derivação no nascimento por `_chave_de_identidade`, `app/chamados/models.py:3592`, `:3599`, `:3607`, `:3616`; episódio vem do contexto de nascimento `:3620`. | Juiz do turno `app/chamados/services/fio.py:1701` e `:1706`; resolução do dono `:1723`. |
| `resposta_colab`, `respondida_em` | Resposta humana `app/chamados/services/disputa_emissao.py:1481`; automática `:1674`; reparo de instante pela trilha `:1696`; carimbo pode preencher instante com texto em `app/chamados/models.py:3240`; ressuscitação zera o par em `:1555`; lote por `:3362`. | C21 `app/chamados/juizes.py:1544` e `:1552`; C6 lê par projetado em `:1404` e `:1423`. |
| `validada_em` | Validação `app/chamados/services/validacao.py:161` e `:168`; devolução zera `:273`; resposta nova zera `app/chamados/services/disputa_emissao.py:1494`; carimbo `app/chamados/models.py:3245`; ressuscitação `:1554`. | C6 `app/chamados/juizes.py:1405`; projeção temporal `:1389`. |
| `materializada_em`, `via_resolucao` | Atribuição única `_escrever_carimbo`, `app/chamados/models.py:3207`; portas carimbar/identidade/descarimbar explicadas `:3183`; lote `:3362`. | C6 `app/chamados/juizes.py:1416`, `:1444`, `:1450`, `:1482`; projeção temporal `:1390`. |
| Nova resposta única da folha | Endereço, derivador e autoridade novos não determinados pelo anexo fornecido [nao verificado]. | Pretendidos export/PDF segundo aval fornecido [nao verificado]. |

O nascimento atual exige dia e verifica contrato de saída no mesmo atomic, mas isso não elimina a nulabilidade do acervo nem determina destino dos casos sem dia. (`app/chamados/models.py:2892`; `app/chamados/models.py:2915`; `app/chamados/models.py:2923`; `app/chamados/models.py:2528`)

Minha avaliação parcial: item 1 ainda PROCEDE por “dia da pergunta”/sem dia e composição L-033 pendente; o complemento fecha unidade e quais vivas, e evita o mínimo do pai apenas se a aplicação respeitar essa unidade. Item 2 precisa da leitura da tarefa pelo agente correspondente para concluir; “sem ação” não pode ser inferido apenas de C6. (`app/chamados/juizes.py:338`; `app/chamados/juizes.py:1482`; `app/core/juizes.py:304`; composição L-033 [nao verificado])

## 17/11 11:44 · sessao e8ef11ac8a65 · - · - · origem=Ronald (tmux)

Achados para consolidação, todos obtidos por leitura da cópia:

- **L-033 segue aberta:** a lei mantém pagável o furo somente de intervalo **mesmo com pergunta aberta**. Portanto, “C6 viva + dia na janela + alguém age” ainda não basta para o SIM: falta compor expressamente a isenção com a resposta única. A autoridade existente da isenção é `nao_retem_pela_pausa`, aplicada no retentor da célula. (`app/docs/LEIS.md:81`; `app/folha/export.py:1158`; `app/folha/export.py:1173`; `app/folha/export.py:1293`)

- **L-007 está citada com sentido ampliado no complemento:** o literal vigente é um escritor canônico por estado, tripwire e trilha de ação crítica. A separação entre dono ESTRUTURA, CADASTRO e BATIDA vem da L-099; só os dois últimos vão à lista do admin. Assim, atribuir uma falha comprovada do sistema à ESTRUTURA é compatível, mas L-007 sozinha não prova que todo “ninguém age” é falha estrutural. (`app/docs/LEIS.md:65`; `CLAUDE.md:32`; `CLAUDE.md:179`)

- **Juízes distintos:** C6 responde se uma pergunta está viva; C19 responde ações exigíveis por instância de marco, de quem e por quais portas; C7 responde fechamento da disputa; `respostas_sem_veredito` tem autoridade própria. O predicado novo combinado não está declarado por essas autoridades individualmente; não cabe apresentá-lo como mera declaração de juiz existente com predicado e universo iguais. (`app/core/juizes.py:112`; `app/core/juizes.py:153`; `app/core/juizes.py:291`; `app/core/juizes.py:304`; `app/core/juizes.py:308`; `CLAUDE.md:279`)

- **Destino RAIZ existe, mas destino operacional ainda não fecha:** RAIZ-DA-DISPUTA está registrada como obra ESTRUTURA, com esqueleto indicado; sua F1 já recebe o desenho da disputa sem pergunta. Isso sustenta o encaixe proposto, sem autorizar construção. O arquivo concreto de “lista nossa”, responsável por recebê-la e brief concreto que receberá estes novos casos não são especificados pelo complemento [nao verificado]. (`app/docs/BACKLOG.md:2055`; `app/docs/CORTES.md:481`; `app/docs/CORTES.md:483`)

- **Item 2 tem ambiguidade de composição com o aval restante:** “ninguém pode agir NÃO retém a pessoa” pode significar remover apenas o novo impedimento por pergunta ou também suspender a retenção atual pela célula. O restante do aval conserva a célula quando a resposta é NÃO; hoje ausência de célula e célula acusando podem reter independentemente de tarefa. É necessário escolher o alcance literal da liberação. (`app/folha/export.py:1270`; `app/folha/export.py:1273`; `app/folha/export.py:1293`; aval textual fornecido [nao verificado])

- **Encaminhamento da tela precisa entrar no pré-desenho:** motivo diferente de `furo_espelho` hoje recebe dono `admin`; motivo novo sem mapeamento recebe `sem_causa`. Portanto, acrescentar apenas um motivo próprio à classificação pode contrariar “nunca para a lista do admin”. (`app/folha/retrato_folha.py:374`; `app/folha/retrato_folha.py:388`)

- **K6 não resolve sozinho a pendência anterior de “competência aberta”:** entregue significa TXT vigente e não invalidado para empresa/mês/ano; K9 acrescenta holerite publicado para outra pergunta. O complemento não redefine “aberta” [nao verificado]. (`app/folha/export.py:556`; `app/folha/export.py:581`; `app/folha/export.py:591`; `app/folha/export.py:597`; `app/docs/CORTES.md:342`; `app/docs/CORTES.md:343`)

- **Efeito financeiro futuro:** o montador só produz linhas para classificados `entra`; o gerador também usa esse conjunto para aprovação automática, sujeita às travas. Alterar o corte muda os eventos enviados e potencialmente os aprovados. Isso é impacto previsto do corte, mas não autoriza mover o gravado exportado ou construir pelo aval de leitura. (`app/folha/export.py:714`; `app/folha/export.py:726`; `app/folha/views.py:215`; `app/folha/views.py:244`; `app/folha/views.py:246`; `CLAUDE.md:265`; `CLAUDE.md:411`)

- **Leitores atuais continuam diferentes:** PDF bloqueia qualquer disputa aberta da pessoa; helper consulta apenas colaborador e `fechada_em=NULL`. Export consulta retenção das células da janela. O lote PDF cai no informacional por qualquer exceção, portanto NÃO do corte não garante PDF auditável. (`app/chamados/services/disputa_emissao.py:40`; `app/relatorios/pdf_espelho.py:1085`; `app/folha/export.py:1301`; `app/relatorios/services.py:244`; `app/relatorios/services.py:250`)

- **Escritores/leitores de resultados e K6:** `classificar_export` deriva `status/motivo/detalhe`; montador e gerador leem esses resultados. K6 deriva ENTREGUE lendo a tripla e a vigência anotada; não grava. O endereço/escritor da nova resposta SIM/NÃO ainda não foi determinado pelo anexo [nao verificado]. (`app/folha/export.py:837`; `app/folha/export.py:968`; `app/folha/export.py:970`; `app/folha/export.py:714`; `app/folha/views.py:212`; `app/folha/export.py:580`; `app/folha/export.py:591`)

Minha indicação: **1 PROCEDE**, pela composição ainda ausente da L-033 e da definição/destino do dia irresolúvel; **2 PROCEDE**, caso a verificação exija literal completo, pela enumeração de ações a conferir com C19, composição com a retenção da célula e destino operacional não especificado. Os dois primeiros fundamentos específicos do item 2 dependem da leitura do agente de tarefa; não os considere fechados apenas por este relatório [nao verificado].

## 17/11 11:44 · sessao dfafddfaf2cf · CORTE-FOLHA-ESPELHO · leitura · origem=Code

**O corte responde à divergência principal, mas ainda não existe como uma autoridade compartilhada no código:** o export decide pela célula da competência; o PDF auditável consulta qualquer disputa aberta da pessoa, sem data. (`app/folha/export.py:968`, `app/folha/export.py:1273`; `app/relatorios/pdf_espelho.py:1085`; `app/chamados/services/disputa_emissao.py:40`)

Esta entrega é leitura e pré-desenho; comportamento posterior significa especificação do aval, sem construção nem RED executado. A correspondência da cópia com um HEAD Git do hash informado não pôde ser confirmada. [nao verificado]

**(i) Censo dos leitores**

Nas tabelas, **A** significa exatamente `DisputaSupervisao.fechada_em IS NULL`. A propriedade `.aberta` também significa somente isso; não consulta estado do chamado nem vitalidade das perguntas. (`app/chamados/models.py:1886`)

**Leitores da existência da disputa e suas projeções**

| Leitor | Predicado exato e recorte temporal | O que faz com a resposta |
|---|---|---|
| Helper público | `colaborador=colaborador AND A`, `.first()`, sem data. (`app/chamados/services/disputa_emissao.py:40`) | Retorna disputa ou `None`; é reexportado por `chamados.models`. (`app/chamados/models.py:4260`) |
| Helper do ponto | Delega ao helper público; colaborador ausente retorna `None`. Sem competência. (`app/ponto/services/disputa.py:18`) | Apenas retorna a disputa; o nome “motor bloqueado” não executa bloqueio. (`app/ponto/services/disputa.py:31`) |
| Espelho HTML | Helper quando há colaborador e não está em modo gestor. Sem recorte da competência navegada. (`app/ponto/views.py:142`) | Mostra banner e continua apresentando cálculo. (`app/ponto/views.py:181`; `app/templates/ponto/espelho.html:13`) |
| Espelho na API | Helper por colaborador, sem data. (`app/api/views.py:1336`) | Calcula e devolve `em_disputa`, ID e `aberta_em`. (`app/api/views.py:1333`, `app/api/views.py:1670`) |
| PDF auditável | Helper por colaborador, sem data, motivos ou perguntas. (`app/relatorios/pdf_espelho.py:1085`) | Havendo disputa, levanta `ValueError`; o bloqueio precede a persistência do registro auditável. (`app/relatorios/pdf_espelho.py:1087`, `app/relatorios/pdf_base.py:129`, `app/relatorios/pdf_base.py:152`) |
| Gate de cadastro/benefício | POST, colaborador identificado, não isento e helper verdadeiro. Sem data. (`app/colaboradores/mixins.py:300`) | Recusa e redireciona; alcança transferência, conta bancária, lançamento e cancelamento de benefício. (`app/colaboradores/mixins.py:308`; `app/colaboradores/views.py:1394`, `app/colaboradores/views.py:1641`, `app/colaboradores/views.py:1807`, `app/colaboradores/views.py:1915`) |
| Painel situacional em lote | `A AND colaborador_id IN IDs`. Sem data. (`app/colaboradores/services/situacional.py:117`) | Alimenta LED vermelho; o contador “em turno” usa outra autoridade. (`app/colaboradores/services/situacional.py:208`, `app/colaboradores/services/situacional.py:210`) |
| Painel por célula | `colaborador_id=c.pk AND A`, `.exists()`. Sem data. (`app/colaboradores/painel_cell.py:31`) | Acende o mesmo LED vermelho. (`app/colaboradores/painel_cell.py:68`) |
| Drawer operacional | Colaborador + A, ordenado por `-aberta_em`, `.first()`. Sem janela. (`app/colaboradores/services/drawer.py:71`) | Mostra motivos das perguntas vivas e link do chamado-pai. (`app/colaboradores/services/drawer.py:76`, `app/colaboradores/services/drawer.py:127`) |
| Relatório de pendências | Todas A, sem data. (`app/chamados/relatorios_disputa.py:45`) | Classifica sem pergunta, pronta para fechar, aguardando colaborador ou supervisão; tela e command leem esse coletor. (`app/chamados/relatorios_disputa.py:11`; `app/colaboradores/views.py:3041`; `app/chamados/management/commands/relatorio_pendencias_disputa.py:15`) |
| Contador do modal fio | Colaborador + A, `.count()`. Sem data, embora a contagem de mensagens vizinha tenha janela. (`app/chamados/views.py:903`, `app/chamados/views.py:909`) | Exibe “disputa(s) aberta(s)”. (`app/templates/chamados/partials/modal_fio.html:19`) |
| Detalhe do chamado | `chamado=chamado AND A`, `.first()`. Sem data. (`app/chamados/services/acoes_chamado.py:345`) | Carrega perguntas vivas e habilita apresentação/regularização. (`app/chamados/services/acoes_chamado.py:350`, `app/chamados/services/acoes_chamado.py:417`) |
| Regularizar pela tela | Chamado + A. Sem data. (`app/chamados/views.py:470`) | Recusa se não encontrar disputa; caso contrário delega a ação. (`app/chamados/views.py:473`) |
| Validar, recusar ou corrigir pergunta | PK da pergunta + disputa A; depois exige `chamado_que_lista(pergunta)` igual ao chamado. Sem janela. (`app/chamados/views.py:493`, `app/chamados/views.py:510`, `app/chamados/views.py:528`) | Autoriza o verbo ou devolve pergunta inválida. (`app/chamados/views.py:499`, `app/chamados/views.py:516`, `app/chamados/views.py:534`) |
| Validar inline | Disputa A pelo pai; fallback por colaborador e pergunta destinada ao chamado. Sem data. (`app/chamados/views_cobrar.py:160`) | Seleciona perguntas vivas respondidas para validação; ausência produz erro. (`app/chamados/views_cobrar.py:166`) |
| Correção solicitada pelo colaborador, web/API | PK da disputa + próprio colaborador + A. Sem data. (`app/chamados/views.py:1328`; `app/api/views.py:2677`) | Delega a `reabrir_por_colab`; disputa fechada/inexistente não passa. (`app/chamados/views.py:1334`; `app/api/views.py:2686`) |
| Lista de chamados na API | Todas A do colaborador. Sem data na consulta das disputas. (`app/api/views.py:1016`) | Associa perguntas aos destinos ou ao pai; ignora disputa fechada nesse caminho. (`app/api/views.py:1033`) |
| Partição de perguntas do app | Sobre perguntas vivas pelo C6: pendente se `respondida_em IS NULL AND disputa.fechada_em IS NULL`. A partição não acrescenta janela. (`app/chamados/services/pergunta_no_app.py:73`, `app/chamados/services/pergunta_no_app.py:81`) | Separa pendentes de histórico; portanto `em_disputa=bool(pendentes)` não equivale à existência de recipiente aberto. (`app/api/views.py:2567`) |
| Badges e dashboard | Herdam a partição anterior. (`app/chamados/services/pergunta_no_app.py:100`) | Contam pendentes na API principal, API core, painel do colaborador e dashboard. (`app/api/views.py:315`; `app/api/views_core.py:303`; `app/colaboradores/views.py:959`; `app/core/views.py:252`) |
| Aviso após texto livre | Mesmas pendentes do app. (`app/chamados/services/aviso_pendencia.py:7`) | Avisa que texto livre não respondeu ao questionário. (`app/chamados/views.py:1119`; `app/api/views.py:987`, `app/api/views.py:2211`) |
| Aparência e botões dos fios | `.aberta`, sem data. (`app/chamados/models.py:1888`) | Cores, ações da supervisão e apresentação de fechamento. (`app/templates/chamados/partials/modal_fio.html:76`, `app/templates/chamados/partials/modal_fio.html:353`, `app/templates/chamados/partials/modal_fio.html:406`; `app/templates/chamados/fio_colaborador.html:164`; `app/templates/chamados/partials/_card_atendimento.html:10`) |
| Censo de folha em `bin` | Perguntas do colaborador com `respondida_em IS NULL AND disputa A`. Sem data. (`bin/censo_folha.py:28`) | Acrescenta agravante `PERG_VIVAS` e pode escrever CSV quando executado. (`bin/censo_folha.py:37`, `bin/censo_folha.py:44`) |

Há texto de apresentação desatualizado: o detalhe ainda afirma que, enquanto aberta a disputa, “motor não calcula” e “fechamento mensal pula esse colab”; a execução do espelho continua calculando. (`app/templates/chamados/detalhe_local.html:304`; `app/ponto/views.py:181`)

**Cascas que herdam a pergunta do PDF**

| Leitor indireto | Uso da resposta |
|---|---|
| Serviço de emissão | Chama o gerador auditável e pode anexar o resultado ao chamado. (`app/relatorios/services.py:70`, `app/relatorios/services.py:75`) |
| Lote por competência | Tenta auditável; qualquer exceção leva à tentativa informacional. Se ambas falharem, pula a pessoa. (`app/relatorios/services.py:237`, `app/relatorios/services.py:244`, `app/relatorios/services.py:250`, `app/relatorios/services.py:255`) |
| Job do lote | Chama o lote e contabiliza informacionais. (`app/relatorios/lote_espelho.py:205`, `app/relatorios/lote_espelho.py:227`) |
| Anexar ao chamado | Recebe o `ValueError`, mostra erro e redireciona. Não faz fallback informacional. (`app/relatorios/views.py:1003`, `app/relatorios/views.py:1007`) |
| Avulso por competência | Recebe o `ValueError` e devolve formulário com erro. Não faz fallback informacional. (`app/relatorios/views.py:1127`, `app/relatorios/views.py:1131`) |
| Avulso por período livre | Gera sempre informacional; não consulta esse bloqueio. (`app/relatorios/views.py:1064`, `app/relatorios/views.py:1098`) |

**Leitores internos, eventos, comandos e crons**

Todos os filtros A abaixo são sobre `fechada_em`, não sobre `aberta_em`; quando há outro recorte de data, ele está indicado na própria linha. (`app/chamados/models.py:1820`, `app/chamados/models.py:1821`)

| Leitor | Predicado adicional a A / data | Consequência |
|---|---|---|
| Nascimento/agregação | Colaborador, `.first()`, sem data. (`app/chamados/services/disputa_emissao.py:540`) | Reutiliza a disputa, agrega motivos/contexto; só cria outra quando não encontra. (`app/chamados/services/disputa_emissao.py:550`, `app/chamados/services/disputa_emissao.py:576`, `app/chamados/services/disputa_emissao.py:601`) |
| Agrupamento de órfãos; âncora | Colaborador, `.first()`, sem data. (`app/chamados/services/disputa_emissao.py:1110`, `app/chamados/services/disputa_emissao.py:1346`) | Reutiliza ou cria recipiente para as perguntas. (`app/chamados/services/disputa_emissao.py:1137`, `app/chamados/services/disputa_emissao.py:1372`) |
| Universo de travas | Disputas explicitamente indicadas **ou** A dos colaboradores quando solicitado. Sem data. (`app/chamados/models.py:2778`) | Trava disputas e inclui seus pais na ordem de travamento. (`app/chamados/models.py:2780`, `app/chamados/models.py:2784`) |
| Fecho do universo do repasse | IDs explícitos, pais indicados **ou** colaborador + A. Sem data. (`app/chamados/services/disputa_emissao.py:991`) | Expande o conjunto que precisa ser travado antes do repasse. (`app/chamados/services/disputa_emissao.py:998`, `app/chamados/services/disputa_emissao.py:1007`) |
| `_disputa_do_chamado` | Primeiro pai + A; fallback pergunta com destino no chamado + disputa A. Sem data. (`app/chamados/services/fio.py:131`, `app/chamados/services/fio.py:135`) | Encontra a disputa para os verbos do fio. (`app/chamados/services/fio.py:137`) |
| Montagem dos fios | Todas as disputas do colaborador com pai; `.aberta` prioriza abertas por pai. Sem janela nessa seleção. (`app/chamados/services/fio.py:213`, `app/chamados/services/fio.py:223`) | Fio do colaborador alimenta-se das abertas; admin também admite perguntas vivas em fechadas. (`app/chamados/services/fio.py:248`, `app/chamados/services/fio.py:253`) |
| Verbos do fio | PK da pergunta + disputa A. Sem data. (`app/chamados/services/fio.py:679`) | Autoriza recusar/corrigir após conferir o chamado que a lista. (`app/chamados/services/fio.py:681`) |
| Trava de encerramento do pai | Pai + A e, depois, nenhuma pergunta **ou** `fio_vivo(perguntas)`. Sem janela. (`app/chamados/juizes.py:2466`, `app/chamados/juizes.py:2470`) | Retorna disputa que impede encerrar o chamado. (`app/chamados/models.py:153`) |
| Resolver perguntas do chamado | Destino no chamado + `materializada_em IS NULL` + disputa A. Sem janela. (`app/chamados/services/disputa_emissao.py:1928`) | Resolve elegíveis; também varre A da pessoa sem credor vivo e tenta fechar por C7, respeitando pergunta em voo. (`app/chamados/services/disputa_emissao.py:2049`, `app/chamados/services/disputa_emissao.py:2111`) |
| Reconciliador: sem turno; absolvição pela célula | Disputa ainda A e C7 permite. Sem janela acrescentada pelo teste de estado. (`app/chamados/reconciliador.py:412`, `app/chamados/reconciliador.py:600`, `app/chamados/reconciliador.py:612`) | Fecha depois dos carimbos/absolvições. (`app/chamados/reconciliador.py:413`, `app/chamados/reconciliador.py:613`) |
| Órfãs abertas | Não materializada, não validada, disputa A, motivo `orfao_14h` ou `saida_sem_entrada`, dia não nulo; IDs opcionais de colaboradores. Sem janela da competência. (`app/chamados/reconciliador.py:1041`, `app/chamados/reconciliador.py:1049`) | Alimenta encerramento no evento e vigia; ao final só tenta fechar disputa ainda aberta. (`app/chamados/reconciliador.py:1148`, `app/chamados/reconciliador.py:1162`, `app/chamados/reconciliador.py:1184`) |
| Órfãs reatáveis | Sem destino, colaborador, disputa A, não materializada, motivo em `MOTIVOS_DO_DIA_SEM_FECHAR`. Sem janela. (`app/chamados/reconciliador.py:1483`, `app/chamados/reconciliador.py:1501`) | Tenta reatar a destino inequívoco ou reconhecer resolução histórica. (`app/chamados/reconciliador.py:1510`) |
| Materialização | Disputa ainda A e C7 permite. Sem janela nesse teste. (`app/chamados/services/materializacao.py:1067`, `app/chamados/services/materializacao.py:1989`) | Fecha após ausência declarada ou quitação das perguntas. (`app/chamados/services/materializacao.py:1068`, `app/chamados/services/materializacao.py:2006`) |
| Entrada sincronizada | Pai manual vivo que agrupa somente aquele órfão; sua disputa A e C7 permite. Sem janela nesse teste. (`app/ponto/services/entrada_sincronizada.py:48`, `app/ponto/services/entrada_sincronizada.py:55`, `app/ponto/services/entrada_sincronizada.py:57`) | Fecha a disputa regularizada pela entrada. (`app/ponto/services/entrada_sincronizada.py:60`) |
| Beco, evento e command | A, pai resolvido, nenhuma pergunta sem resposta com destino não resolvido; C7 separa fechável/barrada. Sem janela. (`app/chamados/services/becos.py:25`) | Evento tenta fechar; command mede e, com `--apply`, fecha. `--alarme` só mede/alarma. (`app/chamados/signals.py:238`; `app/chamados/management/commands/fechar_becos_disputa.py:32`, `app/chamados/management/commands/fechar_becos_disputa.py:57`, `app/chamados/management/commands/fechar_becos_disputa.py:67`) |
| Competência trancada | Perguntas da empresa com `ini <= data_evento <= fim`; depois disputas A desse conjunto, C7 e condição do pai. (`app/chamados/services/competencia_trancada.py:119`, `app/chamados/services/competencia_trancada.py:175`) | Mede elegibilidade e fecha pela porta da competência trancada. Não é juiz de retenção da folha. (`app/chamados/services/competencia_trancada.py:221`) |
| Carimbo desfeito / fábrica / vigia | Carimbo presente, via fora de `VIAS_DE_IDENTIDADE`; quando `so_disputa_aberta=True`, exige A. Sem janela nesse Q. (`app/chamados/services/relavra_via.py:171`) | Porta relê elegibilidade; fábrica anota candidatos; vigia conta carimbadas ainda vivas e separa abertas/exportadas pelo dia. (`app/chamados/services/relavra_via.py:238`; `app/chamados/services/fabrica_da_celula.py:150`; `app/chamados/services/vigia_fio.py:769`, `app/chamados/services/vigia_fio.py:779`) |
| Pauta de calados | `q_vivas()` + disputa A; depois `espera_o_texto_do_colaborador` e C6. Sem janela própria. (`app/chamados/services/cobranca.py:237`, `app/chamados/services/cobranca.py:241`) | Conta pessoas vencidas e sem canal. (`app/chamados/services/cobranca.py:246`) |
| Alerta diário | Todas A, sem data do fato. Idade usa `aberta_em`; log para idade maior que sete dias. (`app/ponto/management/commands/alertar_disputas_pendentes.py:33`, `app/ponto/management/commands/alertar_disputas_pendentes.py:73`) | Push individual diário, agregado por empresa e log de disputa velha. (`app/ponto/management/commands/alertar_disputas_pendentes.py:48`, `app/ponto/management/commands/alertar_disputas_pendentes.py:103`) |
| Escalonamento | Chamado de supervisão no módulo de disputa, status em `COBRANCA`, não silenciado; exige disputa A pelo pai. Sem data do fato. (`app/ponto/management/commands/escalonar_chamados_supervisao.py:42`, `app/ponto/management/commands/escalonar_chamados_supervisao.py:57`) | Sem disputa, suspende escalada e registra órfão; após quatro horas pode anotar incidente na A. (`app/ponto/management/commands/escalonar_chamados_supervisao.py:60`, `app/ponto/management/commands/escalonar_chamados_supervisao.py:148`) |
| Auditoria de disputas | Perguntas não materializadas em A para cegas/sem destino; todas A para contexto/integridade; A com `aberta_em < agora−7 dias` para antigas. Sem janela factual. (`app/chamados/management/commands/auditar_disputas.py:69`, `app/chamados/management/commands/auditar_disputas.py:87`, `app/chamados/management/commands/auditar_disputas.py:135`, `app/chamados/management/commands/auditar_disputas.py:148`, `app/chamados/management/commands/auditar_disputas.py:166`) | Lista/conta ausência de contexto, perguntas cegas, falta de destino, recipiente vazio e dessincronização. (`app/chamados/management/commands/auditar_disputas.py:197`) |
| Auditoria de chamados órfãos | Pai + A, `.exists()`, sem data. (`app/chamados/management/commands/auditar_chamados_orfaos.py:40`) | Lista chamados sem disputa aberta vinculada. (`app/chamados/management/commands/auditar_chamados_orfaos.py:43`) |
| Auditoria de invariantes | I2 separa pergunta respondida em A; I7 exige A sem pai ou pai cancelado. Sem janela nesses predicados. (`app/chamados/management/commands/auditar_invariantes_chamados.py:592`, `app/chamados/management/commands/auditar_invariantes_chamados.py:805`) | Classifica piso/violação e integridade estrutural. (`app/chamados/management/commands/auditar_invariantes_chamados.py:594`) |
| Reconciliar fantasmas | Pergunta com destino no chamado, não materializada, disputa A. Sem janela nesse lookup. (`app/ponto/management/commands/reconciliar_fantasmas.py:210`) | Expõe vínculo pergunta/disputa no diagnóstico. (`app/ponto/management/commands/reconciliar_fantasmas.py:217`) |
| Encerrar perguntas com fato | IDs de disputas atingidas + A; depois ausência de fio vivo. Sem nova janela nesse teste. (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:81`) | Tenta fechar pela porta e contabiliza recusas. (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:83`) |
| Reativação de posto | A + posto + motivo `posto_desativado`, sem data. (`app/colaboradores/signals.py:71`) | Remove o motivo pela porta. (`app/colaboradores/signals.py:76`) |
| Preenchimento de âncora | A + colaborador + motivo `ancora_ausente`, sem data. (`app/escala/signals.py:168`) | Remove o motivo pela porta. (`app/escala/signals.py:173`) |
| Recebimento de foto | A + colaborador + `foto_ausente`; pergunta do episódio `batida_origem=batida`. Sem janela. (`app/api/foto_recebida.py:65`, `app/api/foto_recebida.py:71`) | Valida, remove motivo e tenta fechar por C7. (`app/api/foto_recebida.py:75`) |
| Desligamento | Todas A do colaborador, sem data. (`app/colaboradores/services/desligamento.py:164`) | Fecha pela porta de desligamento. (`app/colaboradores/services/desligamento.py:167`) |
| Regularização externa | Colaborador + A, `.first()`, sem data. (`app/chamados/services/regularizacao_ext.py:59`) | Impede materialização externa enquanto houver disputa. (`app/chamados/services/regularizacao_ext.py:62`) |

As próprias portas também leem o estado: `fechar` recusa/idempotentemente ignora fechada; remover motivo relê a disputa; reabrir admin/sistema exige fechada; correção pelo colaborador exige aberta; limpeza de resolução residual exige aberta com texto. Nenhuma dessas guardas contém janela da folha. (`app/chamados/models.py:1960`, `app/chamados/models.py:2112`, `app/chamados/models.py:2219`, `app/chamados/models.py:2248`, `app/chamados/models.py:2305`, `app/chamados/models.py:2343`)

Os sinais de fechamento fazem o teste inverso, `fechada_em` preenchido: reconciliam o pai, retomam regularizações externas e materializam pendências aprovadas. (`app/chamados/signals.py:29`, `app/chamados/signals.py:131`, `app/chamados/signals.py:155`) Ressuscitar perguntas e reparar órfãs enterradas podem reabrir o recipiente fechado. (`app/chamados/models.py:1638`; `app/chamados/management/com

[truncado em 20000 de 49663 caracteres]

## 17/11 11:42 · sessao a6022346bf74 · - · - · origem=Ronald (tmux)

Achados estáticos, sem banco nem testes executados [nao verificado]: [nao verificado]

- **FALHOU pode desaparecer da resposta:** se já existir carimbo zero do mesmo dia, uma nova execução que falhe grava FALHOU, mas `estado_da_porta` só considera esse estado quando não existe carimbo; acaba em PRONTA e `conferir` devolve `''`. Cenário: VERDE anterior → cron/manual remede → exceção. `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:132`; `app/folha/porta_export.py:1083`; `app/folha/porta_export.py:1090`; `app/folha/porta_export.py:814`.

- **Relógio fora da borda e carimbo com dia diferente da medição:** `medir` congela o instante e deriva seu dia; `lavrar` lê outro relógio ao terminar. Uma medição iniciada ontem pode produzir carimbo de hoje com teto temporal de ontem. Pedido, carimbo e leitores também leem o dia separadamente. `app/folha/porta_export.py:318`; `app/folha/porta_export.py:338`; `app/folha/porta_export.py:774`; `app/folha/porta_export.py:978`; `app/folha/porta_export.py:1028`; `app/folha/porta_export.py:1079`.

- **Exclusão do job por alvo está presente:** a trava precede `assumir`, `medir`, `lavrar` e `encerrar`; quem perde a trava pula a medição. Assim, para mesma chave e mesma base, dois processos não entram simultaneamente nesse trecho. A identidade efetiva das bases usadas por `saas_ui` e `saas_core` é [nao verificado]. `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:122`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:127`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:130`.

- **Essa exclusão não cobre toda submedição:** `medir(A)` executa `cartao_x_txt_divergentes` sem filtro por empresa; o contador varre todas as empresas ativas. Jobs com travas distintas para A e B podem medir simultaneamente B nesse contador. `app/folha/porta_export.py:667`; `app/relatorios/management/commands/cartao_x_txt_divergentes.py:51`; `app/relatorios/management/commands/cartao_x_txt_divergentes.py:63`; `app/relatorios/management/commands/cartao_x_txt_divergentes.py:68`.

- **`pg_locks` é coerente para os dois int4 válidos:** consulta o banco corrente, `classid` convertido para uint32, `objid=k2` positivo e `objsubid=2`. Não foi validado contra PostgreSQL em execução [nao verificado]. `app/folha/porta_export.py:969`; `app/folha/porta_export.py:971`.

- **Há colisões de chave:** `ano % 100` faz 2026 e 2126 coincidirem para mesma empresa/mês; CRC32 do schema também não garante unicidade. Dentro de meses 1–12 e anos residuais 00–99, a fórmula separa empresas, mas `pk=214749` já produz `k2` fora de int4 em 11/2026; não há validação antes da chamada SQL. `app/folha/porta_export.py:935`; `app/folha/porta_export.py:937`; `app/folha/porta_export.py:952`. Existência desses valores em produção: [nao verificado].

- **Processo morto não demonstrou prisão permanente:** EM_CURSO sem trava vira FALHOU na próxima consulta; PEDIDO sem trava expira após 300 segundos. `SystemExit` dentro de `medir` escapa do `except Exception`, mas o `finally` solta a trava e permite essa recuperação. `app/folha/porta_export.py:900`; `app/folha/porta_export.py:1051`; `app/folha/porta_export.py:1054`; `app/folha/porta_export.py:1060`; `app/folha/porta_export.py:956`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:131`.

- **Corrida do vigia usa estado sem geração:** ele consulta a trava e depois atualiza condicionado apenas ao texto do estado. Entre essas operações, outra execução pode assumir a linha; o vigia pode marcar FALHOU sobre o novo EM_CURSO. Uma atualização condicionada que perde também não recarrega o objeto, podendo responder EM_CONFERENCIA uma vez após conclusão. `app/folha/porta_export.py:990`; `app/folha/porta_export.py:992`; `app/folha/porta_export.py:1029`; `app/folha/porta_export.py:1051`; `app/folha/porta_export.py:1054`.

- **Soma por várias competências:** o dicionário novo conta uma vez cada `(mes, ano)` e soma as competências, mas conserva somente a última leitura de cada competência. Se o contador global retornar 1 para A e depois 0 para B da mesma competência, com falhas próprias zero, o command pode imprimir VERDE embora tenha lavrado VERMELHO para A. Essa suposição de estabilidade já existia; não há comparação entre leituras repetidas. `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:135`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:149`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:226`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:236`.

- **Saídas explícitas posteriores à medição não prendem a empresa já terminada:** `encerrar(CONCLUIDO)` acontece antes das validações e respectivos `SystemExit`; empresas ainda não assumidas podem conservar PEDIDO até o vigia expirar. `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:136`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:139`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:193`; `app/folha/porta_export.py:1060`.

## 17/11 11:42 · sessao 4f316b8d54cb · - · - · origem=Ronald (tmux)

Achados estáticos, sem executar testes nem acessar banco/log: [nao verificado]

- **Recorte `--empresa 0` abre todas as empresas ativas.** O parser aceita inteiro; `_alvos` aplica o filtro apenas se truthy. `--rollout --empresa 0` também passa pela incompatibilidade sem rejeição. `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:35`, `:45`, `:52`, `:53`, `:55`.

- **Possível VERDE final após carimbo vermelho na mesma rodada.** Para o mesmo `(mes, ano)`, `da_comp[k][(mes,ano)] = v` sobrescreve a medição anterior. Resultados válidos `falhas_da_competencia=4` e depois `0`, com próprias zero, deixam primeiro carimbo vermelho, mas total final zero e saída VERDE. O contador é repetido com novo instante por empresa; inexiste snapshot comum ou teste de igualdade entre as respostas. É risco já existente na sobrescrita anterior, preservado pela nova estrutura. `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:135`, `:150`, `:188`, `:226`, `:236`; `app/folha/porta_export.py:318`, `:667`.

- **Soma entre competências distintas está implementada:** uma entrada por `(mes,ano)`, soma dos valores únicos e soma das falhas próprias. Isso funciona sob estabilidade do número repetido por competência. Os testes de soma fixam 10/2026 e o teste rollout contempla uma empresa/competência; não provam duas competências na mesma execução. `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:149`, `:151`, `:153`, `:226`; `app/relatorios/tests/test_selo_leitores_no_mesmo_numero.py:136`; `app/folha/tests/test_porta_sem_beco.py:224`.

- **Rollout usa juiz existente, mas relê relógio por empresa.** Não captura `hoje` na borda do comando: chama `competencia_de_hoje(e)` repetidamente, cujo default consulta `timezone.localdate()`. Uma rodada que atravessa a virada pode selecionar competências por dias distintos. A autoridade continua sendo `janela_atual`. `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:48`; `app/ponto/services/gestao_he.py:427`, `:428`.

- **Request dispara sem chamar `medir/lavrar`.** View chama `abrir_a_porta` antes dos fechamentos, retorna quando há recusa; `abrir_a_porta` chama `pedir/disparar`, e `disparar` faz `Popen` do mesmo command com schema, empresa e competência. `app/folha/views.py:130`, `:131`, `:135`, `:150`; `app/folha/porta_export.py:1108`, `:1110`, `:1112`, `:1129`, `:1138`.

- **Selo AST morde literais, não garante toda mensagem de tela.** Ele inspeciona `ast.Constant`, exclui docstrings e procura exatamente as duas substrings; há testes positivos para ambas. Porém só varre `.py` de apps listados. Não cobre fontes reais de texto da tela em HTML/JS, como banner/toast do PWA ou texto do template de exportação; tampouco detecta montagem dinâmica como `'docker ' + 'exec'`. `app/folha/tests/test_porta_sem_beco.py:354`, `:367`, `:371`, `:372`, `:384`, `:390`, `:413`; `app/static/pwa.js:357`, `:369`; `app/templates/folha/exportar.html:12`.

- **Motivo dinâmico vindo do command pode chegar à tela fora da inspeção do literal de origem.** Exceção vira `motivo`, guardado no pedido, interpolado e devolvido pela view via mensagens/HTMX. Commands ficam excluídos do selo. Vazamento real de “docker exec” nesse caminho: **[nao verificado]**. `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:132`; `app/folha/porta_export.py:1040`, `:1096`, `:1097`, `:1099`; `app/folha/views.py:135`; `app/core/respostas.py:65`, `:69`; `app/templates/folha/exportar.html:10`; `app/folha/tests/test_porta_sem_beco.py:388`.

- **SystemExit deliberados posteriores à medição ocorrem após CONCLUIDO.** `cartao_x_txt is None`, forma desconhecida e identidade inválida são julgados após lavrar/encerrar. `SystemExit` levantado dentro de `medir` não entra em `except Exception`; ficará para o vigia após a trava soltar. `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:129`, `:131`, `:135`, `:136`, `:137`, `:179`, `:189`.

## 17/11 11:41 · sessao 33c0ff49b49b · - · - · origem=Ronald (tmux)

Mapa de leitores/escritores na cópia:

| Campo/linha | Quem escreve | Quem lê |
|---|---|---|
| `porta_export_leitores:AAAA-MM`, `valor`, `data_ref`, `detalhe` | `lavrar`: chave e dia em `app/folha/porta_export.py:775`, valor em `:777`, detalhe em `:779`; chamado pelo command em `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:135` | `estado_da_porta`: chave/empresa/dia em `app/folha/porta_export.py:1078`, valor em `:1085`; `conferir` lê detalhe em `:818` e valor em `:863`; `_campos_do_texto` lê detalhe em `:1095` |
| `porta_export_pedido:AAAA-MM`, `data_ref`, `valor=None` | `pedir` cria em `app/folha/porta_export.py:1009`; `assumir` faz update/create em `:1029` | `_linha_do_pedido` filtra chave/empresa/dia em `app/folha/porta_export.py:977`; chamada por `pedir` em `:1014` e `estado_da_porta` em `:1077`; nenhum leitor funcional do `valor` do pedido identificado |
| `detalhe['estado']` do pedido | `pedir` cria PEDIDO em `app/folha/porta_export.py:1005`; `assumir` grava EM_CURSO em `:1032`; `_mudar_estado` escreve em `:988`, chamado por retry em `:1019`, `encerrar` em `:1040`, `vigiar` em `:1054`/`:1061`; `disparar` chama `encerrar` na falha em `:1141` | CAS de `_mudar_estado` em `app/folha/porta_export.py:990`; retry de `pedir` em `:1017`; `vigiar` em `:1051`; `estado_da_porta` em `:1080` |
| `calculado_em` das duas linhas | `auto_now=True` em `app/inteligencia/models.py:96`, portanto create/save dos escritores; update SQL explícito em `app/folha/porta_export.py:991` | Nenhum leitor funcional específico dessas chaves identificado; a porta não usa `calculado_em`, decide por `data_ref` e `valor` em `app/folha/porta_export.py:1078`/`:1085` |

A chave é formada uma única vez por `chave_do_carimbo`, usada no escritor e no leitor: `app/folha/porta_export.py:920`, `:776`, `:1078`. A chave do pedido também tem fábrica única: `app/folha/porta_export.py:925`, `:977`, `:1009`, `:1030`. O campo comporta a chave sem migration (`CharField(60)`) e há unicidade por chave/escopo/empresa/dia: `app/inteligencia/models.py:87`, `:103`.

No censo textual inteiro, não identifiquei leitor ativo de `chave=CHAVE` antiga para esta porta; a constante permanece somente como prefixo em `app/folha/porta_export.py:18`/`:920`. As menções antigas encontradas são históricas em `app/docs/PENDENTES_RONALD.json:2342` e `app/docs/RELATO-ARQUIVO.md:42296`. Há leitor genérico `serie(chave,...)`, sem chamada identificada para as chaves desta porta: `app/inteligencia/metricas.py:62`. O `__str__` genérico lê chave, dia e valor: `app/inteligencia/models.py:113`.

Não identifiquei segundo escritor produtivo dessas chaves; a fixture escreve diretamente o carimbo em `app/folha/tests/test_porta_do_export.py:40`. O helper de teste delega ao escritor `lavrar` em `app/folha/porta_export.py:875`; outros testes leem diretamente a chave e estado em `app/folha/tests/test_porta_sem_beco.py:98`, `:109`, `:133`, `:177`, `:214`.

Achado: **FALHOU fica oculto por um carimbo anterior do mesmo dia.** Depois de uma medição verde, uma nova execução que falhe grava FALHOU (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:132`), mas `estado_da_porta` só considera FALHOU quando não existe carimbo (`app/folha/porta_export.py:1083`); com o verde anterior devolve PRONTA (`:1089`) e `conferir` libera (`:813`). A linha FALHOU persiste, mas deixa de governar a porta.

Achado secundário, preexistente: `lavrar` não inclui `erro_cartao_x_txt` no detalhe (`app/folha/porta_export.py:779` até `:799`), enquanto `_campos_do_texto` busca esse campo (`:1097`). A falha negativa lavrada pode chegar à tela como “motivo nao registrado” (`:1099`).

Não há bug de timestamp ausente no retry: embora `pedir` zere o detalhe em `app/folha/porta_export.py:1018`, `_mudar_estado(..., PEDIDO)` recompõe `pedido_em` dinamicamente em `:989`.

A decisão de job/carimbo não aparenta duplicar as perguntas registradas de fechamento: elas tratam aprovação, entrada no TXT, prontidão, tranca, competência entregue e competência corrente (`app/core/juizes.py:181`, `:329`); `estado_da_porta` decide presença/resultado do carimbo e execução do job (`app/folha/porta_export.py:1077`). A medição conserva a chamada ao juiz registrado `classificar_export` em `app/folha/porta_export.py:469`, registrado em `app/core/juizes.py:330`.

## 17/11 11:39 · sessao 6423bcf443b1 · CORTE-FOLHA-ESPELHO · verificador · origem=Code

**AVAL-DEVOLVIDO nos itens 1 e 3:** faltam fechar, respectivamente, o universo dos fatos da disputa e o significado de competência aberta. O item 2 confere como efeito proposto para o pré-desenho. (`app/chamados/juizes.py:75`; `app/chamados/models.py:2528`; `app/ponto/services/fechamento.py:1523`; anexo textual sem arquivo:linha [nao verificado])

**Item 1 — CORTE**

1. **As premissas conferem? Parcialmente.** Hoje são perguntas diferentes: o export consulta células da janela e suas exceções; o PDF consulta qualquer disputa da pessoa com `fechada_em=NULL`, sem competência nem data do fato. O wrapper do export ignora os três primeiros argumentos e delega à retenção por célula. (`app/folha/export.py:1247`; `app/folha/export.py:1273`; `app/folha/export.py:1301`; `app/chamados/services/disputa_emissao.py:40`; `app/relatorios/pdf_espelho.py:1085`)

   A janela pertence a `janela_fechamento → periodo_apuracao`, usando o corte da empresa, limitado a 2–28; as bordas são inclusivas. Não é mês civil nem necessariamente corte 21. (`app/ponto/janelas.py:10`; `app/ponto/janelas.py:36`; `app/ponto/janelas.py:54`; `app/folha/export.py:1265`)

   **Não há um dia escalar próprio da disputa:** ela reúne perguntas, cada uma podendo ter `data_evento`. Para pergunta de fato, esse campo representa o dia do turno; conversa grava o dia civil da conversa. A atribuição canônica passa por `gravar_dia_do_fato`, após resolução pelo juiz do turno. (`app/chamados/models.py:2521`; `app/chamados/models.py:2888`; `app/chamados/models.py:2900`; `app/chamados/services/fio.py:837`; `app/chamados/services/fio.py:1698`)

2. **Colide com lei ou aval?** Unificar os leitores concorda com LEI-AKITA 2. Entretanto, o SIM não pode ser simplesmente “aberta + algum dia dentro”: a L-033 mantém pagável o dia com entrada e saída batidas, faltando somente intervalo, com pergunta aberta. Hoje essa exceção existe no retentor da célula, mediante protestos exatamente `{'FURO_PARCIAL'}` e conferência da ata pelo juiz de marcos; não está no bloqueador global do PDF. (`CLAUDE.md:20`; `app/docs/LEIS.md:81`; `app/folha/export.py:1150`; `app/folha/export.py:1173`; `app/folha/export.py:1293`; `app/chamados/services/disputa_emissao.py:40`)

   Os juízes registrados para pergunta viva, fechamento da disputa, tarefa e respostas sem veredito respondem perguntas distintas. O helper global existente tampouco responde ao novo recorte temporal. Acrescentar janela e exceção muda predicado e universo; não cabe apresentá-lo como mera declaração de autoridade já existente com comportamento idêntico. (`app/core/juizes.py:291`; `app/core/juizes.py:304`; `app/core/juizes.py:308`; `app/core/juizes.py:329`; `CLAUDE.md:279`)

3. **O literal fecha? Não.** Precisam ficar determinados:

   - **Vários fatos, dentro e fora:** o resolvedor do chamado-pai devolve o **menor** dia das perguntas de fato. Usá-lo para perguntar “há algum dentro?” pode esconder outro fato dentro da janela. (`app/chamados/juizes.py:75`; `app/chamados/juizes.py:85`)
   - **Quais perguntas contam:** todas da disputa aberta, somente vivas ou somente sem decisão? “Disputa aberta” hoje testa apenas `fechada_em`; C7 consulta separadamente perguntas vivas e respostas sem veredito. (`app/chamados/models.py:1888`; `app/chamados/models.py:1928`)
   - **Sem dia resolvível:** o campo permite NULL e o resolvedor pode retornar `None`; conversa não fornece dia de fato. O destino desses casos não foi decidido pelo dado fornecido. (`app/chamados/models.py:2528`; `app/chamados/juizes.py:79`; `app/chamados/juizes.py:85`; [nao verificado])
   - **Exceção L-033:** “preservada” precisa compor a resposta única; deixá-la somente no antigo ramo da célula permite que o novo bloqueio a contorne. (`app/folha/export.py:1173`; `app/folha/export.py:1293`)

4. **Toca exportada, dinheiro ou `!` fora do declarado?** Construído, o corte muda quem pode produzir linhas financeiras, pois o montador só admite os classificados como ENTRA. Isso está no efeito pretendido; não autoriza alterar valores, batidas ou o gravado de exportadas. Trocar o helper global também alcançaria leitores além dos dois declarados, como API e permissão de edição. (`app/folha/export.py:716`; `app/folha/export.py:726`; `CLAUDE.md:265`; `app/api/views.py:1336`; `app/colaboradores/mixins.py:307`)

**Item 2 — EFEITO**

1. **As premissas conferem?** O mecanismo existente permite excluir a pessoa pelo resultado `status='fora'` com motivo e detalhe; o montador omite suas linhas. No PDF, o bloqueio auditável provoca erro; o lote captura a falha e tenta o informacional. O efeito proposto é novo, mas esses caminhos existem. (`app/folha/export.py:837`; `app/folha/export.py:970`; `app/folha/export.py:726`; `app/relatorios/pdf_espelho.py:1087`; `app/relatorios/services.py:244`; `app/relatorios/services.py:250`)

2. **Colide com lei ou aval?** Não identifiquei colisão própria do efeito, desde que o SIM respeite a L-033 e o limite de competência definido no item 3. A exclusão deve continuar passando por `classificar_export`, autoridade registrada de quem entra no TXT. (`app/docs/LEIS.md:81`; `app/core/juizes.py:329`; `app/folha/export.py:716`)

3. **O literal fecha?** Fecha como direção para o pré-desenho, com duas ressalvas:

   - “Com NÃO, vale só o veredito da célula” **não descreve literalmente toda a classificação atual**: existem rescisão, código Domínio, férias com batida, cadastro e homologação, além da porta global. A leitura compatível com “como hoje” é: NÃO remove apenas este novo impedimento por disputa, preservando os demais crivos. (`app/folha/export.py:866`; `app/folha/export.py:872`; `app/folha/export.py:877`; `app/folha/export.py:959`; `app/folha/export.py:974`; `app/folha/views.py:128`)
   - Chave, texto e orientação do motivo novo ainda não estão especificados no anexo [nao verificado]. São entregas do pré-desenho; precisam alcançar apresentação e encaminhamento: hoje motivo diferente de `furo_espelho` recebe dono `admin`, e motivo sem mapeamento de tipo cai em `sem_causa`. Isso pode orientar mal uma disputa que espera resposta do colaborador. (`app/folha/retrato_folha.py:374`; `app/folha/retrato_folha.py:388`; `app/folha/fora_da_folha.py:50`)

   NÃO também não garante PDF auditável: o lote cai no informacional por qualquer exceção da tentativa auditável. (`app/relatorios/services.py:244`)

4. **Toca exportada, dinheiro ou `!` fora do declarado?** Excluir do TXT muda os eventos enviados e pode mudar o conjunto que recebe aprovação automática. Não significa zerar rubricas nem reescrever o fechamento. Esses efeitos precisam constar do pré-desenho, preservando a proteção do gravado exportado. (`app/folha/export.py:726`; `app/folha/views.py:215`; `app/folha/views.py:246`; `CLAUDE.md:411`)

**Item 3 — COMPETÊNCIA ENTREGUE / ABERTA**

1. **As premissas conferem?** ENTREGUE tem autoridade precisa: K6, existência de `ExportacaoDominio` vigente e não invalidada para empresa, mês e ano. Não equivale a `FechamentoMensal.status='exportado'`, aprovação, tranca ou holerite; TXT vigente invalidado sem substituta deixa de entregar. K9, separado, compõe K6 com holerite publicado para proteger a célula. (`app/folha/export.py:556`; `app/folha/export.py:591`; `app/folha/export.py:597`; `app/docs/CORTES.md:342`; `app/docs/CORTES.md:343`)

2. **Colide com lei ou aval?** Preservar o gravado exportado concorda com a L-092 consolidada. Porém, “nada muda” deve limitar **esta obra**, sem proibir correções pelas portas vigentes: a L-113 alterou a L-092, permitindo recalcular e refazer exportação com as garantias exigidas; a regra consolidada mantém `!` e porta específica para mover o gravado exportado. (`app/docs/LEIS.md:125`; `app/docs/LEIS.md:155`; `CLAUDE.md:411`; `CLAUDE.md:418`)

3. **O literal fecha? Não.** “Competência aberta” admite recortes diferentes no código:

   | Recorte | Significado atual |
   |---|---|
   | Marco `aberta` | Nenhum marco aprovada/exportada/trancada/paga verdadeiro. (`app/ponto/services/fechamento.py:1523`; `app/ponto/services/fechamento.py:1543`) |
   | Não entregue por K6 | Ausência de TXT vigente não invalidado. (`app/folha/export.py:591`) |
   | Não trancada | Ausência da tranca vigente; aprovação e exportação são outros marcos. (`app/ponto/services/fechamento.py:1473`) |
   | Escrita permitida por F6 | Não haver fechamento aprovado/exportado da pessoa na competência do dia. (`app/ponto/janelas.py:85`; `app/ponto/janelas.py:87`) |

   Falta escolher explicitamente o alcance para aprovada sem TXT, trancada sem K6, paga só por holerite, competência antiga sem entrega e TXT invalidado sem substituta. O anexo não fornece essa escolha [nao verificado].

4. **Toca exportada, dinheiro ou `!` fora do declarado?** O limite proposto evita movimento do gravado, mas isso sozinho não protege o comportamento dos leitores históricos: a geração recalcula objetos em leitura, e o PDF consulta a disputa atual. É necessário aplicar o recorte também nessas leituras; nenhum `save` financeiro precisa ocorrer para uma nova regra mudar quem entra num TXT regenerado ou qual modalidade de PDF é emitida. (`app/folha/views.py:207`; `app/folha/views.py:212`; `app/folha/views.py:268`; `app/relatorios/pdf_espelho.py:1085`)

**Escritores e leitores dos campos determinantes**

| Campo/valor | Quem escreve | Quem lê neste corte |
|---|---|---|
| Disputa: `colaborador`, `chamado`, `motivos`, `contexto` | Emissão cria e agrega; há também nascimentos por órfãos e âncora. (`app/chamados/services/disputa_emissao.py:550`; `app/chamados/services/disputa_emissao.py:576`; `app/chamados/services/disputa_emissao.py:601`; `app/chamados/services/disputa_emissao.py:1137`; `app/chamados/services/disputa_emissao.py:1372`) | Helper global lê colaborador; resolução do dia do chamado pode consultar contexto e perguntas. (`app/chamados/services/disputa_emissao.py:40`; `app/chamados/juizes.py:101`; `app/chamados/juizes.py:134`) |
| Disputa: `aberta_em` | ORM, `auto_now_add`. (`app/chamados/models.py:1820`) | Não integra o predicado atual do PDF nem o retentor do export. (`app/chamados/services/disputa_emissao.py:40`; `app/folha/export.py:1273`) |
| Disputa: `fechada_em` / aberta | `fechar` preenche; reaberturas admin e sistema zeram. (`app/chamados/models.py:2013`; `app/chamados/models.py:2230`; `app/chamados/models.py:2254`) | Property `aberta` e helper usado pelo PDF. (`app/chamados/models.py:1888`; `app/chamados/services/disputa_emissao.py:41`) |
| Pergunta: `data_evento` | Atribuição em `gravar_dia_do_fato`; nascimento, costura e re-chaveamento persistem. (`app/chamados/services/fio.py:837`; `app/chamados/models.py:3123`; `app/chamados/services/fio.py:926`; `app/chamados/services/fio.py:1090`) | Resolvedor do dia e juiz do turno da linha; os bloqueadores atuais de export/PDF não o consultam diretamente. (`app/chamados/juizes.py:81`; `app/chamados/services/fio.py:1698`; `app/folha/export.py:1273`; `app/chamados/services/disputa_emissao.py:40`) |
| Pergunta: `conversa`, `celula` | Portas de abertura distinguem fato/conversa; nascimento e re-chaveamento vinculam célula. (`app/chamados/models.py:2894`; `app/chamados/models.py:2906`; `app/chamados/models.py:3124`; `app/chamados/services/fio.py:1085`) | Resolvedor exclui conversa e usa célula como alternativa ao dia ausente. (`app/chamados/juizes.py:79`; `app/chamados/juizes.py:83`) |
| Empresa: `dia_inicio_competencia`; janela resultante | Formulário alimenta portas de criar/atualizar empresa; janela é derivada. (`app/colaboradores/views.py:515`; `app/colaboradores/views.py:542`; `app/colaboradores/views.py:557`; `app/colaboradores/services/estrutura.py:13`) | `corte_da_empresa`, `janela_fechamento`, export e lote PDF. (`app/ponto/janelas.py:50`; `app/ponto/janelas.py:57`; `app/folha/export.py:1306`; `app/relatorios/services.py:214`) |
| Célula: `veredito`, `protestos`, `ata`; isenção L-033 | Porta `lavrar_veredito`; isenção é derivada, sem reescrever a célula. (`app/ponto/portas/celula.py:1032`; `app/ponto/portas/celula.py:1090`; `app/folha/export.py:1173`) | Retentor e juiz de marcos usado na exceção. (`app/folha/export.py:1273`; `app/folha/export.py:1286`; `app/folha/export.py:1150`) |
| Nova resposta única SIM/NÃO | Escritor/derivador e endereço ainda não definidos no anexo [nao verificado]. | Leitores pretendidos: export e espelho PDF; leitores atuais seguem distintos. (`app/folha/export.py:968`; `app/relatorios/pdf_espelho.py:1085`) |
| Export: `status`, `motivo`, `detalhe`, `travas_aprovacao` | `classificar_export`. (`app/folha/export.py:837`; `app/folha/export.py:968`) | Montador, aprovação automática e painel. (`app/folha/export.py:716`; `app/folha/views.py:244`; `app/folha/views.py:330`) |
| Rótulo e orientação; retrato `dono`, `acoes`, `tipo`, `saldo`, `dias`, `datas` | Rótulos em `MOTIVO_RH`; ações vêm da tarefa; `montar_linhas` monta projeção, e comando grava retrato. (`app/folha/views.py:14`; `app/folha/fora_da_folha.py:217`; `app/folha/retrato_folha.py:1237`; `app/folha/management/commands/lavrar_retrato_folha.py:77`) | Tela lê retrato; PDF de retidos lê itens e rótulos. A orientação nova ainda não tem texto definido [nao verificado]. (`app/folha/views_retrato.py:278`; `app/folha/views.py:859`) |
| Modalidade do espelho e registro auditável | Bloqueio decide admissão; lote escolhe tentativa informacional; gerador auditável grava registro/hash. (`app/relatorios/pdf_espelho.py:1113`; `app/relatorios/services.py:244`; `app/relatorios/pdf_base.py:152`) | Lote agrega bytes; informacional segue seu gerador próprio. (`app/relatorios/services.py:241`; `app/relatorios/services.py:250`) |
| Exportação: empresa/mês/ano, conteúdo, hash, autoria e instante | `ExportacaoDominio.registrar`, chamado pelo gerador; instante pelo ORM. (`app/folha/models.py:80`; `app/folha/models.py:182`; `app/folha/views.py:297`) | K6 lê tripla e vigência; desempate usa instante e PK. (`app/folha/export.py:581`; `app/folha/export.py:588`; `app/folha/models.py:220`) |
| Exportação: invalidação, substituta, vigência e ENTREGUE | `invalidar` grava invalidação/substituta; `anotar_vigencia` e K6 derivam resultados. (`app/folha/models.py:158`; `app/folha/models.py:162`; `app/folha/models.py:224`; `app/folha/export.py:591`) | K6 e `marcos_da_competencia`. (`app/folha/export.py:591`; `app/ponto/services/fechamento.py:1537`) |
| Fechamento: status; tranca/reabertura; marco aberta | Portas de aprovar/reabrir e trancar/reabrir período; marco é derivado. (`app/ponto/services/fechamento.py:1335`; `app/ponto/services/fechamento.py:1349`; `app/ponto/services/fechamento.py:1400`; `app/ponto/services/fechamento.py:1432`; `app/ponto/services/fechamento.py:1543`) | F6, K4/K7 e marcos; não substituem K6. (`app/ponto/janelas.py:87`; `app/ponto/services/fechamento.py:1470`; `app/ponto/services/fechamento.py:1480`; `app/folha/export.py:591`) |

Os quantitativos individuais, causas dos dias irresolvíveis, valores financeiros e distribuição por empresa não podem ser recuperados do resumo raspado fornecido; não atribuo essas características aos 49 ou aos 58 [nao verificado].

VEREDITO por item: 1 PROCEDE; 2 CONFERE; 3 PROCEDE

## 17/11 11:38 · sessao 0e5cc5138206 · - · - · origem=Ronald (tmux)

Censo dos indiretos, agrupado pela pergunta herdada. **Estes locais hoje perguntam “entra/retém pela célula”; não perguntam existência de disputa aberta.** O novo juiz de disputa só os alcançará se compuser `classificar_export`; migrá-los individualmente duplicaria a pergunta. (`app/folha/export.py:968`, `app/folha/export.py:1307`)

| Leitor/callsite | Efeito da resposta atual |
|---|---|
| `app/folha/export.py:714` | Montador usa `status=entra`; ignora pessoas fora. (`app/folha/export.py:716`, `app/folha/export.py:726`) |
| `app/folha/views.py:212` | Gerar TXT classifica os objetos recalculados em memória; admissão no TXT e aprovação automática têm respostas separadas. (`app/folha/views.py:215`, `app/folha/views.py:244`) |
| `app/folha/views.py:331` | Preview conta entram/fora, agrupa todos motivos e mostra detalhe; K6 gera apenas aviso de exportação anterior. (`app/folha/views.py:332`, `app/folha/views.py:340`, `app/folha/views.py:354`, `app/folha/views.py:403`) |
| `app/folha/views.py:589` | Raio X separa furo, férias, rescisão e complemento de outros motivos. (`app/folha/views.py:591`, `app/folha/views.py:600`) |
| `app/folha/views.py:858` | `retidos_pdf` entrega PDF dos classificados fora. (`app/folha/views.py:840`) |
| `app/folha/validacao_pdf.py:65` | PDF de validação monta o mesmo TXT com esses itens e o traduz. (`app/folha/validacao_pdf.py:66`, `app/folha/validacao_pdf.py:68`) |
| `app/folha/porta_export.py:469` | Medição da porta usa apenas ENTRA como universo das comparações espelho/fechamento; não é um bloqueio de disputa próprio. (`app/folha/porta_export.py:470`, `app/folha/porta_export.py:476`) |
| `app/folha/services/vigia_txt.py:231` | Vigia compara TXT vigente gravado com TXT que seria montado hoje; invalidado sem substituto vira INCOMPARÁVEL. (`app/folha/services/vigia_txt.py:220`, `app/folha/services/vigia_txt.py:232`, `app/folha/services/vigia_txt.py:244`, `app/folha/services/vigia_txt.py:254`) |
| `app/folha/fora_da_folha.py:147` | Conta entram e fora por motivo; só `furo_espelho` ganha carga de dias/tarefa. (`app/folha/fora_da_folha.py:151`, `app/folha/fora_da_folha.py:156`, `app/folha/fora_da_folha.py:165`) |
| `app/folha/fora_da_folha.py:273` | `o_que_segura` retorna situação/motivo do export e lista diária enriquecida. (`app/folha/fora_da_folha.py:275`, `app/folha/fora_da_folha.py:302`) |
| `app/folha/previa.py:43` | Prévia de proposta cruza veredito com células/perguntas da janela. (`app/folha/previa.py:50`, `app/folha/previa.py:64`) |
| `app/ponto/services/fechamento_lista.py:73` | Lista de fechamento lê tique de entrada e travas de aprovação; é alias `_classificar`. (`app/ponto/services/fechamento_lista.py:100`, `app/ponto/services/fechamento_lista.py:102`) |
| `app/ponto/views.py:1299` | Aprovação em lote lê somente `travas_aprovacao`, aplica seleção e decide quais aprovar; disputa não é veto atual. (`app/ponto/views.py:1301`, `app/ponto/views.py:1311`, `app/ponto/views.py:1328`) |
| `app/ponto/views.py:1412` | Localiza aprovados com trava `conta_vazia` para reabertura. (`app/ponto/views.py:1413`, `app/ponto/views.py:1426`) |
| `app/ponto/views.py:1509` | Aprovação individual lê apenas travas do objeto recalculado e recusa ou aprova. (`app/ponto/views.py:1511`, `app/ponto/views.py:1513`, `app/ponto/views.py:1518`) |
| `app/ponto/services/dias_em_aberto.py:104` | Lista dias em aberto somente de quem entra no TXT. (`app/ponto/services/dias_em_aberto.py:107`, `app/ponto/services/dias_em_aberto.py:112`) |
| `app/relatorios/views.py:1660` | Ranking de furos publica coluna Folha APTO/motivo, inclusive PDF. (`app/relatorios/views.py:1668`, `app/relatorios/views.py:1671`, `app/relatorios/views.py:1677`) |
| `app/relatorios/views_pend.py:58` | Pendências de cadastro selecionam só FORA por motivos de cadastro conhecidos; hoje apenas sem código Domínio. (`app/relatorios/views_pend.py:48`, `app/relatorios/views_pend.py:59`, `app/relatorios/views_pend.py:68`) |
| `app/api/views_mensageria.py:283` | API devolve total, entram, retidos e agrupamento por motivo com exemplos; pode servir snapshot em ramo anterior. (`app/api/views_mensageria.py:270`, `app/api/views_mensageria.py:288`, `app/api/views_mensageria.py:299`) |
| `app/inteligencia/calculadores.py:210` | Métrica da competência anterior conta bloqueios e exemplos por motivo. (`app/inteligencia/calculadores.py:206`, `app/inteligencia/calculadores.py:217`) |
| `app/inteligencia/calculadores.py:271` | Métrica da competência atual conta entram/total e bloqueios. (`app/inteligencia/calculadores.py:267`, `app/inteligencia/calculadores.py:277`, `app/inteligencia/calculadores.py:281`) |
| `app/folha/management/commands/simular_folha.py:133` | Fotografia de TXT, hash, fora por motivo e retidos; pode usar lido ou gravado. (`app/folha/management/commands/simular_folha.py:134`, `app/folha/management/commands/simular_folha.py:146`, `app/folha/management/commands/simular_folha.py:155`) |
| `app/relatorios/management/commands/cartao_x_txt_divergentes.py:53` | ENTRA define universo de comparação cartão/TXT e contador de dias em aberto. (`app/relatorios/management/commands/cartao_x_txt_divergentes.py:57`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:64`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:68`) |
| `app/relatorios/management/commands/pdf_x_espelho_divergentes.py:58` | ENTRA define universo da comparação de conteúdo PDF/espelho, via coletores, não geração auditável. (`app/relatorios/management/commands/pdf_x_espelho_divergentes.py:61`, `app/relatorios/management/commands/pdf_x_espelho_divergentes.py:73`) |
| `app/ponto/management/commands/e6_oraculo.py:309` | Com `--so-txt`, restringe universo aos ENTRA. (`app/ponto/management/commands/e6_oraculo.py:307`, `app/ponto/management/commands/e6_oraculo.py:313`) |
| `app/ponto/management/commands/folga_que_sumiu.py:82` | Marca casos/valor de folga sumida que chegam ao TXT. (`app/ponto/management/commands/folga_que_sumiu.py:84`, `app/ponto/management/commands/folga_que_sumiu.py:96`) |
| `app/ponto/management/commands/aplicar_09_corte_b.py:190`, `app/ponto/management/commands/aplicar_09_corte_b.py:273` | Fotografa status/motivo antes/depois do corte de cálculo. |
| `app/ponto/management/commands/desvio_o68b.py:191`, `app/ponto/management/commands/desvio_o68b.py:217` | Fotografa status/motivo e reporta mudança após reversão. (`app/ponto/management/commands/desvio_o68b.py:218`) |
| `app/ponto/management/commands/celula_veredito_velho.py:82`, `app/ponto/management/commands/celula_veredito_velho.py:152` | Fotografa classificação antes/depois de recalcular células/fechamentos. |
| `app/ponto/management/commands/diff_reclassificar_partido.py:153`, `app/ponto/management/commands/diff_reclassificar_partido.py:209` | Fotografa classificação antes/depois da reclassificação/recalculo de escala. |

Leitores diretos da retenção da célula, além do export:

| Callsite | Efeito |
|---|---|
| `app/folha/fora_da_folha.py:121` | `_dias_que_retem`: motivos e dias para agrupamento, retrato e orientação. (`app/folha/fora_da_folha.py:165`) |
| `app/folha/previa.py:170` | Decide se os dias propostos cobrem **todos** os dias que retêm; preserva L-033/data passada/admissão/demissão herdados. (`app/folha/previa.py:171`) |
| `app/folha/services/vigia_pausa.py:125` | Conta barrados, retidos só por pausa e livres com dia só de pausa, conferindo exceção L-033. (`app/folha/services/vigia_pausa.py:127`, `app/folha/services/vigia_pausa.py:128`, `app/folha/services/vigia_pausa.py:133`) |
| `app/ponto/services/fechamento.py:1067` | Alias `_mre`: materializa `FechamentoMensal.motivos_espelho`; não é critério novo de disputa. (`app/ponto/services/fechamento.py:1066`) |

Cascas do PDF, todas herdando a pergunta global atual da geração auditável:

- Service `app/relatorios/services.py:70` → gera bytes/registro e eventualmente anexa ao chamado. (`app/relatorios/services.py:74`, `app/relatorios/services.py:88`)
- Lote `app/relatorios/services.py:237` → auditável; exceção → informacional. (`app/relatorios/services.py:244`, `app/relatorios/services.py:250`)
- Job `app/relatorios/lote_espelho.py:205` → chama lote e conta informacionais para o resultado do job. (`app/relatorios/lote_espelho.py:227`)
- Anexar ao chamado `app/relatorios/views.py:1003` → erro e redirect em disputa. (`app/relatorios/views.py:1007`)
- Avulso competência `app/relatorios/views.py:1127` → formulário com erro em disputa. (`app/relatorios/views.py:1131`)
- Avulso período livre `app/relatorios/views.py:1098` → sempre informacional, não faz essa pergunta. (`app/relatorios/views.py:1064`)

Interação isento/desligado:

- O retentor da célula sai com lista vazia para `isento_ponto(colab)` ou `esta_desligado(colab)` antes de percorrer a janela. (`app/folha/export.py:1205`, `app/folha/export.py:1207`, `app/folha/export.py:1211`)
- Desligado sem decisão aparece pelo ramo `informativo_rescisao`, explicitamente sem retenção; a lei no código diz “não retém ... em NENHUM dia da competência”. (`app/folha/export.py:1197`, `app/folha/export.py:1216`, `app/folha/export.py:1225`)
- Rescisão do export vem antes e pode retirar a pessoa por módulo próprio, independente de disputa. (`app/folha/export.py:866`)
- Portanto um novo bloqueio de disputa que só preserve L-033, mas pule as exclusões isento/desligado, exige esclarecer a interação com essas leis existentes; não assumir revogação implícita [nao verificado].

## 17/11 11:37 · sessao 7d012196cc1a · - · - · origem=Ronald (tmux)

**Item 2: CONFERE.** O efeito SIM constitui a nova política submetida; chave, texto do motivo e orientação são pendências do pré-desenho, não contradições que exijam devolução [nao verificado — literal fornecido].

Leia o NÃO como ausência **deste impedimento por disputa**, preservando os demais crivos atuais; “só célula” não descreve literalmente toda a classificação existente. (`app/folha/export.py:837`, `app/folha/export.py:968`, `app/folha/export.py:716`)

## 17/11 11:37 · sessao b82be6056e62 · - · - · origem=Ronald (tmux)

Censo de apresentação/API e eventos, com `A = DisputaSupervisao.fechada_em IS NULL`; `aberta` é exatamente isso, sem olhar estado do chamado (`app/chamados/models.py:1886-1888`).

| Leitor | Predicado/data | Uso da resposta |
|---|---|---|
| Helper público | `colaborador=... AND A`, `.first()`, sem datas (`app/chamados/services/disputa_emissao.py:34-42`) | Retorna disputa ou `None`; reexportado por `chamados.models` (`app/chamados/models.py:4260`). |
| Helper do ponto | Mesmo helper; colaborador `None` retorna `None` (`app/ponto/services/disputa.py:18-31`) | Nome sugere bloqueio, execução só retorna disputa (`app/ponto/services/disputa.py:29-31`). |
| Espelho HTML | Helper, somente quando colaborador e modo não gestor; sem recorte da competência (`app/ponto/views.py:140-144`) | Mostra banner e continua exibindo cálculos (`app/ponto/views.py:180-183`; `app/templates/ponto/espelho.html:13-18`). |
| API espelho | Helper; sem data (`app/api/views.py:1333-1336`) | Só sinaliza `em_disputa`, ID e abertura; calcula normalmente (`app/api/views.py:1333-1334`, `app/api/views.py:1670-1672`). |
| Gate cadastro/benefício | POST + colaborador + helper; isento passa; sem data (`app/colaboradores/mixins.py:300-312`) | Recusa com mensagem e redirecionamento (`app/colaboradores/mixins.py:308-312`). Afeta transferência de posto, conta bancária, lançamento e cancelamento de benefício (`app/colaboradores/views.py:1394`, `app/colaboradores/views.py:1641`, `app/colaboradores/views.py:1807`, `app/colaboradores/views.py:1915`). Reativar e desligar são explicitamente isentos (`app/colaboradores/views.py:1442`, `app/colaboradores/views.py:1463`). |
| Painel situacional lote | `A AND colaborador_id IN IDs`; sem datas (`app/colaboradores/services/situacional.py:117`) | LED vermelho por existência (`app/colaboradores/services/situacional.py:56-62`, `app/colaboradores/services/situacional.py:210`). Contador “em turno” usa juiz próprio e não a cor (`app/colaboradores/services/situacional.py:208-214`). |
| Painel live-cell | Colaborador + A, `.exists()`, sem datas (`app/colaboradores/painel_cell.py:31-32`) | Mesmo LED vermelho (`app/colaboradores/painel_cell.py:68-70`). |
| Drawer operacional | Colaborador + A, mais recente por `-aberta_em`; sem janela (`app/colaboradores/services/drawer.py:71-73`) | Banner dos motivos das perguntas vivas e link do pai (`app/colaboradores/services/drawer.py:76-87`, `app/colaboradores/services/drawer.py:127-132`). |
| Relatório de pendências | Todas A, sem datas (`app/chamados/relatorios_disputa.py:45-50`) | Separa sem pergunta, pronta fechar por C7, aguarda colaborador e aguarda supervisão (`app/chamados/relatorios_disputa.py:11-28`). Tela e command usam mesmo coletor (`app/colaboradores/views.py:3040-3053`; `app/chamados/management/commands/relatorio_pendencias_disputa.py:15`). |
| Contador modal fio | Colaborador + A, `.count()`, sem datas; mensagens ao lado têm janela (`app/chamados/views.py:903-912`) | Contador mostrado como “disputa(s) aberta(s)” (`app/chamados/views.py:947`; `app/templates/chamados/partials/modal_fio.html:19`). |
| Detalhe chamado | `chamado=... AND A`, `.first()`, sem datas (`app/chamados/services/acoes_chamado.py:345-347`) | Carrega perguntas C6 para tela e habilita bloco de regularização (`app/chamados/services/acoes_chamado.py:350-358`, `app/chamados/services/acoes_chamado.py:417-419`; `app/templates/chamados/detalhe_local.html:296-326`). Texto da tela ainda promete “motor não calcula ... fechamento mensal pula” (`app/templates/chamados/detalhe_local.html:304`), contrariando execução atual documentada pela própria view (`app/ponto/views.py:181-183`). |
| Ação regularizar | Chamado + A; sem datas (`app/chamados/views.py:470-475`) | Recusa se não existe, senão delega ação (`app/chamados/views.py:473-475`). |
| Validar/recusar/corrigir pergunta | PK pergunta + disputa A, depois `chamado_que_lista` igual ao chamado; sem janela (`app/chamados/views.py:493-501`, `app/chamados/views.py:510-518`, `app/chamados/views.py:528-536`) | Autoriza verbo administrativo ou recusa pergunta inválida nas mesmas linhas. |
| Validar inline | Busca A por pai; fallback colaborador + pergunta destinada ao chamado; sem data (`app/chamados/views_cobrar.py:160-167`) | Trabalha perguntas vivas respondidas; se não encontra retorna erro (`app/chamados/views_cobrar.py:166-176`). |
| Solicitar correção web/API | PK disputa + próprio colaborador + A; sem datas (`app/chamados/views.py:1328-1334`; `app/api/views.py:2677-2686`) | Recusa fechada/inexistente; chama `reabrir_por_colab` nas mesmas linhas. |
| API chamados | Todas A do colaborador, sem data na consulta de disputas (`app/api/views.py:1016-1022`) | Associa perguntas do app aos destinos vivos ou ao pai; pula disputa fechada (`app/api/views.py:1033-1040`). |
| API perguntas | **Outro predicado:** perguntas C6 vivas particionadas em `respondida_em IS NULL AND disputa.fechada_em IS NULL`, sem janela própria (`app/chamados/services/pergunta_no_app.py:52-61`, `app/chamados/services/pergunta_no_app.py:76-85`) | `em_disputa` é `bool(pendentes)`, não “existe disputa aberta” (`app/api/views.py:2537-2539`, `app/api/views.py:2567-2578`). |
| Badge API e dashboard | Mesma partição do app (`app/chamados/services/pergunta_no_app.py:100-119`) | API principal, kernel core, painel colaborador e dashboard contam pendentes (`app/api/views.py:315-319`; `app/api/views_core.py:303-307`; `app/colaboradores/views.py:959-960`; `app/core/views.py:252-253`). |
| Aviso após texto livre | Mesmas pendentes do app (`app/chamados/services/aviso_pendencia.py:7-20`) | Avisa que texto livre não respondeu questionário; leitores web/API (`app/chamados/views.py:1119`; `app/api/views.py:987`; `app/api/views.py:2211`). |
| Aparência e botões modal | Property `.aberta` sem data (`app/chamados/models.py:1886-1888`) | Cores abertas/fechadas e ações administrativas (`app/templates/chamados/partials/modal_fio.html:76`, `app/templates/chamados/partials/modal_fio.html:353`, `app/templates/chamados/partials/modal_fio.html:406`; `app/chamados/templatetags/chamados_tags.py:105`). |
| Reativar posto | A + posto + motivo `posto_desativado`; sem data (`app/colaboradores/signals.py:71-75`) | Remove motivo pela porta (`app/colaboradores/signals.py:76-82`). |
| Preencher âncora | A + colaborador + motivo `ancora_ausente`; sem data (`app/escala/signals.py:168-172`) | Remove motivo pela porta (`app/escala/signals.py:173-182`). |
| Receber foto | A + colaborador + `foto_ausente`; depois pergunta do episódio `batida_origem=batida`; sem janela (`app/api/foto_recebida.py:65-74`) | Valida, poda motivo e tenta fechar por C7 (`app/api/foto_recebida.py:75-87`). |
| Desligar | Todas A do colaborador, sem data (`app/colaboradores/services/desligamento.py:164`) | Fecha pela porta de desligamento (`app/colaboradores/services/desligamento.py:167`). |
| Regularização externa | Colaborador + A, `.first()`, sem data (`app/chamados/services/regularizacao_ext.py:59-61`) | Bloqueia materialização externa e retorna orientação (`app/chamados/services/regularizacao_ext.py:62-66`). |
| Utilitário censo folha | Pergunta do colaborador + resposta NULL + disputa A; sem data nesse contador (`bin/censo_folha.py:28`) | Acrescenta agravante `PERG_VIVAS` e grava CSV se executado (`bin/censo_folha.py:37`, `bin/censo_folha.py:44-47`). Não foi executado [nao verificado]. |

Alertas e escalonamento encontrados, para complementar o censo do pai: alerta diário seleciona todas A sem data do fato (`app/ponto/management/commands/alertar_disputas_pendentes.py:33-35`), envia push/agrupa empresa e mede idade por abertura (`app/ponto/management/commands/alertar_disputas_pendentes.py:48-74`); escalonador exige disputa A por chamado, sem data, para evitar órfão e marcar incidente (`app/ponto/management/commands/escalonar_chamados_supervisao.py:57-60`, `app/ponto/management/commands/escalonar_chamados_supervisao.py:148-163`).

Ponto de atenção: `lancar_beneficio` fornece ao gate `POST['colaborador_id']`, mas o corpo lê `POST['colaborador']`; o alcance efetivo do gate para esse formulário depende do payload, não demonstrado no dado (`app/colaboradores/views.py:1807`, `app/colaboradores/views.py:1813`).

## 17/11 11:37 · sessao f42d81e1bbd4 · - · - · origem=Ronald (tmux)

**Juiz mais próximo e ausência do predicado completo**

- `colaborador_em_disputa(colaborador)` retorna `.first()` de `DisputaSupervisao` por `colaborador` e `fechada_em__isnull=True`; não considera datas, motivos, perguntas, janela ou competência. É o corpo mais próximo da pergunta simples “tem disputa aberta?”. (`app/chamados/services/disputa_emissao.py:34`, `app/chamados/services/disputa_emissao.py:40`)
- Estado aberto também existe como propriedade `DisputaSupervisao.aberta`: `self.fechada_em is None`. (`app/chamados/models.py:1886`)
- `disputa_viva_do_chamado` responde outra pergunta: busca disputa aberta do chamado e a retorna somente se não houver perguntas ou se `fio_vivo(perguntas)`; serve à trava do encerramento do pai, sem janela. (`app/chamados/juizes.py:2458`, `app/chamados/juizes.py:2466`)
- O registro vivo contém C6=`pergunta_viva`, C7=`pode_fechar`, K2=`classificar_export`, K6=`competencia_entregue`; não encontrei autoridade registrada para “disputa aberta com fato dentro da janela”. A ausência em toda combinação dinâmica possível permanece [nao verificado]. (`app/core/juizes.py:291`, `app/core/juizes.py:292`, `app/core/juizes.py:330`, `app/core/juizes.py:343`)
- Há leitores de perguntas datadas na janela, porém não são o predicado novo: prévia busca `data_evento__range=(ini,fim)` sem filtrar disputa aberta; pauta de competência trancada também busca datas e depois julga perguntas, sem exigir recipiente aberto. (`app/folha/previa.py:64`, `app/chamados/services/competencia_trancada.py:119`)

**Janela e K6**

- `janela_fechamento(mes,ano,empresa)` chama `periodo_apuracao(mes,ano,corte_da_empresa(empresa))`. (`app/ponto/janelas.py:54`)
- `corte_da_empresa` lê `Empresa.dia_inicio_competencia`, admite empresa ou PK, usa padrão 21 e limita a 2–28. (`app/ponto/janelas.py:36`, `app/ponto/janelas.py:49`, `app/ponto/janelas.py:50`)
- A janela começa no dia do corte do mês anterior e termina no corte−1 do mês da competência, inclusive os dois limites. (`app/ponto/janelas.py:10`, `app/ponto/janelas.py:15`)
- `janela_atual(dia,empresa)` encontra a competência que contém o dia, usando o mesmo corte; não equivale ao mês civil. (`app/ponto/janelas.py:24`, `app/ponto/janelas.py:60`)
- K6 é `competencia_entregue`: existe `ExportacaoDominio` vigente para empresa/mês/ano e o vigente não está invalidado; mais recente desempata por `(gerado_em,pk)`. Não equivale a `FechamentoMensal.status='aprovado'` nem ao juiz F6. (`app/folha/export.py:556`, `app/folha/export.py:588`, `app/folha/export.py:591`, `app/folha/models.py:217`, `app/ponto/janelas.py:75`)
- A composição existente `competencia_do_dia_entregue` faz dia→`janela_atual`→K6 e devolve `None` sem dia. (`app/chamados/services/vigia_fio.py:785`, `app/chamados/services/vigia_fio.py:797`)

**De onde vem o fato**

- A disputa não tem campo próprio `data_fato`; os campos temporais do recipiente são `aberta_em` e `fechada_em`. O campo factual das perguntas é `PerguntaDisputa.data_evento`, nullable. (`app/chamados/models.py:1820`, `app/chamados/models.py:2528`)
- Uma disputa acumula perguntas de vários dias; `contexto[motivo]['data']` guarda uma data, a última, e não representa o conjunto. (`app/chamados/services/disputa_emissao.py:174`)
- `data_evento` deve ser lido com `conversa`: conversa grava o dia civil de criação, embora não tenha fato de turno; usar simplesmente `data_evento` não nulo classificaria conversa como fato. (`app/chamados/models.py:2540`, `app/chamados/models.py:2899`, `app/chamados/models.py:2906`)
- No nascimento datado, emissão deriva dia da grade, contexto do motivo, origem da batida e/ou chamado de destino e passa-o a `PerguntaDisputa.abrir`; hora sem dia não nasce, demais perguntas sem fato vão a `abrir_conversa`. (`app/chamados/services/disputa_emissao.py:359`, `app/chamados/services/disputa_emissao.py:380`, `app/chamados/services/disputa_emissao.py:396`, `app/chamados/services/disputa_emissao.py:419`, `app/chamados/services/disputa_emissao.py:433`, `app/chamados/services/disputa_emissao.py:448`, `app/chamados/services/disputa_emissao.py:453`)
- A porta recalcula o dia pelo juiz `fio.dia_do_turno_da_linha`, antes de gravar. Com episódio, consulta `ponto.turnos.dia_do_turno_da_batida`; com marco, dia da célula; legado sem marco/episódio pode depender da ponte A2; pode faltar resposta ou haver recusa por alcançar vários dias. (`app/chamados/models.py:2985`, `app/chamados/services/fio.py:1676`, `app/chamados/services/fio.py:1699`)
- `chamados.juizes.dia_do_fato(chamado)` é outra autoridade, sobre chamado: célula→chave de catálogo/data_turno→origens fracas. `fortes=True` para antes dos degraus fracos. Seu fallback `_dia_da_disputa` retorna **a menor** data das perguntas não conversa, não o conjunto necessário ao corte. (`app/chamados/juizes.py:88`, `app/chamados/juizes.py:128`, `app/chamados/juizes.py:57`, `app/chamados/juizes.py:75`)

**Escritores e leitores dos insumos**

| Campo | Escritor | Leitores/observações |
|---|---|---|
| Disputa `colaborador`, `motivos`, `contexto`, `chamado` | Nascimento central: `_abrir_ou_agregar_disputa`; outro nascimento de agrupamento manual de órfãos. (`app/chamados/services/disputa_emissao.py:601`, `app/chamados/services/disputa_emissao.py:1137`) | Helper lê colaborador; agregador reutiliza aberta, agrega motivos e funde contexto. (`app/chamados/services/disputa_emissao.py:40`, `app/chamados/services/disputa_emissao.py:540`, `app/chamados/services/disputa_emissao.py:550`, `app/chamados/services/disputa_emissao.py:576`) |
| `aberta_em` | ORM `auto_now_add`. (`app/chamados/models.py:1820`) | Ordenação padrão `-aberta_em`; helper `.first()` herda essa ordenação, não significa fato mais recente. (`app/chamados/models.py:1868`, `app/chamados/services/disputa_emissao.py:42`) |
| `fechada_em` | `fechar` grava agora; `reabrir_admin` e `reabrir_sistema` zeram. (`app/chamados/models.py:2013`, `app/chamados/models.py:2230`, `app/chamados/models.py:2254`) | `.aberta`, helper público e `disputa_viva_do_chamado`. (`app/chamados/models.py:1888`, `app/chamados/services/disputa_emissao.py:41`, `app/chamados/juizes.py:2466`) |
| Pergunta `data_evento` | **Atribuição única** `fio.gravar_dia_do_fato`; salva no nascimento, na costura que data e no rechaveamento. (`app/chamados/services/fio.py:837`, `app/chamados/services/fio.py:847`, `app/chamados/models.py:3126`, `app/chamados/models.py:3127`, `app/chamados/services/fio.py:926`, `app/chamados/services/fio.py:974`, `app/chamados/services/fio.py:1084`, `app/chamados/services/fio.py:1090`) | Prévia por janela; dia do chamado como fallback; materialização e K6 do dia. (`app/folha/previa.py:65`, `app/chamados/juizes.py:81`, `app/chamados/services/materializacao.py:550`, `app/chamados/services/vigia_fio.py:779`) |
| Pergunta `conversa` | `abrir` passa False; `abrir_conversa` passa True; `_nascer` insere. (`app/chamados/models.py:2894`, `app/chamados/models.py:2906`, `app/chamados/models.py:3124`) | `_dia_da_disputa` exclui; juiz de dia distingue civil e turno. (`app/chamados/juizes.py:79`, `app/chamados/services/fio.py:1699`) |
| `celula`, `batida_origem`, `instancia_marco` | `_nascer` deriva origem e consulta juiz; rechaveamento muda célula junto ao dia. (`app/chamados/models.py:2976`, `app/chamados/models.py:2984`, `app/chamados/models.py:3124`, `app/chamados/services/fio.py:1085`) | `dia_do_turno_da_linha`/`_dia_do_fato` usam episódio ou marco para resolver dia. (`app/chamados/services/fio.py:1534`, `app/chamados/services/fio.py:1542`, `app/chamados/services/fio.py:1701`) |
| Empresa `dia_inicio_competencia` | Form recebe e limita; portas estrutura criam/atualizam. (`app/colaboradores/views.py:515`, `app/colaboradores/views.py:542`, `app/colaboradores/views.py:557`, `app/colaboradores/services/estrutura.py:13`, `app/colaboradores/services/estrutura.py:18`) | `corte_da_empresa` e as janelas. (`app/ponto/janelas.py:50`, `app/ponto/janelas.py:57`) |
| ED `empresa`, `mes`, `ano`, `gerado_em`, `conteudo`, hash | `ExportacaoDominio.registrar`, ORM grava data. (`app/folha/models.py:80`, `app/folha/models.py:174`, `app/folha/models.py:182`) | K6 e anotação de vigência. (`app/folha/export.py:588`, `app/folha/models.py:203`) |
| ED `invalidada_em`, `substituida_por` | `ExportacaoDominio.invalidar`. (`app/folha/models.py:143`, `app/folha/models.py:158`) | K6 descarta vigente invalidada. (`app/folha/export.py:591`) |

**Duas decisões que o literal parece não encerrar**

1. “Quando não sabemos de que dia é o problema, a pessoa entra ou fica fora e o PDF pode ser auditável?” O anexo manda medir sem decidir o destino; nenhuma resposta deve ser presumida. [nao verificado — anexo item 5]
2. “Se a disputa continua aberta por problema de outro mês, mas guarda uma pergunta deste mês já resolvida, essa pergunta antiga ainda tira a pessoa da folha?” O literal não define se o conjunto usado contém todas as perguntas ou somente os fatos ainda pendentes; essa diferença existe porque recipiente aberto e pergunta viva têm juízes separados. (`app/chamados/models.py:1888`, `app/core/juizes.py:291`; alcance do aval [nao verificado])

Mistura de vários dias com **ao menos um fato efetivamente em disputa dentro da janela** é o existencial do corte; selecionar apenas a menor data, a última do contexto ou a data de abertura perderia casos. (`app/chamados/services/disputa_emissao.py:174`, `app/chamados/juizes.py:85`; interpretação do corte [nao verificado])

## 17/11 11:37 · sessao 0e5cc5138206 · - · - · origem=Ronald (tmux)

Achados para consolidar, somente da cópia:

1. **Export atual**: `classificar_export` inicia `entra` e segue prioridade rescisão → código → férias com batida → cadastro zero → célula → homologação; disputa aberta não é um ramo. O retentor efetivo lê `CelulaDia` na janela, não disputa: acusação retém salvo protestos não cobráveis/L-033; ausência de célula/veredito tem regras próprias. (`app/folha/export.py:768`, `app/folha/export.py:841`, `app/folha/export.py:866`, `app/folha/export.py:968`, `app/folha/export.py:1247`, `app/folha/export.py:1273`, `app/folha/export.py:1293`, `app/folha/export.py:1295`)

2. `motivos_retencao_espelho` ignora os três primeiros argumentos e delega à célula com `janela_competencia`; esta delega a `janela_fechamento`. (`app/folha/export.py:1301`, `app/folha/export.py:1306`, `app/folha/export.py:47`)

3. O TXT recalcula em memória, classifica e monta com os mesmos objetos; só `entra` chega à montagem, há filtro `STATUS_NO_TXT=('aberto','aprovado')`, zeros não geram rubricas. O painel prévio e o retrato usam fechamento gravado, dívida reconhecida no próprio código. (`app/folha/views.py:207`, `app/folha/views.py:212`, `app/folha/views.py:268`, `app/folha/export.py:687`, `app/folha/export.py:716`, `app/folha/export.py:726`, `app/folha/export.py:733`, `app/folha/views.py:198`)

4. **PDF atual**: `gerar_pdf_espelho_bytes` chama `motor_bloqueado_por_disputa(colab)` sem competência e levanta `ValueError`; o bloqueio é passado à geração auditável. (`app/relatorios/pdf_espelho.py:1082`, `app/relatorios/pdf_espelho.py:1085`, `app/relatorios/pdf_espelho.py:1087`, `app/relatorios/pdf_espelho.py:1113`)

5. **Importante: PDF informacional não acontece em toda porta.** Lote tenta auditável e, em qualquer exceção, cai no informacional da janela; competência avulsa devolve formulário com erro; anexo ao chamado devolve erro/redirect. Período livre já gera sempre informacional, independentemente de disputa. (`app/relatorios/services.py:214`, `app/relatorios/services.py:237`, `app/relatorios/services.py:244`, `app/relatorios/services.py:250`, `app/relatorios/views.py:1126`, `app/relatorios/views.py:1131`, `app/relatorios/views.py:1002`, `app/relatorios/views.py:1007`, `app/relatorios/views.py:1064`, `app/relatorios/views.py:1098`)

6. **K6 não é status do fechamento.** É existência de exportação vigente não invalidada para empresa/mês/ano; `registro=True` devolve o registro. A seleção usa anotação de vigência; comentários documentam desempate por `(gerado_em,pk)`. (`app/folha/export.py:556`, `app/folha/export.py:564`, `app/folha/export.py:588`, `app/folha/export.py:591`)
   
   O preview consulta K6 só para aviso “Já exportado”; gerar TXT permite reexportação, grava outro registro e avisa depois. Logo não presumir que K6 hoje congela a classificação ou a geração. (`app/folha/views.py:354`, `app/folha/views.py:361`, `app/folha/views.py:295`, `app/folha/views.py:306`, `app/folha/models.py:178`, `app/folha/models.py:182`)
   
   Cada geração grava cópia fiel em `ExportacaoDominio.conteudo` e hash; invalidar altera campos de invalidação e preserva conteúdo/hash. (`app/folha/models.py:86`, `app/folha/models.py:143`, `app/folha/models.py:158`, `app/folha/models.py:182`)

7. **Retrato/admin — risco concreto do motivo próprio**:
   - `fora_da_folha` conta todos motivos, mas só enriquece pessoas retidas por `furo_espelho` com dias/fio/ações. (`app/folha/fora_da_folha.py:147`, `app/folha/fora_da_folha.py:156`, `app/folha/fora_da_folha.py:210`, `app/folha/fora_da_folha.py:234`)
   - `montar_linhas` copia status/motivo dos itens; dias/datas vêm dos enriquecidos por furo ou férias com batida. Novo motivo de disputa, sozinho, terá `dias=None`, `datas=None`, `acoes=[]`. (`app/folha/retrato_folha.py:1214`, `app/folha/retrato_folha.py:1227`, `app/folha/retrato_folha.py:1232`, `app/folha/retrato_folha.py:1237`, `app/folha/retrato_folha.py:1249`)
   - Qualquer motivo diferente de `furo_espelho` cai em fio `admin`, mesmo se disputa depender de resposta do colaborador. Tipo novo não mapeado vira `sem_causa`; saldo sem dia fica vazio. (`app/folha/retrato_folha.py:374`, `app/folha/retrato_folha.py:388`, `app/folha/retrato_folha.py:214`, `app/folha/retrato_folha.py:426`)
   - O lavrador único grava `linhas` e `lavrado_em` por `update_or_create`, sem consulta K6 nesse laço. (`app/folha/management/commands/lavrar_retrato_folha.py:65`, `app/folha/management/commands/lavrar_retrato_folha.py:77`)
   - Tela admin lê os retratos gravados pelo accessor e o painel do colaborador lê a mesma linha para “o que fazer”/portas; não recalcula disputa. (`app/folha/views_retrato.py:194`, `app/folha/views_retrato.py:200`, `app/colaboradores/views.py:1001`, `app/colaboradores/views.py:1005`)
   - “O que fazer” lê motivo, dono, destrava, tipo e saldo das chaves do retrato; links vêm de `acoes` ou fallback do fio/tipo. (`app/folha/retrato_folha.py:1044`, `app/folha/retrato_folha.py:1066`, `app/folha/retrato_folha.py:1112`, `app/folha/retrato_folha.py:1119`)
   - Preview agrupa todo motivo desconhecido e exibe chave crua + aviso de falta de rótulo, portanto pessoa não some, mas orientação/rótulo precisam chegar. Seu contador “dias” conta férias ou motivos, não novos dias da disputa. (`app/folha/views.py:339`, `app/folha/views.py:389`, `app/folha/views.py:398`)
   - Toast de retenção e contador no log do gerar TXT atualmente só agregam `furo_espelho`; motivo novo ficaria fora desse aviso. (`app/folha/views.py:257`, `app/folha/views.py:259`, `app/folha/views.py:293`, `app/folha/views.py:302`)

8. **L-033 existente**: exceção é `set(c.protestos or ()) == {'FURO_PARCIAL'} and _furo_so_da_pausa(c)`; esta lê `periodos_do_dia`, exige sem desalinhamento/abertura/desconhecimento e todas lâmpadas apagadas pertencendo aos pares de pausa. Dia misto continua retendo. (`app/folha/export.py:1150`, `app/folha/export.py:1151`, `app/folha/export.py:1153`, `app/folha/export.py:1155`, `app/folha/export.py:1173`, `app/folha/export.py:1293`)
   
   Há testes concretos de só intervalo, volta faltante, ponta faltante, misto e sem ata; eles cobrem o retentor da célula, não o novo bloqueio compartilhado com PDF. (`app/folha/tests/test_furo_so_intervalo_nao_retem.py:83`, `app/folha/tests/test_furo_so_intervalo_nao_retem.py:88`, `app/folha/tests/test_furo_so_intervalo_nao_retem.py:94`, `app/folha/tests/test_furo_so_intervalo_nao_retem.py:111`, `app/folha/tests/test_furo_so_intervalo_nao_retem.py:117`)

REDs propostos, sem construção nem execução:

- Disputa aberta, dia na janela, célula concorde/nenhuma retenção: hoje export entra e lote PDF é informacional; depois motivo próprio exclui TXT e PDF segue informacional. Morde falta de ramo em `classificar_export`. (`app/folha/export.py:968`; `app/relatorios/pdf_espelho.py:1085`)
- Disputa aberta só fora da janela, célula limpa: hoje PDF recusa; depois juiz responde NÃO e auditável pode seguir. Morde helper sem competência. (`app/relatorios/pdf_espelho.py:1085`)
- Dia do fato em novembro e `aberta_em` em setembro: mesmo resultado do primeiro caso; fronteiras `ini` e `fim` inclusive, véspera e dia seguinte excluídos. O caso col193 consta apenas no anexo [nao verificado]; implementação da janela deve vir do juiz existente. (`app/folha/export.py:47`)
- Vários dias: um dentro e outro fora → SIM; todos fora → NÃO; todos fechados → NÃO pela disputa, mas célula acusando continua retendo. Predicado existencial e fechamento devem ser testados junto ao juiz; sem isso troca pode consultar só primeiro dia [nao verificado].
- L-033 com disputa aberta do mesmo furo só de intervalo: hoje export não retém mas PDF recusa; depois juiz comum preserva exceção nos dois leitores. Ponta faltante/motivo adicional devem continuar SIM. (`app/folha/export.py:1173`, `app/relatorios/pdf_espelho.py:1085`)
- K6 verdadeiro + disputa nova dentro da janela: manter TXT gravado/hash/linhas, classificação/retrato preservados conforme alcance do aval; hoje adicionar condição só em export sem guarda mudaria novas classificações e lavratura. K6 invalidado sem substituto não equivale entregue. (`app/folha/export.py:591`, `app/folha/models.py:86`, `app/folha/management/commands/lavrar_retrato_folha.py:77`)
- Avulso e anexo de competência com SIM: hoje erro, depois informacional se “espelho sai informacional” abrange essas portas. (`app/relatorios/views.py:1007`, `app/relatorios/views.py:1131`)
- Motivo próprio chega ao preview, retidos PDF, contagem/log/toast, retrato e drawer com dia/ação e rótulo; teste aguardar colaborador vs aguardar admin morde falso Admin automático e ausência de dias/ações. (`app/folha/views.py:259`, `app/folha/views.py:389`, `app/folha/retrato_folha.py:374`, `app/folha/retrato_folha.py:1232`, `app/folha/retrato_folha.py:1249`)
- Catraca de autoridade: export/PDF devem ler exatamente a mesma resposta; leitores globais de disputa para outras finalidades não podem receber competência por acidente [nao verificado].
- Dias vazios: medir sem atribuir destino; RED comportamental fica pendente da decisão de lei, não inventar “fora da janela” ou “sem disputa” [nao verificado].

Teste atual do PDF cria disputa sem pergunta/data e exige bloqueio; ele deverá ser revisto com a decisão dos sem dia, não simplesmente reescrito para liberar. (`app/relatorios/tests/test_relatorios.py:84`, `app/relatorios/tests/test_relatorios.py:86`, `app/relatorios/tests/test_relatorios.py:90`)

_6528 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
