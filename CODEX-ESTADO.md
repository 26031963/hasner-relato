# CODEX-ESTADO

> Ultima resposta processada: **17/11/2026 09:10** · publicado em **17/11/2026 09:14** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 17/11 09:10 · sessao ebb638ddcccf · - · - · origem=Ronald (tmux)

Contagem dos rótulos `COL` positivos fornecidos: [nao verificado]

| Rótulo | COL positivos | Soma das perguntas | `NUMERO` declarado |
|---|---:|---:|---:|
| `P1_A_candidatas_sem_nova_devolucao` / `P1_B_candidatas_com_devolucao` | 118 | 2145 | 2174 |
| `P2_A_textos_divergentes_conservados` / `P2_B_textos_divergentes_substituidos` | 64 | 191 | 226 |

As somas dos `COL` diferem dos `NUMERO` declarados em 29 para P1 e 35 para P2; a causa não aparece no dado. [nao verificado]

## 17/11 09:10 · sessao af563760d743 · - · - · origem=Ronald (tmux)

P1: **118 col** com valor positivo; soma dos `COL`: **2145**. O `NUMERO P1_A_candidatas_sem_nova_devolucao` fornecido é **2174**. [nao verificado]

P2: **64 col** com valor positivo; soma dos `COL`: **191**. O `NUMERO P2_A_textos_divergentes_conservados` fornecido é **226**. [nao verificado]

As somas por `COL` não coincidem com os respectivos `NUMERO` no dado transcrito. [nao verificado]

## 17/11 09:04 · sessao 29e3e1889e9d · E4C-F2 · verificador · origem=Code

**[bloqueia] A obra conserva uma validação que a ressuscitação acabou de zerar.** O próprio teste exige projeção sem validação, leitura validada, `estado == VALIDADA` e exclusão de `q_vivas` após ressuscitar. Isso contraria a conservação da fila indicada na porta. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:413-428`; `app/chamados/models.py:1640-1648`)

1. **Escritores de RESPOSTA e significado de `ato_motivo`.**

| Emissor | Motivo gravado | Tratamento de `validada_em` |
|---|---|---|
| `responder_pergunta_disputa_core` | Omitido → `''` | Zera a validação existente; salva projeção e registra RESPOSTA no mesmo atomic. (`app/chamados/models.py:4111`; `app/chamados/services/disputa_emissao.py:1494-1496`, `:1517-1526`) |
| `responder_automaticamente` | `resposta automatica (fato entregue por outro caminho)` | Preserva; salva somente texto e instante. (`app/chamados/services/disputa_emissao.py:1674-1679`) |
| `carimbar_resposta_pela_trilha` | `pela trilha: %s` | Preserva; salva somente instante. (`app/chamados/services/disputa_emissao.py:1696-1700`) |
| `linhas_da_projecao_sem_linha` | `projecao sem linha na sequencia (anterior a E1 ou carimbo sem ato)` | A geração da foto não altera a projeção; o efeito depende do chamador. (`app/chamados/models.py:4146`, `:4171-4179`) |

Os caminhos adicionais pedidos se distribuem assim:

- **Nascimento:** registra as fotos das assinaturas recebidas, conservando os valores inseridos. (`app/chamados/models.py:3151-3156`)
- **Backfill E4b:** o ramo `registrar_decisao` grava as fotos pelo helper; esse ramo não limpa a validação. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:138-141`)
- **`backfill_decisoes_pergunta`:** `linhas_a_criar` apenas consulta o helper; a escrita explícita de decisões encontrada no apply é SILENCIO. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:44-48`, `:67`, `:324-327`)
- **`curar_perguntas_em_limbo`:** chama a trilha com `corte 13/09: sem marca de correcao no fio (nota #%d)`; portanto a RESPOSTA recebe esse texto precedido de `pela trilha:`. A limpeza das demais perguntas usa reabertura. (`app/chamados/management/commands/curar_perguntas_em_limbo.py:67-69`; `app/chamados/services/disputa_emissao.py:1700`)
- **`foto_e4b`:** fotografa decisões em um dicionário; não cria RESPOSTA nesse caminho. (`app/chamados/services/foto_e4b.py:247-252`, `:280-281`)
- **Migração 0051:** transforma `RespostaAnterior` em `DecisaoPergunta`, classificando o acervo como REABERTURA. (`app/chamados/migrations/0051_decisao_pergunta.py:41-61`)
- **Admin:** nenhum escritor adicional localizado; ausência absoluta de caminho administrativo indireto [nao verificado].

Entre os três emissores diretos, não encontrei a combinação inversa à regra nova. Entretanto, **há RESPOSTA COM motivo em ato que zera a validação**: as fotos históricas acompanham tanto a resposta humana quanto devoluções e ressuscitações. Logo, “com motivo preserva V no mesmo ato” não vale como afirmação geral. (`app/chamados/services/disputa_emissao.py:1480`, `:1494`, `:1525`; `app/chamados/services/validacao.py:272-282`; `app/chamados/models.py:1629-1648`)

**[bloqueia] A ressuscitação é a exceção funcional:** grava fotos RESPOSTA/VALIDACAO e limpa a projeção sem registrar ato limpador. O replay reaplica a VALIDACAO; `estado` retorna VALIDADA e os filtros deixam de trazer a pergunta. O C6 continua lendo a projeção, criando desacordo entre a pergunta julgada viva e seus coletores SQL. (`app/chamados/models.py:1629-1648`, `:4226-4234`, `:3180-3183`, `:352-355`, `:3557-3560`; `app/chamados/juizes.py:1389`, `:1403-1406`)

**[fila] `ato_motivo` acumula explicação e controle de efeito.** O replay usa sua ausência como comando para desfazer V; a face SQL reproduz essa convenção. O campo permanece texto livre e `registrar` não valida esse protocolo. (`app/chamados/models.py:4201`, `:3496`, `:4111`, `:4248`)

Os demais leitores encontrados usam motivo como explicação no histórico **não-RESPOSTA**, ou reconhecem BUG-PAR-103 em DEVOLUCAO. Não demonstram interpretação conflitante de uma mesma RESPOSTA. (`app/templates/chamados/partials/modal_fio.html:209-212`; `app/chamados/services/materializacao.py:275`)

2. **Cache: a correção cobre a mesma instância, não toda identidade.**

**[fila] Outra instância da mesma pk, ou um queryset com prefetch já avaliado, pode continuar lendo a sequência velha:** `registrar` invalida somente `ln['pergunta']`; `leitura` aceita as linhas de `.decisoes.all()` sem verificar atualização. Linhas somente com `pergunta_id` não invalidam instância alguma. (`app/chamados/models.py:4251-4254`, `:4217`)

Quem passa IDs inclui:

| Caminho | Evidência |
|---|---|
| Todas as fotos produzidas pelo helper | `app/chamados/models.py:4173`, `:4177` |
| Nascimento e ressuscitação, por esse helper | `app/chamados/models.py:3156`, `:1629-1630` |
| Backfill E4b | `app/chamados/management/commands/backfill_fio_morto_e4b.py:141` |
| Reparo L130 | `app/chamados/services/validacao.py:436-440` |
| `reabrir_para_correcao`, diretamente | `app/chamados/services/disputa_emissao.py:1631-1648` |

Nos fluxos examinados, não encontrei uma leitura subsequente **comprovadamente cacheada e velha**: recusa carrega a pergunta sem prefetch; rejeição de ausência segue pelas disputas; limbo termina no relatório; o modal reconstrói o contexto, e a leitura posterior de estado na validação vem após `refresh_from_db()`. (`app/chamados/services/validacao.py:894-895`, `:925-953`; `app/chamados/services/materializacao.py:1118`, `:1153-1175`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:69-70`; `app/chamados/services/fio.py:540-544`, `:659-660`)

Portanto: a possibilidade estrutural existe; ocorrência concreta no mesmo fluxo produtivo fica **[nao verificado]**. O teste novo cobre somente a mesma instância recebida pela porta. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:248-264`)

3. **Correspondência entre leitura e porta.**

**[nota] Sim, usam o mesmo corpo e a mesma chave:** `(pergunta_id, ato, ato_em)`. Além disso, o `ja` da memória também filtra exclusivamente RESPOSTA/VALIDACAO; não há a diferença sugerida de incluir todos os atos. Texto e motivo não participam da correspondência. (`app/chamados/models.py:4165-4179`, `:4218-4222`)

**[fila] A equivalência depende de memória completa e atual.** Cache velho ou parcial muda o conjunto `ja` e omite atos do replay; o corpo único não resolve a atualização dos insumos. (`app/chamados/models.py:4217-4222`, `:4251-4254`)

4. **Mordida dos testes e justificativa do teto.** [nao verificado]

- **[nota] C6 mantém a mordida pedida:** a face entra na varredura; retirar sua participação elimina `validada_em` do conjunto e é cobrado pela asserção. A sentinela também mantém a ligação do scanner ao selo. (`app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:248-266`, `:288-293`)
- **[nota] Respondedores:** `PAROU_EM=0`, quatro declarados e contagens exatas `1/1/2/1`; continuam os mutantes de respondedor novo, leitura adicional em declarado e declaração morta. (`app/chamados/tests/test_e3_catraca_respondedores.py:53-85`, `:577-602`)
- **[nota] Delegação e rótulo:** o sentinela do juiz foi conservado; a fixture do rótulo passou a fornecer um par legal e mantém as asserções valor→rótulo. (`app/chamados/tests/test_f1nova_b2_leitores.py:87-97`; `app/chamados/tests/test_valor_rotulo.py:8-21`)
- **[nota] Falsos sem banco:** acrescentam relação vazia e autor nulo para alimentar a leitura real, conservando o passivo como insumo. (`app/chamados/tests/test_e4b_c4v14_memoria.py:47-53`; `app/chamados/tests/test_e4b_classe_e_memoria.py:282-287`; `app/ponto/tests/test_supra_juiz_executor.py:25-40`)
- **[nota] C12:** permanecem nove identidades nominais e exceções vazias. (`app/chamados/tests/e4b_c12_ast.py:18-29`; `app/chamados/tests/test_e4b_c12_selo.py:34-36`)
- **[bloqueia] Há selo que aceita a divergência:** a caracterização da ressuscitação exige justamente o resultado incompatível com o reset. O controle vizinho usa fixture sem validação e não cobre essa classe. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:388-428`)

**[nota] O custo adicional é por carga de conjunto, não por pergunta:** existe uma chamada de Prefetch para a lista inteira de modelos com pk. Lista vazia ou cache já preenchido pode custar zero. (`app/chamados/models.py:1943-1947`)

A atribuição a `pode_fechar`, `fechar` e CSF é compatível com os caminhos, mas o CSF chama o corpo para **dois universos**. Assim, “+3” é específico do cenário medido, não uma identidade geral por chamada externa. Confirmação dos valores 223/283: **[nao verificado]**. (`app/chamados/reconciliador.py:613-614`; `app/chamados/models.py:1960`, `:1993`; `app/ponto/supra_juiz.py:573-574`)

5. **Checklist das classes de reprovação.**

| Classe | Resultado |
|---|---|
| Segundo juiz | **[nota]** A face SQL está expressamente autorizada como face, e `respondida` delega a C21. O desacordo C6/SQL aparece na ressuscitação descrita acima. (`app/docs/CORTES.md:487`; `app/chamados/models.py:3731`; `app/chamados/juizes.py:1404`) |
| Segundo escritor fora da porta | **[nota]** Nenhum identificado; a persistência permanece em `registrar`, com selo de escritor único. (`app/chamados/models.py:4248`; `app/chamados/tests/test_e1_entradas.py:217-225`) |
| Relógio fora da borda | **[nota]** Os corpos novos de correspondência/replay usam instantes recebidos, sem ler relógio. (`app/chamados/models.py:4149-4180`, `:4204-4235`) |
| Derivação paralela | **[bloqueia]** Correspondência foi unificada, mas projeção/C6 e replay/SQL discordam após reset real. (`app/chamados/models.py:1647`, `:3558`; `app/chamados/juizes.py:1389`) |
| Selo/RED sem mordida | **[bloqueia]** As adaptações examinadas conservam suas cobranças; a caracterização aceita a regressão da ressuscitação. (`app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:262-293`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:413-428`) |
| Ordem de trava | **[nota]** Nenhuma inversão identificada no delta; ressuscitação e reabertura adquirem a ordem antes de registrar. (`app/chamados/models.py:1628-1630`; `app/chamados/services/disputa_emissao.py:1629-1631`) |
| Campo com dois significados | **[fila]** Motivo livre também decide se a RESPOSTA revoga validação, sem protocolo imposto na porta. (`app/chamados/models.py:4111`, `:4201`, `:4248`) |

Verificação executada: 29 testes AST dos selos de respondedores/C12 passaram; os dois testes do escritor único também passaram por extração em memória. Suíte com banco, comparação integral `708162b15..b67ebb3d3` e números de produção ficam **[nao verificado]**. (`app/chamados/tests/test_e3_catraca_respondedores.py:577`; `app/chamados/tests/test_e4b_c12_selo.py:21`; `app/chamados/tests/test_e1_entradas.py:217-225`)

## 17/11 09:03 · sessao 50f0e05fe967 · ENSAIO-DA-FOLHA-11 · verificador · origem=Code

AVAL-DEVOLVIDO — somente item 4. (`respostas_1711_0900.txt:9`, anexo fornecido.)

*Nas referências aos anexos, conto as linhas do conteúdo colado, começando em “aval Ronald”.*

1. **As premissas conferem parcialmente; aplica-se o ramo assíncrono.** O POST chama `pedir`, registra o clique e chama `disparar`; este executa outro `manage.py` por `subprocess.Popen`, que consulta o lote, assume e gera o caderno. (`app/relatorios/views.py:1227`; `app/relatorios/views.py:1238`; `app/relatorios/views.py:1246`; `app/relatorios/lote_espelho.py:127`; `app/relatorios/lote_espelho.py:132`; `app/relatorios/management/commands/gerar_cartoes_em_lote.py:44`; `app/relatorios/management/commands/gerar_cartoes_em_lote.py:57`.)

   As escritas identificadas são:

   | Destino | Efeito |
   |---|---|
   | `LoteEspelho` | Criação, PID/início, estado, progresso, resultado ou erro. Até o polling pode atualizar um lote cujo processo morreu. (`app/relatorios/lote_espelho.py:103`; `app/relatorios/lote_espelho.py:138`; `app/relatorios/lote_espelho.py:161`; `app/relatorios/lote_espelho.py:203`; `app/relatorios/lote_espelho.py:251`; `app/relatorios/lote_espelho.py:264`; `app/relatorios/views.py:1272`; `app/relatorios/lote_espelho.py:75`.) |
   | `LogAuditoria`, via `observ` | Clique, disparo, resultado/falha e deduplicação. A persistência é *best-effort*: pode falhar sem interromper a geração. (`app/relatorios/views.py:1238`; `app/relatorios/views.py:1251`; `app/relatorios/lote_espelho.py:140`; `app/relatorios/lote_espelho.py:254`; `app/relatorios/lote_espelho.py:267`; `app/core/observ.py:105`; `app/core/observ.py:134`; `app/core/observ.py:139`.) |
   | `RegistroPDFAuditavel` | Registro por espelho que percorre a geração auditável. (`app/relatorios/services.py:237`; `app/relatorios/services.py:70`; `app/relatorios/pdf_espelho.py:1103`; `app/relatorios/pdf_base.py:152`.) |
   | Arquivos | Log `MEDIA_ROOT/cartoes_lote/<id>.log`, com stdout/stderr do filho; PDF em `cartoes_lote/`. Há também configuração de logs em `/app/logs/errors.log` e `/app/logs/ponto.log`. (`app/relatorios/lote_espelho.py:124`; `app/relatorios/lote_espelho.py:130`; `app/relatorios/lote_espelho.py:132`; `app/relatorios/lote_espelho.py:250`; `app/relatorios/models.py:125`; `app/config/settings/saas.py:139`; `app/config/settings/saas.py:163`.) |

   **Savepoint + raise no pai não desfaz tudo.** Por inferência da execução de outro Django em outro processo, a transação do pai não abrange os commits da conexão do filho; tampouco desfaz os arquivos. Se o pedido ainda estiver sem commit, o filho pode consultar e não encontrar o lote. (`app/relatorios/lote_espelho.py:132`; `app/relatorios/lote_espelho.py:250`; `app/relatorios/management/commands/gerar_cartoes_em_lote.py:44`.)

   **Existe caminho com escrita pela sombra, mas não pela sonda de leitura:** `sombra.sh --rodar` executa o script com as credenciais da casa e settings que fixam banco `sombra` em `sombra_db`; `sonda_leitura --sombra` mantém a role `leitor`, transações inicialmente em leitura e GRANTs de SELECT. Credenciais e permissões efetivas atuais: **[nao verificado]**. (`bin/sombra.sh:183`; `bin/sombra.sh:487`; `bin/sombra.sh:490`; `app/config/settings/saas.py:87`; `app/config/settings/sombra.py:47`; `app/config/settings/sombra.py:49`; `bin/sonda_leitura.sh:42`; `bin/sonda_leitura.sh:142`; `bin/sonda_leitura.sh:178`.)

   **O PDF não chega automaticamente à pasta do ensaio.** O download apenas devolve `FileResponse`; na porta da sombra, `/app/media` é tmpfs e o container usa `--rm`. O destino persistente disponível é `/sombra`, correspondente a `logs/sombra`. Falta explicitar a espera pelo filho e a transferência do PDF antes de encerrar o container; o mecanismo concreto está **[nao verificado]**. (`app/relatorios/views.py:1311`; `bin/sombra.sh:81`; `bin/sombra.sh:183`; `bin/sombra.sh:189`; `bin/sombra.sh:190`.)

   **Há leitores na esteira.** A completude conta todas as tabelas do tenant; o incremental executa máscara e selo que consultam campos das três tabelas. O cron da sombra executa `--refazer && --bloco`, cujo bloco deriva dos comandos diários. (`bin/sombra.sh:313`; `bin/sombra.sh:204`; `bin/sombra.sh:206`; `bin/sombra_regras_pessoais.py:72`; `bin/sombra_regras_pessoais.py:73`; `bin/sombra_regras_pessoais.py:93`; `bin/sombra_regras_pessoais.py:131`; `bin/sombra_regras_pessoais.py:147`; `bin/sombra_regras_pessoais.py:157`; `bin/sombra_selo_pessoal.py:74`; `app/config/crons.py:350`; `app/config/crons.py:1919`.)

   Há também DIFF que alcança `LogAuditoria` pela carga da tarefa, filtrando ações de `PerguntaDisputa`, e cron que consulta ações de `ChamadoColaborador`. Isso prova leitura da tabela, sem provar dependência das novas linhas `cartao_lote*`. Outros leitores são deduplicação, painel/download e verificação pública do PDF. Scripts concretos usados em `--rodar` e interferência real durante o ensaio: **[nao verificado]**. (`app/chamados/management/commands/diff_e3_sombra.py:134`; `app/chamados/services/tarefa_do_dia.py:534`; `app/chamados/services/carga_pergunta_viva.py:110`; `app/chamados/management/commands/auditar_invariantes_chamados.py:122`; `app/relatorios/lote_espelho.py:96`; `app/relatorios/views.py:1308`; `app/chamados/views.py:1210`; `bin/sombra.sh:490`.)

