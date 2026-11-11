# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 10:36** · publicado em **11/11/2026 10:38** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 10:36 · sessao 1ca6effd2852 · - · - · origem=Ronald (tmux)

Há 48 IDs de primeira coluna adicionais aos 18 nomes pedidos; aliases E3/E4B/UI podem ser integrados às linhas obrigatórias. A lista completa foi enviada em duas mensagens, preservando os números de `backlog_obras.txt:852` a `backlog_obras.txt:2367`.

A instrução literal também alcança obras nomeadas dentro das descrições: UI-ANEXO-AUSENCIA (`backlog_obras.txt:1131`), JUIZ-DO-PAR (`backlog_obras.txt:1732`), JUIZ-1 e CRONS-INSTALL-SOB-MARCA (`backlog_obras.txt:1761`), PLANTIO (`backlog_obras.txt:1852`), E1-FILA-V2-2-TRAVA (`backlog_obras.txt:1853`), BO-0611-05 (`backlog_obras.txt:1889`), BUG-PAR (`backlog_obras.txt:1895`), O137/ESMERIL-NO-RASTRO (`backlog_obras.txt:1899`), BO-0611-05-RESTO-DA-CLASSE (`backlog_obras.txt:1905`), O284-A (`backlog_obras.txt:1911`) e ORDEM-ATE-O-ZERO (`backlog_obras.txt:2367`). A distinção entre obra própria, alias e referência histórica precisa ser declarada; omitir silenciosamente esses nomes contraria a leitura literal. [nao verificado: critério final de identidade]

E4C/E4D e RAIZ F2–F7 existem como etapas nomeadas, sem linha própria no BACKLOG anexado; não devem ser anunciadas como ausentes do BACKLOG inteiro (`backlog_obras.txt:1878`, `backlog_obras.txt:1895`). LASTRO genérico aparece na META, mas seus registros específicos são LASTRO-DEGRAU-2 e LASTRO-PARTE-2/3 (`backlog_obras.txt:1732`, `backlog_obras.txt:1761`, `backlog_obras.txt:1763`, `backlog_obras.txt:2367`).

Todos os impactos de extras em `fio_morto_alcance` e na Central devem permanecer `nao medido`; nenhuma medida desses efeitos foi dada para eles. [nao verificado]

## 11/11 10:36 · sessao 599f583cc054 · - · - · origem=Ronald (tmux)

Censo somente leitura, na BASE `9ca563bb3`; as linhas dos arquivos alterados precisarão ser atualizadas após a cura. [nao verificado]

**Quem escreve e quem lê**

- `pendentes` é uma lista derivada, não um campo persistido: `consulta_do_app` escreve a lista com perguntas vivas sem `respondida_em` e com disputa sem `fechada_em`; conserva `historico` separado. A autoridade da vitalidade é `perguntas_do_app`, que chama `pergunta_viva`. (`chamados/services/pergunta_no_app.py:45`, `:61`, `:69`, `:71`, `:75`)
- Escritores persistentes de `respondida_em`: resposta em `chamados/services/disputa_emissao.py:1468`; reabertura em `:1593`; resposta automática em `:1653`; reconstrução pela trilha em `:1674`; ressurreição em `chamados/models.py:1566`; `carimbar` em `:3132`. A atribuição `models.py:3163` desfaz estado apenas em memória. (`chamados/models.py:3159`)
- Escritores de `DisputaSupervisao.fechada_em`: fechamento em `chamados/models.py:1932`; reaberturas em `:2149` e `:2173`.
- Leitores diretos de `consulta_do_app` em produção: dois badges `/me/` (`api/views_core.py:313`, `api/views.py:324`), listagem de chamados (`api/views.py:1004`), perguntas da disputa (`api/views.py:2542`), dashboard (`core/views.py:252`), aviso de pendência (`chamados/services/aviso_pendencia.py:8`) e cobrança (`chamados/services/cobranca.py:47`).
- `FollowupChamado.lido_colaborador_em` nasce nulo: campo aceita nulo e a criação não fornece carimbo. (`chamados/models.py:1673`, `chamados/services/nota.py:88`) A escrita de leitura passa por `marcar_lidas`, que resolve o nome em `LADOS` e faz `update` com instante. (`chamados/services/nota.py:59`, `:126`, `:132`) Chamadores do lado colaborador: `api/views.py:1050`, `chamados/views.py:1135`, `:1150`.
- Leitores de `lido_colaborador_em`: badges dos kernels (`api/views_core.py:311`, `api/views.py:322`), serializador de followups (`api/views.py:1128`), contador web (`chamados/views.py:603`) e indicação “novo” do cartão (`templates/chamados/partials/_card_atendimento.html:51`, `:59`).
- `ChamadoColaborador.status_local`: escritor de transição persistente é `_gravar_estado`, atribui e salva. (`chamados/models.py:201`, `:228`, `:296`) Nascimento também fornece status no INSERT, pela porta `criar`. (`chamados/models.py:1095`) Atribuições `:356` e `:371` simulam estado em memória. A query do badge lê `status_local__in=VIVOS`; o conjunto inclui `aberto`, `em_analise` e `registrado`. (`api/views_core.py:309`, `api/views.py:320`, `chamados/catalogo/motor.py:24`)

**Outros leitores dos rótulos**

O grep no código de produção, incluindo templates e JS, encontrou a regra duplicada nos dois kernels e uma terceira regra manual no dashboard, além de usos textuais de “Chamados” sem regra de badge. (`api/views_core.py:316`, `:318`, `api/views.py:327`, `:329`, `templates/core/dashboard_colaborador_v2.html:80`)

A linha de fila deve apontar `templates/core/dashboard_colaborador_v2.html:80`: escolhe “Responder DP”/“Chamados” pelo número de perguntas, sem ramo vermelho; o número vem de `core/views.py:253`. Outros usos são rótulo de voltar (`templates/chamados/form_abertura.html:7`), cabeçalhos de relatório (`relatorios/views.py:1672`, `inteligencia/views.py:255`) e chave de comando (`ponto/management/commands/zerar_dados_operacionais.py:52`). O grep em `static` por ambos os literais não encontrou ocorrências. [nao verificado]

**Leitores `api/views.py:1004` e `:2542`**

- `api_chamados_v2`: consulta perguntas e histórico, indexa as perguntas por chamado vivo/destino e serializa a conversa. Não calcula badge. Seu GET marca followups do DP como lidos, portanto não serve para comparar o contador anterior. (`api/views.py:1003`, `:1040`, `:1044`, `:1050`)
- `api_disputa_perguntas`: publica pendentes, histórico e o subconjunto `respondidas`; mantém `em_disputa`, IDs e opções servidas. Não calcula badge. (`api/views.py:2531`, `:2543`, `:2571`, `:2573`, `:2581`, `:2582`) Ambos devem continuar usando `consulta_do_app`; nenhuma mudança nesses leitores é necessária. (`chamados/services/pergunta_no_app.py:61`)

**Contrato adicional a tratar**

O teste de contagem registra os dois sítios inline dos badges como exceções `MENSAGEM`. Ao mover a query, o contrato exige retirar os dois sítios antigos, declarar o sítio da projeção e reduzir o teto de acordo; sem isso haverá assertions por sítio novo e declarações obsoletas. (`core/tests/test_tripwire_contagem_de_pendencia.py:83`, `:85`, `:383`, `:390`, `:395`) Não editei arquivos nem executei banco. [nao verificado]

## 11/11 10:36 · sessao a41024c8adde · - · - · origem=Ronald (tmux)

- **Causas a–e:** E4B é a dona escrita de todas as cinco classes; não é necessário inventar dono por semelhança de código. O BACKLOG anexo diz literalmente “backfill do fio morto (matriz_e4 C3, classes a-e)”; a cópia confirma o escopo. Isso prova responsabilidade, não redução do baseline nem cobertura eficaz medida. (`backlog_obras.txt:1887`; `app/docs/BACKLOG.md:1875`.)
- **Significados:** a = chamado vivo fora da fila; b = resposta carimbada sem texto; c = respondida com destino terminal; d = colaborador na lista CXR e dia fora; e = restante discriminado. O subtipo de e `pergunta_viva_sob_chamado_terminal` tem ramo próprio. (`app/chamados/services/vigia_fio.py:519`; `app/chamados/services/vigia_fio.py:538`; `app/chamados/services/vigia_fio.py:667`; `app/chamados/services/vigia_fio.py:688`.)
- **SEM DONO:** os dados permitem dizer que nenhuma causa a–e ficou sem obra dona explícita, porque E4B nomeia todas. Não permitem dar como zero o resíduo depois das obras; redução efetiva não medida continua “nao medido”. A coluna de tamanho solicitada deve permanecer literalmente “a medir pela sessao”. (`backlog_obras.txt:1887`; impacto pós-obras: [nao verificado].)

Para as linhas RAIZ, a sequência escrita é F0→F1→F2=BO-DISPUTA→F3→F4=BUG-PAR→F5 com E4d→F6→F7. Essa ordem não define por si o inventário de arquivos de cada fase. (`backlog_obras.txt:1895`; `app/docs/BACKLOG.md:1883`.)

- **RAIZ F1:** sítios de destino em `app/chamados/models.py:2982`, `:3014`; `app/chamados/services/fio.py:925`; eleitores em `app/chamados/services/disputa_emissao.py:238`, `:1197`; `app/chamados/services/declaracao_texto.py:377`. F1 aparece respondida no anexo recente, precedendo c22 depois de E3/E4B; a lei antiga ainda aberta no RELATO da cópia é anterior e não deve prevalecer. (`backlog_obras.txt:1875`; `app/docs/RELATO.md:19`.)
- **RAIZ F2:** equivale a BO-DISPUTA e utiliza a cópia c21/c22 preservada: `app/chamados/management/commands/auditar_invariantes_chamados.py`, `app/chamados/services/declaracao_texto.py`, `app/chamados/services/descarte_disputa.py`, `app/chamados/services/disputa_emissao.py`, `app/core/observ.py`, `app/ponto/management/commands/reconciliar_grade.py`. (`arquivos_obras.txt`, bloco c21/c22: [nao verificado quanto à numeração do anexo]; `backlog_obras.txt:1875`; `backlog_obras.txt:1895`.)
- **RAIZ F3/F4:** reutilizar o mesmo censo, sem alegar divisão exaustiva entre as duas fases. Sítios centrais: `app/chamados/models.py:1853`, `:3103`; `app/chamados/services/materializacao.py:425`, `:875`, `:1158`; `app/chamados/services/validacao.py:134`, `:199`, `:461`; `app/chamados/services/disputa_emissao.py:1508`, `:1632`; `app/chamados/reconciliador.py:411`, `:611`, `:1170`; `app/chamados/services/becos.py:56`; `app/chamados/signals.py:238`; `app/chamados/services/explicador.py:137`; `app/chamados/relatorios_disputa.py:20`. Essas referências são as do `f3f4_censo.r` fornecido, não novo levantamento. (`f3f4_censo.r`: [nao verificado quanto à numeração do anexo].)
- **RAIZ F4 especificamente:** par da pausa pela célula, perda em `app/chamados/models.py:3441`, fonte `PARES_PAUSA_DNA`; não atribuir D8 como bloqueio de F3/F4. (`backlog_obras.txt:1896`; `leis_abertas.txt`, RELATO:31.)
- **RAIZ F5:** coordenada com E4d; o inventário fechado próprio dessa fase não foi fornecido. Pode citar a dependência da parada da projeção e preservar as seis exceções, sem inventar lista exaustiva de alterações. (`backlog_obras.txt:1895`; `app/docs/PROMPTS.md:2030`; `app/docs/PROMPTS.md:2034`.)
- **RAIZ F6/F7:** há nome e posição na sequência; escopo, arquivos, voltas e efeito visível específicos = “nao medido”. Não converter os lotes sugeridos do censo em fases oficiais. (`backlog_obras.txt:1895`; `app/docs/BACKLOG.md:1883`.)
- **O168:** `app/ponto/supra_juiz.py:539` e cartório do evento; retirar `--executar` depende da guarda funcionar no evento e de `CSF_RETRATADO` medido em zero. Nada disso é medida atual de fio_morto_alcance. (`backlog_obras.txt:1396`; `app/docs/BACKLOG.md:1395`; `app/ponto/services/cartorio.py:1091`.)
- **LASTRO:** o nome abreviado agrega obras distintas; não declarar uma única etapa para todo o agregado sem ressalva. Degrau 2 toca `app/ponto/services/lastro.py:274` e depende do juiz do par; parte 2 depende de JUIZ-1 e CRONS-INSTALL; parte 3 já pousou e toca emissão/reação. (`backlog_obras.txt:1732`; `backlog_obras.txt:1761`; `backlog_obras.txt:1763`.)

**C — cautela:** compartilhamento de arquivo prova cruzamento e necessidade de integração, não desmente sozinho uma ordem. O censo F3/F4 pede coordenação das testemunhas com E4c e parada com E4d, mas não fornece número que prove uma inversão obrigatória. (`f3f4_censo.r`, “Ordem sugerida”, item 5: [nao verificado quanto à numeração do anexo]; `app/docs/BACKLOG.md:1883`.)

A declaração META de O169-FECHADA “4 arquivos, 0 comuns com E3/E4B” diverge da contagem bruta da cópia anexa, que lista cinco arquivos, dos quais dois de produção; a ausência de arquivos comuns com E3/E4B continua confirmada pelo inventário fornecido. Isso é divergência de contagem, não premissa de ordem desmentida. (`meta18_linha.txt:1`; `arquivos_obras.txt`, blocos E3/E4B/O169-FECHADA: [nao verificado quanto à numeração do anexo].)

## 11/11 10:35 · sessao 5b642dda3528 · - · - · origem=Ronald (tmux)

Furos concretos para D:

- **QUEM CHAMA:** o caminho `logs/MAPA-20/mapa.md → MAPA-20.md` ainda não entra na raspagem, no staging nem no commit; acrescentar apenas o par não publica o arquivo. (`bin/relato.sh:103`, `bin/relato.sh:117`, `bin/relato.sh:121`, `bin/relato.sh:130`, `bin/relato.sh:131`)
- **QUEM ESPERA / LIMITE:** o publicador espera o lock por até 600 s e depende da obtenção da lista de nomes e dos geradores; o contrato não declara essas dependências nem como a atualização por evento trata falha/timeout. (`bin/relato.sh:27`, `bin/relato.sh:32`, `bin/relato.sh:35`, `bin/relato.sh:38`, `bin/relato.sh:42`, `bin/relato.sh:102`)
- **FALHA:** origem ausente é pulada com sucesso; não existe nesses dois programas validação de obras META-18 ou de células vazias. O `rc 1` prometido exige validação adicional antes da cópia. (`bin/raspar_publico.py:25`, `bin/raspar_publico.py:27`, `bin/raspar_publico.py:40`, `bin/raspar_publico.py:46`)
- **MEDIDA:** os programas contam substituições de nomes/CPF, não obras nem cobertura de `fio_morto_alcance`/Central; o contrato não define identificação única, tratamento das obras extras ou deduplicação de impactos. Implementação dessa conferência: [nao verificado]. (`bin/raspar_publico.py:41`, `bin/raspar_publico.py:45`)
- **FALHA DE PUBLICAÇÃO:** a atualização da linha local não prova publicação; o push pode falhar e não há retry por evento declarado no contrato. (`bin/relato.sh:132`)
- **QUEM DESLIGA:** falta definir a operação de desligamento e o destino da página já publicada; o script não possui condição de 20/20 e o próprio precedente documenta remoção do par **e** remoção do arquivo público. (`bin/relato.sh:101`, `bin/relato.sh:103`, `bin/relato.sh:132`)

Não executei os publicadores; execução, chamadas externas, estado público e comportamento da esteira em produção: [nao verificado]. [nao verificado]

## 11/11 10:34 · sessao 8051d24e8124 · MAPA-ATE-O-20 · sonda · origem=Code

```text
sonda_leitura: sonda_central.py [3912ec00c599cf5c] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

central_qs_na_fila n 3634
por_status_local 'aberto' n 2026
por_status_local 'em_analise' n 1608
por_categoria 'Ausencia' n 2710
por_categoria 'disputa_supervisao' n 323
por_categoria 'turno_aberto' n 171
por_categoria 'auditoria_clt' n 128
por_categoria 'Ponto' n 97
por_categoria 'sem_previsao_escala' n 63
por_categoria 'Outros' n 50
por_categoria 'batida_espuria' n 25
por_categoria 'turno_aberto_24h' n 18
por_categoria 'ausencia' n 14
por_categoria 'cadastro' n 12
por_categoria 'Escala' n 5
por_categoria 'App' n 3
por_categoria 'Documentos' n 3
por_categoria 'Ferias' n 3
por_categoria 'acesso_bloqueado' n 3
por_categoria 'Holerite' n 2
por_categoria 'agenda_aceite' n 2
por_categoria 'batida_fora_horario' n 1
por_categoria 'geofence' n 1
por_urgencia 'normal' n 1861
por_urgencia 'urgente' n 1295
por_urgencia 'emergencia' n 322
por_urgencia 'critica' n 140
por_urgencia 'consulta' n 16
agora 2026-11-11 10:34:04
```

## 11/11 10:28 · sessao 8a5868b1f67a · RAIA-2-BO-VISIVEL · sonda · origem=Code

