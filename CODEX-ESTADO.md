# CODEX-ESTADO

> Ultima resposta processada: **17/11/2026 11:24** · publicado em **17/11/2026 11:24** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 17/11 11:24 · sessao 1592a5b9720c · E4C-F2 · verificador · origem=Code

REPROVADO

1. **Bloqueia — o compensador pode não desfazer o estado anterior.** Ele grava `ato_em=agora`, enquanto leitura e SQL ordenam por `ato_em/pk`, incluindo atos futuros. Exemplo: VALIDACAO já existente às 13h; ressuscitação às 12h. A REABERTURA fica antes da validação, que continua vigente na trilha, embora a projeção seja limpa. O mesmo problema permite uma RESPOSTA futura continuar vigente. (`app/chamados/models.py:1662`, `app/chamados/models.py:1673`, `app/chamados/models.py:4273`, `app/chamados/models.py:4279`, `app/chamados/models.py:3530`)

   Isso pode reproduzir a divergência C6 × `q_vivas`: C6 lê a projeção limpa; o filtro SQL ainda encontra validação vigente. Com RESPOSTA futura, a limpeza da materialização também pode fazer a pergunta passar a segurar CSF. Ocorrências no estoque/exportadas: **[nao verificado]**. (`app/chamados/juizes.py:1389`, `app/chamados/models.py:3591`, `app/chamados/models.py:1982`)

2. **Bloqueia — `ato_motivo` continua com função de texto e de discriminador de estado.** Em RESPOSTA, motivo vazio desfaz validação; motivo preenchido conserva. A face SQL repete esse critério. Acrescentar ou retirar uma explicação textual pode, portanto, mudar a validação vigente. (`app/chamados/models.py:4241`, `app/chamados/models.py:4251`, `app/chamados/models.py:3529`)

   Os escritores dependem dessa convenção: resposta humana omite motivo; resposta automática, recuperação pela trilha e reconstrução da projeção o preenchem. Não existe campo explícito para essa diferença. (`app/chamados/services/disputa_emissao.py:1525`, `app/chamados/services/disputa_emissao.py:1678`, `app/chamados/services/disputa_emissao.py:1699`, `app/chamados/models.py:4224`)

   O teste novo chamado `achado5` compara duas **REABERTURAS** com motivo igual. Ele protege a identificação da ressuscitação, mas não ataca a dupla semântica de **RESPOSTA** que motivou a reprovação anterior. (`app/chamados/tests/test_f1nova_b1_juiz_respondida.py:346`, `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:358`)

3. **Bloqueia — RED(b) pode passar sem comparar com a main.** Todas as comparações dependem de `if os.path.exists(GOLDEN)`. Se o arquivo faltar, não ocorre falha nem comparação main × árvore. (`app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:149`)

   Nesta cópia, o golden existe, contém tarefa/fila e CSF não vazios; o teste compara os resultados inteiros quando o encontra. A guarda de CSF vazio tem mordida explícita por `assertIn('P4', ...)`. A tarefa não tem guarda própria contra vazio; fica protegida pela comparação condicional com o golden. (`app/chamados/tests/f2_ressuscitar_tarefa_main.json:9`, `app/chamados/tests/f2_ressuscitar_tarefa_main.json:71`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:147`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:153`)

   O teste cobre três ressuscitações sintéticas. A geração efetiva do golden pela main e a igualdade sobre as 2.025 medidas fornecidas permanecem **[nao verificado]**. (`app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:111`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:146`)

4. **Bloqueia — a cobertura do ato não fecha a matriz fornecida.** A premissa do item 4 não corresponde ao código: pergunta sem validação, instante e texto vigentes executa `continue` e **não recebe REABERTURA**. Isso inclui pergunta sem atos e SILENCIO sem resposta/validação vigentes. Entretanto, a seleção exige materialização e a operação a retira. (`app/chamados/models.py:1543`, `app/chamados/models.py:1668`, `app/chamados/models.py:1680`, `app/chamados/models.py:3457`)

   Portanto não há “ato a mais” nesses casos; há retirada do carimbo sem novo ato por pergunta. A porta comum de reabertura, por comparação, considera expressamente a materialização retirada como condição suficiente para registrar ato. (`app/chamados/services/disputa_emissao.py:1638`, `app/chamados/services/disputa_emissao.py:1646`)

5. **Fila — permanecem leitores em laço sem prefetch de decisões.**

   - **Modal admin, disputa fechada:** o fio chama o explicador sem fornecer perguntas; ele as consulta sem decisões e lê `estado` em laço. O prefetch realizado por `pode_fechar` acontece depois desse laço. (`app/chamados/services/fio.py:348`, `app/chamados/services/explicador.py:160`, `app/chamados/services/explicador.py:171`, `app/chamados/services/explicador.py:95`, `app/chamados/services/explicador.py:177`)
   - **Vigia:** `perg_todas` chega sem decisões; `dono_fora_do_fio` lê `p.estado` em `any`, podendo consultar decisões por pergunta. (`app/chamados/services/vigia_fio.py:570`, `app/chamados/services/vigia_fio.py:615`, `app/chamados/services/vigia_fio.py:259`, `app/chamados/models.py:4267`)

   O teto novo mede exclusivamente `/api/disputa/perguntas/`; não cobre esses caminhos. O caminho principal do fio já carrega decisões e autores em lote. Contagens executadas desses outros caminhos: **[nao verificado]**. (`app/api/tests/test_f2_teto_consultas_app.py:49`, `app/chamados/services/fio.py:263`)

6. **Fila — o dono apresentado pelo explicador muda para colaborador.** Quando a compensação faz a validada ressuscitada passar a PENDENTE, o explicador devolve `dono='colaborador'` e “Cobrar o colaborador”. O modal apresenta “Aguardando o COLABORADOR responder”. A descrição anterior de VALIDADA seguia os ramos de sistema/cadastro. (`app/chamados/models.py:3213`, `app/chamados/services/explicador.py:95`, `app/chamados/services/explicador.py:137`, `app/templates/chamados/partials/modal_fio.html:467`)

   Isso é mudança concreta do **dono descrito**. Não demonstra mudança da fila oficial: o próprio explicador declara seu caráter descritivo, e `CargaDaTarefa` exclui a compensadora antes de escolher o último ato. (`app/chamados/services/explicador.py:174`, `app/chamados/services/tarefa_do_dia.py:426`)

7. **Fila — a foto pode guardar o autor errado em empate de instante.** `_vpor` escolhe a primeira VALIDACAO com o instante vigente. A ordem padrão é crescente por instante/pk, enquanto a leitura faz prevalecer a última. Duas validações no mesmo instante, autores A/B, podem produzir validação vigente B e foto atribuída a A; o modal mostra esse autor. Estoque desse caso: **[nao verificado]**. (`app/chamados/models.py:1670`, `app/chamados/models.py:4165`, `app/chamados/models.py:4273`, `app/chamados/models.py:4279`, `app/templates/chamados/partials/modal_fio.html:212`)

8. **Fila — censo dos campos gravados e lidos.** A nova linha é construída em `ressuscitar_perguntas` e persistida pela porta genérica `registrar`; as fotos legadas RESPOSTA/VALIDACAO são preservadas antes dela. (`app/chamados/models.py:1635`, `app/chamados/models.py:1672`, `app/chamados/models.py:1678`, `app/chamados/models.py:4298`)

   | Campo/valor | Escritor e leitores |
   |---|---|
   | `da_ressuscitacao` | Default falso no modelo/migration; verdadeiro no compensador. O predicado lê tipo + campo; a carga da tarefa usa o predicado. (`app/chamados/models.py:4160`, `app/chamados/migrations/0054_decisao_da_ressuscitacao.py:18`, `app/chamados/models.py:1677`, `app/chamados/models.py:4186`, `app/chamados/services/tarefa_do_dia.py:426`) |
   | `ato=REABERTURA`, `ato_em` | Escritor usa agora. Leitura, SQL, histórico e tarefa ordenam/interpretam o ato; a tarefa descarta a marcada. (`app/chamados/models.py:1673`, `app/chamados/models.py:4273`, `app/chamados/models.py:3530`, `app/chamados/models.py:3779`, `app/chamados/services/tarefa_do_dia.py:422`) |
   | `ato_por` | Compensador grava nulo; o modal mostra autor quando existe. Autor não identifica ressuscitação. (`app/chamados/models.py:1673`, `app/templates/chamados/partials/modal_fio.html:212`, `app/chamados/models.py:4186`) |
   | `ato_motivo` | Compensador grava texto com número do chamado; modal o mostra. A identificação da compensadora não o consulta, mas RESPOSTA ainda o usa para decidir validação. (`app/chamados/models.py:1677`, `app/templates/chamados/partials/modal_fio.html:212`, `app/chamados/models.py:4186`, `app/chamados/models.py:4251`) |
   | `ato_texto` | Recebe resposta vigente da leitura; leitura e histórico consomem o texto. Na REABERTURA ele conserva a foto, enquanto o efeito do tipo limpa a resposta vigente. (`app/chamados/models.py:1674`, `app/chamados/models.py:4269`, `app/chamados/models.py:4283`, `app/templates/chamados/partials/modal_fio.html:212`) |
   | `respondida_original_em`, `validada_original_em`, `validada_original_por` | Compensador copia os valores anteriores; a reabertura comum também grava essas fotos. São conservação histórica, não os campos que a leitura usa para restaurar estado. (`app/chamados/models.py:1674`, `app/chamados/services/disputa_emissao.py:1632`, `app/chamados/models.py:4269`, `app/templates/chamados/partials/modal_fio.html:212`) |
   | `ato_instante` | Não integra a declaração deste modelo; o instante persistido é `ato_em`. Outro significado pretendido: **[nao verificado]**. (`app/chamados/models.py:4139`, `app/chamados/models.py:4141`) |

9. **Fila — censo dos leitores e efeitos.**

   | Leitor | Vê a compensadora? Resultado e alcance |
   |---|---|
   | `DecisaoPergunta.leitura` | Sim; não distingue marca. REABERTURA limpa validação, instante e texto quando vem depois na ordenação. (`app/chamados/models.py:4267`, `app/chamados/models.py:4281`) |
   | `estado`, `respondida`, `resposta_display` | Herdam a limpeza: deixam de conservar validação/resposta antigas; podem devolver PENDENTE, falso e texto vazio. (`app/chamados/models.py:3213`, `app/chamados/models.py:3764`, `app/chamados/models.py:3788`) |
   | `CargaDaTarefa` | Descarta a marcada antes do último ato; REABERTURA comum continua devolvendo ao colaborador. Preservação integral da frota: **[nao verificado]**. (`app/chamados/services/tarefa_do_dia.py:426`, `app/chamados/services/tarefa_do_dia.py:163`) |
   | C6 | Não lê a linha: lê projeção temporal. A limpeza da projeção muda seus insumos. (`app/chamados/juizes.py:1389`, `app/chamados/juizes.py:1404`) |
   | `q_vivas` e fio | A face SQL reconhece REABERTURA como retirada de validação; pode reincluir a ressuscitada no universo largo. O fio usa esse universo em disputas fechadas e depois consulta C6. (`app/chamados/models.py:3529`, `app/chamados/models.py:3591`, `app/chamados/services/fio.py:255`, `app/chamados/services/fio.py:270`) |
   | `perguntas_que_seguram_csf` | Delega a `sem_veredito_entre`, que exige resposta vigente, sem validação nem materialização. A compensadora normalmente impede que uma resposta só na trilha reapareça como bloqueio após a limpeza. Conservação sobre todas as exportadas: **[nao verificado]**. (`app/ponto/supra_juiz.py:573`, `app/chamados/models.py:1982`) |
   | `materializacao.py` | O rótulo BUG-PAR reconhece somente DEVOLUCAO com prefixo próprio; ignora a compensadora. A ausência rejeitada continua produzindo REABERTURA comum, com devolução ao colaborador. (`app/chamados/services/materializacao.py:275`, `app/chamados/services/materializacao.py:1158`, `app/chamados/services/materializacao.py:1167`) |
   | `backfill_decisoes_pergunta` | Correspondência procura RESPOSTA/VALIDACAO; testemunho de `admin_decidiu` procura SILENCIO/VALIDACAO. REABERTURA não substitui esses testemunhos; ressuscitar remove via/materialização do universo. (`app/chamados/models.py:4216`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:99`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:314`, `app/chamados/models.py:3457`) |
   | `curar_perguntas_em_limbo` | Seleciona pela projeção e conjunto nominal; não interpreta a marca. Usa a porta comum para limpar. (`app/chamados/management/commands/curar_perguntas_em_limbo.py:46`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:69`) |
   | `auditar_invariantes_chamados` | I18/I20 leem projeção; não conferem se a compensação efetivamente prevalece na sequência. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1076`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1106`) |
   | Histórico / foto E4b | REABERTURA integra `ATOS_COM_FOTO` e aparece em respostas anteriores. Foto E4b captura todas as decisões e campos concretos, incluindo a marca. (`app/chamados/models.py:4137`, `app/chamados/models.py:3779`, `app/chamados/services/foto_e4b.py:210`, `app/chamados/services/foto_e4b.py:280`) |

   A inclusão no histórico não incrementa o limite de reaberturas do colaborador: esse limite vem do campo da Disputa, não da contagem de REABERTURAS da sequência. (`app/chamados/services/fio.py:388`, `app/chamados/models.py:2294`)

10. **Fila — atomicidade e idempotência sequencial conferem.** Fotos, compensador e limpeza estão no mesmo `atomic`; a trava antecede as escritas e segue Colaborador → Disputa → Chamado → Pergunta. Uma exceção propagada no bloco não confirma apenas metade da operação. (`app/chamados/models.py:1629`, `app/chamados/models.py:1634`, `app/chamados/models.py:1678`, `app/chamados/models.py:1680`, `app/chamados/models.py:2831`, `app/chamados/models.py:2840`, `app/chamados/models.py:2853`, `app/chamados/models.py:2857`)

    Na segunda chamada sequencial, a pergunta já não tem materialização, sai do universo e não recebe outra REABERTURA. Há teste que exige retorno zero e quantidade de decisões inalterada. Execução: **[nao verificado]**. (`app/chamados/models.py:1543`, `app/chamados/models.py:1616`, `app/chamados/models.py:3457`, `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:381`)

11. **Fila — demais classes e selos.** `es_da_ressuscitacao` é predicado sobre procedência persistida; não julga respondida, validada ou viva. O relógio novo é consultado no escritor; a leitura da sequência não consulta relógio. (`app/chamados/models.py:4186`, `app/chamados/models.py:1662`, `app/chamados/models.py:4264`)

    A constraint garante apenas “marca implica REABERTURA”. Não garante a porta escritora: `registrar` aceita dicionários genéricos. O tripwire fiscaliza escritas literais, exige um sítio autorizado e contém exemplos externos; não constitui restrição de execução. Segundo escritor concreto: **[nao verificado]**. (`app/chamados/models.py:4178`, `app/chamados/models.py:4298`, `app/chamados/tests/test_f2_ressuscitacao_um_escritor.py:28`, `app/chamados/tests/test_f2_ressuscitacao_um_escritor.py:70`, `app/chamados/tests/test_f2_ressuscitacao_um_escritor.py:73`)

    RED(a), controle(c) e teto do app têm asserções substantivas; o teste de ordem alterado esvazia o universo da compensação e não exercita a inserção nova. Resultado executado da suíte: **[nao verificado]**. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:424`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:449`, `app/api/tests/test_f2_teto_consultas_app.py:56`, `app/chamados/tests/test_e4_expand.py:323`)

    A migration adiciona campo/default antes da constraint; o deploy oficial aplica migrations antes do HUP. Não há pouso invertido demonstrado nesse caminho. Aplicação da 0054 em produção e conservação efetiva de CSF/folha exportada: **[nao verificado]**. (`app/chamados/migrations/0054_decisao_da_ressuscitacao.py:15`, `app/chamados/migrations/0054_decisao_da_ressuscitacao.py:20`, `bin/deploy.sh:317`, `bin/deploy.sh:360`)

