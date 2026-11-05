# CODEX-ESTADO

> Ultima resposta processada: **05/11/2026 15:45** · publicado em **05/11/2026 15:45** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 05/11 15:45 · sessao 9af039a3dca4 · - · - · origem=Ronald (tmux)

Censo de `app/chamados`, excluindo `models.py`, sem executar banco nem escrever arquivos. **Certeza alta para os sítios abaixo; nenhum chama `pode_executar_csf` diretamente.** As guardas transitivas do model precisam compor a conclusão do pai.

| Entrada | Fechamento |
|---|---|
| Disputa fechada → chamado-pai | `app/chamados/signals.py:42` |
| Reconciliador: premissa morta | `app/chamados/reconciliador.py:1395` → `_retratar`, `:523` |
| Reconciliador: célula absolveu | `app/chamados/reconciliador.py:972` → `_retratar`, `:523` |
| Fecho forçado com retenção | `app/chamados/reconciliador.py:496` |
| Agrupamento de filhos sob adesão | `app/chamados/services/adesao.py:115` |
| Pai adesão após batida | `app/chamados/services/adesao.py:150` |
| Feriado/folga, evento ou comando | `app/chamados/services/feriado_reavaliar.py:47`; evento chama em `:83` |
| Aceitar sugestão de folga, demais chamados do template | `app/chamados/services/limbo_folgas.py:101` |
| Agrupar furo sob LIMBO | `app/chamados/services/limbo_folgas.py:243`, repetição após rollback em `:246` |
| Ausência aprovada | `app/chamados/services/silencio_ausencia.py:101` |
| Ausência declarada já decidida, chamado-destino | `app/chamados/services/materializacao.py:561` |
| Materialização, destino e clones | `app/chamados/services/materializacao.py:738` |
| Regularização externa materializada | `app/chamados/services/regularizacao_ext.py:205` |
| Competência trancada | `app/chamados/services/competencia_trancada.py:184` |
| Auditoria de órfãos `--fechar` | `app/chamados/management/commands/auditar_chamados_orfaos.py:67` |
| Retratar por lei que cala `--apply` | `app/chamados/management/commands/retratar_chamados_lei_cala.py:59` → `_retratar`, `app/chamados/reconciliador.py:523` |
| Aprovar exceção | `app/chamados/services/acoes_chamado.py:70` |
| Aceitar/recusar agenda | `app/chamados/services/acoes_chamado.py:108` |
| Aceitar/recusar escala | `app/chamados/services/acoes_chamado.py:149` |
| Autorizar geofence | `app/chamados/services/acoes_chamado.py:222` |
| Recusar geofence | `app/chamados/services/acoes_chamado.py:277` |
| Resposta livre/status no detalhe | `app/chamados/services/acoes_chamado.py:315` |
| Resposta rápida/status no fio | `app/chamados/services/fio.py:717` |

Efeitos e contrapesos, com certeza alta:

- O detalhe permite mudar status sem conteúdo: lê texto em `app/chamados/services/acoes_chamado.py:286`, só grava nota dentro de `if conteudo` em `:289`, mas transiciona independentemente em `:313`. **Contraexemplo concreto construível:** chamado de cobrança com mensagem anterior do colaborador, gestor envia `novo_status=resolvido` e `conteudo=''`, desde que os gates do model aceitem. A conversa pode terminar sem nova resposta textual do DP; ocorrência efetiva em produção **[não verificado]**.
- O fio não serve como exemplo de ausência de resposta: exige `conteudo` em `app/chamados/services/fio.py:705` e grava `mensagem_dp` em `:714` antes de transicionar em `:717`.
- `_retratar` protege especificamente irmãs respondidas sem consumo: `app/chamados/reconciliador.py:518` → retenção definida em `:446`; depois reconcilia em `:523`, resolve perguntas em `:536` e grava nota `retratado` em `:540`. **Contraexemplo à generalização “sem CSF enterra toda resposta”:** a retenção devolve `False` em `:521` quando há irmã respondida sem consumo.
- `encerrar_com_retencao` usa a mesma retenção antes de `encerrar`: `app/chamados/reconciliador.py:492`.
- Materialização também protege irmãs respondidas sem consumo em `app/chamados/services/materializacao.py:729`, recusando em `:737`.
- Competência trancada preserva o fio consultável segundo a nota escrita em `app/chamados/services/competencia_trancada.py:187`, e respostas que saíram sem veredito são encaminhadas à pauta em `:223`. **Contraexemplo à afirmação de desaparecimento absoluto da conversa:** o caminho declara preservação e pauta; confirmação da visualização concreta em produção **[não verificado]**.
- Ausência ainda aberta não é o caminho de fechamento em `materializacao`: a condição exige status fora de `ABERTAS` em `app/chamados/services/materializacao.py:560`. O ramo aberto silencia em `app/chamados/services/silencio_ausencia.py:86`.

Contraprovas existentes no código de teste, não executadas: [nao verificado]

- Pergunta viva impede reconciliar, cancelar e encerrar: `app/chamados/tests/test_o261e_f1_porta_encerramento.py:86`, `:104`, `:110`.
- Viva em competência entregue não impede reconciliar: `app/chamados/tests/test_o261e_f1_porta_encerramento.py:117`.
- Reatar viva ao sobrevivente permite fechar: `app/chamados/tests/test_o261e_f1_porta_encerramento.py:137`, com destino conferido em `:139`.
- `reconciliaria()` é o instrumento real sem escrita do gate de reconciliação: `app/chamados/models.py:623`; chama `_decisao_do_fechamento(registrar=False)` em `:627`. Ele mede o estado atual, não identifica retrospectivamente o chamador histórico.

## 05/11 15:45 · sessao 9107325fdd0a · CRIVO-AINDA-VALE · censo · origem=Code

Foram selecionados **36 trechos**, priorizando cálculo, descontos, lavratura e exportação. As referências abaixo são relativas a `app/`. Incidência em produção, valores efetivamente pagos e implantação desta cópia: **[não verificado]**. Os contraexemplos são entradas construídas, não registros do anexo.

A certeza indicada refere-se ao comportamento local do código. As notas de revogação do próprio índice foram consideradas: ele distingue texto original de texto posteriormente superado. (`docs/LEIS.md:24`)

**01 — Tolerância de 20 minutos sem HE.** A lápide promete 10 minutos por marcação e 20 por dia sem atraso nem HE; o cálculo de HE elimina somente excedentes até **10 minutos**. **CONTRADIZ.** Contraexemplo: seis minutos antes e seis depois, previsto de 720, trabalho de 732, janela inativa, sem feriado e sem intervalo declarado: surgem **12 minutos de HE50**. (`ponto/motor_calculo_v2.py:53`, `ponto/motor_calculo_v2.py:56`, `ponto/motor_calculo_v2.py:2840`, `ponto/motor_calculo_v2.py:2845`, `ponto/motor_calculo_v2.py:2851`)

**02 — Entrada três horas depois não seria atraso.** O comentário conserva essa exclusão geral. O código a restringe ao dia sem marcos de intervalo; com intervalo declarado, mantém o marco e deixa a pontualidade seguir pela L-084. **NAO VALE MAIS como regra geral.** A substituição está explicitada no próprio ramo. (`ponto/motor_calculo_v2.py:2104`, `ponto/motor_calculo_v2.py:2122`, `ponto/motor_calculo_v2.py:2148`, `ponto/motor_calculo_v2.py:2164`)

**03 — Feriado só dobraria em dia não previsto.** O comentário mantém essa condição, mas o helper exige apenas política de dobra e feriado; a duração inteira recebe HE100. **NAO VALE MAIS.** A correção está documentada no helper: feriado previsto pode dobrar conforme o ciclo. (`ponto/motor_calculo_v2.py:2807`, `ponto/motor_calculo_v2.py:917`, `ponto/motor_calculo_v2.py:934`, `ponto/motor_calculo_v2.py:2809`)

**04 — Literal 240: acima de quatro horas exige intrajornada.** O ramo abre acima de 240, mas só calcula o suprimido quando a duração líquida ultrapassa **360**. **CONTRADIZ quanto à indenização da pausa faltante.** Contraexemplo: jornada comercial de 300 minutos sem intervalo retorna zero de intra indenizada, embora a tabela consultada pelo motor estabeleça piso de 15 minutos para essa duração. (`ponto/motor_calculo_v2.py:3226`, `ponto/motor_calculo_v2.py:3232`, `ponto/motor_calculo_v2.py:3242`, `escala/models.py:19`)

**05 — Literal 480 no regime de minutos legais.** O texto afirma limitar a jornada para HE a 480, preservando jornadas menores. O código aplica `min(limite_dia, 480)` quando `regua_excedente == 'legais'`, antes do tratamento do intervalo suprimido. **AINDA VALE nesse estágio do cálculo.** (`ponto/motor_calculo_v2.py:3120`, `ponto/motor_calculo_v2.py:3122`, `ponto/motor_calculo_v2.py:3129`)

**06 — Literal 720 como jornada padrão.** Na base do motor, ausência de tipo de escala atribui 720 ao atributo contratual. Esse atributo só substitui o previsto diário quando o juiz devolve `None`; previsto numérico, inclusive zero, prevalece. **AINDA VALE como padrão da base e recurso sem previsto.** (`ponto/motor_calculo_v2.py:809`, `ponto/motor_calculo_v2.py:810`, `ponto/motor_calculo_v2.py:1273`)

**07 — Literal 720 como sentinela no plantão.** O plantão troca jornada vazia ou igual a 720 por `8 * 60`. **AINDA VALE para o atributo.** Isso não estabelece um teto universal de HE de 480: o cálculo delegado consulta o previsto diário, que prevalece quando disponível. (`ponto/motor_calculo_v2.py:3499`, `ponto/motor_calculo_v2.py:3503`, `ponto/motor_calculo_v2.py:3511`, `ponto/motor_calculo_v2.py:2835`, `ponto/motor_calculo_v2.py:1273`)

**08 — Não herdar fase do template.** A lápide exige âncora declarada do colaborador. O código usa essa âncora; sem ela, instala sentinela e não confirma o uso da fase. **AINDA VALE neste cálculo.** (`ponto/motor_calculo_v2.py:2316`, `ponto/motor_calculo_v2.py:2312`, `ponto/motor_calculo_v2.py:2321`, `ponto/motor_calculo_v2.py:2369`)

**09 — L-093 usa trabalho real anterior à janela.** O comentário afirma que minutos bloqueados como HE continuam sendo trabalho para o teto de pontualidade. O código soma `minutos_trabalhados` e `minutos_fora_janela` antes de calcular o teto. **AINDA VALE.** (`ponto/motor_calculo_v2.py:1177`, `ponto/motor_calculo_v2.py:1183`, `ponto/motor_calculo_v2.py:1186`)

**10 — L-084 exige as duas pontas distantes.** O comentário exige ambas acima de três horas. O helper usa duas comparações estritas `>180`, ligadas por `and`; quando verdadeiro, a tolerância retorna atraso e antecipada zerados. **AINDA VALE.** (`ponto/motor_calculo_v2.py:1977`, `ponto/motor_calculo_v2.py:1947`, `ponto/motor_calculo_v2.py:1949`, `ponto/motor_calculo_v2.py:1988`, `ponto/motor_calculo_v2.py:2005`)

**11 — Deduplicação de dez minutos no núcleo.** A narrativa da alimentação ainda inclui dedup de dez minutos entre as operações do núcleo. O núcleo registra sua retirada, ordena os instantes e percorre todas as batidas. **NAO VALE MAIS.** Duas batidas separadas por cinco minutos podem formar um par de cinco minutos. (`ponto/calculador/alimentacao.py:332`, `ponto/calculador/alimentacao.py:333`, `ponto/calculador/nucleo.py:38`, `ponto/calculador/nucleo.py:109`, `ponto/calculador/nucleo.py:186`, `ponto/calculador/nucleo.py:246`)

**12 — Lápide sobre tolerância escrita como cinco, executada como dez.** O aviso do calculador continua correto: a docstring do motor diz cinco minutos, enquanto as duas comparações usam a constante de dez. **AINDA VALE a advertência.** (`ponto/calculador/periodo.py:153`, `ponto/motor_calculo_v2.py:1952`, `ponto/motor_calculo_v2.py:60`, `ponto/motor_calculo_v2.py:2006`, `ponto/motor_calculo_v2.py:2008`)

**13 — Noturno depende das duas entradas da CCT.** O comentário recusa transformar ausência de parâmetros em `False`. O código exige ambas as flags diferentes de `None`; sem elas, declara falta de entrada e não executa o cálculo noturno. **AINDA VALE.** (`ponto/calculador/regras.py:607`, `ponto/calculador/regras.py:613`, `ponto/calculador/regras.py:615`, `ponto/calculador/regras.py:844`)

**14 — Feriado convertido em folga não acumula rubrica de folga.** Com `folga_e_feriado` e minutos com escala certa, o calculador escreve HE100 e seu subconjunto feriado. A rubrica de folga fica no `elif`. **AINDA VALE com essa condição explícita.** (`ponto/calculador/regras.py:680`, `ponto/calculador/regras.py:684`, `ponto/calculador/regras.py:686`, `ponto/calculador/regras.py:689`)

**15 — Hora sem escala certa não desaparece.** O comentário preserva a hora trabalhada e retira o adicional de folga. O helper devolve esses minutos na parcela sem escala certa, posteriormente gravada em `horas_trabalhadas`. **AINDA VALE.** (`ponto/calculador/regras.py:692`, `ponto/calculador/regras.py:399`, `ponto/calculador/regras.py:694`)

**16 — Docstring de teste descreve perdão pela proximidade da volta.** O texto com “SEMPRE” ainda descreve entrada próxima de `hfi` e distante de `hi`. O código passou a consultar qual marco a batida ocupa e sua conformidade; a substituição da proximidade está documentada. **NAO VALE MAIS para a entrada.** A saída ainda conserva a condição de proximidade. (`ponto/tests/test_calculador_portoes_de_marco.py:4`, `ponto/tests/test_calculador_portoes_de_marco.py:8`, `ponto/calculador/regras.py:251`, `escala/regua_defesa.py:472`, `escala/regua_defesa.py:492`, `escala/regua_defesa.py:495`, `ponto/calculador/regras.py:259`)

**17 — Meio do vão DENTRO do envelope preserva o turno.** O teste afirma essa proteção contra o corte do núcleo. O código testa inclusão do ponto médio e continua acumulando o turno antes das regras de corte. **AINDA VALE.** O teste contém a expectativa de teto de 729 minutos. (`ponto/tests/test_calculador_regras.py:241`, `ponto/calculador/nucleo.py:183`, `ponto/calculador/nucleo.py:189`, `ponto/tests/test_calculador_regras.py:279`)

**18 — Help de falta inteira fixada em 720.** O campo ainda informa 720 minutos para dia inteiro. Ao aprovar falta sem minutos preenchidos, `save()` soma o previsto de cada dia, sem fixar 720. **NAO VALE MAIS como descrição da derivação automática.** Um único dia previsto em 480 gera 480; minutos positivos já preenchidos não são substituídos. (`ponto/models.py:757`, `ponto/models.py:874`, `ponto/models.py:875`, `ponto/models.py:883`, `ponto/models.py:885`)

**19 — Help `invalidada_em=NULL` significa TXT vigente.** A vigência consulta o último registro por `gerado_em` e `pk`, sem filtrar invalidação. **CONTRADIZ.** Contraexemplo: dois TXT da mesma empresa e competência, ambos com invalidação nula; o antigo tem `e_vigente=False`. (`folha/models.py:97`, `folha/models.py:134`, `folha/models.py:137`)

**20 — Competência paga ficaria sem recalcular para sempre.** O comentário da porta afirma congelamento geral. A função de fechamento documenta a remoção dessa proibição e mantém guarda específica para setembro/2026. **NAO VALE MAIS como proibição geral.** Outubro/2026 exportada, com entradas elegíveis, pode alcançar a chamada do motor; a exportação isoladamente não barra esse caminho. (`folha/porta_export.py:417`, `ponto/services/fechamento.py:476`, `ponto/services/fechamento.py:36`, `ponto/services/fechamento.py:551`, `ponto/services/fechamento.py:745`, `ponto/services/fechamento.py:277`)

**21 — Crédito parcial abate atraso no próprio dia.** O escritor busca crédito pela mesma data, limita o abatimento ao atraso e impede resultado negativo. **AINDA VALE.** (`ponto/services/dia_pago.py:188`, `ponto/services/dia_pago.py:190`, `ponto/services/dia_pago.py:193`)

**22 — Folga transporta hora, noturno e intra, sem duplicar HE ou descontos.** Os laços destinam a hora à rubrica de folga ou às trabalhadas, conforme escala certa, e acrescentam somente noturno e intra. Não transportam atraso, antecipada ou HE dos períodos de folga. **AINDA VALE nesses laços.** (`ponto/services/dia_pago.py:197`, `ponto/services/dia_pago.py:207`, `ponto/services/dia_pago.py:223`, `ponto/services/dia_pago.py:234`, `ponto/services/dia_pago.py:257`)

**23 — Folga em feriado vai para HE100 feriado.** O laço escreve HE100 total e o subconjunto feriado, preservando noturno e intra. **AINDA VALE.** O emissor subtrai o subconjunto para obter HE100 normal, evitando tratá-los como duas parcelas independentes. (`ponto/services/dia_pago.py:241`, `ponto/services/dia_pago.py:249`, `ponto/services/dia_pago.py:251`, `folha/export.py:409`, `folha/export.py:410`)

**24 — HE noturna não gera evento adicional próprio.** O comentário trata os splits noturnos como subconjuntos. A lista emitida usa HE50, HE100 normal, feriado e adicional noturno, sem emitir os campos de HE noturna separadamente. **AINDA VALE no emissor.** A evidência histórica de holerites citada pelo comentário permanece **[não verificado]**. (`folha/export.py:413`, `folha/export.py:423`, `folha/export.py:428`)

**25 — Teste: SO pausa faltante não retém; dúvida não isenta.** A exceção exige resposta legível do juiz, alguma lâmpada apagada, todas de pausa, e protestos exatamente iguais a `{'FURO_PARCIAL'}`. O retentor consulta essa condição. **AINDA VALE para a decisão de retenção.** O estado posterior da pergunta em produção é **[não verificado]**. (`folha/tests/test_furo_so_intervalo_nao_retem.py:1`, `folha/tests/test_furo_so_intervalo_nao_retem.py:27`, `folha/export.py:1124`, `folha/export.py:1129`, `folha/export.py:1147`, `folha/export.py:1235`)

**26 — L-031: flag decide indenização do intervalo não batido.** A cláusula original atribui essa decisão a `intervalo_indenizavel`. A própria linha registra seu adiamento; o motor declara expressamente que não lê a flag e mantém a indenização do suprimido. **NAO VALE MAIS como regra operacional atual; a suspensão já está anotada.** (`docs/LEIS.md:79`, `ponto/motor_calculo_v2.py:822`, `ponto/motor_calculo_v2.py:823`, `ponto/motor_calculo_v2.py:2773`, `ponto/motor_calculo_v2.py:2784`)

**27 — L-032: intervalo cadastrado sempre sai da jornada.** O motor desconta dos minutos trabalhados a pausa efetivamente reconhecida; pausa ausente gera suprimido. O intervalo declarado também corrige o limite de HE, mas isso não equivale a subtraí-lo das horas trabalhadas. **CONTRADIZ o “sempre”.** Contraexemplo: 720 minutos sem pausa reconhecida permanecem 720 trabalhados, com intra indenizada separada. (`docs/LEIS.md:80`, `ponto/motor_calculo_v2.py:1910`, `ponto/motor_calculo_v2.py:2771`, `ponto/motor_calculo_v2.py:2774`, `ponto/motor_calculo_v2.py:2784`, `ponto/motor_calculo_v2.py:2785`, `ponto/motor_calculo_v2.py:1803`)

**28 — L-033: coluna DONO ainda diz “NENHUM” e retenção de todo furo.** Essa descrição ficou antiga: existe exceção específica da pausa no retentor. **NAO VALE MAIS essa coluna.** A regra de não reter já funciona, e o estado da própria linha também reconhece a mudança. (`docs/LEIS.md:81`, `folha/export.py:1147`, `folha/export.py:1235`)

**29 — L-035: exclusão pós-05h somente com CLT declarada.** Com o switch desligado e empresa não CLT, a fábrica pode deixar o parâmetro ausente. O construtor resolve `None` somente pelo ciclo, tornando-o `False` para 12x36. **CONTRADIZ o “somente”.** Contraexemplo: empresa `cct`, switch desligado, ciclo 12x36 e parâmetro omitido recebe janela sem prorrogação pós-05h. (`docs/LEIS.md:83`, `core/regua_cct.py:449`, `core/regua_cct.py:453`, `core/regua_cct.py:457`, `ponto/motor_calculo_v2.py:758`, `core/regua_cct.py:182`)

**30 — L-036: `HORA_REDUZIDA_12X36_EM_SECO=False` em produção.** A constante é `False` nesta cópia; a fábrica condiciona o afastamento da redução ao cadastro e ao ciclo, e o motor honra a flag recebida. A parte “em produção” é **NAO SE SABE**: faltam identificação da implantação e parâmetros efetivos. (`docs/LEIS.md:84`, `core/regua_cct.py:150`, `core/regua_cct.py:470`, `ponto/motor_calculo_v2.py:992`; implantação e cadastro: **[não verificado]**)

**31 — L-086: feriado previsto nunca seria HE100.** A própria linha declara revogada a cláusula de feriado. O helper atual permite dobra conforme política do ciclo, sem consultar previsão de trabalho. **NAO VALE MAIS a cláusula original de feriado; a ressalva do índice está correta.** (`docs/LEIS.md:118`, `ponto/motor_calculo_v2.py:917`, `ponto/motor_calculo_v2.py:934`)

