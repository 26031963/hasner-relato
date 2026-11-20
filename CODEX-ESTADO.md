# CODEX-ESTADO

> Ultima resposta processada: **20/11/2026 02:26** · publicado em **20/11/2026 02:27** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 20/11 02:26 · sessao fbd5107dcd84 · E4C-F5-CONSTROI · verificador · origem=Code

**BLOQUEIA o pouso de E4C-F5-CONSTROI.** A face SQL e o compositor dão respostas diferentes para a mesma pergunta, o C6 aplica o horizonte depois de reduzir a sequência de atos, e o mutante K16 para antes de alterar a função real. Os três defeitos são verificáveis na cópia. `app/chamados/juizes.py:1408,1588,1765,1772` · `app/chamados/models.py:4333,4360` · `app/chamados/tests/e4c_f5_voltaA_mutantes.py:35`

Comparei em leitura as cópias de `eb1e49c1e` e `aa3bdd88c`. Os blobs conferidos de `models.py`, `juizes.py` e `api/views.py` batem com o `prod.diff`. A comparação das cópias reproduz os 96 arquivos do `stat.txt`, incluindo os 50 de teste. A identidade dos commits por metadados Git, a ordem dos commits, os runs de suíte e os efeitos em produção ficam **[nao verificado]**. A contagem final usa os nove quesitos principais.

## 1. Escopo — BLOQUEIA

- **Pureza literal do aval.** O juiz temporal ainda depende do modelo importado no módulo. Trecho exato `if via not in PerguntaDisputa.VIAS_FORA_DO_FATO:`. A catraca abre uma exceção para essa referência. O aval A pede os dois juízes “sem importar modelo”; o aval nominal que liberaria essa exceção está **[nao verificado]**. `app/chamados/juizes.py:11,1401` · `app/chamados/tests/test_e4c_f5_selo_chamada.py:166`
- **Mudanças que precisam de autorização além dos lotes.** O diff troca o critério de M1 de presença do instante para instante **e** texto, passa a devolver o instante da trilha no JSON e cria o memo do modal. Trechos exatos `m1 = all(a is None and respondida_no_horizonte(p) for p, a in ancs)`, `'respondida_em': leitura.respondida_em.isoformat() if leitura.respondida_em else None` e `context.render_context.setdefault(respostas_que_o_reabrir_apaga_no_modal, {})`. O aval A autoriza os pontos da tabela para carga em lote e preserva partição e rota. A tabela nominal e o aval complementar desses três efeitos estão **[nao verificado]**. A autorização do contador de texto tem registro no `RELATO.md:141`. `app/escala/services/cadastro_realidade.py:507` · `app/api/views.py:2581` · `app/chamados/templatetags/chamados_tags.py:234` · `app/docs/RELATO.md:141`
- **Leitores de decisão vigente que escapam da composição.** A exceção de `raiox` aparece na allowlist do teste. O trecho é `pergunta_respondida(SimpleNamespace(respondida_em=p[3], resposta_colab=p[4]))`. Sua autorização literal nos avais fornecidos está **[nao verificado]**. A mesma divergência de fonte fica nas duas faces SQL da resposta, no estoque de resposta vazia usado por `porta_alcancavel_do_dia` e no ramo B do vigia. Os pontos que consomem essas faces para escolher o universo incluem `q_vivas`, corrigíveis, fila de validação, reabertura, calendário, irmã do intervalo e lote de materialização. `app/chamados/services/raiox.py:135,147` · `app/chamados/tests/test_e4c_f5_selo_chamada.py:23` · `app/chamados/juizes.py:1580,1588,1599,1752,1765,1772` · `app/chamados/models.py:1552,3622` · `app/chamados/views.py:1412` · `app/colaboradores/services/calendario.py:505` · `app/chamados/services/materializacao.py:146,2109` · `app/chamados/services/vigia_fio.py:635`
- **Outras leituras diretas com efeito de decisão ou apresentação.** `_slot_esperado` consulta o instante da coluna depois da projeção temporal. `validar_pergunta` e `recusar_pergunta` mantêm a validação da coluna como guarda. A irmã do intervalo e o lote de materialização também usam a coluna. O reconciliador seleciona órfãs pela coluna; S128 faz o mesmo na passagem sem credor. O modal, o cartão do colaborador e o cartão de pergunta exibem carimbos da coluna. A cobertura dessas leituras pela lista fechada dos avais fica **[nao verificado]**. `app/chamados/juizes.py:351,353,365` · `app/chamados/services/validacao.py:685,898` · `app/chamados/services/materializacao.py:200,202,2111,2128` · `app/chamados/reconciliador.py:680,1101` · `app/chamados/services/disputa_emissao.py:2085` · `app/templates/chamados/partials/modal_fio.html:219,228,345` · `app/templates/chamados/partials/_card_atendimento.html:106,143` · `app/templates/chamados/partials/_pergunta_card.html:20,23,32,42`

Entre os leitores diretos da relação `decisoes`, encontrei a seleção simulada da ressuscitação, a foto do autor da validação na ressuscitação real, o histórico de respostas anteriores, o marcador de devolução U103, a carga e a seleção do último ato de T, a foto E4b, o contador e a checagem de atos no backfill. `historico`, `resposta_display`, fotos de silêncio/retratação e o contador de divergência passam por `leitura`. A confirmação nominal de cada exceção contra os K fica **[nao verificado]**. `app/chamados/models.py:1591,1710,2112,2244,3826,3840,4333` · `app/chamados/services/materializacao.py:277` · `app/chamados/services/tarefa_do_dia.py:388,398,429,545` · `app/chamados/services/foto_e4b.py:280` · `app/chamados/services/historico.py:98` · `app/chamados/services/vigia_texto_resposta.py:16` · `app/chamados/management/commands/backfill_decisoes_pergunta.py:126,342` · `app/chamados/management/commands/validar_classe_a.py:63,83`

## 2. Escritores, leitores e significado — BLOQUEIA

| Campo ou valor | Escritores e leitores conferidos |
|---|---|
| `respondida_em`, `resposta_colab` | Resposta humana, automática, recuperação por trilha e reabertura em `app/chamados/services/disputa_emissao.py:1481,1527,1532,1617,1639,1656,1682,1705`. Nascimento, carimbo de fato e limpeza por ressuscitação em `app/chamados/models.py:1721,3229,3350`. Limpeza do estoque L-130 em `app/chamados/services/validacao.py:408,440`. A sequência reduz os atos em `app/chamados/models.py:4333,4350`. As faces SQL, a integridade do par e a ordenação da fila leem colunas em `app/chamados/juizes.py:1538,1580,1588,1599,1774` e `app/chamados/models.py:3716`. |
| `validada_em` | Validação e devoluções em `app/chamados/services/validacao.py:161,168,173,273,279,349,354`. Nova resposta humana limpa a assinatura em `app/chamados/services/disputa_emissao.py:1494`. Carimbo e ressuscitação em `app/chamados/models.py:3354,1721`. Leitores da trilha em `app/chamados/models.py:2025,3260,3571,4345` e `app/chamados/juizes.py:1409`. Leitores da coluna nas guardas de `app/chamados/services/validacao.py:685,898`, `app/chamados/services/materializacao.py:200,2111,2128` e `app/chamados/services/disputa_emissao.py:2085`. |
| `materializada_em`, `via_resolucao` | Carimbo, identidade, descarimbo e limpeza em lote em `app/chamados/models.py:3319,3321,3388,3423,3511,3520`. A relavra tem escrita direta só da via em `app/chamados/services/relavra_via.py:125,127`. A leitura permanece na coluna em `app/chamados/models.py:2025,3262,3623,4362`, `app/chamados/juizes.py:1393`, `app/chamados/services/materializacao.py:263,292,878,891` e `app/chamados/services/historico.py:106`. As vias de identidade podem continuar no horizonte mesmo quando o carimbo futuro é ocultado. `app/chamados/juizes.py:1399` |
| `LeituraComposta` | Um produtor, `return LeituraComposta(lido.validada_em, lido.respondida_em, p.resposta_colab, p.materializada_em, p.via_resolucao)`. Consumidores de estado, vitalidade e retenção em `app/chamados/models.py:2024,3259,3816` e `app/chamados/juizes.py:352,1408`. Consumidores de app e UI em `app/api/views.py:2587,2601,2646`, `app/chamados/views.py:1275`, `app/chamados/views_cobrar.py:170,178`, `app/chamados/services/pergunta_no_app.py:88`, `app/colaboradores/services/ficha.py:173`. Consumidores de escrita e fila em `app/chamados/reconciliador.py:367,1136`, `app/chamados/services/fio.py:189,712,1288,1301`, `app/chamados/services/acoes_disputa.py:134`, `app/chamados/services/becos.py:32`, `app/chamados/services/declaracao_texto.py:418,688`, `app/chamados/services/disputa_emissao.py:1854,2025`, `app/chamados/services/validacao.py:682,902`, `app/chamados/services/materializacao.py:193,261,293,889,1305`, `app/ponto/services/cartorio.py:1689`. Consumidores de auditoria e prévia em `app/chamados/management/commands/dedup_perguntas_fato.py:76`, `app/chamados/management/commands/auditar_invariantes_chamados.py:179,598,1672`, `app/chamados/services/vigia_fio.py:637`, `app/escala/services/cadastro_realidade.py:498`, `app/folha/previa.py:143`. |
| Universo de `fechar` | `perguntas = list(self.perguntas.all().prefetch_related('decisoes'))` alimenta `_pend`, `vivas`, a lista de destinos da trava, o laço que carimba silêncio e os PKs da trilha. O escritor grava perguntas e depois o fechamento da disputa. `app/chamados/models.py:2071,2072,2081,2095,2104,2110,2122,2125` |
| Memo do modal | A tag guarda uma contagem por PK da disputa. A função chamada conta `perguntas_corrigiveis`; confirmação e botão leem o mesmo `n_apaga`. A rota constrói o render em `app/chamados/views.py:927`. `app/chamados/templatetags/chamados_tags.py:208,223,232,237` · `app/templates/chamados/partials/modal_fio.html:396,398,401` |

Três incompatibilidades concretas de significado:

1. **Instante vigente.** `return Q(respondida_em__isnull=False) & Q(resposta_colab__regex=_RE_TEM_TEXTO)` exclui uma resposta com instante só no ato e texto na projeção. A fila de validação e `perguntas_corrigiveis` perdem a linha antes do Python. No inverso, uma `REABERTURA` vigente pode limpar o instante da trilha e o SQL ainda aceitar a coluna velha. Há fixtures das duas divergências. `app/chamados/juizes.py:1588,1765,1772` · `app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:43` · `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:534`
2. **Horizonte inclusivo, feito em ordens diferentes.** O C6 executa `_lido = DecisaoPergunta.composta(p)` e só depois corta carimbos `> agora`. Com uma resposta em T1 e uma reabertura em T2, uma consulta entre T1 e T2 já recebe `respondida_em=None`. A carga de T corta os atos com `d.ato_em <= agora` **antes** de chamar `leitura`. O slot ainda mistura as duas fontes em `if respondida is None and p.respondida_em is not None:`. O GET passa `agora` ao C6, mas a partição e o JSON voltam a usar a composição corrente. `app/chamados/juizes.py:353,1394,1408` · `app/chamados/models.py:4342,4350` · `app/chamados/services/tarefa_do_dia.py:400,429,626` · `app/chamados/services/pergunta_no_app.py:77,88` · `app/api/views.py:2552,2581,2587`
3. **Texto e data.** Na mesma resposta JSON, `'resposta_colab': leitura.resposta_colab` vem da projeção e `'resposta_display': p.resposta_display` pode exibir o texto do ato. T também decide o dono usando `texto_vazio(ult.ato_texto)`. Os carimbos são instantes com `USE_TZ=True`; `data_evento` da conversa é o dia civil local, e o da pergunta de fato é o dia do turno. O fallback C6 ainda lê `date.today()` da máquina quando não recebe `hoje`. A ocorrência de divergências de texto ou fuso em produção está **[nao verificado]**. `app/api/views.py:2579` · `app/chamados/models.py:3011,3840` · `app/chamados/services/tarefa_do_dia.py:165` · `app/config/settings/base.py:93,95` · `app/chamados/juizes.py:337,597`

## 3. Cache e prefetch depois da escrita — BLOQUEIA

- **Memo do C6 ficou incompleto.** Trechos exatos `if _m is not None and _m[0] == _k: return _m[1]` e `_cache.pop('decisoes', None)`. A chave cobre os campos da projeção e a célula. Depois da F5, o resultado também depende da trilha. A porta `registrar` invalida o prefetch da instância que recebeu, mas deixa `_pv_memo` intacto e não alcança outras instâncias com o mesmo PK. Um segundo julgamento na mesma instância, com os mesmos campos projetados e um ato que muda a validação ou a resposta vigente, pode devolver o C6 anterior. `app/chamados/juizes.py:856,895,898,1408` · `app/chamados/models.py:4373,4379,4381`
- **`fechar` usa o universo pré-trava até a escrita.** Trecho exato `perguntas = list(self.perguntas.all().prefetch_related('decisoes'))`, seguido de `_pend = self.respostas_sem_veredito(perguntas=perguntas)` e `vivas = [q for q in perguntas if _pv(q)]`. Só depois entram `atomic()` e `trava_na_ordem`. Uma resposta gravada por outro ato entre a carga e a trava fica fora dos dois julgamentos. No ramo que carimba silêncio, a foto do texto volta a ler a sequência da instância que chegou com prefetch. A trilha do fechamento usa intencionalmente `vivas` como foto do antes. `app/chamados/models.py:2071,2081,2090,2095,2106,2112,2125`
- **Resolver e backfill.** S128 materializa ambos os lotes antes de reabrir credores, e o laço seguinte continua sobre as instâncias já carregadas. A passagem `_todas` e o C7 fazem releituras posteriores. O backfill relê o restante quando `res == 'reaberto'`, e o teste força uma escrita por outro objeto antes desse retorno. A interação com todas as cascatas reais e retornos está **[nao verificado]**. `app/chamados/services/disputa_emissao.py:1985,1992,2000,2080,2127` · `app/chamados/management/commands/backfill_fio_morto_e4b.py:137,140,143` · `app/chamados/tests/test_e4c_f5_voltaC_lotes.py:239,244,260`
- **Portas com releitura identificada.** O upload de foto faz `d.refresh_from_db()` depois da validação. A declaração aplicada chama `validar_pergunta` por PK, e o lote de materialização relê a pergunta sob trava. O reabrir do modal executa a porta antes de renderizar a resposta. O memo novo fica em `context.render_context`, com mapa construído no render. Não encontrei compartilhamento por request ou processo nesse caminho. O teste cobre render novo após reabrir, em outro request. A execução desse teste está **[nao verificado]**. `app/api/foto_recebida.py:74,76,81` · `app/chamados/services/declaracao_texto.py:694,697` · `app/chamados/services/validacao.py:677` · `app/chamados/services/materializacao.py:2120,2122` · `app/chamados/views.py:1393,1432` · `app/chamados/templatetags/chamados_tags.py:234` · `app/chamados/tests/test_e4c_f5_voltaC3_reabrir.py:103`

## 4. Classes de reprovação — BLOQUEIA

| Classe | Resultado |
|---|---|
| **a. Segundo juiz** | **BLOQUEIA.** A fila responde a pergunta de C21 pela face que ainda julga `respondida_em` da coluna. O registro permanece único, mas a resposta pode ser dupla. `app/core/juizes.py:305` · `app/chamados/juizes.py:1560,1588,1772` |
| **b. Segundo escritor** | **NAO BLOQUEIA pelo diff.** A sequência continua escrita por `registrar` e o diff acrescenta uma manipulação de cache em memória. A escrita direta de `via_resolucao` da relavra já existia na base. `app/chamados/models.py:1591,1594,4365,4375` · `app/chamados/services/relavra_via.py:125,127` |
| **c. Relógio fora da borda** | **BLOQUEIA.** `_agora_sonda = _tz_s.now()` e `_agora_r = _tz_r.now()` são duas leituras, separadas pela aquisição da trava. A sonda conserva `d.ato_em > _agora_sonda`; o ato que limpa a sequência é gravado com `_agora_r`. Uma linha entre os instantes pode sobreviver na sonda e ser apagada pelo ato. `app/chamados/models.py:1558,1592,1651,1702,1714` |
| **d. Derivação paralela** | **BLOQUEIA.** `_dec._result_cache = [d for d in p.decisoes.all() if d.ato_em > _agora_sonda]` simula uma reabertura recortando a relação. A autoridade ordena por instante e PK e aplica o ato. No empate exato, a sonda descarta todas as linhas; a leitura real ainda distingue o ato de maior PK. `app/chamados/models.py:1592,1714,4335,4339,4349` |
| **e. RED sem mordida** | **BLOQUEIA.** Executei em memória o `tirar_lote` em `_resolver_perguntas_por_chamado_resolvido`. `assert len(alvos) == 1` encontrou **2** e parou antes da mutação. Os sítios reais são os lotes S128 e da varredura sem credor. `app/chamados/tests/e4c_f5_voltaA_mutantes.py:35,60` · `app/chamados/services/disputa_emissao.py:1955,2080` |
| **f. Ordem de trava** | **BLOQUEIA pela validade da foto pré-trava de `fechar`.** A ordem C → Disputa → Chamado → Pergunta continua em `trava_na_ordem`. O diff consolida os dois julgamentos sobre a mesma foto anterior à trava, sem releitura sob ela. `app/chamados/models.py:2071,2090,2095,2876,2885,2898,2902` |
| **g. Dois significados** | **BLOQUEIA.** A tupla `LeituraComposta` é distinta de `LeituraDoAto`; a divergência fica nos consumidores, como os dois textos do mesmo item JSON e a guarda de validação da coluna após a guarda de resposta da trilha. `app/chamados/models.py:19,24,3840,4361` · `app/api/views.py:2579,2580` · `app/chamados/services/validacao.py:682,685` |
| **h. Manager, subconsulta ou anotação** | **NAO BLOQUEIA por manager novo.** O `prefetch_related` é local. A face de validação pré-existente lê `DecisaoPergunta` por `Exists`, inclusive em `_ancoradas`. A face de resposta é a divergência bloqueante do item 1. `app/chamados/models.py:364,3576,3578,3580,3582,4222` |
| **i. Pré-requisito** | **NAO BLOQUEIA por migration nova na cópia.** As migrations 0054 e 0055 estão na base e o deploy tem checagem de pendência antes do reload. A sequência efetiva de rebase com CADEIA-20, migrations, backfill e deploy desta obra está **[nao verificado]**. `app/chamados/migrations/0054_decisao_da_ressuscitacao.py:11,17` · `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:12,19` · `bin/deploy.sh:185,217,317,324` |

## 5. Adaptações de teste — BLOQUEIA [nao verificado]

A comparação dos 33 arquivos preexistentes encontrou zero queda na contagem de `assert` e `assert*`. Três módulos aumentaram a contagem, em **76 → 84**, **78 → 79** e **161 → 162**, respectivamente. O selo de igualdade literal do 2050, portanto, não fecha. `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:519,532,584,589` · `app/chamados/tests/test_f1nova_b2_leitores.py:97` · `app/chamados/tests/test_o261_vigia_fio.py:831`

| Classe | Arquivos preexistentes compatíveis pelo conteúdo |
|---|---|
| **i — Âncoras** | `app/chamados/tests/e3_mutantes.py:30`, `e3_r3_c6_mutantes.py:49,51`, `e4b_c12_ast.py:19`, `test_cef_c1_sem_banco.py:309`, `test_e4b_classe_e_memoria.py:40,51`, `test_e4c_f6f7_c7_sem_banco.py:113`, `app/core/tests/test_medida_turno_do_par.py:258,271,319`. A nona âncora c12, `462 → 469`, é deslocamento real da chamada e cabe na classe geral (i). `app/chamados/tests/e4b_c12_ast.py:20` · `app/chamados/models.py:469` |
| **ii/v — Dublês** | Em `app/chamados/tests/`: `e3_v8_apoio.py:31,167`, `e3_volta7_apoio.py:33,79,111`, `test_bug_par_c2_sem_banco.py:32,80`, `test_bug_par_sem_banco.py:170,193`, `test_cef_c1_sem_banco.py:88`, `test_disputa_s127.py:23`, `test_e1_v10.py:121`, `test_e3_c8b_sem_banco.py:28`, `test_e3_degrau_dono_e_porta.py:264,272,512`, `test_e3_pos_suite_sem_banco.py:16,54`, `test_e3_r3_c5_sem_banco.py:175`, `test_e3_r3_c6_sem_banco.py:295`, `test_e3_r3_c7_sem_banco.py:181,191`, `test_e3_tarefa_admin_sem_banco.py:87,112,185,202`, `test_e4a2_c5_memoria.py:131`, `test_e4b_c4v7_memoria.py:359,381`, `test_e4b_classe_e_memoria.py:193,286,336`, `test_estado_aplicacao.py:41`, `test_pergunta_viva.py:12`. Também `app/core/tests/test_fio_mudo_placar.py:127` e `app/ponto/tests/test_supra_juiz_executor.py:22`. |
| **iii — Tipo** | `LeituraDoAto → LeituraComposta` cabe em (iii). O `assertIs` adicional altera a contagem do selo. `app/chamados/tests/test_f1nova_b2_leitores.py:95,97` |
| **i com reforço estrutural** | `test_contract_dedup_status.py` troca busca de string por uma atribuição AST com `perguntas=perguntas`, preservando a checagem de chamada a `respostas_sem_veredito`. `app/chamados/tests/test_contract_dedup_status.py:139,142` |

**Mudanças de contrato fora da cobertura literal de i–vi.** `test_cortes_f2_f9_m4.py:58` troca `RESPONDIDA → PENDENTE`. O mutante `_sempre_pendente` agora passa nesse caso; continua mordendo no controle alinhado. `test_e4c_f2_leitura_do_ato.py:501,503,505,507,509,520` troca cinco valores esperados de `True → False`. `test_o261_vigia_fio.py:831,846,876` acrescenta chave, leitura e assert do contador ao carimbo. Os comentários dos testes citam exceções complementares; os respectivos literais estão **[nao verificado]**. `app/chamados/tests/test_cortes_f2_f9_m4.py:54,57,64` · `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:498` · `app/chamados/tests/test_o261_vigia_fio.py:831,846,876`

