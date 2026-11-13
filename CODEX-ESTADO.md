# CODEX-ESTADO

> Ultima resposta processada: **13/11/2026 11:10** · publicado em **13/11/2026 11:10** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 13/11 11:10 · sessao 554163499bde · - · - · origem=Ronald (tmux)

F: **não encontrei quebra de contrato pela entrada nova**: ela declara o comando sob ordem em `FORA_DE_PIPELINE`, com motivo (`app/config/crons.py:1407`).

O contrato B6.1 enumera arquivos de comandos (`app/chamados/tests/test_contract_crons.py:214`), exige casa em agendados ou `FORA_DE_PIPELINE` (`app/chamados/tests/test_contract_crons.py:220`) e proíbe entrada simultaneamente agendada ou sem arquivo vivo (`app/chamados/tests/test_contract_crons.py:230`). A fonte dos agendados contém somente `CRONS + ENCADEADOS` (`app/config/crons.py:1805`); o render percorre somente `CRONS` (`app/config/crons.py:1872`).

Reproduzi essas duas operações B6.1 por `python3 -B`, compilando a fonte em memória: `sem casa=[]`, `podres=[]`; `aplicar_feriado_lei_1609` está vivo, está fora do pipeline e não está agendado. Resultado de execução transitória, sem arquivo de evidência persistido: `[nao verificado]`. Os predicados reproduzidos estão em `app/chamados/tests/test_contract_crons.py:225` e `app/chamados/tests/test_contract_crons.py:234`.

Os demais contratos encontrados sobre estágio, dependências, horários e render examinam `CRONS`, portanto a entrada somente no dicionário de exclusões não os amplia (`app/chamados/tests/test_contract_crons.py:77`, `app/chamados/tests/test_contract_crons.py:269`, `app/chamados/tests/test_contract_crons.py:301`, `app/chamados/tests/test_contract_crons.py:330`).

## 13/11 10:52 · sessao bf56601fcd1a · FERIADO-NASCE-DA-ESCALA · verificador · origem=Code

**REPROVA.** Há caminhos que mantêm ou recriam `5x2=True`, herança sem escolha obrigatória, reversão incompleta e escolha humana sem efeito em dois ciclos. (`app/colaboradores/services/vinculo.py:1027`, `app/colaboradores/services/vinculo.py:1078`, `app/colaboradores/services/vinculo.py:185`, `app/escala/models.py:1422`)

A correspondência da cópia com o hash Git informado, execução da suíte, opus, deploy, aplicação dos 16 e resultados posteriores em produção permanecem **[não verificado]**. [nao verificado]

**A. Todos os nascimentos e reaproveitamentos encontrados**

| Sítio de produção | Origem de `trabalha_em_feriado` |
|---|---|
| `executar_vinculo`: criação | `v['trabalha_em_feriado']`, calculado por `_feriado_do_veredito`. (`app/colaboradores/services/vinculo.py:144`, `app/colaboradores/services/vinculo.py:329`, `app/colaboradores/services/vinculo.py:338`) |
| `executar_vinculo`: reaproveitamento da mesma data | Troca o tipo e recebe a flag do mesmo veredito. (`app/colaboradores/services/vinculo.py:315`, `app/colaboradores/services/vinculo.py:317`) |
| `criar_trecho_retroativo` | `feriado_do_trecho(tipo_destino, flag_original)`. (`app/colaboradores/services/vinculo.py:1017`, `app/colaboradores/services/vinculo.py:1021`) |
| `ajustar_vinculo_pelo_sistema` | `feriado_do_trecho(tipo_derivado, flag_original)`. (`app/colaboradores/services/vinculo.py:1136`, `app/colaboradores/services/vinculo.py:1139`) |
| `desfazer_ajuste_do_sistema`: renascimento | `feriado_do_trecho(tipo_antigo, flag_antiga)`. (`app/colaboradores/services/vinculo.py:1158`, `app/colaboradores/services/vinculo.py:1161`) |
| `desfazer_ajuste_do_sistema`: reabertura | Reativa o antigo, mantendo sua flag sem novo julgamento. (`app/colaboradores/services/vinculo.py:1149`, `app/colaboradores/services/vinculo.py:1154`) |
| `corrigir_vinculo_retroativo`: troca de tipo | **Mantém a flag anterior sem consultar o juiz.** (`app/colaboradores/services/vinculo.py:1027`, `app/colaboradores/services/vinculo.py:1030`) |
| `reverter_para_snapshot`: restauração de tipo | **Mantém a flag atual; o snapshot não contém a anterior.** (`app/colaboradores/services/vinculo.py:108`, `app/colaboradores/services/vinculo.py:1078`) |

Nos quatro `create` encontrados, a flag é explícita. Não encontrei outro nascimento por `get_or_create`, `update_or_create`, construtor ou `bulk_create` na produção examinada; ausência de caminhos dinâmicos adicionais é **[não verificado]**. Os quatro nascimentos estão em `app/colaboradores/services/vinculo.py:329`, `app/colaboradores/services/vinculo.py:1017`, `app/colaboradores/services/vinculo.py:1136` e `app/colaboradores/services/vinculo.py:1158`.

**Bloqueante 1 — troca retroativa fora do julgamento:** `6x1=True → 5x2` pela correção no lugar continua `True`. O comando chama exatamente essa função. (`app/colaboradores/services/vinculo.py:1027`, `app/colaboradores/services/vinculo.py:1030`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:212`)

**Bloqueante 2 — reversão do aceite incompleta:** reaproveitar `5x2=False` como `12x36=True` e depois recusar restaura o tipo `5x2`, mas deixa `True`. Uma mudança somente da flag também não é desfeita. O snapshot omite a flag e a restauração não a escreve. (`app/colaboradores/services/vinculo.py:108`, `app/colaboradores/services/vinculo.py:317`, `app/colaboradores/services/vinculo.py:1078`, `app/chamados/services/acoes_chamado.py:149`)

O default perigoso continua `True`; a proteção depende das portas e do selo, que tem escapes descritos em G. (`app/escala/models.py:897`, `app/escala/tests/test_feriado_nasce_da_escala_selo.py:90`)

**B. Matriz completa e identidade do substituído**

A função não recebe `acao`: a tabela abaixo vale igualmente quando chamada durante `criar`, `atualizar` ou o candidato inicial a `no_op`. `R` significa `ValueError`; `S` significa a flag armazenada no substituído. (`app/colaboradores/services/vinculo.py:144`, `app/colaboradores/services/vinculo.py:165`)

| Juiz | Substituído | `''`/ausente | `'1'` | `'0'` | Inválida |
|---|---|---:|---:|---:|---:|
| `True` | Nenhum / mesmo ciclo / outro ciclo | `True` | `True` | R | R |
| `False` | Nenhum / mesmo ciclo / outro ciclo | `False` | R | `False` | R |
| `None` | Nenhum | R | `True` | `False` | R |
| `None` | Mesmo ciclo | S | `True` | `False` | R |
| `None` | Outro ciclo | R | `True` | `False` | R |

A validação da escolha inválida precede o juiz; as respostas fixas vêm antes da herança; o mesmo ciclo somente é considerado quando o juiz retorna `None`. (`app/colaboradores/services/vinculo.py:173`, `app/colaboradores/services/vinculo.py:178`, `app/colaboradores/services/vinculo.py:185`)

O produto cartesiano tem combinações inalcançáveis pelo chamador:

- **`criar`:** não existe vínculo da mesma data; o substituído pode ser ausente, ativo de mesmo ciclo ou ativo de outro ciclo. A ação continua `criar`. (`app/colaboradores/services/vinculo.py:102`, `app/colaboradores/services/vinculo.py:125`, `app/colaboradores/services/vinculo.py:141`)
- **`atualizar`:** existe vínculo da mesma data, inclusive inativo; ele sempre ganha a preferência como substituído. Pode ter mesmo ciclo ou outro ciclo, mas não ser `None`. (`app/colaboradores/services/vinculo.py:122`, `app/colaboradores/services/vinculo.py:141`)
- **Candidato a `no_op`:** existente ativo, mesmo template e âncora compatível; portanto substituído existente e mesmo ciclo. Se a flag calculada difere, vira `atualizar`; caso contrário permanece `no_op`. (`app/colaboradores/services/vinculo.py:119`, `app/colaboradores/services/vinculo.py:145`)
- **Outras ações:** `julgar_vinculo` somente produz essas três. (`app/colaboradores/services/vinculo.py:120`)

**Bloqueante 3 — nascimento sem escolha:** `criar` um novo vínculo `6x1` em outra data, tendo substituído `6x1`, aceita escolha vazia e herda a flag. Não exige prova de escolha humana; um `True` legado pelo default passa como escolha. Isso é uma exceção implementada à obrigatoriedade de escolha no nascimento. (`app/colaboradores/services/vinculo.py:125`, `app/colaboradores/services/vinculo.py:170`, `app/colaboradores/services/vinculo.py:185`)

**O substituído não representa sempre o vínculo cujo período está sendo substituído:**

- Sem existente na mesma data, a consulta pega o ativo mais recente **sem limitar seu início a D**. Se ele começa depois de D, `fechar_vigencia` o recusa e `absorver_vigencias_posteriores` o absorve. A afirmação de que seria “o ativo que o executor fecha” é falsa nesse caso. (`app/colaboradores/services/vinculo.py:141`, `app/colaboradores/services/vinculo.py:664`, `app/colaboradores/services/vinculo.py:830`)
- Uma vigência inativa anterior que cobre D também pode ser encerrada pelo executor, mas não participa dessa busca de substituído. (`app/colaboradores/services/vinculo.py:141`, `app/colaboradores/services/vinculo.py:282`)
- Existindo vínculo **inativo na mesma data**, ele vence qualquer ativo que também será fechado ou absorvido. Há um objeto reaproveitado definido, mas isso não prova que sua flag é a escolha do período efetivamente substituído. (`app/colaboradores/services/vinculo.py:102`, `app/colaboradores/services/vinculo.py:141`, `app/colaboradores/services/vinculo.py:255`, `app/colaboradores/services/vinculo.py:294`)

Consequência derivável: um futuro `6x1=True` pode fornecer a herança para um novo `6x1` anterior à sua vigência; um futuro de outro ciclo pode provocar recusa mesmo havendo uma vigência anterior `6x1` cobrindo D. São caminhos demonstráveis pelo código; ocorrência nos dados fornecidos é **[não verificado]**. (`app/colaboradores/services/vinculo.py:141`, `app/colaboradores/services/vinculo.py:185`, `app/colaboradores/services/vinculo.py:282`)

**`no_op → atualizar` não executa uma operação restrita à flag.** Ele percorre fechamento e absorção, redefine posto, ativa o vínculo, apaga `data_fim`, pode copiar calendário e recalibrar âncora. Essas operações são incondicionais no executor. (`app/colaboradores/services/vinculo.py:255`, `app/colaboradores/services/vinculo.py:294`, `app/colaboradores/services/vinculo.py:316`, `app/colaboradores/services/vinculo.py:319`, `app/colaboradores/services/vinculo.py:326`, `app/colaboradores/services/vinculo.py:351`)

A regeneração passada e futura é acionada, e a trilha técnica inclui a flag. Entretanto, o histórico classifica essa alteração como `ancora`, enquanto o impacto não recebe a nova flag e calcula o “depois” pela aritmética do template. Logo a prévia não representa fielmente uma alteração somente de feriado. (`app/colaboradores/services/vinculo.py:370`, `app/colaboradores/services/vinculo.py:380`, `app/colaboradores/services/vinculo.py:395`, `app/colaboradores/services/vinculo.py:418`, `app/escala/services/impacto_troca.py:22`, `app/escala/services/impacto_troca.py:40`)

**C. Chamadores da porta**

| Chamador | Passa a escolha? Efeito |
|---|---|
| Tela Vincular | Encaminha `request.POST`; os rádios enviam `'1'`/`'0'`, sem pré-seleção. (`app/colaboradores/views.py:411`, `app/colaboradores/views.py:423`, `app/templates/colaboradores/partials/painel_vinculo.html:176`) |
| Chat: prévia | Não. `_post` monta template, início, modo e eventualmente âncora; a recusa retorna em `erros`. (`app/colaboradores/services/troca_pelo_chat.py:11`, `app/colaboradores/services/troca_pelo_chat.py:58`, `app/colaboradores/services/troca_pelo_chat.py:60`) |
| Chat: execução | Não; a interface nem recebe argumento de feriado. Novo vínculo sem substituído ou troca de ciclo para um ciclo sem padrão passa a ser recusado. Mesmo ciclo pode herdar. (`app/colaboradores/services/troca_pelo_chat.py:64`, `app/colaboradores/services/troca_pelo_chat.py:72`, `app/colaboradores/services/vinculo.py:185`) |
| Régua de defesa/realocar | Não. O dict omite a flag e captura a nova recusa. Destinos cíclicos já são recusados antes; destinos sem padrão precisam satisfazer a herança. (`app/escala/regua_defesa.py:310`, `app/escala/regua_defesa.py:313`, `app/escala/regua_defesa.py:316`) |
| `censo_fase_12x36` | Não. O dict tem template/início/âncora; para destino 12x36 o juiz fornece `True`. (`app/ponto/management/commands/censo_fase_12x36.py:419`, `app/ponto/management/commands/censo_fase_12x36.py:422`, `app/escala/models.py:921`) |

Não encontrei chamador direto adicional por API/importação nas buscas estáticas; caminhos dinâmicos permanecem **[não verificado]**. O Django admin do vínculo é somente leitura. (`app/escala/admin.py:18`)

**D. Trechos, passado e competência exportada**

`criar_trecho_retroativo` realmente alimenta regeneração do passado: o comando cria o trecho e chama a porta até `min(fim, hoje)`, além do suplemento futuro. (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:194`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:223`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:242`)

O novo `feriado_do_trecho` pode mudar a flag copiada: destino 5x2 recebe `False`; 12x36/24x48 recebe `True`; demais destinos recebem `bool(flag_original)`. Assim, também cria `12x36=True → 6x1=True` sem perguntar ao DP, embora a porta principal recuse troca de ciclo sem escolha. (`app/escala/models.py:921`, `app/escala/models.py:928`, `app/escala/models.py:934`, `app/colaboradores/services/vinculo.py:188`)

Por padrão, a regeneração consulta as competências protegidas e pula seus dias; células humanas também não são reescritas pelo ramo de regeneração. A autoridade considera TXT entregue e holerite publicado. (`app/ponto/portas/celula.py:221`, `app/ponto/portas/celula.py:284`, `app/ponto/portas/celula.py:297`, `app/folha/export.py:617`)

Existe uma exceção explícita preexistente: `corrigir_escala_retroativa --apesar-da-lavra` passa o motivo à porta, que esvazia a lista de proteção. Portanto “nunca toca exportada” não é verdade para todas as opções desse comando. Uso dessa exceção nesta obra é **[não verificado]**. (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:42`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:233`, `app/ponto/portas/celula.py:218`)

Já os chamadores do ajuste automático trabalham somente no futuro: aplicação exige `inicio > hoje`; desfazer usa `max(inicio, hoje+1)` e regenera dali em diante. (`app/escala/services/escala_auto_executor.py:100`, `app/escala/services/escala_auto_executor.py:107`, `app/escala/services/escala_auto_executor.py:143`, `app/escala/services/escala_auto_executor.py:147`)

**E. Auditoria, autoria e idempotência**

Com `autor` autenticado, `_ReqDoAutor` fornece o usuário, `registrar_log` o encaminha como ator e `evento` o grava em `LogAuditoria.usuario`. Com `autor=None`, uma request corrente pode fornecer o usuário pelo contexto; fora dela, fica a identificação do processo, sem usuário humano. (`app/colaboradores/services/vinculo.py:19`, `app/colaboradores/services/vinculo.py:939`, `app/core/auditoria.py:24`, `app/core/observ.py:108`, `app/core/observ.py:118`)

`sincronizar_feriado_admin` chama sem autor e sem motivo. Não há uma segunda gravação explícita de auditoria nesse loop; a nova linha vem da porta. (`app/colaboradores/management/commands/sincronizar_feriado_admin.py:58`, `app/colaboradores/management/commands/sincronizar_feriado_admin.py:61`, `app/colaboradores/services/vinculo.py:939`)

Mesmo valor **na instância recebida** retorna antes de save, trilha e regeneração. A porta não relê nem trava o valor persistido antes dessa comparação; portanto isso não prova idempotência sob instância desatualizada ou concorrência. (`app/colaboradores/services/vinculo.py:932`, `app/colaboradores/services/vinculo.py:936`)

**Bloqueante 4 — trilha não é garantia da transação:** `registrar_log` captura falhas; `evento` usa savepoint e engole falha da própria auditoria. A flag pode confirmar sem `LogAuditoria`, apesar do `@atomic` da porta. (`app/colaboradores/services/vinculo.py:910`, `app/core/auditoria.py:25`, `app/core/observ.py:105`, `app/core/observ.py:133`, `app/core/observ.py:139`)

O sincronizador também continua derivando a escolha dos ciclos sem padrão pelo cargo. Pode sobrescrever `6x1` sem resposta DP; não há bloqueio que o restrinja à “só lista” desta obra. Execução dele nos 82 é **[não verificado]**. (`app/colaboradores/management/commands/sincronizar_feriado_admin.py:51`, `app/colaboradores/management/commands/sincronizar_feriado_admin.py:61`, `app/escala/models.py:934`)

**F. Lote, reversão, atomicidade e relógio**

