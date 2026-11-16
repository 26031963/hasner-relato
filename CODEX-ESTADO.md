# CODEX-ESTADO

> Ultima resposta processada: **16/11/2026 09:25** · publicado em **16/11/2026 09:27** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 16/11 09:25 · sessao cf0625b825b4 · - · - · origem=Ronald (tmux)

Auditoria estática concluída e enviada com referências.

- A porta valida o par futuro, exige colaborador travado e escreve sem adquirir novas travas; o chamador trava antes de registrar e zerar. `app/chamados/models.py:1612`, `app/chamados/models.py:3370`, `app/chamados/models.py:3385`.
- Os testes detectam a volta de REABERTURA, mas não provam preservação de texto na corrida; todas as fixtures alteradas têm texto vazio. `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:264`, `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:285`, `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:304`.
- O teste do censo exige a chamada literal; inserir REABERTURA mantendo essa chamada escapa dele individualmente. `app/core/tests/test_censo_escritas.py:241`, `app/core/tests/test_censo_escritas.py:252`.
- Execução dos testes, sonda comparativa e pré-requisitos do pouso: [nao verificado]. [nao verificado]

## 16/11 09:25 · sessao cd853badbde5 · - · - · origem=Ronald (tmux)

Achados para consolidar:

- **PROCEDE — histórico da tela fica sem ato de retirada.** `respostas_anteriores()` considera uma RESPOSTA vigente até outra RESPOSTA ou ato REABERTURA/RECUSA; VALIDACAO não a encerra. Portanto, numa corrida com resposta chegando antes da trava e sendo apagada pela ressuscitação, a linha RESPOSTA permanece na trilha bruta, mas não aparece como resposta anterior na tela. `app/chamados/models.py:3674`, `app/chamados/models.py:3680`; delegação em `app/chamados/services/disputa_emissao.py:2119` e carregamento em `app/chamados/services/fio.py:258`.

- **PROCEDE — há limpeza sem registro dos valores anteriores por pergunta.** Entrada com `materializada_em != None`, texto vazio e ambos os instantes de resposta/validação nulos passa pelo filtro inicial; o helper devolve `[]`, e a porta apaga instante, autor e via da materialização. A auditoria final registra somente IDs e rótulos de estado. `app/chamados/models.py:1530`, `app/chamados/models.py:4091`, `app/chamados/models.py:3391`, `app/chamados/models.py:1653`. Isso comprova perda da foto do carimbo; nessa entrada o texto anterior já é vazio. `app/chamados/models.py:1533`.

- **PROCEDE — leitor histórico de atividade usa projeção apagável.** Painel chama `validada_em/validada_por` de atividade humana e conta pela projeção; `verbos` também conta validações e respostas pelos campos atuais, e procedência da batida usa esses autores/instantes. Ressuscitar pode apagar esses fatos dos respectivos resultados apesar de conservar linhas R/V. `app/chamados/services/painel.py:256`, `app/chamados/services/verbos.py:263`, `app/chamados/services/verbos.py:333`, `app/ponto/services/procedencia_batida.py:70`; limpeza em `app/chamados/models.py:1629` e `app/chamados/models.py:3393`. Exclusividade dessa consequência em relação ao main: [nao verificado].

- **PROCEDE — significados de “respondida” ainda coexistem nos leitores.** C6 exige instante **e** texto; `estado`, propriedade `respondida` e partição do app usam apenas instante. O helper histórico também produz RESPOSTA com texto vazio quando há instante, e tarefa distingue isso como `respondida_sem_texto`. `app/chamados/juizes.py:1421`, `app/chamados/models.py:3154`, `app/chamados/models.py:3664`, `app/chamados/services/pergunta_no_app.py:81`, `app/chamados/models.py:4098`, `app/chamados/services/tarefa_do_dia.py:148`.

Mapa dos escritores efetivos:

| Campo | Escritores na cópia |
|---|---|
| `respondida_em` | resposta normal, automática e recuperação pela trilha: `app/chamados/services/disputa_emissao.py:1482`, `:1675`, `:1696`; carimbo/restauro: `app/chamados/models.py:3242`, `:3273`; limpeza pelo lote: `app/chamados/models.py:1629`, `:3393`. |
| `resposta_colab` | resposta normal/automática: `app/chamados/services/disputa_emissao.py:1481`, `:1674`; reabertura/lote: `app/chamados/services/disputa_emissao.py:1609`, `app/chamados/models.py:1630`, `:3393`. |
| `validada_em`, `validada_por` | validação/devolução: `app/chamados/services/validacao.py:159`, `:224`; nova resposta desfaz: `app/chamados/services/disputa_emissao.py:1494`; carimbo/restauro: `app/chamados/models.py:3245`, `:3274`; limpeza do lote: `app/chamados/models.py:1629`, `:3393`. |
| `materializada_em`, `materializada_por` | atribuição central: `app/chamados/models.py:3209`; portas que a chamam: `:3239`, `:3293`, `:3318`; lote: `:3393`. |
| `via_resolucao` | trio central e lote acima; alteração isolada por fato no espelho: `app/chamados/models.py:3402`; relavra: `app/chamados/services/relavra_via.py:125`. |
| `DecisaoPergunta` | escritor `registrar`, inserção em `app/chamados/models.py:4120`; ressuscitar entrega as linhas em `:1613`; helper preenche ato/instante/texto/autor em `:4100` e `:4104`. |

Inserção por `PerguntaDisputa.abrir(**campos)` também recebe projeção inicial e registra R/V; o `save` aplica a recusa do par. `app/chamados/models.py:3124`, `app/chamados/models.py:3129`, `app/chamados/models.py:3558`.

Outros leitores relevantes:

- C6 lê validação, par e carimbo/via como estado atual: `app/chamados/juizes.py:1389`, `:1405`, `:1416`, `:1423`, `:1444`, `:1450`.
- App seleciona por C6 e depois separa pendentes pelo instante/disputa aberta: `app/chamados/services/pergunta_no_app.py:52`, `:61`, `:81`; `medir` usa os destinos das vivas: `:208`.
- Tarefa lê sequência por `ato_em,pk`, passivo pelo helper e efeitos pela lavra: `app/chamados/services/tarefa_do_dia.py:410`, `:419`, `:424`; RESPOSTA pede validação, REABERTURA/RECUSA devolvem ao colaborador, SILENCIO encerra: `:148`, `:152`, `:165`.
- Materializador seleciona instantes de resposta/validação com materialização nula: `app/chamados/services/materializacao.py:1376`; usa texto como anuência: `:835`.
- Cache da lavra usa máximos dos carimbos da pergunta como marcos de mudança: `app/chamados/veredito_lavrado.py:202`.
- Respostas corrigíveis selecionam pelo instante de resposta: `app/chamados/juizes.py:1739`; fila global de validar usa par com texto e C6: `:1746`.

Chamadores fora do diff: reabertura do chamado em `app/chamados/models.py:1498`, desarquivamento em `app/chamados/views_arquivar.py:86` e cartório em `app/ponto/services/cartorio.py:1673`.

## 16/11 09:24 · sessao d3593eac2ade · - · - · origem=Ronald (tmux)

2. **PROCEDE — universo mais amplo que as duas fixtures novas.** A seleção exige pergunta que resolve o chamado, `materializada_em != None`, texto SQL `''` ou `NULL`, via diferente de `duplicata`/`motivo_retratado`; depois pula marco provadamente aceso e exige C6 vivo na cópia descarimbada. Não restringe `respondida_em`, `validada_em`, autores, última decisão, `conversa` nem disputa fechada. Portanto os eixos de entrada são resposta com/sem instante × validação com/sem instante × qualquer sequência/último ato × disputa aberta/fechada, desde que passem os dois guardas. `resposta_colab` não admite NULL no esquema corrente, embora o filtro o contemple. (`app/chamados/models.py:1530`, `:1548`, `:1550`, `:1553`, `:1557`, `:2498`, `:2552`.)

2. **NAO HA — ato novo de devolução no caminho c2.** Ressuscitar registra apenas projeções ainda sem linha e chama diretamente a porta que zera o par, validação e carimbo; não chama `reabrir_para_correcao`. (`app/chamados/models.py:1612`, `:1613`, `:1628`, `:1629`.)

2. **PROCEDE — resultado da tarefa depende da sequência anterior.** Sem decisão: `aguarda_resposta`/responder-colaborador; RESPOSTA vazia: `respondida_sem_texto`/responder-colaborador; RESPOSTA com texto: `aguarda_validacao`/validar-admin; RECUSA/REABERTURA prévia: `devolvida_ao_colaborador`; SILENCIO: `silencio_decidido`/nenhuma ação; VALIDACAO: resultado depende da lavra/efeito; ABSORCAO: depende do status da ausência. A sequência é cortada por `ato_em <= agora`. (`app/chamados/services/tarefa_do_dia.py:143`, `:145`, `:148`, `:152`, `:156`, `:165`, `:167`, `:410`.)

2. **PROCEDE — antes/depois não é invariância geral desta porta.** Projeção legada com instante e sem linha é `passivo_sem_linha` antes; a porta cria RESPOSTA/VALIDACAO, apaga a projeção e pode passar a outra razão depois. A mudança pode ocorrer mesmo sem REABERTURA. (`app/chamados/services/tarefa_do_dia.py:143`, `:419`; `app/chamados/models.py:4091`, `:4098`, `:4102`, `:1613`, `:1629`.) Isto é comparação antes/depois na c2; igualdade/diferença factual com `ab616d1c7` permanece **[nao verificado]**, porque a árvore não contém histórico Git nem cópia identificada desse main.

2. **PROCEDE — app e medidor ignoram a decisão de SILENCIO.** `perguntas_do_app` usa `q_vivas` seguido de C6; `particao_do_app` chama de pendente a viva com `respondida_em=None` e disputa aberta; `medir` transforma todas as vivas em destinos `no_app`, e esses destinos não recebem causa de retenção. Portanto SILENCIO pode continuar sem ação na tarefa e a pergunta constar viva/pendente no app. (`app/chamados/services/pergunta_no_app.py:52`, `:61`, `:81`, `:141`, `:208`, `:209`; `app/chamados/services/tarefa_do_dia.py:165`.)

2. **PROCEDE — C6 pós não é garantido sem congelar os insumos externos.** A seleção pergunta ao C6 sobre a célula projetada do julgamento; a leitura posterior ordinária pode usar a célula persistida anterior à lavra. O cartório explicitamente pede novo julgamento depois da ressuscitação. Sob os mesmos insumos da cópia, par/carimbo/validação zerados levam ao resultado vivo aprovado pela própria seleção. (`app/chamados/models.py:1548`, `:1553`, `:1557`; `app/chamados/juizes.py:415`, `:428`, `:1405`, `:1454`, `:1482`; `app/ponto/services/cartorio.py:1693`.)