| Teto ou selo de custo | Antes → depois |
|---|---|
| `_ancoradas`, total da carga | **1 exata → 2 exatas** com 1 e 5 perguntas, sendo uma consulta de decisões por lote. `app/chamados/tests/test_e4c_f2_leit

[truncado em 20000 de 30498 caracteres]

## 20/11 02:22 · sessao 1914d62789ee · CADEIA-20-PASSO-1 · censo · origem=Code

**PAREI no PRONTO 1.** O C1 confere nas três linhas raspadas. A matriz identifica escritas que podem ficar depois da última leitura coberta, mas as fotos entre rodadas ainda deixam a etapa escritora de cada célula [nao verificado]. A releitura depende de `emitidos` subir no ato daquele colaborador, e enfileiramentos feitos durante a segunda passada ou durante a releitura entram no descarte. `insumo.out:2617–2620,2879–2882,3051–3054` · `app/ponto/services/cartorio.py:1114,1152–1164,1209–1231`

Censo somente leitura da cópia. O caminho da obra consta no relato. A identidade do HEAD e os arquivos integrais das sondas [nao verificado]. As referências a `insumo.out` e `tres.out` abaixo usam a numeração original transcrita em `nove_registros.txt`. `app/docs/RELATO.md:100` · `insumo.out:803–806` · `tres.out:262–265`

## A. Tabela 1 — Escritores

A projeção está na ordem **T, V, P, F, FP**: chamado total, vivo, vivo que protesta, vivo com fio C6, vivo com fio C6 que protesta. T vem dos módulos de batida agrupados pelo dia do chamado. V vem de `VIVOS`. P exige que o dia admita trabalho e que o lastro do chamado não esteja todo aceso na ata deste julgamento. F exige uma pergunta viva pelo C6. FP exige P e F **no mesmo chamado**. No ramo sem adesão, a leitura do julgamento usa os totais zerados. `app/ponto/services/cartorio.py:141–157,1399–1415,1527–1545,1578–1583,1605–1617,1652–1653` · `app/chamados/catalogo/furo_modulos.py:15–16` · `app/chamados/juizes.py:1510–1519`

`rejulgar` abaixo significa inclusão da célula na fila consumida depois de E2 e do fecho adiado, com `forcar=True` e janela original. **Releitura por emissão** significa que `emitidos` subiu desde a entrada de `julgar_colab`; o corpo roda depois da fábrica, sobre a lista inteira, sem `forcar`, e passa pela guarda da impressão. **NENHUMA** significa que a escrita, naquele ponto, fica sem essas duas vias. Uma chamada avulsa pelo sinal é outro ato. `app/ponto/services/cartorio.py:1114,1123–1164,1179–1181,1209–1231,1552–1558,2174–2205` · `app/chamados/signals.py:189–218`

| Etapa e função | O que escreve e presenças que pode alterar | Ordem e via de cobertura |
|---|---|---|
| Fecho imediato por não protesto, marco inexistente ou cobrança indevida — `_julgar_colab_corpo` | `reconciliar()` grava `resolvido`. Pode mudar V, P, F e FP. O gate também pode reapontar perguntas a outro chamado vivo do dia, e o `save` pode resolver perguntas. T permanece enquanto a linha integra a lista do dia. `app/ponto/services/cartorio.py:1683–1738` · `app/chamados/models.py:422–457,654–668` · `app/chamados/signals.py:91–117` | Depois da leitura, antes do carimbo. **`rejulgar`** no primeiro corpo. Se a escrita ocorrer na segunda passada, resta a **releitura por emissão**, condicionada. Se ocorrer na própria releitura, **NENHUMA**. `app/ponto/services/cartorio.py:1701–1704,1721–1724,1734–1738,1152–1164,1209–1231,2035–2037` |
| Fecho por duplicata, inclusive o irmão no caminho C8 — `_julgar_colab_corpo` | Encerra o chamado todo duplicata ou o irmão selecionado. Pode mudar V, P, F e FP no agregado, também pelo gate de reaponte e pelo resolver do `save`. `app/ponto/services/cartorio.py:1804–1834` · `app/chamados/models.py:422–457,654–668` · `app/chamados/signals.py:91–117` | Depois da leitura, antes do carimbo. **`rejulgar`** no primeiro corpo. Na segunda passada, só **releitura por emissão**, condicionada. Na releitura, **NENHUMA**. `app/ponto/services/cartorio.py:1812–1833,1152–1164,1209–1231,2035–2037` |
| Ressuscitação — `ChamadoColaborador.ressuscitar_perguntas` | Limpa materialização, validação e o par da resposta nas perguntas selecionadas, emite evento e pode reabrir a disputa. Pode mudar F e FP. `app/chamados/models.py:1650–1655,1694–1712` | Depois da leitura, antes do carimbo. **`rejulgar`** quando o corpo recebe sucesso no primeiro julgamento. Na segunda passada, só **releitura por emissão**, condicionada. Na releitura, **NENHUMA**. `app/ponto/services/cartorio.py:1835–1861,1152–1164,1209–1231,2035–2037` |
| Emissão ordinária ou parcial — `Command._abrir_chamado` | Cria ou reabre chamado de batida. A reabertura pode ressuscitar perguntas. O emissor também chama `criar_disputa_a_partir_de_chamados_orfaos`, que pode criar ou ligar perguntas, chamar `analisar()` e fechar disputa. Pode mudar T, V, P, F e FP, inclusive em outro dia do colaborador alcançado pelo repasse. `app/ponto/management/commands/detectar_ausencias.py:694–714,738–745,788–807` · `app/chamados/models.py:965–983,1498–1511` · `app/chamados/services/disputa_emissao.py:1081–1105,1167–1192,1222–1237,1258–1302` | Depois da leitura, antes do carimbo. **Releitura por emissão** se o retorno subir `emitidos` no primeiro corpo ou na segunda passada. Emissão durante a própria releitura é contada em `emitidos_na_releitura` e fica com **NENHUMA** leitura interna posterior. `app/ponto/services/cartorio.py:1952–1955,1990–1993,2035–2037,2136–2146,1209–1231` |
| Emissão agrupada no limbo — `Command._abrir_chamado` → `agrupar_furo` | A porta pode criar a linha de batida, gravar o agrupamento, reapontar perguntas e resolver o chamado. Pode mudar T, V, P, F e FP. `app/ponto/management/commands/detectar_ausencias.py:694–714,748–763` · `app/chamados/services/limbo_folgas.py:267–295` | Depois da leitura, antes do carimbo. `AGRUPADO_LIMBO_FOLGAS` sobe `agrupados`. **NENHUMA** via própria. Só cabe na **releitura por emissão** se outra chamada subir `emitidos` no mesmo ato antes do teste. `app/ponto/management/commands/detectar_ausencias.py:759–763` · `app/ponto/services/cartorio.py:2136–2146,1209–1222,2035–2037` |
| Atualização de marcos — `Command._espelhar_marcos` | Grava `contexto_json.marcos_faltantes`. Pode mudar P e FP mantendo estado e contagem de fio. `app/ponto/management/commands/detectar_ausencias.py:544–569` · `app/ponto/services/lastro.py:153–181` · `app/ponto/services/cartorio.py:1605–1610,1652–1653` | Depois da leitura, antes do carimbo. Quando acompanha emissão contada, **releitura por emissão**. Quando chamado diretamente pelo corpo para um vivo, **NENHUMA** via própria; só cabe se outra emissão abrir a releitura. `app/ponto/management/commands/detectar_ausencias.py:738–745` · `app/ponto/services/cartorio.py:1990–2011,2035–2037,2136–2146,1209–1222` |
| Lavra e carimbo — `_julgar_colab_corpo` → `lavrar_veredito` / `carimbar` | Persiste ata, veredito e insumos do carimbo. Ata, veredito e via podem mudar a resposta de C6, portanto F e FP. O predicado P do próprio corpo já usa a ata recém-calculada. A escrita de ata/veredito também pode publicar `EventoCelula`. `app/ponto/services/cartorio.py:1578–1583,1605–1610,2012–2037` · `app/ponto/portas/celula.py:941–965,1181–1202` · `app/chamados/juizes.py:1418–1435,1450–1469` | A impressão é calculada antes dessa escrita. **`rejulgar`** se a célula estiver na fila por outro efeito, ou **releitura por emissão** se o gatilho abrir. **NENHUMA** via própria, inclusive para delta produzido no último carimbo. `app/ponto/services/cartorio.py:1541–1547,2035–2037,1147–1164,1209–1231` |
| E2 e fecho adiado — `retratar_perguntas_absolvidas_por_celula` e `julgar_colab` | E2 carimba `celula_absolveu` em perguntas que C6 já considerou mortas. Pode fechar disputa, chamar o fecho do pai e carimbar outras perguntas pela cascata. O fecho adiado pode retirar V, P, F e FP. A seleção de E2 abrange o colaborador; a fila do fecho adiado usa a célula do chamado que fechou. `app/chamados/reconciliador.py:634–665,695–722` · `app/chamados/signals.py:19–46,149–165` · `app/ponto/services/cartorio.py:1126–1151` | Depois do primeiro carimbo, antes da segunda passada. **`rejulgar`** se o fecho adiado daquela célula tiver sucesso. Para efeito de E2/cascata fora dessa fila, só **releitura por emissão** se o gatilho abrir. Caso contrário, **NENHUMA**. `app/ponto/services/cartorio.py:1126–1164,1209–1222` |
| Fábrica — `perguntar_pela_celula` → `perguntar` / `gerar_perguntas_disputa` | Desfaz carimbo que C6 já devolve como vivo, pode reabrir disputa, cria perguntas e, no reúso, muda célula, dia ou destino. Pode mudar F e FP. A costura da órfã pode ainda reemitir `batida_ausente`, mudando T, V e P. `app/chamados/services/fabrica_da_celula.py:160–174,206–219` · `app/chamados/services/relavra_via.py:236–262` · `app/chamados/models.py:3132–3209` · `app/chamados/services/disputa_emissao.py:453–479` · `app/chamados/reconciliador.py:960–1008,1662–1673` | Depois de consumir a fila. **Releitura por emissão** se o contador do cartório já subiu naquele ato. A fábrica e a reemissão por costura ficam com **NENHUMA** via própria. `app/ponto/services/cartorio.py:1152–1184,1209–1222,2136–2146` |
| Cascatas de fecho — `resolve_perguntas_ao_resolver`, `fechar_beco`, `_filho_morto_acorda_o_pai` e `retoma_regularizacoes_ao_fechar_disputa` | O resolver carimba perguntas, pode reabrir credor e pode fechar disputa. Os callbacks podem reconciliar outros chamados ou criar, corrigir e remover batidas. Podem mudar V, P, F e FP, e T se houver reemissão no reconciliador. `app/chamados/signals.py:91–146,229–240,351–371` · `app/chamados/services/disputa_emissao.py:1987–2030,2051–2054,2094–2135` · `app/chamados/services/becos.py:37–66` · `app/chamados/services/regularizacao_ext.py:71–83,128–156,219–225` · `app/chamados/reconciliador.py:1463–1493,1662–1673` | No `save` ou no `on_commit` do fecho. Quando a célula atingida já está na fila e a escrita precede o consumo, **`rejulgar`**. Quando precede uma releitura aberta por emissão, **releitura por emissão**. Fora dessas condições, **NENHUMA**. O sinal de chamado e a chamada de julgamento por batida saem se o cartório estiver ativo na thread. `app/ponto/services/cartorio.py:59–64,1126–1164,1209–1231` · `app/chamados/signals.py:210–218` · `app/ponto/signals.py:85–93,206–215` |
| Cluster — `cluster_espurio.reconciliar_colab` | Abre ou fecha `cluster_batida_espuria`, módulo fora da lista do cartório. Efeito direto nas presenças da lista, zero. Efeito indireto por cascata nas células medidas [nao verificado]. `app/ponto/services/cluster_espurio.py:11,176–216` · `app/chamados/catalogo/furo_modulos.py:15–16` · `app/chamados/signals.py:91–117,351–371` | Depois da fila e antes da fábrica. **NENHUMA** via própria. Efeito indireto anterior à releitura pode ser lido se outra emissão abrir o gatilho. `app/ponto/services/cartorio.py:1152–1181,1209–1222` |
| `apurar_furos_diarios` → `sincronizar_adesao` / `agrupar_furos_sob_limbo` | A apuração grava `FuroDiario`. As duas rotinas seguintes podem fechar filhos de batida, gravar agrupamento e reapontar perguntas, mudando V, P, F e FP. Os pais de cadastro usam `sem_previsao_escala`, fora de T. `app/escala/services/furos_diarios.py:95–109` · `app/escala/management/commands/apurar_furos_diarios.py:13–21` · `app/chamados/services/adesao.py:75–93,103–120,143–154` · `app/chamados/services/limbo_folgas.py:267–295,298–332` · `app/chamados/catalogo/furo_modulos.py:15–16` | Elo após o cartório. **NENHUMA** das duas vias do ato anterior. Mudança de estado por `save` pode abrir julgamento avulso. `app/config/crons.py:1527–1528` · `app/ponto/services/cartorio.py:1209–1234` · `app/chamados/signals.py:189–218` |
| `supra_juiz --executar` → `_ato_da_retratacao` | Pode reconciliar o chamado, rejeitar justificativa e resolver perguntas. Pode mudar V, P, F e FP, incluindo os efeitos do resolver sobre o credor. `app/ponto/management/commands/supra_juiz.py:45,257–292` · `app/chamados/reconciliador.py:572–590` · `app/chamados/services/disputa_emissao.py:1987–2030` | Agendado depois do cartório. **NENHUMA** das duas vias do ato anterior. O sinal tenta o julgamento avulso no `save` do chamado, enquanto o retratador ainda tem escritas posteriores. `app/config/crons.py:599` · `app/chamados/signals.py:189–218` · `app/chamados/reconciliador.py:576–586` |
| `flip_automatico --apply` → `flip_tipo` e sinais da batida | Troca o tipo da batida, recompõe turnos e pode disparar julgamento, retratação, reata e reemissão. O fato pode mudar P, F e FP; os escritores acionados podem mudar T e V. `app/ponto/management/commands/flip_automatico.py:51–79` · `app/ponto/services/flip_batida.py:25–39` · `app/ponto/signals.py:85–93,131–135,206–234` · `app/chamados/reconciliador.py:1463–1493,1662–1673` | Agendado depois do cartório. **NENHUMA** das duas vias do ato anterior. O sinal da batida tenta julgamento avulso, e o recompute pode publicar outro evento. `app/config/crons.py:707` · `app/ponto/services/flip_batida.py:30–39` · `app/ponto/nucleo.py:40–59,88–93` |
| `consumir_eventos_celula` | Rejunta a FK do chamado, carimba perguntas absolvidas e órfãs, fecha chamados por premissa ou marco e roda a fábrica. Pode mudar V, P, F e FP; reemissão pela fábrica pode mudar T. `app/chamados/reconciliador.py:1414–1438` · `app/chamados/rejuncao.py:98–111` · `app/chamados/services/vinculos.py:93–102` · `app/ponto/services/lastro.py:364–405` · `app/chamados/services/fabrica_da_celula.py:288–302` | Consumidor contínuo, em ato independente. **NENHUMA** das duas vias do cartório que publicou o evento. Um novo `save` de estado pode tentar a porta avulsa. `app/config/crons.py:394` · `app/ponto/portas/celula.py:945–965` · `app/chamados/signals.py:189–218` |

A auto-rejeição dentro dos fechos grava `Justificativa.status` e `descricao` **depois** de o chamado fechar. A justificativa é entrada da ata e pode afetar P, F e FP se mudar o que a grade dispensa. Para a célula do fecho bem-sucedido, vale a fila da etapa correspondente. Para outro dia atingido, **NENHUMA** via própria, com releitura condicionada ao mesmo gatilho. O sinal da justificativa consulta `em_julgamento`. `app/ponto/services/justificativa.py:292–324,348–355` · `app/ponto/services/cartorio.py:803–835,1140–1150,1693–1704,1209–1222` · `app/ponto/signals.py:85–93,155–165`

Nos argumentos agendados, `disparar_perguntas_competencia`, `reconciliar_perguntas_orfas`, `detectar_intervalo_ausente`, `fechar_becos_disputa` e `silenciar_chamados_isentos` contam ou alarmam. Para `reeavaliar_ausencias_feriado`, conferi a declaração `--alarme` e o teste que exige a preservação do estado do chamado; o corpo do comando [nao verificado]. `app/config/crons.py:491,608–609,663,1539–1545` · `app/chamados/management/commands/disparar_perguntas_competencia.py:184–212` · `app/ponto/management/commands/reconciliar_perguntas_orfas.py:20–36` · `app/ponto/management/commands/detectar_intervalo_ausente.py:98–108,136–139` · `app/chamados/management/commands/fechar_becos_disputa.py:23–30,52–59` · `app/chamados/management/commands/silenciar_chamados_isentos.py:23–44` · `app/chamados/tests/test_jv1_feriado_vira_vigia.py:180–206`

Em `lavrar_deslogados`, a escrita conferida é de retrato e pauta. Os elos de HE escrevem retrato e `DecisaoHE`. `lavrar_retrato_folha`, `lavrar_cxr_do_motor` e o selo de exportação gravam seus retratos ou métricas. Os demais elos dessa sequência são declarados como vigias. Efeito transitivo adicional desses elos sobre as presenças [nao verificado]. No ramo agendado após o flip, `marcar_foto_ausente_retro` abre ou agrega disputa de foto pela porta genérica, que também pode rodar `perguntar_entradas_pendentes`. Se esse efeito secundário escrever fio de batida, a via é **NENHUMA** no ato anterior. Ocorrência nas células medidas [nao verificado]. `app/config/crons.py:686–687,1557–1587,1610–1811` · `app/ponto/management/commands/lavrar_deslogados.py:65–86` · `app/ponto/services/pautas_dos_deslogados.py:73–83` · `app/ponto/management/commands/lavrar_he_pendente.py:48–62` · `app/ponto/portas/he.py:369–372,403` · `app/folha/management/commands/lavrar_retrato_folha.py:65–80` · `app/ponto/services/cxr_na_lista.py:142–151` · `app/folha/porta_export.py:768–790` · `app/ponto/management/commands/marcar_foto_ausente_retro.py:89–105` · `app/chamados/services/disputa_emissao.py:673–702`

## B. Tabela 2 — As nove

`?` significa **[nao verificado]**. Os vetores descrevem os insumos fotografados no delta, e os valores de P/FP exigem o lastro por chamado. `fio:0` comprova V presente e F ausente. Quando a lista passa a conter apenas resolvidos, V, P, F e FP são ausentes. As etapas internas entre as fotos [nao verificado]. `app/chamados/catalogo/motor.py:24–32` · `app/ponto/services/cartorio.py:543–547,1527–1545,1605–1617,1652–1653` · `insumo.out:803–806,2617–2620`

| Célula e delta medido | T,V,P,F,FP antes → depois | Etapa escritora e via | Veredito com (i)+(ii) |
|---|---|---|---|
| **128355** — P2, `39234:em_analise → resolvido`, `fio:1 → ausente`, ata igual. `insumo.out:803–806` · `tres.out:262–265` | `(1,1,?,1,?) → (1,0,0,0,0)`. `tres.out:265` | [nao verificado]. Capturar quem gravou `resolvido` em `39234` e em que corpo ou cascata. Fecho direto/adiado tem fila nas condições da tabela. Fecho após a última leitura fica sem via. `app/ponto/services/cartorio.py:1140–1164,1209–1231,1691–1738` | **INDETERMINADO**. A perda sobrevive a (i). Zera se for lida ainda em P1; **SOBRA EM P2** se cair após a última leitura de P1 e P2 for a primeira a incorporá-la. `tres.out:262–265` · `app/ponto/services/cartorio.py:1152–1164,1209–1231,1552–1558` |
| **129943** — P2, `36531:em_analise → resolvido`, `fio:1 → ausente`, ata igual. `insumo.out:1085–1088` · `tres.out:266–269` | `(1,1,?,1,?) → (1,0,0,0,0)`. `tres.out:269` | [nao verificado]. Capturar a gravação de `36531` e distinguir fecho do corpo, E2/fecho adiado e cascata. Via depende da etapa. `app/ponto/services/cartorio.py:1126–1164,1209–1231,1691–1738` | **INDETERMINADO**. A perda sobrevive a (i). Zera se coberta em P1; **SOBRA EM P2** se a primeira leitura do novo estado for P2. `tres.out:266–269` · `app/ponto/services/cartorio.py:1152–1164,1209–1231,1552–1558` |
| **138359** — P2, `37355:em_analise → resolvido`, `fio:1 → ausente`, ata igual. `insumo.out:2577–2580` · `tres.out:270–273` | `(1,1,?,1,?) → (1,0,0,0,0)`. `tres.out:273` | [nao verificado]. Capturar a gravação de `37355`, a eventual entrada na fila e a posição em relação à última releitura. `app/ponto/services/cartorio.py:1126–1164,1209–1231,1691–1738` | **INDETERMINADO**. A perda sobrevive a (i). Zera se coberta em P1; **SOBRA EM P2** se a primeira leitura do novo estado for P2. `tres.out:270–273` · `app/ponto/services/cartorio.py:1152–1164,1209–1231,1552–1558` |
| **129476** — P2, `fio:1 → 2`, ata igual. `insumo.out:999–1002` | `(1,1,?,1,?) → (1,1,?,1,?)`. `insumo.out:1002` · `app/ponto/services/cartorio.py:1527–1545` | [nao verificado]. Identificar o chamado que ganhou fio e o primeiro ponto em que seu C6 mudou. Ressuscitação no primeiro corpo tem `rejulgar`; repasse, lavra ou fábrica dependem das condições da tabela. `app/ponto/services/cartorio.py:1835–1861,1952–1955,2012–2037,1209–1222` · `app/chamados/services/fabrica_da_celula.py:160–174,206–219` | **INDETERMINADO**. A contagem isolada zera com (i). P e FP, e a cobertura de eventual mudança, seguem [nao verificado]. `insumo.out:1002` · `app/ponto/services/cartorio.py:1605–1617,1652–1653` |
| **129490** — P2, `39044:aberto → em_analise`, `fio:1 → 2`, ata igual. `insumo.out:1003–1006` | `(1,1,?,1,?) → (1,1,?,1,?)`. Depois, `FP = P`, pois os dois vivos têm fio. `insumo.out:1006` · `app/ponto/services/cartorio.py:1527–1545,1652–1653` | [nao verificado]. O repasse chamado pelo emissor tem o par criação de pergunta + `analisar()`. Falta ligar as duas alterações às escritas de `39044`/`39014` e medir o retorno da emissão no ato de col869. `insumo.out:1003–1006` · `app/ponto/management/commands/detectar_ausencias.py:788–807` · `app/chamados/services/disputa_emissao.py:1180–1192,1258–1266` | **INDETERMINADO**. Rótulo e contagem isolados zeram com (i). FP pode ter acendido se o chamado que ganhou fio era o único que protestava. `insumo.out:1006` · `app/chamados/catalogo/motor.py:24–32` · `app/ponto/services/cartorio.py:1605–1610,1652–1653` |
| **138541** — P2, `fio:0 → 1`, ata igual. `insumo.out:2617–2620` | `(1,1,?,0,0) → (1,1,?,1,?)`. `insumo.out:2620` · `app/ponto/services/cartorio.py:543–547,1527–1545` | [nao verificado]. Identificar chamado, pergunt

[truncado em 20000 de 29355 caracteres]

## 20/11 02:18 · sessao ebc6e5eee26f · CADEIA-20-PASSO-1 · verificador · origem=Code

**AVAL-DEVOLVIDO**

- **C1.3** — Falta evidência por etapa que atribua os deltas aos escritores das três células. A matriz de leitura e sonda segue. `cadeia_20_v3_complemento_2011_0215.txt:4–6` · `logs/CADEIA-20-PASSO-1/v3/medidas/insumo.out:2617–2620,2879–2882,3051–3054`
- **C2.3** — Falta fixar a revisão-base e os campos comparados byte a byte na H09b. `cadeia_20_v3_complemento_2011_0215.txt:5,8` · `rascunho_CADEIA-20-IMPRESSAO-V3.txt:49,69`

## C1

1. **PREMISSAS — NÃO PROCEDE / NÃO BLOQUEIA.** O anexo 3 registra as três células em P2, todas na competência 11, com `ata_mudou=False` e `imp_mudou=True`:
   - **138541** — `fio:0 → fio:1` é o único delta de insumo exibido. `logs/CADEIA-20-PASSO-1/v3/medidas/insumo.out:2617–2620`
   - **142995** — chamado `38675:aberto → em_analise` e `fio:1 → fio:2`. `logs/CADEIA-20-PASSO-1/v3/medidas/insumo.out:2879–2882`
   - **143945** — chamado `38907:aberto → em_analise`, sem delta de fio registrado. `logs/CADEIA-20-PASSO-1/v3/medidas/insumo.out:3051–3054`

2. **COLISÃO — NÃO PROCEDE / NÃO BLOQUEIA.** C1 resolve a lacuna dos três deltas de 3b. A causa por célula continua exigida por MUDA 1 e PRONTO 1 e pelo aval 2325. A matriz segue antes do código, com as duas vias de cobertura já aprovadas. C1 preserva a revisão do escopo inteiro do 1815 e o horizonte da L-124. `cadeia_20_v3_complemento_2011_0215.txt:4–8` · `rascunho_CADEIA-20-IMPRESSAO-V3.txt:36,46,60–62,66,88` · `cadeia_20_volta_3_e_horizonte_1911_1815.txt:5–10` · `cadeia20_medir2_dinheiro_1911_2325.txt:5–6` · `app/docs/LEIS.md:145`

3. **LITERAL — PROCEDE / BLOQUEIA.** A leitura identifica candidatos e sua ordem: corpo, E2/fecho, segunda passada, fábrica e releitura condicionada a `emitidos`. A fábrica tem caminho para `analisar()`, e o catálogo admite `aberto → em_analise` também pela porta humana. Os recortes mostram o antes e o depois, sem testemunha intermediária que atribua a escrita. Para **138541**, o delta confirma que surgiu presença de fio. Para **142995**, a presença global de fio já existia. Para **143945**, os dois estados pertencem a `VIVOS`. A interseção fio × protesta e o escritor efetivo de cada célula ficam **[nao verificado]**. Isso bloqueia o aceite causal de PRONTO 1. `app/ponto/services/cartorio.py:1123–1164,1179–1184,1200–1231,1527–1545,1605–1617,1652–1653` · `app/chamados/services/disputa_emissao.py:1189–1192` · `app/chamados/catalogo/motor.py:24–32,98–113` · `rascunho_CADEIA-20-IMPRESSAO-V3.txt:66`

4. **EXPORTADA, DINHEIRO OU `!` — NÃO PROCEDE / NÃO BLOQUEIA.** C1 pede conferência e atribuição causal. Os hashes abreviados de FM e ED coincidem na sonda fornecida. O `_K6` por célula, zero escrita em entregue e o diff financeiro exigido pelo rascunho ficam **[nao verificado]**. `cadeia_20_v3_complemento_2011_0215.txt:3–4` · `logs/CADEIA-20-PASSO-1/v3/medidas/tres.out:283` · `rascunho_CADEIA-20-IMPRESSAO-V3.txt:39–41,60,69–70` · `app/folha/export.py:556–594` · `CLAUDE.md:401–417`

## C2

1. **PREMISSAS — NÃO PROCEDE / NÃO BLOQUEIA.** Os bytes legados são a serialização vigente dos insumos da impressão. O cartório junta as partes com `|`, codifica a string e grava o SHA-256 em `CelulaDia.impressao`. A entrada inclui batidas, status da cobertura, `pk:status_local` dos chamados, DNA e campos condicionais. `fio:N` é a contagem exata de vivos com fio e fica ausente quando inexiste chamado vivo. `aberto → em_analise` altera `pk:status_local` mesmo com fio constante, muda a impressão calculada e pode provocar relavra no caminho normal. `app/ponto/services/cartorio.py:493–578,1527–1558` · `app/ponto/portas/celula.py:1189–1201`

   MUDA 3 fixa o horizonte na **competência 2026-10**, pelo corte da empresa via `versao_para`. Até 09, preserva a impressão para os mesmos insumos e a foto gravada enquanto não houver fato novo. A L-124 mantém a regra anterior para o passado e admite o rejulgamento normal por fato novo. Nesta árvore, `cartorio.py:560–563` contém **comentários sobre isenção**. O molde equivalente é a guarda de horizonte comentada em `564–569` e executada em `570–571`, que acrescenta `isento:1` sob `regra >= 4`. A numeração no main fica **[nao verificado]**. `rascunho_CADEIA-20-IMPRESSAO-V3.txt:38` · `app/docs/LEIS.md:145` · `app/ponto/services/cartorio.py:161–166,280–299,559–571`

2. **COLISÃO — NÃO PROCEDE / NÃO BLOQUEIA.** C2 corrige o conflito do R4 com MUDA 3 separando o caso de foto estável do controle com mudança no insumo legado. A vedação do 1815 continua valendo para levar a razão da sobra à impressão ou relavrar o passado por causa da regra nova. A v5 e o estado nomeado do leitor continuam no escopo. H09 e H09b cabem em fixture de competência aberta, com as restrições de exportada, dinheiro e Pauta DP do 2325 preservadas. `cadeia_20_v3_complemento_2011_0215.txt:5,8` · `rascunho_CADEIA-20-IMPRESSAO-V3.txt:38–41,49,60–62,69–70,81` · `cadeia_20_volta_3_e_horizonte_1911_1815.txt:5–10` · `cadeia20_medir2_dinheiro_1911_2325.txt:2–6` · `app/escala/services/leitor_celula.py:193–222`

3. **LITERAL — PROCEDE / BLOQUEIA.** **H09 é montável por leitura.** Com a célula já carimbada, use a forma do H4: dois chamados vivos do mesmo dia, só um protesta. Após P1, reapontar a única pergunta viva de um chamado para o outro pela porta existente, mantendo-a viva, conserva os pares `pk:status_local` e `fio:1`. A presença de fio em chamado que protesta muda. A fixture deve demonstrar essas pré-condições nos dois braços. Execução **[nao verificado]**. `rascunho_CADEIA-20-IMPRESSAO-V3.txt:48–49` · `app/chamados/services/fio.py:853–861,956–984` · `app/ponto/services/cartorio.py:502–507,546–547,1527–1545,1605–1617,1652–1653`

   **H09b fica subespecificada.** Para fechar o literal, fixe como base uma revisão identificada **da cópia anterior à mudança V3**, com v5 já presente. Rode a mesma transição em 09 aberta nos dois braços, com a mesma foto inicial, PKs, insumos, relógio por ponto de comparação e `forcar=False`. Demonstre a mudança do hash legado entre antes e depois e compare **entre os braços**, após a transição pelo mesmo caminho, a `impressao` persistida e a foto de R4 (`ata`, `veredito`, `julgada_em`) sob a mesma serialização. Fixe no aval se a igualdade se estende a outros campos ou escritas. O molde da isenção tem seu próprio golden, de `97b43098`. A revisão-base e o perímetro exato da H09b ficam **[nao verificado]**. `cadeia_20_v3_complemento_2011_0215.txt:5,8` · `rascunho_CADEIA-20-IMPRESSAO-V3.txt:38,49,69` · `app/ponto/tests/test_e3_isento_na_impressao.py:59–91` · `app/ponto/services/cartorio.py:1541–1558,2012–2037` · `app/ponto/portas/celula.py:1181–1202`

4. **EXPORTADA, DINHEIRO OU `!` — NÃO PROCEDE / NÃO BLOQUEIA.** O controle de relavra cabe em 09 **aberta**, confirmada por `_K6=False`. Na árvore atual, o comando seleciona células passadas sem esse filtro, e o sinal de mudança de status pode chamar a porta avulsa. Por isso, continua exigida a cerca de MUDA 4 antes de julgamento ou escrita, inclusive nos caminhos internos. Zero escrita em entregue e zero impacto financeiro ficam **[nao verificado]**. C2 não concede exceção de `!`. `rascunho_CADEIA-20-IMPRESSAO-V3.txt:39–41,49,60,69–70` · `app/ponto/management/commands/processar_cartorio.py:47–69` · `app/chamados/signals.py:189–218` · `app/docs/JUIZES.md:40–42` · `app/folha/export.py:556–594` · `CLAUDE.md:401–417`

`VEREDITO C1: BLOQUEIA=1 NAO_BLOQUEIA=3`  
`VEREDITO C2: BLOQUEIA=1 NAO_BLOQUEIA=3`

## 20/11 02:10 · sessao 568ad59a3155 · TRAVAS-SEM-MEMORIA · leitura · origem=Code

aval Ronald: TRAVAS-SEM-MEMORIA-V4 (rascunho Codex 20/11) -- unidade: hook e ESTADO; lei: nenhuma nova

## RESPOSTAS Q1-Q4 DO (d)

**Q1.** O piloto fixa `FICHA <ID>` com **8 campos**: ID, OBRA e UNIDADE, OBJETIVO, LEI ou CORTE com `arquivo:linha`, DECISÃO DE DESENHO já tomada, PROIBIDO e NÃO ALCANÇA, PRONTO observável e BASE. Mede por uso os itens devolvidos ou perguntas após o aval final, com esperado **0**, os minutos da ficha ao aval final, os tokens do Codex no rascunho e a comparação com os avais 1710 e 1735. Depois de **3 avais** pelo fluxo, publica a comparação no RELATO. Virar padrão exige aval novo. A contagem de usos já realizados é [nao verificado]. `aval_rascunhado_pelo_codex_1911_1838.txt:4–11`.

O (d) **muda** a forma da ficha e a divisão do trabalho de desenho. A ficha passa a trazer problema, números com sonda, leis, proibido, não muda e perguntas. Uma linha `DECISAO` só vale com fonte conferível. O Codex apresenta os desenhos medidos e redige um inteiro. O chat escolhe. O (d) **acrescenta** a medida de devoluções por origem. A condição de virar padrão continua a do piloto e o `APROVA RASCUNHO` continua o da Ficha 2. `ficha_sem_decisao_2011_0200.txt:5–9,17–18`, `aval_rascunhado_pelo_codex_1911_1838.txt:4–8`, `fichas_v3_para_rascunho_2011_0205.txt:1`.

**Q2.** O piloto descreve a recepção da ficha como conduta da sessão. A mecânica encontrada na cópia é genérica. O wrapper recebe `--anexo`, verifica a presença do arquivo e raspa seu conteúdo antes da chamada. O `PreToolUse` instalado na cópia vê a chamada Bash ao wrapper e extrai papel, obra e anexos. Nesses pontos, a cópia não julga a ficha nem suas linhas `DECISAO`. O `Stop` atua sobre a fila e o settings entregue não registra `UserPromptSubmit`. O literal do piloto foi lido no anexo fornecido. Sua presença em `logs/AVAIS-1515/aval_rascunhado_pelo_codex_1911_1838.txt` e os registros carregados na sessão viva ficam [nao verificado]. `aval_rascunhado_pelo_codex_1911_1838.txt:4–5,10`, `bin/codex_pergunta.sh:57–68,99–100,323–335`, `bin/disjuntor.py:1448–1463,1837–1863`, `.claude/settings.json:2–73`, `bin/hook_stop_fila1.py:1339–1345,1381–1404`.

A recusa cabe no registro Bash de `PreToolUse` já proposto para T1, na chamada dedicada em que a sessão entrega a ficha ao wrapper para redigir. A identificação é pelo papel `leitura`, pela obra `<ID>-RASCUNHO` e pelo anexo da ficha nessa chamada. O `PAREI RASCUNHO` fica no RELATO e afeta só a obra. `base_v4_sem_d.txt:18,21,34–38`, `aval_rascunhado_pelo_codex_1911_1838.txt:5`, `ficha_sem_decisao_2011_0200.txt:1,7,17–18`, `CLAUDE.md:262–275`.

**Q3.** Casos propostos para o RED. O `r.txt` existente é da fixture. Resultados [nao verificado]. `ficha_sem_decisao_2011_0200.txt:7,14`, `bin/codex_pergunta.sh:425–430,464–469`.

- `DECISAO usar A` → `FICHA COM DECISAO SEM LEITURA`. `ficha_sem_decisao_2011_0200.txt:7,14`.
- `DECISAO usar A | logs/TRAVAS-TESTE/r.txt:1`, entrega existente com a linha citada → entra. `ficha_sem_decisao_2011_0200.txt:7,14`, `bin/codex_pergunta.sh:464–469`.
- `DECISAO usar A | logs/TRAVAS-TESTE/ausente/r.txt:1`, caminho inexistente → `FICHA COM DECISAO SEM LEITURA`. `ficha_sem_decisao_2011_0200.txt:7,14`.
- `FICHA TRAVAS-TESTE` com apenas `PERGUNTAS: onde a recusa cabe?` no corpo → entra. Aceitar qualquer texto depois de `DECISAO` é o mutante e tem de derrubar os casos de recusa. `ficha_sem_decisao_2011_0200.txt:6–7,14`. [nao verificado]

**Q4.** Se o chat escolhe o desenho já redigido, **0 recolagens adicionais** do Ronald por obra. Se escolhe outro, há **1 pedido adicional de novo rascunho por escolha**. Pela via manual, isso custa **1 recolagem adicional** do Ronald por pedido. Se o pedido chegar à sessão pela própria via da ficha sem retransmissão humana, custa **0 recolagens**. A via efetiva desse retorno é [nao verificado]. O rascunho novo volta pelo RELATO, que o chat lê, e o `APROVA RASCUNHO` do texto finalmente escolhido segue o rito da Ficha 2. Se houver recusa de ficha, cada correção retransmitida pelo Ronald custa mais **1 recolagem**. O total observado é [nao verificado]. `aval_rascunhado_pelo_codex_1911_1838.txt:4–6,9`, `ficha_sem_decisao_2011_0200.txt:1,7–9,15,17`, `fichas_v3_para_rascunho_2011_0205.txt:1`.

## FONTE

### (d) — desenhos medidos para o ponto de recusa

- **A — `PreToolUse` na entrega do anexo ao wrapper.** O evento já enxerga o comando Bash e o leitor atual distingue invocação, papel, obra e anexos. Custo proposto: **0 registros adicionais** aos da V4, uma conferência por chamada dedicada e **0 recolagens** no caminho aceito. Quebra se a sessão não carregar o registro da raiz ou se entregar a ficha por uma chamada que não exponha o anexo literal. A versão do anexo tem de ficar fechada entre a conferência e o consumo pelo wrapper. A cópia registra precedente de hooks da raiz inertes em sessão aberta em `app/`. `bin/disjuntor.py:1353–1368,1448–1463,1837–1863`, `bin/codex_pergunta.sh:323–335`, `.claude/settings.json:62–73`, `app/docs/RELATO.md:3341`, `base_v4_sem_d.txt:18,21,34–38`, `ficha_sem_decisao_2011_0200.txt:7,13,15`.
- **B — `UserPromptSubmit` no recebimento da ficha.** Usaria o evento já proposto para T2. Custo proposto: **0 registros adicionais** aos da V4, com uma colagem adicional quando a ficha entregue hoje como anexo tiver de passar também pelo prompt. Quebra a correspondência entre ficha julgada e ficha entregue quando o texto colado diverge do anexo aberto pelo wrapper. A cópia não tem esse evento configurado e a passagem obrigatória da ficha por colagem é [nao verificado]. `base_v4_sem_d.txt:24,34–38`, `.claude/settings.json:2–73`, `aval_rascunhado_pelo_codex_1911_1838.txt:4–5`, `bin/codex_pergunta.sh:323–335`.
- **Redigido inteiro: A.** É o desenho que consegue conferir o anexo na entrega usada pelo piloto, dentro do registro Bash já previsto. A decisão final é do chat. Se ele escolher B, pede novo rascunho. A comparação foi feita por leitura estática. Carga, latência e execução dos casos [nao verificado]. `ficha_sem_decisao_2011_0200.txt:7–8,13,17–18`, `aval_rascunhado_pelo_codex_1911_1838.txt:5–6`, `base_v4_sem_d.txt:18,21,34–38`, `bin/disjuntor.py:1448–1463,1837–1863`.

- F0. FICHA 2, alíneas (a)–(c), rege este rascunho sobre o V3, que não foi aprovado. Base informada `eb1e49c1ec1055e6bf43d7f67df63b17054b474e`. A cópia entregue veio por `git archive`, sem `.git`. `HEAD = main = hash`, carga dos hooks na VM, executor da pista e resultados de smoke [nao verificado]. `fichas_v3_para_rascunho_2011_0205.txt:1,24–28`, `CLAUDE.md:344–348`.
- F1. `suite.sh` resolve `--volta` e julga a seleção efetiva. Seleção vazia ou que cobre a descoberta toda é inteira. Inteira sem motivo canônico sai `rc 2`. A volta intermediária junta ruff, selo do disjuntor e recorte com vizinhos e selos globais. `bin/suite.sh:156–180,215–263`, `bin/volta_intermediaria.sh:18–46`.
- F2. Régua tem runner e carimbo próprios. O Git entrega argumentos e stdin ao wrapper do pre-push, que decide se reaproveita a régua e roda `tenants` em runner separado. O pouso usa `suite.sh` pela trava da VM. `bin/regua.sh:46–51,84–101,165–169`, `bin/hooks.sh:58–65`, `bin/pre-push.sh:6–8,27–74,159–200`, `CLAUDE.md:95–106`.
- F3. A opção A registra volta e pre-opus de lateral na pista, espinha e pouso na VM. A interface da pista recebe commit em `refs/pista/<sha>`, `pede <sha> [--only <labels>] [--parallel N]` e `espera <id>`. Há registros de `rc 0`, `rc 1` e `rc 71` sem a ref. `app/docs/RELATO.md:65,201,1497,1529`, `app/docs/BACKLOG.md:2191–2195`.
- F4. `PreToolUse` recebe o comando externo em `tool_input.command`. O disjuntor atual pode sair cedo para Bash da sessão principal. `UserPromptSubmit` está ausente do settings entregue. Stop lê `transcript_path` e cobra a fila pelas portas vigentes. A abertura em `app/` já deixou hooks da raiz inertes. `.claude/settings.json:2–73`, `bin/disjuntor.py:1837–1843,1864–1867,1912–1933`, `bin/hook_stop_fila1.py:1339–1345,1381–1404`, `app/docs/RELATO.md:3341`, `app/docs/PROMPTS.md:1877`.
- F5. `raias_estado.py` cruza worktrees com declarações do escritor `raia_agente.sh`. A união inclui declaradas, à frente e só sujas. Hoje as só sujas saem por nome, há declaração sem caminho e o HEAD por cópia não sai. `gerar_estado.py` lê o medidor e `relato.sh` gera, raspa e publica o ESTADO. `bin/raias_estado.py:42–135,138–174`, `bin/raia_agente.sh:40–59`, `bin/gerar_estado.py:211–229,329–331`, `bin/relato.sh:30–38,114–119,133–147`.
- F6. O construtor Codex escreve só em `<wt>/app`, com uma vaga por repo. `.claude/` consta no ignore e há precedente de instalação local negada pelo classificador. O arquivo do RELATO guarda marcos antigos. `CLAUDE.md:349–356,394,436–442`, `bin/codex_pergunta.sh:121–135,248–259`, `.gitignore:66`, `app/docs/RELATO.md:4356,4411`.
- F7. Janela de 24 h encerrada em 20/11 às 01:54 −03. F5 tem **58 entradas** em 18 horas com commits, pico de 11 às 08h. CADEIA tem **49** em 11 horas, pico de 17 às 16h. `main` tem **33**. São 107 entradas somadas nas duas cópias, 41 fora da lista do main (25 F5 + 16 CADEIA), com histórico alcançável contado pelo `git log`. Mudanças efetivas de HEAD e autoria por volta [nao verificado]. `medida_t2.txt:1–178`. [nao verificado]
- F8. Contagem V3 em 19/11. Contar uma devolução por evento recebido com token literal `CONSTRUIR` na primeira linha e pelo menos um alvo existente cujo prefixo colado difira do HEAD naquele recebimento. Tratar ausência de token de cópia, `copia=nova` e duplicata conforme suas portas. A lista de **seis candidatos de construção pelo título ou resumo** é `E4C-F5-CONSTROI-1911-0815`, `CADEIA-20-PASSO-1-CONSTROI-1911-1010`, `DST-L2-JZ1-C7-CONSTROI-1911-1200`, `E4C-F1-B2B-F0-CONSTROI-1911-1315`, `FERIADO-CONSTROI-E-C8-MEDE-1911-1400` e `E4C-F5-CONSTROI-A-1911-1710`. `medida_t2.txt:194,202,216,222,225,242`, `app/docs/PROMPTS.md:2608,2616,2630,2636,2639`, `CLAUDE.md:436–438`.
- F9. **0 devoluções por divergência demonstráveis, lista confirmada vazia, total real [nao verificado].** Os resumos também registram complementos que liberam ou retomam construção, inclusive F5 1735/1830 e CADEIA 1815/1836. O título com `CONSTROI` e o resumo com `CONSTRUIR` não identificam o token na primeira linha colada. Falta, por evento, o texto recebido com alvos/prefixos, o instante de recebimento e a fotografia de worktrees/HEADs nesse instante, inclusive das obras fora das duas listas. Exemplo medido de exposição: o merge F5 `3f1d63863` é das 17:07 e o 1710 foi recebido às 17:2x, mas o anexo não traz o prefixo colado, nem prova o HEAD naquele evento. `medida_t2.txt:37,181–254`, `app/docs/RELATO.md:60,111,119,158`.
- F10. O complemento rege o (d). Registra **6 devoluções** dos rascunhos anteriores, **5 de origem ficha** e **1 de origem rascunho**, pela medida do chat ali transcrita. A lista individual das devoluções é [nao verificado]. O enunciado informa que a base V4 sem (d) ficou inédita. A publicação externa é [nao verificado]. As referências novas aos anexos contam linhas a partir do primeiro literal após `== anexo: ... ==`. O `medida_t2.txt` integral e a numeração de `fichas_v3_para_rascunho_2011_0205.txt:24–28` herdada da base são [nao verificado]. A Ficha 2 está nas linhas 15–19 da transcrição fornecida. `ficha_sem_decisao_2011_0200.txt:1,3`, `base_v4_sem_d.txt:5,12–14`, `fichas_v3_para_rascunho_2011_0205.txt:15–19`.

## MUDA

- M1. A sessão constrói em cópia e instala os dois registros locais da raiz, somente para este instrumento. Codex verificador lê o diff depois da construção. Antes do código, publicar as cinco linhas abaixo no RELATO e mandá-las uma vez ao Codex. A execução depende de `aval Ronald: APROVA RASCUNHO TRAVAS-SEM-MEMORIA-V4 <sha256 12>`. O rascunho integral com SHA vai ao RELATO, e F5 C2 segue na frente. `fichas_v3_para_rascunho_2011_0205.txt:1`, `aval_rascunhado_pelo_codex_1911_1838.txt:5–7`, `CLAUDE.md:309,349–356,372–394`.
- M2. T1 usa a rota declarada pela sessão no despacho. Para a inteira pre-opus de **lateral ou espinha**, o pedido local carrega `PISTA_LATERAL=<OBRA>` junto de `bin/suite.sh --dir <cópia> --motivo "pre-opus <OBRA>"`. `PISTA_LATERAL` conserva o nome da chave e carrega o ID nas duas origens. Inteira pre-opus com chave ausente ou de outra obra é recusada antes de SSH e de runner VM. A exceção NOMINAL ao contrato PISTA-2 item 3 vale só para **“nem a suite que o opus recebe”**. O pouso segue pela trava da VM e certifica. Divergência VM × pista no mesmo commit = relatar e suspender o uso da pista até Ronald ver. `fichas_v3_para_rascunho_2011_0205.txt:25`, `app/docs/BACKLOG.md:2191,2194–2195`, `app/docs/RELATO.md:65,81`.
- M2, volta. Para volta de lateral, `PISTA_LATERAL=<OBRA>` acompanha `bin/volta_intermediaria.sh <cópia> <base> <saída>`. O script da volta conserva ruff e selo e passa o recorte efetivo ao ramo remoto de `suite.sh`. Volta cujo recorte cobre tudo continua sujeita ao `rc 2` da guarda atual. Régua, pre-push, pouso e teste focado mantêm seus comandos. `bin/suite.sh:129–180,215–263`, `bin/volta_intermediaria.sh:18–46`, `CLAUDE.md:95–106`, `app/docs/RELATO.md:65,81`.
- M3. Registrar um `PreToolUse` de Bash para a porta canônica. Ele lê `PISTA_LATERAL`, a obra do motivo e o SHA completo da cópia. Requer o mesmo ID nos dois campos e, para a inteira pre-opus, uma linha literal no início da linha `PRONTO <OBRA> item a item: sha=<SHA completo>` em `app/docs/RELATO.md` ou `app/docs/RELATO-ARQUIVO.md`. Um registro arquivado vale para o mesmo SHA. Cabeçalho de outro SHA, de outra obra, indentado ou citado vale como ausente. Resposta `SEM PRONTO ITEM A ITEM`. O mesmo verificador de instrumento roda no executor imediatamente antes de criar a ref e pedir o job. O SSH recebe o SHA conferido e as opções documentadas. `bin/disjuntor.py:1796–1798,1837–1843`, `bin/suite.sh:219–263`, `CLAUDE.md:439–442`, `app/docs/BACKLOG.md:2191`.
- M4. A sessão publica o PRONTO com cada item, medida e pendência. O hook confere identidade da obra e SHA. Após mais de três voltas Codex, pendências registradas permitem a inteira e a subida ao opus pela regra vigente. Uma recusa de PRONTO fecha o pedido antes do SSH. Falha de infraestrutura segue a contingência registrada da rota, com `PISTA: VM porque <motivo>` quando couber. Vermelho de teste retorna o rc do job. `CLAUDE.md:372–381`, `app/docs/RELATO.md:81,1010,1209`, `app/docs/BACKLOG.md:2191`.
- M5. T2 faz `raias_estado.py` expor o nome e o HEAD completo observado por `git rev-parse HEAD` em cada worktree da união, incluindo as só sujas. O ESTADO publica um `copia=<nome>@<HEAD12>` por cópia resolvida. Raia declarada sem caminho ou sem HEAD resolvível publica `raia=<nome> SEM WORKTREE`. O hook lê o mesmo medidor na hora do envio. O ESTADO é a fotografia publicada. `bin/raias_estado.py:42–48,73–135,138–174`, `bin/gerar_estado.py:211–224`, `bin/relato.sh:37–38,114–119`.
- M6. `UserPromptSubmit` examina aval colado com o token literal `CONSTRUIR` na primeira linha. Na linha `alvos=<nome>,<nome>`, listar todas as cópias tocadas e dar, na mesma ordem, um `copia=<nome>@<prefixo>` por alvo existente ou `copia=nova` na posição de cada alvo novo. Prefixo hexadecimal de 12 caracteres até o SHA completo. Nome é o basename da worktree, com escape percentual para delimitadores. `fora=<nome>,...` declara cópias vinculadas à obra que ficam fora do aval. Toda raia declarada com a mesma `OBRA` consta em `alvos` ou `fora`. A sessão explicita em `alvos` cópias tocadas sem declaração, inclusive descendentes. `bin/raias_estado.py:73–102,125–135`, `bin/raia_agente.sh:40–59`, `app/docs/RELATO.md:1513`, `travas_sem_memoria_v2_2011_0110.txt:5` [nao verificado].
- M6a. **Recomendada.** Por alvo existente, o prefixo resolve univocamente para o SHA completo lido. Antes da colagem, a sessão registra no RELATO `LEITURA-COPIA <ID-do-aval> <OBRA> <cópia> <SHA-lido> <fim-com-fuso> <referência-da-resposta>`, após leitura Codex pelo wrapper, `rc 0` e amostra conferida de arquivo:linha. Janela proposta de **60 min** entre o fim da leitura e a colagem. O hook aceita HEAD igual ou descendente quando **todos** os commits de `SHA-lido..HEAD` pertencem à mesma volta já autorizada e em curso nessa cópia. A sessão que faz o commit grava nele `Volta-Autorizada: <ID-do-aval-que-abriu-a-volta>/<OBRA>/<cópia>/<volta>`, conferido contra o despacho vigente no RELATO. Commit importado entra no exame do delta, inclusive por merge. O hook guarda o HEAD validado e o despacho repete a checagem se ele andar. `bin/codex_pergunta.sh:266–273,304–319,341–347`, `CLAUDE.md:357–370,439–442`, `bin/raias_estado.py:89–102`.
- M6b. **Custo em recolagens por 24 h.** No cenário de uma colagem com prefixo anterior por cada uma das 41 entradas fora do main, tratando cada entrada como avanço da própria volta com prova recente, V3 = **41**, M6a = **0**. Na medida real, M6a = **0 por avanço próprio coberto**. Recolagens por leitura vencida, prova ausente ou delta alheio [nao verificado]. Para calcular o total, cruzar F9 com hora/SHA de cada leitura e atribuição dos commits por volta. `medida_t2.txt:1–178`, M6a–M7. [nao verificado]
- M7. ALVOS com nome repetido, ambíguo, omitido, sem HEAD ou com prefixo divergente segundo M6a devolvem `AVAL SEM LEITURA DA COPIA (HEAD atual <hash>)` por alvo, com `indisponível` onde nenhum HEAD puder ser lido. `copia=nova` vale para um alvo sem worktree elegível nem declaração, inclusive quando há outros alvos existentes no mesmo aval. Uma declaração sem worktree exige saneamento para aquele alvo. A sessão registra `AVAL-DEVOLVIDO <ID>` no topo do RELATO pelo escritor existente, como recado que preserva a janela do Stop, e segue os itens andáveis. Ordens sem o token no cabeçalho conservam a passagem atual pelo detector e pelas guardas próprias. `bin/raias_estado.py:73–102`, `bin/hook_stop_fila1.py:413–524,1381–1404`, `app/core/esteira_vigia.py:846–892`, `CLAUDE.md:262–275,436–438`, `bin/codex_pergunta.sh:252–259`.
- M8. Fronteira T2 = primeiro evento de controle comprovado com os registros carregados na sessão de trabalho. Registrar data/hora com fuso, sessão, SHA do instrumento e digest do settings. Aval recebido antes, provado em PROMPTS/BACKLOG, segue sem releitura. Toda recolagem após o marco é novo evento de validação. Recolagem válida de título já recebido segue a resposta de duplicata da casa. `CLAUDE.md:436–438`, `.claude/settings.json:2–73`, `app/docs/RELATO.md:3341,4356`.
- Alternativa T1 em uma linha. Um `bin/pista_pede.sh pre-opus <OBRA> <cópia>` pode carregar obra e cópia no argv, usando o mesmo leitor de PRONTO e a mesma interface SSH. `app/docs/BACKLOG.md:2191,2195`.
- Alternativa T2 em uma linha. Segurar o despacho até leitura Codex do HEAD e do delta no recebimento, reaproveitando a colagem se o delta for só da volta autorizada. **0/41 recolagens** no mesmo cenário, uma releitura por evento com delta, total por delta alheio [nao verificado]. `medida_t2.txt:1–178`, `CLAUDE.md:357–370`, M6a–M6b.
- M9. **(d), ficha.** O chat publica `FICHA <ID>` na forma `PROBLEMA / NUMEROS / LEIS / PROIBIDO / NAO MUDA / PERGUNTAS`. Cada número leva sua sonda. A ficha não leva desenho. Uma decisão registrada aparece em linha `DECISAO` com sua referência no fim, na forma `DECISAO <enunciado> | <arquivo>:<linha>`. A porta também reconhece `DECISAO:` e `DECISAO DO CHAT` no início de linha como declarações de decisão. Uma ficha cujo corpo só traz `PERGUNTAS` passa por esta porta. `ficha_sem_decisao_2011_0200.txt:6–7,14`, `aval_rascunhado_pelo_codex_1911_1838.txt:4–5`, `fichas_v3_para_rascunho_2011_0205.txt:4`.
- M9a. **(d), entrega e recusa.** A sessão entrega a ficha em chamada Bash dedicada, com `CODEX_PAPEL=leitura`, `CODEX_OBRA=<ID>-RASCUNHO` e o caminho absoluto literal da ficha no primeiro `--anexo`. Os briefs seguem nos demais anexos. O primeiro anexo tem cabeçalho `FICHA <ID>` correspondente à chamada. Usar uma versão fechada da ficha e outro caminho para a correção. O ramo do (d) no registro Bash de M3 confere, antes do wrapper, cada linha `DECISAO` desse anexo. Para `r.txt`, o caminho deve ser

[truncado em 20000 de 44759 caracteres]

## 20/11 02:02 · sessao 8485b295327f · CADEIA-20-PASSO-1 · leitura · origem=Code

aval Ronald: CADEIA-20-IMPRESSAO-V3 (rascunho Codex 20/11) -- unidade: célula-dia, impressão da ata; lei: L-124, desenho célula soberana (`app/docs/LEIS.md:145`, `app/docs/GLOSSARIO.md:11`)
PAREI — (i)+(ii) não demonstram 9/9. A 129134 ainda muda se P3 atravessar o fim do turno. As 128355, 129943 e 138359 perdem o último fio vivo e podem sobrar em P2 se a mudança cair fora das passadas internas. A etapa escritora dessas três e o resultado a relógio fixo [nao verificado]. `app/ponto/services/cartorio.py:1152–1164,1179–1181,1200–1231,1540–1558`

## RESPOSTAS Q1–Q4

**Q1 — Granularidade.** Para escolher códigos, `classificar_dia` lê zero/não zero de `chamados_vivos`, `chamados_total` e `cobrancas_vivas`. Usa a contagem exata de `chamados_vivos` nos textos de três diagnósticos. O cartório descarta esses textos. O comando `supra_juiz` os publica e usa o veredito já lavrado para decidir a retratação. `emitir_furo_retroativo` consome os códigos. `app/ponto/supra_juiz.py:326–329,373–380,467–484,507–512,543–549` `app/ponto/services/cartorio.py:1614–1627` `app/ponto/management/commands/supra_juiz.py:211–245` `app/ponto/management/commands/emitir_furo_retroativo.py:94–112`
- No cartório, `chamados_total` é presença de qualquer chamado de batida no dia, inclusive morto. `chamados_vivos` é presença no subconjunto que protesta (`VIVOS`, dia de trabalho e lastro que não absolve). `cobrancas_vivas` é presença de um vivo com fio C6. A escolha direta de `cobrado` usa ainda a **interseção** fio vivo × protesta quando não há códigos. Dois dias com as mesmas contagens podem divergir nessa interseção. A emissão lê também a presença de qualquer vivo. O ramo `nunca_bateu` passa os três zeros. `app/ponto/services/cartorio.py:141–157,1399–1415,1527–1545,1605–1653,1889–1897` `app/ponto/services/lastro.py:153–181`
- `status_local` entra literal no hash atual como `pk:status_local`. Para o veredito, sua leitura direta é pertinência a `VIVOS`. C6 lê também a pertinência do destino a `ENCERRADOS` no fallback sem slot. Os gates de emissão e fecho ainda operam sobre os chamados individuais. `aberto` e `em_analise` pertencem aos mesmos conjuntos nesses leitores da palavra. `app/chamados/catalogo/motor.py:24–32,98–113` `app/chamados/juizes.py:769–789,1482–1491` `app/ponto/services/cartorio.py:502–507,1527–1532,1691–1738,1804–1835`
- As referências da ficha ao main correspondem, nesta cópia, a `app/ponto/services/cartorio.py:530–547` para fio no hash, `app/ponto/services/cartorio.py:1200–1209,1220–1223` para o gatilho da releitura e `app/chamados/signals.py:189–218` para o sinal. A fila citada na ficha como `1045–1049` é consumida aqui em `app/ponto/services/cartorio.py:1147–1164`.

**Q2 — Escritores e cobertura.**
- **Enfileiram.** Fechos por marco aceso, inexistente ou cobrança indevida, dedup e ressuscitação bem-sucedida escrevem depois de calcular a impressão e antes de gravar o carimbo físico. Depois do primeiro carimbo, o fechamento adiado enfileira a célula se `reconciliar()` fechar. A segunda passada força só as enfileiradas, com a janela original. O que for enfileirado nessa segunda passada ou na releitura vai ao contador de descarte. `app/ponto/services/cartorio.py:1140–1164,1229–1231,1541–1547,1691–1738,1812–1861,2035–2037`
- **Releitura condicionada.** Abertura ou reabertura pelo emissor que sobe `emitidos` ativa a releitura após a fábrica. `_espelhar_marcos` escreve o contexto que o lastro lê, sem fila própria. Se mover a granularidade, só a releitura acionada por emissão pode cobri-lo hoje. O carimbo também muda a ata e o veredito que C6 lê. `app/ponto/services/cartorio.py:1193–1223,1952–1955,1990–2037,2136–2146` `app/ponto/management/commands/detectar_ausencias.py:544–569` `app/ponto/services/lastro.py:153–181` `app/chamados/juizes.py:1418–1420,1450–1469`
- **Nenhuma cobertura garantida.** E2 carimba perguntas do colaborador e pode fechar disputa e chamado-pai. Só o fecho bem-sucedido da lista adiada enfileira a célula. A fábrica, depois de consumida a fila, pode descarimbar, criar perguntas, chamar `analisar()` e fechar disputa com cascata. Essas escritas não sobem `emitidos` por si. O cluster abre ou fecha chamado de `cluster_batida_espuria`, fora dos quatro módulos da lista do cartório. Efeito indireto do cluster nas nove células [nao verificado]. `app/ponto/services/cartorio.py:1126–1184,1209–1223` `app/chamados/reconciliador.py:634–665` `app/chamados/services/fabrica_da_celula.py:160–174,206–219` `app/chamados/services/disputa_emissao.py:1180–1192,1255–1302` `app/chamados/signals.py:19–46,149–165` `app/ponto/services/cluster_espurio.py:11,176–216` `app/chamados/catalogo/furo_modulos.py:15–16`
- **Depois do cartório, nenhuma das duas passadas internas.** `apurar_furos_diarios` agrupa e reconcilia filhos de adesão e limbo, inclusive reapontando perguntas no limbo. `supra_juiz --executar` pode reconciliar e resolver perguntas. `flip_automatico --apply` grava tipo de batida, aciona o sinal da batida e recompõe turnos. O consumidor contínuo de `EventoCelula`, que roda em paralelo à corrente, carimba, reconcilia e roda a fábrica. Para fechos com mudança de estado vale Q4. `app/config/crons.py:394,599,707,1527–1545` `app/escala/management/commands/apurar_furos_diarios.py:13–21` `app/chamados/services/adesao.py:103–120,143–154` `app/chamados/services/limbo_folgas.py:267–289` `app/ponto/management/commands/supra_juiz.py:257–292` `app/chamados/reconciliador.py:572–590,1414–1438` `app/ponto/management/commands/flip_automatico.py:51–79` `app/ponto/services/flip_batida.py:28–39` `app/ponto/registro_batida.py:255–274` `app/ponto/signals.py:85–93,131–135`
- `disparar_perguntas_competencia`, `reconciliar_perguntas_orfas`, `fechar_becos_disputa`, `detectar_intervalo_ausente` e `silenciar_chamados_isentos`, com os argumentos agendados, são vigias desses insumos. Para `reeavaliar_ausencias_feriado`, conferi o agendamento `--alarme`; corpo [nao verificado]. Os elos seguintes de lavra e alerta estão declarados no cron; censo transitivo de seus efeitos sobre fio [nao verificado]. `app/config/crons.py:491,608–609,661–663,1539–1811` `app/chamados/management/commands/disparar_perguntas_competencia.py:130–145` `app/ponto/management/commands/reconciliar_perguntas_orfas.py:22–34` `app/chamados/management/commands/fechar_becos_disputa.py:23–30` `app/ponto/management/commands/detectar_intervalo_ausente.py:98–108,138–139` `app/chamados/management/commands/silenciar_chamados_isentos.py:23–44`

**Q3 — Nove células, baseline medida e inferência sobre (i)+(ii).**
- **128355, 129943, 138359.** `tres.out`, registros `P2 pk=` correspondentes, mostra `em_analise → resolvido` no último vivo, com `fio:1` sumindo e ata intacta. A granularidade muda de presente para ausente em vivos e fio. (i) preserva essa diferença. (ii) a traz para P1 se o escritor e a célula estiverem cobertos no mesmo passe. Se a mudança vier depois da última leitura coberta, sobra para P2. Escritor efetivo e passe residual após a mudança [nao verificado]. `app/ponto/services/cartorio.py:1126–1164,1200–1231,1527–1545`
- **129476, 129490.** `insumo.out`, registros `P2 pk=129476` e `P2 pk=129490`, mostra `fio:1 → 2` nas duas e `aberto → em_analise` na segunda. (i) elimina esses deltas de contagem e rótulo se a presença de fio que protesta ficar igual. Quais chamados tinham fio e protestavam antes/depois, e eventual residual em P2/P3 [nao verificado]. `app/ponto/services/cartorio.py:1527–1545,1605–1653` `app/chamados/catalogo/motor.py:24–32`
- **138541, 142995, 143945.** Os IDs estão no V2 e na ficha. O corte exibido de `insumo.out` termina durante `P2 pk=131231`, antes das linhas de 138359, 138541, 142995 e 143945. `tres.out` recupera 138359. Faltam os deltas de três das nove no material combinado. Granularidade, escritor e residual destas três [nao verificado].
- **129134.** `insumo.out`, `P2 pk=129134`, mostra primeira lavra `concorde`, sem chamado e com `enc:0`. `tres.out`, `P3 pk=129134`, mostra `concorde → cobrado`, sai `enc:0` e entram chamado 43141 `em_analise` e `fio:1`. Se P3 cruza o fim do turno, a mudança continua exigida mesmo com (i)+(ii). A relógio fixo, a emissão seguida de releitura é um caminho existente no braço encerrado. Resultado nos dois braços e instante efetivo da sonda [nao verificado]. `app/ponto/turnos.py:2124–2134` `app/ponto/services/cartorio.py:523–532,1193–1223,1293–1297,1540–1545,1624,1980–1993`

**Q4 — Fora do passe.** Criação ou mudança de `status_local` por `save`, com dia do fato identificável e fora de `em_julgamento`, tenta `julgar_celula` no ato, seja porta humana ou cron. A chamada usa `forcar=False`. A porta avulsa pode recusar por data ou turno. Se a célula do ramo normal já tem carimbo, impressão igual e veredito preenchido diferente de `nunca_bateu`, o corpo pula. A palavra fica velha se a impressão igual encobrir uma mudança relevante, por exemplo fio que deixa de protestar enquanto a presença global de fio e a de chamados que protestam permanecem. Save de pergunta aciona outbox/beco, sem chamada direta ao cartório. Se não houver fecho de chamado ou outro fato que o acione, a atualização da palavra fica para o próximo julgamento que veja a impressão diferente. `app/chamados/signals.py:180–218,229–259` `app/chamados/models.py:3416–3445` `app/ponto/services/cartorio.py:1552–1558,1605–1653,2174–2205`

## FONTE

- **Base e autoridade.** CADEIA-20-PASSO-1, volta 3, caminho da cópia registrado em `app/docs/RELATO.md:100`. Hash informado `a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6`. A árvore recebida veio sem metadados Git. Identidade do HEAD [nao verificado]. Decisões e precedência dos anexos 1815/2325 e da Ficha 1 vêm dos literais fornecidos; aprovação do V2 [nao verificado na árvore].
- **Impressão e lavra.** SHA-256 de batidas (`pk`, tipo, instante), status da ausência, chamados de batida (`pk`, estado), DNA (versão e marcos) e condicionais `veto`, `teto`, `regra`, `enc`, `fio`, `cobparcial`, `isento`, `sem_adesao`. A parcial incorpora período e índices dispensados quando pertinentes. A porta da célula persiste impressão e lavra. Q1–Q4 localizam os leitores e escritores da mudança. `app/ponto/services/cartorio.py:477–578,1527–1558,2012–2037` `app/ponto/portas/celula.py:1189–1201`
- **Horizonte e relógio.** `versao_para` toma a maior versão aplicável à competência pelo corte da empresa. V1 começa em 2026-09 e v2–v5 em 2026-10. O lote captura `agora` e o repassa ao corpo. C6 já aceita esse instante. `app/docs/LEIS.md:145` `app/ponto/janelas.py:36–63` `app/ponto/services/cartorio.py:161–166,280–299,1110–1123,1333–1337,1399–1401,1541–1547` `app/ponto/management/commands/processar_cartorio.py:40–41,63–69` `app/chamados/juizes.py:866–898`
- **Entrega e dinheiro.** `_K6` pergunta por TXT vigente e não invalidado na tripla empresa × mês × ano. A grade da ata alimenta previsto, abono, realizado, retenção e `DiaPago`. DINHEIRO item 1 exige reversão anterior, exportada intacta e prova de frota no mesmo marco quando há apply admitido. `app/core/juizes.py:334–342` `app/folha/export.py:329–338,556–592` `app/ponto/services/fechamento.py:1046–1068,1181–1202` `CLAUDE.md:401–417`
- **Sondas.** `tres.out`, bloco de diffs P2/P3 até `FIM_SONDA_CELULA`, mostra zero `null → []` nesses passes. Em `P1_0808`, 12711/08 é de 21/07/2026, segue `furo` e muda `razoes_orfas` de `null` para `[]`, junto de `33432:aberto → em_analise`. Os hashes abreviados de FM e ED coincidem nesse anexo. Filtro, colunas, resposta de `_K6`, identidade das 19 e causalidade por célula [nao verificado].

## MUDA

1. **Granularidade e passadas.** (i) Desde o horizonte de MUDA 3, codificar nos trechos chamado/fio a presença de chamado total, de vivo, de vivo que protesta, de vivo com fio C6 e de vivo com fio C6 que protesta, pelas mesmas leituras do julgamento. Preservar a distinção entre ausência de vivo e vivo sem fio. A impressão não ganha assinatura de ato humano. (ii) Toda escrita da cadeia que altere essa projeção depois de lida para carimbar, inclusive após o carimbo, deve levar a célula à segunda passada existente por `out['rejulgar']` ou ficar coberta pela releitura existente. Antes do código, publicar escritor, projeção antes/depois, ordem e cobertura por célula. Se alguma escrita ficar sem cobertura nessas duas vias, PAREI nesse ponto, com a célula e a etapa identificadas. `app/ponto/services/cartorio.py:502–507,533–547,1123–1231,1527–1558,1605–1653,1889–1897,2035–2037`
2. **Relógio.** Passar o `agora` do cartório a `fio_vivo` e deste a `pergunta_viva`. Preservar a chamada existente dos outros leitores quando omitem o argumento. `app/ponto/services/cartorio.py:1110–1123,1532,2202–2205` `app/chamados/juizes.py:866–898,1382–1404,1510–1519`
3. **Vigência.** Usar `versao_para` e o horizonte default **2026-10** da volta 3. Até 2026-09, conservar byte a byte a impressão anterior para os mesmos insumos e a foto gravada sem fato novo. Em fato novo, vale o caminho anterior à mudança. Manter a leitura e a lavra da razão da sobra pela v5. `app/docs/LEIS.md:145` `app/ponto/services/cartorio.py:161–166,280–299,1038–1056,1333–1337,1552–1558` `app/escala/services/leitor_celula.py:194–222`
4. **Entregue.** Consultar `_K6` pela competência do dia antes de qualquer julgamento ou escrita desta obra, inclusive efeitos da releitura e da fábrica. Preservar a janela de leitura dos turnos que atravessam a borda. Provar zero escrita em célula entregue, além do hash de FM e ED. `app/folha/export.py:556–590` `app/ponto/janelas.py:60–63` `app/ponto/services/cartorio.py:1075–1079,1152–1181,1199–1222` `app/ponto/portas/celula.py:1189–1201`
5. **FM/ED, literal proposto.** Empresas **2, 3, 4** × competências **2026-08, 2026-09, 2026-10, 2026-11**. Agosto e novembro aparecem nas células da sonda. Setembro e outubro aparecem na declaração de versões e nos anexos 1815/2325. O filtro dos hashes antigos [nao verificado]. Fotografar imediatamente antes de P1 e após P3, no mesmo ensaio, antes de recálculo financeiro. FM por empresa/ano/mês/colab/PK e ED por empresa/ano/mês/PK, com **todas as colunas persistidas**, inclusive carimbos. Em ED incluir conteúdo, `hash_sha256`, `invalidada_em`, `invalidada_motivo` e `substituida_por_id`, com todas as versões. Ordenar pelas chaves, usar a mesma serialização canônica e publicar contagem e SHA-256 por modelo × empresa × competência, inclusive recortes vazios. `app/ponto/models.py:398–485` `app/folha/models.py:75–104,143–186`
6. **Aberta.** Antes do pouso, medir main × cópia a partir do mesmo dump e relógio, por empresa × colab × competência aberta alcançada. Usar o mesmo recálculo em ambos. Comparar presença da linha, rubricas, saldo, turnos, previsto/abonado/realizado, retenção (`motivos_espelho` e classificação de exportação) e linhas `DiaPago` por data/tipo/versão, inclusive ajuste. Movimento = `PAREI` com empresa, colab, competência, campo, antes/depois e dias afetados. Em 10, manter a Pauta DP de 2325 para 191, 203, 922 e 87 e o acerto a cargo do DP, conforme anexo 2325 [nao verificado na árvore]. `CLAUDE.md:401–417` `app/ponto/models.py:315–351,412–468` `app/ponto/services/fechamento.py:1046–1068,1191–1202` `app/folha/export.py:765,1301`
7. **(A) `null → []`.** Registrar as 19 do levantamento anterior e o zero observado no P2/P3 do dump novo. Sem varredura de normalização. Manter 12711/08 como testemunha do P1 e informar sua resposta de `_K6` no ensaio. Identidade das 19 e resposta de `_K6` para 12711 [nao verificado]. `app/ponto/services/cartorio.py:962–971` `app/docs/LEIS.md:145`

## RED

- **R1 — Oito células.** `128355`, `129476`, `129490`, `129943`, `138359`, `138541`, `142995`, `143945`, todas no recorte informado de 11/2026. Entradas observadas incluem `em_analise → resolvido` com `fio:1` sumindo (128355/129943/138359), só `fio:1 → 2` (129476) e `aberto → em_analise` com `fio:1 → 2` (129490). Detalhe das outras entradas [nao verificado]. Sem ato humano nem fato externo novo, ler `CelulaDia.impressao` e `veredito` por PK após P1, P2 e P3. Esperado **8/8 estáveis em P1=P2 e P2=P3**. As diferenças da base são as do anexo `insumo.out` [nao verificado por reexecução]. `app/ponto/services/cartorio.py:1527–1558` `app/ponto/portas/celula.py:1194–1201`
- **R2 — 129134, col474, 19/11, competência 11.** Entrada do anexo `tres.out` é `enc:0`, sem chamado, depois `fio:1` e chamado 43141 com `concorde → cobrado`. Causa exata [nao verificado]. Rodar três passes com o mesmo instante em cada braço, um em curso e outro já encerrado pelo juiz do turno. Em curso, a regra conserva `concorde` sem cobrança prematura. Encerrado, com cobrança devida e fio vivo, a primeira rodada completa deve sair `cobrado`, com uma só cobrança. Esperado **impressão e veredito iguais em P1=P2=P3** em cada braço. Atravessar o encerramento num controle separado deve permitir o rejulgamento exigido pelo juiz vigente. `app/ponto/services/cartorio.py:523–532,1193–1223,1293–1297,1540–1545,1614–1624,1975–1993` `app/ponto/turnos.py:2124–2134` `app/ponto/supra_juiz.py:317–329,450–463`
- **R3 — Ato entre passes e granularidade.** H1: célula passada, aberta e carimbada no horizonte, um único chamado de batida `em_analise`, premissa regularizada e sem pergunta viva que barre o fecho. Após P1, gestor faz POST real em `detalhe_local` com `novo_status=resolvido`. Esperado: o sinal chama o cartório com `forcar=False`, a projeção perde o último vivo, a impressão muda antes de o POST retornar e P2 já encontra o carimbo em dia. H2: na fábrica, `analisar()` muda `aberto → em_analise`, com as cinco presenças da projeção iguais. Esperado: delta de impressão zero e zero reentrada pelo sinal. H3: validação humana da única pergunta viva, com chamado que permanece vivo. Esperado: evento da pergunta, zero chamada direta do seu sinal ao cartório, e o próximo passe sem `forcar` vê fio 1→0 e atualiza a palavra conforme os códigos. H4: dois vivos, só um protesta, um único fio vivo muda do que protesta para o que não protesta, com os demais insumos e um dia sem códigos fixos. Esperado: impressão diferente e `cobrado → concorde` no julgamento. `app/chamados/urls.py:14,33` `app/chamados/services/acoes_chamado.py:292–297,322–331` `app/chamados/models.py:149–205,859–866` `app/chamados/services/disputa_emissao.py:1180–1192` `app/chamados/services/validacao.py:161–175` `app/chamados/signals.py:189–218,252–259` `app/ponto/services/cartorio.py:1552–1558,1605–1653,2174–2205` `app/escala/models.py:1624–1632`
- **R4 — Horizonte e entregue.** Fixtures `H09` e `H10` abertas e já carimbadas, com a mesma mudança exclusivamente derivada após P1. H09 mantém impressão, ata, veredito e `julgada_em` da base na ausência de fato novo. H10 usa a regra nova e passa R1. Fixture `E10` com TXT vigente e insumo suficiente para relavrar fica intacta em P1–P3 e tem **zero chamadas às portas de escrita**. Controle `I10`, mesma competência com último TXT invalidado sem substituta, entra no caminho de aberta. Para 12711, se `_K6` responder entregue, o esperado é zero toque inclusive em P1. `app/docs/LEIS.md:145` `app/folha/export.py:556–590` `app/escala/tests/test_cadeia20_volta3.py:142–158` `app/ponto/portas/celula.py:1189–1201`
- **R5 — Relógio injetado.** Fixture T1 com pergunta viva às 10:00 e validação datada às 10:01. Fixar relógio ambiente às 10:02. `fio_vivo` deve responder vivo com `agora=10:00` e morto com `agora=10:02`. As duas consultas usam a mesma pergunta. `app/chamados/juizes.py:886–898,1382–1406,1510–1519`

## SELO

- Django na pista da cópia e prova de frota na sombra. Para R1/R2, três rodadas de `processar_cartorio --apply` sem `--forcar` externo, empresas 2, 3, 4 nessa ordem em cada rodada. Usar o mesmo dump por braço, relógio local congelado em `2026-11-20T06:27:00-03:00` como literal proposto para o braço encerrado, e `2026-11-20T00:30:00-03:00` para o controle em curso de 129134. Confirmar os dois lados pelo juiz do turno. Tirar cada foto depois de E2, fechamento adiado, fábrica e releitura internos. Os instantes originais exatos das sondas [nao verificado]. `CLAUDE.md:84–

