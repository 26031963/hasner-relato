# CODEX-ESTADO

> Ultima resposta processada: **13/11/2026 21:06** · publicado em **13/11/2026 21:07** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 13/11 21:06 · sessao b45090b4deb3 · E3-FORMA-SQL · leitura · origem=Code

**O HEAD lido ainda não oferece um contrato único entre todos os leitores.** A tarefa combina donos de ações alcançáveis com donos residuais; o C17 deriva o fio exclusivamente dos verbos. A folha e o vigia consomem a tarefa, mas omitem `nao_sei` em seus retornos públicos. O contador do fio conta chamados do colaborador inteiro, outra unidade. **LEU:** `app/chamados/services/tarefa_do_dia.py:581`; `app/chamados/juizes.py:1688`; `app/folha/fora_da_folha.py:242`; `app/chamados/services/vigia_fio.py:654`; `app/chamados/views.py:901`.

A identificação `2b0739004` é a fornecida no pedido; confirmação por Git **[não verificado]**. Resultados Django, GREEN da v7, ocorrências e contagens atuais em produção **[não verificado]**. Nas tabelas, “prova” significa **teste aberto e asserção lida**, não execução aprovada.

Para manter as referências legíveis, estas abreviações identificam arquivos exatos:

| Referência | Arquivo |
|---|---|
| `T` | `app/chamados/services/tarefa_do_dia.py` |
| `J` | `app/chamados/juizes.py` |
| `M` | `app/chamados/models.py` |
| `Motor` | `app/chamados/catalogo/motor.py` |
| `Painel` | `app/chamados/services/painel.py` |
| `Verbos` | `app/chamados/services/verbos.py` |
| `Fio` | `app/chamados/services/fio.py` |
| `Folha` | `app/folha/fora_da_folha.py` |
| `Vigia` | `app/chamados/services/vigia_fio.py` |
| `Canal` | `app/chamados/services/furo_sem_canal.py` |

Os testes citados abaixo ficam em `app/chamados/tests/`, salvo indicação diferente. [nao verificado]

**1. Censo: estado persistido, estado derivado e escritores**

“Vivo”, “na fila”, “pergunta viva”, “estado da pergunta” e “ação exigível” são respostas diferentes no código. Nenhum desses termos pode substituir automaticamente os demais. **LEU:** `Motor:24`; `Painel:188`; `J:1389`; `M:3060`; `T:126`.

| Estado/campo que entra no julgamento | Representação persistida | Quem escreve | Quem lê neste percurso |
|---|---|---|---|
| Chamado aberto/em análise/registrado/resolvido/fechado/cancelado/superado | `ChamadoColaborador.status_local`, `M:28`, `M:52` | Nascimento `M:1071`, `M:1096`; transição `_gravar_estado`, `M:228` | `qs_vivos`, `Painel:155`; `qs_na_fila`, `Painel:188` |
| Encerramento do chamado | `via_encerramento`, `resolvido_em`, `M:54`, `M:60` | `_gravar_estado` carimba terminal em `M:233`; limpa ao voltar a vivo em `M:248` | Classificação de vida usa `status_local`, não esses carimbos: `Painel:155` |
| Silenciado/arquivado; silêncio vencido/histórico | `silenciado_ate`, autor/motivo, `M:90` | `arquivo.silenciar`, `app/chamados/services/arquivo.py:166`; `levantar`, mesmo arquivo `:202` | Pertinência: `Painel:190`; apresentação: `Verbos:48` |
| Chamado ligado à célula/dia | `ChamadoColaborador.celula`, `contexto_json`, `M:47`, `M:50` | Nascimento recebe célula/contexto: `M:1124`, `M:1153`; demais escritores desses vínculos **[não verificado como censo exaustivo]** | Porta usa célula: `J:1561`, `T:569`; juiz do dia também aceita contexto: `J:98`, `J:114` |
| Natureza/verbo do chamado | `modulo_origem`, `M:46`; verbo não é coluna | Nascimento recebe os campos em `M:1096`; censo completo de alterações posteriores **[não verificado]** | `Verbos:47`; regras em `app/chamados/catalogo/natureza.py:41` |
| Disputa aberta/fechada | `DisputaSupervisao.fechada_em`, autor, resolução, motivo: `M:1758`, `M:1794` | `fechar`, `M:1939`; reabertura administrativa `M:2156`; sistema `M:2180` | App distingue pendência/histórico por `fechada_em`: `app/chamados/services/pergunta_no_app.py:71` |
| Identidade da pergunta | `colaborador`, `data_evento`, `motivo`, `instancia_marco`, `conversa`, `batida_origem`: `M:2442`, `M:2454`, `M:2461` | `_nascer`, inserção em `M:3040`; dia pela porta `gravar_dia_do_fato`, `Fio:829`; reuso/rechaveamento passam por esse percurso | Carga exige `conversa=False` e dia: `T:384`; posição: `T:117` |
| Sob qual chamado a pergunta aparece | `PerguntaDisputa.chamado_destino`; alternativa `DisputaSupervisao.chamado`: `M:3437`, `M:1750` | Nascimento `M:3040`; reuso `M:3008`; `reapontar_resolvedora`, `Fio:964` | `chamado_que_lista`: destino, senão pai, `Fio:117`; tarefa `T:136`; C17 `J:1680` |
| Resposta projetada | `resposta_colab`, `respondida_em`: `M:2468` | Resposta humana `app/chamados/services/disputa_emissao.py:1508`; automática `:1659`; correção pela trilha `:1681`; nascimento `M:3040`; complemento por `carimbar`, `M:3156` | Passivo `M:4017`; painel/crédito `app/chamados/services/raiox.py:143`; estado `M:3071` |
| Validação projetada | `validada_em`, `validada_por`: `M:2470`, `M:3430` | `_carimbar_validacao`, `app/chamados/services/validacao.py:129`; nascimento `M:3040`; complemento por `carimbar`, `M:3160` | C6 `J:1391`; passivo `M:4021`; estado `M:3067` |
| Validação devolvida | Validação retirada; resposta preservada; decisão `devolucao` | `devolver_validacao_recusada`, `app/chamados/services/validacao.py:190` | Última decisão em `T:149`; não existe coluna `estado='devolvida'`, `M:2637` |
| Resposta recusada/reaberta | Projeção de resposta/validação retirada; decisão com fotografia | `reabrir_para_correcao`, `app/chamados/services/disputa_emissao.py:1595`, `:1617`; recusa chama essa porta, `app/chamados/services/validacao.py:592` | `T:147` |
| Ressuscitação | Retirada de carimbos/resposta/validação pelo percurso específico | `ressuscitar_perguntas`, `M:1513`, limpeza em `M:1567` | Tarefa volta a ler fatos/projeção em `T:399`, `T:408` |
| Materialização/via declarada | `materializada_em`, autor, `via_resolucao`: `M:3445`, `M:2582` | Atribuição central `_escrever_carimbo`, `M:3123`; portas `carimbar`, `M:3129`; `carimbar_identidade`, `M:3194`; retirada `M:3229`, lote `M:3306` | C6 `J:1402`; identidade da tarefa `T:240`; isso sozinho não prova efeito, `T:151` |
| Fora do fato | Via `duplicata` ou `motivo_retratado`: `M:2414` | `carimbar_identidade`, `M:3194`; remoção de motivo registra retratação e carimba, `M:2063`, `M:2068` | Tarefa exclui em `T:240`; C6 em `J:1402` |
| Pendência administrativa/recusa | `pendencia_admin`, `recusa_motivo`: `M:2577`, `M:2581` | Materializador `app/chamados/services/materializacao.py:856`, `:1108`; recusa `app/chamados/services/validacao.py:592`; identidade `M:3214` | Não são entradas diretas de `_pela_decisao`, `T:126`; repercussões via autoridades **[não verificado exaustivamente]** |
| Sequência de decisões | `DecisaoPergunta.ato/ato_em/texto/autor/motivo/foto/absorção`: `M:3943`, `M:3957` | Porta `DecisaoPergunta.registrar`, `M:4029`, inserção `M:4039` | Tarefa ordena por `ato_em,pk` e corta no instante: `T:400` |
| Lavra do efeito | `veredito_lavrado`: vazio/sim/não/indeterminado; `julgado_em`, `janela_expira_em`: `M:2600`, `M:2611` | `app/chamados/veredito_lavrado.py:254`; persistência de lote `:247`; também backfill com atribuições em `app/chamados/management/commands/backfill_veredito_lavrado.py:138` | Tarefa aceita somente sim/não vigentes, `T:413`; crédito do painel lê a lavra sem conferir vigência nesse filtro, `app/chamados/services/raiox.py:135` |
| Lâmpada/célula/dia de trabalho | `CelulaDia.trabalha`, `dna`, `ata`, `veredito`: `app/escala/models.py:1686`, `:1697`, `:1770`, `:1778` | Operações da porta da célula: gerar, editar, lavrar, carimbar; `app/ponto/portas/celula.py:74`; lavra `:974` | Tarefa lê pelo leitor da célula, `T:509`; instâncias `T:102`; C6 também lê fatos da célula, `J:1438` |
| Ausência | `Ausencia.status`: aguardando documento/decisão, aprovada/rejeitada; `app/ponto/models.py:701`, `:799` | Estado inicial `app/ponto/services/ausencia.py:780`; `mudar_estado`, `:809`; existe escrita de rejeição em lote `:1284` | Cobertura antes da pergunta, `T:291`; absorção `T:163` |
| Competência trancada/reaberta e toque humano posterior | `PeriodoFechado.reaberto_em`, `app/ponto/models.py:931`; atos em auditoria | Trancar/reabrir: `app/ponto/services/fechamento.py:1400`, `:1432`; escritores de todos os atos de toque **[não verificado exaustivamente]** | `periodo_trancado` usa `reaberto_em IS NULL`, mesmo arquivo `:1470`; tarefa recebe `dias_trancados`, `T:432`; toque `J:1233` |
| Tarefa, ações, rota, fila, verbos, fio, `nao_sei` | **Não há tabela de tarefa neste juiz** | Projeção em memória: `T:192`, `T:589`; consulta não grava decisões nem encerra chamado, `T:563` | Projeção pública `T:592`; leitores identificados na matriz abaixo |

A declaração “escritor único” não basta para afirmar que existe uma única atribuição na árvore: há, por exemplo, complemento de validação em `carimbar`, backfill da lavra e rejeição de ausência em lote. **LEU:** `M:3160`; `app/chamados/management/commands/backfill_veredito_lavrado.py:138`; `app/ponto/services/ausencia.py:1284`. Um inventário absolutamente exaustivo de todos os escritores e leitores de todos esses campos em todo `app/` permanece **[não verificado]**.

As transições de chamado que podem alterar a pertinência à fila são estas; o juiz é `decidir`, e a gravação continua no modelo. **LEU:** `Motor:125`; `M:228`.

| Origem | Transições humanas ordinárias | Eventos de sistema |
|---|---|---|
| Inexistente | — | EMITIR→aberto; REGISTRAR→registrado. `Motor:130` |
| Aberto | em análise/resolvido/fechado | analisar/reconciliar/encerrar/cancelar/superar. `Motor:100`, `Motor:110` |
| Em análise | aberto/resolvido/fechado | reconciliar/encerrar/cancelar/superar. `Motor:101`, `Motor:110` |
| Registrado | em análise/fechado | analisar/reconciliar/encerrar/cancelar/superar. `Motor:105`, `Motor:110` |
| Resolvido | fechado/em análise | reabrir/encerrar. `Motor:102`, `Motor:113` |
| Fechado | Exige perfil `admin_ti` | reabrir. `Motor:150`, `Motor:113` |
| Cancelado/superado | Terminais absolutos | Sem saída nos mapas lidos. `Motor:142`, `Motor:110` |

Não há fase persistida denominada “aguardando” em `ChamadoColaborador.status_local`; as esperas resultam de perguntas, ações e verbos. **LEU:** `M:28`; `T:138`; `app/chamados/catalogo/natureza.py:41`.

O rótulo `PerguntaDisputa.estado` admite pendente/respondida/validada/resolvida, mas pode devolver pendente ou respondida mesmo depois de C6 negar vitalidade. Portanto ele não representa sozinho “quem age”. **LEU:** `M:2637`; `M:3067`; `M:3074`.

**2. Matrizes de transição e publicação**

A matriz precisa de duas etapas: primeiro nasce o resultado da tarefa; depois a porta reúne chamados e ações. Há múltiplas instâncias, perguntas e chamados por dia; por isso a representação abaixo usa **classes e conjuntos**, preservando a composição, em vez de supor um único chamado/pergunta. **LEU:** `T:238`; `T:264`; `T:566`. Alcançabilidade real de cada cruzamento sem fixture específica **[não verificado]**.

Em **todas** as linhas, a persistência feita pela consulta da porta é **zero**. Os campos persistidos são os fatos anteriores identificados no censo. **LEU:** `T:176`; `T:563`.

**2.1. Precedências que decidem a tarefa antes da última decisão**

A ordem das linhas é significativa. **LEU:** `T:195`; `T:270`.

| Estado do dia/instância | Resultado da tarefa antes da publicação | Juiz/ramo | Prova localizada |
|---|---|---|---|
| Fora do vínculo | Razão `fora_vinculo`; sem ação | `T:195` | `test_e2_tarefa_do_dia.py::test_demitido_na_vespera_sem_fato_nao_deve_nada`, `:559` |
| Isento | `isento_ponto`; ações vazias | `T:198` | Mesmo arquivo `::test_isento_de_ponto_nao_deve_marco`, `:573` |
| Trancado sem toque posterior | `competencia_trancada`; ações vazias | `T:205` | Mesmo arquivo `::test_dia_trancado_nao_tem_tarefa`, `:1088` |
| Sem célula | `sem_celula`; `nao_sei=True` | `T:214` | **SEM PROVA de cruzamento com todos os leitores** |
| Trabalho sem lâmpadas, todos os marcos ainda antes do teto | `nao_venceu`; sem ação | `T:218` | `test_e2_tarefa_do_dia.py::test_T_ata_sem_lampadas_antes_de_todo_teto_nada_e_exigivel`, `:840` |
| Trabalho sem lâmpadas, sem a dispensa anterior | `ata_sem_lampadas`; `nao_sei=True` | `T:224` | `test_e3_portas_tarefa.py::test_RED_ata_sem_lampadas_conserva_porta`, `:185` |
| Dia sem trabalho | Dispensa da cobrança; pode expor ação **administrativa** de pergunta viva, respeitando exportação | `T:251`, `T:270` | Testes de exposições administrativas; **SEM PROVA universal do produto com estados/portas** | [nao verificado]
| Trabalho, lâmpada acesa | `aceso`; sem consultar a decisão da pergunta daquele marco | `T:270`, `T:274` | `test_e2_tarefa_do_dia.py::test_L_lampada_acesa_responde_antes_da_pergunta`, `:867` |
| Lâmpada desconhecida | `lampada_nao_sabe`; `nao_sei=True` | `T:280` | **SEM PROVA de paridade completa** |
| Sem correspondência de teto do marco | `marco_fora_do_turno`; `nao_sei=True` | `T:284` | **SEM PROVA de paridade completa** |
| Marco ainda não vencido | `nao_venceu`; sem ação | `T:288` | `test_e2_tarefa_do_dia.py::test_o_teto_e_por_marco`, `:170` |
| Cobertura aprovada | `ausencia_aprovada`; sem ação | `T:292` | Mesmo arquivo `::test_ausencia_sem_pergunta_pela_cobertura`, `:256` |
| Cobertura aguardando documento | Entregar documento/colaborador/sem porta inteira | `T:294` | Regra de estado inicial: `app/ponto/services/ausencia.py:780`; **SEM PROVA universal dos leitores** |
| Outra cobertura devolvida pelo carregador | Decidir ausência/admin/sem porta inteira | `T:297` | `test_e2_tarefa_do_dia.py::test_ausencia_sem_pergunta_pela_cobertura`, `:256` |
| Segunda ocorrência de posição com pergunta sem identidade suficiente | `instancia_sem_identidade`; `nao_sei=True` | `T:302` | **SEM PROVA de paridade completa** |
| Sem pergunta correspondente, inclusive repetição sem pergunta | Perguntar/sistema/`emissor` | `T:308`, `T:310` | `test_e2_tarefa_do_dia.py::test_cada_instancia_tem_a_sua_tarefa`, `:151` |
| Motivo fora da grade ou posição ausente | Pode expor ação admin de pergunta C6 viva; não inventa ação colab | `T:324`, `T:261` | **SEM PROVA de todos os cruzamentos** |
| Dia sem trabalho, `fato_em_ausencia`, sem pergunta viva, não exportado | Validar/admin/sem porta inteira | `T:337` | **SEM PROVA universal dos leitores** |

Essas dispensas não impedem a etapa posterior de conservar um chamado vivo na fila. **LEU:** `T:571`; provas sintéticas em `test_e3_portas_v6_sem_banco.py::test_razao_de_dia_nao_esconde_residual_nem_muda_as_acoes`, `:79`.

**2.2. Transições da pergunta quando os guardas anteriores deixam julgá-la**

Aqui `âncora` significa `chamado_que_lista(p)` **somente se estiver na fila**; caso contrário a porta da ação humana é `None`. **LEU:** `T:136`.

| ID | Estado/fato anterior da pergunta | Tarefa devolvida | Fonte do julgamento | Prova localizada |
|---|---|---|---|---|
| Q0 | Conversa, outra data, fora do fato ou última retratação aplicável | Não participa como pergunta do marco | `T:384`, `T:240`, `T:246` | `test_e2_tarefa_do_dia.py::test_a_conversa_nao_e_a_pergunta_do_marco`, `:352`; demais cruzamentos **SEM PROVA** |
| Q1 | Projeção respondida/validada sem decisão correspondente | `passivo_sem_linha`; ações vazias; desconhecimento | `T:138` | `test_e3_portas_tarefa.py::test_passivo_sem_linha_porta_visivel_rotulo_nao_sei`, `:156` |
| Q2 | Sem decisão | `aguarda_resposta`; responder/colaborador/âncora | `T:140` | `test_e2_tarefa_do_dia.py::test_a_resposta_de_uma_nao_fecha_a_outra`, `:159` |
| Q3 | Última resposta vazia | `respondida_sem_texto`; responder/colaborador/âncora | `T:143` | Produto com todas as portas/leitores: **SEM PROVA** |
| Q4 | Última resposta com texto | `aguarda_validacao`; validar/admin/âncora | `T:146` | `test_e2_tarefa_do_dia.py::test_a_resposta_de_uma_nao_fecha_a_outra`, `:159` |
| Q5 | Última recusa | `devolvida_ao_colaborador`; responder/colaborador/âncora | `T:147` | Produto completo: **SEM PROVA** |
| Q6 | Última reabertura | Mesmo resultado de Q5 | `T:147` | Produto completo: **SEM PROVA** |
| Q7 | Última devolução da validação | `materializacao_recusada`; validar/admin/âncora | `T:149` | `test_e2_tarefa_do_dia.py::test_devolucao_volta_ao_admin`, `:222` |
| Q8 | Validação; lavra ausente, stale ou indeterminada | `veredito_sem_lavra_vigente`; ações vazias; desconhecimento | `T:151` | Mesmo arquivo `::test_sem_lavra_vigente_e_nao_sei`, `:341` |
| Q9 | Validação; lavra vigente “não” | `validada_sem_efeito`; materializar/sistema/`materializacao` | `T:155` | Mesmo arquivo `::test_o_efeito_e_o_veredito_do_juiz_e_nao_suposto`, `:293` |
| Q10 | Validação; lavra “sim”; batida de efeito presente, lâmpada ainda apagada | `efeito_sem_lampada`; rejulgar/sistema/`cartorio` | `T:157` | Mesmo teste, `:308` | [nao verificado]
| Q11 | Validação; lavra “sim”; sem batida de efeito nesse conjunto | `encerrada_pelo_juiz`; nenhuma ação | `T:159` | Mesmo arquivo `::test_o_juiz_do_fechamento_responde_e_nao_a_batida`, `:335` |
| Q12 | Último silêncio | `silencio_decidido`; nenhuma ação | `T:160` | Mesmo arquivo `::test_ato_duplicado_e_a_mesma_tarefa`, `:285` |
| Q13 | Absorção, ausência aprovada | `absorvida_aprovada`; nenhuma ação | `T:164` | Ramo de projeção: `test_e3_portas_v6_sem_banco.py::test_encerrada_externa_tem_porta_do_chamado_e_passivo_preserva_pergunta`, `:92`; integração completa **SEM PROVA** |
| Q14 | Absorção, ausência rejeitada | `absorcao_rejeitada`; responder/colaborador/âncora | `T:166` | `test_e2_tarefa_do_dia.py::test_absorcao_segue_a_decisao_da_ausencia`, `:238` |
| Q15 | Absorção, aguardando documento | Entregar documento/colaborador/`None` | `T:168` | Produto completo: **SEM PROVA** |
| Q16 | Outra absorção | `absorcao_pendente`; decidir ausência/admin/âncora | `T:170` | Ramo lido; alcance fora das precedências de cobertura **[não verificado]** |
| Q17 | Ato desconhecido | `ato_desconhecido:<ato>`; desconhecimento | `T:171` | **SEM PROVA**; produção alcançável **[não verificado]** |

Q13/Q16 não devem ser confundidos com a cobertura: esta pode decidir antes de `_pela_decisao`, impedindo que o ramo de absorção seja o resultado observado. **LEU:** `T:291`; `T:316`.

**2.3. Regra completa de composição da porta**

Esta é a transformação efetivamente lida:

| Conjunto/resultado | Definição no HEAD |
|---|---|
| `F` | `alimentacao['fila_ids']`: membros vistos pela alimentação da fila. `J:1564` |
| `A` | Portas **inteiras** das ações, intersectadas com `F`. `T:567` |
| `C` | Chamados ligados à célula, intersectados com `F`. `T:569` |
| `B` | Âncoras das perguntas carregadas para aquele dia, intersectadas com `F`. `T:570`, `T:571` |
| Portas publicadas `P` | `A ∪ C ∪ B`. `T:571` | [nao verificado]
| `verbos` | Verbo do painel de cada elemento de `P`, ordenado por ID. `T:576` |
| `fila` | Somente elementos de `C` cujo verbo não é arquivado/histórico. `T:577` |
| Donos de ação | Se existe porta publicada, somente ações com porta em `A`; sem porta, conserva donos internos. `T:581` | [nao verificado]
| Donos residuais | `fio_pelos_verbos(P − A)`. `T:582` |
| Prioridade | `colab > admin > chamado > sistema`; depois desconhecimento/nada. `T:586` |
| Rota | Sem `P`: `None`; com chamado da célula ou porta residual não classificada como pergunta: `porta_fila`; demais: `porta_pergunta`. `T:573`, `T:588` |
| Projeção pública | Cinco campos se existe rota; senão `None`. `T:598` |

Essa tabela permite expandir qualquer combinação das duas matrizes anteriores sem inventar uma nova opinião de leitor. A prova de que **todos** esses produtos são alcançáveis ou cobertos por testes é **[não verificado]**. [nao verificado]

Para ler a matriz seguinte:

- `PF`/`PP` = `porta_fila`/`porta_pergunta`; `∅` = projeção pública `None`. **LEU:** `T:588`, `T:598`.
- A folha devolve o fio recebido, ou `nada` sem porta; grupos por dia isolado: admin→A, colab→B, chamado→C, nada→D. **LEU:** `Folha:89`, `Folha:92`.
- O vigia, quando o dia pertence ao seu universo, devolve rota/fio recebidos e conta admin somente se `fio=='admin'`. **LEU:** `Vigia:648`.
- **F5B é o módulo de teste:** `_juiz` chama C17; `_fio_da_folha` chama a folha. Não foi localizado um terceiro juiz de produção chamado F5B. **LEU:** `test_o261e_f5b_juiz_porta.py:92`, `:96`; outro significado de F5B **[não verificado]**. [nao verificado]
- `+1` no contador significa a contribuição daquele chamado; o total do co

[truncado em 20000 de 41493 caracteres]

## 13/11 21:04 · sessao c420634dcb48 · - · - · origem=Ronald (tmux)

| id | balde | existe (SIM/NAO/PARCIAL/?) | prova (arquivo:linha do HEAD ou commit; LEU x INFERIU) | mae | veredito (CONTA/DOBRA/OBSOLETA) | [nao verificado]
|---|---|---|---|---|---|
| DISPARAR-PERGUNTAS-NO-CONTRATO-20 | 20/20 | NAO | LEU app/config/crons.py:1524, só --alarme; app/ponto/services/cartorio.py:1045, emissão pelo evento; INFERIU absorção pela E4 em app/docs/BACKLOG.md:1915. | FIO-E0-V4-E4-ESCRITORES | OBSOLETA |
| O168 | 20/20 | PARCIAL | LEU guarda em app/chamados/models.py:718; pais legados escapam em app/chamados/models.py:716; --executar permanece em app/config/crons.py:599. | - | CONTA |
| O168-FILA-OPUS | 20/20 | SIM | LEU app/chamados/models.py:716, categoria pula guarda; app/chamados/services/validacao.py:416 exige carimbo enquanto app/chamados/models.py:1900 aceita texto sem carimbo; mãe em app/docs/BACKLOG.md:1423. | O168 | DOBRA |
| O169 | 20/20 | PARCIAL | LEU app/ponto/services/cluster_espurio.py:75, ausência de medida preserva chamado; residual: contador só inclui assinatura comprovada em app/ponto/services/cluster_espurio.py:126 e alarme em app/ponto/management/commands/detectar_cluster_espurio.py:100 não acusa os não medidos; INFERIU obra parcialmente curada. | - | CONTA |
| LASTRO-FOLGA-FORA-DO-CATALOGO | 20/20 | SIM | LEU app/ponto/precedencia.py:79, tipo desconhecido ainda vira atestado; INFERIU mãe pela classe declarada em app/docs/BACKLOG.md:1815 e caso em app/docs/BACKLOG.md:1835; acervo atual [não verificado]. | EFEITO-NONE-UM-LEITOR | DOBRA |
| FIO-DUP | 20/20 | NAO | LEU app/ponto/services/cartorio.py:1651 e app/ponto/services/cartorio.py:1666, ambos passam por reconciliar; guarda em app/chamados/models.py:718; mesmo defeito explicitado em app/docs/RELATO.md:218. | O168 | OBSOLETA |
| LASTRO-FAIXA-SEM-DEFAULT | 20/20 | SIM | LEU app/ponto/precedencia.py:79, fallback silencioso permanece; INFERIU mãe pela classe em app/docs/BACKLOG.md:1815 e escopo em app/docs/BACKLOG.md:1840. | EFEITO-NONE-UM-LEITOR | DOBRA |
| LEI-FIO-MUDO-B | 20/20 | ? | LEU pedido em app/docs/BACKLOG.md:1847; reabrir_continentes_das_reatadas não localizada no arquivo nomeado; sem sítio/commit de retirada comprovado [não verificado]; absorção pela E3 declarada na mesma célula. | FIO-E0-V4-E3-LEITORES | DOBRA |
| FIO-E0-V4-E3-LEITORES | 20/20 | SIM | LEU app/chamados/services/fio.py:340, leitor ainda usa respondida_em cru; app/chamados/services/vigia_fio.py:640 ainda lê porta_alcancavel_do_dia; contraponto tarefa_do_dia em app/chamados/services/tarefa_do_dia.py:562. | - | CONTA |
| E3-MATRIZ-DA-RESPOSTA | 20/20 | ? | LEU escritores em app/chamados/services/disputa_emissao.py:1655 e app/chamados/models.py:3073; matriz/selo pedidos em app/docs/BACKLOG.md:1903 não localizados por nome; conclusão dos instrumentos [não verificado]; complemento explícito da E3 nessa célula. | FIO-E0-V4-E3-LEITORES | DOBRA |
| E3-X1-FOLGA-NAO-E-SEM-COBERTURA | 20/20 | NAO | LEU app/ponto/services/cartorio.py:1255 e app/ponto/services/cartorio.py:1289: dia sem trabalho conserva veredito derivado, não recebe nunca_bateu; INFERIU cura local; vínculo à E3 em app/docs/BACKLOG.md:1906. | FIO-E0-V4-E3-LEITORES | OBSOLETA |
| E3-V15-A-S | 20/20 | PARCIAL | LEU ação ADMIN em app/chamados/services/tarefa_do_dia.py:327; leitores antigos persistem em app/chamados/services/fio.py:340; INFERIU entrega parcial; inclusão na E3 em app/docs/BACKLOG.md:1907. | FIO-E0-V4-E3-LEITORES | DOBRA |
| E3-CADASTRO-ADMIN | 20/20 | ? | LEU app/docs/BACKLOG.md:1908, lista de casos cadastrais sem cura por código; situação atual e entrega da lista não constam do dado raspado [não verificado]. | - | CONTA |
| E3-X2-NUNCA-JULGADAS | 20/20 | ? | LEU app/docs/BACKLOG.md:1909, casos da fase 1; código aceita ausência de julgamento em app/ponto/services/cartorio.py:1260, mas as 26 células atuais não foram fornecidas [não verificado]. | FIO-E0-V4-E3-LEITORES | DOBRA |
| E3-COL962-C6-X-LAMPADA | 20/20 | ? | LEU app/docs/BACKLOG.md:1910; divergência individual depende da célula/pergunta atual, ausente do dado [não verificado]; INFERIU complemento da medição E3 pela mesma célula. | FIO-E0-V4-E3-LEITORES | DOBRA |
| FIO-E0-V4-E4B | 20/20 | PARCIAL | LEU backfill e portas em app/chamados/management/commands/backfill_fio_morto_e4b.py:82; DRY/aplicação descritos em app/chamados/management/commands/backfill_fio_morto_e4b.py:33; passivo atual/rodadas finais [não verificado]; fase da obra-mãe em app/docs/BACKLOG.md:1915. | FIO-E0-V4-E4-ESCRITORES | DOBRA |
| E4B-C12-DIFF-DE-PAUTA | 20/20 | ? | LEU exigência e complemento em app/docs/BACKLOG.md:1925; resultado registrado em app/docs/BACKLOG.md:1924, porém DIFF bruto não fornecido [não verificado]; raiz E4 em app/docs/BACKLOG.md:1915. | FIO-E0-V4-E4-ESCRITORES | DOBRA |
| E3-10-CURA-NO-ESCRITOR | 20/20 | SIM | LEU app/chamados/services/disputa_emissao.py:1652: grava texto recebido e carimbo sem recusar texto vazio; app/chamados/models.py:3075 incorpora projeção do nascimento; INFERIU brecha na porta; escopo E3 em app/docs/BACKLOG.md:1944. | FIO-E0-V4-E3-LEITORES | DOBRA |
| O168-GUARDA-NO-GATE-DO-PAI | 20/20 | PARCIAL | LEU guarda implementada em app/chamados/models.py:718; app/chamados/models.py:716 deixa pais legados de outra categoria fora; residual reconhecido em app/docs/BACKLOG.md:1423. | O168 | DOBRA |
| E3-FORMA-SQL-DO-JUIZ | 20/20 | SIM | LEU app/chamados/models.py:3404, SQL usa texto diferente de vazio sem trim; juiz usa strip em app/chamados/juizes.py:1521; complemento explícito em app/docs/BACKLOG.md:1946. | FIO-E0-V4-E3-LEITORES | DOBRA |
| E3-IMPACTO-E-CONTRATOS-DE-ENTRADA | 20/20 | ? | LEU exigências em app/docs/BACKLOG.md:1947 e limitações da análise em app/docs/RELATO.md:53; DIFF e entradas atuais não fornecidos [não verificado]; complemento E3 explícito nessa célula. | FIO-E0-V4-E3-LEITORES | DOBRA |
| BO-0611-07-E4B-COBRE | 20/20 | ? | LEU app/docs/BACKLOG.md:1982, complemento explícito da E4B; backfill existe em app/chamados/management/commands/backfill_fio_morto_e4b.py:82, mas caso 07 e DRY bruto ausentes [não verificado]; raiz em app/docs/BACKLOG.md:1915. | FIO-E0-V4-E4-ESCRITORES | DOBRA |
| E4A1-C6-X-PASSADO-SEM-TURNO | 20/20 | SIM | LEU app/chamados/services/fabrica_da_celula.py:203, retorno sem turno precede descarimbo em app/chamados/services/fabrica_da_celula.py:209; INFERIU caminho ainda existente; CONTRACT/mãe em app/docs/BACKLOG.md:1991 e app/docs/BACKLOG.md:1915; ocorrência atual [não verificado]. | FIO-E0-V4-E4-ESCRITORES | DOBRA |
| E3-ENTRADA-ORFA-SEM-TURNO | 20/20 | SIM | LEU app/chamados/services/disputa_emissao.py:283 e app/chamados/services/disputa_emissao.py:294, entrada real ainda pode produzir pergunta; lei pendente delimitada em app/docs/BACKLOG.md:2019; INFERIU caminho existente, ocorrência atual [não verificado]. | - | CONTA |
| C20-O168 | 20/20 | PARCIAL | LEU juiz no núcleo em app/ponto/supra_juiz.py:578 e guarda em app/chamados/models.py:718; pais legados escapam em app/chamados/models.py:716; INFERIU complemento de O168 pelo mesmo juiz, app/docs/BACKLOG.md:2052. | O168 | DOBRA |
| FERIADO-ESCALA | LATERAL-ESTRUTURA | PARCIAL | LEU regra por posto/tipo em app/escala/models.py:905 e uso na porta em app/colaboradores/services/vinculo.py:296; lote/passivo atual não fornecido [não verificado]; escopo redefinido em app/docs/BACKLOG.md:728. | - | CONTA |
| FERIADO-1609-FILA | LATERAL-ESTRUTURA | SIM | LEU app/ponto/services/fechamento.py:1611, invalidação sem filtro de exportada; app/colaboradores/services/vinculo.py:974 copia flag ao novo tipo; fila explícita do feriado em app/docs/BACKLOG.md:729. | FERIADO-ESCALA | DOBRA |
| LEI-DESLIGADO-NAO-TRAVA | LATERAL-ESTRUTURA | NAO | LEU app/folha/export.py:1207, desligado retorna sem retenção; app/folha/export.py:1216 oferece informativo pelo mesmo laco; app/folha/fora_da_folha.py:98 delega ao juiz; INFERIU cura do defeito no código. | - | OBSOLETA |
| DESLIGAR-LIQUIDA-TUDO | LATERAL-ESTRUTURA | SIM | LEU app/colaboradores/services/desligamento.py:165, respostas sem veredito fazem cascata pular disputa; complemento (d) em app/docs/BACKLOG.md:731, raiz O50 em app/docs/BACKLOG.md:1332. | O50 | DOBRA |
| PARIDADE-ABSOLVIDAS-168 | LATERAL-ESTRUTURA | ? | LEU via de identidade em app/chamados/models.py:2566 e estado registrado em app/docs/BACKLOG.md:733; as 168/103 atuais e efeito da devolução não fornecidos [não verificado]; absorção BUG-PAR em app/docs/BACKLOG.md:733 e raiz F4 em app/docs/BACKLOG.md:1935. | RAIZ-DA-DISPUTA | DOBRA |
| RAIOX-DIAS-DERIVA | LATERAL-ESTRUTURA | SIM | LEU app/folha/views.py:616, Raio-X rederiva furos em vez de ler os dias de app/folha/fora_da_folha.py:98; INFERIU complemento da unificação de projeções em app/docs/BACKLOG.md:756, cuja raiz é PROGRAMA-CELULA-E-O-DIA. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| SITUACAO-LEITOR-CRU | LATERAL-ESTRUTURA | SIM | LEU app/colaboradores/views.py:1417, situação crua em lugar do juiz; censo/migração parte (b) da O50 em app/docs/BACKLOG.md:1332 e vínculo explícito em app/docs/BACKLOG.md:735. | O50 | DOBRA |
| RETRATO-DOIS-RELOGIOS | LATERAL-ESTRUTURA | SIM | LEU app/folha/fora_da_folha.py:222 e app/folha/fora_da_folha.py:224, chamadas sem hoje compartilhado; app/folha/export.py:968 também omite hoje; INFERIU complemento da regra de relógio na borda em app/docs/BACKLOG.md:1057. | RELOGIO-INJETADO | DOBRA |
| HJ-092 | LATERAL-ESTRUTURA | ? | LEU app/chamados/services/materializacao.py:302 e app/chamados/services/materializacao.py:310, divergência registrada com prevalência do marco; encaminhamento das 15 batidas é dado administrativo em app/docs/BACKLOG.md:741, não fornecido atualmente [não verificado]. | - | CONTA |
| HJ-093 | LATERAL-ESTRUTURA | SIM | LEU app/ponto/services/justificativa.py:152, geração da saída depende de saida_informada; classe declarada em app/docs/BACKLOG.md:743; INFERIU mecanismo ainda possível, caso atual [não verificado]. | JUSTIFICATIVA-PAGA-O-PERIODO | DOBRA |
| LANCAR-AUSENCIA-AFASTA-DESLIGADO | LATERAL-ESTRUTURA | SIM | LEU app/ponto/views.py:2373, filtro só na listagem; app/ponto/views.py:2435 grava afastado no POST sem guarda de desligamento; INFERIU brecha descrita em app/docs/BACKLOG.md:745. | - | CONTA |
| FERIADO-RUBRICA-DO-FLAG-CORRENTE | LATERAL-ESTRUTURA | SIM | LEU app/ponto/services/fechamento.py:948 e app/ponto/services/fechamento.py:949, rubrica consulta vínculo/flag atuais; INFERIU complemento da mesma regra de feriado, app/docs/BACKLOG.md:746. | FERIADO-ESCALA | DOBRA |
| PILHA-DA-BATIDA-RECALCULO-INLINE | LATERAL-ESTRUTURA | SIM | LEU app/ponto/services/fechamento.py:1810 e app/ponto/services/fechamento.py:1824, recálculo integral permanece em on_commit; INFERIU custo síncrono possível em autocommit; latência atual [não verificado]. | - | CONTA |
| O-BITEMPORAL-2 | LATERAL-ESTRUTURA | PARCIAL | LEU vigência em app/escala/models.py:875; app/colaboradores/services/vinculo.py:274 sobrescreve tipo na mesma linha e app/colaboradores/services/vinculo.py:283 salva; INFERIU que histórico de auditoria em app/colaboradores/services/vinculo.py:376 não equivale a versão bitemporal completa. | - | CONTA |
| IA-DOMINIO-POR-FERRAMENTA | LATERAL-ESTRUTURA | SIM | LEU app/core/ia_acoes.py:69, autorização pela união das ações; app/core/views_ia.py:908 envia cliente/pergunta/histórico sem permissões por ferramenta; INFERIU brecha ainda existente. | - | CONTA |
| REJULGAR-COMP10-4-REVERTIDOS | LATERAL-ESTRUTURA | ? | LEU app/docs/BACKLOG.md:766, aplicação histórica e divergência restante; nenhuma foto atual dos quatro casos ou DIFF foi fornecida [não verificado]. | - | CONTA |
| TERMOMETRO-GRAVA-SEM-TRILHA | LATERAL-ESTRUTURA | NAO | LEU app/colaboradores/services/vinculo.py:1077, porta grava autor/motivo/antes/depois; app/escala/regua_defesa.py:577 descreve passo por avaliação; INFERIU cura no main. | - | OBSOLETA |
| TROCA-DE-POSTO-SEM-VINCULO | LATERAL-ESTRUTURA | SIM | LEU app/colaboradores/views.py:1388 e app/colaboradores/views.py:1391, transferência grava cadastro; app/colaboradores/views.py:1396 registra histórico sem encerrar/criar vínculo; INFERIU defeito ainda possível. | - | CONTA |
| LAVRAR-HE-PENDENTE-ESCALA | LATERAL-ESTRUTURA | PARCIAL | LEU identity map em app/ponto/services/fechamento.py:278; residual consulta feriado em app/core/feriados.py:82; lista de resíduos em app/docs/BACKLOG.md:773; segunda apuração/custo atual [não verificado]. | - | CONTA |
| N1-ALIMENTACAO-UNICA | LATERAL-ESTRUTURA | PARCIAL | LEU reaproveitamento de identidades em app/chamados/services/vigia_fio.py:608; nova leitura de perguntas em app/chamados/services/vigia_fio.py:619; app/folha/fora_da_folha.py:62 alimenta separadamente; INFERIU residual de alimentação. | - | CONTA |
| BO-ADMIN-2710 | LATERAL-ESTRUTURA | ? | LEU inventário de oito fatos em app/docs/BACKLOG.md:777; medida/destino por item atuais não fornecidos e não há sítio único de código [não verificado]. | - | CONTA |
| TRILHA-DO-VINCULO-MENTE | LATERAL-ESTRUTURA | PARCIAL | LEU cura de criar/editar em app/colaboradores/services/vinculo.py:370 e app/colaboradores/services/vinculo.py:376; RT-03 permanece como pedido em app/docs/BACKLOG.md:778; mecanismo de reparentamento em app/escala/signals.py:204, ocorrência residual atual [não verificado]. | - | CONTA |
| REGISTRADO-NAO-E-VIVO | LATERAL-ESTRUTURA | PARCIAL | LEU leitores curados em app/core/services/painel_op.py:119 e app/core/context_processors.py:150; instrumento próprio ainda pedido em app/docs/BACKLOG.md:780, não localizado por nome/significado [não verificado]. | - | CONTA |
| SILENCIO-VENCIDO-VOLTA | LATERAL-ESTRUTURA | SIM | LEU app/chamados/services/verbos.py:51, silêncio vencido ainda vira HISTORICO; app/chamados/services/painel.py:331 retira-o da lista; ligação ao sítio 1 de PENDENCIA-SEM-JUIZ-3 em app/docs/BACKLOG.md:782 e app/docs/BACKLOG.md:783. | PENDENCIA-SEM-JUIZ-3 | DOBRA |
| QUARENTENA-DO-COLAB | LATERAL-ESTRUTURA | SIM | LEU app/chamados/views_arquivar.py:24 e app/chamados/views_arquivar.py:37, porta atual silencia chamado individual; app/chamados/services/painel.py:189 não consulta quarentena por período; INFERIU falta da entidade/porta pedida; raiz O44 explícita em app/docs/BACKLOG.md:784. | O44 | DOBRA |

