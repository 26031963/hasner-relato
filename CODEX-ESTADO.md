# CODEX-ESTADO

> Ultima resposta processada: **05/11/2026 16:00** · publicado em **05/11/2026 16:00** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 05/11 16:00 · sessao 2dfc52353f61 · - · - · origem=Ronald (tmux)

Achados de código; ocorrência em produção não demonstrada pelo anexo. Contraexemplos abaixo são **sintéticos/especulação rotulada**, não registros observados. [não verificado]

| Contador/achado | Arquivo:linha | Contraexemplo sintético e mecanismo | Certeza / dono L-099 |
|---|---|---|---|
| `crons_declarados_nao_instalados` e `crons_instalados_nao_declarados` | `bin/placar_code.sh:35-39`, `bin/crons.sh:27`, `bin/placar_code.sh:459-460` | Docker indisponível impede render; `crons.sh` imprime `render falhou`, sai 2; coletor ignora retorno e conta apenas linhas `> `/`< ` inexistentes: ambos **0**. | Alta / ESTRUTURA |
| `credencial_sem_trilha` | `bin/placar_code.sh:137-144`, `bin/placar_code.sh:203` | Imagem/container de teste indisponível ou teste falha antes de imprimir marcador; `sed` produz vazio, `${_cred:-0}` imprime **0**. Também contribui zero ao `TOTAL` em `:549`. | Alta / ESTRUTURA |
| `parado_citado_em_commit_posterior` | `bin/placar_code.sh:49-51`, `bin/tickets_placar.sh:54-55`, `bin/tickets_placar.sh:65-75`, `bin/tickets_placar.sh:89-91` | Git indisponível/raiz sem metadados produz arquivo temporário vazio; `commits=[]` e nenhum parado pode ter citação encontrada. Também falha total do script sem marcador vira zero no coletor. | Alta / ESTRUTURA |
| `deploy_sem_contrato` | `bin/placar_code.sh:146-148`, `bin/placar_code.sh:175` | `bin/contrato_deploy.sh` ausente/ilegível: execução falha, mas erro não tem prefixo exato `   - `; `grep -c` devolve `0`, que não aciona `${_deploy:-1}` porque `"0"` não é vazio. | Alta / ESTRUTURA |
| `prompts_repetidos` | `bin/placar_code.sh:394-397` | Arquivo `PROMPTS.md` ilegível/ausente ou `bc` ausente: pipeline sem valor e fallback imprime **0**. | Alta / ESTRUTURA |
| `testes_em_quarentena` | `bin/placar_code.sh:527-529` | Caminho absoluto de outra instalação não existe/está ilegível; `grep` falha, `|| echo 0` imprime **0** apesar de fonte indisponível. | Alta / ESTRUTURA |
| `crons_quebrados` — fonte perdida | `app/core/services/crons_quebrados.py:20-21`, `:29-38`; `app/core/management/commands/cron_quebrado.py:24-29` | Cron morreu com exit137; arquivo `BASE_DIR/logs/placar.jsonl` não montado/ilegível: qualquer `OSError` retorna `[]`; comando imprime `crons_quebrados=0`. Se só registro da quebra tem JSON/data inválida, é descartado e pode resultar igualmente em zero. | Alta / ESTRUTURA |
| `crons_quebrados` — registro incompleto | `app/core/services/crons_quebrados.py:41-47` | Registro do cron tem `nome`, `emp`, `ini` válidos e omite `exit`: `r.get('exit', 0)` interpreta ausência como sucesso, deixando quebra fora da lista. | Alta / ESTRUTURA |
| `TOTAL` do placar — erro convertido em zero | `bin/placar_code.sh:470-473`, `:549-554` | Falha do comando `cron_quebrado`: linha individual fica `?`, mas `_n_cq` recebe **0**. Se demais parcelas forem zero, placar anuncia `OK — nenhum contador ... aceso` e sai 0. Trata-se de falso zero do agregado, não da linha individual. | Alta / ESTRUTURA |
| `paginas_500_5min` | `bin/selo_paginas_500.sh:14-19` | `docker logs` falha nas duas cascas: erro não casa com regex, `grep -c` imprime 0 e `|| true` engole retorno; saída final **0**. Além disso, resposta HTTP **510** existe no universo “5xx” descrito em `:8`, mas regex `50[0-9]` de `:16` a exclui. | Alta / ESTRUTURA |
| `logins_aparelhos_so_falha`, `logins_falhando_pct` | `bin/vigia_logins.sh:45-55`, `bin/vigia_logins_conta.py:15-17`, `:24-38` | Falha de coleta vira entrada vazia e quatro contagens/percentual zero, terminando `OK` pelo piso. Outra entrada concreta sintética: `ip=2001:db8::1 POST /api/auth/login/ -> 401` não casa com regex IPv4, portanto tentativas/401 podem crescer enquanto aparelhos e percentual ficam zero. | Alta / ESTRUTURA |
| `horas_esteira_parada_com_fatia_pronta` | `app/config/esteira.py:22-35`, `:39-71`; `bin/placar_code.sh:509-510` | Fila parada há duas horas, mas arquivo de eventos sumiu: leitor retorna `[]`, acumulador inicia 0 e retorna `0.0`. JSON inválido e eventos sem `ts` são igualmente descartados (`:32-33`, `:45-48`). | Alta / ESTRUTURA |

Diretamente ligado ao anexo `JUIZES.md`, `copias_pergunta_2` imprime `len(copias("."))` em `bin/placar_code.sh:480-483`; o censo tem estas cegueiras:

- **Erro de sintaxe engolido:** `app/core/censo_pergunta2.py:64-68` retorna conjunto vazio para o arquivo inteiro. Contraexemplo sintético: arquivo contendo cópia do predicado e uma linha sintaticamente inválida no fim desaparece do censo. **Alta / ESTRUTURA.**
- **Isenção por função inteira:** `app/core/censo_pergunta2.py:43-59` e `:113-115` subtraem todas as funções de `OUTRA_PERGUNTA`. Contraexemplo sintético: `chamados/detector_anomalias.py::detectar_anomalias`, listado em `:44`, também passar a filtrar `EscalaColaborador` por início/fim: a cópia segue excluída. O próprio módulo admite o limite em `:16-18`. **Alta / ESTRUTURA.**
- **Detector sintático restrito:** `app/core/censo_pergunta2.py:23-24`, `:75-94` só vê keywords ORM literais ou comparações Python sobre atributos de um nome. Contraexemplo sintético válido: `filtros = {'data_inicio__lte': dia, 'data_fim__gte': dia}; EscalaColaborador.objects.filter(**filtros)` não oferece os keywords à AST nem compara atributos Python; pode escapar. **Alta / ESTRUTURA.**
- **Raiz vazia/inexistente:** `app/core/censo_pergunta2.py:98-110` inicia `out=set()` e usa `os.walk` sem validar universo; `copias` de raiz inexistente devolve vazio. Essa possibilidade pertence à função; o coletor atual faz `cd app` e, se esse `cd` falhar, imprime `?`, portanto não atribuir automaticamente esse falso zero à chamada atual do placar. `bin/placar_code.sh:482-483`. **Alta no helper / ESTRUTURA.**

Os caminhos de banco do placar fixam `juliani`, por exemplo `bin/placar_code.sh:59-64`, `:116-117`, `:470-471`. Isso prova escopo fixo; não prova que seja tenant errado. Qual tenant/schema deveria ter sido consultado e quais registros existem em outros tenants: **[não verificado]**.

Não marcar como fallback defeituoso os contadores de lastro: `bin/placar_code.sh:111-123` já converte medida ausente em `sem medida`/`?` e contribuição 1 ao total.

## 05/11 15:59 · sessao d28f14134098 · - · - · origem=Ronald (tmux)

Conclusões estáticas; execução dos testes/RED/GREEN, produção, sombra e ordem efetiva do pouso: **[nao verificado]**. [nao verificado]

**Descartes:**

- **D1 — não refutado pelo código, mas justificativa operacional [nao verificado].** O código recusa arquivo sem qualquer chave nova da identidade; isso inclui arquivos da v2 com `empresa_id`, mas sem `tipo/status/data_fim`. A inexistência de `logs/h06_falta` em produção não consta do dado raspado como resultado de sonda; é alegação da triagem. (`app/ponto/management/commands/rederivar_falta_declarada.py:224`, `app/ponto/management/commands/rederivar_falta_declarada.py:227`.)
- **D2 — motivo compatível com o aval, sem curar a classe.** O aval manda usar `janela_competencia` e separa MODO-COMUM bitemporal; o RELATO põe essa obra na frente. O cálculo permanece dependente do cadastro atual: janela lê corte atual, linha recalcula para o mês pedido. (`app/docs/PROMPTS.md:1860`, `app/docs/PROMPTS.md:1861`, `app/docs/RELATO.md:9`, `app/ponto/janelas.py:50`, `app/ponto/services/fechamento_lista.py:104`.) Exemplo ainda válido: demissão20/11, corte21 => rescisão; mudar corte16 => não rescisão histórica. (`app/ponto/janelas.py:16`, `app/folha/export.py:60`.)
- **D3 — motivo falso.** A view soma `r['rescisao']`, não a chave HTML `data-resc`; o teste renderiza diretamente o partial e não executa `fechamento_mensal`. Mutante `n_rescisao=0`, ou restauração da soma por mera presença de `data_demissao`, deixa o teste verde. (`app/ponto/views.py:1215`, `app/ponto/tests/test_h06_reversao_guardas.py:269`, `app/ponto/tests/test_h06_reversao_guardas.py:271`, `app/ponto/tests/test_h06_reversao_guardas.py:272`.)

**Cada teste v3: mordida real e mutante sobrevivente.** [nao verificado]

| Teste | Morde | Mutante que o deixa verde | [nao verificado]
|---|---|---|
| Tipo alterado, linha193 | Remover conferência de tipo da identidade permitiria voltar720 no atestado420, contrariando `assertRaises` e estado final. (`app/ponto/tests/test_h06_reversao_guardas.py:196`, `app/ponto/management/commands/rederivar_falta_declarada.py:241`.) | Retirar `status` e/ou `data_fim` de `CAMPOS_DO_ATO` continua verde: o caso só muda `tipo`. (`app/ponto/management/commands/rederivar_falta_declarada.py:44`, `app/ponto/tests/test_h06_reversao_guardas.py:196`.) |
| Empresa alterada, linha202 | Retirar confronto `emp_de` com empresa JSON permite retorno720 e quebra o teste. (`app/ponto/management/commands/rederivar_falta_declarada.py:244`, `app/ponto/tests/test_h06_reversao_guardas.py:206`.) | Remover trava da reversão continua verde, pois a transferência é serial e anterior à reversão. (`app/ponto/management/commands/rederivar_falta_declarada.py:231`, `app/ponto/tests/test_h06_reversao_guardas.py:206`.) |
| Falha na segunda gravação, linha212 | Retirar `atomic` da reversão deixa a primeira reposição720 persistida antes do `RuntimeError`, quebrando a expectativa420; serviço realmente chama `save`. (`app/ponto/management/commands/rederivar_falta_declarada.py:229`, `app/ponto/services/ausencia.py:532`, `app/ponto/tests/test_h06_reversao_guardas.py:219`, `app/ponto/tests/test_h06_reversao_guardas.py:228`.) | Remover `ausencia.save` da porta de reposição continua verde **neste teste**: primeira chamada não grava, segunda mockada lança, estado segue420. O teste não inspeciona a primeira escrita entre as duas chamadas. (`app/ponto/services/ausencia.py:532`, `app/ponto/tests/test_h06_reversao_guardas.py:223`, `app/ponto/tests/test_h06_reversao_guardas.py:228`.) |
| Arquivo existente, linha230 | `x`→`w` com nome idêntico agora é morto: há sentinela no nome determinístico e exige `FileExistsError`, sentinela e720 preservados. (`app/ponto/management/commands/rederivar_falta_declarada.py:164`, `app/ponto/tests/test_h06_reversao_guardas.py:233`, `app/ponto/tests/test_h06_reversao_guardas.py:239`, `app/ponto/tests/test_h06_reversao_guardas.py:241`.) | Trocar abertura exclusiva por `if exists: raise FileExistsError; open(...,'w')` continua verde, embora reintroduza janela de corrida. Não há concorrência nesse caso. (`app/ponto/tests/test_h06_reversao_guardas.py:237`, `app/ponto/tests/test_h06_reversao_guardas.py:240`.) |
| Mesma trava, linha245 | Retirar trava de `registrar` ou do apply elimina o SQL esperado. (`app/folha/models.py:181`, `app/ponto/management/commands/rederivar_falta_declarada.py:173`, `app/ponto/tests/test_h06_reversao_guardas.py:253`, `app/ponto/tests/test_h06_reversao_guardas.py:260`.) | Remover trava da **reversão**; remover guarda final da entrega no apply; ou travar uma Empresa errada mas existente. O teste só procura substring de tabela+`FOR UPDATE` e **alguma** consulta ED depois da primeira trava; não compara PK, não chama reversão e não tem duas transações concorrentes. (`app/ponto/tests/test_h06_reversao_guardas.py:249`, `app/ponto/tests/test_h06_reversao_guardas.py:259`, `app/ponto/tests/test_h06_reversao_guardas.py:261`, `app/ponto/management/commands/rederivar_falta_declarada.py:194`, `app/ponto/management/commands/rederivar_falta_declarada.py:231`.) |
| Tela, linha267 | Repor `data-resc` baseado apenas em presença de demissão quebra caso21/11. (`app/templates/ponto/_fech_linha.html:2`, `app/ponto/tests/test_h06_reversao_guardas.py:272`.) | Repor regra antiga apenas na **borda/badge**, ou `n_rescisao=0`, continua verde: só há asserts do atributo. Clonar predicado no produtor em vez de chamar juiz também passa, pois não há injeção de veredito divergente. (`app/templates/ponto/_fech_linha.html:2`, `app/templates/ponto/_fech_linha.html:4`, `app/ponto/views.py:1215`, `app/ponto/services/fechamento_lista.py:109`, `app/ponto/tests/test_h06_reversao_guardas.py:272`.) |

Os métodos herdados foram anulados só na subclasse; permanecem definidos e coletáveis na classe base, portanto `=None` não prova remoção daqueles testes da suíte. (`app/ponto/tests/test_h06_reversao_guardas.py:34`, `app/ponto/tests/test_h06_reversao_guardas.py:90`, `app/ponto/tests/test_h06_reversao_guardas.py:185`.)

**Classes no diff inteiro fornecido:**

- **Segundo juiz:** não identifiquei nova cópia do predicado de rescisão; a função extraída é chamada tanto pelo juiz TXT quanto pela linha. Janela e entrega chamam autoridades registradas. (`app/folha/export.py:54`, `app/folha/export.py:870`, `app/ponto/services/fechamento_lista.py:109`, `app/core/juizes.py:299`, `app/core/juizes.py:312`, `app/ponto/management/commands/rederivar_falta_declarada.py:62`, `app/ponto/management/commands/rederivar_falta_declarada.py:70`.)
- **Parcialidade preexistente:** UI nova passa a chamar `cobre_parte_do_dia`, porém financeiro continua com teste “minutos positivos” em tipos pagos, enquanto cobertura exige `<MINUTOS_DIA_INTEIRO`. Não foi curado pelo diff. (`app/ponto/views.py:2240`, `app/ponto/turnos.py:2598`, `app/ponto/services/fechamento.py:665`, `app/ponto/services/fechamento.py:685`.)
- **Segundo escritor Ausencia fora da porta:** o comando usa edição/reposição, a exceção reside no serviço e grava via `save`; não identifiquei gravação direta nova do comando. (`app/ponto/management/commands/rederivar_falta_declarada.py:183`, `app/ponto/management/commands/rederivar_falta_declarada.py:255`, `app/ponto/services/ausencia.py:518`, `app/ponto/services/ausencia.py:532`.) Outros escritores de ED/travas pertencem ao outro explorer.
- **Relógio:** `_selo` novo lê agora na borda de nome de arquivo; não participa de janela ou previsto. O alcance continua acionando relógios internos preexistentes em `Ausencia.save`, signals e cartório; não alegar pureza temporal de todo caminho. (`app/ponto/management/commands/rederivar_falta_declarada.py:54`, `app/ponto/management/commands/rederivar_falta_declarada.py:162`, `app/ponto/models.py:873`, `app/ponto/signals.py:122`, `app/ponto/services/cartorio.py:1965`, `app/ponto/services/cartorio.py:1986`.)
- **Derivação paralela:** passivo faz soma diária usando mesmo juiz de previsto que o modelo; há repetição da agregação para censo/conferência, sem nova aritmética diária. Rescisão compartilha predicado. Rateio uniforme preexistente permanece diferente de distribuir conforme grade diária. (`app/ponto/management/commands/rederivar_falta_declarada.py:87`, `app/ponto/models.py:885`, `app/ponto/management/commands/rederivar_falta_declarada.py:186`, `app/folha/export.py:60`, `app/folha/export.py:121`.)
- **Selo:** selo AST morde os literais diretos previstos e mapa antigo; não é detector de todo número cravado: `minutos=12*60` ou alias que não termine `criar_ausencia` passa por sua condição. Isso delimita o selo, não prova ocorrência produtiva. (`app/chamados/tests/test_h06_selo_minutos_sem_literal.py:36`, `app/chamados/tests/test_h06_selo_minutos_sem_literal.py:38`, `app/chamados/tests/test_h06_selo_minutos_sem_literal.py:71`.) Lacunas dos testes novos listadas acima; RED efetivamente executado **[nao verificado]**.
- **Auditoria:** porta promete trilha, mas `evento` segue best-effort com savepoint; falha de auditoria pode ser engolida sem desfazer negócio. Preexistente, não curado. (`app/ponto/services/ausencia.py:533`, `app/core/observ.py:105`, `app/core/observ.py:133`, `app/core/observ.py:139`.)
- **Pouso:** aval exige sombra09/10/11, preservação09 e pouso antes exportação11; código não demonstra execução dessas condições. (`app/docs/PROMPTS.md:1860`, `app/docs/PROMPTS.md:1861`.) Ordem efetiva, DIFF da sombra e hash09 **[nao verificado]**. Não há `.git` nesta cópia, portanto confirmação independente de hash/merge-base **[nao verificado]**.

## 05/11 15:59 · sessao 7b19689bbfab · - · - · origem=Ronald (tmux)

Censo em produção, somente leitura; execução e ocorrência dos contraexemplos em produção: [nao verificado].

| Campo do JSON | Quem produz/escreve | Quem lê |
|---|---|---|
| `pk` | `x.pk` no `json.dump`, escritor único do arquivo. (`app/ponto/management/commands/rederivar_falta_declarada.py:165`) | Validação de schema/diagnóstico, filtro das ausências, lookup e seleção para reposição. (`app/ponto/management/commands/rederivar_falta_declarada.py:224`, `:225`, `:232`, `:237`, `:255`) |
| `empresa_id` | `emp.pk`, empresa solicitada no comando, não extraída novamente do colaborador travado. (`app/ponto/management/commands/rederivar_falta_declarada.py:128`, `:165`) | Travas de empresas em ordem fixa, comparação com empresa atual do colaborador e consulta das competências entregues. (`app/ponto/management/commands/rederivar_falta_declarada.py:230`, `:231`, `:233`, `:244`, `:249`) |
| `colaborador_id` | `_identidade(x)` lê a ausência original; inclui o campo em `CAMPOS_DO_ATO`. (`app/ponto/management/commands/rederivar_falta_declarada.py:44`, `:47`, `:165`) | Guarda de identidade da reversão; empresa atual vem do colaborador da ausência travada. (`app/ponto/management/commands/rederivar_falta_declarada.py:233`, `:241`, `:244`) |
| `data`, `data_fim` | `_identidade` transforma datas em ISO, preservando `None`. (`app/ponto/management/commands/rederivar_falta_declarada.py:44`, `:48`, `:165`) | Comparação de identidade na reversão; competências são calculadas a partir da ausência atual, por `janela_atual`. (`app/ponto/management/commands/rederivar_falta_declarada.py:60`, `:62`, `:241`, `:249`) |
| `tipo`, `status` | `_identidade` copia os valores da ausência censada. (`app/ponto/management/commands/rederivar_falta_declarada.py:44`, `:48`, `:165`) | Comparação integral de identidade na reversão. (`app/ponto/management/commands/rederivar_falta_declarada.py:241`) |
| `minutos_antes` | `x.minutos`, número censado anterior ao ato. (`app/ponto/management/commands/rederivar_falta_declarada.py:165`) | Passa para a porta e vira `int(minutos_antes)` na ausência. (`app/ponto/management/commands/rederivar_falta_declarada.py:255`, `app/ponto/services/ausencia.py:531`) |
| `grade` | `g`, soma de chamadas ao juiz diário `minutos_previstos_do_dia`. (`app/ponto/management/commands/rederivar_falta_declarada.py:85`, `:87`, `:165`) | Exige `a.minutos == it['grade']` antes da reversão. (`app/ponto/management/commands/rederivar_falta_declarada.py:247`) |

Todos esses campos são exigidos pelo schema; o arquivo inteiro é carregado por `_reverter`. (`app/ponto/management/commands/rederivar_falta_declarada.py:220`, `:221`, `:224`, `:225`, `:227`.)

O apply compara identidade e minutos do censo com a ausência travada, e compara o valor derivado com `g`; o JSON não é relido no apply. (`app/ponto/management/commands/rederivar_falta_declarada.py:175`, `:176`, `:186`.)

`Ausencia.minutos`:

- Escritores: default0 no modelo; criação recebe argumento e salva; edição atribui e salva; reversão atribui e salva; modelo deriva falta aprovada sem número; mudança de estado inclui a derivação na persistência. (`app/ponto/models.py:755`, `:756`, `:877`, `:885`, `:887`; `app/ponto/services/ausencia.py:130`, `:343`, `:385`, `:418`, `:507`, `:508`, `:531`, `:532`, `:838`, `:839`.)
- Fontes de entrada pela porta: materializações agora omitem minutos; DP, submissão do colaborador, edição, API, regularização, abandono e férias fornecem o argumento ou deixam default. (`app/chamados/services/materializacao.py:484`, `:520`; `app/ponto/views.py:2412`, `:2414`, `:2533`, `:2536`, `:2791`, `:2798`; `app/api/views.py:1820`, `:1824`; `app/ponto/services/regularizacao.py:119`; `app/ponto/management/commands/marcar_abandono_provisorio.py:80`; `app/ponto/services/ausencia.py:1206`, `:1208`.)
- Leitores financeiros/cobertura: rateio de falta, créditos parciais, fechamento, efeito de ausência e juiz da parcialidade. (`app/folha/export.py:121`; `app/ponto/services/credito_parcial.py:24`, `:25`; `app/ponto/services/fechamento.py:665`, `:669`, `:685`; `app/ponto/services/efeito_ausencia.py:44`, `:46`, `:51`; `app/ponto/turnos.py:2600`.)
- Leitores de apresentação: horas do modelo, cartão/palavra do dia, API, admin somente leitura, painel, serializações e relatório de atestados. (`app/ponto/models.py:866`; `app/ponto/services/dia_decidido.py:488`, `:512`; `app/api/views.py:1878`; `app/ponto/admin.py:37`, `:38`, `:44`; `app/templates/ponto/partials/painel_ausencia.html:125`, `:149`, `:299`, `:313`; `app/ponto/views.py:2347`, `:2730`, `:2768`, `:2811`; `app/ponto/services/atestados_lote.py:128`.)
- O comando lê minutos no filtro, censo, excesso, arquivo, comparação anterior/posterior e guarda da reversão. (`app/ponto/management/commands/rederivar_falta_declarada.py:80`, `:93`, `:134`, `:165`, `:175`, `:182`, `:186`, `:247`.)

`ExportacaoDominio`:

- `registrar` cria o registro sob `transaction.atomic()` e trava da Empresa; grava empresa, competência, autor, nome, hash, linhas, reexportação e conteúdo. (`app/folha/models.py:174`, `:179`, `:181`, `:186`, `:187`, `:188`, `:189`, `:190`.)
- `invalidar` escreve `invalidada_em`, `invalidada_motivo` e `substituida_por`, sem tomar `travar_entrega` na própria função. (`app/folha/models.py:143`, `:158`, `:159`, `:161`, `:162`.)
- Chamadores de criação: view e comando de regeração; o comando também chama `invalidar`. (`app/folha/views.py:297`; `app/folha/management/commands/regerar_txt_dominio.py:118`, `:122`.)
- Leitores: vigência individual/lista, juiz de entrega, listas/histórico/download/PDF, haiku, vigia e levantamento de competências para geração de células. (`app/folha/models.py:134`, `:216`; `app/folha/export.py:593`, `:595`; `app/folha/views.py:445`, `:522`, `:554`, `:827`; `app/folha/pdf_export.py:55`; `app/folha/haiku_export.py:67`, `:99`, `:100`, `:157`; `app/folha/services/vigia_txt.py:201`; `app/ponto/management/commands/gerar_celulas.py:161`.)

**Bloqueante residual: transferência entre censo e apply.** O censo seleciona colaborador na empresa A; após transferência já concluída para B, o apply trava A e busca ausência apenas por PK, comparando identidade/minutos que não incluem empresa atual. Assim pode editar ausência hoje em B e conferir entrega apenas de A. A reversão nova contém a comparação de empresa atual, mas o apply não. (`app/ponto/management/commands/rederivar_falta_declarada.py:79`, `:82`, `:173`, `:174`, `:175`, `:176`, `:183`, `:194`, `:195`, `:233`, `:244`; campos de identidade em `:44`.) A porta permite alterar empresa pela operação `posto`. (`app/colaboradores/services/colaborador.py:33`, `:55`, `:60`, `:61`.)

Exemplo estático: censo A/falta720/grade420 → transferência para B entregue, mantendo mesma grade → apply trava A, passa identidade/minutos, grava420 e só pergunta entrega de A. Ocorrência em produção: [nao verificado]. (`app/ponto/management/commands/rederivar_falta_declarada.py:79`, `:87`, `:173`, `:175`, `:183`, `:186`, `:194`.)

Cuidado ao afirmar corrida na leitura de empresa da reversão: `emp_de` não usa `select_for_update`, porém a consulta anterior de `Ausencia` pode travar também Colaborador pelo JOIN implícito de `ordering = ["-data", "colaborador__nome"]`; SQL efetivo [nao verificado]. (`app/ponto/management/commands/rederivar_falta_declarada.py:232`, `:233`; `app/ponto/models.py:819`.)

## 05/11 15:59 · sessao ba6e517d9526 · - · - · origem=Ronald (tmux)

Achados estáticos, todos com dono **ESTRUTURA**; ocorrência, frequência e impacto em produção **[não verificado]**.

1. **Duas disputas abertas no primeiro nascimento — certeza alta.** `_abrir_ou_agregar_disputa` consulta somente a disputa existente com `select_for_update` (`app/chamados/services/disputa_emissao.py:466`). Se não existe disputa aberta, A e B recebem `None`. A entra em `ChamadoColaborador.abrir`, segura Colaborador e cria disputa; B espera a trava de Colaborador, reusa o chamado de A, mas segue diretamente para criar outra disputa, sem repetir a consulta inicial (`app/chamados/services/disputa_emissao.py:504`, `app/chamados/services/disputa_emissao.py:523`; trava do chamado em `app/chamados/models.py:863`). O modelo de disputa tem índice, sem restrição de unicidade, e `chamado` é FK comum (`app/chamados/models.py:1668`, `app/chamados/models.py:1731`).
   
   Contra-exemplo construído: colab ativo com saída ausente, nenhuma disputa aberta; duas declarações ou varreduras iniciam simultaneamente. As duas disputas podem inclusive referenciar o mesmo chamado-pai. A declaração passa por esse helper (`app/chamados/services/disputa_emissao.py:563`); a varredura também (`app/chamados/services/disputa_emissao.py:598`). **Especulação:** que essa sequência tenha ocorrido com algum colNN fornecido **[não verificado]**.

2. **Dedup de PerguntaDisputa é check-then-create sem trava — certeza alta.** O gate consulta identidade `(colaborador, motivo, data)` por `.last()` e depois faz `objects.create`, sem transação ou trava própria (`app/chamados/models.py:2370`, `app/chamados/models.py:2417`, `app/chamados/models.py:2465`). O `Meta` da pergunta não declara unicidade (`app/chamados/models.py:2800`). A trava do helper da disputa termina antes da geração de perguntas: helper em `:598`, geração em `:606`/`:618` (`app/chamados/services/disputa_emissao.py:598`).
   
   Contra-exemplo construído: já existe disputa aberta, mesmo colaborador/dia com motivo datado ainda sem pergunta; A retorna do helper e pausa durante a geração; B retorna do helper; ambos consultam ausência em `models.py:2420`, ambos criam. Não depende do defeito de nascimento do item 1 (`app/chamados/services/disputa_emissao.py:618`, `app/chamados/models.py:2420`). **Especulação:** quantidade real de duplicatas decorrentes dessa janela **[não verificado]**.

3. **Correção de resposta concorrente com validação deixa texto novo com validação/batida antiga — certeza alta para o interleaving.** O PWA carrega a pergunta e verifica sua vitalidade antes do core, sem trava (`app/chamados/views.py:1239`, `app/chamados/views.py:1257`). O core limpa validação somente quando `p.validada_em` da instância já carregada está preenchido, e depois salva resposta e carimbo (`app/chamados/services/disputa_emissao.py:1282`, `app/chamados/services/disputa_emissao.py:1286`, `app/chamados/services/disputa_emissao.py:1301`). O validador também lê sem trava, e seu `atomic` inicia depois das verificações (`app/chamados/services/validacao.py:359`, `app/chamados/services/validacao.py:366`, `app/chamados/services/validacao.py:395`); o carimbo é `UPDATE` sem versão/CAS (`app/chamados/services/validacao.py:127`).
   
   Contra-exemplo construído: pergunta de saída respondida **18:00**, não validada. POST de correção para **17:00** carrega instância sem validação e pausa após as guardas. Admin valida/materializa 18:00 e commita. POST continua com instância antiga: não inclui `validada_em`/`validada_por` em `update_fields` e grava resposta 17:00. Final possível: texto **17:00**, validação e batida de **18:00**; a batida é plantada pelo materializador (`app/chamados/services/disputa_emissao.py:1284`, `app/chamados/services/disputa_emissao.py:1297`, `app/chamados/services/materializacao.py:894`). **Especulação:** se os receivers posteriores corrigem integralmente todas as combinações desse estado **[não verificado]**; o overwrite e a manutenção dos campos não incluídos no save são demonstráveis nesses trechos.

4. **`analisar()` com instância velha revive até cancelamento absoluto — certeza alta.** `analisar` decide pelo `self.status_local`, sem recarregar/travar (`app/chamados/models.py:763`). `_gravar_estado` para destino vivo não relê o estado do banco e limpa `resolvido_em`/`via_encerramento` antes do save (`app/chamados/models.py:213`, `app/chamados/models.py:222`, `app/chamados/models.py:242`, `app/chamados/models.py:274`). O motor só permite ANALISAR a partir de aberto/registrado e declara cancelado como terminal absoluto (`app/chamados/catalogo/motor.py:112`, `app/chamados/catalogo/motor.py:142`).
   
   Contra-exemplo construído: view `ir_para_ausencia` carrega chamado `ausencia_colab` aberto e pausa (`app/chamados/views.py:1507`). Outra execução cancela e commita pelo gate que trava a linha (`app/chamados/models.py:739`). A view chama `analisar` na instância antiga e grava `em_analise`, apagando o cancelamento (`app/chamados/views.py:1513`, `app/chamados/models.py:769`). **Especulação:** ocorrência desse cancelamento específico em produção **[não verificado]**.

5. **Contexto JSON perde atualizações concorrentes de chaves distintas — certeza alta.** As portas copiam o JSON da instância, mesclam em memória e salvam o campo inteiro. O `atomic` cobre só a escrita; não há reload/trava (`app/chamados/services/contexto.py:70`, `app/chamados/services/contexto.py:75`, `app/chamados/services/contexto.py:96`, `app/chamados/services/contexto.py:100`). A própria docstring registra essa limitação (`app/chamados/services/contexto.py:25`).
   
   Contra-exemplo construído: A e B carregam mesmo chamado com contexto `C`. A anota `escalado_n2_1h`, B renova `marcos_faltantes`; se B grava por último, seu JSON `C + marcos_faltantes` remove o carimbo de A. Há chamador de escalada em `app/ponto/management/commands/escalonar_chamados_supervisao.py:138` e de renovação dos marcos em `app/chamados/management/commands/nomear_marco_por_lampada.py:109`. **Especulação:** repetição de push causada pela perda do carimbo, pois depende dos próximos ciclos **[não verificado]**.

Proteções que não devem ser denunciadas como faltantes:

- `ChamadoColaborador.abrir` efetivamente segura Colaborador **antes** do dedup dentro do mesmo `atomic`; duas chamadas desse gate com a mesma chave ficam serializadas em banco que implemente essa trava (`app/chamados/models.py:863`, `app/chamados/models.py:866`, `app/chamados/models.py:867`, `app/chamados/models.py:932`). Backend/configuração efetivos de produção **[não verificado]**.
- `reconciliar` e `cancelar` recarregam/travam chamado antes da decisão, diferentemente de `analisar`/transição humana (`app/chamados/models.py:616`, `app/chamados/models.py:673`, `app/chamados/models.py:739`).
- `reapontar_resolvedora` tem atomic próprio e segura o destino pela guarda até o save da costura (`app/chamados/services/fio.py:837`, `app/chamados/services/fio.py:840`, `app/chamados/services/fio.py:847`). Esse argumento não vale automaticamente para `PerguntaDisputa.abrir`, cuja chamada à guarda pode acontecer fora de atomic (`app/chamados/models.py:2437`, `app/chamados/services/fio.py:761`).
- Followups de sistema com `evento_chave` têm `UniqueConstraint` parcial (`app/chamados/models.py:1616`). Isso não protege perguntas/disputas nem followup com chave nula (`app/chamados/models.py:1607`, `app/chamados/models.py:1618`).

Não executei testes nem consultei banco/log; danos efetivos, planos de isolamento e reprodução em execução permanecem **[não verificado]**. [nao verificado]

## 05/11 15:59 · sessao 842c6f2b3b6a · - · - · origem=Ronald (tmux)

Achados demonstráveis por inspeção estática; ocorrência em produção e frequência: [não verificado]. Nenhum teste executado, arquivo criado ou alterado: [não verificado por auditoria externa]. [nao verificado]

1. **Recalculo concorrente pode desfazer aprovação — certeza alta, ESTRUTURA.**  
   `recalcular_fechamento_mes` carrega `FechamentoMensal` sem trava em `app/ponto/services/fechamento.py:889` e, se a instância carregada estava aberta, faz `save()` inteiro em `app/ponto/services/fechamento.py:1121-1128`. `aprovar_lote` atualiza aprovação em `app/ponto/services/fechamento.py:1313-1316`; existe chamador real em `app/ponto/views.py:1363`. A proteção do model só recusa quando **ambos** os status, gravado e da instância, são `aprovado`: `app/ponto/models.py:497-503`.  
   **Contra-exemplo sintético:** colab ativo, cadastro válido, fechamento aberto de competência corrente; A carrega o fechamento aberto, B aprova o fechamento, A termina e salva a instância aberta com `aprovado_por/aprovado_em` antigos. A aprovação de B desaparece: inferência direta de `app/ponto/services/fechamento.py:889`, `:1121-1128`, `:1313-1316` e `app/ponto/models.py:497-503`.  
   `unique_together(colaborador, mes, ano)` protege contra duas linhas, sem impedir essa sobrescrita: `app/ponto/models.py:485`. **Especulação:** impacto em usuários reais [não verificado].

2. **Recalculo antigo pode vencer o evento da batida e carimbar dados antigos como novos — certeza alta, ESTRUTURA.**  
   A lista de batidas é materializada em `app/ponto/services/fechamento.py:624`; a persistência ocorre depois em `:1127-1128` ou `:1144-1173`, sem comparação de versão dos insumos. O evento da batida agenda recalculo após commit em `app/ponto/registro_batida.py:177-178` e `app/ponto/services/fechamento.py:1791-1794`, `:1807-1808`.  
   **Contra-exemplo sintético:** mesmo colab/dia, entrada existente e saída faltante; A captura a lista antes da saída, B registra a saída e seu callback termina o recalculo com o par completo; A termina depois e sobrescreve com seu resultado antigo, incluindo `derivado_em` posterior. Intercalação inferida dos pontos citados. **Especulação:** persistência até o próximo recalculo e ocorrência concreta [não verificado].

3. **A mesma corrida pode apagar a invalidação de uma justificativa recém-aprovada — certeza alta, ESTRUTURA.**  
   O recalculo coleta justificativas aprovadas em `app/ponto/services/fechamento.py:651-656`. A aprovação grava a justificativa em `app/ponto/services/justificativa.py:190-193` e invalida o fechamento em `:199-201`; invalidar escreve `derivado_em=None` em `app/ponto/services/fechamento.py:1636-1640`. O recalculo antigo, ao terminar, preenche novamente `derivado_em` em `:1127` ou `:1173`. O vigia só busca `derivado_em IS NULL`: `:1675-1680`.  
   **Contra-exemplo sintético:** falta num dia com justificativa pendente sem saída informada; A coleta as aprovadas antes da decisão; B aprova a justificativa e invalida o fechamento; A salva resultado sem a justificativa e elimina o marcador de invalidação. Intercalação inferida dos pontos citados. **Especulação:** número de fechamentos afetados [não verificado].

**Limite das travas dos crons.** O runner geral marca uma execução por PID e chama o comando, sem exclusão mútua: `bin/cron_run.sh:9-11`. O render o usa por empresa em `app/config/crons.py:1821-1830`; `processar_cartorio` chama `julgar_colab` diretamente em `app/ponto/management/commands/processar_cartorio.py:59-65`. Isso permite duas invocações sobrepostas pelo caminho geral; sobreposição real no host [não verificado].

Há proteções específicas que impedem generalizar “os crons não têm trava”: os encadeados usam `flock` por comando e carimbo diário em `bin/encadeado.sh:78-82`, esperam os carimbos dos gatilhos em `:89` e executam o runner em `:102-104`; o consumidor de eventos usa exclusão por advisory lock em `app/chamados/management/commands/consumir_eventos_celula.py:18-23` e `app/chamados/reconciliador.py:1192-1212`. O helper tenant apenas itera schemas e executa `func(cliente)`: `app/tenants/cron.py:19-30`.

Não verificados: crontab instalado, múltiplos hosts, runtime das execuções, isolamento efetivo e migrations aplicadas no banco, batidas/cadastros reais que tenham disparado os casos, quantidade de duplicatas ou estados perdidos [não verificado]. [nao verificado]

## 05/11 15:59 · sessao 3af1edbc60f3 · - · - · origem=Ronald (tmux)

Mapeamento confirmado:

- `_toque_humano_no_dia` lê `LogAuditoria.criado_em`, `usuario_id`, `acao`, `modelo`, `objeto_id` e, para Cobrar, `descricao`; exclui usuário nulo e o PK sistema memoizado. Consulta vínculos atuais `CelulaDia(colaborador_id,data)` e `ChamadoColaborador(colaborador_id,celula_id)`. (app/chamados/juizes.py:1062–1078)
- `evento` é o escritor comum: determina usuário pelo ator explícito ou requisição e cria os campos; `criado_em` é `auto_now_add`. `registrar_log` adapta request para `evento`. (app/core/observ.py:108–138; app/core/models.py:295–307; app/core/auditoria.py:22–24)
- Cobrar escreve ação `cobrar_dia_manual`, modelo `Colaborador`, PK do colaborador e `dia=<ISO>` na descrição, antes de consultar cartório/app. (app/chamados/services/cobrar_dia.py:29–43)
- Resolver escreve `veredito_celula`, `CelulaDia`, PK da célula no serviço; também a porta de lavra escreve a mesma ação/modelo, com ator e `via` na descrição/JSON, inclusive lavra cartório. O leitor não filtra `via`; só usuário/tempo/ação/modelo/objeto. (app/ponto/services/veredito_celula.py:329–336; app/ponto/portas/celula.py:745–789; app/chamados/juizes.py:1065–1074)
- Desarquivar chama `arquivo.levantar(... acao='desarquivar_chamado',por=request.user)`; esta salva o chamado e `_trilha` passa modelo/PK/ator a `evento`. No-op do véu não escreve trilha. (app/chamados/views_arquivar.py:77–86; app/chamados/services/arquivo.py:130–138; app/chamados/services/arquivo.py:193–209)
- As identidades de célula são escritas na criação pela porta, em regeneração ou geração de período. (app/ponto/portas/celula.py:247–248; app/ponto/portas/celula.py:601–604)
- Chamado recebe colaborador/célula no nascimento por `abrir` e `criar`; `apontar_celula` só preenche célula nula, recusa `None` e preserva vínculo existente. (app/chamados/models.py:913–932; app/chamados/models.py:1028–1048; app/chamados/services/vinculos.py:73–78; app/chamados/services/vinculos.py:92–99)
- `get_user_sistema` pode criar o usuário via `get_or_create`; o comando garantidor também cria e normaliza seus atributos. `_toque_humano_no_dia` escreve `memo[('sistema',)]` e o filtro humano lê esse PK. (app/core/utils.py:18; app/core/management/commands/garantir_user_sistema.py:13–41; app/chamados/juizes.py:1062–1066)

Contraexemplo adicional do **diff total**, independente do memo da tranca:

1. P2 de `colNN`, dia 05/08/2026, competência aberta, sem validação/resposta/materialização, com célula já existente cujo veredito é `furo`; esses campos são suficientes para passar a tranca e chegar ao retorno `True` da célula acusadora. (app/chamados/juizes.py:1130–1143; app/chamados/juizes.py:1175–1178)
2. `perguntas_do_app` avalia o queryset e agora carrega antecipadamente todas as células com `select_related('celula')`; antes de P2 ser julgada, o admin confirma Resolver dia=`trabalhou`, que fecha o fio e lavra a célula pela porta. A P2 já carregada conserva seus campos anteriores; ocorrência concreta em produção: [não verificado]. (app/chamados/services/pergunta_no_app.py:46–53; app/ponto/services/veredito_celula.py:325–330)
3. **76626871:** P2 conserva a célula eager `furo` e devolve viva. **Antes de 630131b1:** conforme diff total fornecido, a célula não era eager; `_chave_memo_pv` a carregaria depois do ato, encontrando `trabalhou`, e sem marcos pendentes retornaria morta. Isto demonstra mudança do instante da leitura; execução desse interleaving: [não verificado]. (app/chamados/services/pergunta_no_app.py:47; app/chamados/juizes.py:801; app/chamados/juizes.py:1175–1190; comparação histórica: [não verificado])
4. Essa alteração atende leitores reais fora do vigia: API, ficha, `medir` e `do_dia`, além de `_no_app`. (app/api/views.py:2552; app/colaboradores/services/ficha.py:166; app/chamados/services/pergunta_no_app.py:139; app/chamados/services/pergunta_no_app.py:162; app/chamados/juizes.py:1313)

Não encontrei porta de produção que substitua/remova FK de célula já preenchida em chamado, nem que altere colaborador/data de célula existente; portanto contraexemplo de **SIM do toque invalidado por troca desses vínculos por porta real**: [não verificado]. A porta demonstrada só acrescenta vínculo antes ausente. (app/chamados/services/vinculos.py:92–99)

O comentário “quatro chamadores” está desatualizado: a ficha também chama `perguntas_do_app`. Isso amplia o alcance do carregamento eager, sem constituir sítio sem chamador. (app/chamados/services/pergunta_no_app.py:48–49; app/colaboradores/services/ficha.py:166)

## 05/11 15:59 · sessao 23d8e31b320d · - · - · origem=Ronald (tmux)

1. **Mordida dos testes, por inspeção; execução dos mutantes [não verificado].** [nao verificado]

   | Teste | Helper revertido a 144c7fac | Comando sem lote |
   |---|---|---|
   | Desempenho | Falha: espera crescimento `lg5-lg1=4`, incompatível com coleta plural única. | Passa: abre lote diretamente. (`app/chamados/tests/test_snc2_toque_em_lote.py:85–109`) |
   | Paridade estática | Passa: os toques e vínculos precedem todas as avaliações. | Passa: abre lote diretamente. (`app/chamados/tests/test_snc2_toque_em_lote.py:111–133`) |
   | Cobrar durante lote | Falha no primeiro toque posterior ao aquecimento. | Passa: abre lote diretamente. (`app/chamados/tests/test_snc2_toque_em_lote.py:135–147`) |
   | Chamado ganha célula | Falha: conjunto plural não incorpora o vínculo posterior. | Passa: abre lote diretamente. (`app/chamados/tests/test_snc2_toque_em_lote.py:149–162`) |
   | Comando dentro do lote | Passa: 144c7fac já tinha o contexto no comando. | Falha: registra `False` em `visto`. (`app/chamados/tests/test_snc2_toque_em_lote.py:164–177`; diff_v2_total fornecido [não verificado]) |
   | Guarda por instância | Passa: não instala lote nem request. | Passa: não chama comando. (`app/chamados/tests/test_snc2_toque_em_lote.py:179–187`) |
   | Request depois do lote | Passa: o conjunto negativo do lote anterior já foi descartado quando instala request novo. | Passa: abre lote diretamente. (`app/chamados/tests/test_snc2_toque_em_lote.py:189–207`) |

   O teste do comando verifica apenas `censo`: manter `censo` dentro de contexto e tirar os outros quatro consumidores continuaria satisfazendo sua asserção. (`app/chamados/tests/test_snc2_toque_em_lote.py:169–176`; `app/chamados/management/commands/vigiar_fio_morto.py:171–176`)

2. **Triagem, linha por linha.**

   - **2:** curado o congelamento do NÃO do toque; a consulta ocorre novamente enquanto não houver SIM. (`app/chamados/juizes.py:1037–1044`)
   - **2b:** curado o caso específico de chamado antes sem célula: os vínculos são consultados novamente após resultado negativo. (`app/chamados/juizes.py:1070–1078`; `app/chamados/tests/test_snc2_toque_em_lote.py:149–162`)
   - **3:** curada a causa apontada no v1, mas não demonstrada paridade temporal geral: empresa e tranca continuam congeladas, inclusive tranca ausente. (`app/chamados/juizes.py:938–945`)
   - **4a:** a guarda antiga continua só por instância; o teste novo instala request real, porém somente depois de encerrar lote. Não verifica request que já existe ao abrir lote, exceção ou reentrância. (`app/chamados/tests/test_snc2_toque_em_lote.py:179–207`)
   - **4b:** curado: importação direta substitui fallback permissivo. (`app/chamados/tests/test_snc2_toque_em_lote.py:28–30`)
   - **4c:** curado para ausência integral do contexto do comando; alcance da asserção limita-se ao `censo`. (`app/chamados/tests/test_snc2_toque_em_lote.py:164–177`)
   - **5:** preexistência pode justificar fila, mas a justificativa “o lote não prolonga mais nada” é falsa para empresa/tranca e SIM; novo dono dura toda rodada e tem prioridade sobre instâncias frescas. Descarte não elimina o alcance obrigatório dos leitores/escritores. (`app/chamados/juizes.py:938–945`, `app/chamados/juizes.py:967–974`, `app/chamados/juizes.py:1038–1043`; `CLAUDE.md:371–372`)
   - **6a:** curado: helper atual tem apenas consultas direcionadas, com um chamador de produção. (`app/chamados/juizes.py:1041`, `app/chamados/juizes.py:1047–1078`)
   - **6b:** escrita condicional é preexistente, mas continua alcançável: memoização chama `get_user_sistema`, cujo corpo usa `get_or_create`. Logo a cadeia não é incondicionalmente só leitura. (`app/chamados/juizes.py:1062–1064`; `app/core/utils.py:18`)
   - **6c:** preexistente confirmado; continua no alcance de C6, que lê `date.today()` internamente apesar do relógio capturado pelo comando. (`app/chamados/juizes.py:562`, `app/chamados/juizes.py:1191`; `app/chamados/management/commands/vigiar_fio_morto.py:163–164`)
   - **6d:** não vira comprovação por ser descartado: regra exige suíte inteira verde seguida de Opus final; o dado fornecido relata 283 testes de módulos selecionados, mas não demonstra suíte inteira nem Opus final/pouso. (`CLAUDE.md:369–373`; dado `numeros_v2.txt` [não verificado]) [nao verificado]

   A regra local exige anexar descartes ao Opus, que decide fila ou bloqueio; “fora do diff” não é autorização automática para retirar dependências do exame. (`CLAUDE.md:368–372`)

