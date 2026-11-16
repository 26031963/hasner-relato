# CODEX-ESTADO

> Ultima resposta processada: **16/11/2026 18:53** · publicado em **16/11/2026 18:53** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 16/11 18:53 · sessao dbc9a9749f8d · ESPINHA-F2 · verificador · origem=Code

**AVAL-DEVOLVIDO em 2d e 9a:** o par ilegal não determina sozinho o dono da tarefa, e `_escrever_carimbo` não é escritor exclusivo dos três campos (`app/chamados/services/tarefa_do_dia.py:165`; `app/chamados/models.py:3393`).

As referências aos anexos usam suas linhas fornecidas, incluindo vazias; correspondência independente da cópia com o HEAD/hash informado: [nao verificado].

**2a — Fecha no ramo declarado.** Última RESPOSTA com `ato_texto` vazio retorna `respondida_sem_texto`, com RESPONDER do COLABORADOR; o juiz exige instante e texto não vazio (`app/chamados/services/tarefa_do_dia.py:159`; `app/chamados/services/tarefa_do_dia.py:161`; `app/chamados/juizes.py:1552`). A coerência quanto ao vazio confere, mas são fontes distintas: texto do último ato e texto projetado (`app/chamados/services/tarefa_do_dia.py:160`; `app/chamados/juizes.py:1552`).

Conservar essa fonte na F2 é compatível com conservar a exigibilidade e deixar a lei sobre devolução pendente; não revoga os destinos específicos de F5/F6 (`respostas_f2_estados_1611.txt:5`; `app/docs/RELATO.md:131`; `app/docs/CORTES.md:487`). Não autoriza mudança de tarefa nem intervenção em exportada (`complemento1_respostas_f2_estados_1611.txt:11`; `app/docs/CORTES.md:487`).

**2b — Fecha o defeito anterior de desenho; a prova efetiva continua pendente.** A API realmente calcula `aguarda_admin` por `estado == RESPONDIDA`; conservar essa fonte elimina a mudança adicional em pares legais que surgiria ao trocar estado por dono (`app/api/views.py:2550`; `app/api/views.py:2553`; `app/chamados/services/tarefa_do_dia.py:176`).

Isso é compatível com a prova que admite somente diferenças dos pares ilegais: essas diferenças são permitidas, não obrigatórias (`respostas_registradas.md:5`). Portanto, **essa alteração proposta já não introduz a diferença não admitida apontada anteriormente**; comprovar que nenhuma outra diferença existe exige a comparação por pk, cujo resultado é [nao verificado] (`respostas_registradas.md:5`). A conservação vale na F2; o destino pelo dono previsto para F6 continua registrado (`app/docs/CORTES.md:487`). Não há autorização adicional sobre exportada/dinheiro/`!` (`app/docs/CORTES.md:487`).

**2c — Fecha.** Ressuscitar não grava REABERTURA nesse caminho, e o código declara expressamente que devolver ao colaborador permanece lei não decidida; a porta efetiva zera a projeção pelo descarimbo em lote (`app/chamados/models.py:1622`; `app/chamados/models.py:1626`; `app/chamados/models.py:1629`).

Manter a discussão fora da F2 é compatível com a resposta 8 vigente e com o registro da lei; “ressuscitada sem ação” permanece restrito ao caso de último SILENCIO, não universal (`respostas_f2_estados_1611.txt:12`; `app/docs/RELATO.md:131`; `app/chamados/services/tarefa_do_dia.py:156`; `app/chamados/services/tarefa_do_dia.py:176`). A eventual obra futura depende do corte declarado, sem autorizar agora alteração de fila ou de exportadas (`complemento1_respostas_f2_estados_1611.txt:5`; `app/docs/LEIS.md:151`).

**2d — Não fecha como afirmação universal.** Pergunta viva com instante e sem texto realmente resulta em `estado=RESPONDIDA`, pois esse ramo lê `respondida_em` cru (`app/chamados/models.py:3154`; `app/chamados/models.py:3155`). Porém isso **não implica que a tarefa mande o colaborador responder**: a tarefa consulta o último ato; RESPOSTA vazia manda responder, DEVOLUCAO manda o admin validar e SILENCIO não exige ação (`app/chamados/services/tarefa_do_dia.py:158`; `app/chamados/services/tarefa_do_dia.py:161`; `app/chamados/services/tarefa_do_dia.py:165`; `app/chamados/services/tarefa_do_dia.py:176`).

A medição precisa separar o par projetado, o último ato e o resultado efetivo da tarefa; a conclusão sobre colaborador deve ficar condicionada ao ramo correspondente (`app/chamados/services/tarefa_do_dia.py:159`). Quantidade atual e ocorrência desses contraexemplos em produção: [nao verificado]. O encaminhamento de leitores de instante cru à B2 confere com o escopo documentado, mas não autoriza cura pela F2; estoque entregue continua exigindo `!` próprio (`app/chamados/juizes.py:1551`; `app/docs/LEIS.md:151`).

**9a — Não fecha a premissa nem demonstra o literal.** O comentário chama o trio de “UM ato”, e `_escrever_carimbo` atribui os três campos em memória; isso não equivale a escritor exclusivo de qualquer deles (`app/chamados/models.py:3184`; `app/chamados/models.py:3207`).

| Outro escritor/mecanismo | Escrita comprovada |
|---|---|
| `descarimbar_em_lote` | `update()` direto de instante, autor e via (`app/chamados/models.py:3391`; `app/chamados/models.py:3393`). |
| `encerrar_por_fato_no_espelho` | Atribui e salva somente a via (`app/chamados/models.py:3402`; `app/chamados/models.py:3403`). |
| `relavra_via.relavrar` | Atribui e salva somente a via, junto da lavra (`app/chamados/services/relavra_via.py:125`; `app/chamados/services/relavra_via.py:127`). |
| Exclusão do autor | A FK permite zerar `materializada_por` por `SET_NULL` (`app/chamados/models.py:3534`; `app/chamados/models.py:3536`). |

Os dois escritores exclusivos de via têm chamadores em comandos, e o próprio selo admite expressamente essas exceções (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:68`; `app/chamados/management/commands/relavrar_via_i6.py:102`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:44`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:51`). Incluindo testes, também existem `update()` diretos; nas migrações citadas há adição de campos de esquema, sem que isso sustente exclusividade do helper (`app/chamados/tests/test_e1_volta2.py:204`; `app/chamados/migrations/0016_s84_pergunta_chamado_destino.py:21`; `app/chamados/migrations/0033_perguntadisputa_via_resolucao.py:13`). Inexistência absoluta de outros mecanismos dinâmicos: [nao verificado]; o scanner declara limites para `__dict__` e nomes externos (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:111`).

O corte exige literalmente **“efeito = o ato de materializacao, valor conservado”** (`app/docs/CORTES.md:487`). A cópia registra F2 L1 como **RESIDUAL, não MOOT**, precisamente porque descarimbar apaga e a projeção lê o campo atual (`app/docs/RELATO.md:109`). O código confirma essa perda: o descarimbo elimina o instante, e a projeção temporal parte do valor atual, sem reconstruí-lo após apagado (`app/chamados/models.py:3318`; `app/chamados/juizes.py:1390`; `app/chamados/juizes.py:1396`). Portanto, “ler o campo é ler o ato” não fornece a fonte do instante anterior nem resolve o residual registrado (`app/docs/RELATO.md:109`).

**9b — Fecha como conservação do resultado presente; não sana 9a.** O descarimbo grava instante e autor como `None`, via como `''`, e salva esses campos quando solicitado (`app/chamados/models.py:3318`; `app/chamados/models.py:3320`; `app/chamados/models.py:3322`). Conservar esse resultado é compatível com a prova exigida, mas a execução comparativa antes/depois é [nao verificado] (`respostas_registradas.md:5`). Conservar `None` no presente não recupera o instante anterior referido pelo residual (`app/chamados/juizes.py:1390`; `app/docs/RELATO.md:109`). Não amplia autorização de intervenção em exportada (`app/docs/CORTES.md:487`).

**9c — Fecha como registro lateral; não sana 9a.** Os tipos de `DecisaoPergunta` não incluem materialização (`app/chamados/models.py:4024`; `app/chamados/models.py:4026`; `app/chamados/models.py:4035`). Retirar a remessa de F2 L1 à E4d elimina o desvio anterior; o item 3 do corte destina explicitamente à E4d o ato de absorção, enquanto o item 1 mantém o destino próprio do residual F2 L1 (`app/docs/CORTES.md:487`). Registrar a ausência de materialização não autoriza construir esse ato nem comprova o fechamento de 9a (`complemento1_respostas_f2_estados_1611.txt:10`; `complemento1_respostas_f2_estados_1611.txt:11`; `app/docs/RELATO.md:109`).

Quanto ao alcance material, existe ligação concreta com o CSF: ele consome `sem_veredito_entre` para pai e destino e decide sua permissão pelo resultado (`app/ponto/supra_juiz.py:573`; `app/ponto/supra_juiz.py:574`; `app/ponto/supra_juiz.py:588`). Assim, as exceções da prova não dispensam a restrição de exportadas: mudança exige `PAREI !` com tabela (`app/docs/CORTES.md:487`). Alteração efetiva de exportadas ou dinheiro pelo complemento: [nao verificado].

VEREDITO: AVAL-DEVOLVIDO em 2d, 9a -- 2d deduz dono apenas do par projetado, embora a tarefa dependa do último ato (`app/chamados/services/tarefa_do_dia.py:165`); 9a afirma escritor exclusivo inexistente e não resolve a fonte do instante apagado (`app/chamados/models.py:3393`; `app/docs/RELATO.md:109`; `app/docs/CORTES.md:487`).

## 16/11 18:52 · sessao 080c94e20fc7 · - · - · origem=Ronald (tmux)

A alegação literal de `_escrever_carimbo` como único escritor de qualquer dos três campos é **falsa**; o próprio selo permite quatro funções de escrita em `PerguntaDisputa` (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:43-58`):

- `_escrever_carimbo`: atribui os três campos em memória (`app/chamados/models.py:3207-3211`).
- `descarimbar_em_lote`: executa `UPDATE` direto dos três campos, sem chamar `_escrever_carimbo` (`app/chamados/models.py:3391-3393`).
- `encerrar_por_fato_no_espelho`: atribui e salva somente `via_resolucao`, preservando instante e autor (`app/chamados/models.py:3395-3403`).
- `relavra_via.relavrar`: atribui e salva somente `via_resolucao`, juntamente com a lavra (`app/chamados/services/relavra_via.py:118-128`). A documentação distingue expressamente “escritor único da re-lavra” de escritor único **do campo** (`app/chamados/services/relavra_via.py:58-64`).

Essas exceções têm chamadores de comando: `encerrar_perguntas_com_fato` chama a porta que troca somente a via; `relavrar_via_i6 --apply` chama `relavrar` (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:65-76`; `app/chamados/management/commands/relavrar_via_i6.py:96-102`).

Há ainda a zeragem declarativa de `materializada_por` por exclusão de usuário, porque a FK usa `SET_NULL`; o próprio selo registra que esse mecanismo fica fora de seu alcance (`app/chamados/models.py:3534-3537`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:113-115`).

Em testes há escritas diretas por `UPDATE`, inclusive dos três campos simultaneamente; portanto “em todo o repo” também não fecha se incluir testes (`app/chamados/tests/test_e1_volta2.py:204-206`; `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:169-170`).

As migrações encontradas para esses campos adicionam/alteram esquema: a 0016 adiciona instante e autor nulos, e a 0033 adiciona via com `default=''`; execução efetiva dessas migrações nesta base: [nao verificado] (`app/chamados/migrations/0016_s84_pergunta_chamado_destino.py:21-29`; `app/chamados/migrations/0033_perguntadisputa_via_resolucao.py:13-16`).

A simulação da ressuscitação chama `descarimbar(salvar=False)` em cópia; a ressuscitação efetiva chama `descarimbar_em_lote` (`app/chamados/models.py:1544-1557`; `app/chamados/models.py:1628-1630`). O homônimo de `RegularizacaoExterna` também recebe instante e autor, mas pertence a outro modelo (`app/chamados/services/regularizacao_ext.py:198-201`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:47-49`).

O levantamento por AST incluindo migrações, com tratamento de aliases/dicionários conforme o scanner existente, encontrou os quatro escritores de `PerguntaDisputa` acima; inexistência absoluta de outros mecanismos dinâmicos fora do alcance do scanner: [nao verificado] (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:99-115`). A identidade Git HEAD/hash não pôde ser confirmada porque a cópia não contém repositório Git: [nao verificado].

## 16/11 18:51 · sessao 2e39e403ec2d · - · - · origem=Ronald (tmux)

**9a — Não fecha a premissa nem o literal.** O comentário realmente chama os três campos de “UM ato”, mas `_escrever_carimbo` não é escritor exclusivo: `descarimbar_em_lote` executa `update()` direto dos três; `encerrar_por_fato_no_espelho` escreve apenas `via_resolucao`; o selo declara ainda `relavra_via.relavrar` como escritor da via (`app/chamados/models.py:3184`; `app/chamados/models.py:3391`; `app/chamados/models.py:3393`; `app/chamados/models.py:3402`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:44`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:51`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:57`).

O corte manda, se residual, **“efeito = o ato de materializacao, valor conservado”**; o registro da re-medição declara precisamente F2 L1 **RESIDUAL, não MOOT**, porque descarimbar apaga e a projeção consulta o campo atual (`app/docs/CORTES.md:487`; `app/docs/RELATO.md:109`). O código confirma: descarimbo apaga o instante; a projeção temporal começa pelo valor atual, apenas escondendo valores posteriores a `agora`, sem reconstruir um carimbo apagado (`app/chamados/models.py:3318`; `app/chamados/juizes.py:1390`; `app/chamados/juizes.py:1396`). “Ler o campo é ler o ato” não demonstra a fonte do instante anterior que permanece residual (`app/docs/RELATO.md:109`).

**9b — Fecha como conservação do resultado atual, não como resolução de 9a.** Descarimbar grava `None` para instante e autor, e `''` para via; portanto dizer genericamente “os três gravam None” seria inexato (`app/chamados/models.py:3318`; `app/chamados/models.py:3322`). Preservar esse resultado presente não fornece o instante apagado referido pela re-medição (`app/chamados/juizes.py:1390`; `app/docs/RELATO.md:109`).

**9c — A premissa confere; o registro lateral não sana 9a.** Os oito tipos de `DecisaoPergunta` não incluem materialização, e a remessa explícita do item 3 do corte é do predicado novo de absorção para E4d (`app/chamados/models.py:4024`; `app/chamados/models.py:4035`; `app/docs/CORTES.md:487`). Retirar a remessa de F2 L1 evita aquele desvio, mas não comprova cumprimento do destino residual do item 1 (`app/docs/CORTES.md:487`; `app/docs/RELATO.md:109`).

**Exportada/dinheiro/`!`:** o corte exige que `supra_juiz` em competência exportada não mude; eventual DIFF divergente exige `PAREI !` com tabela (`app/docs/CORTES.md:487`). Existe alcance indireto concreto: CSF lê `sem_veredito_entre` para pai e destino e decide a permissão de execução pelo resultado (`app/ponto/supra_juiz.py:573`; `app/ponto/supra_juiz.py:574`; `app/ponto/supra_juiz.py:588`). Invariância efetiva de frota e efeito monetário: [nao verificado].

## 16/11 18:51 · sessao 3618618bcf91 · - · - · origem=Ronald (tmux)

**2a — confere, no ramo declarado.** Última RESPOSTA com `ato_texto` vazio retorna `respondida_sem_texto` e RESPONDER do COLABORADOR; o juiz exige instante e texto não vazio na projeção. São leituras distintas, coerentes quanto ao vazio, sem equivalência garantida entre ato e projeção (`app/chamados/services/tarefa_do_dia.py:159`; `app/chamados/services/tarefa_do_dia.py:161`; `app/chamados/juizes.py:1546`; `app/chamados/juizes.py:1552`).

**2b — fecha o defeito anterior da conservação da API.** A API calcula `aguarda_admin` exclusivamente de `_estado == RESPONDIDA`; conservar essa expressão elimina a mudança adicional em pares legais que decorreria de consultar o dono (`app/api/views.py:2550`; `app/api/views.py:2553`; `app/chamados/services/tarefa_do_dia.py:176`). A prova efetiva main/cópia, por pk, continua [nao verificado]. O corte F6 trata do dono em F6; o corte F5 mantém resposta vazia fora do app e registra a pergunta de lei, permitindo delimitar a conservação proposta à F2 (`app/docs/CORTES.md:487`; `app/chamados/services/pergunta_no_app.py:81`).

**2c — confere.** Ressuscitar preserva a fila existente e não grava REABERTURA nesse caminho; a lei sobre devolver ao colaborador continua registrada e não decidida, inclusive junto da pergunta sobre resposta vazia (`app/chamados/models.py:1622`; `app/chamados/models.py:1626`; `app/chamados/models.py:1629`; `app/docs/RELATO.md:131`; `app/docs/LEIS.md:151`). Não autoriza correção nova na F2; eventual nova decisão e DIFF são futuro declarado no anexo [nao verificado].

**2d — não fecha como afirmação universal.** Viva com instante e texto vazio satisfaz o predicado ilegal e resulta em `estado=RESPONDIDA`, porque C6 viva exclui validação e o estado usa o instante cru (`app/chamados/juizes.py:1531`; `app/chamados/juizes.py:1534`; `app/chamados/juizes.py:1405`; `app/chamados/models.py:3151`; `app/chamados/models.py:3155`). Porém esse par projetado **não garante** RESPONDER do colaborador: a tarefa lê o último ato; RESPOSTA vazia produz colaborador, DEVOLUCAO produz admin, SILENCIO produz nenhuma ação (`app/chamados/services/tarefa_do_dia.py:158`; `app/chamados/services/tarefa_do_dia.py:161`; `app/chamados/services/tarefa_do_dia.py:165`; `app/chamados/services/tarefa_do_dia.py:176`). Ainda existem guardas anteriores e exposição administrativa específica (`app/chamados/services/tarefa_do_dia.py:298`; `app/chamados/services/tarefa_do_dia.py:306`; `app/chamados/services/tarefa_do_dia.py:309`; `app/chamados/services/tarefa_do_dia.py:267`).

Correção necessária: medir o conjunto I-sem-T e discriminar último ato/resultado real da tarefa; afirmar colaborador apenas no ramo correspondente (`app/chamados/services/tarefa_do_dia.py:159`). Quantidade atual e ocorrência dos contraexemplos: [nao verificado]. Destinar leitores de instante cru ao complemento da B2 corresponde ao escopo documentado B/C/D; mudar a tarefa não corresponde à B2 (`app/chamados/juizes.py:1550`; `app/chamados/juizes.py:1551`; `app/docs/LEIS.md:151`).

O dado fornecido não traz a saída dessa medição; os números históricos dos documentos não certificam a população atual [nao verificado]. `git rev-parse HEAD` falhou porque esta cópia não contém repositório Git; correspondência independente do HEAD com o hash fornecido: [nao verificado].

## 16/11 18:47 · sessao be76af41eae5 · AUSENCIA-EFEITO-PELO-ESTADO · verificador · origem=Code

**AVAL-DEVOLVIDO em a, b e c quanto às condições literais abaixo; d e e conferem.** A identidade do HEAD, a comparação com `71e9576bf` e o cruzamento por `git diff --name-only` ficam **[não verificado]**: esta cópia não contém metadados Git.

1. **As premissas conferem, com correção das linhas dos chips.**