TRECHO: 50 linhas, CONTA 18, DOBRA 27, OBSOLETA 5, ? 12.

## 13/11 21:04 · sessao 8938431984fe · - · - · origem=Ronald (tmux)

| id | balde | existe (SIM/NAO/PARCIAL/?) | prova (arquivo:linha do HEAD ou commit; LEU x INFERIU) | mae | veredito (CONTA/DOBRA/OBSOLETA) | [nao verificado]
|---|---|---|---|---|---|
| ATA-DEFASADA-COMP09 | LATERAL-ESTRUTURA | ? | LEU app/docs/BACKLOG.md:787: diagnóstico contestado; app/ponto/services/cartorio.py:943: forçar ignora impressão. Acervo atual não fornecido [não verificado]; INFERIU mãe pela origem COBPARC. | COBERTURA-PARCIAL-UM-JUIZ | DOBRA |
| COBPARC-ATA-DEFASADA-09 | LATERAL-ESTRUTURA | ? | LEU app/docs/BACKLOG.md:791: resta divergência histórica atribuída a col211; sem ata/medida atual [não verificado]. INFERIU mãe pela mesma correção parcial. | COBERTURA-PARCIAL-UM-JUIZ | DOBRA |
| FECHAR3-SOBREVIVENTE-NO-RECONCILIAR | LATERAL-ESTRUTURA | SIM | LEU app/ponto/services/entrada_sincronizada.py:32 e app/ponto/services/triagem_batida.py:598: encaminham o próprio escolhido ao reconciliar; INFERIU permanência do risco do sobrevivente, ocorrência em produção [não verificado]. | - | CONTA |
| REJULGAR-61-COBPARC | LATERAL-ESTRUTURA | ? | LEU app/docs/BACKLOG.md:801: passivo de atas sem carimbo; app/ponto/services/cartorio.py:770: carimbo escrito hoje. Existência atual das 61 [não verificado]; INFERIU mãe pelo mesmo defeito. | COBERTURA-PARCIAL-UM-JUIZ | DOBRA |
| CXR-MEDIDO-EM-AUTO-NOW | LATERAL-ESTRUTURA | SIM | LEU app/ponto/services/cxr_na_lista.py:160: medido_em recebe calculado_em; INFERIU mãe pelo mesmo lavrador CXR, app/docs/BACKLOG.md:807. | CXR-TELA-LE-O-MOTOR | DOBRA |
| CXR-BECO-SEM-BATIDA-FORA-DA-LAVRA | LATERAL-ESTRUTURA | SIM | LEU app/ponto/services/cxr_na_lista.py:68 e app/ponto/services/cxr_na_lista.py:71: universo deriva de Batida; INFERIU exclusão de quem não bateu e mãe CXR. | CXR-TELA-LE-O-MOTOR | DOBRA |
| TETO-NA-LISTA-DOIS-RELOGIOS | LATERAL-ESTRUTURA | SIM | LEU app/ponto/management/commands/lavrar_teto_do_dia.py:27 e app/ponto/services/teto_na_lista.py:49: localdate na entrada e now no serviço; raiz do relógio em app/docs/BACKLOG.md:1057. | JUIZ-DO-PAR-GRUPO-1 | DOBRA |
| TRAVA-COLAB-X-DISPUTA | LATERAL-ESTRUTURA | NAO | LEU app/chamados/models.py:927 e app/chamados/services/disputa_emissao.py:1053: portas passaram a C→D; app/chamados/services/disputa_emissao.py:1059 trava o repasse completo. INFERIU removida a inversão C/D específica de app/docs/BACKLOG.md:811. | FIO-E0-V4-E1-ENTRADAS | OBSOLETA |
| JUSTIFICATIVA-ADMIN-FORA-DA-PORTA | LATERAL-ESTRUTURA | SIM | LEU app/ponto/admin.py:19: JustificativaAdmin continua ModelAdmin editável, sem porta de aprovação própria; INFERIU mãe pelo bypass da cura de origem descrito em app/docs/BACKLOG.md:815. | JUSTIFICATIVA-ENCERRA-ORIGEM | DOBRA |
| REATAR-ESCREVE-EM-DIA-TRANCADO | LATERAL-ESTRUTURA | SIM | LEU app/chamados/reconciliador.py:1593: dia trancado permanece no caminho histórico; app/chamados/reconciliador.py:1594 aplica cerca somente ao alcance novo. INFERIU residual da E2 do programa, app/docs/BACKLOG.md:893. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| PARCIAL-NA-PORTA | LATERAL-ESTRUTURA | SIM | LEU app/ponto/services/cartorio.py:398 e app/chamados/services/disputa_emissao.py:1047: parcial barrada nos chamadores; app/chamados/models.py:880: porta geral separada. INFERIU centralização ainda faltante, app/docs/BACKLOG.md:857. | COBERTURA-PARCIAL-UM-JUIZ | DOBRA |
| COBPARC-ROTULO-FILA-V2 | LATERAL-ESTRUTURA | PARCIAL | LEU app/inteligencia/calculadores.py:57: contagem usa marcos_do_jogo; app/ponto/propositor.py:40 ainda conta casadas por not missing. INFERIU resíduos da mesma classe, app/docs/BACKLOG.md:859; alcance de produção [não verificado]. | COBERTURA-PARCIAL-UM-JUIZ | DOBRA |
| COBPARC-FILA-RELOGIO | LATERAL-ESTRUTURA | SIM | LEU app/ponto/services/cartorio.py:373: houve_fato_novo ainda usa now quando ate ausente; app/ponto/services/cartorio.py:428 repassa agora opcional. INFERIU residual COBPARC, app/docs/BACKLOG.md:861. | COBERTURA-PARCIAL-UM-JUIZ | DOBRA |
| COBPARC-FILA-CENSO-V7 | LATERAL-ESTRUTURA | SIM | LEU app/ponto/services/simular_dia.py:12 e app/ponto/services/bordas_realizado.py:74: leitores ainda excluem tipo diferente de trabalho. INFERIU permanência de parte do censo, app/docs/BACKLOG.md:863. | COBERTURA-PARCIAL-UM-JUIZ | DOBRA |
| COBPARC-FILA-V8-PARCIAL-DEMISSAO | LATERAL-ESTRUTURA | ? | LEU app/ponto/services/cartorio.py:1145: grade recebe data_demissao; app/docs/BACKLOG.md:864 exige medir após o pouso. Sem dia/ata atual [não verificado]; INFERIU mãe COBPARC. | COBERTURA-PARCIAL-UM-JUIZ | DOBRA |
| COBPARC-FILA-V8-VOCABULARIO | LATERAL-ESTRUTURA | SIM | LEU app/ponto/services/cartorio.py:778: justificativa continua literal reservado da dispensa; app/escala/services/leitor_celula.py:81 interpreta esse valor. INFERIU colisão potencial com código de ausência, app/docs/BACKLOG.md:865. | COBERTURA-PARCIAL-UM-JUIZ | DOBRA |
| COBPARC-FILA-V8-PARCIAL-JUSTIF | LATERAL-ESTRUTURA | SIM | LEU app/escala/utils.py:1210 e app/escala/utils.py:1254: cobertura entra antes do elif da justificativa; INFERIU permanência do encontro parcial+justificativa descrito em app/docs/BACKLOG.md:866. | COBERTURA-PARCIAL-UM-JUIZ | DOBRA |
| COBPARC-FILA-H10-PARCIAIS-09 | LATERAL-ESTRUTURA | ? | LEU app/docs/BACKLOG.md:867: passivo exportado; atualidade das 38 [não verificado]. LEU app/docs/BACKLOG.md:947: BACKFILL-UNICO absorve explicitamente esta linha. | BACKFILL-UNICO | DOBRA |
| COBPARC-FILA-H10-COL926 | LATERAL-ESTRUTURA | ? | LEU app/docs/BACKLOG.md:868: Pauta retirada do pouso; entrega atual [não verificado]. LEU app/docs/BACKLOG.md:947: absorção explícita pelo BACKFILL-UNICO. | BACKFILL-UNICO | DOBRA |
| V-FASE-AS-OF-TRANSFERIDO | LATERAL-ESTRUTURA | SIM | LEU app/ponto/management/commands/e6_oraculo.py:179: passa empresa atual; app/escala/services/fase_12x36.py:146 exige a mesma empresa na lavra. INFERIU defeito de transferência; mãe em app/docs/BACKLOG.md:1774. | V-FASE-DONO-AS-OF | DOBRA |
| V-FASE-AS-OF-PARIDADE-UNIVERSO | LATERAL-ESTRUTURA | SIM | LEU app/ponto/management/commands/censo_fase_12x36.py:179: cadastro lê células/folgas; app/ponto/management/commands/censo_fase_12x36.py:210: proposta lê template. INFERIU universos distintos e residual AS-OF, app/docs/BACKLOG.md:901. | V-FASE-DONO-AS-OF | DOBRA |
| V-FASE-AS-OF-CUSTO | LATERAL-ESTRUTURA | ? | LEU app/escala/services/fase_12x36.py:134: percorre índice enquanto faltam dias; custo real/medição solicitada em app/docs/BACKLOG.md:902 [não verificado]. INFERIU mãe pelo leitor AS-OF. | V-FASE-DONO-AS-OF | DOBRA |
| DEGRAU-HE-100-CADASTRO | LATERAL-ESTRUTURA | SIM | LEU app/ponto/calculador/periodo.py:173: DEGRAU_HE_100_MIN permanece 120.0; app/ponto/calculador/periodo.py:386 aplica a constante. INFERIU falta de cadastro nesse sítio. | - | CONTA |
| V-FASE-AS-OF-SEM-CELULA | LATERAL-ESTRUTURA | SIM | LEU app/escala/services/fase_12x36.py:188: reabertura depende de carimbos de CelulaDia; INFERIU ausência de gatilho no dia sem célula. Mãe explícita em app/docs/BACKLOG.md:905. | V-FASE-DONO-AS-OF | DOBRA |
| LENVIVOS-SOBRAS-ESMERIL | LATERAL-ESTRUTURA | SIM | LEU app/ponto/management/commands/supra_juiz.py:248: CSF_SEM_LAVRA é contado; app/ponto/management/commands/supra_juiz.py:305: não integra ZERO do alarme. INFERIU residual LEN-VIVOS, app/docs/BACKLOG.md:908; raiz E2 em app/docs/BACKLOG.md:893. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| LENVIVOS-EMITIR-INERTE | LATERAL-ESTRUTURA | SIM | LEU app/ponto/management/commands/emitir_furo_retroativo.py:102 e app/ponto/management/commands/emitir_furo_retroativo.py:111: passa chamados_vivos e consome apenas FURO_SEM_COBRANCA; app/ponto/supra_juiz.py:373 usa chamados_total nesse ramo. INFERIU residual LEN-VIVOS, raiz app/docs/BACKLOG.md:893. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| LENVIVOS-CHAVE-DO-DIA | LATERAL-ESTRUTURA | SIM | LEU app/ponto/management/commands/emitir_furo_retroativo.py:84: dia_do_fato(fortes=True); app/ponto/management/commands/supra_juiz.py:200: data_do_chamado. INFERIU divergência de chave; residual app/docs/BACKLOG.md:910, raiz app/docs/BACKLOG.md:893. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| LENVIVOS-COBRANCAS-VIVAS | LATERAL-ESTRUTURA | PARCIAL | LEU app/ponto/services/cartorio.py:1389: _com_fio nasce de vivos; app/ponto/services/cartorio.py:1467: parâmetro recebe universo sem _protesta; app/ponto/services/cartorio.py:1502: há filtro posterior. INFERIU residual, app/docs/BACKLOG.md:911; raiz app/docs/BACKLOG.md:893. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| V-FASE-AS-OF-ATRASO | LATERAL-ESTRUTURA | SIM | LEU app/config/crons.py:638: censo diário depende apenas de reconciliar_vinculo; app/escala/services/fase_12x36.py:188: inclui julgada_em. INFERIU possibilidade de esperar próxima lavra; mãe explícita app/docs/BACKLOG.md:916. | V-FASE-DONO-AS-OF | DOBRA |
| V-FASE-AS-OF-UNIVERSO | LATERAL-ESTRUTURA | NAO | LEU app/ponto/management/commands/censo_fase_12x36.py:109: universo inclui qualquer vínculo histórico 12x36, sem exigir que seja o último; app/ponto/management/commands/censo_fase_12x36.py:161 resolve vínculo por dia. INFERIU removido o filtro descrito em app/docs/BACKLOG.md:917. | V-FASE-DONO-AS-OF | OBSOLETA |
| V-FASE-AS-OF-SOBREPOSTA | LATERAL-ESTRUTURA | SIM | LEU app/escala/services/fase_12x36.py:114: cobertura por intervalo; app/escala/services/fase_12x36.py:179: reabertura usa tripla exata. INFERIU divergência possível em sobreposição; mãe explícita app/docs/BACKLOG.md:918. | V-FASE-DONO-AS-OF | DOBRA |
| V-FASE-AS-OF-INTRADIA | LATERAL-ESTRUTURA | SIM | LEU app/escala/services/fase_12x36.py:91: update_or_create pela mesma data_ref; INFERIU substituição da versão intradia. Mãe explícita app/docs/BACKLOG.md:919. | V-FASE-DONO-AS-OF | DOBRA |
| PROPOSITOR-JANELA-LONGA | LATERAL-ESTRUTURA | SIM | LEU app/escala/services/propositor.py:493: fase invertida lê itens da última lavra; app/ponto/management/commands/censo_fase_12x36.py:89: janelas correntes são anterior+atual. INFERIU lacuna para consulta maior; mãe explícita app/docs/BACKLOG.md:920. | V-FASE-DONO-AS-OF | DOBRA |
| FILA-POR-CAUSA-AS-OF | LATERAL-ESTRUTURA | SIM | LEU app/chamados/services/fila_por_causa.py:184: lê colabs_de_desvio da última lavra, sem AS-OF por dia do chamado. Mãe explícita app/docs/BACKLOG.md:921. | V-FASE-DONO-AS-OF | DOBRA |
| EMIS-PAUTA-DEDUP | LATERAL-ESTRUTURA | SIM | LEU app/chamados/services/fabrica_da_celula.py:277: consulta precede escrever, sem trava nessa porta; app/chamados/services/fabrica_da_celula.py:263: dedup por prefixo textual. INFERIU risco concorrente, residual emissores app/docs/BACKLOG.md:931; raiz E2 app/docs/BACKLOG.md:893. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| GRADE-09-PAUTA | LATERAL-ESTRUTURA | ? | LEU app/docs/BACKLOG.md:940: pede ensaio e Pauta com diferença pago/devido; ensaio/entrega atuais não localizados [não verificado]. INFERIU mãe pela correção de folgas de app/docs/BACKLOG.md:827 e app/docs/BACKLOG.md:844. | GRADE-FOLGA-DO-VINCULO | DOBRA |
| ISENTO-H10-MENORES | LATERAL-ESTRUTURA | PARCIAL | LEU app/ponto/services/cartorio.py:1470: leitor cru de isento permanece; app/docs/ADR/0003-celula-tres-naturezas.md:27: horizonte já documentado. INFERIU resíduos da isenção, app/docs/BACKLOG.md:945; raiz E3 do programa. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| NOREVERSE-UI-NO-CORE | LATERAL-ESTRUTURA | PARCIAL | LEU app/pautas/services.py:439: link usa TELA_CALENDARIO, curando o reverse; app/docs/RELATO.md:1825 conserva resultado 6 após a corrente como pendente. Prova posterior [não verificado]. | - | CONTA |
| ISENTO-FLAG-ATEMPORAL | LATERAL-ESTRUTURA | SIM | LEU app/colaboradores/models.py:294: isento_ponto continua BooleanField sem vigência; app/ponto/services/cartorio.py:1190 lê a flag atual para o julgamento. INFERIU permanência da pergunta de desenho de app/docs/BACKLOG.md:956. | - | CONTA |
| V3-P5-PRORROGACAO-POR-INTERVALO | LATERAL-ESTRUTURA | NAO | LEU app/ponto/motor_calculo_v2.py:162 e app/ponto/motor_calculo_v2.py:191: pausa usa janela do recorte, com união e teto; app/docs/RELATO.md:7735 registra pouso 16c27de8. Mãe explícita app/docs/BACKLOG.md:1862. | V2-BUGS-DO-MOTOR | OBSOLETA |
| V3-HE-NOTURNA-POR-PECA | LATERAL-ESTRUTURA | SIM | LEU app/ponto/motor_calculo_v2.py:441: janela da HE ainda começa em inicio_extra, separada da entrada do turno. INFERIU residual da mesma classe noturna da obra ampla app/docs/BACKLOG.md:1862; efeito atual na frota [não verificado]. | V2-BUGS-DO-MOTOR | DOBRA |
| EMIS-FALSE-ATA-TRABALHO | LATERAL-ESTRUTURA | ? | LEU app/ponto/services/cartorio.py:738: ata copia tipo_dia da grade; app/docs/BACKLOG.md:969 descreve contradição de estoque, sem células atuais fornecidas [não verificado]. INFERIU mãe pelo contrato de coerência da célula do programa. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| GRADE-FOLGA-C5 | LATERAL-ESTRUTURA | SIM | LEU app/chamados/management/commands/auditar_invariantes_chamados.py:1536: C5 seleciona FolgaDia diretamente e aceita outro vínculo cobrindo, sem distinguir origem calendário/manual pelo juiz. INFERIU mesma classe de app/docs/BACKLOG.md:827 e app/docs/BACKLOG.md:970. | GRADE-FOLGA-DO-VINCULO | DOBRA |
| GRADE-FOLGA-GERADOR | LATERAL-ESTRUTURA | SIM | LEU app/escala/models.py:1310: gerador continua lendo FolgaDia do próprio vínculo; INFERIU ausência da folga manual de outro vínculo e mãe da classe, app/docs/BACKLOG.md:827 e app/docs/BACKLOG.md:971. | GRADE-FOLGA-DO-VINCULO | DOBRA |
| V3-MOTOR-FERIADO-HE100-NOTURNA | LATERAL-ESTRUTURA | NAO | LEU app/ponto/motor_calculo_v2.py:3104 e app/ponto/motor_calculo_v2.py:3361: dobra chama split noturno por período; app/ponto/tests/test_o280_bateria_da_lei.py:1654 cobre caso pela regra. Mãe explícita app/docs/BACKLOG.md:1862. | V2-BUGS-DO-MOTOR | OBSOLETA |
| COBPARC-B-CAUSA-NO-MOTOR | LATERAL-ESTRUTURA | SIM | LEU app/ponto/motor_calculo_v2.py:1655: publicador de causas; app/escala/utils.py:1234 mantém sem_periodo na grade, sem chamada ao publicador. INFERIU lacuna da causa parcial descrita em app/docs/BACKLOG.md:973; quantidade atual [não verificado]. | COBERTURA-PARCIAL-UM-JUIZ | DOBRA |
| V3-NOTURNO-RECORTE-LONGO | LATERAL-ESTRUTURA | SIM | LEU app/ponto/motor_calculo_v2.py:94: acima de 26h retorna zero; app/ponto/motor_calculo_v2.py:111 estende segmento e app/ponto/motor_calculo_v2.py:112 soma também 22–24. INFERIU sobreposição. Mãe explícita app/docs/BACKLOG.md:1862. | V2-BUGS-DO-MOTOR | DOBRA |
| REVERTER-REGEN-TRIESTADO | LATERAL-ESTRUTURA | SIM | LEU app/ponto/portas/celula.py:829: trabalha reconstruído por comparação booleana de marcos, sem representar None. INFERIU residual do contrato triestado da célula, app/docs/BACKLOG.md:976 e app/docs/BACKLOG.md:893. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| P14-CONFIRMAR-IMPLAUSIVEL-SEM-MOTIVO | LATERAL-ESTRUTURA | SIM | LEU app/chamados/services/validacao.py:396: porta recebe confirmar_implausivel sem motivo; app/chamados/services/validacao.py:428: confirmação booleana ultrapassa recusa. INFERIU ausência da exigência escrita descrita em app/docs/BACKLOG.md:985. | - | CONTA |
| F5-RELOGIO-DO-EXPORT | LATERAL-ESTRUTURA | SIM | LEU app/folha/export.py:968: classificar_export chama motivos_retencao_espelho sem hoje; app/folha/export.py:1238: retenção admite localdate interno. Raiz explícita do relógio em app/docs/BACKLOG.md:987 e app/docs/BACKLOG.md:1057. | JUIZ-DO-PAR-GRUPO-1 | DOBRA |
| PERGUNTA-VIVA-LOGA-PREVISTO-COMO-FALHA | LATERAL-ESTRUTURA | SIM | LEU app/chamados/services/materializacao.py:302: divergência prevista chama registrar_engolido com Exception em app/chamados/services/materializacao.py:307. INFERIU permanência do alarme indevido; volume atual [não verificado]. | - | CONTA |
| O261-V-FASE-NA-LISTA | LATERAL-ESTRUTURA | NAO | LEU app/escala/services/cadastro_realidade.py:393: C10 lê lavra de fase e publica os dias; app/config/crons.py:620 registra autoridade do censo e retirada do detector paralelo. INFERIU cura da entrega; mãe O261 citada em app/docs/BACKLOG.md:999 não consta no catálogo fornecido. | - | OBSOLETA |

TRECHO FINAL: 52 linhas, CONTA 6, DOBRA 41, OBSOLETA 5, ? 9.

## 13/11 21:03 · sessao f7d268957fb1 · - · - · origem=Ronald (tmux)

| id | balde | existe (SIM/NAO/PARCIAL/?) | prova (arquivo:linha do HEAD ou commit; LEU x INFERIU) | mae | veredito (CONTA/DOBRA/OBSOLETA) | [nao verificado]
|---|---|---|---|---|---|
| E1-FILA-OPUS6-FILA | ESPINHA-E | ? | LEU app/chamados/management/commands/auditar_invariantes_chamados.py:1162; acervo, deadlock e custo atuais [não verificado]; INFERIU vínculo à revisão da E1 por app/docs/BACKLOG.md:1895 | FIO-E0-V4-E1-ENTRADAS | DOBRA |
| E1-FILA-V2-2-TRAVA | ESPINHA-E | PARCIAL | LEU app/chamados/services/disputa_emissao.py:1003: fecho conferido antes da escrita; app/chamados/models.py:290 ainda pré-trava caminho que pode alcançar repasse; INFERIU residual pela composição; app/docs/BACKLOG.md:1896 | FIO-E0-V4-E1-ENTRADAS | DOBRA |
| E1-FILA-V2-2-OUTRAS-PORTAS | ESPINHA-E | SIM | LEU app/chamados/services/declaracao_texto.py:299 e app/chamados/models.py:290: pré-travas locais antes do fecho transitivo do repasse; INFERIU risco de ordem, ocorrência [não verificado]; app/docs/BACKLOG.md:1897 | FIO-E0-V4-E1-ENTRADAS | DOBRA |
| E1-FILA-V2-2-RESIDUOS | ESPINHA-E | PARCIAL | LEU app/chamados/services/disputa_emissao.py:1032: None significa nada a converter, mas app/chamados/services/disputa_emissao.py:1083 grava antes do return None de app/chamados/services/disputa_emissao.py:1094; custo SQL [não verificado]; app/docs/BACKLOG.md:1898 | FIO-E0-V4-E1-ENTRADAS | DOBRA |
| RAIZ-DA-DISPUTA-F0F1 | ESPINHA-E | SIM | LEU app/chamados/services/disputa_emissao.py:237: destino pode ficar indeterminado; app/chamados/services/disputa_emissao.py:587: nascimento separado dos filhos; INFERIU lacuna do agregado; mãe explícita em app/docs/BACKLOG.md:1935 | RAIZ-DA-DISPUTA | DOBRA |
| FIO-E0-V4-E4-ESCRITORES | ESPINHA-E | PARCIAL | LEU app/chamados/services/validacao.py:160: projeta e registra ato; app/chamados/juizes.py:1385 ainda lê projeções; sequência Expand/Contract em app/docs/BACKLOG.md:1915 | - | CONTA |
| FIO-E0-V4-E4-RETIRADA-FISICA | ESPINHA-E | SIM | LEU app/chamados/models.py:2505, app/chamados/models.py:2514 e app/chamados/models.py:3449: campos legados permanecem; INFERIU parte final da obra em app/docs/BACKLOG.md:1916 | FIO-E0-V4-E4-ESCRITORES | DOBRA |
| FIO-E0-V4-FATO-A03 | ESPINHA-E | SIM | LEU app/chamados/models.py:2205: reabertura administrativa grava projeção da disputa sem fato próprio em DecisaoPergunta; catálogo de atos em app/chamados/models.py:3915; exceção em app/docs/BACKLOG.md:1917 | FIO-E0-V4-E4-ESCRITORES | DOBRA |
| FIO-E0-V4-FATO-A05 | ESPINHA-E | SIM | LEU app/chamados/juizes.py:1401: assinatura validada continua decidindo pela projeção; exceção mantida em app/chamados/tests/test_e4_expand_orm.py:3; INFERIU ausência do fato próprio; app/docs/BACKLOG.md:1918 | FIO-E0-V4-E4-ESCRITORES | DOBRA |
| FIO-E0-V4-FATO-A06 | ESPINHA-E | SIM | LEU app/chamados/models.py:3238: identidade grava via/carimbo; catálogo app/chamados/models.py:3915 sem ato próprio de identidade; INFERIU lacuna descrita em app/docs/BACKLOG.md:1919 | FIO-E0-V4-E4-ESCRITORES | DOBRA |
| FIO-E0-V4-FATO-A17 | ESPINHA-E | SIM | LEU app/chamados/services/materializacao.py:856: pendência técnica persiste em pendencia_admin/recusa_motivo; catálogo app/chamados/models.py:3915 sem ato técnico próprio; app/docs/BACKLOG.md:1920 | FIO-E0-V4-E4-ESCRITORES | DOBRA |
| FIO-E0-V4-FATO-A18 | ESPINHA-E | SIM | LEU app/chamados/services/materializacao.py:1107: recusa técnica grava recusa_motivo; catálogo app/chamados/models.py:3915 sem ato próprio dessa recusa; app/docs/BACKLOG.md:1921 | FIO-E0-V4-E4-ESCRITORES | DOBRA |
| FIO-E0-V4-FATO-A20 | ESPINHA-E | SIM | LEU app/chamados/services/validacao.py:691: conferência grava data_conferida_em/por e auditoria; catálogo app/chamados/models.py:3915 sem fato próprio; validade após mudança de dias [não verificado]; app/docs/BACKLOG.md:1922 | FIO-E0-V4-E4-ESCRITORES | DOBRA |
| FIO-E0-V4-E4A-2 | ESPINHA-E | PARCIAL | LEU app/chamados/management/commands/disparar_perguntas_competencia.py:129: cron virou vigia; app/chamados/management/commands/disparar_perguntas_competencia.py:117 ainda conta limbo como alvo e app/chamados/management/commands/disparar_perguntas_competencia.py:153 usa janela global; app/docs/BACKLOG.md:1923 | FIO-E0-V4-E4-ESCRITORES | DOBRA |
| BO-DISPUTA-SEM-PERGUNTA | ESPINHA-E | SIM | LEU app/chamados/services/disputa_emissao.py:587: disputa criada no atomic interno; app/chamados/services/disputa_emissao.py:670 gera perguntas depois do retorno; INFERIU nascimento sem garantia de filho; raiz explícita em app/docs/BACKLOG.md:1935 | RAIZ-DA-DISPUTA | DOBRA |
| BO-DISPUTA-MATRIZ-ANTES-DO-OPUS | ESPINHA-E | ? | LEU app/docs/BACKLOG.md:1930: matriz nomeada em logs, indisponível nesta cópia [não verificado]; código do nascimento em app/chamados/services/disputa_emissao.py:587; raiz por app/docs/BACKLOG.md:1935 | RAIZ-DA-DISPUTA | DOBRA |
| BUG-PAR-FROTA-1110 | ESPINHA-E | SIM | LEU app/chamados/services/materializacao.py:86: paridade considera vizinha individual; app/chamados/services/materializacao.py:1164 processa uma pergunta; INFERIU persistência da classe, frota atual [não verificado]; cadeia explícita em app/docs/BACKLOG.md:1934 e app/docs/BACKLOG.md:1935 | RAIZ-DA-DISPUTA | DOBRA |
| RAIZ-DA-DISPUTA | ESPINHA-E | SIM | LEU app/chamados/services/disputa_emissao.py:587 e app/chamados/services/disputa_emissao.py:670: raiz e filhos nascem em passos separados; app/chamados/models.py:3246 grava identidade sem fechamento do agregado; INFERIU lacuna estrutural de reação | - | CONTA |
| F4-PAR-PELA-CELULA | ESPINHA-E | SIM | LEU app/chamados/models.py:3493: instancia_marco derivada do motivo; app/chamados/juizes.py:957 combina todas as pausas do DNA; INFERIU identificação incompleta da pausa específica; mãe explícita em app/docs/BACKLOG.md:1949 | RAIZ-DA-DISPUTA | DOBRA |
| DISPUTA-ZUMBI-1110 | ESPINHA-E | PARCIAL | LEU app/chamados/reconciliador.py:412: este caminho já usa C7; app/chamados/models.py:3246: carimbo de identidade não aciona fecho do agregado; INFERIU residual; raiz F3/F4 explícita em app/docs/BACKLOG.md:1955 | RAIZ-DA-DISPUTA | DOBRA |
| PERGUNTA-DUPLICATA-1110 | ESPINHA-E | ? | LEU app/chamados/models.py:3257: duplicata tem saída terminal; 486 grupos/848 excedentes e impacto atual só constam como relato em app/docs/BACKLOG.md:1956 [não verificado]; raiz de duplicatas em app/docs/BACKLOG.md:1935 | RAIZ-DA-DISPUTA | DOBRA |
| DATA-EVENTO-CRU | ESPINHA-E | SIM | LEU app/api/views.py:1153: API publica data_evento cru e formata rótulo sem juiz; INFERIU leitor ainda fora da autoridade; raiz semântica por app/docs/BACKLOG.md:1856 e app/docs/BACKLOG.md:1858 | ESMERIL-SEMANTICA | DOBRA |
| LAVRA-PREMATURA-CONCORDE | ESPINHA-E | ? | LEU app/ponto/services/cartorio.py:1363: julgamento até teto durante turno; app/ponto/services/cartorio.py:507 conserva concorde em curso; se isso constitui lavra indevida depende do desenho pendente em app/docs/BACKLOG.md:1984 [não verificado] | - | CONTA |
| FIO-E0-V4-VIGIA-FABRICA-POR-EVENTO-CRESCE | ESPINHA-E | ? | LEU app/chamados/management/commands/disparar_perguntas_competencia.py:211: vigia continua medindo sem emitir; crescimento atual e cobertura dos 39 dias [não verificado]; INFERIU parte da fábrica de app/docs/BACKLOG.md:1915; registro em app/docs/BACKLOG.md:1989 | FIO-E0-V4-E4-ESCRITORES | DOBRA |
| E4A2-RELOGIO-FIO-VIVO | ESPINHA-E | SIM | LEU app/chamados/juizes.py:1513: fio_vivo chama pergunta_viva sem instante; fallback em app/chamados/juizes.py:592 lê date.today; raiz inferida por app/docs/BACKLOG.md:1057 | JUIZ-DO-PAR-GRUPO-1 | DOBRA |
| O169-JUIZES-760 | ESPINHA-E | SIM | LEU app/chamados/juizes.py:780: destino em ENCERRADOS é testemunha sem filtro de módulo/via; alcance atual [não verificado]; INFERIU residual da O169 por app/docs/BACKLOG.md:1994 | O169 | DOBRA |
| O169-NAO-MEDIDO-ESCAPA | ESPINHA-E | SIM | LEU app/ponto/services/cluster_espurio.py:115 e app/ponto/services/cluster_espurio.py:126: sem dia/insumos sai dos pks; retorno em app/ponto/services/cluster_espurio.py:148 não discrimina não medidos; INFERIU residual por app/docs/BACKLOG.md:2000 | O169 | DOBRA |
| O169-V5-GRAVAR-ESTADO-SEM-TRILHA | ESPINHA-E | NAO | LEU app/chamados/models.py:280 e app/chamados/models.py:292: _gravar_estado emite estado_chamado com antes/depois/ator antes de salvar no mesmo atomic; contradiz a premissa de app/docs/BACKLOG.md:2005 | O169 | OBSOLETA |
| E4A2-JANELA-POR-EMPRESA | ESPINHA-E | SIM | LEU app/chamados/management/commands/disparar_perguntas_competencia.py:93 e app/chamados/management/commands/disparar_perguntas_competencia.py:153: janela_atual sem empresa; INFERIU residual da E4a-2 por app/docs/BACKLOG.md:2010 | FIO-E0-V4-E4-ESCRITORES | DOBRA |
| E4A2-LIMBO-EXPORTADO-COMO-ALVO | ESPINHA-E | SIM | LEU app/chamados/management/commands/disparar_perguntas_competencia.py:117: alvo usa motivos_a_perguntar; app/chamados/services/fabrica_da_celula.py:214 encaminha limbo exportado a pauta; INFERIU falso alvo residual de app/docs/BACKLOG.md:2011 | FIO-E0-V4-E4-ESCRITORES | DOBRA |
| O169-FECHADA | ESPINHA-E | PARCIAL | LEU app/ponto/services/cluster_espurio.py:53: fecho por lastro; contador em app/ponto/services/cluster_espurio.py:88; legado trancado ainda para sem porta em app/ponto/services/cluster_espurio.py:237; app/docs/BACKLOG.md:2013 | O169 | DOBRA |
| PASSADO-SEM-TURNO-SEM-PAUTA-NAO-CONTADO | ESPINHA-E | SIM | LEU app/chamados/services/fabrica_da_celula.py:58: vencidos=None produz []; app/chamados/services/fabrica_da_celula.py:242: vigia retorna motivos, sem contador específico do sem-turno; INFERIU lacuna de contagem registrada em app/docs/BACKLOG.md:2017 | FIO-E0-V4-E4-ESCRITORES | DOBRA |
| PLANTIO-DIA-CIVIL-VIZINHO | ESPINHA-E | SIM | LEU app/chamados/juizes.py:53 e app/chamados/juizes.py:200: dia derivado por localdate(timestamp); INFERIU residual da classe dia-do-turno por app/docs/BACKLOG.md:1856 e app/docs/BACKLOG.md:1858 | ESMERIL-SEMANTICA | DOBRA |
| RETRATO-POR-EVENTO | ESPINHA-E | SIM | LEU app/folha/management/commands/lavrar_retrato_folha.py:65 e app/folha/management/commands/lavrar_retrato_folha.py:74: comando monta e grava retrato; escritor exclusivo documentado em app/folha/models.py:293; INFERIU ausência do consumidor por evento pedido em app/docs/BACKLOG.md:2042 | - | CONTA |
| F3-BECO-1-VOLTA-3 | ESPINHA-E | PARCIAL | LEU app/escala/folgas.py:283: mês que saiu do defeito entra no rejulgamento; app/chamados/services/limbo_folgas.py:245 reata pela porta; oito resíduos e medida F4 de app/docs/BACKLOG.md:2053 [não verificado]; raiz em app/docs/BACKLOG.md:893 | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| F3-BECO-2-COBRANCA-MORTA | ESPINHA-E | ? | LEU app/chamados/services/limbo_folgas.py:284: fechamento pode recusar após agrupamento; persistência dos dois casos de app/docs/BACKLOG.md:2054 [não verificado]; INFERIU parte do passivo F3 de app/docs/BACKLOG.md:893 | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| JV1-MERGE-FILA | ESPINHA-E | SIM | LEU app/ponto/services/fechamento.py:605: células limitadas ao envelope; feriados abrangem pontas em app/ponto/services/fechamento.py:595; app/ponto/services/espelho.py:1029 chama feriados_do_colab sem células; INFERIU resíduos ainda presentes | - | CONTA |
| E4-8-FORA-VINCULO | ESPINHA-E | SIM | LEU app/relatorios/furos.py:123: D4 consultado para todos os dias; app/relatorios/furos.py:128 cobra irregular sem recorte adicional por vínculo; ocorrência atual [não verificado]; INFERIU parte do programa de status, app/docs/BACKLOG.md:2069 e app/docs/BACKLOG.md:893 | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| E4-8-COMP-ANTIGA | ESPINHA-E | ? | LEU app/ponto/status_dia.py:294: dia encerrado sem lavra vira sem_certidao; disponibilidade/certificação das competências 06–09 de app/docs/BACKLOG.md:2070 [não verificado]; INFERIU parte do programa de status de app/docs/BACKLOG.md:893 | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| O261-E-F3-DORMENTE | ESPINHA-E | ? | LEU app/chamados/services/limbo_folgas.py:299: tratamento de um beco existe; matriz integral, estoque e demais becos de app/docs/BACKLOG.md:2077 [não verificado]; raiz explícita em app/docs/BACKLOG.md:893 | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| ACESA-ORFA-PROXIMIDADE | ESPINHA-E | SIM | LEU app/ponto/services/cartorio.py:628: órfãs entram nas horas reais; app/ponto/services/cartorio.py:666 acende por proximidade; app/ponto/services/cartorio.py:745 conserva órfãs na ata; INFERIU residual da mesma decisão de casamento tratada pela O118, app/ponto/services/cartorio.py:682 | O118 | DOBRA |