[truncado em 20000 de 27725 caracteres]

## 20/11 02:01 · sessao 00632872258d · TRAVAS-SEM-MEMORIA · leitura · origem=Code

aval Ronald: TRAVAS-SEM-MEMORIA-V4 (rascunho Codex 20/11) -- unidade: hook e ESTADO; lei: nenhuma nova

## FONTE

- F0. FICHA 2, alíneas (a)–(c), rege este rascunho sobre o V3, que não foi aprovado. Base informada `eb1e49c1ec1055e6bf43d7f67df63b17054b474e`. A cópia entregue veio por `git archive`, sem `.git`. `HEAD = main = hash`, carga dos hooks na VM, executor da pista e resultados de smoke [nao verificado]. `fichas_v3_para_rascunho_2011_0205.txt:1,24–28`, `CLAUDE.md:344–348`.
- F1. `suite.sh` resolve `--volta` e julga a seleção efetiva. Seleção vazia ou que cobre a descoberta toda é inteira. Inteira sem motivo canônico sai `rc 2`. A volta intermediária junta ruff, selo do disjuntor e recorte com vizinhos e selos globais. `bin/suite.sh:156–180,215–263`, `bin/volta_intermediaria.sh:18–46`.
- F2. Régua tem runner e carimbo próprios. O Git entrega argumentos e stdin ao wrapper do pre-push, que decide se reaproveita a régua e roda `tenants` em runner separado. O pouso usa `suite.sh` pela trava da VM. `bin/regua.sh:46–51,84–101,165–169`, `bin/hooks.sh:58–65`, `bin/pre-push.sh:6–8,27–74,159–200`, `CLAUDE.md:95–106`.
- F3. A opção A registra volta e pre-opus de lateral na pista, espinha e pouso na VM. A interface da pista recebe commit em `refs/pista/<sha>`, `pede <sha> [--only <labels>] [--parallel N]` e `espera <id>`. Há registros de `rc 0`, `rc 1` e `rc 71` sem a ref. `app/docs/RELATO.md:65,201,1497,1529`, `app/docs/BACKLOG.md:2191–2195`.
- F4. `PreToolUse` recebe o comando externo em `tool_input.command`. O disjuntor atual pode sair cedo para Bash da sessão principal. `UserPromptSubmit` está ausente do settings entregue. Stop lê `transcript_path` e cobra a fila pelas portas vigentes. A abertura em `app/` já deixou hooks da raiz inertes. `.claude/settings.json:2–73`, `bin/disjuntor.py:1837–1843,1864–1867,1912–1933`, `bin/hook_stop_fila1.py:1339–1345,1381–1404`, `app/docs/RELATO.md:3341`, `app/docs/PROMPTS.md:1877`.
- F5. `raias_estado.py` cruza worktrees com declarações do escritor `raia_agente.sh`. A união inclui declaradas, à frente e só sujas. Hoje as só sujas saem por nome, há declaração sem caminho e o HEAD por cópia não sai. `gerar_estado.py` lê o medidor e `relato.sh` gera, raspa e publica o ESTADO. `bin/raias_estado.py:42–135,138–174`, `bin/raia_agente.sh:40–59`, `bin/gerar_estado.py:211–229,329–331`, `bin/relato.sh:30–38,114–119,133–147`.
- F6. O construtor Codex escreve só em `<wt>/app`, com uma vaga por repo. `.claude/` consta no ignore e há precedente de instalação local negada pelo classificador. O arquivo do RELATO guarda marcos antigos. `CLAUDE.md:349–356,394,436–442`, `bin/codex_pergunta.sh:121–135,248–259`, `.gitignore:66`, `app/docs/RELATO.md:4356,4411`.
- F7. Janela de 24 h encerrada em 20/11 às 01:54 −03. F5 tem **58 entradas** em 18 horas com commits, pico de 11 às 08h. CADEIA tem **49** em 11 horas, pico de 17 às 16h. `main` tem **33**. São 107 entradas somadas nas duas cópias, 41 fora da lista do main (25 F5 + 16 CADEIA), com histórico alcançável contado pelo `git log`. Mudanças efetivas de HEAD e autoria por volta [nao verificado]. `medida_t2.txt:1–178`. [nao verificado]
- F8. Contagem V3 em 19/11. Contar uma devolução por evento recebido com token literal `CONSTRUIR` na primeira linha e pelo menos um alvo existente cujo prefixo colado difira do HEAD naquele recebimento. Tratar ausência de token de cópia, `copia=nova` e duplicata conforme suas portas. A lista de **seis candidatos de construção pelo título ou resumo** é `E4C-F5-CONSTROI-1911-0815`, `CADEIA-20-PASSO-1-CONSTROI-1911-1010`, `DST-L2-JZ1-C7-CONSTROI-1911-1200`, `E4C-F1-B2B-F0-CONSTROI-1911-1315`, `FERIADO-CONSTROI-E-C8-MEDE-1911-1400` e `E4C-F5-CONSTROI-A-1911-1710`. `medida_t2.txt:194,202,216,222,225,242`, `app/docs/PROMPTS.md:2608,2616,2630,2636,2639`, `CLAUDE.md:436–438`.
- F9. **0 devoluções por divergência demonstráveis, lista confirmada vazia, total real [nao verificado].** Os resumos também registram complementos que liberam ou retomam construção, inclusive F5 1735/1830 e CADEIA 1815/1836. O título com `CONSTROI` e o resumo com `CONSTRUIR` não identificam o token na primeira linha colada. Falta, por evento, o texto recebido com alvos/prefixos, o instante de recebimento e a fotografia de worktrees/HEADs nesse instante, inclusive das obras fora das duas listas. Exemplo medido de exposição: o merge F5 `3f1d63863` é das 17:07 e o 1710 foi recebido às 17:2x, mas o anexo não traz o prefixo colado, nem prova o HEAD naquele evento. `medida_t2.txt:37,181–254`, `app/docs/RELATO.md:60,111,119,158`.