| MUDA | Conferência na cópia |
|---|---|
| **a** | A coluna lê `a.efeito`, inclusive no `data-value`; a propriedade consulta somente `efeito_vigente(self.tipo)`, sem status. O laço existente permite entregar o veredito ao template. (`app/templates/ponto/ausencias.html:158`; `app/ponto/models.py:849-852`; `app/ponto/views.py:2249-2254`.) |
| **b** | Os chips contam por tipo, sem excluir rejeitadas: Abona em **2208**, Desconta em **2210**, Suprime em **2211**. As linhas 2205–2207 contam estados. (`app/ponto/views.py:2205-2211`.) |
| **c** | O filtro de efeito também consulta somente conjuntos de tipos; o filtro de status é aplicado separadamente. (`app/ponto/views.py:2212-2226`.) |
| **d** | Confere: `veredito_de` delega ao núcleo, que trata especialmente apenas `rejeitada`; os demais estados seguem o efeito/default do tipo. (`app/ponto/services/dia_decidido.py:461-481`.) |
| **e** | Os quatro leitores citados existem: turnos, fechamento, coletor de efeitos e calendário. A proteção por status varia conforme a alimentação, detalhada no censo abaixo. (`app/ponto/turnos.py:2704`; `app/ponto/services/fechamento.py:697`; `app/ponto/services/efeito_ausencia.py:58`; `app/colaboradores/services/calendario.py:705`.) |

Os **quatro status reais** são `aguardando_documento`, `aguardando_decisao`, `aprovada` e `rejeitada`. `pendente` na URL significa os dois estados abertos; `cancelada` não integra essas choices, e cancelar pela tela grava `rejeitada`. (`app/ponto/models.py:701-704`; `app/ponto/views.py:2212-2219`; `app/ponto/views.py:2378-2389`.)

`veredito_do_tipo` aceita os quatro — e aceita outros valores sem validá-los. Seus retornos são exatamente:

| Condição | Retorno |
|---|---|
| `status == 'rejeitada'` | `rejeitado` |
| Outro status, efeito DESCONTA | `descontado` |
| Outro status, efeito SUPRIME | `suprimido` |
| Qualquer outro caso | `abonado` |

Essa é a implementação completa, com as quatro strings declaradas no módulo. (`app/ponto/services/dia_decidido.py:26`; `app/ponto/services/dia_decidido.py:461-473`.)

**Quem escreve status e tipo:**

- **Nascimento:** `criar_ausencia` recebe o tipo, calcula o estado inicial e instancia o registro com ambos. (`app/ponto/services/ausencia.py:475-486`; `app/ponto/services/ausencia.py:529-531`.)
- **Alteração de tipo:** `editar_ausencia_registro` valida o catálogo, atribui/salva o tipo e manda rejulgar o estado aberto. (`app/ponto/services/ausencia.py:624-625`; `app/ponto/services/ausencia.py:648-676`.)
- **Transição normal de status:** `mudar_estado` valida a transição e atribui/salva o novo estado. (`app/ponto/services/ausencia.py:969-988`.)
- **Exceção concreta:** o cancelamento do agendamento de férias atribui diretamente `rejeitada`, com transação e trilha obrigatória; portanto não cabe afirmar que toda atribuição passa por `mudar_estado`. (`app/ponto/services/ausencia.py:1484-1507`.)

**Quem lê status e tipo, por responsabilidade:**

| Leitor | Uso |
|---|---|
| Modelo | Status para abertura/aprovação; tipo para exigência documental e efeito. (`app/ponto/models.py:719-741`; `app/ponto/models.py:849-852`.) |
| Serviço de ausência | Tipo/anexo para estado inicial; tipo/status para rejulgamento e transições. (`app/ponto/services/ausencia.py:687-692`; `app/ponto/services/ausencia.py:937-944`; `app/ponto/services/ausencia.py:969-976`.) |
| Cobertura | Status para folha/cobrança; tipo para cobertura indeterminada. (`app/ponto/turnos.py:2523-2546`.) |
| Lista/template | Tipo/status para filtros, contadores, apresentação e alerta médico. (`app/ponto/views.py:2195-2233`; `app/templates/ponto/ausencias.html:157-179`.) |
| Juiz visual/histórico | Tipo/status para veredito; estados abertos para apresentação do lançamento pendente. (`app/ponto/services/dia_decidido.py:447-481`; `app/ponto/services/dia_decidido.py:610-615`.) |
| Exportação/fechamento | Seleção de aprovadas e classificação por tipo/efeito. (`app/folha/export.py:87-91`; `app/ponto/services/fechamento.py:683-701`.) |

2. **Leis, aval vigente e decisão d.**

**a–c são compatíveis com juiz único** quando apenas transportam/classificam a resposta existente, sem reproduzir a regra de rejeição ou efeito na view. Essa é a exigência da LEI-AKITA 2 e da L2; `veredito_de` já delega ao mesmo núcleo. (`CLAUDE.md:20-22`; `app/docs/LEIS-UI.md:23-25`; `app/ponto/services/dia_decidido.py:453-481`.)

Há uma precisão documental: **`JUIZES.md` não nomeia `dia_decidido` nesse domínio**; ele remete ao registro vivo, que registra `catalogo.efeito_vigente` para “paga, desconta ou suprime”. A autoridade visual compartilhada está declarada no próprio `dia_decidido`. Usá-la não exige nascer outro juiz. (`app/docs/JUIZES.md:10-15`; `app/docs/JUIZES.md:55-58`; `app/core/juizes.py:260-265`; `app/ponto/services/dia_decidido.py:16-24`.)

A grade chama `do_dia`; o espelho publica sua palavra e o PDF consome essa palavra. Portanto a premissa de apresentação compartilhada confere, embora o PDF não precise chamar diretamente `veredito_de`. (`app/colaboradores/services/calendario.py:737-756`; `app/ponto/services/espelho.py:328-345`; `app/relatorios/pdf_espelho.py:923-924`.)

**d não colide com `veredito_de`.** Existe, porém, outra pergunta já resolvida: `veredito_do_lancamento` retorna PENDENTE para os estados abertos, conforme o contrato anterior de pendente amarelo. A decisão d deve permanecer explicitamente sobre a coluna **Efeito**, sem ser apresentada como o estado decidido do pedido. (`app/ponto/services/dia_decidido.py:37-40`; `app/ponto/services/dia_decidido.py:439-450`; `app/ponto/services/dia_decidido.py:461-473`.)

**L-123:** o padrão “juiz unico” já é termo admitido; fazê-lo valer exige que coluna, contador e filtro consumam a autoridade, conforme proposto. (`CLAUDE.md:65-70`; `app/docs/PLANO-PADROES.md:55-58`.)

**L7, em a:** o novo `title` não deve ser prosa colada no template; precisa vir do mapa de apresentação da autoridade. O texto prescrito pode ser preservado sem criar regra de veredito. (`app/docs/LEIS-UI.md:77-84`; `app/ponto/services/dia_decidido.py:476-481`.)

A documentação da cópia registra Repetir em construção e os complementos da UI-Vínculo antes desta obra; a ordem anexada é compatível com esses registros. O cruzamento efetivo entre arquivos das obras em voo permanece **[não verificado]**. (`app/docs/RELATO.md:9`; `app/docs/RELATO.md:20-33`.)

3. **Condições que fecham e condições que precisam de emenda.**

**Resultado 5: SIM, a partição por vereditos fecha para todo par solicitado**, inclusive tipo sem efeito e status `cancelada` ou outro: rejeitada entra exclusivamente em rejeitado; todo restante entra exclusivamente em descontado, suprimido ou abonado. Tipo ausente do catálogo produz `None`, que cai em **abonado**. Isso comprova a partição do juiz atual, sem comprovar que um tipo desconhecido deveria conceder abono. (`app/ponto/catalogo/ausencias.py:293-295`; `app/ponto/services/dia_decidido.py:461-473`.)

**b — o universo precisa ser explicitado:** Total hoje conta registros de `_base`, depois de vigência — ou busca por registro —, colaborador e tipo, **antes de status e efeito**. A lista recebe esses dois cortes depois; portanto Total não significa quantidade de linhas atualmente filtradas. (`app/ponto/views.py:2188-2226`; `app/ponto/views.py:2248-2255`.)

Assim, a soma pode fechar em `_base` e ainda não cumprir a **L1 literal**, que exige o mesmo queryset da lista e da ação. Os links dos chips conservam status/efeito, tornando essa distinção observável na navegação. É necessário resolver essa promessa de universo em b/c, não usar apenas a soma como prova de L1. (`app/docs/LEIS-UI.md:12-15`; `app/templates/ponto/ausencias.html:48-67`; `app/ponto/views.py:2201-2226`.)

**c — a comparação literal não fecha:** a URL pede `abona/desconta/suprime`, mas o juiz devolve `abonado/descontado/suprimido`. É preciso declarar o mapa entre os vocabulários antes de comparar “veredito == pedido”. (`app/templates/ponto/ausencias.html:65-67`; `app/ponto/services/dia_decidido.py:26`.)

O **OR de pares `(tipo,status)` é possível sem juiz novo**: o helper chama o juiz, soma `n` no balde correspondente e reúne exatamente os pares desse veredito. A montagem do predicado apenas transporta a resposta; não precisa testar rejeição ou consultar conjuntos de efeitos novamente. O caso de nenhum par deve produzir filtro vazio. (`app/ponto/services/dia_decidido.py:453-481`; `app/ponto/views.py:2195-2219`; `app/ponto/turnos.py:2773-2774`.)

**a — resultado 6 não fecha como igualdade textual literal:** `ABONA` não é `abonado`, nem `SEM EFEITO` é `rejeitado`. A propriedade deve exigir `data-value == veredito_de(a)` e texto igual à apresentação mapeada desse veredito. A coluna também contém o selo ANEXO!, que não integra o retorno do juiz. (`app/ponto/services/dia_decidido.py:26`; `app/ponto/services/dia_decidido.py:481`; `app/templates/ponto/ausencias.html:158`.)

**O selo é satisfazível:** retirar da contagem/filtro os três conjuntos proibidos e usar `veredito_do_tipo` basta; o núcleo consulta `efeito_vigente`, sem usar aqueles conjuntos. O catálogo pode continuar atendendo perguntas diferentes, como tipos médicos, parcialidade e fim aberto. (`app/ponto/views.py:2208-2211`; `app/ponto/views.py:2222-2253`; `app/ponto/services/dia_decidido.py:460-473`.)

RED executado, mutantes mortos, suite e smoke dos resultados propostos: **[não verificado]**. [nao verificado]

4. **Exportada, dinheiro, `!` e MEDIR.**

**O corpo de `lista_ausencias` é de consulta/renderização; a tela não é só leitura:** ela oferece lançar, editar, aprovar e cancelar, com POSTs. Os itens a–d podem ficar restritos à apresentação/seleção sem escrever dinheiro ou gravado exportado. (`app/ponto/views.py:2165-2270`; `app/templates/ponto/ausencias.html:43`; `app/templates/ponto/ausencias.html:249`; `app/templates/ponto/ausencias.html:277-312`.)

Alterar status pelas ações existentes pode acordar a projeção do fechamento; ampliar esta obra para esses escritores ou para gravado exportado ultrapassaria o escopo e alcançaria as regras de `!`. Conservar a propriedade em e preserva os leitores financeiros existentes. (`app/ponto/services/ausencia.py:987-994`; `CLAUDE.md:265-268`; `app/ponto/services/fechamento.py:697-701`.)

**MEDIR: rejeitada NÃO entra no mapa de `_cap(..., para='cobranca')`.** A cobertura aplica `_pelo_efeito`, que exclui rejeitada antes de construir o mapa; a leitura de `_a.efeito` está protegida a montante. (`app/colaboradores/services/calendario.py:516`; `app/ponto/turnos.py:2530-2531`; `app/ponto/turnos.py:2765-2766`; `app/colaboradores/services/calendario.py:705`.)

Censo dos demais leitores diretos de efeito de registro, distinguindo ausência de filtro local de proteção anterior:

| Leitor | Proteção por status |
|---|---|
| `Ausencia.efeito` | Nenhuma: consulta só tipo. (`app/ponto/models.py:849-852`.) |
| Coluna atual | Nenhuma obrigatória: a lista admite todos os status por padrão. (`app/templates/ponto/ausencias.html:158`; `app/ponto/views.py:2189-2219`.) |
| `tipo_contradiz_documento` | **Nenhuma:** exige documento e consulta efeito do tipo; alimenta ANEXO!, gate e título do chamado. (`app/ponto/services/ausencia.py:754-757`; `app/ponto/services/ausencia.py:765`; `app/ponto/services/ausencia.py:802`; `app/ponto/models.py:867-869`.) |
| `dias_com_ausencia_de_dia_inteiro` | Sem filtro local; recebe cobertura de folha nos chamadores atuais. (`app/ponto/turnos.py:2704-2709`; `app/ponto/services/fechamento.py:683-687`; `app/ponto/services/efeito_ausencia.py:53-65`; `app/ponto/management/commands/diff_calculador.py:412`.) |
| Fechamento | Cobertura `para='folha'`: somente aprovadas. (`app/ponto/services/fechamento.py:683-697`; `app/ponto/turnos.py:2528-2529`.) |
| Coletor `efeito_ausencia` | Mesma proteção de folha. (`app/ponto/services/efeito_ausencia.py:53-58`; `app/ponto/turnos.py:2528-2529`.) |
| Command `ausencias_sem_efeito_na_celula` | Cobertura de folha antes de `efeito_vigente(a.tipo)`. (`app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:44-49`.) |
| Núcleo `veredito_do_tipo` | Rejeitada retorna antes da consulta ao efeito. (`app/ponto/services/dia_decidido.py:461-468`.) |

O outro leitor sem filtro de status é, portanto, **a contradição documental**, expressamente preservada em e; os leitores financeiros citados recebem somente aprovadas. O calendário não fornece o RED adicional de rejeitada previsto condicionalmente no aval. (`app/ponto/services/ausencia.py:754-757`; `app/ponto/turnos.py:2528-2531`; `app/colaboradores/services/calendario.py:516`.)

VEREDITO: AVAL-DEVOLVIDO em a,b,c -- a: resultado 6 exige distinguir texto e veredito (`app/ponto/services/dia_decidido.py:26`; `app/templates/ponto/ausencias.html:158`); b/c: Total anterior aos filtros não prova L1 (`app/ponto/views.py:2201-2226`; `app/docs/LEIS-UI.md:12-15`); c: pedido e retorno usam vocabulários diferentes (`app/templates/ponto/ausencias.html:65-67`; `app/ponto/services/dia_decidido.py:26`).

## 16/11 18:45 · sessao db86dfe23a64 · BUG-PAR-O-PAR · verificador · origem=Code

A prova que bloqueou a volta 2 está preenchida: os onze mutantes pedidos têm execução Django e morte por asserção nos anexos. (`integ_c4b_todos.txt`, `RESUMO.txt:1–11`; respectivos logs, linhas `Creating test database`, `AssertionError`, `FAILED (failures=1)` e `rc=1`.)

**1. Matriz L1–L5**

O helper examina card e modal, restringindo o botão Validar ao formulário da pergunta examinada; a fotografia persistida inclui perguntas, decisões e identificadores das notas. (`app/chamados/tests/test_bug_par_volta.py:192–215`.)

| Linha | Teste e cobrança efetiva | Mutante e resultado EXECUTADO | [nao verificado]
|---|---|---|
| **L1** | `test_matriz_linha1_entregue_fora163_recusa_estado_proprio_esconde_validar`: estado `bloqueada_competencia_entregue`; detalhe nos dois HTMLs; Validar ausente; porta recusa; nenhuma batida; fotografia inalterada. (`app/chamados/tests/test_bug_par_volta.py:222–235`.) | `c4_fundir_l1_em_l2`: **rc=1; FAILED (failures=1)**. Morreu na asserção do estado: `AssertionError: 'bloqueada_competencia' != 'bloqueada_competencia_entregue'`. (`integ_c4b_todos.txt`, bloco `c4_fundir_l1_em_l2.log`, traceback em `test_bug_par_volta.py:229`.) |
| **L2** | `test_matriz_linha2_fechada_nao_entregue_conserva_o_toque_e_o_motivo`: F6 real `aprovado`; estado `bloqueada_competencia`; detalhe e Validar nos dois HTMLs; `nao_gravou`, devolução; VALIDACAO + DEVOLUCAO; validação/materialização vazias; motivo e nota de competência; nenhuma batida; irmã preservada. (`app/chamados/tests/test_bug_par_volta.py:217–220`, `:237–273`.) | `c4_motivo_l2_paridade`: **rc=1; FAILED (failures=1)**. `AssertionError: 'PARIDADE' != 'COMPETENCIA_APROVADA'`. (`integ_c4b_todos.txt`, bloco `c4_motivo_l2_paridade.log`, traceback em `test_bug_par_volta.py:258`.) |
| **L3** | `test_matriz_linha3_163_declarado_x_gravado_mostra_a_espera`: estado de espera; detalhe/espera visíveis nos dois HTMLs; Validar ausente; porta espera; nenhuma batida; fotografia inalterada. (`app/chamados/tests/test_bug_par_volta.py:275–287`.) | `c4_desligar_portao_l3`: **rc=1; FAILED (failures=1)**. `AssertionError: 'aplicavel' != 'espera_declarado_x_gravado'`. (`integ_c4b_todos.txt`, bloco `c4_desligar_portao_l3.log`, traceback em `test_bug_par_volta.py:281`.) |
| **L4** | `test_matriz_linha4_nada_barra_aplicavel_validar_grava`: `aplicavel`; Validar nos dois HTMLs; porta valida o par; ambas recebem VALIDACAO, carimbo e via de declaração; duas batidas novas e sequência E–S–E–S. (`app/chamados/tests/test_bug_par_volta.py:289–302`; `app/chamados/tests/test_bug_par_projetado.py:103–110`.) | `c4_barrar_l4`: **rc=1; FAILED (failures=1)**. `AssertionError: 'bloqueada_competencia_entregue' != 'aplicavel'`. (`integ_c4b_todos.txt`, bloco `c4_barrar_l4.log`, traceback em `test_bug_par_volta.py:294`.) |
| **L5** | `test_matriz_linha5_fora163_sem_dia_do_fato_estado_proprio`: estado próprio `bloqueada_sem_dia_do_fato`; detalhe nos dois HTMLs; Validar ausente; porta igual; K6 não consultado; nenhuma batida; fotografia inalterada. (`app/chamados/tests/test_bug_par_volta.py:304–321`.) | `c4_fundir_l5_em_l1`: **rc=1; FAILED (failures=1)**. `AssertionError: 'bloqueada_competencia_entregue' != 'bloqueada_sem_dia_do_fato'`. (`integ_c4b_todos.txt`, bloco `c4_fundir_l5_em_l1.log`, traceback em `test_bug_par_volta.py:312`.) |

Nenhuma dessas cinco linhas ficou sem mutante executado; nenhuma morte ocorreu por importação ou fixture: todos os blocos mostram o teste identificado, `AssertionError` e `FAILED (failures=1)`, sem `ERROR`. (`integ_c4b_todos.txt`, blocos dos cinco `c4_*` acima.) [nao verificado]

**2. Achados da volta 1 e integração herdada**