```text
sonda_leitura: sonda_lote0311.py [63cd70bd29f499d7] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

LOTE 2026-10-28 09:33:35 pedidas=47 validadas=29 recusadas=18
LOTE 2026-10-28 09:33:57 pedidas=15 validadas=0 recusadas=15
LOTE 2026-10-28 09:34:20 pedidas=15 validadas=0 recusadas=15
LOTE 2026-10-28 09:34:38 pedidas=15 validadas=0 recusadas=15
LOTE 2026-10-28 09:35:34 pedidas=15 validadas=0 recusadas=15
LOTE 2026-10-28 09:40:34 pedidas=15 validadas=0 recusadas=15
LOTE 2026-10-28 14:19:49 pedidas=26 validadas=11 recusadas=15
LOTE 2026-10-28 14:20:09 pedidas=15 validadas=0 recusadas=15
LOTE 2026-10-28 15:41:19 pedidas=29 validadas=12 recusadas=17
LOTE 2026-10-28 16:24:03 pedidas=16 validadas=0 recusadas=16
LOTE 2026-10-28 16:24:24 pedidas=16 validadas=0 recusadas=16
LOTE 2026-10-29 13:27:43 pedidas=24 validadas=10 recusadas=14
LOTE 2026-10-29 14:19:11 pedidas=31 validadas=18 recusadas=13
LOTE 2026-10-29 14:42:19 pedidas=12 validadas=0 recusadas=12
LOTE 2026-10-29 14:42:38 pedidas=12 validadas=0 recusadas=12
LOTE 2026-10-29 14:43:01 pedidas=12 validadas=0 recusadas=12
LOTE 2026-10-29 14:43:19 pedidas=12 validadas=0 recusadas=12
LOTE 2026-10-30 08:20:15 pedidas=33 validadas=4 recusadas=29
LOTE 2026-10-30 08:22:45 pedidas=29 validadas=0 recusadas=29
LOTE 2026-10-30 08:24:19 pedidas=29 validadas=0 recusadas=29
LOTE 2026-10-30 08:42:57 pedidas=29 validadas=0 recusadas=29
LOTE 2026-10-30 09:48:15 pedidas=29 validadas=0 recusadas=29
LOTE 2026-10-30 12:12:51 pedidas=29 validadas=0 recusadas=29
LOTE 2026-10-30 12:30:44 pedidas=29 validadas=0 recusadas=29
LOTE 2026-10-30 12:33:23 pedidas=29 validadas=0 recusadas=29
LOTE 2026-10-30 12:35:49 pedidas=29 validadas=0 recusadas=29
LOTE 2026-10-30 13:04:01 pedidas=29 validadas=0 recusadas=29
LOTE 2026-10-30 15:28:31 pedidas=29 validadas=0 recusadas=29
LOTE 2026-10-30 15:29:07 pedidas=29 validadas=0 recusadas=29
LOTE 2026-11-03 08:06:21 pedidas=87 validadas=45 recusadas=42
LOTE 2026-11-03 16:26:49 pedidas=47 validadas=10 recusadas=37
LOTE 2026-11-05 08:02:56 pedidas=57 validadas=17 recusadas=40
LOTE 2026-11-05 16:11:28 pedidas=46 validadas=6 recusadas=40
LOTE 2026-11-06 07:59:36 pedidas=56 validadas=16 recusadas=40
LOTE 2026-11-06 10:21:21 pedidas=45 validadas=5 recusadas=40
LOTE 2026-11-11 08:56:00 pedidas=119 validadas=55 recusadas=64
LOTE 2026-11-11 09:20:37 pedidas=59 validadas=0 recusadas=59
linhas n 37
```

## 11/11 10:25 · sessao 8fe05992e025 · FIO-E0-V4-E4B · construtor · origem=Code

Adicionado spy no módulo vivo e assertion que detecta a entrada de `(colab, DIA)` no ato, mesmo após rollback: [teste:217](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4_backfill.py:217). [nao verificado]

Memória: 13 testes OK; `entrega_cerca_equivalente` falha pela nova assertion, com `bang:exportada` preservado. [Resultados:70](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v13.txt:70). [nao verificado]

[Causa, RED, mutantes e comandos ORM no recibo v13](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v13.txt:1). ORM desta volta [nao verificado]; certificação fica com Code/suite e opus final. [nao verificado]

## 11/11 10:21 · sessao ffcb7cf7e4ff · LOTE-VALIDAR-REGRESSAO-0311 · leitura · origem=Code

**Classificação: (c), indeterminado sem sonda.** Há multiplicações de trabalho comprováveis no caminho atual, mas faltam quantidade e composição dos lotes, execução concorrente e comparação das revisões efetivamente implantadas para atribuir a regressão a uma causa pontual. A primeira sonda pode aproveitar a auditoria que a própria view grava com as contagens do lote. (`app/chamados/views_cobrar.py:383`; `app/chamados/services/validacao.py:727`; implantação e medições faltantes: [nao verificado])

Uso “código atual” para a cópia fornecida. Ela contém alterações identificadas como posteriores ao primeiro POST lento, incluindo 05/11 e 06/11; portanto, seu custo inteiro não pode ser atribuído ao intervalo 30/10–03/11. (`app/ponto/portas/celula.py:891`; `app/chamados/signals.py:242`) Os diffs históricos por hash não puderam ser conferidos nesta cópia: [nao verificado].

**1. Caminho do POST e custo visível**

A rota chega a `validar_lote_tela`, verifica permissão e método, recebe `refs`, executa o lote, registra auditoria e renderiza o resultado. O HTTP 200 também pode apresentar recusa; não comprova materialização de todos os itens. (`app/chamados/urls.py:37`; `app/chamados/views_cobrar.py:376`; `app/chamados/views_cobrar.py:380`; `app/chamados/views_cobrar.py:386`; `app/templates/chamados/partials/_lote_preview.html:7`)

| Etapa | Chamada e trabalho |
|---|---|
| Seleção do lote | Converte referências para inteiros, deduplica em uma lista, faz **um SELECT inicial** das perguntas com suas disputas e percorre as referências **serialmente**. A deduplicação usa busca na própria lista, podendo exigir trabalho quadrático na quantidade de referências. (`app/chamados/services/validacao.py:713`; `app/chamados/services/validacao.py:719`; `app/chamados/services/validacao.py:724`; `app/chamados/services/validacao.py:727`) |
| Reclassificação por item | `classe_do_lote` resolve delta, resolve slot novamente e consulta existência de batida apurável em ±60 minutos. Só a classe A segue para validação. (`app/chamados/services/validacao.py:735`; `app/chamados/services/validacao.py:864`; `app/chamados/services/validacao.py:872`; `app/chamados/services/validacao.py:875`) |
| Entrada na validação | `acao_validar_pergunta` chama `validar_pergunta`, que busca novamente a pergunta, verifica resposta, validação anterior e implausibilidade. (`app/chamados/services/validacao.py:746`; `app/chamados/services/acoes_disputa.py:19`; `app/chamados/services/validacao.py:389`; `app/chamados/services/validacao.py:404`) |
| Carimbo | Dentro de transação, trava colaborador/disputas/chamados, grava `validada_em/por` e registra a decisão. A gravação da pergunta dispara seus receivers. (`app/chamados/services/validacao.py:425`; `app/chamados/services/validacao.py:431`; `app/chamados/services/validacao.py:435`; `app/chamados/services/validacao.py:137`; `app/chamados/services/validacao.py:139`) |
| Recálculo por validação | Agenda `recalcular_por_evento`, que chama o recálculo da **competência daquele colaborador**. (`app/chamados/services/validacao.py:446`; `app/ponto/services/fechamento.py:1809`; `app/ponto/services/fechamento.py:1810`; `app/ponto/services/fechamento.py:1824`) |
| Materialização da pergunta | Para resposta de hora ainda não materializada, chama `_materializar_uma`; depois verifica se precisa desfazer a validação recusada. (`app/chamados/services/validacao.py:461`; `app/chamados/services/validacao.py:463`; `app/chamados/services/validacao.py:464`) |
| Trabalho sobre irmãs | **A cada item**, chama a materialização da disputa: consulta todas as irmãs respondidas, validadas e não materializadas; tenta cada uma e verifica fechamento da disputa. (`app/chamados/services/validacao.py:469`; `app/chamados/services/materializacao.py:1369`; `app/chamados/services/materializacao.py:1375`; `app/chamados/services/materializacao.py:1378`) |
| Retorno | Atualiza a pergunta do banco, registra resultado e atualiza novamente a instância mantida pelo lote. (`app/chamados/services/validacao.py:472`; `app/chamados/services/validacao.py:479`; `app/chamados/services/validacao.py:752`) |

O POST recebe referências explícitas; a classificação da **fila global** pertence à contagem/prévia. Não há chamada dessa classificação global no corpo da view do POST. (`app/chamados/views_cobrar.py:348`; `app/chamados/views_cobrar.py:361`; `app/chamados/views_cobrar.py:380`)

No ramo de hora, `_materializar_uma` verifica a data do fato, abre transação, calcula o plantio, busca batida existente, verifica paridade e pode criar a batida. Depois reconcilia destino/clones, verifica lastro e carimba a pergunta pela porta que consulta o C6 e relavra seu veredito. (`app/chamados/services/materializacao.py:1192`; `app/chamados/services/materializacao.py:1220`; `app/chamados/services/materializacao.py:846`; `app/chamados/services/materializacao.py:859`; `app/chamados/services/materializacao.py:875`; `app/chamados/services/materializacao.py:927`; `app/chamados/services/materializacao.py:974`; `app/chamados/services/materializacao.py:1233`; `app/chamados/services/materializacao.py:1277`; `app/chamados/models.py:3139`; `app/chamados/models.py:3150`)

**Multiplicação importante:** quando cria uma batida e precisa ligar sua origem, há um INSERT de `Batida` e depois outro `save()` da mesma batida para preencher `pergunta_origem`. Esse segundo save dispara novamente os receivers que não filtram os campos alterados. (`app/ponto/registro_batida.py:157`; `app/chamados/services/materializacao.py:946`; `app/chamados/services/materializacao.py:948`; `app/ponto/registro_batida.py:254`; `app/ponto/signals.py:34`; `app/ponto/signals.py:131`; `app/ponto/signals.py:206`)

Os receivers relevantes são:

| Objeto salvo | Receiver → chamadas e efeitos |
|---|---|
| `Batida`, em cada save | `_nucleo_post_save_batida` → recomposição de turnos entre D−2 e D+1 → reconstrução das linhas, DELETE do recorte e `bulk_create`. (`app/ponto/signals.py:34`; `app/ponto/signals.py:26`; `app/ponto/nucleo.py:47`; `app/ponto/nucleo.py:58`; `app/ponto/nucleo.py:59`) |
| `Batida`, somente criação | Publicação de tempo real; onboarding pode ser marcado se ainda incompleto e houver login. (`app/ponto/signals.py:39`; `app/ponto/signals.py:42`; `app/ponto/signals.py:45`; `app/colaboradores/signals.py:94`; `app/colaboradores/signals.py:102`) |
| `Batida`, em cada save | `_cartorio_post_save_batida` → julgamento de D−1 e D, salvo guarda de reentrada. (`app/ponto/signals.py:131`; `app/ponto/signals.py:135`; `app/ponto/signals.py:112`; `app/ponto/signals.py:90`; `app/ponto/signals.py:93`) |
| `Batida`, em cada save | Reconciliação dos chamados do colaborador e tentativa de fechar adesão. (`app/ponto/signals.py:206`; `app/ponto/signals.py:215`; `app/ponto/signals.py:234`) |
| `PerguntaDisputa` | Beco em `on_commit` → relê disputa e perguntas, classifica e pode resolver perguntas/fechar disputa. (`app/chamados/signals.py:227`; `app/chamados/signals.py:238`; `app/chamados/services/becos.py:72`; `app/chamados/services/becos.py:29`; `app/chamados/services/becos.py:54`; `app/chamados/services/becos.py:57`) |
| `PerguntaDisputa`, código atual | Outbox síncrono se o save toca campos relevantes → consulta linhas/células → INSERT de evento. (`app/chamados/signals.py:250`; `app/chamados/signals.py:255`; `app/chamados/signals.py:257`; `app/chamados/models.py:3228`; `app/chamados/models.py:3248`) |
| `ChamadoColaborador`, pre-save | Consulta o estado anterior. (`app/chamados/signals.py:178`; `app/chamados/signals.py:184`) |
| `ChamadoColaborador`, post-save | Rebaixa urgência; resolve perguntas se o estado foi gravado como encerrado; julga a célula se houve mudança de estado e não estiver em julgamento. (`app/chamados/signals.py:53`; `app/chamados/signals.py:72`; `app/chamados/signals.py:90`; `app/chamados/signals.py:104`; `app/chamados/signals.py:115`; `app/chamados/signals.py:187`; `app/chamados/signals.py:216`) |
| `ChamadoColaborador`, condicionado | Silêncio de isento ao nascer/voltar a vivo; reconciliação do pai quando um filho deixa de ser vivo. (`app/chamados/signals.py:282`; `app/chamados/signals.py:293`; `app/chamados/signals.py:305`; `app/chamados/signals.py:349`; `app/chamados/signals.py:357`; `app/chamados/signals.py:368`) |
| `DisputaSupervisao` fechada | S88 reconcilia o pai e anexa nota; S144 percorre perguntas não materializadas e carimba as aprovadas; callback retoma regularizações confirmadas do colaborador. (`app/chamados/signals.py:18`; `app/chamados/signals.py:42`; `app/chamados/signals.py:45`; `app/chamados/signals.py:147`; `app/chamados/signals.py:163`; `app/chamados/services/materializacao.py:1418`; `app/chamados/signals.py:137`; `app/chamados/services/regularizacao_ext.py:218`; `app/chamados/services/regularizacao_ext.py:224`) |

Portanto, **no ramo de plantio novo + vínculo de origem**, o código solicita dois recomputes de turnos, quatro julgamentos de célula antes das guardas, duas reconciliações do colaborador e duas verificações de adesão. Agenda ainda dois recálculos mensais: um pela validação e outro pela criação da batida. Isso não significa quatro lavras efetivas: guardas e impressão podem pular trabalho. (`app/ponto/signals.py:26`; `app/ponto/signals.py:112`; `app/ponto/signals.py:135`; `app/ponto/signals.py:215`; `app/ponto/signals.py:234`; `app/chamados/services/validacao.py:446`; `app/ponto/registro_batida.py:160`; `app/ponto/services/cartorio.py:1369`)

O `on_commit` usado aqui registra callbacks que chamam diretamente recálculo/fechamento; essas chamadas não demonstram envio a um job. Tempo efetivamente gasto nesses callbacks nos POSTs históricos: [nao verificado]. (`app/ponto/services/fechamento.py:1807`; `app/ponto/services/fechamento.py:1810`; `app/ponto/services/fechamento.py:1824`; `app/chamados/signals.py:132`; `app/chamados/signals.py:144`)

Dentro de **cada julgamento**:

- `julgar_celula` consulta a célula, aplica limites temporais e chama `julgar_colab` com uma célula; depois consulta A2 e pode gravar `ata.precedencia`. (`app/ponto/services/cartorio.py:1987`; `app/ponto/services/cartorio.py:1993`; `app/ponto/services/cartorio.py:2000`; `app/ponto/services/cartorio.py:1959`; `app/ponto/services/cartorio.py:1960`; `app/ponto/services/cartorio.py:2008`)
- O corpo carrega batidas da janela, monta grade, consulta a primeira batida histórica e carrega chamados dos módulos pertinentes **com suas perguntas, sem filtro temporal ou de estado nesse carregador**. O teste de impressão vem depois. (`app/ponto/services/cartorio.py:1118`; `app/ponto/services/cartorio.py:1134`; `app/ponto/services/cartorio.py:1137`; `app/ponto/services/cartorio.py:1165`; `app/ponto/services/cartorio.py:117`; `app/ponto/services/cartorio.py:118`; `app/ponto/services/cartorio.py:1280`; `app/ponto/services/cartorio.py:1369`)
- Havendo adiados, executa E2 do colaborador, reconcilia chamados e pode fazer segunda passada forçada; emissão também pode provocar releitura. (`app/ponto/services/cartorio.py:962`; `app/ponto/services/cartorio.py:970`; `app/ponto/services/cartorio.py:974`; `app/ponto/services/cartorio.py:984`; `app/ponto/services/cartorio.py:1036`; `app/ponto/services/cartorio.py:1049`)
- Havendo células julgadas, executa cluster do colaborador e fábrica por célula; a fábrica pode alcançar pautas. (`app/ponto/services/cartorio.py:1001`; `app/ponto/services/cartorio.py:1005`; `app/ponto/services/cartorio.py:1008`; `app/chamados/services/fabrica_da_celula.py:214`; `app/chamados/services/fabrica_da_celula.py:279`)
- Status e rótulo entram no mesmo save da lavra. A consulta adicional a A2 em `fatos_do_status` é condicionada ao veredito `indefinida`; não cabe atribuir uma nova query ou save a todo item apenas pela existência das colunas. (`app/ponto/status_dia.py:177`; `app/ponto/status_dia.py:181`; `app/ponto/services/cartorio.py:1813`; `app/ponto/portas/celula.py:1038`; `app/ponto/portas/celula.py:1072`)

**Total exato de SQL, julgamentos concluídos, lavras e segundos por item em produção: [nao verificado].** Os números históricos escritos em comentários, como “122 ms”, não medem estes lotes. (`app/chamados/services/validacao.py:439`)

**2. Commits do anexo que alcançam arquivos desse caminho** [nao verificado]

A tabela cruza os arquivos declarados no anexo com o caminho acima. **“Tocou arquivo” não comprova “mudou função executada”. Para cada hash abaixo, o acréscimo histórico exato por item/POST permanece [nao verificado] sem diff.** A última coluna identifica o trabalho atual a conferir, sem tratá-lo como acréscimo comprovado daquele hash.

