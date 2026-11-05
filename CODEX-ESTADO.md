# CODEX-ESTADO

> Ultima resposta processada: **05/11/2026 19:11** · publicado em **05/11/2026 19:16** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 05/11 19:11 · sessao 5816bfa46ce5 · H12H06 · verificador · origem=Code

A v6 fecha os contraexemplos de trava sobre a Ausencia errada e de UPDATE anterior à trava da Empresa. Ainda permanecem guarda copiada, um ciclo possível entre escritores e falhas no ciclo de vida do arquivo de reversão. (`app/ponto/tests/test_h06_reversao_guardas.py:412`, `app/ponto/tests/test_h06_reversao_guardas.py:867`; `app/ponto/services/ausencia.py:528`, `app/ponto/services/ausencia.py:617`, `app/ponto/services/ausencia.py:730`; `app/ponto/views.py:2638`; `app/chamados/models.py:866`.)

Revisão estática. Execução da suíte Django, mutantes de produção, identidade Git/hash da cópia, configuração efetiva do banco, ocorrência dos interleavings e cumprimento dos pré-requisitos em produção: **[nao verificado]**. [nao verificado]

**1. Achados da v5 e triagem**

| Linha da triagem | Situação em 3c6e39a5 |
|---|---|
| PROCEDENTE 1 — trava de Ausencia errada | **Fechado para o mutante apontado.** A porta para na foto anterior à escrita; as sondas agora disputam as três PKs reais. Travar `pk=-1` e carregar A sem trava deixa a sonda de A passar e reprova. (`app/ponto/services/ausencia.py:693`; `app/ponto/tests/test_h06_reversao_guardas.py:850`, `app/ponto/tests/test_h06_reversao_guardas.py:867`, `app/ponto/tests/test_h06_reversao_guardas.py:886`.) |
| PROCEDENTE 2 — ciclos por FK | **Parcial.** `NO KEY UPDATE` remove as arestas que dependiam exclusivamente de `KEY SHARE` da FK. Não elimina o ciclo explícito C→A contra A→C descrito abaixo. (`app/folha/export.py:570`; `app/ponto/services/ausencia.py:651`, `app/ponto/services/ausencia.py:654`; `app/ponto/views.py:2638`; `app/chamados/models.py:866`.) |
| PROCEDENTE 3 — helper escondia UPDATE anterior à Empresa | **Fechado para UPDATE ORM na conexão corrente, dentro da porta.** A contagem começa antes de executar a implementação da porta. Permanecem limites de conexão e reconhecimento textual do SQL. (`app/ponto/tests/test_h06_reversao_guardas.py:411`, `app/ponto/tests/test_h06_reversao_guardas.py:414`, `app/ponto/tests/test_h06_reversao_guardas.py:422`.) |
| PROCEDENTE 4 — motivo/usuário no comando | **Parcial.** O comando apenas resolve o usuário; a porta exige motivo e PK do ator. Entretanto, a exigência de motivo continua repetida na reposição. (`app/ponto/management/commands/rederivar_falta_declarada.py:88`; `app/ponto/services/ausencia.py:617`, `app/ponto/services/ausencia.py:619`, `app/ponto/services/ausencia.py:528`.) |
| PROCEDENTE 5 — trilha individual ignorada | **Fechado no código nas duas direções.** Retorno `None` agora levanta exceção tanto ao aplicar quanto ao repor, dentro da transação do ato. A nova prova de falha individual exercita somente aplicar. (`app/ponto/services/ausencia.py:533`, `app/ponto/services/ausencia.py:646`, `app/ponto/services/ausencia.py:721`; `app/ponto/tests/test_h06_reversao_guardas.py:590`.) |
| PROCEDENTE 6 — arquivo órfão | **Parcial.** Há limpeza após falha posterior à criação completa, mas o arquivo só entra em `escrito` depois de `write` e fechamento. A limpeza também confunde exceção ao sair do `atomic` com ausência de confirmação. (`app/ponto/services/ausencia.py:696`, `app/ponto/services/ausencia.py:698`, `app/ponto/services/ausencia.py:730`.) |
| PROCEDENTE 7 — PK repetido | **Fechado para duplicação.** Não fecha validação dos tipos dos valores. (`app/ponto/services/ausencia.py:631`, `app/ponto/services/ausencia.py:635`; `app/ponto/tests/test_h06_reversao_guardas.py:603`.) |
| PROCEDENTE 8 — AST, expressão e import com `as` | **Fechado para os exemplos apontados.** Agora alcança inteiros dentro da expressão e apelidos de `ImportFrom`; permanecem outros caminhos de transportar o literal. (`app/chamados/tests/test_h06_selo_minutos_sem_literal.py:30`, `app/chamados/tests/test_h06_selo_minutos_sem_literal.py:38`, `app/chamados/tests/test_h06_selo_minutos_sem_literal.py:79`.) |
| DESCARTE 1 — elegibilidade no censo | **Procede como separação entre censo e decisão sobre o estado atual.** O comando seleciona o universo; a porta reconfere identidade, minutos e empresa. Isso não torna a porta autossuficiente diante de um produtor arbitrário de `ato`. (`app/ponto/management/commands/rederivar_falta_declarada.py:55`, `app/ponto/management/commands/rederivar_falta_declarada.py:118`; `app/ponto/services/ausencia.py:665`, `app/ponto/services/ausencia.py:668`, `app/ponto/services/ausencia.py:670`.) |
| DESCARTE 2 — relógios preexistentes | **Procede quanto à autoria do delta; não significa problema curado.** O relógio novo nomeia arquivo na borda; os relógios internos continuam alcançáveis. (`app/ponto/management/commands/rederivar_falta_declarada.py:46`; `app/ponto/models.py:873`; `app/ponto/signals.py:122`; `app/ponto/services/cartorio.py:1965`.) |
| DESCARTE 3 — rateio uniforme | **Procede como fora da cura proposta; permanece aberto.** O leitor continua dividindo o total igualmente pelos dias. (`app/folha/export.py:118`, `app/folha/export.py:121`; `app/docs/PROMPTS.md:1861`.) |
| DESCARTE 4 — pré-requisitos do pouso | **Procede para separar construção de pouso.** Não autoriza considerar o pouso aprovado: medidas 09/10/11 e preservação da 09 continuam exigidas. Cumprimento: **[nao verificado]**. (`app/docs/PROMPTS.md:1860`, `app/docs/PROMPTS.md:1861`.) |
| DESCARTE 5 — motivo na reposição seria outro contrato | **Não procede sob a proibição literal de guarda copiada.** As duas funções exigem o mesmo predicado, na mesma cadeia de reversão. Ser um escritor de exceção não faz essa pergunta mudar. (`app/ponto/services/ausencia.py:528`, `app/ponto/services/ausencia.py:617`, `app/ponto/services/ausencia.py:725`.) |
| NÃO CURADO — ciclo com UI | **Aberto, com correção da fundamentação.** A edição simples não mostra transação envolvente; a aprovação de irmãos mostra. Existe contraexemplo com reversão obsoleta e aprovação da mesma A, detalhado no item 2. (`app/ponto/views.py:2791`, `app/ponto/views.py:2638`; `app/ponto/services/ausencia.py:654`; `app/chamados/models.py:866`.) |

Os demais achados nomeados na v5 ficam assim:

| Achado da v5 | Situação atual |
|---|---|
| Reversão sobre outra identidade/empresa; aplicar usando empresa antiga | **Fechados no código para os contraexemplos apresentados.** A comparação ocorre com C/A travados, antes da escrita. (`app/ponto/services/ausencia.py:651`, `app/ponto/services/ausencia.py:654`, `app/ponto/services/ausencia.py:665`, `app/ponto/services/ausencia.py:670`.) |
| Sobrescrita do arquivo | **Fechado.** Nome contém PID/UUID; `xb` recusa arquivo existente. (`app/ponto/management/commands/rederivar_falta_declarada.py:116`; `app/ponto/services/ausencia.py:696`; `app/ponto/tests/test_h06_reversao_guardas.py:236`.) |
| Concorrência/ranges/atomicidade | **Parcial.** Consulta todas as competências e envolve as escritas em `atomic`; compatibilidade global e arquivo externo à transação permanecem problemáticos. (`app/ponto/services/ausencia.py:559`, `app/ponto/services/ausencia.py:646`, `app/ponto/services/ausencia.py:673`, `app/ponto/services/ausencia.py:696`.) |
| Leitores H12 divergentes | **Fechado no código.** Exportação/lista usam o mesmo predicado; contador e HTML leem `rescisao`. (`app/folha/export.py:60`, `app/folha/export.py:875`; `app/ponto/services/fechamento_lista.py:109`; `app/ponto/views.py:1215`; `app/templates/ponto/_fech_linha.html:2`.) |
| Rollback na segunda reposição | **Fechado como caso estático.** O teste executa a primeira reposição real e falha na segunda. Resultado executado: **[nao verificado]**. (`app/ponto/tests/test_h06_reversao_guardas.py:218`; `app/ponto/services/ausencia.py:646`, `app/ponto/services/ausencia.py:725`.) |
| Parcialidade 480 versus financeiro | **Aberto, preexistente.** Cobertura compara com 480; leitores financeiros ainda usam presença de minutos. (`app/ponto/turnos.py:2580`, `app/ponto/turnos.py:2598`; `app/ponto/services/fechamento.py:665`, `app/ponto/services/fechamento.py:685`.) |
| Cadastro atual decidindo passado | **Aberto, separado no aval.** Corte vem do cadastro atual. (`app/ponto/janelas.py:50`; `app/docs/PROMPTS.md:1861`.) |
| Arquivos antigos | **Aberto como compatibilidade.** Formato antigo em lista continua recusado; existência de arquivos afetados: **[nao verificado]**. (`app/ponto/services/ausencia.py:633`.) |
| Status/data_fim/contador/travas | **Parcial.** Os casos existentes permanecem; a nova sonda fecha a falsa proteção de A, mas não prova a linha real travada por `registrar`. (`app/ponto/tests/test_h06_reversao_guardas.py:326`, `app/ponto/tests/test_h06_reversao_guardas.py:343`, `app/ponto/tests/test_h06_reversao_guardas.py:388`, `app/ponto/tests/test_h06_reversao_guardas.py:557`, `app/ponto/tests/test_h06_reversao_guardas.py:867`, `app/ponto/tests/test_h06_reversao_guardas.py:251`.) |

O protocolo de hash continua dependendo da trava da Empresa: consultar aplicado/consumido e lavrar a ação ocorre dentro dela; não há unicidade própria do hash no modelo de auditoria. Reuso é recusado com motivo, inclusive R1 após reversão e reaplicação por R2. (`app/ponto/services/ausencia.py:580`, `app/ponto/services/ausencia.py:647`, `app/ponto/services/ausencia.py:676`, `app/ponto/services/ausencia.py:685`, `app/ponto/services/ausencia.py:699`; `app/core/models.py:309`; `app/ponto/tests/test_h06_reversao_guardas.py:454`.)

**2. Efeito da mudança das travas**

Localizei dois chamadores de `travar_entrega`: `ExportacaoDominio.registrar` e a porta H06. Ambos entram em `atomic` antes da chamada. (`app/folha/models.py:179`, `app/folha/models.py:181`; `app/ponto/services/ausencia.py:646`, `app/ponto/services/ausencia.py:647`.)

| Escritor/par | Efeito |
|---|---|
| `registrar` × porta; porta × porta | **Continuam serializados na Empresa.** Ambos pedem `NO KEY UPDATE`, modos conflitantes entre si. (`app/folha/export.py:564`, `app/folha/export.py:570`; `app/folha/models.py:181`; `app/ponto/services/ausencia.py:647`.) |
| `invalidar` | Não chama `travar_entrega`; atualiza invalidação/motivo/substituta da ED existente. Continua podendo tornar K6 falso, mas não cria nem desinvalida uma entrega. Não ganhou serialização na Empresa. (`app/folha/models.py:143`, `app/folha/models.py:154`, `app/folha/models.py:162`; `app/folha/export.py:598`, `app/folha/export.py:600`.) |
| Admissão | Mantém Cmax `FOR UPDATE` para matrícula e INSERT de Cnovo dentro do atomic externo. A FK de Empresa passa a avançar durante a trava E da porta/entrega; disputa pelo próprio Cmax continua se ele for alvo H06. (`app/colaboradores/views.py:1175`, `app/colaboradores/views.py:1182`; `app/colaboradores/models.py:321`, `app/colaboradores/models.py:431`, `app/colaboradores/models.py:439`; `app/ponto/services/ausencia.py:651`.) |
| Transferência/edição cadastral | O UPDATE da mesma C continua conflitando com a porta. A checagem da FK da Empresa destino deixa de esperar a trava inicial E, eliminando a aresta do ciclo apresentado na v5. (`app/colaboradores/views.py:1340`, `app/colaboradores/views.py:1342`, `app/colaboradores/views.py:1384`, `app/colaboradores/views.py:1386`, `app/colaboradores/views.py:1388`; `app/folha/export.py:570`; `app/ponto/services/ausencia.py:651`.) |
| `ChamadoColaborador.abrir` | **Continua serializado com a porta pela mesma C.** Seu `FOR UPDATE` explícito permanece antes do dedup/nascimento. (`app/chamados/models.py:863`, `app/chamados/models.py:866`, `app/chamados/models.py:867`, `app/chamados/models.py:932`; `app/ponto/services/ausencia.py:651`.) |
| INSERT que apenas referencia C | Deixa de esperar a trava inicial C da porta pela FK. Isso não libera `abrir`, que pede uma trava explícita antes do INSERT. (`app/ponto/services/ausencia.py:649`, `app/ponto/services/ausencia.py:651`; `app/ponto/models.py:743`; `app/chamados/models.py:866`.) |
| CelulaDia → FK Ausencia | A checagem da FK passa durante a trava inicial A. O ciclo que dependia exclusivamente de célula segurando `KEY SHARE A` perde essa aresta; UPDATE da mesma célula continua sendo disputa de escrita. (`app/ponto/services/ausencia.py:654`; `app/ponto/portas/celula.py:648`, `app/ponto/portas/celula.py:810`; `app/ponto/tests/test_h06_reversao_guardas.py:877`.) |
| Edição simples de Ausencia pela UI | Continua disputando o UPDATE de A. A view e o serviço não abrem `atomic` próprio; sob autocommit, não se pode presumir retenção da trava A durante todo o signal. Transação efetiva em produção: **[nao verificado]**. (`app/ponto/views.py:2748`, `app/ponto/views.py:2791`; `app/ponto/services/ausencia.py:465`, `app/ponto/services/ausencia.py:508`; `app/config/settings/saas.py:83`.) |
| Signals da Ausencia | Continuam síncronos para o cartório. Se houver emissão, alcançam `abrir`, readquirindo C em **FOR UPDATE**. Portanto, “as três linhas ficam NO KEY durante todo o ato” não está demonstrado pela foto anterior ao save. (`app/ponto/signals.py:143`, `app/ponto/signals.py:147`; `app/ponto/services/cartorio.py:1778`; `app/ponto/management/commands/detectar_ausencias.py:693`; `app/chamados/models.py:866`; `app/ponto/tests/test_h06_reversao_guardas.py:819`.) |

**Sim, pares antes serializados incidentalmente deixaram de serializar:** trava E × FK E da admissão/transferência; trava C da porta × INSERT que apenas referencia C; trava A da porta × FK A da célula. Essa liberação é precisamente o efeito pretendido de `no_key=True`, não preservação universal da serialização anterior. (`app/folha/export.py:564`; `app/ponto/services/ausencia.py:649`; `app/ponto/tests/test_h06_reversao_guardas.py:877`.)

**Permanece um ciclo possível nas mesmas linhas, independente de FK**, neste contraexemplo estático:

1. Existe R1 de uma A anteriormente falta/aprovada. Depois, pelas portas de produção, o DP pode editar a mesma A para um tipo parcial, cancelar e reverter a rejeição com documento; a reabertura preserva o PK e pode produzir `aguardando_decisao`. (`app/ponto/services/ausencia.py:497`, `app/ponto/services/ausencia.py:507`, `app/ponto/services/ausencia.py:994`; `app/ponto/views.py:2731`; `app/ponto/services/ausencia.py:1305`, `app/ponto/services/ausencia.py:1312`, `app/ponto/services/ausencia.py:1322`, `app/ponto/services/ausencia.py:1344`.)
2. A aprovação UI entra em `atomic`, atualiza essa A e mantém sua trava. Com célula existente, cobertura parcial, resto faltante e ramo emissor habilitado, o signal pode pedir C `FOR UPDATE` por `abrir`. (`app/ponto/views.py:2638`, `app/ponto/views.py:2640`; `app/ponto/services/ausencia.py:1038`; `app/ponto/signals.py:147`; `app/ponto/services/cartorio.py:1762`, `app/ponto/services/cartorio.py:1777`; `app/chamados/models.py:866`.)
3. Intercalada entre o UPDATE A e o pedido C, a reversão obsoleta obtém E e C e espera **a mesma A**. A UI segura A e espera **a mesma C**. A guarda de identidade só vem depois da obtenção de A. (`app/ponto/services/ausencia.py:647`, `app/ponto/services/ausencia.py:651`, `app/ponto/services/ausencia.py:654`, `app/ponto/services/ausencia.py:664`.)

É **C→A contra A→C**; `NO KEY UPDATE` continua conflitando com esses locks de escrita. Abortar um participante não elimina a inversão. Existência dessa preparação e execução do deadlock em produção: **[nao verificado]**. (`app/ponto/services/ausencia.py:651`, `app/ponto/services/ausencia.py:654`; `app/ponto/services/ausencia.py:1038`; `app/chamados/models.py:866`.)

**3. Porta única e arquivo**

Entrega, identidade, empresa atual, minutos esperados e consumo estão centralizados nos caminhos atuais: aplicar e reverter chamam a mesma função; as decisões ficam nela. O comando não mantém cópia dessas guardas. (`app/ponto/management/commands/rederivar_falta_declarada.py:122`, `app/ponto/management/commands/rederivar_falta_declarada.py:144`; `app/ponto/services/ausencia.py:657`, `app/ponto/services/ausencia.py:665`, `app/ponto/services/ausencia.py:668`, `app/ponto/services/ausencia.py:670`, `app/ponto/services/ausencia.py:681`.)

A unicidade literal permanece incompleta: motivo é exigido pela porta e pela reposição. `repor_minutos_de_reversao` também continua gravável diretamente com motivo, embora seu único chamador de produção localizado seja a porta. Não encontrei segundo caminho atual de produção usando essa possibilidade. (`app/ponto/services/ausencia.py:528`, `app/ponto/services/ausencia.py:532`, `app/ponto/services/ausencia.py:617`, `app/ponto/services/ausencia.py:725`.)

Sobre o arquivo:

- **Arquivo preexistente:** `xb` falha antes de adicioná-lo à lista de limpeza; esse caminho não apaga a sentinela de outro ato. (`app/ponto/services/ausencia.py:696`, `app/ponto/services/ausencia.py:698`; `app/ponto/tests/test_h06_reversao_guardas.py:236`.)
- **Falha em `write`/fechamento:** pode deixar arquivo criado, pois `escrito.append` ainda não ocorreu. O teste novo provoca falha da lavra depois da criação completa, não esse caso. (`app/ponto/services/ausencia.py:696`, `app/ponto/services/ausencia.py:697`, `app/ponto/services/ausencia.py:698`; `app/ponto/tests/test_h06_reversao_guardas.py:581`.)
- **Arquivo substituído no pathname:** a limpeza guarda somente o caminho, sem conferir identidade do arquivo antes de `unlink`. Se houver substituição externa, pode apagar o substituto; nenhum escritor atual que faça essa substituição foi demonstrado. Ocorrência: **[nao verificado]**. (`app/ponto/services/ausencia.py:698`, `app/ponto/services/ausencia.py:732`.)
- **Exceção depois do commit:** o `except BaseException` engloba a saída do `atomic`. O save registra `on_commit`; uma interrupção como `KeyboardInterrupt` nesse callback não é capturada pelo `engolir`, que captura somente `Exception`. Nesse contraexemplo, o banco já confirmou e a porta ainda apaga a reversão. Execução: **[nao verificado]**. (`app/ponto/services/ausencia.py:646`, `app/ponto/services/ausencia.py:730`; `app/ponto/signals.py:254`, `app/ponto/signals.py:260`, `app/ponto/signals.py:272`; `app/core/observ.py:90`.)
- **Atomic externo:** a função não exige transação externa final. Se retornar após um savepoint e o chamador depois desfizer a transação externa, o arquivo fica fora desse rollback. Os chamadores CLI atuais não abrem esse atomic externo. (`app/ponto/services/ausencia.py:646`, `app/ponto/services/ausencia.py:734`; `app/ponto/management/commands/rederivar_falta_declarada.py:93`, `app/ponto/management/commands/rederivar_falta_declarada.py:122`.)

Na saída normal, o arquivo de ato confirmado permanece, como necessário para a reversão. O defeito é tratar toda exceção na saída do `atomic` como prova de que o ato não confirmou. (`app/ponto/services/ausencia.py:730`, `app/ponto/services/ausencia.py:734`.)

**4. Quem escreve e quem lê**