- **Universo:** vínculo que cobre hoje, colaborador ativo, ciclo 5x2 e flag `True`. Não é simplesmente `ativa=True`: pode incluir inativo com fim vigente e excluir ativo futuro. Igualdade com os “16 ativos” raspados é **[não verificado]**. (`app/escala/models.py:943`, `app/escala/models.py:945`, `app/escala/alimentacao.py:219`)
- **Lista:** compara exatamente universo e `--esperado`, antes de escrever. Imprime vínculo/colaborador/tipo/valor, mas não verifica publicação prévia no RELATO. Publicação da lista e do censo é **[não verificado]**. (`app/escala/management/commands/aplicar_feriado_nasce_da_escala.py:35`, `app/escala/management/commands/aplicar_feriado_nasce_da_escala.py:43`)
- **Reversão antes da escrita:** correto na ordem do programa. O JSON completo fecha antes do loop; modo `x` impede sobrescrita. É uma lista de valores anteriores, sem diário de progresso. (`app/escala/management/commands/aplicar_feriado_nasce_da_escala.py:51`, `app/escala/management/commands/aplicar_feriado_nasce_da_escala.py:55`)
- **Atomicidade:** por vínculo, não pelo lote. Queda após N commits deixa N aplicados e os demais intactos; a reversão contém todos. Repetir a lista original falha na igualdade do universo; reutilizar o arquivo falha no `open('x')`. Não há retomada automática. (`app/colaboradores/services/vinculo.py:910`, `app/escala/management/commands/aplicar_feriado_nasce_da_escala.py:38`, `app/escala/management/commands/aplicar_feriado_nasce_da_escala.py:51`)
- **Efeitos incompletos:** rejulgamento captura falhas; dinheiro e E2 têm efeitos após commit que também podem falhar sem desfazer a flag. Repetir `marcar(False)` depois retorna imediatamente, sem reparar esses efeitos. Contador zero não prova cura completa. (`app/ponto/portas/celula.py:475`, `app/ponto/portas/celula.py:540`, `app/ponto/services/fechamento.py:1818`, `app/colaboradores/services/vinculo.py:932`)
- **Exportadas:** a regeneração ordinária do lote mantém a guarda, pois não passa `apesar_da_lavra`. Entretanto, o `save()` também invalida o previsto dos fechamentos do colaborador, sem filtro de competência exportada: muda `previsto_em`/`atualizado_em`, embora isso não demonstre alteração monetária. (`app/colaboradores/services/vinculo.py:936`, `app/colaboradores/services/vinculo.py:951`, `app/ponto/portas/celula.py:218`, `app/escala/signals.py:138`, `app/ponto/services/fechamento.py:1611`)
- **20/11:** o suplemento só regenera células futuras geradas **já existentes**, na janela calculada. Não cria uma célula ausente. As 16 células de 20/11 com `False` são **[não verificado]**. (`app/ponto/services/cadastro_futuro.py:26`, `app/ponto/services/cadastro_futuro.py:31`, `app/ponto/services/cadastro_futuro.py:43`)

**Bloqueante 5 — relógio fora da borda:** o comando chama o contador duas vezes sem fornecer hoje; cada marcação lê outro hoje depois de salvar; o signal do save lê seu próprio hoje; o rejulgamento interno lê novamente. Uma passagem pela meia-noite pode usar datas distintas no mesmo lote. (`app/escala/management/commands/aplicar_feriado_nasce_da_escala.py:36`, `app/escala/management/commands/aplicar_feriado_nasce_da_escala.py:57`, `app/escala/models.py:943`, `app/colaboradores/services/vinculo.py:944`, `app/ponto/signals.py:63`, `app/ponto/portas/celula.py:464`)

**G. Alcance real do selo AST**

| Construção | Resultado do scanner |
|---|---|
| `from ... import EscalaColaborador as EC`; `m.EscalaColaborador.objects.create` | Reconhece. (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:34`, `app/escala/tests/test_feriado_nasce_da_escala_selo.py:44`) |
| `EC = EscalaColaborador`; manager guardado em variável; manager custom/`_default_manager` | Escapa: nascimento exige `.objects` diretamente no nome reconhecido. (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:90`) |
| `_meta.model`, `__class__` | Escapa ao reconhecimento do modelo. (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:44`, `app/escala/tests/test_feriado_nasce_da_escala_selo.py:90`) |
| Construtor reconhecido sem flag, seguido de `save()` | Pega o construtor. (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:93`) |
| Instância carregada/copied/refresh, `pk=None`, `save()` | Escapa: não acompanha identidade ou `save()`. (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:90`, `app/escala/tests/test_feriado_nasce_da_escala_selo.py:102`) |
| `qs.update(trabalha_em_feriado=...)`, queryset em variável | Pega; não precisa conhecer a origem do queryset. (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:102`) |
| `qs.update(**dados)`, `bulk_update`, escrita por `__dict__` | Escapa às formas examinadas. (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:102`, `app/escala/tests/test_feriado_nasce_da_escala_selo.py:108`) |

**Bloqueante 6 — procedência falsificável:** qualquer `v['trabalha_em_feriado']` é aceito, mesmo vindo de um dict literal; qualquer chamada chamada `feriado_do_trecho` também é aceita, sem conferir módulo ou autoridade. O selo não garante que o valor veio do veredito real. (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:48`)

**O mutante literal morde:** retirar a keyword ou substituí-la por `True` no nascimento atual gera achado. O teste faz essas duas mutações; isso não cobre os escapes acima nem equivale a uma campanha de mutação do juiz. (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:137`, `app/escala/tests/test_feriado_nasce_da_escala_selo.py:143`)

O RED “col148” cria colaborador, vínculo e célula sintéticos. Verifica flag

[truncado em 20000 de 27330 caracteres]

## 13/11 10:50 · sessao 3dc36bb74794 · O168 · verificador · origem=Code

**APROVA o delta — sem bloqueantes demonstrados.** A referência nua deixou de contar como chamada, e os dois pontos de atribuição de erros passaram a usar o mesmo separador. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:288`; `app/chamados/services/validacao.py:142`; `app/chamados/services/validacao.py:510`.)

**A. Contador e fila.** `qs_na_fila` aceita `_base`: repassa-o a `qs_vivos`, que usa esse queryset diretamente e aplica `VIVOS`; depois restringe a `COBRANCA` e exclui silêncio futuro. O juiz da fila é `chamados/services/painel.py::qs_na_fila`. (`app/chamados/services/painel.py:153`; `app/chamados/services/painel.py:158`; `app/chamados/services/painel.py:188`; `app/chamados/services/painel.py:190`.)

O significado **muda**: de recusados ainda vivos para recusados ainda cobrados na fila, excluindo `registrado` e silenciados no futuro. A docstring declara o universo novo. (`app/chamados/catalogo/motor.py:24`; `app/chamados/catalogo/motor.py:29`; `app/chamados/juizes.py:2512`; `app/chamados/juizes.py:2525`.)

O leitor executável localizado é `_estado` do selo, que publica o número como `segurados`; não localizei consumidor de placar. Há também uma referência documental em `_registrar_recusa`, sem chamada ao contador. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:67`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:75`; `app/chamados/models.py:712`.) Consumidores externos à cópia: **[não verificado]**.

**B. Escritores e donos.** A entrada é `erros_proprios` da unidade de materialização mais `res['erros']` da disputa. O import por `models.py` reexporta a implementação de `materializacao.py`; não constitui outro escritor. (`app/chamados/services/validacao.py:486`; `app/chamados/services/validacao.py:492`; `app/chamados/services/validacao.py:493`; `app/chamados/models.py:4143`.)

Na tabela, **S(e)** significa todos os pks cuja marca `Pergunta #<pk>` era substring da mensagem — incluindo prefixos numéricos e perguntas citadas depois. A cura atribui exclusivamente ao primeiro identificador admissível. (`app/chamados/services/validacao.py:113`; `app/chamados/services/validacao.py:116`; `app/chamados/tests/test_bug29_30_log_validacao.py:117`; `app/chamados/tests/test_bug29_30_log_validacao.py:124`.)

| Escritor/caso que alimenta `erros` | Arquivo:linha | Dono antes → depois |
|---|---|---|
| `_ts_tipo_do_plantio`: sem data | `app/chamados/services/materializacao.py:352` | S(e) → `p.pk` |
| Resposta de escape | `app/chamados/services/materializacao.py:361` | S(e) → `p.pk` |
| Default do relógio sem marco correspondente | `app/chamados/services/materializacao.py:377` | S(e) → `p.pk` |
| Hora inválida, incluindo exceção | `app/chamados/services/materializacao.py:421` | S(e) → `p.pk` |
| `_declarar_ausencia_de_pergunta`: sem data | `app/chamados/services/materializacao.py:484` | S(e) → `p.pk` |
| `_materializar_resposta_hora`: sem resposta | `app/chamados/services/materializacao.py:843` | S(e) → `p.pk` |
| Vizinha do mesmo tipo/paridade | `app/chamados/services/materializacao.py:884` | S(e) → `p.pk` |
| Conflito devolvido por `registrar_batida` | `app/chamados/services/materializacao.py:942` | S(e) → `p.pk` |
| `_materializar_resposta_data_hora`: parse inválido | `app/chamados/services/materializacao.py:1010` | S(e) → `p.pk` |
| Conflito no plantio da âncora | `app/chamados/services/materializacao.py:1027` | S(e) → `p.pk` |
| Gate `DATA_EM_DISPUTA` | `app/chamados/services/materializacao.py:1201`; `app/chamados/services/materializacao.py:1205` | S(e) → `p.pk` |
| `_Reprovada`: não gerou batida | `app/chamados/services/materializacao.py:1245`; `app/chamados/services/materializacao.py:1290` | S(e) → `p.pk` |
| `_Reprovada`: C6 ainda considera viva | `app/chamados/services/materializacao.py:1285`; `app/chamados/services/materializacao.py:1290` | S(e) → `p.pk` |
| Outras exceções capturadas pela unidade | `app/chamados/services/materializacao.py:1291`; `app/chamados/services/materializacao.py:1292` | S(e) → `p.pk` |

Todos esses formatos colocam o identificador do dono antes dos conteúdos dinâmicos. Conflitos e exceções são embrulhados depois de `Pergunta #p.pk:`; portanto uma pergunta citada dentro deles não antecede a dona. (`app/chamados/services/materializacao.py:421`; `app/chamados/services/materializacao.py:943`; `app/chamados/services/materializacao.py:1010`; `app/chamados/services/materializacao.py:1028`; `app/chamados/services/materializacao.py:1292`.)

As particularidades são:

- **`__REUSO__Pergunta #p.pk`** escreve temporariamente em `erros`, mas o `finally` remove a mensagem dessa lista e a transfere para `reusos`. Se entregue diretamente ao helper, seria S(e) → `p.pk`. (`app/chamados/services/materializacao.py:872`; `app/chamados/services/materializacao.py:1297`; `app/chamados/services/materializacao.py:1301`; `app/chamados/services/validacao.py:106`.)
- **`DATA_EM_DISPUTA Pergunta #p.pk`** permanece reconhecido: o prefixo não contém outro identificador de pergunta. (`app/chamados/services/materializacao.py:1201`; `app/chamados/tests/test_bug29_30_log_validacao.py:136`.)
- **`DecisaoPergunta #N`** é escrito pelas exceções de imutabilidade de outra entidade. Isolado, antes casava com a pergunta N e seus prefixos; agora não tem dono. Capturado pela unidade, recebe antes o prefixo da pergunta processada. Ocorrência efetiva dessa exceção nesse caminho: **[não verificado]**. (`app/chamados/models.py:3961`; `app/chamados/models.py:3965`; `app/chamados/services/materializacao.py:1292`; `app/chamados/tests/test_bug29_30_log_validacao.py:134`.)
- **Sem pk:** se recebido, não era próprio de pergunta alguma e continua sem dono, entrando em `irmas`. Não identifiquei escritor direto desse formato no fluxo enumerado. (`app/chamados/services/validacao.py:116`; `app/chamados/services/validacao.py:124`; `app/chamados/services/materializacao.py:1392`.)
- **Fallback de `erros_da_pergunta`:** é mensagem de saída, não novo insumo da materialização; abre pelo pk consultado e informa ausência de causa própria. (`app/chamados/services/validacao.py:142`; `app/chamados/services/validacao.py:145`.)

**C. Leitores de `irmas`.** Para mensagens com um único identificador legítimo, sem coincidência de prefixo, permanecem os mesmos itens, textos e ordem. O separador acrescenta o objeto original à lista escolhida. (`app/chamados/services/validacao.py:120`; `app/chamados/services/validacao.py:124`.)

Os leitores não refazem a partição: o log da recusa usa `erros_da_pergunta`; o log das irmãs concatena a lista; `traduzir_recusa` classifica motivos, sem conhecer pk; a tela consome esses resultados. (`app/chamados/services/validacao.py:519`; `app/chamados/services/validacao.py:532`; `app/chamados/catalogo/recusa.py:79`; `app/chamados/catalogo/recusa.py:81`; `app/chamados/services/acoes_disputa.py:34`; `app/chamados/services/acoes_disputa.py:59`.)

**Não há invariância total fora do prefixo numérico:** `Pergunta #5: ... Pergunta #7` deixa de ser próprio da #7. Essa mudança é deliberadamente cobrada pelo teste novo. Antes, a mensagem podia ser própria em duas consultas diferentes; dentro de uma mesma chamada, `in`/`not in` já produziam listas complementares. (`app/chamados/tests/test_bug29_30_log_validacao.py:125`; `app/chamados/tests/test_bug29_30_log_validacao.py:127`; `app/chamados/tests/test_bug29_30_log_validacao.py:129`; `app/chamados/services/validacao.py:113`.)

**Ressalva do aviso:** `aviso_das_irmas` continua coletando todas as menções, não apenas donos. Recebendo aquele erro como irmã da #7, pode anunciar “1 pergunta” com IDs `#5, #7`, incluindo a própria #7. É uma inconsistência demonstrável com o exemplo sintético; ocorrência produtiva: **[não verificado]**. Esse coletor apresenta referências e não constitui outro particionador de erros. (`app/chamados/services/validacao.py:547`; `app/chamados/services/validacao.py:549`; `app/chamados/tests/test_bug29_30_log_validacao.py:127`.)

**D. Lookbehind.** Não perde nenhum escritor legítimo identificado. Veta somente letra ASCII imediatamente anterior; espaço, `_` e dígito são aceitos. Assim, `__REUSO__Pergunta`, `_Pergunta` e `3Pergunta` são reconhecidos; `DecisaoPergunta` é excluído. O uso de `search` encontra o primeiro token admissível, sem exigir início literal da string. (`app/chamados/services/validacao.py:106`; `app/chamados/services/validacao.py:116`; `app/chamados/tests/test_bug29_30_log_validacao.py:134`; `app/chamados/tests/test_bug29_30_log_validacao.py:135`.)

**E. Outro separador por substring.** Na varredura textual da cópia, não localizei outro sítio de produção com essa separação. Os dois pontos migrados delegam ao helper; o regex restante do aviso enumera menções. Ausência de variantes construídas dinamicamente: **[não verificado]**. (`app/chamados/services/validacao.py:142`; `app/chamados/services/validacao.py:510`; `app/chamados/services/validacao.py:547`.)

**F. Classes de reprovação.** Não demonstrei novo juiz paralelo nem campo simultaneamente operando com dois significados: o contador consulta a fila existente, e a atribuição de erros tem implementação compartilhada. A mudança de universo do contador está declarada. (`app/chamados/juizes.py:2512`; `app/chamados/juizes.py:2525`; `app/chamados/services/validacao.py:142`; `app/chamados/services/validacao.py:510`.)

