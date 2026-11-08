# CODEX-ESTADO

> Ultima resposta processada: **08/11/2026 10:41** · publicado em **08/11/2026 10:41** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 08/11 10:41 · sessao 16d8ac42b6cf · CENSO-ANTES-DA-VAGA-HOOK · leitura · origem=Code

**O contrato ainda não está implementado nesta árvore:** `ESPERANDO: censo <OBRA> | Codex` retorna **“produtor nao declarado em 1 palavra”**; o parser atual exige produtor único e campos `pid=`/`arquivo=`. (`bin/hook_stop_fila1.py:658–674`)

**(i) Identidade em `/proc`**

Depois da passagem pelo shebang, já executando o corpo do wrapper:

| Chamada | `/proc/<pid>/exe` | `argv[0]` | `argv[1]` |
|---|---|---|---|
| `bin/codex_pergunta.sh ...` | Executável do Bash | `bash` | `bin/codex_pergunta.sh` |
| `bash bin/codex_pergunta.sh ...` | Executável do Bash | `bash` | `bin/codex_pergunta.sh` |

A primeira forma usa `#!/usr/bin/env bash`; a segunda explicita esse interpretador. O caminho absoluto concreto de `exe` nesta execução é **[não verificado]**. (`bin/codex_pergunta.sh:1`)

Consequência para o hook: sendo `exe` um interpretador, `_processo_casa` examina **`argv[1]`**, não exige que `exe` seja o `.sh`. Produtor declarado com caminho absoluto compara o caminho resolvido usando o `cwd` do processo; produtor sem barra compara apenas o basename. (`bin/hook_stop_fila1.py:511–520`)

**(ii) `CODEX_OBRA`**

**É entrada do wrapper, não um export posterior:** o script lê `CODEX_OBRA` para `OBRA` e recusa valor vazio. O selo fornece `CODEX_OBRA` pelo `env` antes de executar o wrapper; nessa chamada, portanto, a variável já entra no ambiente inicial do processo. (`bin/codex_pergunta.sh:80–85`; `bin/tests/test_codex_pergunta.sh:111–113`)

O hook atual não lê esse ambiente: `_prova_pid` consulta `stat`, `exe` e `cmdline`, sem conferir `CODEX_OBRA`. (`bin/hook_stop_fila1.py:534–550`)

**(iii) Furos e limites**

- **Outro usuário:** a exigência de mesmo UID ainda não existe em `_prova_pid`. Falha de leitura bloqueia aquela prova, mas não há comparação explícita de UID. (`bin/hook_stop_fila1.py:523–550`)
- **Wrapper esperando vaga/flock:** processo vivo não prova Codex iniciado. O wrapper espera no gate, nos slots e na trava da resposta antes da chamada ao Codex. Já a vaga exclusiva do construtor usa `flock -n`: ocupada, recusa imediatamente. (`bin/codex_pergunta.sh:341–369`; `bin/codex_pergunta.sh:401–402`; `bin/codex_pergunta.sh:232–233`)
- **Wrapper de outra sessão:** excluir ancestrais não vincula o produtor à sessão principal. `_ancestrais` monta somente a cadeia de pais do hook; `_prova_pid` não compara sessão nem exige descendência do chamador. (`bin/hook_stop_fila1.py:474–491`; `bin/hook_stop_fila1.py:529–550`)
- **Obra com prefixo comum:** a igualdade exata exigida pelo contrato elimina esse casamento por prefixo; sua implementação é **[não verificado]**. Hoje não há comparação de obra nessa prova. (`bin/hook_stop_fila1.py:534–550`)
- **Papel e fase:** o wrapper admite cinco papéis; conferir somente wrapper e obra não distingue `censo` dos demais. Além disso, ele continua vivo fazendo pós-processamento depois que o Codex retorna. (`bin/codex_pergunta.sh:81–85`; `bin/codex_pergunta.sh:404–448`)
- **“Terminou → volta a bloquear” tem limite:** perder a prova apenas continua o fluxo normal do Stop; fila vazia, teto, B2 e exceção ainda possuem caminhos de liberação. (`bin/hook_stop_fila1.py:1231–1246`; `bin/hook_stop_fila1.py:1284–1306`)

**(iv) Outros leitores do topo do RELATO**

Primeiro, **“linha 2” não é posição física para o hook**: ele pula título e brancos e usa a primeira linha útil. A segunda linha útil integra o bloco destinado ao B3. (`bin/hook_stop_fila1.py:400–408`; `bin/hook_stop_fila1.py:415–440`)

| Leitor | Comportamento |
|---|---|
| **Disjuntor B3** | Lê fotografia consistente do documento e usa o mesmo `_bloco_do_topo`, com duas linhas úteis. (`bin/disjuntor.py:662–689`) |
| **`handoff_sessao.sh`** | Percorre o topo até `##`, mas seleciona somente `PAREI`, `lei:` e `!:`; omite ESPERANDO e consulta apenas os vereditos PAREI. (`bin/handoff_sessao.sh:92–123`) |
| **`relato.sh`** | Submete o documento ao checador de afirmações e ao raspador/publicador; não julga prova de ESPERANDO nessas etapas. (`bin/relato.sh:54–61`; `bin/relato.sh:103–107`) |
| **Alarme de sessão ociosa** | Procura `PAREI:` nas primeiras 40 linhas; ESPERANDO não satisfaz esse reconhecimento. (`bin/alarme_sessao_ociosa.py:119–127`) |
| **Selo ESPERANDO** | Monta estado na linha física **3** e verifica ALLOW/NOPROVA; cobre PID vivo, morto, outro programa e ausência de prova. (`bin/tests/test_hook_esperando_com_prova.sh:62–105`) |
| **Selo de orçamento** | Exige que ESPERANDO provado suspenda o disparo do orçamento; morto o produtor, cobra novamente o orçamento. (`bin/tests/test_orcamento_contexto.sh:220–233`) |
| **Selo do disjuntor** | Exercita o recorte compartilhado e inclui mutante que conserva somente a primeira linha útil. (`bin/tests/test_disjuntor.sh:845–865`; `bin/tests/test_disjuntor.sh:1658`) |

## 08/11 10:40 · sessao 55f94ed382ed · - · - · origem=Ronald (tmux)

Adicionais:

- O selo espiona chamadas a `H._bloco_do_topo`, verificando leitura sob a trava: `bin/tests/test_disjuntor.sh:845-865`.
- O mutante que conserva só a primeira linha útil deve reprovar: `bin/tests/test_disjuntor.sh:1658`. Há mutantes de fotografia parcial: `bin/tests/test_disjuntor.sh:1659-1663`.
- `test_no_relato_tem_secao_pinada.py` conserva o topo inteiro por comparação `startswith(TOPO)`; não interpreta a linha 2: `app/core/tests/test_no_relato_tem_secao_pinada.py:31-35`, `app/core/tests/test_no_relato_tem_secao_pinada.py:51-60`. Sua leitura posicional verifica a última linha útil: `app/core/tests/test_no_relato_tem_secao_pinada.py:68-69`.

Nenhum leitor adicional da linha física 2 identificado na varredura, incluindo `.claude`; ausência absoluta: [não verificado].

## 08/11 10:39 · sessao 55f94ed382ed · - · - · origem=Ronald (tmux)

Leitores e efeitos confirmados:

- **O hook não lê “linha física 2”**: `_linha_do_topo()` usa a primeira linha útil; `_bloco_do_topo()` pula brancos e `# RELATO`, tira crases iniciais e retorna até duas linhas úteis. A segunda útil existe para B3. (`bin/hook_stop_fila1.py:400-408`, `bin/hook_stop_fila1.py:415-440`)

- **Disjuntor B3** lê o RELATO inteiro duas vezes, confere identidade/mtime/tamanho e bytes; recorta as duas linhas úteis pelo mesmo `H._bloco_do_topo()`. Portanto continua sendo consumidor do painel, sem uma regra paralela de recorte. (`bin/disjuntor.py:662-689`)

- **Handoff** lê o topo até a primeira seção `## `, mas seleciona somente `PAREI`, `lei:` e `!:`. `ESPERANDO: censo ... | Codex` fica fora da lista; os vereditos mostrados chamam `_parei_do_em_curso()` e `_relato_parou()`, não a prova ESPERANDO. (`bin/handoff_sessao.sh:92-123`)

- **`relato.sh`** entrega o documento ao checador de afirmações de ato e depois ao raspador/publicador; nesse fluxo não interpreta o estado ESPERANDO. O checador percorre linhas procurando títulos/negritos com verbos de ato e prova próxima. (`bin/relato.sh:54-61`, `bin/relato.sh:103-107`, `bin/relato_afirma_com_prova.py:30-50`)

- **Alarme de sessão ociosa** lê as primeiras 40 linhas e só procura a substring `PAREI:`. A linha ESPERANDO não desarma esse reconhecimento. (`bin/alarme_sessao_ociosa.py:119-127`)

Selos relevantes:

- `test_hook_esperando_com_prova.sh` constrói RELATO com cabeçalho na linha 1, branco na 2 e estado na 3; exige liberação pelo motivo `ALLOW ESPERANDO com prova`, ou bloqueio com `siga: ITEM-VIVO` e motivo `NOPROVA`. (`bin/tests/test_hook_esperando_com_prova.sh:62-89`)
- O selo atual cobre produtor vivo, PID morto, outro programa e ausência de prova, usando a sintaxe `produtor | pid=`. (`bin/tests/test_hook_esperando_com_prova.sh:93-105`)
- Também exige que prova de ato seja conferida antes de ESPERANDO e que matar o produtor revogue a liberação. (`bin/tests/test_hook_esperando_com_prova.sh:186-192`)
- `test_orcamento_contexto.sh` exige que ESPERANDO com produtor vivo suspenda o disparo de orçamento e libere pelo motivo correto. (`bin/tests/test_orcamento_contexto.sh:220-230`)
- `test_disjuntor.sh` testa diretamente os juízes compartilhados de leitura do topo, inclusive leitura parcial após a primeira linha útil. (`bin/tests/test_disjuntor.sh:748-765`)
- O selo do handoff exercita perguntas de lei depois de 14 linhas RESPONDIDA, protegendo o alcance até a seção, sem testar censo. (`bin/tests/test_handoff_sessao.sh:139-148`)

## 08/11 10:35 · sessao 950169722c2a · FIO-E0-V4-E3 · censo · origem=Code

**O aval não confere como lista completa.** Dois pontos objetivos: não encontrei chamadas de produção de `o_que_segura`; e `explicador` e `folha/previa` são respondedores, embora estejam entre os nove nomes tratados como simples consumidores de C15. (`app/folha/fora_da_folha.py:203`, `app/chamados/services/explicador.py:163`, `app/folha/previa.py:95`)

| Base da conferência | Limite |
|---|---|
| AST dos arquivos Python da cópia, fora de testes, com conferência dos corpos, imports e aliases; chamadas concretas discriminadas abaixo. | Uso dinâmico externo à cópia: **[nao verificado]**. | [nao verificado]
| Aval e e2.r comparados pelo conteúdo colado no pedido. | Arquivos locais dos anexos e correspondência desta cópia com HEAD `94d41ea`: **[nao verificado]**. |
| `JUIZES.md` é lookup; o próprio documento manda conferir o vivo e aponta `core/juizes.py` como registro. | `app/docs/JUIZES.md:7`, `app/docs/JUIZES.md:11`, `app/docs/JUIZES.md:14`. |
| Resultados abaixo descrevem código existente. | Quantidades atuais, ocorrências individuais em produção e comportamento visual do APK: **[nao verificado]**. |

**1. Conferência do aval**

| Item do aval | Resultado contra o vivo | Chamadores, unidade e fonte |
|---|---|---|
| `vigia_fio.fio_do_dia:156` | **Confere.** | Chamado por `censo` em `app/chamados/services/vigia_fio.py:414` e `app/chamados/services/vigia_fio.py:417`. Devolve booleano sobre a coleção recebida; **lê C15**, sem derivar vitalidade própria. `app/chamados/services/vigia_fio.py:166`. |
| `vigia_fio.censo_alcance` | **Confere.** Definição em `app/chamados/services/vigia_fio.py:542`. | Chamado pelo command em `app/chamados/management/commands/vigiar_fio_morto.py:173`. Responde por **(colab,dia)** retido, com rota, papel e causa; lê retenção, C17 e dono fora do fio, depois deriva contadores/causas. `app/chamados/services/vigia_fio.py:585`, `app/chamados/services/vigia_fio.py:635`, `app/chamados/services/vigia_fio.py:654`, `app/chamados/services/vigia_fio.py:699`. |
| C17 `porta_alcancavel_do_dia:1341` | **Confere.** | Três chamadores diretos: `_porta`, `censo_alcance`, `beco_em_lote`. Devolve `None` ou `{rota,fila,verbos,fio}` por **célula/dia**. Compõe fila, perguntas C6 vivas presentes no app e verbos do painel. `app/folha/fora_da_folha.py:67`, `app/chamados/services/vigia_fio.py:635`, `app/escala/services/cadastro_realidade.py:488`, `app/chamados/juizes.py:1386`. |
| `folha._fio_do_dia:70` | **Confere.** | Três chamadas: `fora_da_folha`, `o_que_segura`, `dias_sem_fio`. Devolve `admin/colab/chamado/nada` por célula; **lê `fio` de C17**. `app/folha/fora_da_folha.py:77`, `app/folha/fora_da_folha.py:178`, `app/folha/fora_da_folha.py:221`, `app/ponto/services/pautas_dos_discordantes.py:77`. |
| `o_que_segura:203`, “6 chamadores” | **Não confere quanto aos seis.** O AST não encontrou chamada de produção da função nesta cópia. | O corpo existe e responde colaborador/competência com dias retidos, fio, destrava e chamados; compõe export, retenção e C17. Definição e retorno: `app/folha/fora_da_folha.py:203`, `app/folha/fora_da_folha.py:215`, `app/folha/fora_da_folha.py:229`. Chamador dinâmico externo: **[nao verificado]**. |
| `resolver_dia_oferecido:97` e `STATUS_QUE_PEDEM_DECISAO:94` | **Confere.** | Única chamada direta: `contexto_calendario`, em `app/colaboradores/services/calendario.py:769`. Booleano por dia; **deriva** último recurso + passado + status em `falta/irregular/aberto/sem_certidao`. `app/colaboradores/services/calendario.py:94`, `app/colaboradores/services/calendario.py:103`. |
| `perguntas_do_app:42 ← api/views:2552` | **Confere, mas faltam quatro chamadores.** | Também leem: `_no_app`, ficha, `medir`, `do_dia`. Unidade **pergunta**; lê `q_vivas` e C6 exato. `app/chamados/juizes.py:1319`, `app/colaboradores/services/ficha.py:166`, `app/chamados/services/pergunta_no_app.py:139`, `app/chamados/services/pergunta_no_app.py:162`, `app/chamados/services/pergunta_no_app.py:53`. |
| `pendentes_do_colaborador:1729 ← dois kernels` | **Confere, mas falta dashboard web.** | Três chamadas: `app/api/views.py:325`, `app/api/views_core.py:314`, `app/core/views.py:253`. Retorna **disputas abertas distintas** com pergunta C6 viva sem resposta; deriva o recorte da disputa e lê C6. `app/chamados/models.py:1736`, `app/chamados/models.py:1740`. |
| C11 `cadastro_realidade.beco_em_lote:442` | **Confere.** A chamada C17 está em **488**, não em 460. | Chamado em `app/escala/services/cadastro_realidade.py:439` e `app/escala/services/cadastro_realidade.py:573`. Devolve assinatura C11 por **(colab,dia)**; compõe célula indefinida, perguntas vivas, ausência de porta, âncora terminal, cadastro e dono. `app/escala/services/cadastro_realidade.py:477`, `app/escala/services/cadastro_realidade.py:488`, `app/escala/services/cadastro_realidade.py:515`, `app/escala/services/cadastro_realidade.py:530`. A linha 460 é docstring. |

**Censo complementar: respondedores que faltaram na lista**

Na coluna “natureza”, **lê** identifica delegação ao juiz; **compõe/deriva** identifica seleção, agregação ou resposta construída no próprio sítio. As fontes e chamadas estão discriminadas em cada linha.