3. **Lacuna central dos selos:** não há mutação de tranca/reabertura/`fechado_em`/empresa/corte durante lote: período nasce uma vez; testes dinâmicos escrevem Cobrar ou preenchem FK. O memo, porém, conserva `None` e objeto `PeriodoFechado` sem reconsulta. (`app/chamados/tests/test_snc2_toque_em_lote.py:40`, `app/chamados/tests/test_snc2_toque_em_lote.py:135–162`; `app/chamados/juizes.py:938–945`) Reabertura grava `reaberto_em`, enquanto o juiz original considera viva só linha com esse campo nulo. (`app/ponto/services/fechamento.py:1421–1425`, `app/ponto/services/fechamento.py:1448–1454`)

4. **Classes:** C6 permanece registrado em endereço único; `_toque_humano_no_dia` é dependência chamada pelo C6 e `tranca_em_lote` tem chamador no comando. (`app/core/juizes.py:266`; `app/chamados/juizes.py:1041`; `app/chamados/management/commands/vigiar_fio_morto.py:171`) Não identifiquei sobrevivência dos símbolos antigos `_toques_humanos_depois`/`_DIA_DO_COBRAR` na busca em `app`; confirmação histórica de remoção depende do diff fornecido [não verificado]. A obrigação de apagar sítio sem chamador consta em `CLAUDE.md:188` e `app/docs/LEIS.md:137`.

5. **Ordem operacional:** dependências declaradas do vigia continuam cartório e reconciliação; cumprimento operacional [não verificado]. (`app/config/crons.py:1680–1682`) O contexto apenas instala/restaura memo, sem transação ou trava que congele escritores externos. (`app/chamados/juizes.py:987–997`)

## 05/11 15:58 · sessao 0e418b94292a · - · - · origem=Ronald (tmux)

Achado bloqueante: o lote ainda congela empresa e tranca, inclusive a ausência de tranca; retirar o cache do NÃO do toque não conserva o veredito sob reabertura/tranca durante a rodada. (app/chamados/juizes.py:938–947, app/chamados/juizes.py:967–974)

1. **Contraexemplo construído, sem depender de `_pv_memo` herdado.** Empresa E, corte 21, julho/2026 trancado; P1 de col01 em 10/07 aquece o lote, guardando o `PeriodoFechado`. Outra execução do admin reabre julho pela view: ela consulta seu próprio objeto e chama a porta, que salva `reaberto_em`. Depois se julga pela primeira vez P2 de col02, mesma empresa, 11/07/2026, sem toque, validação ou materialização. O lote reutiliza a tranca antiga e dá **morta**; o avulso com colaborador fresco consulta a autoridade, recebe `None` e chega a **viva**. A troca de colab impede atribuir essa divergência ao identity map anterior; o novo dono global da rodada é que compartilha a resposta entre colabs. (app/chamados/juizes.py:938–951, app/chamados/juizes.py:967–974, app/chamados/juizes.py:1130–1135, app/chamados/juizes.py:1201–1202, app/ponto/views.py:1541–1548, app/ponto/services/fechamento.py:1421–1425, app/ponto/services/fechamento.py:1454)

   A estrutura da pergunta sem validação/resposta é fornecida pela fixture; ocorrência em produção: [não verificado]. (app/chamados/tests/test_c7_red_minimos.py:110–124)

2. **Sentido inverso.** P1 col01 aquece julho aberto, guardando `None`; admin tranca julho enquanto o lote roda; P2 col02, ainda não julgada e sem toque posterior, continua **viva** no lote, enquanto avulso fresco fica **morta**. A porta escreve a tranca e seu gancho declara que perguntas passam a morrer por leitura, sem carimbá-las; assim esse gancho não elimina o cenário. (app/chamados/juizes.py:942–947, app/chamados/juizes.py:1134–1135, app/chamados/juizes.py:1201–1202, app/ponto/services/fechamento.py:1392–1403, app/chamados/services/competencia_trancada.py:8–11)

3. **Outro contraexemplo, corte durante o lote.** E começa com corte 21, julho trancado e agosto aberto; P1 col01 aquece `('empresa', E)`. Admin muda o corte para 10; P2 col02 de 11/07/2026 é carregada fresca. No lote a empresa antiga classifica julho e mata; no avulso a empresa atual classifica agosto e deixa viva. A edição passa pela view e pela porta que salva a empresa. Portanto o comportamento histórico é herdado, mas o prolongamento da fotografia entre colaboradores é introduzido pelo lote. (app/chamados/juizes.py:938–947, app/chamados/juizes.py:967–974, app/ponto/janelas.py:24–33, app/ponto/janelas.py:50–63, app/colaboradores/views.py:496–499, app/colaboradores/views.py:521–523, app/colaboradores/services/estrutura.py:17–20)

4. **Inventário do memo.**

   | Chave/estado | Escritor e leitor |
   |---|---|
   | `('empresa', empresa_id)` | `dia_da_pergunta_trancado` grava `colab.empresa` e entrega ao juiz da janela. (app/chamados/juizes.py:938–941) |
   | `('tranca', empresa_id, mês, ano)` | A mesma função grava o retorno de `periodo_trancado`, incluindo `None`, e reutiliza-o. (app/chamados/juizes.py:942–947) |
   | `('toque', colab.pk, data, desde)` | `_admin_tocou_o_dia_depois` lê e grava apenas `True`. (app/chamados/juizes.py:1037–1044) |
   | `('sistema',)` | `_toque_humano_no_dia` grava o PK e o usa na exclusão da trilha. (app/chamados/juizes.py:1062–1066) |
   | `_tranca_memo` | `_memo_da_tranca` cria no request ou colaborador; o teste de chokepoint o remove explicitamente. (app/chamados/juizes.py:970–974, app/ponto/tests/test_selo_chokepoint_escrita.py:417) |
   | `LOTE_DA_TRANCA` | Declarado vazio; `tranca_em_lote` consulta reentrância, instala `{}` e restaura em `finally`; `_memo_da_tranca` lê e prioriza esse dono; o comando abre o contexto. (app/chamados/juizes.py:967–969, app/chamados/juizes.py:984–997, app/chamados/management/commands/vigiar_fio_morto.py:171) |
   | Request | Middleware instala/restaura `REQUISICAO_DA_TRILHA`. (app/core/middleware.py:76–82) |
   | Usuário sistema subjacente | `get_user_sistema` usa `get_or_create`; o comando garantidor cria e normaliza atributos. Alteração/substituição desse PK durante lote por uma porta demonstrada: [não verificado]. (app/core/utils.py:18, app/core/management/commands/garantir_user_sistema.py:13–41) |

5. **Leitores fora do diff que importam para o alcance.** A tranca também é consultada diretamente por `respostas_sem_veredito`, pelo emissor e pelo reconciliador; o cache no lote afeta qualquer chamada a essas funções que aconteça sob o contexto, embora cadeia de produção abrindo lote nelas não esteja demonstrada. (app/chamados/models.py:1769–1776, app/chamados/services/disputa_emissao.py:1577–1589, app/chamados/reconciliador.py:576–616, app/chamados/reconciliador.py:715–716)

6. **Triagem item 5.** Sustenta-se dizer que “empresa/corte de hoje classificando passado” é herdado; não se sustenta concluir que o lote não prolonga insumos relevantes, pois conserva empresa e período para toda a rodada. Essa conservação já basta para os contraexemplos 1–3. (app/chamados/juizes.py:938–947, app/chamados/juizes.py:967–974)

7. **Limitação da cura do toque.** A consulta negativa agora relê Cobrar, células, Resolver e chamados; o vínculo que antes faltava pode passar a existir e ser reconhecido. `apontar_celula` realmente só preenche vínculo ausente, por porta; o teste novo que usa `.update(celula=cel)` reproduz o estado, mas não exerce a porta. (app/chamados/juizes.py:1067–1078, app/chamados/services/vinculos.py:92–100, app/chamados/tests/test_snc2_toque_em_lote.py:147–162)

**VEREDITO: REPROVA**, pelos congelamentos restantes de tranca e empresa durante o lote. (app/chamados/juizes.py:938–947, app/chamados/juizes.py:967–974)

## 05/11 15:58 · sessao ddb9e4f4dd8c · - · - · origem=Ronald (tmux)

Achados estáticos; nenhum acesso a banco/log e nenhum teste executado [não verificado]. [nao verificado]

1. **Deduplicação da batida admite corrida — certeza alta; dono ESTRUTURA.** A porta consulta a janela de 60 s, depois a chave exata e a colisão de tipo oposto, antes de executar `create`, sem trava que una consulta e inserção: `app/ponto/registro_batida.py:107`, `:113`, `:120`, `:139`, `:157`. A única unicidade declarada é colaborador + timestamp: `app/ponto/models.py:102-106`.
   - **Contraexemplo construído:** colab ativo sem batida próxima; A recebe E às 08:00:00 e B E às 08:00:01; ambas terminam as consultas antes das inserções. Ambas criam, porque timestamps diferem. O dedup sequencial reutilizaria a primeira (`registro_batida.py:113-115`); concorrente pode produzir duas E.
   - Os chamadores também consultam antes de escrever: API `app/api/views.py:557-563`, calcula sequência `:584-585`, tipo `:600-608`, chama porta `:683`; HTML `app/ponto/services/triagem_batida.py:668-681`, `:708`.
   - **Variante:** mesmo timestamp nas duas chamadas: a constraint impede duplicata, mas o segundo `create` pode levantar `IntegrityError`; a porta não recupera esse conflito (`registro_batida.py:120-161`, `models.py:106`). HTTP efetivo e ocorrência em produção [não verificado].
   - **Especulação rotulada:** frequência, impacto financeiro e colaboradores reais atingidos [não verificado].

2. **Duas inversões de tipo perdem uma operação — certeza alta; dono ESTRUTURA.** A view carrega a batida sem lock (`app/ponto/views.py:3255-3256`); `flip_tipo` usa o tipo da instância (`app/ponto/services/flip_batida.py:25`), calcula o oposto (`:28`) e chama `anotar` (`:30`). A porta compara valores da instância, não valores relidos do banco (`app/ponto/registro_batida.py:244-249`), depois salva e registra trilha (`:254`, `:263`).
   - **Contraexemplo construído:** batida E, não retratada, competência aberta e dois usuários autorizados. A e B carregam E; ambos calculam S. A grava S, B grava S. Duas inversões aceitas terminam em S, enquanto duas inversões sequenciais terminariam em E; as duas trilhas podem dizer E→S (`flip_batida.py:25-33`; `registro_batida.py:254-263`).
   - **Especulação rotulada:** existência desse duplo clique/concorrência em produção [não verificado].

3. **Retratação concorrente perde o primeiro carimbo e duplica trilha — certeza alta; dono ESTRUTURA.** O guard de “já retratada” usa a instância recebida (`app/ponto/services/retratar_humano.py:173-175`; `app/ponto/registro_batida.py:271-272`). Cada execução gera seu próprio `timezone.now()` e salva (`registro_batida.py:274-276`), pela mesma porta sem releitura/trava (`:244-254`) que emite trilha (`:263`).
   - **Contraexemplo construído:** mesma batida aprovada, `retratada_em=None`, motivo ativo e dois usuários autorizados; A e B carregam o estado inicial. A grava t1 e B t2. Estado final contém t2; ficam duas trilhas de retratação, apesar da promessa de não repetir trilha (`registro_batida.py:239-240`, `:269-270`).
   - **Especulação rotulada:** ocorrência, intervalo t1/t2 e qual motivo prevaleceu em produção [não verificado].

**Limites e falsos positivos evitados:** Justificativa de regularização possui tanto unicidade colaborador + dia + entrada quanto constraint parcial para pendente por colaborador/dia; portanto o `get_or_create` de `app/ponto/services/regularizacao.py:91` não foi classificado como “sem unique” (`app/ponto/models.py:182-193`). `FotoBatida.batida` é OneToOne; não alegar que `update_or_create` nesse vínculo permite duas linhas (`app/ponto/models.py:1024-1027`).

**Cobertura de testes vista, sem execução:** o teste de “46 reenvios” é sequencial (`app/ponto/tests/test_bater_mesma_chave_idempotente.py:60-67`); o teste de flip faz uma requisição e verifica uma inversão (`app/ponto/tests/test_flip_batida.py:20-27`). Esses trechos não demonstram proteção de concorrência.

| achado | arquivo:linha | contraexemplo | certeza | dono L-099 |
|---|---|---|---|---|
| Dedup check-then-create | `app/ponto/registro_batida.py:113`, `:157`; `app/ponto/models.py:106` | Duas E 08:00:00/08:00:01 passam antes dos creates | Alta | ESTRUTURA |
| Inversão perdida | `app/ponto/services/flip_batida.py:25`; `app/ponto/registro_batida.py:254` | Duas instâncias E; duas inversões resultam em S | Alta | ESTRUTURA |
| Retratação repetida | `app/ponto/registro_batida.py:271`, `:274`, `:263` | Duas instâncias sem retratação gravam t1/t2 e duas trilhas | Alta | ESTRUTURA |

## 05/11 15:58 · sessao 61f3f3af59e5 · - · - · origem=Ronald (tmux)

Leitura estática: **89 entradas em `CRONS` e 14 em `ENCADEADOS`, total 103**, incluindo três modalidades de `supra_juiz`; a lista começa em `app/config/crons.py:281` e a corrente em `app/config/crons.py:1467`. Não executei comandos Django nem consultei banco/log. Ocorrência desses cenários em produção: **[não verificado]**.

### Achados principais, certeza alta

1. **`termometro_regua --avaliar --lavrar`: histerese e contador avançam por execução.** O cron usa essa modalidade em `app/config/crons.py:428`; chama o avaliador em `app/escala/management/commands/termometro_regua.py:43`. O avaliador reduz a confiança em até 20 por chamada e incrementa `regua_dias_baixo` por avaliação, sem guarda de dia (`app/escala/regua_defesa.py:582`, `:757`, `:764`). Contra-exemplo construído: vínculo ativo de colaborador em operação, não isento; template com início=fim; confiança inicial 100. O diagnóstico será DEGENERADA, alvo 0, e duas execuções dão **100→80→60**, mesmo cadastro e batidas (`app/escala/regua_defesa.py:133`, `:584`, `:596`). A porta grava uma trilha em cada mudança, portanto duas mudanças produzem duas trilhas (`app/colaboradores/services/vinculo.py:989`, `:996`). **Dono ESTRUTURA**: defeito do processamento repetido, embora o gatilho do exemplo seja cadastro degenerado. O próprio comentário declara passo por chamada (`app/escala/regua_defesa.py:578`).

2. **`marcar_foto_ausente_retro --apply`: mesma batida aumenta contador de ocorrências da disputa.** Agendado em `app/config/crons.py:686`. O comando seleciona de novo toda batida de aparelho dentro da janela sem foto e passa `batida_id` ao emissor, sem excluir o que já foi marcado (`app/ponto/management/commands/marcar_foto_ausente_retro.py:66`, `:73`, `:95`). O helper usa `merge_contexto=contexto` (`app/chamados/services/disputa_emissao.py:598`). Em disputa existente, incrementa `qtd` e sobrescreve `ultima_em`, sem comparar `batida_id` (`app/chamados/services/disputa_emissao.py:484`, `:488`, `:493`, `:501`). Contra-exemplo construído: uma batida `origem='app'`, canal APK, ontem 08h, viva, sem `FotoBatida` e sem `Batida.foto`, com disputa aberta `foto_ausente.qtd=1`: duas execuções dão **1→2→3 para a mesma batida**. APK vence em 90 minutos (`app/ponto/registro_batida.py:71`, `:80`). Não prova duas disputas; prova estado agregado diferente. **Dono ESTRUTURA**.

3. **`alertar_disputas_pendentes`: duplica trilha `disputa_velha` em cada passagem.** Cron em `app/config/crons.py:743`. Embora o push individual tenha guarda diária, a trilha da disputa com mais de sete dias fica fora dessa guarda (`app/ponto/management/commands/alertar_disputas_pendentes.py:48`, `:72`, `:75`). `evento_kw` termina em `LogAuditoria.objects.create`, sem deduplicação (`app/core/observ.py:134`, `:142`). Contra-exemplo construído: disputa aberta há oito dias, inclusive já alertada hoje: duas passagens inserem **duas linhas `disputa_velha`**. O push agregado por empresa, diferentemente, consulta a trilha antes de enviar (`app/ponto/management/commands/alertar_disputas_pendentes.py:93`). **Dono ESTRUTURA**.

4. **`alertar_retornos`: repete push com o mesmo retorno.** Cron em `app/config/crons.py:607`. Busca férias terminando hoje ou folga hoje/trabalho amanhã e chama `push_dp` sem carimbo (`app/ferias/management/commands/alertar_retornos.py:20`, `:34`, `:45`). Contra-exemplo construído: agendamento `em_gozo` com `data_fim=hoje`; duas execuções chamam duas vezes o push do mesmo retorno. O utilitário percorre os usuários DP e envia diretamente, sem dedup (`app/chamados/push_utils.py:64`, `:85`, `:40`). **Dono ESTRUTURA**. Entrega efetiva no dispositivo: **[não verificado]**.

5. **`alertar_ferias_vencendo`: repete aviso em 7/30/60 dias.** Cron em `app/config/crons.py:708`. Período ainda adquirido com prazo em uma das datas de aviso entra novamente na lista; DP e colaborador recebem nova chamada de push (`app/ferias/management/commands/alertar_ferias_vencendo.py:38`, `:40`, `:55`, `:62`, `:71`, `:85`). Contra-exemplo construído: período adquirido com saldo, limite daqui a sete dias; duas execuções no mesmo dia repetem os dois avisos. A transição para **vencido** tem comportamento diferente: o comando só busca `em_curso/adquirido`, portanto aquele período deixa o universo na segunda execução (`app/ferias/management/commands/alertar_ferias_vencendo.py:22`, `:35`). **Dono ESTRUTURA**. Entrega efetiva: **[não verificado]**.

6. **`alertar_justificativas_sla`: repete push com a mesma pendência vencida.** Cron em `app/config/crons.py:606`. Não há carimbo de envio entre a consulta das justificativas pendentes fora do SLA e o `push_dp` (`app/ponto/management/commands/alertar_justificativas_sla.py:18`, `:30`, `:32`). Contra-exemplo construído: uma justificativa ainda `pendente` para a qual `prazo_estourou(j, hoje)` é verdadeiro; duas chamadas repetem o aviso. **Dono ESTRUTURA**. Entrega efetiva: **[não verificado]**.

7. **`tripwire_tipo_batida`: testa estar acima do limiar, não cruzamento do limiar.** O cron roda com `--dias 60 --limiar 60` (`app/config/crons.py:676`). Todo `n>limiar` chama push sem guardar o valor anterior (`app/ponto/management/commands/tripwire_tipo_batida.py:66`, `:71`). Contra-exemplo construído: 61 colaboradores, cada um com duas E consecutivas ontem 08h/09h, sem outras batidas; a seleção produz 61 pares e ambas as execuções notificam (`app/ponto/services/flip_auto.py:31`, `:43`). O comentário “push só ao cruzar limiar” não corresponde à guarda executada (`app/ponto/management/commands/tripwire_tipo_batida.py:14`). **Dono ESTRUTURA**. Entrega efetiva: **[não verificado]**.

### Separar dos achados de duplicação

- **SLA avança por lotes**: `alertar_chamados_sla` tem teto 20 por passada (`app/chamados/services/sla.py:14`, `app/chamados/management/commands/alertar_chamados_sla.py:37`). Com 21 chamados vencidos, sem aviso e posteriores ao corte do passivo, primeira passagem avisa 20, segunda avisa o restante. Não é ponto fixo após uma passagem; há dedup individual por followup de SLA (`app/chamados/services/sla.py:42`). **Não afirmar duplicata de chamado/trilha pelo teto.**
- **Estado literal inclui timestamps**: `lavrar_badge_navbar`, `lavrar_placar_situacional`, `lavrar_pautas`, `lavrar_retrato_folha`, `calcular_scores` e snapshots de métricas renovam carimbo mesmo com conteúdo igual (`app/core/management/commands/lavrar_badge_navbar.py:91`; `app/core/management/commands/lavrar_placar_situacional.py:105`; `app/pautas/services.py:354`; `app/folha/management/commands/lavrar_retrato_folha.py:74`; `app/inteligencia/management/commands/calcular_scores.py:104` + `app/inteligencia/models.py:21`; `app/inteligencia/metricas.py:47` + `app/inteligencia/models.py:96`). Isso não demonstra duplicata de entidade.
- **Trilha operacional do cron é por tentativa**: o envelope escreve um JSON por corrida e renova arquivo de término (`bin/cron_run.sh:13`, `:25`). Portanto exigir literalmente uma única linha operacional para duas execuções contradiz esse desenho; separar da trilha de negócio.
- **Vigia também escreve carimbo**: `vigiar_fio_morto --alarme` substitui sua fotografia durável (`app/chamados/management/commands/vigiar_fio_morto.py:114`, `:131`, `:222`).

### Cobertura e inventário

As guardas de repetição foram lidas, sem certificação integral, em:

- `processar_alertas_turno`: envio condicionado ao `created` de `registrar_alerta` (`app/ponto/management/commands/processar_alertas_turno.py:230`).
- `processar_alertas_avancados`: flag N3, dedup do chamado N5 e followup diário (`app/ponto/management/commands/processar_alertas_avancados.py:74`, `:100`, `:177`).
- `escalonar_chamados_supervisao`: flags individuais por nível (`app/ponto/management/commands/escalonar_chamados_supervisao.py:96`, `:112`, `:147`, `:172`, `:188`).
- `pre_fechamento`: pauta viva de hoje impede repetição (`app/folha/management/commands/pre_fechamento.py:98`).
- `pautas_de_ferias`: marcador/âncora/pauta viva impedem repetição sequencial (`app/ferias/management/commands/pautas_de_ferias.py:154`).
- `rotacionar_access_log`: destino datado existente impede segunda rotação (`app/core/management/commands/rotacionar_access_log.py:53`).
- `lavrar_foto_folgas`: reconsulta falta/sobra antes da escrita; os contadores continuam sendo snapshots (`app/escala/management/commands/lavrar_foto_folgas.py:167`, `:180`).
- `recusar_ponta_pequena`: exclui pares com decisão vigente (`app/ponto/management/commands/recusar_ponta_pequena.py:120`, `:141`).
- `consumir_eventos_celula`/`reconciliar_chamados`: consumidor exclusivo e marca de processado; em falha incrementa tentativas, em teto de tempo pode deixar restante para outra passada (`app/chamados/management/commands/consumir_eventos_celula.py:19`; `app/chamados/reconciliador.py:1337`, `:1351`).

**Demais entradas inventariadas, sem conclusão integral de idempotência** — referências abaixo são a declaração em `app/config/crons.py`:

- RAW: `backup_all.sh` :282; `lavrar_abriu_nao_bateu.sh` :290; `vigia_arvore.sh` :296; `vigia_logins.sh --auto` :303; `alarme_sessao_ociosa.py --push` :311; `fabrica_estado.py --lavrar` :313; `tickets_rodape.sh --conferir` :315; `crons.sh check --medir` :318; `deploy.sh --reload-agendado` :326; `sombra.sh --refazer/--bloco` :350; `noturno.py` :594; `sentinela_diario.sh` :688; `negou_fogo_diario.sh` :705. [nao verificado]
- Outros contínuos/grade: `detectar_ausencias` :364; `lavrar_perguntas_stale` :390; `calcular_metricas` :400; `health_crawl` :404; `gerar_celulas` :426; `tripwire_celulas` :429; `efetivar_desligamentos_agendados` :440; `sincronizar_status_ferias` :442; `reverter_situacao_afastado` :448; `reconciliar_grade` :462.
- Lavras/auditoria: `lavrar_fase_conflitante` :498; `lavrar_vigencia_sem_trilha` :500; `lavrar_vigencia_impossivel` :502; `lavrar_previsto_cego` :514; `lavrar_teto_do_dia` :521; `lavrar_furo_sem_canal` :531; `lavrar_jornada_lixo` :540; `auditar_invariantes_chamados` :542; `verificar_nucleo` :544; `vigiar_motor_autoridade` :596; `detectar_vinculo_divergente` :597; `supra_juiz --executar` :599; `detectar_par_relampago --apply --retratar` :605; `processar_cartorio` :616; `censo_fase_12x36 --lavrar` :638; `esmeril_espelho --apply` :640; `pautas_do_esmeril` sem `--apply` :643; `mapa_divergencia` :644; `vigia_plantio_overnight` :646; `vigia_b5_ausencia` :661; `flip_automatico` :707; `escalonar_documentos_ausencia` :715; `fechar_cobranca_com_lastro` :729; `vigia_entrada_faltante` :735; `vigiar_familia_ausencia` :737; `vigiar_familia_escala` :741; `gerar_alertas` :744; `suporte_hasner --fila` :751.
- Modos `--alarme` cujas guardas de não aplicação foram inspecionadas, **sem certificar todos os helpers transitivos**: `reconciliar_fantasmas` :392; `reavaliar_ausencias_lancadas` :396; `fechar_chamados_ausencia` :397; `tripwire_vinculo_orfao` :435; `tripwire_status_ferias` :452; `tripwire_situacao_afastado` :459; `reconciliar_geofence` :486; `reconciliar_vinculo` :489; `detectar_intervalo_ausente` :491; `detectar_cluster_espurio` :598; `supra_juiz --encerrada --alarme` :600; `supra_juiz --alarme` :604; `reavaliar_ausencias_feriado` :608; `silenciar_chamados_isentos` :609; `fechar_becos_disputa` :663; `pipeline_placar` :745.
- Encadeados sem conclusão integral: `apurar_furos_diarios` :1471; `disparar_perguntas_competencia --comp-corrente --apply` :1483; `reconciliar_perguntas_orfas --alarme` :1488; `lavrar_deslogados` :1501; `lavrar_he_pendente` :1516; `vigiar_txt_dominio --alarme` :1554; `vigiar_pausa_export --alarme` :1618; `vigiar_retido_calado --alarme` :1624; `vigiar_carimbo_sem_registro --alarme` :1718; `vigiar_ausencia_x_veredito --alarme` :1725; `vigia_pareador_celula --alarme` :1731. Os outros três encadeados (`recusar_ponta_pequena`, `lavrar_retrato_folha`, `vigiar_fio_morto`) estão discutidos acima (`app/config/crons.py:1529`, `:1594`, `:1680`).

Limites: nenhum estado real, número de afetados, entrega de push, migração efetivamente aplicada, execução 2× ou quantidade efetivamente persistida foi observado **[não verificado]**. Os contra-exemplos são construídos a partir das condições do código, não registros do anexo. Não há especulação necessária para os sete mecanismos principais; falha eventual de serviço externo ou de trilha é **[não verificado]**. [nao verificado]

## 05/11 15:58 · sessao 1ea1ade925cb · CODEX-SUFOCA-02-leis-pela-metade · leitura · origem=Code

**Há dívidas confirmadas e fichas superadas.** L-051 já evita carimbar perguntas novas, L-101 já possui papel `prazo`, e a regeneração em competência entregue ganhou um corte posterior específico. Repetir os diagnósticos antigos produziria falsos achados. (`app/chamados/services/competencia_trancada.py:177`; `app/config/crons.py:1955`; `app/docs/CORTES.md:276`)

Reabri **36 entradas**: as encontradas pelos rótulos, incluindo o histórico de L-112, mais L-050 e L-113 pelas pendências declaradas fora da tabela principal. (`app/docs/LEIS.md:150`; `app/docs/LEIS.md:189`; `app/docs/LEIS.md:197`)

Os contraexemplos abaixo são **sintéticos, deduzidos dos ramos citados**. Nos caminhos corrigidos, apresento o caso antigo desarmado. **Alta** significa ramo explícito; **média**, consequência ou enquadramento ainda dependente de execução. Ocorrência desses casos em produção: **[nao verificado]**.

O dono indicado é o do **desvio de implementação**. Um cadastro errado usado no exemplo não transfere ao cadastro a responsabilidade por leitores incoerentes. A classificação de casos reais depende dos insumos do juiz de dono. (`app/ponto/management/commands/e6_oraculo.py:98`)

**Não consegui verificar:** execução das suítes, alcance na frota, casos nominais, preservação das alegadas 1.994 perguntas, quantidades atuais de leitores pendentes, saldos reais, integridade material dos TXT anteriores, entrega ao Domínio ou deploy. O anexo fornece endereços e contratos; esses resultados operacionais permanecem **[nao verificado]**. [nao verificado]

| achado | arquivo:linha | contra-exemplo | certeza | dono L-099 (ESTRUTURA/CADASTRO/BATIDA) |
|---|---|---|---|---|
| **L-002 — parcial confirmada.** Existe autoridade para dia sem trabalho. `_protesta` conserva uma decisão própria sobre a natureza do dia. Sítio pendente: esse consumidor; consequência final em folha não demonstrada. | `app/chamados/juizes.py:411`; `app/ponto/services/cartorio.py:1407`; `app/core/juizes.py:145`; `app/ponto/services/lastro.py:170` | Célula `trabalha=True`, ata de ausência integral e chamado legado sem `marcos_faltantes`: o juiz considera dia sem trabalho; `marcos_acesos=None` permite que `_protesta` continue verdadeiro. | Alta nos predicados; média no efeito final | ESTRUTURA |
| **L-007 — parcial; dívida antiga envelheceu.** A porta registra antes/depois quando recebe autor. O desligamento sem request ainda pode fechar vínculo sem trilha por PK. Migrar esse chamador para autoria sistêmica pela mesma porta. A família chamado já possui porta declarada na matriz. | `app/colaboradores/services/vinculo.py:614`; `app/colaboradores/services/desligamento.py:73`; `app/colaboradores/services/desligamento.py:122`; `app/core/contratos_estruturais.py:495` | Desligamento agendado sem request: `_ator=None`; vínculo ativo ganha `ativa=False` e `data_fim`, sem executar o registro por PK condicionado ao autor. | Alta | ESTRUTURA |
| **L-011 — parcial confirmada.** A trava compara autoridades com a base e procura o corte. Seu extrator ignora caminhos com dígitos e aspas duplas. Migrar o extrator; o caso adversarial atual testa `comm`, sem passar pela extração. | `bin/tests/test_juiz_novo_tem_corte.sh:28`; `bin/tests/test_juiz_novo_tem_corte.sh:38`; `bin/tests/test_juiz_novo_tem_corte.sh:57`; `app/core/juizes.py:203` | `'ponto/motor_calculo_v2.py::MotorBase.aplicar_tolerancia'` não casa com o regex; `'ponto/turnos.py::parear_turnos'` casa. Uma autoridade nova no primeiro formato escapa dessa trava. | Alta; regex conferido em memória | ESTRUTURA |
| **L-020 — parcial confirmada.** Geração comum conserva passado; regeneração normal denuncia a alteração. O ramo além do fim do vínculo grava `trabalha/dna` sem os campos de denúncia. Migrar esse ramo. | `app/ponto/portas/celula.py:606`; `app/ponto/portas/celula.py:225`; `app/ponto/portas/celula.py:259` | Vínculo termina em 20/10; célula de 21/10 ainda trabalhável, sem sucessor e sem lavra: vira folga com DNA novo, mantendo o contador de regenerações. | Alta | ESTRUTURA |
| **L-022 — parcial confirmada.** A foto manda dentro do mês fotografado. Antes desse ramo, mês sem foto pode retornar a paridade da âncora. Migrar essa restrição mensal. | `app/escala/models.py:1280`; `app/escala/models.py:1289`; `app/escala/models.py:1351` | Âncora 01/09, foto de **folga em 29/09**, consulta 01/10: âncora indica trabalho; herança da foto, dois dias depois da folga, indicaria folga. Sem foto alguma, uma âncora também produz booleano. | Alta | ESTRUTURA |
| **L-024 — parcial no tratamento da fonte indisponível.** O motor consulta o previsto canônico. Uma exceção vira ausência de resposta e permite usar `minutos_jornada`; Plantão ainda possui substituição por 480. Migrar esse fallback. | `app/ponto/motor_calculo_v2.py:1237`; `app/ponto/motor_calculo_v2.py:1254`; `app/ponto/motor_calculo_v2.py:1273`; `app/ponto/motor_calculo_v2.py:3501` | Fonte do previsto existe, mas levanta exceção: cadastro com jornada 720 passa pelo fallback; no ramo Plantão correspondente, pode resultar em 480. Efeito financeiro integral: [nao verificado]. | Alta no fallback; média no dinheiro | ESTRUTURA |
| **L-026 — parcial confirmada.** As janelas respeitam o corte da empresa. `_jornada_que_mente` ainda passa mês civil e `21` literal. Migrar esse leitor para a janela da empresa. | `app/ponto/janelas.py:54`; `app/ponto/janelas.py:60`; `app/escala/services/cadastro_realidade.py:199` | Em 25/10, empresa com corte 16: a janela corrente é 16/10–15/11; o leitor examina 21/09–20/10. Oito dias divergentes de 22–29/10 ficam fora da consulta. | Alta | ESTRUTURA |
| **L-030 — escrita centralizada, “nunca barrada” não é literal no runtime.** A API usa a porta, mas situação, GPS e colisão próxima ainda recusam registro. GPS possui exceção documental explícita; falta reconciliar o alcance da lei com esses portões. | `app/ponto/registro_batida.py:94`; `app/ponto/registro_batida.py:145`; `app/api/views_core.py:402`; `app/api/views_core.py:435`; `app/api/views_core.py:454` | Ativo tenta E sem latitude: 403, sem batida. Outro ramo: S real menos de 60 segundos após E, sem autorização humana, pode retornar `None`. | Alta no comportamento; alcance normativo condicionado | ESTRUTURA |
| **L-031 — cláusula original incompleta, com adiamento documentado.** O juiz entrega `indenizavel`, mas o motor indeniza o suprimido sem consultar essa flag. O próprio código condiciona a leitura a cadastro declarado com trilha. Falta completar esse cadastro e seu leitor. | `app/ponto/juiz_batida.py:310`; `app/ponto/motor_calculo_v2.py:822`; `app/ponto/motor_calculo_v2.py:2773`; `app/ponto/motor_calculo_v2.py:2784` | Template 07–19, pausa 12–13, `intervalo_indenizavel=False`, somente E07/S19: a flag não impede a indenização do intervalo suprimido. | Alta; pendência condicionada pelo adiamento | ESTRUTURA |
| **L-032 — parcial na subtração do intervalo cadastrado.** O juiz reconhece o intervalo cadastrado, mas o motor desconta somente intervalos reais. A defesa contra HE duplicada já existe; não reapresento essa duplicação como falha atual. | `app/ponto/juiz_batida.py:294`; `app/ponto/motor_calculo_v2.py:689`; `app/ponto/motor_calculo_v2.py:1803`; `app/ponto/motor_calculo_v2.py:2771`; `app/docs/CORTES.md:411` | 07–19, pausa cadastrada de 60 minutos, só E/S: `_intra_real=0`; o ramo conserva 720 minutos trabalhados e indeniza o suprimido. A cláusula literal de descontar o cadastrado não se realiza nesse ramo. | Alta no ramo; política posterior ressalvada | ESTRUTURA |
| **L-033 — retenção implementada; falha atual não demonstrada.** O retentor lê o juiz e isenta furo exclusivo da pausa. Existe cadeia de emissão para marcos faltantes. Não localizei leitor contrário nela; permanência da pergunta ponta a ponta: [nao verificado]. | `app/folha/export.py:1124`; `app/folha/export.py:1147`; `app/folha/export.py:1235`; `app/chamados/services/declaracao_texto.py:100`; `app/chamados/services/fabrica_da_celula.py:139` | Bordas acesas, miolo apagado, somente `FURO_PARCIAL`: não retém. Ata desalinhada ou protesto adicional não recebe a mesma isenção. | Alta na retenção; média na cadeia completa | ESTRUTURA — verificação residual |
| **L-035 — parcial confirmada.** A política central distingue CLT declarado. Com switch CCT desligado, empresa não CLT pode chegar ao motor com parâmetro indefinido, convertido pela identificação do ciclo. Migrar esse caminho da fábrica. | `app/core/regua_cct.py:153`; `app/core/regua_cct.py:211`; `app/core/regua_cct.py:449`; `app/ponto/motor_calculo_v2.py:758` | Empresa declarada `cct`, ciclo 12×36, switch desligado, sem parâmetro explícito: fábrica deixa `None`; motor aplica ausência de prorrogação pelo ciclo, sem CLT declarado. | Alta | ESTRUTURA |
| **L-039 — parcial confirmada no PDF sem fonte de folha.** A falta decidida existe, mas o PDF inicializa totais pelo motor. A sobrescrita depende de competência reconhecida e fonte disponível. Migrar totais de período livre/sem apuração. | `app/relatorios/pdf_espelho.py:461`; `app/relatorios/pdf_espelho.py:471`; `app/relatorios/cartao_pela_celula.py:203`; `app/relatorios/cartao_pela_celula.py:321`; `app/relatorios/cartao_pela_celula.py:418` | Período livre de dois dias previstos, sem batidas nem ausência aprovada: os totais podem preservar “Faltas” do motor enquanto o mesmo furo aparece em “Em aberto”. Renderização integral: [nao verificado]. | Alta na origem dos totais; média na apresentação final | ESTRUTURA |
| **L-040 — lista/PDF compartilham estrutura; universo integral não demonstrado.** A lista agrega cadastro e lavras; o PDF percorre seus dados. A lavra intermediária filtra repetições e não ingere integralmente `dias_cadastro_x_realidade`. Falta essa ligação completa. | `app/escala/services/cadastro_realidade.py:555`; `app/escala/services/cadastro_realidade.py:635`; `app/escala/services/cadastro_realidade.py:789`; `app/ponto/services/esmeril_espelho.py:159`; `app/ponto/motor_calculo_v2.py:1670` | Um único dia acusado pelo motor, sem outras assinaturas cadastrais e abaixo dos limiares de três repetições, não tem entrada demonstrada na lista. **[especulação]** Alcance total dessa omissão na frota. | Média | ESTRUTURA — integração da lista |
| **L-041 — parcial confirmada.** Existe propositor por vínculo. C1/VINCULO-FIM apenas formata a impossibilidade e fornece link genérico; outro propositor fornece destinos ao esmeril. Migrar a classe ao decisor canônico e consolidar os decisores. | `app/escala/services/propositor.py:382`; `app/escala/services/cadastro_realidade.py:250`; `app/escala/services/cadastro_realidade.py:449`; `app/ponto/services/esmeril_espelho.py:213` | Vínculo com início 25/10 e fim 31/08: C1 apresenta datas e “Corrigir a vigência”, sem decisão do propositor para essa classe. | Alta | ESTRUTURA |
| **L-050 — pendência histórica desarmada nos sítios examinados.** A fábrica não possui a antiga fronteira de exportação; cobrança manual atravessa pelo toque. Não localizei migração restante nesses sítios. A seção de dívidas contradiz a reauditoria principal. | `app/chamados/services/fabrica_da_celula.py:89`; `app/chamados/services/fabrica_da_celula.py:126`; `app/chamados/services/cobrar_dia.py:39`; `app/docs/LEIS.md:91`; `app/docs/LEIS.md:189` | Dia passado exportado, motivo vencido e ainda não perguntado: exportação não é veto naquela função; o toque manual chega ao cartório. O contraexemplo antigo perdeu essa barreira. | Alta no caminho lido | ESTRUTURA — índice |
| **L-051 — deixou de ser só no papel.** Encerrar competência não escreve a via nas perguntas; o juiz lê a tranca. O teste antigo foi invertido. Reabertura explícita ainda pode limpar legado, compatível com reversão por toque. | `app/chamados/services/competencia_trancada.py:177`; `app/chamados/juizes.py:917`; `app/chamados/models.py:2514`; `app/chamados/tests/test_competencia_trancada.py:122`; `app/chamados/services/competencia_trancada.py:251` | Pergunta nova viva seguida de tranca: o caminho deixa `via_resolucao` vazia e consulta a tranca. Preservação das 1.994 perguntas históricas: [nao verificado]. | Alta no código; passivo [nao verificado] | ESTRUTURA — índice/passivo |
| **L-052 — literal antigo superado; guarda atual ratificada.** Regeneração comum pula lavra; porta excepcional abre com motivo e trilha. O corte de 24/10 confirma TXT entregue ou holerite publicado como guarda. Não há violação atual demonstrada apenas pela existência dessas duas portas. | `app/ponto/portas/celula.py:238`; `app/ponto/portas/regen_exportada.py:174`; `app/ponto/portas/regen_exportada.py:199`; `app/folha/export.py:598`; `app/docs/CORTES.md:276` | Mesmo dia lavrado: regeneração comum pula; retificação autorizada passa `apesar_da_lavra=motivo`. Esse par demonstra a exceção, sem provar descumprimento vigente. | Alta na implementação e no corte | ESTRUTURA — consolidação documental |
| **L-060 — parcial, com três resíduos declarados.** Papel e contador existem. A tupla atual contém três nomes, não 27: disparar perguntas, fechar cobrança com lastro e reconciliar chamados. Migrar os respectivos escritores residuais para evento/consumidor. | `app/config/crons.py:1999`; `app/config/crons.py:2004`; `app/config/crons.py:2018`; `app/chamados/management/commands/disparar_perguntas_competencia.py:264` | Chamado vivo sem pergunta viva — fio mudo — ainda faz a varredura chamar `perguntar` e escrever estado. “Três” é contagem da tupla da cópia, sem medição de execução em produção. | Alta | ESTRUTURA |
| **L-061 — parcial confirmada, compartilhando o desvio de L-002.** A fábrica já lê `dia_sem_trabalho`; `_protesta` conserva somente a cláusula `trabalha`. Sítio pendente explicitamente declarado no registro. Ausência parcial não é contraexemplo: o juiz manda cobrar o restante. | `app/chamados/services/fabrica_da_celula.py:89`; `app/chamados/juizes.py:398`; `app/chamados/juizes.py:426`; `app/ponto/services/cartorio.py:1408`; `app/core/juizes.py:145` | `trabalha=True`, ata de ausência integral, chamado legado sem marcos nomeados: o consumidor ainda considera protestante um chamado em dia que o juiz declara sem trabalho. | Alta nos predicados | ESTRUTURA |
| **L-073 — furo noturno antigo corrigido; veto civil residual.** Cartório protege ontem por instante/turno encerrado; supra recebe essa decisão. `cobrar_dia` ainda recusa todo dia corrente pela data. **[especulação de enquadramento]** Esse veto residual pertence à mesma exigência de encerramento por turno. | `app/ponto/services/cartorio.py:1318`; `app/ponto/services/cartorio.py:1347`; `app/ponto/supra_juiz.py:273`; `app/chamados/services/cobrar_dia.py:26` | Turno de hoje 07–15, agora 16h, já encerrado: botão responde “dia ainda não terminou”. O antigo caso noturno 19–07, observado às 02h, já possui guarda. | Alta nos ramos; média no enquadramento do botão | ESTRUTURA |
| **L-074 — parcial confirmada.** Trava e pre-push preservam o rc. Régua, isolamento e sondas ainda decidem por conteúdo de log. Calendário também executa suíte sem trava, embora use banco próprio. Migrar vereditos ao rc; não inferir colisão de banco desse último caso. | `bin/trava_teste.sh:35`; `bin/pre-push.sh:141`; `bin/regua.sh:165`; `bin/regua.sh:179`; `bin/isolamento.sh:34`; `bin/regua_relogio.sh:67`; `bin/regua_calendario.sh:43` | Runner imprime `Ran ...` e `OK`, depois termina com rc=1: a régua pode gravar `ST=OK`, pois não captura o rc e aceita o texto. | Alta | ESTRUTURA |
| **L-076 — parcial confirmada.** Consumidores usam env-file protegido, mas o nascimento do banco passa `POSTGRES_PASSWORD` no argv. O selo procura somente a forma `DB_PASSWORD`. Migrar criação e cobertura do selo. | `bin/recursos.sh:119`; `bin/recursos.sh:134`; `bin/db_teste.sh:55`; `bin/db_teste.sh:63`; `bin/tests/test_segredo_fora_do_argv.sh:19`; `bin/tests/test_segredo_fora_do_argv.sh:24` | Container de teste inexistente: o ramo de criação executa Docker com `-e POSTGRES_PASSWORD=...`; esse nome escapa ao padrão fiscalizado. Nenhum segredo foi consultado. | Alta | ESTRUTURA |
| **L-085 — parcial no segundo cálculo de DSR.** A geometria atribui o dia da jornada; o comercial constrói esse mapa, mas depois recria trabalho/extras pela data civil da entrada. Migrar esse segundo cálculo. | `app/ponto/turnos.py:283`; `app/ponto/motor_calculo_v2.py:3313`; `app/ponto/motor_calculo_v2.py:3441`; `app/ponto/motor_calculo_v2.py:3453`; `app/ponto/motor_calculo_v2.py:3466` | Juiz atribui jornada ao domingo D; entrada real ocorre após meia-noite de segunda. Com previsto em D, a recontagem civil só vê D+1 e pode considerar D ausente. | Alta na derivação divergente; média no DSR final | ESTRUTURA |
| **L-087 — parcial confirmada.** Ata governa no ramo coberto. Turno partido, falta de célula e ata aberta/desalinhada ainda abandonam essa substituição; o pareador volta ao tipo gravado. Migrar esses estados/regimes. | `app/ponto/motor_calculo_v2.py:559`; `app/ponto/motor_calculo_v2.py:573`; `app/ponto/motor_calculo_v2.py:632`; `app/ponto/turnos.py:1380`; `app/ponto/turnos.py:1680` | E07:01/S13:00/S13:59/E14:59, sem célula alimentada: o fallback conserva o tipo como insumo decisor. Quantidade final de períodos nesse exemplo: [nao verificado]. | Alta no fallback | ESTRUTURA |
| **L-089 — parcial confirmada no direito de férias.** A grade trata folga diante de falta aprovada, mas o contador aquisitivo conta toda linha de falta aprovada na janela. Migrar esse contador ao universo de dias de trabalho. A classificação de outras ausências possui precedência documentada própria. | `app/escala/utils.py:1231`; `app/escala/utils.py:1240`; `app/ferias/models.py:141`; `app/ferias/models.py:54`; `app/ferias/services.py:243` | Cinco faltas aprovadas em dias de trabalho mais uma falta aprovada em folga: contador chega a seis e a tabela reduz o direito de 30 para 24 dias. | Alta | ESTRUTURA |
| **L-090 — parcial confirmada, sem repetir diagnóstico antigo inteiro.** Pontualidade já consulta a competência; assiduidade continua começando no dia 1. Absenteísmo também recorta mês civil. Migrar esses dois universos; a exceção do holerite não os cobre. | `app/inteligencia/management/commands/calcular_scores.py:57`; `app/inteligencia/management/commands/calcular_scores.py:72`; `app/inteligencia/management/commands/calcular_scores.py:93`; `app/relatorios/views.py:195` | Hoje 05/10, corte 21, falta não abonada em 25/09: pertence à competência corrente, mas fica fora da assiduidade iniciada em 01/10. | Alta | ESTRUTURA |
| **L-091 — parcial confirmada.** O desenho corrige alguns templates rotulados `turno_partido`; o restante ainda despacha pelo mapa de rótulos. Migrar a seleção geral da fábrica ao desenho. | `app/ponto/motor_calculo_v2.py:3625`; `app/ponto/motor_calculo_v2.py:3635`; `app/ponto/motor_calculo_v2.py:3663`; `app/ponto/motor_calculo_v2.py:3668` | Template `tipo_base='comercial'`, dois blocos separados por quatro horas: não entra na correção geométrica reservada ao rótulo partido e recebe MotorComercial. | Alta | ESTRUTURA |
| **L-092 — diagnóstico “ninguém cria pauta” superado.** O fluxo de passivo validado retifica e cria Pauta com pago/devido e régua dos dois lados. A proibição genérica foi alterada por L-113. Não localizei falha nesse encadeamento; cobertura de todos os chamadores: [nao verificado]. | `app/chamados/services/passivo_validado.py:178`; `app/chamados/services/passivo_validado.py:192`; `app/chamados/services/passivo_validado.py:438`; `app/ponto/portas/regen_exportada.py:216`; `app/docs/LEIS.md:149` | Retificação exportada com `mudou=True`: o chamador executa `_pauta_da_competencia`. A porta isolada apenas devolve os números; é o chamador que completa o contrato. | Alta nesse fluxo | ESTRUTURA — índice/cobertura |
| **L-095 — parcial na preservação das revisões.** A lavra grava `versao_motor`, mas apaga e recria pela chave `versao`. Falta preservar revisões e DIFF por rubrica. Leitura online, isoladamente, possui corte posterior e não prova violação. | `app/ponto/services/dia_pago.py:333`; `app/ponto/services/dia_pago.py:345`; `app/ponto/servic

[truncado em 20000 de 24291 caracteres]

## 05/11 15:58 · sessao beb743e71c01 · LASTRO-PARTE-2 · leitura · origem=Code

**Certeza alta sobre os caminhos da cópia; autoria do acendimento em #33126 e #36045: [nao verificado].** `acesa=True, luz=None` não identifica sozinho o produtor: o cartório admite uma coluna sem hora, e a dispensa também escreve esse par explicitamente. `app/ponto/services/cartorio.py:647,677-679,773-776`.

| Caminho | Escritor e condição | Rastro que permite distinguir depois |
|---|---|---|
| Cartório, montagem comum | `lampadas_do_dia()` considera aceso um `dict` sem `missing` verdadeiro; `luz` recebe `x.get('hora')`, que pode ser `None`. Há também o ramo sem casamento pelo DNA. `app/ponto/services/cartorio.py:647,677-691`. | Esse ramo **não acrescenta origem nem ID de Batida** à lâmpada; copia `tipo_real` e propaga eventuais selos. Sem selo, o par sozinho permanece ambíguo. `app/ponto/services/cartorio.py:556-562,677-691`. |
| Dispensa | `dispensar_por_ausencia()` transforma somente lâmpadas ainda não acesas em `acesa=True, luz=None`. `app/ponto/services/cartorio.py:773-776`. | `lampada.dispensada`: tipo da ausência ou `'justificativa'`. É uma marca do mecanismo, sem ID do registro na lâmpada. `app/ponto/services/cartorio.py:770-776`. |
| Ausência aprovada | Dia inteiro: cobertura aprovada e sem `cobertura_parcial`, pela dispensa acima. Parcial: a grade substitui o marco faltante dentro do período por `missing=False, hora=None`; o cartório o acende e transporta o selo. `app/ponto/services/cartorio.py:766-776`; `app/escala/utils.py:1251-1271`; `app/ponto/services/cartorio.py:677-680`. | `dispensada=<tipo>`; na parcial, também `ata.cobertura_parcial/periodo_parcial`. A célula recebe `ausencia_id`, `insumos_em`, `julgada_em` e `impressao`. O registro tem responsáveis, aprovação e datas. `app/ponto/services/cartorio.py:732-737`; `app/ponto/portas/celula.py:803-815`; `app/ponto/models.py:749-812`. |
| Justificativa aprovada | A grade consulta justificativas aprovadas pelo `data_turno` e marca `justificado`; a dispensa escreve as lâmpadas ainda não acesas. `app/escala/utils.py:1217-1220,1275-1280`; `app/ponto/services/cartorio.py:768-776`. | `dispensada='justificativa'`. No registro: `aprovada_por/em`, `batida_gerada_id`, `chamado_origem_id`, criação e atualização; aprovação pelo serviço deixa LogAuditoria. Não há campo próprio de criador na classe. `app/ponto/models.py:159-180`; `app/ponto/services/justificativa.py:214-217`. |
| Abono | É `Ausencia.tipo='abono'`; segue o mecanismo de cobertura/dispensa, sem escritor separado de lâmpada. `app/ponto/models.py:660-673`; `app/ponto/services/cartorio.py:766-776`. | `dispensada='abono'`, quando essa ausência fornece o motivo; responsáveis e datas ficam na própria Ausencia. `app/ponto/services/cartorio.py:770-776`; `app/ponto/models.py:797-812`. |
| Edição de célula | A cópia declara `editada_em/editada_por` **sem escritor de produção**. Não há sustentação para atribuir a uma porta implementada de edição humana esse acendimento. `app/escala/tests/test_d3_guarda_da_linha_celula.py:50-51,71-72`. | Existem `origem='editada'`, `editada_em/editada_por`; preenchimento histórico e eventual escrita externa: **[nao verificado]**. `app/escala/models.py:1690-1691,1732-1735`. |
| Troca de escala | É gatilho indireto: executor regenera e chama pós-vínculo, que pode forçar julgamento pelo cartório. O escritor da lâmpada continua sendo o cartório. `app/colaboradores/services/vinculo.py:311-331`; `app/colaboradores/services/pos_vinculo.py:61-74`. | Regeneração normal: `dna_anterior`, `regenerada_em`, `regeneracoes`, DNA e vínculo gerador; logs no EscalaColaborador. A troca também registra HistoricoVinculo e log criar/editar do vínculo. Isso identifica operações, sem provar sozinho qual acendeu. `app/ponto/portas/celula.py:251-265,297-315`; `app/colaboradores/services/vinculo.py:333-366`. |

A persistência comum é **montar ata → `lavrar_veredito(..., save=False, ata=...)` → `carimbar()` → `salvar_e_emitir()`**. A porta atribui `cel.ata`, salva e emite EventoCelula quando ata/veredito entram nos campos lavrados. `app/ponto/services/cartorio.py:1800-1823`; `app/ponto/portas/celula.py:775-783,813-815,642-650`.

Os contraexemplos impedem atribuição automática:

- **Sem dispensa:** `x={}` satisfaz o teste de acesa e produz `luz=None`, sem `dispensada`. A ocorrência histórica dessa entrada nos dois casos é **[nao verificado]**. `app/ponto/services/cartorio.py:647,677-680`.
- **Ausência presente não prova autoria:** a dispensa preserva uma lâmpada já acesa, sem acrescentar selo. Havendo ausência inteira e justificativa simultâneas, o tipo da ausência vence na escolha do motivo. `app/ponto/services/cartorio.py:766-776`.
- **Justificativa aprovada não implica ausência de hora:** a aprovação pode criar uma Batida e preencher `batida_gerada`. `app/ponto/services/justificativa.py:173-193`.
- **Carimbos não bastam:** o ramo de regeneração fora da vigência altera DNA/trabalho sem atualizar `regenerada_em`; EventoCelula não guarda ata anterior/nova nem ator e também pode nascer da abertura de chamado ou semeadura. `app/ponto/portas/celula.py:225-236,634-639`; `app/escala/models.py:1845-1860`.
- **A trilha pode não resolver:** o log da lavra guarda veredito/via/protestos/status, sem as lâmpadas; a auditoria é best-effort. Ausência de log não exclui operação. `app/ponto/portas/celula.py:785-788`; `app/core/observ.py:105,133-139`.

A sonda abaixo usa responsáveis por **ID**, preserva a história relacionada fora do dia civil e marca `criado_no_dia_civil`. A busca adicional de vínculo legado/texto é explicitamente candidata: há logs antigos com ID do colaborador sob EscalaColaborador, e `pos_vinculo` registra o colaborador apenas na descrição. `app/core/models.py:295-307`; `app/escala/services/vigencia_sem_trilha.py:43-47`; `app/colaboradores/services/pos_vinculo.py:91-92`.

Execute o bloco Python a partir de `app/`, por entrada padrão: `PYTHONDONTWRITEBYTECODE=1 python3 -B -`. Ele chama o próprio `manage.py shell`; o arranque desativa os handlers em arquivo e suprime somente a criação de `/app/logs` em memória, pois o settings normal faz essas operações. Execução contra o banco e resultados: **[nao verificado]**. `app/manage.py:9,18`; `app/config/settings/saas.py:118,138-139,162-163`.

===SONDA===
```python
import sys
sys.dont_write_bytecode = True
import os
import runpy
from unittest.mock import patch