## MUDA

- M1. A sessão constrói em cópia e instala os dois registros locais da raiz, somente para este instrumento. Codex verificador lê o diff depois da construção. Antes do código, publicar as cinco linhas abaixo no RELATO e mandá-las uma vez ao Codex. A execução depende de `aval Ronald: APROVA RASCUNHO TRAVAS-SEM-MEMORIA-V4 <sha256 12>`. O rascunho integral com SHA vai ao RELATO, e F5 C2 segue na frente. `fichas_v3_para_rascunho_2011_0205.txt:1`, `aval_rascunhado_pelo_codex_1911_1838.txt:5–7`, `CLAUDE.md:309,349–356,372–394`.
- M2. T1 usa a rota declarada pela sessão no despacho. Para a inteira pre-opus de **lateral ou espinha**, o pedido local carrega `PISTA_LATERAL=<OBRA>` junto de `bin/suite.sh --dir <cópia> --motivo "pre-opus <OBRA>"`. `PISTA_LATERAL` conserva o nome da chave e carrega o ID nas duas origens. Inteira pre-opus com chave ausente ou de outra obra é recusada antes de SSH e de runner VM. A exceção NOMINAL ao contrato PISTA-2 item 3 vale só para **“nem a suite que o opus recebe”**. O pouso segue pela trava da VM e certifica. Divergência VM × pista no mesmo commit = relatar e suspender o uso da pista até Ronald ver. `fichas_v3_para_rascunho_2011_0205.txt:25`, `app/docs/BACKLOG.md:2191,2194–2195`, `app/docs/RELATO.md:65,81`.
- M2, volta. Para volta de lateral, `PISTA_LATERAL=<OBRA>` acompanha `bin/volta_intermediaria.sh <cópia> <base> <saída>`. O script da volta conserva ruff e selo e passa o recorte efetivo ao ramo remoto de `suite.sh`. Volta cujo recorte cobre tudo continua sujeita ao `rc 2` da guarda atual. Régua, pre-push, pouso e teste focado mantêm seus comandos. `bin/suite.sh:129–180,215–263`, `bin/volta_intermediaria.sh:18–46`, `CLAUDE.md:95–106`, `app/docs/RELATO.md:65,81`.
- M3. Registrar um `PreToolUse` de Bash para a porta canônica. Ele lê `PISTA_LATERAL`, a obra do motivo e o SHA completo da cópia. Requer o mesmo ID nos dois campos e, para a inteira pre-opus, uma linha literal no início da linha `PRONTO <OBRA> item a item: sha=<SHA completo>` em `app/docs/RELATO.md` ou `app/docs/RELATO-ARQUIVO.md`. Um registro arquivado vale para o mesmo SHA. Cabeçalho de outro SHA, de outra obra, indentado ou citado vale como ausente. Resposta `SEM PRONTO ITEM A ITEM`. O mesmo verificador de instrumento roda no executor imediatamente antes de criar a ref e pedir o job. O SSH recebe o SHA conferido e as opções documentadas. `bin/disjuntor.py:1796–1798,1837–1843`, `bin/suite.sh:219–263`, `CLAUDE.md:439–442`, `app/docs/BACKLOG.md:2191`.
- M4. A sessão publica o PRONTO com cada item, medida e pendência. O hook confere identidade da obra e SHA. Após mais de três voltas Codex, pendências registradas permitem a inteira e a subida ao opus pela regra vigente. Uma recusa de PRONTO fecha o pedido antes do SSH. Falha de infraestrutura segue a contingência registrada da rota, com `PISTA: VM porque <motivo>` quando couber. Vermelho de teste retorna o rc do job. `CLAUDE.md:372–381`, `app/docs/RELATO.md:81,1010,1209`, `app/docs/BACKLOG.md:2191`.
- M5. T2 faz `raias_estado.py` expor o nome e o HEAD completo observado por `git rev-parse HEAD` em cada worktree da união, incluindo as só sujas. O ESTADO publica um `copia=<nome>@<HEAD12>` por cópia resolvida. Raia declarada sem caminho ou sem HEAD resolvível publica `raia=<nome> SEM WORKTREE`. O hook lê o mesmo medidor na hora do envio. O ESTADO é a fotografia publicada. `bin/raias_estado.py:42–48,73–135,138–174`, `bin/gerar_estado.py:211–224`, `bin/relato.sh:37–38,114–119`.
- M6. `UserPromptSubmit` examina aval colado com o token literal `CONSTRUIR` na primeira linha. Na linha `alvos=<nome>,<nome>`, listar todas as cópias tocadas e dar, na mesma ordem, um `copia=<nome>@<prefixo>` por alvo existente ou `copia=nova` na posição de cada alvo novo. Prefixo hexadecimal de 12 caracteres até o SHA completo. Nome é o basename da worktree, com escape percentual para delimitadores. `fora=<nome>,...` declara cópias vinculadas à obra que ficam fora do aval. Toda raia declarada com a mesma `OBRA` consta em `alvos` ou `fora`. A sessão explicita em `alvos` cópias tocadas sem declaração, inclusive descendentes. `bin/raias_estado.py:73–102,125–135`, `bin/raia_agente.sh:40–59`, `app/docs/RELATO.md:1513`, `travas_sem_memoria_v2_2011_0110.txt:5` [nao verificado].
- M6a. **Recomendada.** Por alvo existente, o prefixo resolve univocamente para o SHA completo lido. Antes da colagem, a sessão registra no RELATO `LEITURA-COPIA <ID-do-aval> <OBRA> <cópia> <SHA-lido> <fim-com-fuso> <referência-da-resposta>`, após leitura Codex pelo wrapper, `rc 0` e amostra conferida de arquivo:linha. Janela proposta de **60 min** entre o fim da leitura e a colagem. O hook aceita HEAD igual ou descendente quando **todos** os commits de `SHA-lido..HEAD` pertencem à mesma volta já autorizada e em curso nessa cópia. A sessão que faz o commit grava nele `Volta-Autorizada: <ID-do-aval-que-abriu-a-volta>/<OBRA>/<cópia>/<volta>`, conferido contra o despacho vigente no RELATO. Commit importado entra no exame do delta, inclusive por merge. O hook guarda o HEAD validado e o despacho repete a checagem se ele andar. `bin/codex_pergunta.sh:266–273,304–319,341–347`, `CLAUDE.md:357–370,439–442`, `bin/raias_estado.py:89–102`.
- M6b. **Custo em recolagens por 24 h.** No cenário de uma colagem com prefixo anterior por cada uma das 41 entradas fora do main, tratando cada entrada como avanço da própria volta com prova recente, V3 = **41**, M6a = **0**. Na medida real, M6a = **0 por avanço próprio coberto**. Recolagens por leitura vencida, prova ausente ou delta alheio [nao verificado]. Para calcular o total, cruzar F9 com hora/SHA de cada leitura e atribuição dos commits por volta. `medida_t2.txt:1–178`, M6a–M7. [nao verificado]
- M7. ALVOS com nome repetido, ambíguo, omitido, sem HEAD ou com prefixo divergente segundo M6a devolvem `AVAL SEM LEITURA DA COPIA (HEAD atual <hash>)` por alvo, com `indisponível` onde nenhum HEAD puder ser lido. `copia=nova` vale para um alvo sem worktree elegível nem declaração, inclusive quando há outros alvos existentes no mesmo aval. Uma declaração sem worktree exige saneamento para aquele alvo. A sessão registra `AVAL-DEVOLVIDO <ID>` no topo do RELATO pelo escritor existente, como recado que preserva a janela do Stop, e segue os itens andáveis. Ordens sem o token no cabeçalho conservam a passagem atual pelo detector e pelas guardas próprias. `bin/raias_estado.py:73–102`, `bin/hook_stop_fila1.py:413–524,1381–1404`, `app/core/esteira_vigia.py:846–892`, `CLAUDE.md:262–275,436–438`, `bin/codex_pergunta.sh:252–259`.
- M8. Fronteira T2 = primeiro evento de controle comprovado com os registros carregados na sessão de trabalho. Registrar data/hora com fuso, sessão, SHA do instrumento e digest do settings. Aval recebido antes, provado em PROMPTS/BACKLOG, segue sem releitura. Toda recolagem após o marco é novo evento de validação. Recolagem válida de título já recebido segue a resposta de duplicata da casa. `CLAUDE.md:436–438`, `.claude/settings.json:2–73`, `app/docs/RELATO.md:3341,4356`.
- Alternativa T1 em uma linha. Um `bin/pista_pede.sh pre-opus <OBRA> <cópia>` pode carregar obra e cópia no argv, usando o mesmo leitor de PRONTO e a mesma interface SSH. `app/docs/BACKLOG.md:2191,2195`.
- Alternativa T2 em uma linha. Segurar o despacho até leitura Codex do HEAD e do delta no recebimento, reaproveitando a colagem se o delta for só da volta autorizada. **0/41 recolagens** no mesmo cenário, uma releitura por evento com delta, total por delta alheio [nao verificado]. `medida_t2.txt:1–178`, `CLAUDE.md:357–370`, M6a–M6b.

### CONTRATO-DE-INSTRUMENTO

1. Quem chama — sessão/raia pelo Bash da raiz para T1, Code ao receber o aval para T2, `relato.sh` ao publicar o ESTADO. `.claude/settings.json:62–72`, `bin/relato.sh:37–38`, M2–M7.
2. Quem espera — sessão em shell de fundo, por `ssh pista espera <id>` e veredito com `Ran` e rc. T2 responde no evento de entrada. `app/docs/BACKLOG.md:2191`, `CLAUDE.md:316–319`, M7.
3. Falha/limite — recusa só do pedido ou aval afetado, antes de enfileirar ou usar o aval. Pista recebe árvore commitada. Divergência VM/pista fica registrada e suspende o uso até Ronald ver. `app/docs/BACKLOG.md:2191`, `CLAUDE.md:262–275`, M3–M8.
4. Medida — por caso, guardar porta, obra, SHA, rc/veredito, chamadas SSH/VM e linha de recusa ou publicação. Em job aceito, guardar id e tempo. `app/docs/RELATO.md:81,1497,1529`, M3–M8.
5. Quem desliga — Ronald, restaurando os dois registros anteriores e retirando a seleção da nova rota, com linha no RELATO. `app/docs/BACKLOG.md:2191`, `app/docs/RELATO.md:81,4356`, M1–M3.

## RED

Entradas de teste: `O=LATERAL-TESTE`, `C=wt-a`, `D=wt-b`, `H/J` = HEADs reais da fixture, `H0` = SHA anterior de C, `B` = base do recorte. `P` é `PISTA_LATERAL=LATERAL-TESTE bash bin/suite.sh --dir "$C" --motivo "pre-opus LATERAL-TESTE"`. `U` é colar `aval Ronald: LATERAL-TESTE CONSTRUIR` com o corpo do caso. Antes da cura, testar as mesmas entradas e registrar qual expectativa falhou. Resultados [nao verificado]. `CLAUDE.md:28–29`, `bin/tests/test_suite_motivo.sh:14–42`.

- **Pre-opus lateral** — P1 `P`, C commitada/ref apta, sem PRONTO → `SEM PRONTO ITEM A ITEM`, zero SSH e zero runner. P2 `P`, só há `PRONTO` de `O@H0` e de outra obra em H → mesma recusa. P3 `P`, marcador em `RELATO.md` para `O@H` → um `pede H` sem `--only`, id e `espera` com `Ran/OK/rc 0`. P4 `P`, mesmo marcador só no ARQUIVO, quatro voltas Codex e pendência escrita → mesmo pedido e subida ao opus após o verde. P5 `P`, PRONTO válido e job vermelho → `rc 1`, zero runner VM. `bin/suite.sh:236–263`, `app/docs/BACKLOG.md:2191`, `CLAUDE.md:372–381,439–442`, M3–M4.
- **Volta de lateral** — V1 `PISTA_LATERAL=LATERAL-TESTE bash bin/volta_intermediaria.sh "$C" "$B" "$OUT"`, C commitada e recorte próprio, sem PRONTO → ruff/selo e um `pede H --only <labels efetivas>`, zero trava VM. V2 mesmo comando, seleção cobre todos os módulos → guarda atual `rc 2` da suite, volta vermelha e zero SSH. V3 mesmo V1, pista indisponível e contingência vigente → recorte na VM e uma linha de causa. `bin/volta_intermediaria.sh:18–47`, `bin/suite.sh:162–180,215–263`, `app/docs/RELATO.md:81,1010`, M2–M4.
- **Pre-opus da espinha** — L1a `PISTA_LATERAL=ESPINHA-TESTE bash bin/suite.sh --dir "$ESP" --motivo "pre-opus ESPINHA-TESTE"`, cópia commitada, sem PRONTO → mesma recusa de P1, zero SSH e zero runner. L1b mesmo comando, PRONTO da obra e do SHA → um job inteiro na pista, `Ran/OK/rc 0`, zero runner VM. L1c mesmo pedido, pista e VM com vereditos divergentes medidos no mesmo SHA → relatar os dois e suspender novos pedidos à pista até Ronald ver. L1d mesmo comando sem `PISTA_LATERAL` → recusa antes de SSH ou VM. `bin/suite.sh:215–263,316–325`, `app/docs/BACKLOG.md:2191`, M2–M4.
- **VM e focado** — L2 `bash bin/suite.sh --dir "$C" --motivo "pouso LATERAL-TESTE"`, cópia sem PRONTO → inteira pela VM. L3 `bash bin/suite.sh --dir "$C" --only "$MODULO"`, módulo próprio e sem PRONTO → recorte roda. L4 `bash bin/suite.sh --dir "$C" --motivo "pre-opus"`, inteira → `rc 2` anterior. Zero SSH em L2–L4. `bin/suite.sh:215–263,316–325`, `app/docs/RELATO.md:65`, M2.
- **Régua e Git** — R1 `bash bin/regua.sh --force`, mesmo com carimbo verde e trava livre → runner integral executa, sem `--motivo`. G1 `git push remoto-teste HEAD:refs/heads/smoke`, hook instalado, commit novo e sem carimbo compatível → arsenal CI e `tenants` executam sobre o commit, push liberado se verdes. G2 mesmo push em fixture com carimbo compatível → reaproveita arsenal e executa `tenants`. Zero SSH. `bin/regua.sh:84–101,134–169`, `bin/hooks.sh:58–65`, `bin/pre-push.sh:27–74,159–200`, M2.
- **Aval** — A1 `U` com `alvos=wt-a`, sem `copia` → devolve com H. A2 `U` com `alvos=wt-a copia=wt-a@H0`, HEAD H, sem leitura registrada de H0 → devolve com H. A3 `U` com `alvos=wt-a,wt-b copia=wt-a@H12 copia=wt-b@J`, ambas da obra com leituras recentes → entra. A4 `U` com só `wt-a`, estando `wt-b` declarada para O → devolve; com `fora=wt-b` explícito → entra para C. A5 `U` com `alvos=wt-nova copia=nova`, nome livre → entra; mesmo envio com nome já declarado sem worktree → devolve. A6 `U` com `alvos=wt-sem copia=wt-sem@H12`, declaração sem worktree → devolve com HEAD indisponível. A7 `U` com C válida e `wt-nova` livre → entra com um token por alvo. Nos negativos, uma linha de devolução por evento e o próximo item andável segue. `bin/raias_estado.py:73–135`, `CLAUDE.md:262–275`, M5–M8.
- **Corte e outras entradas** — A8 `U` inválido recepcionado antes do marco, continuar o item após ativação → continuidade; recolagem do mesmo texto → recusa do novo evento. A9 colar `aval Ronald: LEITURA-TESTE` com `CONSTRUIR` só no corpo → T2 deixa passar. A10 enviar leitura, sonda e despacho pelos seus comandos vigentes de `bin/codex_pergunta.sh`, com entradas válidas e #G ativa → chegam às guardas atuais, zero recusa T1/T2. `CLAUDE.md:344–358,436–438`, `bin/codex_pergunta.sh:241–259`, M7–M8.
- **Volta em curso** — A11 `U` com H0 testemunhado há 10 min, HEAD H dois commits adiante, ambos da volta autorizada → entra com a colagem original e prova do delta. A12 mesma entrada com um commit de fora da volta, inclusive como segundo pai de merge → devolve com H. A13 HEAD H0 e leitura de H0 há 61 min → devolve com H0. A14 HEAD avança entre entrada e despacho com mais um commit da própria volta → despacho repete a checagem e segue com a mesma colagem. `bin/codex_pergunta.sh:266–273`, `bin/raias_estado.py:89–102`, M6a–M8.
- **ESTADO** — E1 `python3 bin/gerar_estado.py "$RAIZ" "$OUT"` e `DEST_RELATO="$DEST_TESTE" bash bin/relato.sh` com fixture raspada, C e D declaradas, uma só suja e uma sem worktree → três `copia=<nome>@<HEAD12>` e uma `SEM WORKTREE`, no ESTADO gerado e publicado. E2 repetir após avançar só H → muda só o token de C entre as cópias. `bin/raias_estado.py:125–174`, `bin/gerar_estado.py:211–224,398–407`, `bin/relato.sh:23,37–38,114–147`, M5.

## SELO

- `bin/tests/test_travas_sem_memoria.sh`, no host, chamado pelo glob da régua antes do Django. Adaptar `test_suite_motivo.sh` para seleção efetiva, `test_raia_declarada_no_estado.sh` para HEAD/publicação e usar o molde de fixtures e mutantes de `test_disjuntor.sh`. SSH falso registra argv e enfileiramentos, Git real temporário dá os HEADs e runners de teste registram chegada. `CLAUDE.md:127–130`, `bin/regua.sh:134–145`, `bin/tests/test_suite_motivo.sh:14–42`, `bin/tests/test_raia_declarada_no_estado.sh:43–84`, `bin/tests/test_disjuntor.sh:82–101,1532–1547`.
- Despachar payloads pelos registros lidos do settings candidato, por evento e matcher. Controle íntegro verde. Retirar só o registro T1 derruba P1 no teste do evento; retirar só o T2 derruba A2. Aceitar PRONTO de qualquer SHA derruba P2, ignorar segunda cópia derruba A4 e omitir HEAD na publicação derruba E1. A chamada direta ao script complementa a prova do executor. Aceitar qualquer descendente derruba A12 e ignorar idade da leitura derruba A13. `.claude/settings.json:62–72`, `bin/tests/test_disjuntor.sh:82–88,1532–1547`, M3–M8.
- **Smoke pré-pouso.** Ronald abre em terminal avulso, sem tmux novo de trabalho, com `cd -- "$COPIA_SMOKE" && timeout --foreground --kill-after=5s 10m claude`. `$COPIA_SMOKE` é o caminho absoluto da raiz da cópia candidata. Settings locais e comandos apontam para essa cópia. SSH, runners e destino de publicação são da fixture. Provar carga real dos dois eventos, P1/P3 e A1/A2/A3/A8, recorte V1, publicação E1 e continuidade com 

[truncado em 20000 de 26763 caracteres]

## 20/11 01:57 · sessao 2ab758cbcfcd · E4C-F5-CONSTROI · construtor · origem=Code

### 1. Por que o filtro rodava duas vezes

O GET renderiza o modal uma vez. A fixture tem dois cartões distintos, o chamado pai da disputa e o chamado destino das perguntas. O pai recebe a disputa pelo mapa de chamados e o destino recebe **a mesma instância Python** pelo mapa de perguntas. Os dois mapas vêm dos mesmos objetos de `todas_disputas`. O template percorre os cartões e pede a contagem uma vez para cada botão. `chamados/views.py:927`, `chamados/views.py:956`, `core/tests/test_selo_performance.py:306`, `chamados/services/fio.py:222`, `chamados/services/fio.py:255`, `chamados/services/fio.py:280`, `chamados/services/fio.py:287`, `templates/chamados/partials/modal_fio.html:68`, `templates/chamados/partials/modal_fio.html:396`.

### 2. Quem produz e quem consome o conjunto

| Sítio | Uso |
|---|---|
| `chamados/juizes.py:1762` | Seleciona as respostas com texto da disputa, carrega as decisões em lote, julga as candidatas e devolve um queryset por PK. A lista de PKs é interna ao juiz. |
| `chamados/views.py:1415` | A view do botão testa `.exists()`, passa o queryset ao escritor e usa o número atualizado no retorno da ação. `chamados/views.py:1454`. |
| `chamados/services/acoes_disputa.py:92` | A ação `reabrir_disputa` do POST do fio testa `.exists()` e passa o queryset ao mesmo escritor. `chamados/services/fio.py:690`, `chamados/services/acoes_disputa.py:99`. |
| `chamados/models.py:2404` | `reabrir_por_colab` testa `.exists()` e passa o queryset ao escritor dentro da transação que incrementa o contador. As entradas HTML e REST delegam a esse método. `chamados/models.py:2414`, `chamados/views.py:1329`, `api/views.py:2712`. |
| `chamados/services/disputa_emissao.py:1617` | `reabrir_para_correcao` lê o queryset para guardar a foto das respostas na trilha e usa `descarimbar_em_lote` para escrever. Devolve a quantidade atualizada. `chamados/services/disputa_emissao.py:1633`, `chamados/services/disputa_emissao.py:1656`. |
| `chamados/templatetags/chamados_tags.py:208` | A tag usa `.count()`. Modal e `detalhe_local.html` usam esse número no botão e na confirmação. A lista `perguntas_disputa` do detalhe vem de `pergunta_viva`, em uma leitura separada. `templates/chamados/partials/modal_fio.html:396`, `templates/chamados/detalhe_local.html:21`, `chamados/services/acoes_chamado.py:345`. |

A reabertura administrativa de disputa fechada chama `reabrir_admin` e abre uma pergunta nova pela porta de conversa. `chamados/services/acoes_disputa.py:239`, `chamados/services/acoes_disputa.py:259`.

### 3. Universo carregado pelo fio

Para a disputa **aberta da fixture**, o fio já carrega todas as perguntas da disputa com `decisoes`, antes de separá-las por cartão. Para uma **fechada**, aplica `q_vivas()` no SQL e depois `pergunta_viva` em Python. Uma resposta com texto, validada e ainda sem carimbo de materialização, exemplifica a diferença. A validação a exclui da carga da fechada, enquanto `pergunta_sem_lastro_ou_viva` a admite pelo carimbo ausente. `chamados/services/fio.py:251`, `chamados/services/fio.py:260`, `chamados/services/fio.py:270`, `chamados/models.py:3636`, `chamados/juizes.py:1410`, `chamados/juizes.py:1503`, `chamados/juizes.py:1762`.

### Cura e arquivos tocados

- **`chamados/templatetags/chamados_tags.py:231`** guarda só o inteiro devolvido pela leitura existente, por PK da disputa, no `RenderContext`. A primeira leitura de cada disputa chama o juiz com o prefetch intacto. `chamados/templatetags/chamados_tags.py:222`, `chamados/juizes.py:1765`.
- **`templates/chamados/partials/modal_fio.html:396`** usa essa tag para o botão e a confirmação, tanto no GET quanto na resposta renderizada após a ação. `chamados/views.py:927`, `chamados/views.py:1393`.
- **`chamados/tests/test_e4c_f5_voltaC3_reabrir.py:89`** cobra uma chamada e dois botões com N=3. Os outros casos associam N=1 e N=2 aos respectivos cartões e verificam N=0 em um novo GET após o POST real. `chamados/tests/test_e4c_f5_voltaC3_reabrir.py:95`, `chamados/tests/test_e4c_f5_voltaC3_reabrir.py:103`.

Ficou de fora alimentar o juiz com a carga do fio, pois isso ampliaria seu contrato e exigiria tratar o universo parcial das fechadas. `chamados/services/fio.py:251`, `chamados/juizes.py:1762`.

**Selo esperado no segundo GET: 92 → 89 consultas [nao verificado].** A previsão subtrai os três SQLs da segunda leitura do mesmo conjunto no anexo `medida_f3.txt`. O teto segue em 90. `core/tests/test_selo_performance.py:143`, `core/tests/test_selo_performance.py:377`.

### Comandos para o Code

Os dois arquivos Python e as quatro mutações compilaram em memória. Banco e suíte ficaram para a certificação. A compilação do template ficou bloqueada pela ausência de `django.template` no interpretador local, e o Ruff estava indisponível. Execução do RED, dos mutantes e do selo [nao verificado]. `chamados/tests/test_e4c_f5_voltaC3_reabrir.py:20`, `templates/chamados/partials/modal_fio.html:396`.

```bash
C3=chamados.tests.test_e4c_f5_voltaC3_reabrir.ReabrirNoModalTest
rodar() { PYTHONDONTWRITEBYTECODE=1 python3 manage.py test --settings=config.settings.ci --noinput "$@"; }

rodar "$C3" \
  core.tests.test_selo_performance.SeloPerformanceTest.test_chamados_modal \
  chamados.tests.test_e4c_f5_voltaB_lotes.CarregadoresVoltaBTest.test_RED_k29_respostas_corrigiveis \
  chamados.tests.test_ui_resposta_diz_o_que_e.AsDuasCascasDeCartaoTest

F5_VOLTA_C3_MUTANTE=sem_memo rodar "$C3.test_RED_dois_cartoes_consultam_o_juiz_uma_vez"
F5_VOLTA_C3_MUTANTE=chave_unica rodar "$C3.test_MORDE_cada_disputa_mostra_sua_contagem"
F5_VOLTA_C3_MUTANTE=entre_renders rodar "$C3.test_MORDE_novo_render_depois_de_reabrir"
F5_VOLTA_C3_MUTANTE=zero rodar "$C3.test_RED_dois_cartoes_consultam_o_juiz_uma_vez"

ruff check --no-cache chamados/templatetags/chamados_tags.py \
  chamados/tests/test_e4c_f5_voltaC3_reabrir.py
```

**LEI-AKITA** origem=`chamados/juizes.py:1762`, testemunha=`chamados/templatetags/chamados_tags.py:231` e `templates/chamados/partials/modal_fio.html:396`, RED=`chamados/tests/test_e4c_f5_voltaC3_reabrir.py:89`, quem-mais-le=`chamados/views.py:1415`, `chamados/services/acoes_disputa.py:99`, `chamados/models.py:2414` e `templates/chamados/detalhe_local.html:21`, juizes novos=0, padrao=Identity Map (`docs/GLOSSARIO.md:30`).

## 20/11 01:40 · sessao fab078a7cd14 · E4C-F5-CONSTROI · construtor · origem=Code

**C2 entregue para E1, E2, F1 e F2.** O censo estático bate com o registro atualizado, e a âncora de F1 rejeita o mutante que deixa a chamada apenas em comentário. Banco e testes ficaram para o Code. [Registro F2](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/core/tests/test_medida_turno_do_par.py:271) · [Âncora F1](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_contract_dedup_status.py:142) [nao verificado]

### Arquivos tocados e usos dos avais

A última coluna conta instruções `assert` e chamadas `self.assert*`, nessa ordem, por AST contra `9d386d1b1`.

| Arquivo | Classe | Antes → depois | Asserções |
|---|---|---|---|
| [chamados/tests/e3_v8_apoio.py:77](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/e3_v8_apoio.py:77) | ii, E1 | O módulo falso de `leitores()` passa a exportar `DecisaoPergunta` do helper único `chamados.tests.duble_pergunta`, já usado em B3 por `e3_volta7_apoio.py:111`. | `0/0 → 0/0` |
| [chamados/tests/e3_volta7_apoio.py:33](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/e3_volta7_apoio.py:33) | v, E2 | `Query` ganha `prefetch_related` como identidade. A subclasse V8 passa a herdar a mesma implementação em `e3_v8_apoio.py:26`. | `0/0 → 0/0` |
| [chamados/tests/test_contract_dedup_status.py:142](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_contract_dedup_status.py:142) | i, F1 | A âncora textual da chamada sem argumentos passa a exigir a atribuição `_pend = self.respostas_sem_veredito(perguntas=perguntas)` no AST do corpo de `fechar`. | `0/12 → 0/12` |
| [core/tests/test_medida_turno_do_par.py:271](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/core/tests/test_medida_turno_do_par.py:271) | i, F2 | `434 → 437`, mesma linha que filtra a célula por `loc.date()`. `chamados/services/materializacao.py:437`. | `0/86 → 0/86` |
| [core/tests/test_medida_turno_do_par.py:271](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/core/tests/test_medida_turno_do_par.py:271) | i, F2 | `455 → 458`, mesma linha que calcula `(loc.date() - cel.data).days`. `chamados/services/materializacao.py:458`. | Mesmo módulo |

As duas ocorrências de F2 têm conteúdo idêntico ao de `d9319a215`. São as únicas diferenças encontradas pelo censo de linhas congeladas em `core/tests/test_medida_turno_do_par.py:558`.

### F3 · atribuição e parada

A atribuição estática aponta **B, `d0210a400`**. Os dois `SELECT` candidatos são sobre `chamados_decisaopergunta`, ambos no `prefetch_related('decisoes')` de `perguntas_corrigiveis`. A fixture abre o modal de um colaborador com dois cartões, pai e destino, ligados à mesma disputa. Cada cartão chama o filtro de Reabrir, que monta um queryset novo para a mesma disputa. Isso dá um `SELECT` de decisões por cartão. `core/tests/test_selo_performance.py:298`, `:377` · `chamados/services/fio.py:275`, `:287` · `templates/chamados/partials/modal_fio.html:396` · `chamados/templatetags/chamados_tags.py:222` · `chamados/juizes.py:1765`.

Os leitores que C acrescentou ao estado de aplicação do cartão recebem as instâncias do lote de decisões do fio. D alterou `fechar`, enquanto o GET medido renderiza o modal. `chamados/services/materializacao.py:193`, `:889`, `:927` · `chamados/services/fio.py:260`, `:378` · `chamados/models.py:2071` · `chamados/views.py:951`.

**PAREI em F3.** O candidato é um prefetch em lote repetido pelo render. A condição do aval era uma leitura avulsa a incorporar ao lote. O anexo informa **92 / teto 90**. A sonda abaixo deve isolar o delta esperado **92 → 90** ao restaurar em memória somente o corpo anterior a B. SQL efetivamente emitido e total da sonda **[nao verificado]**. `chamados/juizes.py:1765` · `core/tests/test_selo_performance.py:143`, `:333`.

### Varredura de C e D

| Família | Resultado da inspeção |
|---|---|
| Vigia | Os dois dublês faltantes estão em E1/E2. Os leitores de vivas em fechadas e carimbos por dono têm cobertura ORM em `chamados/tests/test_e4c_f5_voltaC_lotes.py:154`, `:167`. |
| Materialização | O contador já usa `_Recorte(Linhas)`, com prefetch identidade. A elegibilidade e a anuência já recebem o compositor do helper. A fixture do estado de aplicação já fornece o que o compositor real lê. `chamados/tests/test_bug_par_c2_sem_banco.py:32`, `:78` · `chamados/tests/test_e4b_classe_e_memoria.py:193` · `chamados/tests/test_bug_par_sem_banco.py:182` · `chamados/tests/test_estado_aplicacao.py:38`. |
| Auditoria, dedup e universo do fato | O dublê do piso I6 substitui `_i6_pares` inteiro. Os corpos alterados têm cobertura ORM nos selos de C. O harness sem banco do backfill de decisões chama o contador de validação sem linha. `chamados/tests/test_selo_lapide_legado.py:85` · `chamados/tests/test_e4c_f5_voltaC_lotes.py:89`, `:119`, `:141`, `:230` · `chamados/tests/test_e4c_f5_voltaC_sitios.py:65` · `chamados/tests/test_e4c_f6f7_c7_sem_banco.py:50`. |
| Backfill do fio, disparo, estoque e retroativo | `Linhas` já fornece `filter`, `select_related` e `prefetch_related` para o backfill. A matriz do disparo já fornece `prefetch_related` pelo `Mock`. Estoque e emissão retroativa exercitam os caminhos alterados por ORM. `chamados/tests/test_e4b_classe_e_memoria.py:165`, `:650` · `chamados/tests/test_e4a2_matriz_memoria.py:124` · `chamados/tests/test_f1nova_b2_leitores.py:249` · `chamados/tests/test_e4c_f5_voltaC_lotes.py:198`. |
| Arquivamento, becos e porta D | Os caminhos de execução encontrados usam ORM. O consumidor estrutural de D com âncora deslocada foi F1. `chamados/tests/test_c7_red_minimos.py:283` · `chamados/tests/test_e4c_f5_voltaC_lotes.py:218` · `chamados/tests/test_e4c_f5_opus_models.py:149` · `chamados/tests/test_contract_dedup_status.py:135`. |

### Comandos para o Code

Os quatro labels cobrem E1, E2, F1 e F2. A segunda chamada inclui os dois selos do modal, com `test_selo_teto_modal` em `chamados.tests`. Execução **[nao verificado]**. `chamados/tests/test_e3_portas_tarefa.py:505` · `chamados/tests/test_e3_degrau_dono_e_porta.py:388` · `chamados/tests/test_contract_dedup_status.py:135` · `core/tests/test_medida_turno_do_par.py:558` · `core/tests/test_selo_performance.py:377` · `chamados/tests/test_selo_teto_modal.py:37`.