| Hashes do anexo | Arquivo alcançado / trabalho atual |
|---|---|
| `f059f0521` | Validação e cartório; encadeamento lavra → E2 → reconciliação → segunda passada condicionada. (`app/chamados/services/validacao.py:469`; `app/ponto/services/cartorio.py:962`; `app/ponto/services/cartorio.py:984`) |
| `22c2c2890`, `479b3aeeb`, `b3fc3919d`, `4f0476aa5`, `734dafb5a`, `38c502149`, `4150b482e` | Modelos/fio: fechamento consulta perguntas ancoradas, julga vivas, pode reapontar e relavrar perguntas. Custo por chamado afetado e respectivas perguntas. (`app/chamados/models.py:344`; `app/chamados/models.py:390`; `app/chamados/models.py:437`; `app/chamados/models.py:309`; `app/chamados/services/fio.py:909`) |
| `e046540e5`, `c1d9ee7c4`, `eb768cf2c`, `7b76efa7d`, `69ea3f8fe`, `3737d1027` | Cartório/modelos/reconciliador: absolvição, gates, projeção da célula deste julgamento e passadas condicionais. (`app/ponto/services/cartorio.py:959`; `app/ponto/services/cartorio.py:970`; `app/ponto/services/cartorio.py:984`; `app/ponto/portas/celula.py:932`) |
| `2692b5c50`, `587e5f175`, `d8be2e408`, `424ba425b`, `9e053c0e6` | Modelos e portas de pergunta; carimbo consulta C6 e pode relavrar antes do save. Alteração de comentário/assinatura versus execução por hash: [nao verificado]. (`app/chamados/models.py:3103`; `app/chamados/models.py:3139`; `app/chamados/models.py:3150`; `app/chamados/models.py:3152`) |
| `4ea6ffd2b`, `364ceb42e`, `ae0502e59` | Status na lavra; processamento do status e A2 condicionado a `indefinida`, no save combinado. (`app/ponto/services/cartorio.py:1813`; `app/ponto/services/cartorio.py:1817`; `app/ponto/status_dia.py:177`; `app/ponto/portas/celula.py:1072`) |
| `9d34ce228`, `8cea1769a`, `3a1e285aa`, `8b3858097`, `69d8b7d89`, `a531c8d1b`, `90a1249a0`, `5ec1220ad`, `f408ab32a` | Tocam `status_dia.py`, também usado pelo escritor; títulos sobre D4/calendário não bastam para excluir o arquivo. Hunk executado e custo novo: [nao verificado]. (`app/ponto/status_dia.py:113`; `app/ponto/status_dia.py:166`; `app/ponto/services/cartorio.py:1817`) |
| `15cb377f5` | Modelo de célula no caminho. A migration não é chamada pelo POST; efeito de índice/esquema instalado: [nao verificado]. (`app/ponto/services/cartorio.py:1974`; `app/ponto/services/cartorio.py:1987`) |
| `8ec526f23`, `9f0e75426`, `60258b809`, `2204220e4` | Cartório/grade/precedência: consulta ao juiz da folga e impressão/versionamento. (`app/escala/utils.py:1140`; `app/ponto/precedencia.py:194`; `app/ponto/services/cartorio.py:471`; `app/ponto/services/cartorio.py:1369`) |
| `36fbac826`, `f408ab32a`, `fe9736178`, `5ec1220ad`, `399565438`, `389775e00`, `d03410912`, `356a30b62` | Cobertura parcial, dispensa por marco, instante anterior e portões de emissão/lista. Podem alterar quais ramos e lavras acontecem; quantidade real afetada: [nao verificado]. (`app/ponto/services/cartorio.py:370`; `app/ponto/services/cartorio.py:427`; `app/ponto/services/cartorio.py:951`; `app/ponto/services/cartorio.py:1717`; `app/ponto/services/cartorio.py:1801`) |
| `4ef817f17`, `b89f191ff` | Versão de regra/impressão: mudança de impressão pode impedir o pulo e causar rejulgamento. Delta de cada hash: [nao verificado]. (`app/ponto/services/cartorio.py:132`; `app/ponto/services/cartorio.py:471`; `app/ponto/services/cartorio.py:1369`) |
| `59de66664`, `1222ab98c` | Isento na impressão, com horizonte/versionamento. Pode invalidar impressão para os casos elegíveis; incidência no lote: [nao verificado]. (`app/ponto/services/cartorio.py:520`; `app/ponto/services/cartorio.py:1362`; `app/ponto/services/cartorio.py:1369`) |
| `6995671cb` | `cods` próprio do ato, usado para decidir fábrica/cluster. O código atual restringe esses consumidores ao resultado do ato; não demonstra acréscimo universal. (`app/ponto/services/cartorio.py:944`; `app/ponto/services/cartorio.py:997`; `app/ponto/services/cartorio.py:1001`; `app/ponto/services/cartorio.py:1008`) |
| `d0d75b8f4`, `c5c433608`, `4236474ca`, `17e595092`, `399565438` | Fábrica da célula: decisão de perguntar, parcial só lista e encaminhamento de limbo exportado para pauta. (`app/chamados/services/fabrica_da_celula.py:112`; `app/chamados/services/fabrica_da_celula.py:123`; `app/chamados/services/fabrica_da_celula.py:214`; `app/chamados/services/fabrica_da_celula.py:275`) |
| `40e1bac20`, `9e9590eee`, `d0d75b8f4` | Cluster chamado pelo julgamento; leitura de células/achados e filtro de parcial. (`app/ponto/services/cartorio.py:1005`; `app/ponto/services/cluster_espurio.py:46`; `app/ponto/services/cluster_espurio.py:102`) |
| `387558b71` | `disputa_emissao.py`, alcançado pelos encerramentos e respectivos receivers; mudança específica executada neste ramo: [nao verificado]. (`app/chamados/signals.py:115`; `app/chamados/services/disputa_emissao.py:925`) |
| `72eb11be1`, `d03410912`, `b2fba08fd`, `5ec1220ad`, `fe9736178`, `36fbac826` | Leitor da célula/grade, chamado também pela materialização. (`app/chamados/services/materializacao.py:232`; `app/chamados/services/materializacao.py:281`; `app/escala/services/leitor_celula.py:350`) |
| `b1e3f259e`, `c1c6f565e`, `5ec1220ad`, `36fbac826`, `8ec526f23` | Builders/overlays da grade. (`app/escala/utils.py:1130`; `app/escala/utils.py:1156`; `app/escala/utils.py:1231`; `app/escala/utils.py:2084`) |
| `d117b1201`, `b3bef4032`, `36fbac826`, `f408ab32a`, `fe9736178` | Classificador em `supra_juiz.py`, chamado pelo cartório. (`app/ponto/services/cartorio.py:1237`; `app/ponto/services/cartorio.py:1424`) |
| `f9be08e08`, `de1206907`, `d871bcca8`, `41abcd1f6`, `8cb8a4c55`, `8b3858097`, `36fbac826`, `5ec1220ad`, `b2fba08fd`, `90a1249a0` | `turnos.py` no caminho de encerramento/grade/parcial. Hunk efetivamente alcançado por cada alteração: [nao verificado]. (`app/ponto/services/cartorio.py:1116`; `app/ponto/turnos.py:2127`; `app/ponto/turnos.py:2616`) |
| `03c8d106e`, `5394309c0` | Motor: processamento de intervalos/noturnas por período no recálculo. (`app/ponto/motor_calculo_v2.py:121`; `app/ponto/motor_calculo_v2.py:162`; `app/ponto/motor_calculo_v2.py:173`) |
| `bd9fb40be`, `f9a65d662` | Calculador: dobra/noturnas/pontualidade e intervalos, alcançados por dia da competência recalculada. (`app/ponto/services/fechamento.py:1267`; `app/ponto/services/fechamento.py:1271`; `app/ponto/calculador/alimentacao.py:472`; `app/ponto/calculador/alimentacao.py:527`; `app/ponto/calculador/regras.py:783`; `app/ponto/calculador/regras.py:824`) |
| `eaa414cf7`, `50d1c15ce`, `17e595092` | Pautas, alcançadas condicionalmente pela fábrica; montagem de contexto/link. (`app/chamados/services/fabrica_da_celula.py:279`; `app/pautas/services.py:153`; `app/pautas/services.py:400`) |
| `fc9beecea`, `74123bb1d`, `971374506`, `b621e0c7b`, `794ee000b`, `7f3cc8757`, `68821bbd7`, `01ae542e2`, `1ea81aebc`, `cc62d1683` | `vigia_fio.py` é alcançado condicionalmente pela consulta de competência na costura. Isso **não demonstra execução do censo F5** alterado nesses commits. (`app/chamados/services/fio.py:774`; `app/chamados/services/fio.py:776`) |
| `543d2e524`, `68821bbd7`, `366e07d4f`, `a625b893e`, `1ea81aebc`, `cc62d1683`, `8fef4d216`, `a30153c7f`, `9651ef38e` | Tocam `chamados/juizes.py`, arquivo executado pelo POST. Custo adicional das funções especificamente alteradas: [nao verificado]. (`app/chamados/services/validacao.py:864`; `app/chamados/models.py:3139`) |
| `15136c126` | Toca `fio.py`, mas alteração de proposta/modal não prova 

[truncado em 20000 de 38148 caracteres]

## 11/11 10:19 · sessao 1aa4f63738d0 · O169-FECHADA · verificador · origem=Code

**O delta cura a instância anterior à trava e a reabertura sem trilha do legado. Não identifiquei defeito novo demonstrável nos quatro pontos pedidos.** (`app/ponto/services/cluster_espurio.py:278`; `app/ponto/services/cluster_espurio.py:294`; `app/ponto/services/cluster_espurio.py:295`; `app/ponto/services/cluster_espurio.py:298`)

Da rodada 1, mantendo a numeração:

1. **Escritores/leitores — já regular; preservado.** Continua renascimento → `reabrir` → `_gravar_estado`; a auditoria continua pelo escritor `evento`. Nenhuma escrita direta nova dos três campos. (`app/ponto/services/cluster_espurio.py:284`; `app/chamados/models.py:1019`; `app/chamados/models.py:1442`; `app/core/observ.py:134`)

2. **Juízes — sem defeito novo; concordo com a triagem.** Relatório e renascimento consultam a mesma casca de tranca. A diferença entre dia contado e competência permanece preexistente. (`app/ponto/services/cluster_espurio.py:251`; `app/chamados/models.py:1009`; `app/chamados/models.py:1043`; `app/chamados/juizes.py:2275`)

3. **Relógio — não curado, mas descartado como defeito deste delta.** Concordo: permanece dentro do caminho preexistente competência → perguntas vivas → janela. (`app/chamados/juizes.py:2286`; `app/chamados/juizes.py:2267`; `app/chamados/juizes.py:577`)

4. **Trava/TOCTOU — curado o achado material, severidade alta.** Chamado e colaborador agora nascem depois da trava C; o contador é reconsultado sob ela. `esperados` continua validando a fotografia inicial; concordo em não exigir atomicidade global das linhas. (`app/ponto/services/cluster_espurio.py:261`; `app/ponto/services/cluster_espurio.py:275`; `app/ponto/services/cluster_espurio.py:276`; `app/ponto/services/cluster_espurio.py:278`; `app/ponto/services/cluster_espurio.py:280`)

5. **Campos do relatório — parcialmente curado, baixa.** Agora há `parado` para saída do universo e recusa. Porém `recusa`, `ativo` e classificação continuam sendo a fotografia anterior à trava; não são atualizados pelo apply. `aplicadas` continua contando sucessos, não todas as escritas possíveis de uma recusa. (`app/ponto/services/cluster_espurio.py:256`; `app/ponto/services/cluster_espurio.py:257`; `app/ponto/services/cluster_espurio.py:281`; `app/ponto/services/cluster_espurio.py:285`; `app/chamados/models.py:1427`; `app/chamados/models.py:1436`)

6. **Testes/mutantes — parcialmente curado, média.** A falha de trilha ganhou cenário e mutante; a correção da releitura após trava continua sem cenário concorrente correspondente nos testes do legado. (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:138`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:246`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:100`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:167`)

7. **Porta de relavra — curada a afirmação excessiva.** O texto agora distingue transição via `admin` de relavra para `competencia_trancada`; o gate dedicado continua recusando encerrados. Concordo com manter esse ramo PARADO nesta triagem. (`app/ponto/services/cluster_espurio.py:220`; `app/ponto/services/cluster_espurio.py:269`; `app/chamados/models.py:1630`; `app/chamados/models.py:1639`)

8. **Outros materiais — parcialmente curado.**
   - **Alta, curado:** ausência da trilha específica agora provoca rollback. **Média, residual:** falha seletiva de `estado_chamado` continua ignorada pelo núcleo. (`app/ponto/services/cluster_espurio.py:293`; `app/ponto/services/cluster_espurio.py:294`; `app/chamados/models.py:292`; `app/core/observ.py:139`)
   - **Baixa, parcialmente curado:** vazio/todas trancadas evitam criar usuário; existindo `REABRIR`, `get_or_create` ainda ocorre fora do atomic, mesmo que a linha depois recue. (`app/ponto/services/cluster_espurio.py:269`; `app/ponto/services/cluster_espurio.py:272`; `app/ponto/services/cluster_espurio.py:275`; `app/core/utils.py:18`)
   - **Proveniência parcialmente curada:** a auditoria passou a declarar RECONCILIAR, coerente com o contador; a docstring ainda equipara esse universo a fechamento por idade. (`app/ponto/services/cluster_espurio.py:93`; `app/ponto/services/cluster_espurio.py:228`; `app/ponto/services/cluster_espurio.py:289`)
   - **Relavra não implementada:** permanece a diferença para o aval integral, aceita como parada nesta triagem. (`app/docs/CORTES.md:435`; `app/ponto/services/cluster_espurio.py:234`; `app/ponto/services/cluster_espurio.py:269`)

Quanto às perguntas 2–5:

- **2 — Quem escreve e lê.** `_gravar_estado` escreve `status_local`, limpa `via_encerramento`/`resolvido_em` e salva; `evento` escreve `LogAuditoria`. Antes da trava, contador e relatório leem estado/via, e `recusa_da_reabertura` pode ler `resolvido_em`. Depois da trava, o apply relê chamado/colaborador e universo. Histórico/vigia consomem `estado_chamado`; o teste consulta a ação do legado. Portanto **há leituras anteriores à trava**, mas a instância usada pela porta foi corrigida. (`app/chamados/models.py:228`; `app/chamados/models.py:248`; `app/chamados/models.py:296`; `app/core/observ.py:134`; `app/ponto/services/cluster_espurio.py:107`; `app/ponto/services/cluster_espurio.py:253`; `app/chamados/models.py:1342`; `app/ponto/services/cluster_espurio.py:278`; `app/chamados/services/historico.py:44`; `app/chamados/services/vigia_loop.py:114`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:115`)

- **3 — Mesma transação: sim, por linha.** Reabertura e ação `reabrir_legado_cluster_o169` estão dentro do mesmo atomic. `None` levanta `_SemTrilha` dentro dele e é capturado fora; outra exceção propaga. Não há separação de commit demonstrada entre essas duas escritas. Pode persistir reabertura **sem `estado_chamado`**, se somente essa emissão falhar e a emissão do legado funcionar. Recusas também podem gravar outras auditorias sem reabrir, comportamento preexistente. (`app/ponto/services/cluster_espurio.py:275`; `app/ponto/services/cluster_espurio.py:284`; `app/ponto/services/cluster_espurio.py:288`; `app/ponto/services/cluster_espurio.py:294`; `app/ponto/services/cluster_espurio.py:295`; `app/chamados/models.py:292`; `app/chamados/models.py:1396`)

- **4 — Teste e mutante mordem, por inspeção.** O patch alcança o import local; a foto inclui estado/via/carimbo/notas/logs. Porém o mutante morre primeiro em `aplicadas == 0`, antes da foto: sua morte isolada não comprova rollback. Falta falha seletiva apenas da ação legado, pois o patch desliga todas as emissões. Mutante plausível sem cenário correspondente: remover a trava explícita anterior à releitura; os casos sequenciais não exercitam a espera que causava o defeito. Sobrevivência executada: **[nao verificado]**. (`app/ponto/services/cluster_espurio.py:244`; `app/ponto/services/cluster_espurio.py:276`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:75`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:141`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:143`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:145`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:246`)

- **5 — Sem defeito novo demonstrado.** Só `_SemTrilha` é capturada; trancadas pulam antes de qualquer sobrescrita de `parado`; recusas/falhas continuam antes do incremento. Sucesso sai do universo de encerrados; rollback permite tentar novamente. (`app/ponto/services/cluster_espurio.py:269`; `app/ponto/services/cluster_espurio.py:282`; `app/ponto/services/cluster_espurio.py:286`; `app/ponto/services/cluster_espurio.py:295`; `app/ponto/services/cluster_espurio.py:298`; `app/ponto/services/cluster_espurio.py:107`; `app/chamados/models.py:248`)

Execução Django e ocorrência desses cenários em produção: **[nao verificado]**.

## 11/11 10:18 · sessao a8715dbb8cc7 · - · - · origem=Ronald (tmux)

- **Curado — alto:** instância e colaborador usados pelo apply agora são carregados após trava C; leitura anterior conserva somente `cid`, usado também para revalidar pertencimento ao universo. O relatório continua sendo fotografia anterior à trava. (`app/ponto/services/cluster_espurio.py:250`; `app/ponto/services/cluster_espurio.py:273`; `app/ponto/services/cluster_espurio.py:276`; `app/ponto/services/cluster_espurio.py:278`; `app/ponto/services/cluster_espurio.py:280`)

- **Curado — alto, para a ação legado:** reabertura, nota, ressuscitação e `reabrir_legado_cluster_o169` ficam dentro do mesmo atomic; `evento=None` lança `_SemTrilha` dentro e só é capturado após sair dele, desfazendo a linha. Falha de commit ou outra exceção propaga; não chega ao incremento. (`app/ponto/services/cluster_espurio.py:275`; `app/ponto/services/cluster_espurio.py:284`; `app/ponto/services/cluster_espurio.py:288`; `app/ponto/services/cluster_espurio.py:294`; `app/ponto/services/cluster_espurio.py:295`; `app/ponto/services/cluster_espurio.py:298`; `app/chamados/models.py:1022`; `app/chamados/models.py:1451`)