O bloqueante da volta 4 foi curado: o fecho exige `ast.Call`, e o teste rejeita a auxiliar apenas referenciada. Permanece o limite anterior de aceitar uma chamada cujo resultado seja descartado; ele não foi introduzido pelo delta. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:288`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:334`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:389`.)

A catraca mantém a linha na identidade e `EXCECOES` vazio; as fixtures passam a fornecer os campos do juiz, delegar ao corpo canônico e zerar também o texto quando pretendem representar silêncio. (`app/chamados/tests/e4b_c12_ast.py:18`; `app/chamados/tests/e4b_c12_ast.py:29`; `app/chamados/tests/e4b_c12_ast.py:222`; `app/ponto/tests/test_supra_juiz_executor.py:24`; `app/ponto/tests/test_supra_juiz_executor.py:46`; `app/chamados/tests/test_pep_reentrada.py:280`.)

Executei em memória os seis testes puros de atribuição e os três adversariais do AST: passaram. Restaurar a substring reprova os testes de prefixo e segunda citação; restaurar o fecho por referência nua reprova a isca. Há mordida nessas curas. (`app/chamados/tests/test_bug29_30_log_validacao.py:104`; `app/chamados/tests/test_bug29_30_log_validacao.py:117`; `app/chamados/tests/test_bug29_30_log_validacao.py:124`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:369`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:389`.)

Execução integral Django/ORM, banco, logs e identidade Git do HEAD: **[não verificado]**.

## 13/11 10:48 · sessao 85aeb4ade445 · - · - · origem=Ronald (tmux)

APROVA no escopo do delta: o exemplo multipergunta testa deliberadamente a propriedade exclusiva do primeiro identificador; `irmas` não é invariável nesses casos. (`app/chamados/services/validacao.py:116`; `app/chamados/services/validacao.py:123`.)

O aviso que inclui a própria pergunta merece ressalva, mas, sem escritor real alcançável produzindo esse formato, não demonstra bloqueante atual; ocorrência produtiva: [não verificado].

## 13/11 10:47 · sessao 259a91e99da0 · - · - · origem=Ronald (tmux)

G — Selo AST

- Executei somente a função `achados` em memória, sem Django, banco ou arquivos temporários: árvore de produção retornou `[]`; isso confirma apenas o alcance implementado pelo scanner, que percorre `.py` e exclui testes/migrations/logs (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:28`, `:77`).
- Apelido de import de classe (`from ... import EscalaColaborador as EC`) e módulo (`m.EscalaColaborador`) são reconhecidos; atribuição posterior `EC = EscalaColaborador` escapa (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:34`, `:44`).
- Escapam `manager = EscalaColaborador.objects; manager.create(...)`, manager custom/`_default_manager`, `ec._meta.model.objects.create(...)` e `ec.__class__.objects.create(...)`: o nascimento exige `.objects` diretamente ligado ao nome reconhecido (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:90`).
- `qs.update(trabalha_em_feriado=True)` é pego mesmo com queryset em variável; `qs.update(**dados)`, `bulk_update`, alteração por `__dict__`, cópia/refresh com `pk=None` seguida de `save()` escapam. O scanner só examina keyword literal de `update`, `setattr` com string literal e atribuição direta ao atributo; não rastreia `save`, dados nem identidade da instância (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:102`).
- Construtor reconhecido sem flag é pego antes do `save`; construtor aceito seguido de `save` não tem sua procedência revalidada (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:93`).
- A fonte também é falsificável: `v = {'trabalha_em_feriado': True}` ou `evil.feriado_do_trecho(...)` passam fora da porta. Aceitação compara sintaxe/nome, sem provar que `v` veio de `julgar_vinculo` ou que a função pertence ao modelo (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:48`).
- O mutante literal pedido morde: retirar o keyword ou trocá-lo por `True` na âncora da criação produz achado; isso foi confirmado também em memória. O teste existente faz essas duas substituições, mas não muta o juiz para retornar True em 5x2 nem testa os escapes acima (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:137`).
- Portanto há mordida real e cobertura incompleta, não selo completamente vacuoso (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:143`, `:90`, `:102`).

H — Semântica e leitores

**Bloqueante:** a tela exige resposta para personalizado/intermitente, mas a resposta “Não” não torna o feriado folga nesses ciclos. Só 6x1/5x2/semanal integram `CICLOS_FERIADO_VIRA_FOLGA`; o predicado retorna False para os demais independentemente da flag. A precedência mantém trabalho previsto nesses feriados por `not ec.feriado_vira_folga(data)` (`app/templates/colaboradores/partials/painel_vinculo.html:175`, `app/escala/models.py:32`, `:1420`, `app/ponto/precedencia.py:376`).

- 6x1/semanal: a escolha humana tem efeito consistente sobre o previsto e sobre a partição de pagamento, porque os consumidores consultam `feriado_vira_folga`; não encontrei consumidor que leia o `default` do campo para decidir (`app/escala/models.py:1399`, `:1420`, `app/ponto/precedencia.py:377`, `app/ponto/services/fechamento.py:949`, `app/ponto/calculador/alimentacao.py:521`).
- 24x48 passa a nascer True, porém o predicado já ignora sua flag por não ser weekday; True/False não altera essa supressão do feriado. O código adiciona 24x48 à resposta obrigatória True do juiz de nascimento (`app/escala/models.py:906`, `:921`, `:1422`).
- `feriado_do_posto` não lê a flag do vínculo: usa `Posto.opera_em_feriado`; posto fechado veta cobrança inclusive de vínculo que trabalha em feriado. Há veto explícito adicional depois da consulta à precedência, portanto “flag True” não garante cobrança (`app/colaboradores/services/feriado_do_posto.py:67`, `app/ponto/management/commands/detectar_ausencias.py:617`, `:619`, `:629`).
- Contador antigo continua classificando 5x2/6x1 True em “declarados” conforme o posto, embora agora 6x1 True possa resultar de escolha humana independente do posto. É indicador com critério anterior preservado (`app/escala/models.py:950`, `:959`, `:962`).
- O contador novo existe como método/comando, mas o placar continua chamando somente o antigo; a integração do contador novo ao ESTADO/copiloto não foi encontrada na cópia. O ponto concreto de integração existente é `bin/placar_code.sh:298`; registro novo só declara a “casa” no placar (`app/config/crons.py:1227`). Publicação/exibição efetiva do novo contador: [não verificado].

Inventário direto de leituras da flag em produção:

| Uso | Referências |
|---|---|
| Foto para trilha | `app/colaboradores/services/vinculo.py:43` |
| Comparação no_op / herança | `app/colaboradores/services/vinculo.py:145`, `:187` |
| Idempotência / antes da mudança | `app/colaboradores/services/vinculo.py:932`, `:934` |
| Cópias para trechos | `app/colaboradores/services/vinculo.py:1021`, `:1139`, `:1161` |
| Contadores | `app/escala/models.py:945`, `:959` |
| Supressão do previsto no feriado | `app/escala/models.py:1420` |
| Lote: listagem / reversão | `app/escala/management/commands/aplicar_feriado_nasce_da_escala.py:44`, `:52` |
| Sincronização: comparação / listagem | `app/colaboradores/management/commands/sincronizar_feriado_admin.py:53`, `:57` |
| Entrada humana de UI | `app/templates/colaboradores/partials/painel_vinculo.html:176` |

Leituras indiretas relevantes:

| Consumidor | Referências |
|---|---|
| Geração/regeneração de célula | `app/ponto/portas/celula.py:289`, `:911` |
| Precedência | `app/ponto/precedencia.py:366`, `:377` |
| Fechamento | `app/ponto/services/fechamento.py:949` |
| Calculador diário | `app/ponto/calculador/alimentacao.py:521`, `app/ponto/calculador/regras.py:684` |
| Rubrica de exportação, sem leitura direta da flag | `app/folha/export.py:420`, `:422` |

I — Classes presentes/limites

- Campo com significado incompatível com a pergunta da UI para personalizado/intermitente: presente e demonstrável pelo fluxo acima (`app/templates/colaboradores/partials/painel_vinculo.html:175`, `app/escala/models.py:1422`).
- Selo com escapes: presente; o mutante literal morde, mas a alegação de impedir todo nascimento/escrita fora da porta excede a implementação (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:4`, `:90`, `:102`).
- O RED “col148” é fixture sintética, não replay do EC854: cria outro colaborador/vínculo e célula artificial; verifica flag, célula e tipo de dia, mas não uma cobrança/veredito previamente lavrado de produção nem 20/11 (`app/escala/tests/test_feriado_padrao_por_escala.py:188`, `:198`, `:230`). Resultado real col148, suite completa/opus, publicação de censo, lista dos 16, deploy e apply: [não verificado].
- Nenhuma ocorrência de `FERIADO-NASCE-DA-ESCALA`/contador novo foi encontrada em `app/docs`, `CLAUDE.md` ou `bin`; isso não prova inexistência de relato fora da cópia. Censo e listas publicados: [não verificado]. [nao verificado]
- Não executei os testes existentes porque os testes de selo escrevem arquivos temporários e os funcionais escrevem banco, contrariando a regra de somente leitura (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:115`, `app/escala/tests/test_feriado_padrao_por_escala.py:198`).

## 13/11 10:47 · sessao 85aeb4ade445 · - · - · origem=Ronald (tmux)

A: `qs_na_fila` aceita `_base` e entrega a interseção desse queryset com `VIVOS`, `COBRANCA` e ausência de silêncio futuro; `_base` substitui a montagem padrão, sem exigir as anotações do painel. O juiz da fila é `chamados/services/painel.py::qs_na_fila`. (`app/chamados/services/painel.py:153`; `app/chamados/services/painel.py:155`; `app/chamados/services/painel.py:158`; `app/chamados/services/painel.py:188`; `app/chamados/services/painel.py:189`; `app/chamados/services/painel.py:190`.)

O contador muda de “recusados ainda vivos” para “recusados ainda cobrados na fila”: exclui `registrado` e silenciados no futuro. `VIVOS` contém `registrado`, enquanto `COBRANCA` contém somente `aberto` e `em_analise`. A nova docstring declara esse universo. (`app/chamados/catalogo/motor.py:24`; `app/chamados/catalogo/motor.py:29`; `app/chamados/juizes.py:2512`; `app/chamados/juizes.py:2525`.)

O leitor executável encontrado é `_estado`, no selo O168, que publica o retorno como `segurados`; as expectativas continuam sendo zero/um nos cenários existentes. A outra ocorrência produtiva é documentação de `_registrar_recusa`, sem chamada ao contador. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:67`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:75`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:122`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:159`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:187`; `app/chamados/models.py:712`.)

F/pre-opus: o bloqueante da volta 4 foi curado: o fecho agora segue somente `ast.Call` com função nominal definida no módulo; a referência nua `_isca` deixou de contar. Rodei por extração AST, exclusivamente em memória, os três métodos adversariais: passaram. No fonte atual, obtive `raw=False`, `judge=True`, fecho composto por `perguntas_que_seguram_csf` e `pode_executar_csf`; a isca deu `judge=False`. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:288`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:290`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:357`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:360`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:369`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:389`; `app/ponto/supra_juiz.py:558`; `app/ponto/supra_juiz.py:559`; `app/ponto/supra_juiz.py:573`.)

Permanece o limite já descrito na volta 4: uma chamada ao juiz cujo resultado é descartado ainda satisfaz `_pergunta_ao_juiz`; o teste exige existência da chamada, sem vínculo com o resultado. Reproduzi `ch.sem_veredito_entre(); return True` dando `judge=True`. Isso não foi introduzido pela alteração de `ast.Name` para `ast.Call`. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:332`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:334`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:336`; dado fornecido, preverif4.r, item C.)

As nove entradas de `CORPO` correspondem às chamadas e linhas atuais; a mudança atualiza a identidade de linha, sem adicionar exceções. O scanner usa a tupla completa e `EXCECOES` continua vazio. (`app/chamados/tests/e4b_c12_ast.py:18`; `app/chamados/tests/e4b_c12_ast.py:29`; `app/chamados/tests/e4b_c12_ast.py:222`; `app/chamados/tests/e4b_c12_ast.py:223`; `app/chamados/models.py:199`; `app/chamados/models.py:296`; `app/chamados/models.py:450`; `app/chamados/models.py:655`; `app/chamados/models.py:835`; `app/chamados/models.py:853`; `app/chamados/models.py:1489`; `app/chamados/models.py:1666`; `app/chamados/models.py:1686`.)

A fixture do executor fornece os campos reais do juiz e `_D` delega ao corpo canônico, sem copiar seu predicado. Rodei seus seis testes em memória com as funções reais extraídas por AST e módulos sintéticos somente para resolver imports e o patch da tranca: passaram. Execução Django integral: [não verificado]. (`app/ponto/tests/test_supra_juiz_executor.py:24`; `app/ponto/tests/test_supra_juiz_executor.py:45`; `app/ponto/tests/test_supra_juiz_executor.py:58`; `app/ponto/tests/test_supra_juiz_executor.py:62`; `app/ponto/tests/test_supra_juiz_executor.py:79`; `app/chamados/models.py:1899`.)

A fixture PEP agora zera também `resposta_colab`, coerentemente com o juiz que considera texto sem carimbo uma resposta. O teste preserva controles da irmã carimbada e da pergunta em voo sem escrita. (`app/chamados/tests/test_pep_reentrada.py:280`; `app/chamados/tests/test_pep_reentrada.py:294`; `app/chamados/tests/test_pep_reentrada.py:296`; `app/chamados/models.py:1900`; `app/chamados/models.py:1901`.)

Não demonstrei bloqueante novo em A nem nessa parte de F: o contador passou a consultar o juiz existente e a fixture passou a delegar ao predicado existente. (`app/chamados/juizes.py:2525`; `app/ponto/tests/test_supra_juiz_executor.py:46`.)

## 13/11 10:47 · sessao 5f5a2ad0669a · - · - · origem=Ronald (tmux)

A/C/E — achados confirmados pela cópia:

- **Bloqueante: troca retroativa deixa flag incompatível.** `corrigir_vinculo_retroativo` troca `tipo_escala` e salva apenas tipo/âncora/fim, sem consultar juiz nem atualizar flag. Assim um vínculo 6x1 True corrigido para 5x2 continua True. O comando chama essa função na correção “no lugar”. (`app/colaboradores/services/vinculo.py:1024`, `app/colaboradores/services/vinculo.py:1027`, `app/colaboradores/services/vinculo.py:1030`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:212`)

- **Bloqueante: recusa do aceite não restaura a flag.** O snapshot guarda pk/tipo/início/fim/ativa, sem `trabalha_em_feriado`; a reversão restaura tipo sem flag. Caso concreto derivável: reaproveitar vínculo 5x2 False como 12x36 True em modo aceite; recusar restaura 5x2, mas deixa True. Troca somente da flag também não é desfeita pela recusa. (`app/colaboradores/services/vinculo.py:108`, `app/colaboradores/services/vinculo.py:315`, `app/colaboradores/services/vinculo.py:317`, `app/colaboradores/services/vinculo.py:1078`, `app/chamados/services/acoes_chamado.py:143`, `app/chamados/services/acoes_chamado.py:149`)

Inventário de nascimento encontrado na produção, inclusive procurando aliases de importação por AST em memória:

| Sítio | Origem da flag |
|---|---|
| `executar_vinculo`, criação | `v['trabalha_em_feriado']`, vindo de `_feriado_do_veredito`. (`app/colaboradores/services/vinculo.py:144`, `app/colaboradores/services/vinculo.py:329`, `app/colaboradores/services/vinculo.py:338`) |
| `executar_vinculo`, reaproveitamento | Recebe o mesmo valor do veredito. (`app/colaboradores/services/vinculo.py:315`, `app/colaboradores/services/vinculo.py:317`) |
| `criar_trecho_retroativo` | `feriado_do_trecho(tipo_destino, flag_original)`. (`app/colaboradores/services/vinculo.py:1017`, `app/colaboradores/services/vinculo.py:1021`) |
| `ajustar_vinculo_pelo_sistema` | `feriado_do_trecho(tipo_derivado, flag_original)`. (`app/colaboradores/services/vinculo.py:1136`, `app/colaboradores/services/vinculo.py:1139`) |
| `desfazer_ajuste_do_sistema`, renascimento | `feriado_do_trecho(tipo_antigo, flag_antiga)`. (`app/colaboradores/services/vinculo.py:1158`, `app/colaboradores/services/vinculo.py:1161`) |
| `desfazer_ajuste_do_sistema`, reabertura | Reativa o antigo e mantém a flag antiga sem juiz. (`app/colaboradores/services/vinculo.py:1149`, `app/colaboradores/services/vinculo.py:1154`) |
| `corrigir_vinculo_retroativo`, troca tipo | Mantém flag anterior sem juiz — falha acima. (`app/colaboradores/services/vinculo.py:1027`, `app/colaboradores/services/vinculo.py:1030`) |
| `reverter_para_snapshot`, troca tipo | Mantém flag atual sem restaurar flag anterior — falha acima. (`app/colaboradores/services/vinculo.py:1078`) |

Nos quatro `create` encontrados, nenhum usa diretamente o default. O default ainda existe como True, sem enforcement no modelo para impedir caminhos não capturados pelo selo. (`app/colaboradores/services/vinculo.py:329`, `app/colaboradores/services/vinculo.py:1017`, `app/colaboradores/services/vinculo.py:1136`, `app/colaboradores/services/vinculo.py:1158`, `app/escala/models.py:896`)

`feriado_do_trecho` trata qualquer booleano herdado como “escolha humana”, mas não consulta prova da escolha: para juiz None retorna `bool(escolha)`. Portanto legado que nasceu pelo default pode ser reproduzido como escolha. (`app/escala/models.py:928`, `app/escala/models.py:933`)

Chamadores de `julgar_vinculo`/`executar_vinculo`:

| Chamador | Passa flag? Efeito |
|---|---|
| Tela Vincular | Encaminha `request.POST`; template tem radios 1/0 sem pré-seleção. (`app/colaboradores/views.py:411`, `app/colaboradores/views.py:423`, `app/templates/colaboradores/partials/painel_vinculo.html:176`, `app/templates/colaboradores/partials/painel_vinculo.html:177`) |
| Chat, prévia | Não: `_post` só monta template/início/modo/âncora; erro vira lista `erros`. (`app/colaboradores/services/troca_pelo_chat.py:11`, `app/colaboradores/services/troca_pelo_chat.py:58`, `app/colaboradores/services/troca_pelo_chat.py:59`) |
| Chat, execução | Não; mesma montagem. Novo vínculo ou troca para ciclo sem padrão passa a ser recusado; mesmo ciclo pode herdar. Não há argumento nessa interface para fornecer escolha. (`app/colaboradores/services/troca_pelo_chat.py:64`, `app/colaboradores/services/troca_pelo_chat.py:72`, `app/colaboradores/services/troca_pelo_chat.py:75`, `app/colaboradores/services/vinculo.py:177`) |
| Régua/realocar | Não: dict sem flag. Recusa captura ValueError; mesma restrição em novos/novos ciclos sem padrão. (`app/escala/regua_defesa.py:313`, `app/escala/regua_defesa.py:316`, `app/escala/regua_defesa.py:317`, `app/escala/regua_defesa.py:321`) |
| Comando `censo_fase_12x36` | Não: dict tem template/início/âncora; 12x36 recebe True do juiz. (`app/ponto/management/commands/censo_fase_12x36.py:419`, `app/ponto/management/commands/censo_fase_12x36.py:422`, `app/ponto/management/commands/censo_fase_12x36.py:423`, `app/escala/models.py:921`) |

Chamador direto adicional por API/importação não encontrado nas buscas estáticas; caminhos dinâmicos permanecem [não verificado]. O Django admin de vínculo é somente leitura. (`app/escala/admin.py:18`)

E:

- Mesmo valor em memória retorna antes de save/regeneração/log. Não há releitura/trava do valor persistido para validar idempotência com instância stale ou concorrência. (`app/colaboradores/services/vinculo.py:932`, `app/colaboradores/services/vinculo.py:935`)
- `autor` autenticado ganha `_ReqDoAutor.user`, chega em `evento(... ator=request.user)` e vira `LogAuditoria.usuario`. (`app/colaboradores/services/vinculo.py:19`, `app/colaboradores/services/vinculo.py:939`, `app/core/auditoria.py:24`, `app/core/observ.py:118`, `app/core/observ.py:135`)
- `autor=None`: dentro de request, ContextVar fornece usuário; fora de request, usuário fica None e `ator` identifica o processo/comando. (`app/core/observ.py:108`, `app/core/observ.py:113`, `app/core/observ.py:117`, `app/core/observ.py:123`)
- `sincronizar_feriado_admin` chama sem autor e sem motivo; não tem log próprio nessa aplicação, portanto não encontrei duplicação explícita. Ele continua podendo alterar 6x1 pelo cargo, sem resposta humana DP: o juiz None leva ao booleano do cargo. (`app/colaboradores/management/commands/sincronizar_feriado_admin.py:51`, `app/colaboradores/management/commands/sincronizar_feriado_admin.py:61`, `app/escala/models.py:934`)
- Auditoria é best-effort: falha pode ser engolida e a flag ainda confirmar. `@atomic` não garante que a trilha exista se seu escritor engoliu a falha. (`app/colaboradores/services/vinculo.py:910`, `app/core/auditoria.py:17`, `app/core/auditoria.py:25`, `app/core/observ.py:105`, `app/core/observ.py:133`, `app/core/observ.py:139`)
- Relógio oculto: `ec.save` dispara `_nucleo_post_save_escala` e `_recompute_escala`, que lê outro `timezone.localdate()` antes do `hoje` da porta; portanto não há efetivamente um único relógio no ato completo. (`app/colaboradores/services/vinculo.py:936`, `app/colaboradores/services/vinculo.py:944`, `app/ponto/signals.py:63`, `app/ponto/signals.py:72`)