5. **PROCEDE — comparação dos testes novos não cobre o dono publicado inteiro.** `_portas_da_tarefa` só dá porta de colaborador quando há RESPONDER e a pergunta está no conjunto de pendentes do app; calcula depois `fio`/`rota`. Uma fixture legada com instante pode conservar a razão/ação mas alterar essa disponibilidade depois que o instante é zerado. Os testes novos verificam apenas a razão e ações de uma instância, omitindo `fio`, `rota`, `fila`, `verbos`. (`app/chamados/services/tarefa_do_dia.py:610`, `:616`, `:622`, `:635`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:320`.)

6. **PROCEDE — três chamadas diretas de produção fora do trecho alterado.** `ChamadoColaborador.reabrir` ressuscita depois de gravar o estado do chamado; desarquivar pela tranca chama novamente depois de escrever o toque humano; o cartório ressuscita e conta/pede rejulgamento quando houve mudança. Todos passam a receber o protocolo c2 sem REABERTURA, em lugar do protocolo B1 anterior mostrado no anexo. (`app/chamados/models.py:1489`, `:1498`; `app/chamados/views_arquivar.py:77`, `:86`; `app/ponto/services/cartorio.py:1673`, `:1692`.)

6. **NAO HA — novos filtros paralelos nos consumidores enumerados.** A carga de reabrir usa `perguntas_a_ressuscitar`; o comando de órfãs também usa essa porta para recusar ressuscitação de irmãs. O universo efetivo continua centralizado em `_universo_da_ressuscitacao`. (`app/chamados/services/disputa_emissao.py:1820`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:107`; `app/chamados/models.py:1507`.)

## 16/11 09:00 · sessao 8b0372b78864 · CORTES-F2-F9-1611 · verificador · origem=Code

**AVAL-DEVOLVIDO: CORTES-F2-F9-1611 | somente item 9 | perguntas 2 e 4.** (`cortes_f2_f9_1611.txt:11`; `revisao_do_aval.txt:6`)

Referências aos anexos numeradas desde a primeira linha de cada conteúdo; F2/F4/F6 distinguem os três `matriz.md`.

1) **NAO PROCEDE.** O wrapper **não oferece modo/flag para escrever RED fora do papel construtor**: o cabeçalho exclui essa via e o parser oferece apenas `--sonda`, `--anexo` e `--construtor`. A permissão normativa existe, mas não está implementada como modo separado nesse wrapper. (`CLAUDE.md:349–350`; `bin/codex_pergunta.sh:30–33`, `:58–66`)

A via disponível é `CODEX_PAPEL=construtor` com `--construtor <wt>`; exige `logs/construtor.vaga`, por `flock -n`, e recusa vaga ocupada. Os demais papéis executam em `read-only`. **Isso cabe no “senão” já escrito:** o item 9 diz “fora da vaga”, mas permite entrar atrás da espinha quando o wrapper a exigir. Portanto, essa limitação não torna o item inexequível. (`bin/codex_pergunta.sh:197–200`, `:248–253`, `:418–426`; `cortes_f2_f9_1611.txt:10`)

O contrato deve anteceder o primeiro uso; a existência do contrato específico desta obra está **[nao verificado]**. (`app/docs/PISTA.md:143–145`, `:170–175`)

2) **PROCEDE.** O item exige mordida, mas não determina o encaminhamento quando o teste não derruba o mutante; também não determina o encaminhamento de dependência cuja marca seja ambígua ou de teste inviável sem alterar fixture fora de `tests/`. A necessidade concreta dessa última alteração está **[nao verificado]**. Os “senões” expressos cobrem cruzamento de arquivos e exigência da vaga. (`cortes_f2_f9_1611.txt:10`) [nao verificado]

A ambiguidade de dependência é concreta: F4 M23 não tem marca na linha, mas seu contrato declara `[DEPENDE-F1]`; F2 também concentra dependências em seção separada. Falta dizer como classificar esses degraus antes da F1. (`anexo F4/matriz.md:57`, `:107`; `anexo F2/matriz.md:69–77`)

3) **NAO PROCEDE — nenhuma colisão demonstrada.** A regra permite RED próprio sob contrato e revisão; o item preserva a espera quando cruzar arquivos, inclusive testes. A autorização posterior, específica para **só testes**, não constitui automaticamente colisão com o veto anterior à construção das fatias. (`CLAUDE.md:309–310`, `:349–350`; `anexo F4/matriz.md:4–5`; `cortes_f2_f9_1611.txt:10`)

O complemento BUG-PAR mantém sua construção aguardando F1 e autoriza somente ensaio de leitura; o item 9 não libera essa construção. (`complemento2_prepara_volta_bug_par.txt:1–2`; `cortes_f2_f9_1611.txt:10`)

Ausência de cruzamento com as árvores atuais F1 B1 c2, R3, BUG-PAR e LOTE7/LOTE3 c7: **[nao verificado]**. O registro disponível dos lotes descreve c6. (`app/docs/RELATO.md:7`, `:12–14`)

As matrizes históricas também precisam respeitar a L-130 vigente; isso não demonstra colisão de um teste ainda não escrito, pois entrada ilegal pode testar recusa. (`app/docs/LEIS.md:151`)

4) **PROCEDE.** As matrizes identificam degraus sem prova, mas a seleção literal não é inequívoca: F2 distingue integrais, parciais e coluna BASE; F4 declara dependências também no contrato; F6 inclui S1/S2 na lista sem prova apesar de retirá-los do escopo. (`anexo F2/matriz.md:100–102`; `anexo F4/matriz.md:105–110`; `anexo F6/matriz.md:48–49`, `:94`)

Contando **degraus**, pela marca explícita **na própria linha**:

| Universo | PROVA=NENHUM | Com marca | Sem marca | Fonte |
|---|---:|---:|---:|---|
| F2, integrais da coluna c3 | 10 | 0 | **10** | `anexo F2/matriz.md:35–56`, `:101` |
| F2, incluindo quatro parciais | 14 | 0 | **14** | `anexo F2/matriz.md:100–101` |
| F4 | 21 | 3 | **18** | `anexo F4/matriz.md:43`, `:72`, `:91`, `:127` |
| F6 | 11 | 8 | **3** | `anexo F6/matriz.md:32–49`, `:94` |

Na F4, os três marcados sem prova são **M9b, E9 e J1**. Na F6, os três sem marca são **U9b, S1 e S2**; excluir S1/S2 deixaria **um**, mas o item 9 não explicita essa exclusão. Ausência da marca na linha não comprova independência da F1. (`anexo F4/matriz.md:43`, `:72`, `:91`, `:107`; `anexo F6/matriz.md:44`, `:48–49`; `cortes_f2_f9_1611.txt:10`)

Existência e conteúdo das árvores históricas e dos caminhos externos de logs, além do material anexado: **[nao verificado]**.

VEREDITO: PROCEDE em 2 e 4.

## 16/11 09:00 · sessao 1cdcad368529 · LOTE7-C7 · verificador · origem=Code

**Por leitura, o teste novo cobre a falha intermediária que faltava; não identifiquei defeito novo no delta.** Ele observa duas tentativas de auditoria, zero rejeições remanescentes e contagem de trilhas restaurada após a exceção. Execuções, mutantes, logs, produção e identidade histórica da árvore: `[nao verificado]`. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:277`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:280`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:281`)

1. **Ordem das chamadas e contagem de auditorias**

   A sequência atual é: abrir `atomic`, carregar as ausências por **PK**, salvar a primeira como rejeitada, executar sua auditoria real, salvar a segunda como rejeitada e tentar sua auditoria. O mock devolve `None` nessa segunda tentativa; o serviço levanta dentro do `atomic`, desfazendo as duas rejeições e a primeira trilha. “Primeira” significa primeira PK selecionada; o teste não depende de identificar determinada data. (`app/ponto/services/ausencia.py:1484`; `app/ponto/services/ausencia.py:1490`; `app/ponto/services/ausencia.py:1498`; `app/ponto/services/ausencia.py:1499`; `app/ponto/services/ausencia.py:1506`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:265`)

   O alias `_ev_rej` é importado dentro da função, portanto recebe o mock ativo. `chamadas` aumenta somente para o par `('rejeitar', 'Ausencia')`; outros eventos passam à função real sem aumentar esse contador. (`app/ponto/services/ausencia.py:1482`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:266`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:270`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:272`)

   **Não encontrei outro produtor desse par no caminho conferido.** O signal chama o cartório; pela construção desta fixture, não há células, e o cartório retorna antes do julgamento. O consumidor fica em `on_commit`. O acordador final não é alcançado, pois está depois do laço interrompido. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:64`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:194`; `app/ponto/signals.py:147`; `app/ponto/services/cartorio.py:2027`; `app/ponto/services/cartorio.py:2029`; `app/ponto/signals.py:272`; `app/ponto/services/ausencia.py:1508`)

   **Não identifiquei trilha legítima que sobreviva a esse rollback e faça a contagem global falhar indevidamente.** Havendo células, o cartório pode emitir `('veredito_celula', 'CelulaDia')`, mas essa escrita síncrona também fica dentro da transação da rejeição; `evento` usa um savepoint interno, não uma confirmação independente. O consumidor agendado nesse ato não executa após seu rollback. (`app/ponto/portas/celula.py:1100`; `app/core/observ.py:133`; `app/core/observ.py:134`; `app/ponto/services/ausencia.py:1484`; `app/ponto/services/ausencia.py:1506`; `app/ponto/signals.py:272`)

2. **Comentários sobre o teto**

   **Sim, os dois agora descrevem o comportamento implementado.** `_teto_da_linha_aberta` considera competências com `derivado_em` não nulo e devolve o maior entre hoje e o fim da última competência carimbada. `_acordar_projecao` fornece esse teto ao juiz dos dias; edição e decisão chamam esse mesmo acordador. (`app/ponto/services/ausencia.py:147`; `app/ponto/services/ausencia.py:151`; `app/ponto/services/ausencia.py:192`; `app/ponto/services/ausencia.py:665`; `app/ponto/services/ausencia.py:669`; `app/ponto/services/ausencia.py:1119`; `app/ponto/services/ausencia.py:1123`)

   Não encontrei comentário contrário nos trechos pesquisados da obra. As duas ocorrências potencialmente ambíguas estão qualificadas: “competência futura não tem projeção” aparece para dizer que isso **não é garantido**; “fim vazio = HOJE” descreve a fórmula que **era** usada. Censo universal de toda documentação: `[nao verificado]`. (`app/ponto/services/ausencia.py:170`; `app/ponto/services/ausencia.py:668`)

3. **DESCARTADOS da v2**

   | Motivo | Verificação |
   |---|---|
   | Auditoria antes do `save`, mantendo `atomic`, é equivalente | **Sustenta-se para o contrato persistido “nenhuma rejeição sem trilha”.** Ambas as escritas continuam no mesmo bloco, e `None` provoca rollback. Não implica equivalência de toda ordem observável dos signals. (`app/ponto/services/ausencia.py:1484`; `app/ponto/services/ausencia.py:1498`; `app/ponto/services/ausencia.py:1506`) |
   | `julgada_em = gerada_em` está fora do escopo | **Aceitável como delimitação, não como eliminação da lacuna.** O relógio congelado ainda permite igualdade entre geração e `instante`; o espião demonstra julgamento real por dia, mas não distingue esses timestamps. A implementação atual grava `agora`. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:60`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:481`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:490`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:497`; `app/ponto/portas/celula.py:1194`) |
   | `dias_corridos/hora_ini/hora_fim` podem ficar fora do retrato | **Sustenta-se para o escritor atual:** a rejeição persiste somente `status/atualizado_em`, e o override repassa esse `update_fields`. Continua sem provar preservação dos campos omitidos contra mudanças futuras. (`app/ponto/services/ausencia.py:1498`; `app/ponto/models.py:918`; `app/ferias/services.py:683`) |
   | Adesão, veredito e ata ficam fora | São limites de cobertura coerentes: a fixture percorre `nunca_bateu`; os asserts conferem atividade, status, carimbo e FK, sem comparar ata/veredito. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:487`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:493`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:499`; `app/ponto/services/cartorio.py:1318`) |
   | Relógio, D e invalidação sem retentativa são dívida anterior | Os mecanismos continuam presentes; anterioridade e fila efetiva são `[nao verificado]`. (`app/ponto/services/ausencia.py:184`; `app/ponto/services/ausencia.py:191`; `app/ponto/signals.py:88`; `app/core/observ.py:86`; `app/ponto/services/fechamento.py:1661`) |
   | `obs` como metadado é dívida anterior | A interpretação por substrings existe. Anterioridade e encaminhamento à fila: `[nao verificado]`. (`app/ferias/management/commands/listar_ferias_dp.py:27`; `app/ferias/management/commands/listar_ferias_dp.py:30`) |

   Os demais limites herdados também permanecem: `_nada_mudou` admite qualquer trilha nova, sem validar sua legitimidade; o selo exige presença de chamada viva, enquanto o positivo impede um acordador globalmente inoperante. Execuções relatadas e igualdade com a base: `[nao verificado]`. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:545`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:574`; `app/ponto/tests/test_lote7_selo_escritor_acorda.py:222`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:224`)

4. **Escritores, leitores e significado**

   | Dado | Quem escreve | Quem lê / significado |
   |---|---|---|
   | `LogAuditoria` anterior | A criação das ausências emite `criar`; `evento` insere as linhas. (`app/ponto/services/ausencia.py:585`; `app/core/observ.py:134`) | O teste fotografa a **contagem total**, depois de `_agendar`, e compara após a exceção. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:261`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:262`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:280`) |
   | Trilha da rejeição | `_ev_rej` fornece antes/depois; a primeira chamada é real, a segunda retorna `None`. (`app/ponto/services/ausencia.py:1502`; `app/ponto/services/ausencia.py:1503`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:268`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:270`) | O serviço verifica **sucesso da gravação** pelo retorno; o teste verifica **cardinalidade final**. São medidas diferentes, compatíveis com o rollback esperado. (`app/ponto/services/ausencia.py:1503`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:280`) |
   | `Ausencia.status` | O espelho cria `aprovada`; a rejeição atribui e salva `rejeitada`. (`app/ponto/services/ausencia.py:1391`; `app/ponto/services/ausencia.py:1496`; `app/ponto/services/ausencia.py:1498`) | A seleção exclui rejeitadas, a trilha registra esse mesmo estado e o teste exige zero rejeitadas no banco. O significado concorda. (`app/ponto/services/ausencia.py:1489`; `app/ponto/services/ausencia.py:1503`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:278`) |
   | Status como insumo | `_insumo_da_folha` consulta o estado persistido. (`app/ponto/services/ausencia.py:138`) | Folha inclui somente aprovadas; cobrança exclui rejeitadas. São efeitos distintos do mesmo estado, explicitamente separados pelo predicado. (`app/ponto/turnos.py:2528`; `app/ponto/turnos.py:2531`) |