- **Residual — médio:** `estado_chamado` permanece best-effort com retorno ignorado; falha seletiva nessa ação pode deixar reabertura e ação legado gravadas sem a linha de estado. Não é defeito novo. (`app/chamados/models.py:292`; `app/chamados/models.py:296`; `app/core/observ.py:105`; `app/core/observ.py:133`; `app/core/observ.py:139`)

- **Escritor canônico preservado:** tratamento chama renascimento → `reabrir` → `_gravar_estado`, que escreve `status_local`, limpa `resolvido_em`/via e salva; ambas auditorias usam `evento` → `LogAuditoria.objects.create`. Não há escrita nova direta desses campos. (`app/ponto/services/cluster_espurio.py:284`; `app/ponto/services/cluster_espurio.py:288`; `app/chamados/models.py:1019`; `app/chamados/models.py:1442`; `app/chamados/models.py:228`; `app/chamados/models.py:248`; `app/chamados/models.py:249`; `app/chamados/models.py:296`; `app/core/observ.py:134`)

- **Leitores:** contador lê estado/via antes e depois da trava; relatório lê estado/via e executa `recusa_da_reabertura` antes dela; `recusa_da_reabertura` inclui leitura de `resolvido_em` no ramo administrativo. Histórico/vigia leem `estado_chamado`, não a ação legado. (`app/ponto/services/cluster_espurio.py:107`; `app/ponto/services/cluster_espurio.py:110`; `app/ponto/services/cluster_espurio.py:253`; `app/ponto/services/cluster_espurio.py:256`; `app/ponto/services/cluster_espurio.py:280`; `app/chamados/models.py:1342`; `app/chamados/services/historico.py:44`; `app/chamados/services/historico.py:86`; `app/chamados/services/vigia_loop.py:114`)

- **Sem defeito novo demonstrado de exceção, `parado`, contador ou idempotência:** somente `_SemTrilha` é capturada e ganha motivo explícito; linhas trancadas são puladas, portanto seu `parado` não é sobrescrito; recusas/saída do universo/trilha ausente fazem `continue` antes do incremento. Reabertura bem-sucedida sai do filtro de encerrados; rollback conserva elegibilidade para nova tentativa. (`app/ponto/services/cluster_espurio.py:258`; `app/ponto/services/cluster_espurio.py:269`; `app/ponto/services/cluster_espurio.py:280`; `app/ponto/services/cluster_espurio.py:284`; `app/ponto/services/cluster_espurio.py:295`; `app/ponto/services/cluster_espurio.py:298`; `app/ponto/services/cluster_espurio.py:107`; `app/chamados/models.py:1363`)

- **Parcialmente curado — baixo:** vazio/todas trancadas agora evitam criar usuário sistema; havendo linha `REABRIR`, `get_user_sistema` permanece fora do atomic e pode criar usuário mesmo quando a porta recusa ou a trilha causa rollback. (`app/ponto/services/cluster_espurio.py:269`; `app/ponto/services/cluster_espurio.py:271`; `app/ponto/services/cluster_espurio.py:275`; `app/core/utils.py:18`)

## 11/11 10:17 · sessao 44d9bf1f107a · - · - · origem=Ronald (tmux)

- **Curado por inspeção; execução [nao verificado].** O teste compara estado, via, `resolvido_em`, followups e quantidade de auditorias antes/depois; detecta persistência da reabertura quando a trilha retorna `None`. O import local permite o patch alcançar o tratamento e o núcleo. (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:75`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:138`; `app/ponto/services/cluster_espurio.py:244`; `app/chamados/models.py:255`)

- **Baixa — mordida do mutante menos específica que o nome.** `reabre_sem_trilha` elimina o `raise`; o teste falha primeiro em `aplicadas == 0`, antes de verificar a foto persistida. O harness aceita qualquer falha de asserção do caso, embora rejeite erros e tenha controle intacto. (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:143`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:145`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:246`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:299`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:308`)

- **Média — cobertura parcial da falha de auditoria.** O patch retorna `None` para todas as ações, inclusive `estado_chamado`; falta cenário seletivo em que o núcleo grava a auditoria e somente `reabrir_legado_cluster_o169` falha. A foto atual verifica a contagem total após rollback, mas a execução com auditoria real já inserida não é exercitada nesse caso. (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:78`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:141`; `app/chamados/models.py:292`; `app/ponto/services/cluster_espurio.py:288`)

- **Média — mutante plausível sem cenário correspondente:** remover a trava explícita anterior à releitura preserva os casos sequenciais existentes, mas desfaz a correção do objeto lido depois da trava. A lista de mutantes não altera essa trava; os testes de apply usam fixtures estáveis e não instrumentam espera ou mudança de situação durante a aquisição. Sobrevivência executada: **[nao verificado]**. (`app/ponto/services/cluster_espurio.py:276`; `app/ponto/services/cluster_espurio.py:278`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:100`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:167`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:220`)

## 11/11 10:17 · sessao 441b3c861fbf · - · - · origem=Ronald (tmux)

Cartório: resultados comprováveis na cópia.

- Cada `julgar_celula` consulta uma `CelulaDia`; pode sair por data futura, célula inexistente ou turno atual ainda aberto; cria contadores próprios e chama `julgar_colab` com uma célula. Depois aplica A2 e grava `ata.precedencia` se mudou. (`app/ponto/services/cartorio.py:1971`, `:1985`, `:1987`, `:1993`, `:1999`, `:2000`, `:2008`, `:1958`, `:1960`)
- Cada passada do corpo carrega batidas de `[D−1,D+3)`, monta grade de dois dias para uma célula, consulta primeira batida histórica, carrega todos os chamados dos módulos de batida daquele colaborador e suas perguntas — sem filtro temporal nem de estado — e consulta ausência por célula. O pulo por impressão vem depois dessas cargas. (`app/ponto/services/cartorio.py:1118`, `:1134`, `:1137`, `:1165`, `:1280`, `:117`, `:118`, `:1317`, `:1369`, `:1373`)
- Cadastro e encerramento são cargas adicionais preguiçosas por passada: todos os vínculos e `dias_encerrados`, que também carrega turnos quando não fornecidos. (`app/ponto/services/cartorio.py:1102`, `:1105`, `:1106`, `:1113`, `:1116`; `app/ponto/turnos.py:2147`, `:2148`, `:2150`)
- Há segunda passada condicional: lavra → E2 do colaborador → reconciliação → `_corpo(forcar=True)` nas células reconciliadas. Emissão pode provocar outra releitura da lista inteira. Não é correto dizer “sempre três julgamentos”. (`app/ponto/services/cartorio.py:962`, `:969`, `:970`, `:974`, `:980`, `:984`, `:1036`, `:1049`)
- Após julgamento efetivo há cluster do colaborador e fábrica por célula julgada. Isso se repete em cada `julgar_celula`, sem cache por POST demonstrado nesse código. (`app/ponto/services/cartorio.py:997`, `:1001`, `:1005`, `:1006`, `:1008`)
- Cluster ativo consulta existência de vínculo, células de 14 dias, células dos achados para parcial, chamados vivos e suas células; pode fazer `exists` por achado. (`app/ponto/services/cluster_espurio.py:119`, `:121`, `:46`, `:102`, `:66`, `:71`, `:84`, `:107`)
- Status do dia acrescenta processamento no mesmo save da lavra; consulta A2 adicional apenas quando o veredito é `indefinida`. Nova coluna não significa automaticamente uma query ou save adicional por item. (`app/ponto/services/cartorio.py:1813`, `:1817`; `app/ponto/status_dia.py:177`, `:181`; `app/ponto/portas/celula.py:1038`, `:1070`, `:1072`)
- `lavrar_veredito` grava auditoria; `carimbar` combina os campos num save; `salvar_e_emitir` insere `EventoCelula` quando mudam campos que acordam o consumidor. (`app/ponto/portas/celula.py:1041`, `:1042`, `:1052`, `:1070`, `:1072`, `:903`, `:905`, `:907`, `:896`)
- Validação agenda recálculo da competência inteira do colaborador; materialização com Batida também pode agendá-lo. Há ausência explícita de deduplicação nessa costura, mas os tempos citados no comentário não medem os POSTs fornecidos. (`app/chamados/services/validacao.py:435`, `:439`, `:445`, `:446`; `app/ponto/services/fechamento.py:1807`, `:1810`, `:1824`)
- Recálculo alcança motor e calculador por dia da competência, inclusive regras de dobra/noturnas/intervalos. (`app/ponto/services/fechamento.py:1265`, `:1269`, `:1271`; `app/ponto/calculador/alimentacao.py:472`, `:514`, `:527`; `app/ponto/calculador/regras.py:783`, `:799`, `:824`, `:827`; `app/ponto/motor_calculo_v2.py:162`, `:164`, `:173`, `:177`)

Interseções de commits com arquivos executados, conforme anexo; delta exato por hash permanece [nao verificado], pois a cópia não dispõe de histórico Git acessível. [nao verificado]

- Cartório: `1222ab98c`, `59de66664` → isento na impressão (`cartorio.py:520`); `6995671cb` → `cods` próprio do ato (`:944`, `:997`, `:1079`); `b89f191ff`, `4ef817f17` → versão de regra (`:132`, `:471`); `389775e00` → instante anterior (`:951`); `399565438`, `5ec1220ad`, `fe9736178`, `f408ab32a`, `36fbac826`, `d03410912`, `356a30b62` → parcial/dispensa/emissão (`:370`, `:427`, `:1384`, `:1717`, `:1801`); `60258b809`, `9f0e75426`, `8ec526f23` → folga/versão (`:132`; `escala/utils.py:1140`); `ae0502e59`, `364ceb42e`, `4ea6ffd2b` → status na lavra (`:1813`); `3737d1027`, `69ea3f8fe`, `7b76efa7d`, `eb768cf2c`, `c1d9ee7c4`, `e046540e5`, `734dafb5a`, `38c502149`, `f059f0521` → fecho/reabertura/passadas (`:959`, `:984`, `:1418`, `:1470`, `:1494`); `b3bef4032` → insumo dia sem trabalho (`:1424`).
- `ponto/status_dia.py`: `90a1249a0`, `a531c8d1b`, `69d8b7d89`, `5ec1220ad`, `f408ab32a`, `8b3858097`, `3a1e285aa`, `364ceb42e`, `8cea1769a`, `9d34ce228`, `4ea6ffd2b` — escritor alcançado em `:166`, `:113`; alteração efetiva desses hunks [nao verificado].
- `ponto/turnos.py`: `90a1249a0`, `b2fba08fd`, `5ec1220ad`, `36fbac826`, `8b3858097`, `8cb8a4c55`, `41abcd1f6`, `d871bcca8`, `de1206907`, `f9be08e08` — alcançado por `dias_encerrados(:2127)` e parcial (`:2616`); alteração efetiva dos hunks [nao verificado].
- `escala/utils.py`: `b1e3f259e`, `c1c6f565e`, `5ec1220ad`, `36fbac826`, `8ec526f23` — builders/overlays (`:2084`, `:1130`, `:1156`, `:1231`).
- `ponto/precedencia.py`: `2204220e4`, `8ec526f23`, `364ceb42e` — A2 e folga (`:194`, `:212`, `:242`, `:259`).
- `ponto/supra_juiz.py`: `fe9736178`, `f408ab32a`, `36fbac826`, `b3bef4032`, `d117b1201` — classificador chamado pelo cartório (`cartorio.py:1237`, `:1424`).
- `ponto/portas/celula.py`: `4ea6ffd2b`, `3737d1027` — lavra e projeção (`:932`, `:974`).
- `escala/models.py`: `15cb377f5`, `4ea6ffd2b` — modelo em uso; migrations correspondentes não são executadas pelo POST (`cartorio.py:1974`, `:1987`).
- `ponto/services/cluster_espurio.py`: `9e9590eee`, `40e1bac20`, `d0d75b8f4` (`:25`, `:102`).
- `chamados/services/fabrica_da_celula.py`: `399565438`, `17e595092`, `4236474ca`, `c5c433608`, `d0d75b8f4` (`:112`, `:123`, `:214`).
- `chamados/services/materializacao.py`: `d8be2e408`, `5ec1220ad`, `fe9736178`, `d0d75b8f4`; alcança `leitor_celula.grade_da_celula` (`:232`, `:233`, `:281`, `:282`).
- `escala/services/leitor_celula.py`: `72eb11be1`, `d03410912`, `b2fba08fd`, `5ec1220ad`, `fe9736178`, `36fbac826` (`:350`).
- Motor/calculador: `03c8d106e`, `5394309c0` (`motor_calculo_v2.py:121`); `bd9fb40be` (`calculador/regras.py:783`); `f9a65d662` (`calculador/alimentacao.py:108`, `:133`, `:527`; `calculador/periodo.py:744`).
- Pautas condicionais: `eaa414cf7`, `17e595092`, `50d1c15ce`, alcançados por fábrica (`fabrica_da_celula.py:275`, `:279`) → `pautas/services.py:153`, `:168`, `:400`, `:428`.
- `core/juizes.py` e `core/portas.py` são registros declarativos; custo executado do POST decorrente de seus commits [nao verificado]. Chamadas de fase/cadastro_realidade no núcleo investigado [nao verificado]; verificar render final do POST.

Campos da célula e leitores comprovados:

| Campo | Escritor | Leitor |
|---|---|---|
| `ata` | `ponto/portas/celula.py:1033`, `:1083` | `ponto/services/cluster_espurio.py:27`; `escala/services/leitor_celula.py:350` |
| `veredito`, `protestos` | `ponto/portas/celula.py:1034`, `:1035` | `folha/export.py:1233`, `:1246`, `:1254` |
| `veredito_via` | `ponto/portas/celula.py:1036` | mesmo arquivo `:920`, `:1003` |
| `veredito_em` | `ponto/portas/celula.py:1037` | `chamados/veredito_lavrado.py:193` |
| `status_dia`, `status_rotulo` | `ponto/portas/celula.py:970` | `ponto/status_dia.py:287`, `:288`, `:289`; calendário `:602`, `:606` |
| `julgada_em`, `impressao` | `ponto/portas/celula.py:1065`, `:1066` | `ponto/services/cartorio.py:1369`; `chamados/veredito_lavrado.py:194` |
| `ausencia`, `tem_entrada`, `tem_saida`, `insumos_em` | `ponto/portas/celula.py:1067`, `:1068`, `:1069` | idempotência `:1061`, `:1062`, `:1063`; vigia `ponto/management/commands/vigia_b5_ausencia.py:71`, `:80` |

A cópia inclui código posterior ao primeiro POST lento — comentários identificam alterações de 05/11 e 06/11 no outbox —, portanto custo HEAD não equivale automaticamente ao custo de 03/11. (`app/ponto/portas/celula.py:891`, `:893`; implantação real [nao verificado])

## 11/11 10:16 · sessao ee78a9807660 · - · - · origem=Ronald (tmux)

Achados para consolidação, todos apenas da cópia:

- **Sonda retrospectiva do tamanho do lote já tem fonte no código:** `validou_lote_tela` registra horário da auditoria, quantidade bruta de refs, validadas e recusadas; o registro ocorre depois do serviço. Existência e cobertura dessas linhas em prod são [nao verificado]. (`app/chamados/views_cobrar.py:380-385`; `app/core/models.py:295-307`; auditoria best-effort: `app/core/auditoria.py:17-28`, `app/core/observ.py:105-138`)
- **N bruto difere de N processado:** refs são convertidas para int e deduplicadas; cada ref restante acaba validada ou recusada. Logo, `distintos_processados = validadas + recusadas`; `nao_gravados` já integra validadas e não deve ser somado novamente. (`app/chamados/services/validacao.py:713-727`, `:729-761`)
- **Sonda sugerida, sem nomes:** extrair somente `criado_em` e os três grupos numéricos do sufixo `: N perguntas pedidas, N validadas, N recusadas`, filtrando `acao='validou_lote_tela'`; não projetar descrição, usuário ou IP. Correspondência do timestamp do access log com início/fim da requisição permanece [nao verificado]. (`app/chamados/views_cobrar.py:383-385`; `app/core/models.py:295-307`)

```sql
-- Confirmar schema/tabela da instalação: [nao verificado].
SELECT criado_em,
       m[1]::int AS solicitados_brutos,
       m[2]::int AS validadas,
       m[3]::int AS recusadas,
       m[2]::int + m[3]::int AS distintos_processados
FROM (
  SELECT criado_em,
         regexp_match(
           descricao,
           ': ([0-9]+) perguntas pedidas, ([0-9]+) validadas, ([0-9]+) recusadas$'
         ) AS m
  FROM core_logauditoria
  WHERE acao = 'validou_lote_tela'
    AND criado_em >= TIMESTAMPTZ '2026-10-28 00:00:00-03'
    AND criado_em <  TIMESTAMPTZ '2026-11-07 00:00:00-03'
) a
ORDER BY criado_em;
```

Cron e concorrência:

- Há programação de `alertar_disputas_pendentes` às 08:00, `gerar_alertas` às 08:00/18:00 e `suporte_hasner` às 08:05. Há tarefas contínuas de 1, 5, 10, 15 e 30 minutos também às 16:xx. Estarem instaladas, executando e disputando recursos nesses POSTs é [nao verificado]. (`app/config/crons.py:362-398`, `:745-753`)
- Para separar concorrência: obter apenas intervalos início/fim dos comandos, quantidades de processos ativos e contagens de sessões PostgreSQL esperando lock/IO durante o POST. A presença dessas medições existentes em prod é [nao verificado]; horário agendado sozinho não comprova execução nem contenção. (`app/config/crons.py:362-398`, `:745-753`)

Campos e leitores:

| Entidade/campos | Escritor | Leitor comprovado |
|---|---|---|
| Pergunta: `validada_em`, `validada_por` | `app/chamados/services/validacao.py:133-141` | Estado: `app/chamados/models.py:3041-3047`; vitalidade: `app/chamados/juizes.py:1325`; placar humano: `app/chamados/services/painel.py:261-273` |
| Pergunta: `materializada_em`, `materializada_por`, `via_resolucao` | `app/chamados/models.py:3099-3101`, `:3128-3137`, `:3186-3189` | Vitalidade: `app/chamados/juizes.py:1336`, `:1362-1369`; autoria: `app/ponto/services/procedencia_batida.py:71-72` |
| Pergunta: `pendencia_admin`, `recusa_motivo` | `app/chamados/services/materializacao.py:850-851`, `:1102-1103` | `app/templates/chamados/partials/modal_fio.html:222-226` |
| Pergunta: `veredito_lavrado`, `julgado_em`, `janela_expira_em` | `app/chamados/veredito_lavrado.py:270-275` | `app/chamados/services/raiox.py:175-187`; validade: `app/chamados/veredito_lavrado.py:224-232` |
| Decisão: `pergunta`, `ato`, `ato_em`, `ato_por`, `ato_texto`, `ato_motivo` | `app/chamados/services/validacao.py:139-141`; bulk: `app/chamados/models.py:3953` | Sequência: `app/chamados/models.py:3500-3516`; autores e conteúdo: `app/templates/chamados/partials/modal_fio.html:205-212` |
| Disputa: `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento` | `app/chamados/models.py:1932-1936` | Idempotência: `app/chamados/models.py:1880-1881`; fila aberta: `app/chamados/services/fio.py:131-135`; motivo: `app/chamados/services/vigia_fio.py:734` |
| Turno: `colaborador`, `data_turno`, `entrada`, `saida`, `aberto`, `cross_meianoite`, `dt_fim_previsto` | `app/ponto/nucleo.py:48-59` | Todos: `app/ponto/management/commands/verificar_nucleo.py:30`; alertas: `app/ponto/management/commands/processar_alertas_turno.py:76-100` |
| Colaborador: `onboarding_completo` | `app/colaboradores/signals.py:94-102` | `app/colaboradores/queries.py:48` |
| Nota: `chamado_id`, `autor_tipo`, `autor_user`, `conteudo`, `anexo_hash`, `evento_chave`; `criado_em` automático | `app/chamados/services/nota.py:88-107`; automático: `app/chamados/models.py:1674` | Relação/autor/conteúdo/data: `app/templates/chamados/partials/modal_fio.html:461-473`; chave: `app/chamados/services/nota.py:109`; leitor de `anexo_hash` [nao verificado] |
| Auditoria: `usuario`, `acao`, `modelo`, `objeto_id`, `descricao`, `valor_antes`, `valor_depois`, `ip`, `ator`; `criado_em` automático | `app/core/observ.py:134-138`; automático: `app/core/models.py:307` | UI: `app/templates/core/log_auditoria.html:56-74`; leitores específicos de `valor_antes`, `valor_depois`, `ator` [nao verificado] |

FM: grava `atualizado_em`, `horas_trabalhadas`, `horas_noturnas`, `horas_extras`, `horas_extras_50`, `horas_extras_50_noturna`, `horas_extras_100`, `horas_extras_100_feriado`, `horas_extras_100_noturna`, `horas_folga_trabalhada`, `horas_atraso`, `horas_saida_antecipada`, `horas_falta`, `horas_intra_indenizada`, `turnos_abertos`, `inconsistencias`, `semanas_dsr_ok`, `semanas_dsr_perdido`, `horas_reflexo_dsr`, `saldo_banco_horas`, `minutos_previstos`, `dias_previstos`, `dias_incertos`, `minutos_abonados`, `minutos_realizados`, `motivos_espelho`, `causa_espelho`, `previsto_em`, `derivado_em`. Persistência por `save()` quando aberto ou `update()` dos campos calculados quando aprovado. (`app/ponto/services/fechamento.py:1143-1189`)

Leitores FM: resumo do cartão lê trabalhadas/extras/atraso/antecipada/abertos/inconsistências/banco/falta/folga/previstos/abono; rubricas folha leem noturnas/HE/folga/intra; export lê `previsto_em`, realizados e abonados; retrato lê causa; contador lê derivado; indicadores leem DSR perdido. Leitores individuais restantes não foram censados integralmente [nao verificado]. (`app/relatorios/cartao_pela_celula.py:94-120`; `app/folha/export.py:420-452`, `:1037-1070`; `app/folha/retrato_folha.py:1115`; `app/ponto/services/fechamento.py:1695-1696`; `app/inteligencia/views.py:119`)

DiaPago: grava `colaborador`, `mes`, `ano`, `data`, `tipo`, `detalhe`, `furo_apurado`, `versao`, `versao_motor`, `versao_dna`, `evento_origem`, `lavrado_em` e rubricas: `horas_trabalhadas`, `horas_noturnas`, `horas_extras_50`, `horas_extras_50_noturna`, `horas_extras_100`, `horas_extras_100_feriado`, `horas_extras_100_noturna`, `horas_folga_trabalhada`, `horas_atraso`, `horas_saida_antecipada`, `horas_falta`, `horas_intra_indenizada`, `minutos_abonados`, `minutos_previstos`, `minutos_realizados`, `horas_reflexo_dsr`, `saldo_banco_horas`; substitui todas as linhas de colaborador/competência/versão. (`app/ponto/services/dia_pago.py:34-44`, `:335-360`; automático: `app/ponto/models.py:380`)

Leitores DiaPago: soma todas as rubricas; seleciona identidade por colaborador/versão/tipo/data; lê detalhe para hora paga; lê furo e relógio da lavra. Leitores específicos de `versao_dna` e `evento_origem` fora de reversões [nao verificado]. (`app/ponto/services/dia_pago.py:559-563`, `:855-856`, `:897-898`, `:588-589`, `:649`)

## 11/11 10:16 · sessao 54a8712f0a4e · RAIZ-DA-DISPUTA · leitura · origem=Code

**Não pode abrir como raia disjunta:** F3/F4 alcança `materializacao.py`, já listado na E3, e `disputa_emissao.py`, listado na E4B e na c21; a regra de paralelismo é por **arquivo**, independentemente de conflito entre hunks. (`app/chamados/services/materializacao.py:1375`; `app/chamados/services/disputa_emissao.py:1964`; anexos `arquivos_e3.txt:3`, `arquivos_e4b.txt:3`, `arquivos_raia_c21_9bba8132b.txt:4`; `CLAUDE.md:309`.)

Nas referências aos anexos, linha 1 significa a primeira linha de conteúdo depois do marcador `== anexo… ==`.

**1. Arquivos e sítios da cura**

O quadro é um **mapa de intervenção**, usado nas interseções abaixo. O inventário definitivo de edições depende do patch, ainda **[nao verificado]**.

| Arquivo | Sítios que a cura precisa abranger |
|---|---|
| `app/chamados/models.py` | F3: C7 e porta de fechamento, autoria e trilha (`:1848`, `:1853`, `:1933`, `:1937`); fonte do carimbo e autor (`:3097`, `:3100`, `:3152`, `:3190`). F4: agrupamento na raiz e estado derivado; estado atual em `:3034`, identidade em `:3437`. |
| `app/chamados/services/materializacao.py` | F4: unidade e transação individuais (`:1158`, `:1220`), aplicação individual no lote (`:1375`), estado de aplicação (`:425`). F3: cauda que consulta C7 e fecha (`:1316`, `:1333`). |
| `app/chamados/services/validacao.py` | Valida uma pergunta (`:435`), materializa essa pergunta antes do lote (`:461`, `:463`), devolve validação recusada (`:464`) e chama o lote (`:469`). |
| `app/chamados/services/passivo_validado.py` | Reparador também chama diretamente a unidade individual (`:514`, `:516`); precisa ser abrangido pelo novo contrato, seja nessa chamada, seja dentro da unidade. |
| `app/chamados/signals.py` | O evento recebe a pergunta, mas encaminha somente `disputa_id`, depois do commit (`:228`, `:231`, `:237`, `:238`): sítio de propagação da autoria F3. |
| `app/chamados/services/becos.py` | Recorte exige pai terminal (`:29`, `:31`); entrada do evento fecha usando usuário sistema (`:78`), enquanto a porta aceita o usuário recebido (`:57`). |
| `app/chamados/reconciliador.py` | Caudas de carimbo/fechamento a integrar: `:405`, `:411`, `:412`; `:595`, `:611`, `:612`; `:1147`, `:1170`, `:1171`. |
| `app/chamados/services/disputa_emissao.py` | Fechamento do agrupador (`:1278`, `:1284`) e do resolvedor (`:1961`, `:1963`, `:1964`); resposta/reversão também entram no censo (`:1508`, `:1632`). |
| `app/chamados/services/explicador.py` | Calcula `pode_fechar` só pela ausência de fio vivo (`:137`, `:163`), enquanto C7 também exige ausência de resposta sem veredito (`app/chamados/models.py:1853`). |
| `app/chamados/relatorios_disputa.py` | Publica `PRONTA_FECHAR` pela ausência de vivas (`:18`, `:20`), outra projeção a alinhar ao C7 (`app/chamados/models.py:1853`). |
| `app/chamados/services/declaracao_texto.py` | Catálogo pergunta→posição do marco (`:42`, `:55`); limitação expressa de identidade entre duas pausas (`:49`, `:50`). |
| `app/chamados/services/fio.py` | Identidade/canônica e plano de retratação abrangidos pelo censo (`:1232`, `:1259`, `:1290`); não são a autoridade do par, que está na célula (`app/escala/models.py:1652`). |
| `app/chamados/services/relavra_via.py` | Escrita direta de `via_resolucao` a abranger na consolidação de escritor prevista no censo (`:125`, `:127`; `app/chamados/models.py:3101`). |
| `app/chamados/services/historico.py` | Testemunha da materialização, abrangida pela revisão de significado do carimbo (`:103`, `:105`; `app/chamados/services/materializacao.py:924`, `:1099`). |

A autoridade F4 **já existe como dado**: `CelulaDia.PARES_PAUSA_DNA` declara `hii/hfi` e `hii2/hfi2`; `marcos_presentes()` seleciona os marcos presentes. `pausas_do_dia()` lê esses pares e devolve vazio quando a célula existente não declara marcos. Isso fundamenta leitura da autoridade, não necessidade de editá-la. (`app/escala/models.py:1652`, `:1672`, `:1679`, `:1197`, `:1202`, `:1207`.)

Não foi demonstrada uma função existente que receba duas perguntas e certifique “mesma pausa”: **[nao verificado]**. As funções encontradas respondem outras perguntas: posição pelo motivo, instâncias do turno e marco do chamado. (`app/chamados/services/declaracao_texto.py:55`; `app/chamados/services/tarefa_do_dia.py:91`; `app/chamados/juizes.py:524`.)

A catraca também poderá precisar de edição **se as ocorrências registradas mudarem de linha**; isso depende do patch. (`app/core/tests/test_medida_turno_do_par.py:267`, `:268`, `:269`, `:270`, `:271`, `:272`; anexo `construtor22.q:12`.)

**2. Interseções exatas do mapa acima**

| Par | Arquivos em comum |
|---|---|
| **F3/F4 × E3** | `app/chamados/services/materializacao.py` — sítios `:1158`, `:1375`; anexo `arquivos_e3.txt:3`. |
| **F3/F4 × E4B** | `app/chamados/services/disputa_emissao.py` — sítios `:1284`, `:1964`; anexo `arquivos_e4b.txt:3`. |
| **F3/F4 × c21** | `app/chamados/services/declaracao_texto.py` — sítios `:49`, `:55`; anexo `arquivos_raia_c21_9bba8132b.txt:2`. **E** `app/chamados/services/disputa_emissao.py` — sítios `:1284`, `:1964`; mesmo anexo `:4`. |

Se houver atualização da catraca, acrescenta-se **aos três pares** `app/core/tests/test_medida_turno_do_par.py`. (`app/core/tests/test_medida_turno_do_par.py:268`; anexos `arquivos_e3.txt:15`, `arquivos_e4b.txt:22`, `arquivos_raia_c21_9bba8132b.txt:33`.)

Mudanças adicionais em `juizes.py` ampliariam a interseção com E3 e E4B; necessidade efetiva dessas edições: **[nao verificado]**. Os sítios sensíveis são a agregação de pausas e a morte pela validação. (`app/chamados/juizes.py:919`, `:1296`, `:1325`; anexos `arquivos_e3.txt:1`, `arquivos_e4b.txt:1`.)

Não transformei todos os leitores do censo em edições obrigatórias; interseção definitiva do futuro diff: **[nao verificado]**.

**3. F1/F2 são pré-requisito? E o merge posterior?**

**Pelo critério literal pedido — F3/F4 lê algo que F1/F2 muda — sim, há dependência pelo destino.** Os dois lados são:

| Contrato compartilhado | Lado F1/F2 | Lado F3/F4 |
|---|---|---|
| `PerguntaDisputa.chamado_destino` | F1 concentra a escrita atualmente situada em `models.py:2982`, `:3014` e `fio.py:925`. (Anexo `aval_raiz_f1_um_eleitor_1111.txt:1`.) | Materialização lê o destino (`app/chamados/services/materializacao.py:339`) e busca nele a hora fixa do intervalo (`:408`, `:409`); validação usa o destino na trava (`app/chamados/services/validacao.py:432`). |
| Eleição do destino | F1/c22 substituem as eleições divergentes do derivador, agrupador e declaração. (`app/chamados/services/disputa_emissao.py:238`, `:1197`; `app/chamados/services/declaracao_texto.py:377`; anexos `aval_raiz_f1_um_eleitor_1111.txt:1`, `construtor22.q:8`.) | O marco consumido vem de `chamado.contexto_json['marcos_faltantes']`, casado por motivo. (`app/chamados/juizes.py:524`, `:534`, `:536`; `app/chamados/services/materializacao.py:409`.) |
| Autofecho do agrupador | c22 altera a condição/veto por ata nesse caminho. (Anexo `construtor22.q:10`, `:11`; correspondente atual `app/chamados/services/disputa_emissao.py:1278`, `:1279`.) | F3 precisa preservar essa condição ao integrar autoria e reação da raiz; fechamento atual em `app/chamados/services/disputa_emissao.py:1284`. |

Isso **não equivale a depender de um campo novo para compilar**: destino, autoria do carimbo, autoria do fecho e C7 já existem. (`app/chamados/models.py:3385`, `:3397`, `:1933`, `:1848`.)

Um merge posterior da c22 **pode mudar o destino eleito e, por consequência, o marco/hora consumidos pela materialização**. O significado da FK continua “chamado que a pergunta resolve”; muda o valor eleito. (`app/chamados/models.py:3391`; `app/chamados/juizes.py:534`, `:536`; `app/chamados/services/materializacao.py:409`; anexo `construtor22.q:8`.) Conflito textual real entre os patches finais: **[nao verificado]**.

Há ainda um cuidado independente da c22: `instancia_marco` hoje deriva do motivo, e o nascimento recusa valor diferente. Reutilizá-lo silenciosamente como chave DNA ou índice da pausa quebraria esse contrato já na cópia atual. (`app/chamados/services/declaracao_texto.py:55`; `app/chamados/models.py:3437`, `:3441`, `:3446`.) O par deve ser lido da célula conforme o aval, sem converter `chamado_destino` em identificador de pausa. (Anexo `aval_f4_par_pela_celula_1111.txt:2`, `:6`; `app/escala/models.py:1652`; `app/chamados/models.py:3391`.)

**4. NAO PODE — motivo: interseção com E3 em `materializacao.py`, com E4B/c21 em `disputa_emissao.py`, suficiente pela trava por arquivo; há também leitura do destino alterado por F1/F2.** (`app/chamados/services/materializacao.py:1375`, `:339`; `app/chamados/services/disputa_emissao.py:1964`; anexos `arquivos_e3.txt:3`, `arquivos_e4b.txt:3`, `arquivos_raia_c21_9bba8132b.txt:4`; `CLAUDE.md:309`.)

## 11/11 10:14 · sessao d9f4e17b69b7 · - · - · origem=Ronald (tmux)

Mapa comprovado no HEAD, somente leitura.

1. Entrada e laço:
- Rota `chamados/urls.py:37` → `views_cobrar.validar_lote_tela` (`app/chamados/views_cobrar.py:372`); autorização `:376`, lê `refs` `:380`, chama lote `:381`, registra auditoria por POST `:383`, renderiza `:386`.
- Lote normaliza/deduplica refs em lista (`app/chamados/services/validacao.py:713`), faz um SELECT inicial com disputa (`:724`), percorre serialmente (`:727`), reclassifica cada pergunta (`:735`), chama ação (`:746`), faz `refresh_from_db` (`:752`).
- Classe A resolve delta e slot (`app/chamados/services/validacao.py:864`, `:872`) e consulta existência de batida apurável na janela ±60 min (`:875`). Não varre toda fila de validação nesse POST; recebe refs explícitas (`app/chamados/views_cobrar.py:380`; `app/chamados/services/validacao.py:724`).

2. Uma pergunta aprovada:
- `acao_validar_pergunta` → `validar_pergunta` (`app/chamados/services/acoes_disputa.py:19`).
- Novo SELECT da pergunta (`app/chamados/services/validacao.py:389`), juiz de implausibilidade (`:404`), foto da disputa (`:416`), atomic (`:425`), trava na ordem colaborador/disputas/chamados (`:431`).
- Carimbo `validada_em/por` (`:435` → `:134`) salvo em `:137`; registra `DecisaoPergunta` em `:139` → bulk INSERT (`app/chamados/models.py:3953`), e acorda células (`:3954`).
- Agenda recálculo da competência do colaborador (`app/chamados/services/validacao.py:446` → `app/ponto/services/fechamento.py:1809`, `:1810`, `:1824`).
- Materializa unidade desta pergunta (`app/chamados/services/validacao.py:463` → `app/chamados/services/materializacao.py:1158`), tenta devolver validação recusada (`app/chamados/services/validacao.py:464`).
- Depois disso, por CADA item do lote, varre todas irmãs respondidas/validadas/não materializadas da disputa (`app/chamados/services/validacao.py:469` → `app/chamados/services/materializacao.py:1369`, `:1375`, `:1376`); conta vivas/C7 e pode fechar disputa (`:1378` → `:1314`, `:1316`, `:1333`). Assim, um item não implica somente uma tentativa de materialização.
- Refresh próprio e auditoria de resultado (`app/chamados/services/validacao.py:472`, `:479`, `:494`, `:507`); refresh adicional do lote (`:752`).