ESPINHA DA FAIXA 3: 41 linhas, CONTA 5, DOBRA 35, OBSOLETA 1, ? 8.

## 13/11 21:02 · sessao c5074bca9fb7 · - · - · origem=Ronald (tmux)

| id | balde | existe (SIM/NAO/PARCIAL/?) | prova (arquivo:linha do HEAD ou commit; LEU x INFERIU) | mae | veredito (CONTA/DOBRA/OBSOLETA) | [nao verificado]
|---|---|---|---|---|---|
| ADMIN-FECHA-SEM-MOTIVO | LATERAL-ESTRUTURA | SIM | LEU app/chamados/services/acoes_chamado.py:322: transição independe de conteúdo; app/chamados/models.py:452: resolução opcional. | - | CONTA |
| ABSOLVIDA-PELA-FOLGA | LATERAL-ESTRUTURA | PARCIAL | LEU app/chamados/services/disputa_emissao.py:387: nascimento consulta dia do turno; passivo dos 66 casos [não verificado]. Mãe explícita em app/docs/BACKLOG.md:1979. | FIO-E0-V4-E1-ENTRADAS | DOBRA |
| LOTE-VALIDAR-REGRESSAO-0311 | LATERAL-ESTRUTURA | ? | LEU app/chamados/views_cobrar.py:381: execução síncrona; causa e persistência da regressão de 03/11 [não verificado]. Mãe explícita em app/docs/BACKLOG.md:1980. | LOTE-VALIDAR-VIRA-JOB | DOBRA |
| PREVIEW-REOFERECE-RECUSADA | LATERAL-ESTRUTURA | SIM | LEU app/chamados/services/validacao.py:872: classificação não consulta recusa anterior; app/chamados/services/validacao.py:960: referências da classe A reapresentadas. Mãe explícita em app/docs/BACKLOG.md:1981. | LOTE-VALIDAR-VIRA-JOB | DOBRA |
| REVERTER-LEITOR-JANELAS | LATERAL-ESTRUTURA | SIM | LEU app/escala/services/regeneracao.py:58: exige desde/ate; app/ponto/portas/celula.py:851: volta grava dna/revertida. INFERIU raiz O99 pelo apply COL900 em app/docs/BACKLOG.md:1252. | O99 | DOBRA |
| REVERTER-SEM-SELECT-FOR-UPDATE | LATERAL-ESTRUTURA | SIM | LEU app/ponto/portas/celula.py:817: leitura sem trava; atomic somente em app/ponto/portas/celula.py:826. INFERIU raiz O99, app/docs/BACKLOG.md:1252. | O99 | DOBRA |
| REVERTER-VOLTA-TRILHA-NA-PORTA | LATERAL-ESTRUTURA | SIM | LEU app/ponto/portas/celula.py:851: trilha omite apesar_da_lavra; app/ponto/portas/celula.py:852: falha da trilha é capturada. INFERIU raiz O99, app/docs/BACKLOG.md:1252. | O99 | DOBRA |
| REVERTER-IDA-TRILHA-FRACA | LATERAL-ESTRUTURA | SIM | LEU app/ponto/management/commands/aplicar_col900.py:239: IDA abre guarda pelo motivo; app/ponto/management/commands/aplicar_col900.py:250: recomenda recalcular novamente. INFERIU raiz O99, app/docs/BACKLOG.md:1252. | O99 | DOBRA |
| REVERTER-ANTES-LOG-ANTES-DO-JSON | LATERAL-ESTRUTURA | SIM | LEU app/ponto/management/commands/aplicar_col900.py:79: evento antes de gravar JSON em app/ponto/management/commands/aplicar_col900.py:89. INFERIU raiz O99, app/docs/BACKLOG.md:1252. | O99 | DOBRA |
| PERGUNTA-VIVA-COM-CELULA-ABSOLVEU | LATERAL-ESTRUTURA | ? | LEU app/chamados/reconciliador.py:662: escritor pula pergunta ainda viva; situação atual da pk54831 [não verificado]. INFERIU raiz do programa pelo escopo de pergunta/célula em app/docs/BACKLOG.md:893. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS | LATERAL-ESTRUTURA | SIM | LEU app/chamados/services/materializacao.py:86: vizinha julgada para uma batida; app/chamados/services/materializacao.py:458: guarda aplicada ao plantio individual. | - | CONTA |
| BUG-PAR-ESTADO-EXPLICITO | LATERAL-ESTRUTURA | SIM | LEU app/chamados/services/materializacao.py:458: paridade ainda individual; absorção e substituição do estado gravado explicitadas em app/docs/BACKLOG.md:2022. | BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS | DOBRA |
| PLANTIO-SILENCIO-SEM-DIA | LATERAL-ESTRUTURA | SIM | LEU app/chamados/services/disputa_emissao.py:425: sem dia, apenas logger.info e continue em app/chamados/services/disputa_emissao.py:429. INFERIU raiz E1, via complemento BUG-PLANTIO em app/docs/BACKLOG.md:1978 e residual em app/docs/BACKLOG.md:2034. | FIO-E0-V4-E1-ENTRADAS | DOBRA |
| O10-TROCA-DE-PLANTAO-ENTRA | LATERAL-ESTRUTURA | PARCIAL | LEU app/ponto/portas/celula.py:670: porta construída; busca trocar_plantao em produção encontrou apenas definição. INFERIU falta de integração; escopo original em app/docs/BACKLOG.md:1263. | O10 | DOBRA |
| O10-B-FILA | LATERAL-ESTRUTURA | SIM | LEU app/ponto/portas/celula.py:608: guarda antes do atomic de app/ponto/portas/celula.py:630; resíduos de mesma porta em app/docs/BACKLOG.md:2040. INFERIU raiz O10, app/docs/BACKLOG.md:1263. | O10 | DOBRA |
| O10-D-PERMUTA-RECIPROCA | LATERAL-ESTRUTURA | SIM | LEU app/ponto/portas/celula.py:670: troca de um par; busca permutar_plantoes sem definição. INFERIU compositor de quatro células ausente; ampliação da O10 em app/docs/BACKLOG.md:2041. | O10 | DOBRA |
| E-SEM-VIVA-B | LATERAL-ESTRUTURA | NAO | LEU app/escala/services/cadastro_realidade.py:144: substituição explícita; app/escala/services/cadastro_realidade.py:169: desligados excluídos, ramo antigo removido. | LEI-DESLIGADO-NAO-TRAVA | OBSOLETA |
| H12H06-B-FILA-OPUS | LATERAL-ESTRUTURA | SIM | LEU app/ponto/services/credito_parcial.py:27 e app/ponto/services/efeito_ausencia.py:48: predicado repetido persiste. INFERIU raiz do conjunto A/B em app/docs/BACKLOG.md:1827. | H12H06-SEPARA-E-CRONTAB | DOBRA |
| G3G4-ESTRUTURA-RESIDUO | LATERAL-ESTRUTURA | ? | LEU app/docs/BACKLOG.md:2051: três casos sem sítio causal nomeado; pesquisa pelos casos não estabelece código responsável; persistência [não verificado]. | - | CONTA |
| C6-IDENTITY-MAP | LATERAL-ESTRUTURA | PARCIAL | LEU app/chamados/juizes.py:1169: Identity Map existente; app/escala/services/cadastro_realidade.py:456: alimentação em lote. Custo residual da lista e consultas citadas [não verificado]. | - | CONTA |
| F-346-FILA-OPUS | LATERAL-ESTRUTURA | PARCIAL | LEU app/ferias/services.py:434: trava do agendamento já existe; app/ferias/views.py:688: chamada captura só AgendamentoNaoRemarcavel, não ValidationError. INFERIU raiz HJ-090 pela mesma porta/saldo, app/docs/BACKLOG.md:1874. | HJ-090 | DOBRA |
| E4-8-ROTULO-OK | LATERAL-ESTRUTURA | SIM | LEU app/ponto/status_dia.py:31: rótulo ainda afirma todas as batidas registradas; app/ponto/status_dia.py:289: rótulo gravado prevalece. INFERIU raiz de unificação do dia, app/docs/BACKLOG.md:893. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| FUROS-EMPRESA-TIMEOUT | LATERAL-ESTRUTURA | ? | LEU app/relatorios/furos.py:126: laço síncrono; app/relatorios/furos.py:160: contexto por colaborador. Tempo atual e ocorrência do timeout [não verificado]. | - | CONTA |
| CICLO-C-CEL-TEMPLATE | LATERAL-ESTRUTURA | ? | LEU app/ponto/portas/celula.py:310: escrita Cel; app/ponto/management/commands/gerar_celulas.py:264: renovação de chamado; app/chamados/models.py:290: trava C no fechamento. INFERIU ciclo possível; reprodução [não verificado]. | - | CONTA |
| FAIXA-NO-CADASTRO | LATERAL-ESTRUTURA | SIM | LEU app/ponto/precedencia.py:52: mapa de tipos fixo; app/ponto/precedencia.py:79: faixa sai desse mapa. INFERIU mesma obra de eliminar default da faixa, app/docs/BACKLOG.md:2081. | LASTRO-FAIXA-SEM-DEFAULT | DOBRA |
| EFEITO-SEM-FALLBACK-CATALOGO | LATERAL-ESTRUTURA | SIM | LEU app/ponto/catalogo/ausencias.py:253: exceção vira None; app/ponto/catalogo/ausencias.py:278: substituição silenciosa pelo catálogo congelado. | - | CONTA |
| LASTRO-FILA-PREVERIF-C11 | LATERAL-ESTRUTURA | SIM | LEU app/escala/utils.py:1200: falha na leitura vira conjunto vazio; app/api/views.py:1584: leitor conserva só status. INFERIU residual da obra LASTRO pela origem e escopo em app/docs/BACKLOG.md:2083. | LASTRO-FAIXA-SEM-DEFAULT | DOBRA |
| O261-PAI-TERMINAL | LATERAL-ESTRUTURA | PARCIAL | LEU app/chamados/services/fio.py:117: fallback continua no pai; app/chamados/services/disputa_emissao.py:566: disputa reutilizada retorna sem reparo por pergunta/dia. Desenho antigo vedado em app/chamados/models.py:380; raiz do programa em app/docs/BACKLOG.md:893. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| O10-C | LATERAL-ESTRUTURA | SIM | LEU app/ponto/portas/celula.py:670: porta existente; busca de chamadores produtivos encontrou só definição. INFERIU integração histórica ainda faltante; raiz em app/docs/BACKLOG.md:1263. | O10 | DOBRA |
| TIPO-NA-FONTE-LAMPADA-INSTANTE | LATERAL-ESTRUTURA | SIM | LEU app/ponto/management/commands/gerar_celulas.py:69: combine sem fuso. Absorção DNA em app/docs/BACKLOG.md:2090; raiz ata/dna em app/docs/BACKLOG.md:1836. | TIPO-NA-FONTE | DOBRA |
| RELOGIO-NA-BORDA | LATERAL-ESTRUTURA | SIM | LEU app/escala/signals.py:67 e app/escala/signals.py:119: relógios interiores; app/ponto/portas/celula.py:464: nova leitura no fluxo. INFERIU raiz Clock Injection em app/docs/BACKLOG.md:1057. | RELOGIO-INJETADO | DOBRA |
| RELOGIO-NA-BORDA-OPUS-C5E | LATERAL-ESTRUTURA | SIM | LEU app/escala/signals.py:67: relógio no update; app/ponto/portas/celula.py:475: exceção dentro do fluxo. Complemento explícito em app/docs/BACKLOG.md:2092; INFERIU raiz app/docs/BACKLOG.md:1057. | RELOGIO-INJETADO | DOBRA |
| VIGIA-PAREADOR-DIA-CIVIL | LATERAL-ESTRUTURA | SIM | LEU app/ponto/services/vigia_pareador_celula.py:109: órfã buscada pelo dia civil apesar de turno indexado em app/ponto/services/vigia_pareador_celula.py:101. | - | CONTA |
| TIPO-ATA-12-CHAVES-A | LATERAL-ESTRUTURA | SIM | LEU app/ponto/services/cartorio.py:451 e app/ponto/services/lastro.py:149: acessos crus à ata; acessor escala/services/ata.py não localizado. Programa raiz em app/docs/BACKLOG.md:1836. | TIPO-NA-FONTE | DOBRA |
| TIPO-ATA-12-CHAVES-B | LATERAL-ESTRUTURA | SIM | LEU app/ponto/services/lastro.py:148: leitor continua usando hora/tipo; acessor tipado não localizado. INFERIU migração de instantes ainda não entregue; programa em app/docs/BACKLOG.md:1836 e app/docs/BACKLOG.md:2095. | TIPO-NA-FONTE | DOBRA |
| TIPO-DNA-9-CHAVES | LATERAL-ESTRUTURA | SIM | LEU app/escala/utils.py:153: leitura crua das lâmpadas DNA; app/ponto/management/commands/gerar_celulas.py:69: instante sem fuso. Raiz ata/dna em app/docs/BACKLOG.md:1836. | TIPO-NA-FONTE | DOBRA |
| TRANCA-VIVAS-08-FILA | LATERAL-ESTRUTURA | PARCIAL | LEU app/chamados/services/arquivo.py:181: documentação já reconhece colaborador; app/chamados/services/materializacao.py:725: resposta carimba resgate. Completude do placar e medição pós-cura [não verificado]; residual descrito em app/docs/BACKLOG.md:2098. | - | CONTA |
| E3-PENDENTE-NAO-BOOL | LATERAL-ESTRUTURA | SIM | LEU app/ponto/services/lastro.py:150: pendente julgado por truthiness. Integração ao pouso A explícita em app/docs/BACKLOG.md:2100; raiz em app/docs/BACKLOG.md:1836. | TIPO-NA-FONTE | DOBRA |
| SETNULL-ORFA-DNA | LATERAL-ESTRUTURA | SIM | LEU app/escala/models.py:1693: SET_NULL; app/ponto/portas/celula.py:252: limpeza exige geradora igual ao vínculo. INFERIU falha latente após exclusão; ocorrência em produção [não verificado]. | - | CONTA |
| TIPO-ITEM3-CONTRACT | LATERAL-ESTRUTURA | SIM | LEU app/ponto/services/lastro.py:149: ata crua e tolerância a ausência continuam; acessor estrito não localizado. Fase CONTRACT e raiz em app/docs/BACKLOG.md:2103 e app/docs/BACKLOG.md:1836. | TIPO-NA-FONTE | DOBRA |
| TIPO-V5-NAO-BLOQUEANTES | LATERAL-ESTRUTURA | SIM | LEU app/ponto/management/commands/tripwire_celulas.py:85 e app/ponto/management/commands/tripwire_celulas.py:247: relógios internos; app/ponto/services/vigia_pareador_celula.py:109: mesmo resíduo civil. INFERIU dobra pela sobreposição de sítio. | VIGIA-PAREADOR-DIA-CIVIL | DOBRA |
| CONTRATO-DE-ENTRADA-DO-FIO | LATERAL-ESTRUTURA | SIM | LEU app/ponto/management/commands/marcar_foto_ausente_retro.py:100: emissor grava foto_ausente.batida_id; app/chamados/services/disputa_emissao.py:387: leitor procura ultima_batida.id. INFERIU raiz do nascimento E1 em app/docs/BACKLOG.md:1978 e app/docs/BACKLOG.md:2106. | FIO-E0-V4-E1-ENTRADAS | DOBRA |

TRECHO C: 42 linhas, CONTA 10, DOBRA 31, OBSOLETA 1, ? 5.

## 13/11 21:02 · sessao 4e846a8c807a · - · - · origem=Ronald (tmux)

| id | balde | existe | prova | mae | veredito |
|---|---|---|---|---|---|
| O261-E-CELULA-20-UM-DONO | ESPINHA-E | PARCIAL | LEU app/chamados/models.py:374: gate por pergunta/dia implementado; app/chamados/services/fio.py:117 ainda retorna âncora sem exigir estado vivo; INFERIU residual. Raiz explícita app/docs/BACKLOG.md:893. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| O261-E-PAI-TERMINAL-RESIDUAL | ESPINHA-E | SIM | LEU app/chamados/services/fio.py:117: âncora pelo destino/container, sem filtro de estado; app/chamados/models.py:374: há tratamento por pergunta no gate, não no leitor da âncora. INFERIU residual; app/docs/BACKLOG.md:894. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| O261-E-F5B-JUIZ-PORTA-ALCANCAVEL | ESPINHA-E | NAO | LEU app/chamados/juizes.py:1653: juiz implementado; app/core/juizes.py:293: registrado; app/folha/fora_da_folha.py:66 e app/chamados/services/vigia_fio.py:640: leitores delegam. INFERIU cura do contrato indicado. | PROGRAMA-CELULA-E-O-DIA | OBSOLETA |
| O261-E-F5C-PERGUNTA-SEM-ACAO-POSSIVEL | ESPINHA-E | SIM | LEU app/chamados/services/raiox.py:153: predicado do clique morto permanece separado; busca no registro não encontrou essa função [não verificado: registro completo]. app/docs/BACKLOG.md:896 explicitamente distingue esse predicado da porta do dia. | - | CONTA |
| O261-E-F5C-FILA-DO-OPUS | ESPINHA-E | SIM | LEU app/escala/services/cadastro_realidade.py:224: `_jornada_que_mente` ainda lê `date.today()`; INFERIU ao menos esse residual da lista. Parte explícita app/docs/BACKLOG.md:897. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| E1-PERGUNTA-AO-DP-CONTADOR | ESPINHA-E | ? | LEU app/docs/BACKLOG.md:898: três perguntas ao DP; nenhum arquivo de pergunta/resposta localizado na busca pela obra; sem sítio e resposta não fornecida [não verificado]. | - | CONTA |
| E1-STATUS-VERMELHOS | ESPINHA-E | ? | LEU app/docs/BACKLOG.md:899: lista S02…L10; app/ponto/status_dia.py:166: implementação atual dos fatos. Sem resultado atual dos casos/leitores no dado, não confirma persistência de cada reprovação [não verificado]. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| V-FASE-AS-OF-CADASTRO-SEM-LINHA | ESPINHA-E | SIM | LEU app/escala/services/cadastro_realidade.py:393: C10 lê apenas `itens`; app/escala/services/fase_12x36.py:144: dono as-of considera também `itens_reabertos`. INFERIU diferença de universos ainda presente; app/docs/BACKLOG.md:903. | O261-V-FASE-NA-LISTA | DOBRA |
| O261-E-F1C4-LEN-VIVOS | ESPINHA-E | PARCIAL | LEU app/ponto/management/commands/supra_juiz.py:249: retratador lê lavra; app/ponto/management/commands/supra_juiz.py:223: placar/alarme ainda recalcula `len(vivos)`, contra `_protestam` em app/ponto/services/cartorio.py:1466. INFERIU cura parcial. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| LENVIVOS-A6-ALARME | ESPINHA-E | SIM | LEU app/ponto/management/commands/supra_juiz.py:223 e :305: A6 consome placar derivado; app/ponto/services/cartorio.py:1466 usa outro universo. INFERIU defeito do leitor; residual explícito app/docs/BACKLOG.md:906. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| O261-E-F1C4-MEMO-C6 | ESPINHA-E | ? | LEU app/chamados/juizes.py:851: chave não inclui ata/DNA; app/chamados/models.py:3222 invalida memo em porta existente. Caminho real que altera ata conservando memo não demonstrado pelo dado [não verificado]; app/docs/BACKLOG.md:912. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| MEMO-C6-BATIDAS-PREFETCH | ESPINHA-E | ? | LEU app/ponto/services/cartorio.py:128: prefetch de perguntas; app/ponto/services/cartorio.py:1389: leitura posterior do fio. Ordem efetiva de commit/alteração das batidas não demonstrada [não verificado]; hipótese em app/docs/BACKLOG.md:913. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| F1C4-3-NONE-COBRA | ESPINHA-E | ? | LEU app/chamados/juizes.py:454: `None` não equivale a folga, cura presente; app/docs/BACKLOG.md:922 inclui 12 reabríveis. Estado atual desse passivo não fornecido [não verificado]. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| F1C4-3-NONE-LEITORES | ESPINHA-E | SIM | LEU app/ponto/services/flip_auto.py:212: leitor ainda cala `trabalha is not True`; INFERIU leitor residual da mesma classe. Relação com cura de `None` em app/docs/BACKLOG.md:923. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| CARTORIO-PROTESTA-1A-CLAUSULA | ESPINHA-E | SIM | LEU app/ponto/services/cartorio.py:1459: `_protesta` decide pela primeira cláusula própria; app/chamados/juizes.py:454: juiz tem tratamento adicional da ata/parcial. INFERIU divergência mantida; app/docs/BACKLOG.md:925. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| E3-GRADE-SUBTIPO-FOLGA | ESPINHA-E | SIM | LEU app/escala/utils.py:1220: subtipo vira `tipo_dia='ausencia'`; app/ponto/status_dia.py:189: fatos do status usam `tipo_dia`, sem `ausencia_tipo`. INFERIU perda do subtipo nesse percurso; app/docs/BACKLOG.md:927. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| E3-HASH-A2 | ESPINHA-E | SIM | LEU app/ponto/services/cartorio.py:480: composição do hash não inclui Feriado/FolgaDia; app/ponto/status_dia.py:181: esses fatos participam do status. INFERIU lacuna persistente; E2 explícita app/docs/BACKLOG.md:929. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| EMIS-LEITORES-RESTANTES | ESPINHA-E | SIM | LEU app/ponto/services/flip_auto.py:212: predicado próprio `trabalha is not True`; app/chamados/services/declaracao_texto.py:119: predicado próprio por `tipo_dia`. INFERIU leitores ainda não unificados; app/docs/BACKLOG.md:930. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| BUG-ENC-CELULAS-FORMA | ESPINHA-E | SIM | LEU app/ponto/services/cartorio.py:1154: entrega mapa com chave `(colab,data)`; app/ponto/turnos.py:2150: repassa diretamente a `turnos_do_colab`. INFERIU contrato defeituoso ainda presente; app/docs/BACKLOG.md:933. | - | CONTA |
| E4-NAO-LAVRADO-PASSADO | ESPINHA-E | PARCIAL | LEU app/ponto/status_dia.py:315: encerrado sem lavra normalmente vira `sem_certidao`, mas ramo `sem_fim` ainda devolve `previsto`; app/templates/core/_painel_card.html:67 imprime esse status após “enc.”. INFERIU residual desse ramo. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| E3-STATUS-EXPORTADA | ESPINHA-E | ? | LEU app/ponto/services/cartorio.py:934: porta para completar status existe; app/docs/BACKLOG.md:942 pede aplicação na 09 e hashes preservados. Estoque/aplicação/hashes atuais não fornecidos [não verificado]. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| CODS-DO-ATO | ESPINHA-E | NAO | LEU app/ponto/services/cartorio.py:981: retira `cods` do lote antes do julgamento; app/ponto/services/cartorio.py:1116: recupera saída do ato e restaura acumulador do lote. INFERIU cura do vazamento por data entre colaboradores. | - | OBSOLETA |
| MODO-COMUM-OUT-DO-LOTE | ESPINHA-E | ? | LEU app/ponto/services/cartorio.py:1108: limpa filas do ato no `finally`; app/ponto/services/cartorio.py:1951 declara suas chaves. Prova posterior do comando exigida em app/docs/BACKLOG.md:946 não está no dado [não verificado]. | - | CONTA |
| BACKFILL-UNICO | ESPINHA-E | ? | LEU app/docs/BACKLOG.md:947: mandato de centralização e destino; nenhum arquivo da porta central localizado pelo nome da obra. Sem sítio implementado identificável [não verificado]. | - | CONTA |
| E3-ISENTO-INSUMOS | ESPINHA-E | NAO | LEU app/ponto/services/cartorio.py:548: isenção entra no hash dentro do horizonte vigente; app/docs/BACKLOG.md:954 declara substituição pela regra H10. INFERIU cura da exigência vigente, preservando horizonte. | PROGRAMA-CELULA-E-O-DIA | OBSOLETA |
| ISENTO-PONTO-JUIZ-REGISTRADO | ESPINHA-E | SIM | LEU app/core/constants.py:13: autoridade existe; app/ponto/services/cartorio.py:1272 e :1470 ainda leem diretamente `c.isento_ponto`. INFERIU consultas paralelas ainda existentes; app/docs/BACKLOG.md:955. | - | CONTA |
| E3-L02-PENDENTE | ESPINHA-E | SIM | LEU app/ponto/status_dia.py:189: fatos não recebem lista `dias_cadastro_x_realidade`; app/ponto/motor_calculo_v2.py:289: lista existe na saída do motor. INFERIU ligação ainda ausente; app/docs/BACKLOG.md:958. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| F1C4-3-EMISSORES-FILA | ESPINHA-E | PARCIAL | LEU app/chamados/juizes.py:454: parcial tratada pelo juiz; app/ponto/services/flip_auto.py:212 e app/chamados/services/declaracao_texto.py:119 mantêm predicados próprios. INFERIU resíduos de leitores; lista app/docs/BACKLOG.md:975. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| E3-OPUS-FILA | ESPINHA-E | PARCIAL | LEU app/ponto/services/cartorio.py:927: status preenchido é pulado; app/ponto/services/cartorio.py:480: A2 ausente do hash; app/ponto/status_dia.py:181: status consulta A2. INFERIU ao menos residual de status envelhecido; lista app/docs/BACKLOG.md:977. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| F1C4-FILA-OPUS | ESPINHA-E | PARCIAL | LEU app/chamados/models.py:2722: portas ganharam ordem de travas; app/chamados/catalogo/motor.py:96 e :117 ainda mantêm SUPERAR citado na lista. INFERIU resíduos mistos; demais itens sem prova integral [não verificado]; app/docs/BACKLOG.md:983. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| E6-POR-DONO | ESPINHA-E | ? | LEU app/ponto/management/commands/e6_oraculo.py:427: saída já totaliza por dono; app/docs/BACKLOG.md:984 exige medida depois do pouso F1C4. Execução e resultado atual não fornecidos [não verificado]. | PLACAR-ESTRUTURAL | DOBRA |
| RESPONDIDA-SEM-TEXTO-DUAS-COPIAS | ESPINHA-E | SIM | LEU app/chamados/juizes.py:1417: C6 mantém expressão própria de resposta com texto; app/chamados/juizes.py:1528: helper separado; app/chamados/services/vigia_fio.py:776 importa helper. INFERIU duplicação semântica ainda presente. | - | CONTA |
| E2-D3-TRABALHA-FORA-DA-IMPRESSAO | ESPINHA-E | SIM | LEU app/ponto/services/cartorio.py:480: hash não recebe `CelulaDia.trabalha` diretamente; app/ponto/services/cartorio.py:1465: julgamento recebe esse campo. INFERIU insumo fora da impressão; parte E2 app/docs/BACKLOG.md:990. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| E2-D3-VINCULO-NA-IMPRESSAO | ESPINHA-E | SIM | LEU app/ponto/services/cartorio.py:471: assinatura/hash sem versão do vínculo vigente; app/ponto/services/cartorio.py:484: DNA entra apenas por versão/marcos. INFERIU troca de vínculo não representada integralmente; app/docs/BACKLOG.md:991. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| E2-D3-FERIADO-NA-IMPRESSAO | ESPINHA-E | SIM | LEU app/ponto/services/cartorio.py:480: feriado ausente do hash; app/chamados/signals.py:326: save do feriado chama somente reavaliação de perguntas em :336. INFERIU lacuna do rejulgamento mantida; app/docs/BACKLOG.md:992. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| E2-D3-CCT-NA-IMPRESSAO | ESPINHA-E | SIM | LEU app/ponto/services/cartorio.py:499: hash recebe versão de regra do cartório, não identidade/vigência da CCT; composição em :480 não inclui CCT. INFERIU lacuna persistente; app/docs/BACKLOG.md:993. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| E2-D3-ISENCAO-NA-IMPRESSAO | ESPINHA-E | PARCIAL | LEU app/ponto/services/cartorio.py:548: flag entrou no hash para regra ≥4; app/chamados/signals.py:270: save do colaborador apenas agenda silêncio em :280. INFERIU ausência de rejulgamento imediato por essa entrada; app/docs/BACKLOG.md:994. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| E2-D3-JUSTIFICATIVA-APROVADA | ESPINHA-E | SIM | LEU app/ponto/signals.py:165: signal pede julgamento sem forçar; app/ponto/services/cartorio.py:480: hash não inclui justificativa aprovada. INFERIU possibilidade de pulo com hash igual; app/docs/BACKLOG.md:995. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| E2-D3-BATIDA-DA-VESPERA | ESPINHA-E | SIM | LEU app/ponto/services/cartorio.py:474: hash do dia lê batidas da véspera; app/ponto/signals.py:112: batida julga apenas seu dia civil e o anterior. INFERIU dia seguinte permanece sem gatilho; app/docs/BACKLOG.md:996. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| E2-D3-EDICAO-HUMANA-SEM-PORTA | ESPINHA-E | NAO | LEU app/ponto/portas/celula.py:572: porta `editar_dia`; :639 e :645 gravam autor/data; :713: chamada de produção por troca de plantão. INFERIU ausência de escritor curada. | PROGRAMA-CELULA-E-O-DIA | OBSOLETA |
| O261-FIO-MORTO-PELO-RETRATO | ESPINHA-E | ? | LEU app/chamados/juizes.py:1622: juiz da porta e censo em app/chamados/services/vigia_fio.py:640 existem; app/docs/BACKLOG.md:998 absorve resíduos na E2. Estoque atual dos casos não fornecido [não verificado]. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| DONO-NAO-VE-FASE-DESLOCADA | ESPINHA-E | NAO | LEU app/ponto/management/commands/e6_oraculo.py:108: fase deslocada recebe CADASTRO; :178 e :186 carregam dias as-of e passam insumo ao juiz. INFERIU cura do defeito de atribuição descrito. | O261-V-FASE-NA-LISTA | OBSOLETA |
| VETO-IMPRESSAO-NUNCA-BATEU | ESPINHA-E | SIM | LEU app/ponto/services/cartorio.py:1257: impressão de nunca bateu recebe regra/parcial/isento, sem `veto=`; parâmetro existe em :471. INFERIU omissão persistente. | O261-VETO-NA-PORTA | DOBRA |
| VETO-TRILHA-CICLO | ESPINHA-E | NAO | LEU app/chamados/models.py:1219: consulta última trilha; :1225 e :1228 verificam passagem posterior; só cala sem mudança em :1229. INFERIU ciclo novo volta a registrar. | O261-VETO-NA-PORTA | OBSOLETA |
| VETO-ATO-HUMANO-MOSTRA-E-TRILHA | ESPINHA-E | SIM | LEU app/chamados/models.py:1384: `humano=True` evita consulta do veto nessa guarda; ato não recebe seu motivo para exibição/trilha. INFERIU residual; fatia B explícita app/docs/BACKLOG.md:1016. | O261-VETO-NA-PORTA | DOBRA |
| DIA-DA-PERGUNTA-E-O-TURNO | ESPINHA-E | SIM | LEU app/chamados/services/disputa_emissao.py:1205: dia vem de `resolver_data_evento`; :1232: outro ramo lê contexto do chamado; :451: terceiro caminho fornece dia próprio. INFERIU entradas ainda distintas, sem escritor único pelo juiz do turno. | - | CONTA |
| O261-VETO-NA-PORTA | ESPINHA-E | PARCIAL | LEU app/chamados/models.py:1385: veto automático na porta; :1384: ato humano pula essa leitura, mantendo residual de mostrar/registrar veto. INFERIU cura parcial; B/C app/docs/BACKLOG.md:1026. | - | CONTA |
| CALENDARIO-HOJE-NAO-E-FALTA | ESPINHA-E | NAO | LEU app/colaboradores/services/calendario.py:602 e :606: calendário delega ao D4; app/ponto/status_dia.py:315: dia em curso fica previsto, não falta. INFERIU cura do julgamento prematuro descrito. | PROGRAMA-CELULA-E-O-DIA | OBSOLETA |
| O121-A1-VINCULO-CORRIGE-O-PASSADO | ESPINHA-E | SIM | LEU app/colaboradores/services/vinculo.py:72: piso ainda barra retroação; app/ponto/portas/celula.py:221: guarda de exportada permanece sem motivo de abertura. INFERIU bloqueios ainda contrários à A1 descrita em app/docs/BACKLOG.md:1228. | O121 | DOBRA |