## 17/11 11:24 · sessao 7178b1856410 · FOLHA-ENTRA-COM-DISPUTA-ABERTA · leitura · origem=Code

A causa é a diferença entre as perguntas dos dois juízes: **o export consulta a célula da janela; o PDF auditável consulta a existência de qualquer disputa aberta da pessoa, sem recorte temporal**. (`app/folha/export.py:1247`, `app/folha/export.py:1273`; `app/chamados/services/disputa_emissao.py:40`; `app/relatorios/pdf_espelho.py:1085`)

As referências abaixo são da cópia fornecida; sua correspondência ao HEAD do hash informado não pôde ser confirmada sem metadados Git [nao verificado].

**(i) Por que ENTRA e qual retenção não pegou**

`classificar_export` nasce com `status='entra'`. O motivo existente que poderia retirar essas pessoas é **`furo_espelho`**, acionado somente quando `motivos_retencao_espelho(...)` devolve motivos. (`app/folha/export.py:841`, `app/folha/export.py:968`)

Apesar do nome e dos argumentos recebidos, `motivos_retencao_espelho` **ignora `turnos_abertos`, `inconsistencias` e a grade passada**. Resolve a janela e delega a `motivos_retencao_celula`. Assim, nem disputa aberta nem contador de inconsistências elevado bastam para esse motivo pegar. (`app/folha/export.py:1301`)

O retentor efetivo percorre as células da janela e retém por:

- Célula ausente em dia passado com vínculo, ou veredito ausente nas condições previstas: `sem_celula`. (`app/folha/export.py:1252`, `app/folha/export.py:1267`, `app/folha/export.py:1295`)
- Veredito pertencente a `VEREDITOS_ACUSAM`, descontadas as exceções dos protestos não cobráveis e do furo só de pausa. (`app/escala/models.py:1767`; `app/folha/export.py:1273`, `app/folha/export.py:1293`)
- Isento e desligado saem antes, sem retenção por esse juiz. (`app/folha/export.py:1205`)

**A disputa não chega diretamente a esse predicado.** O comentário do fechamento espera que uma disputa que “sujou a janela” resulte em retenção por `furo_espelho`; entretanto, o leitor atual exige a acusação da célula, com aquelas exceções. (`app/ponto/services/fechamento.py:576`; `app/folha/export.py:1273`)

Portanto, dado o ENTRA informado pela sonda, esse ramo não encontrou motivo retentor naquela execução. **Qual célula/veredito/protesto explica individualmente cada um dos 49 não está no dado fornecido** [nao verificado]. Não é possível afirmar que todos tinham célula concorde, decisão administrativa ou uma mesma exceção [nao verificado].

`porta_export.py` exerce outra função: confere o carimbo da medição e pode recusar o arquivo inteiro. Seu total de falhas soma divergências entre leitores e a condição de HE configurada; não soma a existência de disputa aberta. (`app/folha/porta_export.py:687`, `app/folha/porta_export.py:807`, `app/folha/porta_export.py:818`; `app/folha/views.py:128`)

**(ii) Qual valor chega ao TXT**

**No caminho atual do TXT, a disputa não bloqueia o motor. O valor é recalculado na geração.** O fechamento executa `apurar_por_fatia`, que chama `motor.calcular_mes`; o gerador solicita esse fechamento com `somente_leitura=True, retornar_objeto=True` e passa os mesmos objetos à classificação e à montagem. (`app/ponto/services/fechamento.py:286`, `app/ponto/services/fechamento.py:754`; `app/folha/views.py:207`, `app/folha/views.py:268`)

A promessa de “bypass total” está na docstring antiga do helper; sua execução apenas retorna a disputa aberta. Até a tela continua chamando o espelho, e registra expressamente que o comentário antigo estava ultrapassado. (`app/ponto/services/disputa.py:4`, `app/ponto/services/disputa.py:31`; `app/ponto/views.py:160`, `app/ponto/views.py:181`)

O caminho do valor é:

1. **Par/dia:** `calcular_periodo` produz os campos do período, depois acrescentado a `resultado.periodos` ou `periodos_ft`. (`app/ponto/motor_calculo_v2.py:2458`, `app/ponto/motor_calculo_v2.py:2492`)
2. **Turno aberto:** pode receber crédito **parcial dos pares já fechados**. O motor calcula esses pares e escreve `minutos_trabalhados` no período aberto; sem crédito, não faz essa atribuição. Isso é condição do turno, não consequência automática da disputa. (`app/ponto/motor_calculo_v2.py:2542`, `app/ponto/motor_calculo_v2.py:2553`, `app/ponto/motor_calculo_v2.py:2556`, `app/ponto/motor_calculo_v2.py:2559`)
3. **Total mensal:** as propriedades de `ResultadoMes` somam os períodos por rubrica; o fechamento escreve os atributos mensais correspondentes, inclusive no objeto entregue em leitura. (`app/ponto/motor_calculo_v2.py:295`, `app/ponto/motor_calculo_v2.py:321`, `app/ponto/motor_calculo_v2.py:324`; `app/ponto/services/fechamento.py:902`, `app/ponto/services/fechamento.py:918`, `app/ponto/services/fechamento.py:1135`)
4. **Evento exportável:** `eventos_do_fechamento` lê HE, folga trabalhada, noturno, intra e atraso/saída antecipada. `horas_trabalhadas` não tem evento próprio nessa lista. (`app/folha/export.py:439`)
5. **Linha:** o montador pula valor zero, resolve a rubrica e chama `montar_linha_10`; horas são multiplicadas por 100, arredondadas e formatadas em nove posições. A linha identifica competência e rubrica, sem data do dia. (`app/folha/export.py:730`, `app/folha/export.py:733`, `app/folha/export.py:738`, `app/folha/export.py:34`)

**Resposta entre as alternativas:** não existe “zero por disputa” nem preservação do “último gravado por disputa” nesse fluxo; existe cálculo atual, podendo conter crédito parcial de turno aberto. **Os valores efetivos dos dias dos 49 são [nao verificado].** (`app/folha/views.py:207`, `app/folha/views.py:268`; `app/ponto/motor_calculo_v2.py:2559`)

`DiaPago` é um ramo irmão: recebe os mesmos períodos quando há persistência ou lavratura expressamente solicitada. O montador do TXT recebe o objeto mensal calculado; não busca o valor diário no `DiaPago`. (`app/ponto/services/fechamento.py:1191`, `app/ponto/services/fechamento.py:1201`; `app/ponto/services/dia_pago.py:173`; `app/folha/views.py:268`)

**(iii) Lei já escrita**

**lei: nenhuma** encontrada nos três documentos para decidir a regra geral “disputa aberta cujo fato pertence à janela, por si só, retém a pessoa” [nao verificado].

As candidatas têm estes alcances:

| Texto | O que já decide |
|---|---|
| **L-033** | Entrada e saída batidas, faltando só intervalo: pagável, mantendo a pergunta aberta sem bloquear. É exceção que impede equiparar toda pergunta/disputa aberta a retenção. (`app/docs/LEIS.md:81`) |
| **L-095** | Lavratura e leitura do valor; não estabelece esse predicado de retenção. (`app/docs/LEIS.md:113`) |
| **L-099** | Coerência e encaminhamento de CADASTRO/BATIDA ao admin. (`app/docs/LEIS.md:122`) |
| **L-113** | Calcular o que existe e permitir refazer a exportação. (`app/docs/LEIS.md:155`) |
| **L-129** | Dono da tarefa; não substitui o juiz de retenção. (`app/docs/LEIS.md:149`; `app/folha/fora_da_folha.py:115`) |
| **Corte do carimbo** | `turno_aberto` continua barrando o carimbo; trata de turno aberto, não de disputa aberta. (`app/docs/CORTES.md:317`) |
| **Dossiê da folha** | O crivo é o ENTRA de `classificar_export`; aprovação é carimbo, e TXT/recalculo não consultam tranca. (`app/docs/DOSSIES.md:120`, `app/docs/DOSSIES.md:122`) |

**(iv) Matriz do comportamento atual**

Aqui, “na janela” significa **data do fato**. O anexo distingue esse recorte de `aberta_em`: col193 pertence aos 49 com fato em novembro e abertura em setembro; col222 tem fato desconhecido e `aberta_na_janela=True` [nao verificado — anexo fornecido, sem arquivo:linha].

| Estado da disputa | Juiz do export | Juiz do espelho/PDF | O que pode sair no TXT |
|---|---|---|---|
| Aberta; fato na janela | O estado da disputa sozinho não retém. A célula da janela pode produzir `furo_espelho`. (`app/folha/export.py:968`, `app/folha/export.py:1273`) | Recusa auditável; lote tenta informacional. (`app/relatorios/pdf_espelho.py:1084`; `app/relatorios/services.py:244`) | Se ENTRA e demais condições liberarem: rubricas do cálculo atual, omitindo zeros. Se FORA: nenhuma linha dessa pessoa. (`app/folha/export.py:716`, `app/folha/export.py:726`, `app/folha/export.py:733`) |
| Aberta; fato fora da janela | Mesmo critério: o retentor examina as células da competência pedida. (`app/folha/export.py:1247`) | Mesma recusa: consulta sem filtro de datas. (`app/chamados/services/disputa_emissao.py:40`) | Mesmo comportamento; disputa fora da janela não impõe valor ou exclusão próprios. (`app/folha/export.py:968`, `app/folha/export.py:730`) |
| Fechada; sem outra aberta | Fechar a disputa não substitui a leitura da célula; acusação remanescente ainda pode reter. (`app/folha/export.py:1273`) | Esse bloqueio deixa passar; outras falhas ainda podem levar ao informacional. (`app/chamados/services/disputa_emissao.py:40`; `app/relatorios/services.py:244`) | Mesmo cálculo atual se admitida; nenhuma linha se retida. (`app/folha/views.py:207`, `app/folha/views.py:268`; `app/folha/export.py:726`) |

A emissão ainda filtra fechamento em estado admitido no TXT e depende da porta global; ENTRA não garante linha, pois eventos zero são omitidos. (`app/folha/export.py:717`, `app/folha/export.py:733`; `app/folha/views.py:128`)

**Quem escreve e quem lê os campos determinantes**

| Campo/resultado | Quem escreve | Quem lê neste caminho |
|---|---|---|
| Disputa: `colaborador`, `aberta_em` | Emissão cria o vínculo; ORM grava abertura com `auto_now_add`. (`app/chamados/services/disputa_emissao.py:601`; `app/chamados/models.py:1820`) | Juiz do PDF filtra colaborador; não usa abertura. (`app/chamados/services/disputa_emissao.py:40`) |
| Disputa: `fechada_em` | `fechar` preenche; reaberturas administrativa e sistêmica zeram. (`app/chamados/models.py:2013`, `app/chamados/models.py:2230`, `app/chamados/models.py:2254`) | `colaborador_em_disputa` testa NULL; PDF usa seu resultado. (`app/chamados/services/disputa_emissao.py:41`; `app/relatorios/pdf_espelho.py:1085`) |
| Disputa: `motivos`, `contexto` | Emissão cria e agrega. (`app/chamados/services/disputa_emissao.py:550`, `app/chamados/services/disputa_emissao.py:552`, `app/chamados/services/disputa_emissao.py:603`) | Não participam do predicado do PDF nem do retentor da célula. (`app/chamados/services/disputa_emissao.py:40`; `app/folha/export.py:1273`) |
| Sonda: `data_fato`, `aberta_na_janela` | Código da sonda não fornecido; escritor e derivação exatos [nao verificado]. | Usados no anexo; não integram os predicados citados. (`app/chamados/services/disputa_emissao.py:40`; `app/folha/export.py:1273`) |
| Célula: `veredito`, `protestos`, `ata` | Porta `lavrar_veredito`; decisão administrativa prevalece sobre cartório. (`app/ponto/portas/celula.py:1032`, `app/ponto/portas/celula.py:1060`, `app/ponto/portas/celula.py:1090`) | Retentor lê veredito/protestos; exceção da pausa consulta o juiz da ata. (`app/folha/export.py:1273`, `app/folha/export.py:1286`, `app/folha/export.py:1150`) |
| Períodos e números mensais | Motor calcula períodos; fechamento escreve os totais. (`app/ponto/motor_calculo_v2.py:2458`; `app/ponto/services/fechamento.py:902`) | Propriedades mensais somam; export lê eventos do fechamento. (`app/ponto/motor_calculo_v2.py:295`; `app/folha/export.py:439`) |
| `status` de classificação e `motivo` | `classificar_export`. (`app/folha/export.py:837`, `app/folha/export.py:970`) | Montador seleciona ENTRA. (`app/folha/export.py:716`) |
| Carimbo: `valor`, competência e detalhes | `porta_export.lavrar`. (`app/folha/porta_export.py:768`) | `conferir`, chamado pelo gerador. (`app/folha/porta_export.py:807`; `app/folha/views.py:128`) |

Resta uma decisão: **disputa aberta cujo fato pertence à janela deve, por si só, reter a pessoa, preservada a exceção L-033?** Essa decisão geral não foi localizada [nao verificado].

perguntas de lei: 1

## 17/11 11:23 · sessao 8710aac280f2 · - · - · origem=Ronald (tmux)

