# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 20:45** · publicado em **12/11/2026 20:46** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 20:45 · sessao f2b03b2454de · TIPO-NA-FONTE · verificador · origem=Code

1. **BLOQUEANTE — segundo juiz / derivação paralela: uma batida sai ACESA e ÓRFÃ.** A pergunta “que marco previsto a batida ocupa?” pertence a `_match_marcos`. O cartório volta a respondê-la: incorpora órfãs em `_reais`, escolhe uma por proximidade, acende a lâmpada e depois grava a lista original de órfãs. (`app/core/juizes.py:212`; `app/ponto/services/cartorio.py:629`; `app/ponto/services/cartorio.py:664`; `app/ponto/services/cartorio.py:670`; `app/ponto/services/cartorio.py:770`)

   O próprio teste novo fornece o contraexemplo: DNA 23:00→00:30, régua contendo somente E23:00 e órfã em D+1 00:30. A saída acende com esse instante, que permanece em `orfas`, com `n_orfas=1`. O teste verifica apenas a lâmpada acesa, deixando passar a contradição. (`app/escala/tests/test_tipo_na_fonte_orfas_fluxo.py:92`; `app/escala/tests/test_tipo_na_fonte_orfas_fluxo.py:95`; `app/escala/tests/test_tipo_na_fonte_orfas_fluxo.py:98`; `app/ponto/services/cartorio.py:672`; `app/escala/services/ata.py:233`)

   A aproximação já aparece no trecho anterior do diff fornecido; sua comparação histórica literal com `0a747193e` é [nao verificado]. A contradição permanece no ramo reescrito e exercitado pela obra. (`app/ponto/services/cartorio.py:661`; `app/escala/tests/test_tipo_na_fonte_orfas_fluxo.py:91`)

2. **BLOQUEANTE — célula não lavrada passa a disparar corrupção, inclusive em produção sem ci.** `None` levanta “ata não é mapping”; `{}` e somente `precedencia` levantam “chave obrigatória ausente”. Isso ocorre independentemente de `AtaVigiada`: o acessor estrito também atua sobre dict comum. (`app/escala/services/ata.py:42`; `app/escala/services/ata.py:45`; `app/escala/services/ata.py:47`)

   A fronteira reconhece ata ausente como estado legítimo para transportar precedência; os leitores abaixo não distinguem esse estado de uma ata lavrada corrompida. (`app/escala/services/ata.py:83`; `app/escala/services/ata.py:87`; `app/ponto/portas/celula.py:1086`)

   | Chamador de produção | Entrada e efeito |
   |---|---|
   | `grade_da_celula` → `orfas_da_ata` e contagem | Célula inexistente é tratada antes; célula existente dentro da vida funcional chega aos acessores mesmo com `None/{}/precedencia`, levantando. (`app/escala/services/leitor_celula.py:394`; `app/escala/services/leitor_celula.py:396`; `app/escala/services/leitor_celula.py:422`) |
   | `periodos_do_dia` → `orfas_da_ata` | Chamada incondicional, antes das verificações de marcos e lâmpadas; as três formas não lavradas levantam. (`app/ponto/juiz_batida.py:158`; `app/ponto/juiz_batida.py:163`; `app/ponto/juiz_batida.py:176`) |
   | `_leitura_da_celula`, cluster | Dia sem trabalho retorna antes; célula de trabalho não lavrada chega ao acessor e levanta, antes do retorno “não medido”. (`app/ponto/services/cluster_espurio.py:25`; `app/ponto/services/cluster_espurio.py:30`; `app/ponto/services/cluster_espurio.py:31`) |
   | `assinaturas_do_colab`, esmeril | Carrega todas as células do período, sem filtro de lavra; chega ao acessor salvo saída antecipada por `so_escala`. (`app/ponto/services/esmeril_espelho.py:96`; `app/ponto/services/esmeril_espelho.py:102`; `app/ponto/services/esmeril_espelho.py:126`) |
   | `censo`, vigia pareador | Carrega todas as atas da janela ampliada; transforma `None` em `{}` e chama o acessor, levantando. (`app/ponto/services/vigia_pareador_celula.py:71`; `app/ponto/services/vigia_pareador_celula.py:75`; `app/ponto/services/vigia_pareador_celula.py:76`) |
   | `medir_sobra_com_tipo_trocado` | Consulta células de trabalho; com dois marcos, ata não lavrada chega ao acessor e levanta. (`app/ponto/management/commands/tripwire_celulas.py:228`; `app/ponto/management/commands/tripwire_celulas.py:233`; `app/ponto/management/commands/tripwire_celulas.py:235`) |
   | `_celula_furada` → contagem | `None/{}/precedencia` sem `lampadas` retornam `None` antes do acessor. Com lâmpadas, chaves faltantes podem chegar à validação; a exceção vira `None` no `except` amplo. (`app/chamados/juizes.py:493`; `app/chamados/juizes.py:505`; `app/chamados/juizes.py:508`; `app/chamados/juizes.py:511`; `app/chamados/juizes.py:521`) |
   | Plantio → `horas_orfas` | Sem lâmpadas, as formas não lavradas são puladas. Com lâmpadas e chaves ausentes, a exceção é capturada e a resposta vira `False`. (`app/chamados/services/materializacao.py:172`; `app/chamados/services/materializacao.py:175`; `app/chamados/services/materializacao.py:182`; `app/chamados/services/materializacao.py:186`) |
   | `_ata_ate_o_teto` → `copiar_ata` | `None` ou ausência de pendências retorna antes. Mapping vazio/somente precedência, com pendências, seria validado e rejeitado. Os dois chamadores ordinários constroem a ata com `ata_do_dia`. (`app/ponto/services/cartorio.py:918`; `app/ponto/services/cartorio.py:927`; `app/ponto/services/cartorio.py:1444`; `app/ponto/services/cartorio.py:1859`) |
   | `insumos_da_ata` → `contar_orfas` | Valida incondicionalmente, inclusive quando existem lâmpadas. Cartório fornece atas construídas; backfill e vigia filtram `julgada_em`. (`app/ponto/services/cartorio.py:841`; `app/ponto/services/cartorio.py:1880`; `app/ponto/management/commands/backfill_b5_insumos.py:61`; `app/ponto/management/commands/backfill_b5_insumos.py:73`; `app/ponto/management/commands/vigia_b5_ausencia.py:55`; `app/ponto/management/commands/vigia_b5_ausencia.py:79`) |

   No ramo `nunca_bateu`, `_ata_nb` pode ser construído como `None` quando falta `_dia_nb`; existe chamada posterior a `insumos_da_ata`. A ocorrência real dessa ausência de grade não está demonstrada pela sonda: [nao verificado]. (`app/ponto/services/cartorio.py:1247`; `app/ponto/services/cartorio.py:1261`; `app/ponto/services/cartorio.py:1321`)

   **Exceções engolidas também alteram o resultado:** turnos exclui a célula da alimentação dos papéis; motor registra e ignora o dia; espelho captura a falha da grade e segue com o mapa vazio; calendário captura a falha; inteligência exclui o colaborador da métrica; materialização devolve régua vazia. (`app/ponto/turnos.py:1633`; `app/ponto/turnos.py:1634`; `app/ponto/motor_calculo_v2.py:616`; `app/ponto/motor_calculo_v2.py:621`; `app/ponto/services/espelho.py:569`; `app/ponto/services/espelho.py:571`; `app/colaboradores/services/calendario.py:399`; `app/inteligencia/calculadores.py:43`; `app/inteligencia/calculadores.py:46`; `app/chamados/services/materializacao.py:237`; `app/chamados/services/materializacao.py:241`)

   Há ainda chamadas sem captura local em cadastro da realidade e na avaliação da pausa para exportação. (`app/escala/services/cadastro_realidade.py:261`; `app/escala/services/cadastro_realidade.py:266`; `app/folha/export.py:1150`)

   Portanto, `c=0` nas **lavradas** não cobre o universo das não lavradas informado no prompt. Os números da sonda não possuem arquivo correspondente aberto nesta cópia: [nao verificado em arquivo].

3. **BLOQUEANTE — ainda existem transportes crus de produção; o resíduo da precedência foi corrigido.** Permanecem `dict(ata)` em `celula_como_o_julgamento_deixa` e `lavrar_veredito`. Recebendo `AtaVigiada` com as chaves migradas, essas cópias atravessam a iteração fiscalizada e levantam. (`app/ponto/portas/celula.py:945`; `app/ponto/portas/celula.py:995`; `app/escala/services/ata.py:192`; `app/escala/services/ata.py:195`)

   O script operacional `snapshot_o118_col221.py` também lê `ata.orfas` cru; sua execução atual em produção é [nao verificado]. (`bin/snapshot_o118_col221.py:18`; `bin/snapshot_o118_col221.py:32`)

   Já `carimbar_precedencia` usa agora `copiar_envelope`; o resíduo citado pelo relato em torno de `:1083` não corresponde mais ao código atual. Esse transporte aceita `None`, conserva a catraca e não fabrica `n_orfas=0`. (`app/ponto/portas/celula.py:1085`; `app/ponto/portas/celula.py:1086`; `app/escala/services/ata.py:87`; `app/escala/services/ata.py:90`)

