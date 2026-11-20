# CODEX-ESTADO

> Ultima resposta processada: **20/11/2026 00:22** · publicado em **20/11/2026 00:25** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 20/11 00:22 · sessao 8aca83b0e978 · TRAVAS-SEM-MEMORIA · verificador · origem=Code

# AVAL-DEVOLVIDO — TRAVAS-SEM-MEMORIA-2011-0020

## 1. PREMISSAS — PROCEDE

- **(a) Hooks do Code.** Na cópia, `.claude/settings.json` da raiz aponta para `/home/ronald/saas-hasner/bin/`. `Stop` chama `hook_stop_fila1.py`. `PreToolUse` chama `disjuntor.py pretool`, que recebe o JSON no stdin e lê o comando Bash em `tool_input.command`. O `tool_input.prompt` que ele lê pertence à ferramenta `Agent`. O evento previsto por T2 para o texto colado pelo usuário é `UserPromptSubmit`, sem registro nessa configuração. `PreCompact` manual/auto chama `handoff_sessao.sh`. `SessionStart` compact/clear imprime o handoff e chama `marco_de_clear.py`. A `statusLine` chama `statusline_cota.py`. O RELATO registra uma sessão anterior em `app/` em que a configuração da raiz ficou inerte e só disparou o Stop de `~/.claude/settings.json`. Configuração efetivamente carregada hoje na VM **[nao verificado]**. `.claude/settings.json:2–77`, `bin/disjuntor.py:1837–1843,1864–1867,1912–1923`, `bin/hook_stop_fila1.py:1339–1345`, `app/docs/RELATO.md:3341`.
- **(b) “`bin/suite.sh` (pista 2)” não fecha.** O script da cópia roda Docker local sob `bin/trava_teste.sh`. A **PISTA-2** documentada é o executor remoto no servidor do GLPI, com fila própria e entrada `ssh pista pede` para uma árvore commitada. A escolha de rota dentro de `bin/suite.sh` aparece como obra separada, com opção A registrada para `--volta` e pre-opus de lateral. Estado atual do executor remoto **[nao verificado]**. `bin/suite.sh:316–325`, `app/docs/BACKLOG.md:2191,2193–2195`, `app/docs/RELATO.md:65`.
- **(c) “Por cópia em obra” precisa de universo definido.** `bin/relato.sh` já gera `ESTADO.md` por `bin/gerar_estado.py`, passa pela raspagem e publica. O encaixe de `copia=<nome>@<HEAD12>` é a seção **Raias**, que já usa `bin/raias_estado.py`. Esse medidor cruza `git worktree list --porcelain` e declarações `.agente`, inclui as declaradas, as à frente de `main` e as sujas, exclui a principal e as worktrees do harness, e agrupa as só sujas. Uma declaração pode aparecer sem caminho no disco. `wt-codex-*` é a convenção do construtor Codex. O BACKLOG registra a ordem da fila. O medidor é a fonte existente para as raias do ESTADO, mas a equivalência entre esse universo e **todas** as cópias em obra fica **[nao verificado]**. `bin/relato.sh:34–38,114–117,145–147`, `bin/gerar_estado.py:211–229,329,378–380`, `bin/raias_estado.py:13–27,73–135`, `bin/codex_pergunta.sh:217–232`, `bin/hook_stop_fila1.py:1394–1403`.
- **(d) “Suite completa rodada na VM (fora do `bin/suite.sh`)” abrange chamadas legítimas.** A régua inteira e o pre-push rodam fora de `suite.sh` por exceção expressa do §3 e pegam a trava diretamente. Ambos entram no alcance literal de T1, embora o hook de ferramenta só veja a chamada externa e a negativa efetiva dependa da implementação. A volta intermediária chama `suite.sh --volta`, com ruff e selo do disjuntor. Ela fica livre enquanto a seleção for um recorte efetivo. A guarda atual considera inteira a seleção vazia ou a que cobre todos os módulos de teste reconhecidos, mesmo quando os módulos são nomeados em `--only`. Uma inteira com `--dir … --motivo "pre-opus OBRA"` é válida hoje e cairia na segunda recusa de T1 se faltasse `PRONTO`. Se o argumento for literalmente `--motivo "pre-opus"`, já sai `rc 2` por faltar a obra. `CLAUDE.md:95–106`, `bin/regua.sh:47–51,165–169`, `bin/pre-push.sh:142–145,184–198`, `bin/volta_intermediaria.sh:18–33`, `bin/suite.sh:215–263`.
- **(e) “A linha ‘PRONTO item a item’ dessa obra” não tem formato demonstrado.** O RELATO traz prosa com formas como `CADEIA-20 PASSO 1 PRONTO (…)`, `PRONTO lista de repasse (…)` e `PRONTO (a) E3-NA-PROXIMA-VAGA …`. Alguns exemplos identificam a obra, mas o ID ocupa posições diferentes. Um formato canônico que permita ao hook casar a obra inteira, a volta e a prova exigida está **[nao verificado]**. `app/docs/RELATO.md:254,775,1753`, `CLAUDE.md:439–442`.

## 2. COLISÃO COM AVAL OU REGRA VIGENTE — PROCEDE

- **T1 — “SUITE CHEIA SO PELA PISTA 2” e “só lança se o RELATO tem […] PRONTO”.** A primeira frase conflita com as portas próprias da régua e do pre-push e, se “pista 2” for o executor remoto, com a destinação vigente de pouso e pre-opus da espinha à VM. A regra `TRAVA-MENOS-FILA-COM-SELO` já exige `pre-opus <OBRA>` ou `pouso <OBRA>` para a inteira em `suite.sh`, com `rc 2` na recusa. O novo `PRONTO` também precisa preservar a saída após mais de três voltas Codex, pois a regra manda subir ao opus e exige a inteira verde antes dele. O RELATO registra a retirada de uma guarda anterior que fechava essa saída. `CLAUDE.md:95–106,372–381`, `bin/suite.sh:236–263`, `app/docs/RELATO.md:65,1010,1209`, `app/docs/BACKLOG.md:2089`.
- **T2 — “devolve […] e não executa”.** Recusar apenas o aval inválido é compatível com seguir a fila. Se “devolve” encerrar o turno enquanto houver outro item que possa andar, colide com PARADA. O `PAREI` do Stop exige `!` no item em curso e considera os demais itens andáveis. A regra também prevê `ESPERANDO` com prova. A semântica da recusa de T2 no cliente fica **[nao verificado]**. `CLAUDE.md:262–275,296–298`, `bin/hook_stop_fila1.py:1232–1245,1260–1276,1381–1403`.
- **CODEX, contrato e pouso.** Preservar o wrapper, o despacho e as `#G` é compatível com o aval. A configuração e o código do instrumento cabem à sessão, com verificação pelo Codex. “REGISTRAR […] + CONSTRUIR” continua sujeito às cinco linhas no RELATO **antes** do código — quem chama, quem espera, falha/limite, medida e quem desliga — e ao envio único ao Codex. O contrato desta obra está **[nao verificado]**. O instrumento tem pouso próprio, conforme RAIAS item 5. `CLAUDE.md:325–329,344–358,394`, `app/docs/PISTA.md:169–176`, `bin/codex_pergunta.sh:122–135,241–254`.
- **Retenção.** A regra manda arquivar conteúdo com mais de três dias no mesmo commit. Uma linha `PRONTO` arquivada pode desaparecer do RELATO vivo antes da suite. T1 não define como a prova continua válida depois dessa transferência. `CLAUDE.md:439–442`, `bin/relato.sh:114–131`.

## 3. CONDIÇÃO QUE NÃO FECHA NO LITERAL — PROCEDE

- **“Suite completa rodada na VM (fora do `bin/suite.sh`)”.** T1, como hook do Code, alcançaria o lançamento avulso que o Code envia à ferramenta Bash e que o hook consiga classificar como inteiro. O evento vê o comando externo. Em `git push`, por exemplo, o pre-push roda como hook filho do Git. Na pista remota, a entrada documentada recebe SHA e opções, sem `--motivo` ou ID explícito da obra para casar com o `PRONTO`. Cobertura de toda a VM ou da pista pelo hook local **[nao verificado]**. `bin/disjuntor.py:1794–1798,1837–1843`, `bin/hooks.sh:10–11,61–65`, `bin/trava_teste.sh:35–42`, `app/docs/BACKLOG.md:2191`.
- **“Todo aval colado que diga CONSTRUIR” e “ou `copia=nova`”.** O anexo não define como reconhecer uma ordem efetiva de construção em meio a citações, proibições ou itens só de registro. Também falta a regra que associa cada item à sua cópia quando há várias, que distingue uma abertura real de um `copia=nova` escrito no texto, e que fixa o marco para reconhecer avais anteriores ao pouso. O RELATO já registra uma obra com duas cópias descendentes. Para instrumento sem worktree nomeada, falta definir que cópia e HEAD o aval deve citar. A regra vigente já exige preparar a mudança em cópia do HEAD e aplicar no commit. `app/docs/RELATO.md:1513`, `CLAUDE.md:37–40,309–313,436–438`, `.claude/settings.json:2–73`.
- **“`<HEAD 12>`” comparado com “`git rev-parse` da cópia”.** Igualdade textual entre 12 caracteres e o hash completo recusa um HEAD atual. O aval precisa fixar a normalização e a identidade da cópia. O wrapper já distingue o hash completo usado na guarda do prefixo de 12 usado na trilha, e confere a limpeza da cópia separadamente. Um HEAD igual, por si, deixa alterações locais fora da conferência. `bin/codex_pergunta.sh:79,252–259,304–308,409–415`, `bin/raias_estado.py:100–102`.
- **“Smoke dos 6 casos no ar” e “teste das duas travas na suite”.** O anexo enumera três resultados de T1 e quatro de T2, além da publicação no ESTADO, mas não identifica quais resultados compõem os seis casos nem onde os hooks devem estar ativos para a prova. Se o `PRONTO` exigido para o pre-opus da própria obra já incluir o smoke da configuração ativa, “pouso sem smoke” deixa a ordem de ativação e certificação indefinida. Hoje os selos de host são chamados pela régua, enquanto `suite.sh` executa o runner Django. O RELATO registra um precedente de hook validado com selos de host e configuração ativa, sem deploy do produto. Resultados dos quatro casos pré-cura e smoke na VM **[nao verificado]**. `bin/regua.sh:134–145`, `bin/suite.sh:316–325`, `app/docs/RELATO.md:3341,4356`.

## 4. EXPORTADA, DINHEIRO, MOTOR V2 OU `!` FORA DO DECLARADO — NAO PROCEDE

O escopo declarado é hook do Code e publicação do ESTADO. O anexo não pede alteração do gravado de exportada, apply de dinheiro, mudança em `ponto/motor_calculo_v2.py`, atalho ou outro ato da PARADA item 2. Configuração do Code **é tocada** para registrar o hook de prompt. A vedação de CODEX se aplica ao Codex, inclusive no papel construtor. Há precedente de configuração feita como instrumento, e também de instalação negada à sessão pelo classificador. O `PreToolUse` atual protege contra remover o próprio disjuntor. Permissão efetiva e carregamento da configuração nesta VM **[nao verificado]**. `CLAUDE.md:141–142,265–268,344–355,406–417`, `.claude/settings.json:62–73`, `bin/codex_pergunta.sh:122–135`, `bin/disjuntor.py:218–245`, `app/docs/RELATO.md:4356,4411`.

A posição nas vagas do `ABRE-AS-LATERAIS-1911-2340` e a lista atual de cópias na VM ficam **[nao verificado]**.

**BLOQUEIA=3 NAO_BLOQUEIA=1**

## 20/11 00:17 · sessao ce1b950b7668 · E4C-F5-CONSTROI · construtor · origem=Code

Implementei a volta B2 em seis arquivos. Conferi compilação em memória, `git diff --check`, os asserts preexistentes e as âncoras dos mutantes. Executei também o predicado isolado do M1 com os juízes reais em memória. Resultados com ORM, suíte e certificação pelo Code e pelo Opus final [nao verificado]. `escala/services/cadastro_realidade.py:497`, `chamados/tests/e4c_f5_voltaB_mutantes.py:28`.

## Arquivos tocados

- Produção — `api/views.py:2566`, `chamados/services/validacao.py:914`, `escala/services/cadastro_realidade.py:497`.
- REDs e mutantes — `chamados/tests/test_e4c_f5_voltaB_sitios.py:66`, `chamados/tests/test_e4c_f5_voltaB_lotes.py:182`, `chamados/tests/e4c_f5_voltaB_mutantes.py:28`.

## Antes e depois

| Item | Base `d0210a400` | B2 |
|---|---|---|
| **1 · GET** | O juiz de `respondidas` recebia a composta e `_item` publicava o instante da coluna. `api/views.py:2581`, `:2597` na base. | **Sim, `perguntas` e `historico` usam o mesmo `_item`.** A troca é coerente para os três. `_item` recebe a composta e publica seu par, com `respondida_em` em `isoformat()` ou `None`. O juiz de `respondidas` recebe a mesma instância de leitura usada para montar o item reutilizado do histórico. `api/views.py:2566`, `:2579`, `:2581`, `:2586`, `:2601`. |
| **2 · Recusa** | A foto do evento usava o instante composto e a linha imutável fotografava a coluna. `chamados/services/validacao.py:914` na base, `chamados/services/disputa_emissao.py:1640`, `:1643`. | A composta alimenta só o juiz. A foto do evento lê as colunas antes da reabertura e serializa o instante em `isoformat()` quando presente, preservando `None`. `chamados/services/validacao.py:902`, `:913`, `:926`, `:935`. |
| **3 · M1** | Testava só o instante recortado, pelo índice `[1]`. `escala/services/cadastro_realidade.py:502` na base. | Usa `projecao_temporal_da_pergunta(p, agora=None)` com o `agora` do M1 e entrega o par recortado a `pergunta_respondida(p)` pelo `_replace` da composta. O corte limpa instante e texto quando `respondida_em > agora` e conserva a igualdade. O juiz aplica instante **e** texto não vazio, conforme a L-130. `escala/services/cadastro_realidade.py:497`, `:508`, `chamados/juizes.py:1383`, `:1397`, `:1551`, `:1560`, `docs/LEIS.md:151`. |
| **4 · REDs e K46** | O GET conferia ID e texto, a recusa conferia o estado posterior, e a medição do K46 compartilhava PKs com o K45. `chamados/tests/test_e4c_f5_voltaB_sitios.py:73`, `:118`, `chamados/tests/test_e4c_f5_voltaB_lotes.py:165` na base. | O GET exige o instante vigente do ato. A recusa compara o evento persistido com a linha `RECUSA` para coluna nula e para coluna com instante divergente. O M1 cobre texto vazio, texto presente no limite e instante futuro. O novo K46 compara 3 × 6 perguntas sem validação, fora da janela do K45 e ligadas ao chamado só pelo destino. O conjunto de PKs é próprio do K46. Com `ficam = []`, `fica_pergunta_viva_fora` perde o chamado e a asserção deve falhar. `chamados/tests/test_e4c_f5_voltaB_sitios.py:78`, `:132`, `:276`, `chamados/tests/test_e4c_f5_voltaB_lotes.py:40`, `:182`, `chamados/services/competencia_trancada.py:119`, `:164`, `:175`. |
| **5 · Asserts e tetos** | Contagem AST de `assert` e `self.assert*`. | Sítios **37 → 44**, lotes **24 → 27**, mutantes **2 → 3**. Todos os anteriores têm a mesma expressão AST. A igualdade de SELECTs 3 × 6 continua com o mesmo limite. `chamados/tests/test_e4c_f5_voltaB_sitios.py:66`, `:116`, `:276`, `chamados/tests/test_e4c_f5_voltaB_lotes.py:40`, `:182`, `chamados/tests/e4c_f5_voltaB_mutantes.py:75`. |

## Quem escreve e quem lê

| Campo | Escrita e significado nos sítios tocados |
|---|---|
| `respondida_em` | As portas humana e automática gravam a coluna e registram o instante na linha `RESPOSTA`. A reabertura limpa a coluna pela porta de lote. GET, juiz da recusa e M1 leem o instante **vigente** da composta, com corte adicional no M1. A foto da recusa e a linha imutável leem o instante **gravado na coluna antes do ato**. Esses dois significados podem divergir. `chamados/services/disputa_emissao.py:1528`, `:1532`, `:1683`, `:1686`, `:1617`, `:1656`, `chamados/models.py:4339`, `:4358`, `api/views.py:2581`, `chamados/services/validacao.py:902`, `:914`, `escala/services/cadastro_realidade.py:498`. |
| `resposta_colab` | As portas humana e automática escrevem o texto da projeção e a reabertura o limpa. GET, juiz da recusa, M1 e foto da recusa partem da **mesma coluna**. No M1, o corte pode limpar o par entregue ao juiz. `chamados/services/disputa_emissao.py:1481`, `:1682`, `:1617`, `:1656`, `chamados/models.py:4358`, `api/views.py:2579`, `chamados/services/validacao.py:902`, `:913`, `escala/services/cadastro_realidade.py:498`, `chamados/juizes.py:1397`. |
| `respondida_original_em` | `reabrir_para_correcao` grava na linha `RECUSA` a coluna anterior ao ato. O RED lê a linha persistida e compara seu instante com `LogAuditoria.valor_antes['respondida_em']`. Ambos representam a mesma foto, em `datetime` na linha e ISO no evento, ou `None` nos dois. `chamados/services/disputa_emissao.py:1639`, `:1643`, `chamados/services/validacao.py:935`, `core/observ.py:133`, `chamados/tests/test_e4c_f5_voltaB_sitios.py:132`. |

## Comandos para o Code

A partir de `app/`. Com os REDs da B2 sobre `d0210a400`, as falhas esperadas são `get` pelo instante nulo, `recusa` pela divergência da foto e `m1` pelo subcaso de texto vazio. Os casos K46 verificam o lote já presente na base e devem cair com os respectivos mutantes. Execução [nao verificado]. `chamados/tests/test_e4c_f5_voltaB_sitios.py:78`, `:138`, `:296`, `chamados/tests/test_e4c_f5_voltaB_lotes.py:179`, `:182`.

```bash
s=chamados.tests.test_e4c_f5_voltaB_sitios
l=chamados.tests.test_e4c_f5_voltaB_lotes.CarregadoresVoltaBTest

get="$s.SitiosVoltaBTest.test_RED_get_do_app_reconhece_instante_do_ato"
recusa="$s.SitiosVoltaBTest.test_RED_recusar_usa_instante_do_ato_e_reabre_sem_reusar_prefetch"
m1="$s.BecoTemporalVoltaBTest.test_RED_m1_exige_texto_e_instante_no_horizonte"
k46="$l.test_RED_k46_julga_cada_pergunta_fora_da_competencia"
k46_lote="$l.test_RED_k46_perguntas_do_chamado_na_competencia"
selo="$s.SeloMutanteVoltaBTest.test_MORDE_get_do_app_com_pergunta_crua"

PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  "$get" "$recusa" "$m1" "$k46" "$k46_lote" "$selo"

mutar() {
  local seletor="$1"
  shift
  F5_VOLTA_B_MUTANTE="$seletor" PYTHONDONTWRITEBYTECODE=1 \
    python3 manage.py test "$@"
}

mutar get_instante_da_coluna "$get"
mutar recusa_foto_do_ato "$recusa"
mutar m1_so_instante "$m1"
mutar m1_sem_corte "$m1"
mutar k46_sem_julgamento "$k46"
mutar k46 "$k46_lote" "$k46"
mutar api_p_cru "$selo"
```

As mordidas esperadas são `None` no GET, divergência entre as fotos na recusa, inclusão indevida com texto vazio ou instante futuro no M1, perda do chamado em `fica_pergunta_viva_fora` com `ficam = []`, crescimento dos SELECTs sem o lote K46 e acusação da chamada crua no selo. A acusação do `api_p_cru` foi observada na análise AST em `api/views.py:2590`. `chamados/tests/e4c_f5_voltaB_mutantes.py:21`, `:28`, `:118`, `chamados/tests/test_e4c_f5_voltaB_lotes.py:51`, `:191`.

LEI-AKITA — L-130, padrao=juiz unico puro + fonte unica (compositor). `docs/LEIS.md:151`, `chamados/juizes.py:1551`, `chamados/models.py:4351`.

## 20/11 00:09 · sessao 1ac528925785 · E4C-F5-CONSTROI · verificador · origem=Code

## 1. Escopo — BLOQUEIA

Os **14 chamadores** e os **27 carregadores** declarados aparecem no diff. Os 14 passam uma `LeituraComposta` ao juiz indicado, e os 27 acrescentam o prefetch à consulta que produz as instâncias. Os blobs dos 31 arquivos do diff conferem com a cópia. [nao verificado] para a identidade Git independente do arquivo exportado e para a numeração original da `matriz.md` e do `predesenho.md`, que não vieram como anexos separados.

| Chamador | Sítio conferido |
|---|---|
| GET e POST do app | `app/api/views.py:2597`, `app/api/views.py:2641` |
| `acao_regularizar_disputa` e `classificar` o beco | `app/chamados/services/acoes_disputa.py:134`, `app/chamados/services/becos.py:32` |
| `declarar_hora_colab` e `aplicar_declaracao` | `app/chamados/services/declaracao_texto.py:418`, `app/chamados/services/declaracao_texto.py:688` |
| `validar_pergunta` e `recusar_pergunta` | `app/chamados/services/validacao.py:682`, `app/chamados/services/validacao.py:902` e `:903` |
| As duas listas de `validar_inline` | `app/chamados/views_cobrar.py:170`, `app/chamados/views_cobrar.py:178` |
| Resposta web e ficha do colaborador | `app/chamados/views.py:1275`, `app/colaboradores/services/ficha.py:173` |
| M1 e material da prévia | `app/escala/services/cadastro_realidade.py:502`, `app/folha/previa.py:143` |

