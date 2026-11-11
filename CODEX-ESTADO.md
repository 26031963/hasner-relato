# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 02:13** · publicado em **11/11/2026 02:14** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 02:13 · sessao abd8f65a40e6 · FIO-E0-V4-E4B · sonda · origem=Code

1. **FOTO_SOMBRA:** `2026-11-11 02:03:21.722000-03:00`, com `hoje=2026-11-11`: é posterior ao pouso informado da E4a-2, em 10/11 às 14:5x local. [nao verificado] [nao verificado]

**Porém, a sonda apresentou erro; não concluo os números da remedida**, conforme sua regra. A saída fornecida registra `SemLavra: ENSAIO SEM A LAVRA DE PROD`, com marcador `esmeril_lavra_de_prod` **AUSENTE** e lavra carregada `78b94bfb197d278e3f538ab46c3a747a`. [nao verificado]

O código confirma que essa guarda levanta `SemLavra` quando o marcador não corresponde à lavra carregada. `ListaDoAdmin` captura a exceção e continua sem resposta da lista; portanto, `rc_sombra=0` não basta para validar a medição. [esmeril_espelho.py:266](/home/ronald/codex_ws/52a0022e962812517db4e305d09ac6668ebbf4fc/app/ponto/services/esmeril_espelho.py:266), [vigia_fio.py:219](/home/ronald/codex_ws/52a0022e962812517db4e305d09ac6668ebbf4fc/app/chamados/services/vigia_fio.py:219)

2–5. **Contadores, decomposições, conferência da soma, comparação com o DRY e conclusão sobre “hoje pra frente”: não concluídos devido ao erro.** A condição de remedida válida pós E4a-2 permanece [nao verificado].

As chaves que contam “hoje pra frente” são `CENSO_ALCANCE['alcance_de_hoje_pra_frente']` e `CENSO_VIVO['fio_morto_de_hoje_pra_frente']`. [vigia_fio.py:648](/home/ronald/codex_ws/52a0022e962812517db4e305d09ac6668ebbf4fc/app/chamados/services/vigia_fio.py:648), [vigia_fio.py:420](/home/ronald/codex_ws/52a0022e962812517db4e305d09ac6668ebbf4fc/app/chamados/services/vigia_fio.py:420)

## 11/11 02:11 · sessao bcaec9e8255c · FIO-E0-V4-E4B · leitura · origem=Code

**lei:** identidade histórica não basta para provar porta alcançável; a lei exige pergunta viva ou ação nomeada do admin quando a pendência permanece. As ações exigíveis devem vir de C19, por instância e após decisões/efeitos. (`app/docs/CORTES.md:253`; `app/docs/CORTES.md:263`; `app/docs/CORTES.md:426`; `app/docs/JUIZES.md:78–85`)

**O retorno atual não permite construir essa certidão.** `perguntar_pela_celula` devolve motivos; `perguntar` devolve quantidade de carimbos desfeitos; o gerador mistura perguntas inseridas e reusadas. O backfill descarta esses retornos e decide `segue_sem_porta` pelo recenso. (`app/chamados/services/fabrica_da_celula.py:175`; `app/chamados/services/fabrica_da_celula.py:220`; `app/chamados/services/disputa_emissao.py:470–471`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:137–143`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:343–348`)

Os 82 dias e sua distribuição são o dado fornecido. **A associação de qualquer desses dias aos ramos abaixo é [nao verificado].** Os testes foram inspecionados; execução e resultado atual da suíte são **[nao verificado]**. [nao verificado]

Para tornar a matriz legível, as referências abreviadas abaixo significam estes arquivos:

| Sigla | Arquivo |
|---|---|
| F | `app/chamados/services/fabrica_da_celula.py` |
| E | `app/chamados/services/disputa_emissao.py` |
| PD | `app/chamados/models.py` |
| R | `app/chamados/reconciliador.py` |
| M | `app/chamados/services/materializacao.py` |
| RV | `app/chamados/services/relavra_via.py` |
| T | `app/chamados/services/tarefa_do_dia.py` |
| B | `app/chamados/management/commands/backfill_fio_morto_e4b.py` |
| DT | `app/chamados/services/declaracao_texto.py` |
| C | `app/docs/CORTES.md` |
| J | `app/docs/JUIZES.md` |
| L | `app/docs/LEIS.md` |

Na coluna PROVA, nomes de testes são relativos a `app/chamados/tests/`. **“Sem teste” significa sem teste específico localizado nesta leitura; a inexistência absoluta é [nao verificado].** [nao verificado]

As portas legais usadas nas células são:

| Código | Porta devida |
|---|---|
| **P** | Pergunta viva e alcançável ao colaborador, quando C19 exige responder/perguntar. A identidade histórica sozinha não basta. (`C:253`; `C:426`; `J:78–85`) |
| **A** | Ação nomeada do admin, incluindo validação, decisão ou correção de cadastro. (`C:253`; `J:78–82`; `L:122`) |
| **N** | Nenhuma pergunta/decisão exigível para aquele marco naquele instante: dispensa, marco ainda não vencido ou efeito/decisão encerrando a obrigação. (`J:64–84`; `L:100`; `T:180–210`; `T:241–260`; `T:140–154`) |
| **U** | Porta devida indeterminada: deve permanecer `nao_sei`, sem transformar ausência de informação em dispensa. (`J:83–84`; `C:426`) |

**Matriz da seleção e das saídas da fábrica.** Cada retorno abaixo é o retorno da função indicada; a coluna de distinção informa também o que chega a `perguntar_pela_celula`. (`F:35–220`)

| Ramo — condição | Entrada que o alcança | Retorno exato | Estado gravado / nascimento | PORTA DEVIDA | Retorno distingue? | PROVA |
|---|---|---|---|---|---|---|
| V1 — `F:47–48` | Sem marcos vencidos **nem pendentes**; dia passado | `vencidos_no_instante → None` | Leitura; nada nasce | **A: CADASTRO/DP**, sem pergunta ao colab. (`C:428`; `J:91`) | Distingue falta de turno de conjunto vazio; a saída externa será `[]` | `test_e4_c20.py:20` |
| V2 — `F:47–48` | Sem marcos; hoje ou futuro | `set()` | Nada nasce | **N**, nenhum marco exigível. (`L:100`; `F:41–43`) | Não distingue ausência de turno de nenhum vencimento | `test_e4_c20.py:20` cobre o passado; hoje/futuro: sem teste específico | [nao verificado]
| V3 — `F:49` | Há vencidos ou pendentes | `set(venc)` | Nada nasce | **P/A/N**, segundo marco, decisão e efeito. (`J:78–84`) | Expõe os vencidos; não expõe causa dos excluídos | `test_fabrica_por_evento.py:45`; `test_pergunta_do_dia_em_curso.py:504` |
| F01 — `F:183–184` | `cel.veredito == 'nunca_bateu'` | `[]` | Não cria pergunta nem pauta de falta de turno | **A: lista C5**, zero pergunta. (`C:433`) | **Não** | `test_e4a2_nunca_bateu.py:185–192`; `:289` |
| F02 — `F:201–205`, escritor `F:367–373` | Passado, ativo, não dispensado; V1; nenhuma pauta reconhecida | `pauta_cadastro_sem_turno → True`; externa `[]` | Cria uma cabeça de pauta DP por colaborador | **A: DP/CADASTRO**. (`C:428`; `J:91`) | Booleano distingue criação; fábrica o descarta | `test_e4_c20.py:64`; `test_e4a_pauta_cadastro_contador.py:51` |
| F03 — `F:327–328`, `F:367–368` | Mesmo caso; pauta de cadastro viva | Juiz `True`; escritor `False`; externa `[]` | Não repete pauta | **A já entregue ao DP**; leitura efetiva pelo DP não é provada. (`C:428`; `F:321–323`) | Externa **não** distingue de F02 | `test_e4a_pauta_cadastro_contador.py:51` |
| F04 — `F:331–335`, `F:367–368` | Pauta feita/cancelada em data local ≥ dia consultado | Juiz `True`; escritor `False`; externa `[]` | Não reabre pauta do dia velho | Encaminhamento anterior satisfaz a regra de entrega. (`C:428`) | **Não** na externa | `test_e4a_pauta_cadastro_contador.py:80`; `:90` |
| F05 — `F:334–336`, `F:369–373` | Só há pauta encerrada antes do novo dia sem turno | Juiz `False`; escritor `True`; externa `[]` | Cria pauta nova para a persistência posterior do problema | **A: DP/CADASTRO**. (`C:428`) | **Não** na externa | `test_e4a_pauta_cadastro_contador.py:58`; `:67`; `:88` |
| F06 — `F:105–106` | Chamada de `motivos_a_perguntar` sobre `nunca_bateu` | `[]` | Leitura; nada nasce | **A: C5**, sem pergunta. (`C:433`) | **Não** | `test_e4a2_nunca_bateu.py:185–192` |
| F07 — `F:112–113` | Juiz `dia_sem_trabalho(cel)` verdadeiro | `[]` | Não emite | **N** para cobrança do marco; parcial com resto tem exceção própria. (`J:64–72`) | **Não** | `test_f1c4_3_emissores.py:83`; `:89` |
| F08 — `F:112–113` | Situação atual do colab diferente de `ativo` | `[]` | Não emite | Não estabelece dispensa de toda decisão histórica; C19 pesa vínculo/fato e pode exigir ação admin. (`T:168–182`; `J:78–84`) Lei específica que dispense tudo só pela situação atual: **[nao verificado]** | **Não** | Sem teste específico desta guarda da fábrica | [nao verificado]
| F09 — `F:114–115` | Dia futuro | `[]` | Não emite | **N** por teto temporal. (`L:100`; `F:8–11`) | **Não** | `test_pergunta_do_dia_em_curso.py:95` |
| F10 — `F:120–121` | Veredito preenchido fora de `VEREDITOS_ACUSAM` | `[]` | Não emite | **N** para nova pergunta; guarda inclusive admin deve ficar. (`C:438`) | **Não** | `test_fabrica_por_evento.py:52`; `test_pergunta_do_dia_em_curso.py:479` |
| F11 — `F:123–125`; cartório `:398–399` | Parcial com `periodo_parcial='sem_periodo'` | `[]` | Não cria pergunta/pai/push | **A: CADASTRO**, lista do admin. (`C:319`) | **Não** | `test_cobparc5_parcial_sem_periodo_so_lista.py:157` | [nao verificado]
| F12 — `F:123–125`; cartório `:400` | Parcial, julgamento anterior e nenhum fato novo | `[]` | Não cria pergunta/pai/push | **A: lista admin** pela mudança de regra sem fato novo. (`C:323`) | **Não** | `test_cobparc5_parcial_sem_periodo_so_lista.py:160`; `:222` | [nao verificado]
| F13 — `F:58–61` | `motivos_pela_ata` recebe `None` | `[]` | Não consulta ata nem emite | Falta de turno passado vai ao DP; não autoriza pergunta por data. (`C:428`; `F:55–60`) | **Não** na externa | `test_e4_c20.py:20` |
| F14 — `F:58–61` | Recebe conjunto vazio: nenhum marco venceu | `[]` | Não consulta ata nem emite | **N agora**, teto por instante. (`L:100`) | Não distingue de F13 | `test_fabrica_por_evento.py:45` |
| F15 — `E:897–901` → `F:65–66` | Exceção lendo marcos/ata | `_ata_do_dia → (True,None)`; motivos `[]` | Nenhuma pergunta; registra falha engolida | **U**, não prova dispensa. (`J:83–84`) | **Não**; vira o mesmo `[]` | Sem teste | [nao verificado]
| F16 — `E:902–910` → `F:65–66` | Leitor diz `sem_celula`; sem escala vigente | `(True,None)`; motivos `[]` | Nenhuma pergunta | **U**; se o teto já mostrou falta de turno passado, F02–F05 interceptam antes. (`J:83–84`; `F:201–205`) | **Não** | Sem teste específico da fábrica | [nao verificado]
| F17 — `E:902–912` → `F:65–66` | `sem_celula`, escala vigente | `(False,None)`; motivos `[]` | Não cria pergunta; acende aviso de ata | **U**, aguarda ata/lâmpadas. (`J:83–84`; `E:887–890`) | **Não** | `test_fabrica_pela_ata.py:62` usa stub; não prova o corpo de `_ata_do_dia` |
| F18 — `DT:104–105` | Leitor declara isento | `(fora_vinculo,[])`; `_ata_do_dia → (True,[])`; motivos `[]` | Nada nasce | **N**. (`T:183–189`; `J:78–84`) | **Não** | Sem teste específico desse percurso | [nao verificado]
| F19 — `DT:112–115` | Dia fora do vínculo, sem contribuição elegível da véspera | Faltantes `[]`; motivos `[]` | Nada nasce | **N** pelo vínculo/fato lido. (`T:168–182`; `J:78–84`) | **Não** | Sem teste específico desse percurso | [nao verificado]
| F20 — `DT:119–120` | Dia conhecido sem trabalho e sem parcial cobrando resto | Faltantes `[]`; motivos `[]` | Nada nasce | **N**. (`J:64–72`) | **Não** | `test_f1c4_3_emissores.py:89` caracteriza dispensa |
| F21 — `DT:121–127` | Ata agregada ou sem pares válidos E/S com hora | Estado `sem_celula`; cai em F16/F17 | Nenhuma pergunta | **U**, ata sem lâmpadas não equivale a dia quitado. (`J:83–84`) | **Não** | `test_fabrica_pela_ata.py:62` é prova do bloqueio externo |
| F22 — `DT:131–132` | Nenhuma célula do leitor tem `missing` verdadeiro | Faltantes `[]`; motivos `[]` | Nada nasce | **N** se lâmpadas provam cumprimento; **U** se indeterminadas. (`J:78–84`; `DT:91–94`) | Não separa cumprido de desconhecido | `test_fabrica_pela_ata.py:49` cobre sem faltantes |
| F23 — `F:69–70` | Faltante existe, mas seu tipo/hora não está nos vencidos | Não acrescenta motivo; pode resultar `[]` | Nada nasce para esse marco | **N agora** se ainda não venceu; **U** se incompatibilidade de insumos. (`L:100`; `J:83–84`) | **Não** | `test_fabrica_por_evento.py:45` cobre não vencido |
| F24 — `F:71` | Faltante da véspera, `f[0] != dia` | Não acrescenta motivo; pode resultar `[]` | Não emite por esta célula | Porta do **dia do turno correspondente**, não determinada por esta saída. (`T:212–215`; `J:78–84`) | **Não** | `test_fabrica_pela_ata.py:53` |
| F25 — `F:71–73` | Posição/tipo sem mapeamento | Não acrescenta motivo; pode resultar `[]` | Nada nasce para o marco | **U** se há marco devido sem identidade; C19 não deve inventá-la. (`T:269–275`; `J:83–84`) | **Não** | Sem teste específico | [nao verificado]
| F26 — `F:72–73` | Dois faltantes mapeiam para o mesmo motivo | Acrescenta o motivo uma vez | Ainda só leitura | Exigibilidade é por **instância**, inclusive segunda pausa. (`C:426`; `T:269–275`) | Lista perde multiplicidade de instâncias | `test_fabrica_pela_ata.py:53` cobre mapa; segunda instância: sem teste específico da emissão | [nao verificado]
| F27 — `F:145–157` | Todos os motivos têm linha no domínio do fato | `motivos_a_perguntar → []`; externa `[]` | Não chama `perguntar`; não exige C6 viva, fila ou disputa aberta para dedup | **P/A/N segundo decisão/efeito**, nunca decidido só pela existência histórica. (`C:253`; `C:426`; `PD:2753–2755`) | **Não** | `test_fabrica_por_evento.py:34` cobre repetição normal; dedup morto sem porta: sem teste | [nao verificado]
| F28 — `F:155–156`, `F:208–209` | Linha deduplicada tem carimbo elegível | Externa continua `[]` se nenhum motivo novo | Pode desfazer carimbo; detalhado abaixo | **P/A** se a obrigação voltou; preservar fechamento quando exigido. (`C:253`; `F:195–198`) | **Não**, quantidade é descartada | `test_o261_julgamento_desfaz_carimbo.py:92` |
| F29 — `F:214–216`, `F:277–285` | Limbo `trabalha=None`, motivos presentes, competência entregue, motivos ainda não pautados | Escritor retorna lista `novos`; externa `[]` | Cria pauta DP ancorada no dia; zero pergunta ao colab | **A: DP**, encaminhamento específico do limbo entregue. (`F:210–213`; `L:122`) Corte correspondente em LEIS/CORTES/JUIZES: **[nao verificado]** | **Não** na externa | `test_f1c4_3_emissores.py:199` |
| F30 — `F:262–265`, `F:278–285` | Mesmo caso; todos os motivos em pauta viva/feita/cancelada | Escritor `[]`; externa `[]` | Não repete pauta | Encaminhamento DP anterior; não reabre fechamento humano. (`F:259–261`; `L:122`) | Não distingue de F29 | `test_f1c4_3_emissores.py:218`; `:227`; `:243` |
| F31 — `F:217–220` | Há motivos, limbo não encaminhado ao DP | `ms`, lista de motivos solicitados | Chama escritor; pode criar, reusar, recusar costura ou não gerar pergunta nos ramos internos | **P/A** conforme C19; a lista não prova cumprimento. (`C:426`; `J:78–84`) | Distingue somente motivos pedidos | `test_fabrica_por_evento.py:34`; `test_f1c4_3_emissores.py:207`; `:212` |
| F32 — `F:228–229` | `limbo_vai_a_pauta`: sem motivos | `False` | Leitura | Não determina porta. (`J:78–84`) | Não distingue dos demais `False` | `test_f1c4_3_emissores.py:218` cobre segunda passada |
| F33 — `F:228–229` | `trabalha is not None` | `False` | Leitura | Trabalho entregue segue podendo perguntar. (`C:438`; `F:210–213`) | **Não** | `test_f1c4_3_emissores.py:212` |
| F34 — `F:230–231` | Limbo com motivos; entrega falsa ou indeterminada | `bool(entrega)`, portanto `False` nos dois | Segue para emissão | Falsa e desconhecida não são a mesma autorização no backfill. (`B:286–287`; `B:318–322`) | **Não** | Aberta: `test_f1c4_3_emissores.py:207`; entrega indeterminada: sem teste | [nao verificado]
| F35 — `F:242–245` | Vigia; limbo entregue | `motivos_sem_pauta_do_limbo(cel,devidos)` | Só lê; não cria pauta | **A: DP** para motivos ausentes. (`F:235–241`; `L:122`) | Lista não distingue dispensa de pauta já existente | `test_f1c4_3_emissores.py:218` |
| F36 — `F:242–245` | Vigia; fora do limbo entregue | `devidos` | Só lê | Exigibilidade pelos mesmos guardas da fábrica. (`F:238–241`; `C:426`) | Mesmas perdas de `motivos_a_perguntar` | `test_fabrica_por_evento.py:76` |
| F37 — `F:299–302` | Eventos sem célula atual para as datas | `0` | Nada emite | **U** quanto ao dia sem célula. (`J:83–84`) | Não distingue de células que devolveram listas vazias | Sem teste específico | [nao verificado]
| F38 — `F:299–302` | Células atuais encontradas | `sum(len(perguntar_pela_celula(...)))` | Efeitos dos ramos anteriores; soma **motivos**, não inserções | Não certifica P/A/N. (`F:179`; `F:220`) | **Não** | `test_e4_c20.py:42` |

Os auxiliares de cadastro também têm saídas próprias, consumidas pelo juiz/contador; não são prova de que uma pergunta nasceu. (`F:308–351`)

