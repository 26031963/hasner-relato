# CODEX-ESTADO

> Ultima resposta processada: **05/11/2026 15:39** · publicado em **05/11/2026 15:39** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 05/11 15:39 · sessao e2d128a80f6c · H12H06 · verificador · origem=Code

**Os achados 2 e 4 estão fechados; 1 e 3 estão fechados parcialmente.** Permanecem contraexemplos de reversão sobre estado diferente e de entrega concorrente. (`app/ponto/management/commands/rederivar_falta_declarada.py:154`, `app/ponto/management/commands/rederivar_falta_declarada.py:224`, `app/ponto/management/commands/rederivar_falta_declarada.py:185`, `app/ponto/services/fechamento_lista.py:109`.)

Execução dos testes, RED/GREEN, aplicação em produção, igualdade do hash da 09 e ordem efetiva dos pousos: **[nao verificado]**. Os contraexemplos abaixo são deduções estáticas; ocorrência em produção: **[nao verificado]**. [nao verificado]

**1. Quem escreve e quem lê**

| Campo/função | Quem escreve ou produz | Quem lê |
|---|---|---|
| `Ausencia.minutos` | Default no modelo; criação recebe o valor; edição atribui; `Ausencia.save` deriva falta aprovada sem minutos, somando o juiz diário; reversão repõe o inteiro. A mudança de status inclui minutos na persistência. (`app/ponto/models.py:755`, `app/ponto/services/ausencia.py:343`, `app/ponto/services/ausencia.py:507`, `app/ponto/models.py:877`, `app/ponto/models.py:885`, `app/ponto/services/ausencia.py:531`, `app/ponto/services/ausencia.py:838`.) | Comando: seleção, censo, arquivo, conferência anterior/posterior e reversão. Financeiro: rateio, créditos e abono. Parcialidade: `cobre_parte_do_dia`. (`app/ponto/management/commands/rederivar_falta_declarada.py:72`, `app/ponto/management/commands/rederivar_falta_declarada.py:158`, `app/ponto/management/commands/rederivar_falta_declarada.py:167`, `app/ponto/management/commands/rederivar_falta_declarada.py:177`, `app/ponto/management/commands/rederivar_falta_declarada.py:226`, `app/folha/export.py:121`, `app/ponto/services/credito_parcial.py:24`, `app/ponto/services/fechamento.py:665`, `app/ponto/services/efeito_ausencia.py:44`, `app/ponto/turnos.py:2600`.) |
| Arquivo de reversão | `handle`, antes da transação e das edições, por `json.dump` com abertura exclusiva `x`. (`app/ponto/management/commands/rederivar_falta_declarada.py:157`, `app/ponto/management/commands/rederivar_falta_declarada.py:165`.) | `_reverter` carrega JSON, valida e chama `repor_minutos_de_reversao`; o serviço registra o caminho na descrição da trilha. (`app/ponto/management/commands/rederivar_falta_declarada.py:211`, `app/ponto/management/commands/rederivar_falta_declarada.py:219`, `app/ponto/management/commands/rederivar_falta_declarada.py:234`, `app/ponto/services/ausencia.py:534`.) |
| Chave `'rescisao'` da linha | `montar_resumos`, chamando `sai_pela_rescisao` com o fim da janela da empresa do colaborador. (`app/ponto/services/fechamento_lista.py:103`, `app/ponto/services/fechamento_lista.py:109`.) | Contador, atributo `data-resc`, borda e badge; teste novo lê o dict. (`app/ponto/views.py:1215`, `app/templates/ponto/_fech_linha.html:2`, `app/templates/ponto/_fech_linha.html:4`, `app/ponto/tests/test_h06_reversao_guardas.py:176`.) |
| `n_rescisao` | `fechamento_mensal`, somando as linhas com `'rescisao'` verdadeira. (`app/ponto/views.py:1215`.) | Pílula e número no template; o filtro JavaScript usa o `data-resc` das linhas. (`app/templates/ponto/fechamento.html:65`, `app/templates/ponto/fechamento.html:117`.) |
| `sai_pela_rescisao` | Produz `data_demissao != None and data_demissao <= fim_janela`. (`app/folha/export.py:54`.) | `classificar_export` e `montar_resumos`. (`app/folha/export.py:858`, `app/ponto/services/fechamento_lista.py:109`.) |

O nome efetivo é `logs/h06_falta/reversao_<emp>_<MMAAAA>_<YYYYMMDD_HHMMSS>_<pid>_<uuid8>.json`. O formato é uma lista de objetos com `pk`, `empresa_id`, `colaborador_id`, `data` ISO, `minutos_antes` e `grade`. A docstring ainda descreve o nome antigo, sem PID/UUID. (`app/ponto/management/commands/rederivar_falta_declarada.py:41`, `app/ponto/management/commands/rederivar_falta_declarada.py:154`, `app/ponto/management/commands/rederivar_falta_declarada.py:158`, `app/ponto/management/commands/rederivar_falta_declarada.py:22`.)

No alcance dos chamadores, os dois sítios de materialização omitem minutos; lançamento/edição humana, API, regularização, abandono e férias continuam entrando pela porta. Abandono também fornece `chamado_origem`, portanto esse vínculo sozinho não identifica exclusivamente declaração de disputa. (`app/chamados/services/materializacao.py:484`, `app/chamados/services/materializacao.py:520`, `app/ponto/views.py:2413`, `app/ponto/views.py:2534`, `app/ponto/views.py:2792`, `app/api/views.py:1820`, `app/ponto/services/regularizacao.py:119`, `app/ponto/management/commands/marcar_abandono_provisorio.py:76`, `app/ponto/services/ausencia.py:1206`.)

Outros leitores de minutos incluem horas, cartão, API, admin, painel, serializações e relatório de atestados. (`app/ponto/models.py:866`, `app/ponto/services/dia_decidido.py:488`, `app/ponto/services/dia_decidido.py:512`, `app/api/views.py:1878`, `app/ponto/admin.py:44`, `app/templates/ponto/partials/painel_ausencia.html:125`, `app/ponto/views.py:2347`, `app/ponto/views.py:2768`, `app/ponto/services/atestados_lote.py:128`.)

As autoridades usadas pelos novos caminhos permanecem: janela em `janela_fechamento`/`janela_atual`, minutos em `minutos_previstos_do_dia` e entrega em `competencia_entregue`. A foto lê campos de `FechamentoMensal`, sem comparar ausência ou conteúdo do TXT. (`app/ponto/janelas.py:54`, `app/ponto/janelas.py:60`, `app/ponto/management/commands/rederivar_falta_declarada.py:80`, `app/ponto/management/commands/rederivar_falta_declarada.py:63`, `app/ponto/management/commands/carimbo_gravado.py:36`, `app/ponto/management/commands/carimbo_gravado.py:53`.)

**2. Fechamento dos achados 1–4**

| Achado | Estado e evidência |
|---|---|
| **1 — Reversão sem comprovação do ato/entrega** | **Fechado parcialmente.** Agora verifica existência, colaborador, data, minutos esperados e entrega. Porém não verifica tipo, status, range nem correspondência entre empresa atual e empresa do arquivo. (`app/ponto/management/commands/rederivar_falta_declarada.py:219`, `app/ponto/management/commands/rederivar_falta_declarada.py:224`, `app/ponto/management/commands/rederivar_falta_declarada.py:228`.) |
| **2 — Arquivo sobrescrevível** | **Fechado.** Nome com PID/UUID, abertura `x` e retorno sem arquivo quando não há alvo. (`app/ponto/management/commands/rederivar_falta_declarada.py:145`, `app/ponto/management/commands/rederivar_falta_declarada.py:154`, `app/ponto/management/commands/rederivar_falta_declarada.py:157`.) |
| **3 — Concorrência/ranges/ausência de atomicidade** | **Fechado parcialmente.** Há `atomic`, leitura travada, guarda das competências atravessadas e rollback por divergência da foto. A entrega continua sem trava comum com o comando. (`app/ponto/management/commands/rederivar_falta_declarada.py:140`, `app/ponto/management/commands/rederivar_falta_declarada.py:165`, `app/ponto/management/commands/rederivar_falta_declarada.py:185`, `app/ponto/management/commands/rederivar_falta_declarada.py:193`, `app/folha/export.py:581`, `app/folha/models.py:182`.) |
| **4 — Leitor H12 divergente** | **Fechado.** Gate e linha compartilham o predicado; contador, filtro e badge leem a linha. (`app/folha/export.py:858`, `app/ponto/services/fechamento_lista.py:109`, `app/ponto/views.py:1215`, `app/templates/ponto/_fech_linha.html:2`, `app/templates/ponto/_fech_linha.html:4`.) |

**Contraexemplo do 1, sem concorrência — certeza alta:** falta720 de 07/10, grade420 → aplicar →420; depois o DP edita o mesmo registro para **atestado420**, mantendo colaborador/data. A edição aceita atestado configurado como parcial e preserva a aprovação. A reversão encontra os mesmos colaborador/data/minutos420 e escreve **720 no atestado**, desfazendo parte da decisão posterior do DP. (`app/ponto/migrations/0063_atestado_aceita_horas.py:20`, `app/ponto/services/ausencia.py:480`, `app/ponto/services/ausencia.py:497`, `app/ponto/services/ausencia.py:507`, `app/ponto/services/ausencia.py:544`, `app/ponto/management/commands/rederivar_falta_declarada.py:224`, `app/ponto/services/ausencia.py:531`.)

**Contraexemplo de empresa — certeza alta:** aplicar na empresa A aberta; transferir o colaborador para B; entregar a competência em B, mantendo A aberta; reverter. A guarda consulta **A, retirada do JSON**, e restaura720 na ausência do colaborador atualmente em B. Não existe comparação com a empresa atual. (`app/colaboradores/services/colaborador.py:33`, `app/colaboradores/services/colaborador.py:55`, `app/ponto/management/commands/rederivar_falta_declarada.py:228`, `app/ponto/management/commands/rederivar_falta_declarada.py:234`.)

**Contraexemplo do 3 — certeza alta quanto à possibilidade estática:** competência10 aberta, falta720/grade420; comando altera dentro da transação e sua última consulta encontra “não entregue”; outra execução registra o TXT antes do commit do comando; comando confirma420. Não há trava compartilhada entre a consulta e `ExportacaoDominio.registrar`. Na reversão, a entrega pode ocorrer entre a consulta e a escrita. (`app/ponto/management/commands/rederivar_falta_declarada.py:174`, `app/ponto/management/commands/rederivar_falta_declarada.py:185`, `app/ponto/management/commands/rederivar_falta_declarada.py:228`, `app/ponto/management/commands/rederivar_falta_declarada.py:234`, `app/folha/export.py:581`, `app/folha/models.py:182`.)

**3. Classes**

| Classe | Resultado |
|---|---|
| **(a) Segundo juiz** | **Nenhum novo no delta H12:** há extração compartilhada do predicado usado pelo juiz registrado. No alcance permanece a divergência preexistente de parcialidade: corte480 na cobertura versus minutos positivos nos leitores financeiros, objeto do descarte5. (`app/core/juizes.py:288`, `app/folha/export.py:54`, `app/folha/export.py:858`, `app/ponto/turnos.py:2600`, `app/ponto/services/fechamento.py:665`, `app/ponto/services/fechamento.py:685`.) |
| **(b) Segundo escritor fora da porta** | **Nenhuma ocorrência identificada no delta.** O comando chama edição/reversão na porta declarada; a derivação permanece no modelo. (`app/ponto/management/commands/rederivar_falta_declarada.py:174`, `app/ponto/management/commands/rederivar_falta_declarada.py:234`, `app/ponto/tests/test_chokepoint_ausencia_gate.py:65`, `app/ponto/models.py:877`.) |
| **(c) Relógio fora da borda** | **Preexistente no alcance:** retenção, signal da ausência e cartório leem hoje/agora internamente. `_selo` novo está na borda de IO e só nomeia o arquivo. (`app/folha/export.py:1197`, `app/ponto/signals.py:122`, `app/ponto/services/cartorio.py:1965`, `app/ponto/services/cartorio.py:1986`, `app/ponto/management/commands/rederivar_falta_declarada.py:47`, `app/ponto/management/commands/rederivar_falta_declarada.py:155`.) |
| **(d) Derivação paralela** | **Nenhuma nova aritmética de janela, previsto diário ou rescisão.** Os novos caminhos chamam as autoridades. Permanece o rateio uniforme preexistente, diferente da distribuição diária da grade. (`app/ponto/management/commands/rederivar_falta_declarada.py:55`, `app/ponto/management/commands/rederivar_falta_declarada.py:80`, `app/ponto/services/fechamento_lista.py:104`, `app/ponto/services/fechamento_lista.py:109`, `app/ponto/models.py:885`, `app/folha/export.py:121`.) |
| **(f) Travas/atomic** | Há transação e `select_for_update` nas duas operações; o apply compara **somente minutos** com o censo. Não há serialização com entrega. A ordenação de `Ausencia` referencia colaborador, e os locks não restringem `of=('self',)`; SQL efetivo e ciclo de deadlock: **[nao verificado]**. (`app/ponto/management/commands/rederivar_falta_declarada.py:165`, `app/ponto/management/commands/rederivar_falta_declarada.py:167`, `app/ponto/management/commands/rederivar_falta_declarada.py:216`, `app/ponto/models.py:819`, `app/folha/models.py:182`.) |
| **(g) Pouso antes do pré-requisito** | Ordem efetiva: **[nao verificado]**. Os avais exigem sombra09/10/11, preservação da09 e pouso antes da exportação11; o dado não comprova execução. (`app/docs/PROMPTS.md:1860`, `app/docs/PROMPTS.md:1861`.) |

**(e) Mordida de cada teste novo:** resultados abaixo são análise contrafactual estática; execução: **[nao verificado]**. [nao verificado]

| Teste em `test_h06_reversao_guardas.py` | Passaria com a cura correspondente revertida? | [nao verificado]
|---|---|
| `reverter_recusa_competencia_entregue` | **Não**, retirada a guarda de entrega: deixaria de levantar o erro esperado. (`app/ponto/tests/test_h06_reversao_guardas.py:90`, `app/ponto/management/commands/rederivar_falta_declarada.py:228`.) |
| `reverter_recusa_estado_que_nao_e_o_do_ato` | **Não**, retirada a comparação com `grade`. (`app/ponto/tests/test_h06_reversao_guardas.py:99`, `app/ponto/management/commands/rederivar_falta_declarada.py:226`.) |
| `reverter_duas_vezes_nao_escreve_de_novo` | **Não**, retirada a comparação com `grade`: a segunda reversão seria aceita. (`app/ponto/tests/test_h06_reversao_guardas.py:108`, `app/ponto/management/commands/rederivar_falta_declarada.py:226`.) |
| `reverter_e_tudo_ou_nada` | **Sim, retirando somente `atomic`.** O teste injeta erro antes da chamada; toda validação antecede as escritas. Morde a validação antecipada, mas não prova rollback por falha na segunda gravação. (`app/ponto/tests/test_h06_reversao_guardas.py:120`, `app/ponto/management/commands/rederivar_falta_declarada.py:216`, `app/ponto/management/commands/rederivar_falta_declarada.py:219`, `app/ponto/management/commands/rederivar_falta_declarada.py:233`.) |
| `duas_aplicacoes_no_mesmo_segundo_preservam_a_primeira_reversao` | **Não**, restaurados nome antigo e modo`w`. **Sim**, trocando somente `x` por `w` e mantendo UUID/PID. Reversão literal que remova `_selo` falha no mock, antes da prova comportamental. (`app/ponto/tests/test_h06_reversao_guardas.py:130`, `app/ponto/tests/test_h06_reversao_guardas.py:132`, `app/ponto/tests/test_h06_reversao_guardas.py:137`, `app/ponto/management/commands/rederivar_falta_declarada.py:154`.) |
| `aplicar_sem_alvo_nao_grava_arquivo` | **Não**, retirado o retorno anterior à criação do arquivo. (`app/ponto/tests/test_h06_reversao_guardas.py:142`, `app/ponto/management/commands/rederivar_falta_declarada.py:145`.) |
| `range_que_atravessa_competencia_entregue_nao_e_tocado` | **Não**, retirada a cura completa. **Sim**, retirada isoladamente qualquer uma das duas guardas: a outra mantém a recusa; a posterior depende do rollback. (`app/ponto/tests/test_h06_reversao_guardas.py:146`, `app/ponto/management/commands/rederivar_falta_declarada.py:140`, `app/ponto/management/commands/rederivar_falta_declarada.py:185`.) |
| `linha_marca_rescisao_pelo_juiz_do_txt` | **Não**, restaurada a regra antiga na linha. **Sim**, revertidos somente contador/template: o teste inspeciona o dict, sem renderizar esses consumidores. (`app/ponto/tests/test_h06_reversao_guardas.py:173`, `app/ponto/tests/test_h06_reversao_guardas.py:176`, `app/ponto/views.py:1215`, `app/templates/ponto/_fech_linha.html:2`.) |

**4. Descartes 5–8: agravamento pelo delta**

| Descarte | Agravou? |
|---|---|
| **5 — Parcialidade480 versus financeiro** | **Não.** Os predicados divergentes permanecem; o delta não os altera. (`app/ponto/turnos.py:2600`, `app/ponto/services/fechamento.py:665`, `app/ponto/services/fechamento.py:685`.) |
| **6 — Rateio uniforme** | **Não.** Continua igual; a guarda nova restringe ranges entregues. (`app/folha/export.py:121`, `app/ponto/management/commands/rederivar_falta_declarada.py:140`.) |
| **7 — Trilha engolida/prova posterior** | **Não.** Foto divergente agora aborta dentro da transação. Auditoria continua best-effort, com savepoint próprio, sem cancelar o negócio quando falha. (`app/ponto/management/commands/rederivar_falta_declarada.py:165`, `app/ponto/management/commands/rederivar_falta_declarada.py:193`, `app/core/observ.py:105`, `app/core/observ.py:133`, `app/core/observ.py:139`.) |
| **8 — Cadastro atual decide passado** | **Sim, no alcance.** O corte atual passa a decidir também badge/filtro/contador históricos. Além disso, a nova guarda da reversão usa empresa do arquivo sem conferir a atual, permitindo o contraexemplo A→B acima. (`app/ponto/services/fechamento_lista.py:104`, `app/ponto/janelas.py:50`, `app/ponto/views.py:1215`, `app/ponto/management/commands/rederivar_falta_declarada.py:228`.) |

Exemplo da ampliação do8: novembro2026, demissão20/11; corte21 produz rescisão verdadeira; mudar o corte atual para16 produz falsa ao reler novembro, sem mudar a demissão. (`app/ponto/janelas.py:16`, `app/ponto/janelas.py:50`, `app/ponto/services/fechamento_lista.py:104`, `app/folha/export.py:60`.)

**5. Novidade do delta, exceções e limites**

- **Incompatibilidade nova da reversão — certeza alta:** arquivo legítimo da v1, sem `empresa_id`, com falta já rederivada420 e competência aberta → `--reverter` agora retorna `CommandError`, sem restaurar720. A recusa é explícita e deliberada; não demonstra, sozinha, bug financeiro, mas deixa esse formato legado sem caminho de reversão neste comando. Prejuízo real: **[nao verificado]**. (`app/ponto/management/commands/rederivar_falta_declarada.py:213`.)

- **As falhas de tipo/empresa e concorrência descritas acima são curas incompletas dos achados1/3**, não regressões financeiras inéditas demonstradas desta volta. A nova validação não cobre esses estados, e a transação não cria trava comum com a entrega. (`app/ponto/management/commands/rederivar_falta_declarada.py:167`, `app/ponto/management/commands/rederivar_falta_declarada.py:224`, `app/ponto/management/commands/rederivar_falta_declarada.py:228`, `app/folha/models.py:182`.)

- **Idempotência serial melhorou:** segunda aplicação sem alvo não cria arquivo; segunda reversão imediata recusa720 quando esperava420. Isso não comprova identidade exclusiva do ato: não há estado de consumo do arquivo, e o atestado420 do contraexemplo também satisfaz a comparação. (`app/ponto/management/commands/rederivar_falta_declarada.py:145`, `app/ponto/management/commands/rederivar_falta_declarada.py:226`, `app/ponto/management/commands/rederivar_falta_declarada.py:234`.)

- **Competência sem empresa selecionada: nenhuma regressão identificada nesse caminho.** `empresa_id=None` significa ausência de filtro; `Colaborador.empresa` é obrigatória, e cada linha usa sua própria empresa. O universo sem filtro já une as janelas das empresas. (`app/colaboradores/models.py:321`, `app/ponto/services/fechamento_lista.py:79`, `app/ponto/services/fechamento_lista.py:104`, `app/ponto/services/fechamento.py:424`.)

VEREDITO: REPROVA

## 05/11 15:38 · sessao 12fa111f1aab · - · - · origem=Ronald (tmux)

**Questão 2 — não procede a crítica de que o patch não alcança a janela; fila para a falta de teste concorrente.** [nao verificado]