## 13/11 10:47 · sessao 3e37ce6f8cdc · - · - · origem=Ronald (tmux)

D/F — achados e limites:

- **Troca de tipo fora do juiz:** `corrigir_vinculo_retroativo` grava `tipo_escala`, âncora e fim, preservando a flag anterior sem consultar regra ou escolha. Pode produzir 5x2 True ou 6x1 herdando valor de outro ciclo. `reverter_para_snapshot` também restaura tipo sem restaurar/rejulgar flag. (`app/colaboradores/services/vinculo.py:1024-1030`, `:1078-1079`.)
- **Herança sem prova de escolha humana:** `feriado_do_trecho` usa a regra determinística; quando o juiz devolve None, executa `bool(escolha)`, sem conferir ciclo de origem nem proveniência humana. Logo trecho retroativo novo 12x36 True → 6x1 nasce True sem resposta DP. O caminho principal recusaria essa mesma troca sem escolha. (`app/escala/models.py:928-934`; `app/colaboradores/services/vinculo.py:1017-1021`, `:178-190`.)

**D — passado/exportadas**

- `criar_trecho_retroativo` realmente alimenta regeneração do passado: comando cria trecho nas linhas 194–197 e chama regeneração de `piso` até `min(fim, hoje)` nas linhas 223–239; chama também suplemento futuro nas linhas 242–245. (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:194`, `:223`, `:242`.)
- As células exportadas/publicadas ficam protegidas por padrão: `_competencias_exportadas` pergunta ao juiz `competencia_fechada_para_celula`; este considera TXT entregue e holerite do colaborador com lote publicado; regeneração pula os dias dessas janelas e preserva origem humana. (`app/ponto/management/commands/gerar_celulas.py:160-188`; `app/folha/export.py:617-628`; `app/ponto/portas/celula.py:218-221`, `:284-297`.)
- Há exceção preexistente, explícita: comando aceita `--apesar-da-lavra`, passa-a à regeneração; valor não vazio esvazia `_exp`, permitindo reescrever exportada. Não confundir essa possibilidade com comportamento padrão ou execução comprovada. (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:42-44`, `:231-234`; `app/ponto/portas/celula.py:218-221`.)
- Ajuste automático só futuro: aplica exige `inicio > hoje`, regenera desde início; desfazer calcula `volta = max(inicio, hoje+1)` e regenera desde volta. Portanto esses chamadores não regeneram passado. (`app/escala/services/escala_auto_executor.py:98-108`, `:143-148`.)
- Criar/desfazer usam `feriado_do_trecho`; desfazer antes de valer apaga novo e reativa antigo, sem passar novamente pelo juiz. (`app/colaboradores/services/vinculo.py:1136-1140`, `:1149-1162`.)

**F — lote**

- Universo é **vínculo que cobre hoje**, colaborador ativo, 5x2 True, independentemente de `ativa=True`: inativo com fim cobrindo hoje pode entrar; ativo futuro fica fora. Isso não equivale automaticamente aos “16 ativos” da sonda. Lista real coincidente com os 16: **[não verificado]**. (`app/escala/models.py:937-947`; `app/escala/alimentacao.py:219-225`.)
- `--esperado` é comparado exatamente ao universo; diferença para antes de escrita. Imprime ec/col/tipo/antes; isso não prova publicação anterior no RELATO. Publicação dos 16 por colaborador e censo pré-código: **[não verificado]**. (`app/escala/management/commands/aplicar_feriado_nasce_da_escala.py:35-47`.)
- Reversão JSON completa é escrita e arquivo fechado antes da primeira mutação; modo `x` impede sobrescrever arquivo existente. Não há `fsync` nem diário de progresso/reversão executável. (`app/escala/management/commands/aplicar_feriado_nasce_da_escala.py:51-55`.)
- Há transação **por vínculo**, não por lote: `marcar` é `@transaction.atomic`; loop do comando não tem transação externa. Queda após N commits deixa N aplicados, restantes intactos e reversão contendo todos. Repetir lista original falha na comparação; reutilizar JSON falha no `open('x')`. Retomada exige conferir restante e nova lista/arquivo; considero limitação operacional, não corrupção inerente. (`app/colaboradores/services/vinculo.py:910-911`; `app/escala/management/commands/aplicar_feriado_nasce_da_escala.py:35-40`, `:51-58`.)
- A atomicidade não inclui sucesso de todos os efeitos: julgamento captura falhas; dinheiro é `on_commit` e engole exceção; E2 é outro `on_commit`, protegido por `engolir`. Portanto flag False/contador zero pode coexistir com rejulgamento, dinheiro ou encerramento ainda pendentes. Repetir `marcar` com flag já False retorna imediatamente e não repara efeitos que falharam. (`app/ponto/portas/celula.py:460-482`, `:528-540`; `app/ponto/services/fechamento.py:1798-1827`; `app/core/observ.py:85-91`; `app/colaboradores/services/vinculo.py:932-933`.)
- Lote mantém guarda de exportada: não passa `apesar_da_lavra`; usa padrão vazio da porta tanto no passado quanto no suplemento futuro. Suplemento mantém `recalculo=True`, pois `marcar` não sobrescreve esse argumento. (`app/colaboradores/services/vinculo.py:951-960`; `app/ponto/portas/celula.py:112-114`, `:218-221`, `:284-288`.)
- Futuro alcança **somente células geradas já existentes**, dentro de janela corrente; não cria 20/11 ausente. Filtra dono pelo juiz quando `substitui_dono=False`. Todos os 16 com célula20/11 False: **[não verificado]**. (`app/ponto/services/cadastro_futuro.py:26-45`; `app/ponto/management/commands/gerar_celulas.py:290-300`.)
- **Relógio fora da borda presente:** contador escolhe hoje internamente; comando o chama antes e depois; cada `marcar` lê outro hoje após salvar; descendente `_o_chao_do_dia_mudou` relê hoje para decidir passado/futuro. Lote atravessando meia-noite pode comparar universos diferentes e operar com datas diferentes por vínculo. (`app/escala/models.py:943`; `app/escala/management/commands/aplicar_feriado_nasce_da_escala.py:36`, `:57`; `app/colaboradores/services/vinculo.py:944`; `app/ponto/portas/celula.py:464-469`.)
- Não identifiquei inversão nova de trava nesses caminhos: `marcar.save()` adquire escrita no vínculo antes do suplemento travar células; suplemento usa `select_for_update` nas células. Ausência global de inversão fora do trecho auditado: **[não verificado]**. (`app/colaboradores/services/vinculo.py:935-936`, `:957`; `app/ponto/services/cadastro_futuro.py:30-32`.)

O teste RED usa fixture sintética e verifica `fatos_do_dia`, flag, célula e log; não demonstra caso EC854 real, 16 aplicados, fechamento real ou20/11. Execução GREEN da suíte: **[não verificado]**. (`app/escala/tests/test_feriado_padrao_por_escala.py:230-247`.)

## 13/11 10:47 · sessao 864c700c89c1 · - · - · origem=Ronald (tmux)

C. Para erros com um único `Pergunta #N`, sem coincidência de prefixo numérico, o separador preserva conteúdo e ordem das listas; os consumidores recebem os mesmos textos. O helper central apenas escolhe a lista e acrescenta o item original. (`app/chamados/services/validacao.py:120`; `app/chamados/services/validacao.py:124`.)

Os leitores identificados são:

- **Recusa na tela:** traduz `r['erros'][0]`, sem analisar proprietário. (`app/chamados/services/acoes_disputa.py:32`; `app/chamados/services/acoes_disputa.py:34`.)
- **Tradutor:** classifica o motivo por marcas textuais; não conhece pk nem separa perguntas. (`app/chamados/catalogo/recusa.py:77`; `app/chamados/catalogo/recusa.py:81`; `app/chamados/catalogo/recusa.py:88`.)
- **Log da recusa:** usa `erros_da_pergunta`, portanto passa pelo mesmo separador novo. (`app/chamados/services/validacao.py:515`; `app/chamados/services/validacao.py:519`; `app/chamados/services/validacao.py:142`.)
- **Log das irmãs:** concatena a lista recebida, sem reclassificar. (`app/chamados/services/validacao.py:527`; `app/chamados/services/validacao.py:532`.)
- **Aviso das irmãs na tela:** chama `aviso_das_irmas(r.get('irmas'))`; o resultado acompanha a afirmação de ponto gravado. (`app/chamados/services/acoes_disputa.py:58`; `app/chamados/services/acoes_disputa.py:61`.)
- **Envelopes:** fio entrega a mensagem ao `resposta_render`; cartão entrega a mensagem no `HX-Trigger`; ambos preservam a mensagem montada. (`app/chamados/services/fio.py:494`; `app/chamados/services/fio.py:498`; `app/core/respostas.py:85`; `app/chamados/views.py:404`; `app/chamados/views.py:407`.)

Há mudança além dos prefixos numéricos: `Pergunta #5: ... Pergunta #7` antes era próprio da #7 por substring; agora pertence à #5 e vira irmã para #7. O teste novo assume explicitamente essa mudança. Em uma única chamada antiga, `in`/`not in` já eram complementares; a duplicação ocorria entre consultas separadas para #5 e #7, não nas duas listas da mesma chamada. (`app/chamados/tests/test_bug29_30_log_validacao.py:125`; `app/chamados/tests/test_bug29_30_log_validacao.py:127`; `app/chamados/tests/test_bug29_30_log_validacao.py:129`; antes: diff fornecido, sem arquivo:linha [não verificado].)

**Ponto residual:** `aviso_das_irmas` continua extraindo **todas as menções**, não somente o dono. Para aquele erro recebido como irmã da #7, produz ids `5,7` e pode dizer “1 pergunta ... segue sem gravar (#5, #7)”, incluindo a própria #7. Reproduzi a extração em memória; existência desse texto exato em produção: [não verificado]. O aviso permanece um coletor de menções, embora o separador passe a trabalhar com proprietário único. (`app/chamados/services/validacao.py:547`; `app/chamados/services/validacao.py:549`; `app/chamados/tests/test_bug29_30_log_validacao.py:127`.)

E. Busca global por `Pergunta #`, `marca in str` e `marca not in str` não localizou outro separador de produção por substring na cópia; ausência de formas construídas dinamicamente: [não verificado]. Os dois consumidores migrados delegam ao helper único; o regex de `aviso_das_irmas` coleta IDs para apresentação, sem particionar erros. (`app/chamados/services/validacao.py:142`; `app/chamados/services/validacao.py:510`; `app/chamados/services/validacao.py:547`.)

## 13/11 10:47 · sessao ed9334c48e11 · - · - · origem=Ronald (tmux)

O fluxo de produção é: `validar_pergunta` recebe `erros_proprios` de `_materializar_uma` e soma `res['erros']`; a função pública importada de `models.py` é apenas reexportação de `materializacao.py`. `erros_da_pergunta` só é chamado nesse mesmo fluxo, nos retornos/log da recusa. (`app/chamados/services/validacao.py:484`; `app/chamados/services/validacao.py:492`; `app/chamados/services/validacao.py:493`; `app/chamados/services/validacao.py:500`; `app/chamados/services/validacao.py:519`; `app/chamados/services/validacao.py:524`; `app/chamados/models.py:4143`.)

Todos os escritores que alimentam essa lista:

| Escritor/caso | Evidência | Dono antigo → novo |
|---|---|---|
| `_ts_tipo_do_plantio`: sem data | `app/chamados/services/materializacao.py:352` | `p.pk` e eventuais prefixos numéricos/citações → só `p.pk` |
| Escape não vira batida | `app/chamados/services/materializacao.py:361` | Mesma regra |
| Default do relógio | `app/chamados/services/materializacao.py:377` | Mesma regra |
| Hora inválida, incluindo texto de exceção | `app/chamados/services/materializacao.py:421` | Mesma regra |
| `_declarar_ausencia_de_pergunta`: sem data | `app/chamados/services/materializacao.py:484` | Mesma regra |
| `_materializar_resposta_hora`: sem resposta | `app/chamados/services/materializacao.py:843` | Mesma regra |
| Vizinha do mesmo tipo/paridade | `app/chamados/services/materializacao.py:884` | Mesma regra |
| Conflito retornado por `registrar_batida`, embrulhado | `app/chamados/services/materializacao.py:942` | Mesma regra |
| `_materializar_resposta_data_hora`: parse inválido, incluindo exceção | `app/chamados/services/materializacao.py:1010` | Mesma regra |
| Conflito da âncora, embrulhado | `app/chamados/services/materializacao.py:1027` | Mesma regra |
| Gate `DATA_EM_DISPUTA Pergunta #p.pk` | `app/chamados/services/materializacao.py:1201`; `app/chamados/services/materializacao.py:1205` | Mesma regra; prefixo é reconhecido |
| `_Reprovada`: não gerou batida | `app/chamados/services/materializacao.py:1245`; `app/chamados/services/materializacao.py:1290` | Mesma regra |
| `_Reprovada`: C6 ainda considera viva | `app/chamados/services/materializacao.py:1285`; `app/chamados/services/materializacao.py:1290` | Mesma regra |
| Qualquer outra exceção dentro do `try`, embrulhada como `Pergunta #p.pk: {e}` | `app/chamados/services/materializacao.py:1291`; `app/chamados/services/materializacao.py:1292` | Mesma regra |

Todos esses erros têm o `Pergunta #p.pk` do dono como primeiro token desse formato; textos dinâmicos, conflitos e exceções entram depois dele. Os ramos recebem a mesma lista, e o coletor retorna essa lista diretamente. (`app/chamados/services/materializacao.py:1227`; `app/chamados/services/materializacao.py:1232`; `app/chamados/services/materializacao.py:1372`; `app/chamados/services/materializacao.py:1382`; `app/chamados/services/materializacao.py:1392`.)

O caso `__REUSO__Pergunta #p.pk` é escritor temporário de `erros`, porém o `finally` o remove de `erros` e o transfere para `reusos` sem o marcador; portanto ele não chega normalmente à separação de erros. Caso fosse entregue ao helper, tanto a substring antiga quanto a regex nova reconhecem `p.pk`; só a antiga também reconheceria prefixos/citações. (`app/chamados/services/materializacao.py:872`; `app/chamados/services/materializacao.py:1296`; `app/chamados/services/materializacao.py:1301`; `app/chamados/services/validacao.py:106`.)

Os textos sem `Pergunta` das recusas de competência/colisão vêm de `registrar_batida`, mas entram em `erros` envolvidos pelo prefixo do dono. `DecisaoPergunta #N` pode ser texto de exceção de outra entidade, mas o `except Exception` também o envolveria por `Pergunta #p.pk:`; uma instância real dessa exceção nesse caminho: **[não verificado]**. (`app/ponto/registro_batida.py:135`; `app/ponto/registro_batida.py:137`; `app/ponto/registro_batida.py:146`; `app/chamados/services/materializacao.py:943`; `app/chamados/services/materializacao.py:1028`; `app/chamados/services/materializacao.py:1292`; `app/chamados/models.py:3961`; `app/chamados/models.py:3965`.)

Nenhum escritor identificado perde reconhecimento pelo lookbehind. Ele só veta letra ASCII imediatamente antes de `Pergunta`; `_`, espaço e dígito são aceitos. Executei os helpers extraídos por AST em memória, sem Django, confirmando `__REUSO__Pergunta #9→9`, `DATA_EM_DISPUTA Pergunta #8→8`, `_Pergunta #7→7`, `3Pergunta #6→6`, `DecisaoPergunta #5→None`, `Pergunta #12: DecisaoPergunta #123→12`, erro de `#5` citando `#7→5` e texto sem pk→`None`. (`app/chamados/services/validacao.py:106`; `app/chamados/services/validacao.py:116`; `app/chamados/services/validacao.py:117`.)

Uma mensagem sem pk, se fornecida externamente, já não era própria de nenhuma pergunta e continua sem dono (`None`), ficando em `irmas`; a mudança real é excluir prefixos numéricos, citações posteriores e `DecisaoPergunta` isolado. Escritor de produção que entregue erro sem dono diretamente nesse fluxo não foi encontrado; ausência fora do fluxo estático auditado: **[não verificado]**. (`app/chamados/services/validacao.py:106`; `app/chamados/services/validacao.py:120`; `app/chamados/services/validacao.py:124`.)

## 13/11 09:34 · sessao 25a3a732a702 · O168 · censo · origem=Code

A triagem aponta **3 ERROR curadas pelo delta, 5 deslocamentos, 1 teste a atualizar por mudança deliberada, 3 falhas da entrega da obra e 1 defeito preexistente com causa exata da ocorrência ainda [nao verificado]**. As referências e ressalvas por falha seguem abaixo. [nao verificado]