| Sítio/função | Quem chama | Unidade e resposta | Natureza |
|---|---|---|---|
| `vigia_fio.censo` — `app/chamados/services/vigia_fio.py:318` | `vigiar_fio_morto:172`; `materializar_passivo_validado:203`, em `app/chamados/management/commands/` | Células/dias com fio vivo, morto ou dono fora do fio. | Compõe universo de chamados/células, lê C15 e dono; agrega. `app/chamados/services/vigia_fio.py:334`, `app/chamados/services/vigia_fio.py:417`, `app/chamados/services/vigia_fio.py:431`. |
| `dono_fora_do_fio` — `app/chamados/services/vigia_fio.py:274` | `censo:431`, `censo_alcance:654`, mesmo arquivo | **(colab,dia)** → decisão pendente, ausência aberta, escala não descreve dia, fio do admin ou `None`; opcional papel. | Deriva precedência entre oferta do calendário, ausência, estado da pergunta e lista administrativa. `app/chamados/services/vigia_fio.py:306`. |
| `ListaDoAdmin.dia_do_admin/dia_na_lista/papel_do_dia` — `app/chamados/services/vigia_fio.py:222`, `app/chamados/services/vigia_fio.py:230`, `app/chamados/services/vigia_fio.py:238` | Dono em `app/chamados/services/vigia_fio.py:312`; alcance em `app/chamados/services/vigia_fio.py:661` | Presença e papel por **(colab,dia)**. | Lê lista Cadastro×Realidade e reduz seus donos. `app/chamados/services/vigia_fio.py:216`, `app/chamados/services/vigia_fio.py:246`. |
| `resolver_dia_oferecido_nas_datas` — `app/colaboradores/services/calendario.py:117` | Vigia em `app/chamados/services/vigia_fio.py:271` | `{dia:bool}` para um colaborador. | Lê campo do calendário; dia ausente recebe falso. `app/colaboradores/services/calendario.py:132`, `app/colaboradores/services/calendario.py:135`. |
| `contexto_calendario` — `app/colaboradores/services/calendario.py:159` | `app/colaboradores/views.py:1946`; `app/colaboradores/services/drawer.py:121`; `app/relatorios/furos.py:160`; plural em `app/colaboradores/services/calendario.py:132` | Dias com estado, pendência e oferta de decisão. | Compõe status/último recurso, lê C6 e escreve `resolver_dia_oferecido`. `app/colaboradores/services/calendario.py:476`, `app/colaboradores/services/calendario.py:769`. |
| `cadastro_realidade.lista/dias_na_lista/um_dono_por_dia` — `app/escala/services/cadastro_realidade.py:787`, `app/escala/services/cadastro_realidade.py:1044`, `app/escala/services/cadastro_realidade.py:985` | Tela `app/escala/views.py:487`; vigia `app/chamados/services/vigia_fio.py:216`, `app/chamados/services/vigia_fio.py:217`; lista chama uniformização em `app/escala/services/cadastro_realidade.py:885` | Lista administrativa e dono por **(colab,dia)**. | Compõe assinaturas, incorpora C11 e lê `InsumosDoDono.dono`. `app/escala/services/cadastro_realidade.py:864`, `app/escala/services/cadastro_realidade.py:1009`, `app/escala/services/cadastro_realidade.py:1026`, `app/escala/services/cadastro_realidade.py:1064`. |
| `montar_chamados_fio` — `app/chamados/services/fio.py:160` | Colab/admin/modal em `app/chamados/views.py:569`, `app/chamados/views.py:677`, `app/chamados/views.py:889`; parcial em `app/chamados/services/fio.py:442` | **Chamados**, com perguntas e indicador de ação pendente. | Lê janela, C6/`.viva` e prova de ação possível. **`tem_pergunta_viva` não é apenas C15:** exige `.viva` e ausência de clique morto. `app/chamados/services/fio.py:171`, `app/chamados/services/fio.py:234`, `app/chamados/services/fio.py:279`. |
| `chamados_visiveis_colab` — `app/chamados/services/janela_colab.py:49` | API `app/api/views.py:1016`; fio `app/chamados/services/fio.py:171`; arquivamento `app/chamados/views_arquivar.py:195` | QuerySet de **chamados** da lista do colaborador. | Deriva janela/destinos/silêncio/módulos; lê **pré-filtro SQL `q_vivas`**, sem C6 exato nesse corpo. `app/chamados/services/janela_colab.py:63`, `app/chamados/services/janela_colab.py:67`. |
| `explicar_pergunta/por_que_nao_fecha` — `app/chamados/services/explicador.py:68`, `app/chamados/services/explicador.py:108` | Explicador em `app/chamados/services/explicador.py:133`; montador em `app/chamados/services/fio.py:308`; modal mostra em `app/templates/chamados/partials/modal_fio.html:432` | **Pergunta:** estado/dono/ação; **disputa:** `pode_fechar/resumo/itens`. | Lê `.estado`, materialização de fato e C15; deriva descrição e ações. `app/chamados/services/explicador.py:72`, `app/chamados/services/explicador.py:93`, `app/chamados/services/explicador.py:137`, `app/chamados/services/explicador.py:163`. |
| `desenlace` — `app/chamados/services/desenlace.py:44` | Fio em `app/chamados/services/fio.py:399`; modal em `app/templates/chamados/partials/modal_fio.html:118` | **Chamado** → verbo, rótulo, estado, ação ou `None`. | Compõe natureza, explicação, filhos e porta própria. `app/chamados/services/desenlace.py:50`, `app/chamados/services/desenlace.py:57`, `app/chamados/services/desenlace.py:62`. |
| `montar_contexto_detalhe` — `app/chamados/services/acoes_chamado.py:327` | `app/chamados/views.py:551` | Detalhe do **chamado** e perguntas vivas. | Lê C6 para filtrar perguntas da disputa aberta. `app/chamados/services/acoes_chamado.py:335`, `app/chamados/services/acoes_chamado.py:348`. |
| `contexto_drawer` — `app/colaboradores/services/drawer.py:17` | `app/colaboradores/views.py:2943` | Banner de motivos pendentes; fallback “N chamados de furo aguardando colaborador”. | Primeiro lê perguntas vivas; fallback **deriva por `VIVOS` e quatro módulos**, sem conferir C6. `app/colaboradores/services/drawer.py:78`, `app/colaboradores/services/drawer.py:88`, `app/colaboradores/services/drawer.py:92`. |
| `relatorios_disputa.classificar/coletar` — `app/chamados/relatorios_disputa.py:11`, `app/chamados/relatorios_disputa.py:26` | Coleta em `app/chamados/relatorios_disputa.py:47`; tela `app/colaboradores/views.py:2997`; command `app/chamados/management/commands/relatorio_pendencias_disputa.py:15` | **Disputa** → sem pergunta, pronta fechar, aguarda colab ou supervisão. | Lê C6; deriva categoria pela existência de resposta. `app/chamados/relatorios_disputa.py:18`, `app/chamados/relatorios_disputa.py:21`. |
| `perguntas_pendentes_do_colab/aviso_texto_livre` — `app/chamados/services/aviso_pendencia.py:7`, `app/chamados/services/aviso_pendencia.py:16` | Cobrança em `app/chamados/services/cobranca.py:48`; API `app/api/views.py:997`, `app/api/views.py:2220`; web `app/chamados/views.py:1120` | Lista de **perguntas** e aviso com datas que continuam aguardando resposta. | Lê `q_vivas` + C6 + ausência de `respondida_em`; deriva texto. `app/chamados/services/aviso_pendencia.py:13`, `app/chamados/services/aviso_pendencia.py:20`. |
| `checar_cobranca` — `app/chamados/services/cobranca.py:75` | `cobrar_colaborador`, em `app/chamados/services/cobranca.py:155`; referência de intake em `app/core/intake.py:68` | Quantidade de **perguntas**, datas, disputas, prazo e canal. | Lê aviso de pendência; deriva agregados/prazo. `app/chamados/services/cobranca.py:82`, `app/chamados/services/cobranca.py:87`. |
| `_calados/linhas_para_a_pauta` — `app/chamados/services/cobranca.py:221`, `app/chamados/services/cobranca.py:251` | Pauta em `app/escala/services/fase_12x36.py:272`; redução em `app/chamados/services/cobranca.py:257` | **Pessoas** cobradas/caladas e sem canal. | Deriva com `q_vivas`, disputa aberta e pergunta sem resposta; **não chama C6 exato** nesse corpo. `app/chamados/services/cobranca.py:229`, `app/chamados/services/cobranca.py:248`. |
| `pergunta_no_app.retencao/do_dia/medir` — `app/chamados/services/pergunta_no_app.py:65`, `app/chamados/services/pergunta_no_app.py:152`, `app/chamados/services/pergunta_no_app.py:119` | `dia_do_colab:109`; commands `app/chamados/management/commands/chamados_vivos_sem_pergunta_no_app.py:14`, `app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:21` | Por **chamado/dia**: pergunta no app, causa e dono da retenção. | Lê lista do app, fábrica, ata, cobertura e lista admin; deriva taxonomia. `app/chamados/services/pergunta_no_app.py:77`, `app/chamados/services/pergunta_no_app.py:88`, `app/chamados/services/pergunta_no_app.py:139`, `app/chamados/services/pergunta_no_app.py:165`. |
| `dia_do_colab/_veredito_do_dia` — `app/ponto/services/dia_do_colab.py:6`, `app/ponto/services/dia_do_colab.py:112` | API `app/api/views_mensageria.py:1970`; ficha `app/colaboradores/services/ficha.py:280`; frase em `app/ponto/services/dia_do_colab.py:102` | **(colab,dia)**: marcos, perguntas e frases “Faltou…”/“esperando resposta ou decisão”. | Lê ata, C6 e pergunta no app; **deriva** frase pelos marcos apagados e perguntas vivas. `app/ponto/services/dia_do_colab.py:30`, `app/ponto/services/dia_do_colab.py:71`, `app/ponto/services/dia_do_colab.py:144`, `app/ponto/services/dia_do_colab.py:150`. |
| `ficha_do_colab` — `app/colaboradores/services/ficha.py:99` | Ficha lazy em `app/colaboradores/services/ficha.py:269`; copiloto em `app/colaboradores/services/ficha.py:275`, servido por `app/api/views_mensageria.py:1131` | **Chamados/perguntas** por pessoa, com dono e ações quantificadas. | Lê verbos, perguntas do app e respostas sem veredito; deriva seleção por texto e ações. `app/colaboradores/services/ficha.py:155`, `app/colaboradores/services/ficha.py:166`, `app/colaboradores/services/ficha.py:171`, `app/colaboradores/services/ficha.py:190`. |
| `fila_validar_classificada` — `app/chamados/services/validacao.py:865` | `app/chamados/views_cobrar.py:288`, `app/chamados/views_cobrar.py:349`, `app/chamados/views_cobrar.py:361` | **Perguntas** e **chamados distintos** por classe, idade e total. | Lê fila C6 de respondidas a validar; deriva classes/contagens. `app/chamados/services/validacao.py:874`, `app/chamados/services/validacao.py:882`, `app/chamados/services/validacao.py:884`. |
| `becos.classificar` — `app/chamados/services/becos.py:25` | Command `app/chamados/management/commands/fechar_becos_disputa.py:39`; evento em `app/chamados/services/becos.py:69` | **Disputa** → `None/fechavel/barrada`. | Deriva recorte de beco por abertura, resposta/destino e pai terminal; lê C7 para fechar. `app/chamados/services/becos.py:27`, `app/chamados/services/becos.py:33`. |
| `api_chamados_v2` — `app/api/views.py:975` | Rota `app/api/urls.py:73` | **Chamados** com perguntas da disputa, marcos e flag de regularização. | Monta perguntas a partir de **disputas abertas**, sem filtrar essa coleção por C6; lê projeção por marco separadamente. `app/api/views.py:1029`, `app/api/views.py:1043`, `app/api/views.py:1103`, `app/api/views.py:1158`. |
| `projecao_por_marco/dia_em_regularizacao/marco_aceito_unico` — `app/chamados/services/declaracao_texto.py:376`, `app/chamados/services/declaracao_texto.py:444`, `app/chamados/services/declaracao_texto.py:431` | Fio `app/chamados/services/fio.py:371`; API `app/api/views.py:1103`; wrapper em `app/chamados/services/declaracao_texto.py:501` | **Marco** → aceita/recusa; **dia** → todos recusados; marco único oferecido. | Lê faltantes da célula e identidade/guarda da porta; deriva projeção e reduções. `app/chamados/services/declaracao_texto.py:414`, `app/chamados/services/declaracao_texto.py:423`, `app/chamados/services/declaracao_texto.py:427`, `app/chamados/services/declaracao_texto.py:454`. |
| `montar_contexto_painel` — `app/chamados/services/painel.py:196` | `app/chamados/views.py:727`, `app/chamados/views.py:852`; lavrador `app/core/management/commands/lavrar_badge_navbar.py:39` | Lista/pílulas/contadores de **chamados**. | Lê `qs_na_fila` e partição de verbos; agrega por estado/verbo. `app/chamados/services/painel.py:210`, `app/chamados/services/painel.py:228`. |
| `qs_na_fila/particionar_verbos/creditos_perguntas` — `app/chamados/services/painel.py:158`, `app/chamados/services/verbos.py:30`, `app/chamados/services/raiox.py:49` | Painel acima; alimentação C17 em `app/chamados/juizes.py:1287`, `app/chamados/juizes.py:1290`; consumidores abaixo | **Chamado** → pertença à fila/verbo; crédito de pergunta ao chamado. | Fila deriva `COBRANCA` e silêncio; verbo lê crédito/natureza; crédito usa `q_vivas` e `veredito_lavrado`, **não C6 exato nesse corpo**. `app/chamados/services/painel.py:187`, `app/chamados/services/verbos.py:34`, `app/chamados/services/verbos.py:47`, `app/chamados/services/raiox.py:133`. |
| `coletar_lente` — `app/chamados/services/verbos.py:164` | `app/chamados/views.py:1480` | **Chamados** agrupados da pílula ativa. | Lê partição de verbos e agrega. `app/chamados/services/verbos.py:171`, `app/chamados/services/verbos.py:185`. |
| Navbar `_badge_staff/_badge_registrados/_badge_colab` — `app/core/context_processors.py:59`, `app/core/context_processors.py:131`, `app/core/context_processors.py:185` | `navbar_context`, que publica as lazies em `app/core/context_processors.py:154`, `app/core/context_processors.py:189` | Staff: **chamados que exigem admin**; segundo número: **fila**; colab: **chamados na fila dele**. | Staff lê lavrado, fallback partição e fallback fila; colab lê fila. `app/core/context_processors.py:84`, `app/core/context_processors.py:90`, `app/core/context_processors.py:93`, `app/core/context_processors.py:150`, `app/core/context_processors.py:187`. |
| Dashboard web — `app/core/views.py:253` | Render em `app/core/views.py:271`; badge em `app/templates/core/dashboard_colaborador_v2.html:79` | **Disputas** pendentes do colaborador. | Lê `pendentes_do_colaborador`. `app/core/views.py:253`. |
| `montar_situacional/_led` e `montar_linha_colab` — `app/colaboradores/services/situacional.py:65`, `app/colaboradores/services/situacional.py:56`, `app/colaboradores/painel_cell.py:11` | Telas em `app/colaboradores/views.py:2664`, `app/colaboradores/views.py:2801`; live-cell em `app/colaboradores/painel_cell.py:81` | **Colaborador** → LED vermelho se existe disputa aberta. | **Deriva existência de disputa aberta**, sem exigir pergunta C6 viva. Lote: `app/colaboradores/services/situacional.py:117`; pontual: `app/colaboradores/painel_cell.py:31`; cor: `app/colaboradores/services/situacional.py:58`, `app/colaboradores/painel_cell.py:68`. |
| `montar_painel_operacional` — `app/core/services/painel_op.py:38` | `app/core/views.py:607`, `app/core/views.py:632` | Número de **chamados por colaborador**. | Lê `qs_na_fila` e agrega. `app/core/services/painel_op.py:119`. |
| `api_mensageria_pendencias/sinais` — `app/api/views_mensageria.py:390`, `app/api/views_mensageria.py:427` | Rotas `app/api/urls.py:10`, `app/api/urls.py:11` | **Chamados** com verbo; totais por verbo. | Leem partição e derivam serialização/agregados. `app/api/views_mensageria.py:395`, `app/api/views_mensageria.py:421`, `app/api/views_mensageria.py:453`, `app/api/views_mensageria.py:460`. |
| `fila.por_verbo/idade_e_vazao/gestao` — `app/chamados/services/fila.py:53`, `app/chamados/services/fila.py:88`, `app/chamados/services/fila.py:145` | Redução em `app/chamados/services/fila.py:141`; APIs em `app/api/views_mensageria.py:668`, `app/api

[truncado em 20000 de 48406 caracteres]

## 08/11 10:28 · sessao 02956b4fd4e0 · - · - · origem=Ronald (tmux)

| Sítio confirmado fora de `tests/` | Chamadores de produção confirmados | Unidade e resposta | Deriva ou lê |
|---|---|---|---|
| `app/chamados/services/vigia_fio.py:156` `fio_do_dia` | `censo`, `app/chamados/services/vigia_fio.py:414`, `:417` | Booleano sobre perguntas recebidas: dia tem fio vivo. | Só lê C15 `fio_vivo`, `app/chamados/services/vigia_fio.py:165`. |
| `app/chamados/services/vigia_fio.py:274` `dono_fora_do_fio` | `censo :431`; `censo_alcance :654`, mesmo arquivo | Por `(colab,data)`: decisão pendente, ausência aberta, escala não descreve dia, fio do admin ou `None`; opcional papel. | Compõe, nesta ordem, `oferecido`, cobertura de ausência/ABERTAS, `.estado` + aceite de folga, presença/dono na lista; `app/chamados/services/vigia_fio.py:306`. |
| `app/chamados/services/vigia_fio.py:318` `censo` | `app/chamados/management/commands/vigiar_fio_morto.py:172`; `app/chamados/management/commands/materializar_passivo_validado.py:203` | Contadores e listas de células/dias; universo começa em chamados vivos ligados à célula e módulos de furo, `app/chamados/services/vigia_fio.py:334`; exclui hoje/futuro do alarmado, `:420`. | Lê C15 via `fio_do_dia`, `:417`; lê `dono_fora_do_fio`, `:431`; deriva contadores. |
| `app/chamados/services/vigia_fio.py:542` `censo_alcance` | `app/chamados/management/commands/vigiar_fio_morto.py:173` | Por `(colab,data)` retido: rota, causa, detalhe e papel; `app/chamados/services/vigia_fio.py:625`, `:699`. | Lê `dias_que_seguram :585`, C17 `:635`, dono `:654`; deriva causas sem porta `:667`. |
| `app/chamados/services/vigia_fio.py:222` `ListaDoAdmin.dia_do_admin`; `:230` `dia_na_lista`; `:238` `papel_do_dia` | `dono_fora_do_fio :312`, `:313`; `censo_alcance :661` | Booleano presença com dono BATIDA/CADASTRO; presença+dono; papel do dia. | Lê `cadastro_realidade.lista` e `dias_na_lista`, `app/chamados/services/vigia_fio.py:216`; filtra/traduz `:228`, `:246`. |
| `app/colaboradores/services/calendario.py:97` `resolver_dia_oferecido` | `contexto_calendario`, mesmo arquivo `:769` | Booleano por dia: último recurso, passado, status em `STATUS_QUE_PEDEM_DECISAO`, `:94`, `:103`. | Deriva da projeção do calendário; não considera permissão do observador, `:101`. |
| `app/colaboradores/services/calendario.py:117` `resolver_dia_oferecido_nas_datas` | `app/chamados/services/vigia_fio.py:271` | `{date:bool}`; dia ausente recebe falso, `app/colaboradores/services/calendario.py:135`. | Lê os campos produzidos pelo calendário; `:132`. |
| `app/colaboradores/services/calendario.py:159` `contexto_calendario` | `app/colaboradores/views.py:1946`; `app/colaboradores/services/drawer.py:121`; `app/relatorios/furos.py:160`; plural `app/colaboradores/services/calendario.py:132` | Lista de dias, incluindo `resolver_dia_oferecido`, `:769`. | Lê C6 para perguntas respondidas vivas, `:476`; compõe status e último recurso e escreve projeção do juiz `:769`. |
| `app/escala/services/cadastro_realidade.py:442` `beco_em_lote` | `dias_folga_nao_definida_sem_porta :439`; `_folga_nao_definida_sem_porta :573`, mesmo arquivo | Lista `(colab_id,assinatura C11)`, com data, dono, perguntas, pai limbo e detalhe; `:530`. | Compõe lâmpada indefinida/protesto `:477`, vivas/C17 `:487`, âncora terminal `:515`, cadastro `:518`, dono `:531`. |
| `app/escala/services/cadastro_realidade.py:787` `lista`; `:1044` `dias_na_lista`; `:985` `um_dono_por_dia` | Tela `app/escala/views.py:487`; vigia `app/chamados/services/vigia_fio.py:216`, `:217`; `lista → um_dono_por_dia :885` | Lista administrativa; mapa `(colab,data) → dono`; uniformização do dono do dia. | Lista incorpora C11 `:864`; uniformização reaproveita C11 `:1009` ou lê `InsumosDoDono.dono :1026`; mapa reduz donos `:1064`. |
| `app/chamados/services/fio.py:160` `montar_chamados_fio` | `app/chamados/views.py:569` colab, `:677` admin, `:889` modal; `contexto_parcial_fio`, `app/chamados/services/fio.py:442` | Lista de chamados, com perguntas e `tem_pergunta_viva`, `:249`, `:279`. | Colab: janela e disputas abertas `:169`, `:213`; admin inclui vivas em fechadas via C6 `:234`; flag combina `.viva` e `raiox.pergunta_sem_acao_possivel`, `:279`. |
| `app/chamados/services/janela_colab.py:49` `chamados_visiveis_colab` | `app/api/views.py:1016`; `app/chamados/services/fio.py:171`; `app/chamados/views_arquivar.py:195` | Queryset de chamados da lista de trabalho. | Deriva recorte janela OU destinos de `q_vivas`, silêncio nulo e módulos não internos; `app/chamados/services/janela_colab.py:61`, `:67`, `:70`. Usa pré-filtro SQL, não C6 exato nesse corpo, `:63`. |
| `app/chamados/services/explicador.py:68` `explicar_pergunta`; `:108` `por_que_nao_fecha` | `por_que_nao_fecha → explicar_pergunta :133`; montador `app/chamados/services/fio.py:308`; exibição `app/templates/chamados/partials/modal_fio.html:432` | Por pergunta: estado, dono e ação, `explicador.py:74`; por disputa: `pode_fechar`, resumo, itens, `:163`. | Lê `.estado :72`, materialização real `:93`; C15 decide `pode`, `:137`; deriva descrição e ações. **É respondedor administrativo**, comprovado pelo template `modal_fio.html:435`–`:437`. |
| `app/chamados/services/desenlace.py:44` `desenlace` | `app/chamados/services/fio.py:399`; exibição `app/templates/chamados/partials/modal_fio.html:118` | Por chamado: `{verbo,rotulo,estado,acao}` ou `None`, `desenlace.py:47`. | Compõe `catalogo.natureza.dono_do_chamado :50`, explicação `:62`, filhos `:57`, módulos/porta própria `:78`. |
| `app/chamados/services/acoes_chamado.py:327` `montar_contexto_detalhe` | `app/chamados/views.py:551` | Detalhe de chamado, perguntas vivas da disputa aberta e evidência, `acoes_chamado.py:335`, `:348`. | Lê C6 `:347` e filtra; não usa C17 nesse percurso. |
| `app/colaboradores/services/drawer.py:15` `contexto_drawer` — confirmar linha de definição antes de usar; trechos verificados `:69`, `:78`, `:88` | `app/colaboradores/views.py:2943` | Banner `falha_texto`: motivos das vivas na última disputa aberta; fallback número de chamados de furo “aguardando colaborador”, `drawer.py:82`, `:92`. | Primeiro lê `perguntas_vivas_de :78`; fallback **deriva independentemente** de `VIVOS` e quatro módulos, sem C6, `:88`–`:92`. |
| `app/chamados/relatorios_disputa.py:11` `classificar`; `:26` `coletar` | `coletar → classificar :47`; tela `app/colaboradores/views.py:2997`; command `app/chamados/management/commands/relatorio_pendencias_disputa.py:15` | Por disputa: SEM_PERGUNTA/PRONTA_FECHAR/AGUARDA_COLAB/AGUARDA_SUPERVISAO, `relatorios_disputa.py:13`; buckets de disputas, `:47`. | Lê C6 `:18`; deriva categoria pela existência de resposta `:21`. |
| `app/chamados/services/aviso_pendencia.py:7` `perguntas_pendentes_do_colab`; `:16` `aviso_texto_livre` | `_pendentes`, `app/chamados/services/cobranca.py:48`; aviso `app/api/views.py:997`, `:2220`; `app/chamados/views.py:1120` | Lista de perguntas vivas sem resposta; aviso por conjunto de datas, `aviso_pendencia.py:13`, `:20`. | Lê `q_vivas` e C6, `:11`, `:13`; deriva texto. |
| `app/chamados/services/cobranca.py:75` `checar_cobranca` | `cobrar`, mesmo arquivo `:155`; também referência dinâmica de intake `app/core/intake.py:68` | Pendentes em perguntas, datas distintas e disputas distintas, `cobranca.py:87`–`:94`. | Lê aviso via `_pendentes :82`; deriva prazo/canal/contagens. |
| `app/chamados/services/cobranca.py:221` `_calados`; `:251` `linhas_para_a_pauta` | `linhas_para_a_pauta → _calados :257`; pauta `app/escala/services/fase_12x36.py:272` | Pessoas cobradas com prazo vencido e pessoas sem canal, `cobranca.py:248`; frases de pendência `:261`. | **Deriva com `q_vivas` SQL**, sem C6 exato nesse corpo, exigindo disputa aberta e pergunta sem resposta `:229`; lê juiz prazo `:246`. |
| `app/ponto/services/dia_do_colab.py:6` `dia_do_colab`; `:112` `_veredito_do_dia` | API `app/api/views_mensageria.py:1970`; ficha `app/colaboradores/services/ficha.py:280`; corpo chama frase `dia_do_colab.py:102` | Um `(colab,data)`: marcos/batidas/perguntas e frase “Faltou…” `:144`, vivas esperando resposta/decisão `:150`. | Lê ata `:30`, C6 `:71`, pergunta_no_app `:109`; frase deriva contagens de marcos/perguntas `:139`–`:154`. |
| `app/chamados/services/pergunta_no_app.py:65` `retencao`; `:152` `do_dia`; `:119` `medir` | `medir → retencao :142`; `do_dia → retencao :164`; `app/ponto/services/dia_do_colab.py:109`; commands `app/chamados/management/commands/chamados_vivos_sem_pergunta_no_app.py:14`, `perguntar_chamados_sem_pergunta.py:21`, `:30` | Causa/quem reteve; por dia chamados vivos, perguntas no app, dono da retenção `pergunta_no_app.py:165`; censo por chamado `:145`. | Lê perguntas_do_app `:139`, `:162`, ata `:80`, cobertura `:90`, lista admin `:88`, C6 em `_marco_ja_perguntado :111`; deriva taxonomia de retenção. |
| `app/chamados/services/validacao.py:865` `fila_validar_classificada` | `app/chamados/views_cobrar.py:288`, `:349`, `:361` | Perguntas e chamados distintos por classe, idade, total e rótulo `validacao.py:884`–`:887`. | Lê `perguntas_respondidas_a_validar :874`; deriva classes e contadores. |
| `app/chamados/services/becos.py:25` `classificar` | command `app/chamados/management/commands/fechar_becos_disputa.py:39`; evento `becos.py:69` | Por disputa: `None`, fechável ou barrada, `becos.py:26`. | Deriva aberta, destino e pai terminal `:27`–`:32`; lê C7 `pode_fechar :33`. |

Juízes e leitores que o e2.r incluía, mas o aval não nomeou:

| Função | Chamadores | Resposta e natureza |
|---|---|---|
| `app/chamados/juizes.py:1886` `cobranca_viva_do_dia` | `app/chamados/models.py:373`, `:711`; `app/ponto/services/cartorio.py:1604`; `entrada_sincronizada.py:28`; `triagem_batida.py:589` | Chamado mais novo ou `None`; deriva `VIVOS`, módulo, dia forte e desempate, `juizes.py:1897`–`:1902`. Estes chamadores usam o resultado para planejar/fechar/reconciliar, não são cinco telas novas. |
| `app/chamados/juizes.py:822` `pergunta_viva`, corpo `:1135` | Censo direto extenso é igual ao colado no e2.r; as chamadas adicionais de apresentação estão nas tabelas acima, `explicador.py:72`, `fio.py:280`, `models.py:2502`, `:2809`. | Booleano por pergunta; juiz que deriva vitalidade. |
| `app/chamados/models.py:2492` `.estado`; `:2799` `.viva` | API `app/api/views.py:2569`; explicador `app/chamados/services/explicador.py:72`; vigia `vigia_fio.py:311`; fio `fio.py:280`; também templates `app/templates/chamados/partials/_card_atendimento.html:77`, `:183` | `.viva` delega totalmente a C6, `models.py:2809`; `.estado` lê C6 mas também traduz carimbos/resposta com fallback, `:2499`–`:2506`. |
| `app/chamados/services/fabrica_da_celula.py:74` `motivos_a_perguntar` | `perguntar_pela_celula :164`; `motivos_que_o_evento_deixou :199` | Motivos de perguntas ainda a emitir por célula; deriva deduplicação por motivo `:118`–`:126`, lendo dia_sem_trabalho `:91`, teto `:98`, parcial_so_lista `:102`. Não devolve tarefa completa do dia. |
| `app/chamados/services/fabrica_da_celula.py:191` `motivos_que_o_evento_deixou` | `app/chamados/management/commands/disparar_perguntas_competencia.py:112` | Leitor/vigia dos motivos que evento deixou; lê fábrica `:199`. |
| `app/chamados/models.py:1789` `DisputaSupervisao.pode_fechar` | `becos.py:33`, `:51`; reconciliador `:411`, `:601`, `:1159`; materializacao `:567`, `:1291`; competencia_trancada `:153`, `:202`; disputa_emissao `:1112`, `:1717`; entrada_sincronizada `:58` | Booleano por disputa; compõe C15 e `respostas_sem_veredito`, `models.py:1794`; regra adicional de respostas sem veredito `:1783`–`:1787`. |

Comparação com aval:

| Item do aval | Conferência |
|---|---|
| `vigia_fio.fio_do_dia:156` e `censo_alcance` | Confere; `censo_alcance` está em `app/chamados/services/vigia_fio.py:542`; faltou explicitar `censo:318`, dono `:274` e leitores da lista `:222`. |
| `resolver_dia_oferecido:97` + STATUS `:94` | Confere; único chamador direto é `contexto_calendario:769`; plural é leitura do campo, `calendario.py:135`. |
| C11 `beco_em_lote:442` | Confere função; chamada C17 real é `app/escala/services/cadastro_realidade.py:488`, **não `:460` como colado no e2.r**. A linha 460 é docstring. |
| Nove consumidores C15 “não são respondedores” | Não confere para **explicador**: produz `pode_fechar`, resumo e itens `explicador.py:163`, expostos ao admin `modal_fio.html:432`–`:437`. |
| “TODOS” os leitores na lista | Não confere: montador de fio, painel, relatório, aviso, drawer, pergunta_no_app.do_dia/retenção, dia_do_colab, desenlace, fila de validação e beco de disputa estão documentados acima com seus chamadores. |

O aval acrescenta explicitamente, frente à seção “leitores E3” do e2.r, `resolver_dia_oferecido` e C11, mas ambos já apareciam noutras seções do próprio e2.r colado: juiz “dono fora do fio” e chamadores C17. A identidade da versão da cópia com o HEAD `94d41ea` mencionado no aval é **[nao verificado]** nesta subanálise.

## 08/11 10:28 · sessao 9cde0294b438 · - · - · origem=Ronald (tmux)

| Achado | Evidência |
|---|---|
| **“`o_que_segura` (6 chamadores)” não confere nesta cópia.** O AST não encontrou chamadas de produção à função. A busca textual em Python também só encontra a definição, comentários e chamadas em `tests/`. Não há seis chamadores a enumerar. | Definição: `app/folha/fora_da_folha.py:203`; chamadas de teste: `app/folha/tests/test_haiku_fora_da_folha.py:66`, `:76`, `:145`; `app/chamados/tests/test_n1_alimentacao_unica.py:134`. |
| `_fio_do_dia` tem **três** chamadores de produção. | `fora_da_folha` em `app/folha/fora_da_folha.py:178`; `o_que_segura` em `app/folha/fora_da_folha.py:221`; `dias_sem_fio` em `app/ponto/services/pautas_dos_discordantes.py:77`. |
| `pendentes_do_colaborador` tem três chamadores: os dois kernels **e dashboard web**, ausente da lista do aval. | `app/api/views.py:325`; `app/api/views_core.py:314`; `app/core/views.py:253`. |
| `perguntas_do_app` tem cinco chamadores, incluindo **ficha do colaborador**, ausente da lista do aval. | `app/api/views.py:2552`; `app/chamados/juizes.py:1319`; `app/colaboradores/services/ficha.py:166`; `app/chamados/services/pergunta_no_app.py:139`, `:162`. |
| A ficha responde pendência ao consumidor: forma ações “validar as respostas” e “colaborador responder”, com dono e quantidade. | `app/colaboradores/services/ficha.py:169`, `:171`, `:190`, `:193`. |

A varredura foi feita com `ast.parse`, sem importar Django nem executar consultas, em todos os `.py` da árvore fora de diretórios `tests`, reconhecendo nomes importados e aliases; nomes de método também foram procurados por `ast.Attribute`. Ausência de chamadas obtida por essa varredura é resultado estático, não prova de uso dinâmico em produção. **Uso dinâmico externo à cópia: [nao verificado].**

| Respondedor | Unidade e retorno | Deriva ou lê juiz | Chamadores |
|---|---|---|---|
| `DisputaSupervisao.pendentes_do_colaborador` | QuerySet de **disputas abertas**, uma por PK, com ≥1 pergunta viva sem `respondida_em`. Não agrupa por dia. | Deriva o recorte da disputa; lê `q_vivas` e **C6 `pergunta_viva`** para as perguntas. | Definição/recorte: `app/chamados/models.py:1729`, `:1736`, `:1739`; chamadas `app/api/views.py:325`, `app/api/views_core.py:314`, `app/core/views.py:253`. |
| `perguntas_do_app` | Lista de **perguntas vivas**, ordenada por ID; sem exigir disputa aberta nem pergunta sem resposta. | Lê `q_vivas` e C6; aplica identity map. | `app/chamados/services/pergunta_no_app.py:42`, `:46`, `:53`; cinco chamadas na primeira tabela. |
| `api_disputa_perguntas` | Dois arrays de **perguntas**: `perguntas` e `respondidas`. | Lê `perguntas_do_app`; separa por `.estado == RESPONDIDA`. | `app/api/views.py:2534`, `:2552`, `:2568`, `:2570`; rota em `app/api/urls.py:92`. |
| `ficha_do_colab` | Chamados com verbo/dono; perguntas; ações e quantidade de perguntas a responder/validar, para um colaborador. | Lê `particionar_verbos`, `perguntas_do_app` e `respostas_sem_veredito`; deriva a seleção sem resposta por texto e a lista de ações. | `app/colaboradores/services/ficha.py:155`, `:158`, `:166`, `:169`, `:171`, `:190`, `:193`. |
| `folha._fio_do_dia` | `admin`, `colab`, `chamado` ou `nada`, por célula. | Lê **C17**; devolve `porta['fio']` ou `FIO_SEM_PORTA`. | `app/folha/fora_da_folha.py:70`, `:77`; chamadas `:178`, `:221`, `app/ponto/services/pautas_dos_discordantes.py:77`. |
| `folha.o_que_segura` | Um colaborador/competência, mais lista de **dias retidos**, fio, destrava e IDs de chamados por dia. | Compõe fontes: export, retenção da célula, C17; deriva grupo pelo pior fio. | `app/folha/fora_da_folha.py:203`, `:215`, `:217`, `:221`, `:225`, `:229`; zero chamadas de produção encontradas. |
| `folha.fora_da_folha` | Colaboradores retidos, grupo por pior dia, totais de dias por fio. | Lê `dias_que_seguram` e `_fio_do_dia`; agrega. | `app/folha/fora_da_folha.py:142`, `:161`, `:178`, `:181`, `:194`. |
| `dias_sem_fio` | Lista de **(colaborador, dia)** discordantes sem porta, com empresa/competência/célula. | Lê veredito da célula e `_fio_do_dia`/C17; filtra o universo para pauta. | `app/ponto/services/pautas_dos_discordantes.py:46`, `:69`, `:77`, `:79`. |

| Badge — **mesmo comportamento nos dois kernels** | Unidade | Evidência |
|---|---|---|
| Primeiro calcula `questionarios_pendentes = pendentes_do_colaborador(...).count()` | **Disputas distintas abertas** contendo pergunta C6 viva sem resposta carimbada; não perguntas, não `(colab,dia)`. | `app/api/views.py:325`; `app/api/views_core.py:314`; `app/chamados/models.py:1736`. |
| Se `questionarios_pendentes > 0`: `{count: questionarios_pendentes, cor: '#f59e0b', rotulo: 'Responder DP'}` | Disputas | `app/api/views.py:326`, `:327`; `app/api/views_core.py:315`, `:316`. |
| Senão, se há `chamados_nao_lidos`: `{count: chamados_nao_lidos, cor: '#ef4444', rotulo: 'Chamados'}` | **Chamados vivos** que têm followup `autor_tipo='dp_rh'` não lido pelo colaborador; não número de followups. | `app/api/views.py:318`, `:321`, `:328`, `:329`; `app/api/views_core.py:307`, `:310`, `:317`, `:318`. |
| Sem os dois: `None` | Nenhum objeto badge | `app/api/views.py:331`; `app/api/views_core.py:320`. |
| JSON expõe também os escalares `chamados_nao_lidos`, `questionarios_pendentes` | Chamados / disputas, respectivamente | `app/api/views.py:370`, `:371`, `:372`; gêmeo `app/api/views_core.py:359`, `:360`, `:361`. |

| Divergência concreta **permitida pelo código**, não ocorrência medida | Badge / listas | Evidência |
|---|---|---|
| Uma disputa aberta possui duas perguntas de motivos diferentes, ambas C6 vivas e sem resposta. | Badge **1**; `perguntas_do_app` **2**; array API `perguntas` **2**. Elas podem pertencer ao mesmo dia: a identidade do fato inclui motivo. | Distinct por disputa: `app/chamados/models.py:1736`; identidade por colab/motivo/data: `app/chamados/models.py:2392`; lista sem dedup por disputa/dia: `app/chamados/services/pergunta_no_app.py:46`, `:53`; array: `app/api/views.py:2570`. |
| Uma pergunta C6 viva está numa disputa fechada, sem outras pendências ou não lidos. | Lista **1**; contagem de questionários **0**; `chamados_badge=None`. | Badge exige `disputa__fechada_em IS NULL`: `app/chamados/models.py:1737`; lista não exige: `app/chamados/services/pergunta_no_app.py:46`; badge vazio: `app/api/views.py:331`, `app/api/views_core.py:320`. |
| Uma pergunta tem resposta com texto, continua C6 viva e não foi validada. | `perguntas_do_app` inclui; API coloca em `respondidas`; badge de disputas exclui essa pergunta pelo carimbo de resposta. | Vitalidade com resposta: `app/chamados/juizes.py:1150`, `:1162`; estado: `app/chamados/models.py:2502`; separação: `app/api/views.py:2570`; exclusão: `app/chamados/models.py:1738`. |
| Qual número o colaborador verá **depois de uma E3 futura** | **[nao verificado]**; esta cópia define os números acima. | **[nao verificado]**. |

| `o_que_segura`: origem | O que determina | Direção |
|---|---|---|
| `classificar_export(empresa,mes,ano)` | Item do colaborador; resposta externa `entra = item.status`, `motivo = item.motivo`. Não calcula valor de rubrica nesse corpo. | Chamado antes do fio em `app/folha/fora_da_folha.py:215`; lido no retorno em `:230`; responsabilidade expressa/corpo do export em `app/folha/export.py:757`, `:766`, `:829`. |
| `motivos_retencao_celula(..., com_dias=True)` | Motivos e lista de dias/células com data, veredito e motivo. | `_dias_que_retem`: `app/folha/fora_da_folha.py:95`, `:98`; chamada `:217`; retenção em `app/folha/export.py:1206`, `:1218`, `:1233`, `:1253`. |
| C17 via `_fio_do_dia` | Campo `fio`; `DESTRAVA[fio]`; grupo pelo pior fio. | `app/folha/fora_da_folha.py:77`, `:78`, `:221`, `:226`, `:231`; escada da porta em `app/chamados/juizes.py:1327`, `:1332`. |
| C17 via `_porta` | Campo `chamado`: lista da chave `fila`, não todas as âncoras. | `app/folha/fora_da_folha.py:67`, `:225`; saída C17 em `app/chamados/juizes.py:1385`, `:1386`. |
| Fio → `classificar_export` | **Não há dependência no percurso auditado**: o export decide seus gates e termina retornando `out`; não recebe fio/porta, e `o_que_segura` não escreve seu resultado de volta. | `app/folha/export.py:757`, `:858`, `:864`, `:868`, `:960`, `:966`, `:973`; composição local em `app/folha/fora_da_folha.py:215`, `:229`. |
| `classificar_export` → fio | Em `o_que_segura`, **não altera** a classificação do fio: `_dias_que_retem` é chamado independentemente do status/motivo export, e C17 recebe IDs das células desses dias. Em `dias_que_seguram`/`fora_da_folha`, export **seleciona o universo** de colaboradores `furo_espelho` cujos fios serão lidos; não muda a regra C17. | Independência local: `app/folha/fora_da_folha.py:215`, `:217`, `:218`, `:221`; seleção do universo: `:119`, `:123`, `:128`, `:137`, `:161`, `:178`. |
| Fontes compartilhadas | Ambos leem estados da célula. Isso permite correlação das saídas sem uma escrever na outra. `classificar_export` chama retenção do espelho, que delega à mesma retenção da célula. | `app/folha/export.py:960`, `:1267`; C6 lê veredito em `app/chamados/juizes.py:1181`; C17 alimenta perguntas via C6 em `app/chamados/juizes.py:1302`. |

| Campo por item API | Quem escreve o JSON | Fonte/autor do valor |
|---|---|---|
| `estado` | `api_disputa_perguntas`, `app/api/views.py:2571` | `.estado`, propriedade derivada, `app/chamados/models.py:2492`; validação/materialização primeiro, depois C6 e `respondida_em`: `:2499`, `:2502`, `:2503`. |
| `aguarda_admin` | `app/api/views.py:2572` | Comparação local com `PerguntaDisputa.RESPONDIDA`; não coluna. |
| `disputa_id` | `app/api/views.py:2573` | FK `p.disputa`; declarada em `app/chamados/models.py:2211`, gravada no nascimento em `:2476`. |
| `chamado_id` | `app/api/views.py:2574` | **`p.chamado_destino_id`**, FK declarada em `app/chamados/models.py:2819`; nascimento `:2477`; reapontamento no gate `:2450`; porta de reapontamento `app/chamados/services/fio.py:845`. |
| `id` | `app/api/views.py:2575` | PK da pergunta criada em `app/chamados/models.py:2476`. |
| `motivo` | `app/api/views.py:2576` | Coluna `app/chamados/models.py:2216`; gate `abrir` recebe e cria em `:2398`, `:2476`. |
| `pergunta` | `app/api/views.py:2577` | Coluna `app/chamados/models.py:2217`; emissor fornece texto em `app/chamados/services/disputa_emissao.py:392` e gate grava `**campos` em `app/chamados/models.py:2477`. |
| `tipo_resposta` | `app/api/views.py:2578` | Coluna `app/chamados/models.py:2222`; emissor fornece em `app/chamados/services/disputa_emissao.py:393`, `:774`, `:1199`. |
| `opcoes` | `app/api/views.py:2579` | Coluna `app/chamados/models.py:2223`; emissor fornece em `app/chamados/services/disputa_emissao.py:394`, `:775`; serializer aplica `p.opcoes or []` e remove `OUTRO_MOTIVO` quando `tipo_resposta=='hora'`: `app/api/views.py:2557`, `:2559`. |
| `resposta_colab` | `app/api/views.py:2580` | Coluna `app/chamados/models.py:2227`; POST chama core em `app/api/views.py:2643`; core escreve texto em `app/chamados/services/disputa_emissao.py:1282` e salva em `:1301`; resposta automática também escreve em `:1433`. |
| `resposta_display` — adicional ao recorte do aval | `app/api/views.py:2581` | Propriedade `app/chamados/models.py:2867`: nome de posto/escala, rótulo da opção ou resposta original (`:2874`, `:2882`, `:2889`, `:2891`). |
| `respondida_em` — adicional ao recorte do aval | `app/api/views.py:2582` | ISO ou `None`. Escritores: core `app/chamados/services/disputa_emissao.py:1283`; automático `:1434`; trilha `:1450`; carimbo com texto `app/chamados/models.py:2589`; reset em memória de ressuscitação `app/chamados/models.py:1457`. |

| Autores dos campos de nascimento `motivo`, `pergunta`, `tipo_resposta`, `opcoes`, `disputa`, destino | Chamada do gate |
|---|---|
| Emissão por catálogo: texto com data e opções estáticas/dinâmicas | `app/chamados/services/disputa_emissao.py:294`, `:302`, `:304`, `:310`, `:317`, `:389`. |
| Agrupador de perguntas de hora | `app/chamados/services/disputa_emissao.py:770`. |
| Pergunta de âncora ausente | `app/chamados/services/disputa_emissao.py:1195`. |
| Declaração do colaborador / aplicação de texto pelo admin | `app/chamados/services/declaracao_texto.py:325`, `:614`. |
| Reabertura administrativa | `app/chamados/services/acoes_disputa.py:259`. |
| Persistência central e reaproveitamento de identidade | `app/chamados/models.py:2431`, `:2450`, `:2476`. |

| Campos externos da resposta API | Escritor e conteúdo |
|---|---|
| Lista vazia | `em_disputa=False`, `perguntas=[]`, `respondidas=[]`, em `app/api/views.py:2562`. |
| `em_disputa=True` | Significa lista viva não vazia neste endpoint; não exige disputa aberta, em `app/api/views.py:2561`, `:2585`, e `app/chamados/services/pergunta_no_app.py:46`. |
| `disputa_id` externo | Disputa da **primeira** pergunta da lista, em `app/api/views.py:2563`, `:2586`. |
| `chamado_id` externo | Primeiro destino de pergunta não respondida; fallback para chamado da primeira disputa, em `app/api/views.py:2590`, `:2592`. |
| `perguntas` e `respondidas` | Separação por estado, em `app/api/views.py:2570`, `:2593`, `:2594`. |
| `chamados_badge.count`, `.cor`, `.rotulo` | Objetos construídos diretamente em `api_me` dos dois kernels: `app/api/views.py:327`, `:329`; `app/api/views_core.py:316`, `:318`. Não há coluna gravada para esses campos. |

| Divergência da prosa existente com o vivo | Código |
|---|---|
| Comentário de `pendentes_do_colaborador` afirma badge e lista “no MESMO conjunto”, mas badge adiciona disputa aberta e ausência de carimbo de resposta. | Comentário `app/chamados/models.py:1734`; filtros `:1737`, `:1738`; lista `app/chamados/services/pergunta_no_app.py:46`. |
| Docstring de `api_disputa_perguntas` promete perguntas não respondidas de disputa aberta; corpo retorna vivas inclusive respondidas e sem filtro de abertura. | Docstring `app/api/views.py:2536`; corpo `:2552`, `:2570`; fonte `app/chamados/services/pergunta_no_app.py:46`. |
| Comentário de `perguntas_do_app` enumera “quatro chamadores”; AST encontra também ficha, total cinco. | Comentário `app/chamados/services/pergunta_no_app.py:49`; chamada adicional `app/colaboradores/services/ficha.py:166`. |

Ocorrência/quantidade atual em produção, comportamento visual do APK/PWA e resultado de uma E3 futura: **[nao verificado]**.

## 08/11 10:27 · sessao 24e28cd92da8 · - · - · origem=Ronald (tmux)

AST de todos os `.py` fora de diretórios `tests/`, resolvendo `ImportFrom` e aliases locais (`fio_vivo as _fio_vivo`): **13 chamadas C15, em 12 funções; 3 chamadas C17**. Os pontos abaixo são a evidência concreta do conjunto; uso dinâmico não demonstrado: [nao verificado].

| Chamador direto C15 | Chamada | Papel efetivamente exercido |
|---|---|---|
| `vigia_fio.fio_do_dia` | `app/chamados/services/vigia_fio.py:166` | **Respondedor**: devolve booleano para perguntas recebidas; `censo` lê em `:414` e `:417` e incrementa `fio_vivo` em `:418`. |
| `DisputaSupervisao.pode_fechar` | `app/chamados/models.py:1794` | **Juiz de outra pergunta**: autorização de fechar disputa, combinando ausência de respostas sem veredito com ausência de fio vivo (`:1789–1794`). Também alimenta respondedor de beco: `app/chamados/services/becos.py:33`. |
| `disputa_viva_do_chamado` | `app/chamados/juizes.py:2084` | **Gate de outra ação**: retorna disputa aberta que trava encerramento do chamado; ausência de perguntas também trava (`:2080–2086`). Chamado por alias `_disp_viva(self)` em `app/chamados/models.py:148`, seguido de erro falante ao usuário em `:150–154`. |
| `supra_juiz.Command.handle` | `app/ponto/management/commands/supra_juiz.py:215` | **Insumo de classificação/auditoria**: conta chamados com fio e passa `cobrancas_vivas=len(_com_fio)` a `classificar_dia` (`:223–225`); agrega placar e exemplos (`:235–238`). |
| `reconciliar_grade.Command._relavrar_orfa` | `app/ponto/management/commands/reconciliar_grade.py:460` | **Gate de relavra**: pergunta respondida viva impede relavra, retornando `ORFA_FIO_PRESO` (`:460–462`); chamada em `:228` agrega resultado ao plano. |
| `emitir_furo_retroativo.Command.handle` | `app/ponto/management/commands/emitir_furo_retroativo.py:98` | **Insumo para emitir**: alimenta `classificar_dia(cobrancas_vivas=...)` (`:101–110`) e só prossegue em `FURO_SEM_COBRANCA` (`:111–113`). |
| `folha.previa._material` | `app/folha/previa.py:144` | **Respondedor da prévia**: devolve `resposta do colaborador`, `pergunta viva (sem resposta)` ou `nada` (`:139–146`); escreve campo `material` de cada item de dia/marco em `:95`. |
| `limbo_folgas.filhos_no_defeito` | `app/chamados/services/limbo_folgas.py:201` | **Gate de fechamento do pai**: seleciona filhos cujo dia ainda tem defeito de cadastro e fio vivo (`:191–205`); `detectar_ausencias` lê em `app/ponto/management/commands/detectar_ausencias.py:156`. |
| `cartorio._julgar_colab_corpo` | `app/ponto/services/cartorio.py:1339` | **Insumo da impressão/julgamento**: conta chamados com fio e escreve `fio=len(_com_fio)` em `impressao_insumos` (`:1348–1352`). |
| `cartorio._julgar_colab_corpo` | `app/ponto/services/cartorio.py:1595` | **Gate de reconciliação**: pula chamado sem perguntas ou com fio vivo; só continua tratando fio morto (`:1591–1601`). |
| `explicador.por_que_nao_fecha` | `app/chamados/services/explicador.py:137` | **Respondedor administrativo**: constrói `pode_fechar`, `resumo` e `itens` (`:137–163`); `montar_chamados_fio` chama em `app/chamados/services/fio.py:308` e anexa `ch.explicacao` em `:310`. |
| `encerrar_perguntas_com_fato.Command.handle` | `app/chamados/management/commands/encerrar_perguntas_com_fato.py:83` | **Gate de fechamento**: fio vivo incrementa barrada e pula disputa; do contrário tenta `d.fechar` (`:83–90`). |
| `disputa_emissao._ja_perguntou` | `app/chamados/services/disputa_emissao.py:1244` | **Deduplicação de emissão** por `(disputa,motivo,data_evento)`, com guarda de colaborador ativo (`:1237–1244`); chamada em `:765` e pelo comando `disparar_perguntas_competencia.py:173`. |

**Fora dos nove nomes do aval existem três chamadores diretos:** `vigia_fio.fio_do_dia`, `DisputaSupervisao.pode_fechar` e `juizes.disputa_viva_do_chamado`, conforme chamadas `vigia_fio.py:166`, `models.py:1794`, `juizes.py:2084`.

**A frase “os nove nomeados não são respondedores” não confere:** `explicador` responde ao admin por disputa (`app/chamados/services/explicador.py:163` → `app/chamados/services/fio.py:310`), e `folha/previa` responde com campo `material` apresentado na prévia (`app/folha/previa.py:95`, `app/folha/previa.py:144`). Os demais têm o uso específico descrito na tabela, sem inferir intenção humana.

C15 é uma redução existencial: `any(pergunta_viva(p) for p in perguntas)`; o universo é entregue pelo chamador e não tem necessariamente unidade `(colab,dia)` (`app/chamados/juizes.py:1233`, `app/chamados/juizes.py:1242`).

| Chamador direto C17 | O que lê hoje | Efeito de retirar o símbolo público sem adaptar chamador |
|---|---|---|
| `fora_da_folha._porta` | Importa C17 em `app/folha/fora_da_folha.py:66` e chama em `:67`. `_fio_do_dia` lê `porta['fio']` ou `FIO_SEM_PORTA` em `:77–78`; `o_que_segura` também lê `fila` em `:225`. | A importação deixa de resolver; ficam afetados `fio`, frase `DESTRAVA`, chamados mostrados e grupo por fio (`app/folha/fora_da_folha.py:221–231`). Não equivale automaticamente a alterar `classificar_export`, chamado separadamente em `:215`. |
| `vigia_fio.censo_alcance` | Importa em `app/chamados/services/vigia_fio.py:556`, chama por chave `(cid,dt)` em `:635`. Lê `rota` e `verbos` em `:642–646`; só sem porta recorre a dono e causas em `:654–685`. | A importação deixa de resolver; perde-se a partição porta/sem porta usada nos contadores e nas linhas de alcance (`:635–646`). Chamado pelo comando em `app/chamados/management/commands/vigiar_fio_morto.py:173`. |
| `cadastro_realidade.beco_em_lote` C11 | Importa em `app/escala/services/cadastro_realidade.py:464`; **chamada viva em `:488`**, não `:460`. Filtra dias com perguntas vivas e C17 `None`; depois exige âncora terminal (`:515–517`), defeito folga (`:518`) e produz assinatura C11 (`:530–540`). | A importação deixa de resolver; a seleção “sem porta” do C11 precisa continuar disponível/adaptada para o lote produzir assinaturas. Entradas são a janela individual em `:439` e o lote das competências abertas em `:573`; efeitos adicionais de uma substituição ainda não implementada: [nao verificado]. |

C17 devolve `None` ou `{rota,fila,verbos,fio}`; compõe chamados da célula na fila com perguntas vivas no app, resposta não vazia e âncora na fila (`app/chamados/juizes.py:1373–1387`). “Resolver dia” fica explicitamente fora desse critério (`app/chamados/juizes.py:1367–1369`). A linha `cadastro_realidade.py:460` do anexo não é chamada no vivo: é docstring; a chamada AST está em `:488`.

Outra omissão relevante ao censo amplo: **`chamados/services/becos.py::classificar`** responde `None|FECHAVEL|BARRADA` por disputa e lê C7 em `:33`; combina disputa aberta, visibilidade por `respondida_em`/destino e pai terminal (`app/chamados/services/becos.py:25–33`). Não é chamador direto C15, mas é respondedor indireto de beco.

## 08/11 10:22 · sessao 3832ccb31504 · COBPARC-AUSENCIA-CALA-DOIS-JUIZES · censo · origem=Code

**Não há um “sim” uniforme.** Para a parcial não-falta **sem período**, (A) cala a geração da pergunta; (B) continua julgando célula/ata e considera o dia furado se encontrar lâmpada `acesa=False`. O cartório bloqueia o nascimento do chamado por outro predicado, `parcial_so_lista`. Isso não fecha universalmente chamados existentes. `app/chamados/services/materializacao.py:81`; `app/chamados/juizes.py:480`; `app/ponto/services/cartorio.py:398`; `app/ponto/services/cartorio.py:1777`.

“Parcial”, abaixo, significa o predicado do código: tipo que aceita horas, registro de um dia e `0 < minutos < 480`. A ausência de `hora_ini/hora_fim` não determina parcialidade. `app/ponto/turnos.py:2598`.

**1. Censo dos sítios executáveis**

Nas tabelas, **“não cala” significa que aquela guarda deixa o julgamento continuar**, sem afirmar que efetivamente nascerá cobrança; os próprios emissores possuem outras guardas. `app/chamados/services/disputa_emissao.py:763`; `app/chamados/services/disputa_emissao.py:765`; `app/chamados/services/fabrica_da_celula.py:91`; `app/chamados/services/fabrica_da_celula.py:102`.

| Sítio | Parcial não-falta sem período | Efeito |
|---|---|---|
| `app/ponto/turnos.py:2598` — `cobre_parte_do_dia` | `True`; não lê as horas. | Define parcialidade. |
| `app/ponto/turnos.py:2632` — `periodo_da_parcial_no_dia` | Retorna `'sem_periodo'`. | Classifica período. |
| `app/ponto/turnos.py:2675` — `ausencia_explica_o_dia_inteiro` | `False`. | Não reconhece cobertura inteira. |
| `app/chamados/services/materializacao.py:81` — (A) | **Cala** pelo segundo termo, em `:82`. | Guarda da pergunta e da reemissão. |
| `app/chamados/juizes.py:449` — (B) | Não cala pela ausência; segue à ata. Lâmpada falsa prende em `:480`; sem célula/ata lavrada retorna `None` em `:464`/`:478`. | Premissa/S122/fechamento/reabertura. |
| `app/chamados/juizes.py:410` — `ausencia_parcial_cobra_o_resto` | `True` para registro `tipo_dia='ausencia'` com `cobertura_parcial`. Não lê período. | Predicado compartilhado dos leitores do resto. |
| `app/chamados/juizes.py:426` — `dia_sem_trabalho` | A parcial só responde “sem trabalho” se `trabalha is False`. | Não cala pelo mero rótulo ausência. |
| `app/escala/utils.py:1211` e `app/escala/utils.py:1255` — `_marcar_ausencias` | Marca parcial e `'sem_periodo'`; não dispensa missing, pela guarda em `:1265`. | Grade/folha/insumo da ata. |
| `app/ponto/services/cartorio.py:398` — `parcial_so_lista` | **Cala nascimento**, retornando `True` incondicionalmente. | Chamado/pergunta/push. |
| `app/ponto/services/cartorio.py:436` — `_cob_parcial_da_impressao` | Conserva `'sem_periodo'` na impressão dos insumos. | Controle de rejulgamento; não decide cobrança sozinho. |
| `app/ponto/services/cartorio.py:732` e `app/ponto/services/cartorio.py:736` — `ata_do_dia` | Transporta parcialidade e período. | Lavratura da ata. |
| `app/ponto/services/cartorio.py:766` — `dispensar_por_ausencia` | **Não dispensa** pela ausência parcial. | Mantém lâmpadas recebidas da grade. |
| `app/escala/services/leitor_celula.py:416` e `app/escala/services/leitor_celula.py:418` | Transporta os dois carimbos da ata. | Alimentação dos leitores; não recalcula cobertura. |
| `app/ponto/supra_juiz.py:381` e `app/ponto/supra_juiz.py:439` | Com fato/batida nas condições dos respectivos ramos, produz `PARCIAL_SEM_PERIODO`. | Classificação para cadastro. |
| `app/ponto/supra_juiz.py:530` | Sem período não constitui `_resto_cobrado`; aprovada + chamado vivo produz `COBRANCA_EM_DIA_COBERTO`, em `:532`. | Protesto sobre cobrança existente. |

O supra **não trata todo sem-período como `PARCIAL_SEM_PERIODO`**: o ramo sem batida admite a parcial como dia de cobrança e pode produzir `FURO_SEM_COBRANCA`; a emissão é barrada posteriormente pelo cartório. `app/ponto/supra_juiz.py:304`; `app/ponto/supra_juiz.py:313`; `app/ponto/supra_juiz.py:367`; `app/ponto/services/cartorio.py:1725`.

Todos os leitores executáveis de `ausencia_parcial_cobra_o_resto`, além de `dia_sem_trabalho`:

| Sítio | Resposta local para sem período | Efeito |
|---|---|---|
| `app/chamados/services/declaracao_texto.py:98` | Mantém a parcial; enumera missing em `:110`. | Marcos faltantes/texto da cobrança. |
| `app/escala/services/furos_diarios.py:49` | Mantém o dia e conta furos em `:54`. | `FuroDiario`/métricas. |
| `app/relatorios/furos.py:195` | Mantém a parcial e apresenta marcos apagados em `:202`. | Raio-X/relatório. |
| `app/inteligencia/calculadores.py:50` | Mantém a parcial e conta marcos apagados em `:64`. | `preenchimento_espelho`. |
| `app/inteligencia/calculadores.py:149` | Mantém a parcial e conta previstos/batidos em `:160`. | `preenchimento_marcos`. |

Leitores adicionais de `dia_sem_trabalho`, que recebem essa mesma exceção da parcial:

| Sítio | Resposta local para sem período | Efeito |
|---|---|---|
| `app/chamados/models.py:1245` | Parcial não regulariza apenas pelo rótulo ausência. | Coletor da premissa. |
| `app/chamados/services/fabrica_da_celula.py:91` | Essa guarda deixa passar; `parcial_so_lista` cala depois, em `:102`. | Pergunta/pai/push. |
| `app/chamados/services/materializacao.py:174` | Deixa a parcial participar da avaliação do plantio. | Verificação de preenchimento do furo/paridade. |
| `app/ponto/services/cluster_espurio.py:25` | Deixa participar da leitura das lâmpadas/órfãs. | Diagnóstico do cluster; nascimento recebe bloqueio separado. |
| `app/ponto/management/commands/detectar_intervalo_ausente.py:51` | Deixa participar; a cobrança exige condições das lâmpadas em `:69`. | Furo de intervalo. |
| `app/ponto/management/commands/reconciliar_grade.py:282` | Parcial com `trabalha!=False` não entra nos mortos por essa cláusula. | Retratação de cobrança. |
| `app/ponto/supra_juiz.py:71` e `app/ponto/supra_juiz.py:496` | Consulta o juiz no ramo sem trabalho; esse ramo exige `cobertura_status is None`, em `:495`. | `COBRANCA_EM_DIA_SEM_TRABALHO`. |

Demais chamadas executáveis dos predicados pedidos:

| Sítio | Resposta para parcial sem período | Efeito |
|---|---|---|
| `app/escala/servico_jornada.py:344` | A parcial não cala essa guarda. O laço consulta **todas** as ausências, em `:342`, não só a vencedora. | `classificar_falta`. |
| `app/ponto/turnos.py:2690` | Exclui a parcial dos dias inteiros; exige também efeito `ABONA`. | Folha/pontualidade. |
| `app/ponto/services/fechamento.py:694` | Parcial paga não abona o dia inteiro; conta pelas batidas, em `:695`. | Folha. |
| `app/ponto/services/efeito_ausencia.py:48` | Soma minutos da parcial paga. | Crédito financeiro. |
| `app/ponto/services/efeito_ausencia.py:55` | Parcial paga não justifica/suprime o dia inteiro. | Folha/espelho. |
| `app/ponto/services/credito_parcial.py:27` | Credita minutos pagos no próprio dia. | Folha/espelho. |
| `app/ponto/services/dia_decidido.py:494` | Conserva minutos no rótulo da parcial abonada. | Tela. |
| `app/ponto/services/ausencia.py:883` e `app/ponto/services/ausencia.py:887` | Validador rejeita hora faltante e período em não-parcial. Só é chamado quando alguma hora foi fornecida, em `:906`. | Cadastro. |
| `app/ponto/services/vigia_ausencia_veredito.py:54` | Pula parcial no contador. | Vigia de discordância. |
| `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:49` | Pula parcial no contador. | Diagnóstico. |
| `app/ponto/views.py:2084` | Entrega parcialidade ao painel. | Tela. |
| `app/ponto/views.py:2240` | Marca `a.cobre_parte=True`. | Lista de ausências. |
| `app/templates/ponto/ausencias.html:163` | Mostra minutos. | Tela. |
| `app/templates/ponto/partials/painel_ausencia.html:296` e `app/templates/ponto/partials/painel_ausencia.html:311` | Mostra edição/aprovação do período e os avisos sobre lista do DP, em `:299`/`:313`. | Tela; não publica uma linha administrativa. |

A exclusão da parcial no helper de dias inteiros também chega aos consumidores em `app/ponto/services/fechamento.py:687`, `app/ponto/services/efeito_ausencia.py:65`, `app/ponto/turnos.py:2695`, `app/ponto/management/commands/diff_calculador.py:412`, `app/ponto/services/esmeril_espelho.py:164` e `app/relatorios/management/commands/diff_extrato_parcial.py:67`.

Há ainda o juiz registrado da cobrança viva: `reconciliar_por_ausencia` escolhe a vencedora, mas, para aprovada, exige `_premissa_morre` e sucesso de `ch.reconciliar()`. Não testa `'sem_periodo'` diretamente. `app/chamados/services/silencio_ausencia.py:65`; `app/chamados/services/silencio_ausencia.py:93`; `app/chamados/services/silencio_ausencia.py:101`.

**2. Grade, lâmpadas, emissão e lista administrativa**

A grade conserva os missing da parcial sem período: escreve `tipo_dia='ausencia'`, `cobertura_parcial=True` e `'sem_periodo'`; a transformação de missing em dispensada exige período diferente de `'sem_periodo'`. `app/escala/utils.py:1241`; `app/escala/utils.py:1251`; `app/escala/utils.py:1255`; `app/escala/utils.py:1265`.

Na ata, **os marcos missing conhecidos ficam `acesa=False`**. Isso não significa que todas as lâmpadas do dia apaguem: as células cumpridas continuam acesas; existe também resposta `None` para fato cego sem coluna atribuível. `app/ponto/services/cartorio.py:647`; `app/ponto/services/cartorio.py:677`; `app/ponto/services/cartorio.py:689`; `app/ponto/services/cartorio.py:641`.

`ata_do_dia` grava as lâmpadas, conta as falsas e transporta os carimbos da parcial. `dispensar_por_ausencia` não as acende pela ausência parcial; `dia['justificado']` constitui outra fonte explícita de dispensa. `app/ponto/services/cartorio.py:704`; `app/ponto/services/cartorio.py:712`; `app/ponto/services/cartorio.py:732`; `app/ponto/services/cartorio.py:736`; `app/ponto/services/cartorio.py:766`; `app/ponto/services/cartorio.py:768`.

**O cartório não cria o chamado de furo desse dia com esses carimbos:** calcula `parcial_so_lista` e o exige falso nos dois portões de emissão. Um chamado vivo pode permanecer e receber espelhamento dos marcos. `app/ponto/services/cartorio.py:1706`; `app/ponto/services/cartorio.py:1725`; `app/ponto/services/cartorio.py:1777`; `app/ponto/services/cartorio.py:1787`.

Outros consumidores do bloqueio:

| Sítio | Ação para sem período |
|---|---|
| `app/chamados/services/fabrica_da_celula.py:102` | Retorna sem motivos/pergunta/pai. |
| `app/chamados/services/disputa_emissao.py:871` e `app/chamados/services/disputa_emissao.py:894` | Classifica pelo predicado e remove o chamado da fábrica de disputas/perguntas. |
| `app/chamados/services/pergunta_no_app.py:88` | Rotula retenção como `parcial_na_lista_do_admin`. |
| `app/chamados/reconciliador.py:872` | Bloqueia nascer/renascer fora da costura em chamado já vivo. |
| `app/ponto/services/cluster_espurio.py:103` | Remove o dia de `a_abrir`, em `:107`. |
| `app/chamados/management/commands/disparar_perguntas_competencia.py:154` | Conta e pula nova pergunta. |
| `app/ponto/management/commands/emitir_furo_retroativo.py:121` | Conta e pula emissão retroativa. |

Quanto à **lista do admin**, há três coisas distintas no código:

- Classificação `PARCIAL_SEM_PERIODO`, derivando `indefinida`: `app/ponto/supra_juiz.py:388`; `app/ponto/supra_juiz.py:441`; `app/escala/models.py:1615`.
- Contagem `parcial_na_lista_sem_push`: `_contar_na_lista` apenas incrementa contador em memória e deduplica a chave. `app/ponto/services/cartorio.py:1829`; `app/ponto/services/cartorio.py:1832`.
- Texto da tela dizendo que o dia está na lista do DP: `app/templates/ponto/partials/painel_ausencia.html:299`.

A tela administrativa chama `cadastro_realidade.lista`; sua fonte C11 exige especificamente o protesto `PREVISTO_COM_DIA_INDEFINIDO`. Há também uma fonte genérica para ausência com pelo menos duas batidas. Essas condições não demonstram inclusão de **toda parcial sem período**. `app/escala/views.py:484`; `app/escala/services/cadastro_realidade.py:477`; `app/escala/services/cadastro_realidade.py:478`; `app/ponto/services/esmeril_espelho.py:133`.

**Publicação efetiva de um dia específico numa lista administrativa por ser “parcial sem período”: [não verificado].**

**3. Chamadores de (A), de (B) e ações**

Os chamadores executáveis diretos de **(A)** são:

| Chamador | Onde a resposta vira ação |
|---|---|
| `app/chamados/services/disputa_emissao.py:763` — `_criar_pergunta_se_couber` | `True` retorna antes de `PerguntaDisputa.abrir`, em `:770`. |
| `app/chamados/reconciliador.py:845` — `reemitir_chamado_do_dia` | `True` retorna antes de abrir/reutilizar o chamado, em `:900`. |
| `app/chamados/services/pergunta_no_app.py:90` — `retencao` | Retorna causa diagnóstica; não fecha chamado. A classificação administrativa vem antes, em `:88`. |

Os chamadores executáveis diretos de **(B)** são:

| Chamador | Efeito |
|---|---|
| `app/chamados/models.py:1260` — `premissa_regularizada` | Quando B responde, devolve `not _f`, em `:1262`. |
| `app/chamados/juizes.py:471` | Recursão para a véspera quando o dia é sem trabalho; conserva `para`. |

O coletor pode aceitar provas **antes de B**: justificativa aprovada, decisão administrativa, dia sem trabalho para chamado que não seja `turno_aberto`, marcos acesos ou inexistentes. `app/chamados/models.py:1227`; `app/chamados/models.py:1243`; `app/chamados/models.py:1245`; `app/chamados/models.py:1249`; `app/chamados/models.py:1258`.

As ações posteriores são:

- **S122:** usa `premissa_regularizada()` no efeito padrão cobrança; se não regularizado, recusa transição para `resolvido`. `app/chamados/models.py:175`; `app/chamados/models.py:177`; `app/chamados/models.py:185`.
- **Fechamento do sistema:** consulta a premissa com `para='folha'`; há ainda outras guardas antes de gravar o fechamento. `app/chamados/models.py:701`; `app/chamados/models.py:648`; `app/chamados/models.py:617`.
- **Reabertura:** premissa regularizada com `para='folha'` produz recusa; `reabrir` registra e retorna `False`. Premissa não regularizada apenas deixa essa guarda passar. `app/chamados/models.py:1309`; `app/chamados/models.py:1310`; `app/chamados/models.py:1361`; `app/chamados/models.py:1366`.

**4. Juiz registrado**

**Sim: C11**, pergunta **“a ausência do dia suspende, fecha ou levanta a cobrança viva?”**, aponta para `chamados/services/silencio_ausencia.py::reconciliar_por_ausencia`. `app/core/juizes.py:117`; `app/core/juizes.py:271`.

Também existe **C18**, para a pergunta mais estreita sobre a cobrança calar porque a célula lavrada diz que o dia não é de trabalho, apontando para `dia_sem_trabalho`. `app/core/juizes.py:149`; `app/core/juizes.py:278`.

O documento registra explicitamente a exceção parcial que cobra o resto e a regra especial de (A) para `'sem_periodo'`; portanto não descreve uma resposta única compartilhada por A e B. `app/docs/JUIZES.md:62`; `app/docs/JUIZES.md:65`; `app/docs/JUIZES.md:66`.

**5. Contraexemplos e limites**

Os seguintes resultados decorrem das condições do código; ocorrência produtiva concreta: **[não verificado]**.

| Condição | (A) | (B) |
|---|---|---|
| **Parcial com período; todos os missing dispensados dentro dele; nenhuma órfã** | Retorna `False`: não cala pela ausência. Com horas declaradas, `periodo(a, ())` retorna `'fora'`. `app/chamados/services/materializacao.py:81`; `app/ponto/turnos.py:2641`. | Retorna `False`: sem furo, pois as dispensadas ficam acesas e não há órfãs. **Diferença semântica, apesar dos dois booleanos serem falsos.** `app/escala/utils.py:1265`; `app/ponto/services/cartorio.py:647`; `app/chamados/juizes.py:480`; `app/chamados/juizes.py:490`. |
| **Falta aprovada; ata `'falta'`; véspera responde `False` ou `None`** | Não cala: ambos os termos excluem falta. `app/chamados/services/materializacao.py:82`; `app/ponto/turnos.py:2675`. | Retorna `False` pela regra do dia sem trabalho/véspera. Para chamado não-turno, o coletor já regulariza antes de B. `app/chamados/juizes.py:428`; `app/chamados/juizes.py:471`; `app/chamados/juizes.py:472`; `app/chamados/models.py:1245`. |

**Tipo integral não-falta com efeito diferente de `ABONA` não é, sozinho, divergência A/B:** ambos calam sem testar efeito. A diferença está no helper financeiro, que exige `ABONA`. `app/chamados/services/materializacao.py:81`; `app/chamados/juizes.py:449`; `app/ponto/turnos.py:2690`.

**`para='folha'` também não diferencia uma única vencedora aprovada comum aos dois.** A fixa cobrança; B aceita o efeito recebido. Cobrança admite não rejeitadas, folha somente aprovadas: com lançamentos concorrentes, podem selecionar vencedoras diferentes pelo desempate `-data, -pk`. `app/chamados/services/materializacao.py:78`; `app/chamados/juizes.py:448`; `app/ponto/turnos.py:2514`; `app/ponto/turnos.py:2516`; `app/ponto/turnos.py:2557`.

## 08/11 10:20 · sessao 7681152d2c16 · - · - · origem=Ronald (tmux)

B não dá uma resposta única apenas pela presença da ausência aprovada: retorna `False` imediatamente só quando a ausência vencedora explica o dia inteiro; a parcial segue para célula, ata e lâmpadas. Uma lâmpada `acesa is False` devolve `True`; célula inexistente ou ata sem `lampadas` devolve `None`. Para parcial `tipo_dia='ausencia'`, `cobertura_parcial=True`, `dia_sem_trabalho` só cala por `trabalha is False`. Fontes: `app/chamados/juizes.py:448`, `app/chamados/juizes.py:449`, `app/chamados/juizes.py:460`, `app/chamados/juizes.py:477`, `app/chamados/juizes.py:480`, `app/chamados/juizes.py:426`.

Chamadores diretos executáveis de B:

| Chamador | Efeito |
|---|---|
| `app/chamados/models.py:1260` | `premissa_regularizada` devolve `not _f` quando B sabe responder (`app/chamados/models.py:1261`). |
| `app/chamados/juizes.py:471` | Recursão para véspera quando `dia_sem_trabalho(cel)`; preserva `para` e usa `False` se a véspera retorna `None` (`app/chamados/juizes.py:472`). |

O import em `app/chamados/models.py:3318` reexporta B. As ações que usam o coletor são:

- S122 humano: `transicionar(..., novo_status='resolvido')` chama `premissa_regularizada()` com padrão cobrança; ausência de regularização produz `ValueError`. `app/chamados/models.py:175`, `app/chamados/models.py:177`, `app/chamados/models.py:185`.
- Fechamento pelo sistema: `_gate_premissa_do_fechamento` usa `para='folha'`; premissa regularizada libera essa guarda. O fechamento passa também pela guarda da pergunta viva. `app/chamados/models.py:701`, `app/chamados/models.py:648`, `app/chamados/models.py:651`.
- `reconciliar` grava o estado decidido somente se `_decisao_do_fechamento` permite. `app/chamados/models.py:617`, `app/chamados/models.py:620`.
- Reabertura: `recusa_da_reabertura` usa `para='folha'`; premissa regularizada impede `reabrir`, registrando recusa e retornando `False`. `app/chamados/models.py:1309`, `app/chamados/models.py:1310`, `app/chamados/models.py:1361`, `app/chamados/models.py:1366`.
- Antes de chamar B, o coletor já aceita justificativa aprovada, decisão do admin, dia sem trabalho para chamados que não são `turno_aberto`, marcos acesos e marcos inexistentes. `app/chamados/models.py:1227`, `app/chamados/models.py:1243`, `app/chamados/models.py:1245`, `app/chamados/models.py:1249`, `app/chamados/models.py:1258`.

Censo de leitores executáveis de `ausencia_parcial_cobra_o_resto`:

| Sítio | Parcial sem período | Efeito |
|---|---|---|
| `app/chamados/juizes.py:426` | Cobra pela ata, salvo `trabalha=False`; não consulta período. | Juiz do dia sem trabalho; `app/chamados/juizes.py:427`. |
| `app/chamados/services/declaracao_texto.py:98` | Mantém a parcial e enumera seus `missing`. | Marcos faltantes da declaração; `app/chamados/services/declaracao_texto.py:110`, `app/chamados/services/declaracao_texto.py:123`. |
| `app/escala/services/furos_diarios.py:49` | Mantém o dia na apuração e conta os furos. | Alimenta FuroDiario; `app/escala/services/furos_diarios.py:54`, `app/escala/services/furos_diarios.py:57`. |
| `app/relatorios/furos.py:195` | Mantém a parcial e apresenta marcos apagados. | Raio-X da folha; `app/relatorios/furos.py:202`, `app/relatorios/furos.py:208`. |
| `app/inteligencia/calculadores.py:50` | Mantém parcial; conta marcos apagados. | `preenchimento_espelho`; `app/inteligencia/calculadores.py:57`, `app/inteligencia/calculadores.py:64`. |
| `app/inteligencia/calculadores.py:149` | Mantém parcial; conta previstos e batidos. | `preenchimento_marcos`; `app/inteligencia/calculadores.py:156`, `app/inteligencia/calculadores.py:160`. |

Esses leitores não testam `sem_periodo` nas condições citadas; preservam a leitura do resto da parcial, sujeita às demais guardas locais. `app/chamados/juizes.py:410`, `app/chamados/services/declaracao_texto.py:98`, `app/escala/services/furos_diarios.py:49`, `app/relatorios/furos.py:195`, `app/inteligencia/calculadores.py:50`, `app/inteligencia/calculadores.py:149`.

Leitores adicionais de `dia_sem_trabalho`:

| Sítio | Resposta local para parcial sem período |
|---|---|
| `app/chamados/services/fabrica_da_celula.py:91` | Esta guarda deixa passar se `trabalha!=False`; a guarda seguinte `parcial_so_lista` cala pergunta, pai e push. `app/chamados/services/fabrica_da_celula.py:102`, `app/chamados/services/fabrica_da_celula.py:104`. |
| `app/chamados/services/materializacao.py:174` | Deixa a parcial participar da avaliação de plantio que preenche furo; pode absolver a guarda de paridade. `app/chamados/services/materializacao.py:178`, `app/chamados/services/materializacao.py:181`. |
| `app/ponto/services/cluster_espurio.py:25` | Deixa a parcial participar; assinatura requer pelo menos uma lâmpada apagada e duas órfãs próximas. `app/ponto/services/cluster_espurio.py:32`, `app/ponto/services/cluster_espurio.py:33`, `app/ponto/services/cluster_espurio.py:35`. |
| `app/ponto/management/commands/detectar_intervalo_ausente.py:51` | Deixa a parcial participar; só lista pausas apagadas quando entrada e saída estão acesas. `app/ponto/management/commands/detectar_intervalo_ausente.py:69`, `app/ponto/management/commands/detectar_intervalo_ausente.py:71`. |
| `app/ponto/management/commands/reconciliar_grade.py:282` | Parcial com `trabalha!=False` não entra nos mortos por essa cláusula. `app/chamados/juizes.py:426`, `app/chamados/juizes.py:427`. |
| `app/ponto/supra_juiz.py:71` e `app/ponto/supra_juiz.py:496` | Usa o mesmo juiz no ramo `COBRANCA_EM_DIA_SEM_TRABALHO`, alcançado com `cobertura_status is None`. `app/ponto/supra_juiz.py:494`, `app/ponto/supra_juiz.py:497`. |
| `app/chamados/models.py:1245` | A parcial não regulariza pelo rótulo ausência; continuam as demais provas do coletor. `app/chamados/juizes.py:426`, `app/chamados/models.py:1247`. |

Contraexemplos de código, sem afirmar ocorrência produtiva:

1. **Parcial COM período, todos os marcos faltantes dispensados, sem órfãs:** A retorna `False` — não cala por ausência — porque não explica o dia inteiro e o período declarado não é `sem_periodo`. A grade pode dispensar todos os marcos missing dentro do período; estes viram lâmpadas acesas, e B retorna `False` — sem furo. `app/chamados/services/materializacao.py:81`, `app/ponto/turnos.py:2632`, `app/ponto/turnos.py:2641`, `app/escala/utils.py:1265`, `app/escala/utils.py:1269`, `app/ponto/services/cartorio.py:647`, `app/ponto/services/cartorio.py:677`, `app/chamados/juizes.py:480`, `app/chamados/juizes.py:490`.

2. **Falta aprovada:** A sempre retorna `False`, porque os dois termos excluem falta. B lê a ata: `tipo_dia='falta'` aciona `dia_sem_trabalho`, consulta a véspera e retorna `False` quando a véspera responde `False` ou `None`. No coletor, chamado que não seja `turno_aberto` já fica regularizado por esse tipo de ata antes de B. `app/chamados/services/materializacao.py:81`, `app/chamados/services/materializacao.py:82`, `app/ponto/turnos.py:2675`, `app/chamados/juizes.py:428`, `app/chamados/juizes.py:471`, `app/chamados/juizes.py:472`, `app/chamados/models.py:1245`.

3. **Tipo não ABONA, integral e não falta:** não distingue A de B: ambos calam sem consultar `efeito`. Distingue essa cobrança do helper de folha, que exige `a.efeito == ABONA`. `app/chamados/services/materializacao.py:81`, `app/chamados/juizes.py:449`, `app/ponto/turnos.py:2675`, `app/ponto/turnos.py:2689`.

4. **`para='folha'`:** A fixa cobrança, B aceita o efeito. Cobrança inclui não rejeitadas; folha inclui apenas aprovadas. Portanto podem consultar vencedoras diferentes quando houver mais de um lançamento; desempate é `-data, -pk`. Com uma única ausência aprovada comum aos dois, o filtro de status não constitui diferença. `app/chamados/services/materializacao.py:78`, `app/chamados/juizes.py:448`, `app/ponto/turnos.py:2514`, `app/ponto/turnos.py:2516`, `app/ponto/turnos.py:2552`, `app/ponto/turnos.py:2557`.

O registro tem a pergunta mais estreita `_C18`, “a cobrança do dia cala porque a célula lavrada diz que o dia não é de trabalho?”, apontando para `dia_sem_trabalho`; não é um registro literal de A/B como juiz único da composição ausência → silêncio. `app/core/juizes.py:149`, `app/core/juizes.py:278`.

## 08/11 10:20 · sessao d3bca936a7a8 · - · - · origem=Ronald (tmux)

Censo auxiliar, só leitura:

| Sítio executável | Resposta para parcial não-falta sem horas | Efeito |
|---|---|---|
| `app/ponto/turnos.py:2598` | `cobre_parte_do_dia=True` se tipo aceita horas, registro de um dia, `0<minutos<480`; horas não participam. | Predicado de parcial. |
| `app/ponto/turnos.py:2632` | Retorna `sem_periodo`. | Período declarado. |
| `app/ponto/turnos.py:2675` | `ausencia_explica_o_dia_inteiro=False`. | Cobertura inteira. |
| `app/ponto/turnos.py:2690` | Excluída de dias inteiros; além disso exige efeito `ABONA`. | Folha/calculador, zera pontualidade apenas para inteiro. |
| `app/escala/servico_jornada.py:344` | Parcial não cala esse guarda; continua classificação. Varre todas as ausências, não somente vencedora (`:342`). | `classificar_falta`, furo. |
| `app/ponto/services/fechamento.py:694` | Parcial de `TIPOS_PAGOS` pula abono inteiro; dia conta pelas batidas (`:695`). | Folha. |
| `app/ponto/services/efeito_ausencia.py:48` | Soma minutos pagos quando dentro do período. | Crédito financeiro. |
| `app/ponto/services/efeito_ausencia.py:55` | Parcial paga não justifica nem suprime dia inteiro (`:56`). | Folha/espelho. |
| `app/ponto/services/credito_parcial.py:27` | Minutos pagos viram crédito do próprio dia (`:28`). | Folha/espelho. |
| `app/ponto/services/dia_decidido.py:494` | Parcial abonada conserva minutos no rótulo (`:496`). | Tela. |
| `app/ponto/services/ausencia.py:887` | Permite período somente para parcial; quando este validador recebe hora faltante, rejeita (`:883-884`). | Cadastro. |
| `app/ponto/services/vigia_ausencia_veredito.py:54` | Pula parcial no contador de discordância. | Vigia. |
| `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:49` | Pula parcial no contador. | Diagnóstico. |
| `app/ponto/views.py:2084` | Entrega `True` ao drawer. | Tela. |
| `app/templates/ponto/partials/painel_ausencia.html:296` | Mostra edição de período aprovado; sem início mostra “dia está na lista do DP” (`:299`). | Tela. |
| `app/templates/ponto/partials/painel_ausencia.html:311` | Mostra período na aprovação, com aviso “sem hora [...] lista do DP” (`:313`). | Tela. |
| `app/ponto/views.py:2240` | Marca `a.cobre_parte=True`. | Lista de ausências. |
| `app/templates/ponto/ausencias.html:163` | Exibe minutos da parcial. | Tela. |
| `app/escala/services/leitor_celula.py:416` | Transporta `cobertura_parcial`; transporta literalmente `periodo_parcial` da ata (`:418-419`). | Alimentação dos leitores de ata. |
| `app/chamados/services/declaracao_texto.py:98` | Parcial entra mesmo com `tipo_dia='ausencia'`; conserva marcos `missing` (`:110-111`). | Texto/declarativa de cobrança. |
| `app/escala/services/furos_diarios.py:49` | Parcial entra na conta, inclusive sem período. Conta `missing` (`:54`, `:21`). | `FuroDiario`, métricas/ranking. |
| `app/relatorios/furos.py:195` | Parcial entra; marcos apagados vão ao resultado (`:202-209`). | Raio-X da folha/tela. |
| `app/inteligencia/calculadores.py:50` | Parcial entra; marcos apagados classificam completo/parcial/furo (`:64-70`). | Métrica `preenchimento_espelho`. |
| `app/inteligencia/calculadores.py:149` | Parcial entra; conta previstos e batidos (`:160-161`). | Métrica de preenchimento de marcos. |

Os chamadores indiretos de `dias_com_ausencia_de_dia_inteiro` também excluem a parcial: `app/ponto/services/fechamento.py:687`, `app/ponto/services/efeito_ausencia.py:65`, `app/ponto/turnos.py:2695`, `app/ponto/management/commands/diff_calculador.py:412`, `app/ponto/services/esmeril_espelho.py:164`, `app/relatorios/management/commands/diff_extrato_parcial.py:67`.

`supra_juiz` tem três respostas distintas comprovadas:

- Com fato e turno encerrado, lê `periodo_parcial`, produz `PARCIAL_SEM_PERIODO`, cobra cadastro: `app/ponto/supra_juiz.py:381-389`.
- Com batida sem par, realizado zero, lê o mesmo campo e produz a mesma acusação: `app/ponto/supra_juiz.py:437-442`.
- Sem batida, pode produzir `FURO_SEM_COBRANCA`: a parcial é dia de cobrança (`:304`), a ausência de período não barra o portão (`:313-316`), e o furo é emitido no resultado (`:367-370`). O cartório barra depois o nascimento por `parcial_so_lista`, não pelo classificador (`app/ponto/services/cartorio.py:1706`, `:1723-1725`).
- Com chamado vivo, parcial sem período não satisfaz `_resto_cobrado`, portanto produz `COBRANCA_EM_DIA_COBERTO`: `app/ponto/supra_juiz.py:530-534`.
- `PARCIAL_SEM_PERIODO` deriva veredito `indefinida`: `app/escala/models.py:1615`.

Guardas adicionais da lista do administrador:

| Sítio | Efeito comprovado |
|---|---|
| `app/ponto/services/cartorio.py:398` | Sem período + cobertura parcial retorna `True` incondicionalmente (`:399`). |
| `app/ponto/services/cartorio.py:1723` | Conta na lista e bloqueia emissão do furo do dia (`:1725`). |
| `app/ponto/services/cartorio.py:1775` | Conta na lista e bloqueia nascimento do chamado de furo parcial (`:1777`). |
| `app/ponto/services/cartorio.py:1832` | Incrementa `parcial_na_lista_sem_push`, deduplicado por dia-colaborador (`:1829-1831`). |
| `app/chamados/services/fabrica_da_celula.py:102` | Retorna sem motivos/pergunta/pai/push (`:104`). |
| `app/chamados/services/disputa_emissao.py:871` | `dia_na_lista_do_admin` delega à ata e ao predicado do cartório. |
| `app/chamados/services/disputa_emissao.py:894` | Remove chamados desses dias antes da fábrica de disputas; lista vazia retorna `None` (`:895-896`). |
| `app/chamados/services/pergunta_no_app.py:88` | Retenção retorna `parcial_na_lista_do_admin` (`:89`), antes de consultar A (`:90`). |
| `app/chamados/reconciliador.py:872` | Fora de costura em chamado já vivo (`:865`), barra nascer/renascer (`:875`). |
| `app/ponto/services/cluster_espurio.py:103` | Põe dia em `na_lista`; remove-o de `a_abrir` (`:107`); fechamento continua calculado separadamente (`:108`). |
| `app/chamados/management/commands/disparar_perguntas_competencia.py:154` | Incrementa barra `parcial_lista`, não pergunta (`:155-156`). |
| `app/ponto/management/commands/emitir_furo_retroativo.py:121` | Incrementa `na_lista`, não emite (`:122-123`). |

Registro de juízes:

- Há registro expresso para a pergunta **“a ausência do dia suspende, fecha ou levanta a cobrança viva?”**: `_C11`, `app/core/juizes.py:117`, autoridade `chamados/services/silencio_ausencia.py::reconciliar_por_ausencia`, `app/core/juizes.py:271`.
- C11 não chama A nem B: seleciona ausência vencedora para cobrança (`app/chamados/services/silencio_ausencia.py:65`); aprovada só fecha se `_premissa_morre` (`:93-94`), que exige veredito fora de `VEREDITOS_ACUSAM` (`:40-41`), com exceção `justificativa` (`:34-35`); fechamento efetivo passa por `ch.reconciliar()` (`:101`).
- Há C18 para **“a cobrança do dia cala porque a célula lavrada diz que o dia não é de trabalho?”**, `app/core/juizes.py:149`, autoridade `dia_sem_trabalho`, `:278`.
- O registro de ausência contém a pergunta de cobertura de dias (`app/core/juizes.py:241`) e a pergunta de período (`:249`); não há ali registro de A ou B como composição única para toda cobrança.
- `app/docs/JUIZES.md:59-67` documenta a autoridade `dia_sem_trabalho`, a exceção parcial que cobra resto, e a regra especial de A para `sem_periodo`.

Exemplo adicional relevante: para efeito de folha, ausência inteira não `ABONA` é excluída da dispensa de pontualidade por `app/ponto/turnos.py:2690`, embora `ausencia_explica_o_dia_inteiro` não teste efeito (`:2675`). Isso é diferença entre domínios/composições, não prova isolada de divergência A/B.

## 08/11 10:20 · sessao 4d73f0df7961 · - · - · origem=Ronald (tmux)

Grade e ata:

- `app/escala/utils.py:1209` consulta cobertura `para='folha'`; `:1210-1212` calcula a parcialidade por `cobre_parte_do_dia`. Para não-falta marca `tipo_dia='ausencia'` (`:1241`) e `cobertura_parcial=True` (`:1251`); chama `periodo_da_parcial_no_dia` (`:1255`). Se `sem_periodo`, não entra no ramo de dispensa (`:1265`): os missing permanecem. A limpeza de todas as células só ocorre em `not _parcial` (`:1272-1274`).
- `app/ponto/services/cartorio.py:647` faz `_acesa = isinstance(x, dict) and not x.get('missing')`; grava em `:677-679`. Sem DNA correspondente, o fallback também grava `acesa = not missing` (`:687-690`). Portanto os marcos missing conhecidos permanecem `acesa=False`; não significa que todas as lâmpadas do dia ficam falsas, pois batidas presentes acendem as respectivas lâmpadas (`:618-620`, `:647`).
- Há exceção de lâmpada sem coluna atribuível com fato cego: `acesa=None` (`app/ponto/services/cartorio.py:641-644`). Não transformar a afirmação sobre missing conhecidos numa garantia universal sobre todas as lâmpadas.
- `ata_do_dia` transporta tipo do dia e lâmpadas (`app/ponto/services/cartorio.py:701-705`), conta false em `n_missing` (`:712-713`), escreve `cobertura_parcial=True` (`:732-733`) e `periodo_parcial='sem_periodo'` (`:736-737`).
- `dispensar_por_ausencia` só aceita ausência aprovada SEM `dia['cobertura_parcial']` (`app/ponto/services/cartorio.py:766-769`); parcial sem período sai intacta. Se também houver `dia['justificado']`, existe outra fonte de dispensa (`:768`), que acende e marca `dispensada` (`:770-776`).
- Uso da montagem/dispensa de ata em produção: `app/ponto/services/cartorio.py:1210`, `:1386`, `:1800-1802`; o último lavra via `cel.lavrar_veredito`.
- `app/escala/services/leitor_celula.py:416-419` só transporta os dois carimbos da ata para o dia; não altera sem_periodo nem julga novamente.

Portão comum de emissão:

- `app/ponto/services/cartorio.py:370` define `parcial_so_lista`; sem `cobertura_parcial`, False (`:396-397`); com `periodo_parcial == 'sem_periodo'`, True incondicional (`:398-399`). Para parcial com período, True depende de julgamento anterior e ausência de fato novo (`:400`).
- Cartório calcula uma vez em `app/ponto/services/cartorio.py:1706-1707`; furo sem batida só chama emissor quando `apply_ and not _parcial_so_lista` (`:1725`, chamada `:1740-1742`); furo parcial só emite quando `apply_ and not vivos and not _parcial_so_lista` (`:1777-1780`). Logo não cria chamado de furo para sem_período que tenha os carimbos.
- `app/ponto/services/cartorio.py:1723-1724` e `:1775-1776` chamam `_contar_na_lista`. Este apenas incrementa contador em memória `parcial_na_lista_sem_push` e guarda chave em conjunto (`:1827-1832`): não escreve linha de lista.
- Chamado vivo pode continuar e ter marcos espelhados: `app/ponto/services/cartorio.py:1781-1789`, `:1790-1798`. O bloqueio de nascimento não é um fecho universal dos vivos.

Todos chamadores executáveis de `parcial_so_lista`, fora tests:

| Sítio | Efeito para parcial sem período |
|---|---|
| `app/ponto/services/cartorio.py:1706` | Cala os dois portões de novo chamado, `:1725`, `:1777`. |
| `app/chamados/services/fabrica_da_celula.py:102-104` | `motivos_a_perguntar` retorna `[]`: sem pergunta/pai/push; há guardas anteriores por dia/estado em `:91-100`. |
| `app/chamados/services/disputa_emissao.py:871-873` | `dia_na_lista_do_admin` retorna o predicado; sem dia/célula retorna False (`:864-870`). |
| `app/chamados/management/commands/disparar_perguntas_competencia.py:154-156` | Conta `parcial_lista`, pula nova pergunta do chamado mudo. |
| `app/ponto/management/commands/emitir_furo_retroativo.py:121-123` | Conta `na_lista`, pula emissão retroativa. |
| `app/ponto/services/cluster_espurio.py:102-107` | Exclui dias da lista de `a_abrir`; `reconciliar_colab` pula `abrir` em `:127-130`; `a_fechar` é calculado separadamente (`:108`), não pelo predicado. |

Leitores de `dia_na_lista_do_admin`:

- `app/chamados/services/disputa_emissao.py:894-896`: filtra chamados órfãos antes de criar disputa/pergunta; chamado existente não é fechado pela filtragem.
- `app/chamados/services/pergunta_no_app.py:88-91`: retorna causa `parcial_na_lista_do_admin` ANTES da causa `coberto_por_ausencia`. O rótulo e dono `'admin'` ficam em `:26-27`. `medir` transforma retenções de chamados existentes em itens (`:126-144`) e contagens (`:145-149`). O command `app/chamados/management/commands/chamados_vivos_sem_pergunta_no_app.py:14-21` imprime as contagens.

Supra:

- `app/ponto/supra_juiz.py:381-389`: com fato no dia encerrado + aprovado + sem_periodo, adiciona `PARCIAL_SEM_PERIODO`, texto “cobrar o CADASTRO”.
- Segundo ramo de batida sem par usa o mesmo código em `app/ponto/supra_juiz.py:437-442`.
- O dia sem fato pode seguir sendo FURO: a parcial sem período não é dia inteiro (`app/ponto/supra_juiz.py:268-269`), não ganha dispensa que satisfaça `_parcial_sem_resto` (`:311-312`), e o ramo de furo o alcança em `:313-316`. O cartório posteriormente cala a emissão por `parcial_so_lista`.
- `app/ponto/supra_juiz.py:530-534`: `_resto_cobrado` exige período diferente de sem_periodo; aprovado + chamado vivo sem esse resto produz `COBRANCA_EM_DIA_COBERTO`.
- `app/escala/models.py:1615` mapeia `PARCIAL_SEM_PERIODO` para `indefinida`; a ordem do mapa dá precedência a furo, cobrança indevida e coberta (`:1612-1615`).

Destino “lista do admin”, distinção comprovada:

- Existe promessa literal nos comentários de `app/ponto/services/cartorio.py:379-382` e texto da tela `app/templates/ponto/partials/painel_ausencia.html:296-299`: se parcial aprovada e sem `hora_ini`, mostra “sem periodo: o dia esta na lista do DP”.
- A lista real é `app/escala/views.py:484-492`, chamando `cadastro_realidade.lista`.
- Ela agrega explicitamente fontes de C1–C12 (`app/escala/services/cadastro_realidade.py:800-879`) e as A lavradas (`:886-887`, apresentação `:936-945`). C11 seleciona `veredito='indefinida'` mas **exige** protesto `PREVISTO_COM_DIA_INDEFINIDO` (`:477-478`), não `PARCIAL_SEM_PERIODO`.
- Não localizei publicador específico que coloque o código/dia `PARCIAL_SEM_PERIODO` nessa lista real. Presença efetiva por essa condição: **[não verificado]**. Bloqueio da emissão e contagem `parcial_na_lista_sem_push` estão comprovados, mas não são publicação de linha. [nao verificado]
- Existe caminho genérico A9 para ausência + pelo menos 2 batidas (`app/ponto/services/esmeril_espelho.py:133-134`), sem distinguir sem_período; é rotulado “juiz” (`:23`). Não garante que cada parcial sem período apareça na lista.

## 08/11 09:49 · sessao a44dd9306e29 · ATA-X-TURNO-PAUSA · verificador · origem=Code

**O delta corrige os achados 1 e 2 no caminho novo, mas não sustenta equivalência universal entre `marcos_do_dia` e `pausas_do_dia`. Há consulta adicional no caminho do motor e lacunas nos testes novos.** A comparação contra o main abaixo usa o diff integral fornecido; checkout histórico, execução dos testes e ocorrência em produção: [nao verificado]. (`app/ponto/motor_calculo_v2.py:1705–1754`; `app/ponto/services/cxr_na_lista.py:32–49`; `app/escala/services/cadastro_realidade.py:397–400`)

**A. Correção e contraexemplos**

- **Achado 1: corrigido.** Cada pausa real precisa estar longe, nas duas pontas, de **todas** as declaradas. Basta uma declarada descrevê-la para não acusar. O relato escolhe a menor soma das distâncias absolutas; o corte permanece estritamente `>180`, nas duas pontas. (`app/ponto/motor_calculo_v2.py:1705–1710`, `:2027–2030`)
- **Achado 2: corrigido para uma nova lavra.** Os quatro campos entram na allowlist e C12 possui ramo próprio. A leitura não recupera campos descartados de snapshots anteriores: `ultimo_lavrado` apenas devolve o detalhe persistido. Nova lavra em produção: [nao verificado]. (`app/ponto/services/cxr_na_lista.py:32–34`, `:91`, `:147–166`; `app/escala/services/cadastro_realidade.py:397–400`)
- **Dono sem `pausas_do_dia`: agora silencia.** Não há fallback para `hii/hfi`. Existem entradas desse formato nos testes: `_TE`, passado como template, e `_Vinculo`, passado como `escala_colaborador`. Logo, existe mudança no contrato aceito pelo motor; dono real de produção sem a irmã: [nao verificado]. (`app/ponto/motor_calculo_v2.py:1733–1735`; `app/ponto/tests/test_atraso_cross_midnight.py:8–26`, `:38–48`; `app/ponto/tests/test_motor_pareia_pelo_dna.py:48–60`, `:74–76`)
- **Template versus célula:** com célula existente e `dna.marcos` vazio/nulo, `marcos_do_dia` pode oferecer pausa do template, mas `pausas_do_dia` responde vazio. O delta deixa de acusar nesse estado; respeita a regra já existente de pausa declarada pela célula. (`app/escala/models.py:1124–1133`, `:1138–1158`, `:1189–1208`; `app/ponto/motor_calculo_v2.py:1742–1747`)
- **Noturno com duas pausas:** no exemplo construído `19–07`, pausas `22–23` e `02–03`, o helper ancora a primeira em D e a segunda em D+1, conservando `dia=D` no relato. Essa construção coincide com a do produtor de lâmpadas; o teste novo de duas pausas não exercita esse caso noturno. (`app/ponto/motor_calculo_v2.py:1749–1754`; `app/ponto/management/commands/gerar_celulas.py:55–69`; `app/ponto/tests/test_ata_pausa_l084_cxr.py:56–88`)
- **Ordem do cadastro:** não altera a existência da acusação, pois o predicado consulta todas. Em empate da distância mínima, vence a primeira declarada encontrada; portanto pode alterar o par publicado. A fonte preserva ordem cadastral, sem ordenar por relógio. (`app/ponto/motor_calculo_v2.py:1708–1710`; `app/escala/models.py:290–307`)
- **Dedupe por dia:** conserva somente a primeira pausa acusável daquele dia. Duas pausas divergentes distintas não produzem duas linhas. Isso é explícito e reinicia a cada cálculo; a lavra não consegue recuperar a segunda. (`app/ponto/motor_calculo_v2.py:1695`, `:1711–1723`, `:2364–2365`)

**B. Autoridade dos métodos e DNA**

O seletor usado pela acusação escolhe `escala_colaborador.marcos_do_dia`; na ausência desse método, escolhe `tipo_escala` ou `self.escala`. Nas duas classes reais examinadas, ambos os métodos existem. (`app/ponto/motor_calculo_v2.py:2768–2772`; `app/escala/models.py:281`, `:326`, `:1098`, `:1166`)

| Dono/estado | Relação entre primeira pausa e marcos |
|---|---|
| `TipoEscala` | A primeira pausa vem literalmente de `marcos_do_dia`. Override impede acrescentar a segunda do modelo; modo duração remove pausa fixa. (`app/escala/models.py:304–308`, `:335–345`) |
| EC com DNA contendo primeiro par completo | Ambos leem `hii/hfi` do mesmo DNA congelado. (`app/escala/models.py:1124–1132`, `:1189–1201`) |
| EC sem célula | Ambos recorrem ao template. (`app/escala/models.py:1133`, `:1208`) |
| EC com célula e marcos vazios/nulos | **Divergem:** marcos cai no template; pausas retorna vazio. (`app/escala/models.py:1124–1133`, `:1202–1208`) |
| EC com primeiro par incompleto/ausente e segundo completo | Marcos devolve o primeiro par literal; pausas elimina pares incompletos e pode devolver o segundo como `pausas[0]`. O produtor normal não gera sozinho essa configuração; ocorrência em produção: [nao verificado]. (`app/escala/models.py:1131–1132`, `:1196–1201`; `app/ponto/management/commands/gerar_celulas.py:81–93`) |

Portanto, **“mesmo dono” não implica “mesma resposta no par 1 em todo estado”**. A divergência da célula sem marcos é anterior ao delta e está explicada no próprio leitor de intervalo. (`app/escala/models.py:1138–1164`)

| Campo do DNA | Quem escreve | Quem lê |
|---|---|---|
| `hii/hfi` | `montar_dna`, a partir de `te.marcos_do_dia`. (`app/ponto/management/commands/gerar_celulas.py:81–84`) | EC.marcos, EC.pausas e mapa de marcos do período. (`app/escala/models.py:1131–1132`, `:1197–1200`; `app/escala/utils.py:103–107`) |
| `hii2/hfi2` | `montar_dna`, a partir de `pausas[1:]`. (`app/ponto/management/commands/gerar_celulas.py:92–93`) | EC.pausas e mapas de pausas do período. (`app/escala/models.py:1197–1200`; `app/escala/utils.py:57–61`, `:141–145`) |
| Todos os pares | Vocabulário e ordem em `CelulaDia.PARES_PAUSA_DNA`/`chaves_do_turno`; lâmpadas derivadas pelo gerador. (`app/escala/models.py:1652–1668`; `app/ponto/management/commands/gerar_celulas.py:60–69`) | Juiz da batida, cartório e papel da ata. (`app/ponto/juiz_batida.py:166–176`; `app/ponto/services/cartorio.py:581–586`; `app/ponto/turnos.py:1586–1587`, `:1711–1712`) |

A persistência do blob passa pela porta: geração/regeneração, edição, enriquecimento/geração e reversão que restaura `dna_anterior`. (`app/ponto/portas/celula.py:244–276`, `:591–609`, `:783–794`, `:848–870`)

**C. Quatro campos, `_txt`, dedupe e leitores**

| Campo | Quem escreve | Quem lê diretamente |
|---|---|---|
| `pausa_real` | Acusador: lista com dois instantes ISO localizados. (`app/ponto/motor_calculo_v2.py:1721`) | Allowlist/`medir`, igualdade do item e C12. (`app/ponto/services/cxr_na_lista.py:33`, `:91`, `:112`; `app/escala/services/cadastro_realidade.py:397–399`) |
| `pausa_cadastrada` | Acusador: lista ISO do par escolhido pela menor distância. (`app/ponto/motor_calculo_v2.py:1710`, `:1722`) | Mesmos leitores. (`app/ponto/services/cxr_na_lista.py:33`, `:91`, `:112`; `app/escala/services/cadastro_realidade.py:397–399`) |
| `delta_ida_min` | `(real−prevista)/60`, arredondado a uma casa. (`app/ponto/motor_calculo_v2.py:1705`, `:1723`) | Allowlist/`medir`, igualdade e texto C12. (`app/ponto/services/cxr_na_lista.py:34`, `:91`, `:112`; `app/escala/services/cadastro_realidade.py:399`) |
| `delta_volta_min` | Mesmo sentido e arredondamento. (`app/ponto/motor_calculo_v2.py:1706`, `:1723`) | Mesmos leitores. (`app/ponto/services/cxr_na_lista.py:34`, `:91`, `:112`; `app/escala/services/cadastro_realidade.py:400`) |

`_txt` alcança **todos os 14 campos da allowlist**, não somente as pausas. Agora qualquer lista/tupla vira texto unido por ` -> `; antes virava `str(v)`. Números permanecem números, inclusive `bool`; `None` permanece `None`; os demais tipos viram texto. No fluxo normal, valores `None` já são excluídos antes da chamada. (`app/ponto/services/cxr_na_lista.py:32–49`, `:91`)

Os produtores atuais dos campos antigos escrevem strings/números; os dois pares novos são listas. Assim, a mudança concreta de representação está nos pares: **lista no resultado do motor, string no snapshot**. Os deltas conservam representação numérica. (`app/ponto/motor_calculo_v2.py:1634–1639`, `:1682–1684`, `:1721–1723`, `:2073`; `app/ponto/services/cxr_na_lista.py:46–49`)

O `it in _do_colab` compara o item completo **depois** da conversão e do enriquecimento. Os quatro campos passam a distinguir relatos de pausas diferentes que antes poderiam colapsar. Não é dedupe somente por dia/causa; o dedupe por dia ocorre antes, dentro do motor. (`app/ponto/services/cxr_na_lista.py:89–112`; `app/ponto/motor_calculo_v2.py:1714–1723`)

A cadeia persistente é `medir → lavrar → MetricaSnapshot.detalhe → ultimo_lavrado → C12 → detalhe na lista/HTML/PDF`. A tela e o PDF recebem o texto formatado, sem interpretar os quatro valores separadamente. (`app/ponto/services/cxr_na_lista.py:147–166`; `app/escala/services/cadastro_realidade.py:397–409`, `:875–876`, `:930–932`, `:1110`; `app/templates/escala/cadastro_x_realidade.html:28`)

Outros leitores usam o envelope: fechamento concatena a lista; oráculo lê `dia`; `cxr_lavrado` lê identificação/data; relato de janela e resumo do espelho filtram causas próprias; `diff_janela_he` exclui expressamente `l084_pausa`. (`app/ponto/services/fechamento.py:96–97`, `:294–296`; `app/ponto/management/commands/e6_oraculo.py:170–171`; `app/ponto/services/cxr_na_lista.py:179–189`; `app/ponto/janela_he.py:277–282`; `app/ponto/services/espelho.py:1259–1261`; `app/ponto/management/commands/diff_janela_he.py:72–73`)

**D. Consultas no fechamento/lote**

**Sim: cada nova chamada elegível a `_turnos_do_juiz` acrescenta um SELECT de vínculos.** Ela fornece células e batidas, mas omite `escalas=`. O papel então chama `_escalas_da_ata`, com `select_related('tipo_escala')`: uma consulta, mesmo que depois nenhuma ata produza papel. O comentário “ALIMENTACAO, NUNCA CONSULTA” não descreve esse caminho. (`app/ponto/motor_calculo_v2.py:884–893`; `app/ponto/turnos.py:1561–1564`, `:1595–1597`, `:1634`)

O sítio antigo também omite `escalas=` e já pagava uma consulta. Nesta cópia, o nome da função é `turnos_via_autoridade`; `_parear_motor` aparece no comentário. (`app/ponto/motor_calculo_v2.py:453`, `:494–502`, `:885`)

Contando **somente esses SELECTs de escala para o papel**, em cálculo normal com batidas, vínculo e células elegíveis:

| Motor | Main pelo diff fornecido | Obra | Consultas acrescentadas |
|---|---:|---:|---:|
| Base/12x36 | 1 | 2 | +1. (`app/ponto/motor_calculo_v2.py:502`, `:2428`) |
| TurnoPartido | 1 | 3 | +2: chamada no `super` e novamente no override. (`app/ponto/motor_calculo_v2.py:2995`, `:3008`) |
| Comercial | 1 | 3 | +2: chamada no `super` e novamente via `_dia_do_turno`. (`app/ponto/motor_calculo_v2.py:3401`, `:3412`, `:898–899`) |

Sem as guardas `cid`, mapa alimentado e células desse colaborador, o sítio novo acrescenta zero. Isso não representa o total de consultas de `calcular_mes`. (`app/ponto/motor_calculo_v2.py:888–893`)

Os caminhos atingidos são:

- **Fechamento por fatia:** alimentação em `:750`, apuração em `:754`, cálculo por fatia em `:286`. (`app/ponto/services/fechamento.py:750–755`, `:278–291`)
- **Passada órfã:** alimentação em `:825`, cálculo com `ec=None` e `colaborador_id` em `:827–829`. O sítio novo pode consultar; o antigo depende de `_ec.colaborador_id` e não entra nesse ramo. (`app/ponto/services/fechamento.py:825–829`; `app/ponto/motor_calculo_v2.py:469`, `:496`, `:876`)
- **Espelho:** alimentação, fatias ou cálculo direto; **PDF:** chama o espelho. (`app/ponto/services/espelho.py:1131`, `:1164`, `:1179`, `:1227`; `app/relatorios/pdf_espelho.py:596–598`)

Há ainda custo específico do **delta v2**: fora do contexto de alimentação, cada turno passa a consultar célula duas vezes — marcos, já existente na v1, e pausas, acrescentada agora. Dentro do contexto e da janela, ambos usam o mapa. Turnos do pad fora da janela podem consultar, pois o helper não filtra `data_turno` pelo período pedido. (`app/escala/models.py:1048–1067`, `:1117–1123`, `:1182–1188`; `app/ponto/services/fechamento.py:278–291`; `app/ponto/motor_calculo_v2.py:1738–1754`)

Contagem executada e impacto de latência no lote: [nao verificado].

**E. Universo deduplicado: anterior à obra**

**Sim.** O fluxo normal limpa as batidas antes de `montar_periodos`; as implementações chegam a `_periodos_via_autoridade`, que chama o sítio antigo com essas batidas. Portanto, o papel do pareamento monetário já recebia universo deduplicado. (`app/ponto/motor_calculo_v2.py:2433–2436`, `:2835`, `:2962`, `:3300`, `:3619`, `:530`, `:502`)

`turnos_do_colab` carrega batidas vivas cruas e entrega essa lista ao papel. O papel conta por minuto e exige exatamente uma batida em cada luz. (`app/ponto/turnos.py:1866–1869`, `:1918`, `:1942`, `:1615–1620`, `:1659`)

Contraexemplo estático: duas entradas consecutivas `08:00:01 E` e `08:00:20 E`, no minuto de uma luz. O motor elimina a segunda; o papel sobre o universo cru encontra duas e rejeita a emissão daquele dia. Ocorrência desse exemplo em produção: [nao verificado]. (`app/ponto/motor_calculo_v2.py:736`, `:914–923`; `app/ponto/turnos.py:1659`)

Pelo diff integral fornecido, **essa divergência entre motor monetário e espelho é anterior**; a obra a estende ao novo consumo de papel em `_turnos_do_juiz`. (`app/ponto/motor_calculo_v2.py:502`, `:882–893`; `app/ponto/turnos.py:1942`)

**F. Checklist e força dos testes** [nao verificado]

| Classe | Resultado da leitura |
|---|---|
| Segundo juiz registrado | O par real continua vindo de `intervalos_do_turno`, e o corte reutiliza `_cadastro_nao_descreve`; são autoridades já registradas. O delta não repareia pausas reais. (`app/core/juizes.py:202`; `app/ponto/motor_calculo_v2.py:1708`, `:1753`; `app/ponto/turnos.py:540–544`) |
| Segundo escritor fora da porta | A acusação escreve memória; CXR continua persistindo pelo `lavrar` existente. A reconciliação da grade altera células/orfas em memória; o cartório entrega a ata à porta de lavra. (`app/ponto/motor_calculo_v2.py:1720–1723`; `app/ponto/services/cxr_na_lista.py:147–153`; `app/escala/utils.py:2131–2133`; `app/ponto/services/cartorio.py:1800–1802`) |
| Relógio fora da borda | O delta usa datas/instantes recebidos. A lavra continua recebendo `agora` do command, que lê o relógio na borda. (`app/ponto/motor_calculo_v2.py:1749–1754`; `app/ponto/services/cxr_na_lista.py:52–65`; `app/ponto/management/commands/lavrar_cxr_do_motor.py:30–40`) |
| Derivação paralela | Pausas declaradas vêm da irmã do dono escolhido. A conversão hora→instante permanece local e reproduz a regra do produtor; para duas pausas noturnas bem formadas, coincide. A equivalência universal das fontes, porém, é falsa pelos estados de B. (`app/ponto/motor_calculo_v2.py:1733–1752`; `app/ponto/management/commands/gerar_celulas.py:55–69`; `app/escala/models.py:1202–1208`) |
| Ordem de trava | Os trechos novos não introduzem transação nem aquisição explícita de trava; a ordem de publicação continua acusação → teto de pontualidade → resultado. Verificação concorrente em execução: [nao verificado]. (`app/ponto/motor_calculo_v2.py:1720–1754`, `:2802–2806`; `app/ponto/services/cxr_na_lista.py:147–153`) |
| Campo com dois significados | `dias_cadastro_x_realidade` inclui pausa, que não significa dia retirado da janela. O instrumento foi ajustado para essa diferença; os pares também mudam de lista para string ao atravessar a lavra. (`app/ponto/management/commands/diff_janela_he.py:70–73`; `app/ponto/motor_calculo_v2.py:1721–1723`; `app/ponto/services/cxr_na_lista.py:46–49`) |

Os testes novos **mordem a volta do comparador contra apenas a primeira pausa**, a retirada dos campos da allowlist e a retirada do ramo C12. Entretanto, o caso de duas pausas usa dono fake e intervalos mockados; o teste C12 monta o item manualmente e mocka `ultimo_lavrado`. (`app/ponto/tests/test_ata_pausa_l084_cxr.py:56–88`, `:96–117`)

Eles **não mordem retirar o ramo lista/tupla de `_txt`**: `str(lista_de_ISO)` ainda contém todos os horários exigidos. Tampouco exigem valores/sinais dos deltas ou atravessam `medir → lavrar → snapshot`; não cobrem o helper com EC real, célula vazia, dono sem irmã ou duas pausas noturnas. (`app/ponto/tests/test_ata_pausa_l084_cxr.py:72–88`, `:106–117`)

O teste de reset morde o reaproveitamento do dedupe, mas o teste de dedupe repete a mesma pausa; não demonstra qual relato sobrevive quando há duas divergências distintas no mesmo dia. (`app/escala/tests/test_ata_x_turno_pausa.py:332–350`; `app/ponto/tests/test_ata_pausa_l084_cxr.py:48–53`)

Finalmente, **“só acusa” vale para o acusador, não prova imobilidade financeira da obra inteira**: a nova alimentação de papel altera o caminho que produz o mapa do dia antes de `montar_periodos`. Ausência de diferença monetária nesta cópia: [nao verificado]. (`app/ponto/motor_calculo_v2.py:893–894`, `:2428–2436`, `:1720–1724`)

O restante do estado lido/escrito pelo delta fica assim:

| Valor/estado | Quem escreve | Quem lê |
|---|---|---|
| `_turnos_juiz_calc`, `entrada`, `data_turno` | Pareador produz turnos; `calcular_mes` guarda a resposta. (`app/ponto/turnos.py:321`; `app/ponto/motor_calculo_v2.py:2428`, `:2478`) | Helper filtra turnos e usa o dia explícito. (`app/ponto/motor_calculo_v2.py:1738–1754`) |
| `ida/volta` reais | `intervalos_do_turno`. (`app/ponto/turnos.py:543–544`) | Helper e acusador. (`app/ponto/motor_calculo_v2.py:1753–1754`, `:1705–1706`) |
| `hi/hf`, pausas declaradas | Métodos do dono descritos em B. (`app/escala/models.py:304–345`, `:1124–1133`, `:1189–1208`) | Helper ancora `previstas`; acusador compara e escolhe o par. (`app/ponto/motor_calculo_v2.py:1742–1752`, `:1705–1710`) |
| `dia`, `causa='l084_pausa'` | Acusador. (`app/ponto/motor_calculo_v2.py:1720`) | Lavra, C12, oráculo e filtro do instrumento. (`app/ponto/services/cxr_na_lista.py:91–107`; `app/escala/services/cadastro_realidade.py:399–409`; `app/ponto/management/commands/e6_oraculo.py:170`; `app/ponto/management/commands/diff_janela_he.py:73`) |
| `_l084_pausa_acusada` | Reset no cálculo; inclusão no acusador. (`app/ponto/motor_calculo_v2.py:2365`, `:1716`) | Guarda por dia. (`app/ponto/motor_calculo_v2.py:1714`) |
| `_cadastro_x_realidade` | Reset e append do acusador. (`app/ponto/motor_calculo_v2.py:2364`, `:1720–1723`) | Publicação no resultado e leitores de C. (`app/ponto/motor_calculo_v2.py:2806`; `app/ponto/services/cxr_na_lista.py:84–91`) |
| Campos antigos atravessados por `_txt` | Produtores de janela e L-084 do dia. (`app/ponto/motor_calculo_v2.py:1634–1639`, `:1682–1684`, `:2073`) | Allowlist, conversão, igualdade e ramos antigos de C12. (`app/ponto/services/cxr_na_lista.py:32–49`, `:91`, `:112`; `app/escala/services/cadastro_realidade.py:394–404`) |

## 08/11 09:46 · sessao 64acf49b617b · - · - · origem=Ronald (tmux)

A/B — conclusão sustentada pelo código:

- O delta cura o falso positivo da segunda pausa: cada pausa real agora precisa divergir de **todas** as declaradas; uma declarada que descreva ambas as pontas impede a acusação. A linha escolhe menor soma das distâncias absolutas. `app/ponto/motor_calculo_v2.py:1705-1710`.
- Dia obrigatório elimina fallback pelo instante da saída; grava diretamente o dia do turno recebido. `app/ponto/motor_calculo_v2.py:1688`, `:1720`, `:1754`.
- EC, tipo_escala e self.escala são as três opções do seletor; a prioridade é EC, depois tipo_escala ou self.escala. `app/ponto/motor_calculo_v2.py:2768-2772`.
- Ambas classes reais têm os dois métodos. No template, a primeira pausa vem literalmente de `marcos_do_dia`; override impede soma da segunda pausa e modo duração impede pausa fixa. `app/escala/models.py:281-308`, `:326-345`.
- **A afirmação de mesma autoridade em todo estado é falsa:** EC.marcos cai no template se célula existe com DNA/marcos nulo/vazio; EC.pausas retorna `()` nessa célula. Trata-se de diferença intencional anterior, documentada em `intervalo_do_dia`, e o delta passa a silenciar a acusação nesse estado. `app/escala/models.py:1124-1133`, `:1189-1208`, `:1138-1158`; `app/ponto/motor_calculo_v2.py:1742-1747`.
- Outra diferença possível: DNA com primeiro par ausente/incompleto e segundo completo faz EC.marcos devolver `(None,None)` ou meio par no primeiro, mas EC.pausas comprime completos e devolve a segunda como `pausas[0]`. `app/escala/models.py:1131-1132`, `:1196-1201`. A ocorrência em produção é [nao verificado].
- O produtor normal não cria sozinho essa segunda sem primeira: segundo par é escrito a partir de `pausas[1:]`, e TipoEscala só estende pares extras se o primeiro existir. `app/ponto/management/commands/gerar_celulas.py:81-93`; `app/escala/models.py:304-307`.
- **Dono sem irmã existe na interface exercitada pelos testes:** `_TE` tem só marcos e é passado ao motor/calcular_mes, e `_Vinculo` também tem só marcos e é passado como escala_colaborador. Com o delta esses donos ficam silenciosos; a volta1 ainda compararia hii/hfi. `app/ponto/tests/test_atraso_cross_midnight.py:8-26`, `:38-48`; `app/ponto/tests/test_motor_pareia_pelo_dna.py:48-60`, `:74-76`; guarda nova `app/ponto/motor_calculo_v2.py:1733-1735`. Dono real de produção sem irmã é [nao verificado].
- Noturno com duas pausas 19–07 / 22–23 e 02–03 recebe corretamente D e D+1 pelo `< hi`, mesma construção existente do produtor de lâmpadas. Não achei contraprova para esse caso bem formado. `app/ponto/motor_calculo_v2.py:1749-1752`; `app/ponto/management/commands/gerar_celulas.py:55-69`.
- Ordem cadastral é preservada, não ordenada por relógio; isso mantém identidade do primeiro par. No acusador, ordem só interfere em empate da soma de distâncias, porque `min` conserva primeiro candidato; existência da acusação independe da ordem. `app/escala/models.py:259-260`, `:290-307`; `app/ponto/motor_calculo_v2.py:1708-1710`.
- Dedupe é por dia e guarda apenas a primeira pausa divergente desse dia; duas divergências distintas não viram duas linhas. Reinicia em cada cálculo. `app/ponto/motor_calculo_v2.py:1711-1723`, `:2364-2365`. É redução explícita, não prova de cobertura de todas as pausas divergentes. `app/ponto/motor_calculo_v2.py:1695`.

Custo adicional da autoridade:

- Dentro de `celulas_do_ato`, tanto marcos como pausas leem o mapa, zero queries de célula; contexto cobre os cálculos das fatias. `app/escala/alimentacao.py:24-51`; `app/ponto/services/fechamento.py:277-291`; `app/escala/models.py:1117-1123`, `:1182-1188`.
- Fora do contexto ou fora da janela alimentada, o helper faz duas consultas de célula por turno: uma marcos já existente na v1 e **uma adicional pausas** no delta. Não há memoização por dia. `app/escala/models.py:1048-1067`, `:1122-1123`, `:1187-1188`; `app/ponto/motor_calculo_v2.py:1738-1744`.
- Turnos do pad podem cair fora da janela: helper varre todos sem filtrar data_turno e identidade só cobre ini..fim. `app/ponto/motor_calculo_v2.py:1738-1754`; `app/escala/models.py:1066-1067`.

DNA — escritores/leitores:

| Valor | Quem escreve | Quem lê |
|---|---|---|
| hii/hfi | `montar_dna`, via te.marcos: `app/ponto/management/commands/gerar_celulas.py:81-84` | EC.marcos `app/escala/models.py:1131-1132`; EC.pausas `:1196-1201`; mapas 4-tupla `app/escala/utils.py:103-107` |
| hii2/hfi2 | `montar_dna`, via pausas[1:]: `app/ponto/management/commands/gerar_celulas.py:92-93` | EC.pausas `app/escala/models.py:1196-1201`; mapas de pausas `app/escala/utils.py:57-61`, `:141-145` |
| Todos pares | Vocabulário central: `app/escala/models.py:1652-1668`; lâmpadas derivadas `app/ponto/management/commands/gerar_celulas.py:60-69` | juiz `app/ponto/juiz_batida.py:166-176`; cartório `app/ponto/services/cartorio.py:581-586`; veredito `app/ponto/services/veredito_celula.py:31-38`; papel `app/ponto/turnos.py:1586-1587`, `:1711-1712` |
| Persistência do DNA | Porta gera/regenera `app/ponto/portas/celula.py:244-276`; edição `:591-609`; reversão restaura blob anterior `:783-794`; geração `:852-870` | Métodos/fontes acima |

Cobertura:

- O novo teste de duas pausas morde a volta do comparador contra a primeira, mas usa dono fake e pausa do juiz mockada; não exercita EC/DNA, template real, noturno duplo, dono sem irmã ou célula sem marcos. `app/ponto/tests/test_ata_pausa_l084_cxr.py:56-88`.
- Há selo antigo do produtor para noite com dois pares, mas não é teste do novo helper. `app/escala/tests/test_celula_segundo_intervalo.py:159-170`.
- Não executei testes; aprovação/RED efetivo desta cópia é [nao verificado]. [nao verificado]

## 08/11 09:46 · sessao d2f0642fdd2e · - · - · origem=Ronald (tmux)

D: há consulta nova. Com `cid`, células alimentadas e ao menos uma célula desse colaborador, `_turnos_do_juiz` chama `papel_por_minuto_da_ata` sem `escalas=`; esta chama `_escalas_da_ata`, que executa **um SELECT** com `select_related('tipo_escala')`. O comentário “ALIMENTACAO, NUNCA CONSULTA” não corresponde ao caminho executado. `app/ponto/motor_calculo_v2.py:884`, `app/ponto/motor_calculo_v2.py:888`, `app/ponto/motor_calculo_v2.py:893`, `app/ponto/turnos.py:1596`, `app/ponto/turnos.py:1561`.

A consulta acontece mesmo sem batidas ou com ata ilegível, porque a escala é carregada antes da análise das células. Sem satisfazer as guardas do novo sítio, ele acrescenta zero consultas. `app/ponto/turnos.py:1596`, `app/ponto/turnos.py:1616`, `app/ponto/turnos.py:1634`, `app/ponto/motor_calculo_v2.py:888`.

| Caminho elegível | SELECTs novos por `calcular_mes` | Evidência |
|---|---:|---|
| Base/12x36/Plantão | +1 | `app/ponto/motor_calculo_v2.py:2428` |
| TurnoPartido | +2 | `app/ponto/motor_calculo_v2.py:2995`, `app/ponto/motor_calculo_v2.py:3008` |
| Comercial | +2 | `app/ponto/motor_calculo_v2.py:3401`, `app/ponto/motor_calculo_v2.py:3412`, `app/ponto/motor_calculo_v2.py:899` |

O sítio antigo também omite `escalas=` e já pagava um SELECT. O nome real da função nesta cópia é `turnos_via_autoridade`; `_parear_motor` aparece somente no comentário. Num cálculo normal elegível, contando apenas essas consultas de escala da ata, passa de 1 para 2 no Base e de 1 para 3 nos dois overrides acima. `app/ponto/motor_calculo_v2.py:453`, `app/ponto/motor_calculo_v2.py:494`, `app/ponto/motor_calculo_v2.py:502`, `app/ponto/motor_calculo_v2.py:885`.

Os caminhos atingidos são:

- Fechamento por fatia: alimenta células em `app/ponto/services/fechamento.py:750`, chama a apuração em `app/ponto/services/fechamento.py:754` e calcula cada fatia em `app/ponto/services/fechamento.py:286`.
- Passada órfã: alimenta em `app/ponto/services/fechamento.py:825`, calcula com `escala_colaborador=None` e `colaborador_id` em `app/ponto/services/fechamento.py:827`. Nesse ramo, o novo sítio pode consultar; o antigo depende de `_ec.colaborador_id` e fica desabilitado. `app/ponto/motor_calculo_v2.py:469`, `app/ponto/motor_calculo_v2.py:496`.
- Espelho: alimenta em `app/ponto/services/espelho.py:1131`, chama fatias em `app/ponto/services/espelho.py:1164` ou cálculo direto em `app/ponto/services/espelho.py:1179`; a tela pede essa autoridade em `app/ponto/services/espelho.py:1227`.
- PDF: chama o espelho em `app/relatorios/pdf_espelho.py:596`, `app/relatorios/pdf_espelho.py:598`.

`papel_por_minuto_da_ata` diretamente pode executar três consultas sem alimentação: células, escalas e batidas. Com células e batidas fornecidas, sem `escalas=`, executa uma; fornecendo também `escalas={}` ou mapa carregado, essas três consultas desaparecem. `app/ponto/turnos.py:1588`, `app/ponto/turnos.py:1596`, `app/ponto/turnos.py:1602`.

E: **sim, o sítio antigo recebe batidas deduplicadas** no fluxo normal de `calcular_mes`: limpa em `app/ponto/motor_calculo_v2.py:2433`, chama `montar_periodos(batidas_limpas)` em `app/ponto/motor_calculo_v2.py:2436`, as implementações delegam a `_periodos_via_autoridade`, que chama `turnos_via_autoridade`. `app/ponto/motor_calculo_v2.py:2835`, `app/ponto/motor_calculo_v2.py:2962`, `app/ponto/motor_calculo_v2.py:3300`, `app/ponto/motor_calculo_v2.py:530`.

`turnos_do_colab` carrega batidas vivas cruas e passa a lista ao papel sem essa deduplicação. O papel conta batidas por minuto e rejeita luzes com contagem diferente de um; também rejeita minuto ambíguo dentro do vão. `app/ponto/turnos.py:1866`, `app/ponto/turnos.py:1918`, `app/ponto/turnos.py:1942`, `app/ponto/turnos.py:1615`, `app/ponto/turnos.py:1659`, `app/ponto/turnos.py:1741`.

Contraexemplo estático: duas entradas consecutivas `08:00:01 E` e `08:00:20 E`, no minuto de uma luz, viram uma entrada no motor, porque o dedupe elimina o mesmo tipo com diferença menor que três minutos; no universo cru, a luz tem duas batidas e impede emissão do papel daquele dia. Ocorrência desse caso em produção: [nao verificado]. `app/ponto/motor_calculo_v2.py:70`, `app/ponto/motor_calculo_v2.py:736`, `app/ponto/motor_calculo_v2.py:914`, `app/ponto/motor_calculo_v2.py:921`, `app/ponto/turnos.py:1659`.

Pelo diff integral fornecido, essa diferença de universo entre o pareamento monetário do motor e `turnos_do_colab` é anterior à obra; a obra a estende ao novo consumo de papel em `_turnos_do_juiz`. Comparação independente com checkout do main e contagem real de consultas/latência: [nao verificado]. `app/ponto/motor_calculo_v2.py:502`, `app/ponto/motor_calculo_v2.py:882`, `app/ponto/motor_calculo_v2.py:893`, `app/ponto/turnos.py:1942`.

## 08/11 09:46 · sessao 1364412be57e · - · - · origem=Ronald (tmux)

C e F, apenas por leitura do código; execução dos testes e estado de produção [nao verificado]. [nao verificado]

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `pausa_real` | `_acusar_pausa_l084`: lista de dois ISO com fuso (`app/ponto/motor_calculo_v2.py:1721`) | allowlist/`medir` e `_txt` (`app/ponto/services/cxr_na_lista.py:33`, `:91`); igualdade do item (`:112`); C12 (`app/escala/services/cadastro_realidade.py:397`, `:399`) |
| `pausa_cadastrada` | Mesmo produtor, par declarado escolhido por distância mínima (`app/ponto/motor_calculo_v2.py:1710`, `:1722`) | Mesmos leitores (`app/ponto/services/cxr_na_lista.py:33`, `:91`, `:112`; `app/escala/services/cadastro_realidade.py:397`, `:399`) |
| `delta_ida_min` | `(real-prevista)/60`, arredondado a uma casa (`app/ponto/motor_calculo_v2.py:1705`, `:1723`) | allowlist, transformação, igualdade e texto C12 (`app/ponto/services/cxr_na_lista.py:34`, `:91`, `:112`; `app/escala/services/cadastro_realidade.py:399`) |
| `delta_volta_min` | Mesmo sentido `real-prevista`, arredondado (`app/ponto/motor_calculo_v2.py:1706`, `:1723`) | Mesmos leitores (`app/ponto/services/cxr_na_lista.py:34`, `:91`, `:112`; `app/escala/services/cadastro_realidade.py:400`) |

A lavra persiste o dicionário inteiro em `MetricaSnapshot.detalhe`, JSONField; `ultimo_lavrado` devolve esse detalhe sem reconstruir campos. Portanto snapshot anterior que perdeu os campos precisa de nova lavra; a leitura sozinha não o corrige (`app/ponto/services/cxr_na_lista.py:147`, `:150`, `:153`, `:157`, `:164`; `app/inteligencia/models.py:95`). Nova lavra em produção [nao verificado].

`_txt` aplica-se aos **14 campos** da allowlist, antes da deduplicação. `list/tuple` agora vira `' -> '.join(str(x)...)`; antes virava `str(v)`. `None` permanece `None`; `int/float` permanecem números, inclusive `bool` por herança de `int`; os demais valores viram texto. Os produtores atuais escrevem os campos antigos como strings/números e os pares novos como listas ISO, de modo que o efeito concreto novo aparece nos dois pares da pausa (`app/ponto/services/cxr_na_lista.py:32`, `:46`, `:91`; produtores antigos em `app/ponto/motor_calculo_v2.py:1634`, `:1682`, `:2073`; pares em `:1721`).

`it in _do_colab` compara o item **já convertido e enriquecido**. Os quatro campos passam a distinguir itens que antes seriam iguais por terem os dados da pausa descartados. A igualdade é do item completo; não é dedupe apenas por dia/causa. O dedupe por dia do motor continua anterior e elimina outras pausas acusáveis daquele dia; a lavra não recupera o que o produtor suprimiu (`app/ponto/services/cxr_na_lista.py:89`, `:91`, `:107`, `:112`; `app/ponto/motor_calculo_v2.py:1714`, `:1720`).

O texto atravessa C12 → `lista` → HTML/PDF: `_cxr_do_motor` formata os quatro valores em `detalhe`; `lista` incorpora a assinatura; HTML imprime `a.detalhe`; PDF também lê esse campo (`app/escala/services/cadastro_realidade.py:397`, `:405`, `:875`, `:930`, `:1110`; `app/templates/escala/cadastro_x_realidade.html:28`; `app/escala/views.py:487`, `:489`, `:492`).

Outros leitores do envelope não interpretam esses quatro valores: fechamento concatena a lista; oráculo usa apenas `dia`; `cxr_lavrado` usa `data/colab/competencia`; resumo do espelho filtra causa de chegada; `pontas_do_relato` exige “janela de HE”; instrumento `diff_janela_he` exclui `l084_pausa` expressamente (`app/ponto/services/fechamento.py:96`; `app/ponto/management/commands/e6_oraculo.py:170`; `app/ponto/services/cxr_na_lista.py:183`; `app/ponto/services/espelho.py:1259`; `app/ponto/janela_he.py:277`, `:281`; `app/ponto/management/commands/diff_janela_he.py:72`).

Os novos testes mordem a retirada da allowlist e do ramo C12: usam a saída real do produtor, conferem campos e exigem os horários no texto. Contudo **não mordem retirar o novo ramo list/tuple de `_txt`**: `str(lista_de_ISO)` ainda contém todos os horários exigidos. Também não verificam valores/sinais dos deltas, nem atravessam `medir → lavrar → snapshot`; montam `it` manualmente e mockam `ultimo_lavrado` (`app/ponto/tests/test_ata_pausa_l084_cxr.py:96`, `:102`, `:110`, `:112`, `:113`, `:115`, `:116`).

No trecho da lavra, o delta altera transporte/apresentação; o escritor persistente continua `lavrar`, chamado pelo command com um instante lido na borda. Não introduz relógio nesse serviço nem segundo escritor de snapshot (`app/ponto/services/cxr_na_lista.py:52`, `:65`, `:147`; `app/ponto/management/commands/lavrar_cxr_do_motor.py:30`, `:32`, `:40`).

## 08/11 09:30 · sessao 1eb7712af56e · ATA-X-TURNO-PAUSA · verificador · origem=Code

**Parecer: bloqueia o pouso.** Há falsa acusação com duas pausas, perda dos dados da pausa na lista do admin e divergências entre os caminhos que deveriam consultar o mesmo juiz (`app/ponto/motor_calculo_v2.py:2780`; `app/ponto/services/cxr_na_lista.py:30`; `app/escala/utils.py:2132`; `app/ponto/turnos.py:1659`).

A execução da suíte integrada, o impacto financeiro em produção e a história dos 821 commits permanecem **[nao verificado]**. As reproduções mencionadas abaixo executaram funções reais extraídas por AST em memória; não equivalem à execução completa com Django/banco **[nao verificado]**. [nao verificado]

**Achados — A, C e E**

1. **BLOQUEIA — duas pausas corretas produzem `l084_pausa` falsa.** O laço obtém apenas `hii/hfi` de `marcos_do_dia` e compara **todos** os intervalos reais com esse primeiro par. O main já possui `pausas_do_dia` para devolver todos os pares declarados (`app/ponto/motor_calculo_v2.py:2771`; `app/ponto/motor_calculo_v2.py:2780`; `app/escala/models.py:1166`).

   Caso concreto: previstas **09:15–09:45 e 14:15–15:40**, reais exatamente iguais. A segunda pausa é comparada com 09:15–09:45 e gera deltas **+300/+355 minutos**, suficientes para acusar cadastro incorreto. A reprodução isolada confirmou essa linha; o predicado exige ambas as distâncias absolutas maiores que 180 (`app/ponto/motor_calculo_v2.py:1699`; `app/ponto/motor_calculo_v2.py:1713`; `app/ponto/motor_calculo_v2.py:1989`).

   Isso também pode mudar o dono do dia para CADASTRO, porque E6 consulta a presença do dia na lista sem distinguir a causa; BATIDA ainda precede essa decisão (`app/ponto/management/commands/e6_oraculo.py:104`; `app/ponto/management/commands/e6_oraculo.py:110`; `app/ponto/management/commands/e6_oraculo.py:170`).

2. **BLOQUEIA — CXR-TELA perde os horários e deltas da pausa.** O produtor escreve `pausa_real`, `pausa_cadastrada`, `delta_ida_min` e `delta_volta_min`; a allowlist da lavra não copia esses campos. O formatador C12 reconhece somente os formatos antigos de janela HE e entrada/saída (`app/ponto/motor_calculo_v2.py:1713`; `app/ponto/services/cxr_na_lista.py:30`; `app/ponto/services/cxr_na_lista.py:86`; `app/escala/services/cadastro_realidade.py:394`).

   Para a linha do caso col841, o detalhe resultante é **“entrada real - x cadastrada - (None min), saida real - x cadastrada - (None min)”**. A causa sobrevive na lavra, mas esse ramo nem a apresenta; o admin recebe a acusação sem sua evidência (`app/escala/services/cadastro_realidade.py:398`; `app/escala/services/cadastro_realidade.py:404`).

3. **BLOQUEIA — a cura continua fora do juiz registrado de ocupação do marco.** `_match_marcos` é a autoridade registrada; o bloco novo muda sua resposta posteriormente, somente na grade. A Zona 5 e `marco_ocupado_pela_batida` continuam consultando a fonte sem essa reconciliação (`app/core/juizes.py:200`; `app/escala/utils.py:427`; `app/escala/utils.py:2132`; `app/ponto/motor_calculo_v2.py:3127`).

   Caso concreto do próprio teste: jornada **07–17**, pausa **12:38–13:38**, batidas **06:54 E / 12:38 S / 12:54 E / 17:05 S**. A grade curada exige **hfi=12:54, delta −44**. Na reprodução isolada, `marco_ocupado_pela_batida` com as quatro batidas devolveu `(None, None)` para 12:54; com somente o segundo período, devolveu `hii,+16`. Esse último é o formato passado por `cumpriu_a_volta_do_intervalo`, que recusa a classificação como volta (`app/escala/tests/test_ata_x_turno_pausa.py:139`; `app/escala/tests/test_ata_x_turno_pausa.py:154`; `app/escala/regua_defesa.py:491`; `app/escala/regua_defesa.py:493`).

   **Não é inteiramente redundante com o main:** o alinhamento por papel já senta os pares, mas seu cluster final ainda pode retirar uma perna; a nova reconciliação volta a acendê-la. Há duas respostas para a ocupação, em vez de uma cura completa na fonte (`app/escala/utils.py:561`; `app/escala/utils.py:577`; `app/escala/utils.py:2132`).

4. **BLOQUEIA — “uma entrada só” ainda recebe universos diferentes.** O motor remove duplicatas **antes** de produzir o papel; `turnos_do_colab` produz o papel sobre as batidas carregadas. Isso altera a guarda que exige exatamente uma batida por minuto de luz (`app/ponto/motor_calculo_v2.py:882`; `app/ponto/motor_calculo_v2.py:893`; `app/ponto/turnos.py:1659`; `app/ponto/turnos.py:1942`).

   Contraexemplo reproduzido isoladamente: DNA **07–17 / pausa 12–13**, ata com quatro luzes, cruas **E07:00:01, E07:00:05, S12:00, E13:00, E17:00**, última E gravada cumprindo a saída na ata. Com cruas, o papel fica vazio e surgem três turnos abertos; após o dedupe do motor, o papel é `E/Xi/Xi/S` e surge um turno fechado. A segunda E é removida pelo predicado de duplicata (`app/ponto/motor_calculo_v2.py:921`; `app/ponto/turnos.py:1659`; `app/ponto/turnos.py:1833`; `app/ponto/turnos.py:1380`).

5. **BLOQUEIA a declaração de neutralidade financeira da mudança (3).** O acusador, isoladamente, apenas acrescenta relato; a alteração do papel alimenta caminhos monetários: substituição de períodos, atribuição do dia, julgamento L084 da janela e crédito dos pares de turnos abertos (`app/ponto/motor_calculo_v2.py:1713`; `app/ponto/motor_calculo_v2.py:685`; `app/ponto/motor_calculo_v2.py:2396`; `app/ponto/motor_calculo_v2.py:2447`; `app/ponto/motor_calculo_v2.py:1594`; `app/ponto/motor_calculo_v2.py:2561`).

   Forma concreta noturna reproduzida: DNA **19–07 / pausa 03–04**, luzes **18:56/23:59/03:00/07:01**, cruas **E18:56:01, E18:56:05, S23:59, E03:00, E07:01**. Sem papel, a cauda das 03:00 fica em D+1; com papel após dedupe, fica em D, dentro do turno fechado. O mapa alterado participa dos filtros e da classificação; comercial/partido também o usam para agrupar e recalcular rubricas (`app/ponto/motor_calculo_v2.py:905`; `app/ponto/motor_calculo_v2.py:2392`; `app/ponto/motor_calculo_v2.py:2986`; `app/ponto/motor_calculo_v2.py:3390`). **Valor líquido e rubricas efetivamente alteradas nesse exemplo: [nao verificado].**

6. **FILA — outro contador continua tratando qualquer CXR como isenção de pontualidade.** `diff_reclassificar_partido` coleta toda a lista, soma `len()` e anuncia dias que “deixam de gerar atraso/saida antecipada”. A nova pausa aumenta esse número, embora apenas acuse; seus exemplos também procuram campos de entrada/saída inexistentes (`app/ponto/management/commands/diff_reclassificar_partido.py:188`; `app/ponto/management/commands/diff_reclassificar_partido.py:260`; `app/ponto/management/commands/diff_reclassificar_partido.py:262`; `app/ponto/management/commands/diff_reclassificar_partido.py:269`).

7. **FILA — a chamada nova consulta vínculo apesar do comentário “ALIMENTAÇÃO, NUNCA CONSULTA”.** A omissão de `escalas=` faz `papel_por_minuto_da_ata` chamar `_escalas_da_ata`, com consulta e escolha por `ativa/data_inicio`, em vez de receber diretamente o vínculo da fatia. A dívida dessa escolha por período já está documentada no carregador (`app/ponto/motor_calculo_v2.py:885`; `app/ponto/motor_calculo_v2.py:893`; `app/ponto/turnos.py:1596`; `app/ponto/turnos.py:1544`; `app/ponto/turnos.py:1561`). Impacto de desempenho: **[nao verificado]**.

**B — quem escreve e quem lê**

| Campo/valor | Escritores | Leitores e significado |
|---|---|---|
| `dias_cadastro_x_realidade` | Nasce no resultado; lista privada reiniciada e publicada: `app/ponto/motor_calculo_v2.py:289`, `:2326`, `:2784`. | Fatias concatenam, sem interpretar causa: `app/ponto/services/fechamento.py:96`, `:294`. |
| Itens `dia/causa` | L084 do dia: `app/ponto/motor_calculo_v2.py:1634`; janela HE: `:1682`; pausa: `:1713`; L084 antigo sem `causa`: `:2035`. | E6 projeta qualquer `dia` não nulo em conjunto de datas: `app/ponto/management/commands/e6_oraculo.py:170`. |
| Linha `l084_pausa` | Laço passa explicitamente **dia do turno**; horários são ISO com deslocamento, deltas são minutos: `app/ponto/motor_calculo_v2.py:2781`, `:1714`, `:1716`. | CXR converte `dia→data`, filtra competência e deduplica itens; perde os campos novos: `app/ponto/services/cxr_na_lista.py:86`, `:88`, `:95`, `:107`. |
| `_l084_pausa_acusada` | Criação/adição no acusador e reset por cálculo: `app/ponto/motor_calculo_v2.py:1706`, `:1709`, `:2327`. | Consulta de dedupe por dia no próprio acusador; nenhum outro consumidor encontrado: `app/ponto/motor_calculo_v2.py:1707`. |
| `_turnos_juiz_calc` | Atribuído por cálculo: `app/ponto/motor_calculo_v2.py:2440`. | Janela L084 `:1595`; crédito de aberto `:2561`; acusador novo `:2767`. |
| `_dia_turno_juiz_calc` | Atribuído por cálculo: `app/ponto/motor_calculo_v2.py:2441`. | Janela `:1594`; fallback da pausa `:1703`; relato L084 `:2040`. |
| `papel_por_minuto` | Produzido de ata/DNA: `app/ponto/turnos.py:1568`, `:1808`, `:1809`, `:1833`; alimentação nova: `app/ponto/motor_calculo_v2.py:893`. | Passado ao pareador em `app/ponto/turnos.py:1538`; consultado/aplicado em `:903`, `:1308`, `:1314`, `:1380`. |
| Órfãs e células do matcher | `app/escala/utils.py:615`, `:621`, `:655`, `:662`. | Alinhamento às colunas e mapas do dia: `app/escala/utils.py:1726`, `:1778`, `:1779`. |
| `dias_out[…]['celulas'/'orfas']` | Criação `app/escala/utils.py:1822`, `:1828`; conservação civil `:1853`, `:1857`, `:1859`; pontas `:2064`; pausa nova `:2131`. | Ata provisória `:1917`; guardas `:1992`, `:2111`; cartão `:1107`; cartório `app/ponto/services/cartorio.py:701`; espelho `app/ponto/services/espelho.py:685`; supra-juiz `app/ponto/supra_juiz.py:514`. |
| Overlays das mesmas células/listas | Isenção `app/escala/utils.py:858`; folga `:1173`; ausência `:1238`, `:1266`, `:1277`. | A ata provisória é construída depois desses overlays: `app/escala/utils.py:1895`, `:1917`. |

Os leitores **não atribuem todos o mesmo efeito à causa**: E6 usa pertencimento ao dia; CXR conta itens; C12 deveria apresentar evidências; os leitores de HE filtram causas específicas. Não encontrei conversão nova de dia do turno para dia civil nesses leitores; encontrei perda do conteúdo da pausa e o contador incorreto descritos acima (`app/ponto/management/commands/e6_oraculo.py:170`; `app/ponto/services/cxr_na_lista.py:126`; `app/escala/services/cadastro_realidade.py:394`; `app/ponto/janela_he.py:281`).

- **CXR muda número legitimamente:** aumenta `total`, `por_causa` e numerador do snapshot; o contrato declara contagem de **itens**, inclusive vários no mesmo dia (`app/ponto/services/cxr_na_lista.py:14`; `:123`; `:126`; `:147`).
- **`diff_janela_he`: não encontrei defeito novo no filtro.** Exclui explicitamente `l084_pausa` (`app/ponto/management/commands/diff_janela_he.py:72`).
- **Espelho/HE:** `pontas_do_relato` aceita causas contendo “janela de HE”; o resumo acima do teto aceita somente a causa de chegada. A pausa não entra nesses subconjuntos (`app/ponto/janela_he.py:281`; `app/ponto/services/espelho.py:1260`).
- **FechamentoMensal/lavra financeira:** não encontrei persistência da lista nem consumo direto dela para rubricas; a composição transporta a lista e o instrumento documenta que o fechamento não a persiste (`app/ponto/services/fechamento.py:97`; `app/ponto/services/fechamento.py:294`; `app/ponto/management/commands/diff_reclassificar_partido.py:169`).
- **Fallback `dia=None`: nota.** O mapa real indexa batidas gravadas E, mas a ida da pausa costuma ser S; o teste direto injeta artificialmente essa S no mapa. O chamador de produção evita essa falha passando `dia=` explicitamente (`app/ponto/motor_calculo_v2.py:905`; `app/ponto/tests/test_ata_pausa_l084_cxr.py:23`; `app/ponto/motor_calculo_v2.py:2781`).

**C — caminhos com e sem células**

| Caminho | Alimentação |
|---|---|
| Espelho | Recebe células: `app/ponto/services/espelho.py:1131`. |
| PDF | Recebe células: `app/relatorios/pdf_espelho.py:405`. |
| Fechamento/lote, fatias normais | Recebe células: `app/ponto/services/fechamento.py:750`. |
| Fechamento, batidas órfãs de vínculo | Também recebe células: `app/ponto/services/fechamento.py:825`. |
| `esmeril_espelho` | Cria/calcula sem alimentar células: `app/ponto/services/esmeril_espelho.py:161`, `:165`. |
| `vigiar_motor_autoridade` | Idem: `app/ponto/management/commands/vigiar_motor_autoridade.py:57`. |
| `diff_extrato_parcial` | Idem: `app/relatorios/management/commands/diff_extrato_parcial.py:61`, `:68`. |

O papel novo permanece `None` sem alimentação, sem células daquele colaborador ou sem identificador; ata desalinhada ou sem bordas suficientes também faz o produtor se abster. Portanto, fechamento/lote **não são genericamente o caminho sem células** no main atual (`app/ponto/motor_calculo_v2.py:886`; `:888`; `:891`; `app/ponto/turnos.py:1639`; `:1685`; `app/ponto/services/fechamento.py:750`).

**D — checklist**

- **Segundo juiz/derivação paralela: encontrado**, no pós-processamento da ocupação e nos universos diferentes do papel, achados 3 e 4 (`app/core/juizes.py:200`; `app/escala/utils.py:2132`; `app/ponto/motor_calculo_v2.py:882`).
- **Segundo escritor persistente fora da porta: não encontrei novo em produção.** O bloco altera a grade em memória; a persistência segue cartório → `lavrar_veredito` → porta (`app/escala/utils.py:2132`; `app/ponto/services/cartorio.py:1802`; `app/ponto/portas/celula.py:1028`).
- **Relógio novo fora da borda: não encontrei no diff.** Os acréscimos derivam papel, instantes dos marcos e relato (`app/ponto/motor_calculo_v2.py:882`; `:1699`; `:2777`).
- **Ordem de trava: não encontrei aquisição ou alteração de trava nos trechos novos** (`app/escala/utils.py:2084`; `app/ponto/motor_calculo_v2.py:2764`; `app/ponto/turnos.py:1537`).
- **Pré-requisito “juiz único”: não satisfeito**, pelo contraexemplo de ambiguidade no minuto (`app/ponto/turnos.py:1659`; `app/ponto/motor_calculo_v2.py:893`).
- **ESMERIL-SEMÂNTICA: encontrado**, tanto no contador que chama itens de pausa de dias isentos quanto na comparação da segunda pausa com a primeira (`app/ponto/management/commands/diff_reclassificar_partido.py:262`; `app/ponto/motor_calculo_v2.py:2780`).
- **Testes:** os testes da ata verificam grade final e convergência; os outros dois arquivos verificam predicado/dedupe e presença da acusação no espelho. Não certificam a igualdade com `_match_marcos`, os caminhos sem células, CXR-TELA ou neutralidade das rubricas (`app/escala/tests/test_ata_x_turno_pausa.py:105`; `:193`; `app/ponto/tests/test_ata_pausa_l084_cxr.py:27`; `:56`; `app/ponto/tests/test_juiz_do_par_uma_entrada.py:54`). RED por retirada/mutação e suíte completa: **[nao verificado]**.

**E — demais contraexemplos**

- **D+1 civil:** a escolha da lista usa o instante e a origem registrada em `_orfa_civil`; o relato continua com `data_turno`. Não encontrei erro novo nessa seleção para o caso normal de uma pausa noturna (`app/escala/utils.py:1847`; `:2092`; `app/ponto/motor_calculo_v2.py:2781`).
- **Célula/slot ausente:** interrompe a cura; **não encontrei escrita parcial** nessas guardas (`app/escala/utils.py:2116`; `:2125`).
- **Marco ocupado por outra hora:** impede a ascensão das duas pernas. A identificação de “já ocupada por esta batida” continua somente por HH:MM; colisão completa entre datas permanece **[nao verificado]** (`app/escala/utils.py:2119`; `:2125`).
- **Duas pausas na ata:** além da falsa acusação bloqueante, a cura visual pula qualquer caso com dois pares ou seis marcos; não cobre essa classe já suportada pelo main (`app/escala/utils.py:2087`; `app/escala/models.py:1166`).
- **FILA — expectativa reaplicada incompatível com o alinhamento atual:** no cenário com E02:05 extra e par conhecido 21:44–22:41, a fonte senta o par antes das soltas; a reprodução isolada deixa 02:05 órfã e `hii` ausente. O teste exige 02:05 sentada e nenhuma célula ausente. Resultado integrado desse fixture: **[nao verificado]** (`app/escala/utils.py:561`; `:564`; `:577`; `app/escala/tests/test_ata_x_turno_pausa.py:297`).

## 08/11 09:28 · sessao f4c15bc06c56 · - · - · origem=Ronald (tmux)

Achados da escala/ata:

1. **Bloqueia — cura fora do juiz registrado mantém respostas diferentes para o mesmo marco.** O juiz registrado é `_match_marcos` (`app/core/juizes.py:200`); o novo bloco reescreve sua resposta depois, em `app/escala/utils.py:2131`. No fixture 07:00–17:00, pausa 12:38–13:38, batidas 06:54 E / 12:38 S / 12:54 E / 17:05 S, a grade nova acende **hfi=12:54, delta −44** (`app/escala/tests/test_ata_x_turno_pausa.py:139`, `:153`). Executei os ASTs reais em memória: `marco_ocupado_pela_batida` responde **(None,None)** usando as quatro batidas; usando só o segundo período responde **hii,+16**. Esse segundo formato é precisamente o insumo de `cumpriu_a_volta_do_intervalo`, que retorna falso (`app/escala/regua_defesa.py:491`, `:493`). A Zona 5 consulta `_match_marcos` diretamente e seleciona P2 pelo hfi, sem a reconciliação nova (`app/ponto/motor_calculo_v2.py:3122`, `:3127`, `:3129`). Valor financeiro líquido resultante: **[nao verificado]**.

2. **Fila — expectativa antiga permanece em teste novo.** Com o turno conhecido contendo o par 21:44–22:41 e uma E 02:05 solta, o main senta o par antes das soltas (`app/escala/utils.py:561`, `:564`); E 02:05 perde a disputa do hfi já ocupado. O cluster depois deixa hii missing (`:577`). AST real de `_match_marcos` devolveu hi18:46/hii missing/hfi22:41/hf07:04, órfãs21:44 e02:05. O teste exige nenhuma missing, apenas21:44 órfã e02:05 sentada (`app/escala/tests/test_ata_x_turno_pausa.py:294`, `:297`). Pareador completo e suíte: **[nao verificado]**.

3. **Fila — duas pausas ficam fora da cura.** O bloco exige exatamente um par e quatro marcos (`app/escala/utils.py:2087`), embora o main produza a cadeia de todas as pausas e alinhe pares em ordem (`:1630`, `:1636`, `:539`). Exemplo condicionado a dois pares reconhecidos: jornada07–17, pausas10–10:15/14–14:15, batidas E07/S08/E08:30/S15/E15:30/S17. AST de `_alinhar_por_papel` deixa08/08:30 órfãs e primeira pausa missing; o bloco novo necessariamente pula (`:2087`). Reconhecimento desse turno pelo pareador completo: **[nao verificado]**.

4. **Nota — bloco ainda é vivo no golden; não basta removê-lo como redundante.** No golden, AST real deixa hii missing e21:44 órfã; hfi já recebe22:41. A causa é o cluster final (`app/escala/utils.py:577`). A nova escritura cura hii (`:2131`). O teste golden prende esse comportamento (`app/escala/tests/test_ata_x_turno_pausa.py:101`, `:105`).

Censo agrupado:

| Dado | Escritores | Leitores |
|---|---|---|
| Células/orfãs do matcher | `app/escala/utils.py:615`, `:621`, `:655`, `:662` | alinhamento das colunas `:1726`; mapas `:1778` |
| `dias_out[..]['celulas'/'orfas']` | criação `:1822`, `:1828`; conservação civil/I `:1853`, `:1857`, `:1859`; ordenação `:1863`; pontas `:2064`; pausa `:2131` | ata provisória `:1917`; reconciliação `:1992`, `:2111`; cartão `:1107`, `:1131` |
| Overlays das mesmas listas | folga `:1173`; ausência `:1238`, `:1266`, `:1273`, `:1277`; isenção `:858` | ata provisória montada depois, `:1917` |
| Grade final | — | cartório `app/ponto/services/cartorio.py:701`, `:708`, `:1373`; supra-juiz `app/ponto/supra_juiz.py:230`, `:305`, `:514`; espelho `app/ponto/services/espelho.py:361`, `:685` |

**Não encontrei nova escrita persistente de produção fora da porta.** A escrita nova é sobre listas da grade em memória (`app/escala/utils.py:2131`); a persistência permanece cartório→`lavrar_veredito` (`app/ponto/services/cartorio.py:1802`; `app/ponto/portas/celula.py:969`, `:1001`). Os testes salvam ata diretamente (`app/escala/tests/test_ata_x_turno_pausa.py:206`; `app/ponto/tests/test_juiz_do_par_uma_entrada.py:49`).

Guardas: célula/slot ausente impede cura (`app/escala/utils.py:2116`); marco ocupado por outra hora impede ambas as pernas (`:2119`, `:2125`); D+1 escolhe lista civil usando a origem da órfã e o instante (`:2092`). A comparação da célula já acesa continua só HH:MM (`:2119`); contraexemplo completo com colisão entre datas **[nao verificado]**.

Os testes da ata mordem a retirada do bloco no golden e na volta curta, mas verificam a grade final (`app/escala/tests/test_ata_x_turno_pausa.py:105`, `:146`). A convergência grava e relê a ata sem comparar o juiz de marcos (`:193`). Os outros dois arquivos prendem acusação/ligação no espelho, sem equivalência financeira ou caminhos sem células (`app/ponto/tests/test_ata_pausa_l084_cxr.py:55`; `app/ponto/tests/test_juiz_do_par_uma_entrada.py:53`). Não encontrei comparação nova contra `_match_marcos`, `cumpriu_a_volta` ou Zona 5 nesses três arquivos; execução da suíte **[nao verificado]**.

## 08/11 09:28 · sessao 4a100c439430 · - · - · origem=Ronald (tmux)

Achados da fatia motor/caminhos:

1. **bloqueia — o juiz ainda recebe universos diferentes.** `_turnos_do_juiz` remove duplicatas antes de derivar o papel; `turnos_do_colab` deriva o papel das batidas carregadas, sem esse dedup (`app/ponto/motor_calculo_v2.py:882`, `app/ponto/motor_calculo_v2.py:893`, `app/ponto/turnos.py:1942`). A guarda de ambiguidade exige exatamente uma batida em cada minuto de luz (`app/ponto/turnos.py:1659`).

   Contraexemplo executado em memória com funções reais extraídas por AST: DNA 07–17, pausa 12–13, quatro luzes na ata; batidas `E07:00:01 E07:00:05 S12:00 E13:00 E17:00`, última E gravada cumprindo S na ata. Com cruas: papel `{}` e três turnos abertos; com dedup: mapa `E/Xi/Xi/S` e um turno fechado 07–17. A segunda E é retirada pelo predicado de mesmo tipo e gap inferior a três minutos (`app/ponto/motor_calculo_v2.py:70`, `app/ponto/motor_calculo_v2.py:921`). Portanto “uma entrada só” não está satisfeito nessa forma (`app/ponto/turnos.py:1659`, `app/ponto/motor_calculo_v2.py:893`).

2. **bloqueia a declaração de neutralidade monetária da mudança (3) — seus resultados alimentam dinheiro.** O novo mapa é montado antes de `montar_periodos` e usado para excluir períodos substituídos pela ata; também classifica o dia, decide dispensa da janela L084 e credita períodos abertos (`app/ponto/motor_calculo_v2.py:2392`, `app/ponto/motor_calculo_v2.py:2396`, `app/ponto/motor_calculo_v2.py:685`, `app/ponto/motor_calculo_v2.py:2449`, `app/ponto/motor_calculo_v2.py:1594`, `app/ponto/motor_calculo_v2.py:2561`). Comercial e partido chamam esse juiz novamente para agrupar períodos e recalcular HE/intrajornada/feriado (`app/ponto/motor_calculo_v2.py:2986`, `app/ponto/motor_calculo_v2.py:3007`, `app/ponto/motor_calculo_v2.py:3030`, `app/ponto/motor_calculo_v2.py:3390`, `app/ponto/motor_calculo_v2.py:3415`).

   Forma concreta também exercitada por AST: DNA 19–07, pausa 03–04; luzes `18:56/23:59/03:00/07:01`; cruas `E18:56:01 E18:56:05 S23:59 E03:00 E07:01`. Sem papel, a cauda E03:00 fica em D+1; com papel após dedup, fica em D, num turno fechado único. Isso altera o insumo usado pelos ramos monetários acima; valor financeiro final desse caso **[nao verificado]**. Não confundir com `_acusar_pausa_l084`, cujo corpo apenas acrescenta relato (`app/ponto/motor_calculo_v2.py:1694`, `app/ponto/motor_calculo_v2.py:1713`).

3. **fila — “ALIMENTAÇÃO, NUNCA CONSULTA” não corresponde à chamada nova.** A chamada nova omite `escalas=`, levando obrigatoriamente a `_escalas_da_ata`, que consulta e escolhe vínculo por `ativa/data_inicio`; não usa diretamente o vínculo da fatia (`app/ponto/motor_calculo_v2.py:893`, `app/ponto/turnos.py:1596`, `app/ponto/turnos.py:1561`, `app/ponto/turnos.py:1563`). A dívida de escolha por período está documentada no próprio carregador (`app/ponto/turnos.py:1544`). Comercial/partido podem repetir essa carga na segunda chamada do juiz (`app/ponto/motor_calculo_v2.py:2986`, `app/ponto/motor_calculo_v2.py:3390`). Impacto de desempenho medido **[nao verificado]**.

4. **fila — os testes novos não certificam dinheiro nem todos os bocais.** O teste da entrada única só exige presença da acusação no espelho após lavar a ata; não afirma minutos, HE, atraso, intra ou noturno, nem fechamento/lote, nem cruas versus dedup (`app/ponto/tests/test_juiz_do_par_uma_entrada.py:54`, `app/ponto/tests/test_juiz_do_par_uma_entrada.py:61`). Os testes do acusador exercitam predicado/dedupe e presença no espelho, sem comparar essas rubricas (`app/ponto/tests/test_ata_pausa_l084_cxr.py:27`, `app/ponto/tests/test_ata_pausa_l084_cxr.py:36`, `app/ponto/tests/test_ata_pausa_l084_cxr.py:43`, `app/ponto/tests/test_ata_pausa_l084_cxr.py:56`). O teste direto injeta artificialmente a S da ida no mapa de dia, enquanto o produtor real só indexa batidas gravadas E; o laço real evita o problema passando `dia=` explicitamente (`app/ponto/tests/test_ata_pausa_l084_cxr.py:23`, `app/ponto/motor_calculo_v2.py:905`, `app/ponto/motor_calculo_v2.py:2781`). Eficácia por mutação/remover cura **[nao verificado]**.

Censo dos campos solicitados:

| Campo | Escritores | Leitores |
|---|---|---|
| `_turnos_juiz_calc` | atribuído em cada cálculo, `app/ponto/motor_calculo_v2.py:2440` | janela L084 do dia `:1595`; crédito aberto `:2561`, `:2566`; novo acusador `:2767` |
| `_dia_turno_juiz_calc` | `app/ponto/motor_calculo_v2.py:2441` | janela `:1594`; fallback acusador `:1703`; relato L084 `:2040` |
| `_dia_juiz_para_marco` | entrega `app/ponto/motor_calculo_v2.py:2396`; limpa em `finally` `:2400` | filtro de períodos/intervalos `:684`–`:687` |
| `papel_por_minuto` | produzido por `app/ponto/turnos.py:1568`, saídas `:1808`, `:1809`, `:1833`; `_papel` novo no motor `app/ponto/motor_calculo_v2.py:886`, `:893` | repassado `app/ponto/motor_calculo_v2.py:894`, `app/ponto/turnos.py:1538`; leitor `_papel_da_ata` `:888`, consulta `:903`; aplicação no pareador `:1308`, `:1314`, `:1380` |

Caminhos:

- **Espelho/PDF/fechamento/lote normal têm alimentação:** `app/ponto/services/espelho.py:1131`; `app/relatorios/pdf_espelho.py:405`; `app/ponto/services/fechamento.py:750`; caminho órfão `:825`. A apuração por fatia entrega colaborador, vínculo e datas ao motor (`app/ponto/services/fechamento.py:286`).
- **Sem alimentação:** `esmeril_espelho` cria motor e calcula sem atribuir células (`app/ponto/services/esmeril_espelho.py:161`, `:165`); `vigiar_motor_autoridade` idem (`app/ponto/management/commands/vigiar_motor_autoridade.py:57`); `diff_extrato_parcial` idem (`app/relatorios/management/commands/diff_extrato_parcial.py:61`, `:68`). Nesses caminhos o novo papel permanece `None`; células ausentes/vazias, cid ausente, ou nenhuma célula do cid também deixam `None` (`app/ponto/motor_calculo_v2.py:886`, `:888`, `:891`). Ata desalinhada ou dia sem bordas legíveis faz o produtor se abster (`app/ponto/turnos.py:1635`, `:1686`).
- **Bocal já existente:** `turnos_via_autoridade` já derivava e passava papel, com limites `AUT_*`; a obra migra o outro caminho do juiz, não cria o primeiro fornecimento de papel ao motor (`app/ponto/motor_calculo_v2.py:494`, `:500`, `:506`). Porém a receita nova não é literalmente a mesma: `turnos_de_batidas` usa os limites padrão do juiz; o outro caminho passa `AUT_INTRA_MAX_S/AUT_CONT_MAX_S` (`app/ponto/turnos.py:1537`, `app/ponto/motor_calculo_v2.py:504`).

Não encontrei relógio novo, aquisição de trava ou escrita persistente nova nessa fatia; o trecho acrescentado deriva papel e relato em memória (`app/ponto/motor_calculo_v2.py:882`, `:1694`, `:2764`, `app/ponto/turnos.py:1537`). A suíte Django não foi executada: o interpretador disponível não possui Django **[nao verificado]**; as sondas AST acima não executaram ORM nem criaram arquivos.

## 08/11 09:26 · sessao f14fc2900b56 · - · - · origem=Ronald (tmux)

Achados B:

1. **Bloqueia — nova acusação chega à C12 sem evidência da pausa.** O produtor grava `pausa_real`, `pausa_cadastrada`, `delta_ida_min` e `delta_volta_min`, mas a allowlist da lavra não inclui esses campos; a cópia conserva apenas `causa` e metadados. A apresentação C12 conhece apenas os formatos janela de HE e entrada/saída. Caso col841: aparece **“entrada real - x cadastrada - (None min), saida real - x cadastrada - (None min)”**, sem horários, deltas ou indicação textual da pausa. Reproduzi isso em memória com AST das funções reais e insumo sintético do caso, sem Django, banco ou arquivo criado. Fontes: `app/ponto/motor_calculo_v2.py:1713`, `app/ponto/services/cxr_na_lista.py:30`, `app/ponto/services/cxr_na_lista.py:86`, `app/escala/services/cadastro_realidade.py:394`.

2. **Fila — outro contador interpreta pausa como dia isento de pontualidade.** `diff_reclassificar_partido` captura toda a lista, soma `len()` e anuncia “dia-colab ... deixam de gerar atraso/saida antecipada”; uma pausa agora aumenta esse contador, embora o acusador não retire pontualidade. O exemplo ainda procura campos de entrada/saída inexistentes na linha da pausa. Fontes: `app/ponto/management/commands/diff_reclassificar_partido.py:188`, `app/ponto/management/commands/diff_reclassificar_partido.py:260`, `app/ponto/management/commands/diff_reclassificar_partido.py:262`, `app/ponto/management/commands/diff_reclassificar_partido.py:269`, `app/ponto/motor_calculo_v2.py:1713`.

3. **Nota — testes novos não cobrem essa integração com CXR-TELA.** O teste do espelho confere apenas `dia` e `causa` da lista em memória; não passa pela lavra C12 nem confere os campos/detalhe. Fonte: `app/ponto/tests/test_ata_pausa_l084_cxr.py:56`.

Mapa dos escritores:

- Campo público nasce no `ResultadoMes`; lista privada é zerada a cada cálculo e publicada no resultado. Fontes: `app/ponto/motor_calculo_v2.py:289`, `app/ponto/motor_calculo_v2.py:2326`, `app/ponto/motor_calculo_v2.py:2784`.
- Escritor L084 do dia: `{dia, causa='l084_do_dia', entrada/saida ISO, deltas em minutos}`. Fonte: `app/ponto/motor_calculo_v2.py:1634`.
- Escritor janela HE: `{dia, causa, real/cadastrado HH:MM, minutos_fora arredondados}`. Fonte: `app/ponto/motor_calculo_v2.py:1682`.
- Escritor pausa: `{dia, causa='l084_pausa', pares ISO, deltas em minutos}`; o laço passa explicitamente `data_turno`, inclusive para pausa civil D+1. Fontes: `app/ponto/motor_calculo_v2.py:1713`, `app/ponto/motor_calculo_v2.py:2781`.
- Escritor pontualidade L084 antigo: `{dia do mapa do juiz, entrada/saida ISO, deltas em minutos}`, sem `causa`. Fonte: `app/ponto/motor_calculo_v2.py:2035`.
- Composição de fatias concatena a lista, sem reclassificar causas. Fontes: `app/ponto/services/fechamento.py:96`, `app/ponto/services/fechamento.py:294`.
- Lavra CXR converte `dia` em `data`, filtra pela competência, copia campos permitidos, remove itens iguais e grava snapshot. Fontes: `app/ponto/services/cxr_na_lista.py:86`, `app/ponto/services/cxr_na_lista.py:87`, `app/ponto/services/cxr_na_lista.py:95`, `app/ponto/services/cxr_na_lista.py:107`, `app/ponto/services/cxr_na_lista.py:145`.

Mapa dos leitores:

- **E6:** lê qualquer `dia` não nulo, ignora `causa`/instantes/minutos e pergunta por pertença ao conjunto do dia. BATIDA permanece anterior a CADASTRO, portanto estar na lista não garante dono CADASTRO quando há batida ímpar/solta. Fontes: `app/ponto/management/commands/e6_oraculo.py:170`, `app/ponto/management/commands/e6_oraculo.py:195`, `app/ponto/management/commands/e6_oraculo.py:104`.
- **CXR-TELA:** conta itens, não dias; nova pausa aumenta `total`, `por_causa` e numerador lavrado legitimamente pelo rótulo declarado. Contudo perde os dados da pausa — achado 1. Fontes: `app/ponto/services/cxr_na_lista.py:14`, `app/ponto/services/cxr_na_lista.py:123`, `app/ponto/services/cxr_na_lista.py:126`, `app/ponto/services/cxr_na_lista.py:147`.
- **CXR lavrado para dono:** projeta os `data` dos itens em conjunto de datas; ignora causa e minutos. Fonte: `app/ponto/services/cxr_na_lista.py:178`.
- **C12:** lê a lavra e apresenta item por item; usa `data`, vínculo/template e dono lavrados. Fontes: `app/escala/services/cadastro_realidade.py:393`, `app/escala/services/cadastro_realidade.py:401`.
- **Espelho/pontas HE:** passa a lista a `pontas_do_relato`, que aceita apenas causas contendo “janela de HE”; pausa é ignorada. O resumo acima do teto aceita apenas “chegada fora da janela de HE”. Fontes: `app/ponto/services/espelho.py:741`, `app/ponto/janela_he.py:278`, `app/ponto/janela_he.py:281`, `app/ponto/services/espelho.py:1260`.
- **Diff janela:** exclui especificamente `l084_pausa`; não encontrei falha nova nesse filtro. Fonte: `app/ponto/management/commands/diff_janela_he.py:72`.
- **Fechamento/lavra de dinheiro:** o caminho por fatias transporta a lista composta; não encontrei persistência dela em `FechamentoMensal` nem consumo direto para rubricas. O próprio instrumento documenta que o fechamento não persiste a lista. Fontes: `app/ponto/services/fechamento.py:97`, `app/ponto/services/fechamento.py:294`, `app/ponto/management/commands/diff_reclassificar_partido.py:169`.
- **Calculador v3:** a captura de `_cadastro_x_realidade` em contexto de pontualidade vai para seu `Censo.cxr`, campo separado; não é segundo escritor do resultado público de produção. Fontes: `app/ponto/calculador/regras.py:275`, `app/ponto/calculador/periodo.py:283`.

`_l084_pausa_acusada`: escritor/leitor de dedupe no acusador, reinicialização por `calcular_mes`; não encontrei outro consumidor. Fontes: `app/ponto/motor_calculo_v2.py:1704`, `app/ponto/motor_calculo_v2.py:1707`, `app/ponto/motor_calculo_v2.py:1709`, `app/ponto/motor_calculo_v2.py:2327`.

Quanto a dia do turno versus civil, o novo laço passa `data_turno`; CXR e E6 conservam a chave recebida e não redatam a pausa pelo instante civil. Não encontrei conversão nova incorreta de unidade nesses leitores; a falha verificada é o descarte completo dos campos da pausa na C12. Fontes: `app/ponto/motor_calculo_v2.py:2781`, `app/ponto/services/cxr_na_lista.py:88`, `app/ponto/management/commands/e6_oraculo.py:170`.

## 08/11 09:25 · sessao c8d28fdc59f6 · FIO-E0-V4-E1 · verificador · origem=Code

**Parecer: REPROVA.** Há contraexemplo à igualdade entre o universo eleito pela porta e o universo de seus membros; além disso, o reuso admitido pelo juiz continua sujeito a leitores que tratam a data gravada como dia do turno. (`app/chamados/services/fio.py:1391`; `app/chamados/services/declaracao_texto.py:244`; `app/chamados/juizes.py:336`; `app/chamados/juizes.py:1988`.)

Os cenários abaixo são contraprovas construídas sobre o código. Sua ocorrência em produção, execução integrada dos testes e números SQL observados estão **[nao verificado]**. [nao verificado]

1. **Campos e memo — procedente, gravidade alta quanto ao significado de `data_evento`.**

   | Campo/valor | Quem escreve | Quem lê e qual significado usa |
   |---|---|---|
   | `data_evento` | `_nascer` grava o parâmetro; `abrir_conversa` fornece dia civil; `reapontar_resolvedora` data a existente; `rechavear_data_evento` troca a data. (`app/chamados/models.py:2707`; `app/chamados/models.py:2824`; `app/chamados/services/fio.py:840`; `app/chamados/services/fio.py:959`.) | Busca/índice usam a data gravada; o fecho passa a derivar a identidade pelo juiz. C6, validação de destino, plantio e expiração continuam lendo a gravada como dia relevante. (`app/chamados/models.py:2663`; `app/chamados/models.py:3298`; `app/chamados/services/fio.py:1235`; `app/chamados/juizes.py:336`; `app/chamados/juizes.py:1988`; `app/chamados/services/materializacao.py:344`; `app/chamados/veredito_lavrado.py:229`.) |
   | `batida_origem_id` | `_nascer` aceita explícita ou deriva do contexto; o `save` de inserção deriva quando ausente. (`app/chamados/models.py:2751`; `app/chamados/models.py:3254`.) | Identifica episódio no índice, busca, plano e agrupamento; `_dia_do_fato` carrega a Batida para perguntar seu dia ao juiz. É FK do episódio, não uma data. (`app/chamados/models.py:2632`; `app/chamados/models.py:3295`; `app/chamados/services/fio.py:1020`; `app/chamados/services/fio.py:1122`; `app/chamados/services/fio.py:1234`.) |
   | `instancia_marco` | `_chave_de_identidade` deriva do motivo e recusa explícita divergente; há backfill histórico. (`app/chamados/models.py:3243`; `app/chamados/models.py:3252`; `app/chamados/migrations/0051_decisao_pergunta.py:27`.) | Busca/índice/base do fecho usam posição do marco; `_dia_do_fato` usa seu preenchimento para reconhecer linha de marco. (`app/chamados/models.py:2663`; `app/chamados/models.py:3289`; `app/chamados/services/fio.py:1205`; `app/chamados/services/fio.py:1237`.) |
   | `celula_id` | Nascimento e reuso escolhem célula pela **data pedida**; rechaveamento escolhe pela nova data. (`app/chamados/models.py:2748`; `app/chamados/models.py:2782`; `app/chamados/models.py:2825`; `app/chamados/services/fio.py:956`; `app/chamados/services/fio.py:960`.) | C6 só aceita essa alimentação se `celula.data == data_evento`; outbox prefere a FK. `_dia_do_fato` não lê `p.celula.data`: passa `p.data_evento` ao conversor da célula. (`app/chamados/juizes.py:638`; `app/chamados/models.py:3047`; `app/chamados/services/fio.py:1239`.) |
   | `chamado_destino_id` | Nascimento/reuso em `_nascer`; reapontamento em `reapontar_resolvedora`. (`app/chamados/models.py:2825`; `app/chamados/models.py:2796`; `app/chamados/services/fio.py:868`.) | Âncora da listagem, validação da costura e julgamento do destino. O significado de FK de destino é consistente nesses caminhos. (`app/chamados/services/fio.py:117`; `app/chamados/models.py:2786`; `app/chamados/services/fio.py:856`; `app/chamados/services/fio.py:761`.) |

   **Não existe acordo global sobre `data_evento`.** O comentário novo declara chave gravada versus identidade derivada, mas `chave_do_dia` ainda se documenta como identidade, `_dia_de_turno_da_pergunta` devolve a gravada e o plantio combina a hora com ela. A declaração local não migrou esses leitores. (`app/chamados/services/fio.py:1219`; `app/chamados/services/fio.py:1198`; `app/chamados/juizes.py:332`; `app/chamados/services/materializacao.py:414`.)

   **Memo misto — não procedente a suspeita de colisão ou iteração incompatível.** Vizinhos usam chave de três elementos `(pk, chave_do_dia, bo)`; dia e colaborador usam chaves de dois elementos `('dia_do_turno', bo)` e `('colab', cid)`. Os consumidores acessam chaves específicas; não encontrei consumidor que percorra esse dicionário supondo valores exclusivamente de vizinhança. (`app/chamados/services/fio.py:1230`; `app/chamados/services/fio.py:1246`; `app/chamados/services/fio.py:1258`; `app/chamados/services/fio.py:1264`; `app/chamados/services/fio.py:1278`.)

2. **Propriedade fixa entre porta e membros — procedente, gravidade alta.**

   Contraexemplo: mesmo colaborador, motivo `gps_ausente`, nenhuma linha de acervo; alfa e beta são episódios distintos cujo juiz responde **18/10**. Cada episódio tem uma linha gravada em **10/09** e outra em **18/10**. Essa disposição cabe no índice: o horizonte é 21/09 e a unicidade do episódio só inclui linhas a partir dele. A premissa de duas batidas julgadas no mesmo dia está no teste novo. (`app/chamados/models.py:2313`; `app/chamados/models.py:3294`; `app/chamados/tests/test_e1_v13.py:59`.)

   | Consulta | Universo resultante |
   |---|---|
   | `universo_da_linha(A0)` ou `A1` | `{A0, A1}` |
   | `universo_da_linha(B0)` ou `B1` | `{B0, B1}` |
   | `universo_do_fato(colab, gps_ausente, 18/10, bo=None)` | `{A0, A1, B0, B1}` |

   A causa é concreta: sem episódio, `do_fato` traz as duas linhas gravadas em 18/10; ambas entram em `datadas`; o fecho preserva todas as sementes, sem exigir conectividade entre elas. Com episódio, a busca só liga o próprio episódio ao acervo sem episódio, inexistente neste cenário. Os resultados foram reproduzidos em memória com funções extraídas do código e ORM/juiz substituídos por stubs; integração e produção **[nao verificado]**. (`app/chamados/models.py:2630`; `app/chamados/models.py:2632`; `app/chamados/models.py:2636`; `app/chamados/services/fio.py:1320`; `app/chamados/services/fio.py:1381`; `app/chamados/services/fio.py:1391`.)

   **Os consumidores realmente divergem:** `_candidata_reusavel` chama sem episódio; `grupos_de_colisao` detecta as duas chaves de episódio e expande cada uma por fecho individual, produzindo dois grupos. O dedup consome esses grupos; a migração e `_colisao_ao_datar` começam pelo universo da linha. (`app/chamados/services/declaracao_texto.py:244`; `app/chamados/services/fio.py:1479`; `app/chamados/services/fio.py:1507`; `app/chamados/management/commands/dedup_perguntas_fato.py:58`; `app/chamados/services/fio.py:1634`; `app/chamados/services/fio.py:891`.)

   Há caminho de emissão sem episódio inferido: `gerar_perguntas_disputa` admite contexto sem `ultima_batida`, aplica seu dedup local e pode chegar a `abrir` com dia do contexto. Uma disputa nova não fica impedida pelas perguntas de outras disputas. A ocorrência desse cenário em produção está **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:325`; `app/chamados/services/disputa_emissao.py:327`; `app/chamados/services/disputa_emissao.py:372`; `app/chamados/services/disputa_emissao.py:444`.)

   **Assert de dois dias por chamador real — não demonstrado.** O agrupamento expande sementes individuais; `_colisao_ao_datar` começa por uma linha; a migração entrega ao plano membros do universo encontrado. Não obtive contraprova desse assert nesses caminhos. (`app/chamados/services/fio.py:1507`; `app/chamados/services/fio.py:891`; `app/chamados/services/fio.py:1634`; `app/chamados/services/fio.py:1641`.)