- O teste silencia o pai dentro do patch de `_grava`, após a gravação real, e exige mensagem de saída da fila e destino original preservado (`app/chamados/tests/test_f3_beco1_reata_limbo.py:138`, `app/chamados/tests/test_f3_beco1_reata_limbo.py:145`, `app/chamados/tests/test_f3_beco1_reata_limbo.py:146`).
- A seleção inicial ocorre em `:59`; `_grava` em `:91`; a nova revalidação em `:100`; a costura em `:105`. Portanto o patch está efetivamente entre seleção e costura (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:59`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:91`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:100`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:105`).
- Pela leitura do delta anterior anexado, a versão sem revalidação carregava o pai silenciado e ainda chamava a porta; o gate do fio só testa terminalidade, aceitando qualquer status não terminal sem consultar silêncio. Assim, o teste morde essa versão, por leitura (`app/chamados/services/fio.py:764`, `app/chamados/services/fio.py:765`; versão anterior no anexo do usuário, sem arquivo local ancestral **[não verificado]**).
- **Não prova a trava:** uma reconsulta simples de `qs_na_fila`, sem `select_for_update`, também vê o silêncio já confirmado pelo próprio teste e passa. Não há segunda conexão nem escritor concorrente no teste (`app/chamados/tests/test_f3_beco1_reata_limbo.py:138`, `app/chamados/tests/test_f3_beco1_reata_limbo.py:143`).
- `_grava` é incondicional no caminho normal de `_rodar` com apply, inclusive com `por_pai` vazio, e precede todo o laço de costura. Contudo, “todo caminho de `--apply`” exige ressalva: opções inválidas abortam antes; `--reverter` retorna antes da validação de apply e executa outro caminho, sem `_grava` (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:37`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:39`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:82`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:91`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:110`).
- Execução GREEN/RED ou mutação: **[não verificado]**.

**Questão 3 — não procede manter a crítica do assert antigo; o novo assert morde uma porta muda, por leitura.**

- A fixture exige zero perguntas passadas antes do lançamento; o wrapper chama a função real e registra os motivos retornados por dia; o teste exige retorno não vazio para 27/09 e pergunta persistida com colaborador, dia e algum motivo daquele retorno (`app/escala/tests/test_f3_beco1_cadastro_entrou.py:110`, `app/escala/tests/test_f3_beco1_cadastro_entrou.py:113`, `app/escala/tests/test_f3_beco1_cadastro_entrou.py:127`, `app/escala/tests/test_f3_beco1_cadastro_entrou.py:129`).
- Uma substituição da função real por `return []` produz `emitidos[27/09] == []` e falha em `assertTrue`, mesmo que uma pergunta de 22/09 continue nascendo por outro caminho. Uma porta que retorna motivos sem gravar pergunta também é cobrada pelo assert seguinte (`app/escala/tests/test_f3_beco1_cadastro_entrou.py:114`, `app/escala/tests/test_f3_beco1_cadastro_entrou.py:115`, `app/escala/tests/test_f3_beco1_cadastro_entrou.py:127`, `app/escala/tests/test_f3_beco1_cadastro_entrou.py:129`).
- Na implementação real, a lista exclui motivos já existentes; `perguntar_pela_celula` chama `perguntar` com push silenciado e devolve os motivos; `perguntar` chama as funções de disputa e geração (`app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/services/fabrica_da_celula.py:126`, `app/chamados/services/fabrica_da_celula.py:174`, `app/chamados/services/fabrica_da_celula.py:176`, `app/chamados/services/fabrica_da_celula.py:177`, `app/chamados/services/fabrica_da_celula.py:139`).
- A prova é de **ao menos uma pergunta correspondente**, pois usa `.exists()`, não de todos os motivos, quantidades ou exclusividade causal; silêncio de push também não é assertado nesse caso (`app/escala/tests/test_f3_beco1_cadastro_entrou.py:129`).

**Quem pode criar a pergunta de 22/09 antes dessa porta: existe um caminho explícito no corpo do julgamento.**

`julgar_colab` executa `_julgar_colab_corpo` antes do laço de `perguntar_pela_celula` (`app/ponto/services/cartorio.py:948`, `app/ponto/services/cartorio.py:995`, `app/ponto/services/cartorio.py:997`).

Para furo parcial, o corpo chama `emissor._abrir_chamado` com marcos nomeados; esse emissor invoca diretamente `criar_disputa_a_partir_de_chamados_orfaos`, que possui ramo para chamado passado com marcos da ata; esse ramo chama `_complementar_marcos_faltantes`, que chama `_criar_pergunta_se_couber`, que chama `PerguntaDisputa.abrir`; o INSERT final está em `models.py:2465` (`app/ponto/services/cartorio.py:1777`, `app/ponto/management/commands/detectar_ausencias.py:796`, `app/chamados/services/disputa_emissao.py:666`, `app/chamados/services/disputa_emissao.py:680`, `app/chamados/services/disputa_emissao.py:1004`, `app/chamados/services/disputa_emissao.py:1013`, `app/chamados/services/disputa_emissao.py:813`, `app/chamados/services/disputa_emissao.py:770`, `app/chamados/models.py:2465`).

Assim, **sem passar por `perguntar_pela_celula`, mas ainda pela porta final `PerguntaDisputa.abrir`**. Que esse foi efetivamente o ramo executado para 22/09 nesta rodada é afirmado pela docstring/anexo, sem trace ou execução independente: **[não verificado]** (`app/escala/tests/test_f3_beco1_cadastro_entrou.py:18`).

A fixture cria a entrada de 22/09, cujo signal pode julgar o dia antes do lançamento; não confundir automaticamente a ata lida pela fábrica com `_tl.atas`: `_marcos_faltantes` lê `grade_da_celula`, enquanto a ata em memória é exposta para outros juízes (`app/escala/tests/test_f3_beco1_cadastro_entrou.py:106`, `app/ponto/signals.py:135`, `app/ponto/signals.py:93`, `app/chamados/services/declaracao_texto.py:79`, `app/ponto/services/cartorio.py:48`).

## 05/11 15:38 · sessao 5df850dca63b · - · - · origem=Ronald (tmux)

Resultados para consolidação; caminhos relativos à raiz da cópia. A execução isolada do núcleo confirmou os dois exemplos abaixo sem carregar Django; comportamento em produção não foi medido. [não verificado]

| ID | Tipo | Texto e afirmação | Código que faz | Veredito | Certeza |
|---|---|---|---|---|---|
| C1 | a | `app/ponto/calculador/alimentacao.py:333`: “no núcleo ficam o dedup de 10 min, o corte…” | `app/ponto/calculador/nucleo.py:38` declara retirada do dedup; `:109` apenas ordena; `:186` percorre todas as batidas. | **NÃO VALE MAIS** | Alta |
| C2 | a | `app/ponto/calculador/alimentacao.py:321` e `:323`: “AS RUBRICAS NÃO MORAM AQUI AINDA”, ligado à LEI-AKITA 12. | `app/ponto/calculador/alimentacao.py:353` define `rubricas_por_dia`; `:514` monta os insumos e chama `regras.do_dia`; `:533` devolve as rubricas por dia. | **NÃO VALE MAIS** | Alta |
| C3 | a | `app/ponto/calculador/periodo.py:153`: lápide avisa que a prosa do v2 diz 5 min, mas o corpo usa 10. | `app/ponto/motor_calculo_v2.py:60` define 10; `:1952` ainda escreve 5 na docstring; `:2006` e `:2008` comparam com a constante 10. | **AINDA VALE** | Alta |
| C4 | a | `app/ponto/calculador/regras.py:607`: sem as duas flags da CCT, noturnas devem sair declaradas, sem default booleano. | `app/ponto/calculador/regras.py:613` exige ambas diferentes de `None`; `:615` registra `SEM_ENTRADA`; `:844` só calcula com a guarda satisfeita. | **AINDA VALE** | Alta |
| C5 | a | `app/ponto/calculador/regras.py:616`: “A LEI VEM ANTES DO DADO”; família isenta não cobra mesmo recebendo marcos. | `app/ponto/calculador/regras.py:623` registra a causa nas duas rubricas e `:626` elimina a entrada de pontualidade. A tabela usa identidade de classe em `:491` e o leitor consulta `type(motor)` em `:522`. | **AINDA VALE** | Alta |
| C6 | a | `app/ponto/calculador/regras.py:680`: feriado convertido em folga pelo vínculo deve ser HE100 feriado, sem acumular folga trabalhada. | `app/ponto/calculador/regras.py:684` exige `folga_e_feriado` e minutos com escala certa; `:686`/`:687` escrevem HE100 e HE100 feriado; somente o `elif` de `:689` escreve folga. | **AINDA VALE**, com a condição explícita de escala certa | Alta |
| C7 | a | `app/ponto/calculador/regras.py:692`: hora sem escala certa não some; entra em trabalhadas sem adicional de 100%. | `app/ponto/calculador/regras.py:399` devolve os minutos na segunda parcela quando `escala_certa=False`; `:694` grava essa parcela em `horas_trabalhadas`, mantendo folga em zero conforme `:679`. | **AINDA VALE** | Alta |
| C8 | a | `app/ponto/calculador/regras.py:793`: dobra de feriado não retorna antes de calcular noturno, split noturno e pontualidade. | `app/ponto/calculador/regras.py:783` aplica a dobra; `:799` usa `elif` para o excedente; o fluxo segue para split em `:824`, noturnas em `:844` e pontualidade em `:855`, antes do retorno em `:874`. | **AINDA VALE** | Alta |
| C9 | e | `app/ponto/tests/test_calculador_portoes_de_marco.py:2`, especialmente `:4` e `:8`: docstring com **SEMPRE** descreve perdão da volta do intervalo por proximidade de hfi e distância de hi. | `app/ponto/calculador/regras.py:251` usa `cumpriu_a_volta_do_intervalo`; `app/escala/regua_defesa.py:492` pergunta ao alinhamento qual marco a batida ocupa e `:495` exige delta ≤10. A substituição do critério antigo está declarada em `:472`. | **NÃO VALE MAIS** quanto ao critério de entrada; saída ainda usa proximidade em `regras.py:259` | Alta |
| C10 | e | `app/ponto/tests/test_calculador_regras.py:237`/`:241`: docstring com **DENTRO** afirma que o envelope declarado impede partir a jornada quando o meio do vão está dentro dele. | `app/ponto/calculador/nucleo.py:183` calcula o meio; `:184` testa inclusão; `:189`/`:191` preservam o turno antes dos cortes de `:216`. O próprio teste verifica teto 729 em `test_calculador_regras.py:279`. | **AINDA VALE** | Alta |

Exemplo verificável de C1: duas batidas `08:00` e `08:05` são preservadas e formam **5 minutos**, pela soma em `app/ponto/calculador/nucleo.py:246`; portanto o comentário atual sobre dedup de 10 minutos ficou para trás. A comparação de envelope de C10 resultou em **228 → 729 minutos**, mantendo piso **199**, coerente com os asserts em `app/ponto/tests/test_calculador_regras.py:267`, `:279` e a explicação de `:273`.

No calculador, as ocorrências dos literais solicitados aparecem como exemplos ou referências; não selecionei item tipo c deste subdomínio. [não verificado]

## 05/11 15:38 · sessao 8e6512a7b2ab · - · - · origem=Ronald (tmux)

**Item d — help_text da falta inteira.** `app/ponto/models.py:757` afirma “Para falta dia inteiro: 720”. O código deriva a jornada prevista quando uma falta aprovada chega com minutos zerados e preserva valores positivos (`app/ponto/models.py:875-885`). O export usa os minutos efetivos do registro (`app/folha/export.py:110-111`), e o fechamento soma o mapa resultante (`app/ponto/services/fechamento.py:864-866`). **NAO VALE MAIS; certeza alta, estática.**

Contraexemplo estático: falta aprovada de um dia, inicialmente com minutos=0, jornada 08–17 e pausa 12–13: a fórmula retorna 480, não 720 (`app/escala/utils.py:2293`, `app/escala/utils.py:2335-2341`). Ocorrência desse cenário em produção: [nao verificado].

**Item c — limiar de 480 minutos.** A regra está em `app/ponto/turnos.py:2580`; o predicado considera parcial apenas tipo que aceita horas, limitado a um dia e com `0 < minutos < 480` (`app/ponto/turnos.py:2598-2600`). **AINDA VALE; certeza alta, estática.**

É dependência direta do fechamento: mapa de cobertura → seleção dos dias inteiros (`app/ponto/services/fechamento.py:679`, `app/ponto/services/fechamento.py:682-683`) → predicado de parcial (`app/ponto/turnos.py:2689-2690`, `app/ponto/turnos.py:2675`) → motor, que zera atraso e saída antecipada nesses dias (`app/ponto/services/fechamento.py:742`, `app/ponto/motor_calculo_v2.py:1085-1096`). A própria `cobertura_ausencia_periodo` não consulta o limiar; apenas monta o mapa (`app/ponto/turnos.py:2753-2770`).

## 05/11 15:38 · sessao 9054813c360a · - · - · origem=Ronald (tmux)

Leitura estática; nenhum teste foi executado. A ocorrência de casos reais neste hash não foi verificada. [não verificado] [nao verificado]

| Item | Tipo | Texto:linha / afirmação | Código:linha / comportamento | Veredito | Certeza |
|---|---|---|---|---|---|
| C1 | a | `app/ponto/turnos.py:2510`: “pra FOLHA cobre só APROVADA”; cobrança aceita não rejeitada | `app/ponto/turnos.py:2514`: folha filtra `aprovada`; `:2516` cobrança exclui `rejeitada`; `:2518` rejeita outro efeito | **AINDA VALE** | Alta, estática |
| C2 | a | `app/ponto/turnos.py:2542`: desempate por maior `data`, depois maior `pk` | `app/ponto/turnos.py:2557` usa `order_by('-data','-pk').first()`; `:2757` ordena crescente e `:2769` sobrescreve no mapa do período | **AINDA VALE** | Alta, estática |
| C3 | c | `app/ponto/turnos.py:2575`: parcial = tipo que aceita horas, um dia, minutos abaixo dos 480 cravados; literal em `:2580` | `app/ponto/turnos.py:2598` exige tipo no conjunto; `:2599` exige um dia; `:2600` exige `0 < minutos < 480`. Assim 479 é parcial e 480 explica inteiro, independentemente do previsto | **AINDA VALE** | Alta, estática |
| C4 | a | `app/escala/utils.py:1227`: ausência não falta cobre folga; falta aprovada em folga segue folga | `app/escala/utils.py:1231` exclui precisamente combinação `tipo_dia='folga'` e ausência `falta`; `:1234` marca falta nos demais dias | **AINDA VALE** | Alta, estática |
| C5 | a | `app/escala/utils.py:1256`: parcial dispensa marco dentro do período; fora segue missing; sem período nada dispensa | `app/escala/utils.py:1265` exige período e deslocamentos; `:1269` exige célula missing e marco conhecido; `:1270` dispensa apenas resultado `dentro`. `app/ponto/turnos.py:2639` usa bordas inclusivas | **AINDA VALE** | Alta, estática |
| C6 | a | `app/chamados/services/materializacao.py:61`: sem fim cobre aberto “SO para TIPOS_BLOQUEIAM_PONTO”; bloco contém corte em `:64` | `app/chamados/services/materializacao.py:78` delega a `ausencia_cobre`; `app/ponto/turnos.py:2555` usa `TIPOS_EM_ABERTO`. São conjuntos distintos: aliases em `app/ponto/models.py:684` e `:689`; gramática independente em `app/ponto/catalogo/ausencias.py:198` | **NAO VALE MAIS** — nome e critério antigos permaneceram | Alta sobre código; composição atual do cadastro [não verificado] |
| C7 | a | `app/ponto/precedencia.py:13`: feriado suprime trabalho, salvo `trabalha_em_feriado=True` | `app/ponto/precedencia.py:376` preserva previsto quando `not ec.feriado_vira_folga(data)`; `app/escala/models.py:1392` retorna False para ciclos fora da lista `:32`, inclusive 12x36 | **CONTRADIZ** na leitura literal do texto | Alta, estática |
| C8 | a | `app/chamados/juizes.py:412`: “Duas clausulas, e so elas”; ata preenchida diferente de trabalho, inclusive ausência, torna dia sem trabalho (`:414`) | `app/chamados/juizes.py:424` abre exceção para parcial; `:425` retorna somente `cel.trabalha is False`; predicado parcial em `:408` | **CONTRADIZ** — a ressalva implementada não está na afirmação “e só elas” | Alta, estática |
| C9 | e | `app/ponto/tests/test_cobparc_marco_no_periodo.py:15`: docstring com casos “DENTRO”; 13–17 dispensa E13/S17; 14–17 conserva E13; S07 noturna está em D+1 (`:16`, `:18`, `:20`) | `app/escala/utils.py:1265`–`:1271` aplica dispensa por marco; `app/ponto/turnos.py:2636` soma 1440 ao fim noturno; `:2639` inclui bordas. Asserções correspondentes em `app/ponto/tests/test_cobparc_marco_no_periodo.py:181`, `:191` | **AINDA VALE** | Alta para algoritmo; execução [não verificado] |
| C10 | a | `app/ponto/turnos.py:2686`: “SO O QUE ABONA”; tipos sem abono mantêm cobrança própria | `app/ponto/turnos.py:2690` exige `a.efeito == ABONA` e ausência inteira; motor usa esse mapa em `app/ponto/motor_calculo_v2.py:1085` e zera atraso/saída em `:1095`–`:1096` | **AINDA VALE** | Alta, estática |

Contraexemplos necessários:

- **C7:** dia feriado previsto de vínculo 12x36, `trabalha_em_feriado=False`, sem ausência nem folga lançada: o texto prevê supressão; o código mantém `trabalho`, pois 12x36 não pertence aos ciclos de `feriado_vira_folga`. `app/ponto/precedencia.py:376`, `app/ponto/precedencia.py:382`, `app/ponto/precedencia.py:386`, `app/escala/models.py:32`, `app/escala/models.py:1392`. Existência desse registro em produção: [não verificado].
- **C8:** `cel.trabalha=True`, `ata={'tipo_dia':'ausencia','cobertura_parcial':True}`: a segunda cláusula textual diria dia sem trabalho; a função retorna False, preservando cobrança do resto. `app/chamados/juizes.py:408`, `app/chamados/juizes.py:414`, `app/chamados/juizes.py:424`. Existência em produção: [não verificado].

Outra ocorrência útil para eventual substituição de item: docstring `app/ponto/tests/test_coberto_range_aberto.py:1`–`:5` contém **SO** e repete o critério antigo `TIPOS_BLOQUEIAM_PONTO`; o leitor atual em `app/ponto/management/commands/reconciliar_grade.py:401` delega ao critério `TIPOS_EM_ABERTO` de `app/ponto/turnos.py:2555`. **NAO VALE MAIS**, alta certeza sobre o critério do código; cadastro atual [não verificado].

## 05/11 15:38 · sessao 0aeaf7f36a1e · - · - · origem=Ronald (tmux)

Mapa para a resposta da questão 4, limitado aos campos usados na seleção, reata e prova de emissão do delta (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:59`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:86`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:100`, `app/escala/tests/test_f3_beco1_cadastro_entrou.py:127`).

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `ChamadoColaborador.silenciado_ate` | Produção: `arquivo.silenciar`, atribuição e `save` em `app/chamados/services/arquivo.py:166`; limpeza por `levantar` em `app/chamados/services/arquivo.py:201`, persistida em `app/chamados/services/arquivo.py:208`. Teste novo escreve diretamente por `update` em `app/chamados/tests/test_f3_beco1_reata_limbo.py:140`. | Fila em `app/chamados/services/painel.py:188`; leitura da instância em `app/chamados/models.py:476`; tradução de verbos em `app/chamados/services/verbos.py:45`. |
| `ChamadoColaborador.status_local` | Transições: `_gravar_estado` atribui em `app/chamados/models.py:222`, salva em `app/chamados/models.py:279`; nascimento por `criar` em `app/chamados/models.py:1048`. | Universo/fila em `app/chamados/services/painel.py:155` e `app/chamados/services/painel.py:187`; seleção do pai LIMBO em `app/chamados/services/limbo_folgas.py:217`; costura em `app/chamados/services/fio.py:764`. |
| Identidade do pai: `colaborador`, `modulo_origem`, `subcategoria`, `pk` | Nascimento do chamado por `criar`, com colaborador e campos recebidos em `app/chamados/models.py:1048`; chave primária gerada efetivamente pelo banco: **[não verificado]**. | Pai LIMBO por colaborador/módulo/subcategoria/vivos, escolhendo maior `pk`, em `app/chamados/services/limbo_folgas.py:217`; comando usa `pk` em `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:100`. |
| `PerguntaDisputa.chamado_destino` | `fio.reapontar_resolvedora`: atribui e salva em `app/chamados/services/fio.py:845`. **Também existe escritor no gate de identidade:** pergunta existente recebe destino em `app/chamados/models.py:2439`, persistido em `app/chamados/models.py:2463`. Nascimento em `app/chamados/models.py:2465`. | Reversão em `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:86`; idempotência em `app/chamados/services/limbo_folgas.py:239`; reversor em `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:119`. |
| `PerguntaDisputa.disputa`, `motivo`, `data_evento`, `celula` | Nascimento em `app/chamados/models.py:2465`; atualização da célula existente em `app/chamados/models.py:2425`; data faltante pode ser preenchida pela costura em `app/chamados/services/fio.py:832`. `DisputaSupervisao.colaborador` nasce em `app/chamados/services/disputa_emissao.py:523`. | Identidade/dedup em `app/chamados/models.py:2381`; fábrica procura perguntas existentes por colaborador/dia e recolhe motivos em `app/chamados/services/fabrica_da_celula.py:118`; assert novo em `app/escala/tests/test_f3_beco1_cadastro_entrou.py:129`; juiz da resolvedora lê dia em `app/chamados/juizes.py:1905`. |
| `PerguntaDisputa.veredito_lavrado`, `julgado_em`, `janela_expira_em` | `lavrar` atribui os três em `app/chamados/veredito_lavrado.py:227`; costura persiste junto ao destino em `app/chamados/services/fio.py:847`. | Validade da lavra em `app/chamados/veredito_lavrado.py:179`. |
| `CelulaDia.veredito`, `protestos`, `ata` | Porta da célula em `app/ponto/portas/celula.py:798`; fixture grava `indefinida` diretamente em `app/escala/tests/test_f3_beco1_cadastro_entrou.py:71`. | Teste avalia estado em `app/escala/tests/test_f3_beco1_cadastro_entrou.py:76`; fábrica lê veredito/ata em `app/chamados/services/fabrica_da_celula.py:99`. |
| `CelulaDia.dna`, `trabalha`, `escala_geradora`, `regenerada_em` | Geração/regeneração em `app/ponto/portas/celula.py:253` e `app/ponto/portas/celula.py:266`. | Teste compara foto da exportada em `app/escala/tests/test_f3_beco1_cadastro_entrou.py:108` e `app/escala/tests/test_f3_beco1_cadastro_entrou.py:123`; seleção da regeneração compara em `app/ponto/portas/celula.py:257`. |
| `CelulaDia.julgada_em` | `carimbar` em `app/ponto/portas/celula.py:830`, persistência em `app/ponto/portas/celula.py:837`. | Foto da exportada em `app/escala/tests/test_f3_beco1_cadastro_entrou.py:108`; fábrica usa o instante anterior em `app/chamados/services/fabrica_da_celula.py:102`. |