O219 | GRADE-MESTRA.md:64 | PENDENTE REAL | Versão/horizonte no código (app/ponto/services/cartorio.py:142); faltam TXT novo da 09 e Pauta DP (app/docs/BACKLOG.md:1287); espera atual [nao verificado]; fecho de censo integral [nao verificado] | outras | lei_ou_bang=N | censo=N
O228 | GRADE-MESTRA.md:65 | FEITA | Fecho inteiro escrito: b4d938aa, 7cb0efbb, 31fce87c (app/docs/BACKLOG.md:1291; app/docs/RELATO-ARQUIVO.md:39600); lápides presentes (app/ponto/precedencia.py:151; app/ponto/precedencia.py:548) | - | lei_ou_bang=N | censo=N
O218 | GRADE-MESTRA.md:66 | FEITA | FECHADA no main 1b6d5d02, bateria e prova de frota publicadas (app/docs/TICKETS.md:196); cenários/ponto fixo no código (app/ponto/tests/test_bateria_lavratura.py:169; app/ponto/tests/test_bateria_lavratura.py:193); registro antigo contraditório em app/docs/BACKLOG.md:1304 | - | lei_ou_bang=N | censo=N
O221 | GRADE-MESTRA.md:67 | FEITA | Pousos bb0bd0fa, d0625307, 52bfc524 documentados (app/docs/TICKETS.md:193; app/docs/RELATO-ARQUIVO.md:33156; app/docs/RELATO-ARQUIVO.md:33159); papel prazo permanece no código (app/config/crons.py:2128) | - | lei_ou_bang=N | censo=N
O129 | GRADE-MESTRA.md:68 | PENDENTE REAL | Declarar os 28 campos do wizard (app/docs/BACKLOG.md:1310); wizard continua fora das telas declaradas (app/core/configuracao_efeito.py:239); censo histórico de 28 em app/docs/BACKLOG.md:1310; espera atual [nao verificado] | outras | lei_ou_bang=N | censo=S
O117 | GRADE-MESTRA.md:69 | SEM PROVA | Medição encontrou zero casos e fecho foi condicional, sem encerramento efetivado demonstrado (app/docs/TICKETS.md:250; app/docs/RELATO-ARQUIVO.md:24143; app/docs/RELATO-ARQUIVO.md:24145); cura ou obra sucessora [nao verificado] | - | lei_ou_bang=N | censo=S
O105 | GRADE-MESTRA.md:70 | PENDENTE REAL | Espera ! entre substituir TXT e autopsiar datas_falta; comparação das três empresas isolou uma linha divergente (app/docs/BACKLOG.md:1336); export ainda deriva datas_falta durante geração (app/folha/export.py:740) | outras | lei_ou_bang=S | censo=S
O44 | GRADE-MESTRA.md:71 | PENDENTE REAL | Pousou 70d159b9, mas faltam baixa dos dois sítios mantidos, caso 1 pela tela e smoke do Ronald (app/docs/TICKETS.md:168); censo explicitamente fechado (app/docs/RELATO-ARQUIVO.md:974); complemento QUARENTENA interrompido por desenho (app/docs/BACKLOG.md:872) | outras | lei_ou_bang=S | censo=S
O54 | GRADE-MESTRA.md:72 | PENDENTE REAL | Programa parcial 20/29 (app/docs/RELATO.md:254); faltam portas V0–V4 (app/docs/BACKLOG.md:2267; app/docs/BACKLOG.md:2274), com 12 dias ainda sem nome (app/docs/BACKLOG.md:2270); quatro esperas Ronald registradas (app/docs/RELATO.md:246); censo integral fechado [nao verificado] | espelho-verdade (O54) | lei_ou_bang=S | censo=N
O95 | GRADE-MESTRA.md:73 | PENDENTE REAL | Resgate existe (app/ponto/turnos.py:1106; app/ponto/turnos.py:1502); DIFF do resgate entregue sem apply (app/docs/RELATO.md:278), enquanto candidata exigir volta não está escrita e contradiz selo (app/docs/RELATO.md:282); faltam definição/fecho da cura e censo integral; espera atual [nao verificado] | outras | lei_ou_bang=N | censo=N
O92 | GRADE-MESTRA.md:74 | PENDENTE REAL | Restam alimentação em lote dos quatro leitores enumerados (app/docs/BACKLOG.md:1466); chamadas por dia persistem (app/ponto/calculador/alimentacao.py:516; app/folha/porta_export.py:548; app/relatorios/pdf_espelho.py:457; app/ponto/models.py:895); cópia inline já removida pela CASA-ESCALA-E4 (app/escala/utils.py:2126); espera atual [nao verificado] | outras | lei_ou_bang=N | censo=S
O136 | GRADE-MESTRA.md:75 | PENDENTE REAL | Falta dispatch consultar partido nos dois sentidos, DIFF e ! do motor; censo de 341 templates isolou dois casos (app/docs/BACKLOG.md:1480); código ainda consulta como veto do ramo turno_partido (app/ponto/motor_calculo_v2.py:3659) | outras | lei_ou_bang=S | censo=S
CHAMADO-EM-RAIA | GRADE-MESTRA.md:76 | PENDENTE REAL | Escritor fechado por ESCRITOR-UNICO-DE-CHAMADO (app/docs/RELATO.md:5686; app/docs/RELATO.md:5690), mas célula juiz permanece parcial com condições O168/O169/LASTRO (app/core/contratos_estruturais.py:270); censo original publicado (app/chamados/tests/test_chokepoint_chamado_gate.py:32), fecho do censo restante e espera atual [nao verificado] | contrato 20 | lei_ou_bang=N | censo=N

Contagem deste recorte: FEITA=3 PENDENTE=9 SUPERADA=0 SEM_PROVA=1 TOTAL=13. Quatro documentos consultados; histórico git direto indisponível nesta cópia sem `.git` [nao verificado].

## 17/11 11:22 · sessao 18b8ff1982d2 · - · - · origem=Ronald (tmux)

A premissa “motor bloqueado pela disputa” não descreve o caminho atual do TXT: a disputa só é consultada como flag na tela, que continua chamando o espelho; o fechamento calcula por fatias sem consultar disputa. A docstring S95 do wrapper promete bypass, mas sua execução apenas retorna a disputa aberta. (`app/ponto/views.py:142`, `app/ponto/views.py:161`, `app/ponto/views.py:181`, `app/ponto/services/disputa.py:18`, `app/ponto/services/disputa.py:31`, `app/ponto/services/fechamento.py:279`, `app/ponto/services/fechamento.py:286`.)

O TXT real usa o cálculo de agora, não o último gravado: `gerar_dominio_txt` chama `_recalc(...somente_leitura=True, retornar_objeto=True)` e passa seus objetos para `classificar_export` e `montar_linhas_export`. A consulta anterior de fechamentos persistidos serve à guarda de existência/carimbo; a montagem recebe `_fechs_lido`. (`app/folha/views.py:147`, `app/folha/views.py:163`, `app/folha/views.py:207`, `app/folha/views.py:212`, `app/folha/views.py:268`.)

Caminho do dia/par até a linha:

1. `calcular_periodo` escreve `PeriodoCalculo.minutos_trabalhados`; sem saída, retorna período aberto com campos numéricos inicialmente zero. Há três implementações: `app/ponto/motor_calculo_v2.py:213`, `:2750`, `:2752`, `:2785`, `:2877`, `:2879`, `:2893`, `:3215`, `:3217`.
2. O motor acrescenta o período a `resultado.periodos` ou `resultado.periodos_ft`. Para turno aberto, R3 pode calcular os pares já fechados e escrever o crédito parcial no período aberto; se não há crédito, não o atribui. Portanto, “turno aberto” também não implica zero obrigatório. (`app/ponto/motor_calculo_v2.py:2458`, `:2492`, `:2494`, `:2514`, `:2542`, `:2553`, `:2556`, `:2559`.)
3. As propriedades `ResultadoMes` leem os períodos e somam por rubrica: trabalhadas, noturnas, HE, atraso, saída antecipada e intra. (`app/ponto/motor_calculo_v2.py:295`, `:303`, `:321`, `:324`, `:328`, `:347`, `:351`, `:360`, `:363`, `:370`.)
4. `recalcular_fechamento_mes` escreve os atributos do objeto mensal a partir dessas propriedades — também em modo leitura. Há complementos de folga/feriado e crédito parcial. (`app/ponto/services/fechamento.py:890`, `:902`, `:918`, `:920`, `:923`, `:955`, `:965`, `:984`, `:1001`, `:1002`, `:1004`.) Em modo leitura, entrega o objeto e não executa os ramos de persistência. (`app/ponto/services/fechamento.py:1128`, `:1135`, `:1137`.)
5. `eventos_do_fechamento` lê os atributos mensais e emite eventos de HE50, HE100, feriado, folga, noturno, intra e atraso+saída antecipada. **Não existe evento próprio de `horas_trabalhadas` nessa lista.** (`app/folha/export.py:439`, `:441`, `:446`, `:447`, `:448`, `:451`.)
6. `montar_linhas_export` lê `(evento, valor)`, pula valor zero, resolve rubrica e chama `montar_linha_10`; esta converte horas em centesimal, arredonda e escreve o campo de nove posições. O TXT agrega por competência/rubrica, não produz uma linha por dia. (`app/folha/export.py:723`, `:730`, `:733`, `:735`, `:738`, `:34`, `:42`, `:43`, `:45`.)

`DiaPago` é ramo irmão: o mesmo fechamento chama `dia_pago.lavrar` quando há escrita ou pedido explícito de lavratura; `lavrar` lê os minutos dos períodos, soma por dia e substitui as linhas da competência. No fluxo puro de geração do TXT, `somente_leitura=True` não entra nesse ramo; o montador do TXT lê o objeto mensal injetado. (`app/ponto/services/fechamento.py:1191`, `:1201`, `:1203`, `app/ponto/services/dia_pago.py:173`, `:178`, `:180`, `:185`, `:359`, `:360`, `app/folha/views.py:208`, `:268`.)

Conclusão admissível: **valor recalculado por rubrica; pode haver crédito parcial em dia aberto; zero é omitido; não há mecanismo atual de preservar último valor por disputa. Os valores efetivos dos dias dos 49 são [nao verificado].** (`app/ponto/motor_calculo_v2.py:2559`, `app/folha/export.py:733`, `app/folha/views.py:208`, `:268`.)

## 17/11 11:22 · sessao 31c5238abdd1 · - · - · origem=Ronald (tmux)

Item 1: premissas centrais conferem; há duas lacunas de desenho que precisam ficar explícitas antes da construção. A existência do botão não prova transferência de tarefa, e o leitor atual não identifica ressuscitação por ato próprio. (`app/chamados/views_cobrar.py:34-57`; `app/chamados/services/tarefa_do_dia.py:142-177`; `app/chamados/models.py:1613-1630`.)

- **Sem ato:** `_pela_decisao` produz `aguarda_resposta`, ação `RESPONDER`, dono `COLABORADOR`. **SILENCIO:** produz `silencio_decidido`, ações vazias. A fila atual das duas classes está assim no código. (`app/chamados/services/tarefa_do_dia.py:156-177`.)
- **Escritor do SILENCIO:** encerramento da disputa por desligamento/admin, chamando `DecisaoPergunta.registrar` com `ato`, `ato_em`, `ato_por`, texto e motivo. O escritor final da sequência é `registrar`, que faz `bulk_create`. **Leitor:** carga ordenada por `ato_em, pk`, cortada pelo `agora`, seguida de `_pela_decisao`. (`app/chamados/models.py:1999-2007`; `app/chamados/models.py:4110-4120`; `app/chamados/services/tarefa_do_dia.py:421-424`; `app/chamados/services/tarefa_do_dia.py:158-177`.)
- **Escritor da ressuscitação:** `ChamadoColaborador.ressuscitar_perguntas`; conserva as antigas RESPOSTA/VALIDACAO na sequência e limpa `validada_em`, `validada_por`, `respondida_em`, `resposta_colab`, mais carimbo materializado/via, usando `descarimbar_em_lote`. Não grava ato compensatório nesta cópia. (`app/chamados/models.py:1613-1630`; `app/chamados/models.py:3391-3393`.)
- **Como reconhecer “ressuscitada”:** a prova atual específica é `LogAuditoria`, ação `perguntas_ressuscitadas`, com IDs antes/depois; a carga da tarefa lê perguntas e decisões, sem consultar essa prova. A seleção/discriminante que fará somente a ressuscitada com SILENCIO ir ao admin ainda não está definido no código. (`app/chamados/models.py:1651-1662`; `app/chamados/services/tarefa_do_dia.py:405-430`.)
- **Botão cobrar:** recebe `cid` de chamado, busca colaborador, verifica cobrança por colaborador/dia, envia push genérico ao atendimento e grava apenas nota `[COBRANCA]`; não muda o SILENCIO nem grava ação RESPONDER. Na lista, o botão está no ramo `verbo == "cobrar"`; o ramo `decidir` não o apresenta. Se “cobra colaborador” significa só notificar, confere; se significa devolver a tarefa, não fecha com este botão. (`app/chamados/views_cobrar.py:24-57`; `app/templates/chamados/partials/_lista_chamados.html:83-86`; `app/chamados/services/tarefa_do_dia.py:176-177`.)
- **Escritor/leitor da cobrança:** `views_cobrar` chama `nota.anexar_ocorrencia`, que cria `FollowupChamado`; o próprio `views_cobrar` lê essa nota para throttle e o painel lê para `cobrado_hoje`. (`app/chamados/views_cobrar.py:36-55`; `app/chamados/services/nota.py:75-109`; `app/chamados/services/painel.py:349-359`; `app/chamados/services/painel.py:395-396`.)
- **Dia do fato/competência:** nascimento passa o dia por `gravar_dia_do_fato`; a tarefa consulta `janela_atual(dia, empresa)` e o K6 em lote para saber entregue. O ponto de nascimento e o leitor estão identificados. (`app/chamados/models.py:3123-3127`; `app/chamados/services/tarefa_do_dia.py:467-483`; `app/ponto/janelas.py:60-63`.)
- **K6:** registro vivo aponta para `folha/export.py::competencia_entregue`; ele lê `ExportacaoDominio` vigente por empresa/mês/ano, recusando vigente invalidada. Escritor do registro é `ExportacaoDominio.registrar`; escritor da invalidação é `invalidar`. (`app/core/juizes.py:343`; `app/folha/export.py:580-594`; `app/folha/models.py:174-187`; `app/folha/models.py:143-162`.)
- **`competencia_ja_paga`:** busca em `app/**/*.py` não encontrou definição/chamada; implementação presente [não verificado]. O BACKLOG registra obra em cópia própria, com pouso pendente e cadastro “paga fora”. Não há dado raspado de cadastro real ou estoque nesta solicitação. (`app/docs/BACKLOG.md:1992`; dado real [não verificado].)
- **Lista DP/admin nova:** escritor da lista textual e novo ramo que produzirá ação/admin ainda não existem identificados; são construção futura [não verificado]. A fila administrativa atual vem de `qs_na_fila`, restrita a estados COBRANCA sem silêncio temporal; a propriedade “admin alcançável” também exige porta/tela reconhecida. (`app/chamados/services/painel.py:188-190`; `app/chamados/services/tarefa_do_dia.py:654-659`.)

Item 2 tem conflito executável preciso: o teste guarda `antes`, ressuscita e exige igualdade integral da tarefa depois; o caso SILENCIO usa essa função. F5 que transfira essa mesma classe ao admin fará a igualdade falhar quando coexistirem. Dizer “o teste segue como está” e apenas acrescentar outro em F5 não resolve a contradição final; seria preciso explicitar contexto distinto ou ajustar o contrato do teste. (`app/chamados/tests/test_f1nova_b1_juiz_respondida.py:319-335`.)

## 17/11 11:22 · sessao 1811a98d7545 · - · - · origem=Ronald (tmux)

Correção à linha O142, após conferir a revisão explícita do censo e o apply:

O142 | GRADE-MESTRA.md:78 | FEITA | Pouso d1689254 (app/docs/RELATO-ARQUIVO.md:37752); borda incorporada à cura (app/docs/RELATO-ARQUIVO.md:38964; app/ponto/turnos.py:1135) e três relavras aplicadas (app/docs/RELATO-ARQUIVO.md:38695) | - | lei_ou_bang=N | censo=N

## 17/11 11:22 · sessao 8b7303df858b · - · - · origem=Ronald (tmux)

Juiz do espelho é global por colaborador: `motor_bloqueado_por_disputa` chama `colaborador_em_disputa`; este seleciona qualquer disputa com `fechada_em IS NULL`, sem consultar `aberta_em`, data do fato, mês ou janela (`app/ponto/services/disputa.py:18`, `app/ponto/services/disputa.py:31`, `app/chamados/services/disputa_emissao.py:34`, `app/chamados/services/disputa_emissao.py:40`).

Matriz do espelho:

| Estado | Juiz do espelho | Resultado |
|---|---|---|
| Aberta, fato dentro da janela | Recusa auditável | Lote tenta informacional (`app/relatorios/pdf_espelho.py:1084`, `app/relatorios/services.py:244`, `app/relatorios/services.py:250`). |
| Aberta, fato fora da janela | Mesma recusa: filtro não tem data | Mesmo fallback (`app/chamados/services/disputa_emissao.py:40`, `app/relatorios/services.py:250`). |
| Fechada, sem outra aberta | Esse bloqueio deixa passar | Auditável pode sair; outras falhas ainda causam fallback (`app/chamados/services/disputa_emissao.py:40`, `app/relatorios/pdf_espelho.py:1085`, `app/relatorios/services.py:244`). |