| Mutante | Asserção que o matou | Resultado EXECUTADO |
|---|---|---|
| `c4b_espera_antes_de_f6` | VOLTA primeiro terminou com PARIDADE em vez de COMPETENCIA_APROVADA. (`app/chamados/tests/test_bug_par_volta.py:364–376`.) | **rc=1; FAILED (failures=1)**; `AssertionError: 'PARIDADE' != 'COMPETENCIA_APROVADA'`. (`integ_c4b_todos.txt`, bloco homônimo, traceback `:374 → :355`.) |
| `c4b_motivo_laco` | Laço perdeu o motivo de competência nas metades. (`app/chamados/tests/test_bug_par_volta.py:378–389`.) | **rc=1; FAILED (failures=1)**; mesma divergência de categoria. (`integ_c4b_todos.txt`, bloco homônimo, traceback `:388 → :355`.) |
| `c4b_motivo_passivo` | Passivo contou `recusa:PARIDADE` duas vezes em vez de `recusa:COMPETENCIA_APROVADA`. (`app/chamados/tests/test_bug_par_volta.py:391–403`.) | **rc=1; FAILED (failures=1)**; `AssertionError: Tuples differ`. (`integ_c4b_todos.txt`, bloco homônimo, traceback `:402`.) |
| `fundir_a_em_b` | Estado da testemunha L1 fundido em L2. (`app/chamados/tests/test_bug_par_volta.py:229`.) | **rc=1; FAILED (failures=1)**; `AssertionError` entre os dois estados. (`integ_c4b_todos.txt`, bloco `fundir_a_em_b.log`.) |
| `fundir_a_em_b_porta` | Retorno da porta L1 fundido em L2. (`app/chamados/tests/test_bug_par_volta.py:233`.) | **rc=1; FAILED (failures=1)**; `AssertionError` entre os dois estados. (`integ_c4b_todos.txt`, bloco `fundir_a_em_b_porta.log`.) |
| `f6_esconde` | Testemunha L2 passou a anunciar estado de entrega. (`app/chamados/tests/test_bug_par_volta.py:247`.) | **rc=1; FAILED (failures=1)**; `AssertionError: 'bloqueada_competencia_entregue' != 'bloqueada_competencia'`. (`integ_c4b_todos.txt`, bloco `f6_esconde.log`.) |

As seis mortes também são por asserção, sem erro de importação/fixture. O RED agregado registra **21 testes, três falhas**, precisamente nos três contraexemplos c4b; o verde fornecido registra **108 testes, OK**. (`integ_c4b_todos.txt`, seis blocos acima; `red_c4b.log`, linhas `FAIL:`, `Ran 21 tests` e `FAILED (failures=3)`; `verde_c4b.log`, linhas `Ran 108 tests` e `OK`.) [nao verificado]

O `rc=1` desses logs é o resultado esperado do teste mutado; o harness da cópia considera morte válida somente falha sem erros/skips e retorna zero quando a verificação dos mutantes passa. (`app/chamados/tests/bug_par_mutantes.py:224–229`.)

**3. Contador**

**Existe e chega a zero pela validação do admin:** o teste novo exige `(pendentes_aval, resolvidas, congelados)=(1,0,1)`, chama `validar_pergunta`, confirma o par gravado e exige `(0,1,1)`, conservando a identidade da soma e o resultado de `competencia_do_fato`. (`app/chamados/tests/test_bug_par_volta.py:453–472`.)

A precisão é esta: K6 permanece **mock constante `False`**, herdado da fixture; o teste não altera sua resposta para produzir o zero. Isso atende ao contraexemplo de validação sem mudar o juiz durante o teste; exercício do juiz real e zero das 163 em produção são **[nao verificado]**. (`app/chamados/tests/test_bug_par_projetado.py:46–53`; `app/chamados/tests/test_bug_par_volta.py:461–472`.)

O verde agregado não enumera os testes; sua execução individual sob esse nome não é identificável no anexo. **[nao verificado]** (`verde_c4b.log`, linhas `Found 108 test(s)` e `Ran 108 tests`.) [nao verificado]

**4. `exportado`: escritores, leitores e alcance**

`exportado` é valor de **`FechamentoMensal.status`**. F6 o lê junto de `aprovado`; K6, separadamente, lê `ExportacaoDominio` vigente e não invalidada. Portanto esse status, sozinho, não prova entrega. (`app/ponto/models.py:398–410`; `app/ponto/janelas.py:83–89`; `app/folha/export.py:588–594`.)

Não localizei atribuição literal de produção para `status='exportado'`; a origem de eventuais registros existentes é **[nao verificado]**. Gerar TXT atualmente aprova os fechamentos abertos e registra `ExportacaoDominio`; a porta genérica de restauração aceita campos do modelo, mas uma chamada gravando esse valor não está demonstrada. (`app/folha/views.py:246–252`, `:295–300`; `app/ponto/services/fechamento.py:1329–1332`, `:1373–1393`.)

No caminho examinado:

- **Produção e transporte:** F6 produz o conflito; o chokepoint o devolve; a lista do par conserva conflitos classificados COMPETENCIA_APROVADA; o compositor preserva a mensagem. (`app/ponto/janelas.py:87–88`; `app/ponto/registro_batida.py:159–161`; `app/chamados/services/materializacao.py:1467–1471`, `:1783–1789`.)
- **Escrita:** `_carimbar_recusa` grava `recusa_motivo` e nota; o wrapper o chama novamente após o rollback do par. Competência não recebe carimbo terminal: esse ramo é reservado a COLISAO. (`app/chamados/services/materializacao.py:1648–1664`; `app/chamados/services/validacao.py:554–560`.)
- **Leitura:** devolução usa o motivo, limpa a validação e registra DEVOLUCAO; fotografia/auditoria e telas também o leem. `flip_auto`, leitor capaz de alimentar alteração de batida, exige via terminal de paridade, ausente nessa recusa F6. (`app/chamados/services/validacao.py:181`, `:258–278`, `:293–300`; `app/templates/chamados/partials/modal_fio.html:226`; `app/templates/chamados/partials/_card_atendimento.html:192`; `app/ponto/services/flip_auto.py:128–132`.)

**Não fica somente no payload:** muda pergunta, atos, nota e eventualmente reabertura; retirar `validada_em` também emite eventos de célula. Isso não equivale a provar edição da própria célula. (`app/chamados/services/validacao.py:265–288`; `app/chamados/models.py:3330–3358`; `app/chamados/signals.py:252–259`.)

**Batida:** F6 recusa antes do `create`. **Fechamento:** a porta já agenda recálculo antes da materialização; esse callback herdado pode atualizar `FechamentoMensal` e lavrar `DiaPago`, inclusive com status não aberto; a guarda explícita cobre 09/2026 entregue. Logo, seria incorreto afirmar que essa porta nunca toca fechamento exportado. (`app/ponto/registro_batida.py:159–161`, `:176`; `app/chamados/services/validacao.py:651–663`; `app/ponto/services/fechamento.py:1798–1824`, `:1145–1201`, `:36–43`, `:560–564`.)

O delta não acrescenta esse agendamento nem uma chamada de emissão de TXT; movimento financeiro ou alteração de TXT **causados pela troca espera→recusa** permanecem **[nao verificado]**. (`delta_c4_total.diff`, hunk `validacao.py:647`; `app/chamados/services/materializacao.py:1444–1472`; emissão em `app/folha/views.py:295–300`.)

**Matriz:** sendo K6 falso, o impedimento F6 pode enquadrar-se em **L2**. Sendo efetivamente entregue e U163, nenhuma linha literal cobre esse caso: L1 exige fora163 e L2 exige não entregue; a guarda pula U163. O status fornecido não resolve essa distinção, e o teste F6 usa `aprovado`, não `exportado`. (`anexo complemento2_espinha_bug_par_c4_1611.txt`, item 2, linhas L1/L2; `app/chamados/services/materializacao.py:799–801`; `app/chamados/tests/test_bug_par_volta.py:217–220`.)

**5. PROIBIDO**

Não identifiquei infração nova demonstrada no delta: a guarda compõe K6 existente, F6 continua compartilhado com o chokepoint, e card/modal decidem por **estado**, exibindo o detalhe. O transporte L2 usa o escritor canônico de recusa. (`app/chamados/services/materializacao.py:776–828`, `:1444–1471`, `:1640–1664`; `app/templates/chamados/partials/_pergunta_card.html:44–51`; `app/templates/chamados/partials/modal_fio.html:252–275`.)

Quanto a exportada, há efeito persistido na pergunta e o alcance herdado de recálculo descrito acima; novo movimento de dados financeiros provocado pelo delta não está demonstrado. **[nao verificado]** (`app/chamados/services/validacao.py:662`, `:269–278`; `app/ponto/services/fechamento.py:1810`, `:1160`.)

O JSON atual confere com o SHA esperado e conserva U60/U103 de 60/103 elementos; correspondência histórica independente dos commits, ausência histórica de toque em `motor_calculo_v2.py`, apply/pouso em produção e medição de perguntas vivas sem dia permanecem **[nao verificado]**. (`app/chamados/services/universo_bug_par.py:7–30`; `app/chamados/data/bug_par_163_2026_11_13.json:1`.)

VEREDITO: APROVA -- delta c4 e prova de integração exigida nesta volta: `integ_c4b_todos.txt/RESUMO.txt:1–11`; contador por validação: `app/chamados/tests/test_bug_par_volta.py:453–472`; sem nova infração demonstrada, com os limites de exportada registrados no item 4.

## 16/11 18:44 · sessao d773700f6943 · - · - · origem=Ronald (tmux)

Parecer de leis e escopo:

- **a** pode consumir `veredito_de` sem juiz novo: ele delega integralmente a `veredito_do_tipo`; o módulo declara a resposta visual única para grade/cartão/app (`app/ponto/services/dia_decidido.py:16-24,453-481`). O novo `title` precisa vir da apresentação compartilhada da autoridade: L7 proíbe ajuda hardcoded no template (`app/docs/LEIS-UI.md:77-84`). O anexo prescreve o texto, mas não obriga hardcode.
- **b/c** podem compartilhar um agrupamento por `(tipo,status)` e apenas traduzir respostas do juiz para contadores e predicados ORM; essa tradução não precisa decidir novamente efeito ou rejeição (`app/ponto/services/dia_decidido.py:453-481`). Devem mapear `abona/desconta/suprime` da URL para `abonado/descontado/suprimido` do juiz, pois os vocabulários diferem (`app/ponto/views.py:2220-2226`; `app/ponto/services/dia_decidido.py:26`).
- **d** não colide com `veredito_de`: somente `rejeitada` desvia do efeito/default; outros status seguem o catálogo (`app/ponto/services/dia_decidido.py:461-473`). Existe, entretanto, regra anterior expressa de **estado do lançamento** pendente amarelo, respondida por outra função, `veredito_do_lancamento`; não equiparar as duas perguntas (`app/ponto/services/dia_decidido.py:37-40,439-450`).
- **e** conservar a propriedade e o selo ANEXO! preserva a separação existente: `efeito` consulta só o tipo; `contradiz_documento` delega a `tipo_contradiz_documento` (`app/ponto/models.py:849-869`).

**Resultado 5:** a partição dos quatro vereditos fecha inclusive para tipo desconhecido/sem efeito e status `cancelada`/outro: catálogo pode devolver `None`, mas o juiz termina em `ABONADO` para qualquer valor diferente de DESCONTA/SUPRIME e qualquer status diferente de rejeitada (`app/ponto/catalogo/ausencias.py:293-295`; `app/ponto/services/dia_decidido.py:461-473`). Isso é a classificação literal atual, não prova de que o tipo desconhecido deveria juridicamente abonar.

**Universo/L1:** Total hoje é `_base.count()` depois de vigência/registro/colaborador/tipo e antes dos cortes por status e efeito (`app/ponto/views.py:2188-2226`). Portanto a igualdade pode fechar em `_base` sem ser igual ao comprimento da lista filtrada. Os chips preservam os filtros de status/efeito ao navegar (`app/templates/ponto/ausencias.html:48-67`), enquanto L1 exige o mesmo queryset da lista **e da ação** (`app/docs/LEIS-UI.md:12-15`). O aval deve declarar esse universo; não afirmar cumprimento integral da L1 apenas pela soma.

**Selo:** satisfazível se proibir os conjuntos na contagem/filtro diretamente em `lista_ausencias`; todos esses usos atuais estão em `app/ponto/views.py:2208-2211,2222-2226`. A autoridade pode continuar consultando catálogo, como faz em `app/ponto/services/dia_decidido.py:460-473`. A lista ainda usa legitimamente outras funções/conjuntos do catálogo (`app/ponto/views.py:2232,2247-2253`).

**Registro dos juízes:** `JUIZES.md` não registra `dia_decidido` como juiz de Ausencia; identifica-se como lookup e remete ao registro vivo (`app/docs/JUIZES.md:10-15,55-58`). O registro vivo nomeia `catalogo.efeito_vigente` para “paga, desconta ou suprime” (`app/core/juizes.py:260-265`). A autoridade visual está documentada no próprio módulo (`app/ponto/services/dia_decidido.py:16-24`). L-123 aceita o termo “juiz unico”, já presente no glossário (`CLAUDE.md:65-70`; `app/docs/GLOSSARIO.md:12`).

**Aval vigente/fila:** a documentação da cópia registra AUSENCIA-EFEITO aguardando novo aval e o editor Repetir em construção (`app/docs/RELATO.md:9,20-24`); os complementos 3/4 do painel estão descritos em `app/docs/RELATO.md:28-33`. Cruzamento por `git diff --name-only` não é verificável nesta cópia sem `.git` [não verificado].

**Somente leitura:** o corpo de `lista_ausencias` faz consultas e renderização (`app/ponto/views.py:2165-2270`); a **tela** tem lançar, editar, aprovar e cancelar (`app/templates/ponto/ausencias.html:43,117-121,249,273-282,304-312`). Cancelar chama a porta de decisão para gravar rejeitada (`app/ponto/views.py:2371-2389`). O escopo a–d não exige escrever dinheiro/exportada; ampliar para esses atos entra no rol de `!` de `CLAUDE.md:265-268`.

## 16/11 18:44 · sessao b205a16d7f54 · - · - · origem=Ronald (tmux)

Conferência do juiz e dos estados:

- Os quatro status reais são `aguardando_documento`, `aguardando_decisao`, `aprovada`, `rejeitada` (`app/ponto/models.py:701-704`). `pendente` é alias da URL para os dois estados `ABERTAS`, não status gravado (`app/ponto/services/ausencia.py:912`; `app/ponto/views.py:2212-2219`). `cancelada` não integra as choices nem a tabela de transições (`app/ponto/models.py:701-704`; `app/ponto/services/ausencia.py:914-923`).
- `veredito_do_tipo` aceita os quatro estados e qualquer outro valor sem validação: somente `status == 'rejeitada'` retorna `rejeitado`; nos demais casos consulta o catálogo, devolve `descontado` para DESCONTA, `suprimido` para SUPRIME e `abonado` para todo resto (`app/ponto/services/dia_decidido.py:460-473`). As strings são exatamente `abonado`, `descontado`, `suprimido`, `rejeitado` (`app/ponto/services/dia_decidido.py:26`).
- Tipo ausente do catálogo gera `None` no efeito e portanto `abonado` quando não rejeitado; vale igualmente para `cancelada`, `pendente`, outro status ou `None` (`app/ponto/catalogo/ausencias.py:293-295`; `app/ponto/services/dia_decidido.py:461-473`). Isso preserva a partição matemática por **vereditos**, embora não signifique que o catálogo concedeu abono (`mesmas referências`).
- `veredito_de(a)` só delega tipo/status a esse núcleo, logo d não colide com ele (`app/ponto/services/dia_decidido.py:476-481`).
- Há juiz diferente para o estado de **um lançamento**: `veredito_do_lancamento` devolve PENDENTE para ABERTAS (`app/ponto/services/dia_decidido.py:439-450`), com corte F2 que expressamente critica pintar de verde um atestado ainda não decidido (`app/ponto/services/dia_decidido.py:37-40`). O histórico o usa para mudar palavra/cor (`app/ponto/services/dia_decidido.py:598-615`). Isso é tensão semântica a declarar; a coluna “Efeito” pode continuar perguntando o efeito do tipo conforme d, mas a pergunta “em que pé está o pedido?” já tem outra resposta autorizada (`app/ponto/services/dia_decidido.py:442-445`).

Quem escreve:

| Campo | Escritor efetivo |
|---|---|
| tipo/status inicial | `criar_ausencia`: valida tipo no catálogo, calcula estado inicial, monta kwargs e instancia/salva (`app/ponto/services/ausencia.py:325-327`, `:475-486`, `:529-531`). |
| tipo editado | `editar_ausencia_registro`: valida catálogo e atribui/salva; depois rejulga o status aberto se mudou tipo (`app/ponto/services/ausencia.py:624-625`, `:646-659`, `:674-676`). |
| status por transição | `mudar_estado`: lê atual, valida transição e atribui/salva (`app/ponto/services/ausencia.py:969-988`). |
| status de férias canceladas | **Exceção concreta à frase “escritor único”**: `rejeitar_ferias_do_agendamento` atribui diretamente `'rejeitada'`, salva e exige trilha, sem chamar `mudar_estado` (`app/ponto/services/ausencia.py:1488-1507`). |

`Ausencia.save` não atribui status/tipo: lê os campos para carimbo de decisão, minutos de falta aprovada e duração/range (`app/ponto/models.py:882-887`, `:912-918`). O admin é `SoLeitura` (`app/ponto/admin.py:38`); API e views encaminham entradas às portas, por exemplo API `criar_ausencia` (`app/api/views.py:1812-1818`), edição de tela (`app/ponto/views.py:2873`), decisão/cancelamento (`app/ponto/views.py:2714`, `:2775`, `:2811`).

Leitores de status e tipo, com papéis concretos:

- Modelo: abertura lê status (`app/ponto/models.py:719-720`), pode aprovar lê status (`:741`), exige documento lê tipo (`:728-729`), efeito lê tipo (`:849-852`), save lê ambos (`:882-887`).
- Máquina de estado: estado inicial lê tipo/anexo (`app/ponto/services/ausencia.py:937-944`); rejulgamento lê status e tipo (`:687-692`); anexação lê status para transição (`:1033-1034`); reversão exige rejeitada (`:1275-1281`).
- Cobertura: `_pelo_efeito` lê status, aprovadas para folha e não rejeitadas para cobrança (`app/ponto/turnos.py:2523-2532`); cobertura/range lê tipo para NULL indeterminado (`:2543-2546`, `:2566-2570`). `ausencias_vigentes` lê tipo/range e **não filtra status** (`:3170-3178`).
- Lista: lê tipo em filtro (`app/ponto/views.py:2195-2196`), status em contadores/filtros (`:2197`, `:2205-2219`), tipo em contadores/filtros de efeito (`:2208-2211`, `:2221-2226`), tipo/status em alerta médico (`:2232`), tipo no laço (`:2253`).
- Template lista: tipo exibido (`app/templates/ponto/ausencias.html:157`), status pintado (`:177-179`), efeito atual por tipo (`:158`).
- Juiz visual lê tipo/status (`app/ponto/services/dia_decidido.py:481`); juiz de lançamento lê status (`:447-450`); histórico lê tipo/status (`:614-615`).
- Folha/export: queryset exige tipos e `status='aprovada'` (`app/folha/export.py:87-91`); fechamento lê status aprovado e tipo (`app/ponto/services/fechamento.py:662`, `:694`).
- Sinais leem tipo para expandir dias afetados (`app/ponto/signals.py:124`).

O Total atual conta `_base`, antes dos filtros de status/efeito, depois de vigência ou busca por registro, colaborador e tipo (`app/ponto/views.py:2188-2202`, `:2212-2226`). Portanto não é tamanho da lista já filtrada por status/efeito (`mesmas referências`).

## 16/11 18:44 · sessao c608cc5d094c · - · - · origem=Ronald (tmux)

MEDIR: **rejeitada não entra no mapa** de `calendario.py:516`. A chamada usa `para='cobranca'`; `cobertura_ausencia_periodo` aplica `_pelo_efeito`, que exclui `status='rejeitada'`, antes de construir o mapa; `_a.efeito` em `calendario.py:705` recebe esse universo protegido. (`app/colaboradores/services/calendario.py:516`; `app/ponto/turnos.py:2765`; `app/ponto/turnos.py:2530`; `app/ponto/turnos.py:2531`; `app/ponto/turnos.py:2766`; `app/colaboradores/services/calendario.py:705`)