| K | Carga conferida |
|---|---|
| 4, 29, 30, 31, 32 | Vivas da disputa, corrigíveis, fila a validar, dias das vivas e disputa do chamado. `app/chamados/juizes.py:1512`, `:1765`, `:1773`, `:2437`, `:2490` |
| 11, 23 | Perguntas do evento do beco por `perguntas__decisoes` e perguntas da janela da prévia. `app/chamados/services/becos.py:87`, `app/folha/previa.py:66` |
| 25, 26, 27 | Fato do marco avulso, fatos da regularização e fato memoizado das propostas. `app/chamados/services/declaracao_texto.py:475`, `:588`, `:635` |
| 44, 45, 46 | Detalhe do chamado, janela da competência e perguntas de cada chamado da medição. `app/chamados/services/acoes_chamado.py:352`, `app/chamados/services/competencia_trancada.py:121`, `:166` |
| 47, 48, 49 | Disputa candidata sem credor, destino da migração e origem da migração. `app/chamados/services/disputa_emissao.py:2080`, `app/chamados/services/fio.py:2296`, `:2300` |
| 50, 53, 54, 55 | Fatos do app, pendentes da reabertura, janela do calendário e dia do colaborador. `app/chamados/services/pergunta_no_app.py:186`, `app/chamados/views.py:1412`, `app/colaboradores/services/calendario.py:505`, `app/ponto/services/dia_do_colab.py:65` |
| 61, 62, 63, 64 | Fallback do explicador, foto após validação, disputa após carimbos e perguntas do dia do filho. `app/chamados/services/explicador.py:161`, `app/api/foto_recebida.py:81`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:84`, `app/chamados/services/limbo_folgas.py:205` |
| 65, 67, 68 | Relação externa de todos os chamados do queryset do supra-juiz, guarda do comando de relavra e dedup do emissor. `app/ponto/management/commands/supra_juiz.py:192` e `:215`, `app/ponto/management/commands/reconciliar_grade.py:461`, `app/chamados/services/disputa_emissao.py:1436` |

K41 e K42 já aparecem carregados em `app/chamados/models.py:1555` e `:2077`. O K40 mantém `.get(pk=pk)` dentro do laço. O saldo **24 C + 6 R** é compatível com a reclassificação descrita no relato. O censo nominal completo das 30 sobras fica [nao verificado]. `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:464` e `:466`.

Nenhum sítio de produção nomeado no brief ficou de fora. Os quatro arquivos reservados à C estão fora do diff e suas dez identidades continuam no `RESTAM`. O selo AST passou na cópia. `app/chamados/tests/test_e4c_f5_selo_chamada.py:21` e `:194`.

O trecho fora da lista literal é **`'respondida_em': antes_do_ato.respondida_em.isoformat()`**. Ele muda a fonte da foto de auditoria publicada por `recusar_pergunta`, além de converter a chamada do juiz. A mudança evita o `.isoformat()` sobre `None` no passivo do RED, mas deixa a foto da própria linha `RECUSA` lendo outra fonte. `CLAUDE.md:37`, `app/chamados/services/validacao.py:914` e `:935`, `app/chamados/services/disputa_emissao.py:1640` e `:1643`, `app/chamados/tests/test_e4c_f5_voltaB_sitios.py:110`.

## 2. Cache depois da escrita — NAO BLOQUEIA

Nas oito portas, a leitura composta verifica o estado anterior à escrita de resposta, validação ou recusa. As releituras encontradas usam instância nova, refresh integral, invalidação pela porta de decisões ou leitura exclusivamente escalar. `app/chamados/models.py:4330` e `:4373`.

| Porta | Releitura depois da escrita |
|---|---|
| `validar_pergunta` | `_validar_o_par` faz refresh integral no rollback e no sucesso. A devolução relê escalares, e o refresh final lê só `validada_em` e `materializada_em`. No modal, a instância externa recebe refresh integral antes de `.estado`. `app/chamados/services/validacao.py:646`, `:655`, `:262`, `:576`, `:789`, `app/chamados/services/fio.py:662` e `:663` |
| `recusar_pergunta` | Depois da escrita em lote, a nota e o evento usam a foto anterior já guardada. A resposta do modal monta uma carga nova. O refresh do teste é de outra instância. `app/chamados/services/validacao.py:926`, `:931`, `:935`, `app/chamados/services/fio.py:486` e `:495`, `app/chamados/tests/test_e4c_f5_voltaB_sitios.py:112` e `:118` |
| `validar_inline` | As duas listas são montadas antes de escrever. Cada iteração passa apenas o PK ao escritor, cujo `get()` obtém outra instância. O JSON usa o resultado da porta e a contagem. `app/chamados/views_cobrar.py:170`, `:178`, `:209`, `:215`, `app/chamados/services/validacao.py:677` |
| `responder_pergunta_disputa` | O núcleo grava a decisão com `pergunta=p`, invalidando o cache dessa instância antes de `p.resposta_display`. A view devolve a confirmação. `app/chamados/services/disputa_emissao.py:1532` e `:1568`, `app/chamados/models.py:4376`, `app/chamados/views.py:1278` e `:1282` |
| `api_disputa_responder` | Usa o mesmo núcleo. O JSON lê o instante escalar atualizado em memória. `app/api/views.py:2648` e `:2658`, `app/chamados/services/disputa_emissao.py:1528` e `:1568` |
| `acao_regularizar_disputa` | O materializador relê por PK sob trava e, ao final, recarrega as vivas por um queryset novo. A ação usa o resultado devolvido. `app/chamados/services/acoes_disputa.py:134` e `:143`, `app/chamados/services/materializacao.py:2117`, `:2127`, `:2024`, `app/chamados/juizes.py:1512` |
| `aplicar_declaracao` | Depois da resposta, a validação recebe o PK e faz `get()` novo. `app/chamados/services/declaracao_texto.py:694` e `:697`, `app/chamados/services/validacao.py:677` |
| `declarar_hora_colab` | Há refresh integral depois da costura, antes da guarda de resposta. Depois da resposta, a releitura de exibição é a do núcleo já invalidado. A declaração anexa a nota e devolve o PK. `app/chamados/services/declaracao_texto.py:371`, `:373`, `:418`, `:420`, `:424`, `app/chamados/services/disputa_emissao.py:1568` |

O K11 também relê `d` integralmente depois dos resolvers e antes de `d.pode_fechar()`. No K47, o fecho final faz refresh e consulta as perguntas de novo. `app/chamados/services/becos.py:58` a `:61`, `app/chamados/services/disputa_emissao.py:2127` e `:2134`.

## 3. Escritores, leitores e significado — BLOQUEIA

| Campo ou retorno | Escrita e leitura no percurso |
|---|---|
| `validada_em` | A validação grava, as devoluções limpam, a resposta humana e a reabertura limpam, `carimbar` pode preencher e a ressuscitação limpa em lote. `app/chamados/services/validacao.py:168`, `:273`, `:349`, `app/chamados/services/disputa_emissao.py:1494` e `:1617`, `app/chamados/models.py:3352` e `:1721`. C6, `estado` e respostas sem veredito leem a validação vigente do ato. As guardas de validar, recusar, declarar/aplicar e a seleção S128 ainda leem o carimbo da coluna. `app/chamados/juizes.py:1408`, `app/chamados/models.py:3256` e `:2025`, `app/chamados/services/validacao.py:685` e `:898`, `app/chamados/services/declaracao_texto.py:228`, `app/chamados/services/disputa_emissao.py:2085`. |
| `respondida_em` | Resposta humana, automática e reconstituição pela trilha gravam o instante da operação respectiva. A porta de carimbo pode completar o instante legado. Reabertura, ressuscitação e devolução L-130 zeram o par. `app/chamados/services/disputa_emissao.py:1528`, `:1683`, `:1705`, `:1617` e `:1656`, `app/chamados/models.py:3349` e `:1722`, `app/chamados/services/validacao.py:440`. Os chamadores da B recebem o instante vigente composto. A página do app, faces SQL, ordenação da fila e foto imutável da recusa ainda leem a coluna. `app/api/views.py:2581`, `app/chamados/juizes.py:1588`, `:1599`, `:1774`, `app/chamados/services/disputa_emissao.py:1640` e `:1643`. |
| `resposta_colab` | As portas humana e automática escrevem o texto. As portas que limpam o par zeram o texto junto. O compositor, o texto do GET, o dia do colaborador, o explicador e a igualdade da declaração usam a projeção. A exibição formatada e a tarefa do dia têm leitores preexistentes do texto do ato. `app/chamados/services/disputa_emissao.py:1481`, `:1682`, `:1617`, `app/chamados/models.py:1722`, `:4358`, `:3837`, `app/api/views.py:2579` e `:2580`, `app/ponto/services/dia_do_colab.py:70`, `app/chamados/services/explicador.py:102`, `app/chamados/services/declaracao_texto.py:418`, `app/chamados/services/tarefa_do_dia.py:165`. |
| `materializada_em` | `_escrever_carimbo`, suas portas e o descarimbo em lote mantêm o instante de encerramento registrado. C6, o compositor, as guardas e o materializador leem a coluna. Lastro é um julgamento à parte. `app/chamados/models.py:3316`, `:3346`, `:3400`, `:3425`, `:3508`, `:4359`, `app/chamados/juizes.py:1399` e `:1455`, `app/chamados/services/disputa_emissao.py:2085`, `app/chamados/services/materializacao.py:2107`. |
| `via_resolucao` | As portas de carimbo e descarimbo escrevem a via. A confirmação pelo espelho e `relavrar` também podem trocar só a via. Leem-na o compositor, C6, a identidade do fato, S128, o comando de encerramento e o explicador. `app/chamados/models.py:3318`, `:3508`, `:3517`, `:3418`, `:4359`, `app/chamados/services/relavra_via.py:125`, `app/chamados/juizes.py:1401`, `:1421`, `:1449`, `app/chamados/services/disputa_emissao.py:2095`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:34`, `app/chamados/services/explicador.py:118`. |
| `decisoes`, `leitura()` e `composta()` | A FK é `decisoes`, ordenada por `ato_em, pk`. `registrar()` insere as linhas das portas e invalida o cache da instância passada. `leitura()` incorpora o passivo sem linha correspondente e devolve validação, instante e texto vigentes do ato. `composta()` usa os dois instantes dessa leitura, o texto da projeção e as colunas de materialização e via. C6 e os 14 chamadores acima usam a composição. Histórico da tela, tarefa, auditoria e backfill também leem a sequência para seus próprios fins. `app/chamados/models.py:4188`, `:4222`, `:4273`, `:4330`, `:4348`, `:4358`, `:4372`, `:3823`, `app/chamados/juizes.py:1408`, `app/chamados/services/historico.py:98`, `app/chamados/services/tarefa_do_dia.py:429` e `:545`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:342`. |

Os carimbos são `DateTimeField`, com `USE_TZ=True` e fuso configurado `America/Sao_Paulo`. `data_evento` é uma data, com dia do turno para pergunta de fato e dia civil para conversa. O corte temporal mantém a igualdade com `agora`, pois descarta `> agora`. `leitura()` roda a sequência inteira antes desse corte. A tarefa do dia, quando pede histórico no horizonte, limita as linhas a `ato_em <= agora` antes da leitura. `app/chamados/models.py:2630`, `:2654`, `:3668`, `:4327`, `app/config/settings/base.py:93` e `:95`, `app/chamados/juizes.py:337` e `:1394`, `app/chamados/services/tarefa_do_dia.py:400` e `:429`.

**Divergências que bloqueiam**:

- **GET do app:** `if _pergunta_respondida(DecisaoPergunta.composta(p))` põe na lista `respondidas` a pergunta com instante vigente no ato e texto na projeção. O mesmo item sai com **`'respondida_em': p.respondida_em.isoformat() if p.respondida_em else None`**. O caso do RED devolve `null` nessa chave. O efeito no consumidor APK e a incidência em produção ficam [nao verificado]. `app/api/views.py:2581` e `:2597`, `app/chamados/tests/test_e4c_f5_voltaB_sitios.py:39` e `:64`.
- **Validação e aplicação:** `if not _pergunta_respondida(DecisaoPergunta.composta(perg))` deixa passar o par com instante no ato e texto na projeção. Na mesma operação, `_recusa_sem_anuencia` usa **`if pergunta_respondida(p):`** sobre a instância crua. Para uma resposta de hora com coluna de instante vazia, a validação pode carimbar e encaminhar uma resposta que o materializador recusa como “sem resposta do colaborador”. `app/chamados/services/validacao.py:682`, `:743`, `:775`, `app/chamados/services/declaracao_texto.py:688` e `:697`, `app/chamados/services/materializacao.py:1300` e `:1355`.
- **Recusa:** o evento guarda **`antes_do_ato.respondida_em.isoformat()`**, enquanto a linha imutável guarda **`respondida_original_em=_resp`** obtido da coluna. No passivo montado pelo RED, a foto do evento tem o instante e a foto da linha tem `None`, para a mesma recusa. `app/chamados/services/validacao.py:914` e `:935`, `app/chamados/services/disputa_emissao.py:1640` e `:1643`, `app/chamados/tests/test_e4c_f5_voltaB_sitios.py:110`.

## 4. Classes que já reprovaram obra — BLOQUEIA

| Classe | Veredito |
|---|---|
| **a. Segundo juiz** | Os chamadores delegam aos juízes existentes. O M1 mantém um predicado local divergente para “respondida”, descrito em **d**. `app/docs/JUIZES.md:38`, `:102`, `app/chamados/juizes.py:1551`, `app/escala/services/cadastro_realidade.py:502`. |
| **b. Segundo escritor** | A B troca insumos de guardas e da foto de auditoria. A gravação segue pelas portas existentes, inclusive na recusa. `app/chamados/services/validacao.py:923` e `:926`, `app/chamados/services/declaracao_texto.py:694`, `app/chamados/models.py:4362`. |
| **c. Relógio fora da borda** | Nenhuma leitura nova de relógio no diff. O M1 repassa `agora` e o GET conserva o relógio da borda. O fallback avulso `date.today()` já existe no C6. `app/escala/services/cadastro_realidade.py:449` e `:502`, `app/api/views.py:2552`, `app/chamados/juizes.py:597`. |
| **d. Derivação paralela** | **BLOQUEIA.** No M1, **`projecao_temporal_da_pergunta(DecisaoPergunta.composta(p), agora)[1] is not None`** decide “respondida” só pelo instante. O compositor fornece o texto, mas o predicado o descarta. A divergência passa a valer também para o instante recuperado do ato. `app/escala/services/cadastro_realidade.py:500` e `:502`, `app/chamados/juizes.py:1560`. |
| **e. RED ou selo sem mordida** | **BLOQUEIA nos pontos abaixo.** O filtro conta apenas SELECT cujo primeiro `FROM` é `chamados_decisaopergunta`. A igualdade dos conjuntos confirma que os 3 e 6 PKs chegaram ao compositor em cada percurso. Ela não atribui as chamadas a um carregador específico. `app/chamados/tests/test_e4c_f5_opus_models.py:25`, `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:40` a `:52`. |
| **f. Ordem de trava** | K48/K49 leem depois da trava global, K47 depois da trava do resolver. Os prefetches são SELECTs comuns. O K11 já carregava `perguntas` antes do ato e agora carrega também as decisões. `app/chamados/services/fio.py:2252`, `:2264`, `:2277`, `:2296`, `:2300`, `app/chamados/services/disputa_emissao.py:1914` e `:2080`, `app/chamados/services/becos.py:43`, `:53`, `:87`. |
| **g. Campo com dois significados** | A B conserva os tipos dos campos. O GET e a recusa passam a publicar versões diferentes de um mesmo instante lógico, conforme o item 3. `app/api/views.py:2581` e `:2597`, `app/chamados/services/validacao.py:914`, `app/chamados/services/disputa_emissao.py:1640`. |
| **h. Manager ou SQL proibido** | Os acréscimos estão nos querysets locais. A subconsulta de validação já entra por `q_vivas()` na fila e por `q_nao_validada_pelo_ato()` no comando existente. A B acrescenta prefetch a essas cargas. `app/chamados/juizes.py:1772` e `:1773`, `app/chamados/models.py:3560` e `:3634`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:33` e `:84`. |

Os dois pontos exatos de **4e**:

- O RED do GET compara apenas **`(p['id'], p['resposta_colab'])`** e fica verde com `respondida_em: null`. O RED da recusa verifica **`(depois.respondida_em, depois.resposta_colab)`** numa instância que o próprio teste refresca, sem conferir `respondida_original_em` na linha gravada. `app/chamados/tests/test_e4c_f5_voltaB_sitios.py:73`, `:118`, `:120`, `app/chamados/services/disputa_emissao.py:1640`.
- No K46, **`{c.args[0].pk for c in composta.call_args_list}`** aceita os mesmos PKs julgados pelo K45 e pela guarda de fecho. Com a fixture toda validada, substituir o julgamento K46 por `ficam = []` conserva também `resultado['chamados'] == [chamado.pk]`. O RED de remoção do prefetch pode morder sem que o teste prove, isoladamente, a passagem de cada PK pelo K46. `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:49`, `:165` a `:176`, `app/chamados/services/competencia_trancada.py:144`, `:164` a `:178`.

As 14 âncoras dos mutantes de lote apontam para os prefetches reais. K45/K46 escolhem ordinais distintos de `medir`, K47 escolhe `:2080` e preserva `:1955`, e K11 volta a `'perguntas'`. O mutante `api_p_cru` acusou `app/api/views.py:2597` no selo executado em memória. As falhas dos mutantes ORM na asserção de custo ficam [nao verificado]. O 28/30 na base e o 30/30 na B são os dados agregados fornecidos pela sessão. `app/chamados/tests/e4c_f5_voltaB_mutantes.py:12`, `:31`, `:46`, `:59`, `:63`, `app/chamados/tests/test_e4c_f5_selo_chamada.py:194`.

## 5. Adaptações de teste — NAO BLOQUEIA [nao verificado]

| Arquivo preexistente | Classe e contagem `assert` / `self.assert*` |
|---|---|
| `app/chamados/tests/test_e3_r3_c5_sem_banco.py:180` | **ii**, compositor do helper existente no stub. **34 → 34**. `app/chamados/tests/duble_pergunta.py:12` |
| `app/chamados/tests/test_e3_r3_c6_sem_banco.py:299` | **ii**, mesmo helper. **52 → 52**. `app/chamados/tests/duble_pergunta.py:12` |
| `app/chamados/tests/test_disputa_s127.py:23` | **v**, `Linhas` preserva a lista com `prefetch_related` identidade. **8 → 8**. `app/chamados/tests/test_e4b_classe_e_memoria.py:165` e `:193` |
| `app/chamados/tests/test_e4c_f5_selo_chamada.py:21` | Alteração nominal pedida no brief, de **24 → 10** identidades em `RESTAM`. **12 → 12**. A asserção da catraca conserva o mesmo corpo. `app/chamados/tests/test_e4c_f5_selo_chamada.py:194` |

Os hunks dos quatro arquivos preservam os asserts e os valores esperados de negócio. Nenhum teto foi editado no delta, inclusive o limite O261 de 9 por candidata e os quatro tetos da batida. `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:207`, `app/ponto/tests/test_selo_chokepoint_escrita.py:196`, `:210`, `:228`, `:235`.

## 6. Leis — BLOQUEIA

- **L-130:** o trecho **`projecao_temporal_da_pergunta(DecisaoPergunta.composta(p), agora)[1] is not None`** mantém a segunda definição de “respondida” no M1. A lei exige instante **e** texto após trim. No fluxo da validação, a guarda composta da B e a guarda crua da materialização dão respostas diferentes ao mesmo par divergente, conforme o item 3. `app/docs/LEIS.md:151`, `app/escala/services/cadastro_realidade.py:502`, `app/chamados/services/validacao.py:682`, `app/chamados/services/materializacao.py:1300`.
- Os demais cortes examinados conservam seus pontos de decisão. A L-051 continua tratando tranca por leitura, a L-050 continua sem fronteira no dedup, e o recorte do fio continua delegado a `pergunta_viva`. O prefetch do calendário fica na guarda de perguntas, e a foto continua validando pela porta do dono. `app/docs/LEIS.md:91` e `:92`, `app/docs/CORTES.md:64`, `:82`, `:94`, `app/chamados/services/competencia_trancada.py:131` e `:203`, `app/chamados/services/disputa_emissao.py:1429` e `:1436`, `app/chamados/juizes.py:1526`, `app/colaboradores/services/calendario.py

[truncado em 20000 de 23005 caracteres]

## 19/11 23:42 · sessao 9535a0a5c084 · E4C-F5-CONSTROI · construtor · origem=Code

## E4C-F5-CONSTROI · Volta B

Implementei os **14 chamadores** e os **27 carregadores** da B. K41 e K42 já estavam em lote na base. A varredura AST mediu a catraca de chamadas em **24 → 10**, com zero chamadas cruas fora da lista restante e da permissão nominal do placar. O mutante do GET do app voltou a acusar `api/views.py:2597`. `chamados/tests/test_e4c_f5_selo_chamada.py:21`, `:35`, `:194`, `chamados/models.py:1555`, `:2077`.

A verificação local ficou em compilação em memória, `git diff --check` e análise AST. **RED na base, verde na B, crescimento dos mutantes de lote, paridade da prévia de novembro e os quatro números do chokepoint estão [nao verificado].** `chamados/tests/test_e4c_f5_voltaB_lotes.py:40`, `chamados/tests/test_e4c_f5_voltaB_sitios.py:64`, `ponto/tests/test_selo_chokepoint_escrita.py:435`.

## Chamadores diretos

As linhas indicam **base → B**. Em cada linha, o juiz passa de uma instância ORM para `DecisaoPergunta.composta(...)`. A projeção temporal de M1 conserva o argumento `agora`. Na recusa, a leitura recebe um nome e também fornece o instante à foto de auditoria, de modo que a foto funcione quando a coluna está vazia. `chamados/services/validacao.py:902`, `:913`, `escala/services/cadastro_realidade.py:502`.

| Sítio | Base → B | Carga |
|---|---|---|
| `api_disputa_perguntas` | `api/views.py:2596 → :2597` | App com lote já na base, `chamados/services/pergunta_no_app.py:57` |
| `api_disputa_responder` | `api/views.py:2640 → :2641` | Avulsa, `api/views.py:2632` |
| `acao_regularizar_disputa` | `chamados/services/acoes_disputa.py:134 → :134` | K4 |
| `classificar` | `chamados/services/becos.py:31 → :32` | K11 no evento, K69 no comando da C |
| `declarar_hora_colab` | `chamados/services/declaracao_texto.py:418 → :418` | Reuso via K17 ou abertura, `:251`, `:377` |
| `aplicar_declaracao` | `chamados/services/declaracao_texto.py:688 → :688` | Reuso via K17 ou abertura, `:663`, `:667` |
| `validar_pergunta` | `chamados/services/validacao.py:682 → :682` | K20, avulsa |
| `recusar_pergunta` | `chamados/services/validacao.py:902 → :903` | Avulsa, `:894` |
| `validar_inline`, pendentes | `chamados/views_cobrar.py:170 → :170` | K4 |
| `validar_inline`, aguardando | `chamados/views_cobrar.py:178 → :178` | K4 |
| `responder_pergunta_disputa` | `chamados/views.py:1275 → :1275` | Avulsa, `:1239` |
| `ficha_do_colab` | `colaboradores/services/ficha.py:172 → :173` | App com lote já na base, `chamados/services/pergunta_no_app.py:57` |
| `beco_em_lote` | `escala/services/cadastro_realidade.py:502 → :502` | K2, coberto na A, `chamados/juizes.py:1665` |
| `_material` | `folha/previa.py:142 → :143` | K23. O import local entrou junto da expressão autorizada em `:142` |

## Carregadores e arquivos tocados

Cada linha abaixo troca a carga anterior pela mesma consulta com `prefetch_related('decisoes')`, salvo as duas relações externas indicadas e os dois sítios já cobertos pelo Opus. Em `fio_vivo`, a redução `any` continua recebendo a coleção do chamador. A B adiantou as decisões nas cinco origens autorizadas K32, K62, K63, K64 e K67, mais K68, e documentou a responsabilidade da origem. `chamados/juizes.py:1516`, `:1524`, `:1526`.

| K | Sítio da carga na B | O que passou a vir em lote | [nao verificado]
|---:|---|---|
| 4 | `chamados/juizes.py:1512` | Vivas da disputa, na ordem de PK |
| 11 | `chamados/services/becos.py:87` | `perguntas` → `perguntas__decisoes` no evento |
| 23 | `folha/previa.py:64`, `:66` | Perguntas da prévia agrupadas por colaborador e dia |
| 25 | `chamados/services/declaracao_texto.py:475` | `do_fato` da projeção avulsa por marco |
| 26 | `chamados/services/declaracao_texto.py:588`, `:589` | `linhas_dos_fatos` do lote |
| 27 | `chamados/services/declaracao_texto.py:635` | `do_fato` das propostas, com o memo por fato |
| 29 | `chamados/juizes.py:1765` | Seleção de corrigíveis antes de formar os IDs |
| 30 | `chamados/juizes.py:1773` | Fila de respondidas a validar |
| 31 | `chamados/juizes.py:2435`, `:2437` | Perguntas usadas para extrair os dias vivos |
| 32 | `chamados/juizes.py:2490` | Perguntas da disputa aberta que pode travar o chamado |
| 41 | `chamados/models.py:1555` | Ressuscitação, lote já presente na base |
| 42 | `chamados/models.py:2077` | Segunda carga de `DisputaSupervisao.fechar`, lote já presente na base |
| 44 | `chamados/services/acoes_chamado.py:352` | Perguntas do detalhe do chamado |
| 45 | `chamados/services/competencia_trancada.py:119`, `:121` | Perguntas da competência |
| 46 | `chamados/services/competencia_trancada.py:165`, `:166` | Perguntas do chamado percorrido na medição |
| 47 | `chamados/services/disputa_emissao.py:2080` | Perguntas da disputa candidata no ramo sem credor |
| 48 | `chamados/services/fio.py:2296` | Perguntas do destino sob a trava |
| 49 | `chamados/services/fio.py:2300` | Perguntas da origem sob a trava, na ordem de PK |
| 50 | `chamados/services/pergunta_no_app.py:186` | Fatos de `_marco_ja_perguntado` |
| 53 | `chamados/views.py:1412` | Perguntas sem resposta da reabertura |
| 54 | `colaboradores/services/calendario.py:504`, `:505` | Respondidas na janela do calendário |
| 55 | `ponto/services/dia_do_colab.py:64`, `:65` | Perguntas do dia do colaborador |
| 61 | `chamados/services/explicador.py:160`, `:161` | Perguntas da disputa quando a coleção opcional está ausente |
| 62 | `api/foto_recebida.py:81` | Perguntas de foto, depois da validação |
| 63 | `chamados/management/commands/encerrar_perguntas_com_fato.py:84` | Perguntas da disputa, depois dos carimbos |
| 64 | `chamados/services/limbo_folgas.py:203`, `:205` | Perguntas do dia usadas pela guarda do filho |
| 65 | `ponto/management/commands/supra_juiz.py:192`, `:215` | `perguntas_que_resolvem` → `perguntas_que_resolvem__decisoes`, para a coleção de todos os chamados daquela consulta |
| 67 | `ponto/management/commands/reconciliar_grade.py:461` | Respondidas usadas na guarda de relavra |
| 68 | `chamados/services/disputa_emissao.py:1429`, `:1436` | Perguntas do motivo e dia entregues ao dedup |

**Contagem por item.** Restam **30 K**. Destes, **24** são cargas da C e **6** são classe R. No predesenho, a decomposição dos mesmos 30 era **25 + 5**. O aval 1830, item 6b-1, reclassificou K40, que relê `.get(pk=pk)` dentro do laço. As outras R são K3, releitura sob trava da candidata, K19, execução por pergunta, K20 e K58, portas por ID, e K22, busca da irmã de uma pergunta. A sobra do dedup fica fora dos 69. `chamados/management/commands/migrar_data_evento_dia_do_turno.py:464`, `:466`, `chamados/services/relavra_via.py:230`, `:236`, `chamados/services/materializacao.py:284`, `:2117`, `chamados/services/validacao.py:431`, `:677`, `chamados/management/commands/dedup_perguntas_fato.py:75`.

Os **24 arquivos de produção tocados** são os das duas tabelas, exceto `chamados/models.py`. Os **sete arquivos de teste tocados** são os três novos abaixo, o selo de chamada e os três harnesses da seção de adaptações. `chamados/tests/test_e4c_f5_selo_chamada.py:21`, `chamados/tests/test_e4c_f5_voltaB_lotes.py:24`, `chamados/tests/test_e4c_f5_voltaB_sitios.py:25`, `chamados/tests/e4c_f5_voltaB_mutantes.py:11`.

## Catraca da C

| Arquivo | Chamadas restantes |
|---|---|
| `chamados/management/commands/auditar_invariantes_chamados.py` | `:178` `_contraprova_i21a`, `:597` I2, `:1671` F1 |
| `chamados/management/commands/dedup_perguntas_fato.py` | `:76` `Command.handle` |
| `chamados/services/materializacao.py` | `:192` `par_do_intervalo`, `:259` `paridade_absolvida_bug_par_103`, `:290` `_situacao_da_irma`, `:885` `estado_aplicacao_pergunta`, `:1300` `_recusa_sem_anuencia` |
| `chamados/services/vigia_fio.py` | `:636` `censo_alcance` |

A permissão nominal do placar permanece em `chamados/services/raiox.py:147`, que recebe a tupla montada pelo `values_list` de `:135`. `chamados/tests/test_e4c_f5_selo_chamada.py:35`.

## REDs e mutantes

O RED 3 × 6 compara apenas SELECTs cujo primeiro `FROM` é `chamados_decisaopergunta`. O teste exige os PKs de todas as perguntas julgadas em ambas as medidas. No K47, a medição inclui o carimbo e o fecho reais e desfaz a savepoint para manter as três primeiras perguntas na rodada de seis. `chamados/tests/test_e4c_f5_opus_models.py:25`, `chamados/tests/test_e4c_f5_voltaB_lotes.py:40`, `:178`.

Os REDs de divergência cobrem os 14 sítios da primeira tabela. O GET usa o instante do ato para compor `respondidas`. O POST do app e o PWA verificam o estado anterior entregue ao escritor. A validação vê a reabertura do ato, a recusa cobre o carimbo ausente na projeção, e o M1 exerce a projeção temporal. A **GUARDA** das portas usa texto vazio na projeção. Esse caso já era recusado na base porque o juiz puro lia o texto vazio do ORM. `chamados/tests/test_e4c_f5_voltaB_sitios.py:64`, `:76`, `:92`, `:104`, `:110`, `:123`, `:135`, `:147`, `:157`, `:169`, `:196`, `:205`, `:219`, `:229`, `:243`, `chamados/juizes.py:1556`.

Comandos para o Code, a partir de `app/`. As classes e métodos estão nos dois arquivos novos e no selo. `chamados/tests/test_e4c_f5_voltaB_lotes.py:24`, `chamados/tests/test_e4c_f5_voltaB_sitios.py:25`, `:38`, `:234`, `chamados/tests/test_e4c_f5_selo_chamada.py:194`.

```bash
lotes=chamados.tests.test_e4c_f5_voltaB_lotes.CarregadoresVoltaBTest
sitios=chamados.tests.test_e4c_f5_voltaB_sitios

PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$lotes" "$sitios" \
  chamados.tests.test_e4c_f5_selo_chamada \
  chamados.tests.test_disputa_s127 \
  chamados.tests.test_e3_r3_c5_sem_banco \
  chamados.tests.test_e3_r3_c6_sem_banco
```

Cada comando abaixo ativa um único mutante. Para os 14 carregadores, a mordida esperada é `Q(6) > Q(3)` na asserção de igualdade. Para `api_p_cru`, a mordida é `api/views.py:2597` na catraca. A transformação AST da chamada crua foi conferida estaticamente. O resultado dos casos ORM está [nao verificado]. `chamados/tests/e4c_f5_voltaB_mutantes.py:11`, `:31`, `:63`, `chamados/tests/test_e4c_f5_voltaB_lotes.py:51`, `chamados/tests/test_e4c_f5_voltaB_sitios.py:26`.

```bash
mut_lote() {
  F5_VOLTA_B_MUTANTE="$1" PYTHONDONTWRITEBYTECODE=1 \
    python3 manage.py test "$lotes.$2"
}
```

| Mutante | Caso que deve cair e comando |
|---|---|
| K4 | `mut_lote k4 test_RED_k4_vivas_da_disputa` |
| K11 | `mut_lote k11 test_RED_k11_beco_do_evento` |
| K23 | `mut_lote k23 test_RED_k23_previa` |
| K26 | `mut_lote k26 test_RED_k26_declaracao_por_fato_em_lote` |
| K29 | `mut_lote k29 test_RED_k29_respostas_corrigiveis` |
| K30 | `mut_lote k30 test_RED_k30_fila_a_validar` |
| K31 | `mut_lote k31 test_RED_k31_dias_das_perguntas_vivas` |
| K32 | `mut_lote k32 test_RED_k32_disputa_viva_do_chamado` |
| K45 | `mut_lote k45 test_RED_k45_perguntas_da_competencia` |
| K46 | `mut_lote k46 test_RED_k46_perguntas_do_chamado_na_competencia` |
| K47 | `mut_lote k47 test_RED_k47_escolhas_sem_credor` |
| K54 | `mut_lote k54 test_RED_k54_calendario` |
| K55 | `mut_lote k55 test_RED_k55_dia_do_colab` |
| K68 | `mut_lote k68 test_RED_k68_dedup_da_emissao` |
| `api_p_cru` | `F5_VOLTA_B_MUTANTE=api_p_cru PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$sitios.SeloMutanteVoltaBTest.test_MORDE_get_do_app_com_pergunta_crua"` |

## Adaptações e tetos

| Arquivo | Classe | Antes → depois | Asserts |
|---|---|---|---:|
| `chamados/tests/test_e3_r3_c5_sem_banco.py:180` | ii | Stub sem `DecisaoPergunta` → compositor do helper existente, `chamados/tests/duble_pergunta.py:12` | 34 → 34 |
| `chamados/tests/test_e3_r3_c6_sem_banco.py:299` | ii | Stub sem `DecisaoPergunta` → mesmo helper | 52 → 52 |
| `chamados/tests/test_disputa_s127.py:23` | v | `.all()` devolvia `list` → `Linhas`, subclasse de lista com `prefetch_related` identidade, `chamados/tests/test_e4b_classe_e_memoria.py:165`, `:193` | 8 → 8 |
| `chamados/tests/test_e4c_f5_selo_chamada.py:21` | Catraca solicitada | 24 identidades → 10 | 12 → 12 |

**Chokepoint.** Nenhum sítio da B altera uma chamada a `registrar_batida`. Há alcance indireto pelo fecho S143 até K47, pela eventual reabertura do credor até K48/K49 e pelo repasse do emissor até K68. K11 entra pelo `on_commit` da pergunta. Em K47, uma disputa pode carregar o lote e depois sair pela guarda de destino vivo ou de lista sem abertas. O efeito nos quatro cenários é [nao verificado]. `ponto/registro_batida.py:170`, `ponto/signals.py:215`, `chamados/signals.py:117`, `:240`, `chamados/services/disputa_emissao.py:1878`, `:2080`, `:2086`, `chamados/models.py:1481`, `ponto/services/cartorio.py:1877`, `ponto/management/commands/detectar_ausencias.py:803`, `chamados/services/disputa_emissao.py:846`.

**Quebra fora das classes.** Os quatro casos do chokepoint já têm o excesso informado no aval de passagem, **236/146/300/221** contra **230/145/294/219**. A contribuição da B e outras quebras estão [nao verificado]. `ponto/tests/test_selo_chokepoint_escrita.py:196`, `:210`, `:228`, `:235`, `:435`, `:440`, `:470`, `:475`.

**Tetos a remedir para eventual queda.** O modal prod-like tem teto **2837** e alcança K27 nas propostas e K61 no fallback do explicador. A queda depende de quantas perguntas passam por essas cargas na fixture. O teto pareado S143/S128 é recalculado no próprio teste. A direção líquida e os valores estão [nao verificado]. Nenhum teto foi editado. `chamados/tests/test_selo_teto_modal.py:88`, `:184`, `chamados/services/fio.py:330`, `:351`, `chamados/tests/test_e4b_c4_orm.py:499`, `:507`.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  ponto.tests.test_selo_chokepoint_escrita.SeloChokepointEscritaTest.test_registrar_batida_com_1_furo_vivo_que_ela_fecha \
  ponto.tests.test_selo_chokepoint_escrita.SeloChokepointEscritaTest.test_registrar_batida_com_3_furos_vivos_que_ela_fecha \
  ponto.tests.test_selo_chokepoint_escrita.SeloChokepointEscritaTest.test_registrar_batida_com_1_furo_vivo_que_fica \
  ponto.tests.test_selo_chokepoint_escrita.SeloChokepointEscritaTest.test_registrar_batida_com_3_furos_vivos_que_ficam \
  chamados.tests.test_selo_teto_modal \
  chamados.tests.test_e4b_c4_orm \
  api.tests.test_f2_teto_consultas_app \
  chamados.tests.test_e2_tarefa_do_dia \
  escala.tests.test_f3_beco1_folga_na_lista