O auditável passa `_bloqueio_disputa` ao pipeline e levanta `ValueError` com a disputa aberta (`app/relatorios/pdf_espelho.py:1084`, `app/relatorios/pdf_espelho.py:1113`). O pipeline executa bloqueios antes de criar o registro (`app/relatorios/pdf_base.py:129`); `RegistroPDFAuditavel` só nasce depois (`app/relatorios/pdf_base.py:152`). O informacional gera bytes diretamente e usa rodapé sem hash (`app/relatorios/pdf_espelho.py:1046`, `app/relatorios/pdf_espelho.py:1063`, `app/relatorios/pdf_espelho.py:1070`). Portanto ter um registro auditável antigo não prova que o bloqueio atual passou (`app/relatorios/pdf_base.py:152`, `app/chamados/services/disputa_emissao.py:40`).

Quem escreve e quem lê:

| Campo | Escrita | Leitura relevante |
|---|---|---|
| `DisputaSupervisao.colaborador` | `_abrir_ou_agregar_disputa` cria vínculo (`app/chamados/services/disputa_emissao.py:601`) | `colaborador_em_disputa` filtra pessoa (`app/chamados/services/disputa_emissao.py:40`). |
| `fechada_em` | Nasce nulo pela definição (`app/chamados/models.py:1821`); `fechar` grava instante (`app/chamados/models.py:2013`); `reabrir_admin` e `reabrir_sistema` zeram (`app/chamados/models.py:2230`, `app/chamados/models.py:2254`). | Juiz global lê nulo (`app/chamados/services/disputa_emissao.py:41`), alcançando o bloqueio do auditável pelo wrapper (`app/ponto/services/disputa.py:31`, `app/relatorios/pdf_espelho.py:1085`). |
| `aberta_em` | ORM `auto_now_add` no nascimento (`app/chamados/models.py:1820`, `app/chamados/services/disputa_emissao.py:601`). | Não é lida pelo juiz do espelho (`app/chamados/services/disputa_emissao.py:40`). |
| `motivos` | Nascimento (`app/chamados/services/disputa_emissao.py:603`), agregação (`app/chamados/services/disputa_emissao.py:550`); há também métodos de alteração, portanto não afirmar escritor único absoluto desse campo (`app/chamados/models.py:2033`, `app/chamados/models.py:2236`). | Não participa da decisão do espelho; qualquer disputa aberta serve (`app/chamados/services/disputa_emissao.py:40`). |
| `contexto` | Nascimento (`app/chamados/services/disputa_emissao.py:604`), merge da varredura (`app/chamados/services/disputa_emissao.py:552`, `app/chamados/services/disputa_emissao.py:576`). | Não participa do juiz do espelho (`app/chamados/services/disputa_emissao.py:40`). |
| `RegistroPDFAuditavel` | Pipeline `gerar_pdf_auditavel` (`app/relatorios/pdf_base.py:152`). | O retorno do serviço carrega registro (`app/relatorios/services.py:92`); o anexo lê existência histórica, mas esse leitor da sonda não está demonstrado em arquivo da cópia [nao verificado]. |

Cuidado com a definição temporal do pedido: os 49 são classificados no anexo pela **data do fato**; não são necessariamente disputas cujo `aberta_em` está na janela. Exemplo dado: col193 tem fato 15/11 e abertura 21/09; col222 tem fato ausente e `aberta_na_janela=True` por abertura 09/11. A implementação da sonda não foi fornecida, então não atribuir a `aberta_na_janela` significado diferente do que esses exemplos demonstram [nao verificado].

## 17/11 11:22 · sessao 49b177462bcd · - · - · origem=Ronald (tmux)

Achados de F5/F6, só leitura:

- As matrizes próprias não vieram materialmente nesta cópia: os documentos apontam para `logs/ESPINHA-BRIEFS/<f>/matriz.md`, fora de `app/docs`; conteúdo literal atual dessas matrizes = **[não verificado]**. Ponteiros: `app/docs/RELATO.md:298`, `app/docs/RELATO.md:313`, `app/docs/RELATO.md:425`. Portanto não afirmar que “F5 e F6 já estão RESOLVIDO-PELO-CORTE” nem certificar todo o conteúdo delas.
- O registro existente diz que as perguntas de lei de F5/F6 já passaram a zero pelo corte anterior; isto é registro documental, sem medição nova: `app/docs/RELATO.md:214`, `app/docs/CORTES.md:488`. O corte anterior de F5 conserva a resposta vazia fora do app e dá a ação ao admin; F6 manda derivar `aguarda_admin` do dono publicado por `tarefa_do_dia`, com contador zero para estados sem dono (`app/docs/CORTES.md:488`).
- O código atual confirma `sem decisão -> aguarda_resposta / responder / colaborador` e `SILENCIO -> silencio_decidido / []`: `app/chamados/services/tarefa_do_dia.py:156`, `app/chamados/services/tarefa_do_dia.py:176`. É incorreto estender “sem ninguém” a todo o dia: os verbos residuais da fila podem produzir dono admin/chamado/sistema (`app/chamados/services/tarefa_do_dia.py:665`), e a oferta de resolver dia pode dar admin mesmo sem ação da pergunta (`app/chamados/services/tarefa_do_dia.py:684`).
- A distinção “F2 conserva, F5 altera” é compatível como fronteira de construção, mas há condição literal importante: a asserção em `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:329` compara a tarefa completa antes/depois e é aplicada justamente ao SILENCIO em `:334-335`. Se a futura F5 identificar a ressuscitação dessa fixture e trocar sua tarefa para admin, manter esse teste **literalmente inalterado na árvore combinada** conflita com o resultado novo. Falta explicitar o escopo temporal da prova de invariância ou como a prova F5 convive com ela; resultado na árvore futura = **[não verificado]**.
- O escritor atual da ressuscitação é `ChamadoColaborador.ressuscitar_perguntas`: conserva resposta/validação anteriores na trilha por `DecisaoPergunta.registrar` (`app/chamados/models.py:1613`), limpa `validada_em/validada_por/respondida_em/resposta_colab` pela porta de lote (`app/chamados/models.py:1629`); a documentação do método diz expressamente que ainda não grava ato de devolução (`app/chamados/models.py:1565`). O leitor da fila lê as decisões ordenadas por `ato_em, pk` e recorta por `agora` (`app/chamados/services/tarefa_do_dia.py:421`).
- Campos derivados `razao/acoes/acao/de/porta`: escritor em memória `_pela_decisao/_acao` (`app/chamados/services/tarefa_do_dia.py:135`, `:142`), sem escritor persistente (`:733-736`); projeção da porta publica `rota/fila/verbos/fio/nao_sei/acao/ausencia` (`:708`, `:712-725`).
- `aguarda_admin` hoje ainda é montado no endpoint como `p.estado == PerguntaDisputa.RESPONDIDA`, não pela tarefa (`app/api/views.py:2549-2553`). O endpoint também lê `resposta_colab/respondida_em` diretamente (`:2561-2563`); seleção do app lê `respondida_em is None` e disputa aberta (`app/chamados/services/pergunta_no_app.py:76-85`). Isso dá conteúdo concreto à migração F6, mas o cliente que consome o JSON nesta cópia = **[não verificado]**.
- Porta única da trilha: `DecisaoPergunta.registrar` escreve por `bulk_create` e acorda as células (`app/chamados/models.py:4110-4122`); campos disponíveis `ato/ato_em/ato_por/ato_texto/ato_motivo` em `:4038-4043`. Não há campo persistente “dono” mostrado pela tarefa: o dono é calculado (`app/chamados/services/tarefa_do_dia.py:662-670`).

## 17/11 11:22 · sessao 2384d85bc413 · - · - · origem=Ronald (tmux)

FERIADO-DIA-CHAVE | GRADE-MESTRA.md:91 | PENDENTE REAL | Unificar dia-chave e predicado: ainda há k + conjunto de feriados no calculador versus data da entrada no fechamento (app/ponto/calculador/alimentacao.py:521; app/ponto/services/fechamento.py:949); falta caso atravessando meia-noite (app/docs/BACKLOG.md:1623); censo fechado [nao verificado] | outras | lei_ou_bang=N | censo=N
LISTA-POR-COMPETENCIA | GRADE-MESTRA.md:92 | PENDENTE REAL | Falta uma linha por competência aberta (app/docs/BACKLOG.md:1625); a lavra ainda mede uma janela agregada por colaborador (app/ponto/services/esmeril_espelho.py:38; app/ponto/services/esmeril_espelho.py:219); censo fechado [nao verificado] | outras | lei_ou_bang=N | censo=N
REGUA-DO-PERIODO-COMPLEMENTO-19-10 | GRADE-MESTRA.md:93 | PENDENTE REAL | Complemento exige leitores por fatia, censo 09/10 e encaminhamento/apply por dono (app/docs/BACKLOG.md:1725); a obra-mãe declara três leitores restantes (app/docs/BACKLOG.md:1706), e o vigia ainda usa um vínculo para a janela inteira (app/ponto/management/commands/vigiar_motor_autoridade.py:51); fechamento integral do censo e trava vigente da pergunta 18 [nao verificado] | outras | lei_ou_bang=N | censo=N
O327 | GRADE-MESTRA.md:94 | PENDENTE REAL | Falta cobertura completa e publicação das recusas nomeadas (app/docs/BACKLOG.md:1753); registro posterior ainda aponta 87 recusas idênticas nos dois lados e espera lei do ancoramento (app/docs/RELATO.md:1294); censo fechado atual [nao verificado] | espelho-verdade (O54) | lei_ou_bang=S | censo=N
RETIFICACAO-DA-10-8069-PRIMEIRO | GRADE-MESTRA.md:95 | PENDENTE REAL | Falta resolver o resíduo e retificar pela porta; 243 espera corte do Ronald (app/docs/BACKLOG.md:1757); premissa da 8069 foi corrigida: candidato estrutural ≤10,67 h, sem código/TXT/gravado alterado (app/docs/RELATO.md:6008; app/docs/RELATO.md:6015); censo fechado atual [nao verificado] | outras | lei_ou_bang=S | censo=N
REABRIR-PERGUNTA-A-PREMISSA | GRADE-MESTRA.md:96 | PENDENTE REAL | Porta no ar bdfad18f (app/docs/RELATO.md:4749), mas falta prova do PRONTO: contador zero em 24 h e sete dias sem laço (app/docs/RELATO.md:4751; app/docs/BACKLOG.md:1777); resultado final e censo fechado [nao verificado] | contrato 20 | lei_ou_bang=N | censo=N
LOTE-VAZA-ENTRE-COLABS | GRADE-MESTRA.md:97 | PENDENTE REAL | Falta medir causa e provar lote igual ao individual: divergência de uma linha registrada no HEAD da medição (app/docs/BACKLOG.md:1800; app/docs/RELATO.md:4361); cura/pouso e censo fechado [nao verificado] | outras | lei_ou_bang=N | censo=N
PASSADO-SEM-MARCOS-SEM-TETO | GRADE-MESTRA.md:98 | SUPERADA/ABSORVIDA | Tratamento incorporado à E4a-1, no ar 892159c5f (app/docs/RELATO.md:1729), e MODO-COMUM-PASSADO-SEM-TURNO fechado (app/docs/BACKLOG.md:2147; app/docs/RELATO.md:1722); passado sem turno vai ao cadastro sem pergunta ao colaborador (app/chamados/services/fabrica_da_celula.py:20) | - | lei_ou_bang=N | censo=N
LEGADO-51-ACESAS | GRADE-MESTRA.md:99 | PENDENTE REAL | Falta medir as 51 pela função real e fechar pela porta as que a lei encerra (app/docs/BACKLOG.md:1815); resultado desse universo, pouso/aplicação e censo fechado [nao verificado] | contrato 20 | lei_ou_bang=N | censo=N
DIA-DA-BATIDA-JUIZ-UNICO | GRADE-MESTRA.md:100 | PENDENTE REAL | Falta concluir censo AST com classificação dos sítios, migrar juízos paralelos e entregar selo/DIFF por migração (app/docs/BACKLOG.md:1818); rejulgamento prévio de col33 tem prova (app/docs/RELATO.md:4262), mas fechamento da obra e do censo [nao verificado] | contrato 20 | lei_ou_bang=N | censo=N
PERGUNTA-DO-DIA-EM-CURSO | GRADE-MESTRA.md:101 | FEITA | 165fee20 no ar e push provado (app/docs/RELATO.md:4236); pouso documentado (app/docs/RELATO.md:4255); teto por instante e veto ao futuro presentes no código (app/chamados/services/fabrica_da_celula.py:8) | - | lei_ou_bang=N | censo=N
R1-ESTRUTURA-91 | GRADE-MESTRA.md:102 | PENDENTE REAL | A-i e flip têm fechamento próprio (app/docs/BACKLOG.md:1790; app/docs/BACKLOG.md:1839), mas faltam causas/cura das divergências restantes: R1 atual totaliza 57 dias ESTRUTURA e investigação ainda incompleta (app/docs/RELATO.md:229; app/docs/RELATO.md:227; app/docs/RELATO.md:221); lei/! vigente e censo fechado desse restante [nao verificado] | espelho-verdade (O54) | lei_ou_bang=N | censo=N
BATERIA-QUEIXAS-ADMIN | GRADE-MESTRA.md:103 | PENDENTE REAL | Faltam validação Ronald+DP e selos após FIO MUDO (app/docs/BACKLOG.md:1901; app/docs/RELATO.md:2481); conclusão dos casos Q1–Q4, validação e censo fechado [nao verificado] | outras | lei_ou_bang=N | censo=N
TIPO-NA-FONTE | GRADE-MESTRA.md:104 | PENDENTE REAL | Falta corte LU2-03 entre selo do valor e acessor tipado, seguido da construção em cópia nova (app/docs/RELATO.md:344); censo explicitamente fechado, item 3 apenas na cópia 12e424d5f (app/docs/RELATO.md:827); dependências e parada registradas (app/docs/BACKLOG.md:1928) | contrato 20 | lei_ou_bang=S | censo=S

Notas para consolidação: foram pesquisados também RELATO-ARQUIVO.md e TICKETS.md, sem referência nominal às 14 obras. A atribuição dos grupos acima é inferência temática; principalmente “outras” versus O54 e “contrato 20” para TIPO/REABRIR deve seguir a convenção do consolidador. Ausência de censo fechado significa ausência de prova localizada, não prova de inexistência. A referência do rejulgamento em DIA-DA-BATIDA deve ser preferencialmente app/docs/RELATO.md:4263–4266 após conferência; não consegui fixar a linha individual no último recorte.

## 17/11 11:22 · sessao 5b2926384334 · - · - · origem=Ronald (tmux)