2. **A permissão posterior resolve a proibição original apenas no escopo declarado de B1.** O ENSAIO original proibia tocar a sombra única além de leitura; o item 4 posterior permite expressamente pedido, auditoria e PDF. Isso não amplia a autorização para produção ou outras alterações. (`ensaio_folha_11_1711.txt:16`; `respostas_1711_0900.txt:5`; `respostas_1711_0900.txt:8`; `CLAUDE.md:37`.)

   **Continua pendente a compatibilização com a ZONA INVIOLÁVEL:** a regra vigente exige smoke em banco lateral `juliani_ref_*` dentro de `sombra_db`, `atomic()+raise` e contagem antes/depois. O item 4 propõe a sombra compartilhada e limpeza posterior para geração assíncrona; não explicita a exceção a essas condições. POST real em produção continua sendo escrita. (`CLAUDE.md:205`; `respostas_1711_0900.txt:5`.)

   BANCO SOMBRA, máscara e CPUSET continuam aplicáveis: a porta usa settings da sombra e CPUSET de teste; `sombra_db.sh` também aplica/confere o CPUSET do banco. Apenas `nice 10` não demonstra isolamento de CPU. Configuração efetiva de uma execução futura: **[nao verificado]**. (`CLAUDE.md:87`; `CLAUDE.md:206`; `bin/sombra.sh:183`; `bin/sombra_db.sh:64`; `bin/sombra_db.sh:92`; `app/relatorios/lote_espelho.py:127`.)

   A máscara da sombra tampouco substitui a raspagem exigida por B1: o gerador imprime os campos nome e CPF; B1 exige `colNNN` e CPF removido. (`app/relatorios/pdf_espelho.py:739`; `ensaio_folha_11_1711.txt:8`.)

3. **Três condições não fecham como garantia literal.**

   **Limpeza:** apagar o lote não apaga suas auditorias nem seus registros auditáveis: `LogAuditoria` usa modelo textual/ID inteiro; `RegistroPDFAuditavel` não tem FK para lote. O CASCADE existente é da empresa para seus lotes. A limpeza precisa identificar separadamente registros e arquivos; a exclusão do lote, por si, não demonstra remoção do PDF/log. (`app/core/models.py:297`; `app/core/models.py:298`; `app/relatorios/models.py:43`; `app/relatorios/models.py:50`; `app/relatorios/models.py:106`; `app/relatorios/models.py:125`; `app/relatorios/lote_espelho.py:130`.)

   “Linhas criadas” também não cobre alterações anteriores: `pedir` pode reutilizar lote existente e o vigia pode atualizar lote antigo. Falta um procedimento demonstrado que distinga esses casos e espere o subprocesso terminar antes da limpeza. (`app/relatorios/lote_espelho.py:96`; `app/relatorios/lote_espelho.py:98`; `app/relatorios/lote_espelho.py:75`; `app/relatorios/lote_espelho.py:132`.)

   **Sem DIFF em curso:** o juiz existente consulta a ocupação de `logs/sombra.lock`; erro de consulta conta como ocupado. A exclusão efetiva exige manter a trava durante geração, espera, retirada do PDF e limpeza. Uma foto anterior não reserva a sombra; além de DIFF, há refazimento/incremental e bloco usando o recurso. Estado atual: **[nao verificado]**. (`bin/disjuntor.py:405`; `bin/disjuntor.py:416`; `bin/disjuntor.py:419`; `bin/sombra_trava.sh:28`; `bin/sombra.sh:438`; `bin/sombra.sh:451`; `bin/sombra.sh:487`.)

   **Auditável:** qualquer exceção nesse ramo aciona o informacional; datas explícitas pulam diretamente para ele. O informacional incrementa `n_ok`, e o lote pode terminar `CONCLUIDO`. Portanto “gerados/concluído” não comprova B1; deve-se conferir o fallback, que o aval mantém como falha. Pode ainda sobrar registro auditável de tentativa que falhou depois da criação, pois a segunda renderização ocorre posteriormente. (`app/relatorios/services.py:235`; `app/relatorios/services.py:244`; `app/relatorios/services.py:247`; `app/relatorios/services.py:252`; `app/relatorios/lote_espelho.py:251`; `respostas_1711_0900.txt:5`; `app/relatorios/pdf_base.py:152`; `app/relatorios/pdf_base.py:178`.)

4. **Há efeitos fora da enumeração declarada no caminho existente de escrita.** Antes do script B1, `--rodar` chama `lavra_de_prod`: grava `lavra_prod.json` e `repor_lavra.py` e, em outra execução, faz dois `update_or_create` em `MetricaSnapshot`. Pode modificar registros preexistentes; apagar apenas linhas criadas pelo lote não restaura isso, e o savepoint do script começa depois. (`bin/sombra.sh:489`; `bin/sombra.sh:144`; `bin/sombra.sh:151`; `bin/sombra.sh:157`; `bin/sombra.sh:160`; `bin/sombra.sh:166`; `bin/sombra.sh:490`.)

   Isso não prova alteração de dinheiro ou competência exportada. A lavra continua sendo lida para pautas; o código atual registra que dinheiro deixou de depender dela e o fechamento consulta `escala_certa_no_dia`. Alteração real de `FechamentoMensal`, exportação ou pagamento neste ensaio: **[nao verificado]**. A permissão de B1 não libera os `!` de exportada, escala/vínculo, arquivos usados por produção ou atalhos. (`app/ponto/services/esmeril_espelho.py:345`; `app/ponto/services/esmeril_espelho.py:358`; `app/ponto/services/esmeril_espelho.py:351`; `app/ponto/services/fechamento.py:928`; `CLAUDE.md:265`; `CLAUDE.md:411`.)

VEREDITO: item 4 PROCEDE -- faltam fechar a exceção do POST, a exclusão durante todo o job, a transferência/limpeza dos artefatos e as escritas adicionais de MetricaSnapshot. (`CLAUDE.md:205`; `bin/sombra_trava.sh:28`; `bin/sombra.sh:189`; `bin/sombra.sh:157`; `bin/sombra.sh:160`.)

## 17/11 09:02 · sessao 5bee4a00eef2 · - · - · origem=Ronald (tmux)

**O caminho `--rodar` tem efeitos adicionais que o item 4 não declara:** antes do script B1, chama `lavra_de_prod`; essa função lê o snapshot mais recente de produção, grava `logs/sombra/lavra_prod.json` e `repor_lavra.py`, e executa outro container que faz dois `update_or_create` em `MetricaSnapshot`. (`bin/sombra.sh:489`; `bin/sombra.sh:141`; `bin/sombra.sh:144`; `bin/sombra.sh:151`; `bin/sombra.sh:157`; `bin/sombra.sh:160`; `bin/sombra.sh:166`)

**“Registrar linhas criadas e apagar” não cobre esse efeito:** `update_or_create` pode atualizar snapshots existentes; o savepoint dentro do script B1 começa depois da chamada e não inclui a execução anterior do importador. Para retornar ao estado anterior seria necessário identificar/restaurar também registros modificados, além dos criados e arquivos. (`bin/sombra.sh:157`; `bin/sombra.sh:160`; `bin/sombra.sh:489`; `bin/sombra.sh:490`)

**Não atribuir mudança de dinheiro atual a essa lavra:** os comentários antigos de `sombra.sh` ainda dizem que ela decide HE100, mas o serviço atual registra que, desde 24/10, dinheiro deixou de ler a lavra; o fechamento consulta `escala_certa_no_dia` pela competência do dia. A lavra continua sendo autoridade de leitura para pautas/lista e sua guarda da sombra. (`bin/sombra.sh:127`; `app/ponto/services/esmeril_espelho.py:250`; `app/ponto/services/esmeril_espelho.py:289`; `app/ponto/services/esmeril_espelho.py:345`; `app/ponto/services/fechamento.py:928`; `app/ponto/services/fechamento.py:934`)

**A interação compartilhada é concreta:** `pautas_do_esmeril.sincronizar` lê a lavra e, com `apply`, abre/fecha pautas; `linhas_para_a_pauta` também lê o snapshot. Logo atualizar a lavra excede os registros administrativos do lote e afeta uma fonte compartilhada, embora a execução desses leitores durante o ensaio esteja [nao verificado]. (`app/ponto/services/pautas_do_esmeril.py:57`; `app/ponto/services/pautas_do_esmeril.py:69`; `app/ponto/services/pautas_do_esmeril.py:75`; `app/ponto/services/esmeril_espelho.py:358`)

**A cópia da lavra não passa novamente pela máscara:** copia/importa `detalhe` integral, sem chamada de máscara/selo nessa função. O gerador atual da lavra inclui PKs, contagens, frases de assinatura e destinos de propostas; isso não prova presença de nome/CPF no snapshot real fornecido ao script. Conteúdo real e eventual dado pessoal nessa cópia estão [nao verificado]. (`bin/sombra.sh:141`; `bin/sombra.sh:144`; `bin/sombra.sh:155`; `bin/sombra.sh:163`; `bin/sombra.sh:166`; `app/ponto/services/esmeril_espelho.py:222`; `app/ponto/services/esmeril_espelho.py:227`; `app/ponto/services/esmeril_espelho.py:239`)

**Limite da máscara:** a lista JSON inventariada contempla `LogAuditoria` e `LoteEspelho`, mas não `MetricaSnapshot`; o detector nominal de colunas pessoais não inclui `detalhe`. Portanto selo pessoal verde não demonstra raspagem desse JSON da lavra. (`bin/sombra_regras_pessoais.py:144`; `bin/sombra_regras_pessoais.py:158`; `bin/sombra_regras_pessoais.py:161`; `bin/sombra_selo_pessoal.py:30`; `bin/sombra_selo_pessoal.py:82`)

## 17/11 09:01 · sessao 0b97f3872cfe · - · - · origem=Ronald (tmux)

- **A própria rodada da sombra lê essas tabelas.** `--refazer --incremental` copia somente quatro tabelas de ponto/escala, mas depois roda máscara e selo pessoais; o inventário inclui `LoteEspelho.erro/placar`, `LogAuditoria.ip/descricao/valor_antes/valor_depois` e `RegistroPDFAuditavel.ip_origem`. A máscara percorre linhas de texto/JSON e o selo consulta esses campos. (`bin/sombra_incremental.sh:33`; `bin/sombra.sh:204`; `bin/sombra_regras_pessoais.py:72`; `bin/sombra_regras_pessoais.py:93`; `bin/sombra_regras_pessoais.py:131`; `bin/sombra_regras_pessoais.py:147`; `bin/sombra_regras_pessoais.py:157`; `bin/sombra_mascara_pessoal.py:51`; `bin/sombra_selo_pessoal.py:55`; `bin/sombra_selo_pessoal.py:74`)

- **Há leitura genérica de todas as tabelas:** a completude faz `count(*)` em cada `BASE TABLE` de `juliani`, portanto alcança essas três quando presentes no schema; `refazer()` chama essa conferência. O refazimento restaura dumps completos de `public` e `juliani`, sem excluir essas tabelas. (`bin/sombra.sh:298`; `bin/sombra.sh:308`; `bin/sombra.sh:313`; `bin/sombra.sh:244`; `bin/sombra.sh:257`)

- **O cron da sombra é `--refazer && --bloco`.** O bloco deriva os comandos diários de `config/crons.py`, exceto estágios infra/contínuo, e inclui encadeados; portanto não basta procurar o nome das três tabelas no shell. (`app/config/crons.py:350`; `app/config/crons.py:1919`; `app/config/crons.py:1934`; `app/config/crons.py:1947`)

- **Um cron alcançado pelo bloco lê `LogAuditoria`:** `auditar_invariantes_chamados`, declarado às 07:18, consulta `modelo='ChamadoColaborador'` e `acao='retratacao_retida_irma_orfa'`. Isso prova leitura da tabela, não consumo das novas ações `cartao_lote*`. (`app/config/crons.py:542`; `app/chamados/management/commands/auditar_invariantes_chamados.py:121`)

- **Há DIFF que pode ler `LogAuditoria` transitivamente:** `diff_e3_sombra` chama alimentação/tarefa; a tarefa alcança `carga_da_vida`, que consulta ações `retratar_pergunta_obsoleta` de `PerguntaDisputa`. Não há nessa consulta dependência das ações `cartao_lote*`. (`app/chamados/management/commands/diff_e3_sombra.py:133`; `app/chamados/management/commands/diff_e3_sombra.py:145`; `app/chamados/services/tarefa_do_dia.py:485`; `app/chamados/services/tarefa_do_dia.py:490`; `app/chamados/services/carga_pergunta_viva.py:110`)

- **`--rodar` não tem conjunto fixo de leitores:** executa o script fornecido em `/sombra`, depois de importar a lavra de produção. Os leitores dependem desse script; scripts efetivamente usados pela esteira não foram fornecidos. (`bin/sombra.sh:483`; `bin/sombra.sh:489`; `bin/sombra.sh:490`; [nao verificado])

- **Outros leitores reais:** pedido/deduplicação lê `LoteEspelho`; progresso e download leem o lote; verificação pública por SHA lê `RegistroPDFAuditavel`. (`app/relatorios/lote_espelho.py:96`; `app/relatorios/lote_espelho.py:98`; `app/relatorios/views.py:1288`; `app/relatorios/views.py:1308`; `app/chamados/views.py:1209`)

- **“Sem DIFF em curso” não está demonstrado pelo dado.** A exclusão oficial é obter e manter `flock` em `logs/sombra.lock`, com herança pelos filhos; o código proíbe adivinhar disponibilidade por `pgrep`. O DIFF de folha segura a mesma trava durante as duas fotos e comparação. A trava precisa abranger geração, espera pelo job, retirada do PDF e limpeza para impedir intercalação; observar ausência num instante não reserva o recurso. (`bin/sombra_trava.sh:2`; `bin/sombra_trava.sh:5`; `bin/sombra_trava.sh:21`; `bin/sombra_trava.sh:28`; `bin/sombra_trava.sh:37`; `bin/simular_folha.sh:25`; `bin/simular_folha.sh:96`)

- **O status genérico da esteira não prova ausência de DIFF:** calcula pacotes vivos/trabalhando, sem distinguir estágio DIFF. Estado atual de processos, locks, sondas e DIFF: **[nao verificado]**. (`bin/esteira_status.sh:14`; `bin/esteira_status.sh:63`; `bin/esteira_status.sh:112`)

## 17/11 09:01 · sessao 5bee4a00eef2 · - · - · origem=Ronald (tmux)

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 17/11 09:01 · sessao 3f68a35b23bb · - · - · origem=Ronald (tmux)

- **Assíncrono.** O POST chama `pedir`, grava auditoria e dispara subprocesso; `Popen` executa outro `manage.py`, com `nice 10` e `--schema`. O comando consulta o lote e assume/executa a geração. (`app/relatorios/views.py:1227`, `app/relatorios/views.py:1238`, `app/relatorios/views.py:1246`, `app/relatorios/lote_espelho.py:127`, `app/relatorios/lote_espelho.py:132`, `app/relatorios/management/commands/gerar_cartoes_em_lote.py:44`, `app/relatorios/management/commands/gerar_cartoes_em_lote.py:52`)

- **Escritas observadas:** `LoteEspelho` no pedido, PID, execução, progresso, resultado e falha; `LogAuditoria` no clique, disparo, resultado ou deduplicação; `RegistroPDFAuditavel` por PDF auditável. O polling também pode atualizar lote morto. (`app/relatorios/lote_espelho.py:103`, `app/relatorios/lote_espelho.py:138`, `app/relatorios/lote_espelho.py:161`, `app/relatorios/lote_espelho.py:203`, `app/relatorios/lote_espelho.py:251`, `app/relatorios/lote_espelho.py:264`, `app/relatorios/views.py:1238`, `app/relatorios/views.py:1251`, `app/core/observ.py:134`, `app/relatorios/pdf_base.py:152`, `app/relatorios/views.py:1272`, `app/relatorios/lote_espelho.py:75`)