Censo dos leitores diretos de registro, fora testes: [nao verificado]

| Leitor | Status a montante / consequência |
|---|---|
| `app/templates/ponto/ausencias.html:158` | Lê `a.efeito`; a lista admite registros de qualquer status por padrão e só filtra quando solicitado. (`app/ponto/views.py:2189`; `app/ponto/views.py:2212`) |
| `app/ponto/services/fechamento.py:697` | Recebe cobertura `para='folha'`, portanto somente aprovada; calcula justificadas/suprimidas. (`app/ponto/services/fechamento.py:683`; `app/ponto/turnos.py:2528`; `app/ponto/turnos.py:2529`; `app/ponto/services/fechamento.py:698`) |
| `app/ponto/services/efeito_ausencia.py:58` | Recebe cobertura `para='folha'`, portanto somente aprovada; produz justificadas e suprimidas. (`app/ponto/services/efeito_ausencia.py:53`; `app/ponto/turnos.py:2529`; `app/ponto/services/efeito_ausencia.py:59`) |
| `app/ponto/turnos.py:2704` | Não verifica status localmente, mas todos os chamadores atuais fornecem cobertura `para='folha'`. (`app/ponto/turnos.py:2709`; `app/ponto/services/efeito_ausencia.py:53`; `app/ponto/services/efeito_ausencia.py:65`; `app/ponto/services/fechamento.py:683`; `app/ponto/services/fechamento.py:687`; `app/ponto/management/commands/diff_calculador.py:412`) |
| `app/colaboradores/services/calendario.py:705` | Cobrança exclui rejeitada; aberto continua possível. (`app/colaboradores/services/calendario.py:516`; `app/ponto/turnos.py:2531`) |
| `app/ponto/services/ausencia.py:757` | `tipo_contradiz_documento` chama `efeito_vigente(ausencia.tipo)` **sem condição de status**; só exige documento. É a fonte de `contradiz_documento` e do `ANEXO!`, preservados pelo aval. (`app/ponto/services/ausencia.py:754`; `app/ponto/models.py:867`; `app/ponto/models.py:869`; `app/templates/ponto/ausencias.html:158`) |
| `app/ponto/services/dia_decidido.py:468` | `veredito_do_tipo` já retorna rejeitado antes da leitura do catálogo quando recebe `status='rejeitada'`. (`app/ponto/services/dia_decidido.py:461`; `app/ponto/services/dia_decidido.py:462`) |
| `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:49` | `efeito_vigente(a.tipo)` recebe cobertura `para='folha'`; só aprovada. (`app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:44`; `app/ponto/turnos.py:2529`) |

O outro leitor **sem filtro de status**, além da coluna atual e do núcleo de propriedade `Ausencia.efeito`, é `tipo_contradiz_documento`; seus consumidores adicionais são o gate de aprovação e o título do chamado. (`app/ponto/models.py:852`; `app/ponto/services/ausencia.py:757`; `app/ponto/services/ausencia.py:765`; `app/ponto/services/ausencia.py:802`)

`status_dia.py:311` lê `efeito_vigente` de `f['tipo_do_dia']`, um fato da precedência, e não diretamente de registro de Ausencia. (`app/ponto/status_dia.py:298`; `app/ponto/status_dia.py:308`; `app/ponto/status_dia.py:311`)

Para R5: tipo ausente do catálogo produz `None` em `efeito_vigente`; isso cai em **ABONADO** no juiz. Qualquer status diferente de rejeitada, inclusive cancelada ou desconhecido, segue o mesmo caminho; por isso a classificação por todos os pares não perde esses registros. (`app/ponto/catalogo/ausencias.py:293`; `app/ponto/catalogo/ausencias.py:295`; `app/ponto/services/dia_decidido.py:461`; `app/ponto/services/dia_decidido.py:468`; `app/ponto/services/dia_decidido.py:473`)

Atenção de semântica: há também `veredito_do_lancamento`, que devolve PENDENTE para ABERTAS; seu próprio texto distingue “em que pé está o pedido” de `veredito_de`, a pergunta de efeito. Assim d não contradiz o corpo de `veredito_de`, embora deliberadamente escolha efeito na coluna. (`app/ponto/services/dia_decidido.py:439`; `app/ponto/services/dia_decidido.py:442`; `app/ponto/services/dia_decidido.py:448`; `app/ponto/services/dia_decidido.py:450`; `app/ponto/services/dia_decidido.py:481`)

O registro vivo aponta `efeito_vigente` para “a ausência paga, desconta ou suprime o dia?”. (`app/core/juizes.py:261`)

## 16/11 18:43 · sessao 40dba8c4b7d0 · - · - · origem=Ronald (tmux)

- **`status='exportado'` pertence ao `FechamentoMensal`, não à pergunta nem ao juiz K6.** O modelo aceita esse valor; F6 lê `aprovado/exportado`. K6 lê `ExportacaoDominio` vigente e não invalidada. Portanto `status=exportado` não prova competência entregue. (`app/ponto/models.py:398-410`; `app/ponto/janelas.py:83-88`; `app/folha/export.py:588-594`)

- **Escritor desse status:** não localizei atribuição literal de produção para `exportado`; origem dos registros existentes **[nao verificado]**. Gerar TXT atualmente aprova os abertos e registra `ExportacaoDominio`, sem atribuir esse status. A porta genérica de restauração aceita campos do modelo, mas uma chamada que escreva `exportado` não está demonstrada. (`app/folha/views.py:246-252`, `:295-300`; `app/ponto/services/fechamento.py:1329-1332`, `:1373-1393`)

- **Leitores relevantes:** F6 lê o status e produz mensagem contendo `exportado`; `_competencia_que_barra` a reutiliza na testemunha e na espera tardia; `registrar_batida` consulta o mesmo F6 e retorna antes do `create`. A guarda K6 pula U163. (`app/ponto/janelas.py:86-88`; `app/chamados/services/materializacao.py:799-801`, `:824-828`, `:927`, `:1444-1452`; `app/ponto/registro_batida.py:159-161`, `:176`)

- **Quem escreve a recusa:** o chokepoint produz o conflito; o registro do par guarda conflitos classificados `COMPETENCIA_APROVADA`; o compositor preserva esse texto; `_carimbar_recusa` grava `recusa_motivo` e a nota no fio, sem carimbo terminal de competência. O wrapper refaz isso após rollback do par. (`app/chamados/services/materializacao.py:1462-1472`, `:1783-1789`, `:1648-1664`, `:1960`; `app/chamados/services/validacao.py:554-560`)

- **Quem lê a recusa:** devolução verifica motivo, limpa validação e registra DEVOLUCAO usando-o; fotógrafo/auditoria e telas também o apresentam. O leitor potencialmente ligado a alteração de batida, `flip_auto`, restringe-se à via terminal de paridade, ausente na recusa F6. (`app/chamados/services/validacao.py:258-278`, `:293-300`; `app/templates/chamados/partials/modal_fio.html:226`; `app/templates/chamados/partials/_card_atendimento.html:192`; `app/ponto/services/flip_auto.py:128-132`; `app/chamados/services/materializacao.py:1650-1660`)

- **Não é somente payload da porta:** a troca espera→recusa altera pergunta, motivo, atos, nota e eventualmente reabertura; limpar `validada_em` também emite eventos de célula. Isso não demonstra edição da própria `CelulaDia`. (`app/chamados/services/validacao.py:265-288`; `app/chamados/models.py:3330-3358`; `app/chamados/signals.py:252-259`)

- **Ressalva essencial sobre fechamento:** antes de materializar, a validação já agenda `recalcular_por_evento`; esse trecho é herdado, não acrescentado pelo delta fornecido. `_EsperaNaCriacao` desfazia apenas o atomic interno da unidade; o agendamento externo permanecia. O callback pode atualizar `FechamentoMensal`, inclusive status não aberto, e lavrar `DiaPago`; só há guarda explícita para 09/2026 entregue. Assim, não afirmar “essa porta nunca toca fechamento exportado”. O delta financeiro efetivamente causado pela troca de estado é **[nao verificado]**. (`app/chamados/services/validacao.py:651-663`; `app/chamados/services/materializacao.py:1881`, `:1944-1964`; `app/ponto/services/fechamento.py:1798-1824`, `:1145-1189`, `:1191-1201`, `:36-43`, `:560-564`)

- **TXT:** a emissão persiste `ExportacaoDominio` na porta específica; não há chamada nova dessa porta no ramo F6 examinado. Movimento executado de TXT neste caso **[nao verificado]**. (`app/folha/views.py:295-300`; `app/chamados/services/materializacao.py:1444-1472`, `:1640-1664`)

- **Linha da matriz, condicional:** se K6 falso, `exportado` pode cair em L2 pelo mesmo F6, pois status não prova entrega. Se K6 verdadeiro e pergunta U163, nenhuma linha literal cobre: L1 exige fora163, L2 exige não entregue; U163 pula a guarda K6. Os testes fornecidos usam F6 `aprovado`, sem prova executada do caso `exportado`. (`app/chamados/services/materializacao.py:799-801`; `app/folha/export.py:588-594`; `app/chamados/tests/test_bug_par_volta.py:213-216`; anexo COMPLEMENTO-2, item 2, linhas L1/L2)

**Conclusão para PROIBIDO:** há mudança comprovável na resposta persistida de perguntas associadas a F6 `exportado`; não há prova de novo escritor de competência/exportação nem de movimento financeiro causado pelo delta. A igualdade absoluta “só resposta da porta, nenhum efeito indireto” também não está provada. Os callbacks e suas permissões de escrita precisam aparecer como ressalva herdada, sem transformar possibilidade em dano executado. (`app/chamados/services/materializacao.py:1444-1472`, `:1659-1664`; `app/chamados/services/validacao.py:662`, `:269-278`; `app/ponto/services/fechamento.py:1810`, `:1160`)

## 16/11 18:42 · sessao 37963ea76ea3 · - · - · origem=Ronald (tmux)

Cobertura da matriz confirmada no código completo; o helper renderiza modal/card e restringe a busca de Validar ao formulário cujo `pergunta_id` é o da pergunta examinada. (`app/chamados/tests/test_bug_par_volta.py:192-210`)

| Linha | Cobrança efetiva |
|---|---|
| L1 | `test_matriz_linha1_entregue_fora163_recusa_estado_proprio_esconde_validar`: estado próprio, detalhe K6, Validar ausente nos dois HTMLs, retorno da porta, nenhuma batida e fotografia de perguntas/atos/notas inalterada. (`app/chamados/tests/test_bug_par_volta.py:222-235`; fotografia em `:212-215`) |
| L2 | `test_matriz_linha2_fechada_nao_entregue_conserva_o_toque_e_o_motivo`: FechamentoMensal real `aprovado`; estado, detalhe, Validar nos dois HTMLs; `nao_gravou` e devolução; nenhuma batida/carimbo; `validada_em=None`; motivo, DEVOLUCAO e nota classificados COMPETENCIA_APROVADA; irmã sem validação/materialização/recusa/ato novo. (`app/chamados/tests/test_bug_par_volta.py:217-220`, `:237-273`) |
| L3 | `test_matriz_linha3_163_declarado_x_gravado_mostra_a_espera`: estado/espera/detalhe, Validar ausente nos dois HTMLs, porta em espera, nenhuma batida e fotografia inalterada. (`app/chamados/tests/test_bug_par_volta.py:275-287`) |
| L4 | `test_matriz_linha4_nada_barra_aplicavel_validar_grava`: aplicável, Validar nos dois HTMLs, porta valida o par, ambas recebem VALIDACAO; helper exige validação/carimbo/via declaração e as duas batidas E-S-E-S. (`app/chamados/tests/test_bug_par_volta.py:289-302`; `app/chamados/tests/test_bug_par_projetado.py:103-110`) |
| L5 | `test_matriz_linha5_fora163_sem_dia_do_fato_estado_proprio`: estado próprio e detalhe, Validar ausente nos dois HTMLs; porta igual; K6 não consultado; nenhuma batida e fotografia inalterada. (`app/chamados/tests/test_bug_par_volta.py:304-321`) |

Os cinco mutantes são precisamente os contratados: `c4_fundir_l1_em_l2`, `c4_motivo_l2_paridade`, `c4_desligar_portao_l3`, `c4_barrar_l4`, `c4_fundir_l5_em_l1`. (`app/chamados/tests/bug_par_mutantes.py:123-137`) Cada log individual fornecido termina em `rc=1`, `FAILED (failures=1)`, e exibe `AssertionError` no teste correspondente; nenhuma dessas cinco mortes foi por import/fixture. (`integ_c4b_todos.txt`, blocos dos respectivos `.log`, linhas `AssertionError`, `FAILED` e `rc=1`; referências exatas desses logs estão no texto do usuário)

O novo teste F4 existe e demonstra a passagem `(pendentes_aval,resolvidas,congelados)=(1,0,1)→(0,1,1)` chamando `validar_pergunta` com o admin, confirmando o par gravado e comparando `competencia_do_fato` antes/depois. (`app/chamados/tests/test_bug_par_volta.py:453-472`) A ressalva precisa: seu juiz de entrega é **mock constante False**, herdado de `_Col960Fixture`; ele não é alternado durante esse teste. (`app/chamados/tests/test_bug_par_projetado.py:46-53`) Logo, satisfaz “zera pela validação sem mudar a resposta do juiz”; execução do juiz real ou zero do universo de produção permanece [nao verificado]. O teste antigo zerava ao trocar o mock para True, diferença agora corrigida pelo novo contraexemplo. (`app/chamados/tests/test_bug_par_103.py:87-93`; `app/chamados/tests/test_bug_par_volta.py:461-472`)

Para L5, o código põe uma U103 carimbada sem dia em `bug_par_103_sem_dia` antes do ramo entregue; o comando imprime “sem dia do fato” tanto para U103 quanto para fora163. (`app/chamados/services/materializacao.py:330-336`; `app/chamados/management/commands/bug_par_103.py:58-63`, `:74-77`, `:137-140`) O teste complementar cobra as duas linhas falantes e nenhuma escrita da porta de devolução, mas não asserta diretamente o balde do contador. (`app/chamados/tests/test_bug_par_103.py:101-123`) A pergunta L5 da matriz está fora163, portanto nem participa do contador cuja consulta é exclusivamente por U163. (`app/chamados/tests/test_bug_par_volta.py:24-34`, `:304-321`; `app/chamados/services/materializacao.py:321-324`)

## 16/11 18:41 · sessao 5075b3e1d8b9 · UI-EDITOR-CAL-REPETIR-CONFIRMA · verificador · origem=Code

A análise abaixo é estática. Execução dos testes, mutantes efetivamente executados e correspondência da cópia com o hash informado: **[não verificado]**. O recorte adotado é o delta anexado; sua equivalência ao histórico Git: **[não verificado]**. [nao verificado]

**1) Quem escreve e quem lê**

| Valor | Escritor → leitor |
|---|---|
| `ESCAPE`, `FUNDO`, `resposta`, `_responde` | As constantes contêm JavaScript; os dois testes passam a constante ao helper, que a encaminha a `repete`. `_responde` preserva exatamente essas duas constantes; para as demais respostas, produz clique no elemento cujo ID recebeu. O resultado entra em `%(responde)s` e só executa quando o fundo está aberto. (`app/escala/tests/test_smoke_clique_folga_calendario.py:227`, `app/escala/tests/test_smoke_clique_folga_calendario.py:231`, `app/escala/tests/test_smoke_clique_folga_calendario.py:239`, `app/escala/tests/test_smoke_clique_folga_calendario.py:148`, `app/escala/tests/test_smoke_clique_folga_calendario.py:340`, `app/escala/tests/test_smoke_clique_folga_calendario.py:343`) |
| `SIM`, `NAO` | São IDs dos botões. `repete` assume `NAO`; o teste novo de fevereiro também o fornece explicitamente. `_responde` transforma esse ID em clique. (`app/escala/tests/test_smoke_clique_folga_calendario.py:189`, `app/escala/tests/test_smoke_clique_folga_calendario.py:232`, `app/escala/tests/test_smoke_clique_folga_calendario.py:235`, `app/escala/tests/test_smoke_clique_folga_calendario.py:348`) |
| `pagina`, `porta`, `args` | `_repete` resolve a rota, faz GET, exige HTTP 200 e entrega o HTML a `repete`; `_dom` injeta a sonda antes de `</body>` e abre o HTML no Chromium. Criar e editar apontam para a mesma view. (`app/escala/tests/test_smoke_clique_folga_calendario.py:312`, `app/escala/tests/test_smoke_clique_folga_calendario.py:192`, `app/escala/urls.py:35`) |
| `id`, `preenche`, `tam`, `comum`, `responde` | `repete` monta esses parâmetros: `id=VEREDITO`, `comum=COMUM`, e os demais vêm do chamador ou de `_responde`. A sonda usa `id` no elemento de resultado; executa `preenche`; usa `tam` para selecionar botão, campos, contador e grade; incorpora as funções de visibilidade e linhas de `COMUM`; executa `responde` após registrar o modal. (`app/escala/tests/test_smoke_clique_folga_calendario.py:239`, `app/escala/tests/test_smoke_clique_folga_calendario.py:126`, `app/escala/tests/test_smoke_clique_folga_calendario.py:142`, `app/escala/tests/test_smoke_clique_folga_calendario.py:144`, `app/escala/tests/test_smoke_clique_folga_calendario.py:105`, `app/escala/tests/test_smoke_clique_folga_calendario.py:153`) |
| `antes` | A sonda lê os valores dos inputs `dias_<tam>`, converte para número e ordena, depois de preencher e antes de clicar em Repetir. Fevereiro o afirma; os dois testes novos de saída não o afirmam. (`app/escala/tests/test_smoke_clique_folga_calendario.py:129`, `app/escala/tests/test_smoke_clique_folga_calendario.py:143`, `app/escala/tests/test_smoke_clique_folga_calendario.py:333`, `app/escala/tests/test_smoke_clique_folga_calendario.py:349`) |
| `logo` | A sonda lê os mesmos inputs imediatamente após o clique em Repetir. Fevereiro exige `[9]`; os dois testes de saída não o afirmam. (`app/escala/tests/test_smoke_clique_folga_calendario.py:144`, `app/escala/tests/test_smoke_clique_folga_calendario.py:146`, `app/escala/tests/test_smoke_clique_folga_calendario.py:333`, `app/escala/tests/test_smoke_clique_folga_calendario.py:352`) |
| `depois`, `grade`, `conta`, `aberto` | Após responder e esperar outro timer, a sonda captura: inputs ordenados; células visíveis com fundo azul; texto do contador; e existência do fundo sem `hidden`. Os testes de saída afirmam os quatro; fevereiro afirma `depois` e `grade`, mas deixa `conta` e `aberto` sem asserção. (`app/escala/tests/test_smoke_clique_folga_calendario.py:129`, `app/escala/tests/test_smoke_clique_folga_calendario.py:133`, `app/escala/tests/test_smoke_clique_folga_calendario.py:135`, `app/escala/tests/test_smoke_clique_folga_calendario.py:156`, `app/escala/tests/test_smoke_clique_folga_calendario.py:335`, `app/escala/tests/test_smoke_clique_folga_calendario.py:352`) |
| `modal`, `texto`, `linhas`, `sim`, `nao` | `modal` começa `null`; com fundo aberto, a sonda registra `textContent`, linhas obtidas por `innerText` com verificação de visibilidade, e rótulos dos botões. Os testes novos exigem presença do modal; fevereiro exige também “Fevereiro” no texto. Eles não afirmam linhas nem rótulos. (`app/escala/tests/test_smoke_clique_folga_calendario.py:143`, `app/escala/tests/test_smoke_clique_folga_calendario.py:148`, `app/escala/tests/test_smoke_clique_folga_calendario.py:114`, `app/escala/tests/test_smoke_clique_folga_calendario.py:335`, `app/escala/tests/test_smoke_clique_folga_calendario.py:350`) |
| Resultado da sonda | A sonda serializa `r` no elemento `HX-CLIQUE`. Chromium entrega o DOM em `stdout`; `repete` extrai, desfaz escape HTML e desserializa o JSON. `_repete` exige `dict` antes de devolver aos testes. (`app/escala/tests/test_smoke_clique_folga_calendario.py:48`, `app/escala/tests/test_smoke_clique_folga_calendario.py:157`, `app/escala/tests/test_smoke_clique_folga_calendario.py:197`, `app/escala/tests/test_smoke_clique_folga_calendario.py:241`, `app/escala/tests/test_smoke_clique_folga_calendario.py:316`) |

