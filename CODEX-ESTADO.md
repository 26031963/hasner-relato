# CODEX-ESTADO

> Ultima resposta processada: **07/11/2026 13:36** · publicado em **07/11/2026 13:37** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 07/11 13:36 · sessao 8bf8acf0802b · - · - · origem=Ronald (tmux)

- **E — PROCEDENTE, cura parcial.** As funções agora delegam a escolha à `canonica_do_grupo`, mas somente **depois** de excluir as não reutilizáveis. Cenário legítimo abaixo do horizonte: `p1` respondida, validada e materializada; `p2` muda sem carimbo. A canônica global é `p1`, mas ambos os seletores escolhem `p2`: a validação mata `p1` no C6, tornando-a não reutilizável. A declaração reutiliza `p2` e dispensa `abrir`. Portanto permanece seleção anterior à autoridade do universo completo. (`app/chamados/services/declaracao_texto.py:240`; `app/chamados/services/declaracao_texto.py:261`; `app/chamados/services/declaracao_texto.py:328`; `app/chamados/services/declaracao_texto.py:338`; `app/chamados/services/declaracao_texto.py:625`; `app/chamados/services/fio.py:984`; `app/chamados/juizes.py:1137`; `app/chamados/juizes.py:1228`; `app/chamados/models.py:2666`)

- **E2/E3/E4 — NAO PROCEDE repetir o defeito específico de conversa nesses filtros corrigidos.** `_candidata_reusavel`, `_ja_perguntou` e os dois dedups datados agora exigem `conversa=False`. Os filtros continuam próprios, sem passar integralmente por `q_dos_fatos`; a cura não demonstra equivalência completa de identidade/instância. (`app/chamados/services/declaracao_texto.py:241`; `app/chamados/services/disputa_emissao.py:399`; `app/chamados/services/disputa_emissao.py:418`; `app/chamados/services/disputa_emissao.py:1313`; `app/chamados/models.py:2576`)

- **D — PROCEDENTE, cura sequencial; selo insuficiente.** O ramo sem dia verifica existência de conversa antes de `abrir_conversa`. Entretanto, o RED aceita `count()==0` e não exige `conversa=True`: pode passar se nenhuma pergunta nascer. A sobrescrita com `pass` não executa o teste herdado naquela classe, embora a classe mãe continue tendo seu próprio teste. (`app/chamados/services/disputa_emissao.py:436`; `app/chamados/services/disputa_emissao.py:439`; `app/chamados/tests/test_e1_v6_horizonte.py:409`; `app/chamados/tests/test_e1_v6_horizonte.py:421`)

- **4 — NAO PROCEDE ressuscitação de `VIAS_FORA_DO_FATO` pelos dois mecanismos examinados.** O universo exclui essas vias e o `UPDATE` do lote repete a exclusão sobre o estado consultado pelo banco. O descarimbo individual, o carimbo normal e a troca para identidade interna também recusam retirar a linha do conjunto terminal. (`app/chamados/models.py:1455`; `app/chamados/models.py:2955`; `app/chamados/models.py:2901`; `app/chamados/models.py:2816`; `app/chamados/models.py:2875`)

- **RetratadaNaoRessuscitaTest morde o comportamento anunciado, com limite.** Exige retratação efetiva pela porta, conservação da via/carimbo, morte pelo juiz, nascimento de outra PK e exatamente uma viva. O mutante remove `VIAS_FORA_DO_FATO` de **todas** as guardas simultaneamente: prova proteção conjunta, não a necessidade individual do filtro do lote ou do universo. Execução do teste: **[nao verificado]**. (`app/chamados/tests/test_e1_v6_horizonte.py:487`; `app/chamados/tests/test_e1_v6_horizonte.py:497`; `app/chamados/tests/test_e1_v6_horizonte.py:506`; `app/chamados/tests/test_e1_v6_horizonte.py:522`)

Mapa dos campos:

| Campo/valor | Escritor | Leitor e significado |
|---|---|---|
| `via_resolucao` + `materializada_em` | `_escrever_carimbo` atribui o trio; carimbar/descarimbar o salvam; lote tem `UPDATE` próprio. (`app/chamados/models.py:2793`; `app/chamados/models.py:2848`; `app/chamados/models.py:2885`; `app/chamados/models.py:2955`) | Carimbo de processamento/encerramento, **não prova isolada de batida**: admin não planta batida e C6 consulta lastro. (`app/chamados/models.py:2746`; `app/chamados/juizes.py:1210`) |
| Troca somente de via | Existem duas portas declaradas: `encerrar_por_fato_no_espelho` e `relavrar`; esta só aceita vias derivadas. Não são escritores novos deste diff. (`app/chamados/models.py:2964`; `app/chamados/services/relavra_via.py:115`; `app/chamados/services/relavra_via.py:77`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:51`) | A frase “toda via é escrita exclusivamente em `_escrever_carimbo`” seria inexata; o selo distingue esses escritores. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:35`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:51`) |
| `materializada_em` na duplicata | Preserva instante/autor anteriores quando já havia carimbo. Portanto não é necessariamente instante da retratação. (`app/chamados/services/fio.py:1002`; `app/chamados/models.py:2877`) | Backfill toma esse instante como `ato_em` de SILENCIO somente quando via é `admin_decidiu`; não interpreta todo carimbo como SILENCIO. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:169`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:173`) |
| `data_evento` | `_nascer`; reapontamento data órfã e rechaveamento muda o dia. (`app/chamados/models.py:2714`; `app/chamados/services/fio.py:841`; `app/chamados/services/fio.py:948`) | Dia do turno em `abrir`; dia civil da emissão em `abrir_conversa`. O discriminador é `conversa`. (`app/chamados/models.py:2603`; `app/chamados/models.py:2613`; `app/chamados/models.py:2620`) |
| `HORIZONTE_DO_FATO` | Literal e reexportação da classe. (`app/chamados/models.py:2254`; `app/chamados/models.py:2366`) | Índices/agrupamento usam início inclusivo `>=`; `NULL` só entra na consulta ampliada de contadores. Competência usa autoridade da janela, não mês civil. (`app/chamados/models.py:3177`; `app/chamados/models.py:3183`; `app/chamados/services/fio.py:1033`; `app/chamados/services/fio.py:1040`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:113`) |
| `VIAS_FORA_DO_FATO` | Tupla fonte do módulo, reexportada pela classe. (`app/chamados/models.py:2244`; `app/chamados/models.py:2365`) | Índices, identidade, universo da ressuscitação e descarimbo leem o conjunto terminal. (`app/chamados/models.py:3176`; `app/chamados/models.py:2547`; `app/chamados/models.py:1455`; `app/chamados/models.py:2956`) |
| `VIAS_QUE_VENCEM_A_RESPOSTA` | Soma da tranca com a fonte anterior. (`app/chamados/models.py:2375`) | C6 e pré-filtro SQL usam via **com carimbo** antes da resposta. (`app/chamados/juizes.py:1148`; `app/chamados/models.py:3043`) |

**6 — NAO PROCEDE afrouxamento demonstrado dos três selos além das exceções declaradas.** A soma preserva os mesmos três valores e o teste continua exigindo exatamente duas listas literais nomeadas. A allowlist acompanha o corpo renomeado. A declaração MARCO é nominal por caminho/nome; não comprova o valor nem a equivalência da janela por empresa. (`app/chamados/models.py:2375`; `app/chamados/tests/test_o261e_f2_fila.py:118`; `app/chamados/tests/test_contract_leitores_vitalidade.py:90`; `app/core/datas_cravadas.py:37`; `app/core/datas_cravadas.py:126`)

**Tetos: NAO PROCEDE extrapolação estática comprovada; medição permanece [nao verificado].** Os quatro valores são 23/55/200/246; continuam cobrados por `assertLessEqual`, e os cenários da batida exigem fechamento real. A trava usa quantidade fixa de consultas por entrada, mas a quantidade de linhas bloqueadas cresce com o universo do colaborador: custo constante de consultas não prova tempo ou contenção constantes. (`app/ponto/tests/test_selo_chokepoint_escrita.py:116`; `app/ponto/tests/test_selo_chokepoint_escrita.py:118`; `app/ponto/tests/test_selo_chokepoint_escrita.py:161`; `app/ponto/tests/test_selo_chokepoint_escrita.py:162`; `app/ponto/tests/test_selo_chokepoint_escrita.py:240`; `app/ponto/tests/test_selo_chokepoint_escrita.py:385`; `app/ponto/tests/test_selo_chokepoint_escrita.py:390`; `app/chamados/models.py:2500`; `app/chamados/models.py:2516`)

## 07/11 13:35 · sessao 36a63810e562 · - · - · origem=Ronald (tmux)