5. **Defeito novo no delta**

   **Nenhum identificado por leitura.** O limite restante do teste novo é que zero rejeitadas e contagem igual de trilhas não demonstram, isoladamente, identidade ou restauração integral dos registros. O snapshot mais forte continua no negativo da **primeira** falha; isso delimita a prova do teste intermediário, sem evidenciar regressão no serviço atual. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:238`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:252`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:278`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:280`; `app/ponto/services/ausencia.py:1484`)

## 16/11 09:00 · sessao 7519f15333fc · - · - · origem=Ronald (tmux)

Revalidação por script em memória:

- **F2:** seção `[DEPENDE-F1]`:69; lista14:100; contagem10 integrais +4 parciais:101; coluna BASE:102.
- **F6:** U9b:44; S1:48; S2:49; lista11:94.
- **F4:** usar sua numeração corrigida; minha contagem anterior omitiu a linha de `fabrica_da_celula.py`.

## 16/11 08:59 · sessao 53037c2ed30b · - · - · origem=Ronald (tmux)

1. A ordem observada é `atomic` → carregar/travar ausências por PK → capturar insumo → para cada linha, salvar `status='rejeitada'` → chamar `evento('rejeitar','Ausencia')`; na segunda resposta `None`, a exceção sai do `atomic`, antes de alcançar `_acordar_projecao`. O teste depende de a primeira chamada filtrada voltar da função real e a segunda ser interceptada, não de qual data representa cada PK. (`app/ponto/services/ausencia.py:1484`, `app/ponto/services/ausencia.py:1488`, `app/ponto/services/ausencia.py:1494`, `app/ponto/services/ausencia.py:1498`, `app/ponto/services/ausencia.py:1499`, `app/ponto/services/ausencia.py:1506`, `app/ponto/services/ausencia.py:1508`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:265`)

2. O alias de `evento` é importado dentro de `rejeitar_ausencias_de_ferias`, portanto já recebe o mock ativo. O contador filtra estritamente `(acao,modelo)==('rejeitar','Ausencia')`; outros eventos seguem pela função real sem incrementar `chamadas`. (`app/ponto/services/ausencia.py:1482`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:266`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:270`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:272`)

3. No caminho lido, não há segundo produtor desse par: o `post_save` de Ausência chama `_julgar_dias`/`julgar_celula`; o cartório devolve `None` sem célula, enquanto o consumidor da ausência agenda trabalho em `on_commit`. O acordador final sequer é alcançado pela falha na segunda auditoria. (`app/ponto/signals.py:143`, `app/ponto/signals.py:147`, `app/ponto/signals.py:85`, `app/ponto/signals.py:93`; `app/ponto/services/cartorio.py:2027`, `app/ponto/services/cartorio.py:2029`; `app/ponto/signals.py:250`, `app/ponto/signals.py:272`; `app/ponto/services/ausencia.py:1506`, `app/ponto/services/ausencia.py:1508`)

4. A fixture comum e `_agendar` não criam células ou batidas; criam colaborador e agendamento, que espelha cinco Ausências aprovadas e suas trilhas de criação antes da fotografia `trilhas_antes`. A afirmação de ausência de células é inferência estática desses construtores; estado efetivamente executado `[nao verificado]`. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:64`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:194`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:202`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:205`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:261`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:262`; `app/ferias/models.py:444`, `app/ferias/models.py:449`; `app/ponto/services/ausencia.py:1389`, `app/ponto/services/ausencia.py:1391`, `app/ponto/services/ausencia.py:585`)

5. Mesmo havendo células, o cartório pode escrever auditoria legítima com outro par, `('veredito_celula','CelulaDia')`; ela não aumenta `chamadas`. Esses escritores síncronos ficam dentro do `atomic` externo da rejeição e também desfazem com ele; o `on_commit` do consumidor é registrado nesse mesmo ato e não roda após seu rollback. Não identifiquei escritor legítimo que sobreviva ao rollback e cause falso negativo na contagem global deste teste. (`app/ponto/portas/celula.py:1085`, `app/ponto/portas/celula.py:1100`; `app/ponto/services/ausencia.py:1484`, `app/ponto/services/ausencia.py:1498`, `app/ponto/services/ausencia.py:1506`; `app/ponto/signals.py:272`)

Mapa dos campos no caminho:

| Dado | Escritor | Leitor/significado |
|---|---|---|
| `LogAuditoria` prévio | A criação de cada Ausência chama `evento('criar',...)`; `evento` insere o registro em savepoint. (`app/ponto/services/ausencia.py:585`; `app/core/observ.py:133`, `app/core/observ.py:134`) | O teste fotografa a contagem **total**, após `_agendar`, e exige a mesma contagem após a exceção. Não conta somente rejeições. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:261`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:262`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:280`) |
| Auditoria da rejeição | `_ev_rej` fornece status antes/depois; `evento` grava via `LogAuditoria.objects.create`. A primeira chamada é real, a segunda devolve `None` sem gravar. (`app/ponto/services/ausencia.py:1499`, `app/ponto/services/ausencia.py:1502`, `app/ponto/services/ausencia.py:1503`; `app/core/observ.py:134`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:268`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:270`) | O serviço testa **sucesso de gravação**, pelo retorno `is None`; o teste mede **cardinalidade final**, conceito diferente mas compatível com rollback esperado. (`app/ponto/services/ausencia.py:1503`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:280`) |
| `Ausencia.status` inicial | Espelho chama `criar_ausencia(...status='aprovada')`, e esta usa esse status nos campos da criação. (`app/ponto/services/ausencia.py:1391`, `app/ponto/services/ausencia.py:486`) | `_insumo_da_folha` lê do banco pelo predicado `_pelo_efeito('folha')`, que inclui somente aprovadas. (`app/ponto/services/ausencia.py:138`; `app/ponto/turnos.py:2528`, `app/ponto/turnos.py:2529`) |
| `Ausencia.status` rejeitado | Atribuição e `save(update_fields=['status','atualizado_em'])`. (`app/ponto/services/ausencia.py:1496`, `app/ponto/services/ausencia.py:1498`) | A seleção exclui já rejeitadas; a auditoria registra o mesmo literal; o teste conta esse literal no banco. O modelo também o declara como “Rejeitada”. (`app/ponto/services/ausencia.py:1489`, `app/ponto/services/ausencia.py:1503`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:278`; `app/ponto/models.py:704`) |
| Status para cobrança/cartório | Não há outra escrita de `Ausencia.status` nos receivers lidos; eles julgam ou agendam consumidores. (`app/ponto/signals.py:143`, `app/ponto/signals.py:275`) | Para cobrança, o predicado exclui rejeitadas, coerente com o mesmo estado de negócio; nesta fixture o cartório retorna antes desse processamento por não encontrar célula. (`app/ponto/turnos.py:2530`, `app/ponto/turnos.py:2531`; `app/ponto/services/cartorio.py:2027`, `app/ponto/services/cartorio.py:2029`) |

Limite do teste novo: zero rejeitadas e contagem igual de auditorias não provam, isoladamente, identidade ou restauração integral dos registros; ele não repete o snapshot do teste anterior. Isso não constitui defeito novo observado no serviço. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:278`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:280`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:238`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:252`)

## 16/11 08:59 · sessao 2244d8af6ed9 · - · - · origem=Ronald (tmux)

Os comentários agora concordam com o código: o teto é `max(hoje, fim da última competência com derivado_em não nulo)`; `_acordar_projecao` usa esse teto para consultar `dias_da_ausencia` e invalida os trechos resultantes. Edição e decisão chamam o mesmo acordador. (`app/ponto/services/ausencia.py:147`; `app/ponto/services/ausencia.py:151`; `app/ponto/services/ausencia.py:190`; `app/ponto/services/ausencia.py:195`; `app/ponto/services/ausencia.py:669`; `app/ponto/services/ausencia.py:1123`)

Não encontrei comentário contrário nos arquivos Python pesquisados de `app/ponto`. As ocorrências que poderiam confundir estão qualificadas: “competência futura não tem projeção” aparece para dizer que isso **não é garantido**; “fim vazio = HOJE” é identificado como fórmula **anterior**. Censo universal de qualquer documento da obra: `[nao verificado]`. (`app/ponto/services/ausencia.py:170`; `app/ponto/services/ausencia.py:668`)

Os descartes da v2 sustentam-se com limites:

- **Auditoria antes do save, mantendo atomic:** equivalência aceitável para o contrato persistido “sem trilha não fica rejeição”. A trilha e o save ficam no mesmo bloco, e o `None` provoca saída por exceção; isso não demonstra equivalência de toda ordem observável dos sinais. (`app/ponto/services/ausencia.py:1484`; `app/ponto/services/ausencia.py:1498`; `app/ponto/services/ausencia.py:1499`; `app/ponto/services/ausencia.py:1506`)
- **`julgada_em = gerada_em`:** descartar como escopo é decisão justificável, mas “mutante implausível” não elimina a lacuna temporal. O relógio é congelado, a célula é criada antes de `instante` e ambos podem representar o mesmo instante. O espião prova retorno do juiz real com contador positivo por dia; não prova a correção temporal isoladamente. A implementação real escreve `agora`. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:60`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:481`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:485`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:490`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:497`; `app/ponto/portas/celula.py:1194`; `app/escala/models.py:1702`)
- **Campos omitidos:** a rejeição persiste explicitamente somente `status` e `atualizado_em`; o cancelamento chama essa porta. O override de `save` pode ajustar atributos em memória, mas recebe aquele `update_fields` e o repassa ao `super().save`. Portanto, o motivo de escopo se sustenta para o escritor atual; não constitui teste de preservação dos campos omitidos. (`app/ponto/services/ausencia.py:1498`; `app/ferias/services.py:683`; `app/ponto/models.py:911`; `app/ponto/models.py:917`; `app/ponto/models.py:918`)
- **Trilha nova permitida:** contrato e implementação concordam, mas aceitam qualquer inserção nova, sem verificar se é recusa legítima. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:545`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:574`)
- **Adesão/veredito/ata:** continuam limites reais: a fixture assume o ramo `nunca_bateu`, que retorna antes do restante; os asserts verificam contador, status, timestamp e ausência, sem comparar ata/veredito. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:487`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:493`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:499`; `app/ponto/services/cartorio.py:1318`)
- **Relógio/D/invalidação:** mecanismos continuam presentes: leitura interna de relógio, wrapper `engolir` sem atomic próprio, falha de invalidação devolvendo zero e insumos iguais pulando nova invalidação. Historicidade e fila efetiva: `[nao verificado]`. (`app/ponto/services/ausencia.py:184`; `app/ponto/services/ausencia.py:191`; `app/ponto/signals.py:88`; `app/core/observ.py:86`; `app/ponto/services/fechamento.py:1661`)
- **Selo/maps vazios:** o selo exige chamada viva; o positivo exige que o fechamento perca o carimbo, portanto impede acordador globalmente inoperante. Execuções relatadas: `[nao verificado]`. (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:222`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:224`)
- **`obs` como metadado:** a interpretação por substrings está presente; anterioridade histórica e gestão em fila: `[nao verificado]`. (`app/ferias/management/commands/listar_ferias_dp.py:27`; `app/ferias/management/commands/listar_ferias_dp.py:30`)
- **Histórico/base:** trecho atual é compatível com remoção de J1, pois passa do save da Justificativa para 5b. Igualdade byte a byte com base e logs executados: `[nao verificado]`. (`app/ponto/services/regularizacao.py:106`; `app/ponto/services/regularizacao.py:108`)

Não identifiquei defeito novo no delta lido: o novo negativo observa duas tentativas de trilha, nenhuma rejeição persistida, contagem de trilhas restaurada e exceção; os demais trechos mudam documentação/mensagem para o que os asserts efetivamente verificam. Isso é conclusão estática; execução `[nao verificado]`. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:277`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:280`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:281`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:250`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:498`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:542`)

## 16/11 08:56 · sessao 452fd522f259 · - · - · origem=Ronald (tmux)

Revisão: **NAO PROCEDE quanto ao wrapper/fallback.** O item 9 diz “fora da vaga”, e prevê explicitamente entrar atrás da espinha caso o wrapper exija essa vaga (`cortes_f2_f9_1611.txt:10`).

Não há flag para escrita de RED fora do papel construtor: o parser só oferece `--sonda`, `--anexo` e `--construtor`, e o cabeçalho exclui o RED excepcional desse wrapper (`bin/codex_pergunta.sh:30-33`; `bin/codex_pergunta.sh:58-66`). A via existente é `CODEX_PAPEL=construtor` com `--construtor <wt>`, obrigatoriamente com `logs/construtor.vaga`; isso se encaixa no fallback escrito (`bin/codex_pergunta.sh:197-200`; `bin/codex_pergunta.sh:248-253`; `bin/codex_pergunta.sh:418-426`).

Também não demonstraria impossibilidade só porque a entrega fica em `logs/`: o wrapper admite o arquivo de resposta sob `logs/`, e o Code pode registrar a entrega; o item não atribui explicitamente essa gravação final ao sandbox do Codex (`bin/codex_pergunta.sh:205-210`; `bin/codex_pergunta.sh:422`; `bin/codex_pergunta.sh:430`; `cortes_f2_f9_1611.txt:10`). A escrita direta de `logs/.../reds.md` pelo Codex construtor seria proibida por estar fora de `app/` (`bin/codex_pergunta.sh:134`; `bin/codex_pergunta.sh:345-346`).

O contrato específico de RED desta obra permanece **[nao verificado]**; a exigência geral existe, mas sua ausência no dado não prova inexistência nem exige criar um novo modo (`CLAUDE.md:349-350`; `app/docs/PISTA.md:143-145`).

## 16/11 08:55 · sessao 30eec1e06ad0 · LOTE3-C7 · verificador · origem=Code

**O delta fecha o mutante da volta 2. Não identifiquei sobrevivente plausível na família pedida para a falha injetada; a implementação atual mantém porta e trilha no mesmo `atomic`, com o `except` fora.** (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:246-264`; `app/colaboradores/views.py:333-343`). Execução dos mutantes: **[nao verificado]**.

1. **Poder de distinção do teste** [nao verificado]

| Mutante | Por que o teste o rejeita, por análise estática | [nao verificado]
|---|---|
| Salvar → trilha, sem atomic comum; atomic só na porta ou só na trilha | A porta deixa o posto novo gravado; a consulta final exige o antigo. (`app/colaboradores/services/colaborador.py:158-161`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:256-257`). |
| Trilha → porta, inclusive o m3 com atomic somente na trilha | A leitura durante a trilha encontra o posto antigo; a nova asserção exige o novo. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:247-253`). |
| Atomic envolvendo o `try/except`, capturando a falha internamente sem provocar rollback | O `RuntimeError` ocorre depois do INSERT real; capturá-lo sem desfazer as escritas deixa FK/contagem incompatíveis com as verificações finais. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:248-259`). |
| Atomic somente no tratamento do erro | Não desfaz as escritas anteriores; falham FK ou contagem finais. (`app/colaboradores/services/colaborador.py:158-161`; `app/colaboradores/services/historico_vinculo.py:13-17`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:256-259`). |

Isso não constitui prova universal da estrutura: o snapshot exige o **efeito da gravação**, sem verificar diretamente a chamada de `salvar_inteiro`. Substituí-la por `colaborador.save()` dentro do atomic comum preservaria os efeitos deste teste, removendo a guarda; a recusa é exercitada pelo teste separado. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:211-228`, `:246-264`; `app/colaboradores/services/colaborador.py:158-160`).

2. **Leitura dentro do substituto**

No caminho mostrado, ela acrescenta uma consulta e guarda um inteiro numa lista; não altera o objeto usado pela view nem executa gravação ou comando de rollback. A escrita real e o `RuntimeError` continuam depois dessa consulta. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:246-249`).

**Não há verde por uma asserção engolida:** as asserções ficam depois do POST. Se o `get` falhar, a chamada real não chega ao `append`, e `len(escritas) == 1` falha; se a trilha retornar `None`, outra asserção falha; o toast também exige exatamente a exceção planejada. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:247-264`).

A leitura observa a gravação visível nessa conexão, sem provar commit. O `TestCase` envolvente também não oculta escritas próprias que permaneceram após o POST, pois FK e contagem são consultadas antes do término do teste. (`app/colaboradores/tests/test_lote3_desligado_sem_porta.py:43`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:253-259`). Outra conexão, concorrência e efeitos de timing: **[nao verificado]**.

3. **Triagem e pouso** [nao verificado]

A correção sobre a releitura procede: o gancho altera somente situação/data/motivo, e o delta observa o posto durante a trilha. Esses testes continuam compatíveis com restaurar `_posto_antes` em memória no tratamento do erro; não distinguem essa restauração da releitura atual. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:40-43`, `:216-228`, `:247-253`; `app/colaboradores/views.py:343`).

Os demais itens têm fundamento no código:

| Item mantido em fila | Evidência |
|---|---|
| Homônimos podem trocar FK sem trilha | Comparação textual e representação por nome/empresa. (`app/colaboradores/services/historico_vinculo.py:10-11`; `app/colaboradores/models.py:276-277`). |
| Anterior potencialmente obsoleto | Capturado antes da trava; a guarda relê situação. (`app/colaboradores/views.py:326-334`; `app/colaboradores/services/colaborador.py:65-66`, `:89-91`). |
| `sem_posto` repetido | Dois ramos, painel e pendência registrada. (`app/colaboradores/views.py:346`, `:365`; `app/templates/colaboradores/partials/painel_vinculo.html:34`; `app/core/juizes.py:931-932`). |
| Datas com sentidos distintos | Dia atual, início do vínculo e demissão agendada. (`app/colaboradores/views.py:335`; `app/colaboradores/services/vinculo.py:354`; `app/colaboradores/services/desligamento.py:113-116`). |
| Trava adicional por matrícula ausente | Ramo condicional do `save`. (`app/colaboradores/models.py:427-438`). |

**Para o contrato “falha excepcional da trilha desfaz posto e trilha”, nenhum desses itens demonstra bloqueio funcional do pouso atual.** (`app/colaboradores/views.py:333-343`). O aval integral está apenas referenciado no código; conformidade com exigências além do escopo descrito no anexo, preexistência dos passivos e resultados alegados dos logs: **[nao verificado]**. (`app/colaboradores/views.py:331`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:231`).