| Ramo — condição | Entrada | Retorno exato | Estado gravado | PORTA DEVIDA | Distingue? | PROVA |
|---|---|---|---|---|---|---|
| F39 — `F:312` | `_pautas_cadastro(vivas=True)` | QuerySet de cabeças DP com marcador, usando `Pauta.objects.vivas()` | Nenhum | Seleção para **A/DP**. (`C:428`) | Distingue pelo argumento, não por resultado certificado | `test_juiz_pauta_cadastro_sem_turno.py:241` |
| F40 — `F:312` | `vivas=False` | QuerySet do histórico de cabeças com marcador | Nenhum | Permite reconhecer entrega anterior. (`C:428`) | Sim, consulta histórico | `test_e4a_pauta_cadastro_contador.py:80` |
| F41 — `F:329–330` | Sem pauta viva, consulta com `dia=None` | `False` | Nenhum | Não prova ausência de encaminhamento histórico para um dia. (`F:321–325`) | Distingue do `True`; não informa motivo | `test_e4a_pauta_cadastro_contador.py:58` |
| F42 — `F:346–351` | Contador com ou sem filtro de empresas | `{'nome':'cadastro_sem_turno','numero':<colabs distintos com pauta viva>,'esperado':0,'dono':'CADASTRO'}` | Nenhum nesta função | Lâmpada de **A/CADASTRO**, não juiz de pendência. (`F:340–344`; `L:122`) | Número agregado, sem ramo/dia | `test_e4a_pauta_cadastro_contador.py:74` |

**Matriz do descarimbo chamado pela fábrica.** O retorno de `perguntar` é exclusivamente a soma dos resultados desse descarimbo; emissão não incrementa esse inteiro. (`F:167–175`)

| Ramo — condição | Entrada | Retorno exato | Estado gravado | PORTA DEVIDA | Distingue? | PROVA |
|---|---|---|---|---|---|---|
| RV1 — `RV:238–241` | Pergunta não encontrada/elegível após releitura travada | Não incrementa `n`; função retorna `n` | Nada nessa pergunta | Decisão/identidade preservada; exigibilidade por C19. (`J:78–84`; `RV:177–178`) | **Não** | `test_o261_julgamento_desfaz_carimbo.py:117`; `:152` |
| RV2 — `RV:238–241` | Julgamento passou `reabrir_fechada=False`; disputa fechou | Não incrementa `n` | Não reabre disputa nem limpa carimbo | A viva em disputa fechada mantém tratamento do admin, sem reabrir por esta porta. (`F:195–198`; `C:253`) | **Não** | `test_o261_julgamento_desfaz_carimbo.py:139`; `:165` | [nao verificado]
| RV3 — `RV:242–243` | C6 diz morta: carimbo tem lastro | Não incrementa `n` | Preserva carimbo | **N** quanto a ressuscitar essa pergunta. (`RV:65–68`; `J:78–84`) | **Não** | `test_o261_julgamento_desfaz_carimbo.py:106` |
| RV4 — `RV:246–247` | `descarimbar()` recusa | Não incrementa `n` | Preserva carimbo | Não ressuscita identidade fora do fato. (`RV:163–168`; `PD:2753–2755`) | **Não** | `test_o261_julgamento_desfaz_carimbo.py:117` caracteriza identidade |
| RV5 — `RV:244–262` | Elegível, C6 viva, porta aceita | Incrementa `n`; retorno `int n` | Limpa materialização/via; preserva resposta/validação; grava evento | **P/A** segundo resposta/decisão vigente. (`C:253`; `J:78–84`; `RV:206–207`) | Conta mudanças, não identifica perguntas/portas | `test_o261_julgamento_desfaz_carimbo.py:92` |
| RV6 — `RV:260–261` | Mesmo sucesso, disputa fechada e chamada permite reabri-la | Mesmo incremento | Também chama `reabrir_sistema` | **P/A**; continente acompanha a pergunta ressuscitada. (`RV:69–71`; `C:253`) | Não separa de RV5 | Sem teste específico desta entrada pela fábrica | [nao verificado]

**Matriz da emissão interna.** A fábrica chama o gerador dentro de `criar_ou_atualizar_disputa` e depois novamente em `perguntar`. Uma lista `[p]` pode conter pergunta reusada. (`F:172–174`; `E:669–688`; `E:444–471`)

| Ramo — condição | Entrada | Retorno exato | Estado gravado / nascimento | PORTA DEVIDA | Distingue? | PROVA |
|---|---|---|---|---|---|---|
| E01 — `E:535–566` | Disputa aberta; motivo novo | Núcleo `(d,False,sorted(novos))`; público `d` | Agrega motivos/contexto; tenta gerar perguntas | **P/A** conforme dia/decisão. (`C:426`; `J:78–84`) | Público não distingue nascimento/agregação | `test_regressao_etapa7.py:254` caracteriza núcleo com outro motivo |
| E02 — `E:535–566` | Disputa aberta; motivo já presente | Núcleo `(d,False,[])`; público `d` | Atualiza contexto, inclusive contador/data; tenta emissão outra vez | **P/A/N** por fato do dia, não por motivo já agregado. (`E:674–681`; `C:426`) | **Não** no público | `test_regressao_etapa7.py:236` |
| E03 — `E:568–603` | Sem disputa aberta; pai novo | `(d,True,list(motivos))`; público `d` | Nasce chamado-pai e disputa; tenta perguntas | **P/A** quando devida. (`C:253`; `C:426`) | **Não** no público | `test_regressao_etapa7.py:221` |
| E04 — `E:584–592` | Sem disputa aberta; chamado-pai vivo reusável | Mesma tripla de E03 | Reusa pai, anexa ocorrência, cria disputa | **P/A** quando devida. (`C:253`; `C:426`) | Não distingue pai novo de reusado | Sem teste | [nao verificado]
| E05 — `E:595–603` | Falha de auditoria após nascimento da disputa | Mesma tripla; exceção engolida | Disputa/pai permanecem | Porta humana continua exigível conforme dia. (`C:253`; `J:78–84`) | **Não** | Sem teste | [nao verificado]
| E06 — `E:398–401` | Intervalo com pergunta mesma disputa/motivo/dia, fora vias excluídas | Gerador singleton `[]` | Não insere nem reata | **P/A/N** conforme a pergunta existente; dedup não prova vida. (`C:253`; `C:426`) | **Não** | `test_emissao_costura_intervalo.py:65` |
| E07 — `E:417–420` | Entrada/saída com pergunta na mesma disputa e `_data_ev` derivada | Gerador singleton `[]` | Não insere nem reata | **P/A/N** conforme decisão/efeito. (`J:78–84`) | **Não** | `test_regressao_etapa7.py:236` cobre idempotência; ramo específico: sem teste | [nao verificado]
| E08 — `E:238–244` | Derivador termina com zero candidatos | `(None,None)` | Não esc

[truncado em 20000 de 66776 caracteres]

## 11/11 02:05 · sessao 168b267eaf81 · - · - · origem=Ronald (tmux)

Revisão somente leitura: dois pontos concretos merecem correção.

1. **Residual A/S alcança P com `trabalha=False`.** `dia_sem_trabalho` retorna verdadeiro nesse caso, mesmo com cobertura parcial; o residual aceita `indefinida` e gera `validar/admin`. Confirmei essa saída em memória. O teste P novo fixa `dia_sem_trabalho=False` e não cobre essa variação. A exclusão já possui autoridade em `cartorio.py::parcial_so_lista`. (`chamados/juizes.py:445`, `chamados/services/tarefa_do_dia.py:319`, `chamados/tests/test_e3_c8b_sem_banco.py:129`, `ponto/services/cartorio.py:398`)

2. **Guarda exportada pode suprimir tarefas anteriores à cura.** Qualquer pergunta em marco aceso entra em `pares_admin`, inclusive morta pelo C6. Se K6 disser exportada, o retorno antecipado elimina todas as instâncias, inclusive outro marco apagado que antes exigia `perguntar/sistema`. Isso excede “guardar somente as exposições novas”. (`chamados/services/tarefa_do_dia.py:457`, `chamados/services/tarefa_do_dia.py:464`, `chamados/services/tarefa_do_dia.py:203`)

K6 mantém seu predicado: a carga usa `anotar_vigencia` e a mesma combinação `so_vigentes`/`invalidada_em`. A nova entrada é opcional e preserva as chamadas anteriores. (`folha/export.py:556`, `folha/export.py:583`, `chamados/services/tarefa_do_dia.py:429`)

Os dois alertas anteriores sobre `instancia_marco` precisam desta ressalva:

- Nascimento regular recusa instância diferente da derivada; portanto a forma não-marco com instância explícita divergente depende de acervo ou escrita que contorne a porta [nao verificado]. (`chamados/models.py:3437`)
- Duas perguntas da mesma identidade são impedidas para marcos desde 21/09; antes desse horizonte, escolher apenas `ps[-1]` pode ocultar uma viva anterior. Ocorrência nos dados anexados: [nao verificado]. (`chamados/models.py:2403`, `chamados/models.py:3482`, `chamados/services/tarefa_do_dia.py:263`)

Nenhuma quebra adicional observada nos campos do dict: as chaves permanecem existentes e o agregador continua lendo `acoes`, `de`, `porta`, `celula_id`, `nao_sei`. Resultado dos testes completos dos leitores: [nao verificado]. (`chamados/services/tarefa_do_dia.py:425`)

## 11/11 02:04 · sessao 035cb0e9da4c · - · - · origem=Ronald (tmux)

Dois pontos concretos para corrigir:

1. `CargaDaVida.turnos` executa o pareamento da janela larga apenas para carregar `_bats`. Uma falha em DNA de outro dia pode interromper a subjanela válida que o avulso julgaria. Carregar as batidas diretamente com os filtros da base evita essa mudança de entrada e o processamento descartado. (`chamados/services/carga_pergunta_viva.py:163`, `ponto/turnos.py:3032`, `escala/utils.py:19`, `escala/utils.py:107`)

2. A leitura das testemunhas constrói até três ramos SQL por pergunta. A quantidade de queries é constante, mas a árvore SQL cresce linearmente; o limite concreto do banco não foi executado. [nao verificado] Um envelope mínimo/máximo de timestamps mais `pergunta_origem_id__in` preserva os filtros posteriores e mantém a consulta pequena. (`chamados/services/carga_pergunta_viva.py:107`, `chamados/services/carga_pergunta_viva.py:129`, `chamados/services/carga_pergunta_viva.py:133`)

Nos trechos revistos, a passagem de carga conserva as comparações e a ordem dos degraus do C6; ela substitui fontes de insumos. (`chamados/juizes.py:1338`, `chamados/juizes.py:1408`, `chamados/juizes.py:1421`)

Não identifiquei N+1 residual adicional no caminho alimentado. A confirmação por SQL permanece [nao verificado].

## 11/11 02:04 · sessao b2180b461e03 · - · - · origem=Ronald (tmux)

Todos os caminhos abaixo foram lidos; nenhum teste foi executado. A execução dos testes e a participação dos ramos nos 82 dias são [nao verificado]. [nao verificado]

Use `DE = app/chamados/services/disputa_emissao.py`, `M = app/chamados/models.py`, `R = app/chamados/reconciliador.py`, `F = app/chamados/services/fabrica_da_celula.py`.

O retorno público perde informação em três níveis: `PerguntaDisputa.abrir` devolve `(p, criada)`; `gerar_perguntas_disputa` devolve `[p]` tanto para nascimento quanto para reuso; `perguntar` devolve somente o número de carimbos desfeitos; `perguntar_pela_celula` devolve os motivos pedidos. (M:2791; DE:444–471; F:175; F:220)

| Ramo | Entrada/condição | Retorno e persistência | Prova |
|---|---|---|---|
| Disputa existente, motivo novo — DE:535/540 | Disputa aberta do colab; motivo ainda ausente | `_abrir_ou_agregar_disputa → (d,False,sorted(novos))`; agrega motivo e funde contexto; `criar_ou_atualizar_disputa → d` | `app/chamados/tests/test_regressao_etapa7.py:254` exercita agregação, mas motivo de exemplo é `gps_ausente` |
| Disputa existente, motivo repetido — DE:535/537 | Disputa aberta contendo motivo | `(d,False,[])`; contexto ainda recebe data, `qtd`, `ultima_em`; gera perguntas novamente, chama pendentes; retorna `d` | `app/chamados/tests/test_regressao_etapa7.py:236` |
| Disputa nova, pai novo — DE:568/587 | Nenhuma disputa aberta; porta de chamado cria pai | `(d,True,list(motivos))`; nasce pai, disputa e tentativa de perguntas; retorna `d` em DE:672 | `app/chamados/tests/test_regressao_etapa7.py:221` |
| Disputa nova, pai reusado — DE:584 | Sem disputa aberta, chamado-pai vivo já existente | Mesma tripla; anexa ocorrência ao pai, cria disputa; retorno público `d` | Sem teste específico localizado | [nao verificado]
| Auditoria do nascimento da disputa falha — DE:595/600 | Exceção no registro da trilha após criação | Exceção engolida; disputa/pai continuam; mesma tripla | Sem teste específico localizado | [nao verificado]
| Intervalo repetido — DE:398 | Pergunta mesma disputa/motivo/dia, fora `VIAS_FORA_DO_FATO` | Singleton gera `[]`; não insere nem reata | `app/chamados/tests/test_emissao_costura_intervalo.py:65` |
| Entrada/saída repetida com destino derivado — DE:417 | Pergunta mesma disputa/motivo/`_data_ev` | Singleton gera `[]`; não insere nem reata | `app/chamados/tests/test_regressao_etapa7.py:236` cobre idempotência; ramo específico [nao verificado] |
| Destino não derivado e pergunta legada NULL — DE:238/417 | Zero ou múltiplos candidatos; `_data_ev=None`; pergunta existente `data_evento=None`, `conversa=False`, fora vias excluídas | Derivador `(None,None)`; dedup consulta NULL **antes** do fallback ISO e gera `[]`, apesar de dia válido | Sem teste específico localizado | [nao verificado]
| Nascimento com destino único — DE:245/444 | Um candidato do mesmo colab/dia; não há linha do fato | `abrir → (p,True)`; pergunta datada/célula/destino; gerador `[p]` | `app/chamados/tests/test_fonte_destino.py:34`; intervalo: `app/chamados/tests/test_emissao_costura_intervalo.py:50` |
| Nascimento sem destino — DE:238/444 | Zero ou múltiplos candidatos após desempate | `abrir → (p,True)` com destino NULL; tenta reatar, depois gerador `[p]` | Dois candidatos: `app/chamados/tests/test_fonte_destino.py:44`; data divergente: `:53` |
| Reuso sem mudança — M:2922/3008 | Canônica existente; célula/destino já iguais ou destino pedido NULL | `abrir → (ex,False)`; preserva disputa, resposta, validação/carimbo; gerador `[ex]`; **não chama reatar**, pois DE:459 exige `_criada` | `app/chamados/tests/test_e4_expand_orm.py:236` prova conservação; `app/chamados/tests/test_e1_v15.py:44` prova idempotência genérica |
| Reuso muda célula — M:2965/2997 | Canônica localizada, célula atual diferente da célula do fato | `(ex,False)`; grava célula e lavra; acorda célula antiga; gerador `[ex]` | `app/chamados/tests/test_e4_expand.py:326` verifica trecho real isolado |
| Reuso muda destino autorizado — M:2968/2981 | Novo destino válido e `_costura_nao_mata_o_fio=True` | `(ex,False)`; grava destino + lavra, acorda origem; gerador `[ex]` | `app/chamados/tests/test_e1_v15.py:90` usa episódio BATIDA, caracteriza porta genérica |
| Reuso recusa destino terminal — M:2980/2984 | Destino ficou terminal; pergunta C6 viva em competência não entregue | `(ex,False)` igualmente; destino fica anterior; pode gravar mudança de célula e trilha `costura_recusada`; gerador `[ex]` | `app/chamados/tests/test_e1_v15d.py:60` caracteriza porta genérica |
| Reuso recusa costura por identidade — M:2972/2974 | `resolvedora_valida=False` | Levanta `ResolvedoraCruzada`; atomic da porta desfaz | `app/chamados/tests/test_e1_v15f.py:334` é injeção adversarial, não cenário normal da fábrica |
| Contrato de saída falha — M:2820/2821 | Saída da porta viola dia gravado | Exceção de `contrato_de_saida`; atomic desfaz | `app/chamados/tests/test_e1_v15e_contrato_de_saida.py:251`, `:262`, `:272` são mutantes/injeções |
| Auditoria do nascimento da pergunta falha — M:3023/3028 | Exceção na trilha após inserção | Engolida; pergunta continua inserida; `(p,True)` e `[p]` | Sem teste específico localizado | [nao verificado]
| Reatar levanta exceção — DE:462/466 | Qualquer falha em `reatar_pergunta_orfa` | Savepoint do reatar desfaz; inserção anterior permanece; gerador `[p]`, pergunta possivelmente órfã | Sem teste específico localizado | [nao verificado]
| Push silenciado — DE:1753 | Chamado pelo bloco F:218 | Retorno `None`; nenhum push; disputa/pergunta permanecem | Comportamento de código; teste específico de push da fábrica não localizado | [nao verificado]

O reuso é alcançável também na segunda chamada do gerador: `criar_ou_atualizar_disputa` já gera em DE:670/682; `perguntar` chama novamente em F:174. A primeira pergunta pode excluir o candidato do derivador por DE:226–229; a segunda consulta dedup por NULL, chega à porta e reusa a pergunta datada. Portanto “gerador retornou `[p]`” não prova inserção. (F:172–174; DE:226–229; DE:417–424; M:3008)

O reuso de pergunta em disputa fechada conserva a disputa antiga: o ramo não atribui `ex.disputa`. Isso é comportamento demonstrado pelo corpo, mas atribuí-lo aos 82 dias é [nao verificado]. (M:2922–3008)

| Ramo de `reatar_pergunta_orfa` | Retorno exato/estado | Prova |
|---|---|---|
| R:1569 — nenhuma data | `False`; não costura | Inalcançável pergunta nova normal da fábrica: ela fornece ISO válido em F:172 |
| R:1619 — c1 lavrou | `True`; pergunta carimbada; disputa pode fechar | `app/chamados/tests/test_o261_lavra_le_a_lampada.py:148` |
| R:1621 — c2 emitiu e reatou | `True`; chamado nasce/reusa/renasce e recebe pergunta | `app/chamados/tests/test_emissao_reata_orfa.py:48`, `:57` |
| R:1623 — HIST vazio após c1/c2 recusarem ou HIST múltiplo | `False`; conserva pergunta órfã | Sem teste específico de múltiplo localizado | [nao verificado]
| R:1643 — costura histórica recusada | `False`; conserva pergunta | Sem teste específico localizado | [nao verificado]
| R:1649 — HIST costurado e S128 chamado | `True`; costura no histórico; resolver pode materializar | `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:235` |
| R:1656 — costura no vivo derivado recusada | `False`; conserva pergunta | Sem teste específico localizado | [nao verificado]
| R:1659 — costura no vivo derivado aceita | `True`; destino gravado | `app/chamados/tests/test_fonte_destino.py:34` caracteriza derivação; chamada reatar específica [nao verificado] |

Todos esses retornos viram o mesmo `[p]` no gerador; o booleano serve apenas para decidir `refresh_from_db`. (DE:464–471)

| Saída de `lavrar_sem_turno_aberto` | Retorno/estado | Prova |
|---|---|---|
| R:362 — intervalo fora de `MOTIVOS_C1` | `False`; nada escreve | `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:174` cobre intervalo passando ao c2 |
| R:364 — resposta não vazia | `False`; nada escreve | Pergunta nova da fábrica não nasce respondida; ocorrência concorrente [nao verificado] |
| R:366 — já materializada | `False`; nada escreve | Pergunta nova normal não vem materializada; ocorrência concorrente [nao verificado] |
| R:371 — núcleo tem turno aberto | `False`; nada escreve | Sem teste direto localizado | [nao verificado]
| R:398 — C6 viva | `False`; nada escreve | `app/chamados/tests/test_o261_lavra_le_a_lampada.py:129` |
| R:405 — porta carimbar recusa | `False`; não aplica carimbo | Sem teste direto localizado | [nao verificado]
| R:414 — carimbo aceito | `True`; via `sem_turno_aberto`; se R:411, fecha disputa | `app/chamados/tests/test_o261_lavra_le_a_lampada.py:148` |