A cadeia dos valores reais da página é esta:

- **Seed → seleção:** a view lê os tamanhos persistidos e fornece `marcados`, tamanho e rótulo; o template serializa esses dados e inicializa `sel[tamanho]` como `Set`. Os cliques da sonda chamam os handlers das células, que alteram essa seleção. (`app/escala/views.py:362`, `app/escala/views.py:369`, `app/templates/escala/folga_calendario_editar.html:74`, `app/templates/escala/folga_calendario_editar.html:78`, `app/templates/escala/folga_calendario_editar.html:89`, `app/templates/escala/folga_calendario_editar.html:119`)
- **Seleção → testemunhas:** `renderGrade` escreve células e contador; `syncHidden` escreve os inputs `dias_<tam>`. São esses elementos que a sonda lê, não uma cópia própria da regra de repetição. (`app/templates/escala/folga_calendario_editar.html:85`, `app/templates/escala/folga_calendario_editar.html:95`, `app/templates/escala/folga_calendario_editar.html:110`, `app/escala/tests/test_smoke_clique_folga_calendario.py:129`, `app/escala/tests/test_smoke_clique_folga_calendario.py:135`)
- **Repetir → diálogo → seleção:** `fcRepetir` copia imediatamente quando o alvo está vazio; caso contrário, fornece texto e verbo a `hxConfirmar` e só copia com resposta verdadeira. A cópia retém os dias da grade de 31 que cabem no alvo. (`app/templates/escala/folga_calendario_editar.html:124`)
- **Diálogo:** `hxConfirmar` escreve texto, verbo, visibilidade e promessa pendente; `fechar` oculta o fundo e resolve essa promessa. Sim resolve verdadeiro; Cancelar, fundo direto e Escape resolvem falso. O fundo só fecha quando `e.target === fundo`. (`app/templates/core/_dialogo.html:149`, `app/templates/core/_dialogo.html:157`, `app/templates/core/_dialogo.html:172`, `app/templates/core/_dialogo.html:188`)
- **Inclusão:** o editor estende `base.html`, que inclui o componente da casa. (`app/templates/escala/folga_calendario_editar.html:1`, `app/templates/base.html:388`)

As fixtures relevantes são:

| Fixture | Escritor → leitor |
|---|---|
| `admin` | `setUpTestData` cria usuário staff/superuser; `setUp` o usa em `force_login`, antes do GET protegido por `editar_escalas`. (`app/escala/tests/test_smoke_clique_folga_calendario.py:255`, `app/escala/tests/test_smoke_clique_folga_calendario.py:265`, `app/escala/views.py:314`) |
| `com_trinta` | A fixture grava grade 31 com `[3,10]` e grade 30 com `[8]`. Os dois testes de saída passam seu PK; a view lê esses dados para o seed. `dia(20)` acrescenta 20 à origem de 31, tornando a substituição indevida observável como `[3,10,20]`. (`app/escala/tests/test_smoke_clique_folga_calendario.py:259`, `app/escala/tests/test_smoke_clique_folga_calendario.py:334`, `app/escala/views.py:319`, `app/escala/views.py:362`, `app/templates/escala/folga_calendario_editar.html:127`) |
| Fevereiro | O teste usa criação sem calendário existente e constrói o estado pelos cliques: origem `[5,30]`, alvo 28 `[9]`; repetir produziria `[5]`. (`app/escala/tests/test_smoke_clique_folga_calendario.py:348`, `app/escala/views.py:319`, `app/templates/escala/folga_calendario_editar.html:127`) |
| `existente`, `sistema` | São preparados pelo mesmo `setUpTestData`, mas os três testes novos não os referenciam diretamente. Leitores indiretos de `sistema`: **[não verificado]**. (`app/escala/tests/test_smoke_clique_folga_calendario.py:254`, `app/escala/tests/test_smoke_clique_folga_calendario.py:256`, `app/escala/tests/test_smoke_clique_folga_calendario.py:333`) |

Os três testes novos param na observação do editor: não chamam `_reenvia` nem exercitam a gravação pela porta. Essa gravação permanece na view e em `salvar_calendario_de_folga`. (`app/escala/tests/test_smoke_clique_folga_calendario.py:333`, `app/escala/tests/test_smoke_clique_folga_calendario.py:275`, `app/escala/views.py:354`, `app/escala/folgas.py:571`)

**2) Mutantes por teste — previsão pela leitura, não execução** [nao verificado]

| Teste novo | Derruba | Passaria | [nao verificado]
|---|---|---|
| `test_MORDE_Escape_vale_Cancelar_no_repetir` | Escape resolvendo verdadeiro: substituiria `[8]` por `[3,10,20]`. Escape sem chegar ao listener: deixaria `aberto=true`. Guarda somente para 28: faltaria modal no alvo 30. (`app/escala/tests/test_smoke_clique_folga_calendario.py:334`, `app/escala/tests/test_smoke_clique_folga_calendario.py:335`, `app/escala/tests/test_smoke_clique_folga_calendario.py:340`, `app/templates/escala/folga_calendario_editar.html:127`) | Guarda somente para 30; fundo resolvendo verdadeiro; clique interno fechando. O teste responde exclusivamente com Escape no alvo 30. (`app/escala/tests/test_smoke_clique_folga_calendario.py:227`, `app/escala/tests/test_smoke_clique_folga_calendario.py:334`, `app/escala/tests/test_smoke_clique_folga_calendario.py:340`) |
| `test_MORDE_clique_fora_da_caixa_vale_Cancelar_no_repetir` | Fundo resolvendo verdadeiro: mesma substituição indevida. Fundo sem efeito: falha no fechamento. Guarda somente para 28: faltaria modal. (`app/escala/tests/test_smoke_clique_folga_calendario.py:228`, `app/escala/tests/test_smoke_clique_folga_calendario.py:334`, `app/escala/tests/test_smoke_clique_folga_calendario.py:335`, `app/escala/tests/test_smoke_clique_folga_calendario.py:343`) | Guarda somente para 30; Escape resolvendo verdadeiro; clique interno fechando. O estímulo é clique direto no fundo. (`app/escala/tests/test_smoke_clique_folga_calendario.py:228`, `app/escala/tests/test_smoke_clique_folga_calendario.py:334`, `app/escala/tests/test_smoke_clique_folga_calendario.py:343`) |
| `test_MORDE_repetir_sobre_fevereiro_preenchido_tambem_pergunta` | Guarda somente para 30; Cancelar resolvendo verdadeiro; cópia antes da resposta. Exige modal, `antes=[9]` e `logo/depois/grade=[9]`, enquanto a cópia seria `[5]`. (`app/escala/tests/test_smoke_clique_folga_calendario.py:348`, `app/templates/escala/folga_calendario_editar.html:127`) | Guarda somente para 28; Escape verdadeiro; fundo verdadeiro; clique interno fechando; Cancelar que preserve os dias mas deixe modal aberto ou contador errado. Não estimula Escape/fundo nem afirma `aberto/conta`. (`app/escala/tests/test_smoke_clique_folga_calendario.py:348`, `app/escala/tests/test_smoke_clique_folga_calendario.py:352`) |

**Em conjunto, os novos testes cobrem a guarda nos dois tamanhos**, mas Escape e fundo apenas no tamanho 30. (`app/escala/tests/test_smoke_clique_folga_calendario.py:334`, `app/escala/tests/test_smoke_clique_folga_calendario.py:348`)

O mutante que remove `if(e.target === fundo)` **sobrevive aos três**. Nenhum clica caixa ou texto; nos cliques dos botões, o handler já resolve e limpa `pendente`, fazendo a chamada posterior por borbulhamento retornar sem efeito. (`app/templates/core/_dialogo.html:149`, `app/templates/core/_dialogo.html:157`, `app/escala/tests/test_smoke_clique_folga_calendario.py:153`, `app/escala/tests/test_smoke_clique_folga_calendario.py:227`)

**Não há desencontro de alvo do Escape nesta cópia:** a sonda despacha em `document` e o componente escuta `document`. Se o evento fosse despachado num alvo não ouvido, o teste reprovaria por modal aberto. A simulação, porém, não percorre o caminho de um evento originado no botão focado; um listener que aceitasse apenas `target === document` passaria por ela. (`app/escala/tests/test_smoke_clique_folga_calendario.py:227`, `app/escala/tests/test_smoke_clique_folga_calendario.py:336`, `app/templates/core/_dialogo.html:160`, `app/templates/core/_dialogo.html:195`)

**3) Relógio, propagação e mordida**

- **Não usam relógio físico solto:** há dois timers fixos de 200 ms, mas Chromium recebe orçamento de tempo virtual de 4000 ms; os 120 segundos são o timeout externo do processo. Continua sendo uma janela fixa de observação: mudança posterior à captura pode escapar. (`app/escala/tests/test_smoke_clique_folga_calendario.py:147`, `app/escala/tests/test_smoke_clique_folga_calendario.py:155`, `app/escala/tests/test_smoke_clique_folga_calendario.py:199`)
- **Não há `subTest` nesses testes:** os métodos chamam diretamente o helper, cujas asserções propagam normalmente. (`app/escala/tests/test_smoke_clique_folga_calendario.py:333`)
- **As asserções principais mordem:** os estados preservados diferem dos estados que uma confirmação indevida produziria; ausência de execução da sonda também não aprova, pois `_repete` exige `dict`. As lacunas são específicas: fevereiro não afirma fechamento/contador; Escape e fundo não afirmam `antes/logo`. (`app/escala/tests/test_smoke_clique_folga_calendario.py:260`, `app/escala/tests/test_smoke_clique_folga_calendario.py:316`, `app/escala/tests/test_smoke_clique_folga_calendario.py:333`, `app/escala/tests/test_smoke_clique_folga_calendario.py:348`)
- `_dom` não afirma `returncode` nem erros de console; portanto resultado válido da sonda não equivale a processo sem erros. (`app/escala/tests/test_smoke_clique_folga_calendario.py:197`, `app/escala/tests/test_smoke_clique_folga_calendario.py:241`)

**4) Prosa do Repetir contra L7**

**O estado atual não cumpre L7:** a explicação “Repetir troca esses dias…” e o verbo “Substituir” estão escritos no template. Usar `hxConfirmar` atende ao componente, mas não à exigência de origem no mesmo mapa usado pelo juiz. (`app/templates/escala/folga_calendario_editar.html:131`, `app/docs/LEIS-UI.md:77`)

**O escopo permite corrigir essa confirmação local.** Quem decide se deve perguntar e quando executar a cópia já é `fcRepetir`, na UI. Uma fonte compatível seria um módulo canônico em `static/` com o mapa da ação — decisão de confirmar, mensagem e verbo — consumido pela própria decisão e pela apresentação. Isso é proposta de correção; implementação existente dessa fonte: **[não verificado]**. Apenas transportar a frase para outro arquivo, mantendo uma decisão independente, não satisfaz o compartilhamento exigido pela lei. (`app/templates/escala/folga_calendario_editar.html:124`, `app/docs/LEIS-UI.md:78`)

A view fornece rótulos e dados das grades, mas seu contexto não fornece a explicação do Repetir; `folgas.py` grava o calendário e sincroniza seus efeitos, enquanto a confirmação examinada antecede essa gravação e pertence ao rascunho da tela. Não é necessário mudar essas funções para centralizar a decisão local com sua mensagem. (`app/escala/views.py:318`, `app/escala/views.py:363`, `app/escala/folgas.py:571`, `app/templates/escala/folga_calendario_editar.html:124`)

**VEREDITO: DEVOLVE -- a prosa do Repetir continua hardcoded fora do mapa exigido pela L7; adicionalmente, o mutante de clique interno fechando permanece sem cobertura nos testes novos.** (`app/docs/LEIS-UI.md:78`, `app/templates/escala/folga_calendario_editar.html:131`, `app/templates/core/_dialogo.html:159`, `app/escala/tests/test_smoke_clique_folga_calendario.py:333`)

## 16/11 18:39 · sessao b66e40145c17 · BUG-PAR-O-PAR · verificador · origem=Code

**DEVOLVIDA por prova de integração incompleta.** Os três achados procedentes receberam correção no código e asserções específicas; a execução dos testes e mutantes **c4b com Django** permanece **[nao verificado]**. O harness exclui esses mutantes quando roda sem `--django`; portanto o resultado sem banco não completa essa entrega. (`app/chamados/tests/test_bug_par_volta.py:282`, `:296`, `:316`, `:364`, `:378`, `:391`; `app/chamados/tests/bug_par_mutantes.py:150`, `:189-192`)

1. **Quem escreve e quem lê**

   | Valor | Escritores/produtores | Leitores e significado |
   |---|---|---|
   | `_RECUSA_COMPETENCIA_NO_PAR` | Declarada com `None`; `_validar_o_par` instala a lista externa; `_materializar_resposta_hora` instala temporariamente a lista da irmã. O conflito só é acrescentado depois de classificado como `COMPETENCIA_APROVADA`. (`app/chamados/services/materializacao.py:1780`, `:1363`, `:1467-1471`; `app/chamados/services/validacao.py:545-546`) | O materializador consulta a lista restaurada para repassar os registros; o wrapper consulta a sua lista depois do rollback. O conteúdo é **`(pk, conflito original)` filtrado pela categoria**, não o texto composto de paridade. (`app/chamados/services/materializacao.py:1368-1370`, `:1777-1779`; `app/chamados/services/validacao.py:554-560`) |
   | Lista local `_comp_da_irma` | Nasce vazia por chamada; a materialização da irmã acrescenta o conflito pelo contexto instalado. (`app/chamados/services/materializacao.py:1361-1365`, `:1471`) | É repassada à lista superior e consultada antes do ramo de paridade; o primeiro registro produz o motivo da metade externa. (`app/chamados/services/materializacao.py:1370`, `:1379-1384`) |
   | `motivo_do_par_recusado_pela_competencia` | Produz um texto preservando o conflito original, sem acrescentar marcas de paridade. (`app/chamados/services/materializacao.py:1783-1789`) | Chamado pelo materializador compartilhado e por `_validar_o_par`; o resultado chega ao escritor canônico de recusa. (`app/chamados/services/materializacao.py:1383`, `:1960`; `app/chamados/services/validacao.py:557-559`) |
   | `recusa_motivo` | Permanecem os escritores de pendência, recusa de materialização, recusa humana, escape e duplicata. O c4b usa `_carimbar_recusa`. (`app/chamados/services/materializacao.py:1326`, `:1659`; `app/chamados/services/validacao.py:823`; `app/chamados/services/disputa_emissao.py:1511`; `app/chamados/services/fio.py:1405`; `app/chamados/models.py:3298`) | Devolução, fotografia da validação, explicador, modal, card, fotografia do comando, auditorias e regex de `flip_auto`. Esta última só interpreta motivos de perguntas com via terminal de paridade. (`app/chamados/services/validacao.py:181`, `:262`, `:278`; `app/chamados/services/explicador.py:113`; `app/templates/chamados/partials/modal_fio.html:226`; `app/templates/chamados/partials/_card_atendimento.html:192`; `app/chamados/management/commands/bug_par_103.py:69`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1075`, `:1112`; `app/ponto/services/flip_auto.py:128-132`) |
   | `materializada_em` e `via_resolucao` | O carimbo conjunto continua em `_escrever_carimbo`, chamado pelas portas de carimbo, identidade e descarimbo; há descarimbo em lote e escritores específicos de reclassificação da via. (`app/chamados/models.py:3207-3211`, `:3239`, `:3293-3295`, `:3318`, `:3393`, `:3402`; `app/chamados/services/relavra_via.py:125`) | Testemunha distingue conclusão com batida de recusa terminal; laço relê carimbo; passivo relê carimbo/via e classifica a recusa. (`app/chamados/services/materializacao.py:856-866`, `:2070-2078`; `app/chamados/services/passivo_validado.py:140-145`, `:534-545`) |
   | Nota do fio | `_carimbar_recusa` produz código/texto/ação pelo catálogo e chama `nota.anexar_ocorrencia`, com referência `pk:codigo`. A chave inclui chamado, tipo, referência e autor; colisão recupera a nota existente. (`app/chamados/services/materializacao.py:1648`, `:1664`; `app/chamados/services/nota.py:62-65`, `:95-109`) | Fio carrega os followups; modal e card apresentam seu conteúdo. (`app/chamados/services/fio.py:202`, `:206`; `app/templates/chamados/partials/modal_fio.html:488`; `app/templates/chamados/partials/_card_atendimento.html:43`) |
   | Retorno de `_competencia_que_barra` na espera tardia | Compõe o detalhe K6 e a resposta F6; nessa chamada, `perguntas=()` deixa K6 sem perguntas e consulta F6 pelo instante. (`app/chamados/services/materializacao.py:812-829`, `:1444`) | A espera tardia lê somente sua presença: havendo impedimento, pula a espera e deixa o chokepoint decidir a escrita. A mesma composição já alimenta a testemunha; a unidade também a usa na guarda somente de entrega. (`app/chamados/services/materializacao.py:903`, `:927`, `:1444-1449`, `:1824`) |

   **Leitores fora do diff:** carimbo/via também alimentam modelo e juízes, reconciliação, endpoint de apresentação, tarefa do dia, fábrica da célula, veredito, histórico, painel, fotografia, raio X, procedência da batida, silêncio da célula e cartório. (`app/chamados/models.py:3202-3205`; `app/chamados/juizes.py:683-696`; `app/chamados/reconciliador.py:367`, `:630`; `app/chamados/views.py:1257`; `app/chamados/services/tarefa_do_dia.py:256`; `app/chamados/services/fabrica_da_celula.py:149`, `:171`; `app/chamados/veredito_lavrado.py:203-204`; `app/chamados/services/historico.py:103`; `app/chamados/services/painel.py:67`, `:270`; `app/chamados/services/foto_e4b.py:180`; `app/chamados/services/raiox.py:135`, `:182`; `app/ponto/services/procedencia_batida.py:74`; `app/ponto/services/veredito_celula.py:196`; `app/ponto/services/cartorio.py:1650`)

   Os comandos também selecionam, fotografam ou auditam esses campos para backfill, reabertura, reclassificação e reconciliação; não são consumidores novos do registro c4b. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:101`, `:325`, `:398`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:67`, `:129`; `app/chamados/management/commands/relavrar_via_i6.py:58`, `:68`; `app/chamados/management/commands/encerrar_perguntas_com_fato.py:32-33`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:70`, `:147`; `app/chamados/management/commands/auditar_disputas.py:71`, `:150`; `app/ponto/management/commands/reconciliar_fantasmas.py:213`)

   **ESMERIL-SEMANTICA:** o ponto perigoso continua sendo classificar texto composto: o catálogo procura PARIDADE antes de competência. O texto novo evita essas marcas e conserva a mensagem F6; os leitores não exigem que ela esteja no começo. Não encontrei incompatibilidade nova nesse transporte. (`app/chamados/catalogo/recusa.py:40-54`, `:96-102`; `app/chamados/services/materializacao.py:1788-1789`)

   `materializada_em` continua significando conclusão, inclusive recusa terminal, e exige leitura conjunta da via para afirmar gravação. Testemunha e explicador fazem essa distinção; seleção administrativa e seleção do passivo têm contratos diferentes, expressamente separados. (`app/chamados/services/materializacao.py:856-866`, `:1807-1813`; `app/chamados/services/explicador.py:116-126`)

