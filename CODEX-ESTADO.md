# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 10:20** · publicado em **06/11/2026 10:21** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 10:20 · sessao cffa6b326684 · - · - · origem=Ronald (tmux)

Regras e definições O10 lidas:

- Contrato: `app/docs/CORTES.md:187`; reprodução no backlog: `app/docs/BACKLOG.md:1224`.
- S5b: `app/docs/BACKLOG.md:1248`. A “troca” desse corte é a substituição do motor pelo calculador: classes que não julgavam pontualidade não passam a cobrar. Não atribuir essa origem exclusivamente à troca de plantão.
- Idempotência/coerência: L-099, `app/docs/LEIS.md:122`.
- Competência: L-026, `app/docs/LEIS.md:77`; L-090, `app/docs/LEIS.md:123`.
- Meia-noite: L-085, `app/docs/LEIS.md:117`.
- Célula soberana: L-020, `app/docs/LEIS.md:71`.
- Exportada: L-113, `app/docs/LEIS.md:150`; restrição específica O10 registrada em `app/docs/BACKLOG.md:1781` (“motor_v2/exportada sem !”).

Definições para entradas:

| Model | Campos com definição |
|---|---|
| TipoEscala | `tipo_ciclo` `app/escala/models.py:117`; valores 12x36 `:26`, 6x1 `:30`; `hora_inicio` `:50`; `hora_fim` `:51`; intervalo `:52` e `:53` |
| EscalaColaborador | `colaborador` `app/escala/models.py:872`; `tipo_escala` `:874`; `data_inicio` `:875`; `data_fim` `:876`; `ativa` `:877` |
| CelulaDia | `colaborador` `app/escala/models.py:1681`; `data` `:1685`; `trabalha` `:1686`; `origem` `:1691`; `escala_geradora` `:1692`; `dna` `:1697`; `editada_em` `:1732`; `editada_por` `:1733` |
| Ausencia | `colaborador` `app/ponto/models.py:743`; `tipo` `:749`; `data` `:750`; `data_fim` `:751`; `documento` `:777`; `status` `:799`; `aprovado_por` `:805`; `aprovada_em` `:810` |
| TipoAusencia | `codigo` `app/ponto/models.py:577`; `efeito` `:581` |
| Batida | `colaborador` `app/ponto/models.py:22`; `timestamp` `:23` |
| Empresa | `dia_inicio_competencia` `app/colaboradores/models.py:46` |
| DiaPago, observáveis | `horas_trabalhadas` `app/ponto/models.py:326`; `horas_noturnas` `:327`; HE100 `:330`, `:331`, `:332`; `horas_atraso` `:334`; `horas_saida_antecipada` `:335`; `horas_falta` `:336`; `horas_intra_indenizada` `:337` |

Não achei definição de model/campos próprios do par, do dia cedido e das duas cartas: substituir essas entradas por `[nao verificado]`. O registro do estado atual também diz não haver escritor humano de célula (`app/docs/RELATO.md:3`); isso não define o esperado.

Casos sustentados:

| Caso | Origem | Entrada e esperado pela regra | Mutante |
|---|---|---|---|
| Cedente 12x36 | `app/docs/CORTES.md:187` | Par completo, duas cartas, confirmação admin, cedente 12x36; dia cedido vira folga trocada, sem desconto. Valores concretos de AN/intra não definidos pelo contrato. | Override aplicado só ao substituto. |
| Cedente 6x1 | `app/docs/CORTES.md:187` | Mesmo cenário, cedente 6x1; mesmo efeito de folga sem desconto. | Restringir troca a 12x36. |
| Substituto em folga | `app/docs/CORTES.md:187` | Dia originalmente de folga assumido por par confirmado; dia normal com AN/intra, HE100=0. | Manter folga trabalhada/HE100 ou suprimir AN/intra. |
| Zero anexos | `app/docs/CORTES.md:187` | Pedido de confirmação sem cartas; não vale como troca confirmada. Código/status/texto exato: regra não decide. | Remover exigência de documentos. |
| Só carta do cedente | `app/docs/CORTES.md:187` | Um documento; não confirma. | Trocar exigência dos dois por qualquer um. |
| Só carta do substituto | `app/docs/CORTES.md:187` | Mesmo resultado, em sentido contrário. | Validar apenas documento do substituto. |
| Par incompleto | `app/docs/CORTES.md:187` | Ausencia `tipo='troca_de_plantao'`, só um lado; continua pedido, sem efeito de par. Mesmo status legado “aprovada” não fornece o segundo lado. | Aprovação da ausência isolada basta para efetivar. |
| Origem cedente versus substituto | `app/docs/CORTES.md:187` | Pedido pode nascer de qualquer lado; completado e confirmado, resultado equivalente. | Aceitar apenas pedidos do cedente. |
| Segunda confirmação | L-099, `app/docs/LEIS.md:122` | Repetir confirmação do mesmo par sem alterar entradas; mesmo estado e efeito, sem duplicar desconto, pagamento ou override. Retorno/trilha repetida: regra não decide. | Inserção incondicional na reconfirmação. |
| Duas confirmações concorrentes | L-099, `app/docs/LEIS.md:122` | Mesmo par e entradas em duas confirmações; estado final equivalente a uma confirmação. Status HTTP, perdedor e número de tentativas de trilha: regra não decide. | Duas operações gravam duas trocas/efeitos. |
| Estado intermediário | `app/docs/CORTES.md:187` | Pedido ainda sem par confirmado; nenhum lado deve ser tratado como troca válida. Atomicidade de armazenamento e isolamento de leitura: regra não decide. | Aplicar cedido antes de completar/confirmar o par. |
| Meia-noite | L-085 `app/docs/LEIS.md:117` + contrato | Jornada assumida 20/10 19h–21/10 07h; jornada pertence ao dia 20, sem criar segundo trabalho dia 21; dia assumido normal sem HE100. | Classificar a cauda como trabalho independente dia 21. |
| Competência 20/21 | L-026 `app/docs/LEIS.md:77` + contrato | Empresa com corte 21, dias cedido/assumido em 20/10 e 21/10; dia 20 pertence à comp 10, dia 21 à comp 11. Efeitos da troca não desaparecem na fronteira. | Recortar por mês civil ou exigir ambos na mesma competência. |
| Vínculo trocado | L-020 `app/docs/LEIS.md:71` | Célula com `escala_geradora` antigo e vínculo atual diferente; preservar DNA congelado fora do ato autorizado. Qual vínculo habilita troca: regra não decide. | Resolver passado pelo vínculo atual e reescrever DNA incidentalmente. |
| Colega sem vínculo no dia | Contrato + L-020 | Ausência de vínculo ativo no dia; regra não decide se recusa ou como aceita. | Não há mutante de aceitação/recusa justificável sem regra adicional. |
| Cedido já trabalhado | `app/docs/CORTES.md:187` | Batidas existentes no dia cedido; regra não decide conflito entre trabalho já realizado e folga trocada. | Não há gabarito para escolher aceitar/recusar. |
| Célula editada | L-020 `app/docs/LEIS.md:71` + contrato | `origem='editada'`, DNA já humano; regra não decide precedência entre edição anterior e troca confirmada. | Não há gabarito para substituir/preservar override. |
| Legado solo | `app/docs/CORTES.md:187` | Ausencia `troca_de_plantao`, aprovada, `data_fim=NULL`, sem contraparte; não constitui par válido. Conversão em lote depende de DRY→aval. | Migrar toda aprovação antiga como troca confirmada. |
| S5b contraexemplo | `app/docs/BACKLOG.md:1248` | Classe MotorComercial/Motor12x36/MotorIntermitente que não julgava pontualidade; troca do calculador não começa a cobrar atraso/antecipada. | Habilitar pontualidade dessas classes durante substituição. |

Lacunas procuradas:

- A2 literal O10: não consta no `aval.txt` raspado; `app/docs/BACKLOG.md:1781` só registra execução/prontidão.
- Validação do colega sem vínculo, dia cedido já trabalhado, conflito com célula editada e datas vazias: `[nao verificado]`.
- Status HTTP e texto de recusa, chave do par, armazenamento das duas cartas, retorno de reconfirmação e política de concorrência: `[nao verificado]`.
- O dado legado de produção O10 não foi fornecido no bloco raspado; números históricos do backlog são relato de código/documentação, não nova leitura de produção (`app/docs/BACKLOG.md:1224`).

## 06/11 10:20 · sessao ef523e07fb21 · - · - · origem=Ronald (tmux)

Evidências e casos sustentados:

| Caso | Regra escrita | Entrada com definições | Esperado pela regra | Mutante |
|---|---|---|---|---|
| CXR-vigia-d-e | Aval fornecido PRONTO (5); causas e soma em `app/chamados/services/vigia_fio.py:519`, `app/chamados/services/vigia_fio.py:531` | Dia retido passado; `CelulaDia.colaborador` `app/escala/models.py:1681`, `data` `app/escala/models.py:1685`; sem pergunta/chamado vivo; colaborador já na lista, dia antes ausente; após lavra dia presente com dono ESTRUTURA, definição do item/dono novo `[nao verificado]` | O dia sai de d e entra em e com detalhe `dia_na_lista_dono_estrutura`; d diminui 1, e aumenta 1, mesmo universo; a+b+c+d+e continua igual a `fio_morto_alcance`. ESTRUTURA continua no alarme pela regra literal `app/chamados/services/vigia_fio.py:292`. | Continuar classificando por presença apenas do colaborador; omitir o dia de ambas as classes; abater d sem acrescentar e. |
| CXR-vigia-admin | Corte literal registrado em `app/docs/PROMPTS.md:1670`; transcrição em `app/chamados/services/vigia_fio.py:292` | Mesmo cenário, dia publicado com dono BATIDA ou CADASTRO `[nao verificado]` | Vai para `fio_do_admin`, com dono lido da lista; não é correto exigir e para esses donos. Se sair da lista sem resolver, volta ao fio morto. | Encaminhar todo dono para e; recalcular o dono no vigia; manter fora do alarme depois de sair da lista. |
| CXR-dia-x-colab | Regra documentada em `app/escala/services/cadastro_realidade.py:953` | Lista contém assinatura de vínculo/colaborador, mas nenhuma assinatura com data do dia `[nao verificado]`; `CelulaDia.data` `app/escala/models.py:1685` | Presença do colaborador não equivale a presença do dia; o dia continua fora de `dias_na_lista`. | Marcar todos os dias de um colaborador como presentes porque existe alguma linha dele. |
| CXR-lista-falhou | Regra explícita em `app/chamados/services/vigia_fio.py:191` | Carregamento da lista falha; mesmo dia consultado nos dois universos; parâmetros `hoje`, `empresas`, `agora` definidos em `app/chamados/services/vigia_fio.py:199` | Falha não retira dia do contador; `fio_do_admin_sem_resposta` conta uma vez o par colaborador/data na rodada. | Tratar exceção como lista vazia válida/alarme resolvido; contar duas vezes o mesmo dia. |
| CXR-lavra-nao-reescreve-celula | L-020 `app/docs/LEIS.md:71` | `CelulaDia.origem='editada'` `app/escala/models.py:1691`; DNA congelado `app/escala/models.py:1697`; lavra/publicação de CXR | A publicação da lista não autoriza alterar dia ou marcos congelados; `origem`, `dna`, `trabalha` e proveniência permanecem. | Regerar célula como efeito colateral de publicar lista. |

Definições úteis adicionais:

- `EscalaColaborador`: colaborador `app/escala/models.py:872`; posto `:873`; tipo_escala `:874`; data_inicio `:875`; data_fim `:876`; ativa `:877`.
- `Colaborador.situacao`: `app/colaboradores/models.py:323`; data_demissao: `:325`.
- `CelulaDia.escala_geradora`: `app/escala/models.py:1692`; regenerada_em `:1718`; regeneracoes `:1723`; dna_anterior `:1726`.
- Formato observável do vigia: `[celula_id, colab_id, data, competencia, rota, causa, detalhe, papel]`, documentado em `app/chamados/services/vigia_fio.py:548`.
- Universo e partes: `porta_fila + porta_pergunta + de_hoje_pra_frente + quatro baldes + fio_morto_alcance = universo`; `a+b+c+d+e=fio_morto_alcance`, regra escrita em `app/chamados/services/vigia_fio.py:531`.
- `limite` nunca omite dia sem porta; contagem por rota/papel cobre universo inteiro, regra escrita em `app/chamados/services/vigia_fio.py:551`.

Lacunas encontradas:

- Regra específica de CXR para lavra ainda não rodada hoje: não encontrada. C8 define retorno sem medida em `app/ponto/services/vinculo_na_lista.py:125`, mas isso é contexto de outra assinatura, não gabarito CXR.
- Regra de CXR para exposição durante escrita intermediária e desempate entre publicações concorrentes de conteúdos diferentes: não encontrada; aval apenas exige mesmo estado ao lavrar duas vezes.
- Regra específica CXR para desligado ou competência exportada: não encontrada além da autoridade única do aval; não importar exclusões C8/C11. C8 exclui desligado em `app/ponto/services/vinculo_na_lista.py:151`; C11 tem escopo próprio de competência aberta em `app/escala/services/cadastro_realidade.py:119`.
- Esquema da publicação CXR: `[nao verificado]`. O modelo C8 não foi imposto pelo aval como modelo de dados; `MetricaSnapshot` está definido em `app/inteligencia/models.py:81`.
- A garantia declarada de uma linha no C8 está em `app/ponto/services/vinculo_na_lista.py:111`; não é evidência de comportamento concorrente CXR.

## 06/11 10:18 · sessao 279f5e84cfaa · O10-B · censo · origem=Code

**1. Sítios que escrevem em `CelulaDia`**

| Sítio físico | Escrita | Pela porta ou fora |
|---|---|---|
| `regenerar_celulas_vinculo` | Fora da vigência: `save` de `trabalha=False` e DNA, somente para origem `gerada`. `app/ponto/portas/celula.py:232`, `app/ponto/portas/celula.py:239` | Pela porta. |
| `regenerar_celulas_vinculo` | `create` da célula ausente, com colaborador, data, trabalha, origem `gerada`, geradora e DNA. `app/ponto/portas/celula.py:253` | Pela porta. |
| `regenerar_celulas_vinculo` | `save` de trabalha, geradora, DNA, DNA anterior, momento e contador de regeneração; exige origem `gerada` e diferença. `app/ponto/portas/celula.py:257`, `app/ponto/portas/celula.py:270` | Pela porta. |
| `reverter_regeneracao_dia` | `save` de DNA restaurado, DNA anterior limpo, trabalha, geradora e trilha de regeneração. `app/ponto/portas/celula.py:570`, `app/ponto/portas/celula.py:578` | Pela porta. |
| `gerar_celulas_periodo` | `create` da célula ausente. `app/ponto/portas/celula.py:633` | Pela porta. |
| `gerar_celulas_periodo` | `save` somente do DNA: enriquecimento de célula passada, `gerada`, com DNA nulo. `app/ponto/portas/celula.py:643`, `app/ponto/portas/celula.py:645` | Pela porta. |
| `gerar_celulas_periodo` | `save` de trabalha, geradora e DNA para hoje/futuro; a editada foi pulada antes. `app/ponto/portas/celula.py:638`, `app/ponto/portas/celula.py:650`, `app/ponto/portas/celula.py:655` | Pela porta. |
| `salvar_e_emitir` | `cel.save(update_fields=campos)`, compartilhado pela lavra e pelos carimbos. `app/ponto/portas/celula.py:678`, `app/ponto/portas/celula.py:684` | Pela porta. |
| `carimbar_insumos_em_lote` | `bulk_update` de ausência, entrada, saída e `insumos_em`. `app/ponto/portas/celula.py:887` | Pela porta. |
| Chamadas lógicas do `save` compartilhado | Lavra sobre decisão administrativa: ata/status; atualização somente de status; lavra completa; carimbo; precedência na ata. `app/ponto/portas/celula.py:787`, `app/ponto/portas/celula.py:804`, `app/ponto/portas/celula.py:819`, `app/ponto/portas/celula.py:851`, `app/ponto/portas/celula.py:866` | Todas pela porta. |
| Delegação do modelo e chamadores da lavra | O método do modelo delega; decisão humana e cartório chamam a lavra. Não são novos pontos físicos de `save`. `app/escala/models.py:1826`, `app/ponto/services/veredito_celula.py:330`, `app/ponto/services/cartorio.py:889`, `app/ponto/services/cartorio.py:1242`, `app/ponto/services/cartorio.py:1802` | Pela porta. |
| Outros métodos de DML | Nenhum sítio adicional de `update`, `update_or_create`, `bulk_create` ou SQL direto contra produção foi localizado na cópia; completude além dos sítios identificados: **[nao verificado]**. |
| SQL de cópia da sombra | Inclui `escala_celuladia` e executa `DELETE`/`COPY`, porém o destino declarado é `sombra_db`/`sombra`; produção fornece `SELECT`/`COPY TO`. `bin/sombra_incremental.sh:20`, `bin/sombra_incremental.sh:35`, `bin/sombra_incremental.sh:55`, `bin/sombra_incremental.sh:61`, `bin/sombra_incremental.sh:68` | Fora da porta, destinado à sombra. |
| Migration `0030` | `CreateModel`: cria o esquema da tabela. `app/escala/migrations/0030_celuladia.py:17` | Fora da porta; DDL. |
| Migration `0031` | Adiciona DNA. `app/escala/migrations/0031_celuladia_dna.py:13` | Fora da porta; DDL. |
| Migration `0032` | Adiciona impressão e momento do julgamento. `app/escala/migrations/0032_celuladia_impressao_celuladia_julgada_em.py:13`, `app/escala/migrations/0032_celuladia_impressao_celuladia_julgada_em.py:18` | Fora da porta; DDL. |
| Migration `0036` | Adiciona protestos e campos do veredito. `app/escala/migrations/0036_b5_celula_veredito.py:13`, `app/escala/migrations/0036_b5_celula_veredito.py:18`, `app/escala/migrations/0036_b5_celula_veredito.py:23`, `app/escala/migrations/0036_b5_celula_veredito.py:28` | Fora da porta; DDL. |
| Migration `0037` | Adiciona ata. `app/escala/migrations/0037_hx_ata_celula.py:13` | Fora da porta; DDL. |
| Migration `0038` | Adiciona DNA anterior, contador e momento de regeneração; contador tem default zero. `app/escala/migrations/0038_celuladia_dna_anterior_celuladia_regeneracoes_and_more.py:13`, `app/escala/migrations/0038_celuladia_dna_anterior_celuladia_regeneracoes_and_more.py:18`, `app/escala/migrations/0038_celuladia_dna_anterior_celuladia_regeneracoes_and_more.py:23` | Fora da porta; DDL. |
| Migration `0039` | Adiciona ausência, `insumos_em`, entrada e saída. `app/escala/migrations/0039_b5f2_celula_insumos.py:15`, `app/escala/migrations/0039_b5f2_celula_insumos.py:20`, `app/escala/migrations/0039_b5f2_celula_insumos.py:25`, `app/escala/migrations/0039_b5f2_celula_insumos.py:30` | Fora da porta; DDL. |
| Migration `0044` | Adiciona status e rótulo. `app/escala/migrations/0044_celuladia_status_dia_celuladia_status_rotulo.py:13`, `app/escala/migrations/0044_celuladia_status_dia_celuladia_status_rotulo.py:18` | Fora da porta; DDL. |

**2. Leitores de origem editada, `editada_em`, `editada_por` ou motivo**