| Saída de `reemitir_chamado_do_dia` | Retorno/estado | Prova |
|---|---|---|
| R:808 — motivo fora grupo DIA | `False`; nada | Inalcançável pelos quatro motivos da fábrica: grupo em M:3700 |
| R:812 — sem dia | `False`; nada | Inalcançável pergunta nova normal: F:172 |
| R:820 — colab inativo | `False`; nada | `app/chamados/tests/test_emissao_reata_orfa.py:67`; fábrica já guarda ativo, logo requer mudança após guarda ou chamada genérica |
| R:832 — dia inválido | `False`; nada | Inalcançável pergunta nova normal: F:172 |
| R:840 — sem chamado vivo e abaixo piso | `False`; pergunta permanece sem destino | `app/chamados/tests/test_emissao_reemite_c2.py:44`, `:79`; costura no vivo abaixo piso permitida em `:92` |
| R:850 — isento | `False`; pergunta permanece | Sem teste direto fábrica localizado | [nao verificado]
| R:855 — ausência cobre | `False`; pergunta permanece | Sem teste direto fábrica localizado | [nao verificado]
| R:869 — sem chamado vivo e sem protesto cobrável | `False`; pergunta permanece | `app/chamados/tests/test_emissao_reata_orfa.py:89` |
| R:882 — parcial só lista | `False`; pergunta permanece | Sem teste direto fábrica localizado | [nao verificado]
| R:887 — veto célula | `False`; pergunta permanece | Sem teste direto fábrica localizado | [nao verificado]
| R:903 — veto régua | `False`; pergunta permanece | `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:186`, `:200` |
| R:906 — motor recusa | Estruturalmente impossível: `decidir(ESTADO_NASCENTE,'EMITIR')` sempre permitido | `app/chamados/catalogo/motor.py:130–132` |
| R:908 — intervalo fora alcance C6/K6/C7 | `False`; pergunta permanece | Morta `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:226`; entregue `:299`; trancada `:257` |
| R:933 — `ChamadoColaborador.abrir → None` | `False`; pergunta não costurada | `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:160` |
| R:936 — chamado devolvido terminal | `False`; pergunta não costurada | Sem teste específico localizado | [nao verificado]
| R:955 — reapontar recusa | `False`; **chamado já pode ter nascido** em R:910; pergunta não costurada | Sem teste específico localizado | [nao verificado]
| R:958 — costura aceita | `True`; chamado recebe pergunta | `app/chamados/tests/test_emissao_reata_orfa.py:48`; intervalo `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:174` |

`perguntar_entradas_pendentes` também é alcançável em disputa existente: o merge preserva os campos antigos do motivo, inclusive `entradas_pendentes`; ele pode emitir perguntas para outros dias além da célula sob reparo. (DE:551–559; DE:685; DE:277–297)

| Ramo pendentes | Saída/estado | Prova |
|---|---|---|
| Sem entradas/PKs válidos ou juiz não fornece dia — DE:277–285 | `[]`; nada emite | `app/chamados/tests/test_hx_entradas_pendentes.py:59`, `:64` |
| Juiz `classificar_falta` cala — DE:289 | `[]` para singleton; não altera contexto | `app/chamados/tests/test_hx_entradas_pendentes.py:43` |
| Juiz acusa, gerador não acumula perguntas — DE:294/295 | `[]`; contexto mudado só em memória, não salvo por esta função | Sem teste específico localizado | [nao verificado]
| Juiz acusa, gerador acumula — DE:295 | Lista das perguntas acumuladas; salva contexto, mesmo que lista contenha reusadas | `app/chamados/tests/test_hx_entradas_pendentes.py:33`, `:50` |

Os quatro motivos normais estão em `declaracao_texto.py:42–46`, todos catalogados como `hora` em `catalogo/perguntas.py:48–58,124–134`. `classificar_falta` dos pendentes só retorna entrada/saída em `escala/servico_jornada.py:445–471`. Assim, estes ramos genéricos não pertencem ao caminho normal da fábrica: catálogo ausente DE:315; não datado DE:325; texto sem data DE:346–366; intervalo sem fallback DE:398; hora sem dia DE:425; conversa DE:431–441. Os ISO válidos vêm de F:172 e DE:292. Não os promover a defeitos dos 82 sem entrada concreta.

As exceções de episódio BATIDA também não são saídas normais do pedido da fábrica: `batida_origem_do_nascimento` retorna `None` para família MARCO em M:3460–3462; o dia do marco vem diretamente da célula em `fio.py:1638–1640`. Os testes de `gps_ausente`/episódio/rechaveamento são provas da porta genérica, não prova de alcance por esses quatro motivos. O fecho também exclui canônica sobre batida retratada em `fio.py:1734–1736`. [nao verificado]

Defeito de código demonstrável neste subfluxo: dedup de DE:417 usa `_data_ev=None` quando o derivador não acha destino, embora `_data_fallback` tenha dia válido; linha legada NULL pode calar fato de outro dia. Cura pode ficar em `disputa_emissao.py`, usando a identidade/dia efetivo da porta, sem editar arquivos proibidos. A existência dessas linhas nos 82 dias é [nao verificado]. (DE:238–244; DE:417–424)

Reuso de canônica e veto do c2 não devem ser classificados automaticamente como defeitos: respectivamente preservam identidade e respeitam piso/ausência/régua/célula; a porta devida precisa ser resolvida pelas leis e pela sequência do dia. Uma lista `[p]` não certifica alcance, e `[]` não certifica dia dispensado. (M:2776–2781; R:770–786; DE:470–471; F:175,220)

## 11/11 02:03 · sessao d9f5b575c39a · - · - · origem=Ronald (tmux)

Leitura concluída sem escrita. A cópia tem um ato adicional ao anexo: `reabrir_credor`, planejado e executado pelo backfill. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:85`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:128`)

O defeito mais direto no command é a interrupção após `registrar_decisao`: C19 retorna `passivo_sem_linha` antes de interpretar a sequência; o command grava as decisões e não consulta C19 novamente após essa gravação. Assim, esse ato sozinho não produz a porta eventualmente exigível após regularizar o passivo. A participação disso nos 19 dias fornecidos permanece [nao verificado]. A correção pode ficar no command, voltando ao juiz existente após o registro. (`app/chamados/services/tarefa_do_dia.py:127`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:94`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:137`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:323`)

O command descarta três retornos: a lista de decisões criadas, os motivos devolvidos pela fábrica e o dict da materialização — inclusive suas recusas. Só o retorno `recusa:*` da reabertura entra em `nao_executado`. A classificação final ainda depende do recenso. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:131`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:137`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:139`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:143`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:343`)

`rejulgar` não chama nenhum escritor: retorna apenas o impedimento `rejulgar_janela` na lista do command. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:144`)

A materialização alcança toda a disputa das perguntas selecionadas por C19: a porta processa todas as perguntas respondidas, validadas e não materializadas dessa disputa. Portanto, também alcança perguntas `data_hora` e `escolha` que não foram individualmente selecionadas pelo dia do backfill. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:141`; `app/chamados/services/materializacao.py:1369`)

Censo estático dos retornos da fábrica, excluindo testes: [nao verificado]

| Função | Quem lê hoje |
|---|---|
| `vencidos_no_instante` | A própria fábrica, consumindo conjunto/`None`. (`app/chamados/services/fabrica_da_celula.py:119`; `app/chamados/services/fabrica_da_celula.py:202`) |
| `motivos_pela_ata` | A própria fábrica, lista intermediária. (`app/chamados/services/fabrica_da_celula.py:139`) |
| `dia_nunca_bateu_sem_adesao` | Guardas da fábrica, repasse e contador. (`app/chamados/services/fabrica_da_celula.py:105`; `app/chamados/services/fabrica_da_celula.py:183`; `app/chamados/services/disputa_emissao.py:758`; `app/chamados/management/commands/disparar_perguntas_competencia.py:111`) |
| `motivos_a_perguntar` | Fábrica e contador, consumindo lista. (`app/chamados/services/fabrica_da_celula.py:207`; `app/chamados/services/fabrica_da_celula.py:242`; `app/chamados/management/commands/disparar_perguntas_competencia.py:115`) |
| `perguntar` | Só fábrica; descarta o inteiro `desfeitos`. (`app/chamados/services/fabrica_da_celula.py:219`) |
| `perguntar_pela_celula` | Cartório lê truthiness, quantidade e motivos; evento soma `len`; backfill descarta. (`app/ponto/services/cartorio.py:1008`; `app/ponto/services/cartorio.py:1010`; `app/chamados/services/fabrica_da_celula.py:301`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:139`) |
| `limbo_vai_a_pauta` | Dois `if` da própria fábrica. (`app/chamados/services/fabrica_da_celula.py:214`; `app/chamados/services/fabrica_da_celula.py:243`) |
| `motivos_que_o_evento_deixou` | Vigia do command, iterando os motivos. (`app/chamados/management/commands/disparar_perguntas_competencia.py:186`) |
| `motivos_sem_pauta_do_limbo` | Vigia interno repassa a lista; escritor interno decide o que emitir. (`app/chamados/services/fabrica_da_celula.py:244`; `app/chamados/services/fabrica_da_celula.py:277`) |
| `pauta_dp_do_limbo_exportado` | Fábrica descarta a lista dos motivos novos. (`app/chamados/services/fabrica_da_celula.py:215`) |
| `perguntar_por_eventos` | Reconciliador descarta o total. (`app/chamados/reconciliador.py:1387`) |
| `_pautas_cadastro` | Juiz e contador internos consomem queryset. (`app/chamados/services/fabrica_da_celula.py:327`; `app/chamados/services/fabrica_da_celula.py:331`; `app/chamados/services/fabrica_da_celula.py:345`) |
| `pauta_cadastro_existente` | Escritor interno usa booleano. (`app/chamados/services/fabrica_da_celula.py:367`) |
| `contador_cadastro_sem_turno` | Vigia imprime número/esperado/dono e inclui número no carimbo. (`app/chamados/management/commands/vigiar_fio_morto.py:180`; `app/chamados/management/commands/vigiar_fio_morto.py:204`; `app/chamados/management/commands/vigiar_fio_morto.py:240`) |
| `pauta_cadastro_sem_turno` | Fábrica descarta booleano. (`app/chamados/services/fabrica_da_celula.py:204`) |

Chamadores dinâmicos fora desse censo estático: [nao verificado].

As matrizes detalhadas da reabertura e da materialização, com condições, retornos, persistência e testes identificados, foram enviadas nas mensagens anteriores ao agente principal. [nao verificado] [nao verificado]

## 11/11 02:03 · sessao 3084f7f6e333 · FIO-E0-V4-E4B · sonda · origem=Code

A **FOTO_SOMBRA é anterior ao pouso da E4a-2; esta remedida NÃO vale como pós E4a-2**. Uso a saída raspada fornecida; não houve nova execução nesta sessão. [nao verificado] [nao verificado]

1) `FOTO_SOMBRA = 2026-11-10 03:51:56.165000-03:00`, com `hoje = 2026-11-11`. A foto antecede o pouso informado de **10/11, 14:5x local**. [nao verificado] [nao verificado]

2) Números impressos: [nao verificado]

| Censo | Chave | Valor |
|---|---|---:|
| Alcance | `fio_morto_alcance` | 888 [nao verificado] |
| Alcance | `alcance_universo` | 3001 [nao verificado] |
| Alcance | `alcance_de_hoje_pra_frente` | 0 [nao verificado] |
| Vivo | `fio_morto` | 4 [nao verificado] |
| Vivo | `fio_morto_de_hoje_pra_frente` | 0 [nao verificado] |

3) Decomposição impressa por `rota|papel`: [nao verificado]

| Rota\|papel | Quantidade |
|---|---:|
| `aguarda_decisao_de_ausencia\|Aguardando decisao do DP` | 19 [nao verificado] |
| `aguarda_decisao_de_ausencia\|Aguardando documento do colaborador` | 2 [nao verificado] |
| `decisao_do_dia_pendente\|regularizar_batida` | 278 [nao verificado] |
| `fio_do_admin\|admin-batida` | 4 [nao verificado] |
| `fio_do_admin\|admin-cadastro` | 177 [nao verificado] |
| `porta_fila\|cobrar` | 1014 [nao verificado] |
| `porta_fila\|cobrar,decidir` | 36 [nao verificado] |
| `porta_fila\|cobrar,validar` | 2 [nao verificado] |
| `porta_fila\|decidir` | 75 [nao verificado] |
| `porta_fila\|decidir,validar` | 10 [nao verificado] |
| `porta_fila\|historico` | 2 [nao verificado] |
| `porta_fila\|validar` | 129 [nao verificado] |
| `porta_pergunta\|cobrar` | 251 [nao verificado] |
| `porta_pergunta\|cobrar,validar` | 1 [nao verificado] |
| `porta_pergunta\|historico` | 10 [nao verificado] |
| `porta_pergunta\|validar` | 103 [nao verificado] |
| `sem_porta\|nenhuma porta` | 888 [nao verificado] |

A soma de **todas** as rotas confere com `alcance_universo = 3001`; o grupo `sem_porta|nenhuma porta = 888` confere com `fio_morto_alcance = 888`. [nao verificado] Essa abrangência da tabela está declarada em [vigia_fio.py:553](/home/ronald/codex_ws/52a0022e962812517db4e305d09ac6668ebbf4fc/app/chamados/services/vigia_fio.py:553).

Por competência, a saída imprime somente **`ALCANCE_POR_COMP {}`**; não permite informar distribuição nem conferir sua soma. A sonda anexa procura `lista`/`dias`. [nao verificado] O retorno real guarda as linhas em **`alcance_dias`**, com competência na quarta posição: há erro na extração da sonda, e `{}` não demonstra ausência de competências. [vigia_fio.py:629](/home/ronald/codex_ws/52a0022e962812517db4e305d09ac6668ebbf4fc/app/chamados/services/vigia_fio.py:629)

4) Comparação com o DRY informado, sem produzir deltas numéricos novos: [nao verificado]

| Classe do DRY | DRY 10/11 | Saída fornecida | Movimento |
|---|---:|---|---|
| E3 | 232 | nao separado pela saida | indeterminado [nao verificado] |
| `segue_sem_porta` | 82 | nao separado pela saida | indeterminado [nao verificado] |
| classe e | 45 | 148 | subiu [nao verificado] |
| `nao_sei` | 20 | nao separado pela saida | indeterminado [nao verificado] |
| outros | 6 | nao separado pela saida | indeterminado [nao verificado] |
| Total | 385 | 888 | subiu [nao verificado] |

A classificação impressa atual é `a=2`, `b=0`, `c=16`, `d=722`, `e=148`; **2 + 0 + 16 + 722 + 148 = 888**, conferindo com `fio_morto_alcance`. Dentro de e: `pergunta_viva_sob_chamado_terminal=51` e `sem_pergunta_viva=97`. [nao verificado] Essas causas são as classes usadas pelo código; a saída não estabelece equivalência delas com os demais agrupamentos do DRY. [vigia_fio.py:535](/home/ronald/codex_ws/52a0022e962812517db4e305d09ac6668ebbf4fc/app/chamados/services/vigia_fio.py:535) [nao verificado]

5) **“Hoje pra frente” não é maior que zero na saída:** ambos os contadores imprimem **0**, e não há células/linhas dessa categoria mostradas. Isso não comprova o estado pós E4a-2, porque a foto é anterior. [nao verificado]

As chaves que contam essa categoria são `alcance_de_hoje_pra_frente` no alcance e `fio_morto_de_hoje_pra_frente` no vivo. [vigia_fio.py:649](/home/ronald/codex_ws/52a0022e962812517db4e305d09ac6668ebbf4fc/app/chamados/services/vigia_fio.py:649) [vigia_fio.py:423](/home/ronald/codex_ws/52a0022e962812517db4e305d09ac6668ebbf4fc/app/chamados/services/vigia_fio.py:423)

A execução fornecida registra `ERROR best-effort falhou`, operação `tipo_marco_divergente`, divergências `S x ata=E` e `E x ata=S`, mas imprime os censos e termina com `rc_sombra=0`. [nao verificado] O código registra essa divergência e retorna; esse registro não é, por si, uma interrupção da sonda. [materializacao.py:299](/home/ronald/codex_ws/52a0022e962812517db4e305d09ac6668ebbf4fc/app/chamados/services/materializacao.py:299)

## 11/11 02:02 · sessao d2f2111ab0ff · - · - · origem=Ronald (tmux)

Leis verificadas na cópia:

- Porta exigível por instância, após última decisão e conferência de efeito; desconhecido deve sair `nao_sei`: `app/docs/CORTES.md:426`; `app/docs/JUIZES.md:78–85`.
- Fio vivo obrigatório significa pergunta viva **ou ação nomeada do admin**; identidade histórica da pergunta, sozinha, não satisfaz esse enunciado: `app/docs/CORTES.md:253`; extensão ao dia sem nenhum fio: `app/docs/CORTES.md:263`.
- Folga e ata de tipo diferente de trabalho dispensam cobrança; `trabalha=None` cobra; parcial com resto cobra salvo `trabalha=False`: `app/docs/JUIZES.md:64–72`.
- Sem adesão / `nunca_bateu`: somente lista C5, 0 pergunta e 1 item C5: `app/docs/CORTES.md:433`; matriz do vigia: `app/docs/CORTES.md:434`.
- Não acusador, inclusive decisão admin, permanece sem emissão pela guarda `fabrica:120`: `app/docs/CORTES.md:438`.
- Passado sem turno previsto: pendência CADASTRO entregue ao DP; basta pauta viva ou pauta fechada em data civil local ≥ dia; isso **não afirma leitura pelo DP**: `app/docs/CORTES.md:428`; `app/docs/JUIZES.md:91`; `app/chamados/services/fabrica_da_celula.py:311–335`.
- Parcial sem período: lista admin CADASTRO, sem inventar a ponta: `app/docs/CORTES.md:319`; desconhecido preserva estado anterior: `app/docs/CORTES.md:334`.
- `PERGUNTA-EM-LIMBO` não foi encontrada em LEIS/CORTES/JUIZES **[nao verificado]**. A decisão literal e seu histórico estão em `app/docs/TICKETS.md:534`: só as duas sem reabertura recebem carimbo pela trilha; 19 reabertas terminam com texto limpo.
- Cura na origem: `app/docs/LEIS.md:59`; testemunha lê autoridade: `app/docs/LEIS.md:60`; CADASTRO/BATIDA seguem como lista admin: `app/docs/LEIS.md:122`; furo = pergunta em aberto: `app/docs/LEIS.md:120`.

Testes por ramo, lidos estaticamente; execução não realizada **[nao verificado]**: [nao verificado]

| Ramo | Teste existente | [nao verificado]
|---|---|
| Emissão normal e segunda passada deduplicada | `app/chamados/tests/test_fabrica_por_evento.py:34` (confere lista, inserção e segunda chamada `[]`) |
| Marco de hoje não vencido | `app/chamados/tests/test_fabrica_por_evento.py:45` |
| Não acusador | `app/chamados/tests/test_fabrica_por_evento.py:52`; `app/chamados/tests/test_pergunta_do_dia_em_curso.py:479` |
| Futuro | `app/chamados/tests/test_pergunta_do_dia_em_curso.py:95` |
| Sem turno previsto | `app/chamados/tests/test_e4_c20.py:20` (`vencidos=None`, motivos `[]`); vigia: `app/chamados/tests/test_fabrica_pela_ata.py:66` |
| Pauta cadastro nova / viva existente | `app/chamados/tests/test_e4_c20.py:64`; ORM: `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:51` |
| Pauta feita ou cancelada cobre dia velho, mas não dia posterior | `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:58`, `:67`, `:80`, `:90` |
| Ordem trava → juiz → escritor da pauta | AST em `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:262`; mutantes em `:299` e `:316` |
| `nunca_bateu` sem pergunta e com C5 | `app/chamados/tests/test_e4a2_nunca_bateu.py:289`, corpo verificado em `:185–192` |
| Folga | `app/chamados/tests/test_f1c4_3_emissores.py:83` |
| Ausência lavrada mesmo com `trabalha=True` | `app/chamados/tests/test_f1c4_3_emissores.py:89` |
| Limbo aberto cobra | `app/chamados/tests/test_f1c4_3_emissores.py:80`, `:207` |
| Parcial sem período, primeiro julgamento | `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:157` |
| Parcial sem fato novo após julgamento | `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:160`, `:222` |
| Parcial com período e resto devido | `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:209` |
| Sem faltantes na ata | `app/chamados/tests/test_fabrica_pela_ata.py:49` |
| Ata `(False,None)` | `app/chamados/tests/test_fabrica_pela_ata.py:62` (stub, não exercita exceção interna de `_ata_do_dia`) |
| Mapeamento posição/tipo/data, incluindo exclusão do marco do dia seguinte | `app/chamados/tests/test_fabrica_pela_ata.py:53` |
| Exceção engolida em `_ata_do_dia` → `(True,None)` | sem teste específico encontrado **[nao verificado]** | [nao verificado]
| Carimbo deduplicado porém desfazível | `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:92` |
| Carimbo de pergunta morta preservado | `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:106` |
| Via identidade preservada | `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:117` |
| Disputa fechada não reabre no julgamento | `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:139` |
| Corrida: admin decide depois da coleta | `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:152` |
| Corrida: disputa fecha depois da coleta | `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:165` |
| Limbo entregue → pauta, zero pergunta | `app/chamados/tests/test_f1c4_3_emissores.py:199` |
| Limbo entregue segunda passada | `app/chamados/tests/test_f1c4_3_emissores.py:218` |
| Limbo entregue pauta feita / cancelada não repete | `app/chamados/tests/test_f1c4_3_emissores.py:227`, `:243` |
| Trabalho entregue segue perguntando | `app/chamados/tests/test_f1c4_3_emissores.py:212` |
| Evento deduplica datas e relê célula atual | `app/chamados/tests/test_e4_c20.py:42` |
| Propagação do relógio | `app/chamados/tests/test_e4a_relogio_injetado.py:57`, `:70`, `:82` |
| Gerador sem data não cria | `app/chamados/tests/test_gerar_perguntas_data.py:39`; `app/chamados/tests/test_fio_disputa_resolvido.py:188` |
| Data via grade | `app/chamados/tests/test_gerador_grade.py:32` |
| Derivação ambígua mantém fallback de data | `app/chamados/tests/test_zumbi_data_evento.py:35` |
| Um destino / dois destinos / data divergente | `app/chamados/tests/test_fonte_destino.py:34`, `:44`, `:53` |
| Intervalo dedup por dia | `app/chamados/tests/test_intervalo_datado.py:34`, `:41`; `app/chamados/tests/test_emissao_costura_intervalo.py:65` |
| Motivo retratado volta como pergunta nova (não datado / datado) | `app/chamados/tests/test_e1_volta2.py:251`, `:271` |
| Motivo não datado existente deduplica | `app/chamados/tests/test_e1_volta2.py:293` |
| Conversa não absorve fato | `app/chamados/tests/test_e1_volta2.py:117` |
| Retratada fora do domínio do fato | `app/chamados/tests/test_e1_volta2.py:128` |
| Reuso rechaveia e é idempotente | `app/chamados/tests/test_e1_v15d.py:60` |
| Reuso sem célula / colisão / exportada recusa | `app/chamados/tests/test_e1_v15d.py:84`, `:101`, `:124` |
| Dedup `_data_ev=None` bloqueia fallback válido por linha legada nula | sem teste específico encontrado **[nao verificado]** | [nao verificado]
| Catálogo sem template / exceção em fallback da grade | sem teste específico encontrado **[nao verificado]** | [nao verificado]
| Cura limbo carimba com instante real da trilha / não age fora do meio estado / recusa universo mudado / dry | `app/chamados/tests/test_curar_perguntas_em_limbo.py:29`, `:44`, `:69`, `:75` |

Atenção para o objeto do teste `test_ja_perguntou_consulta_o_JUIZ`: ele protege `_ja_perguntou`, exigindo `pergunta_viva`, e **não** protege o dedup de `motivos_a_perguntar`; portanto não serve de prova de vitalidade naquele ramo. (`app/chamados/tests/test_contract_dedup_status.py:57–67`; `app/chamados/services/fabrica_da_celula.py:145–157`)

O command desta cópia já chama reabertura do credor antes dos outros atos; lê apenas suas recusas, ignora o retorno de registrar decisão, perguntar e materializar; `rejulgar` produz a pendência `rejulgar_janela`. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:128–146`)