Trecho B: 49 linhas; existe SIM 24, PARCIAL 8, NAO 7, ? 10. Vereditos: CONTA 10, DOBRA 32, OBSOLETA 7.

## 13/11 21:02 · sessao 39b2471e749d · - · - · origem=Ronald (tmux)

| id | balde | existe (SIM/NAO/PARCIAL/?) | prova (arquivo:linha do HEAD ou commit; LEU x INFERIU) | mae | veredito (CONTA/DOBRA/OBSOLETA) | [nao verificado]
|---|---|---|---|---|---|
| RELOGIO-INJETADO | ESPINHA-E | SIM | LEU app/ponto/services/dias_em_aberto.py:89 e app/ponto/services/veredito_celula.py:220: relógio interno; INFERIU complemento pelo app/docs/BACKLOG.md:1057 | JUIZ-DO-PAR-DUAS-ENTRADAS | DOBRA |
| GRUPO-1-DATA-SAIDA-ESPERADA | ESPINHA-E | NAO | LEU app/escala/utils.py:945 e app/escala/servico_jornada.py:208: função apagada; pesquisa não encontrou definição sobrevivente | JUIZ-DO-PAR-DUAS-ENTRADAS | OBSOLETA |
| FIO-MORTO-SEM-REPARADOR | ESPINHA-E | ? | LEU app/chamados/services/vigia_fio.py:318: censo existe; falta medição dos casos por caminho no dado fornecido [não verificado]; INFERIU vínculo com obra ampla | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| E2-EMISSORES-FATOS-DO-DIA | ESPINHA-E | SIM | LEU app/ponto/management/commands/detectar_ausencias.py:617: veto continua limitado à faixa feriado; INFERIU complemento pelo app/docs/BACKLOG.md:1073 | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| CHAVE-DE-ENTRADA-INCOMPLETA | ESPINHA-E | ? | LEU app/ponto/services/cartorio.py:471: impressão existe; caso medido de célula velha exigido por app/docs/BACKLOG.md:1066 ausente no dado [não verificado] | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| GRUPO-1-LISTA-CODEX-T11 | ESPINHA-E | SIM | LEU app/ponto/turnos.py:2409 e app/ponto/services/veredito_celula.py:286: casamento e vizinhança próprios sobrevivem; INFERIU complemento pelo app/docs/BACKLOG.md:1072 | JUIZ-DO-PAR-DUAS-ENTRADAS | DOBRA |
| CONTRATO-20-E2-QUAL-ATO | ESPINHA-E | PARCIAL | LEU app/colaboradores/services/vinculo.py:902: evento pela regeneração; app/ponto/management/commands/detectar_ausencias.py:617: emissor ainda parcial; INFERIU raiz pelo app/docs/BACKLOG.md:821 | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| EXPIRACAO-DA-ABERTA-COPIA-ANTIGA | ESPINHA-E | SIM | LEU app/ponto/selecao_periodo.py:283 e app/ponto/selecao_periodo.py:290: jornada e margem calculadas localmente; INFERIU mesma autoridade de encerramento | O248 | DOBRA |
| JUIZ-PERGUNTA-VIVA-MUDA-SEM-FATO | ESPINHA-E | PARCIAL | LEU app/chamados/services/validacao.py:448: carimbo e devolução atômicos; app/chamados/services/validacao.py:492: materialização ainda recebe disputa do objeto; passivo [não verificado] | - | CONTA |
| JUIZ-DE-DONO-DO-DIA | ESPINHA-E | ? | LEU app/ponto/management/commands/e6_oraculo.py:53 e app/chamados/services/vigia_fio.py:274: autoridades atuais existem; alcance das perguntas nominais não fornecido [não verificado] | - | CONTA |
| GATE10-ESTRUTURA-EM-4-RUBRICAS-DA-10 | ESPINHA-E | ? | LEU app/ponto/motor_calculo_v2.py:334: cálculo permanece; divergências atuais das quatro rubricas e predicado não demonstrados pelo dado [não verificado]; relação app/docs/BACKLOG.md:1082 | R1-ESTRUTURA-91 | DOBRA |
| ESMERIL familia (2) batida | ESPINHA-E | NAO | LEU/EXECUTOU censo AST somente leitura de bin/esmeril_batida_censo.py:228; contadores de bin/esmeril_batida_censo.py:269 e bin/esmeril_batida_censo.py:270: escrita fora=0, leitura crua apuração=0 | - | OBSOLETA |
| ABONO-NO-AR-NAO-FECHOU | ESPINHA-E | ? | LEU app/ponto/services/dia_pago.py:405: retrato da exportada implementado; condição numérica da competência 10 não está no dado [não verificado]; INFERIU fatia de app/docs/BACKLOG.md:1131 | O-DIA-PAGO | DOBRA |
| VINCULO-LINHA-DO-TEMPO | ESPINHA-E | PARCIAL | LEU app/colaboradores/services/vinculo.py:253: absorve posteriores; app/escala/models.py:1037: só constraint fim/início, sem exclusão de sobreposição | - | CONTA |
| O-DIA-PAGO | ESPINHA-E | PARCIAL | LEU app/ponto/services/dia_pago.py:346: lavratura presente; app/relatorios/pdf_espelho.py:463 e app/folha/export.py:179: leitores ainda derivam falta; INFERIU alcance de app/docs/BACKLOG.md:1131 | - | CONTA |
| E4-CALENDARIO | ESPINHA-E | NAO | LEU app/colaboradores/services/calendario.py:25 e app/colaboradores/services/calendario.py:652: dinheiro lido do DiaPago; INFERIU absorção explícita app/docs/BACKLOG.md:1131 | O-DIA-PAGO | OBSOLETA |
| E6-14 | ESPINHA-E | ? | LEU app/ponto/management/commands/e6_oraculo.py:53: instrumento atual; situação dos 14 não fornecida [não verificado]; INFERIU parte da certificação app/docs/BACKLOG.md:1338 | O54 | DOBRA |
| PLACAR-ESTRUTURAL | ESPINHA-E | PARCIAL | LEU app/folha/porta_export.py:313: medição presente; app/core/juizes.py:495: pendências estruturais ainda declaradas e apontadas; INFERIU residual de contratos | - | CONTA |
| L111-ORFAOS-DO-MOTOR | ESPINHA-E | SIM | LEU app/ponto/calculador/regras.py:302 e app/ponto/motor_calculo_v2.py:334: ambas funções sobrevivem | - | CONTA |
| O219 | ESPINHA-E | ? | LEU app/ponto/services/cartorio.py:142 e app/ponto/services/cartorio.py:499: versão/horizonte na impressão; TXT novo e pauta exigidos em app/docs/BACKLOG.md:1197 não demonstrados no dado [não verificado] | - | CONTA |
| O228 | ESPINHA-E | PARCIAL | LEU app/ponto/precedencia.py:151: remoções históricas; censo AST atual de bin/esmeril_turno_censo.py:258 identifica app/ponto/turnos.py:2102 sem chamador | - | CONTA |
| O222 | ESPINHA-E | NAO | LEU app/ponto/services/cartorio.py:1085: mesmo corpo relê após emissão; app/ponto/tests/test_bateria_lavratura.py:1181: caso da primeira passada | - | OBSOLETA |
| O218 | ESPINHA-E | ? | LEU app/ponto/tests/test_bateria_lavratura.py:169 e app/ponto/tests/test_bateria_lavratura.py:1037: bateria construída; relavra/prova dos dias de col146 ausentes no dado [não verificado] | - | CONTA |
| O221 | ESPINHA-E | ? | LEU app/ponto/services/cartorio.py:940: corpo integrado presente; sem git e sem prova fornecida dos três pousos, não certifica junção completa [não verificado] | - | CONTA |
| O129 | ESPINHA-E | SIM | LEU app/core/configuracao_efeito.py:39: DECLARACAO não inclui TipoEscala; censo textual/AST da entidade confirma ausência; INFERIU falta pedida em app/docs/BACKLOG.md:1220 | - | CONTA |
| O140 | ESPINHA-E | PARCIAL | LEU app/ponto/services/dia_pago.py:346 e app/ponto/services/dia_pago.py:592: campo/escritor/backfill; app/folha/export.py:179: complemento BUG-8792 ainda lê ausência | O-DIA-PAGO | DOBRA |
| BUG-8792 | ESPINHA-E | SIM | LEU app/folha/export.py:179 e app/folha/export.py:181: catálogo + Ausencia, sem ler furo lavrado; INFERIU complemento explícito app/docs/BACKLOG.md:1225 e raiz app/docs/BACKLOG.md:1131 | O-DIA-PAGO | DOBRA |
| O125 | ESPINHA-E | ? | LEU app/chamados/services/vigia_fio.py:274: dono/lista são consultados; alcance atual dos sete casos não consta no dado [não verificado]; INFERIU residual R2 pelo app/docs/BACKLOG.md:1232 | PLACAR-ESTRUTURAL | DOBRA |
| O117 | ESPINHA-E | ? | LEU app/escala/utils.py:2070: previsto ainda recebe realizado no intermitente; universo atual de trabalho sem marcos não foi fornecido [não verificado] | - | CONTA |
| O108 | ESPINHA-E | NAO | LEU app/ponto/services/espelho.py:1043 e app/ponto/services/espelho.py:1070: previstas, vínculos do período e efeitos da ausência alimentam a autoridade | O54 | OBSOLETA |
| O105 | ESPINHA-E | SIM | LEU app/folha/export.py:740: bloco de falta ainda deriva datas no ato; app/folha/export.py:169: fonte consulta ausências | - | CONTA |
| O9 | ESPINHA-E | PARCIAL | LEU app/relatorios/pdf_espelho.py:596: dias vêm do espelho; app/relatorios/pdf_espelho.py:463: resumo/furo ainda derivados no coletor; INFERIU parte dos leitores do programa app/docs/BACKLOG.md:1338 | O54 | DOBRA |
| PREVISTO-EM-DUAS-[nome] | ESPINHA-E | SIM | LEU app/escala/utils.py:2070: grade sobrescreve previsto com realizado; app/ponto/services/dia_pago.py:287: lavratura recebe mapa da grade; INFERIU defeito da persistência diária | O-DIA-PAGO | DOBRA |
| O14 | ESPINHA-E | NAO | LEU app/api/views.py:1431 e app/api/views.py:2023: dois leitores usam dia_das_batidas; INFERIU parte dos leitores app/docs/BACKLOG.md:1337 e app/docs/BACKLOG.md:1338 | O54 | OBSOLETA |
| O27 | ESPINHA-E | SIM | LEU app/relatorios/pdf_espelho.py:627: totais vêm de folha_manda, enquanto dias são do espelho em app/relatorios/pdf_espelho.py:603; INFERIU mesmo defeito da soma O156 | O156 | DOBRA |
| O33 | ESPINHA-E | SIM | LEU app/relatorios/pdf_espelho.py:627: badges não são soma das linhas desenhadas; INFERIU identidade de decisão explicitada em app/docs/BACKLOG.md:1314 | O156 | DOBRA |
| O35 | ESPINHA-E | PARCIAL | LEU app/core/contratos_estruturais.py:92: matriz implementada; app/core/juizes.py:495: contrato de fechamento ainda tem pendências; INFERIU parte R6 em app/docs/BACKLOG.md:1173 | PLACAR-ESTRUTURAL | DOBRA |
| O37 | ESPINHA-E | ? | LEU app/escala/models.py:1365: fora da foto agora continua paridade, não inventa trabalho; passivo exportado exigido em app/docs/BACKLOG.md:1318 não demonstrado [não verificado]; raiz app/docs/BACKLOG.md:1338 | O54 | DOBRA |
| O40 | ESPINHA-E | PARCIAL | LEU app/core/juizes.py:305 e app/core/juizes.py:322: autoridades distintas para competência e tranca; sem juiz único demonstrado para a fronteira FM/PeriodoFechado; INFERIU residual app/docs/BACKLOG.md:1321 | O44 | DOBRA |
| O44 | ESPINHA-E | ? | LEU app/chamados/services/cobrar_dia.py:40: toque trilha antes e chama cartório com toque=True; smoke dos seis critérios e caso nominal não demonstrados [não verificado] | - | CONTA |
| O45 | ESPINHA-E | NAO | LEU app/ponto/services/espelho.py:1249 e app/ponto/services/espelho.py:1285: tela alimenta/emite datas em aberto pela fonte comum; PDF lê tela em app/relatorios/pdf_espelho.py:598 | O54 | OBSOLETA |
| O48 | ESPINHA-E | PARCIAL | LEU app/ponto/services/fechamento.py:1083: materialização ainda escreve derivados; app/folha/export.py:740: TXT ainda deriva falta; raiz explícita app/docs/BACKLOG.md:1337 e app/docs/BACKLOG.md:1338 | O54 | DOBRA |
| O49 | ESPINHA-E | SIM | LEU app/ponto/motor_calculo_v2.py:822: intervalo_indenizavel segue deliberadamente sem consumo; raiz explícita app/docs/BACKLOG.md:1338 | O54 | DOBRA |
| O50 | ESPINHA-E | PARCIAL | LEU app/colaboradores/services/vinculo.py:548 e app/escala/models.py:1038: validação/constraint existem; app/colaboradores/services/vinculo.py:858: variante desligamento deixa futuro; raiz app/docs/BACKLOG.md:1338 | O54 | DOBRA |
| O50-DESLIGAR-ENCERRA-TUDO | ESPINHA-E | SIM | LEU app/colaboradores/services/vinculo.py:858: vínculo futuro não é fechado; variante explícita app/docs/BACKLOG.md:1332; raiz app/docs/BACKLOG.md:1338 | O54 | DOBRA |
| O50-CELULA-APOS-DESLIGAMENTO | ESPINHA-E | SIM | LEU app/colaboradores/services/vinculo.py:858: vínculo futuro sobrevive ao desligamento; INFERIU risco de previsão futura; complemento explícito app/docs/BACKLOG.md:1333 e raiz app/docs/BACKLOG.md:1338 | O54 | DOBRA |

Trecho final: 46 linhas, CONTA 16, DOBRA 23, OBSOLETA 7, ? 13.

Notas para a consolidação: O40 é filho de O44, mas confira se a mãe O44 recebeu outra raiz no restante da faixa. As mães JUIZ-DO-PAR-DUAS-ENTRADAS e O248 são inferências de mesmo defeito/autoridade; não encontrei absorção literal neste trecho. O105 fica separada porque “TXT estável com gravado idêntico” e “fechamento online” não são automaticamente a mesma entrega.

## 13/11 21:02 · sessao 4548a10e14af · - · - · origem=Ronald (tmux)

| id | balde | existe | prova | mae | veredito |
|---|---|---|---|---|---|
| REGUA-DO-PERIODO-COMPLEMENTO-19-10 | ESPINHA-E | PARCIAL | LEU app/ponto/services/espelho.py:1164 e app/relatorios/pdf_espelho.py:419: cálculo por fatia presente; censo e apply pedidos em app/docs/BACKLOG.md:1634 [não verificado]. | - | CONTA |
| BO-CARTAO-ESTRELA-COMPLEMENTO-19-10 | ESPINHA-E | ? | LEU app/ponto/services/espelho.py:1164: leitor atual; reprodução da estrela e medição do noturno de app/docs/BACKLOG.md:1635 [não verificado]. INFERIU complemento literal. | BO-CARTAO-ESTRELA-X-REALIZADO | DOBRA |
| SUPRA-JUIZ-AINDA-DERIVA-POR-DATA | ESPINHA-E | PARCIAL | LEU app/ponto/supra_juiz.py:291: recebe encerramento; app/ponto/tests/test_contador_teto_por_data_do_dia.py:41 mantém ESPERADO=1. INFERIU residual do contador; mãe literal em app/docs/BACKLOG.md:1655. | O258 | DOBRA |
| O325 | ESPINHA-E | PARCIAL | LEU app/ponto/services/fechamento.py:1696: consulta vencidas; :1721 conta; :1143 recarimba. Situação atual da 09 [não verificado]. INFERIU mesma projeção envelhecida da O48. | O48 | DOBRA |
| O327 | ESPINHA-E | PARCIAL | LEU app/ponto/services/dia_pago.py:316: recusa campo sem dono; app/ponto/services/fechamento.py:1257 declara recusa. Cobertura atual e publicação exigida em app/docs/BACKLOG.md:1662 [não verificado]. | - | CONTA |
| RETIFICACAO-DA-10-8069-PRIMEIRO | ESPINHA-E | ? | LEU app/ponto/services/fechamento.py:448: porta de recálculo; defeito por rubrica e retificação dos casos de app/docs/BACKLOG.md:1666 [não verificado]. | - | CONTA |
| ITEM-4-ORFA-PELAS-LAMPADAS | ESPINHA-E | PARCIAL | LEU app/ponto/management/commands/reconciliar_perguntas_orfas.py:4: evento usa lâmpada e cron vigia; impacto/passivo [não verificado]. Mãe literal em app/docs/BACKLOG.md:1672. | TURNO-ABERTO-UM-EMISSOR | DOBRA |
| CASA-CHAMADO-C2 | ESPINHA-E | PARCIAL | LEU app/ponto/management/commands/detectar_vinculo_divergente.py:35 e :52: lavra lista sem abrir chamado; prova nominal dos 78 e baixa [não verificado]. INFERIU parte da casa dos chamados. | CHAMADO-EM-RAIA | DOBRA |
| CASA-CHAMADO-C3 | ESPINHA-E | PARCIAL | LEU app/ponto/management/commands/reconciliar_fantasmas.py:173: ainda escolhe ativa=True; app/ponto/management/commands/termometro_regua.py:35 também. INFERIU migração incompleta da família escala. | CONTRATOS-RESTANTES-E1-E2-FOLHA-K6 | DOBRA |
| CASA-ESCALA-E4 | ESPINHA-E | PARCIAL | LEU app/escala/folgas.py:487: consulta vinculo_cobre_a_data; app/core/juizes.py:1311 declara autoridade. Lista dos vínculos e regeneração pedidas em app/docs/BACKLOG.md:1683 [não verificado]. | CONTRATOS-RESTANTES-E1-E2-FOLHA-K6 | DOBRA |
| CASA-ESCALA-E5 | ESPINHA-E | NAO | LEU app/core/juizes.py:1310: juiz declarado; app/escala/folgas.py:493: último tomador consulta a autoridade. INFERIU entrega de declaração e migração atendida. | CONTRATOS-RESTANTES-E1-E2-FOLHA-K6 | OBSOLETA |
| PAR-RELAMPAGO-PELO-TURNO | ESPINHA-E | NAO | LEU app/ponto/management/commands/detectar_par_relampago.py:349 e :353: universo carregado e par consultado ao juiz. INFERIU cura do leitor pedido; vínculo com O258 em app/docs/BACKLOG.md:1488. | O258 | OBSOLETA |
| REABRIR-PERGUNTA-A-PREMISSA | ESPINHA-E | PARCIAL | LEU app/chamados/models.py:1437 e :1447: porta consulta e respeita recusa da premissa. Contador zero e sete dias sem ciclo [não verificado]. | - | CONTA |
| TERMOMETRO-VIRA-LISTA | ESPINHA-E | PARCIAL | LEU app/escala/management/commands/termometro_regua.py:60 e :93: lista lê escore e lavra; prova nominal e baixa do estoque [não verificado]. Mãe literal em app/docs/BACKLOG.md:1687. | REABRIR-PERGUNTA-A-PREMISSA | DOBRA |
| REABRIR-PREMISSA-DO-MARCO | ESPINHA-E | NAO | LEU app/chamados/tests/test_reabrir_pergunta_o_marco.py:57 e :68: caracterização exige renascimento pelo marco novo; app/chamados/models.py:1437 conserva porta por premissa. INFERIU proposta refutada, conforme caso codificado. | REABRIR-PERGUNTA-A-PREMISSA | OBSOLETA |
| ADMIN-RESOLVE-FLAP | ESPINHA-E | PARCIAL | LEU app/chamados/models.py:1419: relê banco; :274 e :280: trilha antes do save no atomic. Cura do código presente; estoque/trilha atual [não verificado]. INFERIU residual do escritor de chamado. | CHAMADO-EM-RAIA | DOBRA |
| RAIA-DO-CONTRATO-20-NOITE | ESPINHA-E | PARCIAL | LEU app/ponto/management/commands/detectar_vinculo_divergente.py:52, app/ponto/management/commands/detectar_par_relampago.py:353 e app/ponto/management/commands/marcar_foto_ausente_retro.py:74: três migrações presentes; medição/baixa [não verificado]. | CHAMADO-EM-RAIA | DOBRA |
| ORACULO-RECUSA-LINHA-SO-FOLGA | ESPINHA-E | ? | LEU app/ponto/services/dia_pago.py:313: guardas de completude continuam; app/ponto/services/fechamento.py:1271: produtor de rubricas. Os 79 sem dias e sua causa atual [não verificado]. INFERIU mesmo buraco de cobertura da O327. | O327 | DOBRA |
| LOTE-VAZA-ENTRE-COLABS | ESPINHA-E | ? | LEU app/ponto/services/fechamento.py:448 e :1269: caminho em lote; divergência lote/individual e causa de app/docs/BACKLOG.md:1709 [não verificado]. | - | CONTA |
| CONTRATO-20-DOIS-EVENTOS | ESPINHA-E | PARCIAL | LEU app/ponto/portas/celula.py:531 e :540: regeneração agenda rejunção/reavaliação; app/ponto/signals.py:132: evento de batida. Medição de 24 h [não verificado]. INFERIU parte da migração por evento. | CHAMADO-EM-RAIA | DOBRA |
| EMISSOR-COBRA-MARCO-DO-TURNO-VIZINHO | ESPINHA-E | ? | LEU app/ponto/management/commands/detectar_ausencias.py:625: vínculo do dia recebido do juiz; cobrança atual dos episódios de app/docs/BACKLOG.md:1716 [não verificado]. INFERIU mesmo defeito de atribuição do dia. | DIA-DA-BATIDA-JUIZ-UNICO | DOBRA |
| PASSADO-SEM-MARCOS-SEM-TETO | ESPINHA-E | ? | LEU app/ponto/services/cartorio.py:1397: consulta encerramento para passado; existência de emissão prematura sem marcos de app/docs/BACKLOG.md:1721 [não verificado]. | - | CONTA |
| LEGADO-51-ACESAS | ESPINHA-E | ? | LEU app/chamados/juizes.py:1417 e :1429: resposta e concordância com luz julgadas; situação das 51 materializadas de app/docs/BACKLOG.md:1724 [não verificado]. | - | CONTA |
| C5-FALTA-MAIOR-QUE-PREVISTO | ESPINHA-E | ? | LEU app/ponto/services/dia_pago.py:274 e :292: falta e previsto vêm de mapas distintos; nova medição da classe [não verificado]. INFERIU repetição da C5-LISTA-DE-FROTA, app/docs/BACKLOG.md:1529 e :1725. | C5-LISTA-DE-FROTA | DOBRA |
| DIA-DA-BATIDA-JUIZ-UNICO | ESPINHA-E | SIM | LEU app/chamados/juizes.py:53: _dia_da_batida ainda deriva por timezone.localdate(timestamp). INFERIU leitor residual de dia civil abrangido pelo censo de app/docs/BACKLOG.md:1727. | - | CONTA |
| PERGUNTA-DO-DIA-EM-CURSO | ESPINHA-E | PARCIAL | LEU app/ponto/services/cartorio.py:1363 e :1369: teto por instante hoje e no noturno de ontem. Destino e medição atual das 48 futuras [não verificado]. | - | CONTA |
| VINCULO-DE-HOJE-FORA-DO-CENSO | ESPINHA-E | SIM | LEU app/ponto/services/espelho.py:43: seleciona ativa=True e usa início como piso em :45. INFERIU sítio remanescente do censo de app/docs/BACKLOG.md:1739. | CASA-ESCALA-E4 | DOBRA |
| BUG-LASTRO-VAI-E-VOLTA | ESPINHA-E | PARCIAL | LEU app/chamados/models.py:1437: porta unificada; ciclos atuais de produção [não verificado]. Absorção literal em app/docs/BACKLOG.md:1686. | REABRIR-PERGUNTA-A-PREMISSA | DOBRA |
| BUG-LASTRO-CLASSE-2 | ESPINHA-E | PARCIAL | LEU app/chamados/models.py:1437: recusas comuns na reabertura; ciclos dos outros crons [não verificado]. Absorção literal em app/docs/BACKLOG.md:1686. | REABRIR-PERGUNTA-A-PREMISSA | DOBRA |
| SETE-CRONS-SEM-LEI-ESCRITA | ESPINHA-E | PARCIAL | LEU app/ponto/management/commands/reconciliar_perguntas_orfas.py:1: já vigia; app/ponto/management/commands/reconciliar_fantasmas.py:173 conserva decisão por vínculo atual. Complemento literal em app/docs/BACKLOG.md:1758. | CONTRATOS-RESTANTES-E1-E2-FOLHA-K6 | DOBRA |
| LASTRO-DEGRAU-2 | ESPINHA-E | SIM | LEU app/ponto/services/lastro.py:304 e :307: degrau sem lâmpada ainda monta alvo e compara distância linear. INFERIU leitor residual do casamento batida/marco. | JUIZ-DO-PAR-GRUPO-1 | DOBRA |
| R1-ESTRUTURA-91 | ESPINHA-E | ? | LEU app/core/placar_estrutural.py:130: registro posterior ainda discrimina divergências, sem causa/destino; existência atual das famílias e dos 91 [não verificado]. | - | CONTA |
| V-FASE-FILA-DONO-DEPENDE-DA-JANELA | ESPINHA-E | NAO | LEU app/ponto/management/commands/e6_oraculo.py:179 e app/escala/services/fase_12x36.py:134: usa lavra que cobriu cada dia; :151 reabre janelas alteradas. INFERIU cura pela obra específica. | V-FASE-DONO-AS-OF | OBSOLETA |
| V-FASE-DONO-AS-OF | ESPINHA-E | NAO | LEU app/escala/services/fase_12x36.py:134, :146 e :188: leitura por cobertura e gatilho pelos carimbos do dia; app/ponto/management/commands/e6_oraculo.py:179 consome. INFERIU contrato implementado. | - | OBSOLETA |
| F3-RECORTE-E-CAUDA | ESPINHA-E | PARCIAL | LEU app/relatorios/furos.py:42: leitor paralelo removido; :123 lê D4. Medida do estoque requerida na parte A de app/docs/BACKLOG.md:1779 [não verificado]. | O261-E-CELULA-20-UM-DONO | DOBRA |
| BATERIA-QUEIXAS-ADMIN | ESPINHA-E | ? | LEU app/docs/PROMPTS.md:1863: nomeia ponto/tests/test_bateria_queixas_admin.py; busca por nome/queixas não encontrou esse sítio. Sem sítio; validação humana [não verificado]. | - | CONTA |
| O169-ATA-VAZIA-FOLGA | ESPINHA-E | ? | LEU app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:109: folga testada com orfas/lampadas preenchidas; caso específico trabalha=False com ata={} e mutante pedido [não verificado]. | O169 | DOBRA |
| O169-CONTADOR-NAO-MEDIDO | ESPINHA-E | SIM | LEU app/ponto/services/cluster_espurio.py:29 e :75: fato None conservado; :148 conta encerrados com assinatura, sem expor esse universo vivo não medido. INFERIU lacuna do contador. | O169 | DOBRA |
| O169-LIMIAR-15-DUPLO | ESPINHA-E | SIM | LEU app/ponto/services/cluster_espurio.py:35 e app/escala/orfa_classe.py:28: limite 15 ainda em dois sítios. INFERIU duplicação pedida. | O169 | DOBRA |
| TIPO-NA-FONTE | ESPINHA-E | SIM | LEU app/ponto/services/cluster_espurio.py:27 e :31: leitura crua de ata/orfas permanece. INFERIU migração tipada ainda incompleta; escopo em app/docs/BACKLOG.md:1836. | - | CONTA |
| C20-FIO-MUDO | ESPINHA-E | PARCIAL | LEU app/chamados/signals.py:203 e :259: eventos de retorno e alteração da pergunta; app/chamados/services/fio.py:925 e :934 acorda âncora anterior. Universo dos cinco atos [não verificado]. | CHAMADO-EM-RAIA | DOBRA |
| C20-FIO-MUDO-FILA-CODEX | ESPINHA-E | SIM | LEU app/chamados/models.py:1409 até :1489: reabrir chama escrita sem atomic envolvendo a porta inteira; migração tem atomic próprio em app/chamados/services/fio.py:2084. INFERIU residual D1 ainda presente. | CHAMADO-EM-RAIA | DOBRA |
| C20-FIO-MUDO-FILA-OPUS | ESPINHA-E | SIM | LEU app/chamados/models.py:493 e :498: rebaixar calcula alteração antes da trava; app/chamados/juizes.py:592 mantém relógio interno. INFERIU residual de concorrência/relógio. | CHAMADO-EM-RAIA | DOBRA |
| MOTOR-LE-O-TURNO | ESPINHA-E | SIM | LEU app/ponto/services/dia_pago.py:132: dia ainda pode vir da entrada civil; app/ponto/motor_calculo_v2.py:2942 conserva fallback por data da entrada. INFERIU migração incompleta para turno oficial. | - | CONTA |
| FIO-E0-V4-E1-ENTRADAS | ESPINHA-E | PARCIAL | LEU app/chamados/models.py:2845 e :2876: nascimento por contrato; app/chamados/services/fio.py:801: escrita única do dia. Resíduos de relógio em app/chamados/juizes.py:592; passivo [não verificado]. | - | CONTA |
| FIO-E0-V4-E1-FILA | ESPINHA-E | PARCIAL | LEU app/chamados/services/fio.py:2094 e :2096: conjunto travado numa chamada; app/chamados/models.py:1465: recusa falante. Resíduo em app/chamados/juizes.py:592. INFERIU fila da E1. | FIO-E0-V4-E1-ENTRADAS | DOBRA |
| E1-FILA-V2-3-A-6 | ESPINHA-E | PARCIAL | LEU app/chamados/management/commands/backfill_veredito_lavrado.py:94 e :95: relógio unificado; app/chamados/management/commands/reabrir_orfas_enterradas.py:153: ordem comum; app/chamados/juizes.py:592 ainda date.today. | FIO-E0-V4-E1-ENTRADAS | DOBRA |

47 linhas: CONTA 17, DOBRA 25, OBSOLETA 5, ? 12.

Observação para consolidação: a mãe literal REGUA-DO-PERIODO não aparece no universo fornecido; mantive “-” sem inventar equivalência com O60. As mães amplas CHAMADO-EM-RAIA e CONTRATOS-RESTANTES foram inferidas pelo mesmo conjunto de portas/família; ajustar à raiz escolhida na consolidação, preservando as provas.

## 13/11 21:02 · sessao 1e7249b01e2f · - · - · origem=Ronald (tmux)