| Âmbito / leitor | O que decide |
|---|---|
| Produção — regeneração fora da vigência | Só desliga origem `gerada`; preserva trabalha/DNA da editada. `app/ponto/portas/celula.py:232` |
| Produção — regeneração normal | Só reescreve origem `gerada`. `app/ponto/portas/celula.py:257` |
| Produção — reversão | Retorna `False` se origem diferente de `gerada`. `app/ponto/portas/celula.py:558` |
| Produção — geração periódica | Origem `editada` incrementa `puladas`; enriquecimento passado exige `gerada`. `app/ponto/portas/celula.py:638`, `app/ponto/portas/celula.py:643` |
| Produção — `medir_stale` | Consulta somente `gerada`; editada fica fora dessa acusação de divergência com o cadastro. `app/ponto/management/commands/tripwire_celulas.py:21` |
| Produção — `contra_o_template` | Consulta somente `gerada`; editada não conta como erro do gerador. `app/escala/management/commands/celulas_contra_o_template.py:29` |
| Produção — `houve_fato_novo` | `editada_em` em `(desde, ate]` retorna `True`. `parcial_so_lista` usa esse resultado para decidir se o restante fica somente na lista. `app/ponto/services/cartorio.py:365`, `app/ponto/services/cartorio.py:400` |
| Produção — `marcos_do_lote` / `esta_stale` | Inclui `editada_em` no maior timestamp por colaborador; marco posterior ao julgamento torna a pergunta stale. `app/chamados/veredito_lavrado.py:151`, `app/chamados/veredito_lavrado.py:154`, `app/chamados/veredito_lavrado.py:187` |
| Produção — `janelas_reabertas` | Inclui `Max(editada_em)`; fato posterior à lavra reabre a janela passada para nova lavra do censo de fase. `app/escala/services/fase_12x36.py:188`, `app/escala/services/fase_12x36.py:192` |
| Produção — apresentação da origem | Expõe a origem no retrato do dia, representação textual e detalhe do contador; sem decisão especial por `editada` nesses trechos. `app/ponto/services/dia_do_colab.py:90`, `app/escala/models.py:1842`, `app/escala/management/commands/celulas_contra_o_template.py:74` |
| Produção — `editada_por` | Nenhum leitor de negócio localizado **[nao verificado]**; o campo é declarado como FK opcional. `app/escala/models.py:1733` |
| Produção/testes — motivo da edição | Não existe campo de motivo de edição no catálogo da célula; o catálogo é confrontado com os campos concretos do modelo. `app/escala/tests/test_d3_guarda_da_linha_celula.py:38`, `app/escala/tests/test_d3_guarda_da_linha_celula.py:146`, `app/escala/tests/test_d3_guarda_da_linha_celula.py:171` |
| Teste — preservação da origem | Lê origem após gerar novamente e exige `editada`, com trabalha preservado. `app/escala/tests/test_celula_dia.py:83` |
| Teste — contrato do DNA gerado | Filtra origem `gerada` para verificar vocabulário, versão e vínculo do DNA; editadas ficam fora desse universo. `app/escala/tests/test_celula_dia.py:116` |
| Teste — regeneração não é edição humana | Exige `editada_em is None` após regeneração. `app/escala/tests/test_cobranca_orfa_regeneracao.py:166` |
| Teste — isolamento do relógio de regeneração | Exige `editada_em is None` no caso que prova reabertura por `regenerada_em`. `app/escala/tests/test_vfase_dono_as_of.py:279` |
| Teste — guarda da linha | Classifica os dois campos, mantém a exceção `SEM_ESCRITOR` e procura escritas sintáticas deles. `app/escala/tests/test_d3_guarda_da_linha_celula.py:50`, `app/escala/tests/test_d3_guarda_da_linha_celula.py:72`, `app/escala/tests/test_d3_guarda_da_linha_celula.py:179` |
| Testes com fixture editada — autoridade | Escreve origem editada e verifica que `eh_dia_trabalho` acompanha a célula, divergindo da aritmética. `app/escala/tests/test_celula_dia.py:98`, `app/escala/tests/test_celula_dia.py:100` |
| Testes com fixture editada — tripwire | Verifica que editada não acusa stale. `app/escala/tests/test_celula_dia.py:358`, `app/escala/tests/test_celula_dia.py:361` |
| Testes com fixture editada — fim do vínculo | Verifica que célula humana permanece trabalhando fora da vigência. `app/escala/tests/test_selo_celula_alem_do_vinculo.py:103`, `app/escala/tests/test_selo_celula_alem_do_vinculo.py:106` |
| Testes com fixture editada — foto de folgas | Verifica que a foto não reescreve trabalha humano. `app/escala/tests/test_selo_foto_folgas_relavra.py:106`, `app/escala/tests/test_selo_foto_folgas_relavra.py:109` |
| Testes com fixture editada — contadores | Contra-template ignora editada; folga com marcos inclui editada, pois esse segundo contador vale para qualquer origem. `app/escala/tests/test_foto_nao_apaga_o_ciclo.py:98`, `app/escala/tests/test_foto_nao_apaga_o_ciclo.py:99`, `app/escala/tests/test_foto_nao_apaga_o_ciclo.py:128`, `app/escala/tests/test_foto_nao_apaga_o_ciclo.py:129` |
| Testes com fixture editada — universo de furos | Folga materializada prevalece sobre template sem fase e resulta `sem_dia`. `app/escala/tests/test_sem_dia_pelo_juiz.py:46`, `app/escala/tests/test_sem_dia_pelo_juiz.py:65` |
| Testes com fixture editada — troca de template | Verifica preservação da célula humana após executar mudança do vínculo. `app/colaboradores/tests/test_vinculo_regenera_celulas.py:39`, `app/colaboradores/tests/test_vinculo_regenera_celulas.py:45` |
| Testes com fixture editada — reversão | Exige recusa da reversão mesmo havendo DNA anterior. `app/ponto/tests/test_porta_celula_dia.py:247`, `app/ponto/tests/test_porta_celula_dia.py:250` |
| Testes com fixture editada — reconciliação | Usam folga editada para verificar reconciliação/deduplicação, sobrevivência do novo e absolvição da pergunta; não comparam diretamente origem nos asserts. `app/ponto/tests/test_fechar3_troca_escala_gate.py:134`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:157`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:180`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:203`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:225`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:249` |
| Teste com fixture `editada_em` — impressão | Atualiza ata e `editada_em`; exige hash inalterado. `app/ponto/tests/test_d3_impressao_cobre_insumos.py:140`, `app/ponto/tests/test_d3_impressao_cobre_insumos.py:141` |
| Teste com fixture de edição e DNA novo | Atualiza origem, `editada_em` e marcos; exige hash alterado. A entrada no hash é pelo DNA. `app/ponto/tests/test_d3_impressao_cobre_insumos.py:239`, `app/ponto/tests/test_d3_impressao_cobre_insumos.py:242` |
| Teste com fixture somente `editada_em` | Verifica reabertura da competência no censo de fase. `app/escala/tests/test_vfase_dono_as_of.py:291`, `app/escala/tests/test_vfase_dono_as_of.py:297` |

**3. Quem escreve hoje `editada_em` / `editada_por`**

| Campo / âmbito | Resposta |
|---|---|
| `editada_em`, edição em produção | **Nenhum escritor declarado**; catálogo marca `None`, e o selo exige ausência de escritor nos arquivos de produção que varre. `app/escala/tests/test_d3_guarda_da_linha_celula.py:50`, `app/escala/tests/test_d3_guarda_da_linha_celula.py:179` |
| `editada_por`, edição em produção | **Nenhum escritor declarado**, sob a mesma guarda. `app/escala/tests/test_d3_guarda_da_linha_celula.py:51`, `app/escala/tests/test_d3_guarda_da_linha_celula.py:179` |
| `editada_em`, testes | Há escritas explícitas nas fixtures de impressão e reabertura de fase. `app/ponto/tests/test_d3_impressao_cobre_insumos.py:140`, `app/ponto/tests/test_d3_impressao_cobre_insumos.py:243`, `app/escala/tests/test_vfase_dono_as_of.py:291` |
| Declaração da FK | `editada_por` possui `on_delete=SET_NULL`; isso é a política declarada da relação, distinta de um escritor de edição humana. `app/escala/models.py:1733` |
| Ocorrências reais em produção | **[nao verificado]**; o dado fornecido não contém linhas de células nem auditorias de edição. |

**4. O que a porta e seus selos exigem**

| Aspecto | Contrato e implementação |
|---|---|
| Operações existentes | `OPERACOES` contém `gerar`, `lavrar_veredito` e `carimbar`; não contém `editar_dia`. `app/ponto/portas/celula.py:66` |
| Gate de nomes | O teste exige exatamente esses três nomes e que todas as funções declaradas sejam chamáveis. Acrescentar a quarta operação altera essa expectativa. `app/ponto/tests/test_chokepoint_celula_gate.py:62` |
| Gate de localização | Para entidades identificadas da família, escrita deve estar na porta; allowlist e pendências são vazias. `app/ponto/tests/test_chokepoint_celula_gate.py:21`, `app/ponto/tests/test_chokepoint_celula_gate.py:25`, `app/ponto/tests/test_chokepoint_celula_gate.py:49` |
| Gate dentro da porta | Escritas detectadas devem estar dentro de funções declaradas em `OPERACOES`. `app/ponto/tests/test_chokepoint_celula_gate.py:69` |
| Limite do gate | Ignora achados com entidade não inferida; o varredor exclui migrations e testes. Portanto o gate não substitui a leitura do `save` genérico e das migrations. `app/ponto/tests/test_chokepoint_celula_gate.py:29`, `app/core/censo_escritas.py:66`, `app/ponto/portas/celula.py:684` |
| Guarda de competência | Na regeneração, carrega janelas lavradas e pula seus dias. O carregador consulta o juiz `competencia_fechada_para_celula`, considerando candidatas com exportação ou holerite publicado. `app/ponto/portas/celula.py:202`, `app/ponto/portas/celula.py:244`, `app/ponto/management/commands/gerar_celulas.py:161`, `app/ponto/management/commands/gerar_celulas.py:169`, `app/ponto/management/commands/gerar_celulas.py:173` |
| `barrados` | Regeneração conta dias barrados, guarda extremos e devolve `{dias,de,ate,texto}` se a lista foi fornecida. Também devolve recusa de vínculo sem vigência declarada. `app/ponto/portas/celula.py:139`, `app/ponto/portas/celula.py:244`, `app/ponto/portas/celula.py:474` |
| `apesar_da_lavra` | Valor verdadeiro esvazia as janelas impeditivas; o texto entra na trilha da regeneração. O código não faz validação adicional desse texto nesse ramo. `app/ponto/portas/celula.py:202`, `app/ponto/portas/celula.py:336`, `app/ponto/portas/celula.py:344` |
| Reversão | Repete a guarda de competência, mas devolve `False` ao bloquear; sua assinatura não oferece `barrados`. `app/ponto/portas/celula.py:520`, `app/ponto/portas/celula.py:560` |
| Alcance das guardas | Essas verificações estão nos corpos de regeneração/reversão. `OPERACOES` é uma declaração de funções, sem invólucro que aplique automaticamente guarda, auditoria ou idempotência a uma nova operação. `app/ponto/portas/celula.py:66`, `app/ponto/portas/celula.py:202`, `app/ponto/portas/celula.py:560` |
| Idempotência | Contrato: mesma entrada, mesmo estado e uma linha de trilha. Regeneração compara conteúdo; reversão limpa DNA anterior; lavra e carimbos retornam sem salvar quando iguais. `app/ponto/portas/celula.py:19`, `app/ponto/portas/celula.py:257`, `app/ponto/portas/celula.py:558`, `app/ponto/portas/celula.py:571`, `app/ponto/portas/celula.py:797`, `app/ponto/portas/celula.py:840`, `app/ponto/portas/celula.py:860` |
| Selos de idempotência | Exercitam geração, regeneração, lavra, carimbo e lote; há contagem adicional de chamados/perguntas para detectar duplicação. `app/ponto/tests/test_porta_celula.py:36`, `app/ponto/tests/test_porta_celula.py:46`, `app/ponto/tests/test_porta_celula.py:63`, `app/ponto/tests/test_porta_celula.py:79`, `app/ponto/tests/test_porta_celula.py:142`, `app/ponto/tests/test_porta_celula.py:165` |
| Trilha da regeneração | Emite `regenerar_celulas_vinculo` quando `n` é verdadeiro; inclui motivo, contagem de barrados e abertura da guarda. `app/ponto/portas/celula.py:327` |
| Trilha da lavra | Emite `veredito_celula`; o carimbo guarda sua trilha na própria célula. `app/ponto/portas/celula.py:33`, `app/ponto/portas/celula.py:805`, `app/ponto/portas/celula.py:820`, `app/ponto/portas/celula.py:844` |
| Auditoria e atomicidade | `evento` é escritor de `LogAuditoria`, com savepoint e comportamento best-effort; falha de auditoria não derruba o ato. Já célula e outbox são gravados no mesmo `atomic` de `salvar_e_emitir`. `app/core/observ.py:94`, `app/core/observ.py:105`, `app/core/observ.py:133`, `app/ponto/portas/celula.py:682` |
| Autor/motivo | A documentação exige autor para a trilha, mas regeneração aceita `autor=None`, `motivo=''`; ausência de autor não é recusada pela assinatura/caminho mostrado. `app/ponto/portas/celula.py:41`, `app/ponto/portas/celula.py:100`, `app/ponto/portas/celula.py:330` |

**5. `SEM_ESCRITOR` e o que o selo mede**

| Item | Conteúdo / medida |
|---|---|
| Linha 72 | Exatamente `SEM_ESCRITOR = {'editada_em', 'editada_por'}`. `app/escala/tests/test_d3_guarda_da_linha_celula.py:72` |
| Natureza da exceção | Ambos estão classificados como trilha, com escritor `None`. `app/escala/tests/test_d3_guarda_da_linha_celula.py:50` |
| Completude do catálogo | Compara `CATALOGO` aos campos concretos de `CelulaDia._meta`; campo sem linha e linha sem campo geram violação. `app/escala/tests/test_d3_guarda_da_linha_celula.py:124`, `app/escala/tests/test_d3_guarda_da_linha_celula.py:146` |
| Existência do escritor | Reconhece atribuições de atributos, kwargs de criação/atualização e nomes em listas de campos; verifica também `auto_now_add`. É reconhecimento sintático por nome de campo. `app/escala/tests/test_d3_guarda_da_linha_celula.py:80`, `app/escala/tests/test_d3_guarda_da_linha_celula.py:129` |
| Cobertura da impressão | Insumos devem aparecer como `cel.<campo>` em cada chamada de `impressao_insumos`; a exceção atual é `trabalha`. `app/escala/tests/test_d3_guarda_da_linha_celula.py:70`, `app/escala/tests/test_d3_guarda_da_linha_celula.py:107`, `app/escala/tests/test_d3_guarda_da_linha_celula.py:140` |
| Catracas | Os conjuntos de exceções só podem encolher. `app/escala/tests/test_d3_guarda_da_linha_celula.py:175` |
| Ausência de escritor | Varre Python de produção, excluindo inclusive `tests` e `migrations`, e exige zero achados para os campos em `SEM_ESCRITOR`. `app/escala/tests/test_d3_guarda_da_linha_celula.py:156`, `app/escala/tests/test_d3_guarda_da_linha_celula.py:179` |
| Nascer o escritor humano | O teste manda retirar o campo da catraca e declarar seu escritor no mesmo ato. `app/escala/tests/test_d3_guarda_da_linha_celula.py:180` |
| Unicidade | Reusa o gate da família; aqui verifica concordância entre a porta registrada e o catálogo por campo. `app/escala/tests/test_d3_guarda_da_linha_celula.py:191` |
| O selo não demonstra | Não consulta dados de produção nem prova propagação financeira da edição; suas entradas são AST, metadados e chamadas da impressão. `app/escala/tests/test_d3_guarda_da_linha_celula.py:167` |

**6. Leitores relevantes para falta/HE, cartório, fechamento e folha**

| Leitor | Dependência e decisão afetada |
|---|---|
| `EscalaColaborador.eh_dia_trabalho` | Havendo célula, retorna `trabalha`, sem filtro de origem. `app/escala/models.py:1087` |
| `marcos_do_dia`, `pausas_do_dia`, `extra_declarada_do_dia` | Consomem marcos, intervalos e extra declarada congelados no DNA. `app/escala/models.py:1117`, `app/escala/models.py:1182`, `app/escala/models.py:1227` |
| `minutos_previstos_do_dia` / `intervalo_declarado_do_dia` | Trabalho previsto, marcos e intervalos determinam minutos previsto

[truncado em 20000 de 29536 caracteres]

## 06/11 10:14 · sessao 4ea10ee07e5e · - · - · origem=Ronald (tmux)

| Q2 — leitor de produção | Decisão |
|---|---|
| `app/ponto/portas/celula.py:232` | Regeneração fora da vigência só desliga célula com `origem == 'gerada'`; preserva editada. |
| `app/ponto/portas/celula.py:257` | Regeneração dentro da vigência só reescreve `trabalha/dna` de gerada. |
| `app/ponto/portas/celula.py:558` | Reversão retorna `False` para origem diferente de gerada. |
| `app/ponto/portas/celula.py:638` | Gerador incrementa `puladas` para editada; o enriquecimento passado também exige gerada em `app/ponto/portas/celula.py:643`. |
| `app/ponto/management/commands/tripwire_celulas.py:22` | `medir_stale` só consulta geradas; editadas não são acusadas. |
| `app/escala/management/commands/celulas_contra_o_template.py:29` | Contador contra template só consulta geradas; exclui editadas. |
| `app/ponto/services/cartorio.py:365` | `editada_em` em `(desde, ate]` faz `houve_fato_novo` retornar `True`; `parcial_so_lista` usa esse resultado para decidir lista versus emissão em `app/ponto/services/cartorio.py:400`. |
| `app/chamados/veredito_lavrado.py:154` | Agrega `editada_em` com os outros timestamps no marco por colaborador; marco posterior ao julgamento torna pergunta stale em `app/chamados/veredito_lavrado.py:187`. |
| `app/escala/services/fase_12x36.py:190` | Agrega `editada_em`; timestamp posterior à lavra reabre janela passada em `app/escala/services/fase_12x36.py:192`. |
| `app/ponto/services/dia_do_colab.py:90`; `app/escala/models.py:1842`; `app/escala/management/commands/celulas_contra_o_template.py:74` | Leem `origem` para apresentação, sem ramo especial por editada nesses locais. |
| `editada_por` e motivo de edição | Nenhum leitor de negócio localizado; `editada_por` está declarado em `app/escala/models.py:1733`; não há campo motivo no catálogo completo de campos em `app/escala/tests/test_d3_guarda_da_linha_celula.py:38`. |

| Q2 — testes: leitura direta ou guarda | O que verificam | [nao verificado]
|---|---|
| `test_celula_dia.py::test_editada_intocavel_e_passado_imutavel` | Lê `cel.origem` e exige `'editada'` em `app/escala/tests/test_celula_dia.py:83`; exige trabalho editado preservado em `app/escala/tests/test_celula_dia.py:84`. |
| `test_cobranca_orfa_regeneracao.py::test_reescrita_deixa_marca_na_celula_e_guarda_o_dna_anterior` | Lê `editada_em` e exige `None`: regeneração não é edição humana, em `app/escala/tests/test_cobranca_orfa_regeneracao.py:166`. |
| `test_vfase_dono_as_of.py::test_MORDE_regenerada_em_reabre_a_competencia` | Lê `editada_em` e exige `None`, isolando regeneração, em `app/escala/tests/test_vfase_dono_as_of.py:279`. |
| `test_d3_guarda_da_linha_celula.py` | Classifica os dois campos como trilha sem escritor em `app/escala/tests/test_d3_guarda_da_linha_celula.py:50`; verifica a catraca em `app/escala/tests/test_d3_guarda_da_linha_celula.py:177`; varre possíveis escritores em `app/escala/tests/test_d3_guarda_da_linha_celula.py:179`. |

| Q2 — testes que **escrevem fixtures** editadas e verificam consumidores | Efeito verificado | [nao verificado]
|---|---|
| `app/escala/tests/test_celula_dia.py:98` | Juiz lê trabalho editado e difere da aritmética em `app/escala/tests/test_celula_dia.py:100`. |
| `app/escala/tests/test_celula_dia.py:358` | Tripwire não acusa em `app/escala/tests/test_celula_dia.py:361`. |
| `app/escala/tests/test_selo_celula_alem_do_vinculo.py:103` | Célula humana permanece trabalhando após encerrar vínculo em `app/escala/tests/test_selo_celula_alem_do_vinculo.py:106`. |
| `app/escala/tests/test_selo_foto_folgas_relavra.py:106` | Foto de folgas preserva trabalho humano em `app/escala/tests/test_selo_foto_folgas_relavra.py:109`. |
| `app/escala/tests/test_foto_nao_apaga_o_ciclo.py:98` | Contador contra template ignora editada em `app/escala/tests/test_foto_nao_apaga_o_ciclo.py:99`. |
| `app/escala/tests/test_foto_nao_apaga_o_ciclo.py:128` | Contador de folga com marcos inclui editada em `app/escala/tests/test_foto_nao_apaga_o_ciclo.py:129`. |
| `app/escala/tests/test_sem_dia_pelo_juiz.py:46` | Célula de folga prevalece sobre template e resulta `sem_dia` em `app/escala/tests/test_sem_dia_pelo_juiz.py:65`. |
| `app/colaboradores/tests/test_vinculo_regenera_celulas.py:39` | Troca de template preserva célula humana em `app/colaboradores/tests/test_vinculo_regenera_celulas.py:45`. |
| `app/ponto/tests/test_porta_celula_dia.py:247` | Reversão recusa célula humana em `app/ponto/tests/test_porta_celula_dia.py:250`. |
| `app/ponto/tests/test_fechar3_troca_escala_gate.py:134`, `:180`, `:225` | Fixtures de folga editada; verificam reconciliação/dedup, sobrevivência do novo e absolvição da pergunta em `app/ponto/tests/test_fechar3_troca_escala_gate.py:157`, `:203`, `:249`. Não fazem comparação direta de origem nesses asserts. |
| `app/ponto/tests/test_d3_impressao_cobre_insumos.py:140` | Escreve `editada_em` junto com ata e exige hash inalterado em `app/ponto/tests/test_d3_impressao_cobre_insumos.py:141`. |
| `app/ponto/tests/test_d3_impressao_cobre_insumos.py:243` | Escreve origem, timestamp e DNA; verifica hash alterado via `_mudou` em `app/ponto/tests/test_d3_impressao_cobre_insumos.py:242`. |
| `app/escala/tests/test_vfase_dono_as_of.py:291` | Escreve somente `editada_em`; exige competência reaberta em `app/escala/tests/test_vfase_dono_as_of.py:297`. |

| Q3 | Evidência |
|---|---|
| Escritor de produção de `editada_em/editada_por`: **nenhum**. | Declaração `None` em `app/escala/tests/test_d3_guarda_da_linha_celula.py:50`; selo que exige ausência em `app/escala/tests/test_d3_guarda_da_linha_celula.py:179`. |
| `ORIGEM_EDITADA` | Nenhuma constante localizada; o valor é literal na enumeração em `app/escala/models.py:1690`. |
| Motivo da edição da célula | Campo ausente do catálogo em `app/escala/tests/test_d3_guarda_da_linha_celula.py:38`; o catálogo é comparado aos campos concretos do modelo em `app/escala/tests/test_d3_guarda_da_linha_celula.py:171`. |

| Q5 — selo | O que mede |
|---|---|
| Conteúdo de `SEM_ESCRITOR` | Exatamente `{'editada_em', 'editada_por'}` em `app/escala/tests/test_d3_guarda_da_linha_celula.py:72`. |
| Catálogo completo | Campo vivo sem linha e linha sem campo geram violação em `app/escala/tests/test_d3_guarda_da_linha_celula.py:124`. |
| Escritor declarado | Exige escrita sintática do nome no arquivo ou `auto_now_add`; implementação em `app/escala/tests/test_d3_guarda_da_linha_celula.py:80` e `:129`. Não resolve entidade de cada atributo: reconhece nomes sintáticos. |
| Insumos | Cada chamada de `impressao_insumos` deve receber `cel.<campo>`; exceção atual `{'trabalha'}` em `app/escala/tests/test_d3_guarda_da_linha_celula.py:70`; cobrança em `:140`. |
| Catraça | Conjuntos só podem encolher em `app/escala/tests/test_d3_guarda_da_linha_celula.py:175`. |
| Ausência de escritores | Varre `.py` de produção, excluindo inclusive `tests` e `migrations`, em `app/escala/tests/test_d3_guarda_da_linha_celula.py:156`; exige zero achados para `SEM_ESCRITOR` em `:179`. |
| Unicidade | Reusa gate por entidade; aqui compara porta registrada e escritores do catálogo em `app/escala/tests/test_d3_guarda_da_linha_celula.py:191`. |
| Consequência de nascer escritor humano | Deve retirar campos da catraca e declarar escritor no mesmo ato, conforme teste em `app/escala/tests/test_d3_guarda_da_linha_celula.py:180`. |

Achado adicional: embora a docstring diga que célula humana não é julgada em `app/ponto/portas/celula.py:122`, o ramo `rejulgar` coleta qualquer célula existente e não filtra origem em `app/ponto/portas/celula.py:291`; adiciona dias a `_tocadas` em `:297`. Isso não reescreve DNA nesse ramo, mas precisa ser considerado para o censo de reescrita de saídas.

Não executei testes. Executei somente parsing AST em memória com `python3 -B`, reutilizando `campos_escritos` do selo; retornou lista vazia de escritores dos dois campos [não verificado por execução da suíte]. [nao verificado]

## 06/11 10:14 · sessao 4ece0d2822aa · - · - · origem=Ronald (tmux)

| Sítio físico de escrita em `CelulaDia` | Campos/efeito | Porta |
|---|---|---|
| `app/ponto/portas/celula.py:239` — `_cel_fv.save(...)` | Fora da vigência, transforma em folga; grava `trabalha`, `dna`. Restrito a `origem == 'gerada'` em `:232`. | Pela própria porta. |
| `app/ponto/portas/celula.py:253` — `CelulaDia.objects.create(...)` | Cria célula ausente com colaborador/data/trabalha/origem gerada/geradora/DNA. | Pela própria porta. |
| `app/ponto/portas/celula.py:270` — `cel.save(...)` | Regenera trabalha/geradora/DNA e trilha `dna_anterior`, `regenerada_em`, `regeneracoes`; só gerada (`:257`). | Pela própria porta. |
| `app/ponto/portas/celula.py:578` — `cel.save(...)` | Reverte DNA anterior, trabalha/geradora e trilha. Só gerada e com DNA anterior (`:558`). | Pela própria porta. |
| `app/ponto/portas/celula.py:633` — `CelulaDia.objects.create(...)` | Geração periódica de célula ausente. | Pela própria porta. |
| `app/ponto/portas/celula.py:645` — `cel.save(update_fields=['dna'])` | Enriquece passado com DNA nulo; somente gerada (`:643`). Editada já foi pulada (`:638-639`). | Pela própria porta. |
| `app/ponto/portas/celula.py:655` — `cel.save(...)` | Atualiza trabalha/geradora/DNA hoje/futuro; editada já foi pulada (`:638-639`). | Pela própria porta. |
| `app/ponto/portas/celula.py:684` — `salvar_e_emitir` | Save genérico pelos campos recebidos; atômico com evento quando veredito/ata mudou (`:682-686`). | Pela própria porta. |
| `app/ponto/portas/celula.py:887` — `CelulaDia.objects.bulk_update(...)` | Backfill de ausência/entrada/saída/insumos_em. | Pela própria porta. |

| Sítios lógicos que usam o save genérico de `:684` | Efeito |
|---|---|
| `app/ponto/portas/celula.py:787` | Cartório sobre veredito de admin: atualiza ata/status; preserva decisão admin (`:781-788`). |
| `app/ponto/portas/celula.py:804` | Só status mudou; grava status sem mover veredito_em (`:797-810`). |
| `app/ponto/portas/celula.py:819` | Lavra veredito/via/tempo/protestos/ata/status (`:811-825`). |
| `app/ponto/portas/celula.py:851` | Carimbo julgada_em/impressão/ausência/entrada/saída/insumos_em e campos lavrados (`:844-851`). |
| `app/ponto/portas/celula.py:866` | Grava precedência dentro de ata (`:859-866`). |

O varredor próprio vê oito escritas atribuídas a CelulaDia; `salvar_e_emitir:684` fica sem entidade inferida, pois recebe `cel` como parâmetro e lista dinâmica de campos — a limitação dessa inferência está documentada em `app/core/censo_escritas.py:43-47`. Portanto a lista de nove acima exige leitura manual além do gate (`app/ponto/tests/test_chokepoint_celula_gate.py:25-35`).

| Migrations | Escrita fora da porta? / efeito sobre editada |
|---|---|
| `app/escala/migrations/0030_celuladia.py:17` | `CreateModel`, schema; declara trabalha/origem/editada_em/editada_por (`:22-28`). |
| `app/escala/migrations/0031_celuladia_dna.py:13` | `AddField dna`; nenhuma lógica por origem (`:13-17`). |
| `app/escala/migrations/0032_celuladia_impressao_celuladia_julgada_em.py:13` e `:18` | Acrescenta impressão/julgada_em; schema (`:13-22`). |
| `app/escala/migrations/0036_b5_celula_veredito.py:13`, `:18`, `:23`, `:28` | Acrescenta protestos/veredito/veredito_em/veredito_via; schema. |
| `app/escala/migrations/0037_hx_ata_celula.py:13` | Acrescenta ata; schema. |
| `app/escala/migrations/0038_celuladia_dna_anterior_celuladia_regeneracoes_and_more.py:13`, `:18`, `:23` | Acrescenta dna_anterior/regeneracoes/regenerada_em; regeneracoes tem default 0 (`:21`). |
| `app/escala/migrations/0039_b5f2_celula_insumos.py:15`, `:20`, `:25`, `:30` | Acrescenta ausência/insumos_em/entrada/saída; schema. |
| `app/escala/migrations/0044_celuladia_status_dia_celuladia_status_rotulo.py:13` e `:18` | Acrescenta status_dia/status_rotulo; schema. |

Não encontrei migration de dados `RunPython`/`RunSQL` que reescreva `CelulaDia` na cópia; as operações encontradas são as listadas acima. Não encontrei uso de CelulaDia em admin/forms/serializers nem DML SQL de CelulaDia contra produção; universalidade fora da cópia [não verificado].

Há SQL fora da porta em `bin/sombra_incremental.sh:61-68`, com `escala_celuladia` na lista de tabelas (`:35`), mas o destino declarado é `PG=sombra_db`, `ALVO=sombra` (`:20`); a produção é fonte de SELECT/COPY TO (`:55-57`), não destino de escrita desse script.

| Contraexemplo/caminho | Respeita `origem='editada'`? |
|---|---|
| `regenerar_celulas_vinculo` | Sim para trabalha/DNA: ramo fora da vigência exige gerada (`app/ponto/portas/celula.py:232-239`); ramo normal só reescreve gerada (`:257-271`). |
| `regenerar_celulas_dia` | Delega à mesma regeneração, com intervalo de um dia e sem extensão (`app/ponto/portas/celula.py:514-517`). |
| `reverter_regeneracao_dia` | Sim: retorna False se origem não é gerada (`app/ponto/portas/celula.py:558-559`). |
| `gerar_celulas_periodo` | Sim: editada incrementa puladas e não entra nos ramos seguintes (`app/ponto/portas/celula.py:638-655`). |
| `lavrar_veredito` | Sem guarda de origem; pode relavrar derivados da editada. A proteção é decisão `veredito_via == 'admin'`, e ainda permite ata/status (`app/ponto/portas/celula.py:781-788`); grava veredito/ata/status (`:811-819`). Não escreve trabalha/DNA. |
| `carimbar` | Sem guarda de origem; escreve julgada_em/impressão/ausência/entrada/saída/insumos_em (`app/ponto/portas/celula.py:839-851`). |
| `carimbar_precedencia` | Sem guarda de origem; altera ata (`app/ponto/portas/celula.py:859-866`). |
| `carimbar_insumos_em_lote` | Sem guarda de origem; altera insumos quando diferentes (`app/ponto/portas/celula.py:878-887`). |
| `regenerar_celulas_vinculo(..., rejulgar=...)` | Nuance: a entrada em `_tocadas` exige célula existente e outras guardas, mas não origem (`app/ponto/portas/celula.py:291-298`). O julgamento posterior exclui por **veredito humano**, não por origem (`:390-402`). Assim não reescreve trabalha/DNA editados, mas esse ramo permite relavrar seus derivados. |
| Command `gerar_celulas` | Reexporta funções da porta (`app/ponto/management/commands/gerar_celulas.py:13`), chama geração periódica (`:310-312`); herda pulo da editada. |
| Command `corrigir_escala_retroativa` | Chama regeneração com barrados/motivo/apesar_da_lavra (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:229-238`); herda guarda da origem para DNA. |
| Command `aplicar_col900` | Chama reversão (`app/ponto/management/commands/aplicar_col900.py:77-78`) ou regeneração diária (`:115-119`); herda guardas. |
| Command `backfill_b5_insumos` | Seleciona células julgadas sem filtro de origem (`app/ponto/management/commands/backfill_b5_insumos.py:60-63`) e chama carimbo em lote (`:81`, `:84`); pode carimbar editada. |
| Command `processar_cartorio` | Carrega células passadas sem filtro de origem e passa a `julgar_colab` (`app/ponto/management/commands/processar_cartorio.py:61-65`); pode relavrar derivados. |
| Command `lavrar_status_competencia` | Carrega células da janela sem filtro de origem e chama julgamento com completar_status (`app/ponto/management/commands/lavrar_status_competencia.py:50-57`). |
| Command `curar_residuo_bug115` | Gera pela porta e força julgamento (`app/ponto/management/commands/curar_residuo_bug115.py:132-143`); geração protege editada, julgamento pode mudar derivados. |