**32 — L-087: tipo gravado nunca decide.** Há retorno ao pareamento anterior sem célula alimentada, e o pareador usa o tipo gravado quando a ata não responde. **CONTRADIZ o “nunca”.** Contraexemplo: sem alimentação da ata, o papel de uma batida pode vir de seu campo `tipo`; trocar esse campo pode alterar o pareamento. A cobertura limitada também está declarada no índice. (`docs/LEIS.md:119`, `docs/LEIS.md:172`, `ponto/motor_calculo_v2.py:573`, `ponto/motor_calculo_v2.py:576`, `ponto/turnos.py:1380`)

**33 — L-091: rótulo não manda no cálculo.** A fábrica continua despachando pelo `tipo_base`, com correção geométrica específica para certos rótulos de turno partido. **CONTRADIZ a exclusão geral do rótulo.** Contraexemplo: base `comercial` com vão de quatro horas continua selecionando `MotorComercial`; esse ramo não é convertido pela geometria. (`docs/LEIS.md:124`, `ponto/motor_calculo_v2.py:3572`, `ponto/motor_calculo_v2.py:3635`, `ponto/motor_calculo_v2.py:3668`)

**34 — L-095: mudança de motor preserva passado em nova versão.** O fechamento escreve novamente `versao='motor'`; o escritor apaga as linhas dessa versão e recria. **CONTRADIZ a preservação prometida.** Contraexemplo: duas lavraturas da mesma competência e versão, com resultados diferentes, substituem as linhas anteriores. O `versao_motor` atual carimba parâmetros da régua; não impede essa substituição. (`docs/LEIS.md:113`, `ponto/services/fechamento.py:1211`, `ponto/services/dia_pago.py:357`, `ponto/services/dia_pago.py:358`, `core/regua_cct.py:421`)

**35 — Nota da L-095: ninguém preenche `versao_motor`.** O fechamento já passa o carimbo e o mapa por dia; o escritor os utiliza nas novas linhas. **NAO VALE MAIS essa afirmação.** A outra parte da nota — apagar e recriar — continua correta. (`docs/LEIS.md:170`, `ponto/services/fechamento.py:1214`, `ponto/services/dia_pago.py:345`, `ponto/services/dia_pago.py:357`)

**36 — L-093: soma nunca ultrapassa o teto aritmético.** O código arredonda atraso e antecipada separadamente, sem ajustar o resíduo. **CONTRADIZ o absoluto “nunca”.** Contraexemplo local: previsto 660, trabalho de 399min04s e descontos brutos de 150+150 produzem teto de **260,933333 minutos**, mas resultado de **130,47+130,47=260,94**, excedendo-o em **0,4 segundo**. Ocorrência e efeito financeiro em produção: **[não verificado]**. (`docs/LEIS.md:111`, `ponto/motor_calculo_v2.py:1188`, `ponto/motor_calculo_v2.py:1191`, `ponto/motor_calculo_v2.py:1194`, `ponto/motor_calculo_v2.py:1196`)

| item | tipo a-e | texto:linha | codigo:linha | VEREDITO | certeza |
|---|---|---|---|---|---|
| 01 | a | ponto/motor_calculo_v2.py:54 | ponto/motor_calculo_v2.py:2845 | CONTRADIZ | Alta |
| 02 | a | ponto/motor_calculo_v2.py:2104 | ponto/motor_calculo_v2.py:2122; :2148 | NAO VALE MAIS | Alta |
| 03 | a | ponto/motor_calculo_v2.py:2807 | ponto/motor_calculo_v2.py:934; :2809 | NAO VALE MAIS | Alta |
| 04 | c | ponto/motor_calculo_v2.py:3226 | ponto/motor_calculo_v2.py:3232; escala/models.py:19 | CONTRADIZ | Alta |
| 05 | c | ponto/motor_calculo_v2.py:3121 | ponto/motor_calculo_v2.py:3122 | AINDA VALE | Alta |
| 06 | c | ponto/motor_calculo_v2.py:810 | ponto/motor_calculo_v2.py:810; :1273 | AINDA VALE | Alta |
| 07 | c | ponto/motor_calculo_v2.py:3503 | ponto/motor_calculo_v2.py:3504; :1273 | AINDA VALE | Alta |
| 08 | a | ponto/motor_calculo_v2.py:2316 | ponto/motor_calculo_v2.py:2312; :2369 | AINDA VALE | Alta |
| 09 | a | ponto/motor_calculo_v2.py:1177 | ponto/motor_calculo_v2.py:1186 | AINDA VALE | Alta |
| 10 | a | ponto/motor_calculo_v2.py:1977 | ponto/motor_calculo_v2.py:1947; :1949 | AINDA VALE | Alta |
| 11 | a | ponto/calculador/alimentacao.py:333 | ponto/calculador/nucleo.py:38; :186 | NAO VALE MAIS | Alta |
| 12 | a | ponto/calculador/periodo.py:153 | ponto/motor_calculo_v2.py:60; :2006 | AINDA VALE | Alta |
| 13 | a | ponto/calculador/regras.py:607 | ponto/calculador/regras.py:613; :844 | AINDA VALE | Alta |
| 14 | a | ponto/calculador/regras.py:680 | ponto/calculador/regras.py:684; :689 | AINDA VALE | Alta |
| 15 | a | ponto/calculador/regras.py:692 | ponto/calculador/regras.py:399; :694 | AINDA VALE | Alta |
| 16 | e | ponto/tests/test_calculador_portoes_de_marco.py:4 | ponto/calculador/regras.py:251; escala/regua_defesa.py:492 | NAO VALE MAIS | Alta |
| 17 | e | ponto/tests/test_calculador_regras.py:241 | ponto/calculador/nucleo.py:183; :189 | AINDA VALE | Alta |
| 18 | d | ponto/models.py:757 | ponto/models.py:875; :883 | NAO VALE MAIS | Alta |
| 19 | d | folha/models.py:97 | folha/models.py:134; :137 | CONTRADIZ | Alta |
| 20 | a | folha/porta_export.py:417 | ponto/services/fechamento.py:551; :745 | NAO VALE MAIS | Alta |
| 21 | a | ponto/services/dia_pago.py:188 | ponto/services/dia_pago.py:190; :193 | AINDA VALE | Alta |
| 22 | a | ponto/services/dia_pago.py:197 | ponto/services/dia_pago.py:223; :234; :257 | AINDA VALE | Alta |
| 23 | a | ponto/services/dia_pago.py:241 | ponto/services/dia_pago.py:249; :251 | AINDA VALE | Alta |
| 24 | a | folha/export.py:413 | folha/export.py:428 | AINDA VALE | Alta |
| 25 | e | folha/tests/test_furo_so_intervalo_nao_retem.py:1 | folha/export.py:1124; :1147; :1235 | AINDA VALE | Alta |
| 26 | b | docs/LEIS.md:79 | ponto/motor_calculo_v2.py:822; :2784 | NAO VALE MAIS | Alta |
| 27 | b | docs/LEIS.md:80 | ponto/motor_calculo_v2.py:2774; :2785 | CONTRADIZ | Alta |
| 28 | b | docs/LEIS.md:81 | folha/export.py:1147; :1235 | NAO VALE MAIS | Alta |
| 29 | b | docs/LEIS.md:83 | core/regua_cct.py:457; ponto/motor_calculo_v2.py:758 | CONTRADIZ | Alta |
| 30 | b | docs/LEIS.md:84 | core/regua_cct.py:150; :470 | NAO SE SABE | Indeterminada em produção |
| 31 | b | docs/LEIS.md:118 | ponto/motor_calculo_v2.py:917; :934 | NAO VALE MAIS | 

[truncado em 20000 de 20550 caracteres]

## 05/11 15:44 · sessao 85a23d8438d7 · - · - · origem=Ronald (tmux)

| Candidata | Alteração e custo mínimo identificável | Contraexemplo / limite | Certeza |
|---|---|---|---|
| **(a) Evento em `recompute_turnos`** | Hoje a função apenas reconstrói as linhas, com `delete` e `bulk_create` no mesmo `atomic`; precisaria registrar o fato nessa transação e ter consumidor que invoque o reconciliador existente. São **dois pontos funcionais**: produtor e consumidor; rebaixar o cron ainda envolve o command e o cadastro de papéis. `app/ponto/nucleo.py:40`, `app/ponto/nucleo.py:57`, `app/chamados/reconciliador.py:1357`, `app/chamados/management/commands/reconciliar_chamados.py:51`, `app/config/crons.py:2018`. O documento da cópia já registra a escolha **“JUIZ-3 (a) outbox na transacao, teto 17->18”**; isso comprova a decisão registrada, não a implementação. `app/docs/PROMPTS.md:1853`. | **Só emitir quando alguém recomputa não cria um gatilho de reload ou virada do dia.** A função precisa ser chamada; nada em seu corpo observa relógio, deploy ou início de processo. `app/ponto/nucleo.py:40`. Emitir incondicionalmente produz eventos também em reconstruções sem alteração lógica: os crons chamam recompute antes de ler, a cada 5/30 minutos. `app/ponto/management/commands/processar_alertas_turno.py:54`, `app/ponto/management/commands/processar_alertas_avancados.py:56`, `app/config/crons.py:362`. Volume resultante: **[nao verificado]**. | **Alta**, pelo código; suficiência para os casos produtivos **[nao verificado]**. |
| **(b) Consumidor O-OUTBOX executando o reconciliador inteiro** | O consumidor atual chama somente a retratação dos chamados absolvidos pela célula e o encerramento de perguntas órfãs pela lâmpada; **não chama `reconciliar_chamados_colab`**. `app/chamados/reconciliador.py:1324`. Integrar o reconciliador completo exige **um ponto funcional no consumidor**, além de garantir produtores para todas as alterações relevantes e adaptar command/cadastro/vigia antes do rebaixamento. `app/chamados/reconciliador.py:1357`, `app/chamados/management/commands/reconciliar_chamados.py:51`, `app/config/crons.py:2018`. | **Sem evento pendente, o loop termina antes de reconciliar qualquer colaborador.** Portanto trocar somente o corpo do consumidor não cobre lote sem batida nem `EventoCelula`. `app/chamados/reconciliador.py:1309`. Também não basta o universo atual de chamados absolvidos: `MODULOS_DE_TURNO` contém `turno_aberto`, `_24h` e entrada bloqueada; não contém `disputa_supervisao/orfao_14h`. `app/chamados/reconciliador.py:336`, `app/chamados/reconciliador.py:964`. | **Alta**. |
| **(c) Juiz de prazo — `JUIZES_DO_PRAZO`** | Os juízes existentes respondem horas abertas, degrau e urgência; `turno_aberto_ha` lê o núcleo materializado. `app/ponto/turnos.py:3115`, `app/ponto/turnos.py:3128`, `app/ponto/turnos.py:3141`. Os emissores já delegam a eles e ao lembrete; portanto a abertura temporal **já tem autoridade declarada**, em dois comandos. `app/config/crons.py:1976`, `app/config/crons.py:2098`, `app/ponto/management/commands/processar_alertas_avancados.py:60`, `app/ponto/management/commands/processar_alertas_turno.py:115`. Reclassificar o reconciliador exigiria demonstrar que **todo** seu trabalho é prazo e declarar corpo/autoridade; ele também reata órfãs e retrata premissas, e o command faz rejunção. `app/chamados/reconciliador.py:1357`, `app/chamados/management/commands/reconciliar_chamados.py:56`, `app/config/crons.py:2046`. Custo completo: **[nao verificado]**. | **Idade não demonstra fim da premissa.** Com entrada e sem saída, `turno_aberto_ha` continua devolvendo horas; 24/72 h aumentam o nível/urgência, sem fechar o turno. `app/ponto/turnos.py:3123`, `app/ponto/turnos.py:3136`, `app/ponto/turnos.py:3143`. A premissa das três classes é `FATO_TURNO_ABERTO`, julgada pelo turno específico do chamado, não pela idade. `app/chamados/catalogo/premissa.py:46`, `app/chamados/reconciliador.py:286`. Fazer vencer por idade precisaria da **lei de extinção temporal dessas classes**, não demonstrada nos anexos nem nesses juízes: **parar nessa decisão**. | **Alta** para a inadequação de prazo como substituto universal. |

Complementos para a consolidação:

- **Reload não apaga o estado persistido desse caminho.** O recompute grava `TurnoMaterializado` no banco, e o reconciliador consulta chamados vivos e turnos novamente. `app/ponto/nucleo.py:58`, `app/chamados/reconciliador.py:95`, `app/chamados/reconciliador.py:1384`. Os caches identificados em `turnos_do_colab` e no fechamento de pausa são **locais à chamada**, não globais de processo. `app/ponto/turnos.py:1848`, `app/ponto/turnos.py:1894`, `app/ponto/turnos.py:1905`, `app/ponto/turnos.py:1152`. **Certeza alta**; causa das rajadas produtivas após reload: **[nao verificado]**.
- O veto posicional **reconsulta vínculo e batidas**; não guarda memória entre processos. `app/ponto/propositor.py:325`, `app/ponto/propositor.py:334`, `app/ponto/propositor.py:346`. **Certeza alta**. Contraexemplo à equivalência “núcleo aberto = premissa viva”: se todos os dias dos turnos abertos estão posicionalmente completos, o coletor retorna vazio. `app/chamados/reconciliador.py:105`.
- O consumidor possui uma exceção temporal já implementada: pergunta órfã retida apenas pelo relógio mantém seu **evento existente** pendente, sem gastar tentativa; isso permite nova passada sem batida nova. `app/chamados/reconciliador.py:1341`. **Certeza alta**. Contraexemplo à generalização: a garantia depende de já haver evento; fila vazia encerra o consumidor. `app/chamados/reconciliador.py:1309`.
- O cron atual continua escrevendo a cada 15 minutos: executa o reconciliador por colaborador, faz rejunção e drena outbox. `app/config/crons.py:395`, `app/chamados/management/commands/reconciliar_chamados.py:51`, `app/chamados/management/commands/reconciliar_chamados.py:60`. **Certeza alta de configuração/código**, execução real em produção **[nao verificado]**.
- A retratação deve continuar pela unidade `_retratar`: ela pode **recusar** quando há retenção, e reúne chamado, justificativa pendente, perguntas e nota no mesmo ato. `app/chamados/reconciliador.py:503`, `app/chamados/reconciliador.py:518`, `app/chamados/reconciliador.py:522`. **Certeza alta**; contraexemplo à promessa “evento necessariamente fecha”: a retenção retorna `False`. `app/chamados/reconciliador.py:519`.

## 05/11 15:44 · sessao 096e5e4d0432 · - · - · origem=Ronald (tmux)

Mapa de mecanismos, sem comprovação de incidência em produção:

- **Nascimento sem pergunta — certeza alta.** `abrir()` grava chamado e devolve sem criar pergunta (`app/chamados/models.py:932-942`). Para `batida_ausente`, nem o outbox explícito desse trecho se aplica: `MODULOS_DE_TURNO` contém apenas `turno_aberto`, `turno_aberto_24h`, `entrada_bloqueada_turno_aberto` (`app/chamados/reconciliador.py:334-337`). O emissor da grade termina após abrir/reabrir chamado, expressamente sem fio (`app/ponto/management/commands/reconciliar_grade.py:507-532`). **Contraexemplo:** `detectar_ausencias` tenta criar o fio depois, mas em bloco best-effort cuja exceção é absorvida; essa falha permite conservar o chamado sem pergunta (`app/ponto/management/commands/detectar_ausencias.py:787-803`).

- **Porta `criar()` sem pergunta — certeza alta.** Faz `objects.create`, trilha e retorno; não emite pergunta nem evento explicitamente (`app/chamados/models.py:1048-1050`). Exemplo de turno concreto: `processar_alertas_avancados` cria `turno_aberto_24h` por essa porta (`app/ponto/management/commands/processar_alertas_avancados.py:121-137`). Chamados de cadastro também nascem por ela, por exemplo `colaborador_sem_posto`; portanto o universo amplo “vivo sem pergunta” não equivale automaticamente a defeito do fio de ponto (`app/ponto/utils.py:40-56`).

- **Reabertura/renascimento sem pergunta restaurável — certeza alta.** O dedup de `abrir()` renasce o encerrado e retorna existente (`app/chamados/models.py:867-885`); o renascimento chama `reabrir()` (`app/chamados/models.py:980-985`). `reabrir()` grava estado vivo e chama exclusivamente `ressuscitar_perguntas()` (`app/chamados/models.py:1379-1392`). A ressuscitação só considera perguntas materializadas e sem texto; pula marco aceso ou pergunta que o C6 ainda considera morta (`app/chamados/models.py:1423-1447`). Pode devolver zero (`app/chamados/models.py:1481-1487`). **Contraexemplo:** havendo candidata elegível, descarimba as perguntas e reabre suas disputas (`app/chamados/models.py:1488-1522`).

- **Encerramento da última resolvedora por duplicata — certeza alta quanto à possibilidade.** `retratar_duplicata()` carimba apenas a pergunta; não fecha o chamado (`app/chamados/services/fio.py:878-895`). A via de identidade torna a pergunta morta no C6 (`app/chamados/juizes.py:1105-1114`). Se a canônica permanece ligada a outro chamado, o antigo pode ficar sem resolvedora viva; distribuição concreta em produção **[não verificado]**. **Contraexemplo:** resposta pendente de decisão pode manter viva até uma pergunta carimbada duplicata, pois a resposta é julgada antes da identidade (`app/chamados/juizes.py:1092-1114`; `app/chamados/models.py:2614-2621`).

- **Validação/materialização de um marco com outro furo residual — certeza alta quanto ao mecanismo.** A validação carimba antes da materialização, no mesmo atomic (`app/chamados/services/validacao.py:393-396`); `validada_em` torna a pergunta morta (`app/chamados/juizes.py:1077-1079`). A materialização tenta reconciliar destino e clones (`app/chamados/services/materializacao.py:728-748`, chamada em `:941`), mas o gate recusa encerrar a cobrança sobrevivente se o dia continua furado para folha (`app/chamados/models.py:701-734`). **Contraexemplo:** dia regularizado ou cobrança transferida ao sobrevivente autoriza fechar (`app/chamados/models.py:701-724`).

- **Vitalidade muda por leitura, sem escrita na pergunta — certeza alta.** Tranca já mata a pergunta por leitura (`app/chamados/juizes.py:1080-1088`); célula julgada que não acusa também pode matá-la (`app/chamados/juizes.py:1122-1138`). **Contraexemplo:** resposta discordante da luz e sem decisão administrativa do dia prevalece sobre absolvição da célula (`app/chamados/juizes.py:1092-1104`); marco pendente pode impedir absolvição (`app/chamados/juizes.py:1127-1138`).

**Evento existente não é evento emissor de pergunta — certeza alta.** O nascimento de chamado de turno por `abrir()` insere `EventoCelula` (`app/chamados/models.py:935-941`; `app/ponto/portas/celula.py:634-639`). O consumidor executa retratação de chamados e encerramento de órfãs, depois marca eventos processados; não chama a fábrica (`app/chamados/reconciliador.py:1322-1346`). A fábrica é chamada no cartório apenas para células julgadas naquela passada (`app/ponto/services/cartorio.py:981-999`). A afirmação documental “nenhum evento emite fio mudo” não é refutada pelo outbox de retratação; além disso, `batida_ausente` está fora do conjunto que gera esse evento no nascimento (`app/chamados/reconciliador.py:334-337`).

## 05/11 15:44 · sessao ea4b95dce481 · - · - · origem=Ronald (tmux)

Evidências para consolidação, todas por leitura estática:

| Classe | Nascimento | Estado escrito/lido | Deploy, virada e lote | [nao verificado]
|---|---|---|---|
| `turno_aberto` | Cron `processar_alertas_turno`, no lembrete `pos_20`: `_marco_critico` chama `_abrir_chamado_critico`, que usa `ChamadoColaborador.abrir`, grava módulo/categoria `turno_aberto`, subcategoria `saida_pendente`, contexto com dia/entrada. `app/ponto/management/commands/processar_alertas_turno.py:304-354`. **Alta.** | Nascimento: motor fornece estado inicial e `get_or_create` persiste: `app/chamados/models.py:894-932`. Transição: `_gravar_estado`, atribuição e save: `app/chamados/models.py:195-279`. Fechadores: cron específico consulta materializado e vivo, testa outros turnos do mesmo dia e chama `retratar`: `app/ponto/management/commands/processar_alertas_turno.py:153-218`; reconciliador geral consulta vivos e `_veredito_premissa`: `app/chamados/reconciliador.py:1384-1396`. **Alta.** | Reload não perde o materializado, que é modelo ORM: `app/ponto/models.py:1196-1220`; não há replay de lembrete no código: só envia se `abs(agora - alvo)<=240s`, `app/ponto/turnos.py:3158-3164`. Cron consulta janela recente e recompõe mesmo sem nova batida, desde que haja batida nas últimas 36h: `app/ponto/management/commands/processar_alertas_turno.py:50-79`. **Alta para código; execução real [nao verificado].** Contraexemplo: indisponibilidade durante toda a janela `pos_20` faz perder esse nascimento, pois `not _na_hora` pula: `:120-122`. |
| `turno_aberto_24h` | `processar_alertas_avancados` recompõe e lê **ontem**, usa `turno_aberto_ha`/`nivel_turno_aberto`; N5 é `horas>=24`: `app/ponto/management/commands/processar_alertas_avancados.py:20-22,48-72`; `app/ponto/turnos.py:3115-3137`. Reaproveita `turno_aberto` vivo do mesmo dia ou 24h não fechado, senão `ChamadoColaborador.criar`: `app/ponto/management/commands/processar_alertas_avancados.py:100-140`. Há emissor retroativo explícito, `--ini/--fim/--apply`, via `abrir`: `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:54-60,88-108,151-169`. **Alta.** | Estado inicial de `criar` é motor + `objects.create`: `app/chamados/models.py:1015-1048`. Reconciliador geral lê a classe: catálogo mapeia ao `FATO_TURNO_ABERTO`, `app/chamados/catalogo/premissa.py:46`; coleta materializado e recorte dia/entrada, `app/chamados/reconciliador.py:117-158,264-289`; fecha pela unidade `_retratar` → `ch.reconciliar` → `_gravar_estado`, `app/chamados/reconciliador.py:503-536`; `app/chamados/models.py:607-620`. **Alta.** | Reload não apaga chamado/materializado ORM. Virada altera automaticamente quem é `ontem`, não o estado do chamado: `app/ponto/management/commands/processar_alertas_avancados.py:20-22,58-59`. Sem batida/evento, cron 30min ainda calcula idade e emite/escala; cadastro cron: `app/config/crons.py:363`. **Alta como inferência.** Contraexemplo: entrada ontem 23h; execução hoje 22h tem 23h, ainda N3; amanhã deixa de ser ontem e pode nunca chegar ao N5 deste emissor. Fundamentação do cenário: `:20-22,58-72`; degrau `app/ponto/turnos.py:3133-3137`. Retroativo é outra porta. |
| `disputa_supervisao/orfao_14h` | API detecta antes da batida e cria/atualiza disputa depois: `app/api/views_core.py:432-433,769-780` e gêmeo `app/api/views.py:445-446,769-780`. Detector exige bloqueio e ausência de regularização pendente: `app/chamados/detector_anomalias.py:31-50`; bloqueio é **14≤h<96**, `app/ponto/services/triagem_batida.py:17-30`. Também pode nascer do julgamento/lote da célula pela fábrica: `app/chamados/services/fabrica_da_celula.py:129-141,174-176`. Pai nasce em `ChamadoColaborador.abrir`, disputa em `objects.create`: `app/chamados/services/disputa_emissao.py:504-528`. **Alta.** | Catálogo faz desse par exato `FATO_TURNO_ABERTO`: `app/chamados/catalogo/premissa.py:49`; mesmo coletor e escritor acima. `DisputaSupervisao.motivos/contexto` são outro estado, agregado em `app/chamados/services/disputa_emissao.py:466-502`, não sinônimo do status do chamado. **Alta.** | Sem batida, detector API não é chamado por esse caminho; fábrica/julgamento pode criar por outro ato. Reconciliador cron ainda lê o chamado vivo sem exigir EventoCelula: `app/chamados/management/commands/reconciliar_chamados.py:36-56`. **Alta para esses caminhos; universo de execução real [nao verificado].** Contraexemplo: disputa já aberta agrega `orfao_14h` sem trocar subcategoria do pai (`disputa_emissao.py:471-502`); pai novo escolhe primeiro motivo **ordenado**, `:595-603`. Logo “disputa contém motivo orfao_14h” não garante que pertença ao par catalogado. |

Pontos transversais:

- `recompute_turnos` usa apenas lista local, reconstrói `TurnoMaterializado` por `DELETE` + `bulk_create` no mesmo atomic; não emite evento nem guarda snapshot/memória global: `app/ponto/nucleo.py:40-61`. **Alta.** Evento baseado em diferença precisaria observar antes/depois e identidade estável; o rebuild troca linhas/PK, `:57-60`. **Alta como inferência de desenho.**
- Materializado guarda geometria, e “vivo agora” é cálculo de leitura, não campo persistido: `app/ponto/models.py:1196-1200`. Virada de relógio não equivale a mudança geométrica. **Alta.**
- Sem batida/EventoCelula, cron `reconciliar_chamados` continua varrendo colaboradores com chamado vivo ou pergunta órfã, executa reconciliação e rejunção: `app/chamados/management/commands/reconciliar_chamados.py:36-56`. Ele percebe premissa morta se o materializado estiver atualizado; materializado stale pode segurá-lo indevidamente, pois coleta dele: `app/chamados/reconciliador.py:95-114,264-289`. **Alta.**
- Batida single-save recompõe antes, depois chama reconciliador: `app/ponto/signals.py:19-36,206-218`. Bulk/raw não disparam esses signals, conforme lapide do próprio módulo: `app/ponto/signals.py:1-5,77-83`. **Alta; comportamento real do lote específico [nao verificado].**
- Fechar não é garantido só porque premissa morreu: unidade retém quando há irmã respondida aguardando validação, `app/chamados/reconciliador.py:510-524`; gate/guarda de pergunta viva ainda pode recusar, `app/chamados/models.py:202-221,607-620`. **Alta.**
- Não confundir `disputa_supervisao_manual/orfao_14h`: agrupador gera esse módulo e IDs de filhos, `app/chamados/services/disputa_emissao.py:943-955`; premissa é **filhos vivos**, não turno aberto: `app/chamados/catalogo/premissa.py:50`. **Alta; contraexemplo essencial.**
- Sem dia apurável, reconciliador volta ao `exists` global, podendo outro turno aberto segurar chamado antigo: `app/chamados/reconciliador.py:173-178,276-289`. **Alta.**
- Os emissores de prazo já delegam: cadastro `processar_alertas_avancados → turno_aberto_ha`, `processar_alertas_turno → lembrete_fim_turno`: `app/config/crons.py:2096-2101`. Isso não transforma “há turno aberto?” em prazo; catálogo atual define a premissa estrutural, `app/chamados/catalogo/premissa.py:46-49`. **Alta.**

## 05/11 15:43 · sessao 42f82f462a30 · - · - · origem=Ronald (tmux)

**C11 — texto existente e limite da prova**

- **Alta:** `CORTES.md:305` chama C11 de condição de saída da exceção; não define seu conteúdo. O selo repete exatamente essa condição em `bin/tests/test_papel_prazo_nao_deriva.sh:302–307`.
- **Alta:** existe texto operacional sobre **“FIO-MUDO = C11 antes”**: medir se as 12 células têm furo; sem furo, cobrança sem premissa morre por lastro; com furo, opção **(i), evento de abertura**. `app/docs/PROMPTS.md:1519`. Isso responde melhor que “C11 não tem texto nenhum”.
- **Alta:** há aval posterior explícito para **JUIZ-4 FIO MUDO (i), pergunta no evento de abertura de `batida_ausente`**. `app/docs/PROMPTS.md:1853`; fila correspondente em `app/docs/BACKLOG.md:1738`. Aval documentado não prova implantação.
- **Alta:** `_C11` do registro é homônimo: pergunta **“a ausência do dia suspende, fecha ou levanta a cobrança viva?”**, autoridade `silencio_ausencia.reconciliar_por_ausencia`. `app/core/juizes.py:117`; `app/core/juizes.py:271`. Não há base para identificá-lo automaticamente com o C11 de CORTES:305.
- **Média:** o próprio relato registra “C11 não tem texto em LEIS/CORTES”, mas isso é uma afirmação documental, não prova de inexistência global. `app/docs/RELATO.md:89`. **Commit que defina o C11 referido: [não verificado]**; a cópia não tem histórico Git acessível.

**Comparação das alternativas**

| Alternativa | Cobre | Deixa de fora / contraexemplo | Certeza |
|---|---|---|---|
| **(i) Evento na abertura de `batida_ausente`** | O nascimento que passe pela abertura instrumentada; corresponde à direção do aval. `app/docs/PROMPTS.md:1519`; `app/docs/PROMPTS.md:1853`. | Chamado já existente cuja última pergunta morreu; chamado reaberto sem novo nascimento. Esses caminhos são nomeados pelo próprio comando, que explica que `reabrir()` não emite por conta própria. `app/chamados/management/commands/disparar_perguntas_competencia.py:115–132`. Contraexemplo: pergunta morre por duplicata depois da abertura; nenhum novo evento de abertura ocorre. | Alta para limite causal; implantação [não verificado]. |
| **(ii) Preservar o ramo atual como emissor com papel próprio** | Pode procurar estoque existente e chamados reabertos dentro da janela, desde que estejam vivos e ligados a célula. `app/chamados/management/commands/disparar_perguntas_competencia.py:141–150`. | Não alcança sem célula/fora da janela; pula parcial de lista, ausência de motivo, desligado e qualquer pergunta histórica do mesmo fato. `…/disparar_perguntas_competencia.py:141`; `:154–161`; `:169–175`; `:211–215`. Contraexemplo: última pergunta morta do mesmo fato → continua sem viva, porém entra em `fato_ja_existe` e não emite. `:211–217`. | Alta para comportamento atual; papel proposto [não verificado]. |

**Limite normativo:** a lei existente exige emissão pelo sinal e varredura como vigia; a matriz proíbe reclassificar papel para ficar verde. `CLAUDE.md:168–170`; `app/core/contratos_estruturais.py:269`. O cadastro de papéis não contém `emissor`, e mantém este comando como juiz. `app/config/crons.py:1955`; `app/config/crons.py:1995–1999`. A exceção de prazo declara expressamente que esse ramo julga **estado**, não prazo. `app/docs/CORTES.md:305`. Portanto, para validar **papel novo próprio**, falta definição/autorização desse papel; não deduzir nem propor lei nova. **Certeza alta** quanto ao cadastro atual; validade futura **[não verificado]**.

## 05/11 15:43 · sessao e4872de7f5d4 · - · - · origem=Ronald (tmux)

Oito itens do tipo **e**, confrontados com implementação de produção; certeza refere-se à leitura estática, sem comprovação de execução ou ocorrência em produção. [não verificado]

1. **Batida “NUNCA bloqueada” — CONTRADIZ; certeza alta.** A docstring afirma que colaborador ativo + tipo válido bastam e que GPS ausente vira sinal, sem barreira (`app/chamados/tests/test_invariantes_disputa.py:193`, `:195`, `:197`). A API exige latitude e longitude e devolve `403/gps_obrigatorio` quando falta uma delas (`app/api/views.py:448`, `:449`, `:462`, `:466`). **Contraexemplo estático:** requisição autenticada de colaborador ativo, `tipo='E'`, sem latitude: ultrapassa os controles de situação/tipo e recebe 403 antes do registro (`app/api/views.py:415`, `:438`, `:449`, `:466`). O próprio teste excepciona essa recusa sem atualizar a docstring (`app/chamados/tests/test_invariantes_disputa.py:287`, `:290`).

2. **Carimbo como anuência — CONTRADIZ; certeza alta.** Dentro da docstring que cita “motor NUNCA fabrica batida”, o texto define anuência como texto **ou** `respondida_em` (`app/chamados/tests/test_sem_resposta_nao_planta_batida.py:9`, `:14`). A implementação exige texto não vazio, registra recusa e retorna antes de calcular o plantio (`app/chamados/services/materializacao.py:796`, `:803`, `:808`, `:813`). **Contraexemplo estático:** pergunta de hora com `resposta_colab=''`, `respondida_em` preenchido e validação administrativa: o carimbo satisfaz a alternativa textual, mas a guarda recusa “sem resposta do colaborador” (`app/chamados/services/materializacao.py:796`, `:803`). A implementação explica expressamente que `respondida_em` “NAO SERVE” (`app/chamados/services/materializacao.py:791`).

3. **Linha da pauta “NUNCA cortada” — CONTRADIZ; certeza alta.** A docstring promete linha inteira e retirada integral de quem não couber (`app/pautas/tests/test_pauta_texto.py:80`). `montar_corpo` limita quantidade de linhas, sem medir caracteres: até quatro, inclui todas; acima disso, inclui três e cauda (`app/pautas/semanal.py:178`, `:180`, `:181`, `:184`). A escrita semanal encaminha o corpo ao serviço que grava `texto[:500]` (`app/pautas/semanal.py:276`, `:278`; `app/pautas/services.py:185`). **Contraexemplo estático:** uma linha acesa com frase de 600 caracteres e URL final; o montador inclui frase e URL completas, mas a persistência corta no caractere 500, perdendo parte da linha e a URL, sem retirada integral (`app/pautas/semanal.py:163`, `:168`, `:180`; `app/pautas/services.py:185`). A existência desses comprimentos em produção é [não verificado].

4. **Só furo do dia do plantio absolve — AINDA VALE; certeza alta.** A docstring estabelece “FURO DE HOJE SO ABSOLVE PLANTIO DE HOJE” (`app/chamados/tests/test_o261_vespera_nao_absolve.py:2`). A função converte o instante para horário local e consulta exclusivamente células em `data=loc.date()`; apenas slots dessas células podem produzir `True` (`app/chamados/services/materializacao.py:156`, `:170`, `:178`, `:181`). Não consulta D−1 nessa avaliação (`app/chamados/services/materializacao.py:170`).

5. **Parcial sem período nunca gera chamado — AINDA VALE no emissor da célula; certeza alta.** A docstring afirma lista administrativa, “NUNCA” chamado, incluindo a fábrica do pai `disputa_supervisao` (`app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:2`, `:3`, `:4`). O predicado retorna `True` quando há `cobertura_parcial` e `periodo_parcial='sem_periodo'`, independentemente de julgamento anterior (`app/ponto/services/cartorio.py:395`, `:398`, `:399`). A fábrica consulta esse predicado e retorna `[]` antes da emissão, declarando ausência de pergunta, pai e push (`app/chamados/services/fabrica_da_celula.py:101`, `:102`, `:104`). A universalidade sobre emissores externos a esse caminho não foi comprovada neste item. [não verificado]

6. **E2 pergunta só ao C6 sobre vitalidade — AINDA VALE; certeza alta.** A docstring afirma que o reconciliador pergunta “SO ao C6” se a pergunta vive (`app/chamados/tests/test_e2_pergunta_so_ao_c6.py:2`, `:3`). No universo de células julgadas que não acusam, o laço consulta `pergunta_viva(p)` e preserva as vivas; quando a morte decorre da tranca, evita carimbo permanente (`app/chamados/reconciliador.py:590`, `:595`, `:610`, `:616`). O teto de célula provisória e a exceção do marco perguntado aceso permanecem dentro do C6 (`app/chamados/juizes.py:1123`, `:1136`, `:1137`). A consulta adicional à tranca decide **carimbar**, não recalcula a vitalidade (`app/chamados/reconciliador.py:612`, `:616`, `:620`).

7. **Prévia e envio usam o mesmo universo — AINDA VALE nesse aspecto; certeza alta.** A docstring que contém “DENTRO” afirma que ambos leem o mesmo `por_colab` (`app/chamados/tests/test_cobrar_so_o_que_muda_a_folha.py:128`, `:131`). A view aplica permissão e `muda_a_folha` antes de preencher esse mapa e antes da bifurcação de prévia; a prévia conta seu tamanho e o envio percorre seus itens (`app/chamados/views_cobrar.py:91`, `:95`, `:97`, `:102`, `:109`, `:116`). Essa identidade de universo não garante quantidade efetivamente entregue: o envio contabiliza falhas do push (`app/chamados/views_cobrar.py:123`, `:130`, `:138`).

8. **Retro nunca inverte tipo pela escala — AINDA VALE na criação; certeza alta.** A docstring afirma que o servidor grava o tipo enviado, sem inferir E/S por escala ou atraso (`app/chamados/tests/test_invariantes_disputa.py:303`, `:305`). A API lê `tipo` da requisição, valida E/S e encaminha esse mesmo valor a `registrar_batida` (`app/api/views.py:432`, `:438`, `:683`, `:684`). As duas criações dentro da porta gravam `tipo=tipo`; os outros resultados são devolução de existente ou recusa de colisão, sem inverter uma nova batida (`app/ponto/registro_batida.py:113`, `:120`, `:150`, `:153`, `:158`).

| Item | Tipo | Texto:linha | Código:linha | VEREDITO | Certeza |
|---|---|---|---|---|---|
| 1 | e | `app/chamados/tests/test_invariantes_disputa.py:193` | `app/api/views.py:449`, `:466` | CONTRADIZ | Alta |
| 2 | e | `app/chamados/tests/test_sem_resposta_nao_planta_batida.py:14` | `app/chamados/services/materializacao.py:796`, `:808` | CONTRADIZ | Alta |
| 3 | e | `app/pautas/tests/test_pauta_texto.py:80` | `app/pautas/semanal.py:180`; `app/pautas/services.py:185` | CONTRADIZ | Alta |
| 4 | e | `app/chamados/tests/test_o261_vespera_nao_absolve.py:2` | `app/chamados/services/materializacao.py:170`, `:181` | AINDA VALE | Alta |
| 5 | e | `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:3` | `app/ponto/services/cartorio.py:398`; `app/chamados/services/fabrica_da_celula.py:102` | AINDA VALE no emissor examinado | Alta |
| 6 | e | `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:3` | `app/chamados/reconciliador.py:610`; `app/chamados/juizes.py:1137` | AINDA VALE | Alta |
| 7 | e | `app/chamados/tests/test_cobrar_so_o_que_muda_a_folha.py:128` | `app/chamados/views_cobrar.py:97`, `:109`, `:116` | AINDA VALE | Alta |
| 8 | e | `app/chamados/tests/test_invariantes_disputa.py:303` | `app/api/views.py:684`; `app/ponto/registro_batida.py:158` | AINDA VALE | Alta |

## 05/11 15:43 · sessao 2ca18a197052 · - · - · origem=Ronald (tmux)

Certeza alta, por leitura do código:

- O teste é `SeloChokepointEscritaTest.test_gravar_estado_sem_pergunta`; sua captura envolve `ch.reconciliar()`, não uma chamada isolada a `_gravar_estado` (`app/ponto/tests/test_selo_chokepoint_escrita.py:286`, `:289`). A fixture cria um `turno_aberto`, subcategoria `saida_pendente`, sem contexto de data explícito (`:225`).
- **Nesta cópia o teto ainda é 17**, não 18 (`app/ponto/tests/test_selo_chokepoint_escrita.py:112`). Portanto “o selo passou a 18” não é fato demonstrado pelo código atual. Qual versão alterada, SQL, tabela e sítio exato da nova EXISTS: **[nao verificado]**.
- `_gravar_estado` atual escreve estado/trilha num atomic e pode disparar receivers e relavra de perguntas (`app/chamados/models.py:274`, `:279`, `:286`). Não contém a EXISTS nova descrita na pergunta. A escrita de `EventoCelula` disponível é um INSERT, pela porta `emitir_evento_celula`, não EXISTS (`app/ponto/portas/celula.py:634`, `:639`).
- O selo mede o **total síncrono de queries da pilha de `reconciliar()` naquele cenário**, incluindo decisão/gates, gravação e signals: `reconciliar` chama `_decisao_do_fechamento` e depois `_gravar_estado` (`app/chamados/models.py:617`, `:620`); `_cobrar` só compara a quantidade com o teto (`app/ponto/tests/test_selo_chokepoint_escrita.py:217`).
- Ele **não prova que ocorreu fechamento**, nem atribui a query a uma origem: o teste não verifica retorno nem estado final (`app/ponto/tests/test_selo_chokepoint_escrita.py:286`). Contraexemplo: `reconciliar()` pode retornar False ao receber decisão None (`app/chamados/models.py:618`), reduzindo queries e passando no teste. Já o cenário de batida com furo verifica explicitamente que fechou (`app/ponto/tests/test_selo_chokepoint_escrita.py:360`).
- Ele também não mede custo posterior de callbacks executados após commit. O próprio selo declara que `TestCase` não executa tais callbacks e mede somente seus registros (`app/ponto/tests/test_selo_chokepoint_escrita.py:174`). Essa contagem se aplica apenas às chaves da batida em `ON_COMMIT_DA_BATIDA` (`:177`, `:213`); `_gravar_estado (sem pergunta)` não integra esse conjunto. Contraexemplo: deslocar a EXISTS para `on_commit` pode fazê-la desaparecer dessa captura sem eliminar seu custo real (`:174`, `:286`).

Não consultei logs nem outras árvores. A execução atual, o número efetivamente medido e a origem exata do delta 17→18 permanecem **[nao verificado]**.

## 05/11 15:43 · sessao e8298b0f5b2a · CRIVO-AINDA-VALE · censo · origem=Code

Universo selecionado: **30 itens**, dos tipos a–e. Os vereditos abaixo tratam do comportamento demonstrável na cópia. Identidade Git do hash, execução dos testes, ocorrência dos contraexemplos e impacto efetivamente pago em produção: **[não verificado]**. [nao verificado]

Os contraexemplos são cenários derivados do código, não registros atribuídos ao dado raspado.

1. **L-089: ausência em folga não produz efeito nem consome saldo — CONTRADIZ.** O texto faz ambas as promessas em `app/docs/LEIS.md:121`. A grade transforma folga coberta por atestado integral em `ausencia` e limpa células; férias contam faltas aprovadas sem filtrar jornada (`app/escala/utils.py:1231`, `app/escala/utils.py:1241`, `app/escala/utils.py:1272`, `app/ferias/models.py:144`). **Contraexemplo:** seis faltas aprovadas já existentes em folgas do período entram na contagem e reduzem o direito de 30 para 24 dias ao aplicar o recálculo (`app/ferias/models.py:54`, `app/ferias/models.py:56`, `app/ferias/services.py:246`).