| id | balde | existe (SIM/NAO/PARCIAL/?) | prova (arquivo:linha do HEAD ou commit; LEU x INFERIU) | mae | veredito (CONTA/DOBRA/OBSOLETA) | [nao verificado]
|---|---|---|---|---|---|
| BO-INSS-LEITORES-QUE-NAO-MIGRARAM | LATERAL-ESTRUTURA | PARCIAL | LEU app/escala/services/leitor_celula.py:433: transporta cobertura da ata; app/docs/BACKLOG.md:1087 registra seis resíduos; persistência atual [não verificado]. INFERIU mãe pela cobertura parcial, app/docs/BACKLOG.md:786. | COBERTURA-PARCIAL-UM-JUIZ | DOBRA |
| BO-ATESTADO-ANDROID-ERRO-AO-ENVIAR | LATERAL-ESTRUTURA | PARCIAL | LEU app/api/views.py:1811 e app/api/views.py:1833: reenvio devolve registro existente e incorpora anexo; app/docs/BACKLOG.md:1091 conserva smoke. APP/reenvio real atual [não verificado]. | - | CONTA |
| REABRIR-NAO-DESTROI | LATERAL-ESTRUTURA | PARCIAL | LEU app/chamados/services/disputa_emissao.py:1615: registra foto antes de limpar; app/templates/chamados/partials/modal_fio.html:205 consulta histórico. Smoke exigido em app/docs/BACKLOG.md:1094 [não verificado]. | - | CONTA |
| UNIVERSO-DE-PERGUNTA-DO-DIA-SAO-DOIS | LATERAL-ESTRUTURA | SIM | LEU app/chamados/services/vigia_fio.py:398: universo por celula_id; app/ponto/services/cartorio.py:1389 ainda consulta perguntas_que_resolvem. INFERIU inclusão na unificação do dia, app/docs/BACKLOG.md:893. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| DISPUTA-FECHADA-SEM-RESOLUCAO | LATERAL-ESTRUTURA | SIM | LEU app/chamados/models.py:1911 e app/chamados/models.py:1934: fechar continua aceitando resolução vazia, embora possua motivo categórico. INFERIU mãe do fechamento único em app/docs/BACKLOG.md:1935. | RAIZ-DA-DISPUTA | DOBRA |
| VIGENCIA-SOBREPOSTA-COL515 | LATERAL-ESTRUTURA | ? | LEU app/colaboradores/services/vinculo.py:523: censo só inclui vínculos ativos; app/docs/BACKLOG.md:1102 descreve dois inativos. Sobreposição atual dos registros [não verificado]; INFERIU mãe pela linha temporal, app/docs/BACKLOG.md:1127. | VINCULO-LINHA-DO-TEMPO | DOBRA |
| HE-DE-14H-NUM-DIA (col945 12/09) | LATERAL-ESTRUTURA | ? | LEU app/docs/BACKLOG.md:1103: causa não medida; app/ponto/motor_calculo_v2.py:1646 é leitor de extra declarada, sem prova de explicar este dia. Valor e causa atuais [não verificado]. | - | CONTA |
| BUG-ANCORA-NAO-REGENERA | LATERAL-ESTRUTURA | PARCIAL | LEU app/escala/signals.py:125: alteração de âncora chama regenerar_celulas_vinculo. app/docs/BACKLOG.md:1118 conserva decisões de col824/col899; resolução atual desse residual [não verificado]. | - | CONTA |
| FAMILIA-FASE-12x36 | LATERAL-ESTRUTURA | PARCIAL | LEU app/ponto/management/commands/censo_fase_12x36.py:35: há porta de cura; app/docs/BACKLOG.md:1128 conserva propostas aguardando decisão. INFERIU mãe do diagnóstico/propostas absorvido da O-HAIKU, app/docs/BACKLOG.md:1130; passivo atual [não verificado]. | O-HAIKU-INVESTIGA | DOBRA |
| O-HAIKU-INVESTIGA | LATERAL-ESTRUTURA | PARCIAL | LEU mensageria/nucleo/ferramentas.py:1330: ferramenta de desvios existe; app/escala/services/propositor.py:382 monta propostas. Busca das três ferramentas literais de app/docs/BACKLOG.md:1130 não as encontrou; equivalência completa [não verificado]. | - | CONTA |
| O-JANELA-HE | LATERAL-ESTRUTURA | PARCIAL | LEU app/ponto/janela_he.py:77 e app/ponto/portas/he.py:201: janela e relavratura implementadas. app/docs/BACKLOG.md:1134 exige sete resultados e smoke de 13 passos; conclusão dessas verificações [não verificado]. | - | CONTA |
| CHAMADO-NASCE-EM-EXPORTADA | LATERAL-ESTRUTURA | ? | LEU app/ponto/services/cartorio.py:1627: código admite religar pergunta em competência exportada; app/docs/BACKLOG.md:1137 pede censo dos novos nascimentos. Nascimentos atuais e emissor do caso [não verificado]. | - | CONTA |
| DISPUTA-4418-ABERTA-COM-TUDO-RESOLVIDO | LATERAL-ESTRUTURA | ? | LEU app/chamados/models.py:1951: fechamento consulta perguntas vivas; estado atual da disputa 4418 [não verificado]. INFERIU mãe da reação do agregado/fechamento, app/docs/BACKLOG.md:1935; caso em app/docs/BACKLOG.md:1138. | RAIZ-DA-DISPUTA | DOBRA |
| COL900-VINCULO-X-CELULA | LATERAL-ESTRUTURA | ? | LEU app/ponto/portas/regen_exportada.py:145: porta examina células e vínculos; divergência atual dos 84 dias de app/docs/BACKLOG.md:1140 [não verificado]. INFERIU mãe da correção retroativa com regeneração, app/docs/BACKLOG.md:1228. | O121 | DOBRA |
| COL882-O-PROXIMO-COL515 | LATERAL-ESTRUTURA | ? | LEU app/docs/BACKLOG.md:1142: casos são dias sem cobertura de vínculo; cadastro atual [não verificado]. INFERIU mãe explicitamente abrangente e que nomeia col882, app/docs/BACKLOG.md:1399. | O143 | DOBRA |
| DISPUTA-S84-RETRO-PASSIVO | LATERAL-ESTRUTURA | PARCIAL | LEU app/chamados/services/materializacao.py:842: origem recusa plantio sem resposta. app/docs/BACKLOG.md:1144 pede tratamento das batidas antigas; estoque e marcação atuais [não verificado]. | - | CONTA |
| COL369-TRES-RESTANTES | LATERAL-ESTRUTURA | ? | LEU app/docs/BACKLOG.md:1156: três fatos sem medição; app/escala/utils.py:590 já recebe papel do juiz, mas isso não prova os dias nem o vínculo 1296/1313. Situação dos três casos [não verificado]. | - | CONTA |
| O-JANELA-HE parte B2 (AUTORIZAR) | LATERAL-ESTRUTURA | PARCIAL | LEU app/ponto/portas/he.py:174 e app/ponto/portas/he.py:201: decisão travada e relavratura. app/docs/BACKLOG.md:1158 conserva smoke [não verificado]; parte da obra ampla em app/docs/BACKLOG.md:1134. | O-JANELA-HE | DOBRA |
| BO-354 | LATERAL-ESTRUTURA | PARCIAL | LEU app/escala/servico_jornada.py:452: veto posicional já silencia dias completos; app/escala/servico_jornada.py:459 ainda usa tipos gravados. Casos/perguntas atuais [não verificado]; INFERIU mãe da interpretação única das marcações, app/docs/BACKLOG.md:1337. | O53 | DOBRA |
| O-FOLGA-DIA-ERRADO | LATERAL-ESTRUTURA | SIM | LEU app/escala/services/propositor.py:121: diagnóstico atual compara ritmo diário/alternado; regra semanal ≥3/4 de app/docs/BACKLOG.md:1163 não aparece nesse diagnóstico. Absorção explícita pela mãe em app/docs/BACKLOG.md:1130. | O-HAIKU-INVESTIGA | DOBRA |
| O-GEO-DECISAO | LATERAL-ESTRUTURA | SIM | LEU app/chamados/services/acoes_chamado.py:171 e app/chamados/services/acoes_chamado.py:251: autorizar/advertir e recusar continuam portas separadas; recusa ainda devolve dia à cobrança. Contrato ampliado em app/docs/BACKLOG.md:1165 permanece distinto dessas ações. | - | CONTA |
| O73b | LATERAL-ESTRUTURA | PARCIAL | LEU app/ponto/motor_calculo_v2.py:2709: volta de bloco deixa de gerar atraso; app/ponto/management/commands/aplicar_09_corte_b.py:12 conserva exclusão de col81. Acerto do passivo atual [não verificado]; mãe expressa em app/docs/BACKLOG.md:1166. | O73 | DOBRA |
| CORTE-B-30 | LATERAL-ESTRUTURA | ? | LEU app/ponto/management/commands/aplicar_09_corte_b.py:43 e app/ponto/management/commands/aplicar_09_corte_b.py:80: existem critérios que separam casos. Lista e explicação atuais dos separados [não verificado]; INFERIU residual do corte da O68, app/docs/BACKLOG.md:1354. | O68 | DOBRA |
| O145 | LATERAL-ESTRUTURA | PARCIAL | LEU app/ponto/janela_he.py:234: recupera pontas missing pelo relato; app/ponto/janela_he.py:256 conserva limite para casos sem célula/atribuição divergente. Estoque atual [não verificado]; INFERIU parte da entrega ampla da janela, app/docs/BACKLOG.md:1134. | O-JANELA-HE | DOBRA |
| O146 | LATERAL-ESTRUTURA | PARCIAL | LEU app/escala/models.py:1210 e app/ponto/motor_calculo_v2.py:1646: extra declarada tem autoridade e leitor. Linha HAIKU requerida em app/docs/BACKLOG.md:1171 não foi encontrada pela busca de extra declarada no golden/ferramentas; conclusão completa [não verificado]. | - | CONTA |
| O214 | LATERAL-ESTRUTURA | PARCIAL | LEU app/ponto/portas/he.py:480: autoriza período por colaborador; app/ponto/services/autorizacao_he_periodo.py:2 implementa prévia. Apply/smoke conservados em app/docs/BACKLOG.md:1172 [não verificado]; INFERIU ampliação da autorização da mãe, app/docs/BACKLOG.md:1134. | O-JANELA-HE | DOBRA |
| R1 | LATERAL-ESTRUTURA | ? | LEU app/escala/utils.py:590: matcher já recebe papéis do juiz; app/docs/BACKLOG.md:1178 reduziu premissa a um caso. Persistência do caso restante [não verificado]; INFERIU mãe do casamento batida/marco, app/docs/BACKLOG.md:812. | JUIZ-DO-PAR-GRUPO-1 | DOBRA |
| PAR-NAO-ADJACENTE | LATERAL-ESTRUTURA | SIM | LEU app/escala/utils.py:467: só aceita par interno quando _iz == _i + 1; batida solta intermediária ainda impede transportar esse par. INFERIU mãe da fonte de casamento, app/docs/BACKLOG.md:812. | JUIZ-DO-PAR-GRUPO-1 | DOBRA |
| O261-E-F2-FILA | LATERAL-ESTRUTURA | PARCIAL | LEU app/chamados/services/materializacao.py:573: recusa agora deixa evento; app/chamados/models.py:3180 ainda lê relógio interno. app/docs/BACKLOG.md:1193 lista resíduos; raiz explícita E2 em app/docs/BACKLOG.md:893. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| O261-E-F2-FILA-V2 | LATERAL-ESTRUTURA | PARCIAL | LEU app/chamados/tests/test_o261e_f2_ramo_e_i20.py:265 e app/chamados/tests/test_o261e_f2_porta_carimbo.py:376: proteções adicionadas; app/chamados/tests/test_o261e_f2_ramo_e_i20.py:12 e :143 ainda divergem no rótulo da resposta. Raiz em app/docs/BACKLOG.md:893. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| REABERTURA-RECUSADA-DEDUP | LATERAL-ESTRUTURA | SIM | LEU app/chamados/models.py:1443 e app/chamados/models.py:1449: cada recusa chama evento sem dedup nesse ramo. Cauda F2 expressa em app/docs/BACKLOG.md:1195; raiz E2 em app/docs/BACKLOG.md:893. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| O232 | LATERAL-ESTRUTURA | PARCIAL | LEU app/escala/utils.py:596 e app/escala/utils.py:613: ata já usa papéis do turno; app/escala/utils.py:625 mantém caminho geométrico quando indisponíveis. Remedição/complemento exigidos em app/docs/BACKLOG.md:1196 [não verificado]; mãe L-115/BALDE A em app/docs/BACKLOG.md:1360. | O73 | DOBRA |
| O236 | LATERAL-ESTRUTURA | ? | LEU app/folha/export.py:970 e app/folha/export.py:1176: retenção por furo e sua autoridade existem. Distribuição atual por motivo e efeito da L-033 pedidos em app/docs/BACKLOG.md:1203 [não verificado]. | - | CONTA |
| O238 | LATERAL-ESTRUTURA | SIM | LEU app/ponto/portas/regen_exportada.py:204: trilha ainda declara FechamentoMensal com colaborador.pk; app/ponto/services/fechamento.py:1613 ainda carimba invalidação. Mãe A0-b explícita em app/docs/BACKLOG.md:1205. | O219 | DOBRA |
| O127 | LATERAL-ESTRUTURA | SIM | LEU app/ponto/services/flip_auto.py:201: recebe _pausas; app/ponto/services/flip_auto.py:218 monta slots somente de hi/hf/hii/hfi. Segunda pausa continua fora dos slots. | - | CONTA |
| O121 | LATERAL-ESTRUTURA | SIM | LEU app/colaboradores/services/vinculo.py:72: piso ainda recusa início anterior à janela; app/ponto/portas/celula.py:376 conserva recusa por competência lavrada. Curas A1/A2/A3 exigidas em app/docs/BACKLOG.md:1228 não estão integralmente atendidas. | - | CONTA |

TRECHO B: 36 linhas, CONTA 14, DOBRA 22, OBSOLETA 0, ? 10.

## 13/11 21:02 · sessao f2b0a70daa14 · - · - · origem=Ronald (tmux)

| id | balde | existe | prova | mae | veredito |
|---|---|---|---|---|---|
| O194 | ESPINHA-E | PARCIAL | LEU app/ponto/services/cartorio.py:1397 e :2033: encerramento entrou na impressão para passado; guarda de hoje ainda exclui folga. INFERIU mãe pelo mesmo juiz. | CHAMADO-EM-RAIA | DOBRA |
| O210 | ESPINHA-E | ? | LEU app/ponto/management/commands/recalcular_fechamento.py:118: porta existe; deriva atual e aplicação não verificadas. INFERIU mãe pelo passivo do fechamento. | O54 | DOBRA |
| O229 | ESPINHA-E | SIM | LEU app/ponto/services/espelho.py:1261: lista ainda aceita somente chegada fora da janela, excluindo saída; basta para manter a obra. | - | CONTA |
| O215 | ESPINHA-E | ? | LEU app/folha/views.py:268: montagem do TXT; comparação nominal do TXT pago com atual não fornecida. INFERIU mãe pelo mesmo acerto da 09; app/docs/BACKLOG.md:1467. | O269 | DOBRA |
| O216 | ESPINHA-E | ? | LEU app/ponto/turnos.py:1939: porta atual do pareamento; republicação dos três números não verificada; app/docs/BACKLOG.md:1468. | - | CONTA |
| O217 | ESPINHA-E | ? | LEU app/ponto/tests/test_bateria_lavratura.py:193: existe teste de duas lavras; execução e estabilização da frota não verificadas. LEU encaminhamento à O218 em app/docs/GRADE-MESTRA.md:225. | O218 | DOBRA |
| O248 | ESPINHA-E | NAO | LEU app/ponto/turnos.py:2077, app/ponto/services/flip_auto.py:285 e app/ponto/services/cartorio.py:1153: autoridade e leitores substituem decisão por data. LEU mãe em app/docs/BACKLOG.md:1480. | CHAMADO-EM-RAIA | OBSOLETA |
| O249 | ESPINHA-E | NAO | LEU app/ponto/turnos.py:2199 e app/ponto/management/commands/detectar_par_relampago.py:300 e :353: duas portas consultam par_completo. LEU complemento em app/docs/BACKLOG.md:1481; INFERIU raiz. | CHAMADO-EM-RAIA | OBSOLETA |
| O252 | ESPINHA-E | NAO | LEU app/ponto/views.py:1505 e app/folha/views.py:212: aprovação e geração consultam classificar_export; carimbo deriva dos itens em app/folha/views.py:215. INFERIU raiz O48/O54. | O54 | OBSOLETA |
| O254 | ESPINHA-E | NAO | LEU app/core/contratos_estruturais.py:375: contrato tem teste e exceção; app/core/juizes.py:1272: pendentes da batida vazios. | - | OBSOLETA |
| O257 | ESPINHA-E | NAO | LEU app/ponto/management/commands/gerar_celulas.py:182 e :210: lista e fonte consultam competencia_fechada_para_celula, preservando ramo holerite em :178. INFERIU raiz dos contratos. | DOSSIES-DAS-CELULAS | OBSOLETA |
| O258 | ESPINHA-E | NAO | LEU app/config/crons.py:2035 e app/core/juizes.py:472: ambas as listas exigidas estão vazias; encerramento e par têm autoridades. INFERIU raiz pelo escopo chamado. | CHAMADO-EM-RAIA | OBSOLETA |
| SOBRA-NA-BORDA-O-QUE-FICOU | ESPINHA-E | SIM | LEU app/escala/utils.py:668: raio literal de 90 permanece; basta para impedir baixa integral do conjunto residual descrito em app/docs/BACKLOG.md:1493. | - | CONTA |
| C7-DISPUTA-FECHA-POR-UM-JUIZ | ESPINHA-E | PARCIAL | LEU app/chamados/models.py:1904 e :1953: juiz e porta existem; migração integral dos vinte sítios e acervo não verificadas. INFERIU raiz escritor/juiz do chamado. | CHAMADO-EM-RAIA | DOBRA |
| BUG-DIA-SEM-SAIDA | ESPINHA-E | PARCIAL | LEU app/ponto/services/veredito_celula.py:160: porta trata fio do dia; os dias residuais sem ação não foram fornecidos. INFERIU mãe pelo mesmo defeito de caminho inacessível; app/docs/GRADE-MESTRA.md:182. | O66 | DOBRA |
| BO-B-DIAPAGO-SE-CONTRADIZ | ESPINHA-E | ? | LEU app/ponto/services/dia_pago.py:269 e :274: campos têm alimentações por dia; contradição atual dos 1.305 dias não verificada. INFERIU mãe pelo valor diário único. | O-DIA-PAGO | DOBRA |
| VARREDURA-VIRA-EVENTO-24-CRONS | ESPINHA-E | PARCIAL | LEU app/config/crons.py:2035: lista de juízes vazia; a exigência de zero medido por classe de app/docs/BACKLOG.md:1505 não é demonstrada pelo dado fornecido. INFERIU raiz. | CHAMADO-EM-RAIA | DOBRA |
| CONTRATOS-RESTANTES-E1-E2-FOLHA-K6 | ESPINHA-E | PARCIAL | LEU app/core/juizes.py:1310 e :1352: escala declarada, sem pendentes; app/chamados/services/passivo_validado.py:124 mantém leitor próprio de entrega. INFERIU raiz dos contratos familiares. | DOSSIES-DAS-CELULAS | DOBRA |
| E6-NONE-E-CADASTRO | ESPINHA-E | NAO | LEU app/ponto/management/commands/e6_oraculo.py:98 e :183: ausência de vínculo chega pelo carregador e retorna CADASTRO; função paralela retirada, lápide em :46. | DOSSIES-DAS-CELULAS | OBSOLETA |
| GRADE-ESC-DO-DIA-E1 | ESPINHA-E | NAO | LEU app/escala/utils.py:988 e :1311: as duas esc_do_dia consultam vinculo_do_dia. INFERIU raiz dos contratos E1. | DOSSIES-DAS-CELULAS | OBSOLETA |
| CARIMBO-SEM-VIGENCIA | ESPINHA-E | SIM | LEU app/ponto/services/cartorio.py:471 e :480: impressão não recebe vigência; chamada em :1398 tampouco a fornece. INFERIU mãe pela entrada ausente da impressão. | CHAVE-DE-ENTRADA-INCOMPLETA | DOBRA |
| CONTADOR-PAREADOR-X-CELULA | ESPINHA-E | PARCIAL | LEU app/ponto/services/vigia_pareador_celula.py:42 e :106: contador existe e compara borda; cura D2 e zero atual não verificados. LEU filiação em app/docs/BACKLOG.md:1510; INFERIU raiz. | DOSSIES-DAS-CELULAS | DOBRA |
| CONTADOR-AUSENCIA-X-VEREDITO | ESPINHA-E | PARCIAL | LEU app/ponto/services/vigia_ausencia_veredito.py:30 e app/config/crons.py:1766: contador e leitor existem; quatro exportados não medidos. LEU filiação em app/docs/BACKLOG.md:1511; INFERIU raiz. | DOSSIES-DAS-CELULAS | DOBRA |
| TURNO-ABERTO-UM-EMISSOR | ESPINHA-E | PARCIAL | LEU app/config/crons.py:2075 e :2079: fantasmas e órfãs migraram; diagnóstico, suspensão e demais entregas de app/docs/BACKLOG.md:1513 não comprovados integralmente. LEU complemento; INFERIU raiz. | DOSSIES-DAS-CELULAS | DOBRA |
| ATA-NAO-DISPENSA-JUSTIFICADA | ESPINHA-E | PARCIAL | LEU app/ponto/services/cartorio.py:802 e :804: justificativa agora dispensa marcos; aplicação do acervo não verificada. | - | CONTA |
| CONCORDE-PREVISTO-ZERO | ESPINHA-E | ? | LEU app/escala/utils.py:2058: distingue zero por ausência de turno; causa atual dos 730 dias de app/docs/BACKLOG.md:1517 não fornecida. | - | CONTA |
| VIVA-DELEGA-AO-JUIZ | ESPINHA-E | NAO | LEU app/chamados/models.py:3431: property delega diretamente a pergunta_viva; regra paralela retirada. INFERIU raiz pela autoridade da pergunta. | CHAMADO-EM-RAIA | OBSOLETA |
| Q-VIVAS-LEITORES-SEM-JUIZ | ESPINHA-E | SIM | LEU app/ponto/services/veredito_celula.py:181 e :190: escritor seleciona q_vivas e fecha sem refinar por pergunta_viva. INFERIU raiz pelo mesmo juiz. | CHAMADO-EM-RAIA | DOBRA |
| ESPELHO-VERDADE-REMEDIDO | ESPINHA-E | PARCIAL | LEU app/core/espelho_verdade.py:117 e :141: provas solicitadas foram incorporadas; número R1 em :61 é declaração de medição, execução atual não verificada. | - | CONTA |
| VARREDURA-TRIAGEM | ESPINHA-E | ? | LEU app/config/crons.py:2035 e :2144: migração de papéis existe; segunda medição e tabela completa pedidas em app/docs/BACKLOG.md:1523 não verificadas. INFERIU raiz do mesmo conjunto de crons. | CHAMADO-EM-RAIA | DOBRA |
| BUG-CELULA-SEM-VEREDITO | ESPINHA-E | ? | LEU app/ponto/services/cartorio.py:1409: falta de veredito impede pular célula; alegação de que casos eram testes em app/docs/BACKLOG.md:1524 não é prova do banco atual. INFERIU raiz pelo lote EVENTO citado. | CHAMADO-EM-RAIA | DOBRA |
| BUG-FOLGA-COM-MARCOS | ESPINHA-E | NAO | LEU app/ponto/management/commands/gerar_celulas.py:86 e :88: marcos só nascem quando trabalha é True; folga recebe None. Premissa específica de geração indevida não permanece no código. | DOSSIES-DAS-CELULAS | OBSOLETA |
| C5-LISTA-DE-FROTA | ESPINHA-E | ? | LEU app/ponto/motor_calculo_v2.py:2618 e app/ponto/services/dia_pago.py:274: falta chega por previsão/mapa diário; 27 casos e invariante atual não medidos. INFERIU mãe pelo mesmo defeito. | C5-FALTA-MAIOR-QUE-PREVISTO | DOBRA |
| FERIADO-DIA-CHAVE | ESPINHA-E | SIM | LEU app/ponto/calculador/alimentacao.py:521 versus app/ponto/services/fechamento.py:947: primeiro exige dia k no conjunto de feriados; segundo usa data da entrada e somente feriado_vira_folga. | - | CONTA |
| BUG-117-COMPLEMENTO | ESPINHA-E | NAO | LEU app/chamados/juizes.py:1444: materializada com marco atendido morre; regra de resposta do marco é tratada antes em :1429. LEU complemento em app/docs/BACKLOG.md:1533; INFERIU raiz. | CHAMADO-EM-RAIA | OBSOLETA |
| LISTA-POR-COMPETENCIA | ESPINHA-E | SIM | LEU app/ponto/services/esmeril_espelho.py:55 e :62: janela continua unindo competências abertas; app/escala/services/escala_certa.py:42 calcula assinatura por competência. | - | CONTA |
| O137-ESTA-EXPORTADA-K6 | ESPINHA-E | SIM | LEU app/chamados/services/passivo_validado.py:124 e :127: _esta_exportada mantém predicado próprio sobre exportacoes_da_competencia, sem chamar competencia_entregue. INFERIU raiz K6. | DOSSIES-DAS-CELULAS | DOBRA |
| GEOFENCE-PERGUNTA-AO-JUIZ | ESPINHA-E | NAO | LEU app/ponto/services/geofence.py:431 e :162: reconciliação e caducidade consultam classificar_posicao; migração substituiu distância/raio próprios. | CHAMADO-EM-RAIA | OBSOLETA |
| AUTORIDADE-DE-FOTO-AUSENTE | ESPINHA-E | NAO | LEU app/ponto/management/commands/marcar_foto_ausente_retro.py:61 e :74: comando consulta selfie_ausente pelo canal gravado; autoridade declarada em app/config/crons.py:2159. | CHAMADO-EM-RAIA | OBSOLETA |
| O262 | ESPINHA-E | NAO | LEU app/core/juizes.py:1310 e :1352: duas perguntas têm autoridades e pendentes da escala estão vazios; declaração e migração superaram a célula antiga. | DOSSIES-DAS-CELULAS | OBSOLETA |
| O265 | ESPINHA-E | SIM | LEU app/folha/models.py:293 e app/config/crons.py:1635: retrato segue lavrado pelo comando encadeado; busca dos escritores não encontrou gatilho por dia_encerrado. INFERIU mãe pelo passo 6 do fechamento. | O54 | DOBRA |
| O266 | ESPINHA-E | NAO | LEU app/ponto/management/commands/gerar_celulas.py:283 e app/core/juizes.py:1310: geração consulta autoridade única de cobertura, e vínculo do dia está declarado. | DOSSIES-DAS-CELULAS | OBSOLETA |
| O268 | ESPINHA-E | NAO | LEU app/config/crons.py:2144, :2150, :2154 e :2156: rotinas de documento, ausência e turno têm autoridades externas declaradas; prazo sai do corpo dos crons. | CHAMADO-EM-RAIA | OBSOLETA |
| O276 | ESPINHA-E | NAO | LEU app/ponto/turnos.py:1434 e :1447: ramo S compara span com teto do dia; fallback de 26 h e exceção L-084 estão separados em :1440. | - | OBSOLETA |
| O140-FURO-COMPOSTO | ESPINHA-E | PARCIAL | LEU app/ponto/services/fechamento.py:96 e :99: campos faltantes agora compõem resultado; reapuração e backfill pedidos não verificados. LEU origem comum em app/docs/BACKLOG.md:1603. | O140 | DOBRA |
| TELA-MENOR-QUE-O-GRAVADO | ESPINHA-E | SIM | LEU app/ponto/services/fechamento.py:903 e :985: escritor ainda acrescenta órfãos e folga sem escala ao resultado; propriedade do resultado continua separada. INFERIU mãe do valor diário único. | O-DIA-PAGO | DOBRA |
| FURO-APURADO-FALSO-EM-20378-LINHAS | ESPINHA-E | ? | LEU app/ponto/management/commands/apurar_furo_da_exportada.py:131: recarimbo tem porta; estado das 20.378 linhas e aplicação não fornecidos. LEU mesmo defeito de furo em app/docs/BACKLOG.md:1625. | O140 | DOBRA |
| BO-CARTAO-ESTRELA-X-REALIZADO | ESPINHA-E | ? | LEU app/relatorios/cartao_pela_celula.py:288: cartão obtém órfãs da ata; casos, censos e causa atual não fornecidos. LEU complemento explícito da O256 em app/docs/BACKLOG.md:1629. | O256 | DOBRA |

BLOCO 220–267: 48 linhas, CONTA 8, DOBRA 23, OBSOLETA 17, ? 11.

## 13/11 21:02 · sessao c02f313453b3 · - · - · origem=Ronald (tmux)

| id | balde | existe (SIM/NAO/PARCIAL/?) | prova (arquivo:linha do HEAD ou commit; LEU x INFERIU) | mae | veredito (CONTA/DOBRA/OBSOLETA) | [nao verificado]
|---|---|---|---|---|---|
| O123 | LATERAL-ESTRUTURA | SIM | LEU app/chamados/relatorios_disputa.py:18, app/chamados/relatorios_disputa.py:43 e app/chamados/juizes.py:1405; coleta ainda chama juiz avulso por pergunta; INFERIU persistência do N+1, custo atual [não verificado]. | - | CONTA |
| O118 | LATERAL-ESTRUTURA | PARCIAL | LEU app/escala/utils.py:57 e app/escala/utils.py:77: todas as pausas entram na sequência; app/docs/BACKLOG.md:1233 conserva print pendente, cuja realização atual é [não verificado]. | - | CONTA |
| O103 | LATERAL-ESTRUTURA | PARCIAL | LEU app/ponto/portas/celula.py:773: regeneração limita dia e desliga extensão; app/docs/BACKLOG.md:1248 separa residual TXT; situação atual desse TXT [não verificado]. | O105 | DOBRA |
| O101 | LATERAL-ESTRUTURA | PARCIAL | LEU app/colaboradores/views.py:164 e app/colaboradores/views.py:185: motivo e antes/depois implementados; execução do selo pendente de app/docs/BACKLOG.md:1250 [não verificado]. | - | CONTA |
| O99 | LATERAL-ESTRUTURA | ? | LEU app/ponto/portas/celula.py:741: porta cirúrgica existe; app/docs/BACKLOG.md:1252 lista passivo misto de cadastro/TXT; existência atual dos 61 dias e do residual [não verificado]. | - | CONTA |
| O10 | LATERAL-ESTRUTURA | PARCIAL | LEU app/ponto/portas/celula.py:691, app/ponto/portas/celula.py:702 e app/ponto/portas/celula.py:724: par, duas cartas e trilha implementados; conversão do estoque e entrega integral pela tela [não verificado]. | - | CONTA |
| HE-OCORRENCIA-COMO-LINHA | LATERAL-ESTRUTURA | SIM | LEU app/folha/models.py:348: retrato conserva linhas em JSON; busca por OcorrenciaHE não encontrou modelo/migration; INFERIU ausência da tabela descrita em app/docs/BACKLOG.md:1271. | - | CONTA |
| LOTE-1-PENDENTES | LATERAL-ESTRUTURA | ? | LEU app/docs/BACKLOG.md:1279: pacote de cinco decisões/censos; buscas pelos nomes localizaram portas, mas cumprimento atual dos itens 2–5 [não verificado]. | - | CONTA |
| LOTE-ATRASO-ADMITIDO | LATERAL-ESTRUTURA | SIM | LEU app/chamados/services/validacao.py:891: lote atual usa delta e envelope de tolerância; INFERIU faltar lote próprio com teto por cadastro e DIFF exigido em app/docs/BACKLOG.md:1284. | - | CONTA |
| FECHAMENTO-ZERO-COM-BATIDA | LATERAL-ESTRUTURA | ? | LEU app/ponto/services/espelho.py:971 e app/docs/BACKLOG.md:1285: sítio e casos identificados; vínculos/fechamentos atuais [não verificado]; INFERIU mãe pela classe batida com folha zero de O179. | O179 | DOBRA |
| O15 | LATERAL-ESTRUTURA | NAO | LEU app/pautas/management/commands/pauta_dia_em_aberto.py:26 e app/pautas/management/commands/pauta_dia_em_aberto.py:46: resumo por classe, dez maiores e orientação implementados; app/docs/BACKLOG.md:1294 registra entrega concluída. | - | OBSOLETA |
| O19 | LATERAL-ESTRUTURA | PARCIAL | LEU app/api/views.py:1688 e app/api/views.py:1702: recepção/tamanho registrados antes do parse; lado APK e smoke câmera/arquivo de app/docs/BACKLOG.md:1298 [não verificado], cliente nativo ausente desta cópia. | - | CONTA |
| O28 | LATERAL-ESTRUTURA | SIM | LEU app/ponto/catalogo/ausencias.py:91: saída antecipada permanece SUPRIME; INFERIU persistência da mudança de catálogo pedida, sem afirmar valor do cadastro de produção [não verificado]. | - | CONTA |
| O38 | LATERAL-ESTRUTURA | PARCIAL | LEU app/chamados/services/validacao.py:484 e app/chamados/services/validacao.py:498: validação devolve recusa e estado não gravou; itens de fio/toast agregado e smoke de app/docs/BACKLOG.md:1319 [não verificado]. | - | CONTA |
| O43 | LATERAL-ESTRUTURA | ? | LEU app/core/middleware.py:57 e app/core/middleware.py:85: contexto da trilha/acesso existem; busca por login_falho, sessao_perdida e user_login_failed não localizou executor do rastro completo de app/docs/BACKLOG.md:1324; sem sítio completo. | - | CONTA |
| O47 | LATERAL-ESTRUTURA | NAO | LEU app/core/observ.py:108 e app/core/observ.py:111: evento recebe IP/ator do ContextVar; app/core/middleware.py:78 instala request e app/config/settings/base.py:60 ativa middleware; defeito “service sem request perde IP” curado na origem. | O43 | OBSOLETA |
| O58 | LATERAL-ESTRUTURA | SIM | LEU app/relatorios/services.py:235 e app/relatorios/services.py:250: período livre pula auditável e segue informacional; app/relatorios/views.py:1065 confirma caminho sem auditoria. | - | CONTA |
| O96 | LATERAL-ESTRUTURA | SIM | LEU app/ponto/motor_calculo_v2.py:728, app/ponto/motor_calculo_v2.py:2860 e app/ponto/motor_calculo_v2.py:3512: tetos 3 h, zero e 8 h continuam constantes de classe. | - | CONTA |
| O143 | LATERAL-ESTRUTURA | ? | LEU app/ponto/management/commands/e6_oraculo.py:98: sem vínculo já recebe dono CADASTRO; lista específica, pauta com prazo e censo pedidos em app/docs/BACKLOG.md:1399 não comprovados; INFERIU mãe O125 pela falta de destino administrativo. | O125 | DOBRA |
| O144 | LATERAL-ESTRUTURA | PARCIAL | LEU app/relatorios/views.py:205 e app/relatorios/views.py:275: duas telas usam dias_da_ausencia e limite até hoje; medição chamando ambas e censo de derivações de app/docs/BACKLOG.md:1400 [não verificado]. | - | CONTA |
| O147 | LATERAL-ESTRUTURA | ? | LEU app/docs/BACKLOG.md:1401 e app/ponto/services/espelho.py:971: medição financeira nominal é o pedido; dado raspado não traz atrasos/valores atuais; INFERIU mãe O61 pela diferença monetária da 09 entregue. | O61 | DOBRA |
| O149 | LATERAL-ESTRUTURA | NAO | LEU app/escala/services/cadastro_realidade.py:37 e app/escala/services/cadastro_realidade.py:284: lista C1 publica vigência invertida; app/escala/services/vinculos_inconsistentes.py:30 reconhece mesma classe; entrega é listar, não corrigir cadastro. | O50 | OBSOLETA |
| O151 | LATERAL-ESTRUTURA | ? | LEU app/docs/BACKLOG.md:1405: pedido depende dos treze chamados históricos; busca pelo ID localizou registro, mas dado raspado não informa identidades/vereditos atuais suficientes; estoque [não verificado]. | - | CONTA |
| O157 | LATERAL-ESTRUTURA | SIM | LEU app/ferias/models.py:145: art.130 ainda filtra literal tipo='falta', sem suspensão; app/ferias/services.py:246 consome essa contagem. | - | CONTA |
| O159 | LATERAL-ESTRUTURA | NAO | LEU app/ponto/models.py:877 e app/ponto/models.py:885: falta integral deriva previsto de cada dia; app/ponto/services/ausencia.py:202 registra retirada do literal 720; app/ponto/services/ausencia.py:508 preserva minutos explícitos. | H06-FALTA-PELA-GRADE | OBSOLETA |
| O172 | LATERAL-ESTRUTURA | SIM | LEU app/ponto/services/cartorio.py:471 e app/ponto/services/cartorio.py:480: assinatura/partes não recebem dia julgado; INFERIU mãe CHAVE-DE-ENTRADA-INCOMPLETA pelo mesmo hash incompleto. | CHAVE-DE-ENTRADA-INCOMPLETA | DOBRA |
| O175 | LATERAL-ESTRUTURA | SIM | LEU app/ponto/management/commands/materializar_turnos.py:13 e app/ponto/management/commands/materializar_turnos.py:24: janela padrão 60 dias chama recompute sem guarda de competência exportada no comando. | - | CONTA |
| O181 | LATERAL-ESTRUTURA | NAO | LEU app/ponto/services/fechamento.py:845 e app/ponto/services/fechamento.py:987: trilha nominal permanece INFO; app/config/settings/saas.py:160 e app/config/settings/saas.py:181 agora destinam INFO de ponto a arquivo persistente. | - | OBSOLETA |
| O184 | LATERAL-ESTRUTURA | NAO | LEU app/escala/utils.py:2126: lápide declara remoção de minutos_previstos_periodo pela CASA-ESCALA-E4; busca não localizou definição executável. | O228 | OBSOLETA |
| O198 | LATERAL-ESTRUTURA | NAO | LEU app/ponto/precedencia.py:536: falta consulta escala do dia e recusa jornada False com explicação em app/ponto/precedencia.py:539; app/docs/BACKLOG.md:1453 registra censo publicado. | - | OBSOLETA |
| O199 | LATERAL-ESTRUTURA | PARCIAL | LEU app/chamados/services/validacao.py:435 e app/chamados/services/validacao.py:344: aceite usa porta declarar_dia; estoque antigo explicitamente separado em app/docs/BACKLOG.md:1454; permanência atual do estoque [não verificado]. | - | CONTA |
| O202 | LATERAL-ESTRUTURA | PARCIAL | LEU app/api/views_core.py:934 e app/api/views.py:2405: classificação migrou ao juiz comum; distância segue derivada inline em app/api/views_core.py:920 e app/api/views.py:2387; INFERIU mãe GEOFENCE-PERGUNTA-AO-JUIZ. | GEOFENCE-PERGUNTA-AO-JUIZ | DOBRA |
| O204 | LATERAL-ESTRUTURA | PARCIAL | LEU app/api/hora_do_aparelho.py:72 e app/api/hora_do_aparelho.py:156: parser/contador existem; bin/esmeril_batida_censo.py:68 registra contador sem leitor; linha de placar exigida em app/docs/BACKLOG.md:1459 não comprovada. | - | CONTA |
| O213 | LATERAL-ESTRUTURA | ? | LEU app/chamados/services/materializacao.py:309: sítio confronta tipo pelo motivo; app/docs/BACKLOG.md:1465 diz censo feito, mas resultado executável/artefato e universo atual não estão no dado fornecido; conclusão do censo [não verificado]. | - | CONTA |
| O243 | LATERAL-ESTRUTURA | ? | LEU app/docs/BACKLOG.md:1474; busca por O243/ESMERIL-3-VALIDACAO localizou registro, sem sítio único que prove censo GUARDAS/ANDAIMES e revisão das famílias; sem sítio completo. | - | CONTA |
| O246 | LATERAL-ESTRUTURA | ? | LEU app/chamados/services/validacao.py:344: porta individual já existe; app/docs/BACKLOG.md:1478 nomeia backfill dos aceites de O199; estoque atual e aplicação [não verificado]. | O199 | DOBRA |