## 11/11 02:00 · sessao 07734ad0bb51 · BO-DISPUTA-SEM-PERGUNTA · leitura · origem=Code

**NÃO LIBERA:** permanecem a recostura intervalo→genérico e o fechamento sem testemunho da ata. (`app/chamados/models.py:2982`; `app/chamados/services/disputa_emissao.py:1488`; `app/chamados/services/disputa_emissao.py:1567`.)

Referências abreviadas: serviços em `app/chamados/services/`; `models.py` em `app/chamados/`; auditor = `app/chamados/management/commands/auditar_invariantes_chamados.py`. “Novo/anterior” abaixo compara com o **diff fornecido**; identidade histórica pelo Git **[não verificado]**. Os contraexemplos são entradas de teste; ocorrência em produção **[não verificado]**.

| Achado | Veredito | Arquivo:linha / trecho e input concreto | Origem | Cabe fora dos proibidos? |
|---|---|---|---|---|
| **P1 — Recostura intervalo→genérico**, inclusive intervalo fora do lote | **PROCEDENTE. Mesma classe do achado 1 da triagem5**, não curada universalmente pela c21. | `disputa_emissao.py:1296` fotografa somente o lote; `:1044` conserva o destino recebido quando falta chave; `models.py:2982`: `ex.chamado_destino = chamado_destino`, salvo em `:3006`. Input: P `intervalo_saida`/D, destinada ao intervalo I em dona estrangeira; agrupador recebe apenas `[genérico G]`. Ensaio em memória: **I→G**, com dona aberta e fechada. ORM **[não verificado]**. | Escritor efetivo: `models.py:2982`. A classe também alcança `fio.py:925`; normalizar apenas gerador/declaração não governa esses escritores. | **Sim para impedir a gravação:** ambos já passam pela guarda compartilhada em `models.py:2980` e `fio.py:909`. A recusa do reuso conserva a canônica em `models.py:3008`. Cura implementada/ORM **[não verificado]**. |
| **P2 — Primeiro do lote versus menor PK** | **PROCEDENTE. Mecanismo NOVO c21; mesma classe de derivação paralela do achado 2 da triagem5.** | `disputa_emissao.py:1299`: `setdefault`; `:291`: `sorted(..., key=lambda ch: ch.pk)`. Input: I1/I2, mesmo motivo/dia; lote `[I2,I1]` elege I2; avulso elege I1. Coerência entre consumidores exigida em `CLAUDE.md:175` e `:176`. | Eleições dos pedidos em `disputa_emissao.py:293` e `:1299`; persistência posterior em `models.py:2982` ou `fio.py:925`. | **Sim**, unificando a eleição na autoridade compartilhada. Qual PK deve prevalecer por mérito **[não verificado]**. |
| **P3 — Replay conserva `None`, porta rederiva origem** | **PROCEDENTE; anterior à c21.** Atribuição a achado efetivamente conferido na triagem5 **[não verificado]**. | Argumento conservado em `disputa_emissao.py:545` e repassado em `:766`; `models.py:2866` usa `valor or derivador(dona)`. Input: `foto_ausente`, `batida_origem_id=None`, dona com `ultima_batida.id=71`, batida 71 válida do colaborador: seleciona **71**, não `None` (`models.py:3463`; `:3467`). Replay divergente é desfeito em `disputa_emissao.py:767`; persistência de P errada **[não verificado]**. | `models.py:2866`; há outra rederivação na inserção em `:3448` e `:3449`. | **Não**, para distinguir ausência explícita na própria porta. Hunk abaixo. |
| **P4 — Dedup evita registrar motivo em dona fechada** | **PROCEDENTE; anterior à c21.** Conferência anterior pela triagem5 **[não verificado]**. | `disputa_emissao.py:1050` retorna antes do encaminhamento; `:1116` mantém `criou=False`; `:1332` só agrega se `_cc`. Input: dona fechada com `motivos=['anterior']`, P viva `intervalo_saida`/D destinada a I, ata acusa esse intervalo, lote `[I]`. Ensaio: retorno `None`, **nenhuma trilha**; filtro encerra em `:1346`. ORM **[não verificado]**. | Retorno antecipado `disputa_emissao.py:1050` e guarda que impede o registrador em `:1332`. | **Sim**, no emissor; sem reabrir a dona. |
| **P5 — Trilha pós-commit pode faltar e I23 permanecer zero** | **PROCEDENTE no caso de falha injetada; anterior à c21.** Ocorrência produtiva **[não verificado]**. | `descarte_disputa.py:37` agenda após commit; `core/observ.py:140` pode falhar e `:145` devolve `None`; I23 lê somente linhas existentes (`auditor:81`). Input: descarte confirmado, inserção de auditoria falha: **sem linha de descarte/I23=0**. Há ERROR e contador de engolidos (`core/observ.py:79`), portanto não é totalmente silencioso. | Separação entre confirmação do ato e escrita da trilha em `descarte_disputa.py:37`, com falha absorvida em `core/observ.py:111`. | **Sim**, no serviço de descarte, preservando rollback dos ensaios. |
| I23 isolado não determina exit 2 | **DESCARTADO — alarme já coberto.** Exigir exit 2 adicional é **LEI** não escrita nos avais fornecidos **[não verificado]**. | I23 positivo publica `VIOL` em `auditor:95` e `:1925`; somente F1/F2 determinam exit 2 em `:1942`. | — | — |
| **L1 — Novo veto técnico de autofecho** | **LEI para conservar a extensão** a sem-data/base/marco inválido/falha técnica. Autorização literal específica nos avais **[não verificado]**. **NOVO c21.** | Flags em `disputa_emissao.py:1103`, `:1397`, `:1453`, `:1482`, `:1485`; veto adicional em `:1562`, mesmo após `pode_fechar()` responder sim. Autoridade de fechamento: `models.py:1848`; `app/docs/JUIZES.md:62`; escopo literal: `CLAUDE.md:37`. | Predicado de autofecho em `disputa_emissao.py:1562`. | **Sim para restringir ao escopo HX/C7 existente**; manter a extensão demanda lei. |
| **P6 — Aviso afirma ata ausente sem consultá-la** | **PROCEDENTE; NOVO c21.** | Input: chamado datado, sem origem, `_tem_base=False`, ata existente/vazia. `disputa_emissao.py:1485` marca incompletude; `:1564` aciona aviso; `declaracao_texto.py:71` escreve “ata nao lavrada”. Ensaio: **zero consultas à ata, aviso acionado**. ORM **[não verificado]**. | Classificação da causa em `disputa_emissao.py:1485` e uso indiscriminado da luz em `:1564`. | **Sim**, emissor/porta da luz. |
| **P7 — Outros `continue` ainda permitem fechar sem ata** | **PROCEDENTE. Mesma classe indicada no item 5 da triagem5; anterior à c21.** | Input: chamado genérico datado, base presente, classificador retorna `(None,None,None)`, ata ausente. `disputa_emissao.py:1488` pula `:1490`; `:1567` fecha. Ensaio: **fechada, zero consultas à ata, zero luz**. Contrato HX em `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:59`. ORM **[não verificado]**. | Causa no ordenamento/gate em `disputa_emissao.py:1447` e `:1488`; escritor final de fechamento em `models.py:1932`. | **Sim**, no emissor, antes dos retornos antecipados. |
| WARNING sem leitor / falta de contador técnico próprio | **DESCARTADO — já coberto pelos contadores de estado.** | I7a/I7b consultam abertas sem linhas P e a ata (`auditor:62`; `:70`). Sem dia entra em ESTRUTURA/I7a (`:64`; `:73`). Ler WARNING ou criar contador exclusivo não é requisito demonstrado nos avais **[não verificado]**. | — | — |
| **P8 — Pendentes emitem e agregam sem fronteira transacional comum** | **PROCEDENTE pela inconsistência concreta; anterior à c21.** Conferência anterior pela triagem5 **[não verificado]**. | `disputa_emissao.py:344` emite; `:354` agrega depois; wrapper chama após núcleo em `:893`. Input: DS existente `motivos=['anterior']`; pendente gera `intervalo_saida`; save da agregação falha. Ensaio: **P persistida, motivos ainda `['anterior']`**. ORM **[não verificado]**. | Emissão separada da escrita dos motivos em `disputa_emissao.py:616` e `:622`. | **Sim**, envolvendo emissão e agregação no mesmo ato, com a ordem de trava existente. |
| Push estrangeiro misto não coleta os novos da dona | **DESCARTADO como bloqueador — fora do escopo literal demonstrado dos avais.** | Comportamento confirmado: `disputa_emissao.py:664` ignora retorno; `:706` devolve novos locais; `:894` usa esses no push. Obrigação de notificar cada dona estrangeira **[não verificado]**. | — | — | [nao verificado]
| **P9 — Push antes da confirmação externa** | **PROCEDENTE; anterior à c21.** Conferência anterior pela triagem5 **[não verificado]**. | Input de teste: chamador abre atomic externo, executa repasse com push habilitado e depois lança `ValueError`. Envio em `disputa_emissao.py:1307`/`:1574`, ainda sob atomic de `:1285`; envio síncrono em `app/chamados/push_utils.py:40`. Resultado: **envio pode sobreviver ao rollback dos dados**; efeito externo real **[não verificado]**. | Invocações do envio dentro do ato em `disputa_emissao.py:1307` e `:1574`. | **Sim**, agendamento após commit no emissor. |
| Booleano da primeira abertura da declaração descartado | **DESCARTADO — leitura literal sem saída errada demonstrada.** | `disputa_emissao.py:834` ignora o booleano, mas `:835` confere dona; replay confere criação e identidade em `:843`. | — | — |
| Exceção do gerador deixar nascimento incompleto | **DESCARTADO — já coberto.** | Nascimento e geração dentro do atomic: `disputa_emissao.py:679`, `:709`, `:730`. | — | — |
| Geração vazia confirmar nascimento no núcleo | **DESCARTADO — já coberto.** | Sem motivo local admitido, rollback em `disputa_emissao.py:737` e `:739`. | — | — |
| Canônica estrangeira deixar nova DS sem P | **DESCARTADO quanto ao nascimento; resíduos separados em P1/P3.** | Rollback local em `disputa_emissao.py:739`; replay em `:766`; rollback do agrupador em `:1524`. | — | — |
| Declaração pública aceitar ausência de pergunta | **DESCARTADO — já coberto.** | Porta pública exige pergunta em `disputa_emissao.py:803` e delega em `:807`. | — | — |
| Seed estrangeiro admitido na local / limpeza de acervo | **DESCARTADO — admissão nova coberta; defeito no acervo não demonstrado.** | Guarda de reuso estrangeiro em `disputa_emissao.py:1352`; seed condicionado em `:1506` e `:1508`. Acervo produtivo **[não verificado]**. | — | — |
| Segundo agregador na âncora | **DESCARTADO — já coberto.** | Porta comum em `disputa_emissao.py:602`; uso na âncora em `:1652`. | — | — |
| Analisar chamado após replay divergente | **DESCARTADO — já coberto nesse ramo.** | Confirmação do replay em `disputa_emissao.py:1535`; análise condicionada em `:1545` e `:1547`. | — | — |
| Derivação duplicada índice/B | **DESCARTADO — curada nesse par.** P2 permanece como outra derivação paralela. | Fotografia única em `disputa_emissao.py:1296`, consumida em `:1298` e `:1398`. | — | — |
| H2: ata lida, nada a perguntar, fechar vazio | **DESCARTADO como regressão geral — contraste preservado.** | Caso em memória em `app/chamados/tests/test_bo_disputa_volta5m_memoria.py:189`; contrato protegido em `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:78`. Execução ORM **[não verificado]**. | — | — |
| Regularizável pode produzir fotografia vazia | **DESCARTADO como defeito autônomo — leitura correta, obrigação adicional não demonstrada.** | Admissão delegada em `disputa_emissao.py:1009`; datas inválidas excluídas em `:274`. Exigir pergunta com marco inválido **[não verificado]**. | — | — |
| Helper não total para qualquer JSON | **DESCARTADO como regressão c21 — pré-condições já presentes no índice do patch fornecido.** | `contexto_json=['x']` falha no `.get`; `marcos_faltantes=7` falha na iteração em `disputa_emissao.py:269`, fora do `try`. Ocorrência produtiva **[não verificado]**. | — | — |
| Captura estreita deixa escapar `RuntimeError` extraordinário | **DESCARTADO — contraexemplo fora do JSON ordinário.** | Exige objeto cujo `.get` lança exceção extraordinária; captura atual em `disputa_emissao.py:274`. Entrada persistida desse tipo **[não verificado]**. | — | — |
| Tipo diferente de `S` vira volta | **DESCARTADO como novidade — comportamento preservado.** | Expressão em `disputa_emissao.py:276`; obrigação de outro tratamento **[não verificado]**. | — | — |
| Conversão civil→turno nesse helper | **DESCARTADO — leitura errada.** | `DiaDoTurno` é `NewType` em `app/core/dia_do_turno.py:13`; função retorna identidade em `app/ponto/turnos.py:2943`. | — | — |
| Detector prova procedência atual da data civil | **DESCARTADO — emissão aposentada.** | Derivação civil em `app/ponto/management/commands/detectar_intervalo_ausente.py:82`; aposentadoria/`continue` em `:149` e `:156`. | — | — |
| Mutantes morrerem na montagem / RED sem mordida | **DESCARTADO — já coberto nos runners examinados.** | Montagem fora do runner em `app/chamados/tests/test_bo_disputa_volta5_memoria.py:389`; errors rejeitados em `:415`; c21 rejeita errors em `app/chamados/tests/test_bo_disputa_volta5m_memoria.py:218`. | — | — |
| “Paridade histórica” compara fonte atual consigo | **DESCARTADO — teste declara repetibilidade.** | Declaração explícita em `app/chamados/tests/test_bo_disputa_volta3_memoria.py:254`. Equivalência histórica **[não verificado]**. | — | — |
| Catraca mudou para outro sítio / perdeu mordida | **DESCARTADO — registros correspondem aos sítios atuais e assertion permanece.** | Extrações em `disputa_emissao.py:928`, `:929`, `:1445`; registros em `app/core/tests/test_medida_turno_do_par.py:268`; comparação integral em `:541`. Identidade histórica independente pelo Git **[não verificado]**. | — | — |
| Falha protegida ORM considerada curada pelo double | **DESCARTADO como certificação de cura.** Execução real permanece **[não verificado]**. | Assertion ORM exige destino `intervalo.pk` em `app/chamados/tests/test_bo_disputa_volta5b_orfaos.py:157`; limitação do double em `app/chamados/tests/test_bo_disputa_volta5m_memoria.py:6`. | — | — |
| Novo escritor persistente, relógio novo, campos com dois significados | **DESCARTADO como acusação autônoma; oscilação de destino já é P1/P2.** | Escritores registrados em `fio.py:807` e `app/core/tests/test_contract_escritores_estado.py:61`; atribuições temporárias restauradas em `fio.py:783`; mesmo PK em `models.py:366`; cenários DS em `models.py:1743`, identidade P em `:2427`, conversa em `:2794`. Relógio novo no delta **[não verificado]**. | — | — |
| Censo de campos e cadeia signal→cartório→fábrica | **DESCARTADO como bloqueador autônomo — descrição estática, sem defeito adicional demonstrado.** | Cadeia em `models.py:806`, `app/chamados/signals.py:216`, `app/ponto/services/cartorio.py:1008`, `fabrica_da_celula.py:219`; escrita do dia em `fio.py:800`; leitura na materialização em `materializacao.py:1076` e `:1345`. Execução real **[não verificado]**. | — | — |
| Doubles certificarem ORM, concorrência, produção ou suíte inteira | **DESCARTADO como certificação; não é defeito funcional adicional.** | Limite declarado em `app/chamados/tests/test_bo_disputa_volta5m_memoria.py:6`. Essas fronteiras **[não verificado]**. | — | — |