| Falha | Veredito | Causa e delta |
|---|---|---|
| `test_resposta_direta_barra` | **CURADA-PELO-DELTA** | O novo executor usa o juiz, que exige campos ausentes em `_P`. O delta acrescenta campos, PK e isolamento da tranca. `app/ponto/tests/test_supra_juiz_executor.py:14`; `app/ponto/supra_juiz.py:559`; `app/chamados/models.py:1899`. |
| `test_resposta_na_disputa_barra` | **CURADA-PELO-DELTA** | O novo executor chama `respostas_sem_veredito()`, ausente em `_D`. O delta acrescenta a delegação ao juiz real. `app/ponto/tests/test_supra_juiz_executor.py:27`; `app/ponto/supra_juiz.py:558`. |
| `test_sem_conversa_executa` | **CURADA-PELO-DELTA** | Mesmo método ausente em `_D`, antes de julgar a pergunta sem resposta. `app/ponto/tests/test_supra_juiz_executor.py:39`; `app/ponto/supra_juiz.py:558`. |
| `test_MORDE_P_em_voo_COM_destino_nao_vivo_o_laco_sem_credor_pula` | **DO-DIFF-TESTE-A-ATUALIZAR** | O fixture conserva texto sem carimbo; a obra passou deliberadamente de carimbo **E** texto para carimbo **OU** texto. O HX reabre o pai e impede o laço sem credor. `app/chamados/tests/test_pep_reentrada.py:276`; `app/chamados/juizes.py:2035`; `app/chamados/services/disputa_emissao.py:2009`. Delta não cura. |
| `test_03_a_admin_le_gravado_e_o_aviso_das_irmas` | **PRE-EXISTENTE**, com ressalva abaixo | A porta e a apresentação envolvidas não mudaram nos hunks fornecidos. Há matching por substring suscetível ao erro fictício `#999999`; colisão efetiva nesta execução: **[nao verificado]**. `app/chamados/services/validacao.py:487`; `app/chamados/tests/test_validar_diz_o_seu.py:19`. Delta não cura. |
| `test_toda_contagem_por_status_de_pendencia_le_o_juiz_ou_esta_declarada` | **DO-DIFF-CODIGO-ERRADO** | Função nova conta diretamente por `VIVOS`, precisamente o caminho que o selo exige passar pelo juiz ou declarar. `app/chamados/juizes.py:2523`; `app/core/tests/test_tripwire_contagem_de_pendencia.py:370`. Delta não cura. |
| `test_MORDE_o_mmd_commitado_bate_com_o_codigo` | **DO-DIFF-CODIGO-ERRADO** | Artefatos gerados ficaram inconsistentes com o registro de juízes. `app/core/juizes.py:294`; `app/docs/ARQUITETURA.mmd:340`; `app/docs/MAPA.md:88`. Delta não cura. |
| `test_MORDE_uma_aresta_a_mais_no_mmd_derruba_o_selo` | **DO-DIFF-CODIGO-ERRADO** | Após restaurar o original, o teste reencontra a mesma divergência preexistente naquela entrega da obra. `app/core/tests/test_selo_diagrama_do_codigo.py:95`; `app/core/tests/test_selo_diagrama_do_codigo.py:98`. Delta não cura. |
| Selo: `ChamadoColaborador.analisar` | **DESLOCAMENTO** | Tabela: **836**; escritor: **853**. `app/chamados/tests/e4b_c12_ast.py:24`; `app/chamados/models.py:853`. Delta não cura. |
| Selo: `ChamadoColaborador.cancelar` | **DESLOCAMENTO** | Tabela: **818**; escritor: **835**. `app/chamados/tests/e4b_c12_ast.py:23`; `app/chamados/models.py:835`. Delta não cura. |
| Selo: `ChamadoColaborador.encerrar` | **DESLOCAMENTO** | Tabela: **1649**; escritor: **1666**. `app/chamados/tests/e4b_c12_ast.py:26`; `app/chamados/models.py:1666`. Delta não cura. |
| Selo: `ChamadoColaborador.encerrar_por_competencia_trancada` | **DESLOCAMENTO** | Tabela: **1669**; escritor: **1686**. `app/chamados/tests/e4b_c12_ast.py:27`; `app/chamados/models.py:1686`. Delta não cura. |
| Selo: `ChamadoColaborador.reabrir` | **DESLOCAMENTO** | Tabela: **1472**; escritor: **1489**. `app/chamados/tests/e4b_c12_ast.py:25`; `app/chamados/models.py:1489`. Delta não cura. |

**As três ERROR do executor.** O hunk responsável é `ponto/supra_juiz.py @@ -536,15 +549,25 @@`: antes bastavam `perguntas.all()` e `respondida_em`; agora o executor consulta `respostas_sem_veredito()` e `sem_veredito_entre()`. Os doubles conservaram a interface antiga. A pergunta mudou deliberadamente de “há carimbo de resposta?” para “há resposta **sem veredito**, não dispensada pela tranca?”. Resposta validada ou materializada libera; texto sem carimbo também pode segurar. `app/ponto/supra_juiz.py:552`; `app/ponto/supra_juiz.py:563`; `app/chamados/models.py:1898`; `app/docs/CORTES.json:1414`.

Os três asserts continuam corretos nos seus casos sem veredito; quem precisava atualização eram os doubles. O delta fornece os campos e a delegação exigidos por esse caminho. A verificação das funções reais extraídas por AST, com os doubles do delta, confirmou os seis casos em memória; execução Django e passagem da suíte completa após o delta: **[nao verificado]**. `app/ponto/tests/test_supra_juiz_executor.py:39`; `app/chamados/models.py:1899`; `app/ponto/supra_juiz.py:558`.

**A irmã fora de voo.** O hunk causal é `chamados/juizes.py @@ -2019,19 +2019,20 @@`, não a guarda nova de fechamento do pai. O fixture deixa ambas as perguntas com `resposta_colab='07:00'`, embora zere `respondida_em` e `validada_em`; seu comentário “sem resposta por validar” deixou de corresponder ao novo predicado. `app/chamados/tests/test_pep_reentrada.py:277`; `app/chamados/tests/test_pep_reentrada.py:278`; `app/chamados/models.py:1900`.

Com destino na P, ela entra no universo inicial, é pulada por estar em voo, mas sua disputa participa do HX. O HX consulta o juiz alterado, encontra texto sem veredito e reabre o pai. Depois, o laço sem credor pula a disputa porque o pai está vivo — **antes de chegar à irmã**. Portanto, estar fora de voo não basta para ela receber carimbo. `app/chamados/services/disputa_emissao.py:1906`; `app/chamados/services/disputa_emissao.py:1960`; `app/chamados/services/disputa_emissao.py:2003`; `app/chamados/services/disputa_emissao.py:2011`; `app/chamados/services/disputa_emissao.py:2033`.

Na base fornecida, o predicado exigia `respondida_em` não nulo e texto; essas perguntas ficavam fora dele. A obra passou propositalmente a proteger também texto sem carimbo, e há teste explícito dessa mudança. Para continuar perguntando apenas pela guarda de voo, esse fixture precisa representar silêncio também no texto; a retenção de texto sem carimbo merece permanecer coberta separadamente. `app/chamados/juizes.py:2022`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:252`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:263`.

**`test_validar_diz_o_seu`, test_03.** A pergunta do teste permanece: “a gravação da própria pergunta aparece como sucesso, enquanto o erro da irmã vira aviso separado?”. A obra não declara mudança dessa pergunta. `app/chamados/tests/test_validar_diz_o_seu.py:3`; `app/chamados/tests/test_validar_diz_o_seu.py:87`.

A mensagem observada **não prova competência realmente aprovada/exportada**: o teste injeta exatamente um erro dessa natureza, e a casca traduz o primeiro erro retornado. `app/chamados/tests/test_validar_diz_o_seu.py:19`; `app/chamados/tests/test_validar_diz_o_seu.py:47`; `app/chamados/tests/test_validar_diz_o_seu.py:85`; `app/chamados/services/acoes_disputa.py:34`.

Existe um defeito independente do diff: `marca in str(e)` considera `Pergunta #9` parte de `Pergunta #999999`. Assim, PKs **9, 99, 999, 9999 e 99999** classificariam a irmã fictícia como erro próprio, podendo produzir `nao_gravou` mesmo com materialização concluída. Outro ramo produz esse estado quando há erros e `materializada_em` continua nulo. O dado não informa qual ramo ocorreu. `app/chamados/services/validacao.py:487`; `app/chamados/services/validacao.py:490`; `app/chamados/services/validacao.py:504`.

A porta real lê **timestamp → competência do fato → `FechamentoMensal.status`**; não consulta “hoje” nessa decisão. O fixture mistura data fixa de junho com `timezone.now()`, mas atribuir a ocorrência ao relógio permanece **[nao verificado]**. O gate novo também libera imediatamente a categoria desse fixture. `app/ponto/janelas.py:84`; `app/ponto/janelas.py:87`; `app/chamados/tests/test_validar_diz_o_seu.py:33`; `app/chamados/tests/test_validar_diz_o_seu.py:38`; `app/chamados/models.py:716`.

Logo, **PRE-EXISTENTE** identifica o defeito e a cadeia não alterados; provar a mesma ocorrência na base, com o mesmo PK e estado de materialização, permanece **[nao verificado]**.

**O contador e o universo.** O hunk novo `chamados/juizes.py @@ -2505,3 +2506,18 @@` introduziu a contagem crua. Para preservar **os candidatos da trilha**, a adaptação é passar essa seleção como `_base`, sem recalcular conversa nem acrescentar outra seleção de pais. `app/chamados/juizes.py:2520`; `app/chamados/services/painel.py:153`; `app/chamados/services/painel.py:188`.

```python
return qs_na_fila(
    _base=ChamadoColaborador.objects.filter(pk__in=_ids)
).count()
```

Isso preserva os candidatos, **mas não garante o mesmo total de todos os VIVOS**: `VIVOS` inclui `registrado`; o juiz da fila exclui registrados e silenciados no futuro. Preservar literalmente esse total e usar somente `qs_na_fila` são requisitos incompatíveis no código atual. A inexistência desses estados entre os pais de produção é **[nao verificado]**. `app/chamados/catalogo/motor.py:24`; `app/chamados/services/painel.py:189`.

**Os cinco escritores do selo.** A obra inseriu **46 linhas** antes deles: três na decisão e 43 no novo método. O hunk da tabela atualizou apenas **29**, deixando todas as entradas 17 linhas atrás. O selo compara tuplas que incluem a linha; não surgiu escritor novo de estado nesses hunks. Ajustar somente essas cinco posições em memória fez o censo deixar de apontá-las. `app/chamados/models.py:686`; `app/chamados/models.py:699`; `app/chamados/models.py:742`; `app/chamados/tests/e4b_c12_ast.py:23`; `app/chamados/tests/e4b_c12_ast.py:222`.

**O diagrama exige os dois artefatos.** O registro ganhou duas perguntas, chegando a **23** na família chamado, mas os documentos ficaram em **22**. O MAPA também conserva a formulação anterior sobre conversa e omite a pergunta de respostas sem veredito. `app/core/juizes.py:294`; `app/core/juizes.py:295`; `app/docs/ARQUITETURA.mmd:340`; `app/docs/MAPA.md:88`; `app/docs/MAPA.md:180`.

Portanto, não basta regerar somente `ARQUITETURA.mmd`: o gerador compara e escreve **`ARQUITETURA.mmd` e `MAPA.md`**. A segunda falha resulta de restaurar um original já divergente, não demonstra defeito na restauração. `app/core/management/commands/gerar_diagrama.py:923`; `app/core/management/commands/gerar_diagrama.py:932`; `app/core/tests/test_selo_diagrama_do_codigo.py:95`.

## 13/11 09:33 · sessao 96e9d46f0903 · - · - · origem=Ronald (tmux)

Correção: **a falha não implica `materializada_em=None`**. A condição também entra por `proprios` (`app/chamados/services/validacao.py:490`).

O matching usa substring: `marca = 'Pergunta #%s' % perg.pk` e `marca in str(e)` (`app/chamados/services/validacao.py:487`). Assim, PK **9, 99, 999, 9999, 99999 ou 999999** casa com a irmã fictícia `#999999` (`app/chamados/tests/test_validar_diz_o_seu.py:19`), classificando-a como erro próprio mesmo após gravação correta (`app/chamados/services/validacao.py:488`, `app/chamados/services/validacao.py:504`).

**PRE-EXISTENTE**: defeito de matching em código fora dos hunks da obra; o teste apresenta fixture suscetível (`app/chamados/services/validacao.py:487`; `app/chamados/tests/test_validar_diz_o_seu.py:19`). **Colisão efetiva nesta execução: [nao verificado]**, porque o dado não informa o PK da pergunta. **Relógio como causa: [nao verificado]**. A mensagem de competência pode resultar apenas da tradução do erro injetado (`app/chamados/services/acoes_disputa.py:34`; `app/chamados/catalogo/recusa.py:35`).

## 13/11 09:33 · sessao a5c32dfc0772 · - · - · origem=Ronald (tmux)

As cinco subfalhas de `test_RED_um_subTest_por_sitio` são **DESLOCAMENTO**: o selo inclui a linha na identidade (`app/chamados/tests/e4b_c12_ast.py:17`, `:222`), e as entradas ficaram 17 linhas atrás dos escritores reais (`:23`–`:27`).

| Falha | Tabela atual | Escritor real | Veredito |
|---|---:|---|---|
| `analisar` | 836 (`app/chamados/tests/e4b_c12_ast.py:24`) | `app/chamados/models.py:853` | DESLOCAMENTO |
| `cancelar` | 818 (`app/chamados/tests/e4b_c12_ast.py:23`) | `app/chamados/models.py:835` | DESLOCAMENTO |
| `encerrar` | 1649 (`app/chamados/tests/e4b_c12_ast.py:26`) | `app/chamados/models.py:1666` | DESLOCAMENTO |
| `encerrar_por_competencia_trancada` | 1669 (`app/chamados/tests/e4b_c12_ast.py:27`) | `app/chamados/models.py:1686` | DESLOCAMENTO |
| `reabrir` | 1472 (`app/chamados/tests/e4b_c12_ast.py:25`) | `app/chamados/models.py:1489` | DESLOCAMENTO |

A obra insere 3 linhas em `_decisao_do_fechamento` (`app/chamados/models.py:686`) e 43 linhas para `_gate_conversa_do_pai` (`app/chamados/models.py:699` até antes de `:742`): deslocamento total de +46. O hunk fornecido atualizou a tabela somente +29; faltaram +17. Os cinco escritores não foram acrescentados nesses hunks; permanecem nos métodos citados. A guarda nova lê a lista e registra recusa/nota (`app/chamados/models.py:718`, `:723`, `:735`).

Executei somente o censo AST em memória: ele devolveu precisamente os cinco sítios; ajustando somente as cinco linhas de `CORPO` em memória por +17, devolveu `[]`. Isso usa o comparador que descarta o corpo nominal por tupla exata (`app/chamados/tests/e4b_c12_ast.py:222`). Nenhuma mudança em arquivo foi necessária para essa verificação; reexecução da suíte inteira [nao verificado].

As duas falhas do diagrama decorrem dos artefatos desatualizados pela obra: **DO-DIFF-CODIGO-ERRADO**, entendendo “código” aqui como entrega gerada inconsistente. O teste principal exige igualdade dos artefatos com o gerador (`app/core/tests/test_selo_diagrama_do_codigo.py:44`); o segundo restaura o original e então exige a mesma igualdade (`:95`, `:98`). Portanto, a segunda falha não prova falha na restauração: restaurar um original já divergente mantém `rc=1` (`app/core/management/commands/gerar_diagrama.py:923`).

Prova da divergência:

- `core/juizes.py` ganhou **duas** perguntas em `app/core/juizes.py:294` e `:295`; o bloco conta `len(J.JUIZES[fam])` (`app/core/management/commands/gerar_diagrama.py:346`). A geração em memória produziu **23**, mas `app/docs/ARQUITETURA.mmd:340` mantém **22**. O número “+10 arquivos” já confere: deduplica arquivos por conjunto (`app/core/management/commands/gerar_diagrama.py:348`); as duas perguntas não significam dois arquivos novos.
- `app/docs/MAPA.md:88` mantém **22/0**, mas a geração em memória produz **23/0** pelos critérios de `app/core/management/commands/gerar_diagrama.py:774`, `:829`.
- `app/docs/MAPA.md:180` conserva a pergunta anterior sobre “conversa”, enquanto a fonte agora pergunta por resposta **SEM veredito** (`app/core/juizes.py:295`); falta também a pergunta de `:294`. A lista de perguntas do MAPA vem diretamente do registro (`app/core/management/commands/gerar_diagrama.py:781`, `:850`).

**Não basta regerar somente `ARQUITETURA.mmd`**: `gerar(check=True)` compara os dois arquivos (`app/core/management/commands/gerar_diagrama.py:923`), e a geração normal grava ambos (`:932`). O delta fornecido não toca os selos, o registro ou esses artefatos; portanto não contém a cura dessas sete falhas [nao verificado: anexo `delta_nao_commitado.diff`, sem numeração local].

## 13/11 09:33 · sessao d64075348a39 · - · - · origem=Ronald (tmux)

As 3 ERROR vêm do hunk `ponto/supra_juiz.py @@ -536,15 +549,25 @@`: o executor passou dos campos crus ao juiz real; os doubles ficaram com interface antiga. A intenção está declarada em `app/docs/CORTES.md:449` e `app/core/juizes.py:294`.

| Falha | Prova | Veredito |
|---|---|---|
| `test_resposta_direta_barra` | `_P` só oferece `respondida_em` (`app/ponto/tests/test_supra_juiz_executor.py:14`); o novo caminho exige `validada_em`, `materializada_em`, texto e PK (`app/chamados/models.py:1899`, `app/ponto/supra_juiz.py:559`). | **CURADA-PELO-DELTA** fornecido; execução Django completa [nao verificado]. |
| `test_resposta_na_disputa_barra` | `_D` só oferece `perguntas` (`app/ponto/tests/test_supra_juiz_executor.py:27`); o novo executor chama `respostas_sem_veredito()` (`app/ponto/supra_juiz.py:558`). | **CURADA-PELO-DELTA** fornecido; execução Django completa [nao verificado]. |
| `test_sem_conversa_executa` | Mesmo método ausente, antes de alcançar o predicado (`app/ponto/tests/test_supra_juiz_executor.py:39`, `app/ponto/supra_juiz.py:558`). | **CURADA-PELO-DELTA** fornecido; execução Django completa [nao verificado]. |

A pergunta antiga era “há `respondida_em`?”; a nova é “há resposta sem validação/materialização, não dispensada pela tranca?”, abrangendo carimbo **OU** texto. A obra fez essa mudança deliberadamente (`app/chamados/models.py:1898`, `app/docs/CORTES.md:449`). Os três asserts antigos continuam corretos para perguntas sem veredito; quem precisava atualização eram seus doubles (`app/ponto/tests/test_supra_juiz_executor.py:39`).