MOTOR-LE-O-TURNO | GRADE-MESTRA.md:105 | PENDENTE REAL | Faltam migrações PAR/DIA, RED, DIFF por dono e certificação DP (app/docs/BACKLOG.md:1997); herança própria de dia permanece (app/ponto/motor_calculo_v2.py:2407); censo medido com pontos cegos, fechamento não comprovado (app/docs/RELATO.md:2070) | outras | lei_ou_bang=N | censo=N
FIO-E0-V4-E1-ENTRADAS | GRADE-MESTRA.md:106 | FEITA | FECHADA expressamente: núcleo a8a692f55 e E1-FILA pousados; 16 complementos seguem em linhas próprias (app/docs/BACKLOG.md:2008; app/docs/RELATO.md:284) | - | lei_ou_bang=N | censo=N
FIO-E0-V4-E4-ESCRITORES | GRADE-MESTRA.md:107 | PENDENTE REAL | E4a/E4b pousadas, faltam leitores E4c e Contract E4d (app/docs/BACKLOG.md:2035; app/docs/BACKLOG.md:2044); B2 em construção (app/docs/RELATO.md:30); apply exportado espera ! (app/docs/RELATO.md:285); censo inicial confirmado, fecho global não comprovado (app/docs/RELATO.md:1838) | E4c | lei_ou_bang=S | censo=N
RAIZ-DA-DISPUTA | GRADE-MESTRA.md:108 | PENDENTE REAL | Programa F0–F7 continua aberto, incluindo F5 com E4d (app/docs/BACKLOG.md:2055); F0/F1 prontos (app/docs/BACKLOG.md:2032), mas BO-DISPUTA estacionada exige matriz conferida e aval (app/docs/BACKLOG.md:2049); censo integral fechado [nao verificado] | outras | lei_ou_bang=S | censo=N
LAVRA-PREMATURA-CONCORDE | GRADE-MESTRA.md:109 | PENDENTE REAL | Falta decisão de desenho sobre concorde com marco vencido faltando durante turno em curso; construção vedada (app/docs/BACKLOG.md:2141); censo fechado [nao verificado] | outras | lei_ou_bang=S | censo=N
RETRATO-POR-EVENTO | GRADE-MESTRA.md:110 | PENDENTE REAL | Faltam construtor, suite, preverificação, opus e pouso (app/docs/RELATO.md:1774); faltam evento empresa×competência, consumidor e frescor por evento (app/docs/BACKLOG.md:2199); censo entregue, fechamento não expresso (app/docs/RELATO.md:1812) | folha-online (O48) | lei_ou_bang=N | censo=N
JV1-MERGE-FILA | GRADE-MESTRA.md:111 | PENDENTE REAL | Faltam RED da janela nos leitores, alimentação além do envelope, contador do corte 21 e alimentação dos feriados em espelho/PDF (app/docs/BACKLOG.md:2221; app/docs/RELATO.md:2477); chamada PDF segue sem celulas= (app/relatorios/pdf_espelho.py:298); censo fechado [nao verificado] | outras | lei_ou_bang=N | censo=N
O168 | GRADE-MESTRA.md:117 | FEITA | Pouso 89a5b0d85 registrado (app/docs/BACKLOG.md:1513); NO AR d33c0a899 (app/docs/RELATO.md:890); guarda comum no gate do pai (app/chamados/models.py:718); achados restantes têm linha própria (app/docs/BACKLOG.md:1514) | - | lei_ou_bang=N | censo=N
O169 | GRADE-MESTRA.md:118 | FEITA | Cura C20-O169 FECHADA (app/docs/BACKLOG.md:1913), pouso bede09aa mantido (app/docs/RELATO.md:2403); fecho lê lastro do próprio dia independentemente da idade (app/ponto/services/cluster_espurio.py:53); carimbo persistente é pendência distinta O169-FECHADA (app/docs/RELATO.md:284) | - | lei_ou_bang=N | censo=N
FIO-E0-V4-E3-LEITORES | GRADE-MESTRA.md:119 | FEITA | Escopo recortado autorizado: resto vira obra própria (app/docs/BACKLOG.md:742); merge 90f3fa66b (app/docs/RELATO.md:614), deploy e smoke (app/docs/RELATO.md:597); R2 separado (app/docs/BACKLOG.md:761), R3 pousado f8ee770a5 (app/docs/BACKLOG.md:770); censo fechado (app/docs/RELATO.md:2001) | - | lei_ou_bang=N | censo=S
E3-CADASTRO-ADMIN | GRADE-MESTRA.md:120 | PENDENTE REAL | Falta correção administrativa dos cadastros listados; documento afirma que não se cura por código (app/docs/BACKLOG.md:2028); lista quantificada sem declaração de censo fechado (app/docs/BACKLOG.md:2028) | rabos da E3 | lei_ou_bang=N | censo=N
E3-ENTRADA-ORFA-SEM-TURNO | GRADE-MESTRA.md:121 | PENDENTE REAL | Falta lei para dia sem turno COM batida, fora da lei de passado sem marcos (app/docs/BACKLOG.md:2176); emissor permanece (app/chamados/services/disputa_emissao.py:257); censo fechado [nao verificado] | rabos da E3 | lei_ou_bang=S | censo=N

## 17/11 11:22 · sessao 5a7c2c88111a · - · - · origem=Ronald (tmux)

Item 3 — evidências e limites para consolidação:

- **Premissas:** o HEAD tem carimbo e recusa por mensagem, sem os quatro estados da proposta: `lavrar` grava em `MetricaSnapshot` (`app/folha/porta_export.py:768`); `conferir` lê hoje/empresa, compara competência e distingue valor negativo, positivo e zero (`app/folha/porta_export.py:807`, `:815`, `:818`, `:821`, `:867`); a view devolve a mensagem ao usuário (`app/folha/views.py:128`). A frase atual é “discordam em N ponto(s)” (`app/folha/porta_export.py:865`).
- **N conta divergências, não colaboradores únicos:** soma listas heterogêneas e dias HE (`app/folha/porta_export.py:687`), depois soma `falhas_proprias + cartao_x_txt` (`:762`). Portanto a retirada de “colaborador(es)” fecha com a unidade atual; a redução descritiva a “entre espelho e folha” cobre também PDF, builder, calendário, rubricas e HE apenas por interpretação, não por identidade literal (`:693`, `:725`).
- **Colisão:** o anexo fornecido autoriza substituir a frase e acrescentar FALHOU/R7; confronto dessa autorização com documento versionado local é [não verificado]. O código conserva “só zero libera” (`app/folha/porta_export.py:818`, `:821`, `:867`).
- **“Processo morreu”:** a porta atual não possui identidade de job, PID nem leitor de vitalidade; o snapshot só possui campos genéricos, detalhe e horário (`app/inteligencia/models.py:87`). Existe padrão concreto da casa: `disparar` cria subprocesso e grava PID/início (`app/relatorios/lote_espelho.py:117`, `:138`); `job_morreu` lê PID/início e usa `os.kill(pid,0)` (`app/relatorios/models.py:140`); `vigiar` grava ERRO e libera a deduplicação (`app/relatorios/lote_espelho.py:66`, `:93`); a tela chama esse vigia antes de apresentar estado (`app/relatorios/views.py:1270`). Sua ligação à medição do export ainda é [não verificado].
- **Quem escreve/lê estado, erro, PID e arquivo no padrão existente:** o serviço `disparar` escreve PID/início e cria `media/cartoes_lote/<id>.log` (`app/relatorios/lote_espelho.py:124`, `:138`); `job_morreu` lê o PID (`app/relatorios/models.py:151`); `vigiar` escreve ERRO, limpa PID, grava conclusão e erro (`app/relatorios/lote_espelho.py:75`); `_tela_lote_espelho` lê estado e placar (`app/relatorios/views.py:1276`). Esses são campos do lote de cartões, não um registro já existente da porta (`app/relatorios/models.py:106`, `:118`).
- **Logger:** há `logging.getLogger(__name__)`, portanto `folha.porta_export`, mas as chamadas existentes são WARNING (`app/folha/porta_export.py:83`, `:97`). O command usa stdout e `SystemExit(2)`, sem linha CRITICAL (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:89`, `:179`). A configuração SaaS fornece rota concreta possível via root → `appconsole`/`errfile`, sendo arquivo `/app/logs/errors.log` (`app/config/settings/saas.py:136`, `:183`). Quem emitirá CRITICAL e quem o lerá para que “suporte foi avisado” seja verdadeiro é [não verificado].
- **Outros detalhes que precisam fechar:** o command aceita somente mês/ano e percorre todas as empresas ativas, carimbando uma antes de passar à seguinte (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:27`, `:86`). Não se pode equiparar automaticamente seu código de saída à falha da empresa solicitada: pode existir carimbo zero de uma empresa antes de falha em outra (`:88`, `:92`, `:94`). O erro `erro_cartao_x_txt` nasce na medição (`app/folha/porta_export.py:665`, `:737`), mas `lavrar` não o inclui no detalhe, embora `conferir` tente lê-lo (`:776`, `:820`).
- **Exportada/dinheiro/!:** a mudança declarada cabe em apresentação e controle do job; o escritor mostrado grava snapshot (`app/folha/porta_export.py:772`). A medição já lê e separa divergências de exportada (`:339`, `:705`); não há autorização para mudar valores, cálculo ou TXT nos anexos fornecidos [não verificado]. A garantia da implementação futura e qualquer ocorrência real em produção são [não verificado].

## 17/11 11:21 · sessao 5ba3a53a7bea · - · - · origem=Ronald (tmux)

MODO-COMUM-OUT-DO-LOTE | GRADE-MESTRA.md:51 | FEITA | Pouso fc62165a e prova posterior do cartório em app/docs/RELATO.md:8939; portão cumprido, 0 decisão do ato lendo chave do lote em app/docs/BACKLOG.md:1036; selo atual em app/ponto/tests/test_out_do_lote.py:223 | - | lei_ou_bang=N | censo=N
BACKFILL-UNICO | GRADE-MESTRA.md:52 | PENDENTE REAL | Falta construir porta única, lista com dono, tratamento dos dormentes e decidir alcance sobre exportada; somente registrada em app/docs/BACKLOG.md:1037; espera atual de lei/! e fecho do censo [nao verificado] | outras | lei_ou_bang=N | censo=N
ISENTO-PONTO-JUIZ-REGISTRADO | GRADE-MESTRA.md:53 | PENDENTE REAL | Falta registrar autoridade e comparar predicado/universo antes da frase no CORTES, app/docs/BACKLOG.md:1045; função segue em app/core/constants.py:13 e leituras cruas em app/ponto/services/cartorio.py:1272; espera atual de lei/! e fecho do censo [nao verificado] | outras | lei_ou_bang=N | censo=N
RESPONDIDA-SEM-TEXTO-DUAS-COPIAS | GRADE-MESTRA.md:54 | FEITA | Cura #10 usando juiz em app/docs/RELATO.md:1109, merge E3 90f3fa66b em app/docs/RELATO.md:614 e deploy em app/docs/RELATO.md:597; predicado central em app/chamados/juizes.py:1531 e importado pelo vigia em app/chamados/services/vigia_fio.py:734 | - | lei_ou_bang=N | censo=N
DIA-DA-PERGUNTA-E-O-TURNO | GRADE-MESTRA.md:55 | PENDENTE REAL | Falta escritor único de data_evento/célula/dedup pelo data_turno; cópia anterior reprovada e medição em app/docs/BACKLOG.md:1115; censo dos três escritores em app/docs/RELATO.md:3309; espera atual de lei/! [nao verificado] | outras | lei_ou_bang=N | censo=S
O261-VETO-NA-PORTA | GRADE-MESTRA.md:56 | PENDENTE REAL | A no ar cdc7d8e2, mas faltam comparação posterior e lotes B/C, app/docs/BACKLOG.md:1116; faltam também mostra/trilha do ato humano em app/docs/BACKLOG.md:1106; censo em app/docs/RELATO.md:3338; espera atual de lei/! [nao verificado] | outras | lei_ou_bang=N | censo=S
EXPIRACAO-DA-ABERTA-COPIA-ANTIGA | GRADE-MESTRA.md:57 | PENDENTE REAL | Falta substituir cópia da expiração pelo juiz e guarda AST de janela_turno_de, app/docs/BACKLOG.md:1166; cálculo inline persiste em app/ponto/selecao_periodo.py:288 e app/ponto/selecao_periodo.py:290; espera atual de lei/! e fecho do censo [nao verificado] | outras | lei_ou_bang=N | censo=N
JUIZ-PERGUNTA-VIVA-MUDA-SEM-FATO | GRADE-MESTRA.md:58 | PENDENTE REAL | Cura 2db04de4 e deploy 4be784ff provados em app/docs/RELATO.md:3821 e app/docs/RELATO.md:3822, mas passivo de 9 disputas, cache na materialização e prova RED/trilha seguem incompletos em app/docs/RELATO.md:3823; complemento registrado em app/docs/BACKLOG.md:1168; censo das 9/106 na mesma linha; espera atual de lei/! [nao verificado] | outras | lei_ou_bang=N | censo=S
JUIZ-DE-DONO-DO-DIA | GRADE-MESTRA.md:59 | PENDENTE REAL | Falta predicado autorizado para dono do dia da pergunta; sem predicado novo agora, 787 perguntas/394 dias-colab em app/docs/BACKLOG.md:1170; PAROU aceito em app/docs/RELATO.md:3842; fecho do censo [nao verificado] | outras | lei_ou_bang=S | censo=N
VINCULO-LINHA-DO-TEMPO | GRADE-MESTRA.md:60 | PENDENTE REAL | Falta migrar leitores B ao vinculo_do_dia, medir passivo atual e instalar tripwire/EXCLUDE; censo entregue aponta ~55 sítios B, ausência da constraint e 11 selos ainda [nao verificado], app/docs/RELATO.md:283; porta já absorve posteriores, contrariando descrição antiga de app/docs/BACKLOG.md:1217; fecho integral do censo e espera atual de lei/! [nao verificado] | outras | lei_ou_bang=N | censo=N
O-DIA-PAGO | GRADE-MESTRA.md:61 | SUPERADA/ABSORVIDA | Estado delegado explicitamente às obras S3/S4/S5/S5b em app/docs/BACKLOG.md:1221 e app/docs/RELATO-ARQUIVO.md:19697; S3/S4 fechadas em app/docs/RELATO-ARQUIVO.md:19673 e app/docs/RELATO-ARQUIVO.md:19693; escrita S5b fechada mas troca ainda pede ! em app/docs/BACKLOG.md:1231 e app/docs/BACKLOG.md:1378 | - | lei_ou_bang=N | censo=N
PLACAR-ESTRUTURAL | GRADE-MESTRA.md:62 | PENDENTE REAL | Faltam resultados R3/R4/R6 e autorização dos casos pagos/exportados do R4 em app/docs/BACKLOG.md:1263; código mantém BATERIA parcial (6/7 famílias sem bateria) em app/core/placar_estrutural.py:99 e R4 parcial em app/core/placar_estrutural.py:211; fecho do censo global [nao verificado] | outras | lei_ou_bang=S | censo=N
L111-ORFAOS-DO-MOTOR | GRADE-MESTRA.md:63 | PENDENTE REAL | Falta conferir O275/O277 e remover com lápide os dois sítios sem chamador; fila sem construção e censo explicitamente fechado dos 202 nomes em app/docs/BACKLOG.md:1271, confirmado em app/docs/RELATO-ARQUIVO.md:1579; definições persistem em app/ponto/calculador/regras.py:302 e app/ponto/motor_calculo_v2.py:334; espera atual de lei/! [nao verificado] | outras | lei_ou_bang=N | censo=S

Consolidação deste trecho: FEITA=2, PENDENTE=10, SUPERADA=1, SEM_PROVA=0, TOTAL=13.

Ressalvas para consolidação: li as quatro fontes pedidas; TICKETS:244 também registra PLACAR parcial, mas está mais antigo que o código e o RELATO. O encadeamento de RESPONDIDA usa o marco E3; não há atualização nominal da linha antiga. VINCULO tem censo entregue, porém o fecho integral não está provado — por isso censo=N. A data atual dos passivos citados não foi remedida. Git log indisponível nesta cópia, conforme verificação do agente principal.

## 17/11 11:21 · sessao 887a9672f19a · - · - · origem=Ronald (tmux)