```bash
e1=chamados.tests.test_e3_portas_tarefa.MatrizV8Test
e2=chamados.tests.test_e3_degrau_dono_e_porta.LeitoresPortaApp
f1=chamados.tests.test_contract_dedup_status.ContratoDedupDistingueStatusTest.test_porta_humana_de_disputa_presta_contas
f2=core.tests.test_medida_turno_do_par.CatracaTurnoDoParTest.test_CATRACA_linhas_reais_congeladas_exigem_registro_no_mesmo_commit
f3=core.tests.test_selo_performance.SeloPerformanceTest.test_chamados_modal

PYTHONDONTWRITEBYTECODE=1 python -B manage.py test --settings=config.settings.ci "$e1" "$e2" "$f1" "$f2"
PYTHONDONTWRITEBYTECODE=1 python -B manage.py test --settings=config.settings.ci "$f3" chamados.tests.test_selo_teto_modal
```

Os mutantes abaixo são em memória. E1 deve reproduzir o `ImportError`, E2 o `AttributeError`, F1 deve falhar com a chamada só em comentário, e F2 deve falhar ao restaurar cada coordenada antiga isoladamente. Resultados dos mutantes **[nao verificado]**. `chamados/services/vigia_fio.py:504`, `:571` · `chamados/tests/test_contract_dedup_status.py:142` · `core/tests/test_medida_turno_do_par.py:558`.

```bash
c2() {
  PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci \
    python -B - "$@" <<'PY'
import inspect
import subprocess
import sys
import traceback
from contextlib import ExitStack
from types import SimpleNamespace
from unittest.mock import patch

import django
django.setup()

from django.conf import settings
from django.test.utils import get_runner

modo, label = sys.argv[1:]
with ExitStack() as escopo:
    if modo == 'e1':
        from chamados.tests import e3_v8_apoio as apoio
        modulo = apoio.modulo

        def sem_compositor(nome, **campos):
            if nome == 'chamados.models' and 'ChamadoColaborador' in campos:
                campos.pop('DecisaoPergunta')
            return modulo(nome, **campos)

        escopo.enter_context(patch.object(apoio, 'modulo', sem_compositor))
    elif modo == 'e2':
        from chamados.tests.e3_volta7_apoio import Query
        escopo.callback(setattr, Query, 'prefetch_related', Query.prefetch_related)
        del Query.prefetch_related
    elif modo == 'f1':
        from chamados.models import DisputaSupervisao
        from chamados.tests import test_contract_dedup_status as contrato

        def fonte_com_comentario(fn):
            fonte = inspect.getsource(fn)
            if fn is DisputaSupervisao.fechar:
                chamada = '_pend = self.respostas_sem_veredito(perguntas=perguntas)'
                return fonte.replace(chamada, '# ' + chamada + '\n        _pend = []', 1)
            return fonte

        escopo.enter_context(patch.object(
            contrato, 'inspect', SimpleNamespace(getsource=fonte_com_comentario)))
    elif modo in ('f2-437', 'f2-458'):
        from core.tests import test_medida_turno_do_par as catraca
        chave = ('chamados/services/materializacao.py::_plantio_preenche_furo_na_data', 'B1')
        linhas = [434, 458] if modo == 'f2-437' else [437, 455]
        escopo.enter_context(patch.dict(
            catraca.LINHAS_REGISTRADAS['derivacoes_de_dia'], {chave: linhas}))
    elif modo in ('f3', 'f3-sem-b'):
        from chamados import juizes
        from chamados.tests.e4_apoio import funcao
        from core.tests.test_selo_performance import SeloPerformanceTest
        from django.db import connection

        if modo == 'f3-sem-b':
            fonte = subprocess.check_output(
                ['git', 'show', 'd0210a400^:app/chamados/juizes.py'], text=True)
            anterior = funcao('chamados/juizes.py', 'perguntas_corrigiveis',
                              vars(juizes).copy(), fonte=fonte)
            escopo.enter_context(patch.object(juizes, 'perguntas_corrigiveis', anterior))

        medir = SeloPerformanceTest._medir_rota

        def medir_com_sql(self, nome, url):
            consultas = []

            def registrar(execute, sql, params, many, context):
                pilha = ()
                if sql.lstrip().upper().startswith('SELECT') and 'chamados_decisaopergunta' in sql:
                    pilha = tuple(f for f in traceback.extract_stack() if 'chamados/' in f.filename)
                consultas.append((sql, params, pilha))
                return execute(sql, params, many, context)

            with connection.execute_wrapper(registrar):
                total, ms = medir(self, nome, url)
            print(f'{nome} · {total} consultas')
            for sql, params, pilha in consultas[-total:]:
                if pilha:
                    print(sql, params)
                    print(' <- '.join(f'{f.filename}:{f.lineno}' for f in reversed(pilha[-5:])))
            return total, ms

        escopo.enter_context(patch.object(SeloPerformanceTest, '_medir_rota', medir_com_sql))
    else:
        raise SystemExit(f'Modo desconhecido: {modo}')

    raise SystemExit(bool(get_runner(settings)(verbosity=2, parallel=1).run_tests([label])))
PY
}

c2 e1 "$e1.test_v8_R1_colab_alcancavel_e_leitores"
c2 e2 "$e2.test_vigia_conta_app_sem_KeyError_e_publica_no_log"
c2 f1 "$f1"
c2 f2-437 "$f2"
c2 f2-458 "$f2"
c2 f3 "$f3"
c2 f3-sem-b "$f3"
```

**LEI-AKITA** — origem em `vigia_fio.py:504`, `:571`, `models.py:2072` e `materializacao.py:437`, `:458` · testemunhas no dublê único `duble_pergunta.py:12`, na chamada viva de `fechar` e no censo de `core/tests/test_medida_turno_do_par.py:558` · RED e mutantes entregues para o Code **[nao verificado]** · F3 parado pelo escopo literal `docs/LEIS.md:62` · juízes novos **0**, conforme os quatro arquivos da tabela.

## 20/11 01:35 · sessao fbf1984f8eb2 · CADEIA-20-PASSO-1 · leitura · origem=Code

aval Ronald: CADEIA-20-IMPRESSAO-V2 (rascunho Codex 20/11) -- unidade: célula-dia, impressão da ata; lei: L-124, desenho célula soberana

## FONTE

- **F1 — Base e alcance.** Volta 3 da CADEIA-20, cópia `wt-codex-cadeia-20-passo-1-112113`, hash informado `a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6`. Caminho da cópia em `app/docs/RELATO.md:100`. A árvore local tem o nome do hash e veio sem metadados Git. Identidade do HEAD [nao verificado]. L-124 conserva a foto anterior ao horizonte até fato novo. `app/docs/LEIS.md:145`
- **F2 — Impressão atual.** SHA-256 de batidas (`pk`, tipo, instante), status da ausência que cobre o dia, chamados de batida (`pk`, `status_local`), DNA (`dna_versao`, marcos) e condicionais `veto`, `teto`, `regra`, `enc`, `fio`, `cobparcial`, `isento`, `sem_adesao`. A parcial pode carregar período e índices dos marcos dispensados. A porta da célula grava `impressao` e os campos da lavra. `app/ponto/services/cartorio.py:477–490,493–578,1496–1501` `app/ponto/portas/celula.py:1189–1201`
- **F3 — Portas humanas de `status_local`.** Detalhe do chamado e resposta rápida do DP no fio aceitam `novo_status`. Aprovar exceção, responder agenda ou escala e autorizar ou recusar geofence também chamam a transição. A decisão de benefício chama a mesma porta. O cartório só carrega os quatro `FURO_MODULES` declarados e agrupa os chamados pelo dia lido do catálogo. `app/chamados/urls.py:14,21` `app/chamados/views.py:546–547` `app/chamados/services/acoes_chamado.py:70–77,109–122,154–167,228–241,285–287,322–329` `app/chamados/services/fio.py:756–772` `app/colaboradores/views.py:2107–2113` `app/chamados/catalogo/furo_modulos.py:15–16` `app/ponto/services/cartorio.py:141–157`
- **F4 — Portas humanas que chegam a gates compartilhados.** Reabrir disputa administrativa, desarquivar pela tranca e reabrir competência podem chamar `reabrir(humano=True)`. Abrir o registro de ausência para validar chama `analisar()`. Resolver dia e fechar disputa podem levar a `reconciliar()` no chamado. A confirmação da regularização externa pode levar a `encerrar()`, inclusive por retomada automática. `app/chamados/services/acoes_disputa.py:259–278,301–302` `app/chamados/views_arquivar.py:63–86` `app/chamados/services/competencia_trancada.py:260–296` `app/chamados/views.py:1489–1515` `app/ponto/services/veredito_celula.py:184–207` `app/chamados/signals.py:21–46,122–139` `app/chamados/services/regularizacao_ext.py:199–206,348–356`
- **F5 — Identidade e instante no chamado.** A transição direta passa `user` e via `admin` a `_gravar_estado`. A trilha `estado_chamado` guarda `LogAuditoria.usuario_id`, `criado_em`, objeto, antes/depois e via. `respondido_por_id` guarda o usuário quando o gate o passa. `resolvido_em` marca a saída de vivo, pode ser preservado em `resolvido → fechado` e é limpo na volta a vivo. `atualizado_em` também muda em outras escritas. As portas de reabertura citadas em F4 têm trilhas próprias, mas `reabrir(humano=True)` chega ao escritor sem passar autor ou origem do ato. A auditoria é best-effort e pode atribuir o usuário do request a um efeito automático dentro dele. **Não há um par persistido no chamado que identifique universalmente a decisão humana de origem.** `app/chamados/catalogo/motor.py:88–113` `app/chamados/models.py:58–74,234–255,298–302,459–462,509–511,859–866,1421–1425,1498–1501` `app/core/models.py:295–307` `app/core/observ.py:94–138`
- **F6 — O que alimenta `fio`.** Conta chamados de batida do dia em `VIVOS` com pelo menos uma `perguntas_que_resolvem` viva pelo C6. `fio:0` significa que há chamado vivo sem fio vivo, e a parte fica ausente quando não há chamado vivo. C6 lê validação, resposta e texto, `materializada_em` e `via_resolucao`, além de tranca, veredito/via da célula, lâmpada e lastro do marco. O fechamento adiado e o E2 mudam esses insumos. A fábrica pode criar perguntas ou desfazer carimbos após a lavra. O juiz do dia recebe vivos e fio, e a releitura interna após emissão usa a impressão para decidir se volta a julgar. `app/ponto/services/cartorio.py:543–547,1123–1141,1165–1184,1193–1223,1527–1545,1614–1618` `app/chamados/juizes.py:1382–1489,1510–1519` `app/chamados/reconciliador.py:634–645` `app/chamados/services/fabrica_da_celula.py:160–174,206–219`
- **F7 — Atos humanos no fio.** Resposta do colab pela web/API e correção de resposta pelo admin passam pelo escritor que grava `DecisaoPergunta` com `ato_por_id` e `ato_em`. Validação, devolução e recusa também têm trilha de decisão. Resolver dia e encerrar silêncio da disputa gravam `SILENCIO` com autor e instante do carimbo. A projeção `materializada_por_id`/`materializada_em`/`via_resolucao` é compartilhada. Na reabertura de questionário pela supervisão, a linha por pergunta guarda o instante com `ato_por` nulo, e o autor fica na auditoria da disputa. **Falta nessa porta a assinatura por pergunta.** `app/chamados/views.py:1276–1278` `app/api/views.py:2646–2647` `app/chamados/services/acoes_disputa.py:92–117,374–379` `app/chamados/services/disputa_emissao.py:1527–1534,1584,1617–1645` `app/chamados/services/validacao.py:161–175,237–280,915–939` `app/chamados/models.py:2080–2113,3246–3260,3293–3297,3378–3386,4166–4169` `app/ponto/services/veredito_celula.py:189–197,346–358`
- **F8 — Horizonte e relógio.** `versao_para` usa mês/ano do fim de `janela_atual(data, empresa)`, pelo corte da empresa, e devolve a maior versão com horizonte `<=` à competência, ou `None` se nenhuma alcançar. A tabela tem v1 em `2026-09` e v2–v5 em `2026-10`. `_regra_do_dia` alimenta os dois ramos de impressão. A v5 também é lida pelo contador e pelo leitor de razão ausente. O comando captura `agora`, repassa ao cartório, e C6 já aceita esse instante. `app/ponto/janelas.py:36–63` `app/ponto/services/cartorio.py:161–166,280–299,521–522,1038–1056,1110–1123,1329–1337,1399–1401,1541–1547,2202–2205` `app/escala/services/leitor_celula.py:194–222` `app/ponto/management/commands/processar_cartorio.py:40–41,63–69` `app/chamados/juizes.py:866–898`
- **F9 — Entrega e dinheiro.** O juiz registrado é `_K6`, `app/folha/export.py::competencia_entregue`, por empresa × mês × ano. A resposta é TXT vigente e não invalidado. A grade da ata alimenta previsto, abonado, realizado, retenção e `DiaPago`. DINHEIRO item 1 exige reversão anterior, exportada intacta e prova da frota no mesmo marco quando há apply admitido. `app/docs/JUIZES.md:40–42` `app/core/juizes.py:334–342` `app/folha/export.py:329–338,556–590` `app/ponto/services/fechamento.py:1046–1068,1181–1202` `CLAUDE.md:401–417`
- **F10 — Medidas fornecidas.** `insumo.out` registra oito diferenças só de impressão no P2, enumeradas no RED. `tres.out` registra três no P2 (128355, 129943, 138359) e uma no P3 (129134, `concorde → cobrado`, chamado 43141). (A) é zero no P2/P3 do dump novo. No P1, 12711, competência 08, mudou `razoes_orfas` de `null` para `[]` e manteve `furo`. Os anexos mostram diferenças nos insumos. Autor, causa por célula, relógio exato injetado e filtro/colunas dos hashes impressos [nao verificado].

## MUDA

1. **Ato humano.** Antes do código, publicar a matriz insumo × porta escritora × leitor. No horizonte, substituir o `status_local` derivado e a contagem `fio` da impressão pela identidade e pelo instante dos atos humanos de origem que afetam aquele dia. Retirar da impressão os insumos cujo único escritor é a cadeia. Conservar o gatilho de releitura quando o juiz precisa ler efeito novo da própria emissão. Onde a trilha não identifica o ato humano, vale o **PAREI de 1b**. Se a costura exigir lei ou juiz novo, PAREI no ponto e levo a pergunta ao Ronald. `app/ponto/services/cartorio.py:493–547,1193–1223,1527–1558,1614–1618` `app/chamados/models.py:298–302,4166–4169`
2. **Relógio.** Passar o `agora` do cartório a `fio_vivo` e deste a `pergunta_viva`. Preservar a chamada existente dos outros leitores quando omitem o argumento. `app/ponto/services/cartorio.py:1110–1123,1532,2202–2205` `app/chamados/juizes.py:866–898,1382–1404,1510–1519`
3. **Vigência.** Usar `versao_para` e o horizonte default **2026-10** da volta 3. Até 2026-09, conservar byte a byte a impressão anterior para os mesmos insumos e a foto gravada sem fato novo. Em fato novo, vale o caminho anterior à mudança. Manter a leitura e a lavra da razão da sobra pela v5. `app/docs/LEIS.md:145` `app/ponto/services/cartorio.py:161–166,280–299,1038–1056,1333–1337,1552–1558` `app/escala/services/leitor_celula.py:194–222`
4. **Entregue.** Consultar `_K6` pela competência do dia antes de qualquer julgamento ou escrita desta obra, inclusive efeitos da releitura e da fábrica. Preservar a janela de leitura dos turnos que atravessam a borda. Provar zero escrita em célula entregue, além do hash de FM e ED. `app/folha/export.py:556–590` `app/ponto/janelas.py:60–63` `app/ponto/services/cartorio.py:1075–1079,1152–1181,1199–1222` `app/ponto/portas/celula.py:1189–1201`
5. **FM/ED, literal proposto.** Empresas **2, 3, 4** × competências **2026-08, 2026-09, 2026-10, 2026-11**. Agosto e novembro aparecem nas células da sonda. Setembro e outubro aparecem na declaração de versões e nos anexos 1815/2325. O filtro dos hashes antigos [nao verificado]. Fotografar imediatamente antes de P1 e após P3, no mesmo ensaio, antes de recálculo financeiro. FM por empresa/ano/mês/colab/PK e ED por empresa/ano/mês/PK, com **todas as colunas persistidas**, inclusive carimbos. Em ED incluir conteúdo, `hash_sha256`, `invalidada_em`, `invalidada_motivo` e `substituida_por_id`, com todas as versões. Ordenar pelas chaves, usar a mesma serialização canônica e publicar contagem e SHA-256 por modelo × empresa × competência, inclusive recortes vazios. `app/ponto/models.py:398–485` `app/folha/models.py:75–104,143–186`
6. **Aberta.** Antes do pouso, medir main × cópia a partir do mesmo dump e relógio, por empresa × colab × competência aberta alcançada. Usar o mesmo recálculo em ambos. Comparar presença da linha, rubricas, saldo, turnos, previsto/abonado/realizado, retenção (`motivos_espelho` e classificação de exportação) e linhas `DiaPago` por data/tipo/versão, inclusive ajuste. Movimento = `PAREI` com empresa, colab, competência, campo, antes/depois e dias afetados. Em 10, manter a Pauta DP de 2325 para 191, 203, 922 e 87 e o acerto a cargo do DP, conforme anexo 2325 [nao verificado na árvore]. `CLAUDE.md:401–417` `app/ponto/models.py:315–351,412–468` `app/ponto/services/fechamento.py:1046–1068,1191–1202` `app/folha/export.py:765,1301`
7. **(A) `null → []`.** Registrar as 19 do levantamento anterior e o zero observado no P2/P3 do dump novo. Sem varredura de normalização. Manter 12711/08 como testemunha do P1 e informar sua resposta de `_K6` no ensaio. Identidade das 19 e resposta de `_K6` para 12711 [nao verificado]. `app/ponto/services/cartorio.py:962–971` `app/docs/LEIS.md:145`

## RED

- **R1 — Oito células.** `128355`, `129476`, `129490`, `129943`, `138359`, `138541`, `142995`, `143945`, todas no recorte informado de 11/2026. Entradas observadas incluem `em_analise → resolvido` com `fio:1` sumindo (128355/129943/138359), só `fio:1 → 2` (129476) e `aberto → em_analise` com `fio:1 → 2` (129490). Detalhe das outras entradas [nao verificado]. Sem ato humano nem fato externo novo, ler `CelulaDia.impressao` e `veredito` por PK após P1, P2 e P3. Esperado **8/8 estáveis em P1=P2 e P2=P3**. As diferenças da base são as do anexo `insumo.out` [nao verificado por reexecução]. `app/ponto/services/cartorio.py:1527–1558` `app/ponto/portas/celula.py:1194–1201`
- **R2 — 129134, col474, 19/11, competência 11.** Entrada do anexo `tres.out` é `enc:0`, sem chamado, depois `fio:1` e chamado 43141 com `concorde → cobrado`. Causa exata [nao verificado]. Rodar três passes com o mesmo instante em cada braço, um em curso e outro já encerrado pelo juiz do turno. Em curso, a regra conserva `concorde` sem cobrança prematura. Encerrado, com cobrança devida e fio vivo, a primeira rodada completa deve sair `cobrado`, com uma só cobrança. Esperado **impressão e veredito iguais em P1=P2=P3** em cada braço. Atravessar o encerramento num controle separado deve permitir o rejulgamento exigido pelo juiz vigente. `app/ponto/services/cartorio.py:523–532,1193–1223,1293–1297,1540–1545,1614–1624,1975–1993` `app/ponto/turnos.py:2124–2134` `app/ponto/supra_juiz.py:317–329,450–463`
- **R3 — Humano entre passes.** Fixture H1, célula passada carimbada no horizonte, chamado de batida `em_analise`, dia com premissa regularizada e sem pergunta viva que barre o fecho. Após P1, gestor autenticado faz POST na porta real `detalhe_local`, com `novo_status=resolvido`. Ler antes do POST, ao retornar e após P2. Esperado **ato assinado, impressão nova e rejulgamento no ato**, podendo P2 já encontrar o carimbo em dia. Controle H2 produz a mesma transição por `reconciliar()` sem ato humano, e o trecho da impressão reservado à origem humana fica igual. Para o fio, fixture H3 valida pela porta humana uma pergunta viva entre passes e mede a assinatura e o rejulgamento. `app/chamados/urls.py:14` `app/chamados/services/acoes_chamado.py:292–297,322–331` `app/chamados/models.py:179–205,298–302,654–668` `app/chamados/services/validacao.py:161–175` `app/chamados/signals.py:189–218`
- **R4 — Horizonte e entregue.** Fixtures `H09` e `H10` abertas e já carimbadas, com a mesma mudança exclusivamente derivada após P1. H09 mantém impressão, ata, veredito e `julgada_em` da base na ausência de fato novo. H10 usa a regra nova e passa R1. Fixture `E10` com TXT vigente e insumo suficiente para relavrar fica intacta em P1–P3 e tem **zero chamadas às portas de escrita**. Controle `I10`, mesma competência com último TXT invalidado sem substituta, entra no caminho de aberta. Para 12711, se `_K6` responder entregue, o esperado é zero toque inclusive em P1. `app/docs/LEIS.md:145` `app/folha/export.py:556–590` `app/escala/tests/test_cadeia20_volta3.py:142–158` `app/ponto/portas/celula.py:1189–1201`
- **R5 — Relógio injetado.** Fixture T1 com pergunta viva às 10:00 e validação datada às 10:01. Fixar relógio ambiente às 10:02. `fio_vivo` deve responder vivo com `agora=10:00` e morto com `agora=10:02`. As duas consultas usam a mesma pergunta. `app/chamados/juizes.py:886–898,1382–1406,1510–1519`

## SELO

- Django na pista da cópia e prova de frota na sombra. Para R1/R2, três rodadas de `processar_cartorio --apply` sem `--forcar` externo, empresas 2, 3, 4 nessa ordem em cada rodada. Usar o mesmo dump por braço, relógio local congelado em `2026-11-20T06:27:00-03:00` como literal proposto para o braço encerrado, e `2026-11-20T00:30:00-03:00` para o controle em curso de 129134. Confirmar os dois lados pelo juiz do turno. Tirar cada foto depois de E2, fechamento adiado, fábrica e releitura internos. Os instantes originais exatos das sondas [nao verificado]. `CLAUDE.md:84–102` `app/config/crons.py:616` `app/ponto/management/commands/processar_cartorio.py:27–69` `app/ponto/services/cartorio.py:1123–1223`
- Adaptar `test_cadeia20_volta3.py` para a foto anterior ao horizonte e para a invariância da lavra. Usar os testes de fio vivo/morto como base dos casos humano e automático, e as fixtures `_K6` para entrega e revogação. Mutantes a matar: recolocar `status_local` ou `fio` derivado no hash, omitir o ato humano, aplicar o formato abaixo do horizonte, furar `_K6`, suprimir a releitura de cobrança devida ou ignorar o `agora` injetado. Cada mutante cai no RED correspondente. `app/escala/tests/test_cadeia20_volta3.py:109–158` `app/ponto/tests/test_o261_cobrado_le_o_fio.py:132–164,315–354` `app/folha/tests/test_k6_competencia_entregue.py:58–85`
- **R5 de frota da casa fica como está.** Empresas 2/3/4, 10/2026, cartório e recálculo em duas rodadas, compara foto pós-rodada 1 com foto pós-rodada 2. Sua foto omite `impressao` e `julgada_em`. Os RED acima são prova adicional. `app/core/placar_estrutural.py:416–431` `bin/r5_idempotencia_frota.py:26–32,52–57,104–130`

## PROIBIDO

- Carimbo no fim do passe, apagar ato humano, criar lei ou juiz, backfill ou normalização em massa, escrita em entregue e pouso. Impacto financeiro ou toque em exportada = `PAREI` com a lista. Qualquer exceção à exportada exige o `!` aplicável fora deste rascunho. `app/docs/LEIS.md:67,109,112,145,155` `CLAUDE.md:401–417` Ficha 1 [nao verificado na árvore].
- **Colisão 1815.** Preservar o encaminhamento da razão da sobra por horizonte, o estado nomeado no leitor da ata antiga, os selos dos leitores e a revisão do escopo inteiro. **Colisão 2325.** Preservar causa por célula, identificação do dia de 08, Pauta DP de 10 e verificador antes da linha de PRONTO e do opus. A permissão de pouso do 2325 tem o escopo daquele aval. `app/docs/CORTES.md:563` `app/escala/services/leitor_celula.py:194–222` Anexos 1815 e 2325 [nao verificado na árvore quanto ao cumprimento].

## PRONTO

1. Matriz publicada com porta, campo de identidade/instante e leitor por insumo. **1b resolvido pelo Ronald** antes do código. `app/chamados/models.py:234–255,298–302,4166–4169` `app/ponto/services/cartorio.py:493–578`
2. Oito células com **0 mudança de impressão e 0 de veredito** no P2 e no P3. 129134 com os mesmos zeros por braço a relógio fixo. P1 registra total de rejulgadas e duração. Causa por célula publicada. `app/ponto/services/cartorio.py:1552–1558` `app/ponto/management/commands/processar_cartorio.py:69–74`
3. Ato humano provoca rejulgamento verificado imediatamente após o POST. Controle automático preserva a assinatura humana. Selo do relógio e todos os mutantes mortos. `app/chamados/signals.py:189–218` `app/chamados/juizes.py:886–898,1382–1406`
4. Foto anterior ao horizonte byte a byte no controle sem fato novo. **0 escrita e 0 diferença** na foto integral das células entregues. Contagem e SHA-256 de FM e ED iguais em cada recorte de MUDA 5. `app/docs/LEIS.md:145` `app/ponto/portas/celula.py:1189–1201` `app/folha/models.py:75–104`
5. Dinheiro e retenção main × cópia com **0 colab × competência divergente**, incluindo `DiaPago`. Qualquer movimento produz a lista de MUDA 6 e mantém `PAREI`. `CLAUDE.md:401–417` `app/ponto/services/fechamento.py:1046–1068,1191–1202`
6. (A) contado no P2/P3 e 12711/08 explicado em separado. Contador de atas no horizonte com sobra e sem lavra completa = 0. R5 da casa = 0. Verificador responde item a item e a sessão publica a linha de PRONTO antes do pre-opus. `app/ponto/services/cartorio.py:1038–1056` `app/core/placar_estrutural.py:416–431` `CLAUDE.md:95–103` Anexos 1815 e 2325 [nao verificado quanto à execução].

LEI-AKITA: origem=versao_para e atos das portas humanas, testemunha=CelulaDia.impressao e veredito lidos após cada passe, RED=R1–R5 deste aval, quem-mais-le=classificar_dia e releitura do cartório, grade/espelho/PDF pela ata, juizes novos=0, padrao=celula soberana  
`app/docs/GLOSSARIO.md:11` `app/ponto/services/cartorio.py:280–299,1199–1222,1614–1618` `app/escala/services/leitor_celula.py:194–222` `app/ponto/services/espelho.py:709,862` `app/relatorios/pdf_espelho.py:870–875`

## CONTRADIÇÕES DA ÁRVORE COM A FICHA

- CONTRADIZ a ficha (a): app/ponto/services/cartorio.py:502–507,1541–1547. Hoje entram o estado e a contagem derivados. Nas portas sem assinatura individual, a árvore não fornece o par completo exigido pela ficha. `app/chamados/models.py:1498–1501` `app/chamados/services/acoes_disputa.py:105–117` `app/chamados/services/disputa_emissao.py:1639–1645`
- CONTRADIZ a ficha (c): app/ponto/management/commands/processar_cartorio.py:47–69. O lote atual inclui células passadas sem `_K6` nessa seleção, e a lavra pode persistir impressão e ata. Toque efetivo em entregue nas sondas [nao verificado]. `app/ponto/services/cartorio.py:2013–2037` `app/ponto/portas/celula.py:1189–1201`
- CONTRADIZ a ficha (f): app/ponto/services/cartorio.py:1532 e app/chamados/juizes.py:1510–1519. Hoje a chamada e o juiz omitem o `agora` do cartório.

## BLOQUEIOS RESPONDIDOS

- **1b** — **SEGUE ABERTO: Ronald, qual registro de origem vale para reaberturas e outros gates compartilhados quando a trilha por chamado/pergunta não traz autor e instante inequívocos, 

[truncado em 20000 de 21555 caracteres]

## 20/11 01:33 · sessao 9097ab63bc09 · TRAVAS-SEM-MEMORIA · leitura · origem=Code

aval Ronald: TRAVAS-SEM-MEMORIA-V3 (rascunho Codex 20/11) -- unidade: hook e ESTADO; lei: nenhuma nova

## FONTE

- F0. FICHA 2, alíneas (a)–(e), rege este rascunho. Base informada `eb1e49c1ec1055e6bf43d7f67df63b17054b474e`; o diretório entregue não tem `.git`, então a identidade `HEAD = main = hash` fica [nao verificado]. Foram conferidos os arquivos citados. Carga dos hooks na VM, executor da pista e resultados de smoke [nao verificado]. `fichas_para_rascunho_2011_0130.txt:3`, `CLAUDE.md:344–348`.
- F1. `suite.sh` resolve `--volta` e julga a seleção efetiva. Seleção vazia ou que cobre a descoberta toda é inteira. Inteira sem motivo canônico sai `rc 2`. A volta intermediária junta ruff, selo do disjuntor e recorte com vizinhos e selos globais. `bin/suite.sh:156–180,215–263`, `bin/volta_intermediaria.sh:18–46`.
- F2. Régua tem runner e carimbo próprios. O Git entrega argumentos e stdin ao wrapper do pre-push, que decide se reaproveita a régua e roda `tenants` em runner separado. O pouso usa `suite.sh` pela trava da VM. `bin/regua.sh:46–51,84–101,165–169`, `bin/hooks.sh:58–65`, `bin/pre-push.sh:6–8,27–74,159–200`, `CLAUDE.md:95–106`.
- F3. A opção A registra volta e pre-opus de lateral na pista, espinha e pouso na VM. A interface da pista recebe commit em `refs/pista/<sha>`, `pede <sha> [--only <labels>] [--parallel N]` e `espera <id>`. Há registros de `rc 0`, `rc 1` e `rc 71` sem a ref. `app/docs/RELATO.md:65,201,1497,1529`, `app/docs/BACKLOG.md:2191–2195`.
- F4. `PreToolUse` recebe o comando externo em `tool_input.command`. O disjuntor atual pode sair cedo para Bash da sessão principal. `UserPromptSubmit` está ausente do settings entregue. Stop lê `transcript_path` e cobra a fila pelas portas vigentes. A abertura em `app/` já deixou hooks da raiz inertes. `.claude/settings.json:2–73`, `bin/disjuntor.py:1837–1843,1864–1867,1912–1933`, `bin/hook_stop_fila1.py:1339–1345,1381–1404`, `app/docs/RELATO.md:3341`, `app/docs/PROMPTS.md:1877`.
- F5. `raias_estado.py` cruza worktrees com declarações do escritor `raia_agente.sh`. A união inclui declaradas, à frente e só sujas. Hoje as só sujas saem por nome, há declaração sem caminho e o HEAD por cópia não sai. `gerar_estado.py` lê o medidor e `relato.sh` gera, raspa e publica o ESTADO. `bin/raias_estado.py:42–135,138–174`, `bin/raia_agente.sh:40–59`, `bin/gerar_estado.py:211–229,329–331`, `bin/relato.sh:30–38,114–119,133–147`.
- F6. O construtor Codex escreve só em `<wt>/app`, com uma vaga por repo. `.claude/` consta no ignore e há precedente de instalação local negada pelo classificador. O arquivo do RELATO guarda marcos antigos. `CLAUDE.md:349–356,394,436–442`, `bin/codex_pergunta.sh:121–135,248–259`, `.gitignore:66`, `app/docs/RELATO.md:4356,4411`.

## MUDA

- M1. A sessão constrói em cópia e instala os dois registros locais da raiz, somente para este instrumento. Codex verificador lê o diff depois da construção. Antes do código, publicar as cinco linhas abaixo no RELATO e mandá-las uma vez ao Codex. A execução depende de `aval Ronald: APROVA RASCUNHO TRAVAS-SEM-MEMORIA-V3 <sha256 12>`. `fichas_para_rascunho_2011_0130.txt:1,3`, `aval_rascunhado_pelo_codex_1911_1838.txt:5–7`, `CLAUDE.md:309,349–356,372–394`.
- M2. T1 usa a rota declarada pela sessão no despacho. Para lateral, o pedido local carrega `PISTA_LATERAL=<OBRA>` junto de `bin/suite.sh --dir <cópia> --motivo "pre-opus <OBRA>"` ou `bin/volta_intermediaria.sh <cópia> <base> <saída>`. O script da volta conserva ruff e selo e passa o recorte efetivo ao ramo remoto de `suite.sh`. Volta cujo recorte cobre tudo continua sujeita ao `rc 2` da guarda atual. Régua, pre-push, pouso, pre-opus da espinha e teste focado mantêm seus comandos. `bin/suite.sh:129–180,215–263`, `bin/volta_intermediaria.sh:18–46`, `CLAUDE.md:95–106`, `app/docs/RELATO.md:65,81`.
- M3. Registrar um `PreToolUse` de Bash para a porta canônica. Ele lê `PISTA_LATERAL`, a obra do motivo e o SHA completo da cópia. Requer o mesmo ID nos dois campos e, para a inteira pre-opus, uma linha literal no início da linha `PRONTO <OBRA> item a item: sha=<SHA completo>` em `app/docs/RELATO.md` ou `app/docs/RELATO-ARQUIVO.md`. Um registro arquivado vale para o mesmo SHA. Cabeçalho de outro SHA, de outra obra, indentado ou citado vale como ausente. Resposta `SEM PRONTO ITEM A ITEM`. O mesmo verificador de instrumento roda no executor imediatamente antes de criar a ref e pedir o job. O SSH recebe o SHA conferido e as opções documentadas. `bin/disjuntor.py:1796–1798,1837–1843`, `bin/suite.sh:219–263`, `CLAUDE.md:439–442`, `app/docs/BACKLOG.md:2191`.
- M4. A sessão publica o PRONTO com cada item, medida e pendência. O hook confere identidade da obra e SHA. Após mais de três voltas Codex, pendências registradas permitem a inteira e a subida ao opus pela regra vigente. Uma recusa de PRONTO fecha o pedido antes do SSH. Falha de infraestrutura segue a contingência registrada da rota, com `PISTA: VM porque <motivo>` quando couber. Vermelho de teste retorna o rc do job. `CLAUDE.md:372–381`, `app/docs/RELATO.md:81,1010,1209`, `app/docs/BACKLOG.md:2191`.
- M5. T2 faz `raias_estado.py` expor o nome e o HEAD completo observado por `git rev-parse HEAD` em cada worktree da união, incluindo as só sujas. O ESTADO publica um `copia=<nome>@<HEAD12>` por cópia resolvida. Raia declarada sem caminho ou sem HEAD resolvível publica `raia=<nome> SEM WORKTREE`. O hook lê o mesmo medidor na hora do envio. O ESTADO é a fotografia publicada. `bin/raias_estado.py:42–48,73–135,138–174`, `bin/gerar_estado.py:211–224`, `bin/relato.sh:37–38,114–119`.
- M6. `UserPromptSubmit` examina aval colado com o token literal `CONSTRUIR` na primeira linha. Na linha `alvos=<nome>,<nome>`, listar todas as cópias tocadas e dar, na mesma ordem, um `copia=<nome>@<prefixo>` por alvo existente ou `copia=nova` na posição de cada alvo novo. Prefixo hexadecimal de 12 caracteres até o SHA completo. Nome é o basename da worktree, com escape percentual para delimitadores. `fora=<nome>,...` declara cópias vinculadas à obra que ficam fora do aval. Toda raia declarada com a mesma `OBRA` consta em `alvos` ou `fora`. A sessão explicita em `alvos` cópias tocadas sem declaração, inclusive descendentes. `bin/raias_estado.py:73–102,125–135`, `bin/raia_agente.sh:40–59`, `app/docs/RELATO.md:1513`, `travas_sem_memoria_v2_2011_0110.txt:5`.
- M7. ALVOS com nome repetido, ambíguo, omitido, sem HEAD ou com prefixo divergente devolvem `AVAL SEM LEITURA DA COPIA (HEAD atual <hash>)` por alvo, com `indisponível` onde nenhum HEAD puder ser lido. `copia=nova` vale para um alvo sem worktree elegível nem declaração, inclusive quando há outros alvos existentes no mesmo aval. Uma declaração sem worktree exige saneamento para aquele alvo. A sessão registra `AVAL-DEVOLVIDO <ID>` no topo do RELATO pelo escritor existente, como recado que preserva a janela do Stop, e segue os itens andáveis. Ordens sem o token no cabeçalho conservam a passagem atual pelo detector e pelas guardas próprias. `bin/raias_estado.py:73–102`, `bin/hook_stop_fila1.py:413–524,1381–1404`, `app/core/esteira_vigia.py:846–892`, `CLAUDE.md:262–275,436–438`, `bin/codex_pergunta.sh:252–259`.
- M8. Fronteira T2 = primeiro evento de controle comprovado com os registros carregados na sessão de trabalho. Registrar data/hora com fuso, sessão, SHA do instrumento e digest do settings. Aval recebido antes, provado em PROMPTS/BACKLOG, segue sem releitura. Toda recolagem após o marco é novo evento de validação. Recolagem válida de título já recebido segue a resposta de duplicata da casa. `CLAUDE.md:436–438`, `.claude/settings.json:2–73`, `app/docs/RELATO.md:3341,4356`.
- Alternativa T1 em uma linha. Um `bin/pista_pede.sh pre-opus-lateral <OBRA> <cópia>` pode carregar obra e cópia no argv, usando o mesmo leitor de PRONTO e a mesma interface SSH. `app/docs/BACKLOG.md:2191,2195`.
- Alternativa T2 em uma linha. Um bloco `alvo=<nome> copia=<nome>@<hash>` por alvo explicita a mesma associação da lista ordenada, inclusive `alvo=<nome> copia=nova`. `bin/raias_estado.py:89–135`, `travas_sem_memoria_v2_2011_0110.txt:5`.