Chamadores de lavra em produção: delegação `app/escala/models.py:1826-1828`; decisão humana `app/ponto/services/veredito_celula.py:330`; cartório `app/ponto/services/cartorio.py:889`, `:1242`, `:1802`. Todos chegam à porta, não são save independente.

## 06/11 10:13 · sessao 6c6818576678 · - · - · origem=Ronald (tmux)

Q6 — leitores que dependem de `trabalha`/DNA (a origem não é filtro nestes trechos).

| Leitor | Decisão afetada | Evidência |
|---|---|---|
| Autoridades `EscalaColaborador.eh_dia_trabalho`, `marcos_do_dia`, `pausas_do_dia`, `extra_declarada_do_dia` | Retornam respectivamente `cel.trabalha`, marcos congelados, todos os intervalos congelados e extra declarada. Leem independentemente de `origem`. | `app/escala/models.py:1087-1096`, `1117-1133`, `1182-1208`, `1227-1239` |
| `minutos_previstos_do_dia` / `intervalo_declarado_do_dia` | Dia não explicitamente de trabalho vale zero; DNA determina duração e intervalos. | `app/escala/utils.py:2229-2248`, `2277-2295`, `2312-2326` |
| `classificar_falta` | `trabalha=False` devolve `(None,None,None)`; DNA determina quais batidas/marcos eram esperados. | `app/escala/servico_jornada.py:279-304`, `427-435` |
| Classificador de procedência de falta | Sem batidas, pergunta ao vínculo: False → `improcedente`, True → `falta_real`, None → `ambiguo`. | `app/escala/servico_jornada.py:533-546` (nome da função deve ser confirmado ao redor da linha 500) |
| Precedência `fatos_do_periodo` | Havendo resposta de `cel.trabalha`, utiliza-a como `previsto`; afeta tratamento de feriado/trabalho. | `app/ponto/precedencia.py:357-377` (nome da função deve ser confirmado) |
| Porta de precedência para faltas/ausências | Veta lançamento de tipo que só cabe em jornada quando vínculo responde False. | `app/ponto/precedencia.py:536-543` |
| Gerador da grade `montar_grade_prevista_periodo` | Pula dia de folga, usa marcos/pausas congelados para alinhar batidas e medir previsto. Essa grade alimenta cartório. | `app/escala/utils.py:1619-1660`; chamada pelo cartório em `app/ponto/services/cartorio.py:1126` |
| Motor base, `calcular_mes` | `eh_dia_trabalho=False` manda o período para `periodos_ft`, caso contrário para `periodos`; folga também desativa janela de HE. | `app/ponto/motor_calculo_v2.py:2445-2459`, `2468-2494`, `1522-1536` |
| Motor: faltas e DSR | Dias previstos alimentam faltas injustificadas, quantidade de faltas, horas de falta e abono. | `app/ponto/motor_calculo_v2.py:2593-2622`; origem dos dias previstos em `app/ponto/services/fechamento.py:720-732` |
| Motor: jornada contínua com múltiplos pares | Marcos determinam se deve zerar atraso de pares intermediários e saída antecipada de pares anteriores ao último. | `app/ponto/motor_calculo_v2.py:2686-2716` |
| Motor turno partido, Zona 5 | DNA com os quatro marcos habilita e fornece régua de atraso/saída antecipada. | `app/ponto/motor_calculo_v2.py:3014-3049` |
| Motor: limite de HE diária | Marcos e primeiro intervalo definem limite diário; excesso vira HE50/HE100. | `app/ponto/motor_calculo_v2.py:3363-3398` |
| Motor: banco de horas | Trabalho previsto determina proporcionalização da jornada semanal e saldo. | `app/ponto/motor_calculo_v2.py:3420-3431` |
| `juiz_batida.periodos_do_dia` (usado por motor e folha) | Lê DNA e ata; DNA sem marcos, quantidade diferente ou horas divergentes retorna `desalinhado`, em vez de atribuir papéis às batidas. | `app/ponto/juiz_batida.py:149-174`, `196-204`, `226-235` |
| Alimentação do calculador | Consulta previsto/intervalo pelas autoridades e pergunta `eh_dia_trabalho` para classificar folga trabalhada. | `app/ponto/calculador/alimentacao.py:374-382`, `434-445`; regra pura em `app/ponto/calculador/regras.py:367-396` |
| Cartório | DNA compõe impressão de insumos, lampadas e ata; `trabalha=False` cala protestos de chamados; insumo trabalha+ata alimenta `classificar_dia`; resultado é lavrado. | `app/ponto/services/cartorio.py:443-456`, `565-586`, `1348-1364`, `1407-1424`, `1800-1807` |
| Cartório: emissão e furo parcial | Só examina marcos se vínculo responde trabalho=True; usa marcos do dia. | `app/ponto/services/cartorio.py:1718-1719`, `1853-1856` |
| Fechamento | Carrega células em lote, fornece mapa ao motor, monta datas previstas com `eh_dia_trabalho`, apura fatias e resultado. | `app/ponto/services/fechamento.py:595-604`, `720-746`, `269-282` |
| Fechamento: métricas derivadas da grade | Tipo do dia/previsto afetam dias previstos, minutos previstos, abonos e realizado; retentores viram `motivos_espelho`. | `app/ponto/services/fechamento.py:997-998`, `1030-1052` |
| Leitor soberano `grade_da_celula` | **Prefere ata existente**: `ata.tipo_dia` antes de trabalha e `ata.minutos_previstos` antes de consultar autoridade. Na ausência desses valores usa trabalha/DNA; transporta trabalha+ata ao suprajuiz. | `app/escala/services/leitor_celula.py:380-411` |
| Folha `grade_do_fechamento` | Abre a grade soberana da competência, portanto herda a prioridade da ata. | `app/folha/export.py:329-338` |
| Folha: retenção | Lê veredito/protestos lavrados; DNA também pode mudar resultado de `nao_retem_pela_pausa` porque esta consulta o juiz de períodos. Veredito None com trabalha não None em dia passado retém por `sem_celula`. | `app/folha/export.py:1140-1147`, `1165`, `1206`, `1233-1256` |
| Folha: rubricas | HE em TXT sai de campos já apurados do fechamento, portanto depende da reapuração; não recalcula diretamente o DNA nessa emissão. | `app/folha/export.py:420-446` |