TRECHO C: 36 linhas, CONTA 22, DOBRA 7, OBSOLETA 7, ? 10.

## 13/11 21:02 · sessao 2ec6bd1b6668 · - · - · origem=Ronald (tmux)

| id | balde | existe (SIM/NAO/PARCIAL/?) | prova (arquivo:linha do HEAD ou commit; LEU x INFERIU) | mae | veredito (CONTA/DOBRA/OBSOLETA) | [nao verificado]
|---|---|---|---|---|---|
| SNC2-FILA | LATERAL-ESTRUTURA | SIM | LEU app/chamados/juizes.py:592: relógio interno permanece; app/core/utils.py:18: get_or_create permanece. Cobertura restante [não verificado]. | - | CONTA |
| F-346 | LATERAL-ESTRUTURA | PARCIAL | LEU app/ferias/services.py:466: remarcação usa escritor único; :438 ainda consulta relógio interno. INFERIU mãe pelo mesmo escritor abrangido em app/docs/BACKLOG.md:1874. | HJ-090 | DOBRA |
| H12H06-SEPARA-E-CRONTAB | LATERAL-ESTRUTURA | PARCIAL | LEU app/ponto/management/commands/rederivar_minutos_h06_comp11.py:169: parte B construída; aplicação e instalação do crontab [não verificado]. Raiz A+B: app/docs/BACKLOG.md:1827. | - | CONTA |
| H12H06-B-SIMPLES | LATERAL-ESTRUTURA | PARCIAL | LEU app/ponto/management/commands/rederivar_minutos_h06_comp11.py:50: lista fechada; :169: escritor existente. Apply e segunda rodada [não verificado]. INFERIU raiz A+B em app/docs/BACKLOG.md:1827. | H12H06-SEPARA-E-CRONTAB | DOBRA |
| H12H06-B-COMPLEMENTO | LATERAL-ESTRUTURA | PARCIAL | LEU app/ponto/services/fechamento.py:694, app/ponto/services/efeito_ausencia.py:48 e app/ponto/services/credito_parcial.py:27: leitores usam juiz. Sonda comp10 e portão export11 [não verificado]; complemento explícito em app/docs/BACKLOG.md:1829. | H12H06-SEPARA-E-CRONTAB | DOBRA |
| LEI-H12H06-B-A | LATERAL-ESTRUTURA | PARCIAL | LEU app/ponto/management/commands/rederivar_minutos_h06_comp11.py:169 e app/pautas/management/commands/pauta_pago_x_devido_exportada.py:207: portas construídas; aplicação e pautas emitidas [não verificado]. INFERIU raiz pelo escopo em app/docs/BACKLOG.md:1830. | H12H06-SEPARA-E-CRONTAB | DOBRA |
| H12H06-B-PAUTA-08 | LATERAL-ESTRUTURA | PARCIAL | LEU app/pautas/management/commands/pauta_pago_x_devido_exportada.py:207: grava pauta pago/devido; execução dos casos [não verificado]. Parte explícita em app/docs/BACKLOG.md:1831. | H12H06-SEPARA-E-CRONTAB | DOBRA |
| PAUTA-08-FILA-OPUS | LATERAL-ESTRUTURA | SIM | LEU app/pautas/management/commands/pauta_pago_x_devido_exportada.py:188: autor obtido antes do atomic; :273: reversão percorre pautas separadamente. INFERIU residual da pauta indicada em app/docs/BACKLOG.md:1832. | H12H06-SEPARA-E-CRONTAB | DOBRA |
| H12H06-A-FILA | LATERAL-ESTRUTURA | PARCIAL | LEU app/folha/views.py:658: rótulo depende só de data_demissao; app/api/tests/test_falta_na_resposta.py:55 já consulta jornada. Residual explícito em app/docs/BACKLOG.md:1834. | H12H06-SEPARA-E-CRONTAB | DOBRA |
| TIPO-ITEM3-NAOBLOQ-V5C | LATERAL-ESTRUTURA | SIM | LEU app/escala/utils.py:1887: ata alternativa sem n_orfas; app/ponto/services/vigia_pareador_celula.py:73: None vira dicionário vazio. INFERIU raiz pelo programa citado em app/docs/BACKLOG.md:1837. | TIPO-NA-FONTE | DOBRA |
| DIA-SEM-TRABALHO-PORTA | LATERAL-ESTRUTURA | ? | LEU app/docs/BACKLOG.md:1848: pede porta administrativa sem nomear arquivo/função; sítio específico da decisão não localizado [não verificado]. | - | CONTA |
| AUSENCIA-UMA-VIGENTE | LATERAL-ESTRUTURA | SIM | LEU app/folha/export.py:140: percorre faltas e :142 soma; app/ponto/services/fechamento.py:683 consulta cobertura única. INFERIU persistência de dois critérios; quatro casos atuais [não verificado]. | - | CONTA |
| PAINEL-SITUACIONAL-UM-MONTADOR | LATERAL-ESTRUTURA | SIM | LEU app/colaboradores/painel_cell.py:81: célula chama montar_linha_colab; app/colaboradores/services/situacional.py:65: montador separado permanece. | - | CONTA |
| BUG-PLANTIO-NOTURNO-MAIS-1 | LATERAL-ESTRUTURA | PARCIAL | LEU app/chamados/juizes.py:296: recebe DiaDoTurno; :326: data pela cadeia de marcos. Migração do passivo [não verificado]. Mãe explícita em app/docs/BACKLOG.md:1978. | FIO-E0-V4-E1-ENTRADAS | DOBRA |
| PUSH-DISPUTA-SEM-PERGUNTA | LATERAL-ESTRUTURA | SIM | LEU app/chamados/services/disputa_emissao.py:670: gera perguntas; :671: push independente do retorno. INFERIU mesma classe sem pergunta de BO-DISPUTA-SEM-PERGUNTA; app/docs/BACKLOG.md:1853. | BO-DISPUTA-SEM-PERGUNTA | DOBRA |
| ESMERIL-SEM-INSTANTE | LATERAL-ESTRUTURA | PARCIAL | LEU app/ponto/services/fechamento.py:253: filtro já usa dia local preparado; app/ponto/services/credito_parcial.py:36 ainda extrai data do instante. Migração dos onze campos [não verificado]; escopo em app/docs/BACKLOG.md:1857. | - | CONTA |
| ESMERIL-SEM-DIA-DO-TURNO | LATERAL-ESTRUTURA | SIM | LEU app/ponto/services/triagem_batida.py:260: dia por entrada.date(); app/folha/previa.py:65: data_evento cru. INFERIU mesma unificação de leitores da obra DIA-DA-BATIDA-JUIZ-UNICO; app/docs/BACKLOG.md:1858. | DIA-DA-BATIDA-JUIZ-UNICO | DOBRA |
| ESMERIL-SEM-BORDA | LATERAL-ESTRUTURA | ? | LEU app/docs/BACKLOG.md:1859 e app/ponto/services/ausencia.py:523: há tratamento de fim vazio; célula reúne várias bordas sem apontar divergência concreta. Defeito remanescente [não verificado]. | - | CONTA |
| ESMERIL-SEM-DIAS-PREVISTOS | LATERAL-ESTRUTURA | SIM | LEU app/ponto/services/fechamento.py:1047: inclui tipo ausencia; :1049: conta todos esses dias sem exigir trabalha=True. INFERIU complemento do contrato O155, citado em app/docs/BACKLOG.md:1860. | O155 | DOBRA |
| V2-BUGS-DO-MOTOR | LATERAL-ESTRUTURA | PARCIAL | LEU app/ponto/motor_calculo_v2.py:1014: split recebe pontas brutas; item2 usa he_noturna. Demais itens e aplicações [não verificado]; escopo múltiplo em app/docs/BACKLOG.md:1862. | - | CONTA |
| ESMERIL-SEM-POLITICA (SILENCIADO-ATE-E-PRAZO) | LATERAL-ESTRUTURA | SIM | LEU app/chamados/services/verbos.py:51: vencido vira HISTORICO; app/chamados/services/painel.py:190: filtro só exclui prazo futuro. app/chamados/services/juizes_do_dia.py:75 recebe datas sem consumi-las no corpo. | - | CONTA |
| LOTE-VALIDAR-VIRA-JOB | LATERAL-ESTRUTURA | SIM | LEU app/chamados/views_cobrar.py:381: valida lote diretamente no POST; :386: renderiza resultado na mesma requisição. | - | CONTA |
| FERIAS-SALDO-SEM-TRAVA | LATERAL-ESTRUTURA | SIM | LEU app/ferias/services.py:299 e :324: alteram saldo sem travar período; :504: aprovação também. Absorção explícita em app/docs/BACKLOG.md:1873. | HJ-090 | DOBRA |
| HJ-090 | LATERAL-ESTRUTURA | SIM | LEU app/ferias/services.py:277: atomic sem trava do período; :324 incrementa objeto recebido; :504: outro escritor sem atomic. Necessidade de alinhar saldo permanece. | - | CONTA |
| JUSTIFICATIVA-PAGA-O-PERIODO | LATERAL-ESTRUTURA | SIM | LEU app/ponto/services/efeito_ausencia.py:40: justificativa vira conjunto de dias; :47: minutos parciais só de Ausencia. INFERIU parte financeira do HJ-093, vínculo explícito em app/docs/BACKLOG.md:1875. | HJ-093 | DOBRA |
| FERIAS-2X-FILA | LATERAL-ESTRUTURA | SIM | LEU app/ferias/services.py:438: relógio interno; :602: literal cancelado; app/ponto/services/ausencia.py:1181: regra de status repetida. INFERIU mesma ampliação de escritores de HJ-090 em app/docs/BACKLOG.md:1874. | HJ-090 | DOBRA |
| V2-I2-FILA | LATERAL-ESTRUTURA | SIM | LEU app/ponto/motor_calculo_v2.py:1015: entrada/saída brutas; app/ponto/services/esmeril_espelho.py:175: conta períodos por dia civil. Residual explícito de item2 em app/docs/BACKLOG.md:1877. | V2-BUGS-DO-MOTOR | DOBRA |
| V2-I2-COL270-DONO | LATERAL-ESTRUTURA | ? | LEU app/docs/BACKLOG.md:1878: investigação de mudança histórica do previsto; app/ponto/services/fechamento.py:1048 calcula previsto atual, sem reconstruir ata antiga. Causa e situação atual [não verificado]. INFERIU residual V2 pelo registro. | V2-BUGS-DO-MOTOR | DOBRA |
| HOLERITE-ZIP-MATRICULA | LATERAL-ESTRUTURA | SIM | LEU app/holerite/views.py:1217: upload_lote permanece; :1246: casa por matricula_nexti. Uso atual [não verificado]. | - | CONTA |
| HOLERITE-CAMPO-MATCH-LIVRE | LATERAL-ESTRUTURA | SIM | LEU app/holerite/views.py:1741: campo_match vindo do POST; app/holerite/motor.py:218: filtro dinâmico. Complemento explícito em app/docs/BACKLOG.md:1884. | HOLERITE-ZIP-MATRICULA | DOBRA |
| E4B-D2-ANCORA-RAIOX | LATERAL-ESTRUTURA | SIM | LEU app/chamados/services/raiox.py:146: destino só vence se vivo; app/chamados/services/fio.py:117: destino vence sem esse teste. INFERIU residual E4b registrado em app/docs/BACKLOG.md:1904. | FIO-E0-V4-E4B | DOBRA |
| E4B-D3-ARQUIVAR-PRESCRITOS | LATERAL-ESTRUTURA | ? | LEU app/config/crons.py:1453: comando desligado; app/chamados/management/commands/arquivar_prescritos.py:37: instrumento existe. Pendências sem dono pedidas em app/docs/BACKLOG.md:1905 [não verificado]. INFERIU residual E4b. | FIO-E0-V4-E4B | DOBRA |
| INTERMITENTE-SEM-CONVOCACAO | LATERAL-ESTRUTURA | SIM | LEU app/escala/models.py:820: ciclo intermitente; :823: retorna False sempre. INFERIU mesmo cadastro da convocação da obra INTERMITENTE-CONVOCACAO-CADASTRO; app/docs/BACKLOG.md:1914. | INTERMITENTE-CONVOCACAO-CADASTRO | DOBRA |
| E4B-FILA-OPUS-C14 | LATERAL-ESTRUTURA | SIM | LEU app/chamados/services/disputa_emissao.py:1835: recusa considera exportadas de toda carga; :1948: aceita recusa por limbo para encaminhamento. Residual explícito em app/docs/BACKLOG.md:1926; conformidade fina [não verificado]. | FIO-E0-V4-E4B | DOBRA |
| BO-0611-05 | LATERAL-ESTRUTURA | PARCIAL | LEU app/api/views_core.py:422: GPS ausente segue; app/ponto/registro_batida.py:150 ainda pode recusar colisão automática. Smoke e classe integral [não verificado]; escopo em app/docs/BACKLOG.md:1928. | - | CONTA |
| BO-0611-05-COMPLEMENTO-POUSO | LATERAL-ESTRUTURA | PARCIAL | LEU app/api/views_core.py:397: recusa por situação permanece; app/docs/BACKLOG.md:1932 registra complemento de bateria/smoke. Realização completa do smoke [não verificado]. | BO-0611-05 | DOBRA |
| BO-0611-05-DESLIGADO-FERIAS | LATERAL-ESTRUTURA | SIM | LEU app/api/views_core.py:395: situação pode impedir registro; regra financeira substituta [não verificado]. Complemento explícito de O-PROJECOES em app/docs/BACKLOG.md:1933. | O-PROJECOES | DOBRA |
| REGULARIZAR-TRAVA-PELO-DIA | LATERAL-ESTRUTURA | SIM | LEU app/chamados/services/regularizacao_ext.py:59: disputa aberta filtrada só por colaborador; :62: bloqueia inteira. Complemento explícito da RAIZ em app/docs/BACKLOG.md:1936. | RAIZ-DA-DISPUTA | DOBRA |
| SAIDA-NOTURNA-NO-DIA-CERTO | LATERAL-ESTRUTURA | SIM | LEU app/chamados/services/materializacao.py:419: combina hora com data_evento; app/chamados/juizes.py:330 mantém data fora do envelope. INFERIU mesma família dia da pergunta/turno; caso concreto atual [não verificado], app/docs/BACKLOG.md:1938. | DIA-DA-PERGUNTA-E-O-TURNO | DOBRA |
| DECLARADO-X-GRAVADO | LATERAL-ESTRUTURA | SIM | LEU app/chamados/services/materializacao.py:412: parte da resposta; :414: procura marco; :416: substitui hora declarada. | - | CONTA |
| DECLARADO-X-GRAVADO-PASSIVO-11 | LATERAL-ESTRUTURA | ? | LEU app/chamados/services/materializacao.py:416: origem ainda substitui resposta; existência atual do passivo [não verificado]. INFERIU parte do mesmo defeito em app/docs/BACKLOG.md:1941, embora registrada como execução própria. | DECLARADO-X-GRAVADO | DOBRA |
| BO-0611-05-RESTO-DA-CLASSE | LATERAL-ESTRUTURA | SIM | LEU app/ponto/registro_batida.py:145 e :151: tipo oposto ainda pode ser recusado; janela mesmo-tipo em :113. Complemento explícito em app/docs/BACKLOG.md:1958; raiz da classe BO-0611-05. | BO-0611-05 | DOBRA |
| APP-MENSAGENS-MSG-0 | LATERAL-ESTRUTURA | SIM | LEU app/api/views.py:1955: busca cobrança por contexto cru; app/ponto/views.py:1617: usa dia_do_fato; escrita duplicada em app/api/views.py:1992 e app/ponto/views.py:1635. INFERIU parte do programa APP-MENSAGENS; escopo em app/docs/BACKLOG.md:1960. | APP-MENSAGENS (MSG-1..4) | DOBRA |
| BATIDA-ID-DO-CLIQUE | LATERAL-ESTRUTURA | SIM | LEU app/ponto/registro_batida.py:113: dedup por tempo/tipo; :120: por colaborador/instante. Id estável do clique não participa dessa porta; necessidade registrada em app/docs/BACKLOG.md:1966. | - | CONTA |
| BO-0611-05-TEM-GPS-LEITORES | LATERAL-ESTRUTURA | SIM | LEU app/ponto/services/geofence.py:145: juiz usa truthiness; :52: outro leitor aceita coordenadas zero por isnull=False. Complemento de O254 explícito em app/docs/BACKLOG.md:1974; INFERIU raiz contratual. | O254 | DOBRA |

Subtotal: 45 linhas, CONTA 15, DOBRA 30, OBSOLETA 0, ? 5.

## 13/11 21:01 · sessao 6b4a93631265 · - · - · origem=Ronald (tmux)

| id | balde | existe (SIM/NAO/PARCIAL/?) | prova (arquivo:linha do HEAD ou commit; LEU x INFERIU) | mae | veredito (CONTA/DOBRA/OBSOLETA) | [nao verificado]
|---|---|---|---|---|---|
| JUIZ-UNICO-DA-ANCORA | LATERAL-ESTRUTURA | SIM | LEU app/escala/services/fase_12x36.py:58, app/escala/services/fase_na_tela.py:78, app/ponto/management/commands/censo_fase_12x36.py:203: continuam decisões distintas de âncora. | - | CONTA |
| FOLGA-TRABALHADA-RESOLVE | LATERAL-ESTRUTURA | SIM | LEU app/ponto/services/veredito_celula.py:243: “trabalhou” ainda recusa dia que não seja de trabalho; INFERIU mãe pelo mesmo ato sobre folga, app/docs/BACKLOG.md:1848. | DIA-SEM-TRABALHO-PORTA | DOBRA |
| BO-[nome]-DESTRAVA | LATERAL-ESTRUTURA | SIM | LEU app/folha/retrato_folha.py:1189 e :1069: lavrador conserva fio por grupo; drawer escolhe DESTRAVA por fio, sem dono L-099 do dia. | - | CONTA |
| FIO-DO-ADMIN-LAVRA-VELHA | LATERAL-ESTRUTURA | SIM | LEU app/chamados/services/vigia_fio.py:216 e :228: inclusão depende da presença e do dono da lista, sem conferir idade da lavra nessa porta. | - | CONTA |
| AVISO-ATIVO-SEM-VINCULO-NO-DIA | LATERAL-ESTRUTURA | SIM | LEU app/inteligencia/management/commands/gerar_alertas.py:113: alerta consulta ausência de qualquer escala; INFERIU insuficiência para lacuna de vigência no dia. | - | CONTA |
| ANALISAR-RELE-O-BANCO | LATERAL-ESTRUTURA | SIM | LEU app/chamados/models.py:850: analisar decide por self.status_local sem recarga; vínculo da classe com a mãe explicitado em app/docs/BACKLOG.md:1014. | ADMIN-RESOLVE-FLAP | DOBRA |
| REABRIR-DISPUTA-ADMIN-IGNORA-RETORNO | LATERAL-ESTRUTURA | SIM | LEU app/chamados/services/acoes_disputa.py:259, :270 e :278: reabre disputa, descarta retorno de ch.reabrir e anuncia reabertura. | - | CONTA |
| DIAS-ENCERRADOS-CHAVE-DO-CARTORIO | LATERAL-ESTRUTURA | SIM | LEU app/ponto/turnos.py:2147 e :2150: repassa células com chave dupla; contrato de turnos é chave simples em :1854; mesma obra descrita em app/docs/BACKLOG.md:933. | BUG-ENC-CELULAS-FORMA | DOBRA |
| VETO-NA-PORTA-POR-ELIMINACAO | LATERAL-ESTRUTURA | SIM | LEU app/ponto/management/commands/detectar_ausencias.py:738 e app/ponto/management/commands/reconciliar_grade.py:527: None continua denominado VETO_NA_PORTA; INFERIU complemento da porta descrita em app/docs/BACKLOG.md:1026. | O261-VETO-NA-PORTA | DOBRA |
| CXR-FALHA-CALADA | LATERAL-ESTRUTURA | SIM | LEU app/escala/services/cadastro_realidade.py:779 e :796: falha registra log e omite os itens daquele leitor da lista. | - | CONTA |
| FASE-12X36-SEM-PAPEL-DA-ATA | LATERAL-ESTRUTURA | NAO | LEU app/escala/services/fase_12x36.py:68: lápide de fora_de_fase; antigo pareamento retirado, leitores passam à lavra do censo em :74. | - | OBSOLETA |
| PLANTIO-ENVELOPE-PELO-GERADOR | LATERAL-ESTRUTURA | NAO | LEU app/chamados/juizes.py:320: marcos_turno recebe ec; app/escala/servico_jornada.py:77 chama ec.marcos_do_dia; app/escala/models.py:1124 lê marcos do DNA congelado. | - | OBSOLETA |
| COL309-SETEMBRO | LATERAL-ESTRUTURA | ? | LEU app/docs/BACKLOG.md:1027: registra células aplicadas e censo da classe; aplicação e estado atual do passivo [não verificado], sem banco ou prova raspada posterior. | - | CONTA |
| COL309-09-RECALCULO-DEPOIS-DO-ADMIN | LATERAL-ESTRUTURA | ? | LEU app/docs/BACKLOG.md:1028: recálculo condicionado a quatro decisões; decisões e recálculo atuais [não verificado]; residual encaminhado por app/docs/BACKLOG.md:1027. | COL309-SETEMBRO | DOBRA |
| CAL-PREDICADO-D-MENOR-HOJE-2-COPIAS | LATERAL-ESTRUTURA | NAO | LEU app/colaboradores/services/calendario.py:597 e :606: antiga consulta própria à retenção saiu; calendário lê status_dos_dias_lavrados. | - | OBSOLETA |
| CAL-CUSTO-NO-MAPA-DE-FUROS | LATERAL-ESTRUTURA | ? | LEU app/relatorios/furos.py:160 e app/colaboradores/services/calendario.py:606: permanece montagem por card, agora com D4; custo das consultas e redução efetiva [não verificado]. | - | CONTA |
| CAL-DOIS-RELOGIOS | LATERAL-ESTRUTURA | SIM | LEU app/colaboradores/services/calendario.py:163 e :250: duas leituras de timezone.now na mesma montagem; INFERIU parte da unificação do instante em app/docs/BACKLOG.md:1057. | RELOGIO-INJETADO | DOBRA |
| ENTRADA-BLOQUEADA-SEM-DATA-TURNO | LATERAL-ESTRUTURA | SIM | LEU app/ponto/views.py:280 e :301: contexto contém pendentes e total, sem data_turno, e é passado à criação do chamado. | - | CONTA |
| POSICAO-TOTAL-ATE-A-DATA | LATERAL-ESTRUTURA | SIM | LEU app/ponto/services/dia_pago.py:752 e :755: soma do intervalo exclui ajuste de competência, DSR e banco; INFERIU complemento da fonte diária da obra app/docs/BACKLOG.md:1131. | O-DIA-PAGO | DOBRA |
| POSICAO-FALTAS-E-AUSENCIAS | LATERAL-ESTRUTURA | ? | LEU app/ponto/services/espelho.py:582 e :588: há fontes separadas por efeito; fonte que reúna todos os tipos e faltas até o corte [não verificado], sem sítio próprio localizado. | - | CONTA |
| POSICAO-FERIAS-ATE-A-DATA | LATERAL-ESTRUTURA | SIM | LEU app/ferias/situacao.py:146 e :162: saldo reúne períodos adquiridos cadastrados; INFERIU que essa função não entrega avos proporcionais até o corte. | - | CONTA |
| POSICAO-PORTA-POR-MOTIVO | LATERAL-ESTRUTURA | SIM | LEU app/folha/fora_da_folha.py:234: retorno tem fio, destrava e chamados, sem URL ou pergunta_id; INFERIU complemento do canal obrigatório em app/docs/BACKLOG.md:1351. | O66 | DOBRA |
| VIGENCIA-FUTURA | LATERAL-ESTRUTURA | PARCIAL | LEU app/colaboradores/services/vinculo.py:54 e :197: aceita início e imediatamente fecha vínculos anteriores; agenda completa de posto/fase [não verificado]; INFERIU parte da linha temporal descrita em app/docs/BACKLOG.md:1127. | VINCULO-LINHA-DO-TEMPO | DOBRA |
| BO2910-ESTRUTURA | LATERAL-ESTRUTURA | PARCIAL | LEU app/colaboradores/views.py:327 e :329: vincular_posto ainda grava só posto; app/ponto/portas/celula.py:1061 conserva decisão admin; conjunto original tem classes distintas em app/docs/BACKLOG.md:1059. | - | CONTA |
| PORTAO-DE-FATURAMENTO | LATERAL-ESTRUTURA | PARCIAL | LEU app/ponto/portas/celula.py:572: edição humana existe, mas recebe só trabalha; marcos seguem template do vínculo (:549), sem turno excepcional; INFERIU dobra pela ligação explícita em app/docs/BACKLOG.md:1061. | PORTA-CELULA-EDITADA | DOBRA |
| [nome]-NOTURNO-DIA-CIVIL | LATERAL-ESTRUTURA | ? | LEU app/chamados/models.py:2832: nascimento atual exige dia do turno; estoque de perguntas e batidas em dia civil errado [não verificado]; INFERIU dobra da classe em app/docs/BACKLOG.md:1025 e :1064. | DIA-DA-PERGUNTA-E-O-TURNO | DOBRA |
| GRADE-DEPENDE-DA-JANELA | LATERAL-ESTRUTURA | ? | LEU app/escala/utils.py:2084: há dois construtores conforme configuração; divergência atual entre janelas nos casos registrados em app/docs/BACKLOG.md:1068 [não verificado]. | - | CONTA |
| DETECTOR-SO-A-1A-PAUSA | LATERAL-ESTRUTURA | SIM | LEU app/chamados/detector_anomalias.py:199 e app/escala/servico_jornada.py:83: detector recebe apenas hii/hfi; leitor integra expressamente grupo da mãe em app/docs/BACKLOG.md:812. | JUIZ-DO-PAR-GRUPO-1 | DOBRA |
| PAUTAS-PREFILTRO-REPETIDO | LATERAL-ESTRUTURA | SIM | LEU app/ponto/management/commands/pautas_discordantes.py:98 e :109: pré-filtro chama dono_da_divergencia e depois consulta _ins.dono novamente. | - | CONTA |
| OUTBOX-EVENTO-DUPLO | LATERAL-ESTRUTURA | ? | LEU app/escala/models.py:1851: vários chamadores da porta; não há unicidade pendente por desenho (:1849); repetição atual por ato e medição de 24 h [não verificado]. | - | CONTA |
| JUSTIFICATIVA-ENCERRA-ORIGEM | LATERAL-ESTRUTURA | PARCIAL | LEU app/ponto/services/justificativa.py:46 e :217: aprovação retrata origem dentro do ato; retenção segue permitida (:45); situação dos cinco resíduos de app/docs/BACKLOG.md:1074 [não verificado]. | - | CONTA |
| PASSIVO-VIVA-EM-DISPUTA-FECHADA | LATERAL-ESTRUTURA | PARCIAL | LEU app/chamados/services/validacao.py:492: ainda passa perg.disputa à materialização; nove disputas atuais [não verificado]; complemento explícito de PV-MUDA em app/docs/BACKLOG.md:1078. | JUIZ-PERGUNTA-VIVA-MUDA-SEM-FATO | DOBRA |
| DESLIGAMENTO-NAO-CARIMBA-RESPONDIDA | LATERAL-ESTRUTURA | PARCIAL | LEU app/chamados/models.py:1942: resposta sem veredito recusa fechamento; :1974 conserva admin_decidiu no desligamento, sem via própria sucessora; lei substituta e raiz em app/docs/BACKLOG.md:1079, :731 e :1332; passivo [não verificado]. | O50 | DOBRA |
| O210-COL417-COL931 | LATERAL-ESTRUTURA | ? | LEU app/docs/BACKLOG.md:1081: complemento explícito da O210; fechamentos atuais dos dois colaboradores e recálculo [não verificado], sem banco ou novo dado raspado. | O210 | DOBRA |
| LOTE-PLANTA-MARCO-AVULSO-E-ABRE-TURNO | LATERAL-ESTRUTURA | PARCIAL | LEU app/chamados/services/passivo_validado.py:518 e :542: guarda compara turnos sem par antes/depois e desfaz piora; quatro resíduos registrados em app/docs/BACKLOG.md:1084, estado atual [não verificado]. | - | CONTA |
| LOTE-AGRUPA-POR-DIA-E-RASGA-O-PAR-DA-MEIA-NOITE | LATERAL-ESTRUTURA | PARCIAL | LEU app/chamados/services/passivo_validado.py:476: agrupa pelo dia do plantio; fonte afirma dia da célula em :469; remedição dos 51/32 [não verificado]; residual explícito da guarda em app/docs/BACKLOG.md:1085. | LOTE-PLANTA-MARCO-AVULSO-E-ABRE-TURNO | DOBRA |

SUBFAIXA A: 36 linhas, CONTA 16, DOBRA 17, OBSOLETA 3, ? 9.

## 13/11 21:01 · sessao 171076a2ecc4 · - · - · origem=Ronald (tmux)

| id | balde | existe | prova | mae | veredito |
|---|---|---|---|---|---|
| COL30-FORA-DOS-TRES-CONJUNTOS-DA-FOLHA | LATERAL-ESTRUTURA | ? | LEU app/folha/management/commands/simular_folha.py:133 e :152: monta os três conjuntos; presença atual de col30 [não verificado], sem banco | - | CONTA |
| ABRANGENCIA-DOS-QUATRO-INSTRUMENTOS-SEM-PDF | LATERAL-ESTRUTURA | ? | LEU app/core/models.py:38 e :40: cadastro de vigência/observação; PDFs e enquadramento atual [não verificado], sem sítio nomeado disponível | - | CONTA |
| PERCENTUAL-DO-NOTURNO-SEM-LEITOR | LATERAL-ESTRUTURA | SIM | LEU app/core/regua_cct.py:73: percentual permanece declarado sem efeito no cálculo; campo em app/core/models.py:45 | - | CONTA |
| JANELA-DO-NOTURNO-CRAVADA | LATERAL-ESTRUTURA | SIM | LEU app/ponto/motor_calculo_v2.py:44 e :45: início 22h/fim 05h constantes | - | CONTA |
| EXCEDENTE-IGNORA-O-SINDICATO | LATERAL-ESTRUTURA | SIM | LEU app/core/regua_cct.py:378: sobrescreve regua_excedente com legais | - | CONTA |
| TOLERANCIA-SEM-LEITOR | LATERAL-ESTRUTURA | SIM | LEU app/core/regua_cct.py:74 e :75: tolerâncias seguem declaradas sem consumo; cadastro em app/core/models.py:59 | - | CONTA |
| INTERVALO-MINIMO-SEM-LEITOR | LATERAL-ESTRUTURA | SIM | LEU app/core/regua_cct.py:76: intrajornada mínima permanece sem efeito, regra fixa; campo em app/core/models.py:63 | - | CONTA |
| TETO-SEMANAL-12X36-INEXISTENTE | LATERAL-ESTRUTURA | SIM | LEU app/core/models.py:59 e :70: cadastro tem tolerâncias e teto diário; INFERIU ausência de teto semanal no modelo Sindicato inspecionado | - | CONTA |
| RUBRICA-DE-PRONTIDAO-INEXISTENTE | LATERAL-ESTRUTURA | SIM | LEU app/ponto/models.py:961 e :977: parâmetros/rubricas existentes; INFERIU ausência da regra de prontidão no cadastro inspecionado | - | CONTA |
| FURO-APURADO-FALSE-EM-VEZ-DE-NONE | LATERAL-ESTRUTURA | SIM | LEU app/ponto/services/dia_pago.py:346: lista fornecida produz booleano por pertinência; app/ponto/services/fechamento.py:377 exclui dias não encerrados da lista | - | CONTA |
| TRES-GRAFIAS-DE-HOJE-MENOS-UM | LATERAL-ESTRUTURA | PARCIAL | LEU app/escala/services/furos_diarios.py:32 e :109: teto continua hoje menos um; demais dois sítios originalmente indicados mudaram, equivalência integral [não verificado]. Raiz LEU app/docs/BACKLOG.md:1480 | O258 | DOBRA |
| BO-HE-RECUSAR-LOTE-500 | LATERAL-ESTRUTURA | PARCIAL | LEU app/ponto/portas/he.py:137: atomic voltou à porta decidir_he; smoke exigido ainda pendente no dado fornecido, execução atual [não verificado] | - | CONTA |
| SEM-CELULA-11-DEVE-TER-CELULA | LATERAL-ESTRUTURA | PARCIAL | LEU app/ponto/portas/celula.py:883: universo sem filtro de situação; col305 continua pendente no dado fornecido, aplicação atual [não verificado] | - | CONTA |
| ORIGEM-DISPUTA-S84-RETRO | LATERAL-ESTRUTURA | SIM | LEU app/ponto/models.py:13: catálogo não inclui disputa_s84_retro; app/chamados/services/materializacao.py:934 ainda escreve esse valor | - | CONTA |
| REJULGAR-4-EXPORTADAS | LATERAL-ESTRUTURA | PARCIAL | LEU app/ponto/services/cartorio.py:1289: ramo vigente distingue cobertura/sem trabalho; dado fornecido conserva um resíduo encaminhado; sua cura no passivo [não verificado]. Relação LEU app/docs/BACKLOG.md:1673 | FALTA-EM-FOLGA-VIRA-NUNCA-BATEU | DOBRA |
| FALTA-EM-FOLGA-VIRA-NUNCA-BATEU | LATERAL-ESTRUTURA | PARCIAL | LEU app/ponto/services/cartorio.py:1255 e :1289: dia sem trabalho agora conserva veredito derivado; classe antiga/passivo rejulgado [não verificado] | - | CONTA |
| O249-RETRATADOR-NO-DIA-IMPERFEITO | LATERAL-ESTRUTURA | SIM | LEU app/ponto/management/commands/detectar_par_relampago.py:244: candidata continua exigindo geometria perfeita. Complemento O249 e raiz O258 LEU app/docs/BACKLOG.md:1692 e :1481 | O258 | DOBRA |
| SILENCIO-SEM-PRAZO | LATERAL-ESTRUTURA | SIM | LEU app/chamados/services/arquivo.py:160: aceita ate fornecido sem limite superior neste sítio; estoque atual de silêncios longos [não verificado] | - | CONTA |
| HE100-SEM-PREVISTO-SOZINHA | LATERAL-ESTRUTURA | PARCIAL | LEU app/ponto/motor_calculo_v2.py:1425 e :1456: dia sem previsto consulta autorização; dado fornecido conserva resíduo de 75 dias, tratamento atual [não verificado] | - | CONTA |
| PERIODO-RELER-SEM-CHAMADOR | LATERAL-ESTRUTURA | SIM | LEU app/ponto/calculador/periodo.py:434 e :543: pontualidade_no_dia e reler permanecem; censo atual de todos os chamadores [não verificado] | - | CONTA |
| ESMERIL-EXIGE-MEMOS-NO-TRY | LATERAL-ESTRUTURA | SIM | LEU app/ponto/services/esmeril_espelho.py:159 e :163 e :167: exige_memos dentro do try com exceção convertida em r=None | - | CONTA |
| C2-REVISAO-SO-PELO-POSTO | LATERAL-ESTRUTURA | SIM | LEU app/ponto/services/vinculo_na_lista.py:152 e :154: revisão compara exclusivamente posto_id; mãe INFERIU pelo mesmo leitor C8/C2, app/docs/BACKLOG.md:1680 | CASA-CHAMADO-C2 | DOBRA |
| PAR-COMPLETO-ROTA-A | LATERAL-ESTRUTURA | SIM | LEU app/ponto/turnos.py:2186 e :2187: completude cruza dias de turno; rota a em app/ponto/management/commands/detectar_par_relampago.py:121. Raiz LEU app/docs/BACKLOG.md:1481 | O258 | DOBRA |
| C2-OS-19-SEM-PRAZO | LATERAL-ESTRUTURA | SIM | LEU app/ponto/services/vinculo_na_lista.py:147 e :154: universo vem da lavra de revisão; resíduo fora dela não ganha contador nesse sítio. Estoque 19 atual [não verificado]; mãe INFERIU app/docs/BACKLOG.md:1680 | CASA-CHAMADO-C2 | DOBRA |
| LAUDO-col33-0310 | LATERAL-ESTRUTURA | ? | LEU app/ponto/services/cartorio.py:1264: carimbo vigente pode pular rejulgamento; dado fornecido só confirma universo A e restante sem corte, persistência atual [não verificado] | - | CONTA |
| SELFIE-RETRO-SEM-ULTIMA-BATIDA | LATERAL-ESTRUTURA | SIM | LEU app/ponto/management/commands/marcar_foto_ausente_retro.py:100: contexto grava batida_id; app/api/foto_recebida.py:67 procura ultima_batida.id | - | CONTA |
| ATA-X-TURNO-PAUSA | LATERAL-ESTRUTURA | SIM | LEU app/ponto/services/cartorio.py:617 e :643: ata continua consumindo casamento dos marcos da grade; correção interrompida não observada nesse sítio. Mãe INFERIU mesmo defeito ata versus turno oficial | JUIZ-DO-PAR-DUAS-ENTRADAS | DOBRA |
| EMPRESA-ID-ISDIGIT | LATERAL-ESTRUTURA | NAO | LEU app/relatorios/views.py:1529: conversão protegida por isdecimal() e isascii(), corrigindo o caso ² | - | OBSOLETA |
| PRACA-TOLERANCIA-SEM-LEITOR | LATERAL-ESTRUTURA | SIM | LEU app/colaboradores/models.py:184: campo tolerancia_minutos permanece; INFERIU ausência de consumo no censo textual desse identificador fora de cadastro/testes | - | CONTA |
| FICHA-LE-O-CARIMBO | LATERAL-ESTRUTURA | SIM | LEU app/colaboradores/services/ficha.py:126 e :129: juiz recebe vínculos, sem célula; mãe INFERIU mesma migração de leitores de vínculo | CASA-ESCALA-E4 | DOBRA |
| REGUA-VOLTA-VIGILANTES | LATERAL-ESTRUTURA | PARCIAL | LEU app/core/views_config.py:431 e app/core/services/aplicacao_convencao.py:128: motivo não é prepositado e porta exige motivo novo; ato das aplicações no banco [não verificado]. Mãe INFERIU mesmo cadastro sem trilha | CADASTRO-DE-CCT-SEM-CARTORIO | DOBRA |
| RBAC-ESCRITA-POR-PAPEL | LATERAL-ESTRUTURA | PARCIAL | LEU app/core/views_config.py:391: há autorização em endpoint; matriz abrangente, negativas por endpoint e cobertura integral [não verificado], artefato geral não localizado | - | CONTA |
| V-FASE-FILA-FIO-ADMIN-SEM-PORTA | LATERAL-ESTRUTURA | SIM | LEU app/chamados/services/vigia_fio.py:313: fio_do_admin lê presença na lista; app/escala/services/cadastro_realidade.py:692 permite C10 sem ação. Mãe LEU app/docs/BACKLOG.md:1772 e :895 | O261-E-F5B-JUIZ-PORTA-ALCANCAVEL | DOBRA |
| V-FASE-FILA-MIUDOS | LATERAL-ESTRUTURA | SIM | LEU app/chamados/services/proposta_no_fio.py:70: seleciona somente propostas[0]; demais miúdos [não verificado]. Mãe INFERIU família de fase em app/docs/BACKLOG.md:1128 | FAMILIA-FASE-12x36 | DOBRA |
| TOAST-CORRIDA-2O-POST | LATERAL-ESTRUTURA | SIM | LEU app/escala/folgas.py:577 e :578; :652 e :653: veto continua antes do atomic, inserção depois; corrida INFERIU desse percurso | - | CONTA |
| F3-PORTA-E-CRON-PRAZO | LATERAL-ESTRUTURA | PARCIAL | LEU app/chamados/reconciliador.py:1534: porta de reatar existe; marca de cron instalada e atos dos universos [não verificado]. Mãe INFERIU mesmo reparo de F3, app/docs/BACKLOG.md:2053 | F3-BECO-1-VOLTA-3 | DOBRA |
| LASTRO-PARTE-2 | LATERAL-ESTRUTURA | PARCIAL | LEU app/ponto/services/lastro.py:401: fecho pelo reconciliar; app/ponto/management/commands/fechar_cobranca_com_lastro.py:25: vigia sem apply; instalação sob marca [não verificado]. Mãe INFERIU mesmo ciclo de lastro | BUG-LASTRO-VAI-E-VOLTA | DOBRA |
| LASTRO-PARTE-2-FILA-OPUS | LATERAL-ESTRUTURA | PARCIAL | LEU app/ponto/services/lastro.py:336: isenção/dispensa diferenciadas; :340 conserva or hora; alcance residual e eventos da frota [não verificado]. Mãe INFERIU residual do ciclo de lastro | BUG-LASTRO-VAI-E-VOLTA | DOBRA |
| LASTRO-PARTE-3 | LATERAL-ESTRUTURA | NAO | LEU app/chamados/signals.py:253 e :259: alterações publicam evento; app/chamados/services/disputa_emissao.py:1633: reabertura em lote acorda células | BUG-LASTRO-VAI-E-VOLTA | OBSOLETA |
| PEP-REENTRADA-FILA | LATERAL-ESTRUTURA | SIM | LEU app/chamados/services/disputa_emissao.py:1940 e :1944: carga de perguntas precede teste individual de voo; resíduos de custo/guardas ainda têm sítio; restantes [não verificado] | - | CONTA |
| C6-RELOGIO-BORDA | LATERAL-ESTRUTURA | SIM | LEU app/chamados/juizes.py:1465 chama janela com hoje opcional; :592 relê date.today quando omitido. Mãe INFERIU mesma injeção de relógio | RELOGIO-INJETADO | DOBRA |
| H12-DEMISSAO-NA-JANELA | LATERAL-ESTRUTURA | NAO | LEU app/folha/export.py:60 e :866: saída por rescisão compara demissão ao fim da janela. Mãe LEU app/docs/BACKLOG.md:1827 | H12H06-SEPARA-E-CRONTAB | OBSOLETA |
| H06-FALTA-PELA-GRADE | LATERAL-ESTRUTURA | NAO | LEU app/chamados/services/materializacao.py:527 e :532: não grava 720; app/ponto/models.py:881 e :885 deriva minutos pela grade. Mãe LEU app/docs/BACKLOG.md:1827 | H12H06-SEPARA-E-CRONTAB | OBSOLETA |
| MODO-COMUM-PASSE-LIVRE-PELO-DIA | LATERAL-ESTRUTURA | SIM | LEU app/core/constants.py:13 e :18: juiz recebe somente colaborador e lê flag atual, sem dia; mãe INFERIU mesma atemporalidade, app/docs/BACKLOG.md:956 | ISENTO-FLAG-ATEMPORAL | DOBRA |
| EFEITO-NONE-UM-LEITOR | LATERAL-ESTRUTURA | SIM | LEU app/ponto/services/fechamento.py:700: None suprime; app/ponto/services/efeito_ausencia.py:59: None não faz nada; app/ponto/services/dia_decidido.py:473: fallback ABONADO | - | CONTA |