# Arranque sem arquivos: config/settings/saas.py:118,138,162.
def guarda_fs(evento, args):
    if evento == "open":
        _, modo, flags = args
        escrita = os.O_WRONLY | os.O_RDWR | os.O_CREAT | os.O_TRUNC | os.O_APPEND
        if (modo and any(c in modo for c in "wax+")) or ((flags or 0) & escrita):
            raise RuntimeError("Sonda: escrita em arquivo bloqueada")
    if evento in {
        "os.mkdir", "os.remove", "os.rmdir", "os.rename", "os.link",
        "os.symlink", "os.truncate", "os.chmod", "os.chown", "os.utime",
    }:
        raise RuntimeError("Sonda: mutacao de filesystem bloqueada")

sys.addaudithook(guarda_fs)
os.environ.setdefault("DJANGO_SETTINGS_MODULE", "config.settings.saas")
makedirs_original = os.makedirs

def sem_diretorio_de_logs(path, *args, **kwargs):
    if os.fspath(path) == "/app/logs":
        return
    return makedirs_original(path, *args, **kwargs)

with patch("os.makedirs", sem_diretorio_de_logs):
    from django.conf import settings
    settings.LOGGING_CONFIG = None

SONDA = r'''
import datetime as dt
import json
from zoneinfo import ZoneInfo
from django.db import connection, transaction
from django.db.models import Q
from django.utils import timezone
from django_tenants.utils import schema_context
from chamados.models import ChamadoColaborador
from chamados.juizes import dia_do_fato
from colaboradores.models import HistoricoVinculo
from escala.models import CelulaDia, EscalaColaborador, EventoCelula
from ponto.models import Batida, Ausencia, Justificativa
from core.models import LogAuditoria

def json_default(v):
    if isinstance(v, dt.datetime):
        if timezone.is_aware(v):
            return timezone.localtime(v).isoformat()
        return {"datetime_naive": v.isoformat()}
    if isinstance(v, (dt.date, dt.time)):
        return v.isoformat()
    raise TypeError(type(v).__name__)

def emitir(rotulo, dado):
    print(rotulo, json.dumps(dado, ensure_ascii=False, default=json_default))

def linhas(qs, campos):
    return list(qs.values(*campos))

# Campos: escala/models.py:1681-1805.
CAMPOS_CEL = (
    "id", "colaborador_id", "data", "trabalha", "origem",
    "escala_geradora_id", "dna", "gerada_em", "editada_em", "editada_por_id",
    "regenerada_em", "regeneracoes", "dna_anterior", "julgada_em", "impressao",
    "veredito", "veredito_via", "veredito_em", "protestos", "ata",
    "ausencia_id", "insumos_em", "tem_entrada", "tem_saida",
)
# Campos: ponto/models.py:22-79,136-180,743-812.
CAMPOS_BAT = (
    "id", "timestamp", "tipo", "sequencia", "is_intervalo", "origem",
    "pergunta_origem_id", "aprovada", "aprovada_por_id", "aprovada_em",
    "retratada_em", "criado_em",
)
CAMPOS_AUS = (
    "id", "tipo", "data", "data_fim", "minutos", "dias_corridos",
    "hora_ini", "hora_fim", "status", "registrado_por_id", "aprovado_por_id",
    "aprovada_em", "criado_em", "atualizado_em", "chamado_origem_id",
    "documento_referencia_id",
)
CAMPOS_JUS = (
    "id", "colaborador_id", "data_turno", "tipo", "status", "criado_em",
    "atualizado_em", "aprovada_por_id", "aprovada_em", "batida_entrada_id",
    "batida_gerada_id", "saida_informada", "chamado_origem_id",
)
# Campos: core/models.py:295-307; escala/models.py:1854-1860.
CAMPOS_LOG = (
    "id", "criado_em", "acao", "modelo", "objeto_id", "usuario_id", "ator",
    "descricao", "valor_antes", "valor_depois",
)
CAMPOS_EV = (
    "id", "celula_id", "colaborador_id", "data", "criado_em",
    "processado_em", "tentativas", "erro",
)

if connection.vendor != "postgresql" or not hasattr(connection, "set_schema"):
    raise RuntimeError("Sonda requer PostgreSQL com django-tenants")

with timezone.override(ZoneInfo("America/Sao_Paulo")), schema_context("juliani"):
    with transaction.atomic():
        with connection.cursor() as cur:
            cur.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
            cur.execute("SHOW transaction_read_only")
            assert cur.fetchone()[0] == "on"
            cur.execute("SELECT current_schema()")
            assert cur.fetchone()[0] == "juliani"

        # Numeros tratados como PK, exibindo GLPI para conferir identidade:
        # chamados/models.py:32-45. Dia/ano: chamados/juizes.py:78-119.
        for numero, esperado in ((33126, (21, 10)), (36045, (30, 10))):
            ch = ChamadoColaborador.objects.select_related("celula").get(pk=numero)
            dia = dia_do_fato(ch, fortes=True)
            emitir("ALVO", {
                "chamado_pk": ch.pk, "ticket_id_glpi": ch.ticket_id_glpi,
                "colaborador_id": ch.colaborador_id, "dia": dia,
                "celula_id": ch.celula_id,
            })
            if dia is None or (dia.day, dia.month) != esperado:
                emitir("ALVO_NAO_CONFIRMADO", "[nao verificado]; conferir PK e data")
                continue

            cid = ch.colaborador_id
            inicio = timezone.make_aware(dt.datetime.combine(dia, dt.time.min))
            fim = timezone.make_aware(dt.datetime.combine(
                dia + dt.timedelta(days=1), dt.time.min))

            celulas = linhas(CelulaDia.objects.filter(
                colaborador_id=cid, data=dia).order_by("id"), CAMPOS_CEL)
            emitir("CELULAS", celulas)
            for cel in celulas:
                emitir("ATA", {"celula_id": cel["id"], "ata": cel["ata"]})
                for indice, lamp in enumerate((cel["ata"] or {}).get("lampadas") or []):
                    emitir("LAMPADA", {
                        "celula_id": cel["id"], "indice": indice, "campos": lamp})

            # Dia CIVIL local; bordas sao contexto, sem atribuicao de turno.
            # Fuso: config/settings/base.py:93-95.
            batidas = linhas(Batida.objects.filter(
                colaborador_id=cid, timestamp__gte=inicio-dt.timedelta(days=1),
                timestamp__lt=fim+dt.timedelta(days=1)
            ).order_by("timestamp", "id"), CAMPOS_BAT)
            emitir("BATIDAS_DO_DIA", [
                b for b in batidas if inicio <= b["timestamp"] < fim])
            emitir("BATIDAS_BORDAS_MAIS_MENOS_1_DIA", [
                b for b in batidas if not inicio <= b["timestamp"] < fim])

            # Geometria temporal existente: ponto/turnos.py:2552-2557.
            # Sem filtro de status: inspeciona tambem pendentes/rejeitadas.
            cobertura = (
                Q(data=dia)
                | Q(data__lte=dia, data_fim__gte=dia)
                | Q(data__lte=dia, data_fim__isnull=True,
                    tipo__in=Ausencia.TIPOS_EM_ABERTO)
            )
            ausencias = linhas(Ausencia.objects.filter(
                colaborador_id=cid).filter(cobertura).order_by("data", "id"),
                CAMPOS_AUS)
            emitir("AUSENCIAS_CANDIDATAS_TEMPORAIS", ausencias)
            emitir("ABONOS", [a for a in ausencias if a["tipo"] == "abono"])

            justificativas = linhas(Justificativa.objects.filter(
                colaborador_id=cid, data_turno=dia).order_by("criado_em", "id"),
                CAMPOS_JUS)
            emitir("JUSTIFICATIVAS", justificativas)
            emitir("AUTOR_CRIACAO_JUSTIFICATIVA",
                   "[nao verificado]; model nao possui criado_por")

            # Contexto para troca: colaboradores/models.py:565-584.
            historico = linhas(HistoricoVinculo.objects.filter(
                colaborador_id=cid).order_by("criado_em", "id"), (
                    "id", "tipo", "data", "vigencia_ini", "vigencia_fim",
                    "de", "para", "motivo", "obs", "registrado_por_id", "criado_em"))
            emitir("HISTORICO_VINCULO_CONTEXTO", historico)

            # Pelo par, inclusive eventos cujo celula_id ja nao existe:
            # escala/models.py:1845-1860.
            eventos = linhas(EventoCelula.objects.filter(
                colaborador_id=cid, data=dia).order_by("criado_em", "id"),
                CAMPOS_EV)
            ids_cel = {c["id"] for c in celulas} | {e["celula_id"] for e in eventos}
            ids_ch = set(ChamadoColaborador.objects.filter(
                colaborador_id=cid).filter(
                    Q(pk=ch.pk) | Q(celula_id__in=ids_cel)
                    | Q(contexto_json__data_turno=dia.isoformat())
                ).values_list("pk", flat=True))
            ids_ec = set(EscalaColaborador.objects.filter(
                colaborador_id=cid).values_list("pk", flat=True))
            ids_aus = {a["id"] for a in ausencias}
            ids_aus |= {c["ausencia_id"] for c in celulas if c["ausencia_id"]}

            refs = {
                "Colaborador": {cid}, "CelulaDia": ids_cel,
                "ChamadoColaborador": ids_ch, "EscalaColaborador": ids_ec,
                "Ausencia": ids_aus, "Justificativa": {j["id"] for j in justificativas},
                "Batida": {b["id"] for b in batidas},
                "HistoricoVinculo": {h["id"] for h in historico},
            }
            filtro = Q(pk__in=[])
            for modelo, ids in refs.items():
                filtro |= Q(modelo=modelo, objeto_id__in=ids)

            # Candidatos legados/textuais, nao atribuicao:
            # escala/services/vigencia_sem_trilha.py:43-47;
            # colaboradores/services/pos_vinculo.py:91-92.
            filtro |= Q(modelo="EscalaColaborador", objeto_id=cid)
            filtro |= Q(acao="pos_vinculo",
                        descricao__regex=rf"col{cid}([^0-9]|$)")
            logs = linhas(LogAuditoria.objects.filter(filtro).order_by(
                "criado_em", "id"), CAMPOS_LOG)

            trilha = []
            for log in logs:
                direto = log["objeto_id"] in refs.get(log["modelo"], set())
                log["vinculo_da_busca"] = (
                    "objeto relacionado; nao prova ato sobre este dia"
                    if direto else "candidato legado/textual [nao verificado]")
                trilha.append(("LOG", log))
            trilha.extend(("EVENTO_CELULA", ev) for ev in eventos)
            trilha.sort(key=lambda item: (
                item[1]["criado_em"], item[0], item[1]["id"]))

            emitir("TRILHA_CONTAGENS", {"logs": len(logs), "eventos": len(eventos)})
            for especie, registro in trilha:
                registro["criado_no_dia_civil"] = (
                    inicio <= registro["criado_em"] < fim)
                emitir(especie, registro)
'''