Ressalvas indispensáveis:

1. Não se pode afirmar que todo leitor muda imediatamente quando a célula muda: grade soberana dá preferência à ata existente, e folha lê veredito/fechamento já lavrados. `app/escala/services/leitor_celula.py:380-395`; `app/folha/export.py:1233-1256`, `420-446`.
2. `impressao_insumos` recebe DNA e inclui `dna_versao`/`marcos`, mas não recebe `trabalha`; `_em_dia` pula julgamento se impressão/veredito já estão em dia e não há `forcar`. Assim, editar só `trabalha` não demonstra rejulgamento pelo cartório. `app/ponto/services/cartorio.py:443-456`, `1348-1364`.
3. A ocorrência efetiva de qualquer mudança em produção é `[não verificado]`; foram identificadas dependências no código, sem dados de banco/log.

## 06/11 09:49 · sessao a483d8cfa564 · PEP-REENTRADA · verificador · origem=Code

Execução dos testes e mutantes: **[nao verificado]**. As conclusões abaixo são por inspeção do código. [nao verificado]

1. **Os dois testes têm mordida local, com limites diferentes.** [nao verificado]

   No **laço sem credor**, retirar isoladamente a guarda deve permitir o carimbo da P e contrariar `assertIsNone`: o teste deixa sua validação nula e força o C6 a responder “morta”. A irmã sem destino comprova a passagem pelo segundo laço, pois fica fora do filtro do primeiro. (`app/chamados/tests/test_pep_reentrada.py:227`, `app/chamados/tests/test_pep_reentrada.py:229`, `app/chamados/tests/test_pep_reentrada.py:233`, `app/chamados/tests/test_pep_reentrada.py:239`; `app/chamados/services/disputa_emissao.py:1573`, `app/chamados/services/disputa_emissao.py:1665`, `app/chamados/services/disputa_emissao.py:1671`.)

   Entretanto, substituir essa guarda por **`if q.chamado_destino_id: continue`** também preservaria o teste: P tem destino; irmã não tem. Portanto, ele distingue essas duas perguntas, mas não prova sozinho que a distinção depende exclusivamente do voo. (`app/chamados/tests/test_pep_reentrada.py:58`, `app/chamados/tests/test_pep_reentrada.py:227`, `app/chamados/tests/test_pep_reentrada.py:238`; `app/chamados/services/disputa_emissao.py:1671`.)

   **O mock altera também a porta de escrita.** Seus leitores potenciais nessa chamada são:

   - **Laço 1:** importa a função mockada, mas a guarda de voo pula P antes de consultá-la. (`app/chamados/services/disputa_emissao.py:1577`, `app/chamados/services/disputa_emissao.py:1586`, `app/chamados/services/disputa_emissao.py:1592`.)
   - **`carimbar`:** importa novamente a mesma função e a consulta depois de aplicar o carimbo e os campos de resposta em memória. O mock força essa aceitação também. (`app/chamados/models.py:2576`, `app/chamados/models.py:2586`, `app/chamados/models.py:2596`.)
   - **Cauda:** C7 chega à função por `fio_vivo`, e `fechar` também a importa. Contudo, neste teste a guarda de voo impede essa avaliação; mesmo removendo-a, `respostas_sem_veredito` ainda considera o texto não validado da P. (`app/chamados/services/disputa_emissao.py:1710`; `app/chamados/models.py:1784`, `app/chamados/models.py:1794`, `app/chamados/models.py:1815`; `app/chamados/juizes.py:1242`.)

   Há diferença concreta para o código real: `resposta='responder'` preenche `respondida_em`, sem preencher `validada_em`. Na irmã criada sem célula, o C6 real não comprova concordância com a luz e mantém viva essa resposta, recusando o carimbo. O teste força o resultado contrário na porta; demonstra a guarda **sob C6 artificial**, não a integração real desse cenário. (`app/chamados/tests/test_pep_reentrada.py:227`, `app/chamados/tests/test_pep_reentrada.py:233`; `app/chamados/models.py:2587`, `app/chamados/models.py:2591`, `app/chamados/models.py:2600`; `app/chamados/juizes.py:890`, `app/chamados/juizes.py:1101`, `app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1162`.)

   No **S144**, a irmã corrige a vacuidade apontada: remover a guarda carimbaria P; desligar o receiver ou “pular todas” deixaria a irmã sem a via exigida; continuar bloqueando P depois do voo contrariaria o controle `[p.pk]`. As verificações acontecem dentro do bloco, pelo receiver síncrono. (`app/chamados/tests/test_pep_reentrada.py:252`, `app/chamados/tests/test_pep_reentrada.py:256`, `app/chamados/tests/test_pep_reentrada.py:257`, `app/chamados/tests/test_pep_reentrada.py:259`; `app/chamados/signals.py:139`; `app/chamados/services/materializacao.py:1399`, `app/chamados/services/materializacao.py:1405`.)

   **Mutantes ainda sobreviventes:** desligar S143 deixa os dois verdes — o primeiro chama o resolver diretamente; o segundo exercita S144. Além disso, ambos verificam apenas `materializada_em` da P durante o voo: uma escrita indevida apenas de `via_resolucao` ou `materializada_por`, seguida do `continue`, escaparia desses dois testes. Isso é limite desses testes, não afirmação de que tal escritor exista na cópia. (`app/chamados/signals.py:92`; `app/chamados/tests/test_pep_reentrada.py:235`, `app/chamados/tests/test_pep_reentrada.py:239`, `app/chamados/tests/test_pep_reentrada.py:253`, `app/chamados/tests/test_pep_reentrada.py:256`.)