**PARA O CONSTRUTOR c22**

- **P1/P2:** governar a seleção/proteção nos pontos compartilhados pelos dois escritores; corrigir somente novos chamadores deixa reuso direto desprotegido. Preservar o encaminhamento à dona quando a canônica é devolvida. (`models.py:2980`; `fio.py:909`; `disputa_emissao.py:1065`.)
- **P4/P5:** assegurar trilha dos motivos descartados também após dedup e falha de gravação; preservar a ausência de registros de ensaios desfeitos. (`disputa_emissao.py:1050`; `:1332`; `descarte_disputa.py:37`; `app/chamados/tests/test_bo_disputa_volta5g_memoria.py:198`.)
- **P6/P7:** separar causa técnica de ausência comprovada de ata; eliminar fechamento sem testemunho nos retornos antecipados, preservando HX e h2. (`disputa_emissao.py:1488`; `:1562`; `declaracao_texto.py:71`; `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:59`; `:78`.)
- **P8/P9:** tornar emissão/agregação de pendentes um ato comum e adiar push até confirmação externa. (`disputa_emissao.py:344`; `:354`; `:1307`; `:1574`.) [nao verificado]
- **Reincidência comprovável pelo anexo:** P1 repete a classe do achado 1; P2 introduz outra eleição paralela na classe do achado 2; P7 mantém a classe de fechamento sem ata mencionada no item 5. Os demais resíduos antecedem a c21 pelo delta, mas sua conferência na triagem5 **[não verificado]**.
- Reexecução nesta leitura: **122 testes de memória e 35 da catraca passaram**; mutantes c21 produziram **3/4/1/2/1/4/1 failures**, c20 **5/5/1/1**, antigos **1/1**, todos sem errors; registro antigo da catraca produziu **1 failure, zero errors**. Runners: `app/chamados/tests/test_bo_disputa_volta5m_memoria.py:197`; `app/chamados/tests/test_bo_disputa_volta5l_memoria.py:146`; `app/chamados/tests/test_bo_disputa_volta5_memoria.py:386`; `app/core/tests/test_medida_turno_do_par.py:540`. Suíte Django inteira/protegido ORM **[não verificado]**.

**PARA O OPUS (models.py ou lei)**

- **P3 — `models.py:2866`: menor hunk para a seleção do replay, uma linha substituída:**

```diff
-                _bo = campos.get('batida_origem_id') or cls.batida_origem_do_nascimento(disputa, motivo)
+                _bo = campos['batida_origem_id'] if 'batida_origem_id' in campos else cls.batida_origem_do_nascimento(disputa, motivo)
```

  **Isso não certifica cura integral em uma linha:** a inserção ainda rederiva origem nula em `app/chamados/models.py:3448` e `:3449`.

- **L1 — lei somente para manter a extensão técnica do veto:** falta de data/base, marco inválido e falha de leitura não receberam comportamento literal nos avais fornecidos **[não verificado]**. O veto existe em `app/chamados/services/disputa_emissao.py:1562`; restaurar o escopo HX/C7 cabe no emissor.
- **I23 com exit 2 e push por dona estrangeira:** requisitos adicionais não demonstrados nos avais **[não verificado]**; não sustentam bloqueio por si. (`app/chamados/management/commands/auditar_invariantes_chamados.py:95`; `:1942`; `app/chamados/services/disputa_emissao.py:664`; `:894`.)

## 11/11 02:00 · sessao d769e813e2ca · FIO-E0-V4-E4B · leitura · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 11/11 01:59 · sessao 62cff88166d0 · - · - · origem=Ronald (tmux)

| Achado conferido | Veredito proposto | Prova e origem | Cura fora dos proibidos? |
|---|---|---|---|
| Replay conserva argumento `None`, mas porta rederiva origem | **PROCEDENTE**, sem afirmar persistência de pergunta divergente | `disputa_emissao.py:545/:766` conserva `None`; `models.py:2866` usa `None or derivador(dona)`. A divergência criada/PK diferente é desfeita em `disputa_emissao.py:767-769`. | **Não**, para corrigir a distinção na porta. Origem exclusiva desse defeito: `models.py:2866`. |
| Trilha pós-commit pode faltar e I23 continuar zero | **PROCEDENTE**, limitado à falha injetada; ocorrência produtiva **[não verificado]** | `descarte_disputa.py:19/:37`; `core/observ.py:111/:139-145`; I23 lê exclusivamente trilha em `auditar_invariantes_chamados.py:81-86`. Há ERROR/contador de engolidos, portanto não dizer “totalmente calado”: `core/observ.py:79-87`. | **Sim**, serviço de descarte; preservar ausência de trilha de ensaio desfeito, coberta em `test_bo_disputa_volta5g_memoria.py:198-207`. |
| Dedup impede trilha em dona fechada | **PROCEDENTE** | `_ja_perguntou` retorna antes do encaminhamento em `disputa_emissao.py:1050-1051`; complemento mantém `criou=False` em `:1116`; agregador só é chamado sob `_cc` em `:1332-1333`. | **Sim**, `disputa_emissao.py`. |
| I23 isolado não produz exit 2 | **DESCARTADO como bloqueio:** alarme textual já coberto; exigir exit 2 é **LEI** adicional | I23 positivo aparece `VIOL` em `auditar_invariantes_chamados.py:89-95/:1924-1927`; só F1/F2 provocam exit 2 em `:1942-1945`. | Não requer cura pelo aval fornecido **[não verificado]**. |
| Pendentes emitem e agregam em transações separadas | **PROCEDENTE**, pela inconsistência concreta, não apenas pela localização do código | Emissão em `disputa_emissao.py:340/:344`; agregação posterior em `:351/:354`; wrapper chama após núcleo em `:893`. Escrita dos motivos em `:616/:622`. | **Sim**, atomic/trava no serviço envolvendo emissão e agregação. |
| Push estrangeiro misto não recebe novos motivos da dona | **DESCARTADO como bloqueio por escopo**; comportamento confirmado | `disputa_emissao.py:664` descarta retorno; `:706` devolve novos locais; `:894/:897-900` usa somente esses. Política adicional de push por dona não está nos avais **[não verificado]**. | Tecnicamente sim; autorização dessa política **[não verificado]**. | [nao verificado]
| Push ocorre antes do commit externo | **PROCEDENTE**, como efeito externo sobrevivendo ao rollback | Atomic em `disputa_emissao.py:1285`; envio em `:1307/:1574`; envio síncrono real em `push_utils.py:40-48` e `fcm_utils.py:85`. Bug no caminho deve ser tratado: `CLAUDE.md:30`. | **Sim**, agendamento pós-commit no serviço emissor. | [nao verificado]
| Declaração descarta primeiro booleano `(p, criada)` | **DESCARTADO: leitura literal sem saída errada demonstrada** | `disputa_emissao.py:834` descarta booleano, mas confere dona em `:835`; replay confere criação e identidade em `:842-844`. | Não requer cura. |
| Exceção do gerador e nascimento vazio do núcleo | **DESCARTADO: já cobertos** | Atomic do conjunto em `disputa_emissao.py:679/:709/:730`; nascimento sem motivos locais é desfeito em `:737-739`. | Não requer cura. |
| Replay divergente analisa chamado recusado | **DESCARTADO: já coberto nesse ramo** | `disputa_emissao.py:1535-1537` desfaz divergência; `:1545/:1547` exige confirmação sem recusa antes de analisar. | Não requer cura. |

Inputs concretos dos procedentes:

- **Origem `None`:** `motivo='foto_ausente'`, argumento explícito `batida_origem_id=None`, dona com `contexto={'ultima_batida':{'id':222}}`, batida 222 válida do colaborador. `models.py:2866` resolve 222, alterando o episódio conservado; a origem entra no julgamento e universo em `models.py:2875-2878/:2907`. Se resultar criada/PK diferente, o replay é recusado, não persistido: `disputa_emissao.py:767-773`.
- **Dedup fechado:** dona fechada, `motivos=['anterior']`; P viva `intervalo_saida` no dia D destinada ao intervalo I; ata de D acusa esse mesmo intervalo; lote `[I]`. O complemento encontra dedup, `_cc=False`, não registra descarte e o filtro remove I, retornando `None`: `disputa_emissao.py:1320/:1331-1346`. Reproduzido com AST existente: retorno `None`, `audit=[]`, motivos intactos; ORM **[não verificado]**. O juiz pode considerar P viva sem usar fechamento da dona: `juizes.py:1324-1400`; dedup consulta a dona recebida: `disputa_emissao.py:1698-1705`.
- **Falha da trilha:** descarte confirmado de motivo novo em dona fechada; falha na inserção `LogAuditoria.objects.create`. `evento` retorna `None`, o descarte já passou do commit e I23 não tem linha para contar: `descarte_disputa.py:37`, `core/observ.py:139-145`, `auditar_invariantes_chamados.py:81-86`.
- **Pendentes:** DS aberta existente com `motivos=['anterior']`; pendente em D classifica `intervalo_saida`; emissão cria P; save da agregação levanta `RuntimeError`. Ensaio AST terminou com P persistida e DS ainda `['anterior']`; ORM **[não verificado]**. Ordem produtiva que permite isso: `disputa_emissao.py:340/:344/:354`, escritor `:616/:622`.
- **Push antes do commit:** chamador envolve repasse em atomic externo, repasse envia push e depois o chamador levanta `ValueError`. Dados desfazem, envio externo não: `disputa_emissao.py:1285/:1307/:1574`, `push_utils.py:40-48`. [nao verificado]

Menor hunk para **replay de pergunta existente**, em `app/chamados/models.py:2866`, **uma linha substituída**:

```diff
-                _bo = campos.get('batida_origem_id') or cls.batida_origem_do_nascimento(disputa, motivo)
+                _bo = campos['batida_origem_id'] if 'batida_origem_id' in campos else cls.batida_origem_do_nascimento(disputa, motivo)
```

Isso distingue `None` explícito de argumento ausente no replay; cura universal de nascimento exige conferir também a rederivação de `None` na inserção em `models.py:3448-3449`.

Todos os resíduos acima antecedem a c21 segundo o delta fornecido **[não verificado quanto ao Git]**. A triagem_preverif5 fornecida só valida seus achados 1 e 2 e declara 3–6 não conferidos; portanto, atribuir replay/auditoria/push/pendentes a achados já efetivamente triados na preverif5 é **[não verificado]**. [nao verificado]

## 11/11 01:57 · sessao c24104626e4d · - · - · origem=Ronald (tmux)

| Achado | Veredito | Prova/input | Origem e cura fora dos proibidos |
|---|---|---|---|
| `[ch]` recostura P de intervalo externo no genérico | **PROCEDENTE; mesma classe do achado 1 da triagem5.** | Índice só fotografa o lote (`app/chamados/services/disputa_emissao.py:1296`); ausência da chave conserva `ch` (`:1044`); reuso atribui/salva (`app/chamados/models.py:2982`, `:3006`). Input: P `intervalo_saida`/D destinada ao intervalo PK1 em dona estrangeira; lote contém apenas genérico PK2. Ensaio em memória retornou destino PK2, em dona aberta e fechada; ORM **[não verificado]**. Double explicitamente descrito em `app/chamados/tests/test_bo_disputa_volta5l_memoria.py:64`. | Escritor efetivo desse input: `models.py:2982`. **Cabe fora:** a guarda compartilhada `_costura_nao_mata_o_fio` já é chamada pelo reuso (`models.py:2980`) e pelo outro escritor (`fio.py:909`); retornar falso preserva destino e mantém retorno da canônica (`models.py:2984`, `:3008`). Implementação/ORM da cura **[não verificado]**. |
| Lote escolhe primeiro; avulso escolhe menor PK | **PROCEDENTE como divergência estrutural; mecanismo NOVO c21, mesma classe de derivação paralela do achado 2.** | `setdefault` e ordem do lote (`disputa_emissao.py:1297`, `:1299`) versus `sorted(...pk)` (`:291`). Input: dois intervalos PK1/PK2 com `S`/D; lote `[PK2,PK1]` produz destino PK2; helper avulso retorna PK1. Ensaio em memória confirmado; ORM **[não verificado]**. Coerência/determinismo exigidos em `CLAUDE.md:176`. | Eleições escrevem pedidos em `disputa_emissao.py:293`, `:1299`; persistências em `models.py:2982`, `fio.py:925`. **Cabe fora:** unificar eleição e proteção na autoridade compartilhada permitida; qual PK deve vencer por mérito **[não verificado]**. |
| Replay conserva argumento `None`, mas porta rederiva | **PROCEDENTE como propriedade do código; residual anterior à c21.** | Gerador conserva origem (`disputa_emissao.py:545`), replay repassa argumentos (`:766`), mas `None or derivador(dona)` reconsulta dona (`models.py:2866`). Input concreto: `motivo='foto_ausente'`, argumento `batida_origem_id=None`, dona com `contexto={'ultima_batida':{'id':71}}`, batida71 do colaborador existente: `_bo=71`, não `None` (`models.py:3461`, `:3463`, `:3467`; família em `app/chamados/catalogo/motivos.py:43`). Reprodução ORM **[não verificado]**. | Seleção do replay: `models.py:2866`, arquivo proibido. Há também rederivação na inserção (`models.py:3448`, `:3449`). **Não certificar cura integral em uma linha.** |
| Derivação duplicada índice/B | **DESCARTADO: já coberto pela fotografia c21.** | Uma fotografia em `disputa_emissao.py:1296`, consumida no índice (`:1298`) e B (`:1398`). | — |
| Conversão civil→turno nesse helper | **DESCARTADO: leitura errada.** | `dia_do_turno_da_celula` retorna `DiaDoTurno(dia)` (`app/ponto/turnos.py:2943`), `NewType` (`app/core/dia_do_turno.py:13`). | — |
| Regularizável pode ter fotografia vazia | **DESCARTADO como defeito autônomo: descrição verdadeira, sem input admissível obrigatório demonstrado.** | Regularizabilidade delegada à fábrica (`disputa_emissao.py:1009`), helper exclui data inválida (`:274`). Input `marcos_faltantes=[{'tipo':'S','data':'invalida'}]` retorna `()` (`:273`, `:275`, `:278`). | Exigência de perguntar apesar de dados inválidos **[não verificado]**. |
| Helper não total para qualquer JSON | **DESCARTADO como novidade/bloqueio desta cura: pré-condições já estavam no código removido pelo patch anexo [não verificado].** | Input `contexto_json=['truthy']` gera AttributeError; `{'marcos_faltantes':7}` gera TypeError no `for`, ambos fora do try (`disputa_emissao.py:269`, `:272`). | Situação produtiva **[não verificado]**; não nasceu pela captura estreita de c21. |
| Troca de `Exception` por T/V altera objetos extraordinários | **DESCARTADO: fora do JSON ordinário; nenhuma ocorrência concreta fornecida.** | `dict` extraordinário cujo `.get('data')` lança RuntimeError propagaria (`disputa_emissao.py:273`, `:274`); entrada persistida desse tipo **[não verificado]**. | — |
| Tipo diferente de `S` vira volta | **DESCARTADO como novidade: comportamento preservado do patch anexo [não verificado].** | Input `{'tipo':'X','data':'2026-07-08'}` produz `intervalo_volta` (`disputa_emissao.py:276`). Obrigação de outro tratamento **[não verificado]**. | — |
| Detector escreve data civil potencialmente D+1 | **DESCARTADO como escritor persistente atual: emissão aposentada.** | Data encadeada civil em `app/ponto/management/commands/detectar_intervalo_ausente.py:82`; aposentadoria e `continue` em `:149`, `:156`. | — |
| Booleano da abertura inicial da declaração descartado | **DESCARTADO: booleano não é necessário para a decisão executada.** | Primeiro retorno confere a dona (`disputa_emissao.py:834`, `:835`); replay confere criação e identidade (`:842`, `:843`). Input que cause saída errada apenas por ignorar booleano **[não verificado]**. | — |
| Novos escritores persistentes fora da porta | **DESCARTADO: censo confirma escritores já registrados.** | `models.py:2982`, `:3014`; `fio.py:925`; contrato em `app/core/tests/test_contract_escritores_estado.py:61`. | Defeito é regra não aplicada aos escritores, não escritor novo. |
| Campos com dois significados | **DESCARTADO como acusação genérica; PROCEDENTE apenas na oscilação de destino já contabilizada.** | Cenários DS (`models.py:1743`) e identidade P (`:2427`) são campos distintos; conversa usa porta/data própria (`:2794`, `:2801`). | Oscilação fica no primeiro achado. |

Menor hunk **para impedir especificamente a rederivação de `None` na seleção do replay**, uma linha em `app/chamados/models.py:2866`:

```diff
-                _bo = campos.get('batida_origem_id') or cls.batida_origem_do_nascimento(disputa, motivo)
+                _bo = campos['batida_origem_id'] if 'batida_origem_id' in campos else cls.batida_origem_do_nascimento(disputa, motivo)
```

Esse hunk não certifica a inserção, porque `save` chama `_chave_de_identidade` (`models.py:3420`, `:3421`) e ela rederiva origem nula (`:3448`, `:3449`).

Checklist de referências neutras conferidas: atribuições temporárias/restauradas (`fio.py:779`, `:783`); reatribuição do mesmo PK (`models.py:366`, `:367`); FK SET_NULL (`:3387`); declarações colab/admin (`declaracao_texto.py:365`, `:653`); cadeia estática analisar→signal→cartório→fábrica (`models.py:806`, `signals.py:216`, `app/ponto/services/cartorio.py:1008`, `fabrica_da_celula.py:172`, `:174`, `:219`); contexto persistido (`contexto.py:76`, `:102`); grade usa categoria Ausencia e marcos sem data (`reconciliar_grade.py:498`, `:502`); data atribuída por porta (`fio.py:800`); materialização lê destino/data (`materializacao.py:1076`, `:1079`, `:1345`). Execução real dessa cadeia e efeitos em produção **[não verificado]**.

## 11/11 01:56 · sessao 40cf756ad74a · - · - · origem=Ronald (tmux)