```

## Campos e releituras

| Campo | Quem escreve | Quem lê e qual significado |
|---|---|---|
| `validada_em` | Validação e devolução em `chamados/services/validacao.py:168`, `:173`, `:273`, `:279`, `:349`, `:354`. Nova resposta limpa a assinatura em `chamados/services/disputa_emissao.py:1494`. Carimbo e ressuscitação atualizam a projeção em `chamados/models.py:3352`, `:1721`. | C6 e temporal recebem validação vigente da sequência, com passivo incorporado por `leitura`, `chamados/models.py:4317`, `:4358`, `chamados/juizes.py:1408`, `escala/services/cadastro_realidade.py:502`. Guardas de projeção continuam lendo o carimbo gravado em `chamados/services/validacao.py:685`, `:897`, `chamados/services/declaracao_texto.py:228`, `chamados/services/disputa_emissao.py:2084`. Podem divergir. |
| `respondida_em` | Resposta humana, automática e trilha em `chamados/services/disputa_emissao.py:1528`, `:1532`, `:1683`, `:1686`, `:1705`, `:1708`. Reabertura e recusa limpam o par em `:1615`, `:1640`, `:1656`. Carimbo e ressuscitação também atualizam a projeção em `chamados/models.py:3349`, `:1722`. | Juízes da B usam o instante vigente do ato. A recusa usa esse instante na foto de auditoria. O payload do GET, a face SQL e a foto gravada pelo escritor de reabertura continuam lendo a coluna projetada. `chamados/models.py:4339`, `:4358`, `chamados/services/validacao.py:914`, `api/views.py:2583`, `chamados/juizes.py:1587`, `chamados/services/disputa_emissao.py:1640`. |
| `resposta_colab` | Escritores humano e automático em `chamados/services/disputa_emissao.py:1481`, `:1682`. Reabertura e ressuscitação limpam a projeção em `:1615`, `:1656`, `chamados/models.py:1722`. | Toda chamada convertida usa o texto corrente da projeção. `resposta_display` conserva sua leitura própria de exibição. `chamados/models.py:4359`, `:3837`, `api/views.py:2581`, `chamados/services/declaracao_texto.py:418`. |
| `materializada_em` | `_escrever_carimbo` abastece as portas de carimbo, identidade e descarimbo. A porta de lote limpa a projeção. `chamados/models.py:3314`, `:3346`, `:3401`, `:3424`, `:3508`. | Compositor e guardas escalares leem o carimbo projetado. O juiz temporal aplica o corte de `agora`. O carimbo é o estado registrado, e C6 julga a vitalidade pelo conjunto dos insumos. `chamados/models.py:4359`, `chamados/juizes.py:1394`, `:1400`, `:1474`, `chamados/services/disputa_emissao.py:2084`. |
| `via_resolucao` | Mesmas portas do carimbo e a troca pela via de fato no espelho em `chamados/models.py:3318`, `:3508`, `:3517`. | A origem é a mesma coluna projetada. No corte temporal, vias fora do fato conservam a identidade. `chamados/models.py:4359`, `chamados/juizes.py:1400`, `chamados/services/disputa_emissao.py:2095`. |
| `decisoes` | `DecisaoPergunta.registrar` insere pela FK e invalida `decisoes` na instância recebida por `pergunta=`. Escrita por `pergunta_id=` exige instância fresca para uma leitura posterior. `chamados/models.py:4188`, `:4362`, `:4373`, `chamados/services/disputa_emissao.py:1639`. | Os K adiantam a relação. `DecisaoPergunta.leitura` consome a sequência e incorpora o passivo antes de `composta` entregar os campos aos juízes. `chamados/models.py:4317`, `:4329`, `:4351`. |

**Depois de escrever, no mesmo request**

- **Validar.** A guarda usa a leitura anterior. A validação registra com `pergunta=perg`. Há releitura integral depois da unidade do par e releitura escalar de `validada_em` e `materializada_em` após materializar a disputa. A releitura escalar dispensa prefetch novo. `chamados/services/validacao.py:173`, `:646`, `:655`, `:682`, `:743`, `:786`, `:789`.
- **Recusar.** A foto usa a leitura anterior. A reabertura grava por queryset, e a porta usa a foto e os campos da trilha depois do ato. O RED relê a instância inteira antes de compor novamente, pois a instância do teste havia carregado a sequência anterior. `chamados/services/validacao.py:902`, `:913`, `:925`, `:933`, `chamados/tests/test_e4c_f5_voltaB_sitios.py:110`.
- **Responder via API e PWA.** As duas guardas são anteriores. O escritor grava na mesma instância e invalida o cache dessa instância. A releitura de exibição do escritor, quando há chamado, acessa a sequência atual. O retorno da API usa o instante atualizado em memória e o PWA retorna a confirmação. `api/views.py:2641`, `:2648`, `:2657`, `chamados/views.py:1275`, `:1278`, `:1282`, `chamados/services/disputa_emissao.py:1528`, `:1532`, `:1569`, `chamados/models.py:4373`.
- **Declarar e aplicar.** `declarar_hora_colab` pode reler integralmente depois da costura, antes de julgar a resposta. Depois da resposta, anexa a nota e devolve o PK. `aplicar_declaracao` passa o PK à validação, que faz um `get()` novo. Assim, a validação posterior lê a sequência atual pela própria instância. `chamados/services/declaracao_texto.py:373`, `:418`, `:420`, `:424`, `:688`, `:694`, `:697`, `chamados/services/validacao.py:677`.
- **Validar inline e regularizar.** As listas de `validar_inline` classificam antes das escritas e o laço passa cada PK à porta. A regularização delega ao materializador e usa o retorno dele. A releitura do beco após o resolver passa por `refresh_from_db()` antes do juiz de fecho, descartando a carga reversa anterior. `chamados/views_cobrar.py:170`, `:178`, `:200`, `:209`, `chamados/services/acoes_disputa.py:134`, `:142`, `chamados/services/becos.py:58`, `:59`.

LEI-AKITA — padrao=juiz unico puro + fonte unica (compositor) + fitness function. `chamados/juizes.py:1387`, `:1556`, `chamados/models.py:4351`, `chamados/tests/test_e4c_f5_selo_chamada.py:194`, `chamados/tests/test_e4c_f5_voltaB_lotes.py:40`.

## 19/11 23:39 · sessao 7f9b70778ff8 · CENSO-DAS-COPIAS-1911-2330 · leitura · origem=Code

# CENSO-DAS-COPIAS-1911-2330

**57 cópias no `fatos.json` · 9 POUSOU · 22 PELA-METADE · 26 MORTA.** A soma inclui duas classificações POUSOU com prova fraca, detalhadas no fim. O universo é o dos 57 registros raspados. A conferência direta da lista da VM e da amostra de cinco contra o Git ficou [nao verificado]. `bin/raias_estado.py:73–89,96–109,125–133`

As linhas da tabela seguem a ordem dos objetos do `fatos.json`. Cópia, branch, ahead e já no main vêm do objeto correspondente. **Já no main** reproduz `cherry_ja_no_main`, inclusive `0` quando a sonda não trouxe commits ou quando a cópia contém merges. As referências **vivo** usam a numeração resultante do `docs_vivos_delta.diff`. **Dono** indica quem responde pela trava ou pelo próximo ato registrado, com **Code** para a sessão técnica. `bin/raias_estado.py:101–106,125–130`

| copia | obra | branch | ahead | ja no main | ultima linha da obra no RELATO/RELATO-ARQUIVO | trava | dono | classe | motivo da classe |
|---|---|---|---:|---:|---|---|---|---|---|
| `wt-b2-r4b-002812` | `E4C-F1-B2a` | `raia/wt-b2-r4b-002812` | 1 | 1 | 18/11 07:22 — B2a entrou no ar em `4eb10f99f`. `RELATO.md:571` | ninguém | Code | POUSOU | Patch-id 1/1 e teste R4b de recusa presente na cópia local. `TICKETS.md:1326` `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:313` |
| `wt-bug-enc-pouso-223206` | `BUG-ENC-CELULAS-FORMA` | `raia/bug-enc-pouso-1115` | 1 | 0 | 15/11 23:0x — BUG-ENC entrou no ar em `7825fec5b`. `RELATO.md:1164` | ninguém | Code | POUSOU | O RELATO identifica `2b80dcdca` como a árvore certificada do pouso, com diferenças só em LEIS/TICKETS. `RELATO.md:1164` `TICKETS.md:1325` |
| `wt-catraca-dec-151606` | `DATA-EVENTO-CRU` | `raia-catraca-dec-151606` | 4 | 0 | 11/11 21:04 — Opus reprovou a catraca v4 com três bloqueios. `RELATO.md:2248` | aval | Ronald | PELA-METADE | Falta rearme e correção da cobertura de templates, filtros por `**kw` e retornos de lookup. `RELATO.md:2248` `BACKLOG.md:2214` |
| `wt-cirineu-c4-contador-085632` | `[nome]-NAO-DESFAZ-O-PAPEL` — c4 contador | `raia/cirineu-c4-contador-085632` | 9 | 0 | 18/11 10:3x — A revisão devolveu a unidade do contador P1. `RELATO.md:503` | aval | Ronald | MORTA | A rodada de `4b68ae82b` foi cancelada como superada, e os complementos mudaram o universo do contador. `RELATO.md:503` `RELATO.md:511–513` |
| `wt-codex-bo-disputa-sem-pergunta-183316` | `BO-DISPUTA-SEM-PERGUNTA` | `bodisp-c25-183316` | 30 | 0 | 14/11 19:4x — c25 foi guardada como insumo da F1 da RAIZ. `RELATO.md:1555–1556` | espera `RAIZ-DA-DISPUTA` | Ronald | MORTA | `BO-DISPUTA-DESENHO-RAIZ` encerrou as voltas nessa linhagem e destinou c25, matriz e laudo à RAIZ. `BACKLOG.md:774` `RELATO.md:1556` |
| `wt-codex-bo-disputa-sem-pergunta-221357` | `BO-DISPUTA-SEM-PERGUNTA` | `raia/bo-disputa-221357` | 24 | 0 | 14/11 19:4x — O aval guardou c25 e passou o desenho à RAIZ. `RELATO.md:1556` | espera `RAIZ-DA-DISPUTA` | Ronald | MORTA | Os commits listados até c21 reaparecem em c25, que virou insumo da RAIZ. O conteúdo dos 13 sujos ficou [nao verificado]. `BACKLOG.md:774` `RELATO.md:1560` | [nao verificado]
| `wt-codex-cadeia-20-passo-1-112113` | `CADEIA-20-PASSO-1` | `(detached)` | 16 | 0 | 19/11 23:25 — Ronald autorizou pouso após PRONTO e Opus, com Pauta DP para os quatro casos. `RELATO.md:2` (vivo) | ninguém | Code | PELA-METADE | Faltam a causa das mudanças na segunda passada e da célula de agosto, a verificação e o PRONTO. `RELATO.md:1–4,24` (vivo) | [nao verificado]
| `wt-codex-cirineu-nao-desfaz-o-papel-181941` | `[nome]-NAO-DESFAZ-O-PAPEL` | `(detached)` | 1 | 0 | 18/11 à noite — A pergunta D1 registra a cura no ar e sete casos residuais. `RELATO.md:329` | ninguém [nao verificado] | [nao verificado] | POUSOU | **Prova fraca** para a cópia. O nome aponta para a cura pousada em `94bb5c8f4`, e a guarda consta no código local. O patch da cópia ficou [nao verificado]. `RELATO.md:4000–4007` `app/escala/utils.py:502` |
| `wt-codex-e3-resto-r3-221106` | `E3-RESTO-R3` | `e3-resto-r3-c17-221106` | 1 | 1 | 16/11 01:36 — R3 c17 entrou no ar em `f8ee770a5`. `RELATO.md:1151` | ninguém | Code | POUSOU | Patch-id 1/1 do ajuste c18 nessa cópia. `RELATO.md:1166` `BACKLOG.md:770` |
| `wt-codex-e4c-f5-constroi-090617` | `E4C-F5-CONSTROI` | `(detached)` | 6 | 0 | 19/11 10:0x — A volta 2 mudou a fonte do texto e acrescentou o contador. `RELATO.md:141` | ninguém | Code | MORTA | A montagem `9fa5ad533` sucedeu a cópia das primeiras voltas e foi incorporada à construção da volta A. `RELATO.md:149,80` `RELATO.md:5` (vivo) |
| `wt-codex-e4c-f5-constroi-184647` | `E4C-F5-CONSTROI` | `e4c-f5-constroi-a-170736` | 19 | 0 | 19/11 23:25 — A foi aceita como passagem, B abriu e o teto ficou como condição de certificação. `RELATO.md:5` (vivo) | ninguém | Code | PELA-METADE | Faltam as voltas B e C e a certificação final dentro do teto. `RELATO.md:5,8` (vivo) |
| `wt-codex-justificativa-paga-o-periodo-220358` | `JUSTIFICATIVA-PAGA-O-PERIODO` | `(detached)` | 23 | 0 | 16/11 12:2x — A obra reabriu como lateral pelo censo. `RELATO.md:1079` | espera `COMPETENCIA-PAGA-UM-JUIZ` | Code | PELA-METADE | Falta atualizar a cura pelo censo e certificar o DIFF com a guarda da competência paga. `BACKLOG.md:2082–2083` `RELATO.md:1391,1399` |
| `wt-codex-lastro-faixa-sem-default-215253` | `LASTRO-FAIXA-SEM-DEFAULT` | `(detached)` | 16 | 0 | 12/11 16:4x — O aval encerrou c20 e preservou a cópia como especificação. `RELATO.md:2009` | ninguém | Code | MORTA | `LASTRO-REDESENHA-NA-FONTE` substituiu a série pela base `f3ebd821b`. Os dois sujos ficaram [nao verificado]. `RELATO.md:2009–2012` |
| `wt-codex-lastro-redesenho-164050` | `LASTRO-FAIXA-SEM-DEFAULT` | `raia/lastro-redesenho-164050` | 9 | 0 | 19/11 13:35 — O passo 3 ficou atrás do passo 1 no ar. `RELATO.md:67` | espera `CADEIA-20-PASSO-1` | Code | PELA-METADE | Falta refazer a base `f3ebd821b` após o passo 1 e provar o DIFF de frota. `RELATO.md:2012` `BACKLOG.md:1960,1967` |
| `wt-codex-pauta-sem-turno-prefixo-reservado-030404` | `PAUTA-SEM-TURNO-PREFIXO-RESERVADO` | `(detached)` | 5 | 0 | 10/11 09:37 — `TRES-ITENS` congelou a v5 e registrou os quatro furos de identidade. `RELATO.md:2589` | aval | Ronald | PELA-METADE | **Prova fraca** do vínculo da cópia com `78528a3ec`; falta corte para retomar o desenho e tratar os quatro furos. `BACKLOG.md:2266` `RELATO.md:2595` |
| `wt-codex-tipo-na-fonte-184856` | `TIPO-NA-FONTE` — item 3 | `raia/tipo-item3-v2` | 13 | 0 | 18/11 — A retomada confirmou zero chamadas do acessor no HEAD e marcou a v5c como consulta. `RELATO.md:356` | ninguém | Code | MORTA | `TIPO-ITEM3-DESENHO-1811-1820` cortou o tipo fechado e mandou nascer a implementação em cópia nova do HEAD. `BACKLOG.md:1959–1960` `RELATO.md:355–356` |
| `wt-codex-triagem-offline-red-headless-101137` | `TRIAGEM-OFFLINE-RED-HEADLESS` | `(detached)` | 1 | 0 | 12/11 10:39 — RED de offline e reload reproduzido nas duas cascas em `d680518b0`. `RELATO.md:2151` | aval | Code | PELA-METADE | Falta abrir a volta de cura do `sw.js` e provar verde e smoke nas duas cascas. `RELATO.md:2151` `BACKLOG.md:2800` |
| `wt-codex2raias-090624` | `CODEX-2-RAIAS-PORTAO-CPU` | `codex2raias-090624` | 2 | 0 | 09/11 16:0x — `SERIE-E-SO` congelou a cópia `e381017bf` com a matriz pronta. `RELATO.md:2676–2677` | aval | Ronald | PELA-METADE | Falta liberar e executar a volta 3 pelo contrato E1–E25, com selos, suite e Opus. `RELATO.md:2677,2687` `BACKLOG.md:2281` |
| `wt-comboio-1811-121757` | `POUSOS-COMBOIO-1811-1332` — montagem inicial | `raia/comboio-1811-121757` | 4 | 0 | 18/11 13:10 — A certificação passou às montagens A e B. `RELATO.md:441,449` | ninguém | Code | MORTA | `COMBOIO-DAS-SUITES-1811` substituiu a montagem conjunta por `81f678c7e` e `fa49abfb6`, com pousos por fatia. `RELATO.md:441,449–450` `BACKLOG.md:2386` |
| `wt-comboio-1911-004256` | `QUEIMA-CODEX-E-PISTA-1911-0045` — item A | `(detached)` | 3 | 0 | 19/11 00:4x — A montagem `477a78ea5` juntou F6F7, ESPURIA e F9. `RELATO.md:235` | ninguém | Code | MORTA | O trem usou o comboio B para F6F7/F9 e levou ESPURIA c11 pelo comboio F. `RELATO.md:145,162,168–169` `BACKLOG.md:2050` |
| `wt-comboio-1911-b-011045` | `TREM-DE-POUSO-1911` — vagões 1 e 3 | `(detached)` | 2 | 0 | 19/11 08:25 — F9 entrou no ar pela árvore do comboio B. `RELATO.md:168` | ninguém | Code | POUSOU | `154d30455` igual ao primeiro passo do B e `3308f7599` igual à árvore do passo F9. `RELATO.md:168–169` |
| `wt-comboio-b-1811-123606` | `POUSOS-COMBOIO-1811-1332` — comboio B | `raia/comboio-b-1811-123606` | 3 | 0 | 18/11 13:32 — O aval separou o pouso de Cirineu da volta de ESCALA. `RELATO.md:437,449` | ninguém | Code | MORTA | `fa49abfb6` tinha ESCALA c5 reprovada. Cirineu pousou em `94bb5c8f4` e ESCALA seguiu até o comboio E. `RELATO.md:437,160,4000–4007` |
| `wt-comboio-d-080440` | `COMBOIO-1911-D` | `raia/comboio-d-1911-080440` | 4 | 0 | 19/11 08:3x — O job de `2e027a794` foi cancelado como superado. `RELATO.md:164` | ninguém | Code | MORTA | `COMBOIO-1911-E` substituiu D para retirar a ancestralidade da ESPURIA reprovada. `RELATO.md:164,167` |
| `wt-comp-paga-101739` | `COMPETENCIA-PAGA-UM-JUIZ` | `(detached)` | 7 | 0 | 16/11 15:4x — O cadastro passou a cobrir também as competências anteriores a 09/2026. `RELATO.md:1012` | espera a espinha em `models.py` e `fio.py` | Code | PELA-METADE | Falta completar a migração dos leitores, incluir o cadastro cortado e medir a guarda na sombra. `RELATO.md:1286,1303` `BACKLOG.md:2082` | [nao verificado]
| `wt-contador-codex-090249` | `CONTADOR-CODEX-OCIOSO` | `contador-codex-1711` | 3 | 0 | 17/11 11:2x — Ronald estacionou o contador e manteve a fila de reserva. `RELATO.md:793` | aval | Ronald | PELA-METADE | Falta rearme do B3 e correção dos quatro achados de implementação da v3 guardada. `RELATO.md:793,811` |
| `wt-deploy-exclusao-061631` | `DEPLOY-EXCLUSAO-ENVELOPE` | `deploy-exclusao-061631` | 2 | 0 | 09/11 08:0x — A triagem v2 confirmou cinco procedentes e deixou a volta 3 à espera de vaga. `RELATO.md:2781` | aval | Code | PELA-METADE | Falta a volta 3 para corrigir trava, marcador, métricas e selos, seguida de verificação. `RELATO.md:2776,2781` `BACKLOG.md:2110` | [nao verificado]
| `wt-e3-mesclada-121238` | `E3-FORMA-SQL-DO-JUIZ` — árvore de suite da E3 | `(detached)` | 1 | 0 | 14/11 23:4x — O push da E3 certificada terminou em `4b75cba10`. `RELATO.md:1503` | ninguém | Code | MORTA | A árvore de teste `6c465cb80` era da volta 4. `E3-POUSA-O-VERDE` escolheu `1ee368261`, pousada em `90f3fa66b`. `RELATO.md:1915,1522` `PROMPTS.md:2224` |
| `wt-e4c-f2-103933` | `E4C-F2-RESSUSCITAR-ATO` — protótipo F1/F2 | `e4c-f2-103933` | 4 | 0 | 18/11 17:5x — A revisão retroativa aprovou F2 P2 `8ae23ad62`. `RELATO.md:360` | ninguém | Code | MORTA | A série inicial `8c6577245` foi substituída pela F2 com o corte da reabertura compensadora e o P2 certificado. `RELATO.md:1278,4032–4036` `BACKLOG.md:2052` |
| `wt-escala-c7-021338` | `ESCALA-NAO-DESCREVE` | `raia/escala-c7-021338` | 3 | 0 | 19/11 09:10 — ESCALA c7 entrou no ar pelo comboio E em `433e2e57e`. `RELATO.md:152,160` | ninguém | Code | MORTA | A ponta `2fd98211a` foi rejeitada por descender de `d223c17d5` e substituída pela montagem E. `RELATO.md:164` |
| `wt-eval-contador-112226` | `EVAL-CONTADOR-CASOS` | `raia/eval-contador-112226` | 3 | 0 | 18/11 13:12 — O novo aval respondeu a revisão do contador em `13f5c2913`. `RELATO.md:442` | aval | Code | PELA-METADE | Falta executar os casos a–c e provar a mordida do mutante de data futura na vaga de instrumento. `BACKLOG.md:2829` `RELATO.md:470` |
| `wt-f2-073854` | `E4C-F2-RESSUSCITAR-ATO` | `f2-construtor-073854` | 10 | 0 | 18/11 17:5x — A revisão retroativa aprovou o P2 pousado. `RELATO.md:360` | ninguém | Code | POUSOU | A entrega da reabertura compensadora foi incorporada pelo P2 `8ae23ad62`, com migração 0056 e lógica presente na cópia local. `RELATO.md:4032–4036` `app/chamados/models.py:1669–1681` `app/chamados/migrations/0056_decisao_resposta_mantem_validacao_historico.py:1–8` |
| `wt-f2-p2-152531` | `E4C-F2-RESSUSCITAR-ATO` — P2 | `f2-p2-contract-152531` | 12 | 0 | 18/11 17:5x — A revisão retroativa aprovou o P2 de `8ae23ad62`. `RELATO.md:360` | ninguém | Code | MORTA | A árvore `73dd28073` levava B2 sem cerca e foi substituída pelo P2 refeito sobre B2a. `RELATO.md:533,556,668,4032–4036` |
| `wt-f5m-130211` | `E4C-F5-CONSTROI` | `raia/f5-merge-130211` | 9 | 0 | 19/11 23:25 — O aval da passagem A identifica `3f1d63863` como a base já atualizada. `RELATO.md:5` (vivo) | ninguém | Code | MORTA | `9fa5ad533`, antes candidata a pouso, foi incorporada à construção sobre `eb1e49c1e`, com o desenho A/B/C. `RELATO.md:58,80` `RELATO.md:5,22` (vivo) |
| `wt-f5v4-112404` | `E4C-F5-CONSTROI` | `(detached)` | 8 | 0 | 19/11 13:20 — A suite de `6978b3234` ficou verde como pré-filtro. `RELATO.md:83` | ninguém | Code | MORTA | A certificação dessa árvore foi substituída pelo merge `9fa5ad533`, incorporado à construção A. `RELATO.md:80,83` `RELATO.md:5` (vivo) |
| `wt-f6f7-sobre-main-173528` | `F6F7-LEI3-PELA-LEITURA` | `(detached)` | 1 | 0 | 19/11 08:20 — F6F7 entrou no ar em `154d30455`. `RELATO.md:169` | ninguém | Code | MORTA | A cópia `0726b5c98` era da medida. A série de medidas usou pontas posteriores, e o pouso certificou `67b63e0de`. `RELATO.md:366,271,169` |
| `wt-f6f7c14-sobre-main-202317` | `F6F7-LEI3-PELA-LEITURA` | `(detached)` | 1 | 0 | 18/11 à noite — A verificação bloqueou c14 e cancelou sua rodada de DIFF. `RELATO.md:283` | ninguém | Code | MORTA | **Prova fraca** do vínculo da cópia destacada com a c14; a c15 substituiu a rodada c14. `RELATO.md:280–283` |
| `wt-f6f7c15-sobre-main-203238` | `F6F7-LEI3-PELA-LEITURA` | `(detached)` | 1 | 0 | 18/11 à noite — O selo do escritor bloqueou c15 e abriu c16. `RELATO.md:280` | ninguém | Code | MORTA | **Prova fraca** do vínculo com a montagem `5baeb46a5`; o DIFF da ponta c15 foi cancelado e refeito em c16. `RELATO.md:279–281` |
| `wt-f6f7c16-sobre-main-213517` | `F6F7-LEI3-PELA-LEITURA` | `(detached)` | 1 | 0 | 18/11 22:0x — c17/c18 corrigiram o lint da c16. `RELATO.md:270–271` | ninguém | Code | MORTA | **Prova fraca** do vínculo com a árvore da medida v5; `a555d027b` sucedeu a volta c16. `RELATO.md:256,270–271` |
| `wt-feriado-092818` | `FERIADO-ESCALA` — `HJ-074` | `raia/feriado-escala-092818` | 1 | 0 | 13/11 11:0x — O aval apontou nominalmente esta cópia como superada. `RELATO.md:1928` | ninguém | Code | MORTA | `FERIADO-USA-A-LEI-DE-1609` tomou o lugar de `4289eadaf`; o conteúdo dos nove sujos ficou [nao verificado]. `BACKLOG.md:729` `RELATO.md:1928–1931` |
| `wt-feriado-posto-135549` | `FERIADO-POR-POSTO` | `raia/feriado-posto-135549` | 6 | 0 | 19/11 17:2x — Item 4 v2t ficou verde; A5 e `sombra_c2b=0` seguem pendentes. `RELATO.md:30` (vivo) | aval | Ronald | PELA-METADE | Faltam a resposta da prévia A5, o zero de C2 na sombra e a certificação final. `RELATO.md:30,35,48` (vivo) | [nao verificado]
| `wt-feriado1609-110424` | `FERIADO-USA-A-LEI-DE-1609` | `raia/wt-feriado1609-110424` | 4 | 0 | 15/11 à noite — A medida G parou no item 3(ii) por alcance em dias entregues. `RELATO.md:1196–1198` | ! | Ronald | PELA-METADE | Falta o corte sobre os dias entregues e a prova de G com referência e lote definidos. `RELATO.md:1196–1199` `BACKLOG.md:801` `PROMPTS.md:2216` |
| `wt-frac5-cura-100047` | `RC1-3-FRACAO-14` — FRAC5 | `(detached)` | 4 | 0 | 14/11 — FRAC5-TEXTO entrou no ar em `14b61c124`. `RELATO.md:1608` | ninguém | Code | MORTA | O aval `FRAC5-SO-O-QUE-O-CORTE-MANDOU` reduziu o pouso ao texto e guardou o mínimo de cinco dias em outra ref. `RELATO.md:1626,1657` `BACKLOG.md:760` |
| `wt-instr-travas-002004` | `CORTE-DAS-TRAVAS` | `(detached)` | 22 | 0 | 14/11 08:12 — O rearme foi consumido e o gate por classe saiu do escopo. `RELATO.md:1686` | ninguém | Code | PELA-METADE | Falta verificar e certificar os itens 1 e 5 no escopo reduzido, com o gate retirado. `RELATO.md:1688–1694,1715` `PROMPTS.md:2259` |
| `wt-lote7-ausencia-100250` | `POUSOS-CONDICIONAIS-1611` — LOTE7 | `(detached)` | 5 | 0 | 16/11 13:17 — O smoke fechou a condição do LOTE7 c7 no ar. `RELATO.md:1070` | ninguém | Code | POUSOU | A entrega `6cc3d6f22` é identificada na cópia, e o acordador está no código local após o pouso `42fd467a0`. `RELATO.md:1368,1072` `app/ponto/services/ausencia.py:154` |
| `wt-o10d-023001` | `O10-D-PERMUTA-RECIPROCA` | `o10d-023001` | 5 | 0 | 17/11 08:2x — Opus aprovou `bdbff7c4a`, com corte pendente para o pouso. `RELATO.md:886` | ! | Ronald | PELA-METADE | Falta resolver as condições de carta e tipo, `d1=d2`, recusas e o `!` de escala. `RELATO.md:886` `BACKLOG.md:2294` |
| `wt-par-rebase-060915` | `PAR-NAO-ADJACENTE` | `par-rebase-060915` | 5 | 0 | 09/11 07:3x — A v4 contrariava a regra do par externo. `RELATO.md:2790` | ninguém | Code | MORTA | `LEI-PAR-ANINHADO` superou `84a400e1` e exigiu v5 com o par externo preservado. `RELATO.md:2790` `BACKLOG.md:1313` |
| `wt-porta-pouso-185744` | ? | `(detached)` | 1 | 0 | 17/11 19:46 — A candidata PORTA-EXPORT v5 entrou no ar em `ba4cb6c3c`. `RELATO.md:476,651` | [nao verificado] | [nao verificado] | POUSOU | **Prova fraca**. Nome e horário são compatíveis com a certificação da v5. O vínculo com a árvore `91816030` ficou [nao verificado]. `RELATO.md:651` |
| `wt-pouso-ausencia-215637` | `AUSENCIA-EFEITO-PELO-ESTADO` | `(detached)` | 1 | 0 | 17/11 07:4x — O smoke fechou a obra. `RELATO.md:876` | ninguém | Code | MORTA | A primeira certificação teve dois vermelhos e foi substituída pela cópia `230733` após `44e0be955`. `RELATO.md:928,932,939` |
| `wt-pouso-ausencia-230733` | `AUSENCIA-EFEITO-PELO-ESTADO` | `(detached)` | 1 | 0 | 17/11 07:4x — O smoke fechou a obra. `RELATO.md:876` | ninguém | Code | POUSOU | A cópia nova foi certificada, e o pouso `6ce96a55e` bateu com os cinco arquivos da árvore de pouso. `RELATO.md:921–923,928` `BACKLOG.md:835` |
| `wt-quarentena-svv-151703` | `QUARENTENA-REABRE-COM-CENSO` | `raia/quarentena-svv-151703` | 1 | 0 | 12/11 15:3x — `E3-NA-VAGA` interrompeu o WIP com o RED ainda por rodar. `RELATO.md:2028` | aval | Code | PELA-METADE | Falta retomar com matriz da porta, executar o RED e provar o DIFF de frota. `BACKLOG.md:2775` `RELATO.md:2028` |
| `wt-relavra11-113556` | `RELAVRA-11-DOS-50` | `(detached)` | 9 | 0 | 15/11 12:4x — A resposta definiu que a reversão repõe os dados e os carimbos acompanham a escrita. `RELATO.md:1249–1250` | espera `COMPETENCIA-PAGA-UM-JUIZ` | Code | PELA-METADE | Falta empilhar sobre COMP-PAGA e provar foto, reversão e DIFF dos 47 na sombra. `RELATO.md:1324` `BACKLOG.md:2019` |
| `wt-saida-noturna-135512` | `SAIDA-NOTURNA-NO-DIA-CERTO` | `raia/saida-noturna-135512` | 1 | 0 | 12/11 15:3x — O aval deixou D2 seguir e isolou as decisões de D1/D3. `RELATO.md:2027` | aval | Ronald | PELA-METADE | Falta fechar a fonte e o custo do texto em D1 e a via de desfazer o carimbo em D3. `RELATO.md:2027,2082` `BACKLOG.md:2151` |
| `wt-selo-espera-084620` | `SELO-ESPERA` | `selo-espera-cego-fixture` | 1 | 0 | 11/11 09:33 — O aval estacionou a v1 sem abrir v2. `RELATO.md:2395` | aval | Ronald | PELA-METADE | Falta aval para a v2 e correção da leitura da raiz e da falha silenciosa do censo. `BACKLOG.md:2764,2766` |
| `wt-selos-canon-i3-113049` | `SELOS-CANON-I3` | `(detached)` | 3 | 0 | 09/11 16:0x — A v3 `4561d76e6` ficou congelada antes da matriz. `RELATO.md:2676,2678` | aval | Ronald | PELA-METADE | Faltam matriz, volta 4, suite, preverificação e Opus final. `RELATO.md:2678,2696` `BACKLOG.md:804` |
| `wt-silencio-2810-0915` | `QUARENTENA-DO-COLAB` / `SILENCIO-VENCIDO-VOLTA` | `silencio-vencido-2810` | 11 | 0 | 12/11 14:03 — `QUARENTENA-REABRE` usou o diff antigo como especificação após a reprovação. `RELATO.md:2089` | ninguém | Code | MORTA | O aval `QUARENTENA-REABRE-COM-CENSO` substituiu a cópia antiga por nova base e pelo WIP `c9a2a3846`. `BACKLOG.md:2775` `RELATO.md:2083,4687` |
| `wt-sonda-sem-grant-075803` | `SONDA-SEM-GRANT` | `(detached)` | 8 | 0 | 09/11 16:0x — A cópia `c5e7df309` ficou congelada c

[truncado em 20000 de 24756 caracteres]

## 19/11 23:30 · sessao 6a3bbfd9c659 · E4C-F5-CONSTROI · leitura · origem=Code

**A sonda atribui +6, +1, +6 e +2 SELECTs a sete sítios de leitura. Nenhuma das emissões listadas é ESCRITOR NOVO.** O `blame` atribui os prefetches à F5 volta A/A2 e a chamada avulsa à F5 A1. A volta 2 introduziu a leitura dos instantes pela sequência, que entrou na cópia sobre `main` pelo merge `3f1d63863`. O portão vi1 alterou testes. `atribuicao.txt:1`, `atribuicao.txt:12`, `atribuicao.txt:15`, `atribuicao.txt:26`, `commits.txt:1`, `commits.txt:2`, `commits.txt:7`, `commits.txt:8`, `commits.txt:9`, `commits.txt:10`, `app/chamados/models.py:4330`, `app/chamados/models.py:4357`.

Na tabela, as linhas de código são de `ec7134368`, salvo onde aparece `main`. A numeração de `atribuicao.txt` começa no primeiro cabeçalho de cenário. A sonda traz o prefixo do SQL e a pilha. A igualdade dos IDs entre consultas diferentes fica **[nao verificado]**.

| Cenário e linha da sonda | Emissão e chamadores em `ec7134368` | Commit da emissão | Classe e unidade de custo | O que o `main` lia e onde o lote pode ser reaproveitado |
|---|---|---|---|---|
| **1 fecha, +1** · `atribuicao.txt:2` | **A.** `prefetch_related('decisoes')` em [app/chamados/reconciliador.py:703](/home/ronald/codex_ws/ec71343686835c2e7460b5e25d4a7f3f886e0432/app/chamados/reconciliador.py:703), avaliado no `for` de `um_colaborador_por_identidade` (`app/chamados/juizes.py:1188`). Chamadores em `app/chamados/reconciliador.py:706` e `:635`. | `eaf69fe33`, **F5 A2**, lote K43. A1 liga o C6 ao compositor em `app/chamados/juizes.py:1408`. `commits.txt:2`, `commits.txt:7` | **LEITURA**, um lote de perguntas absolvíveis por chamada, +1 com 1 ou 3 fechamentos. `app/chamados/reconciliador.py:678`, `:702`, `atribuicao.txt:16` | **Substitui colunas** de instante e validação no C6. O cartório já tem decisões das perguntas com destino explícito em chamados de batida. O lote do E2 também pode conter órfãs e perguntas fora desses módulos. Reuso possível para a interseção com dados ainda atuais. Cobertura integral da emissão **[nao verificado]**. `main app/chamados/juizes.py:1389`, `app/chamados/reconciliador.py:678`, `:713`, `app/ponto/services/cartorio.py:145`, `:1026`, `app/chamados/models.py:3660` |
| **1 fecha, +1** · `atribuicao.txt:4` | **B.** `list(...prefetch_related('decisoes'))` de `_ancoradas` em [app/chamados/models.py:366](/home/ronald/codex_ws/ec71343686835c2e7460b5e25d4a7f3f886e0432/app/chamados/models.py:366). Chamadores `_plano_pergunta_viva` em `:400` e `_gate_pergunta_viva` em `:438`. | `18bf86f01`, **F5 volta A**, prefetch de `_ancoradas`. `commits.txt:8`, `app/chamados/models.py:366` | **LEITURA**, um lote por chamada do gate com ancoradas, +1 nos dois cenários de fecho. Pode crescer com o número de chamados que chegam ao gate com lote preenchido. `app/chamados/models.py:364`, `:400`, `:438`, `atribuicao.txt:18` | **Substitui colunas no C6**. O filtro de validação no `main` já consultava a sequência via SQL. O prefetch do cartório cobre destinos explícitos e o do E2 cobre as absolvíveis. Nenhum garante toda a âncora, que inclui órfãs pelo pai. O lote do próprio gate já chega a `_gravar_estado`. Eliminação integral deste SELECT por reuso **[nao verificado]**. `main app/chamados/models.py:351`, `:357`, `main app/chamados/juizes.py:1389`, `app/chamados/models.py:228`, `:361`, `:387`, `:401`, `app/chamados/reconciliador.py:703`, `app/ponto/services/cartorio.py:146` |
| **1 fecha, +1** · `atribuicao.txt:6` | **C.** Mesmo prefetch de `_ancoradas`, `app/chamados/models.py:366`. Chamador S143 em [app/chamados/services/disputa_emissao.py:1985](/home/ronald/codex_ws/ec71343686835c2e7460b5e25d4a7f3f886e0432/app/chamados/services/disputa_emissao.py:1985), sob o `lambda` de `:1920`. | `18bf86f01`, **F5 volta A**, prefetch compartilhado com B. A chamada S143 já estava na base. `commits.txt:8`, `main app/chamados/services/disputa_emissao.py:1983` | **LEITURA**, um lote de âncoras por chamada do resolver com resultado, +1 nos dois fechos. Pode crescer com o número de chamadas do S143 que encontram âncoras. `app/chamados/services/disputa_emissao.py:1985`, `atribuicao.txt:20` | **Substitui colunas no C6**, com sobreposição possível com B. O gate carrega em `app/chamados/models.py:400` e o cache é consumido em `:228`. Entre gate e S143 pode haver reaponte e gravação. O lote S128 de `app/chamados/services/disputa_emissao.py:1951` tem outro recorte e é avaliado em `:1986`, depois das âncoras. Reuso exige confirmar cobertura e frescor. Duplicação integral **[nao verificado]**. `main app/chamados/juizes.py:1389`, `app/chamados/models.py:456`, `:279`, `app/chamados/signals.py:117`, `app/chamados/services/disputa_emissao.py:1973` |
| **1 fecha, +1** · `atribuicao.txt:8` | **D.** `list(self.perguntas.all().prefetch_related('decisoes'))` de `DisputaSupervisao.fechar` em [app/chamados/models.py:2077](/home/ronald/codex_ws/ec71343686835c2e7460b5e25d4a7f3f886e0432/app/chamados/models.py:2077). Chamadores `app/chamados/reconciliador.py:663` e `app/ponto/services/cartorio.py:1026`. | `18bf86f01`, **F5 volta A**. O primeiro prefetch da sequência já existia na **F2**, `41620774bb`. `commits.txt:8`, `app/chamados/models.py:2077`, `main app/chamados/models.py:2001` | **LEITURA**, um lote por disputa que chega a esse trecho de `fechar`, +1 nos dois fechos. `app/chamados/models.py:2077`, `atribuicao.txt:22` | **Duplicada e evitável.** O `main` já carregava decisões para `respostas_sem_veredito()` e lia colunas no C6. Na cópia, `fechar()` chama `respostas_sem_veredito()` em `:2068`, que carrega a mesma coleção em `:1998` e `:2020`. As instâncias desse lote ficam no corpo da primeira leitura. Entregá-las ao C6 evita o segundo prefetch e mantém o compositor. `main app/chamados/models.py:2001`, `:2048`, `:2057`, `main app/chamados/juizes.py:1389`, `app/chamados/models.py:2018`, `:2023`, `:2077`, `:2078` |
| **1 fecha, +2** · `atribuicao.txt:10` | **E.** Prefetch aninhado `perguntas_que_resolvem__decisoes` em [app/ponto/services/cartorio.py:146](/home/ronald/codex_ws/ec71343686835c2e7460b5e25d4a7f3f886e0432/app/ponto/services/cartorio.py:146). Chamador `_julgar_colab_corpo` em `:1347`. `julgar_colab` chama o corpo em `:1015`, `:1048` e, condicionalmente, `:1113`. O par exato da sonda **[nao verificado]**. | `eaf69fe33`, **F5 A2**, lote K24. `commits.txt:2`, `app/ponto/services/cartorio.py:146` | **LEITURA**, um lote por execução do corpo que chega à carga e tem perguntas, duas emissões nos dois cenários de fecho. `app/ponto/services/cartorio.py:145`, `:1347`, `atribuicao.txt:24` | **Substitui colunas**. O `main` prefetchava as perguntas e o C6 lia seus campos. Na cópia, o primeiro lote serve aos leitores do corpo. O segundo usa o mesmo recorte por colaborador e módulos, mas a carga é local a cada passada e há carimbo/fecho entre passadas. Reuso do primeiro lote só para membros e decisões ainda atuais. Duplicação integral **[nao verificado]**. `main app/ponto/services/cartorio.py:147`, `main app/chamados/juizes.py:1389`, `app/ponto/services/cartorio.py:1026`, `:1033`, `:1048`, `:1347`, `:1417`, `:1689` |
| **1 FICA, +1** · `atribuicao.txt:13` | **F.** `list(do_fato(...).select_related(...).prefetch_related('decisoes'))` em [app/chamados/services/fio.py:1964](/home/ronald/codex_ws/ec71343686835c2e7460b5e25d4a7f3f886e0432/app/chamados/services/fio.py:1964). Chamadores `_nascer` em `app/chamados/models.py:3119` e `_sob_contrato` em `:3027`. | `eaf69fe33`, **F5 A2**, lote K17. A1 converte a eleição da canônica em `app/chamados/services/fio.py:1288`. `commits.txt:2`, `commits.txt:7` | **LEITURA**, um lote por chamada de `universo_do_fato` com candidatas, +1 com 1 ou 3 chamados que ficam. `app/chamados/services/fio.py:1963`, `atribuicao.txt:29` | **Substitui a coluna `respondida_em`** na eleição da canônica. O primeiro lote para esse recorte é carregado aqui e semeia `_viz`. A ponte anterior pode ter carregado vizinhas em `:1595` e guardado em `:1848`. Reuso sem nova derivação depende da mesma chave do `do_fato`. Lote anterior equivalente nesta execução **[nao verificado]**. `main app/chamados/services/fio.py:1283`, `:1958`, `app/chamados/services/fio.py:1288`, `:1811`, `:1968`, `:2001`, `app/chamados/models.py:3087` |
| **3 fecham, +1** · `atribuicao.txt:16` | **A**, mesmos sítio e chamadores. | `eaf69fe33`, **F5 A2**, como A. | **LEITURA**, +1 por lote, como A. | Mesma substituição de colunas e mesmo limite de cobertura de A. |
| **3 fecham, +1** · `atribuicao.txt:18` | **B**, mesmos sítio e chamadores. | `18bf86f01`, **F5 A**, como B. | **LEITURA**, +1 por lote do gate, como B. | Mesma substituição de colunas e mesmo limite de cobertura de B. |
| **3 fecham, +1** · `atribuicao.txt:20` | **C**, mesmos sítio e chamadores. | `18bf86f01`, **F5 A**, como C. | **LEITURA**, +1 por lote do resolver, como C. | Mesma substituição de colunas e mesma necessidade de confirmar frescor de C. |
| **3 fecham, +1** · `atribuicao.txt:22` | **D**, mesmos sítio e chamadores. | `18bf86f01`, **F5 A**, como D. | **LEITURA**, +1 por lote da disputa, como D. | Mesma duplicação evitável de D. `app/chamados/models.py:2020`, `:2068`, `:2077` |
| **3 fecham, +2** · `atribuicao.txt:24` | **E**, mesmos sítio e chamadores. | `eaf69fe33`, **F5 A2**, como E. | **LEITURA**, +2 por passadas do corpo, como E. | Mesma substituição de colunas e mesma condição de frescor de E. |
| **3 FICAM, +1** · `atribuicao.txt:27` | **G.** Avaliação de `list(p.decisoes.all())` em [app/chamados/models.py:4330](/home/ronald/codex_ws/ec71343686835c2e7460b5e25d4a7f3f886e0432/app/chamados/models.py:4330). Chamadores `composta` em `:4357` e `lavrar_sem_turno_aberto` em `app/chamados/reconciliador.py:367`. | `0db6afc73`, **F5 A1**, chamada corrente de `lavrar_sem_turno_aberto`. `e673e0efe`, **F5 volta 2**, introduziu a leitura no juiz, incorporada pelo merge `3f1d63863`. `commits.txt:7`, `:9`, `:10`, `app/chamados/reconciliador.py:367` | **LEITURA avulsa**, uma por pergunta persistida que chega sem cache. Nesta pilha, zero com 1 chamado que fica e +1 com 3. O código permite crescer por pergunta sem cache. `app/chamados/models.py:4326`, `:4330`, `atribuicao.txt:12`, `:27` | **Substitui a coluna `respondida_em`**. A varredura de órfãs já entrega o prefetch em `app/chamados/reconciliador.py:1575`, `:1580`. A emissão também pode chamar a porta com a pergunta recém-criada, em `app/chamados/services/disputa_emissao.py:468`, `:473`. Nesse ramo, as linhas reais retornadas por `registrar` já estão disponíveis no nascimento e o retorno é descartado. Repassá-las evitaria a leitura sem derivação paralela. O anexo termina em `lavrar_sem_turno_aberto`, então o ramo exato **[nao verificado]**. `main app/chamados/reconciliador.py:366`, `main app/chamados/juizes.py:1552`, `app/chamados/reconciliador.py:1673`, `app/chamados/models.py:3231`, `:4366`, `:4381` |
| **3 FICAM, +1** · `atribuicao.txt:29` | **F**, mesmos sítio e chamadores. | `eaf69fe33`, **F5 A2**, como F. | **LEITURA**, +1 por lote de candidatas, como F. | Mesma substituição de coluna e mesma condição de chave de F. |

| Cenário | Soma das linhas | Medição conferida |
|---|---:|---:|
| 1 chamado fecha | `1 + 1 + 1 + 1 + 2 = 6` | `236 − 230 = 6` · `atribuicao.txt:1` |
| 1 chamado FICA | `1 = 1` | `146 − 145 = 1` · `atribuicao.txt:12` |
| 3 chamados fecham | `1 + 1 + 1 + 1 + 2 = 6` | `300 − 294 = 6` · `atribuicao.txt:15` |
| 3 chamados FICAM | `1 + 1 = 2` | `221 − 219 = 2` · `atribuicao.txt:26` |

O `+7/+2/+7/+3` do item 2 do aval excede em uma consulta por cenário tanto a soma das linhas da sonda quanto a diferença medida. A origem desse descompasso **[nao verificado]**. `f5_chokepoint_passagem_1911_2325.txt:3`, `atribuicao.txt:1`, `:12`, `:15`, `:26`.

## 19/11 21:12 · sessao 6d7b3f3fd0af · E4C-F5-CONSTROI · construtor · origem=Code

A A2 acrescenta lote a **nove carregadores**. O K15 já vinha em lote, e o O261 conserva a releitura por candidata sob trava. Os dois testes de igualdade de consultas permaneceram intactos. A sintaxe dos dez arquivos e dos nove mutantes foi conferida por AST, e a contagem de `assert` e `self.assert*` ficou igual à base em todos os módulos existentes tocados. Banco, suíte e morte dos mutantes ficam para o Code e o Opus final [nao verificado]. `chamados/models.py:364`, `chamados/services/relavra_via.py:230`, `chamados/tests/test_e2_tarefa_do_dia.py:938`, `escala/tests/test_f3_beco1_folga_na_lista.py:402`.

### Carregadores e arquivos tocados

| K | Base | A2 |
|---|---|---|
| 1 | Mapa temporal em lote, instâncias sem lote. `chamados/services/tarefa_do_dia.py:390`, `:530` | `Prefetch` nas instâncias, com a junção de `absorcao_ausencia` reaproveitada no mapa temporal. `chamados/services/tarefa_do_dia.py:537`, `:545` |
| 2 | Sem lote. `chamados/juizes.py:1662` | `alimentar_portas` entrega as instâncias com decisões. `chamados/juizes.py:1663` |
| 9 | Sem lote. `chamados/reconciliador.py:1574` | Prefetch na consulta que materializa as reatáveis. `chamados/reconciliador.py:1575` |
| 10 | Sem lote. `chamados/reconciliador.py:1102` | Prefetch nas órfãs abertas. As duas varreduras com `iterator` passam blocos explícitos de 2.000. `chamados/reconciliador.py:1103`, `:1245`, `:1356` |
| 15 | Já em lote. `chamados/models.py:366` | Mesmo carregador. O selo mede **2 consultas com 1 e com 5 ancoradas**, conferindo os PKs e a forma do SQL. `chamados/tests/test_e4c_f2_leitura_do_ato.py:578`, `:584`, `:589` |
| 16 | Sem lote. `chamados/services/disputa_emissao.py:1955` | Prefetch nas pendentes do S128. `chamados/services/disputa_emissao.py:1955` |
| 17 | Sem lote. `chamados/services/fio.py:1964` | Prefetch nas sementes do universo do fato. `chamados/services/fio.py:1964` |
| 18 | Sem lote. `chamados/services/fio.py:1597` | Prefetch nos vizinhos do fato. `chamados/services/fio.py:1597` |
| 24 | Prefetch só de `perguntas_que_resolvem`. `ponto/services/cartorio.py:147` | Prefetch de `perguntas_que_resolvem__decisoes`. `ponto/services/cartorio.py:146` |
| 43 | Sem lote. `chamados/reconciliador.py:702` | Prefetch na consulta que materializa as absolvíveis. `chamados/reconciliador.py:703` |

Além dos seis módulos de produção da tabela e do selo de ancoradas, foram tocados `chamados/tests/test_o261_julgamento_desfaz_carimbo.py:187`, `chamados/tests/test_e4c_f5_voltaA_lotes.py:22` e `chamados/tests/e4c_f5_voltaA_mutantes.py:9`. O selo O261 compara `6 × Q(3) == 3 × Q(6)` e limita a **9 consultas por candidata**. O caso novo mede separadamente o SELECT de decisões por candidata e confere os PKs julgados. `chamados/tests/test_o261_julgamento_desfaz_carimbo.py:203`, `chamados/tests/test_e4c_f5_voltaA_lotes.py:162`.

### Origem das consultas extras

- **Tarefa, 36 × 30.** A fixture acrescenta duas perguntas. A leitura do código aponta três SELECTs avulsos por pergunta no percurso medido: partição do app, preparação do slot e entrada do C6. Os três chegam a `DecisaoPergunta.leitura` pelas instâncias de K1. O mapa temporal já usava lote, mas guardava as leituras separadamente. Agora a mesma carga abastece o mapa e o cache das instâncias. A atribuição dos seis SELECTs é por código e pelo total raspado, sem captura individual [nao verificado]. `chamados/tests/test_e2_tarefa_do_dia.py:921`, `:932`, `chamados/services/pergunta_no_app.py:88`, `chamados/services/carga_pergunta_viva.py:119`, `chamados/juizes.py:351`, `:1408`, `chamados/models.py:4330`, `chamados/services/tarefa_do_dia.py:537`, `:545`.
- **C11, 56 × 48.** A fixture acrescenta quatro dias com uma pergunta respondida por dia. Cada dia passa pelo C6 com a instância de K2 e pela partição com outra instância, de K1. O `all` de M1 ainda recebe o ORM e lê seus atributos diretamente. Os dois lotes cobrem as duas origens. A atribuição dos oito SELECTs é por código e pelo total raspado, sem captura individual [nao verificado]. `escala/tests/test_f3_beco1_folga_na_lista.py:104`, `:395`, `escala/services/cadastro_realidade.py:460`, `:466`, `:472`, `:502`, `chamados/juizes.py:1391`, `:1663`, `:1667`, `chamados/services/tarefa_do_dia.py:537`, `:626`.

### Testes e mutantes [nao verificado]

O RED compara apenas SELECTs cuja tabela principal é `chamados_decisaopergunta` e exige igualdade exata entre os PKs da fixture e os recebidos pelo compositor. No cartório, a validação de todas as perguntas força o `any` a percorrer a coleção inteira. `chamados/tests/test_e4c_f5_opus_models.py:25`, `chamados/tests/test_e4c_f5_voltaA_lotes.py:22`, `:28`, `:137`.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  chamados.tests.test_e4c_f5_voltaA_lotes \
  chamados.tests.test_e2_tarefa_do_dia.SeloDeterminismoERelogio.test_lote_tem_queries_constantes \
  escala.tests.test_f3_beco1_folga_na_lista.F3Beco1NaListaTest.test_MORDE_F5_queries_nao_crescem_com_os_dias \
  chamados.tests.test_o261_julgamento_desfaz_carimbo.JulgamentoDesfazCarimboSemLastroTest.test_custo_por_candidata_e_CONSTANTE \
  chamados.tests.test_e4c_f2_leitura_do_ato.AncoradasCustoTest \
  chamados.tests.test_e4c_f5_volta2_juizes.JuizesVolta2Test.test_red_8_custo_montar_chamados_fio_uma_e_cinco

PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  chamados escala.tests.test_f3_beco1_folga_na_lista \
  ponto.tests.test_selo_chokepoint_escrita
```