4. **Censo de escritores e leitores das portas, incluindo sítios fora do diff.** Os escritores de formato JSON encontrados são as duas atribuições em `gravar_orfas`; cartório e grade sintética chamam esse escritor. A persistência do envelope continua na porta da célula. (`app/escala/services/ata.py:233`; `app/escala/services/ata.py:234`; `app/ponto/services/cartorio.py:770`; `app/escala/utils.py:1903`; `app/ponto/portas/celula.py:1006`; `app/ponto/portas/celula.py:1033`)

   | Campo/valor | Origem e leitores encontrados |
   |---|---|
   | `ata.orfas`, `ata.n_orfas` | Escritor: `gravar_orfas`; leitor semântico central: `_valores_orfas`. Cópias/leitor cru: achado 3. (`app/escala/services/ata.py:233`; `app/escala/services/ata.py:234`; `app/escala/services/ata.py:45`) |
   | `ata.precedencia` | Conteúdo vem de `fatos_do_dia`, preparado pelo cartório; atribuição em `carimbar_precedencia`; comparação para idempotência e preservação dinâmica na lavra. (`app/ponto/services/cartorio.py:2007`; `app/ponto/services/cartorio.py:2008`; `app/ponto/portas/celula.py:1087`; `app/ponto/portas/celula.py:1081`; `app/ponto/portas/celula.py:104`; `app/ponto/portas/celula.py:997`) |
   | Retorno de `leitura_orfas` | Produzido nas saídas legado/ISO; lido por `ler_orfas`, `contar_orfas`, `copiar_ata` e `horas_orfas`. `medida` tem default `True`. (`app/escala/services/ata.py:37`; `app/escala/services/ata.py:58`; `app/escala/services/ata.py:70`; `app/escala/services/ata.py:74`; `app/escala/services/ata.py:78`; `app/escala/services/ata.py:101`; `app/escala/services/ata.py:129`) |
   | Retorno de `ler_orfas` | Consumido por `orfas_da_ata`. (`app/escala/services/ata.py:74`; `app/escala/services/leitor_celula.py:186`) |
   | Retorno de `contar_orfas` | Consumido por `contagem_orfas_da_ata` e `insumos_da_ata`. (`app/escala/services/ata.py:78`; `app/escala/services/leitor_celula.py:192`; `app/ponto/services/cartorio.py:841`) |
   | Retorno de `orfas_da_ata` | Grade, juiz da batida, tripwire, esmeril, vigia pareador e cluster. (`app/escala/services/leitor_celula.py:422`; `app/ponto/juiz_batida.py:163`; `app/ponto/management/commands/tripwire_celulas.py:235`; `app/ponto/services/esmeril_espelho.py:126`; `app/ponto/services/vigia_pareador_celula.py:76`; `app/ponto/services/cluster_espurio.py:30`) |
   | Retorno de `contagem_orfas_da_ata` | Grade e `_celula_furada`. (`app/escala/services/leitor_celula.py:422`; `app/chamados/juizes.py:511`) |
   | Retorno de `contar_orfas_da_grade` | Furos, causas, status, supra-juiz, bordas, score de par relâmpago e cartório. O nome `contar_orfas` no supra é alias desta porta da grade. (`app/escala/services/furos_diarios.py:23`; `app/folha/causas.py:44`; `app/ponto/status_dia.py:189`; `app/ponto/supra_juiz.py:379`; `app/ponto/supra_juiz.py:380`; `app/ponto/services/bordas_realizado.py:50`; `app/ponto/management/commands/detectar_par_relampago.py:141`; `app/ponto/services/cartorio.py:820`) |
   | Retorno de `horas_orfas` | Legado copiado como HH:MM; ISO convertido ao fuso local; consumidor de produção: plantio. (`app/escala/services/ata.py:131`; `app/escala/services/ata.py:133`; `app/chamados/services/materializacao.py:182`) |
   | Retorno de `copiar_ata` | `_ata_ate_o_teto`; delega o transporte a `copiar_envelope` depois de validar. (`app/ponto/services/cartorio.py:927`; `app/escala/services/ata.py:101`; `app/escala/services/ata.py:102`) |
   | Retorno de `copiar_envelope` | `copiar_ata` e `carimbar_precedencia`. (`app/escala/services/ata.py:102`; `app/ponto/portas/celula.py:1086`) |
   | `AtaVigiada` | Produzida pelo descriptor e pelas cópias; reconhecida pelo preparo JSON; leitura/escrita fiscalizadas pelos métodos próprios. (`app/escala/services/ata_ci.py:22`; `app/escala/services/ata_ci.py:28`; `app/escala/services/ata.py:90`; `app/escala/services/ata.py:213`; `app/escala/services/ata_ci.py:43`; `app/escala/services/ata_ci.py:44`; `app/escala/services/ata.py:180`; `app/escala/services/ata.py:184`) |

   A projeção `dia.orfas` tem escritores próprios nos builders, overlays e leitor da ata; não é o JSON persistido. Ela é consumida pelo calendário, propositor, cobertura parcial, adaptador de realizado e PDF. (`app/escala/utils.py:622`; `app/escala/utils.py:663`; `app/escala/utils.py:800`; `app/escala/utils.py:964`; `app/escala/utils.py:1157`; `app/escala/utils.py:1816`; `app/escala/utils.py:1839`; `app/escala/services/leitor_celula.py:422`; `app/colaboradores/services/calendario.py:380`; `app/ponto/propositor.py:36`; `app/ponto/propositor.py:81`; `app/ponto/turnos.py:2667`; `app/escala/utils.py:1114`; `app/relatorios/pdf_espelho.py:203`)

   Também existem escritores/leitores diretos em fixtures e testes, inclusive fora dos dirigidos: calendário, juiz da batida, T4.1, O118 e lâmpadas. Eles não desapareceram com o acessor. (`app/colaboradores/tests/test_calendario_orfa.py:79`; `app/ponto/tests/test_juiz_batida.py:188`; `app/escala/tests/test_t41_leitor_celula.py:88`; `app/ponto/tests/test_o118_orfa_que_acendeu.py:90`; `app/ponto/tests/test_lampada_acende_pela_batida.py:84`)

   Os consumidores adicionais das portas nos testes incluem as cinco assertivas migradas e bateria, provas da matriz, fluxo, cópia e ativação. (`app/escala/tests/test_ata_x_turno_entrada.py:144`; `app/escala/tests/test_saida_valida_cross_meia_noite.py:104`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:137`; `app/ponto/tests/test_bateria_lavratura.py:639`; `app/ponto/tests/test_bateria_lavratura.py:1060`; `app/ponto/tests/test_bateria_lavratura.py:544`; `app/ponto/tests/test_bateria_lavratura.py:963`; `app/escala/tests/test_tipo_na_fonte_orfas.py:34`; `app/escala/tests/test_tipo_na_fonte_orfas_fluxo.py:74`; `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:45`; `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:46`; `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:127`; `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:131`; `app/escala/tests/test_ata_ci.py:64`)

   Completude de aliases dinâmicos, acesso indireto por serialização e todas as fixtures do archive: [nao verificado].

5. **Mudanças efetivas para o legado HH:MM, mesmo com contagem coerente.** HH:MM não vazio produz `instantes=None`, preservando quantidade; vazio explícito produz `[]` e zero. Não há exceção nessas duas formas válidas. (`app/escala/services/ata.py:56`; `app/escala/services/ata.py:58`; `app/escala/services/ata.py:70`)

   | Consumidor | Resultado para HH:MM não vazio |
   |---|---|
   | Cluster | Retorna não medido, sem procurar rajada legada. (`app/ponto/services/cluster_espurio.py:30`; `app/ponto/services/cluster_espurio.py:31`) |
   | Esmeril A5 | Não acrescenta essas órfãs aos buckets. (`app/ponto/services/esmeril_espelho.py:126`; `app/ponto/services/esmeril_espelho.py:127`; `app/ponto/services/esmeril_espelho.py:131`) |
   | Tripwire | Pula a célula antes de incrementar o universo. (`app/ponto/management/commands/tripwire_celulas.py:235`; `app/ponto/management/commands/tripwire_celulas.py:236`; `app/ponto/management/commands/tripwire_celulas.py:238`) |
   | Vigia pareador | Não conta a célula como tendo órfã nem inclui o colaborador por essa informação. (`app/ponto/services/vigia_pareador_celula.py:77`; `app/ponto/services/vigia_pareador_celula.py:79`; `app/ponto/services/vigia_pareador_celula.py:82`) |
   | Propositor/classificador | Propositor retorna não avaliável; classificador retorna `None`. (`app/ponto/propositor.py:36`; `app/ponto/propositor.py:37`; `app/escala/orfa_classe.py:54`; `app/escala/orfa_classe.py:56`) |
   | Ausência parcial | A leitura temporal inclui desconhecimento, `[None]`. (`app/ponto/turnos.py:2667`; `app/ponto/turnos.py:2669`) |
   | PDF/calendário | Batidas podem receber `orfa=None`; o PDF não incrementa abertos quando a classificação é desconhecida. (`app/relatorios/pdf_espelho.py:185`; `app/relatorios/pdf_espelho.py:214`; `app/relatorios/pdf_espelho.py:215`; `app/colaboradores/services/calendario.py:624`; `app/colaboradores/services/calendario.py:628`) |

   As portas escalares preservam a quantidade legada; o plantio preserva suas horas HH:MM pelo adaptador. Essas mudanças temporais decorrem do contrato implementado, mas seus totais reais por consumidor não constam da sonda: [nao verificado]. (`app/escala/services/ata.py:78`; `app/escala/services/ata.py:118`; `app/escala/services/ata.py:131`)

6. **CI: a suite canônica usa ci; a catraca não fica desligada por escolha de settings.** `bin/suite.sh` executa explicitamente `--settings=config.settings.ci`; esse perfil ativa a flag e seleciona `RunnerAtaVigiada`. (`bin/suite.sh:232`; `bin/suite.sh:233`; `app/config/settings/ci.py:40`; `app/config/settings/ci.py:42`)

   | Execução/prova | Settings e alcance |
   |---|---|
   | Suite canônica, inclusive testes Django/selos | `config.settings.ci`. (`bin/suite.sh:233`) |
   | Dirigidos puros por `python -m unittest` | O carregador AST executa funções sem inicializar settings Django. (`app/escala/tests/test_tipo_na_fonte_orfas_fluxo.py:19`; `app/escala/tests/test_tipo_na_fonte_orfas_fluxo.py:28`) |
   | Prova local de `ready` | `django.conf` falso, com flag definida pelo teste. (`app/escala/tests/test_ata_ci.py:52`; `app/escala/tests/test_ata_ci.py:55`) |
   | Prova da registry real | Usa settings já ativos, exige a flag e pula quando Django/registry não está disponível. (`app/escala/tests/test_ata_ci.py:129`; `app/escala/tests/test_ata_ci.py:132`; `app/escala/tests/test_ata_ci.py:135`) |
   | Prova do runner fora do caso | Executa a classe real com runner pai simulado. (`app/escala/tests/test_ata_ci.py:109`; `app/escala/tests/test_ata_ci.py:119`) |

   A instalação depende da flag em `ready`. Prod importa base, SaaS importa base, sombra importa SaaS; esses caminhos não importam ci. Não há ativação normal pelo código desses perfis. (`app/escala/apps.py:11`; `app/config/settings/prod.py:1`; `app/config/settings/saas.py:10`; `app/config/settings/sombra.py:27`)

   Entretanto, depois de instalada, a instrumentação modifica a classe do modelo, o field e `unittest.TestCase.run` sem desinstalação. Trocar settings no **mesmo processo** não desfaz isso; ocorrência real dessa troca é [nao verificado]. (`app/escala/services/ata_ci.py:37`; `app/escala/services/ata_ci.py:47`; `app/escala/services/ata_ci.py:72`)

7. **BLOQUEANTE — teste novo de mutação está vermelho na árvore atual.** `test_MORDE_mutante_copia_perde_catraca` altera texto de `copiar_envelope`, mas extrai somente `copiar_ata`. A função extraída chama o `copiar_envelope` original do namespace; a mutação não alcança a operação executada e o `assertRaises` externo falha. (`app/escala/tests/test_selo_tipo_na_fonte_orfas.py:143`; `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:146`; `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:149`; `app/escala/tests/test_tipo_na_fonte_orfas_fluxo.py:24`; `app/escala/services/ata.py:102`)

   A rodada local dos dez módulos puros terminou em **74 testes, uma falha e um skip**; a falha foi essa. Registro persistido da execução: [nao verificado em arquivo]. O skip previsto depende da disponibilidade de Django/registry. (`app/escala/tests/test_ata_ci.py:129`; `app/escala/tests/test_ata_ci.py:132`)

   Além disso, fixtures existentes seguem incompatíveis: T4.1 lavra `n_orfas=2` sem `orfas` e espera `None`; calendário escreve ISO sem `n_orfas`. O novo acessor rejeita ambas. Execução Django desses cenários: [nao verificado]. (`app/escala/tests/test_t41_leitor_celula.py:90`; `app/escala/tests/test_t41_leitor_celula.py:94`; `app/colaboradores/tests/test_calendario_orfa.py:81`; `app/escala/services/ata.py:45`; `app/escala/services/ata.py:47`)

8. **BLOQUEANTE — selo anterior perdeu mordida pela troca de formato.** O selo O118 ainda intersecta `lampada.luz` HH:MM com `ata.orfas` agora ISO. Assim, a mesma batida acesa e órfã pode produzir interseção vazia. A ata usada nesse selo vem diretamente de `ata_do_dia`, como dict comum, sem passar pelo descriptor ORM. (`app/ponto/tests/test_o118_orfa_que_acendeu.py:84`; `app/ponto/tests/test_o118_orfa_que_acendeu.py:89`; `app/ponto/tests/test_o118_orfa_que_acendeu.py:90`; `app/ponto/tests/test_o118_orfa_que

[truncado em 20000 de 24670 caracteres]

## 12/11 20:41 · sessao 4eab9461e94e · - · - · origem=Ronald (tmux)

Confirmado com execução pura do cenário já existente em `app/escala/tests/test_tipo_na_fonte_orfas_fluxo.py:91`: a saída fica acesa e o mesmo instante permanece órfão; o teste verifica somente que acendeu em `:98`.

**Bloqueante — segundo juiz funcional / derivação paralela.** O cartório incorpora a órfã em `_reais` (`app/ponto/services/cartorio.py:629`), atribui-a ao marco por proximidade (`:664`, `:670`, `:672`) e grava a lista original de órfãs (`:770`). A pergunta “que marco previsto a batida ocupa?” já pertence a `_match_marcos` (`app/core/juizes.py:212`; `app/docs/JUIZES.md:27`).

O ramo foi reescrito pelo diff fornecido e o novo teste aceita a contradição (`app/escala/tests/test_tipo_na_fonte_orfas_fluxo.py:98`). A aproximação já aparecia no ramo anterior mostrado no anexo; comparação literal com a base nesta cópia: [nao verificado].

## 12/11 20:40 · sessao 4eab9461e94e · - · - · origem=Ronald (tmux)

Censo estático consolidado:

| Valor | Origem/escritor | Leitores/transportadores de produção |
|---|---|---|
| JSON `orfas` e `n_orfas` | `app/escala/services/ata.py:233` e `:234`; chamado por `app/ponto/services/cartorio.py:770` e `app/escala/utils.py:1903` | Leitura semântica central `app/escala/services/ata.py:45`; persistência/cópias integrais `app/ponto/portas/celula.py:945`, `:995`, `:1006`, `:1033`; leitor cru operacional `bin/snapshot_o118_col221.py:32`. |
| `precedencia` | Conteúdo vem de `fatos_do_dia`, em `app/ponto/services/cartorio.py:2007`; preparado em `:2008`; escrito em `app/ponto/portas/celula.py:1087` | Comparação em `app/ponto/portas/celula.py:1081`; preservação dinâmica em `:997`, `:998`, `:999`, pela chave declarada em `:104`. Outro leitor de conteúdo por alias: [nao verificado]. |
| Retorno `leitura_orfas` | `app/escala/services/ata.py:55`, `:58`, `:70` | `ler_orfas` em `:74`, `contar_orfas` em `:78`, `copiar_ata` em `:101`, `horas_orfas` em `:129`. |
| Retorno `ler_orfas` | `app/escala/services/ata.py:74` | `app/escala/services/leitor_celula.py:186`. |
| Retorno `contar_orfas` | `app/escala/services/ata.py:78` | `app/escala/services/leitor_celula.py:192`; `app/ponto/services/cartorio.py:841`. |
| Retorno `horas_orfas` | Legado copiado em `app/escala/services/ata.py:131`; ISO convertido para hora local em `:133` | `app/chamados/services/materializacao.py:182`. |
| Retorno `orfas_da_ata` | `app/escala/services/leitor_celula.py:186` | `app/escala/services/leitor_celula.py:422`; `app/ponto/juiz_batida.py:163`; `app/ponto/management/commands/tripwire_celulas.py:235`; `app/ponto/services/esmeril_espelho.py:126`; `app/ponto/services/vigia_pareador_celula.py:76`; `app/ponto/services/cluster_espurio.py:30`. |
| Retorno `contagem_orfas_da_ata` | `app/escala/services/leitor_celula.py:192` | `app/escala/services/leitor_celula.py:422`; `app/chamados/juizes.py:511`. |
| Retorno `contar_orfas_da_grade` | `app/escala/services/ata.py:118`, `:124` | `app/escala/services/furos_diarios.py:23`; `app/folha/causas.py:44`; `app/ponto/status_dia.py:189`; `app/ponto/supra_juiz.py:380` por alias definido em `:379`; `app/ponto/services/bordas_realizado.py:50`; `app/ponto/management/commands/detectar_par_relampago.py:141`; `app/ponto/services/cartorio.py:820`. |
| `copiar_ata` | `app/escala/services/ata.py:99` | `app/ponto/services/cartorio.py:927`. |
| `copiar_envelope` | `app/escala/services/ata.py:81` | `app/escala/services/ata.py:102`; `app/ponto/portas/celula.py:1086`. |
| `AtaVigiada` | Construção automática em `app/escala/services/ata_ci.py:22`, `:28`; cópias em `app/escala/services/ata.py:90`, `:210`, `:213` | Reconhecimento em `app/escala/services/ata_ci.py:21`, `:27`, `:43`; transporte JSON interno em `:44`; fiscalização em `app/escala/services/ata.py:162`; métodos de leitura/escrita em `:180`, `:184`, `:188`, `:192`. |

Achados relevantes:

1. **Leitura crua residual.** `bin/snapshot_o118_col221.py:32` lê `ata.orfas` diretamente; a ata foi obtida do modelo em `:18`. É script operacional que escreve snapshot em `:30`; execução atual e uso em produção: [nao verificado].

2. **Cópias cruas residuais.** `celula_como_o_julgamento_deixa` mantém `dict(ata)` em `app/ponto/portas/celula.py:945`; `lavrar_veredito` mantém outro em `:995`. Ambos atravessam a iteração proibida de `AtaVigiada`, definida em `app/escala/services/ata.py:192`. O chamador do primeiro é `app/chamados/juizes.py:424`, alimentado pelo estado corrente do cartório em `:422`. A passagem efetiva de wrapper por cada ramo na suíte completa: [nao verificado].

3. **O resíduo de precedência mencionado no relato foi corrigido na cópia atual.** `app/ponto/portas/celula.py:1086` usa `copiar_envelope`; `None` vira somente envelope vazio em `app/escala/services/ata.py:87`, sem produzir contagem zero. A validação de órfãs continua exclusiva de `copiar_ata`, em `:101`.

4. **Dia civil versus dia do turno permanece divergente.** A grade guarda as órfãs do turno em `app/escala/utils.py:1763` e as projeta em `:1816`; as sobras são distribuídas civilmente em `:1837` e `:1839`. O vigia declara o entendimento civil em `app/ponto/services/vigia_pareador_celula.py:67` e procura pela data civil em `:112`. A preexistência literal frente a `0a747193e` não pôde ser conferida nesta cópia sem `.git`: [nao verificado].

5. **Plantio foi tocado, mas mantém comparação HH:MM.** O código atual introduz o adaptador em `app/chamados/services/materializacao.py:179` e o consome em `:182`; slots e instante plantado continuam reduzidos a HH:MM em `:180` e `:183`. O diff fornecido mostra essa alteração; igualdade literal com o HEAD proibido de alterar: [nao verificado]. `dna.lampadas[].instante` continua sendo produzido sem fuso em `app/ponto/management/commands/gerar_celulas.py:69`.

6. **Não identifiquei juiz novo duplicando autoridade registrada nas novas portas de ata.** O registro conserva a propriedade do turno em `app/core/juizes.py:208` e do casamento em `:212`; a tabela os aponta em `app/docs/JUIZES.md:20` e `:27`. O encadeamento dos marcos usado pelo cartório delega à função existente em `app/ponto/services/cartorio.py:623`. Completude dinâmica dessa conclusão: [nao verificado].

7. **Contrato da grade menos estrito que sua documentação.** `contar_orfas_da_grade` documenta lista de datetimes em `app/escala/services/ata.py:108`, mas valida somente lista e contagem em `:119` e `:122`; não verifica datetime nem fuso dos elementos. Um JSON comum passado por engano pode ser aceito; o teste só prova rejeição quando o argumento é `AtaVigiada`, em `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:124`.

8. **Docstring de retorno desatualizada.** `classificar_orfas` ainda promete chaves HH:MM em `app/escala/orfa_classe.py:50`, mas retorna chaves datetime em `:64`, `:69`, `:75`, ou `None` em `:56`. Chamador de produção dessa função: [nao verificado]; o uso localizado em `app/ponto/turnos.py:1581` importa somente a constante.

Os retornos projetados são consumidos como instantes/`None` pelo calendário em `app/colaboradores/services/calendario.py:380` e `:623`, propositor em `app/ponto/propositor.py:36` e `:81`, cobertura parcial em `app/ponto/turnos.py:2667`, adaptador da grade em `app/escala/utils.py:1089` e `:1114`, PDF em `app/relatorios/pdf_espelho.py:183`, `:203`, `:218`, e template em `app/templates/ponto/espelho.html:247`.

Testes que consomem diretamente as portas, incluindo os existentes alterados: [nao verificado]

- `leitura_orfas`: `app/escala/tests/test_tipo_na_fonte_orfas.py:34`, `:36`.
- `ler_orfas`: `app/escala/tests/test_ata_ci.py:65`, `:71`, `:97`, `:139`; `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:59`, `:74`, `:87`, `:97`, `:120`, `:132`; `app/escala/tests/test_preverif_v3_asserts.py:74`, `:94`; `app/escala/tests/test_ata_x_turno_entrada.py:144`; `app/escala/tests/test_saida_valida_cross_meia_noite.py:104`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:137`; `app/ponto/tests/test_bateria_lavratura.py:639`, `:963`, `:1060`.
- `contar_orfas`: `app/escala/tests/test_ata_ci.py:64`, `:82`; `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:60`; `app/escala/tests/test_preverif_v3_asserts.py:75`; `app/escala/tests/test_tipo_item3_cartorio.py:72`; `app/ponto/tests/test_bateria_lavratura.py:544`, `:554`, `:648`, `:800`, `:952`.
- `orfas_da_ata`: `app/escala/tests/test_tipo_na_fonte_orfas.py:14`, `:53`; `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:45`; `app/escala/tests/test_tipo_na_fonte_orfas_fluxo.py:74`, `:89`, `:121`, `:133`; observação com wrapper em `app/escala/tests/test_tipo_na_fonte_orfas_v2.py:181`.
- `contagem_orfas_da_ata`: `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:46`.
- `contar_orfas_da_grade`: `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:127`.
- `copiar_ata`: `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:131`, `:137`, `:148`.
- `gravar_orfas`: `app/escala/tests/test_tipo_na_fonte_orfas.py:65`, `:70`; `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:58`; `app/escala/tests/test_preverif_v3_asserts.py:60`, `:63`, `:106`, `:112`, `:123`, `:130`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:187`, `:337`; `app/escala/tests/test_tipo_na_fonte_orfas_fluxo.py:131`.
- `horas_orfas`: exercitado indiretamente pelo plantio em `app/chamados/tests/test_tipo_na_fonte_plantio_expand.py:74`, `:79`, `:85`.
- `copiar_envelope`: teste direto específico: [nao verificado]; o teste de cópia atual passa por `copiar_ata` em `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:131`, que delega em `app/escala/services/ata.py:102`.

## 12/11 20:39 · sessao 962f0e17ae3b · - · - · origem=Ronald (tmux)

Achado bloqueante: **ata ainda não lavrada passa a ser corrupção**, em caminhos sem filtro de lavra. `None` falha em `app/escala/services/ata.py:43`; `{}` e só `precedencia` falham em `app/escala/services/ata.py:47`. O transporte de precedência aceita explicitamente esses envelopes, portanto a própria fronteira reconhece sua legitimidade antes da lavra (`app/escala/services/ata.py:81`, `app/ponto/portas/celula.py:1086`).

Matriz dos chamadores de produção:

| Porta e chamador | Entrada possível e resultado |
|---|---|
| `leitura_orfas` ← `ler_orfas` | Chamada sem guarda; valida mapping e ambas as chaves (`app/escala/services/ata.py:74`, `app/escala/services/ata.py:40`). |
| `leitura_orfas` ← `contar_orfas` | Mesmo contrato; HH:MM coerente conserva quantidade (`app/escala/services/ata.py:78`, `app/escala/services/ata.py:57`). |
| `leitura_orfas` ← `copiar_ata` | Valida antes de transportar; `None/{}/precedencia` falham (`app/escala/services/ata.py:101`). |
| `leitura_orfas` ← `horas_orfas` | Legado preserva HH:MM; ISO converte ao fuso local; inválido levanta (`app/escala/services/ata.py:129`). |
| `ler_orfas` ← `orfas_da_ata` | `getattr(..., None)` permite chegar `None`; não há guarda de lavra (`app/escala/services/leitor_celula.py:186`). |
| `contar_orfas` ← `contagem_orfas_da_ata` | Idem (`app/escala/services/leitor_celula.py:192`). |
| `copiar_ata` ← `_ata_ate_o_teto` | Só chama com mapping e pendências; `{}`/precedência seriam rejeitados. Chamadas ordinárias fornecem `ata_do_dia` recém construída (`app/ponto/services/cartorio.py:918`, `app/ponto/services/cartorio.py:927`, `app/ponto/services/cartorio.py:1444`, `app/ponto/services/cartorio.py:1859`). |
| `contar_orfas` ← `insumos_da_ata` | Valida incondicionalmente, mesmo quando lâmpadas seriam suficientes (`app/ponto/services/cartorio.py:841`). Cartório usa atas novas (`app/ponto/services/cartorio.py:1321`, `app/ponto/services/cartorio.py:1880`); backfill e vigia usam persistidas, filtrando `julgada_em` (`app/ponto/management/commands/backfill_b5_insumos.py:61`, `app/ponto/management/commands/backfill_b5_insumos.py:73`, `app/ponto/management/commands/vigia_b5_ausencia.py:55`, `app/ponto/management/commands/vigia_b5_ausencia.py:79`). |
| `contagem_orfas_da_ata` ← `_celula_furada` | `None/{}/precedência` retornam `None` antes do acessor por ausência de `lampadas`; chave faltante numa ata com lâmpadas chega ao acessor salvo apagada anterior; exceção é engolida, retornando `None` (`app/chamados/juizes.py:493`, `app/chamados/juizes.py:505`, `app/chamados/juizes.py:508`, `app/chamados/juizes.py:511`, `app/chamados/juizes.py:521`). |
| `orfas_da_ata` e contagem ← `grade_da_celula` | Célula inexistente não chama; célula existente, inclusive sem lavra, chama ambos sem guarda. `None/{}/precedência` levantam (`app/escala/services/leitor_celula.py:380`, `app/escala/services/leitor_celula.py:396`, `app/escala/services/leitor_celula.py:422`). |
| `orfas_da_ata` ← `periodos_do_dia` | Chamada incondicional antes das verificações do DNA/lâmpadas; `None/{}/precedência` levantam (`app/ponto/juiz_batida.py:158`, `app/ponto/juiz_batida.py:163`, `app/ponto/juiz_batida.py:176`). |
| `orfas_da_ata` ← cluster | Só dia sem trabalho retorna antes; célula de trabalho não lavrada levanta, em vez de devolver não medido (`app/ponto/services/cluster_espurio.py:25`, `app/ponto/services/cluster_espurio.py:30`). |
| `orfas_da_ata` ← esmeril | Consulta todas as células do período; não filtra lavra. Só `so_escala=True` evita essa leitura (`app/ponto/services/esmeril_espelho.py:96`, `app/ponto/services/esmeril_espelho.py:102`, `app/ponto/services/esmeril_espelho.py:126`). |
| `orfas_da_ata` ← vigia pareador | Consulta todas as atas da janela ampliada; transforma `None` em `{}` e chama; não filtra lavra (`app/ponto/services/vigia_pareador_celula.py:71`, `app/ponto/services/vigia_pareador_celula.py:75`, `app/ponto/services/vigia_pareador_celula.py:76`). |
| `orfas_da_ata` ← tripwire | Consulta células `trabalha=True`; apenas menos de dois marcos evita leitura. Ata não lavrada com dois marcos levanta (`app/ponto/management/commands/tripwire_celulas.py:228`, `app/ponto/management/commands/tripwire_celulas.py:233`, `app/ponto/management/commands/tripwire_celulas.py:235`). |
| `horas_orfas` ← plantio | `None/{}/precedência` sem lâmpadas não chega; com lâmpadas e chaves ausentes levanta e `except Exception` devolve `False` (`app/chamados/services/materializacao.py:172`, `app/chamados/services/materializacao.py:175`, `app/chamados/services/materializacao.py:182`, `app/chamados/services/materializacao.py:186`). |

Propagação e mudanças silenciosas relevantes:

- `periodos_do_dia` chega de `turnos`: exceção engolida exclui célula da alimentação de papéis (`app/ponto/turnos.py:1633`, `app/ponto/turnos.py:1634`); chega do motor: exceção registrada e dia ignorado (`app/ponto/motor_calculo_v2.py:616`, `app/ponto/motor_calculo_v2.py:617`); chega do censo de cadastro sem filtro de lavra e sem captura local (`app/escala/services/cadastro_realidade.py:261`, `app/escala/services/cadastro_realidade.py:266`); chega de retenção pela pausa sem captura local (`app/folha/export.py:1150`).
- Uma célula não lavrada dentro de uma janela faz `grade_da_celula` levantar; o espelho captura e **zera a grade do período**, sem cair no builder nesse ramo (`app/ponto/services/espelho.py:519`, `app/ponto/services/espelho.py:569`); o calendário captura e perde o mapa das órfãs (`app/colaboradores/services/calendario.py:334`, `app/colaboradores/services/calendario.py:399`).
- Calculadores de inteligência capturam a falha da grade e excluem o colaborador da métrica (`app/inteligencia/calculadores.py:42`, `app/inteligencia/calculadores.py:43`, `app/inteligencia/calculadores.py:143`, `app/inteligencia/calculadores.py:144`); materialização captura e devolve régua vazia (`app/chamados/services/materializacao.py:235`, `app/chamados/services/materializacao.py:237`).

Para HH:MM válido, as contagens permanecem conhecidas, mas os leitores temporais mudam resultados:

- Toda lista HH:MM não vazia vira `instantes=None` (`app/escala/services/ata.py:57`). Cluster devolve não medido, sem detectar rajada legada (`app/ponto/services/cluster_espurio.py:31`); esmeril deixa de preencher A5 (`app/ponto/services/esmeril_espelho.py:126`); tripwire exclui a célula do universo medido (`app/ponto/management/commands/tripwire_celulas.py:236`); vigia não a inclui em `celulas_com_orfa` nem em `com_orfa` (`app/ponto/services/vigia_pareador_celula.py:77`, `app/ponto/services/vigia_pareador_celula.py:79`, `app/ponto/services/vigia_pareador_celula.py:82`).
- O propositor não avalia o legado projetado (`app/ponto/propositor.py:36`); classificador devolve `None` (`app/escala/orfa_classe.py:54`); horas da ausência parcial passam a incluir desconhecido (`app/ponto/turnos.py:2667`); PDF marca batidas `orfa=None` e não soma turnos abertos cuja classificação é desconhecida (`app/relatorios/pdf_espelho.py:183`, `app/relatorios/pdf_espelho.py:214`).
- Essas mudanças temporais podem decorrer intencionalmente do contrato EXPAND; a perda de quantidade foi evitada nas portas escalares (`app/escala/services/ata.py:105`, `app/ponto/status_dia.py:189`, `app/ponto/services/cartorio.py:820`). O problema bloqueante distinto é aplicar corrupção às células legitimamente não lavradas (`app/escala/services/leitor_celula.py:422`, `app/escala/services/ata.py:43`).

Reprodução local, sem arquivos nem banco: executei accessor, tripwire com doubles existentes e grade completa com carregador de previsto isolado; as três entradas `None`, `{}`, só precedência levantaram `EstadoIlegalDaAta`; HH:MM coerente retornou `None` temporal, e tripwire `( [], 0 )`. As funções reais correspondentes estão em `app/escala/services/ata.py:40`, `app/escala/tests/test_tipo_na_fonte_orfas_v2.py:39`, `app/escala/tests/test_tipo_na_fonte_orfas_v2.py:81`. Registro persistido dessa execução: [nao verificado].

## 12/11 20:38 · sessao 54a4e982ac49 · - · - · origem=Ronald (tmux)

1. **Bloqueante — selo/mutante quebrado na árvore atual.** `test_MORDE_mutante_copia_perde_catraca` modifica a expressão de `copiar_envelope`, mas carrega somente `copiar_ata`; esta chama a implementação original de `copiar_envelope`. Portanto o mutante não atinge a cópia executada e o teste falha em seu `assertRaises` externo. Referências: `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:143`, `:146`, `:149`; `app/escala/services/ata.py:90`, `:102`.

   Executei agregado de dez módulos puros com `PYTHONDONTWRITEBYTECODE=1`: **74 testes, 1 falha, 1 skipped**, falha acima. Registro persistido da execução: [nao verificado]. O skip decorre da indisponibilidade de Django/registry, prevista em `app/escala/tests/test_ata_ci.py:129`.

2. **Bloqueante — selo anterior perdeu mordida pela troca de formato.** O selo O118 ainda intersecta `lampada.luz` HH:MM com as strings ISO de `ata.orfas`: `app/ponto/tests/test_o118_orfa_que_acendeu.py:89`, `:90`, `:92`; escritor ISO: `app/escala/services/ata.py:232`. Reproduzi por extração AST do método real com a mesma identidade ISO na lâmpada acesa e na lista de órfãs: o selo passou. Registro persistido dessa execução: [nao verificado]. A ata usada pelo selo nasce como dict normal de `ata_do_dia`, sem atravessar o descriptor ORM: `app/ponto/tests/test_o118_orfa_que_acendeu.py:84`; a catraca automática não cobre essa leitura.

3. **Bloqueante para alegação de suite verde — fixtures incompatíveis com contrato novo.** [nao verificado]
   - `app/escala/tests/test_t41_leitor_celula.py:88` lavra ata com `n_orfas=2` e sem `orfas`; em `:94` espera `None`. O acessor agora exige ambas as chaves e levanta por ausência: `app/escala/services/ata.py:45`, `:47`.
   - A fixture alterada do calendário escreve ISO sem `n_orfas`: `app/colaboradores/tests/test_calendario_orfa.py:79`, `:81`; mesma rejeição em `app/escala/services/ata.py:45`.
   - Execução Django desses cenários: [nao verificado].

4. **Settings: a hipótese de suite sem ci não procede.** A execução efetiva termina em `manage.py test ... --settings=config.settings.ci`: `bin/suite.sh:232`, `:233`; saída seca também mostra ci: `bin/suite.sh:188`. O perfil ci define o runner novo e ativa a flag: `app/config/settings/ci.py:40`, `:42`. `EscalaConfig.ready` instala descriptor e hook somente quando a flag é verdadeira: `app/escala/apps.py:11`, `:13`, `:14`.

   `prod` importa base, `saas` importa base e `sombra` importa saas: `app/config/settings/prod.py:1`, `app/config/settings/saas.py:10`, `app/config/settings/sombra.py:27`. Não há definição de `VIGIAR_ATA_ORFAS` nesses perfis na busca do archive; ativação normal neles é falsa pelo default explícito em `app/escala/apps.py:11`. Já depois de instalada, a instrumentação modifica a classe e `unittest.TestCase.run` permanentemente naquele processo; não existe desinstalação no código: `app/escala/services/ata_ci.py:37`, `:47`, `:72`. Uso real de troca ci→prod/sombra no mesmo processo: [nao verificado].

5. **Settings de cada tipo de prova.**
   - Selos Django lançados por `bin/suite.sh`: ci, `bin/suite.sh:233`.
   - Testes puros por `python -m unittest`: não inicializam settings no seu carregador AST, `app/escala/tests/test_tipo_na_fonte_orfas_fluxo.py:20`, `:29`.
   - Prova automática local de `ready`: usa `django.conf` falso com a flag escolhida pelo teste, `app/escala/tests/test_ata_ci.py:52`, `:55`.
   - Prova de registry real: usa os settings já ativos, exige a flag e pode pular se registry/Django faltar, `app/escala/tests/test_ata_ci.py:129`, `:132`, `:135`.
   - Prova do runner em teardown: usa pai simulado, `app/escala/tests/test_ata_ci.py:109`, `:119`; execução real paralela: [nao verificado].

6. **Resíduos de indicadores.** A busca do archive encontrou `nao_medidos` apenas em três asserts negativos: `app/escala/tests/test_tipo_item3_expand_contadores.py:37`, `:39`, `:146`. `cego_orfas`: nenhuma ocorrência na busca textual do archive [nao verificado — ausência global não possui linha positiva citável]. Nenhuma saída de produção desses indicadores foi encontrada [nao verificado].

## 12/11 20:25 · sessao 525ac3c7b50c · TIPO-NA-FONTE · construtor · origem=Code

Entrega parcial no [relato completo](/home/ronald/wt-codex-tipo-na-fonte-184856/app/docs/RELATO_TIPO_ITEM3_V3.txt:1), com alterações, leitores/escritores, REDs e comandos dos mutantes.

- **92 testes passaram; 1 pulado.** [Evidência](/home/ronald/wt-codex-tipo-na-fonte-184856/app/docs/TIPO_ITEM3_V3_TESTES.txt:214). [nao verificado]
- **Pendência:** a catraca detectou cópia crua em `ponto/portas/celula.py:1083`, arquivo proibido; a meta zero permanece aberta. [Reprodução](/home/ronald/wt-codex-tipo-na-fonte-184856/app/docs/TIPO_ITEM3_V3_TESTES.txt:220).
- Suite Django: **[nao verificado]**, por ausência de `django.test`. [Tentativa](/home/ronald/wt-codex-tipo-na-fonte-184856/app/docs/TIPO_ITEM3_V3_TESTES.txt:241). [nao verificado]

Sem commit, conforme a proibição do envelope. [Registro](/home/ronald/wt-codex-tipo-na-fonte-184856/app/docs/RELATO_TIPO_ITEM3_V3.txt:151). [nao verificado]

## 12/11 20:08 · sessao d89d76f9010a · - · - · origem=Ronald (tmux)

Item 5 editado: removidos `nao_medidos` e `cego_orfas`; contadores usam `contar_orfas_da_grade`, preservando quantidade legada conhecida. Referências: `escala/services/furos_diarios.py:22`, `folha/causas.py:43`, `ponto/status_dia.py:185`, `ponto/services/bordas_realizado.py:49`, `ponto/management/commands/detectar_par_relampago.py:138`.

Também removidas as saídas acrescentadas em `pos_vinculo`, esmeril, tripwire e PDF; o vigia conserva apenas seu `cego` histórico por universo. `ponto/services/vigia_pareador_celula.py:117`, `ponto/services/esmeril_espelho.py:195`, `relatorios/pdf_espelho.py:183`.

Teste novo: `escala/tests/test_tipo_item3_expand_contadores.py:16`: 6 REDs e 5 mutantes mordidos — furos, status, borda, causa e score. Comando executado:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest \
 escala.tests.test_tipo_item3_expand_contadores \
 escala.tests.test_tipo_na_fonte_orfas_consumidores \
 folha.tests.test_causas folha.tests.test_causas_sem_previsao -q
```

Resultado observado: **35 testes OK**. O negativo de estado ilegal isola o registro esperado e verifica três sítios; `escala/tests/test_tipo_item3_expand_contadores.py:47`.

Achado 6: **pre-existente, DECIDIDO PELA LEI TURNO-G5, obra própria**. Comparação por `git show 0a747193e:app/...`: naquele HEAD, `escala/utils.py:1758` guarda por dia do turno, `:1811` projeta nesse dia e `:1820` agrupa apenas sobras civis; `ponto/services/vigia_pareador_celula.py:109` procura na data civil. Equivalentes atuais: `escala/utils.py:1763`, `:1816`, `:1825` e `ponto/services/vigia_pareador_celula.py:112`. Nenhuma cura desse achado.

Limite para o relato: legado conserva quantidade, porém identidade temporal segue `None`; A5, PDF e vigia não recuperam datas ausentes. Efeito em produção: **[nao verificado]**.

## 12/11 20:06 · sessao 0062468afb3b · - · - · origem=Ronald (tmux)

Alterações concluídas nos arquivos autorizados:

- Cinco asserts via acessor: `test_ata_x_turno_entrada.py:144`, `test_saida_valida_cross_meia_noite.py:104`, `test_juiz_do_par_ata_casa_pelo_turno.py:137`, `test_bateria_lavratura.py:639/:1060`.
- Selo compara identidades com data/fuso: `ponto/tests/test_bateria_lavratura.py:961`.
- Fixture ganhou `orfas=[]`: `chamados/tests/test_emis_ausencia_parcial_cobra.py:60`.
- Fixtures de convergência usam escritor único: `escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:187/:337`.

Provas em `escala/tests/test_preverif_v3_asserts.py:45`: seis testes passaram com:

```sh
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest escala.tests.test_preverif_v3_asserts -v
```

Mutantes mordidos: fixture perde chave (`:78`), escritor duplica a mesma batida acesa como órfã (`:109`) e selo volta a HH:MM versus ISO (`:128`). O segundo compila o escritor real mutado em memória.

AST dos seis arquivos e `git diff --check` passaram. Integração Django/ORM: [nao verificado].

## 12/11 20:04 · sessao 4febaafe94c6 · - · - · origem=Ronald (tmux)

Restaurei o plantio por HH:MM do HEAD; a única diferença preservada é ler órfãs por `horas_orfas(ata)` ([materializacao.py](/home/ronald/wt-codex-tipo-na-fonte-184856/app/chamados/services/materializacao.py:177)).

Prova em [test_tipo_na_fonte_plantio_expand.py](/home/ronald/wt-codex-tipo-na-fonte-184856/app/chamados/tests/test_tipo_na_fonte_plantio_expand.py:74):

- RED 1 executado antes da correção: falhou `False is not true`.
- Mutante que reintroduz instantes naive executado: mordeu, confirmando a exceção `marca exige fuso` ([teste:85](/home/ronald/wt-codex-tipo-na-fonte-184856/app/chamados/tests/test_tipo_na_fonte_plantio_expand.py:85)). [nao verificado]
- Incluídos contrastes dia completo e órfã legado/ISO ocupando a saída ([teste:77](/home/ronald/wt-codex-tipo-na-fonte-184856/app/chamados/tests/test_tipo_na_fonte_plantio_expand.py:77)). [nao verificado]

Comando completo, para rodar após sua implementação de `horas_orfas`:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_tipo_na_fonte_plantio_expand -v
```

Na última execução, o RED positivo ainda falhava porque `horas_orfas` estava ausente; os outros três testes passaram. Não alterei `juiz_batida.py` nem `gerar_celulas.py`; o plantio corrigido deixou de consumir `periodos_do_dia` ([materializacao.py:179](/home/ronald/wt-codex-tipo-na-fonte-184856/app/chamados/services/materializacao.py:179)). [nao verificado]

## 12/11 20:01 · sessao ab67b3003d37 · TRANCA-VIVAS-08 · verificador · origem=Code

**REPROVA — bloqueante: o acumulador ainda pode perder competências sem contrariar os oráculos do arquivo.** A troca `tot[k] += linha[k]` → `tot[k] = linha[k]` sobrevive porque os casos medem uma única competência. (`app/chamados/services/competencia_trancada.py:355`; `app/chamados/tests/test_tranca_vivas_08.py:36`; `app/chamados/tests/test_tranca_vivas_08.py:244`)

1. **A resgatada percorre as entradas reais de `medir()`.** A fixture cria o chamado pela porta, fornece `data_turno`, grava `resgatado_em = fechado_em + 5 minutos` e escreve `q.chamado_destino`. O leitor calcula `competencia_do_chamado`, exige `resgatado_em > fechado_em` e consulta exatamente `q.chamado_destino_id`. O resgate é montado por `update`; o ato de resgatar pela porta não é exercitado nesse teste. (`app/chamados/tests/test_tranca_vivas_08.py:226`; `app/chamados/tests/test_tranca_vivas_08.py:229`; `app/chamados/tests/test_tranca_vivas_08.py:231`; `app/chamados/services/competencia_trancada.py:105`; `app/chamados/services/competencia_trancada.py:117`; `app/chamados/services/competencia_trancada.py:132`)

   A criação resolve a célula pelo dia informado, e a competência usa `dia_do_fato`. O mock não desloca essa competência por perguntas vivas: esse percurso consulta `disputa__chamado_id`, enquanto a fixture escreve somente `chamado_destino`; a disputa nasce sem chamado-pai. (`app/chamados/models.py:1067`; `app/chamados/models.py:1235`; `app/chamados/juizes.py:2359`; `app/chamados/juizes.py:2343`; `app/chamados/tests/test_c7_red_minimos.py:108`)

2. **O mock altera a vida, mas não torna a agregação vácua.** `dia_da_pergunta_trancado=False` remove o degrau que mataria a pergunta pela tranca. Não substitui a formação de `resgatados`, os carimbos ou a FK. O oráculo exige explicitamente `([q.pk], [], [])` nas três parcelas e união **1**, tanto no total quanto na linha. Portanto, prova a agregação **condicionada à vida simulada**; não prova que o resgate real produz essa vida. (`app/chamados/juizes.py:1405`; `app/chamados/services/competencia_trancada.py:117`; `app/chamados/services/competencia_trancada.py:132`; `app/chamados/tests/test_tranca_vivas_08.py:239`; `app/chamados/tests/test_tranca_vivas_08.py:241`; `app/chamados/tests/test_tranca_vivas_08.py:247`)

3. **A perda da resgatada está curada; permanecem sobreviventes de agregação.**

   | Local | Resultado contra os oráculos |
   |---|---|
   | União, linha 348 | Retirar resgatadas é mordido pelo novo MORDE; trocar união por concatenação continua mordido pelo caso de sobreposição. (`app/chamados/tests/test_tranca_vivas_08.py:249`; `app/chamados/tests/test_tranca_vivas_08.py:203`) |
   | Cardinalidade da união, linha 351 | O antigo mutante `len(_com_o_admin) - len(m['vivas_resgatadas'])` agora devolve **0** onde linha e total exigem **1**. (`app/chamados/services/competencia_trancada.py:351`; `app/chamados/tests/test_tranca_vivas_08.py:244`; `app/chamados/tests/test_tranca_vivas_08.py:247`) |
   | Parcelas por competência, linha 353 | `len(m[k])` → `min(1, len(m[k]))` sobrevive: parcelas verificadas são zero ou um. Com duas resgatadas, publicaria parcela **1**, embora a união fosse **2**. (`app/chamados/services/competencia_trancada.py:353`; `app/chamados/tests/test_tranca_vivas_08.py:186`; `app/chamados/tests/test_tranca_vivas_08.py:244`) |
   | Acumulador, linha 355 | `+=` → `=` sobrevive. Com duas competências contendo uma resgatada cada, o original totaliza **2** e o mutante conserva apenas **1**. (`app/chamados/services/competencia_trancada.py:355`; `app/chamados/tests/test_tranca_vivas_08.py:36`; `app/chamados/tests/test_tranca_vivas_08.py:246`) |

   Os dois sobreviventes foram conferidos em memória sobre o corpo real de `medir_frota`, com entradas sintéticas; preservam os resultados dos cenários do arquivo, inclusive os MORDE. Execução Django/ORM e sobrevivência na suíte completa: **[nao verificado]**. (`app/chamados/services/competencia_trancada.py:331`; `app/chamados/tests/test_tranca_vivas_08.py:206`; `app/chamados/tests/test_tranca_vivas_08.py:251`)

4. **A API morde troca entre os quatro campos.** Os valores **7, 3, 2, 4** são distintos, e cada chave é comparada com seu próprio valor; qualquer troca de pares falha. Conferi as seis trocas sobre o dicionário real da view. O residual também é certificado, com **5**. A projeção interna de `por_competencia` não recebe essa prova: o falso usa lista vazia. (`app/api/tests/test_api_mensageria_chamados_em_competencia_trancada.py:57`; `app/api/tests/test_api_mensageria_chamados_em_competencia_trancada.py:64`; `app/api/views_mensageria.py:1573`)

5. **Quem escreve e quem lê os valores publicados:** [nao verificado]

   | Campo | Escritor do valor | Leitor |
   |---|---|---|
   | `decidir_pelo_dp` | `medir()` escreve IDs; agregador escreve contagens. (`app/chamados/services/competencia_trancada.py:139`; `app/chamados/services/competencia_trancada.py:353`) | União e pauta do DP. (`app/chamados/services/competencia_trancada.py:348`; `app/chamados/services/competencia_trancada.py:246`) |
   | `vivas_resgatadas` | `medir()` escreve IDs; agregador conta. (`app/chamados/services/competencia_trancada.py:149`; `app/chamados/services/competencia_trancada.py:353`) | União. (`app/chamados/services/competencia_trancada.py:348`) |
   | `vivas_tocadas_pelo_admin` | `medir()` escreve IDs; agregador conta. (`app/chamados/services/competencia_trancada.py:158`; `app/chamados/services/competencia_trancada.py:353`) | União. (`app/chamados/services/competencia_trancada.py:348`) |
   | `ficam_com_o_admin` | Cardinalidade por competência e acumulação total. (`app/chamados/services/competencia_trancada.py:351`; `app/chamados/services/competencia_trancada.py:355`) | CLI e API. (`app/chamados/management/commands/chamados_em_competencia_trancada.py:22`; `app/api/views_mensageria.py:1573`) |
   | `vivas_apesar_da_tranca` | Classificação residual, cardinalidade e total. (`app/chamados/services/competencia_trancada.py:158`; `app/chamados/services/competencia_trancada.py:352`; `app/chamados/services/competencia_trancada.py:355`) | CLI e API. (`app/chamados/management/commands/chamados_em_competencia_trancada.py:27`; `app/api/views_mensageria.py:1577`) |

   As três parcelas também são lidas pela CLI e pela API. O cliente da mensageria recebe o JSON inteiro; a ferramenta acrescenta `cliente` e repassa o payload. (`app/chamados/management/commands/chamados_em_competencia_trancada.py:24`; `app/api/views_mensageria.py:1574`; `mensageria/nucleo/core_client.py:241`; `mensageria/nucleo/ferramentas.py:2038`)

Para fechar os sobreviventes demonstrados, faltam oráculos com **duas competências positivas** e **mais de um ID numa parcela**. Os atuais conferem uma competência e parcelas zero/um. (`app/chamados/tests/test_tranca_vivas_08.py:36`; `app/chamados/tests/test_tranca_vivas_08.py:186`; `app/chamados/tests/test_tranca_vivas_08.py:244`)

## 12/11 20:01 · sessao c24cf91fbb4c · BUG-CADASTRO-NAO-ALCANCA-O-FUTURO · construtor · origem=Code

C5e escrita. O [relato completo](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-193752/app/docs/RELATO-BUG-CADASTRO-C5E.txt:6) contém funções alteradas, chamadores, origem/leitores de `hoje` e comandos.

- **164 testes sem banco passaram**; RED da meia-noite falhou contra o corpo da base. [Evidências](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-193752/app/docs/RELATO-BUG-CADASTRO-C5E.txt:107). [nao verificado]
- **38 mutantes morderam por asserção**, incluindo leitura nova e `desfazer_apaga_admin`. [Comandos e resultados](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-193752/app/docs/RELATO-BUG-CADASTRO-C5E.txt:162).
- Catraca final: **31**, abaixo do teto herdado 36; sítios remanescentes enumerados. [Censo](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-193752/app/docs/RELATO-BUG-CADASTRO-C5E.txt:61).

Suíte ORM: **[nao verificado]**, sem Django disponível e com acesso a banco vetado. Sem commit, pela proibição inicial. Certificação permanece com Code/suite e opus final. [nao verificado]

## 12/11 20:00 · sessao 1286233fe78e · - · - · origem=Ronald (tmux)

Há sobreviventes concretos nos oráculos do arquivo:

| Local | Mutante | Por que passa | Contracaso |
|---|---|---|---|
| `app/chamados/services/competencia_trancada.py:355` | `tot[k] += linha[k]` → `tot[k] = linha[k]` | A fixture cria uma competência; os testes leem a linha `[0]` e o respectivo total. (`app/chamados/tests/test_tranca_vivas_08.py:36`, `:186`, `:188`, `:244`, `:246`) | Duas competências com uma resgatada cada: original **2**, mutante **1**. |
| `app/chamados/services/competencia_trancada.py:353` | `len(m[k])` → `min(1, len(m[k]))` | As parcelas verificadas têm cardinalidade zero ou um; o MORDE da soma altera somente a união. (`app/chamados/tests/test_tranca_vivas_08.py:186`, `:206`, `:244`) | Uma competência com duas resgatadas: união **2** nos dois; parcela original **2**, mutante **1**. |

Confirmei executando em memória o corpo real de `medir_frota`, extraído por AST, substituindo apenas `PeriodoFechado` e `medir`: ambos os mutantes preservaram os dicionários completos nos regimes residual, sobreposição, resgatada exclusiva, MORDE soma e MORDE sem resgatadas. A origem da função é `app/chamados/services/competencia_trancada.py:331`; execução Django/ORM e sobrevivência na suíte completa: **[nao verificado]**.

Na linha da cardinalidade da união, teto **1** morre porque o MORDE soma exige **2**; teto **2** sobreviveria por leitura aos valores examinados, mas é um exemplo menos natural que a perda do acumulador. (`app/chamados/services/competencia_trancada.py:351`; `app/chamados/tests/test_tranca_vivas_08.py:211`)

## 12/11 19:59 · sessao b87e237b6753 · - · - · origem=Ronald (tmux)

A fixture percorre as entradas reais que `medir()` lê: cria o chamado por `ChamadoColaborador.criar`, grava `resgatado_em` cinco minutos depois de `fechado_em` e aponta `q.chamado_destino` para esse chamado. `medir()` calcula a competência do chamado, exige a desigualdade estrita entre os carimbos e reconhece a pergunta pela FK de destino. (`app/chamados/tests/test_tranca_vivas_08.py:226`; `app/chamados/tests/test_tranca_vivas_08.py:229`; `app/chamados/tests/test_tranca_vivas_08.py:231`; `app/chamados/services/competencia_trancada.py:105`; `app/chamados/services/competencia_trancada.py:117`; `app/chamados/services/competencia_trancada.py:132`)

O chamado ganha a célula de `data_turno` na porta de criação; o juiz usa o dia dessa célula e a janela da empresa. A base criou essa célula para `2026-07-10`, dentro da competência testada. (`app/chamados/models.py:1067`; `app/chamados/models.py:1235`; `app/chamados/models.py:1241`; `app/chamados/juizes.py:98`; `app/chamados/juizes.py:2359`; `app/chamados/juizes.py:2372`; `app/chamados/tests/test_tranca_vivas_08.py:32`; `app/chamados/tests/test_tranca_vivas_08.py:38`)

O mock altera a leitura de vida: remove a morte pelo degrau da tranca em `_pergunta_viva_calc`; por isso o teste prova a agregação **condicionada a uma pergunta viva**, sem provar que o ato real de resgate produz essa vida. Contudo, não fabrica `resgatados`, não troca a comparação dos carimbos e não substitui a FK lida por `medir()`. O oráculo exige expressamente o PK apenas em `vivas_resgatadas`, seguido de total e linha iguais a um; portanto a união não fica vazia nem vacuamente verde. (`app/chamados/tests/test_tranca_vivas_08.py:239`; `app/chamados/juizes.py:1405`; `app/chamados/services/competencia_trancada.py:117`; `app/chamados/services/competencia_trancada.py:132`; `app/chamados/tests/test_tranca_vivas_08.py:241`; `app/chamados/tests/test_tranca_vivas_08.py:244`; `app/chamados/tests/test_tranca_vivas_08.py:247`)

O mock pode alcançar também `pode_fechar()` através de `fio_vivo`, mudando a lista de disputas encerráveis; isso não substitui os campos da união, cujas listas já foram classificadas antes desse laço. (`app/chamados/models.py:1849`; `app/chamados/models.py:1854`; `app/chamados/services/competencia_trancada.py:144`; `app/chamados/services/competencia_trancada.py:149`; `app/chamados/services/competencia_trancada.py:174`)

A competência deste chamado não ganha um atalho pelo mock: a pesquisa de perguntas vivas de `competencia_do_chamado()` filtra `disputa__chamado_id`, enquanto a fixture só escreve `chamado_destino`; a disputa da base é criada sem chamado-pai. (`app/chamados/juizes.py:2343`; `app/chamados/juizes.py:2365`; `app/chamados/tests/test_tranca_vivas_08.py:231`; `app/chamados/tests/test_c7_red_minimos.py:106`)

Execução ORM, resultado da suíte e efeito em produção: **[nao verificado]**. [nao verificado]

## 12/11 19:55 · sessao 67844783dc07 · TRANCA-VIVAS-08 · censo · origem=Code

Numeração dos anexos transcritos: primeira linha de cada corpo = 1, incluindo linhas vazias.

1. **(b) Codex viu; a triagem descartou — agregado com significado indevido e cobertura insuficiente.**  
   **Pergunta:** “Consumidor da mensageria […] muda de significado?” e “campo com dois significados”; também perguntou sobre “selo/RED sem mordida”. (`preverif.q:17,19-20`)  
   **Resposta:** identificou que o CLI publica a união como `decidir_pelo_dp`, enquanto a mensageria descreve “resposta do colaborador sem veredito”; apontou ainda: “Falta […] um caso nominal de `vivas_resgatadas` e da união sem duplicatas”. (`preverif.r:1,116`)  
   **Motivos escritos na triagem:** rótulo descartado para FILA porque a “descricao ja incompleta antes da obra; consumidor fora do compose”; cobertura descartada porque “resgate defendido por test_competencia_trancada.py:164/:177; mudar teste agora invalida a arvore certificada”. (`triagem.txt:2,6`)   [nao verificado]
   A execução específica do “mutante soma passa” consta do laudo, mas sua demonstração não foi fornecida: **[não verificado]**. (`opus_final.md:2`)

2. **(b) Codex viu; a triagem descartou — `resgatado_em` também decorre de resposta do colaborador.**  
   **Pergunta:** “As exclusoes (resgatado, tocado por admin, conversa) usam o discriminador existente ou reimplementam regra?”. (`preverif.q:15-16`)  
   **Resposta:** “`resgatado_em` não significa exclusivamente ‘resgatado pelo admin’”, pois a porta aceita resgate por resposta do colaborador. (`preverif.r:3`)  
   **Motivo escrito na triagem:** “discriminador existente, sem juiz novo”. (`triagem.txt:3`)  
   O código confirma o caminho por resposta e o docstring restrito ao admin; o “em prod 0” permanece **[não verificado]**. (`app/chamados/services/materializacao.py:724-727`; `app/chamados/services/competencia_trancada.py:75`; `opus_final.md:3`)

3. **(b) Codex viu; a triagem descartou — placar não lê o residual.**  
   **Pergunta:** “liste TODOS os escritores e leitores no archive, inclusive fora do diff”, abrangendo o contador `vivas_apesar_da_tranca`. (`preverif.q:8-9`)  
   **Resposta:** “O placar antigo **não lê o residual novo**: extrai somente a soma `chamados + perguntas + disputas`”. (`preverif.r:98`)  
   **Motivo escrito na triagem:** “FILA (instrumento, pouso proprio)”. (`triagem.txt:5`) [nao verificado]

4. **(a) Codex não apontou o achado documental, com o tema presente — assinatura do ato em JUIZES.md.**  
   **Pergunta:** anexou expressamente `app/docs/JUIZES.md` e perguntou se a mudança distingue “via de sistema” de “ato humano legitimo”. (`preverif.q:3,12-14`)  
   **Resposta:** reconheceu o predicado “aceita `via='admin'` **ou presença** de `acoes`”, mas tratou o registro apenas como existente: “C20 e sua relação com C6/tarefa já constam do registro e lookup”. Não apontou nesses trechos a necessidade de atualizar sua descrição. (`preverif.r:15,108`)  
   Classifico como omissão de tema presente: o registro mantém “usuario humano, nunca o sistema”, enquanto o filtro exige a assinatura do ato; essa atualização documental é precisamente o achado do opus. (`app/docs/JUIZES.md:88`; `app/chamados/juizes.py:1298`; `opus_final.md:5`)

5. **(c) Fora do alcance — prova de produção pós-cura.**  
   **Pergunta:** pediu o universo do contador e o censo de leitores. (`preverif.q:8-9,15-16`)  
   **Resposta:** identificou os leitores C20 em tarefa do dia e reconciliador, mas declarou: “A medição pós-cura da frota, universo não vazio, residual zero e publicação no RELATO: **[não verificado]**”. Portanto, conhecer os leitores não demonstrou seu impacto em produção. (`preverif.r:35-36,120`)  
   **Triagem:** “fora do alcance do Codex (frota/sonda_frota.r)”. As lacunas específicas sobre banco, quatro alvos e `chamados=207` são relatadas pelo opus; seus resultados não estão demonstrados no dado fornecido: **[não verificado]**. (`triagem.txt:7`; `opus_final.md:6`)

6. **(c) Fora do alcance quanto aos usuários de produção; os três atos já foram reconhecidos.**  
   **Pergunta:** perguntou sobre alcance em ato humano legítimo e leitura do histórico antigo. (`preverif.q:13-14`)  
   **Resposta:** enumerou os três fundamentos — cobrar, Resolver dia e desarquivar — e ressalvou: “Que todos os atos humanos antigos possuíam essas assinaturas: **[não verificado]**”. (`preverif.r:24,26`)  
   O código confirma o conjunto de três atos. Já as identidades/naturezas dos usuários 657/653 aparecem apenas na referência do opus à `sonda_v4.r`, cujo conteúdo não foi fornecido: **[não verificado]**. (`app/chamados/juizes.py:1242-1250`; `opus_final.md:7`)

**Contagem por classe:** **(a) 1; (b) 3; (c) 2; (e) 0; (d) 0. Total: 6 achados.** (`preverif.q:3,8-20`; `preverif.r:1,3,15,24,26,35-36,98,108,116,120`; `triagem.txt:2-7`; `opus_final.md:2-7`)

## 12/11 19:55 · sessao b923d43079e6 · E3-FORMA-SQL · sonda · origem=Code

Não executei novamente `sonda_espacos.py`: o script não foi localizado na cópia e não há acesso a banco/logs. Execução nova e identidade Git do HEAD: [nao verificado]. Abaixo está a transcrição integral do dado fornecido, incluindo o anexo; sua execução não foi verificada independentemente. [nao verificado] [nao verificado]

```text
sonda_leitura: sonda_espacos.py [71d433320633ce76] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

q_vivas_total=14548  q_vivas_com_carimbo_e_so_espacos=0
carimbo__texto                                7976  amostra=[53332, 4938, 4944, 5000, 52249, 54324, 36412, 53521, 16369, 36272]
carimbo__vazio                                9626  amostra=[48306, 48314, 4880, 4882, 4884, 48318, 4954, 7897, 5022, 5243]
sem_carimbo__vazio                           31556  amostra=[46957, 43188, 43875, 53869, 37786, 40504, 41545, 49486, 40745, 39156]
universo                                     49158  amostra=[]
total_erros=0
== anexo: e3_forma_sql_do_juiz.txt ==
aval Ronald: E3-FORMA-SQL-DO-JUIZ (12/11 19:1x). UNIDADE: PerguntaDisputa. Nenhuma lei nova, nenhum juiz novo; responde a PERGUNTA DE DESENHO E3 porta A (1)(2).
CONFERIDO: o juiz existe -- chamados/juizes.py:1515 respondida_sem_texto, com texto_vazio = (t or '').strip() (:1510); a forma SQL inline em models.py:3349-3350 (q_vivas) NAO e o mesmo predicado: so testa '' e NULL, e resposta so de espacos passa como viva. Derivacao paralela divergente = BUG no caminho (LEI-AKITA 6).
(1) A forma Q nasce como a FACE SQL do juiz respondida_sem_texto, no mesmo modulo, com o mesmo teste de vazio (trim); q_vivas:3349 e os 9 sitios `respondida_em__isnull` leem ela. Selo de equivalencia por propriedade: juiz Python == Q sobre o mesmo universo, incluindo '', '   ', NULL, texto; mutante (tirar o trim) = vermelho. Nao e juiz novo: e a mesma pergunta, declarada uma vez com duas faces (molde qs_perguntas_absolviveis). [nao verificado]
(2) Catraca pela forma de USO, nunca por lista: respondida_em em contexto booleano (isnull, is None, truthiness, filtro) = "respondeu?" e passa pelo juiz/face SQL; respondida_em lido como VALOR (instante, ordenacao, trilha) fica fora. Os 4 sitios (juizes.py:348, verbos.py:342, veredito_lavrado.py:207, backfill/DecisaoPergunta) sao julgados por essa regra no AST; o que for booleano migra.
(3) Os 17 que movem passivo: medida na sombra antes, como ja manda o aval E3-10 item 3; o 8.622 declara a UNIDADE e o recorte contra os 142 do E3-10. App do colab muda ou supra_juiz retrata = PARA e publica a lista.
LEI-AKITA: origem=chamados/juizes.py::respondida_sem_texto (face SQL), testemunha=o juiz, RED=resposta so de espacos viva no SQL e vazia no Python, quem-mais-le=9 sitios SQL + q_vivas + 4 de instante, juizes novos=0, padrao=Single Source of Truth
Segue a fila; PAREI so em lei ou !
```

Na aplicação, os escritores explícitos encontrados são estes:

| Escritor | Onde escreve e persiste | Pode gravar somente espaços com `respondida_em` preenchido? |
|---|---|---|
| `responder_pergunta_disputa_core` | Atribui texto/carimbo em [disputa_emissao.py:1467](app/chamados/services/disputa_emissao.py:1467); salva em [disputa_emissao.py:1508](app/chamados/services/disputa_emissao.py:1508). | **Não para a entrada recebida.** Faz `resposta = (resposta or '').strip()` e rejeita vazio **antes da atribuição e do save**, em [disputa_emissao.py:1432](app/chamados/services/disputa_emissao.py:1432). |
| `responder_automaticamente` | Atribui `texto` e `instante` em [disputa_emissao.py:1652](app/chamados/services/disputa_emissao.py:1652); salva ambos em [disputa_emissao.py:1655](app/chamados/services/disputa_emissao.py:1655). | **Sim, a função permite**, recebendo `texto='   '` e instante preenchido, quando a guarda permite prosseguir. O `strip()` de [disputa_emissao.py:1647](app/chamados/services/disputa_emissao.py:1647) verifica a **resposta antiga**; o novo `texto` é gravado sem trim nem rejeição de vazio. |
| `reabrir_para_correcao` | Define `resposta_colab=''` e `respondida_em=None` em [disputa_emissao.py:1593](app/chamados/services/disputa_emissao.py:1593); persiste por `descarimbar_em_lote` ou `qs.update` em [disputa_emissao.py:1630](app/chamados/services/disputa_emissao.py:1630). | **Não.** Escreve vazio literal e remove o carimbo; não depende de trim. |

O chamador operacional localizado de `responder_automaticamente` passa `_TEXTO = '[auto] Foto recebida em upload separado.'`, definido em [foto_recebida.py:25](app/api/foto_recebida.py:25), com `agora` obtido em [foto_recebida.py:42](app/api/foto_recebida.py:42), na chamada de [foto_recebida.py:48](app/api/foto_recebida.py:48). Essa chamada fornece texto não vazio; atribuir a ela uma ocorrência real de resposta só de espaços é [nao verificado].

Há também caminhos genéricos de persistência e o escritor da sombra, relevantes para não tratar a proteção do core como garantia de toda escrita:

| Caminho | Escrita e tratamento de espaços |
|---|---|
| Nascimento por `abrir` / `abrir_conversa` → `_nascer` | Recebem `**campos` em [models.py:2777](app/chamados/models.py:2777) e [models.py:2795](app/chamados/models.py:2795); `_nascer` os passa ao construtor e salva em [models.py:3015](app/chamados/models.py:3015). Não há trim nesse trecho: um par fornecido como `resposta_colab='   ', respondida_em=instante` chega bruto ao save. Uso operacional desse par por essa entrada: [nao verificado]. |
| `PerguntaDisputa.save` | Em [models.py:3414](app/chamados/models.py:3414), verifica identidade na inclusão e chama `super().save()` em [models.py:3424](app/chamados/models.py:3424), sem normalizar a resposta. Portanto, esse método não barra o par espaços/carimbo recebido em memória. |
| `descarimbar_em_lote` | Aceita `**outros` em [models.py:3253](app/chamados/models.py:3253) e os repassa ao `update` em [models.py:3265](app/chamados/models.py:3265), sem trim. A interface permite fornecer resposta/carimbo por esses argumentos; a reabertura efetiva fornece `''`/`None`, conforme [disputa_emissao.py:1593](app/chamados/services/disputa_emissao.py:1593). Uso operacional para fornecer espaços/carimbo: [nao verificado]. |
| Máscara da sombra | O inventário inclui `PerguntaDisputa.resposta_colab` em [sombra_regras_pessoais.py:85](bin/sombra_regras_pessoais.py:85). O executor atribui por `setattr` em [sombra_mascara_pessoal.py:57](bin/sombra_mascara_pessoal.py:57) e grava por `bulk_update` em [sombra_mascara_pessoal.py:62](bin/sombra_mascara_pessoal.py:62) e [sombra_mascara_pessoal.py:64](bin/sombra_mascara_pessoal.py:64), com banco exigido como `sombra` em [sombra_mascara_pessoal.py:10](bin/sombra_mascara_pessoal.py:10). Não faz trim, mas transforma espaços em `… <hash>`, conforme [sombra_regras_pessoais.py:29](bin/sombra_regras_pessoais.py:29); não produz resposta só de espaços. |

Os testes também fazem escrita direta: há um `update(resposta_colab='   ')` explícito em [test_o261_resposta_vazia.py:152](app/chamados/tests/test_o261_resposta_vazia.py:152), após preencher `respondida_em` em [test_o261_resposta_vazia.py:150](app/chamados/tests/test_o261_resposta_vazia.py:150). Isso monta um cenário de teste; execução desse cenário nesta análise: [nao verificado].

`carimbar_resposta_pela_trilha` não escreve `resposta_colab`: rejeita resposta cujo `strip()` fique vazio em [disputa_emissao.py:1670](app/chamados/services/disputa_emissao.py:1670) e salva exclusivamente `respondida_em` em [disputa_emissao.py:1676](app/chamados/services/disputa_emissao.py:1676).

A proteção demonstrada é anterior ao save no **core**; ela não existe para o texto novo no **automático**, nem no `save` genérico. [disputa_emissao.py:1432](app/chamados/services/disputa_emissao.py:1432), [disputa_emissao.py:1652](app/chamados/services/disputa_emissao.py:1652), [models.py:3414](app/chamados/models.py:3414). Qual escritor originou registros reais, ou se algum já gravou somente espaços em produção, permanece [nao verificado].

## 12/11 19:54 · sessao be142c9f6191 · - · - · origem=Ronald (tmux)

Escritores indiretos e caminhos auditados:

| Caminho | Evidência | Espaços + carimbo |
|---|---|---|
| `PerguntaDisputa._nascer` recebe `**campos`, constrói e persiste | `app/chamados/models.py:2826`, `:3015`, `:3016`, `:3018` | Pode receber `resposta_colab='   '` e `respondida_em=instante`; não faz trim. As portas `abrir`/`abrir_conversa` encaminham os campos em `:2791`/`:2803`. Os chamadores operacionais localizados não passam esse par: `app/chamados/services/disputa_emissao.py:439`, `:444`, `:837`, `:1371`; `app/chamados/services/declaracao_texto.py:376`, `:666`; `app/chamados/services/acoes_disputa.py:263`. Execução disso em produção: [não verificado]. |
| Máscara da sombra | `bin/sombra_regras_pessoais.py:85` inventaria o campo; `bin/sombra_mascara_pessoal.py:57` atribui e `:62`/`:64` fazem `bulk_update` | Restrita ao banco `sombra` por `:10`; não usa trim, mas transforma espaços em texto com hash, conforme `bin/sombra_regras_pessoais.py:29`–`:33`, portanto não produz texto só de espaços. |
| `PerguntaDisputa.save` | `app/chamados/models.py:3414`–`:3425` | Sem normalização/trim global: apenas identidade na inclusão e `super().save` dentro de transação. |
| `DecisaoPergunta.registrar` | `app/chamados/models.py:3954`–`:3956` | Grava decisões e acorda células; não reescreve `PerguntaDisputa.resposta_colab`. |

Entradas do escritor canônico:

- O próprio core faz trim e rejeita vazio antes de atribuir/gravar: `app/chamados/services/disputa_emissao.py:1432`–`:1434`; atribui o par em `:1467`–`:1468`; salva em `:1508`.
- Web faz trim antes da chamada: `app/chamados/views.py:1265`, `:1283`.
- API faz trim e rejeita vazio antes da chamada: `app/api/views.py:2614`–`:2620`.
- Correção administrativa faz trim e exige HH:MM antes da chamada: `app/chamados/services/acoes_disputa.py:334`–`:337`, `:371`.
- Declaração de hora faz trim antes de chamar: `app/chamados/services/declaracao_texto.py:319`, `:419`.
- Aplicação de declaração chama com hora extraída/formada como HH:MM: `app/chamados/services/declaracao_texto.py:33`–`:39`, `:611`–`:612`, `:693`.

A porta automática aceita texto novo sem trim: `app/chamados/services/disputa_emissao.py:1652`–`:1655`. Seu único chamador operacional localizado passa uma constante com texto, definida em `app/api/foto_recebida.py:25` e passada em `:48`. Portanto a capacidade da função de gravar espaços é demonstrável pelo corpo; entrada de espaços pelo fluxo de foto atual não é sustentada por esse chamador. (`app/chamados/services/disputa_emissao.py:1652`–`:1655`; `app/api/foto_recebida.py:25`, `:48`.)

`carimbar_resposta_pela_trilha` não escreve o campo de texto e recusa texto só de espaços antes de salvar o carimbo: `app/chamados/services/disputa_emissao.py:1670`, `:1674`–`:1676`.

## 12/11 19:54 · sessao 2ca5f70d8909 · - · - · origem=Ronald (tmux)

Escritores operacionais explícitos encontrados:

| Função | Escrita e persistência | Só espaços com `respondida_em` preenchido? |
|---|---|---|
| `responder_pergunta_disputa_core` | `app/chamados/services/disputa_emissao.py:1467` atribui texto; `:1468` instante; `:1508` salva ambos. | **Não pelo argumento recebido**: normaliza em `:1432` com `(resposta or '').strip()` e rejeita vazio em `:1433-1434`, antes da atribuição/save. |
| `responder_automaticamente` | Mesmo arquivo `:1652` atribui `texto` bruto; `:1653` instante; `:1655` salva ambos. | **Sim na função**, caso receba `texto='   '` e instante não nulo: não normaliza nem valida o argumento. O `strip()` de `:1647` examina **o texto antigo**, apenas impedindo sobrescrever resposta já existente. |
| `reabrir_para_correcao` | Mesmo arquivo `:1593` prepara `resposta_colab=''` e `respondida_em=None`; persiste por `:1632` ou por `:1630` → `app/chamados/models.py:3265-3267`. | **Não**: grava vazio e remove instante; não depende de trim. |

O único chamador operacional encontrado de `responder_automaticamente` é `app/api/foto_recebida.py:48`: fornece a constante não vazia `_TEXTO = '[auto] Foto recebida em upload separado.'` (`:25`) e `agora = _tz.now()` (`:42`). Portanto, possibilidade na função está demonstrada; geração de espaços por essa chamada atual não está demonstrada, pois o argumento é fixo.

Também há portas genéricas que podem persistir o campo:

- `PerguntaDisputa._nascer`: constrói com `**campos` em `app/chamados/models.py:3015-3016`, salva em `:3018`; recebe esses argumentos através de `abrir` (`:2777`, `:2790-2791`) ou `abrir_conversa` (`:2795`, `:2802-2803`). Não faz trim do campo; `save()` também não o faz (`:3414-3424`). Poderia receber texto de espaços e instante preenchido por esses kwargs. As chamadas operacionais examinadas não passam os campos de resposta: `disputa_emissao.py:439-452`, `:837-846`, `:1371-1378`; `declaracao_texto.py:376-377`, `:666-667`; `acoes_disputa.py:263-268`. O `**kw` de `disputa_emissao.py:845` contém apenas `chamados_relacionados_ids`, preparado em `:834-836`.
- `PerguntaDisputa.descarimbar_em_lote`: `app/chamados/models.py:3253`, `:3265-3267` repassa `**outros` a `update`, sem trim. A chamada que passa `resposta_colab` é a reabertura acima, com `''`/`None` (`disputa_emissao.py:1593`, `:1630`); a outra chamada passa apenas `validada_em`, `validada_por`, `respondida_em` (`models.py:1567`).
- Máscara da **sombra**: inventário `bin/sombra_regras_pessoais.py:85` inclui a coluna; executor `bin/sombra_mascara_pessoal.py:44-64` transforma valores, atribui por `setattr` (`:57`) e persiste por `bulk_update` (`:62`, `:64`). Exige banco chamado `sombra` (`:10`). Não faz trim, mas espaços viram marcador com hash conforme `bin/sombra_regras_pessoais.py:29-33`; não escreve `respondida_em`.

`carimbar_resposta_pela_trilha` **não escreve `resposta_colab`**: só preenche instante em `disputa_emissao.py:1674-1676`; rejeita texto só de espaços antes disso em `:1670-1671`. `PerguntaDisputa.carimbar` também só adiciona instante com texto não vazio, usando `texto_vazio` em `models.py:3131-3134`, sem reescrever o texto.

## 12/11 19:52 · sessao 31a083aa2e1d · TRANCA-VIVAS-08 · verificador · origem=Code

**REPROVA — bloqueante 1: a cobertura ainda permite retirar `vivas_resgatadas` da união sem contrariar os oráculos examinados.** O caso novo mantém essa parcela em zero; falta um caso com resgatada exclusiva conferindo o total e a linha por competência. (`app/chamados/tests/test_tranca_vivas_08.py:186`; `app/chamados/services/competencia_trancada.py:351`)

1. **BLOQUEIA 1: rótulos curados; prova da união ainda incompleta.**

   | Campo | Quem escreve o valor | Quem lê |
   |---|---|---|
   | `decidir_pelo_dp` | `medir()` inclui IDs não resgatados vindos de `respostas_sem_veredito(ignorar_tranca=True)`. (`app/chamados/services/competencia_trancada.py:126`, `:137`) | Pauta do DP, comando de encerramento e agregador. (`app/chamados/services/competencia_trancada.py:246`; `app/chamados/management/commands/encerrar_por_competencia_trancada.py:38`; `app/chamados/services/competencia_trancada.py:353`) |
   | `vivas_resgatadas` | `medir()` inclui perguntas vivas ligadas a chamados resgatados após a tranca. (`app/chamados/services/competencia_trancada.py:117`, `:132`, `:149`) | `medir_frota()` incorpora à união e conta separadamente. (`app/chamados/services/competencia_trancada.py:348`, `:353`) |
   | `vivas_tocadas_pelo_admin` | `medir()` classifica perguntas vivas usando o helper real de toque. (`app/chamados/services/competencia_trancada.py:155`, `:158`) | `medir_frota()` incorpora à união e conta separadamente. (`app/chamados/services/competencia_trancada.py:348`, `:353`) |
   | `ficam_com_o_admin` | `medir_frota()` calcula a cardinalidade da união por competência e acumula os totais. (`app/chamados/services/competencia_trancada.py:348`, `:351`, `:355`) | CLI, API e mensageria, pelo percurso abaixo. (`app/chamados/management/commands/chamados_em_competencia_trancada.py:22`; `app/api/views_mensageria.py:1573`; `mensageria/nucleo/ferramentas.py:2038`) |

   A CLI agora imprime a união como `ficam_com_o_admin=` e publica as três contagens separadas. A API repassa os quatro valores; `core_client` lê JSON, e a ferramenta descreve corretamente a união e devolve o payload. (`app/chamados/management/commands/chamados_em_competencia_trancada.py:22`; `app/api/views_mensageria.py:1573`; `mensageria/nucleo/core_client.py:239`; `mensageria/nucleo/ferramentas.py:2032`)

   O outro comando, `encerrar_por_competencia_trancada`, imprime `decidir_pelo_dp` como quantidade e IDs da lista de respostas; não imprime a união sob esse nome. (`app/chamados/management/commands/encerrar_por_competencia_trancada.py:36`; `app/chamados/services/competencia_trancada.py:139`)

   **Não encontrei leitor da CLI alterada que dependesse de `decidir_pelo_dp=` como união:** o placar extrai exclusivamente `^chamados_em_competencia_trancada=`; o teste antigo que procura `decidir_pelo_dp=1 ids=...` chama o comando de **encerramento**; o teste novo exige a contagem separada. (`bin/placar_code.sh:287`; `app/chamados/tests/test_competencia_trancada.py:54`, `:149`; `app/chamados/tests/test_tranca_vivas_08.py:199`)

   API e mensageria têm testes de contagem geral/repasse, mas suas asserções não certificam os quatro campos. (`app/api/tests/test_api_mensageria_chamados_em_competencia_trancada.py:39`; `mensageria/nucleo/tests/test_ferramenta_chamados_em_competencia_trancada.py:23`)

   Permanecem descrições históricas: o relato de 17/09 registra a semântica antiga do residual e sua soma em `ficam_com_o_admin`; TICKETS registra a regra antiga de escrever a via na pergunta. São registros datados, enquanto o fonte atual declara a retirada dessa escrita. (`app/docs/RELATO-ARQUIVO.md:17319`, `:17323`; `app/docs/TICKETS.md:1180`; `app/chamados/services/competencia_trancada.py:133`)

   Consumidores externos, usos dinâmicos em templates e leitores não identificados pelo censo textual: **[nao verificado]**.

2. **Fixture real, mordida da soma e mutante sobrevivente.**

   Por leitura, a fixture monta a **mesma pergunta** nas duas listas pelo caminho real: cria pergunta sem vínculo com a célula, grava resposta sem validação, lavra a célula via admin e posiciona a trilha após a tranca. `medir()` consulta `respostas_sem_veredito`, `pergunta_viva` e o helper de toque; a asserção exige expressamente o mesmo PK nas duas listas. (`app/chamados/tests/test_c7_red_minimos.py:110`; `app/chamados/tests/test_tranca_vivas_08.py:175`, `:177`, `:49`, `:182`; `app/chamados/services/competencia_trancada.py:126`, `:144`, `:155`) Execução dessa fixture com ORM: **[nao verificado]**.

   Conferi a agregação em memória, extraindo o corpo real de `medir_frota` e substituindo apenas as entradas: a âncora da união ocorre uma vez; DP `[id]`, resgatadas `[]`, tocadas `[id]` produzem **1**, enquanto a troca por concatenação produz **2**. Isso corresponde à transformação e ao resultado exigidos pelo teste. (`app/chamados/services/competencia_trancada.py:348`, `:351`; `app/chamados/tests/test_tranca_vivas_08.py:206`, `:211`, `:221`)

   Os exemplos simples precisam desta distinção:

   - União → somente DP ou somente tocadas mantém o resultado comportamental `1`, mas remove a âncora e é barrado por `fonte.count(velho) == 1`. (`app/chamados/tests/test_tranca_vivas_08.py:186`, `:221`)
   - Cardinalidade → `len()` de DP ou tocadas preserva a âncora, mas devolve `1` quando o teste da soma exige `2`. (`app/chamados/tests/test_tranca_vivas_08.py:211`; `app/chamados/services/competencia_trancada.py:351`)

   **O bloqueante é outro mutante:** substituir, na saída, `len(_com_o_admin)` por `len(_com_o_admin) - len(m['vivas_resgatadas'])`. Ele preserva a âncora, o vazio `0`, a sobreposição `1` e a soma mutada `2`; porém uma resgatada exclusiva passa de **1 para 0**. Conferi esses resultados em memória. Os oráculos examinados deixam passar a perda de uma categoria expressamente integrante da união. (`app/chamados/services/competencia_trancada.py:336`, `:348`, `:351`; `app/chamados/tests/test_tranca_vivas_08.py:163`, `:186`, `:189`, `:198`, `:211`)

   Os testes antigos de resgate verificam desarquivamento e vida da pergunta; o teste antigo do placar confere apenas o contador geral. Não fecham essa lacuna da união. (`app/chamados/tests/test_competencia_trancada.py:164`, `:177`, `:219`) Sobrevivência na suíte Django completa: **[nao verificado]**.

3. **FILA e JUIZES.md.**

   O item 2 continua materialmente presente: resposta do colaborador também carimba `resgatado_em`, embora o docstring da exclusão ainda diga “RESGATADO pelo admin”. (`app/chamados/services/materializacao.py:724`; `app/chamados/services/arquivo.py:205`; `app/chamados/services/competencia_trancada.py:75`)

   O item 3 permanece: o placar lê o contador geral e não extrai `vivas_apesar_da_tranca`. (`bin/placar_code.sh:287`) Registro em arquivo da cópia do encaminhamento dos itens 2, 3 e 5 para FILA, assim como medição pós-cura da frota e das quatro perguntas: **[nao verificado]**.

   **O item 4 está coerente:** JUIZES.md descreve `via='admin'` ou presença de `acoes`; o filtro exige exatamente essa assinatura em um `dict`, além de usuário não nulo, diferente do sistema, e timestamp estritamente posterior à tranca. `acoes=[]` conta por presença da chave e tem caso explícito. (`app/docs/JUIZES.md:88`; `app/chamados/juizes.py:1260`, `:1298`, `:1302`; `app/ponto/services/veredito_celula.py:76`; `app/chamados/tests/test_tranca_vivas_08.py:107`)

4. **Checklist do delta.**

   - **Segundo juiz / derivação paralela:** não identificados nos caminhos examinados; o contador consulta as autoridades existentes, e API/CLI leem `medir_frota`. (`app/core/juizes.py:292`; `app/chamados/services/competencia_trancada.py:144`, `:155`; `app/api/views_mensageria.py:1563`; `app/chamados/management/commands/chamados_em_competencia_trancada.py:14`)
   - **Segundo escritor fora da porta:** não identificado nos trechos novos de produção; são agregação, projeção e impressão. (`app/chamados/services/competencia_trancada.py:341`; `app/api/views_mensageria.py:1573`; `app/chamados/management/commands/chamados_em_competencia_trancada.py:22`)
   - **Relógio fora da borda:** não identificado; o filtro recebe `fechado_em`, e a fixture usa relógio congelado. (`app/chamados/juizes.py:1070`, `:1302`; `app/chamados/tests/test_tranca_vivas_08.py:30`, `:49`)
   - **Selo sem mordida:** há mordida contra soma, mas proteção incompleta contra perda de categoria — **bloqueante 1 acima**. (`app/chamados/tests/test_tranca_vivas_08.py:187`, `:211`)
   - **Campo com dois significados:** a publicação agora distingue união e parcelas; permanece a dívida já descrita sobre a autoria do resgate. (`app/chamados/management/commands/chamados_em_competencia_trancada.py:22`; `mensageria/nucleo/ferramentas.py:2032`; `app/chamados/services/materializacao.py:725`)

Suíte completa, histórico RED/GREEN, integridade do hash e efeito em produção: **[nao verificado]**. [nao verificado]

## 12/11 19:51 · sessao c5e6ab20fd86 · - · - · origem=Ronald (tmux)

A fixture é consistente por leitura e percorre `medir()` real: cria uma pergunta sem `celula`, grava resposta sem validação/materialização e lavra o dia via admin; o log vai para cinco minutos após a tranca. A lista DP vem de `respostas_sem_veredito(ignorar_tranca=True)` e a mesma pergunta viva entra em tocadas pelo helper real. A asserção exige explicitamente o mesmo PK nas duas listas. (`app/chamados/tests/test_tranca_vivas_08.py:34`, `:43`, `:175`, `:182`; `app/chamados/tests/test_c7_red_minimos.py:110`; `app/chamados/models.py:1843`; `app/chamados/services/competencia_trancada.py:123`, `:137`, `:144`, `:155`.) Execução ORM: [nao verificado].

Executei somente a agregação extraída por AST de `medir_frota`, em memória, substituindo import de `PeriodoFechado` e retorno de `medir`. A âncora ocorre uma vez; DP `[1]`, resgatadas `[]`, tocadas `[1]` dão união `1` e soma `2`. Isso corresponde à âncora e ao oráculo escritos no teste. (`app/chamados/services/competencia_trancada.py:348`, `:351`; `app/chamados/tests/test_tranca_vivas_08.py:206`, `:211`, `:221`.)

Os exemplos simples **não sobrevivem à classe inteira**:

- União → só DP ou só tocadas: passam o caso comportamental `1`, mas removem a âncora e falham em `fonte.count(velho) == 1`. (`app/chamados/tests/test_tranca_vivas_08.py:182`, `:221`.)
- `len(_com_o_admin)` → `len(m['decidir_pelo_dp'])`: preserva a âncora, mas o MORDE passa a devolver `1` quando exige `2`. (`app/chamados/services/competencia_trancada.py:351`; `app/chamados/tests/test_tranca_vivas_08.py:211`.)

Há, porém, um mutante ligado ao requisito que escapa aos oráculos examinados: substituir `len(_com_o_admin)` por `len(_com_o_admin) - len(m['vivas_resgatadas'])`. Mantém a âncora, preserva overlap `1`, vazio `0` e soma mutada `2`; portanto preserva as asserções da classe. Numa resgatada exclusiva, produz união `0` quando a fonte real produz `1`, contrariando a categoria declarada. (`app/chamados/services/competencia_trancada.py:336`, `:348`, `:351`; `app/chamados/tests/test_tranca_vivas_08.py:163`, `:186`, `:189`, `:198`, `:209`, `:211`.) Sobrevivência da suíte Django completa: [nao verificado].

Os testes antigos citados de resgate verificam desarquivamento, vida e segunda passada; não conferem a união de `medir_frota`. O teste antigo do placar confere apenas `chamados_em_competencia_trancada`. (`app/chamados/tests/test_competencia_trancada.py:164`, `:177`, `:219`.)

## 12/11 19:51 · sessao ea33eaacf0ca · - · - · origem=Ronald (tmux)

Censo semântico concluído por leitura:

| Campo | Quem escreve | Quem lê/publica |
|---|---|---|
| `decidir_pelo_dp` | `medir()` inclui ids não resgatados vindos de `respostas_sem_veredito(ignorar_tranca=True)` — `app/chamados/services/competencia_trancada.py:126`, `:137-139` | `_pauta_do_dp` usa ids de respostas — `:246-249`; `encerrar` chama pauta — `:232-233`; comando de encerramento imprime quantidade e ids — `app/chamados/management/commands/encerrar_por_competencia_trancada.py:36-39`; contador calcula cardinalidade — `competencia_trancada.py:353`. |
| `vivas_resgatadas` | `medir()` inclui pergunta viva ligada a chamado resgatado depois da tranca — `competencia_trancada.py:117-118`, `:132`, `:145-150` | `medir_frota()` usa na união e conta separadamente — `:348`, `:353`. |
| `vivas_tocadas_pelo_admin` | `medir()` consulta `_admin_tocou_os_dias_depois` e classifica pergunta viva — `competencia_trancada.py:155-158` | `medir_frota()` usa na união e conta separadamente — `:348`, `:353`. |
| `ficam_com_o_admin` | `medir_frota()` calcula cardinalidade da união e soma por competência — `competencia_trancada.py:348-356` | comando imprime união nominal e separadas — `app/chamados/management/commands/chamados_em_competencia_trancada.py:16-25`; API repassa — `app/api/views_mensageria.py:1573-1578`; cliente GET JSON — `mensageria/nucleo/core_client.py:239-241`; ferramenta descreve união corretamente e devolve payload — `mensageria/nucleo/ferramentas.py:2032-2040`. |

Não identifiquei consumidor da CLI que ainda interprete `decidir_pelo_dp=` como união: o placar extrai exclusivamente `^chamados_em_competencia_trancada=` (`bin/placar_code.sh:287-289`); o teste antigo procura `decidir_pelo_dp=1 ids=...` (`app/chamados/tests/test_competencia_trancada.py:149`), mas seu helper chama o **outro comando**, `encerrar_por_competencia_trancada` (`:52-55`); o teste novo procura a lista separada (`app/chamados/tests/test_tranca_vivas_08.py:199`). Consumidores externos à cópia: [nao verificado].

Há textos/fontes antigos preservados: `RELATO-ARQUIVO.md:17319-17323` registra o BO de 17/09 com `vivas_apesar_da_tranca` significando resgatadas e soma em `ficam_com_o_admin`; `:17387` registra o contador antigo. `TICKETS.md:1180` registra a regra FEITO 15/09, inclusive a escrita antiga na pergunta, enquanto o fonte atual declara sua retirada pela C7 (`competencia_trancada.py:133-135`).

O snapshot `app/chamados/tests/e4b_c12_base.json:6` contém a implementação antiga, inclusive `ficam_com_o_admin=len(dp)+len(vivas_apesar_da_tranca)`. É base de teste carregada em `e4b_c12_verificar.py:18`; o modo `base` reenxerta funções escritoras listadas (`:132-153`), e o sítio dessa família é o encerramento (`e4b_c12_sitios.json:68-72`). Não constitui parser vivo da linha alterada nem substituição de `medir_frota()` nesse modo (`e4b_c12_verificar.py:140-153`).

Testes da API e mensageria verificam contagem geral/repasse, sem certificar os quatro novos campos (`app/api/tests/test_api_mensageria_chamados_em_competencia_trancada.py:39-48`; `mensageria/nucleo/tests/test_ferramenta_chamados_em_competencia_trancada.py:23-28`). Execução desses testes: [nao verificado].

_4426 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
