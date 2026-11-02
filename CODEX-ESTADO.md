# CODEX-ESTADO

> Ultima resposta processada: **01/11/2026 20:45** · publicado em **02/11/2026 18:01** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 01/11 20:45 · sessao 6bee4d54689e · COBERTURA-PARCIAL-UM-JUIZ · verificador · origem=Code

**Pré-verificação: bloqueada.** Há perda dos carimbos no legacy, emissores fora do predicado comum e perda do instante anterior em passadas do mesmo ato. Esses pontos impedem confirmar as intenções 1–3 nos caminhos completos (`app/escala/utils.py:1060`; `app/ponto/services/cluster_espurio.py:88`; `app/chamados/services/disputa_emissao.py:763`; `app/ponto/services/cartorio.py:914`).

A equivalência da cópia com o hash/base informados, a suite Django, o deploy, os hashes reais de produção e a incidência dos contracasos abaixo estão **[nao verificado]**. [nao verificado]

1. **BLOQUEIA — A/F/G: o legacy perde a identificação da ausência parcial e pode dispensar o resto indevidamente.**

   `_marcar_ausencias` escreve `cobertura_parcial` e `periodo_parcial`, mas o legacy remonta o resultado copiando apenas alguns campos: os dois carimbos ficam para trás, enquanto as células `dispensada` sobrevivem. O dispatcher continua oferecendo esse caminho quando a flag está falsa (`app/escala/utils.py:1247`; `app/escala/utils.py:1251`; `app/escala/utils.py:1058`; `app/escala/utils.py:1060`; `app/escala/utils.py:2106`).

   Contracaso: parcial 14–17, E13 ainda apagado e S17 dispensado. No resultado legacy falta `cobertura_parcial`; `dispensar_por_ausencia` interpreta essa ausência de chave como cobertura integral e acende também E13. A ata deixa de carregar os carimbos, `parcial_so_lista` retorna falso e `dia_sem_trabalho` volta a calar o dia inteiro (`app/ponto/services/cartorio.py:719`; `app/ponto/services/cartorio.py:726`; `app/ponto/services/cartorio.py:691`; `app/ponto/services/cartorio.py:368`; `app/chamados/juizes.py:424`).

   Os testes da volta 6 não mordem esse defeito: criam ausências com `minutos=0` e verificam férias, aviso, falta e rótulo da ata, sem testar parcial e preservação dos carimbos nas duas flags (`app/escala/tests/test_cobparc6_ausencia_sem_vinculo.py:55`; `app/escala/tests/test_cobparc6_ausencia_sem_vinculo.py:63`; `app/escala/tests/test_cobparc6_ausencia_sem_vinculo.py:83`; `app/escala/tests/test_cobparc6_ausencia_sem_vinculo.py:99`).

2. **BLOQUEIA — B/D/F: cluster nasce da célula e não lê `parcial_so_lista`.**

   O julgamento chama o reconciliador de cluster depois da lavra. Esse serviço lê a ata, passa por `dia_sem_trabalho`, procura duas órfãs próximas e alguma lâmpada apagada, e abre chamado diretamente. Não consulta o predicado comum (`app/ponto/services/cartorio.py:931`; `app/ponto/services/cluster_espurio.py:23`; `app/ponto/services/cluster_espurio.py:28`; `app/ponto/services/cluster_espurio.py:32`; `app/ponto/services/cluster_espurio.py:88`).

   Contracaso: colaborador ativo, vínculo ativo, ata de ausência parcial sem período, duas órfãs separadas por até 15 minutos e uma lâmpada apagada. A exceção nova faz `dia_sem_trabalho=False`; o cluster pode abrir chamado embora `parcial_so_lista=True`. A guarda global de absolvição não resolve isso: restringe-se a `MODULOS_DE_TURNO` (`app/ponto/services/cluster_espurio.py:77`; `app/ponto/services/cluster_espurio.py:79`; `app/chamados/juizes.py:424`; `app/ponto/services/cartorio.py:370`; `app/chamados/models.py:1061`).

   Isso viola a fronteira literal pedida — parcial sem período nunca vai a chamado — mesmo sendo um chamado de supervisão. Ocorrência nos quatro colaboradores apresentados: **[nao verificado]**.

3. **BLOQUEIA — B: o passivo também pergunta pelos marcos da ata sem aplicar a regra comum.**

   `pergunta_no_app.perguntar` encaminha o passivo para `criar_disputa_a_partir_de_chamados_orfaos`. Essa função abre o pai antes de consultar a ata do dia e complementar os marcos faltantes (`app/chamados/services/pergunta_no_app.py:148`; `app/chamados/services/pergunta_no_app.py:159`; `app/chamados/services/disputa_emissao.py:908`; `app/chamados/services/disputa_emissao.py:974`; `app/chamados/services/disputa_emissao.py:978`).

   A guarda da pergunta usa `_coberto_por_ausencia`, que passou a deixar passar parcial **com período**, sem consultar julgamento anterior ou fato novo. Portanto, um chamado preexistente pode ganhar perguntas para o resto de um dia que `parcial_so_lista` mandaria à lista. Na parcial **sem período**, a pergunta é barrada, mas o pai já pode ter nascido (`app/chamados/services/disputa_emissao.py:763`; `app/chamados/services/disputa_emissao.py:770`; `app/chamados/services/materializacao.py:78`; `app/chamados/services/materializacao.py:81`; `app/ponto/services/cartorio.py:372`).

   A exclusão do homônimo no censo não prova que esse fluxo esteja fora da obrigação: ele efetivamente lê os marcos da célula para criar perguntas (`app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:462`; `app/chamados/services/disputa_emissao.py:806`).

4. **BLOQUEIA — C: o instante anterior foi preservado na fábrica, mas não no corpo inteiro nem no c2.**

   | Leitor | Instante efetivamente usado |
   |---|---|
   | Primeira passada do corpo | `cel.julgada_em` anterior, antes do carimbo final (`app/ponto/services/cartorio.py:1618`; `app/ponto/services/cartorio.py:1733`). |
   | Fábrica dentro de `julgar_colab` | Snapshot anterior correto, inclusive quando seu valor é `None` (`app/ponto/services/cartorio.py:888`; `app/ponto/services/cartorio.py:938`; `app/chamados/services/fabrica_da_celula.py:102`). |
   | Segunda passada forçada | Mesmas instâncias, potencialmente já modificadas por `carimbar`; não recebe o snapshot (`app/ponto/services/cartorio.py:914`; `app/ponto/portas/celula.py:800`). |
   | Releitura pós-emissão | Também recebe as instâncias já carimbadas (`app/ponto/services/cartorio.py:979`). |
   | Lote | Último julgamento persistido antes da leitura do lote (`app/chamados/management/commands/disparar_perguntas_competencia.py:120`). |
   | c2 | Ata e `julgada_em` atuais do banco; pode ser o julgamento deste ato (`app/chamados/reconciliador.py:844`). |
   | Retroativo | Célula carregada antes do passe; o registro de cobertura, porém, vem do leitor que recalcula — achado 5 (`app/ponto/management/commands/emitir_furo_retroativo.py:69`; `app/ponto/management/commands/emitir_furo_retroativo.py:121`). |

   Quando o carimbo avança para `agora`, o intervalo de fato novo passa a começar nesse próprio ato. Para parcial com período, a segunda leitura pode mudar de “emite” para “só lista”; sem período, continua sempre “só lista” (`app/ponto/portas/celula.py:800`; `app/ponto/services/cartorio.py:370`; `app/ponto/services/cartorio.py:372`).

   Há um contracaso de fluxo: a ausência nova dispensa os marcos de uma cobrança antiga, deixando outro marco de fora ainda apagado. A cobrança antiga com fio é encerrada somente depois da primeira lavra; a segunda passada, que deveria poder cobrar o resto, já lê o carimbo novo. A fábrica conserva o snapshot anterior, mas o c2 volta a consultar o carimbo atual. Assim, fábrica e emissão podem decidir por instantes diferentes dentro do mesmo julgamento (`app/ponto/services/cartorio.py:1396`; `app/ponto/services/cartorio.py:899`; `app/ponto/services/cartorio.py:914`; `app/ponto/services/cartorio.py:938`; `app/chamados/reconciliador.py:845`). Reprodução integrada desse contracaso: **[nao verificado]**.

5. **BLOQUEIA — A/D: o leitor da célula continua derivando cobertura/período em vez de transportar a lâmpada lavrada.**

   A ata agora grava ambos os carimbos, mas `grade_da_celula` não os copia ao construir o dia. Depois consulta novamente a ausência atual e recalcula cobertura e período — inclusive no trecho alterado pelo diff (`app/ponto/services/cartorio.py:691`; `app/ponto/services/cartorio.py:696`; `app/escala/services/leitor_celula.py:370`; `app/escala/services/leitor_celula.py:385`; `app/escala/services/leitor_celula.py:404`; `app/escala/services/leitor_celula.py:409`; `app/escala/services/leitor_celula.py:420`).

   Não é um segundo algoritmo de período: chama o mesmo juiz. É uma **segunda derivação em outro momento**, misturando lâmpadas persistidas com cadastro atual. Ata `sem_periodo` e cadastro já corrigido podem produzir respostas diferentes na fábrica/lote/c2 e no retroativo. A propriedade que compara grade e ata recém-produzidas não testa essa divergência temporal (`app/ponto/services/cartorio.py:346`; `app/escala/services/leitor_celula.py:404`; `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:247`). Incidência real dessa janela: **[nao verificado]**.

6. **FILA — B/D: a mordida dos mutantes AST é real, mas estreita; o universo não está fechado.**

   O censo reconhece as portas enumeradas, resolve `ImportFrom` e trata somente `_abrir_chamado` como chamada de método. Não reconhece emissão genérica por `ChamadoColaborador.abrir`, `PerguntaDisputa.abrir` nem chamada por módulo importado (`app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:311`; `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:353`; `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:388`; `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:394`).

   Além disso, qualquer chamada sintática ao predicado absolve a função: não se exige que seu resultado proteja a emissão. `if False: parcial_so_lista(...)` seguido de emissão continua conforme segundo esse algoritmo (`app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:390`; `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:403`).

   Os mutantes escritos verificam retirada da chamada, retirada da delegação e novo emissor pela porta conhecida. Isso sustenta essas propriedades sintáticas, não a afirmação universal sobre emissores e fluxo (`app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:455`; `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:465`; `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:472`).

   Outras portas exigem fronteira explícita:

   | Caminho | Situação |
   |---|---|
   | `detectar_ausencias` | Não lê o predicado; a exceção usa guarda mais restritiva que cala qualquer ausência (`app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:334`; `app/ponto/management/commands/detectar_ausencias.py:346`). |
   | `reconciliar_grade` | Emissão direta omitida; sua guarda cala ausência não-falta, portanto não demonstra vazamento da parcial por essa porta (`app/ponto/management/commands/reconciliar_grade.py:215`; `app/ponto/management/commands/reconciliar_grade.py:391`; `app/ponto/management/commands/reconciliar_grade.py:507`). |
   | Retroativo de turno aberto | Não lê o predicado, mas cala qualquer ausência antes da abertura (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:108`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151`). |
   | Par relâmpago | Abre diretamente; origem é batida/par, exigindo distinguir essa fronteira da emissão pela ata (`app/ponto/management/commands/detectar_par_relampago.py:352`; `app/ponto/management/commands/detectar_par_relampago.py:368`). |
   | Régua degenerada | Chamado de cadastro; não é cobrança de marco (`app/escala/regua_defesa.py:36`; `app/escala/regua_defesa.py:51`). |
   | Declaração explícita | Também cria disputa/pergunta pelos marcos; há gesto humano, que precisa ser distinguido da emissão automática (`app/chamados/services/declaracao_texto.py:288`; `app/chamados/services/declaracao_texto.py:310`; `app/chamados/services/declaracao_texto.py:600`). |
   | Fio/reapontamento | O c2 dispensa a leitura de PSL ao costurar em chamado já vivo; isso é diferente de nascer/renascer (`app/chamados/reconciliador.py:805`; `app/chamados/reconciliador.py:842`). A ressuscitação de perguntas também ocorre antes do portão PSL do corpo (`app/ponto/services/cartorio.py:1536`; `app/ponto/services/cartorio.py:1618`). |

7. **FILA — A/D: ainda há leitores que descartam o resto parcial pelo rótulo `ausencia`.**

   O relatório alterado aceita ausência parcial, mas `furos_diarios` continua pulando todo dia que não seja `trabalho`; os calculadores de preenchimento e marcações fazem o mesmo. Portanto o resto pode aparecer no relatório e faltar no acompanhamento/indicadores (`app/relatorios/furos.py:154`; `app/escala/services/furos_diarios.py:39`; `app/inteligencia/calculadores.py:46`; `app/inteligencia/calculadores.py:141`).

   No acompanhamento persistido, essa seleção também alimenta a remoção de furos cuja premissa desapareceu. Não é apenas diferença visual (`app/escala/services/furos_diarios.py:89`; `app/escala/services/furos_diarios.py:98`).

8. **NOTA — E: a v4 não seleciona dias parciais; invalida impressões de toda a competência 09 em diante.**

   `versao_para` devolve a maior versão cujo horizonte alcança a competência empresarial. Assim, 09 passa de 1 para 4 e 10 passa de 3 para 4; não há filtro por ausência ou por colaborador nominal. A versão entra no hash dos dois ramos do cartório (`app/ponto/services/cartorio.py:111`; `app/ponto/services/cartorio.py:235`; `app/ponto/services/cartorio.py:238`; `app/ponto/services/cartorio.py:443`; `app/ponto/services/cartorio.py:1113`; `app/ponto/services/cartorio.py:1266`).

   Na próxima execução, células passadas com impressão antiga deixam de ser puladas sem `--forcar`. O comando percorre todos os colaboradores com célula passada da empresa, sem filtro de situação. A configuração confirma execução automática por cron `--apply`; disparo imediato pelo deploy está **[nao verificado]** (`app/ponto/management/commands/processar_cartorio.py:49`; `app/ponto/management/commands/processar_cartorio.py:61`; `app/ponto/services/cartorio.py:1272`; `app/config/crons.py:597`).

   O rejulgamento usa **todo o código atual**: pode mover folga/trabalho, ata, protestos, veredito/status, chamados, perguntas e fios além dos dias parciais. O próprio teste cobre folga→trabalho na 09 sem forçar (`app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:146`; `app/ponto/services/cartorio.py:899`; `app/ponto/services/cartorio.py:935`; `app/ponto/services/cartorio.py:938`; `app/ponto/services/cartorio.py:1713`).

   Mesmo mantendo o veredito, o hash novo pode avançar `julgada_em`. Fora do diff, isso invalida lavras de perguntas e torna janelas históricas novamente elegíveis no censo de fase 12x36 (`app/ponto/portas/celula.py:796`; `app/ponto/portas/celula.py:800`; `app/chamados/veredito_lavrado.py:151`; `app/chamados/veredito_lavrado.py:187`; `app/escala/services/fase_12x36.py:188`).

   **No percurso de rejulgamento inspecionado**, a porta grava célula/outbox, e o consumidor atua em chamados/perguntas. Há guarda explícita para impedir recalcular o gravado da 09 exportada; o teste verifica FM inteiro e exportações com premissas não vazias. Isso não prova hashes reais nem execução em produção (`app/ponto/portas/celula.py:634`; `app/chamados/reconciliador.py:1310`; `app/ponto/services/fechamento.py:36`; `app/ponto/services/fechamento.py:544`; `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:118`). FM/TXT reais antes/depois: **[nao verificado]**.

9. **NOTA — A: mapa de escrita e leitura dos estados alterados.**

   | Campo/valor | Quem escreve | Quem lê e efeito relevante |
   |---|---|---|
   | `dia.cobertura_parcial` | Overlay da grade; também recalculado no leitor da célula (`app/escala/utils.py:1247`; `app/escala/services/leitor_celula.py:409`). | Supra, cartório, status, relatórios e predicado da lista (`app/ponto/supra_juiz.py:268`; `app/ponto/services/cartorio.py:719`; `app/ponto/status_dia.py:158`; `app/relatorios/furos.py:154`; `app/ponto/services/cartorio.py:368`). |
   | `dia.periodo_parcial` | Overlay pelo juiz de período; novamente derivado no leitor (`app/escala/utils.py:1251`; `app/escala/services/leitor_celula.py:420`). | Supra, impressão e PSL (`app/ponto/supra_juiz.py:381`; `app/ponto/services/cartorio.py:408`; `app/ponto/services/cartorio.py:370`). |
   | Carimbos correspondentes na ata | `ata_do_dia` copia somente quando há cobertura parcial; persistência pela lavra/carimbo (`app/ponto/services/cartorio.py:691`; `app/ponto/services/cartorio.py:696`; `app/ponto/services/cartorio.py:1713`; `app/ponto/services/cartorio.py:1733`). | Fábrica, lote, c2, `dia_sem_trabalho` e calendário (`app/chamados/services/fabrica_da_celula.py:102`; `app/chamados/management/commands/disparar_perguntas_competencia.py:120`; `app/chamados/reconciliador.py:845`; `app/chamados/juizes.py:424`; `app/colaboradores/services/calendario.py:828`). |
   | `celula.dispensada` / `lampada.dispensada` | Grade dispensa missing dentro do período; cartório propaga; dispensa integral/justificativa também escreve (`app/escala/utils.py:1262`; `app/ponto/services/cartorio.py:520`; `app/ponto/services/cartorio.py:729`). | Leitor reconstrói marcador; contagens, status e cartório excluem dispensa de batida real (`app/escala/services/leitor_celula.py:127`; `app/escala/services/leitor_celula.py:91`; `app/ponto/status_dia.py:153`; `app/ponto/services/cartorio.py:1288`). |
   | `missing`, `acesa`, `luz`, `n_missing`, `n_celulas` | Grade altera missing; cartório monta lâmpadas/contagens e pode dispensá-las (`app/escala/utils.py:1263`; `app/ponto/services/cartorio.py:636`; `app/ponto/services/cartorio.py:671`; `app/ponto/services/cartorio.py:685`; `app/ponto/services/cartorio.py:726`). | Supra, lastro, fábrica, cluster e relatórios (`app/ponto/supra_juiz.py:306`; `app/ponto/services/lastro.py:129`; `app/chamados/services/fabrica_da_celula.py:49`; `app/ponto/services/cluster_espurio.py:31`; `app/relatorios/furos.py:154`). |
   | `n_ok`, `n_casadas` | Derivados da ata pelo leitor; não são novo campo persistente (`app/escala/services/leitor_celula.py:91`; `app/escala/services/leitor_celula.py:97`; `app/escala/services/leitor_celula.py:371`). | Supra prefere o número lavrado; corte de admissão/demissão e diagnóstico também mudam (`app/ponto/supra_juiz.py:229`; `app/ponto/supra_juiz.py:464`; `app/escala/services/leitor_celula.py:343`; `app/ponto/services/bordas_realizado.py:51`). |
   | `CelulaDia.ata` | Porta `lavrar_veredito`; operação própria para a chave `precedencia` (`app/ponto/portas/celula.py:709`; `app/ponto/portas/celula.py:768`; `app/ponto/portas/celula.py:811`). | Leitor da célula, juízes, lastro, perguntas, calendário e cluster — incluindo os consumidores fora do diff dos achados 2, 3 e 7 (`app/escala/services/leitor_celula.py:342`; `app/chamados/juizes.py:463`; `app/ponto/services/lastro.py:118`; `app/ponto/services/cluster_espurio.py:24`). |
   | `CelulaDia.julgada_em` | `carimbar`, com guarda de idempotência (`app/ponto/portas/celula.py:796`; `app/ponto/portas/celula.py:800`). | Impressão, PSL e seus emissores; invalidação de perguntas e fase, descritas nos achados 4 e 8 (`app/ponto/services/cartorio.py:1272`; `app/ponto/services/cartorio.py:372`; `app/chamados/veredito_lavrado.py:154`; `app/escala/services/fase_12x36.py:189`). |
   | `REGRA_VERSOES` / `regra` | Declaração no módulo; validação na carga (`app/ponto/services/cartorio.py:111`; `app/ponto/services/cartorio.py:209`). | `versao_para` → `_regra_do_dia` → impressão dos dois ramos (`app/ponto/services/cartorio.py:238`; `app/ponto/services/cartorio.py:1061`; `app/ponto/services/cartorio.py:1113`; `app/ponto/services/cartorio.py:1266`). |
   | `julgada_antes` | Snapshot local por data; sentinela diferencia omissão de `None` explícito (`app/ponto/services/cartorio.py:888`; `app/chamados/services/fabrica_da_celula.py:71`). | `perguntar_pela_celula` repassa a `motivos_a_perguntar`; não alcança corpo/c2 (`app/chamados/services/fabrica_da_celula.py:164`; `app/chamados/services/fabrica_da_celula.py:103`). |
   | `impressao`, `ausencia`, `tem_entrada`, `tem_saida`, `insumos_em` | Continuam juntos em `carimbar`; entrada/saída vêm da ata transitória (`app/ponto/services/cartorio.py:1732`; `app/ponto/portas/celula.py:801`; `app/ponto/portas/celula.py:805`). | Pulo por impressão e leitores da célula; a obra muda a impressão por versão/período/índices dispensados (`app/ponto/services/cartorio.py:1272`; `app/ponto/services/cartorio.py:399`; `app/ponto/services/cartorio.py:443`). |
   | Veredito, prote

[truncado em 20000 de 30171 caracteres]

## 01/11 20:41 · sessao 3d0aefa7323b · - · - · origem=Ronald (tmux)

1. **BLOQUEIA — cluster é emissor omitido e passa a alcançar parcial sem período.** `cartorio.py:931-935` chama `cluster_espurio.reconciliar_colab` após lavrar; `cluster_espurio.py:23-35` lê ATA, exige duas órfãs próximas e lâmpada apagada; `:88` abre chamado sem `parcial_so_lista`. A mudança em `chamados/juizes.py:423-426` faz `dia_sem_trabalho` devolver False para a parcial. Contracaso: colaborador ativo, EC ativa, parcial sem período, duas órfãs em até 15 min e missing → chamado de cluster apesar de PSL=True. As guardas globais não abrangem cluster: `chamados/models.py:1061`, `chamados/reconciliador.py:336`, `chamados/catalogo/furo_modulos.py:15`. Ocorrência em produção: [nao verificado].