Cada invocação abaixo retira um único lote em memória. A morte esperada é a desigualdade entre 3 e 6 em `chamados/tests/test_e4c_f5_voltaA_lotes.py:37` [nao verificado]. `chamados/tests/e4c_f5_voltaA_mutantes.py:22`, `:51`.

```bash
muta() {
  F5_VOLTA_A_MUTANTE="$1" PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
    "chamados.tests.test_e4c_f5_voltaA_lotes.$2"
}
muta k1  TarefaEmLoteTest.test_RED_k1_tarefa
muta k2  CarregadoresEmLoteTest.test_RED_k2_portas
muta k9  CarregadoresEmLoteTest.test_RED_k9_reataveis
muta k10 CarregadoresEmLoteTest.test_RED_k10_orfas_abertas
muta k16 CarregadoresEmLoteTest.test_RED_k16_pendentes_s128
muta k17 CarregadoresEmLoteTest.test_RED_k17_fio_universo_do_fato
muta k18 CarregadoresEmLoteTest.test_RED_k18_fio_vizinhos
muta k24 CarregadoresEmLoteTest.test_RED_k24_cartorio
muta k43 CarregadoresEmLoteTest.test_RED_k43_absolviveis
```

| Mutante | Caso que deve morrer |
|---|---|
| K1, K2 | Tarefa `chamados/tests/test_e4c_f5_voltaA_lotes.py:42` e portas `:85`, respectivamente |
| K9, K10 | Reatáveis `chamados/tests/test_e4c_f5_voltaA_lotes.py:94` e órfãs abertas `:103`, respectivamente |
| K16 | Pendentes S128. `chamados/tests/test_e4c_f5_voltaA_lotes.py:110` |
| K17, K18 | Universo `chamados/tests/test_e4c_f5_voltaA_lotes.py:121` e vizinhos `:129`, respectivamente |
| K24, K43 | Cartório `chamados/tests/test_e4c_f5_voltaA_lotes.py:137` e absolvíveis `:151`, respectivamente |

As contraprovas da A1 para `isinstance`, argumento ORM cru no C6 e composição manual do fio já estão nestes casos. `chamados/tests/test_e4c_f5_selo_chamada.py:269`, `:298`, `chamados/tests/test_e4c_f5_voltaA_sitios.py:65`.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  chamados.tests.test_e4c_f5_selo_chamada.SeloPurezaTest.test_MORDE_tipo_modelo_e_consulta \
  chamados.tests.test_e4c_f5_selo_chamada.SeloChamadaTest.test_MORDE_retirar_compositor_de_cada_chamada_da_A1