PERGUNTA-SEM-SELO | GRADE-MESTRA.md:37 | PENDENTE REAL | Faltam classes do censo além dos pousos parciais (app/docs/BACKLOG.md:806; app/docs/RELATO.md:4129); C7 tem censo FECHADO de 201 sítios e lei=0, mas só dessa classe (app/docs/RELATO.md:2015); lei/! atual e censo fechado integral [nao verificado] | outras | lei_ou_bang=N | censo=N
JUIZ-DO-PAR-DUAS-ENTRADAS | GRADE-MESTRA.md:38 | SUPERADA/ABSORVIDA | ABSORVIDA-POR JUIZ-DO-PAR-CURA-NA-FONTE (app/docs/RELATO.md:1142); cura na fonte presente em app/escala/utils.py:590 | - | lei_ou_bang=N | censo=N
COBERTURA-PARCIAL-UM-JUIZ | GRADE-MESTRA.md:39 | FEITA | FECHADA/NO AR 97bcb01c, prova de frota posterior à reabertura (app/docs/RELATO.md:8908; app/docs/TICKETS.md:156); autoridade parcial_so_lista em app/ponto/services/cartorio.py:398 | - | lei_ou_bang=N | censo=N
FIO-LE-O-JUIZ | GRADE-MESTRA.md:40 | FEITA | Pouso 7c9e4c27 (app/docs/RELATO.md:3835); push posteriormente FEITO 9185652e (app/docs/RELATO.md:3741); leitores atuais consultam pergunta_viva (app/chamados/services/fio.py:244; app/chamados/services/disputa_emissao.py:1988) | - | lei_ou_bang=N | censo=N
CXR-TELA-LE-O-MOTOR | GRADE-MESTRA.md:41 | FEITA | Pouso 94d41ea5 autorizado com filas separadas (app/docs/RELATO.md:2031; app/docs/RELATO.md:2038; app/docs/RELATO.md:2039); tela lê lavra do motor (app/escala/services/cadastro_realidade.py:129); confirmação posterior da primeira lavra [nao verificado] | - | lei_ou_bang=N | censo=N
C6-TETO-MARCO-PENDENTE | GRADE-MESTRA.md:42 | PENDENTE REAL | Falta cura do juiz para marco pendente e discordância com a lâmpada (app/docs/BACKLOG.md:906; app/docs/RELATO.md:3380); código ainda retorna viva pelo veredito acusador (app/chamados/juizes.py:1456); lei/! atual e censo fechado [nao verificado] | outras | lei_ou_bang=N | censo=N
PASSIVO-VALIDADO-K6 | GRADE-MESTRA.md:43 | PENDENTE REAL | Falta substituir cópia do predicado pelo juiz K6 (app/docs/BACKLOG.md:907); cópia permanece em app/chamados/services/passivo_validado.py:127; lei/! atual e censo fechado [nao verificado] | outras | lei_ou_bang=N | censo=N
PROGRAMA-CELULA-E-O-DIA | GRADE-MESTRA.md:44 | PENDENTE REAL | Programa inclui E1 e pista V3 (app/docs/BACKLOG.md:909; app/docs/BACKLOG.md:910); catraca de status já zerou (app/core/medida_leitor_do_dia.py:340), mas lei C16/C06 segue aberta (app/docs/RELATO.md:837); fecho global e censo fechado integral [nao verificado] | outras | lei_ou_bang=S | censo=N
GRADE-FOLGA-JUIZ | GRADE-MESTRA.md:45 | FEITA | Pouso 6f583cbc NO AR após opus aprovado e prova de frota (app/docs/RELATO.md:8803; app/docs/RELATO.md:8804); leitores consultam juiz único (app/ponto/services/declarar_dia.py:73) | - | lei_ou_bang=N | censo=N
A2-RAISE-NO-FUROS | GRADE-MESTRA.md:46 | PENDENTE REAL | Falta tratar recusa A2 na tela de furos (app/docs/BACKLOG.md:971); chamada sem tratamento local em app/relatorios/furos.py:198 e raise em app/escala/services/leitor_celula.py:288; lei/! atual e censo fechado [nao verificado] | outras | lei_ou_bang=N | censo=N
O261-E-F5C-PERGUNTA-SEM-ACAO-POSSIVEL | GRADE-MESTRA.md:47 | PENDENTE REAL | Falta registrar juiz do clique morto com corte exigido pela TRAVA JUIZ-NOVO (app/docs/BACKLOG.md:984); função existe em app/chamados/services/raiox.py:160, registro atual da família em app/core/juizes.py:285; censo fechado [nao verificado] | outras | lei_ou_bang=S | censo=N
E1-PERGUNTA-AO-DP-CONTADOR | GRADE-MESTRA.md:48 | PENDENTE REAL | Faltam respostas DP/contador aos casos legais (app/docs/BACKLOG.md:986; app/docs/RELATO.md:2985); C16/C06 ainda constam entre leis abertas (app/docs/RELATO.md:837); censo fechado [nao verificado] | outras | lei_ou_bang=S | censo=N
V-FASE-AS-OF-CADASTRO-SEM-LINHA | GRADE-MESTRA.md:49 | PENDENTE REAL | Falta listar ao admin dia passado com dono CADASTRO após relavra (app/docs/BACKLOG.md:991); C10 ainda limita leitura às janelas correntes da lavra (app/escala/services/cadastro_realidade.py:390); lei/! atual e censo fechado [nao verificado] | outras | lei_ou_bang=N | censo=N
BUG-ENC-CELULAS-FORMA | GRADE-MESTRA.md:50 | FEITA | Pouso 7825fec5b NO AR 15/11, árvore certificada 2b80dcdca (app/docs/RELATO.md:256; app/docs/TICKETS.md:1325); forma corrigida em app/ponto/turnos.py:2155 e tripwire singular em app/ponto/turnos.py:1866 | - | lei_ou_bang=N | censo=N

Conferidos os quatro documentos solicitados; RELATO-ARQUIVO não trouxe menções nominais adicionais para essas 14 obras. Git/HEAD verificável por histórico local indisponível nesta cópia sem `.git` [nao verificado].

Dúvidas para consolidação: CXR foi classificada FEITA pelo pouso explicitamente autorizado com resíduos transferidos a obras próprias; caso a unidade inclua tais resíduos, usar PENDENTE REAL e citar RELATO:2038-2039. PROGRAMA ficou PENDENTE por E1/V3, embora o instrumento diga que o programa termina em zero (app/core/medida_leitor_do_dia.py:4) e DECLARADOS esteja vazio (:340); fecho global nominal não localizado [nao verificado].

## 17/11 11:21 · sessao 38203a7fb05c · - · - · origem=Ronald (tmux)

**Lei: nenhuma** para a regra geral “disputa aberta cujo fato está na janela retém a pessoa”. Os textos candidatos tratam de assuntos próximos, sem transformar esse estado, sozinho, em condição de retenção [nao verificado — conclusão negativa da busca nos três documentos].

- **L-033:** resolve um subconjunto em sentido contrário: entrada e saída batidas, faltando apenas intervalo, é pagável; a pergunta permanece aberta sem bloquear. Logo, “toda disputa aberta retém” colidiria com essa exceção. `app/docs/LEIS.md:81`.
- **L-095:** determina que o valor seja lavrado e lido pelos consumidores; não estabelece retenção pelo estado da disputa. `app/docs/LEIS.md:113`.
- **L-099:** exige coerência do sistema e separa ESTRUTURA de CADASTRO/BATIDA, estes destinados à lista do admin. `app/docs/LEIS.md:122`.
- **L-113:** manda calcular o que existe e permite refazer exportação; não estabelece retenção por disputa. `app/docs/LEIS.md:155`.
- **L-129:** decide o dono do dia quando o colaborador deve resposta; a retenção continua vindo de outra leitura. `app/docs/LEIS.md:149`; `app/docs/CORTES.md:456`; `app/folha/fora_da_folha.py:18`; `app/folha/fora_da_folha.py:115`.
- O crivo escrito é o `entra` de `classificar_export`; “aprovado” é carimbo, gerar TXT não tranca e TXT/recalculo não consultam tranca. `app/docs/DOSSIES.md:120`; `app/docs/DOSSIES.md:122`.
- **Turno aberto** continua barrando o carimbo: o corte excluiu apenas `conta_vazia`. Esse corte não fala em **disputa aberta**. `app/docs/CORTES.md:317`.

Pergunta de lei proposta, **uma**: “Disputa aberta cujo fato pertence à janela deve, por si só, reter a pessoa, preservada a exceção já escrita da L-033?” [nao verificado — decisão ainda necessária].

## 17/11 11:21 · sessao 1811a98d7545 · - · - · origem=Ronald (tmux)

DOSSIES-DAS-CELULAS | GRADE-MESTRA.md:77 | SEM PROVA | BACKLOG mantém seis itens EM CURSO (app/docs/BACKLOG.md:1487); famílias verdes não comprovam fechamento integral, inclusive dieta de prosa (app/core/contratos_estruturais.py:168, app/core/contratos_estruturais.py:447); falta prova agregada | - | lei_ou_bang=N | censo=N
O142 | GRADE-MESTRA.md:78 | PENDENTE REAL | Cura pousada d1689254 (app/docs/RELATO-ARQUIVO.md:37752), mas falta comprovar invariância dos seis dias da borda direita e destino dos três rejulgamentos ainda nomeados como FALTA; censo fechado não comprovado (app/docs/TICKETS.md:209) | outras | lei_ou_bang=N | censo=N
O177 | GRADE-MESTRA.md:79 | SUPERADA/ABSORVIDA | O119 já soma pausas cadastradas e batidas nos dois sítios (app/ponto/juiz_batida.py:119, app/ponto/juiz_batida.py:271); NO AR 801253ed (app/docs/BACKLOG.md:1316), com DIFF de frota, reversão e 09 intacta (app/docs/RELATO-ARQUIVO.md:24025, app/docs/RELATO-ARQUIVO.md:24030) | - | lei_ou_bang=N | censo=N
O178 | GRADE-MESTRA.md:80 | PENDENTE REAL | Dar prazo e dono à pauta e publicar contador de pauta sem leitura; registro permanece ABERTA, sem prova de censo fechado (app/docs/BACKLOG.md:1524); prazo e leitura continuam campos opcionais (app/pautas/models.py:90) | outras | lei_ou_bang=N | censo=N
O179 | GRADE-MESTRA.md:81 | PENDENTE REAL | Medir horas reais dos três colaboradores no espelho e decidir dono ESTRUTURA/BATIDA pela L-099; causa ainda não medida e censo fechado não comprovado (app/docs/BACKLOG.md:1525) | outras | lei_ou_bang=N | censo=N
O180 | GRADE-MESTRA.md:82 | PENDENTE REAL | Entregar lista dos 102 colaboradores e obter ! para regenerar células do passado; registro de censo existe, fechamento não declarado (app/docs/BACKLOG.md:1526) | outras | lei_ou_bang=S | censo=N
O183 | GRADE-MESTRA.md:83 | PENDENTE REAL | Guarda existe (app/chamados/models.py:1132), mas falta prova do DIFF e tratamento do estoque de 494; censo de quatro escritores explicitamente fechado por AST (app/docs/BACKLOG.md:1529); família pousou pela O221 (app/docs/TICKETS.md:223) | outras | lei_ou_bang=N | censo=S
O229 | GRADE-MESTRA.md:84 | PENDENTE REAL | Nove achados aguardam cura/prova por classe; itens 1, 2 e 6 ainda pedem censo próprio (app/docs/BACKLOG.md:1557); filtro só de chegada permanece no código (app/ponto/services/espelho.py:1261) | outras | lei_ou_bang=N | censo=N
O216 | GRADE-MESTRA.md:85 | PENDENTE REAL | Remedir pela função real os três números da entrada da O191 após O195 e republicar; dívida explicitamente ABERTA e pré-aprovada, sem censo fechado comprovado (app/docs/BACKLOG.md:1559) | outras | lei_ou_bang=N | censo=N
SOBRA-NA-BORDA-O-QUE-FICOU | GRADE-MESTRA.md:86 | PENDENTE REAL | Item a exige obra para rejulgar passado quando mudam batidas; tentativa não certificada, cinco demais achados guardados e nada construído; censo fechado não comprovado (app/docs/BACKLOG.md:1584) | outras | lei_ou_bang=N | censo=N
ATA-NAO-DISPENSA-JUSTIFICADA | GRADE-MESTRA.md:87 | FEITA | Cura NO AR 44e268a8 (app/docs/RELATO.md:5397); acervo da 10 explicitamente APLICADO, reversão e 09 intacta (app/docs/RELATO.md:5360); resumo posterior repete bloqueio antigo de 79 células e contradiz aplicação detalhada (app/docs/RELATO.md:838) | - | lei_ou_bang=N | censo=N
CONCORDE-PREVISTO-ZERO | GRADE-MESTRA.md:88 | PENDENTE REAL | Investigar causa dos 730 dias concorde com previsto zero; foram excluídos sem investigação, censo fechado não comprovado (app/docs/BACKLOG.md:1608) | espelho-verdade (O54) | lei_ou_bang=N | censo=N
ESPELHO-VERDADE-REMEDIDO | GRADE-MESTRA.md:89 | FEITA | Atualizações pedidas aparecem em app/core/espelho_verdade.py:117, app/core/espelho_verdade.py:141, app/core/espelho_verdade.py:183 e app/core/espelho_verdade.py:187; termômetro publicado no POUSO R1-PUBLICA a16637ca (app/docs/RELATO.md:3128, app/docs/RELATO.md:3132) | - | lei_ou_bang=N | censo=N
C5-LISTA-DE-FROTA | GRADE-MESTRA.md:90 | SUPERADA/ABSORVIDA | Mesmo universo de 27 dias/18 colaboradores de 24/10 (app/docs/BACKLOG.md:1620) foi incorporado à C5-FALTA-MAIOR-QUE-PREVISTO, que exige remedida após pousos de 26–27/10 e dono L-099 por caso (app/docs/BACKLOG.md:1816) | - | lei_ou_bang=N | censo=N

## 17/11 11:21 · sessao 5ec202cd60b0 · - · - · origem=Ronald (tmux)

Achados:

1. **fila — N+1 restante no modal admin de disputa fechada.** `fio.py:348-349` chama o explicador sem perguntas pré-carregadas quando a disputa está fechada; `explicador.py:159-160` consulta perguntas sem `decisoes`; o laço `:171` lê `p.estado` em `:95`, que consulta `p.decisoes.all()` (`models.py:4267`). O prefetch de `pode_fechar` só vem depois do laço (`explicador.py:177`; `models.py:1976-1980`). O teto do app não cobre esse caminho (`api/tests/test_f2_teto_consultas_app.py:47-60`). A cadeia já existe no pai da complementação; custo executado [nao verificado].

2. **fila — N+1 restante no vigia.** `vigia_fio.py:570-572` carrega `perg_todas` sem decisões; `:615-616` passa-as a `dono_fora_do_fio`, cujo `:259` lê `p.estado` para cada pergunta até o `any` satisfazer. A leitura alcança `models.py:4267`. Já herdado da F2 anterior; contagem executada [nao verificado].

3. **fila / possível bloqueia pelo corte — outro DONO apresentado muda.** O compensador faz a validada ressuscitada tornar-se `PENDENTE` (`models.py:1673-1681`, `:3213-3222`). `explicador.py:95-99` traduz isso para `dono='colaborador'` e “Cobrar o colaborador”. Sem compensador, o ato VALIDACAO mantinha `VALIDADA` e o explicador chegava a dono cadastro/sistema (`explicador.py:137-143`, `:43`). O resumo agrupa esse dono (`:182-203`) e o modal mostra “Aguardando o COLABORADOR responder” (`templates/chamados/partials/modal_fio.html:467`). O próprio explicador declara que é descrição, sem decidir fechamento (`explicador.py:174`); a tarefa oficial ignora a marca (`tarefa_do_dia.py:426-427`). Portanto há mudança concreta do dono apresentado; efeito na fila oficial por essa descrição [nao verificado].