2. **Cura dos procedentes e entradas alternativas**

   | Achado da volta 1 | Código e cobrança atual |
   |---|---|
   | **VOLTA primeiro: L2 vencendo L3** | A espera tardia consulta F6 antes da divergência. O teste valida VOLTA primeiro e exige `nao_gravou`, devolução, motivo/nota de competência, ausência de carimbo terminal e irmã sem validação. Repor a espera antiga alcança as asserções desse contraexemplo. (`app/chamados/services/materializacao.py:1444-1447`; `app/chamados/tests/test_bug_par_volta.py:348-376`) |
   | **L2 no laço e passivo** | A coleta está no par compartilhado; havendo conflito F6 da irmã, retorna antes da paridade. Os testes exigem competência nas duas metades; o laço não cria atos novos, e o passivo contabiliza duas recusas de competência. (`app/chamados/services/materializacao.py:1361-1384`; `app/chamados/tests/test_bug_par_volta.py:378-403`) |
   | **Matriz L3/L4/L5** | L3 agora exige detalhe visível nos dois HTMLs; L4 exige VALIDACAO nas duas metades; L5 exige K6 não consultado e fotografia persistida inalterada. (`app/chamados/tests/test_bug_par_volta.py:282-283`, `:296-302`, `:316-321`) |

   **Cura demonstrada por inspeção e asserções existentes; execução Django desses testes: [nao verificado].** Os três mutantes c4b apontam precisamente para a ordem da espera e para o desvio que conserva o motivo no laço/passivo. (`app/chamados/tests/bug_par_mutantes.py:139-148`)

   Sobre as outras entradas:

   - **Duas metades barradas por F6:** a irmã recusada fornece o registro; a externa retorna antes da paridade. No laço/passivo, a segunda iteração recebe lista nova. Esse cenário está cobrado nos testes novos. (`app/chamados/services/materializacao.py:1361`, `:1379-1384`, `:1470-1471`; `app/chamados/tests/test_bug_par_volta.py:378-403`)
   - **Somente a externa barrada por F6:** se a irmã gravou, o conflito da externa levanta `_Reprovada`, desfazendo o ato com a irmã; o texto mantém F6. No wrapper, o registro permite refazer a recusa após seu rollback. Teste específico dessa combinação: **[nao verificado]**. (`app/chamados/services/materializacao.py:1467-1472`, `:1490-1492`, `:1946-1960`; `app/chamados/services/validacao.py:554-560`)
   - **Retry de recusa F6:** o motivo anterior não impede por si só a formação do par; as guardas relevantes são pendência, validação, data e geometria. A recusa idêntica não gera outra nota pelo mesmo gesto. Teste específico do retry F6: **[nao verificado]**. (`app/chamados/services/materializacao.py:197-235`, `:1649-1664`; `app/chamados/services/nota.py:100-109`)
   - **Irmã já carimbada ou já presente no espelho:** fica fora da composição do par. A externa pode continuar recusada por PARIDADE se a vizinhança continuar inválida; nesse ramo a testemunha também anuncia paridade, antes de alcançar F6. Isso não demonstra perda de uma recusa efetivamente produzida pelo chokepoint. (`app/chamados/services/materializacao.py:148`, `:229-230`, `:901-925`, `:1385-1436`)
   - **Irmã passa a reutilizar batida durante o ato:** reuso antecede paridade/espera; somente batidas criadas no toque entram no conjunto a desfazer. Teste específico combinado com F6: **[nao verificado]**. (`app/chamados/services/materializacao.py:1335-1347`, `:1458-1461`, `:1683-1685`)
   - **Pergunta já terminal por paridade no passivo:** a seleção normal não a replanta: o juiz aceita essa identidade como materializada, o vigia classifica `com_registro` e o passivo pula essa classe. Não encontrei nesse caminho carimbo terminal de paridade recebendo causa F6. (`app/chamados/models.py:2615`, `:3902`; `app/chamados/juizes.py:683-696`; `app/chamados/services/vigia_carimbo.py:105-108`; `app/chamados/services/passivo_validado.py:327-331`)