45 linhas: CONTA 25, DOBRA 16, OBSOLETA 4, ? 3.

Notas para consolidação: mães marcadas INFERIU são propostas de agrupamento, não prova de absorção documental. O258 é raiz explícita de O249 em BACKLOG:1481. O261-E-F5B pode precisar dobrar na raiz O261-E-CELULA-20-UM-DONO se a leitura dos demais blocos comprovar essa cadeia. A existência de todos os requisitos de RBAC é PARCIAL porque há implementação localizada de autorização; matriz geral não foi encontrada.

## 13/11 21:01 · sessao 3cdb20f2c406 · - · - · origem=Ronald (tmux)

| id | balde | existe | prova | mãe | veredito |
|---|---|---|---|---|---|
| O50-COMPLEMENTO-LAUDO-1311 | ESPINHA-E | SIM | LEU app/colaboradores/services/vinculo.py:855: fecha somente vínculos já iniciados; INFERIU raiz pela absorção em app/docs/BACKLOG.md:1338. | O54 | DOBRA |
| O51 | ESPINHA-E | NAO | LEU app/relatorios/pdf_espelho.py:844: coluna S.ANT separada de ATRASO; INFERIU cura do pedido específico. | O54 | OBSOLETA |
| O52 | ESPINHA-E | PARCIAL | LEU app/ponto/services/espelho.py:862 e app/relatorios/pdf_espelho.py:638: rótulo migrou para palavra comum; caso Treino/folga atual [não verificado]. | O54 | DOBRA |
| O53 | ESPINHA-E | PARCIAL | LEU app/ponto/motor_calculo_v2.py:530 e app/relatorios/pdf_espelho.py:628: autoridades compartilhadas; todas as fases e acervo [não verificado]; absorção app/docs/BACKLOG.md:1338. | O54 | DOBRA |
| O54 | ESPINHA-E | PARCIAL | LEU app/core/espelho_verdade.py:94 e app/core/espelho_verdade.py:150: etapas ainda declaradas em curso; código do motor já consulta autoridade em app/ponto/motor_calculo_v2.py:530; certificação integral [não verificado]. | - | CONTA |
| O60 | ESPINHA-E | PARCIAL | LEU app/ponto/motor_calculo_v2.py:530 e app/ponto/motor_calculo_v2.py:1203: períodos e previsto têm autoridades; DIFF/aplicação 09 [não verificado]; parte O54-E3 em app/docs/BACKLOG.md:1345. | O54 | DOBRA |
| O61 | ESPINHA-E | NAO | LEU app/docs/BACKLOG.md:1346: cancelada integralmente pelo corte 07/08; entrega sobre 09 da primeira passada não corresponde à célula. | - | OBSOLETA |
| O62 | ESPINHA-E | SIM | LEU app/ponto/juiz_batida.py:270 e app/ponto/juiz_batida.py:303: calcula cadastro, mas sem marcos retorna zero/“escala sem intervalo”; INFERIU residual do juiz E2. | O54 | DOBRA |
| O63 | ESPINHA-E | NAO | LEU app/ponto/services/fechamento.py:928 e app/escala/services/escala_certa.py:42: decisão vem do juiz da competência, sem arquivo do host; INFERIU cura das duas faces. | - | OBSOLETA |
| O65 | ESPINHA-E | NAO | LEU app/ponto/turnos.py:1939 e app/ponto/turnos.py:1382: geometria recebe e usa papel da ata; INFERIU cura do sítio descrito. | O54 | OBSOLETA |
| O66 | ESPINHA-E | ? | LEU app/chamados/models.py:148: há guarda de fechamento; garantia de canal para todo furo/cobrado exige universo atual [não verificado]; família E6 em app/docs/BACKLOG.md:1338, INFERIU mãe. | O54 | DOBRA |
| O67 | ESPINHA-E | PARCIAL | LEU app/ponto/turnos.py:1382 e app/ponto/supra_juiz.py:291: papel e teto têm curas; cinco classes/casos da célula em app/docs/BACKLOG.md:1352 não têm prova atual integral [não verificado]; INFERIU programa-mãe. | O54 | DOBRA |
| O67b | ESPINHA-E | PARCIAL | LEU app/relatorios/pdf_espelho.py:628 e app/relatorios/pdf_espelho.py:837: topo da folha e linhas do dia continuam fontes distintas; demais casos [não verificado]; INFERIU parte E4. | O54 | DOBRA |
| O68 | ESPINHA-E | PARCIAL | LEU app/ponto/motor_calculo_v2.py:673: dia não explicado conserva períodos da geometria; parte do papel curada em app/ponto/turnos.py:1382; apply e aposentadorias integrais [não verificado]. | O54 | DOBRA |
| O51b | ESPINHA-E | PARCIAL | LEU app/relatorios/pdf_espelho.py:844: colunas leem rubricas diárias; topo ainda passa por folha_manda em app/relatorios/pdf_espelho.py:628; igualdade integral das somas [não verificado]; INFERIU E4/O51. | O54 | DOBRA |
| O70 | ESPINHA-E | SIM | LEU app/escala/alimentacao.py:269 e app/escala/alimentacao.py:285: ausência de geradora ainda seleciona vínculo por ativa/data; preenchimento do passivo [não verificado]; INFERIU residual E1. | O54 | DOBRA |
| O71 | ESPINHA-E | ? | LEU app/ponto/portas/celula.py:1093: porta grava novo veredito com trilha; vereditos antigos de 09 e resultado do rejulgamento [não verificado]; app/docs/BACKLOG.md:1358 diz parte de O67 item 2, INFERIU raiz. | O54 | DOBRA |
| O72 | ESPINHA-E | NAO | LEU app/ponto/motor_calculo_v2.py:2210: turno não noturno ancora saída prevista na data da entrada; INFERIU cura do sinal invertido descrito. | O54 | OBSOLETA |
| O73 | ESPINHA-E | PARCIAL | LEU app/ponto/motor_calculo_v2.py:2686: núcleo trata blocos do dia; complemento de classes permanece na célula app/docs/BACKLOG.md:1360, fechamento atual desses resíduos [não verificado]. | O54 | DOBRA |
| O76 | ESPINHA-E | NAO | LEU app/ponto/motor_calculo_v2.py:2403: dia vem do mapa do juiz do turno; INFERIU cura da atribuição civil descrita. | O54 | OBSOLETA |
| O88 | ESPINHA-E | ? | LEU app/ponto/turnos.py:1382: papel da ata prevalece; pareamento dos dias deslocados enumerados em app/docs/BACKLOG.md:1368 [não verificado]; INFERIU residual de O86/E6. | O54 | DOBRA |
| O87 | ESPINHA-E | PARCIAL | LEU app/escala/models.py:353 e app/escala/models.py:657: cadastro valida intervalo dentro do turno; situação dos sete templates/passivo [não verificado]. | - | CONTA |
| O86 | ESPINHA-E | PARCIAL | LEU app/ponto/turnos.py:1421: fecho por saída e limites existe; app/docs/BACKLOG.md:1370 corrige diagnóstico e separa resíduos; existência atual dos resíduos [não verificado]; INFERIU parte E6. | O54 | DOBRA |
| O95 | ESPINHA-E | SIM | LEU app/ponto/turnos.py:1356: absorção por marco próximo continua sem prova de retorno antes de marcar intrajornada em :1365; INFERIU persistência do defeito. | - | CONTA |
| O94 | ESPINHA-E | ? | LEU app/ponto/adaptador_motor.py:38: turno sem saída continua aberto; único restante da célula é decisão/aplicação sobre gravado, app/docs/BACKLOG.md:1373; estoque atual [não verificado]; INFERIU E3. | O54 | DOBRA |
| O93 | ESPINHA-E | PARCIAL | LEU app/ponto/motor_calculo_v2.py:2405 e app/ponto/motor_calculo_v2.py:2423: herança exige fim real anterior; aplicação/decisão sobre 09 [não verificado]; INFERIU residual E3. | O54 | DOBRA |
| O92 | ESPINHA-E | SIM | LEU app/ponto/models.py:884: ainda chama previsto por dia sem alimentação; cópia do período já retirada, app/escala/utils.py:2126; INFERIU residual de custo existente. | - | CONTA |
| O89 | ESPINHA-E | NAO | LEU app/ponto/motor_calculo_v2.py:2263 e app/ponto/motor_calculo_v2.py:2268: motor copia as instâncias recebidas; INFERIU cura da mutação do chamador. | O54 | OBSOLETA |
| O85 | ESPINHA-E | PARCIAL | LEU app/ponto/services/fechamento.py:792 e app/ponto/services/fechamento.py:838: horas fora das fatias são recolhidas; noturno dessas horas segue declarado não pago em :779; INFERIU residual E6. | O54 | DOBRA |
| O83 | ESPINHA-E | ? | LEU app/ponto/services/fechamento.py:792: classe C tem caminho corretivo; distribuição atual das quatro classes requer medição [não verificado]; app/docs/BACKLOG.md:1381 identifica E6, INFERIU raiz. | O54 | DOBRA |
| O136 | ESPINHA-E | SIM | LEU app/ponto/motor_calculo_v2.py:3659: consulta ao juiz continua restrita ao rótulo turno_partido; comercial não pode virar partido pelo juiz. | - | CONTA |
| O139 | ESPINHA-E | PARCIAL | LEU app/config/crons.py:2170: papel prazo deriva do cadastro de delegação; resultado do selo/host exigido na célula app/docs/BACKLOG.md:1393 [não verificado]; INFERIU parte da raia descrita em :1394. | CHAMADO-EM-RAIA | DOBRA |
| CHAMADO-EM-RAIA | ESPINHA-E | PARCIAL | LEU app/chamados/models.py:128 e app/config/crons.py:2167: portas e divisão de papéis existem; total de escritores/leitores e prova zero da raia [não verificado]. | - | CONTA |
| O141 | ESPINHA-E | PARCIAL | LEU app/ponto/views.py:1508 e app/core/juizes.py:333: K1 e K6 têm autoridades; baixa integral e justificada dos sítios K4 da célula app/docs/BACKLOG.md:1395 [não verificado]. | - | CONTA |
| DOSSIES-DAS-CELULAS | ESPINHA-E | PARCIAL | LEU app/escala/alimentacao.py:228 e app/core/juizes.py:350: há autoridades e perguntas sem autoridade; seis famílias da célula app/docs/BACKLOG.md:1396 não têm fecho integral demonstrado [não verificado]. | - | CONTA |
| SITUACAO-E-LAMPADA | ESPINHA-E | SIM | LEU app/relatorios/services.py:102 e app/relatorios/services.py:105: universo ainda filtra situação crua; INFERIU persistência de leitor nomeado na célula. | - | CONTA |
| O142 | ESPINHA-E | PARCIAL | LEU app/ponto/turnos.py:1960 e app/ponto/turnos.py:1983: dono por dia e cópia por segmento curam causas nomeadas; invariância integral entre janelas [não verificado]. | - | CONTA |
| O156 | ESPINHA-E | SIM | LEU app/relatorios/pdf_espelho.py:628 e app/relatorios/pdf_espelho.py:837: totais recebem folha, linhas recebem DiaPago; soma do exibido ainda não é fonte única; INFERIU E4. | O54 | DOBRA |
| O164 | ESPINHA-E | PARCIAL | LEU app/chamados/models.py:128 e app/chamados/models.py:1101: portas atuais existem; inventário completo dos 60 pontos e selo vazio [não verificado]; parte da raia em app/docs/BACKLOG.md:1418. | CHAMADO-EM-RAIA | DOBRA |
| O166 | ESPINHA-E | PARCIAL | LEU app/config/crons.py:2170: prazo já tem cadastro próprio; resultado medido e selo de host [não verificado]; app/docs/BACKLOG.md:1420 identifica execução da O139, INFERIU raiz. | CHAMADO-EM-RAIA | DOBRA |
| O170 | ESPINHA-E | PARCIAL | LEU app/config/crons.py:2167 e app/config/crons.py:2189: há migração de eventos e juízes restantes; integralidade dos 12 ganchos/zero por cron [não verificado]; INFERIU mesma obra ampla de migração. | VARREDURA-VIRA-EVENTO-24-CRONS | DOBRA |
| O171 | ESPINHA-E | ? | LEU app/ponto/portas/celula.py:1098: lavra emite evento; convergência do FM no primeiro evento e colaboradores sem evento [não verificado]; INFERIU residual de fechamento online O54, app/docs/BACKLOG.md:1426. | O54 | DOBRA |
| O177 | ESPINHA-E | PARCIAL | LEU app/ponto/juiz_batida.py:128 e app/ponto/juiz_batida.py:280: cadastro e batido somam pausas; motor preserva lista em app/ponto/motor_calculo_v2.py:721; passivo/aplicação integral [não verificado]. | - | CONTA |
| O178 | ESPINHA-E | SIM | LEU app/pautas/services.py:154 e app/pautas/models.py:90: prazo segue opcional; contador de leitura e atribuição obrigatória solicitados não estão garantidos por esse contrato, INFERIU pendência. | - | CONTA |
| O179 | ESPINHA-E | ? | LEU app/ponto/services/fechamento.py:792: há tratamento de horas fora das fatias; causa e estado dos três colaboradores da célula app/docs/BACKLOG.md:1434 [não verificado]. | - | CONTA |
| O180 | ESPINHA-E | ? | LEU app/escala/alimentacao.py:282: juiz recusa geradora fora da vigência; existência e entrega da lista dos 102/passivo de app/docs/BACKLOG.md:1435 [não verificado]. | - | CONTA |
| O183 | ESPINHA-E | PARCIAL | LEU app/chamados/models.py:1131: nascimento valida urgência pela autoridade; estoque de valores inválidos da célula app/docs/BACKLOG.md:1438 [não verificado]. | - | CONTA |
| O189 | ESPINHA-E | ? | LEU app/ponto/supra_juiz.py:243: zero e None são distintos na contagem; produto desta linha é medição publicada, app/docs/BACKLOG.md:1444; validade atual dos números [não verificado]. | - | CONTA |

48 linhas: CONTA 17, DOBRA 22, OBSOLETA 9, ? 10.

## 13/11 21:01 · sessao d96358ad5bc4 · - · - · origem=Ronald (tmux)

| id | balde | existe (SIM/NAO/PARCIAL/?) | prova (arquivo:linha do HEAD ou commit; LEU x INFERIU) | mae | veredito (CONTA/DOBRA/OBSOLETA) | [nao verificado]
|---|---|---|---|---|---|
| PERGUNTA-SEM-SELO | ESPINHA-E | PARCIAL | LEU app/core/regua_cct.py:328 e app/ponto/portas/regen_exportada.py:128: classe de vínculo migrada; app/chamados/services/materializacao.py:86 conserva vizinha por tipo gravado; INFERIU residual estrutural. | - | CONTA |
| O-PROJECOES | ESPINHA-E | ? | LEU app/ponto/models.py:374: DiaPago tem versões; app/docs/BACKLOG.md:756 exige fonte, versão e reconstrução de todas as projeções; cobertura integral [não verificado]. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| JUIZ-DO-PAR-DUAS-ENTRADAS | ESPINHA-E | PARCIAL | LEU app/ponto/motor_calculo_v2.py:872: turnos usam autoridade comum; app/ponto/motor_calculo_v2.py:2117 ainda passa período à volta; app/chamados/detector_anomalias.py:212 conserva segunda resposta; INFERIU resíduos concretos. | - | CONTA |
| PENDENCIA-SEM-JUIZ-3 | ESPINHA-E | PARCIAL | LEU app/chamados/views.py:901 e app/escala/services/furos_diarios.py:212: dois leitores migrados; app/chamados/services/painel.py:324 conserva exclusão por verbos e total próprio em :333; INFERIU residual. | SILENCIO-VENCIDO-VOLTA | DOBRA |
| COBERTURA-PARCIAL-UM-JUIZ | ESPINHA-E | PARCIAL | LEU app/ponto/supra_juiz.py:394 e app/escala/utils.py:1244: período parcial considerado; app/escala/utils.py:1254 ainda limpa furos por justificativa aprovada sem recorte de tipo/período; INFERIU cura incompleta. | - | CONTA |
| GRADE-POS-POUSO | ESPINHA-E | ? | LEU app/ponto/services/cartorio.py:142: versões 3 e 4 coexistem; medições pós-deploy requeridas em app/docs/BACKLOG.md:789 não constam do dado fornecido [não verificado]. | GRADE-FOLGA-JUIZ | DOBRA |
| COBPARC-AUSENCIA-CALA-DOIS-JUIZES | ESPINHA-E | SIM | LEU app/chamados/services/materializacao.py:82 aceita parcial sem período; app/chamados/juizes.py:477 só consulta ausência que explica o dia inteiro; INFERIU divergência permanece. | COBERTURA-PARCIAL-UM-JUIZ | DOBRA |
| COBPARC-FUROS-LEITOR-PARCIAL | ESPINHA-E | ? | LEU app/escala/services/furos_diarios.py:49 e app/inteligencia/calculadores.py:50: dois leitores agora cobram resto parcial; demais sítios também pedidos em app/docs/BACKLOG.md:793 sem confirmação integral [não verificado]. | COBERTURA-PARCIAL-UM-JUIZ | DOBRA |
| FIO-LE-O-JUIZ | ESPINHA-E | ? | LEU app/chamados/services/fio.py:237 e app/chamados/services/disputa_emissao.py:1966: leitores consultam pergunta_viva; escritor dos 26 e clique/push pedidos em app/docs/BACKLOG.md:794 [não verificado]. | - | CONTA |
| CXR-TELA-LE-O-MOTOR | ESPINHA-E | ? | LEU app/ponto/services/cxr_na_lista.py:79 e app/escala/services/cadastro_realidade.py:992: publicação do motor chega à leitura; resultados PRONTO 1/5 e lavra pós-corrente de app/docs/BACKLOG.md:798 [não verificado]. | - | CONTA |
| CXR-PRONTO1-COL841 | ESPINHA-E | ? | LEU app/ponto/services/cxr_na_lista.py:79: medidor lê itens publicados pelo motor; sete dias requeridos em app/docs/BACKLOG.md:799 sem dado atual fornecido [não verificado]. | CXR-TELA-LE-O-MOTOR | DOBRA |
| E1-ACERVO-5-GRUPOS-09 | ESPINHA-E | ? | LEU app/docs/BACKLOG.md:800: cinco grupos dependem de tratamento do acervo exportado; existência atual desses registros e execução pela porta [não verificado]. | FIO-E0-V4-E1-ENTRADAS | DOBRA |
| RETRATADA-CONVERSA-E-REABRIR | ESPINHA-E | SIM | LEU app/chamados/services/disputa_emissao.py:431: episódio sem dia vira conversa sem transportar origem; app/chamados/services/disputa_emissao.py:1593 e :1605: reabertura sem recusa de episódio nesse caminho; INFERIU abertura/reabertura ainda expostas. | FIO-E0-V4-E1-ENTRADAS | DOBRA |
| E1-A2-VAZIO-MEDIR | ESPINHA-E | PARCIAL | LEU app/chamados/services/fio.py:1528: origem inexistente identificada; app/chamados/services/fio.py:1644 conserva retorno da chave quando A2 não alcança dia; medição de frota [não verificado]. | FIO-E0-V4-E1-ENTRADAS | DOBRA |
| E1-DUPLICATA-SOBRE-RETRATADA | ESPINHA-E | SIM | LEU app/chamados/services/fio.py:1744: retratada sai do fecho; app/chamados/services/fio.py:1947: colisões começam por _no_fato, excluindo tais linhas; INFERIU deduplicação descrita continua sem abrangê-las. | PERGUNTA-DUPLICATA-1110 | DOBRA |
| CXR-PRONTO5-DEFINICAO | ESPINHA-E | ? | LEU app/chamados/services/vigia_fio.py:682 e :687: classe d e dia_na_lista_dono_* são condições distintas; definição/medição requerida em app/docs/BACKLOG.md:806 [não verificado]. | CXR-TELA-LE-O-MOTOR | DOBRA |
| JUIZ-DO-PAR-GRUPO-1 | ESPINHA-E | PARCIAL | LEU app/chamados/detector_anomalias.py:208 e :212: leitor ainda usa _match_marcos com derivar=False; INFERIU grupo incompleto. | JUIZ-DO-PAR-DUAS-ENTRADAS | DOBRA |
| MOTOR-PASSA-O-TURNO-A-VOLTA | ESPINHA-E | SIM | LEU app/ponto/motor_calculo_v2.py:2117: cumpriu_a_volta_do_intervalo recebe dia, marcos e entrada/saída do período, sem turno; INFERIU sítio permanece. | JUIZ-DO-PAR-DUAS-ENTRADAS | DOBRA |
| O261-COMPLEMENTO-[nome] | ESPINHA-E | ? | LEU app/chamados/services/fio.py:774: alcance consulta pergunta_viva; estoque materializado sem resposta e nascimento sob pai terminal requeridos em app/docs/BACKLOG.md:816 sem dado atual [não verificado]. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| C6-TETO-MARCO-PENDENTE | ESPINHA-E | SIM | LEU app/chamados/juizes.py:968: bool(acesa) transforma None em False; INFERIU perda do estado pendente permanece. | - | CONTA |
| PASSIVO-VALIDADO-K6 | ESPINHA-E | SIM | LEU app/chamados/services/passivo_validado.py:124 e :127: decide exportada por vigente is not None, sem competencia_entregue; INFERIU cópia do predicado permanece. | O257 | DOBRA |
| PROGRAMA-CELULA-E-O-DIA | ESPINHA-E | PARCIAL | LEU app/ponto/status_dia.py:221 e app/colaboradores/services/calendario.py:606: autoridade e leitor migrados; app/chamados/services/painel.py:324 e app/chamados/detector_anomalias.py:212 conservam decisões paralelas relacionadas às etapas; INFERIU programa incompleto. | - | CONTA |
| PROGRAMA-CELULA-E-O-DIA-DUAS-PISTAS | ESPINHA-E | PARCIAL | LEU app/docs/BACKLOG.md:822: complemento explícito; app/ponto/status_dia.py:221 e app/colaboradores/services/calendario.py:606 mostram pista status implementada; conclusão da certificação v3 [não verificado]. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| E4-CAL-RETIDO-FOLHA-NO-LEITOR | ESPINHA-E | NAO | LEU app/colaboradores/services/calendario.py:602 e :606: status vem do D4; retenção deixou de classificar a palavra no leitor, conforme sítio removido em :597; INFERIU cura do defeito local. | PROGRAMA-CELULA-E-O-DIA | OBSOLETA |
| E4-CAL-FALLBACK-STATUS-ROTULO | ESPINHA-E | NAO | LEU app/colaboradores/services/calendario.py:606 e app/ponto/status_dia.py:288: D4 entrega status/rótulo; app/templates/colaboradores/partials/_calendario_grade.html:173 lê rótulos do núcleo; INFERIU segundo caminho eliminado. | PROGRAMA-CELULA-E-O-DIA | OBSOLETA |
| E4-CAL-DOIS-CONTRATOS-DE-CELULAS | ESPINHA-E | NAO | LEU app/colaboradores/services/calendario.py:606: calendário fornece a mesma carga ao D4; app/ponto/status_dia.py:272 e :274 usam a carga para célula e encerramento; INFERIU eliminada composição dos dois formatos nesse leitor. | PROGRAMA-CELULA-E-O-DIA | OBSOLETA |
| E4-INCLUI-CONTRATO-20 | ESPINHA-E | ? | LEU app/config/crons.py:2068 e :2071: crons migrados a vigia; equivalência integral com status_dia exigida em app/docs/BACKLOG.md:826 e universo final de leitores [não verificado]. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| GRADE-FOLGA-DO-VINCULO | ESPINHA-E | NAO | LEU app/escala/utils.py:1149: grade consulta folga_lancada_do_dia; app/ponto/precedencia.py:216 e :225: filtro rejeita folga de calendário de outro vínculo; INFERIU cura do sítio. | GRADE-FOLGA-JUIZ | OBSOLETA |
| GRADE-V5-SEM-PORTAO | ESPINHA-E | ? | LEU app/escala/utils.py:1149: correção de leitura presente; app/docs/BACKLOG.md:844 também pede preservação de FM/TXT e pauta da 09, sem dado atual fornecido [não verificado]. | GRADE-FOLGA-JUIZ | DOBRA |
| GRADE-FOLGA-JUIZ | ESPINHA-E | NAO | LEU app/ponto/precedencia.py:212; app/escala/utils.py:1149; app/ponto/services/declarar_dia.py:74; app/ponto/management/commands/rejulgar_ausencia_em_folga.py:65: sítios consultam juiz comum; fase_conflitante foi separada em app/docs/BACKLOG.md:846; INFERIU cura local completa. | - | OBSOLETA |
| E4-7-D4-ABERTO-SEM-TURNO | ESPINHA-E | ? | LEU app/ponto/status_dia.py:283: aberto sem turno vivo recebe ROTULO_EM_ANDAMENTO; medição dos 171 registros requerida em app/docs/BACKLOG.md:848 sem dado fornecido [não verificado]. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| E4-8-SAIDA-I | ESPINHA-E | ? | LEU app/relatorios/furos.py:115 e :123: leitor usa D4; _VER_ESTADO retirado em :42; decisão manter/reverter do pouso requerida em app/docs/BACKLOG.md:852 [não verificado]. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| E4-CAL-DIA-COM-FIM-LE-A-FOLHA | ESPINHA-E | NAO | LEU app/colaboradores/services/calendario.py:606 e app/ponto/status_dia.py:291 e :316: passado sem lavra não ganha falta por fim previsto; INFERIU derivação antiga removida. | PROGRAMA-CELULA-E-O-DIA | OBSOLETA |
| E4-CALENDARIO-L1 | ESPINHA-E | ? | LEU app/colaboradores/services/calendario.py:606: migração do leitor presente; D5 do acervo e resultados de frota exigidos em app/docs/BACKLOG.md:856 sem dado fornecido [não verificado]. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| COBPARC-PORTAO-PARCIAL-JUIZ-UNICO | ESPINHA-E | SIM | LEU app/relatorios/furos.py:195; app/inteligencia/calculadores.py:50 e :149; app/escala/services/furos_diarios.py:49; app/chamados/services/declaracao_texto.py:119: portão composto continua repetido; INFERIU necessidade de pergunta única. | COBERTURA-PARCIAL-UM-JUIZ | DOBRA |
| COBPARC-FILA-MATERIALIZACAO | ESPINHA-E | SIM | LEU app/chamados/services/materializacao.py:79 e :83: cobertura vem de ausencia_cobre(para='cobranca') e período do cadastro, sem ata; INFERIU sítio permanece. | COBERTURA-PARCIAL-UM-JUIZ | DOBRA |
| E4-CALENDARIO-QUEM-PUBLICA-EH-TRABALHO | ESPINHA-E | NAO | LEU app/colaboradores/services/calendario.py:103: oferta usa último recurso, passado e status; antiga perna eh_trabalho removida em :647; INFERIU cura completa do predicado. | PROGRAMA-CELULA-E-O-DIA | OBSOLETA |
| D5-VIA-NULA | ESPINHA-E | SIM | LEU app/ponto/services/cartorio.py:1260 e :1409: carimbo não exige veredito_via; :934 chama lavrar_veredito(via='cartorio'); INFERIU via nula segue chegando à porta. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| E4CAL-P1-ROTULO-SEM-CERTIDAO | ESPINHA-E | ? | LEU app/ponto/status_dia.py:288: rótulo persistido é lido; três células mencionadas em app/docs/BACKLOG.md:874 sem dado atual de existência/causa [não verificado]. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| E4CAL-P3-ULTIMO-RECURSO-ENGOLE | ESPINHA-E | SIM | LEU app/colaboradores/services/calendario.py:478: except Exception registra aviso em :479 e segue; INFERIU falha continua absorvida. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| E4CAL-P3-CENSO-HOJE-PORTA-CURTA | ESPINHA-E | SIM | LEU app/chamados/services/vigia_fio.py:271: plural substituta é chamada sem hoje/agora; app/colaboradores/services/calendario.py:107 registra remoção da curta; INFERIU relógio do censo ainda não atravessa essa porta. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| E4CAL-P3-LEITURA-ESCREVE | ESPINHA-E | ? | LEU app/colaboradores/services/calendario.py:476: leitura desce ao C6; ocorrência de materialização mencionada em app/docs/BACKLOG.md:877 sem trajetória atual demonstrada apenas pela leitura deste sítio [não verificado]. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| E4CAL-L1-POUSO-0411 | ESPINHA-E | ? | LEU app/colaboradores/services/calendario.py:606: código previsto pousado na cópia; resultados 1–8 e conferência posterior do vigia exigidos em app/docs/BACKLOG.md:878 sem dado fornecido [não verificado]. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| PAREADOR-JANELA-ORFA | ESPINHA-E | ? | LEU app/ponto/turnos.py:1295 e :1299: ramo consulta turnos e marcos presentes na janela; divergência mensal/unitária requerida em app/docs/BACKLOG.md:880 não demonstrada por dados disponíveis [não verificado]. | O142 | DOBRA |
| E4CAL-CARD-LE-NUCLEO | ESPINHA-E | NAO | LEU app/templates/colaboradores/partials/_calendario_grade.html:173: cartão aberto chama rotulo_do_status e rotulo_em_andamento; demais títulos em :157 e :161 também leem núcleo; INFERIU cura. | PROGRAMA-CELULA-E-O-DIA | OBSOLETA |
| STATUS-ESCALA-SUSPEITA-VOCAB | ESPINHA-E | SIM | LEU app/ponto/status_dia.py:38: rótulo conserva “Sem batida”; app/ponto/status_dia.py:113 permite classificação pela régua/veredito; INFERIU vocabulário indicado não foi alterado. | PROGRAMA-CELULA-E-O-DIA | DOBRA |
| A2-RAISE-NO-FUROS | ESPINHA-E | SIM | LEU app/relatorios/furos.py:194: grade_da_celula é avaliada sem captura da recusa A2; INFERIU exceção ainda pode propagar ao leitor. | - | CONTA |
| E4-APP-MAPA | ESPINHA-E | NAO | LEU app/api/views.py:1480 e :1483: D4 mensal; :1501 adapta pelo núcleo; :1547 publica status_dia aditivo; app/ponto/status_dia.py:79 e :89 cobrem mapa; INFERIU migração completa do sítio. | PROGRAMA-CELULA-E-O-DIA | OBSOLETA |