3. **Porta e célula — dois achados procedentes, gravidade alta.**

   **Nascimento sem linha anterior não verifica o dia da batida pedida.** Se o episódio ainda não possui perguntas, `universo_do_fato` não encontra linha para julgar. A recusa exige uma linha recusada daquele episódio; ausente essa linha, nasce pergunta com a data pedida e célula dessa data. Portanto `abrir(D0, batida cujo juiz diz DIA)` pode criar em D0; uma chamada direta posterior igual pode recusar a própria linha pelo juiz. (`app/chamados/services/fio.py:1362`; `app/chamados/services/fio.py:1376`; `app/chamados/models.py:2766`; `app/chamados/models.py:2748`; `app/chamados/models.py:2824`.)

   O caller de emissão não garante igualdade: grade/contexto têm precedência, e o juiz da última batida só é consultado quando falta a data anterior. Envolver `_dia_p` em `dia_do_turno_da_celula` não confere essa igualdade: o conversor devolve o valor recebido. (`app/chamados/services/disputa_emissao.py:371`; `app/chamados/services/disputa_emissao.py:378`; `app/chamados/services/disputa_emissao.py:424`; `app/chamados/services/disputa_emissao.py:451`; `app/ponto/turnos.py:2933`.)

   **Reuso correto pelo juiz pode falhar na costura.** Tome alfa gravada em 10/09, juiz em 18/10, como no teste novo; acrescente destino de escopo DIA declarado em 18/10. O proxy do pedido passa. A porta elege alfa e prepara célula de 18/10, mas conserva `ex.data_evento=10/09`. A segunda validação lê essa data antiga e lança `ResolvedoraCruzada`, pois a diferença supera um dia. O teste de reuso não fornece destino. (`app/chamados/tests/test_e1_v13.py:200`; `app/chamados/tests/test_e1_v13.py:203`; `app/chamados/models.py:2737`; `app/chamados/models.py:2782`; `app/chamados/models.py:2786`; `app/chamados/juizes.py:1988`; `app/chamados/juizes.py:2016`; `app/chamados/juizes.py:2023`.)

   Sem essa recusa, o estado persistido pode ficar com célula DIA e data gravada D0; C6 descarta a célula como alimentação e continua julgando pelo dia gravado. Logo atribuir a célula certa não reconcilia os leitores. (`app/chamados/models.py:2820`; `app/chamados/juizes.py:638`; `app/chamados/juizes.py:671`; `app/chamados/juizes.py:679`.)

   **Correção do inventário de callers:** os sítios de produção de `abrir` não passam `batida_origem_id` explicitamente; `_nascer` a infere. `reapontar_resolvedora` e `_colisao_ao_datar` não chamam `abrir`: usam a porta de existente. (`app/chamados/services/disputa_emissao.py:444`; `app/chamados/services/disputa_emissao.py:834`; `app/chamados/services/declaracao_texto.py:348`; `app/chamados/services/declaracao_texto.py:636`; `app/chamados/models.py:2751`; `app/chamados/services/fio.py:849`; `app/chamados/services/fio.py:868`.)

   **Não procede acusar automaticamente o agrupador de passar dia civil:** seu `localtime(...).date()` alimenta a classificação; a criação recebe `resolver_data_evento(...batida_origem=...)`, que consulta o juiz. (`app/chamados/services/disputa_emissao.py:1108`; `app/chamados/services/disputa_emissao.py:1114`; `app/chamados/services/disputa_emissao.py:1123`; `app/escala/servico_jornada.py:488`.)