3. **Espera tardia, comportamento fora do par e contadores**

   **Sim, muda uma U163 sozinha:** sendo divergente, sem reuso nem bloqueio de paridade e com F6 ativo, a espera tardia agora deixa chegar ao chokepoint e escrever a recusa. A testemunha já dizia L2 para essa mesma pergunta, e a espera inicial já deixava passar esse estado. A mudança alinha a escrita tardia à precedência da testemunha. (`app/chamados/services/materializacao.py:772-773`, `:927-932`, `:1444-1449`, `:1871-1875`, `:1959-1964`)

   **Pergunta não intervalo:** a espera já retorna `None`, pois filtra motivo de intervalo e pertencimento U163; F6 continua aplicado pelo chokepoint. **Competência apenas fechada:** o predicado real só barra `aprovado` ou `exportado`; outro status não ganha essa dispensa da espera. (`app/chamados/services/materializacao.py:741-744`, `:1444`; `app/ponto/registro_batida.py:157-159`; `app/ponto/janelas.py:87-89`)

   **A ordem externa de `validar_pergunta` permanece:** espera compartilhada → implausibilidade → trava → K6 → VALIDACAO → materialização → releitura K6 → devolução. A mudança está dentro da criação tardia. (`app/chamados/services/validacao.py:605`, `:611`, `:643`, `:648`, `:651`, `:677`, `:685`, `:689`; `app/chamados/services/materializacao.py:1444`)

   **Não entra pela primeira vez no contador das 163.** A seleção atual é por PK congelada, sem filtro por motivo; uma pergunta não materializada com L2 já integra `outras`. Escrever F6 não altera sua participação nem reduz `pendentes_aval`. Os comentários que ainda dizem que a recusa escrita determina pertencimento estão desatualizados em relação à seleção efetiva. (`app/chamados/services/materializacao.py:321`, `:339-356`, `:1672-1673`, `:1868-1870`; `app/chamados/services/universo_bug_par.py:33-35`)

   **Há efeitos em outras filas/contagens:** na porta administrativa, motivo antes vazio impedia a devolução; agora F6 permite limpar `validada_em` e registrar DEVOLUCAO. A pergunta pode voltar a aguardar validação e sair das seleções de validadas do vigia/passivo; escrever motivo também a retira do I18 de recusa ausente. Isso muda o estado participante, sem mudar a definição dessas consultas. (`app/chamados/services/validacao.py:262-278`; `app/chamados/services/explicador.py:100-103`; `app/chamados/services/vigia_carimbo.py:97-98`; `app/chamados/services/painel.py:270`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1074-1076`)

4. **Registro aninhado**

   **Sem duplicação ou vazamento identificado no caminho normal:** cada chamada cria uma lista; a irmã acrescenta seu conflito original uma vez; a externa repassa a lista uma vez e, quando ela explica a falha do par, retorna antes de produzir conflito próprio. O wrapper lê o registro, sem acrescentá-lo novamente. (`app/chamados/services/materializacao.py:1361-1384`, `:1471`; `app/chamados/services/validacao.py:556-560`)

   **O token novo é restaurado inclusive por exceção:** a chamada da irmã está imediatamente protegida por `try/finally`; o wrapper também restaura seu token em `finally`. Se a exceção interromper o repasse, pode impedir esse repasse, mas não deixa o contexto instalado para a próxima pergunta. O laço chama novamente a unidade por pergunta. (`app/chamados/services/materializacao.py:1363-1370`, `:2078`; `app/chamados/services/validacao.py:546-563`)

5. **Classes de reprovação**

   | Classe | Resultado |
   |---|---|
   | Segundo juiz | Não identificado: a leitura nova usa a composição existente, que consulta o mesmo F6 do chokepoint. (`app/chamados/services/materializacao.py:824-828`, `:1444`; `app/ponto/registro_batida.py:157-159`) |
   | Segundo escritor fora da porta | Não identificado: o motivo composto segue para `_carimbar_recusa`; o carimbo permanece nas portas do modelo. (`app/chamados/services/materializacao.py:1383`, `:1960`; `app/chamados/services/validacao.py:558`; `app/chamados/models.py:3207-3211`) |
   | Relógio fora da borda | O trecho novo não acrescenta leitura de relógio; utiliza `ts` já derivado. As leituras existentes de carimbo permanecem nos pontos anteriores. (`app/chamados/services/materializacao.py:1361-1384`, `:1444-1455`; `app/chamados/services/validacao.py:557-558`) |
   | Derivação paralela | Não: é leitura da mesma composição da testemunha; ela apenas escolhe consultar a espera, e o chokepoint continua decidindo a escrita. (`app/chamados/services/materializacao.py:903`, `:927`, `:1444-1449`) |
   | Ordem de trava | Preservada nos caminhos examinados: porta trava antes do carimbo; laço trava e relê antes da unidade; a trava compartilhada inclui a irmã. (`app/chamados/services/validacao.py:643-651`; `app/chamados/services/materializacao.py:2005-2022`, `:2068-2078`) |
   | Campo com dois significados | O registro novo tem um contrato; carimbo/via conservam a distinção entre encerramento e gravação. Permanecem comentários antigos incorretos sobre pertencimento por recusa, sem correspondência no contador atual. (`app/chamados/services/materializacao.py:1777-1789`, `:856-866`, `:321`, `:1868-1870`) |
   | Selo/RED sem mordida | Há asserções adequadas e harness que exige controle verde e falha por asserção, sem erros/skips. A morte **executada** dos c4b com Django continua **[nao verificado]**. (`app/chamados/tests/bug_par_mutantes.py:196-200`, `:218-228`) |

   Na execução sem escrita desta verificação, passaram os **7 testes sem banco** e morreram por asserção os **17 mutantes selecionados sem Django**, com erros zero; esse modo exclui os c4b de integração. (`app/chamados/tests/test_bug_par_sem_banco.py:20`; `app/chamados/tests/bug_par_mutantes.py:189-192`, `:224-225`)

6. **Escopo e bloqueio final**

   Os pontos de produção do c4b correspondem ao transporte L2 e à precedência reclamada na volta 1; os demais toques fornecidos reforçam testes/mutantes. A definição do contador e o pertencimento por PK continuam nos mesmos mecanismos. (`app/chamados/services/materializacao.py:1361-1384`, `:1444`, `:1783-1789`, `:321-356`; `app/chamados/services/validacao.py:557`; `app/chamados/tests/test_bug_par_volta.py:282-403`; `app/chamados/services/universo_bug_par.py:33-35`)

   Não há toque em motor, JSON, contador ou comando/apply no **diff fornecido**; correspondência histórica independente entre esse diff e os commits declarados: **[nao verificado]**, pois a cópia não oferece repositório Git. **[nao verificado]** [nao verificado]

   Não muda o predicado nem escreve competência exportada. Há, contudo, efeito sobre perguntas U163 em status `exportado`: a nova precedência também pode trocar espera tardia por recusa F6 nesse status. O salto das 163 na guarda K6 permanece. (`app/ponto/janelas.py:87-89`; `app/chamados/services/materializacao.py:800`, `:1444`)

   **Bloqueia a aprovação a prova executada de integração dos c4b e dos reforços da matriz, exigida pelo COMPLEMENTO-2, item 3d.** A triagem afirma uma execução anterior, mas o resumo citado não integra o material localizado; resultado, controles verdes e mortes por asserção com Django permanecem **[nao verificado]**. Não identifiquei novo bloqueio funcional demonstrado no código examinado; a entrega ainda não tem essa prova verificável. (`app/chamados/tests/bug_par_mutantes.py:139-150`, `:196-228`; execução Django **[nao verificado]**)

## 16/11 18:39 · sessao 9fed026fbfc8 · - · - · origem=Ronald (tmux)

Análise estática; execução e mutantes efetivamente rodados: [não verificado].

| Teste novo | Mutante que derruba por leitura do código | Mutante que passa | [nao verificado]
|---|---|---|
| `test_MORDE_Escape_vale_Cancelar_no_repetir` | Escape `fechar(true)`: alvo 30 `[8]` viraria `[3,10,20]`; falha no trio depois/grade/conta. Escape sem efeito: falha em `aberto`. Guarda somente para 28: falha em `modal`. | Guarda somente para 30; fundo resolvendo verdadeiro; clique interno fechando. Referências: `app/escala/tests/test_smoke_clique_folga_calendario.py:334`, `:335`, `:336`, `:337`, `:340`; `app/templates/escala/folga_calendario_editar.html:127`, `:133`. |
| `test_MORDE_clique_fora_da_caixa_vale_Cancelar_no_repetir` | Fundo `fechar(true)`: mesma troca indevida e falha no trio. Fundo sem efeito: falha em `aberto`. Guarda somente para 28: falha em `modal`. | Guarda somente para 30; Escape resolvendo verdadeiro; clique interno fechando. Referências: `app/escala/tests/test_smoke_clique_folga_calendario.py:334`, `:335`, `:336`, `:337`, `:343`; `app/templates/core/_dialogo.html:159`. |
| `test_MORDE_repetir_sobre_fevereiro_preenchido_tambem_pergunta` | Guarda somente para 30: fevereiro `[9]` vira `[5]` sem modal; falham presença e preservação. Cancelar verdadeiro: depois/grade viram `[5]`, falhando. Cópia antecipada: falha em `logo`. | Guarda somente para 28; Escape verdadeiro; fundo verdadeiro; clique interno fechando; Cancelar que preserva dias mas deixa modal aberto ou contador errado. Referências: `app/escala/tests/test_smoke_clique_folga_calendario.py:348`, `:349`, `:350`, `:352`; captura dos campos em `:156`. |

Os novos testes, em conjunto, barram guarda exclusiva de um dos tamanhos; fevereiro cobre 28 e os testes das saídas cobrem 30. Os testes antigos já cobriam Cancelar e Sim em 30. (`app/escala/tests/test_smoke_clique_folga_calendario.py:320`, `:334`, `:348`, `:355`.)

O mutante “qualquer clique no fundo borbulhado fecha”, removendo `if(e.target === fundo)`, passa: nenhum novo teste clica caixa/texto; botões resolvem primeiro, zeram `pendente`, e a segunda chamada a `fechar` retorna. (`app/templates/core/_dialogo.html:149`, `:151`, `:157`, `:158`, `:159`; `app/escala/tests/test_smoke_clique_folga_calendario.py:227`, `:228`, `:153`.)

Escape é despachado em `document`, exatamente onde está o listener; não há erro de alvo na cópia. Se a sonda disparasse em alvo não ouvido, o modal permaneceria aberto e `assertFalse(res['aberto'])` reprovaria. Mover o listener para `body` também reprovaria o Escape sintético atual, embora o evento real vindo do botão focado tenha outro caminho: isso limita a equivalência do teste com teclado humano. (`app/escala/tests/test_smoke_clique_folga_calendario.py:227`, `:336`; `app/templates/core/_dialogo.html:160`, `:161`, `:195`.)

Não há `subTest` nos três métodos: chamam o helper diretamente e as asserções propagam. Há timers fixos de 200 ms, mas Chromium recebe orçamento de tempo virtual de 4000 ms; o timeout externo é 120 s. Assim, não são sleeps de relógio físico solto, mas uma janela de observação fixa: alteração posterior à captura pode escapar. (`app/escala/tests/test_smoke_clique_folga_calendario.py:147`, `:155`, `:158`, `:159`, `:199`, `:201`, `:340`, `:343`, `:346`.)

As asserções principais têm mordida: fixture alvo `[8]` difere da origem `[3,10,20]`, e fevereiro alvo `[9]` difere da origem filtrada `[5]`; além disso `_repete` exige resultado `dict`, evitando aprovação quando a sonda não rodou. O teste de fevereiro deixa sem asserção `aberto` e `conta`; os dois testes de saída não afirmam `antes` nem `logo`. (`app/escala/tests/test_smoke_clique_folga_calendario.py:260`, `:261`, `:316`, `:334`, `:337`, `:348`, `:352`; `app/templates/escala/folga_calendario_editar.html:127`.)

## 16/11 18:39 · sessao 7b33704abd66 · - · - · origem=Ronald (tmux)

- **A implementação atual não cumpre a origem exigida pela L7:** `hxConfirmar(...)` põe a mensagem no componente da casa, mas a explicação e o verbo continuam literais em `app/templates/escala/folga_calendario_editar.html:131-132`; a L7 exige componente **e** texto vindo do mesmo mapa que o juiz usa, proibindo hardcode por tela (`app/docs/LEIS-UI.md:77-80`).
- **O escopo permite corrigir essa confirmação local.** O código que decide perguntar e executar o repetir já está na UI: `fcRepetir` copia filtrando `d <= t.n`, dispensa confirmação se o alvo está vazio e só copia após resposta verdadeira (`app/templates/escala/folga_calendario_editar.html:124-134`). Uma solução dentro de `static/` seria um módulo canônico da ação repetir, contendo o mapa de estados/política e a mensagem, usado tanto pela decisão como pela apresentação; essa fonte ainda não existe na implementação examinada [não verificado].
- **Mover somente a frase para outro arquivo sem fazer o juiz consumir o mesmo mapa não demonstra cumprimento da L7**, pois a exigência é explicitamente compartilhar o mapa com quem decide (`app/docs/LEIS-UI.md:78-80`).
- **Não há uma fonte pronta de prosa do repetir na view examinada:** o contexto entrega `cal`, `tamanhos`, `tipos`, `modo` e `celulas`; os tamanhos carregam tamanho, rótulo, intervalo e marcados (`app/escala/views.py:363-371`). O rótulo da grade vem de `TAMS` (`app/escala/views.py:318`), chega ao seed (`app/templates/escala/folga_calendario_editar.html:74`) e é consumido na frase (`app/templates/escala/folga_calendario_editar.html:131`).
- **`folgas.py` é fonte de regras distintas, sem fornecer essa confirmação:** `criar_calendario_de_folga` aplica clamp aos três tamanhos (`app/escala/folgas.py:645-661`); `salvar_calendario_de_folga` grava os dias por tamanho e sincroniza as fotos futuras (`app/escala/folgas.py:571-601`); `datas_que_o_calendario_manda` decide as datas reais (`app/escala/folgas.py:43-56`). A confirmação do rascunho atual é decidida por `fcRepetir`, antes de qualquer gravação (`app/templates/escala/folga_calendario_editar.html:124-134`).

Conclusão para consolidação: **DEVOLVE por prosa hardcoded ainda presente; o escopo não impede a correção**, porque a decisão de confirmar já pertence à UI e pode compartilhar uma fonte em `static/` com sua mensagem (`app/docs/LEIS-UI.md:77-80`; `app/templates/escala/folga_calendario_editar.html:124-134`).

## 16/11 18:37 · sessao 60b99af043b4 · - · - · origem=Ronald (tmux)

- `recusa_motivo`: escritores existentes em `materializacao.py:1326` e `:1659`, `validacao.py:823`, `disputa_emissao.py:1511` e `fio.py:1405` pela porta `models.py:3298`; o c4b continua usando `_carimbar_recusa` (`app/chamados/services/materializacao.py:1383`, `:1960`).
- `materializada_em`/`via_resolucao`: atribuição conjunta em `models.py:3207-3211`, chamada pelas portas `:3239`, `:3293`, `:3295`, `:3318`; descarimbo em lote em `:3392-3393`; re-lavra exclusiva da via em `app/chamados/services/relavra_via.py:125-127`.
- Passivo: `_balde_da_recusa` prioriza via **mudada**, depois classifica o último erro; não exige que o texto comece pela causa (`app/chamados/services/passivo_validado.py:140-145`). `_plantar_o_dia` relê carimbo/via e pula apenas a irmã carimbada durante aquele ato (`:534-537`).
- Nota: `_carimbar_recusa` emite ocorrência com referência `pk:codigo`; a chave inclui chamado/tipo/referência/autor e a porta recupera a ocorrência existente em colisão de chave (`app/chamados/services/materializacao.py:1664`; `app/chamados/services/nota.py:62-65`, `:95-109`). Fio carrega followups e templates apresentam o conteúdo (`app/chamados/services/fio.py:202`, `:206`; `app/templates/chamados/partials/modal_fio.html:488`; `app/templates/chamados/partials/_card_atendimento.html:43`).
- Explicador lê estado, pendência, lastro e testemunha; motivo de recusa é usado diretamente na pendência, enquanto recusa terminal consulta a testemunha (`app/chamados/services/explicador.py:95-126`). Modal exibe `recusa_motivo`; card prioriza via nas perguntas resolvidas (`app/templates/chamados/partials/modal_fio.html:226`; `app/templates/chamados/partials/_card_atendimento.html:192`).
- `flip_auto` só interpreta a regex do motivo após filtrar `VIA_PARIDADE_RECUSADA`; uma recusa F6 sem essa via não entra nessa fila (`app/ponto/services/flip_auto.py:128-140`).
- O catálogo aceita marcas em qualquer posição, mas PARIDADE vem antes de COMPETENCIA_APROVADA; o composto c4b evita as marcas de paridade e preserva o conflito do chokepoint (`app/chamados/catalogo/recusa.py:40-54`, `:96-102`; `app/chamados/services/materializacao.py:1783-1789`).

A hipótese “passivo escreve F6 sobre uma pergunta já terminal por paridade” não é alcançada pela seleção normal: paridade está nas vias de identidade e sem testemunha, o juiz a aceita como materializada, o vigia a classifica `com_registro` e o passivo pula essa classe (`app/chamados/models.py:2615`, `:3902`; `app/chamados/juizes.py:683-696`; `app/chamados/services/vigia_carimbo.py:105-108`; `app/chamados/services/passivo_validado.py:327-331`).

Irmã previamente carimbada fica fora da composição do par; se não existir batida real que corrija a vizinhança, a outra metade pode continuar em PARIDADE, e a testemunha também anuncia paridade nesse ramo. Isso não demonstra causa F6 engolida pelo c4b (`app/chamados/services/materializacao.py:148`, `:901-925`).

Leitores adicionais: histórico, painel, fotografia, raio X, procedência da batida, silêncio da célula, cartório, reconciliação de fantasmas e comandos de auditoria/fotografia (`app/chamados/services/historico.py:103`; `app/chamados/services/painel.py:67`; `app/chamados/services/foto_e4b.py:180`; `app/chamados/services/raiox.py:135`, `:182`; `app/ponto/services/procedencia_batida.py:74`; `app/ponto/services/veredito_celula.py:196`; `app/ponto/services/cartorio.py:1650`; `app/ponto/management/commands/reconciliar_fantasmas.py:213`; `app/chamados/management/commands/auditar_invariantes_chamados.py:536`, `:542`, `:955`, `:1075`, `:1112`; `app/chamados/management/commands/bug_par_103.py:66-69`).

## 16/11 18:36 · sessao 0ce1c8212d89 · - · - · origem=Ronald (tmux)

- **A espera tardia muda comportamento fora do par.** Uma metade U163 sozinha, divergente, sem batida reaproveitável nem vizinha que imponha paridade, com F6 ativo, já recebia L2 da testemunha: F6 antecede a espera (`app/chamados/services/materializacao.py:927`, `:930`). A espera inicial da escrita também consulta esse derivador e só bloqueia o estado de espera (`app/chamados/services/materializacao.py:772`, `:773`, `:1871`). O comportamento antigo da espera tardia fornecido no diff interrompia antes do chokepoint; agora a consulta F6 evita a espera, alcança `registrar_batida` e escreve a recusa F6 (`app/chamados/services/materializacao.py:1444`, `:1449`, `:1467`, `:1471`, `:1959`). Portanto passou de espera sem recusa escrita para recusa escrita, alinhando a escrita à testemunha (`app/chamados/services/materializacao.py:927`, `:929`, `:1944`, `:1962`).

- **Há efeitos nas filas gerais, embora o contador 163 conserve universo e definição.** Se essa pergunta tinha `recusa_motivo=''`, a devolução antiga não retirava sua validação; agora o motivo permite retirar `validada_em` e registrar DEVOLUCAO (`app/chamados/services/validacao.py:262`, `:265`, `:269`, `:275`). Ela pode voltar ao estado `aguarda_validacao` do explicador e à vida pelo C6, sujeitos às demais guardas (`app/chamados/services/explicador.py:100`; `app/chamados/juizes.py:1405`, `:1409`, `:1423`, `:1435`). Pela retirada da validação, sai do universo de perguntas validadas do vigia/passivo e das contagens do painel baseadas nesse carimbo (`app/chamados/services/vigia_carimbo.py:97`; `app/chamados/services/painel.py:261`, `:270`). Mesmo sem devolução, escrever o motivo retira uma pergunta antes sem motivo do I18 (`app/chamados/management/commands/auditar_invariantes_chamados.py:1074`). Esses são efeitos de estado, sem alteração das definições dessas consultas (`app/chamados/services/validacao.py:262`, `:269`).

- **O universo 163 não depende de recusa escrita.** A seleção usa exclusivamente PKs U163; perguntas não materializadas já entram mesmo com motivo vazio, e uma L2 já integra `outras` (`app/chamados/services/materializacao.py:321`, `:325`, `:339`, `:347`). `pendentes_aval` continua somando aplicáveis, legítimas, outras e esperas, mais as 103 devolvíveis (`app/chamados/services/materializacao.py:351`, `:356`). A nova recusa F6 não introduz a pergunta nesse universo, não a resolve e não a faz sair: continua não materializada/L2 (`app/chamados/services/materializacao.py:326`, `:347`, `:356`). Pertencimento consulta somente PK; recusa e devolução não alteram a fonte (`app/chamados/services/universo_bug_par.py:33`).

- **Pergunta não intervalo:** o guardião da espera filtra motivo de intervalo e pertencimento U163; por isso a nova leitura F6 não muda a decisão ordinária da espera desse caso — com ou sem F6 ela já retornava `None`, e o chokepoint já aplicava F6 (`app/chamados/services/materializacao.py:741`, `:743`, `:1444`; `app/ponto/registro_batida.py:159`). **Competência apenas fechada:** F6 real só responde impedimento para status `aprovado` ou `exportado`; outro status conserva a espera (`app/ponto/janelas.py:75`, `:87`, `:89`; `app/chamados/services/materializacao.py:1444`). “Fechada/aprovada” no comentário é mais amplo que o predicado efetivo; não aparece outro predicado no delta (`app/chamados/services/materializacao.py:1441`; `app/ponto/janelas.py:87`).

- **Não há derivação paralela nova:** a espera tardia usa `_competencia_que_barra`, a mesma composição lida pela testemunha; sem `perguntas`, sua componente K6 percorre conjunto vazio e o resultado vem de F6 (`app/chamados/services/materializacao.py:812`, `:819`, `:824`, `:927`, `:1444`). F6 é o mesmo helper do chokepoint (`app/chamados/services/materializacao.py:824`; `app/ponto/registro_batida.py:158`). A função não recusa nem carimba nessa chamada: só determina se deve consultar a espera (`app/chamados/services/materializacao.py:1444`, `:1449`).

- **Ordem da porta preservada no código atual:** espera compartilhada, implausibilidade, trava, K6, VALIDACAO, tentativa de materialização, releitura K6 e devolução (`app/chamados/services/validacao.py:605`, `:611`, `:643`, `:648`, `:651`, `:677`, `:685`, `:689`). O delta fornecido não muda esses pontos; comparação histórica independente com `d1878c99b` é **[nao verificado]** porque a cópia não oferece repositório Git.

- **Ordem de trava:** a porta trava colaborador/disputas/chamados antes do carimbo (`app/chamados/services/validacao.py:643`, `:651`); o laço trava e relê antes da unidade (`app/chamados/services/materializacao.py:2068`, `:2069`, `:2070`, `:2078`). A trava compartilhada reúne pergunta e irmã e chama `PerguntaDisputa.trava_na_ordem` (`app/chamados/services/materializacao.py:2005`, `:2013`, `:2022`). Nenhuma operação de trava foi acrescentada pelo delta apresentado **[nao verificado]** quanto à comparação histórica independente.

- **Exportada:** a nova leitura F6 também pode responder `exportado`, pois esse status está no helper antigo (`app/ponto/janelas.py:87`). O K6 continua excluindo U163 (`app/chamados/services/materializacao.py:800`). O passivo mantém sua autoridade distinta para exportação vigente e a porta de regeneração em exportada (`app/chamados/services/passivo_validado.py:116`, `:124`, `:451`, `:453`). Assim é correto dizer “sem mudança do predicado ou escritor de competência exportada”; não dizer “nenhum efeito em pergunta de competência exportada”, pois a espera tardia agora pode passar à recusa F6 também com `exportado` (`app/ponto/janelas.py:87`; `app/chamados/services/materializacao.py:1444`).

- **Escopo:** no diff fornecido, produção muda somente `materializacao.py` e a extração de texto em `_validar_o_par`; motor, JSON, contador e comando/apply não constam das alterações apresentadas **[nao verificado]** quanto ao diff histórico independente. No código, a fonte continua validando hash/cardinalidade e a consulta continua por PK (`app/chamados/services/universo_bug_par.py:8`, `:15`, `:20`, `:35`). A leitura F6 adicional aplica a precedência já exigida pela testemunha às metades sozinhas também; efeitos concretos de filas devem ser declarados, e não descritos como ausência total de mudança fora do par (`app/chamados/services/materializacao.py:927`, `:1444`; `app/chamados/services/validacao.py:262`, `:269`).

## 16/11 18:35 · sessao c68ae9d2a77b · - · - · origem=Ronald (tmux)

- **Os reforços L3/L4/L5 estão presentes:** L3 exige detalhe e visibilidade nos dois HTMLs; L4 exige ato `VALIDACAO` em ambas; L5 exige que K6 não seja chamado, além de ausência de persistência (`app/chamados/tests/test_bug_par_volta.py:282`, `:283`, `:296`, `:302`, `:316`, `:319`, `:321`).

- **Os três REDs c4b cobram os achados procedentes:** VOLTA primeiro exige `nao_gravou`, causa competência e irmã sem validação; laço exige causa competência nas duas metades e nenhum ato novo; passivo exige duas recusas no balde `COMPETENCIA_APROVADA` (`app/chamados/tests/test_bug_par_volta.py:364`, `:373`, `:376`, `:378`, `:388`, `:389`, `:391`, `:402`, `:403`). O helper também cobra nota de competência, ausência de carimbo e ausência da via terminal de paridade (`app/chamados/tests/test_bug_par_volta.py:355`, `:356`, `:357`, `:362`).

- **O harness tem mordida por asserção no contrato:** exige controle verde, sem skip e com teste executado; troca o código da função real, restaura em `finally` e só aceita `failures` sem `errors/skips` (`app/chamados/tests/bug_par_mutantes.py:196`, `:197`, `:210`, `:218`, `:221`, `:224`, `:228`). Os três c4b são marcados como integração e excluídos sem `--django` (`app/chamados/tests/bug_par_mutantes.py:139`, `:143`, `:146`, `:150`, `:189`, `:192`). **Execução/morte dos c4b com Django: [nao verificado].** Não encontrei nesta cópia o `logs/BUG-PAR-O-PAR/volta/c4/integ/RESUMO.txt` alegado pela triagem; conteúdo do registro: **[nao verificado]**.

- **Executei sem escrita:** sete testes sem banco passaram; harness sem Django terminou RC0, com os 17 mutantes selecionados mortos por asserção e erros zero. As funções são extraídas por AST e as mutações vivem em memória (`app/chamados/tests/test_bug_par_sem_banco.py:20`; `app/chamados/tests/e4_apoio.py:8`; `app/chamados/tests/e4_catracas.py:5`; `app/chamados/tests/bug_par_mutantes.py:224`). Isso não executa os c4b de integração (`app/chamados/tests/bug_par_mutantes.py:192`).

- **Limites da cobertura:** os testes novos usam competência **aprovada**, com ambas previamente validadas no laço/passivo (`app/chamados/tests/test_bug_par_volta.py:220`, `:384`, `:385`, `:397`, `:398`). Não demonstram irmã já materializada, reuso de batida, retry de recusa F6, F6 só na metade externa ou competência somente fechada **[nao verificado]**. As duas ordens e retries existentes exercitam sucesso (`app/chamados/tests/test_bug_par_volta.py:44`, `:60`, `:64`).

- **F4 permanece sob mock K6 fixo `False`:** o teste compara antes/depois e cobra pendentes 1→0, mas herda o patch da fixture (`app/chamados/tests/test_bug_par_volta.py:461`, `:463`, `:468`, `:471`; `app/chamados/tests/test_bug_par_projetado.py:51`). A invariância com autoridade real continua **[nao verificado]**.

## 16/11 18:25 · sessao 2eb499a6ce30 · BUG-PAR-O-PAR · verificador · origem=Code

**DEVOLVIDA — bloqueia.** A cura L2 não alcança o par executado pelo laço/passivo; há também um caminho em que a precedência depende da ordem das metades. A execução exigida dos mutantes `c4_*` com `--django` permanece **[nao verificado]** (`app/chamados/services/materializacao.py:2048`; `app/chamados/services/passivo_validado.py:537`; `app/chamados/services/materializacao.py:1424`; `app/chamados/tests/bug_par_mutantes.py:139`).

1. **Escritores e leitores — sem achado de incompatibilidade entre tupla e texto.**

   | Valor | Quem produz/escreve | Quem lê |
   |---|---|---|
   | `bloqueada_sem_dia_do_fato` / `bloqueada_competencia_entregue` | Constantes e guarda K6; a guarda conserva a exclusão das 163 (`app/chamados/services/materializacao.py:705`, `:706`, `:800`, `:804`, `:806`). | Testemunha e retorno da porta; ambos separam estado e mensagem (`app/chamados/services/materializacao.py:882`; `app/chamados/services/validacao.py:228`, `:230`). |
   | `entrega_que_barra_a_validacao` | Compõe pergunta + irmã e devolve `None | (estado, texto)` (`app/chamados/services/validacao.py:215`, `:221`). | Testemunha e duas guardas da validação; nenhum desses leitores continua esperando `str` (`app/chamados/services/materializacao.py:880`, `:882`; `app/chamados/services/validacao.py:648`, `:650`, `:685`, `:688`). |
   | `_competencia_que_barra` | **Continua textual**: extrai `_k6[1]`, compõe F6 ou retorna `''` (`app/chamados/services/materializacao.py:812`, `:821`, `:828`, `:829`). | Detalhes da testemunha e erro da unidade; esses usos de texto continuam corretos (`app/chamados/services/materializacao.py:903`, `:905`, `:927`, `:929`, `:1794`, `:1796`). |
   | `_RECUSA_COMPETENCIA_NO_PAR` | Declarada com `None`; instalada pelo wrapper do admin; recebe conflitos classificados pelo catálogo como `COMPETENCIA_APROVADA` (`app/chamados/services/materializacao.py:1759`, `:1446`, `:1449`, `:1450`; `app/chamados/services/validacao.py:545`). | Wrapper após rollback, com restauração pelo token (`app/chamados/services/validacao.py:555`, `:563`). |
   | `_PAR_RECUSADO_PELA_COMPETENCIA` | Criada e devolvida exclusivamente pelo wrapper (`app/chamados/services/validacao.py:519`, `:560`). | Comparação por identidade que impede a tentativa isolada (`app/chamados/services/validacao.py:680`, `:682`). |
   | `recusa_motivo` | O delta reutiliza `_carimbar_recusa`; permanecem os escritores anteriores de pendência, recusa humana, escape e duplicata (`app/chamados/services/validacao.py:558`; `app/chamados/services/materializacao.py:1326`, `:1638`; `app/chamados/services/validacao.py:823`; `app/chamados/services/disputa_emissao.py:1511`; `app/chamados/services/fio.py:1405`; `app/chamados/models.py:3298`). | Devolução, explicador, templates e fotografia do comando (`app/chamados/services/validacao.py:262`, `:278`; `app/chamados/services/explicador.py:113`; `app/templates/chamados/partials/modal_fio.html:226`; `app/templates/chamados/partials/_card_atendimento.html:192`; `app/chamados/management/commands/bug_par_103.py:69`). |
   | `validada_em` | Assinatura e devolução canônicas; continuam os caminhos anteriores de descarimbo, reabertura e simetria do modelo (`app/chamados/services/validacao.py:168`, `:269`, `:345`; `app/chamados/services/disputa_emissao.py:1494`, `:1609`; `app/chamados/models.py:1629`, `:3245`, `:3274`). | Porta, estado do modelo, juiz de vida, telas, painel, verbos, vigia e seleção/releitura do laço (`app/chamados/services/validacao.py:593`; `app/chamados/models.py:3151`; `app/chamados/juizes.py:1389`; `app/templates/chamados/partials/_pergunta_card.html:42`; `app/templates/chamados/partials/modal_fio.html:228`; `app/chamados/services/painel.py:270`; `app/chamados/services/verbos.py:264`; `app/chamados/services/vigia_carimbo.py:98`; `app/chamados/services/materializacao.py:2029`, `:2046`). |
   | `materializada_em` | Atribuição em `_escrever_carimbo`, administrada pelas portas de carimbo/descarimbo; o delta não cria atribuição paralela (`app/chamados/models.py:3207`, `:3209`, `:3239`, `:3295`, `:3391`). | Testemunha lê carimbo **e via**; laço relê após trava; passivo usa lastro e também relê para evitar repetir a irmã (`app/chamados/services/materializacao.py:856`, `:865`, `:2040`, `:2043`; `app/chamados/services/passivo_validado.py:534`, `:535`, `:537`). |
   | Nota do fio | `_carimbar_recusa` chama `nota.anexar_ocorrencia`, mantendo chave por pergunta/código/autor (`app/chamados/services/materializacao.py:1643`; `app/chamados/services/nota.py:62`, `:95`, `:100`, `:107`). | Carregamento dos followups e apresentação no fio/card (`app/chamados/services/fio.py:202`, `:206`; `app/templates/chamados/partials/modal_fio.html:500`; `app/templates/chamados/partials/_card_atendimento.html:53`). |
   | `DecisaoPergunta` | `registrar` continua sendo a porta; VALIDACAO e DEVOLUCAO continuam nos escritores da validação (`app/chamados/models.py:4110`, `:4120`; `app/chamados/services/validacao.py:173`, `:275`). | Histórico, fio, rótulo das 103, tarefa do dia, fotografia e backfill (`app/chamados/models.py:3676`; `app/chamados/services/fio.py:261`; `app/chamados/services/materializacao.py:275`; `app/chamados/services/tarefa_do_dia.py:421`, `:430`; `app/chamados/services/foto_e4b.py:280`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:67`). |

   Card e modal decidem pelo **estado**, sem comparar o texto de `detalhe`; fio e filtro publicam a mesma testemunha. Os helpers de espera também verificam o estado antes de devolver o detalhe (`app/templates/chamados/partials/_pergunta_card.html:44`, `:47`, `:49`; `app/templates/chamados/partials/modal_fio.html:252`, `:274`; `app/chamados/services/fio.py:373`, `:378`; `app/chamados/templatetags/chamados_tags.py:131`, `:139`; `app/chamados/services/materializacao.py:758`, `:773`).

   O explicador conserva sua escada por estado/lastro/espera; contador e comando consultam as autoridades compartilhadas. O contador permanece nas 163, sem novo balde L5 fora delas; a lista do comando dá saída falante à ausência de dia (`app/chamados/services/explicador.py:100`, `:116`, `:124`, `:131`; `app/chamados/services/materializacao.py:321`, `:339`, `:347`, `:356`; `app/chamados/management/commands/bug_par_103.py:74`, `:75`, `:77`).

   Há classificação **textual de recusa**, pelo catálogo existente, inclusive no delta e no passivo; isso é distinto de decidir a tela pelo detalhe. Há também regex anterior em `flip_auto`, restrita à via de paridade terminal (`app/chamados/catalogo/recusa.py:96`, `:101`; `app/chamados/services/materializacao.py:1448`; `app/chamados/services/passivo_validado.py:140`, `:144`; `app/ponto/services/flip_auto.py:128`, `:132`).