CORTES_F2_MUTANTE=a1_fio_composicao_local PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  chamados.tests.test_e4c_f5_voltaA_sitios.SitiosVoltaATest.test_MORDE_fio_composicao_local
```

### Quebras previstas fora da lista

O K16 chama `prefetch_related` sobre a consulta real. O harness do S128 recompila o corpo com `Linhas`, que implementa `select_related` e carece de `prefetch_related`. Prevejo `AttributeError` nos casos que passam por `AlcanceTest.resolver`, inclusive o coletor de mutantes e os consumidores em memória abaixo. Esses testes ficaram intactos [nao verificado]. `chamados/services/disputa_emissao.py:1955`, `chamados/tests/test_e4b_classe_e_memoria.py:165`, `:190`, `:294`, `:371`.

- `chamados/tests/test_e4b_classe_e_memoria.py:213`, `:255`, `:395`, `:408`, `:416`, `:422`, `:436`, `:441`, `:448`, `:465`, `:484`, `:501`, `:527`, `:533`, `:539`, `:545`, `:551`, `:558`, `:567`, `:573`, `:586`, `:952`, `:1003`, `:1033`.
- `chamados/tests/test_e4b_c4_tranca_memoria.py:59`, `chamados/tests/test_e4b_c4v14_memoria.py:110` e `chamados/tests/test_e4b_c6_memoria.py:87`, `:120`.

### Quem escreve e quem lê

| Campo ou cache | Escrita, leitura e significado |
|---|---|
| `decisoes` | A FK declara a sequência da pergunta. Resposta, validação, devolução e reabertura usam `registrar`, que grava as linhas. `leitura` consome `p.decisoes.all()` e o compositor mantém instante e validação do ato e texto da projeção. K1 entrega a sequência integral também ao mapa temporal, cujo recorte continua em `agora`. `chamados/models.py:4188`, `:4330`, `:4357`, `:4372`, `chamados/services/disputa_emissao.py:1532`, `:1639`, `chamados/services/validacao.py:173`, `:279`, `chamados/services/tarefa_do_dia.py:393`, `:545` |
| `_prefetched_objects_cache['decisoes']` | O ORM popula o cache de cada instância carregada. O gerenciador reverso o serve a `leitura` e ao mapa de K1. `registrar` invalida a chave na instância recebida como `pergunta`. Nessas cargas a chave guarda a sequência integral. A cópia da ressuscitação tem um cache próprio, com o recorte de simulação já existente. `chamados/models.py:1585`, `:1594`, `:4326`, `:4375`, `chamados/services/tarefa_do_dia.py:537`, `:545` |

**LEI-AKITA** origem=`chamados/models.py:4317`, `:4351` · testemunha=`chamados/tests/test_e4c_f5_voltaA_lotes.py:22` · RED=3 × 6 por lote e por candidata no O261 · juízes novos=0 · padrão=juiz único puro + fonte única (compositor) + fitness function.

## 19/11 20:27 · sessao 0e74903303d8 · CADEIA-20-PASSO-1 · leitura · origem=Code

## 1. Setembro — quem marca `orfa`

**A cópia marca `Batida.orfa` em `_coletar_dados_espelho`, usando `orfas_grade` do espelho.** `montar_dias` pega a `CelulaDia` da data e chama `sobras_da_ata(cel.ata, data, empresa)`. O PDF conta as ocorrências de cada `HH:MM` em `orfas_grade` e consome uma ocorrência a cada batida marcada. [app/ponto/services/espelho.py:704](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/services/espelho.py:704) · [app/relatorios/pdf_espelho.py:595](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/relatorios/pdf_espelho.py:595)

No pai, o PDF chamava `grade_espelho_janela` com as batidas do período, montava um `set` de horas órfãs por dia e marcava toda batida cujo `HH:MM` estivesse no conjunto. A volta 3 trocou tanto a fonte, da grade remontada para `ata['orfas']`, quanto a regra para horas repetidas, de pertencimento ao conjunto para contagem de ocorrências. [Pai, app/relatorios/pdf_espelho.py:543](/home/ronald/codex_ws/7efb46e8e04a977c58923a61c53d8572b5abec83/app/relatorios/pdf_espelho.py:543) · [Pai, app/relatorios/pdf_espelho.py:605](/home/ronald/codex_ws/7efb46e8e04a977c58923a61c53d8572b5abec83/app/relatorios/pdf_espelho.py:605) · [app/escala/utils.py:2384](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/escala/utils.py:2384) · [app/escala/services/leitor_celula.py:198](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/escala/services/leitor_celula.py:198)

**Há um ramo `< 5`, mas ele só muda o texto** para `razao nao lavrada, anterior ao horizonte` quando falta razão ou estado. A sobra é devolvida com a mesma hora, e o PDF usa essa hora para marcar a batida. A escolha entre o caminho do pai e o da cópia é feita sem guarda por versão. Isso permite mudar o cartão com a ata intacta, como no agregado de setembro. O mecanismo de cada uma das 63 diferenças, incluindo quais envolveram horas repetidas, fica **[nao verificado]**. [app/escala/services/leitor_celula.py:207](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/escala/services/leitor_celula.py:207) · [app/relatorios/pdf_espelho.py:595](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/relatorios/pdf_espelho.py:595) · `cartao_diff.txt:81` · `copia_sem_cartao.txt:14` · `copia_sem_cartao.txt:24`

## 2. Outubro e novembro — horas previstas, saldo e turnos

- **`horas_previstas` e `saldo`.** O coletor soma `minutos_previstos_do_dia` para as datas retornadas por `previstas_do_periodo` e divide por 60. Para uma janela reconhecida como competência, `_folha_manda` pode sobrescrever o previsto com os minutos do fechamento lido na hora ou, na falta desse total, com os da lavratura `DiaPago`. O fechamento lido soma `minutos_previstos` dos dias de trabalho e ausência da grade, que lê `tipo_dia` e `minutos_previstos` da ata. O saldo é refeito com as horas trabalhadas menos as previstas. [app/relatorios/pdf_espelho.py:374](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/relatorios/pdf_espelho.py:374) · [app/relatorios/pdf_espelho.py:459](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/relatorios/pdf_espelho.py:459) · [app/relatorios/cartao_pela_celula.py:69](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/relatorios/cartao_pela_celula.py:69) · [app/relatorios/cartao_pela_celula.py:321](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/relatorios/cartao_pela_celula.py:321) · [app/ponto/services/fechamento.py:1046](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/services/fechamento.py:1046) · [app/folha/export.py:329](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/folha/export.py:329) · [app/escala/services/leitor_celula.py:430](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/escala/services/leitor_celula.py:430)
- **`turnos`.** É `len(turnos_do_colab(colaborador, data_ini, data_fim))`. O pareador recebe os papéis por minuto derivados da ata, via `papel_por_minuto_da_ata` e `periodos_do_dia`, que leem as lâmpadas lavradas e o DNA. Uma relavra que mude esses papéis pode mudar a contagem de turnos. [app/relatorios/pdf_espelho.py:453](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/relatorios/pdf_espelho.py:453) · [app/ponto/turnos.py:1944](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/turnos.py:1944) · [app/ponto/turnos.py:1997](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/turnos.py:1997) · [app/ponto/juiz_batida.py:149](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/juiz_batida.py:149)

O diff do pai mantém as fórmulas de `horas_previstas` e `turnos`. A projeção nova acrescenta `sobras` à grade, e o PDF usa `orfas_grade` para marcar batidas e filtrar **`turnos_abertos`**. A mudança da volta 3 que pode atingir os números perguntados é a v5 no hash, que põe as células de outubro em diante na fila de relavra. O P1 registrou 34.641 mudanças no par `ata`/`veredito` em outubro e novembro. A sonda lê os cartões só depois do P2 e guarda os totais numéricos. Para 191, 203 e 922, a fonte final do previsto e os dias que explicam a queda ficam **[nao verificado]**. Para 87, quais atas alteraram o pareamento e se isso ocorreu no P1 ficam **[nao verificado]**. [Pai, app/relatorios/pdf_espelho.py:451](/home/ronald/codex_ws/7efb46e8e04a977c58923a61c53d8572b5abec83/app/relatorios/pdf_espelho.py:451) · [app/relatorios/pdf_espelho.py:453](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/relatorios/pdf_espelho.py:453) · [app/relatorios/pdf_espelho.py:514](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/relatorios/pdf_espelho.py:514) · [app/escala/services/leitor_celula.py:451](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/escala/services/leitor_celula.py:451) · [app/ponto/services/cartorio.py:163](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/services/cartorio.py:163) · [app/ponto/services/cartorio.py:521](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/services/cartorio.py:521) · `copia_sem_cartao.txt:14` · `sonda.py:135` · `sonda.py:168` · `cartao_diff.txt:8` · `cartao_diff.txt:61` · `cartao_diff.txt:71`

## 3. P2 — impressão muda com a ata igual

O caminho é `_julgar_colab_corpo` → `impressao_insumos` → `lavrar_veredito(save=False)` → `carimbar`. O cron recalcula o hash dos insumos e, se ele divergir, a célula passa pela guarda mesmo sem `--forcar`. A lavra compara a ata nova com a anterior e só inclui `ata` nos campos a gravar quando ela difere. `carimbar` grava a impressão nova mesmo quando `ata` fica fora dessa lista. O hash inclui, entre outros insumos, status dos chamados, fio vivo e turno ainda em curso. [app/ponto/services/cartorio.py:502](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/services/cartorio.py:502) · [app/ponto/services/cartorio.py:1540](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/services/cartorio.py:1540) · [app/ponto/services/cartorio.py:2012](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/services/cartorio.py:2012) · [app/ponto/portas/celula.py:1053](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/portas/celula.py:1053) · [app/ponto/portas/celula.py:1189](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/portas/celula.py:1189)

O comando também pode rejulgar internamente com `forcar=True` as células enfileiradas pela reconciliação. A sonda registra oito julgamentos na cópia, zero no MAIN e cinco impressões finais diferentes em novembro. Como ela guarda contagens e hashes, qual insumo mudou em cada célula e se alguma passou pela reconciliação interna ficam **[nao verificado]**. [app/ponto/services/cartorio.py:1147](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/services/cartorio.py:1147) · `copia_sem_cartao.txt:18` · `copia_sem_cartao.txt:24` · `main_sem_cartao.txt:13` · `sonda.py:57` · `sonda.py:168`

## 4. Agosto — como o cron alcança ata anterior ao horizonte

`processar_cartorio` carrega todas as células da empresa com `data < hoje`. Em agosto, `versao_para` devolve `None`, então a v5 fica fora do hash. A célula antiga ainda pode entrar no julgamento comum se faltar carimbo ou veredito, ou se outro insumo mudar a impressão. Uma vez nesse caminho, `_lavrar_razoes_sobras` roda sem guarda por versão e pode acrescentar `razoes_orfas`, inclusive `[]` quando há insumos e nenhuma sobra. [app/ponto/management/commands/processar_cartorio.py:51](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/management/commands/processar_cartorio.py:51) · [app/ponto/management/commands/processar_cartorio.py:63](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/management/commands/processar_cartorio.py:63) · [app/ponto/services/cartorio.py:160](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/services/cartorio.py:160) · [app/ponto/services/cartorio.py:295](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/services/cartorio.py:295) · [app/ponto/services/cartorio.py:1552](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/services/cartorio.py:1552) · [app/ponto/services/cartorio.py:962](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/services/cartorio.py:962) · [app/ponto/services/cartorio.py:2015](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/services/cartorio.py:2015)

O P1 registra uma impressão diferente em agosto em cada árvore e uma diferença no hash conjunto de `ata` e `veredito` na cópia. Se era a mesma célula nas duas árvores, qual insumo a fez entrar na fila e qual campo mudou ficam **[nao verificado]**. `main_sem_cartao.txt:9` · `copia_sem_cartao.txt:14` · `sonda.py:60`

## 19/11 20:24 · sessao 27d6f7e885de · E4C-F5-CONSTROI · leitura · origem=Code

### 1. O argumento de `pergunta_respondida`

Quem entrega o argumento é `_pergunta_viva_calc`. A função monta `_lido = DecisaoPergunta.composta(p)`, aplica o recorte temporal e chama `pergunta_respondida(_lido._replace(respondida_em=respondida, resposta_colab=resposta))`. O argumento continua sendo um `chamados.models.LeituraComposta`, a `namedtuple` de cinco campos devolvida por `composta`. É esse tipo que faz o `isinstance(..., SimpleNamespace)` falhar. [juizes.py:1408](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/juizes.py:1408) · [juizes.py:1428](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/juizes.py:1428) · [models.py:24](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/models.py:24)

`git log -S` e `blame` atribuem o assert a `e673e0efe1` (volta 2). Naquele commit, `projecao_temporal_da_pergunta` lia os instantes do ato para a instância ORM e mantinha o texto da projeção. O calc empacotava o par recortado em `SimpleNamespace(respondida_em=respondida, resposta_colab=resposta)`. O assert verificava a entrega desse par já projetado ao juiz. Naquele código, entregar a instância ORM faria `pergunta_respondida` chamar `leitura` de novo. Os asserts vizinhos verificavam uma leitura e uma chamada ao juiz por pergunta, com zero queries no cálculo após o prefetch. A volta A (`0db6afc73`) trocou o `SimpleNamespace` pelo `_replace` da leitura composta. `e673e0efe1:app/chamados/tests/test_e4c_f5_volta2_juizes.py:127` · `e673e0efe1:app/chamados/juizes.py:1389`, `:1430`, `:1559` · [teste atual:129](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/tests/test_e4c_f5_volta2_juizes.py:129)

### 2. O `1 contra 5` é do `calc`

O traceback de `red8_falhas.txt` mostra a falha em `calc.call_count >= quantidade`, na linha 163. A checagem de `leitor.call_count` vem na linha 164 e ficou sem ser executada nessa medição. `calc >= 5` verde com `leitor == 1` está **[nao verificado]**. [test_e4c_f5_volta2_juizes.py:159](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/tests/test_e4c_f5_volta2_juizes.py:159)

- **Carga.** As cinco perguntas da fixture pertencem à mesma disputa aberta e têm o mesmo chamado de destino. O fio carrega todas as perguntas das abertas com prefetch de `decisoes` e as anexa ao cartão de destino. A chamada a `pergunta_viva(p)` durante essa carga fica no ramo das disputas fechadas. [teste:119](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/tests/test_e4c_f5_volta2_juizes.py:119) · [teste:170](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/tests/test_e4c_f5_volta2_juizes.py:170) · [models.py:1979](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/models.py:1979) · [fio.py:251](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/services/fio.py:251)
- **Vida do cartão.** `tem_pergunta_viva` usa `any(...)`. A primeira pergunta da fixture dá `viva=True`: tem resposta, o teste força `_resposta_concorda_com_a_luz=False` e `_sem_acao` devolve `False` para uma pergunta sem materialização. O `any` para ali. Assim, as outras quatro pulam o **calc** nessa etapa. [fio.py:319](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/services/fio.py:319) · [models.py:3650](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/models.py:3650) · [juizes.py:1428](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/juizes.py:1428) · [raiox.py:182](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/services/raiox.py:182) · [teste:176](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/tests/test_e4c_f5_volta2_juizes.py:176)
- **Resposta de cada pergunta.** O laço de `resposta_enviada` passa pelas cinco via `espera_o_texto_do_colaborador → composta → leitura`, sem chamar o calc. O agregado `aguarda_o_colaborador` volta a ler cada pergunta para montar o recorte e chama `fio_vivo`, substituído por `False` no teste. `creditos_perguntas` também está substituído. Só o laço de `resposta_enviada` já implica pelo menos cinco chamadas a `leitura` sob o patch. [fio.py:173](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/services/fio.py:173) · [fio.py:189](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/services/fio.py:189) · [fio.py:358](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/services/fio.py:358) · [fio.py:388](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/services/fio.py:388) · [teste:179](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/tests/test_e4c_f5_volta2_juizes.py:179)
- **Patch e caches.** A property `.viva` usa o `pergunta_viva` reexportado em `models.py`. Essa função ainda resolve `_pergunta_viva_calc` no módulo `juizes`, onde o teste aplicou o patch. O memo `_pv_memo` pode pular o calc inteiro em uma nova consulta à mesma instância. Quando o calc roda, ele chama `composta` antes das guardas, e `composta` resolve `cls.leitura(p)` na classe sob patch. O prefetch fornece as linhas a `p.decisoes.all()` dentro da leitura. Nenhum desses caminhos produz cinco chamadas ao calc com uma só chamada ao leitor. [models.py:4507](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/models.py:4507) · [juizes.py:893](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/juizes.py:893) · [juizes.py:1406](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/juizes.py:1406) · [models.py:4330](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/models.py:4330) · [models.py:4357](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/models.py:4357)

### 3. Âncora de mutante ausente

Das 42 âncoras, só `rouba_respondida_do_hx` tem zero ocorrências. A busca literal ainda procura `pergunta_respondida(p) or not pergunta_viva(p, hoje=hoje)`. O sítio atual é `if pergunta_respondida(DecisaoPergunta.composta(p)) or not pergunta_viva(p, hoje=hoje):  # L-130: o juiz`. As outras 41 âncoras têm uma ocorrência cada, inclusive `pula_c6` e `perde_hoje_c6`, que procuram trechos dessa mesma linha. O `0 != 1` ocorre na checagem da âncora, antes de aplicar o mutante. O `PicklingError` vem depois, ao reportar o subteste, conforme `mutantes_mordem.txt`. [test_e4b_classe_e_memoria.py:26](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/tests/test_e4b_classe_e_memoria.py:26) · [âncora:51](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/tests/test_e4b_classe_e_memoria.py:51) · [contagem:1040](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/tests/test_e4b_classe_e_memoria.py:1040) · [disputa_emissao.py:1854](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/services/disputa_emissao.py:1854)

## 19/11 20:17 · sessao 1ba02fda85b9 · E4C-F5-CONSTROI · construtor · origem=Code

Implementei o helper único e os dois selos em `chamados/tests/`. A conferência estática mediu **9 módulos migrados com a mesma contagem de asserts do MAIN `eb1e49c1e`**. As expressões dos asserts e os nomes dos testes também conferem com o HEAD da cópia. Banco e suíte [nao verificado]. [Tabela e regra de contagem](chamados/tests/test_e4c_f5_duble_conforme.py:14)

### Arquivos tocados e montagem

| Módulo | Antes → depois | MAIN → cópia |
|---|---|---:|
| `e3_v8_apoio.py` | `N` para a pergunta e módulo sem `DecisaoPergunta` no HEAD :169/:199 → fábrica em :170 e classe do helper em :199. | [0 → 0](chamados/tests/e3_v8_apoio.py:170) |
| `test_e1_v10.py` | `_ln` devolvia `SimpleNamespace` no HEAD :120 → fábrica com os campos e a relação vazia para o compositor real. | [31 → 31](chamados/tests/test_e1_v10.py:121) |
| `test_e3_c8b_sem_banco.py` | O ambiente vinha direto do apoio, cujo mapa não instalava `chamados.models` → o contexto instala a classe do helper durante o ensaio. [Apoio original](chamados/tests/e3_c8b_apoio.py:150) | [86 → 86](chamados/tests/test_e3_c8b_sem_banco.py:29) |
| `test_e3_degrau_dono_e_porta.py` | Perguntas `N` e import do modelo real nos três casos do HEAD :263/:291/:343 → fábrica e classe do helper nos contextos da partição. [Consulta](chamados/tests/test_e3_degrau_dono_e_porta.py:263) [Carga](chamados/tests/test_e3_degrau_dono_e_porta.py:289) | [134 → 134](chamados/tests/test_e3_degrau_dono_e_porta.py:334) |
| `test_e3_pos_suite_sem_banco.py` | Ambiente direto do apoio no HEAD :15 e pergunta `N` no HEAD :53 → contexto compartilhado com C8b e fábrica no S1. [Contexto](chamados/tests/test_e3_pos_suite_sem_banco.py:17) | [29 → 29](chamados/tests/test_e3_pos_suite_sem_banco.py:54) |
| `test_e3_tarefa_admin_sem_banco.py` | Modelo `N` de literais e perguntas `N` no HEAD :86/:111/:200 → classe do helper, fábrica em `forma_b` e ambos na testemunha C6. [Modelo](chamados/tests/test_e3_tarefa_admin_sem_banco.py:85) [Forma B](chamados/tests/test_e3_tarefa_admin_sem_banco.py:112) | [7 → 7](chamados/tests/test_e3_tarefa_admin_sem_banco.py:184) |
| `test_e4b_c4v7_memoria.py` | A foto usava `DecisaoPergunta=modelo()` no HEAD :358 → classe compartilhada com o gerenciador vazio instalado no contexto da foto. O comando usa a montagem de `AlcanceTest`. [Foto](chamados/tests/test_e4b_c4v7_memoria.py:381) [Comando](chamados/tests/test_e4b_c4v7_memoria.py:86) | [65 → 65](chamados/tests/test_e4b_c4v7_memoria.py:358) |
| `test_e4b_classe_e_memoria.py` | `Objeto` montado à mão e `DecisaoPergunta=NS()` no HEAD :282/:334 → fábrica preservando `Objeto` e `Linhas`, com a classe do helper no módulo falso. [Pergunta](chamados/tests/test_e4b_classe_e_memoria.py:283) | [122 → 122](chamados/tests/test_e4b_classe_e_memoria.py:333) |
| `test_pergunta_viva.py` | `_p` devolvia `NS` sem `pk` no HEAD :13 → fábrica com `pk=None` e relação vazia, passando pelo compositor real. [Leitura real](chamados/models.py:4330) | [12 → 12](chamados/tests/test_pergunta_viva.py:9) |

Os dois arquivos novos são [duble_pergunta.py](chamados/tests/duble_pergunta.py:1) e [test_e4c_f5_duble_conforme.py](chamados/tests/test_e4c_f5_duble_conforme.py:1). O helper importa só a biblioteca padrão e lê os cinco valores da própria pergunta. Verifiquei o import normal, o retorno, `_replace` e `decisoes.all()` sem Django. A classe também fornece `RESPOSTA`, `VALIDACAO`, `DEVOLUCAO`, `RECUSA`, `REABERTURA`, `SILENCIO` e `ABSORCAO` para os ramos de `_pela_decisao`. O gerenciador `objects` é fornecido pela fixture da foto, que executa `.filter()`. [Helper](chamados/tests/duble_pergunta.py:3) [Ramos da tarefa](chamados/services/tarefa_do_dia.py:150) [Leitura da foto](chamados/services/foto_e4b.py:280)

`test_e3_c8c_sem_banco` recebe o helper por `forma_b` e `tarefa_em_memoria`. O R11 de `test_e3_portas_tarefa` recebe a montagem de `e3_v8_apoio`. C11, C4 tranca, C4v14 e C6 recebem a montagem compartilhada de `AlcanceTest`. [C8c](chamados/tests/test_e3_c8c_sem_banco.py:41) [R11](chamados/tests/test_e3_portas_tarefa.py:631) [C11](chamados/tests/test_e4b_c11_memoria.py:36) [C4](chamados/tests/test_e4b_c4_tranca_memoria.py:26) [C4v14](chamados/tests/test_e4b_c4v14_memoria.py:33) [C6](chamados/tests/test_e4b_c6_memoria.py:15)

### Casos especiais e previsão

- **`test_mutante_sem_patch_no_pai_cai_na_sentinela`**. A falta de `.composta` interrompia a porta antes da chamada que dispara `SENTINELA modulo real`. A montagem compartilhada corrige esse ponto. Prevejo que o mutante volte a atingir a sentinela [nao verificado]. [Caso](chamados/tests/test_e4b_classe_e_memoria.py:255) [Leitura anterior à chamada](chamados/services/disputa_emissao.py:1854) [Sentinela](chamados/tests/test_e4b_classe_e_memoria.py:224)
- **`test_mutantes_mordem_no_assert_pretendido`**. Prevejo vermelho em série. Medi **zero ocorrências** da âncora de `rouba_respondida_do_hx`, que ainda procura `pergunta_respondida(p)`. O código usa `pergunta_respondida(DecisaoPergunta.composta(p))`, e o teste exige uma ocorrência antes de aplicar o mutante. A hipótese para o `PicklingError` é o transporte do subteste falho, que referencia um caso com mocks. O traceback original está [nao verificado]. [Âncora](chamados/tests/test_e4b_classe_e_memoria.py:51) [Checagem](chamados/tests/test_e4b_classe_e_memoria.py:1038) [Mocks](chamados/tests/test_e4b_classe_e_memoria.py:288) [Transporte](core/tests/test_contract_tblib_no_paralelo.py:4)
- **`test_RED_callbacks_conhecidos_fora_do_alcance_recusam_com_nome`**. A fase de planejamento passa pela montagem corrigida antes de executar os callbacks. O `PicklingError` é compatível com o transporte de um subteste que retém a conexão e sua fila com `Mock`. A falha original e a cor após a cura ficam [nao verificado]. [Planejamento](chamados/management/commands/backfill_fio_morto_e4b.py:82) [Conexão](chamados/tests/test_e4b_c4v7_memoria.py:38) [Subteste](chamados/tests/test_e4b_c4v7_memoria.py:240)
- **Tetos A2 fora do escopo**. A sonda registra E2 `36 × 30`, F3 `56 × 48` e O261 `9 > 8`. Os asserts da cópia ainda exigem igualdade em E2 e F3 e teto de `8` no O261. A previsão continua vermelha pelos dados fornecidos, sem remedição [nao verificado]. [E2](chamados/tests/test_e2_tarefa_do_dia.py:938) [F3](escala/tests/test_f3_beco1_folga_na_lista.py:402) [O261](chamados/tests/test_o261_julgamento_desfaz_carimbo.py:205)

### Comandos para o Code

Selos de conformidade, contagem e um comando por mutante. Os três REDs passam quando a comparação rejeita o mutante em memória. [Conformidade](chamados/tests/test_e4c_f5_duble_conforme.py:38) [Mutantes](chamados/tests/test_e4c_f5_duble_conforme.py:51)

```bash
s=chamados.tests.test_e4c_f5_duble_conforme.DublePerguntaConformeTest
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test --parallel 1 "$s.test_duble_conforme_ao_modelo" "$s.test_contagem_dos_asserts_igual_ao_main"
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test --parallel 1 "$s.test_RED_mutante_sem_um_campo"
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test --parallel 1 "$s.test_RED_mutante_com_assinatura_diferente"
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test --parallel 1 "$s.test_RED_fonte_com_um_assert_a_menos"
```

Harnesses da lista, agrupados por classe onde há vários casos. `PerguntaVivaOrdemTest` seleciona os 11 casos. [Pergunta viva](chamados/tests/test_pergunta_viva.py:15) [C8b](chamados/tests/test_e3_c8b_sem_banco.py:210) [E4b](chamados/tests/test_e4b_classe_e_memoria.py:212)

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test --parallel 1 \
  chamados.tests.test_e1_v10.PlanoParticionaTest.test_RED_merito_sob_canonica_sem_episodio \
  chamados.tests.test_e3_c8b_sem_banco.AlimentacaoC6 \
  chamados.tests.test_e3_c8c_sem_banco.C8cSemBanco.test_RED_posicao_ausente_e_alimentada \
  chamados.tests.test_e3_degrau_dono_e_porta.ConsultaECarga \
  chamados.tests.test_e3_portas_tarefa.MatrizV8Test.test_v8_R11_um_instante_ate_o_app \
  chamados.tests.test_e3_pos_suite_sem_banco.PosSuiteSemBanco \
  chamados.tests.test_e3_tarefa_admin_sem_banco.LimitesDaConstrucao.test_C6_com_relacoes_e_tranca_carregadas_ainda_desce_por_pergunta \
  chamados.tests.test_e4b_c11_memoria.DiaC11Test.test_RED_conversa_e_sem_dia_nao_sao_rotuladas_exportadas \
  chamados.tests.test_e4b_c11_memoria.RelogioC11Test.test_RED_apply_ignora_recorte_argv_mas_DRY_conserva \
  chamados.tests.test_e4b_c4_tranca_memoria.TrancaMemoriaTest.test_RED_S143_retira_avulso_com_memo_do_ato_sem_aquecer_cargas \
  chamados.tests.test_e4b_c4v14_memoria.C7TrancaMemoriaTest \
  chamados.tests.test_e4b_c4v7_memoria.C4v7MemoriaTest \
  chamados.tests.test_e4b_c6_memoria.C6MemoriaTest \
  chamados.tests.test_e4b_classe_e_memoria.AlcanceTest \
  chamados.tests.test_pergunta_viva.PerguntaVivaOrdemTest