| Campo/valor | Quem produz/escreve | Quem lê |
|---|---|---|
| `Ausencia.minutos` | Criação recebe o valor; edição atribui número ou zero; `Ausencia.save` soma o previsto; transição persiste a derivação; reversão repõe o anterior. (`app/ponto/services/ausencia.py:343`, `app/ponto/services/ausencia.py:507`, `app/ponto/services/ausencia.py:1037`, `app/ponto/services/ausencia.py:531`; `app/ponto/models.py:877`, `app/ponto/models.py:885`, `app/ponto/models.py:887`.) | Censo, guarda e pós-checagem; rateio; financeiro/crédito/efeitos; cobertura e apresentação. (`app/ponto/management/commands/rederivar_falta_declarada.py:56`, `app/ponto/management/commands/rederivar_falta_declarada.py:69`; `app/ponto/services/ausencia.py:668`, `app/ponto/services/ausencia.py:714`; `app/folha/export.py:121`; `app/ponto/services/fechamento.py:665`, `app/ponto/services/fechamento.py:685`; `app/ponto/services/credito_parcial.py:24`; `app/ponto/services/efeito_ausencia.py:44`; `app/ponto/turnos.py:2600`; `app/templates/ponto/ausencias.html:163`.) |
| `Ausencia.tipo` | Materializadores fornecem tipo; criação e edição gravam. (`app/chamados/services/materializacao.py:468`, `app/chamados/services/materializacao.py:485`, `app/chamados/services/materializacao.py:521`; `app/ponto/services/ausencia.py:342`, `app/ponto/services/ausencia.py:497`.) | Censo, identidade, derivação, catálogo/efeito e cobertura parcial. (`app/ponto/management/commands/rederivar_falta_declarada.py:55`; 

[truncado em 20000 de 35057 caracteres]

## 05/11 19:09 · sessao 9e2282fbfa3a · C20-JUIZ-3 · verificador · origem=Code

**O diff conecta os dois produtores ao consumidor existente, sem demonstrar juiz ou escritor novo; porém os testes deixam passar a perda da atomicidade própria e do tratamento de falhas.** Há também uma promessa incorreta sobre a atomicidade da trilha. (`app/chamados/models.py:1053`, `app/ponto/nucleo.py:57`, `app/chamados/reconciliador.py:1360`, `app/chamados/tests/test_c20_juiz3_outbox.py:125`, `app/core/observ.py:105`)

Referências abaixo são da cópia fornecida. Identidade Git, ancestralidade, execução da suíte, execução dos mutantes e ocorrência produtiva dos cenários: **[nao verificado]**. [nao verificado]

**1. Quem escreve e quem lê**

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `EventoCelula.celula_id` | `emitir_evento_celula`, copiando `cel.pk`. (`app/ponto/portas/celula.py:643`) | Vigia de absolvição e semeadura procuram pendente por célula. (`app/chamados/reconciliador.py:1262`, `app/chamados/reconciliador.py:1284`, `app/chamados/reconciliador.py:1297`) |
| `EventoCelula.colaborador_id` | Mesma porta, copiando `cel.colaborador_id`. (`app/ponto/portas/celula.py:643`) | Consumidor agrupa/carrega colaborador; vigia constrói `_com_evento`. (`app/chamados/reconciliador.py:1338`, `app/chamados/reconciliador.py:1344`, `app/chamados/management/commands/reconciliar_chamados.py:62`) |
| `EventoCelula.data` | Mesma porta, copiando `cel.data`. (`app/ponto/portas/celula.py:643`) | O consumidor **não restringe o trabalho à data do evento**: percorre o colaborador inteiro. Outro leitor produtivo específico desse campo: **[nao verificado]**. (`app/chamados/reconciliador.py:1338`, `app/chamados/reconciliador.py:1432`) |
| `processado_em` | Nasce nulo; consumidor grava `agora` no sucesso. (`app/escala/models.py:1859`, `app/chamados/reconciliador.py:1380`) | Elegibilidade, contador de mortos, proteção do UPDATE e retenção. (`app/chamados/reconciliador.py:1238`, `app/chamados/reconciliador.py:1244`, `app/chamados/reconciliador.py:1372`, `app/chamados/reconciliador.py:1387`) |
| `tentativas` | Default zero; consumidor incrementa com `F` na falha. (`app/escala/models.py:1860`, `app/chamados/reconciliador.py:1374`) | Separação entre pendentes vivos e mortos. (`app/chamados/reconciliador.py:1238`, `app/chamados/reconciliador.py:1244`) |
| `erro` | Default vazio; consumidor grava falhas concatenadas, limitadas a 500 caracteres. Leitor produtivo específico: **[nao verificado]**. (`app/escala/models.py:1861`, `app/chamados/reconciliador.py:1374`) | **[nao verificado]** |
| `TurnoMaterializado` | Núcleo monta linhas, apaga recorte e faz `bulk_create`. (`app/ponto/nucleo.py:48`, `app/ponto/nucleo.py:58`, `app/ponto/nucleo.py:59`) | O novo `RETURNING` lê dia/entrada/saída; a diferença entre identidades abertas antigas e novas determina emissão. (`app/ponto/nucleo.py:64`, `app/ponto/nucleo.py:79`, `app/ponto/nucleo.py:80`) |
| `ChamadoColaborador.status_local` | Nascimento por `abrir`/`criar`; transições por `_gravar_estado`. O consumidor chega pela sequência `_retratar → reconciliar → _gravar_estado`. (`app/chamados/models.py:932`, `app/chamados/models.py:1054`, `app/chamados/models.py:222`, `app/chamados/reconciliador.py:523`, `app/chamados/models.py:620`) | Seleção de vivos, motor de transição, signals e painel. (`app/chamados/reconciliador.py:1433`, `app/chamados/models.py:639`, `app/chamados/signals.py:70`, `app/chamados/services/painel.py:211`) |
| `LogAuditoria/fechamento_recusado` | Gates de premissa/pergunta chamam `_registrar_recusa`; este deduplica e delega o INSERT a `core.observ.evento`. (`app/chamados/models.py:730`, `app/chamados/models.py:408`, `app/chamados/models.py:386`, `app/chamados/models.py:391`, `app/core/observ.py:134`) | Próprio deduplicador, consulta genérica de auditoria e testes novos. (`app/chamados/models.py:386`, `app/core/views.py:705`, `app/chamados/tests/test_c20_juiz3_outbox.py:239`) |
| Retorno `premissa` | Inicializado em zero, incrementado pelo retorno da função extraída e devolvido no dicionário. (`app/chamados/reconciliador.py:1327`, `app/chamados/reconciliador.py:1360`, `app/chamados/reconciliador.py:1388`) | Testes leem; **o command consumidor não imprime essa chave**, embora imprima `retratados`. Outro leitor produtivo: **[nao verificado]**. (`app/chamados/tests/test_c20_juiz3_outbox.py:254`, `app/chamados/management/commands/consumir_eventos_celula.py:32`) |

Os chamadores do INSERT são `salvar_e_emitir`, nascimento por `abrir`, nascimento por `criar`, recompute e semeadura; todos delegam à mesma porta. Os dois novos produtores não inserem `EventoCelula` diretamente. (`app/ponto/portas/celula.py:654`, `app/chamados/models.py:941`, `app/chamados/models.py:1058`, `app/ponto/nucleo.py:90`, `app/chamados/reconciliador.py:1286`)

**Não identifiquei escritor explícito novo fora da porta no caminho alterado:** turno permanece em `ponto/nucleo.py`, e estado do chamado em `chamados/models.py`, ambos autorizados. `EventoCelula` e `LogAuditoria` não têm entrada própria em `PORTAS`; seus donos estão declarados no modelo do evento e em `core.observ.evento`. A manutenção de processamento pelo consumidor escreve campos distintos dos identificadores inseridos pela porta. (`app/core/portas.py:117`, `app/core/portas.py:233`, `app/core/portas.py:38`, `app/escala/models.py:1851`, `app/core/observ.py:96`, `app/chamados/reconciliador.py:1374`)

**2. Equivalência do DELETE e outros escritores**

Para colaborador persistido, modelo e configuração presentes, **o conjunto apagado é o mesmo**: colaborador por PK e intervalo inclusivo de `data_turno`. O `RETURNING` não limita a exclusão aos abertos: apaga todo o recorte e depois seleciona, no resultado, entrada presente/saída ausente. (`app/ponto/nucleo.py:78`, `app/ponto/nucleo.py:80`; filtro anterior no diff fornecido)

A classe atual é simples, sem manager ou override de exclusão; suas FKs são **saintes**, para colaborador e batidas. Elas não significam apagar colaborador/batidas ao excluir o turno. Dependente entrante ou receiver específico de exclusão de `TurnoMaterializado` não foi localizado; cobertura de registros dinâmicos externos: **[nao verificado]**. (`app/ponto/models.py:1196`, `app/ponto/models.py:1201`, `app/ponto/models.py:1204`, `app/ponto/models.py:1206`)

Logo, **não há perda concreta de signal/cascata de saída demonstrada nesta cópia**, mas o SQL não conserva genericamente o mecanismo de exclusão do ORM: usa diretamente a conexão padrão e executa o DELETE. Equivalência sob outro roteador/alias ou dependentes adicionados depois: **[nao verificado]**. (`app/ponto/nucleo.py:74`, `app/ponto/nucleo.py:76`, `app/ponto/nucleo.py:78`)

Além do núcleo, existe exclusão indireta por cascata ao apagar colaborador ou batida. A porta de batida chama `.delete()`, e seu receiver recompõe depois da exclusão; portanto um turno já eliminado pela cascata pode não estar no `RETURNING`. Esse detector não garante evento para tal caminho. (`app/ponto/models.py:1202`, `app/ponto/models.py:1205`, `app/ponto/models.py:1207`, `app/ponto/registro_batida.py:291`, `app/ponto/signals.py:48`, `app/ponto/nucleo.py:80`)

**Há perda de cobertura do censo:** o novo `cur.execute` fica invisível ao reconhecedor de escritas ORM usado pelo gate de turno. Isso não cria outro escritor, mas enfraquece a capacidade do selo de enxergar esse DELETE. (`app/ponto/nucleo.py:77`, `app/core/censo_escritas.py:364`, `app/core/censo_escritas.py:377`, `app/ponto/tests/test_chokepoint_turno_gate.py:35`)

**3. Atomicidade e chamadores de recompute**

**Sim:** `criar` envolve chamado e evento no mesmo `atomic(savepoint=False)`; recompute envolve DELETE, recriação e emissão no mesmo `atomic`. Não aparece nesses blocos caminho normal de commit do evento elegível com rollback do respectivo chamado/materializado, ou o inverso. (`app/chamados/models.py:1053`, `app/chamados/models.py:1058`, `app/ponto/nucleo.py:57`, `app/ponto/nucleo.py:66`)

Existem estados sem evento por desenho: módulo fora da lista, chamado sem célula, recompute sem identidade aberta removida e fechamento em dia sem célula. Portanto a cobertura não é “qualquer nascimento/qualquer recompute”. (`app/chamados/models.py:1056`, `app/ponto/nucleo.py:64`, `app/ponto/nucleo.py:89`)

A garantia também não engloba necessariamente a **escrita de origem**: batida/vínculo podem persistir enquanto recompute e evento falham, pois os receivers engolem a exceção; flip salva a alteração antes da recomposição. Isso não separa evento de materializado dentro do atomic do núcleo. (`app/ponto/signals.py:29`, `app/ponto/signals.py:67`, `app/ponto/services/flip_batida.py:30`, `app/ponto/services/flip_batida.py:39`)

Chamadores produtivos encontrados por busca textual — execução de `git grep`/completude histórica: **[nao verificado]**:

| Chamador | Atomic externo local |
|---|---|
| Save/delete de batida, via `_recompute_around` | Não abre atomic no wrapper. (`app/ponto/signals.py:19`, `app/ponto/signals.py:26`, `app/ponto/signals.py:48`) |
| Save de vínculo, via `_recompute_escala` | Não abre atomic no wrapper. (`app/ponto/signals.py:53`, `app/ponto/signals.py:64`) |
| Alertas de turno | Sem atomic externo local. (`app/ponto/management/commands/processar_alertas_turno.py:32`, `app/ponto/management/commands/processar_alertas_turno.py:67`) |
| Alertas avançados | Sem atomic externo local. (`app/ponto/management/commands/processar_alertas_avancados.py:19`, `app/ponto/management/commands/processar_alertas_avancados.py:57`) |
| Materialização explícita | Sem atomic externo local. (`app/ponto/management/commands/materializar_turnos.py:16`, `app/ponto/management/commands/materializar_turnos.py:24`) |
| Flip | Sem atomic externo local. (`app/ponto/services/flip_batida.py:10`, `app/ponto/services/flip_batida.py:39`) |
| Veredito humano | Dentro do atomic iniciado antes. (`app/ponto/services/veredito_celula.py:204`, `app/ponto/services/veredito_celula.py:308`) |

**A prosa nova promete mais que o código:** “create, trilha e INSERT juntos ou nenhum” não vale para falha da trilha, porque `evento()` tem savepoint próprio e engole a exceção. Chamado e evento podem persistir sem a trilha de nascimento. Também é incorreto dizer que as guardas anteriores “não escrevem nada”: recusas/vetos podem gravar auditoria antes do create. (`app/chamados/models.py:1050`, `app/chamados/models.py:1175`, `app/core/observ.py:105`, `app/core/observ.py:133`, `app/chamados/models.py:1034`, `app/chamados/models.py:1038`)

**4. Classes de reprovação**

| Classe | Incorre? |
|---|---|
| Segundo juiz duplicando autoridade | **Não demonstrado.** O produtor detecta mudança no materializado; o consumidor continua perguntando `_veredito_premissa`, que delega ao catálogo registrado. (`app/ponto/nucleo.py:64`, `app/chamados/reconciliador.py:1437`, `app/chamados/reconciliador.py:301`, `app/core/juizes.py:263`) |
| Segundo escritor fora da porta | **Não demonstrado no diff.** Novas emissões delegam à porta existente; transição continua no gate do chamado. (`app/ponto/nucleo.py:90`, `app/chamados/models.py:1058`, `app/chamados/models.py:620`) |
| Relógio fora da borda | **Nenhuma leitura nova demonstrada.** Os trechos acrescentados trabalham com estado/dias recebidos; o relógio do consumidor já é capturado na entrada. (`app/ponto/nucleo.py:64`, `app/chamados/reconciliador.py:1325`, `app/chamados/reconciliador.py:1437`) |
| Derivação paralela de fonte única | **Não demonstrada para a decisão da premissa.** A diferença de conjuntos produz notificação; não substitui o julgamento existente. (`app/ponto/nucleo.py:64`, `app/chamados/reconciliador.py:1360`, `app/chamados/reconciliador.py:1437`) |
| Selo/RED sem mordida | **Parcialmente sim:** atomic próprio e propagação de falhas têm mutantes sobreviventes previstos; DELETE cru sai do alcance do censo. (`app/chamados/tests/test_c20_juiz3_outbox.py:125`, `app/chamados/tests/test_c20_juiz3_outbox.py:196`, `app/chamados/reconciliador.py:1447`, `app/core/censo_escritas.py:377`) |
| Ordem de trava | Não há nova trava explícita de colaborador/disputa no consumidor; o fechamento trava o chamado pela porta existente. Deadlock efetivo: **[nao verificado]**. (`app/chamados/reconciliador.py:1311`, `app/chamados/models.py:668`) |
| Pouso antes de pré-requisito | **[nao verificado]**: o material entregue não prova sequência de pousos, suíte ou leitura final; essas condições estão documentadas. (`app/docs/PISTA.md:108`, `app/docs/PISTA.md:113`) |

**5. Extração e `tranca_em_lote`**

No caminho antigo, **a extração preserva ordem, universo, decisão, exceção e retorno**: perguntas absolvidas, órfãs, premissa; vivos do colaborador ordenados por PK; mesmo juiz, mesma retratação; exceção logada/ignorada; soma final mantida. (`app/chamados/reconciliador.py:1411`, `app/chamados/reconciliador.py:1420`, `app/chamados/reconciliador.py:1432`, `app/chamados/reconciliador.py:1443`, `app/chamados/reconciliador.py:1445`; comparação com o diff fornecido)

O consumidor acrescenta deliberadamente `falhas`: exceções por chamado entram na lista e impedem marcar o lote como processado. Um `False` por retenção ou gate **não é exceção** e não impede processamento do evento. (`app/chamados/reconciliador.py:1360`, `app/chamados/reconciliador.py:1447`, `app/chamados/reconciliador.py:1373`, `app/chamados/reconciliador.py:518`, `app/chamados/reconciliador.py:523`)

**Sim, chama fora de `tranca_em_lote`.** Esse contexto é memo em `ContextVar`, não lock SQL, e está expressamente reservado a leitor que não escreve trilha; o consumidor executa escritas. (`app/chamados/reconciliador.py:1360`, `app/chamados/juizes.py:990`, `app/chamados/juizes.py:994`)

**6. Janela criada pelo skip do vigia**

- **Tentativas esgotadas:** não mascaram por si: saem de `eventos_pendentes`; são contadas como eventos mortos e entram no alarme. (`app/chamados/reconciliador.py:1238`, `app/chamados/reconciliador.py:1244`, `app/chamados/management/commands/reconciliar_chamados.py:118`)
- **Processado sem retratar:** sem outro pendente do colaborador, volta ao julgamento do vigia. Recusa/retenção legítima fica como MEDIDA. (`app/chamados/management/commands/reconciliar_chamados.py:62`, `app/chamados/management/commands/reconciliar_chamados.py:73`, `app/chamados/management/commands/reconciliar_chamados.py:79`)
- **Evento de outro dia:** exclui o colaborador inteiro da contagem, mas o consumidor também julga todos os seus vivos; o dia diferente, sozinho, não prova abandono. (`app/chamados/management/commands/reconciliar_chamados.py:73`, `app/chamados/reconciliador.py:1338`, `app/chamados/reconciliador.py:1432`)
- **Há uma janela por fotografia:** `_com_evento` é calculado antes do laço; consumo posterior não atualiza esse conjunto nesta execução. Ocorrência/duração produtiva: **[nao verificado]**. (`app/chamados/management/commands/reconciliar_chamados.py:62`, `app/chamados/management/commands/reconciliar_chamados.py:71`)
- **Há mascaramento prolongável:** órfã adiada mantém evento pendente sem gastar tentativas; enquanto existir, exclui também a premissa do contador. O consumidor ainda tenta julgá-la, mas idade/quantidade de pendentes são somente MEDIDA, fora da soma de alarme. Isso não prova julgamento bem-sucedido. (`app/chamados/reconciliador.py:1360`, `app/chamados/reconciliador.py:1375`, `app/chamados/management/commands/reconciliar_chamados.py:113`, `app/chamados/management/commands/reconciliar_chamados.py:118`)

**7. Os testes mordem? Mutantes** [nao verificado]

**Mordem comportamento real:** emissão, exclusão de módulo estranho, fechamento/repetição/preservação do aberto, pai retratado, recusa do furo e reentrega idempotente. (`app/chamados/tests/test_c20_juiz3_outbox.py:99`, `app/chamados/tests/test_c20_juiz3_outbox.py:109`, `app/chamados/tests/test_c20_juiz3_outbox.py:135`, `app/chamados/tests/test_c20_juiz3_outbox.py:146`, `app/chamados/tests/test_c20_juiz3_outbox.py:155`, `app/chamados/tests/test_c20_juiz3_outbox.py:242`, `app/chamados/tests/test_c20_juiz3_outbox.py:271`)

Dois mutantes relevantes têm sobrevivência prevista por inspeção; execução: **[nao verificado]**:

1. **Retirar `falhas=falhas` da chamada nova.** Caminhos felizes permanecem; exceção por chamado é engolida sem avisar o consumidor, que pode marcar o evento processado. Os erros injetados pelos testes novos são no INSERT do evento, não nesse julgamento. (`app/chamados/reconciliador.py:1360`, `app/chamados/reconciliador.py:1445`, `app/chamados/reconciliador.py:1380`, `app/chamados/tests/test_c20_juiz3_outbox.py:123`, `app/chamados/tests/test_c20_juiz3_outbox.py:194`)
2. **Retirar os atomics próprios de `criar`/recompute, reindentando os corpos.** A base usa `TestCase`, e os dois testes de falha ainda fornecem atomic externo. Assim, não distinguem proteção própria de proteção do chamador; em autocommit, o mutante permitiria estado persistido antes da falha do evento. (`app/chamados/models.py:1053`, `app/ponto/nucleo.py:57`, `app/chamados/tests/test_c20_juiz3_outbox.py:37`, `app/chamados/tests/test_c20_juiz3_outbox.py:125`, `app/chamados/tests/test_c20_juiz3_outbox.py:196`)

**8. Contradição com aval/PROIBIDOS**

Não aparece no diff fornecido nova reclassificação, alteração da nota/listas do contrato, juiz novo ou fallback novo; o caminho principal realiza evento em `criar`/recompute e consumo pelo O-OUTBOX existente. Isso **não equivale a certificar cobertura completa ou fechamento do contrato**. (`app/chamados/models.py:1053`, `app/ponto/nucleo.py:57`, `app/chamados/reconciliador.py:1360`, `app/core/contratos_estruturais.py:263`, `app/core/contratos_estruturais.py:269`)

**Ressalva do “selo 17”:** a cópia já contém teto **18** para `_gravar_estado (sem pergunta)`, enquanto a prosa diz 17. O arquivo desse teto não está no diff anexado; portanto não atribuo essa subida à obra. Origem histórica e conformidade desse 18 com o aval literal: **[nao verificado]**. (`app/ponto/tests/test_selo_chokepoint_escrita.py:116`, `app/docs/RELATO.md:25`)

Os pontos concretos que impedem um aceite sem ressalvas são a prova insuficiente de atomicidade/retry, a exclusão do DELETE do alcance do censo e a promessa falsa sobre a trilha; o novo contador `premissa` também fica ausente da saída produtiva do command. (`app/chamados/tests/test_c20_juiz3_outbox.py:125`, `app/chamados/reconciliador.py:1447`, `app/core/censo_escritas.py:377`, `app/core/observ.py:105`, `app/chamados/management/commands/consumir_eventos_celula.py:32`)

## 05/11 19:07 · sessao b8013929f9c6 · - · - · origem=Ronald (tmux)

O contraexemplo de **reversão obsoleta × aprovação UI** fecha a lacuna das mesmas linhas, como inferência estática:

1. R1 foi emitido para A falta/aprovada; posteriormente o DP edita **a mesma A** para um tipo parcial e minutos abaixo de 480, cancela e reverte a rejeição com documento. A reabertura preserva o PK e usa `estado_inicial`, que com documento devolve `aguardando_decisao`. A edição de tipo da aprovada, isoladamente, **não** a reabre, pois `rejulgar_status_pelo_tipo` só atua em ABERTAS. (`app/ponto/services/ausencia.py:497`, `app/ponto/services/ausencia.py:507`, `app/ponto/services/ausencia.py:744`, `app/ponto/services/ausencia.py:994`; `app/ponto/views.py:2725`, `app/ponto/views.py:2731`; `app/ponto/services/ausencia.py:1297`, `app/ponto/services/ausencia.py:1305`, `app/ponto/services/ausencia.py:1312`, `app/ponto/services/ausencia.py:1322`, `app/ponto/services/ausencia.py:1344`.)

2. A aprovação UI abre atomic, atualiza A e mantém essa trava durante o signal. Com célula existente, tipo parcial vigente, cobertura parcial com resto faltante, data no horizonte e sem chamado vivo, o cartório pode emitir o chamado do resto e pedir **FOR UPDATE C**. (`app/ponto/views.py:2638`, `app/ponto/views.py:2640`; `app/ponto/services/ausencia.py:1038`; `app/ponto/signals.py:147`; `app/ponto/turnos.py:2598`; `app/ponto/services/cartorio.py:1676`, `app/ponto/services/cartorio.py:1762`, `app/ponto/services/cartorio.py:1777`, `app/ponto/services/cartorio.py:1778`; `app/ponto/management/commands/detectar_ausencias.py:693`; `app/chamados/models.py:866`.)

3. Intercale a reversão de R1 depois do UPDATE A e antes desse pedido C: a porta obtém E e **NO KEY UPDATE C**, depois espera A. A UI segura A e espera C. A guarda de identidade da reversão só será avaliada **depois** de obter A, portanto não evita o ciclo. (`app/ponto/services/ausencia.py:647`, `app/ponto/services/ausencia.py:651`, `app/ponto/services/ausencia.py:654`, `app/ponto/services/ausencia.py:664`, `app/ponto/services/ausencia.py:665`.)

Logo permanece um ciclo de travas explícitas **porta C→A × aprovação UI A→C**, com o mesmo C e o mesmo PK A; a troca `FOR UPDATE`→`NO KEY UPDATE` não elimina conflito com o `FOR UPDATE C` do gate de chamados nem com o UPDATE A. O arquivo obsoleto é suficiente; não depende de H06 censar uma Ausencia aberta. As condições descritas e a execução do deadlock em produção são [nao verificado]. (`app/ponto/services/ausencia.py:651`, `app/ponto/services/ausencia.py:654`; `app/chamados/models.py:866`; `app/ponto/views.py:2638`; `app/ponto/services/ausencia.py:1038`.)

## 05/11 19:06 · sessao b8013929f9c6 · - · - · origem=Ronald (tmux)

Censo em `app/**/*.py`: os únicos chamadores de `travar_entrega` são `ExportacaoDominio.registrar` e `porta_do_ato_h06`; ambos abrem `atomic` antes da chamada. (`app/folha/models.py:179`, `app/folha/models.py:181`; `app/ponto/services/ausencia.py:646`, `app/ponto/services/ausencia.py:647`.) Não apareceu outro comando chamando essa função na pesquisa; exaustividade além da cópia: [nao verificado].

| Caminho | Efeito da v6 |
|---|---|
| `registrar` × porta; porta × porta | Continuam serializados pela mesma Empresa em `NO KEY UPDATE`; Colaborador e Ausencia também passam a `NO KEY UPDATE`. (`app/folha/export.py:570`; `app/folha/models.py:181`; `app/ponto/services/ausencia.py:647`, `app/ponto/services/ausencia.py:651`, `app/ponto/services/ausencia.py:654`.) |
| `registrar` × admissão/transferência para a Empresa | A proteção anteriormente incidental por conflito com a checagem FK deixa de existir: o modo novo admite `KEY SHARE`. O teste novo exige justamente isso. (`app/folha/export.py:564`, `app/folha/export.py:570`; `app/ponto/tests/test_h06_reversao_guardas.py:885`.) |
| Admissão | Mantém atomic externo, trava Cmax para matrícula e depois insere Cnovo com FK Empresa. A mudança em E elimina a espera da FK de E contra a porta; a disputa pelo próprio Cmax continua, se for alvo da porta. (`app/colaboradores/views.py:1175`, `app/colaboradores/views.py:1182`; `app/colaboradores/models.py:321`, `app/colaboradores/models.py:431`, `app/colaboradores/models.py:439`; `app/ponto/services/ausencia.py:651`.) |
| Transferência/edição cadastral | Mantêm UPDATE de C no atomic; a mudança de empresa referencia E destino. A porta continua disputando a mesma linha C, mas sua trava E deixa a checagem da FK avançar. (`app/colaboradores/views.py:1313`, `app/colaboradores/views.py:1340`, `app/colaboradores/views.py:1342`, `app/colaboradores/views.py:1384`, `app/colaboradores/views.py:1386`, `app/colaboradores/views.py:1388`; `app/colaboradores/services/colaborador.py:76`; `app/ponto/services/ausencia.py:651`.) |
| `ChamadoColaborador.abrir` | Continua tomando **FOR UPDATE C**, portanto continua conflitando com a trava C da porta. Não perdeu sua serialização de dedup/nascimento. (`app/chamados/models.py:863`, `app/chamados/models.py:866`, `app/chamados/models.py:867`, `app/chamados/models.py:932`; `app/ponto/services/ausencia.py:651`.) |
| INSERTs que apenas referenciam C | Deixam de ser barrados pela trava inicial C da porta; isso não equivale a liberar `abrir`, que toma trava explícita forte antes do INSERT. (`app/ponto/services/ausencia.py:649`, `app/ponto/services/ausencia.py:651`; `app/chamados/models.py:866`, `app/chamados/models.py:932`.) |
| Cartório/Celula→Ausencia | A trava inicial A da porta deixa avançar a checagem FK da célula para A; o cartório atribui a FK e salva a célula em atomic. O antigo ciclo que dependia **somente** de `KEY SHARE A` versus `FOR UPDATE A` perde essa aresta. (`app/ponto/services/ausencia.py:654`; `app/ponto/portas/celula.py:647`, `app/ponto/portas/celula.py:648`, `app/ponto/portas/celula.py:810`, `app/ponto/portas/celula.py:815`; `app/ponto/tests/test_h06_reversao_guardas.py:885`.) |
| `ExportacaoDominio.invalidar` | Não chama a trava E; atualiza apenas invalidação/motivo/substituta em ED. O juiz K6 passa a falso quando o vigente está invalidado, mas `invalidar` não cria nem desinvalida ED. A v6 não acrescentou uma nova serialização para ele. (`app/folha/models.py:143`, `app/folha/models.py:154`, `app/folha/models.py:158`, `app/folha/models.py:162`; `app/folha/export.py:598`, `app/folha/export.py:600`.) |

A alegação “a porta fica NO KEY nas três linhas durante todo o ato” é excessiva: seu save dispara `post_save`; quando o cartório alcança emissão, chama `ChamadoColaborador.abrir`, que readquire C em `FOR UPDATE`. O teste de `KEY SHARE` para na foto **anterior** à escrita e não examina esse modo posterior. Alcançabilidade efetiva desse ramo para uma falta específica de produção: [nao verificado]. (`app/ponto/services/ausencia.py:693`, `app/ponto/services/ausencia.py:711`, `app/ponto/services/ausencia.py:725`; `app/ponto/signals.py:143`, `app/ponto/signals.py:147`; `app/ponto/services/cartorio.py:1740`, `app/ponto/services/cartorio.py:1778`; `app/ponto/management/commands/detectar_ausencias.py:693`; `app/chamados/models.py:866`; `app/ponto/tests/test_h06_reversao_guardas.py:819`, `app/ponto/tests/test_h06_reversao_guardas.py:885`.)

**Correção importante à triagem sobre o ciclo UI A→C.** A view de edição simples não abre `atomic`, e o serviço também não: chama `Ausencia.save()` diretamente. O perfil SaaS da cópia não declara `ATOMIC_REQUESTS`. Sob autocommit, não se pode presumir que o UPDATE A permaneça travado durante o signal posterior; portanto o ciclo dessa view, sozinho, não está demonstrado. Transação efetiva em produção: [nao verificado]. (`app/ponto/views.py:2747`, `app/ponto/views.py:2791`; `app/ponto/services/ausencia.py:465`, `app/ponto/services/ausencia.py:508`; `app/ponto/models.py:908`; `app/config/settings/saas.py:83`.)

Há um caminho UI com atomic explícito: aprovação de irmãos envolve o laço em `atomic`, salva Ausencia e pode chegar a C pelo signal. Ele tem ordem potencial **A→C**, inversa à porta **C→A**; a troca para `NO KEY` não elimina conflito entre esses locks de escrita/explícitos. Porém a seleção desse caminho é por estados ABERTAS, enquanto H06 censa falta aprovada; afirmar ciclo executável nas **mesmas linhas** exige apresentar o interleaving que supera essa diferença e habilita o ramo emissor. Esse interleaving e sua execução permanecem [nao verificado]. (`app/ponto/views.py:2603`, `app/ponto/views.py:2613`, `app/ponto/views.py:2615`, `app/ponto/views.py:2638`, `app/ponto/views.py:2640`; `app/ponto/services/ausencia.py:1038`; `app/ponto/signals.py:147`; `app/chamados/models.py:866`; `app/ponto/services/ausencia.py:651`, `app/ponto/services/ausencia.py:654`; `app/ponto/management/commands/rederivar_falta_declarada.py:55`.)

Os ciclos v5 admissão/transferência por FK de E e célula por FK de A foram tratados no modo inicial das travas; uma garantia global de ausência de ciclos, incluindo upgrades e escritores transacionais, não foi demonstrada pelos testes novos, que sondam a porta antes do primeiro save. (`app/folha/export.py:570`; `app/ponto/services/ausencia.py:651`, `app/ponto/services/ausencia.py:654`; `app/ponto/tests/test_h06_reversao_guardas.py:819`, `app/ponto/tests/test_h06_reversao_guardas.py:875`, `app/ponto/tests/test_h06_reversao_guardas.py:885`.)

## 05/11 19:06 · sessao 32c480cca735 · - · - · origem=Ronald (tmux)

Revisão estática; testes Django e mutantes de produção executados: **[nao verificado]**. Executei somente os helpers AST extraídos em memória, usando `python3 -B`, sem importar o app nem gravar arquivos; o comportamento descrito corresponde a `app/chamados/tests/test_h06_selo_minutos_sem_literal.py:22-52`.

| Teste novo/alterado na v6 | O que morde; mutante que permanece verde isoladamente | [nao verificado]
|---|---|
| `test_MORDE_entrega_e_ato_tomam_a_mesma_trava_da_empresa` | Aceita SQL `FOR UPDATE` e `FOR NO KEY UPDATE`, mas só verifica presença do nome da tabela. **Sobrevive registrar fazer SELECT de Empresa `pk=-1` em vez da Empresa real**, mantendo a porta correta. Não prova conflito real entrega × ato. (`app/ponto/tests/test_h06_reversao_guardas.py:251-267`; produção: `app/folha/models.py:181`.) |
| `test_MORDE_aplicar_trava_e_reconfere_a_empresa_antes_de_escrever` | Cobra ordem SQL Empresa/C/A e UPDATE depois. Sobrevive SELECT de Ausencia sobre PK inexistente seguido de leitura real sem trava; a nova sonda posterior é que mata esse mutante. (`app/ponto/tests/test_h06_reversao_guardas.py:346-374`; produção: `app/ponto/services/ausencia.py:654-655`.) |
| `test_MORDE_reverter_toma_a_trava_da_entrega_na_mesma_ordem` | Mesma limitação do teste de aplicar: ordem de comandos não prova suas linhas. (`app/ponto/tests/test_h06_reversao_guardas.py:346-380`.) |
| `test_MORDE_usuario_inexistente_recusa_na_porta_antes_de_escrever` | Morde retirar a exigência de ator, porque exige a mensagem específica e entrada na porta. Sobrevive trocar `getattr(por,'pk',None) is None` por `por is None`: o comando resolve nome inexistente para `None`; não testa objeto sem PK ou PK sem cadastro. (`app/ponto/tests/test_h06_reversao_guardas.py:565-573`; produção: `app/ponto/services/ausencia.py:619-621`; resolvedor: `app/ponto/management/commands/rederivar_falta_declarada.py:89-91`.) |
| `test_MORDE_porta_chamada_direto_sem_ator_recusa` | Morde ausência da guarda na chamada direta; a forma inválida `ato={}` impede falso verde pela validação seguinte, pois a mensagem exigida é de usuário. Sobrevive a mesma mutação `por is None`, pois o único ator exercitado é `None`. (`app/ponto/tests/test_h06_reversao_guardas.py:575-579`; produção: `app/ponto/services/ausencia.py:619-638`.) |
| `test_MORDE_ato_que_nao_confirma_nao_deixa_arquivo` | Morde retirar limpeza para falha `evento→None`. Sobrevive limitar a limpeza a `AusenciaInvalida`, deixando escapar falha de I/O, commit ou callback posterior: o teste só provoca `AusenciaInvalida`. (`app/ponto/tests/test_h06_reversao_guardas.py:581-588`; produção: `app/ponto/services/ausencia.py:704-706`, `app/ponto/services/ausencia.py:730-733`.) |
| `test_MORDE_trilha_da_linha_que_nao_grava_desfaz_o_ato` | Morde retorno ignorado da trilha individual **de aplicar**, pois deixa a trilha do ato real e faz a individual devolver `None`. **Sobrevive retirar a exigência da trilha individual da reversão**, que esse caso não executa. (`app/ponto/tests/test_h06_reversao_guardas.py:590-601`; aplicar: `app/ponto/services/ausencia.py:717-723`; reversão: `app/ponto/services/ausencia.py:533-537`.) |
| `test_MORDE_arquivo_com_pk_repetido_e_recusado` | Morde tirar unicidade: a recusa posterior por hash não basta porque exige mensagem `pk unico`. Sobrevive aceitar PKs de tipos inadequados/valores malformados: só usa duplicação de PK inteiro válido. (`app/ponto/tests/test_h06_reversao_guardas.py:603-613`; produção: `app/ponto/services/ausencia.py:631-638`.) |
| `test_MORDE_trava_da_ausencia_e_OF_self_sem_join_de_colaborador` | Morde modo diferente de `NO KEY`, remoção de `OF self` ou JOIN na consulta inspecionada. Isoladamente ainda aceita PK inexistente: só vê SQL. (`app/ponto/tests/test_h06_reversao_guardas.py:616-626`; produção: `app/ponto/services/ausencia.py:654-655`.) |
| `test_MORDE_antes_da_primeira_escrita_a_porta_ja_segura_as_TRES_linhas_reais` | **Fecha o sobrevivente original pk=-1 + leitura real sem trava**, pois pausa na primeira foto atual e tenta `NO KEY UPDATE NOWAIT` nas três PKs reais. Sobrevive trocar `NO KEY UPDATE` por `FOR UPDATE`: ambos conflitam com a sonda. O teste de KEY SHARE complementa isso. (`app/ponto/tests/test_h06_reversao_guardas.py:813-875`; ponto atual de pausa: `app/ponto/services/ausencia.py:693`.) |
| `test_MORDE_fk_de_outro_escritor_nao_espera_a_porta` | Morde `FOR UPDATE` nas três linhas no instante anterior às escritas. Isoladamente sobrevive retirar todas as travas: KEY SHARE igualmente passa. Não executa admissão, transferência, cartório ou signals de UI; simula seus modos de FK. (`app/ponto/tests/test_h06_reversao_guardas.py:877-884`, `app/ponto/tests/test_h06_reversao_guardas.py:850-865`.) |
| `test_MORDE_reverter_parada_antes_de_escrever_segura_as_tres_e_solta_a_fk` | Combina conflito NO KEY e compatibilidade KEY SHARE nas linhas reais da reversão; morde tanto remover trava quanto fortalecê-la para UPDATE nesse ponto. Sobrevive alterar exclusivamente as travas de aplicar: o aplicar preparatório é serial e só sua conclusão é usada. (`app/ponto/tests/test_h06_reversao_guardas.py:886-895`.) |
| AST `test_nenhum_sitio_de_producao_escreve_numero_de_minutos` | Agora pega `12*60`, `x or 720` e `ImportFrom ... as nova`. Ainda sobrevive derivador em variável: `n=720; criar_ausencia(minutos=n)`, alias por atribuição `nova=criar_ausencia; nova(minutos=720)`, `criar_ausencia(**{'minutos':720})` ou `minutos=int('720')`. Demonstrei esses quatro retornos vazios em memória. (`app/chamados/tests/test_h06_selo_minutos_sem_literal.py:30-44`, `app/chamados/tests/test_h06_selo_minutos_sem_literal.py:66-72`.) |
| AST `test_a_varredura_MORDE` | Os três novos exemplos exercitam as três curas declaradas. Como o teste só passa strings próprias ao detector, qualquer mutante na materialização produtiva permanece verde **nesse teste isolado**; cabe à varredura real pegá-lo. As quatro expressões anteriores escapam dos dois testes. (`app/chamados/tests/test_h06_selo_minutos_sem_literal.py:74-86`; materialização: `app/chamados/services/materializacao.py:484-494`, `app/chamados/services/materializacao.py:520-528`.) |

Os sobreviventes isolados complementares não invalidam automaticamente a bateria: o teste NO KEY e o teste KEY SHARE foram feitos para se complementar. A cura do mutante original de Ausencia está presente na bateria. (`app/ponto/tests/test_h06_reversao_guardas.py:867-895`.)

Há, porém, um **sobrevivente conjunto relevante**: substituir **somente dentro de `ExportacaoDominio.registrar`** a trava real por `Empresa.objects.select_for_update(no_key=True).filter(pk=-1)` mantém o teste SQL satisfeito, enquanto todas as novas sondas reais continuam examinando a **porta**, nunca a entrega. A entrega intercalada também é injetada serialmente antes da porta. Esse conjunto não demonstra que `registrar` protege a Empresa real; execução do mutante: **[nao verificado]**. (`app/folha/models.py:181`; `app/ponto/tests/test_h06_reversao_guardas.py:251-267`, `app/ponto/tests/test_h06_reversao_guardas.py:495-508`, `app/ponto/tests/test_h06_reversao_guardas.py:867-895`.)

Sobre os helpers:

- `_updates_de_ausencia` **corrigiu o corte temporal apontado na v5**: marca antes de chamar a implementação e conta os UPDATEs desde a entrada, exigindo uma entrada. Um UPDATE ORM da própria porta anterior à trava Empresa agora entra na contagem. (`app/ponto/tests/test_h06_reversao_guardas.py:403-422`.)
- A afirmação “tudo conta” ainda precisa do limite: captura a conexão corrente e só SQL começando literalmente com `UPDATE "ponto_ausencia"`. Escrita no comando antes da porta, outra conexão, CTE contendo UPDATE ou SQL com outro prefixo não é contado. Não encontrei esses desvios no código atual da porta; são limites de prova, não escrita atual demonstrada. (`app/ponto/tests/test_h06_reversao_guardas.py:405-422`; implementação atual: `app/ponto/services/ausencia.py:646-733`.)
- `_trava` só reconhece a expressão textual `FOR (NO KEY )?UPDATE`; não prova PK, linha retornada, `OF`, ausência de JOIN ou modo exato. Os testes SQL que o usam podem aceitar uma trava sobre conjunto vazio ou modo mais forte; as novas sondas reais reduzem essa lacuna para a porta, mas não para `registrar`. (`app/ponto/tests/test_h06_reversao_guardas.py:34-37`, `app/ponto/tests/test_h06_reversao_guardas.py:255-267`, `app/ponto/tests/test_h06_reversao_guardas.py:356-370`.)
- `_pega` transforma **qualquer `OperationalError`** em “linha ocupada”, sem exigir SQLSTATE de lock. O teste KEY SHARE em conjunto exige sucesso e reduz falsos positivos de falha persistente, mas o helper sozinho não identifica a causa da recusa. (`app/ponto/tests/test_h06_reversao_guardas.py:850-860`.)
- O helper de pausa sinaliza `chegou` também no `finally`; o teste KEY SHARE poderia observar porta já concluída se a primeira foto desaparecesse. Os testes que também exigem NO KEY conflitante reprovam esse caso. A pausa é um marco do código atual, não uma medição independente de “primeira escrita”. (`app/ponto/tests/test_h06_reversao_guardas.py:819-848`, `app/ponto/tests/test_h06_reversao_guardas.py:867-895`.)
- O teste de duas reversões continua podendo sobreviver à remoção da trava da Empresa, porque Colaborador ainda serializa as execuções e `_espera_bloqueio` observa apenas `wait_event_type='Lock'`. Isso não derruba a sonda real de Empresa, mas impede atribuir a esse teste isolado a prova de serialização especificamente na Empresa. (`app/ponto/tests/test_h06_reversao_guardas.py:722-737`, `app/ponto/tests/test_h06_reversao_guardas.py:897-941`; produção: `app/ponto/services/ausencia.py:647-655`.)

## 05/11 19:06 · sessao a704c090f59e · - · - · origem=Ronald (tmux)

Q3 — a porta centraliza entrega, identidade, empresa atual, minutos esperados e hash/consumo nas duas direções; o comando chama a mesma função em `app/ponto/management/commands/rederivar_falta_declarada.py:122` e `:144`, e as guardas ficam em `app/ponto/services/ausencia.py:657`, `:664`, `:668`, `:670`, `:676`, `:681`. O consumo e a escrita individual ficam dentro do mesmo `atomic` iniciado em `app/ponto/services/ausencia.py:646`, com lavra em `:699` e reversão em `:725`.

A unificação **literal** continua incompleta: o predicado `(motivo or '').strip()` é exigido tanto pela porta H06 (`app/ponto/services/ausencia.py:617`) quanto pelo escritor de reversão (`app/ponto/services/ausencia.py:528`). O único chamador de produção encontrado desse escritor é a própria porta (`app/ponto/services/ausencia.py:725`); isso afasta um segundo caminho atual, mas não elimina a duplicação. O comando agora apenas resolve o usuário (`app/ponto/management/commands/rederivar_falta_declarada.py:88`), e a porta exige PK (`app/ponto/services/ausencia.py:619`). “Usuário existente” não é validado por consulta da própria porta: ela confere a presença de PK; o comando resolve por consulta e a auditoria tem FK (`app/ponto/management/commands/rederivar_falta_declarada.py:91`; `app/core/models.py:295`).

O censo ainda seleciona falta/aprovada/720/com chamado/data na janela, calcula a grade, separa zero e diferença (`app/ponto/management/commands/rederivar_falta_declarada.py:55`, `:61`, `:65`, `:69`). A porta confere a identidade fornecida; não valida independentemente essa elegibilidade (`app/ponto/services/ausencia.py:633`, `:665`). Portanto, o descarte 1 procede como separação do censo autorizada no texto fornecido, sem significar que a porta seja autossuficiente diante de qualquer produtor arbitrário de `ato` (`app/ponto/management/commands/rederivar_falta_declarada.py:118`; `app/ponto/services/ausencia.py:665`).

Q3 — arquivos:

- **Arquivo preexistente:** `xb` falha antes de `escrito.append`, portanto a sentinela preexistente não entra no `unlink` desse caminho (`app/ponto/services/ausencia.py:696`, `:698`, `:731`).
- **Órfão por escrita/close falhar:** a propriedade do arquivo só é registrada depois de sair do `with open`. Se `write` ou o fechamento falhar depois de criá-lo, `escrito` continua vazio e ele fica no disco. Logo PROCEDENTE 6 está apenas **parcialmente fechado** (`app/ponto/services/ausencia.py:696`, `:697`, `:698`, `:730`).
- **Atomic externo:** a porta usa `transaction.atomic()` comum, sem exigir a transação externa final. Um rollback posterior do chamador não atravessa seu `except`, podendo deixar arquivo de ato não confirmado. Os chamadores CLI atuais não abrem atomic externo, mas a função pública permite aninhamento (`app/ponto/services/ausencia.py:646`, `:730`; `app/ponto/management/commands/rederivar_falta_declarada.py:93`, `:122`, `:144`).
- **Arquivo confirmado apagado:** o `except BaseException` engloba a saída de `atomic`; o `save` registra callback `on_commit` (`app/ponto/signals.py:272`, `:279`). Os blocos normais desse callback usam `engolir`, que captura apenas `Exception`, permitindo escapar `KeyboardInterrupt/SystemExit` (`app/ponto/signals.py:254`, `:260`, `:263`; `app/core/observ.py:90`). Se uma dessas interrupções ocorrer no callback pós-commit, o `unlink` da porta será executado embora a transação já esteja confirmada (`app/ponto/services/ausencia.py:730`). Ocorrência e semântica efetiva do Django instalado: **[nao verificado]**; Django não está instalado no ambiente consultado.
- **Arquivo substituído no caminho:** a limpeza guarda somente o pathname e não confere inode/hash antes de apagar; uma substituição externa entre criação e falha seria apagada. Nenhum escritor de produção que faça essa substituição foi localizado; ocorrência **[nao verificado]** (`app/ponto/services/ausencia.py:698`, `:732`).

Q4 — mapa atualizado:

| Valor | Escritor | Leitor |
|---|---|---|
| `Ausencia.minutos` | criação recebe valor em `app/ponto/services/ausencia.py:343`; edição zera/atribui em `:507`; modelo soma grade em `app/ponto/models.py:877`, `:885`, `:887`; transição persiste em `app/ponto/services/ausencia.py:1037`; reversão repõe em `:531` | censo `app/ponto/management/commands/rederivar_falta_declarada.py:56`, `:69`; guarda/pós-checagem `app/ponto/services/ausencia.py:668`, `:714`; rateio `app/folha/export.py:121`; financeiro `app/ponto/services/fechamento.py:665`, `:685`; crédito `app/ponto/services/credito_parcial.py:24`; efeitos `app/ponto/services/efeito_ausencia.py:44`, `:51`; parcialidade `app/ponto/turnos.py:2600` |
| `Ausencia.tipo` | criação `app/ponto/services/ausencia.py:342`; edição `:497`; produtores materializadores `app/chamados/services/materializacao.py:468`, `:485`, `:521` | censo `app/ponto/management/commands/rederivar_falta_declarada.py:55`; identidade `app/ponto/services/ausencia.py:548`, `:665`; derivação `app/ponto/models.py:877`; parcialidade `app/ponto/turnos.py:2598` |
| `Ausencia.status` | criação `app/ponto/services/ausencia.py:344`; máquina `:1027`, `:1038`; rejeição de férias em lote `:1486` | censo `app/ponto/management/commands/rederivar_falta_declarada.py:55`; identidade `app/ponto/services/ausencia.py:548`, `:665`; derivação `app/ponto/models.py:877` |
| `Colaborador.empresa_id` | portas gerais `app/colaboradores/services/colaborador.py:55`, `:61`, `:73`, `:76`, `:91`, `:93`; lote permitido em `:80`, `:88` | censo `app/ponto/management/commands/rederivar_falta_declarada.py:55`; leitura travada e comparação `app/ponto/services/ausencia.py:651`, `:653`, `:670`; foto `app/ponto/management/commands/carimbo_gravado.py:53`; lista `app/ponto/services/fechamento_lista.py:103` |
| `LogAuditoria.valor_depois` | escritor efetivo `app/core/observ.py:134`, `:136`; ato fornece `{hash,ato,arquivo,linhas}` em `app/ponto/services/ausencia.py:699`, `:702`; linhas fornecem minutos em `:535`, `:720` | H06 lê `hash` em `app/ponto/services/ausencia.py:580`; os outros campos do payload do ato não têm leitor semântico H06 localizado **[nao verificado]** |
| arquivo de reversão | comando monta nome/dict `app/ponto/management/commands/rederivar_falta_declarada.py:116`, `:118`; serviço serializa `app/ponto/services/ausencia.py:623`, escreve `:696`; exceção apaga `:732` | serviço lê bytes `app/ponto/services/ausencia.py:625`, interpreta `:628`, valida chaves/PK único `:631`, `:635`, faz hash `:639`, consulta trilha `:676`, `:681`; testes leem `['linhas']` em `app/ponto/tests/test_h06_rederivar_falta_declarada.py:92` e `app/ponto/tests/test_h06_reversao_guardas.py:145` |

Correção da revisão v5: a tela genérica carrega objetos de auditoria, mas **não renderiza `valor_depois`**; mostra data, usuário, ação, modelo, objeto, descrição e IP (`app/core/views.py:705`; `app/templates/core/log_auditoria.html:55`, `:57`, `:58`, `:68`, `:71`, `:72`, `:73`, `:74`).

A validação de formato agora exige PK único, mas continua sem tipos dos valores; PK não hashable pode gerar `TypeError` no set antes de `atomic`, em vez da recusa nominal (`app/ponto/services/ausencia.py:633`, `:635`, `:646`). Arquivos legados em lista continuam recusados (`app/ponto/services/ausencia.py:633`). Existência desses arquivos em produção: **[nao verificado]**.

Q6:

- Não identifiquei segundo escritor novo fora dos módulos declarados: Ausencia pertence a `ponto/services/ausencia.py`, ED a `folha/models.py`; as novas escritas seguem esses módulos (`app/core/portas.py:485`, `:504`; `app/ponto/services/ausencia.py:508`, `:532`).
- Os juízes registrados continuam grade=`minutos_previstos_do_dia`, TXT=`classificar_export`, entrega=`competencia_entregue`, janela=`janela_atual` (`app/core/juizes.py:173`, `:298`, `:299`, `:312`, `:314`). O helper H12 tem uma implementação compartilhada em `app/folha/export.py:60`; a lista o chama em `app/ponto/services/fechamento_lista.py:109`.
- Materialização omite minutos nos dois sítios; não há novo derivador diário: modelo e censo chamam o mesmo juiz (`app/chamados/services/materializacao.py:484`, `:520`; `app/ponto/models.py:885`; `app/ponto/management/commands/rederivar_falta_declarada.py:63`).
- Relógio novo está na borda do nome do arquivo (`app/ponto/management/commands/rederivar_falta_declarada.py:46`); os relógios internos preexistentes continuam alcançáveis (`app/ponto/models.py:873`; `app/ponto/signals.py:122`). Descarte 2 procede quanto à autoria do delta, não como prova de ausência dessa classe.
- Parcialidade 480 e financeiro por presença de minutos continuam distintos (`app/ponto/turnos.py:2580`, `:2598`; `app/ponto/services/fechamento.py:665`, `:685`); rateio uniforme permanece (`app/folha/export.py:121`).
- Pré-requisitos não podem ser dados como cumpridos: o aval exige medidas 09/10/11, preservação09 e pouso antes da exportação11 (`app/docs/PROMPTS.md:1860`, `:1861`). A foto H06 compara `FechamentoMensal`, não bytes do TXT (`app/ponto/management/commands/carimbo_gravado.py:36`, `:53`). Cumprimento em produção **[nao verificado]**.

## 05/11 19:06 · sessao 4fae3920c9ba · - · - · origem=Ronald (tmux)

Q3 — No banco padrão configurado, evento e estado materializado são escritos no mesmo `atomic`: DELETE, `bulk_create` e emissão ficam em `app/ponto/nucleo.py:57–66`; `criar` envolve create, trilha e emissão em `atomic(savepoint=False)` em `app/chamados/models.py:1053–1058`. A configuração SaaS declara uma conexão `default` em `app/config/settings/saas.py:83–93`.

Há estados sem evento autorizados pelas guardas: nascimento fora de `MODULOS_DE_TURNO` ou sem célula (`app/chamados/models.py:1056`), recompute que não remove uma identidade aberta (`app/ponto/nucleo.py:64–66`) e fechamento em dia sem célula (`app/ponto/nucleo.py:83–90`). Não aparece caminho normal que faça commit do evento elegível e rollback do respectivo chamado/materializado, ou vice-versa, dentro desses blocos; ocorrência produtiva: [nao verificado].

A garantia não inclui a escrita de origem: recompute prepara linhas antes de seu atomic (`app/ponto/nucleo.py:45–57`); salvar Batida ou vínculo pode persistir e o recompute falhar/voltar, porque os wrappers engolem a exceção (`app/ponto/signals.py:19–31`, `app/ponto/signals.py:53–69`). `registrar_batida` cria sem atomic local (`app/ponto/registro_batida.py:157–159`) e flip salva antes de recomputar (`app/ponto/registro_batida.py:254`, `app/ponto/services/flip_batida.py:30–39`).

Chamadores produtivos diretos encontrados por `rg` — `git grep` indisponível pela ausência de `.git` [nao verificado]:

- `app/ponto/signals.py:26`, acionado por save/delete Batida (`:34–50`), sem atomic externo local.
- `app/ponto/signals.py:64`, acionado por save vínculo (`:72–74`), sem atomic externo local.
- `app/ponto/management/commands/processar_alertas_turno.py:67`, sem atomic externo local (`:32–67`).
- `app/ponto/management/commands/processar_alertas_avancados.py:57`, sem atomic externo local (`:19–57`).
- `app/ponto/management/commands/materializar_turnos.py:24`, sem atomic externo local (`:16–24`).
- `app/ponto/services/flip_batida.py:39`, sem atomic externo local (`:10–39`).
- `app/ponto/services/veredito_celula.py:308`, dentro do atomic iniciado em `:204`.

O comentário “guardas acima […] não escrevem nada” é literalmente incorreto: ramos anteriores chamam trilhas (`app/chamados/models.py:1034`, `:1038`) cujas implementações escrevem auditoria (`:1143`, `:1161`). Isso não é nascimento/evento parcialmente persistido: os ramos retornam antes do create (`:1035`, `:1039`).

Também é excessiva a promessa “create, trilha e INSERT […] juntos ou nenhum” (`app/chamados/models.py:1049–1051`): trilha de nascimento é best-effort (`app/chamados/models.py:1175–1180`); `core.observ.evento` engole falha e usa savepoint próprio (`app/core/observ.py:94–105`, `:133–139`). Portanto chamado e EventoCelula podem commitar sem essa linha de trilha; não é separação entre chamado e EventoCelula.

Q5 — A extração preserva, no chamador antigo, etapas anteriores, ordem, universo, decisões, tratamento de exceção e soma retornada: perguntas absolvidas, órfãs, helper e retorno em `app/chamados/reconciliador.py:1411–1421`; filtro de todos os vivos do colaborador por pk em `:1432–1434`; juiz e `_retratar` em `:1437–1444`; exceção logada/engolida sem lista em `:1445–1448`. A comparação com o antes é sustentada pelo diff fornecido [nao verificado quanto ao histórico Git].

O consumidor adiciona lista de falhas ao helper e contador próprio (`app/chamados/reconciliador.py:1360`), fazendo exceção manter eventos pendentes/incrementar tentativas (`:1373–1374`). Retorno `False` de retenção/gate não entra nessa lista (`:518–524`), permitindo marcar evento processado sem retratar (`:1380`).

O consumidor chama fora de `tranca_em_lote` (`app/chamados/reconciliador.py:1343–1380`; command em `app/chamados/management/commands/consumir_eventos_celula.py:17–31`). Esse contexto é memo em `ContextVar`, não trava SQL (`app/chamados/juizes.py:990–1003`), e é expressamente destinado a leitor que não escreve trilha (`:995`).

Quanto a ordem de trava, a chamada nova não introduz trava explícita de Colaborador/Disputa (`app/chamados/reconciliador.py:1311–1314`); segue as portas existentes, que travam a linha do chamado (`app/chamados/models.py:668–675`). O command mantém advisory lock por execução (`app/chamados/management/commands/consumir_eventos_celula.py:19–23`). Ciclo/deadlock efetivo em produção: [nao verificado].

## 05/11 19:06 · sessao f56589db6ad3 · - · - · origem=Ronald (tmux)

Achados Q1/Q2, somente inspeção estática; execução de testes/SQL e identidade independente do hash: **[nao verificado]**. [nao verificado]

**Q1 — escritores/leitores**

| Campo/valor | Escritor | Leitor |
|---|---|---|
| `EventoCelula.celula_id` | `emitir_evento_celula`, a partir de `cel.pk` (`app/ponto/portas/celula.py:643`). | Semeadura verifica pendente por célula (`app/chamados/reconciliador.py:1284`, `:1297`); vigia da absolvição por célula (`:1262`). O consumidor carrega a linha, mas não usa o ID para restringir a ação (`:1331`, `:1338`, `:1344`). |
| `EventoCelula.colaborador_id` | Mesma porta, a partir de `cel.colaborador_id` (`app/ponto/portas/celula.py:643`). | Consumidor agrupa por colaborador e carrega colaborador (`app/chamados/reconciliador.py:1338`, `:1344`); vigia forma `_com_evento` (`app/chamados/management/commands/reconciliar_chamados.py:62`). |
| `EventoCelula.data` | Mesma porta, a partir de `cel.data` (`app/ponto/portas/celula.py:643`). | Consumidor não filtra dia: processa colaborador inteiro (`app/chamados/reconciliador.py:1338`, `:1352`, `:1360`). Outro leitor produtivo específico de `EventoCelula.data`: **[nao verificado]**. |
| `processado_em` | Inicialmente nulo (`app/escala/models.py:1859`); sucesso grava `agora` (`app/chamados/reconciliador.py:1380`). | Pendentes/mortos (`:1238`, `:1244`), guarda do UPDATE (`:1372`), retenção (`:1387`). |
| `tentativas` | Default 0 (`app/escala/models.py:1860`); falhas incrementam com `F` (`app/chamados/reconciliador.py:1374`). | Elegibilidade pendente/morto (`:1238`, `:1244`). |
| `erro` | Default vazio (`app/escala/models.py:1861`); falhas concatenadas, truncadas a 500 (`app/chamados/reconciliador.py:1374`). | Leitor produtivo específico: **[nao verificado]**. |
| `TurnoMaterializado` | Núcleo monta campos (`app/ponto/nucleo.py:48`), raw DELETE do recorte (`:78`) e `bulk_create` (`:59`). `atualizado_em` é `auto_now` (`app/ponto/models.py:1211`). | O novo `RETURNING` lê `data_turno`, `entrada_id`, `saida_id` (`app/ponto/nucleo.py:79`); identifica abertos por entrada presente/saída ausente (`:80`); compara com novos rows (`:64`). Outros leitores: premissa (`app/chamados/reconciliador.py:96`), alertas (`app/ponto/management/commands/processar_alertas_turno.py:76`; `app/ponto/management/commands/processar_alertas_avancados.py:58`), idade (`app/ponto/turnos.py:3122`), seleção (`app/ponto/selecao_periodo.py:270`), testemunha (`app/chamados/juizes.py:1478`), flip (`app/ponto/services/flip_batida.py:37`), auditor (`app/chamados/management/commands/auditar_invariantes_chamados.py:604`), shadow (`app/ponto/management/commands/verificar_nucleo.py:30`). |
| `ChamadoColaborador.status_local` | Nascimento por `abrir`: defaults do motor + `get_or_create` (`app/chamados/models.py:899`, `:932`); por `criar` (`:1054`). Transições por `_gravar_estado` (`:222`, `:279`); novo consumidor chega por `_retratar` → `reconciliar` → `_gravar_estado` (`app/chamados/reconciliador.py:1443`, `:523`; `app/chamados/models.py:620`). | Novo helper seleciona `VIVOS` (`app/chamados/reconciliador.py:1433`); motor/fechamento (`app/chamados/models.py:639`); signals (`app/chamados/signals.py:70`, `:81`, `:169`); fila/painel (`app/chamados/services/painel.py:211`). Escritas `:339/:354` são somente simulação em memória, revertida em `finally`. |
| `LogAuditoria`, ação `fechamento_recusado` | Gate da premissa (`app/chamados/models.py:730`) e gate da pergunta (`:408`) chamam único `_registrar_recusa`; este deduplica por último payload/estado (`:386`, `:388`) e chama `core.observ.evento` (`:391`), que faz INSERT em savepoint próprio best effort (`app/core/observ.py:105`, `:133`, `:134`). | Próprio dedup (`app/chamados/models.py:386`); tela genérica de auditoria (`app/core/views.py:705`). Teste novo lê `valor_depois` e exige motivo (`app/chamados/tests/test_c20_juiz3_outbox.py:225`, `:247`). |
| Retorno `premissa` | Inicialização 0 (`app/chamados/reconciliador.py:1327`); soma retorno do helper (`:1360`); devolve dict (`:1388`). | Testes novos (`app/chamados/tests/test_c20_juiz3_outbox.py:245`, `:271`, `:276`, `:295`). **Command consumidor não imprime essa chave**: template imprime `retratados`, mas não `premissa` (`app/chamados/management/commands/consumir_eventos_celula.py:32`). Outro leitor produtivo específico: **[nao verificado]**. |

A nova emissão usa o mesmo INSERT da porta, sem segundo escritor novo (`app/ponto/nucleo.py:90`; `app/chamados/models.py:1058`; `app/ponto/portas/celula.py:643`). `TurnoMaterializado` permanece no módulo autorizado por `PORTAS` (`app/core/portas.py:117`); `status_local` permanece no módulo da porta do ciclo (`app/core/portas.py:217`, `:233`). `EventoCelula` e `LogAuditoria` não têm entrada no mapa de famílias: o comentário exclui modelos de log/configuração do censo de famílias (`app/core/portas.py:38`). Assim, **não confundir ausência no mapa com novo escritor ilegal**; para Evento, a declaração explícita de escritor único é do INSERT em `escala/models.py:1851`, enquanto metadata de processamento é escrita no consumidor (`app/chamados/reconciliador.py:1374`, `:1380`).

**Q2 — equivalência do DELETE**

Para o modelo/configuração presentes, o predicado SQL é o mesmo do ORM anterior: colaborador por PK e extremos inclusivos de `data_turno` (`app/ponto/nucleo.py:78`; diff fornecido). A classe atual não declara manager ou override de delete; suas únicas FKs são **saintes**, para Colaborador e Batida, com `CASCADE` (`app/ponto/models.py:1196`, `:1201`, `:1204`, `:1206`). Não localizei referência entrante nem receiver de delete para TurnoMaterializado; a afirmação negativa de cobertura completa dinâmica é **[nao verificado]**. Os receivers de delete localizados são para Batida/Ausência/Setor/TipoAusência (`app/ponto/signals.py:48`, `:138`, `:150`, `:282`; `app/colaboradores/signals.py:35`; `app/ponto/models.py:1234`).

Portanto, não há perda de signal/cascata de saída demonstrada nesta cópia; **SQL cru não é equivalente genericamente ao Collector Django**, pois o ignora. Também fixa `django.db.connection` default (`app/ponto/nucleo.py:74`, `:76`); roteamento para outros aliases não é preservado por construção. A cópia configura somente `TenantSyncRouter` nos settings SaaS/CI e routers vazios em teste (`app/config/settings/saas.py:93`; `app/config/settings/ci_tenants.py:19`; `app/config/settings_test.py:5`); uso produtivo de outro alias/router: **[nao verificado]**.

Além do núcleo, há deleção **indireta por cascata** ao apagar Colaborador ou Batida (`app/ponto/models.py:1201`, `:1204`, `:1206`); porta de apagar Batida chama `.delete()` (`app/ponto/registro_batida.py:291`), e ensaio tem deleção em lote (`:345`). O receiver de Batida recompõe **depois** do delete (`app/ponto/signals.py:48`, `:50`): um turno já cascado pode não aparecer no `RETURNING`, portanto esse caminho não tem emissão garantida pelo novo detector (`app/ponto/nucleo.py:80`). Isso é uma lacuna de cobertura, não um segundo escritor introduzido pelo diff.

## 05/11 19:06 · sessao 194b901e3953 · - · - · origem=Ronald (tmux)

**6. O skip pode adiar a contagem, mas os três casos perguntados têm comportamentos distintos.**

- **Tentativas esgotadas:** não mascaram o chamado: `eventos_pendentes()` exige `tentativas<5`; os esgotados entram em contador próprio, incluído no alarme. (`app/chamados/reconciliador.py:1198`, `app/chamados/reconciliador.py:1238`, `app/chamados/reconciliador.py:1244`, `app/chamados/management/commands/reconciliar_chamados.py:118`)
- **Processado sem retratar:** o evento processado sai de `_com_evento`; sem outro pendente daquele colaborador, o vigia volta a perguntar. Se o escritor recusa/retem, o vigia contabiliza somente como MEDIDA, pelas mesmas guardas. (`app/chamados/management/commands/reconciliar_chamados.py:62`, `app/chamados/management/commands/reconciliar_chamados.py:73`, `app/chamados/management/commands/reconciliar_chamados.py:79`, `app/chamados/management/commands/reconciliar_chamados.py:107`)
- **Evento de outro dia:** mascara a contagem por colaborador, mas o consumidor também trabalha por colaborador e a função extraída percorre todos os chamados VIVOS, sem filtro de data/célula do evento. Portanto o dia diferente, sozinho, não demonstra perda de julgamento. (`app/chamados/reconciliador.py:1337`, `app/chamados/reconciliador.py:1360`, `app/chamados/reconciliador.py:1432`)
- **Janela demonstrável por leitura:** `_com_evento` é fotografia tomada antes do laço. Se o evento for processado depois da fotografia, o vigia pode pular aquele colaborador nesta execução mesmo após o consumo. Ocorrência e duração produtivas: **[nao verificado]**. (`app/chamados/management/commands/reconciliar_chamados.py:62`, `app/chamados/management/commands/reconciliar_chamados.py:71`)
- **Pendente persistente:** órfã adiada deixa evento pendente sem consumir tentativas; enquanto esse pendente existir, a premissa fica fora do contador, e quantidade/idade dos pendentes são somente MEDIDA. O consumidor ainda tenta julgar a premissa em cada consumo. (`app/chamados/reconciliador.py:1360`, `app/chamados/reconciliador.py:1366`, `app/chamados/reconciliador.py:1375`, `app/chamados/management/commands/reconciliar_chamados.py:113`)

**7. Os testes mordem caminhos felizes e alguns negativos, mas permitem mutantes relevantes.** [nao verificado]

Mordem emissão pelo nascimento, exclusão de módulo estranho, fechamento do aberto, recompute repetido, preservação do aberto, custo diferencial, pai retratado com recusa do furo, idempotência e vigia pendente/processado. (`app/chamados/tests/test_c20_juiz3_outbox.py:99`, `app/chamados/tests/test_c20_juiz3_outbox.py:109`, `app/chamados/tests/test_c20_juiz3_outbox.py:135`, `app/chamados/tests/test_c20_juiz3_outbox.py:146`, `app/chamados/tests/test_c20_juiz3_outbox.py:155`, `app/chamados/tests/test_c20_juiz3_outbox.py:164`, `app/chamados/tests/test_c20_juiz3_outbox.py:242`, `app/chamados/tests/test_c20_juiz3_outbox.py:271`, `app/chamados/tests/test_c20_juiz3_outbox.py:299`)

**Mutante concreto:** retirar `falhas=falhas` da chamada adicionada em `app/chamados/reconciliador.py:1360`. Os caminhos felizes ficam iguais; diante de exceção por chamado, a função engole/loga sem alimentar a lista do consumidor, que pode marcar o evento processado. (`app/chamados/reconciliador.py:1445`, `app/chamados/reconciliador.py:1447`, `app/chamados/reconciliador.py:1373`, `app/chamados/reconciliador.py:1380`) Os únicos erros injetados no arquivo novo são no INSERT do evento; não há erro injetado no julgamento/retratação do consumidor. (`app/chamados/tests/test_c20_juiz3_outbox.py:123`, `app/chamados/tests/test_c20_juiz3_outbox.py:194`) Sobrevivência prevista por inspeção; execução do mutante: **[nao verificado]**.

**Outra lacuna:** remover os atomics próprios de `criar` e `recompute_turnos`, reindentando os corpos, não é distinguido pelos testes de rollback: a base usa `TestCase` e ambos os testes envolvem explicitamente o ato em outro `transaction.atomic()`. Logo eles demonstram rollback no atomic externo, sem isolar a proteção em autocommit. (`app/chamados/tests/test_c20_juiz3_outbox.py:37`, `app/chamados/tests/test_c20_juiz3_outbox.py:125`, `app/chamados/tests/test_c20_juiz3_outbox.py:196`, `app/chamados/models.py:1053`, `app/ponto/nucleo.py:57`) Execução desse mutante: **[nao verificado]**.

## 05/11 18:45 · sessao 27a1f0698658 · H12H06 · verificador · origem=Code

A v5 corrige a guarda de status e a ordem **explícita** Empresa → Colaborador → Ausencia, mas a prova das travas continua aceitando um mutante que trava a Ausencia errada. Há também inversões com outros escritores, ao considerar FKs. (`app/ponto/tests/test_h06_reversao_guardas.py:422`, `app/ponto/tests/test_h06_reversao_guardas.py:717`; `app/ponto/services/ausencia.py:639`, `app/ponto/services/ausencia.py:641`, `app/ponto/services/ausencia.py:644`; `app/colaboradores/models.py:321`, `app/colaboradores/models.py:431`.)

Revisão estática. Execução de testes/mutantes, identidade Git da cópia, configuração efetiva do banco, ocorrência de deadlock e cumprimento dos pré-requisitos em produção: **[nao verificado]**. [nao verificado]

**1. Achados da volta 4**

| Achado | Situação na v5 |
|---|---|
| Reversão sobre outra identidade/empresa | **Fechado no código para os contraexemplos apresentados.** Confere identidade, minutos esperados e empresa atual sob trava antes da reposição. (`app/ponto/services/ausencia.py:641`, `app/ponto/services/ausencia.py:654`, `app/ponto/services/ausencia.py:658`, `app/ponto/services/ausencia.py:660`.) |
| Arquivo sobrescrevível | **Fechado.** Nome com PID/UUID e abertura exclusiva `xb`; permanece o teste da sentinela. (`app/ponto/management/commands/rederivar_falta_declarada.py:120`; `app/ponto/services/ausencia.py:686`; `app/ponto/tests/test_h06_reversao_guardas.py:230`.) |
| Aplicar usando empresa antiga | **Fechado no código.** Colaborador é travado e a empresa atual é comparada antes da edição. (`app/ponto/services/ausencia.py:641`, `app/ponto/services/ausencia.py:660`, `app/ponto/services/ausencia.py:700`.) |
| Concorrência/ranges/atomicidade | **Parcial.** Há transação, ordem explícita corrigida e consulta de todas as competências; compatibilidade global e prova das linhas travadas continuam incompletas — item 3. (`app/ponto/services/ausencia.py:638`, `app/ponto/services/ausencia.py:644`, `app/ponto/services/ausencia.py:663`; `app/ponto/tests/test_h06_reversao_guardas.py:667`.) |
| Leitor H12 divergente | **Fechado no código.** Exportação e lista compartilham o predicado; contador e HTML leem `rescisao`. (`app/folha/export.py:60`, `app/folha/export.py:870`; `app/ponto/services/fechamento_lista.py:109`; `app/ponto/views.py:1215`; `app/templates/ponto/_fech_linha.html:2`.) |
| Rollback após falha na segunda reposição | **Fechado como caso estático.** O teste executa a primeira reposição real e falha na segunda, dentro da transação da porta. Resultado executado: **[nao verificado]**. (`app/ponto/tests/test_h06_reversao_guardas.py:212`, `app/ponto/tests/test_h06_reversao_guardas.py:224`; `app/ponto/services/ausencia.py:638`, `app/ponto/services/ausencia.py:711`.) |
| Parcialidade 480 versus financeiro | **Aberto, preexistente.** Cobertura usa 480; leitores financeiros continuam usando presença de minutos. (`app/ponto/turnos.py:2580`, `app/ponto/turnos.py:2598`; `app/ponto/services/fechamento.py:665`, `app/ponto/services/fechamento.py:685`.) |
| Rateio uniforme | **Aberto, preexistente.** Continua dividindo minutos pelo número de dias. (`app/folha/export.py:118`, `app/folha/export.py:121`.) |
| Trilha engolida/prova posterior | **Parcial.** Falha da linha **do ato** agora aborta; falha da linha **por Ausencia** continua podendo ser engolida. (`app/ponto/services/ausencia.py:693`, `app/ponto/services/ausencia.py:706`, `app/ponto/services/ausencia.py:533`; `app/core/observ.py:133`, `app/core/observ.py:139`.) |
| Cadastro atual decidindo passado | **Aberto; investigação separada no aval.** O corte continua vindo do cadastro atual. (`app/ponto/janelas.py:50`; `app/docs/PROMPTS.md:1861`.) |
| Arquivos antigos | **Aberto como compatibilidade.** Agora qualquer formato antigo em lista é recusado; não há adaptação nem vinculação retroativa à trilha de hash. Existência de arquivos afetados: **[nao verificado]**. (`app/ponto/services/ausencia.py:628`, `app/ponto/services/ausencia.py:672`.) |
| Casos de status/data_fim/contador/trava | **Parcial.** O novo caso de status distingue guarda de pós-checagem; os testes de trava ainda aceitam proteção da Ausencia errada. (`app/ponto/tests/test_h06_reversao_guardas.py:426`, `app/ponto/tests/test_h06_reversao_guardas.py:545`, `app/ponto/tests/test_h06_reversao_guardas.py:559`, `app/ponto/tests/test_h06_reversao_guardas.py:717`.) |

Dos descartes discutidos na v4: `invalidar` continua sem criar/desinvalidar entrega; a abertura exclusiva continua correta contra sobrescrita; 480/rateio continuam presentes; MODO-COMUM está explicitamente separado no aval. A ausência de trava em leitores não dispensa examinar escritores, sinais e FKs. (`app/folha/models.py:158`, `app/folha/models.py:162`; `app/ponto/services/ausencia.py:686`; `app/ponto/turnos.py:2580`; `app/folha/export.py:121`; `app/docs/PROMPTS.md:1861`; `app/ponto/signals.py:147`.)

**2. A porta é realmente única?**

**Para entrega, identidade, empresa atual, minutos esperados e consumo, sim nos caminhos atuais encontrados.** Aplicar e reverter chamam a mesma função, e essas decisões estão nela. O comando não mantém cópia dessas guardas. (`app/ponto/management/commands/rederivar_falta_declarada.py:126`, `app/ponto/management/commands/rederivar_falta_declarada.py:148`; `app/ponto/services/ausencia.py:647`, `app/ponto/services/ausencia.py:654`, `app/ponto/services/ausencia.py:658`, `app/ponto/services/ausencia.py:660`, `app/ponto/services/ausencia.py:666`.)

**A afirmação irrestrita “toda decisão do ato mora aqui” é excessiva:**

- Motivo obrigatório continua espelhado no comando, na porta e na reposição. (`app/ponto/management/commands/rederivar_falta_declarada.py:93`; `app/ponto/services/ausencia.py:615`, `app/ponto/services/ausencia.py:528`.)
- Usuário existente é exigido pelo comando; a porta não faz essa exigência, e `evento` admite ator sem PK. (`app/ponto/management/commands/rederivar_falta_declarada.py:90`; `app/ponto/services/ausencia.py:615`; `app/core/observ.py:118`.)
- A elegibilidade do censo — falta/aprovada/720/com chamado/janela/grade positiva e diferente — permanece no comando. A porta compara o estado com a identidade fornecida, sem validar esse universo como contrato próprio. Isso é uma fronteira confiando no produtor do censo, não outra cópia do juiz diário. (`app/ponto/management/commands/rederivar_falta_declarada.py:55`, `app/ponto/management/commands/rederivar_falta_declarada.py:65`; `app/ponto/services/ausencia.py:626`, `app/ponto/services/ausencia.py:654`.)

`repor_minutos_de_reversao` continua gravável diretamente, exigindo apenas motivo; seu único chamador de produção localizado é a própria porta. Não há evidência de segundo caminho atual usando essa possibilidade. Ambas as funções permanecem no módulo escritor declarado de Ausencia. (`app/ponto/services/ausencia.py:528`, `app/ponto/services/ausencia.py:532`, `app/ponto/services/ausencia.py:711`; `app/core/portas.py:485`.)

**3. Ordem de travas e outros escritores**

A sequência abaixo descreve aquisições explícitas e escritas/FKs alcançáveis pelo código. Modos físicos, gatilhos instalados, instante das checagens de FK e sequência efetivamente executada no banco: **[nao verificado]**.

| Etapa | Aplicar e reverter |
|---|---|
| 1 | Abre `atomic`; trava **Empresa do ato FOR UPDATE**. (`app/ponto/services/ausencia.py:638`, `app/ponto/services/ausencia.py:639`; `app/folha/export.py:565`.) |
| 2 | Trava **Colaboradores FOR UPDATE**, PK crescente. (`app/ponto/services/ausencia.py:641`.) |
| 3 | Trava **Ausencias FOR UPDATE OF self**, PK crescente, sem ordenação herdada por nome. (`app/ponto/services/ausencia.py:644`.) |
| 4 | Consulta entrega, identidade, empresa e hash. Entrega e foto são leituras sem `FOR UPDATE`; não acrescentam trava explícita de ED/FM. (`app/ponto/services/ausencia.py:647`, `app/ponto/services/ausencia.py:666`; `app/folha/export.py:593`; `app/ponto/management/commands/carimbo_gravado.py:53`.) |
| 5 | Insere **LogAuditoria do ato**, em savepoint. A FK é `usuario → auth.User`; `objeto_id` é inteiro, **não FK para Empresa**. (`app/ponto/services/ausencia.py:688`; `app/core/observ.py:133`; `app/core/models.py:295`, `app/core/models.py:298`.) |
| 6 | **Aplicar:** edição faz UPDATE de Ausencia por `save()` completo. **Reverter:** reposição faz UPDATE de minutos/atualizado_em. Ambas já possuem a trava da linha. (`app/ponto/services/ausencia.py:700`, `app/ponto/services/ausencia.py:508`, `app/ponto/services/ausencia.py:711`, `app/ponto/services/ausencia.py:532`.) |
| 7 | O `post_save` síncrono chama cartório para os dias; a abrangência depende das datas/células e do relógio interno. (`app/ponto/signals.py:115`, `app/ponto/signals.py:122`, `app/ponto/signals.py:143`, `app/ponto/signals.py:147`; `app/ponto/services/cartorio.py:1974`.) |
| 8 | **Se emitir chamado:** cartório → emissor → `ChamadoColaborador.abrir`; readquire **Colaborador FOR UPDATE**, depois encontra/cria Chamado. Chamado tem FKs para Colaborador e CelulaDia. (`app/ponto/services/cartorio.py:1740`, `app/ponto/services/cartorio.py:1778`; `app/ponto/management/commands/detectar_ausencias.py:693`; `app/chamados/models.py:866`, `app/chamados/models.py:932`, `app/chamados/models.py:32`, `app/chamados/models.py:44`.) |
| 9 | Cartório pode atualizar **CelulaDia**, inclusive sua FK de Ausencia, e inserir **EventoCelula** no mesmo atomic. EventoCelula usa IDs inteiros, sem FKs. (`app/ponto/services/cartorio.py:1822`; `app/ponto/portas/celula.py:810`, `app/ponto/portas/celula.py:647`, `app/ponto/portas/celula.py:648`, `app/ponto/portas/celula.py:650`; `app/escala/models.py:1796`, `app/escala/models.py:1853`.) |
| 10 | Insere trilha individual, tira foto final e sai do atomic. O consumidor de Ausencia é agendado em `on_commit`; suas escritas ficam depois do commit. (`app/ponto/services/ausencia.py:706`, `app/ponto/services/ausencia.py:533`, `app/ponto/services/ausencia.py:712`; `app/ponto/signals.py:272`.) |

Outros escritores identificados que podem acumular pelo menos duas das quatro tabelas pedidas:

| Escritor/caminho | Ordem |
|---|---|
| `ExportacaoDominio.registrar` | **Empresa → INSERT ED**, no mesmo atomic; a FK de Empresa incide sobre linha já protegida. (`app/folha/models.py:179`, `app/folha/models.py:181`, `app/folha/models.py:186`.) |
| Admissão | **Cmax FOR UPDATE → INSERT Cnovo → FK Empresa**. A trava de matrícula permanece dentro do atomic externo da admissão. (`app/colaboradores/views.py:1175`, `app/colaboradores/views.py:1182`; `app/colaboradores/models.py:431`, `app/colaboradores/models.py:439`, `app/colaboradores/models.py:321`.) |
| Edição cadastral com mudança de empresa | **UPDATE Colaborador → FK Empresa destino**, dentro do atomic. (`app/colaboradores/views.py:1313`, `app/colaboradores/views.py:1340`, `app/colaboradores/views.py:1342`; `app/colaboradores/services/colaborador.py:76`.) |
| Transferência de posto/empresa | **UPDATE Colaborador → FK Empresa destino**, dentro do atomic. (`app/colaboradores/views.py:1384`, `app/colaboradores/views.py:1386`, `app/colaboradores/views.py:1388`; `app/colaboradores/models.py:321`.) |
| Criação de Ausencia | **INSERT Ausencia → [Colaborador FOR UPDATE se emissão]**, mais FK Colaborador. O atomic envolve o save e os sinais. (`app/ponto/services/ausencia.py:417`, `app/ponto/services/ausencia.py:418`; `app/ponto/signals.py:147`; `app/chamados/models.py:866`; `app/ponto/models.py:743`.) |
| Aprovação de irmãos | **Ausencia → [Colaborador se emissão] → próxima Ausencia**, sob atomic externo; também alcança invalidação de fechamento. (`app/ponto/views.py:2638`, `app/ponto/views.py:2640`; `app/ponto/services/ausencia.py:1020`, `app/ponto/services/ausencia.py:1131`.) |
| Regularização | Pode adquirir **Colaborador pelo signal da Justificativa**, depois acumular **Ausencias → [Colaborador] → próxima Ausencia**. (`app/ponto/services/regularizacao.py:89`, `app/ponto/services/regularizacao.py:91`, `app/ponto/services/regularizacao.py:118`; `app/ponto/signals.py:161`.) |
| Materialização de disputa | Criação de **Ausencia → [Colaborador no signal]** sob atomic externo; outros ramos também materializam batidas antes. (`app/chamados/services/materializacao.py:1145`, `app/chamados/services/materializacao.py:1147`, `app/chamados/services/materializacao.py:1151`, `app/chamados/services/materializacao.py:484`.) |
| Abandono provisório | Laço de criações de **Ausencia → [Colaborador no signal]**, sob atomic do comando. (`app/ponto/management/commands/marcar_abandono_provisorio.py:22`, `app/ponto/management/commands/marcar_abandono_provisorio.py:70`, `app/ponto/management/commands/marcar_abandono_provisorio.py:76`.) |
| Veredito da célula | Atomic externo → criação de **Ausencia → [Colaborador no signal]**. (`app/ponto/services/veredito_celula.py:204`, `app/ponto/services/veredito_celula.py:315`.) |
| Sincronização de férias | Criação/remoção de **Ausencias → [Colaborador nos sinais]**, podendo herdar o atomic do agendamento/correção. (`app/ponto/services/ausencia.py:1375`, `app/ponto/services/ausencia.py:1387`, `app/ponto/services/ausencia.py:1398`; `app/ferias/services.py:315`, `app/ferias/services.py:375`.) |

Exaustividade de todas as combinações transitivas desses escritores, seus chamadores, sinais e FKs: **[nao verificado]**.

**Há ciclos possíveis inferidos nas mesmas linhas:**

- **Admissão × porta:** escolha Cmax como colaborador alvo H06 de E. A porta segura E e espera Cmax; a admissão segura Cmax e a inserção de Cnovo precisa verificar a FK da mesma E. É **E → Cmax** contra **Cmax → E**. As aquisições estão no código; a espera `KEY SHARE` e a execução do ciclo são **[nao verificado]**. (`app/ponto/services/ausencia.py:639`, `app/ponto/services/ausencia.py:641`; `app/colaboradores/views.py:1175`, `app/colaboradores/views.py:1186`; `app/colaboradores/models.py:431`, `app/colaboradores/models.py:439`, `app/colaboradores/models.py:321`.)
- **Transferência × reversão antiga:** arquivo pertence a E, C hoje está em O, e uma transferência O→E segura C. A reversão segura E antes de tentar C; a transferência precisa verificar a FK de E. A guarda “empresa de hoje” só vem depois da espera em C. Execução: **[nao verificado]**. (`app/ponto/services/ausencia.py:639`, `app/ponto/services/ausencia.py:641`, `app/ponto/services/ausencia.py:660`; `app/colaboradores/views.py:1384`, `app/colaboradores/views.py:1386`, `app/colaboradores/views.py:1388`.)
- **Cartório externo × porta:** uma atualização de CelulaDia de `ausencia=NULL` para A pode segurar a célula e verificar a FK de A; a porta segura A e seu signal tenta atualizar a mesma célula. É uma inversão adicional **A → Celula** contra **Celula → A**, com execução **[nao verificado]**. (`app/ponto/services/ausencia.py:644`; `app/ponto/signals.py:147`; `app/ponto/portas/celula.py:648`, `app/ponto/portas/celula.py:810`; `app/escala/models.py:1796`.)

**4. Hash: aplicação, consumo e falha da trilha**

A proteção é a **trava da Empresa**, não uma trava ou constraint única no LogAuditoria. A consulta usa exatamente ação, `modelo='Empresa'`, `objeto_id=empresa_id` e `valor_depois__hash=h`, pegando a primeira PK. O modelo de auditoria não declara unicidade desse hash. (`app/ponto/services/ausencia.py:639`, `app/ponto/services/ausencia.py:575`, `app/ponto/services/ausencia.py:578`; `app/core/models.py:309`.)

Aplicar recusa hash já aplicado; reverter exige aplicado e recusa consumido. A linha aplicada permanece, portanto reaplicar exatamente o mesmo hash também é recusado depois do consumo. (`app/ponto/services/ausencia.py:666`, `app/ponto/services/ausencia.py:668`, `app/ponto/services/ausencia.py:671`, `app/ponto/services/ausencia.py:675`.)

Pelo protocolo do código, duas reversões do mesmo arquivo não confirmam ambas: serializam na Empresa, e a segunda encontra consumo. Duas aplicações sobre o mesmo estado censado também não escrevem ambas: a segunda encontra minutos diferentes ou hash aplicado. Duas execuções CLI podem terminar sem erro se a segunda fizer seu censo depois e retornar zero alvos, ou se operarem alvos distintos. Comportamento concorrente executado/isola­mento efetivo: **[nao verificado]**. (`app/ponto/services/ausencia.py:639`, `app/ponto/services/ausencia.py:658`, `app/ponto/services/ausencia.py:668`, `app/ponto/services/ausencia.py:675`, `app/ponto/services/ausencia.py:681`; `app/ponto/management/commands/rederivar_falta_declarada.py:105`, `app/ponto/management/commands/rederivar_falta_declarada.py:129`.)

`evento` usa savepoint, engole a falha e retorna `None`. Para a **linha do ato**, esse retorno causa exceção e rollback antes dos UPDATEs de Ausencia. Para a **linha por Ausencia**, o retorno é ignorado: o ato pode confirmar sem essa trilha individual. (`app/core/observ.py:105`, `app/core/observ.py:133`, `app/core/observ.py:139`; `app/ponto/services/ausencia.py:693`, `app/ponto/services/ausencia.py:706`, `app/ponto/services/ausencia.py:533`.)

O arquivo é criado antes da lavra; uma falha posterior pode deixá-lo sem ato confirmado. A reversão desse arquivo será recusada por falta do hash aplicado. Não há limpeza do arquivo nesse caminho. (`app/ponto/services/ausencia.py:686`, `app/ponto/services/ausencia.py:688`, `app/ponto/services/ausencia.py:672`.)

Não localizei outro produtor de `h06_ato_aplicado`/`h06_ato_revertido`: as constantes e a emissão comum estão no serviço. Isso não transforma o LogAuditoria em armazenamento com unicidade própria. (`app/ponto/services/ausencia.py:547`, `app/ponto/services/ausencia.py:548`, `app/ponto/services/ausencia.py:688`; `app/core/models.py:309`.)

**5. Quem escreve e quem lê**

| Campo/valor | Escrita | Leitura |
|---|---|---|
| `Ausencia.colaborador_id` | Criação pela porta. (`app/ponto/services/ausencia.py:342`, `app/ponto/services/ausencia.py:418`.) | Censo, identidade, trava e empresa atual. (`app/ponto/management/commands/rederivar_falta_declarada.py:55`; `app/ponto/services/ausencia.py:546`, `app/ponto/services/ausencia.py:642`, `app/ponto/services/ausencia.py:660`.) |
| `tipo` | Criação/edição. (`app/ponto/services/ausencia.py:342`, `app/ponto/services/ausencia.py:497`.) | Censo/identidade, derivação e parcialidade. (`app/ponto/management/commands/rederivar_falta_declarada.py:55`; `app/ponto/services/ausencia.py:546`; `app/ponto/models.py:877`; `app/ponto/turnos.py:2598`.) |
| `status` | Criação, transição e rejeição de férias em lote. (`app/ponto/services/ausencia.py:344`, `app/ponto/services/ausencia.py:1009`, `app/ponto/services/ausencia.py:1020`, `app/ponto/services/ausencia.py:1466`.) | Censo/identidade e derivação. (`app/ponto/management/commands/rederivar_falta_declarada.py:55`; `app/ponto/services/ausencia.py:546`; `app/ponto/models.py:877`.) |
| `data/data_fim` | Criação, `derivar_data_fim`, edição e completamento no modelo. (`app/ponto/services/ausencia.py:66`, `app/ponto/services/ausencia.py:342`, `app/ponto/services/ausencia.py:498`; `app/ponto/models.py:907`.) | Censo, identidade, competências, grade e expansão dos dias. (`app/ponto/management/commands/rederivar_falta_declarada.py:57`, `app/ponto/management/commands/rederivar_falta_declarada.py:61`; `app/ponto/services/ausencia.py:546`, `app/ponto/services/ausencia.py:562`; `app/ponto/models.py:882`; `app/folha/export.py:70`.) |
| `minutos` | Criação; edição zera/recebe número; modelo deriva; transição persiste; reversão restaura. (`app/ponto/services/ausencia.py:343`, `app/ponto/services/ausencia.py:507`, `app/ponto/services/ausencia.py:1020`, `app/ponto/services/ausencia.py:531`; `app/ponto/models.py:887`.) | Censo, guardas, pós-checagem, rateio, financeiro, créditos, efeitos, cobertura e apresentação. (`app/ponto/management/commands/rederivar_falta_declarada.py:56`; `app/ponto/services/ausencia.py:658`, `app/ponto/services/ausencia.py:703`; `app/folha/export.py:121`; `app/ponto/services/fechamento.py:665`; `app/ponto/services/credito_parcial.py:24`; `app/ponto/services/efeito_ausencia.py:44`; `app/ponto/turnos.py:2600`; `app/templates/ponto/ausencias.html:163`.) |
|

[truncado em 20000 de 34368 caracteres]

## 05/11 18:45 · sessao 3a1d4c0f241c · PC3-FIO-MUDO · censo · origem=Code

HASH LIDO = bede09aaded33cbd9e87a04a4d4f5a6be1e00af1

## A — CASOS PELA REGRA

O contrato é **abertura de `batida_ausente` + reabrir + retratar duplicata + validação + morte por leitura + reapontamento**. Vivo legítimo sem pergunta sai do universo antes do RED; **494 não é quantidade demonstrada de defeitos**. O dado não contém os registros necessários para contar essas exclusões. (`app/docs/PROMPTS.md:1875`; `app/docs/PROMPTS.md:1882`; distribuição dos 494: [não verificado])

Os casos abaixo são critérios de aceite, sem resultado executado. A referência é a regra aplicada ao cadastro e aos fatos, conforme L-110. (`app/docs/LEIS.md:136`; execução: [não verificado])

| Caso | Cadastro + estado + ato | Resposta exigida pela regra |
|---|---|---|
| A01 — nascimento | Ativo, vínculo válido no dia, ata acusa marco vencido, sem pergunta; abrir `batida_ausente`. | Entregar pergunta correspondente no evento, lendo a lâmpada. (`app/docs/PROMPTS.md:1875`; `CLAUDE.md:167`; `CLAUDE.md:168`) |
| A02 — nascimento concorrente | Mesmo cadastro e fato; duas aberturas simultâneas. | Mesmo estado final, uma trilha do fato e nenhuma cobrança duplicada. (`CLAUDE.md:175`) |
| A03 — abertura recusada | Cadastro vetado pelo juiz existente; tentar abertura automática. | Respeitar o veto e seu motivo; não criar pergunta para compensar nascimento recusado. (`app/core/juizes.py:276`; `app/chamados/models.py:927`; `app/docs/CORTES.md:403`) |
| A04 — reabertura sem candidata | Dívida persiste; chamado encerrado; nenhuma pergunta restaurável; reabrir. | Estado vivo sozinho não satisfaz o contrato: deve existir pergunta viva ou ação administrativa nomeada. (`app/docs/PROMPTS.md:1882`; `app/docs/CORTES.md:253`) |
| A05 — reabertura com marco resolvido | Pergunta antiga cobra saída já comprovada; outro marco continua devido; reabrir. | Não ressuscitar a pergunta do marco resolvido; conservar fio/ação para o restante. (`app/chamados/models.py:1454`; `app/chamados/juizes.py:1173`; `app/docs/CORTES.md:253`) |
| A06 — última duplicata | A perde sua última pergunta; a canônica está em B; retratar duplicata. | Preservar o fio alcançável do fato sem exigir uma nova pergunta por chamado duplicado. (`CLAUDE.md:175`; `app/docs/CORTES.md:253`; `app/docs/CORTES.md:404`) |
| A07 — duplicata respondida | Pergunta contém resposta discordante sem decisão humana; retratar duplicata. | Preservar a conversa e a possibilidade de decisão humana; identidade não autoriza apagar a resposta. (`app/chamados/juizes.py:1150`; `app/chamados/juizes.py:1171`; `app/chamados/services/fio.py:870`) |
| A08 — validação com resto | Ata acusa saída e intervalo; validar/materializar apenas saída. | Não reperguntar a saída; manter fio ou ação nomeada para o intervalo. (`app/chamados/juizes.py:1173`; `app/docs/CORTES.md:253`) |
| A09 — validação recusada | Resposta de hora; plantio recusado com motivo; fato continua devido. | Não produzir quitação fictícia; devolver a decisão ao admin preservando a resposta. (`app/chamados/services/validacao.py:155`; `app/chamados/services/validacao.py:171`; `app/chamados/services/validacao.py:413`) |
| A10 — reapontamento | A perde a última resolvedora para B, no mesmo fato. | Preservar alcance e tratar a origem que perdeu o fio; não criar irmã apenas porque mudou a âncora. (`app/docs/PROMPTS.md:1882`; `app/docs/CORTES.md:404`; `CLAUDE.md:175`) |
| A11 — costura concorrente | Reapontamento e encerramento de B concorrem. | Não terminar com pergunta viva inacessível sob âncora terminal; reatar legitimamente ou recusar com motivo. (`app/chamados/models.py:202`; `app/chamados/services/fio.py:752`; `app/docs/CORTES.md:404`) |
| A12 — morte por leitura | Batida/célula/tranca muda; a pergunta não recebe novo `save`. | C6 decide a vida. O evento deve nascer da mudança do insumo; não criar outro juiz nem transformar o leitor em escritor. (`app/core/juizes.py:266`; `app/chamados/juizes.py:1135`; `CLAUDE.md:168`; `CLAUDE.md:170`) |
| A13 — premissa resolvida | Dia regularizado; chamado continua vivo sem pergunta. | Encerrar por lastro pela porta; não fabricar pergunta para zerar contador. (`app/core/juizes.py:263`; `app/chamados/models.py:680`; `app/docs/ESTRUTURAL.md:46`) |
| A14 — 21→20 | Empresa com corte cadastrado 21; fatos de 21/09, 20/10 e 21/10. | Os dois primeiros pertencem à competência 10; o terceiro, à 11. Aplicar o corte da empresa, sem cravar 21 universalmente. (`app/ponto/janelas.py:10`; `app/ponto/janelas.py:24`; `app/docs/LEIS.md:123`) |
| A15 — cross-meia-noite | Turno 22h–06h ancorado ontem; observar 00h01 e 06h10. | Meia-noite não encerra o fato. Com margem de 30 minutos, a saída ainda não venceu às 06h10. (`CLAUDE.md:185`; `app/chamados/services/fabrica_da_celula.py:18`; `app/ponto/services/cartorio.py:828`) |
| A16 — instante limítrofe | Marco 18h, margem 30 minutos; observar 18h29m59s e 18h30. | Antes do vencimento não cobrar; no limite aplicar o mesmo juiz por instante. (`CLAUDE.md:185`; `app/ponto/services/cartorio.py:845`; `app/ponto/services/cartorio.py:846`) |
| A17 — vínculo trocado | Novo vínculo ativo hoje; célula do fato aponta antigo dentro de sua vigência. | Usar o vínculo do dia. Geradora fora da vigência não autoriza substituir silenciosamente pelo atual. (`app/escala/alimentacao.py:231`; `app/escala/alimentacao.py:272`) |
| A18 — célula sem ata | Escala vigente, célula sem ata; tentar emitir pergunta de marco. | Aguardar a ata e sinalizar sua ausência; não inventar marco faltante. (`app/chamados/services/disputa_emissao.py:819`; `app/chamados/services/disputa_emissao.py:833`) |
| A19 — limbo | `trabalha=None`, sem ata que declare outro tipo de dia. | Desconhecido não vira folga; a cobrança segue a autoridade do dia, e a pergunta de marco continua dependendo da ata. (`app/core/juizes.py:137`; `app/docs/CORTES.md:416`; `app/chamados/services/disputa_emissao.py:819`) |
| A20 — parcial sem período | Parcial aprovada sem período declarado; tentar perguntar o restante. | Encaminhar à lista administrativa; não julgar dentro/fora nem fabricar falta ou pergunta. (`app/docs/CORTES.md:319`; `app/chamados/services/disputa_emissao.py:847`) |
| A21 — vivos legítimos | Registro sem ação, ausência aguardando decisão, parcial na lista, pergunta viva sob outro chamado ou marco já decidido com ação administrativa. | Excluir cada classe legítima antes do RED; conservar seu dono e motivo. Não excluir apenas porque há histórico. (`app/docs/PROMPTS.md:1882`; `app/chamados/models.py:28`; `app/chamados/services/vigia_fio.py:382`; `app/chamados/services/pergunta_no_app.py:21`) |
| A22 — tranca e exportação | Comparar tranca com exportação; na exportada comparar trabalho e limbo. | Tranca pode calar por leitura sem quitar o fato. Exportação não é silêncio universal: trabalho segue perguntando; limbo entregue vai à Pauta DP. (`app/chamados/juizes.py:1138`; `app/chamados/services/fabrica_da_celula.py:167`) |
| A23 — passado | Introduzir a obra sem fato novo no estoque antigo. | Respeitar o horizonte e o passado como foto; não converter esta obra em backfill automático. (`app/docs/LEIS.md:145`) |

## B — SÍTIOS

**Não falta juiz para “morreu por leitura”: é C6.** C15 responde a coleção e C17 responde alcance; nenhum dos dois substitui C6. O C11 operacional do aval também não deve ser confundido com `_C11` do registro, que trata silêncio por ausência. (`app/core/juizes.py:266`; `app/core/juizes.py:271`; `app/core/juizes.py:275`; `app/core/juizes.py:277`; `app/docs/CORTES.md:305`)

| Pergunta | Juiz registrado |
|---|---|
| Chamado está vivo? | C1, `VIVOS`. (`app/core/juizes.py:261`) |
| Premissa morreu; pode fechar? | C3, `premissa_morta`, consumido pelos gates de fechamento. (`app/core/juizes.py:263`; `app/chamados/models.py:680`) |
| Pergunta está viva? | C6, `pergunta_viva`; cálculo em `_pergunta_viva_calc`. (`app/core/juizes.py:266`; `app/chamados/juizes.py:822`; `app/chamados/juizes.py:1135`) |
| Fábrica alcança o chamado? Por que não? | C12/C13, `motivo_fora_da_fabrica` e `retencao`. (`app/core/juizes.py:272`; `app/core/juizes.py:273`) |
| Há pergunta viva na coleção? | C15, `fio_vivo`. (`app/core/juizes.py:275`; `app/chamados/juizes.py:1233`) |
| Há porta alcançável? | C17, `porta_alcancavel_do_dia`. (`app/core/juizes.py:277`; `app/chamados/juizes.py:1341`) |
| Dia sem trabalho cala cobrança? | C18, `dia_sem_trabalho`. (`app/core/juizes.py:278`) |
| Vínculo, janela e validação | Autoridades já indicadas pelo lookup: vínculo do dia, janela da empresa e porta de validação. (`app/docs/JUIZES.md:35`; `app/docs/JUIZES.md:38`; `app/docs/JUIZES.md:98`) |

**Sítios de nascimento, perda e entrega a alcançar pela obra:** (`app/docs/PROMPTS.md:1875`; `app/docs/PROMPTS.md:1882`)

| Sítio fonte | Escrita/leitura e consequência |
|---|---|
| `ChamadoColaborador.abrir` | Trava colaborador, deduplica e pode renascer existente; resolve célula no nascimento. O evento explícito atual só atende `MODULOS_DE_TURNO`. (`app/chamados/models.py:863`; `app/chamados/models.py:882`; `app/chamados/models.py:913`; `app/chamados/models.py:938`) |
| `ChamadoColaborador.criar` | Outra porta de nascimento; cria e registra trilha sem evento explícito de pergunta no corpo. (`app/chamados/models.py:1015`; `app/chamados/models.py:1048`) |
| `reabrir` / ressuscitação | Reabertura grava estado e tenta restaurar perguntas; zero restauradas não cria pergunta inexistente. (`app/chamados/models.py:1382`; `app/chamados/models.py:1391`; `app/chamados/models.py:1423`; `app/chamados/models.py:1482`) |
| `retratar_duplicata` | Carimba identidade; o corpo não reconcilia a origem que perdeu o fio. (`app/chamados/services/fio.py:878`; `app/chamados/services/fio.py:891`) |
| Validação | Assinatura na porta única; três chamadas incluem os ramos de folga. Hora validada pode ter assinatura devolvida após recusa. (`app/chamados/services/validacao.py:127`; `app/chamados/services/validacao.py:263`; `app/chamados/services/validacao.py:290`; `app/chamados/services/validacao.py:396`; `app/chamados/services/validacao.py:425`) |
| `reapontar_resolvedora` | Testa identidade, protege destino concorrente e salva destino/lavra; precisa considerar a origem anterior. (`app/chamados/services/fio.py:784`; `app/chamados/services/fio.py:837`; `app/chamados/services/fio.py:845`) |
| `PerguntaDisputa.abrir` | Outra via de reapontamento: reutiliza identidade histórica e pode alterar célula e destino. Curar apenas `fio.reapontar_resolvedora` deixa esta irmã fora. (`app/chamados/models.py:2420`; `app/chamados/models.py:2424`; `app/chamados/models.py:2439`) |
| Fonte da pergunta | `perguntar_pela_celula` lê motivos, desfaz carimbo sem lastro e chama `perguntar`; esta usa as portas de disputa/pergunta. (`app/chamados/services/fabrica_da_celula.py:129`; `app/chamados/services/fabrica_da_celula.py:145`; `app/chamados/services/fabrica_da_celula.py:164`) |
| Normalização existente | Cartório distingue fio vivo, resposta humana, duplicata e ressuscitação; pula zero perguntas. Esse ciclo precisa ser reutilizado, não reproduzido como outro critério. (`app/ponto/services/cartorio.py:1591`; `app/ponto/services/cartorio.py:1595`; `app/ponto/services/cartorio.py:1597`; `app/ponto/services/cartorio.py:1599`; `app/ponto/services/cartorio.py:1622`; `CLAUDE.md:175`) |
| Morte por célula/marco | Porta de célula publica mudança de veredito/ata; cartório chama a fábrica somente nas células julgadas na passada. (`app/ponto/portas/celula.py:660`; `app/ponto/portas/celula.py:672`; `app/ponto/services/cartorio.py:985`; `app/ponto/services/cartorio.py:995`) |
| Morte por tranca | Fonte própria de encerramento; C6 pode responder morta sem novo carimbo na pergunta. (`app/chamados/services/competencia_trancada.py:33`; `app/chamados/services/competencia_trancada.py:178`; `app/chamados/juizes.py:1140`) |
| Consumidor existente | Consome eventos para retratar chamados e encerrar órfãs; atualmente não entrega perguntas pela fábrica. Acrescentar evento de nascimento sozinho é insuficiente. (`app/chamados/reconciliador.py:1326`; `app/chamados/reconciliador.py:1329`) |
| Detector | Abre, depois decide agrupamento administrativo e depois tenta emitir pergunta absorvendo exceção. Evento interno antecipado pode observar ato incompleto. (`app/ponto/management/commands/detectar_ausencias.py:693`; `app/ponto/management/commands/detectar_ausencias.py:745`; `app/ponto/management/commands/detectar_ausencias.py:787`; `app/ponto/management/commands/detectar_ausencias.py:802`) |
| Grade / C2 | Grade abre/reabre e retorna; C2 abre e depois costura a resolvedora. (`app/ponto/management/commands/reconciliar_grade.py:507`; `app/ponto/management/commands/reconciliar_grade.py:529`; `app/chamados/reconciliador.py:879`; `app/chamados/reconciliador.py:922`) |
| Cron e configuração | FIO MUDO ainda escreve; corrente usa `--apply` e comando permanece em `JUIZES_POR_VARREDURA`. Migração completa exige retirar essa responsabilidade de escrita e conservar vigia. (`app/chamados/management/commands/disparar_perguntas_competencia.py:280`; `app/config/crons.py:1495`; `app/config/crons.py:2007`; `CLAUDE.md:169`) |

**Campos envolvidos e seus donos de escrita/leitura:** (`CLAUDE.md:193`)

| Campos | Escritores/portas | Leitores relevantes |
|---|---|---|
| Chamado: identidade, contexto, célula | Nascimento por `abrir/criar`; resolução da célula nessas portas. (`app/chamados/models.py:913`; `app/chamados/models.py:1026`; `app/chamados/models.py:1048`) | Dia do fato no sinal; fábrica de órfãos; seleção do disparador. (`app/chamados/signals.py:178`; `app/chamados/services/disputa_emissao.py:683`; `app/chamados/management/commands/disparar_perguntas_competencia.py:141`) |
| `status_local`, `resolvido_em`, `via_encerramento` | Núcleo `_gravar_estado`; gates devem conservar a guarda de perguntas ancoradas. (`app/chamados/models.py:195`; `app/chamados/models.py:202`; `app/chamados/models.py:222`) | Sinais, fechamento/reabertura, fábrica e fila. (`app/chamados/signals.py:169`; `app/chamados/models.py:638`; `app/chamados/services/disputa_emissao.py:691`; `app/chamados/services/painel.py:186`) |
| Pergunta: `chamado_destino`, `data_evento`, `celula` | Nascimento/reuso e costura existente. (`app/chamados/models.py:2387`; `app/chamados/models.py:2424`; `app/chamados/models.py:2439`; `app/chamados/services/fio.py:831`; `app/chamados/services/fio.py:845`) | Identidade, C6, âncora, lavra e alcance. (`app/chamados/models.py:2381`; `app/chamados/juizes.py:1135`; `app/chamados/services/fio.py:97`; `app/chamados/veredito_lavrado.py:229`; `app/chamados/juizes.py:1341`) |
| Resposta e instante | Resposta humana, correção, resposta automática e recomposição de instante; `carimbar` também pode completar resposta. (`app/chamados/services/disputa_emissao.py:1282`; `app/chamados/services/disputa_emissao.py:1393`; `app/chamados/services/disputa_emissao.py:1428`; `app/chamados/services/disputa_emissao.py:1434`; `app/chamados/models.py:2576`) | C6, validação, cartório e app. (`app/chamados/juizes.py:1150`; `app/chamados/services/validacao.py:363`; `app/ponto/services/cartorio.py:1586`; `app/chamados/services/pergunta_no_app.py:53`) |
| `validada_em/por` | Validação/devolução; resposta nova e correção; ressuscitação em lote; compatibilidade em `carimbar`. (`app/chamados/services/validacao.py:131`; `app/chamados/services/validacao.py:188`; `app/chamados/services/disputa_emissao.py:1295`; `app/chamados/services/disputa_emissao.py:1393`; `app/chamados/models.py:1493`; `app/chamados/models.py:2581`) | Primeiro degrau C6, estado e materialização. (`app/chamados/juizes.py:1136`; `app/chamados/models.py:2491`; `app/chamados/services/validacao.py:422`) |
| `materializada_em/por`, `via_resolucao` | `_escrever_carimbo`, `carimbar`, identidade, descarimbo individual/lote. (`app/chamados/models.py:2545`; `app/chamados/models.py:2551`; `app/chamados/models.py:2614`; `app/chamados/models.py:2635`; `app/chamados/models.py:2645`) | C6, fábrica, ressuscitação, seleção aproximada de órfãs e vigia. (`app/chamados/juizes.py:1145`; `app/chamados/juizes.py:1171`; `app/chamados/services/fabrica_da_celula.py:118`; `app/chamados/models.py:1423`; `app/chamados/services/disputa_emissao.py:909`; `app/chamados/services/vigia_fio.py:398`) |
| Lavra da pergunta | Porta `lavrar` e persistência em lote. (`app/chamados/veredito_lavrado.py:197`; `app/chamados/veredito_lavrado.py:211`) | Raio-X e controle de frescor da própria lavra. (`app/chamados/services/raiox.py:173`; `app/chamados/veredito_lavrado.py:173`) |
| Célula: escala/DNA, ata/veredito/protestos, julgamento | Portas de geração/regeneração; lavra; carimbo. (`app/ponto/portas/celula.py:100`; `app/ponto/portas/celula.py:260`; `app/ponto/portas/celula.py:747`; `app/ponto/portas/celula.py:825`) | Vínculo do dia, motivos, C6, impressão e vigias. (`app/escala/alimentacao.py:272`; `app/chamados/services/fabrica_da_celula.py:34`; `app/chamados/juizes.py:1181`; `app/ponto/services/cartorio.py:1348`; `app/chamados/services/vigia_fio.py:377`) |

Os chamadores de costura/retração incluem dedup, migração, gate de encerramento, reconciliador, declaração e limbo; existe também reversão de reata. Uma alteração na porta alcança esses caminhos. (`app/chamados/management/commands/dedup_perguntas_fato.py:88`; `app/chamados/services/fio.py:943`; `app/chamados/services/fio.py:947`; `app/chamados/models.py:420`; `app/chamados/reconciliador.py:1570`; `app/chamados/reconciliador.py:1582`; `app/chamados/services/declaracao_texto.py:320`; `app/chamados/services/limbo_folgas.py:243`; `app/chamados/services/limbo_folgas.py:276`; `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:122`)

## C — DIFF PROPOSTO EM TEXTO

**Não fechei um diff integral que possa afirmar realizar os seis gatilhos.** Não há ausência de juiz que justifique inventar um ou declarar PAREI de lei: C6, C15, C17 e as portas existentes já respondem às perguntas relevantes. A insuficiência é do diff demonstrado nesta resposta. (`app/core/juizes.py:263`; `app/core/juizes.py:266`; `app/core/juizes.py:275`; `app/core/juizes.py:277`; suficiência integral de uma implementação: [não verificado])

O menor candidato concreto abaixo reutiliza a fonte de emissão e o consumidor existente, mas **é reprovado pelos contraexemplos D01–D05**. Portanto, não o apresento como cura pronta, nem proponho retirar o escritor do cron com ele. (`app/chamados/services/fabrica_da_celula.py:145`; `app/chamados/reconciliador.py:1326`; `app/config/crons.py:2007`; validade do candidato: [não verificado])

```diff
--- a/app/chamados/models.py
+++ b/app/chamados/models.py
@@ -938,4 +938,4 @@
                 from chamados.reconciliador import MODULOS_DE_TURNO as _MT
-                if (_mo or '') in _MT and ch.celula_id is not None:
+                if ((_mo or '') in _MT or _mo == 'batida_ausente') and ch.celula_id is not None:
                     from ponto.portas.celula import emitir_evento_celula
                     emitir_evento_celula(ch.celula)
--- a/app/chamados/reconciliador.py
+++ b/app/chamados/reconciliador.py
@@ -1332,1 +1332,8 @@
                     adiar = _orf['adiadas'] > 0
+                    from escala.models import CelulaDia
+                    from chamados.services.fabrica_da_celula import perguntar_pela_celula
+                    celula_ids = {e.celula_id for e in lote}
+                    for cel in CelulaDia.objects.filter(
+                            pk__in=celula_ids
+                    ).select_related('colaborador', 'colaborador__empresa'):
+                        perguntar_pela_celula(cel)
```

Para chegar ao diff pedido, ainda seria necessário consolidar a normalização existente e conectar os cinco atos ao estado final de cada operação, incluindo origem/destino e o reapontamento dentro de `PerguntaDisputa.abrir`; converter o lote antes disso seria meia correção. Essa é exigência de execução do contrato existente, não proposta de juiz novo. (`app/ponto/services/cartorio.py:1591`; `app/chamados/models.py:2439`; `app/docs/PROMPTS.md:1882`; `C

[truncado em 20000 de 25490 caracteres]

## 05/11 18:41 · sessao 026a98dbf789 · - · - · origem=Ronald (tmux)

A porta toma **Empresa → Colaboradores em PK crescente → Ausencias em PK crescente**, usando `FOR UPDATE OF self` para Ausencia; as decisões de identidade/empresa/entrega vêm depois dessas aquisições. (`app/ponto/services/ausencia.py:638`, `app/ponto/services/ausencia.py:641`, `app/ponto/services/ausencia.py:644`, `app/ponto/services/ausencia.py:647`, `app/ponto/services/ausencia.py:654`.)

**Ciclo relevante encontrado, por análise estática: admissão × porta.**

- A admissão abre transação externa, cria Colaborador e informa empresa. (`app/colaboradores/views.py:1175`, `app/colaboradores/views.py:1182`, `app/colaboradores/views.py:1186`.)
- A criação delega a `Colaborador.objects.create`; se não existe matrícula, `Colaborador.save` toma `FOR UPDATE` no colaborador de maior matrícula e depois insere o novo colaborador; a FK aponta Empresa. (`app/colaboradores/services/colaborador.py:93`, `app/colaboradores/models.py:427`, `app/colaboradores/models.py:429`, `app/colaboradores/models.py:431`, `app/colaboradores/models.py:434`, `app/colaboradores/models.py:439`, `app/colaboradores/models.py:321`.)
- Se esse colaborador de maior matrícula é alvo H06 da Empresa E, a admissão pode segurar Cmax enquanto a porta segura E e espera Cmax; a verificação FK da inserção pede proteção da mesma E. Portanto há inversão **Cmax → E** versus **E → Cmax** nas mesmas linhas. A semântica específica PostgreSQL de `KEY SHARE`, momento efetivo da checagem FK e execução do ciclo: **[nao verificado]**; os caminhos que produzem a inversão estão nas referências acima e na aquisição da porta. (`app/ponto/services/ausencia.py:639`, `app/ponto/services/ausencia.py:641`.)

**Outro ciclo potencial: transferência × reversão antiga.**

Uma reversão do arquivo de E toma E antes de C, mesmo se C hoje pertence a O; transferência O→E atualiza C dentro de transação e modifica a FK para E, produzindo C→E antes de a porta alcançar sua guarda de empresa. (`app/ponto/services/ausencia.py:639`, `app/ponto/services/ausencia.py:641`, `app/ponto/services/ausencia.py:660`, `app/colaboradores/views.py:1384`, `app/colaboradores/views.py:1386`, `app/colaboradores/views.py:1388`, `app/colaboradores/services/colaborador.py:76`, `app/colaboradores/models.py:321`.) Ocorrência/checagem FK efetiva: **[nao verificado]**.

**Sequência adicional antes do commit:** [nao verificado]

1. INSERT da linha do ato em LogAuditoria antes das escritas de Ausencia; `objeto_id` é inteiro, e a FK de LogAuditoria é para `auth.User`. (`app/ponto/services/ausencia.py:688`, `app/core/observ.py:133`, `app/core/observ.py:134`, `app/core/models.py:295`, `app/core/models.py:298`.)
2. Aplicar chama edição e `Ausencia.save`; reverter chama reposição e `Ausencia.save(update_fields=...)`. (`app/ponto/services/ausencia.py:700`, `app/ponto/services/ausencia.py:508`, `app/ponto/services/ausencia.py:711`, `app/ponto/services/ausencia.py:532`.)
3. `post_save` síncrono chama cartório para os dias. (`app/ponto/signals.py:143`, `app/ponto/signals.py:147`, `app/ponto/signals.py:88`, `app/ponto/signals.py:93`.)
4. Se houver emissão, cartório chama emissor; este chama `ChamadoColaborador.abrir`, que readquire C `FOR UPDATE` e depois cria/encontra Chamado. Chamado tem FK C e CelulaDia. (`app/ponto/services/cartorio.py:1740`, `app/ponto/services/cartorio.py:1778`, `app/ponto/management/commands/detectar_ausencias.py:693`, `app/chamados/models.py:863`, `app/chamados/models.py:866`, `app/chamados/models.py:932`, `app/chamados/models.py:32`, `app/chamados/models.py:44`.)
5. Cartório grava CelulaDia por `carimbar`; `salvar_e_emitir` envolve UPDATE Celula e eventual INSERT EventoCelula no mesmo atomic. EventoCelula tem IDs inteiros, sem FK. (`app/ponto/services/cartorio.py:1822`, `app/ponto/portas/celula.py:810`, `app/ponto/portas/celula.py:815`, `app/ponto/portas/celula.py:647`, `app/ponto/portas/celula.py:648`, `app/ponto/portas/celula.py:650`, `app/escala/models.py:1853`.)
6. Trilha por Ausencia e foto final antecedem a saída do atomic. Consumidor de ausência está em `on_commit`, portanto suas escritas pertencem ao pós-commit. (`app/ponto/services/ausencia.py:706`, `app/ponto/services/ausencia.py:712`, `app/ponto/services/ausencia.py:716`, `app/ponto/signals.py:272`.)

**Possível inversão adicional com CelulaDia:** cartório externo grava `CelulaDia.ausencia` sob atomic; a FK aponta Ausencia. Se muda NULL→A, pode segurar Celula antes da checagem FK de A; a porta segura A e seu signal tenta gravar a mesma Celula. (`app/ponto/portas/celula.py:810`, `app/ponto/portas/celula.py:647`, `app/ponto/portas/celula.py:648`, `app/escala/models.py:1796`, `app/ponto/services/ausencia.py:644`, `app/ponto/signals.py:147`.) Ciclo PostgreSQL executado: **[nao verificado]**.

**Outros escritores/caminhos identificados:**

| Caminho | Ordem visível/condicional |
|---|---|
| `ExportacaoDominio.registrar` | Empresa → INSERT ExportacaoDominio dentro do mesmo atomic. (`app/folha/models.py:179`, `app/folha/models.py:181`, `app/folha/models.py:186`.) |
| Admissão | Cmax FOR UPDATE → INSERT Cnovo → FK Empresa. (`app/colaboradores/views.py:1175`, `app/colaboradores/models.py:431`, `app/colaboradores/models.py:439`, `app/colaboradores/models.py:321`.) |
| Edição cadastral com mudança real de empresa | UPDATE Colaborador → verificação FK Empresa, dentro do atomic. (`app/colaboradores/views.py:1313`, `app/colaboradores/views.py:1340`, `app/colaboradores/views.py:1342`, `app/colaboradores/services/colaborador.py:76`, `app/colaboradores/models.py:321`.) |
| Transferência | UPDATE Colaborador → FK Empresa destino. (`app/colaboradores/views.py:1384`, `app/colaboradores/views.py:1386`, `app/colaboradores/views.py:1388`.) |
| `criar_ausencia` | INSERT Ausencia → Colaborador se emissão no signal; FK Colaborador na criação também exige avaliação. (`app/ponto/services/ausencia.py:342`, `app/ponto/services/ausencia.py:417`, `app/ponto/services/ausencia.py:418`, `app/ponto/signals.py:147`, `app/chamados/models.py:866`.) |
| Aprovação de irmãos | Ausencia → [Colaborador na emissão] → próximo irmão, sob atomic externo. (`app/ponto/views.py:2638`, `app/ponto/views.py:2640`, `app/ponto/services/ausencia.py:1096`, `app/ponto/services/ausencia.py:1020`.) |
| Regularização | Várias criações de Ausencia sob atomic externo, com aquisições condicionais de Colaborador. (`app/ponto/services/regularizacao.py:89`, `app/ponto/services/regularizacao.py:111`, `app/ponto/services/regularizacao.py:118`.) |
| Materialização | Criação de Ausencia sob atomic externo; signal pode adquirir Colaborador. (`app/chamados/services/materializacao.py:1145`, `app/chamados/services/materializacao.py:1151`, `app/chamados/services/materializacao.py:484`, `app/ponto/signals.py:147`.) |
| Abandono provisório | Laço de criação de Ausencia sob atomic do comando; signal pode adquirir Colaborador. (`app/ponto/management/commands/marcar_abandono_provisorio.py:22`, `app/ponto/management/commands/marcar_abandono_provisorio.py:70`, `app/ponto/management/commands/marcar_abandono_provisorio.py:76`.) |
| Veredito da célula | Atomic externo → criar Ausencia → signal condicional C. (`app/ponto/services/veredito_celula.py:204`, `app/ponto/services/veredito_celula.py:315`.) |
| Sincronização de férias | Cria/apaga ausências, potencialmente dentro dos atomics do agendamento/correção; criação tem signal condicional C. (`app/ponto/services/ausencia.py:1375`, `app/ponto/services/ausencia.py:1387`, `app/ponto/services/ausencia.py:1398`, `app/ferias/services.py:315`, `app/ferias/services.py:375`.) |

Exaustividade de todas as combinações transitivas de chamadores/sinais/FKs e modos exatos de locks da instalação: **[nao verificado]**.

## 05/11 18:41 · sessao 6171dd173358 · - · - · origem=Ronald (tmux)

Revisão estática; nenhum teste ou mutante executado: **[nao verificado]**. [nao verificado]

**Mordida dos 12 testes `PortaUnicaDoAtoV5`** [nao verificado]

Os testes de recusa agora cobram motivo específico, ausência de “derivou” e zero UPDATE contado, corrigindo o falso positivo de status da v4. (`app/ponto/tests/test_h06_reversao_guardas.py:422`, `app/ponto/tests/test_h06_reversao_guardas.py:425`, `app/ponto/tests/test_h06_reversao_guardas.py:426`, `app/ponto/tests/test_h06_reversao_guardas.py:427`.)

| Teste | O que morde | Mutante de produção que permanece verde no teste isolado | [nao verificado]
|---|---|---|
| Reuso R1 após reverter/reaplicar, linha 442 | Remover recusa por consumo: R1 volta a encontrar 420, portanto estado sozinho não basta. | Remover `select_for_update` de Colaborador, mantendo leitura/guarda; execução é serial. (`app/ponto/tests/test_h06_reversao_guardas.py:442`, `app/ponto/tests/test_h06_reversao_guardas.py:453`, `app/ponto/services/ausencia.py:641`, `app/ponto/services/ausencia.py:675`.) |
| Arquivo não emitido, linha 460 | Remover exigência de hash aplicado; altera indentação dos mesmos dados e exige “nao consta”. | Remover guarda de consumo; este arquivo não possui sequer aplicação registrada. (`app/ponto/tests/test_h06_reversao_guardas.py:469`, `app/ponto/tests/test_h06_reversao_guardas.py:470`, `app/ponto/services/ausencia.py:672`, `app/ponto/services/ausencia.py:675`.) |
| Uma trilha por ato, linha 474 | Remover emissão de aplicação/reversão faz a contagem falhar. | **Remover guarda de consumido:** segunda reversão ainda falha porque encontra 720 contra grade 420; teste só exige `CommandError`. (`app/ponto/tests/test_h06_reversao_guardas.py:480`, `app/ponto/tests/test_h06_reversao_guardas.py:482`, `app/ponto/services/ausencia.py:658`, `app/ponto/services/ausencia.py:675`.) |
| Entrega intercalada, linha 487 | Retirar ambas verificações de entrega permite edição. | Remover trava Empresa: entrega ocorre serialmente antes da porta, e consultas continuam recusando. (`app/ponto/tests/test_h06_reversao_guardas.py:492`, `app/ponto/tests/test_h06_reversao_guardas.py:496`, `app/ponto/services/ausencia.py:639`, `app/ponto/services/ausencia.py:647`, `app/ponto/services/ausencia.py:663`.) |
| Reverter/tipo, linha 501 | Retirar `tipo` da identidade. | Remover trava Colaborador. (`app/ponto/tests/test_h06_reversao_guardas.py:504`, `app/ponto/tests/test_h06_reversao_guardas.py:505`, `app/ponto/services/ausencia.py:546`, `app/ponto/services/ausencia.py:641`.) |
| Reverter/status, linha 507 | Retirar `status` da identidade. | Remover trava Colaborador. (`app/ponto/tests/test_h06_reversao_guardas.py:510`, `app/ponto/tests/test_h06_reversao_guardas.py:511`, `app/ponto/services/ausencia.py:546`, `app/ponto/services/ausencia.py:641`.) |
| Reverter/minutos, linha 513 | Retirar comparação dos minutos esperados. | Remover trava Colaborador. (`app/ponto/tests/test_h06_reversao_guardas.py:516`, `app/ponto/tests/test_h06_reversao_guardas.py:517`, `app/ponto/services/ausencia.py:658`, `app/ponto/services/ausencia.py:641`.) |
| Aplicar/minutos, linha 519 | Retirar comparação dos minutos antes da edição; motivo posterior não atende regex. | Remover trava Ausencia, mantendo leitura; mutação é serial após censo. (`app/ponto/tests/test_h06_reversao_guardas.py:521`, `app/ponto/tests/test_h06_reversao_guardas.py:522`, `app/ponto/services/ausencia.py:644`, `app/ponto/services/ausencia.py:658`.) |
| Aplicar/transferência, linha 527 | Retirar guarda da empresa atual. | Remover trava Colaborador, mantendo leitura. (`app/ponto/tests/test_h06_reversao_guardas.py:530`, `app/ponto/tests/test_h06_reversao_guardas.py:531`, `app/ponto/services/ausencia.py:641`, `app/ponto/services/ausencia.py:660`.) |
| Reverter/transferência, linha 535 | Retirar guarda da empresa atual. | Remover trava Colaborador, mantendo leitura. (`app/ponto/tests/test_h06_reversao_guardas.py:539`, `app/ponto/tests/test_h06_reversao_guardas.py:540`, `app/ponto/services/ausencia.py:641`, `app/ponto/services/ausencia.py:660`.) |
| Aplicar/status, linha 545 | **Morde a correção pretendida:** retirar status da identidade leva à pós-checagem “derivou”, expressamente proibida. | Remover trava Ausencia; cenário continua serial. (`app/ponto/tests/test_h06_reversao_guardas.py:547`, `app/ponto/tests/test_h06_reversao_guardas.py:548`, `app/ponto/tests/test_h06_reversao_guardas.py:426`, `app/ponto/services/ausencia.py:703`.) |
| SQL `OF self`, linha 553 | Remover `OF self` ou introduzir JOIN de Colaborador nesse SQL. | Travar `pk=-1`, depois ler as Ausencias reais sem trava: o teste confere formato/tabela, não PK ou linhas retornadas. (`app/ponto/tests/test_h06_reversao_guardas.py:559`, `app/ponto/tests/test_h06_reversao_guardas.py:562`, `app/ponto/tests/test_h06_reversao_guardas.py:563`, `app/ponto/services/ausencia.py:644`.) |

**Mordida dos 6 testes `OrdemDeTravaDuasConexoesV5`** [nao verificado]

| Teste | Mutante sobrevivente | [nao verificado]
|---|---|
| Colaborador preso, linha 708 | Remover trava explícita Ausencia: ainda para em Colaborador com Empresa tomada e Ausencia livre. (`app/ponto/tests/test_h06_reversao_guardas.py:709`, `app/ponto/tests/test_h06_reversao_guardas.py:711`, `app/ponto/tests/test_h06_reversao_guardas.py:712`, `app/ponto/services/ausencia.py:644`.) |
| Ausencia presa, linha 717 | **Travar Ausencia errada/vazia e ler a real sem trava**: a porta para no UPDATE real posterior, já tendo Empresa e Colaborador; o teste observa exatamente o mesmo estado. (`app/ponto/tests/test_h06_reversao_guardas.py:718`, `app/ponto/tests/test_h06_reversao_guardas.py:720`, `app/ponto/tests/test_h06_reversao_guardas.py:721`, `app/ponto/services/ausencia.py:700`, `app/ponto/services/ausencia.py:508`.) |
| Empresa presa, linha 724 | Remover travas Colaborador/Ausencia: enquanto espera Empresa, ambas continuam livres; depois conclui. (`app/ponto/tests/test_h06_reversao_guardas.py:725`, `app/ponto/tests/test_h06_reversao_guardas.py:727`, `app/ponto/tests/test_h06_reversao_guardas.py:728`, `app/ponto/services/ausencia.py:641`, `app/ponto/services/ausencia.py:644`.) |
| Reverter mesma ordem, linha 731 | Remover trava Ausencia: só inspeciona parada em Colaborador. (`app/ponto/tests/test_h06_reversao_guardas.py:734`, `app/ponto/tests/test_h06_reversao_guardas.py:737`, `app/ponto/services/ausencia.py:644`.) |
| Outra Empresa presa, linha 742 | Remover trava Empresa completamente: teste é apenas controle negativo. (`app/ponto/tests/test_h06_reversao_guardas.py:743`, `app/ponto/tests/test_h06_reversao_guardas.py:744`, `app/ponto/services/ausencia.py:639`.) |
| Duas reversões mesmo arquivo, linha 749 | **Remover trava Empresa:** Colaborador ainda serializa ambas, a segunda espera outra trava e depois encontra consumo. O detector aceita qualquer `Lock`. (`app/ponto/tests/test_h06_reversao_guardas.py:667`, `app/ponto/tests/test_h06_reversao_guardas.py:669`, `app/ponto/tests/test_h06_reversao_guardas.py:789`, `app/ponto/tests/test_h06_reversao_guardas.py:791`, `app/ponto/services/ausencia.py:639`, `app/ponto/services/ausencia.py:641`, `app/ponto/services/ausencia.py:671`.) |

**Mutante conjunto relevante:** substituir a consulta real de trava Ausencia por uma `SELECT … FOR UPDATE OF ponto_ausencia WHERE pk=-1`, seguida de leitura normal das Ausencias reais, pode conservar **todos** os testes novos verdes: o caso SQL vê a consulta esperada; o caso da Ausencia presa bloqueia no UPDATE implícito. Os casos restantes têm serialização por Empresa/Colaborador e não verificam uma pausa anterior ao UPDATE. Isso é contrafactual estático, execução **[nao verificado]**. (`app/ponto/tests/test_h06_reversao_guardas.py:559`, `app/ponto/tests/test_h06_reversao_guardas.py:667`, `app/ponto/tests/test_h06_reversao_guardas.py:717`, `app/ponto/services/ausencia.py:644`, `app/ponto/services/ausencia.py:508`.)

A coordenação usual evita depender apenas de “dormir e torcer”: a thread seguradora sinaliza após o SELECT, e a observadora espera `wait_event_type='Lock'`. Entretanto não confere relação/linha, SQL aguardado ou PID bloqueador. Portanto o falso verde acima é estrutural, não requer sorte de agendamento. (`app/ponto/tests/test_h06_reversao_guardas.py:622`, `app/ponto/tests/test_h06_reversao_guardas.py:625`, `app/ponto/tests/test_h06_reversao_guardas.py:667`.)

Há fragilidade adicional de tempo: os retornos de `pronto.wait(10)` e `gravou.wait(20)` não são exigidos como verdadeiros; a seguradora solta sozinha após 30 segundos; joins não cobram `is_alive=False`. Uma máquina excessivamente atrasada pode mudar o instante observado; ocorrência prática **[nao verificado]**. (`app/ponto/tests/test_h06_reversao_guardas.py:626`, `app/ponto/tests/test_h06_reversao_guardas.py:634`, `app/ponto/tests/test_h06_reversao_guardas.py:702`, `app/ponto/tests/test_h06_reversao_guardas.py:780`.)

**Helper de UPDATE**

Sim, pode esconder escrita da própria porta. Um mutante que execute um UPDATE antes de `travar_entrega(emp)`, preserve a posterior recusa com motivo esperado e deixe o `atomic` desfazer a mudança terá seu UPDATE descartado da contagem. O helper exclui todos os SQLs anteriores à primeira trava Empresa, sem distinguir autor; rollback não remove SQL de `CaptureQueriesContext`. (`app/ponto/tests/test_h06_reversao_guardas.py:402`, `app/ponto/tests/test_h06_reversao_guardas.py:409`, `app/ponto/tests/test_h06_reversao_guardas.py:410`, `app/ponto/services/ausencia.py:638`, `app/ponto/services/ausencia.py:639`.)

**Formato e limites da porta**

O produtor escreve objeto com `ato/empresa_id/mes/ano/mes_intacto/linhas`, e a porta lê bytes e exige esse objeto. Os dois leitores de teste que antes usavam lista foram migrados para `['linhas']`; não identifiquei outro leitor H06 na busca da cópia. (`app/ponto/management/commands/rederivar_falta_declarada.py:122`, `app/ponto/services/ausencia.py:620`, `app/ponto/services/ausencia.py:626`, `app/ponto/tests/test_h06_rederivar_falta_declarada.py:92`, `app/ponto/tests/test_h06_reversao_guardas.py:139`.)

Arquivos antigos em lista são recusados antes da transação, mesmo que fossem reversões legítimas anteriores; existência deles em produção **[nao verificado]**. (`app/ponto/services/ausencia.py:628`, `app/ponto/services/ausencia.py:638`.)

A porta valida presença de chaves, `dict/list` e igualdade com a identidade fornecida; não valida tipos dos valores, PKs únicos nem o universo `falta/aprovada/720/com chamado/grade>0`. Esses critérios residem no censo. Um chamador direto pode construir um ato fora desse universo e ainda satisfazer igualdade/empresa/competências, desde que a edição resulte na `grade` fornecida. Chamadores atuais encontrados são somente as duas direções do comando. (`app/ponto/services/ausencia.py:626`, `app/ponto/services/ausencia.py:654`, `app/ponto/services/ausencia.py:703`, `app/ponto/management/commands/rederivar_falta_declarada.py:55`, `app/ponto/management/commands/rederivar_falta_declarada.py:65`, `app/ponto/management/commands/rederivar_falta_declarada.py:126`, `app/ponto/management/commands/rederivar_falta_declarada.py:148`.)

A exigência de motivo segue repetida no comando, porta e reposição excepcional. (`app/ponto/management/commands/rederivar_falta_declarada.py:93`, `app/ponto/services/ausencia.py:615`, `app/ponto/services/ausencia.py:528`.)

## 05/11 18:40 · sessao e2bb77d53088 · - · - · origem=Ronald (tmux)

Achados para consolidação, todos por inspeção estática; execução e ocorrência em produção [nao verificado].

1. **Porta única, com ressalvas concretas.** As decisões de entrega, identidade, empresa atual, estado esperado e aplicado/consumido estão centralizadas em `app/ponto/services/ausencia.py:647`, `:654`, `:658`, `:660`, `:663`, `:666`. Os dois caminhos do comando apenas a chamam em `app/ponto/management/commands/rederivar_falta_declarada.py:126` e `:148`. Contudo, o motivo continua validado em três lugares: comando `:93`, porta `app/ponto/services/ausencia.py:615`, reposição `:528`. Usuário existente é validado apenas no comando `:90`; a porta não exige `por` existente, e `evento` aceita ator sem PK (`app/core/observ.py:118`). O universo elegível continua decidido pelo censo — falta/aprovada/720/chamado/janela/grade positiva e diferente — em comando `:55`, `:65`, `:69`; a porta confere igualdade com o censo, sem repetir esse universo (`app/ponto/services/ausencia.py:626`, `:654`, `:658`).

2. **Não localizei segundo escritor de produção das duas ações.** As ações são definidas em `app/ponto/services/ausencia.py:547` e `:548`, e emitidas pela chamada comum em `:688`. A reposição continua função pública capaz de salvar apenas exigindo motivo (`:528`–`:532`), mas seu único chamador de produção localizado é a própria porta (`:711`); isso é superfície de desvio futura, não evidência de um segundo caminho existente. A declaração de escritor autoriza o arquivo inteiro (`app/core/portas.py:485`), portanto edição/reposição nesse arquivo não constituem escritor fora da porta declarada.

3. **Unicidade é por serialização da Empresa, sem constraint de hash.** A transação trava Empresa antes da consulta (`app/ponto/services/ausencia.py:638`–`:639`); consulta auditoria por ação, modelo `Empresa`, `objeto_id` e `valor_depois__hash` (`:575`–`:579`); aplicado/consumido são decididos em `:666`–`:676`. O modelo não declara unicidade por hash (`app/core/models.py:309`–`:316`). Duas chamadas pela porta, para a mesma empresa/hash, esperam a mesma Empresa; a segunda observa a trilha da primeira após o commit (`app/ponto/services/ausencia.py:639`, `:666`, `:671`). Reaplicar hash já consumido também recusa, porque o registro aplicado permanece (`:668`–`:669`). Isolamento efetivo/configuração do banco [nao verificado].

4. **Falha da trilha tem dois comportamentos.** `evento` usa savepoint e engole exceções, retornando `None` (`app/core/observ.py:105`, `:133`–`:139`). A porta testa o retorno da linha do ato e aborta antes dos saves (`app/ponto/services/ausencia.py:688`–`:695`). As linhas individuais continuam best-effort, pois seus retornos são ignorados (`:533`–`:535`, `:706`–`:709`): o negócio pode confirmar sem elas. O arquivo já foi criado antes da lavra (`:686`–`:687`), portanto uma falha posterior pode deixar arquivo sem ato confirmado; ele será recusado por ausência da trilha aplicada (`:672`–`:674`). `LogAuditoria.objeto_id` é inteiro, sem FK para Empresa (`app/core/models.py:298`); a FK real do insert é `usuario → auth.User` (`:295`, `app/core/observ.py:135`).

Mapa de escrita/leitura para Q5:

| Campo/valor | Escrita | Leitura relevante |
|---|---|---|
| `Ausencia.colaborador_id` | Criação `app/ponto/services/ausencia.py:342`, `:418` | Censo comando `:55`; identidade serviço `:546`; trava/empresa `:642`, `:660` |
| `tipo` | Criação serviço `:342`; edição `:497` | Censo comando `:55`; identidade serviço `:546`; derivação modelo `app/ponto/models.py:877`; parcialidade `app/ponto/turnos.py:2598` |
| `status` | Criação serviço `:344`; transição `:1009`, persistência `:1020`; rejeição férias em lote `:1466`–`:1468` | Censo comando `:55`; identidade serviço `:546`; derivação modelo `:877` |
| `data/data_fim` | Criação serviço `:342`–`:343`; `derivar_data_fim :66`–`:78`; edição `:498`–`:499`; modelo `app/ponto/models.py:901`, `:907` | Censo comando `:57`, `:61`; identidade serviço `:546`; competências `:562`–`:566`; expansão `app/folha/export.py:70`; grade modelo `:882`–`:885` |
| `minutos` | Criação serviço `:343`; edição `:507`; grade modelo `:887`; transição serviço `:1019`–`:1020`; reversão `:531`–`:532` | Censo comando `:56`, `:69`; estado esperado serviço `:658`; pós-checagem `:703`; rateio `app/folha/export.py:121`; financeiros `app/ponto/services/fechamento.py:665`, `:685`; crédito `app/ponto/services/credito_parcial.py:24`; efeitos `app/ponto/services/efeito_ausencia.py:44`, `:51`; parcialidade `app/ponto/turnos.py:2600`; apresentação `app/templates/ponto/ausencias.html:163` |
| `dias_corridos` | Criação serviço `:342`; edição `:500`; modelo `:901` | Aplicar serviço `:700`; reconstrução do fim `app/ponto/models.py:902`; HTML `app/templates/ponto/ausencias.html:160` |
| `chamado_origem` | Porta serviço `:352`–`:353`; produtores materialização `app/chamados/services/materializacao.py:490`, `:526`; FK `SET_NULL` modelo `:786`–`:788` | Censo comando `:56`; auditoria criação serviço `:447` |
| `Colaborador.empresa_id` | Porta `app/colaboradores/services/colaborador.py:61`, `:76`, `:93`; admissão `app/colaboradores/views.py:1186`; edição `:1313`, `:1342`; transferência `:1386`, `:1388` | Censo comando `:55`; porta H06 serviço `:643`, `:660`; lista `app/ponto/services/fechamento_lista.py:103`; foto `app/ponto/management/commands/carimbo_gravado.py:53` |
| `ExportacaoDominio` | `registrar`: `app/folha/models.py:181`, `:186`–`:190`; `invalidar :158`–`:162` | Juiz `app/folha/export.py:593`–`:595`; vigência `app/folha/models.py:134`–`:137`, `:216`–`:225` |
| `LogAuditoria.valor_depois` | Escritor genérico `app/core/observ.py:134`–`:137`; payload ato `app/ponto/services/ausencia.py:691`–`:692`; individuais `:535`, `:709` | Hash aplicado/consumido `:578`; leitor de interface genérico e outros consumidores existem, mas exaustividade de todos os leitores [nao verificado] |
| Arquivo novo | Payload comando `:122`–`:124`; identidade serviço `:551`–`:554`; JSON ordenado bytes `:618`; `xb :686`–`:687` | `rb`/JSON `:620`–`:623`; validação dict com `ato,empresa_id,mes,ano,mes_intacto,linhas`, linhas com identidade+PK+antes+grade `:626`–`:632`; hash dos bytes `:633` |

Classes para Q7:

- **Segundo juiz:** H12 usa o mesmo predicado em `app/folha/export.py:54`–`:60`, `:870` e `app/ponto/services/fechamento_lista.py:109`; janela delega em `app/folha/export.py:47`–`:51`; autoridades registradas em `app/core/juizes.py:173`, `:298`–`:299`, `:312`.
- **Segundo escritor fora da porta:** novas escritas de Ausencia permanecem no arquivo autorizado (`app/core/portas.py:485`, `app/ponto/services/ausencia.py:508`, `:532`); ED permanece no arquivo autorizado (`app/core/portas.py:504`, `app/folha/models.py:186`).
- **Relógio:** novo `_selo` nomeia arquivo na borda (`app/ponto/management/commands/rederivar_falta_declarada.py:44`–`:46`, `:120`–`:121`); caminhos internos preexistentes ainda leem relógio (`app/ponto/models.py:873`, `app/ponto/signals.py:122`, `app/ponto/services/cartorio.py:1965`, `:1986`, `:1989`).
- **Fonte única:** materialização omite minutos (`app/chamados/services/materializacao.py:484`–`:492`, `:520`–`:528`); mapa só tipos (`app/chamados/models.py:3007`–`:3010`); modelo e censo chamam o mesmo juiz diário (`app/ponto/models.py:885`, comando `:63`). Rateio uniforme permanece (`app/folha/export.py:121`).
- **Parcialidade antiga:** cobertura continua com 480 (`app/ponto/turnos.py:2580`, `:2598`–`:2600`), enquanto financeiros usam qualquer minutos positivo (`app/ponto/services/fechamento.py:665`, `:685`); aberta/preexistente.
- **Cadastro atual no passado:** corte continua vindo do cadastro atual (`app/ponto/janelas.py:50`), inclusive no censo (`comando :54`), competências tocadas (`serviço :561`, `:564`) e lista (`app/ponto/services/fechamento_lista.py:104`).
- **Pouso/pré-requisito:** medida 09/10/11, preservação09 e pouso antes da exportação11 são exigidos (`app/docs/PROMPTS.md:1860`–`:1861`); cumprimento [nao verificado]. A foto cobre FechamentoMensal e exclui campos de controle (`app/ponto/management/commands/carimbo_gravado.py:29`–`:36`, `:53`–`:55`), sem comparar o TXT.

## 05/11 18:40 · sessao 39c9008e35b0 · - · - · origem=Ronald (tmux)

Censo somente por leitura; não há diff completo defensável apenas acrescentando hooks à abertura. O evento precisa alcançar a fonte existente e preservar os donos administrativos já definidos. (`app/chamados/services/fabrica_da_celula.py:145`; `app/chamados/services/pergunta_no_app.py:21`)

| Sítio | Evidência e consequência |
|---|---|
| Abertura deduplicada | `abrir` trava colaborador dentro de `atomic`; devolve existente vivo, ou tenta renascer encerrado, antes de retornar `criado=False`. O nascimento escreve pela porta e registra trilha. (`app/chamados/models.py:863`; `app/chamados/models.py:882`; `app/chamados/models.py:932`) |
| Outbox atual | Só módulos `MODULOS_DE_TURNO` com célula recebem `emitir_evento_celula`; `batida_ausente` está fora daquela família. (`app/chamados/models.py:938`; `app/chamados/reconciliador.py:336`) |
| Nascimento sem dedup | `criar` escreve chamado e trilha, retorna sem acionar fábrica/outbox no corpo. (`app/chamados/models.py:1048`) |
| Sinal indireto | `post_save` julga célula apenas para criação/mudança de estado e sai se já houver julgamento; erro é absorvido em `engolir`. Não comprova entrega universal da pergunta. (`app/chamados/signals.py:169`; `app/chamados/signals.py:172`; `app/chamados/signals.py:174`) |
| Fonte existente | `perguntar_pela_celula` lê motivos, desfaz carimbos sem lastro e chama `perguntar`, preservando silêncio de push e Pauta DP do limbo exportado. (`app/chamados/services/fabrica_da_celula.py:145`; `app/chamados/services/fabrica_da_celula.py:164`; `app/chamados/services/fabrica_da_celula.py:171`; `app/chamados/services/fabrica_da_celula.py:176`) |
| Chamador da fonte | Cartório chama a fábrica apenas para células julgadas naquela passada. Ata estável/célula pulada não implica nova emissão. (`app/ponto/services/cartorio.py:981`; `app/ponto/services/cartorio.py:995`) |
| Consumidor de outbox | Consome eventos para retratar chamados absolvidos e encerrar órfãs pela lâmpada; não chama a fábrica de perguntas. Portanto incluir `batida_ausente` em `emitir_evento_celula` sozinho não cura FIO MUDO. (`app/chamados/reconciliador.py:1326`; `app/chamados/reconciliador.py:1329`) |
| Detector | Abre chamado, depois decide agrupamento LIMBO-FOLGAS, depois tenta emitir fio em `try/except`. Hook síncrono dentro de `abrir` pode perguntar antes de receber o dono administrativo. (`app/ponto/management/commands/detectar_ausencias.py:693`; `app/ponto/management/commands/detectar_ausencias.py:745`; `app/ponto/management/commands/detectar_ausencias.py:761`; `app/ponto/management/commands/detectar_ausencias.py:787`; `app/ponto/management/commands/detectar_ausencias.py:802`) |
| Grade | Abre/reabre e retorna; não emite pergunta diretamente. (`app/ponto/management/commands/reconciliar_grade.py:507`; `app/ponto/management/commands/reconciliar_grade.py:529`) |
| C2 | Abre `batida_ausente`, depois costura a resolvedora existente. O evento precisa observar o ato completo para não criar concorrente antes da costura. (`app/chamados/reconciliador.py:879`; `app/chamados/reconciliador.py:922`) |
| Cron escritor | Ramo FIO MUDO seleciona qualquer chamado vivo com célula na janela, não só `batida_ausente`; examina resolvedoras e consulta ata, parcial, colaborador ativo e identidade histórica. Escreve pela mesma `perguntar`. (`app/chamados/management/commands/disparar_perguntas_competencia.py:141`; `app/chamados/management/commands/disparar_perguntas_competencia.py:148`; `app/chamados/management/commands/disparar_perguntas_competencia.py:154`; `app/chamados/management/commands/disparar_perguntas_competencia.py:158`; `app/chamados/management/commands/disparar_perguntas_competencia.py:169`; `app/chamados/management/commands/disparar_perguntas_competencia.py:211`; `app/chamados/management/commands/disparar_perguntas_competencia.py:280`) |
| Registro do cron | Encadeado segue com `--apply`; permanece em `JUIZES_POR_VARREDURA`. Ambos precisam migrar quando o evento cobrir o universo devido. (`app/config/crons.py:1495`; `app/config/crons.py:2007`) |

A fábrica já responde por marco vencido **por instante**, incluindo hoje e noturno de ontem; sem marcos, o passado segue pelo ramo das batidas. Sem ata com escala, abstém; `set()` e `None` têm significados diferentes. (`app/chamados/services/fabrica_da_celula.py:15`; `app/chamados/services/fabrica_da_celula.py:29`; `app/chamados/services/fabrica_da_celula.py:34`; `app/chamados/services/disputa_emissao.py:818`; `app/chamados/services/disputa_emissao.py:839`)

A deduplicação histórica não pode ser simplesmente removida: fábrica exclui todo motivo com identidade histórica; `PerguntaDisputa.abrir` devolve e eventualmente reaponta a mesma pergunta em qualquer estado, sem criar irmã. Validação anterior com marco ainda apagado tem dono administrativo nomeado, e não autoriza reperguntar automaticamente. (`app/chamados/services/fabrica_da_celula.py:118`; `app/chamados/services/fabrica_da_celula.py:126`; `app/chamados/models.py:2387`; `app/chamados/models.py:2420`; `app/chamados/models.py:2464`; `app/chamados/services/pergunta_no_app.py:29`; `app/chamados/management/commands/disparar_perguntas_competencia.py:201`)

Para retirar vivos legítimos do universo antes do RED, há causas existentes: parcial na lista do admin, marco anteriormente decidido, pergunta viva em outro chamado, cobertura por ausência, chamado de disputa e turno fechado na virada. A distribuição dos 494 nessas causas é [não verificado]. Não transplantar `retencao` inteiro como juiz, pois seu teto `dia>=hoje` diverge da emissão atual por instante. (`app/chamados/services/pergunta_no_app.py:21`; `app/chamados/services/pergunta_no_app.py:65`; `app/chamados/services/pergunta_no_app.py:73`; `app/chamados/services/pergunta_no_app.py:88`; `app/chamados/services/pergunta_no_app.py:92`)

Contraexemplos adicionais:

- Hook apenas em `post_save` de Pergunta cobre alterações persistidas, mas não morte apenas por leitura do C6; validação, tranca, resposta concordante, identidade e lâmpada têm degraus próprios. O C6 precisa continuar leitor. (`app/chamados/juizes.py:1135`; `app/chamados/juizes.py:1138`; `app/chamados/juizes.py:1150`; `app/chamados/juizes.py:1171`; `app/chamados/juizes.py:1181`)
- Transferência exige observar origem e destino; unidade do fio pode ser o dia, e não uma nova pergunta para cada chamado duplicado. `fio_vivo` declara explicitamente que o universo pertence ao chamador. (`app/chamados/juizes.py:1233`; `app/chamados/services/fio.py:97`; `app/chamados/models.py:2427`)
- A identidade histórica também é reapontada dentro de `PerguntaDisputa.abrir`; curar somente `fio.reapontar_resolvedora` deixa essa irmã descobert​a. (`app/chamados/models.py:2427`; `app/chamados/models.py:2436`; `app/chamados/models.py:2463`)
- Usar outbox como simples entrega da pergunta não funciona no consumidor atual; criar outro consumidor violaria a exigência do pedido. (`app/chamados/reconciliador.py:1326`; `app/chamados/reconciliador.py:1329`; [não verificado])
- Remover apenas o escritor do cron antes desses eventos deixa o detector dependente de emissão posterior que absorve exceções e a grade sem entrega direta. (`app/ponto/management/commands/detectar_ausencias.py:787`; `app/ponto/management/commands/detectar_ausencias.py:802`; `app/ponto/management/commands/reconciliar_grade.py:531`)

Leitores afetados pela mudança da fonte: cartório chama `perguntar_pela_celula`; vigia do lote lê `motivos_que_o_evento_deixou`; ramo FIO MUDO chama `perguntar`; selos existentes leem esses caminhos e a configuração encadeada. (`app/ponto/services/cartorio.py:985`; `app/chamados/management/commands/disparar_perguntas_competencia.py:112`; `app/chamados/management/commands/disparar_perguntas_competencia.py:280`; `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:152`; `app/chamados/tests/test_selo_emissor_fio_mudo.py:207`)

## 05/11 18:39 · sessao 7f24b9d6f319 · - · - · origem=Ronald (tmux)

Censo dos cinco atos e limites do diff, somente pela cópia:

| Ato | Fonte e escrita atual | Consequência para o diff |
|---|---|---|
| `reabrir` | Recarrega estado; recusa por premissa, decisão administrativa sem fato novo, agrupamento e irmão vivo; só depois `_gravar_estado` e `ressuscitar_perguntas`. (`app/chamados/models.py:1322`, `:1340`, `:1382`, `:1391`) | Evento deve sair da porta após reabertura efetiva; ressuscitação sozinha não cria pergunta inexistente. Seu universo exige `materializada_em` e ausência de texto, e testa C6 no estado restaurado. (`app/chamados/models.py:1423`, `:1444`, `:1447`) |
| `retratar_duplicata` | Só chama `carimbar_identidade`; não reconcilia o chamado anterior. (`app/chamados/services/fio.py:878`, `:891`, `:895`) | Retração de duplicata respondida pode conservar C6 vivo: conversa precede identidade; não deve reemitir nem apagar resposta. (`app/chamados/juizes.py:1150`, `:1171`; `app/chamados/models.py:2614`) |
| Validação | `_carimbar_validacao` é escritor da assinatura. Validação HORA e materialização/refutação rodam no mesmo `atomic`; recusa devolve assinatura e preserva texto. (`app/chamados/services/validacao.py:127`, `:155`, `:188`, `:395`, `:425`) | Evento tem de ler desfecho final, não assinatura intermediária. O ramo folga retorna antes do ramo comum e tem seus próprios desfechos. (`app/chamados/services/validacao.py:382`, `:262`, `:289`, `:337`) |
| Morte por leitura | C6 (`pergunta_viva`) é juiz registrado; wrapper memoizado chama `_pergunta_viva_calc`. Tranca, marco/célula e janela entram no cálculo. (`app/core/juizes.py:266`; `app/chamados/juizes.py:822`, `:1135`, `:1140`, `:1178`, `:1195`, `:1197`) | Não criar juiz nem transformar consulta de vitalidade em escritor. Fonte tranca já dispara encerramento por `encerrar_por_competencia_trancada`; célula/marco passam pelo cartório. (`app/chamados/services/competencia_trancada.py:33`, `:125`, `:178`, `:184`; `app/ponto/services/cartorio.py:985`, `:1595`) |
| `reapontar` | Porta atomic testa identidade de dia, trava destino contra fechamento, altera destino e lavra no mesmo save; corpo não trata o chamado que perdeu a pergunta. (`app/chamados/services/fio.py:784`, `:829`, `:837`, `:845`, `:847`) | Capturar origem antes da troca e tratar perda depois do save; preservar guarda concorrente do destino. Há segunda via de reapontamento no nascimento por identidade existente. (`app/chamados/services/fio.py:752`; `app/chamados/models.py:2420`, `:2427`, `:2439`, `:2463`) |

**Autoridades suficientes, sem lacuna de lei encontrada:** C6 decide a pergunta; C15 reduz coleção; `premissa_regularizada` fornece mesma decisão aos gates de fechar/reabrir; C17 decide alcance. Não encontrei fundamento para PAREI por ausência de juiz. (`app/core/juizes.py:266`, `:275`, `:277`; `app/chamados/models.py:1202`, `:1209`)

**Morte por relógio:** C6 devolve viva imediatamente para célula acusadora com veredito antes de consultar a janela. Portanto, “virou janela” sozinho não mata pergunta dessa classe. O relógio participa do fallback posterior; não equivale a “chamado envelheceu”. (`app/chamados/juizes.py:1181`, `:1183`, `:1197`; `app/chamados/juizes.py:556`)

**Vivo legítimo sem pergunta:** o ramo FIO MUDO já nomeia fato decidido historicamente como ação faltante do admin, parcial na lista, inativo e pergunta viva em outro chamado; a triagem deve excluir legitimidade antes do RED, não produzir nova irmã por assinatura humana existente. (`app/chamados/management/commands/disparar_perguntas_competencia.py:169`, `:173`, `:201`, `:211`, `:224`; `app/chamados/services/pergunta_no_app.py:92`; `app/docs/PROMPTS.md:1882`)

**Por que um diff com só hooks para `perguntar_pela_celula` falha:** `motivos_a_perguntar` exclui todo histórico não duplicata, mesmo morto; `PerguntaDisputa.abrir` reutiliza a morta sem limpar decisão humana. O hook não cobre por si só os cinco atos. (`app/chamados/services/fabrica_da_celula.py:118`, `:123`, `:126`; `app/chamados/models.py:2420`, `:2464`)

**Porta de normalização existente a extrair/reusar:** cartório já verifica última pergunta morta, preserva texto humano, reconcilia duplicatas e ressuscita pelo gate único. Mas pula chamados com zero perguntas; só esse bloco também não cobre nascimento mudo. A extração deve preservar a mesma autoridade, não copiar suas decisões para um segundo escritor. (`app/ponto/services/cartorio.py:1591`, `:1595`, `:1597`, `:1599`, `:1622`; `app/chamados/models.py:1450`)

**Leitores afetados se o diff mudar vitalidade ou carimbos:** estado/viva do modelo (`app/chamados/models.py:2491`, `:2762`), app (`app/chamados/services/pergunta_no_app.py:53`), retencão (`app/chamados/services/pergunta_no_app.py:111`), disputa pode fechar (`app/chamados/models.py:1783`), cartório (`app/ponto/services/cartorio.py:1339`, `:1595`), relatório (`app/chamados/relatorios_disputa.py:18`), aviso (`app/chamados/services/aviso_pendencia.py:13`), calendário/detalhe (`app/ponto/services/dia_do_colab.py:70`), prévia (`app/folha/previa.py:144`), supra e emissão retroativa (`app/ponto/management/commands/supra_juiz.py:215`; `app/ponto/management/commands/emitir_furo_retroativo.py:98`), relavra (`app/chamados/services/relavra_via.py:227`), vigia (`app/chamados/services/vigia_fio.py:166`, `:755`, `:813`), tranca (`app/chamados/services/competencia_trancada.py:122`, `:132`).

**Chamadores de costura/duplicata que o diff precisa alcançar:** dedup (`app/chamados/management/commands/dedup_perguntas_fato.py:88`), migração (`app/chamados/services/fio.py:943`, `:947`), reconciliador (`app/chamados/reconciliador.py:922`, `:1570`, `:1582`), limbo (`app/chamados/services/limbo_folgas.py:243`, `:276`), declaração (`app/chamados/services/declaracao_texto.py:320`) e nascimento com identidade existente (`app/chamados/models.py:2427`).

Não produzi diff completo verificável: a extração da normalização e os pontos de evento precisam ser definidos no consolidado; um hook curto isolado seria demonstravelmente incompleto pelos contraexemplos acima. (`app/ponto/services/cartorio.py:1595`; `app/chamados/services/fabrica_da_celula.py:126`)

## 05/11 18:39 · sessao c49dc6236336 · - · - · origem=Ronald (tmux)

Referências confirmadas nesta cópia, sem uso de banco ou logs:

**Leis e diferença do censo anterior**

- O aval vigente está localmente em `app/docs/PROMPTS.md:1875`: abertura de `batida_ausente` + cinco atos; a enumeração literal e a retirada dos vivos legítimos antes do RED estão em `app/docs/PROMPTS.md:1882`; a autorização de diff textual do pré-código está em `app/docs/PROMPTS.md:1886`.
- C11 operacional é o nome da saída da exceção do disparador em `app/docs/CORTES.md:305`. `_C11` do registro vivo responde outra pergunta — silêncio por ausência — em `app/core/juizes.py:117`, autoridade em `app/core/juizes.py:271`. Não declarar equivalência técnica.
- Nenhum juiz novo é necessário para saber se uma pergunta morreu: C6 está registrado em `app/core/juizes.py:266`, com cálculo em `app/chamados/juizes.py:1135`; C15 responde `any` da coleção em `app/chamados/juizes.py:1233`, registro `app/core/juizes.py:275`; C17 responde alcance, registro `app/core/juizes.py:277`.
- Regra do evento e lâmpada: `CLAUDE.md:166`–`170`; um juiz/escritor e idempotência: `CLAUDE.md:175`; censo completo antes de filtro: `CLAUDE.md:193`; casos pela regra antes do código: L-110, `app/docs/LEIS.md:136`; sítio redundante sai no mesmo ato: L-111, `app/docs/LEIS.md:137`.
- Corte estrutural manda separar chamado vivo de cobrança viva e veda fallback/juiz novo: `app/docs/CORTES.md:253`. Chamado nasce da célula, morre por lastro, nunca sem dono: `app/docs/ESTRUTURAL.md:46`.

**Casos A fundamentáveis pela regra**

| Cadastro + estado + ato | Resposta exigida e fonte |
|---|---|
| Ativo, vínculo do dia válido, ata com marco vencido faltante, competência aberta; abrir `batida_ausente`. | Pergunta correspondente nasce no evento, lendo a dívida da célula; `app/docs/PROMPTS.md:1875`, `CLAUDE.md:168`. |
| Mesmo fato; duas aberturas concorrentes. | Mesmo estado, uma trilha e zero duplicata; `CLAUDE.md:175`. |
| Chamado encerrado, dívida persiste, nenhuma pergunta ressuscitável; `reabrir`. | Vivo sozinho não quita o contrato: persiste pergunta viva ou ação administrativa nomeada; `app/docs/CORTES.md:253`, `app/docs/PROMPTS.md:1882`. |
| A perde sua última resolvedora por `retratar_duplicata` ou por `reapontar` para B. | Preservar um fio alcançável para o fato, sem exigir uma nova pergunta por chamado duplicado; `CLAUDE.md:175`, `app/docs/CORTES.md:253`, `app/docs/CORTES.md:404`. |
| Pergunta respondida discordante, sem decisão humana; retratar como duplicata. | Preservar resposta para decisão humana; lei HX-ADMIN-DECIDE-O-DIA expressa em `app/chamados/juizes.py:1151`, exceção de concordância em `app/chamados/juizes.py:1159`. |
| Saída e intervalo faltantes; validar/materializar só saída. | Marco resolvido não é perguntado de novo; intervalo devido conserva fio/ação; regra por marco em `app/chamados/juizes.py:1173`, lâmpada/fio em `app/docs/CORTES.md:253`. |
| Validação encontra recusa do plantio, fato continua devido. | Recusa não produz quitação fictícia nem retira a decisão humana; corte `app/docs/CORTES.md:253`; porta documenta devolução em `app/chamados/services/validacao.py:413`. |
| Célula/batida/tranca muda sem `save` de PerguntaDisputa; pergunta antes viva passa a morta. | C6 responde vida; não inventar juiz nem escrita em função leitora. Evento precisa cobrir o escritor da dependência que mudou; `app/core/juizes.py:266`, `CLAUDE.md:168`, `app/docs/PROMPTS.md:1882`. |
| Corte cadastrado 21; fatos 21/09, 20/10, 21/10. | Primeiros dois são competência 10; terceiro é 11. Tranca da 10 não cala 21/10; L-090 `app/docs/LEIS.md:123`, autoridade `app/ponto/janelas.py:10`, `app/ponto/janelas.py:24`. |
| Turno 22h–06h do dia anterior; observar 00h01 e 06h10. | Meia-noite não encerra fato; saída ainda não venceu com margem de 30 minutos. `CLAUDE.md:185`, `app/ponto/services/cartorio.py:828`, encadeamento `app/ponto/services/cartorio.py:845`, lei explicitada `app/chamados/services/fabrica_da_celula.py:18`. |
| Marco às 18h, margem 30 min; 18h29m59 e 18h30. | Antes não cobra; no instante de vencimento pode cobrar o marco faltante; `CLAUDE.md:185`, `app/ponto/services/cartorio.py:846`. |
| Novo vínculo ativo hoje; célula de ontem nomeia antigo ainda dentro da vigência. | Antigo responde pelo dia; fora da vigência retorna CADASTRO/None, sem substituição silenciosa; `app/escala/alimentacao.py:231`, `app/escala/alimentacao.py:272`. |
| Escala vigente, célula sem ata; tentar emissão de marco. | Abstém e acende falta de ata; não fabricar marco. `app/chamados/services/disputa_emissao.py:819`, `app/chamados/services/disputa_emissao.py:833`. |
| `trabalha=None`, sem ata que diga outro tipo de dia. | Não converter desconhecido em folga; limbo cobra conforme autoridade, mas pergunta de marco ainda exige ata; `app/docs/CORTES.md:416`, `app/core/juizes.py:137`, `app/chamados/services/disputa_emissao.py:819`. |
| Parcial aprovada sem período; dívida restante. | Lista administrativa CADASTRO; não julga dentro/fora, não fabrica falta/pergunta; `app/docs/CORTES.md:319`, `app/chamados/services/disputa_emissao.py:847`. |
| `registrado` vivo sem ação; ausência aberta suspensiva; parcial na lista administrativa. | Retirar os legítimos do universo dos 494 antes de RED, sem “curar” criando pergunta artificial; `app/docs/PROMPTS.md:1882`, `app/chamados/catalogo/motor.py:24`, `app/chamados/services/silencio_ausencia.py:77`, `app/chamados/services/disputa_emissao.py:856`. |
| Competência trancada, depois destrancada ou tocada pelo admin. | Tranca cala por leitura, sem quitação do fato; destranca/toque humano devolve vida conforme L-051; `app/chamados/juizes.py:919`, `app/chamados/juizes.py:951`. |
| Competência exportada, célula `trabalha=True`; outro controle limbo `trabalha=None`. | Não aplicar guarda universal “exportada não pergunta”: trabalho continua perguntando pelo O44; limbo exportado segue Pauta DP. `app/chamados/services/fabrica_da_celula.py:167`. |
| Passado sem fato novo; introduzir a obra. | Não backfill de todo estoque automaticamente: horizonte para frente e passado FOTO até fato novo; L-124 `app/docs/LEIS.md:145`. |

**Contraexemplos relevantes para C/D**

- `post_save` de pergunta sozinho não cobre morte por leitura: tranca é leitura de `periodo_trancado`, sem carimbo/estado novo, e destranca devolve vida sozinha; `app/chamados/juizes.py:919`. Célula muda vida em `app/chamados/juizes.py:1181`; resposta concordante com luz pode morrer sem novo carimbo em `app/chamados/juizes.py:1159`.
- Adicionar escrita dentro de C6 torna leitor escritor e permite side effects em app/folha/vigia; C6 é explicitamente função de leitura na política de exceção `app/chamados/juizes.py:1210` e usado por `perguntas_do_app` em `app/chamados/services/pergunta_no_app.py:42`.
- Um diff que sempre crie pergunta após perda pode recriar saída já resolvida quando o dia ainda acusa intervalo: marco vem antes do dia, `app/chamados/juizes.py:1173`.
- Um diff que use qualquer pergunta morta como prova de quitação erra: célula acusadora pode manter vida apesar de carimbo, `app/chamados/juizes.py:1181`; validação/tranca/identidade têm precedências próprias em `app/chamados/juizes.py:1135`.
- Guarda universal de exportado erra o caso O44: `app/chamados/services/fabrica_da_celula.py:167`; portanto o antigo pd não pode ser copiado nesse ponto.
- O denominador 494 não veio com registros; distribuição por ato, legítimos e incidências concretas são **[não verificado]**. O aval exige retirá-los antes do RED em `app/docs/PROMPTS.md:1882`; não é possível inventar quantos sobram.
- Mudanças de empresa/corte durante vigia e reabertura durante request afetam frescor da tranca; memo global da rodada não pode congelar empresa/período. Motivo e correção documentados em `app/chamados/juizes.py:982`, guarda só SIM do toque em `app/chamados/juizes.py:1033`.

**Leitores adicionais que precisam aparecer no censo**

- App: `app/chamados/services/pergunta_no_app.py:42`.
- Retenção/classificação de causa: `app/chamados/services/pergunta_no_app.py:65`; medição usa COBRANCA e ativos em `app/chamados/services/pergunta_no_app.py:119`.
- Fábrica: `app/chamados/services/fabrica_da_celula.py:74`, exclusão por identidade histórica em `app/chamados/services/fabrica_da_celula.py:118`.
- Emissão por evento/célula: `app/chamados/services/fabrica_da_celula.py:145`.
- Alcance depende de pergunta no app e âncora na fila, não só C15: `app/core/juizes.py:132`; corte manda folha/raio-X/vigia lerem C17 em `app/docs/CORTES.md:404`.
- Tranca depende dos três toques humanos: Cobrar, Resolver dia, desarquivar; resposta do colaborador e cron não contam. `app/chamados/juizes.py:957`.

## 05/11 18:36 · sessao 7cce7722cbad · PCCMP-FIO-MUDO · censo · origem=Code

HASH LIDO = 9c0fd9d3e90f166cba697ec840422b5585a8ab09

Uso **pc1** e **pc2** para identificar os anexos transcritos, com suas seções e casos. Numeração física das linhas dos anexos: **[não verificado]**. Os **494** são o agregado declarado no aval; sua composição individual e a partição entre defeitos e vivos legítimos permanecem **[não verificado]**. (`app/docs/PROMPTS.md:1875`; `app/docs/PROMPTS.md:1882`)

**1 COINCIDE**

**Casos equivalentes**

| pc1 | pc2 | Coincidência conferida na cópia |
|---|---|---|
| A1/A2 | A01/A02 | Nascimento por evento e idempotência como requisito: mesmo estado, uma trilha, nenhuma duplicata. (`CLAUDE.md:168`; `CLAUDE.md:175`) |
| A3 | A03 | Reabrir o chamado não demonstra recuperação do fio: grava estado e ressuscita perguntas existentes; a ressuscitação pode retornar zero. (`app/chamados/models.py:1382`; `app/chamados/models.py:1391`; `app/chamados/models.py:1482`) |
| A4 | A05 | Duplicata não exige uma pergunta nova por chamado; importa conservar a cobrança alcançável do fato. (`app/chamados/models.py:2388`; `app/chamados/juizes.py:1344`) |
| A5 | A06 | Carimbo de identidade não encerra necessariamente resposta discordante pendente: a resposta é examinada antes da via de identidade. (`app/chamados/models.py:2617`; `app/chamados/juizes.py:1150`; `app/chamados/juizes.py:1171`) |
| A7/A8 | A07/A08 | Resolver um marco não resolve outro; a devolução de validação recusada preserva a resposta. (`app/chamados/juizes.py:1173`; `app/chamados/services/validacao.py:171`) |
| A9/A10 | A10/A11 | Reapontamento precisa preservar alcance; a costura dispõe de guarda contra destino terminal e trava sua linha durante a operação transacional. (`app/chamados/juizes.py:1344`; `app/chamados/services/fio.py:752`; `app/chamados/services/fio.py:837`) |
| A11 | A12 | C6 já julga morte por leitura: consulta tranca, estado da célula, ata e janela. (`app/core/juizes.py:266`; `app/chamados/juizes.py:1140`; `app/chamados/juizes.py:1195`; `app/chamados/juizes.py:1197`) |
| A13/A14 | A15/A14 | Vencimento por instante, incluindo igualdade e turno que cruza meia-noite. O comparador é `<=`; a saída das 06h ainda não venceu às 06h10 com margem de 30 minutos. (`app/ponto/services/cartorio.py:828`; `app/ponto/services/cartorio.py:846`; `app/chamados/services/fabrica_da_celula.py:18`) |
| A15 | A13 | Janela de competência depende do corte cadastrado da empresa. (`app/ponto/janelas.py:10`; `app/ponto/janelas.py:54`) |
| A18 | A16 | Vínculo do dia considera a geradora e sua vigência; não se substitui automaticamente pelo vínculo ativo atual. (`app/escala/alimentacao.py:231`; `app/escala/alimentacao.py:269`; `app/escala/alimentacao.py:272`) |
| A19/A20 | A17/A19 | Sem célula/ata possível para escala vigente, o questionário espera; parcial administrativa pode permanecer sem pergunta. (`app/chamados/services/disputa_emissao.py:833`; `app/chamados/services/disputa_emissao.py:842`; `app/chamados/services/disputa_emissao.py:856`; `app/chamados/services/fabrica_da_celula.py:102`) |
| A21 | A09/A20/A21 | Registro, decisão disponível, ausência aberta e dono efetivamente na lista precisam ser separados do defeito. Estado `em_analise` sozinho não constitui ação nomeada. (`app/chamados/services/painel.py:174`; `app/chamados/services/vigia_fio.py:283`; `app/chamados/services/vigia_fio.py:287`; `app/chamados/services/vigia_fio.py:292`) |
| A22/A23, parte histórica | A23 | Hoje fica fora do contador alarmado; passado sem fato novo não autoriza backfill geral. Isso não impede emissão de marco já vencido por instante. (`app/chamados/services/vigia_fio.py:420`; `app/chamados/services/fabrica_da_celula.py:8`; `app/docs/LEIS.md:145`) |

**Sítios e autoridades**

| Item presente nos dois | Conferência |
|---|---|
| C6, C15 e C17; distinção do C11 | O registro aponta C6 para vitalidade, C15 para coleção, C17 para alcance e C11 para ausência. (`app/core/juizes.py:266`; `app/core/juizes.py:271`; `app/core/juizes.py:275`; `app/core/juizes.py:277`) |
| `ChamadoColaborador.abrir` | Trava colaborador, pode devolver existente/renascido e só chega ao evento no nascimento real. `MODULOS_DE_TURNO` não contém `batida_ausente`. (`app/chamados/models.py:866`; `app/chamados/models.py:885`; `app/chamados/models.py:933`; `app/chamados/models.py:939`; `app/chamados/reconciliador.py:336`) |
| `ChamadoColaborador.criar` | O corpo termina em criação, trilha e retorno, sem emissão explícita equivalente. (`app/chamados/models.py:1048`; `app/chamados/models.py:1049`; `app/chamados/models.py:1050`) |
| Detector, grade e reemissão C2 | Detector tenta recuperar fio e absorve falha; grade abre e retorna; C2 abre e reaponta. (`app/ponto/management/commands/detectar_ausencias.py:790`; `app/ponto/management/commands/detectar_ausencias.py:802`; `app/ponto/management/commands/reconciliar_grade.py:507`; `app/ponto/management/commands/reconciliar_grade.py:531`; `app/chamados/reconciliador.py:879`; `app/chamados/reconciliador.py:922`) |
| Sinal indireto | Julgamento está sob `engolir` e pode retornar durante julgamento já em curso. (`app/chamados/signals.py:172`; `app/chamados/signals.py:174`) |
| Duplicata e transferência | Retratação chama `carimbar_identidade`; reapontamento escreve destino, lavra e salva. Esses corpos não reconciliam explicitamente a origem que perdeu a resolvedora. (`app/chamados/services/fio.py:891`; `app/chamados/services/fio.py:895`; `app/chamados/services/fio.py:845`; `app/chamados/services/fio.py:849`) |
| Validação | Assinatura, materialização individual e devolução ficam no `atomic`; materialização da disputa inteira vem depois. (`app/chamados/services/validacao.py:395`; `app/chamados/services/validacao.py:424`; `app/chamados/services/validacao.py:425`; `app/chamados/services/validacao.py:430`) |
| Identidade histórica na fábrica | A seleção exclui duplicatas da consulta histórica, reúne os motivos já existentes e os retira dos motivos novos, sem consultar C6 nessa seleção. (`app/chamados/services/fabrica_da_celula.py:118`; `app/chamados/services/fabrica_da_celula.py:119`; `app/chamados/services/fabrica_da_celula.py:123`; `app/chamados/services/fabrica_da_celula.py:126`) |
| Reapontamento dentro de `PerguntaDisputa.abrir` | Está nos dois: pc1 na matriz de campos; pc2 como sítio irmão. A identidade existente pode receber célula e destino. (`app/chamados/models.py:2424`; `app/chamados/models.py:2427`; `app/chamados/models.py:2439`) |
| Cartório e consumidor | Cartório chama fábrica para datas julgadas naquela passada. Consumidor atual retrata chamados e encerra órfãs, sem chamada à fábrica nesse corpo. (`app/ponto/services/cartorio.py:996`; `app/ponto/services/cartorio.py:997`; `app/chamados/reconciliador.py:1326`; `app/chamados/reconciliador.py:1329`) |
| Varredura ainda escritora | Comando chama `perguntar` sob `--apply`; agenda usa `--apply`; registro mantém o ramo FIO MUDO como julgamento por varredura. (`app/chamados/management/commands/disparar_perguntas_competencia.py:280`; `app/chamados/management/commands/disparar_perguntas_competencia.py:287`; `app/config/crons.py:1495`; `app/config/crons.py:2007`) |

**Trecho de diff comum**

Ambos propõem a mesma substituição da condição de nascimento; a condição antiga está exatamente em `app/chamados/models.py:939`:

```diff
- if (_mo or '') in _MT and ch.celula_id is not None:
+ if ((_mo or '') in _MT or _mo == 'batida_ausente') and ch.celula_id is not None:
```

O trecho conserva a emissão pela porta existente e fica dentro do `atomic` de `abrir`. (`app/chamados/models.py:863`; `app/chamados/models.py:940`; `app/ponto/portas/celula.py:664`)

**Contraexemplos comuns**

- Existente reutilizado, reabertura sem candidata e nascimento por `criar` escapam do hunk condicionado a `criado`. (`app/chamados/models.py:885`; `app/chamados/models.py:933`; `app/chamados/models.py:1048`; `app/chamados/models.py:1482`)
- O hunk não instrumenta duplicata, validação nem origem de transferência. (`app/chamados/services/fio.py:891`; `app/chamados/services/validacao.py:425`; `app/chamados/services/fio.py:845`)
- Chamar a fábrica não demonstra recuperação da identidade histórica. (`app/chamados/services/fabrica_da_celula.py:123`; `app/chamados/services/fabrica_da_celula.py:126`; `app/chamados/models.py:2420`)
- Morte por leitura não depende exclusivamente de escrita da pergunta; lavra de materialização e C6 respondem perguntas distintas. (`app/chamados/juizes.py:1140`; `app/chamados/juizes.py:1197`; `app/chamados/veredito_lavrado.py:219`)
- Acrescentar efeito ao getter mistura leitura memoizada com entrega; o memo não comprova processamento transacional da obrigação. (`app/chamados/juizes.py:822`; `app/chamados/juizes.py:835`; `app/chamados/juizes.py:847`)
- Nenhum candidato demonstra a cobertura exigida para aposentar a varredura: o mandato inclui abertura e cinco atos; o comando continua escritor. (`app/docs/PROMPTS.md:1882`; `app/chamados/management/commands/disparar_perguntas_competencia.py:280`)

Os dois também nomeiam leitores de app/web/API, relatórios, calendário, fechamento, folha e vigias. Esses consumos aparecem efetivamente na cópia. (`app/chamados/services/pergunta_no_app.py:53`; `app/chamados/views.py:1238`; `app/api/views.py:2627`; `app/chamados/relatorios_disputa.py:18`; `app/colaboradores/services/calendario.py:468`; `app/chamados/models.py:1783`; `app/folha/fora_da_folha.py:67`; `app/chamados/services/vigia_fio.py:635`)

**2 DIVERGE**

**D1 — Consumidor passa a acionar perguntas: DESENHO.**

- **pc1 C:** acrescenta `perguntar_pela_celula` para cada célula encontrada pelos `celula_id` do lote, depois do encerramento de órfãs.
- **pc2 C:** propõe apenas o evento de nascimento e declara que o consumidor atual não entrega a pergunta.
- **Código aberto:** em `app/chamados/reconciliador.py:1326` há `retratar_chamados_absolvidos_por_celula`; em `app/chamados/reconciliador.py:1329`, `encerrar_orfas_pela_lampada`; em `app/chamados/reconciliador.py:1332`, o cálculo de adiamento; depois vem o tratamento de exceção, em `app/chamados/reconciliador.py:1333`.

A diferença muda **quem aciona a escrita de perguntas e seu universo**: o consumidor lê eventos pendentes e os agrupa por colaborador, sem recorte por nascimento de `batida_ausente`. Ambos reprovam seus candidatos; não divergem afirmando que um deles satisfaz a obra. (`app/chamados/reconciliador.py:1309`; `app/chamados/reconciliador.py:1315`; `app/chamados/services/fabrica_da_celula.py:175`)

**D2 — Competência exportada: DESENHO na formulação da regra.**

- **pc1 A23:** manda conservar o “gravado exportado” e encaminhar a diferença, citando L-092. Essa linha realmente contém “depois do export, diferença não se aplica”. (`app/docs/LEIS.md:125`)
- **pc2 A24:** explicita que L-113 revogou a recusa geral de recalcular e a Pauta DP como pré-condição, preservando TXT anterior e trilha. (`app/docs/LEIS.md:149`)
- **Código aberto:** declara “COMPETENCIA EXPORTADA NAO BARRA MAIS” e conserva versão anterior do TXT e trilha. (`app/ponto/services/fechamento.py:476`; `app/ponto/services/fechamento.py:484`)

Se “gravado” em pc1 significa congelar o cálculo, a formulação contraria a alteração registrada e o comportamento documentado na função. Se significa somente preservar o TXT anterior, coincide com pc2; essa delimitação não está explícita em pc1. O tratamento particular do limbo exportado continua na fábrica e não equivale à antiga proibição geral. (`app/docs/LEIS.md:149`; `app/ponto/services/fechamento.py:479`; `app/chamados/services/fabrica_da_celula.py:167`)

**D3 — Premissa do nascimento: detalhe de especificação.**

Pc1 A1 diz “sem pergunta”; pc2 A01 exige “sem identidade histórica”. A segunda formulação é mais restrita: ausência de pergunta viva não significa ausência de identidade. Entretanto, pc1 D também reconhece essa barreira histórica; não há incompatibilidade global demonstrada entre os anexos nesse ponto. (`app/chamados/services/fabrica_da_celula.py:118`; `app/chamados/services/fabrica_da_celula.py:126`; `app/chamados/models.py:2388`)

**D4 — Separação dos vivos legítimos: detalhe de exposição.**

Pc1 reúne condições em A21; pc2 distribui entre A09/A20/A21. “Viva sem porta”, destacado em pc2 A22, também está conceitualmente em pc1 B/D. A cópia distingue vitalidade, alcance e dono fora do fio; não foi demonstrada uma escolha incompatível de universo entre essas apresentações. (`app/chamados/juizes.py:1344`; `app/chamados/juizes.py:1367`; `app/chamados/services/vigia_fio.py:274`)

As divergências por **omissão** estão discriminadas abaixo, com sua classificação e o sítio conferido.

**3 SÓ NUMA**

“Só numa” abaixo significa ausência daquele item explícito na outra resposta; não presume que a outra defenda a regra contrária.

| Origem | Item omitido na outra | Código aberto e classificação |
|---|---|---|
| **pc1 A6** | Caso separado de validação integral, sem dívida residual | O fechamento pergunta a premissa; marco atendido pode encerrar sua cobrança. **Detalhe de cobertura de casos.** (`app/chamados/models.py:681`; `app/chamados/juizes.py:1173`) |
| **pc1 A12** | Caso separado de resposta concordante com a luz | C6 mantém pendência quando a comparação não demonstra concordância. **Detalhe de cobertura de casos.** (`app/chamados/juizes.py:1096`; `app/chamados/juizes.py:1162`) |
| **pc1 A17** | Toque humano posterior à tranca como cenário próprio | Há consulta à trilha para atos humanos nomeados, excluindo usuário de sistema. **Detalhe de cobertura de casos.** (`app/chamados/juizes.py:1054`; `app/chamados/juizes.py:1071`) |
| **pc2 A04** | Caso separado de reabertura recusada | Guardas devolvem `admin_sem_fato_novo` e `premissa_regularizada`. **Detalhe de cobertura de casos.** (`app/chamados/models.py:1292`; `app/chamados/models.py:1297`; `app/chamados/models.py:1300`) |
| **pc2 A18** | Passado sem escala/marcos como cenário próprio | A fábrica preserva esse ramo e pode consultar classificação pelas batidas. **Detalhe de cobertura de casos.** (`app/chamados/services/fabrica_da_celula.py:21`; `app/chamados/services/fabrica_da_celula.py:52`; `app/chamados/services/fabrica_da_celula.py:55`) |
| **pc1 B** | Inventário explícito das quatro portas de contexto e duas de silêncio | `anotar`, `renovar`, `retirar`, `fundir`, `silenciar` e `levantar`. **DESENHO: cobertura dos escritores.** (`app/chamados/services/contexto.py:62`; `app/chamados/services/contexto.py:81`; `app/chamados/services/contexto.py:106`; `app/chamados/services/contexto.py:128`; `app/chamados/services/arquivo.py:141`; `app/chamados/services/arquivo.py:176`) |
| **pc1 B** | Mapa nominal C1/C2/C3/C16/C18 e precedência do registro vivo sobre lookup | As autoridades estão registradas; `JUIZES.md` declara seu papel de lookup. **Detalhe do mapa de autoridades existentes.** (`app/core/juizes.py:261`; `app/core/juizes.py:263`; `app/core/juizes.py:276`; `app/core/juizes.py:278`; `app/docs/JUIZES.md:7`; `app/docs/JUIZES.md:14`) |
| **pc2 B** | C12/C13 nominalmente identificados | Registro aponta exclusão da fábrica e retenção. **Detalhe do mapa de autoridades existentes.** (`app/core/juizes.py:272`; `app/core/juizes.py:273`) |
| **pc1 B/D** | Fonte específica da tranca e comando/agenda stale | Há serviço de tranca e execução própria de lavra vencida. **DESENHO: fontes de acionamento e distinção de julgamento.** (`app/chamados/services/competencia_trancada.py:178`; `app/chamados/services/competencia_trancada.py:276`; `app/chamados/management/commands/lavrar_perguntas_stale.py:87`; `app/chamados/veredito_lavrado.py:219`; `app/config/crons.py:390`) |
| **pc1 A/B** | Advertência expressa de que C17 exclui “Resolver dia”, aceito separadamente pelo vigia | A exclusão está escrita no juiz; o vigia consulta a decisão oferecida. **DESENHO: universos distintos de alcance e dono.** (`app/chamados/juizes.py:1367`; `app/chamados/services/vigia_fio.py:283`) |
| **pc2 B** | Critério de escolha de `canonica_do_grupo` | Prioriza resposta; depois, menor identificador. **Detalhe do dedup existente.** (`app/chamados/services/fio.py:870`; `app/chamados/services/fio.py:873`; `app/chamados/services/fio.py:875`) |
| **pc2 B** | `relavrar` individualizado como escritor de via | A função escreve via e salva junto com a lavra. **DESENHO: cobertura dos escritores.** (`app/chamados/services/relavra_via.py:104`; `app/chamados/services/relavra_via.py:123`; `app/chamados/services/relavra_via.py:125`) |
| **pc1 D** | Agrupamento decidido depois de abrir | Detector decide agrupamento sob limbo após receber o chamado. **DESENHO: ordem entre abertura, agrupamento e emissão.** (`app/ponto/management/commands/detectar_ausencias.py:745`; `app/ponto/management/commands/detectar_ausencias.py:758`; `app/ponto/management/commands/detectar_ausencias.py:761`) |
| **pc1 C/D** | Laço consumidor→fábrica e alerta sobre eventos de outras origens | O lote não distingue esse nascimento dos demais eventos. **DESENHO: acionador e universo**, conforme D1. (`app/chamados/reconciliador.py:1284`; `app/chamados/reconciliador.py:1309`) |
| **pc1 D** | Célula ausente como bloqueio explícito do candidato | A condição exige `ch.celula_id is not None`. **DESENHO: universo coberto pelo evento.** (`app/chamados/models.py:939`) |
| **pc1 D** | Exceção permissiva de leitura da ata explicitamente localizada | A exceção retorna `True, None`; a fábrica pode entrar na classificação pelas batidas quando `vencidos=None`. **DESENHO: autoridade usada após falha.** (`app/chamados/services/disputa_emissao.py:828`; `app/chamados/services/disputa_emissao.py:832`; `app/chamados/services/fabrica_da_celula.py:52`) |
| **pc1 D** | Ausência de ata e relógio na chave do memo explicitamente demonstrada | A chave não os contém, mas o cálculo lê ata e janela; chave igual permite devolver resultado guardado. **DESENHO: frescor da leitura.** Ocorrência nos 494: **[não verificado]**. (`app/chamados/juizes.py:811`; `app/chamados/juizes.py:837`; `app/chamados/juizes.py:1195`; `app/chamados/juizes.py:1197`) |
| **pc2 D10** | Risco de inversão ao emitir sincronamente dentro de `abrir` | Ordem declarada: Disputa→Colaborador; `abrir` já trava colaborador; fábrica chama agregação de disputa. **DESENHO: ordem de trava.** (`app/chamados/models.py:843`; `app/chamados/models.py:866`; `app/chamados/services/fabrica_da_celula.py:139`; `app/chamados/services/disputa_emissao.py:598`) |
| **pc2 D12** | “Forçar julgamento” pode não chegar à fábrica | `julgar_celula` retorna sem célula ou com turno atual ainda em curso; fábrica depende das datas julgadas. **DESENHO: cobertura do acionamento.** (`app/ponto/services/cartorio.py:1976`; `app/ponto/services/cartorio.py:1982`; `app/ponto/services/cartorio.py:1986`; `app/ponto/services/cartorio.py:996`) |
| **pc2 D, complemento** | Duas pausas e pergunta de conversa sem identidade de fato | Duas pausas podem tornar comparação indeterminada; conversa segue ramo sem dedup do fato. **Detalhe de cobertura de casos.** (`app/chamados/juizes.py:1108`; `app/chamados/models.py:2392`; `app/chamados/models.py:2413`; `app/chamados/models.py:2417`) |
| **pc2 D, complemento** | Evento cuja célula foi regenerada | `celula_id` é inteiro sem FK para o evento sobreviver à regeneração. **DESENHO: identidade e universo do consumidor.** (`app/escala/models.py:1853`; `app/escala/models.py:1854`) |
| **pc2 B/D** | Falha da lista administrativa explicitamente tratada | Falha de leitura da lista não autoriza excluir o dia do alarme. **DESENHO: universo de exclusão.** (`app/chamados/services/vigia_fio.py:219`) |
| **pc2 D, complemento** | Resposta nova concorrente com validação como aceite separado | O aval nomeia essa intercalação; resposta nova pode limpar assinatura. **DESENHO: concorrência entre escritores.** (`app/docs/PROMPTS.md:1882`; `app/chamados/services/disputa_emissao.py:1295`) |
| **pc2 B/D** | Exceção formal do selo e condição de saída | A exceção exige medida do fio mudo e pouso do C11 operacional. **Detalhe de verificação da aposentadoria.** (`bin/tests/test_papel_prazo_nao_deriva.sh:303`

[truncado em 20000 de 23754 caracteres]

## 05/11 18:33 · sessao d8df42718d53 · - · - · origem=Ronald (tmux)

Conferência de B: não identifiquei incompatibilidade entre as duas descrições dos sítios centrais; ambas descrevem as mesmas portas e limitações. O acréscimo efetivo no consumidor existe apenas no candidato de pc1, matéria de desenho, pois o consumidor atual termina a reconciliação sem chamar a fábrica (`app/chamados/reconciliador.py:1326`, `app/chamados/reconciliador.py:1329`, `app/chamados/reconciliador.py:1332`).

Coincidências conferidas:

- Registro: C6=`pergunta_viva`, C15=`fio_vivo`, C17=`porta_alcancavel_do_dia`; C11 é ausência, não FIO MUDO (`app/core/juizes.py:266`, `app/core/juizes.py:271`, `app/core/juizes.py:275`, `app/core/juizes.py:277`).
- `abrir` trava colaborador, pode retornar existente/renascido e só emite evento no nascimento real, condicionado a `MODULOS_DE_TURNO`; essa tupla não contém `batida_ausente` (`app/chamados/models.py:866`, `app/chamados/models.py:882`, `app/chamados/models.py:885`, `app/chamados/models.py:933`, `app/chamados/models.py:939`, `app/chamados/reconciliador.py:336`).
- `criar` termina com criação, trilha e retorno, sem emissão explícita nesse corpo (`app/chamados/models.py:1048`, `app/chamados/models.py:1049`, `app/chamados/models.py:1050`).
- Detector tenta fio em `try/except`; grade termina sem emissão direta; C2 usa `abrir` e reapontamento (`app/ponto/management/commands/detectar_ausencias.py:790`, `app/ponto/management/commands/detectar_ausencias.py:801`, `app/ponto/management/commands/detectar_ausencias.py:802`, `app/ponto/management/commands/reconciliar_grade.py:507`, `app/ponto/management/commands/reconciliar_grade.py:531`, `app/chamados/reconciliador.py:879`, `app/chamados/reconciliador.py:922`).
- Sinal do chamado pode retornar durante julgamento e está sob `engolir` (`app/chamados/signals.py:172`, `app/chamados/signals.py:174`).
- Reabertura grava vivo e ressuscita existentes; filtro exige pergunta histórica materializada e pode resultar em zero (`app/chamados/models.py:1382`, `app/chamados/models.py:1391`, `app/chamados/models.py:1423`, `app/chamados/models.py:1482`).
- Duplicata usa `carimbar_identidade` e retorna; reapontamento escreve destino/lavra/save e retorna sem tratar origem nesse corpo (`app/chamados/services/fio.py:891`, `app/chamados/services/fio.py:895`, `app/chamados/services/fio.py:845`, `app/chamados/services/fio.py:846`, `app/chamados/services/fio.py:849`).
- Validação individual e devolução estão em `atomic`; materialização da disputa ocorre depois (`app/chamados/services/validacao.py:395`, `app/chamados/services/validacao.py:422`, `app/chamados/services/validacao.py:425`, `app/chamados/services/validacao.py:430`).
- Fábrica exclui identidade histórica não duplicata, independentemente de C6 (`app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/services/fabrica_da_celula.py:119`, `app/chamados/services/fabrica_da_celula.py:123`, `app/chamados/services/fabrica_da_celula.py:126`).
- Cartório chama fábrica somente para datas presentes no conjunto julgado naquela passada (`app/ponto/services/cartorio.py:986`, `app/ponto/services/cartorio.py:995`, `app/ponto/services/cartorio.py:996`, `app/ponto/services/cartorio.py:997`).
- Cron está configurado com `--apply`, permanece na lista de julgamento por varredura; instrumento já possui `--alarme` sem escrita (`app/config/crons.py:1495`, `app/config/crons.py:2007`, `app/config/crons.py:2011`, `app/chamados/management/commands/disparar_perguntas_competencia.py:251`).

Exclusivos globais relevantes de pc1:

- Matriz formal de campos com escritores/leitores, incluindo quatro portas de contexto e duas de silêncio, ausentes como inventário em pc2 (`app/chamados/services/contexto.py:62`, `app/chamados/services/contexto.py:81`, `app/chamados/services/contexto.py:106`, `app/chamados/services/contexto.py:128`, `app/chamados/services/arquivo.py:141`, `app/chamados/services/arquivo.py:176`).
- Registro explícito de C1/C2/C3/C16/C18 e regra do lookup versus registro vivo (`app/core/juizes.py:261`, `app/core/juizes.py:262`, `app/core/juizes.py:263`, `app/core/juizes.py:276`, `app/core/juizes.py:278`, `app/docs/JUIZES.md:7`, `app/docs/JUIZES.md:14`).
- Tratamento específico da tranca por serviço e gancho no ato (`app/chamados/services/competencia_trancada.py:178`, `app/chamados/services/competencia_trancada.py:184`, `app/chamados/services/competencia_trancada.py:276`, `app/chamados/services/competencia_trancada.py:282`).
- Comando e agenda da relavra stale, distinguindo seu juiz de materialização de C6 (`app/chamados/management/commands/lavrar_perguntas_stale.py:58`, `app/chamados/management/commands/lavrar_perguntas_stale.py:87`, `app/chamados/veredito_lavrado.py:219`, `app/config/crons.py:390`).
- Exclusão explícita de “Resolver dia” de C17, embora o vigia aceite essa ação separadamente (`app/chamados/juizes.py:1367`, `app/chamados/juizes.py:1369`, `app/chamados/services/vigia_fio.py:283`).

Exclusivos globais relevantes de pc2:

- C12/C13 nomeados e registrados como autoridades de alcance da fábrica/retenção (`app/core/juizes.py:119`, `app/core/juizes.py:120`, `app/core/juizes.py:272`, `app/core/juizes.py:273`).
- `canonica_do_grupo` prioriza resposta, depois menor identificador (`app/chamados/services/fio.py:867`, `app/chamados/services/fio.py:873`, `app/chamados/services/fio.py:874`, `app/chamados/services/fio.py:875`).
- `relavrar` como escritor adicional de via; a definição está em 104 e escrita real em 123–126 (`app/chamados/services/relavra_via.py:104`, `app/chamados/services/relavra_via.py:123`, `app/chamados/services/relavra_via.py:124`, `app/chamados/services/relavra_via.py:125`).
- Chamadores irmãos individualizados: dedup, migração, declaração textual, limbo, reata, foto/API, ações e classe A (`app/chamados/management/commands/dedup_perguntas_fato.py:88`, `app/chamados/services/fio.py:943`, `app/chamados/services/fio.py:947`, `app/chamados/services/declaracao_texto.py:320`, `app/chamados/services/limbo_folgas.py:243`, `app/chamados/services/limbo_folgas.py:276`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:122`, `app/api/foto_recebida.py:51`, `app/chamados/services/acoes_disputa.py:375`, `app/chamados/management/commands/validar_classe_a.py:100`).
- Retorno antecipado de `julgar_celula` por ausência de célula ou turno atual ainda em curso (`app/ponto/services/cartorio.py:1976`, `app/ponto/services/cartorio.py:1977`, `app/ponto/services/cartorio.py:1982`, `app/ponto/services/cartorio.py:1986`).
- Falha da lista administrativa não exclui dia e contador textual tem consumidor no placar (`app/chamados/services/vigia_fio.py:195`, `app/chamados/services/vigia_fio.py:219`, `app/chamados/services/vigia_fio.py:225`, `app/chamados/management/commands/disparar_perguntas_competencia.py:235`, `bin/placar_code.sh:381`).
- Exceção formal do selo e condição de saída para cron (`bin/tests/test_papel_prazo_nao_deriva.sh:303`, `bin/tests/test_papel_prazo_nao_deriva.sh:306`, `bin/tests/test_papel_prazo_nao_deriva.sh:307`, `app/docs/CORTES.md:305`).

Cuidados para consolidação:

- Reapontamento irmão **não é SO NUMA global**: pc1 já o inclui na matriz de campos e pc2 o individualiza como sítio; o corpo realmente escreve célula e destino da identidade existente (`app/chamados/models.py:2424`, `app/chamados/models.py:2427`, `app/chamados/models.py:2439`, `app/chamados/models.py:2463`).
- Risco de ordem de trava de pc2 é de desenho; a ordem declarada é Disputa→Colaborador e `abrir` já segura colaborador (`app/chamados/models.py:843`, `app/chamados/models.py:845`, `app/chamados/models.py:866`).
- Universo do ramo é qualquer módulo vivo com célula na janela, sem filtro de `batida_ausente`; pc2 o explicita em D13, pc1 cita a consulta no censo B, portanto diferenciar explicitação de omissão completa (`app/chamados/management/commands/disparar_perguntas_competencia.py:141`).

Citações falsas de B: nenhuma materialmente falsa identificada nos sítios conferidos; conferência literal de todas as referências periféricas de leitores **[não verificado]**. Referências a início de função não devem ser rotuladas falsas quando o corpo sustenta a afirmação: pc1 cita `materializacao.py:728`, cuja reconciliação está em 738 e aplicação a destino/clones em 740–748; pc2 cita `relavra_via.py:104`, cuja escrita está em 123–126 (`app/chamados/services/materializacao.py:728`, `app/chamados/services/materializacao.py:738`, `app/chamados/services/materializacao.py:740`, `app/chamados/services/materializacao.py:748`, `app/chamados/services/relavra_via.py:104`, `app/chamados/services/relavra_via.py:123`).

## 05/11 18:32 · sessao f4be53cc4ea6 · - · - · origem=Ronald (tmux)

Casos equivalentes conferidos:

| pc1 | pc2 | Sustentação na cópia |
|---|---|---|
| A1/A2 | A01/A02 | Evento e idempotência: `CLAUDE.md:168`, `CLAUDE.md:175`. |
| A3 | A03 | Reabertura não equivale a ação nomeada: `app/chamados/services/vigia_fio.py:283`. |
| A4/A5 | A05/A06 | Duplicata preserva identidade; resposta discordante pode continuar viva: `app/chamados/models.py:2617`, `app/chamados/juizes.py:1150`. |
| A7/A8 | A07/A08 | Marco atendido não quita outro; devolução preserva resposta: `app/chamados/juizes.py:1173`, `app/chamados/services/validacao.py:171`. |
| A9/A10 | A10/A11 | Alcance exige porta: `app/chamados/juizes.py:1344`. |
| A11 | A12 | Morte por leitura existente em C6: `app/chamados/juizes.py:1135`, `app/chamados/juizes.py:1140`, `app/chamados/juizes.py:1195`. |
| A13/A14 | A15/A14 | Igualdade vence (`<=`); turno noturno: `app/ponto/services/cartorio.py:846`, `app/chamados/services/fabrica_da_celula.py:18`. |
| A15/A18 | A13/A16 | Janela por empresa; geradora dentro da vigência: `app/ponto/janelas.py:54`, `app/escala/alimentacao.py:272`. |
| A19/A20 | A17/A19 | Ausência de ata; parcial administrativa: `app/chamados/services/disputa_emissao.py:842`, `app/chamados/services/fabrica_da_celula.py:101`. |
| A21 | A09/A20/A21 | Registro não pede ação; ausência/decisão/lista são baldes legítimos: `app/chamados/services/painel.py:174`, `app/chamados/services/vigia_fio.py:283`. |
| A22/A23 histórico | A23 | Hoje fora do alarme; passado até fato novo: `app/chamados/services/vigia_fio.py:420`, `app/docs/LEIS.md:145`. |

Divergência de **DESENHO**: pc1 A23 cita congelamento do gravado exportado (`app/docs/LEIS.md:125`); pc2 A24 cita revogação dessa recusa geral, preservando TXT anterior e trilha (`app/docs/LEIS.md:149`). A cópia declara a queda da recusa geral em `app/ponto/services/fechamento.py:477`, conserva TXT/trilha em `app/ponto/services/fechamento.py:484` e registra exportada por aviso em `app/ponto/services/fechamento.py:532`; ainda há guarda específica em `app/ponto/services/fechamento.py:552`. A fábrica mantém tratamento particular do limbo exportado e permite perguntar em dia de trabalho exportado (`app/chamados/services/fabrica_da_celula.py:167`).

Somente como casos explicitamente discriminados:

- **pc1:** validação integral A6 (`app/chamados/models.py:681`); resposta concordante com a luz A12 (`app/chamados/juizes.py:1090`, `app/chamados/juizes.py:1159`); toque humano posterior à tranca A17 (`app/chamados/juizes.py:1054`, `app/chamados/juizes.py:1071`).
- **pc2:** recusa de reabertura A04, implementada por decisão humana sem fato novo e premissa regularizada (`app/chamados/models.py:1292`, `app/chamados/models.py:1298`); ramo passado sem escala A18 (`app/chamados/services/fabrica_da_celula.py:21`, `app/chamados/services/fabrica_da_celula.py:52`).

O caso pc2 A22 “viva sem porta” também aparece conceitualmente em pc1 B/D; C17 distingue vitalidade e alcance e exclui expressamente “Resolver dia” (`app/chamados/juizes.py:1344`, `app/chamados/juizes.py:1367`). Ausência aberta, registro e decisão legítima estão agrupados em pc1 A21 e separados em pc2; isso é detalhe de exposição (`app/chamados/services/vigia_fio.py:283`, `app/chamados/services/vigia_fio.py:287`, `app/chamados/services/painel.py:174`).

Não classificar `app/docs/LEIS.md:125` como citação falsa: a linha sustenta literalmente o congelamento, mas foi alterada pela regra de `app/docs/LEIS.md:149`. A divergência é normativa e material; falsidade da localização não foi demonstrada. As outras referências da seção A examinadas sustentam seus núcleos, com a ressalva de que `app/docs/LEIS.md:65` trata de escritor/trilha e não basta, isoladamente, para provar preservação da decisão humana (`app/docs/LEIS.md:65`, `app/chamados/models.py:1292`).

_1969 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