- **Arquivos:** cria/abre em append `MEDIA_ROOT/cartoes_lote/<id>.log`, redireciona stdout/stderr do subprocesso e salva PDF no `FileField(upload_to='cartoes_lote/')`. O download é `FileResponse`, sem mecanismo automático de colocação na pasta do ensaio. (`app/relatorios/lote_espelho.py:124`, `app/relatorios/lote_espelho.py:130`, `app/relatorios/lote_espelho.py:132`, `app/relatorios/lote_espelho.py:250`, `app/relatorios/models.py:125`, `app/relatorios/views.py:1311`)

- **Rollback do pai não engloba tudo:** o subprocesso executa outro Django e as escritas de arquivos são explícitas; inferência: savepoint da conexão pai não desfaz os commits do filho nem arquivos. Se o lote ainda não estiver visível quando o filho consultar, o comando pode simplesmente dizer que não existe. (`app/relatorios/lote_espelho.py:127`, `app/relatorios/lote_espelho.py:132`, `app/relatorios/lote_espelho.py:250`, `app/relatorios/management/commands/gerar_cartoes_em_lote.py:44`)

- **Apagar somente o lote não limpa trilhas:** `LogAuditoria.modelo`/`objeto_id` são texto/inteiro; `RegistroPDFAuditavel` não possui FK para lote e tem `colaborador_id` inteiro. A FK CASCADE do lote aponta para empresa: a exclusão da empresa elimina lotes, não o contrário. Registro auditável protege a exclusão de seu usuário; lote usa usuário SET_NULL. (`app/core/models.py:297`, `app/core/models.py:298`, `app/relatorios/models.py:43`, `app/relatorios/models.py:50`, `app/relatorios/models.py:106`, `app/relatorios/models.py:37`, `app/relatorios/models.py:128`)

- **“Linhas criadas” é insuficiente:** `pedir` pode reutilizar lote preexistente e pode atualizar lote antigo pelo vigia. A identificação dos PDFs auditáveis não tem vínculo estruturado com o lote; limpeza precisa distinguir registros próprios, alterações anteriores, arquivos e log. (`app/relatorios/lote_espelho.py:96`, `app/relatorios/lote_espelho.py:98`, `app/relatorios/lote_espelho.py:75`, `app/relatorios/models.py:30`)

- **B1 pode falhar com lote concluído:** qualquer exceção do auditável aciona informacional; datas explícitas pulam diretamente o auditável. Informacional incrementa `n_ok`; resultado pode ser `CONCLUIDO`, com tipo de erro apenas quando existem `pulados`. Logo, “concluído/gerados” não comprova B1 auditável. (`app/relatorios/services.py:235`, `app/relatorios/services.py:244`, `app/relatorios/services.py:247`, `app/relatorios/services.py:250`, `app/relatorios/services.py:252`, `app/relatorios/lote_espelho.py:247`, `app/relatorios/lote_espelho.py:251`)

- **Falha tardia pode deixar registro:** o registro auditável é criado antes da segunda renderização do PDF; inferência: falha nessa renderização pode deixar registro e provocar fallback no serviço. A auditoria `observ` também é best-effort e pode devolver `None` sem interromper o fluxo. (`app/relatorios/pdf_base.py:152`, `app/relatorios/pdf_base.py:178`, `app/relatorios/services.py:244`, `app/core/observ.py:105`, `app/core/observ.py:139`)

- **Outros leitores concretos:** painel/polling/download leem `LoteEspelho`; verificação pública consulta `RegistroPDFAuditavel` por hash. (`app/relatorios/views.py:1291`, `app/relatorios/views.py:1308`, `app/chamados/views.py:1210`, `app/chamados/views.py:1214`)

## 17/11 09:00 · sessao c5918a93429a · SERIE-BLOQUEIO-FOLHA · sonda · origem=Code

Não executei `smk_lavras_11.py`: não localizei o arquivo na cópia. Execução e acesso a produção: [nao verificado]. Reproduzo literalmente o dado fornecido, sem validação independente [nao verificado]: [nao verificado]

```text
sonda_leitura: smk_lavras_11.py [04d2babc4a78503d] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

BANCO saas_hasner AGORA 2026-11-17 08:59
TOTAL_RETRATOS_11_2026 4
EMP 1 lavrado_em=2026-11-17 06:46:09 linhas=3
EMP 2 lavrado_em=2026-11-17 06:46:09 linhas=453
EMP 3 lavrado_em=2026-11-17 06:46:09 linhas=115
EMP 4 lavrado_em=2026-11-17 06:46:09 linhas=24
POR_DATA_DE_LAVRA {'2026-11-17': 4}
RESTRICAO_UNICA ['retrato_folha_um_por_empresa_competencia']
FIM_OK
```

LAVRAS ANTERIORES A 17/11: nao -- POR_DATA_DE_LAVRA {'2026-11-17': 4}; somente nos retratos atuais de 11/2026 do dado fornecido [nao verificado].

`RetratoFolha` guarda **somente a última lavra por empresa/competência**: a chave única é `(empresa, mes, ano)` em [app/folha/models.py:366](app/folha/models.py:366), e o lavrador substitui `linhas` e `lavrado_em` via `update_or_create` em [app/folha/management/commands/lavrar_retrato_folha.py:77](app/folha/management/commands/lavrar_retrato_folha.py:77).

Não identifiquei outra tabela de histórico de lavras do retrato na cópia; o código documenta que preservar duas fotos exige novo modelo e migration em [app/folha/retrato_folha.py:725](app/folha/retrato_folha.py:725). `ExportacaoDominio` preserva gerações de TXT, não versões de `RetratoFolha`: [app/folha/models.py:174](app/folha/models.py:174).

Portanto, a saída não permite concluir que **nunca houve** lavras anteriores a 17/11; sua existência histórica é [nao verificado].

## 17/11 08:59 · sessao 9853300ba392 · - · - · origem=Ronald (tmux)

[nota] Não identifiquei enfraquecimento nas adaptações examinadas: C6 continua cobrando campos lidos, delegação continua exigindo o sentinela do juiz, e a fixture do rótulo mantém o objetivo de conferir a tradução valor→rótulo com par legal. (`app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:248-293`; `app/chamados/tests/test_f1nova_b2_leitores.py:87-97`; `app/chamados/tests/test_valor_rotulo.py:8-21`)

- **C6:** retirar `q_validada_pelo_ato` da varredura é mordido por `test_a_costura_MORDE`, que exige `validada_em` no conjunto; a sentinela também exige que o scanner de filtros entre no resultado. (`app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:253-266`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:288-293`)
- Demonstração em memória: extração AST com metadados simulados passou nos seis testes; mutação retirando somente a face da chamada em `_lidos_pelo_c6` produziu uma falha: `validada_em not found`. Execução da suíte Django real: [nao verificado]. [nao verificado]
- **Respondedores:** `PAROU_EM=0`, quatro declarados e contagem exata por declarado permanecem; os mutantes de respondedor novo, leitura adicional em declarado e declaração morta continuam cobrados. (`app/chamados/tests/test_e3_catraca_respondedores.py:53-85`; `app/chamados/tests/test_e3_catraca_respondedores.py:544-602`)
- Medição AST sem banco: zero respondedores, declarados com contagens 1/1/2/1, divergências `{}`. Os 29 testes de `test_e3_catraca_respondedores` e `test_e4b_c12_selo` passaram, com bytecode desativado. Resultado de execução sem arquivo de prova: [nao verificado]. [nao verificado]
- **C12:** as nove identidades atualizadas apontam para as chamadas reais; o selo mantém corpo com nove membros e exceções vazias. (`app/chamados/tests/e4b_c12_ast.py:18-29`; `app/chamados/models.py:205`; `app/chamados/models.py:302`; `app/chamados/models.py:458`; `app/chamados/models.py:663`; `app/chamados/models.py:843`; `app/chamados/models.py:861`; `app/chamados/models.py:1497`; `app/chamados/models.py:1701`; `app/chamados/models.py:1721`; `app/chamados/tests/test_e4b_c12_selo.py:34-36`)

[nota] A justificativa do teto corresponde a **consultas por chamada com conjunto ainda não carregado**, sem consulta por pergunta: o Prefetch recebe uma lista inteira; vazio ou cache previamente preenchido pode custar zero. A alegação literal “1 SELECT por chamada” exige essa ressalva. (`app/chamados/models.py:1941-1947`)

[nota] O CSF chama o corpo duas vezes, para universos diferentes; portanto não se deve interpretar sua chamada externa como exatamente um SELECT adicional em qualquer cenário. Os caminhos `pode_fechar` e `fechar` também consultam o corpo separadamente. (`app/ponto/supra_juiz.py:573-574`; `app/chamados/models.py:1960`; `app/chamados/models.py:1993`; `app/chamados/reconciliador.py:613-614`)

[nota] O **+3 e os valores 223/283 efetivamente medidos** permanecem [nao verificado], pois não houve leitura de logs nem execução com banco.

Comando utilizado, sem escrita de bytecode:

```bash
PYTHONDONTWRITEBYTECODE=1 PYTHONPATH=app python3 -B -m unittest chamados.tests.test_e3_catraca_respondedores chamados.tests.test_e4b_c12_selo
```

## 17/11 08:58 · sessao 73f8d70f028f · - · - · origem=Ronald (tmux)

(b) **[fila] Invalidação limitada à instância recebida.** `registrar` limpa somente `ln.get('pergunta')._prefetched_objects_cache['decisoes']`; não alcança outra instância da mesma pk nem linhas passadas apenas por ID. `leitura` confia integralmente em `p.decisoes.all()`, portanto outra instância previamente carregada pode conservar sequência velha. Isso é possibilidade demonstrável pela implementação; ocorrência em fluxo produtivo concreto permanece **[nao verificado]**. (`app/chamados/models.py:4217`, `app/chamados/models.py:4251`)

Quem produz/passsa `pergunta_id`:

- `linhas_da_projecao_sem_linha` produz sempre ID, tanto RESPOSTA quanto VALIDACAO; seus consumidores escritores incluem nascimento, ressuscitação, validação/devolução, resposta humana/automática e E4b. Os escritores que acrescentam uma linha `pergunta=p` invalidam essa mesma instância; os que registram somente o passivo não recebem essa proteção. (`app/chamados/models.py:4173`, `app/chamados/models.py:4177`, `app/chamados/models.py:3156`, `app/chamados/models.py:1629`, `app/chamados/services/validacao.py:173`, `app/chamados/services/validacao.py:279`, `app/chamados/services/validacao.py:354`, `app/chamados/services/validacao.py:439`, `app/chamados/services/disputa_emissao.py:1525`, `app/chamados/services/disputa_emissao.py:1678`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:141`)
- `reabrir_para_correcao` registra diretamente por ID e executa o UPDATE por outro queryset. (`app/chamados/services/disputa_emissao.py:1631`, `app/chamados/services/disputa_emissao.py:1648`)

Não encontrei leitura subsequente comprovadamente velha nos chamadores produtivos examinados:

- `recusar_pergunta` carrega instância sem prefetch; depois da reabertura usa foto, nota, evento e push, sem consultar novamente `decisoes`/`estado`. (`app/chamados/services/validacao.py:894`, `app/chamados/services/validacao.py:925`, `app/chamados/services/validacao.py:927`)
- Rejeição de ausência carrega perguntas sem prefetch; depois da reabertura usa suas disputas e a foto anterior. (`app/chamados/services/materializacao.py:1118`, `app/chamados/services/materializacao.py:1153`, `app/chamados/services/materializacao.py:1167`, `app/chamados/services/materializacao.py:1170`)
- `curar_perguntas_em_limbo` termina com o relatório após reabrir por queryset. (`app/chamados/management/commands/curar_perguntas_em_limbo.py:69`)
- O modal reconstrói o contexto após a ação; a leitura posterior de `_p.estado` no caminho de validação de disputa fechada vem depois de `refresh_from_db()`. (`app/chamados/services/fio.py:540`, `app/chamados/services/fio.py:659`)
- Upload de foto valida por ID, usando outra instância internamente, mas o fluxo seguinte atualiza a disputa e consulta novamente suas perguntas. (`app/api/foto_recebida.py:43`, `app/api/foto_recebida.py:51`, `app/api/foto_recebida.py:74`, `app/api/foto_recebida.py:81`)

(c) **[nota] A correspondência é a mesma, desde que as linhas em memória estejam completas e atuais.** Banco e memória restringem `ja` aos mesmos atos RESPOSTA/VALIDACAO; o único corpo compara exatamente `(pk, ato, ato_em)`, sem texto ou motivo. Outros atos presentes na sequência não alteram a correspondência. (`app/chamados/models.py:4165`, `app/chamados/models.py:4171`, `app/chamados/models.py:4175`, `app/chamados/models.py:4218`, `app/chamados/models.py:4222`)

**[fila] `ja` velho/parcial pode diferir do banco.** A leitura não verifica se o cache está atualizado: pode acrescentar uma foto sintética que o banco já possui, além de omitir atos novos. O delta protege a mesma instância recebida; proteção de outras instâncias permanece ausente. (`app/chamados/models.py:4217`, `app/chamados/models.py:4221`, `app/chamados/models.py:4251`)

Candidato de regressão independente do cache: ressuscitação preserva linhas antigas e zera a projeção sem novo ato que limpe a sequência; L130 também mantém a RESPOSTA e zera somente a projeção. A nova leitura reaplica as linhas antigas. O pai está examinando esse ponto. (`app/chamados/models.py:1629`, `app/chamados/models.py:1647`, `app/chamados/services/validacao.py:436`, `app/chamados/services/validacao.py:439`, `app/chamados/models.py:4226`)

## 17/11 08:58 · sessao 35b4f208b605 · - · - · origem=Ronald (tmux)

Inventário de RESPOSTA encontrado em produção:

| Escritor | Motivo | `validada_em` no ato |
|---|---|---|
| `responder_pergunta_disputa_core` | Omitido, portanto `''` | Zera quando preenchida; grava projeção e linha no mesmo `atomic`. `app/chamados/services/disputa_emissao.py:1484`, `:1494`, `:1517`, `:1522`, `:1525`; default em `app/chamados/models.py:4111`. |
| `responder_automaticamente` | `resposta automatica (fato entregue por outro caminho)` | Preserva; salva somente texto/instante. `app/chamados/services/disputa_emissao.py:1674`, `:1677`, `:1679`. |
| `carimbar_resposta_pela_trilha` | `pela trilha: %s` | Preserva; salva somente instante. `app/chamados/services/disputa_emissao.py:1696`, `:1698`, `:1700`. |
| Foto da projeção | `MOTIVO_PROJECAO_SEM_LINHA` | A foto isolada não altera projeção; gera RESPOSTA por correspondência. `app/chamados/models.py:4146`, `:4171`, `:4173`. |

Chamadores que merecem distinção:

- Nascimento com assinaturas recebidas usa a foto e conserva os valores: `app/chamados/models.py:3151`, `:3154`, `:3156`.
- `backfill_fio_morto_e4b` grava fotos sob trava sem mudar resposta/validação nesse ramo: `app/chamados/management/commands/backfill_fio_morto_e4b.py:138`.
- `backfill_decisoes_pergunta.linhas_a_criar` é DRY e consulta o helper; o apply explícito encontrado escreve SILENCIO: `app/chamados/management/commands/backfill_decisoes_pergunta.py:44`, `:67`, `:324`.
- `curar_perguntas_em_limbo` delega à trilha com motivo de corte e nota; a limpeza delega à reabertura: `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`.
- `foto_e4b` fotografa campos, inclusive decisões; sua função `guardar` apenas monta dicionário: `app/chamados/services/foto_e4b.py:247`, `:280`.
- A migração 0051 transforma o acervo de `RespostaAnterior` em REABERTURA, não RESPOSTA: `app/chamados/migrations/0051_decisao_pergunta.py:41`, `:49`, `:61`. A migração 0053 apenas verifica colisões: `app/chamados/migrations/0053_pergunta_unica_por_familia.py:36`.
- Nenhum cadastro administrativo de `DecisaoPergunta` foi encontrado; ausência em todos os caminhos possíveis [nao verificado].

**Exceção literal à frase “RESPOSTA com motivo preserva V no mesmo ato”:** a foto com motivo pode acompanhar um ato que limpa V. O core humano gera `_sem_linha` antes de zerar V e grava as fotos junto à resposta nova; devoluções fazem o mesmo junto à DEVOLUCAO. Não são respostas novas com semântica contraditória, mas fotos históricas no mesmo atomic: `app/chamados/services/disputa_emissao.py:1480`, `:1494`, `:1525`; `app/chamados/services/validacao.py:272`, `:273`, `:279`.

**[bloqueia] Ressuscitação sem ato que represente a limpeza.** A porta grava fotos RESPOSTA/VALIDACAO e depois zera instante/texto/validação, explicitamente sem REABERTURA: `app/chamados/models.py:1629`, `:1640`, `:1647`. A nova leitura só limpa V por DEVOLUCAO/RECUSA/REABERTURA ou RESPOSTA sem motivo; portanto conserva a VALIDACAO fotografada: `app/chamados/models.py:4187`, `:4201`, `:4227`. `estado` devolve VALIDADA após esse reset: `app/chamados/models.py:3180`. Há cenário concreto já na suíte: fixture com validação e instante sem texto, reset esperado, somente fotos R/V e nenhum ato limpador: `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:263`, `:270`, `:280`. A mordida antiga verifica projeção e trilha, mas não `estado`/face SQL após o reset: `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:275`. Impacto em produção [nao verificado].