```

LEI-AKITA: origem=chamados/models.py:24 e :4351, testemunha=chamados/tests/test_e4c_f5_duble_conforme.py:38, RED=chamados/tests/test_e4c_f5_duble_conforme.py:51/:57/:72, padrao=Architecture Conformance Test (docs/GLOSSARIO.md:15).

## 19/11 20:07 · sessao 2cf1fbf9028e · E4C-F5-CONSTROI · verificador · origem=Code

1. **SEGUE.** `LeituraComposta` é um `collections.namedtuple` de `chamados.models`, já importado por um teste Django. `_fields` dá a ordem `validada_em`, `respondida_em`, `resposta_colab`, `materializada_em`, `via_resolucao`. [models.py:24](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/models.py:24) · [test_e4c_f5_opus_models.py:15](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/tests/test_e4c_f5_opus_models.py:15)  
   `composta` é `@classmethod` e `inspect.signature(DecisaoPergunta.composta)` dá `(p)`, sem `cls`. O selo compara com `Duble.composta` também ligada à classe. [models.py:4338](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/models.py:4338)  
   Os harnesses citados já fazem import normal de `chamados.tests.*`, caminho disponível para o helper puro. A leitura por caminho extrai corpos existentes. `e4_apoio.py` faz essa extração, e a troca de `chamados.models` ocorre nos harnesses. A lista integral da prova é [nao verificado]. [test_e3_tarefa_admin_sem_banco.py:23](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:23) · [test_e4b_classe_e_memoria.py:17](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/tests/test_e4b_classe_e_memoria.py:17) · [e3_v8_apoio.py:12](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/tests/e3_v8_apoio.py:12) · [e4_apoio.py:8](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/tests/e4_apoio.py:8) · [e3_v8_apoio.py:199](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/tests/e3_v8_apoio.py:199)

2. **SEGUE.** O 2005 autoriza a classe leve dentro do helper único. O compositor real é referência do selo. As adaptações ficam na exceção nominal do 1945 à vedação do 1710 e à lista fechada do 1830, restritas ao duble e à causa autorizada. `resposta_f5_devolvido_1945_2005.txt:3` · `resposta_f5_duble_1911_1945.txt:2` · `resposta_f5_duble_1911_1945.txt:7` · `e4c_f5_constroi_a_1911_1710.txt:13` · `respostas_f5_constroi_2_1911_1830.txt:10`

3. **SEGUE.** Comparar `_fields` em ordem e a assinatura ligada detecta a remoção de um campo declarado e a mudança da assinatura exposta. Tipos, valores, origem dos valores e comportamento de `_replace` ficam fora desse selo. A execução dos dois REDs é [nao verificado]. `resposta_f5_devolvido_1945_2005.txt:3` · [models.py:24](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/models.py:24) · [models.py:4345](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/models.py:4345)  
   Na cópia, os três consumidores de `composta` leem atributos, sem indexação posicional ou teste de identidade de `LeituraComposta`. O `isinstance` do juiz verifica `PerguntaDisputa`. O `_replace` de `fio.py` opera sobre `LeituraDoAto`, retornada por `.leitura`. O alvo do `_replace` na A1 citada no anexo é [nao verificado]. [models.py:2012](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/models.py:2012) · [models.py:3244](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/models.py:3244) · [models.py:3801](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/models.py:3801) · [juizes.py:1559](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/juizes.py:1559) · [fio.py:189](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/services/fio.py:189) · [models.py:4336](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/models.py:4336)

4. **SEGUE.** O escopo declarado é de teste. A comparação inspeciona a classe e a assinatura, sem acionar o compositor nem alcançar gravado de exportada, dinheiro ou outra ação sujeita a `!`. `resposta_f5_devolvido_1945_2005.txt:3` · [CLAUDE.md:265](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/CLAUDE.md:265) · [CLAUDE.md:406](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/CLAUDE.md:406)

## 19/11 19:50 · sessao e8d143d0a5af · E4C-F5-CONSTROI · verificador · origem=Code

# AVAL-DEVOLVIDO — RESPOSTA-F5-DUBLE-1911-1945

**A exceção resolve falhas de dublê, mas o PRONTO não fecha.** A prova registra falhas de selo, tetos e dois casos ORM cujo resultado diverge do esperado. O caso de ressuscitação F2 atinge uma regra vigente da porta. [a1_porteste.tsv:1, 3, 15, 19, 26, 29, 97–102, 114, anexo] [chamados/models.py:1534](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/models.py:1534)

A cópia tem o compositor real e os corpos dos juízes anteriores à A1. As referências à A1 vêm do diff anexo. Suite após a troca dos dublês e efeito em produção **[nao verificado]**. [chamados/juizes.py:1389](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/juizes.py:1389) · [chamados/juizes.py:1559](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/juizes.py:1559) · `a1_porteste.tsv:1–114` (anexo).

| Item | Veredito | Motivo |
|---|---|---|
| **EXCEÇÃO** | **PROCEDE — TÉCNICO** | O 1945 excepciona o veto do 1710 e responde à exigência de consulta do 1735/1830 para a causa nominal. A lista literal deixa de fora `e3_v8_apoio.py`, que instala o módulo falso sem `DecisaoPergunta` para o R11. A exceção também deixa de fora os dois `NameError` do global `VIAS_FORA_DO_FATO`. `resposta_f5_duble_1911_1945.txt:2`; `e4c_f5_constroi_a_1911_1710.txt:13–14`; `respostas_f5_constroi_1911_1735.txt:9`; `respostas_f5_constroi_2_1911_1830.txt:10`; `app/chamados/tests/e3_v8_apoio.py:196–209`; `a1_porteste.tsv:15,20,70` (anexos). |
| **FONTE** | **SEGUE** | `LeituraComposta` é uma `namedtuple` de exatamente cinco campos, em ordem: `validada_em`, `respondida_em`, `resposta_colab`, `materializada_em`, `via_resolucao`. A definição é `@classmethod def composta(cls, p)`, e a assinatura ligada é `(p)`. Os dois instantes vêm de `leitura`; os três demais valores vêm da pergunta. `app/chamados/models.py:19–25,4338–4347`. |
| **MUDA 1** | **PROCEDE — TÉCNICO** | Um helper único pode abastecer os dicionários existentes sem duplicar a fábrica de dublês. Falta fechar como ele entrega a **classe real** nos harnesses que rodam sem Django/settings, inclusive quando `chamados.models` está substituído. Recompilar uma função por AST é compatível com a fábrica única. Reexecutar a declaração da `namedtuple` cria outra classe. `app/chamados/tests/e4_apoio.py:8–16`; `app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:1–10,83–98`; `app/chamados/tests/test_e4b_classe_e_memoria.py:1–4,152–161,334`. |
| **MUDA 2** | **SEGUE** | A `composta` falsa cobre a ausência do método onde ela é injetada. Nos caminhos que usam a real, a pergunta precisa de `pk`, de `decisoes.all()` se o `pk` for definido e dos campos lidos pelo legado, inclusive `validada_por_id` se houver validação projetada sem linha correspondente. Isso cobre as três faltas de interface sem alteração no compositor. A contagem de verdes segue **[nao verificado]**. `app/chamados/models.py:4261–4279,4318–4323,4345–4347`; `a1_porteste.tsv:2,6,103–113` (anexo). |
| **MUDA 3** | **SEGUE** | `test_pergunta_viva` tem uma fábrica local `_p`, sem `pk` nem `decisoes`, e chama a entrada real. Os 11 erros anexados são de `pk`. A troca da fábrica cabe na exceção e preserva as asserções de precedência. `app/chamados/tests/test_pergunta_viva.py:4–23,74–77`; `a1_porteste.tsv:103–113` (anexo). |
| **SELO (a)** | **SEGUE** | O comparável é a assinatura do método ligado **`duble.composta`** e a sequência de campos do resultado. O caso deve chamar o dublê com cinco valores distinguíveis e verificar tipo e valores. Comparar `_fields` da mesma classe consigo mesma deixaria a construção do dublê sem teste. `app/chamados/models.py:24–25,4338–4347`; `app/chamados/tests/test_e4c_f5_opus_models.py:58–68`. |
| **SELO (b)** | **PROCEDE — TÉCNICO** | Para arquivo ausente no MAIN, a contagem por módulo é **sem base**, e o selo não fecha no literal. `test_e4c_f5_selo_chamada.py` entra como novo na A1. Para módulos que já existiam, falta a contagem no MAIN **[nao verificado]**. Contar apenas o nó Python `assert` também deixaria de fora as chamadas `self.assert*` usadas pelos testes. `resposta_f5_duble_1911_1945.txt:5` (anexo); `a1_porteste.tsv:97` e `puro_porteste.tsv:10` (anexos); `app/chamados/tests/test_e4c_f5_volta2_juizes.py:133–140`. |
| **RED** | **SEGUE** | A soma do anexo fecha em **114 vermelhos em 25 módulos**. O mutante de um campo omitido é um RED separado. A prova fornecida antecede o helper e a execução desse mutante é **[nao verificado]**. `a1_por_modulo.txt:1–25`; `resposta_f5_duble_1911_1945.txt:5–6` (anexos); `app/chamados/models.py:24–25`. |
| **PROIBIDO** | **SEGUE** | A vedação a fallback no compositor e a teste de tipo **dentro dos dois juízes** é compatível com o 1830. A A1 retira os dois ramos de tipo. O `isinstance` do RED 8 está no teste, e trocá-lo continua fora da autorização de dublês do 1945. `CLAUDE.md:14–22`; `respostas_f5_constroi_2_1911_1830.txt:3,11`; `resposta_f5_duble_1911_1945.txt:7` (anexos); `app/chamados/tests/test_e4c_f5_volta2_juizes.py:140`; diff A1, hunks de `chamados/juizes.py:1389,1559`. |
| **PRONTO** | **PROCEDE — TÉCNICO e LEI** | **Técnico:** trocar dublês deixa falhas de harness fora da causa autorizada e a prova não fecha a lista nominal dos 11 da F5/tetos da A2. Os tetos E2/F3 ficam intactos pelo 1830; O261 tem a exceção nominal 8→9 do 1735. **Lei:** o F2 mostra a validada deixando de voltar pela porta de ressuscitação. O PRONTO intermediário também não substitui o PRONTO cumulativo nem a certificação após C. `a1_porteste.tsv:3,15,19,26,70,97–102,114`; `e4c_f5_constroi_a_1911_1710.txt:14`; `respostas_f5_constroi_1911_1735.txt:8`; `respostas_f5_constroi_2_1911_1830.txt:3,9,12` (anexos); `app/chamados/models.py:1534–1545,1567–1583`. |

### Onde cada harness monta os dublês

| Módulos da prova | Modelo e pergunta |
|---|---|
| `test_e4b_classe_e_memoria`, `test_e4b_c4_tranca_memoria`, `test_e4b_c4v14_memoria`, `test_e4b_c4v7_memoria`, `test_e4b_c6_memoria`, `test_e4b_c11_memoria` | **Compartilhado.** `AlcanceTest.setUp` monta `self.p` e instala `DecisaoPergunta=NS()` no módulo falso. Os demais reaproveitam `AlcanceTest`, diretamente ou pela tranca. C4v14 também injeta o **modelo real** em `sem_veredito_entre`; C4v7 tem outro modelo falso no cenário de fotografia. `app/chamados/tests/test_e4b_classe_e_memoria.py:275–287,300–335,759–764`; `app/chamados/tests/test_e4b_c4_tranca_memoria.py:25–28`; `app/chamados/tests/test_e4b_c4v14_memoria.py:32–53`; `app/chamados/tests/test_e4b_c4v7_memoria.py:85–89,342–358`; `app/chamados/tests/test_e4b_c6_memoria.py:14–21`; `app/chamados/tests/test_e4b_c11_memoria.py:35–39,74–88`. |
| `test_e3_tarefa_admin_sem_banco`, `test_e3_c8b_sem_banco`, `test_e3_c8c_sem_banco`, `test_e3_pos_suite_sem_banco` | **Compartilhado em parte.** `tarefa_em_memoria` instala `DecisaoPergunta=N(**literais(...))`, só com constantes. C8b reaproveita essa base; C8c e PosSuite usam a de C8b. As perguntas vêm de `forma_b`, de `testemunha` no apoio C8b ou de fixtures locais. `ambiente_c6` recompila C6 e slot, sem instalar outro `chamados.models`. O teste de limite do módulo admin monta seu próprio ambiente e sua própria pergunta. `app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:72–98,101–116,153–203`; `app/chamados/tests/test_e3_c8b_sem_banco.py:14–23,199–221`; `app/chamados/tests/e3_c8b_apoio.py:83–90,109–120,150–198`; `app/chamados/tests/test_e3_c8c_sem_banco.py:41–61,123–135`; `app/chamados/tests/test_e3_pos_suite_sem_banco.py:47–65,71–83`. |
| `test_e3_degrau_dono_e_porta`, `test_e3_portas_tarefa` | **Dois arranjos.** Os três casos de `ConsultaECarga` compilam a partição com perguntas locais e usam o `DecisaoPergunta` real nesses caminhos. O R11 usa `app_com_relogios`: pergunta local e módulo falso que fornece `PerguntaDisputa` e `pergunta_viva`, sem `DecisaoPergunta`. `app/chamados/tests/test_e3_degrau_dono_e_porta.py:261–274,284–319,328–344`; `app/chamados/tests/test_e3_portas_tarefa.py:631–637`; `app/chamados/tests/e3_v8_apoio.py:158–171,182–209`. |
| `test_e1_v10`, `test_pergunta_viva` | **Locais.** `_ln` e `_p` criam as perguntas em memória. Os caminhos em questão chegam ao modelo real. `_ln` fornece `pk`, sem `decisoes`; `_p` carece de ambos. `app/chamados/tests/test_e1_v10.py:118–127,143–151`; `app/chamados/tests/test_pergunta_viva.py:4–13`; `a1_porteste.tsv:2,103–113` (anexo). |
| `test_cortes_f2_f9_m4`, `test_e2_tarefa_do_dia`, `test_f2_ressuscitar_tarefa_igual`, `test_o261_julgamento_desfaz_carimbo`, `test_f3_beco1_folga_na_lista`, `test_e4c_f5_volta2_juizes` | **ORM real nos casos vermelhos.** Os REDs 8 usam `BaseCortes` e medem o ORM. O `SimpleNamespace` de `medir_calc` é a **carga**, e a asserção de tipo inspeciona o argumento entregue ao juiz. `app/chamados/tests/cortes_f2_f9_apoio.py:58–74`; `app/chamados/tests/test_cortes_f2_f9_m4.py:36–56`; `app/chamados/tests/test_e2_tarefa_do_dia.py:913–938`; `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:276–295`; `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:187–205`; `app/escala/tests/test_f3_beco1_folga_na_lista.py:378–402`; `app/chamados/tests/test_e4c_f5_volta2_juizes.py:118–180`. |
| `test_e3_mutantes_alvo`, `test_e3_volta7_sem_banco`, `test_e4b_c12_selo`, `test_e4c_f5_volta1_leem_o_ato`, `test_e4c_f5_selo_chamada` | **Selos de fonte nos casos vermelhos.** Volta7 reaproveita o selo E3; C12 censa escritores; os dois selos F5 leem AST. Nenhum desses casos monta dublê de `DecisaoPergunta` ou de pergunta para produzir o vermelho. `app/chamados/tests/test_e3_mutantes_alvo.py:15–25`; `app/chamados/tests/test_e3_volta7_sem_banco.py:96–98`; `app/chamados/tests/test_e4b_c12_selo.py:22–32`; `app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:229–251`; diff A1, novo `test_e4c_f5_selo_chamada.py`. |

### Vermelhos fora da troca de dublês

- **M4, mudança de valor.** `test_RED_queda_final_le_resposta_do_ato_com_projecao_vazia` espera `respondida`, mas recebe `pendente`. O texto está só no ato. `estado` usa o texto da projeção pelo compositor. A expectativa antiga diverge do contrato 1830 para esse leitor, e a autorização de mudar a tabela F2 não nomeia o teste M4. `a1_porteste.tsv:1`; `puro_porteste.tsv:1`; `respostas_f5_constroi_2_1911_1830.txt:6` (anexos); `app/chamados/tests/test_cortes_f2_f9_m4.py:43–56`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:500–512`; `app/chamados/models.py:3241–3253,4345–4347`.
- **F2, regressão de lei na porta.** `test_RED_7_a_foto_guarda_o_autor_da_validacao_VIGENTE_no_empate_de_instante` falha antes de conferir o autor: voltam `0`, esperado `1`. A porta zera os campos projetados da cópia para perguntar ao C6 como a pergunta ficaria **depois** da ressuscitação. A A1 recompõe essa cópia a partir dos atos ainda vigentes, recupera a validação e encerra o C6 no primeiro degrau. `a1_porteste.tsv:101` (anexo); `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:276–295`; `app/chamados/models.py:1534–1545,1555,1567–1583,4318–4347`; diff A1, hunk de `_pergunta_viva_calc`.
- **Tetos medidos.** `test_lote_tem_queries_constantes` deu **36 × 30**; `test_MORDE_F5_queries_nao_crescem_com_os_dias`, **56 × 48**; `test_custo_por_candidata_e_CONSTANTE`, **9 > 8**. F3 consta da prova por módulo e do TSV, mas está ausente de `mods_quebrados.txt`. `a1_porteste.tsv:3,102,114`; `a1_por_modulo.txt:13`; `mods_quebrados.txt:1` (anexos); `app/chamados/tests/test_e2_tarefa_do_dia.py:931–938`; `app/escala/tests/test_f3_beco1_folga_na_lista.py:395–402`; `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:203–205`.
- **Selos e globais.** `test_todo_mutante_da_E3_tem_alvo_unico` e `test_alvos` perdem a mesma âncora `inclui_respondidas`. `test_RED_um_subTest_por_sitio` acusa `_gravar_estado`: a identidade C12 inclui a linha `462` e a chamada está na `469`. O selo F5 de renomeação procura `.leitura` e deixa `anterior` sem valor quando o fio passa a `.composta`. No selo de chamada, retirar a entrada composta da projeção também invalida a origem dos campos do `_replace` no segundo juiz. Os outros dois casos são `test_foto_temporal_nao_entrega_carimbos_futuros_ao_C6` e `test_RED_harness_carrega_projecao_real_sem_alterar_pergunta`, ambos sem o global `VIAS_FORA_DO_FATO`. `a1_porteste.tsv:15,19,26,29,70,97–98`; `puro_porteste.tsv:2` (anexos); `app/chamados/tests/e3_mutantes.py:30`; `app/chamados/tests/e4b_c12_ast.py:16–20`; `app/chamados/models.py:469`; `app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:229–251`; diff A1, `_fonte_composta` e hunk de `_pergunta_viva_calc`.
- **Diagnóstico incompleto.** Os dois REDs 8 aparecem como `?`. Há uma incompatibilidade estática em `test_red_8_custo_pergunta_viva_calc_prefetch_uma_e_cinco`: a asserção exige `SimpleNamespace` e a A1 passa `LeituraComposta._replace(...)`. A exceção final dos dois REDs 8, a causa dos dois `PicklingError` e o resultado da sentinela após corrigir os dublês estão **[nao verificado]**. Já o `assertEqual` de `test_slot_hora_noturno_alimenta_grade_vinculo_e_plantio_reais` compara uma `AttributeError` armazenada no slot com a tupla esperada. É a falta de `decisoes` chegando ao assert. `a1_porteste.tsv:9,41,88–89,99–100` (anexo); `app/chamados/tests/test_e4c_f5_volta2_juizes.py:127–149,151–180`; `app/chamados/services/carga_pergunta_viva.py:117–125`; `app/chamados/tests/test_e3_c8b_sem_banco.py:406–410`.
- **Controle com corpo puro.** Os REDs **1, 2, 3, 5 e 6** da volta 2 mudam de valor na árvore só com os juízes puros. São casos com ato e projeção divergentes, e o diff A1 adapta as chamadas para entregar `D.composta(p)`. Eles não aparecem entre os vermelhos A1. `puro_porteste.tsv:3–7`; `a1_porteste.tsv:99–100` (anexos); `app/chamados/tests/test_e4c_f5_volta2_juizes.py:37–104`; diff A1, hunks desse arquivo.

A troca nominal de dublês incide sobre fixtures. Os testes de exportada, dinheiro e `!` continuam exigindo seus resultados originais. A A1 já troca a leitura no guarda que antecede a verificação de exportada e muda o resultado do cenário ORM F2. Efeito em exportada, dinheiro ou `!` em produção **[nao verificado]**. `app/chamados/tests/test_e4b_classe_e_memoria.py:698–702,739–748`; `app/chamados/services/disputa_emissao.py:1848–1878`; `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:292–295`; diff A1, hunk de `reabrir_credor_de_pergunta_viva`.

Para o registro do PRONTO, o comparador aplicado ao vocabulário da cópia rejeita `Test Double com contract test` e aceita `Architecture Conformance Test`, termo do selo de conformidade. A execução do hook sobre um commit A1 está **[nao verificado]**. `bin/tests/test_lei_akita.sh:96–149,337–356`; `app/docs/GLOSSARIO.md:15`; `resposta_f5_duble_1911_1945.txt:8` (anexo).

## 19/11 19:34 · sessao 9d7185de4140 · CADEIA-20-PASSO-1 · sonda · origem=Code

```text
sonda_leitura: sonda.py [09aa035aee18a63b] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

HOJE 2026-11-19 ONTEM 2026-11-18 DIAS_COM_BATIDA 29858 CELULAS 51970
SEM_CELULA TOTAL 36 [(('2026-09', 2), 34), (('2026-11', 2), 2)]
ATA_NULA TOTAL 0 []
ATA_SEM_CHAVE_ORFAS TOTAL 0 []
FIM_SONDA
```

ERRO ou traceback no bloco fornecido? Não. `FIM_SONDA` apareceu? Sim, na última linha (`DADO RASPADO:1–7`).  
Reexecução [nao verificado]. A porta cria arquivos e grava log (`bin/codex_pergunta.sh:307–309`).

## 19/11 19:03 · sessao 93bc4756bb32 · E4C-F5-CONSTROI · construtor · origem=Code

**A1 implementada.** O censo AST mediu **41 chamadas na base e 41 na A1**. Das 36 identidades pendentes na base, 12 passaram a receber leitura composta. Restam **24**, descontada a exceção nominal do placar. A leitura estática dos dois juízes encontrou zero violações de pureza. Compilação em memória e `git diff --check` concluídos. Resultado da suíte, banco e mutantes ORM [nao verificado]. `chamados/tests/test_e4c_f5_selo_chamada.py:20`, `chamados/tests/test_e4c_f5_selo_chamada.py:191`, `chamados/services/raiox.py:147`.

## Arquivos tocados

- Produção: `chamados/juizes.py:1383`, `chamados/reconciliador.py:367`, `chamados/services/disputa_emissao.py:1854`, `chamados/services/fio.py:189`, `chamados/services/pergunta_no_app.py:87`, `ponto/services/cartorio.py:1690`.
- Testes novos: [test_e4c_f5_selo_chamada.py](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_chamada.py:20) e [test_e4c_f5_voltaA_sitios.py](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_voltaA_sitios.py:32). [nao verificado]
- Adaptações: `chamados/tests/test_e4c_f5_volta2_juizes.py:37`, `chamados/tests/e4c_f5_volta2_mutantes.py:33`, `chamados/tests/test_e4c_f2_leitura_do_ato.py:523`, `chamados/tests/test_f1nova_b2_leitores.py:87`.

Os juízes recebem os atributos do objeto e mantêm nome e assinatura. O corte temporal usa a constante `VIAS_FORA_DO_FATO` já definida no módulo de origem, sem consultar a classe. O comentário em cada juiz registra a leitura transitória da coluna pelos chamadores B/C que ainda entregam ORM. `chamados/juizes.py:13`, `chamados/juizes.py:1383`, `chamados/juizes.py:1549`, `chamados/models.py:2588`.

## Sítios antes e depois

`P` = `projecao_temporal_da_pergunta`. `R` = `pergunta_respondida`. `composta` = `DecisaoPergunta.composta`.

| Sítio | Antes na base `18bf86f01` | Depois na A1 |
|---|---|---|
| `_slot_esperado` | `P(p, agora)` · `chamados/juizes.py:351` | `P(composta(p), agora)` · `chamados/juizes.py:351` |
| `_pergunta_viva_calc`, corte temporal | `P(p, agora)` · `chamados/juizes.py:1411` | Uma chamada `_lido = composta(p)`, seguida de `P(_lido, agora)` · `chamados/juizes.py:1408–1409` |
| `_pergunta_viva_calc`, degrau da resposta | `R(SimpleNamespace(...))` · `chamados/juizes.py:1430` | `R(_lido._replace(respondida_em=respondida, resposta_colab=resposta))`, com os dois valores recortados da mesma leitura · `chamados/juizes.py:1428` |
| `lavrar_sem_turno_aberto` | `R(p)` · `chamados/reconciliador.py:366` | `R(composta(p))` · `chamados/reconciliador.py:367` |
| `veredito_orfa` | `R(p)` · `chamados/reconciliador.py:1133` | `R(composta(p))` · `chamados/reconciliador.py:1135` |
| `reabrir_credor_de_pergunta_viva` | `R(p)` · `chamados/services/disputa_emissao.py:1853` | `R(composta(p))` · `chamados/services/disputa_emissao.py:1854` |
| `_resolver_perguntas_por_chamado_resolvido` | `R(p)` · `chamados/services/disputa_emissao.py:2023` | `R(composta(p))` · `chamados/services/disputa_emissao.py:2025` |
| `processar_post_fio` | `R(_p)` · `chamados/services/fio.py:712` | `R(composta(_p))` · `chamados/services/fio.py:711` |
| `canonica_do_grupo` | `R(p)` · `chamados/services/fio.py:1287` | `R(composta(p))` · `chamados/services/fio.py:1287` |
| `merito_perdedor` | `R(p)` · `chamados/services/fio.py:1300` | `R(composta(p))` · `chamados/services/fio.py:1300` |
| `particao_do_app` | `R(p)` via apelido · `chamados/services/pergunta_no_app.py:86` | `R(composta(p))` via apelido · `chamados/services/pergunta_no_app.py:87` |
| `_julgar_colab_corpo._tem_resposta_do_colab` | `R(q)` · `ponto/services/cartorio.py:1689` | `R(composta(q))` · `ponto/services/cartorio.py:1690` |
| `espera_o_texto_do_colaborador` | `leitura(p)` seguida de `_replace(resposta_colab=p.resposta_colab)` · `chamados/services/fio.py:189–192` | `composta(p)` alimenta o mesmo teste de instante e o mesmo `respondida_sem_texto` · `chamados/services/fio.py:189–190` |

A partição e o fio já usavam as fontes corretas na base. Os dois casos de divergência são **GUARDAS**. A fixture grava pela porta de resposta automática e usa `update()` somente para simular o **PASSIVO**. O RED de delegação do fio exige uma chamada ao compositor, portanto a composição manual da base falha nessa asserção. `chamados/services/pergunta_no_app.py:86` na base, `chamados/juizes.py:1559` na base, `chamados/services/fio.py:189` na base, `chamados/tests/test_e4c_f5_voltaA_sitios.py:33`, `chamados/tests/test_e4c_f5_voltaA_sitios.py:65`.

A tabela F2 já veio da base com `False` para sem veredito, CSF, irmãs, estado e property, e `True` para `resposta_display` e `q_vivas`, com motivo por leitor. A prova reforçada mostra coluna com `(None, None)` e leitura temporal com validação e resposta nos instantes exatos `(T2, T1)`. `chamados/tests/test_e4c_f2_leitura_do_ato.py:500`, `chamados/tests/test_e4c_f2_leitura_do_ato.py:523`.

## Selo de chamada — 24 identidades

O ordinal conta as chamadas aos dois juízes em ordem de fonte dentro da função. A catraca compara as identidades encontradas com o conjunto declarado. A exceção é `creditos_perguntas`, `R#1`, pela tupla de `values_list`. `chamados/tests/test_e4c_f5_selo_chamada.py:20`, `chamados/tests/test_e4c_f5_selo_chamada.py:169`, `chamados/services/raiox.py:135`, `chamados/services/raiox.py:147`.

| Arquivo e linha | Função | Juiz e ordinal |
|---|---|---|
| `api/views.py:2596` | `api_disputa_perguntas` | R#1 |
| `api/views.py:2640` | `api_disputa_responder` | R#1 |
| `chamados/management/commands/auditar_invariantes_chamados.py:178` | `_contraprova_i21a` | R#1 |
| `chamados/management/commands/auditar_invariantes_chamados.py:597` | `Command.handle` | R#1 |
| `chamados/management/commands/auditar_invariantes_chamados.py:1671` | `Command.handle` | R#2 |
| `chamados/management/commands/dedup_perguntas_fato.py:76` | `Command.handle` | R#1 |
| `chamados/services/acoes_disputa.py:134` | `acao_regularizar_disputa` | R#1 |
| `chamados/services/becos.py:31` | `classificar` | R#1 |
| `chamados/services/declaracao_texto.py:418` | `declarar_hora_colab` | R#1 |
| `chamados/services/declaracao_texto.py:688` | `aplicar_declaracao` | R#1 |
| `chamados/services/materializacao.py:192` | `par_do_intervalo` | R#1 |
| `chamados/services/materializacao.py:259` | `paridade_absolvida_bug_par_103` | R#1 |
| `chamados/services/materializacao.py:290` | `_situacao_da_irma` | R#1 |
| `chamados/services/materializacao.py:885` | `estado_aplicacao_pergunta` | R#1 |
| `chamados/services/materializacao.py:1300` | `_recusa_sem_anuencia` | R#1 |
| `chamados/services/validacao.py:682` | `validar_pergunta` | R#1 |
| `chamados/services/validacao.py:902` | `recusar_pergunta` | R#1 |
| `chamados/services/vigia_fio.py:636` | `censo_alcance` | R#1 |
| `chamados/views_cobrar.py:170` | `validar_inline` | R#1 |
| `chamados/views_cobrar.py:178` | `validar_inline` | R#2 |
| `chamados/views.py:1275` | `responder_pergunta_disputa` | R#1 |
| `colaboradores/services/ficha.py:172` | `ficha_do_colab` | R#1 |
| `escala/services/cadastro_realidade.py:502` | `beco_em_lote` | P#1 |
| `folha/previa.py:142` | `_material` | R#1 |

## Comandos para o Code

Os labels abaixo cobrem os selos, os dois casos de divergência, o RED de delegação e as adaptações A1. A execução segue [nao verificado]. `chamados/tests/test_e4c_f5_selo_chamada.py:203`, `chamados/tests/test_e4c_f5_voltaA_sitios.py:45`, `chamados/tests/test_e4c_f5_volta2_juizes.py:37`, `chamados/tests/test_e4c_f2_leitura_do_ato.py:517`, `chamados/tests/test_f1nova_b2_leitores.py:87`.

```bash
export PYTHONDONTWRITEBYTECODE=1
teste() { [nao verificado]
  python3 manage.py test "$@"
}

selo=chamados.tests.test_e4c_f5_selo_chamada
sitios=chamados.tests.test_e4c_f5_voltaA_sitios.SitiosVoltaATest
v2=chamados.tests.test_e4c_f5_volta2_juizes.JuizesVolta2Test
f2=chamados.tests.test_e4c_f2_leitura_do_ato.FonteMigradaPorLeitorTest

teste "$selo" "$sitios" \ [nao verificado]
  "$v2.test_red_1_resposta_no_ato_instante_e_texto_corrente" \
  "$v2.test_red_2_reabertura_no_ato_vence_carimbo_projetado" \
  "$v2.test_red_3_validacao_no_ato_sem_carimbo_projetado" \
  "$v2.test_red_5_agora_corta_instante_do_ato_e_texto_corrente" \
  "$v2.test_red_6_respondida_instante_do_ato_texto_da_projecao" \
  "$f2.test_resposta_so_no_ato_cada_leitor_le_o_ato" \
  "$f2.test_validacao_so_no_ato_q_vivas_e_estado_leem_o_ato" \
  "$f2.test_instante_e_o_do_ato_nao_o_da_coluna" \
  chamados.tests.test_f1nova_b2_leitores.ClasseC_PropertyEEstadoTest.test_property_DELEGA_ao_juiz_nao_o_copia
```

| Mutante | Caso que deve morder |
|---|---|
| `a1_app_coluna` | `test_GUARDA_particao_do_app_instante_do_ato_texto_da_projecao` perde a ida ao histórico com instante só no ato. `chamados/tests/test_e4c_f5_voltaA_sitios.py:45` |
| `a1_app_texto_do_ato` | A mesma guarda perde a pendência com texto só na trilha. `chamados/tests/test_e4c_f5_voltaA_sitios.py:45` |
| `a1_fio_coluna` | `test_GUARDA_fio_instante_do_ato_texto_da_projecao` volta a esperar texto apesar do instante no ato e do texto corrente. `chamados/tests/test_e4c_f5_voltaA_sitios.py:55` |
| `a1_fio_texto_do_ato` | A mesma guarda deixa de esperar quando a projeção está vazia. `chamados/tests/test_e4c_f5_voltaA_sitios.py:55` |
| `a1_fio_composicao_local` | `test_MORDE_fio_composicao_local` falha na asserção de chamada única ao compositor. `chamados/tests/test_e4c_f5_voltaA_sitios.py:65` |
| Chamada crua adicional, apelido e atributo | Os casos sintéticos exigem a identidade nova e o ordinal correspondente. `chamados/tests/test_e4c_f5_selo_chamada.py:209`, `chamados/tests/test_e4c_f5_selo_chamada.py:220` |
| `_replace` com texto do ORM ou instante de outra leitura | O caso sintético exige a detecção das duas chamadas. `chamados/tests/test_e4c_f5_selo_chamada.py:232` |
| Retirada da composição em cada uma das 12 chamadas A1 | Um subteste por identidade exige que a chamada volte a cair na catraca. `chamados/tests/test_e4c_f5_selo_chamada.py:264` |
| `isinstance`, import de modelo, `.objects` e consulta | O selo de pureza assinala cada instrução sintética. `chamados/tests/test_e4c_f5_selo_chamada.py:289` |
| Fio com parâmetro renomeado e ORM entregue pelo nome `leitura` | O selo legado acusa que `respondida_sem_texto` recebeu a pergunta inteira. `chamados/tests/e4c_f5_volta2_mutantes.py:33`, `chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:220` |