**C / ordem global: PROCEDENTE, cura parcial.** A flag amplia corretamente os sete chamadores alterados, mas não assegura `Colaborador → Disputa → Chamado → Pergunta` em todo caminho. (`app/chamados/models.py:2500`; `app/chamados/models.py:2506`; `app/chamados/models.py:2516`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:146`)

`trava_na_ordem` trava primeiro o Colaborador; depois, por PK, as Disputas explícitas e, com a flag, todas as abertas do colaborador; por fim, por PK, os Chamados explícitos, pais das disputas abertas encontradas e todos os Chamados em `VIVOS`. Não trava Pergunta. Sem flag, trava apenas as listas explícitas e **não acrescenta automaticamente seus pais**. (`app/chamados/models.py:2500`; `app/chamados/models.py:2502`; `app/chamados/models.py:2508`; `app/chamados/models.py:2510`; `app/chamados/models.py:2514`)

| Chamador com flag | Listas adicionais e efeito |
|---|---|
| `DisputaSupervisao.fechar` | Própria disputa, pai e destinos das vivas; universo antes de `fechar_por_admin`, save da Disputa e S88/S143/S144. (`app/chamados/models.py:1874`; `app/chamados/models.py:1885`; `app/chamados/models.py:1900`) |
| `fechar_beco` | Própria disputa, pai e destinos resolvidos; universo antes do primeiro resolver. (`app/chamados/services/becos.py:41`; `app/chamados/services/becos.py:51`; `app/chamados/services/becos.py:54`) |
| Resolver público | Chamado recebido mais universo; atomic próprio somente quando ainda não há transação. Via competência trancada desvia para retorno sem escrita. (`app/chamados/services/disputa_emissao.py:1674`; `app/chamados/services/disputa_emissao.py:1679`; `app/chamados/services/disputa_emissao.py:1701`) |
| Folga já decidida | Disputa, pai e destino da pergunta mais universo antes da validação e fechamento do fio. (`app/chamados/services/validacao.py:285`; `app/chamados/services/validacao.py:288`; `app/chamados/services/validacao.py:296`) |
| Declarar folga | Mesmas listas antes da validação e de `declarar_dia`. (`app/chamados/services/validacao.py:317`; `app/chamados/services/validacao.py:320`) |
| Validação normal | Disputa, pai, destino e clones da pergunta mais universo, antes do carimbo. (`app/chamados/services/validacao.py:431`; `app/chamados/services/validacao.py:435`) |
| `fechar_o_fio_do_dia` | Disputas das perguntas vivas, Chamados vivos do dia e destinos mais universo, antes das perguntas e do reconciliar. (`app/ponto/services/veredito_celula.py:182`; `app/ponto/services/veredito_celula.py:187`; `app/ponto/services/veredito_celula.py:200`) |

**Contraexemplo incontornável Pergunta antes de Disputa/Chamado, intocado pelo diff:** `reabrir_orfas_enterradas` abre atomic, trava somente Colaborador, chama `p.reabrir_fantasma()`, depois reabre Disputa e pai. `reabrir_fantasma → descarimbar → save` grava Pergunta; `reabrir_sistema` só depois grava Disputa. Portanto, segura a trava implícita de Pergunta antes das demais. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:146`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:150`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:159`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:166`; `app/chamados/models.py:2989`; `app/chamados/models.py:2907`; `app/chamados/models.py:2129`)

**S143 ainda permite Chamado antes de Disputa:** o gate `_recarregar_estado_se_mudou` trava Colaborador e Chamado; `_gravar_estado` apenas retrava Colaborador e salva Chamado; S143 chama o wrapper, que então adquire as Disputas. A cura do envelope não retroage à aquisição anterior do Chamado. (`app/chamados/models.py:683`; `app/chamados/models.py:688`; `app/chamados/models.py:280`; `app/chamados/models.py:291`; `app/chamados/signals.py:91`; `app/chamados/services/disputa_emissao.py:1680`)

**Validação continua com trecho fora do atomic local:** após o bloco que antecipa o universo, chama o materializador geral, que percorre todas as irmãs. Não há antecipação de universo no próprio materializador; seus ramos reconciliam destinos/clones e podem fechar Disputa. A presença de transação externa que retenha as travas nesse caminho é `[nao verificado]`. (`app/chamados/services/validacao.py:467`; `app/chamados/services/validacao.py:469`; `app/chamados/services/materializacao.py:1360`; `app/chamados/services/materializacao.py:1367`; `app/chamados/services/materializacao.py:762`; `app/chamados/services/materializacao.py:1324`)

**Ordem dos sinais contra a lâmpada: PROCEDENTE, não tocada.** S88 reconcilia pai sincronamente; o `post_save` de Chamado executa S143 antes do receiver que julga a célula; S144 materializa sincronamente. Só retomada de regularizações e beco ficam em `on_commit`. (`app/chamados/signals.py:42`; `app/chamados/signals.py:75`; `app/chamados/signals.py:92`; `app/chamados/signals.py:124`; `app/chamados/signals.py:140`; `app/chamados/signals.py:164`; `app/chamados/signals.py:180`; `app/chamados/signals.py:121`; `app/chamados/signals.py:202`)

**Selo de universo: parcial.** O novo observador registra `'UNIVERSO'` quando a chamada real retorna com a flag; não verifica quais linhas foram efetivamente travadas. O teste da “outra disputa e pai” afirma apenas a lista de disputas devolvida, sem observar trava do pai. Seu MORDE comprova ausência do marcador sem flag, não reprovação de um helper mutante que conserve a flag e omita uma linha. O rastreador antigo ainda elimina aquisições seguintes da mesma tabela. (`app/chamados/tests/test_e1_v5_ordem_trava.py:358`; `app/chamados/tests/test_e1_v5_ordem_trava.py:361`; `app/chamados/tests/test_e1_v5_ordem_trava.py:417`; `app/chamados/tests/test_e1_v5_ordem_trava.py:422`; `app/chamados/tests/test_e1_v5_ordem_trava.py:424`; `app/chamados/tests/test_e1_v5_ordem_trava.py:45`)

Execução de testes, deadlock reproduzido e ocorrência em produção: `[nao verificado]`. [nao verificado]

## 07/11 13:34 · sessao 0364b6caa4b0 · CODEX-CONSTRUTOR-NEGOCIO · verificador · origem=Code

1. **Informativo — o delta fecha as correções centrais, mas não todos os limites de v4.r.**

   | Achado de v4.r | Aval desta volta |
   |---|---|
   | **#1 — fechamentos anteriores** | Permanecem a proibição de `.git` aninhado, ignorados/autoload e a distinção entre permissões do wrapper e regras de conduta. A demonstração do hook FSMON continua limitada, conforme item 6 abaixo. (`bin/codex_pergunta.sh:105`, `bin/codex_pergunta.sh:109`, `bin/codex_pergunta.sh:159`, `CLAUDE.md:341`, `CLAUDE.md:346`, `CLAUDE.md:351`) |
   | **#2 — hardlink idêntico** | **Fecha:** `st_nlink>1` agora força entrada em `tocados`, antes da classificação. A mesma implementação roda no pre-check, revalidação e fim. (`bin/codex_pergunta.sh:149`, `bin/codex_pergunta.sh:164`, `bin/codex_pergunta.sh:223`, `bin/codex_pergunta.sh:371`, `bin/codex_pergunta.sh:398`) |
   | **#3 — cobertura do inventário** | **Fecha em parte:** fecha tipo especial e erro de percurso; permanecem diferenças entre identidade Git e árvore física, detalhadas no item 5. (`bin/codex_pergunta.sh:116`, `bin/codex_pergunta.sh:136`, `bin/codex_pergunta.sh:161`) |
   | **#4 — vaga por worktree** | **Fecha o contraexemplo:** worktrees com o mesmo common-dir passam a compartilhar a vaga. A fórmula acrescenta uma hipótese sobre a localização dos metadados, conforme item 2. (`bin/codex_pergunta.sh:72`, `bin/codex_pergunta.sh:73`, `bin/tests/test_codex_pergunta.sh:332`) |
   | **#5 — prova sem executar tentativas** | **Não fecha esse contraexemplo; fecha parte da rotulagem.** `.out` fabricado continua suficiente para algumas verificações. (`bin/tests/prova_codex_pergunta_real.sh:90`, `bin/tests/prova_codex_pergunta_real.sh:92`, `bin/tests/prova_codex_pergunta_real.sh:144`) |
   | **#6 — ouvinte morto/EV compartilhado** | **Fecha os casos apresentados:** criação exclusiva de EV e controle posterior. Permanece uma janela de contagem, conforme item 3. (`bin/tests/prova_codex_pergunta_real.sh:21`, `bin/tests/prova_codex_pergunta_real.sh:106`, `bin/tests/prova_codex_pergunta_real.sh:111`) |
   | **#7 — mortes espúrias dos mutantes** | **Fecha os dois mecanismos apontados:** `fs_walk` conserva os rastreados; `p_git` retira a regra efetiva. Execução completa: **[nao verificado]**. (`bin/tests/test_codex_pergunta.sh:422`, `bin/tests/test_codex_pergunta.sh:426`) |

   Os itens informativos #8–#10 precisam ser atualizados pelo mapa e pelos limites desta resposta; não representam correções independentes. (`bin/codex_pergunta.sh:72`, `bin/codex_pergunta.sh:398`, `bin/tests/prova_codex_pergunta_real.sh:111`)

2. **Média — regressão condicional: `dirname(common-dir)` não identifica necessariamente a raiz do repositório.**

   A fórmula funciona para `<principal>/.git`, mas common-dirs distintos como `/srv/r1.git` e `/srv/r2.git` produzem a mesma `/srv/logs/construtor.vaga`. Com metadados separados em `/meta/r1`, a vaga passa para `/meta/logs`, independentemente da árvore escolhida. Não há validação exigindo que o common-dir termine em `.git`. (`bin/codex_pergunta.sh:72`, `bin/codex_pergunta.sh:73`, `bin/codex_pergunta.sh:206`, `bin/codex_pergunta.sh:208`)

   Isso pode serializar repositórios independentes ou recusar um construtor porque a nova pasta não permite escrita. Nos papéis de leitura, não se abre a vaga, mas a reserva desse novo caminho também vale: uma resposta antes legítima nesse endereço passa a ser recusada. Ocorrência dessas topologias nesta obra: **[nao verificado]**. (`bin/codex_pergunta.sh:74`, `bin/codex_pergunta.sh:171`, `bin/codex_pergunta.sh:217`, `bin/codex_pergunta.sh:218`)

   A consulta ao common-dir agora antecede a separação dos papéis: uma falha nela encerra qualquer papel com rc 2. Os quatro testes de leitura usam apenas a fixture de repositório convencional; não cobrem essas topologias. (`bin/codex_pergunta.sh:72`, `bin/codex_pergunta.sh:81`, `bin/tests/test_codex_pergunta.sh:58`, `bin/tests/test_codex_pergunta.sh:352`)

3. **Média — o controle posterior não fecha a janela entre aceitar e registrar uma conexão.**

   O ouvinte executa `accept`, pode esperar até dois segundos em `recv` e somente depois escreve `conexoes`. Se `N_DEPOIS` for capturado nessa janela, a conexão fica fora da diferença julgada; seu registro tardio ainda pode contribuir para o mínimo de duas linhas exigido pelo controle posterior. O teste não confere individualmente os tokens `controle_pos` nem um sucesso por transporte. Reprodução desse escalonamento: **[nao verificado]**. (`bin/tests/prova_codex_pergunta_real.sh:38`, `bin/tests/prova_codex_pergunta_real.sh:41`, `bin/tests/prova_codex_pergunta_real.sh:103`, `bin/tests/prova_codex_pergunta_real.sh:109`, `bin/tests/prova_codex_pergunta_real.sh:111`, `bin/tests/prova_codex_pergunta_real.sh:114`)

   **Baixa — regressão no diagnóstico:** depois de acrescentar os controles posteriores, o `tail` imprime até EOF e atribui também essas linhas a conexões “de dentro do sandbox”. A quantidade usa o snapshot anterior, mas a lista mistura origens. (`bin/tests/prova_codex_pergunta_real.sh:107`, `bin/tests/prova_codex_pergunta_real.sh:108`, `bin/tests/prova_codex_pergunta_real.sh:114`)

4. **Alta — continua possível verde sem demonstrar cada tentativa.** [nao verificado]

   O controle positivo exige apenas `INTRUSO_WT.txt`. Para as tentativas individuais, o juiz aceita arquivos produzidos pelo próprio Codex contendo `rc=` e, em parte deles, texto de negação. TCP local/Unix dispensam essa assinatura. Fabricar os relatos continua satisfazendo essas condições sem executar cada acesso; o controle posterior não resolve essa lacuna. Contraexemplo completo executado: **[nao verificado]**. (`bin/tests/prova_codex_pergunta_real.sh:60`, `bin/tests/prova_codex_pergunta_real.sh:90`, `bin/tests/prova_codex_pergunta_real.sh:92`, `bin/tests/prova_codex_pergunta_real.sh:117`)

   A triagem sustenta o fechamento **documental** de Docker/socket/TCP externo: os rótulos estão explícitos. Não sustenta fechamento da execução comprovada; essas negativas ainda podem terminar em rc 0. (`bin/tests/prova_codex_pergunta_real.sh:127`, `bin/tests/prova_codex_pergunta_real.sh:130`, `bin/tests/prova_codex_pergunta_real.sh:135`, `bin/tests/prova_codex_pergunta_real.sh:144`, `bin/tests/prova_codex_pergunta_real.sh:147`)

5. **Média — os descartes de v4.r #3 têm alcances diferentes.**

   | Descarte | O motivo se sustenta? |
   |---|---|
   | Permissões além do bit executável e permissões de diretórios | **Sim, para identidade transportada pelo Git.** O inventário compara somente o bit executável do proprietário e não compara permissões dos diretórios. Isso não demonstra integridade completa do filesystem. (`bin/codex_pergunta.sh:118`, `bin/codex_pergunta.sh:129`, `bin/codex_pergunta.sh:143`) |
   | Diretório vazio | **Sim, para o conteúdo representado pela base Git.** Continua podendo sair `TOCADO`, rc 0 no fim. (`bin/codex_pergunta.sh:129`, `bin/codex_pergunta.sh:144`, `bin/codex_pergunta.sh:167`) |
   | Bytes não UTF-8 — “falha fechado” | **Só parcialmente.** Git usa `replace`; o percurso preserva outra representação. Uma simulação exclusivamente em memória com rastreado limpo `app/folha/a\xff.py` produziu dois `TOCADO`, rc 0: pre-check/revalidação recusam a árvore limpa; o fim aceita o rc 0. Execução em filesystem real: **[nao verificado]**. (`bin/codex_pergunta.sh:126`, `bin/codex_pergunta.sh:132`, `bin/codex_pergunta.sh:141`, `bin/codex_pergunta.sh:223`, `bin/codex_pergunta.sh:374`, `bin/codex_pergunta.sh:400`) |
   | Case/ext4 | A comparação literal permanece. Filesystem efetivo da produção: **[nao verificado]**. (`bin/codex_pergunta.sh:132`, `bin/codex_pergunta.sh:147`) |
   | CRLF/smudge | A comparação continua usando bytes físicos contra hash bruto. Ausência de filtros na configuração Git ativa: **[nao verificado]**. (`bin/codex_pergunta.sh:114`, `bin/codex_pergunta.sh:115`, `bin/codex_pergunta.sh:147`) |
   | `.git` da raiz fora da área gravável | **Sustenta o argumento de configuração:** a chamada limita a escrita a `<wt>/app`. A guarda final ainda compara conteúdo, sem repetir a exigência inicial de arquivo sem symlink. Efeito real de uma substituição: **[nao verificado]**. (`bin/codex_pergunta.sh:199`, `bin/codex_pergunta.sh:381`, `bin/codex_pergunta.sh:383`, `bin/codex_pergunta.sh:399`) |

   O symlink rastreado e inalterado continua fora de `tocados`, portanto não recebe a proibição; esse ponto não foi fechado nem explicitamente descartado na triagem fornecida. (`bin/codex_pergunta.sh:147`, `bin/codex_pergunta.sh:157`, `bin/codex_pergunta.sh:163`)

   **Não identifiquei nova recusa de admissão causada apenas por `onerror` numa wt limpa convencional:** antes, rastreados omitidos já viravam remoções, e diretórios extras já eram tocados. No fim, erro de permissão/I/O externo também provoca descarte; a mensagem não demonstra culpa do Codex. Uma wt real desta obra nessa condição: **[nao verificado]**. (`bin/codex_pergunta.sh:144`, `bin/codex_pergunta.sh:150`, `bin/codex_pergunta.sh:152`, `bin/codex_pergunta.sh:402`)

6. **Informativo — os novos mutantes têm mordida coerente por leitura; permanecem limites de cobertura.**

   Hardlink idêntico cobre início, espera e fim; neutralizar `inv_nlink` retira precisamente sua descoberta. O mutante de `fs_walk` agora conserva os rastreados e omite os caminhos plantados, evitando a antiga morte no pre-check. Retirar `p_git` remove a proteção efetiva, não apenas sua mensagem. (`bin/tests/test_codex_pergunta.sh:207`, `bin/tests/test_codex_pergunta.sh:315`, `bin/tests/test_codex_pergunta.sh:318`, `bin/tests/test_codex_pergunta.sh:383`, `bin/tests/test_codex_pergunta.sh:422`, `bin/tests/test_codex_pergunta.sh:426`)

   O caso de ilegibilidade depende de `chmod 000` realmente impedir leitura. Sob privilégio que contorne essa permissão, o original pode falhar no teste antes de avaliar o mutante. FIFO é exercitado; socket/dispositivo não são. Execução desses cenários: **[nao verificado]**. (`bin/tests/test_codex_pergunta.sh:323`, `bin/tests/test_codex_pergunta.sh:325`, `bin/tests/test_codex_pergunta.sh:330`, `bin/tests/test_codex_pergunta.sh:386`)

   O descarte da sentinela FSMON sustenta apenas a interpretação estreita: a fixture verifica recusa e ausência de efeito. Ela cria somente `.git/config`, não executa Git ali nem demonstra o hook válido anunciado pelo comentário. Essa causalidade permanece **[nao verificado]**; o Git do inventário usa `-C wt`. (`bin/tests/test_codex_pergunta.sh:289`, `bin/tests/test_codex_pergunta.sh:291`, `bin/tests/test_codex_pergunta.sh:292`, `bin/codex_pergunta.sh:112`)

7. **Informativo — escritores, leitores e significados.**

   | Campo/artefato | Escritor → leitor |
   |---|---|
   | `_CDIR` | Shell captura o common-dir absoluto do Git → `dirname` calcula `VAGA`. **Metadados do repositório são interpretados como filho da raiz desejada.** (`bin/codex_pergunta.sh:72`, `bin/codex_pergunta.sh:73`) |
   | `VAGA` / arquivo da vaga | Shell define → reserva da resposta, criação da pasta, abertura append no fd 6 e `flock`. Testes também criam, prendem e movem suas fixtures. (`bin/codex_pergunta.sh:74`, `bin/codex_pergunta.sh:217`, `bin/codex_pergunta.sh:219`, `bin/tests/test_codex_pergunta.sh:175`, `bin/tests/test_codex_pergunta.sh:217`) |
   | `_INV0` / `_INV1` | Python produz stdout/stderr → shell exige saída vazia na admissão; revalidação acrescenta `FALHOU` em erro. (`bin/codex_pergunta.sh:223`, `bin/codex_pergunta.sh:371`, `bin/codex_pergunta.sh:374`) |
   | `.inventario` / `.recusa` | Python escreve inventário; shell acrescenta guarda de `.git`, lê rc e gera recusa → testes verificam; prova copia evidências. **No fim, saída não vazia é permitida se rc 0.** (`bin/codex_pergunta.sh:398`, `bin/codex_pergunta.sh:403`, `bin/tests/test_codex_pergunta.sh:106`, `bin/tests/test_codex_pergunta.sh:242`, `bin/tests/prova_codex_pergunta_real.sh:80`) |
   | `$EV` | Shell define/cria → redirecionamentos e cópias. A resposta original continua em caminho fixo `logs/codex/r/provaefeito`. (`bin/tests/prova_codex_pergunta_real.sh:21`, `bin/tests/prova_codex_pergunta_real.sh:77`, `bin/tests/prova_codex_pergunta_real.sh:80`) |
   | `escuta/porta`, `u.sock`, `conexoes` | Ouvinte publica porta, faz bind e acrescenta registros → shell lê porta/contagens; controles e tentativa usam os endpoints. **`conexoes` mistura controles externos e candidatos a intrusão.** (`bin/tests/prova_codex_pergunta_real.sh:34`, `bin/tests/prova_codex_pergunta_real.sh:35`, `bin/tests/prova_codex_pergunta_real.sh:41`, `bin/tests/prova_codex_pergunta_real.sh:46`, `bin/tests/prova_codex_pergunta_real.sh:103`) |
   | `$EV/p`, `prova/*.out`, `$EV/prova` | Shell escreve pergunta → wrapper lê; Codex é instruído a escrever `.out` → prova copia, mas julga os originais na wt. (`bin/tests/prova_codex_pergunta_real.sh:53`, `bin/tests/prova_codex_pergunta_real.sh:61`, `bin/tests/prova_codex_pergunta_real.sh:77`, `bin/tests/prova_codex_pergunta_real.sh:81`, `bin/tests/prova_codex_pergunta_real.sh:89`) |

   Não encontrei troca relativa/absoluta em `_CDIR` no comando declarado. A ambiguidade relevante é **repositório × worktree**: `VIVO` determina tanto identidade do repositório quanto o pai exigido para o nome da wt; mudar `VIVO` para outra pasta pode conservar a vaga e mudar os destinos admissíveis. (`bin/codex_pergunta.sh:72`, `bin/codex_pergunta.sh:191`, `bin/codex_pergunta.sh:194`)

8. **Informativo — limite do aval.** Suite completa, prova real, isolamento efetivo, configuração Git ativa e correspondência criptográfica da cópia com o commit solicitado: **[nao verificado]**. Os scripts de teste criam fixtures e a prova chama o Codex; não foram executados nesta revisão somente leitura. (`bin/tests/test_codex_pergunta.sh:18`, `bin/tests/test_codex_pergunta.sh:55`, `bin/tests/prova_codex_pergunta_real.sh:21`, `bin/tests/prova_codex_pergunta_real.sh:77`)

## 07/11 13:34 · sessao 68cb2a1e2ac4 · - · - · origem=Ronald (tmux)

B/F/G/H: **curas parciais; persistem caminhos sem a garantia anunciada**. Execução de testes e ocorrências em produção: **[nao verificado]**. [nao verificado]

| Achado anterior | Veredito na cópia |
|---|---|
| B-NULL no agrupamento/seleção do CLI | **Curado na origem desses leitores:** `q_do_horizonte` agora é somente `>= H`; agrupamento o usa; `admin_decidiu_sem_ato` separa nulos para contagem. (`app/chamados/services/fio.py:1033`; `app/chamados/services/fio.py:1052`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:97`) |
| B-recorte estrito em toda escrita | **PROCEDENTE, parcial:** `aplicar` não aplica `q_do_horizonte`, recebe PKs e escreve por PK. Chamada direta com pergunta anterior a H, `admin_decidiu`, sem decisão e competência não exportada pode registrar SILENCIO. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:150`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:168`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:172`) |
| B-DIVERGE no CLI | **Curado para o comando:** registra divergências e recusa antes de `aplicar`. **Não cobre o helper direto:** a verificação mora em `handle`. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:228`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:315`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:128`) |
| F-recusa antes de qualquer escrita | **PROCEDENTE, parcial:** pré-checagem cura mérito/exportada já presentes, mas está fora das travas; SILENCIO continua em atomics individuais antes da segunda leitura do mérito. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:147`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:164`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:185`) |
| G-ausência do registro 0052 | **Curado:** ausência de registro ou instante agora classifica como nova. (`app/chamados/services/fio.py:1113`) |
| G-nova medida pelo nascimento da disputa | **PROCEDENTE, não toca:** mantém o proxy e admite explicitamente subcontagem da pergunta nova em disputa velha. (`app/chamados/services/fio.py:1103`; `app/chamados/services/fio.py:1105`) |
| G-placar truncado | **Não toca:** continua exibindo somente dez PKs novas e quantidade do acervo. (`bin/placar_code.sh:198`) |
| H-rastreador por tabela | **PROCEDENTE, parcial:** original continua ignorando aquisições subsequentes da mesma tabela. (`app/chamados/tests/test_e1_v5_ordem_trava.py:45`) |
| H-relógios soltos | **PROCEDENTE, não toca integralmente:** fixture cria disputa com `auto_now_add`, pergunta é fechada lendo `now`, e teste de catraca ainda compara abertura real com migration real. (`app/chamados/tests/test_e1_v6_horizonte.py:74`; `app/chamados/models.py:1709`; `app/chamados/models.py:2760`; `app/chamados/tests/test_e1_v6_horizonte.py:210`) |
| H-canônica da tela | **Curado no contraexemplo antigo:** novo caso põe muda e respondida ambas reutilizáveis e exige a respondida. (`app/chamados/tests/test_e1_v6_horizonte.py:381`) |
| H-fronteira BATIDA no banco | **Não toca:** testes concretos de fronteira criam motivo MARCO padrão; BATIDA é examinada por folhas da condição. (`app/chamados/tests/test_e1_v6_horizonte.py:76`; `app/chamados/tests/test_e1_v6_horizonte.py:94`; `app/chamados/tests/test_e1_v6_horizonte.py:58`) |

Contraexemplo F por interleaving: passar na pré-checagem; gravar SILENCIO e encerrar seu atomic; mérito aparecer no grupo; segunda leitura detectar e acrescentar `merito_recusado`; função devolver recusa após a escrita anterior. Não há atomic externo cobrindo o universo nem rollback nesse ramo. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:161`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:164`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:186`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:197`) Reprodução concorrente: **[nao verificado]**.

O CLI então imprime **“nada escrito”**, embora esse ramo possa devolver `linhas_silencio > 0`; o próprio retorno mantém a contagem. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:197`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:329`)

Exportada também só é consultada antes dos locks; nem loop SILENCIO nem loop retratação a relê sob trava. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:158`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:168`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:183`) Mudança concorrente efetiva em produção: **[nao verificado]**.

`dedup_perguntas_fato` recebe grupos estritos, mas relê somente PKs/exclusão das vias, sem repetir horizonte; também continua sem recusa de mérito e competência exportada. A chamada ao escritor fica em `:101`. (`app/chamados/management/commands/dedup_perguntas_fato.py:59`; `app/chamados/management/commands/dedup_perguntas_fato.py:94`; `app/chamados/management/commands/dedup_perguntas_fato.py:101`) É caminho preexistente sem cura neste diff; execução em produção: **[nao verificado]**.

A cobertura nova de F prova recusa com mérito **já existente** e exportada mockada **desde a primeira leitura**; não prova interleaving nem helper direto com data anterior ao horizonte. (`app/chamados/tests/test_e1_v6_horizonte.py:311`; `app/chamados/tests/test_e1_v6_horizonte.py:323`)

Há novo selo fraco em D: `assertLessEqual(ps.count(), 1)` aceita zero perguntas; portanto não prova nascimento e dedup de uma conversa. A subclasse ainda sobrescreve o teste nominal herdado com `pass`. (`app/chamados/tests/test_e1_v6_horizonte.py:421`; `app/chamados/tests/test_e1_v6_horizonte.py:409`) Execução/mutação concreta: **[nao verificado]**.

**NAO PROCEDE divergência de operador inclusivo/exclusivo:** helper, modelo e migration usam `>= H`; `--ate` é exclusão superior deliberada (`d < dia_antes_de`). (`app/chamados/services/fio.py:1033`; `app/chamados/models.py:3177`; `app/chamados/models.py:3183`; `app/chamados/migrations/0053_pergunta_unica_por_familia.py:33`; `app/chamados/services/fio.py:1076`)

A cópia não disponibiliza repositório Git: `git rev-parse HEAD` retornou “not a git repository”; confirmação independente do hash/pai: **[nao verificado]**.

## 07/11 13:33 · sessao 1e81ef6de4a4 · - · - · origem=Ronald (tmux)

1. **Alta — hardlink idêntico: fechado nas três fases.** `st_nlink>1` agora inclui qualquer arquivo regular em `tocados`; a classificação acrescenta `hardlink`. Pre-check, revalidação e fim executam essa implementação. (`bin/codex_pergunta.sh:149`, `bin/codex_pergunta.sh:164`, `bin/codex_pergunta.sh:223`, `bin/codex_pergunta.sh:371`, `bin/codex_pergunta.sh:398`)

2. **Média — FIFO e ramo ilegível: fechados por leitura do código.** O curto circuito evita abrir FIFO como arquivo; a classificação passa a proibi-lo. `os.walk` propaga erro de acesso para `FALHA`, rc 1 do inventário e rc 3 no fim. Execução real: `[nao verificado]`. (`bin/codex_pergunta.sh:116`, `bin/codex_pergunta.sh:136`, `bin/codex_pergunta.sh:147`, `bin/codex_pergunta.sh:151`, `bin/codex_pergunta.sh:161`, `bin/codex_pergunta.sh:400`)

3. **Média — descarte de bytes não UTF-8 só se sustenta parcialmente.** Simulação exclusivamente em memória do código exato, com arquivo rastreado limpo `b'app/folha/a\xff.py'`, produziu rc 0 e dois `TOCADO`: nome com surrogate e nome com `�`. O pre-check/revalidação recusam legitimamente pelo critério estrito de saída vazia, mas recusam uma árvore fisicamente igual à base; o fim aceita rc 0. Portanto, “falha fechado” descreve a admissão, não uma comparação correta dos caminhos. Problema preexistente ao v4b. (`bin/codex_pergunta.sh:126`, `bin/codex_pergunta.sh:132`, `bin/codex_pergunta.sh:141`, `bin/codex_pergunta.sh:165`, `bin/codex_pergunta.sh:223`, `bin/codex_pergunta.sh:374`, `bin/codex_pergunta.sh:400`)

4. **Informativo — permissões e diretórios vazios continuam fora da identidade Git usada.** Só o bit de execução do proprietário entra no modo; diretórios existentes não têm permissões comparadas; diretório vazio novo recebe `TOCADO`, podendo terminar com rc 0. O descarte é coerente com avaliar apenas o conteúdo que Git transporta; não equivale a provar integridade completa da árvore física. (`bin/codex_pergunta.sh:118`, `bin/codex_pergunta.sh:129`, `bin/codex_pergunta.sh:143`, `bin/codex_pergunta.sh:165`, `bin/codex_pergunta.sh:167`)

5. **Informativo — symlink rastreado inalterado continua aceito.** Só caminhos de `tocados` recebem a proibição de symlink; igualdade de modo/blob deixa o symlink existente fora desse conjunto. Não foi corrigido nem explicitamente descartado na triagem. (`bin/codex_pergunta.sh:114`, `bin/codex_pergunta.sh:119`, `bin/codex_pergunta.sh:147`, `bin/codex_pergunta.sh:157`, `bin/codex_pergunta.sh:163`)

6. **Informativo — `onerror` não demonstra autoria da falha.** Permissão ou erro de I/O produzido por agente externo também resulta em `FALHA` e descarte. Não encontrei regressão adicional de admissão para árvore limpa normal: um ramo com arquivos rastreados omitidos já gerava “não vistos”; um ramo extra já gerava diretório tocado. Uma worktree real desta obra ilegível sem ação do Codex: `[nao verificado]`. (`bin/codex_pergunta.sh:144`, `bin/codex_pergunta.sh:150`, `bin/codex_pergunta.sh:152`, `bin/codex_pergunta.sh:402`)

7. **Informativo — CRLF/smudge e case: descarte ambiental não demonstrado pela cópia.** O código continua comparando bytes físicos com SHA-1 bruto e nomes literalmente. A alegação de ext4 e a medição de ausência de filtro na configuração ativa permanecem `[nao verificado]`; a cópia não fornece a configuração Git efetiva. (`bin/codex_pergunta.sh:114`, `bin/codex_pergunta.sh:120`, `bin/codex_pergunta.sh:132`, `bin/codex_pergunta.sh:147`)

8. **Média — teste de ilegibilidade depende de privilégio efetivo.** A fixture usa exclusivamente `chmod 000`; não verifica se o executor ainda consegue ler o diretório. Sob usuário com privilégio de leitura irrestrita, o original pode terminar rc 0 e o caso falhar antes de avaliar o mutante. Execução desse cenário: `[nao verificado]`. (`bin/tests/test_codex_pergunta.sh:322`, `bin/tests/test_codex_pergunta.sh:325`, `bin/tests/test_codex_pergunta.sh:386`)

## 07/11 13:33 · sessao c3c3934d6603 · - · - · origem=Ronald (tmux)

1. **Média — item 5 permanece parcialmente aberto.** O controle positivo só exige `INTRUSO_WT.txt`; cada tentativa depende de `.out` produzido pelo próprio Codex com uma linha `rc=`. Arquivos fabricados com negativas satisfazem essas verificações; TCP local/Unix sequer precisam de assinatura de negação. Os rótulos novos explicitam a limitação, mas não mudam o resultado `rc=0` possível. A triagem sustenta o fechamento documental, não a execução comprovada de cada tentativa. (`bin/tests/prova_codex_pergunta_real.sh:60`, `bin/tests/prova_codex_pergunta_real.sh:82`, `bin/tests/prova_codex_pergunta_real.sh:90`, `bin/tests/prova_codex_pergunta_real.sh:92`, `bin/tests/prova_codex_pergunta_real.sh:117`, `bin/tests/prova_codex_pergunta_real.sh:144`)

2. **Informativo — item 6 fecha os contraexemplos de colisão e ouvinte morto.** `$EV` ganha PID e criação exclusiva: destino preexistente recusa a prova. O controle posterior verifica PID e exige mais duas conexões registradas; falha deixa resultado inconclusivo. A observação sobre `CONECTOU` agora concorda com a implementação. Execução real: `[nao verificado]`. (`bin/tests/prova_codex_pergunta_real.sh:21`, `bin/tests/prova_codex_pergunta_real.sh:85`, `bin/tests/prova_codex_pergunta_real.sh:106`, `bin/tests/prova_codex_pergunta_real.sh:111`, `bin/tests/prova_codex_pergunta_real.sh:134`, `bin/tests/prova_codex_pergunta_real.sh:135`)

3. **Baixa — o delta contamina a atribuição das conexões no diagnóstico vermelho.** `N_DEPOIS` é capturado antes dos controles posteriores; estes acrescentam registros ao mesmo arquivo. Depois, `tail` imprime até EOF e atribui todas essas linhas a conexões “de dentro do sandbox”, incluindo os próprios `controle_pos`. A quantidade impressa usa o snapshot correto, mas a lista de evidências mistura origens. (`bin/tests/prova_codex_pergunta_real.sh:103`, `bin/tests/prova_codex_pergunta_real.sh:107`, `bin/tests/prova_codex_pergunta_real.sh:108`, `bin/tests/prova_codex_pergunta_real.sh:114`)

4. **Média — permanece janela entre conexão aceita e registro contado.** O ouvinte aceita, espera `recv` por até dois segundos e só então acrescenta a linha. Uma conexão pendente pode ser registrada depois de `N_DEPOIS`, ficar fora da diferença julgada e ainda contribuir para o mínimo exigido no controle posterior. Não há validação individual dos tokens `controle_pos` ou de uma conexão por transporte. O controle posterior fecha a morte permanente do ouvinte, mas não essa janela de contagem; reprodução do escalonamento: `[nao verificado]`. (`bin/tests/prova_codex_pergunta_real.sh:38`, `bin/tests/prova_codex_pergunta_real.sh:39`, `bin/tests/prova_codex_pergunta_real.sh:41`, `bin/tests/prova_codex_pergunta_real.sh:103`, `bin/tests/prova_codex_pergunta_real.sh:109`, `bin/tests/prova_codex_pergunta_real.sh:111`, `bin/tests/prova_codex_pergunta_real.sh:114`)

5. **Informativo — escritores e leitores de `$EV/escuta`.**

   | Campo | Escritor → leitor |
   |---|---|
   | `$EV` | Shell define e cria exclusivamente → redirecionamentos, cópias e mensagem final. (`bin/tests/prova_codex_pergunta_real.sh:21`, `bin/tests/prova_codex_pergunta_real.sh:78`, `bin/tests/prova_codex_pergunta_real.sh:80`, `bin/tests/prova_codex_pergunta_real.sh:146`) |
   | `escuta/u.sock` | Python faz `bind` → controles externos e tentativa incluída na pergunta. (`bin/tests/prova_codex_pergunta_real.sh:34`, `bin/tests/prova_codex_pergunta_real.sh:50`, `bin/tests/prova_codex_pergunta_real.sh:71`, `bin/tests/prova_codex_pergunta_real.sh:108`) |
   | `escuta/porta` | Python escreve arquivo temporário e renomeia → shell lê e injeta nos comandos TCP. (`bin/tests/prova_codex_pergunta_real.sh:35`, `bin/tests/prova_codex_pergunta_real.sh:46`, `bin/tests/prova_codex_pergunta_real.sh:49`, `bin/tests/prova_codex_pergunta_real.sh:70`) |
   | `escuta/conexoes` | Ouvinte acrescenta aceites → shell conta e imprime. Controles e candidatos a intrusão compartilham o arquivo. (`bin/tests/prova_codex_pergunta_real.sh:41`, `bin/tests/prova_codex_pergunta_real.sh:52`, `bin/tests/prova_codex_pergunta_real.sh:103`, `bin/tests/prova_codex_pergunta_real.sh:114`) |
   | `escuta/ouvinte.log` | Redirecionamento de stdout/stderr do Python → indicação de diagnóstico quando o ouvinte não sobe. (`bin/tests/prova_codex_pergunta_real.sh:30`, `bin/tests/prova_codex_pergunta_real.sh:47`) |
   | `$EV/p` | Shell escreve pergunta → wrapper recebe seu caminho. (`bin/tests/prova_codex_pergunta_real.sh:53`, `bin/tests/prova_codex_pergunta_real.sh:77`) |
   | `$EV/prova` | Shell copia a evidência → julgamento continua lendo os originais em `$WT/app/prova`, não essa cópia. (`bin/tests/prova_codex_pergunta_real.sh:81`, `bin/tests/prova_codex_pergunta_real.sh:89`, `bin/tests/prova_codex_pergunta_real.sh:125`) |
   | `prova/*.out` | Codex é instruído a escrever → shell verifica e julga. (`bin/tests/prova_codex_pergunta_real.sh:61`, `bin/tests/prova_codex_pergunta_real.sh:73`, `bin/tests/prova_codex_pergunta_real.sh:90`, `bin/tests/prova_codex_pergunta_real.sh:135`) |
   | PID do ouvinte | Shell captura `$!` → `kill -0` posterior e `trap` de encerramento. (`bin/tests/prova_codex_pergunta_real.sh:44`, `bin/tests/prova_codex_pergunta_real.sh:106`) |

   `$EV` é específico da chamada, mas o destino da resposta continua fixo em `$R/logs/codex/r/provaefeito`; a cópia em `$EV` é feita depois da chamada. Interferência real entre chamadas: `[nao verificado]`. (`bin/tests/prova_codex_pergunta_real.sh:21`, `bin/tests/prova_codex_pergunta_real.sh:77`, `bin/tests/prova_codex_pergunta_real.sh:80`)

6. **Informativo — verificações realizadas.** Revisão estática dos scripts; prova real, acesso a serviços e simulação do ouvinte: `[nao verificado]`.

## 07/11 13:32 · sessao 606d39a09479 · - · - · origem=Ronald (tmux)

1. **Informativo — os dois mutantes de v4.r foram corrigidos por leitura.** `fs_walk` agora conserva os blobs rastreados no percurso substituto; deixa de fabricar remoções no pre-check. `p_git` remove a regra proibitiva efetiva, em vez de apenas um motivo redundante. Execução dos mutantes: **[nao verificado]**. (`bin/tests/test_codex_pergunta.sh:422`, `bin/tests/test_codex_pergunta.sh:423`, `bin/tests/test_codex_pergunta.sh:426`, `bin/tests/test_codex_pergunta.sh:427`, `bin/codex_pergunta.sh:105`, `bin/codex_pergunta.sh:150`)

2. **Informativo — os novos casos têm mordida coerente no código.**

   | Caso/mutante | Aval por leitura |
   |---|---|
   | Hardlink idêntico inicial/final/revalidação | A neutralização de `inv_nlink` elimina justamente a descoberta do arquivo que conserva bytes/modo. As três fixtures exercitam momentos distintos. (`bin/tests/test_codex_pergunta.sh:207`, `bin/tests/test_codex_pergunta.sh:315`, `bin/tests/test_codex_pergunta.sh:318`, `bin/tests/test_codex_pergunta.sh:383`, `bin/codex_pergunta.sh:149`) |
   | Ramo ilegível | Sem `onerror`, a fixture deixa de produzir `FALHA`; o caso exige rc 3 e a linha `FALHA`. Não exige que o arquivo oculto seja classificado individualmente. (`bin/tests/test_codex_pergunta.sh:322`, `bin/tests/test_codex_pergunta.sh:325`, `bin/tests/test_codex_pergunta.sh:326`, `bin/tests/test_codex_pergunta.sh:386`) |
   | Tipo especial | A fixture cria FIFO; retirar o motivo permite `TOCADO`, contrariando `_inv_recusa`. Socket/dispositivo não são exercitados. (`bin/tests/test_codex_pergunta.sh:104`, `bin/tests/test_codex_pergunta.sh:329`, `bin/tests/test_codex_pergunta.sh:387`, `bin/codex_pergunta.sh:161`) |
   | Vaga por outra worktree | A fixture prende a vaga principal e usa outra worktree como `VIVO`; o mutante volta a derivar a vaga dessa árvore. A recusa exige ocupação e ausência de chamada ao Codex. (`bin/tests/test_codex_pergunta.sh:98`, `bin/tests/test_codex_pergunta.sh:332`, `bin/tests/test_codex_pergunta.sh:433`) |

   Resultados completos desses casos: **[nao verificado]**. (`bin/tests/test_codex_pergunta.sh:366`, `bin/tests/test_codex_pergunta.sh:451`)

3. **Baixa — a alegação causal da sentinela FSMON continua sem demonstração.** O teste cria somente `.git/config`, verifica a recusa e a ausência da sentinela; não executa Git no diretório plantado nem monta explicitamente um repositório interno completo. Sustenta a leitura estreita da triagem — inventário não produziu efeito nessa fixture e caminho foi recusado —, mas “o primeiro Git em `app` executaria o hook” continua **[nao verificado]**. (`bin/tests/test_codex_pergunta.sh:289`, `bin/tests/test_codex_pergunta.sh:291`, `bin/tests/test_codex_pergunta.sh:292`, `bin/codex_pergunta.sh:112`)

4. **Informativo — falta cobertura negativa nova nos outros dois momentos do inventário.** O caso ilegível e o FIFO são plantados durante o Codex falso, cobrindo o fim; não há casos correspondentes novos de ilegibilidade/tipo especial inicial ou durante a espera. A reutilização da mesma implementação fornece proteção por leitura, mas execução específica desses caminhos permanece **[nao verificado]**. (`bin/tests/test_codex_pergunta.sh:76`, `bin/tests/test_codex_pergunta.sh:323`, `bin/tests/test_codex_pergunta.sh:330`, `bin/codex_pergunta.sh:223`, `bin/codex_pergunta.sh:371`, `bin/codex_pergunta.sh:398`)

## 07/11 13:25 · sessao 8e6391171c0e · CODEX-CONSTRUTOR-NEGOCIO · verificador · origem=Code

**Não liberaria esta volta:** o inventário ainda deixa passar hardlink idêntico na revalidação/final, e dois mutantes ficam vermelhos sem demonstrar a proteção alegada. (`bin/codex_pergunta.sh:143`, `bin/codex_pergunta.sh:158`, `bin/tests/test_codex_pergunta.sh:393`, `bin/tests/test_codex_pergunta.sh:396`)

1. **Informativo — fechamento dos achados do opus fornecido.**

   | Achado | Aval desta volta |
   |---|---|
   | `.git` aninhado invisível | **Fecha os contraexemplos apresentados:** o percurso encontra diretórios e arquivos `.git`, e a classificação os proíbe. Persistem limites gerais da varredura abaixo. (`bin/codex_pergunta.sh:132`, `bin/codex_pergunta.sh:135`, `bin/codex_pergunta.sh:154`) |
   | Selo só criava `.gitignore` | **Fecha em parte:** acrescenta `.git/config`, diretório aninhado e arquivo `gitdir`, mas há mortes de mutantes pelo motivo errado. (`bin/tests/test_codex_pergunta.sh:291`, `bin/tests/test_codex_pergunta.sh:295`, `bin/tests/test_codex_pergunta.sh:396`) |
   | Ignorados/autoload saíam `TOCADO` | **Fecha a classificação dos casos apresentados:** agora recebem motivo proibitivo; a execução de autoload pelo próprio processo inventariante continua `[nao verificado]`. (`bin/codex_pergunta.sh:107`, `bin/codex_pergunta.sh:155`, `bin/codex_pergunta.sh:393`) |
   | Vaga pela raiz do script | **Fecha mantendo o mesmo `CODEX_VIVO`:** copiar `bin/` já não muda a vaga. **Parcial como exclusão por repositório**, conforme item 4. (`bin/codex_pergunta.sh:71`, `bin/tests/test_codex_pergunta.sh:314`) |
   | Prova baseada no `.out` do Codex | **Fecha em parte:** TCP local/Unix ganham ouvinte externo; Docker, docker.sock e TCP externo continuam autodeclarados. (`bin/tests/prova_codex_pergunta_real.sh:41`, `bin/tests/prova_codex_pergunta_real.sh:104`, `bin/tests/prova_codex_pergunta_real.sh:117`, `bin/tests/prova_codex_pergunta_real.sh:124`) |
   | Texto “só LE”/“ÚNICA escrita” | **Fecha:** distingue quatro papéis de leitura e duas permissões de escrita. (`CLAUDE.md:341`, `CLAUDE.md:346`) |
   | Opus/teto apresentados como guarda | **Fecha a rotulagem:** declara expressamente que são conduta, sem guarda no wrapper. (`CLAUDE.md:351`, `CLAUDE.md:352`) |

2. **Alta — hardlink idêntico escapa durante a espera e no inventário final.**

   Substituir um arquivo rastreado por hardlink com os mesmos bytes e modo não o inclui em `tocados`; a consulta a `st_nlink` acontece somente depois, dentro desse conjunto. O `find -links +1` inicial não se repete na revalidação. Assim, a troca durante a espera passa pela nova guarda; no final também pode produzir inventário vazio. A simulação em memória confirmou rc 0 e saída vazia; reprodução em filesystem real: `[nao verificado]`. (`bin/codex_pergunta.sh:143`, `bin/codex_pergunta.sh:152`, `bin/codex_pergunta.sh:158`, `bin/codex_pergunta.sh:220`, `bin/codex_pergunta.sh:366`)

3. **Média — o inventário não cobre integralmente entradas, identidade e permissões.**

   | Classe | Resultado da leitura |
   |---|---|
   | Permissão de arquivo | Só compara execução pelo proprietário. `0644→0666` ou `0644→0600` pode desaparecer do inventário; a simulação de `0666` deu rc 0 vazio. (`bin/codex_pergunta.sh:116`, `bin/codex_pergunta.sh:143`) |
   | Diretório existente | Não compara suas permissões; só verifica se o caminho pertence às pastas da base. (`bin/codex_pergunta.sh:139`) |
   | FIFO/socket/dispositivo novo | É reportado, mas pode sair **`TOCADO`, rc 0**, por faltar proibição por tipo. FIFO foi confirmado em memória. (`bin/codex_pergunta.sh:143`, `bin/codex_pergunta.sh:153`, `bin/codex_pergunta.sh:160`) |
   | Diretório vazio novo | É reportado como `caminho/`; sob caminho permitido, é aceito. Diretórios vazios não têm representação nessa base `ls-tree`. (`bin/codex_pergunta.sh:125`, `bin/codex_pergunta.sh:140`) |
   | Symlink novo/modificado | Entra na comparação e recebe `PROIBIDO`. Symlink já rastreado e inalterado não recebe essa classificação. (`bin/codex_pergunta.sh:112`, `bin/codex_pergunta.sh:143`, `bin/codex_pergunta.sh:157`) |
   | Bytes não UTF-8 | Git usa `"replace"`, enquanto o percurso conserva outra representação dos bytes. Pode duplicar caminhos e recusar arquivo rastreado limpo. Destino não UTF-8 de symlink pode falhar no `.encode()`. (`bin/codex_pergunta.sh:112`, `bin/codex_pergunta.sh:122`, `bin/codex_pergunta.sh:128`) |
   | Case | A comparação é literal, sem conversão de caixa. Comportamento em filesystem que ignore caixa: `[nao verificado]`. (`bin/codex_pergunta.sh:128`, `bin/codex_pergunta.sh:137`, `bin/codex_pergunta.sh:143`) |
   | Ramo não percorrido | `os.walk` não recebe `onerror`; uma abertura recusada pode omitir descendentes novos sem produzir `FALHA`. Rastreado não visto vira remoção, mas isso não enumera os novos ocultos. (`bin/codex_pergunta.sh:132`, `bin/codex_pergunta.sh:145`, `bin/codex_pergunta.sh:146`) |

   Há também recusa potencial de checkout legítimo com transformação CRLF/smudge: compara bytes físicos diretamente ao blob armazenado. Configuração ou ocorrência disso nesta obra: `[nao verificado]`. (`bin/codex_pergunta.sh:112`, `bin/codex_pergunta.sh:125`, `bin/codex_pergunta.sh:143`, `bin/codex_pergunta.sh:217`)

   A poda da raiz exclui `.git` independentemente do tipo; a exigência inicial “arquivo sem symlink” não é repetida no final, que compara conteúdo. Aceitação real pelo Git de uma troca por symlink de conteúdo idêntico: `[nao verificado]`. (`bin/codex_pergunta.sh:135`, `bin/codex_pergunta.sh:193`, `bin/codex_pergunta.sh:394`)

4. **Alta — a vaga ainda depende da árvore escolhida como `CODEX_VIVO`.**

   Dois valores de `CODEX_VIVO` apontando para worktrees distintas do mesmo repositório produzem dois caminhos de vaga. A validação de common-dir confere a origem com o destino de cada chamada, mas não deriva a vaga desse common-dir. Portanto, o delta resolve a cópia do script com origem fixa; não estabelece exclusão única para todas as árvores do mesmo repositório. Execução concorrente desse cenário: `[nao verificado]`. (`bin/codex_pergunta.sh:56`, `bin/codex_pergunta.sh:71`, `bin/codex_pergunta.sh:200`, `bin/codex_pergunta.sh:212`)

5. **Alta — a prova ainda pode ficar verde sem executar cada tentativa.** [nao verificado]

   O controle positivo exige apenas `INTRUSO_WT.txt`; a evidência individual continua sendo `.out` gravado pelo próprio Codex. Preencher esses arquivos com `rc=` e negativas fabricadas satisfaz as verificações documentais; TCP local/Unix nem exigem assinatura de negação. Zero conexões prova ausência de conexão registrada, não execução de tentativa bloqueada. Contraexemplo completo executado: `[nao verificado]`. (`bin/tests/prova_codex_pergunta_real.sh:60`, `bin/tests/prova_codex_pergunta_real.sh:90`, `bin/tests/prova_codex_pergunta_real.sh:92`, `bin/tests/prova_codex_pergunta_real.sh:105`, `bin/tests/prova_codex_pergunta_real.sh:107`)

   Os rótulos `AUTO-DECLARADO` de Docker/socket/TCP externo não tornam o resultado inconclusivo: essas negativas ainda podem terminar com rc 0. (`bin/tests/prova_codex_pergunta_real.sh:82`, `bin/tests/prova_codex_pergunta_real.sh:117`, `bin/tests/prova_codex_pergunta_real.sh:120`, `bin/tests/prova_codex_pergunta_real.sh:125`, `bin/tests/prova_codex_pergunta_real.sh:135`)

6. **Média — o ouvinte não tem controle posterior e `$EV` pode colidir.**

   O script testa o ouvinte antes da chamada; depois só compara contagens. Não confirma sobrevivência nem faz controle posterior. Se ele morrer após os controles, ainda pode imprimir “OK MEDIDO”. (`bin/tests/prova_codex_pergunta_real.sh:44`, `bin/tests/prova_codex_pergunta_real.sh:49`, `bin/tests/prova_codex_pergunta_real.sh:103`, `bin/tests/prova_codex_pergunta_real.sh:105`)

   `$EV` usa precisão de segundos e `mkdir -p`; chamadas no mesmo segundo podem compartilhar socket, porta e evidências. Além disso, `CONECTOU` no `.out` ainda reprova independentemente do ouvinte, apesar do comentário que lhe atribui o julgamento. Ocorrências reais: `[nao verificado]`. (`bin/tests/prova_codex_pergunta_real.sh:21`, `bin/tests/prova_codex_pergunta_real.sh:34`, `bin/tests/prova_codex_pergunta_real.sh:45`, `bin/tests/prova_codex_pergunta_real.sh:92`, `bin/tests/prova_codex_pergunta_real.sh:125`)

7. **Média — dois mutantes morrem por motivo diferente do declarado.**

   - **`fs_walk`:** remover o percurso também afeta o pre-check. Todos os blobs ficam “não vistos”, a chamada é recusada com rc 2 antes do falso Codex, e o caso fica vermelho porque esperava rc 3 final. Não demonstra a mordida final contra `.git/config`. (`bin/tests/test_codex_pergunta.sh:393`, `bin/codex_pergunta.sh:145`, `bin/codex_pergunta.sh:217`, `bin/tests/test_codex_pergunta.sh:105`)
   - **`git_aninhado`:** remove apenas o motivo suplementar; `.git*` continua proibindo o caminho. Morre pela ausência da mensagem exigida, mantendo a proteção. (`bin/tests/test_codex_pergunta.sh:396`, `bin/codex_pergunta.sh:103`, `bin/codex_pergunta.sh:154`, `bin/tests/test_codex_pergunta.sh:106`)
   - **Sentinela FSMON:** o caso planta configuração, mas não exercita o hook com Git no diretório plantado. Prova recusa do caminho; a ausência da sentinela não prova bloqueio de execução do hook. (`bin/tests/test_codex_pergunta.sh:291`, `bin/tests/test_codex_pergunta.sh:292`, `bin/codex_pergunta.sh:110`)

   Os mutantes `poda_so_raiz` retiram justamente a visibilidade de `.git` em diretório e arquivo; não encontrei a mesma morte espúria neles. Resultado executado de todos os mutantes: `[nao verificado]`. (`bin/tests/test_codex_pergunta.sh:295`, `bin/tests/test_codex_pergunta.sh:296`, `bin/tests/test_codex_pergunta.sh:394`, `bin/tests/test_codex_pergunta.sh:395`)

8. **Informativo — escritores e leitores pedidos.**

   | Arquivo/variável | Escritor → leitor |
   |---|---|
   | `VAGA` | Shell deriva de `VIVO` → reserva da resposta e abertura da trava. (`bin/codex_pergunta.sh:71`, `bin/codex_pergunta.sh:72`, `bin/codex_pergunta.sh:212`) |
   | `construtor.vaga` | Wrapper cria/abre em append → `flock` usa fd 6. O selo também cria, prende, move e restaura sua fixture. (`bin/codex_pergunta.sh:212`, `bin/codex_pergunta.sh:213`, `bin/tests/test_codex_pergunta.sh:175`, `bin/tests/test_codex_pergunta.sh:217`) |
   | `_INVENTARIO_PY` | Uma definição no shell → três chamadas Python. Não são três implementações independentes do juiz. (`bin/codex_pergunta.sh:90`, `bin/codex_pergunta.sh:217`, `bin/codex_pergunta.sh:366`, `bin/codex_pergunta.sh:393`) |
   | `_INV0` | Captura stdout/stderr do Python → exige sucesso e saída vazia. (`bin/codex_pergunta.sh:217`, `bin/codex_pergunta.sh:218`) |
   | `_INV1` | Captura stdout/stderr, acrescentando `FALHOU` em erro → exige saída vazia. (`bin/codex_pergunta.sh:366`, `bin/codex_pergunta.sh:369`) |
   | `.inventario`/`.recusa` | Python escreve inventário; shell acrescenta guarda de `.git` e gera recusa → testes conferem recusa e a prova copia evidências. (`bin/codex_pergunta.sh:393`, `bin/codex_pergunta.sh:394`, `bin/codex_pergunta.sh:398`, `bin/tests/test_codex_pergunta.sh:106`, `bin/tests/prova_codex_pergunta_real.sh:80`) |
   | `$EV/escuta` | Shell cria/inicia processo; ouvinte publica `porta` e acrescenta `conexoes` → shell lê porta e contagens; trap encerra o ouvinte. (`bin/tests/prova_codex_pergunta_real.sh:29`, `bin/tests/prova_codex_pergunta_real.sh:35`, `bin/tests/prova_codex_pergunta_real.sh:41`, `bin/tests/prova_codex_pergunta_real.sh:44`, `bin/tests/prova_codex_pergunta_real.sh:103`) |
   | `$EV/p` e `prova/*.out` | Shell escreve pergunta → wrapper lê; Codex é instruído a escrever `.out` → prova copia e julga. (`bin/tests/prova_codex_pergunta_real.sh:53`, `bin/tests/prova_codex_pergunta_real.sh:61`, `bin/tests/prova_codex_pergunta_real.sh:77`, `bin/tests/prova_codex_pergunta_real.sh:81`, `bin/tests/prova_codex_pergunta_real.sh:89`) |
   | Texto do `CLAUDE.md` | Autor da edição: `[nao verificado]`. Selo lê nomes/ponteiros; hashes cobrem blocos de **PISTA**, não as frases alteradas do item 1. O caso `c_p_claudemd` escreve outra cópia na fixture. (`bin/tests/test_claude_md_regras_preservadas.sh:44`, `bin/tests/test_claude_md_regras_preservadas.sh:72`, `bin/tests/test_claude_md_regras_preservadas.sh:83`, `bin/tests/test_codex_pergunta.sh:259`) |

9. **Informativo — classes históricas e papéis de leitura.**

   Aparecem **RED sem mordida pelo motivo alegado** e **fragmentação da exclusão de escritor**, conforme itens 4 e 7. Também há diferença material entre “blob Git” e “arquivo físico”, conforme item 3. Não identifiquei no delta clone de juiz de negócio registrado, relógio de negócio fora da borda, inversão nova da ordem de travas ou operação de pouso; a vaga precede slot, trava da resposta e revalidação. (`bin/codex_pergunta.sh:96`, `bin/codex_pergunta.sh:213`, `bin/codex_pergunta.sh:324`, `bin/codex_pergunta.sh:349`, `bin/codex_pergunta.sh:366`)

   Não identifiquei regressão no sandbox ou execução do inventário nos quatro papéis de leitura: permanecem no ramo `read-only`, com testes para argv, diretório e cabeçalho. A alteração compartilhada é a nova reserva do caminho da vaga. Execução efetiva desses testes: `[nao verificado]`. (`bin/codex_pergunta.sh:72`, `bin/codex_pergunta.sh:165`, `bin/codex_pergunta.sh:381`, `bin/tests/test_codex_pergunta.sh:327`)

10. **Informativo — limite da verificação.**

    A suite e a prova real não foram executadas: criam arquivos e a prova chama o Codex. Resultados completos desta volta, isolamento real e dados de produção: `[nao verificado]`. As confirmações mencionadas foram simulações exclusivamente em memória do inventário. (`bin/tests/test_codex_pergunta.sh:18`, `bin/tests/test_codex_pergunta.sh:19`, `bin/tests/prova_codex_pergunta_real.sh:21`, `bin/tests/prova_codex_pergunta_real.sh:77`)

## 07/11 13:23 · sessao cd2630cc4e18 · - · - · origem=Ronald (tmux)

1. **Alta — hardlink idêntico escapa da revalidação e do inventário final.** `st_nlink` só é consultado para caminhos já em `tocados`; substituir um arquivo rastreado por hardlink de mesmos bytes/modo não o inclui nesse conjunto. O `find -links +1` ocorre apenas inicialmente. Simulação em memória do código original: `nlink=2`, mesmo blob → rc 0, saída vazia. (`bin/codex_pergunta.sh:143`, `bin/codex_pergunta.sh:158`, `bin/codex_pergunta.sh:220`, `bin/codex_pergunta.sh:366`, `bin/codex_pergunta.sh:393`)

2. **Média — permissões não são inventariadas integralmente.** Só o bit executável do proprietário entra em `modo`; `0644→0666`, `0644→0600`, setuid/setgid e permissões de diretórios existentes ficam fora da comparação. Simulação em memória: `0644→0666` → rc 0, saída vazia. ACL/xattrs também não são lidos pelo inventário. (`bin/codex_pergunta.sh:114`, `bin/codex_pergunta.sh:116`, `bin/codex_pergunta.sh:139`, `bin/codex_pergunta.sh:143`)

3. **Média — FIFO/socket/device são reportados, mas aceitos.** Tipos não regulares entram em `tocados`, porém não recebem motivo por tipo especial. Simulação: FIFO novo `app/pipe` → `TOCADO`, rc 0. Diretório vazio novo também é reportado e aceito sob caminho permitido; permissões de diretórios existentes não são reportadas. (`bin/codex_pergunta.sh:139`, `bin/codex_pergunta.sh:143`, `bin/codex_pergunta.sh:153`, `bin/codex_pergunta.sh:160`)

4. **Média — varredura incompleta pode não ser tratada como falha.** `os.walk(wt)` não recebe `onerror`; erro de abertura de diretório pode omitir sua descendência. Blobs rastreados não vistos aparecem como removidos, mas novas entradas ocultas naquele ramo não são individualmente reportadas. Condições efetivas de permissão/Git em produção: `[nao verificado]`. (`bin/codex_pergunta.sh:132`, `bin/codex_pergunta.sh:145`, `bin/codex_pergunta.sh:146`)

5. **Média — nomes legítimos não UTF-8 recebem tratamento incompatível.** Git é decodificado com `"replace"`; o percurso de filesystem usa nomes reais, com outra representação para bytes inválidos. Um arquivo rastreado limpo pode aparecer simultaneamente novo/removido e reprovar o pre-check. Destino de symlink com bytes inválidos pode falhar no `.encode()` e produzir `FALHA`. (`bin/codex_pergunta.sh:112`, `bin/codex_pergunta.sh:122`, `bin/codex_pergunta.sh:128`, `bin/codex_pergunta.sh:132`, `bin/codex_pergunta.sh:217`)

6. **Média — Git limpo não implica blob físico idêntico.** Comparar bytes físicos diretamente com `ls-tree` recusa checkout legítimo transformado por CRLF/smudge/encoding. Não encontrei `.gitattributes` nesta cópia; configuração efetiva do Git, filtros ou ocorrência concreta: `[nao verificado]`. (`bin/codex_pergunta.sh:112`, `bin/codex_pergunta.sh:125`, `bin/codex_pergunta.sh:143`, `bin/codex_pergunta.sh:217`)

7. **Média — identidade de tipo de `.git` não é repetida.** A poda exclui `.git` da raiz tanto de `dirs` como de `arqs`; pre-check exige arquivo sem symlink, mas revalidação/final comparam conteúdo e resolução Git. Troca posterior por symlink de mesmos bytes não tem guarda explícita de tipo. Aceitação desse cenário pelo Git real: `[nao verificado]`. (`bin/codex_pergunta.sh:135`, `bin/codex_pergunta.sh:193`, `bin/codex_pergunta.sh:368`, `bin/codex_pergunta.sh:394`)

8. **Alta, condicional — o próprio inventário pode carregar autoload antes de recusá-lo.** As chamadas `python3 -c` não usam isolamento nem mudam o diretório corrente. Se o wrapper for iniciado em `WT/app`, um `sitecustomize.py` ali plantado pode executar na inicialização do Python do host antes da regra de proibição. Ambiente efetivo de inicialização Python: `[nao verificado]`. (`bin/codex_pergunta.sh:107`, `bin/codex_pergunta.sh:217`, `bin/codex_pergunta.sh:366`, `bin/codex_pergunta.sh:393`)

## 07/11 13:22 · sessao 6609b154c603 · - · - · origem=Ronald (tmux)

1. **Média — mutante `fs_walk` morre antes do comportamento que pretende provar.** A substituição desliga a varredura também no pre-check; todos os blobs ficam “não vistos”, gerando inventário não vazio e recusa rc 2 antes do Codex. O caso espera rc 3 final, portanto será contado como morto por antecipação: `bin/tests/test_codex_pergunta.sh:393`, `bin/codex_pergunta.sh:145`, `bin/codex_pergunta.sh:217`, `bin/tests/test_codex_pergunta.sh:105`. Execução efetiva da suite: `[nao verificado]`.

2. **Média — mutante `git_aninhado` mede mensagem, mantendo a proteção.** Remove somente o motivo suplementar `.git aninhado`; a regex `.git*` continua produzindo PROIBIDO e rc 3. O caso fica vermelho porque exige aquela substring: `bin/tests/test_codex_pergunta.sh:396`, `bin/codex_pergunta.sh:103`, `bin/codex_pergunta.sh:154`, `bin/tests/test_codex_pergunta.sh:106`, `bin/tests/test_codex_pergunta.sh:295`.

3. **Baixa — sentinela FSMON sem controle positivo de execução do hook.** O caso planta `.git/config` e verifica ausência de FSMON, mas sua ação não chama Git após plantar; o inventário chama Git na raiz da WT. Prova recusa do caminho, sem provar execução potencial do hook: `bin/tests/test_codex_pergunta.sh:291`, `bin/tests/test_codex_pergunta.sh:292`, `bin/tests/test_codex_pergunta.sh:76`, `bin/codex_pergunta.sh:110`.

4. **Sem defeito identificado nos mutantes `poda_so_raiz`.** `if True` poda `.git` tanto de diretórios quanto de arquivos; os dois casos perdem justamente a entrada proibida e ficam com diretórios comuns TOCADO. A expectativa rc 3 deixa de ser satisfeita pelo motivo declarado: `bin/tests/test_codex_pergunta.sh:394`, `bin/tests/test_codex_pergunta.sh:395`, `bin/tests/test_codex_pergunta.sh:295`, `bin/tests/test_codex_pergunta.sh:296`, `bin/codex_pergunta.sh:135`. Execução efetiva: `[nao verificado]`.

5. **Leitor do CLAUDE: nomes e referências, sem guarda das frases alteradas.** O selo preserva nomes de regras, âncoras e hashes dos blocos **PISTA**; não compara literalmente o texto CODEX item 1 do CLAUDE. Logo, verde nesse selo não certifica “DUAS escritas” ou “CONDUTA”: `bin/tests/test_claude_md_regras_preservadas.sh:43`, `bin/tests/test_claude_md_regras_preservadas.sh:55`, `bin/tests/test_claude_md_regras_preservadas.sh:72`, `bin/tests/test_claude_md_regras_preservadas.sh:83`, `CLAUDE.md:346`, `CLAUDE.md:352`. Outros leitores examinados julgam seção 3, seção 0 e senha no argv: `bin/tests/test_suite_sh.sh:22`, `bin/tests/test_lei_akita.sh:24`, `bin/tests/test_lei_akita.sh:35`, `bin/tests/test_segredo_fora_do_argv.sh:33`.

6. **Leitura: cobertura estática preservada; resultado executado não verificado.** Existem casos para os quatro papéis verificando sandbox, cwd, cabeçalho e ausência de sinais do construtor; o verificador usa B3 falso sempre livre. Os testes de sonda cobrem falha do envelope e lista vazia. `bin/tests/test_codex_pergunta.sh:327`, `bin/tests/test_codex_pergunta.sh:337`, `bin/tests/test_codex_pergunta.sh:57`, `bin/tests/test_codex_sonda.sh:71`, `bin/tests/test_codex_sonda.sh:77`. Não executei: a suite cria e remove arquivos temporários, contrariando a restrição de leitura (`bin/tests/test_codex_pergunta.sh:18`, `bin/tests/test_codex_pergunta.sh:19`).

## 07/11 13:21 · sessao 5bdc20a26151 · - · - · origem=Ronald (tmux)

1. **Alta — achado da prova fechado em parte.** TCP local e Unix ganharam medição fora do sandbox: o ouvinte grava conexões e o script compara as contagens antes/depois. Docker, docker.sock e TCP externo continuam julgados pelos `.out` escritos pelo Codex; o delta apenas rotula essa negativa como `AUTO-DECLARADO`, permitindo ainda rc 0. Referências: `bin/tests/prova_codex_pergunta_real.sh:30`, `:41`, `:49`, `:52`, `:101`, `:103`, `:117`, `:120`, `:124`.

2. **Alta — arquivo com `rc=` não comprova execução da tentativa.** A condição aceita qualquer `.out` não vazio contendo `rc=`; TCP local/Unix dispensam inclusive assinatura de erro. Criar o controle positivo e preencher os `.out` com negativas fabricadas satisfaz essas condições sem executar as tentativas; os demais controles externos precisam estar disponíveis para o conjunto retornar 0. Referências: `bin/tests/prova_codex_pergunta_real.sh:60`, `:88`, `:90`, `:92`, `:93`, `:98`, `:106`, `:107`, `:135`. Execução desse contraexemplo: `[nao verificado]`.

3. **Média — zero conexões não é prova suficiente de bloqueio durante toda a chamada.** O script confirma disponibilidade do ouvinte antes do Codex, mas depois apenas lê a contagem; não verifica que o processo continua vivo nem realiza controle posterior ou sincronização do processamento. Se o ouvinte morrer após os controles, permanece possível imprimir `OK MEDIDO ... 0 conexao`. Referências: `bin/tests/prova_codex_pergunta_real.sh:44`, `:49`, `:52`, `:77`, `:101`, `:103`, `:105`. Ocorrência real: `[nao verificado]`.

4. **Média — dois julgamentos continuam ativos para TCP local/Unix.** O comentário atribui o julgamento ao ouvinte, mas `CONECTOU` no `.out` ainda impõe rc 1 independentemente da medição externa. Uma saída fabricada pode produzir “VERMELHO conexão abriu” com contagem externa zero. É duplicação de julgamento da prova; relação com autoridade registrada em `app/core/juizes.py`: `[nao verificado]`. Referências: `bin/tests/prova_codex_pergunta_real.sh:92`, `:104`, `:105`, `:124`, `:125`.

5. **Média — `$EV` não identifica exclusivamente a execução.** O nome usa data até segundos e `mkdir -p` reutiliza o diretório. Duas execuções no mesmo segundo compartilham socket, `porta`, `conexoes`, prompt e evidências; não há recusa de diretório preexistente. O segundo ouvinte pode falhar no `bind` e consumir `porta` do primeiro. Referências: `bin/tests/prova_codex_pergunta_real.sh:21`, `:29`, `:30`, `:34`, `:35`, `:45`, `:46`, `:53`, `:80`, `:81`. Reprodução: `[nao verificado]`.

6. **Informativo — escritores/leitores da prova.** O shell cria `$EV/escuta`, inicia e encerra o ouvinte; o Python escreve `porta.novo`, renomeia para `porta` e acrescenta `conexoes`; o shell lê `porta` e as contagens. O shell escreve `$EV/p`; o wrapper o lê. O Codex recebe instrução para escrever `<wt>/app/prova/*.out`, e o shell os copia/lê para produzir o veredito. Referências: `bin/tests/prova_codex_pergunta_real.sh:21`, `:29`, `:30`, `:35`, `:41`, `:44`, `:46`, `:52`, `:53`, `:61`, `:77`, `:81`, `:89`, `:103`, `:115`.

7. **Informativo — limites desta auditoria.** O ramo de leitura da prova permanece separado após o término do ramo construtor; o ouvinte está somente no construtor. Referências: `bin/tests/prova_codex_pergunta_real.sh:18`, `:135`, `:137`. Não executei o script, que cria arquivos e chama o wrapper (`:21`, `:53`, `:77`); resultados reais e mutantes executados: `[nao verificado]`.

## 07/11 12:59 · sessao a1c39fc87f85 · FIO-E0-V4-E2 · verificador · origem=Code

**A paridade no presente foi restaurada para o predicado de 57acffd2 informado no pedido. Permanece um conflito com a regra escrita: a mesma pergunta retratada pode voltar viva após ressurreição e resposta posterior.** ([seleção atual](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:211), [regra de identidade](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/models.py:2321))

Identidade Git da cópia, comparação direta com a árvore de 57acffd2, execução integrada Django, SQL medido e ocorrência produtiva dos estados abaixo: **[nao verificado]**.

1. **Classe: equivalência da seleção no presente — procedente quanto ao predicado fornecido.**

   Se `agora >= todos os atos`, nenhuma retratação alimenta `_retratada_depois`. Portanto, a seleção reduz-se exatamente a: excluir via em `VIAS_FORA_DO_FATO`; depois excluir última decisão `retratacao`. ([carga](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:321), [seleção](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:211))

   Na tabela, cada célula vale **57 conforme descrito / atual**; “entra” significa passar pela seleção, antes das demais guardas da tarefa. As classes de sequência são exaustivas porque o predicado consulta somente sua existência e seu último ato. ([tarefa_do_dia.py:214](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:214))

   | Via atual | Sem decisões | Última = retratação | Sem retratação; última diferente | Retratação antiga; última diferente |
   |---|---|---|---|---|
   | `duplicata` | exclui / exclui | exclui / exclui | exclui / exclui | exclui / exclui |
   | `motivo_retratado` | exclui / exclui | exclui / exclui | exclui / exclui | exclui / exclui |
   | Vazia ou qualquer outra via | entra / entra | exclui / exclui | entra / entra | entra / entra |

   As duas vias excluídas são exatamente as declaradas pelo modelo; os resultados atuais da tabela seguem dos dois `continue`. ([models.py:2323](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/models.py:2323), [tarefa_do_dia.py:211](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:211), [tarefa_do_dia.py:215](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:215))

2. **Classe: horizonte temporal versus identidade retratada — corte implementado; conflito normativo da ressuscitada permanece.**

   | Estado em relação a `agora` | Resultado atual | Procedência |
   |---|---|---|
   | `motivo_retratado`; resposta anterior; única retratação futura | Entra; `aguarda_validacao` | A retratação futura libera a via; a resposta é a última decisão retida. ([seleção:212](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:212), [teste:761](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/tests/test_e2_tarefa_do_dia.py:761)) |
   | `motivo_retratado`; retratação anterior ou igual; nenhuma futura | Excluída, mesmo havendo resposta posterior | A via vence a resposta. ([models.py:2327](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/models.py:2327), [seleção:211](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:211)) |
   | `motivo_retratado`; nenhuma retratação | Excluída | A porta genérica aceita a via sem exigir linha correspondente; a seleção conserva a exclusão. ([porta:2736](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/models.py:2736), [teste:777](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/tests/test_e2_tarefa_do_dia.py:777)) |
   | Via vazia; retratação antiga; resposta posterior, ambas até `agora` | Entra com a mesma PK; `aguarda_validacao` | É o comportamento exigido pelo teste novo. ([teste:778](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/tests/test_e2_tarefa_do_dia.py:778), [teste:783](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/tests/test_e2_tarefa_do_dia.py:783)) |
   | Há retratação futura, mas a última decisão **até agora** também é retratação | Excluída | Liberar a via não elimina a segunda guarda. ([seleção:212](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:212), [seleção:215](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:215)) |

   **O contraexemplo à regra escrita é a ressuscitada.** `remover_motivo` e `VIAS_FORA_DO_FATO` dizem expressamente que o fato que volta ganha pergunta nova. A tarefa e o teste novo aceitam reutilizar a antiga após `[retratacao, resposta]`. Paridade com o comportamento anterior não resolve essa divergência. ([models.py:1979](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/models.py:1979), [models.py:2321](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/models.py:2321), [teste:783](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/tests/test_e2_tarefa_do_dia.py:783))

   **Origem: conflito preexistente na ressurreição, novamente aceito pela seleção neste delta.** O universo busca carimbadas sem texto, descarimba a cópia antes do C6 e a porta descarimba o registro; a decisão antiga continua imutável. ([universo:1440](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/models.py:1440), [simulação:1461](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/models.py:1461), [escrita:1515](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/models.py:1515), [imutabilidade:3393](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/models.py:3393))

   Há ainda uma limitação precisa: `_retratada_depois` identifica **alguma** retratação futura, sem ligar a via atual ao ato específico que a escreveu. Com retratação antiga, resposta aplicável e nova retratação futura, a pergunta entra. A garantia de correspondência entre essa via e essa retratação específica: **[nao verificado]**. ([carga:323](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:323), [seleção:212](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:212))

3. **Classe: vazamento de ato futuro e queries por N — não achei vazamento pelo `_dec` nem N+1 introduzido nesse delta.**

   | Objeto | Quem escreve | Quem lê |
   |---|---|---|
   | `_retratada_depois` | `CargaDaTarefa.__init__`: inicializa e acrescenta apenas `pergunta_id` de retratação futura. ([314](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:314), [324](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:324)) | `.tarefa` encaminha; `_tarefa_pura` consulta pertinência para liberar a via. ([380](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:380), [212](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:212)) |
   | `_dec` | A carga acrescenta objetos somente sob `d.ato_em <= agora`. ([321](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:321)) | Seleção consulta a última; `_pela_decisao` recebe a lista da pergunta. ([214](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:214), [267](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:267)) |

   Para instantes válidos e comparáveis, o corte Python conserva o significado inclusivo de `ato_em__lte=agora`, incluindo a igualdade e preservando a ordem `(ato_em, pk)` dentro de cada pergunta. `ato_em` não admite nulo no modelo e `USE_TZ=True`. Execução comparativa contra SQL: **[nao verificado]**. ([modelo:3358](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/models.py:3358), [ordenação/corte:320](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:320), [configuração:95](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/config/settings/base.py:95))

   | Caminho examinado | Conclusão |
   |---|---|
   | `absorcao_ausencia` | Lê somente a última decisão retida; o `select_related` continua presente. O **status atual** da ausência permanece um limite declarado, não um ato futuro entrando em `_dec`. ([carga:320](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:320), [leitor:145](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:145), [limite:290](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:290)) |
   | Efeitos | Carga independente de `_dec`, com `criado_em__lte=agora`; conserva o universo atual de Batidas apuráveis. ([340](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:340)) |
   | Passivo | Carga independente; correspondência exata de pergunta, ato e instante, seguida de corte `<= agora`. Uma linha futura não corresponde a um carimbo anterior. ([models.py:3412](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/models.py:3412), [carga:328](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:328)) |
   | Lavra | Carga independente, com `marcos_do_lote(ate=agora)`; `esta_stale` rejeita julgamento futuro. ([332](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:332), [veredito_lavrado.py:196](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/veredito_lavrado.py:196)) |

   Remover o filtro SQL aumenta as **linhas transportadas**, incluindo decisões futuras e suas ausências relacionadas; não acrescenta consulta por pergunta nesse caminho. O selo de constância inclui validações, mas não monta absorções; sua execução e contagem efetiva permanecem **[nao verificado]**. ([query:319](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:319), [teste:920](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/tests/test_e2_tarefa_do_dia.py:920), [comparação:937](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/tests/test_e2_tarefa_do_dia.py:937))

4. **Classes de reprovação — consolidação.**

   | Classe | Procedência e alcance |
   |---|---|
   | Segundo juiz | **Não achei introdução no delta.** A tarefa possui corte explícito e a coexistência até E3 está declarada. Isso não sana o conflito de identidade do item 2. ([CORTES.md:424](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/docs/CORTES.md:424)) |
   | Segundo escritor | **Não achei escritor persistente novo.** As coleções novas são memória; `remover_motivo` continua registrando decisão e carimbando identidade no mesmo `atomic`. ([carga:313](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:313), [models.py:1986](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/models.py:1986)) |
   | Relógio fora da borda | **Procedente, preexistente; já encaminhado como fila.** Cobertura consulta tipos em aberto, cujo conjunto vivo chega a `vigente → monotonic`, com mutação do cache. ([turnos.py:2762](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/ponto/turnos.py:2762), [ausencias.py:433](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/ponto/catalogo/ausencias.py:433), [ausencias.py:275](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/ponto/catalogo/ausencias.py:275)) |
   | Derivação paralela | **Não achei nova nos caminhos alterados:** lista de identidade do modelo, autoridade E1 do passivo, porta das Batidas e regra compartilhada de posição continuam sendo lidas. ([211](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:211), [328](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:328), [340](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:340), [55](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:55)) |
   | Campo com dois significados | **Não achei significado novo no delta.** O conjunto guarda existência de retratação futura; não guarda seu instante nem comprova qual ato escreveu a via. `manter_carimbo` já preservava instante anterior ao trocar via. ([324](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:324), [models.py:2738](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/models.py:2738)) |

   **RED sem mordida: não procede para o teste novo, na comparação dos corpos executados em memória com autoridades alimentadas.** O predicado sem delta foi reconstruído do anexo; histórico Git e Django integrado: **[nao verificado]**. ([corpo atual:157](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:157))

   | Cenário | Sem delta | Atual / exigido pelo teste | [nao verificado]
   |---|---|---|
   | `motivo_retratado`, somente resposta | `aguarda_validacao`, mesma PK | `sem_pergunta`, sem PK. ([asserção:777](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/tests/test_e2_tarefa_do_dia.py:777)) |
   | Via vazia, retratação antiga e resposta posterior | `sem_pergunta`, sem PK | `aguarda_validacao`, mesma PK. ([asserção:783](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/tests/test_e2_tarefa_do_dia.py:783)) |

   Sem delta, o teste para na primeira asserção; o segundo cenário também discrimina quando isolado. **A mordida existe, mas a segunda expectativa certifica a paridade restaurada, permanecendo contrária à regra escrita de pergunta nova.** ([teste:777](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/tests/test_e2_tarefa_do_dia.py:777), [teste:783](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/tests/test_e2_tarefa_do_dia.py:783), [models.py:2321](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/models.py:2321))

## 07/11 12:57 · sessao a97064b5e654 · - · - · origem=Ronald (tmux)

1. **RED sem mordida: não procede para o teste novo, por execução dos corpos em memória.** Extraí `_acao`, `_pela_decisao` e `_tarefa_pura` via AST, retirei imports e alimentei as autoridades; reconstruí o predicado antigo conforme o delta fornecido. Resultado: (`app/chamados/services/tarefa_do_dia.py:105`, `app/chamados/services/tarefa_do_dia.py:109`, `app/chamados/services/tarefa_do_dia.py:157`)

   | Caso | Sem delta | Atual | Asserção |
   |---|---|---|---|
   | `motivo_retratado`, apenas resposta | `p.pk, aguarda_validacao` | `None, sem_pergunta` | Atual exigido em `app/chamados/tests/test_e2_tarefa_do_dia.py:777` |
   | Via vazia, resposta → retratação → resposta | `None, sem_pergunta` | `p.pk, aguarda_validacao` | Atual exigido em `app/chamados/tests/test_e2_tarefa_do_dia.py:783` |

   **O teste completo sem delta falha já na primeira asserção; seu segundo cenário não chega a executar nessa rodada.** Isoladamente, o segundo cenário também discrimina os predicados, conforme execução em memória. Django integrado, RED/GREEN históricos e identidade Git: **[nao verificado]**. (`app/chamados/tests/test_e2_tarefa_do_dia.py:775`, `app/chamados/tests/test_e2_tarefa_do_dia.py:777`, `app/chamados/tests/test_e2_tarefa_do_dia.py:778`)

2. **Segundo juiz: não achei introdução no delta fornecido.** O predicado atual lê a lista única `VIAS_FORA_DO_FATO`; o juiz da tarefa tem registro e corte explícitos, com coexistência dos cinco respondedores até E3 declarada. (`app/chamados/services/tarefa_do_dia.py:211`, `app/chamados/models.py:2323`, `app/core/juizes.py:285`, `app/docs/CORTES.md:424`)

3. **Segundo escritor persistente: não achei introdução nos trechos alterados.** `_dec` e `_retratada_depois` são coleções em memória; a carga só as alimenta e encaminha. As portas persistentes continuam sendo `carimbar_identidade` e `DecisaoPergunta.registrar`; `remover_motivo` chama ambas no mesmo `atomic`. (`app/chamados/services/tarefa_do_dia.py:313`, `app/chamados/services/tarefa_do_dia.py:322`, `app/chamados/services/tarefa_do_dia.py:324`, `app/chamados/services/tarefa_do_dia.py:378`, `app/chamados/models.py:1986`, `app/chamados/models.py:1989`, `app/chamados/models.py:1994`, `app/chamados/models.py:2746`, `app/chamados/models.py:3428`)

4. **Relógio fora da borda: procedente, preexistente e já declarado como fila pelo usuário.** Caminho efetivamente lido: tarefa → `cobertura` → `_q_cobertura` → `Ausencia.TIPOS_EM_ABERTO` → conjunto vivo → `em_aberto_vigentes` → `vigente` → `monotonic`. Também há mutação do cache de processo. (`app/chamados/services/tarefa_do_dia.py:378`, `app/ponto/turnos.py:3072`, `app/ponto/turnos.py:2762`, `app/ponto/models.py:689`, `app/ponto/catalogo/ausencias.py:464`, `app/ponto/catalogo/ausencias.py:433`, `app/ponto/catalogo/ausencias.py:275`, `app/ponto/catalogo/ausencias.py:279`)

   O veneno dinâmico intercepta apenas `timezone.now`; o selo estático examina apenas tarefa e lavra, deixando catálogo fora. (`app/chamados/tests/test_e2_tarefa_do_dia.py:680`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1010`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1011`)

5. **Derivação paralela: não achei nova nos caminhos alterados.** A seleção lê a lista de identidade do modelo; passivo continua usando a autoridade E1; efeito usa `batidas_apuraveis`; posição chama a regra compartilhada; cobertura usa filtro e corpo compartilhados. (`app/chamados/services/tarefa_do_dia.py:211`, `app/chamados/services/tarefa_do_dia.py:328`, `app/chamados/services/tarefa_do_dia.py:340`, `app/chamados/services/tarefa_do_dia.py:55`, `app/ponto/turnos.py:3072`, `app/ponto/turnos.py:3076`)

6. **Campo com dois significados: não achei significado novo no delta.** `_retratada_depois` significa existência de **alguma** retratação com `ato_em > agora`, independentemente do último ato futuro; não carrega a própria decisão. `ato_em` mantém seu papel temporal. (`app/chamados/services/tarefa_do_dia.py:321`, `app/chamados/services/tarefa_do_dia.py:323`, `app/chamados/services/tarefa_do_dia.py:324`)

   Permanecem duas ressalvas anteriores: `materializada_em` pode conservar instante anterior ao trocar a via com `manter_carimbo`; resposta/validação projetadas podem receber instante do fechamento por `carimbar`. Não são introduções desta volta. (`app/chamados/models.py:2738`, `app/chamados/models.py:2739`, `app/chamados/models.py:2667`)

7. **Docstring: correções solicitadas presentes.** A abertura agora limita o corte aos fatos datados da pergunta; enumeração inclui cadastro e catálogo condicionado ao TTL. (`app/chamados/tests/test_e2_tarefa_do_dia.py:702`, `app/chamados/services/tarefa_do_dia.py:282`, `app/chamados/services/tarefa_do_dia.py:283`)

Não existe repositório Git utilizável no diretório consultado; comparação histórica direta `53db08a0..e2dc08e2`: **[nao verificado]**. A comparação acima usa o delta fornecido e os arquivos da cópia; nenhuma suíte Django foi executada: **[nao verificado]**.

## 07/11 12:56 · sessao e4e5706c2a05 · - · - · origem=Ronald (tmux)

- **Paridade no presente: procede para o predicado informado de 57acffd2.** Quando `agora >= todos os atos`, `_retratada_depois` fica vazio; a seleção atual se reduz a “via fora do fato exclui; última decisão retratação exclui”. (`app/chamados/services/tarefa_do_dia.py:211`, `app/chamados/services/tarefa_do_dia.py:215`, `app/chamados/services/tarefa_do_dia.py:321`)

- **Regra escrita versus ressuscitada: permanece conflito, apesar da paridade.** A lei na cópia diz explicitamente que a retratada morre por identidade e, se o fato voltar, nasce pergunta nova. A tarefa admite a mesma pergunta com via vazia e sequência `[retratacao, resposta]`, atribuindo `aguarda_validacao`. O teste novo exige justamente esse comportamento. (`app/chamados/models.py:1979`, `app/chamados/models.py:2320`, `app/chamados/services/tarefa_do_dia.py:211`, `app/chamados/services/tarefa_do_dia.py:215`, `app/chamados/services/tarefa_do_dia.py:129`, `app/chamados/tests/test_e2_tarefa_do_dia.py:766`)

- **Não é só construção impossível pela API:** a ressurreição busca carimbadas sem texto, limpa via na cópia antes de consultar C6 e depois limpa via real; não remove a retratação imutável. C6 aceita célula acusando depois desse descarimbo. Esse caminho não exclui `motivo_retratado`; o cartório exclui apenas o caso em que todas são duplicatas. Uma resposta posterior pode entrar na sequência pela porta normal. Ocorrência produtiva **[nao verificado]**. (`app/chamados/models.py:1440`, `app/chamados/models.py:1461`, `app/chamados/models.py:1515`, `app/chamados/models.py:3393`, `app/chamados/juizes.py:1183`, `app/ponto/services/cartorio.py:1610`, `app/ponto/services/cartorio.py:1633`, `app/chamados/services/disputa_emissao.py:1369`)

- **Há testemunha de regra oposta, já existente:** `test_RED_motivo_retratado_que_volta_nasce_pergunta_nova` exige PK nova e preservação da via retratada na antiga. Embora esse caso seja motivo sem data e não o marco da tarefa, confirma a intenção documental sobre identidade; não prova que a ressurreição de marco obedeça à regra. (`app/chamados/tests/test_e1_volta2.py:244`, `app/chamados/tests/test_e1_volta2.py:260`, `app/chamados/tests/test_e1_volta2.py:261`)

| Via/sequência no horizonte | Seleção atual | Procedência |
|---|---|---|
| `motivo_retratado`, única retratação futura | Via cede; vive se última decisão até `agora` não for retratação | Corte temporal implementado. (`app/chamados/services/tarefa_do_dia.py:211`, `app/chamados/services/tarefa_do_dia.py:323`) |
| `motivo_retratado`, retratação anterior, sem retratação futura | Excluída pela via, mesmo com resposta posterior | Compatível com via que vence resposta. (`app/chamados/models.py:2327`, `app/chamados/services/tarefa_do_dia.py:211`) |
| `motivo_retratado`, nenhuma retratação | Excluída pela via | Porta genérica aceita essa combinação, sem exigir decisão. (`app/chamados/models.py:2736`, `app/chamados/models.py:2746`, `app/chamados/services/tarefa_do_dia.py:211`) |
| Via vazia, retratação antiga + resposta anterior a `agora` | Viva, mesma PK | Paridade restaurada; conflito com “nasce pergunta nova” permanece. (`app/chamados/services/tarefa_do_dia.py:215`, `app/chamados/models.py:2321`) |
| `motivo_retratado`, retratação antiga + resposta até `agora` + nova retratação futura | Vive: qualquer retratação futura libera a via, e a última decisão aplicável é resposta | O conjunto não identifica qual retratação originou a via atual; resta a mesma questão de reutilização de identidade. (`app/chamados/services/tarefa_do_dia.py:323`, `app/chamados/services/tarefa_do_dia.py:211`, `app/chamados/services/tarefa_do_dia.py:215`) |

**Classificação sugerida:** conflito preexistente entre porta de ressurreição e regra de identidade, que o delta volta a aceitar na seleção; não classificar a equivalência com 57 como prova de conformidade à lei. (`app/chamados/models.py:1515`, `app/chamados/models.py:2321`, `app/chamados/services/tarefa_do_dia.py:215`)

## 07/11 12:56 · sessao f420c0cfdb6b · - · - · origem=Ronald (tmux)

**Classe: vazamento de ato futuro — não achei no `_dec` nem nos seus leitores examinados.** A retirada do filtro SQL amplia o que a query transporta, mas o append continua sob `d.ato_em <= agora`; o ramo futuro só guarda o ID quando o ato é `RETRATACAO`. A ordem continua `(pergunta_id, ato_em, pk)`. (`app/chamados/services/tarefa_do_dia.py:319`, `app/chamados/services/tarefa_do_dia.py:321`, `app/chamados/services/tarefa_do_dia.py:323`)

| Objeto | Escritor | Leitor |
|---|---|---|
| `self._retratada_depois` | Inicialização vazia e `add(pergunta_id)` exclusivamente para retratação futura. (`app/chamados/services/tarefa_do_dia.py:314`, `app/chamados/services/tarefa_do_dia.py:323`) | `.tarefa` passa para `_tarefa_pura`; esta usa apenas pertinência do ID para a exceção da via `motivo_retratado`. (`app/chamados/services/tarefa_do_dia.py:380`, `app/chamados/services/tarefa_do_dia.py:211`) |
| `self._dec` | Inicialização vazia e append exclusivamente no ramo `<= agora`. (`app/chamados/services/tarefa_do_dia.py:313`, `app/chamados/services/tarefa_do_dia.py:321`) | `.tarefa` passa o dict; seleção lê a última decisão e `_pela_decisao` recebe a lista dessa pergunta. (`app/chamados/services/tarefa_do_dia.py:379`, `app/chamados/services/tarefa_do_dia.py:214`, `app/chamados/services/tarefa_do_dia.py:267`) |

**Classe: equivalência do corte temporal — sustentada pelo código para instantes comparáveis válidos; execução SQL/Django [nao verificado].** `ato_em` é `DateTimeField` sem `null=True`; tanto `ato_em__lte=agora` quanto `d.ato_em <= agora` incluem igualdade, e filtrar a sequência já ordenada conserva a ordem relativa retida. (`app/chamados/models.py:3358`, `app/chamados/services/tarefa_do_dia.py:320`, `app/chamados/services/tarefa_do_dia.py:321`)

| Leitor | Conclusão examinada |
|---|---|
| `absorcao_ausencia` | O `select_related` permanece; `_pela_decisao` lê o status somente da última decisão retida. O status é **atual**, conforme limite declarado, e não fotografia histórica da ausência. Nenhum ato futuro passa por esse ramo via `_dec`. (`app/chamados/services/tarefa_do_dia.py:320`, `app/chamados/services/tarefa_do_dia.py:125`, `app/chamados/services/tarefa_do_dia.py:145`, `app/chamados/services/tarefa_do_dia.py:290`) |
| Efeitos | Não deriva de `_dec`: vem de query própria de Batidas apuráveis com `criado_em__lte=agora`. A retratação atual da Batida continua excluindo-a, inclusive quando posterior a `agora`, limite já declarado. (`app/chamados/services/tarefa_do_dia.py:340`, `app/ponto/turnos.py:2876`, `app/chamados/services/tarefa_do_dia.py:338`) |
| Passivo | Não deriva de `_dec`: a autoridade consulta correspondência exata `(pergunta_id, ato, ato_em)` e a carga corta as linhas devolvidas por `<= agora`. Uma decisão futura não pode satisfazer correspondência com um carimbo anterior porque o instante deve ser idêntico. (`app/chamados/models.py:3412`, `app/chamados/models.py:3416`, `app/chamados/models.py:3420`, `app/chamados/services/tarefa_do_dia.py:328`) |
| Lavra | Não deriva de `_dec`: usa `marcos_do_lote(ate=agora)` e `esta_stale(..., agora)`. `julgado_em > agora` é recusado antes dos demais motivos. (`app/chamados/services/tarefa_do_dia.py:332`, `app/chamados/services/tarefa_do_dia.py:333`, `app/chamados/veredito_lavrado.py:196`) |

**Classe: queries por N — não achei introdução de N+1 nesse delta; volume de linhas transportadas aumenta.** A decisão segue em uma query de lote com ausência relacionada; o passivo faz outra query de lote; o acesso à âncora usa IDs e a disputa já relacionada. Remover `ato_em__lte` carrega também todas as decisões futuras das perguntas selecionadas, sem acrescentar uma query por pergunta. (`app/chamados/services/tarefa_do_dia.py:309`, `app/chamados/services/tarefa_do_dia.py:319`, `app/chamados/models.py:3412`, `app/chamados/services/fio.py:116`)

O teste de constância compara uma carga com dois colaboradores/três dias contra um colaborador/um dia, incluindo decisões até validação; não inclui absorções nessa montagem. Seu resultado executado e contagem efetiva: **[nao verificado]**. (`app/chamados/tests/test_e2_tarefa_do_dia.py:912`, `app/chamados/tests/test_e2_tarefa_do_dia.py:920`, `app/chamados/tests/test_e2_tarefa_do_dia.py:937`)

## 07/11 12:41 · sessao 216139486a7d · FIO-E0-V4-E2 · verificador · origem=Code

**O delta corrige os dois contraexemplos anteriores, mas não é incondicionalmente neutro no presente.** A correspondência entre via e retratação não é imposta pela porta genérica; também achei um caminho de ressurreição que limpa a via e conserva a retratação antiga. (`app/chamados/models.py:2727`, `app/chamados/models.py:2746`, `app/chamados/models.py:1515`, `app/chamados/services/tarefa_do_dia.py:210`)

Ocorrência desses estados em produção, execução integrada Django, RED/GREEN históricos e identidade Git da cópia: **[nao verificado]**.

1. **Classe: correspondência entre via e sequência — garantia estrutural incompleta; procedente na API, ocorrência produtiva [nao verificado].**

   | Objeto | Escritores encontrados |
   |---|---|
   | Via `motivo_retratado` | `remover_motivo` registra `RETRATACAO` e chama `carimbar_identidade` no mesmo `atomic`. Foi o único chamador produtivo explícito dessa via que achei. (`app/chamados/models.py:1986`, `app/chamados/models.py:1989`, `app/chamados/models.py:1994`) |
   | Via `duplicata` | `retratar_duplicata → carimbar_identidade`; chamadores encontrados: deduplicação e migração de resolvedoras vivas. Não registra decisão `retratacao`. (`app/chamados/services/fio.py:939`, `app/chamados/services/fio.py:952`, `app/chamados/services/fio.py:1004`, `app/chamados/management/commands/dedup_perguntas_fato.py:88`) |
   | Atribuição física das vias | `_escrever_carimbo`; a porta de identidade salva os três campos. `manter_carimbo=True` pode trocar a via preservando instante e autor anteriores. (`app/chamados/models.py:2658`, `app/chamados/models.py:2738`, `app/chamados/models.py:2746`) |
   | Decisão `retratacao` | Chamador produtivo encontrado: `remover_motivo`, pela porta `DecisaoPergunta.registrar`, cujo corpo faz o `bulk_create`. (`app/chamados/models.py:1989`, `app/chamados/models.py:3428`) |

   **Existe testemunha na cópia de via sem decisão:** `test_RED_retratada_nao_esta_no_dominio_do_lote` chama somente `carimbar_identidade(VIA_MOTIVO_RETRATADO, ...)`. A porta aceita a via e não registra retratação. Isso demonstra possibilidade pela API e uso em fixture, sem demonstrar dado produtivo. (`app/chamados/tests/test_e1_volta2.py:128`, `app/chamados/tests/test_e1_volta2.py:131`, `app/chamados/models.py:2736`, `app/chamados/models.py:2746`)

   Não achei migração/backfill que produza essa combinação: a 0051 acrescenta a escolha, converte decisões antigas em `reabertura` e preenche a chave; o backfill de decisões é DRY e recupera resposta/validação. (`app/chamados/migrations/0051_decisao_pergunta.py:22`, `app/chamados/migrations/0051_decisao_pergunta.py:49`, `app/chamados/migrations/0051_decisao_pergunta.py:111`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:7`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:29`)

   Os leitores do domínio do fato e da emissão excluem ambas as vias; `q_vivas` e C6 leem via/carimbo atuais; o juiz do lastro lê o conjunto de identidade. A tarefa passa a excluir duplicata pela via e retratada pela sequência datada. (`app/chamados/models.py:2450`, `app/chamados/services/disputa_emissao.py:323`, `app/chamados/models.py:2888`, `app/chamados/juizes.py:1147`, `app/chamados/juizes.py:658`, `app/chamados/services/tarefa_do_dia.py:208`)

   **Sem nenhuma linha `retratacao`, a via `motivo_retratado` deixa de excluí-la em qualquer `agora`.** Se a pergunta for escolhida para um marco vencido/apagado e superar as guardas anteriores, seguirá `_pela_decisao`: sem decisões, `aguarda_resposta/responder`; com resposta, `aguarda_validacao/validar`; com passivo aplicável, `passivo_sem_linha/nao_sei`. Não existe tratamento específico de inconsistência via/ato. (`app/chamados/services/tarefa_do_dia.py:208`, `app/chamados/services/tarefa_do_dia.py:210`, `app/chamados/services/tarefa_do_dia.py:260`, `app/chamados/services/tarefa_do_dia.py:121`, `app/chamados/services/tarefa_do_dia.py:129`)

2. **Classe: mudança de comportamento no presente — procedente; não significa que o comportamento anterior fosse correto.**

   Reconstituí em memória o ramo anterior fornecido e comparei os corpos, com `agora` posterior aos atos e marco exigível: (`app/chamados/services/tarefa_do_dia.py:208`, `app/chamados/services/tarefa_do_dia.py:210`, `app/chamados/services/tarefa_do_dia.py:262`)

   | Via atual / sequência | Antes do delta | Atual |
   |---|---|---|
   | `duplicata`, qualquer sequência | Excluída | Excluída |
   | `motivo_retratado`, com retratação aplicável | Excluída pela via | Excluída pela decisão |
   | `motivo_retratado`, somente resposta | `sem_pergunta/perguntar` | `aguarda_validacao/validar` |
   | Via vazia, `[retratacao, resposta]` | `aguarda_validacao/validar` | `sem_pergunta/perguntar` |

   Os resultados decorrem da seleção e dos ramos preservados de resposta/ausência de pergunta. (`app/chamados/services/tarefa_do_dia.py:129`, `app/chamados/services/tarefa_do_dia.py:208`, `app/chamados/services/tarefa_do_dia.py:210`, `app/chamados/services/tarefa_do_dia.py:256`)

   **O último estado tem caminho no código de produção:**

   - O cartório considera todas as perguntas do destino; sem fio vivo nem texto, e não sendo todas duplicatas, chama `ressuscitar_perguntas`. Não exclui motivo retratado nesse bloco. (`app/ponto/services/cartorio.py:1605`, `app/ponto/services/cartorio.py:1608`, `app/ponto/services/cartorio.py:1610`, `app/ponto/services/cartorio.py:1633`)
   - O universo da ressurreição aceita carimbadas sem texto e **limpa a via na cópia antes de consultar C6**. Com célula acusando e marco sem fato, essa cópia pode ser julgada viva. Confirmei essa seleção executando os corpos em memória com dependências alimentadas. (`app/chamados/models.py:1440`, `app/chamados/models.py:1461`, `app/chamados/models.py:1464`, `app/chamados/juizes.py:1183`)
   - O lote limpa a via real e reabre a disputa; não apaga a decisão imutável antiga. A API pode então aceitar nova resposta pela vitalidade atual e registrar `resposta` posterior. (`app/chamados/models.py:1515`, `app/chamados/models.py:2801`, `app/chamados/models.py:1542`, `app/api/views.py:2647`, `app/api/views.py:2659`, `app/chamados/services/disputa_emissao.py:1369`)

   Portanto, não confirmei equivalência no presente pelos dois caminhos. Entretanto, a regra documentada determina que a retratada saia da identidade e que o fato que volte ganhe pergunta nova; o `any` é compatível com essa intenção. O caminho de ressurreição é que não preserva essa separação. (`app/chamados/models.py:2320`, `app/chamados/models.py:2323`, `app/chamados/models.py:1461`, `app/chamados/services/tarefa_do_dia.py:210`)

3. **Classe: corte temporal do passivo — não achei defeito novo; equivalência de instantes sustentada pelo código.**

   O dict devolve `ato_em=respondida_em` para resposta e `ato_em=validada_em` para validação. A correspondência exige exatamente `(pergunta_id, ato, instante)`. Assim, `<= agora` tem a mesma inclusão temporal de `ato_em__lte=agora`; são campos `DateTimeField`, com configuração timezone-aware. Consistência de cada registro produtivo: **[nao verificado]**. (`app/chamados/models.py:3412`, `app/chamados/models.py:3418`, `app/chamados/models.py:3422`, `app/chamados/services/tarefa_do_dia.py:310`, `app/chamados/services/tarefa_do_dia.py:316`, `app/config/settings/base.py:93`)

   O censo de escritores dos carimbos encontrou: (`app/chamados/models.py:2257`, `app/chamados/models.py:2258`)

   | Operação | Escrita |
   |---|---|
   | Resposta normal | Projeção e decisão recebem o mesmo `_agora`. (`app/chamados/services/disputa_emissao.py:1340`, `app/chamados/services/disputa_emissao.py:1346`, `app/chamados/services/disputa_emissao.py:1369`) |
   | Resposta automática / recuperação pela trilha | Recebem instante externo, também usado na decisão. (`app/chamados/services/disputa_emissao.py:1516`, `app/chamados/services/disputa_emissao.py:1519`, `app/chamados/services/disputa_emissao.py:1537`, `app/chamados/services/disputa_emissao.py:1540`) |
   | Validação | Projeção recebe `timezone.now()`; decisão recebe exatamente esse valor. (`app/chamados/services/validacao.py:134`, `app/chamados/services/validacao.py:139`) |
   | `carimbar` | Preenche campos vazios com `quando`, havendo texto. (`app/chamados/models.py:2683`, `app/chamados/models.py:2691`, `app/chamados/models.py:2694`) |
   | Nova resposta / devolução | Zeram validação anterior. (`app/chamados/services/disputa_emissao.py:1358`, `app/chamados/services/validacao.py:199`) |
   | Reabertura / ressurreição | Zeram os carimbos pelas portas existentes. (`app/chamados/services/disputa_emissao.py:1462`, `app/chamados/services/disputa_emissao.py:1493`, `app/chamados/models.py:1515`) |
   | Restauração / simulação | Restauram fotografia em memória ou alteram somente a cópia. (`app/chamados/models.py:2722`, `app/chamados/models.py:1462`) |

   Os leitores relevantes incluem estado/vitalidade, seleção para materialização, correspondência do passivo e invalidação da lavra. O novo `_pela_decisao` consulta o conjunto de passivo, sem julgar diretamente esses carimbos. (`app/chamados/models.py:2601`, `app/chamados/juizes.py:1137`, `app/chamados/services/materializacao.py:1361`, `app/chamados/models.py:3416`, `app/chamados/veredito_lavrado.py:180`, `app/chamados/services/tarefa_do_dia.py:121`)

   Os consumidores do dict são resposta, resposta automática, validação, devolução, ressurreição, DRY do backfill e carga da tarefa. Os escritores encaminham as linhas à sequência; a tarefa usa `pergunta_id/ato_em`. (`app/chamados/services/disputa_emissao.py:1344`, `app/chamados/services/disputa_emissao.py:1514`, `app/chamados/services/validacao.py:133`, `app/chamados/services/validacao.py:198`, `app/chamados/models.py:1513`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:45`, `app/chamados/services/tarefa_do_dia.py:316`)

   **Resposta às 19h, validação às 22h, `agora=21h`:**

   - Com linha correspondente de resposta: validação futura não entra no passivo nem nas decisões; resultado `aguarda_validacao/validar`. (`app/chamados/models.py:3420`, `app/chamados/services/tarefa_do_dia.py:310`, `app/chamados/services/tarefa_do_dia.py:317`, `app/chamados/services/tarefa_do_dia.py:129`)
   - Sem linha da resposta: o passivo das 19h permanece; resultado `passivo_sem_linha/nao_sei`. (`app/chamados/models.py:3416`, `app/chamados/services/tarefa_do_dia.py:121`)
   - Consultar a sequência inteira para correspondência não introduz ato futuro nesse caso: uma linha correspondente ao carimbo anterior tem necessariamente o mesmo instante anterior. (`app/chamados/models.py:3412`, `app/chamados/models.py:3416`)

   **Ressalva semântica preexistente:** `carimbar` pode usar o instante de fechamento para preencher resposta/validação. Portanto, esses campos não garantem recuperar o instante original da fala ou validação humana. Isso não foi introduzido pelo novo `<=`. (`app/chamados/models.py:2667`, `app/chamados/models.py:2691`, `app/chamados/models.py:2694`)

4. **Classe: documentação do horizonte — melhorou, mas não está inteiramente exata.**

   A carga agora declara corretamente os estados atuais anteriormente omitidos: identidade/âncora, ausência e absorção, fila, cadastro, vínculo e catálogo. Os cortes descritos de decisão, passivo, lavra e criação da Batida correspondem aos filtros. (`app/chamados/services/tarefa_do_dia.py:278`, `app/chamados/services/tarefa_do_dia.py:310`, `app/chamados/services/tarefa_do_dia.py:316`, `app/chamados/services/tarefa_do_dia.py:328`)

   Ainda sobra a abertura do selo, **“corta TODO fato que a tarefa lê”**, contrariada pelos próprios limites seguintes. A enumeração de queries também omite a consulta de cadastro e a consulta condicional do catálogo. (`app/chamados/tests/test_e2_tarefa_do_dia.py:702`, `app/chamados/tests/test_e2_tarefa_do_dia.py:709`, `app/chamados/services/tarefa_do_dia.py:276`, `app/chamados/services/tarefa_do_dia.py:295`, `app/ponto/catalogo/ausencias.py:278`)

   Declarar catálogo atual não resolve a promessa de relógio somente pela borda: o caminho transitivo continua lendo `monotonic`. (`app/chamados/services/tarefa_do_dia.py:284`, `app/ponto/catalogo/ausencias.py:275`)

5. **Classes de reprovação — consolidação do delta.**

   | Classe | Procedência |
   |---|---|
   | **Segundo juiz** | **Não achei novo no delta.** Há corte/registro para a tarefa e coexistência declarada até E3. (`app/core/juizes.py:150`, `app/docs/CORTES.md:424`) |
   | **Segundo escritor fora da porta** | **Não achei introdução no delta.** A lacuna via/ato está na API existente; ressurreição também usa a porta existente de descarimbo. (`app/chamados/models.py:2727`, `app/chamados/models.py:1515`) |
   | **Relógio fora da borda** | **Procedente, permanece:** tarefa → cobertura → conjunto vivo → catálogo → `monotonic`, com atualização de cache. O AST cobre dois módulos e o veneno intercepta `timezone.now`, deixando esse caminho fora. (`app/chamados/services/tarefa_do_dia.py:366`, `app/ponto/turnos.py:2762`, `app/ponto/catalogo/ausencias.py:433`, `app/ponto/catalogo/ausencias.py:275`, `app/chamados/tests/test_e2_tarefa_do_dia.py:680`, `app/chamados/tests/test_e2_tarefa_do_dia.py:991`) |
   | **Derivação paralela de fonte única** | **Não achei nos caminhos alterados.** Passivo usa a autoridade E1; efeito mantém `batidas_apuraveis`; teto e posição mantêm os corpos compartilhados. (`app/chamados/services/tarefa_do_dia.py:316`, `app/chamados/services/tarefa_do_dia.py:328`, `app/ponto/turnos.py:3059`, `app/chamados/services/tarefa_do_dia.py:55`) |
   | **RED sem mordida** | **Não procede para os dois testes novos, em memória.** Sem o delta, às 21h o primeiro recebe `sem_pergunta` e o segundo `passivo_sem_linha`, contrariando suas asserções; o atual recebe os resultados exigidos. Django integrado: **[nao verificado]**. (`app/chamados/tests/test_e2_tarefa_do_dia.py:761`, `app/chamados/tests/test_e2_tarefa_do_dia.py:774`) |
   | **Campo com dois significados** | **Não achei significado novo introduzido pelo corte.** Permanecem as ressalvas de carimbo de fechamento e `materializada_em` preservado ao trocar via; `ato_em` representa o ato, não o nascimento físico da linha. (`app/chamados/models.py:2691`, `app/chamados/models.py:2738`, `app/chamados/services/disputa_emissao.py:1540`) |
   | **Queries crescendo com N** | **Não achei SELECT por pergunta nos ramos novos.** Perguntas/decisões vêm relacionadas e a correspondência usa consulta de lote. O teste inclui validações, mas não ausências; catálogo depende de TTL. Constância absoluta e SQL medido: **[nao verificado]**. (`app/chamados/services/tarefa_do_dia.py:302`, `app/chamados/services/tarefa_do_dia.py:311`, `app/chamados/models.py:3412`, `app/chamados/tests/test_e2_tarefa_do_dia.py:893`, `app/ponto/catalogo/ausencias.py:276`) |

## 07/11 12:38 · sessao 67c07a42d4d7 · - · - · origem=Ronald (tmux)

**Há caminho produtivo que permite diferença no presente pelo `any`, sem depender apenas da porta genérica. Ocorrência em produção [nao verificado].** (`app/ponto/services/cartorio.py:1633`, `app/chamados/services/tarefa_do_dia.py:210`)

A sequência alcançável por leitura é:

1. `remover_motivo` grava retratação e via `motivo_retratado`; a pergunta sem resposta fica conservada e carimbada. (`app/chamados/models.py:1987`, `app/chamados/models.py:1989`, `app/chamados/models.py:1994`)
2. O cartório carrega **todas** as perguntas que resolvem o chamado. Se não há fio vivo nem resposta com texto e o conjunto não é inteiramente duplicata, chama `ressuscitar_perguntas`; não há guarda de motivo retratado nesse bloco. (`app/ponto/services/cartorio.py:1605`, `app/ponto/services/cartorio.py:1606`, `app/ponto/services/cartorio.py:1608`, `app/ponto/services/cartorio.py:1610`, `app/ponto/services/cartorio.py:1633`)
3. O universo da ressurreição filtra carimbo presente e resposta vazia, sem excluir essa via. Na cópia usada para consultar C6, **descarimba antes de julgar**, removendo o insumo que faria a via matar a pergunta. Com célula acusando e marco sem fato, C6 pode devolvê-la viva. (`app/chamados/models.py:1440`, `app/chamados/models.py:1443`, `app/chamados/models.py:1461`, `app/chamados/models.py:1464`, `app/chamados/juizes.py:1183`, `app/chamados/juizes.py:1186`)
4. O lote limpa a via real; a decisão imutável de retratação permanece. A mesma porta reabre a disputa fechada. (`app/chamados/models.py:1515`, `app/chamados/models.py:2801`, `app/chamados/models.py:1542`, `app/chamados/models.py:1543`)
5. A API aceita a pergunta pela vitalidade atual e chama o escritor da resposta; ele grava uma decisão `resposta` posterior para a mesma pergunta. (`app/api/views.py:2644`, `app/api/views.py:2647`, `app/api/views.py:2659`, `app/chamados/services/disputa_emissao.py:1369`)

Assim, para um estado com `via=''`, sequência `[retratacao, resposta]`, `agora` posterior aos dois atos e marco exigível sem outra pergunta escolhida:

- O código anterior do delta não a excluía pela via nem pela última decisão; chegaria a `aguarda_validacao/validar`. (**Anexo `e2_b3v3_delta.diff`**, contexto da alteração em `_tarefa_pura`; ramo preservado em `app/chamados/services/tarefa_do_dia.py:129`.)
- O código atual a exclui por existir **qualquer** retratação aplicável, chegando a `sem_pergunta/perguntar`. (`app/chamados/services/tarefa_do_dia.py:210`, `app/chamados/services/tarefa_do_dia.py:256`, `app/chamados/services/tarefa_do_dia.py:258`)

**Classe: mudança de comportamento no presente; procedente por caminho de código.** Isso não estabelece que o comportamento anterior era correto: a regra documentada diz que a retratada saiu da identidade e o fato que volta ganha pergunta nova. A ressurreição acima pode estar apagando indevidamente essa identidade antes de chamar C6. (`app/chamados/models.py:2320`, `app/chamados/models.py:2323`, `app/chamados/models.py:1461`)

As portas específicas `reabrir_fantasma`, `reabrir_por_competencia` e `reabrir_por_resposta` têm guardas que não contemplam motivo retratado; `desfazer_carimbo_sem_lastro` exclui todas as vias de identidade. O caminho encontrado é a ressurreição por chamado/cartório. (`app/chamados/models.py:2832`, `app/chamados/models.py:2849`, `app/chamados/models.py:2856`, `app/chamados/services/relavra_via.py:179`)

Na busca adicional em Python **fora de `app`**, excluídos testes e logs, não achei escritor das duas vias nem `RETRATACAO`; essa conclusão limita-se ao texto pesquisado. **[nao verificado]** [nao verificado]

## 07/11 12:38 · sessao 3c3407e2311a · - · - · origem=Ronald (tmux)

**Corte do passivo: não achei defeito novo no caso resposta antes/validação depois.** Ele compara inclusivamente o instante que a autoridade devolve; a sequência também usa comparação inclusiva sobre `ato_em`. (`app/chamados/services/tarefa_do_dia.py:310`, `app/chamados/services/tarefa_do_dia.py:316`, `app/chamados/models.py:3418`, `app/chamados/models.py:3422`)

**Escritores encontrados em código de produção**, por `rg` e AST em `app/**/*.py`, excluídos testes e migrações: [nao verificado]

| Campo | Escritor |
|---|---|
| `respondida_em` | Resposta normal: `_agora = _tz.now()` e atribuição; decisão recebe exatamente `_agora`. (`app/chamados/services/disputa_emissao.py:1340`, `app/chamados/services/disputa_emissao.py:1346`, `app/chamados/services/disputa_emissao.py:1369`) |
| `respondida_em` | Resposta automática: recebe `instante`; decisão recebe o mesmo valor. (`app/chamados/services/disputa_emissao.py:1516`, `app/chamados/services/disputa_emissao.py:1519`) |
| `respondida_em` | Recuperação pela trilha: recebe o instante externo da nota; decisão recebe o mesmo valor. (`app/chamados/services/disputa_emissao.py:1528`, `app/chamados/services/disputa_emissao.py:1537`, `app/chamados/services/disputa_emissao.py:1540`) |
| `validada_em` | Validação: `timezone.now()` e decisão com exatamente `perg.validada_em`. (`app/chamados/services/validacao.py:134`, `app/chamados/services/validacao.py:139`) |
| Ambos | `carimbar`: preenche os campos vazios, havendo texto, com `quando`; `quando` ausente vira `_tz.now()`. (`app/chamados/models.py:2683`, `app/chamados/models.py:2689`, `app/chamados/models.py:2691`, `app/chamados/models.py:2694`) |
| Ambos | Reabertura zera os campos por `campos`, persistidos por `descarimbar_em_lote` ou `qs.update`. (`app/chamados/services/disputa_emissao.py:1462`, `app/chamados/services/disputa_emissao.py:1493`, `app/chamados/services/disputa_emissao.py:1495`) |
| Ambos | Ressuscitação zera pela porta de lote. (`app/chamados/models.py:1515`, `app/chamados/models.py:2801`) |
| `validada_em` | Nova resposta zera a validação anterior; devolução também zera e registra ato. (`app/chamados/services/disputa_emissao.py:1358`, `app/chamados/services/disputa_emissao.py:1366`, `app/chamados/services/validacao.py:199`, `app/chamados/services/validacao.py:202`, `app/chamados/services/validacao.py:205`) |
| Ambos | Desfazer carimbo em memória restaura a fotografia; a simulação de ressuscitação zera somente uma cópia. (`app/chamados/models.py:2722`, `app/chamados/models.py:2723`, `app/chamados/models.py:1449`, `app/chamados/models.py:1462`, `app/chamados/models.py:1463`) |

A migração encontrada apenas declara os dois `DateTimeField`, sem preenchê-los. (`app/chamados/migrations/0014_perguntadisputa.py:25`, `app/chamados/migrations/0014_perguntadisputa.py:26`)

**Há uma ressalva de significado preexistente:** `carimbar` pode preencher `respondida_em/validada_em` com o instante do fechamento, e não recuperar o instante original de fala/validação humana. Seus chamadores incluem absorção, materialização, encerramento por fato e encerramento sem credor. Isso não é introduzido pelo delta do passivo. (`app/chamados/models.py:2667`, `app/chamados/models.py:2691`, `app/chamados/models.py:2694`, `app/chamados/services/materializacao.py:558`, `app/chamados/services/materializacao.py:1268`, `app/chamados/services/disputa_emissao.py:1707`, `app/chamados/services/disputa_emissao.py:1781`)

Na absorção, especificamente, o chamador registra `ABSORCAO`, enquanto `carimbar(resposta='validar')` pode preencher os dois carimbos. A autoridade de passivo procura correspondência por `RESPOSTA/VALIDACAO`, portanto esses carimbos podem aparecer como passivo; esse comportamento já decorre do código anterior ao delta. (`app/chamados/services/materializacao.py:558`, `app/chamados/services/materializacao.py:560`, `app/chamados/models.py:3412`, `app/chamados/models.py:3416`, `app/chamados/models.py:3420`)

**Quem escreve/lê o dict:** `linhas_da_projecao_sem_linha` lê a projeção em memória e uma query da sequência, exigindo correspondência exata `(pergunta_id, ato, ato_em)`. Devolve resposta com `ato_em=respondida_em` e validação com `ato_em=validada_em`. (`app/chamados/models.py:3407`, `app/chamados/models.py:3412`, `app/chamados/models.py:3418`, `app/chamados/models.py:3422`)

Os consumidores de produção encontrados são: resposta normal, resposta automática, validação, devolução, ressuscitação, o DRY do backfill e a carga da tarefa. Os cinco primeiros encaminham linhas a `registrar`; o backfill classifica por pergunta/ato; a tarefa usa `pergunta_id` e `ato_em`. (`app/chamados/services/disputa_emissao.py:1344`, `app/chamados/services/disputa_emissao.py:1369`, `app/chamados/services/disputa_emissao.py:1514`, `app/chamados/services/disputa_emissao.py:1519`, `app/chamados/services/validacao.py:133`, `app/chamados/services/validacao.py:139`, `app/chamados/services/validacao.py:198`, `app/chamados/services/validacao.py:205`, `app/chamados/models.py:1513`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:45`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:58`, `app/chamados/services/tarefa_do_dia.py:316`)

**Caso resposta19, validação22, agora21**, executado em memória com o corpo AST exato da autoridade e ORM falso, sem banco:

- Sem nenhuma linha: a autoridade devolve resposta19 e validação22; o corte mantém o passivo da resposta19. A tarefa responde `passivo_sem_linha`, não inventa validação. (`app/chamados/models.py:3416`, `app/chamados/models.py:3420`, `app/chamados/services/tarefa_do_dia.py:316`, `app/chamados/services/tarefa_do_dia.py:121`)
- Com linha correspondente de resposta19: só validação22 é devolvida e descartada pelo corte; a sequência cortada contém resposta19, logo `aguarda_validacao/validar`. (`app/chamados/models.py:3416`, `app/chamados/models.py:3420`, `app/chamados/services/tarefa_do_dia.py:310`, `app/chamados/services/tarefa_do_dia.py:316`, `app/chamados/services/tarefa_do_dia.py:126`, `app/chamados/services/tarefa_do_dia.py:129`)
- Com ambas as linhas: a autoridade devolve vazio; a validação22 sai pelo filtro SQL e novamente vale resposta19. (`app/chamados/models.py:3412`, `app/chamados/models.py:3416`, `app/chamados/models.py:3420`, `app/chamados/services/tarefa_do_dia.py:310`)

Os três campos são instantes (`DateTimeField`), os escritores normais compartilham o valor entre projeção e sequência e a configuração usa timezone-aware com São Paulo. Isso sustenta a equivalência temporal do `<=` Python e do `__lte`; integridade/fuso de cada registro produtivo e execução SQL/Django permanecem **[nao verificado]**. (`app/chamados/models.py:2257`, `app/chamados/models.py:2258`, `app/chamados/models.py:3358`, `app/chamados/services/disputa_emissao.py:1369`, `app/chamados/services/validacao.py:139`, `app/config/settings/base.py:93`, `app/config/settings/base.py:95`)

## 07/11 12:37 · sessao cef5096f5ef1 · - · - · origem=Ronald (tmux)

1. **RED sem mordida — não procede para os dois testes novos, por execução dos corpos em memória.** Reconstituí o ramo anterior a partir do delta fornecido, sem Django ou banco. Antes do delta, o teste de retratação recebe `sem_pergunta/perguntar` às 21h; o atual recebe `aguarda_validacao/validar`. Antes do delta, o teste do passivo recebe `passivo_sem_linha/nao_sei` às 21h; o atual recebe `aguarda_resposta/responder`. Os resultados anteriores contradizem as primeiras asserções dos respectivos testes. Ambos preservam os resultados posteriores às 23h. (`app/chamados/services/tarefa_do_dia.py:208`, `app/chamados/services/tarefa_do_dia.py:210`, `app/chamados/services/tarefa_do_dia.py:121`, `app/chamados/services/tarefa_do_dia.py:316`, `app/chamados/tests/test_e2_tarefa_do_dia.py:761`, `app/chamados/tests/test_e2_tarefa_do_dia.py:774`) Execução integrada Django e RED/GREEN históricos: **[nao verificado]**.

2. **Docstrings — limites principais agora declarados; ainda sobra uma formulação absoluta.** A carga distingue fatos cortados de estado atual, incluindo ausência/absorção, fila, cadastro, vínculo, âncora, identidade e catálogo. Isso corresponde aos caminhos lidos. (`app/chamados/services/tarefa_do_dia.py:278`, `app/chamados/services/tarefa_do_dia.py:283`, `app/chamados/services/tarefa_do_dia.py:295`, `app/chamados/services/tarefa_do_dia.py:310`, `app/chamados/services/tarefa_do_dia.py:328`, `app/chamados/services/tarefa_do_dia.py:343`, `app/chamados/services/tarefa_do_dia.py:366`) O selo ainda abre dizendo “corta TODO fato”, embora depois explicite os limites atuais; a frase inicial, isoladamente, excede o código. (`app/chamados/tests/test_e2_tarefa_do_dia.py:702`, `app/chamados/tests/test_e2_tarefa_do_dia.py:709`)

   A enumeração de queries da carga também não é completa: falta mencionar a consulta de cadastro e a consulta condicional do catálogo. (`app/chamados/services/tarefa_do_dia.py:276`, `app/chamados/services/tarefa_do_dia.py:295`, `app/ponto/catalogo/ausencias.py:278`)

3. **Relógio transitivo fora da borda — procedente, permanece.** `tarefa → cobertura → TIPOS_EM_ABERTO → vigente()` continua alcançando `time.monotonic()` e atualização do cache do catálogo. Declarar catálogo atual não remove essa leitura de relógio. (`app/chamados/services/tarefa_do_dia.py:366`, `app/ponto/turnos.py:2762`, `app/ponto/turnos.py:2774`, `app/ponto/models.py:689`, `app/ponto/catalogo/ausencias.py:403`, `app/ponto/catalogo/ausencias.py:433`, `app/ponto/catalogo/ausencias.py:275`, `app/ponto/catalogo/ausencias.py:279`) O selo AST limita-se aos módulos da tarefa e da lavra; seu veneno dinâmico intercepta `timezone.now`, deixando esse helper fora. (`app/chamados/tests/test_e2_tarefa_do_dia.py:929`, `app/chamados/tests/test_e2_tarefa_do_dia.py:991`, `app/chamados/tests/test_e2_tarefa_do_dia.py:678`)

4. **Segundo juiz, segundo escritor e derivação paralela — não achei no delta examinado.** As mudanças são filtros de leitura, com passivo pela autoridade E1; o efeito mantém `batidas_apuraveis`, o teto mantém o corpo do cartório e a posição mantém a função compartilhada. (`app/chamados/services/tarefa_do_dia.py:208`, `app/chamados/services/tarefa_do_dia.py:316`, `app/chamados/models.py:3399`, `app/chamados/services/tarefa_do_dia.py:328`, `app/ponto/turnos.py:3059`, `app/ponto/services/cartorio.py:858`, `app/chamados/services/tarefa_do_dia.py:55`, `app/chamados/services/declaracao_texto.py:75`) A existência do novo juiz e a coexistência provisória dos cinco respondedores têm corte explícito. (`app/core/juizes.py:150`, `app/docs/CORTES.md:424`)

5. **Queries crescendo com N — não achei SELECT por pergunta nos ramos novos; constância absoluta não certificada.** Perguntas carregam disputa, decisões carregam ausência e a correspondência do passivo usa consulta de lote. (`app/chamados/services/tarefa_do_dia.py:302`, `app/chamados/services/tarefa_do_dia.py:311`, `app/chamados/models.py:3412`) O teste compara lotes e inclui validações, mas não exercita ausências. (`app/chamados/tests/test_e2_tarefa_do_dia.py:893`, `app/chamados/tests/test_e2_tarefa_do_dia.py:904`, `app/chamados/tests/test_e2_tarefa_do_dia.py:918`) O corpo da cobertura consulta o conjunto vivo por ausência, cujo acesso ao banco depende do TTL; não medi SQL nem confirmei crescimento real. (`app/ponto/turnos.py:2774`, `app/ponto/catalogo/ausencias.py:276`, `app/ponto/catalogo/ausencias.py:278`) Medição produtiva: **[nao verificado]**.

A cópia não contém repositório Git utilizável; a reconstrução anterior usou exclusivamente o delta fornecido. Identidade histórica dos commits: **[nao verificado]**. [nao verificado]

## 07/11 12:37 · sessao 67c07a42d4d7 · - · - · origem=Ronald (tmux)

**Classe: correspondência via/ato; produção sem linha não demonstrada.** O único chamador produtivo explícito que achei para `VIA_MOTIVO_RETRATADO` foi `DisputaSupervisao.remover_motivo`: registra `RETRATACAO` e carimba a via no mesmo `transaction.atomic`. A busca percorreu Python da cópia, excluindo testes, migrações e logs; ocorrência produtiva de via sem linha **[nao verificado]**. (`app/chamados/models.py:1986`, `app/chamados/models.py:1989`, `app/chamados/models.py:1994`)

**A porta genérica permite a combinação sem linha, e existe testemunha de teste.** `carimbar_identidade` valida a participação na lista de vias e salva o carimbo, sem registrar decisão. `test_RED_retratada_nao_esta_no_dominio_do_lote` chama diretamente essa porta com `VIA_MOTIVO_RETRATADO`, sem criar a retratação. Isso prova possibilidade pela API e uso em teste; não prova segundo escritor produtivo. (`app/chamados/models.py:2727`, `app/chamados/models.py:2736`, `app/chamados/models.py:2746`, `app/chamados/tests/test_e1_volta2.py:128`, `app/chamados/tests/test_e1_volta2.py:131`)

Para essa pergunta **sem qualquer linha `retratacao`**, a via `motivo_retratado` não a exclui mais em nenhum `agora`: o filtro atual exclui apenas `VIA_DUPLICATA` pela via e procura retratação na sequência cortada. Se ela for a pergunta escolhida para um marco vencido/apagado e passar pelas guardas anteriores, seguirá `_pela_decisao`: sem decisões/passivo, `aguarda_resposta/responder`; com resposta, `aguarda_validacao/validar`; com passivo, `passivo_sem_linha/nao_sei`. A aplicação concreta depende das demais guardas e perguntas concorrentes. (`app/chamados/services/tarefa_do_dia.py:208`, `app/chamados/services/tarefa_do_dia.py:210`, `app/chamados/services/tarefa_do_dia.py:260`, `app/chamados/services/tarefa_do_dia.py:121`, `app/chamados/services/tarefa_do_dia.py:123`, `app/chamados/services/tarefa_do_dia.py:129`)

**Migração/backfill: não achei criação dessa inconsistência.** A 0051 adiciona a escolha `motivo_retratado`; as decisões anteriores recebem `ato='reabertura'`. O preenchimento de dados da migração altera colaborador/instância, não essa via. `backfill_decisoes_pergunta` é DRY e calcula somente respostas/validações faltantes. (`app/chamados/migrations/0051_decisao_pergunta.py:22`, `app/chamados/migrations/0051_decisao_pergunta.py:49`, `app/chamados/migrations/0051_decisao_pergunta.py:61`, `app/chamados/migrations/0051_decisao_pergunta.py:111`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:3`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:7`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:45`)