## 16/11 08:55 · sessao 7519f15333fc · - · - · origem=Ronald (tmux)

Contando somente a marca `[DEPENDE-F1]` explícita na linha de cada degrau:

| Matriz | PROVA=NENHUM | Com marca | Sem marca |
|---|---:|---:|---:|
| F2, integrais na coluna c3 | 10 | 0 | **10** |
| F2, incluindo parciais | 14 | 0 | **14** |
| F4 | 21 | 3 | **18** |
| F6 | 11 | 8 | **3** |

Fontes: anexo F2/matriz.md:100-102; F4/matriz.md:126 e :130; F6/matriz.md:94 e respectivas linhas dos degraus abaixo. Numeração começa no primeiro `#` de cada anexo e inclui linhas vazias.

- **F2 integrais:** M1b (:37), M4 (:40), M7 (:43), M11 (:47), M13 (:49), M14 (:50), M15 (:51), M17 (:53), M18 (:54), M19 (:55). Parciais adicionais: M3 (:39), M9 (:45), M10 (:46), M20 (:56). A seção final distingue explicitamente “nenhuma mordida” de “algum ramo sem mordida” (:101); a coluna BASE é outro universo (:102).
- **F4 sem marca:** M1, M2, M13, M16, M17, M19, M23, M24; E4, E5, E7, E17, E18, E19, E20; X1, X2; J2. Os três NENHUM marcados são M9b (:42), E9 (:71) e J1 (:90). A lista completa dos21 aparece em :126.
- **F6 sem marca:** U9b (:44), S1 (:48), S2 (:49). S1/S2 trazem “SOME do escopo F6”; excluí-los daria **1**, mas o item9 não determina essa exclusão. A lista declarada dos11 inclui ambos (:94).

Ausência de marca na linha **não demonstra independência**: F2 tem dependências numa seção separada (:69-77), abrangendo M1-M4, M8-M10 e N1/M7; F4 marca dependência no contrato (:104-109), inclusive para M1/M2/M13/M16/M17 e M23, que não receberam a marca na própria linha.

Portanto, a seleção literal do item9 precisa distinguir coluna BASE/c3, integrais/parciais, dependência da linha/dependência do contrato e os degraus F6 excluídos do escopo. As matrizes fornecem as categorias, mas não uma seleção única inequívoca para “antes do pouso da F1”. (Anexo F2/matriz.md:69, :99-102; F4/matriz.md:104-109; F6/matriz.md:48-49, :94.)

## 16/11 08:54 · sessao 4d65f7eb3223 · - · - · origem=Ronald (tmux)

Não encontrei sobrevivente funcional plausível na família restrita solicitada, por análise estática: o teste exige posto novo durante a trilha, escrita real e, após a exceção, posto antigo, contagem original, HTML antigo e toast exato. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:246-264`).

| Mutante | Por que falha |
|---|---|
| Salvar → trilha, sem atomic comum; atomic só na porta; atomic só na trilha após porta | A porta encerra seu bloco próprio e deixa o posto novo; a consulta final exige o antigo. (`app/colaboradores/services/colaborador.py:158-161`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:256-257`). |
| Trilha → porta, com ou sem atomic somente na trilha | A leitura durante a trilha encontra o antigo; o teste exige o novo. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:247`, `:253`). |
| Capturar o `RuntimeError` dentro do único atomic sem marcar rollback | As duas escritas permanecem; as consultas finais exigem desfazimento de ambas. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:248-249`, `:256-259`). |
| Atomic envolvendo somente o tratamento do erro | As escritas anteriores ficam preservadas e violam FK/contagem finais. (`app/colaboradores/services/colaborador.py:158-161`; `app/colaboradores/services/historico_vinculo.py:13-17`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:256-259`). |

A implementação atual confirma atomic comum, porta antes da trilha e `except` fora do bloco. (`app/colaboradores/views.py:333-343`). Execução desses mutantes: **[nao verificado]**.

Limite: este teste sozinho não comprova passagem por `salvar_inteiro`; não há spy nessa função. Substituí-la por `colaborador.save()` dentro do mesmo atomic preservaria os efeitos examinados, mas removeria a guarda. A guarda é exercitada pelo teste separado de desligamento durante a carga. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:211-228`, `:246-264`; `app/colaboradores/services/colaborador.py:158-160`). Esse bypass é um mutante fora da família estrita perguntada. (`app/colaboradores/views.py:334`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:250-264`).

## 16/11 08:54 · sessao eb897fb79a41 · - · - · origem=Ronald (tmux)

Parecer sobre a pergunta 3: **não há colisão substantiva demonstrada pelo material disponível**; a lacuna operacional do wrapper deve ser tratada na pergunta 1. A lei permite expressamente testes RED em cópia própria fora do papel construtor, sob contrato e revisão (`CLAUDE.md:349-350`; `app/docs/PISTA.md:143-145`).

- O critério de colisão é **arquivo**, inclusive arquivo de teste; o item 9 já manda esperar quando cruzar F1/R3/BUG-PAR (`CLAUDE.md:309-310`; anexo `cortes_f2_f9_1611.txt`, item 9). [nao verificado]
- O registro disponível confirma cruzamentos da BUG-PAR com F1 em `fio.py`, `test_s139.py`, `test_medida_turno_do_par.py` e `JUIZES.md`; a construção da volta aguardava o pouso da B1 (`app/docs/RELATO.md:14`, `:25`, `:33`). O anexo COMPLEMENTO-2 preserva essa espera e autoriza apenas ensaio de leitura.
- **Não certificar ausência de cruzamento com LOTE7 c7/LOTE3 c7**: o registro da cópia descreve c6 e seus arquivos, enquanto o usuário informa c7; os deltas atuais das cópias não foram fornecidos (`app/docs/RELATO.md:4`, `:7`, `:12-13`; estado c7 **[nao verificado]**).
- A lei vigente **L-130 substituiu JUIZ-RESPONDIDA e F5/F6(2)**: “respondida” exige instante **e** texto; pares parciais são ilegais (`app/docs/LEIS.md:151`). Portanto, matrizes históricas com P5/texto sem instante precisam ser lidas à luz dessa lei. Não é prova de colisão de um teste ainda não escrito: um caso ilegal pode ser entrada válida para testar a recusa.
- Há uma precondição normativa ausente do item 9: RED em cópia própria exige contrato **antes do primeiro uso**, com cinco linhas e revisão do instrumento (`app/docs/PISTA.md:144-145`, `:170-175`; `CLAUDE.md:394`). Existência de contrato anterior: **[nao verificado]**.

Não atribuir ao veto antigo das matrizes uma colisão automática: o novo aval autoriza especificamente construir **só testes**, preservando a espera por dependências e cruzamentos (anexo `cortes_f2_f9_1611.txt`, item 9). [nao verificado]

## 16/11 08:54 · sessao 8cece979ddda · LOTE7-C7 · verificador · origem=Code

**O c7b fecha a preservação dos campos acrescentados, mas não demonstra rollback após tentativa de escrita nem frescor temporal “neste ato”.** O descarte do mutante que levanta antes de escrever precisa ser restringido; o teste positivo não elimina sua variante condicional. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:252`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:243`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:470`)

Resultados executados de testes/mutantes, logs, igualdade com árvores anteriores e estado de produção: `[nao verificado]`. As conclusões abaixo são estáticas, sobre a cópia e o delta fornecido. [nao verificado]

Nos achados marcados **PROCEDENTES**, o fechamento é o seguinte:

| Achado da v1 | O delta fecha? |
|---|---|
| Item 1: apagar ausências ou alterar outros campos e levantar a exceção esperada | **Sim, para o conjunto e os campos enumerados.** O teste fotografa PK, tipo, status, data, fim, minutos e descrição antes da chamada e exige igualdade depois. “Linha INTEIRA” continua excessivo: ficam fora, por exemplo, `dias_corridos`, `hora_ini` e `hora_fim`. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:238`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:252`; `app/ponto/models.py:761`; `app/ponto/models.py:768`; `app/ponto/models.py:770`) |
| Item 4: campos omitidos em `_retrato` | **Sim, para os campos acrescentados.** Agendamento passa a incluir início/fim/dias; ausências incluem tipo/fim/minutos/descrição; trilhas antigas incluem descrição. A comparação continua limitada às PKs antigas da auditoria. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:520`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:523`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:546`) |
| Item 4: timestamp incorreto, porém não nulo | **Parcialmente.** O filtro agora rejeita valores anteriores a `instante`, mas o relógio congelado permite usar o instante da criação da célula e ainda passar; detalhes abaixo. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:60`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:454`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:463`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:470`) |
| Item 7: comentário sobre competência futura | **Fecha o trecho alterado.** O comentário passa a concordar com o teto que considera a última competência carimbada. Entretanto, permanece a afirmação contrária no comentário da edição. (`app/ponto/services/ausencia.py:1119`; `app/ponto/services/ausencia.py:147`; `app/ponto/services/ausencia.py:151`; `app/ponto/services/ausencia.py:665`) |
| Item 3: “regularização, só leitura” | A formulação corrigida no anexo é compatível se qualificar somente o acréscimo dos ganchos. O serviço grava Justificativa e pode criar ausências; não é integralmente de leitura. (`app/ponto/services/regularizacao.py:106`; `app/ponto/services/regularizacao.py:118`; `app/ponto/services/ausencia.py:597`) |

**Mutante sobrevivente principal:** mover `_ev_rej(...)` para antes de `a.save(...)`, mantendo a exceção quando a auditoria retorna `None`. No sucesso, continua salvando as cinco rejeições e criando as trilhas; na falha, levanta sem tentar salvar a ausência. Os negativos fornecidos fazem a primeira auditoria de rejeição falhar e não exigem tentativa nem ordem de escrita. Portanto, o positivo e os testes alterados não distinguem esse mutante do código atual. Sobrevivência inferida; execução `[nao verificado]`. (`app/ponto/services/ausencia.py:1498`; `app/ponto/services/ausencia.py:1499`; `app/ponto/services/ausencia.py:1506`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:216`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:243`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:531`)

Uma variante mais grave também fica sem testemunha nesses testes: inverter auditoria/save **e retirar o `atomic` local**. A falha na segunda auditoria poderia deixar a primeira rejeição persistida numa chamada direta, enquanto os negativos atuais falham na primeira. O cancelamento ainda teria sua transação externa. Sobrevivência inferida; execução `[nao verificado]`. (`app/ponto/services/ausencia.py:1484`; `app/ponto/services/ausencia.py:1494`; `app/ferias/services.py:674`; `app/ferias/services.py:683`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:243`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:532`)