Os cinco seletores ORM devem produzir falha na asserção indicada. Os casos AST e o mutante legado fazem a contraprova em memória e devem passar ao detectar a violação. Execução [nao verificado]. `chamados/tests/cortes_f2_f9_apoio.py:84`, `chamados/tests/test_e4c_f5_selo_chamada.py:209`, `chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:220`.

```bash
app="$sitios.test_GUARDA_particao_do_app_instante_do_ato_texto_da_projecao"
fio="$sitios.test_GUARDA_fio_instante_do_ato_texto_da_projecao"
CORTES_F2_MUTANTE=a1_app_coluna teste "$app" [nao verificado]
CORTES_F2_MUTANTE=a1_app_texto_do_ato teste "$app" [nao verificado]
CORTES_F2_MUTANTE=a1_fio_coluna teste "$fio" [nao verificado]
CORTES_F2_MUTANTE=a1_fio_texto_do_ato teste "$fio" [nao verificado]
CORTES_F2_MUTANTE=a1_fio_composicao_local teste "$sitios.test_MORDE_fio_composicao_local" [nao verificado]

teste "$selo.SeloChamadaTest.test_MORDE_chamada_crua_nova" [nao verificado]
teste "$selo.SeloChamadaTest.test_MORDE_apelido_e_chamada_por_atributo" [nao verificado]
teste "$selo.SeloChamadaTest.test_MORDE_replace_fora_da_projecao_do_mesmo_objeto" [nao verificado]
teste "$selo.SeloChamadaTest.test_MORDE_retirar_compositor_de_cada_chamada_da_A1" [nao verificado]
teste "$selo.SeloPurezaTest.test_MORDE_tipo_modelo_e_consulta" [nao verificado]
teste chamados.tests.test_e4c_f5_volta1_leem_o_ato.SeloTest.test_MORDE_parametro_renomeado_e_pergunta_inteira [nao verificado]
```

## Pergunta da sessão — testes congelados [nao verificado]

Previsões por leitura do código. Resultado de execução [nao verificado].

| Arquivo e linha | Quebra prevista |
|---|---|
| `chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:236`, `:243`, `:247` | O teste de renomeação ainda procura `.leitura(...)` no fio. Com `.composta(...)`, `anterior` fica sem valor. O caso de `:223` usa o mutante cuja âncora foi atualizada. `chamados/tests/e4c_f5_volta2_mutantes.py:33` |
| `chamados/tests/test_e4c_f5_volta2_juizes.py:122`, `:145`, `:170` | Os dois REDs 8 ainda nascem com texto sem instante e podem parar na porta antes da captura. Superado isso, `:141` ainda exige `SimpleNamespace` no C6. `chamados/models.py:3689`, `chamados/models.py:3721`, `chamados/juizes.py:1428` |
| `chamados/tests/test_pergunta_viva.py:8`, `:17` | Os dublês reais do C6 são `SimpleNamespace` sem `pk`. A leitura chamada pelo compositor acessa `p.pk`. `chamados/models.py:4318`, `chamados/juizes.py:1408` |
| `chamados/tests/test_e3_c8b_sem_banco.py:355`, `:383`, `:410`, `:430`, `chamados/tests/test_e3_c8c_sem_banco.py:136`, `chamados/tests/test_e3_pos_suite_sem_banco.py:66`, `:85`, `:99`, `:109`, `:113` | Os casos do C6 e do slot usam dublês sem a relação ou sem `composta` no módulo simulado. A chamada temporal direta de C8c também alcança o ramo que precisa de `VIAS_FORA_DO_FATO`, que o harness injeta só na classe simulada. `chamados/tests/e3_c8b_apoio.py:83`, `chamados/tests/e3_c8b_apoio.py:194`, `chamados/tests/test_e3_tarefa_admin_sem_banco.py:86` |
| `chamados/tests/test_e3_tarefa_admin_sem_banco.py:200`, `:206`, `chamados/tests/test_e3_degrau_dono_e_porta.py:271`, `:319`, `:346` | O C6 e a partição reais recebem `N(...)` sem relação `decisoes`. `chamados/models.py:4318`, `chamados/services/pergunta_no_app.py:87` |
| `chamados/tests/test_e4b_classe_e_memoria.py:303`, `:334`, `:374`, `:386` e derivados `test_e4b_c11_memoria.py:37`, `test_e4b_c4_tranca_memoria.py:27`, `test_e4b_c4v14_memoria.py:46`, `test_e4b_c4v7_memoria.py:86`, `test_e4b_c6_memoria.py:16`, todos em `chamados/tests/` | O módulo simulado fornece `DecisaoPergunta=NS()` sem `composta`. O namespace da projeção real também fornece a constante de identidade só na classe simulada. Os casos de sentinela e mutantes que executam esses mesmos caminhos podem parar antes da asserção. `chamados/tests/test_e4b_classe_e_memoria.py:254`, `chamados/tests/test_e4b_classe_e_memoria.py:1039` |
| `chamados/tests/test_e2_tarefa_do_dia.py:938`, `chamados/tests/test_o261_julgamento_desfaz_carimbo.py:204`, `escala/tests/test_f3_beco1_folga_na_lista.py:402`, `ponto/tests/test_selo_chokepoint_escrita.py:435`, `:440`, `:470`, `:475` | Tetos em risco com leituras individuais em cargas ainda sem `decisoes`. O delta da A1 permanece [nao verificado]. `chamados/services/tarefa_do_dia.py:530`, `chamados/services/relavra_via.py:236`, `chamados/models.py:4318`, `ponto/services/cartorio.py:146` |

## Escritores e leitores dos campos

| Campo | Quem escreve | Quem lê e significado |
|---|---|---|
| `validada_em` | Validação do admin grava coluna e ato. Devolução, resposta humana subsequente e reabertura podem retirar a validação. Há carimbo por absorção na porta do modelo. `chamados/services/validacao.py:161`, `:273`, `:279`, `chamados/services/disputa_emissao.py:1494`, `:1532`, `:1617`, `:1639`, `chamados/models.py:3340` | O compositor entrega a validação vigente ao juiz temporal e ao C6. `estado`, sem veredito e a face SQL também leem a validação vigente. O significado é o mesmo, com divergência possível entre ato e coluna. `chamados/models.py:2011`, `:3244`, `:3548`, `:4330`, `:4345`, `chamados/juizes.py:1391`, `:1410` |
| `respondida_em` | Resposta humana, automática e carimbo pela trilha gravam o instante. Reabertura e recusa limpam a resposta vigente. Há carimbo projetado na porta do modelo. `chamados/services/disputa_emissao.py:1528`, `:1532`, `:1617`, `:1639`, `:1683`, `:1686`, `:1705`, `:1708`, `chamados/models.py:3337` | A1 julga o instante vigente via compositor. `_slot_esperado` também mantém sua comparação com a coluna e os chamadores transitórios entregam a coluna. O campo representa o instante da resposta, mas essas leituras podem divergir sobre a vigência. `chamados/models.py:4328`, `:4334`, `:4346`, `chamados/juizes.py:353`, `:1395`, `:1557`, `chamados/services/fio.py:190` |
| `resposta_colab` | Respostas humana e automática escrevem o texto corrente, e reabertura/recusa limpam a coluna. A trilha guarda o texto que cada ato registrou. `chamados/services/disputa_emissao.py:1481`, `:1532`, `:1617`, `:1639`, `:1682`, `:1686` | Compositor, juízes e fio usam o texto corrente. `resposta_display` usa o texto da sequência e o placar usa a projeção SQL. Há significados distintos quando ato e projeção divergem. `chamados/models.py:3825`, `:4346`, `chamados/juizes.py:1392`, `:1558`, `chamados/services/fio.py:190`, `chamados/services/raiox.py:135` |
| `materializada_em` | As portas `carimbar` e `carimbar_identidade` gravam o carimbo. `descarimbar` e sua porta em lote o retiram. `chamados/models.py:3302`, `:3334`, `:3387`, `:3413`, `:3496` | Compositor, projeção temporal e C6 leem o mesmo carimbo projetado. A projeção temporal o recorta por `agora` e o C6 interpreta seu efeito junto da via. `chamados/models.py:4347`, `chamados/juizes.py:1393`, `:1399`, `:1421` |
| `via_resolucao` | As portas de carimbo e descarimbo gravam ou limpam a via. `encerrar_por_fato_no_espelho` pode trocar apenas a via. `chamados/models.py:3306`, `:3387`, `:3413`, `:3496`, `:3505` | Compositor, projeção temporal, C6 e mérito do fio leem o motivo projetado. O significado do campo é compartilhado. O corte conserva as vias de identidade fora do fato mesmo quando o carimbo fica depois do horizonte. `chamados/models.py:2588`, `:4347`, `chamados/juizes.py:1399`, `:1421`, `chamados/services/fio.py:1299` |
| `decisoes` | A FK define a relação. `DecisaoPergunta.registrar` insere as linhas e invalida o cache da instância recebida. `chamados/models.py:4176`, `:4350`, `:4360`, `:4366` | `leitura` consome a sequência ordenada, com o passivo, e `composta` usa os instantes vigentes daí. O significado compartilhado é trilha de atos. A regra de vigência pertence a `leitura`. `chamados/models.py:4318`, `:4322`, `:4327`, `:4345` |

O anexo 1735 item 7-iv informa `MUDAM=0 em 51.563` para a property, com referência a `medir_1710/r.txt`. Valor de produção [nao verificado].

LEI-AKITA origem=`chamados/juizes.py:1383` e `chamados/juizes.py:1549` + `chamados/models.py:4339`, testemunha=`chamados/tests/test_e4c_f5_selo_chamada.py:203`, RED=`chamados/tests/test_e4c_f5_voltaA_sitios.py:65`, padrão=juiz único puro + fonte única (compositor) + fitness function.

## 19/11 18:40 · sessao 29cd68d25a8d · E4C-F5-CONSTROI · sonda · origem=Code

Não executei novamente a sonda. A porta cria um arquivo temporário e grava na trilha, e a tarefa proíbe alterações em arquivos. (`bin/sonda_leitura.sh:216`, `bin/sonda_leitura.sh:217`, `bin/sonda_leitura.sh:248`, `bin/sonda_leitura.sh:249`)

**Saída literal raspada fornecida** [nao verificado]

```text
sonda_leitura: sonda.py [e54f137c79492f08] no cpuset 4-7, lendo sombra como leitor (LEITURA)
sonda_leitura: arvore=/home/ronald/wt-f5c-170736/app (copia), banco segue sombra como leitor

BANCO=sombra
UNIVERSO=51563 PERCORRIDAS=51563 ERROS=0
MUDA_RESPONDIDA=0 AMOSTRA=[]
MUDA_ESTADO=0 AMOSTRA=[]
MUDA_SEM_VEREDITO=0 AMOSTRA=[]
DISPLAY_DIF=0 CRU_DIF=0 PKS_DISPLAY=[]
VEREDITO=VALIDO MUDAM=0 DISPLAY_DIF=0
FIM_SONDA_1E
```

- `VEREDITO=VALIDO`. Sim, `ERROS=0` e `UNIVERSO==PERCORRIDAS` (`51563==51563`). [nao verificado]
- Todos os `MUDA_*` são `0`, com `AMOSTRA=[]` nos três campos. [nao verificado]
- `DISPLAY_DIF=0`, `CRU_DIF=0` e `PKS_DISPLAY=[]`. [nao verificado]

## 19/11 18:38 · sessao 7c56dccf9316 · CADEIA-20-PASSO-1 · construtor · origem=Code

**CADEIA-20-PASSO-1 · Volta 3 implementada na cópia.** A v5 começa na competência `2026-10`. O cartório usa o caminho normal de impressão e `_em_dia`. A projeção entrega hora e razão ou estado, e o `--apply` imprime o contador da empresa. `ponto/services/cartorio.py:161`, `:1334`, `:1552`, `escala/services/leitor_celula.py:193`, `ponto/management/commands/processar_cartorio.py:75`.

Conferi a sintaxe, os alvos dos mutantes e o censo estático. Os corpos de `versao_para`, `impressao_insumos` e `_regra_do_dia` conferem com a base. Suite, execução dos REDs e mutantes, smoke e medidas de produção ficam **[nao verificado]** para Code e Opus. `ponto/services/cartorio.py:280`, `:493`, `:1334`, `escala/tests/test_cadeia20_volta3_mutantes.py:116`.

A fixture antiga de espúrias do PDF monta escala e batidas em `relatorios/tests/test_cartao_marca_espuria.py:21`. A marcação agora vem da ata projetada. O resultado dessa fixture fica **[nao verificado]**. `relatorios/pdf_espelho.py:595`.

### Comandos para a sessão

Os REDs cobrem a relavra da 10 e a impressão da 09, os três estados do aval, a conferência por ocorrência e a entrada de um leitor cru. `escala/tests/test_cadeia20_volta3.py:42`, `:73`, `:142`, `:160`, `:166`, `:172`, `escala/tests/test_cadeia20_selo_orfas.py:224`.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  escala.tests.test_cadeia20_volta3 \
  escala.tests.test_cadeia20_selo_orfas \
  ponto.tests.test_o219_versao_da_regra \
  ponto.tests.test_e3_isento_na_impressao
```

Cada chamada abaixo executa um mutante. O harness exige que o RED passe com o código original e falhe na asserção indicada após a troca em memória. `escala/tests/test_cadeia20_volta3_mutantes.py:67`, `:116`.

```bash
mutante() {
  CADEIA20_V3_MUTANTE="$1" PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
    escala.tests.test_cadeia20_volta3_mutantes
}
mutante sem_v5
mutante v5_na_09
mutante estado_antigo_apagado
mutante estado_do_horizonte_errado
mutante falha_apagada
mutante rotulo_trocado
mutante espelho_descarta_sobras
mutante calendario_so_hora
mutante card_so_hora
mutante pdf_so_hora
mutante contador_perde_chave
mutante contador_ignora_ordem
mutante apply_omite_contador
mutante catraca_aceita_leitor
```

### Inventário

| Item | Mudança e localização |
|---|---|
| V3-1 | Declaração da v5 em `ponto/services/cartorio.py:161`. Ajustei os testes existentes que fixavam o teto ou o valor vivo da competência 10 em `ponto/tests/test_o219_versao_da_regra.py:79` e `ponto/tests/test_e3_isento_na_impressao.py:93`. |
| V3-2 | Mapa de rótulos em `escala/orfa_classe.py:6`. Projeção por ocorrência e entrega por dia em `escala/services/leitor_celula.py:193`, `:454`. |
| V3-3 | O serviço usa a projeção sobre as células já carregadas e entrega `orfas_grade` em `ponto/services/espelho.py:152`, `:704`, `:862`. O adaptador preserva o contrato dos demais chamadores em `escala/utils.py:1099`. Calendário e card em `templates/ponto/espelho.html:151`, `:165`, `:249`. Coleta, marcação, texto e legenda do PDF em `relatorios/pdf_espelho.py:478`, `:595`, `:874`, `:1013`. |
| V3-4 | Contador com número e PKs em `ponto/services/cartorio.py:1038`. Saída ao fim do `--apply` em `ponto/management/commands/processar_cartorio.py:75`. |
| V3-5 e V3-6 | Catraca e injeção de leitor cru em `escala/tests/test_cadeia20_selo_orfas.py:11`, `:220`, `:224`. REDs e mutantes em `escala/tests/test_cadeia20_volta3.py:42`, `:142` e `escala/tests/test_cadeia20_volta3_mutantes.py:67`. |

**Linha de base do selo — 5 Python, 0 HTML.** Os sítios contados são `colaboradores/services/calendario.py:410`, `escala/utils.py:1131`, `ponto/juiz_batida.py:161`, `ponto/services/cluster_espurio.py:31` e `relatorios/cartao_pela_celula.py:288`. Tetos em `escala/tests/test_cadeia20_selo_orfas.py:11`.

- **`ata['razoes_orfas']`** é escrita por `_lavrar_razoes_sobras` em `ponto/services/cartorio.py:968`, `:1001`, pelos dois ramos em `:1434`, `:2015`. Leem a chave a projeção, `medir_razoes_sobras` e o contador do horizonte. `escala/services/leitor_celula.py:202`, `ponto/services/cartorio.py:1013`, `:1049`.
- **`REGRA_VERSOES`** é declarada em `ponto/services/cartorio.py:161`. Leem a tupla a validação no módulo e `versao_para`. O cartório, a projeção e o contador consultam `versao_para`. `ponto/services/cartorio.py:269`, `:293`, `:1047`, `:1334`, `escala/services/leitor_celula.py:219`.

## 19/11 18:26 · sessao 42a726cd1503 · E4C-F5-CONSTROI · verificador · origem=Code

### 1c-1 — SEGUE

O ORM cru continua aceito pelo corpo puro. `pergunta_respondida` lê as colunas `p.respondida_em` e `p.resposta_colab`. A projeção temporal lê também `p.validada_em`, `p.materializada_em` e `p.via_resolucao`, e o C6 usa o par já recortado por `agora`. A cópia atual lê os **instantes** pelo ato no ramo ORM. A equivalência com o `main` de hoje fica [nao verificado]. `app/chamados/juizes.py:1389`, `app/chamados/juizes.py:1430`, `app/chamados/juizes.py:1559`, `app/chamados/models.py:2634`, `app/chamados/models.py:2748`, `app/chamados/models.py:3647`.

- **Erro versus valor.** Nenhum chamador ORM examinado exige o `isinstance` para ter os atributos que o juiz puro acessa. A regressão possível é de valor. Os REDs atuais mostram ato e coluna divergindo para resposta, reabertura e validação. `app/chamados/tests/test_e4c_f5_volta2_juizes.py:37`, `app/chamados/tests/test_e4c_f5_volta2_juizes.py:93`.
- **Harnesses.** Sim, o teste de tipo é a causa registrada. Reproduzi em memória o `NameError` quando o harness omite `PerguntaDisputa` e o `TypeError` quando injeta um `SimpleNamespace` no lugar da classe. Ambos passam desse ponto com o ramo removido. O verde das 11 famílias com o compositor ligado na A fica [nao verificado]. `app/docs/RELATO.md:11`, `app/chamados/tests/e4_apoio.py:19`, `app/chamados/tests/e3_c8b_apoio.py:83`.
- **Aval e alcance.** A passagem intermediária com leitura de coluna está declarada. A certificação única após C é compatível com o pouso único registrado para o 0815 e com o selo final do 1735. A prévia recebe o efeito de leitura tanto no juiz de resposta quanto em `fio_vivo`. Escrita em exportada, dinheiro ou `!` fora do declarado não foi identificada nesses caminhos. O cotejo integral com 1710 e 0815 e a prova da competência 11 ficam [nao verificado]. `respostas_f5_constroi_2_1911_1830.txt:3`, `respostas_f5_constroi_2_1911_1830.txt:4`, `respostas_f5_constroi_1911_1735.txt:3`, `app/docs/RELATO.md:45`, `app/folha/previa.py:139`.

### 7-iv — PROCEDE (TÉCNICO)

A tabela fecha em **cinco `False` e dois `True`**. A condição que não fecha é a prova atribuída a `:505–509`. Ali `estado` e `q_vivas` distinguem a validação do ato, mas a exclusão por `sem_veredito_entre` já ocorre pelo texto vazio. Nenhuma das três asserções distingue a fonte do instante da resposta. `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:490`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:505`, `app/chamados/models.py:2004`, `app/chamados/models.py:3235`, `app/chamados/models.py:3613`.

| Leitor | Esperado | Caminho na fixture |
|---|---|---|
| `sem_veredito_entre` | `False` | O instante do ato chega ao juiz com texto corrente vazio. O juiz rejeita o par. `app/chamados/models.py:2004`, `app/chamados/juizes.py:1564` |
| CSF | `False` | Consulta pai e destino pelo mesmo `sem_veredito_entre`, com `ignorar_tranca=True`. `app/ponto/supra_juiz.py:573` |
| Irmãs | `False` | Aplica `sem_veredito_entre` ao universo de crédito, também com `ignorar_tranca=True`. `app/chamados/juizes.py:2127` |
| `estado` | `False` | Sem validação, sem materialização e com o par rejeitado, o rótulo é `PENDENTE`. O teste compara com `RESPONDIDA`. `app/chamados/models.py:3235`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:327` |
| Property `respondida` | `False` | Delega ao juiz, que rejeita o texto corrente vazio sob o contrato novo. `app/chamados/models.py:3786`, `app/chamados/juizes.py:1564` |
| `resposta_display` | `True` | Lê `07:13` do ato e o devolve na fixture de texto com opções vazias. `app/chamados/models.py:3815`, `app/chamados/models.py:3834`, `app/chamados/tests/cortes_f2_f9_apoio.py:62` |
| `q_vivas` | `True` | Passa pela ausência de validação e entra pelo ramo largo da célula `furo`, que acusa. `app/chamados/models.py:3611`, `app/escala/models.py:1767`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:360` |