O delta acrescenta campos, PK e delegação necessários; seu mock de `dia_da_pergunta_trancado=False` isola a questão de veredito, evitando exigir a disputa e o dia reais do double (`app/chamados/models.py:1902`, `app/chamados/juizes.py:991`). Extraí por AST as funções reais e executei, exclusivamente em memória, os seis casos com os doubles do delta: resultados esperados nos três ERROR, vazio, texto sem carimbo e resposta com veredito; suíte Django integral [nao verificado].

O tripwire é provocado pela função nova, cujo terminal conta diretamente `VIVOS` (`app/chamados/juizes.py:2523`), precisamente a forma proibida pelo selo (`app/core/tests/test_tripwire_contagem_de_pendencia.py:23`, `app/core/tests/test_tripwire_contagem_de_pendencia.py:370`). O delta fornecido não altera esse contador [nao verificado].

A adaptação que preserva **os candidatos provenientes da trilha**, sem recalcular conversa nem acrescentar filtro de categoria, é:

```python
from chamados.services.painel import qs_na_fila

return qs_na_fila(
    _base=ChamadoColaborador.objects.filter(pk__in=_ids)
).count()
```

A trilha permanece a definida em `app/chamados/juizes.py:2520`; `_base` é repassada até o filtro vivo (`app/chamados/services/painel.py:153`, `app/chamados/services/painel.py:188`). Validei a proposta contra o AST real do tripwire em memória: deixou de produzir sítio não conforme (`app/core/tests/test_tripwire_contagem_de_pendencia.py:350`).

**Ressalva indispensável:** isso não preserva literalmente o total de **todos os VIVOS**. `VIVOS` inclui `registrado` (`app/chamados/catalogo/motor.py:24`), enquanto `qs_na_fila` limita a `COBRANCA` e remove silenciados no futuro (`app/chamados/services/painel.py:189`). Portanto “mesmos candidatos da trilha, julgados pela fila” é realizável; “mesmo total VIVOS para qualquer estado possível, usando apenas `qs_na_fila`” é incompatível com esse juiz. A inexistência de registrados/silenciados entre esses pais em produção é [nao verificado].

## 13/11 09:32 · sessao 96e9d46f0903 · - · - · origem=Ronald (tmux)

**`test_03`: PRE-EXISTENTE quanto à porta; causa exata da execução [nao verificado].**

- A pergunta do teste é: “a gravação da própria pergunta aparece como sucesso, com o erro das irmãs separado?”. O teste espera isso em `app/chamados/tests/test_validar_diz_o_seu.py:87`; a implementação separa próprios/irmãs em `app/chamados/services/validacao.py:487`, mas, havendo erros e `materializada_em=None`, retorna `nao_gravou` mesmo sem erro próprio (`app/chamados/services/validacao.py:490`, `app/chamados/services/validacao.py:504`).
- A mensagem **não prova competência realmente aprovada/exportada**: o próprio teste injeta `IRMA`, contendo “competencia … aprovado”, em `app/chamados/tests/test_validar_diz_o_seu.py:19`, `app/chamados/tests/test_validar_diz_o_seu.py:47`, `app/chamados/tests/test_validar_diz_o_seu.py:85`. A casca traduz o primeiro erro em `app/chamados/services/acoes_disputa.py:32`; a tradução casa qualquer marcador, incluindo “competencia”, em `app/chamados/catalogo/recusa.py:35`, `app/chamados/catalogo/recusa.py:81`.
- A porta real lê **timestamp → competência do fato → FechamentoMensal.status**, sem consultar “hoje”: `app/ponto/janelas.py:84`, `app/ponto/janelas.py:86`, `app/ponto/janelas.py:87`; sua chamada ocorre em `app/ponto/registro_batida.py:132`. Esses arquivos não constam dos hunks fornecidos [nao verificado].
- O gate novo não retém o chamado deste fixture: ele nasce com categoria `saida_sem_entrada` (`app/chamados/tests/test_validar_diz_o_seu.py:33`), enquanto o gate retorna `True` fora de `disputa_supervisao` (`app/chamados/models.py:716`).
- A validação antecede a materialização (`app/chamados/services/validacao.py:435`, `app/chamados/services/validacao.py:461`), e o novo juiz exclui pergunta já validada (`app/chamados/models.py:1899`); a condição `validada_em__isnull=True` também existia no hunk anterior fornecido [nao verificado].
- O fixture combina data literal de junho com `timezone.now()` e não declara congelamento (`app/chamados/tests/test_validar_diz_o_seu.py:7`, `app/chamados/tests/test_validar_diz_o_seu.py:23`, `app/chamados/tests/test_validar_diz_o_seu.py:38`). **Atribuir esta falha ao relógio, ou explicar por que a materialização ficou nula nesta execução, permanece [nao verificado].**
- O delta fornecido não altera esse teste nem essa cadeia; não apresenta cura dele [nao verificado]. A reprodução da mesma falha na base também permanece [nao verificado]. [nao verificado]

## 13/11 09:28 · sessao 8bbb526bb12e · FERIADO-ESCALA · sonda · origem=Code

```text
sonda_leitura: sonda2.py [b28219536c3a7380] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

== (1) ec1264
  col239 te517 ini=2026-09-20 posto463 trabalha=True ativa=True
  vinc ec206 te212 5x2 ini=2026-07-21 fim=2026-09-19 ativa=False trabalha=True
  vinc ec1264 te517 5x2 ini=2026-09-20 fim=None ativa=True trabalha=True
  hist 968 tipo=escala motivo=admissao data=2026-07-21 criado=2026-08-27 17:22:45.961147+00:00 por=None
  hist 1849 tipo=escala motivo=mudanca data=2026-09-19 criado=2026-09-19 21:48:00.271174+00:00 por=None
  LogAuditoria campos: ['id', 'usuario', 'acao', 'modelo', 'objeto_id', 'descricao', 'valor_antes', 'valor_depois', 'ip', 'ator', 'criado_em']
  log 530254 regenerar_celulas_vinculo None 2026-09-19 21:48:00.267970+00:00 {'ate': '2026-09-20', 'desde': '2026-09-20', 'alteradas': 1, 'apesar_da_lavra': None, 'barrados_exportado': 0, 'cobrancas_orfas_marcadas': 0}
== (2) sincronizar_feriado_admin restrito aos 16 (desejado = not administrativo)
   (2, 'desejado=False') 1
   (2, 'desejado=True') 8
   (3, 'desejado=True') 4
   (4, 'desejado=False') 1
   (4, 'desejado=True') 2
== (3) janelas e competencia fechada pelo juiz K9
  emp2 janela 2026-09-21..2026-10-20 (mes=10/2026) fechada=[]
  emp2 janela 2026-10-21..2026-11-20 (mes=11/2026) fechada=[]
  emp3 janela 2026-09-21..2026-10-20 (mes=10/2026) fechada=[]
  emp3 janela 2026-10-21..2026-11-20 (mes=11/2026) fechada=[]
  emp4 janela 2026-09-21..2026-10-20 (mes=10/2026) fechada=[]
  emp4 janela 2026-10-21..2026-11-20 (mes=11/2026) fechada=[]
== (4) feriados na janela por colab dos 16 (celula trabalha/origem; fatos_do_dia faixa)
  ec203 col236 emp2 ini=2026-07-21: 2026-10-12 cel=True/gerada faixa=ferias ; 2026-11-02 cel=True/gerada faixa=ferias ; 2026-11-15 cel=False/gerada faixa=feriado ; 2026-11-20 cel=True/gerada faixa=trabalho
  ec328 col376 emp2 ini=2026-07-21: 2026-10-12 cel=True/gerada faixa=atestado ; 2026-11-02 cel=True/gerada faixa=trabalho ; 2026-11-15 cel=False/gerada faixa=feriado ; 2026-11-20 cel=True/gerada faixa=trabalho
  ec506 col181 emp2 ini=2026-07-21: 2026-10-12 cel=True/gerada faixa=trabalho ; 2026-11-02 cel=True/gerada faixa=trabalho ; 2026-11-15 cel=False/gerada faixa=feriado ; 2026-11-20 cel=True/gerada faixa=trabalho
  ec749 col142 emp4 ini=2026-06-01: 2026-10-12 cel=True/gerada faixa=trabalho ; 2026-11-02 cel=True/gerada faixa=trabalho ; 2026-11-15 cel=False/gerada faixa=feriado ; 2026-11-20 cel=True/gerada faixa=trabalho
  ec813 col93 emp3 ini=2026-06-24: 2026-10-12 cel=False/gerada faixa=feriado ; 2026-11-02 cel=True/gerada faixa=trabalho ; 2026-11-15 cel=False/gerada faixa=feriado ; 2026-11-20 cel=True/gerada faixa=trabalho
  ec826 col118 emp3 ini=2026-06-24: 2026-10-12 cel=True/gerada faixa=ferias ; 2026-11-02 cel=True/gerada faixa=trabalho ; 2026-11-15 cel=False/gerada faixa=feriado ; 2026-11-20 cel=True/gerada faixa=trabalho
  ec854 col148 emp3 ini=2026-06-24: 2026-10-12 cel=False/gerada faixa=feriado ; 2026-11-02 cel=True/gerada faixa=trabalho ; 2026-11-15 cel=False/gerada faixa=feriado ; 2026-11-20 cel=True/gerada faixa=trabalho
  ec964 col709 emp2 ini=2026-07-21: 2026-10-12 cel=False/gerada faixa=feriado ; 2026-11-02 cel=True/gerada faixa=trabalho ; 2026-11-15 cel=False/gerada faixa=feriado ; 2026-11-20 cel=True/gerada faixa=trabalho
  ec1101 col902 emp2 ini=2026-08-17: 2026-10-12 cel=False/gerada faixa=feriado ; 2026-11-02 cel=True/gerada faixa=trabalho ; 2026-11-15 cel=False/gerada faixa=feriado ; 2026-11-20 cel=True/gerada faixa=trabalho
  ec1111 col712 emp4 ini=2026-08-03: 2026-10-12 cel=True/gerada faixa=trabalho ; 2026-11-02 cel=True/gerada faixa=trabalho ; 2026-11-15 cel=False/gerada faixa=feriado ; 2026-11-20 cel=True/gerada faixa=trabalho
  ec1208 col940 emp2 ini=2026-09-11: 2026-10-12 cel=False/gerada faixa=feriado ; 2026-11-02 cel=True/gerada faixa=trabalho ; 2026-11-15 cel=False/gerada faixa=feriado ; 2026-11-20 cel=True/gerada faixa=trabalho
  ec1235 col857 emp4 ini=2026-08-21: 2026-10-12 cel=False/gerada faixa=feriado ; 2026-11-02 cel=False/gerada faixa=feriado ; 2026-11-15 cel=False/gerada faixa=atestado ; 2026-11-20 cel=True/gerada faixa=trabalho
  ec1264 col239 emp2 ini=2026-09-20: 2026-10-12 cel=True/gerada faixa=trabalho ; 2026-11-02 cel=True/gerada faixa=trabalho ; 2026-11-15 cel=False/gerada faixa=feriado ; 2026-11-20 cel=True/gerada faixa=trabalho
  ec1475 col1007 emp2 ini=2026-11-03: 2026-11-15 cel=False/gerada faixa=feriado ; 2026-11-20 cel=True/gerada faixa=trabalho
  ec1492 col152 emp3 ini=2026-10-20: 2026-11-02 cel=True/gerada faixa=trabalho ; 2026-11-15 cel=False/gerada faixa=feriado ; 2026-11-20 cel=True/gerada faixa=trabalho
  ec1524 col1025 emp2 ini=2026-11-02: 2026-11-02 cel=True/gerada faixa=trabalho ; 2026-11-15 cel=False/gerada faixa=feriado ; 2026-11-20 cel=True/gerada faixa=trabalho
== (5) vigentes por ciclo sem regra literal no aval (semanal, 24x48) e 12x36 em posto opera=False
  semanal/24x48: 0
  12x36 posto opera=False: 2
erros 0
```

## 13/11 09:24 · sessao 166b54ce05bb · FERIADO-ESCALA · censo · origem=Code

O Vincular já usa o juiz de 16/09 para criar vínculo; o formulário não oferece escolha de feriado e o reaproveitamento de vínculo preserva a flag existente. (`app/colaboradores/services/vinculo.py:259`, `app/colaboradores/services/vinculo.py:296`, `app/templates/colaboradores/partials/painel_vinculo.html:136`)

Censo estático. Execução dos testes, identidade do HEAD por Git e resultados de produção além do raspado: **[não verificado]**. As comparações com o aval usam a transcrição fornecida; localização física de `aval.txt`: **[não verificado]**. [nao verificado]

**1. Escritores — produção**

| Sítio | Origem do valor | Passa por `executar_vinculo`? |
|---|---|---|
| `app/colaboradores/services/vinculo.py:287` — criação normal | Explícito: `padrao_trabalha_em_feriado(template, colaborador.posto)`, em `:296`. | Sim, está dentro dele, definido em `:170`. |
| `app/colaboradores/services/vinculo.py:970` — `criar_trecho_retroativo` | Cópia de `ec.trabalha_em_feriado`, em `:974`. | Não. Chamado pelo comando de correção retroativa: `app/colaboradores/management/commands/corrigir_escala_retroativa.py:196`. |
| `app/colaboradores/services/vinculo.py:1089` — `ajustar_vinculo_pelo_sistema` | Cópia de `ec.trabalha_em_feriado`, em `:1092`. | Não. Chamador: `app/escala/services/escala_auto_executor.py:103`. |
| `app/colaboradores/services/vinculo.py:1110` — `desfazer_ajuste_do_sistema` | Cópia de `antigo.trabalha_em_feriado`, em `:1113`. | Não. Chamador: `app/escala/services/escala_auto_executor.py:145`. |
| `app/colaboradores/services/vinculo.py:893` — alteração de existente | Argumento `desejado`; persiste com `save(update_fields=['trabalha_em_feriado'])`, em `:894`. | Não; usa a porta específica `marcar_trabalha_em_feriado`, definida em `:869`. |
| `app/colaboradores/management/commands/sincronizar_feriado_admin.py:47` | Deriva `desejado = not cargo_e_administrativo(cargo)`; administrativos cadastrados em `:4` recebem `False`, demais `True`. | Não; chama `marcar_trabalha_em_feriado` em `:56`, somente com `--apply` (`:53`). |
| `app/escala/migrations/0014_escalacolaborador_trabalha_em_feriado.py:13` | `AddField` com `default=True`, em `:16`. Aplicação dessa migration nesta produção: **[não verificado]**. | Não. |

Os chamadores de produção de `executar_vinculo` são a view do Vincular, a troca pelo chat, a realocação da régua e o comando de fase: `app/colaboradores/views.py:423`; `app/colaboradores/services/troca_pelo_chat.py:75`; `app/escala/regua_defesa.py:321`; `app/ponto/management/commands/censo_fase_12x36.py:423`. O caminho do chat chega pelo executor da proposta em `app/core/views_ia.py:1008`.

Há ainda escrita genérica por `**campos` em `gravar_regua`: `app/colaboradores/services/vinculo.py:1048`, `app/colaboradores/services/vinculo.py:1070`. Os chamadores atuais fornecem somente campos `regua_*`, sem a flag: `app/escala/regua_defesa.py:662`, `app/escala/regua_defesa.py:719`, `app/escala/regua_defesa.py:742`, `app/escala/regua_defesa.py:765`.

Reutilizar vínculo faz `existente.save()` preservando a flag; restaurar snapshot atualiza somente tipo, início, fim e atividade. (`app/colaboradores/services/vinculo.py:259`, `app/colaboradores/services/vinculo.py:283`, `app/colaboradores/services/vinculo.py:1031`)

O Django admin não cria nem altera esse modelo: `EscalaColaboradorAdmin` usa `SoLeitura`; seus fieldsets também omitem a flag. (`app/escala/admin.py:19`, `app/escala/admin.py:30`, `app/core/admin.py:59`, `app/core/admin.py:62`)

Outros criadores por ModelForm, serializer, importador, fixture de produção, `get_or_create`, `update_or_create`, `bulk_create`, construtor seguido de `save`, `setattr` ou reflexão não foram localizados na busca estática; ausência universal de caminhos dinâmicos: **[não verificado]**.

**Escritores — testes** [nao verificado]

As criações diretas abaixo não passam por `executar_vinculo`; o inventário das omissões está no apêndice. Quando omitida, a flag recebe o default atual `True`. (`app/escala/models.py:896`)

| Origem explícita | Sítios de criação |
|---|---|
| `False` | `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:90`; `app/ponto/tests/test_a2_precedencia.py:70`; `app/core/tests/test_contract_feriado.py:139`; `app/escala/tests/test_celula_dia.py:403`; `app/escala/tests/test_escala_models.py:268`; `app/chamados/tests/test_e2_celula_regenerada_reavalia.py:112`; `app/chamados/tests/test_varredura_feriado_evento.py:46`. |
| `True` | `app/colaboradores/tests/test_feriado_admin_sync.py:34`; `app/ponto/tests/test_a2_precedencia.py:283`; `app/ponto/tests/test_precedencia_vinculo_encerrado.py:43`, `:47`, `:98`; `app/chamados/tests/test_e2_celula_regenerada_reavalia.py:57`; `app/chamados/tests/test_jv1_feriado_vira_vigia.py:56`. |
| Parâmetro do helper | `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:82`; `app/ponto/tests/test_reavaliar_feriado_pelo_juiz.py:54`; `app/ponto/tests/test_furo_feriado_pelo_juiz.py:50`; `app/ponto/tests/test_e4cal_l1_d4_natureza.py:78`; `app/escala/tests/test_feriado_padrao_por_escala.py:109`; `app/chamados/tests/test_jv1_volta5_posto_do_dia_nos_vetos.py:41`; `app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:77`; `app/chamados/tests/test_jv1_volta4_posto_do_dia.py:38`. |
| `**kwargs`: pode receber a flag do chamador; sem ela vale o default | `app/colaboradores/tests/test_trilha_do_vinculo.py:66`; `app/ponto/tests/test_precedencia_pela_celula.py:59`; `app/escala/tests/test_e4_parte4_cinco_copias.py:54`; `app/escala/tests/test_escala_vigente_pergunta_a_celula.py:55`; `app/escala/tests/test_e4_vinculo_cobre_a_data.py:25`; `app/escala/tests/test_e4_parte5_copias.py:66`; `app/chamados/tests/test_h06_falta_pela_grade.py:60`. Há chamada explicitando `True` em `app/escala/tests/test_e4_parte4_cinco_copias.py:131`. |