**Achado documental — gravidade: fila.** A alegação de escritor único de `chamado_destino` na docstring do fio diverge do escritor de perguntas existentes em `PerguntaDisputa.abrir`; este último consulta as guardas, mas escreve diretamente. Isso é relevante ao censo, sem demonstrar por si um defeito novo do delta (`app/chamados/services/fio.py:787`, `app/chamados/models.py:2427`, `app/chamados/models.py:2436`, `app/chamados/models.py:2439`, `app/chamados/models.py:2463`).

`JUIZES.md` é lookup e manda privilegiar o registro/código vivo; aponta transições para `motor.decidir`, conjuntos de vida/cobrança para o catálogo, duplicado para `ChamadoColaborador.abrir` e fio/disputa para `_disputa_do_chamado`. O predicado de fila e os escritores do silêncio são comprovados nos serviços citados acima; o lookup não os enumera como autoridade específica (`app/docs/JUIZES.md:7`, `app/docs/JUIZES.md:14`, `app/docs/JUIZES.md:37`, `app/docs/JUIZES.md:40`, `app/docs/JUIZES.md:51`).

As demais autoridades envolvidas são dia sem trabalho, status do dia, folga lançada e vínculo do dia/cobertura, nos endereços do lookup; o delta reaproveita `qs_na_fila`, cujo catálogo distingue `registrado` vivo de cobrança, sem clonar esses conjuntos (`app/docs/JUIZES.md:55`, `app/docs/JUIZES.md:88`, `app/docs/JUIZES.md:90`, `app/docs/JUIZES.md:98`, `app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:29`, `app/chamados/services/painel.py:186`).

## 05/11 15:38 · sessao ca68d03b1fcf · - · - · origem=Ronald (tmux)

Auditoria estática, somente leitura; não executei testes. Ocorrência desses cenários em produção: [não verificado]. [nao verificado]

| ID | Tipo | Texto | Código | Veredito | Certeza |
|---|---|---|---|---|---|---|
| F1 | e | `app/ferias/test_services_leitura.py:2` — juiz e property de vencimento concordam **SEMPRE** | `app/ferias/services.py:118`; `app/ferias/models.py:227`; `app/ferias/models.py:257`; `app/ferias/models.py:95` | CONTRADIZ | Alta |
| F2 | d | `app/ferias/models.py:122` — limite de concessão = **12 meses após data_fim** | `app/ferias/services.py:472`; `app/ferias/services.py:473` | CONTRADIZ | Alta |
| F3 | d | `app/ferias/models.py:307` — aviso **emitido** com antecedência de 30+ dias | `app/ferias/services.py:314`; `app/ferias/services.py:323`; `app/ferias/services.py:378`; `app/ferias/services.py:404` | CONTRADIZ | Alta no cálculo; média na interpretação de “emitido” |
| F4 | d | `app/ferias/models.py:126` — abono máximo de 1/3 do direito | `app/ferias/models.py:235`; `app/ferias/views.py:918`; `app/ferias/services.py:197`; `app/ferias/services.py:295` | AINDA VALE nas validações de venda examinadas | Alta |
| F5 | a | `app/ferias/views.py:474` — correção de datas recusa colisão/integridade; regra legal vira aviso | `app/ferias/views.py:463`; `app/ferias/views.py:466`; `app/ferias/views.py:483`; `app/ferias/services.py:387`; `app/ferias/models.py:428`; `app/ferias/views.py:507` | AINDA VALE | Alta |
| F6 | a | `app/ferias/models.py:383` — materialização das férias passa pelo veto; recusa não grava e gera pauta DP | `app/ferias/models.py:385`; `app/ponto/services/ausencia.py:1186`; `app/ponto/services/ausencia.py:1193`; `app/ponto/services/ausencia.py:1200` | AINDA VALE para dias ainda não materializados | Alta |
| F7 | a | `app/ferias/models.py:405` — guarda permanece no save e delega precedência | `app/ferias/models.py:435`; `app/ferias/models.py:428`; `app/ferias/models.py:431` | AINDA VALE no caminho `save()` | Alta |
| F8 | d | `app/ferias/models.py:506` — previsão de retorno pode ser nula, significando sem previsão | `app/ferias/models.py:505`; `app/ferias/models.py:529`; `app/ferias/models.py:534` | AINDA VALE | Alta |
| F9 | d | `app/ferias/models.py:510` — retorno real encerra afastamento | `app/ferias/models.py:529`; `app/ferias/models.py:534` | AINDA VALE no estado do cadastro | Alta |
| F10 | e | `app/ferias/test_a3_situacao_de_ferias.py:327` — 01/01/2027 fica DENTRO da janela de 60 dias da fixture | `app/ferias/test_a3_situacao_de_ferias.py:315`; `app/ferias/situacao.py:55`; `app/ferias/situacao.py:205`; `app/ferias/situacao.py:208` | AINDA VALE | Alta |

**F1 — contraexemplo estático:** PA com `status='adquirido'`, saldo positivo, `data_fim` passada e limite ontem. O serviço inclui porque filtra o status gravado e apenas um teto de data; a property exclui porque `status_de_periodo()` devolve `vencido`. Não precisa supor mudança de regra nem dados externos. `app/ferias/services.py:118`; `app/ferias/services.py:119`; `app/ferias/models.py:100`; `app/ferias/models.py:257`.

**F2 — contraexemplo estático:** `editar_periodo_aquisitivo(..., data_inicio=2023-01-01)` calcula fim em 31/12/2023 e limite em 30/12/2024; doze meses após 31/12/2023 é 31/12/2024. A criação pela tela usa substituição do ano, portanto há também dois cálculos diferentes no mesmo domínio. `app/ferias/services.py:472`; `app/ferias/services.py:473`; `app/ferias/views.py:138`; `app/ferias/views.py:171`.

**F3 — contraexemplo estático:** agendamento iniciado daqui a 45 dias já nasce com `aviso_emitido=True`; a atribuição testa exclusivamente a distância entre datas. Corrigir ou remarcar recalcula o mesmo booleano sem consultar evidência de emissão. `app/ferias/services.py:314`; `app/ferias/services.py:323`; `app/ferias/services.py:378`; `app/ferias/services.py:404`. Se houve emissão por outro mecanismo em produção: [não verificado].

**F4 — limite da conclusão:** a regra é aplicada pelos caminhos examinados de venda avulsa, agendamento com abono e gozo histórico. O campo é `IntegerField` e o `save()` do PA só chama a guarda de sobreposição; não é uma restrição global do modelo/banco demonstrada por esses pontos. `app/ferias/views.py:918`; `app/ferias/services.py:197`; `app/ferias/services.py:295`; `app/ferias/models.py:126`; `app/ferias/models.py:201`.

**F6/F7 — limites da conclusão:** o espelhamento pula dias já existentes; a guarda de agendamento também pula cancelados e atualizações sem alteração de datas. Portanto, “cada dia passa pelo veto” não deve ser apresentado como revalidação integral do acervo em todo `save()`. `app/ponto/services/ausencia.py:1183`; `app/ferias/models.py:394`; `app/ferias/models.py:397`; `app/ferias/models.py:402`.

## 05/11 15:38 · sessao 2e9960cecb9c · - · - · origem=Ronald (tmux)

Leitura estática; nenhum teste executado, nenhum arquivo alterado. Ocorrências reais em produção e consequências em banco/log permanecem [nao verificado]. [nao verificado]

| Item | Tipo | Texto:linha | Código:linha | VEREDITO | Certeza |
|---|---|---|---|---|---|---|
| H1 | a | `app/holerite/models.py:138` | `app/holerite/views.py:1174`, `:1191`, `:1193`; `app/holerite/urls.py:8` | **CONTRADIZ** | Alta, estática |
| H2 | e | `app/holerite/tests/test_selo_c2_match_nome.py:3` | `app/holerite/views.py:1736`, `:1741`; `app/holerite/motor.py:298`, `:218` | **CONTRADIZ**, se “SEMPRE/NUNCA” for aplicado a todo match do domínio | Alta quanto ao ramo; escopo do texto merece delimitação |
| H3 | d | `app/holerite/models.py:123` | `app/holerite/views.py:1199`, `:1202`; lote conforme em `app/holerite/motor.py:479` | **CONTRADIZ** | Alta, estática |
| H4 | a | `app/holerite/motor.py:316` | `app/holerite/motor.py:323`, `:344`, `:468` | **AINDA VALE** | Alta, estática |
| H5 | a | `app/holerite/motor.py:373` | `app/holerite/motor.py:382`, `:385`, `:386` | **AINDA VALE**, com a ressalva explícita sobre arquivos | Alta, estática |
| H6 | a | `app/holerite/models.py:143` | `app/holerite/motor.py:445`, `:448`, `:449`, `:456` | **AINDA VALE** para republicação dos mesmos bytes | Alta, estática |
| H7 | a | `app/holerite/views.py:1514` | `app/holerite/views.py:1524`, `:1537` | **AINDA VALE** | Alta, estática |
| H8 | d | `app/holerite/models.py:109` | `app/holerite/motor.py:465`, `:470`, `:471`, `:480` | **AINDA VALE** | Alta, estática |
| H9 | a | `app/holerite/motor.py:180` | `app/holerite/motor.py:99`, `:117`, `:199` | **NAO SE SABE** quanto à afirmação sobre os três exports reais | Alta quanto à insuficiência da evidência |

**H1 — Versão anterior nunca apagada.** O comentário afirma que a folha refeita preserva o holerite anterior no histórico, com sua ciência, e cria versão nova (`app/holerite/models.py:138`). A porta avulsa, existente em URL (`app/holerite/urls.py:8`), recusa os anteriores assinados, mas deleta os não assinados e cria outro holerite (`app/holerite/views.py:1176`, `:1191`, `:1193`). **Contraexemplo estático:** holerite sem ciência já existente + POST válido no upload avulso → anterior deletado; novo criado sem incremento explícito de `versao`, cujo default é 1 (`app/holerite/models.py:153`). A rotina denominada `nova_versao` apenas calcula o próximo número, registra evento e retorna; não carimba a substituição nem cria a nova linha (`app/holerite/servicos.py:59`, `:64`, `:73`).

**H2 — Match sempre CPF/nome, nunca matrícula.** A docstring de teste faz essa afirmação absoluta (`app/holerite/tests/test_selo_c2_match_nome.py:3`). O padrão Domínio usa `nome_completo` e está conforme nesse caminho (`app/holerite/motor.py:161`). Entretanto, a porta configurável aceita `campo_match` do POST e usa `codigo_dominio` por default (`app/holerite/views.py:1741`); o motor aplica o campo recebido como filtro ORM (`app/holerite/motor.py:218`, `:224`). **Contraexemplo estático:** formato `config`, regex que capture código, `campo_match` omitido e um único colaborador com aquele `codigo_dominio` → match por código, sem nome/CPF (`app/holerite/views.py:1736`, `:1741`; `app/holerite/motor.py:216`, `:226`). Se o item for restrito estritamente ao default Domínio, o veredito desse escopo é AINDA VALE (`app/holerite/motor.py:161`).

**H3 — Empresa do documento sempre vem do CNPJ impresso.** O `help_text` promete essa origem e exclui a empresa atual do cadastro (`app/holerite/models.py:123`). O lote faz isso (`app/holerite/motor.py:479`, `:482`); o upload avulso declara que não lê CNPJ e usa `colab.empresa_id` (`app/holerite/views.py:1199`, `:1202`). **Contraexemplo estático:** documento de empresa A enviado para pessoa cujo cadastro atual é B → avulso grava B no campo (`app/holerite/views.py:1202`). A ocorrência real dessa entrada não foi fornecida [nao verificado].

**H4 — Republicação incremental preserva publicados.** O comentário promete pular quem já possui holerite na competência e não deletar pelo lote (`app/holerite/motor.py:316`). O motor levanta os colaboradores já publicados, marca recibos correspondentes como `ja_publicado`, e só cria recibos cujo status seja `ok` (`app/holerite/motor.py:323`, `:344`, `:468`). O escopo é o lote; não generalizar para o avulso, que deleta (`app/holerite/views.py:1191`).

**H5 — Lote tudo ou nada.** A afirmação já traz a ressalva de que rollback desfaz linhas, não arquivos previamente escritos em storage (`app/holerite/motor.py:373`, `:382`). A chamada de `_gravar` está dentro de `transaction.atomic()` (`app/holerite/motor.py:385`). Portanto o texto, incluindo sua limitação, segue compatível com a implementação (`app/holerite/motor.py:385`, `:386`). Comportamento do storage e falhas reais não foram demonstrados [nao verificado].

**H6 — Republicar não anexa a mesma carta novamente.** O comentário especifica SHA256 do conteúdo recebido, evitando comparação com o PDF concatenado (`app/holerite/models.py:143`, `:149`). A anexação compara o hash de cada entrada com `anexos_sha`, pula quando não há novidade e grava os hashes novos (`app/holerite/motor.py:448`, `:449`, `:451`, `:456`). O código satisfaz a repetição dos mesmos bytes; estabilidade dos bytes após novo parse do mesmo PDF real não foi demonstrada [nao verificado].

**H7 — Download não assina.** O comentário separa ciência de GET e promete apenas carimbo de entrega na sessão (`app/holerite/views.py:1514`, `:1520`). A função adiciona o ID à sessão e retorna a entrega do PDF, sem criar assinatura nesse corpo (`app/holerite/views.py:1524`, `:1537`).

**H8 — Sequência dos recibos complementares.** O `help_text` define `seq` como ordem da página do colaborador no lote (`app/holerite/models.py:109`). O gravador mantém contador por colaborador, incrementa-o para cada recibo `ok` e o passa ao novo holerite (`app/holerite/motor.py:465`, `:468`, `:471`, `:480`).

**H9 — Recibos reais Domínio não trazem CPF; contagem histórica.** A docstring atribui a três exports medidos a ausência de CPF e registra resultados 29/29 e 28/29 (`app/holerite/motor.py:180`, `:184`, `:186`). O parser desta cópia extrai código/nome/CNPJ e não preenche CPF no recibo (`app/holerite/motor.py:99`, `:117`); isso mostra o que o parser lê, não prova o conteúdo dos PDFs mencionados. Faltam os três exports originais ou sua extração verificável e o cadastro usado nas contagens [nao verificado].

## 05/11 15:38 · sessao daa7bb0265bb · - · - · origem=Ronald (tmux)

Contraexemplo atual confirmado por leitura: **editar template em lote reescreve DNA, preserva ata e DiaPago anteriores e conserva o carimbo de prontidão**. A chamada usa explicitamente `recalculo=False`; o julgamento e o dinheiro dependem de `recalculo=True`. (`app/escala/services/cadastro_tipo.py:348`; `app/ponto/portas/celula.py:260`; `app/ponto/portas/celula.py:360`; `app/ponto/portas/celula.py:384`)

Entrada construída, sem atribuir ocorrência à produção: empresa com corte21; edição em 05/11/2026 após08h; dia04/11 na competência aberta; vínculo ativo, célula gerada e julgada, DiaPago anteriormente lavrado; nenhum chamado vivo de falta. Para 12x36, âncora de trabalho04/11. Existência desses cadastros na produção: [não verificado].

| Escala | Cadastro anterior e batidas | Alteração do mesmo template | Previsto anterior → novo |
|---|---|---|---|
| 6x1, folga domingo | 07–11; E07/S11 | 07–16, pausa12–13 | 240→480min |
| 12x36 diurno | 07–19, pausa12–13; E07/S12/E13/S19 | Pausa12–13:30 | 660→630min |
| 12x36 noturno | 04/11 19–05/11 07, pausa00–01; E19/S00/E01/S07 | Pausa00–01:30 | 660→630min |
| Personalizado sem intervalo | HPD quarta08–11, duração0; E08/S11 | HPD quarta08–12, duração0 | 180→240min |

Os números são a aplicação da própria aritmética dos marcos, com volta de24h e subtração das pausas; o personalizado até4h passa pelo piso zero. (`app/escala/utils.py:2290`; `app/escala/utils.py:2330`; `app/escala/models.py:6`)

Ordem e caminho:

1. Salvar a edição pela `salvar_tipo`; os horários e HPD são atualizados e o tipo é salvo. (`app/escala/services/cadastro_tipo.py:204`; `app/escala/services/cadastro_tipo.py:264`; `app/escala/services/cadastro_tipo.py:306`)
2. Propagação percorre vínculos ativos e regenera com `recalculo=False`. (`app/escala/services/cadastro_tipo.py:334`; `app/escala/services/cadastro_tipo.py:348`)
3. DNA recebe os novos marcos, preservando o anterior e denunciando a regeneração. (`app/ponto/management/commands/gerar_celulas.py:79`; `app/ponto/portas/celula.py:259`)
4. Ata e dinheiro não são atualizados nesse caminho. Os próprios testes afirmam zero julgamento e zero chamada à porta do dinheiro quando `recalculo=False`. (`app/ponto/portas/celula.py:360`; `app/ponto/portas/celula.py:384`; `app/ponto/tests/test_ordem_regen_julga_antes_do_dinheiro.py:108`; `app/ponto/tests/test_r6_dinheiro_acompanha_cadastro.py:107`)
5. A invalidação só zera `FechamentoMensal.derivado_em`; mantém `previsto_em` e números. (`app/escala/services/cadastro_tipo.py:357`; `app/ponto/services/fechamento.py:1636`)

**Código × regra:** `minutos_previstos_do_dia` responde o novo número do DNA, enquanto `grade_da_celula` prefere o número anterior da ata e DiaPago continua anterior. A regra exige coerência com o cadastro existente e lavratura no evento; o passado corrigido por cadastro tem exceção expressa à imutabilidade. (`app/escala/models.py:1124`; `app/escala/utils.py:2290`; `app/escala/services/leitor_celula.py:374`; `app/docs/LEIS.md:71`; `app/docs/LEIS.md:113`; `app/docs/LEIS.md:122`)

A prontidão também pode conservar o denominador anterior: aceita `previsto_em` preenchido e lê `fech.minutos_previstos`, sem consultar `derivado_em`. O signal que invalida `previsto_em` é do vínculo, enquanto esse caminho salva o template. (`app/folha/export.py:1019`; `app/folha/export.py:1024`; `app/escala/signals.py:124`; `app/escala/services/cadastro_tipo.py:306`)

| Item | Leitores/autoridades divergentes | Certeza | Dono |
|---|---|---|---|
| E-lote, quatro escalas | Previsto pelo DNA novo × grade pela ata antiga × DiaPago antigo; prontidão conserva snapshot | Alta, inferência estática; execução integral e ocorrência em produção [não verificado] | ESTRUTURA: o cadastro corrigido é igual para todos, mas o sistema conserva projeções anteriores (`app/docs/LEIS.md:122`) |

Não usar como defeito atual a antiga falta espúria após lote: a reavaliação agora também executa com `recalculo=False`. (`app/ponto/portas/celula.py:428`; `app/ponto/portas/celula.py:438`)

## 05/11 15:38 · sessao 1372e6b8a2c6 · - · - · origem=Ronald (tmux)

12 itens candidatos, todos por inspeção estática; não executei testes nem escrevi arquivos. As ocorrências de 720/480/660/420/240 nestes quatro arquivos são exemplos/comentários, não constantes executáveis de regra: `app/folha/porta_export.py:361`, `:362`, `:525`, `:527`, `:528`, `:553`.