2. **Há dois descartes incorretos e uma justificativa incompleta.**

   **Receiver S143 vivo:** o descarte está errado. `UM_escritor` exige destino encerrado, batida e carimbo único do materializador; tudo isso também ocorre com S143 desligado, porque o próprio materializador reconcilia o destino e carimba P. `S143_nao_fecha` igualmente observaria apenas o fechamento normal posterior, fora do voo. Compartilhar o fixture não prova que o receiver executou. (`app/chamados/tests/test_pep_reentrada.py:87`, `app/chamados/tests/test_pep_reentrada.py:91`, `app/chamados/tests/test_pep_reentrada.py:113`; `app/chamados/services/materializacao.py:746`, `app/chamados/services/materializacao.py:1251`, `app/chamados/services/materializacao.py:1352`.)

   **Divergência de documentação:** reconhecer e adiar não cumpre a instrução expressa de corrigir no mesmo commit. A tabela ainda diz “disputa fecha […] SEM juiz”, enquanto C7 está implementado e registrado. A divergência pode ser anterior à obra; o descarte como dispensável nesta entrega continua contrário à regra do próprio documento. (`app/docs/JUIZES.md:14`, `app/docs/JUIZES.md:54`; `app/chamados/models.py:1789`; `app/core/juizes.py:267`.)

   **Censo de escrita crua:** “só `_escrever_carimbo` e descarimbos” é incompleto para `via_resolucao`: existem duas portas adicionais que escrevem somente a via, inclusive reconhecidas pelo selo AST. Isso corrige a justificativa, mas não demonstra escritor adicional durante o voo. (`app/chamados/models.py:2673`; `app/chamados/services/relavra_via.py:123`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:51`.)

3. **Nos caminhos atuais examinados, não sobra outro escritor da P em voo; continuam existindo leitores.**

   **Quem escreve:** o materializador chama `carimbar`; as atribuições do trio ficam em `_escrever_carimbo`, e a persistência na porta. Os carimbos de identidade por paridade/colisão também pertencem ao materializador. A chamada de recusa no `finally` da unidade ocorre depois da saída do contexto de voo. (`app/chamados/services/materializacao.py:1251`, `app/chamados/services/materializacao.py:899`, `app/chamados/services/materializacao.py:1074`, `app/chamados/services/materializacao.py:1271`; `app/chamados/models.py:2556`, `app/chamados/models.py:2609`, `app/chamados/models.py:2644`.)

   **Outros escritores alcançáveis pela cadeia:** S143 pula P nos dois laços e barra sua cauda; S144 pula P antes de carimbar. O absolvedor do reconciliador exige validação nula; o descarimbo exige C6 vivo; o dedup também seleciona perguntas vivas. Os chamadores atuais materializam com validação gravada, que faz C6 retornar morto já no primeiro degrau. (`app/chamados/services/disputa_emissao.py:1586`, `app/chamados/services/disputa_emissao.py:1671`, `app/chamados/services/disputa_emissao.py:1710`; `app/chamados/services/materializacao.py:1399`, `app/chamados/services/materializacao.py:1343`; `app/chamados/services/validacao.py:396`; `app/chamados/services/vigia_carimbo.py:98`; `app/chamados/reconciliador.py:620`; `app/chamados/services/relavra_via.py:227`; `app/chamados/services/fio.py:935`; `app/chamados/juizes.py:1136`.)

   **Quem lê o trio nessa cadeia:**

   | Leitor | Campos e finalidade |
   |---|---|
   | S143 | Seleciona por `materializada_em`; o laço sem credor também lê a via para a trilha, depois da guarda de voo. (`app/chamados/services/disputa_emissao.py:1574`, `app/chamados/services/disputa_emissao.py:1665`, `app/chamados/services/disputa_emissao.py:1676`.) |
   | S144 | Seleciona por `materializada_em`, antes da guarda. (`app/chamados/services/materializacao.py:1392`.) |
   | `carimbar` | Lê os três campos para restaurá-los se houver recusa ou exceção. (`app/chamados/models.py:2583`, `app/chamados/models.py:2619`.) |
   | C6 e juiz do lastro | Leem instante/via; C6 da P validada retorna antes desses degraus. (`app/chamados/juizes.py:647`, `app/chamados/juizes.py:1136`, `app/chamados/juizes.py:1171`, `app/chamados/juizes.py:1207`.) |
   | C7 e crédito de irmãs | Leem `materializada_em` nas pendências; são chamados pela cadeia de fechamento/reconciliação. (`app/chamados/models.py:1784`; `app/chamados/juizes.py:1748`; `app/chamados/services/materializacao.py:737`.) |
   | Recusa | Relê instante e via antes de decidir o carimbo de colisão. (`app/chamados/services/materializacao.py:1064`.) |

   A exclusividade é dos caminhos examinados, não um bloqueio universal na porta: `materializacao_em_voo` registra o PK, mas `carimbar_identidade` e `descarimbar` não consultam esse sinal. (`app/chamados/services/materializacao.py:1120`, `app/chamados/services/materializacao.py:1129`; `app/chamados/models.py:2625`, `app/chamados/models.py:2646`.)

4. **No delta V3, a ressalva é de cobertura do selo, não uma nova infração demonstrada no código de produção.**

   Os dois testes mordem as remoções locais descritas; o S144 ganhou controle positivo efetivo. Persistem os limites do mock amplo, da identidade confundida com presença de destino e da conexão S143 sem prova positiva. Portanto, não os classifico como totalmente “sem mordida”, nem como prova completa do contrato. (`app/chamados/tests/test_pep_reentrada.py:227`, `app/chamados/tests/test_pep_reentrada.py:233`, `app/chamados/tests/test_pep_reentrada.py:235`, `app/chamados/tests/test_pep_reentrada.py:257`.)

   O delta fornecido altera testes; nele não identifiquei novo segundo juiz/escritor de produção, relógio fora da borda, derivação paralela, inversão de trava ou estado de módulo sobrevivente. No código completo, o voo mantém `reset` em `finally` e termina antes da cauda normal de fechamento. Pouso efetivo e efeitos em produção: **[nao verificado]**. (`app/chamados/tests/test_pep_reentrada.py:219`, `app/chamados/tests/test_pep_reentrada.py:241`; `app/chamados/services/materializacao.py:1123`, `app/chamados/services/materializacao.py:1194`, `app/chamados/services/materializacao.py:1352`.)

## 06/11 09:48 · sessao 4f4e1062af72 · - · - · origem=Ronald (tmux)

- **O teste sem credor morde a remoção isolada da guarda**, por análise estática: a P entra em `_abertas` com validação nula; sem o `continue`, o mock permite `carimbar`, contrariando `assertIsNone`. A irmã sem destino comprova execução desse laço, pois não participa do primeiro filtro por destino. (`app/chamados/tests/test_pep_reentrada.py:227`, `:229`, `:233`, `:239`; `app/chamados/services/disputa_emissao.py:1573`, `:1665`, `:1671`, `:1686`).

- **O mock substitui mais que a consulta desse laço.** O primeiro laço importa a função mockada; `carimbar` também a importa e consulta depois de aplicar os campos; C7 chega a ela por `fio_vivo`, e `fechar` importa a mesma função. Neste teste, as guardas de voo pulam a P no primeiro laço e na cauda. (`app/chamados/services/disputa_emissao.py:1577`, `:1586`, `:1592`, `:1710`; `app/chamados/models.py:2576`, `:2596`, `:1793`, `:1815`, `:1837`; `app/chamados/juizes.py:1242`).

- **Há comportamento real removido pelo mock:** `resposta='responder'` preenche `respondida_em`, mas não valida; o C6 real prioriza essa resposta e exige concordância com a luz. A irmã foi criada sem célula/destino; o teste não demonstra concordância real e força a aceitação da porta. Portanto, prova a guarda sob C6 artificial, sem demonstrar a integração real desse cenário. (`app/chamados/models.py:2587`, `:2591`, `:2596`; `app/chamados/juizes.py:1150`, `:1162`, `:1101`; `app/chamados/tests/test_pep_reentrada.py:227`, `:233`).

- **Mutante sobrevivente do primeiro teste:** substituir a guarda de voo do laço sem credor por `if q.chamado_destino_id: continue`. A P tem destino e a irmã não, preservando exatamente os dois asserts sem consultar voo. (`app/chamados/services/disputa_emissao.py:1671`; `app/chamados/tests/test_pep_reentrada.py:58`, `:227`, `:238`, `:239`).

- **O S144 ganhou mordida adequada para os mutantes indicados:** remover sua guarda carimba P; desligar receiver ou pular todas deixa a irmã sem `VIA_JUIZ_APROVOU`; manter o bloqueio depois do voo viola o retorno `[p.pk]`. São previsões estáticas, sem execução. (`app/chamados/services/materializacao.py:1399`, `:1405`; `app/chamados/signals.py:139`; `app/chamados/tests/test_pep_reentrada.py:252`, `:256`, `:257`, `:259`).

- **Os dois novos testes deixam sobreviver um S143 desligado:** o primeiro chama o resolver diretamente; o segundo depende do receiver S144. Também deixam sem aferição a preservação de `materializada_por` e `via_resolucao` da P durante o voo: verificam somente `materializada_em`. (`app/chamados/tests/test_pep_reentrada.py:235`, `:239`, `:253`, `:256`; `app/chamados/signals.py:92`, `:140`).

- **O descarte “S143_nao_fecha sem prova de receiver vivo” está errado.** O `UM_escritor` exige destino fechado, batida e único carimbo do materializador, resultado igualmente possível com S143 desligado. O próprio materializador reconcilia o destino, carimba P e chama a cauda depois da unidade; o teste de fecho apenas observaria seu fechamento normal fora do voo. (`app/chamados/tests/test_pep_reentrada.py:83`, `:87`, `:91`, `:108`, `:113`; `app/chamados/services/materializacao.py:746`, `:1251`, `:1350`, `:1352`; `app/chamados/signals.py:92`).

## 06/11 09:47 · sessao ebc4941dec11 · - · - · origem=Ronald (tmux)

Nos chamadores atuais, não achei segundo escritor da P em voo: a validação já está gravada antes de `_materializar_uma` em `app/chamados/services/validacao.py:395-424`; o laço normal exige `validada_em` não nulo em `app/chamados/services/materializacao.py:1343-1350`; o universo do passivo também exige validação em `app/chamados/services/vigia_carimbo.py:96-98`.

Escritores alcançados pelo bloco:

- Materializador: carimbo normal em `app/chamados/services/materializacao.py:1251-1252`; identidade por paridade em `app/chamados/services/materializacao.py:899`; identidade por colisão na porta de recusa em `app/chamados/services/materializacao.py:1064-1075`. As atribuições do trio passam por `app/chamados/models.py:2556-2560`; a persistência sai por `app/chamados/models.py:2609` ou `app/chamados/models.py:2644`.
- S143: primeiro laço guardado antes de `carimbar` em `app/chamados/services/disputa_emissao.py:1586-1587`, escrita em `app/chamados/services/disputa_emissao.py:1612-1613`; segundo laço guardado em `app/chamados/services/disputa_emissao.py:1671-1672`, escrita em `app/chamados/services/disputa_emissao.py:1686`; cauda guardada em `app/chamados/services/disputa_emissao.py:1710-1711`.
- S144: receiver síncrono em `app/chamados/signals.py:139-140`; guarda em `app/chamados/services/materializacao.py:1399-1400` antes da escrita em `app/chamados/services/materializacao.py:1405`.
- Absolvedor do reconciliador: potencial escrita em `app/chamados/reconciliador.py:586`, mas a seleção exige `validada_em` nulo em `app/chamados/reconciliador.py:617-620`, excluindo a P dos chamadores atuais.
- Lavra sem turno: potencial escrita em `app/chamados/reconciliador.py:405`, mas recusa texto em `app/chamados/reconciliador.py:364-365`; a P validada também é morta pelo C6 em `app/chamados/juizes.py:1135-1137`.
- Descarimbo da fábrica: o julgamento pode coletar carimbadas e chamar a porta em `app/chamados/services/fabrica_da_celula.py:118-125` e `app/chamados/services/fabrica_da_celula.py:163-166`; a porta só descarimba se o C6 der viva em `app/chamados/services/relavra_via.py:221-231`, impossível para a P validada pelo degrau inicial em `app/chamados/juizes.py:1135-1137`.
- Dedup do fio: só considera perguntas vivas pelo C6 em `app/chamados/services/fio.py:931-943`; portanto não alcança a P validada. A escrita de identidade existe em `app/chamados/services/fio.py:891-894`.

Leitores relevantes dentro da cadeia:

- S143 seleciona por `materializada_em` em `app/chamados/services/disputa_emissao.py:1572-1575`; o segundo laço lê `materializada_em` e `via_resolucao` em `app/chamados/services/disputa_emissao.py:1665-1678`.
- S144 seleciona por `materializada_em` em `app/chamados/services/materializacao.py:1392-1393`.
- A porta `carimbar` lê o trio anterior para desfazer em memória em `app/chamados/models.py:2583-2584`; aplica-o antes de consultar o C6 em `app/chamados/models.py:2586-2596`.
- C6 lê via e instante em `app/chamados/juizes.py:1145`, `app/chamados/juizes.py:1171`, `app/chamados/juizes.py:1177` e `app/chamados/juizes.py:1207`; a P validada retorna antes desses degraus em `app/chamados/juizes.py:1135-1137`.
- Juiz do lastro lê via/instante em `app/chamados/juizes.py:647-673`; o materializador o chama antes do carimbo em `app/chamados/services/materializacao.py:1206-1207`.
- C7 lê `materializada_em` para respostas sem veredito em `app/chamados/models.py:1783-1787`, depois consulta o fio em `app/chamados/models.py:1789-1794`.
- Crédito de irmãs lê instante na consulta em `app/chamados/juizes.py:1747-1753`; é chamado antes da reconciliação do destino em `app/chamados/services/materializacao.py:736-746`.
- Recusa relê instante/via em `app/chamados/services/materializacao.py:1064-1068`.

A justificativa da triagem “só `_escrever_carimbo` e descarimbos” é literalmente incompleta para `via_resolucao`: há escrita direta em `app/chamados/models.py:2673-2674`, usada pelo comando em `app/chamados/management/commands/encerrar_perguntas_com_fato.py:68`; e há re-lavra direta em `app/chamados/services/relavra_via.py:123-125`. Isso não demonstra segundo escritor dentro do bloco: o primeiro é comando, e a chamada reativa de re-lavra foi retirada, conforme a cadeia documentada em `app/ponto/signals.py:94-105`.

A exclusividade demonstrada vale para os chamadores atuais; `materializacao_em_voo` não é intertravamento universal das portas de escrita, pois apenas registra o PK e verifica atomicidade em `app/chamados/services/materializacao.py:1115-1129`, enquanto `carimbar_identidade` e `descarimbar` não consultam o sinal em `app/chamados/models.py:2625-2652`.

## 06/11 09:47 · sessao 0efc7137717a · AKITA-MERGE-DE-VOLTA · verificador · origem=Code

Referências **v2** indicam as linhas resultantes do patch; **base**, as da cópia.

1. **A cura fecha os dois contraexemplos de rename.** No commit simples, a origem apagada entra na lista. No merge plantado, `app/ponto/x.py` aparece como exclusão contra ambos os pais; portanto sobrevive à interseção e reprova `n/a`. (`bin/tests/test_lei_akita.sh:184`, `:197`, `:198`, v2; `bin/tests/test_lei_akita.sh:133`, base.)

   Cópia que mantém a origem não altera aquele caminho; cópia seguida de exclusão fica exposta com `--no-renames`. Typechange e alteração de modo continuam sendo diferenças do caminho: os comandos não filtram esses tipos. (`bin/tests/test_lei_akita.sh:184`, `:185`, `:197`, v2.)

   **Ressalva condicional: submódulos.** Os comandos não fixam `--ignore-submodules=none`, e o wrapper herda a configuração Git. Uma configuração que ignore submódulos pode ocultar alterações de gitlink; existência e reprodução desse caso aqui: **[nao verificado]**. Não é regressão demonstrada deste delta. (`bin/tests/test_lei_akita.sh:154`, base; `bin/tests/test_lei_akita.sh:184`, `:185`, `:197`, v2.)

2. **Não identifiquei commit legítimo que devesse passar e passe a falhar por esse flag.** A mudança relevante é negócio→docs/tests/fora de `app/`: agora a exclusão da origem conta, conforme o predicado existente. Negócio→negócio e docs→negócio já expunham um destino de negócio; movimentos inteiramente fora do negócio continuam passando. No merge, ainda se aplica a interseção. (`bin/tests/test_lei_akita.sh:123`, `:133`, base; `bin/tests/test_lei_akita.sh:184`, `:198`, v2.)

   **Quais hashes reais mudam isolando apenas esse flag: [nao verificado].** O agregado fornecido não contém essa comparação por commit. **[nao verificado]** [nao verificado]

3. **O plantio novo não apresenta vacuidade aparente.** `c('m3', …)` grava exatamente `m3\n`; o primeiro pai recebe `B` repetido 4.000 vezes; depois do merge, `rm -qf` exclui `x.py` e o arquivo em docs recebe exatamente `m3\n`. Assim, o resultado pretendido mantém a exclusão contra ambos os pais e a identidade A→A apenas contra o publicado. (`bin/tests/test_lei_akita.sh:246`, base; `bin/tests/test_lei_akita.sh:342`, `:346`, `:347`, `:348`, `:349`, v2.)

   `-X theirs` resolve o conflito intermediário; o `rm -f` posterior determina a exclusão final. O teste exige exatamente os três hashes esperados, incluindo esse merge. Erro de preparo imprime `MORDE_FALHOU`, que impede aprovação mesmo havendo `MORDE_OK`. (`bin/tests/test_lei_akita.sh:347`, `:348`, `:352`, `:354`, v2; `bin/tests/test_lei_akita.sh:241`, `:330`, base.)

## 06/11 09:46 · sessao acc844691f54 · - · - · origem=Ronald (tmux)

- **Pergunta 2:** não identifiquei falso positivo legítimo novo. O caso que muda é rename com origem de negócio e destino excluído, cuja origem sobreviva à interseção dos pais publicados; passa a reprovar `n/a`, como exige a regra existente. Rename negócio→negócio já tinha destino de negócio; docs/tests→negócio também; excluído→excluído continua sem negócio. Referências: `bin/tests/test_lei_akita.sh:123`, `bin/tests/test_lei_akita.sh:133–135`; patch, linhas novas `bin/tests/test_lei_akita.sh:184–199`. **Quais commits reais mudariam**, isolando apenas esse flag, permanece **[não verificado]**: o dado mede a cura inteira com zero falhas, não fornece comparação por commit.

- **Pergunta 3:** não há vacuidade aparente no caso novo. `c('m3', ..., x)` grava exatamente `m3\n` pela implementação de `c`; o primeiro pai recebe `B` repetido 4.000 vezes; o merge resolve conflitos com `-X theirs`, mas o `rm -qf` posterior registra a exclusão de `x`, e o arquivo em docs recebe `m3\n`. O resultado pretendido é, portanto, `x` ausente, docs=A, contra primeiro pai `x=B` e segundo publicado `x=A`. Referências: `bin/tests/test_lei_akita.sh:244–247`; patch, linhas novas `bin/tests/test_lei_akita.sh:342–350`.

- Essa diferença de conteúdos é deliberada: evita que o diff contra o primeiro pai reconheça B→A como rename; deixa A→A no diff contra o pai publicado exposto ao mutante sem `--no-renames`. O teste compara exatamente os três hashes esperados, incluindo o merge, e não apenas a quantidade de falhas. Referências: patch, linhas novas `bin/tests/test_lei_akita.sh:345–354`. A ocorrência medida de os dois mutantes sem flag falharem foi fornecida pelo usuário; reprodução local **[não verificado]**.

- Falha de preparo não vira aprovação silenciosa: `g()` emite `MORDE_FALHOU` para retorno não zero, e o shell exige `MORDE_OK` junto da ausência de `MORDE_FALHOU`. Referências: `bin/tests/test_lei_akita.sh:238–243`, `bin/tests/test_lei_akita.sh:329–331` (base).

## 06/11 09:46 · sessao 6154e5a50e09 · - · - · origem=Ronald (tmux)

- A cura fecha os dois renames descritos: a origem apagada permanece na lista inicial e no diff contra o pai publicado; a interseção preserva `app/ponto/x.py` porque ele existe nos dois pais e falta no merge. Referências: `bin/tests/test_lei_akita.sh:184`, `:191`, `:197`, `:198` **no arquivo resultante do delta**.

- Cópia que conserva a origem não modifica aquele caminho; cópia seguida de remoção deve aparecer como adição/remoção com `--no-renames`. Typechange e alteração de modo continuam sendo diferenças do caminho; não existe `--diff-filter` nos comandos. Essas conclusões decorrem da semântica esperada do Git, mas não foram executadas nesta cópia: **[não verificado]**; comandos em `bin/tests/test_lei_akita.sh:184`, `:185`, `:197` no delta.

- Há uma ressalva condicional para submódulos: o wrapper herda a configuração do Git (`bin/tests/test_lei_akita.sh:154`) e os comandos novos não fixam `--ignore-submodules=none` (`:184`, `:185`, `:197` no delta). Uma configuração que ignore submódulos pode ocultar alteração de gitlink; existência dessa configuração/submódulo e reprodução do efeito nesta cópia: **[não verificado]**. Não apresentaria isso como falha medida nem como regressão introduzida pela cura.

## 06/11 09:43 · sessao c300187d6afa · LEI-FIO-MUDO-B · verificador · origem=Code

**Os dois consertos da v3 estão corretos por leitura estática. A ressalva é o descarte amplo da variante sem resposta: o vizinho cobre partes dela, mas não a adoção dessa variante nem uma asserção da tela “Aguardando resposta”.** [app/chamados/tests/test_c20_fio_mudo.py:289; app/chamados/tests/test_c20_fio_mudo.py:304; app/chamados/tests/test_fio_le_o_juiz.py:105; app/chamados/tests/test_fio_le_o_juiz.py:235]

**1. Delta e alcance dos comentários**

- **Foto depois dos callbacks: corrigido.** O contexto interno `captureOnCommitCallbacks(execute=True)` termina antes de `_no_post`; `pilha` permanece ativa durante essa saída. A fotografia copia as listas antes das chamadas artificiais. Assim, um push observado nesses alvos durante o POST ou seus callbacks capturados entra na comparação de silêncio. [app/chamados/tests/test_c20_fio_mudo.py:289; app/chamados/tests/test_c20_fio_mudo.py:294; app/chamados/tests/test_c20_fio_mudo.py:303]
- **Controle positivo dos seis: corresponde ao comentário atual.** O laço percorre todos os alvos, resolve cada atributo no módulo e chama o resultado; o assert exige uma chamada em cada mock. Isso demonstra que **buscar aqueles seis símbolos no módulo durante o patch alcança os mocks**. Não demonstra passagem pelos chamadores reais, guardas ou transportes: as chamadas positivas são feitas pelo próprio teste, diretamente nos símbolos substituídos. [app/chamados/tests/test_c20_fio_mudo.py:231; app/chamados/tests/test_c20_fio_mudo.py:295; app/chamados/tests/test_c20_fio_mudo.py:304]
- **A ressalva sobre imports ficou correta.** `views_cobrar` mantém um alias importado no módulo e o chama depois; substituir o atributo em `fcm_utils` não substitui esse alias. O Validar do fio segue por `acao_validar_pergunta` e `validacao.validar_pergunta`; a adoção chama `reapontar_resolvedora`. Esses caminhos lidos não passam pela ação de cobrança citada. [app/chamados/views_cobrar.py:11; app/chamados/views_cobrar.py:44; app/chamados/services/fio.py:605; app/chamados/services/acoes_disputa.py:18; app/chamados/models.py:502]

**2. Descartes**

| Descarte | Conferência nas linhas |
|---|---|
| **Corrigir com `hora`: sustentado.** | A fixture cria `tipo_resposta='hora'`; o cenário fechado recebe resposta `07:55` e verifica ausência de Corrigir. O controle aberto exige os três botões. Portanto, o vizinho cobre justamente a condição que tornava a asserção do R08f, com `escolha`, insuficiente isoladamente. [app/chamados/tests/test_fio_le_o_juiz.py:66; app/chamados/tests/test_fio_le_o_juiz.py:170; app/chamados/tests/test_fio_le_o_juiz.py:189; app/chamados/tests/test_fio_le_o_juiz.py:228; app/chamados/tests/test_c20_fio_mudo.py:68] |
| **`sem_reabrir` na recusa de hora: sustentado.** | O vizinho usa a pergunta de hora respondida, substitui `_materializar_uma` por uma recusa que grava `recusa_motivo`, exige retirada da validação, disputa ainda fechada e trilha de devolução. É o ramo que encaminha `sem_reabrir` e chega a `reabrir=not sem_reabrir`. [app/chamados/tests/test_fio_le_o_juiz.py:249; app/chamados/tests/test_fio_le_o_juiz.py:252; app/chamados/tests/test_fio_le_o_juiz.py:257; app/chamados/services/fio.py:605; app/chamados/services/acoes_disputa.py:22; app/chamados/services/validacao.py:422; app/chamados/services/validacao.py:427] |
| **Sem resposta: descarte apenas parcial.** | O teste de `:235` faz POST sem resposta e exige somente ausência do log de validação. Outro teste verifica presença no fio e preservação do estado sem resposta. Nenhum desses casos executa a adoção de uma órfã sem resposta; tampouco há nesses asserts uma conferência da mensagem “Aguardando resposta”. O texto existe no template, condicionado à ausência de resposta. [app/chamados/tests/test_fio_le_o_juiz.py:105; app/chamados/tests/test_fio_le_o_juiz.py:109; app/chamados/tests/test_fio_le_o_juiz.py:235; app/chamados/tests/test_fio_le_o_juiz.py:243; app/templates/chamados/partials/modal_fio.html:212; app/templates/chamados/partials/modal_fio.html:314] |

Portanto, **não renovo os apontamentos de Corrigir e recusa de hora**. Já considerar inteiramente coberta a **reatada sem resposta** extrapola os testes citados: o R08f continua preenchendo a resposta antes da adoção, enquanto a seleção de candidatas não exige resposta. Isso é uma limitação de cobertura identificável, sem demonstração de defeito de produção nessa variante. [app/chamados/tests/test_c20_fio_mudo.py:253; app/chamados/models.py:478; app/chamados/models.py:492]

**3. Quem escreve e quem lê os nomes envolvidos no delta**

| Nome | Quem escreve/define | Quem lê |
|---|---|---|
| `importlib` | Import local do teste. [app/chamados/tests/test_c20_fio_mudo.py:287] | Resolução do módulo de cada alvo. [app/chamados/tests/test_c20_fio_mudo.py:298] | [nao verificado]
| `alvo` | Iteração sobre `_CANOS_DE_PUSH`. [app/chamados/tests/test_c20_fio_mudo.py:296] | `rsplit` que separa módulo e atributo. [app/chamados/tests/test_c20_fio_mudo.py:297] |
| `_mod`, `_nome` | Desempacotamento de `alvo.rsplit`. [app/chamados/tests/test_c20_fio_mudo.py:297] | `import_module` e `getattr`, respectivamente. [app/chamados/tests/test_c20_fio_mudo.py:298] |
| `_CANOS_DE_PUSH` | Tupla da classe; o delta muda seu comentário. [app/chamados/tests/test_c20_fio_mudo.py:217] | Montagem dos patches, novo controle positivo e tamanho esperado nos asserts. [app/chamados/tests/test_c20_fio_mudo.py:231; app/chamados/tests/test_c20_fio_mudo.py:296; app/chamados/tests/test_c20_fio_mudo.py:303; app/chamados/tests/test_c20_fio_mudo.py:304] |
| `pilha` | `_push_gravado` cria `ExitStack`, instala os patches e o devolve; o teste recebe o objeto. [app/chamados/tests/test_c20_fio_mudo.py:230; app/chamados/tests/test_c20_fio_mudo.py:232; app/chamados/tests/test_c20_fio_mudo.py:288] | `with pilha` delimita a restauração dos atributos. [app/chamados/tests/test_c20_fio_mudo.py:289] |
| `pushes` | Recebe a lista de mocks montada por `_push_gravado`. [app/chamados/tests/test_c20_fio_mudo.py:231; app/chamados/tests/test_c20_fio_mudo.py:288] | Fotografia e contagem final dos seis controles. [app/chamados/tests/test_c20_fio_mudo.py:294; app/chamados/tests/test_c20_fio_mudo.py:304] |
| `m` | Variável local das duas compreensões sobre `pushes`. [app/chamados/tests/test_c20_fio_mudo.py:294; app/chamados/tests/test_c20_fio_mudo.py:304] | Acesso ao histórico de cada mock nessas mesmas expressões. [app/chamados/tests/test_c20_fio_mudo.py:294; app/chamados/tests/test_c20_fio_mudo.py:304] |
| `_no_post` | Cópia dos históricos após os callbacks e antes do controle positivo. [app/chamados/tests/test_c20_fio_mudo.py:294] | Assert que exige seis históricos vazios. [app/chamados/tests/test_c20_fio_mudo.py:303] |
| `call_args_list` | Histórico mantido pelos mocks instalados pelo helper, alimentado pelas chamadas interceptadas e pelas chamadas artificiais. [app/chamados/tests/test_c20_fio_mudo.py:231; app/chamados/tests/test_c20_fio_mudo.py:298] | Fotografia e contagem final. [app/chamados/tests/test_c20_fio_mudo.py:294; app/chamados/tests/test_c20_fio_mudo.py:304] |

Quanto aos **seis atributos substituídos**, o escritor temporário é o mesmo `mock.patch`; o novo leitor comum é o `getattr` do controle positivo. [app/chamados/tests/test_c20_fio_mudo.py:231; app/chamados/tests/test_c20_fio_mudo.py:298]

| Atributo | Definição original e leitores da cópia |
|---|---|
| `chamados.fcm_utils.push_fcm_colaborador` | Definido em FCM; exemplo de leitor com alias preexistente em `views_cobrar`. [app/chamados/fcm_utils.py:101; app/chamados/views_cobrar.py:11; app/chamados/views_cobrar.py:44] |
| `chamados.push_utils.push_colaborador` | Definido em `push_utils`; exemplo de import e chamada locais em `acoes_disputa`. [app/chamados/push_utils.py:114; app/chamados/services/acoes_disputa.py:147; app/chamados/services/acoes_disputa.py:150] |
| `ponto.webpush.push_web_colaborador` | Definido em `ponto.webpush`; lido pelo novo controle. [app/ponto/webpush.py:24; app/chamados/tests/test_c20_fio_mudo.py:223; app/chamados/tests/test_c20_fio_mudo.py:298] |
| `chamados.fcm_utils.push_fcm` | Definido em FCM; chamado pelo wrapper de colaborador após a guarda de token. [app/chamados/fcm_utils.py:49; app/chamados/fcm_utils.py:104; app/chamados/fcm_utils.py:106] |
| `chamados.push_utils._enviar_push_usuario` | Definido em `push_utils`; chamado pelo wrapper após a guarda de usuário. [app/chamados/push_utils.py:12; app/chamados/push_utils.py:116; app/chamados/push_utils.py:118] |
| `pywebpush.webpush` | Implementação da dependência: **[nao verificado]**. Leitores locais importam e chamam esse símbolo nos dois transportes Web Push. [app/chamados/push_utils.py:16; app/chamados/push_utils.py:40; app/ponto/webpush.py:41; app/ponto/webpush.py:50] |

Execução verde nesta sessão, sobrevivência experimental de mutantes e efeitos efetivos em produção: **[nao verificado]**. [nao verificado]

## 06/11 09:42 · sessao c5a68eae791b · AKITA-MERGE-DE-VOLTA · verificador · origem=Code

**Não aprovaria como está: a interseção por nomes admite um furo com renomeação.** O filtro novo presume que ausência no diff significa igualdade naquele caminho; essa premissa pode falhar com `--name-only` e renames (`bin/tests/test_lei_akita.sh:183`; anexo → `bin/tests/test_lei_akita.sh:196–197`).

Abaixo, **“novo” indica a numeração resultante do delta fornecido**.

1. **Há um contraexemplo que deve continuar vermelho.**

   Considere este merge hipotético: primeiro pai contém `app/x.py=B`; segundo pai, publicado, contém `app/x.py=A`; resultado remove `app/x.py` e cria `app/docs/x.py=A`. Escolha B suficientemente diferente de A para não haver rename detectado contra o primeiro pai. Reprodução desse cenário com Git: **[nao verificado]**.

   Com detecção de rename ativa, o diff contra o primeiro pai pode listar a deleção `app/x.py` e a adição documental; contra o publicado, o rename exato pode listar somente o destino `app/docs/x.py`. A interseção elimina a deleção de negócio, embora **ambos os pais ainda tenham `app/x.py`**. O juiz antigo reprova pelo caminho de negócio; o novo aceita `n/a` porque sobra apenas documentação (`bin/tests/test_lei_akita.sh:123–135`, `:183–185`; novo `bin/tests/test_lei_akita.sh:196–198`).

   A correção indicada é usar `--no-renames` nos dois diffs, ou contabilizar origem e destino. Há precedente no repo que inclui ambas as pontas ao tratar renames (`bin/tests/test_lei_protege_sitio.sh:104–109`).

   Para caminhos contabilizados integralmente, os demais casos são coerentes com a exceção proposta:

   - **Pouso main←raia e primeiro pai publicado:** o diff inicial já representa a comparação com esse pai; intersectá-lo novamente seria redundante. Pais adicionais não publicados não alteram a lista (`bin/tests/test_lei_akita.sh:183`; novo `bin/tests/test_lei_akita.sh:190–197`).
   - **Octopus:** percorre todos os pais adicionais; basta igualdade com um publicado para retirar o caminho. Não exige igualdade com todos eles (novo `bin/tests/test_lei_akita.sh:190–197`).
   - **Resolução de conflito:** resultado diferente de todos os pais publicados permanece julgado; escolher integralmente uma versão publicada fica dispensado, conforme a exceção proposta (novo `bin/tests/test_lei_akita.sh:186–198`).
   - **Raia publicada por outro caminho:** a ancestralidade é consultada contra `origin/main`, sem restringir ao primeiro pai dessa história (novo `bin/tests/test_lei_akita.sh:191–195`).

   Reintroduzir uma versão antiga de pai publicado também fica dispensado. Isso decorre expressamente do critério proposto; o cabeçalho não estabelece uma exigência adicional de igualdade com o **main vigente** (`bin/tests/test_lei_akita.sh:58–62`; novo `bin/tests/test_lei_akita.sh:186–197`).

2. **QUEM ESCREVE e QUEM LÊ.**

   `falhas_do_universo` produz a lista; `julgar → _julgar_linha → negocio` a consome. Externamente saem defeitos e contadores, sem exportação da lista (`bin/tests/test_lei_akita.sh:180–187`, `:122–146`, `:317–321`).

   O pre-push chama esse mesmo selo com `LEI_AKITA_HEAD=PUSH_SHA` e barra retorno não zero; a régua executa o mesmo arquivo pelo glob de selos (`bin/pre-push.sh:33–38`; `bin/regua.sh:138–145`). O instalador atual escreve um wrapper que delega a `bin/pre-push.sh`; conteúdo do hook efetivamente instalado: **[nao verificado]** (`bin/hooks.sh:49–56`).

   Existe a cópia antiga `app/core/hooks/pre-push.sh`, identificada pelo próprio repo como migração que não pousou (`bin/tests/test_recursos_sourced.sh:26–30`). Não encontrei outro juiz desse predicado; inexistência absoluta de implementação equivalente: **[nao verificado]**.

3. **O novo MORDE morde os dois mutantes pedidos, por leitura.**

   | Mutante | Por que falha |
   |---|---|
   | “Merge não é julgado” | Falta a volta maligna no conjunto exigido. |
   | “Nenhum pai é publicado” | A volta limpa também reprova, sobrando um hash indevido. | [nao verificado]

   A expectativa é exatamente `{maligna}` (novo `bin/tests/test_lei_akita.sh:335–338`).

   Sobrevive um mutante plausível: **examinar apenas o segundo pai**, ignorando terceiro e seguintes; o plantio usa merges de dois pais. Também não há nesse plantio rename nem injeção de erro do `merge-base` (novo `bin/tests/test_lei_akita.sh:317–338`). O caso antigo já exige um pouso vermelho quando a raia traz negócio, protegendo contra dispensar merges indiscriminadamente (`bin/tests/test_lei_akita.sh:280–293`).

4. **Sim: rc do `merge-base` fora de 0/1 gera `PADRAO_ERRO` e impede verde silencioso nesse caminho.** [nao verificado]

   O delta lança `SystemExit` explicitamente; os demais comandos novos passam pelo wrapper estrito (novo `bin/tests/test_lei_akita.sh:190–196`; base `bin/tests/test_lei_akita.sh:153–159`). A interrupção impede `PADRAO_FIM`, cuja ausência incrementa falhas e leva à saída vermelha (`bin/tests/test_lei_akita.sh:317–326`, `:333–337`).

Execução do MORDE: **[nao verificado]**; seu preparo cria arquivos e repositórios, incompatível com a regra de somente leitura (`bin/tests/test_lei_akita.sh:237–255`).

## 06/11 09:40 · sessao 076be9594384 · - · - · origem=Ronald (tmux)

Achado principal: **renomeação pode invalidar a inferência “sumiu do diff = igual ao pai publicado”**. Hipótese: P1 contém `app/x.py=B`; P2 publicado contém `app/x.py=A`; H remove `app/x.py` e cria `app/docs/x.py=A`. Sem detectar rename contra P1, a lista contém deleção de negócio; detectando rename exato contra P2, `--name-only` pode listar apenas o destino documental. A interseção então elimina a deleção e permite `n/a`. A execução dessa hipótese está `[nao verificado]`; o risco decorre da interseção por nomes no delta proposto `bin/tests/test_lei_akita.sh:196-197`, da classificação em `bin/tests/test_lei_akita.sh:122-135` e da ausência de `--no-renames` em `bin/tests/test_lei_akita.sh:183`. Correção sugerida: desligar renames em **ambos** os diffs, ou considerar ambas as pontas.

Os demais casos têm leitura consistente com a regra proposta:

- Primeiro pai publicado: já está representado pelo diff inicial; intersectá-lo outra vez seria idempotente. Pouso com raia não publicada mantém o julgamento atual. `bin/tests/test_lei_akita.sh:180-185`; delta proposto `:190-197`.
- Octopus: examina todos os pais adicionais e mantém apenas caminhos diferentes de cada pai publicado. Resolução diferente de todos eles permanece; escolha integral de versão publicada sai deliberadamente. Delta proposto `:190-197`. [nao verificado]
- Raia publicada por outro caminho: `merge-base --is-ancestor` não depende do nome do ramo nem de primeiro pai. Delta proposto `:191-195`. [nao verificado]
- Ressuscitar versão antiga de pai publicado é **ambiguidade de política**, não contraexemplo inequívoco à exceção declarada: a versão coincide integralmente com pai publicado, precisamente o critério do delta. O cabeçalho define universo de commits, sem definir atualidade de blobs, em `bin/tests/test_lei_akita.sh:58-62`; o juiz não verifica semanticamente a cura, conforme `:7-8`.

O novo MORDE mata “merges não são julgados” porque faltaria `maligna`, e “nenhum pai é publicado” porque sobraria também `limpa`; compara exatamente `{maligna}` no delta proposto `:335-338`. Sobrevivem “examinar só o segundo pai/parar no primeiro publicado”, ausência de tratamento de erro do `merge-base`, e o problema de renomeação: não há octopus, injeção desse erro ou rename no novo plantio, delta proposto `:317-338`. O mutante “todos os pais são publicados” é morto pelo MORDE **já existente**, cujo `pouso` deve continuar vermelho: `bin/tests/test_lei_akita.sh:280` e `:288-293`.

RC do novo `merge-base` fora de 0/1 gera explicitamente `PADRAO_ERRO`, delta proposto `:193-194`. A interrupção impede `PADRAO_FIM`, exigido pelo envelope em `bin/tests/test_lei_akita.sh:317-326`; portanto não fica verde silencioso nesse caminho. Os demais novos comandos passam pelo wrapper estrito em `bin/tests/test_lei_akita.sh:153-159`.

## 06/11 09:39 · sessao 4b900836e2b1 · PEP-REENTRADA · verificador · origem=Code

**A v2 protege a P corrente nos caminhos síncronos demonstrados de S143 e S144. Não encontrei outro escritor automático dessa P nesses caminhos; isso não prova exclusão em chamadas arbitrárias, pois a proteção está nos chamadores, não na porta.** As guardas estão em `app/chamados/services/disputa_emissao.py:1586`, `:1671`, `:1710` e `app/chamados/services/materializacao.py:1399`; `carimbar` não consulta o sinal (`app/chamados/models.py:2562`).

**Os testes melhoraram, mas ainda deixam passar mutantes relevantes, especialmente S144 desligado e S144 “pula todas”.** O controle posterior chama a função diretamente e não inclui irmã fora de voo (`app/chamados/tests/test_pep_reentrada.py:223–229`). Resultados de execução dos testes e mutantes: **[nao verificado]**.

1. **Quem escreve, quem lê e quais caminhos alcançam o bloco**

O censo abaixo trata dos caminhos de aplicação; os testes novos são examinados no item 4. Chamadas dinâmicas externas à cópia: **[nao verificado]**. [nao verificado]

| Estado | Escritores encontrados | Leitores relevantes |
|---|---|---|
| Trio da P: `materializada_em`, `materializada_por`, `via_resolucao` | A atribuição conjunta está em `_escrever_carimbo`; persistem `carimbar`, `carimbar_identidade`, `descarimbar` e `descarimbar_em_lote` (`app/chamados/models.py:2556`, `:2609`, `:2644`, `:2652`, `:2663`). | Lastro e C6 leem materialização/via; `.estado`, seleção do materializador, S143 e S144 também (`app/chamados/juizes.py:647`, `:673`, `:1171`, `:1207`; `app/chamados/models.py:2501`; `app/chamados/services/materializacao.py:1343`, `:1392`; `app/chamados/services/disputa_emissao.py:1572`). |
| Somente `via_resolucao` | Há escritores preexistentes fora de `carimbar`: `encerrar_por_fato_no_espelho` e `relavra_via.relavrar` (`app/chamados/models.py:2673`; `app/chamados/services/relavra_via.py:123`). | C6, explicador e apresentação leem a via (`app/chamados/juizes.py:1171`; `app/chamados/services/explicador.py:95`; `app/templates/chamados/partials/_card_atendimento.html:186`). |
| Autor da materialização | Além da escrita conjunta, é lido para procedência, fotografia anterior e auditoria (`app/chamados/models.py:2559`, `:2583`; `app/ponto/services/procedencia_batida.py:71`; `app/chamados/reconciliador.py:1134`). | A procedência escolhe `validada_por` ou `materializada_por` (`app/ponto/services/procedencia_batida.py:72`). |
| D: `fechada_em`, `resolucao` | `fechar` preenche ambos; `reabrir_admin` e `reabrir_sistema` zeram ambos; limpeza residual zera somente resolução (`app/chamados/models.py:1861–1865`, `:2063–2075`, `:2087–2091`, `:2173–2174`). | `.aberta`, idempotência de `fechar`, receivers, caudas e apresentação (`app/chamados/models.py:1765`, `:1820`; `app/chamados/signals.py:106`, `:130`; `app/chamados/services/materializacao.py:1290`, `:1302`; `app/templates/chamados/partials/modal_fio.html:374`). |
| `_EM_VOO` | Declaração no módulo, `set` na entrada e `reset(token)` no `finally` (`app/chamados/services/materializacao.py:1106`, `:1120`, `:1124`). | `pergunta_em_voo` e, por ele, as três guardas S143 e a guarda S144; o teste da cauda lê diretamente o conjunto (`app/chamados/services/materializacao.py:1129`, `:1399`; `app/chamados/services/disputa_emissao.py:1586`, `:1671`, `:1710`; `app/chamados/tests/test_pep_reentrada.py:109`). |

**Cada chamador de `PerguntaDisputa.carimbar`:**

| Chamador | Alcance dentro do bloco e efeito sobre a P corrente |
|---|---|
| Materializador, `app/chamados/services/materializacao.py:1251` | **Sim:** é o escritor esperado, após lastro aprovado, usando declaração ou via derivada do fato (`:1207`, `:1235–1252`). |
| S143, `app/chamados/services/disputa_emissao.py:1612` | **Sim**, pelo fecho do destino e receiver; pula a P antes do julgamento/carimbo (`app/chamados/services/materializacao.py:949`; `app/chamados/signals.py:91–92`; `app/chamados/services/disputa_emissao.py:1586`). |
| S143 sem credor, `app/chamados/services/disputa_emissao.py:1686` | A varredura é alcançável, mas exclui perguntas validadas e ainda tem guarda explícita de voo (`:1665–1672`). |
| S144, `app/chamados/services/materializacao.py:1405` | **Sim**, por qualquer fecho síncrono de disputa; pula a P corrente (`app/chamados/signals.py:139–140`; `app/chamados/services/materializacao.py:1399`). |
| Reconciliador sem turno, `app/chamados/reconciliador.py:405` | **Sim para irmãs**, pela batida. A P-hora corrente tem texto obrigatório e é barrada nesse reconciliador (`app/ponto/signals.py:215`; `app/chamados/reconciliador.py:1417`, `:1603`, `:364`; `app/chamados/services/materializacao.py:804`). |
| Reconciliador por célula, `app/chamados/reconciliador.py:586` | **Sim para irmãs**, pela batida; sua seleção exclui a P validada corrente (`app/ponto/signals.py:215`; `app/chamados/reconciliador.py:1416`, `:620`; `app/chamados/services/materializacao.py:1345`). |
| Absorção de ausência, `app/chamados/services/materializacao.py:549` | Vem da porta de resposta explícita. Não foi demonstrada chamada dessa porta pelos receivers síncronos do plantio; alcance adicional **[nao verificado]** (`app/chamados/services/disputa_emissao.py:1327–1330`). |
| Wrapper fantasma, `app/chamados/models.py:2685` | Chamado pelo consumidor de eventos, não pela cadeia síncrona de batida examinada (`app/chamados/reconciliador.py:1136`, `:1363`; `app/config/crons.py:394`). |

**Cada chamador de `DisputaSupervisao.fechar`:**

| Chamador | Alcance durante o bloco / tratamento da P em voo |
|---|---|
| S143, `app/chamados/services/disputa_emissao.py:1713` | Síncrono; **não fecha disputa que contenha P em voo**, pela guarda `:1710`. |
| Reconciliador, `app/chamados/reconciliador.py:412` | Pode fechar por uma irmã no signal da batida; o S144 resultante pula a P corrente (`app/ponto/signals.py:215`; `app/chamados/services/materializacao.py:1399`). |
| Reconciliador, `app/chamados/reconciliador.py:602` | Mesmo alcance, por irmã absolvida pela célula; S144 pula a P corrente (`app/chamados/reconciliador.py:1416`; `app/chamados/services/materializacao.py:1399`). |
| Cauda do materializador, `app/chamados/services/materializacao.py:1307` | Executa **depois** da unidade e da saída do sinal (`:1350–1353`). |
| Absorção, `app/chamados/services/materializacao.py:568` | Porta de resposta explícita; alcance pelo plantio **[nao verificado]** (`app/chamados/services/disputa_emissao.py:1330`). |
| Becos, `app/chamados/services/becos.py:52` | O gancho da pergunta usa `on_commit`, portanto executa depois do bloco normal (`app/chamados/signals.py:197–202`). |
| Fantasmas, `app/chamados/reconciliador.py:1160` | Consumidor de eventos separado (`:1363`; `app/config/crons.py:394`). |
| Emissão manual, `app/chamados/services/disputa_emissao.py:1118` | Alcance síncrono pelo plantio **[nao verificado]**. A fábrica chama outra porta, `criar_ou_atualizar_disputa` (`app/chamados/services/fabrica_da_celula.py:139`). |
| Tranca, `app/chamados/services/competencia_trancada.py:204` | Alcance pelo plantio **[nao verificado]**; se fechasse dentro dele, o S144 ainda passaria pela guarda (`app/chamados/services/materializacao.py:1399`). |
| Desligamento, `app/colaboradores/services/desligamento.py:142` | Alcance pelo plantio **[nao verificado]**; mesmo tratamento S144 (`app/chamados/services/materializacao.py:1399`). |
| Entrada sincronizada, `app/ponto/services/entrada_sincronizada.py:59` | Chamador API; alcance inverso pelo materializador **[nao verificado]** (`app/api/views.py:805`). |
| Foto recebida, `app/api/foto_recebida.py:80` | Chamador API; alcance inverso pelo materializador **[nao verificado]** (`app/api/views.py:871`). |
| Comando, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88` | Execução separada; chamada pelo plantio **[nao verificado]**. |
| Wrapper `admin_encerra`, `app/chamados/models.py:1879` | Alcance pelo plantio **[nao verificado]**. |
| Wrapper `remover_motivo`, `app/chamados/models.py:1986` | Associado aos eventos de posto e âncora; alcance pelo plantio **[nao verificado]** (`app/colaboradores/signals.py:77`; `app/escala/signals.py:161`). |

O preenchimento efetivo de `fechada_em` encontrado na aplicação continua sendo o save da porta `fechar`; as outras escritas encontradas o zeram nas reaberturas (`app/chamados/models.py:1865`, `:2075`, `:2091`).

**Ressalva sobre “pela porta `carimbar`”:** o próprio materializador já possui encerramentos por identidade — paridade e colisão — usando `carimbar_identidade`. Não são um segundo escritor automático introduzido pela obra, mas impedem afirmar que *todo* encerramento durante o voo usa literalmente `carimbar` (`app/chamados/services/materializacao.py:899`, `:1074`).

Também existe caminho do cartório para descarimbo, mas ele exige C6 vivo; a P corrente validada é morta no primeiro degrau do C6 e não passa nessa condição (`app/chamados/services/fabrica_da_celula.py:166`; `app/chamados/services/relavra_via.py:227–231`; `app/chamados/juizes.py:1136–1137`).

2. **S144: outros chamadores e eventual falta de carimbo**

O único chamador direto de aplicação encontrado é o receiver; existem chamadas diretas nos testes de porta e no teste novo (`app/chamados/signals.py:139–140`; `app/chamados/tests/test_porta_desfecho_pergunta.py:226`, `:244`, `:248`, `:263`, `:292`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:691`; `app/chamados/tests/test_pep_reentrada.py:228`).

A premissa **“S144 roda só uma vez por fecho” está errada**: o receiver aceita qualquer `post_save` de disputa fechada, sem comparar estado anterior ou `update_fields`. Uma gravação posterior pode acioná-lo novamente; que ela ocorrerá é **[nao verificado]** (`app/chamados/signals.py:124–140`).

No cenário proposto — fecho durante o plantio, S144 pula, materializador reprova — **o fecho ocorrido dentro daquele atomic também volta**. Tanto a reprovação do lastro quanto a recusa de `carimbar` levantam `_Reprovada` dentro do mesmo bloco (`app/chamados/services/materializacao.py:1194`, `:1208–1217`, `:1251–1258`).

Os desfechos demonstráveis são:

- **Sucesso:** o materializador carimba pela via dele antes de sair (`app/chamados/services/materializacao.py:1235–1256`).
- **Rollback com lastro anterior que permaneça aprovado:** a cauda pode fechar depois do sinal e o S144 carimbar por `VIA_JUIZ_APROVOU`, condicionado aos juízes aceitarem (`app/chamados/services/materializacao.py:1287–1307`, `:1352`, `:1401–1405`).
- **Recusa persistente:** não há garantia de carimbo eventual. O `continue` não agenda repetição; novas tentativas existem pelas portas humanas, mas sua execução futura é **[nao verificado]** (`app/chamados/services/materializacao.py:1399–1400`; `app/chamados/services/acoes_disputa.py:139`; `app/chamados/services/fio.py:666`; `app/chamados/services/validacao.py:430`).

Portanto, **não há demonstração de orfandade permanente causada pelo exemplo de rollback**, nem base para prometer “alguém sempre carimba depois”. A própria validação também pode devolver a validação recusada e reabrir a disputa, em vez de esconder a recusa com um carimbo (`app/chamados/services/validacao.py:424–427`).

3. **Escopo do sinal**

| Situação | Resultado pela implementação |
|---|---|
| Sucesso/commit | O atomic entra primeiro; o sinal entra depois e é restaurado antes da saída do atomic (`app/chamados/services/materializacao.py:1194`, `:1124`). |
| `_Reprovada`, exceção ou retorno antecipado | Todos atravessam o `finally` do contexto; a restauração ocorre antes do tratamento externo (`app/chamados/services/materializacao.py:1121–1124`, `:1201`, `:1257–1260`). |
| Savepoint interno desfeito e exceção absorvida | A P externa **continua em voo**, porque seu contexto externo ainda está ativo. O S143 tem precisamente savepoint interno com exceção absorvida (`app/chamados/signals.py:88–94`; `app/chamados/services/materializacao.py:1124`). |
| Outra P aninhada | A união mantém ambas; o token restaura o conjunto externo na saída (`app/chamados/services/materializacao.py:1120–1124`). |
| Mesma P aninhada | O token restaura a presença externa da mesma PK; não há remoção simples que a derrube (`app/chamados/services/materializacao.py:1120–1124`). |
| `on_commit` real depois do bloco | Encontra o sinal restaurado pela ordem dos managers (`app/chamados/services/materializacao.py:1194`, `:1124`; gancho em `app/chamados/signals.py:202`). |
| Outra thread/request | Não há armazenamento explícito do conjunto em variável global mutável ou thread-local: há `ContextVar` com conjunto imutável e reset. Comportamento efetivo entre requests/threads: **[nao verificado]** (`app/chamados/services/materializacao.py:1106`, `:1120–1124`). |

A garantia é **lexical**, não uma identidade de transação armazenada: o valor contém PKs, e a consulta verifica apenas se há atomic naquele momento (`app/chamados/services/materializacao.py:1111`, `:1120`, `:1129`). Uma cópia de contexto não seria invalidada pelo reset do contexto original; caminho concreto que faça essa cópia durante o voo: **[nao verificado]**.

Não encontrei sobrevivência no contexto corrente após saída normal, rollback ou exceção; afirmar ausência universal de sobrevivência em contexto transportado excederia o código demonstrado (`app/chamados/services/materializacao.py:1124`).

4. **Os testes mordem?** [nao verificado]

A tabela apresenta **mutantes previstos por leitura**, não mutantes executados. “Passa” abaixo é **[nao verificado]**; alguns sobrevivem apenas ao teste isolado e são mortos por outro teste do arquivo. [nao verificado]

| Teste | Mordida e mutante que pode escapar | [nao verificado]
|---|---|
| `UM_escritor`, `app/chamados/tests/test_pep_reentrada.py:83` | Conta carimbos bem-sucedidos e exige uma declaração. Uma segunda escrita crua da P por `save/update` escapa do spy, que observa somente `carimbar` (`:69–72`, `:90–92`). |
| `via_gravada`, `app/chamados/tests/test_pep_reentrada.py:94` | Verifica via/autor finais. Retirar a guarda S143 pode permitir escrita antecipada seguida de sobrescrita correta pelo materializador, preservando essas asserções (`:98–99`; `app/chamados/services/materializacao.py:1251`). |
| `S143_nao_fecha`, `app/chamados/tests/test_pep_reentrada.py:101` | Agora exige exatamente uma chamada fora de voo: ausência de fecho também falha. Desligar o receiver S143 ainda pode deixá-lo verde, pois não há prova positiva da passagem pelo receiver (`:108–117`; `app/chamados/signals.py:81`). |
| `DURANTE_o_plantio`, `app/chamados/tests/test_pep_reentrada.py:122` | Prova sinal na entrada do ramo e ausência no retorno. Restringir o contexto apenas ao ramo de plantio, retirando-o antes do julgamento/carimbo, preserva essas observações (`:126–134`; `app/chamados/services/materializacao.py:1207`, `:1251`). |
| `ROLLBACK`, `app/chamados/tests/test_pep_reentrada.py:136` | A v2 prova nascimento do sinal e desaparecimento da batida. Limpar apenas em `_Reprovada`, vazando no sucesso, passa neste teste isolado (`:141–151`). |
| `EXCECAO`, `app/chamados/tests/test_pep_reentrada.py:153` | Agora tem assert positivo. Limpar somente na exceção passa neste teste isolado (`:159–163`). |
| `aninhado`, `app/chamados/tests/test_pep_reentrada.py:165` | Cobre outra P e mesma P. Restaurar token somente após `yield`, sem `finally`, passa nas saídas normais testadas e falha nas excepcionais não presentes aqui (`:169–177`; `app/chamados/services/materializacao.py:1121–1124`). |
| `fora_de_voo`, `app/chamados/tests/test_pep_reentrada.py:189` | Controle positivo útil. `pergunta_em_voo` sempre falso passa neste teste isolado (`:193–196`). |
| `S143_pula`, `app/chamados/tests/test_pep_reentrada.py:198` | A irmã nova mata “pula todas” no primeiro laço. Retirar a guarda do laço sem credor escapa: ambas estão validadas, enquanto esse laço exige validação nula (`:207–215`; `app/chamados/services/disputa_emissao.py:1665–1671`). |
| `qualquer_fecho/S144`, `app/chamados/tests/test_pep_reentrada.py:219` | Trocar a guarda por “se existe qualquer voo, pule todas” preserva a única P pulada e o controle fora de voo. Falta irmã fora de voo no S144 (`:223–229`; `app/chamados/services/materializacao.py:1399`). |
| `fora_de_atomic`, `app/chamados/tests/test_pep_reentrada.py:234` | Exige recusa fora de atomic. Um contexto que sempre lance `RuntimeError` passa neste teste isolado (`:238–241`). |

**O teste S144 não é intrinsecamente vazio:** chama `fechar` real e exige depois que a mesma P seja carimbável. Porém, desligar o receiver S144 mantém o assert interno nulo e o controle direto posterior funcionando; logo ele não prova sozinho que houve tentativa pelo receiver (`app/chamados/tests/test_pep_reentrada.py:225–229`; `app/chamados/signals.py:130–140`).

**O spy da cauda resolveu a fraqueza do estado final**, mas observa apenas chamadas de `fechar`; um fecho antecipado por `update` não aparece nele. A cauda também usa a instância recebida sem atualização prévia (`app/chamados/tests/test_pep_reentrada.py:108–117`; `app/chamados/services/materializacao.py:1290–1307`).

Faltam demonstrações de commit real, rollback de savepoint interno, callback observando o sinal depois do bloco e isolamento entre threads/requests. Além disso, no teste S144 a captura de callbacks é o último manager e executa sua saída **antes** da saída do contexto de voo (`app/chamados/tests/test_pep_reentrada.py:36`, `:160–177`, `:224`).

5. **Triagem da volta 1**

- **S144 “procedente e curado”:** a guarda cura o escritor identificado, mas o selo ainda não prova receiver vivo nem poupa irmã fora de voo. “Curado no código” e “coberto integralmente pelo teste” não são equivalentes aqui (`app/chamados/services/materializacao.py:1399`; `app/chamados/tests/test_pep_reentrada.py:223–229`).
- **Multi-alias:** descarte sustentado pelas configurações da cópia, que declaram somente `default`; configuração efetivamente carregada em produção: **[nao verificado]** (`app/config/settings/saas.py:83–95`; `app/config/settings/prod.py:9–18`).
- **Contextos copiados:** aceitável descartar como defeito concreto não demonstrado; errado converter isso em prova de impossibilidade. O reset restaura apenas o contexto corrente; transporte concreto: **[nao verificado]** (`app/chamados/services/materializacao.py:1120–1124`).
- **Absorção / `:405` / `:586`:** os descartes têm fundamento no alcance encontrado: absorção vem da resposta explícita; `:405` barra texto, obrigatório no plantio de hora; `:586` exclui validada (`app/chamados/services/disputa_emissao.py:1330`; `app/chamados/reconciliador.py:364`, `:620`; `app/chamados/services/materializacao.py:804`).
- **Custo da cauda “menor”:** há leitura adicional por disputa, mas custo desprezível não foi medido no dado fornecido: **[nao verificado]** (`app/chamados/services/disputa_emissao.py:1703–1710`).
- **Duplo fecho:** é possibilidade preexistente por instância desatualizada, não segundo escritor da P. O reconciliador fecha outra instância; a cauda e a porta consultam o estado em memória (`app/chamados/reconciliador.py:600–602`; `app/chamados/services/materializacao.py:1290`; `app/chamados/models.py:1820`). Ocorrência concreta: **[nao verificado]**.
- **JUIZES.md “observação”: descarte insuficiente.** A própria documentação manda corrigir divergência no mesmo commit; “disputa fecha … SEM juiz” contradiz o registro e a implementação de C7 (`app/docs/JUIZES.md:14`, `:54`; `app/core/juizes.py:267`; `app/chamados/models.py:1789–1794`).

6. **Classes de reprovação, uma a uma**

| Classe | Avaliação neste diff |
|---|---|
| Segundo juiz duplicando registrado | Não identificado: o predicado novo decide posse temporária; lastro, C6 e C7 continuam nas autoridades existentes (`app/chamados/services/materializacao.py:1129`, `:1207`; `app/chamados/models.py:2596`; `app/chamados/services/disputa_emissao.py:1712`; `app/core/juizes.py:266–267`). |
| Segundo escritor fora da porta | As adições não escrevem o trio: pulam concorrentes. Há escritores preexistentes de identidade e somente de via, que precisam permanecer distintos da afirmação de exclusividade da P

[truncado em 20000 de 21751 caracteres]

## 06/11 09:38 · sessao b8b07d486070 · - · - · origem=Ronald (tmux)

- **Quem escreve/lê a lista:** `falhas_do_universo` produz a lista contra o primeiro pai e a entrega diretamente a `julgar`; este chama `_julgar_linha`, cujo ramo `n/a` consulta `negocio`. A lista é local, sem arquivo ou protocolo externo: [bin/tests/test_lei_akita.sh:180](/home/ronald/codex_ws/858b5d7adfc797252076767a10f8f2dd82f1b7db/bin/tests/test_lei_akita.sh:180), [bin/tests/test_lei_akita.sh:141](/home/ronald/codex_ws/858b5d7adfc797252076767a10f8f2dd82f1b7db/bin/tests/test_lei_akita.sh:141), [bin/tests/test_lei_akita.sh:122](/home/ronald/codex_ws/858b5d7adfc797252076767a10f8f2dd82f1b7db/bin/tests/test_lei_akita.sh:122). A saída externa contém falhas e contadores, não a lista: [bin/tests/test_lei_akita.sh:317](/home/ronald/codex_ws/858b5d7adfc797252076767a10f8f2dd82f1b7db/bin/tests/test_lei_akita.sh:317).

- **Consumidores do veredito:** `bin/pre-push.sh` passa `PUSH_SHA` como `LEI_AKITA_HEAD` e interrompe no retorno não zero; a régua executa o mesmo script pelo glob `tests/test_*.sh`. Ambos recebem a alteração automaticamente, sem predicado duplicado nesses chamadores: [bin/pre-push.sh:33](/home/ronald/codex_ws/858b5d7adfc797252076767a10f8f2dd82f1b7db/bin/pre-push.sh:33), [bin/pre-push.sh:38](/home/ronald/codex_ws/858b5d7adfc797252076767a10f8f2dd82f1b7db/bin/pre-push.sh:38), [bin/regua.sh:138](/home/ronald/codex_ws/858b5d7adfc797252076767a10f8f2dd82f1b7db/bin/regua.sh:138).

- **Hook previsto:** o instalador atual escreve `.git/hooks/pre-push` como wrapper com `exec bash "$(git rev-parse --show-toplevel)/bin/pre-push.sh" "$@"`; portanto a cadeia prevista usa o mesmo juiz: [bin/hooks.sh:49](/home/ronald/codex_ws/858b5d7adfc797252076767a10f8f2dd82f1b7db/bin/hooks.sh:49). O conteúdo do hook efetivamente instalado é `[nao verificado]`.

- **Cópias antigas:** há `app/core/hooks/pre-push.sh`, reconhecido no próprio censo como migração que não pousou; esse arquivo não chama o selo e não contém outro predicado PADRAO-NA-CURA: [bin/tests/test_recursos_sourced.sh:26](/home/ronald/codex_ws/858b5d7adfc797252076767a10f8f2dd82f1b7db/bin/tests/test_recursos_sourced.sh:26), [app/core/hooks/pre-push.sh:6](/home/ronald/codex_ws/858b5d7adfc797252076767a10f8f2dd82f1b7db/app/core/hooks/pre-push.sh:6). Há também instalador antigo que copia o `bin/pre-push.sh`, mas a fonte continua sendo `bin/`: [app/core/hooks/hooks.sh:10](/home/ronald/codex_ws/858b5d7adfc797252076767a10f8f2dd82f1b7db/app/core/hooks/hooks.sh:10), [app/core/hooks/hooks.sh:35](/home/ronald/codex_ws/858b5d7adfc797252076767a10f8f2dd82f1b7db/app/core/hooks/hooks.sh:35).

- A busca por `test_lei_akita`, `PADRAO_FALHA`, `PADRAO_FIM`, `falhas_do_universo`, `_julgar_linha`, `LEI_AKITA_HEAD` e literais `n/a (instrumento)` encontrou o predicado somente neste selo; inexistência absoluta de outra implementação equivalente: `[nao verificado]`.

## 06/11 09:36 · sessao b2c4c9acd30d · LEI-FIO-MUDO-B · verificador · origem=Code

**Parecer: a remoção atende à cura literal, mas o R08f ainda está incompleto como selo.** Saíram a função e o agendamento de reabertura; os problemas encontrados estão na cobertura do teste. [app/chamados/models.py:516; app/chamados/services/fio.py:899; app/chamados/tests/test_c20_fio_mudo.py:283; app/chamados/tests/test_c20_fio_mudo.py:290]

**1. Errado ou incompleto na volta 2**

- **O POST fotografa os pushes antes dos callbacks.** `_no_post` é copiado dentro de `captureOnCommitCallbacks(execute=True)`, antes de sua saída. Um POST que agendasse `on_commit(lambda: push_colaborador(...))` escaparia da comparação: o callback executaria depois da fotografia. Os cinco alvos diferentes de `pushes[0]` ficam descobertos nesse intervalo; o primeiro ainda é conferido indiretamente pelo total final igual a 1. [app/chamados/tests/test_c20_fio_mudo.py:287; app/chamados/tests/test_c20_fio_mudo.py:290; app/chamados/tests/test_c20_fio_mudo.py:293; app/chamados/tests/test_c20_fio_mudo.py:298; app/chamados/tests/test_c20_fio_mudo.py:299]
- **“Corrigir” continua com asserção vazia para o ramo relevante.** A pergunta é `escolha`; o botão exige `hora`. Liberar indevidamente Corrigir para perguntas de hora em disputa fechada, mantendo Rejeitar oculto, não seria detectado. [app/chamados/tests/test_c20_fio_mudo.py:68; app/chamados/tests/test_c20_fio_mudo.py:283; app/templates/chamados/partials/modal_fio.html:284; app/templates/chamados/partials/modal_fio.html:295]
- **O POST novo não exercita a proteção `sem_reabrir` na recusa da materialização.** Esse parâmetro impede reabertura no ramo de resposta de hora recusada; a fixture `escolha` não entra nele. Retirar sua passagem em `fio.py:605` escaparia deste caso. [app/chamados/services/fio.py:605; app/chamados/services/validacao.py:152; app/chamados/services/validacao.py:199; app/chamados/services/validacao.py:422; app/chamados/services/validacao.py:427]

Para fechar essas lacunas, a observação dos seis mocks precisa ocorrer depois dos callbacks, separada da chamada artificial; Corrigir precisa de cenário `hora`; e a proteção contra reabertura no Validar precisa de cenário de hora cuja materialização seja recusada. [app/chamados/tests/test_c20_fio_mudo.py:287; app/chamados/tests/test_c20_fio_mudo.py:290; app/templates/chamados/partials/modal_fio.html:295; app/chamados/services/validacao.py:425]

**2. Descartes e causalidade do REATAR**

**Não encontrei descarte indevido pelo critério perguntado. Retifico a abrangência do parecer v1:** pertencer ao fluxo amplo de `abrir/reabrir` não demonstra ser efeito do REATAR. `reabrir` chama a ressuscitação antes da adoção. [app/chamados/models.py:1495; app/chamados/models.py:1496]

O caminho causal lido é: adoção → reapontamento/lavra → save da pergunta → callback de beco. A classificação do beco exclui imediatamente disputa FECHADA; a adoção também pode retratar duplicatas, sem chamar ressuscitação. [app/chamados/models.py:502; app/chamados/models.py:515; app/chamados/services/fio.py:845; app/chamados/services/fio.py:847; app/chamados/signals.py:202; app/chamados/services/becos.py:27]

- **HX-RESSUSCITA:** seleciona MATERIALIZADAS sem texto, descarimba e reabre suas disputas. Continua sendo ato distinto, anterior à adoção, e não reabertura causada pela reatada. [app/chamados/models.py:1528; app/chamados/models.py:1531; app/chamados/models.py:1598; app/chamados/models.py:1625]
- **F2:** diagnostica a viva não materializada em fechada; esse trecho não reabre nem envia push. Seu filtro também não depende do reapontamento. [app/chamados/management/commands/auditar_invariantes_chamados.py:1761] [nao verificado]
- **Reabertura administrativa:** exige ação própria, permissão, motivo e justificativa; procura a disputa pelo chamado-container, reabre e envia FCM. A adoção não chama essa ação. [app/chamados/services/fio.py:577; app/chamados/services/acoes_disputa.py:242; app/chamados/services/acoes_disputa.py:245; app/chamados/services/acoes_disputa.py:252; app/chamados/services/acoes_disputa.py:256; app/chamados/services/acoes_disputa.py:283]
- **RELATO:** a passagem da cópia já começa com `RESPONDIDA`; não sustento o apontamento anterior de decisão ainda pendente nessa passagem. [app/docs/RELATO.md:65]

Há fábrica posterior à abertura que pode notificar por outra disputa ABERTA. A exceção para dia passado com marcos nomeados independe da reatada e também funciona sem ela; não demonstra reabertura da fechada original nem push causado pelo REATAR. [app/ponto/management/commands/detectar_ausencias.py:790; app/chamados/services/disputa_emissao.py:658; app/chamados/services/disputa_emissao.py:876; app/chamados/services/disputa_emissao.py:925; app/chamados/services/disputa_emissao.py:935; app/chamados/services/disputa_emissao.py:1123]

**3. O que o R08f efetivamente morde**

| Parte | Alcance por leitura estática |
|---|---|
| Disputa | Exige pergunta viva, cobrança criada e destino reapontado; executa callbacks antes de comparar fechamento e resolução. Morde a volta do callback antigo nesse cenário respondido. [app/chamados/tests/test_c20_fio_mudo.py:255; app/chamados/tests/test_c20_fio_mudo.py:257; app/chamados/tests/test_c20_fio_mudo.py:262; app/chamados/tests/test_c20_fio_mudo.py:264] |
| Push no reatar | Corrigiu os guardas: patcha os wrappers e confere os seis alvos depois dos callbacks da adoção. [app/chamados/tests/test_c20_fio_mudo.py:221; app/chamados/tests/test_c20_fio_mudo.py:257; app/chamados/tests/test_c20_fio_mudo.py:266] |
| Card | Corrigido: exige a pergunta especificamente sob `ch.pk` e confere a flag de fechada. [app/chamados/tests/test_c20_fio_mudo.py:270; app/chamados/tests/test_c20_fio_mudo.py:272] |
| Modal | Confere presença do ID, presença de Validar e ausência de Rejeitar. ID e ação são procurados separadamente, sem conferir o mesmo formulário; Corrigir conserva a vacuidade descrita acima. [app/chamados/tests/test_c20_fio_mudo.py:279; app/chamados/tests/test_c20_fio_mudo.py:283] |
| POST Validar | Agora faz POST real e exige `validada_em` persistido, preservando fechamento/resolução após callbacks. Permanecem as lacunas de push tardio e de recusa de hora. [app/chamados/tests/test_c20_fio_mudo.py:288; app/chamados/tests/test_c20_fio_mudo.py:294; app/chamados/tests/test_c20_fio_mudo.py:298] |

**O “MORDE” do mock prova apenas que o import explícito do próprio teste recebe o primeiro símbolo patchado.** Ele chama diretamente esse mock; não exercita um chamador de produção nem dá controle positivo dos outros cinco alvos. [app/chamados/tests/test_c20_fio_mudo.py:230; app/chamados/tests/test_c20_fio_mudo.py:291; app/chamados/tests/test_c20_fio_mudo.py:299]

A generalização “todo sítio de produção importa localmente” também é inexata: `views_cobrar` importa FCM no nível do módulo e usa esse nome depois. Isso limita a promessa do comentário, sem demonstrar alcance desse outro ato pela adoção. [app/chamados/tests/test_c20_fio_mudo.py:218; app/chamados/views_cobrar.py:11; app/chamados/views_cobrar.py:44; app/chamados/models.py:502]

A variante **sem resposta** também permanece fora do R08f: a adoção não exige resposta, o teste a preenche, e a interface/validação atuais mostram “Aguardando resposta” e recusam validar sem `respondida_em`. A cobertura dessa variante pelo novo selo é ausente. [app/chamados/models.py:478; app/chamados/models.py:492; app/chamados/tests/test_c20_fio_mudo.py:252; app/templates/chamados/partials/modal_fio.html:314; app/chamados/services/validacao.py:363]

**4. Escritores e leitores adicionais da volta 2**

| Alvo adicional | Quem define/escreve | Quem lê/chama |
|---|---|---|
| `_CANOS_DE_PUSH` / `_push_gravado` | Tupla e montagem dos seis patches no teste. [app/chamados/tests/test_c20_fio_mudo.py:217; app/chamados/tests/test_c20_fio_mudo.py:230] | Blocos de adoção/POST e comparações das chamadas. [app/chamados/tests/test_c20_fio_mudo.py:256; app/chamados/tests/test_c20_fio_mudo.py:266; app/chamados/tests/test_c20_fio_mudo.py:286; app/chamados/tests/test_c20_fio_mudo.py:298] |
| Novo POST | Teste fornece ação e IDs. [app/chamados/tests/test_c20_fio_mudo.py:288] | View → `processar_post_fio` → `acao_validar_pergunta` → `validar_pergunta`. [app/chamados/views.py:951; app/chamados/services/fio.py:581; app/chamados/services/fio.py:605; app/chamados/services/acoes_disputa.py:18] |
| `sem_reabrir` agora exercitado pelo POST | Fio calcula pela disputa da pergunta e passa à casca. [app/chamados/services/fio.py:596; app/chamados/services/fio.py:605] | Casca encaminha; validação usa na devolução recusada. [app/chamados/services/acoes_disputa.py:22; app/chamados/services/validacao.py:427] |
| `validada_em` agora observado | No ato testado, `_carimbar_validacao` grava instante e autor. [app/chamados/services/validacao.py:127; app/chamados/services/validacao.py:133] | Novo assert, juiz de vida, estado do modelo, seleção do materializador e modal. [app/chamados/tests/test_c20_fio_mudo.py:296; app/chamados/juizes.py:1136; app/chamados/models.py:2593; app/chamados/services/materializacao.py:1296; app/templates/chamados/partials/modal_fio.html:223] |

`validada_em` possui outras escritas preexistentes: devolução recusada, nova resposta, reabertura para correção, ressuscitação e preenchimento pela porta `carimbar`. Portanto, “dono do ato de validar” não equivale a atribuição exclusiva da coluna; essas escritas não nasceram na volta 2. [app/chamados/services/validacao.py:188; app/chamados/services/disputa_emissao.py:1295; app/chamados/services/disputa_emissao.py:1393; app/chamados/models.py:1598; app/chamados/models.py:2686]

**5. Órfãos, segundo escritor e relógio**

Não encontrei chamador executável sobrevivente da função apagada: as referências restantes identificadas são lápides e registros documentais. [app/chamados/models.py:517; app/chamados/services/fio.py:899; app/docs/PROMPTS.md:1895; app/docs/RELATO.md:65; app/docs/BACKLOG.md:1778]

Não há novo escritor ou relógio de produção introduzido pela volta 2: as adições exercitam portas existentes; o teste mantém o relógio congelado e a validação usa seu carimbo existente. [app/chamados/tests/test_c20_fio_mudo.py:31; app/chamados/tests/test_c20_fio_mudo.py:269; app/chamados/tests/test_c20_fio_mudo.py:288; app/chamados/services/validacao.py:131]

Correspondência da cópia com HASH/HEAD, RED→GREEN, suite, sobrevivência experimental dos mutantes e efeitos efetivos em produção: **[nao verificado]**. [nao verificado]

## 06/11 09:36 · sessao b60541119bfe · - · - · origem=Ronald (tmux)

Censo de produção, excluídos testes/migrações: [nao verificado]

- `PerguntaDisputa.carimbar`: `materializacao.py:549`, `:1251`, `:1405`; `disputa_emissao.py:1612`, `:1686`; `reconciliador.py:405`, `:586`; wrapper `models.py:2685`. Os sítios S143/S144 pulam a P em voo em `disputa_emissao.py:1586`, `:1671` e `materializacao.py:1399`; o materializador escreve em `materializacao.py:1251`.
- `reconciliador.py:405` é alcançável sincronamente pela batida: `ponto/signals.py:215` → `reconciliador.py:1417` → `:1510` → `:1603`. Porém não alcança a P-hora corrente: exige ausência de texto em `reconciliador.py:364`, e o plantio de hora exige texto em `materializacao.py:804`. `MOTIVOS_C1` só contém dois motivos (`reconciliador.py:332`), ambos `hora` no catálogo (`catalogo/perguntas.py:48`, `:56`); escolha sem texto com motivo C1 seria dado fora desse catálogo [nao verificado].
- `reconciliador.py:586` também é síncrono, via `ponto/signals.py:215` → `reconciliador.py:1416`, e alcança irmãs, mas exclui a P validada pelo filtro `reconciliador.py:620`.
- A absorção `materializacao.py:549` vem da resposta explícita em `disputa_emissao.py:1330`; nenhum chamador desse responder foi encontrado nos receivers síncronos do plantio [nao verificado para alcance absoluto]. O fantasma `models.py:2685` vem de `reconciliador.py:1136`, chamado pelo consumidor EventoCelula em `reconciliador.py:1363`, com cron próprio em `config/crons.py:394`.

Escritores adicionais dos mesmos campos:

- Atribuição tríplice: `models.py:2558`–`:2560`; persistência por `carimbar` em `models.py:2609`, identidade em `:2644`, descarimbo em `:2652` e descarimbo em lote em `:2663`.
- Identidade: `models.py:2522` (`fechar_por_admin`); `materializacao.py:899` (paridade), `:1074` (colisão); `fio.py:891` (duplicata). Paridade e colisão podem escrever durante o bloco, mas são ramos do próprio materializador, por `carimbar_identidade`, preexistentes ao diff (`materializacao.py:899`, `:1074`, `:1194`).
- Só troca da via: `models.py:2673`–`:2674`, chamada pelo comando `encerrar_perguntas_com_fato.py:68`; `relavra_via.py:123`–`:126`, chamada pelo comando `relavrar_via_i6.py:102`. `relavrar_dia` delega em `relavra_via.py:155`, mas está sem chamador de produção [nao verificado por execução].
- Volta do carimbo: `models.py:2696` (fantasma), `:2713` (resposta), `:2720` (competência), `:1504` (ressuscitação); `disputa_emissao.py:1412` (desfazer absorção); `relavra_via.py:231` (carimbo sem lastro).
- O cartório pode atingir `relavra_via.py:231` durante o bloco por `cartorio.py:985` → `fabrica_da_celula.py:166`, mas a porta exige que C6 considere a pergunta viva (`relavra_via.py:227`); a P corrente está validada e C6 a considera morta [pai está verificando a referência exata do degrau].
- A ressuscitação exige carimbo presente e ausência de texto (`models.py:1434`–`:1438`); a migração/deduplicação exige C6 vivo (`fio.py:931`–`:936`), portanto não selecionam a P-hora validada corrente nesses estados (`materializacao.py:804`, `:1343`–`:1346`).

`DisputaSupervisao.fechar`, todos os sítios de produção:

| Sítio | Alcance durante voo |
|---|---|
| `materializacao.py:568` | Absorção da resposta explícita, chamador em `disputa_emissao.py:1330`; alcance pelo plantio não demonstrado [nao verificado]. |
| `materializacao.py:1307` | Cauda depois de `_materializar_uma`, portanto após saída do contexto (`materializacao.py:1350`, `:1352`). |
| `disputa_emissao.py:1713` | S143 síncrono, barrado quando há P em voo em `disputa_emissao.py:1710`. |
| `reconciliador.py:412` | Síncrono por irmãs via batida; P corrente com texto não passa por `reconciliador.py:364`. S144 acionado por esse fecho pula a P em `materializacao.py:1399`. |
| `reconciliador.py:602` | Síncrono por irmã absolvida; P corrente excluída por `reconciliador.py:620`; S144 pula a P em `materializacao.py:1399`. |
| `reconciliador.py:1160` | Consumidor EventoCelula (`reconciliador.py:1363`), cron separado (`config/crons.py:394`). |
| `becos.py:52` | Evento registrado em `chamados/signals.py:202`, por `on_commit`, portanto depois do bloco normal. |
| `disputa_emissao.py:1118` | Pertence a `criar_disputa_a_partir_de_chamados_orfaos`; fábrica chama outra porta, `criar_ou_atualizar_disputa` (`fabrica_da_celula.py:139`), não prova esse alcance [nao verificado]. |
| `competencia_trancada.py:204` | Porta da tranca, nenhum caminho síncrono pelo plantio demonstrado [nao verificado]. |
| `colaboradores/services/desligamento.py:142` | Porta de desligamento, nenhum caminho síncrono pelo plantio demonstrado [nao verificado]. |
| `ponto/services/entrada_sincronizada.py:59` | Chamadores API em `api/views.py:805`, `api/views_core.py:805`; nenhum caminho inverso do plantio demonstrado [nao verificado]. |
| `api/foto_recebida.py:80` | Chamadores API em `api/views.py:871`, `api/views_core.py:861`; nenhum caminho inverso do plantio demonstrado [nao verificado]. |
| `management/commands/encerrar_perguntas_com_fato.py:88` | Comando separado, sem chamada do plantio demonstrada [nao verificado]. |
| `models.py:1879` | Wrapper `admin_encerra`; sem chamada do plantio demonstrada [nao verificado]. |
| `models.py:1986` | Wrapper `remover_motivo`, associado a eventos de posto/âncora (`models.py:1911`–`:1913`); sem chamada do plantio demonstrada [nao verificado]. |

O fechamento efetivo escreve `fechada_em`/`resolucao` e salva ambos em `models.py:1861`–`:1865`. Reaberturas zeram ambos em `models.py:2063`–`:2075` e `:2087`–`:2091`; limpeza residual só zera resolução em `models.py:2173`–`:2174`. Não encontrei outro escritor de produção que ponha `fechada_em` preenchida [nao verificado por execução].

Correção relevante da hipótese do usuário: S144 **não** roda apenas uma vez por fecho; o receiver aceita qualquer `post_save` de disputa já fechada, porque a guarda verifica somente `raw`, `created` e ausência de `fechada_em` (`chamados/signals.py:130`). O único chamador direto de produção encontrado de `materializar_pendentes_aprovadas` é esse receiver (`chamados/signals.py:139`–`:140`).

## 06/11 09:35 · sessao e8e99bc140d8 · - · - · origem=Ronald (tmux)

Auditoria estática, sem execução dos testes; resultados reais de verde/vermelho e dos mutantes são [nao verificado]. Abaixo, “sobrevive” significa previsão pela leitura das asserções citadas. [nao verificado]

**Sinal e transações**

- `_EM_VOO` é um objeto `ContextVar` de módulo; seu valor é um conjunto imutável de PKs. A entrada exige `connection.in_atomic_block`, une a PK ao conjunto e guarda token; a saída sempre restaura o token em `finally` — não usa `clear()` que quebraria a reentrada da mesma P (`app/chamados/services/materializacao.py:1106`, `:1118`, `:1120`, `:1124`).
- No uso produtivo, o atomic entra primeiro e o sinal depois; na saída, o sinal é restaurado **antes** de o atomic confirmar ou desfazer. Isso cobre sucesso, `_Reprovada`, exceção e retorno antecipado (`app/chamados/services/materializacao.py:1194`, `:1201`, `:1213`, `:1253`, `:1257`).
- Savepoint interno desfeito, com exceção absorvida dentro do bloco externo, deve preservar a P externa em voo: o manager externo ainda não saiu. O S143 possui precisamente atomic/savepoint interno com exceção absorvida; não altera `_EM_VOO` (`app/chamados/signals.py:88`, `:91`, `:93`; `app/chamados/services/materializacao.py:1124`). Falta teste desse caso (`app/chamados/tests/test_pep_reentrada.py:136`, `:153`, `:165`).
- Aninhamento de P diferente preserva a anterior; mesma P usa token para restaurar o conjunto anterior. Há asserções positivas e negativas para ambos, mas só com saída normal (`app/chamados/tests/test_pep_reentrada.py:169`, `:171`, `:173`, `:174`, `:176`, `:177`).
- `on_commit` realmente executado depois da saída de `_materializar_uma` encontra o sinal restaurado, pela ordem dos managers (`app/chamados/services/materializacao.py:1194`, `:1124`). **Os testes não demonstram esse caso**: `_Base` herda `TestCase`, e `captureOnCommitCallbacks(execute=True)` é o último manager em `:224`, portanto a execução capturada ocorre antes de sair do manager do sinal (`app/chamados/tests/test_pep_reentrada.py:36`, `:224`).
- O teste da materialização também executa callbacks capturados, mas só verifica o sinal na função de plantio e no retorno, não dentro de callback posterior ao bloco (`app/chamados/tests/test_pep_reentrada.py:74`, `:122`, `:129`, `:134`).
- Não há identidade de transação/token de conexão no valor: apenas PK e a pergunta “há atomic agora?”; a garantia vem do escopo lexical e do reset (`app/chamados/services/materializacao.py:1111`, `:1120`, `:1129`). Migração de contexto para outra thread/request não está exercitada: [nao verificado]. A imagem declara Python 3.13 (`app/Dockerfile:1`).
- Multi-alias não é bug demonstrado nesta configuração: `saas.py` declara somente `default`, com backend django-tenants e router (`app/config/settings/saas.py:83`, `:84`, `:85`, `:93`). Troca de schema com sinal ativo não foi demonstrada: [nao verificado].

**Teste S144: útil, mas não suficiente** [nao verificado]

- Não considero o teste `qualquer_fecho...` intrinsecamente vazio: ele cria batida real, mantém a P sem carimbo, chama `d.fechar` real e exige que, fora do voo, a **mesma função** carimbe exatamente aquela P (`app/chamados/tests/test_pep_reentrada.py:179`, `:181`, `:186`, `:225`, `:227`, `:228`).
- O caminho do receiver é síncrono: `fechar` salva `fechada_em`, e o receiver chama imediatamente `materializar_pendentes_aprovadas`; não é o callback capturado (`app/chamados/models.py:1861`, `:1865`; `app/chamados/signals.py:124`, `:139`, `:140`).
- A P está validada no fixture; o C6 a considera morta imediatamente por `validada_em`. Logo o fecho normal não é barrado por pergunta viva e `carimbar` também aceita a P validada (`app/chamados/tests/test_pep_reentrada.py:59`, `:60`; `app/chamados/juizes.py:1135`, `:1136`, `:1137`; `app/chamados/models.py:2596`, `:2609`).
- Mutante sobrevivente grave: receiver S144 retornar sempre em `signals.py:130`. O assert interno continua vendo NULL e o controle final chama a função diretamente, portanto não prova que o receiver foi alcançado (`app/chamados/signals.py:130`; `app/chamados/tests/test_pep_reentrada.py:225`, `:227`, `:228`).
- Outro mutante sobrevivente: trocar `if pergunta_em_voo(p.pk)` por `if bool(_EM_VOO.get())` no S144. A única P do teste continua pulada; fora do voo continua carimbada. Falta uma irmã **fora** de voo no mesmo S144, como a irmã já adicionada para S143 (`app/chamados/services/materializacao.py:1399`; `app/chamados/tests/test_pep_reentrada.py:223`, `:228`, `:204`, `:214`).

**Teste que espia a cauda** [nao verificado]

- Ficou mais forte: registra cada chamada de `DisputaSupervisao.fechar` antes da execução e exige exatamente uma, com conjunto vazio; não depende apenas da resolução final (`app/chamados/tests/test_pep_reentrada.py:108`, `:109`, `:113`, `:115`, `:117`).
- Também falha se nenhuma chamada ocorrer; portanto não é vazio por lista vazia (`app/chamados/tests/test_pep_reentrada.py:113`).
- Continua observando apenas a porta `fechar`. Mutante que escrevesse cedo `fechada_em/resolucao` via `QuerySet.update` e deixasse a instância original do materializador chamar `fechar` ao final poderia escapar do spy; a instância da cauda não recebe `refresh_from_db` (`app/chamados/tests/test_pep_reentrada.py:111`; `app/chamados/services/materializacao.py:1290`, `:1307`).
- Mutante mais simples: desativar o receiver S143. O destino ainda pode terminar fechado, a única chamada final permanece fora de voo e o teste continua verde; não existe assert de que o S143 foi realmente chamado dentro do plantio (`app/chamados/tests/test_pep_reentrada.py:101`, `:112`, `:113`; `app/chamados/signals.py:81`).

**Um mutante sobrevivente por teste** [nao verificado]

| Teste | O que observa / mutante que pode escapar | [nao verificado]
|---|---|
| `:83 UM_escritor` | Conta somente chamadas **bem-sucedidas de `carimbar`** e via. Uma segunda escrita crua `save/update` da P durante S143 não entra em `gravados`; o carimbo posterior do materializador deixa a lista esperada (`:69-72`, `:90-92`). |
| `:94 via_gravada` | Remover a guarda S143 e permitir carimbo antecipado seguido de sobrescrita pelo materializador pode deixar via/autor finais corretos; não conta escritas (`:98-99`; materializador `:1251`). |
| `:101 cauda` | Desativar S143 preserva única chamada final fora de voo; falta assert positivo de passagem pelo receiver (`:108-117`; signals `:81`). |
| `:122 durante/saída` | Reduzir o manager do sinal à chamada `_materializar_resposta_hora`, retirando-o antes das etapas posteriores/carimbo, ainda produz `[True]` e `False` ao final; não espia a duração inteira (`:126-134`; materializador `:1207`, `:1251`). |
| `:136 rollback` | Resetar apenas no caminho `_Reprovada` e vazar no sucesso ainda satisfaz este teste isolado; agora ele prova nascimento do sinal e rollback da batida, corrigindo a fraqueza da v1 (`:141-151`). | [nao verificado]
| `:153 exceção` | Resetar somente quando há exceção ainda passa este teste; não exige limpeza na saída normal (`:159-163`). | [nao verificado]
| `:165 aninhamento` | Retirar `finally` e restaurar token só após `yield` na saída normal passa este teste; rollback/exceção aninhados não estão presentes (`:169-177`; manager `:1121-1124`). | [nao verificado]
| `:189 fora_de_voo` | Retornar sempre `False` em `pergunta_em_voo` mantém esse controle verde; ele só exige carimbo normal (`:193-196`). | [nao verificado]
| `:198 S143_pula` | Retirar somente a guarda do laço sem credor (`disputa_emissao.py:1671`) escapa: ambas as perguntas do fixture estão validadas e esse laço filtra `validada_em is None` (`test :207-208`; serviço `:1665-1666`). |
| `:219 S144` | Guarda global “se existe qualquer voo, pula todas” sobrevive; falta irmã fora de voo no mesmo S144 (`materializacao.py:1399`; teste `:223-229`). | [nao verificado]
| `:234 fora_atomic` | Manager que sempre lance `RuntimeError`, mesmo em atomic, passa este teste isolado; ele só exerce recusa (`:238-241`). | [nao verificado]

Os mutantes das linhas `:136`, `:153`, `:165`, `:189`, `:234` seriam mortos por outros testes do arquivo; não os apresentaria como buracos da **suíte inteira**. Os buracos coletivos mais claros são: escrita crua invisível aos spies, receiver não demonstrado, S144 “pula todas”, ausência de caso sem credor não validado em voo e ausência dos cenários savepoint/on_commit/thread (`app/chamados/tests/test_pep_reentrada.py:69`, `:111`, `:204`, `:224`, `:228`; `app/chamados/services/disputa_emissao.py:1665`; `app/chamados/services/materializacao.py:1399`).

_2196 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