2. **Matriz e precedência — bloqueia: a precedência L2/L3 depende da ordem.**

   A estrutura das cinco linhas está implementada e os testes distinguem estado da testemunha, retorno da porta e apresentação, com as limitações abaixo; sua execução Django nesta verificação é **[nao verificado]** (`app/chamados/tests/test_bug_par_volta.py:222`, `:237`, `:275`, `:288`, `:299`).

   | Linha | Resultado conferido no código/teste | [nao verificado]
   |---|---|
   | L1 | Guarda entregue antes do carimbo; estado próprio, detalhe e ausência de Validar; fotografia de perguntas/atos/notas e ausência de batidas (`app/chamados/services/validacao.py:648`, `:651`; `app/chamados/tests/test_bug_par_volta.py:229`, `:231`, `:233`, `:235`). |
   | L2 | F6 conserva Validar; no cenário testado, exige `nao_gravou`, VALIDACAO→DEVOLUCAO, motivo/nota de competência e irmã preservada (`app/chamados/tests/test_bug_par_volta.py:247`, `:249`, `:252`, `:258`, `:261`, `:267`, `:273`). |
   | L3 | Espera restrita ao pertencimento congelado; porta espera sem persistir (`app/chamados/services/materializacao.py:742`, `:745`; `app/chamados/tests/test_bug_par_volta.py:281`, `:284`, `:286`). |
   | L4 | Aplicável, Validar e par gravado; o helper cobra batidas, carimbos e via (`app/chamados/tests/test_bug_par_volta.py:293`, `:296`, `:297`; `app/chamados/tests/test_bug_par_projetado.py:103`, `:109`). |
   | L5 | Fora163 sem dia devolve estado próprio antes do carimbo, sem consultar K6 nesse helper; tela oculta Validar (`app/chamados/services/materializacao.py:784`, `:800`, `:804`; `app/chamados/tests/test_bug_par_volta.py:307`, `:309`, `:311`, `:313`). |

   **Contraexemplo bloqueante:** saída U163 divergente, volta fora163, F6 aprovado, **VOLTA primeiro**. A testemunha anuncia L2; na escrita, a irmã alcança a espera tardia antes de `registrar_batida`. A espera desfaz o par sem preencher o registro de competência; o wrapper retorna `None`, e a tentativa isolada termina em PARIDADE. Portanto a precedência declarada não se realiza nessa ordem (`app/chamados/services/materializacao.py:903`, `:905`, `:1424`, `:1428`, `:1685`, `:1688`; `app/chamados/services/validacao.py:551`, `:561`, `:682`; `app/chamados/services/materializacao.py:1373`).

   O teste de precedência só valida **SAÍDA primeiro**; os testes das duas ordens exercitam sucesso sem F6. **Mudar:** corrigir a composição compartilhada para conservar a precedência nas duas ordens, sem antecipar globalmente outra guarda da porta, e acrescentar o contraexemplo (`app/chamados/tests/test_bug_par_volta.py:334`, `:44`, `:60`, `:64`).

   **Bloqueia a prova completa da matriz:** L3 descarta justamente a verificação de detalhe visível; L4 não cobra atos; L5 não verifica ausência de chamadas ao K6. Assim esses testes podem passar, respectivamente, com espera sem texto, validação sem atos e consulta indevida ao K6. **Mudar:** cobrar esses efeitos na própria linha da matriz (`app/chamados/tests/test_bug_par_volta.py:207`, `:209`, `:282`, `:297`, `:303`, `:313`; `app/chamados/tests/test_bug_par_projetado.py:103`).

   **Fila — F4:** cobra corretamente pendentes 1→0 num recorte resolvível, mas compara entrega sob mock fixo `False`. Fortalecer a invariância usando fixture da autoridade real (`app/chamados/tests/test_bug_par_volta.py:396`, `:401`, `:404`, `:407`; `app/chamados/tests/test_bug_par_projetado.py:51`).

3. **L2 — bloqueia: o motivo aprovado ainda se perde no par compartilhado pelo laço/passivo.**

   A lista só é instalada por `_validar_o_par`. Laço e passivo chegam diretamente à unidade, com a contextvar em `None`. Quando a irmã já validada é recusada por F6, seu savepoint desfaz a recusa; o impedimento retorna composto em texto, e a metade externa escreve “guarda de paridade”. O catálogo encontra PARIDADE antes da marca de competência (`app/chamados/services/validacao.py:545`; `app/chamados/services/materializacao.py:1759`, `:2048`, `:1690`, `:1693`, `:1373`, `:1930`; `app/chamados/services/passivo_validado.py:537`; `app/chamados/catalogo/recusa.py:45`, `:54`).

   Esse caminho é alcançável pelo passivo: sua seleção admite validadas sem carimbo e classifica ausência de plantio. **Mudar:** transportar a causa canônica também no par compartilhado; não ampliar automaticamente os atos/devoluções do lote para fazer essa correção (`app/chamados/services/vigia_carimbo.py:97`, `:98`, `:116`; `app/chamados/services/passivo_validado.py:537`).

   Os dois contraexemplos foram reproduzidos nesta sessão por funções reais extraídas por AST, com transação/ORM/insumos dublados: resultaram em PARIDADE e contextvar restaurada. Isso sustenta o caminho lógico; reprodução com Django permanece **[nao verificado]** (`app/chamados/services/materializacao.py:1373`, `:1693`; `app/chamados/services/validacao.py:563`).

   **Sem achado de vazamento no wrapper:** o token é restaurado em `finally`, inclusive no retorno da sentinela; esta impede o fallback. A irmã previamente validada não recebe nova assinatura dentro do savepoint; irmã materializada ou com batida existente não forma esse par (`app/chamados/services/validacao.py:548`, `:560`, `:563`, `:680`; `app/chamados/services/materializacao.py:148`, `:229`).

   No cenário L2 coberto, a trilha é coerente: rollback da tentativa da irmã, recusa canônica da pergunta examinada, preservação de sua VALIDACAO e DEVOLUCAO com o motivo persistido. As variantes com irmã previamente validada, retry recusado, laço e passivo não estão demonstradas pelo teste novo. **Acrescentar esses cenários**, cobrando pergunta, nota e atos conforme o contrato de cada entrada (`app/chamados/services/validacao.py:547`, `:558`, `:689`, `:275`; `app/chamados/tests/test_bug_par_volta.py:251`, `:268`, `:273`).

4. **Mutantes — bloqueia a entrega: integração executada permanece [nao verificado].**

   As nove âncoras foram conferidas em memória: ocorrência única, função correspondente, código mutado compilável e teste existente. O harness exige falha por asserção sem erros ou skips e restaura o código; erro não conta como morte (`app/chamados/tests/bug_par_mutantes.py:193`, `:204`, `:207`, `:210`, `:213`, `:217`).

   | Mutante | Alteração efetiva | Situação |
   |---|---|---|
   | `c4_fundir_l1_em_l2` | Troca estado do helper K6 (`app/chamados/services/materializacao.py:806`). | Asserção-alvo da testemunha em `app/chamados/tests/test_bug_par_volta.py:229`; execução Django **[nao verificado]**. |
   | `c4_motivo_l2_paridade` | Desliga a coleta que preserva o motivo (`app/chamados/services/materializacao.py:1447`). | Asserção-alvo em `app/chamados/tests/test_bug_par_volta.py:258`; execução **[nao verificado]**. |
   | `c4_desligar_portao_l3` | Elimina pertencimento nas divergências (`app/chamados/services/materializacao.py:742`). | Asserção-alvo em `app/chamados/tests/test_bug_par_volta.py:281`; execução **[nao verificado]**. |
   | `c4_barrar_l4` | Barra o ramo aplicável **do par** (`app/chamados/services/materializacao.py:919`). | Asserção-alvo em `app/chamados/tests/test_bug_par_volta.py:293`; execução **[nao verificado]**. |
   | `c4_fundir_l5_em_l1` | Troca o estado sem dia por entregue (`app/chamados/services/materializacao.py:804`). | Asserção-alvo em `app/chamados/tests/test_bug_par_volta.py:307`; execução **[nao verificado]**. |
   | `fundir_a_em_b` | Funde estados na testemunha (`app/chamados/services/materializacao.py:882`). | Asserção-alvo em `app/chamados/tests/test_bug_par_volta.py:229`; execução **[nao verificado]**. |
   | `fundir_a_em_b_porta` | Funde estados na porta (`app/chamados/services/validacao.py:230`). | Asserção-alvo em `app/chamados/tests/test_bug_par_volta.py:233`; execução **[nao verificado]**. |
   | `k6_omitido` | Desliga o ramo entregue (`app/chamados/services/materializacao.py:805`). | **Executado: morto por AssertionError, erros=0**, em `app/chamados/tests/test_bug_par_sem_banco.py:143`. |
   | `f6_esconde` | Troca F6 pelo estado que oculta Validar (`app/chamados/services/materializacao.py:905`). | Asserção-alvo em `app/chamados/tests/test_bug_par_volta.py:247`; execução **[nao verificado]**. |

   Ressalva de alcance: `c4_fundir_l1_em_l2` não funde efetivamente a porta, pois `_recusa_da_entrega` converte todo estado diferente de L5 em L1; quem exerce a fusão na porta é o mutante separado. Isso limita a prova desse mutante, sem constituir falha do contrato atual de dois estados (`app/chamados/services/validacao.py:228`, `:230`; `app/chamados/tests/bug_par_mutantes.py:103`, `:124`).

   Nesta sessão, os **7 testes sem banco passaram** e os **17 mutantes selecionados sem `--django` morreram por asserção, sem erros**. Esse modo exclui os mutantes de integração; não satisfaz o item 3d do aval. **Mudar na entrega:** executar e anexar a prova `--django` em ambiente autorizado para escrita de teste; não chamar o resultado sem Django de prova dos `c4_*` (`app/chamados/tests/test_bug_par_sem_banco.py:20`; `app/chamados/tests/bug_par_mutantes.py:178`, `:181`, `:213`). Não executei essa integração porque a configuração cria diretórios e usa banco; seu resultado permanece **[nao verificado]** (`app/config/settings/ci.py:6`, `:26`, `:29`, `:33`).

5. **Classes de reprovação — conferência do delta.**

   | Classe | Conclusão e mudança necessária |
   |---|---|
   | Segundo juiz | Sem achado novo: K6 chama o juiz registrado; F6 e catálogo continuam compartilhados (`app/core/juizes.py:343`; `app/chamados/services/materializacao.py:786`, `:789`, `:824`, `:1448`). |
   | Segundo escritor de motivo/nota/atos | Sem achado novo: o delta chama as portas existentes (`app/chamados/services/validacao.py:558`; `app/chamados/services/materializacao.py:1638`, `:1643`; `app/chamados/models.py:4110`). |
   | Relógio fora da borda | Nenhuma leitura nova de relógio identificada no trecho acrescentado; os carimbos continuam nos escritores anteriores (`app/chamados/services/validacao.py:545`, `:558`; `app/chamados/services/materializacao.py:1446`; `app/chamados/services/validacao.py:168`, `:275`). |
   | Derivação paralela | Sem achado novo: testemunha/porta compartilham K6; a guarda textual extrai seu detalhe (`app/chamados/services/validacao.py:221`; `app/chamados/services/materializacao.py:819`, `:821`, `:880`). |
   | Selo/RED sem mordida | **Bloqueia a prova** pelas lacunas da matriz e integração não executada, descritas nos itens 2 e 4 (`app/chamados/tests/test_bug_par_volta.py:282`, `:297`; `app/chamados/tests/bug_par_mutantes.py:178`). |
   | Ordem de trava | Sem deslocamento identificado no delta: trava anterior ao carimbo e à recusa nova; laço/passivo compartilham a trava do plantio (`app/chamados/services/validacao.py:643`, `:651`; `app/chamados/services/materializacao.py:1992`, `:2039`). |
   | Pouso antes do pré-requisito | Pouso, ancestralidade e execução da suíte inteira: **[nao verificado]**. | [nao verificado]
   | ESMERIL-SEMANTICA | A separação L1/L5 corrige a sobreposição; tupla e texto têm contratos distintos e leitores compatíveis. **Permanece bloqueante a causa real F6 apresentada como PARIDADE** nos caminhos dos itens 2 e 3 (`app/chamados/services/materializacao.py:804`, `:806`, `:821`, `:882`, `:1373`). |

   A sequência externa da porta continua com espera/implausibilidade antes da guarda K6 transacional; a prioridade interna da testemunha é K6, depois F6, depois espera onde seria aplicável. Não confundir isso com uma nova ordenação global da porta (`app/chamados/services/validacao.py:605`, `:611`, `:648`; `app/chamados/services/materializacao.py:880`, `:903`, `:916`).

   Universo do contador, exclusão das 163 na guarda e fonte JSON permanecem definidos nos mesmos pontos; o carregador valida hash, cardinalidade e pertencimento por PK. Comparação histórica independente com `c8b3ad99e`: **[nao verificado]** (`app/chamados/services/materializacao.py:321`, `:800`; `app/chamados/services/universo_bug_par.py:8`, `:15`, `:20`, `:35`).

6. **Escopo — sem excesso identificado no diff fornecido.**

   As alterações de produção apresentadas realizam a separação C3-A, seus leitores e o transporte do motivo L2; os demais acréscimos são testes/mutantes. Não identifiquei no diff fornecido alteração de motor, JSON, contador, comando/apply ou juiz de entrega (`app/chamados/services/materializacao.py:792`, `:1446`; `app/chamados/services/validacao.py:224`, `:545`; `app/templates/chamados/partials/_pergunta_card.html:47`; `app/templates/chamados/partials/modal_fio.html:274`).

   “Nenhum toque em competência exportada” não pode ser afirmado globalmente: as 163 continuam pulando essa guarda, conforme Q1, e o delta não altera esse ramo. Efeitos efetivos em produção e quantidade de perguntas vivas sem `data_evento` são **[nao verificado]**; o dado anexado não fornece essa medição (`app/chamados/services/materializacao.py:800`; `app/chamados/services/universo_bug_par.py:35`).

## 16/11 18:23 · sessao cb52556b7368 · - · - · origem=Ronald (tmux)

1. **Bloqueia — cura L2 incompleta no laço/passivo; defeito herdado que permanece.** O registro só nasce em `_validar_o_par` (`app/chamados/services/validacao.py:545`); laço e passivo chamam diretamente `_materializar_uma` (`app/chamados/services/materializacao.py:2048`; `app/chamados/services/passivo_validado.py:537`). Com ambas validadas, irmã recusada pelo chokepoint F6: o savepoint desfaz sua recusa (`materializacao.py:1690`), o wrapper devolve impedimento textual (`materializacao.py:1693`), e a metade externa escreve primeiro “guarda de paridade” (`materializacao.py:1373`; `materializacao.py:1930`), classificado PARIDADE antes de COMPETENCIA_APROVADA (`app/chamados/catalogo/recusa.py:45`; `recusa.py:54`). O passivo efetivamente admite perguntas sem carimbo de materialização (`app/chamados/services/vigia_carimbo.py:97`; `vigia_carimbo.py:116`). **Mudar:** transportar a causa canônica também no par compartilhado; preservar motivo sem ampliar automaticamente devolução/atos dos lotes. A execução Django deste cenário é [nao verificado]; probe AST em memória com helpers reais e transação/ORM/chokepoint simulados devolveu `False`, motivo externo `PARIDADE`, recusa da irmã desfeita e contextvar `None`.

2. **Bloqueia — precedência L2 versus L3 depende da ordem das metades.** Com saída U163 divergente, volta fora163 e F6 aprovado, a testemunha da volta diz L2 (`materializacao.py:903`). Validando VOLTA primeiro, a irmã SAÍDA alcança a espera tardia antes do chokepoint (`materializacao.py:1424`; `materializacao.py:1428`); a espera atravessa o wrapper da irmã (`materializacao.py:1685`), deixando o registro de competência vazio. `_validar_o_par` desfaz e retorna `None` (`validacao.py:551`; `validacao.py:561`), e o fallback isolado (`validacao.py:682`) escreve PARIDADE (`materializacao.py:1373`). A irmã pode inicialmente aparecer bloqueada por paridade porque a metade externa em voo é excluída (`materializacao.py:149`); isso ainda faz `espera_na_escrita` retornar vazio (`materializacao.py:773`). **Mudar:** resolver a precedência na composição compartilhada e testar as duas ordens; não antecipar globalmente nova guarda da porta. O teste novo só valida SAÍDA primeiro (`app/chamados/tests/test_bug_par_volta.py:334`). Execução Django é [nao verificado]; probe AST com helpers reais e insumos simulados produziu wrapper `None`, zero chamadas ao chokepoint, motivo final PARIDADE e contextvar restaurada.

3. **Sem achado novo no ciclo de vida da contextvar/sentinela.** Há `reset(token)` em `finally`, inclusive no retorno da sentinela (`validacao.py:560`; `validacao.py:562`); o chamador reconhece identidade e evita a tentativa isolada (`validacao.py:680`). Irmã previamente validada passa pelo mesmo wrapper, sem novo carimbo (`validacao.py:548`); irmã materializada é excluída da seleção (`materializacao.py:148`), e irmã com batida existente impede formar o par (`materializacao.py:229`). Vazamento sob integração Django é [nao verificado].

4. **Sem segundo escritor/juiz ou relógio novo identificado no toque L2.** O novo caminho chama `_carimbar_recusa` (`validacao.py:558`), que conserva escrita do motivo e nota pela porta canônica (`materializacao.py:1638`; `materializacao.py:1643`; `app/chamados/services/nota.py:75`). VALIDACAO e DEVOLUCAO continuam nos escritores existentes (`validacao.py:173`; `validacao.py:275`), após a trava existente (`validacao.py:643`). O catálogo é reutilizado para classificar o retorno do chokepoint (`materializacao.py:1448`); não há derivação própria de competência nesse trecho (`materializacao.py:1449`).

5. **Cobertura insuficiente para os casos acima.** A matriz L2 nova parte de ambas sem validação e valida somente SAÍDA (`test_bug_par_volta.py:244`; `test_bug_par_volta.py:251`; `test_bug_par_volta.py:268`). Os testes das duas ordens exercitam sucesso/retry, sem F6 (`test_bug_par_volta.py:44`; `test_bug_par_volta.py:60`). O teste anterior de rollback no laço fecha somente a saída e verifica apenas o motivo dela (`app/chamados/tests/test_bug_par_projetado.py:710`; `test_bug_par_projetado.py:722`). **Mudar:** acrescentar L2 nas duas ordens, irmã previamente validada, ambas sob F6, laço e passivo; medir motivo da pergunta, nota e atos nos respectivos contratos.

_6152 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