| ID | Tipo | Texto e afirmação | Código e comportamento | Veredito | Certeza |
|---|---|---|---|---|---|
| E01 | d | `app/folha/models.py:97`: `invalidada_em` nulo significa “vale”. | `app/folha/models.py:134-137` escolhe somente o registro mais recente; `app/folha/export.py:570-575` também exige que esse registro não esteja invalidado. | **CONTRADIZ** | Alta, estática |
| E02 | a | `app/folha/views.py:213-214`: quem entra no TXT ganha `aprovado` no ato. | `app/folha/views.py:244-248` exclui do carimbo quem possui trava diferente de `conta_vazia`; `app/folha/views.py:268` monta o TXT com os mesmos itens. A exceção já está descrita no comentário seguinte, `:216-221`, mas a frase anterior ficou absoluta. | **CONTRADIZ** | Alta, estática |
| E03 | e | Docstring `app/folha/tests/test_export_59a.py:1-5`: 12x36 não deve emitir parcela de feriado; regra “SO de emissão”. | `app/folha/export.py:411-412` suprime apenas quando **não** há cláusula de dobra. O próprio teste `app/folha/tests/test_export_59a.py:48-50` espera emissão de 20 horas com `feriado_12x36_em_dobra=True`. | **NAO VALE MAIS** | Alta, estática |
| E04 | e | Docstring `app/folha/tests/test_rescisao_modulo_proprio.py:1-5`: demissão “DENTRO” da competência sai do TXT mensal como rescisão. | `app/folha/export.py:840-845` exclui qualquer colaborador com `data_demissao` preenchida; portanto também o caso descrito. O teste cobre demissão em 10/07 na competência 07, `app/folha/tests/test_rescisao_modulo_proprio.py:24-32`. O texto não afirma que datas externas devam entrar. | **AINDA VALE** | Alta, estática |
| E05 | d | `app/folha/models.py:85`: `reexportacao=True` quando gerado mais de uma vez. | `app/folha/models.py:178` verifica existência anterior por empresa/mês/ano; `:182-186` cria novo registro com esse resultado. | **AINDA VALE** | Alta, estática |
| E06 | d | `app/folha/models.py:86`: `conteudo` guarda cópia fiel do TXT entregue. | `app/folha/views.py:281-285` produz o conteúdo da resposta; `:297-300` passa a mesma variável ao registro. `app/folha/models.py:185-186` persiste-a sem transformação. Entrega efetiva ao cliente é [nao verificado]. | **AINDA VALE** no caminho de geração | Alta para o código |
| E07 | d | `app/folha/models.py:99`: motivo obrigatório ao invalidar. | `app/folha/models.py:151-153` remove espaços e levanta `ValueError` se vazio; `:158-162` grava o motivo. A validação ocorre antes do retorno idempotente de `:154-155`. | **AINDA VALE** pela porta `invalidar` | Alta, estática |
| E08 | d | `app/folha/models.py:343`: empresa sempre preenchida; não há retrato global. | `app/folha/models.py:341-343` define FK sem `null=True`; `:363-364` estabelece unicidade por empresa/mês/ano. Existência de dados antigos inválidos é [nao verificado]. | **AINDA VALE** no modelo | Alta, estática |
| E09 | a | Docstring com corte, `app/folha/export.py:375-378`: atraso e saída antecipada somam na mesma rubrica de desconto. | `app/folha/export.py:388` soma os dois valores; `:440-441` usa essa função para o evento `atraso_diurno`. O número efetivo de rubrica depende do cadastro lido em `:716-720`; configuração de produção é [nao verificado]. | **AINDA VALE** para a soma; 8069 em produção [nao verificado] | Alta para a soma |
| E10 | a | Lápide `app/folha/export.py:196-203`: férias/abono não são emitidos como eventos variáveis de férias. | A lista emitida em `app/folha/export.py:428-450` contém HE, folga trabalhada, adicional, intrajornada e atraso; faltas são acrescentadas separadamente em `:721-725`. Não existe evento de pagamento de férias nessa montagem. O que o Domínio calcula externamente é [nao verificado]. | **AINDA VALE** para a montagem local | Alta, estática |
| E11 | a | Comentário com corte, `app/folha/porta_export.py:14-15`: carimbo é por empresa, competência e dia; ontem não libera hoje. | `app/folha/porta_export.py:807-808` consulta empresa e data atual; `:811-817` recusa ausência ou competência diferente; `:818-821` recusa não medido ou falhas positivas. `app/folha/views.py:128-133` interrompe a geração quando recebe mensagem. | **AINDA VALE** | Alta, estática |
| E12 | a | Comentário vinculado à L-092, `app/folha/porta_export.py:417-427`: motor não roda sobre competência paga/exportada, divergência é irreparável e furo da exportada permanece nulo. | A restrição atual é específica a 09/2026: `app/ponto/services/fechamento.py:36-43`, `:551-555`. Fora dela, o fluxo grava fechamento em `:1121-1146` e lavra dinheiro/furo em `:1175-1211`. O texto genérico sobre toda exportada ficou superado. **Não usar isto para afirmar que 09 está liberada.** | **NAO VALE MAIS** como regra geral | Alta, estática |

Contraexemplos construtivos, sem alegar ocorrência em produção:

- **E01:** gere A e depois B para a mesma empresa/competência, sem chamar `invalidar` em A. A continua com `invalidada_em=None`, mas `A.e_vigente=False`; `app/folha/models.py:178-187`, `:134-137`.
- **E02:** item com `status='entra'` e `travas_aprovacao=('turno_aberto',)` permanece na montagem, porém é excluído da seleção de aprovação; `app/folha/views.py:244-248`, `:268`.
- **E03**, embora classificado como texto superado: fechamento 12x36 com 20 horas de feriado e cláusula de dobra verdadeira emite as 20 horas; há caso explícito em `app/folha/tests/test_export_59a.py:48-50`.

Ressalva para seleção: E09 usa docstring de produção que cita “corte”, enquanto a definição literal de (a) fala “comentários”. Se quiser restringir (a) exclusivamente a linhas `#`, substitua E09 por este:

**E09b — a, AINDA VALE, alta:** `app/folha/export.py:730-735` afirma que conta vazia não barra o carimbo do TXT; `app/folha/export.py:743` declara a exclusão e `app/folha/views.py:243-248` efetivamente remove essa trava antes de selecionar quem aprovar.

## 05/11 15:38 · sessao ffe78200000f · - · - · origem=Ronald (tmux)