Para os **DESCARTADOS**, os motivos se sustentam nestes limites:

| Descarte | Verificação |
|---|---|
| “Levantar antes de escrever é morto pelo positivo” | **Somente para `raise` incondicional.** O mutante condicional acima sobrevive por leitura. Além disso, o positivo citado exige retorno `5` e cinco trilhas, mas não conta diretamente cinco ausências rejeitadas; essa contagem está no teste da célula. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:217`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:219`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:468`) |
| Trilha nova indevida permitida deliberadamente | **Coerente com o contrato escrito**, que preserva apenas as antigas. Contudo, o teste permite qualquer inserção nova, sem verificar que represente recusa legítima. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:517`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:546`) |
| Ramo com adesão fora do escopo | É uma delimitação de escopo; a lacuna permanece. A fixture percorre `nunca_bateu`, cujo ramo retorna antes do processamento normal. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:460`; `app/ponto/services/cartorio.py:1317`; `app/ponto/services/cartorio.py:1318`) |
| Veredito/ata fora do escopo | Também delimita escopo, sem demonstrar esses conteúdos. Os asserts verificam atividade, status, carimbo e FK, não veredito ou ata. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:466`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:470`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:472`) |
| Relógio, D e falha de invalidação sem retentativa estão em fila | Os mecanismos continuam presentes: leitura interna de relógio, julgamento sob `engolir` sem savepoint nesse wrapper, invalidação que retorna zero após erro e repetição que ignora insumos iguais. Antiguidade histórica e gestão efetiva da fila: `[nao verificado]`. (`app/ponto/services/ausencia.py:184`; `app/ponto/services/ausencia.py:191`; `app/ponto/signals.py:88`; `app/core/observ.py:86`; `app/ponto/services/fechamento.py:1661`) |
| Selo é apenas piso; testes comportamentais cobrem mapas vazios | **Sustenta-se para silenciar globalmente a invalidação.** O positivo exige fechamento invalidado. Isso não prova cobertura de todos os ramos; o selo verifica presença de chamada viva, sem validar seus argumentos. Os “18 FAIL” executados: `[nao verificado]`. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:224`; `app/ponto/tests/test_lote7_selo_escritor_acorda.py:222`) |
| `obs` interpretado como metadado é dívida anterior | A interpretação por substrings existe. Sua antiguidade e tratamento em fila: `[nao verificado]`. (`app/ferias/management/commands/listar_ferias_dp.py:27`; `app/ferias/management/commands/listar_ferias_dp.py:28`; `app/ferias/management/commands/listar_ferias_dp.py:30`) |
| Histórico/base/logs foram conferidos por outro verificador | Isso não substitui verificação nesta cópia. Igualdade byte a byte com a base e execuções relatadas: `[nao verificado]`. A ausência do bloco J1 é compatível com a passagem direta da atualização da Justificativa para o passo 5b. (`app/ponto/services/regularizacao.py:102`; `app/ponto/services/regularizacao.py:108`) |

Quanto às armadilhas novas do delta:

- **Não identifiquei asserção universalmente incapaz de falhar.** Em particular, o filtro por timestamp falha se `julgada_em` continuar nulo: a fixture não o preenche e o modelo permite `NULL`. Entretanto, o acréscimo temporal não prova avanço entre atos. Sob o congelamento, um mutante que grave `cel.julgada_em = cel.gerada_em`, conservando os demais efeitos, satisfaz `>= instante`. Sobrevivência inferida; execução `[nao verificado]`. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:60`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:454`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:470`; `app/escala/models.py:1702`; `app/escala/models.py:1745`; `app/ponto/portas/celula.py:1194`)
- **Não há dependência de ordem indeterminada nas comparações acrescentadas.** Ausências são ordenadas por tuplas começando pela PK; auditorias são dicionários por PK; células usam contagem ou comparação com vazio. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:239`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:470`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:472`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:523`)
- **Todos os campos acrescentados existem.** (`app/ponto/models.py:749`; `app/ponto/models.py:751`; `app/ponto/models.py:755`; `app/ponto/models.py:776`; `app/ferias/models.py:312`; `app/ferias/models.py:314`; `app/core/models.py:299`; `app/escala/models.py:1745`)
- **Há imprecisão documental:** o novo comentário “linha INTEIRA” excede o retrato implementado; a docstring de `_retrato` continua enumerando os campos antigos. O comentário corrigido sobre futuro está correto, mas permanece outro contraditório em `editar_ausencia_registro`. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:250`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:515`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:520`; `app/ponto/services/ausencia.py:1119`; `app/ponto/services/ausencia.py:665`; `app/ponto/services/ausencia.py:669`)

O mapa de escritores/leitores abaixo cobre os caminhos conferidos; censo exaustivo do repositório: `[nao verificado]`.