**[fila] `ato_motivo` passou a acumular descrição e controle de efeito.** O template exibe o motivo como explicação, enquanto o replay e SQL usam sua presença para decidir se a RESPOSTA desfaz V: `app/templates/chamados/partials/modal_fio.html:212`; `app/chamados/models.py:4201`, `:3496`. Os escritores diretos atuais concordam com essa convenção, mas `registrar` aceita qualquer motivo e as constraints não impõem o protocolo: `app/chamados/models.py:4248`, `:4125`. O outro leitor de conteúdo usa prefixo BUG-PAR-103 apenas para DEVOLUCAO: `app/chamados/services/materializacao.py:275`.

**[nota] Comentário residual contradiz a regra nova.** Diz “salvo a foto da projeção”, mas automática e trilha também preservam V: `app/chamados/models.py:4183`, `:4197`.

## 17/11 08:42 · sessao e193028b9d06 · ENSAIO-DA-FOLHA-11 · leitura · origem=Code

**A. Export Domínio**

A porta é `/folha/exportar/`; a geração usa **POST `/folha/exportar/gerar/`**, view `gerar_dominio_txt`, com permissão `editar_folha`. (`app/config/urls.py:34`; `app/folha/urls.py:13`; `app/folha/urls.py:14`; `app/folha/views.py:102`)

A cadeia efetiva é:

1. Sanitizar empresa/mês/ano → carregar empresa → `porta_export.conferir` → conferir integração Domínio → exigir algum fechamento gravado `aberto` ou `aprovado`. (`app/folha/views.py:109`; `app/folha/views.py:128`; `app/folha/views.py:138`; `app/folha/views.py:148`)
2. `recalcular_fechamento_mes(... somente_leitura=True, retornar_objeto=True)` → `classificar_export(... fechamentos=_fechs_lido)` → aprovar parte dos fechamentos gravados elegíveis. (`app/folha/views.py:207`; `app/folha/views.py:212`; `app/folha/views.py:243`)
3. `montar_linhas_export(... itens=_itens, fechamentos=_fechs_lido)` → recusar linhas vazias → `validar_layout_dominio` → `conteudo_do_txt` → `nome_canonico` → resposta TXT, auditoria e `ExportacaoDominio.registrar`. (`app/folha/views.py:268`; `app/folha/views.py:270`; `app/folha/views.py:275`; `app/folha/views.py:280`; `app/folha/views.py:285`; `app/folha/views.py:288`; `app/folha/views.py:297`)

**Efeitos e pureza:**

| Função/caminho | Efeito observado no código |
|---|---|
| `porta_export.conferir` | Consulta o carimbo; exige medição de hoje, mesma competência e zero falhas. Não chama `lavrar`, que é o escritor separado do snapshot. (`app/folha/porta_export.py:800`; `app/folha/porta_export.py:768`) |
| `aprovar_lote` | Atualiza `FechamentoMensal`: status, aprovador e data. A view registra `aprovacao_por_geracao`. (`app/ponto/services/fechamento.py:1323`; `app/folha/views.py:250`) |
| `ExportacaoDominio.registrar` | Sempre cria registro com conteúdo, SHA-256, usuário e contagem de linhas; também marca reexportação. (`app/folha/models.py:174`) |
| Auditoria do POST | Grava `export_txt` e, quando houve aprovação, `aprovacao_por_geracao`. (`app/folha/views.py:254`; `app/folha/views.py:289`; `app/core/observ.py:94`) |
| TXT em disco | A view entrega `HttpResponse`; a cópia fica no `TextField` de `ExportacaoDominio`, sem salvamento explícito de TXT em disco nessa cadeia. (`app/folha/views.py:285`; `app/folha/views.py:297`; `app/folha/models.py:86`) |
| Recalculador em leitura | Evita salvar fechamento, mas contém emissão de logs mesmo nesse modo; o logger `ponto` está configurado para arquivo. (`app/ponto/services/fechamento.py:1128`; `app/ponto/services/fechamento.py:845`; `app/ponto/services/fechamento.py:987`; `app/config/settings/saas.py:160`; `app/config/settings/saas.py:181`) |
| Juiz/montador | Consultam banco e células; alcançam cache de catálogo em RAM e logging condicional da resolução de convenção. Portanto não são estritamente puros. (`app/folha/export.py:740`; `app/folha/export.py:1247`; `app/ponto/catalogo/ausencias.py:267`; `app/core/regua_cct.py:269`) |
| Transformações puras | `montar_linha_10`, `montar_bloco_falta`, `partes_da_linha`, `validar_layout_dominio`, `agregar_export_dominio`, `conteudo_do_txt` e `hash_do_txt` operam sobre entradas já fornecidas. (`app/folha/export.py:34`; `app/folha/export.py:215`; `app/folha/export.py:224`; `app/folha/export.py:266`; `app/folha/export.py:310`; `app/folha/export.py:525`; `app/folha/export.py:542`) |

**Conteúdo e hash sem gravar:** existe a composição `montar_linhas_export(empresa, 11, 2026)` → `conteudo_do_txt(linhas)` → `hash_do_txt(conteudo)`, sem registrar exportação nem aprovar fechamento. O conteúdo usa CRLF, inclusive no final; lista vazia vira `''`, cujo SHA-256 também é calculável. (`app/folha/export.py:690`; `app/folha/export.py:746`; `app/folha/export.py:539`; `app/folha/export.py:553`)

**Mas ela não cumpre uma garantia literal de zero escrita incluindo cache/log:** o montador alcança `vigente()`, que atualiza cache em RAM quando frio ou vencido; a consulta de convenção pode emitir warning. Somente `conteudo_do_txt` + `hash_do_txt`, recebendo linhas prontas, é a transformação pura. Por esse critério literal, A recebe **“nao existe”** no mapa. (`app/folha/export.py:740`; `app/folha/export.py:185`; `app/ponto/catalogo/ausencias.py:331`; `app/ponto/catalogo/ausencias.py:276`; `app/core/regua_cct.py:269`; `app/folha/export.py:539`; `app/folha/export.py:553`)

**Competência aberta e fechamento:**

- O montador padrão lê `FechamentoMensal` existente e aceita `aberto` e `aprovado`; não exige aprovação prévia. Sem fechamentos, retorna linhas vazias. A porta HTTP recusa explicitamente quando não encontra nenhum fechamento nesses dois status. (`app/folha/export.py:687`; `app/folha/export.py:714`; `app/folha/export.py:719`; `app/folha/views.py:163`)
- Há parâmetro `fechamentos=` para objetos fornecidos pelo chamador; o admin usa objetos recalculados em leitura, enquanto a chamada padrão lê os gravados. Portanto são autoridades distintas para os valores. (`app/folha/export.py:700`; `app/folha/views.py:207`; `app/folha/views.py:268`)
- Para corte cadastrado igual a 21, novembro corresponde a **21/10–20/11/2026**; o corte é por empresa, com default 21 e limite de 2 a 28. O corte efetivo das empresas do banco de ensaio está **[nao verificado]**. (`app/ponto/janelas.py:10`; `app/ponto/janelas.py:36`; `app/ponto/janelas.py:54`)

**Quem decide e quais motivos devolve:**

- **`classificar_export` decide quem entra**, nesta precedência: `rescisao_modulo_proprio` → `sem_codigo_dominio` → `ferias_com_batida` → `cadastro_zero` → `furo_espelho` → `nao_homologado` → entrada com `motivo=None`. (`app/folha/export.py:765`; `app/folha/export.py:842`; `app/folha/export.py:866`; `app/folha/export.py:959`; `app/folha/export.py:968`; `app/folha/export.py:974`)
- Separadamente, devolve `travas_aprovacao`: `conta_vazia`, `turno_aberto`, `sem_ancora_12x36`. Essas travas afetam o carimbo de aprovação; não substituem a decisão de entrada. O carimbo do TXT ignora `conta_vazia`. (`app/folha/export.py:830`; `app/folha/export.py:848`; `app/folha/export.py:762`; `app/folha/views.py:243`)
- **`motivos_retencao_celula` decide a retenção pelo espelho**; a função compatível `motivos_retencao_espelho` ignora os três primeiros argumentos e delega a ela. Isento e desligado retornam vazio. (`app/folha/export.py:1301`; `app/folha/export.py:1205`)
- Seu vocabulário é `sem_celula`, `espelho_furo`, `espelho_discordante`, `espelho_indefinida`, `espelho_cobranca_indevida`, `espelho_cobrado`, `espelho_nunca_bateu`, `espelho_fato_em_ausencia`. Há exceções para protestos integralmente não cobráveis e furo somente de pausa. (`app/folha/export.py:1272`; `app/folha/export.py:1285`; `app/folha/export.py:1294`; `app/escala/models.py:1767`)
- **`prontidao` é indicador**, com `{pct, trab, previsto, cego, motivo}`; motivos `isento`, `rescisao`, `sem_escala` ou `None`. Possui memo de janela em RAM e não participa da cadeia decisória do montador. (`app/folha/export.py:1025`; `app/folha/export.py:1037`; `app/folha/export.py:1063`; `app/folha/export.py:1108`; `app/folha/export.py:714`)
- Os retornos não trazem responsável estruturado `admin`/`colaborador`/`sistema`. Há `detalhe` com orientação textual, como cadastrar vínculo/escala e recalcular, mas sem campo de atribuição de ação. (`app/folha/export.py:837`; `app/folha/export.py:961`; `app/folha/export.py:1259`)

**Conferências e rollout:**

- `validar_layout_dominio` confere tamanho 43 do reg10, constante `11`, campos numéricos e quantidade de reg11 da rubrica 8792; reg11 alcançado individualmente tem tamanho, flag e data conferidos. **Limitação:** os filhos reg11 contados sob 8792 são pulados por `i=j`, sem passar pela validação individual de data/formato. (`app/folha/export.py:266`; `app/folha/export.py:280`; `app/folha/export.py:291`; `app/folha/export.py:294`)
- `agregar_export_dominio` soma valores inteiros crus por matrícula/rubrica; ignora registros incompatíveis. Não compara com fechamento, não converte horas por 100 e não separa competência/empresa na chave. (`app/folha/export.py:310`)
- “Em rollout” lê-se pelo booleano `Empresa.em_rollout`, default `False`. `colaboradores_em_operacao` acrescenta empresa em rollout e vínculo vigente ao universo de colaboradores comuns. O dropdown do export usa `empresas_visiveis`: ativas, excluindo CNPJ `11222333*`, sem exigir rollout. (`app/colaboradores/models.py:45`; `app/colaboradores/queries.py:50`; `app/colaboradores/queries.py:78`; `app/folha/views.py:89`)

**B. Espelho-ponto em lote**

A porta é **`/relatorios/espelho/lote/`**, view `espelho_lote`, com login e verificação de staff. GET mostra formulário; POST pede empresa/competência ou datas livres e devolve HTML de progresso. O download final é **`/relatorios/espelho/lote/<id>/baixar/`**, view `baixar_lote_espelho`, com resposta `application/pdf`. (`app/config/urls.py:30`; `app/relatorios/urls.py:29`; `app/relatorios/urls.py:33`; `app/relatorios/views.py:1151`; `app/relatorios/views.py:1255`; `app/relatorios/views.py:1311`)

A cadeia é `espelho_lote` → `pedir` → `disparar` → command `gerar_cartoes_em_lote` → `assumir`/`executar` → **`gerar_pdf_espelho_lote_bytes`**. O lote produz PDF único, em ordem alfabética, com espelhos por colaborador. (`app/relatorios/views.py:1227`; `app/relatorios/views.py:1246`; `app/relatorios/lote_espelho.py:127`; `app/relatorios/management/commands/gerar_cartoes_em_lote.py:52`; `app/relatorios/management/commands/gerar_cartoes_em_lote.py:57`; `app/relatorios/lote_espelho.py:205`; `app/relatorios/services.py:165`)

**Há escritas em várias camadas:**

- Pedido cria `LoteEspelho`; o clique grava `cartao_lote`, ou `cartao_lote_dedup` para pedido repetido vivo. (`app/relatorios/lote_espelho.py:103`; `app/relatorios/views.py:1238`; `app/relatorios/views.py:1251`)
- Disparo cria diretório/log em mídia, escreve o log, inicia subprocesso e atualiza PID/início. (`app/relatorios/lote_espelho.py:124`; `app/relatorios/lote_espelho.py:130`; `app/relatorios/lote_espelho.py:132`; `app/relatorios/lote_espelho.py:138`)
- Execução atualiza progresso, salva PDF em `cartoes_lote/`, atualiza resultado e registra evento final. Até o polling pode gravar estado de erro pelo vigia de processo morto. (`app/relatorios/lote_espelho.py:203`; `app/relatorios/lote_espelho.py:250`; `app/relatorios/lote_espelho.py:254`; `app/relatorios/models.py:125`; `app/relatorios/views.py:1272`; `app/relatorios/lote_espelho.py:75`)

**Fora do HTTP:** o gerador aceita `request=None`, mas também exige `user`. No modo competência chama `gerar_pdf_espelho` → `gerar_pdf_espelho_bytes` → `gerar_pdf_auditavel`, que cria `RegistroPDFAuditavel`. Portanto, **não basta chamá-lo diretamente para obter geração sem escrita**. (`app/relatorios/services.py:165`; `app/relatorios/services.py:235`; `app/relatorios/services.py:70`; `app/relatorios/pdf_espelho.py:1103`; `app/relatorios/pdf_base.py:152`)

Com **ambas as datas explícitas**, há outro ramo que evita o auditável e gera PDF informacional em `BytesIO`. Ele exige parâmetros adicionais e não estabelece garantia estrita de zero cache/log nos serviços transitivos. Se a tentativa auditável falhar, o lote também usa esse informacional como fallback. (`app/relatorios/services.py:211`; `app/relatorios/services.py:235`; `app/relatorios/services.py:244`; `app/relatorios/services.py:250`; `app/relatorios/pdf_espelho.py:1056`; `app/ponto/services/espelho.py:557`; `app/ponto/catalogo/ausencias.py:267`)

**O papel por colaborador contém:**

- Nome, matrícula **Hasner**, CPF, admissão, empresa, posto, cargo, escala com código/ciclo/horários e carimbo da convenção. **PIS não é impresso no cabeçalho**, embora exista no cadastro. (`app/relatorios/pdf_espelho.py:731`; `app/relatorios/pdf_espelho.py:738`; `app/relatorios/pdf_espelho.py:743`; `app/colaboradores/models.py:335`)
- Totais: trabalhadas; HE 50%/100% ou HE genérica; adicional noturno; atraso; saída antecipada; intrajornada; dias de abono, falta e em aberto; saldo mensal de banco quando diferente de zero. (`app/relatorios/pdf_espelho.py:766`; `app/relatorios/pdf_espelho.py:773`; `app/relatorios/pdf_espelho.py:782`; `app/relatorios/pdf_espelho.py:791`; `app/relatorios/pdf_espelho.py:815`)
- Grade diária: previsto, marcações, motivo, trabalhado pago e colunas condicionais NOTURNO, ATRASO, S.ANT, HE50, HE100 e INTRAJ; inclui marcações espúrias e informação de procedência. (`app/relatorios/pdf_espelho.py:844`; `app/relatorios/pdf_espelho.py:857`; `app/relatorios/pdf_espelho.py:885`; `app/relatorios/pdf_espelho.py:950`; `app/relatorios/pdf_espelho.py:976`)

Para competência aberta, o coletor limita os dias até ontem; selecionar novembro não implica imprimir jornadas futuras até 20/11. O lote inteiro inclui, por padrão, situações `ativo`, `afastado` e `ferias`; ausência de batidas é informativo e não elimina o colaborador. (`app/relatorios/pdf_espelho.py:287`; `app/relatorios/services.py:102`; `app/relatorios/services.py:165`; `app/relatorios/services.py:228`)

**O trabalho longo vai para job.** A configuração tem `gthread`, quatro threads e timeout 30; a própria view documenta que esse timeout não limita a duração de uma requisição longa e desloca o caderno para processo separado. (`app/gunicorn.conf.py:19`; `app/gunicorn.conf.py:21`; `app/relatorios/views.py:1216`; `app/relatorios/lote_espelho.py:132`)

**C. Casamento sem nome/CPF**

O PDF imprime **`matricula_hasner`**; o TXT usa **`codigo_dominio.strip().zfill(10)`**, no campo matrícula, posições Python `[2:12]`. São campos distintos: não há chave técnica comum direta impressa nos dois artefatos. (`app/relatorios/pdf_espelho.py:739`; `app/folha/export.py:728`; `app/folha/export.py:251`; `app/colaboradores/models.py:302`; `app/colaboradores/models.py:311`)

A ponte por leitura de cadastro é **matrícula Hasner → Colaborador → código Domínio → matrícula TXT**, preservando o contexto da empresa. A matrícula Hasner é única e anulável; `codigo_dominio` não declara unicidade. Igualdade, preenchimento e duplicidades reais no banco de ensaio: **[nao verificado]**. (`app/colaboradores/models.py:302`; `app/colaboradores/models.py:311`; `app/colaboradores/models.py:321`; `app/folha/export.py:728`)

MAPA: A nao existe; B nao

## 17/11 08:41 · sessao 79086f9ebfb3 · COMPLEMENTO-2-QUEIMA-1711 · verificador · origem=Code