2. **FILA/BLOQUEIA conforme fronteira exigida — passivo pergunta pela ATA sem PSL.** `pergunta_no_app.py:148-159` chama `disputa_emissao.criar_disputa_a_partir_de_chamados_orfaos`; esta cria pai em `:908-932`, lê ATA em `:974`, complementa marcos em `:978` e pergunta por `_criar_pergunta_se_couber:756-780`. A guarda `:763` usa `_coberto_por_ausencia`; parcial COM período agora devolve False (`materializacao.py:76-82`), independentemente de fato novo. Assim pode perguntar o resto de dia já julgado que PSL mandaria à lista. SEM período cala perguntas nessa guarda, mas pai pode ter sido criado antes dela. Esse caminho parte de chamado existente e foi excluído nominalmente pelo teste de homônimo (`test_cobparc5...py:454`); não está protegido pela propriedade geral anunciada. `declaracao_texto.declarar_hora_colab:288,310,325` e `aplicar_declaracao:600,608,614` também criam pai/pergunta a partir de marcos da célula sem PSL, porém mediante declaração/toque explícito. [nao verificado]

3. **FILA — censo prova presença sintática, não universo nem proteção do fluxo.** `_imports` reconhece apenas `ImportFrom` (`test_cobparc5...py:353`); chamadas por módulo não entram. `censo:394` só reconhece método `_abrir_chamado`, omitindo `ChamadoColaborador.abrir`, `PerguntaDisputa.abrir` e as portas de disputa. `violacoes:403` aceita qualquer chamada ao predicado, inclusive sob `if False`, sem exigir retorno ou domínio sobre emissão. Executei em memória os nove métodos AST reais: **9/9 OK**; os três contramutantes “via módulo”, “PSL inalcançável” e “porta genérica direta” também escaparam. Isso confirma mordida real e estreita dos mutantes escritos, sem equivaler à suite Django. Código das verificações: `test_cobparc5...py:374-413,430-484`. [nao verificado]

4. **C — snapshot correto só para fábrica; demais passadas podem ler julgamento do próprio ato.** `julgar_colab:888` captura anterior e passa-o à fábrica em `:938`; fábrica usa explicitamente esse argumento em `fabrica_da_celula.py:102-103`. Primeira passagem do corpo lê `cel.julgada_em` em `cartorio.py:1618`, antes de `carimbar:1733`; a porta escreve e modifica a instância em `portas/celula.py:800`. Segunda passagem forçada (`cartorio.py:914`) e releitura pós-emissão (`:979`) recebem as mesmas instâncias: se o primeiro carimbo mudou, PSL lê **agora**, não o anterior. COM período, sem fato posterior ao carimbo, passa a True; pode calar tentativa na segunda passagem. SEM período continua True independentemente do instante (`:370-372`). Efeito concreto em produção: [nao verificado].

5. **C — c2 lê o carimbo atual, inclusive dentro da fábrica do próprio julgamento.** `disputa_emissao.py:404-409` chama reapontamento após criar pergunta; `reconciliador.py:1531` chega ao c2, que consulta ATA e `julgada_em` atuais em `:844-845`, sem snapshot nem `agora`. Se fábrica rodou após carimbo, c2 vê o julgamento deste ato. COM período, um fato que habilitou fábrica pelo snapshot anterior pode já ter sido consumido pelo carimbo e não habilitar c2. Chamado do dia já vivo pula PSL por `_costura_no_vivo` (`:805-806,842`), o que preserva costura. O fallback temporal é relógio próprio: `cartorio.houve_fato_novo:317`. Lote lê julgamento anterior ao lote (`disparar_perguntas_competencia.py:120`); retroativo lê célula carregada antes do passe (`emitir_furo_retroativo.py:69,121`). [nao verificado]

6. **Portas adicionais, com alcance distinguido.**
   - `detectar_par_relampago.py:352-368`: abre chamado sem PSL; origem é batida/par completo, não ATA. Precisa fronteira explícita do universo.
   - `reconciliar_grade.py:250-254,488-530`: emissão direta omitida pelo AST; `_coberto:391-403`, consultado em `:216`, cala toda ausência não-falta, inclusive parcial. Não demonstra vazamento da parcial não-falta por esse caminho.
   - `reconciliar_grade.py:412-428`: pai de disputa por porta omitida.
   - `regua_defesa.py:36-78`: abre chamado de **cadastro** `regua_degenerada`, não cobrança de marco; exclusão legítima exige declarar essa fronteira.
   - `emitir_turno_aberto_retroativo.py:108-110`: não lê PSL, mas cala qualquer ausência antes de abrir em `:151`.
   - `detectar_ausencias.py:346-361`: allow tem fundamento na guarda de qualquer ausência; sua exceção enfraquece a formulação literal “todo emissor lê o mesmo predicado”, embora seja mais restritiva na emissão.

## 01/11 20:41 · sessao ebbf0da4d5d1 · - · - · origem=Ronald (tmux)

1. **Bloqueia — o leitor da célula ainda recalcula cobertura/período, embora a ata agora os grave.** `ata_do_dia` passa a transportar `cobertura_parcial` e `periodo_parcial` (`app/ponto/services/cartorio.py:689-696`), mas `grade_da_celula` não os copia ao construir o dia (`app/escala/services/leitor_celula.py:370-380`): chama `_carimbar_cobertura_parcial` (`:385`), consulta ausência atual (`:404`) e recalcula ambos (`:409-420`, trecho modificado no diff). Portanto fábrica/lote/c2 consultam a ata persistida e retroativo consulta uma composição ata+cadastro atual; uma ata com `sem_periodo` e cadastro já corrigido pode dar respostas distintas. A divergência é demonstrável pelo código; sua ocorrência nos quatro casos raspados é [nao verificado].

2. **Nota — v4 alcança todas as células de comp09 em diante, não apenas os dias parciais.** `REGRA_VERSOES` declara v4 em `'2026-09'` (`app/ponto/services/cartorio.py:111`); `versao_para` usa competência empresarial e devolve a maior versão aplicável (`:220-239`). O hash incorpora `regra:4` (`:443-444`) nos dois ramos (`:1113`, `:1267`), vencendo o pulo por impressão (`:1114`, `:1272-1278`). O comando entrega todas as células passadas da empresa, sem filtro de situação (`app/ponto/management/commands/processar_cartorio.py:49-65`). Assim o código inteiro atual pode mudar atas, vereditos/status, protestos, chamados, perguntas e fios além dos dias parciais. A configuração identifica execução automática pelo cron `--apply` (`app/config/crons.py:597`); disparo imediato pelo deploy é [nao verificado].

3. **Nota — outros leitores fora do diff observam o novo `julgada_em`.** A escrita persistente está em `carimbar`, que só poupa o relógio quando nenhum insumo/lavra mudou (`app/ponto/portas/celula.py:787-807`). O novo hash faz o relógio avançar mesmo se o veredito final permanecer igual. Fora do diff, ele compõe o marco de invalidação das perguntas (`app/chamados/veredito_lavrado.py:151-155`, `:187-190`) e reabre janelas históricas do censo de fase 12x36 (`app/escala/services/fase_12x36.py:184-193`). Mudança de ata/veredito também emite `EventoCelula` (`app/ponto/portas/celula.py:622-642`), cujo consumidor retrata chamados e encerra perguntas órfãs (`app/chamados/reconciliador.py:1306-1315`).

4. **Nota — `n_ok` muda também para dispensas integrais e justificativas antigas.** `_n_dispensadas` conta qualquer lâmpada com `dispensada`, não somente ausência parcial (`app/escala/services/leitor_celula.py:63-68`); `n_ok_lavrado` subtrai essa quantidade (`:91`) e `n_casadas_lavrado` delega (`:94-97`). A mesma alteração afeta `_teve_fato` e a exclusão por admissão/demissão (`:343-351`): no dia da demissão, uma ata apenas com dispensas deixa de sustentar “teve fato” e pode passar a `fora_vinculo`. O supra lê os números fornecidos (`app/ponto/supra_juiz.py:229-230`); o consumidor externo `bordas_realizado._teve_batida_na_ata` passa a observar `n_ok` menor (`app/ponto/services/bordas_realizado.py:49-51`), embora seu laço filtre dias de trabalho (`:74`).

5. **Nota — as escritas persistentes continuam pelas portas identificadas.** `CelulaDia.ata` nasce pela lavra (`app/ponto/portas/celula.py:709-782`); a chave `precedencia` tem operação própria que atravessa a substituição da ata (`:94`, `:732-734`, `:811-823`). `julgada_em` permanece em `carimbar` (`:800-807`). O cartório usa `lavrar_veredito(save=False)` e salva junto pelo carimbo (`app/ponto/services/cartorio.py:1713-1734`). A atribuição de ata em `escala/utils.py:1904` é a uma cópia em memória explicitamente criada em `:1903`, não uma escrita persistente nova. Não identifiquei nova aquisição de trava nesses trechos; deadlock por outro percurso é [nao verificado].

6. **Nota — o percurso de rejulgamento inspecionado não grava FM/TXT; a comprovação em produção falta.** A porta de célula salva a célula e insere outbox (`app/ponto/portas/celula.py:634-642`); o consumidor inspecionado atua em chamados/perguntas (`app/chamados/reconciliador.py:1306-1315`). A guarda de setembro está declarada (`app/ponto/services/fechamento.py:36-43`) e retira do universo de escrita colaboradores de empresas exportadas (`:542-559`). O teste verifica linha inteira de FM e todas as exportações, com premissas não vazias (`app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:118-131`), além da mudança sem `--forcar` (`:146-151`). Execução desses testes e hashes reais antes/depois são [nao verificado].

7. **Nota — há caminho de cadastro para sair da lista; execução dos pré-requisitos não foi comprovada.** `_aplicar_periodo` escreve os campos pela porta `decidir` (`app/ponto/services/ausencia.py:875-917`); existem rota (`app/ponto/urls.py:66`), formulário de edição (`app/templates/ponto/partials/painel_ausencia.html:297-302`) e migration `app/ponto/migrations/0076_ausencia_periodo_parcial.py:1`. Migrate aplicado, smoke do Ronald, DIFF de frota e Pauta DP efetivamente produzidos são [nao verificado].

Inventário complementar: `REGRA_VERSOES` tem declaração em `cartorio.py:111`, validação em `:209-217`, único leitor produtivo `versao_para` em `:233-239`; seu chamador produtivo é `_regra_do_dia` em `:1059-1062`. `ata_do_dia` é usada nos ramos sem adesão (`:1125`), projeção para o julgamento (`:1299-1300`) e lavra final (`:1711`); `dispensar_por_ausencia` lê o carimbo da grade (`:719-720`) e escreve apenas a ata transitória (`:725-731`). `julgada_antes` é fotografada antes do primeiro corpo (`:888`), enviada à fábrica (`:938`), mas não à segunda passada (`:914`) nem à releitura (`:979`); estas podem ler o carimbo do próprio ato em `parcial_so_lista` (`:1618`). A consequência detalhada depende dos emissores e está na exploração paralela.

## 01/11 20:41 · sessao a863d9288f3c · - · - · origem=Ronald (tmux)

1. **Bloqueia — o legacy descarta os dois carimbos da parcial.** O overlay escreve `cobertura_parcial` e `periodo_parcial`, mas `_montar_grade_prevista_periodo_legacy` reconstrói o dia sem essas chaves. As células `dispensada` sobrevivem, criando combinação inconsistente: dia parcial sem a identificação da parcial. `dispensar_por_ausencia` passa a dispensar também os marcos de fora do período; `ata_do_dia` não grava os carimbos; `parcial_so_lista` retorna falso. Fontes: `app/escala/utils.py:1247`, `app/escala/utils.py:1251`, `app/escala/utils.py:1060`, `app/ponto/services/cartorio.py:719`, `app/ponto/services/cartorio.py:726`, `app/ponto/services/cartorio.py:689`, `app/ponto/services/cartorio.py:368`. Reproduzido executando somente em memória a função real extraída por AST, com carregadores e subgrade dublados: entrada com as duas chaves; saída sem ambas. Execução Django completa: [nao verificado].

2. **Fila — a bateria v6 não cobre essa perda.** O helper sempre cria ausência com `minutos=0`; os cenários cobrem férias, aviso integral, falta e folga com vínculo vivo, verificando principalmente rótulo e ata. Executar os mesmos casos nas duas flags não testa parcial nem preservação dos carimbos. Fontes: `app/escala/tests/test_cobparc6_ausencia_sem_vinculo.py:55`, `app/escala/tests/test_cobparc6_ausencia_sem_vinculo.py:63`, `app/escala/tests/test_cobparc6_ausencia_sem_vinculo.py:83`, `app/escala/tests/test_cobparc6_ausencia_sem_vinculo.py:94`, `app/escala/tests/test_cobparc6_ausencia_sem_vinculo.py:99`.

3. **Nota — “uma vez por grade” vale para o montador por turno, não para o legacy inteiro.** Chamadas de produção de `_marcar_ausencias`: single em `app/escala/utils.py:807`; segmento legacy sem vínculo em `app/escala/utils.py:1029`; por turno em `app/escala/utils.py:1865`. O legacy chama o single por segmento com vínculo, portanto pode executar o overlay várias vezes na janela: `app/escala/utils.py:1021`, `app/escala/utils.py:1037`. Chamadas diretas em testes: `app/escala/tests/test_grade_segue_a_precedencia.py:40`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:60`.

4. **Nota — censo dos outros dois montadores.** `montar_grade_prevista_periodo_por_turno` é chamado pelo dispatcher (`app/escala/utils.py:2104`) e por `_score_geometria` do detector de par relâmpago (`app/ponto/management/commands/detectar_par_relampago.py:136`). Teste direto: `app/escala/tests/test_grade_por_turno_override_ponta.py:61`; testes indiretos pelo callable retornado por `_novo_or_skip`: `app/escala/tests/test_grade_por_turno.py:92`, `app/escala/tests/test_grade_por_turno.py:141`, `app/escala/tests/test_grade_por_turno.py:321`. `_montar_grade_prevista_periodo_legacy` tem o dispatcher como chamada de produção (`app/escala/utils.py:2106`); testes chegam pela flag falsa, incluindo `app/escala/tests/test_cobparc6_ausencia_sem_vinculo.py:99`, `app/escala/tests/test_grade_folga_vale_em_toda_competencia.py:103`, `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:102`.

5. **Nota — impacto da ausência sem vínculo.** O builder por turno monta inicialmente folga com previsto zero; a ausência agora troca o rótulo e não cria jornada: `app/escala/utils.py:1818`, `app/escala/utils.py:1237`. O juiz de previsto continua retornando zero sem vínculo: `app/escala/utils.py:2277`. `EC.eh_dia_trabalho` lê `cel.trabalha`; `EC.marcos_do_dia` lê DNA ou template, sem consultar o novo rótulo da ata: `app/escala/models.py:1071`, `app/escala/models.py:1099`. O motor continua consultando esses insumos: `app/ponto/motor_calculo_v2.py:1241`, `app/ponto/motor_calculo_v2.py:2278`. A agregação de dinheiro passa a incluir o dia `ausencia`, porém com previsto zero gera abono zero e realizado limitado a zero; pode acrescentar chaves de valor zero aos mapas: `app/ponto/services/dia_pago.py:385`, `app/ponto/services/dia_pago.py:391`, `app/ponto/services/dia_pago.py:399`. Impacto monetário real na frota: [nao verificado].

6. **Nota — admissão/demissão continuam distinguindo leitores.** O overlay consulta cobertura da pessoa sem guarda por admissão, demissão ou situação: `app/escala/utils.py:1183`, `app/escala/utils.py:1205`. Já `grade_da_celula` devolve `fora_vinculo` após a demissão e antes da admissão sem fato, antes de ler o tipo da ata: `app/escala/services/leitor_celula.py:337`, `app/escala/services/leitor_celula.py:347`, `app/escala/services/leitor_celula.py:350`. Portanto a v6 não transforma universalmente esses dias em ausência em todos os leitores. Uma regra que obrigasse rejeitar a ausência já aprovada nesses casos não foi demonstrada pelo material: [nao verificado].

7. **Nota — ordem dos overlays e exceção da falta estão preservadas.** A2 declara ausência acima de folga (`app/ponto/precedencia.py:11`, `app/ponto/precedencia.py:18`); o builder aplica folga, depois ausência, depois isenção (`app/escala/utils.py:1861`, `app/escala/utils.py:1865`, `app/escala/utils.py:1867`). A condição impede falta aprovada de substituir folga: `app/escala/utils.py:1227`. A isenção usa a mesma função para quaisquer tipos, preservando o rótulo; células `None` de dia sem vínculo não são creditadas: `app/escala/utils.py:833`, `app/escala/utils.py:848`.

8. **Nota — rejeitadas e múltiplas ausências.** O overlay usa `para='folha'`, que aceita somente aprovadas: `app/escala/utils.py:1205`, `app/ponto/turnos.py:2514`. A cobertura do período desempata por maior `(data, pk)`, substituindo a ausência anterior; não combina períodos de várias ausências: `app/ponto/turnos.py:2756`, `app/ponto/turnos.py:2768`. A consulta de um dia usa a mesma ordenação: `app/ponto/turnos.py:2557`.

9. **Fila — leitor da célula recalcula os carimbos em vez de ler a ata.** `grade_da_celula` constrói o dia sem copiar `ata.cobertura_parcial`/`ata.periodo_parcial` e chama `_carimbar_cobertura_parcial`, que consulta a ausência atual e calcula novamente: `app/escala/services/leitor_celula.py:370`, `app/escala/services/leitor_celula.py:385`, `app/escala/services/leitor_celula.py:404`, `app/escala/services/leitor_celula.py:409`, `app/escala/services/leitor_celula.py:420`. Entre mudança do cadastro e nova lavra, esse caminho pode combinar lâmpadas antigas com cobertura/período atuais, enquanto fábrica/lote/c2 consultam os carimbos gravados. Ocorrência em produção: [nao verificado].

10. **Nota — escala_auto não consome diretamente esses montadores.** As propostas consultam `turnos_do_colab` e `EC.eh_dia_trabalho`; o executor altera o vínculo por sua porta: `app/escala/services/escala_auto.py:87`, `app/escala/services/escala_auto.py:144`, `app/escala/services/escala_auto_executor.py:103`. A v6 não modifica diretamente esses insumos; repercussões indiretas pela geometria/ata exigiriam execução adicional: [nao verificado].

## 01/11 18:57 · sessao cc9e17bf9b48 · O261-E-F3 · censo · origem=Code

**O contador do F0 existe, mas mede um recorte: carimbadas, vivas pelo C6, em disputa aberta e fora das vias de identidade. Não equivale ao estoque inteiro de perguntas vivas sob destino terminal.** (`app/chamados/services/vigia_fio.py:743`, `app/chamados/services/vigia_fio.py:800`, `app/chamados/services/relavra_via.py:170`.)

Censo estático da cópia. Correspondência efetiva com o HASH/HEAD informado, quantidades atuais e ocorrência de cada classe em produção: **[nao verificado]**. As referências abaixo são relativas à raiz da cópia.

**1. Predicados e juízes**

| Pergunta | Autoridade existente e representações |
|---|---|
| “Esta PerguntaDisputa está viva?” | Juiz exato: `pergunta_viva(p)`, cuja regra está em `_pergunta_viva_calc`. Registrado como C6 na família chamado e novamente como autoridade da pergunta de tela TT8. (`app/chamados/juizes.py:799`, `app/chamados/juizes.py:1032`, `app/core/juizes.py:112`, `app/core/juizes.py:266`, `app/core/juizes.py:710`.) |
| Outras formas de consultar vida | `.viva` delega diretamente ao mesmo juiz; `perguntas_vivas_de` filtra por ele; `fio_vivo` reduz uma coleção com `any(pergunta_viva(...))`. São consumidores/composição da autoridade. (`app/chamados/models.py:2728`, `app/chamados/juizes.py:1128`, `app/chamados/juizes.py:1133`.) |
| Pré-filtro SQL | `q_vivas()` tem contrato de **continência**, não igualdade: pode trazer mortas, e quem precisa da resposta exata consulta C6. (`app/chamados/models.py:2697`, `app/chamados/models.py:2722`.) |
| “Este chamado está terminal?” | Pertencimento a `motor.TERMINAIS`: `resolvido`, `fechado`, `cancelado`, `superado`. `ENCERRADOS` contém somente `resolvido` e `fechado`; `VIVOS` contém `aberto`, `em_analise`, `registrado`. O registro C1 aponta para `motor.VIVOS`; a tabela documental explicita a família de conjuntos. (`app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:30`, `app/core/juizes.py:261`, `app/docs/JUIZES.md:51`.) |
| Outra expressão de chamado morto | `historico._morto` usa `status_local not in VIVOS`. Para os estados declarados no catálogo, corresponde ao complemento terminal; não há nesse helper outro cadastro de estados. (`app/chamados/services/historico.py:178`, `app/chamados/catalogo/motor.py:19`.) |

O lookup `JUIZES.md` não nomeia C6 no bloco de chamado: registra os conjuntos do motor; sua linha “FIO / DISPUTA” aponta para `_disputa_do_chamado`. O registro vivo nomeia explicitamente `pergunta_viva`; o próprio documento determina que o vivo prevalece. (`app/docs/JUIZES.md:10`, `app/docs/JUIZES.md:14`, `app/docs/JUIZES.md:40`, `app/docs/JUIZES.md:51`, `app/core/juizes.py:266`.)

Duas distinções afetam o censo:

- **Carimbo não implica morte.** C6 considera validação, tranca, resposta, identidade, marco, célula, lei e testemunha do carimbo, nessa precedência. Resposta ainda por validar pode manter viva até uma pergunta carimbada por identidade. (`app/chamados/juizes.py:1032`, `app/chamados/juizes.py:1047`, `app/chamados/juizes.py:1071`, `app/chamados/juizes.py:1107`.)
- O fallback de materialização lê **`ENCERRADOS`**, não `TERMINAIS`: cancelado/superado não satisfazem aquela testemunha. (`app/chamados/juizes.py:706`, `app/chamados/juizes.py:720`.)

**2. Contadores e vigias**

| Medidor | Unidade, universo e exclusões |
|---|---|
| `materializada_viva` / `carimbos_sem_lastro_por_dono` | Conta perguntas com carimbo, disputa **aberta**, via fora das quatro identidades e C6 viva. Divide em `aberta`/`exportada` pela competência do dia; sem data entra em `aberta`. Exclui disputas fechadas e identidades. **É exatamente o contador citado no F0 item 3**, agora na linha 800. (`app/chamados/services/vigia_fio.py:743`, `app/chamados/services/vigia_fio.py:760`, `app/chamados/services/vigia_fio.py:768`, `app/chamados/services/vigia_fio.py:800`, `app/chamados/services/relavra_via.py:166`.) |
| `censo_alcance` → `fio_morto_alcance` | Conta **dias retidos sem porta**, alimentados por `fora_da_folha`, nas empresas e competências enumeradas. Separa hoje/futuro e donos fora do fio. A causa `c` identifica respondida sob âncora terminal; o detalhe `pergunta_viva_sob_chamado_terminal` cobre outro ramo. Não é contagem de perguntas do acervo inteiro. (`app/chamados/services/vigia_fio.py:534`, `app/chamados/services/vigia_fio.py:545`, `app/chamados/services/vigia_fio.py:604`, `app/chamados/services/vigia_fio.py:630`, `app/chamados/services/vigia_fio.py:642`.) |
| `viva_em_disputa_fechada` | `q_vivas` + disputa fechada + C6 viva; retorna perguntas e disputas distintas. Não exige destino terminal nem carimbo; dono declarado é admin, sem reabertura automática pelo vigia. (`app/chamados/services/vigia_fio.py:692`, `app/chamados/services/vigia_fio.py:703`.) |
| Auditor `I2` / `I2p` | Busca **não carimbadas** com destino terminal. Retira do I2 as respondidas em disputa aberta e as publica em I2p. I2 conta chamados distintos; não consulta C6. (`app/chamados/management/commands/auditar_invariantes_chamados.py:586`.) |
| Auditor `I8b` / `I8bc` / `I8bcx` / `I8bx` | Exige destino e data presentes, C6 viva e **resolvedora inválida pelo turno**. Carimbadas vão para I8bc/I8bcx; não carimbadas sem célula e exportadas para I8bx; demais para I8b. Portanto I8bc não mede toda carimbada viva. A separação histórica de exportadas usa `_competencias_exportadas`, não `competencia_do_dia_entregue`. (`app/chamados/management/commands/auditar_invariantes_chamados.py:885`, `app/chamados/management/commands/auditar_invariantes_chamados.py:890`, `app/chamados/management/commands/auditar_invariantes_chamados.py:904`, `app/chamados/management/commands/auditar_invariantes_chamados.py:923`.) |
| Auditor `I21` / `I21a` | Respondida, texto diferente de `''`, não validada, não carimbada, destino **e pai** fora de VIVOS. Separa ausência aprovada em I21a. Não consulta C6. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1568`, `app/chamados/management/commands/auditar_invariantes_chamados.py:156`.) |
| Auditor `I6` / pisos | Disputa fechada, recusa vazia, sem carimbo **ou** sem materialização de fato; exclui C6 viva que tenha porta para o colaborador. Divide violação/piso por via e pelo corte de `legado_sem_lastro`. Pode incluir carimbadas, mas não é um contador geral C6-viva. (`app/chamados/management/commands/auditar_invariantes_chamados.py:454`, `app/chamados/management/commands/auditar_invariantes_chamados.py:524`, `app/chamados/management/commands/auditar_invariantes_chamados.py:534`.) |
| Contraprovas `I6c` e pisos associados | Inspecionam o piso do I6 contra célula e movimento do dia, separando ausência de célula/vigência, exportação, isenção e ausência lavrada. Não aplicam simplesmente “carimbada e C6 viva”. (`app/chamados/management/commands/auditar_invariantes_chamados.py:305`, `app/chamados/management/commands/auditar_invariantes_chamados.py:350`, `app/chamados/management/commands/auditar_invariantes_chamados.py:375`, `app/chamados/management/commands/auditar_invariantes_chamados.py:399`, `app/chamados/management/commands/auditar_invariantes_chamados.py:776`.) |