4. **Juiz, derivação e relógio — não procedente como clonagem nova; inconsistência de leitores permanece.**

   `_dia_do_fato` delega a batida ao recorte do juiz registrado: `dia_do_turno_da_batida → dia_das_batidas → turnos_do_colab`. Não reproduz pareamento nem deriva dia do timestamp por conta própria. A ponte é a implementação expressamente descrita para linhas sem dia próprio. (`app/core/juizes.py:196`; `app/chamados/services/fio.py:1235`; `app/ponto/turnos.py:2925`; `app/ponto/turnos.py:2903`; `app/chamados/services/fio.py:1227`.)

   Os acréscimos de identidade/memo/contador não introduzem leitura direta de relógio. A catraca de calendário cobre `.date()` sobre timestamp e lookups `timestamp__date`; isso não detecta o conflito semântico entre o novo juiz e leitores antigos de `data_evento`. A correspondência histórica dessa catraca com `935c8d96` está **[nao verificado]**. (`app/chamados/services/fio.py:1229`; `app/chamados/services/fio.py:1268`; `app/chamados/services/fio.py:1410`; `app/ponto/tests/test_contract_no_batida_date.py:175`; `app/ponto/tests/test_contract_no_batida_date.py:203`; `app/chamados/juizes.py:336`.)

   Para marco, o código declara `p.data_evento` como dia de célula sem consultar a FK. Erro concreto adicional dependeria de marco com data gravada divergente da célula soberana; presença desse caso está **[nao verificado]**. (`app/chamados/services/fio.py:1237`; `app/chamados/services/fio.py:1239`.)