### CONTRATO-DE-INSTRUMENTO

1. Quem chama — sessão/raia pelo Bash da raiz para T1, Code ao receber o aval para T2, `relato.sh` ao publicar o ESTADO. `.claude/settings.json:62–72`, `bin/relato.sh:37–38`, M2–M7.
2. Quem espera — sessão em shell de fundo, por `ssh pista espera <id>` e veredito com `Ran` e rc. T2 responde no evento de entrada. `app/docs/BACKLOG.md:2191`, `CLAUDE.md:316–319`, M7.
3. Falha/limite — recusa só do pedido ou aval afetado, antes de enfileirar ou usar o aval. Pista recebe árvore commitada. Divergência VM/pista fica registrada e suspende o uso até Ronald ver. `app/docs/BACKLOG.md:2191`, `CLAUDE.md:262–275`, M3–M8.
4. Medida — por caso, guardar porta, obra, SHA, rc/veredito, chamadas SSH/VM e linha de recusa ou publicação. Em job aceito, guardar id e tempo. `app/docs/RELATO.md:81,1497,1529`, M3–M8.
5. Quem desliga — Ronald, restaurando os dois registros anteriores e retirando a seleção da nova rota, com linha no RELATO. `app/docs/BACKLOG.md:2191`, `app/docs/RELATO.md:81,4356`, M1–M3.

## RED

Entradas de teste: `O=LATERAL-TESTE`, `C=wt-a`, `D=wt-b`, `H/J` = HEADs reais da fixture, `H0` = SHA anterior de C, `B` = base do recorte. `P` é `PISTA_LATERAL=LATERAL-TESTE bash bin/suite.sh --dir "$C" --motivo "pre-opus LATERAL-TESTE"`. `U` é colar `aval Ronald: LATERAL-TESTE CONSTRUIR` com o corpo do caso. Antes da cura, testar as mesmas entradas e registrar qual expectativa falhou. Resultados [nao verificado]. `CLAUDE.md:28–29`, `bin/tests/test_suite_motivo.sh:14–42`.

- **Pre-opus lateral** — P1 `P`, C commitada/ref apta, sem PRONTO → `SEM PRONTO ITEM A ITEM`, zero SSH e zero runner. P2 `P`, só há `PRONTO` de `O@H0` e de outra obra em H → mesma recusa. P3 `P`, marcador em `RELATO.md` para `O@H` → um `pede H` sem `--only`, id e `espera` com `Ran/OK/rc 0`. P4 `P`, mesmo marcador só no ARQUIVO, quatro voltas Codex e pendência escrita → mesmo pedido e subida ao opus após o verde. P5 `P`, PRONTO válido e job vermelho → `rc 1`, zero runner VM. `bin/suite.sh:236–263`, `app/docs/BACKLOG.md:2191`, `CLAUDE.md:372–381,439–442`, M3–M4.
- **Volta de lateral** — V1 `PISTA_LATERAL=LATERAL-TESTE bash bin/volta_intermediaria.sh "$C" "$B" "$OUT"`, C commitada e recorte próprio, sem PRONTO → ruff/selo e um `pede H --only <labels efetivas>`, zero trava VM. V2 mesmo comando, seleção cobre todos os módulos → guarda atual `rc 2` da suite, volta vermelha e zero SSH. V3 mesmo V1, pista indisponível e contingência vigente → recorte na VM e uma linha de causa. `bin/volta_intermediaria.sh:18–47`, `bin/suite.sh:162–180,215–263`, `app/docs/RELATO.md:81,1010`, M2–M4.
- **VM e focado** — L1 `bash bin/suite.sh --dir "$ESP" --motivo "pre-opus ESPINHA-TESTE"`, cópia sem PRONTO → inteira pela VM. L2 `bash bin/suite.sh --dir "$C" --motivo "pouso LATERAL-TESTE"`, cópia sem PRONTO → inteira pela VM. L3 `bash bin/suite.sh --dir "$C" --only "$MODULO"`, módulo próprio e sem PRONTO → recorte roda. L4 `bash bin/suite.sh --dir "$C" --motivo "pre-opus"`, inteira → `rc 2` anterior. Zero SSH nos quatro casos. `bin/suite.sh:215–263,316–325`, `app/docs/RELATO.md:65`, M2.
- **Régua e Git** — R1 `bash bin/regua.sh --force`, mesmo com carimbo verde e trava livre → runner integral executa, sem `--motivo`. G1 `git push remoto-teste HEAD:refs/heads/smoke`, hook instalado, commit novo e sem carimbo compatível → arsenal CI e `tenants` executam sobre o commit, push liberado se verdes. G2 mesmo push em fixture com carimbo compatível → reaproveita arsenal e executa `tenants`. Zero SSH. `bin/regua.sh:84–101,134–169`, `bin/hooks.sh:58–65`, `bin/pre-push.sh:27–74,159–200`, M2.
- **Aval** — A1 `U` com `alvos=wt-a`, sem `copia` → devolve com H. A2 `U` com `alvos=wt-a copia=wt-a@H0`, HEAD H → devolve com H. A3 `U` com `alvos=wt-a,wt-b copia=wt-a@H12 copia=wt-b@J`, ambas da obra → entra. A4 `U` com só `wt-a`, estando `wt-b` declarada para O → devolve; com `fora=wt-b` explícito → entra para C. A5 `U` com `alvos=wt-nova copia=nova`, nome livre → entra; mesmo envio com nome já declarado sem worktree → devolve. A6 `U` com `alvos=wt-sem copia=wt-sem@H12`, declaração sem worktree → devolve com HEAD indisponível. A7 `U` com C válida e `wt-nova` livre → entra com um token por alvo. Nos negativos, uma linha de devolução por evento e o próximo item andável segue. `bin/raias_estado.py:73–135`, `CLAUDE.md:262–275`, M5–M8.
- **Corte e outras entradas** — A8 `U` inválido recepcionado antes do marco, continuar o item após ativação → continuidade; recolagem do mesmo texto → recusa do novo evento. A9 colar `aval Ronald: LEITURA-TESTE` com `CONSTRUIR` só no corpo → T2 deixa passar. A10 enviar leitura, sonda e despacho pelos seus comandos vigentes de `bin/codex_pergunta.sh`, com entradas válidas e #G ativa → chegam às guardas atuais, zero recusa T1/T2. `CLAUDE.md:344–358,436–438`, `bin/codex_pergunta.sh:241–259`, M7–M8.
- **ESTADO** — E1 `python3 bin/gerar_estado.py "$RAIZ" "$OUT"` e `DEST_RELATO="$DEST_TESTE" bash bin/relato.sh` com fixture raspada, C e D declaradas, uma só suja e uma sem worktree → três `copia=<nome>@<HEAD12>` e uma `SEM WORKTREE`, no ESTADO gerado e publicado. E2 repetir após avançar só H → muda só o token de C entre as cópias. `bin/raias_estado.py:125–174`, `bin/gerar_estado.py:211–224,398–407`, `bin/relato.sh:23,37–38,114–147`, M5.

## SELO

- `bin/tests/test_travas_sem_memoria.sh`, no host, chamado pelo glob da régua antes do Django. Adaptar `test_suite_motivo.sh` para seleção efetiva, `test_raia_declarada_no_estado.sh` para HEAD/publicação e usar o molde de fixtures e mutantes de `test_disjuntor.sh`. SSH falso registra argv e enfileiramentos, Git real temporário dá os HEADs e runners de teste registram chegada. `CLAUDE.md:127–130`, `bin/regua.sh:134–145`, `bin/tests/test_suite_motivo.sh:14–42`, `bin/tests/test_raia_declarada_no_estado.sh:43–84`, `bin/tests/test_disjuntor.sh:82–101,1532–1547`.
- Despachar payloads pelos registros lidos do settings candidato, por evento e matcher. Controle íntegro verde. Retirar só o registro T1 derruba P1 no teste do evento; retirar só o T2 derruba A2. Aceitar PRONTO de qualquer SHA derruba P2, ignorar segunda cópia derruba A4 e omitir HEAD na publicação derruba E1. A chamada direta ao script complementa a prova do executor. `.claude/settings.json:62–72`, `bin/tests/test_disjuntor.sh:82–88,1532–1547`, M3–M8.
- Smoke pré-pouso em sessão Code descartável aberta na raiz da cópia candidata, com settings locais e comandos apontando para essa cópia. Provar carga real dos dois eventos, P1/P3 e A1/A2/A3/A8, recorte V1, publicação E1 e continuidade com próximo item andável. Se a instalação for negada, registrar a negativa e levar o evento ao Ronald. Carga, instalação e smoke [nao verificado]. `app/docs/PROMPTS.md:1877`, `app/docs/RELATO.md:3341,4356,4411`, M1–M8.

## PROIBIDO

- Barrar teste focado, leitura, sonda ou despacho; mexer nas #G, exigir `--motivo` de régua ou pre-push, mover espinha/pouso para a pista, transmitir dado de produção, tocar regra de negócio ou criar juiz. Pouso próprio só com o smoke e a verificação do instrumento. `fichas_para_rascunho_2011_0130.txt:3`, `bin/codex_pergunta.sh:241–259`, `app/docs/BACKLOG.md:2191`, `app/docs/RELATO.md:65`, `CLAUDE.md:265–275,325–328,372–381`.

## PRONTO

1. M1 — contrato de cinco linhas publicado e um envio ao Codex; diff de `bin/` e configuração local conferidos pela sessão e pelo Codex verificador. `CLAUDE.md:369–394`, `.gitignore:66`. [nao verificado]
2. M2–M4 — P1–P5, V1–V3, L1–L4, R1 e G1–G2 com contagens por porta; um job real aceito com id, SHA e rc, e zero jobs para pedido sem PRONTO. `app/docs/BACKLOG.md:2191`, `bin/regua.sh:134–145`.
3. M5–M8 — A1–A10 e E1–E2 com HEADs de referência, topo do RELATO, continuidade de fila e marco de ativação registrados; ESTADO publicado bate com o medidor da mesma geração. `bin/gerar_estado.py:211–224`, `bin/relato.sh:114–147`, `CLAUDE.md:262–275,436–442`.
4. SELO — RED na base pelo motivo da regra, controle íntegro verde, os cinco mutantes vermelhos e smoke da configuração candidata provado antes do pouso próprio. `CLAUDE.md:28–29,325–328`, `bin/tests/test_disjuntor.sh:1532–1547`.

LEI-AKITA: origem=bin/suite.sh:215–263 + .claude/settings.json:62–72, testemunha=app/docs/RELATO.md:65 + app/docs/BACKLOG.md:2191 + bin/raias_estado.py:89–135, RED=P1–E2, quem-mais-le=bin/gerar_estado.py:211–224 + bin/raia_agente.sh:80–81 + bin/hook_stop_fila1.py:413–524, juizes novos=0, padrao=fonte unica (`app/docs/GLOSSARIO.md:11`)

## CONTRADIÇÕES COM A FICHA

- CONTRADIZ a ficha (b): `app/docs/BACKLOG.md:2191` conserva o literal da suite para opus na VM; `app/docs/BACKLOG.md:2195` e `app/docs/RELATO.md:65` registram a opção A para lateral. `app/docs/PROMPTS.md:2459` registra também uma ampliação condicional para espinha. Vigência dessa ampliação [nao verificado].
- Nos anexos, `abre_as_laterais_e_despacha_2011_0110.txt:2,7` põe Codex como construtor e `travas_na_frente_2011_0112.txt:3` generaliza a rota interina. Os literais na VM ficam [nao verificado]. M1–M4 seguem a FICHA 2. `fichas_para_rascunho_2011_0130.txt:3`, `bin/codex_pergunta.sh:121–135,248–259`.

## BLOQUEIOS RESPONDIDOS

- **1 — Premissas.** 1(a) F3 confirma a interface documentada; 1(b) F5/M5 fixam a fonte; 1(c) F5/M5 fixam o publicador; 1(d) F4/SELO exigem carga real; **1(e)** M2 e V1–V2 julgam seleção efetiva, com o recorte da volta e o `rc 2` da inteira coberta. `app/docs/BACKLOG.md:2191`, `bin/suite.sh:162–180,215–263`.
- **2 — Colisão.** **2(a)** M2, L2 e R1/G1–G2 preservam as três portas; 2(b) M7/A1–A2 provam recado e fila; **2(c)** M2–M4 e P4/L1–L2 preservam a subida após três voltas e a VM para espinha/pouso; 2(d) PROIBIDO, A10 e M1 mantêm #G e contrato prévio; **2(e)** M1 nomeia a sessão para construir e instalar, com verificação posterior. `CLAUDE.md:95–106,262–275,349–356,372–394`, `app/docs/RELATO.md:65,4411`.
- **3(a)** M2–M4 ligam obra e SHA ao pedido antes de enfileirar; P2–P4 distinguem obra, SHA e arquivo. `app/docs/BACKLOG.md:2191`, `CLAUDE.md:439–442`.
- **3(b)** RED enumera as portas e os sinais, inclusive volta e ESTADO; SELO fixa a sessão de smoke pré-pouso. `bin/volta_intermediaria.sh:18–46`, `bin/relato.sh:114–147`, `app/docs/RELATO.md:3341`.
- **3(c)** SELO nomeia o teste de host e exige mutação independente de cada registro. `bin/regua.sh:134–145`, `bin/tests/test_disjuntor.sh:1532–1547`.
- **3(d)** M5–M7 e A4–A7 fecham alvos, nova por alvo, raia sem worktree e alcance do token. `bin/raias_estado.py:73–135`, `bin/codex_pergunta.sh:252–259`.
- **3(e)** M8 e A8 fecham ativação, continuidade e recolagem. `CLAUDE.md:436–438`, `app/docs/RELATO.md:3341,4356`.
- **4 — Sem bloqueio na revisão.** PROIBIDO delimita instrumento e metadados. `CLAUDE.md:265–268`, `bin/relato.sh:114–119`.

## 20/11 01:21 · sessao 9305ea6475f1 · E4C-F5-CONSTROI · leitura · origem=Code

TRIAGEM C | vermelho | o comando entregue usa `ponto.tests.test_selo_teto_modal`, mas o selo está em `chamados/tests/test_selo_teto_modal.py:37` | brief, entrega (d) | PROXIMO corrigir o label para `chamados.tests.test_selo_teto_modal` e medir a C no hash `57582ee14`

- Catraca AST conferida, 10 → 0 fora da permissão nominal. Amostras em `chamados/tests/test_e4c_f5_selo_chamada.py:21`, `chamados/management/commands/auditar_invariantes_chamados.py:179` e `chamados/services/materializacao.py:193`.
- Entraram 24 dos 25 K e a sobra de `dedup_perguntas_fato.py:75`. K40 ficou avulso, classe R. Amostras em `chamados/management/commands/backfill_decisoes_pergunta.py:534`, `chamados/services/vigia_fio.py:714`, `chamados/services/vigia_fio.py:719` e `chamados/management/commands/migrar_data_evento_dia_do_turno.py:466`.
- Os outros cinco R são K3, K19, K20, K22 e K58. `chamados/services/relavra_via.py:236`, `chamados/services/materializacao.py:2122`, `chamados/services/validacao.py:677`, `chamados/services/materializacao.py:286`, `chamados/services/validacao.py:431`.
- G segue avulso. K13 recarrega o restante após cada reabertura bem-sucedida, com custo SQL [nao verificado]. `chamados/reconciliador.py:367`, `chamados/services/disputa_emissao.py:473`, `chamados/management/commands/backfill_fio_morto_e4b.py:140`.
- O construtor preservou todos os `models.py`. O item D entrou no commit posterior `9d386d1b1`, em `chamados/models.py:2068`.
- As adaptações i, ii e v preservaram os asserts e seus operandos. A fixture da catraca ainda acusa a chamada crua de ordem 2. `chamados/tests/test_e4c_f5_selo_chamada.py:199`, `chamados/tests/test_bug_par_sem_banco.py:197`, `chamados/tests/test_bug_par_c2_sem_banco.py:32`, `chamados/tests/test_bug_par_c2_sem_banco.py:86`, `chamados/tests/test_estado_aplicacao.py:55`.
- A lista `RESTAM` passou a vazia conforme o brief. Os valores esperados e os tetos do modal e dos quatro cenários de PASSAGEM foram preservados. RED/verde, mordida ORM e contagens SQL da C [nao verificado]. `chamados/tests/test_e4c_f5_selo_chamada.py:21`, `chamados/tests/test_selo_teto_modal.py:184`, `ponto/tests/test_selo_chokepoint_escrita.py:196`, `ponto/tests/test_selo_chokepoint_escrita.py:210`, `ponto/tests/test_selo_chokepoint_escrita.py:228`, `ponto/tests/test_selo_chokepoint_escrita.py:235`.

## 20/11 01:12 · sessao d4e67d9bba0e · CADEIA-20-PASSO-1 · verificador · origem=Code

# AVAL-DEVOLVIDO — CADEIA-20-IMPRESSAO-SEM-REALIMENTACAO-2011-0110

O `PAREI` do item 2 do aval já se aplica. `status_local` recebe a mesma transição pela porta humana e pela cadeia. A contagem `fio` também pode mudar por atos dos dois lados. `app/chamados/catalogo/motor.py:98–113`, `app/chamados/models.py:459–462,654–668`, `app/ponto/services/cartorio.py:1123–1141,1527–1545`, `app/ponto/services/veredito_celula.py:184–197`.

Identidade Git da cópia com `a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6` [nao verificado].

## 1. Premissas

- **1a — NAO PROCEDE / NAO BLOQUEIA.** `502–507` monta a base da impressão. `543–547` explica e acrescenta `fio`. A lista completa é **batidas** (`pk`, `tipo`, `timestamp`); **status da ausência que cobre o dia**; **chamados de batida** (`pk`, `status_local`); **DNA** (`dna_versao`, `marcos`); e os condicionais **`veto`, `teto`, `regra`, `enc`, `fio`, `cobparcial`, `isento`, `sem_adesao`**. `cobparcial` pode incluir período e índices de marcos dispensados. O resultado é SHA-256, gravado em `CelulaDia.impressao`. `app/ponto/services/cartorio.py:141–157,477–490,493–578,1496–1501`, `app/ponto/portas/celula.py:1189–1201`.
- **1b — PROCEDE / BLOQUEIA.** O código permite rastrear as portas e suas dependências. A divisão binária do aval falha em dois insumos. **`ChamadoColaborador.status_local`** é misto até na transição `em_analise → resolvido`. A porta humana e `reconciliar()` chegam ao mesmo escritor, e o fechamento adiado chama `reconciliar()`. **`fio`** é uma derivação mista. Conta chamados vivos com pelo menos uma pergunta viva. `fio:0` indica chamados vivos sem fio e a parte é omitida quando não há chamados vivos. Entre os insumos do fio, **`PerguntaDisputa.materializada_em` e `via_resolucao`** mudam tanto no fechamento pelo admin quanto no E2. A fábrica também pode desfazer carimbos ou criar perguntas após a lavra. Qual ato causou cada uma das oito diferenças [nao verificado]. `app/chamados/services/acoes_chamado.py:322–325`, `app/chamados/catalogo/motor.py:98–113`, `app/chamados/models.py:234–235,459–462,654–668,3246–3260,3320–3325,3378–3386`, `app/chamados/reconciliador.py:634–645`, `app/chamados/services/fabrica_da_celula.py:160–174,206–219`, `app/ponto/services/cartorio.py:543–547,1123–1141,1179–1181,1527–1545`.
- **1c — NAO PROCEDE / NAO BLOQUEIA.** A chamada omite `agora` e `fio_vivo` hoje só aceita `perguntas`. O lote captura `agora` na entrada e o passa a `julgar_colab`. O corpo recebe o mesmo valor, também disponível em `agora_deste_julgamento()`. O avulso aceita `agora` ou o captura na entrada. `pergunta_viva` já aceita `agora`, deriva `hoje` dele e projeta os carimbos naquele instante. `app/ponto/management/commands/processar_cartorio.py:40–41,63–69`, `app/ponto/services/cartorio.py:101–106,1110–1123,1264–1265,1532,2202–2204`, `app/chamados/juizes.py:866–898,1382–1404,1510–1519`.
- **1d — PROCEDE / BLOQUEIA.** R5 compara a foto após a primeira rodada com a foto após a segunda. O instrumento roda cartório sem `--forcar` e recálculo na competência 10/2026, empresas 2, 3 e 4. A foto da célula inclui ata e veredito, mas omite `impressao` e `julgada_em`. Uma troca só de carimbo pode dar R5 zero. O RED precisa comparar `CelulaDia.impressao` diretamente. `app/core/placar_estrutural.py:416–431`, `bin/r5_idempotencia_frota.py:26–32,52–57,104–130`, `app/ponto/portas/celula.py:1189–1201`.

## 2. Colisão com o aval vigente

- **2a — NAO PROCEDE / NAO BLOQUEIA.** Pela ordem informada no pedido, o RED é compatível com o aval vigente se entrar como prova adicional. A causa por célula e a revisão do verificador continuam antes de `PRONTO`, seguido de opus e pouso. O literal integral do aval 2325 e o cumprimento dessa ordem [nao verificado]. [nao verificado]
- **2b — PROCEDE / BLOQUEIA.** Falta delimitar a vigência da mudança. A retirada incondicional de uma parte muda o hash de células já carimbadas e pode provocar relavra pelo simples pouso. L-124 preserva a foto anterior ao horizonte até fato novo. `versao_para` seleciona a regra por competência e a proteção depende de preservar os bytes da impressão antiga fora do horizonte. A soberania do DNA segue pela porta da célula. `app/docs/LEIS.md:71,145`, `app/docs/PLANO-PADROES.md:70–76`, `app/ponto/services/cartorio.py:280–299,502–507,543–547,1541–1558`.
- **2c — NAO PROCEDE / NAO BLOQUEIA.** O desenho pode manter o mesmo juiz e a mesma porta. O cartório consulta `classificar_dia` e lavra pela porta da célula. `pergunta_viva` e `fio_vivo` continuam como autoridades registradas. A retirada de um insumo do hash ainda precisa preservar o gatilho de releitura, porque o juiz segue recebendo vivos e fio, e a releitura interna sem `forcar` depende da impressão. `app/docs/JUIZES.md:10–15,123–134`, `app/core/juizes.py:289–299`, `app/ponto/services/cartorio.py:1–11,1202–1223,1614–1618,2013–2037`.

## 3. Literal

- **3a — NAO PROCEDE / NAO BLOQUEIA.** O caso humano é montável pela rota real. Use célula passada já carimbada no ramo normal e chamado de batida `em_analise`, com dia identificável e sem pergunta viva que impeça a resolução. Entre os passes, o gestor envia `novo_status=resolvido`. O `post_save` tenta julgar na hora. O teste deve capturar a impressão antes e logo após o ato humano, pois o segundo lote já pode encontrar o carimbo em dia. Execução do teste [nao verificado]. `app/chamados/urls.py:14`, `app/chamados/views.py:546–547`, `app/chamados/services/acoes_chamado.py:292–297,322–331`, `app/chamados/models.py:134–177`, `app/chamados/signals.py:189–218`, `app/ponto/services/cartorio.py:1552–1558`.
- **3b — PROCEDE / BLOQUEIA.** O literal de `8 → 0` aponta para as oito de (B). O anexo nomeia seis e deixa `+2` [nao verificado]. Para fechar o RED, faltam os oito IDs e a comparação da impressão após as rodadas 1 e 2, com o relógio e a sequência de etapas definidos. O zero de frota do R5 tem universo próprio. O resultado do terceiro passe das 19 de (A) [nao verificado]. `app/core/placar_estrutural.py:416–431`, `bin/r5_idempotencia_frota.py:26–27,104–130`, `app/ponto/management/commands/processar_cartorio.py:40–41`.
- **3c — PROCEDE / BLOQUEIA.** **FM** é `ponto.FechamentoMensal` e **ED** é `folha.ExportacaoDominio`. A sigla é identificável. O literal de aceite ainda precisa fixar empresas, competências, linhas, campos e o instante das duas fotos. Há um hash de FM por colaborador e competência na porta de regeneração. Em ED, `hash_sha256` cobre o TXT e pode ficar igual mesmo quando a linha tem sua vigência alterada. `app/ponto/portas/regen_exportada.py:56–74`, `app/ponto/management/commands/lavrar_status_competencia.py:10–14`, `app/folha/models.py:75–96,143–169,174–186`.

## 4. Exportada, dinheiro e `!`

- **4a — PROCEDE / BLOQUEIA.** Em competência **aberta**, mudar quando a célula é pulada ou relavrada pode alterar a ata que o próximo recálculo lê. O fechamento usa a grade da ata para previsto, abono, realizado e retenção, e a mesma derivação alimenta `DiaPago`. Isso abre um caminho indireto para mexer no dinheiro. O delta efetivo [nao verificado]. `app/ponto/services/cartorio.py:1552–1558,2013–2037`, `app/folha/export.py:329–338`, `app/escala/services/leitor_celula.py:409–432`, `app/ponto/services/fechamento.py:1013–1014,1046–1068,1191–1200`.
- **4b — PROCEDE / BLOQUEIA.** Em competência **exportada**, o lote seleciona células passadas sem filtro de entrega, e a porta de carimbo pode persistir impressão e ata. Portanto, FM e ED iguais ainda permitem toque em célula exportada. Um recálculo posterior pode escrever no gravado pelos caminhos admitidos pela L-113. O aval precisa delimitar a exclusão da exportada pelo juiz de entrega e provar a invariância; se houver movimento de dinheiro, vale o regime de `!` aplicável. Toque efetivo ou delta [nao verificado]. `app/ponto/management/commands/processar_cartorio.py:47–69`, `app/ponto/portas/celula.py:1189–1201`, `app/folha/export.py:556–574`, `app/ponto/services/fechamento.py:525–565`, `app/docs/LEIS.md:67,109,112,155`.

**O que falta no aval por bloqueio**

- **1b** — Decidir o tratamento de `status_local` e `fio`, ambos mistos. `app/chamados/catalogo/motor.py:98–113`, `app/ponto/services/cartorio.py:1123–1141,1527–1545`.
- **1d** — Exigir a comparação direta de `CelulaDia.impressao`. `bin/r5_idempotencia_frota.py:52–57`.
- **2b** — Fixar o horizonte e preservar a impressão anterior fora dele. `app/docs/LEIS.md:145`, `app/ponto/services/cartorio.py:280–299`.
- **3b** — Nomear as oito células e fixar relógio, etapas e universo de cada prova. `app/core/placar_estrutural.py:416–431`, `app/ponto/management/commands/processar_cartorio.py:40–41`. Dois IDs [nao verificado].
- **3c** — Fixar o escopo e os campos das fotos FM e ED. `app/ponto/portas/regen_exportada.py:56–74`, `app/folha/models.py:75–96,143–169`.
- **4a** — Exigir diff de dinheiro e retenção na aberta, incluindo `DiaPago`. `app/ponto/services/fechamento.py:1046–1068,1191–1200`.
- **4b** — Exigir prova de que a exportada fica intacta e explicitar o `!` se houver impacto financeiro. `app/ponto/management/commands/processar_cartorio.py:47–69`, `app/docs/LEIS.md:67,109,112,155`.

`VEREDITO: BLOQUEIA=7 NAO_BLOQUEIA=5`

## 20/11 01:12 · sessao 98f2e59db7cb · TRAVAS-SEM-MEMORIA · verificador · origem=Code

# AVAL-DEVOLVIDO — TRAVAS-SEM-MEMORIA-V2-2011-0110

Revisão só de leitura. As referências aos anexos 1 e 4 contam as linhas a partir de `aval Ronald`. O hash da cópia, a configuração carregada na VM e os resultados de smoke no ar ficam **[nao verificado]**.

## 1. Premissas — PROCEDE · BLOQUEIA

- **1(a) Pista remota — NAO PROCEDE · NAO BLOQUEIA.** A **PISTA-2** está documentada no servidor do GLPI, com fila própria. A sessão registra o commit em `refs/pista/<sha>` e pede `ssh pista pede <sha> [--only <labels>] [--parallel N]`. Sem `--only`, o registro mostra uma inteira do commit `a0bf6e78e`, `Ran 15875`, `OK`, `rc 0`. Quem devolve o rc da suíte é `ssh pista espera <id>`. Há registro de `rc 1` em teste vermelho e de `rc 71` numa tentativa sem a ref. O código do executor e sua disponibilidade atual ficam **[nao verificado]**. `app/docs/BACKLOG.md:2191–2192`, `app/docs/RELATO.md:201,1497,1529`.
- **1(b) Fonte das cópias — NAO PROCEDE · NAO BLOQUEIA.** `bin/raias_estado.py` cruza `git worktree list --porcelain` com `logs/raias/<raia>.agente`, escrito por `bin/raia_agente.sh`. Inclui as declaradas, as à frente de `main` e as sujas. Exclui a árvore principal e as worktrees do harness. As só sujas saem numa lista de nomes. Uma declaração pode sair sem caminho no disco. A lista efetiva da VM fica **[nao verificado]**. `bin/raias_estado.py:51–69,73–135`, `bin/raia_agente.sh:40–59`.
- **1(c) Encaixe no ESTADO — NAO PROCEDE · NAO BLOQUEIA.** `bin/relato.sh` gera, raspa e publica `ESTADO.md`. A seção **Raias** já usa `ler()` e `linha()` de `bin/raias_estado.py`, onde cabe acrescentar o `copia=<nome>@<HEAD12>`. O retorno atual traz caminho e branch, mas ainda não publica o HEAD por cópia. `bin/relato.sh:37–38,114–119,133–147`, `bin/gerar_estado.py:211–229,329–331,378–380`, `bin/raias_estado.py:125–160`.
- **1(d) Eventos — NAO PROCEDE · NAO BLOQUEIA.** T1 tem o encaixe de `PreToolUse` em `.claude/settings.json`, que chama `disjuntor.py pretool`. O script lê JSON de stdin e usa `tool_input.command` para `Bash`. O `tool_input.prompt` ali é da ferramenta `Agent`. Para T2, o encaixe previsto é `hooks.UserPromptSubmit` no settings do Code, ausente no arquivo da cópia. O Stop usa `transcript_path` do payload. O RELATO registra um caso em que abrir em `app/` deixou os hooks da raiz inertes e a resposta mandou abrir na raiz no próximo restart. Suporte efetivo de T2 e carga atual dos hooks ficam **[nao verificado]**. `.claude/settings.json:2–73`, `bin/disjuntor.py:1837–1843,1864–1867,1912–1923`, `bin/hook_stop_fila1.py:1339–1345`, `app/docs/RELATO.md:3341`, `app/docs/PROMPTS.md:1877`.
- **1(e) Inteira versus recorte — PROCEDE · BLOQUEIA.** A guarda atual julga a **seleção efetiva**. Uma seleção vazia ou que cubra todos os módulos reconhecidos é inteira, mesmo com módulos em `--only`. `--volta <base>` calcula a seleção depois de entrar no script. A volta intermediária roda ruff, o selo do disjuntor e o recorte de obra, vizinhos e selos globais. Uma leitura superficial de “sem módulos nomeados” barraria `bin/volta_intermediaria.sh` ou `suite.sh --volta` mesmo quando o recorte é efetivo. Já `suite.sh --dir <cópia> --motivo "pre-opus <OBRA>"` e a variante `pouso <OBRA>` são portas válidas hoje e ficam barradas pelo literal da V2 quando rodam localmente. Com `--motivo "pre-opus"` sem obra, a inteira **já sai `rc 2`**. A régua usa `${ONLY:-$LABELS}` e o pre-push usa as `LABELS` no arsenal e `tenants` em runner próprio. `travas_sem_memoria_v2_2011_0110.txt:3–4,8`, `bin/suite.sh:162–180,215–263,316–325`, `bin/volta_intermediaria.sh:18–33`, `bin/regua.sh:47–51,165–169`, `bin/pre-push.sh:184–199`.

## 2. Colisão com aval ou regra vigente — PROCEDE · BLOQUEIA