| Achado | Veredito | Prova/origem | Fora dos proibidos? |
|---|---|---|---|
| `motivo=None` permite fechar sem consultar ata | **PROCEDENTE**, mesma classe da triagem5 item 5; não introduzido pela c21. Input: chamado genérico datado, `_tem_base=True`, `_classificar_falta → (None,None,None)`, ata ausente. Resultado no ensaio AST: fechada, zero consultas à ata, zero luz. ORM/prod **[não verificado]**. | `app/chamados/services/disputa_emissao.py:1487-1490` pula a leitura; `:1565-1570` solicita fechamento; escritor de `fechada_em`: `app/chamados/models.py:1932`. A HX exige conservar aberta sem ata: `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:51-61`. | **Sim**: gate/ordenação no emissor, antes dos `continue` em `:1447` e `:1488`. |
| Veto adicional para sem-data/base/marco inválido/falha de leitura | **LEI para conservar essa extensão**; autorização literal específica **[não verificado]** nos avais fornecidos. É novo no diff c21 **[não verificado: anexo sem linhas]**. | Flags em `app/chamados/services/disputa_emissao.py:1097`, `:1103`, `:1397`, `:1453`, `:1482`, `:1485`; veto em `:1562`. Juiz único declarado: `app/chamados/models.py:1848-1853`, `:1858-1863`; `app/docs/JUIZES.md:62`; escopo literal `CLAUDE.md:37`. | **Sim** para restaurar o escopo HX/C7 existente no emissor; manter a extensão exige decisão de lei. |
| Aviso afirma ata ausente sem ter consultado ata | **PROCEDENTE**, novo c21 **[não verificado: diff anexo sem linhas]**. Input: chamado datado, sem origem, `_tem_base=False`, ata existente/vazia. Ensaio AST: aberta, zero consultas à ata, aviso acionado. | Origem da classificação indevida: `app/chamados/services/disputa_emissao.py:1485`, `:1562-1564`; mensagem persistida somente no logger: `app/chamados/services/declaracao_texto.py:70-72`. | **Sim**: emissor/porta da luz. |
| I7a/I7b não leem o WARNING | **DESCARTADO** como defeito: o aval exige contadores do estado da disputa, não interpretação de log **[não verificado: anexo sem linhas]**. | Contadores consultam abertas sem linhas P e a autoridade da ata: `app/chamados/management/commands/auditar_invariantes_chamados.py:62-74`. | — |
| Extensão técnica não tem contador próprio | **DESCARTADO**: ausência de contador exclusivo não significa falta de leitura; aberta sem P e sem dia entra I7a. | `app/chamados/management/commands/auditar_invariantes_chamados.py:64-73`. | — |
| H2 com ata lida e nenhum faltante deixou de fechar | **DESCARTADO**, já coberto. | Contraste protegido: `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:63-79`; caso c21: `app/chamados/tests/test_bo_disputa_volta5m_memoria.py:185-191`. | — |
| Regularizável pode ter fotografia vazia | **DESCARTADO como bloqueador autônomo**: leitura correta, mas não prova violação adicional dos avais. | Regularizabilidade trata admissão pela fábrica: `app/chamados/services/disputa_emissao.py:1003-1009`; fotografia exige dados válidos: `:266-276`. | — |
| Helper não total para qualquer JSON | **DESCARTADO como achado novo**: `.get` e iteração desprotegidos já existiam no índice do patch fornecido **[não verificado: anexo sem linhas]**; ocorrência produtiva **[não verificado]**. | Sítios atuais: `app/chamados/services/disputa_emissao.py:269-274`. | — |
| `RuntimeError` extraordinário deixou de ser capturado no B | **DESCARTADO**: exemplo demanda objeto extraordinário, não JSON ordinário; não demonstra regressão global nem ocorrência na produção **[não verificado]**. | Captura atual `app/chamados/services/disputa_emissao.py:272-275`; captura antiga somente no patch anexo **[não verificado]**. | — |
| Tipo diferente de `S` vira volta | **DESCARTADO como novo defeito**: comportamento mantido pelo patch **[não verificado: anexo sem linhas]**; regra autorizando rejeição de outros tipos **[não verificado]**. | `app/chamados/services/disputa_emissao.py:276`. | — |
| Conversão civil→turno nesta derivação | **DESCARTADO**, leitura errada. | `app/core/dia_do_turno.py:13`; identidade em `app/ponto/turnos.py:2939-2943`. | — |
| Detector prova procedência atual de `marcos[].data` | **DESCARTADO**: a emissão desse detector está aposentada. | `app/ponto/management/commands/detectar_intervalo_ausente.py:149-156`. | — |

**Para c22:** corrigir o caminho sem testemunho de ata antes dos `continue`, preservar o contraste protegido HX/h2 e corrigir a falsa mensagem de ausência de ata. (`app/chamados/services/disputa_emissao.py:1447`, `:1488`, `:1562-1567`; `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:51-79`.)

**Para Opus:** decidir somente se pretende manter uma nova regra de conservação da disputa para sem-data/base/marco inválido/falha técnica; essa extensão específica não está escrita nos avais fornecidos **[não verificado]**. Não há necessidade demonstrada de editar `models.py` para corrigir os achados deste recorte. (`app/chamados/services/disputa_emissao.py:1485`, `:1562`; juiz existente em `app/chamados/models.py:1848-1853`.)

## 11/11 01:56 · sessao dcc8ff78aebf · - · - · origem=Ronald (tmux)

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 11/11 01:56 · sessao 69cf86931e4d · - · - · origem=Ronald (tmux)

Triagem referenciada para consolidação:

- **Harness:** não basta substituir `sys.modules`. O teste instala filhos falsos (`django.db`, `escala.models`, `chamados.services.nota`) em `app/chamados/tests/test_e4b_classe_e_memoria.py:189`, mas a produção importa `nota` e `tarefa_do_dia` pelo pacote pai em `app/chamados/services/disputa_emissao.py:1810` e `app/chamados/management/commands/backfill_fio_morto_e4b.py:80`. Atributos preexistentes no pai podem devolver o módulo real, enquanto seus imports internos encontram filhos falsos. O double de `django.db` não tem `IntegrityError` (`test_e4b_classe_e_memoria.py:191`), e o de escala não tem `EscalaColaborador` (`test_e4b_classe_e_memoria.py:204`); os imports reais falham em `app/chamados/services/nota.py:83` e `app/ponto/turnos.py:2975`.

- **Reprodução em memória:** o teste `test_P3_carga_inclui_validada_ressuscitavel` passa sozinho; pré-carregando no pacote pai `chamados.services.nota` com o corpo real de `anexar_ocorrencia`, produz exatamente `ImportError: cannot import name 'IntegrityError' from '<unknown module name>'`. Caminho: `app/chamados/tests/test_e4b_classe_e_memoria.py:580` → `:486` → `app/chamados/services/disputa_emissao.py:1846` → `app/chamados/services/nota.py:83`. A execução usou `PYTHONDONTWRITEBYTECODE=1`, sem escrita.

- **Cura mínima do harness:** contexto único que substitua e restaure tanto filhos em `sys.modules` quanto atributos correspondentes dos pacotes pais; usar esse contexto em `resolver`, `_porta`, `_comando` e nos blocos próprios de testes. Esses pontos hoje usam somente `patch.dict` em `app/chamados/tests/test_e4b_classe_e_memoria.py:235`, `:413`, `:479`, `:503`. Não remediar importando modelos reais ou acrescentando apenas `IntegrityError`: isso preservaria a fuga para código real. As duas fugas são demonstradas pelos imports anteriores.

- **Contrato de vitalidade:** o juiz é `chamados.juizes.pergunta_viva`/`q_vivas`, explicitamente em `app/chamados/tests/test_contract_leitores_vitalidade.py:2` e `:321`; a porta E4B pergunta corretamente a C6 em `app/chamados/services/disputa_emissao.py:1812`. O ofensor novo `:1789` entrou no delta c3.

- **Ressalva decisiva para a cura do contrato:** `:1789` enumera carga de escrita, reproduzindo o conjunto efetivamente relavrado por `_gravar_estado` em `app/chamados/models.py:309`. Substituí-lo por `q_vivas` pode omitir perguntas mortas por C6 que o escritor ainda relavra; C6 também mata por tranca, identidade, fato e célula, além de validação (`app/chamados/juizes.py:1324`). O contrato reconhece exceções estritas para enumeração/elegibilidade de escritor (`app/chamados/tests/test_contract_leitores_vitalidade.py:107`, `:132`) e explica por que substituir uma guarda de escritor por C6 pode mudar sua pergunta (`:62`). Cura coerente: compartilhar o seletor do escritor mediante hunk autorizado em `models.py`, ou justificar uma entrada estrita da função na allowlist como **carga de escrita**, mantendo C6 na decisão de vitalidade e acrescentando contraprova de carga completa. Não maquiar a comparação para escapar do AST.

- **Sumido F1 h:** força K6 entregue e exige chamado resolvido em `app/chamados/tests/test_o261e_f1_porta_encerramento.py:113`. O c3 introduziu consulta de competência e motivo impeditivo em `app/chamados/services/disputa_emissao.py:1826`, e somente chama reabrir se não houver motivo em `:1840`. O S143 também interrompe a sequência após essa recusa em `:1914`. Portanto existe mudança comportamental pertinente ao sumiço. O teste usa relógio congelado (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:46`). Sem traceback baseline, qual assert falhava antes: **[nao verificado]**; intermitência: **[nao verificado]**.

- **Sumido R4b:** cria exportação vigente em `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:101`, reata e exige preservar `resolvido` em `:312`. A cerca nova acima impede renascimento por E4B; a composição K6 lê a exportação vigente por janela em `app/chamados/services/vigia_fio.py:822`. Compatível com correção comportamental do c3. Causa histórica exata da falha suite1 e intermitência: **[nao verificado]**.

- **Não confirmar “24 comuns sem tocar delta”:** os 14 ERRORs comuns são do próprio arquivo alterado `app/chamados/tests/test_e4b_classe_e_memoria.py`; os tracebacks dados atravessam `app/chamados/services/disputa_emissao.py:1913`, `:1841`, `:1846` ou `app/chamados/management/commands/backfill_fio_morto_e4b.py:83`. Portanto essa afirmação é falsa. Os 24 comuns se decompõem em 14 ERRORs do harness e 10 FAILs; os dois conflitos de passivo continuam em `app/chamados/tests/test_e4_passivo.py:62` e `:95`. A pertença aos conjuntos vem dos anexos sem numeração: **[nao verificado]** como referência arquivo:linha.

## 11/11 01:56 · sessao 16c3dd523cd9 · - · - · origem=Ronald (tmux)

**1+4: alcance da antecipação e da reversão insuficiente.** O backfill expande a carga somente das âncoras das perguntas do dia; os demais chamados vivos entram em `chs` sem expansão das perguntas/disputas que a ressuscitação poderá escrever (`app/chamados/management/commands/backfill_fio_morto_e4b.py:197`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:200`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:206`).

O caminho completo está no código: `reabrir()` → `_gravar_estado()` → `save()` → receiver → `julgar_celula()` → `x.ressuscitar_perguntas()` para outros vivos do dia (`app/chamados/models.py:1442`, `app/chamados/models.py:296`, `app/chamados/signals.py:216`, `app/ponto/services/cartorio.py:1602`, `app/ponto/services/cartorio.py:1633`). A autoridade da ressuscitação pode devolver perguntas de outro dia e reabrir disputas fechadas: o seletor parte de `perguntas_que_resolvem`, sem recorte de data, e a escrita chama `reabrir_sistema()` (`app/chamados/models.py:1483`, `app/chamados/models.py:1566`, `app/chamados/models.py:1574`).

Existe ainda uma segunda expansão que o brief deve exigir explicitamente: havendo adiados pela lâmpada, o cartório chama E2 para o colaborador inteiro; seu queryset não filtra data/competência, o juiz filtra em Python, a porta carimba e pode fechar a disputa (`app/ponto/services/cartorio.py:962`, `app/ponto/services/cartorio.py:970`, `app/chamados/reconciliador.py:627`, `app/chamados/reconciliador.py:661`, `app/chamados/reconciliador.py:595`, `app/chamados/reconciliador.py:611`). Portanto, “expandir todas as cargas dos vivos” cura o contraexemplo anunciado, mas sua suficiência para todas as escritas da cascata permanece **[nao verificado]**.

A ordem canônica é C→D→Ch→P; cada grupo em PK crescente, e `trava_na_ordem` é o único adquirente das quatro famílias (`app/chamados/models.py:2631`, `app/chamados/models.py:2666`, `app/chamados/models.py:2675`, `app/chamados/models.py:2688`, `app/chamados/models.py:2692`). A expansão deve terminar **antes** da aquisição de D/Ch/P, e a cerca de exportada/limbo precisa consultar essa mesma carga, não somente a carga do credor direto (`app/chamados/management/commands/backfill_fio_morto_e4b.py:208`, `app/chamados/services/disputa_emissao.py:1820`, `app/chamados/services/disputa_emissao.py:1826`, `app/chamados/services/disputa_emissao.py:1828`).

**Reversão:** `_foto_do_dia()` fotografa somente P do dia; é chamada antes de entrar no atomic e antes de `_travar_lote()` (`app/chamados/management/commands/backfill_fio_morto_e4b.py:215`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:303`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:307`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:310`). Deve fotografar sob trava a carga efetiva inteira e reler a reversão antes de escrever, preservando o requisito já implementado em `_gravar_reversao()` (`app/chamados/management/commands/backfill_fio_morto_e4b.py:52`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:59`).

Campos cuja fotografia atual é insuficiente:

| Família | Escrita a preservar |
|---|---|
| P: assinatura e carimbo | Ressuscitação limpa `validada_em`, `validada_por`, `respondida_em` e descarimba; fotografia inclui `validada_em`, mas omite `validada_por` (`app/chamados/models.py:1566`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:219`). |
| P: lavra | S2 relavra perguntas de destino não validadas; `lavrar` escreve `veredito_lavrado`, `julgado_em`, `janela_expira_em`; trio ausente da fotografia (`app/chamados/models.py:309`, `app/chamados/veredito_lavrado.py:270`, `app/chamados/veredito_lavrado.py:275`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:219`). |
| D: fechamento | Reabertura limpa `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento`; fotografia registra apenas IDs das disputas abertas (`app/chamados/models.py:2173`, `app/chamados/models.py:2177`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:225`). |
| Ch: ciclo e urgência | Núcleo limpa instante/via e restaura urgência; fotografia contempla esses campos somente nos credores derivados das P do dia (`app/chamados/models.py:248`, `app/chamados/models.py:253`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:230`). |

**Decisão de sítio para 1+4:** a sessão deve escolher entre preservar a cascata e antecipar sua carga completa, ou recortar formalmente a cascata acionada pela porta E4B; a alteração cabe nos pontos `backfill:_travar_lote`, snapshot e cerca, ou nos chamadores da cascata (`app/chamados/management/commands/backfill_fio_morto_e4b.py:179`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:211`, `app/chamados/services/disputa_emissao.py:1820`, `app/chamados/signals.py:216`, `app/ponto/services/cartorio.py:970`, `app/ponto/services/cartorio.py:1633`). Há `cartorio.calado()` existente, mas sua finalidade declarada é competência **trancada**, e ele faz o receiver pular o julgamento; reaproveitá-lo para a competência aberta exige decisão sobre os efeitos que deixarão de ocorrer (`app/ponto/services/cartorio.py:90`, `app/ponto/services/cartorio.py:92`, `app/chamados/signals.py:210`).

**2: corrida de exportação — nenhum protocolo existente na cópia fecha a janela.** K6 faz leitura simples dos registros; `registrar()` faz `create()` sem passar pelo protocolo C/D/Ch/P (`app/folha/export.py:581`, `app/folha/models.py:178`, `app/folha/models.py:182`). Repetir K6 imediatamente antes de `reabrir()` conserva o intervalo entre leitura e escrita (`app/chamados/services/disputa_emissao.py:1826`, `app/chamados/services/disputa_emissao.py:1840`). Trancar apenas registros ED existentes não cobre a primeira exportação de uma competência sem linha; prova concorrente disso na cópia **[nao verificado]**.

Os dois chamadores de produção encontrados chamam diretamente `registrar()`; mudar somente o request não cobre o comando de regeração (`app/folha/views.py:297`, `app/folha/management/commands/regerar_txt_dominio.py:118`). `folha/porta_export.py` é conferência de carimbo/medição, não a porta comum do registro (`app/folha/porta_export.py:7`, `app/folha/porta_export.py:14`).

Menor **cura existente** fora de `models.py`: **não identificada [nao verificado]**. Uma trava física nova da tabela ED no topo do atomic do backfill poderia ser candidata fora de models; sua compatibilidade, abrangência e segurança concorrente não estão demonstradas pela cópia **[nao verificado]**. Logo o brief deve escrever: **“decisão do opus/hunk da sessão: protocolo compartilhado com `ExportacaoDominio.registrar`, sítio `app/folha/models.py:174`, antes do `create` em `app/folha/models.py:182`; ou autorizar explicitamente protocolo alternativo fora de models, com prova concorrente e ordem global. Não considerar reconferência K6 como cura.”** A necessidade decorre dos sítios de leitura/escrita atuais (`app/folha/export.py:581`, `app/folha/models.py:182`).

**REDs concretos para o brief:**

- Outro vivo Ch12, P90 ressuscitável fora do dia e D90 fechada: provar inclusão de P90/D90 antes de qualquer P, cerca de competência/limbo e reversão de todos os campos; a composição capaz de escrever existe nos sítios acima (`app/ponto/services/cartorio.py:1633`, `app/chamados/models.py:1562`, `app/chamados/models.py:1574`).
- Adiados pela lâmpada com P absolvida de outro dia: provar que o alcance E2 não escapa da antecipação/cerca/snapshot (`app/ponto/services/cartorio.py:970`, `app/chamados/reconciliador.py:627`, `app/chamados/reconciliador.py:595`).
- Primeira exportação entre última consulta K6 e `reabrir`: duas conexões e barreiras, exigir serialização pelo protocolo decidido; execução real **[nao verificado]** (`app/folha/models.py:182`, `app/chamados/services/disputa_emissao.py:1826`, `app/chamados/services/disputa_emissao.py:1840`).
- Snapshot sob trava, round trip tipado, preservando lavra, assinatura, disputa fechada reaberta e urgência, com ato compensatório para `DecisaoPergunta`, conforme contrato já declarado (`app/chamados/management/commands/backfill_fio_morto_e4b.py:31`, `app/chamados/veredito_lavrado.py:270`, `app/chamados/models.py:1566`, `app/chamados/models.py:2177`).

MAPA exato da fila: `FIO-E0-V4-E4B`, fila 1, backfill classes a–e por porta canônica, DRY default, competência 11 primeiro e exportadas em lista para `!` (`app/docs/BACKLOG.md:1875`).

## 11/11 01:54 · sessao b789c3943605 · BO-DISPUTA-SEM-PERGUNTA · verificador · origem=Code

**NAO LIBERA.** Ainda há caminho que troca o destino de uma pergunta de intervalo para o chamado genérico. A c21 corrige pedidos do gerador e da declaração, mas não estabelece a regra nos escritores; também acrescenta um veto ao autofechamento cuja autorização literal não está demonstrada. (`app/chamados/services/disputa_emissao.py:542`, `:824`, `:1044`, `:1296`, `:1562`; `app/chamados/models.py:2982`.)

Referências abreviadas: serviços em `app/chamados/services/`; `models.py` em `app/chamados/`; testes de disputa em `app/chamados/tests/`; auditor em `app/chamados/management/commands/auditar_invariantes_chamados.py`.

**1. Estado dos achados da preverif5.r e das duas falhas**