2. **L-039: falta significa decisão lançada — CONTRADIZ.** A promessa está em `app/docs/LEIS.md:87`. O PDF inicialmente copia `dias_falta` do motor; a substituição pelos totais depende de reconhecer uma competência e obter totais (`app/relatorios/pdf_espelho.py:461`, `app/relatorios/cartao_pela_celula.py:322`, `app/relatorios/cartao_pela_celula.py:346`). **Contraexemplo:** período livre com um dia previsto, sem trabalho, justificativa ou ausência decidida: o motor conta falta, o ramo não substitui esse total e o PDF imprime o badge “Faltas” (`app/ponto/motor_calculo_v2.py:2600`, `app/ponto/motor_calculo_v2.py:2611`, `app/relatorios/cartao_pela_celula.py:34`, `app/relatorios/pdf_espelho.py:787`).

3. **Registro da insuficiência de L-039 — AINDA VALE nos ramos descritos.** A linha diz que, sem apuração e em período livre, o cartão conserva o furo do motor como falta (`app/docs/LEIS.md:163`). Sem totais, `aplicar_nos_totais` não altera o resumo; fora da competência, nem chega à substituição (`app/relatorios/cartao_pela_celula.py:203`, `app/relatorios/cartao_pela_celula.py:322`). Havendo lavratura, existe substituição por faltas decididas; portanto a insuficiência não alcança todo caso sem fechamento (`app/relatorios/cartao_pela_celula.py:342`, `app/relatorios/cartao_pela_celula.py:184`).

4. **L-088: falta custa o previsto daquele dia — AINDA VALE no cálculo examinado.** Essa cláusula está em `app/docs/LEIS.md:120`. O motor seleciona dias obrigatórios sem trabalho e sem justificativa e soma o previsto individual de cada data, em vez de multiplicar uma jornada fixa (`app/ponto/motor_calculo_v2.py:2596`, `app/ponto/motor_calculo_v2.py:2600`, `app/ponto/motor_calculo_v2.py:2618`, `app/ponto/motor_calculo_v2.py:1293`). Isso não elimina a divergência de apresentação do item 2.

5. **L-104: lista de ausências consulta vigência, não fechamento — AINDA VALE.** A cláusula está em `app/docs/LEIS.md:130`. A listagem monta `de/até`, usa mês civil como padrão e consulta `ausencias_vigentes`, cuja interseção contempla início, fim e tipos indeterminados (`app/ponto/views.py:2177`, `app/ponto/views.py:2180`, `app/ponto/turnos.py:3087`). Existe uma exceção explícita: busca por `registro` troca a consulta por identificação direta, sem o filtro temporal (`app/ponto/views.py:2181`).

6. **Censo de dez faltas aprovadas fora de jornada — NAO SE SABE.** O comentário registra dez casos na competência 09 e zero na 10 (`app/ponto/precedencia.py:519`, `app/ponto/precedencia.py:520`). O código atual demonstra uma recusa para tipos sujeitos à jornada quando o vínculo responde `False`; não demonstra aquelas contagens (`app/ponto/precedencia.py:536`, `app/ponto/precedencia.py:538`). Faltam as linhas históricas de ausências, vínculos/células e a reprodução do censo. Os números permanecem **[não verificado]**.

7. **Serviço e property de vencimento concordam “SEMPRE” — CONTRADIZ.** A docstring promete concordância em `app/ferias/test_services_leitura.py:2`. O serviço filtra `status='adquirido'` gravado; a property usa status derivado por saldo e datas (`app/ferias/services.py:118`, `app/ferias/models.py:227`, `app/ferias/models.py:257`). **Contraexemplo:** PA com saldo positivo, status gravado `adquirido` e limite ontem: o serviço inclui, mas a property exclui porque o status derivado é `vencido` (`app/ferias/services.py:119`, `app/ferias/models.py:100`).

8. **Limite de concessão = doze meses após o fim — CONTRADIZ.** O help_text afirma doze meses (`app/ferias/models.py:122`). A edição soma 365 dias ao fim (`app/ferias/services.py:473`). **Contraexemplo:** início em 01/01/2023 produz fim em 31/12/2023 e limite em 30/12/2024, um dia antes de completar doze meses (`app/ferias/services.py:472`, `app/ferias/services.py:473`).

9. **`aviso_emitido` representa aviso emitido com antecedência — CONTRADIZ na semântica literal.** O help_text fala em emissão (`app/ferias/models.py:307`). A escrita calcula apenas a distância entre início e hoje; correção e remarcação repetem esse cálculo (`app/ferias/services.py:314`, `app/ferias/services.py:323`, `app/ferias/services.py:378`, `app/ferias/services.py:404`). **Contraexemplo:** agendamento para daqui a 45 dias recebe `True` sem informação de emissão participar dessa decisão. Emissão externa efetiva: **[não verificado]**.

10. **Abono pecuniário limitado a um terço — AINDA VALE nas portas examinadas.** O help_text declara o teto (`app/ferias/models.py:126`). `dias_vendaveis` desconta o já vendido de `dias_direito // 3`; agendamento com abono e registro histórico recusam ultrapassagem (`app/ferias/models.py:235`, `app/ferias/services.py:197`, `app/ferias/services.py:295`). A conclusão é sobre essas validações; o campo isolado não demonstra uma restrição global de banco (`app/ferias/models.py:126`).

11. **Correção de férias: integridade e colisão recusam; regra legal vira aviso — AINDA VALE.** O comentário declara essa separação (`app/ferias/views.py:474`). A porta verifica quantidade e saldo, coleta avisos, chama a correção e apresenta os avisos após salvar; a gravação continua sujeita ao veto de precedência (`app/ferias/views.py:463`, `app/ferias/views.py:466`, `app/ferias/views.py:483`, `app/ferias/views.py:490`, `app/ferias/views.py:507`, `app/ferias/models.py:428`).

12. **Exigir documento significa recusar lançamento sem anexo — CONTRADIZ.** É o que afirma o help_text (`app/ponto/models.py:584`). A porta calcula o estado e salva; documento exigido e ausente resulta em `aguardando_documento` (`app/ponto/services/ausencia.py:333`, `app/ponto/services/ausencia.py:418`, `app/ponto/services/ausencia.py:779`). **Contraexemplo:** tipo vigente que exige documento, sem anexo e com as demais validações satisfeitas: nasce uma ausência aguardando documento.

13. **`bloqueia_ponto`: colaborador não consegue bater — CONTRADIZ.** A promessa está em `app/ponto/models.py:586`. O caminho online admite colaborador ativo, registra a batida e depois calcula o aviso; a frase de maternidade declara que a batida foi registrada (`app/api/views.py:415`, `app/api/views.py:683`, `app/api/views.py:822`, `app/ponto/services/aviso_da_batida.py:44`). **Contraexemplo:** colaborador ativo, com licença-maternidade aprovada coberta e pedido válido pode registrar a batida; a flag é consultada para o aviso (`app/ponto/services/aviso_da_batida.py:79`, `app/ponto/services/aviso_da_batida.py:88`).

14. **Flag `parcial` significa cobertura de apenas parte do dia — CONTRADIZ.** O help_text afirma isso (`app/ponto/models.py:588`). O catálogo usa a flag para permitir lançamento por minutos; cobertura parcial exige também duração de um dia e minutos estritamente entre zero e 480 (`app/ponto/catalogo/ausencias.py:336`, `app/ponto/catalogo/ausencias.py:358`, `app/ponto/turnos.py:2598`). **Contraexemplo:** tipo com flag parcial e registro de zero ou 720 minutos não é considerado parcial (`app/ponto/turnos.py:2600`).

15. **Falta de dia inteiro = 720 minutos — NAO VALE MAIS como regra universal.** O help_text conserva esse número (`app/ponto/models.py:757`). Para falta aprovada com minutos zero, o `save()` soma o previsto real de cada dia; valores positivos informados são preservados (`app/ponto/models.py:875`, `app/ponto/models.py:883`, `app/ponto/models.py:885`). Assim, o preenchimento automático deixou de estabelecer 720 como duração universal; ainda existe outro caminho que envia 720, descrito no item 30.

16. **Férias geram aviso, “nunca recusa”, na API — CONTRADIZ.** O comentário promete isso (`app/api/views.py:815`, `app/api/views.py:817`). Antes do registro, a mesma função recusa situação `ferias` com HTTP 403 (`app/api/views.py:415`, `app/api/views.py:418`, `app/api/views.py:423`). **Contraexemplo:** colaborador com situação `ferias`, inclusive em dia coberto por férias, recebe `colab_ferias` antes de alcançar o aviso. O comentário anterior reconhece esse bloqueio (`app/api/views.py:413`).

17. **Troca de tipo rejulga o estado aberto e preserva decisão — AINDA VALE.** A docstring delimita o contrato ao estado aberto (`app/ponto/services/ausencia.py:519`, `app/ponto/services/ausencia.py:521`). A edição chama o rejulgamento quando muda o tipo; estados fora de `ABERTAS` retornam intactos, e os abertos passam por `estado_inicial` e `mudar_estado` (`app/ponto/services/ausencia.py:512`, `app/ponto/services/ausencia.py:525`, `app/ponto/services/ausencia.py:527`, `app/ponto/services/ausencia.py:530`).

18. **Férias ficam fora da cobrança documental A4 ao colaborador — AINDA VALE.** O comentário atribui o fluxo ao DP (`app/ponto/services/ausencia.py:731`). `ferias` integra `FORA_DO_A4`; sem documento, recebe `aguardando_decisao`, sem prazo documental (`app/ponto/services/ausencia.py:735`, `app/ponto/services/ausencia.py:777`). Essa conclusão é sobre o estado inicial A4.

19. **Quem exige documento e não tem “NUNCA some”, nem por filtro — CONTRADIZ.** A promessa absoluta está na docstring de teste (`app/ponto/tests/test_atestados_lote.py:11`). O serviço exclui linhas por tipo/status antes de montar a seção de documentos faltantes (`app/ponto/services/atestados_lote.py:60`, `app/ponto/services/atestados_lote.py:62`, `app/ponto/services/atestados_lote.py:77`). **Contraexemplo:** atestado sem documento na janela, com filtro `tipos=['falta']`, desaparece dessa seção. O teste concreto promete algo menor: aparecer **se o tipo estiver no recorte** (`app/ponto/tests/test_atestados_lote.py:84`, `app/ponto/tests/test_atestados_lote.py:87`).

20. **Para folha, cobertura exige aprovação — AINDA VALE.** A cláusula financeira está em `app/ponto/turnos.py:2510`. O ramo `para='folha'` aplica exatamente `status='aprovada'`; o ramo de cobrança usa outro filtro (`app/ponto/turnos.py:2514`, `app/ponto/turnos.py:2516`). Isso verifica a cláusula de aprovação, sem transformar toda ausência parcial em suspensão integral da cobrança.

21. **Desempate: maior data, depois maior PK — AINDA VALE.** O comentário declara esses critérios (`app/ponto/turnos.py:2542`, `app/ponto/turnos.py:2543`). A consulta diária ordena decrescente e pega a primeira; a consulta do período ordena crescente e sobrescreve o mapa, entregando o mesmo vencedor (`app/ponto/turnos.py:2557`, `app/ponto/turnos.py:2757`, `app/ponto/turnos.py:2769`).

22. **480 minutos como fronteira fixa da cobertura parcial — AINDA VALE.** O comentário afirma que o limite é cravado, independente do previsto real; o literal permanece 480 (`app/ponto/turnos.py:2575`, `app/ponto/turnos.py:2577`, `app/ponto/turnos.py:2580`). O predicado usa comparação estrita: 479 pode ser parcial; 480 deixa de ser parcial, satisfeitas as demais condições (`app/ponto/turnos.py:2598`, `app/ponto/turnos.py:2600`).

23. **Falta aprovada sobre folga mantém folga — AINDA VALE na grade.** O comentário distingue falta das demais ausências (`app/escala/utils.py:1227`). O overlay exclui precisamente a combinação de dia `folga` e ausência `falta`; nos demais dias, pode marcar `falta` (`app/escala/utils.py:1231`, `app/escala/utils.py:1234`). Isso não se estende à contagem de faltas do período aquisitivo, que tem o comportamento do item 1.

24. **Parcial dispensa somente marcos dentro do período — AINDA VALE.** O comentário afirma que fora permanece missing e, sem período, nada é dispensado (`app/escala/utils.py:1256`, `app/escala/utils.py:1261`). O código exige período, deslocamento conhecido e célula missing, substituindo apenas marcos julgados `dentro`; as bordas são inclusivas (`app/escala/utils.py:1265`, `app/escala/utils.py:1269`, `app/escala/utils.py:1270`, `app/ponto/turnos.py:2639`).

25. **Range aberto somente para tipos que bloqueiam ponto — NAO VALE MAIS.** O comentário conserva o conjunto antigo (`app/chamados/services/materializacao.py:61`, `app/chamados/services/materializacao.py:62`). O leitor delega a `ausencia_cobre`, que usa `TIPOS_EM_ABERTO`; a autoridade documenta expressamente a substituição do critério antigo (`app/chamados/services/materializacao.py:78`, `app/ponto/turnos.py:2539`, `app/ponto/turnos.py:2555`). A composição efetiva do cadastro em produção é **[não verificado]**.

26. **Feriado só preserva trabalho com `trabalha_em_feriado=True` — CONTRADIZ na formulação literal.** Essa exceção está no texto (`app/ponto/precedencia.py:13`, `app/ponto/precedencia.py:14`). O código usa `not feriado_vira_folga`, cujo critério também considera o ciclo (`app/ponto/precedencia.py:376`, `app/escala/models.py:1392`). **Contraexemplo:** feriado previsto de vínculo 12x36, flag `False`, sem ausência nem folga lançada: permanece trabalho, porque 12x36 não integra os ciclos que folgam no feriado (`app/escala/models.py:32`, `app/ponto/precedencia.py:382`, `app/ponto/precedencia.py:386`).

27. **Dia sem trabalho tem “duas cláusulas, e só elas” — CONTRADIZ.** A docstring inclui qualquer ata preenchida diferente de `trabalho`, inclusive ausência (`app/chamados/juizes.py:412`, `app/chamados/juizes.py:414`). O corpo abre exceção para cobertura parcial (`app/chamados/juizes.py:424`). **Contraexemplo:** `trabalha=True` e ata com `tipo_dia='ausencia', cobertura_parcial=True`: a cláusula textual indicaria dia sem trabalho; a função retorna `False`, preservando a cobrança do resto (`app/chamados/juizes.py:408`, `app/chamados/juizes.py:425`).

28. **Docstring “DENTRO”: marcos cobertos são dispensados, inclusive na madrugada — AINDA VALE por leitura do algoritmo.** Os casos estão em `app/ponto/tests/test_cobparc_marco_no_periodo.py:16`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:18` e `app/ponto/tests/test_cobparc_marco_no_periodo.py:20`. A grade aplica a dispensa por marco; o juiz soma 1440 ao fim noturno e inclui as bordas (`app/escala/utils.py:1269`, `app/ponto/turnos.py:2636`, `app/ponto/turnos.py:2639`). Resultado da execução desses testes: **[não verificado]**.

29. **Ausência integral alimenta zeragem de atraso e saída — AINDA VALE com a restrição ao abono.** A docstring vincula o mapa ao teto L-093; o comentário seguinte limita a ausência abonadora (`app/ponto/turnos.py:2682`, `app/ponto/turnos.py:2686`). O mapa exige `efeito == ABONA` e cobertura integral; o motor usa esse mapa para zerar atraso e saída antecipada (`app/ponto/turnos.py:2690`, `app/ponto/motor_calculo_v2.py:1092`, `app/ponto/motor_calculo_v2.py:1095`).

30. **Literal 720 na ausência materializada por resposta — AINDA VALE nesse caminho.** A regra numérica continua em `app/chamados/services/materializacao.py:521`. A chamada envia 720 e desliga a checagem de lançamento parcial; o `save()` só deriva a jornada quando os minutos da falta aprovada são zero (`app/chamados/services/materializacao.py:523`, `app/ponto/models.py:875`). Portanto, esse caminho preserva 720 mesmo quando o previsto do dia tem outra duração. O valor efetivamente pago em produção é **[não verificado]**.

Na tabela, certeza **alta** significa conclusão sustentada pela leitura estática; não significa teste executado ou ocorrência comprovada em produção. No item 9, a interpretação de “emitido” reduz a certeza; no item 6, o número histórico permanece indeterminado. [nao verificado]

| item | tipo a-e | texto:linha | codigo:linha | VEREDITO | certeza |
|---|---|---|---|---|---|
| 1 — efeito/saldo em folga | b | `app/docs/LEIS.md:121` | `app/escala/utils.py:1231`; `app/ferias/models.py:144`; `app/ferias/services.py:246` | CONTRADIZ | Alta |
| 2 — falta só decidida | b | `app/docs/LEIS.md:87` | `app/relatorios/pdf_espelho.py:461`; `app/relatorios/cartao_pela_celula.py:322` | CONTRADIZ | Alta |
| 3 — insuficiência do cartão | b | `app/docs/LEIS.md:163` | `app/relatorios/cartao_pela_celula.py:203`; `app/relatorios/cartao_pela_celula.py:322` | AINDA VALE | Alta |
| 4 — custo por dia | b | `app/docs/LEIS.md:120` | `app/ponto/motor_calculo_v2.py:2618`; `app/ponto/motor_calculo_v2.py:1293` | AINDA VALE | Alta |
| 5 — listagem por vigência | b | `app/docs/LEIS.md:130` | `app/ponto/views.py:2180`; `app/ponto/turnos.py:3087` | AINDA VALE | Alta |
| 6 — censo histórico | a | `app/ponto/precedencia.py:519` | `app/ponto/precedencia.py:536` | NAO SE SABE | Número indeterminado |
| 7 — vencimento “SEMPRE” igual | e | `app/ferias/test_services_leitura.py:2` | `app/ferias/services.py:118`; `app/ferias/models.py:257` | CONTRADIZ | Alta |
| 8 — doze meses | d | `app/ferias/models.py:122` | `app/ferias/services.py:473` | CONTRADIZ | Alta |
| 9 — aviso emitido | d | `app/ferias/models.py:307` | `app/ferias/services.py:314`; `app/ferias/services.py:323` | CONTRADIZ | Média |
| 10 — teto do abono | d | `app/ferias/models.py:126` | `app/ferias/models.py:235`; `app/ferias/services.py:197` | AINDA VALE | Alta nas portas |
| 11 — correção com aviso | a | `app/ferias/views.py:474` | `app/ferias/views.py:483`; `app/ferias/views.py:490`; `app/ferias/models.py:428` | AINDA VALE | Alta |
| 12 — recusa sem documento | d | `app/ponto/models.py:584` | `app/ponto/services/ausencia.py:418`; `app/ponto/services/ausencia.py:779` | CONTRADIZ | Alta |
| 13 — bloqueio da batida | d | `app/ponto/models.py:586` | `app/api/views.py:683`; `app/ponto/services/aviso_da_batida.py:79` | CONTRADIZ | Alta |
| 14 — significado de parcial | d | `app/ponto/models.py:588` | `app/ponto/catalogo/ausencias.py:358`; `app/ponto/turnos.py:2598` | CONTRADIZ | Alta |
| 15 — falta inteira 720 | d | `app/ponto/models.py:757` | `app/ponto/models.py:875`; `app/ponto/models.py:883` | NAO VALE MAIS | Alta |
| 16 — férias nunca recusam | a | `app/api/views.py:815` | `app/api/views.py:415`; `app/api/views.py:423` | CONTRADIZ | Alta |
| 17 — troca de tipo rejulga | a | `app/ponto/services/ausencia.py:521` | `app/ponto/services/ausencia.py:525`; `app/ponto/services/ausencia.py:530` | AINDA VALE | Alta |
| 18 — férias fora do A4 | a | `app/ponto/services/ausencia.py:731` | `app/ponto/services/ausencia.py:735`; `app/ponto/services/ausencia.py:777` | AINDA VALE | Alta |
| 19 — sem documento nunca some | e | `app/ponto/tests/test_atestados_lote.py:11` | `app/ponto/services/atestados_lote.py:60`; `app/ponto/services/atestados_lote.py:77` | CONTRADIZ | Alta |
| 20 — folha só aprovada | a | `app/ponto/turnos.py:2510` | `app/ponto/turnos.py:2514` | AINDA VALE | Alta |
| 21 — desempate de cobertura | a | `app/ponto/turnos.py:2542` | `app/ponto/turnos.py:2557`; `app/ponto/turnos.py:2769` | AINDA VALE | Alta |
| 22 — fronteira de 480 | c | `app/ponto/turnos.py:2580` | `app/ponto/turnos.py:2600` | AINDA VALE | Alta |
| 23 — falta em folga | a | `app/escala/utils.py:1227` | `app/escala/utils.py:1231` | AINDA VALE | Alta |
| 24 — dispensa parcial por marco | a | `app/escala/utils.py:1256` | `app/escala/utils.py:1265`; `app/escala/utils.py:1270` | AINDA VALE | Alta |
| 25 — conjunto do range aberto | a | `app/chamados/services/materializacao.py:61` | `app/chamados/services/materializacao.py:78`; `app/ponto/turnos.py:2555` | NAO VALE MAIS | Alta |
| 26 — feriado e flag | a | `app/ponto/precedencia.py:13` | `app/ponto/precedencia.py:376`; `app/escala/models.py:1392` | CONTRADIZ | Alta |
| 27 — somente duas cláusulas | a | `app/chama

[truncado em 20000 de 20616 caracteres]

## 05/11 15:42 · sessao 25bd4976d69a · - · - · origem=Ronald (tmux)

Auditei 10 candidatos c/d. Certeza alta refere-se ao comportamento estático; ocorrência e efeito financeiro em produção [não verificado].