- **2(a) §3, INTEIRA SO COM MOTIVO — PROCEDE · BLOQUEIA.** A V2 nomeia régua e pre-push, mas exige também `--motivo` para a inteira local. A régua recebe `--force` ou `--only` no fluxo atual. O pre-push é chamado pelo Git com os argumentos e o stdin do push. Nenhum dos dois exige `--motivo` hoje. O §3 também admite a inteira canônica de **pouso** por `suite.sh`, que ficou fora da exceção da V2. `travas_sem_memoria_v2_2011_0110.txt:3,6`, `CLAUDE.md:95–106`, `bin/regua.sh:46–51,84–94`, `bin/hooks.sh:58–65`, `bin/pre-push.sh:6–8,27–39`, `app/docs/RELATO.md:205`.
- **2(b) §7b, PARADA 1 — NAO PROCEDE · NAO BLOQUEIA.** A V2 manda devolver só o aval inválido, registrar `AVAL-DEVOLVIDO` e seguir a fila andável. Isso é compatível com PARADA 1. O Stop cobra a fila e só trata `PAREI` como saída ordinária quando há `!` no item em curso e nenhum outro item andável. Há também a saída de `ESPERANDO` provado. A entrega do recado de T2 à sessão precisa ser provada no smoke. `travas_sem_memoria_v2_2011_0110.txt:5,10`, `CLAUDE.md:262–275,296–298`, `bin/hook_stop_fila1.py:1232–1245,1260–1276,1381–1404`.
- **2(c) CODEX 5(c), SUITE-ANTES-DO-OPUS — PROCEDE · BLOQUEIA.** A regra exige a inteira verde da árvore da obra antes do opus. A rota aprovada manda à PISTA-2 a volta e a pre-opus **de lateral**, mantendo **pre-opus da espinha e pouso na VM**. O contrato da pista é de pré-filtro e há registro separado da certificação ainda pendente. A V2 generaliza a rota remota e pode fechar também a subida ao opus após mais de três voltas Codex se o `PRONTO` depender de aprovação que essa subida dispensa. O RELATO registra a retirada de uma trava anterior por esse último motivo. `travas_sem_memoria_v2_2011_0110.txt:3–4`, `CLAUDE.md:372–381`, `app/docs/BACKLOG.md:857–858,2191,2194–2195`, `app/docs/RELATO.md:65,1209`.
- **2(d) Travas `#G` — NAO PROCEDE · NAO BLOQUEIA.** A V2 proíbe mexer nelas e preserva leitura, sonda e despacho. O wrapper continua sendo a porta do censo, da vaga e da validação da worktree. A exigência de contrato de cinco linhas no RELATO antes do código e envio único ao Codex continua aplicável. `travas_sem_memoria_v2_2011_0110.txt:2,8`, `CLAUDE.md:344–358,394`, `bin/codex_pergunta.sh:241–259`.
- **2(e) ABRE-AS-LATERAIS-E-DESPACHA — PROCEDE · BLOQUEIA.** A posição depois de LASTRO-REDESENHO e COMP-PAGA é compatível com a ordem declarada. O conflito é de executor. O anexo 4 diz `Construtor = Codex` e reserva opus novo para o final do pouso. O construtor Codex só escreve em `<wt>/app` e o wrapper proíbe `bin/` e `.claude/`, justamente os locais do instrumento. O teto de quatro também não abre uma segunda vaga de construtor Codex no mesmo repo, pois o wrapper mantém um `flock` não bloqueante. A ocupação e o cruzamento real de arquivos ficam **[nao verificado]**. `abre_as_laterais_e_despacha_2011_0110.txt:1–2,6–7`, `travas_sem_memoria_v2_2011_0110.txt:1–5,8`, `CLAUDE.md:349–356`, `bin/codex_pergunta.sh:121–135,248–254`.

## 3. Literal — PROCEDE · BLOQUEIA

- **3(a) T1, ponto e alcance da guarda — PROCEDE · BLOQUEIA.** `PreToolUse` vê a chamada externa enviada à ferramenta. Um `git push` chega como `git push`, e o pre-push roda como filho do Git. Em `ssh pista pede <sha>`, a interface documentada recebe SHA e opções de teste, sem obra nem motivo. O marcador `PRONTO <OBRA> item a item:` agora tem forma canônica e pode ser lido no `RELATO-ARQUIVO.md`. Ainda falta dizer onde a guarda vai relacionar a obra ao pedido remoto e se um `PRONTO` arquivado de volta anterior vale para o SHA atual. `travas_sem_memoria_v2_2011_0110.txt:3–4`, `bin/disjuntor.py:1837–1843`, `bin/hooks.sh:58–65`, `app/docs/BACKLOG.md:2191`, `CLAUDE.md:439–442`.
- **3(b) Oito casos e RED — PROCEDE · BLOQUEIA.** **(a)** precisa de chamada local com motivo válido para distinguir a trava nova do `rc 2` existente. **(b)** precisa exercitar as duas portas, incluindo o arsenal do pre-push e uma régua que realmente execute, pois ambos têm caminhos de reaproveitamento. **(c–d)** precisam do mesmo pedido remoto válido, variando o `PRONTO`, com prova de recusa antes de enfileirar e de job aceito no positivo. **(e–h)** descrevem quatro resultados testáveis em sessão com T2 ativo, incluindo a continuidade da fila após **(e–f)**. Os oito casos deixam sem prova direta a volta intermediária livre e a publicação de `copia=…` no ESTADO. A V2 também deixa em aberto como ativar os hooks para o smoke exigido **antes** de autorizar o pouso. Os resultados pré-cura e no ar ficam **[nao verificado]**. `travas_sem_memoria_v2_2011_0110.txt:5–8`, `bin/suite.sh:255–263`, `bin/regua.sh:84–94`, `bin/pre-push.sh:43–74,159–188`, `app/docs/BACKLOG.md:2191`, `app/docs/RELATO.md:3341,4356,4411`.
- **3(c) Selo e mutantes — PROCEDE · BLOQUEIA.** Cabe como **selo de host da régua**. Já há teste que injeta payloads no hook e mata mutantes numa cópia temporária com controle sem mutação. A `suite.sh` roda o runner Django, cujo container não monta `bin/` nem `.git`. O literal precisa dizer onde o selo roda e provar que remover o registro de T1 ou T2 faz falhar a recusa correspondente. Invocar o script diretamente pode passar com o registro ausente no settings. `travas_sem_memoria_v2_2011_0110.txt:7`, `CLAUDE.md:127–130`, `bin/regua.sh:134–145`, `bin/suite.sh:316–325`, `bin/tests/test_disjuntor.sh:80–88,1532–1547`.
- **3(d) T2, identidade da cópia — PROCEDE · BLOQUEIA.** Comparar por prefixo resolve a diferença entre HEAD12 e SHA completo. Falta definir como o texto identifica **todas** as cópias tocadas para detectar uma omitida, o que fazer com uma raia sem worktree resolvível e quando `copia=nova` vale por alvo. Uma obra já aparece no RELATO com duas cópias descendentes. O próprio anexo 4 manda abrir vagas sem `CONSTRUIR` na primeira linha, portanto uma ordem futura nesse formato ficaria fora do detector literal. `travas_sem_memoria_v2_2011_0110.txt:5–6`, `abre_as_laterais_e_despacha_2011_0110.txt:1–2`, `bin/raias_estado.py:89–102,125–135`, `bin/codex_pergunta.sh:252–259`, `app/docs/RELATO.md:1513`.
- **3(e) T2, corte temporal — PROCEDE · BLOQUEIA.** É mensurável se o aval fixar a fronteira observável. Aplicar só aos novos eventos após ativação permite provar um aval recebido antes, sua continuidade após ativar e a recusa de um novo envio inválido. Se **recolar** o mesmo aval também preservar a dispensa, é preciso identificar o recebimento anterior. A regra atual registra recebimento e título repetido, mas o anexo não amarra essa trilha ao pouso de T2. `travas_sem_memoria_v2_2011_0110.txt:5–7`, `CLAUDE.md:436–438`, `.claude/settings.json:2–73`.

## 4. Exportada, dinheiro ou `!` fora do declarado — NAO PROCEDE · NAO BLOQUEIA

O literal autoriza hooks e publicação de metadados no ESTADO. Não pede escrita em competência exportada, apply de dinheiro, alteração do motor v2 ou atalho de teste. O publicador atual consulta a lista de nomes para a raspagem. Instalar a configuração do Code faz parte do escopo declarado e precisa do executor resolvido em 2(e). A permissão efetiva de instalação na VM fica **[nao verificado]**. `travas_sem_memoria_v2_2011_0110.txt:1–8`, `CLAUDE.md:141–142,265–268,344–355`, `bin/relato.sh:30–38,114–119`, `bin/relato_nomes.sh:8–19`, `app/docs/RELATO.md:4356,4411`.

**Falta no aval, por item bloqueante**

- **1(e)** Adotar a seleção efetiva da guarda atual e declarar a passagem da volta intermediária quando ela é recorte. `bin/suite.sh:162–180,222–263`.
- **2(a)** Preservar as entradas atuais da régua e do pre-push e explicitar a porta local do pouso. `CLAUDE.md:95–106`, `bin/hooks.sh:58–65`.
- **2(c)** Manter a certificação da espinha e do pouso na VM e preservar a subida após três voltas Codex. `app/docs/RELATO.md:65,1209`, `CLAUDE.md:372–381`.
- **2(e)** Nomear quem constrói e instala o instrumento sob o anexo 4 com as `#G` vigentes. `abre_as_laterais_e_despacha_2011_0110.txt:2,7`, `bin/codex_pergunta.sh:121–135,248–254`.
- **3(a)** Fixar a porta que associa o pedido remoto à obra e a validade do `PRONTO` arquivado. `app/docs/BACKLOG.md:2191`, `CLAUDE.md:439–442`.
- **3(b)** Fixar a ativação pré-pouso e os sinais observáveis de RED, smoke, volta livre e ESTADO publicado. `travas_sem_memoria_v2_2011_0110.txt:5–8`, `app/docs/RELATO.md:3341,4356`.
- **3(c)** Nomear o selo de host e exigir falha ao retirar cada registro de hook. `bin/regua.sh:134–145`, `bin/tests/test_disjuntor.sh:80–88,1532–1547`.
- **3(d)** Definir a associação obra–cópias, o alcance de `copia=nova` e as ordens de construção sem o token no cabeçalho. `travas_sem_memoria_v2_2011_0110.txt:5`, `abre_as_laterais_e_despacha_2011_0110.txt:1–2`, `bin/raias_estado.py:89–135`.
- **3(e)** Fixar o marco observável de ativação e dizer se recolagem conta como novo envio. `travas_sem_memoria_v2_2011_0110.txt:5`, `CLAUDE.md:436–438`.

VEREDITO: BLOQUEIA=3 NAO_BLOQUEIA=1

## 20/11 01:11 · sessao 5b1249fc8197 · ABRE-AS-LATERAIS · leitura · origem=Code

QUARENTENA | andável NÃO | perguntas de lei abertas: 1 identificada, como semear o início da isenção sem tirar #3057 do corte dos 8, com 15 ou 16 janelas. Um consolidado registrou 3, mas o enunciado e o estado das outras duas ficam [nao verificado]. [RELATO.md:2085](/home/ronald/saas-hasner/app/docs/RELATO.md:2085) [RELATO.md:1801](/home/ronald/saas-hasner/app/docs/RELATO.md:1801) | aval `QUARENTENA-REABRE-COM-CENSO` já veio, `E3-NA-VAGA` suspendeu a execução e o corte sobre as janelas segue pendente. [PROMPTS.md:2168](/home/ronald/saas-hasner/app/docs/PROMPTS.md:2168) [RELATO.md:2084](/home/ronald/saas-hasner/app/docs/RELATO.md:2084) [RELATO.md:2085](/home/ronald/saas-hasner/app/docs/RELATO.md:2085) | após o corte, abrir cópia nova do main, rodar o RED preservado em `c9a2a3846`, reaplicar a especificação de Quarentena+SVV pela matriz da porta e provar o DIFF de frota. [BACKLOG.md:2781](/home/ronald/saas-hasner/app/docs/BACKLOG.md:2781) [RELATO.md:2139](/home/ronald/saas-hasner/app/docs/RELATO.md:2139) | o WIP toca só [`test_silencio_vencido_volta.py:49`](/home/ronald/wt-quarentena-svv-151703/app/chamados/tests/test_silencio_vencido_volta.py:49). Lista final da reaplicação [nao verificado].

FERIADO-POR-POSTO | andável SIM | perguntas de lei abertas: 0 registradas. Fonte única, R7, C2 e A3 têm resposta. A5 segue como proposta de tela com três perguntas, cujos enunciados ficam [nao verificado]. [RELATO.md:149](/home/ronald/saas-hasner/app/docs/RELATO.md:149) [RELATO.md:76](/home/ronald/saas-hasner/app/docs/RELATO.md:76) [RELATO.md:77](/home/ronald/saas-hasner/app/docs/RELATO.md:77) [RELATO.md:78](/home/ronald/saas-hasner/app/docs/RELATO.md:78) [RELATO.md:44](/home/ronald/saas-hasner/app/docs/RELATO.md:44) | `FERIADO-RESPOSTAS-1911-1530` já veio. Falta resposta à `FERIADO-A5-TELA`. [PROMPTS.md:2654](/home/ronald/saas-hasner/app/docs/PROMPTS.md:2654) [RELATO.md:44](/home/ronald/saas-hasner/app/docs/RELATO.md:44) | retomar de `c853e8f2b`, fechar a prévia A5 nos dois sentidos após a resposta e comprovar `sombra_c2b=0` antes de certificar o pouso. [RELATO.md:39](/home/ronald/saas-hasner/app/docs/RELATO.md:39) [RELATO.md:57](/home/ronald/saas-hasner/app/docs/RELATO.md:57) [teste do item 4:519](/home/ronald/wt-feriado-posto-135549/app/colaboradores/tests/test_feriado_por_posto.py:519) | a prévia referencia a porta em [`feriado_do_posto.py:268`](/home/ronald/wt-feriado-posto-135549/app/colaboradores/services/feriado_do_posto.py:268). Arquivos a editar para A5 [nao verificado].

TRIAGEM-OFFLINE-RED-HEADLESS | andável SIM | perguntas de lei abertas: 0 registradas. A classe C1 já está identificada e o RED reproduziu clique e reload offline nas duas cascas. [BACKLOG.md:2806](/home/ronald/saas-hasner/app/docs/BACKLOG.md:2806) [RELATO.md:2207](/home/ronald/saas-hasner/app/docs/RELATO.md:2207) | `TRIAGEM-REVERTE-JA` e `TRIAGEM-REVERTE-JA-COMPLEMENTO` já vieram. `RESCALDO-CORTE-1511` deixou a cura na fila de construção quando abrir vaga. [PROMPTS.md:2146](/home/ronald/saas-hasner/app/docs/PROMPTS.md:2146) [PROMPTS.md:2148](/home/ronald/saas-hasner/app/docs/PROMPTS.md:2148) [RELATO.md:4102](/home/ronald/saas-hasner/app/docs/RELATO.md:4102) | partir de `d680518b0`, curar a navegação offline, passar o teste headless e colher o smoke do Ronald nas duas cascas. [RELATO.md:2207](/home/ronald/saas-hasner/app/docs/RELATO.md:2207) | cura em [`app/static/sw.js:46`](/home/ronald/wt-codex-triagem-offline-red-headless-101137/app/static/sw.js:46), com prova em [`test_sw_offline_headless_red.py:459`](/home/ronald/wt-codex-triagem-offline-red-headless-101137/app/core/tests/test_sw_offline_headless_red.py:459).

**Ordem no recorte das três candidatas do aval 01:10** — vaga 3 FERIADO-POR-POSTO, vaga 4 TRIAGEM-OFFLINE-RED-HEADLESS. O aval posterior `TRAVAS-NA-FRENTE-2011-0112` destina a vaga 3 a TRAVAS-V2 após revisão sem bloqueio e reserva a vaga 4 ao LASTRO-REDESENHO até liberar os arquivos. [RELATO.md:8](/home/ronald/saas-hasner/app/docs/RELATO.md:8) [PROMPTS.md:2671](/home/ronald/saas-hasner/app/docs/PROMPTS.md:2671) [RELATO.md:2](/home/ronald/saas-hasner/app/docs/RELATO.md:2)

## 20/11 01:08 · sessao c5737160597a · E4C-F5-CONSTROI · construtor · origem=Code

## E4C-F5-CONSTROI · Volta C

A catraca AST mediu **10 → 0** chamadas cruas fora da permissão nominal. Foram convertidos os dez chamadores e carregados em lote **24 K + a sobra do dedup**. Restam **6 K por item**, todos classe R. A checagem local compilou os 19 arquivos em memória, conferiu as âncoras dos mutantes e passou em `git diff --check`. RED na base, verde na C, contagens SQL e suíte estão [nao verificado]. A certificação fica com o Code e o Opus. `chamados/tests/test_e4c_f5_selo_chamada.py:21`, `:183`, `chamados/tests/test_e4c_f5_voltaC_lotes.py:74`, `chamados/tests/e4c_f5_voltaC_mutantes.py:12`.

## Chamadores diretos

Todos passam de ORM cru para `DecisaoPergunta.composta(...)`. O instante vigente vem da sequência com o passivo, e o texto vem de `p.resposta_colab`. A tabela lista as dez identidades que saíram da catraca. A lista final de `RESTAM` está vazia. A única permissão é `creditos_perguntas`, que entrega a tupla do `values_list`. `chamados/models.py:4317`, `:4351`, `chamados/tests/test_e4c_f5_selo_chamada.py:21`, `:23`, `chamados/services/raiox.py:135`, `:147`.

| Sítio | Base → C | Carga |
|---|---|---|
| Auditoria `_contraprova_i21a` | `chamados/management/commands/auditar_invariantes_chamados.py:178 → :179` | K5 |
| Auditoria I2 | `chamados/management/commands/auditar_invariantes_chamados.py:597 → :598` | K6 |
| Auditoria F1 | `chamados/management/commands/auditar_invariantes_chamados.py:1671 → :1672` | K7 |
| Dedup `Command.handle` | `chamados/management/commands/dedup_perguntas_fato.py:76 → :76` | Sobra |
| `par_do_intervalo` | `chamados/services/materializacao.py:192 → :193` | K21 no contador, K19 no plantio sob trava, K20 na validação. O fio leva a carga de `chamados/services/fio.py:262` |
| `paridade_absolvida_bug_par_103` | `chamados/services/materializacao.py:259 → :261` | K21 no contador. A devolução relê uma pergunta sob trava em `chamados/services/validacao.py:333` |
| `_situacao_da_irma` | `chamados/services/materializacao.py:290 → :293` | K22, `q` carregada avulsa em `:286` |
| `estado_aplicacao_pergunta` | `chamados/services/materializacao.py:885 → :889` | K21, K61 e carga do fio em `chamados/services/fio.py:262` |
| `_recusa_sem_anuencia` | `chamados/services/materializacao.py:1300 → :1305` | K19 no plantio sob trava. A porta de validação também entrega a instância por ID de `chamados/services/validacao.py:677` |
| Vigia `censo_alcance` | `chamados/services/vigia_fio.py:636 → :637` | K2 para `ps`, em `chamados/services/vigia_fio.py:560`, `:566`. K60 abastece `dono_fora_do_fio` em `:616` |

## Carregadores e arquivos tocados

As referências usam `app/` como raiz. Os 12 arquivos de produção são os desta tabela. Os sete arquivos de teste são o selo, os três módulos novos de lotes, sítios e mutantes, e os três harnesses listados nas adaptações. `chamados/tests/test_e4c_f5_selo_chamada.py:21`, `chamados/tests/test_e4c_f5_voltaC_lotes.py:73`, `chamados/tests/test_e4c_f5_voltaC_sitios.py:37`, `chamados/tests/e4c_f5_voltaC_mutantes.py:11`.

| K | Arquivo e linha base → C | Antes → depois |
|---:|---|---|
| 5 | `chamados/management/commands/auditar_invariantes_chamados.py:1586 → :1592` | Universo I21a → `decisoes` no queryset |
| 6 | `chamados/management/commands/auditar_invariantes_chamados.py:590 → :595` | Pendentes I2 → `decisoes` no queryset |
| 7 | `chamados/management/commands/auditar_invariantes_chamados.py:1642 → :1646` | Perguntas do destino → `perguntas_que_resolvem__decisoes` |
| 8 | `chamados/services/fio.py:2078 → :2081` | Sementes do grupo → `decisoes` no queryset |
| 12 | `chamados/management/commands/backfill_fio_morto_e4b.py:89 → :90` | Perguntas do plano do dia → `decisoes` no queryset |
| 13 | `chamados/management/commands/backfill_fio_morto_e4b.py:134 → :135` | Alvos da reabertura → `decisoes` no queryset e releitura em lote dos restantes após `reaberto` |
| 14 | `chamados/management/commands/backfill_fio_morto_e4b.py:166 → :178` | Varredura das exportadas → `decisoes` no queryset |
| 21 | `chamados/services/materializacao.py:324 → :328` | Universo U163 → `decisoes` no queryset |
| 28 | `chamados/management/commands/backfill_decisoes_pergunta.py:531 → :534` | `only()` do DRY → `decisoes` mais escalares do compositor, do passivo e da chave usada pelo plano |
| 33 | `chamados/management/commands/auditar_invariantes_chamados.py:1723 → :1726` | Perguntas F1t → `decisoes` no queryset reutilizado pela contraprova |
| 34 | `chamados/management/commands/auditar_invariantes_chamados.py:537 → :540` | Pares I6 → `decisoes` no queryset |
| 35 | `chamados/management/commands/auditar_invariantes_chamados.py:860 → :864` | Candidatas I8 → `decisoes` no queryset |
| 36 | `chamados/management/commands/auditar_invariantes_chamados.py:896 → :899` | Candidatas I8b → `decisoes` no queryset |
| 37 | `chamados/management/commands/auditar_invariantes_chamados.py:1781 → :1786` | Universo F2 → `decisoes` no queryset |
| 38 | `chamados/management/commands/auditar_invariantes_chamados.py:1794 → :1798` | Universo F3 → `decisoes` no queryset |
| 39 | `chamados/management/commands/disparar_perguntas_competencia.py:107 → :109` | Perguntas do contador de fio mudo → `perguntas_que_resolvem__decisoes` |
| 51 | `chamados/services/vigia_fio.py:711 → :714` | Vivas em fechadas → `decisoes` por bloco, `iterator(chunk_size=500)` em `:719` |
| 52 | `chamados/services/vigia_fio.py:770 → :774` | Carimbos sem lastro → `decisoes` por bloco, `iterator(chunk_size=500)` em `:779` |
| 56 | `chamados/management/commands/estoque_l130_instante_sem_texto.py:37 → :38` | Foto do estoque → `decisoes` no queryset |
| 57 | `chamados/management/commands/estoque_l130_instante_sem_texto.py:97 → :98` | Releitura sob trava → `decisoes` no queryset |
| 59 | `chamados/management/commands/arquivar_competencia_encerrada.py:158 → :158` | `d.perguntas.all()` → `d.perguntas.prefetch_related('decisoes')` |
| 60 | `chamados/services/vigia_fio.py:570 → :571` | Todas as perguntas das células → `decisoes` no queryset |
| 66 | `ponto/management/commands/emitir_furo_retroativo.py:83 → :83` | Perguntas dos chamados do colaborador → `perguntas_que_resolvem__decisoes` |
| 69 | `chamados/management/commands/fechar_becos_disputa.py:32 → :33` | Perguntas das disputas abertas → `perguntas__decisoes` |
| Sobra | `chamados/management/commands/dedup_perguntas_fato.py:75 → :75` | Perguntas do grupo → `decisoes` no queryset |

K28 carrega `validada_por` junto de `validada_em`, pois uma validação projetada sem ato correspondente lê o ID do autor. O RED confere os seis escalares da leitura antes de chamar a canônica. K8 carrega as sementes e continua usando o percurso de vizinhança de K18. O ganho no percurso integral de descoberta dos grupos está [nao verificado]. `chamados/models.py:4274`, `:4286`, `chamados/management/commands/backfill_decisoes_pergunta.py:531`, `chamados/tests/test_e4c_f5_voltaC_lotes.py:119`, `chamados/services/fio.py:1597`, `:2081`, `:2146`.

**R restantes, 6.** K3 relê a candidata sob trava, `chamados/services/relavra_via.py:230`, `:236`. K19 relê cada item do plantio, `chamados/services/materializacao.py:2121`, `:2122`. K20 e K58 entram por ID, `chamados/services/validacao.py:677`, `:431`. K22 procura a irmã de uma pergunta, `chamados/services/materializacao.py:286`. K40 faz `.get(pk=pk)` dentro do laço, `chamados/management/commands/migrar_data_evento_dia_do_turno.py:464`, `:466`.

## Chokepoint G

- A emissão abre uma pergunta e tenta reatá-la imediatamente na mesma iteração e savepoint. Há também uma entrada que chama a emissão com um único motivo. `chamados/services/disputa_emissao.py:294`, `:450`, `:468`, `:473`.
- A abertura retorna pergunta e indicador de criação. As linhas que a porta interna registra ficam fora do retorno. O outro caminho de reate já carrega K9 em lote. `chamados/models.py:2983`, `:3231`, `:3243`, `chamados/reconciliador.py:1575`.
- Deixei G na leitura avulsa. Nessa entrada, o lote exigiria adiar o reate para depois de outros nascimentos, alterando a ordem dos atos. O compositor continua sendo chamado pelo juiz. `chamados/services/disputa_emissao.py:320`, `:468`, `:473`, `chamados/reconciliador.py:367`.

| Cenário | Base do anexo / teto | Previsão C |
|---|---:|---|
| 1 fecha | 236 / 230 | Δ 0 [nao verificado]. `ponto/tests/test_selo_chokepoint_escrita.py:435` |
| 1 FICA | 146 / 145 | Δ 0 [nao verificado]. `ponto/tests/test_selo_chokepoint_escrita.py:470` |
| 3 fecham | 300 / 294 | Δ 0 [nao verificado]. `ponto/tests/test_selo_chokepoint_escrita.py:440` |
| 3 FICAM | 221 / 219 | Δ 0 [nao verificado]. `ponto/tests/test_selo_chokepoint_escrita.py:475` |

A previsão considera o caminho síncrono de fecho, que chega a `materializar_pendentes_aprovadas` pelo S144. Essa função usa a guarda do lastro e `carimbar`. O item D fica em `chamados/models.py:2077` para o Opus. `chamados/signals.py:150`, `:164`, `chamados/services/materializacao.py:2173`, `:2185`.

## REDs, guardas e comandos para o Code

O RED de lote confere os PKs que chegaram ao compositor e compara somente os SELECTs cujo primeiro `FROM` é `chamados_decisaopergunta`. Dedup e K28 injetam um grupo completo na fronteira do DRY e isolam a busca K18 de vizinhos. A emissão retroativa usa o `handle` real e o juiz real do fio, com grade de um dia e saída do classificador de dia controladas. K13 tem um caso próprio de frescor, com a porta real de reabertura gravando por PK dentro de savepoint desfeito. `chamados/tests/test_e4c_f5_opus_models.py:25`, `chamados/tests/test_e4c_f5_voltaC_lotes.py:32`, `:49`, `:74`, `:119`, `:198`, `:239`.

| Sítio | Caso em `chamados.tests.test_e4c_f5_voltaC_sitios.SitiosVoltaCTest` |
|---|---|
| I21a | `test_RED_i21a_reabertura_desmente_a_projecao`, `:65` |
| I2 | `test_RED_i2_resposta_do_ato_entra_no_piso`, `:72` |
| F1 | `test_RED_f1_resposta_do_ato_retira_o_chamado_do_piso_sem_dia`, `:78` |
| Dedup | `test_RED_dedup_nomeia_resposta_do_ato`, `:87` |
| Par | `test_RED_par_do_intervalo_reconhece_resposta_do_ato`, `:93` |
| BUG-PAR-103 | `test_RED_bug_par_103_reconhece_resposta_do_ato`, `:99` |
| Irmã | `test_RED_situacao_da_irma_le_instante_do_ato`, `:110` |
| Aplicação | `test_RED_estado_aplicacao_le_instante_do_ato`, `:117` |
| Anuência | `test_RED_anuencia_recusa_par_antigo_depois_de_reabrir`, `:125` |
| Vigia | `test_RED_vigia_alcance_resposta_do_ato_sob_terminal`, `:135` |
| **GUARDA** | `test_GUARDA_ato_com_texto_e_coluna_vazia_nao_da_anuencia_ao_par`, `:145`. A base já recusava o texto vazio da projeção, `chamados/services/materializacao.py:192` na base e `chamados/juizes.py:1556` |

A partir de `app/`, o Code pode usar os mesmos labels dos REDs na cópia de medição da base e na C. Os módulos de teste estão em `chamados/tests/test_e4c_f5_voltaC_lotes.py:73`, `chamados/tests/test_e4c_f5_voltaC_sitios.py:24`, `:37`.

```bash
lotes=chamados.tests.test_e4c_f5_voltaC_lotes.CarregadoresVoltaCTest
sitios=chamados.tests.test_e4c_f5_voltaC_sitios
choke=ponto.tests.test_selo_chokepoint_escrita.SeloChokepointEscritaTest

PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  "$lotes" "$sitios.SitiosVoltaCTest" \
  chamados.tests.test_e4c_f5_selo_chamada \
  chamados.tests.test_bug_par_c2_sem_banco \
  chamados.tests.test_bug_par_sem_banco \
  chamados.tests.test_estado_aplicacao

PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  "$choke.test_registrar_batida_com_1_furo_vivo_que_ela_fecha" \
  "$choke.test_registrar_batida_com_1_furo_vivo_que_fica" \
  "$choke.test_registrar_batida_com_3_furos_vivos_que_ela_fecha" \
  "$choke.test_registrar_batida_com_3_furos_vivos_que_ficam" \
  chamados.tests.test_selo_teto_modal
```

O seletor tem **exatamente duas** ocorrências de `os.environ.get(SELETOR)`. O efeito previsto para cada mutante de lote é `Q(6) > Q(3)` na igualdade. A mordida ORM de cada um está [nao verificado]. A transformação de `par_p_cru` foi conferida por AST e acusa somente `par_do_intervalo` em `chamados/services/materializacao.py:193`. `chamados/tests/e4c_f5_voltaC_mutantes.py:27`, `:63`, `chamados/tests/test_e4c_f5_voltaC_lotes.py:85`.

```bash
mut_lote() {
  F5_VOLTA_C_MUTANTE="$1" PYTHONDONTWRITEBYTECODE=1 \
    python3 manage.py test "$lotes.$2"
}
```

| Mutante → caso que deve cair | Comando |
|---|---|
| K8, universo `:89` | `mut_lote k8 test_RED_k8_universo_do_grupo` |
| K21, contador `:100` | `mut_lote k21 test_RED_k21_contador_bug_par` |
| K28, DRY `:119` | `mut_lote k28 test_RED_k28_dry_backfill_decisoes` |
| K34, auditor I6 `:141` | `mut_lote k34 test_RED_k34_auditor_i6` |
| K51, vigia de fechadas `:154` | `mut_lote k51 test_RED_k51_vivas_em_fechada` |
| K52, vigia de carimbos `:167` | `mut_lote k52 test_RED_k52_carimbos_por_dono` |
| K60, censo `:181` | `mut_lote k60 test_RED_k60_vigia_alcance` |
| K66, emissão retroativa `:198` | `mut_lote k66 test_RED_k66_emissao_retroativa` |
| K69, becos `:218` | `mut_lote k69 test_RED_k69_fechar_becos` |
| Sobra, dedup `:230` | `mut_lote sobra test_RED_sobra_dedup_por_grupo` |
| `par_p_cru`, catraca em `chamados/tests/test_e4c_f5_voltaC_sitios.py:25` | `F5_VOLTA_C_MUTANTE=par_p_cru PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$sitios.SeloMutanteVoltaCTest.test_MORDE_par_do_intervalo_com_pergunta_crua"` |

As linhas de K na tabela de mutantes referem-se a `chamados/tests/test_e4c_f5_voltaC_lotes.py`.

## Adaptações e tetos

| Arquivo | Classe | Antes → depois | Asserts |
|---|---|---|---:|
| `chamados/tests/test_bug_par_sem_banco.py:170`, `:193` | ii | `NS` de pergunta e módulo sem compositor → `pergunta` e `DecisaoPergunta` do helper existente | 16 → 16 |
| `chamados/tests/test_bug_par_c2_sem_banco.py:80`, `:82` | ii | `NS` de pergunta e módulo sem compositor → mesmos helpers | 15 → 15 |
| `chamados/tests/test_bug_par_c2_sem_banco.py:32` | v | `_Recorte(list)` com métodos próprios → `_Recorte(Linhas)` com `prefetch_related` identidade e o mesmo filtro de disputa | incluídos nos 15 → 15 |
| `chamados/tests/test_estado_aplicacao.py:41` | ii | `NS` sem os insumos de `leitura` → `pergunta` do helper existente, com os mesmos valores do caso | 5 → 5 |
| `chamados/tests/test_e4c_f5_selo_chamada.py:21`, `:189` | Catraca solicitada e i na âncora sintética | Dez identidades → conjunto vazio. A chamada de ordem 1 da fixture usa a composição real da C, preservando a acusação da ordem 2 | 12 → 12 |

O import de `DIA` referido no anexo já existe em `d9319a215`. `chamados/tests/test_e4c_f5_voltaB_sitios.py:17`.

**Fora das classes**, a previsão é persistência dos quatro FAILs de PASSAGEM pelos excessos acima. Outras quebras estão [nao verificado]. **Tetos que prevejo cair:** nenhum dos existentes foi identificado. No modal, os sítios convertidos recebem perguntas com `decisoes` já carregadas. O valor medido na C está [nao verificado]. `ponto/tests/test_selo_chokepoint_escrita.py:435`, `:440`, `:470`, `:475`, `chamados/services/fio.py:262`, `:268`, `:360`, `:381`, `chamados/tests/test_selo_teto_modal.py:184`.

## Campos, escritores e releituras