**Achado 4 — fechado no caminho de aplicação do comando; gravidade: não procede manter o bloqueio anterior.** O comando abre `atomic` por pai, revalida pela fila com `select_for_update(pk=pai_pk)` e só então executa a costura dentro do bloco (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:94`, `:99`, `:100`, `:105`).

- A função que trava no fio chama-se `reapontar_resolvedora`/`_costura_nao_mata_o_fio`; `reatar_perguntas_ao_limbo` pertence a `limbo_folgas` e delega para ela (`app/chamados/services/limbo_folgas.py:224`, `:243`; `app/chamados/services/fio.py:784`, `:837`, `:840`).
- É a mesma linha `ChamadoColaborador(pk=pai.pk)`: a leitura interna usa `type(chamado).objects.filter(pk=chamado.pk).select_for_update()` quando dentro de transação (`app/chamados/services/fio.py:758`, `:760`, `:762`, `:763`).
- A ordem neste caminho é pai → pergunta; as perguntas são lidas sem trava explícita e atualizadas depois da trava do destino (`app/chamados/services/limbo_folgas.py:237`, `:238`, `:243`; `app/chamados/services/fio.py:837`, `:840`, `:845`, `:847`). O `atomic(savepoint=False)` interno permanece dentro da transação externa; o código mantém a trava até sair do `atomic` por pai (`app/chamados/services/fio.py:837`; `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:99`, `:105`). Auto-deadlock por readquirir a própria linha: não procede por essa estrutura; reprodução concorrente/ausência global de deadlocks: **[não verificado]**.
- O novo queryset evita o montador do painel: `_base` segue diretamente para `qs_vivos` e recebe apenas filtros de `status_local`/`silenciado_ate` (`app/chamados/services/painel.py:153`, `:154`, `:155`, `:186`, `:187`, `:188`). Os joins, `Case` e `Window` estão em `montar_qs_filtrado`, que não é chamado nessa leitura travada (`app/chamados/services/painel.py:96`, `:98`, `:100`, `:111`, `:115`; `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:100`). Portanto, a incompatibilidade temida não aparece na forma da query nova; compilação/execução real em PostgreSQL: **[não verificado]**. O modelo usa manager padrão e ordenação local `-criado_em` (`app/chamados/models.py:21`, `:97`, `:100`).

**Escritores dos campos que retiram o pai da fila.**

| Campo | Escritores | Trava |
|---|---|---|
| `silenciado_ate` | `arquivo.silenciar`: atribui e salva; `arquivo.levantar`: limpa e salva (`app/chamados/services/arquivo.py:166`, `:170`, `:201`, `:208`). O contrato lista esse arquivo como escritor (`app/core/tests/test_contract_escritores_estado.py:35`). | As portas não tomam `select_for_update` nem abrem `atomic` próprio; fazem `save(update_fields=...)` na própria linha do chamado (`app/chamados/services/arquivo.py:141`, `:170`, `:176`, `:208`). A concorrência desse UPDATE com FOR UPDATE da mesma linha serializa a escrita; comprovação executada: **[não verificado]**. |
| `status_local`, mudança de chamado existente | Núcleo `_gravar_estado`, atribuição e `save` (`app/chamados/models.py:195`, `:222`, `:274`, `:279`). Entradas: humana `_aplicar`, reconciliar, cancelar, analisar, reabrir, encerrar e encerrar por tranca (`app/chamados/models.py:433`, `:620`, `:751`, `:769`, `:1382`, `:1538`, `:1558`). | Reconciliação/cancelamento/encerramentos recarregam sob `select_for_update` dentro do `atomic` (`app/chamados/models.py:616`, `:638`, `:661`, `:674`, `:744`, `:745`, `:1531`, `:1532`, `:1547`, `:1548`). Humana/analisar/reabrir não possuem universalmente essa leitura travada; a gravação passa pelo `save` do núcleo (`app/chamados/models.py:193`, `:433`, `:763`, `:769`, `:1382`, `:279`). |
| `status_local`, nascimento/legado | Nascimento `abrir` define estado inicial e `criar` cria com estado; migrações antigas fazem backfill direto (`app/chamados/models.py:899`, `:1048`; `app/chamados/migrations/0038_a3_registrado_backfill.py:10`; `app/chamados/migrations/0039_a3_via_backfill.py:24`). | A trava do nascimento `abrir` é da pessoa, outra linha; nascimento não modifica o pai já existente (`app/chamados/models.py:863`, `:866`, `:867`). |

Os leitores efetivos da fila são `qs_vivos`/`qs_na_fila`: vivo, cobrança e silêncio futuro; `registrado` fica fora de `COBRANCA` (`app/chamados/services/painel.py:155`, `:187`, `:188`; `app/chamados/catalogo/motor.py:29`). O fio lê somente terminalidade do destino nesse gate, por isso a revalidação externa era necessária (`app/chamados/services/fio.py:764`, `:765`). O documento entrega a autoridade de estado ao motor e de vivo/cobra às constantes do catálogo, como o código usa (`app/docs/JUIZES.md:37`, `:51`, `:52`, `:53`; `app/chamados/services/painel.py:18`).

**Limite:** a mudança não garante que o pai permaneça para sempre na fila depois do commit; os escritores continuam podendo silenciá-lo ou mudar seu estado posteriormente (`app/chamados/services/arquivo.py:170`; `app/chamados/models.py:279`). Ocorrência real dessa sequência em produção: **[não verificado]**.

## 05/11 15:37 · sessao 8e6512a7b2ab · - · - · origem=Ronald (tmux)

Auditoria estática, sem executar testes ou consultar banco/logs. Sugestão de **11 itens** para consolidação: [nao verificado]

| ID local | Tipo | Texto afirma | Código faz | Veredito | Certeza |
|---|---|---|---|---|---|
| F1 | a | Guarda temporária barra escrita de 09/2026 nas empresas exportadas e permite leitura pura (`app/ponto/services/fechamento.py:27`, `:31`). | Guarda cadastrada em `:36-43`; `_exportadas` só é calculado em escrita/backfill (`:530-531`); colaboradores são excluídos em `:551-555`. | AINDA VALE | Alta, estática |
| F2 | a | Exportada não barra mais; correção entra em **qualquer competência**, sem degrau exportada/trancada/paga (`app/ponto/services/fechamento.py:476-482`, docstring com L-113). | A guarda posterior de 09/2026 remove do universo os colaboradores de empresas exportadas (`:36-43`, `:530-531`, `:551-555`). | NAO VALE MAIS | Alta, estática |
| F3 | a | Trancada significa “nada mais muda” (`app/ponto/services/fechamento.py:1440-1441`, comentário com corte). | O escritor grava números de fechamentos aprovados/exportados por `update` (`:1144-1174`); não consulta `competencia_trancada` nesse caminho. A tranca é consultada separadamente em `:1463-1464`. | CONTRADIZ | Alta, estática |
| F4 | a | Folga trabalhada recebe adicional de 100% **só com escala certa no dia** (`app/ponto/services/fechamento.py:917-918`, comentário com corte). | Filtra `periodos_ft` por `_certa(...)` em `:925-926`; soma somente os remanescentes em `horas_folga_trabalhada` em `:949-950`. | AINDA VALE | Alta, estática |
| F5 | a | Feriado trabalhado em escala que folga no feriado sai em HE100 feriado, separada de folga trabalhada (`app/ponto/services/fechamento.py:927-930`, corte/L-086). | Reconhece `feriado_vira_folga` em `:938-941`; remove da lista de folga em `:943-944`; acrescenta HE100 feriado em `:945-948`. | AINDA VALE | Alta, estática |
| F6 | a | Hora de folga sem escala certa permanece em horas trabalhadas, sem adicional de 100% (`app/ponto/services/fechamento.py:951-954`, corte/L-007). | Particiona por identidade, excluindo os conjuntos de folga certa e feriado (`:964-965`); soma as horas restantes em trabalhadas (`:966-969`). | AINDA VALE | Alta, estática |
| F7 | a | Crédito de ausência parcial paga abate atraso apenas do próprio dia (`app/ponto/services/fechamento.py:973-977`, corte). | Monta mapas diários e usa `abater_no_proprio_dia` (`:982-985`); a função usa `min(atraso_do_dia, credito_do_mesmo_dia)` (`app/ponto/services/credito_parcial.py:54-57`). | AINDA VALE | Alta, estática |
| F8 | a | Nenhum leitor usa ainda as linhas de DiaPago, pois a fatia S1 é aditiva (`app/ponto/services/fechamento.py:1179-1182`, bloco L-095). | Calendário chama `_linhas_do_periodo` (`app/colaboradores/services/calendario.py:290`), que consulta DiaPago (`app/ponto/services/dia_pago.py:852-854`); porta do export também consulta (`app/folha/porta_export.py:588-589`). | NAO VALE MAIS | Alta, estática |
| F9 | a | Escopo `colaborador_ids=[]` significa ninguém; filtro nunca amplia universo (`app/ponto/services/fechamento.py:502-506`, docstring com corte). | Parte de `universo_da_competencia`; aplica `.filter(pk__in=list(colaborador_ids))` quando argumento não é `None` (`:545-548`). | AINDA VALE | Alta, estática |
| F10 | a | Evento usa competência do fato, nunca mês civil (`app/ponto/services/fechamento.py:1780-1781`, corte). | Evento chama `competencia_do_fato` (`:1793-1794`); adaptador delega ao período da empresa (`:1578-1588`); janela respeita corte (`app/ponto/janelas.py:60-63`, `:24-33`). | AINDA VALE | Alta, estática |
| F11 | e | Universo inclui quem não está desligado e desligado com demissão dentro/depois do início da janela (`app/ponto/tests/test_fechamento_tela_escopo.py:40-44`, docstring DENTRO). | Predicado usa `~Q(situacao='desligado')` ou desligado com `data_demissao__gte` ao início da janela própria da empresa (`app/ponto/services/fechamento.py:422-432`). | AINDA VALE | Alta, estática |

**Contraexemplo de F3:** competência 10/2026 com `PeriodoFechado` sem reabertura e fechamento aprovado, colaborador dentro do universo e apuração bem-sucedida. `recalcular_fechamento_mes(10, 2026, colaborador_ids=[id])` alcança a escrita em `app/ponto/services/fechamento.py:1144-1174`; a guarda temporária só contém `(9, 2026)` em `:37`. A existência desse cenário em produção é [nao verificado].

**Exemplo demonstrativo de F2:** uma escrita de 09/2026 cujo escopo contenha colaborador de empresa exportada exclui esse colaborador em `app/ponto/services/fechamento.py:553-555`; leitura pura evita essa guarda por `_exportadas=[]` em `:530-531`. Empresas/colaboradores efetivamente atingidos são [nao verificado].

**Observação de escopo:** não encontrei os literais 720/480/660/420/240 como regra no escritor `app/ponto/services/fechamento.py` ou em `app/ponto/janelas.py` [nao verificado quanto a outros módulos]. O `help_text` com 720 está em `app/ponto/models.py:757`, no campo de Ausencia, e convém cruzá-lo com os itens do explorador de export antes de selecionar.

## 05/11 15:37 · sessao 454a4b4f98bc · - · - · origem=Ronald (tmux)

Encontrei um contraexemplo estático forte na correção por lote, com duas variantes. São entradas construídas para reprodução; ocorrência em produção e execução ponta a ponta: [não verificado].

**CE-B1 — 6x1: corrigir a saída mantém DiaPago anterior.**

Entrada concreta proposta: competência aberta, dia passado D, escala 6x1 `08:00–17:00`, intervalo `12:00–13:00`; batidas aprovadas `E08:00 S12:00 E13:00 S17:00`; lavra inicial com `minutos_realizados=480`, `horas_trabalhadas=8`. Depois, lote de Regularização Externa confirmado, sem disputa aberta, contendo somente `acao='corrigir'`, alvo `S17:00`, nova saída `16:00`. Valores iniciais e execução: [não verificado].

Caminho atual:

- A PWA encaminha a confirmação à porta `confirmar_e_materializar`: `app/chamados/views_regularizacao.py:191`; esta grava `confirmada` e chama o materializador: `app/chamados/services/regularizacao_ext.py:347`.
- A porta exige confirmação e recusa disputa aberta: `app/chamados/services/regularizacao_ext.py:51`, `app/chamados/services/regularizacao_ext.py:58`.
- O ramo corrigir escreve diretamente `tipo`, `timestamp` e `save`, sem chamar `registrar_batida` ou evento financeiro: `app/chamados/services/regularizacao_ext.py:149`, `app/chamados/services/regularizacao_ext.py:153`.
- O `post_save` recompõe núcleo e julga célula: `app/ponto/signals.py:34`, `app/ponto/signals.py:131`. A janela de julgamento é `(nova_data−1, nova_data)`: `app/ponto/signals.py:108`.
- A grade obtém realizado do juiz e publica `minutos_realizados`: `app/escala/utils.py:2066`, `app/escala/utils.py:2079`; a ata copia o número: `app/ponto/services/cartorio.py:696`, `app/ponto/services/cartorio.py:706`.
- O materializador conclui o lote e eventualmente encerra chamado, sem recalcular fechamento: `app/chamados/services/regularizacao_ext.py:197`, `app/chamados/services/regularizacao_ext.py:207`.
- A atualização financeira explícita existe na criação e na retratação, respectivamente `app/ponto/registro_batida.py:160`, `app/ponto/registro_batida.py:281`; a correção não passa por essas chamadas: `app/chamados/services/regularizacao_ext.py:149`.
- `DiaPago.minutos_realizados` recebe o mapa da grade somente na lavratura financeira: `app/ponto/services/fechamento.py:1196`, `app/ponto/services/dia_pago.py:286`.

Resposta deduzida: célula atual com **420 minutos**, DiaPago preservando **480 minutos / 8 horas**, até outro evento financeiro; o valor exato foi deduzido, não executado [não verificado]. A conta do realizado desconta o intervalo do período fechado: `app/ponto/turnos.py:738`, `app/ponto/turnos.py:754`, `app/ponto/turnos.py:765`. Pela L-095, a mudança deveria ser lavrada no evento e todos deveriam ler essa lavra; pela L-113, calcula-se o cadastro e as batidas existentes: `app/docs/LEIS.md:113`, `app/docs/LEIS.md:149`.

Leitores: espelho e app conservam a lavra antiga, pois leem DiaPago: `app/ponto/services/espelho.py:406`, `app/ponto/services/espelho.py:805`, `app/api/views.py:1539`. A comparação sem ambiguidade é **ata.minutos_realizados versus DiaPago.minutos_realizados**, ambos provenientes da grade: `app/ponto/services/cartorio.py:706`, `app/ponto/services/dia_pago.py:288`. Não confundir cru com pago em geral: a distinção está documentada em `app/ponto/services/espelho.py:398`.

**CE-B2 — a composição do lote muda a convergência financeira.**

Repita B1 em lote contendo apenas correções: não existe chamada de fechamento nesse ramo: `app/chamados/services/regularizacao_ext.py:124`, `app/chamados/services/regularizacao_ext.py:149`. Acrescente ao lote uma batida nova legítima da mesma competência: o ramo adicionar chama `registrar_batida`, que agenda o recálculo no `on_commit`: `app/chamados/services/regularizacao_ext.py:76`, `app/ponto/registro_batida.py:160`, `app/ponto/services/fechamento.py:1782`, `app/ponto/services/fechamento.py:1808`. Esse recálculo reprocessa a competência do colaborador, incluindo a correção anterior: `app/ponto/services/fechamento.py:1793`.

Assim, corrigir sozinho pode deixar a foto financeira antiga; corrigir junto com uma adição aciona sua atualização. Resultado executado e números de produção: [não verificado]. A divergência de propagação é estrutural, independentemente da qualidade da batida: `app/docs/LEIS.md:122`.

**CE-B3 — corrigir entre datas não reavalia a origem.**

Entrada construída: mesmo cadastro de B1; mover as quatro batidas de D para D+7 usando quatro itens `corrigir`, ambos dias passados dentro de competência aberta. Execução e valores: [não verificado]. Cada `save` envia ao cartório somente D+6/D+7; a origem D não integra a janela: `app/chamados/services/regularizacao_ext.py:149`, `app/ponto/signals.py:108`, `app/ponto/signals.py:135`. O receptor não guarda o timestamp anterior: `app/ponto/signals.py:132`. Portanto, a ata de D pode continuar declarando os 480 minutos anteriores, embora não restem batidas em D; confirmação de execução: [não verificado]. Aqui há duas lacunas: falta reavaliar a origem e falta propagar o dinheiro; L-099 exige coerência com o dado atual: `app/docs/LEIS.md:122`.

O TXT de produção usa, por padrão, `FechamentoMensal` gravado: `app/folha/export.py:681`, `app/folha/export.py:700`. Não afirmo emissão de TXT errado: classificação/gates e conteúdo efetivamente emitido nestes cenários [não verificado].

| Caso | Falha deduzida | Certeza | Dono |
|---|---|---|---|
| B1 | Correção muda ata; não agenda nova lavra financeira | Alta para ausência do evento; números executados [não verificado] | ESTRUTURA — `app/docs/LEIS.md:122` |
| B2 | Adicionar no mesmo lote agenda o recálculo que corrigir sozinho não agenda | Alta por caminhos distintos — `app/chamados/services/regularizacao_ext.py:76`, `app/chamados/services/regularizacao_ext.py:149` | ESTRUTURA — `app/docs/LEIS.md:122` |
| B3 | Correção entre datas julga destino e omite origem | Alta por janela explícita — `app/ponto/signals.py:108` | ESTRUTURA — `app/docs/LEIS.md:122` |

Testes não executados; tentativa de importar Django falhou porque o módulo não está instalado [não verificado]. Nenhum arquivo foi criado, editado ou apagado [não verificado]. [nao verificado]

## 05/11 15:37 · sessao 01e3b8424db0 · - · - · origem=Ronald (tmux)

**Confirmado, certeza alta:** falta720 → aplicação420 → DP edita a mesma ausência para atestado420 → reversão aceita e grava720 no atestado. A edição aceita atestado configurado como parcial, troca tipo/minutos e preserva status aprovada; a reversão não compara tipo. É lacuna remanescente do achado1, não regressão nova. (`app/ponto/migrations/0063_atestado_aceita_horas.py:20`, `app/ponto/services/ausencia.py:480`, `app/ponto/services/ausencia.py:497`, `app/ponto/services/ausencia.py:507`, `app/ponto/services/ausencia.py:544`, `app/ponto/management/commands/rederivar_falta_declarada.py:224`, `app/ponto/management/commands/rederivar_falta_declarada.py:226`, `app/ponto/services/ausencia.py:531`.)

Arquivos antigos sem `empresa_id` agora são recusados explicitamente: quebra de compatibilidade deliberada pela guarda; prejuízo financeiro decorrente [nao verificado]. (`app/ponto/management/commands/rederivar_falta_declarada.py:213`.)

## 05/11 15:36 · sessao 01e3b8424db0 · - · - · origem=Ronald (tmux)

- **Achado 1: fechado parcialmente.** Reversão agora valida existência, colaborador/data, minutos esperados e entrega antes de escrever; tudo é validado antes do loop de gravação. Entretanto, a empresa consultada vem exclusivamente do arquivo, sem comparar com a empresa atual do colaborador. Transferir um colaborador de A para B após aplicar, entregar B e manter A aberta permite restaurar720 na ausência atualmente pertencente a B. Certeza alta para esse caminho sintético; ocorrência em produção [nao verificado]. (`app/ponto/management/commands/rederivar_falta_declarada.py:219`, `app/ponto/management/commands/rederivar_falta_declarada.py:224`, `app/ponto/management/commands/rederivar_falta_declarada.py:228`, `app/ponto/management/commands/rederivar_falta_declarada.py:234`; transferência permitida por `app/colaboradores/services/colaborador.py:33`, `app/colaboradores/services/colaborador.py:55`.)

- **Achado 2: fechado.** Nome inclui selo, PID e oito caracteres UUID; abertura exclusiva `x` impede sobrescrita. Sem alvo retorna antes de criar arquivo. (`app/ponto/management/commands/rederivar_falta_declarada.py:145`, `app/ponto/management/commands/rederivar_falta_declarada.py:154`, `app/ponto/management/commands/rederivar_falta_declarada.py:157`.)

- **Achado 3: fechado parcialmente.** Atomic engloba saves/foto e há guarda de todas as competências do range, inclusive consulta posterior. Porém entrega não participa de trava comum: `competencia_entregue` consulta registros sem lock, enquanto `registrar` insere diretamente. Exportação pode registrar após a última consulta e antes do commit do comando. Na reversão não há segunda consulta depois das gravações. Certeza alta para possibilidade no código; execução concorrente real [nao verificado]. (`app/ponto/management/commands/rederivar_falta_declarada.py:140`, `app/ponto/management/commands/rederivar_falta_declarada.py:165`, `app/ponto/management/commands/rederivar_falta_declarada.py:185`, `app/ponto/management/commands/rederivar_falta_declarada.py:228`, `app/folha/export.py:580`, `app/folha/models.py:174`, `app/folha/models.py:182`.)

- **Censo versus estado travado:** somente minutos são comparados; empresa, tipo, status, data e range não entram nessa comparação. A edição usa o objeto novo, mas a guarda final percorre objetos antigos de `linhas`. Transferência A→B entre censo e trava, com mesma grade420 e competência B entregue, passa nas guardas feitas para A e rederiva a ausência em B. Esse caminho agrava o descarte8 e torna a nova prova `empresa_id` insuficiente; a dependência do cadastro atual já era preexistente. (`app/ponto/management/commands/rederivar_falta_declarada.py:167`, `app/ponto/management/commands/rederivar_falta_declarada.py:172`, `app/ponto/management/commands/rederivar_falta_declarada.py:174`, `app/ponto/management/commands/rederivar_falta_declarada.py:185`; `app/colaboradores/services/colaborador.py:33`, `app/colaboradores/services/colaborador.py:61`.)

- **Atomicidade dos testes:** `test_MORDE_reverter_e_tudo_ou_nada` sobreviveria à remoção isolada do `atomic`, pois a linha inválida é detectada antes de qualquer escrita; nenhuma falha é provocada durante a segunda gravação. O teste de range sobreviveria à remoção isolada da guarda antecipada, graças à guarda posterior e rollback; não confere o arquivo deixado. O teste do mesmo segundo sobreviveria à troca isolada de `x` por `w`, conservando UUID/PID; ele distingue a reversão da cura completa do nome/modo. (`app/ponto/tests/test_h06_reversao_guardas.py:120`, `app/ponto/tests/test_h06_reversao_guardas.py:130`, `app/ponto/tests/test_h06_reversao_guardas.py:147`, `app/ponto/management/commands/rederivar_falta_declarada.py:185`, `app/ponto/management/commands/rederivar_falta_declarada.py:219`, `app/ponto/management/commands/rederivar_falta_declarada.py:233`.)

- **Ordem de trava:** não afirmar que se trava exclusivamente Ausencia: seu `Meta.ordering` referencia `colaborador__nome`, implicando join, e as consultas usam `select_for_update` sem `of=('self',)`. Ciclo efetivo/deadlock [nao verificado]. Há caminho síncrono save→signal→cartório→emissor→disputa, enquanto a emissão também possui ordem Disputa→Colaborador; para falta inteira aprovada, a emissão pode ser calada pelo julgamento, portanto o ciclo exige reprodução. (`app/ponto/models.py:819`, `app/ponto/management/commands/rederivar_falta_declarada.py:166`, `app/ponto/management/commands/rederivar_falta_declarada.py:217`, `app/ponto/signals.py:143`, `app/ponto/services/cartorio.py:1989`, `app/ponto/services/cartorio.py:1740`, `app/ponto/management/commands/detectar_ausencias.py:801`, `app/chamados/services/disputa_emissao.py:467`, `app/chamados/services/disputa_emissao.py:504`, `app/chamados/models.py:866`.)

- **Arquivo e exceção:** o JSON é uma lista de objetos `{pk, empresa_id, colaborador_id, data, minutos_antes, grade}`, escrita antes do atomic e lida em `_reverter`; rollback deixa o arquivo existente, pois nenhuma remoção acompanha a exceção. A docstring ainda anuncia o nome antigo sem PID/UUID. (`app/ponto/management/commands/rederivar_falta_declarada.py:22`, `app/ponto/management/commands/rederivar_falta_declarada.py:154`, `app/ponto/management/commands/rederivar_falta_declarada.py:158`, `app/ponto/management/commands/rederivar_falta_declarada.py:165`, `app/ponto/management/commands/rederivar_falta_declarada.py:211`.)

- **Trilha:** falha continua podendo ser engolida com savepoint próprio e sem cancelar a escrita de negócio; atomic externo não altera essa política. (`app/core/observ.py:105`, `app/core/observ.py:124`, `app/core/observ.py:133`, `app/core/observ.py:139`.)

## 05/11 15:36 · sessao 8a918fb5d1e8 · QUEIMA-E-CONTRAEX · leitura · origem=Code

**Há contra-exemplos estruturais: a justificativa pode vencer na grade enquanto a falta continua no DiaPago/TXT; a precedência também pode escolher uma ausência diferente da cobertura.** Os caminhos estão em `app/escala/utils.py:1209`, `app/ponto/precedencia.py:396`, `app/folha/export.py:129` e `app/ponto/services/dia_pago.py:272`.

São entradas construídas a partir da implementação, **não casos identificados em produção**. Execução integrada, incidência em produção e cadastro vivo de CCT: **[nao verificado]**. A referência normativa usada é a escrita na cópia; a L-110 manda comparar com a regra aplicada ao cadastro, e a L-099 atribui à ESTRUTURA as contradições sob o mesmo cadastro. (`app/docs/LEIS.md:136`, `app/docs/LEIS.md:122`.)

Para tornar os valores concretos, adoto **D = 04/11/2026**, colaborador ativo, vínculo vigente, competência aberta, célula inicialmente `trabalha=True`, DNA correspondente aos horários abaixo e **batidas `[]`**. São parâmetros dos contra-exemplos; sua existência em produção é **[nao verificado]**. A célula fornece os marcos congelados e o previsto líquido vem desses marcos e pausas. (`app/escala/models.py:1124`, `app/escala/utils.py:2287`, `app/escala/utils.py:2290`, `app/escala/utils.py:2312`.)

| Cadastro de entrada | Previsto P em D | Fundamentação do cálculo |
|---|---:|---|
| 6x1: segunda a sexta 08–17, intervalo 12–13; sábado 08–12; domingo folga | 480 min | Override por dia e duração menos pausas: `app/escala/models.py:337`, `app/escala/utils.py:2293`, `app/escala/utils.py:2326` |
| 12x36 diurno: 07–19, intervalo 12–13; D declarado plantão | 660 min | Marcos do vínculo e duração menos pausas: `app/escala/models.py:1124`, `app/escala/utils.py:2293`, `app/escala/utils.py:2326` |
| 12x36 noturno: 19–07, intervalo 00–01; D declarado plantão | 660 min | A duração atravessa meia-noite; desconta a pausa: `app/escala/utils.py:2293`, `app/escala/utils.py:2326` |
| Personalizado sem intervalo: `horarios_por_dia={"2":{"hi":"08:00","hf":"10:00"}}`, sem `hii/hfi` | 120 min | Quarta-feira corresponde à chave `2`; marcos do override: `app/escala/models.py:225`, `app/escala/models.py:337`, `app/escala/utils.py:2293` |

O cadastro do cenário define `falta=DESCONTA`, `atestado=ABONA` e `ferias=ABONA`, correspondendo à semente do catálogo; o cadastro efetivo de produção é **[nao verificado]**. (`app/ponto/catalogo/ausencias.py:88`, `app/ponto/catalogo/ausencias.py:95`, `app/ponto/catalogo/ausencias.py:101`.)

1. **E1 — Falta → atestado aprovado: abono e desconto coexistem no mesmo dia.**

   **Entrada e ordem:** criar falta inteira de D, aprová-la com `minutos=0`; depois criar atestado inteiro de D, com documento e `minutos=0`, e aprová-lo. A falta deriva P minutos; atestado sobre falta passa pela exceção de justificativa. (`app/ponto/models.py:875`, `app/ponto/precedencia.py:127`, `app/ponto/services/ausencia.py:315`.)

   **Caminho:** criação consulta a precedência → aprovação altera a ausência alvo → cobertura escolhe o atestado pelo maior `(data, pk)` → grade passa a `ausencia/atestado`. A aprovação não rejeita a falta irmã nesse caminho. (`app/ponto/services/ausencia.py:377`, `app/ponto/services/ausencia.py:895`, `app/ponto/turnos.py:2757`, `app/ponto/turnos.py:2769`, `app/escala/utils.py:1231`, `app/escala/utils.py:1241`.)

   **Código:** sem batidas, a grade fornece **abono=P**. Paralelamente, o extrator encontra a falta ainda aprovada, conserva seus P minutos, o fechamento recebe `P/60` horas de falta e a lavratura transmite esse desconto ao DiaPago. (`app/ponto/services/dia_pago.py:385`, `app/ponto/services/dia_pago.py:394`, `app/folha/export.py:76`, `app/folha/export.py:129`, `app/ponto/services/fechamento.py:864`, `app/ponto/services/fechamento.py:987`, `app/ponto/services/fechamento.py:1189`, `app/ponto/services/dia_pago.py:272`.)

   | Escala | Resposta do código: abono **e** falta | Resposta da regra após justificar D |
   |---|---|---|
   | 6x1 | 480 min abonados **e 8 h de falta** | 480 min abonados, **0 h de falta** |
   | 12x36 diurno | 660 min abonados **e 11 h de falta** | 660 min abonados, **0 h de falta** |
   | 12x36 noturno | 660 min abonados **e 11 h de falta** | 660 min abonados, **0 h de falta** |
   | Personalizado sem intervalo | 120 min abonados **e 2 h de falta** | 120 min abonados, **0 h de falta** |

   Cada linha resulta dos dois caminhos acima; a regra define falta como dia previsto **sem justificativa**, e o contrato da exceção promete que o atestado ganha da falta na aprovação. (`app/docs/LEIS.md:120`, `app/ponto/precedencia.py:119`, `app/ponto/services/dia_pago.py:394`, `app/ponto/services/dia_pago.py:272`.)

   **Leitores divergentes:** grade/palavra do dia reconhecem atestado; DiaPago conserva desconto. O extrator TXT inclui D como falta; se o colaborador estiver classificado para entrar e houver rubrica de falta mapeada, o montador gera **1 dia de falta**, contra **0 esperado**. Emissão efetiva: **[nao verificado]**. (`app/ponto/services/dia_decidido.py:363`, `app/ponto/services/dia_decidido.py:428`, `app/folha/export.py:170`, `app/folha/export.py:207`, `app/folha/export.py:695`, `app/folha/export.py:721`.)

   **Certeza:** alta por leitura estática; execução **[nao verificado]**. **Dono: ESTRUTURA / L-099.** (`app/docs/LEIS.md:122`.)

2. **E2 — Falta → férias: férias vencem, mas a falta continua descontável.**

   **Entrada e ordem:** mesmos cadastros e batidas; primeiro falta aprovada em D; depois agendamento de férias cobrindo D, com período aquisitivo, saldo e fracionamento válidos. A primeira fração pode ser de 14 dias; os demais impedimentos precisam estar satisfeitos como entrada. (`app/ferias/models.py:350`, `app/ferias/models.py:361`, `app/ferias/models.py:373`.)

   **Caminho:** agendamento chama o veto; férias passam sobre falta porque abonam; o `save` materializa uma ausência aprovada de férias por dia, sem rejeitar a falta anterior. (`app/ferias/models.py:428`, `app/ponto/precedencia.py:127`, `app/ponto/catalogo/ausencias.py:101`, `app/ferias/models.py:434`, `app/ponto/services/ausencia.py:1186`.)

   **Código × regra:** férias vencem na precedência e na grade, mas o desconto continua pelo mesmo caminho de E1: **8/11/11/2 horas**, conforme a escala. A regra exige **zero falta no dia de férias**; o próprio export declara essa obrigação expressamente. (`app/ponto/precedencia.py:46`, `app/ponto/precedencia.py:396`, `app/escala/utils.py:1241`, `app/folha/export.py:129`, `app/ponto/services/dia_pago.py:272`, `app/folha/export.py:202`.)

   **Leitores divergentes:** grade/cobertura de férias × falta no DiaPago e no extrator TXT. Na inversão **férias→falta**, a falta é vetada; portanto as duas ordens não produzem o mesmo conjunto de registros aceitos. (`app/ponto/turnos.py:2769`, `app/folha/export.py:76`, `app/ponto/precedencia.py:145`, `app/ponto/precedencia.py:461`.)

   **Certeza:** alta estática; emissão e ocorrência **[nao verificado]**. **Dono: ESTRUTURA / L-099.** (`app/docs/LEIS.md:122`.)

3. **E3 — Falta com início anterior → atestado: precedência e grade escolhem vencedores diferentes.**

   **Entrada concreta:** 6x1 acima; falta aprovada de **03–04/11/2026**, 960 minutos; depois atestado inteiro aprovado somente em **04/11**, sem batidas. Ambos os dias da falta eram previstos; A sobre X é aceito. (`app/escala/utils.py:2293`, `app/ponto/models.py:875`, `app/ponto/precedencia.py:127`.)

   **Código:** `fatos_do_periodo` ordena ausências por `data` crescente; falta e atestado ficam na mesma faixa; o sort final compara somente a faixa. Assim, em D, a falta iniciada em 03/11 permanece primeira e vence como `tipo_do_dia='falta'`. Não há empate de data neste exemplo. (`app/ponto/precedencia.py:79`, `app/ponto/precedencia.py:273`, `app/ponto/precedencia.py:379`, `app/ponto/precedencia.py:396`.)

   **Outra resposta do código:** a cobertura ordena por `(data, pk)` e sobrescreve com a mais recente: escolhe o atestado iniciado em 04/11; a grade fica `ausencia/atestado`. O cartório, ao carimbar a precedência, recebe o vencedor oposto. (`app/ponto/turnos.py:2757`, `app/ponto/turnos.py:2769`, `app/escala/utils.py:1209`, `app/escala/utils.py:1241`, `app/ponto/services/cartorio.py:1948`.)

   **Regra:** a justificativa aprovada deve ganhar da falta, conforme o contrato explícito. **Leitores divergentes:** `ata.precedencia`/juiz de precedência × cobertura/grade. Efeito adicional em número de chamados: **[nao verificado]**. (`app/ponto/precedencia.py:119`, `app/ponto/services/cartorio.py:1950`, `app/escala/utils.py:1241`.)

   **Certeza:** alta estática, com datas distintas; execução **[nao verificado]**. **Dono: ESTRUTURA / L-099.** (`app/docs/LEIS.md:122`.)

4. **E4 — Falta → folga de calendário: folga na grade, falta no desconto.**

   **Entrada e ordem:** em qualquer um dos quatro cadastros, aprovar falta de D com P minutos; depois aplicar ao vínculo um calendário que declare D como folga. A tela troca o calendário e gera sua foto. (`app/ponto/models.py:875`, `app/escala/views.py:409`, `app/escala/views.py:411`.)

   **Caminho:** o gerador recria `FolgaDia` de calendário sem consultar o veto de ausência e chama a regeneração; o overlay transforma o dia em folga e impede que a falta o transforme novamente em falta. (`app/escala/folgas.py:169`, `app/escala/folgas.py:173`, `app/escala/folgas.py:192`, `app/escala/utils.py:1170`, `app/escala/utils.py:1231`.)

   **Código × regra:** grade **folga, sem jornada**; extrator conserva **P minutos de falta**, que seguem ao DiaPago, e inclui **1 dia** para o bloco de falta do TXT. A L-089 exige **zero efeito de desconto em folga**. (`app/escala/utils.py:1172`, `app/folha/export.py:129`, `app/ponto/services/fechamento.py:864`, `app/ponto/services/dia_pago.py:272`, `app/folha/export.py:149`, `app/folha/export.py:207`, `app/docs/LEIS.md:121`.)

   **Ordens inversas:** calendário→falta recusa a falta; falta→folga **manual** também recusa. O caminho que permite a coexistência é a foto de calendário. (`app/ponto/precedencia.py:514`, `app/escala/folgas.py:258`, `app/ponto/precedencia.py:461`.)

   **Certeza:** alta estática; execução **[nao verificado]**. **Dono: ESTRUTURA / L-099.** (`app/docs/LEIS.md:122`.)

5. **E5 — A falta já justificada, ou agora em folga, ainda reduz o direito de férias.**

   **Entrada:** período aquisitivo de **01/01–31/12/2026**, seis faltas diárias aprovadas em dias inicialmente previstos; depois justificar uma delas por E1 ou transformá-la em folga por E4. O contador consulta somente registros `tipo='falta', status='aprovada'`, sem resolver cobertura ou jornada. (`app/ferias/models.py:140`, `app/ferias/models.py:144`.)

   **Código × regra:** continua contando **6**, e `aplicar_art130` determina **24 dias**. Desconsiderando a falta justificada ou situada em folga, restariam **5**, correspondentes a **30 dias** na tabela local do art. 130. (`app/ferias/services.py:243`, `app/ferias/services.py:246`, `app/ferias/models.py:54`, `app/ferias/models.py:56`, `app/ponto/precedencia.py:119`, `app/docs/LEIS.md:121`.)

   **Leitores divergentes:** cobertura/grade de D × contador e direito do período aquisitivo. **Certeza:** alta estática; saldo efetivamente gravado em produção **[nao verificado]**. **Dono: ESTRUTURA / L-099.** (`app/ponto/turnos.py:2769`, `app/escala/utils.py:1231`, `app/ferias/models.py:144`, `app/docs/LEIS.md:122`.)

A matriz abaixo cobre **os seis pares nas duas ordens e os quatro trios nas seis permutações**, para o cadastro comum. `F` significa folga; na coluna **manual**, lançamento manual; na coluna **calendário**, materialização da foto. `X=falta`, `A=atestado`, `V=férias`. Resultado **`aceitos / vencedor na grade`**; símbolos tentados que não aparecem nos aceitos foram vetados. Após veto, tenta-se a operação seguinte. A/V são aprovados antes da próxima tentativa; férias têm os demais requisitos satisfeitos. As portas e a seleção estão em `app/escala/folgas.py:258`, `app/escala/folgas.py:173`, `app/ponto/precedencia.py:127`, `app/ponto/precedencia.py:461`, `app/ponto/precedencia.py:496`, `app/ponto/turnos.py:2769` e `app/escala/utils.py:1231`.

| Ordens tentadas | Folga manual: aceitos/grade | Folga de calendário: aceitos/grade | Sítio decisivo |
|---|---|---|---|
| FX; XF | F/F; X/X | F/F; XF/F | `app/ponto/precedencia.py:514`, `app/escala/folgas.py:173` |
| FA; AF | F/F; A/A | F/F; AF/A | `app/ponto/precedencia.py:514`, `app/escala/folgas.py:173` |
| FV; VF | FV/V; V/V | FV/V; VF/V | `app/ponto/precedencia.py:496`, `app/escala/folgas.py:173` |
| XA; AX | XA/A; A/A | XA/A; A/A | `app/ponto/precedencia.py:127`, `app/ponto/precedencia.py:461` |
| XV; VX | XV/V; V/V | XV/V; V/V | `app/ponto/precedencia.py:127`, `app/ponto/precedencia.py:461` |
| AV; VA | A/A; V/V | A/A; V/V | `app/ponto/precedencia.py:145` |
| FXA; FAX; XFA; XAF; AFX; AXF | F/F; F/F; XA/A; XA/A; A/A; A/A | F/F; F/F; XF/F; XAF/A; AF/A; AF/A | `app/escala/folgas.py:173`, `app/ponto/precedencia.py:514`, `app/escala/utils.py:1231` |
| FXV; FVX; XFV; XVF; VFX; VXF | FV/V; FV/V; XV/V; XV/V; V/V; V/V | FV/V; FV/V; XFV/V; XVF/V; VF/V; VF/V | `app/escala/folgas.py:173`, `app/ponto/precedencia.py:496`, `app/ponto/precedencia.py:461` |
| FAV; FVA; AFV; AVF; VFA; VAF | FV/V; FV/V; A/A; A/A; V/V; V/V | FV/V; FV/V; AF/A; AF/A; VF/V; VF/V | `app/ponto/precedencia.py:496`, `app/ponto/precedencia.py:514`, `app/escala/folgas.py:173` |
| XAV; XVA; AXV; AVX; VXA; VAX | XA/A; XV/V; A/A; A/A; V/V; V/V | XA/A; XV/V; A/A; A/A; V/V; V/V | `app/ponto/precedencia.py:127`, `app/ponto/precedencia.py:145` |

**“Vencedor na grade” não significa que os registros perdedores deixaram de descontar:** precisamente E1, E2 e E4 mostram que o extrator continua lendo faltas aprovadas. A assimetria manual **F→V permite / V→F recusa** é uma exceção documentada, não um defeito por si. (`app/folha/export.py:76`, `app/folha/export.py:129`, `app/ponto/precedencia.py:470`, `app/ponto/precedencia.py:496`.)

Folga **automática do ciclo/template** é outro caso: ela não veta A/V; falta em dia sem jornada é vetada. Atestado aprovado sobre essa folga transforma a grade em ausência e limpa células/orfas, efeito classificatório que o próprio índice registra como conflito com L-089. Divergência monetária adicional desse caso: **[nao verificado]**. (`app/ponto/precedencia.py:435`, `app/ponto/precedencia.py:538`, `app/escala/utils.py:1241`, `app/escala/utils.py:1272`, `app/docs/LEIS.md:173`.)

Não ficou demonstrado **app ≠ espelho admin** numericamente: a API consulta a fonte do espelho e seus totais. Número divergente de chamados para os exemplos: **[nao verificado]**. (`app/api/views.py:1358`, `app/api/views.py:1369`, `app/api/views.py:1431`.)

| CE | Código × regra | Leitores que divergem | Certeza | Dono |
|---|---|---|---|---|
| E1: X→A | Abono P **e falta P/60 h** × falta zero após justificativa (`app/ponto/services/dia_pago.py:394`, `app/ponto/services/dia_pago.py:272`, `app/ponto/precedencia.py:119`) | Grade/palavra × DiaPago/extrator TXT (`app/ponto/services/dia_decidido.py:428`, `app/folha/export.py:129`) | Alta estática; produção **[nao verificado]** | ESTRUTURA / L-099 (`app/docs/LEIS.md:122`) |
| E2: X→V | Falta preservada em férias × zero falta (`app/folha/export.py:76`, `app/folha/export.py:202`) | Grade de férias × DiaPago/extrator TXT (`app/escala/utils.py:1241`, `app/ponto/services/dia_pago.py:272`) | Alta estática; produção **[nao verificado]** | ESTRUTURA / L-099 (`app/docs/LEIS.md:122`) |
| E3: X anterior→A | Precedência escolhe X × justificativa deve vencer (`app/ponto/precedencia.py:396`, `app/ponto/precedencia.py:119`) | Ata de precedência × cobertura/grade (`app/ponto/services/cartorio.py:1950`, `app/ponto/turnos.py:2769`) | Alta estática; produção **[nao verificado]** | ESTRUTURA / L-099 (`app/docs/LEIS.md:122`) |
| E4: X→F calendário | Falta P minutos em folga × zero desconto (`app/folha/export.py:129`, `app/docs/LEIS.md:121`) | Grade folga × DiaPago/extrator TXT (`app/escala/utils.py:1231`, `app/ponto/services/dia_pago.py:272`) | Alta estática; produção **[nao verificado]** | ESTRUTURA / L-099 (`app/docs/LEIS.md:122`) |
| E5: seis faltas, uma coberta | Direito **24 dias × 30 dias** (`app/ferias/models.py:144`, `app/ferias/services.py:246`, `app/ferias/models.py:54`) | Cobertura/grade × direito aquisitivo (`app/ponto/turnos.py:2769`, `app/ferias/models.py:144`) | Alta estática; saldo real **[nao verificado]** | ESTRUTURA / L-099 (`app/docs/LEIS.md:122`) |

## 05/11 15:36 · sessao 674274a87afe · - · - · origem=Ronald (tmux)

Censo e testes por leitura estática; execução dos testes e resultados RED/GREEN: **[nao verificado]**. [nao verificado]

**`Ausencia.minutos`**

- Escrita: default no modelo; criação recebe e monta o valor; edição atribui; reversão atribui; `save` deriva falta aprovada com valor falso somando o juiz diário; mudança de status inclui o valor na persistência. (`app/ponto/models.py:755`, `app/ponto/services/ausencia.py:130`, `app/ponto/services/ausencia.py:343`, `app/ponto/services/ausencia.py:507`, `app/ponto/services/ausencia.py:531`, `app/ponto/models.py:877`, `app/ponto/models.py:885`, `app/ponto/services/ausencia.py:838`.)
- Leitura financeira: rateio; crédito parcial; abono/supressão do fechamento e do espelho. (`app/folha/export.py:121`, `app/ponto/services/credito_parcial.py:24`, `app/ponto/services/fechamento.py:665`, `app/ponto/services/fechamento.py:685`, `app/ponto/services/efeito_ausencia.py:44`, `app/ponto/services/efeito_ausencia.py:51`.)
- Leitura de apresentação/parcialidade: horas; juiz de parcialidade; cartão; API; admin; painel; serializações; relatório de atestados. (`app/ponto/models.py:866`, `app/ponto/turnos.py:2600`, `app/ponto/services/dia_decidido.py:488`, `app/ponto/services/dia_decidido.py:512`, `app/api/views.py:1878`, `app/ponto/admin.py:44`, `app/templates/ponto/partials/painel_ausencia.html:125`, `app/templates/ponto/partials/painel_ausencia.html:149`, `app/ponto/views.py:2347`, `app/ponto/views.py:2730`, `app/ponto/views.py:2768`, `app/ponto/views.py:2811`, `app/ponto/services/atestados_lote.py:128`.)
- Comando lê na seleção/censo, arquivo, comparação de estado e conferência pós-derivação. (`app/ponto/management/commands/rederivar_falta_declarada.py:72`, `app/ponto/management/commands/rederivar_falta_declarada.py:86`, `app/ponto/management/commands/rederivar_falta_declarada.py:158`, `app/ponto/management/commands/rederivar_falta_declarada.py:167`, `app/ponto/management/commands/rederivar_falta_declarada.py:177`, `app/ponto/management/commands/rederivar_falta_declarada.py:226`.)
- Escritor fora da porta introduzido pelo delta: nenhuma ocorrência identificada; comando chama edição/reversão na porta declarada. (`app/ponto/management/commands/rederivar_falta_declarada.py:174`, `app/ponto/management/commands/rederivar_falta_declarada.py:234`, `app/ponto/tests/test_chokepoint_ausencia_gate.py:65`.)

**Cada teste com a cura correspondente revertida** [nao verificado]

| Teste | Passaria? | [nao verificado]
|---|---|
| Recusa competência entregue | Não: exige `CommandError` e manutenção420; a guarda nova consulta entrega. (`app/ponto/tests/test_h06_reversao_guardas.py:90`, `app/ponto/management/commands/rederivar_falta_declarada.py:228`.) |
| Recusa estado alterado | Não: exige erro para300; comparação nova verifica grade. (`app/ponto/tests/test_h06_reversao_guardas.py:99`, `app/ponto/management/commands/rederivar_falta_declarada.py:226`.) |
| Reversão duas vezes | Não: segunda chamada encontra720 em lugar da grade420 e deve recusar. (`app/ponto/tests/test_h06_reversao_guardas.py:108`, `app/ponto/management/commands/rederivar_falta_declarada.py:226`.) |
| Reversão tudo ou nada | Não com `_reverter` antigo; **sim retirando apenas `atomic`**, pois o teste injeta estado inválido antes da chamada e todas as validações antecedem qualquer escrita. Não prova rollback diante de falha na segunda escrita. (`app/ponto/tests/test_h06_reversao_guardas.py:120`, `app/ponto/management/commands/rederivar_falta_declarada.py:216`, `app/ponto/management/commands/rederivar_falta_declarada.py:219`, `app/ponto/management/commands/rederivar_falta_declarada.py:233`.) |
| Duas aplicações no mesmo segundo | Não com nome antigo e modo`w`: exige dois arquivos e preservação da PK original. Se reverter literalmente todo o delta, `_selo` desaparece e o próprio mock falha antes da prova comportamental. (`app/ponto/tests/test_h06_reversao_guardas.py:132`, `app/ponto/tests/test_h06_reversao_guardas.py:137`, `app/ponto/tests/test_h06_reversao_guardas.py:140`, `app/ponto/management/commands/rederivar_falta_declarada.py:154`.) |
| Sem alvo não grava arquivo | Não: depende do retorno anterior à abertura. (`app/ponto/tests/test_h06_reversao_guardas.py:142`, `app/ponto/management/commands/rederivar_falta_declarada.py:145`, `app/ponto/management/commands/rederivar_falta_declarada.py:157`.) |
| Range cruza entregue | Não retirando a cura completa; **sim retirando somente a guarda prévia**, porque a pós-checagem dentro da transação ainda recusa e desfaz. Também passa retirando somente a pós-checagem, pela guarda prévia. (`app/ponto/tests/test_h06_reversao_guardas.py:146`, `app/ponto/management/commands/rederivar_falta_declarada.py:140`, `app/ponto/management/commands/rederivar_falta_declarada.py:165`, `app/ponto/management/commands/rederivar_falta_declarada.py:185`.) |
| Rescisão na linha | Não revertendo a chave nova do montador; **sim revertendo somente contador/template**, porque o teste só chama `montar_resumos` e inspeciona a chave. (`app/ponto/tests/test_h06_reversao_guardas.py:173`, `app/ponto/tests/test_h06_reversao_guardas.py:175`, `app/ponto/tests/test_h06_reversao_guardas.py:176`, `app/ponto/services/fechamento_lista.py:109`.) |

Os testes novos não provocam concorrência, divergência da foto, exceção no segundo save ou identidade col/data/empresa divergente. (`app/ponto/tests/test_h06_reversao_guardas.py:90`, `app/ponto/tests/test_h06_reversao_guardas.py:99`, `app/ponto/tests/test_h06_reversao_guardas.py:108`, `app/ponto/tests/test_h06_reversao_guardas.py:120`, `app/ponto/tests/test_h06_reversao_guardas.py:130`, `app/ponto/tests/test_h06_reversao_guardas.py:142`, `app/ponto/tests/test_h06_reversao_guardas.py:146`, `app/ponto/tests/test_h06_reversao_guardas.py:173`.)

**Classes e descartes**

- Segundo juiz preexistente: parcialidade abaixo480 versus minutos positivos nos leitores financeiros; delta não modifica esses predicados. Descarte5 não se agrava. (`app/ponto/turnos.py:2580`, `app/ponto/turnos.py:2600`, `app/ponto/services/fechamento.py:665`, `app/ponto/services/fechamento.py:685`.)
- Derivação paralela preexistente: total vem da soma diária da grade, leitor distribui uniformemente pelos dias; descarte6 não se agrava, e guarda nova restringe ranges entregues. (`app/ponto/models.py:885`, `app/folha/export.py:121`, `app/ponto/management/commands/rederivar_falta_declarada.py:140`.)
- Descarte7 não se agrava: foto divergente agora aborta dentro de atomic; auditoria continua best-effort, com savepoint próprio, podendo retornar`None`. (`app/ponto/management/commands/rederivar_falta_declarada.py:165`, `app/ponto/management/commands/rederivar_falta_declarada.py:193`, `app/core/observ.py:105`, `app/core/observ.py:133`, `app/core/observ.py:139`.)
- Descarte8 pode se agravar no novo caminho: reversão usa empresa do JSON sem compará-la à empresa atual do colaborador. Contraexemplo sintético: apply na E1 aberta, transferência do colaborador à E2, entrega da mesma competência na E2, reversão consulta somente E1 aberta e restaura720. Certeza alta quanto à seleção da empresa; incidência real **[nao verificado]**. (`app/ponto/management/commands/rederivar_falta_declarada.py:158`, `app/ponto/management/commands/rederivar_falta_declarada.py:224`, `app/ponto/management/commands/rederivar_falta_declarada.py:228`, `app/ponto/management/commands/rederivar_falta_declarada.py:234`.)
- Relógios internos preexistentes: retenção lê hoje; signal de ausência lê hoje e chama cartório; cartório lê hoje/now mesmo aceitando`agora`. (`app/folha/export.py:1197`, `app/ponto/signals.py:122`, `app/ponto/signals.py:147`, `app/ponto/signals.py:93`, `app/ponto/services/cartorio.py:1960`, `app/ponto/services/cartorio.py:1965`, `app/ponto/services/cartorio.py:1986`, `app/ponto/services/cartorio.py:1989`.)
- `_selo` novo lê relógio na borda de IO para nomear o arquivo; não participa do cálculo de competência/minutos/rescisão. (`app/ponto/management/commands/rederivar_falta_declarada.py:45`, `app/ponto/management/commands/rederivar_falta_declarada.py:47`, `app/ponto/management/commands/rederivar_falta_declarada.py:154`.)

## 05/11 15:36 · sessao 257accf54eca · QUEIMA-E-CONTRAEX · leitura · origem=Code

**Há contraexemplos estruturais na cópia.** A L-099 exige coerência com o cadastro existente; a L-095 exige lavratura no evento e leitura comum do dinheiro. (`app/docs/LEIS.md:122`, `app/docs/LEIS.md:113`)

Os casos abaixo são **entradas sintéticas sustentadas pela implementação**, sem atribuição a colaboradores de produção. Execução integrada nesta cópia, ocorrência em produção e conteúdo efetivamente emitido em TXT: **[não verificado]**.

**E1 — Responder `FALTA_DIA`: resumo muda para 12h, DiaPago conserva o valor anterior**

- **Entrada e ordem:** empresa CLT; colaborador ativo; vínculo vigente; célula de **04/11/2026** com trabalho previsto; nenhuma ausência anterior; zero batidas. Primeiro lavrar fechamento e DiaPago, com falta decidida **0h**; depois responder `FALTA_DIA` numa pergunta viva que oferece essa opção. Dia sem trabalho registrado pode ganhar linha de DiaPago pelo furo, sem falta decidida. (`app/ponto/services/dia_pago.py:272`, `app/ponto/services/dia_pago.py:277`, `app/chamados/models.py:3011`)
- **Cadastros concretos:** personalizado sem intervalo, `tipo_base='comercial'`, 08–12, intervalos nulos e `horarios_por_dia={'2': {'hi':'08:00','hf':'12:00'}}`, previsto **240 min**; ou 12x36 diurno 07–19, pausa 12–13, âncora em 04/11, previsto **660 min**. O personalizado lê dias e horários desse mapa. (`app/escala/models.py:335`, `app/escala/models.py:862`; cadastro diurno equivalente em `app/ponto/tests/test_o280_bateria_da_lei.py:1082`)
- **Caminho:** API → core da resposta → materializador de ausência. `FALTA_DIA` vira falta **aprovada**, e o materializador passa **`minutos=720`**. A porta conserva esse valor; `Ausencia.save()` só deriva o previsto quando os minutos estão vazios. (`app/api/views.py:2642`, `app/chamados/services/disputa_emissao.py:1327`, `app/chamados/services/materializacao.py:577`, `app/chamados/services/materializacao.py:519`, `app/ponto/services/ausencia.py:341`, `app/ponto/models.py:875`)
- **Código × regra:** falta lançada de **12h**, contra **4h** no personalizado ou **11h** no diurno. A L-088 manda cobrar o previsto daquele dia. (`app/chamados/services/materializacao.py:521`, `app/docs/LEIS.md:120`)
- **Leitores:** o resumo do espelho consulta fechamento online e passa a somar **12h**; a resposta percorre julgamento da célula e reconciliação de chamados, sem relavrar DiaPago nesse caminho. A linha anteriormente lavrada permanece com **0h de falta**. (`app/ponto/services/espelho.py:1283`, `app/relatorios/cartao_pela_celula.py:69`, `app/relatorios/cartao_pela_celula.py:115`, `app/ponto/services/fechamento.py:865`, `app/ponto/services/fechamento.py:1743`, `app/ponto/signals.py:143`, `app/ponto/signals.py:250`, `app/ponto/services/espelho.py:805`)
- **Certeza:** alta por leitura estática; execução **[não verificado]**. **Dono L-099: ESTRUTURA**, tanto pelo literal 720 quanto pela atualização incompleta no evento. (`app/docs/LEIS.md:122`)

**E2 — 12x36 diurno: pausa efetivamente gozada aparece como trabalho pago**

- **Entrada e ordem:** cadastro 07–19, pausa 12–13; lançar cronologicamente `09:30 E → 12:00 S → 13:00 E → 15:00 S`; depois apurar e lavrar. É exatamente o cenário F7c da bateria, cuja geometria forma um período fechado. (`app/ponto/tests/test_o280_bateria_da_lei.py:1752`, `app/ponto/tests/test_o280_bateria_da_lei.py:1786`)
- **Caminho:** `Motor12x36.montar_periodos()` delega ao pareador; `calcular_periodo()` encontra **330 min** de presença, mas só desconta a pausa dentro de `if duracao > 360`. Como 330 não passa, grava **330 trabalhados**, desconto de pausa **0**. (`app/ponto/motor_calculo_v2.py:2739`, `app/ponto/motor_calculo_v2.py:2761`, `app/ponto/motor_calculo_v2.py:2767`, `app/ponto/motor_calculo_v2.py:2785`)
- **Código × regra:** **330 × 270 min**. A pausa batida de 60 min deve sair do trabalho; o Art.71 §2 transcrito na bateria exige **330 − 60 = 270**. O juiz do realizado efetivamente subtrai essa pausa. (`app/ponto/tests/test_o280_bateria_da_lei.py:1741`, `app/ponto/turnos.py:754`, `app/ponto/turnos.py:765`)
- **Leitores:** o espelho alimenta o realizado pelo juiz, portanto **270 min**; a lavratura leva as horas do motor para DiaPago, portanto **5,5h**. O app, havendo linha lavrada, lê essas horas de DiaPago. A versão oráculo também preserva o envelope quando o motor declarou desconto zero. (`app/ponto/services/espelho.py:675`, `app/ponto/services/espelho.py:783`, `app/ponto/services/fechamento.py:1185`, `app/ponto/services/dia_pago.py:178`, `app/api/views.py:1539`, `app/ponto/services/espelho.py:120`, `app/ponto/calculador/alimentacao.py:124`)
- **Certeza:** alta por leitura e caso explícito ainda marcado `expectedFailure`; execução **[não verificado]**. **Dono L-099: ESTRUTURA** — mesma pausa reconhecida, duas contas incompatíveis. (`app/ponto/tests/test_o280_bateria_da_lei.py:1766`, `app/docs/LEIS.md:122`)

**E3 — 12x36 noturno: a última batida muda de dia e os leitores discordam da pausa**

- **Entrada e ordem:** cadastro 19–07, pausa 01–02, janela de HE ligada como na fixture; lançar `22:30 E → 04:30 S (+1) → 05:30 E (+1) → 10:30 E (+1)` e então apurar. (`app/ponto/tests/test_bob_dia_pago_se_contradiz.py:67`, `app/ponto/tests/test_bob_dia_pago_se_contradiz.py:89`, `app/ponto/tests/test_bob_dia_pago_se_contradiz.py:166`)
- **Caminho:** uma nova `E`, sem absorção como intervalo, encerra o turno anterior aberto e inicia outro; a atribuição de dia passa por `_data_do_turno`. A batida 10:30 fica no dia seguinte, impedindo a borda posicional do dia inicial. (`app/ponto/turnos.py:1400`, `app/ponto/turnos.py:1415`, `app/ponto/turnos.py:299`; caracterização específica em `app/ponto/tests/test_bob_dia_pago_se_contradiz.py:172`)
- **Código × regra:** o motor informa **360 min**; o juiz do realizado informa **300**, após subtrair a janela declarada de 60 min. O cenário de regra exige **660 min**: 22:30–04:30 = 360, mais 05:30–10:30 = 300, sem corte pela L-084 nesse caso. (`app/ponto/tests/test_bob_dia_pago_se_contradiz.py:170`, `app/ponto/tests/test_bob_dia_pago_se_contradiz.py:183`, `app/ponto/turnos.py:758`, `app/docs/LEIS.md:116`)
- **Leitores:** a cadeia motor/DiaPago carrega **360**; o realizado do espelho consulta o juiz que retorna **300**. São os dois resultados cravados pelo teste gêmeo, enquanto a expectativa de 660 permanece pinada. (`app/ponto/tests/test_bob_dia_pago_se_contradiz.py:168`, `app/ponto/tests/test_bob_dia_pago_se_contradiz.py:188`, `app/ponto/services/dia_pago.py:178`, `app/ponto/services/espelho.py:675`)
- **Certeza:** alta para o cenário da bateria e os ramos identificados; resultado integrado com outra ata alimentando o pareador **[não verificado]**. **Dono L-099: ESTRUTURA** — o tipo inadequado da última batida não explica duas respostas internas para o mesmo conjunto recebido. (`app/docs/LEIS.md:122`, `app/docs/LEIS.md:147`)

**E4 — 12x36 noturno em empresa CCT: desligar o switch aplica uma restrição reservada ao CLT declarado**

- **Entrada e ordem:** empresa `regime_trabalhista='cct'`, nenhuma aplicação vigente; 12x36 19–07, pausa 01–02; lançar as quatro batidas exatas, cronologicamente, e apurar com `regua_cct_ativa=0`. A fábrica possui explicitamente esse ramo desligado. (`app/core/regua_cct.py:428`, `app/core/regua_cct.py:449`)
- **Caminho:** com switch desligado, só empresa declarada CLT recebe o parâmetro de prorrogação; empresa CCT chega ao motor sem ele. O construtor decide pelo ciclo, e `prorrogacao_pos5h_legal('12x36')` retorna `False`. (`app/core/regua_cct.py:453`, `app/core/regua_cct.py:457`, `app/ponto/motor_calculo_v2.py:758`, `app/core/regua_cct.py:182`)
- **Código × regra:** código: **360 min noturnos de relógio / 411,43 legais**; régua preservada para CCT sem aplicação: **480 / 548,57**. A diferença são as duas horas 05–07; a pausa 01–02 sai nos dois resultados. A L-035 e `_regua_legal()` reservam a restrição ao CLT explicitamente declarado. (`app/ponto/motor_calculo_v2.py:110`, `app/ponto/motor_calculo_v2.py:983`, `app/ponto/motor_calculo_v2.py:990`, `app/core/regua_cct.py:185`, `app/core/regua_cct.py:210`, `app/docs/LEIS.md:83`)
- **Leitores:** há conflito demonstrável **código × régua interna**; divergência simultânea entre telas **[não verificado]**. Com o switch ligado, a fábrica consulta a régua e injeta sua resposta, alcançando o outro resultado para a mesma lacuna cadastral. (`app/core/regua_cct.py:458`, `app/core/regua_cct.py:466`)
- **Certeza:** alta por leitura estática; execução **[não verificado]**. **Dono L-099: ESTRUTURA** — a lacuna cadastral tem tratamento declarado, mas o ramo desligado não o preserva. (`app/core/regua_cct.py:204`, `app/docs/LEIS.md:122`)

**E5 — 6x1: editar o template muda a célula e deixa DiaPago com a previsão anterior**

- **Entrada e ordem:** 6x1 07–15:20, pausa 13–14, domingo de folga; lançar `07:00 E → 13:00 S → 14:00 E → 15:20 S`; lavrar fechamento e DiaPago com **440 min previstos**. Depois editar o template compartilhado para saída **16:20**, mantendo a pausa: novo previsto **500 min**. A porta calcula jornada como envelope menos intervalos. (`app/escala/services/cadastro_tipo.py:290`)
- **Caminho:** salvar a edição propaga regeneração, porém passa explicitamente **`recalculo=False`**; depois apenas invalida o fechamento. Na regeneração, julgamento antecipado e recálculo financeiro dependem dessa flag. (`app/escala/services/cadastro_tipo.py:307`, `app/escala/services/cadastro_tipo.py:348`, `app/escala/services/cadastro_tipo.py:357`, `app/ponto/portas/celula.py:360`, `app/ponto/portas/celula.py:384`)
- **Código × regra:** após o próximo julgamento do cartório, antes de recálculo financeiro, célula/ata informam **500 previstos**, enquanto DiaPago conserva **440**. Invalidar só limpa `derivado_em`; DiaPago recebe previsão nova apenas numa lavratura posterior. A L-099 exige coerência com o cadastro que o sistema tem. (`app/ponto/services/cartorio.py:704`, `app/ponto/services/fechamento.py:1636`, `app/ponto/services/dia_pago.py:286`, `app/docs/LEIS.md:122`)
- **Leitores:** divergência concreta entre **célula/ata e DiaPago**. O próprio caminho declara que o recálculo dessa edição continua manual; não há garantia de convergência no término do lançamento. (`app/escala/services/cadastro_tipo.py:339`)
- **Certeza:** alta para o intervalo de desatualização; execução **[não verificado]**. **Dono L-099: ESTRUTURA** — atualizar corretamente o cadastro não deve deixar suas projeções incompatíveis. (`app/docs/LEIS.md:122`)

| Caso | Escala | Resposta do código | Resposta exigida | Leitores/conflito | Certeza e dono |
|---|---|---|---|---|---|
| E1 | Personalizado sem intervalo | Falta 720 min; resumo 12h; DiaPago anterior 0h | 240 min / 4h | Resumo × DiaPago | Alta estática; ESTRUTURA. `materializacao.py:521`, `LEIS.md:120` |
| E1 | 12x36 diurno | Falta 720 min; resumo 12h; DiaPago anterior 0h | 660 min / 11h | Resumo × DiaPago | Alta estática; ESTRUTURA. `materializacao.py:521`, `LEIS.md:120` |
| E2 | 12x36 diurno | Motor/pago 330; realizado 270 min | 270 min | Realizado × DiaPago/app | Alta estática; ESTRUTURA. `motor_calculo_v2.py:2767`, `turnos.py:754` |
| E3 | 12x36 noturno | Motor 360; juiz 300 min | 660 min no cenário pinado | Motor/DiaPago × realizado | Alta na bateria; integração **[não verificado]**. `test_bob_dia_pago_se_contradiz.py:168`, `:183` |
| E4 | 12x36 noturno CCT | Noturno legal 411,43 min | 548,57 min pela régua preservada | Código × L-035 | Alta estática; ESTRUTURA. `regua_cct.py:449`, `LEIS.md:83` |
| E5 | 6x1 | Célula 500; DiaPago 440 previstos | Ambos acompanharem 500 | Célula/ata × DiaPago | Alta estática; ESTRUTURA. `cadastro_tipo.py:348`, `LEIS.md:122` |

Na tabela, os nomes abreviados pertencem aos arquivos completos citados em cada ficha. TXT emitido e estado final exato dos chamados nesses cenários: **[não verificado]**.

## 05/11 15:36 · sessao a93682f0e2e1 · BATERIA-QUEIXAS-ADMIN · leitura · origem=Code

**Q3 — gabarito normativo proposto: 25 casos, sem execução do sistema e sem alteração de arquivos. Cadastros, batidas e lançamentos abaixo são entradas hipotéticas [nao verificado].** A referência dos resultados é a regra aplicada às entradas, conforme L-110; a implementação não é o gabarito. `app/docs/LEIS.md:136`; `app/docs/CORTES.md:240`.

Para compactar as referências: **L** = [app/docs/LEIS.md](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/docs/LEIS.md:57); **C** = [app/docs/CORTES.md](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/docs/CORTES.md:240); **G** = [app/docs/GLOSSARIO.md](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/docs/GLOSSARIO.md:9). Assim, `L:75` significa `app/docs/LEIS.md:75`.

**Como ler os resultados.** Todos os valores são minutos: **P** previsto; **T** trabalho computável na jornada; **F** falta decidida; **A** abono; **N** trabalho noturno; **HE** extras autorizadas; **HP** pontas pendentes de decisão; **I** intervalo suprimido indenizável. T já contém a HE autorizada; N e I são rubricas separadas, não parcelas para somar novamente a T. O intervalo cadastrado sai da jornada; sua ausência não se transforma automaticamente em HE. `L:80`, `L:113`, `L:115`; `C:411`.

**“5 leitores”**, nas tabelas, significa: espelho do admin e app mostram o mesmo resultado; célula contém o correspondente julgamento; DiaPago reproduz seus valores; chamado/pergunta refletem as pendências desse mesmo julgamento. O chamado não precisa armazenar todos os números, mas não pode contradizê-los nem decidir novamente o dia. `L:60`, `L:95`, `L:113`, `L:122`; `G:22`.

Cada seta corresponde ao estado **depois do lançamento indicado**. Enviar resposta e validar resposta são etapas distintas: resposta sem veredito continua pendente; encerrar uma disputa não decide o dia. `C:272`, `C:342`.

**Cadastros comuns propostos:** vínculo vigente; competência aberta; sem isenção ou quarentena; dias sem feriado, salvo indicação; janela de HE ligada nas duas pontas, piso zero e teto de 60 minutos; sem outras ocorrências [nao verificado]. Horários apresentados em UTC−3; D é o dia inicial do turno e D+1 o seguinte. A jornada noturna pertence integralmente a D. `L:107`, `L:117`.

**6x1 — 44h semanais, 7h20 por dia**

Cadastro **S**: segunda a sábado, 07:00–15:20; intervalo 11:00–12:00; domingo de folga; regime CLT; `intervalo_indenizavel=True`, exceto S03 [nao verificado]. P = 500 − 60 = **440**, pela dedução do intervalo e pela grade, independentemente das batidas. `L:75`, `L:80`.

| id | cadastro | batidas locais | lançamentos, na ordem | resposta da regra | regra citada | leitores |
|---|---|---|---|---|---|---|
| S01 | S; dia previsto | 07:00, 11:00, 12:00, 15:20 | 1. Julgar. 2. Julgar novamente sem fato novo. | Nas duas vezes: **P440 T440 F0 A0 N0 HE0 I0**. | L-024/L-032: `L:75`, `L:80`; idempotência: `C:240`. | 5 leitores: mesmos valores; nenhuma cobrança de marco ausente. |
| S02 | S; intervalo indenizável | 07:00, 15:20 | 1. Julgar. 2. Colaborador informa pausa 11:00–12:00. 3. Admin valida o intervalo completo. | **P440 T440 F0 A0 N0 HE0** em todos os estados. **I60 → I60 → I0**. A simples resposta não retira a indenização. | L-031/032/033: `L:79`, `L:80`, `L:81`; resposta validada: `C:273`; HE0: `C:411`. | 5 leitores acompanham a mudança de I. Pergunta aguarda validação e depois perde o objeto; somente esse furo não bloqueia a folha. |
| S03 | S, mas `intervalo_indenizavel=False`; pré-assinalação | 07:00, 15:20 | 1. Julgar. 2. Receber resposta de intervalo. 3. Recusar a resposta com motivo. | Em todos: **P440 T440 F0 A0 N0 HE0 I0**. A recusa não transforma o intervalo pré-assinalado em HE ou indenização. | Política do cadastro: `L:79`; dedução: `L:80`; pergunta não bloqueante: `L:81`. | 5 leitores mantêm os números. Resposta recusada não aparece como validada; pendência continua sem duplicar o fato. |
| S04 | S; ausência de tipo que desconta | Nenhuma | 1. Dia encerrado sem decisão. 2. Colaborador responde “não trabalhei”. 3. Admin decide ausência descontável integral. | 1–2: **P440 T0 F0 A0 N0 HE0 I0**, dia em aberto. 3: **P440 T0 F440 A0 N0 HE0 I0**. Furo não era falta antes da decisão. | Falta decidida: `L:87`; furo versus falta: `L:120`; dia sem trabalho: `C:416`. | 5 leitores passam de “em aberto” para “falta decidida: 440”; nenhuma linha vazia ou cobrança para inventar trabalho. |
| S05 | S; parcial remunerada 07:00–08:00, preenchida pelo admin | 08:00, 11:00, 12:00, 15:20 | 1. Julgar. 2. Receber pedido de justificativa. 3. Admin informa o período e aprova o abono. | 1–2: **P440 T380 F0 A0 N0 HE0 I0**, déficit de 60. 3: **P440 T380 F0 A60 N0 HE0 I0**; esses 60 não podem continuar descontados como atraso. | Período explícito: `C:319`; cobertura limitada: `C:315`; abono residual: `C:277`; teto: `L:111`. | 5 leitores reconhecem **380 trabalhados + 60 abonados**. Aprovação da justificativa encerra seu chamado de origem. `C:362`. |
| S06 | S; ponta antecipada de 30 minutos | 06:30, 11:00, 12:00, 15:20 | 1. Julgar. 2. Admin autoriza os 30 minutos, com motivo. 3. Repetir o processamento do mesmo fato. | 1: **P440 T440 F0 A0 N0 HE0 HP30 I0**. 2–3: **P440 T470 F0 A0 N0 HE30 HP0 I0**. P continua 440. | Janela e autorização: `L:115`; previsto independente do realizado/idempotência: `C:240`. | 5 leitores: primeiro ponta riscada e pendente; depois **440 normais + 30 HE**. Exportação permanece bloqueada enquanto HP>0. |

**12x36 diurno — 07:00–19:00**

Cadastro **D**: regime CLT; 07:00–19:00; intervalo 12:00–13:00; intervalo indenizável; foto declara D como trabalho e D+1 como folga [nao verificado]. P = 720 − 60 = **660**; não 720. A fase depende da foto, e o intervalo sai da jornada. `L:73`, `L:75`, `L:80`.

| id | cadastro | batidas locais | lançamentos, na ordem | resposta da regra | regra citada | leitores |
|---|---|---|---|---|---|---|
| D01 | D; plantão previsto | 07:00, 12:00, 13:00, 19:00 | 1. Julgar. 2. Reprocessar sem mudança. | **P660 T660 F0 A0 N0 HE0 I0**, nas duas vezes. | `L:75`, `L:80`; `C:240`. | 5 leitores: mesmos valores, sem pergunta de marco. |
| D02 | D; atestado integral remunerado | Nenhuma | 1. Julgar sem decisão. 2. Receber atestado. 3. Admin aprova a ausência integral. | 1–2: **P660 T0 F0 A0 N0 HE0 I0**, em aberto. 3: **P660 T0 F0 A660 N0 HE0 I0**. Não criar trabalho fictício. | Furo não é falta: `L:120`; efeito no dia previsto: `L:121`; abono: `C:277`. | 5 leitores: **660 abonados, zero trabalhados**. A cobertura aprovada elimina a cobrança dos marcos abrangidos. |
| D03 | D; parcial remunerada 07:00–09:00 | 09:00, 12:00, 13:00, 19:00 | 1. Julgar. 2. Receber justificativa. 3. Admin declara o período e aprova. | 1–2: **P660 T540 F0 A0 N0 HE0 I0**, déficit 120. 3: **P660 T540 F0 A120 N0 HE0 I0**. | Parcial e período: `C:315`, `C:319`; pagamento do período: `C:336`, `C:353`. | 5 leitores: **540 + 120 = 660**; não registrar falta integral nem manter desconto dos minutos abonados. |
| D04 | D; abono integral aprovado coexistindo com trabalho | 08:00, 12:00, 13:00, 19:00 | 1. Julgar as batidas. 2. Aprovar abono abrangendo o dia inteiro. | 1: **P660 T600 F0 A0 N0 HE0 I0**. 2: **P660 T600 F0 A60 N0 HE0 I0**. O abono não pode ser 660: só faltavam 60 do previsto. | B-B21, abono somente do previsto não trabalhado: `C:277`; coerência: `L:122`. | 5 leitores mostram **600 trabalhados + 60 abonados**; preservam as batidas. O chamado acompanha os sinais restantes da ata, sem declarar que o trabalho desapareceu. `L:95`. |
| D05 | D; intervalo indenizável | 07:00, 19:00 | 1. Julgar. 2. Receber resposta de pausa. 3. Recusar. 4. Receber correção 12:00–13:00. 5. Validar. | Sempre **P660 T660 F0 A0 N0 HE0**. **I60 → 60 → 60 → 60 → 0**. | Cadastro: `L:79`; intervalo deduzido: `L:80`; validação/recusa: `C:273`; HE0: `C:411`. | 5 leitores conservam I60 até a validação. A pendência de intervalo não bloqueia a folha; não duplicar pergunta a cada tentativa. `L:81`; `C:273`. |
| D06 | D+1, folga expressamente declarada na foto | Nenhuma | 1. Julgar a folga. 2. Receber atestado para D+1. 3. Julgar seu efeito nessa data. | Em todos: **P0 T0 F0 A0 N0 HE0 I0**. O atestado não produz abono nem consome saldo nessa folga. | Foto/fase: `L:73`; ausência sem efeito em folga: `L:121`. | 5 leitores: “folga”, zero minutos; nenhuma cobrança de entrada, intervalo ou saída. `C:416`. |

**12x36 noturno — 19:00–07:00, CCT Vigilantes Londrina**

Cadastro **N**: CCT aplicável; D 19:00 até D+1 07:00; intervalo D+1 00:00–01:00; intervalo indenizável; foto com D de trabalho [nao verificado]. P = **660**. A hora é de **60 minutos**, com adicional noturno de **20%**, sem converter os minutos pela hora reduzida. `L:80`, `L:84`; `app/core/regua_cct.py:132`, `app/core/regua_cct.py:136`.

Nos casos N01–N06, declara-se **prorrogação após 05h desativada**; N07 é a variante ativada [nao verificado]. **lei:** a cl.38-d, isoladamente, não fornece essa decisão: é necessário explicitar a regra cadastrada da CCT. `L:83`.

| id | cadastro | batidas locais | lançamentos, na ordem | resposta da regra | regra citada | leitores |
|---|---|---|---|---|---|---|
| N01 | N; sem prorrogação após 05h | D 19:00; D+1 00:00, 01:00, 07:00 | 1. Julgar. 2. Reprocessar. | **P660 T660 F0 A0 N360 HE0 I0**. N = 420 da janela 22–05 − 60 de intervalo. Adicional de 20% sobre essas horas. | Janela/cadastro: `L:83`; hora normal/adicional: `L:84`; intervalo: `L:80`. | 5 leitores: **11h trabalhadas; 6h noturnas de 60 minutos**; tudo pertence a D. `L:117`. |
| N02 | N; pausa cadastrada 00–01, posteriormente declarada 05–06 | D 19:00; D+1 07:00 | 1. Julgar. 2. Colaborador informa pausa 05:00–06:00. 3. Admin valida essa pausa. | 1–2: **P660 T660 F0 A0 N360 HE0 I60**. 3: **P660 T660 F0 A0 N420 HE0 I0**. A pausa validada ficou fora de 22–05; T não muda, N muda. | Pausa deslocada no envelope: `L:80`; validação: `C:273`; mesma janela para intervalo e turno: `C:418`. | Os 5 leitores mudam juntos **N360→420 e I60→0**. Não basta encerrar o chamado e conservar o DiaPago anterior. `L:95`, `L:113`. |
| N03 | N; atestado integral remunerado para o plantão de D | Nenhuma | 1. Julgar. 2. Receber documento. 3. Admin aprova a ausência do plantão. | 1–2: **P660 T0 F0 A0 N0 HE0 I0**, em aberto. 3: **P660 T0 F0 A660 N0 HE0 I0**. N0 significa nenhum trabalho noturno efetivo; ver ressalva remuneratória abaixo. | Ausência no dia previsto: `L:121`; abono residual: `C:277`; dia inicial: `L:117`. | 5 leitores: abono660 em D; não lançar outra ausência nem outro plantão em D+1. |
| N04 | N; parcial paga D 23:00–D+1 01:00 | D 19:00, 23:00; D+1 01:00, 07:00 | 1. Julgar o trabalho. 2. Admin declara a parcial 23:00–01:00 e aprova. | 1: **P660 T600 F0 A0 N300 HE0 I0**, déficit60. 2: **P660 T600 F0 A60 N300 HE0 I0**. Das 2h declaradas, 1h já era intervalo: abona apenas a outra. N = 22–23 mais 01–05. | Parcial que cruza meia-noite: `C:330`; abono sem sobreposição: `C:277`; período: `C:315`; intervalo: `L:80`. | 5 leitores: **600 trabalhados + 60 abonados**, em D. Não abonar120 nem deslocar a parcial para D+1. |
| N05 | N; saída ausente | D 19:00; D+1 00:00, 01:00 | 1. Julgar. 2. Colaborador responde saída D+1 07:00. 3. Admin valida. | 1–2: **P660; F0 A0; “Em aberto”**, sem homologar T/N/HE finais. 3: **P660 T660 F0 A0 N360 HE0 I0**. Responder não equivale a validar. | Batida ímpar sem número: `C:278`; resposta sem veredito: `C:272`, `C:342`; valores finais: `L:80`, `L:83`, `L:84`. | Após validar, 5 leitores têm o resultado final e a pergunta perde o objeto. Antes, aplica-se a ressalva sobre zero técnico abaixo. |
| N06 | N; saída 30 minutos depois do marco | D 19:00; D+1 00:00, 01:00, 07:30 | 1. Julgar. 2. Admin autoriza os 30 minutos finais. 3. Reprocessar. | 1: **P660 T660 F0 A0 N360 HE0 HP30 I0**. 2–3: **P660 T690 F0 A0 N360 HE30 HP0 I0**. Sem prorrogação, 07–07:30 não aumenta N. | Pontas: `L:115`; prorrogação por cadastro: `L:83`; dia inicial: `L:117`. | 5 leitores: ponta pendente antes; depois **660 normais + 30 HE**, ainda em D. |
| N07 | N, mas CCT cadastrada concede prorrogação até 07h | D 19:00; D+1 00:00, 01:00, 07:00 | 1. Julgar. 2. Reprocessar. | **P660 T660 F0 A0 N480 HE0 I0**. N = 540 de 22–07 − 60 de intervalo, em horas de 60 minutos e adicional20%. | Prorrogação da CCT: `L:83`; cl.38-d: `L:84`; janela do intervalo: `C:418`. | 5 leitores: **8h noturnas**. A diferença para N01 vem do cadastro expressamente diferente, não do leitor. `L:122`. |

**Personalizado sem intervalo — exemplo de 6h corridas**

Cadastro **P6**: regime CLT; 08:00–14:00; jornada prevista360; intervalo cadastrado zero; `intervalo_indenizavel=True` [nao verificado]. Com intervalo cadastrado zero, L-032 não autoriza deduzir uma pausa fictícia. `L:80`.

**Esse cadastro não dispensa o intervalo legal.** Trabalho contínuo superior a quatro e até seis horas exige15 minutos; acima de seis horas, aplica-se o mínimo de uma hora, nas hipóteses destes casos. A supressão gera indenização do período suprimido com adicional mínimo de50%, sem criar HE por si só. CLT art.71, caput, §§1º/4º, [texto oficial](https://legis.senado.leg.br/norma/530547/publicacao/37353698); referência local integral desses dispositivos [nao verificado]. Política indenizável: `L:79`; distinção de HE: `C:411`.

| id | cadastro | batidas locais | lançamentos, na ordem | resposta da regra | regra citada | leitores |
|---|---|---|---|---|---|---|
| P01 | P6; trabalho contínuo sem pausa comprovado como entrada do caso | 08:00, 14:00 | 1. Julgar. 2. Reprocessar. | **P360 T360 F0 A0 N0 HE0 I15**. Não transformar T em345 nem I em HE15. | `L:79`, `L:80`; CLT71 §§1º/4º, referência local [nao verificado]. | 5 leitores:360 trabalhados e15 indenizáveis separados; o cadastro sem pausa não aparece como dispensa legal. |
| P02 | P6; ausência integral descontável | Nenhuma | 1. Julgar. 2. Colaborador responde “não trabalhei”. 3. Admin decide falta integral. | 1–2: **P360 T0 F0 A0 N0 HE0 I0**, em aberto. 3: **P360 T0 F360 A0 N0 HE0 I0**. | Falta decidida: `L:87`; furo não é decisão: `L:120`. | 5 leitores: falta360 somente depois da decisão; não usar ausência de intervalo para criar indenização sem trabalho. |
| P03 | P6; atestado integral remunerado | Nenhuma | 1. Julgar. 2. Receber atestado. 3. Admin aprova. | 1–2: **P360 T0 F0 A0 N0 HE0 I0**, em aberto. 3: **P360 T0 F0 A360 N0 HE0 I0**. | Efeito no dia previsto: `L:121`; abono residual: `C:277`. | 5 leitores:360 abonados, sem trabalho fictício ou indenização de intervalo trabalhado. |
| P04 | P6; parcial remunerada 08:00–10:00 | 10:00, 14:00 | 1. Julgar. 2. Receber justificativa. 3. Admin declara 08–10 e aprova. | 1–2: **P360 T240 F0 A0 N0 HE0 I0**, déficit120. 3: **P360 T240 F0 A120 N0 HE0 I0**. Trabalho efetivo de exatamente4h não ultrapassa o limite do §1º. | Parcial: `C:315`, `C:319`; abono: `C:277`; CLT71 §1º, referência local [nao verificado]. | 5 leitores: **240 + 120 = 360**; não aplicar I15 apenas porque o previsto era6h. |
| P05 | P6; data declarada como folga | Nenhuma | 1. Julgar. 2. Receber atestado para essa data. 3. Julgar o efeito. | Em todos: **P0 T0 F0 A0 N0 HE0 I0**; sem consumo de saldo da ausência. | Dia previsto como condição do efeito: `L:121`. | 5 leitores: folga, zero minutos; nenhuma pergunta de intervalo inventada para um dia sem trabalho. `C:416`. |
| P06 | P6; trabalho contínuo comprovado 08:00–14:30, sem pausa | 08:00, 14:30 | 1. Julgar com ponta pendente. 2. Admin autoriza30 minutos. 3. Reprocessar. | 1: **P360 T360 F0 A0 N0 HE0 HP30 I60**. 2–3: **P360 T390 F0 A0 N0 HE30 HP0 I60**. I60 decorre das6h30 contínuas comprovadas, antes da autorização interna da ponta. | Autorização da ponta: `L:115`; CLT71 caput/§4º, referência local [nao verificado]. | 5 leitores preservam I60; depois da autorização mostram **360 normais + 30 HE**. Não somar a indenização aos minutos trabalhados. |

**Propriedades fixas exigidas em todos os casos**

- **Partes somam o total, sem sobreposição.** Nos casos fechados apresentados, o previsto é coberto por trabalho dentro do previsto, abono e falta decidida. Durante a apuração, qualquer restante fica explicitamente pendente; não vira falta automaticamente. HE, N e I não podem ser somados novamente como se fossem trabalho adicional independente. `C:240`, `C:277`; `L:87`, `L:113`.
- **Idempotência.** Julgar duas vezes as mesmas entradas produz os mesmos minutos, estado e cobranças. Reentregar a mesma resposta ou evento não cria outro efeito pago ou outra pergunta para o mesmo fato. `C:240`, `C:273`, `C:333`; `G:27`.
- **Previsto independente do realizado.** Responder, validar, abonar ou autorizar HE não modifica P. Uma alteração válida da grade é outro fato, com sua própria trilha. `L:75`; `C:240`, `C:333`.
- **Todo leitor lê o mesmo resultado.** Conferir cada estado intermediário das tabelas, inclusive depois de recusa e reprocessamento. Espelho atualizado com DiaPago antigo, ou chamado resolvido com resposta ainda sem veredito, reprova a bateria. `L:60`, `L:95`, `L:113`, `L:122`; `C:272`.
- **Cobrança acompanha o fato.** Batida que resolve o marco absolve a cobrança na passada seguinte; o próprio chamado vivo não pode manter artificialmente a premissa do furo. `C:413`.
- **Trilha e pertencimento temporal.** Decisões guardam autor, motivo e antes/depois; a cauda depois da meia-noite continua no dia inicial. `L:65`, `L:117`; `C:330`, `C:333`.

**lei: pontos que o texto consultado não fecha completamente**

| ponto | consequência para a bateria | referência |
|---|---|---|
| Instante exato da sincronia | Há consumidor previsto a cada minuto e absolvição “na passada seguinte”. Não inventar garantia de atualização simultânea no mesmo instante do envio. É necessário fixar o limite entre resposta recebida, julgamento concluído e projeções atualizadas. | `C:351`, `C:413`; exigência de coerência: `L:122`. |
| Batida ímpar: “sem número” versus zero técnico | N05 deve continuar “Em aberto” antes da validação. L-103 manda zero declarado no fechamento sem par; L-102 e o corte R3 mandam apresentação sem número em determinadas situações. Falta explicitar o contrato comum dessa representação entre os cinco leitores; zero técnico não deve virar “0h homologadas”. | `L:128`, `L:129`; `C:278`. |
| CCT noturna sem cadastro de prorrogação | Não escolher silenciosamente entre N360 e N480 para 19–07 com pausa00–01. A cl.38-d resolve duração da hora e adicional; a prorrogação precisa da regra cadastrada. | `L:83`, `L:84`; `app/core/regua_cct.py:132`. |
| Remuneração noturna durante ausência abonada | N0 nos casos de ausência significa zero **trabalho noturno efetivo**. Os trechos consultados não bastam para decidir eventual manutenção remuneratória de adicional habitual no afastamento. | Alcance da cl.38-d: `app/core/regua_cct.py:132`; decisão remuneratória específica [nao verificado]. |

## 05/11 15:35 · sessao 2ec9cf527994 · QUEIMA-E-CONTRAEX · leitura · origem=Code

Os exemplos abaixo são **entradas construídas para esta cópia**, com cadastro, batidas e ordem explícitos. Reprodução integrada, ocorrência em produção e pessoas afetadas: [nao verificado]. O confronto usa as regras locais; enquadramento jurídico externo e CCT efetivamente aplicada: [nao verificado]. [nao verificado]

**1. Falta lançada antes do atestado continua descontando.**

Entrada: 12x36 noturno, plantão em 03/11/2026, 19h–07h, intervalo 02h–03h, célula de trabalho com **660 minutos previstos**, nenhuma batida. Primeiro lançar falta aprovada com minutos zero; depois lançar e aprovar atestado integral da mesma data, com documento e minutos zero. A falta deriva os 660 minutos, e a porta permite justificativa sobre falta existente. (`app/ponto/models.py:875`; `app/ponto/services/ausencia.py:309`; `app/ponto/precedencia.py:119`)

Caminho: cobertura escolhe o atestado pelo maior PK no empate de data → grade passa a ausência → cálculo da grade abona 660 minutos. Entretanto, a consulta de faltas continua somando a falta aprovada, sem excluir a vencida pelo atestado → fechamento e DiaPago recebem **11h de falta**. (`app/ponto/turnos.py:2753`; `app/escala/utils.py:1231`; `app/ponto/services/dia_pago.py:391`; `app/folha/export.py:129`; `app/ponto/services/fechamento.py:864`; `app/ponto/services/dia_pago.py:272`)

**Código:** abono de 660 minutos e desconto de 660 minutos coexistem no mesmo dia. **Regra:** atestado aprovado justifica a falta; falta exige ausência de justificativa. Divergem cobertura/grade e desconto do DiaPago; o emissor TXT também conserva a data da falta, condicionado à elegibilidade e à rubrica cadastrada. **Certeza alta por leitura; dono L-099: ESTRUTURA.** (`app/ponto/precedencia.py:119`; `app/docs/LEIS.md:120`; `app/folha/export.py:721`; `app/docs/LEIS.md:122`)

**2. Atestado de dias, lançado depois, perde porque começou antes.**

Entrada: 6x1, 08h–17h, intervalo 12h–13h, domingo de folga; nenhuma batida em 28/10/2026. Primeiro aprovar falta de 28/10; depois aprovar atestado integral de **27–29/10**, minutos zero. A sobreposição sobre falta é aceita pela porta. (`app/ponto/services/ausencia.py:309`; `app/ponto/precedencia.py:106`)

Caminho: cobertura ordena por **data inicial**, depois PK: a falta de 28/10 vence o atestado iniciado em 27/10, embora este tenha sido lançado depois. A grade fica `tipo_dia='falta'`. Já a precedência percorre datas crescentes e ordena apenas pela faixa; falta e atestado pertencem à mesma faixa, deixando o atestado primeiro. O cartório grava esse resultado em `ata.precedencia`. (`app/ponto/turnos.py:2753`; `app/escala/utils.py:1234`; `app/ponto/precedencia.py:273`; `app/ponto/precedencia.py:76`; `app/ponto/precedencia.py:396`; `app/ponto/services/cartorio.py:1947`)

**Código:** precedência diz atestado, grade diz falta e o desconto permanece **480 minutos**. **Regra:** aprovação do atestado deve justificar a falta. Divergem dois julgamentos usados pela própria célula. **Certeza alta por leitura; dono L-099: ESTRUTURA.** (`app/ponto/precedencia.py:119`; `app/folha/export.py:129`; `app/docs/LEIS.md:120`; `app/docs/LEIS.md:122`)

**3. Atestado de duas horas recebe abono de onze horas.**

Entrada: 12x36 diurno em 03/11/2026, 07h–19h, intervalo 12h–13h, previsto **660 minutos**, nenhuma batida. Depois lançar atestado de **120 minutos** e aprovar com período 07h–09h. A porta admite minutos pelo cadastro parcial e aceita declarar o período. (`app/ponto/services/ausencia.py:206`; `app/ponto/services/ausencia.py:863`; `app/ponto/services/ausencia.py:890`)

Caminho: grade marca `tipo_dia='ausencia'`, preservando `cobertura_parcial=True` e os marcos restantes; porém `por_dia_da_grade` calcula abono como **previsto − realizado**, sem consultar os 120 minutos nem a parcialidade. Para previsto 660 e realizado zero, devolve **660 de abono**; o mapa alimenta fechamento e DiaPago. (`app/escala/utils.py:1241`; `app/escala/utils.py:1251`; `app/escala/utils.py:1265`; `app/ponto/services/dia_pago.py:391`; `app/ponto/services/fechamento.py:1044`; `app/ponto/services/fechamento.py:1196`)

**Código:** registro de 120 minutos e cobertura parcial contra abono de 660 minutos. **Regra documentada:** parcial concede os minutos declarados, sem abonar o dia inteiro; restam **540 minutos sem cobertura**. A conta da função foi também chamada isoladamente em memória; chegada desse caso ao TXT [nao verificado]. **Certeza alta no cálculo local; dono L-099: ESTRUTURA.** (`app/ponto/services/efeito_ausencia.py:19`; `app/ponto/services/dia_pago.py:391`; `app/docs/LEIS.md:122`)

**4. Oito horas de atestado viram dia inteiro no 12x36.**

Entrada: mesma jornada diurna de 660 minutos; atestado de **480 minutos**, um dia, tipo habilitado para minutos. Ordem: lançar e tentar aprovar com período 07h–15h; alternativamente, aprovar sem período. (`app/ponto/services/ausencia.py:206`; `app/ponto/services/ausencia.py:890`)

Caminho: o juiz fixa dia inteiro em **480 minutos** e considera parcial apenas `0 < minutos < 480`. Por isso, com período, a aprovação responde **“So ausencia PARCIAL tem periodo.”** Sem período, a grade considera a ausência integral e apaga os marcos restantes; o cartório os dispensa. (`app/ponto/turnos.py:2580`; `app/ponto/turnos.py:2598`; `app/ponto/services/ausencia.py:871`; `app/escala/utils.py:1251`; `app/escala/utils.py:1272`; `app/ponto/services/cartorio.py:763`)

**Código:** recusa representar a cobertura parcial; se aprovado sem período, explica o dia inteiro. **Regra do parcial:** 480 minutos declarados não cobrem uma jornada líquida de 660. A mesma promoção alimenta o conjunto que zera atraso e saída antecipada. **Certeza alta na recusa e classificação; dono L-099: ESTRUTURA.** (`app/ponto/services/efeito_ausencia.py:19`; `app/ponto/turnos.py:2684`; `app/ponto/motor_calculo_v2.py:1092`; `app/docs/LEIS.md:122`)

**5. Falta inteira de sexta e sábado é distribuída igualmente, apesar das jornadas diferentes.**

Entrada: 6x1, domingo de folga; padrão 08h–17h, intervalo 12h–13h; sábado com `horarios_por_dia={"5":{"hi":"08:00","hf":"12:00"}}`. Nenhuma batida em **30–31/10/2026**. Lançar uma falta aprovada de dois dias, minutos zero. O cadastro suporta esse sábado reduzido e retira o intervalo no override sem `hii/hfi`. (`app/escala/models.py:193`; `app/escala/models.py:198`; `app/escala/models.py:336`)

Caminho: modelo soma **480 + 240 = 720 minutos**; export divide pelo número de dias e produz **360 + 360**. Esse mapa segue para a lavratura do DiaPago. (`app/ponto/models.py:875`; `app/folha/export.py:102`; `app/folha/export.py:110`; `app/ponto/services/fechamento.py:1184`; `app/ponto/services/dia_pago.py:272`)

**Código:** sexta com 6h de falta e sábado com 6h. **Regra:** custo do previsto **daquele dia**, portanto **8h e 4h**. O total mensal coincide, mas a distribuição diária contradiz o cadastro. **Certeza alta por leitura; dono L-099: ESTRUTURA.** (`app/docs/LEIS.md:120`; `app/docs/LEIS.md:122`)

**6. Personalizado sem intervalo apaga três horas de falta parcial — com conflito documental explícito.**

Entrada: personalizado, base comercial, segunda-feira 26/10/2026, 08h–12h, sem intervalo, célula de trabalho; batidas **11h E / 12h S**, nenhuma ausência. O personalizado aceita horários pelas chaves do mapa semanal. (`app/escala/models.py:326`; `app/escala/models.py:862`)

Caminho: comercial chama a pontualidade; entrada 180 minutos depois do previsto satisfaz o limiar; sem marcos de intervalo, o código elimina `prevista_entrada`. A tolerância recebe essa previsão ausente e o atraso fica zero. (`app/ponto/motor_calculo_v2.py:3293`; `app/escala/regua_defesa.py:498`; `app/ponto/motor_calculo_v2.py:2122`; `app/ponto/motor_calculo_v2.py:2142`; `app/ponto/motor_calculo_v2.py:2217`)

**Código:** atraso zero. **Texto de LEIS.md:** uma única ponta distante desconta normalmente; aqui seriam **180 minutos**, dentro do teto `240−60`. Entretanto, os comentários registram a exceção sem intervalo como recorte aprovado. Logo, este é um **contra-exemplo ao texto de LEIS.md**, com colisão documental; qual comando deve prevalecer [nao verificado]. Divergência entre leitores [nao verificado]. **Dono L-099: ESTRUTURA quanto à reconciliação código/regra.** (`app/docs/LEIS.md:116`; `app/docs/LEIS.md:111`; `app/ponto/calculador/regras.py:197`; `app/docs/LEIS.md:122`)

**7. Falta parcial de 30 minutos vira um dia no TXT.**

Entrada: personalizado sem intervalo, 14/10/2026, 08h–12h; batidas **08h E / 11h30 S**. Cadastro explícito de `saida_antecipada`: `efeito='desconta'`, `parcial=True`, `fim='dia_unico'`, ativo. Depois das batidas, lançar e aprovar 30 minutos. Essa combinação é aceita pelo cadastro e pela porta. (`app/ponto/views_tipos_ausencia.py:64`; `app/ponto/views_tipos_ausencia.py:76`; `app/ponto/views_tipos_ausencia.py:78`; `app/ponto/services/ausencia.py:206`)

Caminho: consulta de tipos descontáveis → rateio de 30 minutos → fechamento e DiaPago com **0,5h de falta**. O leitor do TXT transforma o registro em uma data; `montar_bloco_falta` usa `len(datas)` e emite **1 dia**, com registro `11`. (`app/ponto/catalogo/ausencias.py:331`; `app/folha/export.py:110`; `app/ponto/services/fechamento.py:864`; `app/ponto/services/dia_pago.py:272`; `app/folha/export.py:149`; `app/folha/export.py:204`; `app/folha/export.py:721`)

**Código:** palavra do admin/app informa desconto de 30min, DiaPago registra 0,5h, emissor TXT produz um dia de falta. **Regra local:** saída antecipada desconta em minutos. A emissão completa depende de `status='entra'`, fechamento admitido e rubrica; valor monetário no Domínio [nao verificado]. **Certeza alta, condicionada ao cadastro explícito; dono L-099: ESTRUTURA.** (`app/ponto/services/dia_decidido.py:219`; `app/ponto/services/espelho.py:340`; `app/api/views.py:1395`; `app/docs/CORTES.md:202`; `app/folha/export.py:697`; `app/folha/export.py:723`; `app/docs/LEIS.md:122`)

**8. Falta inteira ainda não lançada aparece simultaneamente como falta e pendência no PDF.**

Entrada: personalizado 08h–12h sem intervalo, célula de trabalho válida em 14/10/2026, nenhuma batida, ausência ou justificativa. Pedir PDF do período livre **14–14/10**, depois desse dia. O motor encontra um dia previsto sem trabalho e produz `dias_falta=1`, `horas_falta=4`. (`app/ponto/motor_calculo_v2.py:2595`; `app/ponto/motor_calculo_v2.py:2610`; `app/ponto/motor_calculo_v2.py:2618`)

Caminho: PDF copia a falta do motor; `folha_manda` só substitui os totais dentro do ramo de competência, mas calcula o dia em aberto também no período livre. A renderização conserva ambos os números. (`app/relatorios/pdf_espelho.py:461`; `app/relatorios/pdf_espelho.py:471`; `app/relatorios/cartao_pela_celula.py:321`; `app/relatorios/cartao_pela_celula.py:373`; `app/relatorios/cartao_pela_celula.py:418`)

**Código:** no mesmo PDF, **“Faltas 1 dia(s)” e “Em aberto 1 dia(s)”**. **Regra:** zero faltas decididas e um dia em aberto. **Certeza alta por leitura; dono L-099: ESTRUTURA.** (`app/relatorios/pdf_espelho.py:787`; `app/relatorios/pdf_espelho.py:791`; `app/docs/LEIS.md:87`; `app/docs/LEIS.md:120`; `app/docs/LEIS.md:122`)

Para **atestado sobre folga**, a implementação exige distinguir as entradas: sobre folga lançada, a porta veta; sobre folga do template, aceita e muda a classificação para ausência. Neste segundo caso, o previsto permanece zero e o abono calculado também é zero. Contradição monetária nesse cenário: [nao verificado]. (`app/ponto/precedencia.py:435`; `app/ponto/precedencia.py:514`; `app/escala/utils.py:1231`; `app/escala/utils.py:1822`; `app/ponto/services/dia_pago.py:391`)

| Caso | Escala / situação | Código × regra | Leitores envolvidos | Certeza / dono L-099 |
|---|---|---|---|---|
| 1 | 12x36 noturno; atestado depois da falta | Abono 660min **e** falta 660min × falta justificada | Grade, DiaPago, emissor TXT — `app/folha/export.py:129`; `app/ponto/services/dia_pago.py:391` | Alta estática; ESTRUTURA — `app/docs/LEIS.md:122` |
| 2 | 6x1; atestado de dias começando antes | Precedência atestado × grade falta | Dois julgamentos da célula — `app/ponto/precedencia.py:396`; `app/ponto/turnos.py:2757` | Alta estática; ESTRUTURA — `app/docs/LEIS.md:122` |
| 3 | 12x36 diurno; atestado120 | Abono660 × crédito120 | Registro/grade parcial, fechamento/DiaPago — `app/ponto/services/efeito_ausencia.py:19`; `app/ponto/services/dia_pago.py:394` | Alta local; ESTRUTURA — `app/docs/LEIS.md:122` |
| 4 | 12x36; atestado480 | Classificação integral × jornada660 | Porta, grade, dispensa da célula — `app/ponto/turnos.py:2580`; `app/ponto/services/ausencia.py:871` | Alta estática; ESTRUTURA — `app/docs/LEIS.md:122` |
| 5 | 6x1; sexta8h/sábado4h | Falta6h/6h × 8h/4h | Previsto diário, DiaPago — `app/folha/export.py:110`; `app/docs/LEIS.md:120` | Alta estática; ESTRUTURA — `app/docs/LEIS.md:122` |
| 6 | Personalizado sem intervalo | Atraso0 × texto exige180min | Código × LEIS; exceção documentada — `app/ponto/motor_calculo_v2.py:2142`; `app/docs/LEIS.md:116`; `app/ponto/calculador/regras.py:197` | Alta na colisão; precedência normativa [nao verificado]; ESTRUTURA |
| 7 | Personalizado; parcial descontável30min | DiaPago0,5h × TXT1dia | Admin/app, DiaPago, emissor TXT — `app/ponto/services/dia_decidido.py:219`; `app/ponto/services/dia_pago.py:274`; `app/folha/export.py:207` | Alta condicional; ESTRUTURA — `app/docs/LEIS.md:122` |
| 8 | Personalizado; falta inteira sem decisão | Faltas1 + aberto1 × faltas0 + aberto1 | Resumo e linha do PDF — `app/relatorios/pdf_espelho.py:787`; `app/docs/LEIS.md:87` | Alta estática; ESTRUTURA — `app/docs/LEIS.md:122` |
| Folga | Atestado sobre folga do template | Classificação muda; abono0 | Grade/abono — `app/escala/utils.py:1231`; `app/escala/utils.py:1822`; `app/ponto/services/dia_pago.py:394` | Erro monetário [nao verificado] |

_1662 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