| ID | Tipo | Texto e afirmação | Código e comportamento | Veredito | Certeza |
|---|---|---|---|---|---|
| C1 | c | `app/chamados/models.py:3004` e `:3005`: respostas `FALTA_DIA`/`ATESTADO` correspondem a ausência de **720 min**. | `app/chamados/services/materializacao.py:468` lê o mapa; `:484` e `:487` fornecem seus minutos a `criar_ausencia`. Na criação nova, `app/ponto/services/ausencia.py:343` repassa esse valor ao modelo. | AINDA VALE | Alta |
| C2 | c | `app/chamados/services/materializacao.py:521`: ausência da resposta recebe **720 min**. | `app/chamados/services/disputa_emissao.py:1330` chama esse materializador para os escapes; `materializacao.py:519`–`:523` fornece 720 à criação, com `checar_parcial=False`. A regra não consulta a jornada nesse ponto. | AINDA VALE | Alta |
| C3 | c | `app/chamados/services/declaracao_texto.py:25`: hora declarada precisa ficar a **≤240 min** de marco faltante. | `:128`–`:131` calcula distância circular; `:137`–`:142` rejeita distância >240 e também proximidade ambígua entre os dois melhores candidatos (<30 min de diferença). O teto é necessário, não suficiente. | AINDA VALE | Alta |
| C4 | c | `app/chamados/services/desvio_escala.py:12`: “batida casa com o marco mais próximo até 4h”. | `:82`–`:85` aplica isso somente ao dia incompleto; `:74`–`:80` casa dias completos por posição, sem teto de 240 e sem procurar marco mais próximo. | CONTRADIZ | Alta |
| C5 | c | `app/chamados/catalogo/motor.py:79`: escalonamento N3 em **240 min**. | `app/ponto/management/commands/escalonar_chamados_supervisao.py:145` usa a constante para marcar incidente; `:170` reutiliza o limiar para aviso ao DP, condicionado à janela comercial. | AINDA VALE | Alta |
| C6 | c | `app/chamados/juizes.py:1451`: suspeita AM/PM começa em **660 min** de distância circular. | `:1450`–`:1455` calcula e devolve essa classificação; `:1553` replica o mesmo limiar no delta da resposta; `app/chamados/services/validacao.py:822` leva resposta com `ampm` ao lote B. | AINDA VALE | Alta |
| D1 | d | `app/chamados/models.py:1679`: correções solicitadas pelo colaborador têm máximo **2**. | `:2126`–`:2130` recusa quando contador ≥2; `:2131`–`:2137` só incrementa depois de encontrar e reabrir respostas corrigíveis. | AINDA VALE | Alta |
| D2 | d | `app/chamados/models.py:1683`: reabertura administrativa tem máximo **2**. | `:2043`–`:2044` recusa quando contador ≥2; `:2056` incrementa a reabertura aceita. Reabertura por sistema não consome esse contador (`:2066`–`:2080`). | AINDA VALE | Alta |
| D3 | d | `app/chamados/models.py:2782`: `materializada_em` significa “batida criada + chamado fechado”. | `app/chamados/services/materializacao.py:519`–`:525` cria ausência; `:545`–`:546` pode carimbar perguntas; `:559`–`:561` deixa de reconciliar o destino enquanto ausência está aberta. O carimbo escreve o campo em `app/chamados/models.py:2547` e grava em `:2598`. Há processamento sem batida e sem fechamento. | CONTRADIZ | Alta |
| D4 | d | `app/chamados/models.py:2797`: chamados clones são “fechados junto quando pergunta materializada”. | `app/chamados/services/materializacao.py:742`–`:748` visita clones, mas `:729`–`:737` retém qualquer alvo com irmã respondida sem consumo; `:738` apenas solicita reconciliação aos demais, condicionada ao gate. | CONTRADIZ | Alta |

Contraexemplos estáticos:

- **C4:** template com marcos 08:00/17:00 e dia com exatamente duas batidas, 13:00/22:00. Cada batida fica a 300 min do marco posicional; ambas entram nas amostras pelo ramo de `app/chamados/services/desvio_escala.py:74`–`:77`, que executa `continue` antes do teto em `:84`. A ocorrência desse cenário em produção [não verificado].
- **D3:** resposta `ATESTADO` cria ausência sem documento pelo caminho `app/chamados/services/disputa_emissao.py:1330` → `app/chamados/services/materializacao.py:519`; quando a porta aceita o carimbo em `:546`, `materializada_em` fica preenchido sem batida. Se a ausência permanece aberta, o destino não recebe reconciliação nesse ramo (`:559`–`:561`). O requisito do contraexemplo é a porta aceitar o carimbo; uma ocorrência real [não verificado].
- **D4:** clone vivo possui irmã respondida ainda sem consumo. Ao materializar outra pergunta e visitar esse clone, `app/chamados/services/materializacao.py:729`–`:737` retorna `False` antes da reconciliação; portanto o clone permanece aberto. Ocorrência atual em produção [não verificado].

Não encontrei uso de 480/420 como regra de minutos na busca delimitada; essa ausência é resultado de busca, não prova de exaustividade do app inteiro [não verificado].

## 05/11 15:42 · sessao f65ed8bae200 · - · - · origem=Ronald (tmux)

Itens tipo **b**, sem execução nem escrita. Os contraexemplos abaixo são de fluxo estático; ocorrência em produção **[não verificado]**.

| item | texto e afirmação | código observado | VEREDITO | certeza |
|---|---|---|---|---|
| B1 | `app/docs/LEIS.md:125`, L-092: depois do export, diferença não altera o gravado; vira pauta. | `app/ponto/services/fechamento.py:530` detecta exportada apenas para registrar; `:551` aplica guarda específica; `:1128` salva aberto e `:1144` atualiza outros estados. Só `(9,2026)` está guardada em `:36`. | **CONTRADIZ** | Alta, estática. |
| B2 | `app/docs/LEIS.md:91`, cláusula “`dia_em_competencia_exportada` decide”. A própria linha também declara a cláusula revogada. | `app/chamados/juizes.py:1722` registra a remoção; `app/chamados/services/fabrica_da_celula.py:91`–`:126` não chama esse juiz. | **NAO VALE MAIS** | Alta. |
| B3 | `app/docs/LEIS.md:189`: `motivos_a_perguntar` ainda cala emissão em dia exportado. | `app/chamados/services/fabrica_da_celula.py:91`–`:126` aplica trabalho, ativo, futuro, veredito, parcial e dedup; `:185` restringe o desvio para pauta ao limbo `trabalha=None`. | **NAO VALE MAIS** | Alta; não generalizar à exceção do limbo. |
| B4 | `app/docs/LEIS.md:189`: a tranca recusa cobrança em `cobrar_dia.py`. | `app/chamados/services/cobrar_dia.py:37` registra o toque e `:39` chama `julgar_celula(..., toque=True)`; `app/chamados/juizes.py:949` deixa a tranca de calar depois de toque humano. | **NAO VALE MAIS** | Alta. |
| B5 | `app/docs/LEIS.md:92`, coluna de implementação: `competencia_trancada.encerrar` continua carimbando a via em pergunta viva. | `app/chamados/services/competencia_trancada.py:76` inicializa `perguntas=[]`; `:125`–`:133` conta por leitura; `:181` encerra chamados e `:196` fecha disputas. | **NAO VALE MAIS** | Alta, no caminho citado. |
| B6 | `app/docs/LEIS.md:92`, cláusula normativa: nenhuma pergunta nova nasce com `competencia_trancada`. | O encerramento atual não carimba perguntas (`app/chamados/services/competencia_trancada.py:177`–`:200`); o escritor antigo foi removido (`app/chamados/models.py:2514`), enquanto o C6 lê a tranca (`app/chamados/juizes.py:1082`). | **AINDA VALE** | Alta para as portas examinadas. |
| B7 | `app/docs/LEIS.md:90`, L-042: a proposta é recalculada a cada corrida. | A faixa chama `propostas` em `app/chamados/services/proposta_no_fio.py:67`; o POST recalcula em `:106` antes de aplicar (`:138`) ou recusar (`:173`). | **AINDA VALE** | Alta, escopo do fio. |
| B8 | `app/docs/LEIS.md:90`, leitura literal: “a proposta não se guarda; só a decisão mora em `DecisaoProposta`”. | Existe `resumo` JSON descrito como foto da proposta (`app/escala/models.py:1924`); aceitar e recusar persistem `_resumo(p)` (`app/escala/services/propositor.py:720`, `:776`), contendo template, âncora e métricas (`:723`). | **CONTRADIZ**, na leitura literal de armazenamento | Alta. A proposta **viva** continua recalculada, conforme B7. |
| B9 | `app/docs/LEIS.md:92`: existem 1.994 perguntas históricas e ficam intocadas. | O código preserva leitura da via histórica (`app/chamados/juizes.py:1087`) e permite desfazê-la por reabertura (`app/chamados/models.py:2704`). A quantidade e a conservação dos registros exigiriam dado histórico/atual e trilha, ausentes no anexo. | **NAO SE SABE** | Alta quanto à insuficiência; quantidade e integridade **[não verificado]**. |

**Contraexemplo B1:** competência **10/2026**, empresa já exportada, colaborador elegível e recalculo com escrita: `(10,2026)` não consta na guarda (`app/ponto/services/fechamento.py:36`), exportação gera aviso (`:532`), e o escritor atualiza o fechamento (`:1144`). Se os números calculados diferirem, o gravado muda; a execução real desse cenário é **[não verificado]**.

**Contraexemplo B8:** recusar proposta com `template`, `ancora` e métricas preenchidos grava esses valores em `DecisaoProposta.resumo` (`app/escala/services/propositor.py:717`–`:727`), contrariando o absoluto “a proposta não se guarda” (`app/docs/LEIS.md:90`).

## 05/11 15:41 · sessao 29b1d849d184 · CRIVO-AINDA-VALE · censo · origem=Code

Referências abaixo relativas a `app/`. Certeza **alta** significa evidência estática no código; execução e ocorrência em produção são [nao verificado]. O vínculo desta cópia com um commit Git correspondente ao hash informado é [nao verificado]. [nao verificado]

1. **L-033: “retém todo furo”.** A coluna de implementação ainda afirma isso em `docs/LEIS.md:81`. Hoje há isenção explícita para protesto exclusivamente `FURO_PARCIAL` com falta apenas de pausa (`folha/export.py:1147`, `folha/export.py:1235`). **NAO VALE MAIS** para essa coluna antiga; a própria linha documental já registra a correção (`docs/LEIS.md:81`).

2. **L-051: nenhuma pergunta nova recebe a via da tranca.** Essa cláusula está em `docs/LEIS.md:92`. O encerramento atual atua sobre chamados e disputas, deixando perguntas fora da escrita (`chamados/services/competencia_trancada.py:125`, `chamados/services/competencia_trancada.py:178`). **AINDA VALE** para a cláusula; a quantidade histórica de 1.994 perguntas é [nao verificado].

3. **L-052: folha emitida não pode ser corrigida.** O texto principal diz “nem para corrigir”, mas a própria linha declara superação parcial (`docs/LEIS.md:93`). Existe porta de retificação com autor e motivo, que abre a guarda e solicita recálculo (`ponto/portas/regen_exportada.py:106`, `ponto/portas/regen_exportada.py:174`, `ponto/portas/regen_exportada.py:179`). **NAO VALE MAIS** como proibição absoluta; permanece a guarda específica de 09/2026 (`ponto/services/fechamento.py:551`).

4. **L-095: leitores deixam de chamar o motor.** A promessa inclui fechamento e TXT (`docs/LEIS.md:113`). A geração do TXT chama recálculo em leitura, cujo caminho chama `motor.calcular_mes` (`folha/views.py:207`, `ponto/services/fechamento.py:277`). **CONTRADIZ. Contraexemplo:** uma geração válida com colaborador e fatia apurável executa o motor para obter o número, em vez de apenas ler `DiaPago` (`folha/views.py:208`, `ponto/services/fechamento.py:269`).

5. **L-097: qualquer HE pendente trava o export.** Essa cláusula absoluta está em `docs/LEIS.md:115`. O código conta somente dias acima do limite e sem decisão; a trava depende de cadastro que nasce desligado (`folha/porta_export.py:230`, `folha/porta_export.py:232`, `folha/porta_export.py:690`, `colaboradores/models.py:114`). **CONTRADIZ. Contraexemplo:** pendência com trava desligada e demais verificações verdes não acrescenta falha nem impede a liberação pelo conferidor (`folha/porta_export.py:687`, `folha/porta_export.py:867`).

6. **L-104: universo do holerite usa mês civil.** A afirmação está em `docs/LEIS.md:130`. `_limites` calcula primeiro e último dia do mês; `universo_por_empresa` usa essas janelas para admissão e demissão (`holerite/matriz.py:51`, `holerite/matriz.py:89`, `holerite/matriz.py:100`). **AINDA VALE** nesse recorte do universo.

7. **L-113: regeneração em exportada deixa de ser trava.** Essa revogação está em `docs/LEIS.md:149`, que também reconhece a implementação pendente. A porta ordinária ainda consulta competências exportadas e só dispensa essa guarda quando recebe `apesar_da_lavra` (`ponto/portas/celula.py:196`). **CONTRADIZ. Contraexemplo:** regeneração ordinária de dia protegido, sem esse motivo, continua submetida ao bloqueio; a porta de exceção precisa fornecer explicitamente o motivo (`ponto/portas/celula.py:199`, `ponto/portas/regen_exportada.py:174`).

8. **L-113: DIFF publicado antes do ato de frota.** A exigência permanece no texto (`docs/LEIS.md:149`). O regerador produz cópia anterior, arquivo novo e hash antes do registro (`folha/management/commands/regerar_txt_dominio.py:75`, `folha/management/commands/regerar_txt_dominio.py:99`, `folha/management/commands/regerar_txt_dominio.py:118`). Isso não comprova publicação prévia. **NAO SE SABE:** faltam o DIFF publicado com data, a abrangência da frota e o instante do ato [nao verificado].

9. **`invalidada_em` nulo significa “vale”.** O `help_text` afirma isso (`folha/models.py:97`), mas a vigência também depende de ser a geração mais recente (`folha/models.py:134`, `folha/export.py:572`). **CONTRADIZ. Contraexemplo:** registrar A e depois B sem invalidar A deixa A com campo nulo e `e_vigente=False` (`folha/models.py:137`, `folha/models.py:182`).

10. **Quem entra no TXT ganha aprovação.** O comentário faz essa afirmação universal (`folha/views.py:213`). O código exclui do carimbo quem possui determinadas travas, mantendo os mesmos itens na montagem (`folha/views.py:244`, `folha/views.py:268`). **CONTRADIZ. Contraexemplo:** item `entra` com `turno_aberto` continua no TXT, mas fica fora da seleção de aprovação (`folha/views.py:247`). A exceção já aparece no comentário seguinte (`folha/views.py:216`).

11. **12x36 nunca emite parcela de feriado.** A docstring de teste apresenta a regra sem exceção (`folha/tests/test_export_59a.py:1`). O código emite quando a cláusula de dobra está ligada; o próprio teste espera essa emissão (`folha/export.py:411`, `folha/tests/test_export_59a.py:48`). **NAO VALE MAIS** como descrição completa da emissão.

12. **`conteudo` guarda cópia fiel do TXT entregue.** A promessa está em `folha/models.py:86`. A mesma variável alimenta a resposta e o registro, sendo persistida sem transformação adicional (`folha/views.py:281`, `folha/views.py:285`, `folha/views.py:299`, `folha/models.py:186`). **AINDA VALE** para o conteúdo preparado pelo servidor; recebimento efetivo pelo cliente é [nao verificado].

13. **Invalidar exige motivo.** O `help_text` exige motivo (`folha/models.py:99`). A porta remove espaços e rejeita valor vazio antes de qualquer gravação (`folha/models.py:151`, `folha/models.py:153`). **AINDA VALE** pela porta `invalidar`.

14. **Conta vazia não barra o carimbo do TXT.** O corte está documentado em `folha/export.py:730`. A tupla exclui `conta_vazia`, e a view remove essa trava antes de selecionar os aprováveis (`folha/export.py:743`, `folha/views.py:243`). **AINDA VALE**, desde que não exista outra trava (`folha/views.py:247`).

15. **Férias não são emitidas como pagamento de evento variável.** A lápide afirma isso (`folha/export.py:196`). A montagem local emite os eventos de ponto e acrescenta faltas separadamente, sem evento de pagamento de férias (`folha/export.py:428`, `folha/export.py:721`). **AINDA VALE** para essa montagem; o cálculo realizado externamente pelo Domínio é [nao verificado].

16. **Carimbo de ontem não libera TXT hoje.** A afirmação está no texto da porta (`folha/porta_export.py:14`). O conferidor busca a data atual e recusa carimbo ausente ou de outra competência; a view interrompe a geração diante da recusa (`folha/porta_export.py:807`, `folha/porta_export.py:815`, `folha/views.py:130`). **AINDA VALE**.

17. **Guarda de 09/2026 barra escrita e permite leitura pura.** O comentário distingue os dois modos (`ponto/services/fechamento.py:27`, `ponto/services/fechamento.py:31`). O código calcula empresas exportadas apenas para escrita/backfill e exclui seus colaboradores quando a competência está guardada (`ponto/services/fechamento.py:530`, `ponto/services/fechamento.py:551`). **AINDA VALE**.

18. **Recálculo pode corrigir qualquer competência exportada.** A docstring mantém essa generalização (`ponto/services/fechamento.py:476`). A guarda posterior de 09/2026 exclui colaboradores de empresas exportadas (`ponto/services/fechamento.py:37`, `ponto/services/fechamento.py:553`). **NAO VALE MAIS** como regra sem exceção.

19. **Trancada significa “nada mais muda”.** Essa definição está no comentário da máquina de estados (`ponto/services/fechamento.py:1441`). O escritor atualiza números sem consultar a tranca nesse caminho (`ponto/services/fechamento.py:1144`); a consulta da tranca está separada (`ponto/services/fechamento.py:1464`). **CONTRADIZ. Contraexemplo:** fechamento aprovado de 10/2026, com período trancado e apuração válida, pode alcançar o `update`; a guarda específica contém apenas 09/2026 (`ponto/services/fechamento.py:37`, `ponto/services/fechamento.py:1144`).

20. **Folga trabalhada recebe 100% somente com escala certa.** O comentário declara essa condição (`ponto/services/fechamento.py:917`). O código filtra os períodos pelo juiz de escala certa e soma os períodos remanescentes na rubrica de folga (`ponto/services/fechamento.py:925`, `ponto/services/fechamento.py:949`). **AINDA VALE**, com a separação de feriado descrita no item seguinte.

21. **Feriado trabalhado deve sair separado da folga trabalhada.** O corte determina essa separação (`ponto/services/fechamento.py:927`). O código identifica feriado, remove esses períodos da folga e os soma em HE100 de feriado (`ponto/services/fechamento.py:940`, `ponto/services/fechamento.py:944`, `ponto/services/fechamento.py:946`). **AINDA VALE** nesse conjunto de períodos.

22. **Hora de folga sem escala certa não desaparece.** O comentário promete horas trabalhadas sem o adicional de 100% (`ponto/services/fechamento.py:951`). O código separa esses períodos dos conjuntos de folga certa e feriado, acrescentando seus minutos em trabalhadas (`ponto/services/fechamento.py:964`, `ponto/services/fechamento.py:968`). **AINDA VALE**.

23. **Crédito parcial abate atraso apenas do próprio dia.** O texto afirma essa restrição (`ponto/services/fechamento.py:973`). O fechamento usa mapas diários; o helper aplica o mínimo entre atraso e crédito da mesma chave de data (`ponto/services/fechamento.py:982`, `ponto/services/credito_parcial.py:54`). **AINDA VALE**.

24. **Ninguém lê `DiaPago` ainda.** O comentário da fase S1 mantém essa afirmação (`ponto/services/fechamento.py:1182`). O calendário chama o leitor de linhas, que consulta `DiaPago`; a porta do export também consulta essas linhas (`colaboradores/services/calendario.py:290`, `ponto/services/dia_pago.py:852`, `folha/porta_export.py:588`). **NAO VALE MAIS**.

25. **Versão anterior do holerite nunca é apagada.** O comentário promete histórico preservado (`holerite/models.py:138`). O upload avulso rejeita anteriores com ciência, mas deleta os sem ciência (`holerite/views.py:1176`, `holerite/views.py:1191`). **CONTRADIZ. Contraexemplo:** novo upload válido para competência que já possui documento sem ciência apaga o anterior e cria outro, sem incrementar explicitamente a versão (`holerite/views.py:1193`, `holerite/models.py:153`).

26. **Empresa da folha sempre vem do CNPJ impresso.** O `help_text` exclui expressamente a empresa atual do cadastro (`holerite/models.py:123`). O lote segue essa origem; o avulso usa `colab.empresa_id` (`holerite/motor.py:479`, `holerite/views.py:1202`). **CONTRADIZ. Contraexemplo:** documento da empresa A enviado após transferência cadastral para B grava B no upload avulso (`holerite/views.py:1199`).

27. **Republicação do lote preserva recibos publicados.** O comentário define publicação incremental e ausência de deleção pelo motor (`holerite/motor.py:316`). Recibos já existentes recebem `ja_publicado`, enquanto o gravador cria apenas itens `ok` (`holerite/motor.py:344`, `holerite/motor.py:468`). **AINDA VALE** no lote; o comportamento avulso está delimitado no item 25.

28. **Lote é tudo ou nada.** O comentário já ressalva que arquivos escritos em storage não são revertidos (`holerite/motor.py:373`, `holerite/motor.py:382`). `_gravar` executa dentro de `transaction.atomic()` (`holerite/motor.py:385`). **AINDA VALE** para as linhas de banco, com a limitação expressa sobre arquivos.