5. **Custo — procedente como amplificação, gravidade média; estouro de teto [nao verificado].**

   Considere **B** batidas existentes distintas examinadas, incluindo as alcançadas na ponte e recusadas:

   | Operação | Chamadas ao juiz pesado |
   |---|---|
   | Um fecho com memo novo | Uma por batida distinta examinada. |
   | Um `abrir` | Zero com busca vazia; caso contrário, B no universo/ponte/fecho compartilhado. |
   | Recusa `FatoEmOutroDia` | Mais uma para o episódio, pois o rótulo abre outro memo. |
   | `fecho_cruza_dias` | Soma dos B de cada fecho executado; não compartilha memo entre componentes. |

   Essas contagens decorrem das chaves por batida, do compartilhamento local e da criação de `_memo_t` separado. Batida inexistente custa a busca, mas não chama o juiz. (`app/chamados/services/fio.py:1230`; `app/chamados/services/fio.py:1234`; `app/chamados/services/fio.py:1339`; `app/chamados/services/fio.py:1392`; `app/chamados/models.py:2772`; `app/chamados/services/fio.py:1428`.)

   **Memo por batida não basta para compartilhar trabalho por turno/janela:** cada batida distinta reconstrói `dia_das_batidas(d−1,d+1)`, carregando turnos e batidas novamente, mesmo com janelas sobrepostas. (`app/ponto/turnos.py:2925`; `app/ponto/turnos.py:2903`; `app/ponto/turnos.py:2905`; `app/ponto/turnos.py:1863`; `app/ponto/turnos.py:1884`.)

   Os selos citados têm coberturas diferentes: chokepoint impõe tetos SQL; modal monta 330 perguntas sem episódio; `test_porta_chamado_e1` mede atos/no-ops de Chamado; cadeia/ordem verificam transação e precedência de travas, sem impor teto de consultas. Não são prova conjunta do novo custo da porta por episódio. (`app/ponto/tests/test_selo_chokepoint_escrita.py:397`; `app/chamados/tests/test_selo_teto_modal.py:68`; `app/chamados/tests/test_porta_chamado_e1.py:57`; `app/chamados/tests/test_e1_v13_cadeia.py:32`; `app/chamados/tests/test_e1_v13_ordem.py:59`.)