3. Plantio de hora e multiplicação:
- Unidade verifica data do fato (`app/chamados/services/materializacao.py:1192`), passa por atomic e guarda “em voo” (`:1220`), ramo hora (`:1222`).
- Ramo hora calcula instante/tipo (`:846`), pode gravar `pendencia_admin/recusa_motivo` (`:850`, `:851`), busca existente (`:859`), testa paridade (`:875`), chama `registrar_batida` (`:927`).
- `registrar_batida`: SELECT mesmo tipo (`app/ponto/registro_batida.py:113`), SELECT mesma chave (`:120`), guarda competência (`:135`), janela oposta/exists (`:138`, `:145`), INSERT de Batida (`:157`), agenda fechamento por evento (`:160` → `:178`).
- Após inserir, liga `pergunta_origem` (`app/chamados/services/materializacao.py:946`, `:948`) pela porta `anotar`, que faz OUTRO `Batida.save` (`app/ponto/registro_batida.py:254`).
- Portanto, quando cria batida e liga origem, são dois saves de Batida, com as mesmas cascatas abaixo, exceto NOTIFY que só ocorre na criação (`app/chamados/services/materializacao.py:927`, `:946`; `app/ponto/registro_batida.py:157`, `:254`; `app/ponto/signals.py:42`).
- Reconciliam-se destino e clones (`app/chamados/services/materializacao.py:974` → `:762`, `:771`, `:777`, `:780`).
- Juiz confirma lastro (`:1233`), porta `carimbar` (`:1277`) aplica materializada/via e pergunta ao C6 (`app/chamados/models.py:3129`, `:3139`), relavra S2 (`:3150`), salva (`:3152`). Recusa pode gravar motivo e nota (`app/chamados/services/materializacao.py:1297` → `:1089`, `:1099`, `:1103`, `:1107`).

4. Receivers de cada save de Batida:
- Recompute `TurnoMaterializado` em quatro datas, d−2..d+1 (`app/ponto/signals.py:19`, `:26`, `:34`); reconstrói linhas e DELETE+bulk INSERT (`app/ponto/nucleo.py:47`, `:48`, `:58`, `:59`). HEAD também emite eventos de turnos fechados (`:64`, `:66`).
- Só criação: `publicar_batida` (`app/ponto/signals.py:39`, `:45`) → um SQL com dois `pg_notify`, condicionado a posto (`app/ponto/realtime.py:29`, `:42`).
- Cartório julga d−1 e d (`app/ponto/signals.py:108`, `:112`, `:131`, `:135` → `:93`), salvo reentrada `em_julgamento` (`:90`).
- Reconciliador do colaborador (`app/ponto/signals.py:206`, `:215`) → retrata perguntas absolvidas (`app/chamados/reconciliador.py:1432`), reconcilia órfãs (`:1433`), percorre chamados vivos/testa premissa/retrata (`:1441`, `:1453`, `:1456`, `:1458`, `:1464`).
- Fecha chamado de adesão com batida (`app/ponto/signals.py:234`).

**Custo estrutural para novo plantio+vínculo:** dois recomputes, quatro chamadas de julgamento de célula antes das guardas, duas reconciliações do universo vivo do colaborador, duas verificações de adesão; dois recálculos mensais agendados, um pela validação e um pela criação da batida (`app/ponto/signals.py:26`, `:93`, `:112`, `:215`, `:234`; `app/ponto/registro_batida.py:160`; `app/chamados/services/validacao.py:446`). Número total de queries e tempo em produção: [nao verificado]. Os comentários com “122 ms”, “20 queries”, etc. são relatos históricos do código, não medição do lote fornecido (`app/chamados/services/validacao.py:439`; `app/ponto/signals.py:96`).

5. Receivers de PerguntaDisputa:
- Todo save passa por atomic do modelo (`app/chamados/models.py:3413`, `:3422`).
- Beco: agenda `_fechar` em `on_commit` (`app/chamados/signals.py:227`, `:237`, `:238`); callback consulta disputa+perguntas (`app/chamados/services/becos.py:72`), classifica percorrendo perguntas (`:29`), pode resolver perguntas por chamados e fechar disputa (`:54`, `:57`).
- HEAD, alteração de campo relevante: outbox síncrono (`app/chamados/signals.py:250`, `:255`, `:257`); SELECT linhas de evento (`app/chamados/models.py:3228`), SELECT célula por FK ou par (`:3242`, `:3244`), emite evento (`:3248`). Esses caminhos HEAD são posteriores a 03/11 pelos próprios comentários de 06/11; não atribuir a eles o início do salto (`app/chamados/signals.py:242`).
- Lavra de `veredito_lavrado/julgado_em/janela_expira_em` também dispara callback de beco; filtro do outbox exclui esses campos (`app/chamados/veredito_lavrado.py:275`; `app/chamados/signals.py:255`; `app/chamados/models.py:3220`).

6. Fechamento de Chamado e novo F1:
- `reconciliar` chama decisão, premissa e gate de pergunta viva (`app/chamados/models.py:651`, `:682`, `:685`).
- F1 busca todas não validadas ancoradas no chamado, sem filtro de competência no SELECT (`:344`), julga entrega/C6 (`:364`, `:368`), busca C8 por pergunta viva (`:390`), pode reapontar (`:437`) ou registrar recusa (`:425`).
- `_gravar_estado` repete julgamento na lista guardada (`:221`, `:224`), grava estado (`:296`), relavra perguntas destino não validadas na travessia (`:303`, `:309`).
- Pre-save consulta status anterior (`app/chamados/signals.py:178`, `:184`).
- Post-save urgência (`:53`, `:72` → `app/chamados/models.py:636`); S143 resolver perguntas (`app/chamados/signals.py:90`, `:115`); cartório se status mudou (`:187`, `:192`, `:216`); silêncio isento condicional (`:282`, `:293`, `:305`); HEAD evento filho morto condicional (`:349`, `:357`, `:368`).
- F1 portanto acrescenta trabalho por chamado afetado e por suas perguntas ancoradas, não simplesmente por POST (`app/chamados/models.py:344`, `:360`, `:390`).

7. Fechamento de Disputa:
- C7 lê respostas sem veredito e fio vivo (`app/chamados/models.py:1853`); `fechar` verifica novamente (`:1886`, `:1895`), trava ordem (`:1910`), grava `fechada_em/por`, `resolucao`, `motivo_fechamento` (`:1932`–`:1936`).
- S88 busca pai e reconcilia, depois nota (`app/chamados/signals.py:18`, `:32`, `:42`, `:45`).
- Retoma regularizações confirmadas em on_commit (`:120`, `:137`, `:144`) → SELECT confirmadas não materializadas do colaborador e materialização de cada (`app/chamados/services/regularizacao_ext.py:218`, `:221`, `:223`, `:224`).
- S144 percorre perguntas ainda não materializadas e carimba as aprovadas (`app/chamados/signals.py:147`, `:163` → `app/chamados/services/materializacao.py:1418`, `:1427`, `:1431`).

Callbacks `on_commit` aqui não são evidência de job: a porta apenas registra funções com `transaction.on_commit`, e estas chamam diretamente recálculo/fecho/materialização (`app/ponto/services/fechamento.py:1807`, `:1810`, `:1824`; `app/chamados/signals.py:132`, `:137`, `:144`, `:233`, `:237`, `:238`). Tempo real consumido por cada uma no POST histórico: [nao verificado].

## 11/11 10:14 · sessao 2f4f18edb999 · - · - · origem=Ronald (tmux)

Q3 — Há leitura de campo compartilhado, mas nenhuma dependência demonstrada de interface nova da F1/F2. A FK `chamado_destino` já existe e significa “chamado que esta pergunta resolve”; a F1/c22 altera quem a escreve/elege. A materialização que a F4 precisa curar já lê esse destino para buscar o marco. (`app/chamados/models.py:3385`, `:3391`; `app/chamados/services/materializacao.py:339`, `:408`, `:409`.)

| Vínculo | Lado F1/F2 na cópia | Lado F3/F4 na cópia |
|---|---|---|
| Destino gravado | Reuso escreve `chamado_destino`; nascimento o recebe; reapontamento o escreve. (`app/chamados/models.py:2982`, `:3014`; `app/chamados/services/fio.py:925`.) | Materializador lê destino e busca o marco nele; validação também o usa na trava. (`app/chamados/services/materializacao.py:339`, `:794`, `:408`, `:409`; `app/chamados/services/validacao.py:432`.) |
| Eleição do destino | Derivador exige candidato único; agrupador escolhe primeiro do lote; declaração pede chamado do ato. (`app/chamados/services/disputa_emissao.py:238`, `:1197`; `app/chamados/services/declaracao_texto.py:377`.) | `entrada_de_marco` lê `destino.contexto_json['marcos_faltantes']`, portanto trocar eleição pode trocar o marco fonte consumido pelo materializador. (`app/chamados/juizes.py:524`, `:534`, `:536`, `:537`; `app/chamados/services/materializacao.py:409`.) |
| Identidade do par | A eleição do chamado resolve um destino, não expressa o índice de uma pausa. A identidade atual agrega duas pausas na mesma posição. (`app/chamados/models.py:3391`; `app/chamados/services/declaracao_texto.py:49`, `:50`.) | A célula já declara os pares `hii/hfi`, `hii2/hfi2`; o leitor `pausas_do_dia` retorna somente pares completos e retorna vazio quando a célula não declara marcos. (`app/escala/models.py:1652`, `:1197`, `:1199`, `:1201`, `:1202`, `:1207`.) |
| Autoria F3 | O destino não é a autoria do carimbo ou do fecho. São campos distintos já existentes. (`app/chamados/models.py:3385`, `:3397`, `:1766`.) | Carimbo grava `materializada_por_id`; fecho recebe usuário e grava `fechada_por`. (`app/chamados/models.py:3100`, `:3125`, `:1933`, `:1936`.) |
| C7 e autofecho | O agrupador já consulta C7 e distingue abstenção por ata; esse é o ponto que o brief c22 manda modificar/restaurar — aplicação futura do brief: [nao verificado]. (`app/chamados/services/disputa_emissao.py:1278`, `:1279`, `:1282`.) | O materializador consulta C7 e fecha; outras caudas fazem o mesmo. (`app/chamados/services/materializacao.py:1316`, `:1333`; `app/chamados/services/becos.py:56`, `:57`; `app/chamados/reconciliador.py:411`, `:611`, `:1170`.) |

Pelo critério literal do usuário — “F3/F4 lê algo que F1/F2 muda” — **sim, existe acoplamento pelo destino**: escritor/eleitores de um lado, leitor do marco e plantio do outro. Isso não demonstra necessidade de aguardar um campo ou função ainda inexistente para começar a implementar a cura. (`app/chamados/models.py:2982`, `:3014`; `app/chamados/services/fio.py:925`; `app/chamados/services/disputa_emissao.py:238`, `:1197`; `app/chamados/services/materializacao.py:339`, `:409`.)

A identificação do par deve consumir os pares da célula; usar `chamado_destino` como identificador do par criaria um segundo significado para uma FK cujo contrato é outro. Não há evidência de que os avais exijam esse reaproveitamento; implementação futura: [nao verificado]. (`app/escala/models.py:1652`; `app/chamados/models.py:3391`.)

Um merge posterior da c22 pode mudar o valor escolhido para destino e, por consequência, o marco consumido, preservando o significado da FK. Conflito textual real e resultado semântico dos patches finais: [nao verificado]. (`app/chamados/services/disputa_emissao.py:238`, `:1197`; `app/chamados/juizes.py:534`, `:536`; `app/chamados/models.py:3391`.)

A trava de paralelismo local é por **arquivo em comum**, suficiente mesmo sem dependência de interface nova; não exige provar conflito de hunks. (`CLAUDE.md:309`, `:310`, `:311`.)

## 11/11 10:14 · sessao e7ab94268b20 · - · - · origem=Ronald (tmux)

O par já existe como **dado da célula**: `CelulaDia.PARES_PAUSA_DNA` declara `(hii,hfi)` e `(hii2,hfi2)`; `marcos_presentes()` fornece as chaves efetivamente presentes e sua ordem. Não exige mudar essa autoridade para F4 lê-la. (`app/escala/models.py:1652`, `app/escala/models.py:1659`, `app/escala/models.py:1672`, `app/escala/models.py:1679`.)

`EscalaColaborador.pausas_do_dia()` fornece todos os pares declarados no DNA; célula existente sem marcos devolve `()`. `intervalo_do_dia()` é só o leitor da primeira pausa. O fallback para template ocorre quando não há célula; logo, a F4 literal “pela célula” precisa considerar essa diferença ao escolher como consumir a autoridade. (`app/escala/models.py:1163`, `app/escala/models.py:1166`, `app/escala/models.py:1197`, `app/escala/models.py:1202`, `app/escala/models.py:1208`.)

A enumeração AST em memória encontrou consumidores desses pares, mas não identificou função que recebe duas perguntas e responde “mesma pausa”. Completude contra lógica construída/reflexão e execução do enumerador como evidência `arquivo:linha`: [nao verificado]. Exemplos conferidos de funções existentes que **não** respondem isso:

- `fato_do_marco_no_espelho(p)` agrega **todas** as pausas do dia com AND para um motivo; não distingue duas perguntas de pausas diferentes. (`app/chamados/juizes.py:892`, `app/chamados/juizes.py:911`, `app/chamados/juizes.py:919`, `app/chamados/juizes.py:933`.)
- `_resposta_concorda_com_a_luz(p)` devolve `None` quando duas pausas compartilham o motivo. (`app/chamados/juizes.py:1276`, `app/chamados/juizes.py:1296`, `app/chamados/juizes.py:1298`.)
- `instancias_do_turno()`/`_posicoes()` já separam a segunda repetição como `int_ida:2`, mas são projeções por posição, sem casamento entre duas perguntas. (`app/chamados/services/tarefa_do_dia.py:57`, `app/chamados/services/tarefa_do_dia.py:65`, `app/chamados/services/tarefa_do_dia.py:67`, `app/chamados/services/tarefa_do_dia.py:91`.)

O gargalo de identidade merece constar no brief: o catálogo de pergunta só deriva `int_ida/int_volta`, e comenta expressamente que os emissores não carregam índice da pausa. O nascimento veta `instancia_marco` diferente da derivação do motivo. Portanto, usar `instancia_marco` como “hii/hfi” ou “int_ida:2” sem alterar o contrato introduziria incompatibilidade **já com a cópia atual**, independentemente de c22. (`app/chamados/services/declaracao_texto.py:45`, `app/chamados/services/declaracao_texto.py:49`, `app/chamados/services/declaracao_texto.py:55`, `app/chamados/models.py:3437`, `app/chamados/models.py:3441`, `app/chamados/models.py:3446`.)

Sobre arquivos da cura efetiva:

| Arquivo / sítio | Motivo concreto |
|---|---|
| `app/chamados/models.py:3034` | Estado da pergunta hoje diz VALIDADA antes de consultar C6; candidato para estado derivado de espera. Agrupamento na DS exigido pelo aval: implementação futura [nao verificado]. |
| `app/chamados/services/materializacao.py:1158`, `:1220`, `:1375` | Unidade e atomic são por pergunta; o lote itera individualmente. O par num ato precisa atuar nessa fronteira. |
| `app/chamados/services/validacao.py:435`, `:461`, `:463`, `:469` | Valida uma e materializa essa uma antes do lote; precisa respeitar a espera/unidade nova. |
| `app/chamados/services/passivo_validado.py:514`, `:516` | Reparador chama a unidade individual diretamente; precisa migrar ou estar coberto por mudança na própria unidade. |
| `app/chamados/services/declaracao_texto.py:49`, `app/chamados/models.py:3441` | Só são mudanças obrigatórias se a implementação ampliar a identidade para distinguir duas pausas; escolha desse desenho [nao verificado]. |
| `app/chamados/juizes.py:919`, `:1296` | Precisam entrar na análise de impacto se a pergunta passar a identificar uma pausa específica; suas leituras atuais agregam/abstêm diante de duas pausas. Mudança efetiva necessária para a implementação escolhida [nao verificado]. |

Não é correto pôr `escala/models.py`, `escala/servico_jornada.py`, `ponto/turnos.py` ou `ponto/services/cartorio.py` no diff só porque são autoridades/chamadores. O dado do par já está em `escala/models.py:1652`; o materializador já consome autoridades de turno e plantio. Necessidade de alteração nesses outros arquivos: [nao verificado]. (`app/chamados/services/materializacao.py:421`, `app/chamados/services/materializacao.py:1222`, `app/chamados/services/passivo_validado.py:506`.)

Há dependência **de leitura do valor existente de destino** no materializador: lê `p.chamado_destino`, usa-o para tipo e para substituir a hora do intervalo pelos `marcos_faltantes`. Isso não demonstra pré-requisito da função nova de F1 ou do eleitor de c22; demonstra que mudanças na eleição do destino podem mudar o marco consumido pelo plantio. (`app/chamados/services/materializacao.py:339`, `app/chamados/services/materializacao.py:386`, `app/chamados/services/materializacao.py:408`, `app/chamados/services/materializacao.py:409`; autoridade do casamento por motivo: `app/chamados/juizes.py:524`, `app/chamados/juizes.py:534`, `app/chamados/juizes.py:536`.)