Censo para consolidação:

| Leitor | REABERTURA marcada / efeito |
|---|---|
| `DecisaoPergunta.leitura` | Não distingue marca; limpa validação, instante e texto (`models.py:4267-4284`). |
| `estado`, `respondida`, `resposta_display` | Herdam a leitura: pendente, não respondida, display vazio conforme os demais estados (`models.py:3213-3222`, `:3764`, `:3788`). |
| SQL validação / `q_vivas` | A REABERTURA conta como limpeza posterior; ressuscitada pode voltar ao SQL largo (`models.py:3529-3537`, `:3591-3594`). |
| `CargaDaTarefa` | Descarta somente marcada antes da escolha do último ato; comum continua devolvendo ao colaborador (`tarefa_do_dia.py:421-430`, `:163-164`). |
| CSF | `sem_veredito_entre` lê sequência com prefetch e exige resposta vigente sem validação/materialização (`models.py:1976-1985`); `perguntas_que_seguram_csf` aplica-o a pai e destino (`ponto/supra_juiz.py:573-575`). Mudança real na frota/exportada [nao verificado]. |
| Fio | Pré-carrega decisões e autores, inclusive foto; caminho principal não paga consulta de sequência por pergunta (`fio.py:257-264`). |
| Histórico | Toda REABERTURA entra em `ATOS_COM_FOTO` e em respostas anteriores (`models.py:4137`, `:3779-3782`); mostra texto, instante anterior, autor da validação, autor/motivo do ato (`modal_fio.html:212`). |
| Materialização | Rótulo BUG-PAR só reconhece DEVOLUCAO com prefixo próprio: não reconhece compensador (`materializacao.py:275-278`). Ausência rejeitada grava REABERTURA comum por outra porta, devolvendo ao colaborador (`:1158-1168`). |
| Backfill | Correspondência só de RESPOSTA/VALIDACAO; REABERTURA não impede fotos faltantes (`models.py:4216-4229`). `admin_decidiu_sem_ato` só considera SILENCIO/VALIDACAO e exige via/materialização que ressuscitar limpa (`backfill_decisoes_pergunta.py:99-105`, `:314-327`). |
| Cura de limbo | Não lê decisões: universo por projeção, conjunto nominal fechado; reabre pela porta comum (`curar_perguntas_em_limbo.py:46-52`, `:69`). |
| Auditor de invariantes | I18 e I20 leem projeção; nenhuma classificação da marca (`auditar_invariantes_chamados.py:1075-1078`, `:1106-1108`). |
| Foto E4b | Captura todas as decisões, incluindo linha e campo novos (`foto_e4b.py:280-281`). |

Correções de premissa:

- Não há compensador para toda ressuscitada: pergunta sem validação, instante e texto na leitura é explicitamente pulada (`models.py:1668-1669`). Isso inclui pendente sem atos e SILENCIO sem par vigente; número afetado [nao verificado].
- `ato_instante` não integra a declaração do modelo; o instante persistido é `ato_em` (`models.py:4139-4152`). `ato_instante` pedido pelo usuário [nao verificado].
- Contadores de reaberturas são campos de `DisputaSupervisao`, incrementados nas portas próprias (`models.py:1873-1877`, `:2294`, `:2380`); o compensador apenas acrescenta decisão (`:1672-1678`).
- CSF pode mudar **contra o pai F2**: resposta só no ato, projeção vazia/materializada antes, deixa de ser resposta após compensação; sem compensador, depois da limpeza passaria a segurar CSF (`models.py:1982-1984`, `:4283-4284`). O golden compara uma main anterior à F2; conservação de toda a frota [nao verificado].

## 17/11 11:21 · sessao a6104a0a2331 · - · - · origem=Ronald (tmux)

1. **bloqueia — compensador não desfaz atos futuros já existentes.** O escritor fixa `ato_em=timezone.now()` e ignora o maior instante da sequência (`app/chamados/models.py:1662`, `:1673`). A leitura ordena todos os atos por instante/pk, sem cortar futuro (`:4264`, `:4269`, `:4273`); a face SQL usa a mesma ordem (`:3530`). Uma `VALIDACAO` anterior na escrita, mas com instante futuro, fica depois da compensadora e continua válida (`:4279`). Prova isolada, sem banco ou escrita: executei os métodos reais extraídos por AST com validação às 13h e compensador às 12h; retornou `validada_em=13h`. Ocorrência em produção: [nao verificado].

2. **bloqueia — significado duplo de `ato_motivo` continua.** O campo segue controlando se `RESPOSTA` desfaz validação: vazio desfaz, preenchido conserva (`app/chamados/models.py:4241`, `:4251`). SQL repete o discriminador (`:3529`). A porta humana omite motivo (`app/chamados/services/disputa_emissao.py:1525`); escritores automáticos gravam textos (`:1678`, `:1699`). O teste novo compara apenas duas `REABERTURAS` com motivos iguais; não testa a semântica problemática de `RESPOSTA` (`app/chamados/tests/test_f1nova_b1_juiz_respondida.py:345`).

3. **bloqueia — cópia diverge do diff fornecido e da premissa “toda ressuscitada recebe compensador”.** Na cópia, leitura sem resposta nem validação executa `continue` (`app/chamados/models.py:1668`). Portanto pergunta materializada, sem resposta/validação, inclusive último `SILENCIO`, perde materialização sem nova `DecisaoPergunta` (`:1543`, `:1680`, `:3457`). O diff anexado não contém essa condição. A porta comum de reabertura considera expressamente a materialização retirada como motivo para registrar ato (`app/chamados/services/disputa_emissao.py:1638`, `:1646`).

4. **fila — foto pode atribuir validação ao autor errado em empate de instante.** `_vpor` escolhe a primeira validação com aquele instante (`app/chamados/models.py:1670`); a ordem padrão é crescente por instante/pk (`:4165`), enquanto a leitura deixa prevalecer a última (`:4273`, `:4279`). Duas validações no mesmo instante, autores A/B, resultam em validação vigente B e foto com A. O modal apresenta esse autor (`app/templates/chamados/partials/modal_fio.html:212`). Não há unicidade desse trio nas constraints (`app/chamados/models.py:4166`). Estoque desse caso: [nao verificado].

5. **Atomicidade/travas corretas no trecho examinado.** Fotos, compensador e limpeza estão sob o mesmo `atomic`, com a trava antes das escritas (`app/chamados/models.py:1629`, `:1634`, `:1635`, `:1678`, `:1680`). A implementação trava Colaborador→Disputa→Chamado→Pergunta (`:2830`, `:2840`, `:2853`, `:2857`). O log de ressuscitação fica depois da transação (`:1694`, `:1704`), mas a linha compensadora e a projeção não ficam separadas por isso.

6. **Idempotência sequencial preservada para esse escritor.** A seleção exige materialização (`app/chamados/models.py:1543`), a limpeza a remove (`:3457`) e a segunda chamada retorna zero antes das escritas (`:1616`). Seleção antes da trava permanece existente (`:1615`, `:1634`); execução concorrente sobre atos futuros continua vulnerável pelo achado 1: [nao verificado].

7. **Marca não duplica juiz de estado.** `es_da_ressuscitacao` verifica apenas tipo e campo persistido (`app/chamados/models.py:4183`); `CargaDaTarefa` exclui essas linhas antes de escolher a última decisão (`app/chamados/services/tarefa_do_dia.py:426`). O escritor nominal encontrado é `ressuscitar_perguntas` (`app/chamados/models.py:1677`). A constraint garante somente “marca implica reabertura”, não autenticidade da porta (`:4178`); `registrar` recebe dicionários genéricos (`:4288`, `:4298`). Nenhum segundo escritor atual encontrado; futuro escritor dinâmico: [nao verificado].

8. **`ato_instante` não está definido nesse modelo.** O conjunto de campos contém `ato_em`, autor, texto, motivo, fotos, ligações e marca (`app/chamados/models.py:4139`). Seu significado fora disso: [nao verificado]. A foto de conservação captura todos os campos concretos e todas as decisões, incluindo a nova marca (`app/chamados/services/foto_e4b.py:209`, `:280`).

## 17/11 11:20 · sessao 735199da21a3 · - · - · origem=Ronald (tmux)

1. **Bloqueia — `ato_motivo` continua decidindo estado.** A leitura de `RESPOSTA` desfaz validação somente quando o motivo é vazio, e a face SQL repete esse significado (`app/chamados/models.py:4241`, `app/chamados/models.py:4251`, `app/chamados/models.py:3529`). O teste novo chamado `achado5` usa apenas duas `REABERTURAS` com motivos iguais; prova a distinção pelo campo novo, mas não ataca a dupla função do motivo em `RESPOSTA` (`app/chamados/tests/test_f1nova_b1_juiz_respondida.py:346`, `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:358`).

2. **Bloqueia — RED(b) pode passar sem comparar a main.** Todas as comparações main × F2 dependem de `if os.path.exists(GOLDEN)`; não há falha caso o arquivo falte (`app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:149`). Nesta cópia, o golden está presente e contém tarefa e fila não vazias, além de CSF com P4 (`app/chamados/tests/f2_ressuscitar_tarefa_main.json:9`, `app/chamados/tests/f2_ressuscitar_tarefa_main.json:71`). O teste compara uma fixture de três ressuscitações; não mede as 2.025 unidades do anexo (`app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:111`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:146`). A alegada geração do golden pela main é **[nao verificado]**.

3. **Fila — escritor único é fiscalização estática, sem trava na porta.** O tripwire cobre kwargs, dicts literais, atribuição de atributo e `setattr` literal, exige exatamente um sítio autorizado e testa três sintaxes externas (`app/chamados/tests/test_f2_ressuscitacao_um_escritor.py:28`, `app/chamados/tests/test_f2_ressuscitacao_um_escritor.py:65`). A porta `registrar` aceita os campos fornecidos e faz `bulk_create`, sem conferir a origem da marca (`app/chamados/models.py:4288`, `app/chamados/models.py:4298`). A constraint permite qualquer `REABERTURA` marcada, independentemente de escritor (`app/chamados/models.py:4178`). Segundo escritor concreto nesta cópia: **[nao verificado]**.

4. **Selos que têm mordida estática identificável.** RED(a) exige validação anterior, ressuscitação efetiva, leitura/SQL desvalidadas, C6 vivo e continência em `q_vivas` (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:424`). RED(c) exige que a pergunta protegida mantenha estado e quantidade de decisões (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:448`). RED(b) impede CSF vazio por `assertIn('P4', ...)` e verifica competência exportada (`app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:106`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:147`). O teto do app impede resposta vazia, compara 1 × 5 perguntas e exige consultas iguais (`app/api/tests/test_f2_teto_consultas_app.py:54`). Execução desses testes: **[nao verificado]**.

5. **Fila — selo de ordem não percorre a inserção nova.** O mock retorna universo vazio para o compensador; a instrumentação continua cobrando somente captura → update → acordar (`app/chamados/tests/test_e4_expand.py:323`, `app/chamados/tests/test_e4_expand.py:327`). Há teste separado de segunda chamada sem nova trilha (`app/chamados/tests/test_f1nova_b1_juiz_respondida.py:381`).

6. **Pouso antes da migration não demonstrado na porta oficial.** A migration adiciona campo com default falso e depois constraint (`app/chamados/migrations/0054_decisao_da_ressuscitacao.py:15`). O deploy recusa `--sem-migrate`, reload agendado e reload da cópia quando há migration pendente; no caminho normal aplica migrations antes do HUP (`bin/deploy.sh:205`, `bin/deploy.sh:317`, `bin/deploy.sh:360`). Migration aplicada em produção: **[nao verificado]**.

## 17/11 11:20 · sessao 302818cc2ab8 · PERSPECTIVA-ESPINHA · sonda · origem=Code

ESTADO 17/11/2026: **19/20 verdes**, conforme a sonda fornecida e a contagem declarada pelo juiz; **GLOBAL verde**. (app/core/contratos_estruturais.py:1053; app/core/contratos_estruturais.py:1063; app/core/contratos_estruturais.py:81)

A única vermelha é **chamado:um juiz por pergunta**. (app/core/contratos_estruturais.py:262; app/core/contratos_estruturais.py:62)

| Verificação na cópia | Resultado | Fonte |
|---|---|---|
| Classe de teste existe? | Sim: `JuizChamadoContratoTest` | app/chamados/tests/test_contract_juiz_chamado.py:22 |
| `JUIZES_POR_VARREDURA` | Vazia: **0 itens** | app/config/crons.py:2056–2121 |
| `PENDENTES_CHAMADO` | Vazia: **0 itens** | app/core/juizes.py:482; app/core/juizes.py:1380 |
| Qual exceção está cheia? | **Nenhuma das duas declaradas** | app/core/contratos_estruturais.py:264; fontes acima |

**O impedimento registrado é a condição explícita que mantém a célula PARCIAL até O168, O169 e LASTRO**, com proibição de torná-la verde por reclassificação de papel. (app/core/contratos_estruturais.py:270) O RELATO confirma “VERMELHA POR DECLARAÇÃO”, com exceções vazias e desbloqueio dependente de aval após o fechamento dessas condições. (app/docs/RELATO.md:839)

A nota não demonstra que as três obras continuam integralmente pendentes: O168 já registra pouso, e O169-FECHADA registra pouso de uma fase. A satisfação integral da condição para liberar o verde permanece **[nao verificado]**. (app/docs/BACKLOG.md:1513; app/docs/RELATO.md:1445)

**Comparação com 26/10:** o registro das 21:1x já era **19/20**, portanto o resultado atual é igual, **delta 0**. Naquele dia houve queda intermediária para 18/20, antes da recuperação. (app/docs/RELATO.md:4414; app/docs/RELATO.md:4419; app/docs/RELATO.md:4486)

N/20 = 19; vermelhas = chamado:um juiz por pergunta

## 17/11 11:05 · sessao 10256d37c037 · E4C-F1-B2 · verificador · origem=Code

A consulta está antes da escrita, mas a cerca **não cobre exatamente a unidade**: há sobrealcance HX, subalcance pela Defesa 1 e contorno da recusa em fantasmas. (`app/chamados/reconciliador.py:557`; `app/chamados/services/disputa_emissao.py:1862`; `app/chamados/signals.py:43`; `app/ponto/management/commands/reconciliar_fantasmas.py:281`.)

**1. Posição correta; universo insuficiente e excessivo.**

`_retratar` abre o atomic, chama `trava_na_ordem`, consulta a cerca e retorna antes de `transicionar_com_pauta`. O veto recebe uma lista deduplicada de dias por empresa, obtidos por `_dia_de_turno_da_pergunta`. (`app/chamados/reconciliador.py:519`; `app/chamados/reconciliador.py:549`; `app/chamados/reconciliador.py:556`; `app/chamados/reconciliador.py:559`.)

A seleção direta é compartilhada com o resolvedor; inclui perguntas não materializadas de disputa aberta, inclusive validadas. As ancoradas abrangem também a relavra de destino não validado em disputa fechada. (`app/chamados/services/disputa_emissao.py:1827`; `app/chamados/services/disputa_emissao.py:1969`; `app/chamados/models.py:344`; `app/chamados/models.py:309`.)

**Sobrealcance: pelo menos dois casos-base montáveis**, sem relação da pergunta ENTREGUE com o chamado A retratado:

- Outra disputa aberta tem pai B vivo e pergunta ENTREGUE sem destino.
- Outra disputa aberta tem pai ausente/terminal, pergunta ENTREGUE sem destino e uma irmã com destino C vivo.