| Campo | Quem escreve | Quem lê e concordância com o teste | [nao verificado]
|---|---|---|
| `Ausencia.tipo` | Criação, edição e espelho de férias. (`app/ponto/services/ausencia.py:484`; `app/ponto/services/ausencia.py:648`; `app/ponto/services/ausencia.py:1390`) | O insumo da folha inclui tipo; o juiz o usa para interpretar ausência sem fim. A igualdade protege a classificação anterior, sem validar sua correção. (`app/ponto/services/ausencia.py:127`; `app/ponto/turnos.py:2805`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:522`) |
| `Ausencia.data_fim` | Criação recebe; edição deriva/atribui; `save` pode derivar de `dias_corridos`. (`app/ponto/services/ausencia.py:485`; `app/ponto/services/ausencia.py:627`; `app/ponto/services/ausencia.py:650`; `app/ponto/models.py:917`) | O juiz interpreta fim inclusivo; `NULL` prolonga somente tipos em aberto, senão representa dia único. O snapshot preserva o valor sem impor interpretação divergente. (`app/ponto/turnos.py:2798`; `app/ponto/turnos.py:2803`; `app/ponto/turnos.py:2808`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:522`) |
| `Ausencia.minutos` | Criação/edição; `save` deriva falta aprovada sem minutos; férias fornece zero. (`app/ponto/services/ausencia.py:485`; `app/ponto/services/ausencia.py:658`; `app/ponto/models.py:887`; `app/ponto/models.py:897`; `app/ponto/services/ausencia.py:1391`) | Insumo da folha e propriedade `horas` leem duração. O teste exige preservação; zero em férias não significa inexistência da cobertura, que é criada por dia. (`app/ponto/services/ausencia.py:127`; `app/ponto/models.py:876`; `app/ponto/services/ausencia.py:1390`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:522`) |
| `Ausencia.descricao` | Criação, edição, reabertura e espelho; este grava `Ferias agendamento #N`. (`app/ponto/services/ausencia.py:486`; `app/ponto/services/ausencia.py:653`; `app/ponto/services/ausencia.py:1316`; `app/ponto/services/ausencia.py:1393`) | Painel exibe; backfill interpreta a marca `#N`. Preservar a descrição protege também esse metadado. (`app/templates/ponto/partials/painel_ausencia.html:56`; `app/ponto/services/ausencia.py:1552`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:522`) |
| `AgendamentoFerias.data_inicio` | Criação e troca de janela. (`app/ferias/services.py:582`; `app/ferias/services.py:716`; `app/ferias/services.py:729`) | O espelho inicia a enumeração nessa data; snapshot exige a mesma janela após recusa. Significado concordante. (`app/ponto/services/ausencia.py:1380`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:520`) |
| `AgendamentoFerias.data_fim` | Criação calcula fim; troca de janela atribui/persiste. (`app/ferias/services.py:565`; `app/ferias/services.py:584`; `app/ferias/services.py:716`; `app/ferias/services.py:729`) | O espelho inclui o último dia. A comparação preserva esse limite inclusivo. (`app/ponto/services/ausencia.py:1381`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:520`) |
| `AgendamentoFerias.dias` | Criação e troca de janela. (`app/ferias/services.py:584`; `app/ferias/services.py:716`) | Modelo confere quantidade inclusiva; cancelamento subtrai `vivo.dias` dos concedidos. O teste preserva quantidade e janela, sem recalculá-las. (`app/ferias/models.py:382`; `app/ferias/services.py:680`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:520`) |
| `LogAuditoria.descricao` | `evento` persiste o argumento; rejeição fornece dia/agendamento. (`app/core/observ.py:136`; `app/ponto/services/ausencia.py:1500`) | Busca e tela leem texto. O teste preserva descrições antigas; não valida a descrição das trilhas novas. (`app/core/views.py:714`; `app/templates/core/log_auditoria.html:73`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:523`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:546`) |
| `CelulaDia.julgada_em` | `carimbar` grava `agora`, salvo retorno por idempotência; desfazer julgamento futuro pode limpar. (`app/ponto/portas/celula.py:1190`; `app/ponto/portas/celula.py:1194`; `app/ponto/portas/celula.py:1165`) | Cartório combina carimbo, impressão e veredito para frescor. O delta usa apenas o limite temporal para afirmar “neste ato”; sob congelamento, essa afirmação é mais forte que a evidência. (`app/ponto/services/cartorio.py:1260`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:463`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:470`) |

## 16/11 08:54 · sessao 6c1c5237105b · - · - · origem=Ronald (tmux)

A triagem atualizada procede no limite do contrato mostrado no código: `salvar_inteiro` e `registrar_mudanca` estão no mesmo `atomic`, e o `except` fica depois da saída desse bloco. Nenhum dos passivos abaixo demonstra quebra desse rollback excepcional na implementação atual. (`app/colaboradores/views.py:331-343`)

| Item | Conferência |
|---|---|
| Releitura sem teste que a distinga da restauração em memória | A correção da entrega é adequada: o gancho de desligamento só altera situação/data/motivo, preservando posto; suas asserções continuam compatíveis com restaurar `_posto_antes`. A leitura nova ocorre durante a trilha e não testa a origem do objeto usado no `except`. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:40-43`, `:216-228`, `:247-253`; `app/colaboradores/views.py:343`) |
| Homônimos | `de == para` retorna sem histórico; a view passa representações textuais, e `Posto.__str__` contém nome e empresa. Dois postos com essa mesma representação podem trocar FK sem trilha, por inferência estática. (`app/colaboradores/services/historico_vinculo.py:10-17`; `app/colaboradores/views.py:326-336`; `app/colaboradores/models.py:276-277`) |
| Anterior potencialmente obsoleto | `_posto_antes` é obtido antes da trava da porta; a guarda trava e relê situação, sem atualizar esse anterior. Corrida real: **[nao verificado]**. (`app/colaboradores/views.py:326-334`; `app/colaboradores/services/colaborador.py:65-66`, `:89-91`, `:158-160`) |
| `sem_posto` repetido | Está nos dois ramos e no painel, com pendência explícita registrada. (`app/colaboradores/views.py:346`, `:365`; `app/templates/colaboradores/partials/painel_vinculo.html:34`; `app/core/juizes.py:931-932`) |
| Datas distintas | A view usa dia atual; vínculo usa `data_inicio`; desligamento agendado usa `data_demissao`. (`app/colaboradores/views.py:335`; `app/colaboradores/services/vinculo.py:354`; `app/colaboradores/services/desligamento.py:107-116`) |
| Trava adicional na matrícula | O `save` pode travar a linha da maior matrícula quando a matrícula estiver ausente. (`app/colaboradores/models.py:427-438`) |

O aval integral `RESPOSTAS-DA-MANHA-1611` não foi encontrado na cópia; o código apenas o referencia. Portanto, dizer que esses itens estão “fora do aval literal” depende do resumo da entrega; conformidade com o aval integral e autorização final de pouso: **[nao verificado]**. (`app/colaboradores/views.py:331`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:231`)

Preexistência/ancestralidade dos passivos e resultados alegados de testes, mutantes, ruff e logs: **[nao verificado]**; a cópia não oferece metadados Git utilizáveis, e nenhum teste foi executado. **[nao verificado]** [nao verificado]

## 16/11 08:54 · sessao 452fd522f259 · - · - · origem=Ronald (tmux)

**1) PROCEDE.** A escrita excepcional de RED em cópia própria existe na regra, sob CONTRATO-DE-INSTRUMENTO e revisão pelo Code (`CLAUDE.md:349-350`; `app/docs/PISTA.md:143-145`). Entretanto, **não existe modo/flag do wrapper que a execute fora do papel construtor**: parser aceita apenas `--sonda`, `--anexo`, `--construtor` (`bin/codex_pergunta.sh:58-66`), e o próprio cabeçalho declara que o RED excepcional “não passa por aqui” (`bin/codex_pergunta.sh:30-33`).

O único caminho com escrita é `CODEX_PAPEL=construtor` combinado com `--construtor <wt>`; combinações divergentes são recusadas (`bin/codex_pergunta.sh:197-200`). Esse caminho exige `logs/construtor.vaga`, com `flock -n`, e recusa vaga ocupada (`bin/codex_pergunta.sh:248-253`). Os demais papéis recebem ordem explícita de não criar/editar/apagar e executam em sandbox `read-only` (`bin/codex_pergunta.sh:348-350`; `bin/codex_pergunta.sh:424-426`).

Assim, “se o wrapper exigir a vaga, entra atrás da espinha” resolve a espera, mas não torna executável a escrita **fora do papel construtor** prescrita no item 9 (`cortes_f2_f9_1611.txt:10`, numerando o anexo a partir de “aval Ronald”).

Se houver criação/adaptação de instrumento para suprir esse caminho, o contrato de cinco linhas e sua revisão precisam anteceder o código; o contrato específico desta obra não foi fornecido **[nao verificado]** (`CLAUDE.md:394`; `app/docs/PISTA.md:170-175`).

## 16/11 08:52 · sessao 0bf595a634d6 · - · - · origem=Ronald (tmux)

Mapa limitado aos caminhos de criação/edição, espelho/cancelamento e consumidores conferidos; censo exaustivo do repositório `[nao verificado]`.

| Campo | Escritores conferidos | Leitores e significado; concordância do teste | [nao verificado]
|---|---|---|
| `Ausencia.tipo` | `criar_ausencia` prepara tipo; edição troca; espelho escreve `'ferias'`. (`app/ponto/services/ausencia.py:484`; `app/ponto/services/ausencia.py:648`; `app/ponto/services/ausencia.py:1390`) | `_insumo_da_folha` fotografa tipo; juiz usa tipo para decidir extensão sem fim; folha usa tipos pagos. Comparar igualdade antes/depois protege a classificação existente, sem provar que ela era correta. (`app/ponto/services/ausencia.py:127`; `app/ponto/services/ausencia.py:139`; `app/ponto/turnos.py:2805`; `app/ponto/services/fechamento.py:694`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:522`) |
| `Ausencia.data_fim` | Criação recebe; edição deriva e atribui; `Ausencia.save` deriva de `dias_corridos` quando aplicável. (`app/ponto/services/ausencia.py:485`; `app/ponto/services/ausencia.py:627`; `app/ponto/services/ausencia.py:650`; `app/ponto/models.py:912`; `app/ponto/models.py:917`) | Cobertura usa range fechado, ou tipo em aberto até horizonte, ou dia único. Snapshot concorda ao exigir preservação, sem interpretar `NULL` como universalmente aberto. O help text isolado é mais amplo que o juiz real. (`app/ponto/turnos.py:2798`; `app/ponto/turnos.py:2803`; `app/ponto/models.py:753`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:522`) |
| `Ausencia.minutos` | Criação recebe; edição zera/recebe minutos; `save` deriva falta aprovada sem minutos pelo previsto; espelho de férias fornece zero. (`app/ponto/services/ausencia.py:485`; `app/ponto/services/ausencia.py:658`; `app/ponto/models.py:887`; `app/ponto/models.py:895`; `app/ponto/models.py:897`; `app/ponto/services/ausencia.py:1391`) | `_insumo_da_folha` inclui minutos; `horas` divide por 60; export distribui `a.minutos` entre dias. Igualdade no teste significa “não alterar a duração gravada”; zero em férias não significa ausência de cobertura. (`app/ponto/services/ausencia.py:127`; `app/ponto/models.py:876`; `app/folha/export.py:117`; `app/folha/export.py:121`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:522`; `app/ponto/services/ausencia.py:1391`) |
| `Ausencia.descricao` | Criação recebe; edição substitui se não vazia; reabertura concatena; espelho grava `Ferias agendamento #N`. (`app/ponto/services/ausencia.py:486`; `app/ponto/services/ausencia.py:652`; `app/ponto/services/ausencia.py:1316`; `app/ponto/services/ausencia.py:1393`) | Painel exibe texto; backfill também interpreta a marca `#N`. Portanto não é somente texto ornamental; snapshot protege inclusive esse metadado legado. (`app/templates/ponto/partials/painel_ausencia.html:56`; `app/templates/ponto/partials/painel_ausencia.html:57`; `app/ponto/services/ausencia.py:1552`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:522`) |
| `AgendamentoFerias.data_inicio/data_fim/dias` | Agendamento calcula fim inclusivo e grava dias; gozo histórico calcula dias inclusivos; `_trocar_janela` atribui e persiste os três. (`app/ferias/services.py:565`; `app/ferias/services.py:582`; `app/ferias/services.py:584`; `app/ferias/services.py:523`; `app/ferias/services.py:524`; `app/ferias/services.py:716`; `app/ferias/services.py:729`) | Modelo valida `dias == fim-início+1`; espelho enumera datas inclusivas; cancelamento subtrai `vivo.dias` do concedido. Snapshot tem o mesmo significado: preservar janela e quantidade em recusa, sem recalculá-las. (`app/ferias/models.py:382`; `app/ferias/models.py:383`; `app/ponto/services/ausencia.py:1380`; `app/ponto/services/ausencia.py:1381`; `app/ferias/services.py:680`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:520`) |
| `LogAuditoria.descricao` | `core.observ.evento` recebe e cria campo; rejeição fornece texto com dia e agendamento. (`app/core/observ.py:94`; `app/core/observ.py:134`; `app/core/observ.py:136`; `app/ponto/services/ausencia.py:1500`) | Busca filtra descrição e tela exibe; snapshot exige texto antigo idêntico. Não valida conteúdo/descrição das novas trilhas: `_nada_mudou` restringe comparação às PKs anteriores. (`app/core/views.py:714`; `app/templates/core/log_auditoria.html:73`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:523`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:546`) |
| `CelulaDia.julgada_em` | Porta `carimbar` atribui `agora`, salvo quando idempotência evita save; `desfazer_julgamento_da_futura` também pode limpar os campos de julgamento. (`app/ponto/portas/celula.py:1190`; `app/ponto/portas/celula.py:1194`; `app/ponto/portas/celula.py:1201`; `app/ponto/portas/celula.py:1165`) | Cartório exige carimbo + impressão + veredito para frescor; consumidores de parcial usam julgamento anterior. Delta interpreta como carimbo recente, mas `>= instante` não demonstra avanço entre dois atos sob relógio congelado. (`app/ponto/services/cartorio.py:1260`; `app/ponto/services/cartorio.py:1409`; `app/ponto/services/cartorio.py:417`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:60`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:463`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:470`) |

Campos novos existem, sem ocorrência de campo inexistente nestas comparações. (`app/ponto/models.py:749`; `app/ponto/models.py:751`; `app/ponto/models.py:755`; `app/ponto/models.py:776`; `app/ferias/models.py:312`; `app/ferias/models.py:313`; `app/ferias/models.py:314`; `app/core/models.py:299`; `app/escala/models.py:1745`)

Não há comparação nova dependente de ordem indefinida: ausências são `sorted` por tuplas cuja primeira coluna é PK única; trilhas são dicionário por PK; a consulta residual de células é comparada ao vazio. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:239`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:240`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:521`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:523`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:472`)

O comentário corrigido em `:1119` passa a concordar com `_teto_da_linha_aberta`, mas ficou comentário contraditório em `editar_ausencia_registro`: ainda diz que futuras não são alcançadas, apesar de chamar o mesmo acordador que calcula teto das competências carimbadas. Isso é remanescente, não nova mudança de código demonstrada pelo patch. (`app/ponto/services/ausencia.py:1119`; `app/ponto/services/ausencia.py:147`; `app/ponto/services/ausencia.py:151`; `app/ponto/services/ausencia.py:665`; `app/ponto/services/ausencia.py:666`; `app/ponto/services/ausencia.py:669`; `app/ponto/services/ausencia.py:190`; `app/ponto/services/ausencia.py:191`)

A docstring de `_retrato` ficou desatualizada: enumera somente status/obs e pk/status/data, enquanto código compara também os campos acrescentados. “INTEIRO” continua sendo somente o retrato enumerado, não linha completa. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:515`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:516`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:520`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:524`)

## 16/11 08:52 · sessao f5d22736a7d3 · - · - · origem=Ronald (tmux)

Auditoria estática, sem execução de testes ou mutação; resultados executados `[nao verificado]`. [nao verificado]

- **`julgada_em >= instante` tem mordida, mas não prova frescor temporal adicional.** A fixture cria células sem `julgada_em`, cujo campo permite NULL, então omitir o carimbo faz a contagem falhar. Entretanto, o relógio congelado faz `gerada_em` e `instante` coincidirem: um mutante de `carimbar` que grave `cel.julgada_em = cel.gerada_em`, mantendo os demais efeitos, passa. A asserção não distingue julgamento atual de um carimbo anterior no mesmo instante congelado. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:56`, `:60`, `:191`, `:439`, `:454`, `:463`, `:470`; `app/escala/models.py:1702`, `:1745`; `app/ponto/portas/celula.py:1194`)

- **Não identifiquei dependência de ordem indeterminada nas comparações novas.** Ausências usam `sorted` com PK como primeiro componente; trilhas são dicionários indexados por PK; carimbo usa contagem. A comparação com lista vazia independe de ordenação. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:239`, `:470`, `:472`, `:521`, `:523`)