6. **Candidatas de `fecho_cruza_dias` — não procedente a falta de cobertura, sob as invariantes de nascimento.**

   Todo passo entre linhas distintas ocorre por chave gravada ou por episódio/motivo. O contador seleciona chave com múltiplas linhas e episódio com múltiplas chaves; assim cobre ambas as formas de aresta. Colaborador e instância são normalizados no nascimento. (`app/chamados/models.py:2630`; `app/chamados/models.py:2632`; `app/chamados/models.py:2636`; `app/chamados/services/fio.py:1416`; `app/chamados/services/fio.py:1421`; `app/chamados/models.py:3236`; `app/chamados/models.py:3243`.)

   Dois episódios distintos na mesma data, sem acervo, são candidatas excedentes, mas seus fechos não recusam vizinhos e não entram no resultado. A confirmação usa o fecho real. (`app/chamados/models.py:2632`; `app/chamados/services/fio.py:1428`; `app/chamados/services/fio.py:1431`.)

   O contador, porém, acrescenta membros quando existe **qualquer** recusa; não verifica novamente se sua causa foi outro dia, outra base ou ponte ambígua. Garantia irrestrita para acervo que viole as invariantes, ou ocorrência de falso positivo nesse acervo: **[nao verificado]**. (`app/chamados/services/fio.py:1340`; `app/chamados/services/fio.py:1431`.)