| Achado | Estado em 9bba8132b e fundamento |
|---|---|
| Exceção do gerador deixar nascimento incompleto | **Curado, mantido:** nascimento e geração estão no atomic; exceção propagada desfaz o conjunto. `disputa_emissao.py:679`, `:709`, `:730`. |
| Geração vazia confirmar nascimento no núcleo | **Curado, mantido:** ausência de motivo local admitido desfaz o nascimento. Não elimina o h2 autorizado do agrupador. `disputa_emissao.py:737`, `:739`, `:1565`. |
| Canônica estrangeira deixar nova DS sem pergunta | **Curado quanto ao nascimento; parcial quanto à costura/replay:** rollback local permanece; destino e origem ainda têm os resíduos abaixo. `disputa_emissao.py:737`, `:766`, `:1524`; `models.py:2866`, `:2982`. |
| Declaração pública aceitar ausência de pergunta | **Curado, mantido:** porta pública exige a pergunta; ensaio sem emissão permanece separado. `disputa_emissao.py:779`, `:803`, `:807`. |
| Seed estrangeiro indevidamente admitido na local | **Curado para admissão nova:** a guarda continua específica ao reuso estrangeiro de `orfao_14h`; não limpa acervo anterior. `disputa_emissao.py:1352`, `:1506`, `:1508`. |
| Segundo agregador na âncora | **Curado, mantido:** agregação permanece na porta comum. `disputa_emissao.py:602`, `:1652`. |
| Luz HX sem leitor | **Parcial:** I7a/I7b medem disputas abertas sem nenhuma P; não leem o WARNING. A extensão técnica da c21 não ganhou contador próprio. `auditar_invariantes_chamados.py:62`, `:69`, `:73`; `declaracao_texto.py:63`; `disputa_emissao.py:1562`. |
| Dona fechada: trilha e alarme | **Parcial, igual:** encaminha ao registrador, mas gravação pós-commit pode falhar; I23 isolado não determina exit 2. `disputa_emissao.py:608`; `descarte_disputa.py:19`, `:37`; `auditar_invariantes_chamados.py:1942`. | [nao verificado]
| Replay rederivar episódio conservado como `None` | **Igual, residual:** o gerador conserva o argumento, mas a porta usa `valor or derivador(dona)`. `disputa_emissao.py:545`, `:766`; `models.py:2866`. |
| Analisar chamado após replay divergente | **Curado nesse ramo:** análise exige destino confirmado e não recusado. `disputa_emissao.py:1535`, `:1545`, `:1547`. |
| Mutantes antigos morrerem na montagem | **Curado:** montagem precede o runner e errors não contam como RED. `test_bo_disputa_volta5_memoria.py:389`, `:415`. |
| “Paridade histórica” comparar fonte atual consigo mesma | **Curada a alegação:** o teste declara repetibilidade, não equivalência histórica. Histórico independente: **[nao verificado]**. `test_bo_disputa_volta3_memoria.py:252`, `:254`. | [nao verificado]
| Pendentes fora da fronteira do núcleo | **Igual:** wrapper chama pendentes após o retorno; helper abre P e depois agrega DS. `disputa_emissao.py:893`, `:340`, `:351`, `:354`. |
| Push dos novos motivos estrangeiros no núcleo misto | **Igual:** retorno dos novos da dona estrangeira não é coletado; wrapper usa `novos` da local. `disputa_emissao.py:664`, `:706`, `:894`. | [nao verificado]
| Push antes da confirmação externa | **Igual:** complementos e principal notificam dentro do atomic externo. `disputa_emissao.py:1285`, `:1307`, `:1574`. | [nao verificado]
| Retornos `(p, criada)` integralmente consumidos | **Igual literalmente:** primeira abertura da declaração descarta o booleano, embora confira a dona; replay confere criação e identidade. `disputa_emissao.py:834`, `:835`, `:842`, `:843`. |
| Doubles não certificarem ORM/signals/concorrência | **Igual:** limite explicitamente declarado. `test_bo_disputa_volta5m_memoria.py:6`, `:7`. |
| Fechamento vazio por recusa técnica | **Parcial:** falta de data/base, fotografia vazia e falha do complemento passaram a vetar o autofecho; outros `continue` ainda antecedem a consulta à ata. `disputa_emissao.py:1103`, `:1397`, `:1453`, `:1482`, `:1485`, `:1447`, `:1488`. |
| Derivação duplicada índice/B | **Curado nesse par:** ambos consomem a mesma fotografia. A eleição do chamado continua divergente entre lote e avulso. `disputa_emissao.py:1296`, `:1298`, `:1398`, `:291`, `:1299`. |
| Declaração/pendentes recosturarem no genérico | **Parcial:** entradas novas normalizam o pedido, mas há agrupador sem intervalo no lote e outras entradas diretas nos escritores. `disputa_emissao.py:542`, `:824`, `:1044`; `reconciliador.py:1655`. |
| Falha suite16: catraca das linhas congeladas | **Curada na verificação local:** 35 testes passaram; recolocar os números antigos produz uma failure, zero errors. `app/core/tests/test_medida_turno_do_par.py:268`, `:269`, `:540`, `:541`. |
| Falha suite16: `test_RED_F1_lote_misto_local_vazia_nao_ganha_seed_estrangeiro` | **Parcial:** o retorno simulado ao gerador passou com a correção; a assertion ORM continua exigindo destino `intervalo.pk`. Execução real nesta árvore: **[nao verificado]**. `test_bo_disputa_volta5m_memoria.py:82`, `:98`; `test_bo_disputa_volta5b_orfaos.py:157`. | [nao verificado]

Suíte inteira desta árvore, protegido ORM, concorrência, produção e efeitos externos: **[nao verificado]**. Os testes locais não certificam essas fronteiras. (`test_bo_disputa_volta5m_memoria.py:6`, `:7`.) [nao verificado]

**2. Censo de escritores e contraexemplos**

Os escritores persistentes encontrados continuam sendo estes; a própria porta registra os dois arquivos autorizados. (`fio.py:807`; `app/core/tests/test_contract_escritores_estado.py:61`.)

| Escritor | Escrita |
|---|---|
| Nascimento de P | Construção e inserção: `models.py:3014`, `:3017`. |
| Reuso da canônica | Atribuição e save: `models.py:2982`, `:3006`. |
| `reapontar_resolvedora` | Atribuição e save: `fio.py:925`, `:927`. |
| Banco, ao apagar chamado | FK `SET_NULL`; não escolhe outro chamado. `models.py:3385`, `:3387`. |

As atribuições de `fio.py:779`/`:783` são temporárias e restauradas; `models.py:366`/`:367` reassocia a instância do mesmo PK. (`fio.py:778`, `:783`; `models.py:366`.)

O alcance dos pedidos é:

| Caminho | Situação |
|---|---|
| Agrupador, complementos e B | Usa apenas o índice dos intervalos **do lote**; replay conserva esse pedido. `disputa_emissao.py:1044`, `:1296`, `:1534`. |
| Gerador, inclusive pendentes | Normaliza antes de `abrir`. `disputa_emissao.py:340`, `:344`, `:542`, `:546`. |
| Declaração de colaborador/admin | Ambas chegam ao helper normalizado; reaponte e replay usam os argumentos resultantes. `declaracao_texto.py:365`, `:653`; `disputa_emissao.py:824`, `:829`, `:842`. |
| Retorno do julgamento da célula | `analisar` → signal → cartório → fábrica → gerador alcança a normalização. Signals reais executados: **[nao verificado]**. `models.py:800`, `:806`; `app/chamados/signals.py:208`, `:216`; `app/ponto/services/cartorio.py:1008`; `fabrica_da_celula.py:172`, `:174`, `:219`. |
| Outros pedidos a reapontar | Permanecem diretos: reconciliador, gate do chamado, LIMBO e migração de fio. `app/chamados/reconciliador.py:954`, `:1642`, `:1655`; `models.py:437`; `limbo_folgas.py:244`, `:282`; `fio.py:2174`. |

**Sim, permanece recostura para o genérico.** Contraexemplo: existe P `intervalo_saida` na dona estrangeira, destinada ao intervalo; o agrupador recebe somente `[ch]`. O índice fica sem aquela chave, o fallback mantém `ch`, e o reuso pode gravá-lo na P existente. Os juízes conferem colaborador/turno/fio, sem eleger o chamado do marco explícito. (`disputa_emissao.py:1296`, `:1044`, `:1055`; `models.py:2972`, `:2980`, `:2982`; `app/chamados/juizes.py:2185`, `:2208`, `:2217`.)

A reprodução em memória conservou a dona e trocou destino **intervalo→ch**, tanto com dona aberta quanto fechada. A porta, ORM e signals desse ensaio são doubles; execução ORM do contraexemplo: **[nao verificado]**. (`test_bo_disputa_volta5m_memoria.py:6`; `test_bo_disputa_volta5l_memoria.py:64`; caminho produtivo: `disputa_emissao.py:1044`, `:1055`, `:1296`.)

| Contraexemplo pedido | Resultado e limite |
|---|---|
| Só `ch`, sem intervalo externo | Passa no ensaio: complemento fica em `ch`. Não cobre o intervalo existente fora do lote. `test_bo_disputa_volta5l_memoria.py:111`; `disputa_emissao.py:1296`. |
| Só intervalo | Passa no ensaio de B. `test_bo_disputa_volta5l_memoria.py:118`. |
| `[ch, intervalo]` e `[intervalo, ch]` | Passam nos ensaios com um único intervalo explícito. `test_bo_disputa_volta5l_memoria.py:81`, `:87`, `:102`. |
| Dona fechada e replay 2x | Passam nos cenários existentes; não impedem o contraexemplo `[ch]` acima. `test_bo_disputa_volta5l_memoria.py:83`, `:88`, `:104`, `:109`; `models.py:2982`. |
| Avulso sem chave explícita | Helper conserva o destino recebido. Se a canônica era do intervalo, nada nessa seleção impede pedir o genérico. Reproduzido em memória; ORM: **[nao verificado]**. `disputa_emissao.py:289`, `:292`, `:294`, `:542`; `models.py:2982`. |

Há ainda duas eleições para a mesma chave: o agrupador escolhe **primeiro do lote**, enquanto o avulso escolhe **menor PK**. Com dois intervalos para o mesmo motivo/dia, o ensaio observou mudança de destino entre eles. (`disputa_emissao.py:1299`, `:291`, `:292`; limite do ensaio: `test_bo_disputa_volta5m_memoria.py:6`.)

**3. `_marcos_de_intervalo`: unicidade, regularizabilidade e exceções**

**Uma redação da derivação para índice/B: sim. Uma garantia universal de concordância semântica: não.** A fotografia é produzida em `:1296` e consumida por ambos; avulsos executam novamente o mesmo helper sobre outro universo. (`disputa_emissao.py:257`, `:292`, `:1296`, `:1298`, `:1398`.)

Para o mesmo `data` válido, o dia concorda com `dia_do_turno_da_celula`: ela retorna `DiaDoTurno(dia)`, e `DiaDoTurno` é `NewType`. Não há conversão civil→turno nessa chamada. (`disputa_emissao.py:273`; `app/ponto/turnos.py:2939`, `:2943`; `app/core/dia_do_turno.py:13`.)

Regularizável não significa “possui marco válido”: o predicado admite a fábrica; o helper exige categoria exata, descarta marcos não dict/datas inválidas e transforma qualquer tipo diferente de `S` em volta. Portanto, regularizável com fotografia vazia é possível pelo código. (`disputa_emissao.py:1003`, `:1009`, `:266`, `:270`, `:274`, `:276`.)

Também não é função total para qualquer JSON: contexto truthy não dict falha no `.get`; `marcos_faltantes` truthy não iterável falha no `for`. Esses pontos ficam fora do `try` e já ficavam fora da captura antiga. (`disputa_emissao.py:269`; comparação histórica pelo patch fornecido: **[nao verificado]** quanto ao Git.)

No antigo B, a captura genérica foi substituída por `TypeError/ValueError`. Uma outra subclasse de `Exception` lançada dentro da expressão protegida — por exemplo, `RuntimeError` de um `.get` extraordinário — agora propaga nesse sítio. O índice antigo já tinha captura estreita; isso não demonstra uma mudança global para JSON ordinário. Ocorrência concreta em produção: **[nao verificado]**. (`disputa_emissao.py:272`, `:274`; histórico da captura conforme patch fornecido: **[nao verificado]** quanto ao Git.)

O construtor de dia também entrou no `try`: seus eventuais T/V passam a ser capturados, embora a implementação atual seja identidade. (`disputa_emissao.py:273`; `app/ponto/turnos.py:2943`.)

Finalmente, o tipo não certifica a procedência do dia: o detector calcula `marcos[].data` pela data civil do instante encadeado, potencialmente D+1. Sua emissão está aposentada; não é escritor persistente atual desses chamados. (`app/ponto/management/commands/detectar_intervalo_ausente.py:77`, `:82`, `:149`, `:156`.)

**4. Qual luz, quem escreve/lê e qual lei**

`acender_sem_ata` escreve **somente WARNING**, com a mensagem “sem_celula: ata nao lavrada”. Não incrementa contador nem persiste o flag técnico. (`declaracao_texto.py:63`, `:70`, `:71`.)

I7a/I7b não leem esse WARNING: procuram DS abertas com zero linhas P, reconstroem dias e consultam a ata atual. Sem dia vai para ESTRUTURA/I7a; somente dias todos `sem_celula` vão para I7b. (`auditar_invariantes_chamados.py:62`, `:64`, `:68`, `:69`, `:73`, `:74`.)

Os novos escritores são de memória: `ato['insumo_incompleto']` e `_insumo_incompleto`; seu leitor decisório é o veto em `:1562`. Não nasceu escritor persistente, mas nasceu **predicado adicional de autofechamento**, capaz de impedir a chamada a `fechar` mesmo depois de `pode_fechar()` responder sim. (`disputa_emissao.py:1097`, `:1103`, `:1314`, `:1397`, `:1453`, `:1482`, `:1485`, `:1561`, `:1562`, `:1567`.)

A autoridade registrada para “disputa pode fechar?” é `DisputaSupervisao.pode_fechar`. Portanto, a afirmação da entrega de que não há novo julgamento não está demonstrada apenas por continuar chamando esse método: existe outra condição decidindo o autofecho. (`app/docs/JUIZES.md:62`; `models.py:1848`, `:1853`; `disputa_emissao.py:1562`.)

DUAS-RESPOSTAS preserva HX sem ata; DUAS-LEIS preserva h2 com ata e nada a perguntar; LIBERA-B3 autoriza cura na origem sob escritor único. Autorização literal para estender o veto a sem-data/base/marco inválido: **[nao verificado]** nos avais fornecidos. Escopo literal e unicidade são exigidos por `CLAUDE.md:37`, `:175`.

Há dois limites adicionais:

- Sem base, o código pode emitir “ata não lavrada” sem consultar a ata. Essa mensagem não demonstra a causa anunciada. (`disputa_emissao.py:1484`, `:1485`, `:1564`; `declaracao_texto.py:71`.)
- Se `_classificar_falta` devolver motivo `None`, há saídas anteriores à consulta da ata. O ensaio com ata simulada `sem_celula` terminou fechado, sem consulta nem luz; ORM: **[nao verificado]**. (`disputa_emissao.py:1446`, `:1447`, `:1456`, `:1487`, `:1488`, `:1490`, `:1567`; doubles: `test_bo_disputa_volta5m_memoria.py:6`.)

**5. Catraca: mesmo sítio ou outra coisa**

**Os números novos apontam aos mesmos sítios de extração de data**, pelo cotejo da cópia com o delta fornecido:

| Registro atual | Conteúdo |
|---|---|
| `[928, 929]` | `de = _tz.localtime(t.entrada.timestamp).date()` e `ds = _tz.localtime(t.saida.timestamp).date()`. `disputa_emissao.py:928`, `:929`. |
| `[1445]` | `data_entrada = _tz.localtime(batida_origem.timestamp).date()`. `disputa_emissao.py:1445`. |

Não apontam ao helper novo. O censo B1 reconhece essas extrações `.date()`, e a assertion continua comparando o mapa completo de linhas efetivas. (`app/core/medida_turno_do_par.py:217`, `:225`; `app/core/tests/test_medida_turno_do_par.py:534`, `:540`, `:541`.)

A catraca passou nos 35 testes locais; restaurar os números antigos produziu uma failure, zero errors. Isso prova a mordida do registro atual, não correção semântica dos sítios congelados. (`app/core/tests/test_medida_turno_do_par.py:541`; sítios: `disputa_emissao.py:928`, `:929`, `:1445`.)

Identidade histórica independente pelo Git: **[nao verificado]**; a cópia não disponibilizou repositório Git.

**6. Classes que já reprovaram obra**

| Classe | Resultado |
|---|---|
| Segundo juiz | **Não certifico zero:** o novo veto técnico decide o autofecho além da resposta do juiz registrado. `disputa_emissao.py:1561`, `:1562`; `app/docs/JUIZES.md:62`. |
| Segundo escritor fora da porta | **Nenhum novo persistente encontrado**, mas a regra não governa todos os pedidos aos escritores existentes. `fio.py:807`; `models.py:2982`; `disputa_emissao.py:1044`. |
| Relógio fora da borda | **Nenhuma leitura nova de relógio identificada na c21**; extrações civis congeladas e `now()` legados permanecem. `disputa_emissao.py:273`, `:595`, `:928`, `:929`, `:1371`, `:1445`. |
| Derivação paralela de fonte única | **Curada para `(motivo,dia)` no índice/B; presente na eleição do destino:** primeiro do lote versus menor PK. `disputa_emissao.py:1296`, `:1398`, `:1299`, `:291`. |
| Selo/RED sem mordida | **Curado nos mutantes executados:** failures por assertion, zero errors; montagem não conta como RED. `test_bo_disputa_volta5m_memoria.py:68`, `:214`, `:218`; `test_bo_disputa_volta5l_memoria.py:157`, `:162`. |
| Campo com dois significados | `data_evento` distingue conversa/fato pela porta; `DS.motivos` é cenário/seed, não identidade de P; `chamado_destino` continua podendo oscilar entre genérico e explícito. `models.py:1743`, `:2427`, `:2794`; `disputa_emissao.py:1508`, `:1044`; `models.py:2982`. |

Resultados locais observados: controle c21 **6 testes OK**; sete mutantes c21 com **3, 4, 1, 2, 1, 4, 1 failures**, zero errors; quatro c20 com **5, 5, 1, 1 failures**, zero errors. Os runners exigem controle verde e RED por assertion. (`test_bo_disputa_volta5m_memoria.py:28`, `:207`, `:214`, `:218`; `test_bo_disputa_volta5l_memoria.py:154`, `:162`.) [nao verificado]

Os mutantes antigos `nasce_sem_pergunta` e `replay_insere` também produziram uma failure cada, zero errors. Isso não certifica ORM/signals/cartório. (`test_bo_disputa_volta5_memoria.py:393`, `:394`, `:415`; `test_bo_disputa_volta5m_memoria.py:6`.)

**7. Quem escreve e quem lê os campos envolvidos**

