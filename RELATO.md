# RELATO — esteira saas-hasner

**PERGUNTA DE LEI (folha/export x um juiz) -- a fila SEGUE sem ela, TRES partes de uma origem** (PAREI-DE-LEI-NAO-DEVOLVE-TURNO; **nenhuma lei nova proposta**, L-117). Tudo medido na frota real, so leitura.

- **(1) O botao de aprovar passa a barrar tambem o balde `bloqueado` do juiz?** Universo **993** `FechamentoMensal` ABERTO com linha no retrato gravado (`logs/fx/dois_por_dois_20261012.out`, `sem_linha_no_retrato=0`): botao barra x juiz bloqueia **310** · barra x juiz livre **172** · passa x bloqueia **352** · passa x livre **159** -- **discordam 524 (53%)**. Unificar barraria **+352** (349 `furo_espelho`, que e o juiz da CELULA, e 3 `ferias_com_batida`) e liberaria **-172** (95 `rescisao_modulo_proprio`, que o aval do retrato manda nao contar como bloqueio, e **77** que o juiz diz `(entra)` e o botao barra por `horas_trabalhadas == 0` sem `cadastro_zero` firmar). Os 77 sao contradicao VIVA: entram no TXT, nao podem ser aprovados pelo botao, e **o ato de GERAR o TXT os aprova** (`folha/views.py:164`, lei 31/07 *"aprovado e consequencia do ato de gerar"*). Nao e crivo mais largo: o botao julga por dois CONTADORES gravados e o juiz pela CELULA SOBERANA -- sao duas AUTORIDADES.
- **(2) A linha `:375` do `_K1` -- a familia fica com DUAS fontes para "turno aberto bloqueia?" durante toda a travessia do O48, e foi a sua ordem que escolheu isso.** MEDIDO: o comboio que acabou de pousar poe `aprovar_colaborador` lendo a CELULA (`_lido.turnos_abertos`, via `fechamento_lido`), e `folha/export.py:716` le o **GRAVADO** (`motivos_retencao_espelho(fech.turnos_abertos, ...)`); `folha/export.py` tem **0** ocorrencia nos 17 paths do comboio. Logo migrar `:375` para o campo novo **devolveria aquele sitio ao gravado** -- regressao da cura de hoje --, e montar o crivo a partir de `fechamento_lido` dentro do juiz e N chamadas de motor por lote, isto e **O48 passo 5**, que o seu aval proibe antes do export da competencia 10. **Declaro `classificar_export` juiz do `_K1` com a fonte de hoje (gravado) e o passo 5 move as duas juntas, ou a declaracao espera o passo 5? SIGO PELA PRIMEIRA** -- juiz unico e sobre QUEM responde, nao sobre qual campo ele le, e a **L-099** separa estrutura de dado. `:375` **fica** como pendente, com a impressao intacta (**PROIBIDO** editar impressao para o pendente sair).
- **(3) A linha `:377` (`folha/views.py`) sai como UNIFICACAO ou como CONFORME?** A impressao ja a nomeia: *"o TXT aprova pelo entra de classificar_export, sem os crivos do botao, sem trancar"*. (a) unificar = o DIFF de (1), que e L-009; (b) CONFORME = declarar que a pergunta dela e *"quem ganha o carimbo pelo ato de gerar?"*, derivada do `status` e respondida pelo MESMO juiz, com a condicao de queda escrita no sitio -- a forma pela qual `holerite/matriz.py` saiu do `_K5` em 04/10 por corte literal seu.

**O QUE EU CONSTRUO SEM AS TRES RESPOSTAS** (a parte INVARIANTE, DIFF 0 por CONSTRUCAO): o `_K1` ganha autoridade declarada (`folha/export.py::classificar_export`, **juiz novo = 0** medido pela varredura do proprio selo), nasce o campo declarado `travas_aprovacao` com o predicado LITERAL do botao de hoje, e os crivos do LOTE passam a LE-LO -- universo unico pela costura que ja existe (`classificar_export(..., fechamentos=<a propria lista do sitio>)`, 2 injetores de producao hoje), UMA materializacao, uma chamada por EMPRESA (o `qs` do botao cruza empresas quando `empresa_id` vem vazio, `ponto/views.py:1223-1225`), **sem ramo de ausencia** e sem fallback. Sai **1 das 6** linhas do `_K1` -- so a `:373` (conta vazia) --, `PENDENTES['fechamento']` **12 -> 11**, e o teto desce no MESMO commit (`len(pend)` **13 -> 11**, vetor `_teto` **(8,0,1,4) -> (7,0,1,4)**) -- apertar a guarda, nunca afrouxa-la. **ERA 2 ATE EU MEDIR, e a linha que nao sai e a medicao que me corrigiu**: a impressao da `:374` (`tipo_escala__tipo_ciclo='12x36',`) aparece **DUAS vezes** em `ponto/views.py` -- `:1120` (TELA) e `:1271` (lote) --, logo migrar so o lote deixa a impressao no arquivo e o pendente **fica**. E a TELA nao migra nesta fatia por medicao, nao por escolha: `ponto/services/fechamento_lista.py:64-70` so chama o juiz `if empresa_id:`, por decisao de PERF DECLARADA na DOCSTRING da propria porta (`:43-47`: *"`empresa_id` NAO e enfeite: `prontidao` e o juiz da folha so rodam com empresa escolhida (31/07 PERF -- em 'Todas' seriam centenas de leituras de escala para uma coluna que ninguem consegue ler)"*) -- e aqui eu errei a FONTE antes de reler: o numero **6424 queries / 4.1s a cada troca de mes** e do `prontidao` (comentario do `_pr`, `:78-82`, ~600 colabs POR LINHA), **nao** do juiz, que corre UMA vez por lista (`:64-70`); o que a docstring poe sobre o juiz e a decisao QUALITATIVA, sem numero medido. Enquanto `_ids_anc_tela` (`ponto/views.py:1119-1124`) e os quatro contadores (`:1166-1180`) correm **SEMPRE**, inclusive em "Todas", onde o juiz esta ausente -- fazer a tela ler a trava e REVERTER aquela decisao de desenho, e usar o juiz quando ha empresa e a query propria quando nao ha e **fallback** (PROIBIDO). Vai para a fila com numero, em duas linhas: (i) `:374` tem **DOIS escritores** do predicado da ancora (tela e lote); (ii) o predicado de VAZIO da tela **ja difere** do do botao hoje -- tela `float(r['fechamento'].horas_trabalhadas or 0) == 0 and (r['turnos'] or 0) == 0` (horas **E** turnos) contra botao `qs.filter(horas_trabalhadas=0)` (horas sozinhas), um TERCEIRO derivador ao lado de `ponto/views.py::reavaliar_vazios`. A celula da matriz **NAO fica verde** (a excecao declarada e `PENDENTES_FECHAMENTO` INTEIRA: 12 linhas, 6 do `_K1` + 4 do `_K4` + 2 do `_K8`), entao o placar segue **15/20** -- dizer 16/20 aqui seria rotulo, nao medicao.

`!` DE JANELA DE AUTH -- **RESPONDIDO PELA PROPRIA LEI em 12/10 06:08, e o `!` NUNCA FOI GASTO** (aberto 12/10 01:1x -- **nao devolvia turno; o default era a LEI, que e ESPERAR as 06:00, e a fila SEGUIU**). O que ele pedia era a TRAVESSIA (`SEM_JANELA_AUTH_MOTIVO`, atalho, NUNCA PRE-APROVADO), e a travessia nao aconteceu: o gate esperou a janela abrir e publicou DENTRO dela. Fica aqui como HISTORIA do default, nao como aval em pe. A O44 POUSOU (`70d159b9`, 63 paths, arvore do indice == `d7f17cd9` certificada) e o `bin/deploy.sh --sem-migrate` foi **CORRETAMENTE RECUSADO** pela janela de auth as **00:39**: `app/api/views.py` e sitio de auth declarado (`bin/auth_sitios.txt:20`) e 00:39 esta dentro da faixa 23:20-06:00 do P0 de 20/09. **PROD esta em ESTADO MISTO e os numeros sao estes:** os 56 `.py` do merge estao no DISCO e **nao** nos workers (`logs/deploy.stamp` `COMMIT=bf783df7`); o UNICO template do merge so **removeu** um ramo (`dia-trancado`) e os seus `{% load tranca_tags %}` estao dentro de `{% comment %}`, que **nao compila**; a exposicao real e o BUG 143 -- **3** imports LAZY de modulo deletado na arvore nova (`chamados/catalogo/recusa.py:18` via `_FRASE_DA_TRANCA` de nivel de MODULO, `chamados/services/regularizacao_ext.py:50`, `ponto/views.py:3131`), alcancaveis por disputa/veredito/validacao do admin e materializacao de regularizacao externa, **nunca por batida** (`triagem_batida.py` importa `catalogo.motor`/`premissa`, jamais `recusa`); **500 desde o merge: saas_ui 0, saas_core 0**. A travessia custaria `SEM_JANELA_AUTH_MOTIVO`, que e **atalho** e por isso o seu `!` (NUNCA PRE-APROVADO). **Nao atravessei e nao usei a porta isenta do `--reload-agendado` como caminho de tras** -- ela era contorno, e virou a cura do marco abaixo. **O deploy FOI FEITO PELO GATE em 12/10 06:08**, nao a mao -- GATE TEMPORAL (cron de 1 disparo, corte 16/09: espera longa nunca e processo do Code), `bin/deploy_agendado.sh` rotulo `o44-noar`, fatia `fatias_agendadas/o44-deploy`, usuario `ronald`. Veredito LIDO POR ARQUIVO: `fatia.done` comeca com `OK 12/10 06:08 carimbo=4c9cad93` -- e esse arquivo e escrito em TODOS os NOVE desfechos da esteira, oito deles `PAROU_*`, entao o que prova o deploy e o `OK`, nunca a existencia do arquivo. Os tres numeros que escolheram 06:08, e nenhum deles e "depois das 06:00": a borda da janela de auth e 06:00; a CORRENTE do cartorio nasce 06:27 e leva ~18 min (medido 11/10 23:08->23:23), e o deploy normal e `docker restart` (deploy.sh:279), que derruba o `docker exec` do elo encadeado por ARQUIVO; e a O44 muda o PISO DA EMISSAO, que e justamente o que `processar_cartorio` usa as 06:27 -- publicar antes da corrente poe a cura em uso HOJE. 06:08 ja tem precedente de ponta a ponta nesta casa (`logs/deploy_agendado/o221-pouso3.done`, 08/10 06:08:15, deploy real a partir do cron.d).

POUSO-EM-COMBOIO, a resposta em uma linha (12/10 03:5x) -- **as DUAS raias estavam prontas, entao e UMA copia e UMA suite.** O gate `o44-noar` das 06:08 PINA `HEAD==4c9cad93` e exige `app/` limpa (`fatias_agendadas/o44-deploy/esteira.sh:46-58`), logo **nenhuma das duas pode pousar antes dele** -- e trocar o PIN a mao poria obra nao-O44 num deploy sem ninguem olhando, cujo smoke e o do O44. Medido, nao suposto: **O48 passo 3 = 11 paths** e **O247 = 6 paths**, as duas copias nascidas de `4c9cad93` (censo `git read-tree` + `diff --name-status`), **intersecao VAZIA** (`comm -12`) -- mas disjunto e o criterio de CONSTRUIR ao lado (RAIA-NAO-FICA-PARADA), **nunca o de certificar**: a suite que certificou `main+O247` nao certificou `main+O48+O247`. Entao a arvore do pouso e UMA, `$S/comboio/copia` = `4c9cad93` + os 17 paths (12 M + 5 novos, `cmp` 17/17 contra as copias de origem), e a suite dela correu atras do veredito da do O247 -- que voltou **VERMELHO**, e nao no produto: `Ran 10456` / `FAILED (failures=1)`, unica falha em 10.456, e era o meu PROPRIO RED novo furando TESTE-SEM-RELOGIO (corte de 19/09) por ler `timezone.now()` em tres casos e cravar o dia no quarto -- a classe exata da bomba de 19/09, porque `detectar_bloqueio_turno_orfao` julga pelo INSTANTE e `_completude` por um DIA cravado. Curado com `@freeze_time` no instante em que os 9 verdes foram medidos (nenhum nome entrou em `testes_sem_relogio.txt` -- a lista SO ENCOLHE), conferido chamando a FUNCAO REAL do selo (4 sitios -> 0, e o conferidor morde), RED 4/4 e GREEN 9/9 REFEITOS, ruff limpo nos 16 `.py`. O runner do comboio RECUSOU sair com a raia vermelha, que e o unico motivo de ele existir. A certificacao do pouso e UMA: a suite do comboio, e ela voltou **OK (skipped=42)** / **Ran 10472 tests in 1444.394s**, rc_comboio=0 (`logs/o247/suite_comboio.out`), com o censo da clausula 2 da L-117 feito por `sha256sum -c logs/o247/comboio_17.sha256` sobre a arvore VIVA depois do `cp`: **17 de 17 OK**. Ordem do pouso, pela L-105 (produto antes de instrumento) e pela fila declarada: **O48 passo 3 primeiro, O247 depois**, dois commits e UM `bin/deploy.sh --sem-migrate`, e **depois do carimbo `logs/fim/lavrar_retrato_folha.-.20261012`** -- a faixa 06:15-06:27 nao cabe dois commits mais deploy, e a corrente do cartorio nasce 06:27.

PERGUNTA DE LEI (nao devolve turno, L-117/PAREI-DE-LEI-NAO-DEVOLVE-TURNO) -- **a O44 destrava o TOQUE, e a premissa "passivo 1.994" da linha dela e FALSA: o que a tranca calou nao esta arquivado, esta INEXISTENTE.** O censo e o universo da O44 estao FECHADOS e sao a prova; a fatia SEGUE pela parte que a lei ja responde (o toque do admin) e esta pergunta sobe com os numeros, sem parar nada. Tudo medido na sombra, so leitura, `rc_processo=0`: `logs/o44/CENSO_VIVO_20261011.md` (a autoridade de cada linha), `logs/o44/sonda_universo_20261011.txt`, `logs/o44/sonda_universo2_20261011.txt`.

**RESPONDIDA 11/10 ~19:5x -- aval `O44-LEITURA-C`, a leitura (c).** Letra dele: *"Horizonte declarado: a emissao passa a ter piso de data, 2026-09, pela mesma forma do REGRA_VERSOES do cartorio. Os 1.390 dia-colab calados em competencia trancada ficam abaixo do piso por decisao escrita; so o dia tocado pelo admin emite. Nenhum chamado nasce em lote."* O item (4) abaixo FICA DE PE sem uma linha mudada, e era o que ele previa: a cura de escopo ja construida nao se desfaz -- o que entra e **um ATO a mais**, o piso declarado, no molde de `ponto/services/cartorio.py:85` (`REGRA_VERSOES`) + `versao_para:159-166` (`horizonte <= comp`, competencia por `ponto/janelas.py::janela_atual`, zero juiz novo). A obra e da raia `wt-bos` (`faa783e1`), que ja tem o RED em pe (`app/chamados/tests/test_o44_tranca_nao_barra_o_toque.py`) com `cartorio.py` e `chamados/juizes.py` modificados; o aval foi RELAIADO a ela no mesmo turno e a linha do `PROMPTS.md` esta escrita. Censo do leitor da tranca, medido antes do relato: `chamados/juizes.py::dia_em_competencia_trancada`, **7 arquivos de producao**.

- **(1) OS QUATRO NUMEROS.** Em competencia trancada (5 trancas vivas: emp2 08, emp3 07, emp3 08, emp4 07, emp4 08) ha **1.390** dia-colab com veredito de FURO. Pelo leitor DO PROPRIO CARTORIO -- `ponto/services/cartorio.py::chamados_que_falam_de_batida:58-82`, `modulo_origem__in=FURO_MODULES` + `chamados/catalogo/modulos.py::data_do_chamado`, nao um censo meu --, desses 1.390: **0 com chamado vivo, 0 com chamado ARQUIVADO, 0 encerrado por outra via**. Sao **1.390 SEM NADA**. O arquivo da via (`competencia_trancada`) existe e e grande -- **638** chamados e **1.994** perguntas --, mas **nao cobre um unico dia de furo**: ele arquivou o que ja havia NASCIDO antes da tranca. E a tranca **ja foi rejulgada 7.431 vezes** (`julgada_em > fechado_em`), **796 delas em celula de furo** (emp2 08 sozinha: 7.144 celulas / 720 furos) -- os furos do col317 foram julgados em **2026-09-30**, duas semanas DEPOIS da tranca de 16/09.
- **(2) O QUE ISSO SIGNIFICA, e e a parte que muda a fatia.** O cartorio **nunca deixou de julgar** dia trancado -- ele deixou de **EMITIR**, por UMA clausula: `cartorio.py:829-830` (`_cala_emissao = bool(vivos) or _trancado`). A tranca **nao e insumo da impressao** (`impressao_insumos:168-197` hasheia batidas, cobranca, chamados, dna, veto, teto, regra -- nao ha tranca entre as partes), entao a celula carimbou e recarimbou com a emissao calada. Nenhum arquivamento, nenhum registro: o furo foi **calado no silencio**, e por isso o "passivo 1.994" da linha do BACKLOG descreve outra coisa (as perguntas que a via arquivou), nao o que a cura encontra.
- **(3) A PERGUNTA, uma so, e as tres leituras levam a trabalhos DIFERENTES:** quando o `_trancado` deixar de calar o toque, **o que acontece com os 1.390 que a tranca calou e nunca arquivou?** (a) **nada** -- so o dia TOCADO pelo admin emite, e os outros 1.389 seguem calados para sempre sem registro de que foram calados; (b) **emitem na proxima varredura** -- o cartorio ja os rejulga (796 provados), e tirar a clausula os poe na fila: **1.390** chamados + perguntas nascendo de uma vez em competencia ja paga; (c) **HORIZONTE DECLARADO**, pela forma que esta casa JA usa -- `cartorio.py:85 REGRA_VERSOES = ((1, '2026-09'),)`, cujo comentario diz em letra que *"exportada NAO e criterio -- o que recorta e o HORIZONTE"*: a emissao passa a ter piso de data declarado, e os 1.390 ficam abaixo dele por DECISAO escrita, nao por efeito colateral de uma tranca.
- **(4) O QUE A FILA SEGUE FAZENDO SEM ESPERAR A RESPOSTA, porque a lei existente responde.** A fatia **em construcao** adota a leitura **(a)**: a cura e de ESCOPO, nao de recusa -- `_cala_emissao = bool(vivos) or (_trancado and not forcar)` --, que e exatamente a forma que `escala/services/furos_diarios.py:66-69` ja usa para separar varredura de toque. Medido chamador por chamador (tabela no censo): **`config/crons.py` nao passa `--forcar` em lugar nenhum**, `furos_sem_cobranca.py:69-70` tira o dia trancado da varredura ANTES (essa linha FICA), `rejulgar_ausencia_em_folga` so ve `janela_atual`, `curar_residuo_bug115` so ve dia que ja tem chamado vivo, e `pos_vinculo` nao alcanca tranca porque o piso de `colaboradores/services/vinculo.py:34` e **2026-08-21**, depois das 5 janelas. Entao a cura muda **zero** byte do caminho automatico: quem emite e o CLIQUE. Se a resposta for (b) ou (c), o que falta e um ATO a mais (varredura autorizada ou piso declarado), nao desfazer esta.

PERGUNTA DE DESENHO (nao devolve turno) -- **GEO-SILENCIO-DE-PING: a premissa do aval nao se cumpre em producao, e o aparelho que o vigia deveria vigiar NUNCA PINGOU.** O censo que o aval deixou para "a vez" e a MEDIDA que ele exigiu (*"X e Y nascem de MEDIDA, nao de palpite"*) estao fechados; o vigia **nao se construiu**, e pela PAREI-DE-LEI-NAO-DEVOLVE-TURNO a pergunta sobe com os numeros e a esteira SEGUIU o proximo item da ordem do proprio aval (GEO -> **O197** -> O44). Nenhum dado tocado: tudo na sombra e so leitura, `rc_processo=0`. PROVA: `logs/geo/censo_leitores_de_ping_20261011.txt`, `logs/geo/medida_x_y_20261011.txt`, `logs/geo/porque_sem_ping_20261011.txt`, `logs/geo/quem_pinga_20261011.txt`, decisao em `logs/geo/ACHADO_o_aparelho_nunca_pinga_20261011.txt` (131 linhas), fila em `logs/fila/geo_tres_portas_de_ping_20261011.txt`.

- **(1) NAO EXISTE X UTIL, e o numero e esse.** Em 14 dias (27/09-11/10), **6143 de 6177 turnos (99,4%) tem ZERO ping**; **2** turnos chegam aos 4 pings que `ponto/presenca.py:12` declara "o esperado" (decidido S87); **444 de 480** colabs com entrada na janela NUNCA pingaram. Os gaps dizem o resto: `entrada -> 1o ping` p50=**341 min**, p95=716, max=**724**; `ping -> ping` consecutivo max=**256 min**. Qualquer X abaixo de ~4-12 h alarma comportamento NORMAL dos que pingam; e com X util os **36** que pingam seriam alarmados em **461 de 495 turnos (93,1%)**.
- **(2) O APK KOTLIN NUNCA PINGOU UMA VEZ**, e isso se mediu por PING, nao por colab -- a discriminacao por colab e por `version_code` nao separava nada (320 colabs `app` nunca pingaram e 10 colabs `app` pingaram). Pelos tres marcadores que `ponto/presenca.py:14` nomeia como *"preenchidos pelo APK Kotlin; vazios em PWA"*: `timestamp_dispositivo_ns` preenchido = **0 de 505**, `provider` vazio em **100%**, `assinatura` preenchida em **3 de 505** (1 colab). Sao **505 pings em 5 meses** (24/05 a 11/10), **45,3% deles de UM colab** (id 549), top 10 = 77,8%.
- **(3) A ORIGEM ESTA NO CLIENTE, e e a LEI-AKITA 1 que decide a fatia.** O unico chamador de front do repo e `static/pwa.js:404` (grep de `ping-geo` em `static/` + `templates/` = **1** ocorrencia), e ele agenda um `setTimeout` de **15-90 min aleatorios** armado no `DOMContentLoaded` (`:380-390,418`): timer de PAGINA, sem `periodicSync` de service worker. O celular que descarta a aba em segundo plano mata o timer antes dos 15 min -- **isso E o 93% de zero**. Vigia servidor-side nao cura cliente que nao fala.
- **(4) POR QUE O VIGIA NAO SE CONSTRUIU HOJE**, por lei existente e nao por gosto: (a) **LEI-AKITA 1** -- a cura iria no sitio errado, e metade do cliente (o APK) nao esta neste repo, logo curar so o PWA seria a **MEIA-CORRECAO** que a casa proibe; (b) o **precedente O124** (03/10) tirou da tela 15 parametros editaveis sem leitor, e nascer dois campos de `Empresa` cujo leitor vigia universo vazio e exatamente o que a casa acabou de desfazer; (c) a migration custaria deploy COM migrate, portao da sombra vermelho e ~38 min de ensaio devido, por instrumento inerte. **0 juiz novo, 0 linha de codigo, 0 migration, 0 deploy.**
- **(5) O `!`, em uma linha** (`GEO-SILENCIO-DE-PING-ORIGEM-NO-CLIENTE` no `PENDENTES_RONALD.json`, nao trava a fila 1): **(a) CURAR O CLIENTE PRIMEIRO** -- o ping passa a nascer de evento que sobrevive a aba fechada (service worker / disparo na batida e no `visibilitychange`) mais o release do APK, **irmao do `APP-DESENHA-A-PALAVRA-DO-DIA`** que ja esta na mesa pelo mesmo motivo (metade da cura e release de app) -- e so entao o vigia; **ou (b) CONSTRUIR O VIGIA INERTE AGORA** mesmo assim, ciente de que ele vigia 36 de 480 e que com qualquer X util alarma 93% dos turnos deles. **NAO esta na mesa:** X alto para o contador ficar quieto (mudar tolerancia para o numero cair, PROIBIDO pela **L-099**) nem cravar X no codigo (PROIBIDO no proprio aval). A **O-GEO-DECISAO** segue CONGELADA pela **L-096**.
- **(6) O CENSO CORRIGIU O AVAL em dois pontos, e fica dito:** `ponto/painel_op.py` **nao existe** -- o leitor vivo e `core/services/painel_op.py:30,97`, e ele JA le em LOTE --, e `ponto/views.py:2900` e hoje `:3049`. O resto: `ponto/presenca.py` **nao tem nocao de TEMPO** (turno com zero ping devolve `'cinza'`, o MESMO valor de "100% dos pings sao mock"), entao o leitor de silencio nasceria sem concorrente -- o eixo do SELO *"leitor com regra propria fora de `presenca.py` = 0"* esta verde HOJE nos 4 leitores de presenca --, e as duas autoridades de LOTE que o aval exige **ja existem**: `ponto/turnos.py::turnos_abertos_de` (5 queries para o universo, marco-aware, que da o **RED f** de graca) e `ponto/presenca.py::pings_por_batida` (1 query). O "isento" do **RED e** tem juiz na casa: `core/constants.py:13::isento_ponto`.
- **(7) QUATRO LINHAS DE FILA, nao curadas agora (ESMERIL-DO-MARCO)**, em `logs/fila/geo_tres_portas_de_ping_20261011.txt`: as **tres portas de ping** (`ponto/views.py:2721`, `api/views_core.py:915`, `api/views.py:2287`) com **DUAS regras de alerta** diferentes, e a divergente (`ponto/views.py:2800-2813`, sem teto de 2 h, abrindo `ChamadoColaborador`) e a **UNICA que dispara**, porque e a unica que o front chama -- as duas copias de `api_ping_geo` nao tem **um** ping no nome; `api/views.py:2287` como escritor duplicado inalcancavel (candidato **L-111**); as duas referencias corrigidas do aval; e a expectativa declarada de 4 amostras/turno que a producao nao cumpre (2 de 6177).

`!` DE CADASTRO DE ESCALA (novo, 11/10 01:5x) -- **o `!` do BACKFILL DOS 7 esta VAZIO como foi dado, e o passivo nao e "cadastro incompleto" em geral: e CALENDARIO PREENCHIDO PARA UM TAMANHO DE MES SO.** Nao e pergunta de LEI: a lei existe e responde (**L-099** -- dono CADASTRO, *"e proibido curar por codigo"*; **L-009** -- dado de ESCALA e o seu `!`), e a **L-117** proibe eu propor lei nova. O que falta e o CONTEUDO do cadastro, que so voce ou o admin sabem. Registrado em `PENDENTES_RONALD.json` como `O148-TEMPLATE-DE-CALENDARIO-DE-FOLGA` (`!`, nao trava a fila 1). A fila SEGUE (PAREI-DE-LEI-NAO-DEVOLVE-TURNO). PROVA, toda na sombra e so leitura: `logs/foto_folga/primeira_medicao_20261011.txt` (a PRIMEIRA MEDICAO que o aval pediu), `logs/foto_folga/cal_dos_7_20261011.txt` (por calendario, pelo JUIZ DA CASA `EscalaColaborador.defeito_folga_nao_definida`, nunca por regex) e `logs/foto_folga/janela_corrente_e_seguinte_20261011.txt` (a FUNCAO REAL `medir()`, hoje e na virada).

- **(1) os 7 com o seu `!` sao UM calendario, e o template dele esta VAZIO.** Os vinculos sem nenhuma `FolgaDia` em 10/2026 que o juiz acusa apontam todos para `cal#28`, modo `semana_dia`, **`celulas=0`** -- zero celula gravada. `datas_que_o_calendario_manda(cal#28, 2026, 10)` = **0 datas**, e o juiz acusa **7 de 7**. Entao a lavra **nada tem a gravar ali**: *"aplica se SO esses 7 mudam"* nao pode ser cumprido, porque **nenhum dos 7 muda** -- nao ha data para gravar. O backfill por codigo nao e a cura; a cura e **preencher as celulas do `cal#28`**. URGENTE pelo que vale: 7 colabs em competencia **ABERTA**, todo domingo desde 01/10 contando como dia de trabalho.
- **(2) a CAUSA, medida, e uma so -- e nao e "novembro".** Calendario `dias_mes` se preenche **por TAMANHO DE MES** (`FolgaCalendarioTamanho.tamanho_mes` em 28/30/31), e a casa preencheu **UM tamanho so em cada calendario**. Os 50+ medidos tem `{28: 0, 30: 0, 31: N}` -- so o de 31 --, entao funcionam em outubro e **caem em todo mes de 30**: em 11/2026, **67 dos 71** recebem ZERO data e **47** sao acusados. E o espelho disso ja esta vivo HOJE: `cal#50` tem `{28: 0, 30: 1, 31: 0}` -- so o de 30 --, e cai em **outubro**. Nao e a lavra que falha: ela pergunta ao calendario e o calendario responde *"nenhuma folga"*.
- **(3) o OITAVO caso de 10/2026 abre classe que ninguem havia nomeado, e o juiz da casa e CEGO para ela POR DESENHO.** Sao **8** sem foto, nao 7: o oitavo e `ec910`, `cal#50`, e ele e **12x36**. `defeito_folga_nao_definida` so olha 6x1, entao devolve **False** -- e esta **certa**, nao e bug dela. Quem ve e a vigia nova `escala/folgas.py::calendario_com_template_vazio_no_mes`, e e exatamente por isso que ela existe ao lado do juiz, sem o substituir (0 juiz novo).
- **(4) a PRIMEIRA MEDICAO respondida (aval, item "publicada ANTES da cura"):** dos **71** do universo, **63 com foto** em 10/2026, **303 `FolgaDia`** -- **303 de `calendario_origem` e ZERO manual** (so-calendario 63, so-manual 0, misto 0). A "colisao manual x calendario = 0" que ele mediu em prod tem causa estrutural: **nao existe uma FolgaDia manual neste universo**. A MUDA 2 segue no codigo como GUARDA, com RED sintetico declarado -- nao ha caso vivo. `gerado_em`: 9 em 01/09, 9 em 02/09, 193 em 02/10, 65 em 06/10, 4 em 07/10, 10 em 08/10, 9 em 09/10; as de 11/10 sao **ENSAIO MEU nesta sombra** (o `--apply` cronometrado).
- **(5) o que a lavra faz, e por que NAO cura:** manda ao contador `calendario_com_template_vazio_no_mes` (esperado 0) e **nao escreve nada** -- dono **CADASTRO** pela **L-099**. **O prazo e real e esta medido** com a janela do aval (corrente + seguinte): HOJE, universo **203 vinculo-mes**, **4 alvos / 23 datas** (re-medido as 04:5x na sombra REFEITA de hoje, `dia=20261011 status=OK`; eram 3/19 na sombra de 10/10 -- entrou `ec1278 10/2026` com 4 datas, causa nao investigada: fora do marco, vai para a fila) e **132 de template vazio** (57 em 09 + 8 em 10 + 67 em 11); em **21/10**, universo **213**, **66 alvos / 321 datas** e **83** de template vazio. Em 21/10 as celulas de novembro nascem as 05:50 com os 67 trabalhando todos os dias.
- **(6) o `!`, em uma linha:** preencher template de calendario de folga em massa -- os tamanhos **28 e 30** que faltam nos 50+ calendarios `dias_mes` e as **celulas do `cal#28`** -- e **cadastro pela UI, do admin**, ou voce autoriza um ato de cadastro com trilha pela porta; e, se autoriza, **com que conteudo**, que e justamente o que o sistema nao sabe deduzir sem inventar regra. (Lateral, para a fila e nao para voce: a tela deveria **RECUSAR** salvar calendario `dias_mes` com tamanho vazio -- itens 2 e 5 do complemento.)

- **(7) o CRON DA LAVRA NAO FOI INSTALADO, e esse e o estado honesto (11/10 04:5x, depois do pouso).** Instalar `47 5 * * *` nao e passo mecanico: **ele E o apply**, porque arma um `--apply` para as 05:47 de hoje. Re-perguntei o numero a funcao REAL na sombra refeita, em dry-run inerte (`lavrar_foto_folgas.py:159-161`): **4 vinculo-mes / 23 datas** -- `ec1278` 10/2026 (4), `ec910` 11/2026 (1), `ec184` 11/2026 (9), `ec749` 11/2026 (9) --, e **nenhum deles e dos 7**. O aval diz *"aplica SO se esses 7 mudam e ninguem SAI do TXT; qualquer outra coisa = PAREI com a tabela"*: os 7 estao entre os 132 de template vazio e nao mudam, logo a condicao e INCUMPRIVEL por cadastro (L-099) e o resto e "qualquer outra coisa". Entao **`bin/crons.sh install` nao rodou** e `crontab -l | grep -c lavrar_foto_folgas` = **0** -- as 05:47 nada dispara e nao havera log para ler. **O vermelho disso e CONTADOR, nao portao, e isso esta MEDIDO**: `crons.sh check` nao e lido por `bin/deploy.sh`, `bin/pre-push.sh` nem `bin/hook_stop_fila1.py`; o unico leitor e `bin/placar_code.sh:37`, que engole o rc e conta `^> ` (sai como a linha de placar de `:491`); e o cron das 04:05 roda `check --medir`, **nunca** `install` -- nada se arma sozinho amanha. O codigo FICA no ar (o command e inerte sem o cron) e o placar fica em 1 cron declarado-e-nao-instalado: nao se maquia tirando o cron do `config/crons.py` nem instalando "so para ficar verde". **E o cron nao inventa escrita**: `escala/folgas.py::gerar_foto_folgas` ja tinha **3 chamadores de producao** (`colaboradores/services/vinculo.py:131`, `colaboradores/views.py:246`, `escala/views.py:418`), entao o que ele muda e o **GATILHO** -- sem abrir tela, frota inteira de uma vez --, nao a regra. Se o `!` vier, a ordem nao e cortesia: **DIFF "ninguem SAI do TXT" publicado ANTES** e so entao o `install`, porque `2026-10-07` e dia JA VIVIDO da competencia corrente. Tabela inteira em `logs/foto_folga/PAREI_install_backfill_20261011.txt`; dry-run em `dryrun_pos_pouso_20261011_0450.txt` (rc_processo=0). **[SUPERADO em 11/10 11:15: o aval dele derrubou a condicao do BACKFILL DOS 7 (*"os 7 sao cal#28 vazio, cadastro"*) e o `install` RODOU -- `crontab -l | grep -c lavrar_foto_folgas` = 1, `crons.sh check` rc 0. O que este item MEDIU segue verdadeiro para o estado das 04:5x; o estado de agora esta no MARCO 11:1x.]**

PERGUNTA DE LEI x2 -- **RESPONDIDAS em 10/10 18:0x** pelos dois cortes dele (`juiz dia_encerrado nasce` e `juiz par_completo nasce`), que viraram a **O248** e a **O249**. Os numeros ficam AQUI, e nao se apagam, porque e deles que a obra parte: as duas seguram o **item 4 do R6** do PLACAR-ESTRUTURAL e as duas eram CORTE, nao implementacao -- `JUIZES_POR_VARREDURA` = **25** em 08/10 e **25** em 10/10, e os dois nomes que a lei dele de 04/10 destinou ao papel `prazo` seguiam dentro dela por falta de corte, nao por omissao. PROVA: `logs/placar_estrutural/numeros_do_r6_20261010.txt` (todo numero por AST do vivo).

**(1) `corte Ronald: juiz dia_encerrado nasce`** -- a pergunta que estava PERDIDA, e o caso e meu: `config/crons.py:1480` diz que ela *"esta no topo do RELATO"*, e ela nao estava -- caiu quando eu reescrevi o topo; em `docs/CORTES.md` a frase nao existe (`grep` 10/10). Volta com os numeros originais: sem ela o `flip_automatico` nao pode delegar *"o dia esta encerrado?"* e ficam **2 derivadores paralelos da mesma pergunta** -- `ponto/services/flip_auto.py:274` (`if dia >= hoje`) e `ponto/supra_juiz.py:149` (`dia_encerrado = not (hoje and data and data >= hoje)`). Quem escreve `flip_tipo` e escritor unico, entao a pergunta decide o universo de um cron de `--apply`. A outra candidata que JA EXISTE e perguntar `t.aberto` a `turnos_do_colab` (turno encerrado em vez de dia fechado) -- muda o universo do cron, entao tambem e corte.

**(2) o `detectar_par_relampago` pode ocupar o papel `prazo`, e com qual fato encerrado?** O VEREDITO dele JA delega, medido por AST em 10/10: `varrer_pares_embutidos:232` -> `par_embutido_retratavel:208` -> `_par_embutido_retratavel_geometria:220` -> `_score_geometria:86` -> `escala/utils.py::montar_grade_prevista_periodo_por_turno`, a autoridade da GRADE. O proprio dele e a CANDIDATURA (`:245-249`, `zip` adjacente + tipos alternados + gap <= `DURACAO_MAX_MIN=10`), e ela nao e derivacao de prazo pelo criterio do proprio selo -- fato GRAVADO contra teto de FORMA, sem relogio. **Nao movi o nome, e por duas razoes somadas:** (a) `bin/tests/test_papel_prazo_nao_deriva.sh` DECLARA no cabecalho que nao ve essa segunda porta e NOMEIA este cron, logo o verde dele seria ausencia de sinal, a familia que esta casa diz ser a que mais custou; (b) o seu aval nesta celula (`core/contratos_estruturais.py:252-262`) proibe em letra *"reclassificar o papel de um sitio para ela virar verde -- o que falta e juiz, nao rotulo"*. E a prescricao do censo de 04/10 nao fecha: `turnos_do_colab` nao enumera este par, porque `ponto/turnos.py::parear_turnos` declara *"E abre turno; S fecha o aberto"* e o par embutido e o ECO DENTRO de um turno -- ele nao E um turno (`pares_relampago` usa turnos_do_colab para o caso complementar, o turno inteiro curto). Entao: **(i)** o fato encerrado e o PAR, e ai falta funcao da casa que responda *"este par e completo?"* -> juiz novo, TRAVA JUIZ-NOVO; ou **(ii)** o fato encerrado e o TURNO QUE CONTEM o par (`t.aberto`), e ai `turnos_do_colab` ja e o juiz, mas de outra pergunta que a do censo -- e isso MUDA O UNIVERSO de um cron que roda `--apply` e RETRATA batida. Nao propus nenhuma (L-117).

CORTE QUE FALTA (TRAVA JUIZ-NOVO, nao devolve turno -- a fila SEGUE pelos 6 que a lei ja responde) --
**o item 2 da CONTRATOS-ATE-A-VIRADA (`folha/export x um juiz`) esta PARTIDO EM DOIS, e a metade de DINHEIRO e a
que precisa de voce.** CONTRATO DE ENTRADA, medido na FONTE (`core/juizes.py` importado no `saas_core`, so leitura,
rc 0 -- `logs/contrato_folha_export_juiz_20261011.out`): FONTE `PENDENTES['fechamento']` x `JUIZES['fechamento']`;
UNIDADE sitio; UNIVERSO os 13 pendentes declarados; EXCLUSOES nenhuma (os 13 contam).
- **os numeros da nota da celula CONFERIRAM hoje, nao envelheceram**: `len(PENDENTES['fechamento'])` = **13**
  (dinheiro **8**, tela 4, cobranca 1) e a **IMPRESSAO de todos os 13 ainda esta VIVA** no arquivo
  (`vivas=13 mortas=0`) -- nenhum pendente e fantasma, nenhum sitio se mudou de lugar.
- **6 dos 13 podem ser curados AGORA**, porque a pergunta JA tem juiz declarado: 4 em
  *"o que 'fechado' quer dizer"* -> `ponto/services/fechamento.py::competencia_trancada` e 2 em *"que
  FechamentoMensal e o da competencia de hoje"* -> `ponto/janelas.py::janela_atual`. **3 deles sao dinheiro.**
- **7 estao atras de CORTE SEU, nao de trabalho meu:** **6** na pergunta *"a competencia pode ser
  aprovada/trancada (o que bloqueia)?"* -- **5 de DINHEIRO**, em `ponto/views.py` (4) e `folha/views.py` (1), o lote
  cego que pula conta vazia e o TXT que aprova sem os crivos do botao -- e **1** na pergunta *"a competencia ja foi
  entregue (lavrada)?"* (`chamados/juizes.py`). As DUAS perguntas tem **juiz declarado NENHUM**, e registrar
  autoridade exige a frase `corte Ronald: juiz <nome> nasce` no CORTES.md. **Nao propus nome** (L-117).
- consequencia que fica dita: a celula `folha/export x um juiz` **nao fica verde** sem esse corte -- allowlist zero
  e a linha, e `PENDENTES_FECHAMENTO` nao chega a zero com 7 sem autoridade. O que a fila faz sem esperar: os **6**
  com juiz, que e cura de LEITOR (o sitio passa a chamar a autoridade), zero juiz novo.

**POUSOU: `0224e9d5`, e o SMOKE EM PROD e do caminho curado, nao da porta** (DEPLOY JA cobra o smoke com o
resultado aqui). Suite CHEIA sobre a arvore da raia: `Ran 10406 tests in 1447.405s` / `OK (skipped=42)` /
rc 0. A arvore que pousou e a MESMA que ela certificou -- `git write-tree` do indice mergeado =
`17a5ce2f4a5bd96f69adec22c9acfc0c8c9900a2` = `2e2a7fd4^{tree}`, provado ANTES dos docs entrarem no indice,
mais `cmp` arquivo a arquivo 12 iguais / 0 diferentes. `deploy.sh --sem-migrate` rc 0: 3 cascas
reiniciadas juntas, 3 rotas provadas, selo BUG 128 verde, `importerror_500=0`. SMOKE (prod, SO LEITURA,
`relatorios/cartao_pela_celula.py::totais_da_folha` chamada de verdade): competencia VIVA 10/2026, **60
colabs lidos, 60 com numero, 0 `None`, 0 erro, 175 ms/colab** -- o mesmo custo dos 176 ms medidos antes do
pouso, e nenhum colaborador caiu na abstencao por causa da cura.

- **FILA, e o smoke e que achou: ha FOTO de competencia FUTURA no banco de prod.** O censo de
  `FechamentoMensal` por (ano, mes) devolve **19 em 11/2026 e 18 em 12/2026** -- contra 587 na 10 (a viva),
  607 na 09, 618 na 08, 629 na 07. Hoje e 11/10/2026: a competencia 12/2026 corre de 21/11 a 20/12 e nao
  comecou. Isso e da familia da K8-FM-DE-HOJE ("que FechamentoMensal e o da competencia de hoje?"), nao
  deste marco -- ESMERIL-DO-MARCO manda virar LINHA, nao trabalho agora. Medido chamando o ORM em
  `saas_core` com as duas assercoes de `saas_hasner`, so leitura.

FILA DO MARCO DO PASSO 3 (nao devolve turno, nada parado por elas):

- **O PASSO 3 POUSA 1 DE 4, e foi o esmeril DO PROPRIO MARCO que derrubou a palavra que eu ja havia escrito**
  (ESMERIL-DO-MARCO; `app/docs/FECHAMENTO-ONLINE.md:60` dizia "os leitores por colab (cartao, ranking,
  `ponto/views.py`, scores) leem `fechamento_lido`" -- plural). MEDIDO na arvore da raia:
  `grep -rn fechamento_lido --include=*.py` fora de testes da **1** chamador de producao,
  `relatorios/cartao_pela_celula.py:73` (mais o command `cartao_x_txt_divergentes.py`). Os TRES que faltam
  seguem lendo o GRAVADO direto -- `folha/services/ranking_he.py:179`, `ponto/views.py:945` (o espelho de um
  colab) e `inteligencia/management/commands/calcular_scores.py:72`. Os `:1026`/`:1082` de `ponto/views.py` sao
  FROTA e pertencem ao passo 6, nao a este. A PROVA de frota (362/509/264/ZERO) e dos DADOS, nao do leitor: ela
  mede gravado x lido na competencia e vale igual para os quatro. Doc corrigido no mesmo commit do marco.
- **ESMERIL DO MARCO: zero orfao.** O marco removeu UMA linha de chamada (`from folha import export as fx` em
  `totais_da_folha`), e ela MUDOU DE SITIO para `_totais_do_fechamento` -- `fx.eventos_do_fechamento`,
  `fx._ciclo_do_colab`, `fx.feriado_12x36_em_dobra` e `fx.datas_falta` seguem com chamador. Nenhuma funcao
  perdeu o ultimo chamador de producao, entao nada a apagar pela L-111 neste marco.

**BALDE v = CADASTRO, nao ESTRUTURA (L-099; dono MEDIDO, nao PAREI).** Os 4 da 10 com horas e ata zerada (col960,
col956, col936, col511) tem ata LAVRADA e o juiz `realizado_do_dia` diz SEM_TURNO em 38 de 39 dias -> `ata_x_juiz_residual=0`:
as batidas provam DESLOCAMENTO DE UM DIA no DNA 12x36 (trabalham as noites IMPARES). **Proibido curar por codigo** -> Cadastro x
Realidade + Pauta DP. FILA, e aqui ha ESTRUTURA: `dias_cadastro_x_realidade` ve col956=16 e col511=1, mas col960=0 e col936=0 --
a lista que a L-099 nomeia como autoridade do dono e cega em 2 dos 4. PROVA: `logs/o48p3_baldev_{juiz,dono}_20261011.out`.

- **POUSO-EM-COMBOIO respondido por MEDICAO, nao por suposicao (L-117):** `git worktree list` + `git status --porcelain`
  por raia dizem que `wt-ui` esta **266 atras / 0 a frente** com arvore suja e `wt-bos` **5 atras / 0 a frente**, suja --
  nenhuma das duas fica pronta em 30 min, entao **`wt-o48p3` roda sozinha** e as duas pousam depois, UMA POR VEZ.
- **A PASTA DE SELOS DE HOST E VERMELHA NA WORKTREE E VERDE NO MAIN, e o motivo e AMBIENTE, nao codigo (medido 18:5x):**
  **7 de 69** vermelhos em `/home/ronald/wt-o48p3` (`test_handoff_sessao` -- `settings.json` ilegivel; `test_hook_nao_e_copia`
  -- `hook_pre_push=AUSENTE`, o hook mora no `.git` do repo principal; `test_prepush_testa_o_commit` e
  `test_montagem_vem_do_arvore_do_push` -- `app/staticfiles` existe e esta VAZIO, e faltam os pontos `app/logs` e
  `app/media` para o tmpfs; `test_furo_encadeado_ao_cartorio`, `test_import_tardio_contra_o_ar`,
  `test_inventario_pessoal_no_commit`) e **0 de 7** vermelhos no main para os MESMOS sete. Dois deles nao eram ambiente e
  sim DIVIDA desta copia, e foram pagos ANTES da suite certificadora: `mkdir app/logs app/media` + `collectstatic`
  (186 arquivos) na worktree, e `test_montagem_vem_do_arvore_do_push` virou **OK** com a porta entregando o
  `-v .../staticfiles:ro` e os cinco `--tmpfs`. Sem isso os cinco smokes de chromium dariam **RED FALSO** por css/js
  ausente -- o defeito que a memoria da casa ja tinha nome para ("suite em worktree precisa do staticfiles").

- **AS 6 RAIAS SEM AGENTE ganham MOTIVO, e a medicao derrubou a primeira linha que eu ia escrever** (item 4 do
  RAIAS-ANDAM-JA; `bin/raias_estado.py --linhas`: uniao=30, sem_agente=6, velhas=6). A declaracao canonica mora em
  `logs/raias/<raia>.agente` (escrita por `bin/raia_agente.sh`) -- quem nao tem obra viva nao ganha declaracao
  FALSA, ganha motivo aqui, que e o que o proprio medidor cobra:
  - `recon_onprem` -- **nao e raia**: `git merge-base HEAD main` responde **"no merge base"** (historia DISJUNTA,
    HEAD `612be166` de 04/07, "WIP chamados: pre-resync"). Os "1053 commit a frente" sao a historia INTEIRA dela
    contada por `rev-list main..HEAD`; ela nao pode pousar nunca. Apagar e `!` dele.
  - `wt-cert` (`0bb105db`, 05/10, 17 arquivos / 4 `.py`: `bin/`, selos de host, hooks, mensageria) e `wt-esmeril`
    (`0dfaef6f`, 30/09, `bin/esmeril_mecanico_censo*`, que NAO existe no main) sao **INSTRUMENTO**: pouso PROPRIO
    depois do produto -- **lei, L-105**.
  - `wt-lps` (`79da3e2a`, 04/10) e **ensaio descartavel pelo proprio commit** ("commit de ensaio, descartavel"): a
    coluna PROTEGE + selo JA pousaram no main por `1cde743a`, e os 2 arquivos que sobram sao `docs/LEIS.md` e
    `bin/tests/test_lei_protege_sitio.sh` de ensaio -- **lei**: coluna de `LEIS.md` so muda no commit da lei.
  - `wt-comboio` (`02261de0`, 11/10 03:17) e **copia de certificacao MINHA**, nao obra: sai na limpeza.
  - `wt-pdf` (`584265a9`, 29/09) esta no **`!` dele: "nao mergear raia-pdf"**.
  FILA (achado FORA deste marco -- ESMERIL-DO-MARCO manda virar LINHA, nao trabalho agora): `bin/raias_estado.py`
  chama "N commit a frente do `main`" o que `rev-list` conta numa historia SEM merge-base, e cobra L-105 de uma raia
  que nao tem como mergear. O rotulo certo e "historia disjunta", e o selo da L-105 nunca podera ficar verde com ela.

## MARCO 12/10 04:0x — O247: o pareador passa a ver o MESMO universo que a apuracao, e o tripwire B5-j NASCE
(**CERTIFICADA EM COMBOIO** com o O48 passo 3 -- 17 paths, UMA copia, UMA suite: a que certificou
`main+O247` nao certificava `main+O48+O247`. A 1a versao do RED furou TESTE-SEM-RELOGIO; a suite pegou.)

LEI-AKITA: origem=`ponto/turnos.py::batidas_apuraveis` (a porta ganha janela OPCIONAL -- nao nasce funcao),
testemunha=o universo de apuracao (L-025 / secao 4), RED=`test_o247_retratada_fora_do_pareamento.py` (4/4
vermelhos na crua, `logs/o247/red_comportamento_congelado.out`) + `test_tripwire_b5j_universo_de_apuracao.py`
(CRUS 3 -> 0), quem-mais-le=AST COM APELIDO, 15 chamadas de `parear_turnos` (2 por parametro, 8 copiadores
a mao com `retratada_em` explicito, 3 CRUS), juizes novos=0.

- **O CENSO ANTIGO DIZIA 2 E ESTAVA ERRADO: SAO 3.** `RELATO.md:632` e o BACKLOG falavam de
  `utils_turno_orfao.py:26` e `reconciliar_fantasmas.py:85`; o 3o esta no caminho **VIVO** da batida,
  `triagem_batida.py:162`, invisivel a censo de TEXTO -- a chamada e por APELIDO (`parear_turnos as _pt`).
- **O QUE MUDOU:** `batidas_apuraveis(colab, ini=None, fim=None)` (mesmo filtro, cada ponta solta quando
  `None`); os tres sitios passam a LER a porta; os 2 imports de `Batida` que ficaram sem uso morreram (L-111).
- **DIFF DE FROTA NA SOMBRA** (`logs/o247/DIFF_O247.md`; carimbo `dia=20261012 OK completa diverge=0`; as
  funcoes REAIS, mesma sonda nas DUAS arvores): `reconciliar_fantasmas --dry-run` **IDENTICO** (146 linhas,
  `fantasmas=0 legitimos=145 skip=0`); `tem_bloqueio` IGUAL em **82 de 82**; orfas **463 -> 440** (47 colabs).
- **A ORIGEM DE CADA LINHA DO DELTA, MEDIDA** (nao suposta -- a minha expectativa escrita caiu): das 46 que
  sairam, 37 eram a propria retratada e **9 eram entradas VIVAS que uma retratada SOMBREAVA**; das 23 que
  entraram, **23/23** tinham por proxima batida uma **S RETRATADA**. A cura iguala o universo, nos 2 sentidos.
- **O QUE O TRIPWIRE NAO VARRE** (escrito nele e na secao 4): universo recebido por PARAMETRO (2 sitios
  declarados) e caminho que nao passa pelo pareador. O vizinho `test_tripwire_bocais_apuracao.py` responde
  outra pergunta (6 arquivos NOMEADOS, regex, allowlist de contagem) e **nenhum** dos 3 curados estava nela.
- INCOMPLETO (fila): a docstring de `utils_turno_orfao.py:3` aponta `ponto/views.py:350-395`, hoje `:241`.

## MARCO 12/10 04:2x — O48 passo 3 FECHADO: os QUATRO leitores por colaborador leem a CELULA, e a linha gravada fica so com o que e ATO
`LEI-AKITA: origem=ponto/services/fechamento.py::fechamento_lido, testemunha=CelulaDia via recalcular_fechamento_mes(somente_leitura=True, retornar_objeto=True), RED=13 failures + 2 errors sobre Ran 38 (logs/o48p3/red.out) + o 6o caso evidenciado A PARTE, guarda por guarda (logs/o48p3/red_guarda_{C,A,B}_*.out), quem-mais-le=censo fechado abaixo, juizes novos=0`
O que mudou (arquivo::funcao): `ponto/views.py::painel_fechamento` (:929) e `::aprovar_colaborador` (:1396),
`inteligencia/management/commands/calcular_scores.py` (laco), `folha/services/ranking_he.py::apuracao_colab`,
`templates/ponto/partials/painel_fechamento.html` (hidden do ato + guarda do bloco DECISAO).
A LINHA segue respondendo DUAS coisas, e so elas: *ha folha nesta competencia?* (`.exists()`, sem numero) e o
ENDERECO do ato (`values_list('pk')`). O objeto lido nao tem `pk` e nunca chega ao `.save()`.
CUSTO MEDIDO na frota, pela porta unica (`bin/sonda_frota.sh`, cpuset 4-7, leitura de `saas_hasner`): universo
**531** colabs, FORMA A (por colab) **101,2 s** = 0,191 s/colab, `None=0`; FORMA B (lote por competencia)
**103,6 s** = 0,195 s/colab -- o lote **nao paga nada** e o por-colab preserva o degrau por pessoa, entao FORMA A.
Com os 30 s que o `calcular_scores` ja custa, **131 s** dentro da folga de 300 s ao vizinho das 03:30.
A REGRESSAO que a troca de chave trouxe: `{{ fechamento.pk }}` com `fechamento=None` renderizava `value=""`, e
`{{ fechamento_pk }}` com `None` imprime a PALAVRA `None` -- o form de aprovar postaria texto no lugar de um pk. As
DUAS guardas sao load-bearing, e qual carrega o que foi MEDIDO em copia descartavel, nao suposto: revertendo SO a do
template (`ponto/views.py:955` intacta) -> `Ran 6` **OK**; SO a da view -> **1 FAIL**; as duas -> **2 FAILs**, o 6o
caso incluso. A 1a e a view nao entregar objeto sem `pk`; a 2a e o `{% if fechamento_pk and ... %}` (L-082, as duas).
INCOMPLETO, dito em lista: o passo 6 FICA no gravado de proposito -- a LISTA do fechamento
(`ponto/views.py:1026`/`:1082`), `ranking_he.py:75::ranking`, `folha/export.py`, `relatorios/views.py` e
`gerar_alertas.py`. Entre este marco e o passo 6 a casa tem **lista no gravado e painel online**, e e isso que
esta escrito aqui para ninguem descobrir pela tela.
ESMERIL DO MARCO (so o que ELE deixou orfao, com prova): **zero**. As curas trocaram a FONTE do numero no mesmo
sitio; nenhuma funcao perdeu o ultimo chamador. (O censo por AST sobre os 4 arquivos devolve `marco_da_competencia`
em `ponto/services/fechamento.py:1064` com zero chamador -- achado de FORA do marco, vai para a fila, nao para a obra.)
PISTA: **certificada em COMBOIO** com o O247 -- 17 paths, UMA copia, UMA suite sobre a tree `d981f572`,
porque a que certificou `main+O48` nao certificava `main+O48+O247`. `wt-ui` (`e1449cb9`) espera o smoke dele.

## MARCO 12/10 01:1x — a ISENCAO de auth do reload das 03:30 MORRE, e a base da guarda passa a ser o CARIMBO DE PUBLICACAO

LEI-AKITA: origem=`bin/deploy.sh` (a isencao) + `bin/janela_auth.sh` (a base errada), testemunha=`logs/deploy.stamp::COMMIT` (escritor unico de "o que esta no ar", AUTO-REVERT 20/09), RED=`bin/tests/test_janela_auth.sh` secoes D e E, **4 achados** contra a arvore doente, quem-mais-le=`bin/deploy.sh` e o UNICO chamador da guarda (1 sitio), juizes novos=**0**.

ACHADO, e o caso e meu: as 00:39 a janela de auth me barrou o deploy da O44 (certo). Mas o cron das 03:30 chama `bin/deploy.sh --reload-agendado`, e esse modo se declarava **ISENTO** da janela com esta razao escrita: *"ele nao publica codigo novo"*. A premissa e **FALSA** -- o HUP publica a ARVORE VIVA, que E o bind-mount, e a arvore viva tem o `app/api/views.py` novo desde 00:39. A lei que me barrou as 00:39 ia ser atravessada as 03:30, dentro da faixa 23:20-06:00, sem ninguem olhando: **o P0 de 20/09 entrando pela porta isenta**. O `--reload-copia` dividia o buraco e publica codigo POR DESENHO.

SEGUNDO DEFEITO no mesmo sitio: no minuto em que a guarda bloqueia alguem de madrugada, ela imprimia `emergencia: bin/janela_auth.sh --forcar "<motivo>"`, e **`--forcar` NUNCA existiu neste arquivo**. A porta real e `SEM_JANELA_AUTH_MOTIVO="<motivo>" bin/deploy.sh ...`.

O QUE MUDOU (`cb3c201f`, 3 arquivos, +137/-15):
- `bin/deploy.sh:155-169` — a isencao `if [ "$AGENDADO" != 1 ]` **cai**: a guarda e chamada em TODO modo, **sem argumento**. A prosa da isencao do ENSAIO (`:134-136`), que emprestava a mesma frase falsa, foi corrigida mantendo a razao VERDADEIRA dela (as 03:30 o carimbo do dia ainda nao existe).
- `bin/janela_auth.sh:25-42,95-98` — a BASE sai de `logs/deploy.stamp::COMMIT` e o diff e contra o **DISCO** (`diff --name-only "$BASE" --`), nao `"$BASE"...HEAD`. Sem carimbo legivel, cai no ramo que ja existia (*"base nao existe -- trato como se auth tivesse mudado"*): UM escritor dessa regra.
- `bin/tests/test_janela_auth.sh` — secoes D (base pelo carimbo, costura declarada `JANELA_AUTH_STAMP`) e E (a chamada nao mora em isencao nenhuma, por PILHA de `if` em awk).

POR QUE A BASE, e nao so tirar a isencao: tirada sozinha, a guarda gritaria toda noite (o repo esta 44 commits a frente de `origin/main`), e guarda ruidosa e guarda que alguem comenta. O ruido era da PERGUNTA: *"mudou auth desde `origin/main`?"* fala do REMOTO. Ela errava nos DOIS sentidos -- **CEGA** depois do push (HEAD == origin/main, diff VAZIO, auth subindo as 03:00) e **RUIDOSA** com commits a frente que ninguem vai publicar. Com a base certa, a isencao cai E o reload de higiene das 03:30 segue correndo em toda noite sem auth pendente: o que ele perde e o direito de publicar auth na surdina.

NUMEROS: RED 4 achados (`D1` rc 1 onde esperava 0 · `D3` sem carimbo · `E` chamada dentro da isencao de `AGENDADO` · `E` base `origin/main` ditada pelo deploy). GREEN: 9 bordas + 10 sitios + D1/D2/D3 + E(2) + MORDE, **rc 0**. Pasta `bin/tests/` inteira: **so** `test_lei_akita.sh` vermelho, **pre-existente** (band-aid em `relatorios/` = 3, teto 1, 2 dos 3 sendo PROSA em `cartao_pela_celula.py:66,229`). Guarda ao vivo as 01:06: `BLOQUEADO -- antes das 06:00 (0106)`, `sitio(s) ... app/api/views.py` — **e ela bloqueia o 03:30 de hoje, que e o ponto**.

INCOMPLETO: (a) o reload de higiene das 03:30 **nao vai rodar hoje** (auth pendente), e isso e a lei funcionando, nao defeito -- `falhou()` e `echo` + `exit 1`, nada destrutivo; (b) o deploy da O44 segue DEVIDO para depois das 06:00; (c) `bin/commit_so_o_declarado.sh` aceita `-F <arquivo>` e sai **rc 0 sem commitar nada** (ele e TRIPWIRE, nao wrapper) -- perdi um ciclo com isso; vai para a fila como linha, nao como obra.

## MARCO 12/10 00:39 — a O44 POUSA (`70d159b9`): a TRANCA deixa de ser criterio do chamado e o PISO da emissao passa a ser o horizonte

LEI-AKITA: origem=`chamados/juizes.py` + `ponto/services/cartorio.py` (o `_cala_emissao`), testemunha=`REGRA_VERSOES`/`versao_para` (a forma de horizonte que a casa JA usa), RED=`app/chamados/tests/test_o44_tranca_nao_barra_o_toque.py` (614 linhas novas), quem-mais-le=7 arquivos de producao do leitor da tranca, censados antes, juizes novos=**0**.

Pelo aval `O44-POUSO-E-OS-DOIS-DOCS`, na forma da **L-107** (merge + deploy num ato so) e com a prova de arvore da L-117:
- **0** insumos conferidos: `main` e `raia-bos` nos hashes certificados, os 3 blobs de doc resolvidos iguais aos esperados, sobreposicao sujo x diff da raia **VAZIA**.
- **0b** guarda de CONTEUDO da copia (a `mtime` nao distingue reescrita identica): **3** tocados depois do T0 (`ARQUITETURA.mmd`, `MAPA.md`, `base_app.html`), **todos IGUAIS byte a byte**, **DIVERGEM EM CONTEUDO: 0**. Veredito: *"a arvore certificada e a arvore que pousa"*.
- **3** a prova que manda: `certificada = d7f17cd98118c05a450e8b43930c60e9631c8582` == `indice vivo`, **63 paths** (56 `.py`, 4 `.md`, 1 `.txt`, 1 `.mmd`, 1 `.html`).
- **5** deploy no mesmo ato: **RECUSADO pela janela de auth** (00:39, `app/api/views.py`). A recusa esta no marco acima, com o `!` no topo.

O merge levou as **2 delecoes** da L-111 declaradas (`ponto/services/tranca_do_dia.py`, `ponto/templatetags/tranca_tags.py`) com `HASNER_COMMIT_DEL=1`. PROVA de tudo: `logs/pouso_o44_20261012_003946.log` (`rc_pouso=0`). CUIDADO AO LER O COMMIT: `git show --stat 70d159b9` devolve **3** arquivos porque e MERGE -- a lista real sai de `git diff --name-only bf783df7 70d159b9` = **63**.

INCOMPLETO: (a) o **deploy** (acima); (b) o smoke em prod por `$S/smoke_o44_prod.py`, que prova o **DISCO** (`manage.py shell` e processo NOVO), logo depois do deploy; (c) a celula de ESTADO da O44 no `TICKETS.md` ainda diz **"RAIA VERDE, PRONTA PARA POUSAR -- NAO POUSADA"** e vai no marco do instrumento.

## MARCO 11/10 22:5x — o INCOMPLETO (a) de `dbd72d28` FECHADO: o lavrador do retrato nasce **ULTIMO ELO** da corrente da manha; e a corrente sai da LINHA do crontab, que tem teto MEDIDO de 998 chars

LEI-AKITA: origem=`config/crons.py` (a CASA do command, nao um leitor) e `linhas()` (o sitio que COPIAVA a declaracao para um meio com teto), testemunha=`ENCADEADOS` lido por `corrente()` + `bloco_da_manha()`, RED=`test_fora_de_pipeline_nao_esta_podre` + `test_todo_command_tem_casa` + os dois MORDE novos do teto e do INLINE (`FAILED (failures=2)`, 1059 chars em 3 linhas), quem-mais-le=`core/tests/test_selo_diagrama_do_codigo.py` (derivado de `todas_as_entradas`) e `bin/tests/test_furo_encadeado_ao_cartorio.sh` (forma de 5 args, preservada), juizes novos=0 (`corrente()` PERCORRE a autoridade, nao decide)
- A CONDICAO era ESCRITA pela propria casa e se cumpriu em letra -- *"QUANDO MEDIR: ele nasce elo da corrente da manha, DEPOIS do que fecha o espelho do dia, e sai daqui no mesmo ato"* (comentario de `FORA_DE_PIPELINE`, removido aqui). CUSTO MEDIDO na frota (prod, 20:3x, `--apply` real): **17,8 s para 8 retratos** -- emp1 1,31 · emp2 11,04 · emp3 3,77 · emp4 1,65 s. Era o unico motivo escrito para ele estar fora do pipeline.
- GATILHO `vigiar_txt_dominio`, e a dependencia nao foi copiada do vizinho, foi TRACADA: o retrato retem por
  `furo_espelho` (`folha/export.py:718`), que sai de `motivos_retencao_celula` (`:862`) lendo `CelulaDia.veredito`, cujo escritor unico `lavrar_veredito` e dirigido por `processar_cartorio` -- dai `depende=('vigiar_txt_dominio', 'processar_cartorio')`; `apurar_furos_diarios` **nao** e dependencia de dado (o retrato nao le `FuroDiario`).
- O ALARME DO GATILHO NAO DEIXA O ELO SEM COMER: o vigia alarma com `SystemExit(2)` (`vigiar_txt_dominio.py:118`) e `bin/cron_run.sh:22` CARIMBA no rc 2 por declaracao, entao o retrato se lavra TAMBEM no dia em que o TXT diverge -- o dia em que a tela do fechamento mais e olhada. `teto_s=900` e o teto do GATILHO: duracao entra em `config/crons_duracao.json` pelo MEDIDOR das 04:05 (B6), nunca se digita.
- **BUG NO CAMINHO (LEI-AKITA 6), e e o DESENHO, nao o elo**: `bin/crons.sh install` recusou o arquivo (`"-":79: command too long`) e deixou o host com o crontab VELHO -- declaracao que nao chega ao cron. O teto se MEDIU com `crontab -n` (dry run, nao instala): **998 chars entram, 999 recusa** (`MAX_COMMAND 1000` do vixie-cron menos NUL e `\n`). A causa nao e o 9o elo: `linhas()` RENDERIZAVA a corrente transitiva como TEXTO na linha do gatilho, ~98 chars por elo, **em cada empresa** -- 961 chars de comando antes, 1059 com o elo novo (61 acima). Qualquer elo futuro batia ali.
- CURA NA ORIGEM, e o atalho se recusou por escrito: dar linha propria a corrente (~810 chars) ou encurtar o texto compram FOLGA, e folga e band-aid (LEI-AKITA 1) -- pior, a linha propria trocaria o carimbo por uma APOSTA sobre a duracao da corrente inteira (hoje 06:27 -> `vigiar_txt_dominio` carimbado **06:39:50**, ~12,5 min, ~2,5 min sob um teto de 900 s). Agora a linha so **NOMEIA** a raiz (` ; bin/encadeado.sh --corrente processar_cartorio`, 58 chars, UMA vez) e quem percorre e `config/crons.py::corrente(raiz)`, lido por `manage.py crons corrente <raiz>`: a testemunha LE a autoridade (LEI-AKITA 2). O `;` nunca foi o que ordenava os elos -- os CARIMBOS ordenam; cada elo mantem flock, carimbo e teto proprios.
- EQUIVALENCIA MEDIDA, nao argumentada: a travessia INLINE de HEAD, aplicada ao MESMO `ENCADEADOS`, bate char a char com `corrente()` em **74 de 74** entradas de `CRONS` (0 divergentes), **8 de 8** elos alcancados, 0 orfao; e contra o crontab INSTALADO os **7** elos velhos dao diff **0**. Comando maior do arquivo: 961 -> **298** chars, e `crontab -n` ACEITA. CONSEQUENCIA declarada: elo novo passa a ser **DEPLOY**, nao `crons.sh install`.
- ALARME, porque corrente vazia nao pode ser silencio: `--corrente` sai **2** por DOIS caminhos, e eu medi um de cada vez. Render que falha: PROVA as 22:39 contra a arvore viva, que ainda nao conhecia o verbo -- `invalid choice: 'corrente'` no log e `rc=2 elos=0`; esse rc e do RENDER, nao da contagem. Render que volta VAZIO -- o ramo que **so o `|| N == 0`** novo pega -- medido as 23:08:10: `ALARME: corrente de raiz_que_nao_existe vazia -- rc=0 elos=0`, render saindo **0** e o processo saindo **2**. A forma de 5 args que o selo do furo exercita segue alarmando (`rc=2`, ALARME no log).
- SEM `--empresa`, de proposito (ele varre `empresas_visiveis()` numa chamada): `test_command_com_empresa_default_e_declarado_por_empresa` itera **`CRONS`**, nunca `ENCADEADOS`. E NAO declara `log=`: o campo nao tem leitor em `ENC` -- `linhas()` so o consome para `C` e `RAW`.
- DERIVACOES PROVADAS na copia (funcoes RODADAS, nao lidas): crontab **103 linhas**, elo em **3** ocorrencias (uma por empresa), bloco da manha 72 -> **73**, ordem terminando `398 vigiar_txt_dominio -> 399 lavrar_retrato_folha`, `papel = lavra`.
- DIAGRAMA no MESMO commit (precedente `10de79cb`): `docs/ARQUITETURA.mmd` regenerado -- **59 -> 60 nos diarios**, +2 arestas (`vigia -> retrato`, `cartorio -> retrato`), `arestas depende: 44 -> 46`; `docs/MAPA.md` saiu BYTE A BYTE igual (o diagrama le `ENCADEADOS`/`todas_as_entradas`, nao `linhas()`). `docs/CRONS.md:41` deixou de chamar o vigia de "ultimo elo".
- ESMERIL DO MARCO: zero orfao -- as 24 linhas do percurso INLINE viraram o corpo de `corrente()` (movidas, nao duplicadas: `grep -c 'encadeado.sh %s'` em `linhas()` = **0**), `_por_empresa_da_raiz` e lookup e so se chama se houver elo, e **0 `def`/`class` removida**.
- PROVA: ruff limpo nos 4 `.py`; selo do teto `Ran 6 tests` / `OK` rc 0 (era `FAILED (failures=2)`); 31 modulos de crons/diagrama/retrato `Ran 235` / `OK (skipped=10)`; suite CHEIA na copia `Ran 10445 tests in 1413.740s` / `OK (skipped=42)`, rc 0, `FAIL:`/`ERROR:` = 0; `crons.sh install` instalou **103** linhas (backup `logs/crontab_backup_20261011_230808.txt`) com a corrente nomeada em **3** linhas, `encadeado.sh` em **3**, elo INLINE sobrevivente **0** e maior comando **299** chars (teto medido 998), e o `check` diz crontab == `config/crons.py`; `bin/tests/test_furo_encadeado_ao_cartorio.sh` **OK** rc 0 na arvore VIVA (carimbo, sem minuto proprio, ordem garantida, teto que alarma). O `bin/deploy.sh --sem-migrate` e o ato SEGUINTE a este commit -- a prova dele vai no handoff da L-108, nao aqui.
- O ELO RODOU EM PROD HOJE, e o "caminho feliz" que eu ia medir foi sabotado pela MINHA PROPRIA prova 1 -- digo os TRES comportamentos em vez de um rotulo: `--corrente processar_cartorio` as 23:08:11 achou **8 elo(s)**; o elo 1 esperou os 900 s e ALARMOU as 23:23:11 (`faltaram 3 carimbo(s) de processar_cartorio`) porque a prova 1 apagou os carimbos REAIS de hoje (achado abaixo); os elos 2-7 sairam **0** pelo carimbo proprio do dia (idempotencia de `encadeado.sh:75`); e o elo **8**, o novo, correu de ponta a ponta -- gravou **8 retratos** (emp2 10/2026 total=445 entram=129 bloqueado=280) e carimbou `logs/fim/lavrar_retrato_folha.-.20261011` as 23:23:25. O rc da corrente e **2**, o pior dos elos, como ela promete. Amanha 06:3x e a corrida sem minha mao.
- **ACHADO, e e o meu caso (LEI-AKITA 6)**: `bin/tests/test_furo_encadeado_ao_cartorio.sh:18` resolve `$R` pela raiz VIVA e `rm -f` nos carimbos de PRODUCAO -- `logs/fim/processar_cartorio.{2,3,4}.$DIA` e `apurar_furos_diarios.-.$DIA` -- na entrada E no `trap EXIT`. `app/logs/placar.jsonl` diz que os dois rodaram HOJE com `exit=0` (cartorio n=3, apurar n=1) e `bin/cron_run.sh` carimba no rc 0, logo os carimbos existiram e foram APAGADOS: dos dois nomes sobrevivem no disco so `20261002` e `20261006`. Um SELO DE HOST escrevendo no estado de prod tira a porta da corrente: o dependente ALARMA (foi o que eu acabei de medir) e a guarda de idempotencia para de pular. CANDIDATO, nao provado: `apurar_furos_diarios` rodou **3x em 10/10** (06:31:54/06:32:01/06:32:03, dur 27 s cada) contra 1x nos outros dias, e TRES largadas em 9 s e a assinatura do LOCK apagado, nao do carimbo: `flock -n` (`:72`) vem ANTES da guarda de idempotencia (`:75`), logo inode novo = as tres empresas passam juntas. CURA na origem = raiz **e lock** por env com default vivo em `bin/encadeado.sh` -- o `flock` mora em `/tmp/encadeado_$CMD.lock` (`:71`), FORA de `$R`, entao override de raiz SO nao basta --, e o selo exercita em diretorio temporario; `bin/` e INSTRUMENTO, **pouso proprio** (L-105), com RED que planta carimbo antes e confere depois. Censo em `logs/achados/selo_apaga_carimbo_de_prod_20261011.md`.
- INCOMPLETO: o smoke de clique do Ronald (`O48-RETRATO-DA-FOLHA-SMOKE`) segue segurando o push, e minha prova 2 afirmou `NF != 5 esperado VAZIO` com a expectativa ERRADA -- a forma do elo e `cmd gatilho emps teto [args]`, entao **4** campos (sem args) e **6** (com dois) sao legitimos; as duas linhas que ela acusou rodaram certas.
- FILA (achados FORA do marco -- ESMERIL-DO-MARCO manda virar LINHA): (a) os 6 elos vizinhos declaram `log=` que ninguem le, e `logs/fio_mudo.log` / `logs/perguntas_orfas.log` estao parados em **29/09**; (b) `bin/crons.sh install` devolve **rc 0** mesmo quando o `crontab -` recusa o arquivo (`echo | crontab - && echo; exit 0`) -- foi por isso que a recusa de 22:03 passou por sucesso; instrumento, pouso proprio pela L-105; (c) `bin/tests/test_lei_akita.sh` esta VERMELHO na arvore VIVA desde antes deste marco (band-aid em `relatorios/` = 3, teto 1) e **2 dos 3 hits sao PROSA que explica a cura** (`cartao_pela_celula.py:66,229` dizem "NAO e o fallback") -- selo que varre TEXTO morde o comentario; ele barra o proximo push.

## ALARMES DA ESTEIRA

**12/10 03:40 vigia da esteira** -- esteira em espera de janela: 8 fatias prontas, reabre 04:45.

**11/10 22:40 vigia da esteira** -- esteira em espera de janela: 8 fatias prontas, reabre 00:00.

## MARCO 11/10 20:1x — UI-FECHAMENTO-NOVA etapa 1 (complemento do passo 6 da O48): a tela do fechamento LE um retrato GRAVADO, e o badge e o len da MESMA lista que a tabela mostra

LEI-AKITA: origem=folha/fora_da_folha.py::fora_da_folha (le folha/export.py::classificar_export), testemunha=o retrato GRAVADO (folha/models.py::RetratoFolha.linhas), RED=folha/tests/test_retrato_folha_contador.py + test_retrato_folha_smoke_chromium.py, quem-mais-le=censo por AST em folha/tests/test_retrato_folha_um_derivador.py, juizes novos=0
- Tela nova FORA do menu (`folha/urls.py::retrato_folha`) responde "quem entra na folha desta competencia,
  quem esta bloqueado, e por que?" LENDO retrato GRAVADO, sem calcular ao abrir (PROVA:
  `test_MORDE_a_tela_NAO_calcula_ao_abrir`, as duas fontes patcheadas, `call_count == 0`).
- `fora_da_folha` ganhou `com_itens=False` ADITIVO (PROVA: `test_com_itens_False_devolve_EXATAMENTE_o_que_
  devolvia`); `itens` sai da MESMA chamada de `classificar_export`: 0 derivador novo de quem entra (AST).
- `folha/models.py::RetratoFolha` (molde `core.PlacarSituacional`, unico por empresa+mes+ano), ESCRITOR
  UNICO `manage.py lavrar_retrato_folha --apply` (PROVA: `LavradorEscritorUnicoTest`, 2x = mesmo estado).
- `folha/retrato_folha.py::linhas_do_retrato` e a UNICA derivacao: badge e tabela sao a MESMA chamada
  (L1 -- PROVA: badge lido da PAGINA == `html.count('data-retrato-linha=')`).
- COLUNA "dias que seguram" NASCEU porque a fonte ENTREGA (`motivos_retencao_celula(com_dias=True)`, o
  juiz do export), e so para retido por `furo_espelho`; nos outros 5 motivos sai VAZIA, nunca zero.
- Estilo nao se moveu (O122): `divergencias: 0 de 14 elementos`; o selo do O122 so GANHOU caso (3 que
  MORDEM, 48 linhas, ZERO removida) e o include do icone na pilula e GUARDADO por `{% if it.icone %}`.
  Smoke headless 1920 / 1366x768 / 1024x768: `4 == pilulas`, `2 == icones`, `pag <= win`, console limpo.
- ESMERIL DO MARCO: zero orfao, medido no diff -- as 3 linhas de `.py` removidas sao assinatura, return e
  import SUBSTITUIDOS no lugar; mapa de icone 9 -> 11 chaves, ZERO perdida (na arvore MERGEADA).
- RE-CERTIFICADA em COPIA de `main` + `raia-ui` (mount conferido por `docker inspect`), porque a bateria
  da propria raia certificou arvore 266 commits atras do main: `Ran 10443 tests in 1414.303s` / `OK (skipped=42)`, rc 0. POUSO-EM-COMBOIO medido:
  `wt-bos` 0 commit a frente, ~60 arquivos sujos, agente no meio da O44 -- **a wt-ui roda SOZINHA**.
- INCOMPLETO: o CRON do lavrador (`config/crons.py::FORA_DE_PIPELINE`, custo na frota nao medido -- e ele
  nao e a unica linha de nucleo: `folha/models.py`, a migration `0011`, `fora_da_folha.py`, `folha/urls.py`
  e o command tambem sao) e o smoke de clique do Ronald.

## MARCO 11/10 19:0x — O48 passo 3, O VINCULO NAO E SILENCIO: a abstencao do previsto se mede DENTRO do vinculo, e a medicao escolheu o corte ESTREITO

LEI-AKITA: origem=`ponto/services/fechamento.py:584` (o predicado da abstencao, nao um leitor), testemunha=
`grade_do_fechamento` -> `leitor_celula.py::grade_da_celula` (a MESMA autoridade de ontem), RED=
`test_o48_sem_celula_nao_e_zero::test_MORDE_admitido_no_meio_sem_celula_no_vinculo_tambem_nao_diz_zero`,
quem-mais-le=5 sitios e TODOS tratam `None` (`folha/export.py:791-824` -- `:822-823` PREFERE `horas_trabalhadas`
quando o realizado e None --, `cartao_pela_celula.py:81-85`, `e6_piso_autopsia.py:95-125`, `dia_pago.py:406-408`
le o dict); aritmetica sem guarda **0**; juizes novos **0** (porta por porta, escritor do `None` unico).

**A CURA.** `if _dias_g and all(tipo_dia=='sem_celula')` -> o mesmo `all` sobre `_dentro = [d for d in _dias_g
if tipo_dia != 'fora_vinculo']`. Sem ninguem fora do vinculo `_dentro == _dias_g`: nao e porta nova, e a de
ontem dita no universo certo. MEDIDO EM PROD, LEITURA, 3 redacoes no MESMO universo (`logs/o48p3_predicado_20261011.out`):

| comp | universo | HOJE | DENTRO | LARGO (`not _dt_trab`) | DENTRO-HOJE | valor que a cura MOVE |
|---|---|---|---|---|---|---|
| 10/2026 | 587 | 10 | 10 | 41 | **0** | 0 |
| 09/2026 | 607 | 9 | 14 | 51 | 5 | 0 |
| 08/2026 | 618 | 7 | 18 | 77 | 11 | 0 |
| 07/2026 | 629 | 490 | 544 | 615 | 54 | 0 |

**O CORTE LARGO FOI MEDIDO E RECUSADO**, e e o 2o caso novo: `not _dt_trab` silenciaria na 10 **31 colabs que a
grade RESPONDEU** (`fora_vinculo` e resposta -- "nao havia vinculo, previsto 0" --, nao silencio), quatro com
previsto GRAVADO (col316/col846/col870/col927: 14.400/9.900/10.800/9.900 min). Os +70 que a cura alcanca estao
TODOS com 0 hoje: na competencia viva o delta e **zero**. E por isso que o DIFF do passo 4 disse "10: zero" sem
mentir -- gravado x lido NAO VE o balde v (os dois lados leem a MESMA ata), e `horas_trabalhadas` vem do motor.

## MARCO 11/10 19:0x — O48, a Pauta DP dos colabs com horas e zero realizado: CONTRATO DE ENTRADA, e a medicao PARTIU o balde de 22 em seis -- com 5 CERTOS

O aval manda "os colabs com horas e zero hora realizada no gravado das exportadas (6, 14 e 2) saem como Pauta DP com
os dois numeros, sem apply". Antes de existir arquivo, o contrato -- e a pergunta zero ja tinha resposta na casa.

**CONTRATO DE ENTRADA.** FONTE: `FechamentoMensal.causa_espelho['causa']`, gravado pelo escritor unico
(`ponto/services/fechamento.py:619`) a partir do juiz unico da causa do furo (`folha/causas.py::classificar_causa`) --
a Pauta LE o rotulo, **nao** monta `filter(horas_trabalhadas__gt=0, minutos_realizados=0)` proprio (seria 2o derivador,
LEI-AKITA 8). UNIDADE: colaborador x competencia. UNIVERSO: o GRAVADO das competencias exportadas 07, 08 e 09/2026
(629 / 618 / 607). EXCLUSOES: `causa='sem_furo'` -- e e aqui que a medicao morde.

**O BALDE DE 22 NAO E UMA DOENCA, SAO SEIS -- e 5 dos 22 estao CERTOS** (LEITURA em prod, rc 0,
`logs/o48p3_pauta_causa_20261011.out`): o juiz ja rotula 16 dos 22, com `sem_furo` **5**, `sem_escala` **5**,
`dias_trocados` **2**, `escala_fase_deslocada` **2**, `sem_previsao_escala` **1**, `falta_real` **1**. Os **5
`sem_furo` dizem `isento de ponto (Art.62)`** (col41, col128, col181, col440, col642): isento tem hora de batida e
nao tem previsto nem realizado **por lei** -- nao e divergencia, e o desenho. Eles SAEM da Pauta, e o balde do aval
cai de 22 para **17**.

**OS 6 DA 07 ESTAO SEM ROTULO** (`causa=None` nos seis: col34, col41, col79, col142, col753, col754) -- a foto deles
e anterior ao campo do plano de acao, e esse e o `gravado_velho` com nome: nao e defeito de derivacao, e foto que
ninguem re-lavrou. Eles entram na Pauta como **"sem causa gravada"**, nunca com causa inventada na hora.

Por L-099 o dono dos 11 restantes e **CADASTRO** ou **BATIDA** -- **proibido curar por codigo**, que e o que o aval
pediu. Molde: `ponto/services/pautas_dos_deslogados.py` (`sincronizar(apply=)`, MARCADOR, ancora `colab`, fecha por
`marcar_feita_pelo_sistema`). INCOMPLETO: o servico nao existe -- entra depois do pouso do passo 3.

## MARCO 11/10 18:4x — O44 itens 2-8: a TRANCA deixa de ser CRITERIO, e o escopo do silencio AUTOMATICO passa a ser um PISO DE DATA declarado, de onde o TOQUE do admin sai

LEI-AKITA: origem=os 9 sitios do grupo A do censo do item 1 (nao os leitores) + `ponto/services/cartorio.py::PISO_EMISSAO`, testemunha=`ponto/janelas.py::janela_atual` + a celula (`escala/models.py::CelulaDia`), RED=`chamados/tests/test_o44_tranca_nao_barra_o_toque.py` (`logs/o44/red_selo_20261011_172309.txt`: `Ran 9 tests`/`FAILED (failures=7)`/rc=1; o do PISO em `logs/o44/red_piso_20261011_202033.txt`: `Ran 11 tests`/`FAILED (failures=3, errors=1)`/rc=1, com os vizinhos em `logs/o44/viz_piso_20261011_202449.txt`: `Ran 151 tests`/`FAILED (failures=2)`), quem-mais-le=censo do item 1 (6 juizes + 22 consumidores, `docs/RELATO-ARQUIVO.md`), juizes novos=**0** (`::emissao_no_horizonte` e LEITOR PURO na categoria do `::versao_para` -- nenhuma chave nova em `core/juizes.py`, TRAVA JUIZ-NOVO intacta).

**A LEI (c) GANHOU HORIZONTE** -- aval Ronald 11/10 19:5x, literal: *"a emissao passa a ter piso de data, 2026-09, pela mesma forma do REGRA_VERSOES do cartorio. Os 1.390 dia-colab calados em competencia trancada ficam abaixo do piso por decisao escrita; so o dia tocado pelo admin emite. Nenhum chamado nasce em lote."* Entao a tranca deixa de ser criterio de QUALQUER COISA no cartorio (o import de `chamados.juizes` saiu do modulo) e quem recorta o silencio automatico e `ponto/services/cartorio.py::PISO_EMISSAO = '2026-09'` -- constante + docstring que diz de que pergunta ela e + assert de formato --, lido pelo leitor puro `::emissao_no_horizonte`, que deriva a COMPETENCIA por `janela_atual` e nao o mes civil. **ENCAIXE decidido pela PAREI-SO-LEI, registrado aqui e seguido**: o piso entra no MESMO sitio e com a MESMA escapatoria de escopo -- `fora do horizonte` cala a VARREDURA e **nao** cala o TOQUE --, o discriminador segue sendo `toque=` e **nao** `forcar=` (que tem 6 chamadores de producao, 2 deles crons, e rejulga com a mesma impressao), e o piso **nao** entra na `impressao_insumos`, de proposito: piso dentro do hash re-julgaria a frota, que e o lote que o aval proibe. A pergunta de lei (3) do topo esta **RESPONDIDA** por este aval.

**O que mudou** -- 23 arquivos de producao, 2 APAGADOS pela **L-111** (zero chamador: `ponto/services/tranca_do_dia.py`, `ponto/templatetags/tranca_tags.py`, com os 2 testes deles), 1 template (`_calendario_grade.html`: sai o ramo `dia-trancado` e o `{% load %}` da library apagada -- tela que recusasse o que a porta aceita seria LEI-AKITA 2 ao contrario):
- `ponto/services/cartorio.py` -- nascem `PISO_EMISSAO` e `::emissao_no_horizonte`; `toque=False` atravessa `::julgar_colab` / `::_julgar_colab_corpo` / `::julgar_celula`; e o `from chamados import juizes` SAI (tinha 1 chamador) -- selo `test_o_cartorio_nao_le_mais_a_TRANCA`. Os 2 leitores que FICAM com a tranca por `PERMITIDO` foram MEDIDOS: `escala/services/furos_diarios.py::apurar` (:51-70) e `ponto/services/furos_sem_cobranca.py::medir` (:50) so rodam na competencia ABERTA (+ a anterior nao trancada), logo >= piso por construcao e para sempre -- o piso nao os alcanca e nao se estende a eles.
- as 9 recusas do grupo A caem: `chamados/services/cobrar_dia.py`, `::validacao.py`, `::regularizacao_ext.py`, `::materializacao.py`, `ponto/services/ausencia.py`, `::justificativa.py`, `::regularizacao.py`, `::checar_ausencia.py`, `colaboradores/services/vinculo.py` (so a recusa de tranca; o **PISO** de retroatividade FICA, caso 6 do aval).
- `chamados/catalogo/recusa.py` -- a regra `COMPETENCIA_APROVADA` **FICA e foi re-escrita da fonte VIVA**: a recusa dela nunca foi da tranca, e do chokepoint de escrita (`ponto/registro_batida.py:94` -> `ponto/janelas.py::competencia_fechada`), cujo texto ainda chega a `traduzir_recusa` por `materializacao.py:707` e `:792`. Apaga-la era regressao P14 (`OUTRO` + texto cru na tela do admin); o RED era meu, nao do teste.
- **A PAUTA DP DO DIA MUDOU DE ENDERECO, nao morreu.** O botao *Abrir Pauta DP* morava DENTRO do ramo `dia-trancado` e teria ido com ele: medido, `hxPautasAbrir('dp', {colab, data})` so existia ali (`base.html:275` abre a gaveta generica, sem ancora de dia), logo era a UNICA porta de Pauta DP ancorada em DIA (P7.1, corte 18/09, selo `pautas/tests/test_pauta_do_dia.py`). Ela passa para a MESMA fileira do *Resolver dia*, nos mesmos dias-problema, **sem derivacao de tranca no template** -- por **CURA-MAIS-RESTRITIVA**: entre tirar uma porta que o aval nao mandou tirar e deixa-la em todo dia-problema, deixa.
- `core/juizes.py::PENDENTES['fechamento'][_K6]` -- a divisao com `chamados/juizes.py::dia_em_competencia_exportada` sai porque a funcao foi apagada: a lista **13 -> 12**.

**Numeros MEDIDOS** (nenhum de cabeca): selo novo **9/9 OK rc=0** (`logs/o44/verde_selo_20261011_174205.txt`, `Ran 9 tests in 3.548s`) · **22** casos de caracterizacao INVERTIDOS (renome com a asserçao trocada; **0** sem par, em 16 arquivos), **14** corpos reescritos sem renomear e **14** casos que foram com os 2 arquivos de teste apagados, em **29** arquivos de teste no escopo -- medido por AST contra o HEAD da raia, nao contado a mao · os 26 modulos afetados **`Ran 220 tests` / `OK` / rc=0** (`logs/o44/afetados_20261011_183206.txt`) · a pergunta de lei re-medida na sombra fecha a leitura (a): **633 de 634** dia-colab que a varredura perguntaria estao tambem em competencia EXPORTADA (topo do RELATO, item 5) · selo do PISO **14/14 OK rc=0** (`logs/o44/verde_piso_20261011_202230.txt`) e vizinhos **`Ran 55 tests` / `OK` / rc=0** depois da inversao (`logs/o44/viz_piso2_20261011_202622.txt`) · **o ensaio do piso na SOMBRA** (`SOMBRA_ARVORE` na raia, so leitura, `assert NAME=='sombra'`): a varredura REAL (`processar_cartorio` DRY, emp 2/3/4) emite **0 antes e 0 depois** (`logs/o44/sombra_runA_antes_20261011.txt`, `logs/o44/sombra_runB_piso_20261011.txt`) -- o numero de HOJE e ZERO, nao 1.390 --, e as **5** linhas de `PeriodoFechado` vivas (07 e 08/2026) estao TODAS abaixo do piso, logo "trancada E >= piso" e vazio por estrutura · censo de predicado: **passa_a_EMITIR=0** (nao ha lote, estruturalmente) e **passa_a_CALAR=16.366** dia-colab (emp2 16.337, emp3 2, emp4 27), que e TETO DE CELULA e nao de emissao (a varredura pula 104.275 celulas por impressao igual) · os quatro numeros NAO sao o mesmo numero: **1.390** (chamados arquivados pela tranca) ≠ **634** (censo da pergunta) ≠ **0** (emissao real de hoje) ≠ **16.366** (teto de celula) · defeito de SONDA escrito em `logs/o44/plano_piso.md`: a RUN A ancorou a data no FIM da linha e imprimiu `FURO_PARCIAL >=piso=0` onde o proprio `[DRY] fim:` dizia `furo_parcial=40` -- corrigido na RUN B, e o 0 da RUN A nao se le como medicao.

**L-050 passa de PELA-METADE a INTEIRA** (`docs/LEIS.md:91`): 1 de 1 clausula vigente com teste que morde, e a 2a clausula do texto esta SUPERADA pelo proprio aval de origem -- a funcao que "decidia" foi apagada.

**INCOMPLETO, em lista** (nada disto se maquia):
1. **2 dos 9 sitios do grupo A ficam** por **CURA-MAIS-RESTRITIVA**: `ponto/services/flip_batida.py:22` (consumido pelo cron `flip_automatico` 07:12, que e VARREDURA) e `ponto/registro_batida.py:94` (chokepoint de escrita). Os dois leem `competencia_fechada` (FechamentoMensal aprovado/exportado), **nao** a tranca. Sai quando houver corte sobre VARREDURA em dia trancado.
2. **O caso 1 do aval nao foi repetido em PROD pela tela** -- a raia nao pousou, e o pouso tem **1 dono** (ordem 11/10). Sem isso o PRONTO do aval nao fecha.
3. **Sem DIFF de frota e sem reversao em `logs/`**: esta fatia nao move dinheiro (nenhum campo de `FechamentoMensal` escrito), mas o recalculo que o toque DESTRAVA move -- e isso se mede no pouso, nao aqui.
4. O template mexido e partial de pagina, **nao** base: a FRONT SEM SMOKE nao morde pela letra. Fica DITO para quem pousa decidir o smoke de clique.
5. **O PISO e CONSTANTE no codigo**, no molde declarado do `REGRA_VERSOES` -- e o aval o fixou literalmente --, mas o censo de `core/datas_cravadas.py` **nao o ve**: as 3 regex dele exigem `AAAA-MM-DD` e o piso e `AAAA-MM` (o `REGRA_VERSOES` ja era invisivel pela mesma razao, medido hoje, selo `test_selo_sem_data_cravada` VERDE). Nao e risco de vencimento silencioso -- piso de acervo no passado so fica mais passado --, mas o selo nao cobre esta forma. Alargar a regex e INSTRUMENTO, logo pouso proprio (**L-105**): fica como linha de fila, nao como cura agora.

## MARCO 11/10 17:4x — O250 RAIA-COM-AGENTE-DECLARADO: o ESTADO passa a ter **uma linha por raia**, e "agente vivo" e DECLARACAO, nunca `pgrep`

`LEI-AKITA: origem=bin/raias_estado.py (o medidor) + bin/raia_agente.sh (o escritor unico), testemunha=a declaracao em logs/raias/<raia>.agente mais o git da propria worktree, RED=bin/tests/test_raia_declarada_no_estado.sh (repo git de verdade, worktree 1 commit a frente: sem declaracao -> SEM AGENTE/sem_agente=1; com declaracao -> agente vivo/sem_agente=0), quem-mais-le=censo fechado, grep de logs/raias em bin/ = 0 fora do escritor/medidor; juizes novos=0 -- medidor de HOST no molde de bin/fabrica_estado.py, nada registrado em core/juizes.py`

Item 4 do aval **RAIAS-ANDAM-JA**, e ele nao e obra nova: e a divida que a propria ficha da **L-105** nomeava em `LEIS.md:131` -- *"nao achei contador de raia em `bin/gerar_estado.py`"*. O CLAUDE.md a punha depois da CELULA-TURNO-FECHA e da O145; o aval a antecipa.

- **O que mudou** (3 arquivos novos + 1 patch): `bin/raia_agente.sh` (ESCRITOR UNICO: `abrir`/`falta`/`fechar`/`listar`, trilha em `logs/raias/historico.log`, recusa segunda obra na mesma raia -- *"um por vez ate o POUSO"* --, e `fechar` grava a declaracao INTEIRA antes do `rm`) · `bin/raias_estado.py::ler/linha/linha_sujas` (MEDIDOR, universo = UNIAO medida de declarada + a frente do `main` + suja) · `bin/tests/test_raia_declarada_no_estado.sh` (selo de host) · `bin/gerar_estado.py::gerar` ganha a secao `## Raias` **importando o medidor**, do mesmo jeito que ja importa `fabrica_estado` -- nenhuma derivacao nova dentro do gerador.
- **Por que declaracao e nao processo.** `pgrep` esta proibido duas vezes nesta casa (GATE TEMPORAL 16/09; e o caso em que o `pgrep -f` casou a propria linha do monitor), e um agente de sessao **nao e um processo que o host veja**. Molde: `bin/pausar.sh`, que ja faz isso para a esteira. **PROVA:** o selo morde nas duas direcoes -- gerador que deriva sozinho -> `VERMELHO ... deriva raia por conta propria`; medidor cego a declaracao -> `VERMELHO ... as duas leituras deram a MESMA saida`.
- **Numeros medidos na arvore viva, 11/10 17:3x** (`python3 bin/raias_estado.py --linhas`): uniao **30**; **9** raias de verdade (declarada ou com commit a frente); **21** copias sujas sem commit, dobradas em UMA linha contada; `sem_agente=7`; `velhas=6` (teto 6 h da ficha da L-105). A dobra nasceu de medicao, nao de gosto: publicar as 30 iguais eram ~90 linhas para esconder as 2 que trabalham.
- **As duas raias vivas estao DECLARADAS** (item 1 e 2 do aval), com o `DESDE` medido por `stat -c %w` no arquivo da task e conferido contra `date -d` (`2026-10-11T17:11:52-03:00`, os dois): `wt-bos` = O44 itens 2-8, `wt-ui` = UI-FECHAMENTO-NOVA etapa 1. **FALTA de cada uma**: suite verde na copia + commit na propria branch; **o POUSO e meu** (um por vez, L-105/L-107) -- e eu revoguei por mensagem a autoridade de pouso que os dois prompts davam, porque `bin/pista_lock.sh:11` diz que a fila de pouso nao se automatiza e dois merges no mesmo bind-mount e o apagao de 30/09.
- **Item 4, segunda frase -- o motivo de cada raia SEM AGENTE** (7, medido): `wt-o48p3` (`e14e653f`, 3 h) -- **`!`**, esta atras do aval `O48-PASSO5-CHAVE-COM-O-DIFF-LIDO` · `wt-pdf` (`584265a9`, 12 d) -- **lei**, nao-mergear declarado · `wt-comboio` (`02261de0`, 14 h) -- **pista**, copia minha de comboio, limpeza na fila · `wt-cert` (`0bb105db`, 6 d, O194) -- **pista**, instrumento sem pouso aberto · `wt-esmeril` (`0dfaef6f`, 11 d) -- **pista**, celula "editada" sem escritor, obra registrada · `wt-lps` (`79da3e2a`, 7 d) -- **pista**, commit de ensaio descartavel · `recon_onprem` (`612be166`, 3 meses, 1053 commits) -- **pista**, nao e raia desta esteira.
- **Selos:** pasta de host **0 vermelho de 68** na arvore viva; na COPIA do HEAD com os 4 arquivos, **11 de 69** vermelhos e **nenhum** acusa arquivo novo -- todos da classe sem-ambiente (`git archive` nao leva `.git`, `logs/`, hooks nem `settings.json`), inclusive o `test_lei_protege_sitio.sh` que fica verde no vivo. `bin/selo_espera_por_processo.sh` passou: ele ignora comentario puro, e as duas unicas mencoes a `pgrep` nos arquivos novos sao comentario que escreve a propria lei.
- **INCOMPLETO:** o `ESTADO.md` so passa a publicar a secao quando este commit entrar -- o timer de 5 min (`bin/relato.sh`) le o gerador do disco, e `bin/` nao e bind-mount, entao **nao ha deploy neste pouso**. Pousa AGORA e nao atras das raias, de proposito: SUITE-UMA-VEZ-POR-ARVORE proibe `.py` novo no `main` entre o veredito e o merge de uma raia, e nenhuma das duas chegou a suite. O `bin/simular_folha.sh --lido` **fica para o pouso seguinte**: ele arrasta a remocao do `--recalcular` em `simular_folha.py` (L-111, mesmo marco) e isso e codigo de app -- suite e deploy.

## MARCO 11/10 17:0x — O48 passo 4 -- o DIFF de folha passa a LER a competencia, e por isso passa a poder rodar em PROD

LEI-AKITA: origem=`folha/management/commands/simular_folha.py::instantaneo`, testemunha=`ponto/services/fechamento.py::recalcular_fechamento_mes(somente_leitura=True, retornar_objeto=True)`, RED=`folha/tests/test_o48_passo4_simular_folha_lido.py` (11 casos), quem-mais-le=censo abaixo, juizes novos=0.

**O que mudou** (3 arquivos, 0 juiz novo, 0 derivador novo):
- `folha/export.py::montar_linhas_export` e `::classificar_export` ganham `fechamentos=` -- os objetos JA EM MAO no lugar da consulta ao gravado. `None` (todo chamador de producao) le o gravado **byte-identico**. A bolha segue com o `@invariante nao escreve nada e nao RECALCULA`: quem deriva e o CHAMADOR.
- `folha/export.py::STATUS_NO_TXT` -- o filtro de status era literal dentro do `filter(status__in=(...))`; objeto destacado nao tem pk e nao se filtra por queryset, entao o ramo injetado filtraria em Python. Dois literais iguais seriam duas regras que so hoje concordam: UM nome, lido pelos dois ramos.
- `simular_folha.py`: `--lido`, `_retidos(fechamentos=)`, `'autoridade'` na foto e a RECUSA de autoridade cruzada no `comparar`.

**Por que isso nao e cosmetico**: `--recalcular` **grava a foto para poder medi-la**, e por isso a medida inteira estava presa a `exigir_banco_de_ensaio`. `--lido` nao encosta no gravado (o objeto nunca tem `pk`), entao a MESMA medida roda em **producao, em leitura**. O custo segue sendo o motor por colaborador: lugar dela e o cpuset de teste (vCPU 4-7) ou a sombra, **nunca `docker exec saas_core`**.

**RED, em escada** (PROVA `logs/o48p4/red1..red4*.log`, os quatro degraus contra o MESMO arquivo de teste, remedidos depois da cura da assercao vazia): HEAD `541ce443` `Ran 11`/`FAILED (failures=2, errors=7)` -> com a cura `Ran 11`/`OK` -> **anti-vacuidade 1**, costura desfeita em `montar_linhas_export` (`fechamentos = None`), `FAILED (failures=4)`, os 4 casos de COMPORTAMENTO vermelhos e os de API verdes -> **anti-vacuidade 2**, costura desfeita DENTRO de `_retidos` (a meia-correcao que o docstring proibe), `FAILED (failures=1)`, e o caso que morde e o (a): `retidos[...]['horas_extras_50']` **`3.00` contra `99.00`**, o retido entrando com a foto velha enquanto a linha do vizinho sai com a nova.

**Achado do proprio review, curado no mesmo ato**: a assercao dos `retidos` do caso (a) era **VAZIA** (`{} == {}` -- um colaborador so, e ele ENTRA no TXT). Nasceu o colaborador RETIDO e a costura 2 e a prova. Historia no corpo do commit.

**PROVA de frota na sombra** (`banco=sombra`, `hoje=2026-10-11`, PROVA `logs/o48p4_prova_lido_20261011.out`, rc 0):
- **nao escreve**: carimbo ANTES == DEPOIS (`FechamentoMensal=3804`, maior `atualizado_em` 14:04:00.879294+00:00, `DiaPago=32749`) -> `ESCREVEU=NAO`, e `carimbo final: NADA ESCRITO` depois de SEIS leituras de frota (cinco em `--lido`). **O TXT nao muda de tamanho**: 10/2026 `linhas_lido`/`linhas_gravado` = emp1 0/0, emp2 **136/136**, emp3 **67/67**, emp4 **5/5**.
- **+7 retidos no lido** (320/317, 65/63, 16/14): o universo do lido e o do vinculo na janela, nao o das linhas gravadas -- quem **nunca teve foto** aparece como retido e **some do TXT em vez de entrar nele**. O proprio codigo ja dizia isso (`simular_folha.py:124-131`); agora tem numero. Fila: nomear os 7.
- **idempotente** `lido x lido` = `DIFF=0`; **autoridade cruzada RECUSADA** `gravado x lido` = `DIFF=4`, uma linha `AUTORIDADE DIFERENTE` por empresa (esse DIFF e o do passo 5, que esta no `!`). **Custo**: lido 112,9 s x gravado 3,6 s nas 4 empresas; 09/2026 310 linhas/394 retidos, 08/2026 382/389, 07/2026 8/344 -- tudo sem escrever.

**Censo de quem mais le** (`fechamentos=` com default `None`): ~50 chamadores de `classificar_export` e 12 de `montar_linhas_export`, **todos no default**; 1 chamador de `--recalcular` (`bin/simular_folha.sh:70`), que e por isso que `--recalcular` **nao e orfao** e NAO morre neste pouso -- ele troca para `--lido` e morre no pouso do INSTRUMENTO (L-105/L-111), em commit proprio.

**Vizinhos**: `Ran 217`/`OK (skipped=3)` (20 modulos). **Ruff**: `All checks passed` -- um `B904` nasceu no `raise CommandError` e foi curado com `from e`. **Suite cheia**: `Ran 10401 tests in 1406.217s` / `OK (skipped=42)`, rc 0, 0 `FAILED`/`ERROR` no log. **Comboio** (L-117): roda **sozinha** -- `wt-bos` esta com 15 arquivos de codigo em pe no meio da O44 e `wt-o48p3` esta gated pelo mesmo `!` do passo 5. **Portao do deploy**: `sombra.sh --conferir` = `dia=20261011 status=OK tipo=completa diverge=0 erros=0`.

INCOMPLETO: (1) o docstring de `relatorios/tests/test_cartao_pela_celula.py:1-9` segue com os nomes velhos -- a inversao vive so no `e14e653f`, gated; (2) ESMERIL do marco anterior: `relatorios/cartao_pela_celula.py:194::orfas_e_isencao_da_ata` com **zero chamador de producao** espera a medicao de QUEM responde "orfa"/"isencao" para o cartao hoje antes da L-111; fila em `logs/fila/orfas_e_isencao_da_ata_sem_chamador_20261011.txt`.

## MARCO 11/10 15:1x — O48 passo 3, os 9 selos INVERTIDOS: a duvida que eles levantavam se mediu na frota e a resposta e **ZERO NA TELA = 0**
LEI-AKITA: origem=`relatorios/cartao_pela_celula.py::totais_da_folha`, testemunha=`ponto/services/fechamento.py::fechamento_lido` (a celula soberana pela porta do passo 2), RED=`relatorios/tests/test_o48_passo3_cartao_le_online.py::test_MORDE_a_foto_velha_nao_manda_no_numero_que_o_leitor_mostra`, quem-mais-le=9 selos em 7 arquivos de teste + 1 command de producao, juizes novos=0.
- **A DUVIDA NAO SE ARGUMENTOU, SE MEDIU.** Os 7 selos vermelhos mostravam `0.0` onde a foto dizia horas, e a pergunta era se o leitor online cala para quem trabalhou. PROVA `logs/o48p3_zero_na_tela_20261011.out` (sombra, rc 0): **10/2026** gravado 587, com horas>0 **484**, lido>0 **484**, **ZERO NA TELA = 0**, **SE CALA = 0**; **09/2026** gravado 607, com horas>0 **514**, lido>0 **514**, **ZERO NA TELA = 0**, **SE CALA = 0**; erro 0 nas duas. Os `0.0` eram **artefato de fixture** (foto escrita sem batida), nao regressao -- entao **nao nasceu guarda nova** em `fechamento_lido` e nao houve PAREI.
- **4 CORTES SEUS SAO SUPERADOS, e cada docstring nomeia o seu**, pelo aval posterior sobre a MESMA pergunta (FECHAMENTO-ONLINE, `CORTES.md:65`): FALTA-UM-SIGNIFICADO (`:70`, L-039), RESUMO-UMA-FONTE (`:79`), ESPELHO-TELA-LE-A-FOLHA (`:83`) e CARTAO-PELA-CELULA (17/09, na docstring do proprio arquivo -- nao ha entrada de 17/09 no CORTES.md). Selo nao se apaga: a assercao TROCA e passa a morder a VOLTA do numero velho.
- **A ANCORA DE TODAS e `totais_da_folha(...)`**, a autoridade que o leitor le -- nunca um literal, nunca `fechamento_lido(...).<campo>`: `total_noturnas` e `total_intra_indenizada` **nao sao campos de modelo**, saem de `eventos_do_fechamento`. Cada inversao traz guarda ANTI-VACUIDADE `assertNotEqual(<o que a FIXTURE gravou na foto>, autoridade[k])`; 3 fixtures nao tinham FATO (comparavam 0 com 0) e ganharam batida real ANTES da assercao -- e o dia da ficha se pergunta a `EscalaColaborador.eh_dia_trabalho` (`escala/models.py:1048`), porque aritmetica de 12x36 poderia cair em FOLGA, onde a batida paga em rubrica propria e deixaria `trabalhadas` em 0.
- **O UNICO RED QUE SOBROU ERA PRECISAO, nao autoridade**: `12.02 != 12.0 within 2 places` no payload do app. `api/views.py:1431` entrega `round(_res['total_trabalhadas'], 1)` -- o KPI mostra **UMA casa** -- e o turno da fixture e 19:00->07:01 (12 h 1 min). O `assertNotEqual(11.0, ...)` passou, logo **nao era a foto vazando**. A assercao passa a comparar contra o arredondamento DELE com igualdade **EXATA**, citando o sitio: alargar `places` seria mexer na tolerancia (**L-099**, PROIBIDO).
- **MEDIDO: nenhum caminho de request itera a frota pelo leitor por colab.** `totais_da_folha`/`folha_manda` tem ZERO ocorrencia em `views*.py` de request; quem itera e so management command (6 sitios, territorio do passo 6), sem exposicao ao `timeout=30` do gthread. Conhecido e aceito: `espelho_do_colab` paga o motor 2x por pagina (~150 ms numa pagina de um colab).
- **`divergencia` tinha ficado TAUTOLOGICA** (os dois lados liam a foto): **0** divergentes no HEAD sobre os 221 da 09, contra **7** em 23/09. Passa a ler o GRAVADO no segundo lado pelo MESMO mapeamento.
- **A SUITE CHEIA PEGOU O QUE OS 27 VIZINHOS NAO PEGAVAM**, e e a defesa de rodar a cheia: a 1a rodada veio `Ran 10393 tests` / **FAILED (failures=1)**, e a falha era `core/tests/test_selo_teste_sem_relogio.py` (TESTE-SEM-RELOGIO, seu corte de 19/09) acusando o arquivo NOVO -- ele escolhia a competencia com `timezone.localdate()`. O selo estava **certo**, e nao por formalidade: a competencia corre 21->20, entao no dia 21 00:05 o arquivo mediria outra competencia e o dia de 12 h cairia no FUTURO, onde `ate_ontem` o corta -- a classe exata da bomba de 00:00. Cura **pela forma do corte, nao pela lista** (`testes_sem_relogio.txt` SO ENCOLHE): `@freeze_time` na classe e competencia CRAVADA. Depois: 46 testes dos 9 modulos afetados + o selo, `Ran 46 tests in 15.986s` / **OK**.
- PISTA: ruff `All checks passed!` rc 0 · 27 vizinhos `Ran 174 tests in 39.552s` / **OK** · suite cheia na propria raia **`Ran 10393 tests in 1417.302s` / OK (skipped=42)**, zero FAIL e zero ERROR. **COMBOIO medido** (`git worktree list` + status por raia): wt-bos esta no meio da O44 com 16 sujos e nao fica pronta em 30 min, wt-ui limpa -> **RODA SOZINHA**. SUITE-UMA-VEZ-POR-ARVORE provada por impressao digital do diff antes do veredito e antes do commit (iguais, no corpo do commit).
- **NAO POUSA, e o motivo e `!`**: o merge espera o MESMO `!` do passo 5 (`O48-PASSO5-CHAVE-COM-O-DIFF-LIDO`, aberto 11/10 13:12), que pela CONTRATOS-ATE-A-VIRADA (`BACKLOG.md:56`) nao abre antes do export da competencia 10 (~20/10). Commit na raia `raia-o48p3`: `e14e653f`.
- INCOMPLETO: (a) a docstring de MODULO de `relatorios/tests/test_cartao_pela_celula.py:1-9` ainda traz o paragrafo "O que MORDE" de 17/09 -- em letra nao ficou falso, ficou INCOMPLETO --, e nao se corrigiu aqui porque a suite ja estava montada nesta arvore (vai no ato do passo 4, mesma raia); (b) FILA, **pre-existente e nao deste marco**: `relatorios/cartao_pela_celula.py::orfas_e_isencao_da_ata` tem **ZERO chamador** em qualquer lugar (so o proprio teste, `test_cartao_pela_celula.py:151,158`) -- candidata **L-111**, e o ESMERIL-DO-MARCO proibe abrir achado de fora do marco; (c) o censo falso **"2 chamadores"** em `BACKLOG.md:531` e `FECHAMENTO-ONLINE.md:16` (sao TRES caminhos; `recalcular_por_evento` tem 5 chamadores de producao) viaja como passageiro no proximo marco da principal (L-106).

## MARCO 11/10 13:3x — O48 passo 3, a cura do UNIVERSO: `situacao='ativo'` era um SEGUNDO juiz de "esta afastado?", e a cura muda **QUEM esta no TXT**, nao so quanto
LEI-AKITA: origem=`ponto/services/fechamento.py:136` (o universo do recalculo lia o VALOR `ativo`), testemunha=`ponto/turnos.py::afastado_hoje` -- o juiz POR DIA da pergunta, que sai da Ausencia que o motor JA le dentro do mesmo laco, RED=`ponto/tests/test_fechamento_escopo_competencia.py::test_afastado_hoje_entra_no_recalculo_e_o_lido_responde`, quem-mais-le=`recalcular_fechamento_mes` tem 3 chamadores (botao da tela, `simular_folha --recalcular`, `recalcular_por_evento`) e nenhum deles enumera situacao por conta propria, juizes novos=0.
- **A FORMA E PELA NEGATIVA de proposito**: `~situacao='desligado' | desligado com demissao na janela`. `afastado` deixou de ser VALOR de cadastro em 03/10, entao enumerar `('ativo','afastado','ferias')` seria reler o valor morto e cada situacao nova nasceria FORA do dinheiro sem ninguem notar. `data_demissao` NULA em `desligado` segue FORA, byte-identico.
- **QUEM ENTRA, medido na sombra e nao previsto** (PROVA `logs/o48uni_reversao_20261011.out`, rc 0): **ENTRAM 11, SAEM 0** nas quatro competencias, e sao **os MESMOS 11 ids todo mes** -- col53 col75 col83 col199 col210 col271 col645 col646 col649 col674 col675, todos `situacao='afastado'` com `data_demissao=None`. Por competencia (universo velho -> novo / INSERT / UPDATE): **07** 729->740 / 3 / 8 · **08** 671->682 / 10 / 1 · **09** 622->633 / 11 / 0 · **10** 583->594 / 11 / 0. **35 inserts possiveis.**
- **SAO AFASTADOS DE VERDADE, E A AUSENCIA E A AUTORIDADE** (PROVA `logs/o48uni_os11_20261011.out`, universo pedido a funcao REAL `ponto/turnos.py::batidas_apuraveis`): 10 dos 11 tem **0 batida** em toda janela, com Ausencia cobrindo. O unico com FATO e **col649** -- 20 batidas e **112,18 h GRAVADAS** na 08 --, e e exatamente o caso que a cura cita: `fechamento_lido` devolvia `None` para quem trabalhou.
- **A CONSEQUENCIA QUE EU NAO IA PERGUNTAR, e ela e a que importa** (LEI-AKITA 8): a linha ZERO de um afastado **NAO e filtrada** pelo juiz unico do TXT. Perguntado ao `folha/export.py::classificar_export` sobre a **07**, que JA tem 8 dessas linhas (PROVA `logs/o48uni_entra_no_txt_20261011.out`): **col53, col199, col271, col646 e col649 saem `status=entra, motivo=None`** -- DENTRO das 300 --, e so col75/col210/col645 saem `fora/furo_espelho`. Na 08/09/10 todos os 11 estao AUSENTES do universo do juiz. Entao a cura leva o universo do juiz da **10** de 587 para ate 598 e o `entra` de **189** para ate ~**200**.
- **A 07 E ACIDENTE DE TEMPO, NAO PRECEDENTE**: aquelas 8 linhas existem porque o recalculo da 07 rodou ANTES de eles virarem `afastado`, nao porque alguem decidiu que afastado entra.
- **"NADA RECALCULA SOZINHO" ERA FALSO, e a frase era MINHA**: eu a tirei do censo de CODIGO (`docs/FECHAMENTO-ONLINE.md`, *"dois chamadores: o botao e `simular_folha --recalcular`"*) e o censo esta INCOMPLETO. `recalcular_por_evento` e automatico, com **5 chamadores de producao** -- `ponto/registro_batida.py:136-137` (no `on_commit` da batida), `ponto/portas/he.py:185`, `ponto/portas/celula.py:322`, `chamados/services/validacao.py:240` e `:307` --, sempre `colaborador_ids=[pk]` e na competencia **DA DATA DO FATO**.
- **ENTAO EU PERGUNTEI AO DADO, nao ao codigo** (PROVA `logs/o48uni_quem_escreve_de_noite_20261011.out`, rc 0, 14 dias, carimbo por `timezone.localtime`): das **1.324** escritas de `FechamentoMensal`, **220 caem em faixa de cron (00h-07h)**, e **185 delas sao de 28/09** -- o proprio congelamento do gravado da 09. As outras em **02/10 (4), 06/10 (29), 09/10 (1)**: **quatro dias de catorze, logo NAO HA varredura noturna de frota**. E **dos 11, ZERO foi tocado em faixa de cron**.
- **ACHADO LATERAL, com numero, que vai para a fila e NAO e desta fatia**: a competencia **EXPORTADA (09) JA e tocada** pelo caminho automatico depois do TXT -- 198 linhas dela na faixa de cron, 198 colabs distintos -- desde que a clausula 1 da L-113 derrubou a recusa em 10/10 (`fechamento.py:107-113` hoje so carimba `logger.warning`).
- **O DEPLOY DESTE MARCO NAO MUDA UMA TELA HOJE, e eu medi em vez de supor**: `fechamento_lido` segue com **zero chamador de producao** (grep no vivo: so a propria definicao, 2 docstrings e 4 arquivos de teste), e `ponto/views.py:945` le o **GRAVADO** direto por `FechamentoMensal.objects.filter(...).first()`. A cura e LATENTE: ela so materializa linha na PROXIMA vez que alguem recalcular, ou quando um evento de fato alcancar col649.
- **AS QUATRO CONDICOES DA DINHEIRO-EM-COMPETENCIA-ABERTA**: DIFF de frota publicado ANTES (`logs/o48p3_diff_gravado_x_lido_20261011.md`, 206 linhas) · reversao em `logs/o48uni_reversao_20261011.out` com os ids e os 35 inserts · competencia EXPORTADA intacta -- agora **PROVADO pela medicao da faixa de cron**, nao suposto, porque nenhum dos 11 e alcancado la e nenhum cron chama `recalcular_fechamento_mes` sem colaborador · PROVA depois, na primeira recalculacao.
- **O QUE ESTE MARCO DERRUBA DO MARCO DAS 11:5x, dito com o numero**: eu havia registrado *"0 inserts na 10, ~6 na 08, 1 na 09"*. Medido: **11 / 10 / 11**, e a 10 -- a competencia VIVA, a que a porta do TXT vai rodar de verdade -- e justamente a que recebe 11. A tabela gravado x lido daquela secao e de `1ceed412` e e **PRE-cura de universo**.
- **PISTA**: suite CHEIA na copia `$S/o48uni` -- `Ran 10390 tests in 1432.029s` / `OK (skipped=42)` / rc 0 --, e a arvore certificada difere do HEAD em **2 arquivos e so neles** (`cmp` arquivo a arquivo sobre os 3.209 rastreados, citado no corpo do commit): a cura e o selo. Portao `sombra --conferir`: `dia=20261011 status=OK tipo=completa diverge=0`.
- **INCOMPLETO**: a cura do `:568` (zero -> sem resposta) segue nao escrita e e o proximo ato; a chave do passo 5 espera a leitura dele e **nao vira antes do export da 10** (BACKLOG.md:56, limite dele); e o achado da 09 exportada tocada de madrugada e linha de fila, com os 198.

## MARCO 11/10 11:5x — O48 passo 3: **o DIFF que o aval dele pede, colado aqui** -- e ele achou um bug de DINHEIRO que ja esta no gravado e no TXT de tres competencias
LEI-AKITA: origem=`ponto/services/fechamento.py:568` (o `sum({}.values())` que vira 0) e `relatorios/cartao_pela_celula.py::totais_da_folha` (o leitor que copia a foto), testemunha=`ponto/services/fechamento.py::fechamento_lido` -- a MESMA funcao que grava quando grava, zero derivador novo, RED=`relatorios/tests/test_o48_passo3_cartao_le_online.py` (2 casos, `FAILED (failures=2)` evidenciado na copia `git archive 1ceed412`), quem-mais-le=3 leitores de request por AST (`cartao_pela_celula.py:51`, `folha/services/ranking_he.py:161`, `ponto/views.py:945`) + `calcular_scores`, que e FROTA e vai no passo 6, juizes novos=0
**A CHAVE NAO VIRA NESTE MARCO, e e a unica coisa que o `!` dele cobre** (`CORTES.md:65`, literal: *"Virar a chave em prod SO depois do DIFF lido pelo Ronald"*). Construir, medir, commitar e publicar o DIFF **nao** estao no `!` -- entao e isso que este marco faz, e a fila SEGUE para o passo 4 com a chave intacta. O `CONTRATOS-ATE-A-VIRADA` de hoje poe os passos 3 e 4 como item 1 da principal e **nao** toca esta condicao (LEI-AKITA 9).
- **O NUMERO, gravado x lido pelas 24 rubricas do `CAMPOS` canonico** (nenhuma lista digitada por mim), sombra `status=OK tipo=completa diverge=0`, sem nome/CPF/reais, PROVA `logs/o48_diff_070809.out` e `logs/o48_diff_b.out`: **07/2026** 629 gravados, 258 IGUAIS, **362 DIFEREM**, 9 sem resposta · **08/2026** 618, 107, **509**, 2 · **09/2026** 607, 342, **264**, 1.
- **ANTI-VACUIDADE DO PROPRIO DIFF**, porque `IGUAIS` alto seria o jeito barato de parecer seguro: as **24 de 24** rubricas sao ATRIBUIDAS no corpo do calculo (varredura de `fechamento.<campo> =` sobre o vivo, **0 herdadas** do gravado). Rubrica so herdada leria IGUAIS por nunca ter sido respondida.
- **OS TRES BALDES**: `nulo->valor` **vazio** nas tres, `valor->nulo` **nenhum** -- toda diferenca e `valor->valor`. A 08 e a que ele precisa ver: **509 de 618**, com `horas_trabalhadas` **+2.345,42 h**, `horas_folga_trabalhada` **+2.369,28 h**, `horas_extras` **-601,67 h**, `horas_noturnas` **-1.966,31 h**, `horas_saida_antecipada` **-2.385,58 h** -- o acumulado da CLASSE3-FOLGA-100 (19/09), do *"trabalhadas ganha a folga"* (27/09) e das curas seguintes sobre uma competencia **ja paga**. Na 09, `horas_noturnas` -2.242,23 h em 86 colabs e `minutos_realizados` +56.425 em 123.
- **ROTULO DA 09**: a sombra leva a virada da O219 DENTRO (bloco de 20:15, 152 atas da emp4/09 re-julgadas), entao a linha dela e *RECALC-09 + efeito da virada*, e **prod alcanca esse estado no `06:28 processar_cartorio --apply`**, nao antes.
- **O RED DO AVAL ESTA VERDE, E NAO FOI O O48 QUE O FECHOU**: `col37` e `col39` na 09 dao gravado == lido nas 24 -- foi o RECALC-09 que ja moveu a foto deles para o numero da celula. O RED que prova o passo 3 e outro, e e meu: (1) a foto gravada muda de 3 para 99 e **o numero do leitor muda com ela** (`3.0 != 99.0`); (2) a celula daquele dia de 12 h responde **2,0 h** de HE 50% e o leitor mostra os **3,0 h** da foto (`2.0 != 3.0`). O caso 2 nasceu com guarda anti-vacuidade (`assertNotEqual(3.0, da_celula)`) e ela se pagou: eu havia **chutado 4,0 h** e a medicao disse 2,0.
- **A LINHA DA 07 NAO E "REGRA VELHA", E EU IA PUBLICAR QUE ERA** -- fui medir a forma porque `minutos_previstos` **-2.036.747 min / 181 colabs = -187 h cada**, que e o mes inteiro e nao tem forma de deriva (PROVA `logs/o48_diff_c.out`, `_d.out`, `_e.out`). A 07 e **PRE-CELULA**: 217 celulas para 15 colabs no periodo 21/06..20/07, contra 16.894/560 na 08 e 17.332/577 na 09. Os colabs que o lido ZERA sao exatamente os sem celula (`minutos_previstos` 175 de 175, `dias_previstos` 175 de 175, `minutos_realizados` 124 de 124; na 08 isso cai para 1 e na 09 para **0**). **E O OBJETO LIDO SE CONTRADIZ**: 131 colabs com `minutos_realizados=0` **e** `horas_trabalhadas>0` no MESMO objeto (col89 com 189,90 h trabalhadas e 0 minuto realizado), contra 2 coerentes. Dono **ESTRUTURA** pela L-099 -- o sistema se contradiz dado o cadastro que TEM.
- **ORIGEM, e nao e no leitor**: `fechamento.py:568`, `fechamento.minutos_realizados = sum(_real_dia_m.values())`. Sem celula a grade sai VAZIA e `sum({}.values())` **e 0** -- ausencia de sinal lida como sinal bom. O `None` ja existe no sitio (`:595`), mas so no `except Exception`: *"estourou"* responde sem resposta e *"nao ha o que ler"* responde zero. **E O CONSUMIDOR DISTINGUE OS DOIS, com dinheiro no meio**: `folha/export.py:791-792` -- `_real_h = (fech.minutos_realizados / 60.0) if fech.minutos_realizados is not None else trab` -- com `None` cai em `horas_trabalhadas` e acerta; com **0** escreve **zero hora realizada no TXT de quem trabalhou 189,90 h**. Nao e defeito da porta de LEITURA: `recalcular_fechamento_mes` GRAVA pelo mesmo caminho, e a leitura so o tornou visivel.
- **PAUTA DP, porque o defeito NAO e hipotetico e ja saiu no TXT** (`logs/o48_diff_e.out`, contadores do GRAVADO): `minutos_realizados=0` **e** `horas_trabalhadas>0` **no gravado** em **6 colabs da 07**, **14 da 08** e **2 da 09** -- as tres **exportadas**, e o TXT delas levou zero hora realizada. `minutos_realizados IS NULL` no gravado: **0**. Pela lei do TXT (30/09) e pela L-092 isso **nao se corrige em silencio**: a diferenca tem DONO e sai como Pauta DP com os dois numeros, nunca como numero que mudou sozinho.
- **O UNIVERSO DA 07 TAMBEM MEDE O TAMANHO DA PERGUNTA**: dos **629** gravados, **497 tem `horas_trabalhadas == 0`** e **132 tem > 0**. Entao a 07 nao e "a competencia que divergiu": e a competencia em que 79% do gravado nao fala de hora trabalhada nenhuma, e o lido nao tem celula para ler.
- **O QUE O PASSO 3 INVERTE**, e sao dois selos do corte CARTAO-PELA-CELULA (17/09), citados pelo nome porque selo nao se apaga -- a assercao troca: `test_cartao_pela_celula.py::test_MORDE_o_total_do_cartao_e_o_da_folha_mesmo_quando_o_motor_discorda` e `::test_sem_fechamento_gravado_o_cartao_nao_inventa_numero:124`. O terceiro sitio e clausula, nao selo: `cartao_pela_celula.py:70-74` carrega a RESUMO-UMA-FONTE 2a metade (23/09 19:2x).
- **ACHEI UMA PROMESSA SEM SELO, e ela muda o custo do passo 3**: `CORTES.md:79` prometia *"Selo de performance: 0 query de motor a mais na tela dos 750"* e ele **nunca foi construido** (`grep` de `query de motor|0 query|sem corrida de motor` em `test_*.py` = **0**; `dos 750` em `*.py` = **0**; nenhum `assertNumQueries` cobre a tela do espelho/cartao). Quando o passo 3 puser motor na pagina, **nada fica vermelho sozinho** -- o custo se mede a mao (DIFF=0 e **<=0,15 s por pagina**) e o selo que falta e linha de fila.
- **DECISAO TECNICA, minha pela PAREI-SO-LEI** (desenho de encaixe, nao lei): `calcular_scores` **nao e leitor de request** -- varre `colaboradores_em_operacao()` com um `filter(colaborador=..)` por colab, as 03:25. *"<=0,15 s por pagina"* nao quer dizer nada para um cron, entao ele vai no **passo 6** com o cache declarado que o censo previu para o caminho de frota (~65 s), nao no 3.
- **`fechamento_lido` TEM ZERO CHAMADOR DE PRODUCAO HOJE** (AST no vivo) -- nasceu no passo 2 e quem o chamaria e o passo 3. Isso **nao** o faz morto pela L-111 (ela fala de funcao que PERDEU o ultimo chamador), e tem consequencia pratica: mudar o que ele responde quando nao ha o que ler **nao move um numero em prod**, e e o momento mais barato que vai existir para acertar isso.
- **INCOMPLETO**: a cura do `:568` (zero -> sem resposta) **nao esta escrita** -- e BUG NO CAMINHO (LEI-AKITA 6) e e o proximo ato, com RED antes; a cura do leitor online (as 2 inversoes de selo) tambem nao; o selo de performance do `CORTES.md:79` segue como linha de fila; e a **chave em prod espera a leitura dele**, que e o unico `!`.

## MARCO 11/10 11:1x — O148 item 1 **NO AR DE VERDADE**: o cron da lavra instalado, e a pegada da lavra e maior que as 23 FolgaDia porque o escritor **tambem re-lavra a celula**
LEI-AKITA: origem=`config/crons.py:317` (a lavra ganha horario) + o aval literal dele de hoje, testemunha=`escala/folgas.py::divergencia_foto_x_calendario` / `ponto/janelas.py::janela_atual` / `ponto/precedencia.py::fatos_do_dia`, RED=nao se aplica (install de cron; a guarda e o DIFF), quem-mais-le=8 leitores de `FolgaDia` em producao, juizes novos=0.
**PROVA:** do ato que o titulo afirma ("instalado"), medido ao vivo em 11/10 17:1x e nao copiado da prosa: `crontab -l | grep -c lavrar_foto_folgas` = **1**, a linha instalada e `47 5 * * * ... lavrar_foto_folgas ... --apply`, e `bin/crons.sh check` devolve **rc 0** com `crontab == config/crons.py (103 linhas)` -- a derivacao e o crontab vivo dizem a MESMA coisa. (Cura da linha sem prova cobrada pelo `bin/relato_afirma_com_prova.py`; item 5 do aval RAIAS-ANDAM-JA.)
O aval derrubou a condicao que me parou as 04:5x: os 7 do BACKFILL sao `cal#28` vazio (CADASTRO, L-099) e quem move sao OUTROS 4 ECs, com `ec1278` autorizado nomeadamente. O item (7) do bloco `!` DE CADASTRO do topo (04:5x) dizia `crontab -l | grep -c lavrar_foto_folgas` = **0**; agora e **1**.
- **O DELTA DO `install` TINHA DUAS LINHAS E A SEGUNDA NAO E EDICAO MINHA** (autoridade: `diff` do backup contra o `crontab -l`, nao memoria): `noturno.py` move **`33 2` -> `35 2`** porque `config/crons.py:473` nao guarda literal -- e `inicio_derivado('noturno.py', vizinho='03:20', piso='00:15')`. A duracao medida **SUBIU** (2518 -> **2574 s** no `crons_duracao.json` de hoje, 42 -> 43 min), e por isso o derivado anda para TRAS: 03:20 - 2 min de `TOLERANCIA_B6_S` - 43 min = **02:35**, e cabe por construcao (02:35+43=03:18). O `33 2` que estava instalado vinha de medida AINDA MAIOR (~45 min): era o ATRASO do crontab contra a derivacao, e a funcao real perguntada agora devolve `35 2`. Tabela em `logs/foto_folga/PAREI_install_backfill_20261011.txt` -- ela compara derivacao x derivacao (02:36 -> 02:35), e o crontab vivo era um TERCEIRO valor: quem manda e o backup.
- **A JANELA CORRIGE O AVAL em um ponto**: `gerar_celulas::_janela_default(hoje)` = **2026-09-21..2026-10-20**, e as "4 datas em 10/2026" de `ec1278` sao mes CIVIL (e assim que `divergencia_foto_x_calendario` pergunta). Por COMPETENCIA sao **3 + 1**: 10-07/10-12/10-19 na 10/2026 e **10-24 na 11/2026**. As duas abertas.
- **DRY-RUN EM PROD, so leitura, identico a sombra** (`logs/foto_folga/dryrun_PROD_20261011.txt`, rc 0, **DURACAO_S=1,48**): universo 203 vinculo-mes, **A GRAVAR 4 vinculo-mes / 23 datas**, BLOQUEADO 16 por sobra (65 datas) e 1 por competencia fechada, template vazio 132. E a unica leitura que liga a medicao de hoje ao apply de amanha.
- **DIFF DE FOLHA = 0**, pelo device da casa, UMA trava, arvore identica nos dois lados, `--antes "lavrar_foto_folgas --apply"`: `sombra_cobre=sim (completa de hoje)`, emp2 136->136 · emp3 67->67 · emp4 5->5, `entraram=0 sairam=0 mudaram=0`, retidos iguais, **`DIFF_FOLHA=0`**, `comparar rc=0`, rc do processo 0 (`logs/foto_folga/diff_folha_o148_20261011.txt`).
- **A METADE (b) EXISTE PORQUE O MOTOR NAO LE `FolgaDia`** (`motor_calculo_v2.py:1801` e comentario): quem move e `ponto/precedencia.py::_folga_avulsa:146-151`, e ele **nao filtra `tipo`** -- `FolgaDia.tipo` e declarado informativo no modelo (`escala/models.py:1548`). Veredito de `fatos_do_dia` nas 4 datas, ANTES -> DEPOIS: 10-07 `folga`->`folga`, 10-12 `feriado`->`feriado`, 10-19 `folga`->`folga`, **10-24 `trabalho`->`folga`** (a unica que muda de palavra). `batidas_apuraveis=0` nas quatro.
- **A PEGADA REAL NAO SAO AS 23 FolgaDia**: `gerar_foto_folgas:189-191` chama SEMPRE `relavrar_celulas_da_foto` do mes civil (TORNEIRA FOLGAS-NO-ESPELHO, corte 02/09), que delega a `regenerar_celulas_vinculo`. Medido na frota INTEIRA: **1 celula regenerada** (col746 2026-10-07, `trabalha` True->False, `regeneracoes` 0->1 -- a celula discordava do proprio cadastro, e `fatos_do_dia` ja dizia `folga`) e **101 celulas NASCIDAS**, todas futuras (col128 30, col142 30, col216 30 em 11/2026; col746 11 em 10-21..10-31). **0 em competencia com exportacao** -- as exportadas sao (2,08) (2,09) (3,07) (3,09) (4,06) (4,07) (4,09) e nenhuma das 23 datas as alcanca.
- **CONSEQUENCIA DIARIA DO INSTALL, dita porque amanha acontece em prod**: por a porta TORNEIRA no cron, vinculo com calendario ganha celula ate o fim do mes civil da competencia SEGUINTE (11-30), a frente do horizonte do proprio `gerar_celulas`. E comportamento pre-existente da porta, nao fatia.
- **IRRETROATIVIDADE E IDEMPOTENCIA MEDIDAS, nao inferidas**: o gerador rodou na mesma sombra logo depois, sob trava propria -> **`celulas 2026-09-21..2026-10-20 criadas=0 atualizadas=0 puladas=10113`**, rc 0, e a pegada repetida depois dele e IDENTICA. Por isso nao ha segundo DIFF de folha: com `criadas=0 atualizadas=0` o gerador nao tem por onde mover dinheiro.
- **O VAO 05:47->05:50 E APOSTA DE RELOGIO, e isso fica dito**: `depende=('lavrar_foto_folgas',)` numa entrada `C` e DECLARACAO, nao espera -- quem espera arquivo e `bin/encadeado.sh`, anexado so no ramo das entradas `ENC` (`crons.py:1415`), e mesmo lá o teto e limitado (900 s, exit 2 = alarme, nao roda). A aposta esta coberta: 1,48 s medidos contra 180 s de vao e 120 s declarados.
- **REVERSAO** em `logs/foto_folga/reversao_lavra_20261011.txt` (68+ linhas): as 23 FolgaDia por `(escala_colaborador_id, data)`, `LINHAS EM COMPETENCIA COM EXPORTACAO: 0`, o estado ANTES da celula #104359 e a ordem de desfazer. **E a linha que importa: celula nascida ou regenerada NAO se desfaz por delete** -- a reversao dela e `regenerar_celulas_vinculo` com trilha, que e dado de ESCALA e exige o `!` (L-009).
- **PROVA DO INSTALL**: `bin/crons.sh install` rc 0 (backup `logs/crontab_backup_20261011_111541.txt`, 103 linhas), `bin/crons.sh check` rc 0 -> `crontab == config/crons.py (103 linhas)`, e a linha viva lida do crontab: `47 5 * * * .../bin/cron_run.sh lavrar_foto_folgas - 'docker exec saas_core python manage.py tenant_command lavrar_foto_folgas --schema=juliani --apply' >> .../logs/lavrar_foto_folgas.log`.
- **INCOMPLETO**: o *resultado com PROVA depois* e o log das **05:47 de amanha** (`logs/lavrar_foto_folgas.log`), com o `logs/gerar_celulas.log` das 05:50 ao lado; e os 5 RESULTADOS OBSERVAVEIS do `docs/O148-COMPLEMENTO.md:62-67` seguem sem prova em PROD.

## MARCO 11/10 06:4x — O197 FUTURO-NAO-E-EM-ABERTO: a tela chamava "Em aberto" 6.609 dia-colab, e **92** sao legitimos

LEI-AKITA: origem=`ponto/services/dia_decidido.py::datas_em_aberto_de` (o derivador) + `relatorios/cartao_pela_celula.py::folha_manda` (o escritor que o alimenta), testemunha=`relatorios/cartao_pela_celula.py::ate_ontem` (o teto que JA tinha nome, `:37`) e `core/constants.py::isento_ponto`, RED=`relatorios/tests/test_o197_futuro_nao_e_em_aberto.py` (14 casos; 9 VERMELHOS medidos na arvore sem cura), quem-mais-le=3 leitores da lista (grade `colaboradores/services/calendario.py:610`, cartao `relatorios/pdf_espelho.py:286`, app) + 2 contadores (`folha/porta_export.py:476`, `ponto/services/dias_em_aberto.py::passivo_da_competencia`), juizes novos=0

- **O DEFEITO ERA SOMA DE PARCELAS DE NATUREZA DIFERENTE.** `datas_em_aberto_de` somava **FURO APURADO** -- AUSENCIA de fato ("nao bateu"), afirmavel so de dia ENCERRADO e so de quem REGISTRA jornada -- com **TURNO ABERTO** -- PRESENCA de fato ("bateu a entrada, falta a saida"), que existe desde o instante da batida, hoje incluido. Uma soma, dois tetos devidos, nenhum aplicado.
- **A CURA.** O derivador passa a receber `ate` e `isento` **keyword-only, sem default** (CURA-MAIS-RESTRITIVA: default seria fallback esperando o proximo leitor esquecer a lei): a parcela de PRESENCA segue sem teto nenhum; a de AUSENCIA filtra `d <= ate` (inclusivo -- um `<` perderia o dia encerrado e zeraria os 92) e desaparece inteira quando `isento` (Art.62). O ESCRITOR calcula `_ate = ate_ontem(data_fim_pedido)` -- **a 4a grafia de `hoje - 1` desta metrica ja morava no proprio arquivo da cura**, e escrever `d < hoje` a mao seria a 5a, invisivel a um grep por `ate_ontem` (LEI-AKITA 2).
- **O TETO E DE PARCELA, NAO DO UNIVERSO, e o que prova isso e o relogio**: `ponto/turnos.py::turno_aberto_vivo:1772` mata o turno aberto em `janela_turno_de` + 4 h, entao das **21:00** a meia-noite um turno aberto de HOJE e legitimamente "em aberto" toda noite -- teto por data sobre o universo apagaria esses dias e os devolveria a meia-noite. Medido: h=10/18/20 vivo=True, h=22/23 vivo=False. `test_07` congela 22:00 e e VERDE antes **e** depois: ele nao prova a cura, prova que a cura nao virou silencio.
- **O LEITOR DEFEITUOSO E EXATAMENTE UM, e nao era o que o nome sugeria**: a GRADE (`calendario.py:617`) pergunta pelo MES INTEIRO; o CARTAO ja clampa a propria janela em `pdf_espelho.py:286`, e por isso o cartao de 08/2026 congelado em 23/07 publica 21 e 22/07 e nada mais. **Ninguem entra nem sai do TXT**: o portao `furo_espelho` de `folha/export.py::classificar_export:687` le campos GRAVADOS do `FechamentoMensal`, nao esta lista -- so os 2 CONTADORES se movem.
- **O NUMERO** (10/2026, universo 557 ativos, erros 0, `logs/o197/frota_20261011_0548.out`): de **6.609** dia-colab que a tela chamava "Em aberto", **92** sao legitimos -- **1,4%**. FUTURO 6.285 em 511 colabs; HOJE 232 em 232 colabs as 05:48; CASO 5 (status `ok` **E** palavra de em-aberto no MESMO dia) 55 dia-colab em 8 colabs, todos `isento=True | periodos=0`.
- **A ANTI-VACUIDADE ERA O RISCO REAL, e esta nomeada**: `test_palavra_do_dia.py::test_09` passava VERDE sobre este defeito porque os tres leitores erravam IGUAL -- *"dois leitores calados sao iguais"*. O RED novo prende DATAS NOMEADAS presentes/ausentes e exige `em_aberto_perguntado` em cada dia, nunca "os tres concordam". As duas curas se DISCRIMINAM: `test_05` e o discriminador do Art.62, `test_01/02` o do teto.
- **PROVA**: RED `Ran 14 tests` / `FAILED (failures=6, errors=3)` / rc=1 (`logs/o197/RED_20261011_0640.out`, os 9 previstos) · GREEN `Ran 14 tests` / `OK` / rc=0 · vizinhos (19 modulos que nomeiam a lista) `Ran 151 tests` / `OK` / rc=0 · ruff `All checks passed!` · 68 selos de host na copia, 5 vermelhos todos sem-ambiente e nenhum nomeando arquivo desta fatia · censo: **1** chamador de producao do derivador · desenho em `logs/o197/DESENHO_20261011.txt` (128 linhas).
- **SUITE CHEIA sobre a arvore certificada, VERDE**: `Ran 10389 tests in 1422.926s` / `OK (skipped=42)` / rc_processo=0 (`logs/o197/SUITE_20261011_0646.out`, 1978 linhas), 0 `FAILED` no corpo. Mais **68 selos de host no MAIN, 0 vermelhos**, rodados DEPOIS da dieta.
- **LEI ANTES DO PATCH, os dois sitios tem lei que os protege e nenhuma clausula se move**: **L-039** (FALTA TEM UM SIGNIFICADO) PROTEGE `cartao_pela_celula.py::folha_manda` -- o catalogo que desconta nao se toca, `_decididas` segue saindo de `datas_falta_janela` (`:291`) e chega INTACTO como `decididas=`; muda o UNIVERSO de onde se subtrai, nunca o significado de falta. **L-102** (DIA COM ATA E SEM PAR TEM PALAVRA) PROTEGE `dia_decidido.py::ata_sem_turno`/`::veredito_do_dia`/`::palavra_do_dia` -- **nenhuma das tres editada**, a cura mora em `::datas_em_aberto_de`, e o RED novo EXIGE `em_aberto_perguntado` em todo dia: reforca a L-102.
- **DIETA DE PROSA PAGA NO MESMO ATO (L-109), e o tripwire mordeu**: o vivo guardava SETE dias (04/10 a 11/10) e a lei manda tres -- **7.297 linhas liquidas** para o `RELATO-ARQUIVO.md`, **3.773 vivas**, corte por CONTEUDO (o `## O146` nao tem data no titulo e e de 09/10, entao FICA; `## DEPLOYS AGENDADOS` e registro vivo). O corte levou 7.766 e **469 VOLTARAM**: `bin/tests/test_relato_guarda_pedido_de_patch.sh` ficou VERMELHO em menos de 1 s cobrando do vivo os 4 contratos de NOME da fatia 2 de Gestao de HE. **Terceira vez da classe** (02/10 push 94, 05/10 `fdd6f42c`, hoje) e a **primeira** em que o selo morde ANTES da suite em vez de o push voltar depois de 10 min de bateria -- e e o selo que nasceu em 05/10 porque a cura daquele dia foi so restaurar a palavra. A HISTORIA do push vermelho ficou no arquivo; o pedido ABERTO voltou ao vivo.
- **COMBOIO, em uma linha** (aval de 10/10 23:0x): a principal tem **so docs** sujos -- pelo criterio do proprio aval nao fica pronta em 30 min --, entao a `wt-bos` **rodou sozinha**. Commit da raia `5ecebd95`; suite cheia sobre a arvore certificada, pouso pela L-105 + L-107.
- **CORRECAO DE UMA LINHA, que o corpo de `5ecebd95` nao pode mais receber**: ele cita `logs/o197/RED_20261011_0730.out` e o arquivo e `logs/o197/RED_20261011_0640.out` -- datei de cabeca e o `date` dizia 06:46.
- **PROVA DEPOIS DO DEPLOY -- a MESMA sonda nas DUAS arvores, na MESMA sombra** (11/10 07:2x-08:3x, `dia=20261011`, erros 0 e rc 0 nas 4 rodadas; sem cura = `6dcaf1a7:app`, com cura = a arvore no ar; `logs/o197/PROVA_p1_*`, `PROVA_p234_*`, `PROVA_p5_*`, resumo em `PROVA_ACHADO_numero_publicado_20261011.txt`): GRADE FUTURO **6.285 -> 0**, HOJE **232 -> 0** (nesta hora: as 07:2x todo turno vivo de hoje sai por `em_curso`, e quem prova a noite e o `test_07`), CASO5 **55 -> 0**, PASSADO **92 -> 16**. O diff do PASSADO e **so delecao**: as 12 linhas que sairam sao TODAS `isento=True` (76 dia-colab), as 12 que ficaram sao TODAS `isento=False` com as datas IDENTICAS, e **nenhuma linha mudou**. Pauta DP (`passivo_da_competencia`): 10/2026 **194 dias / 61 colabs -> 17 / 17**, 09/2026 **309 / 43 -> 52 / 32**. Placar do export (`porta_export.medir`) emp2/emp3/emp4: na 10 **133/29/32 -> 16/1/0**, na 09 **218/45/46 -> 41/7/4** -- **os dois leitores dao o MESMO numero nas duas arvores**, empresa por empresa, que e a evidencia de que nao ha segundo derivador herdando a lista. E a classe `nunca_bateu` da Pauta ERA o Art.62: na 09 **12 pessoas e 216 dias -> 1 pessoa e 3 dias**.
- **"NINGUEM ENTRA NEM SAI DO TXT" DEIXOU DE SER LEITURA DE CODIGO E VIROU MEDICAO** (LEI-AKITA 8, cobranca do advisor): `folha/export.py::classificar_export` nas duas arvores, 7 empresas x 09 e 10, e o `diff` das saidas e **VAZIO**. E nao e so o contador -- a sonda imprime o **sha256 dos ids de quem ENTRA**, ordenados, e os **14 shas sao iguais**: o CONJUNTO e o mesmo, nao so o tamanho (221 entram na 09, 189 na 10), com os motivos de recusa inclusos (`furo_espelho` 239 e 277 na emp2).
- **CORRECAO DO NUMERO DO TITULO DESTA SECAO, e ela e minha, nao do codigo**: **92** e **1,4%** sao o balde PASSADO da arvore SEM cura -- nao o que SOBRA depois dela. O que sobra, MEDIDO, e **16 dia-colab em 12 colabs = 0,24%** de 6.609, porque os outros **76** sao de ISENTO e a parcela de AUSENCIA nao se afirma de quem nao registra jornada: e exatamente o que o parametro `isento` faz e o que `test_05` discrimina. A decomposicao agora FECHA, e antes nao fechava: 6.285 FUTURO + 232 HOJE + 76 ISENTO + **16** = 6.609 (com 55 era 6.572, e sobravam 37 sem dono). O titulo de `faa783e1` nao se emenda -- o deploy esta feito -- e secao escrita nao se reescreve (L-116): a correcao entra aqui, nao por cima.
- **O INCOMPLETO (c) GANHOU NUMERO E NOME**: dos 76 dias de isento, **53** saiam com `status=ok` (o passe livre F5c respondendo) e **23** com status diferente -- `col186` 7, `col257` 7, `col291` 5, `col216` 4 --, e e nesses QUATRO que o cartorio lavrou `veredito='nunca_bateu'` e `_classificar:161` sobrescreveu o `ok`. No cadastro ha **14** isentos e **482** `CelulaDia(veredito='nunca_bateu')` deles: esse e o universo da linha de fila propria.
- **INCOMPLETO, com dono**: (a) `calendario.py::_classificar:723` chama HOJE `falta` as 05:48 para 162 colabs usando teto por DATA (`'futuro': data > hoje`) -- defeito no juiz de STATUS, dono **O248 `juiz dia_encerrado`** (autorizada, NAO encomendada); (b) o residuo cross-meia-noite do teto por data na parcela de furo -- mesmo dono; (c) o cartorio lavra `veredito='nunca_bateu'` para ISENTO e `_classificar:161` deixa sobrescrever o `ok` do F5c, contra `ponto/supra_juiz.py:125-127` -- **linha de fila propria**, documentada em `test_05b`.

## 11/10 05:0x — R4 par 6, INCOMPLETO (a) FECHADO: as duas hipoteses do +6 eram FALSAS

LEI-AKITA: origem=a propria celula do R4 (hipotese nomeada e nao discriminada), testemunha=`folha/porta_export.py::dias_fora_do_dia_pago` e `folha/export.py::classificar_export` (as funcoes REAIS, na sombra), RED=n/a (medicao de discriminacao, nao cura), quem-mais-le=o campo `numero`/`prova` da celula R4 e o `!` R4-PAR6-DIFERENCA-DA-09-EXPORTADA, juizes novos=0

- **A pergunta**: a celula publicava "1 na 09, 6 na 10" (prova de 03/10) e hoje o contador diz 7 e 0. Ela nomeava DUAS hipoteses -- espelho moveu, ou universo moveu por codigo -- e as deixava NAO discriminadas.
- **PERNA 1, o gravado nao se moveu**: os 7 `DiaPago` estao lavrados 30/09 16:56-16:58 e, na frota, **0 das 11.475** linhas da 09 foi re-lavrada depois de 03/10 (L-092 cumprida). PROVA `logs/placar_estrutural/r4_par6_quando_20261011_0331.txt`, rc 0.
- **PERNA 2, o espelho nao se moveu**: a funcao REAL, os mesmos 4 colabs, a MESMA sombra, em **TRES arvores** -- `ca2d50d9` (03/10), `1f3d616f` (08/10, antes das curas de turno/motor) e HEAD `a96169b5` -- devolve **7** nas tres, com os MESMOS dias e os MESMOS valores. E o contador tem UM commit em toda a historia (`0a918855`, 29/09), anterior a propria baseline. PROVA `r4_par6_bisect_20261011_0335.txt` e `_0343.txt`, rc 0 nas tres.
- **PERNA 3, o codigo do universo nao move o universo**: `classificar_export` perguntado nas DUAS arvores contra a MESMA sombra responde IDENTICO -- 221 na 09 (1+149+59+12) e 19 na 10. PROVA `r4_par6_universo_20261011_0344.txt`, rc 0.
- **O que isso CORRIGE na celula**: ela dizia que col882 "saiu do universo na cura de 04/10". Nao saiu: na arvore VELHA ele **ja esta `fora`**, por `furo_espelho`; `98924ef0` so deu nome proprio a porta (`cadastro_zero`) -- que e exatamente o que aquele commit mediu no corpo ("UMA linha move"). Numero que envelheceu se TROCA, nao se apaga.
- **A resposta**: o que mudou e **DADO** -- fechamento novo e/ou RETENCAO que caiu. Quem retinha e `furo_espelho`, que le `motivos_retencao_celula` (CELULA; `folha/export.py:914` prova que `turnos_abertos`/`inconsistencias` sao IGNORADOS desde HX-BORDA-CELULA, e por isso col499 ENTRA com 4 turnos abertos e 10 inconsistencias). Os fechamentos de col290 (#4623, 30/09 16:57) e col146 (#4927, 28/09 00:46) JA EXISTIAM em 03/10 e estavam fora do par: caiu a retencao, ata lavrada entre 03/10 e 11/10.
- **O que NAO se afirma**: a MAGNITUDE do delta. Os dois arquivos de 03/10 discordam -- `r4_pares_20261003.txt` (15:34), o que produziu o "1 e 6" da celula, diz emp2 147/58/12 (e sob ELE o crescimento e +2 emp2 +1 emp3, CONSISTENTE com os tres colabs novos); `r4_pares.txt` (00:09) diz emp2 143 na 09 e 15 na 10, irreconciliavel com os 156 do outro na 10.
- **Consequencia para o `!` na mesa**: o par 6 da 09 EXPORTADA nao mede cura que escapou -- mede colabs cujo gravado discorda da autoridade que recalcula AGORA e que a porta so passou a mostrar quando a retencao caiu, o caso que a propria docstring de `dias_fora_do_dia_pago` preve. O `!` segue valendo, agora com procedencia.
- Discriminacao fechada em `logs/placar_estrutural/r4_par6_discriminacao_20261011.txt`. Banco=sombra (carimbo `dia=20261010`), SO LEITURA, cpuset estreito (cpu7) para nao disputar com a suite do comboio. Commit `6dcaf1a7`.
- **INCOMPLETO**: (i) QUAL ata, de que dia, derrubou a retencao de col290/col499/col146 -- a sombra so tem o estado de HOJE; (ii) qual dos dois arquivos de 03/10 mediu outra pergunta no universo da 10 (15 x 156). Os 7 REDs seguem abertos e NENHUM e curado -- a 09 esta EXPORTADA (RECALC-09).

## MARCO 11/10 04:4x — O148 FOTO-DE-FOLGA-CONTINUA: a foto do calendario deixa de depender de alguem abrir a tela, e a varredura e **ADITIVA** porque o escritor **RECONCILIA**

LEI-AKITA: origem=`escala/folgas.py::gerar_foto_folgas` (escritor unico da foto) + `escala/management/commands/lavrar_foto_folgas.py` (a lavra que passa a chama-lo), testemunha=`FolgaDia` e `EscalaColaborador.eh_dia_trabalho_calculado`, RED=`escala/tests/test_foto_folga_continua.py` (11 casos; os dois REDs de guarda medidos DESLIGANDO a cura), quem-mais-le=os 4 sitios do selo de escritor unico, juizes novos=0

- **O DEFEITO.** Os 3 chamadores de `gerar_foto_folgas` sao todos de TELA, entao o 6x1 sem `folga_dia_semana` chega ao mes nao visitado sem nenhuma `FolgaDia` e `eh_dia_trabalho_calculado` devolve **trabalho em todos os dias**. Dono **ESTRUTURA** (L-099): o cadastro declara o calendario e o sistema para de cumpri-lo.
- **A LINHA QUE A FATIA NAO PODIA FURAR.** `gerar_foto_folgas` **APAGA** toda `FolgaDia` de `calendario_origem` nao nulo do mes antes de recriar, e relavra as celulas: varredura que o chamasse em todo vinculo-mes apagaria **foto gravada por versao ANTERIOR do calendario** onde o template de hoje manda menos -- **65 datas em 16 vinculo-mes**, MEDIDO (nenhuma e manual: as 303 `FolgaDia` do universo saem todas de `calendario_origem`). As 16 estao **todas em 09/2026** (`sobra por mes: {'2026-09': 16}`), a competencia EXPORTADA, e em 21/10 a sobra e **0**: a guarda nao custa alvo nenhum hoje. A lavra so chama onde **FALTA**, nunca onde **SOBRA**, e re-pergunta a MESMA fonte imediatamente antes de cada escrita.
- **MUDA 2**: data com `FolgaDia` manual (`calendario_origem` NULL) nao entra no `bulk_create` -- a manual fica, sem `IntegrityError`. **UM JUIZ DE LEITURA**: `datas_que_o_calendario_manda` e fonte unica, e os dois resolvedores que ja existiam seguem com **um** chamador (selo por AST).
- **A JANELA E O ESCOPO LITERAL DO AVAL** (LEI-AKITA 9): *"corrente **e** seguinte"*. A 1a versao de `meses_da_janela` devolvia so `_janela_default(hoje)` -- a CORRENTE --, e **escrevia ZERO hoje**: a foto de novembro nasceria 05:47 de **21/10**, tres minutos antes do `gerar_celulas` que a LE, e o contador de template vazio **nao olhava novembro** durante toda a competencia em que ainda da tempo de cadastrar. RED: `[(2026,9),(2026,10),(2026,11)] != [(2026,9),(2026,10)]`. Cura sem juiz novo: o MESMO `_janela_default` perguntado **duas vezes**, a 2a em `fim+1 dia` -- o idioma de `ponto/janelas.py::janela_anterior` para frente.
- **CRON** `05:47 lavrar_foto_folgas --apply`, papel `lavra`, `depende=()`, **120 s**. O slot nao e gosto: a 1a versao dizia 05:30 e `test_ordem_dos_estagios_diarios` caiu -- o relogio estava livre, o ESTAGIO nao, e a madrugada termina 05:46.
- **MEDIDO NA SOMBRA 11/10, pela FUNCAO REAL `medir()`** (`current_database()=='sombra'` provado antes; PROVA `logs/foto_folga/janela_corrente_e_seguinte_20261011.txt`): janela de HOJE `(2026,9)(2026,10)(2026,11)`, universo **203 vinculo-mes**, **3 alvos / 19 datas** (todas em 11/2026: ec910=1, ec184=9, ec749=9), **16 bloqueados por sobra (65 datas)**, **132 cujo calendario manda ZERO** (57 em 09, 8 em 10, 67 em 11). Com a janela estreita davam **0 alvos**.
- **NA VIRADA DE 21/10, mesma funcao**: universo **213**, **66 alvos / 321 datas**, sobra **0**. TEMPO REAL do `--apply`: **1,9 s para 3 alvos = 0,65 s/alvo**, entao o pior caso ja datado da **~43 s** -- dentro dos 120 s declarados, com um minuto de folga antes do gerador. Idempotencia reconferida: `medir()` logo depois do `--apply` = **0 alvos, 0 datas**.
- **A PRIMEIRA MEDICAO que o aval pediu, respondida** (`logs/foto_folga/primeira_medicao_20261011.txt`): dos **71** do universo, **63 com foto** em 10/2026 e **303 `FolgaDia`** -- **303 de `calendario_origem`, ZERO manual** (misto 0). A *"colisao manual x calendario = 0"* que ele mediu em prod tem causa estrutural: nao existe UMA manual neste universo, entao a MUDA 2 fica como GUARDA com RED **sintetico**, declarado.
- **O QUE O NUMERO DIZ, e nao e o que a fatia esperava**: o passivo e **CADASTRO** com causa UNICA -- calendario `dias_mes` se preenche **por tamanho de mes** e a casa preencheu **um tamanho so** em cada um. Os 50+ com so o 31 caem em todo mes de 30 (**67 dos 71** em 11/2026); `cal#50`, com so o 30, **ja cai hoje** em outubro. Sao **8** sem foto em 10, nao 7: o oitavo (`ec910`, 12x36) o juiz **nao acusa, e esta certo** -- quem o ve e a vigia nova. Curar cadastro por codigo e proibido (**L-099**): vai ao contador e a lista do admin. Detalhe no TOPO.
- **REDs das guardas, medidos desligando a cura**: sem a MUDA 2, `IntegrityError: duplicate key ... (escala_colaborador_id, data)=(1, 2026-10-11)`; sem a guarda de sobra, `[] != [(1, 2026, 10, [4 datas])]`.
- **ESMERIL do proprio marco** (L-111): a allowlist do meu selo nasceu com **5** sitios e `vistos` so podia chegar a **4** -- `escala/folgas.py` define a funcao e nunca a chama. A linha saiu e a asercao virou **igualdade**, que morde nos dois sentidos. E o esmeril do COMBOIO, feito na espera da suite 2 e **so dele**: **0 orfao**. As duas raias sao ADITIVAS no codigo (O198 +30 linhas e nenhuma removida; foto 647/9), e as 9 removidas da foto sao o corpo que virou `datas_que_o_calendario_manda` -- `folgas_semana_dia` e `folgas_do_mes` seguem com chamador de producao em `escala/folgas.py:55-56`, entao a **L-111 nao cobra apagamento nenhum neste marco**.
- **A SUITE DO COMBOIO ACHOU UM DEFEITO DESTA FATIA, e nao foi eu**: sobre a arvore `678c1a73` (commit `a0ce5e84` = main `80b1db30` + O198 `50d2a6f6` + foto `8272b439`, **0 conflito**, **0 migration**) a bateria deu **`FAILED (failures=2, skipped=42)` / `Ran 10375 tests in 1406.119s` / rc 1**, as DUAS no MESMO selo: `core/tests/test_selo_diagrama_do_codigo.py`, com `DIVERGE: docs/ARQUITETURA.mmd nao bate com o codigo` (a 2a cai por consequencia -- ela restaura o arquivo e reconfere a base, que ja divergia). DONO MEDIDO pela funcao REAL `gerar_diagrama --check`, nao suposto: main rc **0**, O198 rc **0**, foto `8272b439` rc **1** -- e os tres `.mmd` com o MESMO md5 (`9dcab1d4`), logo mudou o CODIGO, nunca o arquivo. Causa: o cron novo entra em `config/crons.py` e o desenho sai do codigo por AST. Regenerado: no `c_lavrar_foto_folgas 05:47` no bloco `grade`, aresta `--> c_gerar_celulas` (o `depende` declarado), **58->59** nos e **43->44** arestas, `MAPA.md` inalterado, diff byte a byte igual nas duas arvores. A suite anterior, sobre `04bf5104`, foi **ABANDONADA DE PROPOSITO** na correcao do escopo da janela.
- **SUITE 2**: arvore `a6787fd3` (commit `3ecec8f8`), a do comboio **mais o derivado do diagrama** -- `Ran 10375 tests in 1423.240s` / `OK (skipped=42)` / **rc 0** (L-074, as tres pernas), log `logs/comboio/suite_comboio_20261011_0154.txt`. Ela **deixou de certificar** no instante em que o marco do R4 par 6 (`a96169b5`) moveu o main com um arquivo FORA de `app/docs` (`core/placar_estrutural.py`): a prova de pouso e *"diff contra a arvore certificada vazio em tudo que nao e docs"*, e ela nao fecharia. **SUITE 3, a que CERTIFICA** (SUITE-UMA-VEZ-POR-ARVORE): arvore ``3bf9c778`` (commit `02261de0` = `a96169b5` + O198 + foto, 0 conflito), `Ran 10375 tests in 1415.384s` / `OK (skipped=42)` / **rc 0**, log `logs/comboio/suite_comboio_20261011_0320.txt`. Em vez de pousar com a deviacao escrita, a espera do portao da sombra pagou a bateria inteira -- custo zero de relogio.
- **POUSO** (L-105 + L-107, um ato so cada): o O198 em **`2eb92a98`** (deploy `--sem-migrate` provado: prova de casca com 16 estaticos / 5 paginas / 611 rotas em 2 urlconf, tres cascas recarregadas juntas, tres rotas 200/302/200, `importerror_500=0`, selo BUG 128 verde) e a foto neste commit -- um commit nao cita o proprio sha, e ele sai no commit SEGUINTE do marco, como o `a96169b5` fez, o derivado do diagrama **regenerado DENTRO do commit do merge da foto** (L-106: docs no commit do MARCO, e a copia que o certificou nao pousa), PROVA `bin/gerar_diagrama.py --check` rc 0 depois do pouso -- a prova de arvore exclui `app/docs` de proposito, entao quem responde por ele e o `--check`; e depois `bin/crons.sh install` + `check` -- `bin/deploy.sh` **nao instala crontab** (grep: so a logica DEPLOY-ESPERA-CRON em `:255-258`), entao sem esse ato o cron nao existe.
- **INCOMPLETO, nomeado:** (a) os **5 RESULTADOS OBSERVAVEIS** do `docs/O148-COMPLEMENTO.md` nao estao provados em PROD -- so em sombra e fixture; (b) o item 1 deles (*11/2026 com foto nos 48*) **nao se cumpre por este codigo**: o template de 30 dias esta vazio, e isso e CADASTRO; (c) o **RESULTADO 3** (*"nenhum mes perde folga ja gravada"*) se cumpre por **NAO ESCREVER** nos 16 vinculo-mes com sobra -- escolha deliberada pela CURA-MAIS-RESTRITIVA, e nao por eles terem sido conciliados; (d) o **BACKFILL dos 7** com o `!` dele nao foi aplicado -- esta VAZIO como foi dado, ver o TOPO; (e) a LINHA HAIKU (a)(b)(c) nao foi ligada: o contador existe como `MetricaSnapshot`, nao como ferramenta do copiloto, e o documento do Fernando nao esta no repo.

## MARCO 11/10 04:4x — O198 FALTA-EM-DIA-SEM-JORNADA: a lei ja existia (**L-089**) e o que faltava era **esta porta perguntar** -- 10 dia-colab na frota, todos nascidos pela porta humana

LEI-AKITA: origem=`ponto/precedencia.py::veto_de_lancamento` (a porta de lancamento, escritora unica da recusa), testemunha=`escala/servico_jornada.py::escala_vigente` -> `EscalaColaborador.eh_dia_trabalho` (a MESMA autoridade que a celula e o motor leem, chamada pelo nome dela), RED=`ponto/tests/test_o198_falta_em_dia_sem_jornada.py` (235 linhas), quem-mais-le=o censo dos chamadores da porta mais os 3 sitios de `se_existir` que a PULAM (nomeados abaixo), juizes novos=0

- **O DEFEITO, e nao era regra faltando.** A **L-089** ja dizia *"ausencia que abona/desconta so alcanca dia que a escala previa"*, e o motor e a grade ja jogam a falta fora depois (`escala/utils.py::_marcar_ausencias`). O que sobrava para o admin era uma linha **aprovada que nao desconta nada e nao diz por que**: era o LEITOR QUE NAO MIGROU, e o leitor e a porta. Pela LEI-AKITA 4, a pergunta nunca foi "qual a regra".
- **A CURA, tri-estado, e o `None` e a metade que importa**: so o `False` EXPLICITO recusa. Sem vinculo que responda pelo dia, ou 12x36 sem ancora declarada, `eh_dia_trabalho` devolve `None` -- **limbo** --, e quem responde pelo limbo e o emissor de CADASTRO (L-099), nunca uma porta barrando por ignorancia. Lista `SO_EM_DIA_DE_JORNADA = ('falta',)`, curta de proposito: `falta` e o unico codigo que DESCONTA e `feriado_folga` existe no catalogo PARA dia sem jornada -- alargar daqui e regra de negocio nova, nao cura (e o `!` dele nao cobre).
- **O CENSO DA FROTA, pela FUNCAO REAL, em PROD por leitura** (role `leitor`, porta `bin/sonda_leitura.sh`, cpuset 4-7; prova `logs/o198/censo_falta_sem_jornada_20261010_1828.txt`, rc 0): das **187** faltas aprovadas que tocam a janela 21/08-20/10, **10 dia-colab** caem em dia sem jornada -- **10 pela CELULA e 0 pela aritmetica do vinculo**, **10 na competencia 09 e 0 na 10** (emp2 7, emp3 3), contra **182** com jornada que seguem aceitas como hoje. **0 dia da ausencia ficou fora das duas competencias**, logo nao ha dia nao julgado.
- **[nome] DOS 10: todos pela PORTA HUMANA**, nenhum por script (prova `logs/o198/nascimento_dos_10_20261011_0011.txt`): **2 usuarios** (u657 em 9, u395 em 1), e a descricao e **vazia em 4**, de **6 chars em 4**, de 9 em 1 e de 62 em 1 -- a trilha que existe nao explica nenhuma. **7 das 10 sao de UM colaborador** (col338), 2 de col515 e 1 de col502.
- **FORENSE: a celula nao mudou embaixo delas** (prova `logs/o198/forense_celula_20261010_1832.txt`): dos 10, **8 estao em celula NUNCA regenerada** (`regeneracoes=0`: col338 x7 e col502 x1) e so 2 (col515, aus3881/aus3883) em celula com `regeneracoes=4` -- ou seja, nao e o caso de "aprovou certo e o cadastro virou depois". **+1 classe que o censo separou e nao conta como caso**: aus4304 (col942, 16/09) nao tem **nenhum vinculo cadastrado**, celula inexistente -- dono **CADASTRO** pela L-099, vai para a lista do admin.
- **O EXEMPLO DO PROPRIO BACKLOG NAO E UM CASO**, e isso se mediu antes de escrever a fatia: col951 / 04/10 tem `eh_dia_trabalho = True` e celula existente (`CelulaDia#104716`, `regeneracoes=3`, `regenerada_em` 08/10 13:48, posterior a `Ausencia#4703` de 05/10) -- a falta dele e **legitima pela porta nova**. O BO de producao estava certo sobre a existencia do problema e errado sobre o exemplo.
- **O DIA E A UNIDADE, medido antes de escolher**: na janela ha **4** faltas de mais de um dia e **NENHUMA** toca dia sem jornada (`logs/o198/range_falta_20261011_0010.txt`), entao nomear o dia que ofende nao custa alvo nenhum e deixa o admin partir o lancamento em vez de adivinhar.
- **O `!` DELE, LITERAL, NAO FOI ALARGADO**: *"a Ausencia ja aprovada NAO se apaga nem se altera -- e decisao de admin"*. As 10 ficam onde estao; a porta so impede a **11a**. Nenhum dado tocado nesta fatia.
- **REDs e vizinhos**: `logs/o198/red_20261011_0012.txt` (o RED literal antes da cura), `green_20261011_0016.txt`, `vizinhos_20261011_0018.txt`, `green_selos_20261011_0026.txt` e as tres rodadas de `ponto`+`escala` (`suite_ponto_escala_20261011_00{22,33,38}.txt`). O veredito que CERTIFICA o pouso e o da suite do comboio, abaixo.
- **INCOMPLETO, nomeado:** (a) os **3 sitios de `se_existir`** que PULAM o veto -- `ponto/services/regularizacao.py:121`, `ponto/management/commands/marcar_abandono_provisorio.py:78`, `chamados/services/materializacao.py:430` (achado **B5-k**) -- seguem como linha de fila, nao curados aqui; (b) `codigo='sobreposicao'` e o UNICO rotulo que o unico leitor (`ponto/views.py:2607`) conhece, entao a recusa nova chega a tela pelo rotulo generico.

## MARCO 10/10 23:3x — O48 SEM-CELULA-NAO-E-ZERO: a grade DIZ `sem_celula` e a derivacao respondia **0**; o par controlado da o DIFF em **45 celulas / 9 colabs, todo movimento `0.0 -> None`**

LEI-AKITA: origem=`ponto/services/fechamento.py:570` (o filtro que absorvia a palavra da grade), testemunha=a GRADE (`escala/services/leitor_celula.py::grade_da_celula`, que ja rotula o dia), RED=`ponto/tests/test_o48_sem_celula_nao_e_zero.py` (3 casos, evidenciado), quem-mais-le=censo fechado abaixo, juizes novos=0

- **O ACHADO.** `grade_do_fechamento` devolve o dia rotulado `sem_celula`; o filtro `in ('trabalho','ausencia')` o absorve, `sum([])` da **0**, e `folha/export.py:791` lia esse 0 como zero hora realizada. Na 07 eram **131** colabs com `minutos_realizados=0` e `horas_trabalhadas>0` contra **2** coerentes.
- **A CURA.** `raise _GradeSemCelula()` roteado para o `except` que JA existe (`:611`), que nula os seis campos: UM escritor da resposta "sem resposta", nao uma segunda copia. IMPOSSIBILIDADE, nunca cobertura parcial -- UM dia com celula ja responde com o que existe (3o caso do selo).
- **O PAR CONTROLADO** (a unica forma de isolar a cura; o gravado envelheceu contra o motor e misturaria tudo). Mesma sombra, mesma competencia, unica variavel a cura, cada ponta se identificando sozinha por `hasattr(_GradeSemCelula)`: a cura move **45 celulas = 9 colabs x 5 campos** (`minutos_realizados`, `minutos_previstos`, `minutos_abonados`, `dias_previstos`, `dias_incertos`), **todo movimento `0.0 -> None`**, e **zero outro campo de zero outro colaborador** (universo lido 579, gravado 587). Coerencia: a ponta sem cura move 174 contra o gravado, a com cura 174+9=**183**. PROVA: `logs/sombra/o48_par_COM_CURA.out` e `o48_par_SEM_CURA.out` (15.054 linhas canonicas cada).
- **A 09, QUE ESTA EXPORTADA** -- medida porque a clausula 1 da L-113 caiu hoje (`1ceed412`) e recalculo passou a escrever exportada com trilha, entao a clausula 3 virou material. Mesma assinatura: **45 celulas / 9 colabs**, e os 9 tem `horas_trabalhadas=0.0` (colab sem fato, nao a contradicao). **Zero campo de dinheiro move**, nas duas competencias. E o que o TXT imprime **nao muda**: `folha/export.py:791-792` ja tem o ramo `None` e ele le `trab`, que nesses 9 vale 0.0 -- o mesmo numero que a mentira produzia. PROVA: `logs/sombra/o48_par_*_09.out`.
- **CENSO DE QUEM LE** (LEI-AKITA 5). Os cinco campos sao `null=True` no `FechamentoMensal` (`ponto/models.py:400-414`), entao o None e gravavel. Todo leitor de atributo em producao ja guarda: `folha/export.py:760/791` (`is not None`), `:766/793` (`or 0`), `relatorios/cartao_pela_celula.py:81-85` (`is not None`). A lavratura do `DiaPago` nao recebe esses campos, recebe os tres MAPAS, e o `raise` esta em `:570`, ANTES de `:586` onde eles se enchem -- chegam `{}`, e dia sem grade nao ganha linha: o "nao sei" declarado em vez de zero.
- **SUITE sobre a arvore certificada**: `OK (skipped=42)` / `Ran 10352 tests in 1410.781s` / rc do container **0**. Censo de arvore antes do pouso: o delta de `.py` entre a copia e a viva e exatamente `fechamento.py` (md5 `9232d64a`) e o teste novo (`466c4fe4`), copia nascida de `1ceed412` -- a arvore que a suite certificou e a que pousa.
- **POUSO** (L-107, num ato so): commit `3f87ede9` por PATH (9 arquivos; `crons_duracao.json` e `bin/sonda_frota.sh` ficaram FORA -- cron-dirty e INSTRUMENTO, que pousa sozinho pela L-105) e `bin/deploy.sh --sem-migrate` no mesmo sopro, dentro do carimbo de hoje (`dia=20261010 status=OK diverge=0`): prova de casca 16 estaticos / 5 paginas / 611 rotas, as tres cascas recarregadas juntas, tres rotas provadas, selo BUG 128 verde, `importerror_500=0`.
- **PROVA EM PROD, no worker vivo** (clausula 4 pelo caminho de LEITURA): `_GradeSemCelula` importavel dentro do `saas_core`, e `fechamento_lido` em dois dos nove (col43, col66) devolve `minutos_realizados=None`, `minutos_previstos=None`, `dias_incertos=None` -- nao mais 0. Leitura pura, dois colabs, zero escrita (a porta nunca chama `save`), e a assercao de banco `saas_hasner` no proprio script.
- **INCOMPLETO, nomeado:** (a) o GRAVADO da competencia aberta so se alinha quando `recalcular_por_evento` disparar (batida `ponto/registro_batida.py:136`, portas de celula/HE, validacao de chamado) -- nao ha cron que regue a aberta, entao essa data nao existe; o caminho de LEITURA ja esta provado em prod acima (batida `ponto/registro_batida.py:136`, portas de celula/HE, validacao de chamado) -- nao ha cron que regue a competencia aberta, entao ela nao tem data; (b) **174 de 579** colabs da 10 ja divergem do gravado SEM a cura -- envelhecimento do gravado contra o motor, vai para a fila com dono a medir pela L-099, e **nao e fatia desta**; (c) a secao de REVERSAO dos arquivos do par lista tambem campo de mesmo valor e tipo diferente (`Decimal('98.31')` x `98.31`) nos colabs que movem: ruido da sonda, nao movimento -- o censo numerado acima ja normaliza.

## CONFERENCIA 10/10 23:0x — O98: as **20 afirmacoes** do mapa antigo contra o vivo, **12 entram, 7 corrigidas, 1 nao entra**

LEI-AKITA: origem=nenhuma -- **ato de CONFERENCIA, zero linha de codigo**, testemunha=o VIVO no HEAD `1ceed412` (grep/sed ao vivo, nunca memoria -- secao 6 LER ANTES DE AFIRMAR), RED=nao se aplica, quem-mais-le=0, juizes novos=0 (L-005, TRAVA JUIZ-NOVO)

O texto dele das 23:2x e INSUMO sob a condicao literal (*"onde divergir, o VIVO manda"*), conferido na ESPERA da
suite da O48 -- tempo de grep, nao de CPU. Resultado em `docs/O98-MAPA-ANTIGO-ENTRADA.md`, com `arquivo:linha`:

- **12 ENTRAM** (A1 A2 A5 A6 A7 A10 A11 A18 limpas; A12 A13 A16 A19 com precisao acrescentada). Os dois numeros
  do fim conferem exatos: **300 s** ao vivo (`api/views_core.py:588-591`) e **tol_min=30** retroativo
  (`ponto/turnos.py:2009`), este ultimo mais ESTRITO que o texto -- so decide com `len(casados) == 1`.
- **7 CORRIGIDAS pelo vivo**: **A9** a "porta de sessao que barra nao-staff" **nao existe como middleware**
  (`grep is_staff core/middleware.py` = **0**; a cadeia da casa e `settings/base.py:60-63`); **A8** sao **TRES**
  cartoes, nao dois, e o 1o discriminador nao e o UA e sim `carimbo_app` (`core/utils.py:45`), com `gate_apk`
  antes dos outros dois; **A15** nao sao "dois kernels gravando" -- e **um escritor** (`colaboradores/services/
  aparelho.py:16`, E3) chamado pelas duas portas; **A20** mora em `chamados/services/materializacao.py:71`, nao
  em `chamados/models.py` (la so ha re-export); **A4** o `/webview/entrar/` roda na casca **UI** (app `core` !=
  casca CORE); **A17** o 64800 esta em `saas.py:108`/`prod.py:49`, nao em `base.py`; **A3** `/plano/*` nao chega
  ao UI, o Caddy o serve (`Caddyfile:34-37`).
- **1 NAO ENTRA**: **A14** (*"`is_staff` e semantica, nao permissao"*) nao se confere por grep -- sao **66 sitios**
  de producao e alguns DECIDEM tela (`core/portas_da_tela.py` x3, `chamados/views.py` x4, `core/views_usuarios.py`
  x9). Afirmacao normativa pede censo proprio: **linha de fila**, nao linha de glossario.

INCOMPLETO, nomeado: o `GLOSSARIO.md` em si **nao nasceu** -- a O98 segue esperando a VAGA da raia (a O228 inteira
pousou, mas o aval 11:0x abriu a familia turno nela). O que esta feito e a conferencia do insumo, que era o unico
passo que nao precisa de raia nem de nucleo.

## MEDICAO 11/10 03:0x — R4 par 6: o numero sai do **CONTADOR**, e ele diz **7 na 09**, nao 1 — e a secao de 10/10 22:3x comparava CAMPO com CAMPO

LEI-AKITA: origem=`core/placar_estrutural.py` (a celula R4, que afirmava "1 na 09, 6 na 10"), testemunha=`folha/porta_export.py::medir` (a 7a testemunha, uma chamada por empresa ativa, na sombra), RED=o numero publicado x o da funcao que JULGA, quem-mais-le=`bin/gerar_estado.py` e o topo do TICKETS, pelo `linha_do_placar()` (censo fechado), juizes novos=0

- **O NUMERO** (prova `logs/placar_estrutural/r4_par6_20261011_0223.txt`, rc 0, `banco=sombra`, carimbo `dia=20261010 diverge=0`): par 6 = **7 na 09** e **0 na 10**; o campo dizia **1** e **6**. Universo do TXT **221 na 09** (emp1 1, emp2 149, emp3 59, emp4 12) e **19 na 10**; o campo dizia 214 e 21 — e o arquivo de onde o 214 saiu (`logs/e6_cauda2c/r4_pares.txt`) diz emp2 **143** enquanto o `r4_pares_20261003.txt` do MESMO dia diz **147**: o universo tambem andou sem dono. Os OUTROS oito contadores da mesma chamada: **ZERO** nas duas, menos `gravado_discorda_da_propria_grade` = **1 na 09**.
- **as duas pontas mudam por razoes DIFERENTES.** Na 10 o 6 do col882 caiu por motivo LEGITIMO: ele SAIU do universo na cura de 04/10 (`classificar_export` responde `fora/cadastro_zero`, perguntado hoje) e o contador pula `status != 'entra'` (`folha/porta_export.py:329`). Os dias dele nao sumiram — viraram **10** e seguem divergentes FORA do portao (prova `r4_par6_linha_ou_zero_20261011_0157.txt`).
- **na 09 o 1 NAO envelheceu em silencio, e eu escrevi o contrario antes de medir.** Os sete gravados estao lavrados em **30/09 16:56-16:58**, TRES dias ANTES da medicao que publicou 1, e o `FechamentoMensal` de col290, col146 e col935 nao foi tocado depois dela (so o de col499, em 05/10 11:31) — prova `r4_par6_quando_lavrou_20261011_0238.txt`. Ficam **DUAS hipoteses NAO discriminadas**: universo por premissa que nao e o fechamento (e **nao foi** o `cadastro_zero` — o DIFF dele move **0** linha na 09), ou o lado **ESPELHO** movido entre as duas datas (candidato `3c42f82c`; do lado do universo, os candidatos datados sao os 6 commits de `folha/export.py`+`porta_export.py` desde `ca2d50d9`). **Um numero ja em maos, medido e que NAO e prova**: o universo por empresa era 147/58/12 em 03/10 e e 149/59/12 hoje — **+2 na emp2, +1 na emp3**, exatamente a contagem dos colabs novos por empresa (col290 e col499 na emp2, col146 na emp3). Linha de fila, nao conclusao.
- **a bissecao dos 18 commits desde `ca2d50d9` NAO cai**: ela cai **so para col935** (os dois lados dao o mesmo numero em 03/10 e hoje) e **CRESCE** para os seis casos novos, que sao maiores.
- **os 6 casos NOVOS, dono ESTRUTURA (L-099), maior primeiro** — provas `r4_par6_dono_dos_novos_20261011_0237.txt` e `r4_par6_universo_e_borda_20261011_0244.txt`:
  - **col290 20/09, 12,00 h x 0,00 — BORDA DA JANELA, classe NOVA** (nao e a do O48): 20/09 e o ULTIMO dia da competencia, entrada 19:00 e saida **21/09 07:00**, fora da janela 21/08–20/09; o espelho da o turno inteiro ao dia 20 e o `DiaPago#97762` grava 0,0 com previsto 720 e celula `trabalha=True`. **E a borda tem DOIS lados** (prova `r4_par6_col290_destino_na_10_20261011_0304.txt`): o dia **21/09**, que e da competencia **10, JA LAVRADA** (`FechamentoMensal#5339`, `previsto_em` 09/10 06:45, 15 linhas `dia/motor`), **nao tem NENHUMA linha de `DiaPago`** — as 12 h nao estao gravadas em **nenhuma** das duas competencias, e a 10 tambem nao as acusa porque a entrada (20/09 19:00) cai fora da janela dela e o par compara 0 com 0.
  - **col146 08 e 14/09, 9,00 e 8,99 h x 14,99 e 14,89 — `parear_turnos` em fatia de 1 dia**: as batidas do dia sao 06:30 **S** e 21:30 **E** e a seguinte e 09/09 06:30 S, logo o turno real e 21:30→06:30 = 9 h; o gravado pareou a cauda da vespera com a cabeca do dia (06:30→21:30 = 14,99 h) em dia que a celula diz `trabalha=False` e o previsto da 0. **~11,9 h A MAIS (5,99 + 5,90), ja dentro do TXT vigente da emp3.**
  - **col499 24/08, 4,01 h x 0,00 — O48 literal**: `DiaPago#104031` tem `minutos_realizados=241` (que E 4,01 h, certo) e `horas_trabalhadas=0,0` — dois campos da MESMA linha se contradizendo. Mais 26 e 30/08 com 1,8 e 3,0 min, cross-meia-noite com `min_real` 660 exato: arredondamento, nada mais afirmado.
- **o RED (a) segue ABERTO, com 7,8 min** (`10,97 h x 11,10 h = 0,13 h`) contra tolerancia de 1,2 min — e a borda INVERTE quem discorda da batida: as cruas somam 353 + 313 = **666 min**, que e EXATAMENTE o gravado (665,99 min); quem esta **7,73 min ABAIXO** da batida crua e o **ESPELHO** (658,27), e por que nao esta medido. **Nao estampei FECHADO.**
- **a 09 esta EXPORTADA** (`ExportacaoDominio` vigentes #27 emp2, #24 emp3, #25 emp4): **nenhum dos sete e curado aqui** (RECALC-09), e a diferenca entre o PAGO e o que passa a valer foi para PENDENTES como `!` com os dois numeros. PROIBIDO mexer na tolerancia para o numero cair.
- **esmeril deste marco** (ESMERIL-DO-MARCO, so o que ele deixou orfao): `folha/porta_export.py:81` colapsa **AUSENCIA e ZERO** — `lin is None` e uma linha zerada chegam ao contador como o mesmo `0.0` —, e e por isso que lavratura PARCIAL passa sem nome (col882: 10 dias com batida apuravel e **nenhum** `DiaPago`, contra 6 linhas `dia/motor` na competencia, enquanto `colabs_sem_lavratura` pergunta "NENHUMA linha no periodo"). A cura e contador PROPRIO, nunca fallback no comparador. Linha em `logs/fila/porta_export_81_ausencia_e_zero_dao_o_mesmo_numero_20261011.txt`.
- hash no commit SEGUINTE do marco (um commit nao cita o proprio). **INCOMPLETO**: (a) discriminar as duas hipoteses do +6; (b) quem e o `gravado_discorda_da_propria_grade=1` da 09 hoje (nao reaberto, por ESMERIL); (c) por que o espelho fica 7,73 min abaixo da batida crua no col935.

## MEDICAO 10/10 22:3x — R4 do PLACAR-ESTRUTURAL: o DONO do par 6 e **ESTRUTURA**, e os 8 min que a celula publica **nao existem mais na sombra**

LEI-AKITA: origem=nenhuma -- **ato de MEDICAO, zero linha de codigo**, testemunha=`e6_oraculo::dono_da_divergencia` + `minutos_do_oraculo` + `espelho_do_colab`, as funcoes REAIS que o comando chama (nao reconstruidas: a sonda espelha a janela `-12h/+14h` e a query de `Batida` do proprio caller), RED=nao se aplica, quem-mais-le=0, juizes novos=0

- **DONO = `'ESTRUTURA'`** para col935 / 05-09, perguntado AO JUIZ e nao deduzido: as 4 batidas do dia dao contagem PAR (logo nao e BATIDA pela lei do BUG-144) e `dias_cadastro_x_realidade` da competencia inteira vem **VAZIA** (logo nao e CADASTRO pela autoridade declarada). Pela L-099, **ESTRUTURA e o residuo, e e fatia.**
- **O NUMERO DA CELULA ENVELHECEU**: ela diz *"espelho 10,97 h x dia_pago 11,10 h = 8 min"*, e hoje na sombra os dois lados dao o MESMO: espelho `minutos_realizados=666` / `pago_h=11.1` / `realizado_h=11.1`, `pago_do_dia(DiaPago#93555)=11,0998 h`, oraculo **665,99 min** -- diferenca **-0,01 min**. E os 666 min sao EXATAMENTE o pareamento cru (18:52->00:45 = 353 + 01:45->06:58 = 313): o espelho parou de ancorar no marco de TEMPLATE que a celula nomeia como causa.
- **QUEM MOVEU: NAO MEDIDO**, e nao escrevo hipotese como numero. O universo esta datado: o numero da celula foi escrito em `ca2d50d9` (03/10 00:09) e desde ele ha **18 commits** em `espelho.py`/`bordas_realizado.py`/`turnos.py`/`escala/models.py`. O candidato mais forte pelo MOTIVO -- nao pela medida -- e `5bdf439c` **O142** (04/10 01:06, *"o dono do dia vem do vinculo DAQUELE dia"*), porque o dia 05/09 esta FORA do vinculo (EC#1186 e a admissao comecam em 07/09) e era dali que o marco de template vinha emprestado. Atribuir exige bisect na sombra: **linha de fila.**
- **CAVEAT que viaja com os dois numeros**: isto e SOMBRA, e esta sombra carrega o re-julgamento da O219 (1894 atas da 09). A confirmacao em prod e a conferencia de amanha no `06:28`, na mesma lista do marco de 19:xx. O dia nao tem celula nem ata, entao o re-julgamento nao pode ser o movedor -- mas "nao pode ser" nao e "medi", e por isso a palavra fica **sombra**.
- ACHADO DE CARONA, da MESMA familia da cura da O48 um nivel abaixo: `DiaPago#93555` carrega `horas_trabalhadas=11,0998` com `minutos_previstos=0.0` **e** `minutos_realizados=0.0`, num dia SEM celula. Nada visivel quebra hoje porque o espelho nao le esses dois campos -- mas dois campos da MESMA linha medem a mesma coisa e discordam, e o `minutos_realizados` do espelho e o do `DiaPago` sao **escritores DIFERENTES do mesmo nome**. Fila, nao agora (ESMERIL-DO-MARCO).
- PROVA: `logs/placar_estrutural/r4_dono_col935_20261010_2230.txt` e `r4_passo2_col935_20261010_2231.txt`; sondas em `logs/sombra/r4_passo1_medicao.py` e `r4_passo2_de_onde_vem_o_666.py`, as duas com a assercao `current_database()=='sombra'`.
- **R4 NAO E CARIMBADO FECHADO** (ordem dele de pe). O campo `numero` da celula se corrige no commit do PLACAR-ESTRUTURAL, **depois** do pouso da O48: a arvore que a suite certificou e a arvore que pousa (SUITE-UMA-VEZ-POR-ARVORE), e editar `core/placar_estrutural.py` agora descertificaria o pouso.

## MARCO 10/10 19:xx — A QUEDA DA CLAUSULA 1 DA L-113 E A VIRADA DA O219 NO MESMO ATO (a CONDICAO dele de 16:xx, na ordem que ela manda)

LEI-AKITA: origem=`ponto/services/fechamento.py::recalcular_fechamento_mes` (a recusa sai de onde ela nascia) e `ponto/services/cartorio.py::REGRA_VERSOES` (a declaracao), testemunha=`empresas_exportadas_no_escopo` -- a MESMA autoridade de antes, agora lida para TRILHA em vez de para barrar, RED=`ponto/tests/test_o80_selo_l092.py` (10 casos, o par anti-vacuidade da trilha evidenciado VERMELHO antes da cura) e `ponto/tests/test_o219_versao_da_regra.py` (22), quem-mais-le=os 3 `except` que traduziam a recusa, censados por AST, juizes novos=0

O QUE MUDOU — **21 arquivos: 12 de codigo e 9 de TESTE** (o aval dizia 4 selos; a superficie medida e 9)
- `recalcular_fechamento_mes`: saem a guarda, o `raise`, a classe `CompetenciaExportada`, o portao `permitir_exportada` (6 chamadores perdem o argumento) e os 3 `except`. ENTRA TRILHA: `WARNING` com competencia, empresas e motivo, e **so quando ESCREVE** -- `(not somente_leitura) or lavrar_dias_pagos`, o MESMO predicado do laco que lavra, nao um segundo juiz de "isto grava?". CENSO POR AST do que a queda deixou orfao (ESMERIL deste marco): **0** uso EXECUTAVEL de `CompetenciaExportada` e de `permitir_exportada` na copia inteira -- as 35 mencoes que sobram sao LAPIDE ou assercao MORDE, e `motivo_exportada` fica porque e ele que a trilha carimba.
- os 3 `except`, por papel: `ponto/views.py:1201` e `::recalcular_por_evento` TRADUZIAM a recusa e morrem inteiros; `ponto/management/commands/lavrar_dias_pagos.py:98` ROTEAVA para `retratar_exportada` e sobrevive por LEITURA DECLARADA da mesma autoridade -- comportamento identico, zero juiz novo.
- `cartorio.py::REGRA_VERSOES` = `((1, '2026-09'),)`, com lapide da tupla vazia. O consumidor e INTERNO (`:599` -> `:635`/`:710`): grep que exclui o proprio arquivo diz "nenhum chamador" e mente.
- testes: 4 selos INVERTIDOS com lapide (a asserção troca, o selo nao se apaga) e 5 vizinhos curados na ORIGEM -- o do `b5u` passa a PEDIR a regra a `versao_para` em vez de replicar a chamada do juiz, e o do `e5` passa a varrer **AST** (`ast.ExceptHandler`) em vez de TEXTO, 6a vez que esta casa paga por selo de texto.

OS NUMEROS — ensaio com `--apply` na sombra (`completa diverge=0`); PROVA `logs/o219/ensaio_virada_apply_v2_20261010_1902.txt`
- 09/2026: **1894** dias com ata movida, dos quais **465 movem MINUTO (+151.038 min)**; 10/2026: **1606**, dos quais **30 (-722 min)**. FORA-DO-HORIZONTE **0** em celula e em chamado; celulas novas=0 sumidas=0 de 124.399; **10** chamados emitidos; 458,8 s nas tres empresas.
- 2o METODO, e nao e DIFF de codigo (L-110): o o209/O218 mediu em **PROD, 05/10**, **424** divergentes de 17.330 na 09 e **+155.323 min** -- **-2,8%** contra os 465 / +151.038 daqui. Na 10 ele achou **2 de 7.859 (+192 min)**, e a propria casa afirmava "0 de 9.008": era falso, e esta corrigido no sitio.
- a 09 nao e so exportada: e **`marco=paga` nas TRES empresas** (`violacoes=['paga sem trancada']`). Pela O TXT E FOTOGRAFIA nenhum marco barra correcao provada, entao nao nasce parada; a diferenca entre o PAGO e o que passa a valer e **Pauta DP com os dois numeros**, e e ATO SEPARADO.
- a lampada da 09 JA e motor: `DiaPago` da 09 = **10.866** linhas, TODAS `recalcular_fechamento_mes`, **103** gravadas DEPOIS do 1o TXT pela porta declarada; e o ramo `retrato` que a decisao (B) preserva byte-a-byte **nunca disparou em prod -- 0 de 32.224**.
- **A QUEDA NAO MOVE UM NUMERO**: ela tira a barreira. Nada recalcula ate alguem chamar o recalculo, e **NENHUM TXT novo sai deste ato** (ordem dele, literal). DECISAO DE HORARIO, minha pela PAREI-SO-LEI (horario de cron e decisao TECNICA): o re-julgamento unico de ~26.861 atas **nao se adianta a mao** -- corre no `06:28 processar_cartorio --apply` declarado em `config/crons.py`. Adiantar seria EU tocar a competencia EXPORTADA por ato proprio, que e porta de `!`; o cron de pe e a consequencia que o aval dele comprou, com o DIFF publicado antes. Medido na sombra em 4 nucleos DEDICADOS, DUAS vezes: cartorio SOZINHO 379/67/12 s (458 s) e cartorio DENTRO do bloco da manha 396/70/14 s (480 s) -- em prod o nucleo e compartilhado, entao a emp2 deve passar por cima de `06:36 apurar_furos_diarios` e `06:40 reconciliar_perguntas_orfas`, os dois idempotentes e diarios (um dia de defasagem se refaz sozinho). 3a TESTEMUNHA (md5 nao basta: ele passaria por uma reescrita de valores IGUAIS): `FechamentoMensal.atualizado_em` e `auto_now=True` (`ponto/models.py:436`), entao todo `save()` o move. Corte = o mtime do proprio `/sombra/bloco.txt` (10/10 20:15:50, impresso pela sonda -- nao digitado por mim): **ESCRITAS DEPOIS DO CORTE = 0** na 09 (607 linhas, `atualizado_em` mais novo de 09/10 16:04) **e 0 na 10** (587 linhas) -- nenhum cron da manha grava folha, com ou sem barreira. 4a TESTEMUNHA, e a melhor porque nao e sonda minha: `vigiar_txt_dominio` (0 escrita no command) correu DUAS vezes na MESMA sombra, uma com a arvore de HEAD e outra com a da copia -- a arvore se identificando SOZINHA pela classe da barreira, nunca por rotulo meu --, e as saidas sao **IDENTICAS byte a byte**, com `VALOR MOVEU: 0 competencia(s), 0 matricula-competencia`. E ela desmente um susto MEU: os dois logs do vigia de blocos DIFERENTES divergiam em 4 linhas da propria 09 (`porta moveu` de 3 matriculas, 223->219 e 89->90) e eu quase atribui isso a queda -- o par CONTROLADO mostra que vinha do BLOCO mexendo em celula e ata antes do vigia correr, nao da QUEDA (o par exclui a queda, nao o codigo inteiro: a copia leva a virada tambem). Dois blocos sobre estados diferentes nao sao experimento; e a lembranca de que comparacao sem variavel isolada acusa o inocente. PROVA `logs/o219_escrita_depois.out`, `logs/o219_vigia_sem_queda.out` x `logs/o219_vigia_com_queda.out`, `logs/o219/tres_testemunhas_20261010_2053.txt`. O **8o alarme** do bloco tambem passou pelo par controlado, e nao e meu: o A6 do `supra_juiz` mudou de CONJUNTO entre os dois blocos -- `emp3/10 FURO_SEM_COBRANCA=1` saiu e **`emp4/09 COBRANCA_SEM_FURO=1` apareceu**, na 09, que e a competencia deste marco. `--encerrada --alarme` SEM `--executar` e dry-run (o `retratar` esta atras de `o.get('executar')`, `ponto/management/commands/supra_juiz.py:227`), entao rodei as DUAS arvores na MESMA sombra: `ALARME A6 emp4 2026-08-21..2026-09-20: {'COBRANCA_SEM_FURO': 1}` **identico** com e sem a queda, cada lado se identificando sozinho. **o alarme nao e da QUEDA** -- e o par so prova isso, nao prova 'estado': a arvore da copia leva tambem a VIRADA. Entao fui ao DIA. O juiz nomeia (`--dump`, leitura: o `ALARME` levanta SystemExit ANTES do dump): **col51, 2026-09-19, `realizado=240 >= previsto=240, 2 celulas casadas, mas ha 1 chamado vivo`**. A ata daquele dia (`CelulaDia#32868`, `julgada_em` com escritor UNICO `processar_cartorio`, `escala/models.py:1692`) foi julgada **10/10 20:28:00 -- DEPOIS do inicio do bloco** e ficou `cobrado`, enquanto a cobranca viva (`#29664`, `batida_ausente`) nasceu em **08/10**, ANTES do re-julgamento: e a forma literal da COBRANCA ORFA, e consequencia da VIRADA re-julgando as **152 atas da emp4/09** que o ensaio das 18:15 ja havia publicado -- nao da queda, que as outras tres testemunhas mostram inerte. E a guarda REAL responde o resto: `pode_executar_csf(#29664)` = **True** (nenhuma pergunta respondida), entao o **`06:54 --executar` retrata e o `07:29` nao alarma**. CONFERIR amanha nessa ordem. PROVA `logs/o219_a6_sem_queda.out` x `logs/o219_a6_com_queda.out`, `logs/o219_a6_dia.out`, `logs/o219_a6_ata.out`, `logs/o219_a6_guarda.out`. CONFERENCIA de amanha, pelo `cartorio.log`: `julgadas/carimbadas/emitidos` por empresa na FAIXA das duas medicoes -- **20069-20119/21175-21225/240-244 · 5727-5742/5777-5792/15-16 · 999-1000/999-1000/4-5** --, e o que as separa esta nos CARIMBOS, nao numa causa minha: a primeira correu as 18:15 sobre sombra `status=OK` -- o bloco da manha JA havia passado por ela, cartorio incluso, entao foi SEGUNDA passada do dia --, e a segunda as 20:15 sobre sombra `REFEITA`, como PRIMEIRA passada, dentro do proprio bloco. E a SEGUNDA que descreve amanha, porque o `06:28` de prod tambem sera a primeira passada do dia -- e a primeira com a regra nova. **POR QUE** a primeira passada julga 50 atas MENOS eu nao medi, e nao escrevo hipotese como numero. Mais ~**10** chamados emitidos de uma vez (4 `batida_ausente` + 6 `disputa_supervisao_manual`), medido pelo teto de `ChamadoColaborador.id` -- o `emitidos` do `fim:` conta EMISSAO JULGADA, nao linha nova.

REVERSAO (clausula 2 da CONDICAO) `logs/o219/reversao_09_20261010_1912.txt`: 607 linhas do gravado da 09 (emp1 3 · emp2 460 · emp3 122 · emp4 22), 24 campos numericos + `status` + `previsto_em`, md5 do snapshot `892768aa7803aa3e116d775e59c11332`, com `motivos_espelho`/`causa_espelho` por HASH; mais `logs/o219/reversao_09_20261010_1745.txt`, a foto de celula e o teto de `ChamadoColaborador.id`. SELOS: **68 de host VERDES** na arvore viva, ruff limpo nos 21 arquivos, SUITE CHEIA sobre a copia: `Ran 10349 tests in 1457.478s` / `OK (skipped=42)`. E a frase "nao move um numero" DEIXOU de ser afirmacao: o BLOCO DA MANHA inteiro correu na sombra com a arvore da COPIA -- a queda dentro -- as 10/10 20:42 (71 comandos, erro=0, alarme de negocio=8, diverge=0, sombra refeita 10/10 20:11), e o gravado da 09 ficou no MESMO md5 `892768aa7803aa3e116d775e59c11332` (o mesmo SELECT da reversao, 607 linhas), com 0 comando do bloco carimbando a trilha da queda. E esse md5 e O DE PROD: o snapshot de reversao tirado em saas_hasner deu o MESMO md5 sobre as mesmas 607 linhas, entao o numero que nao se move e o numero real, nao um numero de sombra. PROVA `logs/o219_hash_antes.out` x `logs/o219_hash_depois.out`.

INCOMPLETO (fila, nao trabalho agora)
- a clausula **2** da L-113 segue de pe, e de proposito: `HX-REGEN-NAO-TOCA-EXPORTADO` e TRAVA em `escala/services/leitor_celula.py:235` e `ponto/portas/celula.py:88`, fora do aval desta queda. A clausula **3** caiu por ASSIMETRIA -- nunca teve codigo proprio, so o texto do `raise` (`fechamento.py:82`).
- os **+41** dias entre os 465 daqui e os 424 do o209 sao **HIPOTESE** (batida nova em 5 dias de intervalo), nao medida.
- a 10/2026 tem **18** dias que movem minuto em 21-30/09 contra **2** do o209 na sub-janela que se sobrepoe: nao reconciliado.
- a reversao **nao cobre** o conteudo anterior da lampada: ele existe so como `min_antes` em 3.500 linhas DET e hashes por campo.

## MARCO 10/10 17:3x — RECALC-09: O DIFF DE IMPACTO DA 09 EXPORTADA, PUBLICADO E **NAO APLICADO** (clausula 1 da CONDICAO dele de 16:xx)

LEI-AKITA: origem=`ponto/services/fechamento.py::recalcular_fechamento_mes` -- a MESMA derivacao que grava quando grava, chamada com `somente_leitura=True`, testemunha=o GRAVADO da 09 (`FechamentoMensal`, a foto que virou TXT) contra o LIDO de hoje, RED=nao se aplica -- **ZERO linha de codigo mudou**, e a nao-escrita e de CONSTRUCAO, nao de promessa (role `leitor` com ZERO grant de escrita + `objetos lidos COM pk = 0` nas tres empresas), quem-mais-le=0, juizes novos=0

O ATO: `bin/sonda_leitura.sh --sombra` (cpuset 4-7) sobre sombra `tipo=completa diverge=0 erros=0`, UMA chamada por empresa de `recalcular_fechamento_mes(9, 2026, empresa_id=e, somente_leitura=True, retornar_objeto=True, permitir_exportada=True, motivo_exportada=...)` -- a porta da L-113 deixou os tres `recalculo_de_competencia_EXPORTADA por porta` na trilha, como tem de deixar. **PROD nao foi tocado, nada foi aplicado, NENHUM TXT foi gerado.** `delta = lido - gravado`, os **26** campos de `CAMPOS`, so `colXXX` (nome e CPF suprimidos por filtro de log: `erro_colab=0 outros=0`, nada a suprimir de fato).
PROVA: `logs/recalc09/recalc09_20261010_1731.txt` — rc=0, 17:31:26 -> 17:33:17.

O NUMERO — dos **603** colabs presentes nos DOIS lados: **IGUAIS 360 · MUDAM 243 (40,3%)**
- emp2 gravado 460 / lido 473 (84,1 s) · ambos 459 · **IGUAIS 248 MUDAM 211**
- emp3 gravado 122 / lido 123 (22,0 s) · ambos 122 · **IGUAIS 92 MUDAM 30**
- emp4 gravado 22 / lido 22 (3,6 s) · ambos 22 · **IGUAIS 20 MUDAM 2**

O QUE MAIS SE MOVE (emp2, por campo, `colabs`/`delta`): `minutos_realizados` 93 / **+32.251 min** · `horas_noturnas` 84 / **-2.260,68 h** · `horas_atraso` 51 / -29,77 h · `horas_saida_antecipada` 42 / -68,32 h · `horas_trabalhadas` **31 / -4,51 h** · `minutos_abonados` 10 / +19.580 · `horas_folga_trabalhada` 11 / +123,01 h · `saldo_banco_horas` 15 / +94,51 h · `minutos_previstos` 7 / -5.510. Na emp3: `horas_trabalhadas` 6 / +131,11 h e `horas_folga_trabalhada` 2 / -141,23 h. Na emp4: so `horas_atraso` 2 / -0,53 h.
LEITURA DA TABELA, e so isso (a CAUSA de cada campo nao esta medida, e nao se adivinha aqui): o que mais anda e **geometria** (`minutos_realizados`) e **rubrica noturna**; a `horas_trabalhadas` da emp2 anda **-4,51 h em 31 colabs** -- ou seja, o total de hora trabalhada quase nao se move, e quem se move e a CLASSIFICACAO dela.

UNIVERSO, nomeado e nao curado (L-099: isto e CADASTRO, nao ESTRUTURA)
- `so_gravado=1`: `col305` (desligado 2026-02-24) -- e exatamente o RED (b) que o R4 do PLACAR-ESTRUTURAL ja tem aberto, com `minutos_previstos` 13.020 x grade 0. Nao e buraco da porta.
- `so_lido=15` (14 na emp2 + `col980` na emp3): colabs que o recalculo de hoje PASSARIA a cobrir e que nao tem linha gravada na 09. `motor_falhou=0` e `SEM EXPLICACAO=0` nas tres empresas -- nenhum colab caiu no `except` que confunde "motor falhou" com "fora do universo".

O QUE ISTO E, PELA L-110: **medida de IMPACTO, nunca validacao.** O caminho velho (o gravado da 09) se le UMA vez, aqui, so para dizer QUEM muda de valor e QUANTO. Nenhuma linha acima certifica o calculo novo -- quem certifica sao os casos e as propriedades fixas, e isso e outro ato.

INCOMPLETO (fila, nao trabalho agora)
- a CAUSA por campo (quais curas pos-export -- L-084, folga trabalhada, noturno -- respondem por quais dos 243) nao esta medida; a tabela acima nomeia o universo, nao a origem.
- os **15** `so_lido` sao pergunta de UNIVERSO da 09, nao de porta: se a competencia for relavrada, eles entram pela primeira vez.

## MARCO 10/10 16:3x — O48 PASSO 2: A PORTA DE LEITURA DO FECHAMENTO NASCE SEM SEGUNDO DERIVADOR, E A PROVA DE QUE NAO HA SEGUNDO E O DIFF DE FROTA ZERO

LEI-AKITA: origem=`ponto/services/fechamento.py` (a propria `recalcular_fechamento_mes`, que passa a saber devolver OBJETO -- nenhuma derivacao nova), testemunha=a MESMA derivacao que grava quando grava, RED=`ponto/tests/test_o48_fechamento_lido.py` (4 casos; o 2 morde `objects.get` disfarcado de leitura e o 3 morde escrita medida no banco), quem-mais-le=0 consumidor novo nesta fatia (a porta nasce sem leitor; o passo 3 e quem a le), juizes novos=0

TOPO FECHADO: a pergunta de lei do backfill de folga em LOTE foi RESPONDIDA pelo aval dele de 16:0x e virou a **O246**; o censo que a abriu (340 / 265 / 100 / 135 / 23 / **112 dias em 60 colaboradores** / 103 ACEITA / 9 vetados) mudou de casa para a celula da O246 no BACKLOG, pela L-109 (mover, nunca apagar).

O QUE MUDOU
- `ponto/services/fechamento.py:1007` nasce **`fechamento_lido(colaborador, mes, ano)`**: devolve o `FechamentoMensal` DERIVADO AGORA, instancia DESTACADA (`pk is None`, entao um `save()` seria alteracao muda e nao acidente), e `None` -- nunca casca vazia -- fora do universo. Ela nao tem UMA LINHA de derivacao propria: delega a `recalcular_fechamento_mes(..., somente_leitura=True, retornar_objeto=True)`. **UM JUIZ, NAO DOIS**, que e o PROIBIDO literal do aval. Competencia exportada LEVANTA `CompetenciaExportada` de proposito -- a porta nao passa `permitir_exportada`, entao ela nao e a terceira porta da L-113.
- `recalcular_fechamento_mes` ganha `retornar_objeto=False` e a SEMENTE do ramo `somente_leitura` passa a copiar a linha INTEIRA do gravado, nao so `status` e `inconsistencias`.
- `ponto/tests/test_s4_ninguem_recalcula_para_ler.py:43`: so o MOTIVO da linha permitida, que passa a nomear a porta. `len(PERMITIDO)` segue **12** -- a fatia nao abriu excecao nenhuma.

OS NUMEROS, MEDIDOS NA FONTE
- **DIFF DE FROTA = 0**, pela porta unica da casa (`bin/simular_folha.sh par o48p2`), competencia **10/2026** na sombra (carimbo `dia=20261010 status=OK tipo=completa diverge=0`), as duas fotos dentro de **UMA** trava: emp2 133 linhas, emp3 70, emp4 5, o `sha` de cada empresa **IGUAL nos dois lados** (`dcec03c1dcdc` / `8bfd0c0fecb2` / `b47fa40cecd9`), `entraram=0 sairam=0 mudaram=0`, `TXT=0 RETIDOS=0`, `comparar rc=0` (rc 5 seria foto de trava diferente).
- RAIO DE ALCANCE da semente, por **AST** e nao por argumento: `recalcular_fechamento_mes` tem **27** atributos escritos, **37** leituras e **4** leituras-antes-de-escrita (`inconsistencias`, `status`, `.save`, `.pk`) -- **as MESMAS 4 no HEAD e na cura** --, e os dois campos de dado entre elas sao exatamente os que o HEAD JA copiava do gravado. O `save()` mora no `elif` do MESMO `if somente_leitura:`: o caminho que grava dinheiro nao foi tocado, e isso se mediu antes de commitar, nao se argumentou.
- SUITE CHEIA sobre a copia certificada por md5 (`abded237f546df7ff42f6b6f4712dd90`): `Ran 10344 tests in 1418.789s` / `OK (skipped=42)` / rc 0, zero `^FAIL:`/`^ERROR:`. **+4** contra os 10340 do pouso da O228 -- os 4 casos novos correram DENTRO dela, nao ao lado.

INCOMPLETO (linha de fila, nao trabalho agora)
- o numero de FROTA da porta em si (lido x gravado, colab a colab) sai **depois** do deploy: `bin/sonda_leitura.sh --sombra` monta a arvore VIVA (`/app:ro`), e e o commit que poe `fechamento_lido` nela. A sonda esta pronta e RECUSA a arvore sem a porta em vez de medir outra coisa.
- `ponto/services/fechamento.py:717-722` faz "o motor falhou" e "fora do universo" responderem os DOIS `None`, e os confunde num so balde.
- a diferenca **26 x 28** entre `CAMPOS` do `aplicar_09_corte_b` e os campos concretos do modelo -- e o fato de um dict de servico importar a sua lista de campos de um management command de uma vez.

FROTA DA 10/2026 E A PROVA NO AR (o INCOMPLETO de cima, fechado neste commit)
- **lido == gravado em 583 de 583**: `fechamento_lido` colab a colab contra o `FechamentoMensal` da sombra, **IGUAIS nos 26 campos = 583 e DIFEREM = 0**, lista de campos divergentes `[]`. E a frase do RED do aval ao pe da letra, e isto tem de ser dito: o `par` das **16:26-16:31** rodou `--recalcular` na mesma sombra, entao o "gravado" da 10/2026 **E** a lavratura derivada -- isto e *"lido == gravado logo apos recalcular"*, nao comparacao contra o gravado de prod. Custo medido: lote de 583 em **107,5 s** (0,184 s/colab), **UMA** chamada da porta em **0,19 s**, `pk=None` confirmado, porta-de-um-colab x lote divergem em **0** campo(s), colab fora do universo -> `None`. No ar por `bin/deploy.sh --sem-migrate` rc=0 com as **tres rotas provadas** (core `/health/` 200, ui `/colaboradores/` 302, mensageria `/health/` 200), selo BUG 128 verde nas tres cascas, `importerror_500=0`.
- os **4** `so_gravado` sao UNIVERSO, nao buraco da porta, e isso se mediu com o PROPRIO predicado da funcao em vez de um filtro meu: **4 de 4** sao `desligado` com demissao ANTERIOR a `2026-09-02`, que e o inicio de escala que o filtro de `recalcular_fechamento_mes` usa -- fora do laco, e o `None` e o contrato escrito da porta, nao falha de motor engolida pelo `continue`. Fica a linha de fila: os 4 seguem com linha `status=aberto` no gravado de uma competencia que o recalculo ja nao cobre.

## MARCO 10/10 15:2x — O228 POUSA INTEIRA: O PRODUTO E O INSTRUMENTO EM ATOS SEPARADOS, E A CATRACA SE MEDE EM COPIA PORQUE O CENSO LE A ARVORE ONDE MORA

LEI-AKITA: origem=o pouso (nenhum sitio de producao tocado por mim; o codigo e o da raia), testemunha=a suite cheia sobre a copia certificada por md5 e o proprio selo de host do censo rodando de dentro da copia do main de destino, RED=`bin/tests/test_esmeril_turno_censo.sh` (mordida 4 -> 5 com sitio plantado), quem-mais-le=0 (o instrumento e `bin/`, que o container nao monta), juizes novos=0

- **PRODUTO `31fce87c`** (L-107, ato unico): a suite cheia correu na copia certificada — `Ran 10340 tests in 1391.244s` / `OK (skipped=42)` / `rc=0` / 0 FAIL-ERROR (10343 achados menos 3 em quarentena). O merge devolveu **rc 1 POR MEDIDA**, nao por falha: os dois conflitos sao `RELATO.md` e `TICKETS.md`, exatamente onde o pouso do O199 acabou de inserir. Por isso a guarda do ato julga a **lista literal de nao-mergeados**, nunca o rc — a forma do O199 (`|| morre`) teria abortado NO MEIO, deixando `MERGE_HEAD` de pe e marcador de conflito em doc que **e** o bind-mount. Os tres docs vieram da copia que a suite rodou, resolvidos por **UNIAO** (`--ours` apagaria a linha da raia, `--theirs` reverteria o marco do main). **md5 5 de 5** iguais a copia. Deploy `--sem-migrate` rc=0, tres rotas provadas, selo BUG 128 verde, `importerror_500=0`.
- **INSTRUMENTO (neste commit)** em ato PROPRIO e sem deploy (L-105: *"bin/, selo de host e portao do push vao em pouso PROPRIO"*; `bin/` nao vai ao container). 5 arquivos, +434/-4, zero doc de produto.
- **A DECISAO TECNICA que vale guardar** (PAREI-SO-LEI: decisao tecnica o Code decide e registra). A catraca TEM de ser remedida na arvore de destino — catraca que pousa sem remedicao nasce VERMELHA no primeiro pre-push (precedente `cb6cd342`). Mas `bin/esmeril_turno_censo.py` **nao tem `--dir`**: deriva a raiz do proprio `__file__` (`:48`,`:51-52`), e o selo de host do proprio `BASH_SOURCE` (`:25-30`, que PROVA `[ -d "$RAIZ/app/ponto" ]` antes de afirmar). Ele so mede a arvore onde FISICAMENTE mora, e gravar o script em `bin/` fora do ato do commit e proibido. Saida pela lei da casa: **"a arvore onde ele mora" pode ser uma COPIA** — `git archive HEAD` do main JA com o produto, mais os 5 de `git show 61c83e49:`, com o selo rodando de dentro. Verde: **4 sitios sem chamador, igual a base**, e a mordida subiu 4 -> 5. A janela do merge ficou so com o que pode falhar POR CAUSA do merge. Bonus medido: o sitio que o selo planta (`app/ponto/_selo_turno_mordida_tmp.py`) nasce e morre na copia, nunca no bind-mount vivo.
- **A catraca SO DESCE**: o veredito e o selo, nao um diff do `.txt` a mao. Verde -> pousa como a raia o tem. Vermelho que DESCEU -> remede e certifica por md5; que SUBIU -> **PARA**, porque copiar o numero novo por cima e mudar o teto para ele caber, o PROIBIDO da L-099.

- **RESPOSTA AO AVAL DO RELATO RETIDO (ele, hoje: *"ponha a PROVA ao lado de cada ato ou troque a palavra pelo estado real"*).** A linha retida era a `:149` e ela **nao afirmava ato**: o token de fechamento estava dentro da PROSA que explica por que **nao** escrever aquela palavra na celula do portao -- mesma familia do `commit-que-explica-parado-cita-parado` (o texto que explica o alarme virou o alarme). Das duas curas que ele ofereceu escolhi a SEGUNDA, trocar a palavra pelo estado real, porque **ato ali nao houve** e uma linha `PROVA:` seria carimbo para ato inexistente -- o oposto do que o portao defende. 1 sitio trocado, sentido preservado. **PROVA:** `python3 bin/relato_afirma_com_prova.py app/docs/RELATO.md app/docs/RELATORIOS-PLANO.md` -> `afirma_com_prova: OK (2 arquivo(s), 0 afirmacao sem prova)`, **rc=0** -- os dois arquivos que o `bin/relato.sh:72-73` confere.
- **OS DOIS AVAIS DE HOJE FORAM RECEBIDOS, e o registro e a prova, nao a minha palavra:** `RECALC-09-PARA-AUDITORIA` esta em `app/docs/PROMPTS.md:1132` (10/10 13:5x, **complemento da O219, nao id novo**, com a decisao tecnica da porta da exportada ja escrita -- `somente_leitura=True` + `permitir_exportada=True` com motivo -- e o **QUANDO** dele, *"depois do deploy que esta esperando a suite"*, agora cumprido); o **CORTE DE ORDEM da O48** esta em `:1133` (10/10 14:0x), registrado onde o corte manda pela **O158**: o marcador de autoridade `app/docs/BACKLOG.md:7` le `<!-- ORDEM-VIVA-TOPO: O48 -->`, movido de O219 para O48, e o censo de 20 dias do `FECHAMENTO-ONLINE.md` ja foi relido contra o HEAD (achado: `fechamento_lido` tem **0** referencia `.py` em todo o `git log --all -S` e nas 3 raias vivas; `fechamento_em_memoria` andou `:534` -> `:974`).

**INCOMPLETO, em lista:**
1. **FILA (achado do esmeril deste marco, com prova):** `escala/utils.py:210::calibrar_ancora_se_aplicavel` e um `return 0` cuja PROPRIA docstring declara a condicao de saida (*"Mantida como no-op ate F2b remover o ultimo chamador"*) — e **a condicao esta cumprida**: 0 chamador de producao por AST (os 3 hits sao o `def`, a docstring do modulo e uma lapide em `:1558`), 0 referencia dinamica. Agravante: `escala/utils.py:3-6` ainda AFIRMA que ele e *"chamado pelos endpoints que criam Batida"* — falso duas vezes (nao ha chamador; nao escreve nada) e ensina que batida escreve a fase, que o corte F2 proibe. Pela **L-111** ele se apaga com lapide, levando as **3** assercoes que o chamam (`test_ancora_catalogo_premissa.py:79`, `test_contract_fase_declarada.py:100`, `test_f2_batida_nao_escreve.py:28`) e **mantendo** as **2** que mordem o CHAMADOR (`test_contract_fase_declarada.py:110`, `test_f2_batida_nao_escreve.py:41`, ambas `assertNotIn`) — essas selam o CORTE, nao a funcao. Companheiros pelo mesmo criterio: `minutos_previstos_periodo:1563` (0 chamador, e `core/placar_estrutural.py:288` ja o chama *"derivacao PARALELA"*) e `pausas_de_celulas:112` (0 chamador). **A catraca da raia rotula os tres `[FORA DE ALCANCE]`**, entao isto e achado de FORA deste marco e vira linha, nao trabalho agora (ESMERIL-DO-MARCO). Prova em `scratchpad/o228/fila_calibrar_ancora.md`.
2. **PLACAR-ESTRUTURAL R6 item (4), medido e NAO aberto** (L-105 nao deixa abrir fatia antes do pouso; L-099 manda medir primeiro): `config.crons::juizes_por_varredura()` = **25** = `JUIZES_POR_VARREDURA`, **identico** aos 25 de 08/10 — **0 deriva**, perguntado a funcao real. **25 de 25 tem arquivo de command** (0 fantasma) e **0 de 25** declaram o papel de VIGIA na propria docstring/`help`, que o tabuleiro (CLAUDE.md 4a) exige. **O que NAO vai como numero:** tentei medir *quem dos 25 escreve* e o criterio pela FORMA errou nos DOIS sentidos — `core/utils.py::get_user_sistema` e so `get_or_create` do usuario sistema e sozinho acusa 7 falsos; e `flip_automatico`, que o CLAUDE.md 7 declara *"escritor unico flip_tipo"*, escapa porque escreve um nivel abaixo (`:51`,`:65`). A autoridade e a **PORTA** da familia, nao profundidade de import. Medida em `scratchpad/o228/medida_r6_item4.md`.
3. **PEDIDO A ELE:** a O228 pousou INTEIRA, entao a **vaga da raia abre para a O98 GLOSSARIO** pela ordem de 11:4x — e ela depende do texto do mapa antigo, que ele disse colar quando a raia abrisse. **Cola o texto do mapa?** Sem ele a O98 nao comeca.

## MARCO 10/10 13:4x — O228 METADE TURNO: O CENSO DA FAMILIA NASCE, E O 24x48 RECEBE O ENVELOPE QUE O MOTOR DECLARA

PERGUNTA DE LEI (L-117, e ela nao devolve turno): `ponto/turnos.py::batidas_apuraveis(colab, ini, fim)` EXIGE janela, e os DOIS sitios que alimentam `parear_turnos` com universo CRU querem historia INTEIRA — `ponto/utils_turno_orfao.py:26` e `ponto/management/commands/reconciliar_fantasmas.py:85` (cron `*/15` com `--apply`, `config/crons.py:279`). MEDIDO na sombra: **81** colabs com batida retratada, **58** deles com geometria de pareamento divergente, **10** dia-turno em que o universo cru diz FECHADO e a apuracao diz ABERTO, **35** ao contrario, **113** pares `(dia, aberto)` divergentes. E o filtro **nao e monotono**: **107** `E` e **~97** `S` retratadas, e tirar uma `S` retratada REABRE turno. Ou a autoridade ganha forma sem janela, ou os dois sitios ganham janela declarada — as duas sao REGRA NOVA, entao nenhuma se escolhe aqui.

LEI-AKITA: origem=`ponto/turnos.py:310` (o ramo `_ENVELOPE_SS` lia o literal `14 * 3600` onde os outros dois ramos da MESMA funcao leem o parametro `cont_max_s`), testemunha=o envelope que o CHAMADOR declara — `MotorPlantao.AUT_CONT_MAX_S = 26 * 3600` (`ponto/motor_calculo_v2.py:2752`, *"volta do plantao dista >14h da saida"*), RED=`ponto/tests/test_marca_intra_duracao_invariantes.py::EnvelopeSSLeOEnvelopeDoChamadorTest`, quem-mais-le=censo do literal FECHADO (1 de 1 em corpo de funcao em `app/`; as outras 6 ocorrencias sao DEFAULT de assinatura, que e o valor do MotorBase), juizes novos=0

O QUE MUDOU
- `ponto/turnos.py::_marca_intra_duracao` — o ramo `_ENVELOPE_SS` passa a ler `cont_max_s`; as duas prosas que diziam "14h" passam a dizer de quem e o envelope (LEI-AKITA 11). 1 token de comportamento, 3 hunks.
- `bin/esmeril_turno_censo.py` + `bin/esmeril_turno_censo_de_hoje.txt` + `bin/tests/test_esmeril_turno_censo.sh` — o censo da familia **turno/marcos** NASCE, na forma do par da familia celula, com DUAS catracas.
- `bin/tests/test_commit_so_o_declarado.sh:52` **e** `bin/index_vs_arvore.sh::--conferir` — os DOIS curados: liam `.git/hooks/` da raia, e em worktree `.git` e um PONTEIRO de 58 bytes. O segundo e o PIOR dos dois: `bin/pre-push.sh:19` o chama, entao ele barrava o push de QUALQUER raia (rc **1** em `wt-celula`, rc **0** em `saas-hasner`, MESMO commit), e a prosa do ALARME mandava `printf > .git/hooks/pre-commit`, que na raia SOBRESCREVE o ponteiro do worktree. Morde nos tres casos: sem hook, hook mudo, hook certo.

OS NUMEROS, COM A PROVA AO LADO
- censo turno, que NASCE hoje: **16** modulos declarados por pergunta, **251** def de producao, **247** com chamador, **4** em ZERO; e a 2a catraca, **11** juizes declarados na superficie e **0** sem chamador fora de `core/juizes.py`. PROVA: `bash bin/tests/test_esmeril_turno_censo.sh` → OK (4 = base) e MORDE (sitio plantado: 4 → 5), rc 0.
- a cura, RED PRIMEIRO: `Ran 10 tests in 0.001s` + `FAILED (failures=1)` com a mensagem nomeando o sitio (`logs/o228turno/red.log`) → depois `Ran 10 tests in 0.001s` + `OK` (`logs/o228turno/green.log`). Os outros 9 passaram nas duas rodadas: a mordida sao DOIS valores dando DOIS status.
- impacto de FROTA, medido na sombra: **0**. Defeito PROVADO e LATENTE — **5** templates de plantao (`TPL-24x48`, `PAI-PLANTAO.1`, `PAI-PLANTAO.2`, `PAI-PLANTAO`, `PAI-CUSTOM.7`) e **0** vinculo ativo e **0** vinculo de QUALQUER vigencia; ativos por `tipo_base`: 12x36=**344**, comercial=**162**, turno_partido=**30**, intermitente=**10**.
- a origem, datada: `6af9b27d` (21/08 21:04) e o commit que CRIOU `cont_max_s` e converteu **2 dos 3** usos, deixando o terceiro literal; o bloco `_ENVELOPE_SS` nasceu antes, em `ce736f62` (04/08), sem parametro nenhum. **50 dias** de migracao pela metade.
- pasta de selos de HOST na raia: **68** selos, **6** VERMELHOS e todos os 6 VERDES no main no MESMO commit — RED FALSO de worktree. **1** curado aqui, **5** em fila.
- bateria da raia pela porta unica, contra COPIA, banco `test_juliani_celula`: **`Ran 10312 tests in 1390.537s`** + **`OK (skipped=42)`** (`logs/o228turno/full.log`, forma da casa com `tail -1`). O rc do PROCESSO NAO foi capturado — o lancamento redirecionou so o stdout, e `bin/suite.sh` nao escreve carimbo de rc; `bin/suite.sh` nao escreve carimbo de rc, entao o rc nao se inventa aqui -- e a trava ja passou para `o219-full2` (13:46:01), que e a prova de que o run TERMINOU.

INCOMPLETO (fila, nao trabalho agora — ESMERIL-DO-MARCO)
- universo CRU em `parear_turnos`: `ponto/utils_turno_orfao.py:26` (**36** batidas retratadas vazadas nas **2.306** entradas pendentes da frota; **102** colabs com `tem_bloqueio=True`, **0** bloqueado POR retratada) e `reconciliar_fantasmas.py:85` (os numeros da pergunta de lei). Censo dos **11** chamadores de producao de `parear_turnos`: **2** pela porta, **7** copiam `retratada_em__isnull=True` a mao, **2** crus — e isso E o tripwire B5-j que a CLAUDE.md sec.4 declara inexistente.
- chamada PROVADAMENTE morta em `api/views.py:340` e `api/views_core.py:329`: `regularizacao_obrigatoria = False` e literal em `:343`/`:332`, e por request de `/api/me/` o `detectar_bloqueio_turno_orfao` materializa mediana **125**, media **136,4**, max **430** batidas (frota **74.449**) mais pareamento de historia inteira, e joga fora. Otimizacao exige medida ANTES e DEPOIS, e `api_me` e comparada por AST nos dois kernels.
- `ponto/turnos.py:1963::sequencia_relogio_x_jornada`, 0 chamador: **FILA, nao baixa** — e CONTADOR do ticket `TURNO-G5-SEQUENCIA-DA-BATIDA` (FEITO 15/09), LE a autoridade `sequencia_na_jornada` e nao tem derivacao propria. Mesmo criterio que `cb6cd342` usou para deixar `com_dia_sem_celula` e `fila_de_orfas` em pe.
- os **5** selos de host que dao RED falso em qualquer raia: `test_furo_encadeado_ao_cartorio`, `test_handoff_sessao`, `test_hook_nao_e_copia`, `test_import_tardio_contra_o_ar`, `test_inventario_pessoal_no_commit` — todos leem caminho que so existe na arvore principal (`.claude/settings.json`, `logs/deploy.stamp`, `logs/` do cron). O curado aqui e o precedente da forma, e o SEXTO caso da mesma classe apareceu agora: `bin/tickets_placar.sh --conferir` da **ALARME** na raia e **OK** no main com o TICKETS.md BYTE-IDENTICO (`diff` vazio) — a divergencia e o prefixo `regua OK (04/10 17:26)` da linha 93, que ele deriva do carimbo da regua, e o carimbo so existe na arvore principal.

## O199 POUSA — **O ACEITE DE FOLGA DECIDE O DIA, E A CASCA PARA DE DIZER "GRAVADO"** (10/10 14:4x, L-105 + L-107)

LEI-AKITA: origem=chamados/services/validacao.py::validar_pergunta (o verbo do humano, onde o aceite ficava sem destino), testemunha=ponto/services/declarar_dia.py::checar_declaracao + escala/folgas.py::lancar_folga, RED=chamados/tests/test_o199_aceite_de_folga_decide_o_dia.py (293 linhas novas na raia) + test_MORDE_a_tela_nao_diz_gravado_quando_nada_foi_gravado, quem-mais-le=censo no corpo de `c44c6c80`, juizes novos=0

O QUE MUDOU (pousa SO o produto; a raia `raia-bos` nao tinha instrumento):
- `chamados/services/validacao.py::validar_pergunta` — o aceite de `FOLGA_CONTESTA` entra como GATE, ANTES do
  carimbo, e decide o dia por `ponto/services/declarar_dia.py::declarar_dia(tipo='folga')`. Compara o CODIGO,
  nunca texto livre. `_carimbar_validacao` vira sitio proprio: dois atos decidem, UM escritor de `validada_em`.
- `ponto/services/declarar_dia.py` — a PERGUNTA `o_dia_ja_e_folga` nasce na PORTA, nao no chamado, e `_folga_do_dia` fica leitor UNICO da `FolgaDia` no modulo.
- `chamados/services/acoes_disputa.py` — BUG NO CAMINHO, curado na hora (LEI-AKITA 6): a casca dizia *"ponto
  gravado no espelho"* para TODO estado desconhecido, `ok=False` inclusive (`competencia_trancada` de 16/09 ja
  caia la). Passa a LER o `ok` do service, que e quem sabe o que aconteceu.

NUMEROS, medidos com o juiz REAL na frota: `FOLGA_CONTESTA` tem **340** respostas, **265** aceitas e **135** dias
distintos aceitos — **23** ja eram folga e **112 NAO**, em **60** colaboradores; `FALTA_DIA` (54) e `ATESTADO`
(260) tem backlog **0**, e era essa assimetria o item. IDEMPOTENCIA pela forma do BO: 4 aceites do mesmo dia =
**1** FolgaDia, 1 linha de trilha, 1 nota no fio, 1 chamado resolvido.

PROVA DO POUSO: `merge-tree` sobre o HEAD vivo = **0 conflito**, **0 migration**, 5 arquivos (+503/-7). A copia
certificada e BYTE-IDENTICA ao que pousa — md5 dos 5 arquivos contra `raia-bos` e dos 3 do O219 contra o HEAD,
todos IGUAIS: a suite verde certificou ESTA combinacao, nao uma base velha. Suite CHEIA na copia mergeada:
`Ran 10337 tests in 1400.476s` / `OK (skipped=42)` / `rc=0`, **0** linha `^FAIL:`/`^ERROR:` em 1.992 linhas.

INCOMPLETO: o **backfill dos 112 dias ja carimbados** NAO entra — dado retroativo de escala (L-009, NUNCA
PRE-APROVADO), e a pergunta de lei fica no TOPO deste arquivo com os numeros.

## MARCO 10/10 14:xx — O219 MIOLO: A VERSAO DA REGRA ENTRA NA IMPRESSAO DO CARTORIO, E A DECLARACAO NASCE VAZIA

LEI-AKITA: origem=ponto/services/cartorio.py::impressao_insumos (o hash dos insumos nao sabia QUAL regra julgou o dia, entao correcao de regra nao rejulgava nada), testemunha=ponto/janelas.py::janela_atual (o juiz da competencia, S7 14/09 -- `versao_para` so o LE, nao recalcula), RED=`ponto.tests.test_o219_versao_da_regra` dando `Ran 21 tests` / `FAILED (failures=3, errors=23)` contra o cartorio do HEAD, quem-mais-le=2 chamadores de producao (`:610` e `:685`, os dois em `_julgar_colab_corpo`), 1 patcher (`test_selo_b5u...:162`, curado no mesmo ato), 1 leitor da fonte por `inspect.getsource`; juizes novos=**0**

O QUE MUDOU
- `ponto/services/cartorio.py`: `REGRA_VERSOES: tuple[tuple[int, str], ...] = ()` (`:85`), validacao no IMPORT (`:110`), `versao_para(data, empresa)` (`:121`), `impressao_insumos(..., regra=None)` (`:143` e `:170`), e o helper UNICO `_regra_do_dia` (`:572`) chamado nos dois ramos (`:610`, `:685`) -- cada ramo montando a sua faria o dia alternar entre dois hashes para sempre.
- `ponto/tests/test_o219_versao_da_regra.py` novo, 21 selos com os cinco GOLDEN e as fixtures que os geram carimbadas no arquivo; `ponto/tests/test_selo_b5u_nunca_bateu_ve_ausencia.py:162` ganha `**kw` no `_cega`.
- `app/docs/LEIS.md`: a L-113 deixa de dizer "selo NENHUM ainda" — os 4 REDs dela passam a existir. O ESTADO dela **nao** muda: segue 0 de 3 clausulas.

OS NUMEROS, COM A PROVA AO LADO
- **este pouso move ZERO dia**: 5 de 5 impressoes byte-identicas as de hoje. PROVA: `conferir_byte_identico.py` recorta `impressao_insumos` dos DOIS arquivos por AST e executa as duas com `hashlib`, sem Django; e a parte nova MORDE quando a versao chega (`regra=None/0/1/2` -> quatro hashes distintos).
- RED -> GREEN: `FAILED (failures=3, errors=23)` -> `Ran 145 tests` / `OK`, rc 0 (os 21 novos + os 8 vizinhos nomeados). A linha que mordia o defeito: `AssertionError: 'regra' not found in ['veto', 'teto']`.
- custo por celula com a tupla vazia: **zero** banco, **zero** janela. PROVA: `if not REGRA_VERSOES: return None` esta em `:134-135`, ANTES do `from ponto.janelas import janela_atual` de `:136` — e o cron das 06:28 visita **103.880 dia-colab** (rodada de hoje: `pulados=103.609` em **442 s**).
- a suite CHEIA achou um RED **meu**, curado na ORIGEM: `core.tests.test_contract_mypy` acusou `ponto/services/cartorio.py:110 Need type annotation for "_v"/"_h"` (`Ran 10330 tests` / `FAILED (failures=1, skipped=42)`, rc 1). Quem nao tinha tipo nao era o laco, era a DECLARACAO: tupla VAZIA nao tem elemento de onde inferir. `REGRA_VERSOES` passa a carregar `tuple[tuple[int, str], ...]`, a forma que `core/juizes.py:246` ja usa na casa -- **zero** `type: ignore`, **zero** linha nova no `mypy.ini`.
- suite CHEIA contra a copia do HEAD `8735524c`: `Ran 10330 tests in 1408.700s` / `OK (skipped=42)`, rc=0. A copia e o HEAD **mais exatamente os tres arquivos deste commit** -- `diff -rq` contra `git archive 8735524c` nao devolve um quarto arquivo, so os pontos de montagem e os caches de ferramenta: a suite certificou ESTE commit, nada mais e nada menos.
- commit: ESTE marco (sem sha — um commit nao cita o proprio); deploy `--sem-migrate`, nenhum modelo tocado.

INCOMPLETO
- **A VIRADA NAO ACONTECEU, e com ela a pauta DP.** Declarar `(1, '2026-09')` e ato de FROTA e exige os tres numeros do ensaio na sombra publicados ANTES: dias com ata movida (esperado **424** da 09 e **0** da 10), `emitidos` (a O209 teve 23), e **SEGUNDOS**. O **TXT novo da 09 espera a VIRADA, nao este pouso** — gerado agora, entregaria ao DP exatamente o numero que a obra existe para corrigir.
- fora deste miolo, dito e nao escorregado: o selo de host do bump da versao (`bin/` e INSTRUMENTO, pousa sozinho pela L-105), os adendos AVISO-E-ESCOLHA e excecao-como-cadastro (obra de tela/tabela), o contador `dias_com_regra_velha`, e o rotulo "recalculado em <data>" (escrita de ata, pertence a tela do adendo c).
- a **L-113 segue 0 de 3 clausulas**: este commit nao toca `ponto/services/fechamento.py:74-75` nem os 6 `permitir_exportada` — a clausula 1 espera o `!` dele (AVAIS `L113-CLAUSULA1-RECUSA-DE-RECALCULO`).
- **RECALC-09-PARA-AUDITORIA** (aval 13:5x, complemento desta O219) nao rodou: o QUANDO dele e *"depois do deploy"* e *"nao fura o pouso da O199"*. A decisao tecnica que ele me deixou esta tomada e e UMA linha: `somente_leitura=True` **mais** `permitir_exportada=True` com `motivo_exportada` escrito -- o primeiro porque e *a MESMA conta sem gravar NADA* (`fechamento.py:19-21`, `:723`), o segundo porque a guarda da L-092 dispara ANTES do laco e INDEPENDE do modo (`:71-82`). Zero patch no codigo de prod, sombra que nao suja, portao do deploy intocado.
- **a PRINCIPAL muda de alvo neste commit** (corte de ordem dele 14:0x): **O48 FECHAMENTO-ONLINE** entra logo atras deste miolo e ANTES dos contratos, e por isso o `ORDEM-VIVA-TOPO` vai de `O219` para `O48` AQUI -- pelo O158 o marcador e a AUTORIDADE da ordem, nao a prosa da celula. A ordem que eu executo, declarada: pouso da O199 -> RECALC-09 (medicao) -> O48. **A VIRADA da O219 fica atras da O48 pela ordem dele**, com a celula do O48 corrigida de uma afirmacao FALSA minha (`fechamento_lido` = **0** arquivo .py em toda ref e nas 3 raias; `fechamento_em_memoria` migrou da **:534** para a **:974**).

## MARCO 10/10 12:3x — POUSO DO INSTRUMENTO DO O228, E AS DUAS LEIS DE ECONOMIA NUMERADAS

**Primeira secao sob a L-116** (teto de 25 linhas): a historia deste pouso esta no corpo do commit.

LEI-AKITA: origem=bin/ (a ferramenta que mediu o O228 nao morava no repo), testemunha=bin/esmeril_comum.py::producao (o escritor unico de "o que e producao?") e a propria arvore de b4d938aa^ relida por git archive, RED=bin/tests/test_esmeril_celula_censo.sh nas duas mordidas -- base falsificada para 5 devolve rc=1, e sitio novo plantado sobe 6 para 7, quem-mais-le=0 (nada em app/ importa bin/; o container da suite nao monta bin/, e o selo e chamado pela pasta que a regua ja varre), juizes novos=0

O QUE MUDOU
- `bin/esmeril_celula_censo.py` + `bin/esmeril_celula_censo_de_hoje.txt` + `bin/tests/test_esmeril_celula_censo.sh` entram no main vindos de `7cb0efbb`, byte a byte como a raia os escreveu.
- `app/docs/LEIS.md`: **L-116 RELATO-CURTO** e **L-117 MORATORIA-DE-LEI** numeradas; `CLAUDE.md` secao 7b ganha as duas em 12 linhas.
- `app/docs/BACKLOG.md`: complemento **CENSO-DE-TESTES** na celula da O243 (nao e id novo) e obra **RELATO-CURTO-SELO** registrada, nada construido.

OS NUMEROS, COM A PROVA AO LADO
- catraca REMEDIDA no main de hoje (`cb6cd342`, que andou 17 commits desde a base da raia, um deles em `ponto/turnos.py`): **6 -> 6**. PROVA: `python3 bin/esmeril_celula_censo.py` contra `bin/esmeril_celula_censo_de_hoje.txt`, contador identico — base NAO se regrava.
- selo do censo: **OK** (6 igual a base) e **MORDE** (sitio novo plantado: 6 -> 7). PROVA: `bash bin/tests/test_esmeril_celula_censo.sh`, rc 0.
- pasta de selos de HOST inteira na arvore mergeada: **67 selos, 0 vermelhos**. PROVA: o laco da `regua.sh:138-145`, rodado a mao porque o pre-push ainda espera o smoke dele.
- juiz do hook perguntado por import, nao por leitura: `_proximo_da_fila()` segue **PLACAR-ESTRUTURAL**, `_ids_que_nao_andam()` **214 -> 215** (a linha nova), O228 ABERTA, O243 e RELATO-CURTO-SELO fora.
- **SEM deploy, e e assercao, nao escolha**: `git diff --cached --name-only | grep '^app/'` = **0**. A L-107 fala da arvore viva, e `bin/` nao esta no bind-mount.

INCOMPLETO
- **RELATO-CURTO-SELO**: registrado, nao construido. A secao de `cb6cd342` tem 39 linhas e e legitima — o selo tem de ancorar o "novo" num COMMIT.
- **deriva informacional no censo**: no main, a varredura complementar de TEXTO lista o proprio ledger (`bin/esmeril_*_censo_de_hoje.txt`) e o selo como "leitor fora do .py". Nao move o contador (`fora_py` nunca alimenta `sem`, `:226`) e a base da raia ja listava o ledger da familia batida: **FILA**, nao cura agora (ESMERIL-DO-MARCO).
- **O219 miolo**: construido em copia e medido (5 de 5 impressoes byte-identicas), REDs ainda nao rodados — a pista esta com a raia do O199.

## MARCO 10/10 — POUSO DA RAIA-CELULA (O228 metade CELULA), E A VAGA DA O98 DECIDIDA NO MESMO ATO
Ordem dele de 11:0x item 2: *"a raia-celula pousa a O228 da familia celula e abre a familia turno; raia
com item aberto nao fica sem agente"*. Pousou o **PRODUTO** (`b4d938aa`); o **INSTRUMENTO** (`7cb0efbb`)
ficou na raia para pouso proprio, pela **L-105** (*"INSTRUMENTO nao pousa junto com PRODUTO"*).

**A PERGUNTA DO POUSO NAO E "a raia estava verde".** Ela ficou verde em 09/10 23:03 e o main andou **17
commits / 48 arquivos** desde a base `b5700cbb`. A pergunta e se o que a raia faz ainda responde no main de
HOJE, e isso se mede antes de tocar a arvore:
- `git merge-tree` acusa conflito **so** em `app/docs/BACKLOG.md` e `app/docs/TICKETS.md`. Os dois arquivos
  de codigo entram limpos, e os que pousam sao **byte-identicos** ao `b4d938aa` -- nao resultado de
  auto-merge (`git show | diff -q`).
- **ZERO chamador novo** para os tres apagados (`_ausencias_do_dia`, `_escala_do_dia`,
  `fase_apos_folga_compensatoria`): grep em `.py .html .sh .md .json` de `app/`, `bin/` e `scripts/`, fora
  de `app/docs/`, devolve so as tres definicoes mais as 3 linhas do teste que o diff substitui. Censo de
  chamador **envelhece**, e a L-111 apaga por ele: um leitor nascido no main depois de 09/10 faria a baixa
  virar `NameError` em prod.
- Os docs se resolvem por **UNIAO** (`git show HEAD:` + a celula/linha da raia, com o estado atualizado):
  `--ours` apagaria a linha da raia, `--theirs` reverteria a O224 nos dois arquivos.

**A O228 NAO FECHA, e a mecanica importa mais que a redacao.** O `hook_stop_fila1.py:151-162` le `_FECHADO`
nas celulas **[2] e [3]** e `_NAO_ANDA` na **[3]**. A palavra de fechamento na celula do portao seria a certidao de
obito da obra INTEIRA -- o hook nunca mais nomearia a familia turno, que o aval de 11:2x acabou de abrir.
O estado do pouso foi para a celula **[4]**; a [3] diz a raia e a ordem. Conferido **perguntando ao juiz**:
`_proximo_da_fila()` devolve `PLACAR-ESTRUTURAL` antes e depois, `_ids_que_nao_andam()` da **214** nos dois,
com `O228` DENTRO da fila e `O98` fora.

**A VAGA DA O98: dois avais dele de hoje se cruzam, e isso nao devolve turno.** O de 11:0x manda a raia
*"pousar a O228 da familia celula e ABRIR a familia turno"*; o de 11:4x descongela a O98 e diz que ela *"so
abre quando a raia do esmeril da familia celula (O228) pousar -- ocupa a vaga dela, nao soma raia nova"*.
Ao pe da letra, o primeiro ja ocupou a vaga que o segundo prometeu. **Decisao tecnica pela PAREI-SO-LEI**
(que nao devolve turno, e nem e pergunta de lei -- e ordem de recurso): **a vaga e da O228 INTEIRA**. A
metade celula pousou, a raia **NAO** vagou, e a celula da O98 passa a dizer isso por escrito, mantendo a
palavra `aguardando` -- que e tambem o device que a tira da fila 1. **Se ele quiser a O98 andando JA, e uma
linha de aval**, e eu abro a raia.

**PEDIDO A ELE (segue de 11:4x, nao cumprido e nao vencido):** quando a vaga da O98 abrir, o texto do mapa
antigo -- o que hoje mora na memoria `[[mapa-sistema]]`, apontada por `CLAUDE.md:281` -- tem de ser COLADO
por ele. Sem ele nao ha o que ancorar, e o PROIBIDO do proprio aval e *"definicao sem ancora"*.

## AVAL 10/10 11:4x — TRES REGISTROS COM GATILHO, O DESCONGELAMENTO DA O98, E UM `:365` QUE JA NASCEU VELHO
Chegou no meio deste marco e entra NELE (L-106: docs no commit do marco). Nada construido: *"sem obra agora"*.

**(A) Os tres com gatilho `contratos_estruturais = 20/20`.** Os itens **1** e **2** ja estavam registrados
neste mesmo turno, pelo aval de 11:2x -- **O243 ESMERIL-3-VALIDACAO** e **O244 CONTRATOS-DAS-FAMILIAS** --,
entao nao nascem de novo (**PROMPT-NAO-SE-REPETE**: prompt cujo titulo ja esta no `PROMPTS.md` responde em uma
linha e nao refaz nada). O que entra e o **item 3**: **O245 DIAGRAMA-VIVO-EXPLICA** (pedido dele de 27/09) --
o `manage.py gerar_diagrama` passa a mostrar, por bloco e **em camadas** (familias -> familia -> juiz), a
pergunta que o bloco responde, o juiz, leis, selos, **numero vivo** e obras abertas, *"gerado do codigo, nunca
a mao"*. Os tres com `aguardando` na celula de estado, que e a palavra que o `hook_stop_fila1` le para NAO os
cobrar como fila 1 -- conferido chamando o proprio `_ids_que_nao_andam`: O243, O244 e O245 fora. Estados em
**172**, **156** e **126** chars (teto 300 da **L-109**).

**(B) A O98 GLOSSARIO DESCONGELA, e o portao dela nao e a fila -- e uma VAGA.** *"A L-096 segue congelando
dinheiro e celula; a O98 e so documento e nao toca nucleo. Anda em RAIA PROPRIA, e SO abre quando a raia do
esmeril da familia celula (O228) pousar -- ocupa a vaga dela, nao soma raia nova."* Isso e a **L-105** lida
pelo lado do RECURSO: o que limita nao e o numero de itens abertos, e o numero de raias com agente vivo. A
celula da O98 saiu de `**CONGELADA (L-096)**` para `**aguardando a VAGA**`, com a especificacao dele inteira
na coluna da obra -- FONTE, FAZ (os **21** termos nomeados), MUDA, LINHA HAIKU, SELO de host, PROIBIDO e
PRONTO, palavra por palavra.

**O `:365` dele ja nascera velho, e o caso e meu.** O aval manda *"CLAUDE.md:365 deixa de apontar para
memoria [[mapa-sistema]]"*. Esse ponteiro esta HOJE na **:281**, e no HEAD ja estava na **:334** -- a O224
encurtou o arquivo em 7.437 chars **neste mesmo commit**, e offset anda a cada commit (o CLAUDE.md diz isso
de si mesmo: *"ancorar por grep, nunca por linha"*). O proprio PROIBIDO dele (*"definicao sem ancora"*) e a
cura, e ela ja esta escrita na celula: o sitio se acha por **grep do duplo colchete**, que acha **1** so no
arquivo inteiro -- o que tambem mede o selo que ele pediu (*"ponteiro para memoria dentro de CLAUDE.md =
VERMELHO"*): hoje ele morderia **1** linha, e depois da O98, **0**.

**PEDIDO A ELE, como o aval manda** (*"Eu colo o texto do mapa antigo quando a raia abrir -- pedir no
RELATO"*): **quando a vaga da O98 abrir** -- isto e, quando a raia da O228/familia celula pousar -- **cole
aqui o texto do mapa antigo** (`[[mapa-sistema]]`: roteamento, middleware, fluxo app x PWA), porque a memoria
nao entra no repo por mim e o `GLOSSARIO.md` nasce sem ele. Sem o texto, a O98 fecha **INCOMPLETA com a
lista**, pelo seu proprio PRONTO.

## O224 DIETA-DO-CLAUDE-MD — FECHADA: AS DUAS TABELAS DE LOOKUP SAEM, E O PRONTO PASSA A SER O NUMERO QUE ISSO DEU (10/10 11:3x)
`LEI-AKITA: origem=CLAUDE.md (a tabela de consulta pesa em TODO turno e nao decide nada: quem decide e o codigo vivo), testemunha=app/docs/JUIZES.md + app/docs/CRONS.md + LAPIDES.md 58-60 (o lookup e a janela vencida passam a ter endereco proprio, com ponteiro NO LUGAR de onde sairam), RED=$SC/o224b/prova2.py (3 perguntas, 0/0/0) + os 66 selos de host da pasta inteira, quem-mais-le=grep de `OS JUIZES`/`GEOMETRIA DE TURNO`/`processar_alertas_turno`/`SUPRA_JUIZ` em bin/tests/*.sh, bin/*.sh e bin/*.py = 0 hit; 4 testes de Python citam as secoes 5 e 7 em PROSA e por isso os dois cabecalhos FICAM, juizes novos=0`

**A pergunta que estava no topo, e a resposta.** A 2a fatia fechou em **-6.898** chars e eu escrevi aqui que
o PRONTO de **-10k** nao se alcancava sem tocar REGRA -- com os 15.737 que faltavam abertos em tres classes,
cada uma dentro de um PROIBIDO do seu proprio aval. Nao devolvi o turno por isso
(**PAREI-DE-LEI-NAO-DEVOLVE-TURNO**): o numero ficou no topo, a esteira seguiu, e as **11:0x** ele respondeu,
literal: *"O224: as tabelas de consulta (secao 5 juizes e lista de crons da secao 7) saem para arquivo
proprio com ponteiro, e as regras de janela vencida (RODADA 3, 'um colaborador por ato', ordem da fila 2 de
29/09) vao para LAPIDES. O PRONTO passa a ser o numero que isso der."* Ele cortou a classe **(a)** e a **(b)**
e **redefiniu o PRONTO** -- que era a classe (c). A pergunta certa, na hora certa, fez a obra fechar.

**O numero, e ele E o PRONTO.** `CLAUDE.md` **56.473 -> 49.036** chars nesta fatia (**-7.437**). Desde o
`037ae715`, inicio da obra: **60.865 -> 49.036 = -11.829** chars. Pela razao DERIVADA da leitura DELE
(**30,2k tokens** em 08/10 13:25 para os **60.865** chars daquele commit = **2.016 chars/token**), isso e
**~-5,9k tokens**, de ~30,2k para **~24,3k**. Nascem `app/docs/JUIZES.md` (**5.748** B) e `app/docs/CRONS.md`
(**3.809** B); `LAPIDES.md` passa a **60** lapides.

**O QUE NAO SAIU, e aqui esta o desenho da fatia.** A secao 7 **nao sai inteira**, porque tem REGRA DENTRO
da lista: a **CORRENTE SEM MINUTO** (corte 29/09, BLOCO-DA-MANHA-SATURADO -- os crons de 06:36 e 06:40
perderam o horario e cada elo espera o ARQUIVO do anterior, *"porque horario e aposta sobre duracao e arquivo
e FATO"*) e o aviso do **SUPRA_JUIZ** (*"o nome engana: NAO e so leitura"* + *"mexer em
`ponto/supra_juiz.py::classificar_dia` muda o que e RETRATADO em prod"*). Levar qualquer uma delas seria
exatamente o *"mover regra para outro arquivo"* que o aval de 08/10 proibe. Entao a secao foi recortada em
**4 trechos** e so os HORARIOS viajaram: a corrente ficou, com `ver app/docs/CRONS.md` no lugar da ordem;
o aviso do SUPRA_JUIZ ficou, com os 9 horarios fora. Os cabecalhos `## 5.` e `## 7.` **ficam**, carregando o
ponteiro, porque **4 testes de Python citam as duas secoes em prosa** --
`chamados/tests/test_porta_chamado_ciclo.py:407`, `colaboradores/tests/test_led_pelo_juiz.py:17`,
`escala/tests/test_contador_vigencia_impossivel.py:116`,
`ponto/tests/test_realizado_do_dia_autoridade.py:189` -- e o aval proibe tocar teste: apagar o cabecalho
deixaria quatro citacoes apontando para o vazio. **Decisao tecnica registrada, nao perguntada**
(PAREI-SO-LEI): DOIS arquivos de destino, um por assunto, e nao um "CONSULTA.md" unico.

**A lapide 59 nasce DENTRO do item numerado.** A REGEN-EM-EXPORTADA diz *"estas quatro condicoes juntas"*;
o *"UM colaborador por ato"* era a condicao **1 de 4**, e a TXT-E-FOTOGRAFIA de 30/09 ja a revoga no proprio
paragrafo (*"cai tambem o 'UM colaborador por ato'"*). Renumerar 2 para 1 seria **reescrever a regra**, entao
a numeracao fica e o ponteiro ocupa o slot: `    1. (historia: LAPIDES.md#lap-59)`.

**PROVA (`$SC/o224b/prova2.py`, 3 perguntas, 0/0/0).** (a) toda ADICAO no `CLAUDE.md` e ponteiro de lapide ou
um dos 3 ponteiros DECLARADOS -- **0** fora disso; (b) toda REMOCAO reaparece no destino -- **0** perdida;
(c) todo trecho declarado esta **NO SEU** destino, tabela byte a byte e lapide linha a linha -- **0** faltando.
A pergunta (c) nasceu de um defeito do proprio conferidor: o normalizador herdado da 1a passada fazia
`.replace('> ', ' ')` cego para desfazer a citacao de lapide, e isso **come a seta** -- `-> periodo_apuracao`
virava `- periodo_apuracao` so do lado da remocao, e 4 trechos deram VERMELHO falso. Curei o NORMALIZADOR
(a desmarcacao passou a ser ancorada em inicio de linha, `^> ?`, que e onde a citacao mora), nao a pergunta,
e a (b) sozinha passaria por COINCIDENCIA se um trecho igual ja existisse no destino -- por isso a (c).

**PORTAO.** A pasta INTEIRA de selos de host, do jeito que `bin/regua.sh:138-145` a roda: **66 selos, 0
VERMELHO**; `bin/node_check.sh` verde (12 arquivos). **A suite do Django nao e o portao desta fatia**, e o numero
certo nao e zero -- sao DOIS numeros: `grep` da string `CLAUDE.md` em `app/**/*.py` da **270 linhas em
193 arquivos**, todas PROSA, e **0** delas tem `open(`/`read_text`/`Path(`/`BASE_DIR`. **Ninguem LE o arquivo; 193
arquivos o CITAM.** Eu ia publicar `= 0` para a pergunta da citacao, e ela e falsa pelo meu proprio
paragrafo acima: os 4 testes que citam as secoes 5 e 7 escrevem `CLAUDE.md` na prosa. Citacao nao e
leitura, e um grep so nao separa as duas. E o ponto cego que eu fui procurar: `grep` de
`OS JUIZES`, `GEOMETRIA DE TURNO`, `processar_alertas_turno` e `SUPRA_JUIZ` em `bin/tests/*.sh`, `bin/*.sh` e
`bin/*.py` = **0 hit** -- nenhum dos 66 esta verde **porque** a tabela saiu.

**O que mais entra neste commit do marco (L-106: docs entram com o codigo do marco).** `PROMPTS.md` com as
**3** linhas dos avais de 11:2x (PAUTAS-09-UM-TXT-SO, O228-TURNO-E-ESMERO, DOIS-MANDATOS-COM-GATILHO-20-20);
`PENDENTES_RONALD.json` + `AVAIS.md` com **7** itens respondidos pelas palavras dele -- a mesa cai de **14
para 7**, e o `O224-OS-10k-PEDEM-REGRA` e um deles; e os **tres** itens novos do bloco OBRAS -- **O243
ESMERIL-3-VALIDACAO**, **O244 CONTRATOS-DAS-FAMILIAS** e **O245 DIAGRAMA-VIVO-EXPLICA** --, registrados com o
gatilho dele (*"abre quando contratos_estruturais = 20/20"*, hoje **15/20**) e **sem obra agora**, mais a
**O98 GLOSSARIO DESCONGELADA** pelo aval de 11:4x (secao acima) -- a palavra `aguardando`
na celula de estado e o que faz o `hook_stop_fila1` NAO os cobrar como fila 1, conferido chamando o proprio
`_proximo_da_fila`/`_ids_que_nao_andam` (O243 e O244 fora, O224 fora por `**FECHADA`).

**A celula da O224 fechou na celula 3, nao na 4, e isso tambem foi medido.** O juiz de *"qual item esta
ABERTO"* e um so -- `bin/hook_stop_fila1.py::_proximo_da_fila` --, e ele le `celulas[3]`. O cabecalho da
tabela chama essa coluna de `portao`, mas **as linhas fechadas poem o ESTADO ali** (O223:260, O126:283,
O120, O124). A linha da O224 nascera com o portao na 3 e o estado na 4: fechar so na 4 deixaria o hook
cobrando para sempre um item resolvido. Estado subiu para a 3, portao desceu para a 4, as duas dentro do
teto de **300** chars da **L-109** (**291** e **220**, medidos).

**PROXIMO.** Pela ordem dele de 10:4x (*"fechada a O224, a principal volta ao miolo da O219"*): o pouso da
`raia-celula` (O228 da familia celula) e a abertura da familia turno como **passada de esmero** pelo aval de
11:2x, e dai o **miolo da O219** -- a versao da regra em `impressao_insumos` --, que e tambem o portao do TXT
unico da 09 com as cinco correcoes do DP pelo aval das 11:2x.

## LEI RESPONDIDA 10/10 11:0x — era esta a pergunta do topo, e ele a respondeu ALARGANDO o aval (o registro fica; a resposta esta na secao acima)
Nao devolvo o turno por isto (**PAREI-DE-LEI-NAO-DEVOLVE-TURNO**): o numero fica aqui no topo, a esteira
seguiu para o miolo da O219, e a resposta entra quando vier. Item no AVAIS: `O224-OS-10k-PEDEM-REGRA`.

**O numero.** O `CLAUDE.md` saiu de **63.335** para **56.437** chars -- **-6.898**. Pela razao DERIVADA da
sua leitura (**30,2k tokens** em 08/10 13:25 para os **60.865** chars do `037ae715` = **2,016 chars/token**)
isso e **~-3,4k tokens**, de ~31,4k para **~28,0k**. O seu PRONTO pede **-10k**, ou seja <= ~40.700 chars:
faltam **15.737**.

**Por que e lei e nao trabalho.** Os 15.737 que faltam **nao sao narrativa** -- a narrativa acabou. Sao tres
classes, e cada uma esta num PROIBIDO do seu proprio aval (*"mover regra para skill ou para outro arquivo,
reescrever regra, apagar em vez de mover"*): **(a)** tabela de **LOOKUP** -- a secao 5 (juizes, **4.860**
chars) e a lista de crons da 7 (**3.434**), **8.294** somadas: nao e historia, e consulta, e sairia inteira
para arquivo proprio com ponteiro, que e "mover regra para outro arquivo"; **(b)** regra com **janela
vencida dentro do texto** -- as duas janelas da RODADA 3 (JANELA DE FECHAMENTO de 20/09 e CONGELAMENTO DE
DINHEIRO de 16-17/09, ~460 chars), o "UM colaborador por ato" da REGEN-EM-EXPORTADA que a
TXT-E-FOTOGRAFIA de 30/09 **ja revoga no proprio paragrafo**, e a ORDEM da FILA-2-EM-RAIA-PROPRIA de 29/09:
tirar isso e "mover regra", ainda que a regra esteja morta; **(c)** **-3,4k e o numero**, e o PRONTO se
corrige. Escolher sozinho seria eu decidindo o que e regra e o que nao e, dentro de um aval que me proibiu
de tocar regra. **O que NAO depende da resposta:** as 57 lapides estao movidas e provadas, e nenhuma
palavra de regra mudou.

## O224 DIETA-DO-CLAUDE-MD — 57 LAPIDES MOVIDAS, -6.898 CHARS, E A PROVA E DE PALAVRA, NAO DE INTENCAO (10/10 10:4x)
`LEI-AKITA: origem=CLAUDE.md (a narrativa mora encostada na regra e pesa em TODO turno, nao no turno que a le), testemunha=app/docs/LAPIDES.md (o arquivo que a L-109 abriu em 04/10 exatamente para isto) + o word-diff dos dois arquivos, RED=o conferidor de palavra ficou VERMELHO duas vezes contra MIM -- corte largo em §6 reintroduzindo a palavra "Antes" fora de ponteiro, e dois cortes que desgrudavam a virgula de "CONTRARIO," e os dois pontos de "`TOTAL`:" --, e a cura foi ENCOLHER ou DESISTIR do corte, nunca afrouxar o conferidor, quem-mais-le=os 3 selos de host que afirmam sobre o CONTEUDO do CLAUDE.md (bin/tests/test_lei_akita.sh, test_suite_sh.sh, test_segredo_fora_do_argv.sh) e **0 leitor em Python** (varredura de open/read_text/Path sobre os dois nomes: nada -- os 2 arquivos que citam LAPIDES citam em prosa), juizes novos=0`

**O que a passada fez.** 42 lapides novas (ids **16 a 57**) nas secoes **7b, 6, 2, 3, 0 e 4b**, cada uma com
a linha "De onde saiu" dizendo secao e REGRA, e no lugar de onde saiu fica exatamente
`(historia: LAPIDES.md#lap-NN)`. `CLAUDE.md` **63.335 -> 56.437** chars; `LAPIDES.md` **11.182 -> 31.300**,
**57** lapides.

**A O220 havia declarado 0, 2 e 4b fora, e a O224 entrou em tres desses sitios -- o que mudou nao foi a
lei, foi o CORTE.** O motivo da O220 (*"nelas o caso MEDIDO e o alcance da regra"*) nao alcanca uma cauda
que comeca em `NASCEU MEDIDA, e o caso e meu:`: ali a regra JA acabou, com ponto, e o corte e no limite da
frase. O cabecalho da `LAPIDES.md` **diz isso no mesmo commit** -- o paragrafo da O220 fica palavra por
palavra e ganha a ressalva embaixo, porque deixa-lo afirmando "ficaram onde estavam, de proposito" seria o
arquivo mentindo sobre si.

PROVA: conferidor de palavra sobre `git diff --no-index --word-diff=porcelain` dos dois pares base/novo --
**ADICIONADO ao CLAUDE.md que NAO e ponteiro: 0** · **REMOVIDO que NAO reaparece na lapide: 0**; ponteiros
**57/57 unicos**, conjunto IGUAL ao dos 57 titulos, sequencia 01..57 sem buraco; os 3 selos de conteudo
verdes (13 itens numerados da LEI-AKITA, bloco canonico da §3 ainda nomeando `bin/suite.sh`, 0
`-e DB_PASSWORD` sem backtick) e a **pasta inteira de host 66/66 verde**. A dieta nao se emenda: ela se
**refaz do HEAD** a cada rodada (`git show HEAD:` -> 6 especificacoes -> cabecalho -> conferidor), e o
conferidor e a ULTIMA linha da rodada.

**O PRONTO dele NAO esta alcancado, e isto nao se arredonda:** -3,4k contra -10k. A lei esta no topo, com as
tres classes e os numeros de cada uma.

## LEI RESPONDIDA 10/10 — Q1 DO VIGIA: VIGENTE E O ARQUIVO QUE A CASA ENTREGOU
A pergunta que estava neste topo desde 07:xx (os 44 dia-matricula entregues antes da HX-BORDA-CELULA, hoje
retidos por `sem_celula`) foi respondida por ele, literal: *"na Q1 do vigia, VIGENTE e o arquivo que a casa
entregou, pela TXT-E-FOTOGRAFIA de 30/09. Os 44 sao porta movida legitima e seguem sem alarme. Nenhuma lei
nova, e leitura."* Entao: **o vigia esta certo como esta**, os 44 continuam em `porta_moveu` -- que nao
alarma --, nenhum deles entra em `valor_moveu`, e o alarme segue **1 de 7**. **Nada se construiu**: a
resposta confirma o codigo no ar, e e por isso que ela nao tem commit proprio. Historia em
`RELATO-ARQUIVO.md` quando esta entrada vencer os 3 dias da **L-109**.

## O223 pouso B — A EMPRESA ENTRA NO CENSO DE CADASTRO, E O CUSTO QUE EU HAVIA DECLARADO EM 7 FOI MEDIDO EM 15 (10/10 09:4x)
`LEI-AKITA: origem=core/configuracao_efeito.py (o censo de "chave sem leitor" -- unico sitio onde um campo editavel se declara), testemunha=core/tests/test_contract_configuracao_nao_mente.py::test_MORDE_todo_campo_editavel_esta_declarado (o SELO fez o censo, nao a minha AST), RED=logs/o223/red_pousob.out (`FAILED (failures=1)`, `First list contains 15 additional elements`), quem-mais-le=core/contratos_estruturais.py (nota da celula folha/export x contrato 3) + core/configuracao_efeito.py::familias_com_parametro (lido por core/contratos_estruturais.py::total) + os dois pinos E0 do proprio selo, juizes novos=0`

**O que a fatia fecha, dito sem inflar.** A L-006 estava `PELA-METADE` com a 3a clausula (*chave sem leitor*)
e a 2a metade da 2a em aberto, e a propria linha dela previa que **UMA** fatia fechava as duas: por a
`Empresa` no censo. Fechou -- a L-006 passa a `INTEIRA -- 3 de 3 clausulas no codigo, 3 de 3 com teste que
morde`. **O que NAO se move: o placar de contratos.** Perguntei ao juiz na copia antes de escrever a linha:
`total()=20 | verdes=15`, e as familias com parametro seguem as **MESMAS 5** (`ausencia/ferias`, `batida`,
`celula/precedencia`, `folha/export`, `turno/marcos`) -- o teto da L-100 nao se mexe e nenhuma celula da
matriz vira verde com isto. Esta fatia fecha clausula de **LEI**, nao celula de matriz, e dizer o contrario
seria inflar o placar.

**O NUMERO QUE EU ERREI, e como ele se corrigiu.** A linha da L-006 declarava o custo do pouso B em **7
declaracoes** -- as chaves de POST que a `colaboradores/views.py::form_empresa` le (`ativa`, `cnpj`,
`dia_inicio_competencia`, `em_rollout`, `nome_fantasia`, `razao_social`, `regime_trabalhista`). Sao **15**.
A diferenca inteira e o **ADMIN**: `EmpresaAdmin` nao declara `fields`, e `ModelAdmin` sem `fields` deixa
**todo** campo editavel -- a mesma segunda porta de edicao que a O124 fechou em `Praca` e em `Posto`, e o
selo ja a varria por `editaveis_dos_admins()`. Declarar 7 e deixar 8 editaveis pelo admin seria a
**MEIA-CORRECAO** de sempre, com censo meio aberto: a `Empresa` seria a primeira entidade VIGIADA na tela e
CEGA no admin. **E o 15 nao e estimativa minha**: armei o RED com a `Empresa` em `TELAS` e em
`ENTIDADES_COM_ADMIN` e **nenhuma** declaracao, e o selo respondeu o censo por escrito --
`logs/o223/red_pousob.out`, `First list contains 15 additional elements`, nomeando os 8 que a minha AST da
tela nunca veria. **O selo e o censo** (LEI-AKITA 5 e 8 no mesmo ato): quem conta e a autoridade, nao a
sonda de quem esta curando.

**O que entrou.** 15 declaracoes em `core/configuracao_efeito.py::DECLARACAO`, cada uma com leitor NOMEADO:
13 `CONSUMIDO` e 2 `CADASTRO`. Os 2 `CADASTRO` sao `razao_social` e `nome_fantasia`, que **so se imprimem**
(cabecalho do TXT, painel, bootstrap do tenant) e nao decidem valor -- e `CADASTRO` entra com `familia=None`
por lei do proprio selo (`familias_com_parametro` pula `familia is None`, e
`test_declaracao_so_tem_os_tres_estados` cobra *"cadastro nao e parametro de familia"*). O `cnpj` entrou
como `CONSUMIDO`, e nao como identidade, porque `colaboradores/queries.py::empresas_visiveis` **tira do
universo das telas** o CNPJ de diagnostico `11222333*`: ele decide quem aparece. Dos 13, **10 sao da familia
`folha/export`** -- `dia_inicio_competencia` (o corte da competencia, nunca 21 cravado),
`regime_trabalhista` (piso legal ou CCT, pela O211 pouso B) e os **8 da janela de HE**, que o
`motor_calculo_v2` e a `folha/porta_export.py` leem. Pinos E0 movidos **com o motivo medido ao lado**:
`CONSUMIDO 26 -> 39` e `DECLARACAO 49 -> 64`.

**O que esta fatia NAO faz, e esta dito no codigo e nao so aqui.** Nao tranca campo nenhum. Declarar e
documentar; `readonly_fields` e comportamento, e `rotulo_de_efeito` so devolve rotulo para `SEM_EFEITO` --
entao **zero template mudou** e a fatia nao passa pelo FRONT-SEM-SMOKE. **Fila nova nascida aqui**: os 8
campos de janela de HE (e os vizinhos do `regime_trabalhista`) tem escritor de producao em
`colaboradores/services/estrutura.py::atualizar_empresa`, e o **admin escreve por fora dele**. Isso e achado
de **contrato 2** (*um escritor por entidade*), nao desta fatia: vai para a fila, nao para este commit.
Trancar aqui seria regra de negocio fora do pedido. **A linha tem nome e endereco**, para nao morrer em
prosa: `ADMIN-ESCREVE-EMPRESA-FORA-DA-PORTA` no bloco OBRAS, com portao `**registrada**` -- fila DEPOIS
dos contratos. E ela NAO entrou em `core/juizes.py::PENDENTES` de proposito: escolher a familia e a
pergunta ali **move celula do placar**, e isso nao se decide de passagem dentro de outra fatia.

**Uma nota que teria comecado a mentir.** A celula `folha/export x parametro consumido ou sem efeito` de
`core/contratos_estruturais.py` dizia *"Sobram 2 CONSUMIDO ... O total da DECLARACAO nao mudou (49)"*. Com
esta fatia os 2 viram 12 e o total vai a 64 -- emendei a nota **no mesmo commit**, porque testemunha que
recalcula ou envelhece e exatamente o que a LEI-AKITA 2 proibe.

**Esmeril do marco (ESMERIL-DO-MARCO), so o que o pouso A deixou orfao.** Dois itens, os dois declarados
antes: (1) o docstring de `core/management/commands/semear_aplicacao_convencao.py` dizia que o
`get_or_create` *"guarda aqui"* -- o endereco **mudou no pouso A** para
`core/services/aplicacao_convencao.py::cadastrar`, e docstring que aponta para o sitio velho manda a proxima
sessao fazer arqueologia; (2) o `--listar` tinha **queryset proprio** (`order_by('empresa_id','praca_id')`)
enquanto a tela lia `servico.listar()` (ordenado por razao social). Nao davam numero diferente -- davam
**ORDEM** diferente, e e assim que um leitor paralelo comeca. Agora `--listar` le a porta, e a ordem impressa
passa a ser a que a tela ja mostrava.

**Verde.** Selo do censo + 11 vizinhos: `Found 139` / `Ran 139 tests in 53.802s` / `OK`, rc 0, `0` linhas
`^(FAIL|ERROR):` (`logs/o223/verde_pousob.out`). Suite CHEIA na **copia**: `Ran 10309 tests in 1400.556s` /
`OK (skipped=42)`, rc 0, `0` linhas `^(FAIL|ERROR):` (`logs/o223/suite_pousob.out`). `ruff check` **All
checks passed** nos **5** arquivos, e o censo md5 da copia inteira contra `HEAD:app/*` da exatamente esses 5
DIFERE e nenhum FALTA.

**O 5o arquivo entra com os bytes do commit DIFERENTES dos bytes da suite, e isso se diz com os md5.** O
docstring de `core/tests/test_o223_porta_humana_da_aplicacao.py` apontava para `docs/LEIS.md:149` dizendo
`PELA-METADE` -- a linha que a cura deste pouso tirou de la -- e dizia que a 3a clausula custava **7
declaracoes**, que e o numero que eu errei. Ele **ficou de fora da copia enquanto a suite corria**, de
proposito: editar a arvore que a medicao monta troca os bytes medidos. Depois do veredito foi editado na
copia (md5 `df5cddb71d0d66b5ffc8036db4113747` -> `021ce7d55df0663fd0c03a07062bc7a0`), e o que mudou e
**docstring, zero linha executavel** -- a correcao de 7 para 15 com a causa (`EmpresaAdmin` sem `fields`), e
o endereco passando a citar a LEI em vez do numero de linha, que foi o que envelheceu. O modulo e os
vizinhos de varredura correram outra vez sobre os bytes novos: `Ran 54 tests in 2.904s` / `OK`, rc 0
(`logs/o223/revizinho_docstring.out`, com `core.tests.test_contract_configuracao_nao_mente`,
`chamados.tests.test_selo_lapide_legado`, `core.tests.test_regua_pela_empresa` e
`core.tests.test_admin_so_leitura`). Entao: dos 5 arquivos, 4 foram medidos pela suite cheia e 1 por 54
casos dirigidos -- nao por "e so comentario".

**NO AR** (commit `04392fe4`, 10 arquivos, +196/-31).
PROVA: `bin/deploy.sh --sem-migrate` rc 0 --
`migrations pendentes no schema do cliente: 0`; `sombra: carimbo dia=20261010 status=OK tipo=completa
diverge=0 erros=0`; `prova de casca: 16 estaticos conferidos, 5 paginas compiladas, 611 rotas importadas em 2
urlconf(s)`; as tres rotas -- `saas_core /health/ -> 200`, `saas_ui /colaboradores/ -> 302`,
`mensageria /health/ -> 200`; selo BUG 128 verde nas 3 cascas; `importerror_500=0` (09:06 a 10:06).


## O223 pouso A — A APLICACAO DE CONVENCAO GANHA PORTA HUMANA, E O CAMPO QUE DECIDE PISO x CCT GANHA ESCRITOR (10/10 08:0x)
`LEI-AKITA: origem=core/services/aplicacao_convencao.py (porta unica) + colaboradores/views.py::form_empresa, testemunha=core/regua_cct.py::_aplicacao_vigente (quem decide segue sendo ela; a tela so cadastra e DIZ o empate), RED=core/tests/test_o223_porta_humana_da_aplicacao.py (12 casos, 9 VERMELHOS no HEAD) + core/tests/test_regua_pela_empresa.py (13 casos, 1 VERMELHO) reconstituido em logs/o223/red_head.out: Ran 25 / FAILED (failures=3, errors=7), quem-mais-le=censo de 4 sitios com PROVA por papel em core/tests/test_regua_pela_empresa.py, juizes novos=0`

**O que estava medido antes da fatia, e nao e pedido novo.** O pouso B da O211 (08/10) deixou duas dividas
NOMEADAS, as duas da L-006 (*tudo tem cadastro pela UI*): o cadastro `core.AplicacaoConvencao` nasceu com
**0 view, 0 rota, 0 template** (censo na arvore, 08/10) e nao estava em **admin NENHUM** (censo de
`admin.register`, 13 registros); e `Empresa.regime_trabalhista` tinha **LEITOR e escritor NENHUM em codigo
algum** -- so a definicao do modelo e duas migrations --, entao o valor que escolhe entre **piso legal** e
**CCT de uma empresa inteira** se escrevia por shell. Desde o achado (3) de 08/10 a tabela esta no admin com
o mixin `SoLeitura`: visivel e nao editavel, de proposito.

**O desenho, e por que ele e UM e nao dois.** A escrita da tabela tinha UM escritor -- o corpo do
`semear_aplicacao_convencao`. Uma tela com caminho proprio seria o **segundo escritor do mesmo estado**, e
campo de estado com dois escritores mente mesmo com cada escritor certo (LEI-AKITA 7). Entao nasceu
`core/services/aplicacao_convencao.py` -- `listar / obter / empates / cadastrar / desativar` + `CadastroInvalido`
-- e **os dois** entram por ela: o command passou a DELEGAR e a view **nao nomeia o modelo**. Isso nao e
promessa de conduta, e assercao medida por AST no selo: a view nao casa o nome, o command tem **0** verbo de
escrita (`create/get_or_create/save/update/delete/bulk_create` = `[]`) e a porta tem `get_or_create` e `save`.

**O que a tela diz, e que a tabela nao dizia.** `empates()` devolve as linhas ATIVAS do mesmo nivel, e a
lista as marca em vermelho com a frase: *duas linhas ativas do mesmo nivel se anulam e a empresa cai no piso
legal*. O criterio de nivel e o MESMO de `_aplicacao_vigente` -- a tela **repete para DIZER**, nunca para
julgar; quem decide dinheiro continua sendo a regua. Desativar **nao apaga**: `ativo=False` com trilha, e a
tela diz isso no proprio campo.

**UMA mudanca de contrato, declarada e nao escondida.** `--desativar` passou a **EXIGIR `--motivo`**, que
antes dispensava (`and not o['desativar']`). Nao e regra nova: e a LEI-AKITA 7 alcancando um caminho que lhe
escapava -- desativar a linha derruba a CCT de uma empresa inteira no piso legal, e sem motivo escrito a
forense nao reconstroi isso depois.

**O erro que eu cometi no meio da fatia, e a licao que ele deixou na ORDEM.** Escrevi
`{% url 'core:lista_aplicacoes' %}` dentro de `templates/colaboradores/empresa_form.html`, que e template
**VIVO**: sem `cached.Loader`, o Django rele o arquivo do disco a cada request enquanto o `.py` espera o
reload -- a tela de edicao de empresa ficou em **NoReverseMatch (500) por cerca de um minuto**, ate eu
trocar o link por texto. Era exatamente o risco que eu havia NOMEADO para o `hub.html` e no qual pisei em
outro arquivo. O que mudou por causa disso: o link do hub nasceu na **COPIA** (`/tmp/o223-copia-suite`), a
suite cheia correu **la**, e ele so entra na arvore viva no ato do commit, com o `deploy.sh` em seguida e
nada no meio (L-107).

**E A COPIA PEGOU O SEGUNDO ERRO MEU NA MESMA FATIA -- que e exatamente o que ela existe para fazer.** A
suite cheia na copia voltou `Ran 10309 tests` / **`FAILED (failures=1, skipped=42)`**: UMA falha em 10.309,
e minha. `holerite/tests/test_contract_lapide_nao_vaza.py::test_nenhuma_lapide_multilinha_em_templates`
apontou `templates/core/config/hub.html:34` -- eu escrevi a lapide do link como comentario de uma cerquilha
esticado em **TRES LINHAS**, e esse comentario so vale numa LINHA: em varias ele nao fecha e o texto **VAZA
PARA A TELA**. Nao e descoberta nova desta casa e eu nao tenho escusa: a licao estava escrita (*compilar nao
e renderizar*), e eu pisei nela no mesmo arquivo em que acabara de me queimar com o `{% url %}`. Cura na
ORIGEM: o bloco virou `{% comment %}` ... `{% endcomment %}`, que e o que o proprio selo manda na mensagem de
erro. **O que isso prova sobre o desenho do pouso**: se o link tivesse nascido na arvore viva, como eu fiz com
o `empresa_form.html`, esse texto estaria VAZANDO no hub de todos os admins no instante em que eu gravei o
arquivo -- template nao espera reload. A copia nao foi cautela de processo: foi a diferenca entre um selo
vermelho e uma tela suja em prod.

**E A CURA FICOU VERMELHA TAMBEM, pela SEXTA vez na mesma classe.** O selo re-rodado seguiu reprovando, agora
na linha 38: a frase que eu escrevi DENTRO da lapide para explicar a regra continha o token proibido, e o selo
varre **TEXTO**, nao estrutura. E a mesma familia que esta casa ja viu cinco vezes (*selo estrutural varre
AST, nao texto -- varrer texto faz o selo morder a prosa que explica a cura*), e a sexta fui eu. Duas curas
eram possiveis: dizer a regra **por extenso**, sem o literal, ou ensinar o selo a varrer estrutura. Pela
CURA-MAIS-RESTRITIVA eu fiz a primeira, que nao mexe em selo de outra familia no meio de uma fatia; a segunda
e a linha de fila que ja existe para a impercisao desse selo, e ESMERIL-DO-MARCO proibe abri-la agora. A
lapide nova diz a regra por extenso e **declara por que**: a frase que explica a regra nao pode conter o token
que a regra proibe.

**Depois das duas curas a bateria correu de novo sobre a copia**, porque custo de tempo nao e argumento
(LEI-AKITA 3) -- e com um portao: o selo da lapide (0,7 s) corre ANTES da suite cheia, e se ele nao ficar
verde a suite nao roda. Queimar 23 min sobre uma copia que um selo de 0,7 s ja reprova nao e rigor, e
desperdicio.

**A vacuidade que eu devia responder, agora com o numero MEDIDO e nao o lembrado.** Esta linha dizia *"dois
dos 12 passavam"*, e esse dois vinha do log que se perdeu -- ou seja, de memoria. O RED reconstituido
(`logs/o223/red_head.out`) responde: **sao TRES**, e os tres tem nome e motivo diferente:

| caso | passa no HEAD porque |
|---|---|
| `test_RED11_a_VIEW_nao_nomeia_o_modelo` | e assercao de **AUSENCIA**: no HEAD nao ha view nenhuma, entao nada nomeia o modelo e o verde e por vazio |
| `test_MORDE_o_criterio_do_RED11_se_a_view_nomear_o_modelo` | e **auto-contido** de proposito: corre o criterio de AST sobre um fonte de view **sintetico**, entao passa em qualquer arvore -- e por isso que ele vale |
| `test_RED9_regime_invalido_NAO_grava` | **VACUIDADE, e eu nao a tinha visto**: no HEAD a `form_empresa` nao grava o campo de jeito nenhum, entao *"valor invalido nao grava"* e verdade por ausencia de caminho. Ele so morde ACOMPANHADO do RED8, que prova que existe caminho de escrita |

O RED11 e assercao de ausencia e por isso ganhou o gemeo que MORDE -- sem ele, apagar o criterio deixaria o
selo verde. O RED9 e o caso que a medicao me ensinou: um teste que eu teria contado como prova e que, sozinho,
nao prova nada. Ele fica, porque junto do RED8 a dupla cobre as duas metades (grava o valido, recusa o
invalido); o que muda e o que eu digo dele.

**O RED, dito como esta.** O log da primeira rodada vermelha **se perdeu**: a redirecao apontava para
`logs/o223/` antes de o diretorio existir. RED que nao se le nao e evidencia (LEI-AKITA 5), entao a rodada
foi **REFEITA** -- modulo novo contra o codigo de **HEAD**, em copia (`/tmp/o223-red-head`: porta e
templates fora, `core/urls.py`, `core/views_config.py`, `colaboradores/views.py` e o command voltados a
HEAD) --, e o numero esta em `logs/o223/red_head.out`.

**MEDIDO.** `logs/o223/cura1.out`: `Ran 24 tests` / `FAILED (failures=1, errors=1)` -- as **duas** eram
defeito do MEU teste, nao da cura: a fixture dava `rbac_gerir_usuarios` e a `form_empresa` cobra
`editar_cadastro` (duas portas com acoes genuinamente diferentes), e `{{ motivo|default:linha.motivo }}`
explodia com `linha=None` no caminho de recusa. `logs/o223/cura2.out`: `Ran 24 tests` / `OK` -- **e este log nao e o verde desta fatia**, e eu so descobri isso conferindo a contagem antes do commit. Ele abre com `Found 24 test(s)`, e os dois modulos tem **25** casos agora (12 no da O223 e 13 no censo, contados por AST, sem nome de metodo repetido e sem decorador que pule nada): o `cura2` correu ANTES da ultima edicao dos modulos, e o mtime dele e o do arquivo de teste caem no mesmo minuto. Um caso ficou fora do verde. **Verde que nao cobre um caso nao e verde dos bytes que entram no commit**, entao o numero citado passa a ser o de `logs/o223/verde25.out`, rodado sobre a COPIA que a suite mediu -- e a cura nao foi mudar o log, foi rodar de novo. Fica aqui porque e exatamente a forma de erro que esta casa chama de numero sem medicao: o `OK` era verdadeiro, e descrevia outra arvore.
`logs/o223/vizinhos.out` (`core colaboradores`): `Ran 1934 tests` / `OK (skipped=21)` -- e eles importam,
porque e ali que moram `test_rota_pede_sessao`, `test_contract_configuracao_nao_mente` e
`test_admin_so_leitura`, os contratos que rota e view novas derrubam. `ruff` limpo nos 7 arquivos.

**L-006: 1 de 3 -> 2 de 3 clausulas no codigo, e a aritmetica esta declarada na celula** (a contagem antiga
ja creditava a 2a clausula pela metade LEITOR, e essa metade nao mudou hoje). FALTA a 3a -- `Empresa` fora
de `core/configuracao_efeito.py::TELAS` --, que e a MESMA falta da 2a metade da 2a clausula: UMA fatia fecha
as duas, o **pouso B**, com custo **MEDIDO por AST**: **7 declaracoes** -- as 6 chaves de POST que a
`form_empresa` le (`ativa`, `cnpj`, `dia_inicio_competencia`, `em_rollout`, `nome_fantasia`,
`razao_social`) mais o `regime_trabalhista`. A docstring do modulo dizia **12** por suposicao minha, e a
medicao a corrigiu antes de o numero virar lei.

**ESMERIL DESTE MARCO (ESMERIL-DO-MARCO, durante a espera da suite): `0` orfao e `2` linhas de fila, as
duas no MESMO arquivo que o marco mudou.** Orfao zero nao e por sorte: o corpo de escrita do command foi
**SUBSTITUIDO** pela chamada a porta, nao abandonado -- conferido verbo a verbo contra `git show HEAD:`, e o
`get_or_create` e o `save(update_fields=...)` que o HEAD tinha ali nao existem mais em sitio nenhum do
command. As duas linhas de fila, as duas para o pouso B, que ja abre este arquivo:
(1) o `--listar` monta queryset proprio (`order_by('empresa_id', 'praca_id')`) em vez de ler
`servico.listar()` -- e um SEGUNDO leitor com regra propria de ordem, LEI-AKITA 2, e nao se cura hoje porque
a suite cheia esta correndo sobre estes bytes;
(2) a docstring do modulo, linha 20, ainda diz que *"o `get_or_create` guarda aqui"* -- e desde hoje ele
guarda na PORTA. **Nao e numero errado, e endereco errado**, e endereco errado em docstring e o que faz um
programador novo procurar o escritor no lugar que nao o tem (LEI-AKITA 11). Por que nao curei agora, dito
sem enfeite: editar o `.py` depois de a suite cheia comecar faria os bytes do commit divergirem dos bytes
MEDIDOS, e esta casa ja me ensinou duas vezes hoje que comentario nao e inerte -- ha selo que varre TEXTO.
Curar custaria 23 min de suite para uma frase; declarar custa uma linha e o pouso B ja abre o arquivo.

**O POUSO TEM DOIS COMMITS, E A ORDEM FOI MEDIDA, NAO ESCOLHIDA.** O primeiro commit leva o codigo, os
tres caminhos novos do `core/urls.py` e os docs, e o `bin/deploy.sh --sem-migrate` vem nele, sem nada no meio
(L-107). O `templates/core/config/hub.html` fica FORA dele e entra **depois do reload**, num segundo commit,
com a tela provada por `curl`. A razao e aritmetica: o link do hub cita `{% url %}` de uma rota que nasce
**neste mesmo marco**, e template vale no instante da gravacao enquanto o `.py` espera o reload -- gravar o
link antes do reload poe o hub de **todos** os admins em `NoReverseMatch`, que e exatamente o 500 que eu
causei hoje de manha no `empresa_form`. Commit de template nao precisa de reload, entao a segunda metade nao
custa um segundo deploy.
**A ordem inversa (deploy antes do commit) tambem dava zero 500 e foi DESCARTADA POR MEDICAO:**
`bin/deploy.sh:326` carimba `COMMIT=$(git -C ... rev-parse HEAD)` em `logs/deploy.stamp`, e esse carimbo e
lido por `bin/import_tardio_contra_o_ar.py` -- o vigia que decide se um vermelho nasceu **depois** do deploy e
so entao reverte. Deployar antes de commitar escreveria `d20a0e45` como "o commit no ar" com o codigo da O223
rodando: carimbo mentindo para o unico leitor que o usa para reverter. **Quase segui por esse ramo errado por
causa de um grep meu:** o primeiro padrao pedia `git ` colado ao verbo e voltou **0 linhas**, porque o
`deploy.sh` escreve `git -C <raiz> rev-parse` com o `-C` no meio -- e 0 linhas se leu como "o deploy.sh e
git-agnostico", que e falso. Com o `-C` opcional o mesmo grep acha **2 linhas**. Criterio pela FORMA conta
errado, e aqui contou a favor do ramo mais conveniente.

**A ORDEM CUMPRIU-SE, E A PRIMEIRA PROVA QUE EU DEI DELA ERA VAZIA.** Commit do codigo `ae775a7b` +
`deploy.sh --sem-migrate` (prova de casca: 16 estaticos, 5 paginas, 611 rotas em 2 urlconf; 3 rotas provadas;
selo BUG 128 verde; `importerror_500=0`). A rota nova respondeu **302** deslogada contra **404** de uma rota
inventada -- esse contraste e o que prova que ela EXISTE no worker, e nao que um catch-all respondeu. So
**depois** disso o `hub.html` entrou na arvore viva, e `/configuracoes/` respondeu 302 nas tres tentativas.
**Mas 302 deslogado nao prova nada sobre o template**: a view redireciona para o login ANTES de renderizar,
entao o `{% url %}` nunca e resolvido -- eu quase assinei como prova uma ausencia de sinal, que e a doenca
que o SELO ANTI-VACUIDADE desta casa existe para pegar. A prova que vale RENDERIZA: no mesmo molde da
`bin/prova_de_casca.sh` (container efemero, `config.settings.saas`, sem rede, `/app` so leitura),
`get_template('core/config/hub.html').render({})` devolveu **84.311 bytes**, os dois `reverse` resolveram
(`/configuracoes/aplicacoes-convencao/` e `.../nova/`), o href da rota nova **aparece** no HTML e o texto da
lapide **nao** vaza para a tela. A `prova_de_casca` nao pegaria isso sozinha: ela faz `get_template(nome)` em
5 paginas e o `hub.html` nao esta nelas -- e compilar nao e renderizar.
**Guarda errada minha, de novo pelo mesmo vicio:** a primeira guarda do ato 2 perguntava a rota a um
`docker exec python`, que e processo NOVO -- le o disco, nao a memoria do worker -- e nem chega a responder,
porque `config.settings` nao tem `ROOT_URLCONF` (o urlconf do tenant vem do compose). Quem sabe o que esta no
ar e o worker, e pergunta-se a ele por HTTP.

**O SEGUNDO COMMIT DO MARCO NAO TEM SHA EM LUGAR NENHUM, E ISSO E DE PROPOSITO.** O item de smoke do AVAIS
se gera DEPOIS do deploy, e eu o gerei citando o sha do commit 2; depois emendei esse mesmo commit para levar
os dois derivados (`PENDENTES_RONALD.json` e `AVAIS.md`), porque commit SO de docs nao existe (L-106) -- e a
emenda trocou o sha. O item ficou apontando para um commit **fora da historia**: `git cat-file -t` ainda o
resolve como objeto solto, mas `git log` nao o tem. E a mesma mentira de carimbo que me fez descartar a ordem
inversa do deploy, agora dentro de um artefato meu. Nao se cura citando o sha novo -- um commit nao pode citar
o proprio --, entao o item passa a dizer "o link do hub no commit SEGUINTE do marco", sem sha, como o TICKETS
e este RELATO ja dizem. Quem procura o commit 2 acha pelo `[O223] pouso A, 2 de 2` no titulo.

**O QUE SEGURA O PUSH: o smoke de clique.** A fatia toca template, e FRONT SEM SMOKE NAO SOBE -- o push
espera o clique nas DUAS cascas.

## O219 A0-b — O VIGIA DO TXT NASCE, E O ALARME CAI DE 6 DE 7 PARA 1 DE 7 (10/10 06:4x)
A A0-a deu **um** sitio para a forma e a prova do TXT. A A0-b faz a pergunta que a L-113 precisa
respondida antes de a clausula 1 cair: *"o gravado pode mudar debaixo de um arquivo entregue?"*. Fatia de
**leitura pura** -- zero mudanca em caminho de escrita, zero `--apply`, e o comando nao existe com flag de
conserto.

**DUAS perguntas, dois universos, e o terceiro estado de cada um e DIFERENTE.** `Q1 = VIGENTE`: *"o TXT que
saiu da casa ainda e o que o gravado de HOJE montaria?"* -- universo 7. `Q2 = ACERVO`: *"o conteudo guardado
bate com a prova guardada?"* -- universo 26. Na Q1 o terceiro estado e `CEGO` e **alarma** (nao consegui
comparar, e isso e defeito meu). Na Q2 e `HISTORICO` e **nunca alarma** (registro anterior ao corte da
migration 0005, `2026-07-02 13:13:20.524068+00:00`, que nao tem prova porque a prova nasceu depois).

**O QUE MUDOU O NUMERO: separar VALOR MOVIDO de PORTA MOVIDA.** A primeira versao do vigia comparava
linha-a-linha e acusava **6 de 7**. Seis de sete e o numero de um alarme que ninguem vai ler. Lendo o que
cada divergencia era, duas coisas MUITO diferentes estavam no mesmo balde: a matricula que esta nos **dois**
lados com **valor diferente** (dinheiro se moveu debaixo do arquivo -- e o perigo que a L-113 nomeia) e a
matricula que esta **em um lado so** (a PORTA mudou: ela entrou ou saiu do TXT, e de valor nao se pode
dizer nada). O corte e o conjunto `ambos = v_mat & h_mat`, e so o primeiro grupo alarma.
**MEDIDO PELO COMANDO REAL em prod** (`logs/a0b_comando.out`):
`Q1 universo 7: bate=1 diverge=6 incomparavel=0 (cego=0 historico=0)` ·
**`VALOR MOVEU: 1 competencia(s), 27 matricula-competencia | PORTA MOVEU: 6 competencia(s)`** ·
`Q2 universo 26: bate=25 diverge=0 incomparavel=1 (cego=0 historico=1)`.
**De 6 de 7 para 1 de 7**, sem mexer em tolerancia e sem tirar caso da lista -- o que mudou foi a pergunta
ter deixado de misturar duas.

**O UNICO VALOR MOVIDO TEM DATA, HORA E COMMIT.** emp2/08, `pk=19`, `gerado_em=2026-09-01 10:08:07-0300`:
**27 matriculas, TODAS na rubrica 0025** (adicional noturno), `entregue 263.964 (2.639,64 h)` contra
`hoje 230.968 (2.309,68 h)`, delta **-32.996 (-329,96 h)**. A razao e **7/8 em 27 de 27** (mat 224:
686 -> 600, e 686x7/8 = 600,25 -- a diferenca e arredondamento), entao e **causa unica**, nao 27 casos.
A causa tem commit: `HORA_REDUZIDA_12X36_EM_SECO = False` (`core/regua_cct.py:123`) entrou em `2206451b`,
01/09 **14:59** -- **4h51 DEPOIS** de o TXT sair. Direcao, pela clausula: a hora reduzida faz o adicional
**MAIOR** (60/52,5 = 8/7), logo **o entregue esta COM reduzida e o gravado de hoje esta SEM** -- o arquivo
que o Dominio recebeu paga **mais** do que o parecer diz devido. Corrigir **reduziria** o pagamento de 27
pessoas numa competencia **EXPORTADA e paga**: isso e **mesa com os dois numeros** (L-092, *"a diferenca
tem dono"*), **nunca fatia**. Entrou nos AVAIS.

**A PORTA MOVIDA conta TRES historias, e o vigia nao as mistura** (ele imprime `status` e `motivo` de
`classificar_export` ao lado de cada uma, sem tabela propria): (1) `so_no_vigente motivo=rescisao_modulo_proprio`
-- 6 em emp2/08 e col954 em emp2/09, **correto** pela lapide de 31/07 (TRCT, art.477 par.6); (2)
`so_no_vigente motivo=furo_espelho|ferias_com_batida` -- sao os **44** da pergunta de lei la em cima; (3)
`so_em_hoje status=entra` -- 5 em emp2/09, 2 em emp3/09 e 1 em emp3/07, e e a classe da **pauta DP**: col900
confere digito a digito com a pauta que ja estava na mesa, e a medicao e **mais larga que a pauta**.

**DOIS CEGOS DECLARADOS, nomeados e nao curados.** (a) De matricula que **nao e emitida hoje** o vigia **nao
pode** dizer se o gravado dela tambem se moveu -- ele compara LINHA. `porta_moveu` autoriza *"nao ha valor
comparavel"*, e **nunca** *"nada mudou"*. (b) `mat 570` e `mat 637` de emp3/07 voltam `status=None
motivo=None`: estao no TXT entregue e **hoje nao tem item nenhum** no `classificar_export` de emp3 -- nem
entra, nem fora. Virou linha de fila, nao cura agora.

**A FIACAO: ELO, nao horario.** O vigia entrou como **ultimo elo dos ENCADEADOS**, com gatilho
`recusar_ponta_pequena`. Nao e gosto: a Q1 **le a celula viva** -- `classificar_export` delega a
`motivos_retencao_celula`, que julga pelo `CelulaDia.veredito`, cujo escritor unico (`lavrar_veredito`) e
acionado pelo `processar_cartorio`. Por minuto fixo eu perguntaria *"o gravado se moveu?"* sobre celula que o
cartorio ainda nao julgou, e celula pela metade vira **porta movida que amanhece sozinha**. Por isso o
`depende` declara `('recusar_ponta_pequena', 'processar_cartorio')`: dependencia declarada e um **fato** que os
contratos de ordem leem. Confirmado tambem que **nenhum cron diario escreve `FechamentoMensal`**
(`recalcular_fechamento` nao esta nos CRONS diarios), entao a unica autoridade diaria que o vigia le e a
celula. **Sem `--empresa`**, de proposito: `bin/encadeado.sh` (flock + espera pelos carimbos das tres) garante
UMA corrida depois de TODAS, o universo e o acervo inteiro de `ExportacaoDominio`, e a Q2 nem olha empresa.
**DURACAO MEDIDA, nao digitada** (LEI-AKITA 8, e o B6 ja diz que a duracao deixou de ser digitada):
`logs/sonda/a0b_duracao.py` chamou o **comando real** em prod e cronometrou -- **7,0 s e 6,9 s, duas corridas
identicas, 124 linhas de saida cada**. O `teto_s=900` do ENC **nao e a minha duracao**: e o teto de ESPERA
pelo carimbo do **gatilho** (`recusar_ponta_pequena` mede 30 s), e a minha duracao entra no
`crons_duracao.json` pelo medidor que le o placar.
**E O ALARME TEM LEITOR NOMEADO, porque alarme sem leitor e selo provado e mudo.** O `--alarme` sai com
`SystemExit(2)`, e o 2 e a convencao da casa para *alarme de negocio*: o `bin/cron_run.sh` grava a corrida em
`app/logs/placar.jsonl` com `"exit":2`, o `core/management/commands/pipeline_placar.py::medidos` conta isso
como `alarmes` (e nao como `erros` -- comentario ESMERIL2-ALARME no proprio sitio), e **de proposito nao abre
pauta de TI**: o `cron_run.sh` so chama `cron_quebrado` para `exit != 0 e != 2`. E o mesmo caminho pelo qual o
`supra_juiz --alarme` grita o A6 desde 09/09. O texto do grito fica em `logs/vigia_txt_dominio.log`.

**UMA CORRECAO DE ROTULO DE HORA, minha, medida contra o `date` e nao contra a cabeca.** Escrevi a primeira
versao desta secao e do item de mesa com `07:5x`/`08:05`, e o `date` no ato devolveu **06:47** -- 1h15 a
frente. Os horarios certos sao: o comando real correu as **06:12** (`logs/a0b_comando.out`) e a sonda de
duracao as **06:21** (`logs/sonda/a0b_duracao.py`), entao o comentario do `ENC` que diz *"duas corridas
identicas em 10/10 07:5x"* esta com a hora errada e se corrige para **06:2x** no commit da raia -- nao agora,
porque a suite esta montando aquela arvore e nao se edita arvore que medicao monta. Os **7,0 s e 6,9 s** nao
mudam: foram cronometrados pelo relogio do processo, nao pelo meu rotulo.

**O ACHADO DE DENTRO DO MARCO: o contador do tabuleiro lia METADE do universo.**
`gerar_diagrama.py::bloco_tabuleiro` montava `por_papel` a partir de `CRONS` -- e `CRONS` **nao tem os
ENCADEADOS**. Os **sete** elos da corrente da manha nao tinham papel nenhum no desenho, e o numero publicado
era **20 vigias com 21 declarados**. Papel declarado que o desenho nao mostra e papel nao declarado para quem
le o desenho. Pior: `juizes_por_varredura()` **ja** contava a corrente -- a propria docstring dela diz *"OS
ENCADEADOS CONTAM"* --, entao uma divida certa aparecia ao lado de um contador pela metade, que e exatamente
a mistura que faz um placar **parecer** coerente. Cura na origem: `por_papel` passa a ler
`todas_as_entradas()`, a fonte unica de *"quais crons existem"*. **RED evidenciado**: o selo do diagrama
(`core/tests/test_selo_diagrama_do_codigo.py`) ficou VERMELHO com 2 falhas -- `AssertionError: 1 != 0 :
docs/ARQUITETURA.mmd divergiu do codigo` -- e voltou VERDE com o `.mmd` regenerado pelo
`manage.py gerar_diagrama` (nunca a mao): **57 -> 58 nos diarios, 41 -> 43 arestas `depende`, 20 -> 21
vigias**. Selo novo nao fez falta: o selo que existe **morde a volta** sozinha, porque o `.mmd` divergiria.
A regeneracao correu em container montando a RAIA no cpuset de teste, com `--user $(id -u):$(id -g)` -- o
`bin/gerar_diagrama.py` faz `docker exec saas_core` e escreveria na arvore VIVA.

**DUAS LINHAS DE DOC CURADAS NA ORIGEM, porque as duas tinham envelhecido.** (1) A secao 7 do CLAUDE.md
listava `06:36 apurar_furos_diarios` e `06:40 reconciliar_perguntas_orfas` **por minuto**, e os dois nao tem
mais horario desde o corte BLOCO-DA-MANHA-SATURADO: quem procurasse por minuto nao acharia. O instinto errado
era acrescentar o vigia aquela lista; o certo foi trocar a linha pela **corrente declarada**, na ordem, dos
oito elos. (2) A celula `carimbo_gravado` do `FORA_DE_PIPELINE` tinha uma **bifurcacao em aberto** (*"ou vira
cron da competencia exportada ou sai daqui"*) e **nenhuma das duas saidas era a certa**: o que a investigacao
ganhou nao foi um escritor nomeado, foi um **vigia diario uma camada acima**. O carimbo FICA, e passa a ter
gatilho declarado -- **o alarme do vigia** -- e papel mais nitido: o vigia compara **linha de TXT**, o
carimbo desce ao **campo** do `FechamentoMensal`. Gatilho que era suspeita virou fato.

**A SUITE CHEIA, LIDA PELA FORMA ESTRITA E COM A CONTA DA VACUIDADE FEITA ANTES DO VEREDITO.**
`logs/o219/a0b_suite.out`: `Ran 10297 tests in 1408.453s` / `OK (skipped=42)`, `rc=0` do processo,
**0** linha `^FAIL:` ou `^ERROR:`, veredito lido por `grep -nE '^(OK|FAILED)( \(.*\))?$' | tail -1`.
A conta que importa vem ANTES do `OK`: a A0-a correu `Ran 10265`, esta correu `Ran 10297`, **+32** --
e os 32 estao nomeados, nao estimados: **31** `def test_` no modulo novo
`folha/tests/test_l113_a0b_vigia_do_txt.py` mais **1** no existente, que foi de 12 (no HEAD) para 13.
Verde com `Ran` IGUAL ou MENOR seria verde oco, porque significaria modulo nao coletado -- e e por isso
que a conta vem primeiro. Selos de alvo depois, na arvore da raia: 61 testes `OK` em 52,274 s
(diagrama + bolha + relogio + os dois modulos da L-113), `ruff check --no-cache folha/ config/ core/`
**All checks passed!**, e o `core.tests.test_selo_pendentes_so_decisao` correu contra o JSON do **main**
(o que tem o item 12) -- `Ran 3 tests` / `OK` --, porque a copia da raia nao o tem e o selo le a arvore
em que esta montado.

**O ACHADO DE FORA DO MARCO VAI PARA A FILA, NAO PARA A FATIA, E ELE QUASE ME FEZ CULPAR A MINHA PROPRIA
FIACAO.** Dos 8 selos de host VERMELHOS na raia eu havia disposto dos 8 re-rodando-os VERDES no `main` --
conclusivo para 7, **nao** para o `bin/tests/test_furo_encadeado_ao_cartorio.sh`: ele e justamente o selo
da corrente dos ENCADEADOS, e o `main` **nao tem o elo que eu acabei de somar**, entao "verde no main" nao
responde por uma raia que mexeu na corrente. Medido no sitio em vez de suposto: `bin/encadeado.sh:24`
crava `R=/home/ronald/saas-hasner`, entao a corrida disparada DE DENTRO da raia escreveu o ALARME em
`logs/encadeado/_echo_teste.log` do **main** -- a linha esta la, `2026-10-10T07:05:45-03:00 ALARME:
_echo_teste nao rodou -- faltaram 3 carimbo(s)` -- enquanto `wt-l113/logs/encadeado/` ficou **vazio**, que
e onde o selo procura. RED de raiz trocada, nao de fiacao: os quatro casos do selo perguntam por
`cron_run.sh`, por `apurar_furos_diarios` e por duas corridas de `_echo_teste`, e o elo novo nao aparece em
nenhum deles. **E A MEDICAO DEVOLVEU UM SEGUNDO FATO, que e a linha de fila**: esse selo mexe em carimbo
VIVO por desenho -- o `_limpar` dele apaga `logs/fim/processar_cartorio.{2,3,4}.<dia>` e
`logs/fim/apurar_furos_diarios.-.<dia>` --, e os quatro de hoje estao **AUSENTE** agora, contra **64**
carimbos do dia que seguem de pe. Hoje nao ha dano (a corrente do dia ja fechou -- `fechar_cobranca_com_lastro`
carimbou 06:40 -- e nenhum cron a re-invoca), mas um selo que apaga o carimbo de idempotencia de um cron de
prod e capaz de fabricar ALARME falso no proximo que o leia. **Fila, pela ESMERIL-DO-MARCO**: achado fora do
marco e linha, nunca trabalho agora.

**O POUSO, E O QUE ELE ME ENSINOU SOBRE A FORMA DA L-107.** Eu pedi
`git merge --no-commit raia-l113` pela forma da L-107 e recebi um **FAST-FORWARD**: `--no-commit` **nao**
suprime o fast-forward, e o main estava exatamente um commit atras da raia. Entao nao houve commit de
merge, o `MERGE_HEAD` nasceu **vazio**, e `10de79cb` entrou no main sozinho -- com o codigo **ja na arvore
viva**, que E o bind-mount, as 07:11. A L-107 existe justamente contra essa janela, e ela se fechou do jeito
certo porque o `bin/deploy.sh` veio **imediatamente** depois, sem nada no meio. Fica como FORMA e nao como
susto: quem quer o commit de merge pede `--no-ff` junto com o `--no-commit`; quem pede so `--no-commit`
pode receber um fast-forward e deve tratar o pouso como **ja comecado**. Os docs do marco entraram em
`d20a0e45`, no mesmo ato, pela L-106.

**PROVA DO AR, NAO DO DISCO.** `bin/deploy.sh --sem-migrate` rc **0**: `migrations pendentes no schema do
cliente: 0`, `sombra: carimbo dia=20261010 status=OK tipo=completa diverge=0 erros=0`, prova de casca com
`16 estaticos conferidos, 5 paginas compiladas, 608 rotas importadas em 2 urlconf(s)`, tres rotas provadas
(core `/health/` 200, ui `/colaboradores/` 302, mensageria `/health/` 200), selo BUG 128 verde nas tres
cascas e `importerror_500=0` na janela 06:12-07:12. O elo novo entrou no crontab por `bin/crons.sh install`
(backup em `logs/crontab_backup_20261010_071308.txt`) e o `check` seguinte fecha a conta:
`crontab == config/crons.py (102 linhas)` -- o `check` anterior acusava exatamente a diferenca de **1** elo,
nas tres linhas do cartorio, porque **as tres empresas o chamam** e o `flock -n 9` faz so a primeira
trabalhar.

**E O COMANDO RESPONDEU DO CODIGO PUBLICADO, nao do que eu media antes de commitar**: rodado em prod as
07:13:49, **8 s**, `Q1 ... universo 7: bate=1 diverge=6 incomparavel=0 (cego=0 historico=0)` com
**VALOR MOVEU 1 competencia e 27 matricula-competencia** contra **PORTA MOVEU 6 competencias**, e
`Q2 ... universo 26: bate=25 diverge=0 incomparavel=1 (cego=0 historico=1)` -- o MESMO numero da medicao de
06:12, agora saindo do ar. Sem `--alarme` ele sai **0**; com `--alarme` sai **2** e a linha e
`ALARME TXT-DOMINIO: {'q1.competencias_com_valor_movido': 1}` -- **uma** chave, nomeada: nao e a Q2, nao e
`cego`, nao e o `diverge=6`. O alarme aponta para a unica coisa que tem dono na mesa, e as 124 linhas de
saida nao trazem **nenhum nome** de colaborador.

`LEI-AKITA: origem=folha/services/vigia_txt.py (a pergunta nasce onde a forma e a prova do TXT ja moram, apos a A0-a), testemunha=folha/export.py::classificar_export + montar_linhas_export + hash_do_txt (nenhuma regra propria de dia, soma ou classe) e ponto/e6_oraculo.py::dono_da_divergencia para dono, RED=folha/tests/test_l113_a0b_vigia_do_txt.py::test_MORDE_matricula_em_um_lado_so_e_PORTA_MOVEU_nao_valor + ::test_MORDE_a_q1_nao_alarma_por_diverge_e_sim_por_valor_movido + core/tests/test_selo_diagrama_do_codigo.py (2 falhas antes da regeneracao), quem-mais-le=folha/export.py sem mudanca de assinatura publica, 1 ENC novo nos ENCADEADOS, 1 papel novo no PAPEL_DO_CRON, 1 contador curado em gerar_diagrama.py, juizes novos=0 (o censo deixou de carregar `dono` -- tabela motivo->dono seria juiz novo e pede corte pela TRAVA JUIZ-NOVO)`

## RAIA QUE NAO POUSA — O MOTIVO EM UMA LINHA CADA (L-105, medido 10/10 03:3x)
A **L-105** manda a raia que nao pode pousar dizer o motivo em UMA linha, e eu nunca tinha escrito
essas linhas: o censo de agora acha **0** ocorrencia de `raia-celula`/`wt-celula` no BACKLOG e no
RELATO. O contador que isto vira (*"raias com commit a frente do main ha mais de 6 h, esperado 0"*)
e fila 2 por ordem dele -- **ate la a guarda e a conduta, e a conduta e esta tabela**. Medido com
`git rev-list --count main..<raia>`, nao de memoria:

| raia | a frente do main | motivo de nao ter pousado | lei / `!` / janela |
|---|---|---|---|
| `raia-celula` (O228) | 2 (`b4d938aa` produto, `7cb0efbb` instrumento) | a suite da raia deu `FAILED (failures=1, skipped=42)` sobre `Ran 10242`, e a falha **nao e dela** -- e alheia, JA curada no main por `1c5b2e20`. O ato e: merge do **main NA raia** -> suite -> so entao o pouso, e o produto pousa ANTES do instrumento (L-105) | **janela** (pista de teste) |
| `raia-pdf` | 4 (ate `584265a9`, 29/09) | o ultimo commit dela **retira do contrato** a composicao de `total_trabalhadas` dizendo que e PERGUNTA DE LEI, com as duas formas em prod. Nao pousa sem a lei respondida, e ha ordem expressa dele de **nao mergear** | **`!`** |
| `raia-l113` (O219 A0-a) | **0** -- e o dado importante: a A0-a esta na arvore de `wt-l113` **sem commit**, entao `rev-list` a enxerga como zero. Pousa neste marco | — |
| `cert-ast` · `pousos-0410` | 7 cada, compartilhando `a13ec7da` e `cb84a4ac` | **MOTIVO NAO NOMEADO POR MIM.** Os commits de `pousos-0410` DIZEM *"pouso 1/3, 2/3, 3/3 entra no main"* (04/10 18:5x) e o main **nao os tem** -- ou pousaram por outra rota e o ramo ficou, ou nao pousaram. Nao vou escrever motivo que nao medi: isto e **linha de fila**, nao diagnostico -- e a linha existe: obra **O241** | a MEDIR |
| `lps-prova` · `tmp-ui` | 1 e 2 | o proprio commit de `lps-prova` se declara *"commit de ensaio, descartavel"*; `tmp-ui` traz o UI-GRADE-CALENDARIO *"COMMITADO SEM PUSH, aguarda o smoke"* (28/09) | **janela** (smoke dele) |

O que esta tabela NAO e: triagem das 47 worktrees. Ela nomeia as **6** raias com commit a frente do
main agora, e duas delas saem daqui com o motivo em aberto -- escrito como aberto, que e a diferenca
entre uma linha de L-105 e uma linha de conforto.

### O achado da espera: o padrao de veredito da suite ainda casa prosa de log (10/10 03:42)
Esperando a suite cheia da A0-a, a ESMERIL-DO-MARCO manda censo **do que este marco deixou orfao** e
manda achado de fora virar **linha na fila, nunca trabalho na hora**. O achado veio do meu proprio
log: ao ler o veredito pela forma que a secao 3 do CLAUDE.md ensina, a primeira linha que casou foi
`OK -- nenhuma divergencia em 2026-10-10.` -- **prosa de um teste**, nao o veredito do Django.
**PROVA:** `grep -nE '^(OK|FAILED)( |\(|$)' logs/o219/suite_a0a_cheia.out` devolve a linha **965**
com essa prosa, e `grep -rnE "\^\(OK\|FAILED\)" bin/` devolve **12 linhas vivas em 9 arquivos** com o mesmo
padrao (o `bin/pre-push.sh` nao entra: padrao outro, papel de formatador, veredito pelo rc da L-074), entre eles `bin/regua.sh:170-171` (o veredito da REGUA) e `bin/isolamento.sh:34`.
O proprio CLAUDE.md ja mediu isso em 08/10 14:03 e escreve que o espaco nao cura -- e o comando
literal que ele ensina continua aceitando `OK` seguido de espaco. O que salva as 12 hoje e o
`tail -1`, e ele salva **enquanto a suite termina**: suite morta no meio deixa a prosa como ultima
linha casada e o teste da **VERDE em suite que nao acabou**. Nao curei agora, por duas razoes
escritas: nao e deste marco, e a cura certa e **uma funcao unica** lida pelos 9 arquivos, nao a 13a copia do
padrao. Virou a obra **O240**, com o numero e o PROIBIDO (nada de allowlist de log que pode dizer
`OK`, nada de corrigir em 11 lugares). Aqui eu passei a usar a forma estrita -- fim de linha ou abre parenteses depois do veredito --, e e
por ela que o veredito desta fatia se le, nao pela do CLAUDE.md.

## O219 A0-a — A FORMA DO TXT DO DOMINIO TINHA CINCO COPIAS, E DUAS SONDAS MINHAS MORRERAM ANTES DESTA (10/10 03:1x)
A L-113 manda a competencia exportada deixar de BARRAR o recalculo, e o unico risco que a casa soube
nomear contra isso foi *"o gravado passa a mudar EM SILENCIO debaixo de um TXT entregue"*. Para
responder isso e preciso PERGUNTAR se o TXT entregue ainda e o que o gravado montaria -- e essa
pergunta nao tinha onde ser feita: a FORMA do arquivo estava escrita TRES vezes e a PROVA dele DUAS,
e nenhuma das cinco copias respondia a pergunta. Cada uma montava a sua.

**O NUMERO 5 NAO E MEU, E DO SELO.** O predicado do selo novo, rodado contra o HEAD anterior, acha
exatamente as cinco: `regerar_txt_dominio` (forma + hash), `models.py` (hash), `validacao_pdf.py`
(forma) e `views.py` (forma).
**PROVA:** `logs/o219/SELO_RED_forma_do_txt.txt` -- *"ARVORE HEAD (a364847b): 30 arquivo(s) de
producao em folha/, 5 violacao(oes)"*, com o predicado EXTRAIDO do proprio selo por regex em vez de
reescrito (replicar a logica na sonda seria a segunda verdade). Na arvore curada: **0**.

**O QUE NASCEU**, em `folha/export.py`, ao lado do `nome_canonico` que ja era fonte unica pelo mesmo
caso: `conteudo_do_txt` (a forma), `hash_do_txt` (a prova) e `txt_vigente_x_gravado` (o juiz da
pergunta). O Dominio le o arquivo por POSICAO -- terminador diferente e ARQUIVO DIFERENTE para ele
ainda que o olho nao veja --, e com tres copias o PDF podia um dia validar um conteudo que nao era o
que saiu.
**PROVA:** `folha/tests/test_l113_a0_forma_do_txt.py`, **`Ran 12 tests` / `OK`** em 0,104 s; `ruff
check folha/` **All checks passed!**; e o caso que amarra os dois lados -- `ExportacaoDominio.registrar`
(quem GRAVA a prova) e `hash_do_txt` (quem a CONFERE) dao o MESMO sha256, que era o pior lugar
possivel para divergir: a divergencia apareceria na tela como *"o TXT mudou"* sem nada ter mudado.

**UM FURO REAL CURADO NO CAMINHO: o empate de carimbo.** `regerar_txt_dominio` escolhia o TXT
anterior por `.order_by('-gerado_em').first()`, que e a MESMA pergunta que
`folha/models.py::anotar_vigencia` responde -- com um desempate a mais, `(gerado_em, pk)`. Em empate
de carimbo os dois podiam escolher registros DIFERENTES, e o comando guardaria o anterior de um
enquanto comparasse contra o outro.
**PROVA:** `test_MORDE_empate_de_carimbo_escolhe_por_pk` (dois registros com o MESMO `gerado_em`, o
juiz escolhe o pk maior); e a guarda no comando e `raise CommandError`, nao `assert` -- `python -O`
apaga `assert` e com ele a guarda.

**O NOME MUDOU ANTES DE POUSAR, e isso e a TRAVA JUIZ-NOVO funcionando.** Ele nasceu
`txt_divergente`, e a casa ja tem `cartao_x_txt_divergentes` (relatorios/, corte 17/09) respondendo
OUTRA pergunta: se o CARTAO do colaborador bate com o TXT. Dois nomes 90% iguais para duas perguntas
diferentes e vocabulario paralelo -- e vocabulario paralelo so aparece depois que os numeros se
afastaram.
**PROVA:** censo dos chamadores fora de `folha/` antes do rename (`grep` de 5 nomes em `--include=*.py`):
os 10 arquivos que casavam citavam o contador ANTIGO, nenhum monta TXT nem calcula hash; o novo nome
diz os dois lados, como o dele.

**E O SELO PRENDE O AVESSO DE UM FALLBACK.** Empresa sem codigo no Dominio nao tem TXT para montar e
o juiz PROPAGA o erro da autoridade que ele le. Um `try/except` devolvendo `divergente=False` ali
seria o `[]` de dois sentidos -- lampada apagada lida como *"esta tudo em ordem"*.
**PROVA:** `test_MORDE_sem_integracao_o_juiz_PROPAGA_em_vez_de_dizer_que_nao_diverge`; o caso nasceu
de um ERROR real da primeira rodada (11 testes, 1 error), nao de imaginacao.

**NENHUMA CLAUSULA DA L-113 SE MOVE AQUI, de proposito:** a linha dela em `docs/LEIS.md` segue
`SO-NO-PAPEL -- 0 de 3`. Isto e pre-condicao, nao clausula.

### AS DUAS SONDAS QUE EU MATEI COM A MINHA PROPRIA MEDICAO (fica registrado, e o custo foi meu)
A pergunta de A0-b -- *"o gravado mudou debaixo do TXT?"* -- eu tentei responder DUAS vezes errado.

**(i) PELO HASH DO TXT: 7 de 7 competencias "divergem", e a causa dominante nao e o gravado.** O
contador nasceu dizendo 7/7 e eu quase o publiquei como alarme. Ele confunde TRES causas: a populacao
mudou (RETENCAO), a traducao mudou (`_rubricas_map`, `eventos_do_fechamento`, `_ciclo_do_colab`,
`feriado_12x36_em_dobra`, `datas_falta`) e o gravado mudou. **So a terceira e o risco da L-113.**
**PROVA:** `logs/o219/CONTRATO_DE_ENTRADA.md`; a retencao por `furo_espelho` mede **200/238/94** nas
tres empresas -- ela sozinha move a populacao do TXT e portanto o hash.

**(ii) PELO CARIMBO `atualizado_em > foto`: 125 linhas, e 123 sao RUIDO DE UM CRON DE ESCALA.** O
contador leu 125 dia-colab em 4 de 7 competencias em 7,18 s, e **123 deles carregam
`previsto_em IS NULL`** -- a assinatura de `ponto/services/fechamento.py::invalidar_previsto:965`,
que ao mudar escala anula o previsto materializado em TODA competencia daquele colaborador, exportada
ou nao, e carimba `atualizado_em` **na mao**. Nao e defeito dele: invalidar o previsto MUDA o
registro. O defeito era o meu contador.
**PROVA:** `logs/o219/CONTRATO_DE_ENTRADA.md` -- distribuicao por dia e status, **0 de 125** pela
porta de aprovacao que eu havia DECLARADO no docstring como a falsa-positiva (declarei a porta errada),
e censo de escritores por AST.

**E O CARIMBO ERRA NOS DOIS SENTIDOS -- a casa ja tinha escrito isso, em arquivo que eu nao havia
lido.** `ponto/management/commands/carimbo_gravado.py` registra um falso NEGATIVO medido: o
`FechamentoMensal` de emp2 **09/2026 (EXPORTADA)** mudou entre 09:14 e 09:27 de 29/09 com **zero**
linhas `atualizado_em=hoje`, porque `auto_now` nao dispara em `queryset.update()`. Carimbo testemunha
*"alguem escreveu algo"*, nunca *"um NUMERO mudou"*.
**PROVA:** o docstring de `carimbo_gravado.py` e o censo dos 11 nomes excluidos, que aparece
IDENTICO em `ponto/portas/regen_exportada.py::_FORA_DO_HASH` -- a autoridade de *"o gravado mudou, e em
qual campo"* **ja existe, em duas copias literais**. A LEI-AKITA 4 em estado puro: a pergunta nao era
"qual a regra", era "qual leitor nao migrou".

### OS DOIS CASOS DE `previsto_em > foto`: NAO HA BYPASS VIVO (achado historico, vira linha de fila)
Dos 125, dois nao sao do cron: emp3 09/2026 col49 (`FechamentoMensal` pk4884, recalculo 30/09 22:27
contra foto de 30/09 20:07) e emp2 09/2026 col221 (pk4689, 03/10 00:55 contra 01/10 22:44) -- escritas
no gravado de competencia EXPORTADA, posteriores ao commit da trava (`ae058f68`, 27/09 10:12).
**A trava FIRA hoje para os dois, e nao foi pela porta.**
**PROVA:** reproducao na sombra dentro de `atomic()` com `raise` no fim --
`empresas_exportadas_no_escopo(9, 2026, colaborador_ids=[49])` = `[3]`, `([221])` = `[2]`, e
`recalcular_fechamento_mes` **RECUSOU** nos dois; as 7 linhas `regen_em_exportada` do `LogAuditoria`
nomeiam 414/416/418/515/616/877/932 e **nenhuma cita 49 nem 221**; e toda ocorrencia de
`recalculo_de_competencia_EXPORTADA` em `logs/` e de SUITE (`empresas=[1]`, `[499]`, `[1858]`), nenhuma
de prod. Nao se cura um caminho que nao se nomeou, e nao ha bug vivo: o provado e que a trava funciona.
**E as duas competencias estao em `marco='paga'`**, com `violacoes=['paga sem trancada']` -- vale dito
em voz alta antes da clausula 1: a trava que a L-113 remove e, hoje, a unica coisa entre um recalculo
e o gravado de uma competencia **ja paga**.

### O SELO DA CASA ME PEGOU, E A SUITE CHEIA E O MOTIVO (10/10 04:0x)

O modulo novo sozinho deu `Ran 12` / `OK` e os 97 vizinhos de `folha`+`relatorios` deram
`Ran 613` / `OK`. A suite **CHEIA** voltou **VERMELHA**, num selo que mora em outro app:

**PROVA:** `logs/o219/suite_a0a_cheia.out` -> `Ran 10265 tests in 1402.381s` ·
`FAILED (failures=1, skipped=42)` · rc do processo **1** ·
`FAIL: test_MORDE_teste_novo_com_relogio_solto_e_vermelho
(core.tests.test_selo_teste_sem_relogio)` ->
`Lists differ: ['folha/tests/test_l113_a0_forma_do_txt.py'] != []`.

A falha era **minha**: o caso (d), o do empate de carimbo, precisa do *mesmo instante duas
vezes* e eu o escrevi lendo o relogio real. O selo TESTE-SEM-RELOGIO (corte dele de 19/09)
existe exatamente para isso, e o que ele proibe nao e o relogio: e **nascer** um teste novo
que o le sem congelar.

Cura na **ORIGEM**, e nao na allowlist -- `core/tests/testes_sem_relogio.txt` **so encolhe**
(`test_a_lista_so_encolhe`), entao escrever o meu arquivo nela era a porta proibida. O caso
passa a usar um **instante FIXO** dentro da competencia que ele grava. Isso e **mais
restritivo que congelar**: congelar intercepta a leitura, o instante fixo **remove** a
leitura -- e de quebra tira o caso da classe que o `bin/regua_calendario.sh` caca (virada de
dia, dia 20, dia 21 00:05). O `from django.utils import timezone` saiu no mesmo ato, porque
era o unico uso.

**PROVA da cura:** o predicado do proprio selo, rodado sobre o arquivo curado, devolve
`sitios de relogio solto = 0`; `logs/o219/alvo_pos_cura.out` -> `Ran 15 tests` / `OK`, rc **0**
(os 12 casos do modulo mais os 3 do selo, juntos); `ruff check folha/` no container ->
`All checks passed!`; e a pasta inteira de selos de host (**66**) sem um vermelho.

**E A PROVA QUE FALTAVA -- A SUITE CHEIA SOBRE A ARVORE CURADA, VERDE:** `Ran 10265 tests in
1401.635s` / `OK (skipped=42)`, **0** linha `^FAIL:` ou `^ERROR:`, **rc do processo = 0**
(`logs/o219/suite_a0a_cheia2.out`, veredito na linha 1990 pela forma ESTRITA
`^(OK|FAILED)($| \()` com `tail -1`). **O MESMO universo do run VERMELHO** -- 10265 nos dois --,
entao o verde nao e um run menor disfarcado de cura. E a notificacao da task dizia *"failed with
exit code 1"*: o rc do COMPOSTO era o do ultimo comando, um `grep -c` que devolve **1 quando a
conta da 0**. O veredito da suite e o `rc do PROCESSO`, que a propria linha imprimia -- a mesma
classe do `rc depois de pipe e do ultimo`, agora vista num `;` em vez de num `|`.

A arvore medida e a que pousou: `raia-l113` e `main` tinham a MESMA base `a364847b`
(`git rev-list --count raia-l113..main` = **0**, `merge-base` = `a364847b`), entao a suite correu
sobre `main` + A0-a, nao sobre uma base velha. Commit da raia: `89570af4`, 6 arquivos, 350
insercoes.


### A0-b, A MEDICAO ANTES DO CODIGO: **A IMPRESSAO DIGITAL DO GRAVADO TEM TRES COPIAS, E A TERCEIRA DISCORDA DAS OUTRAS DUAS EM UM CAMPO** (10/10 04:0x)

A lista de pendentes mandava eu LER `lavrar_dias_pagos::_hash_dos_fechamentos` em vez de aceitar
que ele e *"o mesmo universo"* das outras duas -- porque essa frase saia de **duas docstrings**, e
nao de mim. Lido, e o contrario:

**PROVA, por comando:**
- `ponto/management/commands/carimbo_gravado.py:27-28` -> `FORA` com **11** nomes;
- `ponto/portas/regen_exportada.py:35-36` -> `_FORA_DO_HASH` com os **MESMOS 11**;
- `ponto/management/commands/lavrar_dias_pagos.py:68-69` -> `_fora` com **10**. O que falta e
  **`previsto_em`** -- isto e, o terceiro **INCLUI** no hash o campo que os outros dois excluem.

E a casa ja escreveu por que ele tem de ficar fora, com data: `hash_do_gravado` diz
*"`previsto_em` fica FORA do universo, e a razao foi medida em 29/09: ele e carimbo de controle e
muda a cada recalculo, entao inclui-lo faria o hash acusar mudanca em toda corrida -- ruido que faz
a proxima pessoa parar de olhar o alarme"*. E **exatamente o campo** que o `invalidar_previsto`
(`ponto/services/fechamento.py:965`) anula em TODA competencia do colaborador a cada mudanca de
escala -- os **123 de 125** que fizeram o meu contador ser 98% ruido. Ou seja: a terceira copia
carrega o defeito que as outras duas ja curaram, e a diferenca entre elas e de **um nome**.

Mais tres fatos que decidem a forma da A0-b, e nenhum deles vem de docstring:
1. os **tres** derivam os nomes de `FechamentoMensal._meta.concrete_fields` (campo novo entra sem
   ninguem lembrar) -- isso e **bom** e se preserva; o que nao se preserva e o conjunto de
   exclusao escrito **tres vezes**;
2. `_hash_dos_fechamentos` **nao e persistido**: e `md5` truncado em 12, calculado em memoria por
   corrida e **impresso**. Entao **nao** e um baseline sem migration, como eu havia suposto -- a
   suposicao morreu aqui, antes de virar codigo;
3. `logs/carimbo_gravado/` tem **0 arquivo**, entao nao ha foto velha de onde partir. O baseline da
   A0-b nasce no `ExportacaoDominio.registrar` -- no ato em que o TXT sai da casa --, que e o unico
   instante em que *"o gravado que gerou ESTE arquivo"* existe sem precisar de passado.

**Nao se cura aqui**: a A0-a esta no portao de pouso e e deduplicacao pura em 5 arquivos de `folha/`
com a suite cheia ja medida contra essa arvore -- tocar `ponto/` agora invalida a medicao e faz a
fatia deixar de ser o que a sua propria linha diz que e. A cura e a A0-b, **o proximo passo deste
mesmo item**, e ela nasce ja sabendo que **o juiz da impressao do gravado e UM** e que
`previsto_em` fica fora dele.

### UM ID CARREGANDO DUAS OBRAS: a contradicao que eu achei ANTES de commitar com o rotulo `[O219]` (10/10 04:1x)

Eu ia commitar a A0-a com o rotulo `[O219]` e fui conferir a linha dele. **A celula de OBRA
(col2) da linha `O219` do BACKLOG nao fala de TXT nenhum**: ela e a `REGRA-NA-IMPRESSAO`
(aval 07/10 19:xx) -- a versao da regra entrando como quinto insumo do
`ponto/services/cartorio.py::impressao_insumos`, com os adendos `AVISO-E-ESCOLHA` (19:5x) e
`HORIZONTE-PADRAO-ABERTA-MAIS-ANTERIOR` (23:4x). **A minha A0-a e outra coisa**: a forma e a
prova do TXT do Dominio passando a ter UM sitio. E a linha `O219` do TICKETS (`:121`) diz, na
coluna da frente, *"estrutural (familia folha/export) -- L-113 O-SISTEMA-CALCULA-O-QUE-TEM,
fatia A0-a"*. Um id, duas obras.

**NAO INVENTEI ID NOVO, E NAO RENOMEEI LINHA NENHUMA.** Medi, e a resposta estava dentro da
propria celula dele: a obra `REGRA-NA-IMPRESSAO` declara, com estas palavras, **"SEM EXCECAO
DE COMPETENCIA (L-113)"** -- que E a clausula 1, a recusa de recalculo em competencia
exportada. E o adendo dele de 09/10 18:3x, no topo do BACKLOG, e literal: *"a O219 e a cura
dessa classe e tira a L-113 do papel"*. Entao a O219 **tem** duas pernas, e isso nao e
vocabulario paralelo: a impressao com versao e o caminho pelo qual a regra nova alcanca o dia,
e a clausula 1 e a trava que hoje barra esse alcance na competencia exportada. **A A0 e o
instrumento que a clausula exige**: sem um vigia que diga se o TXT vigente ainda bate com o
gravado, apagar a recusa e apagar a trava sem por nada no lugar.

O que eu mudei foi **a unica celula que e minha**: a de ESTADO (col4), que passa a dizer a
ponte em vez de so o progresso -- *"A0 e o instrumento que a clausula SEM EXCECAO DE
COMPETENCIA (L-113) desta linha exige: o vigia que ve se o TXT vigente ainda bate com o
gravado"*. A celula de OBRA (col2) e texto de aval **dele**, com dois adendos, e nao se
reescreve. LEI-AKITA 11: um programador novo tem de se situar sem arqueologia, e a arqueologia
que eu acabei de fazer e exatamente o que a ponte poupa do proximo.

**PROVA:** `awk -F'|'` sobre `app/docs/BACKLOG.md` -- a linha 252 e a UNICA com id `O219` e a
unica do bloco OBRAS que cita `L-113`; `grep -nE 'O-SISTEMA-CALCULA|txt_vigente|vigia do TXT'`
no BACKLOG = **0 linhas**, entao nao existe outra obra para onde a A0 pudesse ir. No TICKETS,
as linhas que citam L-113 sao `O219` (:121) e `O220` (:133, instrumento de contexto) -- nenhuma
terceira. Celula de estado reescrita com **295 caracteres** (teto L-109 = 300, cobrado por
`assert` que **ja me barrou duas vezes nesta mesma edicao**, em 415 e em 331), 5 colunas de
conteudo, zero `|` no texto. Selos de host do BACKLOG verdes: `test_fabricante_backlog.sh`,
`test_fabricante_le_a_ordem.sh`, `test_hook_stop_saida_valida.sh`,
`test_hook_parei_casa_id_inteiro.sh`, `test_hook_nao_cobra_congelado.sh` (rc=0 nos cinco).


### A LINHA DO CLAUDE.md QUE ENSINAVA O FURO QUE ELA MESMA CONDENA (curada neste marco, 10/10 04:3x)

O assunto do commit de pouso diz *"o veredito da suite se le pela forma estrita, nao pela do
CLAUDE.md"*, e eu fui conferir se isso ainda era verdade depois do paragrafo que a casa escreveu
em 08/10. Era, e de um jeito pior do que eu pensava: **o COMANDO da linha 197 ensinava
`^(OK|FAILED)( |\(|$)` enquanto a PROSA das linhas 201-206, logo abaixo, dizia com todas as
letras que esse padrao NAO CURA** -- *"`OK -- nenhuma divergencia` tem `OK` seguido de ESPACO,
entao casa igual"*. A prosa estava certa; o comando ao lado dela estava errado. Quem copiasse
copiava o furo -- e copiar o comando e exatamente o que se faz com uma linha que comeca em
`Veredito:`.

O furo me pegou **duas vezes**, e as duas estao medidas neste RELATO: em 08/10 14:03 ele deu por
verde um `.out` que terminava em `FAILED`, e em 10/10 03:4x um laco de espera meu deu a suite por
terminada com ela ainda correndo, casando a linha de prosa `OK -- nenhuma divergencia em
2026-10-10` da linha 965 do log.

Curado na ORIGEM, que e a propria linha: o comando passou a ser
`grep -E '^(OK|FAILED)($| \()' <log> | tail -1`, com o `rc do PROCESSO` (L-074) nomeado como
terceira perna, e a prosa abaixo passou a registrar que a linha literal tambem estava errada e
quando caiu. **Isto NAO e a O240** -- a O240 e a cura dos **12 sitios em 9 scripts** que carregam
o padrao solto, e ela segue na fila como instrumento, com commit proprio, porque a cura la e UMA
funcao que os 9 leem e nao uma 13a copia do padrao. O que se curou aqui e o ENSINO: enquanto a
linha 197 estivesse errada, a O240 nasceria copiando o furo de novo.

**PROVA:** `grep -n 'OK|FAILED' CLAUDE.md` -> as duas ocorrencias sao a linha 197 (o comando,
agora `($| \()`) e a linha 199 (a prosa que cita o padrao velho como historia). Selos que leem
o CLAUDE.md, todos rc 0: `test_suite_sh.sh` (recorta a sec.3 pela estrutura),
`test_lei_akita.sh` (13 linhas numeradas no topo), `test_labels_fonte_unica.sh`,
`test_segredo_fora_do_argv.sh`, `test_recursos_sourced.sh`. E a pasta inteira de selos de host
(**66**) verde depois da edicao.


### A0-b, A SEGUNDA DECISAO MEDIDA: **O UNIVERSO DA IMPRESSAO NAO E O GRAVADO INTEIRO, E QUEM ENTRA NO TXT** (10/10 04:2x)

A pergunta que faltava decidir antes de escrever a A0-b era o UNIVERSO: a impressao digital
cobre todo `FechamentoMensal` da competencia, ou so quem entrou no arquivo? Lido o codigo, a
resposta nao e de gosto:

**PROVA:** `folha/export.py::montar_linhas_export` filtra
`status__in=('aberto', 'aprovado')` (`:396-397`) e, dentro do laco, faz
`if fech.colaborador_id not in entra_set: continue` (`:402-403`). **Um colaborador RETIDO nao
move um byte do TXT** -- e a retencao nao e marginal: as tres empresas retiveram 200, 238 e 94
na medicao que eu ja tinha publicado. Impressao sobre o gravado INTEIRO alarmaria sobre linhas
que nao podem mudar o arquivo, e isso e exatamente o ruido que a casa escreveu em 29/09 sobre o
`previsto_em`: *"inclui-lo faria o hash acusar mudanca em toda corrida -- ruido que faz a
proxima pessoa parar de olhar o alarme"*. Duas vezes a mesma licao, por dois caminhos.

**MAS `entra_set` DE QUANDO?** O `entra_set` de HOJE sozinho tem um furo que eu mesmo medi antes:
**a POPULACAO e uma das tres causas de divergencia de hash** (as outras duas sao traducao e
gravado). Quem era retido e virou entrante MUDA o TXT e **nao estaria no baseline**; quem era
entrante e virou retido tambem. Entao o universo da A0-b e **`entra_set` do baseline UNIAO
`entra_set` de hoje**, e nao a interseccao -- a uniao e a leitura mais restritiva, a que nao
pode calar.

E a divisao de trabalho fica limpa, sem juiz novo: **quem diz SE o arquivo divergiu e o
`txt_vigente_x_gravado` que a A0-a acabou de pousar** (ele reconstroi e compara hash, que e a
pergunta de verdade); a impressao digital do gravado responde **QUEM** se moveu, depois que o
hash do TXT ja acusou. Um juiz para cada pergunta, e nenhum dos dois reimplementando o outro.


### O ACERVO DO TXT ESTA LIMPO, E O QUE EU QUASE PUBLIQUEI ERA O CONTRARIO (10/10 04:4x)

A espera do portao da sombra foi gasta no esmeril DESTE marco, perguntando o que a L-110 manda perguntar
ao trocar cinco sitios por um: **quem muda de valor e quanto**. As cinco formas NAO eram iguais --
`views.py:195` e `regerar_txt_dominio.py:88` montavam `'\r\n'.join(linhas) + '\r\n'` **sem a guarda do
caso vazio** que o `validacao_pdf.py:67` tinha, e com `linhas == []` as duas davam `'\r\n'` onde a forma
unica da `''`. A unificacao **escolheu** a guardada, e quem prende a escolha e o selo
`test_MORDE_lista_vazia_da_string_vazia`. **PROVA do impacto:** `ExportacaoDominio` com
`conteudo == '\r\n'` no acervo = **0 de 26**. A troca nao move um byte do que esta gravado.

**E e' aqui que eu errei, e o erro vale mais escrito que escondido.** Recalculei o sha256 do `conteudo`
de cada registro contra o `hash_sha256` guardado e achei **25 batendo e 1 nao**: o `pk=2`,
empresa_id=4, 06/2026, `len(conteudo)=0` com hash `80f417a64296` quando o sha256 de `''` e'
`e3b0c44298fc`. Escrevi, nesta mesma secao, que o registro *"guardou o hash e perdeu o conteudo"* e que
a promessa da lei do TXT -- *"as versoes anteriores ficam guardadas em silencio"* -- tinha falhado numa
linha. **Era hipotese vestida de fato.** Faltava uma pergunta: o campo `conteudo` existia em 30/06?

**Nao existia.** `folha/migrations/0005_exportacaodominio_conteudo` foi aplicada em
**2026-07-02T13:13:20** (lido de `django_migrations` do schema `juliani`); o `pk=2` nasceu em
**2026-06-30T20:19:14**, **dois dias ANTES do campo**, e o campo tem `default=''`. O `''` dele nao e'
copia perdida: e' o default de uma migration, e o hash `80f417a64296` e' a prova legitima do TXT que
existiu naquele dia. Nada se perdeu.

**O censo refeito com o corte que a casa tem, e nao com um corte meu** -- a data de aplicacao da 0005 --
da TRES estados: **comparaveis=25, batem=25, divergentes=0, incomparaveis=1**. E
`conteudo == ''` DEPOIS de o campo existir = **0**. O acervo do TXT esta **limpo**: nenhuma prova
guardada deixa de descrever o conteudo que ela carimba.

**O que fica, e e' o valor do achado:** a A0-b -- o vigia que a clausula 1 da L-113 exige, proxima fatia
desta mesma O219 -- ja ia nascer com tres estados na pergunta (1) *"o TXT vigente ainda bate com o
gravado?"*, porque `comparaveis=0` jamais pode sair como `divergencias=0`. O que esta medicao prova e'
que a pergunta (2) -- *"o conteudo de cada registro bate com a PROVA dele?"*, que entra no mesmo vigia
por ser a mesma bolha e o mesmo juiz -- **precisa da mesma disciplina, com o corte sendo a aplicacao da
0005**: sem isso o `pk=2` alarma para sempre, e alarme que grita sem causa e' o caminho mais curto para
alguem parar de olhar o alarme -- o mesmo ruido que o `carimbo_gravado` ja documenta como razao de
deixar o `previsto_em` FORA do hash. O `pk=2` passa a ser o caso que MORDE o selo do estado
INCOMPARAVEL, em vez de um falso vermelho eterno.

**E nao toco o registro, em nenhuma das duas leituras.** A lei do TXT diz que a invalidacao e' PELA
PORTA e que ela *"nunca toca `conteudo` nem `hash_sha256`"*, e 06/2026 e' competencia EXPORTADA.

---

### A0-b, A TERCEIRA DECISAO MEDIDA: **EU IA CONSTRUIR UMA BASELINE QUE JA EXISTE** (10/10 05:0x)

Eu tinha a A0-b empacotada como UMA fatia: o vigia, a extracao da impressao digital para
`ponto/services/` e a persistencia da baseline da `foto()` no `registrar`. Tres decisoes num
pacote, e a primeira nao depende das outras duas -- o que e' o bastante para o pacote estar errado.

O que desfez o pacote foi ler o que a A0-a acabou de pousar, nao raciocinar sobre ele: o
`conteudo` guardado em `ExportacaoDominio` **JA E uma baseline persistida**, e do universo
exatamente certo -- o `entra_set` do momento do export, porque foi `montar_linhas_export` quem o
montou (`folha/export.py:457`). Um diff de **linhas** entre o guardado e o regerado ja nomeia
matricula + rubrica + valor, isto e, **QUEM** e **O QUE**. Sem nenhuma `foto()`.

O que a `foto()` acrescenta e' mais fino, e e' por isso que ela nao e' o alarme: *qual CAMPO do
`FechamentoMensal` mexeu*, e a discriminacao "o gravado mexeu" contra "a traducao para TXT mexeu".
Isso e' detalhe forense **depois** do alarme. Construir a persistencia dela agora seria pagar uma
migration (ou um arquivo escrito de dentro de um request do `saas_ui`) para responder a segunda
pergunta antes de a primeira existir.

E a persistencia nao esta nem medida: *"no `registrar`"* diz QUANDO, nao ONDE. A `PASTA` de hoje e'
`os.path.join('logs','carimbo_gravado')` (`ponto/management/commands/carimbo_gravado.py:30`) e
`logs/carimbo_gravado/` tem **0 arquivos**; antes de escolher esse caminho e' preciso PROVAR que
`app/logs` e' bind-mount duravel na casca do `saas_ui`, e nao tmpfs. Baseline em tmpfs nao e'
baseline. Fica como terceira decisao, com a medicao antes dela.

Entao o corte: **A0-b = Q1 + Q2 + diff de linhas + `ENC`**, leitura pura, zero mudanca em caminho
de escrita -- a mesma forma de instrumento da A0-a. A extracao de `FORA`/`campos_do_fechamento`/
`foto`/`_mudou` vira marco PROPRIO de dedup, e e' ela que cura o 10-contra-11 do
`lavrar_dias_pagos._fora` medido por AST em 04:0x. Nao se empacotam tres decisoes quando as duas
primeiras nao dependem da terceira.

### A0-b, A QUARTA DECISAO MEDIDA: **O GATILHO DO VIGIA E UM ARQUIVO, E A CAUDA DA CADEIA JA ESTA ESCRITA** (10/10 05:0x)

A pergunta era em que minuto o vigia roda, e ela esta mal feita. Nenhum cron escreve
`FechamentoMensal`: a escrita e' por EVENTO, via `recalcular_por_evento`. Entao nao existe
"escritor agendado" para pendurar o vigia, e qualquer minuto que eu escolhesse seria aposta -- o
que o proprio `ENC` ja diz em docstring: *"Horario e uma aposta sobre duracao; arquivo e um fato"*.

Medido em `config/crons.py`, nao suposto. A cadeia `ENC` viva, encadeada por ARQUIVO, e':

`processar_cartorio` -> `apurar_furos_diarios` (:1235) -> `disparar_perguntas_competencia` (:1247)
-> `reconciliar_perguntas_orfas` (:1250) -> `lavrar_deslogados` (:1263) -> `lavrar_he_pendente`
(:1278) -> **`recusar_ponta_pequena`** (:1291)

Nenhum `ENC` tem `gatilho='recusar_ponta_pequena'`: ela e' a **CAUDA**, e e' justamente a cadeia que
poderia mexer em HE e em ponta, isto e, no que vira dinheiro. Logo o vigia espera o carimbo
`logs/fim/recusar_ponta_pequena.<emp>.<dia>` que o `bin/cron_run.sh` deixa (`crons.py:47`), com a
razao escrita: **nada agendado escreve o gravado; o vigia le depois de tudo que PODERIA ter
escrito.** A entrada fica DECLARADA em `ENCADEADOS`, para contrato nao ficar verde por ausencia de
sinal.

O candidato que eu ia usar por reflexo era o ultimo por *schedule*: medi 54 entradas `C()` com
minuto fixo entre 04h e 09h, e a ultima e' `pautas_do_esmeril` as **08:40** (`crons.py:489`).
Schedule e' aposta; a cauda da cadeia e' fato. No MESMO commit muda a celula de `carimbo_gravado`
em `FORA_DE_PIPELINE` (`crons.py:601`/`:625`/`:632`), que e' onde a bifurcacao esta escrita --
*"ou ele vira cron da competencia exportada ou sai daqui com o command"* -- e a secao 7 do
CLAUDE.md.

### A0-b, A QUINTA DECISAO MEDIDA: **DUAS PERGUNTAS, DOIS UNIVERSOS, E DOIS TERCEIROS ESTADOS QUE NAO SAO O MESMO** (10/10 05:0x)

Eu ia escrever "tres estados nas duas perguntas" e deixar o leitor supor que o terceiro estado e' o
mesmo. Ele nao e', e unificar os dois e' precisamente o que faz o alarme do pk=2 voltar.

A **Q1** -- *"o TXT que saiu da casa ainda e' o que o gravado de hoje montaria?"* -- ja tem os tres
estados, e eles sao da A0-a (`folha/export.py:413-455`): `tem_export=False` e' o INCOMPARAVEL, com
o motivo escrito *"nao ha TXT vigente nesta competencia: nada saiu da casa"*; `divergente=False`
bate; `divergente=True` diverge. E o `hash_sha256` e' comparavel para **toda** linha, inclusive
pre-0005, porque ele sempre foi computado do conteudo no ato do `registrar`.

A **Q2** -- *"o conteudo guardado bate com a prova guardada?"* -- NAO herda esse corte. O
INCOMPARAVEL dela e' outro: *nasceu antes de o campo `conteudo` existir*. Medido em prod agora:

- `folha/0005_exportacaodominio_conteudo` aplicada em **2026-07-02T10:13:20-03:00**
- registros: **26** · VIGENTES: **7** -- nascidos antes da 0005: **0** · conteudo vazio: **0**
- NAO vigentes (versao anterior guardada): **19** -- nascidos antes da 0005: **1** (o pk=2)

A minha suposicao estava certa, e e' exatamente por isso que ela e' uma armadilha: **se a Q2 olhar
so o vigente, ela da 7 comparaveis e 0 incomparaveis -- e o terceiro estado nasce VAZIO**, isto e,
nasce por ausencia de sinal, que e' o selo vacuo que a casa proibe por regra. A lei do
TXT-fotografia promete que as versoes anteriores ficam guardadas em silencio com conteudo, hash,
data e quem gerou; quem guarda tem de provar que o guardado continua inteiro. Entao a Q1 pergunta
sobre o **VIGENTE** (7) e a Q2 pergunta sobre o **ACERVO** (26) -- e e' no acervo que o terceiro
estado tem **1** ocupante que MORDE. Universo unico aqui seria conveniencia: ou a Q2 ficaria cega
para 19 versoes guardadas, ou o terceiro estado dela seria decorativo.

O corte da Q2 continua sendo a data de APLICACAO da migration lida do `django_migrations`, e nao o
atalho `conteudo == ''`: o atalho leria uma escrita genuina pos-0005 como INCOMPARAVEL em vez de
DIVERGENTE -- trocaria um defeito por um silencio.

E os dois eixos do censo ficam separados na escrita, porque apontam para lados opostos por razoes
opostas: **populacao = UNIAO** do `entra_set` da baseline com o de hoje, porque quem SAIU do TXT e'
precisamente a divergencia que se quer ver; **campos = INTERSECCAO**, que e' a regra que o
`_mudou()` ja documenta, porque campo que entra ou sai do modelo e' VOCABULARIO, nao dado -- tirar
`previsto_em` do universo fez uma foto nova relatar 454 mudancas espurias, todas
`previsto_em: <data> -> None`. Uma frase nao pode colapsar os dois.

Erro de sonda no caminho, registrado para nao voltar: o campo e' `gerado_em`, nao `criado_em` --
`AttributeError` no primeiro tiro. A excecao pegou o erro antes de a conclusao sair, que e' o
contrario da sonda que conta erro e segue imprimindo zero.

### O `!` QUE A CLAUSULA 1 PEDE (nao devolve turno -- a fila segue em A0-b)
Tirar o `raise CompetenciaExportada` de `ponto/services/fechamento.py:72-77` e **`!` de dinheiro
exportado** pelo item (c) da DINHEIRO-EM-COMPETENCIA-ABERTA, e esta na lista NUNCA PRE-APROVADO.
Vai para a mesa com o numero, e o turno **nao para**: A0-b (o vigia) e a proxima do mesmo item e nao
depende do `!`.

## O218 CORRETIVO — **EU CRIEI O SEGUNDO JUIZ E ESCREVI NO COMENTARIO QUE ELE NAO EXISTIA** (10/10 01:3x, ESMERIL-DO-MARCO + LEI-AKITA 2)
O esmeril do proprio marco achou, no unico arquivo que o marco mudou, um defeito que a mensagem do
`672f88e1` NEGA. Ao extrair a receita da guarda de banco para `E_SOMBRA` (bin/sombra.sh:63) eu deixei de pe
a copia LITERAL que `bloco()` usa, e comentei que a variavel era *"o unico sitio onde ela mora"*. A
pergunta "estou na sombra?" passou a ter **dois juizes no mesmo arquivo**, e o que ficou de fora e o sitio
que o `bin/deploy.sh:167` exige antes de publicar.
**PROVA:** `git show 672f88e1 -- bin/sombra.sh | grep '^+.*current_database'` = **1 linha** (a :63);
`git blame -L311,311 -- bin/sombra.sh` = **`3d8d835f`** (a fatia E2, o nascimento da sombra). Uma copia
antes daquele commit, **duas** depois. O comentario falso esta em `bin/sombra.sh:421-423`.

**O SELO NASCEU VERMELHO NO COMMIT ANTERIOR, e morde nos TRES sentidos.** "Copias == 1" ficaria **VERDE**
com a guarda apagada do portao do deploy -- a familia do `[]` de dois sentidos --, entao o selo afirma
tambem que a definicao existe UMA vez e que **dois** sitios a LEEM.
**PROVA:** `logs/o218/SELO_RED_guarda.txt` sobre o HEAD anterior: `(a) ... aparece 2 vez(es), esperado 1` e
`(c) '$E_SOMBRA' lida em 1 sitio(s), esperado >= 2`. Medido em quatro arvores: HEAD = RED por (a)+(c);
curada = `OK -- 1 assercao, 1 definicao, 2 leitores da MESMA guarda`; curada sem o `$E_SOMBRA` do `bloco()`
= RED por (c); curada sem a definicao = RED por (a)+(b). Os comentarios saem da conta antes de contar
(`grep -vE '^\s*#'`), pela 5a lapide da casa.

**O COMANDO QUE `container()` RECEBE NAO MUDA UM BYTE, e isso nao ficou no papel.** `container()` termina em
`sh -c "$1"` (bin/sombra.sh:159) -- UM argumento --, e o bloco multilinha de aspas simples ja provava que
ele aceita newline.
**PROVA:** `sh -n` OK nas duas formas; o diff do argumento composto, extraido VERBATIM dos arquivos, e
**uma linha vazia e 4 espacos** de indentacao -- o texto da guarda e o resto do bloco sao byte-identicos; e
`container "$E_SOMBRA"'<NL>echo ALCANCEI'` rodou na funcao **REAL** e imprimiu **ALCANCEI**, com a arvore
entrando pela porta nova (`SOMBRA_ARVORE`). A guarda MORDE com dois valores dando dois status: constante
esperada trocada -> `NAO_E_SOMBRA` **rc=90**; verdadeira -> `PASSOU` **rc=0**.

**A MINHA PRIMEIRA CONTRAPROVA ESTAVA MAL PARAMETRIZADA, e fica registrada.** Tentei falsificar a guarda com
`-e POSTGRES_DB=juliani` e ela nao barrou -- que por um instante se le como guarda furada. Nao e: o env e
ignorado **por desenho**, e o desenho e uma protecao.
**PROVA:** `app/config/settings/sombra.py:30` faz `DATABASES['default']['NAME'] = NOME_DO_BANCO`, e `:16`
avisa que `POSTGRES_DB` esquecido no `.env` nao desvia este settings. Quem errou foi a sonda -- setima vez
nesta casa. A sonda certa troca so a constante ESPERADA, e e a do paragrafo acima.

**E O NUMERO 2 ESTAVA NA MINHA MAO HORAS ANTES.** O censo que eu mesmo escrevi nesta noite diz, da porta:
``tem (`E_SOMBRA`, 2 sitios)``. Eu li aquele 2 como "definida + lida" e nunca perguntei se havia uma
terceira copia literal: contei o TOKEN, nao o JUIZO.
**PROVA:** `logs/o218/CENSO_PORTA_SOMBRA.md:11`. O achado inteiro, com as quatro provas e o que ele **nao**
fecha, esta em `logs/o218/ACHADO_GUARDA_DOIS_SITIOS.md` (101 linhas).

**O QUE NAO SE FEZ, e por que:** nao se emendou `672f88e1` -- ele estava a caminho do remoto, e a cura vem
em commit proprio; nao se re-rodou `--refazer`/`--bloco` (~38 min dentro da faixa cega 00:00-04:00, e o
carimbo de hoje e o portao vivo do deploy); e **nao se declara fechada** a linha de fila dos 3 forks da
receita (`diff_janela_he_total.sh`, `simular_folha.sh`, `r5_idempotencia_frota.sh`): esta cura e de UM
arquivo. Os comentarios :62, :145 e :421-423 **nao foram reescritos** -- a cura os torna verdadeiros.
**PROVA:** `bin/sombra.sh --conferir` segue **rc=0** com `dia=20261010 status=OK tipo=completa diverge=0
erros=0`; `bash -n bin/sombra.sh` OK; pasta de selos de host verde.

## O207 POUSA — **UM ATO SO, E O PORTAO DA SOMBRA FOI LIDO DUAS VEZES COM A MESMA RESPOSTA** (10/10 00:4x, L-105 + L-107)
A terceira opcao do *Inicio da apuracao* -- a data que o admin DECLARA -- esta no ar. O pouso foi na forma
da **L-107**: `git merge --no-commit` -> resolver -> commit -> `bin/deploy.sh --sem-migrate`, **sem nada no
meio**, porque a arvore E o bind-mount e um merge escreve dezenas de `.py` nela enquanto o worker segue com
o codigo de antes.
**PROVA:** commit `353818c5`, **12 arquivos, 532 insercoes, 38 delecoes**; `pouso.sh` rc=**0** com o log
inteiro em `logs/o207/pouso.out`; `deploy: OK -- migrations em dia, tres cascas reiniciadas juntas, tres
rotas provadas` (core `/health/` 200, ui `/colaboradores/` 302, mensageria `/health/` 200), `prova de casca:
16 estaticos, 5 paginas compiladas, 608 rotas em 2 urlconf(s)`, selo BUG 128 verde, `importerror_500=0`.

**A SUITE JULGOU A ARVORE QUE O MERGE PRODUZ, e isso se mediu em vez de se supor.** `git diff HEAD raia-bos`
e **SIMETRICO**: ele listava 14 arquivos, e 6 deles eram aqueles em que o *main* andou (PLACAR-ESTRUTURAL,
O206) e a raia nunca tocou. O que o merge escreve e o que a RAIA mudou desde a base.
**PROVA:** `git merge-base HEAD raia-bos` = `b5700cbb`; `git diff --name-only b5700cbb raia-bos` = **8
arquivos**; o `status --porcelain` da copia `wt-o207` -- a arvore onde correu a suite, com HEAD em
`f415e116` -- tem **exatamente esses 8**, e o `diff` das duas listas imprimiu `IDENTICO`. Veredito
`OK (skipped=42)` sobre `Ran 10253 tests in 1412.830s`, **0 FAIL/ERROR no log**.

**A FALHA QUE A RAIA HERDAVA MORRE NO MERGE, e isso se prova por ANCESTRALIDADE, nao por um run que eu
nao tenho.** A raia nasceu de `b5700cbb`, que **antecede** a cura `1c5b2e20` (*"a lapide do dono da
cobranca para de nomear o delimitador que o selo varre"*), entao ela carrega a versao velha de
`templates/chamados/partials/_js_cobranca.html` e o selo da lapide a morde quando ela corre sozinha.
**PROVA:** `git merge-base --is-ancestor 1c5b2e20 b5700cbb` = **NAO** (a raia e anterior a cura);
`git merge-base --is-ancestor 1c5b2e20 HEAD` = **SIM** (o merge a traz); `git diff raia-bos HEAD --
app/templates/chamados/partials/_js_cobranca.html` = **DIFERENTE**, e a arvore viva depois do pouso
esta igual a HEAD. Nao se cita aqui o veredito vermelho da raia isolada: eu nao o medi nesta sessao, e
afirmacao sem arquivo que a persiga nao conta.

**O MESMO PORTAO, NAO UMA SEGUNDA VERSAO DELE.** O gate de sombra do `pouso.sh` e o **rc de
`bin/sombra.sh --conferir`**, a condicao byte-identica do `deploy.sh:167`. Escrever ali um `grep` de
`dia=... status=OK` seria a segunda verdade: ela passaria e o deploy recusaria **depois** do merge, com a
arvore viva ja escrita. E esse rc responde TRES perguntas de uma vez, porque `conferir()` exige
`SOMBRA_STATUS=OK` e esse valor so e escrito em `bloco():360`, no fim, com `diverge=0` e `erros=0`.
**PROVA:** a linha `sombra: carimbo dia=20261010 status=OK tipo=completa diverge=0 erros=0` aparece **duas
vezes** em `logs/o207/pouso.out` -- a primeira e o meu gate, a segunda e a do `deploy.sh`.

**O ATO IRREVERSIVEL FOI ENSAIADO INTEIRO ANTES, e o ensaio achou um VERMELHO.** Numa terceira copia
descartavel (`wt-ensaio`, detached em `f415e116`) o merge e a cadeia inteira correram: depois do resolve
reescrever `BACKLOG.md`/`TICKETS.md` -- os dois **staged pelo merge** --, `bin/index_vs_arvore.sh` RECUSA o
commit, porque arquivo staged que tambem difere da arvore e a condicao que ele morde.
**PROVA:** `index_vs_arvore` rc=**1** antes do `git add` dos dois, rc=**0** depois; cadeia completa rc=0 com
o commit de ensaio `89a41c82`. Sem o ensaio, esse vermelho apareceria com a arvore VIVA ja escrita.

**DOIS GATES MEUS ESTAVAM ERRADOS, e os dois eram do mesmo tipo: ausencia de sinal lida como sinal bom.**
(1) O veredito da suite: eu escrevi `^OK(\(| |$)`, e o espaco SOLTO casa **prosa de log** -- `OK -- nenhuma
divergencia em 2026-10-10` passa por veredito. Pior: essa forma **nao casa `FAILED` de forma alguma**, entao
ela nao distingue "falhou" de "nao achei veredito". O discriminador e **espaco-parentese ou fim de linha**
(`^(OK|FAILED)( \(|$)`) mais `tail -1`, mais um `Ran N` com N >= 9000, porque recorte nao e suite cheia. (2) `CONF="$(cmd)"; RC=$?` **morre sob `set -e` antes de `$?` ser lido**: o PAROU nunca sairia.
A forma e `RC=0; CONF="$(cmd)" || RC=$?`.
**PROVA:** `logs/o207/gates_RED.txt` -- tabela de 5 strings com as duas formas lado a lado (a velha CASA
`OK -- nenhuma divergencia`, recusa `FAILED`; a nova separa os tres) e os dois `bash -c` do gate 2, onde a
forma velha mata o shell com rc=1 **sem imprimir nada** e a nova imprime `cheguei aqui, rc=1`.
**E O PROPRIO CENSO SE CORRIGIU AO SER MEDIDO**, no mesmo arquivo: eu havia escrito que aquela prosa estava
"literalmente dentro deste run" -- ela esta **0** vez no `suite_merge.out` (vive no log da SOMBRA) -- e que
a forma velha casava `OK:   31`, que ela **recusa**, porque ali ha dois-pontos e nao espaco. O erro era do
mesmo tipo do gate: afirmar de cabeca o que o arquivo responderia.

**E quase li numero de ONTEM como de hoje:** o carimbo mostrava `SOMBRA_COMANDOS=70 SOMBRA_ERROS=0` ao lado
de `SOMBRA_BLOCO=nao_rodou`, porque o `--refazer` zera o `STATUS` e deixa os campos do bloco anterior no
lugar. Um gate em `SOMBRA_ERROS` teria lido 09/10. Registrado no comentario do script.

**O ESMERIL DO MARCO, pela ESMERIL-DO-MARCO: 0 orfao, 0 segundo juiz, 1 linha de fila.** Censo pedido a
arvore mergeada, nao a memoria.
**PROVA:** `logs/o207/ESMERIL.md`; `git grep 'modo_inicio\|MODOS_INICIO' raia-bos -- app` da
`templates/colaboradores/partials/_fase_12x36.html` (linhas 22, 26, 31) como **unico** leitor das chaves
cruas -- **0** `.py` de producao, **0** JS, **0** outro template; o unico teste removido e um **RENAME** com
assercao invertida e lapide no sitio. **Fila (nao trabalho agora):** o template **enumera** os tres modos no
markup em vez de iterar `MODOS_INICIO` (`colaboradores/views_fase.py:23`), entao um quarto modo envelheceria
em silencio e o selo `html.count('name="modo_inicio"') == 3` ficaria VERDE contando o markup do proprio
template. Iterar pede rotulo e `hx-get` por modo: e DESENHO, nao cura.

**O que o commit NAO levou, e por que:** `app/config/crons_duracao.json` (cron-sujo, nunca se commita),
`bin/sombra.sh` (instrumento da O218 -- a **L-105** manda instrumento pousar SOZINHO, depois do produto) e
`app/docs/HANDOFF-SESSAO.md` (derivado). **PROVA:** `git status --porcelain` depois do ato = exatamente
esses tres, e nenhum deles no `git show --stat HEAD`.

**Na mesa:** `O207-INICIO-DECLARADO-SMOKE` (AVAIS 10), o unico item do aval que nao e codigo -- o print da
tela com a terceira opcao num colab de TESTE. **ORDEM VIVA**, pela ordem dele de 18:3x: **O219** -> O223 ->
O224 -> contratos -> O228, com a **O235** (BO-WIZARD-DIZ-O-QUE-GRAVA) na vaga logo apos a O207 na raia
`wt-bos`. O `siga: PLACAR-ESTRUTURAL` do hook **nao se cura aqui**: ele mesmo disse que isso e pouso de
instrumento, depois.

## W12X36-HPD FECHADO PELO SMOKE DELE, E O SMOKE ACHOU UM BO — **MEDIDO ANTES DE UMA LINHA DE CURA** (10/10 00:0x)
O smoke dele fechou as duas metades que o numero previa: *"marquei o domingo com horario proprio e salvou;
um 12x36 sem marcar nada continuou igual"* -- e o "continuou igual" era exatamente o `0 de 896 dia-tipo
mudam` da medicao de 02/10. Item **respondido** pelo escritor canonico (AVAIS 10 -> 9).
**PROVA:** `app/docs/PENDENTES_RONALD.json` com `W12X36-HPD-SMOKE.estado='respondido'` e
`respondido_em='2026-10-10T00:0x'`; `git diff --stat` do JSON = **1 arquivo, 3 insercoes, 2 delecoes**
(so este item, nada de churn); `bin/gerar_avais.py --escrever` imprimiu `AVAIS NA MESA: 9`.

No MESMO smoke ele achou um BO de TELA e mandou medir: *"Publica quantos TipoEscala foram criados com o
campo vazio desde 24/08, so leitura"*. **O numero pedido nao se conta pela linha, e isso e medicao, nao
desculpa**: `intervalo_duracao_min` **nao e nullable** (`escala/models.py:110`, `default=60`, sem
`null=True`), entao "vazio no wizard" e "60 digitado" caem no MESMO valor; e `TipoEscala` **nao tem campo
de criacao** nem trilha (`grep` de auditoria em `cadastro_tipo.py` = 0), entao "desde 24/08" nao tem
autoridade na linha. O que esta medido, em prod e so leitura: **355 tipos**, **333 em 60** -- que e **TETO**
da classe "nasceu sem duracao declarada", nunca a classe --, **22** com duracao digitada de verdade, e
**0 em valor 0**: o caso que o rotulo da tela promete nunca foi gravado uma vez em 355 tipos, embora seja
gravavel (`request.POST.get` devolve `'0'`, que passa pelo `isdigit`). Recorte por data **como PROXY
declarado**: dos 333 em 60, 49 tem vinculo com `data_inicio >= 24/08`, 205 tem vinculo mais antigo e 79
nao tem vinculo nenhum.
**PROVA:** `logs/w12x36/sonda_dur.out` (sonda com as duas assercoes de banco, `settings` e `connection`
== `saas_hasner`, nada escrito), sonda em `logs/w12x36/sonda_dur.py` e a leitura em
`logs/w12x36/MEDIDO.md`.

**O BO e PIOR do que o aval descreve, e o sentido se INVERTE.** O aval diz "vazio nao grava"; medido, vazio
tem DOIS efeitos e nenhum deles e "sem pausa": na **CRIACAO** o atributo nunca e atribuido
(`cadastro_tipo.py:243-245`, `_dur.isdigit()` False) e o tipo nasce com o `default=60` do modelo -- o
sistema desconta **uma hora**, o OPOSTO do que o rotulo promete --; na **EDICAO**, vazio **preserva o valor
anterior**: limpar o campo nao limpa nada. E `cadastro_tipo.py:239` promete por escrito *"Vazio = None"*,
valor que um `PositiveSmallIntegerField` sem `null=True` nao pode guardar. Virou **O235**, na raia wt-bos
depois da O207, como ele mandou; cura de TELA e de ROTULO, **0 juiz novo, 0 escritor novo**.

## PLACAR-ESTRUTURAL — AS TRES **NO REMOTO**, E A ORDEM DELE REPOE O ITEM NO 5o LUGAR (10/10 00:0x)
O marco fechou onde estava: as tres (`ec4b1994` R3, `1c281f92` BATERIA, `f415e116` instrumento) estao no
tronco, e o veredito e **do remoto**, nao de rc de background nem de `tail` de log -- a licao de 08/10,
quando `OK -- nenhuma divergencia` casou um `grep` de veredito e me deu por verde uma suite que terminava
em `FAILED`.
**PROVA:** `git fetch && git rev-list --count origin/main..HEAD` = **0**; HEAD == origin/main ==
`f415e116`; `git merge-base --is-ancestor` devolve ancestral para as **tres**.

O aval das 00:0x **corrige a ordem que eu vinha seguindo**: eu estava no `siga` do hook, que manda
PLACAR-ESTRUTURAL, e ele diz que o item e o **5o** -- a principal vai para a **O219** (ordem de 18:3x:
O232 -> O219 -> O223 -> O224 -> contratos -> O228). A divergencia do hook **nao se cura agora**, por ordem
dele (*"se cura em pouso de instrumento, depois"*): entao ela fica como esta, e eu **nao** invento palavra
de estado no BACKLOG para o hook concordar com a fila. Antes da O219 entra o **pouso da O207**, que e o
lugar que a L-105 reserva para raia verde.

**O marco nao devia nada ao ar, e isso foi medido, nao assumido.** `core/placar_estrutural.py` nao tem UM
importador na aplicacao: quem o le e o HOST, por `spec_from_file_location` (`bin/gerar_estado.py:54-69`),
entao nenhuma casca o importa e um reload nao mudaria nada para ele. Foi por isso que eu **nao** rodei
`bin/deploy.sh` as 23:5x com o portao da sombra a minutos de ficar cego -- e `--sem-sombra` com motivo
inventado para fechar o marco antes da meia-noite seria atalho, e atalho e `!`.
**PROVA:** `logs/placar/DEPLOY_NAO_E_DEVIDO.md` (0 importador Python na app, 0 template, so os leitores de
host); e o veredito do placar sai da funcao REAL: `veredito()` devolveu `VERDE (termometro em dia)` com
`medido_em={'R1': '2026-10-09'}` lido do NOME do arquivo da fonte declarada, nao de prosa.

## PLACAR-ESTRUTURAL BATERIA — **A CONTAGEM ENVELHECEU EM HORAS, PELA MAO DE QUEM A ESCREVEU** (09/10 23:2x, mesmo item em curso)
Com o R3 medido e o selo dele construido, fui remedir os outros DOIS registros ainda PARCIAIS -- porque
a licao do R3 nao e "aquela frase estava errada", e **ninguem perguntava a fonte**. O R6 passou: pelas
funcoes REAIS (`core/contratos_estruturais.py`, importado por caminho com `python3` puro -- sem Django,
sem banco, sem container, o que importa porque a pista de teste estava TOMADA pelo push), o placar da
`contratos_estruturais: 15/20 verdes`, 17 declaradas, teto 20 por `familias_sem_cadastro() ->
('escala', 'chamado')` da L-100, e **5** celulas faltando. A frase copiada no R6 esta CORRETA e ATUAL:
nada a corrigir ali. As 5 que faltam, com o que falta em cada uma, para quando a vez delas chegar:
`batida x um juiz por pergunta` (teste NENHUM, mas censo e as duas chaves de `core/juizes.py` ja
existem, remedido 08/10) · `escala x um juiz por pergunta` (teste NENHUM, ninguem comecou, sem
`JUIZES["escala"]`) · `chamado x um juiz por pergunta` (2 excecoes cheias, 58 sitios) ·
`chamado x um escritor por entidade` (a maior: 131 escritas na familia, 12 na porta, **119 FORA**, 49
arquivos) · `folha/export x um juiz por pergunta` (1 excecao, 13 sitios, **8 de dinheiro**).

**E A MINHA PROPRIA SONDA DO R6 MENTIU PRIMEIRO, E VALE DIZER COMO.** Ela lia cada celula da matriz com
`getattr(cel, 'verde', False)` e me imprimiu `verdes=0 vermelhas=19` contra os `15` do juiz. Nao
publiquei a divergencia: fui ao fonte. `C(...)` devolve um **dict**, nao um objeto, e `verdes()` le
`c['verde']` -- entao o meu `getattr` entregava o default `False` para **todas** as 19 celulas. E a
classe que a casa ja nomeou: **ausencia de sinal lida como sinal bom**, com o agravante de que aqui a
ausencia de sinal era a minha, e o juiz estava certo o tempo todo.

**O BATERIA, porem, nao passou -- e o numero vencido e de HOJE.** O registro declarava *"medida 09/10 em
DOIS modulos: **15 cenarios / 48 testes** verdes"*. Por AST sobre os dois modulos que o campo `fonte` do
proprio registro nomeia, o real e **16 cenarios / 64 testes**. O metodo nao se autoriza por ser
razoavel: ele se autoriza **reproduzindo o numero antigo**. Os 12 cenarios que a linha velha nomina
somam **34 testes exatos** (2+1+2+4+3+4+3+3+3+4+3+2) e o segundo modulo da **3 / 14 exatos** -- os dois
numeros que a linha velha afirmava. Reproduzidos os dois, o universo e o mesmo, e o **delta inteiro e UM
cenario**: `SaidaPosicionalDepoisDaPausaTest`, 16 testes, nascido nos pousos de 09/10 `185b9af0` (R1
balde P) e `5d9ec893` (L-115 pouso 1), **no ar as 22:1x**. A linha ficou atras do codigo que eu mesmo
pus nela, com menos de uma hora de diferenca.

A minha primeira contagem tambem errou, para o outro lado: deu **20 / 64**, porque contou **classe pela
forma**. `_BaseLavratura`, `_BaseMarcos`, `_B` e `_Bd` sao bases com **zero** `def test_` -- nao sao
cenario. E a memoria `criterio-pela-forma-conta-errado` em ato, e foi por nao publicar aquele 20 que o
16 apareceu.

**O QUE NAO SE MOVE, e e a parte que importa**: a BATERIA segue **PARCIAL**, e nao pela contagem. Ela e
parcial porque **UMA familia das sete** tem bateria declarada; trocar 48 por 64 nao fecha nada. Numero e
estado sao coisas diferentes neste placar -- o patch afirma isso em assercao antes de gravar, igual ao do
R3 (*placar nao carimba a propria meta*).

**E ISTO EXPOE O LIMITE DO SELO QUE EU ACABEI DE CONSTRUIR**, o que e melhor descobrir sozinho do que
depois: ele cobra afirmacao de **PUBLICACAO** -- hash que o tronco ja tem e a linha diz fora do ar -- e
teria deixado esta passar **inteira**. Contagem vencida e outra classe, e ela tambem e derivavel, porque
a `fonte` nomeia os modulos e declara a unidade em letra (*cenario = classe de caso, teste = `def
test_`*). Virou a **O234** no bloco OBRAS, registrada **no mesmo commit que e o RED dela** -- pela
ESMERIL-DO-MARCO, achado fora do marco e linha na fila, nunca trabalho agora.

**UMA ARMADILHA DE FORMA, medida e nao suposta.** Os dois patches constroem a copia de
`git show HEAD:` (LEI-AKITA 10), entao rodar o do BATERIA **antes** de commitar o do R3 **descartaria o
R3 em silencio** -- sem erro, sem conflito, a frase velha voltando calada. O patch passou a exigir, em
assercao, que a frase nova do R3 esteja no HEAD de onde ele construiu, e a rodar o selo contra o
candidato. A exigencia foi **vista VERMELHA** antes do pouso do R3
(`AssertionError: o commit A nao esta no HEAD de onde este patch construiu`): a ordem dos dois commits
deixou de depender de eu lembrar dela.

## PLACAR-ESTRUTURAL R3 — **A LINHA DIZIA QUE A O130 ESTAVA FORA DO AR, E O GIT DIZ QUE ELA POUSOU EM 04/10** (09/10 23:2x, item em curso)
Medido pelo publicador REAL, e nao pela minha propria chamada: `pe.placar()` me devolveu
**AMARELO** (*"termometro sem data de medicao na fonte declarada (ou `hoje` nao foi passado): R1"*) e eu
quase li isso como o estado da casa -- o parentese da propria mensagem e que me pegou. Carregando
`bin/gerar_estado.py` e chamando `_placar_estrutural(raiz)`, que e quem PUBLICA, o placar diz
**`3 fechado(s), 4 parcial(is), 0 pendente(s) de 7 · VERDE (termometro em dia)`**: fechados SOMA, R2 e
R5; parciais BATERIA, R3, R4 e R6; o R1 e o TERMOMETRO, com `fonte_do_medido` em
`logs/e6_cauda2c/r1_dono_*.txt` e data `2026-10-09`. LEI-AKITA 2 e 8 na pratica: chamar a funcao que o
sistema usa, nunca reproduzir a chamada. Pela ordem da **L-099** (*R1 e R2 primeiro, depois R5, R3, R4,
R6*), com R1/R2/R5 fechados, o proximo e o **R3**.

**E O R3 DAVA DUAS RAZOES PARA FICAR PARCIAL; UMA DELAS ERA FALSA.** A linha dizia, com nome e hash:
*"a parte que falta tem nome: a O130 ... A O130 ESTA CONSTRUIDA E MEDIDA (raia `raia-o130`,
`97079d6e`), e NAO ESTA NO AR: merge e deploy sao UM ato e o deploy publica o disco, que e o `!` da
`janela_auth`"*. **A O130 ESTA NO AR desde 04/10.**
PROVA: `git merge-base --is-ancestor 97079d6e main` e `... 4c8a50aa main` respondem **SIM** nos dois;
a entrada no main foi o merge **`381af46b`** (`Merge branch 'raia-o130' into raia-merge`); o compositor
unico `ponto/services/dia_decidido.py::frase_do_que_falta` esta no disco desde **04/10 20:43** e houve
deploy depois (`c7b8bfdb` 09/10 20:17 e `b5700cbb` 22:1x, `logs/l115/o206_deploy.out`). E o `!` que a
linha nomeava **nao prendia nada**: dos **10** sitios declarados em `bin/auth_sitios.txt`, o merge da
O130 toca **0** -- medido pela forma do proprio portao, `git diff --name-only main...raia-o130`.

**A outra razao segue de pe, e e dele**: *"a clausula `com o que falta` e redacao dele, e placar nao
carimba a propria meta"*. Entao o **estado do R3 nao se move** -- continua PARCIAL, e eu nao o carimbo.
O que se move e uma frase falsa: a linha passa a dizer que a O130 pousou, por qual merge, e que **esta
linha afirmou o contrario por cinco dias**. A clausula vai para a mesa como o item
`R3-CLAUSULA-COM-O-QUE-FALTA` (`!`, nao trava a fila 1), com os numeros ao lado: MUDOS=0 nos dois
universos, 2.405 dias de 16.161 com a palavra, 2a palavra em 48 dia-colab e 312,9 h, grade 25 de 48
porque os 23 restantes sao a discordancia de janela do **O65**. Pela PAREI-DE-LEI-NAO-DEVOLVE-TURNO a
esteira nao espera a resposta: segue no **R4**.

**O DEFEITO DE ORIGEM NAO ERA A FRASE, ERA NINGUEM PERGUNTAR AO GIT.** A prosa do placar envelheceu
cinco dias em silencio, e isso nao e novidade na propria tabela: o R4 diz de si mesmo *"este campo
dizia '0 na 10', e era numero que envelheceu em silencio, o mesmo defeito que o R4 existe para
nomear"*. O que faltava era uma guarda, e ela nasce agora:
`bin/tests/test_placar_nao_afirma_fora_do_ar.sh` cruza **todo** hash citado no placar com
`merge-base --is-ancestor` e fica VERMELHO quando uma frase declarada de "fora do ar" aparece na MESMA
SENTENCA que um commit que o tronco ja tem. Ele le a autoridade de dois lados e nao replica nenhum: o
`RESULTADOS` do proprio modulo (que e PURO, importado por caminho, sem Django) e o git.
RED: contra a arvore de hoje o selo acusou **1** -- *"R3 (campo numero) diz fora do ar sobre 97079d6e,
que ESTA no tronco"* --, e contra o candidato curado, **0**. E ele MORDE, com os dois casos no corpo
antes de julgar o vivo: um registro de mentira que afirma fora do ar o proprio HEAD do tronco **tem** de
ser acusado, e uma raia de verdade nao pousada (`0000000`) **nao** pode ser. Raia que nao pousou tem
hash fora do tronco, e o selo fica verde nela de proposito.

**E ELE E A SEXTA VEZ DA CLASSE QUE ME MORDEU HA UMA HORA**, entao a diferenca esta escrita no cabecalho
dele: este selo le **UM** arquivo de produto, por **sentenca**, com as frases **declaradas** em lista, e
**nunca a si mesmo** -- era exatamente o que faltava ao selo da lapide, que varre a pasta inteira e por
isso mordeu a prosa que explicava a cura. A disciplina que isso impoe ao placar: historia de frase velha
nao se cita com hash ao lado; ela mora aqui, no RELATO.

## O206 POUSA — **O AVISO PARA DE MANDAR NA ACAO QUE APAGA RESPOSTA** (09/10 22:0x, aval dele de 21:4x)

PROVA: merge **em COPIA** antes da arvore viva (L-107 segunda forma, LEI-AKITA 10): worktree
`wt-merge-o206` em `a5ae5013`, `git merge --no-commit raia-bos` -> *"Automatic merge went well"*,
**0 arquivo em conflito**. Suite da copia MERGEADA pela porta unica
(`bin/suite.sh --dir ... --only "chamados core"`, cpuset 4-7): **`Ran 3669 tests` / `OK (skipped=23)`**,
rc 0, **0** FAILED/ERROR (`logs/l115/o206_suite_copia.out`). Ruff limpo nos dois `.py`.

**O SELO NOVO CORREU DENTRO DISSO, e nao e suposicao:** `chamados/tests/` tem **2334** `def test_` na
copia contra **2322** no main = **+12**, exatamente os 12 casos `test_MORDE_*` do
`test_selo_painel_oferece_cobrar.py`; a classe e `SimpleTestCase` (coletada) e o arquivo tem **zero**
`skip`. Os 23 skipped sao os de sempre.

**O BO, em uma linha:** com pergunta sem resposta o painel do fio mandava *"Aguarde a resposta ou use
Reabrir questionario"* -- e o Reabrir **APAGA** as respostas ja dadas (col267, chamado 8954: 5 respostas
certas apagadas em 28/09). A tela empurrava o admin para a acao destrutiva. Agora o aviso indica
**"Cobrar resposta (nao apaga nada)"**, que so manda push pela rota que **ja existia**
(`chamados:cobrar_chamado`, `views_cobrar.py:33`) -- **0 rota nova** --, e o Reabrir fica ABAIXO com a
condicao escrita, dizendo **quantas** respostas apaga (o `n_apaga` da UI-RESPOSTA-DIZ-O-QUE-E, que
sobreviveu ao merge intacto em `modal_fio.html:332-335`).

**UMA DEFINICAO SO, pela lei das DUAS CASCAS (corte 08/09).** `cobrarChamado` morava em
`_lista_chamados.html:171`, entao a tela que abre o painel SEM a lista nao tinha como cobrar. Ela saiu
para `_js_cobranca.html:35` (`window.cobrarChamado`), incluido por `base.html` -- e o sitio velho ficou
com **lapide**, nao vazio (`_lista_chamados.html:170`). `git grep` na raia da **1** definicao. O selo
afirma sobre as DUAS raizes pelo **fecho transitivo do `extends`** (`CASCA_ADMIN`/`CASCA_COLAB`), nao
por fixture: casos 04b (o admin alcanca), 04c (as telas que abrem o painel alcancam), 04d (a casca do
COLABORADOR **nao** recebe acao de gestor) e 04f (a previa do push e a mesma nas duas telas).

**NAO FUREI A FRONT-SEM-SMOKE-NAO-SOBE, e o commit toca `base.html`.** Quem suspendeu a espera foi o
aval dele, literal: *"O206 pousa agora por deploy --sem-migrate; o smoke de clique nas duas cascas e do
Fernando logo em seguida, e o resultado vem por mim; a frase do smoke entra no AVAIS"*. A frase entrou
**neste ato**: `O206-PAINEL-COBRAR-SMOKE`, AVAIS **5 -> 6**.

**E AQUI EU CORRIJO UMA AFIRMACAO MINHA, que estava FALSA no arquivo.** O RELATO de 20:1x dizia, sobre
esta mesma raia: *"e a frase ja esta no AVAIS"*. **Nao estava.** Medido: `0` item citando `O206` no
`PENDENTES_RONALD.json` (215 itens), e a busca por `PAINEL`/`COBRAR`/`Reabrir` devolvia 25 itens, **nenhum
deles este**. Eu dei por feito um ato que nao existia -- e o aval dele de 21:4x mandou fazer justamente
isso. O item nasceu agora, com `desde` lido do `date` no ato, e o JSON cresceu **12 linhas e removeu 0**
(forma canonica `indent=2`, sem churn). Nao e detalhe: AVAIS e a fila de DECISAO dele, e afirmar que a
trava esta na mesa quando ela nao esta e deixar a fatia pendurada em prosa.

**O CRITERIO DE PRONTO SAIU DA CELULA DO BACKLOG E FICA AQUI** (L-109, mover e nunca apagar). Era, literal:
*"PRONTO = selo de tela com os **4 casos** nas telas que renderizam o `modal_fio` + commit + print do
painel do chamado **#28304** com o botao; sem isso = INCOMPLETO."* Os 4 casos viraram **12**; o commit e
este; o print do **#28304** e o smoke do Fernando. **E o #28304 nao e instrucao velha**: medido no vivo
agora -- `status_local=em_analise`, VIVO pelo `motor.VIVOS`, col382 da emp2, **3 perguntas e 3 sem
resposta**, que e exatamente o gatilho do aviso que esta fatia reescreve.

**AS DUAS RAIAS ABREM NESTE MESMO ATO**, pelos avais dele de 21:4x: a `wt-bos` segue **uma de cada vez,
em paralelo com a principal** e abre a **O207** (VINCULO-INICIO-DECLARADO) assim que a O206 pousa -- com o
limite 4 do BOS-EM-RAIA-UM-POR-VEZ colado: constroi, mede e **PARA no `!`** antes de qualquer apply, o
codigo da porta pode pousar e **dado de vinculo nao**; depois dela, `O199 -> O198 -> GEO-SILENCIO-DE-PING
-> O197 -> O44 itens 2-8`. E a **O228** (ESMERIL-CELULA-TURNO) abre em **SEGUNDA raia**, so na familia
**celula**, pelo metodo da certidao de obito da L-111 -- censo por **AST, nunca grep de texto**. Nenhuma
das duas mergeia, deploya ou empurra: o pouso e meu, entre dois pousos meus (L-105).

**NO AR, com a prova do deploy** -- a DEPLOY-JA cobra smoke em prod logo depois, e a metade que e
minha e esta.
PROVA: deploy rc **0**, 3 rotas provadas (200/302/200), `importerror_500=0`, selo BUG 128 verde nas
tres cascas -- `logs/l115/o206_deploy.out`, 22 linhas.

O que o portao mediu: `bin/deploy.sh --sem-migrate` **rc 0** (`logs/l115/o206_deploy.out`) -- `0` migration pendente
no schema do cliente, `janela_auth` OK (nenhum sitio de auth mudou), sombra do **dia** com
`status=OK tipo=completa diverge=0 erros=0`, **collectstatic rodou** (static/template mais novo que o
manifest), prova de casca com **16 estaticos, 5 paginas compiladas, 608 rotas em 2 urlconf**, as **tres
cascas recarregadas juntas** e **tres rotas provadas** (core `/health/` 200, ui `/colaboradores/` 302,
mensageria `/health/` 200), selo BUG 128 verde nas tres e **`importerror_500=0`** na janela de 21:13 a
22:13. **O template ja estava na tela no ato do merge** -- nao ha `cached.Loader` (`base.py:74`) --, e
por isso o merge e o deploy foram **um ato so** (L-107). A metade que falta e o clique do Fernando, que
e o `O206-PAINEL-COBRAR-SMOKE` na mesa.

**ESMERIL DESTE MARCO (L-108 / ESMERIL-DO-MARCO): `0` orfao, e o censo e por ALCANCE, nao por grep de
prosa.** A O206 move duas funcoes, e mover e o caso em que orfao nasce: `cobrarChamado` tem **1**
definicao (`_js_cobranca.html:35`) e `hxPreviewCobranca` tem **1** (`:23`, mais o `window.` de
exportacao); os dois sitios velhos ficaram com **lapide**, nao vazios (`_lista_chamados.html:170` e
`painel_gestao.html:48`); o partial tem **UM** include, de `base.html:377` -- e **nao** do
`base_app.html`, que e exatamente o caso 04d do selo (*a casca do COLABORADOR nao recebe acao de
gestor*); e os **tres** sitios de chamada (`modal_fio.html:317`, `_lista_chamados.html:84`,
`painel_gestao.html:63`) alcancam a definicao pelo fecho do `extends`. Nenhum achado de fora do marco
entrou aqui: o que apareceu virou linha na fila.

**UMA LINHA DE HIGIENE, para nao ser descoberta depois**: para levar a `wt-bos` ao main eu descartei o
`app/docs/HANDOFF-SESSAO.md` local DELA, que era um handoff **gerado** de 08/10 23:11 apontando
`6c53bc46` -- derivado de `bin/handoff_sessao.sh`, vencido, que **prod nao le**, e com copia guardada
antes. Nao e o `!` de *voltar ao HEAD arquivo que prod usa*; se fosse, eu teria parado.

**E O PRE-PUSH FICOU VERMELHO, no meu proprio arquivo: `test_nenhuma_lapide_multilinha_em_templates`.**
Nada vazou -- e isso **se mediu**, nao se supos. A lapide do `_js_cobranca.html` esta em
`{% comment %}`, e dentro de um bloco de comentario o lexer do Django consome tudo: a linha 14 so
**mencionava** o delimitador de uma cerquilha na prosa que explica por que o cabecalho nao usa essa
forma. O selo (`holerite/tests/test_contract_lapide_nao_vaza.py`, corte dele de 05/09) varre **TEXTO**,
nao estrutura -- entao ele mordeu a prosa que documenta a cura. **E a 6a vez dessa classe nesta casa.**
PROVA: render pelo lexer REAL do Django, md5 **identico** antes e depois do patch
(`bc334ff3106fd89e1874f444899cdcac`, 2095 bytes, saida comecando em `<script>`, zero delimitador de
cerquilha na saida); e o **proprio codigo do selo** (sua regex e sua funcao `lapides_que_vazam`,
extraidas por AST do arquivo dele e executadas) acusa **0** nos templates depois do patch, contra **1**
antes.

**A cura foi a MINHA PROSA, nao o selo, e a escolha tem razao escrita** (CURA-MAIS-RESTRITIVA): das duas
candidatas, reescrever tres palavras de um comentario meu **nao relaxa nada**, enquanto ensinar o selo a
pular blocos `{% comment %}` **estreita o alcance de uma guarda** cujo falso negativo e texto na tela do
DP. O selo erra para o lado certo de proposito -- o docstring dele de 05/09 ja sabia que `{% comment %}`
existe e ainda assim escolheu *"qualquer cerquilha que nao fecha na propria linha"*. O arquivo do selo
**nao foi tocado** neste commit.

**A imprecisao do selo vira LINHA NA FILA, nao trabalho agora** (ESMERIL-DO-MARCO: achado de fora deste
marco nao abre escopo): *o selo lapide-nao-vaza varre texto e acusa cerquilha escrita em prosa DENTRO de
`{% comment %}`; 1 caso medido em 09/10, hoje zero; a cura certa le o `TextNode` do parser real, nunca
uma segunda regex que rastreie `{% comment %}` -- isso seria o derivador paralelo da LEI-AKITA 1.*

## O232 POUSO B — **NO AR, E O APPLY NAO E O DEPLOY: E A BATIDA** (09/10 21:3x, CONDICAO 4)

PROVA: `bin/deploy.sh --sem-migrate` rc **0** (`logs/l115/o232B_deploy.out`) — prova de casca com 16
estaticos, 5 paginas e 608 rotas em 2 urlconf; tres rotas provadas (core /health/ 200 · ui
/colaboradores/ 302 · mensageria /health/ 200); selo BUG 128 verde nas 3 cascas; **importerror_500=0**
(20:33 a 21:33). Remoto em `a5ae5013`, **0** a empurrar, suite do pre-push `Ran 10230 tests` /
`OK (skipped=42)` e `Ran 22` / `OK`, **0** ocorrencia de FAILED no log.

**SMOKE EM PROD, PELA TELA: 0 de 6 casos DIFEREM da sombra.** Nao e render de pagina — e a porta que a
tela usa (`_realizado_do_dia_tela`, com os quatro insumos montados como `montar_dias` os monta):

| dia-colab | prod (tela) | sombra | janela descontada |
|---|---|---|---|
| col923 03/09 | **533** | 533 | 60 |
| col923 07/09 | **581** | 581 | 60 |
| col923 18/09 | **541** | 541 | 60 |
| col735 10/09 | **447** | 447 | 0 |
| col736 17/09 | **325** | 325 | 0 |
| col899 26/09 | **538** | 538 | 0 |

**A 09 EXPORTADA, RELIDA DEPOIS DO DEPLOY (L-092).** 8 registros, **3 vigentes**, os tres com o sha
IDENTICO ao de antes: emp2 `361d0f9685f86d3a` (210 linhas), emp3 `5c503b95f9f9cd35` (86),
emp4 `84c78cd0871f5f52` (9). E o GRAVADO tambem nao se moveu, medido pelo CARIMBO em vez de por hash:
as 607 linhas de `FechamentoMensal` da 09/2026 tem o ultimo `atualizado_em` em **09/10 16:04:48** — ou
seja, **nada escreveu a 09 depois disso**, nem a re-lavra da 10 das 18:26, nem os dois deploys.

**EU ESCREVI ERRADO NA SECAO ANTERIOR, E A CORRECAO E O ACHADO DESTE ATO.** Eu disse que *"o gravado
nao se move por este deploy; o que o deploy move e a TELA"*. A primeira metade e verdade e esta medida
acima. A segunda e **falsa**, e quem me corrigiu foi o carimbo: a 10/2026 tem **344 escritas hoje**,
uma por colaborador, espalhadas de 05:01 a 21:34 — col155 21:30:07, col107 21:33:26, col259 21:34:26,
depois do meu deploy. Nao e cron (nenhum dos dois escritores do gravado e agendado, e isso segue
valendo). **E A BATIDA**, e e lei declarada: `ponto/registro_batida.py:119` chama
`_fechamento_por_evento(colaborador, timestamp, 'batida')` -> `recalcular_por_evento`, com o aval ao
lado — *"A BATIDA E O EVENTO (aval Ronald 27/09, E5)"*, nascido de um numero (*"em 28/09 ao recalcular
09 inteira: 58 colabs se moveram, horas_trabalhadas +289,50 h"*).

**ENTAO O APPLY DESTA CURA CHEGA POR TRAFEGO, UM COLABORADOR POR VEZ, E ISSO ESTA DENTRO DA LEI:**
- **a 09 EXPORTADA esta protegida na PORTA, nao por convencao.** `recalcular_por_evento` tem
  `except CompetenciaExportada` com a frase escrita no sitio (`ponto/services/fechamento.py:1033`):
  *"L-092: competencia exportada nao muda o gravado. Nao e falha -- e a lei funcionando."* Batida em dia
  da 09 e RECUSADA e logada. Por isso os +5.095 min da 09 seguem **publicados e nao aplicados**, e por
  isso **nao ha PAREI** aqui;
- **a 10 ABERTA recebe.** `recalcular_por_evento` recalcula a competencia **do dia do fato** (`quando`,
  nao hoje) para **aquele** colaborador. col899 ainda nao bateu depois do deploy — o gravado dele esta
  como a reversao o fotografou: `minutos_realizados=6264`, `horas_trabalhadas=102.24`,
  `horas_extras_50=0.00`, `turnos_abertos=2`, carimbo **16:00:08**. Na proxima batida dele o escritor
  CANONICO do sistema aplica os +419 min, sem ninguem rodar comando.

**AS QUATRO CONDICOES DA DINHEIRO-EM-COMPETENCIA-ABERTA ESTAVAM CUMPRIDAS ANTES DO DEPLOY**, e e por
isso que esse apply por trafego nao e um apply as escuras: (1) DIFF de frota publicado na secao acima,
ANTES; (2) reversao em `logs/l115/o232B_reversao_col899.json` — o gravado de col899 campo a campo mais
as 52 linhas de `DiaPago`; (3) 09 intacta, com hash e carimbo antes e depois; (4) esta secao.
**A conferencia que falta e de AMANHA, nao de hoje**: reler col899 e comparar com a reversao. Se ele
nao tiver batido, a re-lavra dele na 10 e ato PROPRIO e PRE-APROVADO pela mesma lei.

**O que fica na fila, nomeado:** a guarda identica em `_borda_posicional:507` e a discordancia de
duracao de pausa que ela cria em col107 21/08 (+5 min, 09 EXPORTADA, publicado e nao aplicado — **nao
vai a pauta DP** enquanto nao se decidir qual leitor manda). As pautas de DP da 09 sao de col923
(+4.649), col735 (+234) e col736 (+207), uma por colaborador, em `AVAIS.md`.

## O232 POUSO B — **O DIFF DA FROTA, ANTES DE QUALQUER ESCRITA** (09/10 21:3x, CONDICAO 1)

PROVA: medido na sombra nas DUAS arvores (`c7b8bfdb` limpa contra a curada), uma trava de sombra,
pelo espelho (`ponto/services/espelho.py`) sobre **12.889 dia-colab** — universo = todo colaborador
com batida nao retratada na janela, nao amostra. Log: `logs/l115/casos_o232.md`.

**A PREVISAO ERROU, e fica escrito.** Eu previa 9 dia-colab, col923 sozinho, +4.647 min na 09 e **0**
na 10 — previsao montada sobre os 18 dias de col923, isto e, uma AMOSTRA. A frota diz:

| competencia | dia-colab | minutos |
|---|---|---|
| 09/2026 (EXPORTADA) | 12 | **+5.095** (84,9 h) |
| 10/2026 (aberta) | 1 | **+419** (7,0 h) |

Por colaborador: **col923 +4.649** (9 noites), **col735 +234** (10/09), **col736 +207** (17/09),
**col107 +5** (21/08), **col899 +419** (26/09, comp 10). Uma chave de um lado so: `col923 08/09`,
que pagava **0** e foi absorvida em 07/09 (39 → 581) — zero minuto perdido.

col735, col736 e col899 sao a MESMA familia de col923 fora dele: a saida nasce depois da meia-noite e
o pareador a largava como turno proprio. **col107 nao e desta familia e nao se afirma como acerto**: o
dia sai do leitor `_borda_posicional` (que mede a pausa por criterio proprio, 65 min) e entra no corpo
de `realizado_dos_turnos`, onde `_pares_marcados` RECUSA o par `S 14:09 / S 15:14` (duas pontas
gravadas `S`, ata MUDA = abstencao pela R1 balde P) e vale a janela DECLARADA de 60. Sao **dois
leitores discordando de quanto durou a pausa** — a classe do pouso A, que JA existia e que esta cura
so atravessa. Fila, com a guarda identica de `_borda_posicional:507`.

**A FOLHA DA 10: `DIFF_FOLHA=1`, e o que se move NAO e o TXT.** O `sha256` das tres empresas e
IDENTICO antes e depois (`87e36e9ae36a`, `7c753392f8ea`, `58efa3d18779`) — `TXT=0 RETIDOS=1`. O unico
retido que muda e **col899/emp2**, que segue retido pelo MESMO motivo (`furo_espelho`) e com o mesmo
`apto_folha pct=56.0`: `horas_trabalhadas 95.40 → 104.36`, `horas_extras_50 0 → 1.96`,
`turnos_abertos 2 → 0`. **O Dominio da 10 nao recebe um centavo diferente por esta cura.**

A sonda dos turnos de col899 explica os dois numeros sem inventar um terceiro: em 26/09 a base tem
**DOIS** turnos (`n=3` e `n=1`) e a cura tem **UM** (`n=4`) — os dois turnos abertos sao o mesmo dia. O
motor pagava **0** no dia aberto e passa a pagar **538 min = 8,96 h**; o espelho ja MOSTRAVA 119 min,
entao o delta dele e `538 − 119 = 419`. `8,96 − 6,98 = 1,98 h = 119 min`, a menos de 1 min
(arredondamento de horas a 2 casas). Mesma familia do "+5.451 contra +7.345" da re-lavra da 10: *ata =
o que a casa viu; DiaPago = o que o dia paga*.

**A 09 NAO SE TOCA** (L-092): os +5.095 min ficam **PUBLICADOS e nao aplicados**, e a diferenca entre o
que a 09 pagou e o que passa a valer vai por **Pauta DP**, por colaborador. Condicao 2 cumprida ANTES:
`logs/l115/o232B_reversao_col899.json` — o GRAVADO de prod de col899 na 10 (`minutos_realizados=6264`,
`horas_trabalhadas=102.24`, `turnos_abertos=2`, mais os 52 `DiaPago`, campo por campo). Os hashes da 09
lidos ANTES pela porta de vigencia: emp2 `361d0f9685f86d3a` (210 linhas), emp3 `5c503b95f9f9cd35` (86),
emp4 `84c78cd0871f5f52` (9) — 8 registros, 3 vigentes.

**DOIS DONOS, e os dois com destino (L-099)**: a metade ESTRUTURAL de col899 26/09 e esta cura; a de
CADASTRO (DNA 08:00–16:00 contra batidas 11:00–21:00) e de col923 14/09 (noite trabalhada em dia de
FOLGA, recusada pela condicao 3) aparece na lista CADASTRO x REALIDADE — que e DERIVADA do motor
(`dias_cadastro_x_realidade`), nao escrita a mao.

**O DEPLOY NAO E O APPLY, e isso se LE no codigo -- nao se supoe.** Eu ia escrever aqui que o
`processar_cartorio --apply` das 06:28 lavraria sozinho o dia de col899 que o pouso B passa a fechar.
**Esta errado.** Os dois escritores do gravado nao sao rotina, e a casa diz por que, com o corte ao lado:
`config/crons.py:902` -- *"`recalcular_fechamento` NAO tem cron de proposito: quem decide QUANDO uma
competencia se recalcula e o DP -- cronificar seria o sistema reescrevendo a folha sozinho na
madrugada"* (corte Ronald 24/09) -- e `config/crons.py:797` -- *"BACKFILL DE OBRA, e obra nao vira
rotina"* para o `lavrar_dias_pagos`. `grep -E '^\s*C\('` nos dois nomes da **0** entradas agendadas, e
o `processar_cartorio` importa `impressao_insumos, julgar_colab, contadores`, nao `lavrar`. E a 09
EXPORTADA tem porta PROPRIA e estreita, `ponto/services/dia_pago.py::retratar_exportada`: *"sem
recalcular motor, sem tocar o fechamento"*, com a grade saindo de `grade_do_fechamento(fech)`.
CONSEQUENCIA: o gravado **nao se move por este deploy** -- nem o da 09 (L-092 intacta, sem PAREI) nem o
de col899 na 10. O que o deploy move e a TELA. A re-lavra de col899 na 10 e ato PROPRIO, PRE-APROVADO
pela DINHEIRO-EM-COMPETENCIA-ABERTA com as 4 condicoes ja cumpridas aqui (DIFF publicado, reversao em
`logs/l115/o232B_reversao_col899.json`, 09 intacta com hash antes e depois, prova depois) -- e e'
trabalho do ato seguinte, nao deste. **E a mesma classe que o aval nomeia**: *"a L-115 foi ao ar e a
folha nao se moveu sem re-lavra; a O219 e a cura dessa classe"*.

## O232 pouso A — **A GUARDA DA PAUSA LIA UMA AUTORIDADE E O CORPO SOMAVA OUTRA** (09/10 19:5x, marco)

Bug **PROVADO no caminho do O232**, curado na hora (LEI-AKITA 6). O pouso B — a borda que nasce,
que e o O232 propriamente — fica ABERTO e vem em cima deste.

**A ORIGEM, em uma frase.** Em `ponto/turnos.py::realizado_dos_turnos` (`:572` no HEAD `03fbfde9`) a
guarda perguntava `if _pares_marcados(t)` — QUALQUER par que a autoridade marcou — e o corpo somava
`intervalos_do_turno(t)`, que e so o par **acima** do `PISO_INTERVALO_SEGUNDOS` (900 s). Quando as
duas discordavam — todo par do turno abaixo do piso — a guarda entrava, o corpo somava **0** e o
`elif` da **janela DECLARADA** nunca era consultado: o dia pagava como jornada a hora de almoco que o
cadastro pre-assinala. Guarda e corpo passam a ler a mesma autoridade (LEI-AKITA 2), e
`_pares_marcados` deixa de ser um SEGUNDO juiz desta porta. **A cura e de UM token.**

**A LEI JA EXISTIA** (LEI-AKITA 4), e e por isso que nao nasce corte novo: a **L-032**
(pre-assinalacao do intervalo, CLT art. 74 par.2 — *"o intervalo CADASTRADO sempre sai da jornada"*),
o Art.71 par.1 que o **proprio docstring do juiz** ja citava (*"abaixo do piso nao e intervalo e
conta como trabalho"*) e o contador da casa `pares_de_intervalo_abaixo_do_piso` (esperado 0,
dono=admin). Nao faltava regra: faltava o leitor cumpri-la. Este commit toca o sitio que a **L-115**
protege (`realizado_dos_turnos`) e **nao** mexe em clausula dela.

**A ASSIMETRIA, o defeito dito do jeito mais curto:** quem **nao batia pausa nenhuma** tinha a janela
declarada descontada; quem batia um par de **14 min** nao tinha. O par sub-piso dava MAIS credito de
jornada que par nenhum. Nenhuma lei jamais disse isso.

**DUAS FORMAS, e elas NAO sao o mesmo bug** — discriminador = `intervalos_do_turno(t)` sem filtro:
- **FORMA A** (31 dia-colab, 28 perdendo janela): nenhum par acima do piso em todo o turno — nao
  houve pausa batida, e a janela declarada se aplica. **E so esta que a cura alcanca;**
- **FORMA B** (163 dia-colab): existe par acima do piso, mas ele cai **DEPOIS** do `_fim_fechado`.
  Forma medida **unanime** — 163/163 com a ultima batida `E`, 161/163 com o par comecando exatamente
  no `_fim_fechado` (`E 07:26 / S 13:01 (=fim) / E 14:00`): saiu para o almoco e **nao bateu a saida
  do fim do dia**. O trecho CONTADO e o de antes do almoco, que nao teve pausa nenhuma — entao o
  **0 de hoje e a resposta certa**, e descontar tiraria 60 min de col206 21/08, que trabalhou direto.
  A guarda **nao** virou `pausas == 0` por causa dela, e os casos B e C do RED mordem essa cura
  errada.

**E TESTEMUNHA, NAO FOLHA — medido, nao argumentado.** `ponto/motor_calculo_v2.py` **nao le** esta
porta (0 mencoes) e tem derivacao propria de pausa. Censo `quem-mais-le` = **6 leitores, nenhum
escreve dinheiro**: `ponto/services/espelho.py:300-301` (tela), `relatorios/pdf_espelho.py:522-523`,
`ponto/services/bordas_realizado.py:76` (contador de borda), `escala/regua_defesa.py:352` (vigia),
`escala/utils.py:1484` — dentro de `montar_grade_prevista_periodo_por_turno`, montagem em MEMORIA com
a linha literal *"copia em MEMORIA — nunca salva, nunca .save()"* — e
`relatorios/management/commands/vinculo_do_dia_divergentes.py:40` (relatorio). O lado da FOLHA
declara o mesmo por escrito: `folha/porta_export.py:36` registra que a porta do export **LIA**
`realizado_do_dia` e foi curada em 29/09 para comparar dinheiro com dinheiro — **ela le o MOTOR**.

**DIFF DE FOLHA DA COMPETENCIA 10, NA SOMBRA = 0** (`bin/simular_folha.sh par o232a`, as duas fotos
na MESMA trava — `logs/l115/o232_folha_par.out`):

    empresa 2 10/2026: linhas 131 -> 131 · retidos 315 -> 315 · entraram=0 sairam=0 mudaram=0 · IGUAL
    empresa 3 10/2026: linhas  68 ->  68 · retidos  58 ->  58 · entraram=0 sairam=0 mudaram=0 · IGUAL
    empresa 4 10/2026: linhas   5 ->   5 · retidos  12 ->  12 · entraram=0 sairam=0 mudaram=0 · IGUAL
    TXT=0 RETIDOS=0 · DIFF_FOLHA=0

E na forma mais forte que havia: o **sha do conteudo do TXT nao se moveu** em nenhuma empresa —
emp2 `87e36e9ae36a`, emp3 `7c753392f8ea`, emp4 `58efa3d18779`, identicos nos dois lados. Por isso
**nao ha apply, nao ha re-lavra, nao se devem as 4 condicoes da DINHEIRO-EM-COMPETENCIA-ABERTA e a
competencia 09 EXPORTADA nao e tocada**.

**DIFF DE ESPELHO, DUAS ARVORES, UMA TRAVA** (`o232_espelho_base.out` x `o232_espelho_cura.out`):
universo **12.889** dia-colab, **12.861 identicos, 28 MOVERAM**. A sonda ASSEGUROU, e nao so
imprimiu: universos iguais, **nenhum dia CRESCEU**, e os 28 todos de `janela 0` para `janela > 0`.

    == 09/2026: 20 dia-colab, -1.140 min, 7 colaboradores
       col252  21/08 25/08 27/08 31/08 01/09 02/09 03/09 04/09 09/09 10/09 11/09 16/09 17/09 18/09  (-60 cada)
       col269  24/08 -60    col349 14/09 -57    col592 30/08 -60
       col666  08/09 -60    col906 15/09 -60    col923 09/09  -3
    == 10/2026:  8 dia-colab,  -480 min, 2 colaboradores
       col217  29/09 -60     col252 23/09 24/09 25/09 28/09 01/10 02/10 06/10  (-60 cada)

**Zero** da forma B, como a regra previa. O `col923 09/09` a **-3** e o encaixe previsto com o pouso
B: hoje o dia fecha 04:03 e a janela 04:00-05:00 so o cruza por 3 min; quando a borda nascer ele
fecha 10/09 09:31, a janela cabe INTEIRA e o dia vale **543**, como os 8 vizinhos. **Sem este pouso A
o pouso B entregaria 603** — a pausa paga como trabalho. Os dois COMPOEM, e e a medicao que impoe a
ordem.

**A CURA LEVA A TELA *PARA* A FOLHA, nao contra ela** — a pergunta da coerencia da **L-099**,
respondida com numero (`logs/l115/o232_diapago.out`, so leitura na sombra):

    comp     colab   data        espelho base -> cura | DiaPago(motor): htrab  min_real  min_prev | intra
    09/2026  col252  2026-08-21   606 -> 546           |                600,0     540,0     540,0 | 1,00 h
    09/2026  col269  2026-08-24   717 -> 657           |                717,0     660,0     660,0 | 1,00 h
    10/2026  col217  2026-09-29   482 -> 422           |                471,0     420,0     420,0 | 0,88 h

O `DiaPago` de col252 paga `minutos_realizados` = **540** (o previsto) e lanca a hora de pausa que
faltou em **rubrica propria**, `horas_intra_indenizada = 1,00 h` (Art.71 par.4): **o motor nunca
pagou aquela hora como jornada**. A tela era o unico leitor que a somava dentro de *"Realizado"* —
606 contra 540 eram 66 min de folga; com a cura, 546 contra 540 sao **6**, o excedente REAL do
relogio. Nos 28 dia-colab a distancia tela-x-folha **diminui**; em 2 deles o motor paga 0 hoje
(col349 14/09 e col923 09/09) e ali a pergunta da tela e independente da folha. Nota de forma: a
leitura do `DiaPago` e da foto DEPOIS, porque foi o ultimo `--recalcular` da trava — e vale para as
duas arvores **exatamente porque** o `DIFF_FOLHA=0` provou que a base produz a mesma folha.

**RED EVIDENCIADO ANTES**, na copia do HEAD: `ponto/tests/test_pausa_zero_guarda_le_o_mesmo.py`,
10 casos em 3 classes — `Ran 10 tests` / `FAILED (failures=2)`, as duas `AssertionError: 606 != 546`.
Com a cura: `Ran 10 tests` / `OK`. Os casos que mordem a cura ERRADA estao la de proposito
(`test_MORDE_a_cura_ERRADA_pausa_DEPOIS_do_fim_segue_sem_desconto`, col206 21/08 a 335/0, e o gemeo
de col39 22/08 a 313/0), mais o caso normal que segue descontado e **sem carimbo** de janela.
**VIZINHOS:** 19 modulos que citam
`realizado_dos_turnos|realizado_do_dia|janela_descontada|_pares_marcados|intervalos_do_turno` —
`Ran 203 tests` / `OK`. Ruff limpo. **0 juiz novo, 0 escritor novo.**

**ESMERIL DO MARCO** (L-111): o unico candidato era `_pares_marcados`, que perdeu esta porta como
chamadora. Censo: segue com **3 chamadores de producao** (`ponto/turnos.py:373`, `:393` e `:507`) —
**zero orfaos a apagar neste marco**.

**A METADE DE CADASTRO NAO SE CURA POR CODIGO** (L-099): col252 declara 60 min de intervalo e bate
~14 min **todos os dias**. Vai para a lista CADASTRO x REALIDADE do admin, com o horario real ao lado
do cadastrado — nao por script.

**O CASO FOI ESCRITO ANTES DO CODIGO** (L-110): `logs/l115/casos_o232.md`, 390 linhas, com a regra, o
porque de cada condicao, as propriedades fixas, os casos medidos, o que a medicao MUDOU na regra, os
contra-exemplos e o que o numero NAO autoriza.

**A AUTORIDADE DA FILA E O MARCADOR, NAO O `PROXIMO PASSO` DO HANDOFF (linha de fila, medida 09/10 20:3x).**
Fechando o marco eu corrigi a celula da **O214** para a palavra que o unico leitor conhece (`espera o !` --
ela aguarda o `!` do apply do item 1 e o smoke do item 2, AVAIS #5), e o `hook_stop_fila1` passou a ve-la
como parada. Mas o `PROXIMO PASSO` do handoff NAO passou a dizer O232: ele chama
`H._proximo_da_fila()`, que e o **1o ABERTO na ordem FISICA da tabela**, e medido com a O214 fora o 1o
sobrevivente fisico e `PLACAR-ESTRUTURAL` -- item de fila 1 legitimo (o *"contratos"* do aval), mas **5o** na
ordem dele. A O158 ja havia desacoplado o marcador `ORDEM-VIVA-TOPO` de *"1o aberto"*, e esta e a ponta que
faltou migrar. **Quem manda e a linha `ORDEM VIVA (topo declarado)`, que o handoff le do marcador e que diz
`O232`.** Marcar os intermediarios como parados para forcar o numero seria mentir no dado para consertar o
leitor; a cura e o `PROXIMO PASSO` passar a ler o marcador, e ela e **INSTRUMENTO**, entao vai em pouso
proprio (L-105), nao neste.

**NO AR, e a prova e PELA PORTA (09/10 20:1x).** Push liberado em `c7b8bfdb` — negocio
PROVA: remoto em `c7b8bfdb` com **0** a empurrar (`git fetch` e `origin/main..HEAD` vazio); as tres cascas provadas no proprio deploy com `importerror_500=0` e selo BUG 128 verde; sombra do dia `dia=20261009 status=OK tipo=completa diverge=0 erros=0`.
`Ran 10216 tests` / `OK (skipped=42)`, control-plane `Ran 22 tests` / `OK`, `rc=0`, remoto em
`c7b8bfdb`, 0 a empurrar. `bin/deploy.sh --sem-migrate` (a fatia nao toca modelo): sombra
`dia=20261009 status=OK tipo=completa diverge=0 erros=0`, prova de casca 16 estaticos / 5 paginas /
608 rotas em 2 urlconf, tres cascas recarregadas juntas, tres rotas provadas, selo BUG 128 verde,
`importerror_500=0`. **SMOKE DE PROD, so leitura, pela porta que a TELA usa**
(`ponto/services/espelho.py::_realizado_do_dia_tela`, com os quatro insumos montados como
`montar_dias` os monta — nao se reconstroi a chamada que o sistema faz):

    col217 2026-09-29  minutos=422 (esperado 422)  janela_descontada=60 (esperado 60)  -> OK

Era **482 com janela 0**. O numero que a sombra previu e o numero que prod devolve.
**O QUE A DISTANCIA tela↔folha FEZ, MEDIDO E NAO SUPOSTO (LEI-AKITA 8).** Eu havia escrito aqui que
ela *"caiu de 60 para 0"*, e isso era CONTA MINHA, nao leitura: eu provei 482→422 pela porta da tela e
nao li o lado da folha. Lido (uma linha, so leitura, `DiaPago` de col217 em 2026-09-29, lavrada 09/10
05:55 UTC-3 — `lavrado_em` cru e `08:55:52+00:00`, UTC --, ANTES do deploy das 20:17): `minutos_realizados=420`, `minutos_previstos=420`. Entao a distancia
tela↔lavrado deste dia foi de **62 para 2 min**, nao de 60 para 0 -- e os 2 que sobram NAO sao ruido de
arredondamento enquanto ninguem os mediu. A mesma linha carrega `horas_trabalhadas=7,85 h` (471 min), um
TERCEIRO numero para o mesmo dia: fica como LINHA DE FILA, nao como achado desta fatia (ESMERIL).
E o lado da folha **nao se moveu com o deploy**: a lavratura e de antes, e so a re-lavra a move -- que e
exatamente o motivo do aval que poe a **O219** depois da O232.

**FILA (uma linha, pela ESMERIL — nao e trabalho agora):** `_borda_posicional` (`turnos.py:507`) usa
a guarda IDENTICA, `if any(_pares_marcados(t) for t in do_dia)`. A hipotese e que um par sub-piso de
14 min bloqueie a saida posicional da L-115 exatamente nos dias do tipo col252. **Mesma classe,
hipotese e nao prova** — fica como linha, para medir quando a fila chegar nela.

## O231 — **O COMANDO IMPRIMIA UMA MEDICAO E FECHAVA OUTRA; agora ha UMA** (09/10 18:4x, marco)

**AVAL** (`!` dele 09/10 17:2x, literal): *"curar fechar_cobranca_com_lastro na origem: fechar()
devolve o quadro que usou e o comando imprime ESSE"*.

**O DEFEITO, na origem.** `ponto/management/commands/fechar_cobranca_com_lastro.py::handle` chamava
`julgar()` para IMPRIMIR e, no fim, `fechar(True)` -- que julga de NOVO para AGIR. Duas medicoes do
mesmo universo, com minutos de distancia entre elas, uma a cada hora pelo cron. O log descrevia um
quadro e o comando fechava outro: na serie, `(a)=0` em dia que fechou 6 (soma **5 contra 10**
fechados). Nao e divergencia de contador -- e **a testemunha recalculando** (LEI-AKITA 2).

**A CURA, no sitio.** `ponto/services/lastro.py::fechar` passa a devolver `'quadro': r` -- a medicao
que USOU, com as LINHAS e nao so as contagens -- e o `handle` le ESSA, inclusive no dry-run
(`fechar(False)` nao escreve nada, entao o caminho de leitura e o mesmo dos dois lados). O rodape
`fechados=` sai do MESMO retorno. Nao sobrou caminho que meca duas vezes: o `julgar()` do comando
foi removido, nao mantido "por seguranca".

**RED EVIDENCIADO ANTES.** `ponto/tests/test_o231_lastro_mede_uma_vez.py`, 3 casos
(`Ran 3` / `OK` na copia): `test_MORDE_as_duas_medicoes_discordando` faz as duas passadas
discordarem e exige que a impressao siga a que AGIU -- vermelho no HEAD, verde na cura;
`test_dry_run_imprime_o_quadro_e_nao_escreve`; `test_a_porta_devolve_o_quadro_que_usou`.
**VIZINHOS:** `logs/l115/o231_vizinhos.out` -- `Ran 5571 tests` / `OK (skipped=9)`. Ruff limpo nos
tres arquivos.

**ESMERIL DO MARCO** (L-111, pelo metodo da certidao de obito): o unico candidato era
`ponto/services/lastro.py::julgar`, que perdeu o comando como chamador. Censo: `contador()` no
mesmo arquivo segue chamando -- **chamador de producao != 0, zero orfaos a apagar neste marco**.

**L-105, e em UMA linha:** a raia do **O206** esta verde e **nao pousa** -- ela toca template BASE e
depende do smoke de clique dele nas duas cascas (FRONT-SEM-SMOKE-NAO-SOBE). Nao e janela nem lei: e
smoke dele, e a frase ja esta no AVAIS. Por isso a fila 1 abre o **O232** com ela no ar.

**UI-RESPOSTA-DIZ-O-QUE-E FECHADA no mesmo ato**, pelo smoke dele de 18:3x (literal): *"abri a mesa
de disputa, a resposta longe do marco agora diz a direcao e o que validar faz, e o Reabrir avisa
quantas respostas apaga -- pode fechar a UI-RESPOSTA-DIZ-O-QUE-E"*. As duas celulas (BACKLOG e
TICKETS) diziam "espera o smoke dele" e viraram FECHADA **neste commit** (L-106: docs andam com o
marco). AVAIS: 6 -> 5.

**A ORDEM DELE DE 18:3x ESTA NO BACKLOG**, e nao em prosa: marcador `ORDEM-VIVA-TOPO` = **O232** (a
autoridade, pela O158) e as linhas subiram na tabela de 5 colunas na ordem O232, O219, O223, O224,
O228. O slot "contratos" e o **PLACAR-ESTRUTURAL** (R6 item 4) pela glosa DELE de 08/10 19:5x --
*"O219 -> R6 item 4 e os contratos que faltam -> O228"*. O `siga:` do hook segue dizendo `O214`
porque a ordem interliga as DUAS tabelas do bloco OBRAS, e a **O158 ja julgou isso**: o marcador e a
autoridade, o `siga:` e o segundo leitor, registrado como fila de INSTRUMENTO. Selo:
`bin/tests/test_hook_nao_cobra_congelado.sh` = `OK -- marcador 'O232' EXISTE e esta aberto`.

LEI-AKITA: origem=ponto/services/lastro.py::fechar (a porta devolve o quadro que USOU), testemunha=o
`quadro` da propria porta lido pelo comando, RED=ponto/tests/test_o231_lastro_mede_uma_vez.py::test_MORDE_as_duas_medicoes_discordando,
quem-mais-le=censo por AST de `lastro.julgar` e `lastro.fechar` -- so o comando e `contador()`, e o
`contador()` nao mudou; juizes novos=0

## RE-LAVRA 10 (L-115) — **NO AR, E A FOLHA SE MOVEU: +5.451 MIN DE REALIZADO EM 6 COLABS** (09/10 18:2x, CONDICAO 4)

PROVA: o GRAVADO, medido em `FechamentoMensal.minutos_realizados` da 10/2026 depois do apply (tabela do ATO 2: 4.617->4.829 · 6.397->7.087 · 1.003->2.104 · 5.849->6.754 · 5.990->6.641 · 4.298->6.190 = **+5.451**); ata `julgadas=108 carimbadas=108`; 09 EXPORTADA IGUAL nos 4 hashes (frota `67375ead…`, 607 linhas); `logs/l115/relavra10_apply.out` rc **0**.

A prova que a DINHEIRO-EM-COMPETENCIA-ABERTA cobra DEPOIS. Log integral:
`logs/l115/relavra10_apply.out` (rc **0**). Os quatro requisitos, cada um com onde se confere:

| condicao | como foi cumprida | prova |
|---|---|---|
| 1 DIFF publicado ANTES | pre-check de leitura em prod, reconciliado com o publicado | secao abaixo + `logs/l115/prod_precheck.out` |
| 2 reversao em `logs/` ANTES | foto FRESCA (0 s) + restore provado em DRY | `logs/l115/relavra10_foto_182709.json` (180 celulas, 6 fechamento, 340 DiaPago) · `logs/l115/relavra10_restore_dry.out` |
| 3 09 exportada intacta | 4 hashes, antes e depois | atas 09 dos 6 `ae1a20fa…` IGUAL · fechamento 09 dos 6 IGUAL · fechamento 09 da FROTA `67375ead…` (607 linhas) IGUAL · `ExportacaoDominio` 09 (8 registros) IGUAL |
| 4 prova DEPOIS | esta secao | — |

**ATO 1 — A ATA.** `julgar_colab(..., forcar=True)` sobre a lista INTEIRA da comp10 de cada colab,
piso em `periodo_apuracao(10, 2026, 21)` = 2026-09-21 e teto `min(FIM10, hoje-1)` = 2026-10-08.
`julgadas=108 carimbadas=108 protestos=43 emitidos=5 nunca_bateu=0 vetados=0`.
**A ata moveu em 38 dia-colab, e os 38 estavam escritos antes de escrever:**

- **36 do censo**, cada um com o delta EXATO do DIFF publicado (o selo e por dia, nao por soma):
  soma **+7.345 min**, identica a publicada. `0` dia do censo deixou de mover;
- **2 declarados FORA** na condicao 1 — `col941 2026-09-30` (+16) e `col941 2026-09-25` (+3),
  familia `ata_x_juiz_residual`/BUG-144, que re-lavra de janela inteira drena: **+19**;
- **0 dia-colab fora do censo e nao declarado**, e **0** movimento sub-minuto;
- **0 chamado e 0 pergunta nascidos** (delta de CONJUNTO no banco contra a foto, nao `contadores()`).

**ATO 2 — O FECHAMENTO** (`recalcular_fechamento --mes 10 --ano 2026 --colabs 204,263,270,502,941,946 --apply`).
E ele que responde a frase do aval de 18:3x — *"a folha nao se moveu sem re-lavra"*:

| colab | minutos_realizados 10/2026 | delta |
|---|---|---|
| col204 | 4.617 -> 4.829 | **+212** |
| col263 | 6.397 -> 7.087 | **+690** |
| col270 | 1.003 -> 2.104 | **+1.101** |
| col502 | 5.849 -> 6.754 | **+905** |
| col941 | 5.990 -> 6.641 | **+651** |
| col946 | 4.298 -> 6.190 | **+1.892** |
| | | **+5.451 min** |

**POR QUE +5.451 E NAO +7.345, e isto nao e divergencia**: a ata guarda o realizado CRU
(col263 24/09 vai a 551 min), e o `DiaPago(motor)` do dia se fecha no previsto do DNA (420 no caso).
Os dois leem a mesma autoridade; o que difere e a PERGUNTA — ata = "o que a casa viu", DiaPago =
"o que o dia paga". O numero do aval e o da ATA, e ele fechou no minuto.
**Um unico dia-colab fora do censo mexeu no ATO 2**: `col263 2026-10-09` 306 -> 367 (**+61**), com a
ata PARADA nesse dia — e deriva de fechamento do dia de HOJE, que o recalculo drena e que esta
competencia aberta absorve. Nenhum outro campo de `FechamentoMensal` de nenhum dos 6 se moveu:
so `minutos_realizados`.

**O QUE A 09 FEZ: NADA.** Os 36 dias do censo ficam todos em 21/09..08/10 por CONSTRUCAO (o piso e
`INI10`), e o DRY imprimiu `dias do censo dentro da 09 EXPORTADA: nenhum` antes de qualquer escrita.
`processar_cartorio --forcar` NAO foi usado, e o motivo esta no docstring do apply: a fila dele e
todo `CelulaDia` com `data < hoje` da empresa, **sem piso** — ele alcancaria a 09. A guarda de
exportada (`chamados/juizes.py:1195`) silencia EMISSAO, nao lavratura.

**A REVERSAO SEGUE DE PE**, e e uma linha: `logs/l115/relavra10_restore.py` com
`DRY=False` sobre `logs/l115/relavra10_foto_182709.json` (copiar para
`saas_core:/tmp/l115_relavra10_restore.py` e a foto para `/tmp/l115_relavra10_foto.json`).

**ORDEM DA FILA 1, ordem Ronald 09/10 18:3x (literal):** *"ordem da fila 1 passa a ser O232 ->
O219 -> O223 -> O224 -> contratos -> O228. Motivo: a L-115 foi ao ar e a folha nao se moveu sem
re-lavra; a O219 e a cura dessa classe e tira a L-113 do papel. Nenhuma lei nova."* Registrada no
bloco OBRAS do BACKLOG; **nao e lei nova**, e re-ordenacao.

LEI-AKITA: origem=ponto/services/cartorio.py::julgar_colab (a ata, re-lavrada no sitio), testemunha=CelulaDia.ata lida por folha/export.py::grade_do_fechamento, RED=logs/l115/relavra10_dry.out (36 esperados, delta por dia) -> logs/l115/relavra10_apply.out (36/36 OK), quem-mais-le=DiaPago(motor) + FechamentoMensal dos 6, medidos campo a campo; juizes novos=0

## RE-LAVRA 10 (L-115) — **CONDICAO 1: O DIFF RECONCILIADO CONTRA PROD, ANTES DE ESCREVER** (09/10 18:1x)

**AVAL** (Ronald, 09/10 18:1x, literal): *"a re-lavra da L-115 na competencia 10/2026 (aberta) e
PRE-APROVADA pela DINHEIRO-EM-COMPETENCIA-ABERTA: DIFF ja publicado (36 dia-colab, +7.345 min),
reversao em logs/ antes, 09 intacta com hash antes e depois, prova no RELATO. A 09 nao se toca: fica
publicada."*

**O DIFF QUE DESCREVE ESTE ATO E O DE PROD, NAO O DA SOMBRA** (condicao 1). A sombra mediu sobre o
dump das 04:00; o ato escreve em prod as 18:xx. Pre-check de LEITURA em prod, recortado nos 6 colabs
alcancados e **sem** o filtro `tipo_dia == 'trabalho'` da sonda da sombra
(`logs/l115/prod_precheck.py` -> `.out`, 18:11:56):

| | dia-colab | minutos |
|---|---|---|
| publicado na sombra (04:00) | 36 | +7.345 |
| prod, 18:11 | **37** | **+7.489** |
| menos `col941 2026-10-09` — **HOJE** | -1 | -144 |
| **o que este ato alcanca** | **36** | **+7.345** |

A deriva e UMA linha e ela **nao entra por construcao**: o cartorio so julga dia passado e a lista
do ato tem teto `LIM = hoje - 1` (fato encerrado, CLAUDE.md 6). O numero do ato e **identico ao
publicado**. A cura do pouso 1 esta **VIVA na casca que vai escrever** (`pos=True` em 37 dias; se
fosse `0` o deploy das 17:35 nao teria pegado, e o ato pararia aqui).

**O QUE A LISTA INTEIRA DRENA ALEM DOS 36, DECLARADO ANTES** (a lista vai inteira: `_julgar_colab_corpo`
tira a janela de batidas de `cels[0].data`/`cels[-1].data`, e recorte por dia fabrica par fechado com a
cauda da vespera). Nos 6 colabs ha **2 dias de familia diferente**, que o `forcar` vai relavrar junto:
`col941 2026-09-30` (ata 444, juiz 460, **+16**) e `col941 2026-09-25` (447 -> 450, **+3**) — **+19 min**,
`pos=False`: sao do passivo `ata_x_juiz_residual` (BUG-144), **nao** da L-115. Ficam **declarados fora do
criterio**, com o numero, em vez de aparecerem depois como surpresa. Qualquer OUTRO dia-colab que se mover
= PAREI com a tabela e restauro daquele colab antes do ATO 2.

**FORA DO ESCOPO, nomeado** (ESCOPO DO AVAL E LITERAL): `col373 2026-10-07` e alcancado pela lei e e
**invisivel** para a sonda por ser dia de **folga**; delta **nao medido**, nao entra neste ato — fica
residual nomeado para o pouso 2. Os **35 dia-colab / +6.871 min** da **09/2026** seguem **publicados e
nao aplicados** (L-092 / TXT-FOTOGRAFIA): o unico caminho deles e a porta REGEN-EM-EXPORTADA.

## L-115 POUSO 1 — **A SAIDA PELA POSICAO ESTA NO AR, E A FIGURA DO AVAL NAO E ESTA** (09/10 17:3x)

**A FORMA B DO POUSO 2 ESTA MEDIDA, e o primeiro numero que ela deu era sonda errada (18:0x).**
Duas rodadas devolveram `dia-colab=0` nas duas janelas, e zero contradizia fato ja medido -- as 8
noites de col923 que pagam 0. Em vez de publicar o zero, perguntei **por clausula**, dia a dia
(`logs/l115/diag_col923.out`), e as duas clausulas erradas eram minhas: (1) exigir a pausa
**INTERIOR**, quando a noite tem 3 batidas e a pausa e o **ULTIMO** par -- a batida que fecha a noite
esta arquivada no turno do dia seguinte; (2) exigir que o 1o turno do dia seguinte tivesse UMA
batida, quando em 06/09 tem duas. Corrigidas, a sonda acha exatamente as noites medidas:
**9 dia-colab, 1 colab (col923), +4.887 min = 81,5 h de TRABALHO NAO CONTADO na 09/2026 (EXPORTADA), e ZERO na 10/2026
(aberta)** -- nenhum centavo da competencia em curso depende disto. Forma real: turno ABERTO,
`pares_marcados=0`, 3 batidas todas `E`, pausa de 55-77 min no ultimo par, `previsto=420`, **juiz=0**,
saida real ~09:2x do dia seguinte. Dono **ESTRUTURA** (L-099); origem `parear_turnos`/`data_turno`,
nao o leitor de borda. A conta e `(span - pausa) - juiz`: **nao e HE** -- 3.780 min dos 4.887 sao jornada normal, e o resto
(~1.107 min) e ponta de HE pela L-097 contra um marco agora **LIDO**: `hi 23:30 / hii 04:00 /
hfi 05:00 / hf 07:30`, 6x1, 420 min, celula `origem=gerada` em todos os 18 dias
(`logs/l115/marcos_col923.out`). O `cel.marcos=[None]*4` da sonda anterior era atributo errado, nao
cadastro vazio -- e com o marco lido a **L-084 nao se aplica** (a entrada esta EM CIMA do `hi`; so a
saida esta longe), entao o dono nao e CADASTRO. Universo:
ECs ativas HOJE, entao "so col923" nao e exclusividade de frota. Terceira figura nomeada: col923
09/09 (borda fechada em 10/09 04:03, juiz=275), ao lado de 02/09 (+121), as duas na lista do O232.
Prova: `logs/l115/shapeB_MEDIDO.md`. **O que o numero nao autoriza**: a 09
esta exportada -- as 81,5 h nao entram por recalculo; se forem ao Dominio, vao por pauta DP com os
dois numeros.
LEI-AKITA: origem=`ponto/turnos.py::realizado_dos_turnos` (ramo de DIA, antes do laco por turno),
testemunha=`CelulaDia.ata` + `_fim_fechado`/`_pares_marcados` (nenhum leitor novo deriva nada),
RED=`logs/l115/red1.out` (A 370 -> 550, B 192 -> 421), quem-mais-le=todo leitor de `realizado_do_dia`
pela ATA (censo no `casos.md`), juizes novos=**0** (`_borda_posicional` e clausula do juiz que ja existe,
nao autoridade nova — nada em `core/juizes.py`).

**O QUE POUSOU.** `_borda_posicional` responde a lei do aval de 13:3x com SEIS clausulas, cada uma medida
contra o dia que a exige, e o dia alcancado deixa de ser ABERTO, carimba `borda_posicional` e ganha
contador informativo `realizado_borda_posicional` em `ponto/services/bordas_realizado.py` (esperado **nao**
e 0 — e o universo da lei; o que se vigia e ele crescer sem obra que o explique). Os 8 casos da regra
estao em `SaidaPosicionalDepoisDaPausaTest`, escritos **antes** do codigo (L-114). Suite inteira na copia:
**`Ran 10211 tests` / `OK (skipped=42)`**, rc=0 (`logs/l115/suite_final.out`).

**O UNIVERSO, MEDIDO NA SOMBRA, NAO ESTIMADO.** Competencia **10/2026**: **36 dia-colab, +7.345 min
(122,4 h)** — e **nao** os 33 do balde do oraculo; col204 (1 dia) e col270 (4) estao na lei e fora do balde,
e col373 07/10 e alcancado mas invisivel para a sonda por ser folga. `realizado_turno_aberto` **289 -> 253**
e `ata_x_juiz_residual` **27 -> 63**: a MESMA familia de 36 dias, saindo de um contador e entrando no outro.
Competencia **09/2026**: **35 dia-colab, +6.871 min (114,5 h)** — numero para **PUBLICAR, nunca para
aplicar** (L-110), porque a 09 esta EXPORTADA; o unico caminho dela e a porta REGEN-EM-EXPORTADA, com TXT
parcial de retificacao. Cinco colaboradores aparecem **so** la (col207, col913, col788, col868, col107).

**O DEPLOY SOZINHO NAO MOVE A FOLHA, e isso nao e consolo — e a leitura certa de `DIFF_FOLHA=0`.** A folha
le a **ATA** (`cartorio.py:285` escreve `minutos_realizados` -> `leitor_celula.py:364` le -> `dia_pago` ->
`fechamento.py:533` -> `folha/export.py`), e `impressao_insumos` hasheia **so INSUMOS** (batidas, cobranca,
chamados, dna, veto, teto), nunca a derivacao: entao um deploy de codigo nao invalida impressao nenhuma e o
cartorio das 06:28 **pula** os dias alcancados. O numero so se move na **RE-LAVRA**, que e o ato `!` ja
declarado da BUG-144 (`app/docs/TICKETS.md:936`: Pauta DP + `!` dele), com **reversao em `logs/` antes**.
Nenhuma lei nova para isso (LEI-AKITA 4). O que `ata_x_juiz_residual` 27 -> 63 diz e exatamente isso:
**63 dias em que a ata guardada discorda do juiz vivo e esperam a re-lavra** — passivo nomeado, nao divida
escondida.

**A FIGURA DO AVAL DE 15:1x NAO E ESTA BORDA, e o complemento se mediu antes de se responder.** col923,
02 a 19/09: a noite generica dele entra ~23:2x, faz pausa nos marcos ~04:0x/~05:0x e sai ~09:2x do dia
seguinte **com as quatro batidas gravadas `E`** — o botao errado toda vez. Hoje **8 dos 18 dias pagam ZERO**
(03,04,05,06,10,16,17,18/09, turno `aberto=True`): quem trabalhou ~10 h recebe nada. **A clausula (1) do
pouso 1 nao os alcanca** — ela exige um turno com borda FECHADA, e nestas noites **nenhuma `S` existe** —,
e a batida da manha o pareador poe no dia SEGUINTE (`data_turno`), fora da lista do dia que ela encerra.
Ali a borda nao se move: ela **NASCE**, e a origem e `parear_turnos`, nao o leitor de borda. Fundir as duas
curas obrigaria a remedir tudo de novo com a origem trocada, entao ela anda como **O232 / POUSO 2**, com o
caso escrito pela regra em `logs/l115/casos.md` (L-110) e os 8 dias passando de dono **BATIDA para
ESTRUTURA** (L-099, e e a correcao de uma classificacao minha de hoje de manha). No mesmo pouso 2 fica
col923 **02/09**, o unico dos 18 com borda fechada: passa (1),(2),(2b),(4),(5) e a **clausula (3)** o recusa
porque a ata MARCOU a pausa (`papel=Xi` nas duas pontas) — pela regra o dia valeria **544** contra **423**
de hoje (+121). Afrouxar a (3) mexe na guarda que a cura do balde P acabou de pousar (col518 05/09, +38 min;
col923 12/09 e 19/09), entao ela so se afrouxa com censo proprio na sombra, nunca de passagem.

**O que NAO entrou neste commit, nomeado:** a sonda `app/ponto/tests/test_zz_sonda_l115.py` (instrumento de
medicao, ficou na copia); `bin/sombra.sh` (instrumento, pouso proprio pela L-105); a Pauta DP da 09 do balde
P. **Avais respondidos neste turno** (`bin/gerar_avais.py --escrever`, mesa 8 -> 6): `O214-ITEM3-TRAVA-LIGADA`
(*ok-desenho*, a trava do TXT fica LIGADA sobre os 81 dias — e o estado que ja esta no ar, nada construido)
e `LASTRO-MEDE-DUAS-VEZES` (`!` concedido, virou **O231**: `fechar()` devolve o quadro que usou e o comando
imprime ESSE — LEI-AKITA 2, testemunha LE, nao recalcula).

## L-114 TERMOMETRO RODADO — **O AMARELO CAIU POR MEDICAO, E OS 19 SEM DESTINO GANHARAM DONO** (09/10 15:1x)

PROVA: `logs/e6_cauda2c/r1_dono_10_20261009.txt` (o nome e o que a fonte declarada do R1 pede,
`fonte_do_medido: 'logs/e6_cauda2c/r1_dono_*.txt'`) — competencia **10/2026 ABERTA**, na sombra, carimbo
`dia=20261009 tipo=completa diverge=0 erros=0`: **4411 dias comparados, BATE 93,9%**, `erros no espelho: 0`.
R1 por dono: **ESTRUTURA 110** (487,8 h, 42 colabs) · **CADASTRO 11** (84,2 h, 7) · **BATIDA 146**
(706,4 h, 102) = **267**, e a soma fecha com o total de divergentes — o proprio comando a cobra.
O placar renderizado pelo publicador saiu **`VERDE (termometro em dia)`**, R1 com `medido em 2026-10-09`
e `EM DIA`, contra o `AMARELO (termometro sem data de medicao na fonte declarada)` de antes.
Selos: `bin/tests/` **INTEIRA verde**, ruff limpo, `core/tests/test_l114_placar_papel_e_termometro.py`
na suite `--only core`: **`Ran 1263 tests` / `OK (skipped=21)`**, rc=0 (`logs/l114/suite_core_l114.out`), e o selo rodado NOMEADO a parte -- **`Ran 11 tests` / `OK`** com as 11 `test_MORDE_*` listadas em `logs/l114/selo_l114_so.out`, porque o ponto da suite longa nao diz QUAL teste correu.

**A CADENCIA E DO AVAL, e ela mordeu a casa antes de eu rodar**: *"o termometro e OBRIGATORIO com
cadencia: e6_oraculo por competencia aberta 1x por semana e apos cada pouso de obra de calculo; cada dia
de dono ESTRUTURA novo vira CENARIO na bateria ou linha na lista do admin, nunca fica sem destino. Bateria
verde sem termometro rodado na semana = placar AMARELO."* O `185b9af0` foi pouso de obra de calculo, entao
o termometro **devia** rodar — e foi o AMARELO do placar que cobrou, nao a minha memoria. Era para isso que
ele nasceu.

**A SEMANA SE LE POR TAXA, NAO POR CONTAGEM.** ESTRUTURA foi de 95 (02/10) para **110** (09/10) e isso
**nao e regressao**: o denominador da mesma competencia cresceu de **2694 para 4411** dias comparados —
a 10 estava pela metade em 02/10 — enquanto o **BATE subiu de 92,4% para 93,9%**. Registro tambem o que o
instrumento **nao** sabe responder: entre 02/10 e hoje, 26 dia-colab SAIRAM do csv, e um csv so de
DIVERGENCIA nao distingue *"passou a bater"* de *"deixou de ser comparado"*. Chamar os 26 de "curados"
seria inventar sinal onde ha ausencia dele.

### OS 19 DIAS DE `ESTRUTURA` SEM BATIDA NENHUMA — 4 formas medidas, 19 com destino, 0 sem
49,7 h de 487,8 h, 10 colaboradores, todos da classe `e6_zero_esp_trabalho`. Censo em
`logs/l114/censo19b_sombra.txt` (por dia: marcos da celula, quais batidas a grade USOU `hora->marco`,
quais slots ficaram `missing`, orfas, e D-1/D/D+1 com esp/orac/paridade). Leitura so de autoridade
(`espelho_do_colab`, `minutos_do_oraculo`, `Batida`, `CelulaDia`, `DiaPago`), pela porta
`bin/sonda_leitura.sh --sombra` — 5 sondas, rc=0 nas cinco.

| forma | dias | destino |
|---|---|---|
| **G1** jornada noturna que acaba em D+1 com a **ENTRADA ausente**; conjunto do turno IMPAR | **5** | **dono BATIDA**, lista do admin (L-099) |
| **G1b** mesma forma, turno **COMPLETO** — so a chave difere | **1** | limite nomeado do instrumento; zero fatia |
| **G2** dia de **FOLGA** pago com o rabo do turno de D-1, e D-1 paga ~0 | **3** | fecha com o dia D-1, que ja esta no csv |
| **G3** cadastro **DESLOCADO** (celula diz turno diurno, o colaborador trabalha a noite) | **10** | **CADASTRO x REALIDADE**, lista do admin — proibido curar por codigo (L-099) |

**"DESTINO" AQUI QUER DIZER LINHA QUE EXISTE, e eu fui perguntar a casa em vez de rotular.**
PROVA: `logs/l114/casa_sombra.txt` e `casa2_sombra.txt`. Os **5 de G1 tem chamado `batida_ausente`
carimbado NO DIA** — #28873 `em_analise`, #25155 `resolvido`, #27445 `em_analise`, #27070 `em_analise`,
#28011 `aberto` —, e essa e a casa que o proprio R2 mediu para o dono BATIDA (*"com chamado carimbado
NO DIA"*), nao a lista. Os **3 colaboradores de G3 estao na lista CADASTRO x REALIDADE** (`lavrado_em
2026-10-08T08:31`, universo 519, 357 listados): col788 `A2 A3 A5 A8 A10`, col868 `A8`, col949 `A5 A7 A10`.
A granularidade da lista e **colaborador+assinatura, nao dia**, e e o desenho certo para um cadastro
deslocado em 7 noites seguidas. Isso corrige tambem uma suposicao minha: eu achava que a lista fosse
alimentada por `dias_cadastro_x_realidade` (o juiz da L-084, que exige as DUAS pontas longe) e que por
isso nao veria um dia de perna so — ela le `esmeril_espelho::ler_lavra`, outro juiz, e e por isso que
os tres aparecem.

**G1 — e o espelho que NOMEIA quem falta.** col37 05/10 `missing E@19:00`; col174 23/09 `E@21:00` e
`S@02:00`; col235 30/09 `E@21:00`; col250 29/09 `E@18:00`; col594 02/10 `E@19:00`. Nos cinco, o conjunto
da JORNADA (nao o do dia de calendario) e **IMPAR**, que e a lei da BUG-144 lida no conjunto certo.
Nao se adivinha a batida ausente: o slot vazio da grade ja diz qual marco ficou sem ninguem.

**G3 e o maior do censo, e nao e estrutural.** col949 tem **7 noites seguidas** com celula
`hi 07:00 / hii 12:00 / hfi 13:00 / hf 19:00` e batidas 18:00→06:00: cada noite paga **0** no espelho
enquanto o oraculo le 660 · 660 · 659 · 385 · 664 · 664 · 659 · 481. A causa esta medida — a grade do dia
SEGUINTE toma a ultima batida da noite (`06:00 -> marco 07:00`, 60 min de distancia) como se fosse a sua
ENTRADA, e a noite fica com uma perna so. Isso e **consequencia do cadastro deslocado**, nao defeito
independente: com a celula certa a batida acha o seu proprio marco. Vai para a lista do admin com o
horario real ao lado do cadastrado, e a L-099 **proibe** curar por codigo. Com col868 21/09, col788 26/09
e col788 08/10, sao 10 dias.

### CORRECAO DE UMA AFIRMACAO MINHA, DO MEIO DESTE MESMO TRABALHO
Eu escrevi em `logs/l114/achado_chave_do_dia.md` que col174 23/09 **contava a mesma cauda DUAS VEZES**.
**Nao conta.** A leitura crua da grade (`logs/l114/cru_sombra.txt`) mostrou que as duas primeiras linhas
do dia sao `{'missing': True, ...}` e que o campo `hora` ali e o **MARCO**, nao uma batida — e as batidas
usadas (#108731, #108746, #108754, todas de 24/09) sao disjuntas das de 22/09 (#107686, #107798, #107816,
#107824). **Nenhum minuto e pago duas vezes**, e nos 19 casos os dois leitores do PRODUTO (espelho e
`DiaPago`) dizem o MESMO numero. O que eu li como duplicidade era a hora do marco impressa no slot vazio
pela **minha propria sonda**. A correcao esta no topo do arquivo, nao enterrada nele.

### O QUE MUDA NO INSTRUMENTO — e por que nao muda neste commit
A escolha escrita no `dono_da_divergencia` (*"o dia sem batida nenhuma cai em ESTRUTURA... ali o fato
existe e foi atribuido a outro dia, e atribuicao e do sistema"*) tinha a **suspeita certa e o reu errado**.
Nos **6 de G1+G1b** a atribuicao que difere e a do **ORACULO** — a chave dele e `tu[0].date()`, o dia de
calendario da primeira batida do turno, e o espelho lavra pelo dia da **JORNADA**. O oraculo nao le celula
**de proposito** (e o que o faz testemunha independente), entao isso e limite declarado, nao bug do produto. Nos **13 de G2+G3** o que difere nao e chave nenhuma: e o CADASTRO, que nao descreve o
turno. Em **nenhum dos 19** os dois leitores do produto discordam entre si — era isso que a palavra
"ESTRUTURA" estava afirmando, e e isso que a medicao nega.
A revisao, pela CARACTERIZACAO-SE-INVERTE: o selo `test_MORDE_dia_sem_batida_e_ESTRUTURA_nao_BATIDA`
**nao se apaga** — a assercao dele troca e passa a morder a VOLTA, perguntando a paridade ao conjunto do
TURNO do proprio oraculo, sem ler celula. Juiz novo = 0, dono novo = 0. **Isso e INSTRUMENTO e nao pousa
junto com produto (L-105)**: vai em pouso proprio, com RED antes.

**LEI-AKITA**: origem=`core/placar_estrutural.py` (o numero do R1) e `bin/gerar_estado.py` (quem publica
faz o I/O), testemunha=`r1_dono_10_20261009.txt` lido da fonte DECLARADA pelo nome do arquivo (nunca
`mtime`), RED=`AMARELO (termometro sem data de medicao na fonte declarada)` no artefato real, virando
`VERDE (termometro em dia)`, quem-mais-le=`placar_estrutural` e importado por **um** sitio
(`bin/gerar_estado.py`) e pelos seus testes — **nada em container o importa, entao prod nao precisa de
deploy**, juizes novos=0.

## O230 POUSO 1 — **A SONDA DE LEITURA DO DEV DA CASA ESTA NA ARVORE** (09/10 14:1x, POUSO 1 FECHADO)

PROVA: `bin/sonda_leitura.sh` na arvore (de `1723901e` da raia `agent-a375cb746ca034f0c`), os tres REDs
rodados **como `fernando`** pelo caminho real `sudo -u ronald`: `logs/o230/red_a_20261009.txt` (886
colaboradores pelo ORM e **886** por SQL cru, nos DOIS bancos), `logs/o230/red_b_20261009.txt` (**4 de 4**
escritas negadas por `permission denied for table` com o cinto DESLIGADO, contagem 886 antes e depois nos
dois bancos), `logs/o230/red_e_20261009.txt` (**18** variaveis no env da sonda, **0** chaves da casa,
contra **26** no `saas_core`) e `logs/o230/red_dono_e_argv_20261009.txt`; **64 selos de host verdes**.

**O AVAL, LITERAL** (09/10 13:4x, registrado no PROMPTS): *"O230 pousa AGORA, na frente do que vier depois
do apply do R1. O role leitor e o sudoers ja existem; falta bin/sonda_leitura.sh na arvore e a leitura de
logs e docs. Pousa o script com os REDs a, b e e provados; os outros REDs e o selo vem no pouso seguinte de
instrumento. Nenhuma lei nova."* O escopo e literal e foi cumprido literalmente: pousou **um arquivo**,
`bin/sonda_leitura.sh`. O selo `bin/tests/test_sonda_leitura.sh`, que existe na raia e pergunta ao CATALOGO
do Postgres, **ficou lá** — ele e o POUSO 2, com os REDs c, d e f. Ato de INSTRUMENTO proprio (L-105),
depois do produto, e **sem deploy**: nada que o gunicorn serve mudou.

**UMA CORRECAO AO SCRIPT ANTES DE POUSAR, e e a secao 4 do CLAUDE.md se aplicando a mim.** As linhas 15-16
prometiam que *"o selo le `has_table_privilege` do Postgres, nao esta prosa"* — e o selo nao vinha neste
pouso. Promessa de guarda inexistente e exatamente o que fez o cartorio ler batida crua por meses. A linha
agora DIZ que o selo e o POUSO 2 e que ate ele a guarda e documental mais o RED b.

**O RED b MEDIA O CINTO E DAVA ISSO POR PROVA — e o cinto e desfazivel em uma linha.** Aqui o defeito era
meu, e ele e o ponto inteiro da fatia. `default_transaction_read_only = on` faz o Postgres recusar com
**25006** (`cannot execute UPDATE in a read-only transaction`) **antes** de olhar privilegio de tabela: a
mensagem prova o CINTO, e o proprio aval chama o cinto de cinto (*"quem segura e o GRANT, nao o parametro
de sessao"*; um `SET ... = off` o desfaz, e e `USERSET` — o leitor pode). O RED passou a morder nos DOIS
valores, e so a rodada 2 certifica:

- **rodada 1, cinto LIGADO**: UPDATE, DELETE, INSERT e UPDATE-pelo-ORM todos `cannot execute ... in a
  read-only transaction`;
- **rodada 2, cinto DESLIGADO pelo proprio leitor**: os quatro `permission denied for table
  colaboradores_colaborador`. **4 de 4 pelo GRANT**, em `saas_hasner` **e** em `sombra`.

Toda tentativa tem `where/values id = -1` (0 linhas possiveis) dentro de `atomic()` com `raise` no fim — a
lei de 27/08 —, e a contagem fecha **886 antes e 886 depois** nas quatro rodadas.

**E A PRIMEIRA FORMA DO RED b ERA UMA SONDA MAL PARAMETRIZADA**, o erro que esta casa ja leu como bug do
sistema sete vezes: ela escrevia `set matricula = matricula`, e `matricula` **nao e coluna** de
`colaboradores_colaborador` nem campo do modelo. Duas das quatro tentativas voltavam `column "matricula"
does not exist` e `Colaborador has no field named 'matricula'` — negadas pela minha SONDA, nunca pelo
GRANT, e a rodada 2 saiu **VERMELHA** com `2 de 4 por PERMISSION DENIED`. A sonda passou a usar `situacao`,
que existe, preservando o valor nas duas formas (`situacao = situacao`, `F('situacao')`). A trilha guardou
as duas: sha `d4f4e79d…` (errada) e `141188fd…` (certa), `quem=fernando` nas seis linhas.

**O ARGUMENTO E DO CHAMADOR, e isso se mediu, nao se supos** (`logs/o230/red_dono_e_argv_20261009.txt`).
Como `fernando`, os quatro alvos de outro dono dao **rc 2** sem devolver um byte de conteudo — `.env`,
`logs/.env_teste`, `logs/.env_leitor` e `app/ponto/turnos.py` —, porque o veto julga o **DESCRITOR** ja
aberto, antes do `exec`. O ultimo e o caso que mostra o desenho: o `fernando` **LE** `turnos.py` pela ACL,
e mesmo assim nao o roda como sonda. E o argv estrito devolve `rc 2` em `--sombra` sem arquivo, sem
argumento, com um `--settings=` colado e com dois arquivos.

**A ACL DE `logs/` E `app/docs/` JA EXISTIA, e o aval dizia que faltava.** Medido como `fernando`, antes de
tocar em nada: **NEGADO** em `.env`, `logs/.env_teste`, `logs/.senha_teste` e `backups/`; **LE** em `logs/`,
`app/docs/`, `app/docs/LEIS.md`, `app/ponto/turnos.py` e `bin/recursos.sh`. Nao se refez o que estava
feito. Depois do primeiro uso, o env-file nasceu **600 sem entrada de ACL** (`user::rw-`, `group::---`,
`other::---`) e segue **NEGADO** ao `fernando`; a trilha `logs/sonda_leitura.log` ele **LE**, e isso e
certo — ela nao carrega segredo, carrega quem/quando/banco/sha/rc.

**O QUE ESTA PORTA ESCREVE, declarado porque e escrita no banco do CLIENTE** (LEI-AKITA 7): cada chamada de
producao reafirma 5 linhas de catalogo em `saas_hasner` (`GRANT CONNECT`, `GRANT USAGE` x2 e
`ALTER DEFAULT PRIVILEGES` x4) e um `ALTER ROLE leitor ... PASSWORD` no cluster. O `GRANT SELECT ON ALL
TABLES` **so** sai quando o catalogo responde que falta — em `saas_hasner` a resposta e 0 e nao se escreve;
na `sombra` recem-restaurada e 111, e e para isso que o ponto de uso existe: `bin/sombra.sh:221-225` a
derruba toda noite com `pg_restore --no-privileges` e leva todo GRANT com ela.

`LEI-AKITA: origem=bin/sonda_leitura.sh (porta unica de leitura do dev), testemunha=catalogo do Postgres
(has_table_privilege / permission denied), RED=logs/o230/red_{a,b,e,dono_e_argv}_20261009.txt,
quem-mais-le=0 (porta nova, nenhum chamador de produto), juizes novos=0`

## R1-RESIDUO-DO-INTERVALO — **EM CURSO** (09/10 09:xx; complemento da O73, sem id novo)

**LEI RESPONDIDA às 13:3x — `L-115`, e a resposta não era nenhuma das minhas duas candidatas.**
A pergunta subiu aqui às 09:xx com número e a esteira seguiu (PAREI-DE-LEI-NAO-DEVOLVE-TURNO): *a L-084
diz que 180 min nas DUAS pontas é o limite para o cadastro ainda DESCREVER o dia; o raio de atribuição
da ata (`tol_min`) é 90. **Qual dos dois governa uma SAÍDA a 120 min do marco?*** A resposta dele:
**nenhum dos dois se move.** *"O raio de 90 e a L-084 ficam como estão. Dia com par de pausa FECHADO e
uma última batida depois dele: essa batida é a SAÍDA do turno pela posição, qualquer que seja o tipo
gravado; o que passa do marco é ponta de HE pela L-097. Altera a borda da BUG-144 só nesse caso; 'última
E solta' sem pausa fechada antes segue como está."* O que a **pausa fechada** faz na lei é o que eu não
tinha: ela é a testemunha de que a pessoa **saiu e voltou**, e depois disso a última batida não pode ser
uma entrada — então a POSIÇÃO responde onde o RAIO se cala, sem alargar envelope nenhum e sem juiz novo.
E o excedente não vira trabalho silencioso: vira **ponta de HE pela L-097**, lei que já existia para o
minuto fora do marco. Escrita em `docs/LEIS.md` como **L-115**, ESTADO `SO-NO-PAPEL`: os casos pela
REGRA entram na bateria **antes** do código (L-114), na fatia seguinte desta mesma obra. Medido:
**33 dia-colab / 103,1 h** das 500,7 h da competencia 10/2026 (balde A). Nesses dias a ata acende 3 dos
4 marcos, da o marco de SAIDA (`hf`) a batida do MEIO e a ultima batida fica **ORFA** por estar fora do
envelope de 90 min (a: 19:00 a 120 min de `hf` 17:00; b: 16:53 a 93 min de `hf` 15:20) — conservada pela
S133, invisivel para todo leitor de marco. As duas curas candidatas batem em lei declarada: alargar o
`tol_min` muda TODA ata da frota, e ler a `E` final como saida contradiz a borda da BUG-144
(`ponto/turnos.py:394`, *"a ultima E solta nao conta"*) = juiz novo. Prova e aritmetica em
`logs/r1/ata_a_b.out` e `logs/r1/MECANISMO.md`. **O balde A deixou de estar travado**: ele é a fatia
seguinte, com os casos na bateria primeiro; o balde P é o que pousa agora.

ORDEM-VIVA-TOPO passou a `O73`. O O214 fica ABERTO so pelo smoke dele (AVAIS #7) — os quatro itens
pousaram (`3c610491`..`6a259f0b`, no ar). O handoff ainda imprime "item EM CURSO: O214" porque
`_proximo_da_fila` le a ORDEM DA TABELA e o marcador e a autoridade desde a O158: divergencia
LEGITIMA, declarada no selo (`bin/tests/test_hook_nao_cobra_congelado.sh` diz as duas). A cura dessa
segunda voz e instrumento (`bin/handoff_sessao.sh`), e pousa sozinha pela L-105.

**MEDIDO ANTES DE CURAR, como o aval exige, e as partes somam 126 com ZERO "outro"**
(`logs/r1/MECANISMO.md`, `logs/r1/censo_126.out`, pela funcao REAL `turnos_do_colab` +
`_pares_marcados` + `realizado_do_dia`, so leitura):
`A-pausa-meio-absorvida 33 · G-celula-sem-marco 22 · P-par-interior-rejeitado 17 · D-par-sub-piso 15 ·
X-turno-aberto-sem-pausa 14 · M-cruza-meia-noite 10 · T-tipo-nao-alterna 9 · Z-zero-batida 3 ·
N-pareador-sem-defeito 3 = 126` (500,7 h, 46 colabs, competencia 10/2026).

**O MECANISMO NAO E UM** — a primeira redacao deste achado dizia que era, e estava ERRADA. Sao tres
sitios distintos, cada um a ORIGEM do seu balde: `_pares_marcados` (`ponto/turnos.py:513`) nega um par
cujas DUAS pontas vieram marcadas, porque testa tipo GRAVADO `S`->`E` (balde P, caso c col114: 492
contra 434); a porta PAUSA-DESLOCADA (`:966`) nao abre para a pausa 2 h fora do envelope de 90 min
porque exige vizinho de tipo GRAVADO `E` (balde A, caso b col941: 294 contra 467, 173 min num turno
solitario); e o desempate S158 entrega a batida a BORDA **por 14 segundos** (caso a col263: 370 contra
551, 180 min perdidos). O sitio NUNCA e o tipo gravado — batida gravada e fato, e corrigir tipo por
script e PROIBIDO pelo aval e pela ZONA INVIOLAVEL. O sitio e quem LE o tipo como se fosse papel.

**Tres dos 126 sao erro do TERMOMETRO, nao da casa**, e isso e a lei de 09/10 se provando no primeiro
uso: col949 29/09 pareia `00:28`->`06:00` = **332 min fechados e certos**, e a linha do oraculo traz
`piso 0`, `teto 0`, coluna `batidas` VAZIA e `dif=-332` — ele atribuiu a noite a outro dia. O 126 e
termometro; a certificacao e a BATERIA.

**FATO, sem reescrever o RED dele**: os casos d e e do aval nomeiam 2 e 4 batidas; prod tem **3 e 5**,
com um par sub-piso (1m28s em d, 9 min em e). Em `e` a alternancia pura da 296, nao 480 — o 480 so sai
se o par de 9 min for lido como marcacao duplicada, e disso NAO ha lei escrita. Os dois cenarios entram
na bateria nas DUAS formas (O218).

### A PROVA DEPOIS — NO AR as 13:49, e os quatro casos dao o numero DECLARADO em PROD

PROVA: commit `185b9af0` no ar as 13:49 (3 rotas 200/302/200, `importerror_500=0`); GRAVADO da comp 09
medido em prod antes e depois, **607 colabs `952550a8…`** e exportacoes vigentes `361d0f96…` /
`5c503b95…` / `84c78cd0…` (combinado `5d017af6…`), **identicos**; smoke da funcao real **4/4**
(`logs/r1/smoke_prod_20261009.txt`).

Exigencia 4 da DINHEIRO-EM-COMPETENCIA-ABERTA, fechada com os numeros:

- **commit `185b9af0`**, deploy `bin/deploy.sh --sem-migrate` **no mesmo ato** (L-107): migrations
  pendentes **0**, portao da sombra `dia=20261009 status=OK tipo=completa diverge=0 erros=0`, prova de
  casca **16 estaticos / 5 paginas / 608 rotas em 2 urlconf**, tres cascas recarregadas JUNTAS, tres
  rotas provadas (core `/health/` 200, ui `/colaboradores/` 302, mensageria `/health/` 200), selo
  BUG 128 verde, **`importerror_500=0`** na janela 12:49–13:49.
- **SMOKE EM PROD chamando a funcao REAL** `ponto.turnos.realizado_do_dia` -- nao uma sonda que
  reconstroi a chamada (`logs/r1/smoke_prod_20261009.txt`): **4 de 4** com o numero declarado na
  bateria -- col438 11/09 **360** (o achado do papel `X`, turno aberto), col518 05/09 **353** (o par
  guloso), col114 22/09 **434** (os 16 dias em que a ata chama a abertura de SAIDA), col853 28/08
  **135** (a ata deu `S` a abertura -> ADMITE). O caminho velho pagava 243 e 391 nos dois primeiros.
- **COMP 09 EXPORTADA INTACTA, hash ANTES e DEPOIS**: as **3 exportacoes vigentes** seguem
  `361d0f96…` (emp2, 210 linhas), `5c503b95…` (emp3, 86) e `84c78cd0…` (emp4, 9), **hash combinado
  `5d017af6…` identico** antes (13:44) e depois (13:49) do deploy; o `FechamentoMensal` da 09 tambem:
  **607 colabs, `952550a8…`** nas duas medicoes. As 5 linhas de registro invalidadas seguem
  invalidadas, nenhuma apagada.
- **A 10/2026 segue andando por conta propria**, como a dupla medicao de 13:43/13:48 ja mostrara:
  `cfe10382…` as 13:48 e `910d2ba3…` as 13:49. E por isso que a reversao dela se tira no instante, e
  por isso que ela e a competencia onde o numero se move.

### O DIFF DE FROTA, PUBLICADO ANTES DO APPLY (09/10 12:29, na sombra — IMPACTO, nao prova)

DINHEIRO-EM-COMPETENCIA-ABERTA pede os quatro, e o primeiro e este. Medido na SOMBRA pelas funcoes
REAIS (`turnos_do_colab` + `_pares_marcados` + `realizado_dos_turnos`), janela **2026-08-21..2026-10-08**,
**551 colaboradores / 26.999 dia-colab**, arvore base `ponto/turnos.py` md5 `ed186512…` (HEAD `6a259f0b`)
contra `f4058711…` (veto v2). Censo inteiro em `logs/r1/censo_impacto.out`:

**TOTAL −1.162 min (−19,37 h) em 55 dia-colab, 20 colaboradores.** Por competencia: **09/2026 29
dia-colab, −447 min**; **10/2026 26 dia-colab, −715 min**. **0 linha nova** (a v2 e estritamente mais
restritiva que a v1: ela admite um SUBCONJUNTO, entao o universo so pode encolher) e **0 delta deslocado**
nas que ficam — as que ficam mantem o numero digito a digito. Os pares latentes de turno aberto, que a v1
criava, foram de **14 a 0**.

**AS SEIS SAIDAS, nomeadas** (a predicao escrita ANTES, `logs/r1/predicao_censo_v2.txt`, afirmava DUAS
e o censo achou SEIS — a predicao esta FALSIFICADA no numero e e por isso que ela nao certifica nada):
col438 **−117** · col518 **+38** · col921 **−60** · col890 **+1** · col923 12/09 **+1** · col923 19/09
**+1**. As quatro ultimas sao as bordas que o arredondamento move; as duas primeiras sao os dois achados
de `logs/r1/achado_papel_x.md`, que a v2 nasceu para curar.

**O que o DIFF NAO e**: ele nao certifica. L-114 (abaixo) poe o nome em cada papel — o certificado e a
BATERIA, o DIFF e IMPACTO, o oraculo e TERMOMETRO.

### O CERTIFICADO: a BATERIA, com os quatro dias medidos escritos como CASO

`ponto/tests/test_r1_pausa_de_autoridade_mista.py` — **3 cenarios / 14 testes**, cada caso com cadastro,
batidas, ata e **a resposta da regra escrita ANTES do codigo** (L-110). O que ela cobra, e nenhuma delas
e geometria pura: **o MINUTO do dia** ao lado do par (`realizado_dos_turnos`, o leitor que PAGA) —
col438 11/09 vale **(360, 60)** onde a primeira forma pagava 243; col518 05/09 vale **(353, 60)** onde a
primeira forma pagava 391. As duas assercoes passaram de primeira, o que e a predicao do minuto se
confirmando. E o **contra-exemplo adversarial** (LEI-AKITA 5) guarda a ata FUTURA: `['E','S','Xi','Xi','S']`
tem de dar o par DECLARADO `14:00→15:00`, e da — **nao por regra nova**, mas pela passada `_usados` da
R2b (27/09, caso col843), que casa o par `_intra_declarado` ANTES do laco guloso. Se esse teste ficar
vermelho algum dia, a cura e ESTENDER aquela passada, nunca afrouxar o veto.

O selo do comportamento velho **nao foi apagado**: a assercao dele se inverteu e ele passou a morder a
VOLTA do defeito (`test_MORDE_ata_MUDA_o_MARCO_SOZINHO_nao_abre_pausa`).

**VEREDITO DAS SUITES** (`logs/r1/suite_veredito_20261009.txt`, os tres lidos pelo PAR que a secao 3 do
CLAUDE.md exige — `^(OK|FAILED)( |$)` **mais** `^Ran N tests`): modulo **Ran 14 / OK**; familia ponto
inteira **Ran 3163 / OK (skipped=7)**; vizinhos **Ran 7018 / OK (skipped=35)**. A nota no pe daquele
arquivo mostra a linha de PROSA de log (`OK — nenhuma divergencia em 2026-10-09`) que casaria o padrao
sozinha: foi ela que deu uma suite por verde em 08/10.

### L-114 CERTIFICADO x IMPACTO x TERMOMETRO — a lei entra no codigo do placar, sem obra nova

O corte das 09:0x pediu `L-NNN` em `LEIS.md` **e** mudanca em `core/placar_estrutural.py` no marco da
obra em curso. Os dois estao neste commit. O placar ganhou **papel** por resultado:

- **PRINCIPAL** — a **BATERIA** (o certificado, linha nova: 1 de 7 familias com bateria declarada, e a
  de turno/marcos tem **15 cenarios / 48 testes** verdes somando os dois modulos), a **SOMA** (linha nova:
  *as partes somam o total*, **invariante** com meta ZERO em producao — 197+29+208=434 na 09 e 95+15+95=205
  na 10), mais R2..R6 como estavam;
- **TERMOMETRO** — **so o R1**, a comparacao com o oraculo, porque so ela e pergunta de VALOR.

A **correcao 1** dele e exatamente o que eu ia escrever errado: eu desceria R1 **e** R4, lendo *"os numeros
de frota de R1 e R4"* ao pe da letra. R4 (*todo leitor da o mesmo numero*), R5 (idempotencia) e a soma das
partes sao **invariantes**: valem sobre dado sujo tambem, e por isso continuam no placar principal com meta
zero em prod. A **correcao 2** poe cadencia: `CADENCIA_TERMOMETRO_DIAS = 7`, e **bateria verde com
termometro vencido = AMARELO**. HOJE O PLACAR SAI AMARELO, e isso e o primeiro uso da lei contra a propria
casa: a ultima medicao do R1 e de **02/10 23:15** (`logs/e6_cauda2c/r1_dono_09_e_10.txt`), 7 dias — a semana
correu. O `e6_oraculo` da competencia aberta e o passo logo depois do pouso, pela propria cadencia.

O modulo **nao le relogio**: `placar(hoje)` recebe o dia de quem publica e, sem ele, o vencimento e `None`
— *nao perguntado* —, nunca `False`. Ausencia de sinal lida como sinal bom e o defeito que custou quatro
selos vazios em 01/09. **RED evidenciado** em `logs/r1/red_selo_l114_20261009.txt`: trocar o `>=` da borda
por `>` derruba 3 dos 9 casos de `core/tests/test_l114_placar_papel_e_termometro.py` — a borda, a cor do
veredito e a linha do ESTADO. A secao TERMOMETRO do ESTADO e render, mora em `bin/gerar_estado.py` e pousa
em ato PROPRIO de instrumento, depois do produto (L-105).

**E HOUVE UM SEGUNDO RED, que nao e meu: o selo da casa mordeu a primeira forma deste placar.** Eu havia
escrito `medido_em: '2026-10-02'` LITERAL no modulo, e a suite `core` inteira voltou
**`FAILED (failures=2)`** em `core/tests/test_selo_sem_data_cravada.py` (corte de 23/09) --
`core/placar_estrutural.py:107`, classe **DECIDE**, **-7 dias**: *"migre por ESTADO, nao empurre a data"*.
Ele estava certo e e a mesma classe do P0 de 21/09 (`api/credencial.py`, 304 pessoas sem autenticar as
00:00): o arquivo compara com `hoje`, entao a data decide, e **a lei que eu estava escrevendo seria a
primeira a envelhecer sozinha**. A cura foi na ORIGEM, nao em `DECLARADAS` -- que e a porta para data que
NAO decide: quando a medicao aconteceu e **fato do mundo**, entao o R1 declara `fonte_do_medido`
(`logs/e6_cauda2c/r1_dono_*.txt`, caminho e nao data), `placar(hoje, medido_em)` recebe a data de quem
publica -- que e quem faz o I/O --, e **sem data o veredito e AMARELO, nunca VERDE**. Depois da cura:
**Ran 16 / OK** nos dois modulos, e o censo da casa da **0** data em `core/placar_estrutural.py`, **0**
DECIDE no repo e **0** vencendo. Os dois REDs estao em `logs/r1/red_selo_l114_20261009.txt`.

### O ANTES DO GRAVADO, MEDIDO EM PROD — e a 09 nao se move em silencio

`logs/r1/gravado_quem_escreve.md` (56 linhas) e `logs/r1/hash_antes_deploy_20261009_1249.txt`. A
competencia **09/2026, EXPORTADA, esta INTACTA** em TRES medicoes da mesma funcao `_h` ao longo de ~19 h
(08/10 18:09, 08/10 19:11 e 09/10 12:49): **607 linhas de `FechamentoMensal`, `4da388d4…`** nas tres.
As **exportacoes** vigentes da 09 sao **TRES, uma por empresa** -- emp2 `361d0f96…` 210 linhas, emp3
`5c503b95…` 86, emp4 `84c78cd0…` 9, **305 linhas de TXT**, as tres com `invalidada_em=None` --, remedidas
as 13:44 e **identicas** as de 08/10 (O211b, `RELATO.md` mais abaixo). As outras 5 linhas de registro de
09/2026 estao INVALIDADAS pela porta, nunca apagadas, e e assim que a memoria de versao funciona.
*(Eu havia escrito aqui "21 exportacoes vigentes": era numero de outra conta. O registro da comp 09 tem
**8 linhas**, **3 vigentes**; medido agora, pk a pk.)* A UNICA linha de comp 09 gravada no meio disso (09/10
07:48:14) **moveu ZERO dos 24 campos de VALOR**, e o escritor tem NOME: `invalidar_previsto`
(`ponto/services/fechamento.py:965`, chamado so de `escala/signals.py:132`), que escreve `previsto_em` e
`atualizado_em` — e `previsto_em` nao esta em `CAMPOS`.

**A guarda e ESTRUTURAL, nao conduta**: `ponto/services/fechamento.py:72-82` chama
`empresas_exportadas_no_escopo(...)` e LEVANTA sem `permitir_exportada=True` + motivo escrito, com trilha
nominal — e as tres empresas tem exportacao vigente de 09/2026. Entao nenhum caminho recalcula a 09 em
silencio, e o deploy nao pode mover dinheiro exportado. A testemunha de quem pode escrever
`FechamentoMensal` nao e um grep meu: e `folha/tests/test_chokepoint_folha_gate.py`, familia
`folha/export`, **ALLOWLIST VAZIA**, por AST — porta unica `ponto/services/fechamento.py`.

**A REVERSAO FOI TIRADA AS 13:48:10, imediatamente antes do commit e do deploy** (exigencia 2 da
DINHEIRO-EM-COMPETENCIA-ABERTA): `logs/r1/reversao_comp10_20261009_1348.json` guarda o **GRAVADO** campo
a campo, pelos **24 campos de VALOR** do `CAMPOS` canonico (`ponto/management/commands/aplicar_09_corte_b.py:67`),
das DUAS competencias -- **10/2026: 587 colabs, `cfe10382…`** e **09/2026: 607 colabs, `952550a8…`** --,
mais os 8 registros de `ExportacaoDominio` da 09 com hash e estado.

**E A PROPRIA RETIRADA DUAS VEZES MEDIU A DIFERENCA ENTRE AS DUAS COMPETENCIAS**, sem sonda nova: a
mesma funcao rodou as **13:43:00** e as **13:48:10**, cinco minutos de intervalo, com a esteira so
escrevendo documento. A **10/2026 mudou de hash** (`7ea5dd89…` -> `cfe10382…`) e a **09/2026 NAO**
(`952550a8…` nas duas). E por isso que o "antes" da 10 nao se reusa e tem de ser tirado no instante do
apply, e e por isso que o "antes" da 09 vale desde 08/10: uma se move por conta propria, a outra esta
congelada pela L-092 e pela guarda estrutural. *(O arquivo das 13:43 foi REMOVIDO no ato: dois
"antes" da mesma competencia seriam dois escritores do mesmo estado, e o vigente e o de 13:48. O que
ele media -- o hash -- esta aqui.)* A reversao se executa **pela PORTA**
(`ponto/services/fechamento.py`), nunca por `UPDATE` cru, e a 09 ainda exigiria `permitir_exportada=True`
com motivo escrito: o arquivo e o VALOR de volta, nao uma licenca.

**A 10/2026 se move por conta propria**, e por isso o "antes" dela nao se reusa: **tres hashes diferentes**
nas tres medicoes (`127d6a83…` → `d6422334…` → `1bb6ec36…`, 587 linhas nas tres), **293 linhas** reescritas
desde 08/10 19:11, **81 na hora corrente**. `recalcular_fechamento` **nao tem cron** por desenho
(`config/crons.py:902`: *"quem decide QUANDO uma competencia se recalcula e o DP"*) — entao ela nao anda no
instante do deploy, e sim no proximo ato pela porta. Consequencia pratica, e e ela que manda no passo
seguinte: **o snapshot de reversao da 10 se tira NO INSTANTE do apply**, sobre as 587 linhas x 24 campos.
Nao atribuo as 293 linhas a um dos dois movedores possiveis — isso exigiria trilha por linha, que o modelo
nao guarda.

### O RELATO DESTRAVOU (aval dele de 08/10 19:2x)

`bin/relato_afirma_com_prova.py app/docs/RELATO.md` -> **rc=0, 0 afirmacao sem prova**. A linha que o
retinha era o titulo da O146 afirmando ato sem `PROVA:` ao lado, e a PROVA esta no lugar
(`docs/RELATO.md:480`: 353 `TipoEscala`, 0 com extra declarada, 124.358 celulas, 0 com
`dna['extra_declarada']`, `dna_versao` 1: 20.785 / 2: 103.573). Com rc=0 o proximo ciclo publica o RELATO
e a faixa **RELATO retido** do topo do ESTADO nao nasce — ela e escrita por `bin/relato.sh:84` **so**
quando o portao recusa.

## O214 ITEM 4 — **O DIA TEM DONO, E A DIFERENCA TEM DONO** (09/10 07:xx→08:xx, ITEM 4 FECHADO)

A ordem era literal: *"os dias acima do limite ficam decidiveis na Gestao de HE e o autorizado sai como
linha na pauta do TXT da 09 ja aberta, com os dois numeros; a 09 nao se recalcula por este ato"*. Entrou
assim, e o que ela obrigou a desenhar foi uma distincao que o sistema nao tinha palavra para dizer.

**`estado` x `dono`, e nao e um terceiro estado.** `estado` responde *o que foi decidido*
(`sem_decisao|autorizado|nao`); `dono` responde *quem decide*. Um dia `sem_decisao` do **SISTEMA** -- ponta
abaixo do `Empresa.limite_decisao_he_min` -- nunca esteve na fila do admin: quem o recusa e o
`recusar_ponta_pequena` do item 1. Soma-lo ao que "espera o admin" e o contador discordando do universo
(LEI-AKITA 8). Entao `gestao_he.DONOS = ('admin', 'sistema')` e `enriquecer` carimba o dono de cada dia.

**ZERO JUIZ NOVO, e o censo esta fechado.** A pergunta "de quem e este dia" tem **UMA** comparacao:
`ponto/portas/he.py::acima_do_limite(minutos, limite)` -> `int(minutos) > int(limite)`. Quem a CHAMA sao
exatamente tres: `ponto/services/gestao_he.py` (o dono na tela), `folha/porta_export.py` (a trava do item 3)
e a propria porta, em **duas guardas espelhadas** -- `:288` o sistema recusa o dia ACIMA, `:375` o admin
recusa o dia ABAIXO. `ponto/services/autorizacao_he_periodo.py` **nao decide**: le o cadastro e entrega o
`limite` a porta. O selo cobra isso por **AST** (2 chamadas na porta, `assertNotIn` no servico).

**`limite` virou OBRIGATORIO em `enriquecer`, sem default.** Default aqui seria o leitor inventando o
cadastro de quem esqueceu de perguntar -- e quem tem a empresa na mao e o chamador. Os cinco chamadores
migraram (2 views, 3 selos); os selos que medem OUTRA regra passaram `limite=0`, que e o estado honesto da
empresa sem cadastro: com zero nenhum dia e do sistema, e a guarda do dono nao morde a regra deles.

**`so_acima` nasceu ASSIMETRICO, tambem sem default.** `True` no ato de PERIODO (autorizar PAGA, entao so o
universo do admin entra); `False` no `recusar_em_lote` (ciencia nao move nada, entao os dois donos entram).
Um default deixaria um chamador distraido autorizar o dia que o sistema ja recusou.

**OS DOIS NUMEROS, cada um da SUA autoridade** (`_linha_para_o_dp`, marcador
`[HE-AUTORIZADA-EM-EXPORTADA]`). O que foi **PAGO** sai da LAVRATURA (`dia_pago.soma_do_periodo` +
`he_lavrada_do_colab`); o que **PASSA A VALER** sai do MOTOR (o mesmo `depois` que o admin viu na previa).
Misturar as fontes seria a testemunha recalculando (LEI-AKITA 2). **Silencio chega como ROTULO, nunca como
zero**: sem lavratura a linha diz *"sem apuracao ainda"*, porque "0,00 h" ali seria a AFIRMACAO de que o
colaborador nao tem HE. `he_lavrada_do_colab` nasceu em `dia_pago.py` por ter ganhado o SEGUNDO leitor
(L-111 ao contrario: o rotulo estava inline no laco de `enriquecer` e a linha do DP precisa do mesmo).

**UMA CABECA VIVA por empresa+competencia.** O segundo ato RESPONDE o primeiro (`pai=_cab`) em vez de abrir
pauta nova -- a ancora e `'%s:%04d-%02d'`, byte a byte a mesma de
`folha/management/commands/pre_fechamento.py:86`. O marcador e proprio, e **nao** `[PRE-FECHAMENTO]`: aquela
cabeca o cron reescreve toda noite.

**A 09 NAO SE RECALCULA, e isso se le no codigo, nao na promessa.** Nao ha `if exportada` que mude a
DECISAO: `decidir_he` grava na exportada do mesmo jeito que na aberta, e quem recusa mover o GRAVADO e o
`recalcular_por_evento` (L-092), com a lapide que ja estava la. O unico `if r['exportada']` do item 4
**escreve a linha** -- ele nao faz a decisao ser outra, faz a **diferenca ter dono** (REGEN-EM-EXPORTADA).

**A PREVIA NAO FALA COM O DP.** A pauta mora DENTRO do `_ato`, e `previa` e o `_ato` dentro de
`transaction.atomic()` + rollback: a linha morre com a transacao, por construcao -- nao por um `if previa`
que alguem esquece. Conferido antes de aplicar: `pautas.services.escrever` e **so banco** (`Pauta.objects
.create` + trilha + `lavrar`), sem FCM, sem arquivo -- entao o rollback e completo.

**E se a pauta falhar, o ato INTEIRO cai** -- inclusive as `DecisaoHE`. Esta e a escolha, por
CURA-MAIS-RESTRITIVA: autorizacao em competencia exportada **sem** a linha do DP e exatamente *"diferenca sem
dono"*, que e o que a REGEN-EM-EXPORTADA proibe.

**O SELO ESTAVA ERRADO E O CODIGO O CORRIGIU.** A primeira versao de `test_quem_DECIDE_o_dono_do_dia_CHAMA_o
_juiz_da_porta` exigia que `autorizacao_he_periodo` nomeasse `acima_do_limite` -- e o desenho certo nao
nomeia, porque esse servico nao decide. Satisfazer o selo seria o primeiro passo para uma segunda decisao.
O selo foi reescrito; o porque ficou no docstring dele.

**O RED que o banco de teste tinha e prod nao.** Os vizinhos deram **1 error** em
`test_o214_item2_autorizar_periodo.ExportadaTest`: `PautaRecusada: Voce nao pertence ao departamento ti.` --
`autor_sistema('ti')` recua para o primeiro superuser ATIVO, prod tem tres, banco virgem tem zero. Curado na
**fixture** (a forma que a casa ja usa, `test_pautas_do_esmeril.py:14`), nao com fallback: o ato cair sem o
superuser esta CERTO pelo paragrafo acima.

**O CARRIER CORTAVA EM SILENCIO, E O CORTE COMIA O PORQUE** (LEI-AKITA 6 -- bug provado no meio da fatia,
curado na hora; o bullet da lista NOMEADO, logo abaixo, dizia *"NAO CURADO"* e durou o tempo de medir).
`pautas/models.py:89` da a `Pauta.texto` **500** caracteres e `pautas/services.py:128` faz `texto[:500]`.
A linha do DP no pior caso mede **678** -- e os 178 que sobram sao EXATAMENTE a ultima linha: o texto
gravado terminava em `'Motivo '`, o rotulo cortado no meio da palavra. O DP receberia um pedido de
retificacao de folha **sem o porque**, que e justamente a trilha que a REGEN-EM-EXPORTADA exige por escrito.
RED literal contra o HEAD: `Ran 7 tests` / `FAILED (failures=1)`, com o `'Motivo '` na mensagem.

**A CURA E ORCAMENTO, E A ORDEM DO QUE CEDE E RECUPERABILIDADE -- nao gosto.** `escrever` ja tem o idioma
do orcamento para `ancora_tipo=='dia'` (`pautas/services.py:122`), e seguir o idioma existente e a
LEI-AKITA 4; alargar a coluna seria curar o carrier por causa de UM leitor. Quem cede, cede na ordem do que
o sistema **ainda sabe dizer depois**: **(1)** o *motivo*, porque `DecisaoHE.motivo` e `TextField` gravado
por DIA pela porta e o pedaco que fica aponta para la; **(2)** o *autor*, porque `DecisaoHE.decidida_por` e
FK e responde "quem assina" dia a dia; **(3)** a lista das outras rubricas por ULTIMO, e so para um
CONTADOR, nunca para o silencio. O que **nunca** cede: os dois numeros com as suas fontes, HE50/HE100, o
colab, a competencia, a contagem de dias e a linha da L-092. Se nem a forma minima couber, o ato **falha
INTEIRO** -- `DecisaoHERecusada` dentro do `atomic()` do `_ato` --, porque encolher abaixo disso e entregar
ao DP uma trilha que nao ensina ninguem (CURA-MAIS-RESTRITIVA).

**O teto se LE do campo**, `Pauta._meta.get_field('texto').max_length`: um `500` literal ali seria a
TERCEIRA copia do tamanho da coluna (LEI-AKITA 2). Medido em cinco formas de entrada --
**413 / 500 / 496 / 496 / 344**, todas `<= 500`, nenhuma silenciosa, todas com o rotulo do motivo, a linha da
L-092 e a informacao das movidas presentes. O ramo do `raise` e **inalcancavel com o cadastro de hoje**
(maior `username` ativo = **23** caracteres de 150 possiveis, em **727** usuarios, p95 **11**) e existe para
o dia em que alguem alargar o nome ou a frase: nesse dia ele diz o NUMERO em vez de cortar. `MIN_MOTIVO`
saiu de dentro de `_exigir_motivo` para `ponto/portas/he.py` porque o `>= 10` ganhou o **terceiro** leitor --
o piso do fragmento que ainda ensina alguem --, e a docstring da propria funcao previa a divergencia de duas
copias.

**O SEGUNDO DEFEITO ERA MEU, E O PORTAO O PEGOU.** O censo de chamadores de `enriquecer` que eu declarei
fechado tinha um fora: `chamados/tests/test_atalho_he_na_central.py` chamava sem o `limite` agora
obrigatorio, e o push foi recusado -- `ERROR: test_RED_o_numero_do_atalho_E_o_total_sem_decisao_da_tela`
sobre `Ran 10160 tests`. Curado com `limite=0`, que e o estado honesto para um selo que mede OUTRA regra:
com zero nenhum dia e do SISTEMA, a guarda do dono nao morde e a Central segue medindo a MESMA foto da tela.
Passar 15 ali faria o selo medir o recorte do dono, que e pergunta de outro selo. A licao esta no censo --
eu varri a familia do SITIO (`ponto/`) e o chamador morava na familia do CHAMADOR (`chamados/`).

**PROVA.** Selos do item 4: `Ran 12 tests` / **OK** (5 falhas + 3 errors no caminho, todos evidenciados).
Vizinhos `ponto folha pautas colaboradores`: `Ran 4240 tests` / 1 error -> fixture -> os quatro modulos do
O214 `Ran 82 tests` / **OK**. `ruff check` nos 9 arquivos: **All checks passed**. Juizes novos: **0**.
Rubrica nova: **0**. Escritor novo de `DecisaoHE`: **0** (segue `decidir_he`). **Da segunda janela** (as duas curas,
em copia nascida de `git show HEAD:`): RED evidenciado, depois `Ran 72 tests in 14.638s` / **OK** nos quatro
modulos (item 4 + chamados + item 2 + ponta pequena), `ruff check` **All checks passed** nos 4 arquivos, e os
vizinhos pela familia do CHAMADOR -- `ponto pautas chamados folha` -- `Ran 5988 tests in 586.954s` / **OK** (skipped=9).

**NOMEADO, NAO CURADO** (vai para o BACKLOG, nao para esta fatia):
- o filtro `?dono=` existe no servidor e **nao tem controle no markup** -- quem o liga e a **fatia 2** da
  Gestao de HE, que ele proibiu construir agora. Ate la e chave com leitor e sem gesto, declarada aqui para
  nao ser lida como chave sem leitor (LEI-AKITA 12);
- `int(getattr(empresa, 'limite_decisao_he_min', 0) or 0)` aparece **5 vezes**; candidato a unificacao;
- `_ato` devolve o `diff` por rubrica como RETORNO e **nao o persiste em lugar nenhum** (`_gravar` guarda
  estado/minutos/motivo). E por isso que a lista das outras rubricas e a unica das tres partes que cede para
  um contador: o motivo e o autor se recuperam na `DecisaoHE` do dia, o diff nao se recupera de ninguem;
- `pautas/services.py` tem o **500 literal** em `:122` e `:128` -- segunda e terceira copia do tamanho da
  coluna. A linha do DP ja le o teto do campo; o carrier ainda nao;
- os **tres campos de cadastro de HE** (`limite_decisao_he_min` e os dois da janela) **nao tem controle de UI
  nenhum**: nao existe `EmpresaForm` em lugar algum do repo. Hoje se cadastram por shell, o que e o pior tipo
  de cadastro pela LEI-AKITA 12 -- tem leitor, nao tem gesto;
- as duas metades de `UmJuizDoDonoDoDiaTest` que varrem `G` e `porta_export` medem por **TEXTO**, nao por
  AST; o resto do selo ja e AST (memoria: selo estrutural varre AST, nao texto);
- a ancora de competencia tem uma **terceira grafia**, sem zero a esquerda, em
  `chamados/services/competencia_trancada.py:205`. Pre-existente; nomeada, nao tocada.

**LEI-AKITA:** origem=`portas/he.py::autorizar_em_lote` (guarda espelho) + `autorizacao_he_periodo::_ato`
(linha DP), testemunha=`acima_do_limite` (dono) / `soma_do_periodo`+`he_lavrada_do_colab` (PAGO) / `_medir`
(VALE), RED=`test_o214_item4` 12 selos (5F+3E -> OK), quem-mais-le=`enriquecer` 2 views + 3 selos,
`itens_sem_decisao` 2 views, `autorizar_em_lote` 1 servico + 3 selos, juizes novos=0.

**LEI-AKITA (orcamento da linha do DP):** origem=`autorizacao_he_periodo::_linha_para_o_dp` (o orcamento
no PRODUTOR da linha, nao no carrier) + `ponto/portas/he.py::MIN_MOTIVO` (o piso num sitio so),
testemunha=`Pauta._meta.get_field('texto').max_length` (teto lido do campo) e
`DecisaoHE.motivo`/`decidida_por` (o que o ponteiro promete),
RED=`test_RED_10_a_linha_CABE_no_teto_da_pauta_e_o_motivo_NAO_desaparece` +
`test_RED_10_MORDE_o_caso_que_CABE_chega_INTEIRO_e_sem_marca_de_corte`, quem-mais-le=`escrever` (1 carrier,
as 2 copias do 500 nomeadas no BACKLOG) e os 5 chamadores de `enriquecer` (o 5o era o que faltava),
juizes novos=**0**.

### O ITEM 3 ESTA LIGADO (09/10 08:3x) -- e o numero nao era 70, era **81**

O item 3 e literal: *"liga depois do item 2 no ar, nunca antes"*. O item 2 esta no ar desde o deploy de
`a28ca8cf`, com prova de rota nas tres cascas -- entao a condicao se cumpriu, e **ligar e cadastro com
trilha** (CLAUDE.md 7b item 1), nao apply de dinheiro: a trava BARRA o `Gerar TXT`, nao move um centavo.

**O numero se LEU da autoridade, nao se recalculou** (LEI-AKITA 2 e 8): o carimbo de HOJE da porta
(`folha/porta_export.py::CHAVE == 'porta_export_leitores'`, escopo `empresa`, `data_ref=2026-10-09`,
falhas **0**), lido pelo `detalhe` como o `conferir` o le. Os **70** do paragrafo que esta linha substitui
eram de ANTES de duas curas desta manha -- a que conta por DIA acima do limite e a da L-097, que tirava
dia de outra competencia da conta --, e envelheceram em horas:

| empresa | dias acima do limite e SEM decisao (10/2026) | limite | antes | depois |
|---|---|---|---|---|
| emp2 | **62** | 15 min | `False` | `True` |
| emp3 | **17** | 15 min | `False` | `True` |
| emp4 | **2** | 15 min | `False` | `True` |
| **total** | **81** | | | |

**A TRILHA SE CONTOU NO BANCO, nao no `print` do script.** `core/observ.py::evento` e best-effort por
`engolir`: uma `acao` torta viraria rastro e eu teria um campo virado **sem trilha** -- LEI-AKITA 7 furada
pela minha propria mao. Entao o ato conta `LogAuditoria` antes e depois: **409 -> 412, delta +3**, cada
linha com `acao=editar`, `ator=sistema:shell`, `antes={'he_pendente_trava_export': False}`,
`depois={... True}` e o motivo escrito (trilha #668545/6/7). **Idempotente**: rodar o mesmo ato de novo
leu `antes=True` nas tres e gravou **delta +0** -- um escritor, uma linha por mudanca, zero duplicada
(contrato 2 da secao 4b).

**emp1 e as tres sem carimbo ficaram DESLIGADAS, e isso e leitura, nao esquecimento**: emp1 tem carimbo
com `universo=0` e emp20/21/29 nao tem carimbo nenhum -- nao ha TXT para travar. Ligar trava onde nao ha
porta do outro lado e exatamente o que `folha/porta_export.py:571-575` chama de *"paralisia com nome de
lei"*. Uniformizar e cadastro dele, pela tela.

**QUANDO MORDE:** o `conferir` le o DETALHE do carimbo, nunca o retorno vivo do `medir` -- entao a trava
passa a barrar no **proximo carimbo**, sobre estes 81. Reversao: o mesmo ato com `LIGAR = False`
(`logs/o214/flip_trava.py`), que devolve as tres a `False` e grava mais 3 linhas de trilha.

**O QUE FICA EM PE DA O214:** o **smoke do item 2** (AVAIS #7) e o **`!` do apply do item 1**
(`recusar_ponta_pequena --apply`, ensaio de 4.692 dias / 439,6 h, hashes em `logs/o214/hash_antes_09.txt`).

## O214 — **A TRAVA DO TXT CONTAVA DIA DE OUTRA COMPETENCIA** (09/10 05:5x, LEI-AKITA 6)

**Bug PROVADO no caminho do item 4, curado na hora.** Eu fui medir, na sombra, o numero com que o cadastro
`he_pendente_trava_export` seria ligado -- a pre-condicao escrita no item 3, *"o numero da trava se le do
carimbo, depois do deploy e ANTES do flip"* -- e a PROPRIA `folha/porta_export.py::medir` respondeu um numero
que nao era da competencia.

**MEDIDO pela funcao REAL** (`logs/sombra/vazamento_janela_he_pendente.py`, saida em `logs/vazamento_janela.out`;
competencia **10/2026**, janela 21/09..20/10, limite 15 min; a sonda **particiona a saida do `medir`**, nao
replica o laco):

| empresa | `medir` em | dia-colab com pendencia | TRAVARIAM o TXT | dentro da janela | **VAZADOS** |
|---|---|---|---|---|---|
| 2 J.A | 191,4 s | 2.098 (776 dentro / 1.322 fora) | 255 | 52 | **203** (68 de ago, 135 da 09) |
| 3 JSP | 140,4 s | 949 (337 dentro / 612 fora) | 68 | 16 | **52** (17 de ago, 35 da 09) |
| 4 G3 | 100,8 s | 106 (35 dentro / 71 fora) | 3 | 2 | **1** (de ago) |
| **total** | 432,6 s | **3.153** (1.148 dentro / **2.005 fora**) | **326** | **70** | **256** |

Entao **256 dos 326** dia-colab que travariam o TXT da **10** sao dias de **outra** competencia -- **86 de
agosto** e **170 da 09, que esta EXPORTADA** -- e **2.005 de 3.153 (63,6%)** das pendencias que o portao
publica nao sao da competencia que ele mede. **O numero real da trava e 70**, nao 326 e nao os 469 da
pre-medida de 04:5x (que era teto por outro universo, e esta declarada como teto no item 3).

**A ORIGEM, e ela ja tinha lapide.** `medir` percorria `esp['dias']` **inteiro**, e `espelho_do_colab`
devolve MAIS dias do que a janela pedida (medido no col87 em 01/10: **89 dias para uma competencia de 31**).
O `apurar` do retrato lavrado curou **este mesmo** vazamento em 01/10 -- *"de 5.767 dias, 4.198 estavam FORA
de 21/09..20/10"* -- e a lapide que ele deixou nomeia, por escrito, quem ainda leria errado: *"um numero
errado gravado, que o portao `he_pendente` e o contador da Central tambem leriam"*. Era **este** portao, e
ele ficou oito dias sem migrar. Band-aid seria filtrar no leitor; a cura vai no sitio que coleta.

**E O DANO NAO E INFLACAO, E PRISAO.** `gestao_he.estado_por_dia(ids, ini, fim)` so carrega decisao **dentro**
da janela, entao a chave de um dia vazado **nunca tem estado**: a trava o conta como "sem decisao" **mesmo
depois de alguem o decidir**, e nenhum gesto do admin o solta. Hoje sao **0 de 256** -- ninguem decidiu dia de
fora da janela ainda --, e **o item 4 e exatamente o que criaria os primeiros**, porque ele torna decidiveis
os dias da 09 (170 dos 256). A cura e, portanto, **pre-requisito do item 4**, nao so do flip.

**NAO HOUVE DANO EM PROD:** `he_pendente_trava_export` esta **False** nas quatro empresas, entao `falhas` nao
somava a lista e nenhum TXT foi barrado. O que o vazamento contaminava era o **numero publicado no carimbo** --
e era com ele que o flip ia ser decidido.

**TRES FORMAS DA MESMA PERGUNTA DENTRO DE UMA FUNCAO, agora UMA** (LEI-AKITA 2). "Este dia e da competencia?"
era respondida em `medir` de tres jeitos: o laco do `he_pendente` (**nenhum** -- o furo), a soma do `dif_topo`
comparando por **TEXTO** (`str(ini) <= str(d['data'])[:10] <= str(fim)`) e o `apurar` do modulo vizinho
comparando por **DATA**. A comparacao por texto e a que a lapide de `data_do_dia` proibe com nome: *"texto
compara certo em ISO e erra em qualquer outro formato, e o dia em que alguem mudar o formato o filtro passa a
aceitar tudo em silencio"*. As tres passam a chamar **`ponto/services/he_pendente_lavrado.py::data_do_dia`**,
que era `_data_do_dia` e ficou **publica porque ganhou o segundo leitor** -- o mesmo motivo pelo qual
`minutos_fora_do_dia` ficou publica no item 3. **Zero juiz novo** (o O214 proibe): a regra da janela e a de
`janela_fechamento`, e quem a aplica e uma funcao que ja existia.

**A COLETA VIROU FUNCAO PURA E NOMEADA**: `folha/porta_export.py::pendencias_he_da_janela(dias, ini, fim,
colab_id)`, chamada por `medir` em uma linha. Ela nao mudou a **FORMA** da entrada -- segue **por ponta**, com
`sentido`, `minutos_fora` da ponta e `minutos_do_dia` do dia (selo do B1, 30/09) --, e a razao de ser funcao
e o caso da **borda**: dentro de `medir` o dia 21 e o dia 20 so se exercitam com um colaborador que
`classificar_export` diga que ENTRA no TXT, o que custa codigo do Dominio, fechamento, celula e catalogo de
rubricas. Fora dela, o caso morde em 17 ms.

**RED PRIMEIRO, evidenciado** (`logs/o214item4/red_vazamento.out`): `Ran 8 tests` / **`FAILED (failures=6)`**
contra uma copia de HEAD com o laco de HEAD **sob o nome novo** -- isto e, o defeito sob teste, nao um
`ImportError`. Vermelhos: **o** (dia de agosto entra), **p** (dia da 09 exportada trava a 10), **q** (as duas
bordas), **r** (dia sem data legivel entra por omissao), **u** (AST: `str(ini)` na comparacao, 2 ocorrencias),
**v** (AST: DOIS sitios lendo `he_fora_da_janela`). Os dois verdes desde o RED -- **s** (segue por ponta com o
minuto do DIA) e **t** (dia sem ponta nao vira entrada) -- sao guardas de FORMA, e e de proposito que eles
passem no defeito: o que eles cobram e que a **cura** nao mude a forma. Verde depois: `Ran 22 tests` / `OK`
(os 14 do item 3 mais os 8 novos), 276 vizinhos OK, ruff limpo.

**CERTIFICADO NA SOMBRA PELA MESMA FUNCAO REAL** (`logs/o214item4/trava_curada.out`, arvore CURADA montada
por `SOMBRA_ARVORE`): `medir` em 196,9 + 138,3 + 102,2 s, **`TOTAL emp2+3+4: trava 70 em 1148 dia-colab com
pendencia`**, e **FORA da janela = 0** nas tres. Os numeros fecham com a particao da medida do defeito **sem
sobra**: 52+16+2 = **70** (os "dentro" de cada empresa) e 776+337+35 = **1.148** (o "dentro" do universo) --
`estado_por_dia` nao mudou, entao a cura tinha de reproduzir a particao exatamente, e reproduziu. `falhas = 0`
nas tres e a testemunha de IMPACTO do terceiro ato (a soma do `dif_topo` saindo de TEXTO para DATA): nenhuma
empresa ganhou falha de portao. **O flip do cadastro se decide sobre 70**, nao sobre 326 e nao sobre 469.

**UM SELO EXISTENTE FICOU VERMELHO SOBRE O CODIGO CERTO, e foi RE-APONTADO, nao apagado**:
`test_MORDE_o_contador_le_a_MESMA_fonte_que_a_TELA_risca` ancorava no **texto** de `inspect.getsource(medir)`,
e a cura moveu o laco para fora de `medir`. A pergunta dele sobrevive inteira -- "o contador le a mesma fonte
que a tela risca?" --, entao ele passa a ler o **coletor**, e a metade negativa ficou **mais forte** no mesmo
ato: `assertNotIn("r.get('dias_he_fora_da_janela')")` passa a varrer o **modulo inteiro**, nao mais so
`medir`. Ganhou tambem a linha que cobra a **delegacao** (`medir` tem de chamar `pendencias_he_da_janela`),
para que o laco nao possa desaparecer sem alarme. E a licao e a de sempre nesta casa: **selo ancorado em texto
de funcao de 400 linhas acusa o codigo certo no dia em que a cura move o laco**.

**LEI ANTES DO PATCH** (grep em `LEIS.md`/`CORTES.md`/`DOSSIES.md` antes de montar o patch): o sitio esta
protegido pela **L-097** -- *"o portao do export trava com HE pendente > 0: `folha/porta_export.py::medir`
ganha `he_pendente`, esperado 0"* --, e e justamente a clausula (2) dela que o vazamento tornava mentirosa:
"esperado 0" sobre um contador que inclui dia de agosto nao e portao, e numero. Ela nao muda de texto; o que
muda e o universo passar a ser o que ela sempre disse, **a competencia**. Tambem passam por aqui a **L-095**
(todo leitor le, ninguem recalcula -- o `medir` e contador, e por isso a janela se le do juiz) e a **L-003**
(zero numero sem medicao na fonte, que foi o que pegou o bug: medir chamando a funcao REAL). O `DOSSIES.md:362`
ja nomeava `folha/porta_export.py::medir::he_pendente` como sitio da familia.

`LEI-AKITA: origem=folha/porta_export.py::medir (o laco que coletava a pendencia sem a janela) + a terceira forma por TEXTO no dif_topo, testemunha=ponto/janelas.py::janela_fechamento aplicada por he_pendente_lavrado.py::data_do_dia (o MESMO juiz que o retrato lavrado usa), RED=folha/tests/test_o214_item3_trava_export.py casos o-v (6 de 8 vermelhos em logs/o214item4/red_vazamento.out), quem-mais-le=censo fechado -- `he_pendente` so e lido dentro de folha/porta_export.py (medir -> carimbo -> conferir) e por NENHUM template, `data_do_dia` por apurar e agora pelo portao, juizes novos=0 (L-097, L-095, L-003 citadas)`

## O214 item 3 — **A TRAVA DO TXT CONTA SO O DIA QUE E DO ADMIN** (09/10 04:5x)

Item da fila 1, na ordem dele. A lei, literal: *"**ITEM 3 TRAVA**: `colaboradores/models.py:97::he_pendente_trava_export` passa a contar **so dia ACIMA do limite e sem decisao** em `folha/porta_export.py::medir` (:468-478), e **liga depois do item 2 no ar, nunca antes**."* O item 2 esta no ar desde `041fd2ac`/`c32acf2e`, entao a condicao temporal esta cumprida.

**A PRE-MEDIDA, na sombra, ANTES de ligar nada** (`logs/sombra/medir_o214_item2.py`, competencia 10/2026, limite cadastrado 15 min nas quatro empresas). **E PRE-MEDIDA, e nao o numero da trava**, e a distincao e de UNIVERSO (LEI-AKITA 8): a sonda varreu o **retrato lavrado** -- *"colab com pendencia lavrada"* -- e replicou o predicado a mao, porque a funcao ainda nao existia; o `medir` alimenta `he_pendente` **so para quem `classificar_export` diz que ENTRA no TXT** (fica fora rescisao, sem codigo do Dominio, cadastro_zero) e le o **espelho vivo**, nao a foto da lavratura. Entao o 469 e **teto**, nao o numero: ele diz a ORDEM DE GRANDEZA do que a trava passaria a barrar.

| empresa | dias SEM decisao | ACIMA do limite (= travam) | abaixo (do SISTEMA) |
|---|---|---|---|
| 1 Confiance | 0 | **0** | 0 |
| 2 J.A | 399 (141 colabs) | **371** | 28 |
| 3 JSP | 103 (37 colabs) | **93** | 10 |
| 4 G3 | 7 (4 colabs) | **5** | 2 |
| **total** | **509** | **469** | **40** |

**O NUMERO DA TRAVA SE LE DO CARIMBO, depois do deploy e ANTES do flip**: `len(detalhe['he_pendente_trava_dias'])` do proximo `medir` em prod, que e a propria funcao julgando o proprio universo. Essa leitura e **pre-condicao do flip** do cadastro, nao um conferir depois dele. E o flip tem DOIS momentos, nao um: o `conferir` le `falhas` e `he_pendente_trava` do CARIMBO, entao ligar o cadastro so morde quando o `medir` seguinte rodar.

Os **40** dias de diferenca sao a razao de ser do item 3: eles estao abaixo do limite, o proprio sistema **se recusa a decidi-los** (`recusar_ponta_pequena` devolve *"e do ADMIN"* so acima do limite), e com a conta antiga -- `len(he_pendente)` -- eles travariam o TXT **sem porta do outro lado**. O cadastro segue DESLIGADO: este numero e o que a trava passaria a valer se ligada, e e por isso que ele se publica antes.

**O DEFEITO QUE ESTAVA NO CAMINHO, curado em passagem (LEI-AKITA 6 e 8).** A mensagem da recusa dizia *"HE fora da janela SEM decisao em %d dia(s)"* sobre `len(d['he_pendente'])`, que e uma entrada por **PONTA** desde 30/09 -- entao o dia com entrada **E** saida fora da janela se lia como **"2 dia(s)"**, e o **mesmo** `len` somava em `falhas`. Rotulo dizendo dia e conta contando ponta, no numero que vai no motivo da recusa do TXT. RED **g**.

**TRES SITIOS, ZERO JUIZ NOVO** (o O214 proibe um), e nenhum deles nasceu: os tres **ganharam o segundo leitor** e por isso ganharam nome.
1. **A soma do DIA** -- `ponto/services/he_pendente_lavrado.py::minutos_fora_do_dia`. O `apurar` ja a fazia inline; o `medir` precisa do **mesmo** total porque o limite se compara com o DIA, nao com a ponta (duas pontas de 8 min fazem 16, e com o limite em 15 o dia deixa de ser do sistema). Somar de novo no export seria a testemunha recalculando.
2. **O predicado do limite** -- `ponto/portas/he.py::acima_do_limite`. O `recusar_ponta_pequena` ja o tinha como `_min > _lim`; a trava pergunta a **mesma** coisa ("este dia e do admin?"). Ele e **uniforme inclusive em LIMITE 0**: zero desliga a recusa automatica do item 1, nao a fila do admin -- com limite 0 nenhum dia e pequeno, e `minutos > limite` ja diz isso sem caso especial. RED **h**.
3. **A decisao viva** -- `ponto/services/gestao_he.py::estado_por_dia`, que era `_estado_por_dia` e ficou **publica**. Uma terceira query ao `DecisaoHE` dentro do export nasceria com outra forma de chave (o lavrador indexa por `date`, esta funcao por `'YYYY-MM-DD'`) e com outra janela: as duas so discordariam **no dia da borda**, que e o dia que importa.

A conta em si e `folha/porta_export.py::dias_acima_do_limite_sem_decisao` -- funcao **pura**, que so desduplica por dia e ordena; ela chama os dois juizes acima e recebe os estados como **dicionario**, lido UMA vez fora do laco (e zero query quando nao ha pendencia). **`NAO` LIBERA**: a lapide do `DecisaoHE` diz que `NAO` **e** decisao, nao ausencia dela, e travar por um `nao` deixaria o admin com um gesto so para liberar o TXT -- autorizar. RED **c**.

**A lista vai ao CARIMBO** (`he_pendente_trava_dias`) porque o `conferir` le o `detalhe`, nunca o retorno vivo do `medir` -- fora de la a mensagem nao teria como nomear o dia que barrou. A entrada da pendencia segue **por ponta** com `sentido` (selo do B1, 30/09) e passa a carregar `minutos_do_dia` ao lado: `minutos_fora` e da ponta, `minutos_do_dia` e do dia, nenhuma no lugar da outra.

**RED PRIMEIRO, evidenciado**: `logs/o214item3/red_item3.out` -- `Ran 14 tests` / **`FAILED (failures=3, errors=10)`**, 13 vermelhos nomeados antes de uma linha de cura. Verde depois: `Ran 14 tests` / `OK`. Os **dois selos existentes que a cura tornaria mentirosos foram VIRADOS, nao apagados** (`folha/tests/test_b1_portao_he_nasce_desligado.py`): a conta do `falhas` passa a chamar a funcao REAL e ganhou as duas metades novas (dia pequeno nao trava, dia com `nao` nao trava), e a assercao da mensagem passa a cobrar a chave nova. O `test_n` cobra a **procedencia** do valor por AST, nao por texto: `assertIn("'minutos_do_dia'")` ficaria verde com `'minutos_do_dia': _hf.get('minutos')` -- a chave do dia com o numero da ponta, que e exatamente o erro que o item 3 existe para nao cometer.

`LEI-AKITA: origem=folha/porta_export.py::medir (a conta do portao) + ponto/portas/he.py (o predicado) + ponto/services/he_pendente_lavrado.py (a soma do dia), testemunha=DecisaoHE via gestao_he.estado_por_dia e o retrato de he_fora_da_janela que a TELA risca, RED=folha/tests/test_o214_item3_trava_export.py (14 casos, 13 vermelhos em logs/o214item3/red_item3.out), quem-mais-le=censo fechado -- `he_pendente` so e lido por folha/porta_export.py (7 sitios) e por nenhum template; `estado_por_dia` por gestao_he (2) e agora pelo export, juizes novos=0 (tres funcoes nomeadas sobre regra que ja existia, nenhuma com regra propria)`

**O QUE O SMOKE DE PROD VAI MOSTRAR, dito ANTES para ninguem ler silencio como defeito**: o `conferir` le o carimbo, e o carimbo de hoje **nao tem** a chave `he_pendente_trava_dias` -- entao a linha de HE fica **ausente** da mensagem ate o proximo `medir`. Com o cadastro desligado ela ficaria ausente de todo jeito. O smoke e *"a rota responde e nao ha 500"*; a linha reaparece quando o carimbo for refeito.

**ACHADO LATERAL DA MESMA MEDICAO, nomeado e nao curado** (a fatia esta no BACKLOG): o `confirmar` de 14 dias custa **5,32 s**, dos quais **5,12 s sao os 14 callbacks** de `recalcular_por_evento` no `on_commit` -- um `recalcular_fechamento_mes` **por dia**, todos da MESMA competencia do MESMO colab. O ato em si leva 0,20 s. O sobrecusto do caminho real contra a previa (0,30 s) e de **5,02 s**: e este o numero da decisao sincrono-x-job, e a cura e desduplicar por (colab, competencia) dentro de `recalcular_por_evento`, nao na tela.

---

## O214 item 2 — **UM ATO, UM MOTIVO, E A TELA MOSTRA O DIFF ANTES DE CONFIRMAR** (09/10 03:xx)

Item da fila 1, pela ordem de 08/10 19:5x. A lei, literal: *"ITEM 2 AUTORIZAR POR COLABORADOR E PERIODO: um
ato, um motivo, e a tela mostra **ANTES de confirmar** as horas que entram por rubrica (o DIFF daquele
colab); grava uma `DecisaoHE` por dia."* **21 selos `a-p` verdes** (`Ran 21 tests` / `OK`), 97 nos vizinhos
nomeados, e a **suite inteira da copia** -- `Ran 10126 tests` -- que achou DOIS vermelhos que os
vizinhos nao viam, os dois meus e os dois curados na ORIGEM (abaixo). **Zero juiz novo, zero rubrica nova, zero escritor novo de
`DecisaoHE`**: a porta `ponto/portas/he.py::decidir_he` segue sendo chamada dia a dia, como o lote de recusa
ja faz, e as 7 rubricas tem UMA declaracao -- elas nasceram no `diff_janela_he.py`, mudaram de casa
para `ponto/services/autorizacao_he_periodo.py::RUBRICAS` e o comando passou a LE-LAS, porque duas
tuplas significariam a proxima rubrica entrando numa e nao na outra.

**A PREVIA SAI DO MOTOR REAL, DENTRO DE TRANSACAO DESFEITA.** `ponto/services/autorizacao_he_periodo.py`
grava as N `DecisaoHE` pela porta, roda `autoridade_do_periodo` antes e depois, diffa as 7 rubricas e
`raise` no fim. Nao e a regra simulada por fora — isso seria a sonda propria que o CLAUDE.md secao 6
proibe, e e a mesma lapide do `diff_janela_he`. **A tela nao soma nada**: o `mostrado` viaja no form em
**minutos inteiros**, serializado canonicamente, e o confirmar **RECUSA** quando o gravado difere do
mostrado — tela que mentiu nao confirma. O RED **m** prova que a recusa chega ao admin em PALAVRA, e a
guarda vive no SERVICO, e nao na view, que e por isso que ela tambem cobre POST forjado.

**O QUE A LEITURA DO VIVO MUDOU NO CONTRATO, antes de uma linha de codigo** (os cinco achados estao em
`DOSSIES.md` secao 8, arquivo SEM teto, nunca aqui — a licao de 08/10):
1. **`decidir_he` TROCA decisao existente.** O contrato dizia *"a porta devolve `no_op` e nao sobrescreve
   humano"* e **e falso**: `no_op` so acontece com o estado IGUAL; estado diferente cai em `_gravar` com
   `_trocada=True` e reescreve tudo. Virar um `nao` HUMANO em `sim` moveria dinheiro sem ninguem pedir, e o
   guarda "mostrado == gravado" **nao pega** (a previa flipa igual, os dois lados batem). A exclusao e do
   **LOTE**, cumprida por `autorizar_em_lote` pela lapide do `_gravar` — **nao um parametro novo na porta**
   (seria juiz novo) nem um filtro na view (seria a regra escondida na tela). RED **c**.
2. **Competencia EXPORTADA nao e exclusao de escrita.** O RED **h** dizia "o ato nao entra, porta propria",
   e o vivo responde o contrario: `decidir_he` **grava** e quem recusa e `recalcular_por_evento`, com a
   lapide *"nao e falha -- e a lei funcionando"*. Recusar no ato de PERIODO o que o ato de UM DIA aceita
   seria um **segundo juiz da mesma pergunta** (LEI-AKITA 2). Mesma conduta nas duas portas.
3. **A previa nao roda o motor N vezes**: `recalcular_por_evento` agenda em `on_commit`, e na previa os N
   callbacks sao **descartados**. Custo ~2 `autoridade_do_periodo` + N inserts baratos.
4. **ACHADO LATERAL, NOMEADO E NAO CURADO** (L-009, regra de negocio fora do pedido): `recusar_em_lote`
   tem o buraco espelhado **em producao** — `ponto/views.py:559` filtra `sem_decisao` so no gesto "todos",
   e a marcacao por CAIXA vai crua do form, entao um `sim` humano pode virar `nao` em lote por form velho.
5. **No CONFIRMAR sao N `recalcular_fechamento_mes` da MESMA competencia do mesmo colab**, redundantes e
   pos-commit. Nao e dano (idempotente); e numero, e a dedup por (colab, competencia) pertence a
   `recalcular_por_evento` — origem, e fora deste pedido.

**OS k-p FICARAM VERMELHOS POR FIXTURE, E O QUE ELA ENSINOU VALE MAIS QUE O VERDE.** Nenhuma linha de view,
url ou template mudou para passar. Duas causas, as duas medidas:
- **a grade do espelho sai da `CelulaDia.ata`, nunca do `dna`**: `leitor_celula.py::grade_da_celula` chama
  `_celulas_da_ata`, que monta celulas e regua a partir de `ata['lampadas']`. Celula com DNA e ata NULA
  devolve 0 celula e regua vazia -> `marcar_pontas_fora` sem o que marcar -> `he_fora_da_janela` vazio ->
  retrato VAZIO -> e a tela respondia, **com razao**, que nao havia dia SEM DECISAO. A fixture passa a pedir
  a lavratura **pela PORTA** (`cartorio.julgar_celula(forcar=True)`, o juiz do cron das 06:28 e do signal),
  nunca escrevendo `ata=` a mao: fixture que grava ata e um SEGUNDO escritor. Ela precisa pedir porque
  `bulk_create` nao dispara signal e `on_commit` nao roda dentro de `TestCase`; em prod a ata ja existe.
  O dia IGUAL a hoje volta `None` do cartorio — teto temporal, o turno 07-19 nao terminou.
- **o ator nao tinha `ver_folha`**: o ato gravava e o redirect para `/ponto/gestao-he/` dava **404**
  (`views.py::gestao_he` levanta `Http404` sem ela). Em prod quem autoriza chega pelo BOTAO daquela tela;
  ator com `autorizar_he` e sem `ver_folha` nao existe. **A tela nao ganhou gate** — a permissao do ato
  segue so na porta, um juiz.

`LEI-AKITA: origem=ponto/portas/he.py + a tela da Gestao de HE, testemunha=autoridade_do_periodo (as 7
rubricas do diff_janela_he), RED=a-p (21, verdes), quem-mais-le=censo fechado em DOSSIES secao 8,
juizes novos=0`

FALTA, e esta nomeado: **smoke de clique do Ronald** nas duas cascas (FRONT SEM SMOKE NAO SOBE — a fatia
toca template), e a **medicao do custo do confirmar** na sombra (N recalculos), que e o numero em que a
decisao sincrono-x-job se apoia.

### O QUE A SUITE INTEIRA ACHOU, e os vizinhos nao (09/10 03:2x -- `Ran 10126 tests` / `FAILED (failures=2)`)

Os 21 selos do item 2 e os 97 dos seis modulos vizinhos estavam VERDES, e a suite cheia ficou **vermelha em
dois**. Os dois eram meus, nenhum dos dois estava nos vizinhos que eu escolhi, e e por isso que a suite cheia
nao e cerimonia: **eu escolhi os vizinhos pelo assunto, e estes dois cobram FORMA** -- um contrato de arvore
e um selo de tela de outra fatia.

1. **`test_contract_no_except_pass` morde `except: pass` em arquivo NOVO**, e o meu
   `autorizacao_he_periodo.py` tinha um: `raise _Rollback()` / `except _Rollback: pass`, lendo `_out` de uma
   atribuicao anterior ao `raise`. Nada era silenciado ali -- a excecao e propria e o rollback e o ato --,
   **e a proibicao esta certa mesmo assim**: handler vazio obriga quem le a descobrir, pelo fluxo, que a
   medicao sobreviveu a transacao desfeita. Cura na ORIGEM, nao na allowlist: a `_Rollback` passou a
   **carregar a medicao** (`.medido`), e o handler faz o que handler faz -- `_out = _desfeita.medido`. O dado
   anda pelo caminho declarado, e a allowlist do contrato segue do mesmo tamanho.
2. **`test_tela_gestao_he_forma_b::test_MORDE_o_expandido_NAO_repete_rotulo_por_linha`**: `15 != 13`
   campos `name="motivo"` ocultos. O numero 15 esta CERTO -- a linha da pessoa ganhou um segundo ato, e ele
   leva o seu motivo --, e **a cura nao foi trocar 13 por 15**. Somar os dois atos num numero so era o
   defeito do selo: o total cresce quando um ato novo nasce e **nao diz qual deles mudou**, que e exatamente
   o sinal que se perde. Ele passou a contar **por ATO**, separando os forms pela `action`: um motivo por DIA
   sem decisao (13), um por PESSOA com dia sem decisao (2), e **nunca dois no mesmo form** -- este ultimo e
   novo e morde o caso em que o segundo campo venceria no POST sem ninguem saber qual o dialogo preencheu.

Depois das duas curas: `Ran 59 tests` / **`OK`** nos quatro modulos envolvidos (`logs/o214item2/cura1.out`),
ruff limpo, e a suite cheia **relancada**: `Ran 10126 tests in 1362.734s` / **`OK (skipped=42)`**
(`logs/o214item2/suite_cheia2.out`, 03:32-03:55). O veredito que autoriza o pouso e **esse**, nao o dos
vizinhos -- e a frase "suite da copia verde" so entrou neste RELATO depois dele existir.

### O contrato, escrito ANTES do codigo (09/10 01:2x — a parte que a L-110 exige)

Enquanto o ensaio da sombra da O146 corre (o portao e CEGO entre 00:00 e 04:00, CLAUDE.md secao 2, por
isso `--refazer --dump-agora` + `--bloco`), o proximo item da fila 1 **nao ficou esperando**: o contrato
de entrada, o desenho, os **REDs a-h** e o PROIBIDO da **O214 item 2** estao escritos **pela regra, antes
do codigo** (L-110), em `app/docs/DOSSIES.md` **secao 8** — arquivo **sem teto**, e nao aqui, pela licao de
08/10 (o dossie da O146 teve de ser recuperado do transcrito porque o ponteiro apontava para prosa que a
DIETA apagou). **Nada construido**: a LEI 10 exige copia nascida do HEAD **no ato do patch**, e o HEAD muda
no commit da O146.

O achado que o contrato traz, e ele muda o desenho: **a recusa de `autorizar_em_lote` nao proibe lote.**
Ela diz, literal (`ponto/portas/he.py:272`), *"dinheiro em lote exige DIFF publicado ANTES -- que e um ATO
de esteira, nao um clique de tela"*. A lei do item 2 **cumpre** essa condicao em vez de dispensa-la (a tela
mostra o DIFF antes de confirmar), entao a revogacao e do **TAMANHO DO ATO** — um dia passa a um
colaborador num periodo —, nunca da condicao: **varios colaboradores e empresa inteira continuam
levantando**. E a PREVIA sai do motor REAL dentro de transacao desfeita, pelo molde que
`ponto/management/commands/diff_janela_he.py` ja usa, com as **7 rubricas que ele declara** — zero rubrica
nova, zero escritor novo de `DecisaoHE` (a porta segue sendo chamada dia a dia, como `recusar_em_lote` faz).

## O146 — **FECHADA, NO AR**: a extra declarada da escala desloca o limite, e o limite
tem UM sitio (09/10 00:4x · `cfd4ff83`+`ce212bb8`, deploy 01:35, smoke OK)

**PROVA:** gravado medido em prod 09/10 10:0x (so leitura, agregado): `TipoEscala` **353**, com extra declarada (`he_extra_antes_min>0 | he_extra_depois_min>0`) **0**; `CelulaDia` **124.358**, com `dna['extra_declarada']` **0**; `dna_versao` gravado **1: 20.785 · 2: 103.573** (nenhuma 3 -- a impressao digital do cartorio nao andou, como o item 4 prometeu). O sitio unico esta NO AR e o IMPACTO de hoje e **zero cadastro**: o limite se desloca quando alguem declarar a extra, e ninguem declarou ainda.

Fatia da fila 1, pela ordem de 08/10 19:5x (`O145 -> O146 -> item 2 da O214 -> itens 3 e 4`), contra o
dossie que mora em `app/docs/DOSSIES.md` secao 7. A lei que a governa e a resposta dele de 08/10 21:5x
(`O146-EXTRA-E-SO-HE`): **a extra alarga o que se PODE ganhar, nunca o que se DEVE cumprir.**

**OS 4 ITENS DE `MUDA`, cada um com o sitio:**

1. **CADASTRO.** `escala/models.py:78` `he_extra_antes_min` e `:83` `he_extra_depois_min` —
   `PositiveSmallIntegerField`, **default 0**, migration `escala/0042_o146_he_extra_declarada.py`. Entram
   **pelo WIZARD** (`escala/views_wizard.py:40-41` e `:240`, `escala/services/cadastro_tipo.py:255`,
   `templates/escala/wizard_tipo_escala.html` + `_wizard_preview.html`). **`permite_hora_extra` nao foi
   reusado** — e booleano e tem outro uso (`ponto/triagem_batida.py:293`,
   `ponto/management/commands/processar_alertas_turno.py:129`), os dois intactos.
2. **DNA.** `gerar_celulas.py::montar_dna:123` congela `dna['extra_declarada'] = {'antes': N, 'depois': M}`
   — chave de **topo** e emitida **so quando declarada**, para que a impressao digital do cartorio
   (`ponto/services/cartorio.py:85::impressao_insumos`, que digere `dna_versao` + `marcos`) e as duas
   comparacoes de dict inteiro (`tripwire_celulas.py:30`, `ponto/portas/celula.py:496`) **nao se movam em
   dia que nao declara nada**. `DNA_VERSAO` segue **2**.
3. **UMA FUNCAO DESLOCA O LIMITE.** `ponto/janela_he.py:54::minutos_fora_da_extra` — uma subtracao, um
   sitio. Lida por `entrada_efetiva:76`, `saida_efetiva:117` e `marcar_pontas_fora:156`, e por cima dela
   o motor (`motor_calculo_v2.py:1363` e `:1401`, pelo `j['extra_antes']`/`j['extra_depois']` de
   `_janela_do_dia:1303`). **Nenhum leitor com conta propria** — e por isso que a comparacao `> piso` ja
   havia virado `dentro_da_janela:41` em 29/09.
4. **MINUTO DENTRO DA EXTRA = HE AUTORIZADA PELA ESCALA**, sem `DecisaoHE`. Fora dela, **ponta**, e a
   L-097 segue inteira: o miudo que sobra continua bloqueado e cai no item 1 da O214.

**QUEM PERGUNTA A EXTRA, e por que nao e o motor que le o cadastro:** o juiz e
`EscalaColaborador.extra_declarada_do_dia` (`escala/models.py:1183`), **irmao do `intervalo_do_dia`** e com
o contrato dele palavra por palavra — `celulas` e ALIMENTACAO, **chave ausente = "nao ha celula nesse dia",
e dia sem celula le o template vivo** (`TipoEscala.extra_declarada_do_dia:310`). O motor **pergunta**
(`motor_calculo_v2.py:1154::_extra_declarada`, memoizado por dia, injetado em `:1677`); ler
`te.he_extra_antes_min` de dentro do motor seria o motor com cadastro proprio, e o passado reescrito por
troca de template.

**OS CASOS, pela regra ANTES do codigo (L-110).** `ponto/tests/test_o146_extra_declarada_da_escala.py` —
**26 casos, `Ran 26 tests` / `OK`**. A **suite INTEIRA na copia**, pela porta unica
(`bin/suite.sh --dir`): **`Ran 10105 tests in 1363.127s`** / **`OK (skipped=42)`** — mais os 143
vizinhos do modulo e `ruff` limpo nos 10 `.py` tocados.

| caso | o que a regra manda | teste |
|---|---|---|
| **a** | saida em marco+127 com extra 120 → **120 de HE, ponta de 7** | `test_a_...127_com_extra_120_da_120_de_HE_e_ponta_de_7` (+ 2 do teto: dentro nao acusa, acima **ainda** acusa CADASTRO x REALIDADE) |
| **b** | saida em marco+60 com extra 120 → **60 de HE, ponta 0** | `test_b_...da_60_de_HE_e_ponta_zero` |
| **c** | saida **no marco** com extra 120 → 0 de HE e **NAO e saida antecipada** | `test_c_...nao_da_HE_e_NAO_e_saida_antecipada` |
| **d** | entrada 10 min antes **sem** extra → ponta de 10, **como hoje** | `test_d_...e_ponta_de_10_como_hoje` |
| **e** | extra antes 60: entrada em marco−60 → 60 de HE; marco−70 → 60 **+ ponta de 10** | `test_e_...` e `test_e2_...` |
| **f** | escala **sem** declaracao → **identica a hoje** | `test_f_escala_SEM_declaracao_e_IDENTICA_a_hoje` |
| **g** | dia com `DecisaoHE` autorizado → tudo conta, como hoje | `test_g_...conta_TUDO_como_hoje` |
| **h** | turno que **cruza a meia-noite** com extra depois | `test_h_extra_depois_no_turno_que_cruza_a_meia_noite` |
| **i** | rodar 2x = **mesmo estado** | `test_i_...` e `test_i2_marcar_pontas_fora_2x_...nao_duplica_pendencia` |
| **j** | **MORDE**: motor e `marcar_pontas_fora` dao o **MESMO** numero no caso **a** | `test_j_MORDE_motor_e_marcar_pontas_fora_dao_o_MESMO_numero` |
| **k** | *(meu, do contrato do irmao)* celula que EXISTE e nao declara → **ZERO, nunca o template vivo**; dia **sem** celula → template vivo; e a alimentacao com **0 query** no laco | `test_k_...`, `test_k2_...`, `test_k3_a_alimentacao_tem_o_contrato_do_irmao` |

Mais os selos do dossie: **previsto NAO cresce** com a extra (a clausula 1 da lei, com caso que morde a
volta), a extra entra **pela PORTA do wizard** e **nao reusa** `permite_hora_extra`, o default e **ZERO**,
a chave do DNA **nasce so quando declarada**, a impressao do cartorio **nao muda sem declaracao**, **a
conta mora em UM sitio so** e **os dois leitores IMPORTAM a funcao do dono** (por AST).

**O RED que vale mais do que os outros**, porque e o que distingue cura de silencio: o montador do espelho
ficou **`AssertionError: 7 != 67`** com o CONTROLE verde ao lado (dia sem declaracao segue riscando os 67).
Vermelho com controle verde quer dizer *"falta uma subtracao"*; vermelho sozinho quer dizer *"o montador
esta mudo"*, e e esse o engano que a familia de vacuidade de 01/09 cobra.

**IMPACTO DE FROTA = 0 h, por UNIVERSO VAZIO — e medido na fonte, nao por amostra.** Sonda
`logs/sombra/universo_o146.py` pela porta `bin/sombra.sh --rodar` (`logs/o146/universo.out`, 09/10 00:48):

```
colunas novas presentes no esquema de prod: []     (nascem na 0042)
TipoEscala cadastrados: 350                        (o denominador de quem PODERIA declarar)
emp2 12.990 celulas / 8.327 de trabalho  |  emp3 3.434 / 1.901  |  emp4 600 / 352
TOTAL 17.024 celulas, 10.580 dia-colab de TRABALHO, com a chave `extra_declarada` = 0
```

As duas fontes do juiz dao vazio ao mesmo tempo: **0 celulas** carregam a chave e as **duas colunas nao
existem** no esquema de prod, logo nenhuma linha pode ter numero nelas — o unico escritor delas e o wizard,
que **nasce nesta fatia**. Entao o deslocamento vale **0 min em 10.580 de 10.580** dia-colab da competencia
10, e `minutos_fora_da_extra(x, 0) == x` e a **identidade** (auditada linha a linha nas duas efetivas: com
`extra_min=0`, `sobra == bruto`, `marco − timedelta(0) == marco`, e o `max(0.0, …)` sobre bruto **negativo**
— chegada atrasada, saida antecipada — cai no **mesmo ramo** de antes, porque `dentro_da_janela` compara
`<= piso` e `piso >= 0`). **Nao ha apply de dinheiro nesta fatia.** Os 10.580 contra os 10.578 medidos em
08/10 sao a diferenca de FONTE (sombra do dia x prod naquele dia), nao de regra.

**POR QUE O DIFF NAO SE MEDE NA SOMBRA COM O CODIGO NOVO, e isso e lei da casa:** `bin/sombra.sh` **nunca
migra a sombra** — ele so COMPARA `django_migrations` prod x sombra. A sombra tem o esquema de PROD, sem as
duas colunas, e qualquer consulta a `TipoEscala` com o codigo novo quebraria ali. A pergunta certa para o
esquema de hoje e a que a sonda faz: *as colunas existem?* Nao existem — e e isso que fecha o universo.

**QUEM MAIS LE, censo fechado (o `quem-mais-le` do LEI-AKITA do aval):**
- `ponto/services/espelho.py:728-736` — montador, **passa a extra** pelo mesmo juiz;
- `colaboradores/services/calendario.py:434-437` — idem, com a carga de celulas **icada** para servir os
  dois consumidores do mes numa consulta so;
- `ponto/services/he_pendente_lavrado.py:53-98` — chama `espelho_do_colab` e le `d['he_fora_da_janela']`:
  **herda**, sem linha nova;
- `recusar_ponta_pequena` — documenta que le `marcar_pontas_fora` *"pelo montador do espelho"*: **herda**;
- `ponto/services/espelho.py::pontas_do_relato` — **deliberadamente NAO recebe a extra**: ele TRADUZ a
  linha publicada pelo motor, cujo `minutos_fora` ja e a sobra. Passar a extra ali subtrairia duas vezes;
- `motor_calculo_v2.py:2725` (`Motor12x36ComEscala._entrada_efetiva`) — **herda** pelo `_janela_do_dia`,
  sem copia;
- `_acusa_cadastro_x_realidade` — **nao muda**: o `cadastrado` segue sendo o marco **CRU**, porque
  `pontas_do_relato` casa por `it['cadastrado'] != marco_da_celula`; deslocar ali deixaria a cura da O145
  **muda exatamente nos dias que declaram extra**. O `minutos_fora` ja chega como sobra.
- **O pre-julgamento da L-084 fica nos marcos CRUS**, e isso e da propria L-084: ela pergunta se o
  **cadastro descreve o dia** (3 h nas duas pontas). Deslocar o marco antes dela deixaria a extra decidir
  se a L-084 se aplica.

**TRES COISAS QUE EU VI E NAO CUREI, porque curar aqui seria regra de negocio fora do pedido:**
1. declarar extra num template torna as celulas existentes **STALE** sob `tripwire_celulas.py:30` ate o
   `regenerar_celulas_vinculo` — **igual a editar `hora_fim`**, e e o desenho (a celula e soberana);
2. com `piso > 0`, o minuto que fica no piso **alem** da extra vira HE — mas a L-097 fixou **piso 0** nas
   tres empresas, entao hoje isso nao alcanca ninguem;
3. os dois leitores de `permite_hora_extra` ("batida tardia") seguem inalterados.

`LEI-AKITA: origem=ponto/janela_he.py:54 (o limite da janela) + TipoEscala/DNA (cadastro), testemunha=a
celula (dna.extra_declarada) lida pelo motor E por marcar_pontas_fora, RED=casos a-k + 10 selos,
quem-mais-le=censo fechado acima (7 leitores, 2 herdam, 1 recusado com motivo), juizes novos=0`

**FALTA, nomeado e nao escondido:** a **LINHA HAIKU** do dossie — contador *"extra da escala"* no payload
do copiloto com rotulo de admin e o golden *"quanto de HE da escala o colab X tem no dia Y"*, degrau
**leitura**. Ela mora na stack `mensageria/nucleo/ferramentas.py`, que tem deploy proprio, e **nao esta no
`PRONTO` do dossie**; fica na celula da O146 como a PORTA-RETRATAR-BATIDA ficou com a dela.

---
# PEDIDOS DE PATCH **ABERTOS** — A DIETA NAO MOVE ISTO

> **A LEI E DA PROPRIA DIETA DE PROSA**, escrita em 02/10 23:5x: *"a dieta move o que ja aconteceu,
> nao o que ainda tem de acontecer"*. Pedido de patch ABERTO e' **contrato entre duas metades de uma
> fatia**, nao historia: a metade de TELA ja esta na arvore, atras de um gate, e a metade de NUCLEO
> le **aqui** o nome das chaves de contexto. Nome diferente nao reprova -- so **CALA** (o `{% if %}`
> nao abre e a tela sai byte a byte igual), e e' por isso que o nome e' cobrado por selo.
>
> **ESTA SECAO JA FOI QUEBRADA DUAS VEZES PELO MESMO ATO**, as duas descobertas pela suite inteira:
> 02/10 (push 94, levou `competencia_rotulo`) e **05/10 `fdd6f42c`, que e' meu** -- a dieta do marco
> O191 moveu 5.548 linhas do vivo e levou o pedido INTEIRO da fatia 2 junto, e o push do marco voltou
> VERMELHO em 2 selos **depois de 619 s de suite**. Na primeira vez a cura foi restaurar a palavra, e
> so isso: **nenhuma guarda nasceu**, entao a classe sobreviveu para reaparecer tres dias depois.
> Agora existe o tripwire: **`bin/tests/test_relato_guarda_pedido_de_patch.sh`**, que le por AST dos
> PROPRIOS selos quais nomes o vivo tem de carregar (nunca uma lista digitada: a lista repetida foi o
> buraco dos LABELS em quatro lugares) e fica VERMELHO **em menos de 1 s**, na regua, antes da suite.
>
> **Ao arquivar pela dieta: esta secao fica.** Quando a metade de nucleo pousar, o pedido vira
> historia e vai para o `RELATO-ARQUIVO.md` **no mesmo commit** que religa os selos pulados -- e a
> assercao do selo **inverte** para a autoridade de codigo, como a da UI-CAL-COMPETENCIA inverteu
> hoje (ela passou a cobrar `competencia_rotulo` em `colaboradores/services/calendario.py`, porque as
> duas metades dela pousaram: `:225` o modo, `:857` o rotulo). Selo nao se apaga; a pergunta troca.
>
> Copia integral da historia deste pedido, com a conversa em volta: `app/docs/RELATO-ARQUIVO.md`
> (a partir de `# PEDIDO DE PATCH DA RAIA UI -> MAIN`, linha ~23.954). **Mover, nunca apagar.**

## O QUE A TELA JA FAZ SEM PATCH NENHUM, e por que isso e seguro

Com o contexto de hoje o HTML servido muda de FORMA (celula com `▲19`/`▼5`, tres estados por led + risco +
fundo, dia clicavel que marca, tabela recolhida) e **nao muda de PODER**: a barra nao tem `method` nem
`action`, o `<button>` dela nasce `disabled` e o motivo esta em texto visivel ao lado -- nunca num `title`,
que e o que a LEI-UI **L4** proibe (`app/docs/LEIS-UI.md:39`). `{% url %}` de rota inexistente **nao entra**:
`NoReverseMatch` nao desabilita um botao, derruba a tela -- foi o apagao de 23/09 (cinco 500 em
`/colaboradores/<id>/calendario/`).

> **ATENCAO NA ORDEM DO MERGE, e isto e medido, nao suposto.** Template e bind-mount: ele muda a tela NA HORA
> (secao 2 do CLAUDE.md). O calendario novo **nao tem botao Autorizar por linha** -- o desenho aprovado pede
> *"tabela recolhida e sem botao por linha"* --, e a barra que o substitui so liga com o patch 1. Entao, entre
> o merge e o `deploy.sh` do patch 1, **a tela nao tem caminho de AUTORIZAR** (a CIENCIA em lote continua
> inteira: ela e `recusar_he_em_lote`, que ja existe). Merge e deploy sao **UM ato** (MERGE DE RAIA CAI E
> RECARREGA NO MESMO ATO, corte 30/09 17:4x): aplicar o patch 1, `bin/deploy.sh`, e so entao mergear a tela --
> ou mergear e deployar sem nada no meio. Nunca a tela primeiro.

## PATCH 1 -- a view de laco e o contrato de NOME `url_autorizar_marcados`

**L-081 nao cai, e nao nasce porta de lote** (decisao de 01/10 20:1x, item 3): um clique na barra = **N ATOS,
um por dia marcado**, cada um pela porta `decidir_he`, com a propria trilha, a propria idempotencia e a
propria relavratura. `ponto/portas/he.py:175::autorizar_em_lote` **continua levantando** -- e a funcao que
existe para explicar por que nao existe --, e o laco mora na VIEW, exatamente como
`ponto/views.py:512::recusar_he_em_lote` ja faz com `recusar_em_lote`.

**(a) `app/ponto/views.py`** -- view nova, logo depois de `decidir_he` (que termina em `:508`):

```python
@login_required
def autorizar_he_marcados(request):
    """A BARRA UNICA DO CALENDARIO: N dias marcados, UM motivo, N ATOS (desenho Ronald 01/10 18:3x). POST.

    NAO E LOTE E NAO E PORTA NOVA: cada `item` vira UMA chamada de `ponto/portas/he.py::decidir_he`, com a
    propria trilha e a propria idempotencia -- o mesmo desenho de `recusar_he_em_lote`, e pela mesma razao
    escrita la (um "modo lote" dentro da porta criaria um caminho de escrita que nenhum selo de idempotencia
    cobre). O que a barra economiza e o GESTO do admin, nunca o ato.

    O MOTIVO E UM E VALE PARA OS N DIAS: a tela mostrou os N dias e a conta antes do clique, e a porta grava
    o mesmo porque em cada um. Motivo por dia eram treze campos abertos -- a poluicao de 01/10 11:1x.

    OS MINUTOS VEM DA TELA, por dia, como na `decidir_he`: a porta nao os recalcula (a lapide dela conta por
    que). `item` = `colab:data:minutos`, o MESMO formato que `recusar_he_em_lote` ja le -- nao nasce
    vocabulario de transporte novo.
    """
    import datetime as _dt

    from django.urls import reverse

    from core.respostas import resposta_acao
    from ponto.portas.he import DecisaoHERecusada, decidir_he as _porta
    if request.method != 'POST':
        from django.http import Http404
        raise Http404
    _volta = request.POST.get('voltar') or reverse('ponto:gestao_he')
    _motivo = (request.POST.get('motivo') or '').strip()
    brutos = request.POST.getlist('item')
    if not brutos:
        return resposta_acao(request, ok=False, redirect_url=_volta,
                             msg='Nenhum dia marcado. Clique nos dias do calendario e confirme de novo.')
    _ids = set()
    for b in brutos:
        _p = (b or '').split(':')
        if len(_p) == 3 and _p[0].isdigit():
            _ids.add(int(_p[0]))
    _colabs = {c.pk: c for c in Colaborador.objects.filter(pk__in=_ids)}
    ok, no_op, erros, malformados = 0, 0, [], 0
    for b in brutos:
        _p = (b or '').split(':')
        if len(_p) != 3:
            malformados += 1
            continue
        try:
            _c, _d, _m = _colabs[int(_p[0])], _dt.date.fromisoformat(_p[1]), int(_p[2])
        except (KeyError, ValueError, TypeError):
            malformados += 1
            continue
        try:
            r = _porta(_c, _d, 'autorizado', minutos=_m, autor=request.user,
                       motivo=_motivo, request=request)
        except DecisaoHERecusada as e:
            # CADA DIA E UM ATO INDEPENDENTE: um dia que a porta recusa nao desfaz os outros, e volta
            # NOMEADO -- nunca contado em silencio.
            erros.append((getattr(_c, 'pk', _c), _d, str(e)))
            continue
        no_op += 1 if r['no_op'] else 0
        ok += 0 if r['no_op'] else 1
    _partes = ['%d dia(s) autorizados, com trilha por dia. O fechamento deles foi posto na fila de '
               'recalculo.' % ok]
    if no_op:
        _partes.append('%d ja estavam autorizados (nada mudou).' % no_op)
    if erros:
        _partes.append('%d recusados pela porta: %s' % (
            len(erros), '; '.join('col%s %s -- %s' % x for x in erros[:5])))
    if malformados:
        _partes.append('%d item(ns) ilegiveis foram ignorados.' % malformados)
    return resposta_acao(request, ok=not (erros or malformados), msg=' '.join(_partes),
                         redirect_url=_volta)
```

**(b) `app/ponto/urls.py:19`** -- depois da linha do `decidir_he`:

```python
    # A BARRA UNICA DO CALENDARIO: N dias marcados num gesto, N ATOS na porta `decidir_he` (18:3x).
    # NAO e porta de lote -- `autorizar_em_lote` segue levantando, e o laco mora na view (L-081).
    path('gestao-he/autorizar-marcados/', views.autorizar_he_marcados, name='autorizar_he_marcados'),
```

**(c) `app/ponto/views.py:768`** -- no dict do `render` de `gestao_he`, ao lado de `'pode_autorizar'`:

```python
        # O CONTRATO DE NOME com a metade de TELA: sem esta chave o `{% if %}` nao abre, a barra nasce sem
        # `method`/`action` e diz por que. Mandar OUTRO nome nao reprova -- so cala (a barra nunca liga).
        # Selo: `ponto/tests/test_tela_gestao_he_calendario.py` (constante `VAR`).
        'url_autorizar_marcados': reverse('ponto:autorizar_he_marcados') if tem_acao(
            request.user, 'autorizar_he') else '',
```
(a view ja tem `from django.urls import reverse`? **nao**: `gestao_he` nao importa `reverse` -- incluir
`from django.urls import reverse` no corpo dela, como as irmas fazem.)

**O selo da metade de tela que fica VERMELHO se o nome divergir** ja esta escrito:
`test_MORDE_o_contrato_de_NOME_e_a_MESMA_palavra_nos_QUATRO_sitios` e
`test_MORDE_o_contrato_de_NOME_esta_escrito_no_RELATO`.

**RED a escrever no MESMO commit do patch** (o que a tela nao pode provar): marcar 3 dias + 1 confirmacao =
**3 `DecisaoHE` autorizadas e 3 linhas de `LogAuditoria` com `acao='decidir_he'`**, e a segunda confirmacao
com os mesmos 3 itens = **0 trilha nova** (idempotencia herdada da porta, contrato 2 de 13/09). Mais o caso
que MORDE: um item com `minutos=0` volta em `erros` e **nao derruba** os outros dois.

## PATCH 2 -- os dois selos que a L-081 trava e que o desenho de 18:3x SUPERA

Estes dois ficam VERMELHOS no instante em que a barra ligar, e **isso e correto**: eles guardam a redacao
antiga da L-081 no nivel de TELA, e o desenho aprovado em 01/10 18:3x a alterou (a porta segue um dia por
ato; o que passou a poder ser multiplo e o GESTO). Quem aplicar o patch 1 ajusta os dois no mesmo commit:

* `app/ponto/tests/test_tela_gestao_he.py:122::test_MORDE_AUTORIZAR_e_um_dia_por_ato_e_RECUSAR_pode_ser_em_LOTE`
  -- ele proibe controle de lote dentro de form que autoriza. Hoje ele passa **por vacuidade** (nao ha mais
  form com `decidir` na tag de abertura), e isso ja esta registrado aqui para nao passar por cura. A pergunta
  dele muda de *"ha controle de lote num form que autoriza?"* para *"o form que autoriza manda UM `item` por
  DIA, e nao uma lista de dias num campo?"* -- o que prende a lei que sobrou: **N atos, nunca 1 ato com
  lista**.
* `app/ponto/tests/test_tela_gestao_he.py:384::test_MORDE_AUTORIZAR_em_lote_NAO_EXISTE` -- **NAO muda**.
  `ponto/portas/he.py:175::autorizar_em_lote` continua levantando, porque **nao nasce porta de lote**. Ele e o
  selo que prova que o laco ficou na view.

## PATCH 3 -- o LIMITE DE DECISAO e cadastro da empresa (default 10 min, SO DE TELA)

Dois contratos de NOME, e os dois estao cravados na metade de tela: a chave por dia **`abaixo_limite`** e o
rotulo pronto **`limite_decisao_rotulo`**. O template **nao compara minuto com nada** -- sem a chave, nenhuma
celula aparece apagada, e sem o rotulo a legenda do limite nem se escreve (explicar um estado que a tela nao
produz e a testemunha afirmando sobre o que nao mediu). Selo:
`test_MORDE_o_template_NAO_compara_minuto_com_LIMITE_nenhum`.

**(a) `app/colaboradores/models.py:46`** -- ao lado de `dia_inicio_competencia`, que e o vizinho certo (as
duas sao regra que varia por EMPRESA, LEI-AKITA 12):

```python
    limite_decisao_he_min = models.PositiveSmallIntegerField(
        default=10,
        help_text='SO DE TELA: dia com menos minutos de HE fora do marco que isto nao e CHAMADO a decisao na '
                  'Gestao de HE (aparece apagado, sem led). NAO muda dinheiro e NAO muda a L-097 -- o minuto '
                  'segue bloqueado do mesmo jeito; o que o limite decide e se o admin e chamado a olhar.')
```

**(b) migration** `app/colaboradores/migrations/0056_limite_decisao_he_min.py` (`AddField`, default 10, sem
`RunPython`). **Ela segura o deploy** (migration pendente trava o `deploy.sh`, DEPLOY JA), entao sobe junto.

**(c) `app/colaboradores/admin.py:6`** -- o cadastro E a UI do django admin, como e para o
`dia_inicio_competencia`; o campo ganha LEITOR visivel em `list_display`:

```python
    list_display = ["nome", "cnpj", "ativa", "dia_inicio_competencia", "limite_decisao_he_min"]
```

**(d) `app/ponto/services/gestao_he.py:121`** -- em `_tira`, no dict de `por_data`, UMA chave nova. O leitor
COMPARA (e e o unico que compara), e o limite chega por argumento -- nunca por `getattr(empresa, ...)` dentro
da funcao, que seria o juiz lido duas vezes:

```python
            'abaixo_limite': bool(limite) and _m <= limite,
```
`_tira(dias, ini, fim, limite=0)` ganha o parametro, `enriquecer(..., limite=0)` o repassa nas TRES chamadas
(`:271`, `:296` e `:306`), e `ponto/views.py` o le do cadastro: `getattr(empresa, 'limite_decisao_he_min', 0)`.

**(e) o CONTADOR muda de universo, e e aqui que mora o observavel dele** (*"col616 comp 09 com o 'sem
decisao' caindo de 13 para os acima do limite, numero publicado"*). Em
`app/ponto/services/gestao_he.py:246` e `:302`, `sem_decisao` passa a contar **so o que PEDE decisao**:

```python
        linha['sem_decisao'] = sum(1 for d in linha['dias']
                                   if d['estado'] == 'sem_decisao' and not d.get('abaixo_limite'))
```
**CUIDADO, e isto e L1 (CONTADOR == UNIVERSO):** o mesmo `sem_decisao` alimenta (i) o chip da linha, (ii) o
total do topo, (iii) o botao *"Dar ciencia em tudo que esta sem decisao -- N dia(s)"* e (iv) o universo que
`ponto/views.py:538::recusar_he_em_lote` relê com `todos_sem_decisao=1`. Se o contador encolher e o ATO nao,
o botao promete 4 e faz 13 -- **o numero e a lista tem de sair do mesmo lugar**, e saem: os quatro leem a
MESMA `enriquecer`. O que o patch precisa decidir **por escrito** e se a ciencia em lote tambem passa a
ignorar os dias abaixo do limite (o `brutos` de `:558` filtra por `d.get('estado') == 'sem_decisao'`, hoje
sobre a lista COMPLETA). **Pergunta de LEI no topo deste RELATO, nao decidida aqui.**

**(f) `app/ponto/views.py:768`** -- o ROTULO vai PRONTO para a tela (L7/L2: o texto sai de onde a decisao
mora; o NUMERO nunca entra no template):

```python
        'limite_decisao_rotulo': ('abaixo do limite de decisao (%d min ou menos, cadastro da empresa)'
                                  % _lim) if _lim else '',
```
com `_lim = getattr(empresa, 'limite_decisao_he_min', 0) if empresa is not None else 0`.

**RED do patch 3, no mesmo commit:** empresa com limite **7** -> dia de 7 min tem `abaixo_limite=True` e dia
de 8 min **nao** (o caso que MORDE: com o default 10 cravado em qualquer sitio, 8 cairia abaixo e o selo
passaria por coincidencia de numero -- e o gemeo exato do `21` cravado que `ponto/janelas.py` nasceu para
matar); e empresa com limite **0/None** -> **nenhum** dia abaixo, que e o estado de hoje.

---

**L-110 `REFERENCIA-E-A-LEI` NASCEU HOJE, E REVOGOU UMA LEI DE MINUTOS ANTES DELA** (secao de 00:3x): a
referencia de um numero e a **REGRA** aplicada ao cadastro e as batidas; o codigo de hoje **nao e gabarito**
do codigo novo. `FROTA-PROVA-AMOSTRA-EXPLICA` foi desfeita nos quatro sitios no mesmo marco.

**AS DUAS LEIS DE ESTEIRA DAS 19:2x JA VALEM NESTE POUSO** (`RAIA-VERDE-POUSA` = **L-105**,
`DOCS-NO-MARCO` = **L-106**; `MERGE DE RAIA` = **L-107**, `MARCO FECHADO / PUSH UM POR MARCO` = **L-108**,
`DIETA DE PROSA` = **L-109**). O pacote que estava em voo **se separou no mesmo turno**: K8 e K5 pousam
como PRODUTO, o CERT-AST vai a pouso proprio. Por **L-105c**, em uma linha: *`cert-ast` (`cb84a4ac`) nao
pousa aqui porque e INSTRUMENTO (`bin/suite_nucleo.sh`, selo de host, paragrafo da CLAUDE.md 3) -- pouso
proprio, logo atras, levando `8ffcd44d`.*

**MARCO ANTERIOR, NO AR E NO REMOTO -- `5d31530f..024608c7`.** O pacote de PRODUTO (K8 + K5) pousou com o deploy num
ato so (L-107) e o range inteiro chegou ao remoto na **terceira** tentativa.
PROVA: `HEAD` = `024608c7` == `origin/main`, **0 commit a frente**; `pre-push: OK -- push liberado`; a suite
do commit empurrado deu **9.625 testes OK (skipped=42)** e o control-plane **22 OK**.
PROVA: o sha DEPLOYADO (`8c3035bc`) nao e a ponta (`024608c7`), e isso esta certo --
`git diff --stat 8c3035bc..024608c7 -- '*.py'` devolve **saida vazia**; no range sobraram so
`app/docs/LEIS.md` e `app/docs/TICKETS.md` (4 linhas). **Nenhum `.py` se moveu depois do deploy**, entao a
prova de casca e o smoke do K8 seguem valendo para o codigo que esta no ar.
AS TRES TENTATIVAS, a causa de cada uma em UMA linha:
1. `tickets_placar: ALARME` -- o topo derivado do TICKETS nasceu dentro da copia `wt-pousos`, onde nao havia
   carimbo de regua, e saiu sem o prefixo `regua OK (04/10 17:26)`. Curado pelo escritor CANONICO
   (`bin/pos_push.sh` -> `77eafa5f`), nunca a mao.
2. `test_lei_protege_sitio: RED` em **3 leis, todas minhas** -- as celulas PROTEGE da L-104 e da L-108
   carregavam PROSA, e a coluna cobra `arquivo.py::funcao` (gramatica em
   `bin/tests/test_lei_protege_sitio.sh:83`). A L-102 ja estava conforme: **o leitor que nao havia migrado
   era eu**. Curado em `024608c7`; a celula da L-108 fica VAZIA de proposito, com o motivo na coluna do
   leitor, porque o alvo dela e um `.md` e a coluna so aceita `.py`.
3. verde.
CONDUTA NOVA, e ela e barata: **a pasta `bin/tests/` INTEIRA roda na arvore viva ANTES do push** -- 61
selos, menos de 1 min, contra ~10 min de suite por rejeicao. As duas rejeicoes de hoje foram selo de HOST;
**nenhuma foi a suite**. (Memoria `pasta-de-selos-antes-do-push`.)
**A L-106 teve o primeiro caso MEDIDO no mesmo dia em que nasceu, e e meu**: a correcao do item (2) e um
commit que toca SO `app/docs/`. Nao e excecao e nao abre allowlist -- ela caiu DENTRO do range aberto do
push, e a **L-108 define o marco pelo PUSH**, nao pelo commit. O texto da L-106 nao se tocou.

**O191 PASSO 5 -- A CONDICAO DE ENTRADA ESTA MEDIDA, e ela LIBERA o passo.** Ele pediu que a O191 comecasse
provando que o dia sem par e REAL e nao defeito do juiz de geometria. Medido na SOMBRA, 6 dia-colab
(`logs/o134/o191_condicao_entrada_20261004.out`):

| colab | dia | celula casada | previsto | turnos com esse `data_turno` | `sem_turno` | ABERTO | soma propria |
|---|---|---|---|---|---|---|---|
| col200 | 2026-08-25 | sim | 420 | **0** | True | False | 60 |
| col235 | 2026-09-30 | sim | 420 | **0** | True | False | 61 |
| col250 | 2026-09-29 | sim | 660 | **0** | True | False | 240 |
| col277 | 2026-08-31 | sim | 660 | **0** | True | False | 280 |
| col302 | 2026-09-09 | sim | 660 | **0** | True | False | 300 |
| col852 | 2026-08-21 | sim | 660 | **0** | True | False | 305 |

Os seis tem celula casada com **4 marcos congelados** e previsto (420 ou 660 min), `eh_dia_trabalho=True`, e
**ZERO turno** com aquele `data_turno`. As batidas EXISTEM -- sao 11 no col200 -- e pertencem aos turnos
**VIZINHOS**: no noturno a entrada e 22:00 de d-1 e a saida 06:00 de d, entao o par cai em
`data_turno=24/08` e o dia 25/08 nao tem turno proprio. Contradicao do juiz (turno na janela E
`sem_turno=True`): **0 de 6**. Dia que a propria autoridade marca ABERTO: **0 de 6**. Veredito:
`ponto/turnos.py::realizado_do_dia` **nao erra** -- ele devolve `minutos=None sem_turno=True`, que e "nao
sei", e e a resposta certa. O passo 5 segue.
A SONDA ANTERIOR NAO SE PUBLICA, e o erro era meu: ela passava **DATAS** a `batidas_apuraveis`, cuja janela
e de **INSTANTES** (`ponto/turnos.py:1865`), e o censo truncava na meia-noite do ultimo dia com
`RuntimeWarning: naive datetime`. Corrigida em `logs/sombra/o191_condicao_entrada_20261004.py:68-71`, a
janela impressa agora le `24/08 00:00:00 -> 26/08 23:59:59`.

**ITEM (b): O `or 0` QUE EU IA CURAR ESTA MORTO -- a origem e outra linha.** O aval nomeava
`ponto/supra_juiz.py:131` (`real = dia.get('minutos_realizados') or 0`) como o ZERO a declarar. Censo dos
**tres** chamadores reais (`ponto/services/cartorio.py:585`, `ponto/management/commands/supra_juiz.py:190`,
`ponto/management/commands/emitir_furo_retroativo.py:82` -- o `classificar_dia` de `relatorios/furos.py` e
outra funcao com o mesmo nome e nao conta): **nenhum entrega `None` ali**, porque os dois produtores ja
colapsaram antes --
- `escala/services/leitor_celula.py:364` faz `int(ata.get('minutos_realizados') or 0)`, e o `base` da
  linha 337 ja nasce com `'minutos_realizados': 0`;
- `escala/utils.py:1334`, quando a autoridade devolve `sem_turno`, **nao colapsa para zero: ela FABRICA um
  numero** pelas celulas (`minutos_realizados_do_dia({'celulas': cel_por_data[d]}, marcos_out)`). Sao
  exatamente os `60, 61, 240, 280, 300, 305` da ultima coluna da tabela acima.
O comentario da propria linha confessa: *"ali o numero nao e da autoridade, o dia leva
`realizado_sem_turno`"*. **E essa e a SOMA PROPRIA que o passo 5 manda sair** -- S133 e LEI-AKITA 2, a
testemunha recalculando em vez de ler. Curar a 131 seria band-aid sobre um colapso que ja aconteceu
(LEI-AKITA 1): a origem e a **1334**, e a 131 so passa a precisar distinguir `None` DEPOIS que ela sair.
MEDIR ANTES DE TOCAR, e o motivo tem numero: esse `minutos_realizados` desce para `folha/export.py:698` e
`ponto/services/dia_pago.py:341`, os dois com `min(..., previsto)` -- **tirar a soma propria MOVE
DINHEIRO**. Entra por DINHEIRO-EM-COMPETENCIA-ABERTA: DIFF de frota publicado ANTES, reversao em `logs/`,
competencia exportada intacta com hash, PROVA depois. O precedente diz para esperar numero longe de zero:
o `e6_oraculo` curou o MESMO `or 0` no leitor dele em 02/10 e mediu que, dos 247 dia-colab de
`esp_zero_e6_trabalho` da competencia 09, **so 67 eram zero de verdade** -- 170 (1.681,0 h) tinham valor
positivo no `DiaPago`.

**LEI RESPONDIDA as ~14:xx -- (a), E ELA ALTERA A PROPRIA L-102.** O que segue e a pergunta como ela foi para a mesa, com os numeros que a decidiram; a resposta esta logo depois dela. (Nasceu sob PAREI-DE-LEI-NAO-DEVOLVE-TURNO; o adendo dele de 12:xx nomeia este
caso: *"Se a ata precisar guardar o numero por outro caminho, isso e pergunta de LEI: topo do RELATO com o
numero, e segue o proximo item"*). **DE QUAL AUTORIDADE SAI O NUMERO QUE A L-102 MANDA ROTULAR** --
`Sem turno pareado (ata Xh)`, *"nem se grava 0, nem se cala"* -- quando a soma propria de
`escala/utils.py` sair? A autoridade do pareamento **declina o numero**: `ponto/turnos.py:398` devolve
`RealizadoDoDia(minutos=None, sem_turno=True, ...)` quando nenhum turno tem `data_turno` naquele dia, e
nao ha campo nela que carregue "quanto havia". OS NUMEROS: **88 dia-colab** sem par na 09+10 (emp2+3+4),
**15 deles com numero hoje** (45,2 h) e **73 com zero**; dos 15, **2 sao a classe A** -- o numero e o turno
do dia VIZINHO -- e **13 a classe B**, onde o numero e o intervalo entre batidas orfas DAQUELE dia. Os tres
caminhos que eu enxergo estao **todos barrados por lei existente**, e por isso nao escolho: (a) a palavra
fica SEM numero -- contraria a L-102 literal, e as horas so restam visiveis nas BATIDAS da linha (Portaria
671); (b) nasce autoridade para *"quanto tempo ha entre as batidas deste dia, sem julgar turno"* -- **juiz
novo**, que o aval PROIBE e a TRAVA JUIZ-NOVO condiciona a `corte Ronald: juiz <nome> nasce` no CORTES.md;
(c) a soma propria fica **so para a ata** -- o pendente de `celula/precedencia` continua aberto, o passo 6
nao fecha (`tirar pendente com a impressao ainda no codigo` e PROIBIDO) e o placar nao vai a 15/20
(o teto virou **20** hoje as 14:1x, por lei dele -- L-100, secao O135 abaixo).
**O QUE A RESPOSTA ARRASTA, agora MEDIDO nos dois sentidos** (aval dele de 13:0x, secao O189 abaixo): se
for (a), o leitor que migra junto e `ponto/supra_juiz.py:131` (`real = dia.get('minutos_realizados') or 0`,
que colapsa `None` em 0). Com a linha distinguindo `None` de 0, na sombra (emp2+3+4, 09 e 10): **73 de 73**
dias de ata ZERO trocam de veredito e **6 dos 15** com numero; relavrando pela porta, **2 colaboradores
ENTRAM no TXT** (col302 emp2 09, col250 emp2 10, os dois do grupo dos **15**) e **0 SAEM** -- nenhum dos 73
move o TXT. E o preco aparece inteiro: os **73 perdem o protesto `BATIDA_ORFA_FORA_TOLERANCIA`**, porque o
ramo de `supra_juiz.py:~290` esta preso a `real == 0` -- com `None` troca-se um zero falso por **silencio
sobre uma orfa que existe**, exatamente nos dias definidos por ter uma. O numero antigo desta linha ("15
trocam, col454 sai do TXT") nao sai de cena: e o DIFF das 11:07, que mede o mundo **com** o `or 0`, e os
dois sentidos estao lado a lado na secao do O189.

**A RESPOSTA, literal:** *"(a). O dia sem par diz 'Sem turno pareado' SEM numero; as horas ficam nas
batidas da linha, e a L-102 passa a valer assim. O supra_juiz NAO passa a tratar None como 'nao sei': dia
sem turno conta como realizado ZERO DECLARADO (escrito, nao por acidente do `or 0`), para os 88 dias terem
o MESMO protesto de batida orfa."* Ela fecha os dois lados de uma vez e nenhum dos tres caminhos barrados
foi o escolhido inteiro: a TELA vai pelo (a) -- palavra sem numero, horas nas batidas da linha, Portaria
671 --, e o FECHAMENTO **nao** recebe o terceiro valor que eu media como consequencia do (a). O `None` nao
sobe ate o juiz como "nao sei": ele se escreve **ZERO por decisao** em `ponto/supra_juiz.py:131`, e e isso
que mantem os **88** dias protestando `BATIDA_ORFA_FORA_TOLERANCIA` pelo mesmo ramo -- o preco que eu havia
medido as 13:3x (os **73 perdendo o protesto**) **nao se paga**, porque a cura nao e distinguir `None`, e
declarar o zero. Nenhuma lei nova de juiz: a L-102 muda de rodape e nasce a **L-103** (as duas em
`app/docs/LEIS.md`). Registro em `app/docs/PROMPTS.md`. **Efeito aceito por ele**: 15 dia-colab trocam de
veredito, os 73 nao mudam, col454 sai do TXT da 09 na relavratura.
**A CONDICAO DE ENTRADA e DELE e e LITERAL** (LEI-AKITA 9), e **nada se aplica antes dela**: dos **15**,
listar os que tem **celula casada E previsto** (o exemplo dele e col250 29/09) e dizer se aquilo e **turno
ABERTO que o juiz da geometria devia ter achado** -- *"se for, e bug do juiz, cura na origem primeiro"*.
Entao a ordem da fatia (**O191**) nasce invertida de como eu a teria escrito: primeiro a autopsia dos 15,
depois a palavra e o zero; e a relavratura vai **dentro** do apply, com reversao em `logs/`.
**A ESTEIRA SEGUE:** passos 1-4 fechados com prova (secao das 12:xx); o passo 5 **nao espera mais** --
a lei chegou e ele vira a **O191**, que comeca pela CONDICAO DE ENTRADA dele, nao pela cura. O item
que andou em vez dele foi o **O135 TETO-20** (secao abaixo) -- nao por escolha de ordem, mas porque a
REGUA estava VERMELHA: as 13:45 o tripwire do `test_cortes_registrados.sh` mordeu o corte
`TETO-20-SEM-FAMILIA-SEM-CADASTRO` com **29 h em `recebido`** e FORA da pausa declarada (a `SAIDA` da
pausa e o export da 09, que nao o cobre), entao a suite nem rodava e NENHUM push saia -- nem o
`1cde743a` do LEI-PROTEGE-SITIO. As duas curas rapidas eram band-aid que a propria lapide do selo
nomeia (engordar o `COBRE=`, ou derivar o TOTAL das celulas ausentes), e a cura de ORIGEM era construir
a obra que o corte pede.
**`!` CUMPRIDO -- O135 TETO-20 ESTA NO AR** (`fb1c78ac`, ff + `deploy.sh --sem-migrate` num ato so as
**16:59**, 3 cascas provadas, `importerror_500=0`; secao das 16:59 abaixo).
PROVA: `logs/deploy.stamp::COMMIT` = `8c3035bc`; `fb1c78ac` (ponta do ff) e `48805bbf` (a fatia) sao os DOIS
ancestrais dele -- `git merge-base --is-ancestor` rc=0 nos dois --, e no ar
`app/core/contratos_estruturais.py:351` diz *"Nao ha constante `TOTAL`: o teto e uma"*.
O portao que o segurava era de
**ACESSO, nao de dinheiro** -- `bin/janela_auth.sh` recusando por `dow=7` sobre o UNICO sitio declarado que
a fatia tocava (`app/api/urls.py`, um `path()` read-only, **+2 linhas**, zero logica de auth) --, e eu nao
o havia forcado porque forcar portao e ATALHO e atalho esta na lista NUNCA PRE-APROVADO (L-009), e porque o
aval da manha era LITERAL sobre O142+O130+R4. O `!` dele das 14:4x resolveu o QUANDO e o aval das 16:4x
resolveu o COMO: **pela saida que JA EXISTE** (`SEM_JANELA_AUTH_MOTIVO`, `bin/deploy.sh:147`), nao por uma
segunda. A GUARDA 3 da esteira mede antes de atravessar e **recusa fora do escopo do aval** (se a guarda
falhou por ESTRUTURA, ou se nomear outro sitio de auth). O `--forcar` que o `bin/janela_auth.sh` **anuncia
e nao implementa** -- e que, lido como ref de git, sairia 1 de novo -- fica como fatia propria: **O185**,
por ordem do mesmo aval. O agendado de segunda 06:05 foi **desarmado com trilha**
(`logs/deploy_agendado/o135-teto20.desarmado`): com `teto20 == main` as quatro guardas passariam VACUAS e
ele publicaria a arvore viva daquele minuto, sem ninguem olhando. `deploys_agendados=0`.
E com isso a REGUA destravou: o corte `TETO-20-SEM-FAMILIA-SEM-CADASTRO` saiu de `recebido` (`no ar
fb1c78ac`) e `test_cortes_registrados.sh` responde `OK (63 cortes; 0 sem fatia)` -- a cura foi a obra que o
corte pedia, nao o `COBRE=` engordado.
**O PROXIMO que nao depende desta lei:** **PLACAR-ESTRUTURAL R6 item 3**.
O marco anterior esta NO AR: o `!` das 09:3x cumprido as **10:02:41** (`d1689254`).
PROVA: `d1689254` e ancestral de `logs/deploy.stamp::COMMIT` (`8c3035bc`) -- `git merge-base
--is-ancestor d1689254 8c3035bc` rc=0.
**FORA DO ATO, por LEI-AKITA 9 (escopo do aval e' LITERAL):** `raia-chamado` (`142238fc`) e
`raia-pdf` (`584265a9`) seguem sem merge -- o aval nomeia O142 + O130 + R4 e nada mais. O item O183
do BACKLOG previa a `raia-chamado` neste mesmo `!`, e eu NAO a estendi por conta propria: **uma
linha dele** ("inclui a chamado e a pdf") e elas entram no proximo ato.

### SEUS CORTES -- o que voce mandou e ainda nao esta no ar

> **Esta tabela e REGISTRO, nao cronologia**, e por isso ela nao vai para o `RELATO-ARQUIVO.md`: o
> gerador (`bin/gerar_cortes.py`) a REESCREVE entre os marcadores e
> `bin/tests/test_cortes_registrados.sh` a exige **aqui**. **A DIETA ja a levou DUAS vezes** -- 03/10 e
> 05/10, as duas vezes eu --, e nas duas quem pegou foi o selo. Na segunda a cura foi na ORIGEM: o
> ramo "marcador ausente" do gerador tentava uma ancora MORTA e, nao a achando, jogava o bloco no topo
> **calado**; agora o lugar e declarado (acima da 1a secao) e o motivo sai no stderr.

<!-- SEUS-CORTES:INICIO -->
### SEUS CORTES -- o que voce mandou e ainda nao esta no ar (45)

> **ALARME: 12 corte(s) com mais de 24 h em "recebido"** -- TROCA-DE-PLANTAO (407 h), FECHAMENTO-UI-PORTAS (389 h), CATALOGO-SAIDA-ANTECIPADA-DESCONTA (386 h), ESTEIRA-RETA-FINAL (385 h), ZUMBIDO (382 h), CARTAO-TOTAL-IGUAL-SOMA (379 h), CERT-VIGIA (368 h), CHAMADO-GANHA-CADASTRO (367 h), JUIZ-BATIDA-NASCE (367 h), JUIZ-ESCALA-NASCE (367 h), PERTO-DO-MOTOR-ESPERA-O-EXPORT (367 h), E3-CHAMADO-APOS-ARQUIVO-SIMPLES (367 h). Cada um vira Pauta de sistema para o DP ate sair de "recebido".

| corte | hora | idade | estado | fatia que consome |
|---|---|---|---|---|
| **ACESSO-NUNCA-EM-LOTE** | 2026-09-23 08:4x | 417 h | construindo | O4 + CREDENCIAL-POR-ESTADO |
| **COL200-DIA-DO-TURNO** | 2026-09-23 17:xx | 409 h | construindo | O9 PDF-E-O-ESPELHO |
| **CORTES-REGISTRADOS** | 2026-09-23 18:xx | 408 h | construindo | CORTES-REGISTRADOS |
| **TROCA-DE-PLANTAO** | 2026-09-23 18:3x | 407 h | recebido | O10 TROCA-DE-PLANTAO (porta no Resolver dia) |
| **NOITE-23-09** | 2026-09-23 18:4x | 407 h | construindo | NOITE-23-09 (infra) |
| **FABRICANTE-LE-O-BACKLOG** | 2026-09-23 20:1x | 406 h | construindo | FABRICANTE-LE-O-BACKLOG |
| **FECHAMENTO-UI-PORTAS** | 2026-09-24 13:xx | 389 h | recebido | O24 FECHAMENTO-UI-PORTAS |
| **JANELA-EXATA** | 2026-09-24 15:xx | 387 h | construindo | O27 JANELA-EXATA |
| **CATALOGO-SAIDA-ANTECIPADA-DESCONTA** | 2026-09-24 15:5x | 386 h | recebido | CATALOGO-SAIDA-ANTECIPADA-DESCONTA |
| **FILA-24-09-16-5X** | 2026-09-24 16:5x | 385 h | construindo | FILA-24-09-16-5X |
| **RELATORIO-ATESTADOS-FOTOS** | 2026-09-24 16:5x | 385 h | construindo | O29 RELATORIO-ATESTADOS-FOTOS |
| **AUSENCIAS-DRAWER-E-LOTE** | 2026-09-24 17:xx | 385 h | construindo | O30 AUSENCIAS-DRAWER-E-LOTE |
| **ESTEIRA-RETA-FINAL** | 2026-09-24 17:xx | 385 h | recebido | O31 ESTEIRA-RETA-FINAL |
| **ZUMBIDO** | 2026-09-24 20:xx | 382 h | recebido | O32 ZUMBIDO |
| **SUSPENSAO-DESCONTA-JORNADA** | 2026-09-24 22:3x | 379 h | construindo | SUSPENSAO-DESCONTA-JORNADA |
| **CARTAO-TOTAL-IGUAL-SOMA** | 2026-09-24 22:3x | 379 h | recebido | O33 CARTAO-TOTAL-IGUAL-SOMA |
| **CONTRATO-3-SEM-CONSUMIDOR-SAI** | 2026-09-25 00:xx | 378 h | esperando "!" | O35 CONTRATOS-14 |
| **CHAMADO-VARREDURA-NAO-JULGA** | 2026-09-25 00:xx | 378 h | RESPONDIDO 03/10 09:5x pelo PAPEL-PRAZO-NASCE (4a opcao: nasce o papel prazo) | O35 CONTRATOS-14 |
| **TETO-DA-MATRIZ-E-21** | 2026-09-25 00:xx | 378 h | esperando "!" | O35 CONTRATOS-14 |
| **JUIZ-DE-BATIDA-E-DE-ESCALA** | 2026-09-25 00:xx | 378 h | esperando "!" | O35 CONTRATOS-14 |
| **PERTO-DO-MOTOR-E-DO-JUIZ-DE-TURNO** | 2026-09-25 00:xx | 378 h | esperando "!" | O35 CONTRATOS-14 |
| **CERT-VIGIA** | 2026-09-25 09:4x | 368 h | recebido | CERT-VIGIA |
| **K8-COMPETENCIA-NAO-E-MES-CIVIL** | 2026-09-25 09:2x | 368 h | construindo | O40 K8-COMPETENCIA-NAO-E-MES-CIVIL |
| **ESTEIRA-SECA-1-E-2-AGORA** | 2026-09-25 10:3x | 367 h | construindo | O42 ESTEIRA-SECA-25-09 |
| **EXPORTADO-SEM-FRONTEIRA** | 2026-09-25 10:3x | 367 h | construindo | O44 ARQUIVO-SIMPLES v2 |
| **PASSIVO-TRANCADA-E-HISTORIA** | 2026-09-25 10:3x | 367 h | construindo | O44 ARQUIVO-SIMPLES v2 item 7 |
| **CHAMADO-GANHA-CADASTRO** | 2026-09-25 11:0x | 367 h | recebido | O35 CONTRATOS-14 |
| **JUIZ-BATIDA-NASCE** | 2026-09-25 11:0x | 367 h | recebido | S-BATIDA |
| **JUIZ-ESCALA-NASCE** | 2026-09-25 11:0x | 367 h | recebido | S-ESCALA |
| **PERTO-DO-MOTOR-ESPERA-O-EXPORT** | 2026-09-25 11:0x | 367 h | recebido | O35 CONTRATOS-14 |
| **E3-CHAMADO-APOS-ARQUIVO-SIMPLES** | 2026-09-25 11:0x | 367 h | recebido | E3-CHAMADO |
| **JUIZES-TRES-ASSINATURAS** | 2026-10-03 05:30 | 180 h | construindo | registro em `app/docs/CORTES.json` (03/10 08:4x) -- a TRAVA cai de 2 para 1 FALHA. O `batidas_apuraveis` e o `escala_vigente` entram em `app/core/juizes.py` nos itens 6 e 3 da ordem de 08:13, cada um com o censo do seu ponto |
| **ESPINHA-ANTES-DA-UI** | 2026-10-03 08:13 | 177 h | construindo | O134 ESPINHA-ANTES-DA-UI (ordem da fila 1) + O133 CLEAR-NO-MARCO na fila 2 |
| **PAPEL-PRAZO-NASCE** | 2026-10-03 09:5x | 176 h | registrado -- lei L-101, obra O139; censo dos 27 a medir antes de mover um nome | O139 PAPEL-PRAZO |
| **HOLERITE-MES-CIVIL** | 2026-10-04 17:5x | 144 h | construindo na raia `k5-encerrada` -- o corte RATIFICA `6a350aa9`, que ja tirou os dois sitios com nota MEDIDA (08/2026, a unica competencia com holerite publicado: 16 de 19 admitidos 21-31/08 TEM holerite de 08, contra 1 de 17 demitidos 21-31/07). Falta a segunda frase dele -- a CONDICAO de que cada conforme depende, na forma do `_A14 CURADO` -- e o teto dos dois contratos, que as duas raias deixaram no numero do main. | PLACAR-ESTRUTURAL R6 item 3, raia `k5-encerrada` (`6a350aa9`) |
| **RAIA-VERDE-POUSA** | 2026-10-04 19:2x | 142 h | lei L-105 escrita e a conduta vale DESDE JA: o pacote de pouso em curso se separou no mesmo turno -- K8 e K5 pousam como PRODUTO (`02391558`), o CERT-AST sai para pouso proprio porque e INSTRUMENTO (cria `bin/suite_nucleo.sh` e o selo `test_nucleo_tem_porta.sh`). Os itens (3) contador no ESTADO, (4) selo de host no pre-push e (5) veredito das raias velhas ficam na FILA 2, depois da CELULA-TURNO-FECHA e da O145, por ordem dele | o pouso de produto de 04/10 19:3x (K8+K5) e a lei no LEIS.md, no commit do marco |
| **DOCS-NO-MARCO** | 2026-10-04 19:2x | 142 h | lei L-106 escrita; conduta desde ja. O selo que a cobra no pre-push (push com commit so de app/docs/ alem do derivado = VERMELHO, com RED nos DOIS sentidos) e o item (4) e fica na fila 2. PROIBIDO allowlist de commit de docs, e PROIBIDO contar como marco o que nao fechou item | a lei no LEIS.md + a linha na CLAUDE.md 7b, no commit do marco de 04/10 19:3x |
| **REFERENCIA-E-A-LEI** | 2026-10-05 00:3x | 137 h | lei L-110 escrita, no marco da O191 (L-106: docs viajam com o codigo). A lei REVOGADA foi desfeita no mesmo marco e nos quatro sitios em que ja havia entrado: linha do LEIS.md, mapeamento CORTES-que-viraram-lei, entrada do CORTES.json e a LEI-AKITA 13 do CLAUDE.md. O contador cravado do test_lei_akita.sh FICA em 13, porque a lei nova ocupa a mesma linha 13 -- e o rotulo dele, que dizia 12 em texto fixo, passou a derivar do $N medido | a lei no LEIS.md + a LEI-AKITA 13 na CLAUDE.md (com o contador do selo junto) + esta linha, no commit do marco da O191 |
| **SITIO-SEM-CHAMADOR-AINDA-RESPONDE** | 2026-10-05 09:0x | 129 h | lei L-111 escrita e APLICADA no mesmo marco (CELULA-TURNO-FECHA): a funcao de 03/08 foi apagada com lapide, os 6 testes de test_realizado_intervalo.py foram com ela, PENDENTES['turno/marcos'] ficou () com a vaga nomeada e a celula (turno/marcos x um juiz por pergunta) ficou verde -- contratos_estruturais 13/20 -> 14/20, medido por verdes(). O RED veio do selo da casa (test_MORDE_pendente_curado_sai_da_lista), e nasceu um selo de EXISTENCIA por AST ao lado do de CHAMADA | CELULA-TURNO-FECHA (item 1 da ordem dele de 05/10 09:0x) -- a lei no LEIS.md e esta linha viajam no commit do marco (L-106) |
| **VINCULO-INICIO-DECLARADO** | 2026-10-05 10:4x | 127 h | lei L-112 escrita; vale como CONDUTA desde ja. A obra e a O207 e entra ATRAS da O204 e A FRENTE dos BOs de tela (posicao literal do aval). PRONTO = RED com os 6 casos + commit + deploy + print da tela com a terceira opcao num colab de teste; sem isso = INCOMPLETO com lista. PROIBIDO: segunda porta; regra de data propria na tela; tirar o piso ou a trava de competencia da porta; fallback; mexer em vigencia ja gravada | O207 (a terceira opcao na tela) -- a lei no LEIS.md + as linhas no PROMPTS.md + o item no BACKLOG |
| **W12X36-HPD** | 2026-09-24 14:xx / 16:5x | 0 h | construindo | O26 W12X36-HPD |
| **FECHAMENTO-ONLINE** | 2026-09-20 21:0x (corte original, NAO registrado na epoca) / reafirmado 2026-09-25 12:0x | 0 h | recebido | O48 FECHAMENTO-ONLINE |
| **JUIZ-DIA-ENCERRADO-NASCE** | 2026-10-10 18:0x | 0 h | recebido -- a autoridade esta AUTORIZADA, nao encomendada: o aval termina em 'Segue a fila', entao a ORDEM VIVA segue O48/O219 e o juiz nasce quando a vez dele chegar. O que este registro faz AGORA e deixar a assinatura em caixa: a TRAVA JUIZ-NOVO (bin/tests/test_juiz_novo_tem_corte.sh) exige a frase literal no CORTES.md no momento em que o nome entrar em app/core/juizes.py, e sem ela a fatia nao pousa. PRONTO = um escritor so do predicado, flip_automatico e supra_juiz LENDO dele (nenhum recalcula 'dia < hoje' por conta), universo do cron medido ANTES e DEPOIS com o mesmo numero, e o registro em core/juizes.py no mesmo commit. PROIBIDO: teto por DATA em sitio novo; segundo predicado; mudar o universo do cron a pretexto da troca | O248 JUIZ-DIA-ENCERRADO (item de OBRAS neste turno) -- atras de O48/O219 |
| **JUIZ-PAR-COMPLETO-NASCE** | 2026-10-10 18:0x | 0 h | recebido -- AUTORIZADA e nao encomendada, igual a irma: 'Segue a fila' mantem a ORDEM VIVA em O48/O219. A assinatura fica em caixa para a TRAVA JUIZ-NOVO, que so morde no commit em que o nome entrar em app/core/juizes.py. A ordem interna e DELE e e literal: o juiz nasce ANTES, e so DEPOIS o detectar_par_relampago muda de papel -- nao se encosta no emissor que RETRATA batida (cron 06:26, --apply --retratar) antes de a autoridade existir. PRONTO = um juiz so de 'o par esta completo?', o relampago LENDO dele, universo do cron igual antes e depois, registro em core/juizes.py no mesmo commit. PROIBIDO: juiz do par dentro do emissor; mudar o que o relampago retrata nesta fatia; universo novo | O249 JUIZ-PAR-COMPLETO (item de OBRAS neste turno) -- atras de O48/O219 |
| **O198-ABRE-NA-RAIA** | 2026-10-10 18:1x | 0 h | recebido e ABERTO no mesmo turno -- este e o unico dos tres avais de 18:0x/18:1x que manda COMECAR, e o que ele abre e a raia, nao a principal. RATIFICA a ordem que o BACKLOG ja declarava nas linhas 32-37 (O206 -> O207 -> O199 -> O198 -> GEO-SILENCIO-DE-PING -> O197 -> O44 itens 2-8): a O199 pousou e a O198 e a vez. O QUE A O198 FAZ E PARA: publica quantas faltas aprovadas em dia sem jornada existem na frota (09 e 10) e PARA ai -- a cura da porta vem depois do censo. O ! dele continua literal e NAO foi alargado por este aval: a Ausencia ja aprovada nao se apaga nem se altera. 'A principal nao para' e a clausula que impede este aval de virar troca de vez: a ORDEM VIVA segue O48/O219 e a raia anda ao lado | O198 FALTA-EM-DIA-SEM-JORNADA na raia wt-bos (c44c6c80, raia-bos), depois GEO-SILENCIO-DE-PING, O197 e O44 itens 2-8 |
<!-- SEUS-CORTES:FIM -->

## DEPLOYS AGENDADOS

- 12/10 06:08 deploy agendado o44-noar, fatia o44-deploy: rc=0 -- == O44-NO-AR  publicado 12/10 06:08:15 carimbo=4c9cad9306cf893331cf88c864ecd01ad9582227 FIM
- 08/10 06:08 deploy agendado o221-pouso3, fatia o221-pouso3: rc=0 -- 08/10 06:08:15 O221 pouso 3 NO AR em 1f3d616f. Falta so o push, que e da mao. FIM