- **Literal da sonda.** A sonda existente compara os textos brutos. A property pode converter IDs e valores em nomes ou rótulos. Comparar sua saída com a coluna bruta pode acionar o `PAREI` mesmo com ato e projeção alinhados. A contagem pedida fica [nao verificado]. `app/chamados/services/vigia_texto_resposta.py:16`, `app/chamados/models.py:3815`, `app/chamados/models.py:3834`, `respostas_f5_constroi_2_1911_1830.txt:6`.
- **Aval e alcance.** A tabela é compatível com o contrato do 1735 e com o corte posterior ao 0815 registrado no RELATO. O item altera expectativas de leitura e mantém o display fora da F5. Não identifiquei escrita em exportada, dinheiro ou `!` fora do declarado. O cotejo integral com 1710 e 0815 fica [nao verificado]. `respostas_f5_constroi_1911_1735.txt:9`, `respostas_f5_constroi_2_1911_1830.txt:6`, `app/docs/RELATO.md:34`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:500`.

## 19/11 18:21 · sessao 5976fa43d10f · CADEIA-20-PASSO-1 · verificador · origem=Code

## AVAL-DEVOLVIDO por item — CADEIA-20-PASSO-1

O mecanismo de versão funciona sem `--forcar` e sem editar `impressao_insumos`. Ele **muda o hash gravado**. O 1815 precisa explicitar esse efeito, distinguir ausência de chave dentro e fora do horizonte e corrigir o literal de `padrao=` que o selo rejeita. `app/ponto/services/cartorio.py:518`, `app/ponto/services/cartorio.py:575`, `app/ponto/services/cartorio.py:1528`, `bin/tests/test_lei_akita.sh:337`.

A cópia traz a referência ao CONSTRÓI 1010. O anexo `aval_1010.txt:1` é o RESPOSTA-LEI 2320. O PRONTO e o PROIBIDO integrais do CONSTRÓI, o histórico Git e o estado atual da produção ficam [nao verificado]. `app/docs/PROMPTS.md:2592`, `app/docs/PROMPTS.md:2616`.

### Desenho

- **(a) SEGUE.** A volta 3 é pré-requisito do pouso. Os quatro pontos têm as sobras em horas, ainda sem a razão. O censo da catraca também precisa enxergar o calendário do HTML e a leitura do PDF por dia que marca a batida órfã. CHECK-DE-TELA (d) e smoke de três colabs [nao verificado]. `app/ponto/services/espelho.py:703`, `app/ponto/services/espelho.py:858`, `app/templates/ponto/espelho.html:151`, `app/templates/ponto/espelho.html:242`, `app/relatorios/pdf_espelho.py:477`, `app/relatorios/pdf_espelho.py:552`, `app/relatorios/pdf_espelho.py:607`, `laudo.md:2`.
- **(b) PROCEDE.** O default vigente é **2026-10 em diante**. A função retorna v4 da 10 em diante, v1 na 09 e `None` até a 08, pelo mês do fim da janela de cada empresa. Com corte 21, começa em **21/09/2026**, abrange 10, 11 e as seguintes. O seletor usa um piso fixo. `app/ponto/services/cartorio.py:161`, `app/ponto/services/cartorio.py:277`, `app/ponto/services/cartorio.py:290`, `app/ponto/janelas.py:10`, `app/ponto/janelas.py:60`.
- **(b), caminho real.** Uma `(5, '2026-10')` chega por `_regra_do_dia` a `regra:5` no hash. No ramo normal, `_em_dia` fica falso e o fluxo passa pelo `continue` até a lavra. No `nunca_bateu`, `imp0` faz o mesmo. O comando passa `forcar=False` por padrão e lavra com `--apply`. A cópia ainda tem v4. `app/ponto/services/cartorio.py:161`, `app/ponto/services/cartorio.py:518`, `app/ponto/services/cartorio.py:1310`, `app/ponto/services/cartorio.py:1375`, `app/ponto/services/cartorio.py:1410`, `app/ponto/services/cartorio.py:1517`, `app/ponto/services/cartorio.py:1528`, `app/ponto/services/cartorio.py:1988`, `app/ponto/management/commands/processar_cartorio.py:25`, `app/ponto/management/commands/processar_cartorio.py:67`.
- **(b), estado nomeado.** Chave ausente e `razoes_orfas: []` são distinguíveis. A ausência também ocorre quando faltam insumos e pode aparecer dentro do horizonte. O texto “anterior ao horizonte” exige confirmar a competência da célula. O contador existente recebe uma lista sem recorte e conta chave ausente só quando `orfas` tem elementos. `app/ponto/services/cartorio.py:959`, `app/ponto/services/cartorio.py:1001`, `app/escala/utils.py:2117`, `app/escala/utils.py:2131`.

### MEDIR na sombra

| Item | Veredito e motivo |
|---|---|
| **1** | **PROCEDE.** O universo é definível por ocorrência em `ata['orfas']`, com `n_orfas` identificando atas agregadas que só têm a contagem. Recorte pela competência da célula. O contador atual pode fechar a conservação com chave ausente ou `razao=None` e falha nomeada. Para o literal `com_razao == universo`, ambos precisam zerar, com alinhamento por célula e universo não vazio. O resultado atual é [nao verificado]. `app/ponto/services/cartorio.py:761`, `app/ponto/services/cartorio.py:1001`, `app/escala/services/leitor_celula.py:180`, `app/escala/orfa_classe.py:74`. |
| **2** | **SEGUE.** Compare rubricas por colab e competência no mesmo recorte de MAIN e CÓPIA, após o caminho real. O motor lê lâmpadas e papéis da ata, e o fechamento lê os minutos da grade para realizado e abono. Esses são sítios que podem mover valores se a relavra mudar esses insumos. O RELATO registra 618 colabs com zero delta num ensaio anterior. O zero com a volta 3, sem a flag, é [nao verificado]. `app/ponto/motor_calculo_v2.py:495`, `app/ponto/motor_calculo_v2.py:611`, `app/ponto/juiz_batida.py:149`, `app/ponto/services/fechamento.py:1013`, `app/ponto/services/fechamento.py:1060`, `app/docs/RELATO.md:16`. |
| **3** | **SEGUE.** Compare os campos persistidos após a primeira e a segunda chamada com os mesmos insumos e relógio. A entrada roda sem `--forcar` e o comando ainda pode acionar a segunda passada interna forçada das reconciliadas. O teste da obra força a chamada e compara a ata. O zero no caminho exigido é [nao verificado]. `app/ponto/services/cartorio.py:1128`, `app/ponto/portas/celula.py:1181`, `app/escala/tests/test_cadeia20_costura.py:31`, `app/escala/tests/test_cadeia20_costura.py:55`. |
| **4** | **SEGUE.** Conte células únicas que o bump fez sair da guarda, com um controle na mesma base para descontar a fila normal, e cronometre a primeira passada. O comando varre todos os dias passados da empresa e o total impresso inclui outros motivos de julgamento. Quantidade e minutos [nao verificado]. `app/ponto/management/commands/processar_cartorio.py:49`, `app/ponto/management/commands/processar_cartorio.py:63`, `app/ponto/management/commands/processar_cartorio.py:68`, `app/ponto/services/cartorio.py:1528`. |

### Mecânicos, verificador e proibido

- **(3) SEGUE.** O corte fornecido e o 1815 autorizam registrar a pergunta já cortada, hoje implementada por `escala/orfa_classe.py::razao_da_sobra`. Faltam o registro e a frase em CORTES. O selo de juiz exige a forma `corte Ronald: juiz razao_da_sobra nasce` em `CORTES.md`. `corte_razao_da_sobra.txt:1`, `cadeia_20_volta_3_e_horizonte_1911_1815.txt:7`, `app/escala/orfa_classe.py:29`, `app/core/juizes.py:1324`, `bin/tests/test_juiz_novo_tem_corte.sh:27`, `bin/tests/test_juiz_novo_tem_corte.sh:37`.
- **(4) PROCEDE.** A linha nos commits é exigência do aval. O selo pula commits sem linha, e seu validador rejeita o literal `projecao reconstruivel + horizonte (L-124)`. Conferi com o vocabulário da cópia. `Backfill / Reprocessing` é aceito e consta no ADR da L-124. Os 13 de 15 são do laudo, sem reconferência do histórico [nao verificado]. `bin/tests/test_lei_akita.sh:110`, `bin/tests/test_lei_akita.sh:337`, `bin/tests/test_lei_akita.sh:523`, `app/docs/PLANO-PADROES.md:75`, `laudo.md:5`.
- **VERIFICADOR — SEGUE.** O escopo é o das três voltas e a linha item a item antecede a suite. O laudo confirma que leitores, catraca, CHECK-DE-TELA e smoke ficaram pendentes. O checklist integral do 1010 fica [nao verificado]. `cadeia_20_volta_3_e_horizonte_1911_1815.txt:8`, `laudo.md:2`, `app/docs/BACKLOG.md:2044`.
- **PROIBIDO — PROCEDE.** “Entrar na impressão” precisa significar preservar o código do hash e deixar a versão já prevista mudar seu valor. “Rejulgar anterior” precisa significar **por efeito da obra**. A fila ainda julga por fato novo e o escritor atual roda em qualquer competência que passe pela guarda. O 2320 fornecido veda mexer no motor ou no pareador e tomar vaga ou suite da espinha. Os demais limites do CONSTRÓI e o cruzamento de diffs com F5 ou FERIADO ficam [nao verificado]. `app/docs/LEIS.md:145`, `app/ponto/services/cartorio.py:518`, `app/ponto/services/cartorio.py:1410`, `app/ponto/services/cartorio.py:1532`, `app/ponto/services/cartorio.py:1991`, `aval_1010.txt:1`, `cadeia_20_volta_3_e_horizonte_1911_1815.txt:9`.

**Exportada, dinheiro e `!`.** O teste real é K6 por empresa e competência, pela existência de TXT vigente e não invalidado. A cópia registra medições antigas com exportadas até a 09 e registra a 10 como paga fora do sistema nas empresas 2, 3 e 4. A exportação vigente de cada empresa no horizonte hoje é [nao verificado]. A relavra escreve diretamente `CelulaDia` e pode emitir `EventoCelula`. O recálculo é quem grava `FechamentoMensal`. A relavra também pode mudar veredito e protestos, que a folha lê para reter. L-113 retirou a recusa técnica de recálculo. O gravado exportado segue sob L-092 e a regra consolidada exige `!` e porta própria se houver movimento. Zero rubricas no cálculo e zero campos gravados são duas provas distintas. `app/folha/export.py:556`, `app/folha/export.py:588`, `app/ponto/services/cartorio.py:224`, `app/docs/CORTES.md:529`, `app/ponto/portas/celula.py:941`, `app/ponto/portas/celula.py:1181`, `app/folha/export.py:1273`, `app/ponto/services/fechamento.py:1137`, `app/docs/LEIS.md:125`, `app/docs/LEIS.md:155`, `CLAUDE.md:404`.

### PRONTO na cópia

| Item | Veredito e motivo |
|---|---|
| **1** | **PROCEDE.** O registro termina em v4. `app/ponto/services/cartorio.py:161`. |
| **2** | **PROCEDE.** A distinção existe no dado, mas a projeção hoje entrega horas e o template só as imprime. `app/ponto/services/cartorio.py:963`, `app/escala/services/leitor_celula.py:420`, `app/templates/ponto/espelho.html:242`. |
| **3** | **PROCEDE.** Há função de censo, sem recorte interno por horizonte e sem chamada no comando. Zero [nao verificado]. `app/ponto/services/cartorio.py:1001`, `app/ponto/management/commands/processar_cartorio.py:61`. |
| **4** | **PROCEDE.** As quatro medidas publicadas na árvore final e no caminho pedido ficam [nao verificado]. O registro anterior traz outro commit e relata 19 mudanças na segunda passada daquele ensaio. `app/docs/RELATO.md:5`, `app/docs/RELATO.md:16`, `laudo.md:3`. |
| **5** | **PROCEDE.** O juiz existe e é chamado na lavra. Permanecem pendentes o registro e o corte. `app/escala/orfa_classe.py:29`, `app/ponto/services/cartorio.py:991`, `laudo.md:4`. |
| **6** | **PROCEDE.** A publicação do PRONTO integral, item a item, antes da suite fica [nao verificado]. `cadeia_20_volta_3_e_horizonte_1911_1815.txt:8`. | [nao verificado]

**Pela metade, faltam 6 itens adicionais comprovados.** A contagem acima cobre os seis itens acrescentados pelo 1815. O total com o PRONTO integral do 1010 fica [nao verificado]. `cadeia_20_volta_3_e_horizonte_1911_1815.txt:10`, `app/docs/PROMPTS.md:2616`.

### Quem escreve e quem lê

- **`ata['razoes_orfas']`.** `_lavrar_razoes_sobras` escreve a chave nos dois ramos do cartório e `lavrar_veredito` com `carimbar` a persiste. `medir_razoes_sobras` lê a chave e tem chamadas em testes. Na cópia, zero chamador de produção do contador e zero leitor de produção da chave encontrado. `app/ponto/services/cartorio.py:959`, `app/ponto/services/cartorio.py:998`, `app/ponto/services/cartorio.py:1015`, `app/ponto/services/cartorio.py:1410`, `app/ponto/services/cartorio.py:1991`, `app/ponto/portas/celula.py:1090`, `app/ponto/portas/celula.py:1201`, `app/escala/tests/test_cadeia20_costura.py:52`, `laudo.md:2`.
- **`ata['regra']` e `versao_para`.** Zero escritor ou leitor dessa chave na ata encontrado. A versão entra como `regra:N` no hash de `CelulaDia.impressao`. Em produção, `_regra_do_dia` é o único chamador direto encontrado. Ele alimenta os dois ramos. Há leitores em testes. Chamados e a migração de identidade citam o horizonte em comentários, com piso próprio. `app/ponto/services/cartorio.py:749`, `app/ponto/services/cartorio.py:518`, `app/ponto/services/cartorio.py:1310`, `app/ponto/services/cartorio.py:1375`, `app/ponto/services/cartorio.py:1520`, `app/ponto/portas/celula.py:1195`, `app/ponto/tests/test_o219_versao_da_regra.py:85`, `app/chamados/models.py:2582`, `app/chamados/migrations/0053_pergunta_unica_por_familia.py:8`.

## 19/11 17:47 · sessao 7f0d1e40588a · E4C-F5-CONSTROI · verificador · origem=Code

# AVAL-DEVOLVIDO — E4C-F5-CONSTROI, aval 1735

**1 DESENHO — PROCEDE. 6 RED — PROCEDE em (b). 7 ADAPTAÇÕES DE TESTE — PROCEDE.** O bloqueio é a entrada em vigor dos juízes puros na A com chamadores crus em B/C, a falta de uma base definida para a catraca e de cenários nomeados para os REDs, e uma lista de adaptações que deixa de fora asserções incompatíveis com o desenho. `respostas_f5_constroi_1911_1735.txt:2–4`, `:8–11`, `chamados/juizes.py:1389–1397`, `:1559–1564`, `chamados/tests/test_f1nova_b2_leitores.py:87–97`.

Conferi a cópia somente leitura. A identidade do commit, da ref e do merge está [nao verificado]. O wrapper documenta a extração da árvore por hash com `git archive`. `bin/codex_pergunta.sh:304–320`.

## 1 DESENHO — PROCEDE

**(a) SEGUE quanto ao escopo declarado.** `:2004` monta `LeituraDoAto` e `:2006` entrega ao juiz o **texto do ato**, incluindo o passivo incorporado pela leitura. A lista exige também ausência de validação e materialização e aplica a tranca, salvo `ignorar_tranca=True`. `estado` monta a mesma leitura em `:3235` e passa o **texto do ato** ao juiz em `:3241` e `:3244`. Para decidir o ramo, `estado` ainda chama C6 com `self` em `:3240`, caminho que usa **texto da projeção** e pode reler o ato. `chamados/models.py:1998–2007`, `:3233–3244`, `:4295–4326`, `chamados/juizes.py:1389–1411`.

- **CSF e Pauta leem `sem_veredito_entre`.** A função do CSF chama a lista para o pai e para o destino, ambas com `ignorar_tranca=True`, e devolve os PKs ordenados. A medição da tranca também pede essa lista com `True`, via `respostas_sem_veredito`, e ainda chama C6 em dois pontos e `pode_fechar` no fechamento de disputas. Nenhuma dessas duas funções lê `PerguntaDisputa.estado`. A API e o vigia leem `estado`. `ponto/supra_juiz.py:552–588`, `chamados/services/competencia_trancada.py:119–144`, `:164–183`, `chamados/models.py:1979–1980`, `api/views.py:2566–2585`, `chamados/services/vigia_fio.py:256–260`.
- **Alcance de escrita.** A lista guarda `fechar`, o CSF e a retenção da retratação. Em competência trancada, `medir` monta `decidir_pelo_dp`; no ramo de aplicação, `_pauta_do_dp` lê essa lista e pode gravar a Pauta por `pautas.services.escrever`. Esses efeitos chegam pelas portas existentes. Seu resultado em produção e eventual efeito no gravado de exportada ou em dinheiro estão [nao verificado]. `chamados/models.py:2009–2015`, `:2048–2058`, `:2095–2099`, `ponto/management/commands/supra_juiz.py:257–287`, `chamados/juizes.py:2124–2129`, `chamados/reconciliador.py:439–452`, `chamados/services/competencia_trancada.py:137–139`, `:188–257`, `pautas/services.py:180–185`, `CLAUDE.md:265–268`, `:401–417`.

**(b) SEGUE com a descrição corrigida.** `values_list` produz uma tupla de cinco colunas. O juiz recebe um `SimpleNamespace` montado com `p[3]` e `p[4]`, o instante e o texto **da projeção**. `verbos.py` consome o par de conjuntos produzido pelo placar. Já existe a entrada `E4D-PROJECAO-UM-ESCRITOR` para o complemento nominal previsto no aval. O registro efetivo está [nao verificado]. `chamados/services/raiox.py:135–158`, `chamados/services/verbos.py:34`, `docs/BACKLOG.md:2023`, `respostas_f5_constroi_1911_1735.txt:3`.

**(c) PROCEDE.** A varredura AST da cópia encontrou **41 expressões diretas** dos dois juízes. A linha 15 da matriz aponta `reconciliador.py:1130`, onde se ajusta o relógio. A chamada está em `:1133`. Pela regra do arquivo do chamador, a distribuição é esta. `respostas_f5_constroi_1911_1735.txt:3–5`, `chamados/reconciliador.py:1127–1137`.

| Volta ou dono | Linhas da matriz A e chamadas na cópia |
|---|---|
| **A — 12** | 3–5 `chamados/juizes.py:351`, `:1411`, `:1430`. 14–15 `chamados/reconciliador.py:366`, `:1133`. 20–21 `chamados/services/disputa_emissao.py:1853`, `:2023`. 22–24 `chamados/services/fio.py:712`, `:1287`, `:1300`. 30 `chamados/services/pergunta_no_app.py:86`. 41 `ponto/services/cartorio.py:1689`. |
| **B — 20** | 1–2 `api/views.py:2596`, `:2640`. 16 `chamados/services/acoes_disputa.py:134`. 17 `chamados/services/becos.py:31`. 18–19 `chamados/services/declaracao_texto.py:418`, `:688`. 25–29 `chamados/services/materializacao.py:192`, `:259`, `:290`, `:885`, `:1300`. 32–33 `chamados/services/validacao.py:682`, `:902`. 34 `chamados/services/vigia_fio.py:636`. 35 `chamados/views.py:1275`. 36–37 `chamados/views_cobrar.py:170`, `:178`. 38 `colaboradores/services/ficha.py:172`. 39 `escala/services/cadastro_realidade.py:502`. 40 `folha/previa.py:142`. |
| **C — 4** | 6–8 `chamados/management/commands/auditar_invariantes_chamados.py:178`, `:597`, `:1671`. 9 `chamados/management/commands/dedup_perguntas_fato.py:76`. |
| **Opus — 4** | 10–13 `chamados/models.py:2006`, `:3241`, `:3244`, `:3791`. `:2004` e `:3235` preparam os argumentos. |
| **Exceção — 1** | 31 `chamados/services/raiox.py:147`. |

- **Os 55 indiretos são entradas, não 55 conversões.** Contei 53 expressões Python para `pergunta_viva`, incluindo a implementação da propriedade `.viva`, e os dois usos externos da propriedade. No cálculo, `:1411` recebe a pergunta e `:1430` recebe o par já recortado pelo horizonte. Memo quente pode devolver antes do cálculo. Validação, tranca e via terminal também podem sair antes de `:1430`. A matriz tem referências deslocadas no reconciliador após o merge. `chamados/models.py:3629`, `templates/chamados/partials/_card_atendimento.html:77`, `chamados/management/commands/arquivar_competencia_encerrada.py:158`, `chamados/juizes.py:887–898`, `:1398–1430`, `chamados/reconciliador.py:711`, `:808`, `:1137`, `:1645`.
- **Pureza e tipo próprio conferem com o 1710 e o 1510.** Hoje ambos fazem `isinstance` e chamam `DecisaoPergunta.leitura` no ramo ORM. A projeção temporal também depende da constante de via do modelo e lê cinco atributos. `LeituraDoAto` tem três campos e devolve o texto do ato. Remover só os ramos ORM deixaria a constante do modelo no juiz temporal. O contrato da composição precisa preservar materialização, via, o corte por `agora` e o par já recortado entregue em `:1430`. `chamados/juizes.py:1382–1430`, `:1551–1564`, `chamados/models.py:19`, `:2580`, `:2701`, `:4308–4326`, `e4c_f5_constroi_a_1911_1710.txt:4`, `respostas_f5_e_c7_1911_1510.txt:2–3`.
- **O literal da sequência A/B/C quebra a autoridade do instante.** Se a A retira a leitura do ato do corpo dos juízes, as **24 chamadas ORM** programadas para B/C passam a entregar os instantes das colunas até sua volta. Isso também alcança o temporal de C11 em `:502`, enquanto K2 e o RED de C11 estão em A. Falta dizer em que commit entram em vigor os corpos puros e a conversão de **todas** essas entradas, com a suíte verde exigida entre voltas. Além disso, a linha 40 e K23 pedem edição em `folha/previa.py`. O 0815 proíbe tocar folha e a exceção do 1710 descreve carga em lote, enquanto a linha 40 é a própria chamada ao juiz. Falta autorização nominal para a edição em folha e para eventuais mudanças de rota ou partição fora do recorte declarado. `chamados/juizes.py:1389–1397`, `:1559–1564`, `escala/services/cadastro_realidade.py:460–466`, `:502`, `escala/tests/test_f3_beco1_folga_na_lista.py:395–402`, `folha/previa.py:39–43`, `:64–67`, `:139–146`, `e4c_f5_constroi_1911_0815.txt:3`, `e4c_f5_segue_as_tres_voltas_1911_1435.txt:16`, `e4c_f5_constroi_a_1911_1710.txt:3–8`, `respostas_f5_constroi_1911_1735.txt:3–4`.

**(d) PROCEDE.** Na cópia original são **41 chamadas sem compositor, 40 fora da exceção do placar**. O commit do Opus antecede a A e já tem ordem para trocar quatro expressões. “Medida na abertura da A” deixa indefinido se a referência é o hash original ou a árvore entregue pelo Opus. Para a catraca só encolher, falta fixar a árvore de referência, impedir entrada nova mesmo quando outra sai e definir como o AST reconhece a origem de uma leitura atribuída a variável e a derivação temporal de `:1430`. O texto da exceção deve identificar o adaptador de `raiox.py:147`, que é quem entra no juiz. O 1735 dá autorização nominal para esse sítio no selo novo. O selo de leitores crus do 0815 tem outro objeto e seu veto geral a allowlist continua pertinente ali. `respostas_f5_constroi_1911_1735.txt:3–4`, `chamados/models.py:2004–2006`, `:3235–3244`, `:3791`, `chamados/juizes.py:1411–1430`, `chamados/services/raiox.py:135–147`, `e4c_f5_constroi_1911_0815.txt:3`.

**(e) PROCEDE.** Há entradas reais de leitura. `q.estado` devolve um **rótulo**. `DisputaSupervisao.sem_veredito_entre([q], ignorar_tranca=…)` devolve uma **lista**, da qual se observa se o PK aparece. Falta fixar no literal a comparação do rótulo inteiro e da presença do PK nas duas modalidades da tranca. Medir só com o padrão pode ocultar mudança para CSF, retratação e Pauta, que pedem `True`. O `MUDAM=0 em 51.563` fornecido é da property `respondida`. Os dois resultados estendidos, os PKs e a identidade da sombra estão [nao verificado]. O contador de divergência literal de texto mede outra coisa. `chamados/models.py:1982–2007`, `:3225–3244`, `:3785–3791`, `ponto/supra_juiz.py:573–574`, `chamados/juizes.py:2127–2129`, `chamados/services/competencia_trancada.py:123–139`, `chamados/services/vigia_texto_resposta.py:8–18`, `e4c_f5_constroi_a_1911_1710.txt:10`, `respostas_f5_constroi_1911_1735.txt:3`, `:9`.

**Quem escreve e quem lê os dados da composição**

| Campo ou valor | Escrita e leitura |
|---|---|
| Texto e instante da projeção | Resposta humana e automática gravam o par. A trilha pode gravar o instante sobre texto existente, e reabertura/recusa limpa o par pela porta de lote. Os juízes hoje leem o texto corrente no ramo ORM. O placar lê as duas colunas. `chamados/services/disputa_emissao.py:1481–1496`, `:1527–1534`, `:1617–1656`, `:1680–1710`, `chamados/juizes.py:1395–1397`, `:1559–1564`, `chamados/services/raiox.py:135–147`. |
| Instantes e texto do ato, mais passivo | Resposta, validação, devolução e recusa/reabertura alimentam `DecisaoPergunta.registrar`, que insere as linhas e invalida o cache recebido. `leitura` combina as linhas com o passivo, que lê `respondida_em`, `validada_em`, `validada_por_id` e texto da projeção. Os leitores afetados estão em `:2004`, `:3235` e `:3791`. `chamados/services/disputa_emissao.py:1532–1534`, `:1639–1656`, `chamados/services/validacao.py:161–175`, `:269–282`, `chamados/models.py:2004–2006`, `:3235–3244`, `:3791`, `:4251–4269`, `:4295–4347`. |
| `materializada_em` e `via_resolucao` | `_escrever_carimbo` atribui os campos. `carimbar`, `carimbar_identidade` e `descarimbar` os gravam pelas portas existentes. A projeção temporal lê ambos, e `estado` e a lista sem veredito leem a materialização. `chamados/models.py:3293–3297`, `:3320–3348`, `:3373–3408`, `:2005`, `:3236–3244`, `chamados/juizes.py:1397–1407`. |

## 6 RED — PROCEDE em (b)

**(a) SEGUE.** O teste captura o **SQL total da porta real** com 1 e 3 PKs. Hoje exige `Q(3) = 3 × Q(1)` e `Q(1) ≤ 8`. É teto **por candidata**. A porta abre transação, trava e relê por candidata, e C6 lê a relação `decisoes`. O 1735 autoriza nominalmente `Q(3)/3 = Q(6)/6 ≤ 9` para O261, inclusive diante do veto geral do 1710. A triagem relata 9 no teste anterior. O SQL da sonda e o resultado em 3 × 6 estão [nao verificado]. A fixture validada exercita o caminho de recusa. Em produção, `desfazer_carimbo_sem_lastro` lê carimbo, via e C6, e pode chamar `descarimbar` e reabrir a disputa. O aval aqui muda a métrica do teste, com `DecisaoPergunta.leitura` como fronteira da leitura do ato. `chamados/tests/test_o261_julgamento_desfaz_carimbo.py:187–205`, `chamados/services/relavra_via.py:215–262`, `chamados/models.py:3399–3409`, `:4308`, `e4c_f5_constroi_a_1911_1710.txt:8–9`, `:13`, `respostas_f5_constroi_1911_1735.txt:8–11`, `triagem.md:14`.

**(b) PROCEDE.** As **cinco funções L** existem. Três pertencem à A ou ao seu commit preparatório, `CargaDaTarefa.__init__` (K1), `alimentar_portas` (K2) e `_ancoradas` (K15). Duas pertencem à B, `perguntas_vivas_de` (K4) e `fio_vivo` (seis K). O predesenho lista **22 Q de B**, incluindo K41/K42 no commit do Opus, e **25 M de C**, mais a sobra. O 1735 pede três mutantes Q e três M, mas **não nomeia os seis IDs**, nem diz em que suíte se prova a mordida dos três L de A. `chamados/services/tarefa_do_dia.py:518–540`, `chamados/juizes.py:1512–1526`, `:1639–1672`, `chamados/models.py:343–359`, `:1542`, `:2057`, `chamados/management/commands/dedup_perguntas_fato.py:74–76`, `e4c_f5_constroi_a_1911_1710.txt:6–8`, `respostas_f5_constroi_1911_1735.txt:7–8`.

- **A igualdade total não fecha para todos os K sob a instrução de só carregar decisões em lote.** K40 faz um `.get(pk=pk)` dentro do laço antes de julgar, portanto o SQL total já cresce por pergunta mesmo com decisões previamente disponíveis. `fio_vivo` faz `any`, que pode julgar apenas a primeira pergunta nas duas medidas. A sobra faz uma carga por grupo, logo o total depende também da quantidade de grupos. Faltam a unidade de captura por cenário, a prova de que 3 e 6 perguntas foram efetivamente julgadas, e a autorização de qualquer alteração de carga além das decisões. O selo AST verifica formato de chamada. Ele não prova custo SQL nem substitui a mordida de cada caso do 1510. `chamados/management/commands/migrar_data_evento_dia_do_turno.py:448–467`, `chamados/juizes.py:1517–1526`, `chamados/management/commands/dedup_perguntas_fato.py:74–82`, `respostas_f5_e_c7_1911_1510.txt:3–4`, `e4c_f5_constroi_a_1911_1710.txt:7–9`, `respostas_f5_constroi_1911_1735.txt:3`, `:8`.
- **Escrita e alcance dos cenários.** As cargas leem `PerguntaDisputa`; o consumo das decisões ocorre em `leitura`. K14 percorre exportadas com `executar=False`. A sobra tem relatório de leitura e um ramo `--apply` que despacha ao backfill. O RED precisa identificar qual caminho mede. Prova de custo em leitura não verifica equivalência de valores exportados, dinheiro ou execução das portas escritoras [nao verificado]. `chamados/models.py:4308–4326`, `chamados/management/commands/backfill_fio_morto_e4b.py:153–175`, `chamados/management/commands/dedup_perguntas_fato.py:89–106`, `CLAUDE.md:265–268`, `:401–417`.

## 7 ADAPTAÇÕES DE TESTE — PROCEDE [nao verificado]

| Subitem | Veredito |
|---|---|
| **(i) SEGUE** | São os dois REDs 8. A fixture tenta criar o texto sem instante antes de `atos` chamar `D.registrar`. `save` a rejeita antes das capturas. O segundo RED está em `:167–180`, com o medidor em `:151–165`. A autorização é para a fixture. Contagem 1 × 5, zero SQL no cálculo com prefetch e conferência do cache fazem parte da prova. `chamados/tests/test_e4c_f5_volta2_juizes.py:27–30`, `:118–180`, `chamados/models.py:3680–3683`, `:3707–3712`, `chamados/services/fio.py:1718–1721`, `respostas_f5_constroi_1911_1735.txt:9`. |
| **(ii) SEGUE** | `:37–104` chama os juízes com ORM cru. A fixture escreve atos por `registrar` e altera a projeção por `update` explícito. Os asserts leem a tupla e o booleano, incluindo passivo, reabertura, futuro e texto em branco. O caso seguinte já usa objeto sintético e vigia que a leitura do ato jamais seja chamada para ele. `chamados/tests/test_e4c_f5_volta2_juizes.py:27–116`. |
| **(iii) SEGUE** | O mutante exige uma ocorrência literal de `_replace` e troca o argumento de `respondida_sem_texto` pela pergunta inteira renomeada para `questao`. O mutante atualizado precisa continuar chegando ao assert de **pergunta inteira** do selo 3d. Falhar na busca da âncora não prova a mordida. A transformação opera sobre texto em memória. `chamados/tests/e4c_f5_volta2_mutantes.py:1–9`, `:12–39`, `chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:187–196`, `:220–227`, `chamados/services/fio.py:187–192`, `e4c_f5_segue_as_tres_voltas_1911_1435.txt:12`. |
| **(iv) PROCEDE** | O teste escreve uma `RESPOSTA` com `ato_texto='07:13'` e atesta a projeção vazia. O assert atual exige `True` para **sete** leitores. Pelo desenho, `sem_veredito_entre`, CSF, irmãs, `estado == RESPONDIDA` e a property passam a dar `False` nessa fixture. `resposta_display` continua lendo o texto do ato, e `q_vivas` continua incluindo a pergunta pelo ramo largo da célula furada. Falta explicitar as expectativas por leitor e conservar a prova do instante e da validação pelo ato. Inverter os sete quebraria o contrato. O `MUDAM=0` da property não certifica os outros leitores. `chamados/tests/test_e4c_f2_leitura_do_ato.py:313–365`, `:490–509`, `chamados/models.py:2004–2007`, `:3235–3244`, `:3609–3616`, `:3791`, `:3815`, `chamados/juizes.py:2127–2129`, `respostas_f5_e_c7_1911_1510.txt:2–3`, `respostas_f5_constroi_1911_1735.txt:9`. |
| **(v) SEGUE** | A adaptação nominal é a do O261 descrita em 6(a). A captura lê o total de SQL da porta com PKs de fixtures validadas. `chamados/tests/test_o261_julgamento_desfaz_carimbo.py:187–205`, `respostas_f5_constroi_1911_1735.txt:8–9`. |

**O que falta na autorização de 7.** O teste B2 exige que `respondida` chame o juiz exatamente com `DecisaoPergunta.leitura(p)`. O tipo próprio exigido pelo 1735 muda justamente esse argumento. O selo de `_ancoradas()` mede **exatamente uma consulta** com retorno não vazio, enquanto a carga de decisões de K15 no mesmo caminho acrescenta um SELECT por lote. O 1710 admite remedir custo constante por lote, mas a lista fechada do 1735 só nomeia o O261 entre as adaptações de métrica. Tarefa e C11 ainda medem 1 × 3 e 1 × 5 dias. Falta dizer se o RED 3 × 6 entra como caso novo ou se os dois testes existentes podem ser adaptados. `chamados/tests/test_f1nova_b2_leitores.py:75–97`, `chamados/tests/test_e4c_f2_leitura_do_ato.py:534–551`, `chamados/models.py:343–359`, `chamados/tests/test_e2_tarefa_do_dia.py:913–938`, `escala/tests/test_f3_beco1_folga_na_lista.py:378–402`, `e4c_f5_constroi_a_1911_1710.txt:8–9`, `respostas_f5_constroi_1911_1735.txt:3`, `:9–11`.

**As 11 famílias ainda precisam passar com seus dublês atuais.** Há harness que recompila C6 com globais próprios e há stub de `chamados.models` que fornece só `ChamadoColaborador` para a rota de C11. A pureza do juiz resolve a dependência de tipo dentro dele. A compatibilidade desses chamadores com uma nova dependência de composição e o verde das famílias estão [nao verificado]. As alterações de fixture, mutante e métrica são de teste. Qualquer efeito das mudanças de produção sobre as portas de fechamento, Pauta, exportada ou dinheiro continua sujeito às cercas e às medidas correspondentes. `chamados/tests/e3_c8b_apoio.py:83–120`, `chamados/tests/test_e3_r3_c6_sem_banco.py:284–312`, `chamados/tests/test_e4b_c4v14_memoria.py:43–53`, `chamados/tests/test_e4b_classe_e_memoria.py:377–394`, `e4c_f5_constroi_1911_0815.txt:3`, `CLAUDE.md:265–268`, `:401–417`.

## 19/11 17:15 · sessao eaf908cd6bf6 · E4C-F5-CONSTROI · sonda · origem=Code

- **Hoje.** `PerguntaDisputa.respondida` chama `pergunta_respondida(DecisaoPergunta.leitura(self))`. A leitura usa o instante e o texto da resposta vigente na sequência, incluindo o legado da projeção quando falta a linha correspondente. `app/chamados/models.py:3785–3791`, `app/chamados/models.py:4295–4326`
- **Novo.** A composição descrita no item 2 do 1510 preserva `leitura.respondida_em` e troca apenas `leitura.resposta_colab` por `p.resposta_colab` antes de chamar o mesmo juiz. O `RELATO` registra essa composição no fio. Na cópia disponível, `fio.py:189–192` contém o início de `montar_chamados_fio`. A implementação da sonda e a confirmação do hash pelo Git ficam [nao verificado]. `app/docs/RELATO.md:11`, `app/chamados/juizes.py:1544–1552`, `app/chamados/services/fio.py:189–192`

**Medida válida pela saída fornecida.** `ERROS=0` e `UNIVERSO=51563` é igual a `PERCORRIDAS=51563`. `DADO RASPADO:4`

Transcrição literal de `DADO RASPADO:3–9`:

```text
BANCO=sombra
UNIVERSO=51563 PERCORRIDAS=51563 ERROS=0
MUDA_TRUE_PARA_FALSE=0
MUDA_FALSE_PARA_TRUE=0
PKS_TRUE_PARA_FALSE=[]
PKS_FALSE_PARA_TRUE=[]
VEREDITO=VALIDO MUDAM=0
```

MUDAM=0

_7429 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