48 linhas neste trecho. Mães propostas para E1/FIO, GRADE e JUIZ-DO-PAR precisam ser conciliadas com eventuais raízes mais amplas encontradas pelos outros exploradores; os vínculos explícitos com PROGRAMA foram normalizados conforme orientação do consolidante.

## 13/11 20:58 · sessao 6e1e5b0fe612 · - · - · origem=Ronald (tmux)

LEU, somente leitura; nenhum teste foi executado. [nao verificado]

| Classe de estado | Retorno exigido pelos testes abertos | Prova estática | [nao verificado]
|---|---|---|
| Vivo na fila, sem pergunta | `porta_fila`, chamado nomeado, `decidir`, fio `chamado`; ação emissor preservada | `test_e3_portas_tarefa.py::test_rota_so_fila_sem_pergunta`, linhas 39–46; `test_o261e_f5b_juiz_porta.py::test_so_a_fila_sem_pergunta_e_chamado`, 159–168 |
| Residual A na célula + responder sob B na fila | A e B nos verbos, A em `fila`, fio `colab` | `test_e3_portas_tarefa.py::test_fila_A_pergunta_sob_B`, 48–55; `ResidualComAcaoTest.test_RED_A_na_fila_aparece_com_acao_sob_B`, 207–212 |
| Mesma situação, B no painel `validar` | Ação continua responder; tarefa fio `colab` | `test_e3_portas_tarefa.py::test_verbo_nao_e_traducao_da_acao_da_tarefa`, 84–96 |
| Ação responder alcançável, painel validar | Fio `colab` | `test_e3_portas_v6_sem_banco.py::test_acao_responder_conserva_colab_apesar_de_validar_no_painel`, 16–20 |
| Ação validar alcançável, painel cobrar | Fio `admin` | Mesmo arquivo, `test_acao_validar_conserva_admin_apesar_de_cobrar_no_painel`, 22–26 |
| Ação e residual com donos diferentes | Prioridade literal: colab > admin > chamado > sistema | Mesmo arquivo, `test_residual_e_acao_combinam_donos_sem_retraduzir_a_acao`, 28–42 |
| Ação admin sem porta, porta textual ou D fora da fila + C decidir | Porta C e fio `chamado`; ação original preservada | Mesmo arquivo, `test_acao_sem_porta_tambem_conserva_dono_humano`, 44–53 |
| C na fila; pergunta respondida em D vivo registrado/silenciado vigente, fora da fila | C17=tarefa=folha `chamado`; verbo C decidir | `test_o261e_f5b_juiz_porta.py`, auxiliar 218–251; testes 253–259 | [nao verificado]
| Isento ou outras dispensas com residual | Porta conservada; nenhuma ação criada | `test_e3_portas_tarefa.py::test_isento_fila_residual_conserva_porta`, 65–74; `test_e3_portas_v6_sem_banco.py`, 67–90 |
| Marco aceso + vivo na fila | Porta fila e fio chamado, ações vazias | `test_e3_portas_tarefa.py::test_marco_aceso_com_chamado_vivo_na_fila_mantem_porta_fila`, 143–154 |
| Passivo sem linha, âncora da célula na fila | Porta fila, validar/admin, `nao_sei=True`, ações vazias | Mesmo arquivo, `test_passivo_sem_linha_porta_visivel_rotulo_nao_sei`, 156–169 |
| Passivo sem linha, âncora externa na fila | Porta pergunta, `fila=()`, verbo validar | Mesmo arquivo, `test_passivo_sem_linha_ancora_fora_da_celula_porta_pergunta`, 171–181 |
| Encerrada/silêncio/absorvida aprovada, âncora externa viva na fila | Porta fila; passivo mantém pergunta | `test_e3_portas_v6_sem_banco.py::test_encerrada_externa_tem_porta_do_chamado_e_passivo_preserva_pergunta`, 92–100 |
| Vivo fora da fila, sem outro residual | Nenhuma porta | `test_e3_portas_tarefa.py::test_vivo_fora_da_fila_nao_vira_porta_nem_reabre_chamado`, 257–262; F5B, 200–204, 261–272 |
| Silêncio vencido → histórico | Porta fila, verbos histórico, `fila=()` e fio chamado | `test_e3_portas_tarefa.py`, 264–269; F5B, 170–191 |
| Fila lida, mas partição não viu o chamado | Nenhuma porta | F5B `test_MORDE_membro_da_fila_que_a_particao_nao_viu_nao_abre_porta`, 274–290 |
| Hoje sem porta / hoje com residual | Sem porta fica no teto; residual conta porta antes do teto | `test_o261e_f5_alcance.py::test_MORDE_so_a_fila_sem_pergunta_nao_vence_o_teto_de_hoje`, 466–475 |

Conflitos semânticos persistentes a destacar: C17 julga fio por verbos; tarefa julga ação alcançável antes do residual. Portanto os casos responder/validar e validar/cobrar acima produzem resultados diferentes entre esses juízes por construção. A própria prova exige essas escolhas da tarefa, em `test_e3_portas_v6_sem_banco.py:16–26`; o cálculo C17 é `fio_pelos_verbos`, cuja escada é testada em `test_o261e_f5b_juiz_porta.py:293–300`.

Propriedades já com selos: idempotência sintética em `test_e3_c8b_sem_banco.py::BordasDaCura.test_idempotente:78`; esquema completo em `:81–89`; lote repetido com zero consultas e mesma resposta em `test_e3_portas_tarefa.py:127–141`; folha/pauta/alcance em amostras em `test_e3_leitores_c17_chamadores.py:123–179`; leitores migrados não chamam C17 em `:182–192`.

SEM PROVA universal encontrada: produto cartesiano de todos os estados de `_pela_decisao` com fases de chamado, residual A/B, rota, instante e todos os leitores; paridade universal incluindo contador do fio. Em particular, razões `ato_desconhecido:*`, `lampada_nao_sabe`, `marco_fora_do_turno`, `instancia_sem_identidade` cruzadas com todas as portas não têm selo de paridade na bateria aberta — INFERIU a partir de `tarefa_do_dia.py:171,280–306` e dos casos lidos.

Os executores v7 são fornecedores sintéticos e declaram que não certificam integração ORM: `e3_volta7_mutantes.py:1–5`; `test_e3_volta7_sem_banco.py:1–4`. O resultado de execução do v7 atual permanece [não verificado].

## 13/11 20:58 · sessao 2623f0bb1785 · - · - · origem=Ronald (tmux)

LEU — censo e escritores relevantes à tarefa, com limites de exaustividade explícitos.

| Entidade/estado | Persistido | Escritor no HEAD |
|---|---|---|
| Chamado `aberto`, `em_analise`, `registrado`, `resolvido`, `fechado`, `cancelado`, `superado` | `ChamadoColaborador.status_local` (`app/chamados/models.py:28`, `:52`) | Nascer: `models.py:1071`, `:1096`; transição: `_gravar_estado`, `models.py:201`, atribuição `:228`, gravação em `:229` em diante. |
| Vivo versus cobrança versus terminal | Classificação, não campo adicional: `VIVOS=(aberto,em_analise,registrado)`; `COBRANCA=(aberto,em_analise)`; terminais resolvido/fechado/cancelado/superado | Autoridade: `app/chamados/catalogo/motor.py:24`; transições humanas `:99`, sistema `:110`, juiz `decidir` `:125`. |
| Encerramento | `via_encerramento`, `resolvido_em` (`models.py:54`, `:60`) | `_gravar_estado` terminal carimba `models.py:233` a `:245`; vivo limpa `:248` a `:249`. |
| Arquivado/silenciado, histórico/retornado | `silenciado_ate`, `silenciado_por`, `silenciado_motivo`; silêncio não muda status (`models.py:87`, `:90`) | `arquivo.silenciar`, `app/chamados/services/arquivo.py:166` a `:172`; `levantar`, `:202` a `:210`. |
| Na fila | Não persistido como booleano: COBRANCA e ausência de `silenciado_ate > agora` | Leitor/juiz `qs_na_fila`, `app/chamados/services/painel.py:158`, cálculo `:188` a `:190`. Igualdade `silenciado_ate == agora` já volta à fila. |
| Pergunta pendente/respondida/validada/resolvida | Rótulo **derivado**, não coluna de estado: `PerguntaDisputa.estado` | `models.py:3060` a `:3074`. Não equivale a vitalidade: a última alternativa pode devolver pendente/respondida mesmo após `pergunta_viva` negar vida. |
| Identidade da pergunta | `colaborador`, `data_evento`, `instancia_marco`, `motivo`, `conversa`, `batida_origem` (`models.py:2442`, `:2444`, `:2454`, `:2455`, `:2456`, `:2461`) | Nascimento `_nascer`, `models.py:2846`, inserção `:3040` a `:3045`; atribuição do dia somente `fio.gravar_dia_do_fato`, `app/chamados/services/fio.py:829` a `:840`. |
| Porta/âncora da pergunta | `chamado_destino`, fallback do pai `disputa.chamado` (`models.py:3437`, `:1750`) | Nascimento `models.py:3040`; reuso `:3008`; costura `fio.reapontar_resolvedora`, `fio.py:964` a `:967`. Seleção lida por `fio.chamado_que_lista`, `fio.py:97`. |
| Respondida | `resposta_colab`, `respondida_em` (`models.py:2468`, `:2469`) | Resposta humana `disputa_emissao.py:1467` a `:1508`, registra decisão `:1511`; automática `:1640`, escreve `:1659` a `:1664`; correção pela trilha `:1668`, escreve `:1681` a `:1685`; nascimento com projeção `models.py:3040` a `:3045`; carimbo complementar `models.py:3156` a `:3163`. |
| Validada | `validada_em`, `validada_por` (`models.py:2470`, `:3430`) | `_carimbar_validacao`, `validacao.py:129` a `:143`; nascimento `models.py:3040`; carimbo complementar `models.py:3160` a `:3163`. |
| Validação devolvida | `validada_em/por = NULL`, resposta preservada; decisão `devolucao` | `validacao.devolver_validacao_recusada`, `validacao.py:190` a `:210`. Não há estado persistido denominado “devolvida”. |
| Recusada/reaberta ao colaborador | Par resposta e validação zerados, materialização/via zeradas; decisão com fotografia | `disputa_emissao.reabrir_para_correcao`, `:1562`, campos `:1595`, decisão `:1617` a `:1632`, lote `:1634`; recusa chama essa porta com `ato='recusa'`, `validacao.py:592` a `:602`. |
| Ressuscitada | Resposta/validação e carimbo retirados; texto pode permanecer neste escritor | `ChamadoColaborador.ressuscitar_perguntas`, `models.py:1513`, materializa projeção antiga em decisões `:1564`, limpa por lote `:1567`. **Diferente da reabertura normal**, que também zera texto em `disputa_emissao.py:1595`. |
| Materializada/encerrada por via | `materializada_em`, `materializada_por`, `via_resolucao` (`models.py:3445`, `:3449`, `:2582`) | Atribuição do trio `_escrever_carimbo`, `models.py:3123`; portas `carimbar` `:3142` a `:3183`, `carimbar_identidade` `:3194`; retirada individual `:3235` a `:3238`; retirada lote `:3306` a `:3308`. |
| Fora do fato | `via_resolucao` em `VIAS_FORA_DO_FATO`; identidade/conversa separadas | Constantes/contrato `models.py:2531` a `:2535`; retirada de identidade no leitor tarefa `tarefa_do_dia.py:240` a `:245`; lote não ressuscita essas linhas `models.py:3306` a `:3308`. |
| Pendência administrativa/recusa | `pendencia_admin` (`''`/`tipo_nao_conferido`), `recusa_motivo` | Campos `models.py:2572` a `:2581`; materialização escreve `materializacao.py:856`, `:1108`; recusa humana `validacao.py:592`; escape humano `disputa_emissao.py:1497`; carimbo identidade aceita motivo `models.py:3214`. Exhaustividade de escritores fora desse domínio: [não verificado]. |
| Lavra de efeito | `veredito_lavrado` `''/sim/nao/indeterminado`, `julgado_em`, `janela_expira_em` (`models.py:2600` a `:2619`) | `veredito_lavrado.lavrar`, `veredito_lavrado.py:254` a `:275`; persistência lote `:247` a `:250`; restauração memória `:284` a `:287`. Leitor tarefa só aceita sim/nao vigentes `tarefa_do_dia.py:410` a `:414`. |
| Sequência de decisões | `DecisaoPergunta.ato`, `ato_em`, autor/texto/motivo/foto/absorção (`models.py:3943` a `:3969`) | Escritor único `DecisaoPergunta.registrar`, `models.py:4029` a `:4041`. Atos: resposta/validação/devolução/recusa/reabertura/silêncio/retratação/absorção. |
| Disputa aberta/fechada | `DisputaSupervisao.fechada_em`, autor, resolução, motivo (`models.py:1758`; escrita `:1939` a `:1943`) | `fechar` `models.py:1862`; reabre admin `:2156` a `:2168`; sistema `:2170`, grava `:2180` a `:2184`. |
| Silêncio encerrado pelo admin | Via admin + decisão `silencio` | `DisputaSupervisao.fechar`, `models.py:1925` a `:1933`. |
| Motivo retratado | Via `motivo_retratado`, decisão `retratacao` | `DisputaSupervisao.remover_motivo`, `models.py:1978`; corpo posterior detalhado [não verificado]. |

As transições que a **tarefa** efetivamente observa são as seguintes; todas são LEU em `app/chamados/services/tarefa_do_dia.py:126` a `:171`:

| Último fato | Retorno da instância |
|---|---|
| Projeção resposta/validação sem decisão correspondente | `passivo_sem_linha`, ações indeterminadas → `nao_sei` |
| Sem decisão | `aguarda_resposta`, responder/colaborador/âncora se na fila |
| Resposta vazia | `respondida_sem_texto`, responder/colaborador |
| Resposta com texto | `aguarda_validacao`, validar/admin |
| Recusa/reabertura | `devolvida_ao_colaborador`, responder/colaborador |
| Devolução de validação | `materializacao_recusada`, validar/admin |
| Validação, lavra ausente/stale/indeterminada | `veredito_sem_lavra_vigente`, `nao_sei` |
| Validação, lavra não | `validada_sem_efeito`, materializar/sistema/materialização |
| Validação, lavra sim e batida de efeito | `efeito_sem_lampada`, rejulgar/sistema/cartório |
| Validação, lavra sim sem batida de efeito | `encerrada_pelo_juiz`, sem ação |
| Silêncio | `silencio_decidido`, sem ação |
| Absorção aprovada | `absorvida_aprovada`, sem ação |
| Absorção rejeitada | `absorcao_rejeitada`, responder/colaborador |
| Absorção aguardando documento | `absorcao_aguarda_documento`, entregar documento/colaborador/sem porta |
| Outra absorção | `absorcao_pendente`, decidir ausência/admin |
| Ato desconhecido | `ato_desconhecido:<ato>`, `nao_sei` |

A tarefa **não persiste seus estados**, ações, rota, fila, verbos ou fio: saída é projeção pura; contrato declarado `tarefa_do_dia.py:36` a `:54`, corpo `_tarefa_pura` `:174`, saída `:192`. A matriz precisa separar “persistência feita pelo evento anterior” de “persistência da consulta”: a última é zero. LEU `tarefa_do_dia.py:52`, `:176`.

Semântica temporal LEU:

- Identidade usa **dia do turno**, `data_evento`, não `data_civil`; seleção `tarefa_do_dia.py:228` a `:246`. O campo `data_civil` é `localtime(instante_do_marco).date()`, `:102` a `:113`. Escritor canônico do dia recebe dia já julgado, `fio.py:829` a `:839`.
- Janela de carga `[ini,fim]` inclusiva: `data_evento__gte` e `__lte`, `tarefa_do_dia.py:384` a `:386`.
- Decisões e efeitos entram no instante **inclusive**: `ato_em <= agora`, `:401`; passivo `ato_em <= agora`, `:408`; Batida `criado_em__lte=agora`, `:420`.
- Fila usa status **atual**, mas silêncio comparado com o `agora` recebido; limite histórico expressamente declarado em `tarefa_do_dia.py:360` a `:369`. Portanto não é reconstrução integral do banco “como estava naquele instante”.
- `fila_ids` e `verbos` não têm o mesmo significado: fila é cobrança visível por `qs_na_fila`; universo vivo inclui registrado e silenciado, `painel.py:181` a `:190`. Testes e contadores devem distinguir esses universos. [nao verificado]
- Na tarefa, lâmpada acesa, dia sem trabalho, teto não vencido e ausência podem decidir antes da pergunta: `tarefa_do_dia.py:270` a `:299`. “Última decisão → ação” só vale depois dessas precedências; marco aceso em dia de trabalho nem consulta a pergunta (`:273` a `:274`).
- Tranca, isenção, fora do vínculo, sem célula, ata sem lâmpadas são retornos globais, `tarefa_do_dia.py:194` a `:225`; chamado vivo na fila ainda deve ser considerado pela etapa de porta posterior, conforme corpo dessa etapa [não verificado neste subcenso].
- O rótulo do modelo `estado` não responde sozinho “quem age”: o próprio código devolve pendente/respondida mesmo com vitalidade negada, `models.py:3067` a `:3074`. Tarefa usa decisões/efeito, `tarefa_do_dia.py:126` a `:171`; tratar as duas coisas como sinônimos é INFERIU conflito semântico.

Não executei Django nem consultei banco/log; resultados de execução no HEAD e inventário absolutamente exaustivo de escritores/leitores em todo `app/`: [não verificado]. [nao verificado]

## 13/11 20:58 · sessao f0874d63e6e6 · - · - · origem=Ronald (tmux)

LEU — mapa dos leitores:

| Leitor | Fonte efetivamente consumida | O que devolve |
|---|---|---|
| C17 `porta_alcancavel_do_dia` | `fila_cel`; perguntas C6 vivas, no app, âncora na fila, sem resposta vazia carimbada | `rota`, `fila`, `verbos`, `fio`; fio sempre por verbos. `app/chamados/juizes.py:1677-1688` |
| Tarefa/porta | Ações com porta inteira na fila + chamados da célula + todas as âncoras das perguntas carregadas | Donos das ações alcançáveis + donos residuais por verbo; prioridade `colab > admin > chamado > sistema`. `app/chamados/services/tarefa_do_dia.py:565-598` |
| Folha `_porta` / `_fio_da_porta` | `porta_da_tarefa(carga.tarefa(...))` | Fio recebido; `None → nada`; não recalcula dono. `app/folha/fora_da_folha.py:71-89` |
| Folha `o_que_segura` | Porta da tarefa | `fio`, texto `DESTRAVA`, `chamado=list(porta['fila'])`; descarta `nao_sei`. `app/folha/fora_da_folha.py:233-242` |
| Vigia/F5 alcance | Porta da tarefa, uma carga | Contagem por rota; `exige_admin = fio == admin`; porta vence teto civil de hoje. `app/chamados/services/vigia_fio.py:637-658` |
| Furo sem canal | Porta da tarefa | Canal existe se porta não é `None`; conta `nao_sei` independentemente da existência da porta. `app/chamados/services/furo_sem_canal.py:87-113` |
| Contador do fio | `qs_na_fila` de todo o colaborador | Conta chamados, sem passar pela porta colab-dia. `app/chamados/views.py:900-902` |

LEU — provas presentes, execução [não verificado]:

- Residual A + ação sob B conserva ambos os verbos e rota da fila: `app/chamados/tests/test_e3_portas_tarefa.py::test_fila_A_pergunta_sob_B`, linhas 48–55; teste puro `ResidualComAcaoTest::test_RED_A_na_fila_aparece_com_acao_sob_B`, linhas 207–212.
- Ação fora da fila não fornece dono: dois casos C na fila/D fora da fila, D registrado ou silenciado, exigem fio `chamado`: `app/chamados/tests/test_o261e_f5b_juiz_porta.py::_o_juiz_pinta_o_verbo_do_painel`, linhas 234–251; testes nas linhas 253 e 257.
- Marco aceso com chamado vivo conserva porta: `app/chamados/tests/test_e3_portas_tarefa.py::test_marco_aceso_com_chamado_vivo_na_fila_mantem_porta_fila`, linha 143.
- Passivo com porta conserva `nao_sei=True`: mesmo arquivo, `test_passivo_sem_linha_porta_visivel_rotulo_nao_sei`, linha 156.
- Porta residual hoje precede teto, sem porta o mesmo dia cai no teto: `app/chamados/tests/test_o261e_f5_alcance.py::test_MORDE_so_a_fila_sem_pergunta_nao_vence_o_teto_de_hoje`, linhas 466–475.
- Leitores migrados deixam de chamar C17: `app/chamados/tests/test_e3_leitores_c17_chamadores.py::test_MORDE_nenhum_chamador_migrado_pergunta_ao_C17`, linhas 182–192.
- Contador do fio igual a `qs_na_fila` do colaborador: `app/chamados/tests/test_fio_contador_ativos_pela_fila.py::test_o_numero_e_o_da_fila_do_painel_para_o_colab`, linhas 59–65.

LEU/INFERIU — conflitos para a matriz:

| Estado | Divergência | Evidência / critério |
|---|---|---|
| Pergunta exige responder; porta viva na fila com módulo GPS cujo verbo é validar | Tarefa `colab`; C17 `admin` | LEU tarefa/teste `app/chamados/tests/test_e3_portas_tarefa.py:84-96`; LEU C17 `app/chamados/juizes.py:1688` e escada `:1628-1639`. Resultado do par INFERIU; LEI-C17(1). |
| Pergunta encerrada; âncora viva B na fila, sem chamado da célula | Tarefa publica `porta_fila`, `fila=()`, B nos verbos; C17 filtra a pergunta morta e pode devolver `None` | LEU teste tarefa `app/chamados/tests/test_e3_portas_tarefa.py:236-245`; LEU filtro C17 `app/chamados/juizes.py:1679-1684`. Comparação INFERIU; aval porta viva / critério (2). |
| Silêncio vencido, chamado aberto/em análise | `qs_na_fila` e contador incluem; verbo histórico, painel padrão esconde; porta existe com `fila=()` | LEU `app/chamados/services/painel.py:188-190`, `:325-331`; `app/chamados/services/verbos.py:48-51`; `app/chamados/views.py:900-902`. Prova de porta histórica `app/chamados/tests/test_o261e_f5b_juiz_porta.py:170-191`. |
| Passivo sem linha com âncora viva na fila | Tarefa publica `fio=admin, nao_sei=True`; folha e vigia publicam fio sem rótulo desconhecido; C17 não possui o campo | LEU teste `app/chamados/tests/test_e3_portas_tarefa.py:156-169`; folha `app/folha/fora_da_folha.py:233-242`; vigia `app/chamados/services/vigia_fio.py:648-654`; C17 `app/chamados/juizes.py:1687-1688`. Prova end-to-end do rótulo: SEM PROVA. |
| Leitura C17 com `agora` diferente do relógio da máquina | Alimentação C6 recebe `agora`, consulta “no app” não recebe | LEU `app/chamados/juizes.py:1601-1604`, `:1620`; `app/chamados/services/pergunta_no_app.py:45-58`; limite declarado no instrumento `app/chamados/management/commands/diff_e3_sombra.py:107`. Efeito num estado concreto: [não verificado]. |
| Corrida entre partição viva e leitura da fila: chamado na segunda leitura, ausente do mapa da primeira | Alimentação descarta chamado, embora segunda leitura o tenha na fila | LEU `app/chamados/juizes.py:1561-1563`; teste assume ausência de porta: `app/chamados/tests/test_o261e_f5b_juiz_porta.py::test_MORDE_membro_da_fila_que_a_particao_nao_viu_nao_abre_porta`, linhas 274–290. Exceção operacional à propriedade incondicional “vivo na fila tem porta”. |

LEU — significados que não são intercambiáveis:

- `fila_ids` significa pertinência a `qs_na_fila`, enquanto `fila` da saída contém somente chamados **da célula** visíveis na lista padrão; âncoras externas e histórico podem existir em `verbos` e faltar em `fila`. `app/chamados/services/tarefa_do_dia.py:567-577`; `app/chamados/juizes.py:1654-1661`.
- O verbo vem do universo `qs_vivos`, porque o crédito da pergunta pode pertencer a destino vivo fora da fila; não se deve particionar somente a fila. `app/chamados/juizes.py:1558-1561`, `:1577-1583`; `app/chamados/services/raiox.py:146-155`.
- Silêncio vigente usa `silenciado_ate > agora`; igualdade já entra na fila e ganha `historico`. `app/chamados/services/painel.py:188-190`; `app/chamados/services/verbos.py:48-51`.
- A janela da carga é fechada, `ini <= dia <= fim`. `app/chamados/services/tarefa_do_dia.py:525-528`.
- O teto do vigia é dia civil local derivado de `agora`, aplicado apenas depois de ausência de porta. `app/chamados/services/vigia_fio.py:567-570`, `:648-658`.
- O contador do fio é por colaborador; contadores de alcance são por dia retido; `len(porta['fila'])` é número de chamados da célula visíveis. Exigir igualdade aritmética entre esses três universos seria INFERIU sem contrato comum. `app/chamados/views.py:900-902`; `app/chamados/services/vigia_fio.py:549-554`; `app/chamados/services/tarefa_do_dia.py:577`.


## 13/11 20:52 · sessao 406035113e6f · E3-FORMA-SQL · construtor · origem=Code

Cura escrita: o fio publicado considera somente ações cuja porta está na fila; C em `decidir` conserva fio `chamado` quando a ação administrativa está sob D fora da fila. Sem porta publicada, o fio interno da E2 foi preservado. ([tarefa_do_dia.py:579](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/services/tarefa_do_dia.py:579)) [nao verificado]

| Teste | Classe | Arquivo:linha | O que mudou | Mutante que morde | [nao verificado]
|---|---|---|---|---|
| `RED_8_chamado_da_celula_sem_acao_exigivel_nao_e_canal` | I | [bloqueantes:148](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/test_e3_bloqueantes.py:148) | Exige `porta_fila` e saída do contador sem canal; comentário cita o aval. | `omitir_vivos` |
| `AB_categorizado_e_diff_por_consumidor` | I | [diff:170](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/test_e3_diff_sombra.py:170) | Exige igualdade com C17, mantendo universo não vazio. | `omitir_vivos` |
| `RED_contraex1_chamado_da_celula_na_fila_sem_pergunta_NAO_e_porta` | I | [beco:166](/home/ronald/wt-codex-e3-forma-sql-195919/app/escala/tests/test_f3_beco1_folga_na_lista.py:166) | Exige porta completa e ausência no beco; comentário cita o aval. | `omitir_vivos` |
| `MORDE_destino_registrado_o_verbo_de_C_e_o_do_painel` | II | [F5B:257](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/test_o261e_f5b_juiz_porta.py:257) | Asserts preservados; correção dos donos em produção. | `fio_v6` |
| `MORDE_destino_silenciado_vigente_o_verbo_de_C_e_o_do_painel` | II | [F5B:253](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/test_o261e_f5b_juiz_porta.py:253) | Asserts preservados; mesma correção. | `fio_v6` |
| `so_a_fila_sem_pergunta_e_chamado` | I | [F5B:159](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/test_o261e_f5b_juiz_porta.py:159) | Exige porta e fio `chamado`, conservando a ausência de perguntas. | `omitir_vivos` |
| `MORDE_o_mesmo_dia_so_com_chamado_na_fila_SEM_pergunta_segue_sem_porta` | I | [alcance:152](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/test_o261e_f5_alcance.py:152) | Exige uma porta, zero mortos e soma preservada. | `omitir_vivos` |
| `MORDE_o_papel_SEGUE_o_juiz_e_nao_uma_tabela_do_vigia` | I | [alcance:366](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/test_o261e_f5_alcance.py:366) | Atualiza o residual; mantém a prova com `fio-xyz`. | `omitir_vivos`, `vigia_rejulga_fio` |
| `MORDE_so_a_fila_sem_pergunta_nao_vence_o_teto_de_hoje` | I | [alcance:466](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/test_o261e_f5_alcance.py:466) | Mesmo dia: sem porta `(0,1)`; com vivo `(1,0)`; ambas as somas conferidas. | `omitir_vivos`, `teto_antes_porta`, `teto_removido` |
| `chamado_vivo_SEM_porta_da_tarefa_nao_e_canal` | I | [O66:105](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/test_o66_furo_sem_canal.py:105) | Exige saída da amostra e uma porta de fila. | `omitir_vivos` |
| `fachada_candidata_conserva_campos_exigiveis` | III | [diff:131](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/test_e3_diff_sombra.py:131) | Compara os quatro campos protegidos e exige `nao_sei` separadamente, em `True` e `False`. O campo permanece publicado conforme o aval. | `projecao_sem_nao_sei`, `projecao_fio_errado` | [nao verificado]
| `todo_mutante_da_E3_tem_alvo_unico` | II — executor | [catraca:15](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/test_e3_mutantes_alvo.py:15) | Corrige três alvos obsoletos e exige registro dos 15 mutantes v6. | `alvos_antigos`, `registro_v6_omitido` |

A classificação II da última linha corresponde à quebra real do executor pela v6: foram corrigidos os alvos, sem inverter a exigência de alvo único. ([e3_mutantes.py:89](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/e3_mutantes.py:89))

Os comandos usam:

```bash
m() { env -u DJANGO_SETTINGS_MODULE PYTHONDONTWRITEBYTECODE=1 python3 -m "$@"; }
```

| Comando | Resultado observado |
|---|---|
| `m unittest chamados.tests.test_e3_volta7_sem_banco` | Ran 12; OK. Corpos dos selos executados com fornecedores sintéticos, sem fixtures ORM. ([executor:18](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/test_e3_volta7_sem_banco.py:18)) |
| `m chamados.tests.e3_volta7_mutantes --mutante omitir_vivos` | Controle: Ran 8, OK. Cura revertida em memória: Ran 8, FAILED=8, ERROR=0. ([saída:131](/home/ronald/wt-codex-e3-forma-sql-195919/app/e3-v7-mutantes.out:131)) |
| `m chamados.tests.e3_volta7_mutantes --mutante fio_v6` | Controle: Ran 2, OK. Fio v6 restaurado: Ran 2, FAILED=2, ERROR=0. ([saída:196](/home/ronald/wt-codex-e3-forma-sql-195919/app/e3-v7-mutantes.out:196)) |
| `m chamados.tests.e3_volta7_mutantes --mutante todos` | 12/12 mutantes detectados; cada alvo passa antes e falha por assert depois, ERROR=0. ([critério:109](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/e3_volta7_mutantes.py:109), [saída final:502](/home/ronald/wt-codex-e3-forma-sql-195919/app/e3-v7-mutantes.out:502)) |
| `m chamados.tests.e3_portas_v6_mutantes --mutante todos` | **15/15 detectados**, ERROR=0; cada controle Ran 1, OK. ([saída:26](/home/ronald/wt-codex-e3-forma-sql-195919/app/e3-v6-mutantes.out:26), [último:586](/home/ronald/wt-codex-e3-forma-sql-195919/app/e3-v6-mutantes.out:586)) |
| `m chamados.tests.e3_portas_regra` | Ran 10; OK. ([executor:69](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/e3_portas_regra.py:69)) |

O conjunto abaixo deu **Ran 70; OK**. ([saída:3](/home/ronald/wt-codex-e3-forma-sql-195919/app/e3-v7-green.out:3))

```bash
m unittest \
  chamados.tests.test_e3_tarefa_admin_sem_banco \
  chamados.tests.test_e3_c8b_sem_banco \
  chamados.tests.test_e3_c8c_sem_banco \
  chamados.tests.test_e3_pos_suite_sem_banco \
  chamados.tests.test_e3_portas_v6_sem_banco \
  chamados.tests.test_e3_volta7_sem_banco
```

Estes laços fornecem o comando individual de cada mutante. RC 1 significa mordida válida; RC 2 significa prova inválida. ([v7:127](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/e3_volta7_mutantes.py:127), [v6:130](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/e3_portas_v6_mutantes.py:130))

```bash
for n in omitir_vivos fio_v6 teto_antes_porta teto_removido \
  vigia_rejulga_fio projecao_sem_nao_sei projecao_fio_errado \
  legado_fila_residual legado_fila_A_em_vez_B legado_rota_fila_sempre \
  alvos_antigos registro_v6_omitido
do
  m chamados.tests.e3_volta7_mutantes --mutante "$n"
done

for n in razao_esconde sistema_esconde residual_A_some ancora_some \
  ancora_so_exposta fora_da_fila_entra historico_nomeado verbo_inventado \
  fio_unificado_pela_escada fio_residual_admin fio_residual_ignorado \
  fio_acao_sem_porta_ignorada encerrada_rota_pergunta \
  projecao_sem_nao_sei contador_so_sem_porta
do
  m chamados.tests.e3_portas_v6_mutantes --mutante "$n"
done
```

O teto tem duas provas adicionais por assert: antecipá-lo à porta e removê-lo do controle sem porta. A remoção também amplia, em memória, a carga de permissões necessária ao ramo seguinte. ([mutação:74](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/e3_volta7_mutantes.py:74), [RED antecipação:227](/home/ronald/wt-codex-e3-forma-sql-195919/app/e3-v7-mutantes.out:227), [RED remoção:262](/home/ronald/wt-codex-e3-forma-sql-195919/app/e3-v7-mutantes.out:262))

“Escreve” abaixo significa compor a projeção em memória. ([tarefa_do_dia.py:563](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/services/tarefa_do_dia.py:563))

| Campo | Quem escreve/fornece | Quem lê |
|---|---|---|
| `rota` | `_portas_da_tarefa`, [tarefa:588](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/services/tarefa_do_dia.py:588) | [vigia:652](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/services/vigia_fio.py:652), [furo:97](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/services/furo_sem_canal.py:97), [diff:53](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/management/commands/diff_e3_sombra.py:53). |
| `fila` | `alimentar_fila` fornece pertença; tarefa seleciona a lista visível: [juiz:1549](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/juizes.py:1549), [tarefa:577](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/services/tarefa_do_dia.py:577) | [folha:239](/home/ronald/wt-codex-e3-forma-sql-195919/app/folha/fora_da_folha.py:239), [diff:50](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/management/commands/diff_e3_sombra.py:50). |
| `verbos` | Partição do painel; seleção pela tarefa: [verbos:52](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/services/verbos.py:52), [tarefa:576](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/services/tarefa_do_dia.py:576) | Dono residual em [tarefa:585](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/services/tarefa_do_dia.py:585); assinatura do [diff:144](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/management/commands/diff_e3_sombra.py:144). |
| `fio` | Donos alcançáveis + residual + prioridade: [tarefa:581](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/services/tarefa_do_dia.py:581) | [folha:89](/home/ronald/wt-codex-e3-forma-sql-195919/app/folha/fora_da_folha.py:89), [vigia:653](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/services/vigia_fio.py:653); pauta indiretamente em [pautas:81](/home/ronald/wt-codex-e3-forma-sql-195919/app/ponto/services/pautas_dos_discordantes.py:81). |
| `nao_sei` | `_tarefa_pura`, inclusive passivo; publicado por `porta_da_tarefa`: [tarefa:319](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/services/tarefa_do_dia.py:319), [projeção:598](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/services/tarefa_do_dia.py:598) | O contador lê a **tarefa original** em [furo:98](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/services/furo_sem_canal.py:98); o selo lê o campo publicado em [diff/teste:141](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/test_e3_diff_sombra.py:141). | [nao verificado]

O beco lê a existência da porta, sem recalcular seus campos. ([cadastro_realidade.py:497](/home/ronald/wt-codex-e3-forma-sql-195919/app/escala/services/cadastro_realidade.py:497))

A tentativa dos sete módulos Django deu **Ran 7; FAILED(errors=7)** na importação, sem executar os casos ORM. Execução Django efetiva, suite completa e efeito em produção: **[nao verificado]**. ([saída:3](/home/ronald/wt-codex-e3-forma-sql-195919/app/e3-v7-django.out:3), [resultado:94](/home/ronald/wt-codex-e3-forma-sql-195919/app/e3-v7-django.out:94)) [nao verificado]

_4842 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