| Campo/insumo | Quem escreve | Quem lê nesta cura e efeito relevante |
|---|---|---|
| `Ch.categoria`, `colaborador`, PK | Nascimento pela porta/inserção. `models.py:961`, `:971`. | Recorte exato, identidade da fotografia e desempate. `disputa_emissao.py:266`, `:289`, `:291`, `:1296`. |
| `Ch.status_local` | `_gravar_estado`; inserção inicial pela porta. `models.py:201`, `:228`, `:971`. | Avulso usa `VIVOS`; autoridade dos estados permanece no catálogo. `disputa_emissao.py:14`, `:290`; `app/docs/JUIZES.md:59`. |
| `Ch.contexto_json.marcos_faltantes[].tipo/data` | Portas de contexto/nascimento. Detector calcula formato com data, mas emissão está aposentada. Grade atual também escreve `marcos_faltantes`, em categoria `Ausencia` e sem `data` por marco. `contexto.py:76`, `:102`; `models.py:971`; `app/ponto/management/commands/detectar_intervalo_ausente.py:80`, `:149`; `app/ponto/management/commands/reconciliar_grade.py:498`, `:502`. | Helper deriva fatos; índice, B e avulso consomem. `disputa_emissao.py:269`, `:273`, `:276`, `:292`, `:1298`, `:1398`. |
| `P.chamado_destino` | Nascimento/reuso, reapontar ou FK `SET_NULL`. `models.py:3014`, `:3017`, `:2982`, `:3006`, `:3387`; `fio.py:925`, `:927`. | Juiz, filtro/complemento e materialização. `app/chamados/juizes.py:2185`, `:2208`; `disputa_emissao.py:1320`, `:1341`; `materializacao.py:1076`, `:1079`. |
| `P.motivo` | Pedido derivado pelo helper; persistência no nascimento. `disputa_emissao.py:276`; `models.py:3014`, `:3017`. | Chave de destino, identidade e materialização. `disputa_emissao.py:292`, `:1044`; `models.py:2427`; `materializacao.py:1077`. |
| `P.data_evento` | Atribuição exclusivamente por `gravar_dia_do_fato`; nascimento passa por ela. `fio.py:790`, `:800`; `models.py:3016`. | Chave motivo/dia, dedup, juiz e plantio. `disputa_emissao.py:292`, `:1044`, `:1698`; `fio.py:1608`; `materializacao.py:1345`. |
| `P.disputa` | Inserção; reuso conserva a dona. `models.py:3014`, `:3008`. | Encaminhamento estrangeiro, rollback e replay. `disputa_emissao.py:654`, `:835`, `:1065`, `:1524`. |
| `P.batida_origem_id` | Derivação no nascimento e preparação da identidade. `models.py:2866`, `:2868`, `:3449`. | Argumento do replay e juiz do dia/fato. `disputa_emissao.py:545`, `:766`; `models.py:2875`; `fio.py:1633`. |
| `P.celula` | Nascimento e ajuste no reuso. `models.py:2896`, `:2966`, `:3014`. | Costura/lavra e despertar das células afetadas. `models.py:2972`, `:3004`, `:3007`; `fio.py:933`. |
| `P.pergunta`, `tipo_resposta`, `opcoes` | Catálogo/gerador e payload da declaração; inserção pela porta. `disputa_emissao.py:484`, `:502`, `:543`; `decl

[truncado em 20000 de 23684 caracteres]

## 11/11 01:50 · sessao 168b267eaf81 · - · - · origem=Ronald (tmux)

Censo somente leitura concluído. Nenhum arquivo alterado.

**C6: chamadas produtivas diretas e por alias identificadas por AST**

| Arquivo | Linhas das chamadas |
|---|---|
| `chamados/relatorios_disputa.py` | 18 |
| `chamados/views.py` | 1242, 1257, 1417 |
| `chamados/juizes.py` | 1417, 1422, 1434, 1504, 1605, 2267 |
| `chamados/models.py` | 368, 1509, 1896, 3044, 3139, 3375 |
| `chamados/reconciliador.py` | 398, 661, 758, 1086, 1594 |
| `api/views.py` | 2620 |
| `chamados/management/commands/reabrir_orfas_enterradas.py` | 83 |
| `chamados/management/commands/auditar_invariantes_chamados.py` | 202, 540, 858, 894, 1653, 1710, 1769, 1782 |
| `chamados/management/commands/migrar_data_evento_dia_do_turno.py` | 449 |
| `chamados/management/commands/disparar_perguntas_competencia.py` | 109 |
| `chamados/services/relavra_via.py` | 242 |
| `chamados/services/fio.py` | 238, 606, 781, 960, 2128, 2133 |
| `chamados/services/competencia_trancada.py` | 122, 132, 142 |
| `chamados/services/disputa_emissao.py` | 1843, 1925 |
| `chamados/services/acoes_chamado.py` | 348 |
| `chamados/services/pergunta_no_app.py` | 58, 136 |
| `chamados/services/vigia_fio.py` | 755, 813 |
| `ponto/services/dia_do_colab.py` | 71 |
| `colaboradores/services/calendario.py` | 476 |

São 52 chamadas em 19 arquivos pelo critério acima. `chamados/models.py:4083` reexporta a função; as propriedades `estado` e `viva` delegam em `chamados/models.py:3044` e `chamados/models.py:3375`. O template lê essas propriedades em `templates/chamados/partials/_card_atendimento.html:77` e `templates/chamados/partials/_card_atendimento.html:183`. Chamadas dinâmicas sem identificador literal: [nao verificado].

**Leitores reais do dict da tarefa**

O único consumidor produtivo externo identificado é `chamados/management/commands/diff_e3_sombra.py:125`. Ele lê:

- `rota`, `fila`, `verbos`, `fio`: `chamados/management/commands/diff_e3_sombra.py:44`.
- `razao`, para classificar isento com fila residual: `chamados/management/commands/diff_e3_sombra.py:68`.

A candidata retorna `None` quando `rota=None`, portanto não mede a mudança `fio='admin', porta=None`. Esse comportamento é explicitamente exigido pelo teste atual em `chamados/tests/test_e3_diff_sombra.py:136`. A sombra necessária para esta fase deve comparar o dict inteiro, conservando `admin` sem porta. (`chamados/management/commands/diff_e3_sombra.py:44`)

`painel.py` fornece a fila; `declaracao_texto.py` fornece a posição; `juizes.py::alimentar_fila` fornece os verbos e IDs. Não consomem o dict. (`chamados/services/painel.py:186`, `chamados/services/declaracao_texto.py:78`, `chamados/juizes.py:1452`)

A agregação interna lê `instancias[].acoes`, `acao.porta`, `acao.de`, `celula_id`, `nao_sei`; publica `rota/fila/verbos/fio`. (`chamados/services/tarefa_do_dia.py:425`)

**Limites de vocabulário e exportação**

Viva sem resposta produz `responder/colaborador` pela E1, e nenhum verbo existente representa mera exposição administrativa. (`chamados/services/tarefa_do_dia.py:129`, `chamados/services/tarefa_do_dia.py:49`)

FIO-LE-O-JUIZ manda mostrar viva de disputa fechada ao ADMIN sem reabrir/push, mas não autoriza validar sem resposta. A própria porta tem teste que exige recusa sem log nesse caso. (`chamados/services/fio.py:212`, `chamados/tests/test_fio_le_o_juiz.py:235`) O modal declara que Validar só aparece depois da resposta. (`templates/chamados/partials/modal_fio.html:440`)

A base deliberadamente conserva ação exigível na exportada. (`chamados/tests/test_e2_tarefa_do_dia.py:29`) A fronteira `dia_em_competencia_exportada` foi removida por lei escrita no código. (`chamados/juizes.py:1982`) Há testes exigindo mostrar Validar e efetivamente validar em disputa fechada exportada, sem reabrir/push. (`chamados/tests/test_fio_le_o_juiz.py:277`, `chamados/tests/test_fio_le_o_juiz.py:291`)

Assim, “consulta somente leitura, sem executar escritor” cabe na base; “suprimir qualquer verbo potencialmente escritor na exportada” modifica o comportamento exigido por esses testes. O pedido atual prevalece, mas essa diferença precisa constar da entrega. [nao verificado]

**Testes diretamente pertinentes encontrados** [nao verificado]

- C6/precedência/memo: `chamados/tests/test_pergunta_viva.py:15`, `chamados/tests/test_selo_memo_pergunta_viva.py:86`, `chamados/tests/test_contract_pergunta_viva.py:25`, `chamados/tests/test_pergunta_respondida_e_marco_aceso.py`.
- Fio, fechamento e portas: `chamados/tests/test_fio_le_o_juiz.py:32`, `chamados/tests/test_o261e_f1_porta_encerramento.py:75`, `chamados/tests/test_o261e_f2_porta_carimbo.py:665`, `chamados/tests/test_pvmuda_carimbo_em_voo.py:73`.
- App/dia: `chamados/tests/test_pergunta_no_app.py:145`, `chamados/tests/test_n1_perguntas_do_app.py`, `api/tests/test_dia_do_colab.py`.
- Tranca: `chamados/tests/test_competencia_trancada.py:123`, `chamados/tests/test_tranca_de_verdade.py:52`, `chamados/tests/test_snc2_toque_em_lote.py:85`.
- Calendário: `colaboradores/tests/test_calendario_ultimo_recurso_le_o_c6.py:52`.
- Reconciliador/relavra: `chamados/tests/test_reconciliador_c1_escopo.py:48`, `chamados/tests/test_reabrir_orfas_enterradas.py:101`, `chamados/tests/test_orfas_pelo_evento.py:102`, `ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:84`.
- Tarefa/sombra: `chamados/tests/test_e2_tarefa_do_dia.py:137`, `chamados/tests/test_e3_portas_tarefa.py:36`, `chamados/tests/test_e3_diff_sombra.py:131`, `chamados/tests/e3_mutantes.py:76`, `chamados/tests/e4_verificar.py:50`.

Não executei testes neste subtrabalho; cobertura completa de todos os consumidores: [nao verificado]. [nao verificado]

O RED c8 exige cuidado: chama `_tarefa_pura` sem insumo de vida, e seu helper não fornece `pergunta_viva`. Será necessário ampliar somente a infraestrutura do helper para alimentar o novo argumento, preservando os testes/asserts. Usar `p.viva_c6` como autoridade produtiva fabricaria um predicado fora do juiz. (`chamados/tests/test_e3_tarefa_admin_sem_banco.py:74`, `chamados/tests/test_e3_tarefa_admin_sem_banco.py:77`, `chamados/tests/test_e3_tarefa_admin_sem_banco.py:123`)

## 11/11 01:50 · sessao 1a7b48f4d91d · - · - · origem=Ronald (tmux)

**Derivação e exceções**

- A duplicação índice/B foi removida: `_marcos_de_intervalo` deriva `(motivo,dia)` em `disputa_emissao.py:257-278`; a fotografia nasce em `:1296`, índice lê `:1298` e B lê `:1398`. Avulsos chamam o mesmo derivador novamente (`:292`).
- Para o mesmo dado válido, o dia concorda: `dia_do_turno_da_celula` devolve `DiaDoTurno(dia)` sem conversão (`app/ponto/turnos.py:2939-2943`); `DiaDoTurno` é `NewType` (`app/core/dia_do_turno.py:13`).
- Regularizável não garante fotografia não vazia: o predicado permite a fábrica (`disputa_emissao.py:1003-1009`), enquanto o helper exige categoria exata e descarta marcos/data inválidos (`:266-276`). Tipo desconhecido vira `intervalo_volta` (`:276`).
- Não é total para todo JSON: contexto truthy não dicionário causa `AttributeError`; `marcos_faltantes` truthy não iterável causa `TypeError` fora da captura (`:269`). Esses pontos também ficavam fora do `try` antigo, portanto não são efeito novo da captura.
- A antiga captura genérica de B para `.get/fromisoformat` virou `(TypeError,ValueError)` (`:273-275`): outras subclasses de `Exception`, como `RuntimeError`, passam a propagar nesse sítio. Para dict/list/string JSON ordinários, não identifiquei exceção adicional normal; comportamento extraordinário concreto em produção: **[nao verificado]**. O índice antigo já capturava somente T/V, conforme patch fornecido.
- O construtor de dia passou para dentro do `try` (`:273`): T/V dele também seriam agora engolidos, embora sua implementação atual seja identidade (`app/ponto/turnos.py:2943`).

**Fonte do dia**

- O detector calcula `marcos[].data` pela data civil do instante encadeado (`app/ponto/management/commands/detectar_intervalo_ausente.py:77-84`), potencialmente D+1 num turno de célula D; o NewType não certifica semanticamente esse insumo (`app/ponto/turnos.py:2939-2943`).
- Entretanto, a emissão desse detector está aposentada: só reconcilia legado, com `continue` antes de nascimento (`detectar_intervalo_ausente.py:122-139`, `:149-157`). Não deve ser apresentado como escritor persistente atual.
- Escritores atuais de `marcos_faltantes` incluem nascimento na grade (`app/ponto/management/commands/reconciliar_grade.py:491-502`), relavra pela porta (`:431`, `:457`) e detectar ausências (`app/ponto/management/commands/detectar_ausencias.py:497`, `:558-559`, `:675-711`). Esses formatos normalmente levam tipo/hora/motivo, sem data; o nascimento da grade usa categoria `Ausencia`, fora do helper de intervalo (`reconciliar_grade.py:498-502`; `disputa_emissao.py:266`).

**Luz e autoridade**

- `acender_sem_ata` escreve somente WARNING, com a mensagem literal “sem_celula: ata nao lavrada” (`app/chamados/services/declaracao_texto.py:63-72`). Os contadores não leem esse log nem o flag.
- I7a/I7b leem DS abertas sem nenhuma linha P, reconstroem os dias e consultam a ata atual (`app/chamados/management/commands/auditar_invariantes_chamados.py:62-74`). Sem dia é ESTRUTURA/I7a (`:64-73`); o relatório publica INFO para I7b e VIOL para I7a positivo (`:89-95`, `:1925-1927`). Falha dessa leitura não tem captura local (`:69-71`).
- Os novos escritores de memória são `ato['insumo_incompleto']` (`disputa_emissao.py:1097`, `:1103`) e `_insumo_incompleto` (`:1314`, `:1397`, `:1453`, `:1482`, `:1485`); o único leitor decisório é `:1562`.
- Não nasceu escritor persistente; nasceu **predicado adicional de auto-fechamento**, que veta chamar `fechar` mesmo quando `pode_fechar()` responde sim (`:1561-1567`). A autoridade “disputa pode fechar?” continua registrada em `app/docs/JUIZES.md:62` e definida em `app/chamados/models.py:1848-1853`. Os anexos autorizam HX sem ata; autorização literal para estendê-la a sem-base/data/marco inválido: **[nao verificado]**.
- O novo WARNING pode afirmar ausência de ata sem tê-la consultado, por exemplo no ramo sem base (`disputa_emissao.py:1484-1485`, `:1562-1564`). I7a/I7b classificam depois por outra leitura, portanto o aviso não determina o contador (`auditar_invariantes_chamados.py:68-74`).

**Resíduos concretos**

- Marco válido + inválido não marca incompletude: o helper descarta o inválido, e B só marca quando a fotografia inteira está vazia (`disputa_emissao.py:274-275`, `:1396-1397`). “Todo marco inválido conserva a disputa” é mais forte que o implementado.
- Continua fechamento vazio sem testemunho de ata quando `_classificar_falta` devolve motivo `None`, tanto por origem quanto sem origem (`:1446-1448`, `:1487-1489`); a leitura da ata vem depois (`:1456`, `:1490`).
- Reproduzi esse segundo ramo com AST produtiva/Ambiente5m: base=True, classificar=(None,None,None), estado de ata sem_celula → DS fechada, zero P, zero luz, zero consultas ao juiz da ata. Estrutura dos doubles: `app/chamados/tests/test_bo_disputa_volta5m_memoria.py:6-7`, `:74-78`; caminho produtivo: `disputa_emissao.py:1487-1489`, `:1565-1567`. ORM: **[nao verificado]**.
- Reproduzi também ata vazia disponível + sem_base → DS aberta, zero P, luz=1, zero consultas à ata. Demonstra que o novo veto pode conservar disputa num universo em que o aval h2 manda conservar o fechamento vazio; código do veto: `disputa_emissao.py:1484-1485`, `:1562-1564`; enquadramento ORM concreto: **[nao verificado]**.

## 11/11 01:49 · sessao ce9dd6eece4b · - · - · origem=Ronald (tmux)

Achado principal: **a cura continua parcial na origem**. O agrupador só indexa intervalos do lote; quando recebe apenas `ch`, pode recosturar para `ch` uma pergunta já destinada a intervalo fora do lote. A normalização nova existe no gerador/declarador, mas não governa os escritores persistentes. (`app/chamados/services/disputa_emissao.py:1296`, `:1299`, `:1044`, `:542`, `:824`; `app/chamados/models.py:2982`, `:3006`.)

Reproduzi em memória, com `Ambiente5m`, estes casos:

- DS local vazia, dona estrangeira com P `intervalo_saida` destinada a intervalo=1, genérico `ch`=2; agrupador recebe `[ch]`: P permanece na dona e troca destino **1→2**, tanto dona aberta quanto fechada. A porta/ORM/signals são doubles; execução ORM **[nao verificado]**. (`app/chamados/tests/test_bo_disputa_volta5m_memoria.py:6`; `app/chamados/tests/test_bo_disputa_volta5c_memoria.py:101`, `:104`; causa produtiva: `app/chamados/services/disputa_emissao.py:1044`, `:1055`, `:1296`.)
- Avulso do gerador sem chave explícita: intervalo com `marcos_faltantes=[]` ou fora de `VIVOS`; helper devolve genérico e canônica troca intervalo→genérico. Demonstração em memória; ORM **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:269`, `:289`, `:290`, `:294`, `:542`, `:546`; `app/chamados/models.py:2982`.)
- Dois intervalos explícitos i1=1/i2=2 para mesmo motivo/dia: lote `[i2,i1,ch]` elege i2; gerador posterior elege i1. Observei **2→1** em memória. Portanto há arbitragem paralela de **quem recebe**: primeiro do lote versus menor PK. (`app/chamados/services/disputa_emissao.py:1299`, `:291`, `:292`; limite: `app/chamados/tests/test_bo_disputa_volta5m_memoria.py:6`.)

Censo persistente:

| Escritor | Prova |
|---|---|
| Inserção de P | `app/chamados/models.py:3014`, `:3017` |
| Reuso canônico | `app/chamados/models.py:2982`, `:3006` |
| `reapontar_resolvedora` | `app/chamados/services/fio.py:925`, `:927` |
| FK ao apagar chamado | `app/chamados/models.py:3385`, `:3387` |

As atribuições de `fio.py:779`/`:783` são temporárias; `models.py:367` reassocia instância do mesmo PK. O contrato enumera os dois arquivos escritores. (`app/chamados/services/fio.py:778`, `:783`, `:807`; `app/chamados/models.py:366`; `app/core/tests/test_contract_escritores_estado.py:61`.)

Chamadores diretos que continuam sem a seleção nova:

- Reconciliador: reemissão, histórico e derivação avulsa pedem `reapontar_resolvedora` diretamente. O derivador legado exclui candidato que já tenha P não materializada. (`app/chamados/reconciliador.py:954`, `:1642`, `:1655`; `app/chamados/services/disputa_emissao.py:226`, `:229`.)
- Gate de fechamento de chamado; reata/agrupamento LIMBO; migração de fio. (`app/chamados/models.py:433`, `:437`; `app/chamados/services/limbo_folgas.py:244`, `:282`; `app/chamados/services/fio.py:2174`.)

Os escritores conferem colaborador/turno e se a costura mata fio; não verificam preferência por chamado com marco explícito. Destino não terminal passa imediatamente pelo segundo gate. (`app/chamados/juizes.py:2179`, `:2185`, `:2208`, `:2217`; `app/chamados/services/fio.py:771`, `:772`, `:907`, `:925`; `app/chamados/models.py:2972`, `:2982`.)

Rota de retorno do julgamento: `analisar`→`_gravar_estado`→signal→cartório→fábrica→gerador alcança a normalização nova; há guarda contra julgamento recursivo. Não executei esses signals reais. (`app/chamados/models.py:800`, `:806`; `app/chamados/signals.py:208`, `:210`, `:216`; `app/ponto/services/cartorio.py:1008`; `app/chamados/services/fabrica_da_celula.py:172`, `:174`, `:219`; `app/chamados/services/disputa_emissao.py:542`, `:546`.)

Para os cenários já existentes — só ch sem intervalo externo, só intervalo, ambas ordens, replay 2x/dona fechada — os selos conferem o resultado em memória, não ORM. (`app/chamados/tests/test_bo_disputa_volta5l_memoria.py:81`, `:87`, `:109`, `:111`, `:118`; limite `:6`.)

**Bloqueio de desenho preservado:** seleção corrigida em chamadores específicos continua permitindo recostura divergente pela mesma porta; o contraexemplo `[ch]` com intervalo externo basta para negar cura universal. (`app/chamados/services/disputa_emissao.py:1044`, `:1296`; `app/chamados/models.py:2982`, `:3006`.)

_3611 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