**AVAL-DEVOLVIDO — por item:** a porta lateral exigida pelo item 2 não está disponível nas portas examinadas; no item 5, faltam fechar atividade real e idade da reserva. (`bin/sombra.sh:67-80,255-260`; `bin/sonda_leitura.sh:63-70,93-97`; `bin/codex_limite_estado.py:82-88`; `fila_reserva.md:3-10`, literal)

Referências aos anexos contam linhas desde a primeira linha de cada literal fornecido.

**Item 2**

**1. As premissas conferem? Parcialmente.**

- **Banco lateral:** há previsão de `juliani_ref_*` dentro de `sombra_db`, mas isso não constitui uma porta pronta de restore histórico. `sombra.sh` fixa `ALVO=sombra` e executa drop/create/restore nesse alvo; a sonda escolhe apenas produção ou sombra. O settings da sombra também fixa nome e servidor. (`CLAUDE.md:205-206`; `bin/sombra.sh:67-80,236-260`; `bin/sonda_leitura.sh:63-70,93-97`; `app/config/settings/sombra.py:29,47-50`)
- **Tabela no dump:** o backup inclui o schema inteiro; `folha` pertence aos aplicativos do tenant e sua migration cria `RetratoFolha`. Isso sustenta a inclusão **se a tabela existia naquele schema quando o dump foi produzido**. Existência, integridade, tabela e retratos efetivos dos dumps de 03/11 e 10/11: **[nao verificado]**. (`scripts/backup_tenant.sh:3-12`; `app/config/settings/saas.py:34-45`; `app/folha/migrations/0011_retratofolha.py:15-29`)
- **Leitura versus derivação:** a consulta usa `linhas` gravadas e não chama o motor nem o lavrador. Entretanto, executa os leitores atuais: `dono_da_linha` computa o balde exibido mediante fio gravado, lookup e teto temporal; para legado sem fio válido, há interpretação por `_fio_cru`. É leitor canônico do painel, não uma coluna histórica de balde simplesmente copiada. (`smk_b_retrato.py:7-20`, literal; `app/folha/retrato_folha.py:351-376,436-449,468-493`)
- **“Divisão do painel”:** na consulta fornecida, corresponde ao eixo `dono`: Admin, Colab, Hoje, Sem ação disponível e Sem leitura (sistema). `chamado` entra em Admin; a chave JSON `dono` guarda o fio. A consulta imprime também o eixo separado `tipo`. (`smk_b_retrato.py:19-25`, literal; `app/folha/retrato_folha.py:170-201`)

**2. Colide com regra vigente?**

O desenho lateral não colide por si: bancos laterais no servidor de ensaio já estão previstos. Executá-lo por `sombra.sh --refazer`, porém, substituiria justamente a sombra excluída pelo aval; `scripts/restore_tenant.sh` também não serve como alternativa, pois é expressamente vedado pela regra. (`CLAUDE.md:205-206`; `bin/sombra.sh:255-260`; `complemento2_queima_util_1711.txt:3,7`, literal)

PostgreSQL e leitor devem respeitar o CPUSET de teste; dado pessoal não pode ser exportado, e a sessão principal não pode fazer a sonda inline. Medir espaço, registrar insuficiência e esperar disputa de disco/CPU, sem pedir a trava, são condições adicionais expressas, ainda sem execução demonstrada: **[nao verificado]**. Uma porta nova, se construída como instrumento, exige contrato prévio. (`CLAUDE.md:87,206,344-348,357-365,394`; `bin/sombra_db.sh:91-98`; `complemento2_queima_util_1711.txt:5`, literal)

**3. Que condições não fecham no literal?**

- **“Mesma consulta” não recupera automaticamente a tela histórica.** O código atual interpreta linhas antigas; `hoje=timezone.localdate()` participa do balde Hoje, e `em_rollout` será lido de cada banco. Portanto, referência temporal e universo de empresas também podem variar. A ressalva do item 3 deve acompanhar a comparação, sem atribuição causal à obra. (`smk_b_retrato.py:9,11,16-20`, literal; `app/folha/retrato_folha.py:489-493`; `complemento2_queima_util_1711.txt:4`, literal)
- **Schema antigo:** compatibilidade física com o ORM atual e formato efetivo do JSON são **[nao verificado]**. O modelo documenta evolução de chaves sem migration e leitura de legado; isso não prova compatibilidade dos dois dumps concretos. (`app/folha/models.py:304-323,334-341`)
- **“Sem retrato” versus zero:** a consulta não imprime uma empresa sem retrato e inicializa totais que podem terminar em zero; `linhas_do_retrato(None)` também retorna lista vazia. É necessário distinguir presença por empresa/competência antes de totalizar. Falha de schema tampouco prova “sem retrato”. (`smk_b_retrato.py:13-16,30-34`, literal; `app/folha/retrato_folha.py:98-111`; `complemento2_queima_util_1711.txt:3`, literal)
- **Timestamp e descarte:** `lavrado_em` significa início da rodada do lavrador. Apagar o lateral ao fim está autorizado, mas encerramento com alvo protegido, inclusive em falha/interrupção, não foi demonstrado: **[nao verificado]**. Os anexos fornecem a consulta, sem os resultados numéricos das três datas: **[nao verificado]**. (`app/folha/models.py:356-360`; `smk_b_retrato.py:1-34`, literal; `complemento2_queima_util_1711.txt:3-4`, literal)

**4. Toca exportada, dinheiro ou `!` fora do declarado?**

O escopo autoriza restaurar e descartar o lateral e ler retratos existentes; não autoriza alterar produção, gravado exportado ou recalcular folha. Mantido esse limite, não há movimento de dinheiro/exportada declarado. Cumprimento na execução futura: **[nao verificado]**. (`complemento2_queima_util_1711.txt:3-5,7`, literal; `CLAUDE.md:265-268,411-413`)

**Item 5**

**1. As premissas conferem? Parcialmente.**

- **Publicação:** `relato.sh` gera `ESTADO.md`, passa pela raspagem, copia ao repositório definido por `DEST_RELATO` — padrão `/home/ronald/hasner-relato` — e faz commit/push para `origin HEAD`. O destino remoto concreto da instalação é **[nao verificado]**. (`bin/relato.sh:2,23,37-38,114-117,145-147`)
- **“Em curso”:** o pareamento atual usa `(obra, hash, pid)` e chama de “em curso” um `ini` sem `fim`, sem conferir processo vivo. O PID registrado é `$$`, do wrapper; `ini` antecede espera por slot, trava, possíveis recusas e execução do Codex. O código reconhece que isso registra tentativa, sem provar execução. (`bin/codex_limite_estado.py:63-67,82-88`; `bin/codex_pergunta.sh:272-273,308-309,364-394,425-426`; `bin/disjuntor.py:2071-2075`)
- **Fila:** o literal contém zero ABERTOS: R1 DESPACHADO e R2–R7 ENTREGUE. Há horários junto aos estados, mas nenhuma evidência de que cada abertura receba data/hora própria. Essa premissa permanece **[nao verificado]**. (`fila_reserva.md:3-10`, literal)

**2. Colide com regra vigente?**

A nova redação resolve objeções anteriores: substitui “maior intervalo”, fixa a foto, o predicado do alarme, ausência/ilegibilidade como SEM DADO, ausência de despacho automático e desligamento por Ronald. Não revoga CODEX-NAO-FICA-OCIOSO nem os cortes de orçamento. (`complemento2_queima_util_1711.txt:6,10`, literal; `fila_reserva.md:1-2`, literal; `bin/disjuntor.py:1468-1485`)

Continuam exigidos contrato antes da construção, preservação dos indicadores existentes e pouso próprio de instrumento. Já existe produtor da linha Codex; contrato efetivamente registrado para esta alteração: **[nao verificado]**. (`bin/gerar_estado.py:267-270,337-339`; `CLAUDE.md:327-328,389-394`; `app/docs/PISTA.md:170-176`)

**3. Que condições não fecham no literal?**

- **Atividade e órfão:** falta fechar se espera do wrapper conta como chamada em curso. `fim` depende do trap EXIT e de gravação no log; um processo morto sem encerramento registrado continuaria “em curso” no leitor atual. Um ABERTO de 45 minutos mais esse órfão esconderia o alarme exigido. Ocorrência real desse órfão: **[nao verificado]**. (`bin/codex_pergunta.sh:269-282,364-394`; `bin/codex_limite_estado.py:82-88`; `complemento2_queima_util_1711.txt:6`, literal)
- **Hora do ABERTO:** é necessário um instante de abertura com data e hora; horário do estado exibido não comprova abertura. ABERTO sem hora deixa a idade **SEM DADO**, sem permitir idade zero ou verde. Com nenhum ABERTO, idade é não aplicável. (`fila_reserva.md:3-10`, literal; `CLAUDE.md:35-38`; `complemento2_queima_util_1711.txt:6`, literal) [nao verificado]
- **Instante da foto:** gerar ESTADO ocorre antes das demais etapas e do push. É preciso fixar a coleta em relação à publicação; essas etapas não constituem uma leitura atômica. (`bin/relato.sh:38-42,97-117,145-147`)
- **Borda das 24 h:** a janela pertence ao contexto de inícios por hora; uma chamada ainda viva pode ter começado antes dela. Faltam explicitar limites e agrupamentos horários. O leitor atual aceita a borda exata e timestamps futuros porque testa apenas `agora-t <= 24h`; arquivo ausente vira listas vazias, incompatível com reutilização direta para SEM DADO. (`bin/codex_limite_estado.py:49-78`; `complemento2_queima_util_1711.txt:6`, literal)
- **Mordida:** com dados válidos, ABERTO de 45 minutos e nenhuma chamada acusa; com uma chamada realmente em curso, não acusa; exatamente 30 minutos não satisfaz `>30`. A amostra tem vinte pares completos `ini/fim`, mas é uma cauda, sem prova da foto na publicação nem das 24 horas completas: **[nao verificado]**. (`complemento2_queima_util_1711.txt:6`, literal; `chamadas_amostra.log:1-40`, literal)

**4. Toca exportada, dinheiro ou `!` fora do declarado?**

O escopo mede log/fila e publica agregados; não declara alteração de dinheiro ou competência exportada. A publicação passa pelo fluxo público de raspagem, sem autorização para publicar fontes brutas. Implementação futura respeitando esses limites: **[nao verificado]**. (`complemento2_queima_util_1711.txt:6`, literal; `bin/relato.sh:2,114-117`; `CLAUDE.md:265-268`)

VEREDITO: item 2 PROCEDE -- falta fechar porta lateral nomeada, compatibilidade e ausência distinta de zero (`bin/sombra.sh:67-80,255-260`; `smk_b_retrato.py:13-16,30-34`, literal); item 5 PROCEDE -- ini sem fim não prova atividade e a hora de abertura não está demonstrada (`bin/codex_limite_estado.py:82-88`; `fila_reserva.md:3-10`, literal).

## 17/11 08:41 · sessao b9a945b88925 · - · - · origem=Ronald (tmux)

Auditoria de pureza:

- O caminho `montar_linhas_export` não persiste modelos diretamente, mas chama `datas_falta` para cada fechamento selecionado (`app/folha/export.py:725`, `app/folha/export.py:740`). Essa chamada chega a `descontam_vigentes` (`app/folha/export.py:185`, `app/folha/export.py:180`), que chama `vigente` (`app/ponto/catalogo/ausencias.py:331`). Com cache frio ou vencido, `vigente` escreve `_CACHE['v']` e `_CACHE['t']` em RAM; TTL de 60 segundos (`app/ponto/catalogo/ausencias.py:257`, `app/ponto/catalogo/ausencias.py:276`, `app/ponto/catalogo/ausencias.py:279`).
- Portanto, a montagem canônica empresa/competência não oferece contrato de **zero escrita incluindo cache**, embora ofereça separação da persistência ORM (`app/folha/export.py:697`, `app/ponto/catalogo/ausencias.py:279`). Sob leitura literal do pedido, recomendo `MAPA: A nao existe`, explicando que existe montagem sem persistência, mas não rigorosamente pura (`app/folha/export.py:740`, `app/ponto/catalogo/ausencias.py:279`).
- `conteudo_do_txt` e `hash_do_txt` são puros para entradas já prontas: CRLF e SHA-256, respectivamente (`app/folha/export.py:539`, `app/folha/export.py:552`). Isso não resolve a seleção/montagem empresa/competência (`app/folha/export.py:714`, `app/folha/export.py:725`).
- Há log condicional transitivo: `montar_linhas_export` chama `feriado_12x36_em_dobra`, que consulta `regua_para`, chegando a `_aplicacao_vigente`; empate entre aplicações vigentes emite `logger.warning` (`app/folha/export.py:732`, `app/folha/export.py:380`, `app/core/regua_cct.py:366`, `app/core/regua_cct.py:269`). Em SaaS esse WARNING chega ao console; o handler de arquivo da raiz aceita ERROR (`app/config/settings/saas.py:155`, `app/config/settings/saas.py:137`, `app/config/settings/saas.py:183`).
- `recalcular_fechamento_mes(somente_leitura=True, retornar_objeto=True)` constrói objetos destacados e coleta retorno, evitando o `save/update` de FM (`app/ponto/services/fechamento.py:876`, `app/ponto/services/fechamento.py:890`, `app/ponto/services/fechamento.py:1128`). Contudo, há INFO condicionais fora da proteção dessa flag e ERROR por exceção (`app/ponto/services/fechamento.py:845`, `app/ponto/services/fechamento.py:987`, `app/ponto/services/fechamento.py:1305`). O logger `ponto` escreve INFO em `/app/logs/ponto.log` (`app/config/settings/saas.py:160`, `app/config/settings/saas.py:181`).

Espelho:

- O lote por competência tenta o auditável por colaborador (`app/relatorios/services.py:235`, `app/relatorios/services.py:237`), cuja cadeia cria `RegistroPDFAuditavel` (`app/relatorios/services.py:70`, `app/relatorios/pdf_espelho.py:1103`, `app/relatorios/pdf_base.py:152`).
- Passar ambas as datas explícitas pula o auditável e chama diretamente o informacional; o resultado é unido em `BytesIO` (`app/relatorios/services.py:211`, `app/relatorios/services.py:235`, `app/relatorios/services.py:246`, `app/relatorios/services.py:250`, `app/relatorios/services.py:262`). Esse é um ramo com argumentos adicionais, e continua sujeito ao cache transitivo e aos logs do motor; não constitui contrato cache/log-zero apenas com empresa/competência (`app/relatorios/services.py:165`, `app/relatorios/pdf_espelho.py:394`, `app/core/regua_cct.py:448`).
- O informacional usa `BytesIO`, `SimpleDocTemplate` e devolve bytes, sem criar registro auditável nesse corpo (`app/relatorios/pdf_espelho.py:1056`, `app/relatorios/pdf_espelho.py:1070`). Ele também consulta `get_commit_git`, que executa somente `git rev-parse HEAD` ou lê variável de ambiente (`app/relatorios/utils_audit.py:12`, `app/relatorios/utils_audit.py:18`).
- O job externo escreve progresso, arquivo, estado final e evento de observabilidade (`app/relatorios/lote_espelho.py:203`, `app/relatorios/lote_espelho.py:250`, `app/relatorios/lote_espelho.py:251`, `app/relatorios/lote_espelho.py:254`).
- `calibrar_ancora_se_aplicavel` está aposentada e retorna zero; não encontrei chamada do gerador para o escritor `recalibrar_ancora_retroativa` (`app/escala/utils.py:209`, `app/escala/utils.py:214`; ausência de chamada: [nao verificado]).

## 17/11 08:40 · sessao 0e8e8e08493b · - · - · origem=Ronald (tmux)

A — evidência consolidada, somente leitura estática:

- A porta está montada sob `/folha/`, com formulário em `exportar/` e POST em `exportar/gerar/`, nomes `folha:exportar` e `folha:gerar_txt` (`app/config/urls.py:34`; `app/folha/urls.py:13-14`). A tela exige `editar_folha`; o gerador também exige essa ação e método POST (`app/folha/views.py:83-106`). A prévia monta o formulário POST quando há elegíveis (`app/folha/views.py:416-430`).
- Cadeia efetiva do POST: sanitiza empresa/mês/ano → carrega Empresa → `porta_export.conferir` → `codigo_no_sistema_dominio` → exige algum `FechamentoMensal` gravado aberto/aprovado → `recalcular_fechamento_mes(... somente_leitura=True, retornar_objeto=True)` → `classificar_export(... fechamentos=_fechs_lido)` → aprova os elegíveis sem travas consideradas → `montar_linhas_export(... itens=_itens, fechamentos=_fechs_lido)` → `validar_layout_dominio` → `conteudo_do_txt` → `nome_canonico` → resposta TXT + auditoria + `ExportacaoDominio.registrar` (`app/folha/views.py:109-166`, `207-268`, `275-301`).
- `conferir` apenas consulta `MetricaSnapshot`; exige carimbo de hoje, da mesma competência, medição válida e zero falhas; devolve mensagem de recusa quando falta ou diverge (`app/folha/porta_export.py:800-867`). `lavrar`, função separada, escreve esse snapshot por `update_or_create` (`app/folha/porta_export.py:768-796`).
- Escritas do POST: `aprovar_lote` faz `qs.update(status='aprovado', aprovado_por, aprovado_em)`; a porta só o chama sobre gravados abertos dos elegíveis sem travas, ignorando `conta_vazia`; registra auditoria `aprovacao_por_geracao` (`app/folha/views.py:243-256`; `app/folha/export.py:762`; `app/ponto/services/fechamento.py:1323-1332`). Registra também auditoria `export_txt` e nova `ExportacaoDominio`, com conteúdo, hash, linhas, usuário e marca de reexportação (`app/folha/views.py:288-300`; `app/folha/models.py:174-188`). `registrar_log` chega a `LogAuditoria.objects.create` (`app/core/auditoria.py:8-25`; `app/core/observ.py:94-138`). O arquivo é entregue como `HttpResponse`; nessa cadeia o conteúdo fica no banco como `TextField`, sem escrita explícita de TXT em disco (`app/folha/views.py:280-301`; `app/folha/models.py:82-86`).
- `montar_linhas_export` não aprova nem registra exportação: devolve `(linhas, entra_set)`; usa integração ativa, rubricas empresa sobre global, juiz de entrada e fechamentos aberto/aprovado (`app/folha/export.py:690-746`, `984-991`). Converte `colaborador.codigo_dominio.strip().zfill(10)` no campo chamado matrícula do TXT, competência `AAAAMM`, eventos com valor não zero e rubrica cadastrada, mais datas de falta (`app/folha/export.py:723-745`).
- Existe composição sem persistência de negócio: `linhas, ids = montar_linhas_export(empresa, 11, 2026)`; `conteudo = conteudo_do_txt(linhas)`; `hash = hash_do_txt(conteudo)` (`app/folha/export.py:690-746`, `525-553`). `conteudo_do_txt` é junção CRLF com CRLF final, ou `''` se vazio; hash é SHA-256 de `conteudo.encode()` (`app/folha/export.py:539`, `552-553`).
- Esse caminho padrão lê os `FechamentoMensal` existentes; sem eles, o juiz e o montador iteram vazio e devolvem linhas vazias, conteúdo `''` e SHA-256 dessa string — não há recusa de competência aberta no montador (`app/folha/export.py:783-788`, `714-746`, `539`, `553`). Aberto é status admitido (`app/folha/export.py:687`, `717-722`). A porta HTTP, por outro lado, recusa se não existir nenhum gravado aberto/aprovado, com “Nenhum fechamento calculado… recalcule a competencia” (`app/folha/views.py:148-166`).
- Para a mesma autoridade atual usada pelo POST, pode-se obter objetos destacados pelo recalculador e injetá-los no juiz/montador; o recalculador semeia a linha gravada se existir, senão constrói objeto `status='aberto'`, sem salvar no ramo de leitura (`app/ponto/services/fechamento.py:876-901`, `1128-1144`; `app/folha/views.py:207-212`, `265-268`). Não chamar isso de função pura: pode emitir logs `info`/`error`, mesmo em leitura (`app/ponto/services/fechamento.py:843-846`, `982-988`, `1303-1307`).
- Também não chamar `classificar_export`/montador de “puros” no sentido estrito: fazem consultas e alcançam cache de catálogo no processo, atualizado em `vigente()`; consulta CCT pode emitir warning quando há aplicações empatadas (`app/folha/export.py:789-791`, `974-978`, `1002-1013`, `363-380`, `740`; `app/ponto/catalogo/ausencias.py:257-280`; `app/core/regua_cct.py:265-274`). Não encontrei escrita persistente de célula nessa composição; os métodos de retenção consultam `CelulaDia` e escalas (`app/folha/export.py:1247-1298`). O agente purity está auditando a cadeia transitiva.
- Puramente locais: `montar_linha_10`, `montar_bloco_falta`, `partes_da_linha`, `validar_layout_dominio`, `agregar_export_dominio`, `rubrica_atraso_parcial`, `eventos_do_fechamento`, `conteudo_do_txt`, `hash_do_txt`, pelas operações de seus corpos (`app/folha/export.py:34-45`, `215-255`, `266-321`, `383-461`, `525-553`). `prontidao` tem memo global `_JANELA_MEMO` no processo, escrito quando resolve a janela, e não participa do juiz/montador (`app/folha/export.py:1022-1025`, `1063-1067`; `765-981`, `690-746`).
- Juiz de quem entra: `classificar_export`, cadeia de motivos efetivos: `rescisao_modulo_proprio` → `sem_codigo_dominio` → `ferias_com_batida` → `cadastro_zero` → `furo_espelho` → `nao_homologado` → entra (`app/folha/export.py:765-780`, `866-884`, `959-980`). `nao_aprovado` sobrevive na legenda da UI, mas não é emitido pelo juiz atual; o status não é porta do juiz (`app/folha/views.py:14-23`; `app/folha/export.py:851-855`).
- Segunda pergunta do mesmo juiz: `travas_aprovacao`, tupla de `conta_vazia`, `turno_aberto`, `sem_ancora_12x36`; afeta aprovação, separadamente de entrada no TXT (`app/folha/export.py:792-848`; `app/folha/views.py:215-248`).
- Juiz subordinado de retenção do espelho: `motivos_retencao_espelho` exige `fech`, ignora os três primeiros parâmetros e delega a `motivos_retencao_celula` (`app/folha/export.py:1301-1307`). Isento e desligado devolvem vazio; nos demais, motivos são `sem_celula` e `espelho_` + veredito acusador (`app/folha/export.py:1205-1213`, `1264-1298`). Vocabulário acusador: `furo`, `discordante`, `indefinida`, `cobranca_indevida`, `cobrado`, `nunca_bateu`, `fato_em_ausencia` (`app/escala/models.py:1767-1768`). Protestos todos não cobráveis e furo só de pausa isentam retenção (`app/folha/export.py:1285-1294`, `1173`).
- `prontidao` devolve `{pct, trab, previsto, cego, motivo}` como indicador; motivos `isento`, `rescisao`, `sem_escala`, ou `None`; não decide entrada (`app/folha/export.py:1025-1039`, `1068-1113`; `765-981`).
- O retorno do juiz não contém campo normalizado de “quem age” (`admin`, `colaborador`, `sistema`): item contém IDs/nome/código/status/motivo/detalhe/travas e extras específicos de cada motivo (`app/folha/export.py:837-850`, `866-980`). O retorno diário também contém apenas data, motivo, célula e veredito (`app/folha/export.py:1259-1262`). Há instruções em prosa em `detalhe`, por exemplo cadastrar vínculo/escala e recalcular no `cadastro_zero`, mas sem responsável estruturado (`app/folha/export.py:961-964`).
- `validar_layout_dominio`: reg10 com 43 caracteres; constante `11` nos offsets 22:24; campos numéricos matrícula, competência, rubrica, valor e código de sistema; reg8792 exige número de reg11 consecutivos igual ao valor; reg11 alcançado isoladamente verifica comprimento, dígitos, flag `1` e data válida; outros registros dão erro (`app/folha/export.py:266-307`). Limite concreto: no ramo8792 ele conta reg11 consecutivos e avança `i = j`, portanto pula validação individual de comprimento/data desses filhos (`app/folha/export.py:280-292`). Não valida semanticamente mês/competência, vínculo de rubrica com pessoa nem catálogo nesses testes de layout (`app/folha/export.py:266-307`).
- `agregar_export_dominio` não valida/reconcilia: aceita reg10 com 43 caracteres e valor inteiro, ignora demais e soma inteiros crus por matrícula e rubrica; não divide horas por100 nem agrupa por competência/código de empresa (`app/folha/export.py:310-321`).
- “Empresas em rollout” é campo `Empresa.em_rollout`, booleano default `False` (“Empresa em producao… piloto/rollout”); leitura ORM direta é filtro nesse campo (`app/colaboradores/models.py:40-46`). `colaboradores_em_operacao` significa colaboradores comuns, em empresa com essa flag e vínculo vigente hoje; comuns exige ativo, empresa ativa e username com onze dígitos (`app/colaboradores/queries.py:15-29`, `50-65`). Dropdown de export usa `empresas_visiveis`: ativas, exclui CNPJ11222333*, ordena razão social; não exige rollout (`app/folha/views.py:83-93`; `app/colaboradores/queries.py:78-87`).
- Janela11/2026 corresponde a21/10–20/11 somente com corte21; a função respeita o cadastro de corte por empresa, default21, clamp2–28 (`app/ponto/janelas.py:10-21`, `36-57`; `app/colaboradores/models.py:46`).

## 17/11 08:40 · sessao 863522386e97 · - · - · origem=Ronald (tmux)

B — Porta e cadeia:

- A rota completa é `/relatorios/espelho/lote/`: prefixo em `app/config/urls.py:30`, rota/view `espelho_lote` em `app/relatorios/urls.py:29`. A view exige login e `_requer_staff`; GET entrega formulário, POST lê empresa/mês/ano ou datas livres (`app/relatorios/views.py:1151`, `app/relatorios/views.py:1162`, `app/relatorios/views.py:1169`, `app/relatorios/views.py:1172`, `app/relatorios/views.py:1195`, `app/relatorios/views.py:1212`).
- O POST chama `lote_espelho.pedir(...)`, registra auditoria `cartao_lote` e chama `disparar`; pedido duplicado vivo registra `cartao_lote_dedup` (`app/relatorios/views.py:1227`, `app/relatorios/views.py:1238`, `app/relatorios/views.py:1246`, `app/relatorios/views.py:1251`).
- `pedir` cria `LoteEspelho`; `disparar` cria diretório de mídia, abre/grava `media/cartoes_lote/<id>.log`, inicia `nice -n 10 ... manage.py gerar_cartoes_em_lote <id> --schema ...`, atualiza PID/início e registra evento (`app/relatorios/lote_espelho.py:103`, `app/relatorios/lote_espelho.py:124`, `app/relatorios/lote_espelho.py:127`, `app/relatorios/lote_espelho.py:130`, `app/relatorios/lote_espelho.py:138`, `app/relatorios/lote_espelho.py:140`).
- O command chama `assumir` e `executar`; `assumir` grava estado/PID, `executar` chama o gerador `relatorios.services.gerar_pdf_espelho_lote_bytes`, grava progresso, salva PDF e estado/placar e evento final (`app/relatorios/management/commands/gerar_cartoes_em_lote.py:52`, `app/relatorios/management/commands/gerar_cartoes_em_lote.py:57`, `app/relatorios/lote_espelho.py:161`, `app/relatorios/lote_espelho.py:203`, `app/relatorios/lote_espelho.py:205`, `app/relatorios/lote_espelho.py:250`, `app/relatorios/lote_espelho.py:251`, `app/relatorios/lote_espelho.py:254`). O arquivo usa `FileField(upload_to='cartoes_lote/')` (`app/relatorios/models.py:125`).
- O download final é `/relatorios/espelho/lote/<id>/baixar/`, view `baixar_lote_espelho`, que abre o arquivo pronto em `rb` e responde `application/pdf` como anexo; não chama gerador (`app/relatorios/urls.py:33`, `app/relatorios/views.py:1295`, `app/relatorios/views.py:1308`, `app/relatorios/views.py:1311`). O POST retorna HTML da tela de progresso; o arquivo final é um PDF único (`app/relatorios/views.py:1255`, `app/relatorios/views.py:1274`, `app/relatorios/services.py:167`, `app/relatorios/services.py:262`).

Chamar sem HTTP ≠ chamar sem escrita:

- O gerador aceita `request=None`, mas exige argumento `user`, além de empresa/mês/ano (`app/relatorios/services.py:165`, `app/relatorios/services.py:175`). O próprio job o chama assim (`app/relatorios/lote_espelho.py:205`).
- Na competência padrão, ele chama `gerar_pdf_espelho` → `gerar_pdf_espelho_bytes` → `gerar_pdf_auditavel`, que executa `RegistroPDFAuditavel.objects.create`; portanto **não é gerador sem escrita só com empresa/competência**, mesmo sem criar job nem salvar arquivo (`app/relatorios/services.py:235`, `app/relatorios/services.py:237`, `app/relatorios/services.py:70`, `app/relatorios/pdf_espelho.py:1103`, `app/relatorios/pdf_base.py:152`).
- Se o auditável lança exceção, o lote tenta PDF informacional; só pula colaborador se esse também falhar (`app/relatorios/services.py:244`, `app/relatorios/services.py:250`, `app/relatorios/services.py:254`). Ausência de batidas é apenas informativo e não retira do caderno (`app/relatorios/services.py:228`, `app/relatorios/services.py:234`).
- Existe ramo distinto passando **ambas** `data_ini` e `data_fim`: ele evita tentativa auditável e chama diretamente `gerar_pdf_espelho_informacional_bytes`, que constrói PDF em `BytesIO` e retorna bytes (`app/relatorios/services.py:211`, `app/relatorios/services.py:235`, `app/relatorios/services.py:246`, `app/relatorios/services.py:250`, `app/relatorios/pdf_espelho.py:1056`, `app/relatorios/pdf_espelho.py:1070`). Isso não é a chamada “só empresa/competência”; ademais os serviços chamados pelo coletor possuem ramos de `logger.warning`, portanto não prometer silêncio absoluto de logs (`app/relatorios/pdf_espelho.py:598`, `app/ponto/services/espelho.py:557`, `app/ponto/services/espelho.py:570`, `app/ponto/services/espelho.py:651`).
- Não afirmar que ausência de fechamento necessariamente força fallback: embora a docstring mencione isso, o bloqueio implementado do auditável é disputa aberta; o coletor suporta ausência de `FechamentoMensal`, usando lavratura ou estado “sem apuracao ainda” (`app/relatorios/services.py:168`, `app/relatorios/pdf_espelho.py:1084`, `app/relatorios/pdf_espelho.py:1113`, `app/relatorios/cartao_pela_celula.py:71`, `app/relatorios/cartao_pela_celula.py:321`, `app/relatorios/cartao_pela_celula.py:340`).

Conteúdo impresso:

- Nome, matrícula **Hasner**, CPF, admissão, empresa, posto, escala (código/ciclo/horário), cargo e carimbo da convenção (`app/relatorios/pdf_espelho.py:731`, `app/relatorios/pdf_espelho.py:738`, `app/relatorios/pdf_espelho.py:743`). O cabeçalho completo não inclui PIS, `codigo_dominio` nem PK; PIS existe no cadastro (`app/relatorios/pdf_espelho.py:738`, `app/colaboradores/models.py:335`).
- Totais/badges: trabalhadas; HE 50%, HE 100% ou HE genérica quando as faixas não vieram; adicional noturno legal; atraso; saída antecipada; intrajornada; dias de abono/falta; dias em aberto; saldo mensal do banco quando não zero (`app/relatorios/pdf_espelho.py:766`, `app/relatorios/pdf_espelho.py:771`, `app/relatorios/pdf_espelho.py:782`, `app/relatorios/pdf_espelho.py:791`, `app/relatorios/pdf_espelho.py:815`).
- Dia a dia: dia, horário previsto, marcações, motivo, colunas condicionais NOTURNO/ATRASO/S.ANT/HE50/HE100/INTRAJ e trabalhado pago; marcações normais, espúrias com `*` e horários de isenção com `(i)`; palavra do dia e procedência de marcações (`app/relatorios/pdf_espelho.py:844`, `app/relatorios/pdf_espelho.py:857`, `app/relatorios/pdf_espelho.py:865`, `app/relatorios/pdf_espelho.py:881`, `app/relatorios/pdf_espelho.py:889`, `app/relatorios/pdf_espelho.py:923`, `app/relatorios/pdf_espelho.py:950`, `app/relatorios/pdf_espelho.py:976`).
- Na competência, os totais do cartão consultam `fechamento_lido` online; o TXT padrão usa `FechamentoMensal` gravado. Não presumir igualdade apenas pelos comentários de “mesma fonte” (`app/relatorios/cartao_pela_celula.py:73`, `app/folha/export.py:719`).
- O mês/ano resolve janela da competência; o coletor limita os dias até ontem. A validação HTTP que recusa data final hoje/futura ocorre no modo datas livres (`app/relatorios/pdf_espelho.py:661`, `app/relatorios/pdf_espelho.py:287`, `app/relatorios/views.py:1187`, `app/relatorios/services.py:38`). Portanto, para competência ainda aberta, não pressupor PDF até 20/11 com dias futuros preenchidos (`app/relatorios/pdf_espelho.py:287`, `app/relatorios/pdf_espelho.py:602`).
- O trabalho longo vai para processo/job; configuração tem `worker_class="gthread"`, quatro threads e timeout 30, e a view documenta que esse timeout não encerra requisição longa (`app/relatorios/lote_espelho.py:132`, `app/gunicorn.conf.py:19`, `app/gunicorn.conf.py:21`, `app/relatorios/views.py:1219`).
- Atenção ao polling rotulado “LEITURA”: `_tela_lote_espelho` chama `vigiar`, que pode gravar estado ERRO se processo morreu (`app/relatorios/views.py:1285`, `app/relatorios/views.py:1272`, `app/relatorios/lote_espelho.py:75`).

C — Chave de casamento:

- O espelho imprime `matricula_hasner`; o TXT preenche a “matrícula” usando `codigo_dominio.strip().zfill(10)`, posições 2:12 (`app/relatorios/pdf_espelho.py:739`, `app/folha/export.py:728`, `app/folha/export.py:738`, `app/folha/export.py:251`).
- Esses são campos distintos; `matricula_hasner` é única, mas nullable; `codigo_dominio` não tem `unique=True` (`app/colaboradores/models.py:302`, `app/colaboradores/models.py:311`). Os artefatos, portanto, não apresentam chave técnica comum direta para casamento sem nome/CPF; é necessária ponte de leitura no cadastro: matrícula Hasner → colaborador → código Domínio → matrícula TXT (`app/relatorios/pdf_espelho.py:739`, `app/colaboradores/models.py:302`, `app/colaboradores/models.py:311`, `app/folha/export.py:728`). Igualdade real desses valores no banco de ensaio: **[não verificado]**.