29. **Republicar o mesmo folhão não duplica a carta.** Essa promessa está em `holerite/models.py:143`. A primeira publicação conjunta concatena a carta, mas cria o holerite sem preencher `anexos_sha`, cujo default é vazio (`holerite/motor.py:475`, `holerite/motor.py:480`, `holerite/models.py:152`). **CONTRADIZ. Contraexemplo:** recibo de uma página mais carta de uma página gera duas páginas; repetir os mesmos bytes faz a carta passar pelo filtro de hash vazio e ser anexada novamente, produzindo três páginas (`holerite/motor.py:341`, `holerite/motor.py:448`, `holerite/motor.py:458`).

30. **Download não assina o holerite.** O comentário separa download de ciência (`holerite/views.py:1514`). O corpo registra entrega na sessão e retorna o PDF, sem criar assinatura (`holerite/views.py:1524`, `holerite/views.py:1537`). **AINDA VALE** nesse endpoint.

31. **`seq` ordena os recibos complementares por colaborador.** A definição está no `help_text` (`holerite/models.py:109`). O gravador mantém contador por colaborador, incrementa para cada recibo `ok` e grava esse número (`holerite/motor.py:465`, `holerite/motor.py:471`, `holerite/motor.py:480`). **AINDA VALE**.

32. **Recibos reais não trazem CPF; match teria resultado 29/29.** A docstring atribui essas conclusões a exports medidos (`holerite/motor.py:180`, `holerite/motor.py:186`). O parser mostra quais campos extrai, não comprova o conteúdo dos documentos históricos (`holerite/motor.py:99`, `holerite/motor.py:117`). **NAO SE SABE:** faltam os PDFs ou extrações verificáveis e o cadastro usado naquela comparação [nao verificado].

33. **Falta de dia inteiro corresponde a 720 minutos.** O `help_text` afirma isso (`ponto/models.py:757`). Para falta aprovada com minutos zerados, `save` deriva a soma do previsto; o export utiliza os minutos efetivamente registrados (`ponto/models.py:875`, `folha/export.py:110`). **NAO VALE MAIS** como valor universal. Exemplo construtivo: jornada 08–17 com pausa 12–13 deriva 480, pela subtração do intervalo (`escala/utils.py:2293`, `escala/utils.py:2335`).

34. **480 minutos é o limiar fixo de ausência parcial.** O literal permanece em `ponto/turnos.py:2580`. O predicado exige tipo que aceita horas, um único dia e `0 < minutos < 480` (`ponto/turnos.py:2598`). **AINDA VALE** e alcança o dinheiro: o fechamento usa esse predicado para selecionar ausências inteiras; o motor zera atraso e saída antecipada desses dias (`ponto/services/fechamento.py:683`, `ponto/turnos.py:2690`, `ponto/motor_calculo_v2.py:1093`).

| item | tipo a-e | texto:linha | codigo:linha | VEREDITO | certeza |
|---|---|---|---|---|---|
| 01 | b | `docs/LEIS.md:81` | `folha/export.py:1235` | NAO VALE MAIS | Alta |
| 02 | b | `docs/LEIS.md:92` | `chamados/services/competencia_trancada.py:178` | AINDA VALE | Alta |
| 03 | b | `docs/LEIS.md:93` | `ponto/portas/regen_exportada.py:174` | NAO VALE MAIS | Alta |
| 04 | b | `docs/LEIS.md:113` | `folha/views.py:208`; `ponto/services/fechamento.py:277` | CONTRADIZ | Alta |
| 05 | b | `docs/LEIS.md:115` | `folha/porta_export.py:690` | CONTRADIZ | Alta |
| 06 | b | `docs/LEIS.md:130` | `holerite/matriz.py:51`; `holerite/matriz.py:89` | AINDA VALE | Alta |
| 07 | b | `docs/LEIS.md:149` | `ponto/portas/celula.py:196` | CONTRADIZ | Alta |
| 08 | b | `docs/LEIS.md:149` | `folha/management/commands/regerar_txt_dominio.py:118` | NAO SE SABE | Alta: evidência insuficiente |
| 09 | d | `folha/models.py:97` | `folha/models.py:134`; `folha/export.py:572` | CONTRADIZ | Alta |
| 10 | a | `folha/views.py:213` | `folha/views.py:247`; `folha/views.py:268` | CONTRADIZ | Alta |
| 11 | e | `folha/tests/test_export_59a.py:1` | `folha/export.py:411` | NAO VALE MAIS | Alta |
| 12 | d | `folha/models.py:86` | `folha/views.py:285`; `folha/views.py:299` | AINDA VALE | Alta |
| 13 | d | `folha/models.py:99` | `folha/models.py:151` | AINDA VALE | Alta |
| 14 | a | `folha/export.py:730` | `folha/export.py:743`; `folha/views.py:244` | AINDA VALE | Alta |
| 15 | a | `folha/export.py:196` | `folha/export.py:428`; `folha/export.py:721` | AINDA VALE | Alta |
| 16 | a | `folha/porta_export.py:14` | `folha/porta_export.py:807`; `folha/views.py:130` | AINDA VALE | Alta |
| 17 | a | `ponto/services/fechamento.py:27` | `ponto/services/fechamento.py:530`; `ponto/services/fechamento.py:551` | AINDA VALE | Alta |
| 18 | a | `ponto/services/fechamento.py:476` | `ponto/services/fechamento.py:553` | NAO VALE MAIS | Alta |
| 19 | a | `ponto/services/fechamento.py:1441` | `ponto/services/fechamento.py:1144` | CONTRADIZ | Alta |
| 20 | a | `ponto/services/fechamento.py:917` | `ponto/services/fechamento.py:925`; `ponto/services/fechamento.py:949` | AINDA VALE | Alta |
| 21 | a | `ponto/services/fechamento.py:927` | `ponto/services/fechamento.py:940`; `ponto/services/fechamento.py:946` | AINDA VALE | Alta |
| 22 | a | `ponto/services/fechamento.py:951` | `ponto/services/fechamento.py:964`; `ponto/services/fechamento.py:968` | AINDA VALE | Alta |
| 23 | a | `ponto/services/fechamento.py:973` | `ponto/services/credito_parcial.py:54` | AINDA VALE | Alta |
| 24 | a | `ponto/services/fechamento.py:1182` | `ponto/services/dia_pago.py:852`; `folha/porta_export.py:588` | NAO VALE MAIS | Alta |
| 25 | a | `holerite/models.py:138` | `holerite/views.py:1191` | CONTRADIZ | Alta |
| 26 | d | `holerite/models.py:123` | `holerite/views.py:1202` | CONTRADIZ | Alta |
| 27 | a | `holerite/motor.py:316` | `holerite/motor.py:344`; `holerite/motor.py:468` | AINDA VALE | Alta |
| 28 | a | `holerite/motor.py:373` | `holerite/motor.py:385` | AINDA VALE | Alta |
| 29 | a | `holerite/models.py:143` | `holerite/motor.py:475`; `holerite/motor.py:480`; `holerite/motor.py:458` | CONTRADIZ | Alta |
| 30 | a | `holerite/views.py:1514` | `holerite/views.py:1524`; `holerite/views.py:1537` | AINDA VALE | Alta |
| 31 | d | `holerite/models.py:109` | `holerite/motor.py:471`; `holerite/motor.py:480` | AINDA VALE | Alta |
| 32 | a | `holerite/motor.py:180` | `holerite/motor.py:99`; `holerite/motor.py:117` | NAO SE SABE | Alta: evidência insuficiente |
| 33 | d | `ponto/models.py:757` | `ponto/models.py:875`; `folha/export.py:110` | NAO VALE MAIS | Alta |
| 34 | c | `ponto/turnos.py:2580` | `ponto/turnos.py:2598`; `ponto/services/fechamento.py:683` | AINDA VALE | Alta |

## 05/11 15:41 · sessao 948d1f05a7df · - · - · origem=Ronald (tmux)

Levantamento só por leitura; os contraexemplos abaixo são de fluxo do código, e sua ocorrência em produção é `[nao verificado]`.

| ID | Tipo | Texto | O que afirma / o que faz | Veredito | Certeza |
|---|---|---|---|---|---|
| G1 | b | `app/docs/LEIS.md:71` — L-020 | A exceção à imutabilidade deve ser denunciada pela célula. O ramo normal guarda `dna_anterior`, data e contador em `app/ponto/portas/celula.py:259`; o ramo fora da vigência altera `trabalha`/DNA, mas salva somente esses dois campos em `app/ponto/portas/celula.py:231`. Contraexemplo: célula gerada, ainda ligada à EC encerrada, sem outro vínculo cobrindo o dia e sem bloqueio de lavra: é reescrita sem os campos de denúncia (`app/ponto/portas/celula.py:225`). | **CONTRADIZ** | Alta, estática |
| G2 | b | `app/docs/LEIS.md:93` — L-052 | “Nem para corrigir” não é mais absoluto: `apesar_da_lavra` esvazia o conjunto bloqueado em `app/ponto/portas/celula.py:196`. A própria linha da LEIS registra a superação parcial; a função por dia também repassa essa abertura em `app/ponto/portas/celula.py:482`. | **NAO VALE MAIS**, para a proibição absoluta | Alta |
| G3 | b | `app/docs/LEIS.md:149` — L-113 | A cláusula de revogar a regeneração bloqueada em exportada ainda não está executada no caminho ordinário: sem `apesar_da_lavra`, carrega competências fechadas e pula os dias (`app/ponto/portas/celula.py:199`, `app/ponto/portas/celula.py:238`). Contraexemplo: correção comum de DNA em dia abrangido por competência fechada permanece barrada. A mesma linha documental reconhece expressamente essa pendência. | **CONTRADIZ**, cláusula 2 normativa | Alta |
| G4 | b | `app/docs/LEIS.md:124` — L-091 | Diz que `tipo_base` é rótulo e não manda no cálculo. `get_motor` ainda seleciona por ele em `app/ponto/motor_calculo_v2.py:3625`, `app/ponto/motor_calculo_v2.py:3633` e `app/ponto/motor_calculo_v2.py:3668`; só um ramo de `turno_partido` é reinterpretado pelo desenho. Contraexemplo: mesmo desenho 08–17 com `tipo_base='12x36'` escolhe `Motor12x36ComEscala`; com `tipo_base='comercial'`, `MotorComercial` (`app/ponto/motor_calculo_v2.py:3633`, `app/ponto/motor_calculo_v2.py:3668`). | **CONTRADIZ**, alcance geral | Alta |
| G5 | a | `app/ponto/portas/celula.py:349` | “A ata se re-julga antes do dinheiro”. O código tenta julgar primeiro, mas engole falha por dia em `app/ponto/portas/celula.py:375`; o bloco seguinte ainda chama o recálculo em `app/ponto/portas/celula.py:397`. Contraexemplo: `julgar_celula` levanta exceção; ela é registrada e o recálculo continua. A precedência garantida é da tentativa, não de julgamento concluído (`app/ponto/portas/celula.py:368`). | **CONTRADIZ**, se lido como garantia de ata atualizada | Alta quanto ao fluxo |
| G6 | a | `app/escala/alimentacao.py:272` — LEI E1 | A geradora só vale dentro de sua vigência; fora dela devolve `None`, sem escolher outra. A implementação localiza a geradora, recusa falta dela e verifica início/fim antes de devolver (`app/escala/alimentacao.py:279`, `app/escala/alimentacao.py:282`). O ramo não exige `ativa=True` (`app/escala/alimentacao.py:284`). | **AINDA VALE** | Alta |
| G7 | a | `app/ponto/management/commands/gerar_celulas.py:105` — O146/L-020 | A extra declarada congela no DNA. A chave é produzida em `app/ponto/management/commands/gerar_celulas.py:119`; com célula existente o leitor retorna a extra desse DNA ou zero, e só sem célula consulta o template (`app/escala/models.py:1234`, `app/escala/models.py:1239`). O motor chama esse leitor em `app/ponto/motor_calculo_v2.py:1356`. | **AINDA VALE** | Alta |
| G8 | a | `app/ponto/management/commands/gerar_celulas.py:146` — corte K6 | O gerador enumera candidatas e entrega o julgamento da competência ao juiz de folha. Importa e chama `competencia_fechada_para_celula` em `app/ponto/management/commands/gerar_celulas.py:152`, `app/ponto/management/commands/gerar_celulas.py:173`. O juiz combina TXT entregue e holerite da pessoa com lote publicado (`app/folha/export.py:599`, `app/folha/export.py:603`). | **AINDA VALE** | Alta |
| G9 | a | `app/ponto/portas/celula.py:147` — corte horizonte | Na regeneração de vínculo, o horizonte é estendido até o fim da competência pela autoridade de janela. Com o padrão `estender_horizonte=True`, consulta `janela_atual` e só aumenta `ate` (`app/ponto/portas/celula.py:171`). A correção de um dia desliga expressamente essa política (`app/ponto/portas/celula.py:484`); falha da consulta mantém a janela recebida (`app/ponto/portas/celula.py:178`). | **AINDA VALE**, com as ressalvas do próprio código | Alta |
| G10 | a | `app/ponto/portas/celula.py:453` — corte “um dia, e só ele” | O invólucro passa `[dia,dia]` e desliga extensão do horizonte em `app/ponto/portas/celula.py:482`. A escrita de células ocorre pelo laço limitado por `desde`/`ate` (`app/ponto/portas/celula.py:208`). O efeito financeiro pode envolver a competência porque o recálculo recebe data representativa por competência (`app/ponto/portas/celula.py:390`); a frase vale para a janela de regeneração de células. | **AINDA VALE**, nesse escopo | Alta |

Complemento útil de G1: a reescrita fora da vigência entra em `_reescritos` (`app/ponto/portas/celula.py:236`), mas o marcador de órfãs consulta `regenerada_em` e chama `cobranca_orfa` com janelas vazias (`app/ponto/management/commands/gerar_celulas.py:241`, `app/ponto/management/commands/gerar_celulas.py:249`). Sem carimbo anterior suficiente, esse ramo também não marca a cobrança como órfã (`app/escala/services/regeneracao.py:78`). O comentário CQ-C10 afirma que a porta acabou de gravar esse carimbo (`app/ponto/management/commands/gerar_celulas.py:238`); convém consolidar isso em G1 para não contar duas vezes a mesma causa.

## 05/11 15:41 · sessao e180414e9a8a · - · - · origem=Ronald (tmux)

8 itens tipo **e**, somente leitura, por inspeção estática. Ocorrência em produção e execução dos testes: `[nao verificado]`. [nao verificado]

| Item | Texto | O código faz | VEREDITO | Certeza |
|---|---|---|---|---|
| E1 — 6x1 sem folga | `app/escala/tests/test_f4_template_sem_fase.py:4`: “6x1 sem folga_dia_semana => None”. | O ramo 6x1 sem folga retorna **True** em `app/escala/models.py:835` e `:851`. Contraexemplo: template `tipo_ciclo='6x1'`, `folga_dia_semana=''`, qualquer data ⇒ True. O próprio teste agora espera True em `app/escala/tests/test_f4_template_sem_fase.py:41`. | **CONTRADIZ** | Alta, estática |
| E2 — nunca bateu não gera furo | `app/escala/tests/test_furos_diarios.py:62`: “quem NUNCA bateu nunca gera FuroDiario”. | `app/escala/services/furos_diarios.py:43` lê a célula; `:49` aceita dia trabalho, `:54` conta marcos, `:57` devolve linha sem exigir batidas anteriores. `:108` chama a porta, que grava em `app/ponto/portas/celula.py:872`. Contraexemplo: não isento, dia passado dentro do vínculo, ata trabalho com duas lâmpadas apagadas e zero batidas ⇒ linha com previstos=2/furados=2. A conversão de lâmpada apagada para `missing=True` está em `app/escala/services/leitor_celula.py:119` e `:133`; leitura do tipo trabalho em `:374`. | **CONTRADIZ** | Alta, estática |
| E3 — avaliações iguais não repetem trilha | `app/escala/tests/test_termometro_grava_com_trilha.py:3`: “2 avaliacoes com o mesmo resultado = 1 linha na 1a e 0 na 2a”. | Em escore abaixo de `FAIXA_INCERTA`, `app/escala/regua_defesa.py:764` incrementa `regua_dias_baixo` a cada avaliação; `:765` chama a escrita. A porta registra campos diferentes em `app/colaboradores/services/vinculo.py:993` e `:998`. Contraexemplo: duas avaliações com o mesmo escore suspeito e mesma classe, permanecendo abaixo do limite, mudam o contador e geram duas trilhas. A exceção é explicitada em `app/colaboradores/services/vinculo.py:978`. | **CONTRADIZ** para a universal literal; mantém-se apenas quando todos os campos de estado, inclusive contador, ficam iguais | Alta, estática |
| E4 — lavra contínua de folga é aditiva | `app/escala/tests/test_foto_folga_continua.py:16`: grava “SO onde o calendario de hoje nao manda apagar nada”. | A medição exclui mês com sobra em `app/escala/management/commands/lavrar_foto_folgas.py:123`; o `apply` recalcula e recusa sobra novamente em `:167`, só chamando o escritor em `:170`. | **AINDA VALE** para esse comando | Alta, estática |
| E5 — parcial só dispensa dentro do período | `app/escala/tests/test_cobparc7_legacy_carrega_parcial.py:5`: parcial carrega os marcadores e “so dispensa as celulas DENTRO do periodo”. | `app/escala/utils.py:1251` grava `cobertura_parcial`; `:1255` calcula período; `:1265` recusa ausência sem período; `:1269` só substitui célula `missing` cujo deslocamento é julgado `dentro`. O legacy preserva o dict com `{**sd}` em `:1064`. A ata transporta os dois marcadores em `app/ponto/services/cartorio.py:732`. | **AINDA VALE**, com a precisão de que o overlay dispensa os marcos faltantes dentro do período | Alta, estática |
| E6 — Art.62 não acusa | `app/escala/tests/test_art62_nunca_acusa.py:2` e `:4`: “NUNCA ACUSA”, nem alerta nem falta; builder/leitor usam a mesma função (`:6`). | `app/escala/utils.py:850` torna a batida real `status='ok'`, preservando hora/delta; `:858` faz missing virar `ok`/não missing. Builder chama em `:910`; leitor importa/chama em `app/escala/services/leitor_celula.py:212`, após gate de isenção em `:412`. | **AINDA VALE** para os dois caminhos explicitamente nomeados; universal sobre todas as fontes de todo o sistema `[nao verificado]` | Alta no escopo |
| E7 — batida não escreve fase | `app/escala/tests/test_f2_batida_nao_escreve.py:1`: “BATIDA NUNCA escreve fase”. | O helper auditado é no-op, `return 0`, em `app/escala/utils.py:209` e `:214`. A pesquisa de referências de produção encontrou apenas a definição e documentação do helper; o caminho `recalibrar_ancora_retroativa` continua separado em `app/escala/utils.py:2114` e é chamado pelo serviço de vínculo em `app/colaboradores/services/vinculo.py:299`. | **AINDA VALE** quanto ao calibrador de batidas aposentado; prova global de ausência de qualquer outro escritor por batida `[nao verificado]` | Alta no helper; média na universal |
| E8 — alimentação de escalas é lista inteira | `app/escala/tests/test_escalas_alimentadas_do_dia.py:14`: “LISTA INTEIRA DE VINCULOS, NUNCA UMA JANELA”; `:21`: lista vazia significa ausência de vínculo. | Sem alimentação, `app/escala/utils.py:2200` consulta todas as escalas do colaborador sem filtro de data/ativa; alimentada, usa exatamente `rows=list(escalas)` em `:2207`; lista vazia retorna None em `:2208`. Minutos previstos também retorna 0 antes de consultar a célula em `:2277`. O chamador tem de obedecer ao contrato: a função não detecta subconjunto. | **AINDA VALE** como contrato e semântica de `[]`; validação de completude não existe nesse corpo | Alta, estática |

Observação para consolidação: E6/E7 têm linguagem universal; não converter evidência dos caminhos nomeados em prova de todos os consumidores. E3 pode ser classificado **NAO VALE MAIS** se “mesmo resultado” for interpretado como diagnóstico/escore, mas **CONTRADIZ** é o confronto literal do enunciado sem a exceção que a implementação documenta em `app/colaboradores/services/vinculo.py:978`.

## 05/11 15:41 · sessao e6e01d9429a7 · - · - · origem=Ronald (tmux)

Validado: o arredondamento separado em `app/ponto/motor_calculo_v2.py:1194` e `app/ponto/motor_calculo_v2.py:1196` pode produzir soma superior ao teto calculado em `app/ponto/motor_calculo_v2.py:1188`. Cabe **CONTRADIZ**, com certeza alta sobre o contrato aritmético local.

Ressalva: atraso300/antecipada300 não prova alcançabilidade integrada; ambas as distâncias superiores a180 acionam L-084 (`app/ponto/motor_calculo_v2.py:1947`). Apresente como contraexemplo unitário: excesso de0,01min=0,6s; ocorrência e efeito financeiro em produção [não verificado].

Alternativa sem ambas as parcelas acima de180: atraso150, antecipada150, previsto660, trabalhado399min04s. O teto é260,933333min; o rateio arredonda para130,47+130,47=260,94min, excesso de0,4s (`app/ponto/motor_calculo_v2.py:1188`, `app/ponto/motor_calculo_v2.py:1191`). A alcançabilidade desse conjunto pela cadeia completa continua [não verificado].

## 05/11 15:41 · sessao 3be61a6443c6 · - · - · origem=Ronald (tmux)

10 itens para consolidação; leitura estática, sem inferir ocorrência em produção `[nao verificado]`.