| Campo | Escrita e significado para os leitores da C |
|---|---|
| `validada_em` | A validação e as devoluções gravam a projeção e o ato. Nova resposta e reabertura podem limpar a validação. O compositor recebe validação vigente da sequência com passivo. As guardas escalares da irmã e do plantio continuam usando o carimbo projetado, que pode divergir da sequência. `chamados/services/validacao.py:168`, `:173`, `:273`, `:279`, `:349`, `:354`, `chamados/services/disputa_emissao.py:1494`, `:1615`, `chamados/models.py:4335`, `:4358`, `chamados/services/materializacao.py:200`, `:2128` |
| `respondida_em` | Resposta humana, automática e pela trilha gravam instante e ato. Reabertura limpa o par. Carimbo pode completar o instante projetado. Nos dez chamadores, o juiz recebe o instante vigente do ato. As faces SQL da auditoria e a guarda de integridade do vigia ainda leem a coluna projetada, que pode divergir. `chamados/services/disputa_emissao.py:1482`, `:1532`, `:1615`, `:1683`, `:1686`, `:1705`, `:1708`, `chamados/models.py:3349`, `:4358`, `chamados/juizes.py:1538`, `:1583`, `chamados/management/commands/auditar_invariantes_chamados.py:1587`, `chamados/services/vigia_fio.py:635` |
| `resposta_colab` | Resposta humana e automática gravam o texto atual. Reabertura e ressuscitação o limpam. O compositor, a derivação da hora e as guardas de projeção usam esse mesmo texto corrente. `chamados/services/disputa_emissao.py:1481`, `:1615`, `:1682`, `chamados/models.py:1722`, `:4358`, `chamados/services/materializacao.py:640`, `chamados/juizes.py:1538` |
| `materializada_em` | `_escrever_carimbo` abastece as portas de carimbo e identidade. A porta de lote limpa os campos do carimbo. Os leitores usam a mesma coluna como carimbo gravado. O C6 ainda exige os demais insumos para julgar vitalidade. `chamados/models.py:3314`, `:3346`, `:3401`, `:3508`, `:4359`, `chamados/services/materializacao.py:262`, `:882`, `chamados/juizes.py:1446` |
| `via_resolucao` | As mesmas portas gravam a via junto do carimbo. O compositor lê a coluna, e o BUG-PAR e a testemunha de aplicação leem o mesmo motivo gravado. O temporal recorta carimbos futuros de acordo com a via. `chamados/models.py:3318`, `:3346`, `:3401`, `:3508`, `:4359`, `chamados/services/materializacao.py:263`, `:882`, `chamados/juizes.py:1398` |
| `decisoes` | `registrar` insere pela FK. Resposta, validação, reabertura, absorção, ressuscitação e backfill chamam essa porta. `leitura` consome a relação e incorpora o passivo, e `composta` fornece a entrada dos juízes. O rótulo BUG-PAR lê a trilha de devolução para exibição. `chamados/models.py:4188`, `:1713`, `:4329`, `:4362`, `chamados/services/disputa_emissao.py:1532`, `:1639`, `chamados/services/validacao.py:173`, `chamados/services/materializacao.py:277`, `:1065`, `chamados/management/commands/backfill_decisoes_pergunta.py:351` |

**Materialização no mesmo fluxo**

- **Par.** O juízo lê antes de plantar a irmã. Na validação, o ato anterior entra por `pergunta=perg`, invalidando o cache dessa instância. Depois de sucesso ou rollback do par a porta faz `refresh_from_db()` integral. No plantio, a releitura imediata depois da irmã é da vizinhança de batidas, e a próxima metade é buscada por PK sob trava. `chamados/services/validacao.py:173`, `:632`, `:646`, `:655`, `:743`, `:771`, `chamados/models.py:4376`, `chamados/services/materializacao.py:1409`, `:1416`, `:1737`, `:2122`.
- **BUG-PAR-103.** O contador é leitura. A devolução busca `perg` sob trava, julga o estado anterior, grava descarimbo e devolução e monta a trilha a partir da foto anterior e dos campos atualizados. O registro recebe `pergunta=perg` e invalida o prefetch dessa instância. `chamados/services/materializacao.py:328`, `:332`, `chamados/services/validacao.py:332`, `:333`, `:334`, `:345`, `:354`, `:362`, `chamados/models.py:4376`.
- **Irmã e aplicação.** `_situacao_da_irma` consulta `q` avulsa. `estado_aplicacao_pergunta` lê a testemunha, e a via de recusa tem precedência. A porta de escrita chama o derivador antes do plantio. Esses caminhos não servem cache de decisão anterior a uma escrita do próprio percurso. `chamados/services/materializacao.py:286`, `:293`, `:882`, `:893`, `:896`, `:1930`.
- **Anuência.** Os três ramos consultam antes de escrever. Na recusa, a releitura em `refresh_from_db(fields=...)` é dos escalares do carimbo e da recusa. O ramo retorna sem recompor a pergunta. No caminho que aplica, o carimbo e a cauda do fecho vêm depois, e as vivas são buscadas novamente. `chamados/services/materializacao.py:1305`, `:1360`, `:1595`, `:1679`, `:1694`, `:1986`, `:2029`, `chamados/juizes.py:1512`.

**Outras releituras com lote.** Em K13, a ressuscitação pode registrar por PK. Depois de `reaberto`, o comando refaz a carga dos restantes e conserva a ordem inicial. K57 carrega novamente sob trava, e a porta K58 ainda relê por ID. Em K69, o fecho atualiza a disputa com `refresh_from_db()` antes de perguntar se pode fechar. `chamados/models.py:1706`, `:1713`, `chamados/management/commands/backfill_fio_mort

[truncado em 20000 de 20416 caracteres]

## 20/11 00:32 · sessao b9ffd9798ead · CADEIA-20-PASSO-1 · leitura · origem=Code

**1. Quem escolhe a grade**

A escolha é a mesma para todos os chamadores. `_julgar_colab_corpo` chama `montar_grade_prevista_periodo`, que usa **POR TURNO** se `settings.GRADE_POR_TURNO` for verdadeiro e **LEGADA** se for falso ou ausente. A configuração base vem do ambiente e tem padrão `True`. A POR TURNO devolve `insumos_razao_sobras` com turnos, avulsas e instantes. A LEGADA devolve `marcos` e `dias`. `app/ponto/services/cartorio.py:1317`, `app/escala/utils.py:2131`, `app/config/settings/base.py:165`, `app/escala/utils.py:2117`, `app/escala/utils.py:1065`.

- **Dois chamadores diretos de `_lavrar_razoes_sobras`**. O ramo `nunca_bateu` entra quando `adesao is None` e grava com `apply_`. O ramo normal atende quem tem adesão e também grava com `apply_`. Ambos passam `g.get('insumos_razao_sobras')` imediatamente antes de `lavrar_veredito(ata=...)`. A data, o teto do dia e a versão da regra não participam do seletor da grade. `app/ponto/services/cartorio.py:1353`, `app/ponto/services/cartorio.py:1410`, `app/ponto/services/cartorio.py:1434`, `app/ponto/services/cartorio.py:1461`, `app/ponto/services/cartorio.py:2012`, `app/escala/utils.py:2131`.
- **Três chamadas ao corpo**. `julgar_colab` chama o corpo na entrada, repete com `forcar=True` se houver células na fila `rejulgar` e faz a releitura sem forçar se houve emissão nova. As três passam pelo mesmo seletor. `app/ponto/services/cartorio.py:1123`, `app/ponto/services/cartorio.py:1152`, `app/ponto/services/cartorio.py:1209`, `app/ponto/services/cartorio.py:1222`, `app/ponto/services/cartorio.py:1317`.
- **Entradas de `julgar_colab`**. `processar_cartorio`, `julgar_celula`, `rejulgar_furos_do_vinculo`, `celula_veredito_velho` e o script `rejulgar_o118_col221`. `lavrar_status_competencia` também entra, com `completar_status=True`, e escreve só status. `app/ponto/management/commands/processar_cartorio.py:69`, `app/ponto/services/cartorio.py:2203`, `app/escala/regua_defesa.py:695`, `app/ponto/management/commands/celula_veredito_velho.py:144`, `bin/rejulgar_o118_col221.py:24`, `app/ponto/management/commands/lavrar_status_competencia.py:56`, `app/ponto/services/cartorio.py:942`.
- **Entradas por `julgar_celula`**. Vigia de hora, sinais de batida, ausência, justificativa e chamado, reconciliador de chamados, reescrita da célula, pós-vínculo, `cobrar_dia`, `furos_sem_cobranca`, `rejulgar_ausencia_em_folga` e `curar_residuo_bug115`. Todos chegam a `julgar_colab` pela mesma chamada. `app/ponto/services/vigia_de_hora.py:134`, `app/ponto/signals.py:93`, `app/ponto/signals.py:131`, `app/ponto/signals.py:161`, `app/chamados/signals.py:218`, `app/chamados/reconciliador.py:752`, `app/ponto/portas/celula.py:472`, `app/colaboradores/services/pos_vinculo.py:73`, `app/chamados/services/cobrar_dia.py:41`, `app/ponto/services/furos_sem_cobranca.py:128`, `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:80`, `app/ponto/management/commands/curar_residuo_bug115.py:146`, `app/ponto/services/cartorio.py:2203`.

**2. Célula que nasce no próprio dia**

`gerar_celulas` cria a célula com DNA e `ata` nula. Ainda no próprio dia, o vigia de hora pode lavrá-la quando ela passa pelos filtros e tem marco vencido, usando `teto_de_hoje=True`. Uma chamada avulsa também pode lavrá-la no dia se `trabalha is False` ou se já passou o fim do turno. O lote `processar_cartorio` só a alcança quando `data < hoje`. Nessa rodada, a impressão em dia pode fazê-lo pular a célula, salvo `--forcar`. `app/ponto/management/commands/gerar_celulas.py:349`, `app/ponto/portas/celula.py:913`, `app/escala/models.py:1778`, `app/ponto/services/vigia_de_hora.py:117`, `app/ponto/services/cartorio.py:2196`, `app/ponto/management/commands/processar_cartorio.py:65`, `app/ponto/services/cartorio.py:1402`, `app/ponto/services/cartorio.py:1552`.

O chamador efetivo de cada um dos dois passes das 19 células e o valor da flag em cada passe ficam **[nao verificado]**. A troca de passada interna, por si só, usa o mesmo seletor. `app/ponto/services/cartorio.py:1123`, `app/ponto/services/cartorio.py:1156`, `app/ponto/services/cartorio.py:1222`, `app/ponto/services/cartorio.py:1317`.

**3. Quem grava a ata e quem lê a ausência**

- `ata_do_dia` monta a ata-base com `orfas` e `n_orfas`. `lavrar_veredito` grava o dicionário recebido, preservando da ata anterior apenas a chave `precedencia` quando ela falta na nova. Assim, uma entrada sem `razoes_orfas` pode substituir uma ata que tinha a chave, mesmo com veredito igual. `carimbar_precedencia`, chamado depois do julgamento avulso, copia a ata atual e acrescenta `precedencia`. Se a ata era nula, pode gravar um dicionário contendo só essa chave. `app/ponto/services/cartorio.py:752`, `app/ponto/portas/celula.py:106`, `app/ponto/portas/celula.py:1049`, `app/ponto/portas/celula.py:1076`, `app/ponto/portas/celula.py:1090`, `app/ponto/services/cartorio.py:2209`, `app/ponto/portas/celula.py:1205`.
- `sobras_da_ata`, usado pela grade lavrada e pelo espelho, devolve `[]` quando não há lista de `orfas` a projetar. Se há órfãs, chave ausente e lista de razões vazia resultam em **“razao nao lavrada”** por ocorrência, com o sufixo **“anterior ao horizonte”** abaixo da v5. A contagem de órfãs vem de `orfas` ou de `n_orfas`. `app/escala/services/leitor_celula.py:180`, `app/escala/services/leitor_celula.py:193`, `app/escala/services/leitor_celula.py:454`, `app/ponto/services/espelho.py:708`.
- `medir_razoes_sobras` **distingue as formas quando há órfãs**. Chave ausente conta em `orfas_sem_chave`. Chave presente com `[]` e órfãs na ata conta como desalinhamento. `atas_no_horizonte_sem_razao` pula atas sem órfãs pela lista e pela contagem, e acusa ambas as formas quando há órfãs e a versão é pelo menos 5. `app/ponto/services/cartorio.py:1004`, `app/ponto/services/cartorio.py:1038`.

**4. Escritor único da chave**

O único ponto que atribui `razoes_orfas` é **`_lavrar_razoes_sobras`**. Ali estão os três resultados atuais: omissão quando faltam insumos, `[]` quando há insumos e zero órfãs, e uma lista de respostas quando há órfãs a classificar. Para uma ata com `orfas=[]` e `n_orfas=0`, os leitores acima produzem o mesmo resultado com a chave ausente ou com `[]`. Com órfãs na lista, a troca muda o significado para `medir_razoes_sobras`. `app/ponto/services/cartorio.py:962`, `app/ponto/services/cartorio.py:1001`, `app/ponto/services/cartorio.py:1010`, `app/ponto/services/cartorio.py:1042`, `app/escala/services/leitor_celula.py:198`.

## 20/11 00:29 · sessao e8d689a8df3e · CADEIA-20-PASSO-1 · verificador · origem=Code

**CADEIA-20-PASSO-1, item (3) — BLOQUEIA.** O raspado informa mudança de ata na segunda passada com o insumo capturado igual em (A). Em (B), a mudança de hash acompanha mudanças nos insumos que o código manda hashear. O bloqueio de (B) é de ordem da cadeia: o cartório pode carimbar antes de a própria rodada terminar de alterar esses insumos. A meta de R5 é diferença zero na segunda rodada. `app/core/placar_estrutural.py:416–420`, `app/ponto/services/cartorio.py:502–507`, `app/ponto/services/cartorio.py:543–547`, `app/ponto/services/cartorio.py:1133–1157`, `app/ponto/services/cartorio.py:1179–1181`.

Identidade Git da cópia com `a4a9b6bfe5f7` [nao verificado]. `git rev-parse HEAD` retornou que o diretório fornecido não é um repositório Git.

### 1. (A) — BLOQUEIA

A primeira lavra e a relavra passam pelo mesmo corpo do cartório. Tanto o ramo normal quanto `nunca_bateu` montam a ata e chamam `_lavrar_razoes_sobras`. Com ata, zero órfãs e insumos da grade presentes, o escritor põe `[]`. Com insumos `None`, retorna sem incluir a chave na ata recém-montada. A grade por turno entrega os insumos, e a grade legada omite essa chave. O escritor do campo nunca atribui `None` a `razoes_orfas`. `app/ponto/services/cartorio.py:752–794`, `app/ponto/services/cartorio.py:962–971`, `app/ponto/services/cartorio.py:994–1001`, `app/ponto/services/cartorio.py:1123`, `app/ponto/services/cartorio.py:1156–1157`, `app/ponto/services/cartorio.py:1393`, `app/ponto/services/cartorio.py:1434–1459`, `app/ponto/services/cartorio.py:2013–2037`, `app/escala/utils.py:1065`, `app/escala/utils.py:2117–2137`.

A porta `lavrar_veredito` compara a ata inteira e atribui `cel.ata` quando ela difere. `carimbar` persiste os campos devolvidos pela lavra. A entrada avulsa `julgar_celula` ainda chama `carimbar_precedencia`, que copia a ata e altera a chave `precedencia`. A limpeza de julgamento de célula futura zera a ata inteira pela própria porta. O caminho efetivo da primeira gravação nas 19 células [nao verificado]. `app/ponto/portas/celula.py:1049–1067`, `app/ponto/portas/celula.py:1090–1098`, `app/ponto/portas/celula.py:1146–1167`, `app/ponto/portas/celula.py:1189–1216`, `app/ponto/services/cartorio.py:2202–2211`.

| Leitor direto | Ausente, `None` e `[]` |
|---|---|
| `sobras_da_ata` | Sem órfãs, devolve `[]` antes de ler a chave. Com órfãs, normaliza ausência e `None` para `[]`. É a projeção usada pela grade e pelo espelho. `app/escala/services/leitor_celula.py:193–223`, `app/escala/services/leitor_celula.py:450–454`, `app/ponto/services/espelho.py:708–710` |
| `medir_razoes_sobras` | Chave ausente tem ramo próprio. `[]` é uma lista válida para `len`; `None` explícito cai em `len(None)` e levanta `TypeError`. Com zero órfãs, chave ausente e `[]` dão a mesma contagem. `app/ponto/services/cartorio.py:1004–1035` |
| `atas_no_horizonte_sem_razao` | Sem órfãs nem contagem positiva, pula a ata antes de ler a chave. Com órfãs no horizonte, ausência e `None` falham no teste de tipo, e `[]` falha no alinhamento. `app/ponto/services/cartorio.py:1038–1056` |

A comparação da ata inteira também distingue os três estados na escrita. `app/ponto/portas/celula.py:1049–1059`.

### 2. (B) — BLOQUEIA pela ordem

**Sim, a impressão inclui insumo que a própria cadeia pode alterar adiante.** Ela inclui `pk:status_local` dos chamados do dia. `fio` conta **chamados vivos com pelo menos uma pergunta viva**. Com chamados vivos e nenhum com fio, entra `fio:0`. Sem chamados vivos, a parte `fio` é omitida. O juiz da pergunta também lê a célula, portanto a própria lavra pode mudar o resultado da próxima leitura do fio. `app/ponto/services/cartorio.py:146–155`, `app/ponto/services/cartorio.py:502–507`, `app/ponto/services/cartorio.py:543–547`, `app/ponto/services/cartorio.py:1527–1547`, `app/chamados/juizes.py:1450–1469`, `app/chamados/juizes.py:1510–1519`.

1. **Dentro do cartório**, o corpo calcula `imp` e depois o carimba. Há fechamentos e ressuscitações entre esses dois pontos. Depois do primeiro carimbo, o E2 carimba perguntas absolvidas e o cartório tenta fechar os chamados adiados. Os casos enfileirados voltam ao mesmo corpo. Depois, a fábrica pode criar perguntas ou desfazer carimbos. A releitura seguinte exige aumento de `emitidos`, e as filas produzidas nas releituras têm caminhos de descarte. `app/ponto/services/cartorio.py:1541–1547`, `app/ponto/services/cartorio.py:1691–1704`, `app/ponto/services/cartorio.py:1835–1861`, `app/ponto/services/cartorio.py:2035–2037`, `app/ponto/services/cartorio.py:1123–1164`, `app/ponto/services/cartorio.py:1179–1181`, `app/ponto/services/cartorio.py:1209–1231`, `app/chamados/reconciliador.py:634–648`, `app/chamados/services/fabrica_da_celula.py:160–174`, `app/chamados/services/fabrica_da_celula.py:206–219`.
2. **Na ordem configurada**, `reconciliar_grade` precede `processar_cartorio --apply`. Depois do cartório, um ramo segue por `apurar_furos_diarios` → `disparar_perguntas_competencia --alarme` → `reconciliar_perguntas_orfas --alarme`. `apurar_furos_diarios` chama o agrupamento sob limbo, que pode reatar perguntas e reconciliar o furo. `supra_juiz --executar` também depende do cartório e pode chamar o retratador. A transição `em_analise → resolvido` vem de `RECONCILIAR` e é persistida por `_gravar_estado`. `app/config/crons.py:462`, `app/config/crons.py:599`, `app/config/crons.py:616`, `app/config/crons.py:1527–1545`, `app/escala/management/commands/apurar_furos_diarios.py:13–19`, `app/chamados/services/limbo_folgas.py:279–287`, `app/ponto/management/commands/supra_juiz.py:257–287`, `app/chamados/reconciliador.py:572–586`, `app/chamados/catalogo/motor.py:109–113`, `app/chamados/models.py:234–235`, `app/chamados/models.py:302`, `app/chamados/models.py:654–668`.
3. **Pelo evento da célula**, a porta salva e publica no mesmo ato. O consumidor tem agenda própria e pode carimbar perguntas, retratar chamados e rodar a fábrica. O `post_save` do chamado tenta rejulgar quando está fora de um julgamento ativo e tem dia identificável. O comando, evento ou signal que produziu cada uma das oito transições, e a ordem efetiva da sonda, ficam [nao verificado]. `app/ponto/portas/celula.py:942–965`, `app/config/crons.py:393–394`, `app/chamados/reconciliador.py:1414–1438`, `app/chamados/signals.py:189–218`.

### 3. Terceiro passe — BLOQUEIA o aceite de idempotência

- **(A)** Com zero órfãs e a mesma grade por turno, uma nova lavra volta a produzir `[]`. Com a mesma impressão, o caminho comum pula a célula. Logo, o próprio caso `ausente/None → []` não se repete sob essas condições. O resultado efetivo do terceiro passe [nao verificado]. `app/ponto/services/cartorio.py:964–971`, `app/ponto/services/cartorio.py:1402–1408`, `app/ponto/services/cartorio.py:1552–1558`.
- **(B)** Se o status e o fio lidos no terceiro passe já estiverem refletidos no hash do segundo e os demais insumos se mantiverem, o caminho comum pula. Se o segundo passe tiver deixado uma mudança posterior ao último carimbo, o terceiro pode gravar outro hash com a mesma ata. O código não garante convergência em três passes, e o raspado só vai até o segundo [nao verificado]. `app/ponto/services/cartorio.py:1209–1231`, `app/ponto/services/cartorio.py:1541–1558`, `app/ponto/portas/celula.py:1189–1201`.

### 4. Classes de reprovação

| Classe | Veredito no caminho lido |
|---|---|
| Segundo juiz registrado | **NAO BLOQUEIA.** O registro aponta `razao_da_sobra` e `fio_vivo`, e o cartório chama essas autoridades. `app/core/juizes.py:290–299`, `app/core/juizes.py:1332–1333`, `app/ponto/services/cartorio.py:994–996`, `app/ponto/services/cartorio.py:1466`, `app/ponto/services/cartorio.py:1532` |
| Segundo escritor fora da porta | **NAO BLOQUEIA.** As duas lavras de razões convergem para o mesmo escritor e persistem pela porta da célula. `app/ponto/services/cartorio.py:1434–1436`, `app/ponto/services/cartorio.py:2013–2016`, `app/escala/models.py:1826–1828`, `app/ponto/portas/celula.py:1090–1098` |
| Relógio fora da borda | **BLOQUEIA.** O cartório tem `agora`, mas a leitura de `fio` chama `fio_vivo` sem esse instante. O juiz pode cair em `date.today()` e em `classificar_falta` com fallback para `_tz.now()`. Efeito nas oito células [nao verificado]. `app/ponto/services/cartorio.py:1122`, `app/ponto/services/cartorio.py:1532`, `app/chamados/juizes.py:588–596`, `app/chamados/juizes.py:1470–1477`, `app/chamados/juizes.py:1519`, `app/escala/servico_jornada.py:385–389` |
| Derivação paralela | **BLOQUEIA na contagem do fio.** O cartório compartilha `_com_fio` entre hash e julgamento, mas o comando `supra_juiz` recalcula a agregação para o placar. Ambos consultam o mesmo predicado por chamado. Divergência de contagem no raspado [nao verificado]. `CLAUDE.md:20–22`, `app/ponto/services/cartorio.py:1527–1545`, `app/ponto/services/cartorio.py:1614–1617`, `app/ponto/management/commands/supra_juiz.py:211–225` |
| Campo com dois significados | **BLOQUEIA em (A).** A ausência da chave e a lista vazia são distinguíveis na persistência, e o contador ainda trata `None` explícito de forma diferente. `app/ponto/services/cartorio.py:964–971`, `app/ponto/services/cartorio.py:1013–1022`, `app/ponto/portas/celula.py:1049–1059` |
| Ordem de etapa | **BLOQUEIA em (B).** Há escrita posterior ao carimbo e a releitura é condicional. `app/ponto/services/cartorio.py:1123–1157`, `app/ponto/services/cartorio.py:1179–1181`, `app/ponto/services/cartorio.py:1209–1231` |

## 20/11 00:27 · sessao 670009206e83 · E4C-F5-CONSTROI · construtor · origem=Code

Apliquei as duas adaptações pedidas e atualizei quatro âncoras de mutação deslocadas pela B/B2. A B2 deixa uma pendência no beco: o módulo falso de `chamados.juizes` ainda precisa expor `pergunta_respondida`. Esse import vem antes de `DecisaoPergunta` e está fora das classes aprovadas para esta volta. `chamados/tests/test_cef_c1_sem_banco.py:88`, `chamados/tests/e3_volta7_apoio.py:106`, `escala/services/cadastro_realidade.py:439`.

### Arquivos tocados e usos dos avais

| Arquivo e linha | Classe | Método ou alvo | Antes → depois |
|---|---|---|---|
| `chamados/tests/test_cef_c1_sem_banco.py:88` | v | `Consulta.prefetch_related` | Ausente → devolve `self`, sem ramo. |
| `chamados/tests/test_cef_c1_sem_banco.py:309` | i | `upload_arranca_motivo` | Âncora do `fio_vivo` sobre `filter(...)` → âncora sobre `filter(...).prefetch_related('decisoes')`. A mutação continua trocando a condição por `if True:`. |
| `chamados/tests/e3_volta7_apoio.py:106` | ii | `cenario`, módulo falso `chamados.models` | `DecisaoPergunta` ausente → exporta a classe do helper único `chamados/tests/duble_pergunta.py:12`, já usada em `chamados/tests/test_e3_r3_c5_sem_banco.py:180` e `chamados/tests/test_e3_r3_c6_sem_banco.py:299`. |
| `chamados/tests/e3_r3_c6_mutantes.py:48` | i | `K6_m1_sem_respondida` | Âncora no teste direto do instante → `a is None and respondida_no_horizonte(p)`. A mutação continua removendo o requisito de resposta. |
| `chamados/tests/e3_r3_c6_mutantes.py:50` | i | `K6_m1_ignora_ancora` | Âncora no teste direto do instante → `a is None and respondida_no_horizonte(p)`. A mutação continua removendo o requisito de ausência de âncora. |
| `chamados/tests/test_e4c_f6f7_c7_sem_banco.py:113` | i | `test_reflexao_e_mutacao_de_perguntas_MORDEM` | `_item(p)` → `_item(p, DecisaoPergunta.composta(p))` na âncora. A mutação continua substituindo a lista publicada por `[]`. |

### Varredura por família

| Família | Resultado da leitura estática |
|---|---|
| **Foto e emissão** | O upload CEF C1 precisava de `prefetch_related`. Os casos CEF C3 reutilizam o mesmo dublê pelo `setUp` do C1. A resolução S128 em memória já usa `Linhas`, que tem o método. `api/foto_recebida.py:81`, `chamados/tests/test_cef_c3_sem_banco.py:143`, `chamados/tests/test_cef_c3_sem_banco.py:171`, `chamados/tests/test_e4b_classe_e_memoria.py:193`, `chamados/tests/test_e4b_classe_e_memoria.py:374`. |
| **Beco C11 e M1** | C5 e C6 já fornecem `DecisaoPergunta.composta` e a projeção temporal. O retorno do helper é um `namedtuple` com `_replace`. A volta 7 já fornecia a projeção e agora exporta o compositor. A lista de imports pendentes está abaixo. `chamados/tests/duble_pergunta.py:7`, `chamados/tests/duble_pergunta.py:23`, `chamados/tests/test_e3_r3_c5_sem_banco.py:161`, `chamados/tests/test_e3_r3_c6_sem_banco.py:290`, `chamados/tests/e3_volta7_apoio.py:74`, `escala/services/cadastro_realidade.py:497`. |
| **Juízes e calendário** | S127 já recebe `Linhas` em `.all()`. No calendário inteiro de LU3, `Consulta.__getattr__` já devolve a própria consulta para `prefetch_related`. O C11 de navegação executa o trecho anterior à consulta. `chamados/juizes.py:2490`, `chamados/tests/test_disputa_s127.py:22`, `chamados/tests/e3_lu3_apoio.py:17`, `chamados/tests/e3_lu3_apoio.py:93`, `chamados/tests/test_e3_r3_c11_sem_banco.py:64`, `colaboradores/services/calendario.py:505`. |
| **API, views e serviços de chamados** | O selo sem banco da API tinha a âncora antiga. Nos demais sítios localizados, os selos leem fonte, dublam outra função ou chegam ao sítio alterado com ORM real. Isso inclui validação e recusa, declaração, detalhe e cobrança, classificação de becos, competência, explicador, migração do fio, limbo e pergunta no app. `api/views.py:2601`, `chamados/tests/test_bug_par_sem_banco.py:116`, `chamados/tests/test_bug_par_c2_sem_banco.py:97`, `chamados/tests/test_e1_volta2.py:141`, `chamados/tests/test_selo_resolucao_um_escritor.py:184`, `chamados/tests/test_e4b_c4v7_memoria.py:204`, `chamados/tests/test_tranca_vivas_08.py:123`, `chamados/tests/test_explicador_disputa.py:46`, `chamados/tests/test_e1_v7_catracas.py:467`, `chamados/tests/test_e1_v10.py:259`, `chamados/tests/test_e1_v6_horizonte.py:479`. |
| **Ficha, folha, dia do colaborador e comandos** | Os casos localizados para as consultas alteradas usam ORM real ou verificam fonte. O dublê do executor supra exerce `pode_executar_csf`; o `prefetch_related` alterado fica em `Command.handle`. `colaboradores/tests/test_ficha_praca_uf.py:8`, `folha/tests/test_previa_proposta.py:33`, `escala/tests/test_leitores_segundo_intervalo.py:246`, `chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:116`, `chamados/tests/test_porta_chamado_e2.py:58`, `ponto/tests/test_supra_juiz_executor.py:67`, `ponto/management/commands/supra_juiz.py:192`. |

**Fora das classes aprovadas.** Pela leitura estática, `pergunta_respondida` falta no juiz falso da volta 7, de C5, de C6 e do caso de porta só no app. O import falha antes de qualquer ramo do beco. Alterar o módulo falso do juiz exigiria ligar uma função de julgamento, além das adaptações de compositor e de ORM autorizadas. `escala/services/cadastro_realidade.py:439`, `chamados/tests/e3_volta7_apoio.py:66`, `chamados/tests/test_e3_r3_c5_sem_banco.py:174`, `chamados/tests/test_e3_r3_c6_sem_banco.py:294`, `chamados/tests/test_e3_degrau_dono_e_porta.py:507`, `chamados/tests/test_e3_tarefa_admin_sem_banco.py:79`.

### Selo de asserções

Comparei por AST com `a81f888dc`. A sequência e o conteúdo das asserções e os nomes dos testes são idênticos nos quatro módulos. `git diff --check` passou. Execução de testes e resultado dos mutantes **[nao verificado]**.

| Módulo | `assert` base → final | `self.assert*` base → final | Testes base → final | [nao verificado]
|---|---:|---:|---:|
| `chamados/tests/test_cef_c1_sem_banco.py:1` | 0 → 0 | 25 → 25 | 10 → 10 |
| `chamados/tests/e3_volta7_apoio.py:1` | 0 → 0 | 0 → 0 | 0 → 0 |
| `chamados/tests/e3_r3_c6_mutantes.py:1` | 0 → 0 | 0 → 0 | 0 → 0 |
| `chamados/tests/test_e4c_f6f7_c7_sem_banco.py:1` | 0 → 0 | 14 → 14 | 6 → 6 |

### Comandos para o Code

Executar a partir de `app/`. Os comandos abaixo incluem os consumidores dos módulos de apoio e os harnesses que chegam aos sítios alterados. Em B2, os casos do beco têm a pendência de import descrita acima. `chamados/tests/test_e3_volta7_sem_banco.py:52`, `chamados/tests/test_e3_r3_c5_sem_banco.py:194`, `chamados/tests/test_e3_r3_c6_sem_banco.py:313`.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest -v \
  chamados.tests.test_cef_c1_sem_banco \
  chamados.tests.test_cef_c3_sem_banco \
  chamados.tests.test_e4c_f6f7_c7_sem_banco \
  chamados.tests.test_e3_volta7_sem_banco \
  chamados.tests.test_e3_r3_c5_sem_banco \
  chamados.tests.test_e3_r3_c6_sem_banco \
  chamados.tests.test_e3_r3_c11_sem_banco \
  chamados.tests.test_e3_degrau_dono_e_porta \
  chamados.tests.test_e3_portas_v6_sem_banco \
  chamados.tests.test_e4b_classe_e_memoria

PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  chamados.tests.test_disputa_s127 \
  chamados.tests.test_e4b_c4v14_memoria \
  core.tests.test_medida_turno_do_par \
  ponto.tests.test_supra_juiz_executor
```

**Foto.** O primeiro comando remove o método em memória e reproduz o RED de protocolo esperado. O segundo usa o mutante comportamental: retirar o motivo enquanto a irmã continua viva deve falhar no `assertIn`. `api/foto_recebida.py:81`, `chamados/tests/test_cef_c1_sem_banco.py:236`, `chamados/tests/test_cef_c1_sem_banco.py:242`, `chamados/tests/test_cef_c1_sem_banco.py:309`.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -c '
import unittest
from chamados.tests import test_cef_c1_sem_banco as m
del m.Consulta.prefetch_related
unittest.main(module=m)
' UploadTest.test_T3_foto_tardia_da_primeira_so_extingue_primeira

CEF_C1_MUTANTE=upload_arranca_motivo PYTHONDONTWRITEBYTECODE=1 \
  python3 -m unittest -v \
  chamados.tests.test_cef_c1_sem_banco.UploadTest.test_T3_foto_tardia_da_primeira_so_extingue_primeira
```

**Beco.** Esta sonda usa em memória o corpo da B intermediária para isolar o RED anexado. Roda `test_beco` com o modelo falso atual e depois sem o export de `DecisaoPergunta`. O segundo caso deve dar `ImportError` nesse nome. A asserção de negócio continua no teste de origem. `chamados/tests/e3_volta7_apoio.py:106`, `chamados/tests/test_e3_volta7_sem_banco.py:52`, `escala/tests/test_f3_beco1_folga_na_lista.py:248`.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
import subprocess
import sys
import unittest
from contextlib import contextmanager
from unittest.mock import patch

from chamados.tests import test_e3_volta7_sem_banco as teste [nao verificado]
from chamados.tests.e4_catracas import FONTES_MUTADAS

arquivo = 'escala/services/cadastro_realidade.py'
fonte_b = subprocess.check_output(
    ['git', 'show', 'd0210a400:app/' + arquivo], text=True)
cenario_real = teste.cenario [nao verificado]

@contextmanager
def sem_compositor(caso):
    with cenario_real(caso) as contexto:
        del sys.modules['chamados.models'].DecisaoPergunta
        yield contexto

def rodar():
    caso = teste.SelosAnexadosTest('test_beco') [nao verificado]
    return unittest.TextTestRunner(verbosity=2).run(caso)

with patch.dict(FONTES_MUTADAS, {arquivo: fonte_b}):
    controle = rodar()
    with patch.object(teste, 'cenario', sem_compositor): [nao verificado]
        red = rodar()

raise SystemExit(0 if controle.wasSuccessful() and len(red.errors) == 1
                 and 'ImportError: cannot import name' in red.errors[0][1]
                 and 'DecisaoPergunta' in red.errors[0][1] else 1)
PY
```

**Âncoras C6 e API.** Os dois comandos C6 ficam bloqueados pelo import de `pergunta_respondida` no controle da B2; o próprio runner exige controle verde antes de aplicar cada mutante. O último comando executa o mutante interno da lista vazia da API. `chamados/tests/e3_r3_c6_mutantes.py:48`, `chamados/tests/e3_r3_c5_mutantes.py:76`, `chamados/tests/test_e4c_f6f7_c7_sem_banco.py:125`.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_r3_c6_mutantes K6_m1_sem_respondida
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_r3_c6_mutantes K6_m1_ignora_ancora
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest -v \
  chamados.tests.test_e4c_f6f7_c7_sem_banco.CosturasSemDjango.test_reflexao_e_mutacao_de_perguntas_MORDEM
```

`LEI-AKITA | padrao=n/a (instrumento) | classes=i,ii,v | execução e certificação [nao verificado]`

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

_7447 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