- **Os campos novos existem.** Ausência: `tipo`, `data_fim`, `minutos`, `descricao`; agendamento: início, fim, dias; auditoria: descrição; célula: julgamento. (`app/ponto/models.py:749`, `:751`, `:755`, `:776`; `app/ferias/models.py:312`, `:313`, `:314`; `app/core/models.py:299`; `app/escala/models.py:1745`)

- **O comentário de produção corrigido concorda com o código.** O teto é o máximo entre hoje e o fim da última competência carimbada, e o botão aceita mês/ano do POST. (`app/ponto/services/ausencia.py:1119`, `:147`, `:151`, `:190`; `app/ponto/views.py:1240`, `:1241`, `:1255`)

- **“Linha INTEIRA” continua exagerado.** O retrato ampliado omite `dias_corridos`, `hora_ini`, `hora_fim` e outros campos existentes. O delta fecha os campos enumerados, sem provar preservação integral da linha. A docstring de `_retrato` também continua enumerando o retrato antigo. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:239`, `:250`, `:515`, `:520`; `app/ponto/models.py:761`, `:768`, `:770`)

- **Descartes periféricos:** “trilha nova pode nascer” corresponde expressamente ao contrato implementado, mas o teste permite qualquer inserção nova, sem validar que seja trilha legítima de recusa. Ramo sem adesão retorna antes do normal, portanto a lacuna do ramo com adesão é real; classificá-la fora do escopo não equivale a demonstrar cobertura. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:517`, `:546`, `:460`; `app/ponto/services/cartorio.py:1317`, `:1318`)

- **D/E e relógio permanecem na árvore.** `engolir` não abre savepoint; o signal envolve o julgamento nesse wrapper; o acordador lê `localdate()` para linha mudada sem fim; a invalidação captura exceção e retorna zero; o contador só encontra carimbo NULL. Origem histórica em c6, fila efetiva e providências externas `[nao verificado]`. (`app/core/observ.py:86`, `:88`, `:90`; `app/ponto/signals.py:88`, `:93`; `app/ponto/services/ausencia.py:184`, `:190`; `app/ponto/services/fechamento.py:1661`, `:1665`, `:1696`)

- **Selo como piso é descrição correta; “18 FAIL” não prova suficiência.** O selo verifica presença de chamada viva e não o conteúdo dos mapas; o comportamento com mapa vazio global é observado por testes que exigem invalidação, mas sobrevivência de mutantes condicionais não fica eliminada por essa contagem relatada. (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:35`, `:222`, `:224`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:224`; execução dos 18 FAIL `[nao verificado]`)

- **`obs` é efetivamente lido como metadado por substring.** A existência da dívida está demonstrada; sua idade e gestão em fila `[nao verificado]`. (`app/ferias/management/commands/listar_ferias_dp.py:27`, `:28`, `:30`)

## 16/11 08:52 · sessao 02b77040be5f · - · - · origem=Ronald (tmux)

- O descarte de “levantar antes de tentar escrever” **só se sustenta para `raise` incondicional**. O positivo chama o serviço sem mock e exige retorno 5, cinco trilhas, invalidação e idempotência; uma exceção incondicional escaparia. Ele não exige a ordem escrita→auditoria nem tentativa de escrita quando a auditoria falha. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:211`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:216`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:227`)

- **Mutante sobrevivente plausível:** mover `_ev_rej(...)` para antes de `a.save`; se retornar `None`, levantar `TrilhaDaRejeicaoNaoGravada`; caso contrário, salvar. O positivo continua observando cinco rejeições/trilhas; o negativo encontra exatamente o retrato anterior e a exceção esperada, sem nenhuma tentativa de salvar ausência. Os mocks negativos falham já na primeira auditoria de rejeição. Mutação executada `[nao verificado]`. (`app/ponto/services/ausencia.py:1494`; `app/ponto/services/ausencia.py:1498`; `app/ponto/services/ausencia.py:1499`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:243`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:252`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:531`)

- Variante mais forte: além dessa inversão, retirar o `atomic` local da rejeição. Falha na segunda trilha de uma chamada direta poderia deixar a primeira rejeição persistida, mas os negativos falham na primeira; o positivo não injeta falha. No cancelamento, o `atomic` externo ainda protegeria o ato. A sobrevivência é inferência estática; execução `[nao verificado]`. (`app/ponto/services/ausencia.py:1484`; `app/ponto/services/ausencia.py:1494`; `app/ferias/services.py:674`; `app/ferias/services.py:683`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:243`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:531`)

- Os retratos novos fecham alterações/exclusões nos **campos enumerados**, não na linha inteira. Permanecem fora, por exemplo, `Ausencia.hora_ini/hora_fim/aprovado_por/aprovada_em/atualizado_em`, `AgendamentoFerias.confirmacao/confirmado_em/aviso_emitido` e `LogAuditoria.usuario/ip/ator/criado_em`. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:238`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:520`; `app/ponto/models.py:768`; `app/ponto/models.py:815`; `app/ponto/models.py:824`; `app/ferias/models.py:315`; `app/ferias/models.py:324`; `app/core/models.py:295`; `app/core/models.py:302`)

- O descarte de trilha nova é coerente com o contrato explicitamente escolhido: comparar somente PKs antigas. Porém o teste permite **qualquer** trilha nova, sem conferir que seja recusa. Os handlers de falha consultados apenas retornam mensagem; não criam explicitamente auditoria de recusa. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:517`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:546`; `app/ferias/views.py:398`; `app/ferias/views.py:452`; `app/ferias/views.py:621`)

- Os descartes de adesão e conteúdo do julgamento delimitam escopo, mas não eliminam as lacunas: a fixture percorre o ramo `nunca_bateu`, que carimba e retorna antes do ramo normal; os asserts conferem contador, carimbo e FK, sem veredito/ata. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:454`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:460`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:470`; `app/ponto/services/cartorio.py:1314`; `app/ponto/services/cartorio.py:1318`)

- D/E continuam presentes: `engolir` não abre savepoint; a invalidação captura falha e retorna zero; repetição com insumos iguais não invalida; o contador olha apenas `derivado_em=NULL`. Classificá-los como fora desta volta é decisão de escopo; antiguidade independente dos anexos `[nao verificado]`. (`app/ponto/signals.py:88`; `app/core/observ.py:85`; `app/ponto/services/fechamento.py:1661`; `app/ponto/services/ausencia.py:184`; `app/ponto/services/fechamento.py:1696`)

- O selo continua apenas estrutural quanto à chamada: detecta uma chamada viva ao acordador, não valida seus argumentos. Os testes comportamentais positivos matam silenciar **todos** os acordamentos, mas não demonstram cobertura de todo ramo/contexto. Os “18 FAIL” executados permanecem `[nao verificado]`. (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:222`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:224`)

- O leitor de `obs` realmente infere origem por substrings. A árvore também confirma que regularização grava Justificativa; a formulação atual da entrega, restringindo “só SELECT” aos acréscimos dos ganchos, corrige a ambiguidade anterior. Igualdade byte a byte com a base e logs de execução `[nao verificado]`. (`app/ferias/management/commands/listar_ferias_dp.py:26`; `app/ponto/services/regularizacao.py:102`; `app/ponto/services/regularizacao.py:106`)

## 16/11 08:51 · sessao 46d141e5339f · COMPLEMENTO-LAUDO-[nome]-1611 · sonda · origem=Code

Os números abaixo são transcritos do anexo. Uso **[nao verificado]** porque a saída foi fornecida na mensagem, sem arquivo:linha para citação ou confirmação independente.

**1. Frota e empresas**

Em cada coluna, a ordem é **colaboradores no recorte / colaboradores divergentes / vínculos divergentes**. A unidade principal declarada é colaborador distinto; divergência significa algum vínculo do recorte com posto diferente do cadastro. [nao verificado]

| Empresa | `flags_ativos` | `flags_ativos_e_vigencia_hoje` | Evidência |
|---|---:|---:|---|
| 1 | 2 / 0 / 0 | 2 / 0 / 0 | [nao verificado] |
| 2 | 419 / 44 / 44 | 419 / 44 / 44 | [nao verificado] |
| 3 | 100 / 6 / 6 | 100 / 6 / 6 | [nao verificado] |
| 4 | 19 / 3 / 3 | 19 / 3 / 3 | [nao verificado] |
| 20 | 0 / 0 / 0 | 0 / 0 / 0 | [nao verificado] |
| 21 | 0 / 0 / 0 | 0 / 0 / 0 | [nao verificado] |
| 29 | 0 / 0 / 0 | 0 / 0 / 0 | [nao verificado] |
| **Total da frota** | **540 / 53 / 53** | **540 / 53 / 53** | [nao verificado] |

A saída declara schema `juliani`, consulta em `2026-11-16 11:50:25.864495+00:00` e `hoje=2026-11-16`. [nao verificado]

**2. Estado de `col237`**

| Campo | Valor exibido | Evidência |
|---|---|---|
| Cadastro | `pk=237`, empresa `2`, situação `ativo` | [nao verificado] |
| Posto do cadastro | `444` | [nao verificado] |
| Vínculos exibidos | Um, com `ec_pk=204` | [nao verificado] |
| Posto do vínculo | `487` | [nao verificado] |
| Tipo de escala | `tipo_escala_id=177` | [nao verificado] |
| Flag do vínculo | `ativa=true` | [nao verificado] |
| Datas do vínculo | `data_inicio=2026-07-21`; `data_fim=null` | [nao verificado] |
| Divergência | `diverge_do_cadastro=true` | [nao verificado] |
| Inclusão nos recortes | `entra_recorte_flags=true`; `entra_recorte_vigencia_hoje=true` | [nao verificado] |

**3. Erros e linhas ausentes**

- O trecho termina com `rc_sombra=0` e não exibe mensagem de erro ou traceback. [nao verificado]
- Há sete linhas por empresa e uma de total em cada recorte, além do cabeçalho e do detalhe de `col237`; as somas por empresa coincidem com os totais apresentados. [nao verificado]
- Não há lacuna aparente nessa estrutura; a existência de outras linhas obrigatórias segundo a seção 4 ou o código exato da sonda permanece [nao verificado].

**4. O que a saída não responde**

- Qual posto a geofence ou a apuração de feriado efetivamente usaram em dias passados, inclusive para `col237`. [nao verificado]
- Quando surgiu a divergência, quem a causou ou qual posto deveria prevalecer; `data_inicio` é a data exibida do vínculo, sem histórico de alterações anexado. [nao verificado]
- Quais são os demais colaboradores divergentes, seus vínculos, ou os nomes e características dos postos `444` e `487`. [nao verificado]
- Se houve impacto em batidas, feriados, pagamentos ou alertas, e qual é o estado posterior à consulta. [nao verificado]

_5978 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