| Item | Tipo | Texto | O código faz | Veredito | Certeza |
|---|---|---|---|---|---|
| M1 — Jornada padrão 720 | c | `app/escala/models.py:49`: `minutos_jornada` nasce com 720. | O campo mantém `default=720` em `app/escala/models.py:49`; a porta substitui o valor na criação de personalizada ou comercial/partido em `app/escala/services/cadastro_tipo.py:278` e `app/escala/services/cadastro_tipo.py:282`. Portanto é default do modelo, não jornada universal. | AINDA VALE | Alta |
| M2 — Limite de 240 no piso do intervalo | c | `app/escala/models.py:19`: só acima de 240 minutos passa ao piso de 15; o texto explicativo está em `app/escala/models.py:9`. | `>360 → 60`, `>240 → 15`, restante `0`, em `app/escala/models.py:17`; o validador consome essa função em `app/escala/models.py:594`. Exatamente 240 resulta em zero. | AINDA VALE | Alta |
| M3 — Pausa de até 240 no rótulo | c | `app/escala/models.py:556`: pausa coerente de até 240 minutos recebe `PAUSA`. | Calcula duração circular e exige `0 < span <= 240`, em `app/escala/models.py:562` e `app/escala/models.py:565`. O literal rege o rótulo; não estabelece sozinho o desconto de folha. | AINDA VALE | Alta |
| M4 — `tipo_base` determina o motor | d | `app/escala/models.py:174`: “Tipo base determina qual subrotina do motor de cálculo usar.” | A base continua sendo entrada da fábrica em `app/ponto/motor_calculo_v2.py:3623`, mas `turno_partido` com horários completos e vão de até 120 minutos é desviado para `MotorComercial` ou `Motor12x36ComEscala`, em `app/ponto/motor_calculo_v2.py:3635` e `app/ponto/motor_calculo_v2.py:3663`; o vão é decidido em `app/escala/servico_jornada.py:148`. A descrição ficou insuficiente após a mudança. | NAO VALE MAIS | Alta |
| M5 — Perfil de apuração governa cálculo | d | `app/escala/models.py:191`: “Perfil de regras de cálculo — se vazio usa o perfil padrão do sistema.” | A fábrica recebe parâmetros e escolhe motor sem consultar essa FK em `app/ponto/motor_calculo_v2.py:3602` e `app/ponto/motor_calculo_v2.py:3671`; a fábrica de CCT resolve por `regua_para` em `app/core/regua_cct.py:458`. A própria lápide registra que o motor não lê perfil em `app/ponto/views.py:2903`. A busca em `.py` de produção encontrou apenas a declaração da FK e essa lápide. | NAO VALE MAIS | Alta |
| M6 — DNA versão 1 | d | `app/escala/models.py:1699`: snapshot “dna_versao 1” emitido por `montar_dna`. | O produtor declara `DNA_VERSAO = 2` em `app/ponto/management/commands/gerar_celulas.py:16` e escreve esse valor em `app/ponto/management/commands/gerar_celulas.py:96`. | NAO VALE MAIS | Alta |
| M7 — Folga padrão copiada para novos vínculos | d | `app/escala/models.py:92`: calendário copiado a novos vínculos do template. | `_copiar_folga_padrao` lê o template e copia se houver calendário e o vínculo ainda não tiver um, em `app/colaboradores/services/vinculo.py:155` e `app/colaboradores/services/vinculo.py:159`; o caminho de vínculo novo chama a cópia em `app/colaboradores/services/vinculo.py:289`. | AINDA VALE | Alta |
| M8 — Feriado vira folga só nos ciclos indicados | d | `app/escala/models.py:898`: `trabalha_em_feriado=False` transforma feriado em folga para 6x1/5x2/semanal. | O conjunto desses ciclos está em `app/escala/models.py:32`; o predicado recusa os demais e consulta feriado pelo lugar do vínculo em `app/escala/models.py:1392`. A aritmética converte apenas `base is True` para `False` em `app/escala/models.py:1370`. Não transforma uma folga prévia em trabalho quando o flag é True. | AINDA VALE | Alta |
| M9 — Indeterminado não é folga | d | `app/escala/models.py:1688`: `None=indeterminado (limbo cobra)`. | `dia_sem_trabalho` não silencia cobrança apenas por `None`; exige `trabalha is False` ou ata com tipo preenchido diferente de trabalho, com exceção da ausência parcial, em `app/chamados/juizes.py:420` e `app/chamados/juizes.py:426`. Vale para o estado indeterminado por si só; “cobra” não prova que determinado chamado foi emitido. Emissão efetiva `[nao verificado]`. | AINDA VALE | Alta no predicado |
| M10 — Célula existente sem pausa não herda pausa viva | a | `app/escala/models.py:1138`: “LEI: celula soberana”; a explicação afirma que célula existente sem marcos não deve ganhar pausa do template. | `intervalo_do_dia` delega à lista de pausas em `app/escala/models.py:1163`; essa lista retorna `()` quando a célula existe sem marcos em `app/escala/models.py:1202` e só consulta o template quando não há célula em `app/escala/models.py:1208`. | AINDA VALE | Alta |

Para M4, o exemplo determinístico é um tipo `turno_partido` com turno 08–17 e intervalo 12–13: o vão de 60 minutos não é partido por `app/escala/servico_jornada.py:148`, e a fábrica escolhe `MotorComercial` por `app/ponto/motor_calculo_v2.py:3663`. É ilustração do texto ultrapassado, não ocorrência de produção `[nao verificado]`.

Não recomendo acusar “variante herda tudo” nesta seleção sem delimitar o caminho: existe `valor_efetivo` recursivo em `app/escala/models.py:806`, mas a porta de criação herda explicitamente apenas ciclo/base em `app/escala/services/cadastro_tipo.py:152` e `app/escala/services/cadastro_tipo.py:172`; os consumidores acessam campos próprios, por exemplo `marcos_do_dia` em `app/escala/models.py:338`. A promessa ampla merece investigação, mas o recorte exato ainda precisa ser estabelecido `[nao verificado]`.

## 05/11 15:41 · sessao 541ab7558349 · QUEIMA-E-CONTRAEX · leitura · origem=Code

Os casos abaixo são **contraexemplos construídos a partir do código**, sem atribuição a colaborador real. Ocorrência na produção, frequência e reprodução integrada: **[nao verificado]**.

A referência exige coerência entre leitores e lavratura do dinheiro no evento; a L-124 permite foto antiga sem fato novo, mas mantém o rejulgamento quando o fato muda (`app/docs/LEIS.md:113`, `app/docs/LEIS.md:122`, `app/docs/LEIS.md:145`).

1. **E1 — Troca de horário compartilhado: DNA novo, ata e DiaPago antigos.**

   **Entrada e ordem:** em 05/11/2026, após 08h, empresa com corte 21, competência aberta, vínculo ativo e célula gerada de 04/11 já julgada e lavrada. Sem veredito humano nem chamado vivo de falta que provoque outro julgamento. Editar o mesmo `TipoEscala` pela porta `salvar_tipo`. Existência desse estado em produção: [nao verificado].

   | Escala | Cadastro anterior; batidas | Correção do cadastro | Previsto anterior → correto |
   |---|---|---|---|
   | 6x1, folga domingo | 07–11; E07/S11 | 07–16, pausa 12–13 | 240 → **480 min** |
   | 12x36 diurno, trabalho em 04/11 | 07–19, pausa 12–13; E07/S12/E13/S19 | Pausa 12–13:30 | 660 → **630 min** |
   | 12x36 noturno, trabalho em 04/11 | 04/11 E19; 05/11 S00/E01/S07 | Pausa 00–01:30 | 660 → **630 min** |
   | Personalizado sem intervalo | Quarta 08–11, duração `"0"`; E08/S11 | Quarta 08–12, duração `"0"` | 180 → **240 min** |

   Os valores decorrem dos marcos menos as pausas; até quatro horas o piso de intervalo codificado é zero (`app/escala/utils.py:2290`, `app/escala/utils.py:2326`, `app/escala/models.py:17`).

   **Caminho:** salvar o template → percorrer vínculos ativos → regenerar com `recalculo=False` → substituir DNA e registrar regeneração. Os blocos de julgamento e recálculo financeiro ficam condicionados a `recalculo=True` (`app/escala/services/cadastro_tipo.py:306`, `app/escala/services/cadastro_tipo.py:334`, `app/escala/services/cadastro_tipo.py:348`, `app/ponto/portas/celula.py:260`, `app/ponto/portas/celula.py:360`, `app/ponto/portas/celula.py:384`).

   **Código × regra:** o previsto consultado pelos marcos passa ao número novo da tabela; a grade lavrada prefere o número antigo da ata, e o DiaPago permanece anterior. Deveriam representar o cadastro corrigido de forma coerente (`app/escala/models.py:1124`, `app/escala/utils.py:2290`, `app/escala/services/leitor_celula.py:376`, `app/docs/LEIS.md:71`, `app/docs/LEIS.md:122`).

   A invalidação só zera `FechamentoMensal.derivado_em`; não atualiza números nem `previsto_em`. A prontidão ainda aceita o previsto materializado por esse segundo carimbo (`app/escala/services/cadastro_tipo.py:357`, `app/ponto/services/fechamento.py:1636`, `app/folha/export.py:1019`).

   **Certeza:** alta sobre a omissão; os testes existentes exigem ausência de julgamento e de chamada financeira nesse modo. Não foram executados nesta auditoria [nao verificado] (`app/ponto/tests/test_ordem_regen_julga_antes_do_dinheiro.py:108`, `app/ponto/tests/test_r6_dinheiro_acompanha_cadastro.py:107`). **Dono do defeito: ESTRUTURA**, pela manutenção de projeções incompatíveis com o mesmo cadastro (`app/docs/LEIS.md:122`).

2. **E2 — Corrigir uma saída atualiza a célula sem atualizar o dinheiro.**

   **Entrada e ordem:** 6x1, 04/11/2026, previsto 08–17 com pausa 12–13; batidas E08/S12/E13/S17, ata e DiaPago inicialmente com 480 minutos realizados e 8h trabalhadas. Depois, regularização externa confirmada, sem disputa aberta, contendo apenas `corrigir`: S17 → S16. Estado inicial em produção: [nao verificado].

   **Caminho:** a tela aceita correção e alvo; a porta grava diretamente `tipo` e `timestamp`. O `post_save` julga a célula, mas o ramo termina sem chamar o evento financeiro (`app/chamados/views_regularizacao.py:102`, `app/chamados/views_regularizacao.py:128`, `app/chamados/services/regularizacao_ext.py:51`, `app/chamados/services/regularizacao_ext.py:149`, `app/ponto/signals.py:131`, `app/chamados/services/regularizacao_ext.py:197`).

   **Código × regra:** dedução do caminho: realizado atual **420 min**, DiaPago conservando **480 min/8h**, até outro recálculo. A ata recebe o realizado atual da grade; o DiaPago só recebe esse mapa na lavratura financeira. O valor deveria acompanhar a correção no evento (`app/escala/utils.py:2066`, `app/escala/utils.py:2079`, `app/ponto/services/cartorio.py:706`, `app/ponto/services/fechamento.py:1197`, `app/ponto/services/dia_pago.py:288`, `app/docs/LEIS.md:113`).

   **Leitores:** célula/ata atual versus DiaPago anterior. Espelho e app continuam lendo o DiaPago anterior; aqui a comparação segura é **realizado contra realizado**, sem confundir horas cruas com horas pagas (`app/ponto/services/espelho.py:406`, `app/ponto/services/espelho.py:805`, `app/api/views.py:1539`).

   **Dependência da composição do lote:** se houver também uma adição legítima na mesma competência, ela passa por `registrar_batida`, agenda o recálculo no commit e reprocessa a competência inteira. Corrigir sozinho e corrigir junto de uma adição percorrem caminhos financeiros diferentes (`app/chamados/services/regularizacao_ext.py:76`, `app/ponto/registro_batida.py:160`, `app/ponto/services/fechamento.py:1793`, `app/ponto/services/fechamento.py:1808`).

   **Variante entre datas:** mover as quatro batidas de 04/11 para 28/10 por quatro itens `corrigir` deixa a origem fora dos julgamentos disparados: o signal usa apenas a **nova data e sua véspera**. A ata de 04/11 pode conservar os 480 minutos embora suas quatro batidas tenham mudado de data; reprodução integrada [nao verificado] (`app/chamados/services/regularizacao_ext.py:149`, `app/ponto/signals.py:108`, `app/ponto/signals.py:135`).

   **Certeza:** alta sobre os gatilhos ausentes; números integrados [nao verificado]. **Dono: ESTRUTURA**, pela propagação incompleta de uma correção aceita (`app/docs/LEIS.md:122`).

3. **E3 — Retroativo: a impressão reconhece a correção, mas o detector responde “sem fato novo”.**

   **Entrada e ordem:** mesmo 6x1 08/12/13/17, agora em 15/10; quatro batidas criadas naquele dia, célula julgada em 16/10. Sem ausência, justificativa ou alteração posterior do DNA. Em 05/11, corrigir S17 → S16 pela regularização externa; competência sem tranca ou impedimento. Ocorrência real: [nao verificado].

   **Caminho:** a correção salva somente `tipo` e `timestamp`; a impressão inclui ambos. Entretanto, `houve_fato_novo` procura nas batidas apenas `criado_em` ou `retratada_em`, sem ler a auditoria ou `item.aplicada_em` (`app/chamados/services/regularizacao_ext.py:155`, `app/chamados/services/regularizacao_ext.py:159`, `app/chamados/services/regularizacao_ext.py:162`, `app/ponto/services/cartorio.py:453`, `app/ponto/services/cartorio.py:352`).

   **Código × regra:** para essas entradas, impressão **diferente**, detector **False**. Como os marcos já venceram antes de 16/10, a guarda temporal também não identifica fato novo. A L-124 exige tratar a mudança efetiva de batida como fato, preservando o rejulgamento retroativo (`app/ponto/services/cartorio.py:365`, `app/ponto/services/cartorio.py:417`, `app/docs/LEIS.md:145`, `app/docs/LEIS.md:149`).

   **Leitor divergente:** se um chamado do dia foi encerrado via ADMIN em 16/10, seu portão devolve `admin_sem_fato_novo` antes de avaliar a premissa atual. Isso comprova divergência de reconhecimento do fato; quantidade de chamados ou dinheiro afetado [nao verificado] (`app/chamados/models.py:1292`).

   **Certeza:** alta por leitura estática do predicado. **Dono: ESTRUTURA**, pois a correção aparece numa autoridade e desaparece na outra (`app/ponto/services/cartorio.py:453`, `app/ponto/services/cartorio.py:353`, `app/docs/LEIS.md:122`).

4. **E4 — Personalizado sem intervalo: cinco horas recebem zero de intrajornada suprimida.**

   **Entrada e ordem:** empresa com CLT declarado, personalizado de base `comercial`, quarta 08–13, sem `hii/hfi`, duração mínima cadastrada de 15 minutos, sem perfil excepcional; vínculo e célula válidos. Registrar E08/S13, sem pausa, e apurar. Cadastro real correspondente: [nao verificado]. A porta aceita base comercial e o cadastro por duração valida o piso de 15 minutos (`app/escala/services/cadastro_tipo.py:163`, `app/escala/models.py:608`, `app/escala/models.py:19`).

   **Caminho:** fábrica escolhe `MotorComercial`; o período calcula 300 minutos, mas só calcula a supressão quando a duração excede 360 (`app/ponto/motor_calculo_v2.py:3572`, `app/ponto/motor_calculo_v2.py:3668`, `app/ponto/motor_calculo_v2.py:3213`, `app/ponto/motor_calculo_v2.py:3232`).

   **Código × regra:** **300 min trabalhados + 0 min indenizados**, contra **300 min trabalhados + 15 min de supressão**, pelo art. 71 conforme codificado na própria cópia. Executei o método real isolado em memória, com contexto neutro; integração completa [nao verificado] (`app/ponto/motor_calculo_v2.py:3226`, `app/ponto/motor_calculo_v2.py:3242`, `app/escala/models.py:19`).

   **Leitores:** não demonstrei divergência entre telas; podem concordar no valor errado. O calculador paralelo também zera a supressão quando `trab <= 360`, portanto sua concordância não certifica a regra (`app/ponto/calculador/regras.py:760`, `app/ponto/calculador/regras.py:764`, `app/docs/LEIS.md:136`). **Certeza alta no método isolado; dono ESTRUTURA**, pelo ramo que omite o piso codificado (`app/ponto/motor_calculo_v2.py:3232`, `app/docs/LEIS.md:122`).

Não afirmo TXT efetivamente emitido com esses erros: sua fonte é o fechamento gravado, mas a inclusão depende da classificação e dos portões; emissão concreta [nao verificado] (`app/folha/export.py:695`, `app/folha/export.py:700`). Também não classifiquei a foto antiga, sozinha, como defeito: esse custo está expressamente permitido pela L-124 (`app/docs/LEIS.md:145`).

| Caso | Resposta do código × resposta exigida | Divergência demonstrada | Certeza | Dono L-099 |
|---|---|---|---|---|
| E1 — lote, quatro escalas | Novo previsto no DNA; previsto anterior na ata/DiaPago × projeções coerentes | DNA × ata × DiaPago (`app/ponto/portas/celula.py:360`, `app/escala/services/leitor_celula.py:376`) | Alta estática; produção [nao verificado] | ESTRUTURA (`app/docs/LEIS.md:122`) |
| E2 — corrigir S17→S16 | Ata 420, DiaPago 480 × atualizar a lavra no evento | Célula × DiaPago/espelho/app (`app/ponto/services/cartorio.py:706`, `app/api/views.py:1539`) | Alta do caminho; integração [nao verificado] | ESTRUTURA (`app/docs/LEIS.md:122`) |
| E2 — mover entre datas | Origem fora do julgamento × reavaliar origem e destino | Batidas atuais × ata da origem (`app/ponto/signals.py:108`) | Alta da omissão; integração [nao verificado] | ESTRUTURA (`app/docs/LEIS.md:122`) |
| E3 — retroativo | Impressão muda, fato novo=False × reconhecer a correção | Impressão × portão do chamado (`app/ponto/services/cartorio.py:453`, `app/chamados/models.py:1292`) | Alta estática; impacto monetário [nao verificado] | ESTRUTURA (`app/docs/LEIS.md:122`) |
| E4 — personalizado 5h | Intra 0 × 15 min suprimidos | Código × piso do art. 71 codificado (`app/ponto/motor_calculo_v2.py:3232`, `app/escala/models.py:19`) | Método isolado executado; integração [nao verificado] | ESTRUTURA (`app/docs/LEIS.md:122`) |

## 05/11 15:40 · sessao e7452648085a · F3-BECO-1 · verificador · origem=Code