## 17/11 08:39 · sessao da21324c0f35 · - · - · origem=Ronald (tmux)

**Item 2 — evidências para consolidação**

1. **Premissas: parcialmente conferem.**
   - Existe previsão vigente de banco lateral `juliani_ref_*` **dentro de `sombra_db`**, separado de produção; isso não autoriza usar a sombra única como alvo deste item. (`CLAUDE.md:205-206`; `complemento2_queima_util_1711.txt:3,7`, literal)
   - **As portas indicadas não implementam restore para nome próprio:** `sombra.sh` crava `ALVO=sombra`, guarda esse nome e executa `dropdb/createdb/pg_restore` nele; a CLI não oferece alvo nem arquivo histórico específicos. `sonda_leitura.sh` aceita apenas arquivo, opcionalmente `--sombra`, escolhendo entre `saas_hasner` e `sombra`. (`bin/sombra.sh:67-80,236-260,436-495`; `bin/sonda_leitura.sh:63-70,93-97`)
   - Só mudar `POSTGRES_DB` não resolve: `config.settings.sombra` fixa nome `sombra` e host `sombra_db`. (`app/config/settings/sombra.py:29,47-50`)
   - O produtor de backup faz dump de schema inteiro, sem filtro de tabela; `folha` está nos `TENANT_APPS`, e a migration cria `RetratoFolha`, com `mes/ano/linhas/lavrado_em/empresa`. Portanto, um dump íntegro de `juliani` feito depois da aplicação dessa migration **deve** carregar a tabela. Existência, integridade, schema e conteúdo efetivos dos dumps de 03/11 e 10/11: **[nao verificado]**. (`scripts/backup_tenant.sh:3-12`; `app/config/settings/saas.py:34-45`; `app/folha/migrations/0011_retratofolha.py:15-29`)
   - `smk_b_retrato.py` lê o retrato gravado, filtra as empresas `em_rollout=True`, recorta bloqueados e agrupa em memória; não chama `montar_linhas`, `fora_da_folha` nem `classificar_export`. (`smk_b_retrato.py:7-20`, literal; `app/folha/retrato_folha.py:89-122,674-709`)
   - **`dono_da_linha` é o leitor canônico do eixo**, com lookup do fio gravado e teto temporal; não relavra nem consulta motor de cálculo. Para linha legada sem `dono`, usa `_fio_cru(status,motivo,None)`, interpretação prevista do gravado antigo. (`app/folha/retrato_folha.py:351-376,436-449,468-493`; `app/folha/models.py:315-323`)
   - “Divisão do painel”, se for a usada em `DONO(retrato)` pela consulta-base, corresponde a `Admin`, `Colab`, `Hoje`, `Sem ação disponível`, `Sem leitura (sistema)`; a chave JSON `dono` guarda **fio**, não esse balde da UI. A consulta também imprime outro eixo, `TIPO`; não é divisão organizacional cadastrada. (`smk_b_retrato.py:19-25`, literal; `app/folha/retrato_folha.py:167-201,588-597`)

2. **Colisão: não necessária para o desenho lateral; existe se tentarem encaixá-lo na porta errada.**
   - O banco nomeado deve ficar no servidor de ensaio, com lado PostgreSQL e Python no cpuset teste; os limites são `0-3` produção e `4-7` teste, e `sombra_db` nasce/é ajustado nesse cpuset. (`CLAUDE.md:87,205`; `bin/recursos.sh:40-56,81`; `bin/sombra_db.sh:91-98`)
   - Usar `sombra.sh --refazer` apagaria/substituiria a sombra única, contrariando o item; usar `scripts/restore_tenant.sh` restaura no banco do compose com `--clean` e é expressamente proibido pela regra da sombra. (`bin/sombra.sh:255-260`; `scripts/restore_tenant.sh:8`; `CLAUDE.md:206`; `complemento2_queima_util_1711.txt:3,7`, literal)
   - Dado pessoal não pode ser exportado; a sonda e a contagem seguem pelo Codex, sem sonda inline da sessão principal. O wrapper exige saída raspada antes do Codex. (`CLAUDE.md:206,344-350,357-365`)
   - O item exige medir espaço antes, não restaurar se faltar e esperar conflito de disco/CPU com a espinha, sem pedir a trava da sombra; implementação concreta desse preflight e dessa espera: **[nao verificado]**. (`complemento2_queima_util_1711.txt:5`, literal)

3. **Condições literais que não fecham ainda.**
   - “Mesma consulta” produz **leitura atual de retratos antigos**, não a antiga tela histórica: `hoje=timezone.localdate()` participa do balde `Hoje`, e a seleção `em_rollout` será a de cada banco consultado. Assim, variam dados, código de leitura, referência temporal e possivelmente universo de empresas; a ressalva do item 3 deve acompanhar o resultado e impedir atribuição causal à obra. (`smk_b_retrato.py:9,11,16-20`, literal; `app/folha/retrato_folha.py:489-493`; `complemento2_queima_util_1711.txt:4`, literal)
   - Schema físico histórico compatível com o ORM atual e formato real do JSON nos dois dumps: **[nao verificado]**. Chaves `dono/tipo/saldo/datas` evoluíram sem migration, e o leitor admite legado; isso não prova a compatibilidade efetiva de cada arquivo. (`app/folha/models.py:304-323,334-341`; `app/folha/retrato_folha.py:436-449,496-503`)
   - **A consulta-base não distingue sozinha “sem retrato” de zero:** o `for` não imprime empresa sem retrato; contadores globais começam vazios e acabam imprimindo zero. `linhas_do_retrato(None)` também devolve `[]`. Precisa conferir presença por empresa/competência antes de totalizar; ausência não pode virar zero. Tabela ausente/incompatível tampouco demonstra ausência de retrato. (`smk_b_retrato.py:13-16,30-34`, literal; `app/folha/retrato_folha.py:98-111`; `complemento2_queima_util_1711.txt:3`, literal)
   - O timestamp pedido existe e é impresso, mas significa **início da rodada do lavrador**, não instante de `save`; compará-lo com data do dump exige manter essa distinção. (`smk_b_retrato.py:23`, literal; `app/folha/models.py:356-360`)
   - “Apagar ao fim” é expresso no aval, porém falta porta nomeada com guarda de alvo e encerramento garantido também em falha/interrupção; as portas disponíveis apagam a própria sombra no começo do refazer. (`complemento2_queima_util_1711.txt:3`, literal; `bin/sombra.sh:67-80,255-260`)

4. **Exportada, dinheiro ou `!`:**
   - O escopo declarado é restauração lateral, leitura e descarte do banco criado; não manda alterar `FechamentoMensal`, exportação nem regra de cálculo. Se mantido nesse limite, não exige `!` por dinheiro/exportada. Se atingir produção, gravado exportado, escala/vínculo ou arquivo usado por produção, cai nas proibições vigentes. Execução futura respeitando esses limites: **[nao verificado]**. (`complemento2_queima_util_1711.txt:3-5,7`, literal; `CLAUDE.md:265-268`)

**Recomendação:** item 2 **PROCEDE / AVAL-DEVOLVIDO** para fechar porta lateral nomeada, compatibilidade dos dumps, detecção explícita de ausência e descarte seguro; o leitor `dono_da_linha` não é motivo para reprovar como motor de cálculo. (`bin/sombra.sh:67-80,255-260`; `bin/sonda_leitura.sh:63-70,93-97`; `app/folha/retrato_folha.py:468-493`; `CLAUDE.md:35-38`)

## 17/11 08:39 · sessao 89861552b2f3 · - · - · origem=Ronald (tmux)

**Item 5 — PROCEDE: permanecem lacunas operacionais na definição da foto, atividade e idade da reserva.** O literal já substitui “maior intervalo”, estabelece o predicado do alarme, falha por ausência/ilegibilidade, ausência de despacho e quem desliga; essas objeções anteriores foram respondidas. (`complemento2_queima_util_1711.txt:6`, literal)

1. **Premissas.** `relato.sh` gera `ESTADO.md`, raspa nomes/CPF, copia ao repositório público definido por `DEST_RELATO` ou `/home/ronald/hasner-relato`, commita e faz `git push origin HEAD`. O remoto concreto da instalação é **[nao verificado]**. (`bin/relato.sh:2,11,23,37-38,114-117,145-147`)

   O escritor do log registra timestamp local, obra, papel, hash12, PID do **wrapper** (`$$`) e `ini|fim`; o pareamento existente usa `(obra, hash, pid)`. O leitor atual chama de “em curso” simplesmente `ini` sem `fim`; não verifica processo vivo. (`bin/codex_pergunta.sh:266-273,308-309`; `bin/codex_limite_estado.py:63-67,82-88`)

   `ini` precede a espera pelo slot, a trava da resposta, recusas possíveis e a execução do Codex. Portanto, mede tentativa do wrapper iniciada; não prova execução ativa do Codex. O próprio disjuntor explicita isso. (`bin/codex_pergunta.sh:308-309,364-394,425-426`; `bin/disjuntor.py:2071-2075`)

   A fila fornecida apresenta R1 DESPACHADO e R2–R7 ENTREGUE: **zero ABERTOS no literal**. Há HH:MM junto aos estados, mas nenhuma linha ABERTO nem campo/data explicitamente identificado como instante de abertura; a premissa de que todo ABERTO tenha a hora de abertura gravada é **[nao verificado]**. (`fila_reserva.md:3-10`, literal)

2. **Regras.** Medir e publicar não revoga CODEX-NAO-FICA-OCIOSO: o anexo mantém a fila e reserva a Ronald desligar a regra. Também não autoriza ultrapassar os cortes de orçamento existentes. (`fila_reserva.md:1-2`, literal; `complemento2_queima_util_1711.txt:6,10`, literal; `bin/disjuntor.py:1468-1485`)

   O contador deve entrar no produtor existente da linha Codex, preservando suas medidas e o placar exigido, com contrato antes da construção e pouso de instrumento separado. Contrato de cinco linhas efetivamente registrado no RELATO para este contador: **[nao verificado]**. (`bin/gerar_estado.py:267-270,337-339`; `bin/codex_limite_estado.py:235-249`; `CLAUDE.md:327-328,389-394`; `app/docs/PISTA.md:170-176`)

   A raspagem é requisito material porque ESTADO vai ao repositório público; publicar log/fila brutos não está autorizado pelo pedido de três números e contexto por hora. (`bin/relato.sh:2,114-117`; `complemento2_queima_util_1711.txt:6`, literal)

3. **Condições que faltam fechar.**

   - **Atividade real:** definir se conta wrapper vivo, incluindo espera, ou execução efetiva do Codex. `ini` sem `fim` é só evidência de falta de encerramento registrado; `fim` é escrito pelo trap EXIT, e o append tolera erro sem informar ao consumidor. Um órfão de processo morto pode permanecer “em curso” indefinidamente no leitor atual. Existência desse caso na amostra: **[nao verificado]**. (`bin/codex_pergunta.sh:269-282,308-309,364-394`; `bin/codex_limite_estado.py:82-88`)
   - **Contraexemplo que morde:** um ABERTO de 45 minutos com um `ini` órfão de processo morto teria `(a)=1` no leitor atual e esconderia a situação que o aval manda acusar. O caso dado de chamada realmente em curso deve continuar sem acusar. (`bin/codex_limite_estado.py:82-88`; `complemento2_queima_util_1711.txt:6`, literal)
   - **Abertura:** falta definir gravação e leitura de instante completo da entrada em ABERTO, incluindo eventual reabertura. ABERTO legível sem hora não é arquivo ausente/ilegível, portanto a falha declarada não cobre literalmente esse caso; idade não pode virar zero nem “verde” por inferência. (`fila_reserva.md:3-10`, literal; `complemento2_queima_util_1711.txt:6`, literal; `CLAUDE.md:35-38`) [nao verificado]
   - **Instante da foto:** a coleta atual ocorre na geração de ESTADO, antes de gerar sessão, verificar/raspar documentos, gerar CODEX-ESTADO/placar, copiar, commitar e publicar. “Na hora em que publica” ainda precisa fixar o instante da coleta e sua relação com publicação bem-sucedida; não há atomicidade entre essas etapas. (`bin/relato.sh:38-42,55-73,97-117,145-147`)
   - **24 horas:** separar a janela dos inícios por hora da contagem de chamadas ainda vivas: uma chamada iniciada antes da borda pode continuar ativa. Fixar limite inferior/superior e agrupamento horário. O leitor existente só testa `agora - t <= 24h`, aceita a borda exata e também timestamps futuros; não é definição suficiente do novo contexto. (`bin/codex_limite_estado.py:71-78`; `complemento2_queima_util_1711.txt:6`, literal)
   - **SEM DADO:** o leitor atual transforma arquivo de chamadas ausente em listas vazias; reutilizá-lo diretamente permitiria confundir ausência com zero. A nova medida precisa distinguir esses estados, além de tratar registros incompletos necessários à idade/atividade. (`bin/codex_limite_estado.py:49-68`; `complemento2_queima_util_1711.txt:6`, literal)

   A amostra termina em 08:19:53 e os vinte `ini` exibidos têm `fim` exibido; isso não prova atividade zero na hora futura da publicação nem cobre toda a janela de 24 horas, pois foi fornecida como cauda. A fila fornecida também não exercita ABERTO de 45 minutos. (`chamadas_amostra.log:1-40`, literal; `fila_reserva.md:4-10`, literal)

4. **Exportada, dinheiro, `!`.** O escopo declarado só lê fontes e publica contador: não prevê alteração de competência exportada, dinheiro, escala/vínculo ou arquivo usado por PROD. Implementação futura é **[nao verificado]**; continuam vigentes os limites de `!`, raspagem e pouso próprio de instrumento. (`complemento2_queima_util_1711.txt:6`, literal; `CLAUDE.md:265-268,327-328,411-413`; `bin/relato.sh:114-117`)

## 17/11 08:19 · sessao 728030717fda · E4C-F2 · verificador · origem=Code

**Parecer: bloqueia.** Há contraexemplos com pares de resposta legais que rompem a conservação: ressuscitação de pergunta validada, resposta automática posterior à validação e texto composto apenas de brancos. (`app/chamados/models.py:1647`; `app/chamados/services/disputa_emissao.py:1677`; `app/chamados/models.py:4173`)

Execução Django/SQL, incidência em produção, contagens mencionadas em comentários e identidade Git da cópia: **[nao verificado]**. Os contraexemplos abaixo são de código, sem afirmar ocorrência em produção.