7. **Classes de reprovação e selos.**

   - **Segundo escritor fora da porta — não procedente no censo AST de `data_evento`.** Executei em memória o scanner existente: retornou somente `_nascer`, `reapontar_resolvedora` e `rechavear_data_evento`, sem sítio fora das portas. Isso verifica localização da escrita, não correção do significado. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:30`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:122`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:143`.)
   - **Ordem de trava — nenhuma inversão nova demonstrada.** `_nascer` trava Colaborador antes do universo; as leituras novas de Batida/Colaborador não pedem `FOR UPDATE`. Execução dos selos de ordem e concorrência: **[nao verificado]**. (`app/chamados/models.py:2724`; `app/chamados/models.py:2764`; `app/chamados/services/fio.py:1234`; `app/chamados/services/fio.py:1252`.)
   - **RED v13 sem mordida — não procedente contra a variante simples por data gravada.** O teste exige B0, gravada D0, dentro do fato DIA; voltar a decidir pela gravada a exclui. Também exige reuso de A0 em outra data e recusa de beta pedido em D0. (`app/chamados/tests/test_e1_v13.py:108`; `app/chamados/tests/test_e1_v13.py:121`; `app/chamados/tests/test_e1_v13.py:157`; `app/chamados/tests/test_e1_v13.py:198`.)
   - **Lacunas dos novos selos — procedente, gravidade média.** O cenário de união de episódios usa acervo como ponte; o teste de reuso omite destino. Não cobrem as contraprovas dos itens 2 e 3. (`app/chamados/tests/test_e1_v13.py:78`; `app/chamados/tests/test_e1_v13.py:203`.)
   - **Preservação byte a byte dos v5/v8/v10/v11, RED executado, mutante executado e passagem dos tetos:** **[nao verificado]**.
   - **Campo com dois significados não reconciliados — procedente, gravidade alta:** a identidade nova usa o juiz, enquanto destino, C6 e plantio continuam usando a data gravada como dia do fato. (`app/chamados/services/fio.py:1235`; `app/chamados/juizes.py:1988`; `app/chamados/juizes.py:336`; `app/chamados/services/materializacao.py:414`.)

_2854 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