# Invoca manage.py shell sem arquivo temporario: manage.py:18.
sys.argv = ["manage.py", "shell", "-c", SONDA]
runpy.run_path("manage.py", run_name="__main__")
```
===FIM===

## 05/11 15:57 · sessao 88bc2abdbb23 · CODEX-SUFOCA-05-borda-competencia · leitura · origem=Code

**Os riscos mais fortes estão na empresa atual usada para interpretar o passado e na partição de vínculos sobrepostos. A aritmética central 20/21 tem limites inclusivos corretos.** (`app/ponto/services/fechamento.py:612`; `app/core/regua_cct.py:352`; `app/ponto/services/fechamento.py:232`; `app/ponto/janelas.py:15`; `app/ponto/janelas.py:27`)

Os contraexemplos abaixo são sintéticos. Incidência, quantidade de pessoas e prejuízo em produção: **[não verificado]**. O anexo fornecido contém o lookup dos juízes, sem cadastros, batidas ou estados individuais. **[não verificado]**

**1. Troca de CNPJ desloca o fechamento inteiro para a empresa atual.**

A transferência substitui `Colaborador.empresa`; o universo do fechamento e o export filtram essa empresa atual. O fechamento é único por colaborador/mês/ano, sem dimensão de empresa nessa chave. (`app/colaboradores/views.py:1385`; `app/ponto/services/fechamento.py:435`; `app/folha/export.py:767`; `app/ponto/models.py:485`)

**Contraexemplo:** A e B têm corte 21. Trabalho com pares completos em A de 21/09 a 04/10/2026; transferência para B em 05/10; trabalho em B até 20/10. Depois da transferência, o fechamento de outubro deixa de ser selecionado em A e passa a ser selecionado integralmente em B, inclusive pelos dias anteriores à transferência. **Certeza alta; ESTRUTURA.** (`app/folha/export.py:701`; `app/folha/export.py:767`)

Com cortes diferentes, muda também o intervalo: A=21 e B=10 fazem o recálculo de outubro trocar **21/09–20/10 por 10/09–09/10**, pois a janela inteira usa a empresa atual. (`app/ponto/services/fechamento.py:612`; `app/ponto/janelas.py:16`; `app/ponto/janelas.py:20`)

**Especulação rotulada:** um TXT anterior de A e outro posterior de B podem conservar valores sobrepostos; a vigência do TXT é decidida separadamente por empresa. A dupla exportação e o pagamento efetivo não foram demonstrados. (`app/folha/models.py:134`; `app/folha/models.py:182`) **[não verificado]**

**2. A régua histórica encontra o posto antigo, mas aplica a empresa atual.**

`regua_para` resolve vínculo e praça na data solicitada, porém escolhe regime e aplicação de convenção por `colaborador.empresa`. O fechamento passa o mesmo colaborador atual para os motores das fatias históricas. (`app/core/regua_cct.py:329`; `app/core/regua_cct.py:352`; `app/core/regua_cct.py:366`; `app/ponto/services/fechamento.py:727`)

**Contraexemplo:** A possui regime CCT e aplicação vigente; B possui regime CLT. Transferência em 05/10, mantendo vínculo, células e batidas anteriores. Com o switch da régua ativo, recalcular uma fatia anterior a 05/10 encontra a praça antiga, mas retorna a régua CLT de B. **Certeza alta para a seleção indevida da empresa histórica; ESTRUTURA.** (`app/core/regua_cct.py:330`; `app/core/regua_cct.py:354`; `app/core/regua_cct.py:458`)

**Especulação rotulada:** alteração de adicional noturno, HE ou feriado depende dos parâmetros divergentes entre A e B; valor financeiro **[não verificado]**.

**3. Depois da troca de empresa, a trava de uma batida histórica pode consultar outra competência.**

`competencia_fechada` combina a data local da batida com a empresa **atual** do colaborador. O flip aceita ou recusa a alteração conforme essa resposta. (`app/ponto/janelas.py:84`; `app/ponto/janelas.py:85`; `app/ponto/services/flip_batida.py:22`)

**Contraexemplo:** A corte 21; B corte 10; batida local de 15/09/2026, quando a pessoa estava em A; transferência em 05/10. Fechamento 09 aprovado e 10 aberto. Pelo corte A, a batida pertence a setembro; após a transferência, a trava consulta outubro e permite prosseguir com o flip. Invertendo os estados dos fechamentos, ocorre a recusa pelo mês errado. **Certeza alta; ESTRUTURA.** (`app/ponto/janelas.py:24`; `app/ponto/janelas.py:85`; `app/ponto/janelas.py:87`; `app/ponto/services/flip_batida.py:23`; `app/ponto/services/flip_batida.py:30`)

**Especulação rotulada:** ocorrência desse estado em produção **[não verificado]**.

**4. Sem células, dois vínculos sobrepostos recebem as mesmas batidas.**

A exclusividade de `apurar_por_fatia` depende de `_julgados`, preenchido pelos dias com geradora. Sem esses dias, cada vínculo executa seu intervalo integral e os períodos retornados são concatenados. (`app/ponto/services/fechamento.py:189`; `app/ponto/services/fechamento.py:203`; `app/ponto/services/fechamento.py:232`; `app/ponto/services/fechamento.py:277`; `app/ponto/services/fechamento.py:285`)

**Contraexemplo:** vínculo A inativo, com vigência 21/09–20/10/2026; B ativo desde 20/10; nenhuma célula; batidas 20/10 às 08h E e 17h S. Ambos são candidatos válidos ao carregamento, mas o juiz do dia escolhe somente B. O apurador entrega o par aos dois motores. **Certeza alta para dupla entrega e composição; ESTRUTURA.** (`app/escala/alimentacao.py:148`; `app/escala/alimentacao.py:214`; `app/escala/alimentacao.py:285`; `app/ponto/services/fechamento.py:244`)

A reprodução em memória, extraindo as funções da cópia e usando motor instrumentado, confirmou as duas entregas; não executou o motor financeiro real. **[não verificado por arquivo:linha]** A sobreposição inicial é problema de CADASTRO; contrariar a escolha única do juiz na apuração é ESTRUTURA. (`app/escala/alimentacao.py:286`; `CLAUDE.md:176`)

Há outra manifestação: células A em 18 e 20/10 e B em 19/10 viram, para A, intervalo contínuo 18–20; as batidas de 19 são retiradas, mas o cursor de previsto visita 19. Isso permite apurar trabalho em B e falta em A no mesmo dia. **Certeza alta por análise estática; execução financeira [não verificado].** (`app/ponto/services/fechamento.py:236`; `app/ponto/services/fechamento.py:250`; `app/ponto/services/fechamento.py:720`; `app/escala/models.py:1094`; `app/ponto/motor_calculo_v2.py:2595`; `app/ponto/motor_calculo_v2.py:2610`)

**5. Transferir o posto não estabelece a vigência do novo posto.**

A rota altera posto/empresa do colaborador e registra histórico, mas não fecha nem cria o vínculo de escala. Os feriados continuam usando o posto do vínculo escolhido para cada dia. (`app/colaboradores/views.py:1384`; `app/colaboradores/views.py:1392`; `app/colaboradores/services/colaborador.py:73`; `app/core/feriados.py:121`; `app/core/feriados.py:151`)

**Contraexemplo:** vínculo aberto no posto A; transferência pela rota para B em 10/11; nenhuma operação posterior de escala; feriado municipal exclusivo de B em 11/11. O vínculo continua apontando A, e o feriado é consultado pelo município de A. **Certeza alta para esse estado; dono CADASTRO pela vigência que permanece declarada.** (`app/escala/alimentacao.py:285`; `app/core/feriados.py:151`)

**Especulação rotulada:** considerar isso defeito da integração da transferência depende de demonstrar que essa ação deveria estabelecer também a nova vigência; esse contrato completo **[não verificado]**.

**6. Janela viva e expiração de perguntas usam corte 21 implícito.**

As chamadas a `janela_atual` e `janela_anterior` omitem empresa tanto no juiz quanto no cálculo de expiração; a omissão aplica o padrão 21. (`app/chamados/juizes.py:561`; `app/chamados/juizes.py:580`; `app/chamados/veredito_lavrado.py:114`; `app/chamados/veredito_lavrado.py:115`; `app/ponto/janelas.py:50`)

**Contraexemplo:** hoje 05/11/2026, empresa corte 10, evento em 15/09. Pela regra “anterior + corrente + véspera”, o intervalo dessa empresa seria 09/09–09/11; o helper calcula 20/09–20/11 e devolve falso para o evento. **Certeza alta para a janela; ESTRUTURA.** (`app/chamados/juizes.py:564`; `app/chamados/juizes.py:586`; `app/ponto/janelas.py:66`)

**Especulação rotulada:** desaparecer ou permanecer uma pergunta específica depende dos demais ramos de `pergunta_viva`; efeito final **[não verificado]**. (`app/chamados/juizes.py:1123`; `app/chamados/juizes.py:1139`; `app/chamados/juizes.py:1149`)

**7. O diagnóstico C2 escolhe a competência pelo mês civil e crava corte 21.**

`_jornada_que_mente` chama `periodo_apuracao(hoje.month, hoje.year, 21)`. Essa leitura alimenta a lista exibida em “Cadastro x realidade”. (`app/escala/services/cadastro_realidade.py:199`; `app/escala/services/cadastro_realidade.py:571`; `app/escala/views.py:487`)

**Contraexemplo:** hoje 31/10/2026, corte 21, jornada declarada 480 minutos, oito dias fechados entre 21 e 30/10 com 600 minutos líquidos cada; nenhuma célula anterior. A competência corrente contém esses dias, mas C2 pesquisa 21/09–20/10 e não os inclui na média. **Certeza alta; ESTRUTURA, limitada ao diagnóstico.** (`app/ponto/janelas.py:27`; `app/escala/services/cadastro_realidade.py:207`; `app/escala/services/cadastro_realidade.py:220`; `app/escala/services/cadastro_realidade.py:225`)

**Especulação rotulada:** correção humana equivocada ou impacto posterior na folha **[não verificado]**.

**8. A tela oferece início 1, mas grava início 2.**

Formulário e ajuda anunciam faixa 1–28; o POST converte 1 para 2 e retorna sucesso. O juiz também limita a faixa a 2–28. (`app/templates/colaboradores/empresa_form.html:54`; `app/templates/colaboradores/empresa_form.html:55`; `app/colaboradores/views.py:499`; `app/colaboradores/views.py:524`; `app/ponto/janelas.py:51`)

**Contraexemplo:** editar empresa com demais campos válidos, escolher início 1 e salvar: grava 2. Um cadastro já contendo 1 também produz janela efetiva 02/09–01/10 para outubro/2026. **Certeza alta; ESTRUTURA na correspondência tela/gravação.** (`app/colaboradores/views.py:523`; `app/ponto/janelas.py:15`; `app/ponto/janelas.py:57`)

**Especulação rotulada:** qual deveria ser a nomenclatura da competência com início 1 **[não verificado]**; o achado demonstrado é a substituição silenciosa.

**Admissão, demissão e limites da verificação.**

O retentor recorta pela admissão e demissão; a grade preserva fatos anteriores à admissão e trata o dia da demissão conforme existência de fato. Portanto, esses leitores têm critérios distintos, mas não demonstrei erro com cadastro coerente. (`app/folha/export.py:1180`; `app/folha/export.py:1182`; `app/escala/services/leitor_celula.py:363`; `app/escala/services/leitor_celula.py:366`) Cobrança pré-admissão ou perda efetiva do último dia em produção: **[não verificado]**.

O vínculo inclui primeiro e último dia; a troca normal de escala encerra o anterior em D−1; o desligamento encerra vínculos já iniciados na data informada. Vínculo futuro posterior à demissão é recusado pelo encerramento e declarado problema de cadastro. (`app/escala/alimentacao.py:214`; `app/escala/alimentacao.py:282`; `app/colaboradores/services/vinculo.py:202`; `app/colaboradores/services/desligamento.py:123`; `app/colaboradores/services/vinculo.py:809`)

Para turno noturno 20→21, há busca ampliada e guarda dos dois limites pela data da jornada; não tratei o simples carregamento da véspera como dupla contagem. (`app/ponto/services/fechamento.py:617`; `app/ponto/services/fechamento.py:622`; `app/ponto/motor_calculo_v2.py:2429`; `app/ponto/motor_calculo_v2.py:2431`) Execução integral das suítes, incidência produtiva e identidade Git da cópia sem `.git`: **[não verificado]**.

| Achado | arquivo:linha | Contraexemplo | Certeza | Dono L-099 |
|---|---|---|---|---|
| CNPJ atual recebe fechamento inteiro | [app/folha/export.py:767](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/folha/export.py:767) | A→B em 05/10; dias anteriores seguem no fechamento selecionado em B | Alta | ESTRUTURA |
| Régua atual aplicada ao passado | [app/core/regua_cct.py:352](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/core/regua_cct.py:352) | A=CCT, B=CLT; recalcular fatia anterior à transferência | Alta | ESTRUTURA |
| Trava consulta competência de outro corte | [app/ponto/janelas.py:85](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/ponto/janelas.py:85) | Batida 15/09; A21→B10; setembro aprovado/outubro aberto | Alta | ESTRUTURA |
| Partição perde exclusividade | [app/ponto/services/fechamento.py:232](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/ponto/services/fechamento.py:232) | A termina e B começa em 20/10; sem células; mesmo par entregue duas vezes | Alta para entrega | ESTRUTURA |
| Posto atual muda; vínculo continua antigo | [app/colaboradores/views.py:1385](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/colaboradores/views.py:1385) | Transferir A→B sem estabelecer vínculo de B | Alta para estado | CADASTRO |
| Perguntas usam corte padrão | [app/chamados/juizes.py:561](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/chamados/juizes.py:561) | Corte 10, hoje 05/11, evento 15/09 excluído da janela | Alta | ESTRUTURA |
| C2 usa mês civil e 21 literal | [app/escala/services/cadastro_realidade.py:199](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/escala/services/cadastro_realidade.py:199) | Em 31/10, oito dias de 21–30/10 ficam fora da média | Alta | ESTRUTURA |
| Início 1 vira 2 | [app/colaboradores/views.py:499](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/colaboradores/views.py:499) | Escolher 1 na tela e salvar | Alta | ESTRUTURA |

## 05/11 15:57 · sessao ce4cccf2e72d · - · - · origem=Ronald (tmux)

Achados estáticos; contra-exemplos são construídos, **não casos observados em produção [não verificado]**. Todos têm dono **ESTRUTURA**: a repetição altera estado ou cria trilha/registro com o mesmo cadastro e payload; critério L-099 em `CLAUDE.md:180`.

| Achado | Arquivo:linha | Contra-exemplo construído | Certeza |
|---|---|---|---|
| Cancelamento DP devolve os dias novamente | `app/ferias/views.py:324`, `app/ferias/views.py:325`, `app/ferias/views.py:332`; `app/ferias/services.py:333` | PA com 30 concedidos e agendamento de 10 dias, não concluído: dois POSTs no mesmo ID deixam 20 e depois 10 concedidos. A view barra só `concluido`, portanto aceita `cancelado`. O gate `isento=True` passa direto (`app/colaboradores/mixins.py:301`). | Alta |
| Venda avulsa soma o mesmo pedido novamente | `app/ferias/views.py:913`, `app/ferias/views.py:918`, `app/ferias/views.py:926`; `app/ferias/services.py:512` | Direito=30, vendidos=0, concedidos=0, POST `dias=1`: vendidos 0→1→2; ambas as passagens satisfazem teto e saldo. | Alta |
| Exportação TXT sempre cria outro registro e outra trilha | `app/folha/views.py:289`, `app/folha/views.py:297`; `app/folha/models.py:178`, `app/folha/models.py:182` | Empresa/competência exportável, sem mudança nas linhas, repetir geração: dois `ExportacaoDominio` com o mesmo hash; segundo recebe `reexportacao=True`. `ja_existia` é aviso, não deduplicação. Segundo passa a vigente (`app/folha/models.py:134`). | Alta |
| Criar/copiar template de escala cria outra entidade | `app/escala/views_wizard.py:102`, `app/escala/views_wizard.py:108`; `app/escala/services/cadastro_tipo.py:174`, `app/escala/services/cadastro_tipo.py:306`, `app/escala/services/cadastro_tipo.py:369`; `app/escala/views.py:148` | Repetir um payload válido de criação sem `tid`, ou copiar o mesmo template duas vezes: duas PKs e dois códigos; o model gera código novo (`app/escala/models.py:763`). Nome não é unique (`app/escala/models.py:130`). Na cópia, o `reverse` de destino pode falhar depois do create; o `except` não desfaz a criação (`app/escala/views.py:149`). | Alta |
| Texto livre no fio duplica nota | `app/chamados/services/nota.py:95`, `app/chamados/services/nota.py:103`; chamadores abaixo | Mesmo chamado e texto `Recebido`, duas requisições: duas `FollowupChamado`, pois humanos livres chegam sem `texto_fixo`, `identico` ou `objeto_ref`. | Alta |
| Upload do mesmo anexo repete escrita e auditoria | `app/ponto/views.py:2152`; `app/ponto/services/ausencia.py:840`, `app/ponto/services/ausencia.py:843`, `app/ponto/services/ausencia.py:854` | Ausência já `aguardando_decisao`, repetir mesmo upload: serviço sempre atribui/salva o arquivo e emite `documento_anexado`; duas trilhas. Novo nome/arquivo físico depende do storage **[não verificado]**. | Alta para trilha |
| Edição idêntica de ausência repete trilha | `app/ponto/views.py:2784`, `app/ponto/views.py:2798`; `app/ponto/services/ausencia.py:508` | Ausência sem sobreposição, repetir os mesmos tipo/data/dias/minutos: dois logs `editar_ausencia`; nenhuma guarda compara os valores antes de emitir. | Alta |
| Validar lote pela tela registra novamente mesmo sem nova validação | `app/chamados/views_cobrar.py:381`, `app/chamados/views_cobrar.py:383` | Mesmas refs submetidas duas vezes: `validou_lote_tela` é emitido em ambas, ainda que a segunda valide zero. | Alta |
| Ping geográfico replica a mesma amostra e pode criar sinal a partir do retry | `app/ponto/views.py:2936`, `app/ponto/views.py:2976`, `app/ponto/views.py:2999`, `app/ponto/views.py:3002` | Turno aberto, fora do intervalo, localização classificada fora, `from_mock=False`, inicialmente sem pings fora; repetir payload com o mesmo `timestamp_dispositivo_ns`/assinatura: duas linhas com horários de servidor distintos. A segunda satisfaz o limiar de dois pings e chama abertura de `ping_geo_fora`. Modelo não impõe chave de amostra (`app/ponto/models.py:1011`, `app/ponto/models.py:1015`). | Alta |
| Enviar regularização repete os efeitos de comunicação | `app/chamados/views_regularizacao.py:160`, `app/chamados/views_regularizacao.py:170`, `app/chamados/views_regularizacao.py:177`; `app/chamados/services/regularizacao_ext.py:339` | Lote `aguardando` com item, mesmo POST enviar 2x: continua aguardando, mas chama push PWA/FCM 2x. Entrega externa efetiva **[não verificado]**. | Alta para chamadas |
| Recalcular fechamento renova carimbos mesmo sem mudança de insumo | `app/ponto/views.py:1253`; `app/ponto/services/fechamento.py:1127`, `app/ponto/services/fechamento.py:1145`, `app/ponto/services/fechamento.py:1173` | Mesma competência, cadastro e batidas: `derivado_em` muda nas duas gravações; no aprovado também `atualizado_em`. É falha do critério literal de estado inteiro; divergência monetária entre execuções **[não verificado]**. | Alta para carimbos |

Chamadores confirmados da classe “texto livre duplica nota”:

- Resposta colaborador: `app/chamados/views.py:1104` e log `app/chamados/views.py:1114`.
- Mensagem rápida DP: `app/chamados/views.py:986`.
- Detalhe local: `app/chamados/views.py:547` → `app/chamados/services/acoes_chamado.py:292`, log em `app/chamados/services/acoes_chamado.py:295`.
- Fio DP: `app/chamados/views.py:674` → `app/chamados/services/fio.py:714`.
- Modal DP: `app/chamados/views.py:954` → o mesmo escritor `app/chamados/services/fio.py:714`.

A trilha comum efetivamente faz INSERT por chamada: `app/core/auditoria.py:22` → `app/core/observ.py:134`; não há chave de deduplicação nessa porta.

Rotas confirmadas: férias vender/cancelar em `app/ferias/urls.py:17` e `app/ferias/urls.py:18`; wizard/cópia em `app/escala/urls.py:24` e `app/escala/urls.py:32`; TXT em `app/folha/urls.py:14`; mensagens/lote em `app/chamados/urls.py:14`, `app/chamados/urls.py:21`, `app/chamados/urls.py:37`, `app/chamados/urls.py:39`, `app/chamados/urls.py:40`, `app/chamados/urls.py:42`; anexo/edição/ping em `app/ponto/urls.py:71`, `app/ponto/urls.py:76`, `app/ponto/urls.py:78`.

Proteções conferidas, evitando falsos positivos:

- Cancelar férias **admin** barra o já cancelado via `pode_cancelar_admin`: `app/ferias/models.py:339`, consumido em `app/ferias/views.py:360`.
- Agendar férias bloqueia o mesmo início antes de incrementar saldo: `app/ferias/services.py:316`.
- Avaliação de solicitação filtra pendente: `app/ferias/views.py:718`.
- Resposta de confirmação de férias filtra `aguardando`: `app/ferias/views.py:879`.
- Abrir lote vazio e adicionar item regularização têm deduplicação: `app/chamados/services/regularizacao_ext.py:312`, `app/chamados/services/regularizacao_ext.py:325`.
- Atribuir chamado só grava nota/log quando `mudou`: `app/chamados/views.py:1024`.
- Plano de folgas recebe **tipo absoluto**, apesar da docstring antiga dizer “cicla”: `app/escala/views_plano_folgas.py:116`; foto manual compara tipo em `app/escala/folgas.py:286`.
- Nome de calendário repetido tem veto: `app/escala/views_wizard.py:173`, `app/escala/views.py:358`.
- Novo motivo de retratação e novo tipo de ausência têm guarda por nome/código: `app/ponto/views_motivos_retratacao.py:55`, `app/ponto/views_tipos_ausencia.py:92`.

Limites: nenhuma execução HTTP, banco, log, storage ou push foi feita **[não verificado]**. `adicionar_followup` chama GLPI a cada POST (`app/chamados/views.py:304`), mas deduplicação no serviço externo e resultado remoto **[não verificado]**. O inventário de funções com referência a POST foi percorrido nos arquivos solicitados; não completei prova individual de todos os ramos/serviços transitivos, portanto não certifico os restantes como idempotentes **[não verificado]**.

## 05/11 15:57 · sessao 3f402cdf6c30 · CODEX-SUFOCA-03-p13-escritores · leitura · origem=Code

O censo encontrou **escritas múltiplas autorizadas dentro de uma porta e escritas que escapam da porta declarada**. “Um escritor” significa, em vários contratos desta cópia, **um módulo com várias operações**, não uma função única. (`app/core/portas.py:98`; `app/ponto/portas/celula.py:3`; `app/ponto/portas/celula.py:66`.)

O P13 não prova exclusividade universal: procura nomes de campos por regex em seis apps, sem resolver a entidade; o gate por entidade também descarta escritas que não consegue atribuir a um modelo. (`app/core/tests/test_contract_escritores_estado.py:12`; `app/core/tests/test_contract_escritores_estado.py:177`; `app/ponto/tests/test_chokepoint_celula_gate.py:29`.)

Os contraexemplos abaixo são entradas construídas pela leitura do código; ocorrência em produção, reprodução executada e resultado atual dos testes são **[não verificado]**. O anexo fornecido não contém linhas de cadastro ou batidas para demonstrar ocorrência. **[não verificado]** A identidade Git do hash também ficou **[não verificado]**: não havia repositório Git consultável na cópia. [nao verificado]

**CelulaDia e escala.** A autoridade registrada para a célula é `ponto/portas/celula.py`; existem gate por entidade, catálogo dos campos e testes de idempotência por operação. (`app/core/portas.py:111`; `app/ponto/tests/test_chokepoint_celula_gate.py:49`; `app/escala/tests/test_d3_guarda_da_linha_celula.py:38`; `app/ponto/tests/test_porta_celula.py:36`.)

| Campo(s) | Escritores encontrados | Canônico e tripwire | Contraexemplo concreto; certeza |
|---|---|---|---|
| `CelulaDia.trabalha` — acompanhado de `dna/escala_geradora` | Regeneração: `app/ponto/portas/celula.py:231`, `app/ponto/portas/celula.py:260`; reversão: `app/ponto/portas/celula.py:538`; geração periódica: `app/ponto/portas/celula.py:601`, `app/ponto/portas/celula.py:620`. | Uma porta, três funções escritoras. Gate controla módulo/operação; `trabalha` está explicitamente dispensado de entrar na impressão dos insumos. (`app/ponto/tests/test_chokepoint_celula_gate.py:69`; `app/escala/tests/test_d3_guarda_da_linha_celula.py:70`.) | Célula gerada, `trabalha=True`, pertencente ao vínculo, em dia posterior ao seu fim, sem outra cobertura nem lavra: a regeneração escreve `False` e outro DNA, mas esse ramo salva somente `trabalha/dna`, sem atualizar os três campos de regeneração. **Alta**, por `app/ponto/portas/celula.py:225` e `app/ponto/portas/celula.py:233`. |
| `CelulaDia.origem` | Dois nascimentos explícitos com `gerada`: `app/ponto/portas/celula.py:247`, `app/ponto/portas/celula.py:601`. | Mesma porta. O catálogo declara `editada_em/editada_por` sem escritor de produção; não encontrei implementação da edição humana prometida no model. (`app/escala/tests/test_d3_guarda_da_linha_celula.py:44`; `app/escala/tests/test_d3_guarda_da_linha_celula.py:50`; `app/escala/models.py:1640`.) | Uma célula já marcada `editada` é pulada pelo gerador; produzir essa edição por uma porta da cópia permanece **[não verificado]**. **Alta** para o bloqueio, por `app/ponto/portas/celula.py:606`. |
| `CelulaDia.regenerada_em`, `regeneracoes`, `dna_anterior` | Regenerar: `app/ponto/portas/celula.py:261`; reverter: `app/ponto/portas/celula.py:539`. | Mesma porta. Há teste de repetição da regeneração, mas ele exercita o ramo normal, não o ramo fora da vigência acima. (`app/ponto/tests/test_porta_celula.py:46`.) | DNA anterior existente: reverter restaura o DNA, limpa `dna_anterior`, incrementa regenerações e move o carimbo; repetir retorna sem escrita. **Alta**, por `app/ponto/portas/celula.py:526` e `app/ponto/portas/celula.py:538`. |
| `CelulaDia.tem_entrada`, `tem_saida`, `insumos_em` | Carimbo individual: `app/ponto/portas/celula.py:811`; lote: `app/ponto/portas/celula.py:847`, persistido por `bulk_update` em `app/ponto/portas/celula.py:851`. | Mesma porta; testes de idempotência individual e lote. (`app/ponto/tests/test_porta_celula.py:79`; `app/ponto/tests/test_porta_celula.py:142`.) | Célula sem carimbo recebe entrada/saída pelo lote; repetição com os mesmos insumos é ignorada. **Alta**, por `app/ponto/portas/celula.py:843`. |
| `CelulaDia.ata` | Lavrar: `app/ponto/portas/celula.py:749`, `app/ponto/portas/celula.py:776`; carimbar precedência: `app/ponto/portas/celula.py:828`. | Dois donos **por chave**, na mesma porta; a lavra conserva `precedencia`. Há teste cruzando as duas operações. (`app/ponto/portas/celula.py:94`; `app/ponto/portas/celula.py:740`; `app/ponto/tests/test_porta_celula.py:105`.) | Ata já contém precedência de férias; chega nova ata do cartório sem essa chave. A chave é preservada, e a segunda lavra igual não move o carimbo. **Alta**, por `app/ponto/tests/test_porta_celula.py:117`. |
| `EscalaColaborador.ativa` | Reutilizar/criar: `app/colaboradores/services/vinculo.py:267`, `:283`; fechar: `:615`; reparar: `:691`; absorver: `:753`; encerrar: `:797`; trecho retroativo: `:915`; snapshot: `:955`; ajuste/desfazer: `:1011`, `:1021`, `:1027`. | Canônico: módulo `vinculo.py`, não função única; gate de escala por módulo. (`app/core/portas.py:469`; `app/escala/tests/test_chokepoint_escala_gate.py:55`.) | Vínculo iniciado em setembro não pode ser encerrado em agosto: fechamento recusa o posterior e fecha o anterior. **Alta**; cenário está escrito em `app/escala/tests/test_vigencia_um_escritor.py:72`. |
| `EscalaColaborador.trabalha_em_feriado`, `origem_ancora` | Feriado nasce em `app/colaboradores/services/vinculo.py:287`, é alterado em `:846` e copiado em `:916`, `:1013`, `:1028`. Origem da âncora é limpa/declarada em `:264`, `:273`, nasce em `:285` e é copiada em `:1012`, `:1027`. | Mesma porta registrada. Há contrato específico para atribuição da **data** da âncora; isso não equivale a um contrato exclusivo de todos esses campos. (`app/core/portas.py:469`; `app/escala/tests/test_contract_fase_declarada.py:26`.) | Vínculo comercial muda `trabalha_em_feriado=True→False`: o setter salva a flag e inicia regeneração. **Alta**, por `app/colaboradores/services/vinculo.py:843`. |
| `EscalaColaborador.regua_avaliada_em`, `regua_corroborada_em`, `regua_corroborada_por`, `regua_confianca`, `regua_dias_baixo` | Confirmar cadastro errado: `app/escala/regua_defesa.py:662`; desfazer: `:719`; avaliação/expiração: `:742`, `:765`. Persistência comum: `app/colaboradores/services/vinculo.py:991`. | Vários produtores semânticos, uma função de persistência: `gravar_regua`. As atribuições posteriores ao objeto são sincronização em memória. (`app/colaboradores/services/vinculo.py:969`; `app/escala/regua_defesa.py:668`.) | Confirmação administrativa coloca confiança em zero; diagnóstico posterior confiável limpa a confirmação. **Alta**, por `app/escala/regua_defesa.py:666` e `app/escala/regua_defesa.py:741`. |
| `TipoEscala.noturno`, `permite_hora_extra`, `is_template`, `editavel` | Cadastro atribui `noturno/permite_hora_extra` em `app/escala/services/cadastro_tipo.py:246`; criação atribui `is_template/editavel` em `:175`; cópia escreve `noturno/is_template/editavel` em `:378`, `:385`; derivação escreve `is_template=False` e admite ajustes por `setattr` em `:410`, `:415`. | Canônico: módulo `cadastro_tipo.py`; gate de escala. (`app/core/portas.py:473`; `app/escala/tests/test_chokepoint_escala_gate.py:55`.) | Derivar template compartilhado produz outro registro com `is_template=False`; copiar produz template `True`. **Alta**, por `app/escala/services/cadastro_tipo.py:385` e `app/escala/services/cadastro_tipo.py:410`. |
| `AgendaDia.status`, `atualizado_em` | Nascer rascunho: `app/escala/services/agenda.py:17`; propor: `:33`; responder: `:43`. `atualizado_em` também é automático no model. (`app/escala/models.py:1442`.) | Canônico: `agenda.py`, com gate de escala; resposta filtra estado anterior e chamado. (`app/core/portas.py:475`; `app/escala/services/agenda.py:44`.) | Proposta aceita deixa de casar com o filtro; repetir a resposta não altera novamente. **Alta**, por `app/escala/services/agenda.py:43`. |
| `DecisaoProposta.estado` | Recusar: `app/escala/services/propositor.py:717`; aceitar: `:773`. | Mesma porta, em **registros de decisão**, não duas transições concorrentes da mesma linha; módulo registrado em `app/core/portas.py:477`. | Mesma proposta pode gerar decisão recusada ou aceita conforme o ato; recusa exige motivo. **Alta**, por `app/escala/services/propositor.py:714` e `app/escala/services/propositor.py:775`. |

`veredito/veredito_via/veredito_em/protestos` e `status_dia/status_rotulo` não acrescentam módulos escritores à célula: as atribuições ficam na porta; o `lavrar_veredito` do model delega. `julgada_em/impressao` são escritos por `carimbar`. (`app/ponto/portas/celula.py:713`; `app/ponto/portas/celula.py:777`; `app/ponto/portas/celula.py:808`; `app/escala/models.py:1828`.)

**Chamados.** O anexo chama `validacao.py` de escritor único de `validada_em`, mas o P13 da cópia autoriza três arquivos para esse campo. A afirmação do anexo é, portanto, incompleta para o campo inteiro. (`app/core/tests/test_contract_escritores_estado.py:133`.)

| Campo(s) | Escritores encontrados | Canônico e tripwire | Contraexemplo concreto; certeza |
|---|---|---|---|
| `ChamadoColaborador.status_local` | Transição: `app/chamados/models.py:222`, salva em `:279`; nascimentos pelas portas `abrir/criar`, enumeradas em `app/chamados/tests/test_selo_status_so_pelas_portas.py:45`. | Canônico da **transição**: `_gravar_estado`; selo AST enumera portas e permite simulação em memória. (`app/chamados/tests/test_selo_status_so_pelas_portas.py:37`.) | Chamado com pergunta que continuaria viva no destino terminal: transição é recusada antes da atribuição. **Alta**, por `app/chamados/models.py:215`. |
| `ChamadoColaborador.urgencia`, `urgencia_original` | Restaurar na reabertura: `app/chamados/models.py:245`; rebaixar: `:454`; persistir rebaixamento: `:467`, `:602`; escalar urgência: `:519`, `:521`; nascimento: `:1045`. | Canônicos por ato dentro do model. O selo valida vocabulário e ritual, não uma única função escritora. (`app/chamados/tests/test_porta_chamado_ciclo.py:414`; `app/chamados/tests/test_porta_chamado_ciclo.py:536`.) | Urgente fecha e vira normal; reabertura restaura urgente e consome `urgencia_original`. **Alta**, por `app/chamados/models.py:244` e `app/chamados/models.py:454`. |
| `ChamadoColaborador.atribuido_em` | Assumir: `app/chamados/services/vinculos.py:130`; liberar: `:141`. | Mesma porta; testes de comportamento e idempotência. (`app/chamados/tests/test_porta_vinculos.py:205`; `app/chamados/tests/test_porta_vinculos.py:242`.) | Assumir por outro gestor troca instante; liberar limpa responsável e instante. **Alta**, por `app/chamados/services/vinculos.py:125`. |
| `DisputaSupervisao.fechada_em`, `fechada_por`, `motivo_fechamento` | Fechar: `app/chamados/models.py:1850`; reabrir admin: `:2052`; reabrir sistema: `:2076`. | Uma família de portas no model; selo enumera esses atos. (`app/chamados/tests/test_selo_status_so_pelas_portas.py:47`.) | Disputa fechada com motivo administrativo reabre pelo sistema: os três campos são limpos. **Alta**, por `app/chamados/models.py:2076`. |
| `DisputaSupervisao.resolucao` | Fechar: `app/chamados/models.py:1852`; reabrir admin: `:2054`; reabrir sistema: `:2078`; limpar residual: `:2162`. | Selo específico enumera quatro atos. (`app/chamados/tests/test_selo_resolucao_um_escritor.py:32`.) | Disputa aberta com resolução residual pode ser limpa; fechada preserva o texto. **Alta**, por `app/chamados/models.py:2159`. |
| `DisputaSupervisao.motivos` | Model: `app/chamados/models.py:1884`, `:1966`, `:2062`; emissão: `app/chamados/services/disputa_emissao.py:477`, `:922`, `:970`, `:998`, `:1089`, `:1190`; nascimentos em `:962`, `:1184`. | Dois módulos autorizados, com selo de atos. (`app/chamados/tests/test_porta_disputa_ciclo.py:81`; `app/chamados/tests/test_porta_disputa_ciclo.py:162`.) | Foto recebida apara motivo; fato inexistente remove motivo e suas perguntas. São operações diferentes sobre o mesmo estado. **Alta**, por `app/chamados/models.py:1884` e `app/chamados/models.py:1965`. |
| `DisputaSupervisao.contexto`, incluindo carimbos JSON | Model: `app/chamados/models.py:1885`, `:1967`, `:2007`, `:2025`; emissão: `app/chamados/services/disputa_emissao.py:273`, `:498`; nascimentos `:962`, `:1184`. | Mesmos dois módulos; `anotar` conserva primeiro valor e `renovar` aceita mudança. (`app/chamados/models.py:2004`; `app/chamados/models.py:2022`.) | Chave de alerta diário pode renovar; chave já preenchida passada a `anotar` permanece. **Alta**, pelos mesmos dois pontos. |
| `PerguntaDisputa.validada_em`, `validada_por` | Validar/devolver: `app/chamados/services/validacao.py:131`, `:188`; responder/corrigir: `app/chamados/services/disputa_emissao.py:1295`, `:1393`; auto-validar: `app/chamados/models.py:2581`; ressurreição/lote: `:1493`, `:2653`. | Canônico humano: validação; auto-validação e desvalidação têm outros donos explícitos. P13 permite três arquivos; selo de dono também usa allowlist por arquivo. (`app/core/tests/test_contract_escritores_estado.py:133`; `app/chamados/tests/test_contract_dono_validada.py:30`.) | Pergunta validada recebe resposta nova e perde validação; outra com texto pode ser auto-validada pelo carimbo se o C6 a julgar morta. **Alta**, por `app/chamados/services/disputa_emissao.py:1295` e `app/chamados/models.py:2576`. |
| `PerguntaDisputa.respondida_em` | Responder: `app/chamados/services/disputa_emissao.py:1283`; corrigir: `:1393`; resposta automática: `:1429`; recompor pela trilha: `:1445`; auto-carimbo: `app/chamados/models.py:2578`; ressurreição: `:1493`, `:2653`. | Dois módulos enumerados no P13. (`app/core/tests/test_contract_escritores_estado.py:163`.) | Correção limpa instante **e texto**; ressurreição do chamado zera os carimbos indicados, preservando o texto. **Alta** para a diferença de escrita; consequência na frota **[não verificado]**. (`app/chamados/services/disputa_emissao.py:1393`; `app/chamados/models.py:1493`.) |
| `PerguntaDisputa.materializada_em`, `materializada_por` | Instância: `app/chamados/models.py:2547`, persistida por `:2598`, `:2633`, `:2641`; lote direto: `:2653`. | Família canônica no model; selo AST por ato, incluindo lote. O homônimo de `RegularizacaoExterna` é outra entidade. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:44`.) | Pergunta ainda viva rejeita carimbo não-identidade e restaura a instância. **Alta**, por `app/chamados/models.py:2589`. |
| `PerguntaDisputa.via_resolucao` | Carimbo/lote: `app/chamados/models.py:2549`, `:2653`; fato no espelho: `:2662`; relavra: `app/chamados/services/relavra_via.py:123`. | Quatro atos autorizados pelo selo específico. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:51`.) | Pergunta carimbada por completude passa a declaração quando a resposta fornece esse fato; relavra grava via e cache juntos. **Alta**, por `app/chamados/services/relavra_via.py:97` e `app/chamados/services/relavra_via.py:123`. |
| `PerguntaDisputa.veredito_lavrado`, `julgado_em`, `janela_expira_em` | Lavra: `app/chamados/veredito_lavrado.py:227`; backfill atribui: `app/chamados/management/commands/backfill_veredito_lavrado.py:138`; persistência do lote já está na porta: `app/chamados/veredito_lavrado.py:207`; relavra salva valores produzidos pela porta em `app/chamados/services/relavra_via.py:124`. | Porta de persistência única, mas cálculo/atribuição também no backfill. P13 enumera ambos; selo exige zero ORM direto no command. (`app/core/tests/test_contract_escritores_estado.py:77`; `app/chamados/tests/test_porta_veredito_pergunta.py:203`.) | Veredito salvo igual, janela diferente: backfill pula a linha; lavra normal recalcula os três campos. **Alta** para a diferença; prejuízo **[não verificado]**. (`app/chamados/management/commands/backfill_veredito_lavrado.py:120`; `app/chamados/veredito_lavrado.py:227`.) |
| `PerguntaDisputa.recusa_motivo` | Escape: `app/chamados/services/disputa_emissao.py:1340`; materialização: `app/chamados/services/materializacao.py:817`, `:1069`; recusa humana: `app/chamados/services/validacao.py:545`; identidade: `app/chamados/models.py:2631`. | Não há função única para o campo. O selo administrativo declara não garantir essa unicidade. (`app/chamados/tests/test_porta_validacao_pergunta.py:30`.) | O mesmo campo recebe “folga contestada”, recusa humana ou motivo de duplicata; não identifica exclusivamente a última recusa do materializador. **Alta**, por `app/chamados/services/disputa_emissao.py:1340`, `app/chamados/services/validacao.py:545` e `app/chamados/services/fio.py:892`. |
| `RegularizacaoExterna.status`; `RegularizacaoItemBatida.aplicada_em` | Status: materializar/recusar/enviar/confirmar em `app/chamados/services/regularizacao_ext.py:197`, `:266`, `:342`, `:353`. Aplicação: dois ramos em `:138`, `:162`. | Mesma porta registrada; gate por entidade/módulo, sem impor uma máquina única de transição. (`app/core/portas.py:236`; `app/chamados/tests/test_chokepoint_regularizacao_gate.py:21`.) | Chamada direta de recusa sobre regularização materializada muda status sem limpar o carimbo; alcançabilidade desse estado pela tela **[não verificado]**. **Alta** para a escrita, **média** para risco, por `app/chamados/services/regularizacao_ext.py:264`. |

`resolvido_em/via_encerramento` do chamado têm os dois sentidos na **mesma função**, `_gravar_estado`; `reaberturas_colab` da disputa tem um incremento persistente, enquanto o homônimo anotado no chamado serve à apresentação. Esses casos não comprovam dois escritores independentes. (`app/chamados/models.py:238`; `app/chamados/models.py:242`; `app/chamados/models.py:2136`; `app/core/tests/test_contract_escritores_estado.py:117`.)

**Ponto e folha.** As portas registradas não eliminam, por si, caminhos alternativos dentro do módulo nem escritas cujo tipo o sensor perdeu. (`app/core/portas.py:485`; `app/core/portas.py:499`; `app/folha/tests/test_chokepoint_folha_gate.py:38`.)

| Campo(s) | Escritores encontrados | Canônico e tripwire | Contraexemplo concreto; certeza |
|---|---|---|---|
| `OrdemSubstituicao.status` | Nascer designada/concluída: `app/ponto/services/cobertura.py:100`, `:289`; iniciar fora da porta: `app/ponto/services/triagem_batida.py:733`, `app/api/views.py:722`, `app/api/views_core.py:719`. | Canônico registrado: `cobertura.py`. Gate de escala ignora entidade não resolvida; os retornos de `ativa_para` não são ligados à entidade por esses pontos. (`app/core/portas.py:478`; `app/escala/tests/test_chokepoint_escala_gate.py:35`; `app/ponto/services/triagem_batida.py:685`.) | Ordem designada cobrindo o instante da batida: os três caminhos gravam `em_curso` com `save`, fora da porta. **Alta** para a fonte; configuração usada em produção **[não verificado]**. |
| `Justificativa.status`, `aprovada_em`, `aprovada_por` | Decisões individuais: `app/ponto/services/justificativa.py:190`, `:268`, `:368`; lotes: `app/ponto/views.py:1797`, `:1876`. `status` também nasce/é definido em `app/ponto/services/regularizacao.py:105`, `app/ponto/services/triagem_batida.py:607`, `app/ponto/services/entrada_sincronizada.py:40`, `app/chamados/reconciliador.py:532`. | Canônico funcional: service de justificativa; a própria view registra o lote como dívida fora da porta. (`app/ponto/views.py:1801`.) Exclusividade efetiva desses campos **[não verificado]**. | **Inferência de concorrência:** rejeição em lote lê pendente sem lock; aprovação individual pode concluir antes do `save` da rejeição. O lote então sobrescreve a decisão. **Média** para a corrida, **alta** para a ausência de lock naquele trecho. (`app/ponto/views.py:1875`; `app/ponto/services/justificativa.py:262`.) |
| `Ausencia.status`, `aprovada_em` | Status: criação em `app/ponto/services/ausencia.py:333`, transição em `:808`, rejeição de férias por `update` em `:1267`. Carimbo: criação `:349`, decisão explícita `:901`, derivação no model `app/ponto/models.py:870`. | Canônico registrado: `ausencia.py`; `mudar_estado` se declara único, mas o lote escreve diretamente. Gate controla módulo, n

[truncado em 20000 de 35761 caracteres]

## 05/11 15:57 · sessao 5d5234ba0c46 · CODEX-SUFOCA-04-relogio · leitura · origem=Code

A cópia configura `America/Sao_Paulo` e `USE_TZ=True`; portanto, **dia civil local, data UTC e dia do turno precisam ser distinguidos**. Os contra-exemplos abaixo são construídos a partir do código; ocorrência em produção é **[não verificado]**. (`app/config/settings/base.py:93`, `app/config/settings/base.py:95`)

**1. O pareador consulta os marcos do dia seguinte durante o turno noturno — certeza alta.**

`_ivm_de()` e `_bm_de()` escolhem o DNA por `localdate(b.timestamp)`, sem conservar o dia do turno em andamento. Esses marcos decidem se a batida será absorvida como intervalo. (`app/ponto/turnos.py:1229`, `app/ponto/turnos.py:1237`, `app/ponto/turnos.py:1354`)

**Contra-exemplo reproduzido em memória:** célula de **04/11/2026** com 22–06, pausa 02–03; célula de **05/11** com 22–07, pausa 05–06. Batidas: **04/11 22h E; 05/11 02h S, 03h E, 06h S**. O pareador produz `22→02` fechado e `03→None` aberto; absorve a saída das 06h usando a pausa de 05/11. Mantendo os marcos de 04/11 também em 05/11, produz um turno fechado `22→06`. Overrides distintos por dia são suportados pelo cadastro. (`app/ponto/turnos.py:1362`, `app/ponto/turnos.py:1369`, `app/escala/models.py:326`)

**Limite:** papel de ata pode mitigar o caso, e a borda posicional pode recuperar o realizado; perda financeira não foi demonstrada. (`app/ponto/turnos.py:1314`, `app/ponto/turnos.py:723`) **Dono: ESTRUTURA.**

**2. A apuração por fatia filtra batidas pela data UTC — certeza alta no descarte.**

O filtro usa `b.timestamp.date()` tanto na janela ampliada quanto na seleção dos dias pertencentes à escala. Depois entrega esse conjunto ao motor. (`app/ponto/services/fechamento.py:245`, `app/ponto/services/fechamento.py:252`, `app/ponto/services/fechamento.py:277`)

**Contra-exemplo:** duas escalas A/B vigentes entre **01 e 03/11**; células de 01 e 03 pertencem a A, célula de 02 pertence a B. A prevê 22–06. Batidas do turno de 01: **01/11 22h−03 = 02/11 01hZ** e **02/11 06h−03 = 02/11 09hZ**. Para A, `_ok={01,03}`: ambas recebem data UTC 02 e são descartadas. Converter apenas para data civil preservaria a entrada, mas ainda excluiria a saída; a seleção precisa respeitar o turno. (`app/ponto/services/fechamento.py:207`, `app/ponto/services/fechamento.py:250`)

O efeito demonstrável é o motor receber batidas incompletas; valor financeiro resultante é **[não verificado]**. **Dono: ESTRUTURA.**

**3. Entrada depois da meia-noite empurra a saída esperada mais um dia — certeza alta.**

As duas versões de `data_saida_esperada()` ancoram o cálculo na data civil da entrada. (`app/escala/servico_jornada.py:203`, `app/escala/utils.py:963`)

**Contra-exemplo reproduzido em memória:** escala diária **22–06**, entrada tardia em **05/11 00h30**, saída ausente. O juiz atribui essa entrada ao turno de **04/11**; a saída esperada é **05/11 06h**. A função de jornada devolve **06/11**, pois monta outro turno iniciado em 05/11 às 22h. (`app/ponto/turnos.py:299`, `app/escala/servico_jornada.py:208`)

A data alimenta `resolver_data_evento`; a versão de `utils` alimenta o contexto de `orfao_14h`. (`app/escala/servico_jornada.py:491`, `app/chamados/detector_anomalias.py:45`) **Dono do deslocamento: ESTRUTURA; a ausência da saída, separadamente, é BATIDA.**

**4. O chamado de desvio leve recebe o dia civil da saída — certeza alta.**

O detector encontra `data_turno_ref`, mas o emissor da API escreve `data_turno=localdate(batida.timestamp)`. O mesmo ocorre no arquivo gêmeo. (`app/chamados/detector_anomalias.py:161`, `app/api/views.py:745`, `app/api/views_core.py:745`)

**Contra-exemplo:** escala fixa não comercial **22–06**, dia 04/11 de trabalho, **E04/11 22h; S05/11 06h20**. Os 20 minutos passam pelo ramo `leve`; o chamado nasce com **data_turno=05/11**, embora a saída pertença ao turno de **04/11**. (`app/chamados/detector_anomalias.py:225`, `app/chamados/detector_anomalias.py:243`, `app/api/views.py:746`)

Essa chave determina o dia do módulo e a procura da célula: existindo célula de 05/11, ela pode receber o chamado da jornada anterior. (`app/chamados/catalogo/modulos.py:168`, `app/chamados/models.py:1184`, `app/chamados/models.py:1190`) **Dono: ESTRUTURA.**

**5. O detector escolhe o vínculo antes de resolver o dia do turno — certeza alta no mecanismo.**

A escala é escolhida pela data civil da batida; depois o detector descobre o dia do turno, mas mantém a escala anterior. (`app/chamados/detector_anomalias.py:138`, `app/chamados/detector_anomalias.py:144`, `app/chamados/detector_anomalias.py:164`)

**Contra-exemplo reproduzido com dependências em memória:** A noturna **22–06 até 04/11**; B comercial **08–17 desde 05/11**; célula de 04/11 trabalhando, gerada por A. Batidas **E04/11 22h; S05/11 06h45**, turno atribuído a 04/11. O detector seleciona B pelo dia 05 e retorna `ok` pelo regime comercial, antes de avaliar os 45 minutos contra A. (`app/chamados/detector_anomalias.py:155`, `app/chamados/detector_anomalias.py:186`, `app/chamados/detector_anomalias.py:237`)

A integração completa com ORM/pareador é **[não verificado]**. **Dono: ESTRUTURA.**

**6. O resumo por competência exclui a continuação depois da meia-noite — certeza alta na soma; média no resultado final da tela.**

`somar_periodos()` recorta pela data civil de `p.entrada`, enquanto o motor mantém um mapa próprio do dia da jornada. (`app/ponto/services/espelho.py:84`, `app/ponto/motor_calculo_v2.py:2460`)

**Contra-exemplo reproduzido em memória:** competência terminando em **20/11**, turno **20/11 22h → 21/11 06h**, pausa 00–01. Dois períodos de trabalho: **22–00 = 2h** e **01–06 = 5h**, ambos da jornada de 20/11. A soma sem recorte dá **7h**; com recorte até 20/11 dá **2h**, excluindo a continuação. O espelho chama essa soma com a competência. (`app/ponto/services/espelho.py:85`, `app/ponto/services/espelho.py:98`, `app/ponto/services/espelho.py:439`)

**Condição de visibilidade:** totais posteriores da folha/lavratura podem sobrescrever o resultado; o exemplo persiste quando esses totais não existem. (`app/relatorios/cartao_pela_celula.py:203`, `app/relatorios/cartao_pela_celula.py:340`) **Dono: ESTRUTURA.**

**7. O propositor divide cada noite em dois dias — certeza alta; limitação declarada.**

`medir_padrao()` agrupa por `lt.date()` e calcula entradas, saídas e alternância desses grupos civis. O arquivo declara sua limitação para cruzamento da meia-noite. (`app/escala/detector_proposta.py:18`, `app/escala/detector_proposta.py:58`, `app/escala/detector_proposta.py:67`)

**Contra-exemplo reproduzido em memória:** 15 turnos alternados, **E22h nos dias 01,03,…,29/11; S06h no dia seguinte**. A função retorna **30 dias**, uma batida por dia, **entrada=22h, saída=22h, duração=None, padrão=semanal**. Um catálogo contendo somente o template 22–06 deixa de casar a saída calculada. (`app/escala/detector_proposta.py:74`, `app/escala/detector_proposta.py:83`, `app/escala/detector_proposta.py:100`) **Dono: ESTRUTURA.**

As reproduções foram parciais, em memória, com funções reais e dependências adaptadas; execução integrada com Django/ORM é **[não verificado]**. Ocorrência por colaborador, quantidade afetada, comportamento do processo de produção e identidade Git do hash informado são **[não verificado]**. Não classifico `date.today()` isoladamente como defeito de fuso: o fuso efetivo do processo é **[não verificado]**.

**Especulação rotulada:** extrapolar esses exemplos para frequência ou prejuízo financeiro em produção é **[não verificado]**. Os donos abaixo classificam o mecanismo, conforme a separação estrutural/dado da L-099. (`CLAUDE.md:176`)

| achado | arquivo:linha | contra-exemplo | certeza | dono L-099 |
|---|---|---|---|---|
| Marcos do dia seguinte no pareador | `app/ponto/turnos.py:1229` | Turno04 22–06; pausa de05 absorve S06 | Alta no pareamento | ESTRUTURA |
| Data UTC no filtro de fatias | `app/ponto/services/fechamento.py:252` | A nos dias01/03; E01 22h e S02 06h descartadas como dia02 | Alta no descarte | ESTRUTURA |
| Saída esperada avançada um dia | `app/escala/servico_jornada.py:203`; `app/escala/utils.py:963` | E05 00h30 do turno04 retorna saída06, em vez de05 | Alta | ESTRUTURA |
| Chamado no dia civil da saída | `app/api/views.py:745`; `app/api/views_core.py:745` | E04 22h/S05 06h20 gera chamado05 | Alta | ESTRUTURA |
| Vínculo escolhido pelo dia civil | `app/chamados/detector_anomalias.py:144` | A22–06 → Bcomercial; S05 06h45 retorna ok | Alta no mecanismo | ESTRUTURA |
| Continuação excluída do resumo | `app/ponto/services/espelho.py:84` | Jornada20 com 7h soma2h no recorte até20 | Alta na soma; média na tela | ESTRUTURA |
| Noite partida no propositor | `app/escala/detector_proposta.py:58` | 15 jornadas22–06 viram30 dias e saída22h | Alta | ESTRUTURA |

## 05/11 15:56 · sessao e8c3d0176b7b · - · - · origem=Ronald (tmux)

Consolidado de commands: falhas de negócio/trilha confirmadas por leitura; ocorrência em produção **[não verificado]**. Todos os donos abaixo são **ESTRUTURA** segundo a separação de `CLAUDE.md:176`.

| Achado | Referências | Contra-exemplo construído | Certeza |
|---|---|---|---|
| **`aplicar_09_he_intra --aplicar` soma novamente o mesmo delta financeiro.** O delta nasce de dois cálculos sobre os mesmos fatos; a base gravada é relida a cada execução, e recebe `atual + delta`. Não há marcador de aplicação. | `app/ponto/management/commands/aplicar_09_he_intra.py:105`, `:115`, `:125`, `:131`, **`:146`**, `:149`; persistência em `app/ponto/services/fechamento.py:1375` | Fechamento com HE50=2h e delta isolado da cura=-1h: 1ª execução grava 1h; 2ª grava 0h. Cadastro que produz delta: intervalo declarado/descontado e suprimido indenizado; o helper curado acrescenta até o intervalo descontado ao limite (`app/ponto/motor_calculo_v2.py:1785`, `:1803`), enquanto o substituto sem cura devolve só o limite (`aplicar_09_he_intra.py:78`). O valor numérico é exemplo construído, não dado de prod. | **Alta**, mecanismo algébrico |
| **`regerar_senhas_iniciais --apply` repete reset e trilha**. A pessoa continua no filtro `deve_trocar_senha=True`; a porta não verifica se já resetou. | `app/colaboradores/management/commands/regerar_senhas_iniciais.py:25`, `:70`; `app/core/credenciais.py:54`, `:57`, **`:58`** | Uma pessoa com usuário, CPF de 11 dígitos e troca obrigatória=True; executar 2x sem login intermediário chama reset 2x e registra duas linhas `reset_senha_adm`. Não é necessário expor CPF. | **Alta** para trilha; diferença do hash salgado depende da biblioteca instalada **[não verificado]** |
| **`aplicar_janela_he_total --aplicar` duplica auditoria mesmo quando cadastro já igual.** | `app/ponto/management/commands/aplicar_janela_he_total.py:119`, **`:129`**, `:132`, `:133`; escritor `app/core/observ.py:134` | Empresa já com ativa/piso/desde/saída iguais aos argumentos; primeira e segunda passagem produzem evento, inclusive `antes == depois`. | **Alta** |
| **`marcar_feriado_do_posto --apply` duplica auditoria da mesma declaração absoluta.** | `app/colaboradores/management/commands/marcar_feriado_do_posto.py:128`, `:131`, **`:132`** | Posto com município e `opera_em_feriado=False`; repetir `--fecha --apply`: False→False também registra evento. Chamados podem nem existir. | **Alta** |
| **`corrigir_escala_retroativa --apply` sem `--novo-trecho` duplica trilha mesmo template/âncora/fim.** | `app/colaboradores/management/commands/corrigir_escala_retroativa.py:206`, `:212`, **`:213`**; porta `app/colaboradores/services/vinculo.py:919`, `:925` | Vínculo cujo template/âncora/fim já são os argumentos, âncora não futura: duas execuções registram duas linhas de edição. Há ainda regeneração subsequente (`corrigir_escala_retroativa.py:230`), cujo efeito completo não foi certificado. | **Alta** para trilha |
| **`backfill_veredito_lavrado --apply` poupa perguntas iguais, mas registra resumo incondicional.** | `app/chamados/management/commands/backfill_veredito_lavrado.py:120`, `:123`, **`:157`**, `:158` | Uma pergunta cuja lavra já corresponde ao juiz; as duas passagens têm `n_div=0`, mas as duas registram `backfill_veredito_lavrado`. Até conjunto vazio alcança esse evento. | **Alta** |
| **`lembrar_holerites --apply` registra uma trilha a cada passagem, inclusive sem alvo; com `--dias 0`, pode repetir push e incremento.** | `app/holerite/management/commands/lembrar_holerites.py:69`, `:77`, `:82`, `:94`, **`:99`**, **`:106`** | Padrão: holerite antigo, não assinado, token presente, push bem-sucedido. 1ª envia e carimba; 2ª imediata não envia, mas registra outra linha. Variante autorizada pela CLI `--dias 0 --teto 3`: 1ª e 2ª incluem novamente o documento, e `lembretes_n` passa 0→1→2 se `_push` retorna ok. | **Alta** para fluxo condicional; sucesso real do serviço **[não verificado]** |

O mecanismo comum da duplicação de auditoria está explícito: `registrar_log` encaminha a `evento` (`app/core/auditoria.py:22`) e este executa `LogAuditoria.objects.create` sem chave de repetição (`app/core/observ.py:133`). Não deduzir daí que toda chamada de `evento` é defeituosa: os seis caminhos acima foram conferidos quanto à ausência da guarda no chamador.

**Grupo separado: só carimbo, sem evidência de duplicata de negócio.** Sob leitura literal de “mesmo estado”, os seguintes escrevem relógio novo a cada repetição:

- `lavrar_retrato_folha --apply`: `app/folha/management/commands/lavrar_retrato_folha.py:74` e `:76` (`lavrado_em=now`).
- `lavrar_badge_navbar --apply`: `app/core/management/commands/lavrar_badge_navbar.py:91` e `:93`.
- `lavrar_placar_situacional --apply`: `app/core/management/commands/lavrar_placar_situacional.py:105` e `:106`.
- `recalcular_fechamento --apply`: delega em `app/ponto/management/commands/recalcular_fechamento.py:118`; aberto recebe `derivado_em=now` e `save` em `app/ponto/services/fechamento.py:1121`; demais ramos persistidos recebem `atualizado_em=now` em `:1144`. Não concluir mudança financeira só desses carimbos.
- Snapshots com `update_or_create`: `MetricaSnapshot.calculado_em=auto_now` em `app/inteligencia/models.py:96`. Chamadores observados: `escala/management/commands/lavrar_vigencia_impossivel.py:39`, `lavrar_vigencia_sem_trilha.py:33`, `lavrar_fase_conflitante.py:35`, `lavrar_previsto_cego.py:37`, `lavrar_foto_folgas.py:180`; `inteligencia/management/commands/lavrar_jornada_lixo.py:61`, `lavrar_fabrica.py:49` e `:58`; `ponto/management/commands/lavrar_deslogados.py:69`, `lavrar_abriu_nao_bateu.py:47`; `chamados/management/commands/lavrar_furo_sem_canal.py:48`. Afirmar somente alteração de carimbo; duplicata sequencial não foi demonstrada.

**Não classificar como falha do critério pedido:** `corrigir_escala_retroativa --novo-trecho --apply` chama CREATE sem recuperação (`app/colaboradores/services/vinculo.py:912`), mas `(colaborador,data_inicio)` é único (`app/escala/models.py:1004`, migração `app/escala/migrations/0004_alter_escalacolaborador_unique_together.py:16`). A segunda execução tende a erro se essa migração estiver aplicada **[não verificado]**, preservando estado/zero duplicata; isso sozinho não prova violação do critério estado/trilha.

**Proteções lidas que impedem alegações vagas:** `regerar_txt_dominio` interrompe quando TXT é idêntico (`app/folha/management/commands/regerar_txt_dominio.py:113`); `apurar_furo_da_exportada` retorna quando não há colunas NULL (`app/ponto/management/commands/apurar_furo_da_exportada.py:105`); `rejulgar_status_por_tipo` pula status igual (`app/ponto/management/commands/rejulgar_status_por_tipo.py:25`); `curar_residuo_bug115` evita repetir a mutação ambígua quando candidatos iguais (`app/ponto/management/commands/curar_residuo_bug115.py:118`). Isso certifica essas guardas específicas, não toda a cadeia.

**Cobertura e limites:** busca textual encontrou 101 arquivos com declaração de `--apply`/`--aplicar` na mesma linha **[não verificado por referência estática única; resultado de rg]**; flags declaradas em múltiplas linhas também existem (`app/ponto/management/commands/escalonar_documentos_ausencia.py:20`). Houve triagem do inventário e aprofundamento nos mecanismos relatados; **não foi certificado o comportamento completo de cada command**. Ausências, emissão de chamados/perguntas, regularizações, propostas e sincronizações que delegam a serviços ficaram sem prova completa 2x **[não verificado]**. Não executei comandos da aplicação, testes, banco ou logs; os contra-exemplos são construídos a partir do código, e a existência das respectivas entradas em produção permanece **[não verificado]**.

## 05/11 15:55 · sessao f268d24a7305 · - · - · origem=Ronald (tmux)

1. **Sem células, vigências sobrepostas recebem as mesmas batidas — certeza alta, ESTRUTURA.** `apurar_por_fatia` só restringe a exclusividade quando `_julgados` contém dias com geradora; vazio, executa cada intervalo integral e concatena os resultados. (`app/ponto/services/fechamento.py:189`, `:203`, `:222`, `:232`, `:277`, `:285`)

   Contraexemplo sintético: competência 21/09–20/10/2026; A inativa com vigência 21/09–20/10; B ativa desde 20/10; nenhuma célula; batidas em 20/10 às 08h E e 17h S locais. O carregador aceita A e B, enquanto o juiz escolhe somente B. (`app/escala/alimentacao.py:148`, `:214`, `:285`)

   Reproduzi em memória com AST das funções reais e motor instrumentado: juiz devolveu B, mas os dois motores receberam os mesmos IDs de batida; a composição preservou as duas entregas. **Limite:** não executei o motor financeiro real; total monetário [não verificado]. A sobreposição inicial é CADASTRO; a divergência entre o juiz único e a partição é ESTRUTURA. (`app/escala/alimentacao.py:286`; `app/ponto/services/fechamento.py:277`, `:285`)

2. **Células alternadas permitem trabalho e falta no mesmo dia — certeza alta por análise estática, ESTRUTURA.** Contraexemplo sintético: A e B cobrem 18–20/10; células de A em 18 e 20, de B em 19; todas `trabalha=True`; batidas completas 08h–17h nos três dias. Para A, o código transforma `[18,20]` em intervalo contínuo 18–20, mas remove suas batidas de 19. (`app/ponto/services/fechamento.py:207`, `:236`, `:250`)

   O montador prevê todos os dias do intervalo; `eh_dia_trabalho` lê `cel.trabalha` sem conferir a geradora. O motor recebe 19 como previsto sem trabalho e acrescenta falta, enquanto B pode apurar o trabalho de 19. (`app/ponto/services/fechamento.py:720`; `app/escala/models.py:1089`; `app/ponto/motor_calculo_v2.py:2595`, `:2610`) Execução integral dessa variante e valor financeiro [não verificado].

3. **Transferir posto não transfere o vínculo que governa os próximos dias — certeza alta do caminho, ESTRUTURA.** `transferir_posto` altera imediatamente `Colaborador.posto` e `Colaborador.empresa`, registrando histórico; não fecha nem cria `EscalaColaborador`. A porta chamada apenas salva o colaborador. (`app/colaboradores/views.py:1384`, `:1392`; `app/colaboradores/services/colaborador.py:73`)

   Contraexemplo sintético: vínculo aberto em posto A, município A; transferência pela tela para posto B, município B, em 10/11; feriado municipal de B em 11/11. Sem outra operação, o juiz continua devolvendo o vínculo de A e o feriado continua resolvido pelo município de A, pois o lugar vem de `EscalaColaborador.posto`. (`app/escala/alimentacao.py:269`, `:285`; `app/core/feriados.py:114`, `:151`) Incidência real [não verificado].

**Bordas que estão explícitas e inclusivas:** início e fim de vínculo usam `<=`/`>=`; a geradora também só é rejeitada estritamente antes/depois da vigência. Troca normal de escala fecha a anterior em D−1. (`app/escala/alimentacao.py:214`, `:282`; `app/colaboradores/services/vinculo.py:202`)

**Limite adicional:** desligamento fecha os vínculos ativos que já começaram; um vínculo futuro posterior à demissão é recusado pelo fechamento e permanece para correção de cadastro. Não classificar automaticamente como falha estrutural: essa exceção é declarada pela própria porta. (`app/colaboradores/services/vinculo.py:809`, `:816`; `:596`)

## 05/11 15:55 · sessao 4683680741e7 · - · - · origem=Ronald (tmux)

1. **Marcos da madrugada vêm do dia civil seguinte ao turno — certeza alta; dono ESTRUTURA.** `_ivm_de()` e `_bm_de()` indexam o DNA com `timezone.localdate(b.timestamp)`, inclusive quando a batida pertence ao turno iniciado na véspera. A absorção posterior pode transformar a saída verdadeira em intervalo. Referências: `app/ponto/turnos.py:1227`, `app/ponto/turnos.py:1235`, `app/ponto/turnos.py:1354`, `app/ponto/turnos.py:1369`.

   **Contra-exemplo sintético reproduzido:** cadastro de 04/11: 22h–06h, pausa 02h–03h; cadastro de 05/11: 22h–07h, pausa 05h–06h. Batidas: `04/11 22h E; 05/11 02h S; 03h E; 06h S`. O pareador devolveu um turno fechado `22h→02h` e outro aberto `03h→None`, ambos ancorados em 04/11; a saída das 06h foi absorvida. Controle com marcos iguais nos dois dias devolveu um único turno fechado `22h→06h`. O uso do mapa e a decisão de absorção estão em `app/ponto/turnos.py:1229`, `app/ponto/turnos.py:1237`, `app/ponto/turnos.py:1362`.

   A mudança entre dias é suportada pelo cadastro: overrides por weekday em `app/escala/models.py:326`, validação em `app/escala/models.py:667`, leitura do DNA congelado em `app/escala/models.py:1124`. O mapa chega pelo provedor de `app/ponto/turnos.py:1907` e pela chamada de `app/ponto/turnos.py:1990`; o motor também o fornece, sem papel da ata, em `app/ponto/motor_calculo_v2.py:867`.

   **Limite:** papel `Xi` de ata completa pode proteger esse caso (`app/ponto/turnos.py:1314`). A borda posicional pode recuperar o realizado (`app/ponto/turnos.py:723`); portanto, efeito monetário permanece **[não verificado]**.

2. **Entrada tardia depois da meia-noite desloca a data da saída esperada em um dia — certeza alta; dono ESTRUTURA.** As duas versões de `data_saida_esperada()` ancoram o turno na data civil local da entrada: `app/escala/servico_jornada.py:203`, `app/escala/utils.py:963`.

   **Contra-exemplo sintético reproduzido:** escala diária 22h–06h, entrada tardia `05/11/2026 00h30 E`, saída ausente. O juiz atribui a entrada ao turno de **04/11**, conforme `app/ponto/turnos.py:299`; a saída desse turno seria **05/11 às 06h**. Entretanto, `servico_jornada.data_saida_esperada()` retorna **06/11**, porque combina 05/11 com 22h e encadeia o fim para o dia seguinte (`app/escala/servico_jornada.py:80`, `app/escala/servico_jornada.py:208`). A versão `utils` contém a mesma ancoragem (`app/escala/utils.py:963`, `app/escala/utils.py:968`).

   Há consumidores vivos: contexto de anomalia em `app/chamados/detector_anomalias.py:42` e resolução da data de evento em `app/escala/servico_jornada.py:483`. Na emissão, a data resolvida participa da pergunta e da consulta de ata (`app/chamados/services/disputa_emissao.py:1041`). A ocorrência real e a passagem pelas demais guardas permanecem **[não verificado]**.

As reproduções executaram os corpos reais em memória, com adaptadores de timezone da biblioteca padrão e constantes lidas da cópia; Django não está instalado no ambiente. Execução com Django/ORM, ocorrência em produção e impacto por colaborador permanecem **[não verificado]**. Nenhum arquivo foi escrito. **[não verificado por artefato persistente]**

## 05/11 15:55 · sessao b0f0b0cde35e · - · - · origem=Ronald (tmux)

Dois achados demonstráveis; ambos ESTRUTURA, sem evidência de ocorrência em produção [não verificado].

1. **Detector escolhe o vínculo pelo dia civil antes de descobrir o dia do turno.** `app/chamados/detector_anomalias.py:138` extrai a data civil, `:144` escolhe o vínculo; somente `:155-162` identifica `data_turno`, mas conserva o vínculo anterior. O filtro `:186` pode então encerrar pelo regime errado.

   **Contra-exemplo concreto:** vínculo A, `tipo_base='12x36'`, turno 22–06, termina em 04/11/2026; vínculo B, `tipo_base='comercial'`, começa em 05/11, 08–17. Célula de 04/11 gerada por A, trabalha=True, DNA 22–06; E em 04/11 22:00 e S em 05/11 06:45. A autoridade devolve turno de 04/11, mas o detector consulta B em 05/11 e retorna `('ok', {})` em `:186`, ignorando a saída 45 minutos após o marco de A. A régua noturna encadeada coloca o fim em D+1 (`app/escala/servico_jornada.py:61-85`); desvio superior a 30 minutos deveria alcançar o ramo grave (`app/chamados/detector_anomalias.py:225-238`).

   **Certeza alta no mecanismo:** executei a função original extraída por AST, `python3 -B`, dependências e resposta da autoridade simuladas em memória. Resultado `('ok', {})`; rastro consultou vínculo B em 05/11 e `eh_dia_trabalho` de B em 04/11. Integração com ORM/pareador real [não verificado]. **Especulação rotulada:** frequência e colaboradores afetados [não verificado]. [nao verificado]

   Variante: A termina em 04/11 sem sucessor; S05/11 06:45 gera consulta a 05/11 e retorno `ok` antes sequer de ler o turno (`:144-147`). Vigência termina inclusivamente no dia cadastrado (`app/escala/alimentacao.py:214-216`).

2. **Propositor de escala divide cada noite em dois dias civis.** `app/escala/detector_proposta.py:55-58` agrupa por `lt.date()`; `:66-88` mede quantidade de dias, entrada, saída e duração usando os grupos. O próprio arquivo declara a limitação em `:18-25`.

   **Contra-exemplo concreto:** 15 turnos alternados, E22:00 nos dias 01,03,…,29/11/2026 e S06:00 no dia seguinte, janela 01–30/11, sem outras batidas. A execução da função original por AST retornou `dias=30`, `marcos_moda=1`, `entrada='22:00'`, `saida='22:00'`, `duracao_h=None`, `dispersao_entrada_min=480`, `padrao_dias='semanal'` (`:61-91`). O padrão real contém 15 jornadas de oito horas em dias alternados. Um catálogo contendo somente o template 22–06 não casa a saída calculada 22:00 com 06:00 pelo raio de 30 minutos (`:100-112`), devolvendo proposta insuficiente (`:126-132`). Consumidores incluem PDF (`app/escala/proposta_pdf.py:45-46`) e propostas de troca de tipo (`app/escala/services/escala_auto.py:117-125`).

   **Certeza alta:** reprodução da função real, com carregador de batidas simulado em memória; nenhuma escrita. O dado fornecido não contém batidas nem cadastro que demonstrem ocorrência em produção [não verificado].

Descartado como falso positivo UTC: `app/folha/porta_export.py:538` usa `.entrada.date()`, porém os períodos são construídos com `entrada=localizar(entrada)` nos motores (`app/ponto/motor_calculo_v2.py:2751`, `:2878`, `:3203`). Possível erro por dia de turno diferente do dia civil ainda dependeria de outro contra-exemplo [não verificado]. Não atribuí erro a `date.today()` isoladamente; configuração é São Paulo e `USE_TZ=True` (`app/config/settings/base.py:93-95`).

| achado | arquivo:linha | contra-exemplo | certeza | dono L-099 |
|---|---|---|---|---|
| Vínculo da data civil aplicado ao turno anterior | `app/chamados/detector_anomalias.py:144`, `:162`, `:186` | A22–06 até04/11; Bcomercial desde05/11; E04 22h/S05 06h45 retorna ok | Alta no mecanismo; integração [não verificado] | ESTRUTURA |
| Noite dividida em dias no propositor | `app/escala/detector_proposta.py:58` | 15 turnos alternados22–06 viram30 dias, entrada/saída22h | Alta | ESTRUTURA |

_1744 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