1. **[bloqueia] Ressuscitar pergunta validada conserva justamente a validação que a porta apaga — perguntas 1 e 6.**

   Contraexemplo: pergunta com `validada_em=T2`, `materializada_em=T2`, resposta `(None, '')`, via `completude_turno` e célula furada. A ressuscitação grava a foto `VALIDACAO(T2)` e depois zera validação, resposta e carimbo; não grava ato que desfaça a validação. A dobra conserva `T2`, portanto o novo `estado` é **VALIDADA**, embora a projeção persistida esteja sem validação. O próprio teste exige esse resultado. (`app/chamados/models.py:1629`; `app/chamados/models.py:1647`; `app/chamados/models.py:4230`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:377`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:381`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:383`)

   Esse par de resposta é legal: não tem instante nem texto após o trim. Logo, a exceção dos pares ilegais não cobre a mudança. (`app/chamados/juizes.py:1534`; `app/chamados/juizes.py:1541`)

   Outro caminho com o mesmo formato é `devolver_instante_sem_texto_l130`: fotografa a resposta e zera o par sem ato limpador. Nesse caminho, porém, a guarda exclui perguntas validadas; permanece divergência do instante bruto, mas a resposta continua falsa pelo texto vazio. (`app/chamados/services/validacao.py:392`; `app/chamados/services/validacao.py:436`; `app/chamados/services/validacao.py:439`; `app/chamados/services/validacao.py:440`; `app/chamados/juizes.py:1552`)

2. **[bloqueia] RESPOSTA recebe um efeito que nem todos os seus escritores gravam — perguntas 1, 2 e 5.**

   | Escritor de RESPOSTA | O que faz com a projeção | Coerência com a regra nova |
   |---|---|---|
   | Resposta humana | Grava texto/instante e zera validação existente. (`app/chamados/services/disputa_emissao.py:1481`; `app/chamados/services/disputa_emissao.py:1494`; `app/chamados/services/disputa_emissao.py:1525`) | Coerente com RESPOSTA desfazendo validação. (`app/chamados/models.py:4201`) |
   | Resposta automática | Grava somente texto/instante; preserva validação. (`app/chamados/services/disputa_emissao.py:1674`; `app/chamados/services/disputa_emissao.py:1677`; `app/chamados/services/disputa_emissao.py:1678`) | Incoerente quando a resposta fica depois da validação. (`app/chamados/models.py:4232`) |
   | Resposta pela trilha | Grava somente instante; preserva texto e validação. (`app/chamados/services/disputa_emissao.py:1696`; `app/chamados/services/disputa_emissao.py:1698`; `app/chamados/services/disputa_emissao.py:1699`) | Mesma incoerência se o instante recuperado for posterior à validação. (`app/chamados/models.py:4232`) |
   | Foto da projeção, inclusive backfill/nascimento | Copia o par, com motivo especial. (`app/chamados/models.py:4175`; `app/chamados/models.py:4176`) | Excepcionalmente não desfaz validação, pelo conteúdo de `ato_motivo`. (`app/chamados/models.py:4201`) |

   Contraexemplo legal da automática: projeção inicial `(validada_em=T1, respondida_em=None, resposta_colab='')`; executar automática com texto e `T2>T1`. A projeção termina `(T1,T2,texto)`, mas a dobra termina `(None,T2,texto)`. A única guarda da automática é texto já preenchido; não há guarda de validação. (`app/chamados/services/disputa_emissao.py:1663`; `app/chamados/services/disputa_emissao.py:1673`; `app/chamados/services/disputa_emissao.py:1677`; `app/chamados/models.py:4201`)

   O chamador real também não impede: responde automaticamente **antes** de chamar `validar_pergunta`; esta encontra o carimbo preservado e devolve `ja_validada`. (`app/api/foto_recebida.py:48`; `app/api/foto_recebida.py:51`; `app/chamados/services/validacao.py:685`)

   A trilha também não guarda validação: só exige texto presente e instante ausente. Sua entrada pertence ao par ilegal excluído do corte, mas continua refutando a afirmação de que todo escritor de RESPOSTA grava o efeito atribuído pela dobra. (`app/chamados/services/disputa_emissao.py:1692`; `app/chamados/juizes.py:1541`)

   Portanto, há o problema de semântica pedido: **RESPOSTA humana desfaz validação; automática/trilha não; foto é distinguida por uma frase em `ato_motivo`**. (`app/chamados/services/disputa_emissao.py:1494`; `app/chamados/services/disputa_emissao.py:1677`; `app/chamados/services/disputa_emissao.py:1698`; `app/chamados/models.py:4201`)

3. **[bloqueia] Brancos sem instante são legais, mas deixam de ser conservados — pergunta 3.**

   Contraexemplo: sem linhas, `respondida_em=None`, `resposta_colab=' \t '`. Os predicados consideram esse par legal, porque brancos são texto vazio. `_sem_correspondencia` não fabrica RESPOSTA sem instante, e a dobra devolve `resposta_colab=''`. (`app/chamados/juizes.py:1528`; `app/chamados/juizes.py:1534`; `app/chamados/juizes.py:1541`; `app/chamados/models.py:4173`; `app/chamados/models.py:4226`)

   Pelo diff fornecido, `resposta_display` antes devolvia os brancos; agora devolve `''`. As fotos de SILENCIO e RETRATACAO também trocam os brancos por vazio. Isso viola conservação literal fora dos pares ilegais. (`app/chamados/models.py:3753`; `app/chamados/models.py:3755`; `app/chamados/models.py:2032`; `app/chamados/models.py:2164`)

   O M15 monta precisamente `None + brancos` e exige SILENCIO vazio: ele certifica essa mudança, não sua conservação. (`app/chamados/tests/test_cortes_f2_f9_m15.py:50`; `app/chamados/tests/test_cortes_f2_f9_m15.py:57`)

4. **[bloqueia] `q_vivas` deixa de conter toda pergunta viva pelo C6 — pergunta 4.**

   No contraexemplo da ressuscitação validada, o C6 recebe `validada_em=None` da projeção e pode declarar viva a pergunta da célula furada; o novo grau 0 SQL a exclui pela VALIDACAO conservada no ato. Isso contradiz o contrato explícito de continência. (`app/chamados/juizes.py:1389`; `app/chamados/juizes.py:1405`; `app/chamados/juizes.py:1456`; `app/chamados/models.py:3556`; `app/chamados/models.py:3525`)

   O efeito alcança seleções anteriores ao juiz: o app filtra por `q_vivas` antes de chamar C6; o fio faz isso para disputas fechadas. A pergunta excluída não chega ao juiz. (`app/chamados/services/pergunta_no_app.py:52`; `app/chamados/services/pergunta_no_app.py:61`; `app/chamados/services/fio.py:255`; `app/chamados/services/fio.py:269`)

   Outros leitores que discordam no mesmo domínio:

   | Leitor que permanece | Discordância concreta |
   |---|---|
   | Card administrativo, lendo `validada_em` | Ressuscitada aparece sem selo de validação e pode oferecer ações, enquanto `estado` diz VALIDADA. (`app/templates/chamados/partials/_pergunta_card.html:20`; `app/templates/chamados/partials/_pergunta_card.html:23`; `app/templates/chamados/partials/_pergunta_card.html:42`; `app/chamados/models.py:3179`) |
   | Porta de validação, lendo projeção | No caso da automática, responde `ja_validada`, enquanto o estado pelo ato volta a RESPONDIDA. (`app/chamados/services/validacao.py:685`; `app/chamados/models.py:3187`) |
   | Fila de materialização | Continua exigindo validação projetada e não reconhece a VALIDADA apenas pelo ato da ressuscitada. (`app/chamados/services/materializacao.py:2093`; `app/chamados/services/materializacao.py:2110`) |
   | Partição do app e lista `respondidas` da API | Julgam a projeção, enquanto `.respondida` passou a julgar a dobra. (`app/chamados/services/pergunta_no_app.py:82`; `app/api/views.py:2578`; `app/chamados/models.py:3729`) |
   | Payload da API | Mistura texto/instante projetados com `resposta_display` migrado; por exemplo, pode devolver brancos em `resposta_colab` e vazio em `resposta_display`. (`app/api/views.py:2561`; `app/api/views.py:2562`; `app/api/views.py:2563`) |

   **`aguarda_admin` não é leitor independente da projeção:** continua exatamente derivado de `estado == RESPONDIDA`. Sua fonte está preservada, mas herda as mudanças de `estado`. (`app/api/views.py:2550`; `app/api/views.py:2553`)

   `tarefa_do_dia` já lê atos, porém julga a última linha, não a dobra: com V(T1) seguida de RESPOSTA-foto(T2), a dobra preserva validação e a tarefa pede VALIDAR pela última RESPOSTA. Essa divergência existe sem alterar `tarefa_do_dia`. (`app/chamados/models.py:4201`; `app/chamados/services/tarefa_do_dia.py:158`; `app/chamados/services/tarefa_do_dia.py:162`)

5. **[bloqueia] Prefetch velho altera o resultado da leitura — pergunta 5.**

   Contraexemplo concreto de operações:

   - Carregar `p` com decisões prefetched contendo V(T1); há fornecedor desse formato no fio. (`app/chamados/services/fio.py:263`)
   - Responder novamente em T2 pelo core: ele atualiza a instância, zera a validação e registra R(T2). (`app/chamados/services/disputa_emissao.py:1481`; `app/chamados/services/disputa_emissao.py:1494`; `app/chamados/services/disputa_emissao.py:1525`)
   - `registrar` usa `bulk_create` e não invalida o cache relacionado da instância. `leitura(p)` continua lendo o cache anterior. (`app/chamados/models.py:4247`; `app/chamados/models.py:4249`; `app/chamados/models.py:4217`)
   - Como R(T2) falta no cache, o novo par projetado entra como passivo com `desfaz=False`: V(T1) sobrevive. Em instância fresca, a RESPOSTA real desfaz V(T1). (`app/chamados/models.py:4221`; `app/chamados/models.py:4201`; `app/chamados/models.py:4232`)

   Resultado: mesmo banco, `leitura` da instância antiga validada e leitura fresca não validada; a face SQL acompanha a fresca. `prefetch_related_objects` também não resolve por si a instância que já chegou com decisões carregadas. (`app/chamados/models.py:1945`; `app/chamados/models.py:3494`; `app/chamados/models.py:4217`)

   O core aceita a instância e volta a lê-la após registrar, em `resposta_display`; encadeamento HTTP concreto passando uma instância prefetched até esse core: **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:1439`; `app/chamados/services/disputa_emissao.py:1560`)

6. **[fila] Há N+1 novo em caminhos sem prefetch — pergunta 5.**

   O carregamento do app não faz prefetch de decisões. Cada `_item` da API lê `estado` e `resposta_display`; ambos executam `leitura`, que busca `decisoes.all()` sem cache. São duas buscas adicionais de decisões por pergunta nesse percurso, nos loops de histórico e pendentes. (`app/chamados/services/pergunta_no_app.py:52`; `app/api/views.py:2550`; `app/api/views.py:2562`; `app/chamados/models.py:3178`; `app/chamados/models.py:3753`; `app/chamados/models.py:4217`; `app/api/views.py:2567`; `app/api/views.py:2577`)

   Também há busca por pergunta nas fotos de fechamento e retratação: os respectivos lotes não recebem prefetch nesse carregamento. O prefetch de `sem_veredito_entre` não garante cache nas novas instâncias buscadas depois em `self.perguntas.all()`. (`app/chamados/models.py:1945`; `app/chamados/models.py:2000`; `app/chamados/models.py:2032`; `app/chamados/models.py:2160`; `app/chamados/models.py:2164`)

   Contagem executada e impacto de latência: **[nao verificado]**.

7. **[fila] Os testes não passam por universo vazio, mas não fecham a conservação — pergunta 5.** [nao verificado]

   Há asserções que impedem vazio: universo com 13 perguntas e oito validadas; registro com sete leitores; descarimbo efetivo; ressuscitação de exatamente uma pergunta; M18 exigindo exatamente uma chamada observada ao C6. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:165`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:168`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:303`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:326`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:353`; `app/chamados/tests/test_cortes_f2_f9_m18.py:72`)

   As lacunas concretas são:

   - Automática e trilha são exercitadas **antes** da validação; não mordem os escritores que preservam validação. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:188`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:191`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:235`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:239`)
   - Os helpers recarregam a pergunta, retirando do teste o cache velho. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:38`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:176`)
   - O controle da ressuscitação usa instante sem texto, justamente uma exceção do corte. A caracterização seguinte exige a quebra com pergunta validada legal. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:352`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:381`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:383`)
   - A mordida do selo conserva projeção e atos alinhados; ela demonstra retorno aos leitores após descarimbo, mas não distingue leitura antiga da projeção de leitura nova dos atos nesses casos. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:315`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:328`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:331`)
   - M4/M13/M14 mordem mudança para ato com projeção vazia; essa mudança não é, por si, prova de conservação. M15 exige a normalização de brancos identificada no achado 3. (`app/chamados/tests/test_cortes_f2_f9_m4.py:51`; `app/chamados/tests/test_cortes_f2_f9_m13.py:43`; `app/chamados/tests/test_cortes_f2_f9_m14.py:48`; `app/chamados/tests/test_cortes_f2_f9_m15.py:57`)

   Execução dos testes e dos mutantes: **[nao verificado]**. [nao verificado]

8. **[nota] Face SQL e dobra concordam com dados frescos; isso não resolve os bloqueios — pergunta 3.**

   A equivalência estrutural da validação é:

   | Combinação | Resultado das duas faces |
   |---|---|
   | VALIDACAO real sem limpador posterior | Validada. (`app/chamados/models.py:3497`; `app/chamados/models.py:4230`) |
   | Empate entre linhas reais | Desempate por pk. (`app/chamados/models.py:3496`; `app/chamados/models.py:4219`; `app/chamados/models.py:4224`) |
   | Validação passiva empatada com limpador real | Passivo vem depois e permanece. (`app/chamados/models.py:3501`; `app/chamados/models.py:4221`) |
   | Carimbo casado com VALIDACAO posteriormente desfeita | Não recria passivo. (`app/chamados/models.py:3499`; `app/chamados/models.py:4177`) |
   | RESPOSTA-foto sem VALIDACAO-foto | Não limpa validação anterior. (`app/chamados/models.py:3494`; `app/chamados/models.py:4201`) |
   | Texto apenas de brancos | Não participa do predicado de validação; afeta resposta/display separadamente. (`app/chamados/models.py:3494`; `app/chamados/models.py:4232`) |

   `_sem_correspondencia` é efetivamente o mesmo corpo usado pela porta de fotos e pela dobra; a diferença relevante é a origem de `ja`: banco na porta, decisões carregadas na dobra. Com cache velho, essa origem pode divergir, como no achado 5. (`app/chamados/models.py:4160`; `app/chamados/models.py:4162`; `app/chamados/models.py:4218`; `app/chamados/models.py:4222`)

   A verificação em memória das funções extraídas por AST não encontrou divergência em 50.895 combinações finitas; execução do SQL real: **[nao verificado]**. A conclusão geral acima decorre dos dois ramos do predicado e da ordenação da dobra, não dessa amostra isoladamente. (`app/chamados/models.py:3494`; `app/chamados/models.py:3503`; `app/chamados/models.py:4224`; `app/chamados/models.py:4232`)

9. **[nota] Demais classes de reprovação — pergunta 5.**

   - **Segundo juiz:** `respondida` continua chamando o C21 registrado; mudou seu insumo. O bloqueio demonstrado é a divergência com C6 e com os escritores, não a criação de outro predicado de “instante E texto”. (`app/core/juizes.py:291`; `app/core/juizes.py:306`; `app/chamados/models.py:3729`)
   - **Derivação paralela:** a correspondência passiva foi compartilhada. A condição de RESPOSTA limpadora permanece expressa em Python e SQL; hoje as expressões concordam entre si, mas não com todos os escritores. (`app/chamados/models.py:4162`; `app/chamados/models.py:3494`; `app/chamados/models.py:4201`; `app/chamados/services/disputa_emissao.py:1677`)
   - **Relógio fora da borda:** os novos corpos da face SQL e da dobra não leem relógio. A dobra lê inclusive atos futuros; `tarefa_do_dia` corta pelo horizonte recebido, portanto são leituras temporalmente diferentes. (`app/chamados/models.py:3490`; `app/chamados/models.py:4217`; `app/chamados/services/tarefa_do_dia.py:423`)
   - **Ordem de trava:** os novos leitores não adquirem travas; os blocos de escrita examinados continuam adquirindo a ordem centralizada antes de escrever. Não demonstrei inversão nova causada pela F2. (`app/chamados/models.py:1628`; `app/chamados/models.py:2015`; `app/chamados/models.py:2132`; `app/chamados/models.py:2134`; `app/chamados/models.py:2796`)
   - **Segundo escritor da sequência:** a escrita física de produção localizada continua em `registrar`; o modelo também permite inserção inicial por `save`, embora recuse reescrita. Não apareceu produtor novo fora da porta no diff fornecido. (`app/chamados/models.py:4249`; `app/chamados/models.py:4136`; `app/chamados/models.py:4139`)
   - **Ato/campo conservado novo:** os atos continuam os oito declarados; `LeituraDoAto` é estrutura de retorno, não campo persistido. (`app/chamados/models.py:19`; `app/chamados/models.py:4090`; `app/chamados/models.py:4091`)

10. **[nota] Censo dos escritores e dos resets — pergunta 1.**

    Nesta tabela, V=`validada_em`, R=`respondida_em`, T=`resposta_colab`, M=`materializada_em`, via=`via_resolucao`.

    | Escritor | Campos/linha registrada no mesmo ato | A leitura reproduz a projeção? |
    |---|---|---|
    | Nascimento | Recebe campos por `**campos`, salva e grava fotos R/V. (`app/chamados/models.py:3149`; `app/chamados/models.py:3152`; `app/chamados/models.py:3154`) | Sim para as fotos correspondentes. (`app/chamados/models.py:4173`; `app/chamados/models.py:4177`) |
    | Resposta humana | T/R; V=None quando validada; fotos antigas + RESPOSTA. (`app/chamados/services/disputa_emissao.py:1480`; `app/chamados/services/disputa_emissao.py:1481`; `app/chamados/services/disputa_emissao.py:1494`; `app/chamados/services/disputa_emissao.py:1525`) | Na ordenação cronológica correspondente, sim. (`app/chamados/models.py:4224`; `app/chamados/models.py:4232`) |
    | Automática | T/R; fotos + RESPOSTA. (`app/chamados/services/disputa_emissao.py:1673`; `app/chamados/services/disputa_emissao.py:1674`; `app/chamados/services/disputa_emissao.py:1678`) | Não no contraexemplo do achado 2. |
    | Pela trilha | R; RESPOSTA, preservando V. (`app/chamados/services/disputa_emissao.py:1696`; `app/chamados/services/disputa_emissao.py:1699`) | Não necessariamente; achado 2. |
    | Validação | V=agora; fotos + VALIDACAO no mesmo instante. (`app/chamados/services/validacao.py:168`; `app/chamados/services/validacao.py:173`) | VALIDACAO grava V na dobra. (`app/chamados/models.py:4230`) |
    | Devolução por recusa de materialização | **V=None**; fotos + DEVOLUCAO. (`app/chamados/services/validacao.py:273`; `app/chamados/services/validacao.py:279`) | DEVOLUCAO limpa V e preserva R/T. (`app/chamados/models.py:4189`; `app/chamados/models.py:4232`) |
    | Devolução BUG-PAR-103 | Descarimbo e **V=None**; fotos + DEVOLUCAO. (`app/chamados/services/validacao.py:346`; `app/chamados/services/validacao.py:349`; `app/chamados/services/validacao.py:354`) | Mesmo efeito sobre V/R/T. (`app/chamados/models.py:4232`; `app/chamados/models.py:4234`) |
    | Reabertura/recusa | **V=None, R=None, T=''**, mais descarimbo; RECUSA ou REABERTURA com foto. (`app/chamados/services/disputa_emissao.py:1609`; `app/chamados/services/disputa_emissao.py:1631`; `app/chamados/services/disputa_emissao.py:1648`) | Esses atos limpam V/R/T. (`app/chamados/models.py:4189`; `app/ch

[truncado em 20000 de 35429 caracteres]

_6411 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