F4 não deve usar `chamado_destino` como identidade do par: o par está nas chaves do DNA, enquanto destino é FK de chamado e os marcos consumidos por ele vêm de contexto do chamado. Essa separação é inferência de desenho sustentada pelos sítios; patch futuro: [nao verificado]. (`app/escala/models.py:1652`, `app/chamados/models.py:3385`, `app/chamados/juizes.py:534`.)

## 11/11 10:13 · sessao 6563a0ff60ea · - · - · origem=Ronald (tmux)

A cura F3 tem estes sítios demonstráveis; o diff final e a escolha entre centralizar o disparo na porta ou no evento são [nao verificado].

| Arquivo | Sítios relevantes para F3 |
|---|---|
| `app/chamados/models.py` | C7 completo em `:1848`, `:1853`; porta de fecho recebe usuário em `:1855` e persiste autor/motivo/resolução com evento em `:1932`, `:1933`, `:1934`, `:1935`, `:1936`, `:1937`, `:1940`; escritor do carimbo e autor em `:3097`, `:3099`, `:3100`; portas persistem em `:3152`, `:3190`. |
| `app/chamados/signals.py` | Evento da pergunta recebe a instância em `:228`, mas passa somente `disputa_id` ao consumidor em `:231`, `:237`; dispara depois do commit em `:238`. |
| `app/chamados/services/becos.py` | Recorte exige pai terminal em `:29`, `:31`; consulta C7 em `:33`; fecha com usuário recebido em `:57`; entrada do evento substitui autoria por usuário sistema em `:78`. |
| `app/chamados/services/materializacao.py` | Cauda consulta C7 em `:1316` e fecha com `request_user` em `:1333`; lote chama essa cauda em `:1378`. |
| `app/chamados/reconciliador.py` | Caudas existentes consultam C7/fecham em `:411`, `:412`, `:611`, `:612`, `:1170`, `:1171`; carimbam antes em `:405`, `:595`, `:1147`. |
| `app/chamados/services/disputa_emissao.py` | Agrupador fecha em `:1284`; resolvedor fecha em `:1963`, `:1964`, com guarda de pergunta em voo em `:1961`. |
| `app/chamados/services/explicador.py` | Deriva `pode_fechar` apenas de `fio_vivo` em `:136`, `:137`, publica em `:163`; diverge do C7 completo em `app/chamados/models.py:1853`. |
| `app/chamados/relatorios_disputa.py` | Publica `PRONTA_FECHAR` pela ausência de vivas em `:18`, `:19`, `:20`; não consulta `respostas_sem_veredito`, exigido pelo C7 em `app/chamados/models.py:1853`. |

O autor do último ato de carimbo pode ser preservado pelo argumento da porta (`app/chamados/models.py:3125`, `:3185`) e já tem campo próprio (`app/chamados/models.py:3100`, `:3397`). Eleger o autor por “maior timestamp encontrado” não está demonstrado como implementação correta: `manter_carimbo` preserva instante e autor anteriores ao trocar a via (`app/chamados/models.py:3182`, `:3183`).

F1 não cria o campo consumido por F3: `chamado_destino` já existe e significa chamado resolvido pela pergunta (`app/chamados/models.py:3385`, `:3391`); autoria de carimbo é outro campo (`app/chamados/models.py:3397`). A cura F3 lê destino em travas/recorte já existentes (`app/chamados/models.py:1911`; `app/chamados/services/becos.py:30`); dependência obrigatória da nova função proposta por F1 permanece [nao verificado].

Para interseção por arquivo do núcleo F3 acima: E3 contém `app/chamados/services/materializacao.py`; E4B contém `app/chamados/services/disputa_emissao.py`; c21 contém `app/chamados/services/disputa_emissao.py` — pertencimento às listas fornecidas: [nao verificado por arquivo:linha, dado textual recebido]. Os sítios F3 desses dois arquivos são `app/chamados/services/materializacao.py:1316`, `:1333` e `app/chamados/services/disputa_emissao.py:1284`, `:1964`.

A catraca de endereços também registra materialização/explicador/emissão em `app/core/tests/test_medida_turno_do_par.py:267`, `:268`, `:269`, `:270`; necessidade de modificá-la depende do deslocamento efetivo dessas ocorrências, ainda [nao verificado].

## 11/11 10:12 · sessao 88613664e51b · UI-PAINEL-MOSTRA-O-APP · censo · origem=Code

Censo estático da cópia, somente leitura. Identidade do HEAD/hash informado: **[nao verificado]** — a cópia não contém metadados Git. Execução dos testes, situação em produção e smoke: **[nao verificado]**. [nao verificado]

1. **Regra do rótulo: duas cópias completas e uma variante no template.**

   | Local | Leitura/escrita |
   |---|---|
   | `app/api/views_core.py:307` | Conta chamados não lidos no recorte `VIVOS`. |
   | `app/api/views_core.py:314` | Conta perguntas de `consulta_do_app()['pendentes']`. |
   | `app/api/views_core.py:315` | Escreve a regra completa: pendentes → `{count, cor: '#f59e0b', rotulo: 'Responder DP'}`; senão não lidos → vermelho `'#ef4444'`, `"Chamados"`; senão `None`. |
   | `app/api/views_core.py:359` | Expõe os dois contadores e `chamados_badge` na resposta. |
   | `app/api/views.py:318` | Repete a consulta de não lidos. |
   | `app/api/views.py:325` | Repete contagem e regra completa. |
   | `app/api/views.py:370` | Expõe os mesmos três campos. |
   | `app/core/views.py:253` | Lê pendentes para o dashboard web. |
   | `app/templates/core/dashboard_colaborador_v2.html:78` | Variante: pendentes mudam destino do link, badge amarelo/count e texto `"Responder DP"`; sem pendentes escreve `"Chamados"`, sem ramo vermelho de não lidos. |
   | `app/templates/core/dashboard_colaborador.html:90` | Template anterior lê `chamados_nao_lidos` e mostra “novo(s)”; não contém a regra completa. |

   Portanto, **duas cópias da decisão ternária da API**; contando qualquer escrita manual da escolha `"Responder DP"/"Chamados"`, há também a variante do dashboard. (`app/api/views_core.py:315`; `app/api/views.py:326`; `app/templates/core/dashboard_colaborador_v2.html:80`)

   Os dois kernels têm rotas próprias para `/me/`; nenhum deles pode ser tratado como cópia morta. (`app/api/urls.py:64`; `app/api/urls_core.py:5`; `app/api/urls_core.py:13`)

   Leitores de teste da regra: contrato do badge da API, matriz dos dois kernels e testes do dashboard; os mutantes também dependem dos trechos literais atuais nos dois kernels. (`app/api/tests/test_api_me_badge.py:54`; `app/chamados/tests/test_e3_app_orm.py:28`; `app/core/tests/test_dashboard_badge.py:52`; `app/chamados/tests/e3_mutantes.py:23`)

   A busca textual incluiu templates e `static/js`; outras implementações JavaScript dessa regra não foram encontradas. Ausência absoluta de implementação equivalente com outros nomes: **[nao verificado]**.

2. **Fontes, leitores e escritores dos contadores.**

   **Pendentes de `consulta_do_app`:** são listas derivadas, não um contador persistido. A função lê perguntas vivas e separa como pendente cada pergunta com `respondida_em is None` **e** disputa com `fechada_em is None`; as demais vivas ficam no histórico, em ordem por ID. (`app/chamados/services/pergunta_no_app.py:45`; `app/chamados/services/pergunta_no_app.py:61`)

   Há **sete sítios de consumo em produção**:

   | Consumidor | Referência |
   |---|---|
   | Badge do kernel core | `app/api/views_core.py:314` |
   | Badge do kernel UI | `app/api/views.py:325` |
   | GET de chamados nativos | `app/api/views.py:1005` |
   | GET de perguntas da disputa | `app/api/views.py:2543` |
   | Dashboard web | `app/core/views.py:253` |
   | Aviso de questionário ainda pendente | `app/chamados/services/aviso_pendencia.py:9` |
   | Cobrança | `app/chamados/services/cobranca.py:48` |

   Os dois últimos alimentam, respectivamente, `aviso_texto_livre` e as operações de cobrança; continuam usando a mesma lista. (`app/chamados/services/aviso_pendencia.py:12`; `app/chamados/services/cobranca.py:82`; `app/chamados/services/cobranca.py:163`)

   Escritores diretos dos campos que separam pendentes/histórico:

   | Ato | Escritor |
   |---|---|
   | Nascimento da pergunta | Campo nullable; `_nascer` constrói com `**campos` e insere. (`app/chamados/models.py:2448`; `app/chamados/models.py:3014`) |
   | Resposta do colaborador | `responder_pergunta_disputa_core` atribui e salva `respondida_em`. (`app/chamados/services/disputa_emissao.py:1468`; `app/chamados/services/disputa_emissao.py:1508`) |
   | Resposta automática | `responder_automaticamente`. (`app/chamados/services/disputa_emissao.py:1653`) |
   | Recuperação pela trilha | `carimbar_resposta_pela_trilha`. (`app/chamados/services/disputa_emissao.py:1674`) |
   | Carimbo com resposta textual | `PerguntaDisputa.carimbar`. (`app/chamados/models.py:3130`; `app/chamados/models.py:3152`) |
   | Reabertura para correção | Monta `respondida_em=None` e atualiza diretamente ou pelo descarimbo em lote. (`app/chamados/services/disputa_emissao.py:1593`; `app/chamados/services/disputa_emissao.py:1629`) |
   | Ressuscitação | Delega `respondida_em=None` a `descarimbar_em_lote`. (`app/chamados/models.py:1566`; `app/chamados/models.py:3264`) |
   | Nascimento da disputa aberta | Campo nullable e criações do emissor. (`app/chamados/models.py:1757`; `app/chamados/services/disputa_emissao.py:587`; `app/chamados/services/disputa_emissao.py:1123`; `app/chamados/services/disputa_emissao.py:1358`) |
   | Fechamento da disputa | `DisputaSupervisao.fechar` carimba `fechada_em`. (`app/chamados/models.py:1932`) |
   | Reabertura da disputa | `reabrir_admin` e `reabrir_sistema` zeram `fechada_em`. (`app/chamados/models.py:2149`; `app/chamados/models.py:2173`) |

   As atribuições em simulação de ressuscitação e restauração após recusa de carimbo são apenas em memória. (`app/chamados/models.py:1508`; `app/chamados/models.py:3163`)

   **Limite do censo:** a vitalidade anterior à partição também depende de C6, célula/ata/veredito, validação, materialização, resposta e tranca da competência. O fechamento transitivo de **todos** os escritores desses insumos é **[nao verificado]**; não seria correto reduzir a fonte somente aos dois campos acima. (`app/chamados/models.py:3326`; `app/chamados/juizes.py:1324`; `app/core/portas.py:393`)

   **Não lido:** o campo efetivo é `FollowupChamado.lido_colaborador_em`, nullable; não é `lido_dp_em`. (`app/chamados/models.py:1672`; `app/chamados/models.py:1673`)

   - **Leitores:** os dois badges; serialização nativa de `novo`/`nao_lidos`; predicado web `_nao_lido`; duas posições no card de atendimento. (`app/api/views_core.py:311`; `app/api/views.py:322`; `app/api/views.py:1128`; `app/chamados/views.py:603`; `app/templates/chamados/partials/_card_atendimento.html:51`; `app/templates/chamados/partials/_card_atendimento.html:59`)
   - **Escritor do carimbo:** `nota.marcar_lidas`, via mapa dinâmico `LADOS`, atualizando somente nulos. (`app/chamados/services/nota.py:59`; `app/chamados/services/nota.py:126`)
   - **Três chamadores para o lado colaborador:** GET nativo de chamados, endpoint web de um chamado e endpoint web em lote. (`app/api/views.py:1050`; `app/chamados/views.py:1135`; `app/chamados/views.py:1150`)
   - **Nascimento não lido:** `nota.anexar_ocorrencia` cria a nota sem preencher o carimbo; a porta está registrada em `core/portas`. (`app/chamados/services/nota.py:88`; `app/chamados/services/nota.py:103`; `app/core/portas.py:241`)
   - **Remoção da fonte:** apagar notas em ensaio ou excluir o chamado por cascade elimina followups. (`app/chamados/services/nota.py:136`; `app/chamados/models.py:1668`)

   O vermelho conta **chamados distintos com pelo menos um followup DP não lido**, não quantidade de followups. A serialização nativa tem outro contador, por followup. (`app/api/views.py:318`; `app/api/views.py:1128`; `app/chamados/tests/test_e3_app_orm.py:62`)

   **`status_local` no recorte da contagem:** `VIVOS = ('aberto', 'em_analise', 'registrado')`; `registrado` entra nesse badge. `COBRANCA` é outro conjunto. A própria `consulta_do_app` não contém esse filtro de chamados. (`app/chamados/catalogo/motor.py:24`; `app/chamados/catalogo/motor.py:29`; `app/api/views_core.py:309`; `app/api/views.py:320`; `app/chamados/services/pergunta_no_app.py:69`)

   Escritores de status que podem alterar entrada/saída desse recorte:

   - Default de nascimento: `aberto`; nascimentos pelas portas `abrir` e `criar`. (`app/chamados/models.py:52`; `app/chamados/models.py:938`; `app/chamados/models.py:971`; `app/chamados/models.py:1095`)
   - Transição persistente: `_gravar_estado`, atribuição seguida de `save(update_fields=campos)`. (`app/chamados/models.py:228`; `app/chamados/models.py:296`)
   - Gates que delegam a essa escrita: `_aplicar`, `reconciliar`, `cancelar`, `analisar`, `reabrir`, `encerrar`, `encerrar_por_competencia_trancada`. (`app/chamados/models.py:450`; `app/chamados/models.py:654`; `app/chamados/models.py:788`; `app/chamados/models.py:806`; `app/chamados/models.py:1442`; `app/chamados/models.py:1619`; `app/chamados/models.py:1639`)
   - `_vivas_entre` troca/restaura status somente em memória. (`app/chamados/models.py:356`; `app/chamados/models.py:371`)
   - Migrações de dados alteram `aberto↔registrado` e `cancelado↔superado`, fora do runtime normal. (`app/chamados/migrations/0038_a3_registrado_backfill.py:10`; `app/chamados/migrations/0038_a3_registrado_backfill.py:15`; `app/chamados/migrations/0039_a3_via_backfill.py:24`; `app/chamados/migrations/0039_a3_via_backfill.py:33`)

   Existem selos de escritores de status e da porta de notas; sua aprovação nesta cópia é **[nao verificado]**. (`app/chamados/tests/test_selo_status_so_pelas_portas.py:37`; `app/chamados/tests/test_selo_status_so_pelas_portas.py:449`; `app/chamados/tests/test_porta_nota_do_fio.py:44`; `app/chamados/tests/test_porta_nota_do_fio.py:207`)

3. **View do painel, contexto atual e contador de Mensagens.**

   A view é `painel_colaborador(request, cid)`, com login e permissão `ver_cadastro`; renderiza diretamente o partial indicado. (`app/colaboradores/views.py:914`; `app/colaboradores/views.py:917`; `app/colaboradores/views.py:988`; `app/colaboradores/urls.py:48`)

   Para mensagens, ela passa apenas `chamados`: últimos cinco chamados do colaborador, ordenados por criação, com followups prefetched. Apesar do comentário “abertos”, **não há filtro de status**. Não passa perguntas, histórico, rótulo nem contagem de pendentes. (`app/colaboradores/views.py:936`; `app/colaboradores/views.py:937`; `app/colaboradores/views.py:988`)

   Os demais dados explícitos são colaborador, escala, bloco/portas/legenda do retrato, batidas, mês e CSRF. (`app/colaboradores/views.py:988`)

   O contador do botão é hoje **`{{ chamados|length }}`**, portanto entre zero e cinco chamados carregados; não mede perguntas nem mensagens não lidas. (`app/templates/colaboradores/partials/painel_colaborador.html:39`; `app/colaboradores/views.py:937`)

   A aba contém formulário HTMX e `msgs_lista.html`; a lista mostra os chamados e até cinco followups por chamado, truncados. O envio atualiza somente `#msgs-lista`, não o botão da aba. (`app/templates/colaboradores/partials/painel_colaborador.html:188`; `app/templates/colaboradores/partials/painel_colaborador.html:198`; `app/templates/colaboradores/partials/msgs_lista.html:4`; `app/templates/colaboradores/partials/msgs_lista.html:7`; `app/chamados/views.py:997`)

4. **As chamadas em `api/views.py:1004` e `:2542`.**

   - **`:1004`, `api_chamados_v2`:** consulta pendentes e histórico, agrupa perguntas por chamado, usa destino vivo ou chamado pai, serializa a conversa e **marca followups do DP como lidos**. Não calcula o badge ternário. (`app/api/views.py:973`; `app/api/views.py:1004`; `app/api/views.py:1040`; `app/api/views.py:1044`; `app/api/views.py:1050`; `app/api/views.py:1155`)
   - **`:2542`, `api_disputa_perguntas`:** serve pendentes, histórico, subconjunto `respondidas`, `em_disputa` e IDs de ancoragem. Não calcula rótulo/cor/count do badge. (`app/api/views.py:2531`; `app/api/views.py:2542`; `app/api/views.py:2571`; `app/api/views.py:2573`)

   **Extrair somente a regra para uma função separada não exige mudar essas duas chamadas.** Preservar `consulta_do_app` mantém seus contratos atuais; há teste AST exigindo essa delegação. (`app/chamados/services/pergunta_no_app.py:61`; `app/chamados/tests/test_contract_pergunta_viva.py:68`)

   O selo deve comparar o painel com **`/me/`**; chamar antes o GET de chamados pode consumir o estado não lido e alterar o resultado seguinte. (`app/api/urls.py:64`; `app/api/views.py:318`; `app/api/views.py:1050`)