Há medidores vizinhos com outras perguntas: `I8` mede órfãs de destino NULL; `F2` do auditor exclui carimbadas; `respondida_sem_resposta` mede apenas o par texto/carimbo de resposta a partir de um marco temporal; `vigia_carimbo` parte de perguntas hora/data_hora **validadas**. Não substituem o contador do F0. (`app/chamados/management/commands/auditar_invariantes_chamados.py:853`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1760`, `app/chamados/services/vigia_fio.py:728`, `app/chamados/services/vigia_carimbo.py:97`.)

**3. Portas e escritores que podem alcançar o estoque**

Primeiro, “status da pergunta” não é uma coluna: `estado` é propriedade derivada de validação, C6, resposta e materialização. (`app/chamados/models.py:2467`.)

Para compactar os campos nas linhas seguintes:

- **Carimbo:** `materializada_em`, `materializada_por`, `via_resolucao`. (`app/chamados/models.py:2527`.)
- **Cache do julgamento:** `veredito_lavrado`, `julgado_em`, `janela_expira_em`. (`app/chamados/services/fio.py:845`.)

| Porta | Chamadores existentes | Escrita, trilha e guardas |
|---|---|---|
| `fio.reapontar_resolvedora` | Gate F1, reemissor/reatador, migração de resolvedoras, declaração textual e LIMBO. (`app/chamados/models.py:418`, `app/chamados/reconciliador.py:903`, `app/chamados/reconciliador.py:1539`, `app/chamados/services/fio.py:941`, `app/chamados/services/declaracao_texto.py:317`, `app/chamados/services/limbo_folgas.py:242`.) | Grava destino, data somente se faltava e cache. Consulta E4-c e a guarda F1 de costura, com destino relido/travado e C6 no pós-destino. Não escreve carimbo F2. A porta não cria evento próprio; gate/migração deixam `pergunta_reatada`/`resolvedoras_migradas`. (`app/chamados/services/fio.py:758`, `app/chamados/services/fio.py:831`, `app/chamados/models.py:421`, `app/chamados/services/fio.py:947`.) |
| `PerguntaDisputa.abrir`, reutilizando identidade | Emissão e declaração textual. (`app/chamados/services/disputa_emissao.py:389`, `app/chamados/services/declaracao_texto.py:322`.) | Também atualiza pergunta existente: célula, destino e cache. Usa a mesma guarda F1; recusa deixa `costura_recusada` idempotente. É uma exceção concreta à alegação documental de exclusividade de `reapontar_resolvedora`. (`app/chamados/models.py:2407`, `app/chamados/models.py:2423`, `app/chamados/models.py:2434`, `app/chamados/models.py:2447`.) |
| `reatar_pergunta_orfa` e `reemitir_chamado_do_dia` | Reconciliação por colaborador **e geração da pergunta**. A frase “HOJE SO O CRON” ficou desatualizada. (`app/chamados/reconciliador.py:1424`, `app/chamados/reconciliador.py:1436`, `app/chamados/services/disputa_emissao.py:404`.) | Derivam candidato, tentam histórico/c1/c2; costuram pela porta F1. c2 abre/deduplica chamado, exige retorno vivo e reaponta. Recusas de costura ficam no logger; não há evento próprio de reata nesses trechos. (`app/chamados/reconciliador.py:1454`, `app/chamados/reconciliador.py:1483`, `app/chamados/reconciliador.py:875`, `app/chamados/reconciliador.py:1539`.) |
| `lavrar_sem_turno_aberto` | Reatador. (`app/chamados/reconciliador.py:1517`.) | Só motivos c1, sem texto/carimbo/turno aberto e C6 morta. Carimba `sem_turno_aberto` pela F2 e tenta fechar disputa pela porta. (`app/chamados/reconciliador.py:332`, `app/chamados/reconciliador.py:362`, `app/chamados/reconciliador.py:405`.) |
| `retratar_perguntas_absolvidas_por_celula` | Reconciliação por colaborador e cartório. (`app/chamados/reconciliador.py:1348`, `app/ponto/services/cartorio.py:818`.) | Sem carimbo/validação, célula julgada que não acusa; inclui disputas fechadas. Pula C6 viva e dia trancado. Carimba `celula_absolveu` pela F2; fecha disputa aberta se C7 permite. Não cria evento específico nesse corpo; fechamento passa por `d.fechar`. (`app/chamados/reconciliador.py:581`, `app/chamados/reconciliador.py:608`, `app/chamados/reconciliador.py:618`, `app/chamados/reconciliador.py:630`.) |
| S128 `resolver_perguntas_por_chamado_resolvido` | Signal de chamado, retratação, histórico único e serviço de becos. (`app/chamados/signals.py:75`, `app/chamados/reconciliador.py:536`, `app/chamados/reconciliador.py:1545`, `app/chamados/services/becos.py:49`.) | Primeiro ramo atua no destino; segundo em disputa sem credor vivo. Pula viva pelo C6/tranca e usa F2, podendo preencher resposta/validação com texto e relavrar cache. Segundo ramo deixa `fecho_sem_credor`; pode reabrir pai com resposta pendente. (`app/chamados/services/disputa_emissao.py:1532`, `app/chamados/services/disputa_emissao.py:1544`, `app/chamados/services/disputa_emissao.py:1565`, `app/chamados/services/disputa_emissao.py:1583`, `app/chamados/services/disputa_emissao.py:1607`.) |
| `desfazer_carimbo_sem_lastro` | Fábrica por motivo e julgamento da célula. (`app/chamados/services/fabrica_da_celula.py:121`, `app/chamados/services/fabrica_da_celula.py:148`.) | Relê linha travada, exige elegibilidade e C6 viva; limpa os três campos pela porta F2 `descarimbar`. Deixa `carimbo_desfeito_por_lastro` com antes/depois. Pode reabrir disputa; julgamento chama com `reabrir_fechada=False`. Não altera resposta/validação. (`app/chamados/services/relavra_via.py:159`, `app/chamados/services/relavra_via.py:204`, `app/chamados/services/relavra_via.py:217`, `app/chamados/services/relavra_via.py:229`, `app/chamados/services/relavra_via.py:243`.) |
| `relavrar` | Comando `relavrar_via_i6`; wrapper `relavrar_dia`. (`app/chamados/management/commands/relavrar_via_i6.py:102`, `app/chamados/services/relavra_via.py:140`.) | Só vias derivadas; muda `via_resolucao` e cache, com evento antes/depois. Sem testemunha nova não toca. Não escreve `materializada_em`, portanto não passa por `carimbar` F2. (`app/chamados/services/relavra_via.py:95`, `app/chamados/services/relavra_via.py:116`, `app/chamados/services/relavra_via.py:123`.) |
| `encerrar_fantasma` | Consumidor de evento da célula → `encerrar_orfas_pela_lampada`. (`app/chamados/reconciliador.py:1301`, `app/chamados/reconciliador.py:1093`.) | Exige universo específico, sem resposta, marco próprio aceso e dia encerrado; carimba `fantasma` pela F2. Evento `pergunta_fantasma_pela_lampada` somente se escreveu. (`app/chamados/reconciliador.py:993`, `app/chamados/reconciliador.py:1031`, `app/chamados/models.py:2645`, `app/chamados/reconciliador.py:1089`.) |
| `encerrar_por_fato_no_espelho` | Comando homônimo de encerramento. (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:68`.) | Troca somente a via para `fato_materializado`; preserva carimbo, logo não chama `carimbar` F2. O comando deixa evento. (`app/chamados/models.py:2629`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:65`.) |
| `ChamadoColaborador.reabrir` / `ressuscitar_perguntas` | Cartório e desarquivamento; reabrir chama ressuscitação. (`app/ponto/services/cartorio.py:1452`, `app/chamados/views_arquivar.py:86`, `app/chamados/models.py:1380`.) | Reabre chamado pelo núcleo ou redireciona para irmão vivo, migrando resolvedoras. Ressuscitação seleciona materializadas sem texto, julga pós-estado, limpa carimbo pela F2 em lote e outros carimbos de resposta/validação; deixa `perguntas_ressuscitadas` e pode reabrir disputa. (`app/chamados/models.py:1359`, `app/chamados/models.py:1420`, `app/chamados/models.py:1483`.) |
| `ChamadoColaborador.encerrar_por_competencia_trancada` | Serviço `competencia_trancada.encerrar`. (`app/chamados/services/competencia_trancada.py:177`.) | Passa pelo gate e núcleo F1; grava estado, via, resolução temporal e urgência. Serviço deixa nota/evento. **Não carimba pergunta:** C7 mata por leitura; disputas fecham pela porta. (`app/chamados/models.py:1543`, `app/chamados/models.py:222`, `app/chamados/services/competencia_trancada.py:122`, `app/chamados/services/competencia_trancada.py:178`, `app/chamados/services/competencia_trancada.py:189`.) |

Também existem portas dependentes de resposta/decisão:

- **Responder/reabrir correção:** core web/API grava texto e `respondida_em`, desfaz validação anterior e carimbo elegível; deixa eventos de resposta/validação. Correção preserva histórico em `RespostaAnterior` e limpa resposta/validação, podendo descarimbar pela F2. (`app/chamados/services/disputa_emissao.py:1247`, `app/chamados/services/disputa_emissao.py:1358`, `app/chamados/views.py:1283`, `app/api/views.py:2695`.)
- **Validar/materializar:** validação grava `validada_em/por` e chama a unidade de materialização. `_materializar_uma` planta dentro de transação; recusa F2 provoca rollback do plantio. Fechamento de disputa também chama `materializar_pendentes_aprovadas`, com carimbo F2. (`app/chamados/services/validacao.py:127`, `app/chamados/services/validacao.py:396`, `app/chamados/services/materializacao.py:1109`, `app/chamados/services/materializacao.py:1160`, `app/chamados/signals.py:125`, `app/chamados/services/materializacao.py:1307`.)
- **Decisão administrativa/identidade:** `fechar_por_admin` é chamado pelo veredito do dia e aceitação de folga; usa `carimbar_identidade`. Essa exceção F2 grava sem consultar C6 e admite continuar viva por resposta pendente. (`app/chamados/models.py:2484`, `app/ponto/services/veredito_celula.py:160`, `app/chamados/services/validacao.py:263`, `app/chamados/models.py:2588`.)

A antiga porta **da pergunta** `fechar_por_competencia_trancada` foi removida; F2 não aceita criar essa via. O acervo antigo continua sendo lido. (`app/chamados/models.py:2501`, `app/chamados/models.py:2546`.)

**4. Management commands existentes**

| Comando | Portas usadas e exclusões |
|---|---|
| `reabrir_orfas_enterradas --apply` | Fantasma carimbada, não validada, com data, marco False e C6 viva. Janela fixa de carimbo; exclui luz desconhecida, pai que recusa e reabertura que ressuscitaria irmãs. Exportadas vão à Pauta DP. Usa reabertura de fantasma/disputa/pai. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:35`, `:67`, `:92`, `:104`, `:145`.) |
| `encerrar_por_competencia_trancada --apply` | Usa serviço e portas de chamado/disputa. Exige tranca real; exclui sem/outra competência, módulos fora do universo, resgatados, perguntas vivas remanescentes e disputa com pai vivo fora. Não escreve perguntas. (`app/chamados/management/commands/encerrar_por_competencia_trancada.py:13`; `app/chamados/services/competencia_trancada.py:82`, `:122`, `:132`, `:174`.) |
| `encerrar_perguntas_com_fato --apply` | Só carimbadas não validadas, via vazia/NULL/`legado_sem_lastro` e marco próprio True. Usa `encerrar_por_fato_no_espelho`; fecha disputa somente sem fio vivo. (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:23`, `:31`, `:65`, `:80`.) |
| `relavrar_via_i6 --apply` | Parte da violação I6; exclui vias não derivadas, IDs de `--excluir`, ausência de nova testemunha e via inalterada. Usa `relavrar`. (`app/chamados/management/commands/relavrar_via_i6.py:44`, `:58`, `:76`, `:102`.) |
| `dedup_perguntas_fato --apply` | Grupos duplicados por colaborador/motivo/dia, anteriores a `--ate`; exclui canônica e já duplicatas. Usa `retratar_duplicata`. (`app/chamados/management/commands/dedup_perguntas_fato.py:40`, `:71`, `:86`.) |
| `materializar_passivo_validado --apply` | Universo validado hora/data_hora; exclui `com_registro`, `plantio_fora_do_slot`, `slot_nao_derivavel` e recortes solicitados. Usa `passivo_validado.reparar` → `_materializar_uma`. (`app/chamados/management/commands/materializar_passivo_validado.py:49`; `app/chamados/services/passivo_validado.py:52`, `:324`, `:514`.) |
| `curar_perguntas_em_limbo --apply` | Conjunto fixo de IDs; recusa se conjunto atual divergir. Duas perguntas dependem de nota da resposta; demais passam por `reabrir_para_correcao`. (`app/chamados/management/commands/curar_perguntas_em_limbo.py:21`, `:46`, `:55`, `:67`.) |
| `disparar_perguntas_competencia --apply` | Lote de emissão, que também pode desfazer carimbo pela fábrica. Seleciona células/marcos e chamados vivos sem pergunta viva; exclui desligados e fatos já servidos/decididos. Não percorre todo estoque carimbado: o primeiro universo deduplica perguntas existentes. Dry executa a unidade dentro de transação e força rollback. (`app/chamados/management/commands/disparar_perguntas_competencia.py:45`, `:76`, `:124`, `:155`, `:229`, `:276`, `:310`.) |

`reconciliar_chamados` já escreve, mas não oferece dry/apply: visita colaborador com chamado vivo ou órfã reatável. `reconciliar_perguntas_orfas` é vigia. Não foi identificado executor genérico F3 para todo o conjunto C6-viva/destino-terminal **[nao verificado]**. (`app/chamados/management/commands/reconciliar_chamados.py:22`, `app/ponto/management/commands/reconciliar_perguntas_orfas.py:1`.)

“Dry” não significa necessariamente ausência de escrita em arquivo: `materializar_passivo_validado` cria diretório/JSON de resultado; o auditor também pode escrever HTML. (`app/chamados/management/commands/materializar_passivo_validado.py:85`, `:114`; `app/chamados/managemen

[truncado em 20000 de 26646 caracteres]

## 01/11 18:54 · sessao 0d1e007159c1 · - · - · origem=Ronald (tmux)

Censo Q3/Q6, somente código, sem executar comandos de aplicação.

**Premissa:** não há campo `status` em PerguntaDisputa; `estado` é property que deriva validação, C6, resposta e materialização (`app/chamados/models.py:2467-2482`). Portanto “mudar status da pergunta” significa mudar esses insumos, não escrever coluna de status (`app/chamados/models.py:2475-2482`).

| Porta / alcance | Chamadores no HEAD | Escrita / trilha / F1-F2 |
|---|---|---|
| `fio.reapontar_resolvedora(p,ch,data_evento)` | Gate F1 (`models.py:418`); reatador e reemissor (`reconciliador.py:903,1539,1551`); migração (`fio.py:941`); declaração textual (`declaracao_texto.py:317`); agrupamento LIMBO (`limbo_folgas.py:242`). Todos sob `app/chamados/`. | Escreve `chamado_destino`, opcionalmente `data_evento` somente se faltava, e `veredito_lavrado/julgado_em/janela_expira_em` (`app/chamados/services/fio.py:831-848`). Valida E4-c e F1 `_costura_nao_mata_o_fio`; esta trava destino e julga C6 com destino pós costura, barrando viva de competência aberta em terminal (`fio.py:758-780,838-844`). A porta não cria evento de auditoria próprio; migração e gate criam, conforme linhas seguintes (`fio.py:845-849,947-953`; `models.py:421-425`). Não escreve carimbo F2 (`fio.py:847`). |
| `PerguntaDisputa.abrir` ao reutilizar identidade existente | Emissor chama em `app/chamados/services/disputa_emissao.py:389-397`; declaração textual em `app/chamados/services/declaracao_texto.py:322-323`. | Além do nascimento, reaponta pergunta existente e atualiza `celula` (`app/chamados/models.py:2407-2427`). Mesma guarda F1; recusa deixa `costura_recusada` idempotente; relavra S2 no mesmo save (`models.py:2423-2450`). Exceção necessária à alegação de escritor único em `fio`: existe esta outra porta de identidade (`models.py:2374-2381,2426`). |
| `reatar_pergunta_orfa` / reemissão c2 | Cron/por batida via `reconciliar_chamados_colab` (`app/chamados/reconciliador.py:1348-1349,1424`), **e nascimento** (`app/chamados/services/disputa_emissao.py:404-409`): docstring “HOJE SO O CRON” em `reconciliador.py:1436` está desatualizada. | Deriva candidato; se não há, tenta histórico único ou c1/c2 (`reconciliador.py:1454-1457,1483-1521`). C6 viva em competência aberta sai do histórico terminal (`1491-1493`). Costura pela porta F1 (`1539-1554`). c2 usa `ChamadoColaborador.abrir`, exige retorno vivo, e reaponta (`reconciliador.py:875-905`). Não cria evento próprio de reata; recusas de costura são logadas (`1540-1542,1552-1554`). |
| `lavrar_sem_turno_aberto` c1 | Reatador (`app/chamados/reconciliador.py:1517`). | Só `MOTIVOS_C1=('orfao_14h','saida_sem_entrada')`, sem texto, sem carimbo, sem turno aberto do dia e C6 morta (`reconciliador.py:332,362-399`). Carimba `sem_turno_aberto` pela F2 e fecha disputa pela porta se `pode_fechar()` (`405-413`). |
| `retratar_perguntas_absolvidas_por_celula` | `reconciliar_chamados_colab` (`app/chamados/reconciliador.py:1348`), chamado por signal de batida (`app/ponto/signals.py:215`) e management cron (`app/chamados/management/commands/reconciliar_chamados.py:52`). | Universo: sem materialização/validação, célula julgada que não acusa; inclui disputa fechada (`reconciliador.py:581-596`; ausência de filtro de disputa em `571-573`). Pula C6 viva e dia trancado; carimba `celula_absolveu` pela F2; fecha disputa aberta quitada (`608-636`). Não há evento de auditoria específico nesta função; fechamento fica a cargo de `d.fechar()` (`618-636`). |
| `resolver_perguntas_por_chamado_resolvido` S128 | Post-save de chamado encerrado (`app/chamados/signals.py:75-94`); retratação (`reconciliador.py:536`); histórico único (`1545`); becos (`services/becos.py:49`); comando fantasmas (`app/ponto/management/commands/reconciliar_fantasmas.py:280`). | Primeiro ramo: destino igual ao chamado, disputa aberta, sem carimbo; pula C6 viva, tranca e resposta sem testemunha; F2 `carimbar(...resposta='validar',lavrar_veredito=True)` (`app/chamados/services/disputa_emissao.py:1532-1568`). Pode reabrir pai com resposta pendente (`1583-1593`). Segundo ramo: disputas abertas sem pai/destino vivo, carimba só C6 morta sem materialização/validação; deixa `fecho_sem_credor` (`1607-1645`). O chamado já passa pela F1 em sua própria porta; S128 não fecha viva por destino terminal (`1544-1549`). |
| `desfazer_carimbo_sem_lastro` | Fábrica por motivo (`app/chamados/services/fabrica_da_celula.py:121-123`) e julgamento da célula (`148`). | Relê pergunta travada, exige carimbo não identidade e C6 viva; limpa **três** campos via F2 `descarimbar`; evento `ACAO_DESFAZER` com antes/depois; opcionalmente reabre disputa pela porta (`app/chamados/services/relavra_via.py:217-245`). Julgamento chama com `reabrir_fechada=False`: exclui disputa fechada (`fabrica_da_celula.py:141-148`; `relavra_via.py:223`). Não relavra cache S2: assimetria declarada, leitores guardam carimbo (`relavra_via.py:40-49`). |
| `relavrar` | Chamador direto encontrado: comando `relavrar_via_i6` (`app/chamados/management/commands/relavrar_via_i6.py:102`); `relavrar_dia` é wrapper (`app/chamados/services/relavra_via.py:140-155`). Outro chamador de produção de `relavrar_dia`: `[nao verificado]`. | Só vias derivadas `declaracao/completude_turno`; sem destino e sem declaração pode devolver None e não tocar (`relavra_via.py:77,95-101,116-120`). Troca `via_resolucao` e relavra S2; evento `relavra_via_i6` por pergunta (`123-136`). Não escreve `materializada_em`, portanto não passa por `carimbar` F2 (`125-126`). |
| `encerrar_fantasma` | Consumidor de EventoCelula chama `encerrar_orfas_pela_lampada` (`app/chamados/reconciliador.py:1301`), que chama porta (`1093`). | Universo aberto, sem materialização/validação, dia presente, motivos orfão/saída (`993-1000`). Exige sem resposta, próprio marco aceso, C6 viva e dia encerrado; hoje/teto adia (`1031-1045`). F2 `carimbar(fantasma)` (`app/chamados/models.py:2645-2648`); evento `pergunta_fantasma_pela_lampada` só se escreveu (`reconciliador.py:1089-1101`). |
| `encerrar_por_fato_no_espelho` | Comando `encerrar_perguntas_com_fato` (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:68`). | Só muda via para `fato_materializado`, idempotente; não muda carimbo e não passa por `carimbar` F2 (`app/chamados/models.py:2629-2638`). |
| `ChamadoColaborador.reabrir` / `ressuscitar_perguntas` | Reabrir chama ressuscitação (`app/chamados/models.py:1380-1389`); cartório (`app/ponto/services/cartorio.py:1452`); view arquivar (`app/chamados/views_arquivar.py:86`). | Reabrir pode redirecionar a irmão vivo e migrar resolvedoras/duplicatas com `resolvedoras_migradas` e `reabertura_redirecionada` (`models.py:1359-1373`; `fio.py:927-953`). Ressuscitação só materializadas sem texto, pula marco aceso e C6 morta no pós estado; limpa materialização/autor/via, validação/autor/resposta por F2 lote; `perguntas_ressuscitadas`; reabre disputa fechada (`models.py:1420-1442,1483-1516`). |
| `encerrar_por_competencia_trancada` | Serviço `competencia_trancada.encerrar` (`app/chamados/services/competencia_trancada.py:177`). | **Não carimba pergunta:** C7 a mata por leitura; `perguntas` de ação vazio por construção (`competencia_trancada.py:122-131,171-173`). Fecha chamado via F1 e núcleo, deixando estado/via/urgência/data; nota e evento com antes/depois (`app/chamados/models.py:1543-1553,222-246,275-279`; serviço `178-187`). Disputa fecha pela porta se juiz permite (`competencia_trancada.py:189-198`). Método antigo **da pergunta** `fechar_por_competencia_trancada` foi removido, zero chamador de produção declarado; F2 não aceita criar essa via (`models.py:2501-2506,2546-2547`). |

**Outras portas humanas que impedem dizer “nenhuma porta” em sentido absoluto:**

- Responder pelo core (web/API/declarador): grava texto/`respondida_em`, desfaz validação anterior, descarimba vias sem lastro, deixa `resp_pergunta_gravada` e `validacao_desfeita_por_resposta`, e tenta ressurreição por resposta (`app/chamados/services/disputa_emissao.py:1247-1291`; chamadores `app/chamados/views.py:1283`, `app/api/views.py:2695`, `app/chamados/services/declaracao_texto.py:365`). `reabrir_por_resposta` só descarimba `VIAS_SEM_LASTRO` (`app/chamados/models.py:2674-2676`).
- Validar: escritor `validada_em/por` (`app/chamados/services/validacao.py:127-133`), chama materializador, desfaz validação de hora recusada antes de eventual fechamento (`396-430`). Materializador seleciona respondida+validada sem materialização (`app/chamados/services/materializacao.py:1258-1266`); `_materializar_uma` planta dentro de atomic e F2 recusada levanta rollback da batida (`1109-1115,1160-1173`).
- Admin decide dia: `fechar_por_admin` carimba identidade (`app/chamados/models.py:2484-2498`), chamado pelo veredito/declarar dia (`app/ponto/services/veredito_celula.py:160-186`), aceitação de folga (`app/chamados/services/validacao.py:263-271,290-299`) e fechamento por veredito (`app/chamados/models.py:1839`). Identidade usa exceção F2: pode continuar viva por resposta pendente de validação, porque esse degrau precede a via (`models.py:2588-2595`).
- Reabrir correção limpa texto/resposta/validação; preserva versão em `RespostaAnterior`; se desfaz absorção também descarimba via F2 lote (`app/chamados/services/disputa_emissao.py:1358-1378`). Rejeição de ausência chama essa operação e reabre disputa (`app/chamados/services/materializacao.py:622-625`).
- `materializar_pendentes_aprovadas`, gatilhada por fechamento da disputa (`app/chamados/signals.py:125-140`), exige aprovação pelo juiz do lastro e carimba via `juiz_aprovou` com F2 (`app/chamados/services/materializacao.py:1307-1314`).

**Q6: contraexemplos verificáveis de alcance, sem inventar linha de produção:**

1. Pergunta viva com **destino terminal não NULL** fica fora do reatador automático de órfãs, porque exige NULL; se materializada também fica fora pelo carimbo (`app/chamados/reconciliador.py:1401-1406`). O gate F1 só faz plano na travessia vivo→terminal, logo não revisita espontaneamente terminal antigo (`app/chamados/models.py:398-400`). Isso demonstra falta de varredura genérica nessa porta, **não ausência de porta parametrizada**, pois `reapontar_resolvedora` aceita pergunta existente (`app/chamados/services/fio.py:784-849`).
2. Carimbada por **identidade**, ainda C6 viva por resposta não validada: é estado explicitamente permitido; desfazimento de carimbo exclui identidade (`app/chamados/models.py:2591-2595`; `app/chamados/services/relavra_via.py:179`). Limpar essa identidade automaticamente por ser viva não é regra de `desfazer_carimbo_sem_lastro`; há validação humana existente (`app/chamados/services/validacao.py:359-396`).
3. Reemissão c2 não cobre motivos fora do grupo DIA, dia ausente, desligado, vetos de célula/ausência/isenção/piso/protesto/régua; cada exclusão está no corpo e no contrato (`app/chamados/reconciliador.py:764-788,801-855`). Ligar automaticamente esses casos à emissão exigiria alterar o escopo da porta; ocorrência no estoque fornecido `[nao verificado]`.
4. Marco aceso com turno aberto sem escala/sem previsto não satisfaz encerramento por fantasma; segue viva e volta ao julgamento em evento seguinte, explicitamente (`app/chamados/reconciliador.py:1046-1052`). Transformar essa falta de prova em encerramento não é regra existente (`1048-1051`).
5. Pergunta sem dia / ambiguidade sem candidato único retorna False no reatador (`app/chamados/reconciliador.py:1465-1468,1521-1522`); escolha de destino/dia sem esses insumos não é feita por essa porta (`app/chamados/services/disputa_emissao.py:219-237`).

O dado recebido não lista linhas/pks nem atributos do estoque; portanto nenhum contraexemplo acima pode ser atribuído a uma linha real ou ter quantidade confirmada `[nao verificado]`.

## 01/11 18:54 · sessao d3a74d0e3ea4 · - · - · origem=Ronald (tmux)

Para Q2, o contador F0 existe: `materializada_viva()['aberta']` é literalmente o tamanho do balde `aberta` devolvido por `carimbos_sem_lastro_por_dono`, não um contador geral de toda pergunta carimbada e viva (`app/chamados/services/vigia_fio.py:800-804`).

| Medidor | Universo e exclusões |
|---|---|
| `materializada_viva` / `carimbos_sem_lastro_por_dono` | Carimbo presente, disputa aberta e via fora de `VIAS_DE_IDENTIDADE`; depois exige C6 `pergunta_viva`. Particiona aberta/exportada pela competência do dia; sem data conta em aberta. Exclui disputa fechada e vias de identidade (`app/chamados/services/vigia_fio.py:743-772`; `app/chamados/services/relavra_via.py:166-182`). |
| `censo_alcance` → `fio_morto_alcance`, causa `c` e detalhe `pergunta_viva_sob_chamado_terminal` | Conta **dias retidos sem porta**, não perguntas de todo acervo; universo de empresas visíveis e competências enumeradas por `competencias_a_lavrar`, alimentado por `fora_da_folha`. Retira hoje/futuro e baldes de dono fora do fio antes de classificar causas; causa c requer pergunta respondida sob âncora terminal, detalhe e cobre outras vivas sob terminal (`app/chamados/services/vigia_fio.py:534-552`, `:604-654`). |
| `viva_em_disputa_fechada` | Pré-filtro `q_vivas`, disputa fechada, C6 viva; conta perguntas e disputas; sem filtro de destino terminal nem de carimbo (`app/chamados/services/vigia_fio.py:692-713`). |
| Auditor `I2` / `I2p` | Pergunta **não carimbada** com destino terminal; exclui do I2 respondida em disputa aberta, publicada em I2p. Conta chamados, não todas as perguntas; não consulta C6 (`app/chamados/management/commands/auditar_invariantes_chamados.py:586-598`). |
| Auditor `I8` | C6 viva, destino NULL, motivo atendível, colaborador ativo; retira piso de 2+ candidatos sem entrada no contexto (`app/chamados/management/commands/auditar_invariantes_chamados.py:853-865`, `:82-95`). |
| Auditor `I8b`, `I8bc`, `I8bcx`, `I8bx` | C6 viva com destino e data, cuja `resolvedora_valida` é falsa. Carimbadas vão I8bc/I8bcx; não carimbadas sem célula exportadas vão I8bx; restante I8b. I8bc não é o contador geral de carimbada viva (`app/chamados/management/commands/auditar_invariantes_chamados.py:890-947`). |
| Auditor `I21` / `I21a` | Respondida, não validada, não carimbada, texto diferente de `''`, destino **e pai** fora de VIVOS; retira ausência aprovada em I21a. Não consulta C6 (`app/chamados/management/commands/auditar_invariantes_chamados.py:1568-1582`, `:156-163`). |
| Auditor `I22` | Chamado vivo com pelo menos uma resolvedora, todas inválidas; exclui cinco IDs históricos explícitos. Não mede estoque viva-terminal (`app/chamados/management/commands/auditar_invariantes_chamados.py:1040-1061`). |
| Auditor `F2` | C6 viva, disputa fechada, carimbo NULL, validação NULL e colaborador ativo; exclui carimbadas (`app/chamados/management/commands/auditar_invariantes_chamados.py:1760-1765`). |
| Auditor `S6` | Amostra aleatória de até 300 carimbadas; compara veredito lavrado com `_pergunta_materializada_de_fato`, excluindo stale e exceções. Não consulta vitalidade C6 (`app/chamados/management/commands/auditar_invariantes_chamados.py:1153-1173`). |
| `vigia_carimbo.censo` | Perguntas de hora/data_hora validadas; classifica por materialização de fato e slot, não por C6 nem destino terminal (`app/chamados/services/vigia_carimbo.py:97-124`). |

`vigiar_fio_morto` publica `materializada_viva` aberta com esperado zero e exportada separada, sem alarme; escreve carimbo apenas no modo frota (`app/chamados/management/commands/vigiar_fio_morto.py:189-191`, `:205-224`). A cerca correta aberta/entregue é `competencia_do_dia_entregue`: janela da empresa + `folha.export.competencia_entregue`; sem dia retorna None (`app/chamados/services/vigia_fio.py:775-797`).

Para Q4, existem comandos específicos de estoque; não foi identificado um executor genérico que percorra todo o estoque C6-viva/destino-terminal e aplique reata/descarimbo [nao verificado].

| Comando | Portas e exclusões |
|---|---|
| `reabrir_orfas_enterradas --apply` | Usa `reabrir_fantasma`, `reabrir_sistema` e `pai.reabrir`; só fantasma carimbada, não validada, com data, lâmpada própria False e C6 viva. Exclui luz None, carimbo fora da janela fixa, pai que recusa ou ressuscitaria irmãs; exportada recebe Pauta DP (`app/chamados/management/commands/reabrir_orfas_enterradas.py:35-37`, `:67-118`, `:145-167`). |
| `encerrar_por_competencia_trancada --apply` | Delega a `competencia_trancada.encerrar`; usa `ch.encerrar_por_competencia_trancada()` e `d.fechar()`. Exige tranca real; não escreve perguntas desde C7. Exclui chamados sem competência, doutra competência, módulos fora da tranca, resgatados, perguntas vivas fora e disputa com pai vivo fora (`app/chamados/management/commands/encerrar_por_competencia_trancada.py:13-29`; `app/chamados/services/competencia_trancada.py:82-105`, `:122-151`, `:174-198`). |
| `encerrar_perguntas_com_fato --apply` | Usa `encerrar_por_fato_no_espelho` e depois `DisputaSupervisao.fechar`; só carimbadas não validadas, via vazia/NULL/legado_sem_lastro e marco próprio True. Preserva demais vias; fecha disputa somente sem fio vivo (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:23-43`, `:65-94`). |
| `dedup_perguntas_fato --apply` | Usa `fio.retratar_duplicata`; só grupos duplicados colab/motivo/dia, anteriores a `--ate` (default 24/08); exclui canônica e já duplicatas. Recortes opcionais colab/limite (`app/chamados/management/commands/dedup_perguntas_fato.py:40-61`, `:71-92`). |
| `materializar_passivo_validado --apply` | Usa `passivo_validado.reparar` → `_materializar_uma`; universo hora/data_hora validada do `vigia_carimbo`. Exclui `com_registro`, `plantio_fora_do_slot`, `slot_nao_derivavel` e recortes de colaborador/competência; demais recusas continuam nas guardas da unidade (`app/chamados/management/commands/materializar_passivo_validado.py:49-83`, `:110-111`; `app/chamados/services/passivo_validado.py:52-60`, `:324-350`, `:514-524`; `app/chamados/services/vigia_carimbo.py:97-116`). |
| `curar_perguntas_em_limbo --apply` | Conjunto histórico fixo de 21 IDs; recusa se conjunto atual diverge. Duas usam `carimbar_resposta_pela_trilha` somente com nota; demais `reabrir_para_correcao` (`app/chamados/management/commands/curar_perguntas_em_limbo.py:21-23`, `:46-69`). |

`reconciliar_chamados` já escreve, mas **não oferece dry/apply**: visita colaborador com chamado vivo ou pergunta órfã reatável, chama `reconciliar_chamados_colab`, rejunta célula e drena eventos (`app/chamados/management/commands/reconciliar_chamados.py:22-64`). `reconciliar_perguntas_orfas` é vigia sem escrita, com `--alarme` (`app/ponto/management/commands/reconciliar_perguntas_orfas.py:1-15`, `:24-36`).

`auditar_invariantes_chamados` não tem `--corrigir`/`--apply`; seus argumentos são `--html` e `--amostra`; `--html` grava arquivo (`app/chamados/management/commands/auditar_invariantes_chamados.py:545-550`, `:1838-1849`). Mesmo o dry de `materializar_passivo_validado` cria diretório e JSON de resultado, portanto não é execução compatível com a restrição desta tarefa (`app/chamados/management/commands/materializar_passivo_validado.py:85`, `:114-127`).

## 01/11 18:54 · sessao 86f8555eafdd · - · - · origem=Ronald (tmux)

Q5 — leitores e efeito sobre folha/export, somente código:

- **“Status da pergunta” é `PerguntaDisputa.estado`, uma `@property`:** traduz `validada_em`, resultado de `pergunta_viva`, `respondida_em` e `materializada_em` em `VALIDADA/RESOLVIDA/PENDENTE/RESPONDIDA`; alterar apenas esse rótulo não é uma escrita de coluna de estado. A property `viva` delega ao C6; `q_vivas` é pré-filtro generoso, não resposta exata. (`app/chamados/models.py:2467-2482`, `2727-2738`.)

| Leitor | Campos/efeito |
|---|---|
| `pergunta_viva` / `_pergunta_viva_calc` | `validada_em`, tranca, resposta, via, carimbo e célula determinam vida; carimbo não basta para declarar morta. Destino terminal participa da testemunha de materialização. (`app/chamados/juizes.py:1032-1118`, `720-721`.) |
| `chamado_que_lista` | Ancora no `chamado_destino_id`; sem destino usa `disputa.chamado_id`. Reapontar muda o card ao qual pergunta é atribuída. (`app/chamados/services/fio.py:97-117`.) |
| `perguntas_do_app` e API | Seleção `q_vivas` + C6; API separa `p.estado == RESPONDIDA`, expõe `estado`, `aguarda_admin` e `chamado_destino_id`. (`app/chamados/services/pergunta_no_app.py:39-44`; `app/api/views.py:2603-2634`.) |
| `montar_chamados_fio` | Admin carrega perguntas vivas de disputas fechadas; ramo `so_janela` não acrescenta essas disputas; agrupa pela âncora canônica. (`app/chamados/services/fio.py:209-240`.) |
| `raiox.creditos_perguntas` | Credita destino vivo; senão, pai da disputa. Filtra `q_vivas`, exclui carimbadas com `veredito_lavrado=VL_SIM`, separa respondidas/não respondidas. O cache pode estar velho até ciclo do cron. (`app/chamados/services/raiox.py:49-72`, `110-149`.) |
| `qs_na_fila` | Lê `status_local` via `qs_vivos`, restringe a `COBRANCA` e exclui silêncio vigente; terminal não entra nessa fila. (`app/chamados/services/painel.py:153-155`, `158-188`.) |
| `alimentar_portas` / `porta_alcancavel_do_dia` | Cruza fila, verbos, C6, perguntas do app e âncora. Porta pela pergunta exige viva, no app, resposta não vazia quando respondida e âncora na fila. Logo reapontamento/status do chamado podem alterar existência da porta e fio/grupo retornados. (`app/chamados/juizes.py:1160-1196`, `1226-1272`.) |
| Cartório | Impressão inclui `pk:status_local` e quantidade de chamados vivos com fio. Julgamento deriva `_com_fio` por C6 e o passa ao suprajuiz; pode lavrar `cobrado`. Mudança real depende do novo julgamento. (`app/ponto/services/cartorio.py:356-410`, `1167-1186`, `1243-1246`, `1281-1282`.) |
| Signal do chamado | Criação/mudança de `status_local` chama `cartorio.julgar_celula` pelo dia forte do fato; save sem mudar status retorna. (`app/chamados/signals.py:155-180`.) |
| `furos_sem_cobranca` | Lê status do chamado e de `superado_por`; classifica vivo/coberto/decidido e exclui dia trancado. (`app/ponto/services/furos_sem_cobranca.py:66-86`.) |
| `competencia_do_chamado`, fechamento da disputa | Competência considera dias das perguntas que C6 diz vivas; `disputa_viva_do_chamado` lê existência de perguntas e `fio_vivo`. (`app/chamados/juizes.py:1909-1941`, `1955-1968`.) |

**Caminho de dinheiro:** `classificar_export` chama `motivos_retencao_espelho`; se há motivos marca pessoa `fora/furo_espelho`. O wrapper delega a `motivos_retencao_celula`. Esse retentor consulta célula por dia e lê `veredito`, `protestos`, `trabalha`, cobertura cadastral e isenções; não decide diretamente pelo carimbo, destino ou estado da pergunta. (`app/folha/export.py:942-947`, `1243-1249`, `1174-1238`.)

Assim, mudar âncora/carimbo pode mudar **fio, grupo e ação indicada** sem mudar quem entra; pode mudar quem entra quando os efeitos chegarem à célula e alterarem os motivos de retenção. O cartório lê vida/estado dos chamados e fio para julgar, e o export lê a célula resultante; o efeito efetivo neste estoque é `[nao verificado]`. (`app/chamados/juizes.py:1261-1272`; `app/ponto/services/cartorio.py:1167-1186`, `1243-1246`, `1281-1282`; `app/folha/export.py:1215-1238`.)

`fora_da_folha` toma **quem entra** de `classificar_export`; usa `motivos_retencao_celula(com_dias=True)` para dias retidos e `porta_alcancavel_do_dia` para o fio/grupo. `o_que_segura` usa as mesmas autoridades. Não existe juízo próprio de retenção por pergunta nessas ferramentas. (`app/folha/fora_da_folha.py:55-74`, `91-95`, `120-155`, `180-208`.)

A montagem do TXT consome `classificar_export`, constrói `entra_set`, restringe fechamento aos estados `aberto/aprovado` e só monta linhas dos IDs admitidos. Portanto comparar só valores não prova preservação de pertencimento ao TXT. (`app/folha/export.py:668`, `695-708`.)

**Aberta × entregue:** `competencia_do_dia_entregue` compõe `janela_atual(dia, empresa)` com `competencia_entregue`; sem dia retorna `None`. Entregue significa TXT vigente não invalidado, não apenas existência histórica de exportação. (`app/chamados/services/vigia_fio.py:775-797`; `app/folha/export.py:545-575`.)

- Na **aberta**, o alcance novo C pode operar; a cerca também exclui sem data e trancada. (`app/chamados/reconciliador.py:683-716`.)
- Na **entregue**, a cerca preserva o caminho anterior e deixa fora apenas o alcance novo C; o texto explicita O44 sem fronteira. Isso não é proibição global dos leitores ou de todas as portas. (`app/chamados/reconciliador.py:686-699`.)
- `classificar_export`/retentor continuam lendo estado atual; o juiz `txt_vigente_x_gravado` monta o atual e compara seu hash ao TXT vigente. Logo “entregue” não garante que a leitura atual permaneça igual ao documento entregue. Divergência real neste estoque é `[nao verificado]`. (`app/folha/export.py:635-660`, `746-769`, `942-955`.)
- Não confundir com `competencia_fechada_para_celula`: outro juiz combina TXT entregue **ou holerite publicado**. (`app/folha/export.py:578-609`.)

## 01/11 18:26 · sessao 2e2239d89df6 · O261-E-F2 · verificador · origem=Code

1. **Tratamento dos itens da triagem_v8**

Nas referências abaixo, **T** é [test_o261e_f2_ramo_e_i20.py](/home/ronald/codex_ws/5c2bb8a4370998765c54bbb1926895b27a5ec1bd/app/chamados/tests/test_o261e_f2_ramo_e_i20.py) e **A** é [auditar_invariantes_chamados.py](/home/ronald/codex_ws/5c2bb8a4370998765c54bbb1926895b27a5ec1bd/app/chamados/management/commands/auditar_invariantes_chamados.py); os demais caminhos são relativos a `app/`.

- **Item 1 — seis procedentes da v7:** a resolução individual dos seis é **[nao verificado]**: o anexo não enumera os seis achados. O código atual permite confirmar restauração do juiz, leitura após o mock, comparação exata da amostra I20 e preservação da célula sem reescrita posterior. (`T:216`, `T:221`, `T:245`, `T:168`, `T:170`.)

- **Item 2a — procedente:** o novo cenário distingue as guardas. Sem resposta, a pergunta pode estar viva antes do carimbo e morta depois dele pelo BUG 117; com texto, a porta preenche `respondida_em` e o degrau da resposta decide antes desse BUG. (`chamados/juizes.py:1047`, `chamados/juizes.py:1077`, `chamados/juizes.py:1083`, `chamados/models.py:2556`.) A execução histórica de `mordida_v9_F.log` é **[nao verificado]** nesta revisão.

- **Item 2b — sustentado:** o meio-estado é uma entrada adversarial explícita: o teste escreve somente `resposta_colab`; o escritor normal grava texto e `respondida_em` juntos. (`T:156`, `T:158`, `chamados/services/disputa_emissao.py:1247`, `chamados/services/disputa_emissao.py:1248`.)

- **Item 3 — sustentado com limite de proveniência:** resposta, veredito e lâmpadas vêm da fixture; `_fechar` relê e verifica preservação de veredito, via e lâmpadas. Não verifica preservação de todos os campos da célula ou de toda a ata. (`T:158`, `T:160`, `T:169`, `T:170`.) O cálculo dessas lâmpadas pelo cartório real é **[nao verificado]**.

- **Item 4a — correção parcial:** retirar o incremento do contador por causa viola a soma; retirar o detalhe do ramo viola a asserção específica. Entretanto, a distribuição entre `alcance_a..e` continua sem verificação individual — há mutante sobrevivente descrito no item 4 abaixo. (`chamados/services/vigia_fio.py:651`, `chamados/services/vigia_fio.py:653`, `T:196`, `T:197`.)

- **Item 4b — descarte sustentado para estes cinco testes:** uma restauração em memória inoperante não altera o banco: a porta retorna `False` antes do `save`, o fecho executa `continue` e os leitores seguintes recarregam os dados. (`chamados/models.py:2569`, `chamados/models.py:2576`, `chamados/services/disputa_emissao.py:1638`, `chamados/services/disputa_emissao.py:1655`, `T:183`.)

- **Item 5 — ressalva:** o código contém proteção explícita de relógio e mock, mas a afirmação incondicional de ausência de dependência de ordem deve considerar a possível colisão do PK gerado com a cicatriz fixa. (`T:109`, `T:213`, `T:216`, `T:265`, `A:1098`.)

2. **Caminho exato do C6 no teste da guarda sozinha** [nao verificado]

**Antes do fecho:**

- A pergunta usa `saida_sem_entrada`, resposta vazia, DNA `hi=08:00` e lâmpada E das 08:00 acesa. Esse motivo aponta para E/hi. (`T:236`, `T:147`, `T:61`, `chamados/juizes.py:845`.)
- O degrau da resposta não entra; o BUG 117 também não entra, pois `materializada_em` está nulo. O veredito `furo` então determina **viva**. (`chamados/juizes.py:1047`, `chamados/juizes.py:1077`, `chamados/juizes.py:1083`, `escala/models.py:1735`.)
- A guarda do fecho recebe esse resultado e pula a pergunta. (`chamados/services/disputa_emissao.py:1626`.)

**Retirando somente essa guarda:**

- `carimbar` aplica o carimbo em memória; como o texto está vazio, não preenche `respondida_em`. (`chamados/models.py:2555`, `chamados/models.py:2556`.)
- O C6 é chamado novamente. Agora há carimbo e o fato E/hi está provado pela lâmpada: o BUG 117 retorna **morta antes de chegar ao veredito `furo`**, permitindo o `save`. (`chamados/models.py:2565`, `chamados/juizes.py:887`, `chamados/juizes.py:893`, `chamados/juizes.py:1077`, `chamados/models.py:2576`.)

**O docstring corresponde ao código.** Com texto, a porta preencheria `respondida_em`; o degrau da resposta viria primeiro, e o dia acusador impediria a concordância, mantendo a pergunta viva e provocando recusa. (`T:229`, `chamados/models.py:2558`, `chamados/juizes.py:1059`, `chamados/juizes.py:1029`, `chamados/models.py:2569`.)

3. **Quem escreve e quem lê os campos assertados**

| Campos/resultado | Quem escreve | Quem lê/asserta |
|---|---|---|
| `veredito`, `veredito_via`, `ata.lampadas` | O teste fornece os valores à porta; a porta persiste a célula. (`T:160`, `ponto/portas/celula.py:768`, `ponto/portas/celula.py:770`.) | `_fechar` relê o banco e compara os três; C6 lê veredito e lâmpadas. (`T:169`, `T:170`, `chamados/juizes.py:1081`, `chamados/juizes.py:887`.) |
| `resposta_colab`, `respondida_em` | Fixture injeta o texto; controles escrevem ambos explicitamente. A porta pode preencher `respondida_em` durante o carimbo e restaurá-lo na recusa. (`T:158`, `T:255`, `chamados/models.py:2558`, `chamados/models.py:2583`.) | C6, I20 e a asserção de preservação do caso 2. (`chamados/juizes.py:1047`, `A:1102`, `T:264`.) |
| `materializada_em`, `via_resolucao` | A pergunta nasce sem carimbo e com via vazia; a porta escreve os campos e desfaz na recusa. (`chamados/models.py:2298`, `chamados/models.py:2529`, `chamados/models.py:2531`, `chamados/models.py:2582`.) | Os três testes do ramo verificam ausência/presença do carimbo e vivacidade; o caso 2 verifica carimbo nulo. (`T:184`, `T:185`, `T:186`, `T:218`, `T:239`, `T:264`.) |
| `pergunta_viva(...)` | C6 calcula; no MORDE o teste substitui deliberadamente sua resposta durante o fecho. (`chamados/juizes.py:816`, `T:213`.) | Premissas e selo usam o juiz real; o MORDE verifica sua restauração antes das testemunhas. (`T:202`, `T:237`, `T:260`, `T:216`, `T:220`.) |
| `alcance_universo`, `fio_morto_alcance` | Censo calcula o universo a partir de `fora_da_folha` e incrementa os dias sem porta. (`chamados/services/vigia_fio.py:550`, `chamados/services/vigia_fio.py:553`, `chamados/services/vigia_fio.py:650`.) | Selo do ramo e caso regularizado. (`T:194`, `T:268`.) |
| `alcance_dias`, incluindo célula, rota, causa e detalhe | `_listar` monta a linha produzida pelo censo. (`chamados/services/vigia_fio.py:589`, `chamados/services/vigia_fio.py:654`.) | Teste filtra pelo `cel.pk`, confere uma linha `sem_porta/e`, confere `sem_porta/c` no MORDE e ausência da linha no caso regularizado. (`T:176`, `T:191`, `T:192`, `T:224`, `T:267`.) |
| `alcance_a..e` | Censo inicializa e incrementa o contador da causa. (`chamados/services/vigia_fio.py:543`, `chamados/services/vigia_fio.py:651`.) | Teste lê somente a **soma** dos cinco. (`T:196`.) |
| `alcance_e_detalhe` | Censo incrementa a chave correspondente ao detalhe. (`chamados/services/vigia_fio.py:653`.) | Teste exige somente a chave do ramo igual a 1. (`T:197`.) |
| I20: `flag`, `total`, `amostra` | Auditor consulta os campos, calcula contagem/amostra e imprime a classificação. (`A:1102`, `A:564`, `A:565`, `A:1834`, `A:1835`.) | Teste captura, interpreta e compara a tupla exata nos dois testes I20. (`T:95`, `T:80`, `T:245`, `T:293`, `T:297`.) | [nao verificado]
| Exclusão por PK e unicidade da âncora | A cicatriz vem do auditor; o teste lê seu AST e acrescenta o PK somente à cópia mutante. (`A:1098`, `T:73`, `T:282`, `T:283`.) | Caso real exige PK fora da cicatriz; MORDE compara cópia e comando real. (`T:265`, `T:293`, `T:297`.) | [nao verificado]

**Há valores escritos pelo teste consumidos pelos leitores reais:** texto, veredito e lâmpadas. Sua origem está declarada; a lavra pela porta não demonstra que o cartório calculou esses valores a partir de batidas. (`T:156`, `T:158`, `T:160`.) Os resultados `alcance_*` e a saída I20 são obtidos das funções reais, sem preenchimento desses resultados pelo teste. (`T:175`, `T:270`, `T:297`.) [nao verificado]

4. **Mutantes de produção que ficam vivos**

**Sim, por análise estática; sobrevivência executada nesta revisão: [nao verificado].**

- **Contador na causa errada:** substituir, em `chamados/services/vigia_fio.py:651`, `d['alcance_' + causa] += 1` por `d['alcance_a'] += 1`. A soma permanece correta; linha e detalhe continuam usando a causa original. O teste não exige os valores individuais dos contadores. (`chamados/services/vigia_fio.py:653`, `chamados/services/vigia_fio.py:654`, `T:192`, `T:196`, `T:197`.) No MORDE, o helper já falha pela linha `c`, antes da soma. (`T:192`, `T:224`, `T:226`.)

- **Restauração em memória inoperante:** tornar `_desfazer_em_memoria` um no-op. A porta continua recusando sem salvar, e o fecho abandona a instância; estes testes observam dados recarregados. É precisamente a lacuna aceita na triagem 4b. (`chamados/models.py:2579`, `chamados/models.py:2569`, `chamados/services/disputa_emissao.py:1638`, `T:183`, `T:262`.)

- **Autor do carimbo perdido:** trocar a atribuição de `materializada_por_id` por `None`. Os selos verificam instante, via, vivacidade e par resposta/carimbo, mas não o autor; o C6 também não consulta esse autor. (`chamados/models.py:2530`, `T:184`, `T:218`, `T:264`, `chamados/juizes.py:788`, `chamados/juizes.py:1032`.)

5. **Relógio, mock e ordem**

- **Relógio:** a classe congela o tempo; o censo recebe `HOJE` explicitamente e o controle usa timestamp UTC literal. Não identifiquei relógio solto nesses caminhos por leitura. (`T:109`, `T:55`, `T:175`, `T:251`.)
- **Mock:** está limitado ao `with`, e a identidade da função real é assertada antes das leituras do selo e do vigia. Não há vazamento demonstrado no código. (`T:213`, `T:216`, `T:220`, `T:221`.)
- **Ordem:** cada teste cria seu cenário, usando `TestCase` e `setUpTestData`; não aparece dependência explícita de dados deixados por outro dos cinco. (`T:110`, `T:112`, `T:201`, `T:212`, `T:236`, `T:259`, `T:276`.) [nao verificado]
- **Risco condicional de sequência de PK:** o banco gera o PK, mas o teste exige que ele não coincida com IDs históricos fixos. Uma colisão faria o selo falhar por causa da sequência, não do comportamento pretendido; ocorrência efetiva **[nao verificado]**. (`T:154`, `T:265`, `A:1098`, `T:297`.) [nao verificado]

Não executei os testes: o quinto cria arquivo temporário, incompatível com a restrição de somente leitura. (`T:284`, `T:286`.) As contagens históricas de testes verdes/vermelhos dos anexos não foram reproduzidas: **[nao verificado]**. [nao verificado]

## 01/11 18:24 · sessao dae46ff611b1 · - · - · origem=Ronald (tmux)

- Relógio fixado pela classe em `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:109`; o vigia recebe `hoje=HOJE` explicitamente em `:175`, definido em `:55`; controles usam timestamp UTC literal em `:251`. Não encontrei relógio solto demonstrável nesses pontos; execução [nao verificado].
- O único patch fica no `with` em `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:213`; a restauração da função real é assertada em `:216`, antes dos leitores em `:220` e `:221`. Vazamento demonstrado: nenhum; execução [nao verificado].
- Há **risco condicional de dependência da sequência de PKs**: `_cenario` deixa o banco gerar `p.pk` em `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:154`, mas depois exige que ele não esteja na tupla histórica em `:265`. Essa tupla contém IDs fixos em `app/chamados/management/commands/auditar_invariantes_chamados.py:1098`. Outra sequência de execução pode produzir colisão; ocorrência real [nao verificado]. O quinto teste também exige que o comando real conte esse PK, em `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:297`.
- O teste declara e injeta o meio-estado em `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:156` e `:158`; o escritor normal grava o par texto/carimbo em `app/chamados/services/disputa_emissao.py:1247`. Portanto é entrada adversarial explícita, não resposta produzida pelo core.
- Veredito e lâmpadas vêm do teste em `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:56` e `:160`; a porta escreve os campos em `app/ponto/portas/celula.py:768`. `_fechar` chama produção, relê o banco e verifica preservação em `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:168`; ele não reescreve o resultado. Isso comprova preservação da premissa, sem comprovar cálculo real das lâmpadas.
- A classe usa `TestCase` e `setUpTestData` em `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:110`; cada um dos cinco testes cria seu cenário em `:201`, `:212`, `:236`, `:259` e `:276`. Dependência de dados deixados por outro dos cinco testes não aparece no código; comportamento do framework em execução [nao verificado].
- Não executei os testes: o quinto cria arquivo temporário em `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:284`, incompatível com a auditoria somente leitura. Resultados dos logs anexados [nao verificado].

## 01/11 18:24 · sessao 0c1e939852f1 · - · - · origem=Ronald (tmux)

Auditoria estática, sem executar testes ou mutantes; resultados empíricos dos logs citados: [nao verificado]. [nao verificado]

- **Triagem 2b/3 sustentada com ressalva de proveniência:** o próprio teste injeta `resposta_colab` sem `respondida_em`; os controles também são escritos pelo teste. São entradas declaradas, não resultados produzidos pelo fecho (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:154-160`, `:251-255`). O teste verifica que o meio-estado permanece e que o I20 real o observa (`:264`, `:270-271`). A escrita normal do par pelo “core” não foi localizada nesta subtarefa: [nao verificado].

- **Preservação da célula:** o teste fornece veredito e lâmpadas pela porta (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:160`); a produção escreve `ata`, `veredito`, `veredito_via` em `app/ponto/portas/celula.py:768-773`. `_fechar` consulta novamente o banco e compara exatamente esses insumos; não os reescreve (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:168-171`). São premissas preservadas, não prova de que o cartório calculou corretamente essas premissas (`:160`, `:170-171`).

- **Quem escreve/lê alcance:** `censo_alcance` inicializa os campos em `app/chamados/services/vigia_fio.py:539-543`; calcula `alcance_universo` de `fora_da_folha` em `:548-553`; escreve `alcance_dias` em `:589`, `fio_morto_alcance` em `:650`, `alcance_a..e` em `:651`, `alcance_e_detalhe` em `:652-653`. O teste chama o censo real e filtra pelo identificador da célula (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:175-176`); lê linha, totais, soma e detalhe em `:191-197`, e a causa `c` em `:224`. Nenhum desses resultados é preenchido pelo teste nesses pontos (`:175-197`).

- **Quem escreve/lê I20:** o auditor lê `respondida_em`, `resposta_colab` e `pk` no banco e exclui a cicatriz (`app/chamados/management/commands/auditar_invariantes_chamados.py:1098-1103`); `add` calcula total e amostra (`:559-566`); o auditor calcula flag e imprime (`:1833-1837`). O teste captura saída (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:95-106`), interpreta a linha (`:80-89`) e compara flag/total/amostra (`:245`, `:293`, `:297`). A mordida injeta o `pk` apenas na cópia textual do auditor, explicitamente identificada como mutante (`:280-291`).

- **Triagem 4a parcialmente suficiente:** remover o incremento inteiro em `vigia_fio.py:651` viola a soma exigida pelo teste em `test_o261e_f2_ramo_e_i20.py:196`; remover o detalhe do ramo viola `:197`. Entretanto, o selo não garante que cada contador corresponde à causa da linha (`:192`, `:196-197`). [nao verificado]

- **Mutante vivo concreto, por análise estática:** substituir `d['alcance_' + causa] += 1` por `d['alcance_a'] += 1` em `app/chamados/services/vigia_fio.py:651`. A soma permanece correta, a linha continua com causa `e` e o detalhe permanece correto (`:652-654`); o teste não compara nenhum bucket individual, apenas a soma (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:192-197`). No MORDE, o helper já lança pela linha `c` antes de chegar à soma (`:192`, `:224-226`). A sobrevivência executada deste mutante: [nao verificado].

- **Outro mutante vivo:** omitir em `vigia_fio.py:653` o detalhe `sem_pergunta_viva`, mantendo o detalhe do ramo. O teste exige apenas `alcance_e_detalhe[CAUSA_DO_RAMO] == 1`, sem somar detalhes ou conferir o detalhe do outro dia (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:193-197`). Sobrevivência executada: [nao verificado].

## 01/11 18:24 · sessao 853d16ba45bf · - · - · origem=Ronald (tmux)

- **Triagem 2a / pergunta 2: corresponde ao código.** O cenário cria pergunta `saida_sem_entrada`, sem texto, com DNA `hi=08:00` e lâmpada E acesa; esse motivo corresponde a E/hi. Antes do carimbo, o C6 pula o degrau da resposta, pula o BUG 117 por `materializada_em=None`, e retorna viva porque `furo` acusa. Referências: `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:61`, `:147`, `:236`; `app/chamados/juizes.py:845`, `:1047`, `:1077`, `:1081`; `app/escala/models.py:1735`.

- **Retirando apenas a guarda do fecho:** a porta aplica o carimbo em memória, mantém `respondida_em=None` porque o texto está vazio e chama novamente o C6. Agora o BUG 117 encontra carimbo + fato do marco provado e retorna morta antes de consultar o veredito do dia; a porta grava. Referências: `app/chamados/services/disputa_emissao.py:1626`, `:1638`; `app/chamados/models.py:2529`, `:2555`, `:2556`, `:2565`, `:2576`; `app/chamados/juizes.py:869`, `:887`, `:893`, `:1077`.

- **Com resposta:** a porta preenche `respondida_em` antes de perguntar ao C6. O degrau da resposta precede o BUG 117; dia `furo` impede que `_resposta_concorda_com_a_luz` retorne verdadeiro, conservando a pergunta viva e causando recusa da porta. Referências: `app/chamados/models.py:2556`; `app/chamados/juizes.py:1029`, `:1047`, `:1059`; `app/chamados/models.py:2569`. A execução histórica `mordida_v9_F.log` é **[nao verificado]**.

- **Triagem 4b: descarte sustentado para estes cinco testes.** Tornar `_desfazer_em_memoria` inoperante não grava os campos: a porta continua retornando `False` antes do `save`, o fecho executa `continue`, a avaliação final da disputa consulta novamente suas perguntas e os selos recarregam o banco. Referências: `app/chamados/models.py:2569`, `:2576`, `:2579`, `:1767`, `:1778`; `app/chamados/services/disputa_emissao.py:1638`, `:1655`; `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:169`, `:183`, `:217`, `:262`, `:278`. Sobrevivência desse mutante executada nesta revisão: **[nao verificado]**.

- **Outro mutante vivo dedutível:** substituir `self.materializada_por_id = autor_id` por `None` em `app/chamados/models.py:2530`. Os cinco testes não assertam o autor; suas verificações da pergunta cobrem carimbo temporal, via, vivacidade e o par resposta/carimbo, enquanto a chave e os degraus do C6 não consultam `materializada_por_id`. Referências: `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:184`, `:218`, `:264`; `app/chamados/juizes.py:788`, `:1032`. Sobrevivência por execução: **[nao verificado]**.

## 01/11 18:15 · sessao 3fb825a7930a · O261-E-F2 · verificador · origem=Code

Análise estática; resultados de execução e medições mencionadas nos logs: **[nao verificado]**. Não executei a suíte: o quarto teste cria e escreve arquivo temporário (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:259`, `:261`, `:262`).

Referências abreviadas, todas relativas à cópia:

- **T** = `app/chamados/tests/test_o261e_f2_ramo_e_i20.py`
- **E** = `app/chamados/services/disputa_emissao.py`
- **M** = `app/chamados/models.py`
- **J** = `app/chamados/juizes.py`
- **V** = `app/chamados/services/vigia_fio.py`
- **A** = `app/chamados/management/commands/auditar_invariantes_chamados.py`
- **P** = `app/ponto/portas/celula.py`

1. **Os seis procedentes foram tratados no código; a confirmação por execução permanece [nao verificado].**

   | Achado da triagem | Avaliação do delta |
   |---|---|
   | **1 — causa versus detalhe** | Resolvido: compara conjuntamente rota, causa e detalhe com `['sem_porta', 'e', CAUSA_DO_RAMO]`; a produção escreve exatamente nessa disposição (`T:183`; `V:589`). |
   | **2 — retorno não prova o ramo** | Resolvido: `_fechar` deixa de verificar o retorno; a prova passa pelo estado persistido da pergunta e pelo vigia (`T:159`, `T:175`, `T:176`, `T:193`). |
   | **3 — MORDE abortava antes do vigia** | Resolvido: os helpers são separados; o MORDE verifica explicitamente a causa `c` e depois exige que o selo do vigia rejeite esse resultado (`T:171`, `T:179`, `T:212`, `T:214`). |
   | **4 — `ok` inválido e célula sobrescrita depois** | Resolvido: usa `concorde` pela porta, cuja validação recusa vocabulário inválido; depois do fecho apenas relê e compara a célula, sem restaurá-la (`T:151`, `T:234`, `T:159`, `T:160`, `T:161`; `P:659`). |
   | **5 — falta de controles negativos do I20** | Resolvido no desenho: cria texto com instante e texto vazio sem instante; ambos coexistem com o caso positivo, cujo total esperado continua exatamente um (`T:226`, `T:230`, `T:244`, `T:220`). |
   | **6 — PK por substring** | Resolvido: interpreta a amostra com `literal_eval` e exige igualdade com `[p.pk]` (`T:79`, `T:220`). |

2. **Descartado 7: sustenta-se para estes cenários; “mutante equivalente” globalmente não se sustenta.** A guarda consulta a pergunta **antes** do carimbo; a porta aplica carimbo e eventualmente `respondida_em` antes de consultar novamente o C6. É o mesmo juiz com entradas diferentes (`E:1626`; `M:2555`, `M:2558`, `M:2565`).

   Há um contraexemplo estático: pergunta sem resposta, motivo `saida_sem_entrada`, célula `furo` por outro marco, mas E/hi acesa. Sem carimbo, o veredito acusador a mantém viva; com carimbo, a lâmpada própria a mata antes dessa decisão. Retirar somente a guarda permite então gravar o carimbo (`J:845`, `J:893`, `J:1077`, `J:1079`, `J:1083`, `J:1084`; `M:2576`). Execução desse exemplo e ocorrência em produção: **[nao verificado]**.

   **Descartado 8: sustenta-se como entrada adversarial do detector.** O teste declara e injeta o meio-estado; o escritor normal atribui texto e instante e salva ambos juntos; o I20 consulta precisamente texto preenchido com instante nulo (`T:147`, `T:149`; `E:1247`, `E:1248`, `E:1266`; `A:1102`, `A:1103`). Isso prova detecção/preservação da entrada injetada, sem provar sua origem em produção; “B=0 na frota” permanece **[nao verificado]**. [nao verificado]

3. **Origem das escritas e leituras.**

   Nos quatro cenários, o próprio teste escreve `resposta_colab='07:55'`; os instantes nulos e a via vazia vêm da criação com os defaults do modelo. O veredito e as lâmpadas também são entradas escolhidas pelo teste, gravadas pela porta real (`T:145`, `T:149`, `T:151`; `M:2212`, `M:2299`, `M:2756`; `P:768`, `P:770`, `P:771`, `P:775`). [nao verificado]

   A produção escreve carimbo/via em `_escrever_carimbo`, acrescenta `respondida_em` quando aplicável e só persiste após aceitação pelo C6. Na recusa, restaura a instância em memória (`M:2529`, `M:2531`, `M:2558`, `M:2569`, `M:2576`, `M:2582`, `M:2583`).

   | Teste | Campos da pergunta efetivamente verificados | Célula, censo e I20 | [nao verificado]
   |---|---|---|
   | **Ramo normal** | Relê o banco: `materializada_em=None`, via vazia e pergunta viva. Não compara diretamente texto nem `respondida_em` (`T:174`, `T:175`, `T:176`, `T:177`). | Confere preservação da célula e linhas/contadores do censo real; não roda I20 (`T:191`, `T:193`, `T:161`, `T:183`, `T:185`). |
   | **MORDE do ramo** | A produção sob mock escreve o carimbo; o teste relê e exige carimbo existente. O helper rejeita já na primeira asserção: sua comparação de via não é alcançada nessa chamada (`T:201`, `T:205`, `T:206`, `T:208`, `T:175`). | Confere célula preservada, linha real com causa `c` e rejeição pelo selo do vigia; não roda I20 (`T:202`, `T:161`, `T:209`, `T:212`, `T:214`). | [nao verificado]
   | **Caso 2 normal** | Relê e compara texto, `respondida_em` e `materializada_em`; não compara via (`T:237`, `T:239`). | Confere célula, ausência do dia no censo e contadores; o auditor real produz I20, exigido como `VIOL,1,[pk]` (`T:236`, `T:161`, `T:242`, `T:243`, `T:245`, `T:220`). |
   | **MORDE do I20** | Relê a pergunta, mas não compara diretamente aqueles quatro campos (`T:253`, `T:254`). | Confere célula; não chama o censo. O teste escreve a cópia alterada do auditor; essa cópia produz `OK,0,[]`, e o auditor original deve produzir `VIOL,1,[pk]` (`T:252`, `T:161`, `T:258`, `T:262`, `T:266`, `T:268`, `T:272`). | [nao verificado]

   As **linhas do censo** são construídas pela produção e apenas filtradas/lidas pelo teste. A **linha I20** vem da consulta, contagem/amostra e formatação do auditor, capturada em `StringIO` (`V:589`, `V:654`; `T:166`, `T:167`; `A:1102`, `A:564`, `A:565`, `A:1835`, `A:1837`; `T:86`, `T:97`). [nao verificado]

   Portanto, há valores escritos pelo teste e depois comparados — sobretudo resposta, veredito e lâmpadas — como **preservação de premissas**. Eles não demonstram que o sistema criou essas premissas; o código agora explicita essa distinção e não reescreve a célula depois do fecho (`T:147`, `T:156`, `T:159`, `T:161`, `T:238`, `T:239`). [nao verificado]

4. **Sim, existem outros sobreviventes por inspeção estática; sobrevivência medida: [nao verificado].**

   - **Eliminar `d['alcance_' + causa] += 1` em `V:651`.** Universo, total de fio morto e linhas permanecem iguais; são os resultados comparados pelos testes. `alcance_c`/`alcance_e` ficariam errados sem uma asserção correspondente (`V:553`, `V:650`, `V:654`; `T:183`, `T:185`, `T:212`, `T:243`). [nao verificado]
   - **Eliminar somente `_desfazer_em_memoria(antes)` em `M:2570`.** A porta continua retornando `False` antes do `save`, e o fecho continua sem persistir aquela instância. Os testes relêem o banco, deixando invisível o objeto interno sujo em memória; isso viola a restauração prometida pela porta (`M:2571`, `M:2576`, `M:2579`; `E:1638`, `E:1639`; `T:174`, `T:205`, `T:237`, `T:253`). [nao verificado]

5. **Não identifiquei relógio solto, vazamento do mock ou dependência explícita de ordem no desenho destes quatro testes.** A classe congela o tempo, o censo recebe `HOJE` explicitamente e o controle usa instante literal; os `now()` desses caminhos de produção ficam sob o congelamento declarado (`T:100`, `T:166`, `T:226`; `E:1623`; `M:2550`; `P:772`). [nao verificado]

   O mock termina antes das testemunhas, e sua restauração é verificada por identidade. Cada método cria seu cenário, com dados comuns declarados em `setUpTestData`, numa classe `TestCase` (`T:201`, `T:204`, `T:101`, `T:103`, `T:189`, `T:200`, `T:234`, `T:251`). Execução em ordens diferentes e confirmação prática do isolamento: **[nao verificado]**.

## 01/11 18:14 · sessao 43184c58c13e · - · - · origem=Ronald (tmux)

Mutantes sobreviventes por inspeção estática; execução **[não verificado]**:

1. **Excluir o rollback da recusa** em `app/chamados/models.py:2570`. A porta continua retornando `False` antes do `save` (`models.py:2571`, `models.py:2576`), e o fecho continua imediatamente (`app/chamados/services/disputa_emissao.py:1638`). O objeto interno fica alterado em memória, mas os testes observam instâncias relidas do banco (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:174`, `:205`, `:237`, `:253`). Viola o contrato de restituição integral da instância (`app/chamados/models.py:2579`).

2. **Excluir a atualização do contador por causa** em `app/chamados/services/vigia_fio.py:651`. Preserva universo (`:553`), total de fio morto (`:650`) e linhas (`:654`); são precisamente os resultados assertados (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:181`, `:185`, `:212`, `:242`, `:243`). Os testes não assertam `alcance_c`/`alcance_e`, embora a produção declare que as causas devem somar o total (`app/chamados/services/vigia_fio.py:499`). Excluir a atualização de `alcance_e_detalhe` também passa por essa lacuna (`:652`).

O descarte do mutante da linha 1626 exige uma ressalva: **“mesmo juiz” não demonstra equivalência universal**, pois a porta consulta o C6 após mudar o estado (`app/chamados/models.py:2555`, `:2558`, `:2565`). Contraexemplo estático: pergunta sem resposta com motivo `saida_sem_entrada`, célula `furo` por outro marco, mas E/hi acesa (`app/chamados/juizes.py:844`). Sem carimbo, o veredito acusador a mantém viva (`:1083`); com carimbo, a lâmpada própria a mata antes dessa decisão (`:1077`). Retirar a guarda permitiria gravar (`app/chamados/models.py:2576`). Os fixtures atuais usam `batida_ausente`, sem esse marco próprio (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:145`; mapa em `app/chamados/juizes.py:844`). Ocorrência desse contraexemplo em produção: **[não verificado]**.

## 01/11 18:14 · sessao 2bb9051113f1 · - · - · origem=Ronald (tmux)

Os dois descartes têm alcances diferentes:

- **#7: sustentado para o cenário dos quatro testes, mas “equivalente” globalmente é forte demais.** A guarda lê a pergunta antes do carimbo (`app/chamados/services/disputa_emissao.py:1626`); a porta escreve carimbo e eventualmente resposta em memória antes de consultar novamente o juiz (`app/chamados/models.py:2555`, `app/chamados/models.py:2558`, `app/chamados/models.py:2565`). Portanto, mesmo juiz não significa mesma entrada.

  Contraexemplo estático: pergunta `saida_sem_entrada`, sem resposta, célula `furo`, E acesa e outro marco apagado. Sem carimbo, C6 chega ao veredito acusador e retorna viva (`app/chamados/juizes.py:1081`). Com carimbo, C6 verifica antes a lâmpada do marco e retorna morta (`app/chamados/juizes.py:1077`); o mapeamento desse motivo para E está em `app/chamados/juizes.py:845`, e a consulta da lâmpada em `app/chamados/juizes.py:863`. Retirar só a guarda permite gravar nesse caso (`app/chamados/models.py:2576`). Contraexemplo inferido do código; execução [não verificado].

- **#8: sustentado como entrada adversarial.** O teste declara e injeta texto sem `respondida_em` (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:147`). O escritor real de resposta atribui ambos e salva juntos (`app/chamados/services/disputa_emissao.py:1247`, `app/chamados/services/disputa_emissao.py:1266`). O I20 consulta justamente texto preenchido com instante nulo (`app/chamados/management/commands/auditar_invariantes_chamados.py:1101`). Isso verifica detecção/preservação da entrada injetada, sem comprovar sua origem em produção; B=0/frota/sonda [não verificado].

Quanto a tempo e isolamento: classe congelada em `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:100`; vigia recebe `HOJE` explicitamente na linha 166; controle usa instante literal na linha 226. O mock está limitado ao contexto das linhas 201–202, com identidade restaurada assertada na linha 204. As quatro rotinas criam cenário próprio nas linhas 189, 200, 234 e 251; a classe usa `TestCase` na linha 101. Ausência de dependência de ordem em execução [não verificado].

Não executar o conjunto sob esta autorização: o MORDE do I20 cria e escreve arquivo temporário (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:259`, `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:262`).

## 01/11 17:56 -- HISTORICO (mais de 24 h) · sessao 90d1cb24554b · E4-CALENDARIO-L1 · leitura · origem=Code

1) **O problema de desenho, em seis linhas.**

- “Status do dia” reúne natureza do dia, andamento do turno, julgamento e disponibilidade de dados no mesmo vocabulário. (`app/ponto/status_dia.py:28`)
- Há duas regras: o calendário deriva `status`; o D4 combina relógio e coluna persistida. Compartilhar o vocabulário não unifica essas regras. (`app/colaboradores/services/calendario.py:801`; `app/ponto/status_dia.py:196`)
- A própria palavra `falta` tem contratos diferentes: o calendário pode produzi-la por ausência de batida; L-039 exige falta decidida. (`app/colaboradores/services/calendario.py:133`; `app/docs/LEIS.md:87`)
- A célula é referência viva em qualquer competência, mas o lote de status congela justamente as entregues. (`app/docs/ADR/0003-celula-tres-naturezas.md:25`; `app/ponto/management/commands/lavrar_status_competencia.py:40`)
- “Lavrado” identifica a origem da resposta, sem certificar sua atualidade: o D4 lê o status sem conferir a impressão dos insumos. (`app/ponto/status_dia.py:203`)
- Igualdade entre dois status não certifica frase, ação disponível ou cobrança do marco; esses campos têm outros predicados. (`app/colaboradores/services/calendario.py:886`; `app/colaboradores/services/calendario.py:189`; `app/chamados/juizes.py:1073`)

2) **Quais colunas não encontram explicação suficiente na regra escrita?**

Os quantitativos abaixo são os anexos fornecidos, sem reprodução independente [nao verificado]. A comparação mede impacto; nenhum dos caminhos vira gabarito pelo número de coincidências, conforme L-110. (`app/docs/LEIS.md:136`)

| Coluna ou classe da saída | O que é explicável; o que permanece sem explicação |
|---|---|
| `tempo` | Passado não implica encerrado: é necessário ultrapassar o fim previsto e não haver turno vivo. Portanto `tempo` sozinho não explica nem condena `aberto`, `previsto` ou `falta`. (`app/ponto/turnos.py:2113`) O instante e o motivo individual de encerramento não aparecem nos anexos [nao verificado]. |
| `tela_hoje=falta` × `d4=irregular/ok` | A diferença entre receitas é explícita: a tela admite falta sem decisão; a lavra trata `nunca_bateu` como irregular e admite isento sem batida como `ok`. (`app/colaboradores/services/calendario.py:133`; `app/ponto/status_dia.py:55`; `app/ponto/status_dia.py:108`) **A acepção automática de falta não se sustenta em L-039.** (`app/docs/LEIS.md:87`) Qual dessas condições ocorreu em cada amostra [nao verificado]. |
| `folga/ausencia` × `previsto`, hoje ou futuro | O D4 consulta o relógio antes da coluna; a tela pode devolver ausência ou feriado antes do ramo temporal. Isso explica mecanicamente a diferença, sem provar erro individual. (`app/ponto/status_dia.py:197`; `app/colaboradores/services/calendario.py:109`) O ADR D4 manda devolver `aberto/previsto` antes do encerramento. (`app/docs/ADR/0002-celula-e-o-dia.md:21`) |
| Demais diferenças com `fonte=lavrado` | Não há no anexo veredito/via, ata, cobertura parcial, isenção ou impressão por dia; escolher o resultado devido para cada par fica [nao verificado]. Esses insumos mudam a resposta da lavra. (`app/ponto/status_dia.py:96`; `app/ponto/portas/celula.py:694`) |
| `nulo`, `completaveis`, `fila_cartorio`, `entregue` | O segundo anexo relata **35.432 completáveis, fila zero; 29.950 em entregues** [nao verificado]. Nulo completável é distinto de carimbo antigo no código. (`app/ponto/services/cartorio.py:758`) O bloqueio das entregues tem respaldo no ADR 0002, mas contraria a natureza viva declarada no ADR 0003: **há conflito escrito, não uma explicação normativa única.** (`app/docs/ADR/0002-celula-e-o-dia.md:68`; `app/docs/ADR/0003-celula-tres-naturezas.md:25`) |
| `sem_dados/sem_celula` | D4 retorna isso antes de consultar natureza ou relógio. (`app/ponto/status_dia.py:191`) Dos 1.230 dias do segundo anexo, 120 estão classificados como vínculo existente e 1.110 como sem vínculo [nao verificado]. Os primeiros são candidatos à quebra de cobertura; os demais não são defeito demonstrado apenas pela situação atual da pessoa. A obrigação escrita é por **dia de vínculo vigente**. (`app/ponto/portas/celula.py:561`) |
| `rotulo_perdido={'outro':…,'em_curso':…}` | A tela tem frases específicas para andamento e retenção; a lavra também guarda frase própria do isento. (`app/colaboradores/services/calendario.py:886`; `app/ponto/status_dia.py:48`) O significado operacional de `outro` e quais frases ele agrega ficam [nao verificado]. |
| `resolver_dia={'nasce':27,'some':250}` | O predicado depende de status, trabalho, data e último recurso; este último também depende da pergunta viva respondida. (`app/colaboradores/services/calendario.py:189`; `app/colaboradores/services/calendario.py:571`; `app/colaboradores/services/calendario.py:901`) A transformação usada pela sonda e a legitimidade de cada nascimento/desaparecimento ficam [nao verificado]. |
| `iguais`, `diferentes`, `ERROS 0`, `SONDA_OK`, `janela_x_dia_dia_a_dia {}` | Os anexos relatam resultados de comparação e execução [nao verificado]. Eles não demonstram conformidade com a regra nem cobertura adversarial; igualdade com o código anterior é impacto, não certificação. (`app/docs/LEIS.md:136`) |

3) **Que perguntas faltaram, e quem escreve/lê os campos?**

A afirmação histórica de que “ninguém perguntou” seria [nao verificado]. O confronto exige estas perguntas:

- **Estamos comparando a mesma pergunta?** Natureza vencedora, cobrança do dia encerrado, turno vivo e pendência de um marco têm contratos distintos. (`app/ponto/precedencia.py:46`; `app/ponto/status_dia.py:158`; `app/ponto/turnos.py:2105`; `app/chamados/juizes.py:1073`)
- **Qual é o universo obrigatório?** Pessoa atualmente viva, vínculo cobrindo aquela data e célula existente não são equivalentes; o gerador consulta vínculos ativos, enquanto a escolha diária também admite histórico encerrado. (`app/ponto/portas/celula.py:568`; `app/escala/alimentacao.py:177`)
- **Conhecer um fato humano exige célula?** A precedência pode reconhecer ausência/feriado sem célula, enquanto D4 encerra a leitura em `sem_celula`. (`app/ponto/precedencia.py:375`; `app/ponto/status_dia.py:192`)
- **“Lavrado” significa atual, completo ou apenas existente?** D4 não confere impressão; D5 considera já lavrado qualquer status não nulo, sem examinar o rótulo. (`app/ponto/status_dia.py:203`; `app/ponto/services/cartorio.py:758`)
- **Todo insumo capaz de mudar a resposta invalida o julgamento?** O ADR reconhece isenção fora da impressão; a função de impressão não recebe esse campo. (`app/docs/ADR/0002-celula-e-o-dia.md:59`; `app/ponto/services/cartorio.py:356`)
- **O passado consultado é referência atual ou fotografia entregue?** O ADR 0003 reserva a fotografia ao gravado e declara a célula viva. (`app/docs/ADR/0003-celula-tres-naturezas.md:25`)
- **A comparação conserva frase, palavra, ação e contagem?** O calendário publica esses componentes separadamente; seu resumo não possui parcelas para `sem_certidao` e `sem_dados`, embora ambos pertençam ao vocabulário. (`app/colaboradores/services/calendario.py:883`; `app/colaboradores/services/calendario.py:935`; `app/ponto/status_dia.py:42`)

Esta é a cadeia de escritores e leitores constatados; completude de um censo global além dos arquivos abertos [nao verificado].

| Campos envolvidos | Quem escreve ou deriva | Quem lê |
|---|---|---|
| `trabalha`, `dna`, `escala_geradora` | Geração, regeneração e reversão, declaradas no inventário do ADR; criação concreta pela porta. (`app/docs/ADR/0003-celula-tres-naturezas.md:41`; `app/ponto/portas/celula.py:593`) | Precedência, vínculo diário e encerramento. (`app/ponto/precedencia.py:349`; `app/escala/alimentacao.py:237`; `app/ponto/turnos.py:2164`) |
| Vigência `data_inicio/data_fim/ativa` | Fechamento por `fechar_vigencia`; demais escritores de início não recenseados aqui [nao verificado]. (`app/colaboradores/services/vinculo.py:561`) | Cobertura diária, geração e seleção do vínculo. (`app/escala/alimentacao.py:182`; `app/ponto/portas/celula.py:588`; `app/escala/alimentacao.py:250`) |
| Batidas: `timestamp`, `tipo`, origem/retratação | Registro por `registrar_batida`; operações de alteração/retratação no mesmo módulo. (`app/ponto/registro_batida.py:94`; `app/ponto/registro_batida.py:235`; `app/ponto/registro_batida.py:267`) | Calendário pelos turnos; impressão dos insumos. (`app/colaboradores/services/calendario.py:301`; `app/ponto/services/cartorio.py:366`) |
| Ausência, estado, efeito e cobertura | Criação pelo serviço de ausência; decisão do dia pode criar ausência aprovada. (`app/ponto/services/ausencia.py:130`; `app/ponto/services/veredito_celula.py:315`) | Calendário consulta cobertura para cobrança e folha separadamente; palavra lê cobertura/decisão. (`app/colaboradores/services/calendario.py:582`; `app/colaboradores/services/calendario.py:626`; `app/ponto/services/dia_decidido.py:363`) |
| `FolgaDia` e origem de calendário/manual | `gerar_foto_folgas` e `lancar_folga`. (`app/escala/folgas.py:150`; `app/escala/folgas.py:228`) | Precedência seleciona a folga válida para o vínculo. (`app/ponto/precedencia.py:358`) |
| `ata`, `veredito`, `veredito_via`, `protestos`, `veredito_em` | `lavrar_veredito`; preserva a decisão administrativa durante lavra do cartório. (`app/ponto/portas/celula.py:709`; `app/ponto/portas/celula.py:737`) | Calendário, regra do status e C6. (`app/colaboradores/services/calendario.py:510`; `app/ponto/status_dia.py:101`; `app/chamados/juizes.py:1081`) |
| `impressao`, `julgada_em`, `ausencia`, `tem_entrada/tem_saida`, `insumos_em` | `carimbar`, com fontes/naturezas inventariadas. (`app/ponto/portas/celula.py:787`; `app/docs/ADR/0003-celula-tres-naturezas.md:49`; `app/docs/ADR/0003-celula-tres-naturezas.md:58`) | D5 recebe a distinção entre fatos atuais e fila antiga. (`app/ponto/services/cartorio.py:752`) |
| `status_dia`, `status_rotulo` persistidos | `_aplicar_status`, dentro de `lavrar_veredito`, pela regra da lavra ou decisão administrativa. (`app/ponto/portas/celula.py:694`; `app/ponto/portas/celula.py:705`) | D4 lê o par; D5 lê a nulidade do status. (`app/ponto/status_dia.py:203`; `app/ponto/services/cartorio.py:758`) |
| `status`, `status_rotulo`, `resumo` da tela | `_classificar` e montagem do calendário. (`app/colaboradores/services/calendario.py:801`; `app/colaboradores/services/calendario.py:886`; `app/colaboradores/services/calendario.py:935`) | Grade e rodapé; portas curtas de status e contagem. (`app/templates/colaboradores/partials/_calendario_grade.html:69`; `app/templates/colaboradores/partials/_calendario_grade.html:142`; `app/colaboradores/services/calendario.py:204`; `app/colaboradores/services/calendario.py:216`) |
| Encerramento; retorno D4 `status/rotulo/fonte` | Encerramento derivado por `dias_encerrados`; resposta derivada pelo D4, sem persistir o relógio. (`app/ponto/turnos.py:2125`; `app/ponto/status_dia.py:196`) | Calendário lê motivos; D4 lê célula e motivos. (`app/colaboradores/services/calendario.py:699`; `app/ponto/status_dia.py:191`) |
| Pergunta: resposta, validação, materialização e via | Resposta em `disputa_emissao`; validação em `validacao`; materializador e portas do modelo também carimbam estados. (`app/chamados/services/disputa_emissao.py:1247`; `app/chamados/services/validacao.py:127`; `app/chamados/services/materializacao.py:509`; `app/chamados/models.py:2485`) | C6 lê esses estados, veredito, fato do marco e pendências; há classificação residual pela jornada. (`app/chamados/juizes.py:1032`; `app/chamados/juizes.py:1094`; `app/chamados/juizes.py:1099`) |
| `pode_ultimo_recurso`, `resolver_dia_oferecido` | Calendário combina competência, pergunta viva e predicado da ação. (`app/colaboradores/services/calendario.py:901`; `app/colaboradores/services/calendario.py:932`) | Template condiciona o controle pela permissão do usuário. (`app/templates/colaboradores/partials/_calendario_grade.html:125`) |
| `DiaPago`/rubricas e palavra visual | Dinheiro por `dia_pago.lavrar`, chamado pelo fechamento; palavra por `dia_decidido.do_dia`. (`app/ponto/services/dia_pago.py:135`; `app/ponto/services/fechamento.py:1168`; `app/ponto/services/dia_decidido.py:399`) | Calendário lê dinheiro e publica palavra/cor separadamente do status. (`app/colaboradores/services/calendario.py:370`; `app/colaboradores/services/calendario.py:874`; `app/colaboradores/services/calendario.py:883`) |
| Contadores/rótulos exclusivos das sondas | Escritores e critérios de `rotulo_perdido`, `nasce/some`, `iguais/diferentes` não apresentados [nao verificado]. | Leitura pelos anexos fornecidos [nao verificado]. |

4) **Propriedades verificáveis por máquina, sem LLM.**

São critérios de detecção derivados dos contratos citados; sua execução nesta cópia e seus resultados ficam [nao verificado].

| Classe de divergência | Propriedade exigível |
|---|---|
| **Falta sem decisão** | `status=falta ⇒ decisão descontável válida para o dia`. Casos sintéticos distinguem furo, falta aprovada e abono, sem tomar o caminho antigo como oráculo. (`app/docs/LEIS.md:87`; `app/docs/LEIS.md:136`) |
| **Precedência/realização** | Casos pela regra cobrem ausência integral/parcial, folga humana, feriado, trabalho em feriado, batida e decisão administrativa; o resultado deve respeitar cada condição, inclusive a exceção parcial. (`app/ponto/precedencia.py:11`; `app/ponto/status_dia.py:96`; `app/ponto/portas/celula.py:694`) |
| **Relógio/teto** | Com instante fixo, nenhum encerramento antes do fim previsto ou com turno vivo; sem previsto, motivo `sem_previsto`. Testar fronteiras e turnos atravessando meia-noite. (`app/ponto/turnos.py:2113`; `app/docs/LEIS.md:100`) |
| **Dependência da janela** | Para o mesmo instante e dados, `dias_encerrados(c,I,F)[d] == dia_encerrado(c,d)`; também igualdade entre consulta unitária e lote D4. (`app/ponto/turnos.py:2134`; `app/ponto/status_dia.py:169`) |
| **Buraco de materialização** | Anti-join: todo `(pessoa,data)` com vínculo diário válido no universo obrigatório deve possuir célula. Segregar “sem vínculo” evita contar ausência legítima de domínio como falha. (`app/ponto/portas/celula.py:561`; `app/escala/alimentacao.py:182`) |
| **Nulo/completável/fila** | No mesmo universo elegível, `total = já_lavrado + completável + fila`; aplicação repetida é idempotente e completar status preserva veredito, ata e data do veredito. (`app/ponto/services/cartorio.py:752`; `app/ponto/services/cartorio.py:758`; `app/ponto/portas/celula.py:753`) |
| **Congelamento indevido da referência** | Mantidos insumos e instante, marcar a competência como entregue não pode alterar o resultado da referência viva; a fotografia histórica é outra entidade. Essa propriedade expõe o conflito do lote atual. (`app/docs/ADR/0003-celula-tres-naturezas.md:25`; `app/ponto/management/commands/lavrar_status_competencia.py:40`) |
| **Lavratura desatualizada** | Após consumir os eventos, impressão persistida deve corresponder aos insumos atuais. Teste metamórfico: alteração que muda julgamento precisa mudar a chave de atualidade ou provocar invalidação explícita; variar isenção detecta a lacuna já declarada. (`app/docs/ADR/0002-celula-e-o-dia.md:16`; `app/docs/ADR/0002-celula-e-o-dia.md:59`) |
| **Rótulo perdido** | Comparar o par `(status,rótulo)`, incluindo isenção, andamento e retenção; igualdade apenas do status não satisfaz esse contrato. (`app/ponto/status_dia.py:48`; `app/ponto/status_dia.py:205`; `app/colaboradores/services/calendario.py:886`) |
| **Ação nasce/some** | Verificar `resolver == último_recurso ∧ trabalho ∧ passado ∧ status_pede_decisão`; na migração, comparar a projeção completa sob os mesmos insumos. Para C6, marco satisfeito não pode permanecer cobrado apenas porque outro marco acusa o dia. (`app/colaboradores/services/calendario.py:189`; `app/chamados/juizes.py:1073`) |
| **Rodapé incompleto** | Histograma exclusivo dos status deve somar `len(dias)` e coincidir entre tela, contagem e resumo; HE fica fora dessa soma porque é selo sobreposto. (`app/colaboradores/services/calendario.py:217`; `app/colaboradores/services/calendario.py:935`; `app/colaboradores/services/calendario.py:940`) |
| **Dois leitores concordam no erro** | Casos e propriedades independentes pela regra continuam obrigatórios mesmo com divergência zero; comparação entre implementações só mede coerência/impacto. (`app/docs/LEIS.md:136`) |

## 01/11 17:53 -- HISTORICO (mais de 24 h) · sessao 81f096571943 · - · - · origem=Ronald (tmux)

Leitura concluída, sem escrita. Os anexos não trazem perguntas, respostas, atas ou marcos; qualquer explicação individual por C6 continua [nao verificado].

**Fato de desenho útil à consolidação**

`_pergunta_viva_calc` não consome `status_dia`/`status_rotulo`: decide por assinatura, tranca, resposta, veredito/via, via/materialização, fato do marco e provisoriedade da ata; ainda chama `classificar_falta` quando não resolveu pelos degraus anteriores. Portanto “status da tela”, “status lavrado” e “pergunta viva” respondem perguntas distintas, com uma recomputação residual no C6. (`app/chamados/juizes.py:1032–1118`; L-061 em `app/docs/LEIS.md:95`.)

C6 distingue explicitamente **marco perguntado** de **dia inteiro**: uma pergunta materializada morre se a lâmpada daquele marco acendeu, mesmo se outro marco mantém o dia acusando. (`app/chamados/juizes.py:1073–1084`.) Em célula não acusadora, `marcos_pendentes` impede absolvição provisória; o fato do próprio marco pode absolvê-la. (`app/chamados/juizes.py:1085–1096`.)

**Regras escritas relevantes**

- L-002: testemunhas leem a mesma autoridade; derivação própria é divergência estrutural. (`app/docs/LEIS.md:60`.)
- L-020: DNA do passado congelado, exceção formal pela porta com denúncia. (`app/docs/LEIS.md:71`.)
- L-039: “falta” significa ausência decidida cujo tipo desconta; não basta furo de batida. (`app/docs/LEIS.md:87`.)
- L-061: chamado, pergunta, tela e folha leem célula/ata. (`app/docs/LEIS.md:95`.)
- L-073: data passada sozinha não prova encerramento; comparar instante ou exigir fato encerrado. (`app/docs/LEIS.md:100`.)
- L-089: atestado só produz efeito em dia previsto para trabalho; o índice registra tensão com classificação em folga. (`app/docs/LEIS.md:121`, `:172`.)
- L-110: comparação velho × novo mede impacto; regra aplicada a cadastro/batidas e propriedades fixas certificam. (`app/docs/LEIS.md:136`.)

**Perguntas de desenho a explicitar — não é possível afirmar historicamente que “ninguém perguntou” [nao verificado]**

1. “Falta” da tela é a falta decidida de L-039, ou ausência de marcação? Sem cadastro da ausência e seu status no anexo não há gabarito individual. (`app/docs/LEIS.md:87`; `app/chamados/juizes.py:1097–1103`.)
2. Quando duas projeções concordam em `status`, concordam também em ação oferecida, marco cobrado, frase e informação provisória? O C6 decide por marco e teto, enquanto o mapa declara `resolver_dia_oferecido` como outro predicado. (`app/chamados/juizes.py:1073–1096`; `app/docs/MAPA/calendario-dia-da-tela.md:10–13`.)
3. A pergunta aponta para a mesma `(colaborador,data)` da célula? O próprio alimentador recusa FK divergente; ausência de célula confiável é `None`, diferente de mapa que afirma inexistência. (`app/chamados/juizes.py:586–606`.)
4. “Sem pergunta viva” significa fato resolvido ou apenas competência trancada? C6 cala a cobrança por leitura de tranca sem absolver o dia. (`app/chamados/juizes.py:1035–1043`.)
5. Qual instante e qual estado dos insumos sustentam a comparação? C6 usa janela viva e ata provisória; o anexo não fornece essa identidade completa. (`app/chamados/juizes.py:533–565`, `:1085–1096`; [nao verificado].)

**Inventário dos campos lidos pelo C6 e escritores efetivamente abertos**

| Campo | Escritor observado | Leitor observado |
|---|---|---|
| `resposta_colab`, `respondida_em` | resposta do colaborador; automática; carimbo pela trilha | C6, ramo resposta |
| | `app/chamados/services/disputa_emissao.py:1247–1266`, `:1381–1413` | `app/chamados/juizes.py:1047–1059` |
| `validada_em`/`validada_por` | `_carimbar_validacao`; resposta nova desfaz; materializador também assina irmãs com texto | C6 encerra antes dos demais degraus |
| | `app/chamados/services/validacao.py:127–133`; `app/chamados/services/disputa_emissao.py:1250–1266`; `app/chamados/services/materializacao.py:513–532` | `app/chamados/juizes.py:1033–1034` |
| `materializada_em`, `via_resolucao` | materializador; portas do modelo para admin, tranca e fato; retratação de duplicata | C6 encerra por identidade ou exige testemunha |
| | `app/chamados/services/materializacao.py:509–532`; `app/chamados/models.py:2485–2541`; `app/chamados/services/fio.py:878–897` | `app/chamados/juizes.py:1042`, `:1066–1079`, `:1107–1118` |
| `celula`, `data_evento`, `motivo`, `tipo_resposta` | modelo declara identidade/snapshot; censo completo de escritores não realizado [nao verificado] | alimentador, lâmpada do marco e C6 |
| | `app/chamados/models.py:2193–2207` | `app/chamados/juizes.py:598–606`, `:863–890`, `:1097–1103` |
| `CelulaDia.veredito`, `veredito_via`, `ata` | modelo delega à porta; ato admin chama `lavrar_veredito`; admin cria ausência aprovada ou materializa batida | C6 e prova do marco |
| | `app/escala/models.py:1787–1796`; `app/ponto/services/veredito_celula.py:299–330` | `app/chamados/juizes.py:1044–1046`, `:1081–1096`, `:887–893` |
| `status_dia`, `status_rotulo` | declaração de escritor na porta e nulidade “ainda não lavrado” | D4 declarado; C6 não os lê |
| | `app/escala/models.py:1775–1785` | `app/escala/models.py:1780`; corpo aberto em `app/chamados/juizes.py:1032–1118` |

**Propriedades mecânicas úteis**

- **Identidade:** toda pergunta usada como prova satisfaz `p.celula.colaborador_id == p.disputa.colaborador_id` e `p.celula.data == p.data_evento`; violações são identidade divergente, não diferença de classificação. (`app/chamados/juizes.py:589–606`.)
- **Falta sem decisão:** `status == falta ⇒ existe ausência decidida descontável para o dia`; distingue furo de falta sem usar o código antigo como oráculo. (`app/docs/LEIS.md:87`, `:136`.)
- **Prova do marco:** pergunta materializada, não respondida pelo ramo especial nem retida por assinatura/tranca, com `fato_do_marco_no_espelho(p) is True`, deve estar morta; dia acusador por outro marco não a revive. (`app/chamados/juizes.py:1033–1079`.)
- **Teto:** célula sem acusação mas com marco pendente não prova encerramento da pergunta sem a lâmpada do próprio marco; executar com relógio fixo permite testar fronteiras antes/depois do vencimento. (`app/chamados/juizes.py:1085–1096`; `app/docs/LEIS.md:100`.)
- **Par resposta:** `respondida_em != None ⇒ trim(resposta_colab) != ''`; o código já nomeia esse estado ilegal. (`app/chamados/juizes.py:1154–1157`.)
- **Idempotência:** repetir a mesma decisão não altera estado/carimbo; portas admin e duplicata têm guardas explícitas. (`app/chamados/models.py:2490–2502`; `app/chamados/services/fio.py:882–897`.)
- **Ação não reduzida a cor:** igualdade de status, isoladamente, não certifica igualdade de `resolver_dia_oferecido` ou de pergunta viva, pois seus predicados têm entradas diferentes. A propriedade verificável é comparar a projeção completa do contrato, sob os mesmos insumos/instante. (`app/docs/MAPA/calendario-dia-da-tela.md:10–13`; `app/chamados/juizes.py:1032–1118`; formulação de propriedade: [nao verificado].)

## 01/11 17:53 -- HISTORICO (mais de 24 h) · sessao c8a34173d33c · - · - · origem=Ronald (tmux)

Fatos e pontos de desenho confirmados:

- ADR 0002 declara status/rotulo na célula, escritor único no mesmo ato do veredito e relógio derivado; leitores permanecem congelados até E4. `app/docs/ADR/0002-celula-e-o-dia.md:8`, `:12`, `:21`, `:73`.
- Há conflito declarado entre o lote que recusa competência entregue e a natureza viva da célula. A recusa está em `app/ponto/management/commands/lavrar_status_competencia.py:40`; o próprio ADR 0003 registra-a em `app/docs/ADR/0003-celula-tres-naturezas.md:106`, embora declare julgamento vivo em qualquer competência (`:25`) e fotografia somente no gravado (`:27`).
- A L-092 protege o GRAVADO (`app/docs/LEIS.md:125`); L-113 revoga recusa de recálculo em exportada (`:148`). Não cabe justificar congelamento do status vivo apenas pela L-092. O ADR 0002 ainda manda o lote não tocar entregue (`app/docs/ADR/0002-celula-e-o-dia.md:68`).
- L-039 define falta como decidida, ausência cujo tipo desconta (`app/docs/LEIS.md:87`). A regra da lavra trata `nunca_bateu/furo/cobrado/...` como irregular (`app/ponto/status_dia.py:54`, `:112`), enquanto falta requer o `tipo_dia='falta'` (`:96`) ou decisão admin `faltou` (`:52`). Para classificar cada par `falta/ok` do anexo seriam necessários decisão humana, isenção, grade e fatos do mesmo julgamento; estes não estão no dado [nao verificado].
- L-110 exige oráculo pela regra, não igualdade com código anterior; comparação velho/novo mede impacto (`app/docs/LEIS.md:136`). Assim `iguais` não certifica correção e `diferentes` não prova defeito.

D4, com ordem exata:

1. Sem célula → `sem_dados/sem_celula`, antes de relógio ou natureza humana: `app/ponto/status_dia.py:191`.
2. Motivo de não encerramento diferente de `sem_previsto` → `previsto/relogio` quando motivo é relógio e não há turno com aquela `data_turno`; demais casos `aberto/relogio`: `app/ponto/status_dia.py:196`.
3. Depois → coluna lavrada e rótulo persistido, com fallback para rótulo canônico: `app/ponto/status_dia.py:203`.
4. Célula sem status → `previsto/nao_lavrado` se data ≥ hoje, `sem_certidao/nao_lavrado` caso contrário: `app/ponto/status_dia.py:207`.

Logo, `tempo='passado'` não prova encerramento, e “sem janela” segue lavra; o contrato escrito confirma a distinção em `app/ponto/status_dia.py:160`. Os anexos não mostram motivos individuais de encerramento [nao verificado].

Escritores/leitores verificados:

| Campo | Escritor | Leitores verificados |
|---|---|---|
| `status_dia`, `status_rotulo` persistidos | `_aplicar_status`, chamado pela porta `lavrar_veredito`; par atribuído junto em `app/ponto/portas/celula.py:701`; save junto do veredito em `:773` | D4 em `app/ponto/status_dia.py:203`; D5 verifica apenas `status_dia` em `app/ponto/services/cartorio.py:758` |
| Status/rotulo provenientes de decisão humana | `_status_alvo` usa `status_do_admin` quando via/admin prevalece: `app/ponto/portas/celula.py:694`; tabela em `app/ponto/status_dia.py:52` | A própria porta preserva decisão admin durante lavra cartório, podendo atualizar ata/status: `app/ponto/portas/celula.py:737` |
| Status/rotulo provenientes do cartório | `status_da_lavra` sobre fatos e o mesmo veredito gravado, `app/ponto/portas/celula.py:698`; fatos extraídos em `app/ponto/status_dia.py:143` | D4 conforme acima |
| `fonte`, `status`, `rotulo` de retorno do D4 | D4, sem gravação: `app/ponto/status_dia.py:192`, `:199`, `:204`, `:208` | Painel `_d4_em_lote`, atribui resposta à linha após confirmar encerramento: `app/core/services/painel_op.py:334` |
| `impressao`, `julgada_em`, `ausencia`, `tem_entrada`, `tem_saida`, `insumos_em` | `carimbar`, `app/ponto/portas/celula.py:800` | D5 compara julgamento/impressão/veredito em `app/ponto/services/cartorio.py:1032` e `:1190` |
| `status_completaveis`, `status_fila_cartorio`, `status_ja_lavrado`, `status_completados` | `_completar_status`, `app/ponto/services/cartorio.py:758` | Command imprime em `app/ponto/management/commands/lavrar_status_competencia.py:57` |

Inventário mais amplo, **declarado pelo ADR, não recenseado por mim**: natureza/fonte/escritor de todas as colunas está em `app/docs/ADR/0003-celula-tres-naturezas.md:35`; leitores são pisos de contagem (`:5`) e não lista completa. `status_dia/status_rotulo` estão nas linhas `:56` e `:57`.

Sobre D5:

- D5 considera já lavrado qualquer `status_dia is not None`, sem verificar rótulo ou atualidade: `app/ponto/services/cartorio.py:758`.
- Nulo com impressão atual é completável; nulo sem fatos atuais é fila cartório: `app/ponto/services/cartorio.py:761`.
- D5 não rejulga, não altera veredito/ata/data do veredito; passa fatos à mesma porta e só completa status: `app/ponto/services/cartorio.py:752`, `app/ponto/portas/celula.py:753`.
- DRY com `completaveis` no anexo2 prova contagem desse ramo na execução relatada, não execução futura do command público nem migração já realizada [nao verificado].

Perguntas de desenho e propriedades mecânicas:

- **Status mede qual pergunta?** Há teste que explicita “dia encerrado tem o que cobrar?” e rejeita usar status como natureza conhecida do dia (`app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:4`). Propriedade: leitores de natureza/precedência não podem importar D4/status nem filtrar por status; AST pode impor contrato por papel, não por igualdade textual de status.
- **Ausência de célula é ausência de vínculo, lacuna materializada ou período fora do universo?** D4 responde igual para todos (`app/ponto/status_dia.py:192`). Propriedade: anti-join entre universo de dias esperado definido pela regra de vigência e `CelulaDia`, segregando dias fora de vínculo; uma contagem de sem-célula sem domínio não distingue defeito.
- **“Lavrado” significa atual?** D4 lê coluna sem comparar impressão (`app/ponto/status_dia.py:203`). Propriedade: após processamento dos eventos, `impressao_gravada == impressao_insumos_atuais` e par status/rotulo corresponde ao julgamento daqueles insumos. Não se pode inferir isso de `fonte='lavrado'`.
- **Todo insumo que muda resultado muda impressão?** ADR 0002 admite isenção fora da impressão (`app/docs/ADR/0002-celula-e-o-dia.md:59`); função de impressão efetivamente recebe batidas/cob_status/chamados/DNA/veto/teto/regra/fio/cobparcial/enc, sem isento (`app/ponto/services/cartorio.py:356`). Propriedade metamórfica: duas entradas com saída de julgamento diferente exigem fingerprint diferente ou invalidação/evento explícito. Teste pode variar isenção, cobertura, feriado, vínculo etc. sem LLM.
- **Rótulo é dado semântico?** Modelo aceita ambos nulos independentemente (`app/escala/models.py:1784`); escritor os muda juntos (`app/ponto/portas/celula.py:705`); D4 reconstrói canônico se rótulo ausente (`app/ponto/status_dia.py:205`). Propriedade: par `(status,rotulo)` completo e preservado em leitura/render, incluindo `ok/isento de ponto` (`app/ponto/status_dia.py:48`). Comparar só status não captura perda de mensagem.
- **Entregue muda elegibilidade de referência viva?** Propriedade: mesmos insumos vivos + mesmo instante dão o mesmo par vivo independentemente do flag entregue; hash do gravado permanece intacto. Base declarada: `app/docs/ADR/0003-celula-tres-naturezas.md:25` e `:27`. O command atual contradiz a primeira metade em `app/ponto/management/commands/lavrar_status_competencia.py:40`.
- **Migração está completa?** Propriedade contábil por partição: `total = ja_lavrado + completaveis + fila_cartorio` sobre o mesmo universo elegível, com testes de DRY sem escrita, segunda aplicação sem mudança e preservação de veredito/ata/data. Ramos em `app/ponto/services/cartorio.py:758`; testes atuais de idempotência em `app/ponto/tests/test_e3_status_lavrado.py:271`. Não executei testes nem consultei banco [nao verificado].

## 01/11 17:53 -- HISTORICO (mais de 24 h) · sessao a8a6c2bba5fc · - · - · origem=Ronald (tmux)

Achados com evidência direta:

- `passado` não significa encerrado: encerramento exige ausência de turno vivo e fim previsto ultrapassado; sem previsto, a resposta é `sem_previsto`, mesmo para data passada (`app/ponto/turnos.py:2034`, `app/ponto/turnos.py:2050`, `app/ponto/turnos.py:2113`). Logo a coluna `tempo` do anexo não decide legitimidade de `falta`, `aberto` ou `previsto`; sem instante, DNA, turnos e motivo por linha, isso permanece [nao verificado].
- Há dois eixos: jurisdição do vínculo e previsão congelada. `vinculo_do_dia` rejeita geradora fora da vigência e retorna `None`, sem substituir por outro vínculo (`app/escala/alimentacao.py:237`); a precedência ainda lê `celula.trabalha` diretamente quando não nulo (`app/ponto/precedencia.py:349`, `app/ponto/precedencia.py:359`). Pergunta necessária: o que uma célula com previsão, mas sem jurisdição válida, pode afirmar e cobrar?
- A regra explícita é “todo dia de vínculo vigente tem célula; afastamento/ausência cobrem a célula” (`app/ponto/portas/celula.py:561`). Porém o gerador procura apenas vínculos atualmente `ativa=True` (`app/ponto/portas/celula.py:568`), enquanto leitores admitem vínculos encerrados com `data_fim` (`app/escala/alimentacao.py:116`, `app/escala/alimentacao.py:173`). Pergunta necessária: qual universo histórico deve ser materializado?
- Portanto os 120 dias do anexo classificados como `vinculo_ja_existia` — 114 sem fato, quatro feriados, dois atestados — são candidatos à violação de cobertura, mas o predicado da sonda e a validade temporal daqueles vínculos não foram fornecidos [nao verificado]. Os outros 1.110 dias sem vínculo não provam defeito por haver situação atual viva [nao verificado].
- Ausência e feriado podem existir independentemente de vínculo/célula: a precedência carrega ausências por pessoa e intervalo e acrescenta feriado antes de formar o vencedor (`app/ponto/precedencia.py:273`, `app/ponto/precedencia.py:375`). Logo `ausencia/folga` na tela versus `sem_dados/sem_celula` pode representar universos diferentes, sem permitir concluir qual resultado específico é devido [nao verificado].
- A precedência resolve “fato que vence”: férias > afastamento > atestado > feriado > folga > trabalho (`app/ponto/precedencia.py:46`); não resolve, sozinha, realização ou certificado. Folga humana e feriado têm predicado próprio (`app/ponto/precedencia.py:178`). Folga de calendário só vale no vínculo dela; folga manual pertence à pessoa (`app/ponto/precedencia.py:222`).
- O contrato de janela promete igualdade com chamada diária (`app/ponto/turnos.py:2134`), embora o pareamento dependa da janela e a alimentação deva ser da mesma janela (`app/ponto/turnos.py:2128`). O anexo informa `janela_x_dia_dia_a_dia {}`; não mostra violação desse contrato [nao verificado].

Escritores/leitores abertos:

| Campos | Escritor observado/declarado | Leitores abertos |
|---|---|---|
| `trabalha`, `dna`, `escala_geradora` | Geração (`app/ponto/portas/celula.py:593`), regeneração (`app/ponto/portas/celula.py:243`), reversão (`app/ponto/portas/celula.py:532`). Modelo admite edição humana, mas seu executor não foi localizado nesta leitura (`app/escala/models.py:1606`). | Precedência (`app/ponto/precedencia.py:349`); escolha do vínculo (`app/escala/alimentacao.py:237`); turnos (`app/ponto/turnos.py:1882`); encerramento (`app/ponto/turnos.py:2164`). |
| `origem`, trilha da regeneração | Geração marca `gerada` (`app/ponto/portas/celula.py:595`); regeneração guarda DNA anterior, instante e contador (`app/ponto/portas/celula.py:259`). | Gerador preserva `editada` e passado (`app/ponto/portas/celula.py:598`); regeneração restringe às geradas (`app/ponto/portas/celula.py:251`). |
| Vigência `data_inicio/data_fim/ativa` | `validar_vigencia/fechar_vigencia` são donos declarados na L-021; implementação não foi aberta (`app/docs/LEIS.md:72`). | Carregador de candidatos (`app/escala/alimentacao.py:116`), cobertura (`app/escala/alimentacao.py:173`), vínculo diário (`app/escala/alimentacao.py:247`), gerador (`app/ponto/portas/celula.py:588`). |
| `FolgaDia`/`calendario_origem` | Executor de escrita não aberto [nao verificado]. | Carga de todas as folgas da pessoa (`app/ponto/precedencia.py:194`), validade pelo vínculo ou origem manual (`app/ponto/precedencia.py:222`), precedência (`app/ponto/precedencia.py:358`). |
| `status_dia/status_rotulo` | Escritor declarado: porta `lavrar_veredito`, regra `status_da_lavra`; não o gerador (`app/escala/models.py:1775`). | Leitor D4 declarado `status_do_dia_lavrado`; migração das telas é E4 (`app/escala/models.py:1779`). |
| `veredito/ata` e `julgada_em/impressao` | Operações `lavrar_veredito` e `carimbar` da porta (`app/ponto/portas/celula.py:6`). `ata.precedencia` tem dono específico (`app/ponto/portas/celula.py:94`). | Os comentários do modelo declaram folha/exportação como leitores da ata (`app/escala/models.py:1742`); chamadas concretas da folha não abertas [nao verificado]. |
| Motivo de encerramento | Derivado puro, sem persistência: turno vivo, sem previsto ou relógio (`app/ponto/turnos.py:2105`). | `dia_encerrado` (`app/ponto/turnos.py:2075`), janela (`app/ponto/turnos.py:2164`), carga em lote (`app/ponto/turnos.py:3009`). |

Propriedades verificáveis por máquina, como especificações de detecção:

1. **Cobertura:** para cada pessoa/data cujo vínculo diário válido existe dentro do escopo obrigatório, existe exatamente uma célula. A obrigação está em `app/ponto/portas/celula.py:561`; unicidade existe em `app/escala/models.py:1801`. Separar contadores por vínculo histórico/atual e por fato humano permite detectar buracos sem confundir situação atual com vigência.
2. **Proveniência:** geradora pertence à pessoa e sua vigência contém a data; nenhuma escolha muda ao reordenar ou acrescentar candidatos irrelevantes. As regras de autoria/candidatura estão em `app/escala/alimentacao.py:96`, `app/escala/alimentacao.py:237`.
3. **Precedência:** `fatos_do_dia(c,d) == fatos_do_periodo(c,d,d)[d]`, já garantido por delegação (`app/ponto/precedencia.py:242`); fornecimento completo de insumos versus carregamento interno produz o mesmo resultado (`app/ponto/precedencia.py:263`). Folga de outro calendário não altera o vencedor; folga manual válida altera conforme a ordem (`app/ponto/precedencia.py:222`).
4. **Encerramento:** `dias_encerrados(c,I,F,t)[d] == dia_encerrado(c,d,t)` e `bool == (motivo is None)` (`app/ponto/turnos.py:2075`, `app/ponto/turnos.py:2134`). Há teste existente dessas propriedades (`app/ponto/tests/test_c1_dias_encerrados.py:26`, `app/ponto/tests/test_c1_dias_encerrados.py:65`); execução nesta cópia não verificada.
5. **Teto temporal:** nenhum dia é encerrado antes do fim previsto congelado; turno ainda vivo impede encerramento; sem previsto impede encerramento. Fórmula executável em `app/ponto/turnos.py:2113`. Permite distinguir divergência por relógio de divergência por fato/cadastro.
6. **Imutabilidade/idempotência:** repetir geração mantém célula editada e passado; repetir operação com mesmos insumos não altera conteúdo nem contador de regeneração (`app/ponto/portas/celula.py:19`, `app/ponto/portas/celula.py:598`). Após exportação, comparação não autoriza escrita: L-092 (`app/docs/LEIS.md:125`).
7. **Referência:** igualdade entre leitores detecta incoerência estrutural, mas não prova valor correto: a referência deve ser regra aplicada a cadastro e batidas, conforme L-110 (`app/docs/LEIS.md:136`). O anexo não contém esses insumos por dia; nenhum dos dois status pode ser eleito gabarito só por sua origem [nao verificado].

## 01/11 17:51 -- HISTORICO (mais de 24 h) · sessao 06c9e96978f9 · O261-E-F2 · verificador · origem=Code

**A cobertura é parcial:** os quatro testes podem continuar verdes sem a guarda do fecho; o MORDE do ramo não chega ao vigia; e o caso 2 usa um veredito que a porta de produção rejeita. — `app/chamados/services/disputa_emissao.py:1626`; `app/chamados/models.py:2569`; `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:171`; `app/ponto/portas/celula.py:658`.

Esta é uma análise estática da cópia. Execução dos testes, resultados dos logs citados no cabeçalho, suíte completa e correspondência Git com `7af133ad`/`cd623758`: **[não verificado]**.

Para compactar as referências, os caminhos abaixo são relativos a `app/`, salvo `CLAUDE.md`.

**1. O que os quatro testes provam** [nao verificado]

- **T1 — `test_ramo_variante…`:** chama o C6 real, `resolver_perguntas_por_chamado_resolvido` real e `censo_alcance` real. Entretanto, `_fechar` sobrescreve novamente o estado da célula depois da função de produção. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:155`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:163`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:181`.
  
  As asserções medem: C6 vivo antes; retorno dos contadores igual a `{0,0}`; `materializada_em` nulo; `via_resolucao` vazia; C6 vivo depois; exatamente uma linha daquela célula; rota `sem_porta`; detalhe `pergunta_viva_sob_chamado_terminal`; universo e dias sem porta iguais a `(2,2)`. **O índice 6 verificado é o detalhe; a causa está no índice 5 e não é verificada.** — `chamados/tests/test_o261e_f2_ramo_e_i20.py:184`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:168`; `chamados/services/vigia_fio.py:585`.

  O contador `perguntas_resolvidas=0` não prova que o ramo sem credor deixou de carimbar: `n_pq` é incrementado no primeiro ramo, com `chamado_destino`; o ramo sem credor não o incrementa. A pergunta da fixture tem destino nulo. — `chamados/services/disputa_emissao.py:1537`, `chamados/services/disputa_emissao.py:1569`, `chamados/services/disputa_emissao.py:1638`, `chamados/services/disputa_emissao.py:1661`; `chamados/tests/test_o261e_f2_ramo_e_i20.py:142`.

- **T2 — `test_MORDE_ramo…`:** chama o fecho real com `pergunta_viva` substituído por `False`; exige que tenha nascido materialização e que `_assert_ramo` lance `AssertionError`. Isso demonstra rejeição do carimbo pela primeira asserção do helper. **Não demonstra uma leitura adversarial do vigia**, porque o helper aborta antes dela. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:193`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:196`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:171`.

- **T3 — `test_caso2_dia_regularizado…`:** chama C6 real, fecho real, vigia real e auditor real por `call_command`. Verifica C6 morto inicialmente; manutenção de `('07:55', None, None)` em texto/respondida/materializada; PK fora da cicatriz; ausência da célula no alcance; totais `(1,1)`; I20 com `VIOL 1`; e presença da representação textual do PK na linha. Não verifica o retorno do fecho nem o código de saída do auditor. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:207`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:202`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:79`.

- **T4 — `test_MORDE_caso2…`:** chama o fecho real, executa uma cópia integral do auditor com o PK acrescentado à cicatriz e depois executa o comando real. Verifica âncora textual única; cópia com `OK 0`; rejeição dessa saída pelo helper que exige `VIOL 1`; e comando real contando novamente a pergunta. O `assertRaises` da cópia é satisfeito pela divergência de flag/total, antes da checagem do PK. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:222`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:204`.

Os helpers também exigem uma única atribuição AST de `_I20_CICATRIZ` e uma única linha I20 reconhecível na saída. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:65`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:72`.

**Mutantes concretos, sem aplicá-los**

| Mutante | Alteração de produção de 1–3 linhas |
|---|---|
| **F — desligar guarda do fecho** | Trocar `if pergunta_viva(q):` por `if False:`. — `chamados/services/disputa_emissao.py:1626` |
| **P — desligar recusa da porta** | Trocar `if viva:` por `if False:`. — `chamados/models.py:2569` |
| **Z — silenciar I20** | Substituir o queryset passado a `add('I20',…)` por `PD.objects.none()`. — `chamados/management/commands/auditar_invariantes_chamados.py:1101` |
| **C — remover exclusão da cicatriz** | Remover `.exclude(pk__in=_I20_CICATRIZ)`. — `chamados/management/commands/auditar_invariantes_chamados.py:1103` |
| **R — remover filtro do instante** | Trocar `PD.objects.filter(respondida_em__isnull=True)` por `PD.objects.all()`, preservando as exclusões. — `chamados/management/commands/auditar_invariantes_chamados.py:1102` |

Veredictos **previstos por leitura estática**, supondo a fixture isolada, PK fora da cicatriz e execução-base verde; execução efetiva: **[não verificado]**. Os pontos de observação são T1/T2/T3/T4 acima. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:181`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:188`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:207`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:222`.

| Mutante | T1 ramo | T2 MORDE ramo | T3 caso 2 | T4 MORDE I20 |
|---|---:|---:|---:|---:|
| F | Verde | Verde | Verde | Verde | [nao verificado]
| P | Verde | Verde | Vermelho | Vermelho | [nao verificado]
| F + P | Vermelho | Verde | Vermelho | Vermelho | [nao verificado]
| Z | Verde | Verde | Vermelho | Vermelho | [nao verificado]
| C | Verde | Verde | Verde | Vermelho | [nao verificado]
| R | Verde | Verde | Verde | Verde | [nao verificado]

A redundância explica F e P: sem F, a porta aplica provisoriamente o carimbo e `respondida_em`, consulta C6 e desfaz; sem P, T1 continua pulando antes da porta. T3 alcança a porta e denuncia a remoção de sua recusa. — `chamados/services/disputa_emissao.py:1626`; `chamados/models.py:2555`, `chamados/models.py:2565`, `chamados/models.py:2569`; `chamados/tests/test_o261e_f2_ramo_e_i20.py:214`.

R sobrevive porque não existe controle negativo com texto **e** `respondida_em` preenchido; a fixture oferece apenas o caso positivo do meio-estado. Portanto os testes não prendem integralmente o predicado do I20. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:143`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:214`; `chamados/management/commands/auditar_invariantes_chamados.py:1102`.

**2. Quem escreve e quem lê**

| Campo | Escrita de produção | Leituras de produção relevantes |
|---|---|---|
| `PerguntaDisputa.resposta_colab` | Resposta pelo core, gravação automática e limpeza na reabertura. — `chamados/services/disputa_emissao.py:1247`, `chamados/services/disputa_emissao.py:1393`, `chamados/services/disputa_emissao.py:1358` | C6, respostas sem veredito, materializador e I20. — `chamados/juizes.py:1047`; `chamados/models.py:1767`; `chamados/services/materializacao.py:763`; `chamados/management/commands/auditar_invariantes_chamados.py:1101` |
| `PerguntaDisputa.respondida_em` | Escritores do par acima; recuperação pela trilha; `carimbar` quando solicitado e com texto; limpeza na ressuscitação. — `chamados/services/disputa_emissao.py:1408`; `chamados/models.py:2556`, `chamados/models.py:1488` | C6, `.estado`, respostas sem veredito e I20. — `chamados/juizes.py:1047`; `chamados/models.py:2477`, `chamados/models.py:1769`; `chamados/management/commands/auditar_invariantes_chamados.py:1102` |
| `PerguntaDisputa.materializada_em` | `_escrever_carimbo`, persistido pelas portas `carimbar`, `carimbar_identidade`, `descarimbar` e `descarimbar_em_lote`. — `chamados/models.py:2527`, `chamados/models.py:2533`, `chamados/models.py:2588`, `chamados/models.py:2609`, `chamados/models.py:2619` | C6, seleção do fecho, respostas sem veredito e `.estado`. — `chamados/juizes.py:1071`, `chamados/juizes.py:1077`; `chamados/services/disputa_emissao.py:1620`; `chamados/models.py:1768`, `chamados/models.py:2480` |
| `PerguntaDisputa.via_resolucao` | Portas do carimbo; `encerrar_por_fato_no_espelho` possui operação explícita que troca apenas a via. — `chamados/models.py:2527`, `chamados/models.py:2629` | C6 e seleção/trilha do fecho. — `chamados/juizes.py:1042`, `chamados/juizes.py:1071`; `chamados/services/disputa_emissao.py:1628` |
| `CelulaDia.veredito` | `CelulaDia.lavrar_veredito` delega à porta da célula. — `escala/models.py:1787`; `ponto/portas/celula.py:770` | C6 e retenção da folha, que fornece o universo do vigia. — `chamados/juizes.py:1081`; `folha/export.py:1215`; `chamados/services/vigia_fio.py:545` |
| `CelulaDia.veredito_via` | Mesma porta de lavra. — `ponto/portas/celula.py:771` | C6 distingue decisão do admin; a própria lavra preserva sua precedência. — `chamados/juizes.py:1045`; `ponto/portas/celula.py:738` |
| `CelulaDia.ata` | Porta de lavra; chave `precedencia` por `carimbar_precedencia`. — `ponto/portas/celula.py:768`, `ponto/portas/celula.py:811` | Fato do marco, concordância com a luz e marcos pendentes usados pelo C6. — `chamados/juizes.py:887`, `chamados/juizes.py:1008`, `chamados/juizes.py:1094` |
| `DisputaSupervisao.fechada_em` | `fechar`, `reabrir_admin`, `reabrir_sistema`. — `chamados/models.py:1845`, `chamados/models.py:2047`, `chamados/models.py:2071` | Seleção/fechamento do serviço, badge e signal de materialização. — `chamados/services/disputa_emissao.py:1608`, `chamados/services/disputa_emissao.py:1658`; `chamados/models.py:1721`; `chamados/signals.py:130` |
| `ChamadoColaborador.status_local` | Transições por `_gravar_estado`; nascimento por `abrir`/`criar`. — `chamados/models.py:195`, `chamados/models.py:222`, `chamados/models.py:897`, `chamados/models.py:1046` | Credores do fecho, vigia e signal de rejulgamento da célula. — `chamados/services/disputa_emissao.py:1613`, `chamados/services/disputa_emissao.py:1616`; `chamados/services/vigia_fio.py:569`, `chamados/services/vigia_fio.py:578`; `chamados/signals.py:169` |

Há três distinções importantes na montagem:

- **Meio-estado injetado:** o teste escreve só `resposta_colab` por `QuerySet.update`; o core normal grava texto e instante juntos, e a reabertura limpa ambos. Isso é uma entrada adversarial para o tripwire, mas não prova um caminho atual que a produza. O auditor documenta cicatrizes históricas; ocorrência atual em produção: **[não verificado]**. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:143`; `chamados/services/disputa_emissao.py:1247`, `chamados/services/disputa_emissao.py:1358`; `chamados/management/commands/auditar_invariantes_chamados.py:1089`.

- **Estado exato inválido no caso 2:** `_cenario('ok', ACESA)` acaba gravando `veredito='ok', veredito_via='cartorio'` diretamente. `ok` não pertence a `VEREDITOS_CARTORIO`; a porta lança `ValueError`. A lavra sem códigos produz `concorde`. Portanto esse estado exato **não sai da porta canônica**. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:209`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:152`; `escala/models.py:1719`, `escala/models.py:1596`; `ponto/portas/celula.py:658`.

- **Resultado da célula sobrescrito:** `_fechar` chama produção e depois restaura veredito/ata por `update`, sem passar pela lavra que coordena instante, protestos, status e evento. O vigia mede essa célula imposta pelo teste, sem comprovar o resultado integral deixado pela função de produção. O chamado também nasce já resolvido, sem testar sua transição; não há asserção direta posterior de `status_local` ou `fechada_em`. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:135`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:152`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:157`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:185`; `ponto/portas/celula.py:770`.

**3. Testemunha e universo do auditor**

O `mock.patch` cobre somente `_fechar`. Já terminou durante o `refresh_from_db`, a asserção de materialização e o `assertRaises`. Contudo, `_assert_ramo` encontra o carimbo e falha em sua primeira asserção, antes do C6 real e antes de `censo_alcance`. **Nenhuma leitura do vigia acontece nesse MORDE, sob ou depois do patch.** — `chamados/tests/test_o261e_f2_ramo_e_i20.py:193`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:195`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:171`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:174`.

A cópia textual executa a mesma implementação de `Command.handle`, com os mesmos defaults relevantes: `amostra=5`, `html=False`. O conjunto candidato do I20 é global: perguntas com instante nulo e texto diferente de `''`, sem filtro por empresa, data, disputa ou materialização. **O conjunto final difere intencionalmente pela exclusão adicional de um PK.** `add` conta tudo e limita apenas a amostra. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:90`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:228`; `chamados/management/commands/auditar_invariantes_chamados.py:549`, `chamados/management/commands/auditar_invariantes_chamados.py:559`, `chamados/management/commands/auditar_invariantes_chamados.py:1101`.

A cópia chama `handle` diretamente; o controle chama `call_command`. Logo, a consulta I20 é comparável, mas a cópia não exercita toda a infraestrutura de despacho do comando. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:85`.

**4. Relógio, empresas, PK e formato**

- **Relógio:** há datas literais, `hoje` explícito para o vigia e `freeze_time` na classe. Fecho e lavra leem `now`; I20 não tem predicado temporal. Não há evidência suficiente para acusar um vazamento concreto de relógio. Cobertura efetiva do `setUpTestData` e de todas as dependências pelo decorador: **[não verificado]**. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:43`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:96`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:163`; `chamados/services/disputa_emissao.py:1623`; `ponto/portas/celula.py:772`; `chamados/management/commands/auditar_invariantes_chamados.py:1101`.

- **`empresas_visiveis`: sim, depende.** Passar `[self.emp]` faz interseção com as visíveis; não substitui essa fonte. A empresa criada satisfaz o filtro atual de ativa e CNPJ fora do prefixo ocultado. O carregador da lista repete a interseção. — `chamados/services/vigia_fio.py:535`, `chamados/services/vigia_fio.py:210`; `colaboradores/queries.py:85`; `chamados/tests/test_o261e_f2_ramo_e_i20.py:104`.

- **PK/sequência:** o PK é automático, sem reserva explícita fora da cicatriz. T3 detecta colisão por `assertNotIn`; T4 detecta ao exigir que o comando real conte no final. Assim, colisão pode causar vermelho sem regressão semântica; não produz o verde esperado silenciosamente. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:141`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:215`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:245`; `chamados/management/commands/auditar_invariantes_chamados.py:1098`.

- **Formato:** o offset fixo `68` corresponde ao formato atual. O comando trunca a descrição em 58 caracteres, não a linha nem a amostra de PKs. Linha ausente, duplicada ou `ERR` provoca falha; mudança de layout também pode quebrar o parser. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:69`; `chamados/management/commands/auditar_invariantes_chamados.py:1833`.

- **Identidade fraca:** `assertIn(str(p.pk), ln)` busca substring na linha inteira; não verifica o PK como elemento da amostra e aceita coincidências parciais. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:205`.

**5. Juiz, escritor ou derivação próprios; selos da casa**

Não há predicado novo de vitalidade ou retenção: o teste lê C6, chama o censo real e seleciona sua linha por célula. Os helpers AST e de saída interpretam configuração/protocolo. Há, porém, escrita própria da fixture sobre campos de estado e reposição da célula depois do fecho. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:59`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:69`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:143`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:152`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:163`.

**Não cabe afirmar que os selos estáticos reprovariam esse arquivo:** os censos de escritores e os contratos de par resposta, porta da célula e carimbo excluem testes. As escritas seriam problemáticas como produção, mas essa exclusão impede atribuir-lhes um vermelho automático neste teste. — `core/censo_escritas.py:66`; `chamados/tests/test_contract_par_resposta.py:54`; `ponto/tests/test_selo_veredito_so_pela_porta.py:173`; `chamados/tests/test_o261e_f2_porta_carimbo.py:208`.

A insuficiência sustentada pelas regras da casa é precisa: a mordida denuncia um carimbo, mas não executa a leitura adversarial do vigia; e nenhuma das quatro provas prende isoladamente a guarda do fecho. Isso limita a alegação de proteção da costura, diante da exigência de função real e caso que morde. — `CLAUDE.md:318`; `chamados/tests/test_o261e_f2_ramo_e_i20.py:171`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:198`; `chamados/services/disputa_emissao.py:1626`; `chamados/models.py:2569`.

_1458 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