**Escritor de duplicata:** `fio.retratar_duplicata → carimbar_identidade → _escrever_carimbo`; o helper não cria decisão `retratacao`. Os dois chamadores produtivos explícitos achados são `dedup_perguntas_fato` e `migrar_resolvedoras_vivas`. `manter_carimbo=True` pode preservar instante/autor antigos enquanto troca a via. (`app/chamados/services/fio.py:939`, `app/chamados/services/fio.py:952`, `app/chamados/models.py:2658`, `app/chamados/models.py:2738`, `app/chamados/management/commands/dedup_perguntas_fato.py:88`, `app/chamados/services/fio.py:1004`)

**Escritor de decisão `retratacao`:** achei o chamador produtivo `remover_motivo`, que passa por `DecisaoPergunta.registrar`, único corpo que realiza o `bulk_create` da sequência. O teste novo escreve uma retratação diretamente por `registrar` e muda a via por `update`, como fixture; esse segundo caminho é de teste. (`app/chamados/models.py:1989`, `app/chamados/models.py:3430`, `app/chamados/tests/test_e2_tarefa_do_dia.py:757`, `app/chamados/tests/test_e2_tarefa_do_dia.py:759`)

**Leitores relevantes das vias:**

| Leitor | Uso |
|---|---|
| Domínio do fato | Exclui ambas por `VIAS_FORA_DO_FATO`. (`app/chamados/models.py:2450`, `app/chamados/models.py:2467`) |
| Emissão e censo de colisões | Excluem ambas pela mesma lista. (`app/chamados/services/disputa_emissao.py:323`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:74`) |
| `q_vivas` e juiz de vida | Leem via/carimbo atuais; motivo retratado vence a resposta. (`app/chamados/models.py:2888`, `app/chamados/juizes.py:1147`) |
| Juiz do lastro | Lê via+carimbo e conjunto de identidade. (`app/chamados/juizes.py:648`, `app/chamados/juizes.py:658`) |
| Fábrica da célula e cartório | Têm leitores específicos de duplicata. (`app/chamados/services/fabrica_da_celula.py:119`, `app/ponto/services/cartorio.py:1610`) |
| Tarefa nova | Exclui duplicata pela via atual e retratação por qualquer decisão aplicável. (`app/chamados/services/tarefa_do_dia.py:208`, `app/chamados/services/tarefa_do_dia.py:210`) |

`respostas_anteriores` percorre a sequência, mas só trata `resposta` e atos com foto (`reabertura/recusa`); não transforma retratação em resposta anterior. (`app/chamados/models.py:3354`, `app/chamados/models.py:2993`, `app/chamados/models.py:2998`)

_2732 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