Há dois `bulk_create`, com construtores que omitem a flag: `app/colaboradores/tests/test_situacional_pergunta_em_lote.py:61`, `:62`; `app/ponto/tests/test_col129_teto_vira_lista.py:280`. Outros três construtores não são salvos: `app/ponto/tests/test_r4_celula_sem_marcos.py:49`; `app/escala/tests/test_ancora_retroativa.py:124`; `app/escala/tests/test_propositor_escala.py:144`.

Mutações diretas de teste: [nao verificado]

- `update(False)`: `app/chamados/tests/test_e2_celula_regenerada_reavalia.py:76`; `app/chamados/tests/test_jv1_feriado_vira_vigia.py:162`, `:182`; `app/ponto/tests/test_precedencia_vinculo_encerrado.py:69`; `app/ponto/tests/test_o280_bateria_da_lei.py:1736`.
- Atribuição `False` seguida de `save(update_fields=...)`: `app/ponto/tests/test_a2_precedencia.py:310`, `:311`.
- Atribuição `True` somente em memória: `app/escala/tests/test_escala_models.py:277`.
- Pela porta da flag: `app/escala/tests/test_porta_escala.py:62`, `:65`; `app/chamados/tests/test_jv1_feriado_vira_vigia.py:89`; `app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:133`; exercício sem banco em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:445`.

Chamadas diretas de teste ao executor: `app/colaboradores/tests/test_pos_vinculo.py:23`; `test_vinculo_linha_do_tempo.py:148`; `test_vinculo_folga_padrao.py:49`; `test_vinculo_ancora_so.py:21`; `test_historico_vinculo.py:14`, `:23`, `:26`; `test_vinculo_ancora_geometria.py:54`; `test_trilha_do_vinculo.py:75`; `test_vinculo_regenera_celulas.py:41`, todos sob `app/colaboradores/tests/`.

O modelo falso do teste sem banco declara default `True` e instala juiz falso; não é persistência de `EscalaColaborador` real. (`app/ponto/tests/test_cadastro_futuro_sem_banco.py:250`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:252`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:328`)

**2. Leitores**

| Leitor | Decisão |
|---|---|
| `app/escala/models.py:1390` | `feriado_vira_folga`: com flag verdadeira, não transforma feriado em folga; com falsa, exige ciclo permitido e feriado do lugar do vínculo (`:1392`, `:1395`). |
| `app/escala/models.py:929` | Filtra vínculos vigentes de colaborador ativo, flag verdadeira e ciclo 5x2/6x1; separa declaração do posto em `:932`. |
| `app/colaboradores/services/vinculo.py:890` | Idempotência da troca; lê valor anterior em `:892`, usado no motivo em `:907` e `:912`. |
| `app/colaboradores/services/vinculo.py:974`, `:1092`, `:1113` | Lê para copiar a flag nas três criações derivadas. |
| `app/colaboradores/management/commands/sincronizar_feriado_admin.py:48`, `:52` | Detecta divergência e imprime valor atual versus desejado por cargo. |
| `app/escala/models.py:1369` | Pelo predicado, transforma base de trabalho `True` em `False`. |
| `app/ponto/precedencia.py:376` | Pelo predicado do vínculo, decide se feriado suprime trabalho ou fica somente no pagamento. |
| `app/ponto/services/fechamento.py:949` | Pelo mesmo predicado, separa feriado trabalhado de folga trabalhada; direciona HE100 em `:955` e exclui da folga em `:965`. |
| `app/ponto/calculador/alimentacao.py:521` | Alimenta `folga_e_feriado`; a rubrica é escolhida em `app/ponto/calculador/regras.py:684`. |
| `app/ponto/management/commands/detectar_ausencias.py:617` | Consome a faixa da precedência e veta cobrança por feriado. |
| `app/chamados/services/feriado_reavaliar.py:34` | Consome precedência; seleciona falta espúria nas faixas feriado/folga em `:37`. O vigia conta e alarma em `app/ponto/management/commands/reavaliar_ausencias_feriado.py:39`, `:43`. |

Há decisões independentes da flag: o veto por posto fechado consulta `opera_em_feriado`, e a classificação da fila marca `feriado_nao_lido` só pela data no conjunto de feriados. Esta última está registrada como pendência. (`app/colaboradores/services/feriado_do_posto.py:69`, `app/ponto/management/commands/detectar_ausencias.py:629`, `app/chamados/services/fila_por_causa.py:228`, `app/core/juizes.py:470`)

O comando por cargo deriva um valor próprio para a flag; o juiz de nascimento deriva por ciclo/base/posto. (`app/colaboradores/management/commands/sincronizar_feriado_admin.py:47`, `app/escala/models.py:911`)

Leitores diretos adicionais em HTML, JS, API/serializers, forms, relatórios ou PDF não foram encontrados; negativo global: **[não verificado]**. Os consumidores indiretos de previsto/DNA não equivalem a uma nova leitura da flag: ela não está no blob produzido. (`app/ponto/management/commands/gerar_celulas.py:104`)

**3. Juiz `padrao_trabalha_em_feriado`**

A chamada de produção está na criação normal: `app/colaboradores/services/vinculo.py:296`. Chamadas reais de teste: `app/escala/tests/test_feriado_padrao_por_escala.py:46`, `:64`; substituição por stub: `app/ponto/tests/test_cadastro_futuro_sem_banco.py:328`.

Para qualquer combinação de ciclo/base, posto declarado `True` retorna `True`; declarado `False` retorna `False`, antes de examinar o tipo. (`app/escala/models.py:911`, `app/escala/models.py:913`)

Com posto `None` ou sem declaração:

| `tipo_ciclo` | Base `comercial` | Bases `12x36`, `turno_partido`, `plantao`, `intermitente` |
|---|---:|---:|
| `5x2` | `False` | `True` |
| `6x1` | `False` | `True` |
| `12x36` | `True` | `True` |
| `semanal` | `True` | `True` |
| `24x48` | `True` | `True` |
| `personalizado` | `True` | `True` |
| `intermitente` | `True` | `True` |

Toda a tabela decorre de `app/escala/models.py:902`, `app/escala/models.py:916`, `app/escala/models.py:917`. Sem tipo e sem declaração, retorna `True`. (`app/escala/models.py:914`)

Os sete ciclos são os choices de `app/escala/models.py:36`; default de ciclo `12x36` em `:117`. As cinco bases são os choices de `app/escala/models.py:163`; default de base `12x36` em `:170`.

**4. Vincular e troca da flag existente**

A tela é `colaboradores/vincular.html`, servida por `vincular_colaboradores`; o painel é carregado por `painel_vinculo`. (`app/colaboradores/urls.py:9`, `app/colaboradores/views.py:126`, `app/colaboradores/views.py:262`)

O formulário é HTML no partial. Expõe `q`, `template_id`, `data_ancora`, `confirm_txt`, `data_inicio` e botão `modo=direto`; os valores iniciais de escala, âncora e início vêm do vínculo ativo. **A flag não aparece e não tem valor inicial visual.** (`app/templates/colaboradores/partials/painel_vinculo.html:136`, `:149`, `:155`, `:159`, `:163`, `:167`, `:175`)

O POST chega à rota `<cid>/vincular/escala/`; a view exige POST e `editar_escalas`, chama `julgar_vinculo` e, salvo `no_op`, `executar_vinculo`. O parser não recebe escolha de feriado. (`app/colaboradores/urls.py:13`, `app/colaboradores/views.py:382`, `:411`, `:416`, `:423`; `app/colaboradores/services/vinculo.py:53`, `:59`, `:144`)

A troca específica de existente é `marcar_trabalha_em_feriado`. Mesmo valor retorna; valor diferente salva somente a flag. O chamador de produção encontrado é `sincronizar_feriado_admin`, sem autor/motivo explícitos. (`app/colaboradores/services/vinculo.py:869`, `:890`, `:894`; `app/colaboradores/management/commands/sincronizar_feriado_admin.py:56`)

A função chama:

- **Passado/hoje:** regeneração desde `max(data_inicio, início da competência anterior)` até hoje, com extensão automática desativada. (`app/colaboradores/services/vinculo.py:899`, `:900`, `:904`, `:906`)
- **Futuro:** suplemento de células já existentes, `origem='gerada'`, de amanhã até o fim da união das competências correntes; preserva dias atribuídos a outro vínculo e chama a mesma porta por dia. (`app/colaboradores/services/vinculo.py:910`; `app/ponto/services/cadastro_futuro.py:26`, `:27`, `:31`, `:40`, `:44`; `app/ponto/management/commands/gerar_celulas.py:299`)

A porta preserva células editadas e barra dias lavrados; ao reescrever geradas, guarda DNA anterior, instante e número de regenerações. (`app/ponto/portas/celula.py:118`, `:284`, `:297`, `:305`, `:310`)

A trilha encontrada é **condicional à regeneração alterar células**: evento `regenerar_celulas_vinculo`, modelo/objeto do vínculo, janela e contagens no `valor_depois`; o texto do motivo contém flag antes→depois. A função de troca não grava evento próprio da flag com valores estruturados antes/depois; a fotografia geral do vínculo também não inclui a flag. (`app/ponto/portas/celula.py:369`, `:373`, `:380`; `app/colaboradores/services/vinculo.py:907`, `:912`, `:28`, `:42`)

Depois, a porta rejulga dias tocados até hoje, exclui vereditos humanos, recalcula por competência e agenda os eventos de rejunção/falta espúria. (`app/ponto/portas/celula.py:460`, `:465`, `:469`, `:472`, `:494`, `:496`, `:528`, `:538`, `:540`)

**5. CelulaDia, DNA e cartório**

A geração chama `eh_dia_trabalho_calculado`, grava o resultado em `CelulaDia.trabalha` e o fornece ao produtor do DNA. Marcos/lâmpadas só são montados para `trabalha is True`; a flag não é congelada como chave do blob. (`app/ponto/portas/celula.py:911`, `:914`, `:917`; `app/ponto/management/commands/gerar_celulas.py:88`, `:104`)

`eh_dia_trabalho_calculado` calcula pela fase/foto/template e, na parte semanal, transforma base `True` em `False` por `feriado_vira_folga`. O leitor `eh_dia_trabalho` prefere a célula existente e usa o cálculo no fallback. (`app/escala/models.py:1252`, `:1289`, `:1366`, `:1369`, `:1092`, `:1096`)

`CICLOS_FERIADO_VIRA_FOLGA = (6x1, 5x2, semanal)`. O predicado exige flag falsa, ciclo nessa tupla e feriado do lugar do vínculo; `12x36`, `24x48`, personalizado e intermitente não viram folga por essa flag. (`app/escala/models.py:32`, `:1390`, `:1392`, `:1395`)

`fatos_do_dia` delega ao período de um dia. A implementação escolhe o vínculo com `vinculo_do_dia`, lê `cel.trabalha` como previsto e só calcula pelo vínculo quando a célula não responde. A cláusula vigente é:

`feriado and previsto is True and ec is not None and not ec.feriado_vira_folga(data)`.

Portanto o código atual consulta o predicado compartilhado, além da flag e do ciclo contidos nele. (`app/ponto/precedencia.py:256`, `:349`, `:363`, `:366`, `:376`, `:377`)

Para rejulgar 02/11, a porta unitária é `ponto.services.cartorio.julgar_celula(colaborador, data, apply_=True, forcar=True)`. Ela busca a célula existente, entrega-a ao julgamento e depois carimba a precedência por `fatos_do_dia`; célula futura/inexistente retorna `None`. **Rejulgar não regenera o previsto/DNA.** (`app/ponto/services/cartorio.py:2011`, `:2025`, `:2027`, `:2040`, `:2048`, `:1999`)

A resposta a “célula ou vínculo?” é **ambos**: célula/DNA para o julgamento e previsto; vínculo escolhido para o dia para o predicado de feriado. (`app/ponto/services/cartorio.py:1437`, `:1465`; `app/ponto/precedencia.py:349`, `:363`, `:377`)

O resultado efetivo de col148 após eventual cura, a regeneração de 20/11 e a relação individual dos outros vínculos do lote: **[não verificado]**. O raspado fornecido não contém essa relação nem o resultado posterior.

**6. Contadores, ESTADO, placar e HAIKU**

O contador existente seleciona vínculo que cobre hoje, colaborador ativo, ciclo 5x2/6x1 e flag `True`; não filtra base. Separa `declarados` pelo posto do vínculo com `opera_em_feriado=True`; `nao_declarados = total - declarados`. Ele não distingue origem por default, cópia ou escolha. (`app/escala/models.py:925`, `:929`, `:932`, `:933`)

O command imprime as três contagens; o placar o executa e publica a linha fora do TOTAL do Code. Está declarado em `FORA_DE_PIPELINE`, com motivo para não cronificar. (`app/escala/management/commands/vinculos_5x2_6x1_trabalha_feriado.py:15`, `:16`, `:17`; `bin/placar_code.sh:298`, `:300`; `app/config/crons.py:769`, `:1227`)

Publicação desse contador no ESTADO ou copiloto e existência do novo `feriado_por_padrao_5x2`: não localizadas; **[não verificado]**.

Os pontos concretos de integração existentes são:

| Destino | Registro/composição |
|---|---|
| ESTADO | `bin/gerar_estado.py:178` compõe o documento. Exemplo de contador: leitura em `:260`, posição no texto em `:329`, interpolação em `:374`. `bin/relato.sh:38` chama o gerador. |
| Snapshot de contadores para a ponte da IA | Dicionário `saida['contadores']` em `app/api/views_mensageria.py:643`; rota em `app/api/urls.py:16`. |
| Entrada efetiva no contexto do copiloto | `mensageria/nucleo/ferramentas.py:372`; chamadas dos blocos em `:380` e `:418`. Exemplo de pergunta roteada: reconhecimento em `:1890`, ferramenta em `:1900`, bloco em `:1912`. |
| Cliente HTTP do bloco | Exemplo em `mensageria/nucleo/core_client.py:331`. |
| Pergunta de avaliação HAIKU | Lista `GOLDEN` em `eval/golden.py:5`; exemplo de registro de pergunta em `:337`; medidor correspondente em `eval/oraculo.py:17`, `:62`. |

A ferramenta atual `saude_da_operacao` transforma `dados['metricas']` e retorna somente as medidas de saúde; não repassa o dicionário `contadores` da API. Assim, acrescentar chave somente naquela resposta não equivale a fazê-la chegar ao contexto por essa ferramenta. (`mensageria/nucleo/ferramentas.py:265`, `:294`; `app/api/views_mensageria.py:643`)

**7. Contradições/fatos a confrontar com o aval**

- **(a) Posto versus ciclo:** posto declarado vence todo tipo: 5x2 com posto `True` nasce `True`; 12x36 com posto `False` nasce `False`. Isso diverge das duas atribuições incondicionais por ciclo transcritas no aval. (`app/escala/models.py:911`, `:913`; `app/escala/tests/test_feriado_padrao_por_escala.py:59`)
- **(b) 6x1:** base comercial sem posto declarado recebe `False` automaticamente; no posto declarado `True`, nasce `True` sem escolha enviada pelo POST. (`app/escala/models.py:916`; `app/escala/tests/test_feriado_padrao_por_escala.py:73`, `:86`)
- **(c) Intermitente/personalizado/outros:** sem declaração do posto, o juiz retorna `True` para intermitente, personalizado, semanal e 24x48; o formulário não exige escolha. A flag falsa só transforma feriado em folga nos três ciclos da tupla, excluindo intermitente/personalizado/24x48. Regra nova para semanal/24x48 no aval recebido: **[não verificado]**. (`app/escala/models.py:916`, `:32`, `:1392`; `app/templates/colaboradores/partials/painel_vinculo.html:136`)
- **(d) Base não comercial:** 5x2 com `turno_partido`, ou qualquer base diferente de comercial, nasce `True` sem declaração; o teste fixa expressamente o turno partido. O predicado de folga, quando a flag é

[truncado em 20000 de 46776 caracteres]

## 13/11 09:18 · sessao 71e9ef609e10 · - · - · origem=Ronald (tmux)

Censo entregue nas mensagens partes 1–6, incluindo todos os 807 nós estáticos de teste: 800 `objects.create`, dois `bulk_create` e cinco construtores; três construtores são apenas objetos em memória. Referências individuais constam nos inventários enviados. [nao verificado]

Produção: quatro criadores, todos com valor explícito: juiz em `app/colaboradores/services/vinculo.py:287`, cópias em `:970`, `:1089`, `:1110`; única mudança literal persistida em `:893`, com `save(update_fields=...)` em `:894`.

Limite: ausência de escritores produzidos por reflexão ou código construído em strings permanece [não verificado].

## 13/11 09:14 · sessao c0661e3c88a4 · - · - · origem=Ronald (tmux)

**Leitores diretos de produção**

| Local | O que lê/decide |
|---|---|
| `app/escala/models.py:1390` | `feriado_vira_folga`: flag `True` impede folga por feriado; com `False`, exige ciclo `6x1/5x2/semanal` (`:1392`, constante em `:32`) e feriado no lugar do vínculo (`:1394-1395`). |
| `app/escala/models.py:929` | Contador seleciona vínculos vigentes, colaborador ativo, flag `True` e ciclo `5x2/6x1`; separa posto declarado `True` em `:932`, retorna total/declarados/não declarados em `:933`. Não filtra `tipo_base`. |
| `app/colaboradores/services/vinculo.py:890` | Compara flag com desejado para idempotência; guarda valor antigo em `:892`, usa antes/depois no motivo da regeneração em `:907/:912`. |
| `app/colaboradores/services/vinculo.py:974` | Copia flag do vínculo anterior ao criar vínculo temporário. |
| `app/colaboradores/services/vinculo.py:1092` | Copia flag ao iniciar novo vínculo. |
| `app/colaboradores/services/vinculo.py:1113` | Copia flag do antigo ao reconstruir vínculo. |
| `app/colaboradores/management/commands/sincronizar_feriado_admin.py:48` | Detecta divergência do desejado por cargo; imprime flag atual em `:52`; desejado vem de `not cargo_e_administrativo(...)` em `:47`, grava pela porta em `:56`. |

**Leitores por predicado e efeito**

- `app/escala/models.py:1369`: `eh_dia_trabalho_calculado` transforma `base is True` em `False` quando `feriado_vira_folga` responde sim.
- `app/ponto/precedencia.py:347`: carrega célula do dia; `:363` usa `CelulaDia.trabalha` como previsto, `:366` recorre à aritmética do vínculo quando célula não responde. A cláusula atual em `:376-377` consulta `not ec.feriado_vira_folga(data)`, **não somente a flag**. Portanto `12x36` com flag `False` continua trabalho quando previsto.
- `app/ponto/services/fechamento.py:949`: separa período trabalhado em feriado no qual vínculo folga; promove às rubricas HE100/HE100 feriado em `:955-957`, exclui da folga trabalhada em `:965-966`.
- `app/ponto/calculador/alimentacao.py:521`: injeta `folga_e_feriado` a partir do mesmo predicado; `app/ponto/calculador/regras.py:684-690` escolhe HE100 feriado ou folga trabalhada.
- `app/ponto/management/commands/detectar_ausencias.py:617`: consulta precedência e veta faixa feriado. Há veto adicional independente pela marca do posto em `:628-630`; `app/colaboradores/services/feriado_do_posto.py:69-71` trata posto `False` como fechado no feriado, mesmo que vínculo trabalhe.
- `app/chamados/services/fila_por_causa.py:228-229`: classifica qualquer chamado em data do conjunto de feriados como `feriado_nao_lido`, sem consultar flag/ciclo. Está declarado pendente em `app/core/juizes.py:470`.
- Nenhum literal do campo foi encontrado em HTML/JS/TS/TSX, API, forms, relatórios ou PDF; é negativo de busca sem linha própria: **[não verificado]**. O Django admin do vínculo é somente leitura e seus fieldsets omitem a flag (`app/escala/admin.py:18-36`).

**Juiz de criação e tabela completa**

- Única chamada real de produção encontrada: `app/colaboradores/services/vinculo.py:296`.
- Chamadas diretas reais de teste: `app/escala/tests/test_feriado_padrao_por_escala.py:46` e `:64`; stub e instalação do stub em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:252/:328`.
- Tabela, para **cada** par ciclo/base: posto `True → True`; posto `False → False`; posto `None → False` somente se ciclo pertence a `{5x2,6x1}` **e** base é `comercial`; todos os demais pares dão `True`. Tipo ausente e posto não declarado dão `True` (`app/escala/models.py:911-917`).
- Ciclos choices: `12x36`, `semanal`, `24x48`, `5x2`, `6x1`, `personalizado`, `intermitente` (`app/escala/models.py:36-44`); default `12x36` (`:117-120`).
- Bases choices: `12x36`, `turno_partido`, `comercial`, `plantao`, `intermitente`; default `12x36` (`app/escala/models.py:159-174`).
- Logo intermitente/personalizado/semanal/24x48 dão `True` sem posto declarado; `5x2/6x1` não comercial também dão `True` (`app/escala/models.py:916-917`).