Ambos entram na cerca; o resolvedor pula o primeiro pelo pai vivo e o segundo pelo destino vivo. Tornando a pergunta ENTREGUE viva por célula acusadora, C7 impede também o fechamento final da disputa: A pode ser legitimamente retratado sem escrever nessa pergunta, mas a cerca recusa. São duas classes combináveis; número de ocorrências produtivas **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:1862`; `app/chamados/services/disputa_emissao.py:2090`; `app/chamados/services/disputa_emissao.py:2094`; `app/chamados/juizes.py:1454`; `app/chamados/models.py:1944`; `app/chamados/reconciliador.py:527`.)

**Subalcance bloqueante pela Defesa 1**, montável pelo encadeamento do código:

- A é retratado; outra disputa dB aberta tem pai B vivo e uma pergunta morta de competência ABERTA.
- O HX pula o carimbo de dB, mas o laço final ainda pode fechar dB: esse laço não conserva o pulo por credor vivo.
- O fechamento dispara Defesa 1 e transiciona B.
- Uma pergunta ENTREGUE, já materializada, não validada, em disputa dC FECHADA e com destino B ficou fora da enumeração de A; a transição de B relavra essa pergunta.

C7 examina as perguntas de dB; a guarda de vivas de B exclui competência ENTREGUE. Portanto essas guardas não impedem o caminho. Execução integrada desse cenário **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:1859`; `app/chamados/services/disputa_emissao.py:2091`; `app/chamados/services/disputa_emissao.py:2131`; `app/chamados/services/disputa_emissao.py:2148`; `app/chamados/models.py:1943`; `app/chamados/signals.py:43`; `app/chamados/models.py:364`; `app/chamados/models.py:309`.)

**2. Zero escrita na recusa local; ordem local compatível.**

No ramo que retorna pela nova cerca, chamado, justificativa, carimbo, lavra e ocorrência não são escritos: todos esses efeitos estão depois do retorno. Isso não garante zero escrita no chamador completo, especialmente no fallback de fantasmas descrito abaixo. (`app/chamados/reconciliador.py:558`; `app/chamados/reconciliador.py:559`; `app/chamados/reconciliador.py:571`; `app/chamados/reconciliador.py:572`; `app/chamados/reconciliador.py:574`; `app/ponto/management/commands/reconciliar_fantasmas.py:281`.)

A trava nova adquire Colaborador → Disputa → Chamado; perguntas, quando solicitadas, vêm por último. `_gravar_estado` e o envelope do resolvedor pedem o mesmo universo. Não identifiquei inversão nova nesses pontos; ausência universal de deadlock **[nao verificado]**. (`app/chamados/models.py:2781`; `app/chamados/models.py:2790`; `app/chamados/models.py:2803`; `app/chamados/models.py:2807`; `app/chamados/models.py:289`; `app/chamados/services/disputa_emissao.py:1943`.)

Não se pode estender “zero escrita” a qualquer `False`: a retenção anterior pode registrar auditoria antes de entrar na cerca. (`app/chamados/reconciliador.py:545`; `app/chamados/reconciliador.py:484`.)

**3. Quem escreve, quem lê e quem contorna.**

O delta de produção acrescenta seleção, trava e consulta; as persistências continuam nas portas existentes. O mapa abaixo identifica os escritores e leitores pertinentes; exaustividade de todos os escritores históricos da cópia **[nao verificado]**. (`app/chamados/reconciliador.py:505`; `app/chamados/services/disputa_emissao.py:1848`.)

| Campo | Escritores identificados | Leitores pertinentes |
|---|---|---|
| Pergunta `pk/disputa/conversa/chamado_destino/celula` | Nascimento: `app/chamados/models.py:3134`; reuso de célula/destino: `app/chamados/models.py:3085`, `app/chamados/models.py:3102` | Seleções: `app/chamados/services/disputa_emissao.py:1830`, `app/chamados/services/disputa_emissao.py:1860`; conversa: `app/chamados/reconciliador.py:520` |
| Pergunta `data_evento` | Ponto de atribuição: `app/chamados/services/fio.py:848` | Cerca: `app/chamados/reconciliador.py:523`; dia: `app/chamados/juizes.py:342`; veto: `app/chamados/services/fio.py:1365` |
| Chamado `pk/colaborador_id` | Nascimento: `app/chamados/models.py:1018` | Trava/carga HX: `app/chamados/reconciliador.py:556`; `app/chamados/services/disputa_emissao.py:1862` |
| Disputa `colaborador/chamado` | Nascimento: `app/chamados/services/disputa_emissao.py:601` | Seleção HX: `app/chamados/services/disputa_emissao.py:1839`; credor vivo: `app/chamados/services/disputa_emissao.py:2090` |
| Colaborador `empresa_id` | Edição/transferência: `app/colaboradores/views.py:1335`, `app/colaboradores/views.py:1417` | Agrupamento da cerca: `app/chamados/reconciliador.py:522` |
| Disputa `fechada_em/por/resolucao/motivo_fechamento` | Fechar: `app/chamados/models.py:2023`; reabrir: `app/chamados/models.py:2240`, `app/chamados/models.py:2264` | Seleções: `app/chamados/services/disputa_emissao.py:1833`, `app/chamados/services/disputa_emissao.py:1839`; receivers: `app/chamados/signals.py:29`, `app/chamados/signals.py:131` |
| Pergunta `materializada_em/por/via_resolucao` | Atribuição: `app/chamados/models.py:3218`; persistência: `app/chamados/models.py:3273`; limpeza em lote: `app/chamados/models.py:3402` | Seleção: `app/chamados/services/disputa_emissao.py:1845`; C6: `app/chamados/juizes.py:1389`; preservação de autoria: `app/chamados/models.py:3304`; histórico: `app/chamados/services/historico.py:103` |
| Pergunta `respondida_em/resposta_colab/validada_em/por` | Complementação pelo carimbo: `app/chamados/models.py:3251`; limpeza na ressuscitação: `app/chamados/models.py:1639` | C21: `app/chamados/juizes.py:1552`; universo de ressuscitação: `app/chamados/models.py:1533`, `app/chamados/models.py:1546`; histórico: `app/chamados/services/historico.py:99` |
| Pergunta `veredito_lavrado/julgado_em/janela_expira_em` | `app/chamados/veredito_lavrado.py:270`; lote: `app/chamados/veredito_lavrado.py:250` | `esta_stale`: `app/chamados/veredito_lavrado.py:224`, `app/chamados/veredito_lavrado.py:228` |
| Chamado `status_local/resolvido_em/via_encerramento/atualizado_em` | `_gravar_estado`: `app/chamados/models.py:228`, `app/chamados/models.py:296` | Gate/recarga: `app/chamados/models.py:674`, `app/chamados/models.py:761`; via no resolvedor: `app/chamados/services/disputa_emissao.py:1964`; leitor específico de apresentação de `atualizado_em`: **[nao verificado]** |
| Chamado `urgencia/urgencia_original` | Rebaixamento/restauração: `app/chamados/models.py:246`, `app/chamados/models.py:250` | Restauração: `app/chamados/models.py:250` |
| Justificativa `status/descricao` | `app/chamados/reconciliador.py:568` | Status nas portas DP: `app/ponto/services/justificativa.py:143`, `app/ponto/services/justificativa.py:278`; leitor específico do sufixo da descrição: **[nao verificado]** |
| Ocorrência `chamado/autor/conteudo/anexo_hash` | `app/chamados/services/nota.py:88`, `app/chamados/services/nota.py:103` | Histórico: `app/chamados/services/historico.py:80` |
| Auditoria de estado | `app/chamados/models.py:292` | Histórico: `app/chamados/services/historico.py:86` |
| Decisão da ressuscitação | `app/chamados/models.py:1621` | Projeção do histórico decisório: `app/chamados/juizes.py:1382` |

Os caminhos pedidos têm estas diferenças:

| Caminho | Alcance sem a cerca de `_retratar` |
|---|---|
| Fantasmas | Seleciona chamados VIVOS e, após `False`, chama diretamente o resolvedor. O resolvedor não exige estado encerrado; pode carimbar mortas ENTREGUE e o comando ainda pode anexar ocorrência. `app/ponto/management/commands/reconciliar_fantasmas.py:157`, `:280`, `:286`; `app/chamados/services/disputa_emissao.py:1964`, `:2045`, `:2117` |
| Órfãs/HIST | Conserva explicitamente o caminho anterior para ENTREGUE e chama o resolvedor. `app/chamados/reconciliador.py:1627`, `:1685` |
| Becos | Seleciona pai/destinos terminais e chama o resolvedor. `app/chamados/services/becos.py:21`, `:44`, `:55` |
| S143 | Exige ENCERRADOS e save de status; chama o resolvedor sem a nova cerca. `app/chamados/signals.py:97`, `:105`, `:117` |
| Defesa 1 | Chama `reconciliar` diretamente; pode alcançar a relavra ENTREGUE descrita no item 1. `app/chamados/signals.py:43`; `app/chamados/models.py:655`, `:309` |

Pelo delta fornecido, esses escritores e seleções são preexistentes; a extração dos filtros preserva o alcance anterior. A cerca cria **uma causa nova para “chamado ainda aberto + resolvedor direto”**: antes, nessa mesma tentativa bem-sucedida, a retratação transicionava; agora o veto devolve `False` e aciona o fallback. Comparação independente com o objeto Git de `708162b15` **[nao verificado]**. (`app/chamados/reconciliador.py:557`; `app/ponto/management/commands/reconciliar_fantasmas.py:280`; `app/chamados/services/disputa_emissao.py:1969`.)

**4. Recusa silenciosa: sem laço infinito demonstrado, com repetição e relatório falso.**

- Os dois reconciliadores contam somente `True`; `False` não entra em `falhas`. O consumidor pode marcar o evento como processado apesar da recusa. (`app/chamados/reconciliador.py:1040`; `app/chamados/reconciliador.py:1501`; `app/chamados/reconciliador.py:1431`; `app/chamados/reconciliador.py:1438`.)
- Justificativa devolve o booleano; aprovação e fechamento do chamado continuam sendo resultados distintos. A aprovação já salva antes da tentativa de encerramento. (`app/ponto/services/justificativa.py:46`; `app/ponto/services/justificativa.py:205`; `app/ponto/services/justificativa.py:217`.)
- Grade registra `RETRACT` antes da chamada e ignora seu retorno; imprime o plano sem separar recusas. (`app/ponto/management/commands/reconciliar_grade.py:303`; `:309`; `:318`.)
- Vínculo conta e anuncia apenas sucessos: esse contador permanece correto. (`app/ponto/management/commands/reconciliar_vinculo.py:68`; `:83`; `:84`.)
- **Geofence anuncia sucesso falso:** devolve todos os candidatos `perdeu`, inclusive recusados, e imprime seu tamanho como “chamados reconciliados”. (`app/ponto/services/geofence.py:434`; `:441`; `:448`; `app/ponto/management/commands/reconciliar_geofence.py:44`.)
- Alertas de turno ignora `False` e relata somente exceções. (`app/ponto/management/commands/processar_alertas_turno.py:216`; `:219`.)

Esses trechos percorrem candidatos, sem repetir sincronamente até obter `True`. Como a recusa conserva o chamado vivo, novas invocações podem repetir o trabalho; frequência e custo produtivos **[nao verificado]**. (`app/chamados/reconciliador.py:1032`; `:1493`; `:558`; `app/ponto/services/geofence.py:440`.)

**5. Semântica do dia correta, com distinção explícita da conversa.**

Para pergunta de fato, `_dia_de_turno_da_pergunta` retorna justamente `data_evento`, cujo contrato é dia do turno. Conversa grava dia civil e retorna `None` nesse juiz. O veto espera o dia usado na janela da empresa, não o mês civil nem o timestamp da batida. (`app/chamados/juizes.py:338`; `app/chamados/models.py:2898`; `app/chamados/models.py:2911`; `app/chamados/services/vigia_fio.py:802`.)

A exclusão de conversas usa o mesmo discriminador de `q_do_horizonte`; não reproduz seu recorte completo, que também exige data maior ou igual ao horizonte. Dia ausente recusa por `is not False`, conservando o significado de `None` do veto. (`app/chamados/reconciliador.py:520`; `:527`; `app/chamados/services/fio.py:1463`; `:1365`.)

**6. RED e mutantes: desenho verificável; execução Django não certificada.**

A matriz exigida pelo selo é esta; suas mortes efetivas nesta cópia **[nao verificado]**:

| Mutante | Casos que devem morrer por asserção |
|---|---|
| `sem_cerca` | a, b, b2, n |
| `so_direto` | b, b2 |
| `if_veto` | n |

Os patches fazem essas alterações; nenhum é equivalente ao original no domínio exercitado. O harness só aceita `AssertionError` com mensagem da recusa: erro operacional não conta como mordida. (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:46`; `:249`; `:252`; `:255`; `:242`.)

A fotografia compara linhas completas das quatro entidades principais; decisões, pautas, ocorrências e auditoria são verificadas somente por contagem. Os casos não exercitam sobrealcance HX, cascata por outro pai nem o chamador fantasmas. (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:125`; `:155`; `:206`.)

Executei os dois mutantes reancorados em memória, com bytecode desativado: **cada execução apresentou `Ran 1`, rc=0 e morte por `AssertionError`**. `funde_perdendo_validadas` mantém a substituição pela seleção ancorada; `evento_volta_varredura_global` mantém a varredura global, agora localizada no resolvedor. O harness exige alvo único, original verde e falha sem erro operacional. (`app/chamados/tests/test_e4b_classe_e_memoria.py:38`; `:143`; `:1043`; `:1050`; `:1057`.)

A ALLOWLIST cresceu duas seleções extraídas dos mesmos escritores, sem novo persistidor. Entretanto elas agora também são **leitores da cerca**: a justificativa “mesmo escritor” não demonstra que a nova enumeração seja exata. (`app/chamados/tests/test_contract_leitores_vitalidade.py:101`; `:102`; `app/chamados/services/disputa_emissao.py:1859`; `:1864`.)

Suíte Django inteira, pista, hashes de sombra e mortes executadas dos três mutantes da cerca: **[nao verificado]**. [nao verificado]

**7. Classes de reprovação.**

Não identifiquei segundo juiz de competência nem novo persistidor: a consulta reutiliza o veto e as escritas continuam nas portas existentes. Há, porém, **escritor indireto sem cobertura**, **derivação de alcance excessiva** e **selo sem cobertura desses contraexemplos**. (`app/chamados/reconciliador.py:517`; `app/chamados/signals.py:43`; `app/chamados/models.py:309`; `app/chamados/services/disputa_emissao.py:1862`; `app/chamados/tests/test_e4c_b2_cerca_retratar.py:206`.)

A ordem local de trava está preservada. O campo com dois significados, `data_evento`, é discriminado por `conversa` neste delta. Quanto ao relógio, a enumeração chama o universo de ressuscitação, que consulta C6 sem instante explícito; o fallback de relógio da máquina continua alcançável. Estabilidade atravessando a borda temporal **[nao verificado]**. (`app/chamados/reconciliador.py:556`; `app/chamados/juizes.py:342`; `app/chamados/services/disputa_emissao.py:1820`; `app/chamados/models.py:1564`; `app/chamados/juizes.py:596`.)

VEREDITO: REPROVA -- subalcance pela Defesa 1 em app/chamados/signals.py:43 e app/chamados/models.py:309; sobrealcance HX em app/chamados/services/disputa_emissao.py:1862; contorno da recusa em app/ponto/management/commands/reconciliar_fantasmas.py:281; sucesso falso em app/ponto/management/commands/reconciliar_geofence.py:44

_6503 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