5. **Cascas e grafo de carregamento.**

   A premissa precisa de um ajuste: o GET do painel entra por **`fetch → innerHTML → htmx.process`**, e não por `hx-get`; HTMX passa a processar os controles do conteúdo injetado. (`app/templates/core/_drawer_generico.html:19`; `app/templates/core/_drawer_generico.html:26`; `app/templates/core/_drawer_generico.html:33`)

   | Caminho | Evidência |
   |---|---|
   | `base.html → core/_drawer_generico.html` | `app/templates/base.html:399` |
   | `base_app.html → core/_drawer_generico.html` | `app/templates/base_app.html:154` |
   | Lista: `colaboradores/lista.html → _lista_tabela.html → abrirPainelColab → abrirDrawer → /colaboradores/<cid>/painel/` | `app/templates/colaboradores/lista.html:1`; `app/templates/colaboradores/lista.html:57`; `app/templates/colaboradores/partials/_lista_tabela.html:16`; `app/templates/core/_drawer_generico.html:89`; `app/templates/core/_drawer_generico.html:104` |
   | Retrato: `folha/retrato_folha.html → _retrato_lista.html → mesma rota`, com mês/ano | `app/templates/folha/retrato_folha.html:1`; `app/templates/folha/retrato_folha.html:53`; `app/templates/folha/_retrato_lista.html:61` |
   | Retrato parcial → mesma lista | `app/templates/folha/_retrato_parcial.html:7` |
   | Raio-X → `_retrato_raiox.html` → mesma rota | `app/templates/folha/_retrato_barra.html:73`; `app/folha/views_retrato.py:510`; `app/templates/folha/_retrato_raiox.html:68` |
   | Painel → `_cabecalho_drawer.html`, condicional `_retrato_o_que_fazer.html`, `msgs_lista.html` | `app/templates/colaboradores/partials/painel_colaborador.html:8`; `app/templates/colaboradores/partials/painel_colaborador.html:26`; `app/templates/colaboradores/partials/painel_colaborador.html:198` |
   | Bloco do retrato → `_led.html`, `_retrato_legenda.html` | `app/templates/folha/_retrato_o_que_fazer.html:36`; `app/templates/folha/_retrato_o_que_fazer.html:71` |

   O partial não estende casca. Os hosts encontrados usam `base.html` fixo; ambas as cascas oferecem a infraestrutura do drawer. A variável global `base_tpl` não substitui esses `extends` fixos. (`app/templates/colaboradores/partials/painel_colaborador.html:1`; `app/templates/colaboradores/lista.html:1`; `app/templates/folha/retrato_folha.html:1`; `app/core/context_processors.py:205`; `app/templates/base_app.html:154`)

   Consequência para o selo: pedir o partial duas vezes não demonstra as duas cascas; é necessário renderizar a integração com cada uma. Já existe exemplo de smoke que verifica os templates efetivamente usados nas duas cascas. (`app/colaboradores/views.py:988`; `app/core/tests/test_smoke_chromium.py:151`; `app/core/tests/test_smoke_chromium.py:172`)

6. **Juiz existente e autoridade da leitura.**

   Não há entrada específica para o rótulo/count “o que o app mostra agora” no registro de chamados nem no lookup documental; há autoridades próximas para vitalidade, retenção, fio e tarefa do dia. (`app/core/juizes.py:272`; `app/core/juizes.py:278`; `app/core/juizes.py:285`; `app/core/juizes.py:287`; `app/core/juizes.py:291`; `app/docs/JUIZES.md:59`)

   **`consulta_do_app` já é a autoridade única da seleção pendentes/histórico apresentada nesses consumidores; ainda não é autoridade do badge inteiro.** Ela delega vitalidade ao C6, enquanto a decisão de badge continua nos dois kernels. (`app/chamados/services/pergunta_no_app.py:3`; `app/chamados/services/pergunta_no_app.py:45`; `app/chamados/services/pergunta_no_app.py:61`; `app/api/views_core.py:315`; `app/api/views.py:326`)

   O nome `estado_app` encontrado no situacional mede adesão/uso do app, não esse rótulo de mensagens/perguntas. (`app/colaboradores/services/situacional.py:191`)

7. **Decisões técnicas para o construtor, sem lei nova.**

   | Candidata | Pró e consequência |
   |---|---|
   | **Função de projeção ao lado de `consulta_do_app`, no mesmo serviço — recomendação** | Reúne lista, consulta de não lidos e decisão do badge, sem fazer o painel importar uma view HTTP. Preserva a consulta existente. (`app/chamados/services/pergunta_no_app.py:61`; `app/api/views_core.py:307`; `app/api/views.py:318`) |
   | Acrescentar tudo à própria `consulta_do_app` | Menos funções, mas muda o retorno e acrescenta consulta a todos os consumidores; há igualdade exata do dicionário e exigência de nove queries em cenário existente. (`app/chamados/tests/test_e3_v4_orm.py:85`; `app/chamados/tests/test_e3_v4_orm.py:91`; `app/chamados/tests/test_e3_v4_orm.py:98`) |
   | Novo módulo em `chamados/services/` | Dá nome próprio à projeção e preserva a seleção; acrescenta arquivo/indireção. Admissibilidade desse arquivo no escopo literal: **[nao verificado]**, pois o limite vem do anexo. |
   | Helper puro recebendo duas contagens | Centraliza prioridade/cor, mas sozinho deixa a produção da consulta de não lidos nos leitores; precisaria de um montador compartilhado para o painel. (`app/api/views_core.py:307`; `app/api/views_core.py:315`) |
   | Helper dentro de uma view da API | Extração local simples, mas cria dependência do painel em camada HTTP e escolhe um kernel como dono. (`app/api/views_core.py:273`; `app/api/views.py:284`; `app/colaboradores/views.py:915`) |

   **Retorno sugerido da projeção:** listas `pendentes`/`historico`, inteiros `questionarios_pendentes`/`chamados_nao_lidos` e `chamados_badge` no formato atual `{count, cor, rotulo} | None`. Assim os três leitores recebem dados prontos; a decisão não reaparece no template. É proposta técnica, apoiada nos contratos existentes. (`app/chamados/services/pergunta_no_app.py:75`; `app/api/views_core.py:314`; `app/api/views_core.py:359`)

   Para a lista visual, os campos já existem: texto `pergunta`, dia `data_evento`, marco `instancia_marco` e motivo. Usar esses dados evita reconstruir identidade a partir do texto. (`app/chamados/models.py:2421`; `app/chamados/models.py:2423`; `app/chamados/models.py:2434`)

   O contador solicitado da aba é uma soma de apresentação sobre o contador atual; não deve ser confundido com o count do badge, que aplica prioridade entre pendentes e não lidos. A soma está fixada no anexo **[nao verificado]**; as duas medidas atuais estão em `app/templates/colaboradores/partials/painel_colaborador.html:39` e `app/api/views_core.py:315`.

   Há quatro cuidados concretos para a construção:

   - Atualizar os dois kernels de forma equivalente: existe contrato de paridade AST. (`app/api/tests/test_contract_paridade_kernels.py:25`)
   - Preservar a mordida dos mutantes que hoje procuram expressões literais nos kernels; a extração muda seus alvos. (`app/chamados/tests/e3_mutantes.py:23`)
   - Acrescentar o selo painel × API e integração das duas cascas; a matriz atual compara endpoint, dashboard e kernels, não este painel. (`app/chamados/tests/test_e3_app_orm.py:28`; `app/colaboradores/tests/test_retrato_no_drawer.py:151`; `app/core/tests/test_smoke_chromium.py:151`)
   - Tratar explicitamente a variante do dashboard no relatório de escopo: ela é outra escrita manual do rótulo, embora não seja a terceira cópia da regra completa. (`app/templates/core/dashboard_colaborador_v2.html:78`)

   O golden atual contém perguntas sobre presença de pergunta no app por colaborador/dia; o caso novo de rótulo/count “agora” permanece requisito do anexo, **[nao verificado]** como implementação. (`eval/golden.py:35`; `eval/golden.py:184`; `eval/golden.py:187`)

8. **Perguntas de lei: N = 0.**

   O anexo fixa fonte, prioridade, apresentação, ausência de badge e proibições, além de declarar “Nenhuma lei nova”: **[nao verificado]** como registro em arquivo desta cópia. As escolhas restantes são técnicas; seleção, vitalidade e regra atual já têm fontes identificadas. (`app/chamados/services/pergunta_no_app.py:61`; `app/core/juize

[truncado em 20000 de 20039 caracteres]

## 11/11 10:10 · sessao 1a4869192285 · - · - · origem=Ronald (tmux)

Censo direto, somente leitura; testes não executados, estado verde atual [nao verificado]. [nao verificado]

**Consulta e leitores**

- `consulta_do_app` deriva listas novas, preserva ordem por ID e separa `pendentes` por `respondida_em is None` **e** `disputa.fechada_em is None`; demais perguntas vivas vão ao histórico. Fonte anterior: `perguntas_do_app → q_vivas + pergunta_viva(C6)`. `app/chamados/services/pergunta_no_app.py:45`, `app/chamados/services/pergunta_no_app.py:61`, `app/chamados/models.py:3326`, `app/chamados/juizes.py:839`.
- **7 sítios de consumo em produção**: dois badges (`app/api/views_core.py:314`, `app/api/views.py:325`), GET chamados (`app/api/views.py:1005`), GET perguntas (`app/api/views.py:2543`), dashboard (`app/core/views.py:253`), aviso de pendência (`app/chamados/services/aviso_pendencia.py:9`), cobrança (`app/chamados/services/cobranca.py:48`).
- A consulta não contém `status_local__in=VIVOS`; esse filtro pertence à contagem de **chamados com followup DP não lido**, nos badges. `VIVOS = aberto, em_analise, registrado`, diferente de `COBRANCA`. `app/api/views_core.py:309`, `app/api/views.py:320`, `app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:29`.

**Escritores diretos dos campos que separam pendentes/histórico**

| Campo/operação | Sítios |
|---|---|
| Resposta do colaborador | `responder_pergunta_disputa_core`: atribui e salva `respondida_em`. `app/chamados/services/disputa_emissao.py:1468`, `app/chamados/services/disputa_emissao.py:1508` |
| Resposta automática | `responder_automaticamente`. `app/chamados/services/disputa_emissao.py:1653` |
| Recuperação de carimbo pela trilha | `carimbar_resposta_pela_trilha`. `app/chamados/services/disputa_emissao.py:1674` |
| Carimbo com resposta já textual | `PerguntaDisputa.carimbar`, quando `resposta` solicitado e texto presente. `app/chamados/models.py:3130`, `app/chamados/models.py:3152` |
| Voltar a pendente | `reabrir_para_correcao`: monta `respondida_em=None`, executa `qs.update(**campos)` ou delega ao descarimbo em lote. `app/chamados/services/disputa_emissao.py:1593`, `app/chamados/services/disputa_emissao.py:1629` |
| Ressuscitar perguntas | `ChamadoColaborador.ressuscitar_perguntas` delega a `descarimbar_em_lote(...respondida_em=None)`; este faz update dinâmico. `app/chamados/models.py:1566`, `app/chamados/models.py:3264` |
| Nascimento da pergunta | `_nascer` recebe `**campos`, constrói e insere; inclui assinaturas recebidas. Campo nullable por definição. `app/chamados/models.py:2448`, `app/chamados/models.py:3014` |
| Fechar disputa | `DisputaSupervisao.fechar`: `fechada_em=now`, seguido de save. `app/chamados/models.py:1932` |
| Reabrir disputa | `reabrir_admin` e `reabrir_sistema`: `fechada_em=None`, seguido de save. `app/chamados/models.py:2149`, `app/chamados/models.py:2173` |
| Nascer disputa aberta | Default nullable; criações do emissor. `app/chamados/models.py:1757`, `app/chamados/services/disputa_emissao.py:587`, `app/chamados/services/disputa_emissao.py:1123`, `app/chamados/services/disputa_emissao.py:1358` |

Atribuições de `respondida_em` que **não persistem**: simulação em cópia (`_universo_da_ressuscitacao`) e restauração da memória quando carimbar recusa. `app/chamados/models.py:1508`, `app/chamados/models.py:3163`.

Vitalidade C6 também depende de `validada_em`, tranca da competência, `via_resolucao`, `materializada_em`, texto da resposta, célula/ata/veredito e testemunha do fato; o conjunto de escritores transitivos de todos esses insumos não foi encerrado neste recorte [nao verificado]. As dependências aparecem em `app/chamados/juizes.py:1324`. O registro existente reconhece oito módulos-porta de `PerguntaDisputa`, incluindo modelo, emissor, materialização, fio, reconciliador, lavra, relavra e validação. `app/core/portas.py:393`.

**Não lido do colaborador**

- Campo real: `FollowupChamado.lido_colaborador_em`, nullable. Criar nota sem carimbo faz nascer uma fonte não lida. `app/chamados/models.py:1673`, `app/chamados/services/nota.py:88`, `app/chamados/services/nota.py:103`.
- Escritor do carimbo: `nota.marcar_lidas`, usando mapa dinâmico `LADOS`; atualiza apenas registros ainda nulos. `app/chamados/services/nota.py:59`, `app/chamados/services/nota.py:126`.
- **3 chamadores com lado colaborador**: GET nativo marca **todos** followups DP daquele colaborador; endpoint web de um chamado; endpoint web em lote. `app/api/views.py:1050`, `app/chamados/views.py:1135`, `app/chamados/views.py:1150`.
- Chamador com lado DP modifica **outro campo**, `lido_dp_em`. `app/chamados/views.py:880`, `app/chamados/services/nota.py:59`.
- Leitores em produção: dois badges; serialização nativa `novo`/contador por followup; `_nao_lido` web/contador por chamado; duas posições no card de atendimento. `app/api/views_core.py:311`, `app/api/views.py:322`, `app/api/views.py:1128`, `app/chamados/views.py:603`, `app/templates/chamados/partials/_card_atendimento.html:51`, `app/templates/chamados/partials/_card_atendimento.html:59`.
- Outros atos que eliminam fonte: `nota.apagar_em_ensaio` apaga notas sob guarda de banco de ensaio; excluir o chamado faz cascade da FK. `app/chamados/services/nota.py:136`, `app/chamados/models.py:1668`.
- Registro da porta de criação/leitura/apagamento: `app/core/portas.py:239`.

**`status_local`: escritores que alteram entrada/saída de VIVOS**

- Campo/default `'aberto'`. `app/chamados/models.py:52`.
- **Escritor persistente de transição**: `_gravar_estado`, atribuição em `:228`, `save(update_fields=campos)` em `:296`. `app/chamados/models.py:228`, `app/chamados/models.py:296`.
- Gates locais que delegam ao núcleo: `_aplicar`, `reconciliar`, `cancelar`, `analisar`, `reabrir`, `encerrar`, `encerrar_por_competencia_trancada`. `app/chamados/models.py:450`, `app/chamados/models.py:654`, `app/chamados/models.py:788`, `app/chamados/models.py:806`, `app/chamados/models.py:1442`, `app/chamados/models.py:1619`, `app/chamados/models.py:1639`.
- **Nascimento idempotente**: `abrir` monta `_def['status_local']`, agrega defaults e passa ao `get_or_create`. `app/chamados/models.py:938`, `app/chamados/models.py:971`.
- **Nascimento direto**: `criar` passa `status_local=status` ao `objects.create`. A construção anterior `_nascente` é só memória. `app/chamados/models.py:1070`, `app/chamados/models.py:1095`.
- **Simulação sem persistência**: `_vivas_entre` aplica e restaura o status em memória. `app/chamados/models.py:356`, `app/chamados/models.py:371`.
- Registro existente da porta de ciclo: `app/core/portas.py:218`.
- Migrações de dados, fora do runtime: `aberto↔registrado` e `cancelado↔superado`. `app/chamados/migrations/0038_a3_registrado_backfill.py:10`, `app/chamados/migrations/0038_a3_registrado_backfill.py:15`, `app/chamados/migrations/0039_a3_via_backfill.py:24`, `app/chamados/migrations/0039_a3_via_backfill.py:33`.

**Contratos já presentes, sem executar**

- Selo AST de escritores de status permite precisamente `_gravar_estado`, `_vivas_entre`, `criar`, `abrir`; distingue memória e persistência, examina update/create/defaults/bulk/setattr/SQL e declara limites de resolução de tipos. `app/chamados/tests/test_selo_status_so_pelas_portas.py:10`, `app/chamados/tests/test_selo_status_so_pelas_portas.py:17`, `app/chamados/tests/test_selo_status_so_pelas_portas.py:37`, `app/chamados/tests/test_selo_status_so_pelas_portas.py:340`, `app/chamados/tests/test_selo_status_so_pelas_portas.py:449`.
- Selo da nota: nenhum escritor fora da porta e idempotência do marcar lidas. `app/chamados/tests/test_porta_nota_do_fio.py:44`, `app/chamados/tests/test_porta_nota_do_fio.py:207`.
- Matriz ORM compara endpoint de perguntas, dashboard e **ambos** kernels, incluindo dicionário completo do badge; prova que vermelho conta chamados e não followups. `app/chamados/tests/test_e3_app_orm.py:28`, `app/chamados/tests/test_e3_app_orm.py:62`.
- Contratos de leitores: aviso/cobrança/badges mesma pendente; lista nativa pendentes + histórico; frescor após tranca/toque/reabertura. `app/chamados/tests/test_e3_v4_orm.py:20`, `app/chamados/tests/test_e3_v4_orm.py:42`, `app/chamados/tests/test_e3_v4_orm.py:101`.
- Outros leitores de teste explícitos da consulta: `app/chamados/tests/test_esmeril2_left_join.py:19`, `app/chamados/tests/test_fio_le_o_juiz.py:267`, `app/core/tests/test_dashboard_badge.py:45`; contratos AST em `app/chamados/tests/test_contract_pergunta_viva.py:68`, `app/chamados/tests/test_pergunta_no_app.py:168`; ferramentas de mutação em `app/chamados/tests/e3_mutantes.py:23`.

_3785 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