1. **Achado 4 — fechado no caminho normal de aplicação; gravidade: não procede manter esse bloqueio.** A revalidação agora ocorre dentro do `atomic` por pai, usando `qs_na_fila` sobre a linha travada; a costura acontece antes de sair desse bloco. (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:99`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:100`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:105`)

   A nomenclatura precisa é: `limbo_folgas.reatar_perguntas_ao_limbo` delega para `fio.reapontar_resolvedora`; esta chama `_costura_nao_mata_o_fio`, que recarrega e trava **a mesma linha de `ChamadoColaborador`, pelo mesmo `pk` do pai**. (`app/chamados/services/limbo_folgas.py:243`, `app/chamados/services/fio.py:837`, `app/chamados/services/fio.py:840`, `app/chamados/services/fio.py:760`, `app/chamados/services/fio.py:763`)

   A ordem nesse caminho é **pai → escrita da pergunta**. O `atomic(savepoint=False)` interno continua na transação externa; readquirir a própria linha não introduz auto-deadlock. Isso não certifica ausência de deadlocks envolvendo outros caminhos concorrentes: **[não verificado]**. (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:99`, `app/chamados/services/fio.py:837`, `app/chamados/services/fio.py:847`)

   **Não aparece a incompatibilidade SQL suspeitada:** `_base` evita `montar_qs_filtrado`, onde estão `select_related`, anotações e `Window`; a consulta travada recebe somente os filtros locais de `pk`, `status_local` e `silenciado_ate`, sem `distinct`. Compilação/execução efetiva em PostgreSQL: **[não verificado]**. (`app/chamados/services/painel.py:98`, `app/chamados/services/painel.py:111`, `app/chamados/services/painel.py:153`, `app/chamados/services/painel.py:155`, `app/chamados/services/painel.py:186`, `app/chamados/services/painel.py:188`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:100`)

   Quanto aos escritores concorrentes:

   - **Silêncio:** `arquivo.silenciar` e `arquivo.levantar` escrevem `silenciado_ate`. Não fazem leitura prévia com `select_for_update`; fazem `save` na própria linha do chamado. Logo, seu UPDATE disputa a linha já travada pelo comando — a proteção não exige que ambos executem explicitamente `SELECT FOR UPDATE`. Comprovação concorrente executada: **[não verificado]**. (`app/chamados/services/arquivo.py:166`, `app/chamados/services/arquivo.py:170`, `app/chamados/services/arquivo.py:201`, `app/chamados/services/arquivo.py:208`)
   - **Estado:** mudanças persistidas passam por `_gravar_estado`. Reconciliação e encerramentos recarregam sob trava; humana/analisar/reabrir não têm universalmente essa leitura travada, mas a escrita final também atinge a mesma linha do pai. (`app/chamados/models.py:222`, `app/chamados/models.py:279`, `app/chamados/models.py:638`, `app/chamados/models.py:674`, `app/chamados/models.py:433`, `app/chamados/models.py:769`, `app/chamados/models.py:1382`, `app/chamados/models.py:1532`)

2. **Teste com `_grava` — morde a versão sem revalidação, por leitura; gravidade: não procede a objeção à posição do patch.** A sequência é seleção inicial → `_grava` → revalidação → costura. O patch silencia depois da gravação real da reversão, portanto dentro da janela questionada. Sem a revalidação, o gate do fio aceitaria o pai não terminal, pois não consulta silêncio; falhariam a mensagem esperada e a preservação do destino original. (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:59`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:91`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:100`, `app/chamados/tests/test_f3_beco1_reata_limbo.py:138`, `app/chamados/tests/test_f3_beco1_reata_limbo.py:145`, `app/chamados/tests/test_f3_beco1_reata_limbo.py:146`, `app/chamados/services/fio.py:765`)

   **Limitação do teste — gravidade: fila:** ele prova a reconsulta, não a trava. Uma reconsulta sem `select_for_update` também enxergaria o silêncio escrito sequencialmente pelo próprio patch. Execução GREEN/RED e teste concorrente: **[não verificado]**. (`app/chamados/tests/test_f3_beco1_reata_limbo.py:138`, `app/chamados/tests/test_f3_beco1_reata_limbo.py:143`)

   `_grava` é obrigatório antes de qualquer costura no `_rodar` com aplicação normal, inclusive com seleção vazia. “Todo caminho de `--apply`” exige ressalvas: opções inválidas abortam antes; `--reverter` tem precedência e segue outro caminho, sem `_grava`. (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:37`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:39`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:82`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:91`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:94`)

3. **Caso 107 — a troca para 27/09 corrige a mordida; gravidade: não procede manter a crítica ao assert antigo.** A fixture exige zero perguntas passadas antes do lançamento; o wrapper chama a porta real e guarda seu retorno; o teste exige retorno não vazio em 27/09 e pergunta persistida da mesma pessoa/data, com motivo desse retorno. Uma porta substituída por `return []` falha no primeiro desses asserts, mesmo que 22/09 continue ganhando pergunta. (`app/escala/tests/test_f3_beco1_cadastro_entrou.py:110`, `app/escala/tests/test_f3_beco1_cadastro_entrou.py:113`, `app/escala/tests/test_f3_beco1_cadastro_entrou.py:127`, `app/escala/tests/test_f3_beco1_cadastro_entrou.py:129`)

   Na implementação real, essa porta calcula motivos ainda ausentes, chama a emissão com push silenciado e devolve os motivos. A prova do teste é de **ao menos uma pergunta correspondente**; `.exists()` não exige pergunta para cada motivo retornado nem verifica o silêncio do push. (`app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/services/fabrica_da_celula.py:126`, `app/chamados/services/fabrica_da_celula.py:174`, `app/chamados/services/fabrica_da_celula.py:177`, `app/escala/tests/test_f3_beco1_cadastro_entrou.py:129`)

   **Existe outro caminho de criação antes dessa porta.** O corpo do julgamento pode abrir cobrança de furo parcial; `_abrir_chamado` chama `criar_disputa_a_partir_de_chamados_orfaos`, que complementa marcos faltantes e chega a `PerguntaDisputa.abrir`, cujo nascimento efetivo está em `objects.create`. Esse caminho precede o laço de `perguntar_pela_celula`. (`app/ponto/services/cartorio.py:948`, `app/ponto/services/cartorio.py:1777`, `app/ponto/management/commands/detectar_ausencias.py:796`, `app/chamados/services/disputa_emissao.py:1013`, `app/chamados/services/disputa_emissao.py:813`, `app/chamados/services/disputa_emissao.py:770`, `app/chamados/models.py:2465`, `app/ponto/services/cartorio.py:997`)

   Esse é o caminho que, por leitura, explica a pergunta de 22/09 sem passar por `perguntar_pela_celula`; continua passando pela porta final `PerguntaDisputa.abrir`. A docstring relata o nascimento anterior e o dedup posterior por `ja`; confirmação independente do ramo efetivamente executado em 22/09: **[não verificado]**. (`app/escala/tests/test_f3_beco1_cadastro_entrou.py:18`, `app/chamados/services/fabrica_da_celula.py:123`, `app/chamados/services/disputa_emissao.py:770`)

4. **Censo de campos e autoridades — gravidade: fila na divergência documental indicada abaixo.** Os escritores e leitores relevantes à seleção, costura e asserções são estes. (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:86`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:100`, `app/escala/tests/test_f3_beco1_cadastro_entrou.py:129`)

   | Campo | Quem escreve | Quem lê neste fluxo |
   |---|---|---|
   | Pai: `silenciado_ate` | `arquivo.silenciar` / `levantar`; teste injeta UPDATE direto. (`app/chamados/services/arquivo.py:166`, `app/chamados/services/arquivo.py:201`, `app/chamados/tests/test_f3_beco1_reata_limbo.py:140`) | Fila e leitura da instância. (`app/chamados/services/painel.py:188`, `app/chamados/models.py:480`) |
   | Pai: `status_local` | `_gravar_estado`; nascimento por `criar`. (`app/chamados/models.py:222`, `app/chamados/models.py:279`, `app/chamados/models.py:1048`) | Fila, seleção LIMBO e gate do fio. (`app/chamados/services/painel.py:155`, `app/chamados/services/painel.py:187`, `app/chamados/services/limbo_folgas.py:219`, `app/chamados/services/fio.py:764`) |
   | Pai: `pk`, `colaborador`, `modulo_origem`, `subcategoria` | Nascimento do chamado com colaborador e campos recebidos. (`app/chamados/models.py:1048`) | Seleção do pai LIMBO e recarga pelo `pk`. (`app/chamados/services/limbo_folgas.py:217`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:100`) |
   | Pergunta: `pk`, `chamado_destino` | Nascimento; costura; **também reapontamento em `PerguntaDisputa.abrir`**. (`app/chamados/models.py:2465`, `app/chamados/services/fio.py:845`, `app/chamados/models.py:2439`, `app/chamados/models.py:2463`) | Reversão e idempotência. (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:86`, `app/chamados/services/limbo_folgas.py:239`) |
   | Pergunta: `disputa`, `motivo`, `data_evento`, `celula`; disputa: `colaborador` | Nascimento; atualização de célula; preenchimento tardio da data pela costura. (`app/chamados/models.py:2465`, `app/chamados/models.py:2425`, `app/chamados/services/fio.py:832`, `app/chamados/services/disputa_emissao.py:523`) | Identidade, dedup da fábrica, validação de resolvedora e assert novo. (`app/chamados/models.py:2381`, `app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/juizes.py:1905`, `app/escala/tests/test_f3_beco1_cadastro_entrou.py:129`) |
   | Pergunta: `veredito_lavrado`, `julgado_em`, `janela_expira_em` | `lavrar` calcula; costura persiste junto ao destino. (`app/chamados/veredito_lavrado.py:227`, `app/chamados/services/fio.py:847`) | Validade da lavra. (`app/chamados/veredito_lavrado.py:179`) |
   | Célula: `data`, `colaborador`, `trabalha`, `escala_geradora`, `dna`, `regenerada_em` | Geração/regeneração; fixture. (`app/ponto/portas/celula.py:253`, `app/ponto/portas/celula.py:266`, `app/escala/tests/test_f3_beco1_cadastro_entrou.py:65`) | Regeneração, fábrica e foto da exportada. (`app/ponto/portas/celula.py:257`, `app/chamados/services/fabrica_da_celula.py:91`, `app/chamados/services/fabrica_da_celula.py:118`, `app/escala/tests/test_f3_beco1_cadastro_entrou.py:123`) |
   | Célula: `veredito`, `veredito_via`, `protestos`, `ata`, `julgada_em` | Porta da célula; fixture força `indefinida`. (`app/ponto/portas/celula.py:798`, `app/ponto/portas/celula.py:830`, `app/escala/tests/test_f3_beco1_cadastro_entrou.py:71`) | Fábrica, avaliação do estado e preservação da exportada. (`app/chamados/services/fabrica_da_celula.py:99`, `app/chamados/services/fabrica_da_celula.py:102`, `app/escala/tests/test_f3_beco1_cadastro_entrou.py:78`, `app/escala/tests/test_f3_beco1_cadastro_entrou.py:123`) |

   `JUIZES.md` atribui estado/transição e conjuntos de vida/cobrança ao catálogo, fio/disputa a `_disputa_do_chamado`, folga lançada à precedência e vínculo/cobertura à alimentação. A revalidação reutiliza o predicado existente de fila; o lookup não enumera especificamente `qs_na_fila` nem os escritores do silêncio. (`app/docs/JUIZES.md:37`, `app/docs/JUIZES.md:40`, `app/docs/JUIZES.md:51`, `app/docs/JUIZES.md:90`, `app/docs/JUIZES.md:98`, `app/chamados/services/painel.py:186`)

   **Divergência documental:** a alegação de escritor único do destino no fio não descreve todo o código vivo: `PerguntaDisputa.abrir` também reaponta pergunta existente diretamente, embora consulte as guardas. Isso deve constar no censo; isoladamente não demonstra novo bloqueio deste delta. (`app/chamados/services/fio.py:787`, `app/chamados/models.py:2436`, `app/chamados/models.py:2439`, `app/chamados/models.py:2463`)

**Achado 3 anterior — permanece bloqueia, por leitura:** fechar o achado 4 não resolve a exceção por dia capturada no re-julgamento, enquanto novas solicitações dependem da transição mensal do defeito. Recuperação efetiva em produção: **[não verificado]**. (`app/ponto/portas/celula.py:402`, `app/ponto/portas/celula.py:405`, `app/escala/folgas.py:220`, `app/escala/folgas.py:227`)

## 05/11 15:39 · sessao bbd7b5c641df · - · - · origem=Ronald (tmux)

Consolidação de 10 itens. Certeza refere-se à leitura estática; execução em produção e valores efetivamente pagos são [não verificado].

| Item | Tipo | Texto | Código | Veredito | Certeza |
|---|---|---|---|---|---|
| F1 | d | `app/ponto/models.py:757`: falta de dia inteiro = 720 minutos | `app/ponto/models.py:875-885`; `app/folha/export.py:110`; `app/ponto/services/fechamento.py:865-866` | NAO VALE MAIS | Alta |
| F2 | d | `app/folha/models.py:97`: `invalidada_em` nulo = TXT vale | `app/folha/models.py:134-137` | CONTRADIZ | Alta |
| F3 | a | `app/folha/porta_export.py:417-419`: motor não roda sobre competência paga; lavratura fica defasada para sempre | `app/ponto/services/fechamento.py:36-43`, `:551-566`, `:277`, `:745-746` | CONTRADIZ | Alta |
| F4 | a | `app/ponto/services/dia_pago.py:188-189`: crédito parcial abate atraso no próprio dia | Mesmo arquivo `:190-195` | AINDA VALE | Alta |
| F5 | a | `app/ponto/services/dia_pago.py:197-210`: folga certa tem rubrica própria; sem escala certa preserva hora; ambas levam noturno/intra, sem atraso/antecipada/HE duplicada | Mesmo arquivo `:223-240`, `:257-265` | AINDA VALE | Alta |
| F6 | a | `app/ponto/services/dia_pago.py:241-243`: folga em feriado vai para HE100 feriado, mantendo noturno/intra | Mesmo arquivo `:244-256` | AINDA VALE | Alta |
| F7 | a | `app/folha/export.py:413-426`: HE noturna é subconjunto e não gera linha adicional | Mesmo arquivo `:428-441` | AINDA VALE | Alta |
| F8 | a | `app/folha/export.py:442-449`: reflexo DSR não é evento de importação | Mesmo arquivo `:428-450` | AINDA VALE | Alta |
| F9 | e | `app/folha/tests/test_furo_so_intervalo_nao_retem.py:1-30`: somente pausa faltante não retém; dúvida não vira isenção | `app/folha/export.py:1124-1129`, `:1147`, `:1235-1236` | AINDA VALE | Alta |
| F10 | e | `app/folha/tests/test_pdf_export_conservacao.py:8-11`: cadastro serve somente para tradução; valores vêm do TXT | `app/folha/pdf_export.py:63`, `:87-101`, `:111-125`, `:147-150` | AINDA VALE | Alta |

**F1 — texto antigo de 720 minutos.** `Ausencia.save()` deriva uma falta aprovada com `minutos=0` somando o previsto de cada dia; não fixa 720 (`app/ponto/models.py:875-885`). Exemplo dedutível: dia único cujo juiz devolva 480 grava 480, e o valor chega ao fechamento pelo rateio/mapa (`app/folha/export.py:110`, `app/ponto/services/fechamento.py:865-866`). Caso real com esse cadastro é [não verificado].

**F2 — nulo não determina vigência.** `e_vigente` seleciona o registro mais recente por `gerado_em` e `pk`, sem consultar `invalidada_em` (`app/folha/models.py:134-137`). Contraexemplo: dois registros da mesma empresa/competência, ambos não invalidados; o antigo tem `invalidada_em=NULL`, mas `e_vigente=False` (`app/folha/models.py:134-137`). Na direção inversa, invalidar o último sem criar substituto mantém seu `pk` como último; `invalidar()` preenche a invalidação, enquanto o predicado de vigência continua ignorando esse campo (`app/folha/models.py:158-162`, `:134-137`).

**F3 — congelamento geral da exportada ficou no comentário.** A guarda atual nomeia somente setembro/2026 e exclui colaboradores apenas quando a competência está nessa guarda (`app/ponto/services/fechamento.py:36-43`, `:551-566`). Fora dela, o fluxo chega à apuração por fatia e a `motor.calcular_mes()` (`app/ponto/services/fechamento.py:745-746`, `:277`). Contraexemplo dedutível: outubro/2026 exportada, com colaborador elegível, vínculo e batidas válidos, segue para cálculo; a exportação por si não o impede (`app/ponto/services/fechamento.py:551-566`, `:745-746`). A remoção da guarda geral também está explicitada na documentação da própria função (`app/ponto/services/fechamento.py:476-488`).

**F4 — crédito limitado ao dia.** O código busca crédito pela mesma chave de data, limita o abatimento ao atraso daquele dia e impede resultado negativo (`app/ponto/services/dia_pago.py:190-195`).

**F5 — partição da folga.** A folga certa alimenta `horas_folga_trabalhada`; a sem escala alimenta `horas_trabalhadas`; a função compartilhada acrescenta somente noturno e intra (`app/ponto/services/dia_pago.py:223-240`, `:257-265`). Os laços não transportam os campos de atraso, antecipada e HE dos períodos de folga (`app/ponto/services/dia_pago.py:234-265`).

**F6 — feriado segregado.** O laço `ft_feriado` adiciona a mesma quantidade ao total HE100 e ao subconjunto HE100 feriado, aplica noturno/intra e registra o fato em `folga_feriado_min` (`app/ponto/services/dia_pago.py:244-256`). Esses campos total/subconjunto não representam duas horas distintas no emissor: ele subtrai o subconjunto para obter HE100 normal (`app/folha/export.py:409-412`).

**F7 — sem duplicação de HE noturna no emissor.** A lista emitida contém HE50, HE100 normal, HE100 feriado e adicional noturno; não emite `horas_extras_50_noturna` ou `horas_extras_100_noturna` como eventos extras (`app/folha/export.py:428-441`). A declaração histórica sobre holerites externos continua [não verificado]; a seleção de campos atual está comprovada no código (`app/folha/export.py:428-441`).

**F8 — DSR ausente da importação.** A lista retornada por `eventos_do_fechamento` não usa `horas_reflexo_dsr` nem emite evento `dsr` (`app/folha/export.py:428-450`). O cálculo efetivo realizado pelo sistema externo é [não verificado].

**F9 — isenção estreita da pausa.** A folha exige resposta do juiz sem `desalinhado`, `em_aberto` ou `nao_sei`; exige pelo menos uma lâmpada apagada, todas pertencentes à pausa; e exige protestos exatamente iguais a `{'FURO_PARCIAL'}` (`app/folha/export.py:1124-1129`, `:1147`). A retenção consulta esse predicado e não altera a célula nessa decisão (`app/folha/export.py:1235-1236`). A docstring inteira qualifica a abertura: dia misto ou dúvida não ganha essa isenção (`app/folha/tests/test_furo_so_intervalo_nao_retem.py:18-30`).

**F10 — PDF traduz os valores do TXT.** O caminho do registro chama o tradutor com `reg.conteudo`; o valor bruto é extraído da fatia textual da linha e somado sem consultar fechamento ou motor (`app/folha/pdf_export.py:63`, `:113-125`). Os cadastros fornecem tipo/descrição e nome, enquanto a magnitude continua vindo do TXT (`app/folha/pdf_export.py:87-101`, `:116-119`, `:147-150`). A interpretação da unidade depende do tipo cadastrado; isso já faz parte da tradução (`app/folha/pdf_export.py:117-119`).

## 05/11 15:39 · sessao e6e01d9429a7 · - · - · origem=Ronald (tmux)

Dez candidatos finais. Todos os caminhos abaixo são de `app/ponto/motor_calculo_v2.py`; os contraexemplos são entradas controladas, não ocorrências em produção. Ocorrência em produção: [não verificado].

| Item | Tipo | Texto | Código | Veredito | Certeza |
|---|---|---|---|---|---|
| M1 | a | `:52-61`: LAPIDE promete 10 minutos por marcação/20 por dia sem atraso nem HE. | `:2006-2009` usa 10 por ponta para pontualidade; `:2840-2854` admite HE integral quando excedente supera **10**, não 20. | **CONTRADIZ** | Alta |
| M2 | a | `:2104-2106`: entrada ≥3h depois do início “NÃO é atraso”; fica sem entrada prevista. | `:2122-2142` conserva essa exclusão somente sem marcos de intervalo; `:2148-2167` mantém o marco quando há intervalo declarado. | **NAO VALE MAIS** | Alta |
| M3 | a | `:2807`: dobra de feriado exige dia não previsto. | `:934` exige apenas `feriado_em_dobra` e feriado; `:2809-2813` grava toda duração como HE100. | **CONTRADIZ** | Alta |
| M4 | c | `:3226`: literal **240**, comentado “>4h exige intrajornada”. | `:3226-3242`: entra acima de 240, mas só calcula indenização quando a duração líquida supera **360**. | **CONTRADIZ** | Alta |
| M5 | c | `:3120-3122`: limite legal **480**, preservando jornadas menores. | `:3115-3122`: determina limite pelo previsto e aplica `min(limite_dia, 480)` no regime `legais`. | **AINDA VALE** | Alta |
| M6 | c | `:809-810`: jornada contratual padrão de **720**, quando não existe tipo de escala. | `:810` usa exatamente esse padrão; `:1273-1277` só o utiliza no fallback de ausência de previsto. | **AINDA VALE**, como fallback | Alta |
| M7 | a/c | `:3495-3499`: no plantão, HE conta da oitava hora; jornada legal = `8*60`, **480**. | `:3503-3504` altera apenas `self.jornada_minutos`; `:3511` delega ao 12x36; `:2835-2841` usa previsto do juiz; `:1273-1277` prioriza esse previsto. | **CONTRADIZ**, se lido como teto obrigatório de HE | Alta |
| M8 | a | `:2316-2317`: LAPIDE proíbe herdar fase do template; fase vem da âncora do colaborador. | `:2312-2321` usa âncora do colaborador ou sentinela; `:2369-2373` exige `ancora_confirmada` para classificação por fase. | **AINDA VALE** nesse cálculo | Alta |
| M9 | a | `:1177-1185`: L-093 usa trabalhado real anterior à janela; minuto bloqueado não aumenta desconto. | `:1186-1196` soma `minutos_trabalhados + minutos_fora_janela`, calcula teto e reduz proporcionalmente a pontualidade. | **AINDA VALE** | Alta |
| M10 | a | `:1936-1938`: L-084 exige as duas pontas distantes mais de três horas; `and`, nunca `or`. | `:1946-1949` compara ambas com `>180` e retorna conjunção; `:1988-2005` zera pontualidade quando verdadeiro. | **AINDA VALE** | Alta |

Contraexemplos:

- **M1:** corpo real de `Motor12x36.calcular_periodo`, previsto 720, duração real 732, janela inativa e não feriado: retorna **12 minutos de HE50**, dentro dos 20 prometidos pela LAPIDE. Prova por AST em memória executada; auxiliares forneceram respostas controladas. Fontes: `app/ponto/motor_calculo_v2.py:52`, `app/ponto/motor_calculo_v2.py:2840`, `app/ponto/motor_calculo_v2.py:2845`, `app/ponto/motor_calculo_v2.py:2851`.
- **M3:** dia feriado também presente em `datas_previstas_trabalho`, com `feriado_em_dobra=True` e duração positiva: o helper não consulta previsão e o período recebe HE100 integral. Fontes: `app/ponto/motor_calculo_v2.py:934`, `app/ponto/motor_calculo_v2.py:2809`.
- **M4:** corpo real de `MotorComercial.calcular_periodo`, jornada de **300 minutos sem intervalo**: retorna **zero** de intrajornada indenizada. Prova por AST em memória executada. O piso central para essa duração é 15 minutos, mas não é consultado nesse ramo: `app/escala/models.py:19`; guarda impeditiva: `app/ponto/motor_calculo_v2.py:3232`.
- **M7:** previsto devolvido pelo juiz =1440, duração trabalhada=1440 e intervalo declarado descontado=0: o limite é1440 e o excedente diário é zero; a atribuição de480 ao atributo não prevalece sobre o juiz. Fontes: `app/ponto/motor_calculo_v2.py:1273`, `app/ponto/motor_calculo_v2.py:1803`, `app/ponto/motor_calculo_v2.py:2835`, `app/ponto/motor_calculo_v2.py:3503`.

As provas AST extraíram somente os corpos dos dois métodos e usaram `python3 -B`, sem importar o projeto. São provas locais dos ramos indicados; resultado integrado de folha e presença dessas condições no dado fornecido: [não verificado].

_1685 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