**Selos/testes existentes: leitura, não execução** [nao verificado]

Arquivo `app/escala/tests/test_feriado_padrao_por_escala.py`:

| Linhas | Afirmação fixada | Relação com aval |
|---|---|---|
| `48-50` | 5x2 e 6x1 comercial sem declaração dão `False`. | 5x2 permanece compatível; 6x1 automático conflita com escolha obrigatória. |
| `52-54` | 12x36 e 24x48 dão `True`. | 12x36 compatível; 24x48 não está decidido no aval fornecido. |
| `56-57` | 5x2 `turno_partido` dá `True`. | Contraria “5x2 = False”. |
| `59-61` | Posto shopping faz 5x2 `True`; posto fechado faz 12x36 `False`. | Contraria os valores incondicionais novos por ciclo. |
| `63-64` | Sem tipo/posto dá `True`. | Fixação do fallback atual. |
| `78-79` | POST novo 5x2 comercial nasce `False`. | Compatível. |
| `81-84` | Novo 12x36 com âncora nasce `True`. | Compatível. |
| `86-87` | POST 6x1 shopping sem escolha nasce `True`; POST helper só fornece template/início em `73-74`. | Quebraria com recusa de 6x1 sem escolha. |
| `89-97` | Criação ORM sem flag recebe `True`; executar contador não muda existente. | A primeira asserção quebra se default deixar de fornecer `True`; contador continua somente leitura. |
| `112-121`, `123-124` | Contador conta somente `True` 5x2/6x1 e separa declaração do posto; caso vazio zero. | Contador antigo continua com seu universo. |

A comparação acima toma o aval transcrito pelo usuário; localização física de `aval.txt`: **[não verificado]**.

Outros selos relevantes:

- `app/escala/tests/test_escala_models.py:255-279`: 6x1 `False` folga em feriado nacional, dia útil comum trabalha, `True` volta a trabalhar no feriado; compatível com escolha explícita.
- `app/core/tests/test_contract_feriado.py:118-146`: feriado municipal não atravessa cidade para 6x1 `False`.
- `app/ponto/tests/test_contract_juiz_feriado.py:74-80`: tuple de ciclos que viram folga é exatamente `(6x1,5x2,semanal)` e não deve ser duplicado.
- `app/ponto/tests/test_a2_precedencia.py:291-303`: 12x36 previsto trabalha no feriado e feriado fica somente no pagamento; na folga feriado vence. `:305-315` fixa que flag `False` também não suprime plantão 12x36.
- `app/ponto/tests/test_precedencia_vinculo_encerrado.py:62-72`: vínculo encerrado do dia segue trabalho com `True`, feriado suprime com `False`.
- `app/ponto/tests/test_furo_feriado_pelo_juiz.py:67-86`: `False` veta cobrança; `True` segue gate da célula; feriado alheio não veta; posto fechado veta mesmo com flag `True`.
- `app/ponto/tests/test_reavaliar_feriado_pelo_juiz.py:81-105`: reavaliação usa vínculo do dia, fecha `False`, preserva `True`, e célula `True` desatualizada não impede encerramento pelo juiz.
- `app/chamados/tests/test_jv1_volta4_posto_do_dia.py:69-105`: lugar do vínculo do dia prevalece sobre cadastro atual; sem vínculo só nacional. `:147-174`: 12x36 `False/True` continua trabalho; 5x2 `False` suprime.
- `app/chamados/tests/test_jv1_feriado_vira_vigia.py:107-116`: trocar True→False pela porta em competência aberta encerra falta sem cron; `:119-133` exportada preserva célula/chamado e vigia alarma; `:144-154` repetição mantém uma trilha.
- `app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:145-163`: 12x36 flag `False` mantém célula, precedência e falta legítima, vigia zero.
- `app/chamados/tests/test_jv1_volta3_municipio_e_quem_chegou.py:52-74`: evento de município deixa passado intocado e muda futuro para folga.
- `app/chamados/tests/test_e2_celula_regenerada_reavalia.py:84-103`: regeneração encerra falta espúria em feriado e preserva falta legítima; `:125-134` idempotência; `:173-191` rejuízo após encerramento não reabre nem emite outro chamado.
- `app/ponto/tests/test_cadastro_futuro_c3.py:111-116`: falha ao regenerar futuro desfaz save da flag.
- `app/ponto/tests/test_cadastro_futuro_c5b.py:122-146`: trocar flag de outro vínculo não toma célula; dono do dia pode regenerar para folga.
- `app/escala/tests/test_porta_escala.py:58-67`: marcar flag/calendário duas vezes mantém mesmo estado.
- `app/colaboradores/tests/test_feriado_admin_sync.py:38-47`: comando por cargo fixa administrativo `False`, vigilante `True`, idempotente; esta continua uma regra distinta da origem por escala.
- `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:117-164`: feriado trabalhado por vínculo que folga vai HE100 feriado, não folga trabalhada, contado uma vez; domingo comum fica folga trabalhada. `:230-238` fixa a mesma partição no calculador.
- `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:311-324`: feriado de hoje em 5x2 `False` aparece folga e coincide com juiz.
- `app/ponto/tests/test_e4cal_l1_d4_natureza.py:337-354`: casos 12x36 `False`, 5x2 `False`, 12x36 `True` entram na matriz de natureza/status e feriado nunca diz previsto.

## 13/11 09:14 · sessao 17d0f16548b6 · - · - · origem=Ronald (tmux)

4. **Vincular e alteração de vínculo existente**

- A rota da tela é `vincular/`, servida por `vincular_colaboradores`, que renderiza `colaboradores/vincular.html`; clicar na linha abre o painel carregado por `painel_vinculo`. Fontes: `app/colaboradores/urls.py:9`, `app/colaboradores/views.py:17`, `app/colaboradores/views.py:126`, `app/templates/colaboradores/partials/vinculo_row.html:4`, `app/templates/colaboradores/vincular.html:147`, `app/colaboradores/views.py:262`.
- O formulário de escala é HTML escrito no partial, com `q`, `template_id`, `data_ancora`, `confirm_txt`, `data_inicio` e botão `modo=direto`; os valores iniciais de template, âncora e início vêm do vínculo ativo. **Não há campo `trabalha_em_feriado` nem inicial visual desse campo nesse formulário**. Fontes: `app/templates/colaboradores/partials/painel_vinculo.html:136`, `:149`, `:155`, `:159`, `:163`, `:167`, `:175` (bloco completo até `:180`).
- O POST vai para `vincular_escala`, na rota `<cid>/vincular/escala/`; exige POST/permissão `editar_escalas`, chama `julgar_vinculo(colaborador, request.POST, perfil)` e então `executar_vinculo(...)`, salvo `no_op`. Fontes: `app/colaboradores/urls.py:13`, `app/colaboradores/views.py:373`, `:382`, `:411`, `:416`, `:423`.
- O parser lê template/início/âncora/modo e devolve o veredito sem parâmetro `trabalha_em_feriado`; sua idempotência compara template/início/âncora, não a flag. Fontes: `app/colaboradores/services/vinculo.py:52`, `:53`, `:54`, `:57`, `:59`, `:118`, `:144`.
- Criar pelo executor passa explicitamente `padrao_trabalha_em_feriado(template, colaborador.posto)`; reutilizar vínculo existente troca template/posto/vigência/âncora e faz `save()` sem recalcular a flag. Fontes: `app/colaboradores/services/vinculo.py:259`, `:274`, `:283`, `:287`, `:296`.
- A porta específica de alteração é `marcar_trabalha_em_feriado(ec, desejado, autor=None, motivo='')`: mesmo valor retorna sem escrita; diferente atribui e salva apenas a flag. O único caller de produção encontrado pelo nome é `sincronizar_feriado_admin`, que calcula pelo cargo e chama sem autor/motivo. Fontes: `app/colaboradores/services/vinculo.py:868`, `:869`, `:890`, `:893`, `:894`; `app/colaboradores/management/commands/sincronizar_feriado_admin.py:47`, `:54`, `:56`.
- A alteração alcança passado/hoje desde `max(data_inicio, início da competência anterior)`, pela porta `ponto.portas.celula.regenerar_celulas_vinculo`, com horizonte automático desativado. Depois chama `regenerar_futuras_do_cadastro`. Fontes: `app/colaboradores/services/vinculo.py:895`, `:899`, `:900`, `:904`, `:906`, `:909`, `:910`.
- O suplemento futuro seleciona **apenas células existentes, `origem='gerada'`, de amanhã até o fim da janela corrente**; por se tratar de atributo e não troca de linha do tempo, preserva dias cuja juíza aponta outro vínculo. Cada dia passa pela mesma porta de regeneração. Fontes: `app/ponto/services/cadastro_futuro.py:9`, `:26`, `:27`, `:31`, `:37`, `:40`, `:44`; horizonte é união das janelas correntes das empresas ativas em `app/ponto/management/commands/gerar_celulas.py:290`, `:299`, `:300`.
- A flag **não recebe evento próprio de auditoria antes/depois nessa função**. O antes/depois vai no texto de `motivo` entregue à regeneração; havendo mudança de célula (`if n`), esta registra evento `regenerar_celulas_vinculo`, modelo `EscalaColaborador`, objeto vínculo, com janela/quantidades/barrados no `valor_depois`. Fontes: corpo completo `app/colaboradores/services/vinculo.py:890` até `:914`, especialmente `:907`, `:912`; `app/ponto/portas/celula.py:369`, `:372`, `:373`, `:375`, `:380`.
- A fotografia geral usada pela trilha do executor também não contém a flag: campos da fotografia terminam em `folga_calendario`. Fontes: `app/colaboradores/services/vinculo.py:28`, `:33`, `:42`, `:43`.
- A regeneração não reescreve célula humana `origem='editada'` e barra dias em competência lavrada; alterações de células geradas guardam `dna_anterior`, `regenerada_em`, `regeneracoes`. Fontes: `app/ponto/portas/celula.py:118`, `:122`, `:284`, `:297`, `:305`, `:310`.
- Depois de mudar o chão, rejulga dias tocados até hoje com `julgar_celula(..., apply_=True, forcar=True)`, preservando vereditos humanos; recalcula uma vez por competência e agenda rejunção/falta espúria após commit. Fontes: `app/ponto/portas/celula.py:392`, `:460`, `:465`, `:469`, `:472`, `:483`, `:494`, `:496`, `:528`, `:538`, `:540`.
- Testes callers da porta da flag: `app/escala/tests/test_porta_escala.py:62`, `:65`; `app/chamados/tests/test_jv1_feriado_vira_vigia.py:89`; `app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:133`; teste sem banco dedicado em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:445`.

5. **Célula, DNA, precedência e rejulgamento**

- `gerar_celulas` reexporta as operações da porta da célula; a geração calcula `ec.eh_dia_trabalho_calculado(d)`, grava o resultado em `CelulaDia.trabalha` e fornece esse resultado a `montar_dna`. Fontes: `app/ponto/management/commands/gerar_celulas.py:13`; `app/ponto/portas/celula.py:863`, `:911`, `:914`, `:916`, `:917`.
- `montar_dna` congela vínculo, tipo, ciclo, minutos e marcos/lâmpadas; marcos/lâmpadas só são montados quando `trabalha is True`. **A flag `trabalha_em_feriado` não integra o blob**: entra indiretamente no resultado `trabalha` e, por ele, na presença de marcos. Fontes: `app/ponto/management/commands/gerar_celulas.py:73`, `:88`, `:90`, `:103`, `:104` até `:113`.
- `eh_dia_trabalho_calculado` não lê célula; calcula a base pela fase/foto/template e, se a base é `True`, chama `feriado_vira_folga` para devolver `False`. Os retornos diretos com âncora de `12x36`/`24x48` ficam antes desse trecho. Fontes: `app/escala/models.py:1252`, `:1255`, `:1289`, `:1290`, `:1293`, `:1366`, `:1369`.
- `CICLOS_FERIADO_VIRA_FOLGA` contém **6x1, 5x2 e semanal**. `feriado_vira_folga` exige flag falsa, ciclo nessa tupla e feriado reconhecido pelo juiz com o lugar do vínculo. Portanto `12x36`, `24x48`, intermitente e personalizado não viram folga por essa flag. Fontes: `app/escala/models.py:32`, `:1389`, `:1390`, `:1392`, `:1394`, `:1395`.
- O leitor usual `eh_dia_trabalho` devolve a célula existente; só sem ela chama o calculado. Fontes: `app/escala/models.py:1069`, `:1092`, `:1094`, `:1096`.
- `fatos_do_dia` delega ao período de um dia. O período carrega células/vínculos/feriados, escolhe o vínculo com `vinculo_do_dia`, usa `cel.trabalha` como `previsto` se preenchido e só recorre ao cálculo sem esse valor. Fontes: `app/ponto/precedencia.py:242`, `:256`, `:303`, `:308`, `:312`, `:331`, `:347`, `:348`, `:349`, `:357`, `:363`, `:366`.
- Para decidir `feriado_so_no_pagamento`, o juiz usa **feriado + previsto True + vínculo existente + `not ec.feriado_vira_folga(data)`**; a regra vigente já usa o predicado do gerador, não a flag isoladamente. Se essa condição é falsa, o fato `feriado` participa da precedência. Fontes: `app/ponto/precedencia.py:369`, `:376`, `:377`, `:382`.
- A porta unitária do cartório para 02/11 é `ponto.services.cartorio.julgar_celula(colaborador, data, apply_=True, forcar=True)`: carrega a célula existente e a entrega a `julgar_colab`; futura ou inexistente retorna `None`. **Rejulgar não regenera `trabalha`/DNA**. Fontes: `app/ponto/services/cartorio.py:2011`, `:2025`, `:2027`, `:2028`, `:2040`.
- O cartório lê célula/DNA para julgar e depois carimba a precedência via `fatos_do_dia`; logo a resposta a “célula ou vínculo?” é **ambos**: previsto/DNA da célula, predicado de feriado do vínculo escolhido para o dia. Fontes: `app/ponto/services/cartorio.py:1398`, `:1437`, `:1465`, `:1986`, `:1998`, `:1999`, `:2048`; `app/ponto/precedencia.py:349`, `:363`, `:376`.
- A geração rotineira preserva `trabalha` de células passadas já existentes; correção do cadastro usa a operação explícita de regeneração, que recalcula `trabalha`/DNA e depois rejulga. Fontes: `app/ponto/portas/celula.py:289`, `:290`, `:306`, `:392`, `:472`, `:921`, `:925`.
- O resultado real de col148 em 02/11 e sua mudança após eventual cura não foi executado nem verificado nesta cópia; o dado recebido é a única evidência de produção disponível. [não verificado]

_4539 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
