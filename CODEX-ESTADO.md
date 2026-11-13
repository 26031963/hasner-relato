# CODEX-ESTADO

> Ultima resposta processada: **13/11/2026 11:25** · publicado em **13/11/2026 11:27** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 13/11 11:25 · sessao 038c6893fc6f · - · - · origem=Ronald (tmux)

Confirmei em memória, com `python3 -B` e sem escrita: o helper real retorna censo **43**, soma dos tetos **43**, `TOTAL_RELOGIO=43` e igualdade exata entre censo e tetos, sem diferenças (`app/ponto/tests/test_cadastro_futuro_c5e.py:334`, `:374`, `:574`, `:591`).

`marcar_trabalha_em_feriado` retorna zero leituras no scanner; a regeneração retorna os dois carimbos `now()` de `app/ponto/portas/celula.py:263` e `:308`, compatíveis com teto 2 (`app/ponto/tests/test_cadastro_futuro_c5e.py:369`).

Foi execução dos helpers AST, não execução da suíte Django [nao verificado]. [nao verificado]

## 13/11 11:25 · sessao 9e470830c035 · LEI-DESLIGADO-NAO-TRAVA · sonda · origem=Code

Fonte: saída fornecida na mensagem, sem referência arquivo:linha; uso o marcador exigido `[nao verificado]`.

(a) Casos solicitados — dias retornados por `motivos_retencao_celula` e resultado de `classificar_export`. [nao verificado]

| Colaborador | situacao | data_demissao | Competência | Dias | Motivos | Export: status/motivo | Referência |
|---|---|---|---|---:|---|---|---|
| col718 | desligado | 2026-11-05 | 10/2026 | 12 | `espelho_furo` | `fora/furo_espelho` | [nao verificado] |
| col718 | desligado | 2026-11-05 | 11/2026 | 8 | `espelho_furo` | `fora/rescisao_modulo_proprio` | [nao verificado] |
| col270 | desligado | 2026-10-30 | 10/2026 | 9 | `espelho_furo` | `fora/furo_espelho` | [nao verificado] |
| col270 | desligado | 2026-10-30 | 11/2026 | 3 | `espelho_furo` | `fora/rescisao_modulo_proprio` | [nao verificado] |
| col373 | desligado | 2026-10-31 | 10/2026 | 8 | `espelho_cobrado`, `espelho_furo` | `fora/furo_espelho` | [nao verificado] |
| col373 | desligado | 2026-10-31 | 11/2026 | 1 | `espelho_indefinida` | `fora/rescisao_modulo_proprio` | [nao verificado] |

(b) Frota — contagens exatamente como impressas. [nao verificado]

| Competência | Empresa | Desligados com célula | Retidos | Dias | Motivos e contagens | Export: status/motivo e contagens | Referência |
|---|---|---:|---:|---:|---|---|---|
| 10/2026 | emp2 | 77 | 42 | 228 | `espelho_furo`: 121; `espelho_fato_em_ausencia`: 2; `espelho_nunca_bateu`: 55; `espelho_indefinida`: 47; `espelho_cobrado`: 3 | `fora/furo_espelho`: 14; `fora/rescisao_modulo_proprio`: 28 | [nao verificado] |
| 10/2026 | emp3 | 19 | 10 | 40 | `espelho_furo`: 37; `espelho_cobrado`: 2; `espelho_indefinida`: 1 | `fora/rescisao_modulo_proprio`: 7; `fora/furo_espelho`: 3 | [nao verificado] |
| 10/2026 | emp4 | 2 | 1 | 6 | `espelho_furo`: 3; `espelho_fato_em_ausencia`: 1; `espelho_indefinida`: 2 | `fora/furo_espelho`: 1 | [nao verificado] |
| 11/2026 | emp2 | 40 | 13 | 88 | `espelho_furo`: 45; `espelho_nunca_bateu`: 26; `espelho_fato_em_ausencia`: 3; `espelho_indefinida`: 7; `espelho_cobrado`: 7 | `fora/rescisao_modulo_proprio`: 12; `fora/furo_espelho`: 1 | [nao verificado] |
| 11/2026 | emp3 | 15 | 6 | 21 | `espelho_furo`: 14; `espelho_cobrado`: 1; `espelho_indefinida`: 6 | `fora/rescisao_modulo_proprio`: 6 | [nao verificado] |
| 11/2026 | emp4 | 2 | 1 | 5 | `espelho_indefinida`: 1; `espelho_furo`: 4 | `fora/rescisao_modulo_proprio`: 1 | [nao verificado] |

(c) Vocabulário de `situacao`. [nao verificado]

| situacao | Quantidade | Referência |
|---|---:|---|
| `ativo` | 547 | [nao verificado] |
| `desligado` | 386 | [nao verificado] |
| `afastado` | 11 | [nao verificado] |

(d) A sonda informa **`ERROS 0`**. [nao verificado]

## 13/11 11:24 · sessao 0faef1817065 · - · - · origem=Ronald (tmux)

**Âncora/mutante curados.** A âncora de criação fora de `executar_vinculo` ocorre uma vez em `vinculo.py:975`; o teste agora exige uma ocorrência e retira a flag desse trecho, cobrando especificamente `criar_trecho_retroativo` (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:209`). Reproduzi os seis corpos de teste com armazenamento temporário em memória: passaram; o mutante do trecho retornou o achado esperado. Execução pelo runner Django original: [nao verificado].

**n–s:**

| Caso | O que realmente cobra |
|---|---|
| n | Detecta `EC.objects.all().create(...)` sem flag (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:235`, `:254`). |
| o | Detecta cadeia `_base_manager.filter(...).get_or_create(...)` sem flag (`:236`, `:255`). |
| p | Detecta nascimento sem flag quando só `defaults` tem a flag e `create_defaults` não tem (`:237`, `:256`). |
| q | Controle negativo: aceita flag em `create_defaults`; não testa escrita indevida em existente (`:239`, `:257`). |
| r | Detecta ausência de keyword literal; acusa até `**kw` correto, como declarado (`:25`, `:241`, `:257`). |
| s | Controle negativo de receptor não identificado como modelo; não prova que `c` nunca representará um modelo (`:242`, `:257`). |

São exemplos AST, não exercícios de criação no ORM: são strings entregues ao scanner (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:219`, `:243`).

**Buracos não declarados, reproduzidos em memória com resultado `[]`:**

1. **Atualização pelo `update_or_create`.** Exemplo:

   ```python
   EC.objects.update_or_create(
       pk=1,
       defaults={"trabalha_em_feriado": True},
       create_defaults={"trabalha_em_feriado": origem.trabalha_em_feriado},
   )
   ```

   O scanner aprova a origem do valor de nascimento e não examina a atualização de existente por essa API; só há branches de escrita para `update`, `bulk_update`, `setattr` literal e atribuição direta (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:110`, `:139`, `:147`, `:156`).

2. **Prioridade incorreta de busca versus defaults.** `get_or_create(trabalha_em_feriado=origem.trabalha_em_feriado, defaults={"trabalha_em_feriado": True})` passa porque `_kw` retorna primeiro a keyword de busca (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:112`).

3. **Sobreposição dentro do próprio dict.** Passam tanto `defaults={"trabalha_em_feriado": origem.trabalha_em_feriado, **kw}` como uma segunda chave literal com valor `True`: `_kw` aceita a primeira ocorrência literal e não resolve sobreposições (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:116`).

4. **Gerente reverso.** `colaborador.escalas.create(posto=p, tipo_escala=t, data_inicio=d)` escapa: a relação existe (`app/escala/models.py:872`), mas `_do_gerente` exige acesso reconhecido pelo modelo e pelos nomes `objects`, `_default_manager`, `_base_manager` (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:43`, `:97`).

5. **Resolução indireta do modelo/construtor.** Alias anotado (`EC: type = EscalaColaborador`), função ligada (`criar = EC.objects.create`) e modelo obtido por `apps.get_model(...)` passam. `_apelidos` só acompanha `ImportFrom` e atribuição simples cujo valor já seja reconhecido como modelo (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:57`, `:68`, `:76`).

6. **APIs assíncronas.** Nomes como `acreate`, `aget_or_create`, `aupdate_or_create`, `abulk_create`, `aupdate` e `abulk_update` não entram nas listas/branches (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:42`, `:136`, `:147`, `:149`).

7. **Escritas indiretas/desempacotadas.** Passam atribuição `ec.trabalha_em_feriado, ec.ativa = True, True`, `ec.__dict__["trabalha_em_feriado"] = True`, `setattr(ec, campo, True)` e `bulk_update(objs, campos)` com o nome armazenado em variável. A detecção exige alvo `ast.Attribute` direto e constantes literais nos dois últimos casos (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:149`, `:153`, `:156`).

Esses limites não aparecem na lista declarada (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:24`). São possibilidades demonstradas do scanner; uso atual dessas formas para escrever a flag na produção da cópia: [nao verificado].

O censo do scanner atual retorna zero achados e um escritor. Os quatro nascimentos explícitos estão em `app/colaboradores/services/vinculo.py:287`, `:971`, `:1090`, `:1111`; a atribuição direta da porta está em `:893`. Essa contagem não fecha os buracos acima, pois usa justamente o scanner limitado (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:124`).

## 13/11 11:23 · sessao c09f9bf8868b · - · - · origem=Ronald (tmux)

F e pontos de E/D auditados:

- **4 — FILA com ressalva.** A porta salva a flag (`app/colaboradores/services/vinculo.py:894`); o signal chama a invalidação (`app/escala/signals.py:137-146`), que alcança todos os fechamentos com previsto preenchido do colaborador, sem excluir exportados (`app/ponto/services/fechamento.py:1607-1613`). Ela **zera `previsto_em`, mas atualiza `atualizado_em` para agora** (`app/ponto/services/fechamento.py:1613`). O comportamento é anterior no delta fornecido; a execução de produção é [nao verificado].

- A alegação da triagem de que **“o apply mede hash do exportado antes/depois” não está implementada nesse comando**: seu corpo mede universo/contagens e escreve reversão, sem hash (`app/escala/management/commands/aplicar_feriado_lei_1609.py:43-77`). Além disso, os hashes usuais excluem deliberadamente `previsto_em` e `atualizado_em` (`app/ponto/portas/regen_exportada.py:35-38`, `app/ponto/portas/regen_exportada.py:62-74`; `app/ponto/management/commands/carimbo_gravado.py:24-36`). Portanto, mesmo um hash desses intacto não provaria preservação dos carimbos.

- O motivo para tratar 4 como dívida anterior é defensável **se preservar exportada significa preservar dinheiro/TXT**, pois `previsto_em` é explicitamente o frescor do previsto, separado de `derivado_em`, carimbo do registro inteiro (`app/ponto/models.py:430-431`, `app/ponto/models.py:453-464`). Entretanto, sua alteração tem leitor real: `prontidao` abandona snapshot e recalcula ao vivo quando o campo é nulo (`app/folha/export.py:1045-1049`, `app/folha/export.py:1081`). Não declarar o fechamento inteiro literalmente intocado.

- **5 — DESCARTE sustentado pelo código.** O universo usa a autoridade `q_vinculo_cobre_a_data` (`app/escala/models.py:947-950`). Cobertura exige início ≤ data, fim nulo ou ≥ data, e vigência declarada: `ativa` **ou** fim não nulo (`app/escala/alimentacao.py:195-196`, `app/escala/alimentacao.py:214-225`). Portanto, um `ativa=False` com fim hoje/futuro é vigente e entra; um `ativa=True` com início futuro não entra. As duas pontas são inclusivas (`app/escala/alimentacao.py:214-225`). Não há bloqueio autônomo de pouso demonstrado por esse descarte.

- **6 — FILA sustentada quanto à anterioridade e ausência de cron**, pois o delta fornecido só injeta o dia nesse comando e o cadastro o classifica como sincronização sob ordem (`app/config/crons.py:1406`). A divergência continua viva: cargo decide o desejado (`app/colaboradores/management/commands/sincronizar_feriado_admin.py:19-21`, `app/colaboradores/management/commands/sincronizar_feriado_admin.py:42-58`), enquanto a lei decide por posto/ciclo (`app/escala/models.py:905-917`). Exemplo lógico: cargo administrativo em posto que declara operar recebe False no sincronizador e True no juiz (`app/colaboradores/management/commands/sincronizar_feriado_admin.py:47`; `app/escala/models.py:911-913`). É segunda derivação do desejado em sítio tocado, embora a escrita continue pela porta (`app/colaboradores/management/commands/sincronizar_feriado_admin.py:58`).

- **Ordem de trava:** comprova-se `atomic` → UPDATE vínculo → UPDATE FechamentoMensal → escrita de células / trava das futuras (`app/colaboradores/services/vinculo.py:868`, `app/colaboradores/services/vinculo.py:894`; `app/ponto/services/fechamento.py:1613`; `app/ponto/portas/celula.py:308-311`; `app/ponto/services/cadastro_futuro.py:30-36`). **Não encontrei trava de competência nesse caminho**: a guarda de exportada é consulta, seguida de `atomic`, sem `select_for_update` (`app/ponto/portas/celula.py:218-226`). O dinheiro é agendado para depois do commit (`app/ponto/portas/celula.py:483-496`; `app/ponto/services/fechamento.py:1798-1824`). Deadlock concreto/violação de ordem global: [nao verificado]. Não confundir consulta de guarda com aquisição de trava.

- **Pré-requisitos:** lista esperada é conferida antes da primeira escrita; reversão é criada com modo exclusivo antes do primeiro vínculo (`app/escala/management/commands/aplicar_feriado_lei_1609.py:61-73`). O comando publica contagens por grupo e lista plana de IDs; não oferece hash, conferência explícita de 20/11 nem relatório do rejulgamento de 02/11 (`app/escala/management/commands/aplicar_feriado_lei_1609.py:36-77`). Há teste de Finados em fixture, sem equivaler a produção (`app/escala/tests/test_feriado_lei_1609.py:112-177`). Aplicação, publicação da lista, reversão efetiva, 7 casos 12x36 encaminhados, resultado de 02/11 e conferência de 20/11: [nao verificado].

- **Um significado da flag:** o nascimento consulta posto em todo ciclo (`app/escala/models.py:905-917`), mas o efeito de virar folga é condicionado à mesma flag False, ciclo elegível e feriado do município do vínculo (`app/escala/models.py:1410-1432`). A precedência chama esse predicado (`app/ponto/precedencia.py:369-377`). False em 12x36 com efeito nulo está expressamente previsto, não demonstra por si campo com dois significados (`app/escala/models.py:1427-1430`).

## 13/11 11:23 · sessao 038c6893fc6f · - · - · origem=Ronald (tmux)

Censo C: 16 pontos de chamada, todos com `hoje` explícito; não encontrei chamador sem ele. A assinatura exige keyword sem default (`app/colaboradores/services/vinculo.py:869`).

| Chamador | Origem de `hoje` | Leituras diretas para `hoje` |
|---|---|---|
| `app/colaboradores/management/commands/sincronizar_feriado_admin.py:58` | `timezone.localdate()` em `:56` | 1 por execução com `--apply`; 0 no DRY |
| `app/escala/management/commands/aplicar_feriado_lei_1609.py:73` | `timezone.localdate()` em `:48` | 1 por execução, inclusive DRY; mesmo valor antes `:49` e depois `:75` |
| `app/chamados/tests/test_jv1_feriado_vira_vigia.py:89` | `timezone.localdate()`, sob freeze de `:33`/`:38` | 1 por `_marcar`; chamadas em `:110`, `:124`, `:139`, `:147`, `:149` |
| `app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:133` | `timezone.localdate()`, freeze em `:124` | 1 por `_marcar`; chamadas em `:154`, `:168` |
| `app/escala/tests/test_porta_escala.py:62`, `:65` | Literal `datetime.date(2026,11,13)` | 0 |
| `app/ponto/tests/test_cadastro_futuro_c3.py:114` | `HOJE` importado da base em `:15` | 0 |
| `app/ponto/tests/test_cadastro_futuro_c5b.py:124`, `:138`, `:144` | `HOJE` importado da base em `:25` | 0 |
| `app/ponto/tests/test_cadastro_futuro_sem_banco.py:446`, `:469`, `:476`, `:481`, alias `evento` em `:662`, `:664` | Literal `HOJE = date(2026,11,12)` em `:20`; alias em `:661` | 0 |

Chamadores indiretos dos comandos: `call_command` de admin em `app/colaboradores/tests/test_feriado_admin_sync.py:41` e `:45` usa o relógio real do comando; aplicar é chamado em `app/escala/tests/test_feriado_lei_1609.py:102` e no helper `:140`, a segunda classe sob freeze `:112`. O helper é usado em `:150`, `:157`, `:164`, `:183`, `:189` e `:190`.

A alegação “um relógio por ato” só ficou verdadeira para a captura explícita de `hoje` nessas bordas, não para a cadeia:

- A porta salva a flag em `app/colaboradores/services/vinculo.py:894`; o signal `app/escala/signals.py:137` chama `invalidar_previsto` em `:146`, que lê `timezone.now()` em `app/ponto/services/fechamento.py:1613`, por vínculo alterado.
- A regeneração carimba cada célula reescrita com `timezone.now()` em `app/ponto/portas/celula.py:308` — outro ramo em `:263`.
- Há uma leitura **decisória**, não apenas carimbo: `_o_chao_do_dia_mudou` não recebe `hoje` em `app/ponto/portas/celula.py:404`; relê `timezone.localdate()` em `:464` e usa esse novo dia para separar passado/futuro em `:469`.
- O suplemento futuro chama a regeneração uma vez por dia em `app/ponto/services/cadastro_futuro.py:43`; cada dia tocado pode chegar novamente à leitura de `app/ponto/portas/celula.py:464`.
- O recalculo financeiro também não recebe o instante da borda: `app/ponto/services/fechamento.py:1798`, callback `:1807`, chamada `:1810`; lê relógio para teto do fato em `:616`, previsto em `:1086`, derivação em `:1143` ou `:1189`, atualização em `:1161`. Número total executado por ato depende das células, competências e callbacks [nao verificado].
- O no-op retorna antes desses efeitos em `app/colaboradores/services/vinculo.py:890`.

D, significado e fluxo:

- `hoje` das bordas é dia civil local: `timezone.localdate` em ambos comandos; configuração `America/Sao_Paulo` e `USE_TZ=True` em `app/config/settings/base.py:93` e `:95`.
- A porta usa `janela_anterior(hoje, empresa)` em `app/colaboradores/services/vinculo.py:900`; começa no máximo entre início do vínculo e início anterior em `:901`; vai até `hoje` em `:902`/`:906`.
- Janelas são inclusivas: corte do mês anterior a corte−1 do mês corrente, `app/ponto/janelas.py:10` a `:21`; atual e anterior em `:60` e `:66`. A célula percorre `while d <= ate` em `app/ponto/portas/celula.py:228` e barra exportadas usando `a <= d <= z` em `:284`.
- Futuro exclui hoje, começa em `hoje+1`, `app/ponto/services/cadastro_futuro.py:26`–`:27`; seleciona geradas existentes com `data__range` em `:31`–`:32`. Seu horizonte é união das competências correntes das empresas ativas, `app/ponto/management/commands/gerar_celulas.py:290`–`:300`, não estritamente a janela da empresa do vínculo.
- Vigência cobre início e fim inclusivos, `app/escala/alimentacao.py:214`–`:216`; SQL equivalente em `:219`–`:225`. `ativa=False` com fim futuro pode entrar legitimamente.
- `previsto_em` é carimbo de frescor do previsto, não garantia de frescor do registro inteiro; isso está explícito em `app/ponto/models.py:430`, `:453`–`:464`.
- Escritores: recalculo atribui timestamp em `app/ponto/services/fechamento.py:1086`, limpa em falha `:1099`, persiste via `save` em `:1144` ou update `:1188`; invalidador limpa em `:1613`.
- Leitor operacional da prontidão: `app/folha/export.py:1045` aceita snapshot quando `previsto_em` existe e minutos não são `None`; explica a invalidação ativa em `:1046`–`:1049`.
- Fotografia E4B trata `previsto_em` como carimbo mutável da leitura: `app/chamados/services/foto_e4b.py:10`–`:15`.
- Hash financeiro o exclui deliberadamente, juntamente com `atualizado_em`: `app/ponto/portas/regen_exportada.py:37`–`:38`, `:62`–`:74`.
- Flag é lida para no-op/trilha em `app/colaboradores/services/vinculo.py:890`/`:892`; lida/copiada nos nascimentos `:975`, `:1093`, `:1114`; nascimento novo pergunta ao juiz em `:296`; escritor de alteração `:893`–`:894`.
- Juiz padrão posto/ciclo em `app/escala/models.py:905`–`:917`; contador lê a flag em `:961`, usa esse juiz em `:960`; consumidor efetivo `feriado_vira_folga` lê a flag em `:1427` e restringe ciclos em `:1429`. O campo tem help text restrito a weekday em `:898`; a flag pode ser False em 12x36 sem tornar aquele plantão folga (`:1429`).
- Há divisão de instante dentro do ato: hoje da borda rege universo/regeneração e o novo hoje de `celula.py:464` rege julgamento; execução atravessando meia-noite pode discordar nesses limites, pela comparação `:469`. Ocorrência real [nao verificado].

F4: a justificativa “o apply mede hash do exportado antes/depois” **não aparece no comando**: `app/escala/management/commands/aplicar_feriado_lei_1609.py:43`–`:77` mede contador antes/depois, não hash. O hash padrão excluiria exatamente `previsto_em` e `atualizado_em` (`app/ponto/portas/regen_exportada.py:37`–`:38`). O signal atinge todos os fechamentos do colaborador com previsto preenchido, sem guarda de exportado/competência (`app/escala/signals.py:137`–`:146`; `app/ponto/services/fechamento.py:1610`–`:1613`). Ele zera `previsto_em`, **empurra**, não zera, `atualizado_em` (`:1613`).

## 13/11 11:14 · sessao 488ea9e39dfa · FERIADO-USA-A-LEI-DE-1609 · verificador · origem=Code

A. Os quatro nascimentos localizados em produção são:

| Sítio | Origem de `trabalha_em_feriado` |
|---|---|
| `executar_vinculo` | Pergunta a `padrao_trabalha_em_feriado(template, colaborador.posto)`. `app/colaboradores/services/vinculo.py:287`, `app/colaboradores/services/vinculo.py:296`. |
| `criar_trecho_retroativo` | Copia `ec.trabalha_em_feriado`. `app/colaboradores/services/vinculo.py:970`, `app/colaboradores/services/vinculo.py:974`. |
| `ajustar_vinculo_pelo_sistema` | Copia `ec.trabalha_em_feriado`. `app/colaboradores/services/vinculo.py:1089`, `app/colaboradores/services/vinculo.py:1092`. |
| `desfazer_ajuste_do_sistema` | Copia `antigo.trabalha_em_feriado`. `app/colaboradores/services/vinculo.py:1110`, `app/colaboradores/services/vinculo.py:1113`. |

Nenhum desses quatro depende do default; o default `True` continua no modelo. O censo declarado pelo próprio selo também nomeia esses quatro nascimentos. `app/escala/models.py:897`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:16`.

A catraca reconhece importação com apelido, atribuições de apelidos e os managers `objects`, `_default_manager`, `_base_manager`. Construtor reconhecido sem keyword da flag é denunciado, mesmo seguido de `save`. `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:33`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:47`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:110`.

Há uma imprecisão no limite declarado: `EC.objects.create(**kw)` **é visto**, mas `_kw` não resolve o dicionário e acusa ausência da flag; pode reprovar inclusive um dicionário correto. Já `.update(**kw)` escapa à busca de keyword explícita. `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:20`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:87`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:116`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:122`.

**Formas invisíveis e não declaradas no limite:**

- `EC.objects.all().create(...)`: o detector exige o manager imediatamente antes do criador. `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:110`.
- Cópia por `ec.pk = None; ec.save()`: não há fiscalização de reset da PK nem de nascimento por `save`. Ocorrência real dessa forma para `EscalaColaborador`: `[nao verificado]`. `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:108`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:131`.
- `update_or_create(..., defaults={flag: origem.flag}, create_defaults={...})`: `create_defaults` não é examinado; a flag de `defaults` pode satisfazer a catraca sem garantir a criação. `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:87`.
- A autenticidade do juiz e da cópia não é comprovada: qualquer chamada terminada em `.padrao_trabalha_em_feriado(...)` e qualquer atributo terminado em `.trabalha_em_feriado` são aceitos pelo nome. `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:70`.

B. O universo implementado é:

`data_inicio <= hoje`, término nulo ou `>= hoje`, vigência declarada, colaborador com `situacao='ativo'` e ciclo em `CICLOS_FERIADO_VIRA_FOLGA`. Vigência declarada significa `ativa=True` **ou** término preenchido. `app/escala/alimentacao.py:196`, `app/escala/alimentacao.py:214`, `app/escala/alimentacao.py:219`, `app/escala/models.py:949`.

Portanto, **não equivale a `ativa=True`**: admite inativo com intervalo cobrindo hoje; exclui ativo futuro ou cujo término já passou. São contraexemplos do predicado, não constatações de produção. A equivalência pretendida entre “ATIVOS” do aval e esse recorte está `[nao verificado]`. `app/escala/alimentacao.py:214`, `app/escala/models.py:949`.

O mapa de escrita e leitura dos insumos é:

| Campo | Escritores | Leitores relevantes |
|---|---|---|
| `ativa` | Nascimentos; reativação; fechamento; absorção; snapshot; desfazer. `app/colaboradores/services/vinculo.py:276`, `:292`, `:641`, `:730`, `:799`, `:843`, `:1031`, `:1106`. | Juiz de cobertura e seleção do vínculo. `app/escala/alimentacao.py:196`, `:214`, `:286`. |
| `data_inicio` | Nascimentos e restauração do snapshot. `app/colaboradores/services/vinculo.py:291`, `:972`, `:1032`, `:1090`, `:1111`. | Cobertura e início da regeneração. `app/escala/alimentacao.py:214`, `app/colaboradores/services/vinculo.py:900`. |
| `data_fim` | Reativação, fechamento/reabertura, encerramento, correção retroativa, snapshot e desfazer. `app/colaboradores/services/vinculo.py:277`, `:641`, `:730`, `:844`, `:982`, `:1032`, `:1106`. | Cobertura e guarda de regeneração. `app/escala/alimentacao.py:214`, `app/ponto/portas/celula.py:229`. |
| `situacao` | Cadastro pela porta do colaborador; desligamento e reativação. `app/colaboradores/services/colaborador.py:32`, `:55`, `:73`, `:91`; `app/colaboradores/services/desligamento.py:109`, `:215`. | Filtro do contador e julgamento de vínculo. `app/escala/models.py:949`, `app/colaboradores/services/vinculo.py:91`. |
| `tipo_ciclo` e `tipo_base` | Cadastro do tipo; cópia do tipo. Edição pelo cadastro conserva o par existente. `app/escala/services/cadastro_tipo.py:152`, `:177`, `:179`, `:409`. | Universo, classificação, juiz padrão e feriado virar folga. `app/escala/models.py:916`, `:950`, `:955`, `:1429`. |
| `opera_em_feriado` | Criação/atualização genérica do posto e porta específica acionada pelo command. `app/colaboradores/services/estrutura.py:43`, `:67`, `:83`; `app/colaboradores/management/commands/marcar_feriado_do_posto.py:132`. | Juiz padrão, classificação DP, contador antigo, veto de cobrança e relatório de presença. `app/escala/models.py:911`, `:932`, `:956`; `app/colaboradores/services/feriado_do_posto.py:69`, `:127`. |
| Posto do vínculo | Nasce do cadastro ou é copiado do vínculo; atualização do vínculo pode substituir pelo posto do colaborador. `app/colaboradores/services/vinculo.py:275`, `:289`, `:971`, `:1090`, `:1111`. | É o posto usado pelo contador e pelo juiz padrão; também determina o lugar do feriado daquele vínculo. `app/escala/models.py:956`, `:960`, `:1420`. |
| Posto do colaborador | Cadastro e operações de troca pela porta do colaborador. `app/colaboradores/services/colaborador.py:33`, `:73`, `:91`; `app/colaboradores/views.py:327`, `:1388`. | Alimenta criação/atualização do vínculo; **não é consultado diretamente pelo contador**. `app/colaboradores/services/vinculo.py:275`, `:296`; `app/escala/models.py:960`. |

O valor **devido** vem exclusivamente da chamada ao juiz: posto declarado vence; sem declaração, somente 5x2/6x1 com base comercial recebem `False`; demais recebem `True`. `app/escala/models.py:911`, `app/escala/models.py:916`, `app/escala/models.py:960`.

A leitura direta de `opera_em_feriado` para separar o DP **não calcula outro devido**: identifica a ausência de declaração exigida pelo recorte e interrompe antes do julgamento. Inclui todos os 6x1 sem declaração, mesmo os cuja flag já coincidiria com o padrão. `app/escala/models.py:956`, `app/escala/models.py:960`.

Quanto a `posto=None`: a FK do vínculo é obrigatória. O ramo defensivo, se receber efetivamente `None`, encaminha 6x1 para `dp[None]`; nos demais ciclos o juiz usa o tipo. Existência de vínculo assim em produção: `[nao verificado]`. `app/escala/models.py:873`, `app/escala/models.py:911`, `app/escala/models.py:956`.

C. O command altera a flag somente chamando `marcar_trabalha_em_feriado`; a atribuição e o `save` estão nessa porta. `app/escala/management/commands/aplicar_feriado_lei_1609.py:73`, `app/colaboradores/services/vinculo.py:893`.

A reversão usa modo exclusivo `'x'`, grava os pares e fecha o arquivo antes da primeira chamada à porta. Lista diferente do universo interrompe antes da reversão e das escritas de negócio. Não há transação ou bloqueio abrangendo o lote inteiro; a comparação protege a fotografia consultada. `app/escala/management/commands/aplicar_feriado_lei_1609.py:64`, `app/escala/management/commands/aplicar_feriado_lei_1609.py:68`, `app/escala/management/commands/aplicar_feriado_lei_1609.py:69`, `app/escala/management/commands/aplicar_feriado_lei_1609.py:72`.

O command publica os grupos antes, os IDs com seus valores desejados, a lista DP por posto e as contagens depois. `app/escala/management/commands/aplicar_feriado_lei_1609.py:36`, `app/escala/management/commands/aplicar_feriado_lei_1609.py:51`, `app/escala/management/commands/aplicar_feriado_lei_1609.py:55`, `app/escala/management/commands/aplicar_feriado_lei_1609.py:75`.

**Relógio:** há uma leitura de `localdate()` no command e outra por vínculo efetivamente alterado na porta: **`1 + N` leituras diretas**. Não há `now()` diretamente nesses dois corpos; o `hoje` do command não é transmitido à porta. `app/escala/management/commands/aplicar_feriado_lei_1609.py:48`, `app/escala/management/commands/aplicar_feriado_lei_1609.py:73`, `app/colaboradores/services/vinculo.py:890`, `app/colaboradores/services/vinculo.py:897`.

Existem leituras transitivas adicionais: `now()` na invalidação do previsto, `now()` por célula reescrita e `localdate()` no julgamento da regeneração. Logo, “UM relógio” não descreve o ato completo. `app/ponto/services/fechamento.py:1613`, `app/ponto/portas/celula.py:308`, `app/ponto/portas/celula.py:464`.

O primeiro dia **solicitado** é `max(data_inicio, início da janela anterior)`. Com hoje em 13/11/2026, corte 21 e vínculo iniciado antes disso, resulta em **21/09/2026**; primeiro dia efetivamente alterado na frota: `[nao verificado]`. `app/colaboradores/services/vinculo.py:899`, `app/ponto/janelas.py:10`, `app/ponto/janelas.py:66`.

A guarda das células considera TXT entregue ou holerite publicado; `FechamentoMensal.status` sozinho não a fecha. Os dias barrados são pulados antes da regeneração. `app/ponto/management/commands/gerar_celulas.py:135`, `app/folha/export.py:618`, `app/ponto/portas/celula.py:284`.

**O `FechamentoMensal` exportado não fica integralmente intacto:** o `save` da flag dispara `invalidar_previsto`, que zera `previsto_em` e altera `atualizado_em` em todas as competências do colaborador com previsto preenchido, sem filtro de exportação. `app/colaboradores/services/vinculo.py:894`, `app/escala/signals.py:137`, `app/ponto/services/fechamento.py:1611`.

Se houver somente status exportado, sem entrega reconhecida pela guarda, a regeneração pode chegar ao recálculo; o escritor atualiza inclusive fechamentos de status diferente de aberto. Isso é possibilidade do caminho, não ocorrência comprovada na frota. `app/ponto/management/commands/gerar_celulas.py:141`, `app/ponto/services/fechamento.py:1809`, `app/ponto/services/fechamento.py:1145`, `app/ponto/services/fechamento.py:1160`.

Para o futuro, a porta alcança somente células geradas **já existentes**, dentro da janela selecionada. O teste novo monta e confere 02/11; conferência de 20/11 e resultados reais dos corrigidos estão `[nao verificado]`. `app/ponto/services/cadastro_futuro.py:26`, `app/ponto/services/cadastro_futuro.py:31`, `app/escala/tests/test_feriado_lei_1609.py:113`, `app/escala/tests/test_feriado_lei_1609.py:159`.

D. **12x36 fica fora** pelo filtro de ciclos; **6x1 sem declaração fica fora do lote** pelo `continue` para DP. `app/escala/models.py:32`, `app/escala/models.py:950`, `app/escala/models.py:957`.

“Outros” recebe somente **semanal** no conjunto atual. 24x48, personalizado e intermitente também ficam fora do universo; não entram escondidos em “outros”. O aval fornecido admite o grupo “outros”; sua população real está `[nao verificado]`. `app/escala/models.py:26`, `app/escala/models.py:32`, `app/escala/models.py:964`.

E. As classes de reprovação ficam assim:

- **Segundo escritor da flag:** os nascimentos passam valor explícito; a alteração posterior localizada está na porta. O command novo não cria escritor cru. `app/colaboradores/services/vinculo.py:296`, `:893`, `:974`, `:1092`, `:1113`; `app/escala/management/commands/aplicar_feriado_lei_1609.py:73`.
- **Derivação paralela:** `sincronizar_feriado_admin` continua calculando a flag pelo cargo, sem consultar o juiz de posto/ciclo, e depois chama a porta. Pode contrariar o valor de 16/09, embora use o escritor correto; execução desse command em produção está `[nao verificado]`. `app/colaboradores/management/commands/sincronizar_feriado_admin.py:19`, `:47`, `:56`; `app/escala/models.py:911`.
- **Segundo julgamento da cobrança:** após consultar `fatos_do_dia`, o emissor ainda aplica `cobra_no_dia`, que veta por posto fechado independentemente da flag. Isso é um veto paralelo vivo, declarado no comentário, e não a classificação DP. O juiz registrado para supressão por feriado é `fatos_do_dia`. `app/ponto/management/commands/detectar_ausencias.py:616`, `:619`, `:628`; `app/colaboradores/services/feriado_do_posto.py:69`; `app/core/juizes.py:296`.
- **Relógio fora da borda:** presente na porta e na regeneração, apesar da leitura inicial do command. `app/colaboradores/services/vinculo.py:897`, `app/ponto/portas/celula.py:464`.
- **Selo/RED:** os mutantes de remoção da keyword e literal `True` são denunciados pelo detector, mas o teste exige **duas** ocorrências de uma âncora que possui apenas **uma**. A outra criação tem `folga_calendario` depois da flag. Assim, o teste falha antes de alcançar o mutante fora de `executar_vinculo`. Suite Django executada: `[nao verificado]`. `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:177`, `:184`, `:185`, `:186`; `app/colaboradores/services/vinculo.py:974`, `:1092`.
- **Semântica:** `trabalha_em_feriado` é autorização do vínculo com efeito limitado aos ciclos weekday; `opera_em_feriado` é declaração triestado do posto, usada tanto para inicialização quanto como veto direto de cobrança. Não são sinônimos nem uma cópia permanentemente sincronizada. `None` no posto significa “cobra” no veto, mas pode produzir folga pelo ciclo no juiz padrão. `app/escala/models.py:898`, `:911`, `:1427`; `app/colaboradores/models.py:256`; `app/colaboradores/services/feriado_do_posto.py:63`.

F. **A entrada nova não quebra o contrato enumerador encontrado.** O command ganha casa em `FORA_DE_PIPELINE`; o contrato exige justamente estar agendado ou nesse dicionário, e proíbe exclusão podre ou simultaneamente agendada. A enumeração de agendados contém `CRONS + ENCADEADOS`. `app/config/crons.py:1407`, `app/config/crons.py:1805`, `app/chamados/tests/test_contract_crons.py:220`, `app/chamados/tests/test_contract_crons.py:230`.

**REPROVA — bloqueantes: selo com âncora inválida (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:185`); nascimentos invisíveis não declarados (`:87`, `:110`); relógio relido fora da borda (`app/colaboradores/services/vinculo.py:897`); fechamento exportado alcançado pela invalidação (`app/ponto/services/fechamento.py:1611`); universo não equivalente a `ativa=True` (`app/escala/models.py:949`); derivação paralela por cargo (`app/colaboradores/management/commands/sincronizar_feriado_admin.py:47`).**

## 13/11 11:11 · sessao 4564e305e0e5 · - · - · origem=Ronald (tmux)

A/E — resultados do explorador, somente leitura:

- Há quatro nascimentos explícitos em produção, todos por `EscalaColaborador.objects.create`: `executar_vinculo` pergunta ao juiz usando `template` e `colaborador.posto` (`app/colaboradores/services/vinculo.py:287`, `:296`); `criar_trecho_retroativo` copia `ec.trabalha_em_feriado` (`:970`, `:974`); `ajustar_vinculo_pelo_sistema` copia `ec.trabalha_em_feriado` (`:1089`, `:1092`); `desfazer_ajuste_do_sistema` copia `antigo.trabalha_em_feriado` (`:1110`, `:1113`). Nenhum desses usa o default, que permanece `True` (`app/escala/models.py:896`).
- O selo reconhece apelidos importados e atribuídos, os três managers nomeados, construtor sem keyword e chamadas diretas `create(**kw)`; esta última é acusada como ausência de valor explícito, contrariando a descrição de “não vê” (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:47`, `:87`, `:110`, `:116`). Executei as funções AST em memória: censo atual `achados=[]`; escritor contado uma vez (`:98`, `:131`). Suite Django não executada `[nao verificado]`.
- Formas invisíveis **não declaradas**: `EC.objects.all().create(...)`; clone `ec.pk=None; ec.save()`; `update_or_create(defaults={flag: origem.flag}, create_defaults={"ativa":True})`. As duas primeiras escapam porque o detector exige manager imediatamente antes da criação e não observa `save`/reset de PK; a terceira porque `_kw` ignora `create_defaults` (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:87`, `:110`, `:131`). Reprodução em memória confirmou `[]` nas três.
- A autenticidade da fonte também não é provada: qualquer `unrelated.padrao_trabalha_em_feriado(...)` ou atributo `qualquer_objeto.trabalha_em_feriado` é aceito apenas pelo nome (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:70`).
- **Bloqueante determinístico do selo:** o teste exige duas ocorrências da string terminada em `ec.trabalha_em_feriado)\n`, mas há uma só; a ocorrência do ajuste leva `folga_calendario` na sequência (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:184`, `:185`; `app/colaboradores/services/vinculo.py:974`, `:1092`). Portanto esse teste falha antes de exercitar o mutante externo pretendido (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:186`). Contagem literal por leitura confirmou `1`.
- O command antigo deriva flag por cargo, depois escreve pela porta: é derivação paralela da política da flag, embora não seja segundo escritor (`app/colaboradores/management/commands/sincronizar_feriado_admin.py:19`, `:47`, `:56`).
- `cobra_no_dia` está vivo: `_abrir_chamado` é chamado no command e aplica primeiro `fatos_do_dia`, depois veto independente do posto (`app/ponto/management/commands/detectar_ausencias.py:440`, `:616`, `:628`). O código declara expressamente que posto fechado veta mesmo vínculo com flag de trabalho (`:619`). `opera_em_feriado=False` é, assim, também veto direto de cobrança; `None` cobra nesse predicado, enquanto o juiz padrão usa o ciclo (`app/colaboradores/services/feriado_do_posto.py:60`; `app/escala/models.py:911`).

## 13/11 11:11 · sessao 733cc442c44b · - · - · origem=Ronald (tmux)

C — conclusões por leitura:

- `--apply` escreve a flag exclusivamente pela porta `marcar_trabalha_em_feriado`; a reversão usa criação exclusiva (`'x'`) e termina antes do primeiro chamado à porta. Universo diferente da lista interrompe antes dessas escritas. Não há transação nem bloqueio do lote inteiro. `app/escala/management/commands/aplicar_feriado_lei_1609.py:61-73`.
- Relógios diretos: **1 `localdate()` no command + 1 por vínculo efetivamente alterado na porta**, total `1+N`; nenhum `now()` direto nesses dois corpos. O command não transmite seu `hoje` à porta. `app/escala/management/commands/aplicar_feriado_lei_1609.py:48`, `app/escala/management/commands/aplicar_feriado_lei_1609.py:73`, `app/colaboradores/services/vinculo.py:890`, `app/colaboradores/services/vinculo.py:897`.
- Há outras leituras transitivas: `now()` na invalidação do previsto disparada pelo save, `now()` por célula reescrita e `localdate()` no julgamento posterior. Portanto, o comentário “UM relógio” não descreve o ato completo. `app/escala/signals.py:137-146`, `app/ponto/services/fechamento.py:1607-1613`, `app/ponto/portas/celula.py:308`, `app/ponto/portas/celula.py:464`.
- Primeiro dia **solicitado**: `max(ec.data_inicio, início da janela anterior à atual)`. Em 13/11/2026 com corte 21 e vínculo já iniciado, isso resulta em 21/09/2026. Primeiro dia efetivamente alterado em produção: `[nao verificado]`. `app/colaboradores/services/vinculo.py:897-905`, `app/ponto/janelas.py:10-33`, `app/ponto/janelas.py:66-72`.
- Células de competência entregue por TXT vigente ou holerite publicado são barradas; `FechamentoMensal.status='exportado'` sozinho **não** participa dessa guarda. `app/ponto/portas/celula.py:221`, `app/ponto/portas/celula.py:284-288`, `app/ponto/management/commands/gerar_celulas.py:135-188`, `app/folha/export.py:597-628`.
- **FechamentoMensal exportado não fica integralmente preservado:** todo save da flag chama `invalidar_previsto`, que altera `previsto_em` e `atualizado_em` de todas as competências do colaborador com previsto preenchido, sem filtro por status/exportação. `app/colaboradores/services/vinculo.py:894`, `app/escala/signals.py:137-146`, `app/ponto/services/fechamento.py:1607-1613`.
- Quando uma competência chega ao recalculo por evento, o dinheiro também não guarda exportada: chama o escritor, cujo ramo de status diferente de aberto atualiza os campos calculados, preservando status e aprovação. As células de competências entregues normalmente ficam fora de `_tocadas` pela guarda anterior; apenas status exportado não oferece essa proteção. `app/ponto/portas/celula.py:483-496`, `app/ponto/services/fechamento.py:1798-1810`, `app/ponto/services/fechamento.py:1137-1190`.
- Futuro: regenera somente células geradas **já existentes**, desde o máximo entre início da janela, amanhã e `desde`, até a união das janelas correntes das empresas ativas. A existência de 20/11 em produção é `[nao verificado]`. `app/ponto/services/cadastro_futuro.py:9-45`, `app/ponto/management/commands/gerar_celulas.py:290-300`.
- A prova nova monta e confere 02/11; não demonstra a conferência de 20/11. Execução real, resultados de produção e aprovação dos testes: `[nao verificado]`. `app/escala/tests/test_feriado_lei_1609.py:113-134`, `app/escala/tests/test_feriado_lei_1609.py:159-177`.

Bloqueante firme: **relógio relido fora da borda**, com `hoje` do command descartado na chamada da porta. A alegação de preservar integralmente `FechamentoMensal` exportado também é falsa pelos metadados; preservação das células depende da guarda de entrega, não do status do fechamento. `app/escala/management/commands/aplicar_feriado_lei_1609.py:48`, `app/escala/management/commands/aplicar_feriado_lei_1609.py:73`, `app/colaboradores/services/vinculo.py:897`, `app/ponto/services/fechamento.py:1607-1613`.

## 13/11 11:10 · sessao 554163499bde · - · - · origem=Ronald (tmux)

F: **não encontrei quebra de contrato pela entrada nova**: ela declara o comando sob ordem em `FORA_DE_PIPELINE`, com motivo (`app/config/crons.py:1407`).

O contrato B6.1 enumera arquivos de comandos (`app/chamados/tests/test_contract_crons.py:214`), exige casa em agendados ou `FORA_DE_PIPELINE` (`app/chamados/tests/test_contract_crons.py:220`) e proíbe entrada simultaneamente agendada ou sem arquivo vivo (`app/chamados/tests/test_contract_crons.py:230`). A fonte dos agendados contém somente `CRONS + ENCADEADOS` (`app/config/crons.py:1805`); o render percorre somente `CRONS` (`app/config/crons.py:1872`).

Reproduzi essas duas operações B6.1 por `python3 -B`, compilando a fonte em memória: `sem casa=[]`, `podres=[]`; `aplicar_feriado_lei_1609` está vivo, está fora do pipeline e não está agendado. Resultado de execução transitória, sem arquivo de evidência persistido: `[nao verificado]`. Os predicados reproduzidos estão em `app/chamados/tests/test_contract_crons.py:225` e `app/chamados/tests/test_contract_crons.py:234`.

Os demais contratos encontrados sobre estágio, dependências, horários e render examinam `CRONS`, portanto a entrada somente no dicionário de exclusões não os amplia (`app/chamados/tests/test_contract_crons.py:77`, `app/chamados/tests/test_contract_crons.py:269`, `app/chamados/tests/test_contract_crons.py:301`, `app/chamados/tests/test_contract_crons.py:330`).

## 13/11 10:52 · sessao bf56601fcd1a · FERIADO-NASCE-DA-ESCALA · verificador · origem=Code

**REPROVA.** Há caminhos que mantêm ou recriam `5x2=True`, herança sem escolha obrigatória, reversão incompleta e escolha humana sem efeito em dois ciclos. (`app/colaboradores/services/vinculo.py:1027`, `app/colaboradores/services/vinculo.py:1078`, `app/colaboradores/services/vinculo.py:185`, `app/escala/models.py:1422`)

A correspondência da cópia com o hash Git informado, execução da suíte, opus, deploy, aplicação dos 16 e resultados posteriores em produção permanecem **[não verificado]**. [nao verificado]

**A. Todos os nascimentos e reaproveitamentos encontrados**

| Sítio de produção | Origem de `trabalha_em_feriado` |
|---|---|
| `executar_vinculo`: criação | `v['trabalha_em_feriado']`, calculado por `_feriado_do_veredito`. (`app/colaboradores/services/vinculo.py:144`, `app/colaboradores/services/vinculo.py:329`, `app/colaboradores/services/vinculo.py:338`) |
| `executar_vinculo`: reaproveitamento da mesma data | Troca o tipo e recebe a flag do mesmo veredito. (`app/colaboradores/services/vinculo.py:315`, `app/colaboradores/services/vinculo.py:317`) |
| `criar_trecho_retroativo` | `feriado_do_trecho(tipo_destino, flag_original)`. (`app/colaboradores/services/vinculo.py:1017`, `app/colaboradores/services/vinculo.py:1021`) |
| `ajustar_vinculo_pelo_sistema` | `feriado_do_trecho(tipo_derivado, flag_original)`. (`app/colaboradores/services/vinculo.py:1136`, `app/colaboradores/services/vinculo.py:1139`) |
| `desfazer_ajuste_do_sistema`: renascimento | `feriado_do_trecho(tipo_antigo, flag_antiga)`. (`app/colaboradores/services/vinculo.py:1158`, `app/colaboradores/services/vinculo.py:1161`) |
| `desfazer_ajuste_do_sistema`: reabertura | Reativa o antigo, mantendo sua flag sem novo julgamento. (`app/colaboradores/services/vinculo.py:1149`, `app/colaboradores/services/vinculo.py:1154`) |
| `corrigir_vinculo_retroativo`: troca de tipo | **Mantém a flag anterior sem consultar o juiz.** (`app/colaboradores/services/vinculo.py:1027`, `app/colaboradores/services/vinculo.py:1030`) |
| `reverter_para_snapshot`: restauração de tipo | **Mantém a flag atual; o snapshot não contém a anterior.** (`app/colaboradores/services/vinculo.py:108`, `app/colaboradores/services/vinculo.py:1078`) |

Nos quatro `create` encontrados, a flag é explícita. Não encontrei outro nascimento por `get_or_create`, `update_or_create`, construtor ou `bulk_create` na produção examinada; ausência de caminhos dinâmicos adicionais é **[não verificado]**. Os quatro nascimentos estão em `app/colaboradores/services/vinculo.py:329`, `app/colaboradores/services/vinculo.py:1017`, `app/colaboradores/services/vinculo.py:1136` e `app/colaboradores/services/vinculo.py:1158`.

**Bloqueante 1 — troca retroativa fora do julgamento:** `6x1=True → 5x2` pela correção no lugar continua `True`. O comando chama exatamente essa função. (`app/colaboradores/services/vinculo.py:1027`, `app/colaboradores/services/vinculo.py:1030`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:212`)

**Bloqueante 2 — reversão do aceite incompleta:** reaproveitar `5x2=False` como `12x36=True` e depois recusar restaura o tipo `5x2`, mas deixa `True`. Uma mudança somente da flag também não é desfeita. O snapshot omite a flag e a restauração não a escreve. (`app/colaboradores/services/vinculo.py:108`, `app/colaboradores/services/vinculo.py:317`, `app/colaboradores/services/vinculo.py:1078`, `app/chamados/services/acoes_chamado.py:149`)

O default perigoso continua `True`; a proteção depende das portas e do selo, que tem escapes descritos em G. (`app/escala/models.py:897`, `app/escala/tests/test_feriado_nasce_da_escala_selo.py:90`)

**B. Matriz completa e identidade do substituído**

A função não recebe `acao`: a tabela abaixo vale igualmente quando chamada durante `criar`, `atualizar` ou o candidato inicial a `no_op`. `R` significa `ValueError`; `S` significa a flag armazenada no substituído. (`app/colaboradores/services/vinculo.py:144`, `app/colaboradores/services/vinculo.py:165`)

| Juiz | Substituído | `''`/ausente | `'1'` | `'0'` | Inválida |
|---|---|---:|---:|---:|---:|
| `True` | Nenhum / mesmo ciclo / outro ciclo | `True` | `True` | R | R |
| `False` | Nenhum / mesmo ciclo / outro ciclo | `False` | R | `False` | R |
| `None` | Nenhum | R | `True` | `False` | R |
| `None` | Mesmo ciclo | S | `True` | `False` | R |
| `None` | Outro ciclo | R | `True` | `False` | R |

A validação da escolha inválida precede o juiz; as respostas fixas vêm antes da herança; o mesmo ciclo somente é considerado quando o juiz retorna `None`. (`app/colaboradores/services/vinculo.py:173`, `app/colaboradores/services/vinculo.py:178`, `app/colaboradores/services/vinculo.py:185`)

O produto cartesiano tem combinações inalcançáveis pelo chamador:

- **`criar`:** não existe vínculo da mesma data; o substituído pode ser ausente, ativo de mesmo ciclo ou ativo de outro ciclo. A ação continua `criar`. (`app/colaboradores/services/vinculo.py:102`, `app/colaboradores/services/vinculo.py:125`, `app/colaboradores/services/vinculo.py:141`)
- **`atualizar`:** existe vínculo da mesma data, inclusive inativo; ele sempre ganha a preferência como substituído. Pode ter mesmo ciclo ou outro ciclo, mas não ser `None`. (`app/colaboradores/services/vinculo.py:122`, `app/colaboradores/services/vinculo.py:141`)
- **Candidato a `no_op`:** existente ativo, mesmo template e âncora compatível; portanto substituído existente e mesmo ciclo. Se a flag calculada difere, vira `atualizar`; caso contrário permanece `no_op`. (`app/colaboradores/services/vinculo.py:119`, `app/colaboradores/services/vinculo.py:145`)
- **Outras ações:** `julgar_vinculo` somente produz essas três. (`app/colaboradores/services/vinculo.py:120`)

**Bloqueante 3 — nascimento sem escolha:** `criar` um novo vínculo `6x1` em outra data, tendo substituído `6x1`, aceita escolha vazia e herda a flag. Não exige prova de escolha humana; um `True` legado pelo default passa como escolha. Isso é uma exceção implementada à obrigatoriedade de escolha no nascimento. (`app/colaboradores/services/vinculo.py:125`, `app/colaboradores/services/vinculo.py:170`, `app/colaboradores/services/vinculo.py:185`)

**O substituído não representa sempre o vínculo cujo período está sendo substituído:**

- Sem existente na mesma data, a consulta pega o ativo mais recente **sem limitar seu início a D**. Se ele começa depois de D, `fechar_vigencia` o recusa e `absorver_vigencias_posteriores` o absorve. A afirmação de que seria “o ativo que o executor fecha” é falsa nesse caso. (`app/colaboradores/services/vinculo.py:141`, `app/colaboradores/services/vinculo.py:664`, `app/colaboradores/services/vinculo.py:830`)
- Uma vigência inativa anterior que cobre D também pode ser encerrada pelo executor, mas não participa dessa busca de substituído. (`app/colaboradores/services/vinculo.py:141`, `app/colaboradores/services/vinculo.py:282`)
- Existindo vínculo **inativo na mesma data**, ele vence qualquer ativo que também será fechado ou absorvido. Há um objeto reaproveitado definido, mas isso não prova que sua flag é a escolha do período efetivamente substituído. (`app/colaboradores/services/vinculo.py:102`, `app/colaboradores/services/vinculo.py:141`, `app/colaboradores/services/vinculo.py:255`, `app/colaboradores/services/vinculo.py:294`)

Consequência derivável: um futuro `6x1=True` pode fornecer a herança para um novo `6x1` anterior à sua vigência; um futuro de outro ciclo pode provocar recusa mesmo havendo uma vigência anterior `6x1` cobrindo D. São caminhos demonstráveis pelo código; ocorrência nos dados fornecidos é **[não verificado]**. (`app/colaboradores/services/vinculo.py:141`, `app/colaboradores/services/vinculo.py:185`, `app/colaboradores/services/vinculo.py:282`)

**`no_op → atualizar` não executa uma operação restrita à flag.** Ele percorre fechamento e absorção, redefine posto, ativa o vínculo, apaga `data_fim`, pode copiar calendário e recalibrar âncora. Essas operações são incondicionais no executor. (`app/colaboradores/services/vinculo.py:255`, `app/colaboradores/services/vinculo.py:294`, `app/colaboradores/services/vinculo.py:316`, `app/colaboradores/services/vinculo.py:319`, `app/colaboradores/services/vinculo.py:326`, `app/colaboradores/services/vinculo.py:351`)

A regeneração passada e futura é acionada, e a trilha técnica inclui a flag. Entretanto, o histórico classifica essa alteração como `ancora`, enquanto o impacto não recebe a nova flag e calcula o “depois” pela aritmética do template. Logo a prévia não representa fielmente uma alteração somente de feriado. (`app/colaboradores/services/vinculo.py:370`, `app/colaboradores/services/vinculo.py:380`, `app/colaboradores/services/vinculo.py:395`, `app/colaboradores/services/vinculo.py:418`, `app/escala/services/impacto_troca.py:22`, `app/escala/services/impacto_troca.py:40`)

**C. Chamadores da porta**

| Chamador | Passa a escolha? Efeito |
|---|---|
| Tela Vincular | Encaminha `request.POST`; os rádios enviam `'1'`/`'0'`, sem pré-seleção. (`app/colaboradores/views.py:411`, `app/colaboradores/views.py:423`, `app/templates/colaboradores/partials/painel_vinculo.html:176`) |
| Chat: prévia | Não. `_post` monta template, início, modo e eventualmente âncora; a recusa retorna em `erros`. (`app/colaboradores/services/troca_pelo_chat.py:11`, `app/colaboradores/services/troca_pelo_chat.py:58`, `app/colaboradores/services/troca_pelo_chat.py:60`) |
| Chat: execução | Não; a interface nem recebe argumento de feriado. Novo vínculo sem substituído ou troca de ciclo para um ciclo sem padrão passa a ser recusado. Mesmo ciclo pode herdar. (`app/colaboradores/services/troca_pelo_chat.py:64`, `app/colaboradores/services/troca_pelo_chat.py:72`, `app/colaboradores/services/vinculo.py:185`) |
| Régua de defesa/realocar | Não. O dict omite a flag e captura a nova recusa. Destinos cíclicos já são recusados antes; destinos sem padrão precisam satisfazer a herança. (`app/escala/regua_defesa.py:310`, `app/escala/regua_defesa.py:313`, `app/escala/regua_defesa.py:316`) |
| `censo_fase_12x36` | Não. O dict tem template/início/âncora; para destino 12x36 o juiz fornece `True`. (`app/ponto/management/commands/censo_fase_12x36.py:419`, `app/ponto/management/commands/censo_fase_12x36.py:422`, `app/escala/models.py:921`) |

Não encontrei chamador direto adicional por API/importação nas buscas estáticas; caminhos dinâmicos permanecem **[não verificado]**. O Django admin do vínculo é somente leitura. (`app/escala/admin.py:18`)

**D. Trechos, passado e competência exportada**

`criar_trecho_retroativo` realmente alimenta regeneração do passado: o comando cria o trecho e chama a porta até `min(fim, hoje)`, além do suplemento futuro. (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:194`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:223`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:242`)

O novo `feriado_do_trecho` pode mudar a flag copiada: destino 5x2 recebe `False`; 12x36/24x48 recebe `True`; demais destinos recebem `bool(flag_original)`. Assim, também cria `12x36=True → 6x1=True` sem perguntar ao DP, embora a porta principal recuse troca de ciclo sem escolha. (`app/escala/models.py:921`, `app/escala/models.py:928`, `app/escala/models.py:934`, `app/colaboradores/services/vinculo.py:188`)

Por padrão, a regeneração consulta as competências protegidas e pula seus dias; células humanas também não são reescritas pelo ramo de regeneração. A autoridade considera TXT entregue e holerite publicado. (`app/ponto/portas/celula.py:221`, `app/ponto/portas/celula.py:284`, `app/ponto/portas/celula.py:297`, `app/folha/export.py:617`)

Existe uma exceção explícita preexistente: `corrigir_escala_retroativa --apesar-da-lavra` passa o motivo à porta, que esvazia a lista de proteção. Portanto “nunca toca exportada” não é verdade para todas as opções desse comando. Uso dessa exceção nesta obra é **[não verificado]**. (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:42`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:233`, `app/ponto/portas/celula.py:218`)

Já os chamadores do ajuste automático trabalham somente no futuro: aplicação exige `inicio > hoje`; desfazer usa `max(inicio, hoje+1)` e regenera dali em diante. (`app/escala/services/escala_auto_executor.py:100`, `app/escala/services/escala_auto_executor.py:107`, `app/escala/services/escala_auto_executor.py:143`, `app/escala/services/escala_auto_executor.py:147`)

**E. Auditoria, autoria e idempotência**

Com `autor` autenticado, `_ReqDoAutor` fornece o usuário, `registrar_log` o encaminha como ator e `evento` o grava em `LogAuditoria.usuario`. Com `autor=None`, uma request corrente pode fornecer o usuário pelo contexto; fora dela, fica a identificação do processo, sem usuário humano. (`app/colaboradores/services/vinculo.py:19`, `app/colaboradores/services/vinculo.py:939`, `app/core/auditoria.py:24`, `app/core/observ.py:108`, `app/core/observ.py:118`)

`sincronizar_feriado_admin` chama sem autor e sem motivo. Não há uma segunda gravação explícita de auditoria nesse loop; a nova linha vem da porta. (`app/colaboradores/management/commands/sincronizar_feriado_admin.py:58`, `app/colaboradores/management/commands/sincronizar_feriado_admin.py:61`, `app/colaboradores/services/vinculo.py:939`)

Mesmo valor **na instância recebida** retorna antes de save, trilha e regeneração. A porta não relê nem trava o valor persistido antes dessa comparação; portanto isso não prova idempotência sob instância desatualizada ou concorrência. (`app/colaboradores/services/vinculo.py:932`, `app/colaboradores/services/vinculo.py:936`)

**Bloqueante 4 — trilha não é garantia da transação:** `registrar_log` captura falhas; `evento` usa savepoint e engole falha da própria auditoria. A flag pode confirmar sem `LogAuditoria`, apesar do `@atomic` da porta. (`app/colaboradores/services/vinculo.py:910`, `app/core/auditoria.py:25`, `app/core/observ.py:105`, `app/core/observ.py:133`, `app/core/observ.py:139`)

O sincronizador também continua derivando a escolha dos ciclos sem padrão pelo cargo. Pode sobrescrever `6x1` sem resposta DP; não há bloqueio que o restrinja à “só lista” desta obra. Execução dele nos 82 é **[não verificado]**. (`app/colaboradores/management/commands/sincronizar_feriado_admin.py:51`, `app/colaboradores/management/commands/sincronizar_feriado_admin.py:61`, `app/escala/models.py:934`)

**F. Lote, reversão, atomicidade e relógio**

- **Universo:** vínculo que cobre hoje, colaborador ativo, ciclo 5x2 e flag `True`. Não é simplesmente `ativa=True`: pode incluir inativo com fim vigente e excluir ativo futuro. Igualdade com os “16 ativos” raspados é **[não verificado]**. (`app/escala/models.py:943`, `app/escala/models.py:945`, `app/escala/alimentacao.py:219`)
- **Lista:** compara exatamente universo e `--esperado`, antes de escrever. Imprime vínculo/colaborador/tipo/valor, mas não verifica publicação prévia no RELATO. Publicação da lista e do censo é **[não verificado]**. (`app/escala/management/commands/aplicar_feriado_nasce_da_escala.py:35`, `app/escala/management/commands/aplicar_feriado_nasce_da_escala.py:43`)
- **Reversão antes da escrita:** correto na ordem do programa. O JSON completo fecha antes do loop; modo `x` impede sobrescrita. É uma lista de valores anteriores, sem diário de progresso. (`app/escala/management/commands/aplicar_feriado_nasce_da_escala.py:51`, `app/escala/management/commands/aplicar_feriado_nasce_da_escala.py:55`)
- **Atomicidade:** por vínculo, não pelo lote. Queda após N commits deixa N aplicados e os demais intactos; a reversão contém todos. Repetir a lista original falha na igualdade do universo; reutilizar o arquivo falha no `open('x')`. Não há retomada automática. (`app/colaboradores/services/vinculo.py:910`, `app/escala/management/commands/aplicar_feriado_nasce_da_escala.py:38`, `app/escala/management/commands/aplicar_feriado_nasce_da_escala.py:51`)
- **Efeitos incompletos:** rejulgamento captura falhas; dinheiro e E2 têm efeitos após commit que também podem falhar sem desfazer a flag. Repetir `marcar(False)` depois retorna imediatamente, sem reparar esses efeitos. Contador zero não prova cura completa. (`app/ponto/portas/celula.py:475`, `app/ponto/portas/celula.py:540`, `app/ponto/services/fechamento.py:1818`, `app/colaboradores/services/vinculo.py:932`)
- **Exportadas:** a regeneração ordinária do lote mantém a guarda, pois não passa `apesar_da_lavra`. Entretanto, o `save()` também invalida o previsto dos fechamentos do colaborador, sem filtro de competência exportada: muda `previsto_em`/`atualizado_em`, embora isso não demonstre alteração monetária. (`app/colaboradores/services/vinculo.py:936`, `app/colaboradores/services/vinculo.py:951`, `app/ponto/portas/celula.py:218`, `app/escala/signals.py:138`, `app/ponto/services/fechamento.py:1611`)
- **20/11:** o suplemento só regenera células futuras geradas **já existentes**, na janela calculada. Não cria uma célula ausente. As 16 células de 20/11 com `False` são **[não verificado]**. (`app/ponto/services/cadastro_futuro.py:26`, `app/ponto/services/cadastro_futuro.py:31`, `app/ponto/services/cadastro_futuro.py:43`)

**Bloqueante 5 — relógio fora da borda:** o comando chama o contador duas vezes sem fornecer hoje; cada marcação lê outro hoje depois de salvar; o signal do save lê seu próprio hoje; o rejulgamento interno lê novamente. Uma passagem pela meia-noite pode usar datas distintas no mesmo lote. (`app/escala/management/commands/aplicar_feriado_nasce_da_escala.py:36`, `app/escala/management/commands/aplicar_feriado_nasce_da_escala.py:57`, `app/escala/models.py:943`, `app/colaboradores/services/vinculo.py:944`, `app/ponto/signals.py:63`, `app/ponto/portas/celula.py:464`)

**G. Alcance real do selo AST**

| Construção | Resultado do scanner |
|---|---|
| `from ... import EscalaColaborador as EC`; `m.EscalaColaborador.objects.create` | Reconhece. (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:34`, `app/escala/tests/test_feriado_nasce_da_escala_selo.py:44`) |
| `EC = EscalaColaborador`; manager guardado em variável; manager custom/`_default_manager` | Escapa: nascimento exige `.objects` diretamente no nome reconhecido. (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:90`) |
| `_meta.model`, `__class__` | Escapa ao reconhecimento do modelo. (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:44`, `app/escala/tests/test_feriado_nasce_da_escala_selo.py:90`) |
| Construtor reconhecido sem flag, seguido de `save()` | Pega o construtor. (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:93`) |
| Instância carregada/copied/refresh, `pk=None`, `save()` | Escapa: não acompanha identidade ou `save()`. (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:90`, `app/escala/tests/test_feriado_nasce_da_escala_selo.py:102`) |
| `qs.update(trabalha_em_feriado=...)`, queryset em variável | Pega; não precisa conhecer a origem do queryset. (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:102`) |
| `qs.update(**dados)`, `bulk_update`, escrita por `__dict__` | Escapa às formas examinadas. (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:102`, `app/escala/tests/test_feriado_nasce_da_escala_selo.py:108`) |

**Bloqueante 6 — procedência falsificável:** qualquer `v['trabalha_em_feriado']` é aceito, mesmo vindo de um dict literal; qualquer chamada chamada `feriado_do_trecho` também é aceita, sem conferir módulo ou autoridade. O selo não garante que o valor veio do veredito real. (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:48`)

**O mutante literal morde:** retirar a keyword ou substituí-la por `True` no nascimento atual gera achado. O teste faz essas duas mutações; isso não cobre os escapes acima nem equivale a uma campanha de mutação do juiz. (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:137`, `app/escala/tests/test_feriado_nasce_da_escala_selo.py:143`)

O RED “col148” cria colaborador, vínculo e célula sintéticos. Verifica flag

[truncado em 20000 de 27330 caracteres]

## 13/11 10:50 · sessao 3dc36bb74794 · O168 · verificador · origem=Code

**APROVA o delta — sem bloqueantes demonstrados.** A referência nua deixou de contar como chamada, e os dois pontos de atribuição de erros passaram a usar o mesmo separador. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:288`; `app/chamados/services/validacao.py:142`; `app/chamados/services/validacao.py:510`.)

**A. Contador e fila.** `qs_na_fila` aceita `_base`: repassa-o a `qs_vivos`, que usa esse queryset diretamente e aplica `VIVOS`; depois restringe a `COBRANCA` e exclui silêncio futuro. O juiz da fila é `chamados/services/painel.py::qs_na_fila`. (`app/chamados/services/painel.py:153`; `app/chamados/services/painel.py:158`; `app/chamados/services/painel.py:188`; `app/chamados/services/painel.py:190`.)

O significado **muda**: de recusados ainda vivos para recusados ainda cobrados na fila, excluindo `registrado` e silenciados no futuro. A docstring declara o universo novo. (`app/chamados/catalogo/motor.py:24`; `app/chamados/catalogo/motor.py:29`; `app/chamados/juizes.py:2512`; `app/chamados/juizes.py:2525`.)

O leitor executável localizado é `_estado` do selo, que publica o número como `segurados`; não localizei consumidor de placar. Há também uma referência documental em `_registrar_recusa`, sem chamada ao contador. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:67`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:75`; `app/chamados/models.py:712`.) Consumidores externos à cópia: **[não verificado]**.

**B. Escritores e donos.** A entrada é `erros_proprios` da unidade de materialização mais `res['erros']` da disputa. O import por `models.py` reexporta a implementação de `materializacao.py`; não constitui outro escritor. (`app/chamados/services/validacao.py:486`; `app/chamados/services/validacao.py:492`; `app/chamados/services/validacao.py:493`; `app/chamados/models.py:4143`.)

Na tabela, **S(e)** significa todos os pks cuja marca `Pergunta #<pk>` era substring da mensagem — incluindo prefixos numéricos e perguntas citadas depois. A cura atribui exclusivamente ao primeiro identificador admissível. (`app/chamados/services/validacao.py:113`; `app/chamados/services/validacao.py:116`; `app/chamados/tests/test_bug29_30_log_validacao.py:117`; `app/chamados/tests/test_bug29_30_log_validacao.py:124`.)

| Escritor/caso que alimenta `erros` | Arquivo:linha | Dono antes → depois |
|---|---|---|
| `_ts_tipo_do_plantio`: sem data | `app/chamados/services/materializacao.py:352` | S(e) → `p.pk` |
| Resposta de escape | `app/chamados/services/materializacao.py:361` | S(e) → `p.pk` |
| Default do relógio sem marco correspondente | `app/chamados/services/materializacao.py:377` | S(e) → `p.pk` |
| Hora inválida, incluindo exceção | `app/chamados/services/materializacao.py:421` | S(e) → `p.pk` |
| `_declarar_ausencia_de_pergunta`: sem data | `app/chamados/services/materializacao.py:484` | S(e) → `p.pk` |
| `_materializar_resposta_hora`: sem resposta | `app/chamados/services/materializacao.py:843` | S(e) → `p.pk` |
| Vizinha do mesmo tipo/paridade | `app/chamados/services/materializacao.py:884` | S(e) → `p.pk` |
| Conflito devolvido por `registrar_batida` | `app/chamados/services/materializacao.py:942` | S(e) → `p.pk` |
| `_materializar_resposta_data_hora`: parse inválido | `app/chamados/services/materializacao.py:1010` | S(e) → `p.pk` |
| Conflito no plantio da âncora | `app/chamados/services/materializacao.py:1027` | S(e) → `p.pk` |
| Gate `DATA_EM_DISPUTA` | `app/chamados/services/materializacao.py:1201`; `app/chamados/services/materializacao.py:1205` | S(e) → `p.pk` |
| `_Reprovada`: não gerou batida | `app/chamados/services/materializacao.py:1245`; `app/chamados/services/materializacao.py:1290` | S(e) → `p.pk` |
| `_Reprovada`: C6 ainda considera viva | `app/chamados/services/materializacao.py:1285`; `app/chamados/services/materializacao.py:1290` | S(e) → `p.pk` |
| Outras exceções capturadas pela unidade | `app/chamados/services/materializacao.py:1291`; `app/chamados/services/materializacao.py:1292` | S(e) → `p.pk` |

Todos esses formatos colocam o identificador do dono antes dos conteúdos dinâmicos. Conflitos e exceções são embrulhados depois de `Pergunta #p.pk:`; portanto uma pergunta citada dentro deles não antecede a dona. (`app/chamados/services/materializacao.py:421`; `app/chamados/services/materializacao.py:943`; `app/chamados/services/materializacao.py:1010`; `app/chamados/services/materializacao.py:1028`; `app/chamados/services/materializacao.py:1292`.)

As particularidades são:

- **`__REUSO__Pergunta #p.pk`** escreve temporariamente em `erros`, mas o `finally` remove a mensagem dessa lista e a transfere para `reusos`. Se entregue diretamente ao helper, seria S(e) → `p.pk`. (`app/chamados/services/materializacao.py:872`; `app/chamados/services/materializacao.py:1297`; `app/chamados/services/materializacao.py:1301`; `app/chamados/services/validacao.py:106`.)
- **`DATA_EM_DISPUTA Pergunta #p.pk`** permanece reconhecido: o prefixo não contém outro identificador de pergunta. (`app/chamados/services/materializacao.py:1201`; `app/chamados/tests/test_bug29_30_log_validacao.py:136`.)
- **`DecisaoPergunta #N`** é escrito pelas exceções de imutabilidade de outra entidade. Isolado, antes casava com a pergunta N e seus prefixos; agora não tem dono. Capturado pela unidade, recebe antes o prefixo da pergunta processada. Ocorrência efetiva dessa exceção nesse caminho: **[não verificado]**. (`app/chamados/models.py:3961`; `app/chamados/models.py:3965`; `app/chamados/services/materializacao.py:1292`; `app/chamados/tests/test_bug29_30_log_validacao.py:134`.)
- **Sem pk:** se recebido, não era próprio de pergunta alguma e continua sem dono, entrando em `irmas`. Não identifiquei escritor direto desse formato no fluxo enumerado. (`app/chamados/services/validacao.py:116`; `app/chamados/services/validacao.py:124`; `app/chamados/services/materializacao.py:1392`.)
- **Fallback de `erros_da_pergunta`:** é mensagem de saída, não novo insumo da materialização; abre pelo pk consultado e informa ausência de causa própria. (`app/chamados/services/validacao.py:142`; `app/chamados/services/validacao.py:145`.)

**C. Leitores de `irmas`.** Para mensagens com um único identificador legítimo, sem coincidência de prefixo, permanecem os mesmos itens, textos e ordem. O separador acrescenta o objeto original à lista escolhida. (`app/chamados/services/validacao.py:120`; `app/chamados/services/validacao.py:124`.)

Os leitores não refazem a partição: o log da recusa usa `erros_da_pergunta`; o log das irmãs concatena a lista; `traduzir_recusa` classifica motivos, sem conhecer pk; a tela consome esses resultados. (`app/chamados/services/validacao.py:519`; `app/chamados/services/validacao.py:532`; `app/chamados/catalogo/recusa.py:79`; `app/chamados/catalogo/recusa.py:81`; `app/chamados/services/acoes_disputa.py:34`; `app/chamados/services/acoes_disputa.py:59`.)

**Não há invariância total fora do prefixo numérico:** `Pergunta #5: ... Pergunta #7` deixa de ser próprio da #7. Essa mudança é deliberadamente cobrada pelo teste novo. Antes, a mensagem podia ser própria em duas consultas diferentes; dentro de uma mesma chamada, `in`/`not in` já produziam listas complementares. (`app/chamados/tests/test_bug29_30_log_validacao.py:125`; `app/chamados/tests/test_bug29_30_log_validacao.py:127`; `app/chamados/tests/test_bug29_30_log_validacao.py:129`; `app/chamados/services/validacao.py:113`.)

**Ressalva do aviso:** `aviso_das_irmas` continua coletando todas as menções, não apenas donos. Recebendo aquele erro como irmã da #7, pode anunciar “1 pergunta” com IDs `#5, #7`, incluindo a própria #7. É uma inconsistência demonstrável com o exemplo sintético; ocorrência produtiva: **[não verificado]**. Esse coletor apresenta referências e não constitui outro particionador de erros. (`app/chamados/services/validacao.py:547`; `app/chamados/services/validacao.py:549`; `app/chamados/tests/test_bug29_30_log_validacao.py:127`.)

**D. Lookbehind.** Não perde nenhum escritor legítimo identificado. Veta somente letra ASCII imediatamente anterior; espaço, `_` e dígito são aceitos. Assim, `__REUSO__Pergunta`, `_Pergunta` e `3Pergunta` são reconhecidos; `DecisaoPergunta` é excluído. O uso de `search` encontra o primeiro token admissível, sem exigir início literal da string. (`app/chamados/services/validacao.py:106`; `app/chamados/services/validacao.py:116`; `app/chamados/tests/test_bug29_30_log_validacao.py:134`; `app/chamados/tests/test_bug29_30_log_validacao.py:135`.)

**E. Outro separador por substring.** Na varredura textual da cópia, não localizei outro sítio de produção com essa separação. Os dois pontos migrados delegam ao helper; o regex restante do aviso enumera menções. Ausência de variantes construídas dinamicamente: **[não verificado]**. (`app/chamados/services/validacao.py:142`; `app/chamados/services/validacao.py:510`; `app/chamados/services/validacao.py:547`.)

**F. Classes de reprovação.** Não demonstrei novo juiz paralelo nem campo simultaneamente operando com dois significados: o contador consulta a fila existente, e a atribuição de erros tem implementação compartilhada. A mudança de universo do contador está declarada. (`app/chamados/juizes.py:2512`; `app/chamados/juizes.py:2525`; `app/chamados/services/validacao.py:142`; `app/chamados/services/validacao.py:510`.)

O bloqueante da volta 4 foi curado: o fecho exige `ast.Call`, e o teste rejeita a auxiliar apenas referenciada. Permanece o limite anterior de aceitar uma chamada cujo resultado seja descartado; ele não foi introduzido pelo delta. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:288`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:334`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:389`.)

A catraca mantém a linha na identidade e `EXCECOES` vazio; as fixtures passam a fornecer os campos do juiz, delegar ao corpo canônico e zerar também o texto quando pretendem representar silêncio. (`app/chamados/tests/e4b_c12_ast.py:18`; `app/chamados/tests/e4b_c12_ast.py:29`; `app/chamados/tests/e4b_c12_ast.py:222`; `app/ponto/tests/test_supra_juiz_executor.py:24`; `app/ponto/tests/test_supra_juiz_executor.py:46`; `app/chamados/tests/test_pep_reentrada.py:280`.)

Executei em memória os seis testes puros de atribuição e os três adversariais do AST: passaram. Restaurar a substring reprova os testes de prefixo e segunda citação; restaurar o fecho por referência nua reprova a isca. Há mordida nessas curas. (`app/chamados/tests/test_bug29_30_log_validacao.py:104`; `app/chamados/tests/test_bug29_30_log_validacao.py:117`; `app/chamados/tests/test_bug29_30_log_validacao.py:124`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:369`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:389`.)

Execução integral Django/ORM, banco, logs e identidade Git do HEAD: **[não verificado]**.

## 13/11 10:48 · sessao 85aeb4ade445 · - · - · origem=Ronald (tmux)

APROVA no escopo do delta: o exemplo multipergunta testa deliberadamente a propriedade exclusiva do primeiro identificador; `irmas` não é invariável nesses casos. (`app/chamados/services/validacao.py:116`; `app/chamados/services/validacao.py:123`.)

O aviso que inclui a própria pergunta merece ressalva, mas, sem escritor real alcançável produzindo esse formato, não demonstra bloqueante atual; ocorrência produtiva: [não verificado].

## 13/11 10:47 · sessao 259a91e99da0 · - · - · origem=Ronald (tmux)

G — Selo AST

- Executei somente a função `achados` em memória, sem Django, banco ou arquivos temporários: árvore de produção retornou `[]`; isso confirma apenas o alcance implementado pelo scanner, que percorre `.py` e exclui testes/migrations/logs (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:28`, `:77`).
- Apelido de import de classe (`from ... import EscalaColaborador as EC`) e módulo (`m.EscalaColaborador`) são reconhecidos; atribuição posterior `EC = EscalaColaborador` escapa (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:34`, `:44`).
- Escapam `manager = EscalaColaborador.objects; manager.create(...)`, manager custom/`_default_manager`, `ec._meta.model.objects.create(...)` e `ec.__class__.objects.create(...)`: o nascimento exige `.objects` diretamente ligado ao nome reconhecido (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:90`).
- `qs.update(trabalha_em_feriado=True)` é pego mesmo com queryset em variável; `qs.update(**dados)`, `bulk_update`, alteração por `__dict__`, cópia/refresh com `pk=None` seguida de `save()` escapam. O scanner só examina keyword literal de `update`, `setattr` com string literal e atribuição direta ao atributo; não rastreia `save`, dados nem identidade da instância (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:102`).
- Construtor reconhecido sem flag é pego antes do `save`; construtor aceito seguido de `save` não tem sua procedência revalidada (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:93`).
- A fonte também é falsificável: `v = {'trabalha_em_feriado': True}` ou `evil.feriado_do_trecho(...)` passam fora da porta. Aceitação compara sintaxe/nome, sem provar que `v` veio de `julgar_vinculo` ou que a função pertence ao modelo (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:48`).
- O mutante literal pedido morde: retirar o keyword ou trocá-lo por `True` na âncora da criação produz achado; isso foi confirmado também em memória. O teste existente faz essas duas substituições, mas não muta o juiz para retornar True em 5x2 nem testa os escapes acima (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:137`).
- Portanto há mordida real e cobertura incompleta, não selo completamente vacuoso (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:143`, `:90`, `:102`).

H — Semântica e leitores

**Bloqueante:** a tela exige resposta para personalizado/intermitente, mas a resposta “Não” não torna o feriado folga nesses ciclos. Só 6x1/5x2/semanal integram `CICLOS_FERIADO_VIRA_FOLGA`; o predicado retorna False para os demais independentemente da flag. A precedência mantém trabalho previsto nesses feriados por `not ec.feriado_vira_folga(data)` (`app/templates/colaboradores/partials/painel_vinculo.html:175`, `app/escala/models.py:32`, `:1420`, `app/ponto/precedencia.py:376`).

- 6x1/semanal: a escolha humana tem efeito consistente sobre o previsto e sobre a partição de pagamento, porque os consumidores consultam `feriado_vira_folga`; não encontrei consumidor que leia o `default` do campo para decidir (`app/escala/models.py:1399`, `:1420`, `app/ponto/precedencia.py:377`, `app/ponto/services/fechamento.py:949`, `app/ponto/calculador/alimentacao.py:521`).
- 24x48 passa a nascer True, porém o predicado já ignora sua flag por não ser weekday; True/False não altera essa supressão do feriado. O código adiciona 24x48 à resposta obrigatória True do juiz de nascimento (`app/escala/models.py:906`, `:921`, `:1422`).
- `feriado_do_posto` não lê a flag do vínculo: usa `Posto.opera_em_feriado`; posto fechado veta cobrança inclusive de vínculo que trabalha em feriado. Há veto explícito adicional depois da consulta à precedência, portanto “flag True” não garante cobrança (`app/colaboradores/services/feriado_do_posto.py:67`, `app/ponto/management/commands/detectar_ausencias.py:617`, `:619`, `:629`).
- Contador antigo continua classificando 5x2/6x1 True em “declarados” conforme o posto, embora agora 6x1 True possa resultar de escolha humana independente do posto. É indicador com critério anterior preservado (`app/escala/models.py:950`, `:959`, `:962`).
- O contador novo existe como método/comando, mas o placar continua chamando somente o antigo; a integração do contador novo ao ESTADO/copiloto não foi encontrada na cópia. O ponto concreto de integração existente é `bin/placar_code.sh:298`; registro novo só declara a “casa” no placar (`app/config/crons.py:1227`). Publicação/exibição efetiva do novo contador: [não verificado].

Inventário direto de leituras da flag em produção:

| Uso | Referências |
|---|---|
| Foto para trilha | `app/colaboradores/services/vinculo.py:43` |
| Comparação no_op / herança | `app/colaboradores/services/vinculo.py:145`, `:187` |
| Idempotência / antes da mudança | `app/colaboradores/services/vinculo.py:932`, `:934` |
| Cópias para trechos | `app/colaboradores/services/vinculo.py:1021`, `:1139`, `:1161` |
| Contadores | `app/escala/models.py:945`, `:959` |
| Supressão do previsto no feriado | `app/escala/models.py:1420` |
| Lote: listagem / reversão | `app/escala/management/commands/aplicar_feriado_nasce_da_escala.py:44`, `:52` |
| Sincronização: comparação / listagem | `app/colaboradores/management/commands/sincronizar_feriado_admin.py:53`, `:57` |
| Entrada humana de UI | `app/templates/colaboradores/partials/painel_vinculo.html:176` |

Leituras indiretas relevantes:

| Consumidor | Referências |
|---|---|
| Geração/regeneração de célula | `app/ponto/portas/celula.py:289`, `:911` |
| Precedência | `app/ponto/precedencia.py:366`, `:377` |
| Fechamento | `app/ponto/services/fechamento.py:949` |
| Calculador diário | `app/ponto/calculador/alimentacao.py:521`, `app/ponto/calculador/regras.py:684` |
| Rubrica de exportação, sem leitura direta da flag | `app/folha/export.py:420`, `:422` |

I — Classes presentes/limites

- Campo com significado incompatível com a pergunta da UI para personalizado/intermitente: presente e demonstrável pelo fluxo acima (`app/templates/colaboradores/partials/painel_vinculo.html:175`, `app/escala/models.py:1422`).
- Selo com escapes: presente; o mutante literal morde, mas a alegação de impedir todo nascimento/escrita fora da porta excede a implementação (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:4`, `:90`, `:102`).
- O RED “col148” é fixture sintética, não replay do EC854: cria outro colaborador/vínculo e célula artificial; verifica flag, célula e tipo de dia, mas não uma cobrança/veredito previamente lavrado de produção nem 20/11 (`app/escala/tests/test_feriado_padrao_por_escala.py:188`, `:198`, `:230`). Resultado real col148, suite completa/opus, publicação de censo, lista dos 16, deploy e apply: [não verificado].
- Nenhuma ocorrência de `FERIADO-NASCE-DA-ESCALA`/contador novo foi encontrada em `app/docs`, `CLAUDE.md` ou `bin`; isso não prova inexistência de relato fora da cópia. Censo e listas publicados: [não verificado]. [nao verificado]
- Não executei os testes existentes porque os testes de selo escrevem arquivos temporários e os funcionais escrevem banco, contrariando a regra de somente leitura (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:115`, `app/escala/tests/test_feriado_padrao_por_escala.py:198`).

## 13/11 10:47 · sessao 85aeb4ade445 · - · - · origem=Ronald (tmux)

A: `qs_na_fila` aceita `_base` e entrega a interseção desse queryset com `VIVOS`, `COBRANCA` e ausência de silêncio futuro; `_base` substitui a montagem padrão, sem exigir as anotações do painel. O juiz da fila é `chamados/services/painel.py::qs_na_fila`. (`app/chamados/services/painel.py:153`; `app/chamados/services/painel.py:155`; `app/chamados/services/painel.py:158`; `app/chamados/services/painel.py:188`; `app/chamados/services/painel.py:189`; `app/chamados/services/painel.py:190`.)

O contador muda de “recusados ainda vivos” para “recusados ainda cobrados na fila”: exclui `registrado` e silenciados no futuro. `VIVOS` contém `registrado`, enquanto `COBRANCA` contém somente `aberto` e `em_analise`. A nova docstring declara esse universo. (`app/chamados/catalogo/motor.py:24`; `app/chamados/catalogo/motor.py:29`; `app/chamados/juizes.py:2512`; `app/chamados/juizes.py:2525`.)

O leitor executável encontrado é `_estado`, no selo O168, que publica o retorno como `segurados`; as expectativas continuam sendo zero/um nos cenários existentes. A outra ocorrência produtiva é documentação de `_registrar_recusa`, sem chamada ao contador. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:67`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:75`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:122`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:159`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:187`; `app/chamados/models.py:712`.)

F/pre-opus: o bloqueante da volta 4 foi curado: o fecho agora segue somente `ast.Call` com função nominal definida no módulo; a referência nua `_isca` deixou de contar. Rodei por extração AST, exclusivamente em memória, os três métodos adversariais: passaram. No fonte atual, obtive `raw=False`, `judge=True`, fecho composto por `perguntas_que_seguram_csf` e `pode_executar_csf`; a isca deu `judge=False`. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:288`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:290`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:357`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:360`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:369`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:389`; `app/ponto/supra_juiz.py:558`; `app/ponto/supra_juiz.py:559`; `app/ponto/supra_juiz.py:573`.)

Permanece o limite já descrito na volta 4: uma chamada ao juiz cujo resultado é descartado ainda satisfaz `_pergunta_ao_juiz`; o teste exige existência da chamada, sem vínculo com o resultado. Reproduzi `ch.sem_veredito_entre(); return True` dando `judge=True`. Isso não foi introduzido pela alteração de `ast.Name` para `ast.Call`. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:332`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:334`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:336`; dado fornecido, preverif4.r, item C.)

As nove entradas de `CORPO` correspondem às chamadas e linhas atuais; a mudança atualiza a identidade de linha, sem adicionar exceções. O scanner usa a tupla completa e `EXCECOES` continua vazio. (`app/chamados/tests/e4b_c12_ast.py:18`; `app/chamados/tests/e4b_c12_ast.py:29`; `app/chamados/tests/e4b_c12_ast.py:222`; `app/chamados/tests/e4b_c12_ast.py:223`; `app/chamados/models.py:199`; `app/chamados/models.py:296`; `app/chamados/models.py:450`; `app/chamados/models.py:655`; `app/chamados/models.py:835`; `app/chamados/models.py:853`; `app/chamados/models.py:1489`; `app/chamados/models.py:1666`; `app/chamados/models.py:1686`.)

A fixture do executor fornece os campos reais do juiz e `_D` delega ao corpo canônico, sem copiar seu predicado. Rodei seus seis testes em memória com as funções reais extraídas por AST e módulos sintéticos somente para resolver imports e o patch da tranca: passaram. Execução Django integral: [não verificado]. (`app/ponto/tests/test_supra_juiz_executor.py:24`; `app/ponto/tests/test_supra_juiz_executor.py:45`; `app/ponto/tests/test_supra_juiz_executor.py:58`; `app/ponto/tests/test_supra_juiz_executor.py:62`; `app/ponto/tests/test_supra_juiz_executor.py:79`; `app/chamados/models.py:1899`.)

A fixture PEP agora zera também `resposta_colab`, coerentemente com o juiz que considera texto sem carimbo uma resposta. O teste preserva controles da irmã carimbada e da pergunta em voo sem escrita. (`app/chamados/tests/test_pep_reentrada.py:280`; `app/chamados/tests/test_pep_reentrada.py:294`; `app/chamados/tests/test_pep_reentrada.py:296`; `app/chamados/models.py:1900`; `app/chamados/models.py:1901`.)

Não demonstrei bloqueante novo em A nem nessa parte de F: o contador passou a consultar o juiz existente e a fixture passou a delegar ao predicado existente. (`app/chamados/juizes.py:2525`; `app/ponto/tests/test_supra_juiz_executor.py:46`.)

## 13/11 10:47 · sessao 5f5a2ad0669a · - · - · origem=Ronald (tmux)

A/C/E — achados confirmados pela cópia:

- **Bloqueante: troca retroativa deixa flag incompatível.** `corrigir_vinculo_retroativo` troca `tipo_escala` e salva apenas tipo/âncora/fim, sem consultar juiz nem atualizar flag. Assim um vínculo 6x1 True corrigido para 5x2 continua True. O comando chama essa função na correção “no lugar”. (`app/colaboradores/services/vinculo.py:1024`, `app/colaboradores/services/vinculo.py:1027`, `app/colaboradores/services/vinculo.py:1030`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:212`)

- **Bloqueante: recusa do aceite não restaura a flag.** O snapshot guarda pk/tipo/início/fim/ativa, sem `trabalha_em_feriado`; a reversão restaura tipo sem flag. Caso concreto derivável: reaproveitar vínculo 5x2 False como 12x36 True em modo aceite; recusar restaura 5x2, mas deixa True. Troca somente da flag também não é desfeita pela recusa. (`app/colaboradores/services/vinculo.py:108`, `app/colaboradores/services/vinculo.py:315`, `app/colaboradores/services/vinculo.py:317`, `app/colaboradores/services/vinculo.py:1078`, `app/chamados/services/acoes_chamado.py:143`, `app/chamados/services/acoes_chamado.py:149`)

Inventário de nascimento encontrado na produção, inclusive procurando aliases de importação por AST em memória:

| Sítio | Origem da flag |
|---|---|
| `executar_vinculo`, criação | `v['trabalha_em_feriado']`, vindo de `_feriado_do_veredito`. (`app/colaboradores/services/vinculo.py:144`, `app/colaboradores/services/vinculo.py:329`, `app/colaboradores/services/vinculo.py:338`) |
| `executar_vinculo`, reaproveitamento | Recebe o mesmo valor do veredito. (`app/colaboradores/services/vinculo.py:315`, `app/colaboradores/services/vinculo.py:317`) |
| `criar_trecho_retroativo` | `feriado_do_trecho(tipo_destino, flag_original)`. (`app/colaboradores/services/vinculo.py:1017`, `app/colaboradores/services/vinculo.py:1021`) |
| `ajustar_vinculo_pelo_sistema` | `feriado_do_trecho(tipo_derivado, flag_original)`. (`app/colaboradores/services/vinculo.py:1136`, `app/colaboradores/services/vinculo.py:1139`) |
| `desfazer_ajuste_do_sistema`, renascimento | `feriado_do_trecho(tipo_antigo, flag_antiga)`. (`app/colaboradores/services/vinculo.py:1158`, `app/colaboradores/services/vinculo.py:1161`) |
| `desfazer_ajuste_do_sistema`, reabertura | Reativa o antigo e mantém a flag antiga sem juiz. (`app/colaboradores/services/vinculo.py:1149`, `app/colaboradores/services/vinculo.py:1154`) |
| `corrigir_vinculo_retroativo`, troca tipo | Mantém flag anterior sem juiz — falha acima. (`app/colaboradores/services/vinculo.py:1027`, `app/colaboradores/services/vinculo.py:1030`) |
| `reverter_para_snapshot`, troca tipo | Mantém flag atual sem restaurar flag anterior — falha acima. (`app/colaboradores/services/vinculo.py:1078`) |

Nos quatro `create` encontrados, nenhum usa diretamente o default. O default ainda existe como True, sem enforcement no modelo para impedir caminhos não capturados pelo selo. (`app/colaboradores/services/vinculo.py:329`, `app/colaboradores/services/vinculo.py:1017`, `app/colaboradores/services/vinculo.py:1136`, `app/colaboradores/services/vinculo.py:1158`, `app/escala/models.py:896`)

`feriado_do_trecho` trata qualquer booleano herdado como “escolha humana”, mas não consulta prova da escolha: para juiz None retorna `bool(escolha)`. Portanto legado que nasceu pelo default pode ser reproduzido como escolha. (`app/escala/models.py:928`, `app/escala/models.py:933`)

Chamadores de `julgar_vinculo`/`executar_vinculo`:

| Chamador | Passa flag? Efeito |
|---|---|
| Tela Vincular | Encaminha `request.POST`; template tem radios 1/0 sem pré-seleção. (`app/colaboradores/views.py:411`, `app/colaboradores/views.py:423`, `app/templates/colaboradores/partials/painel_vinculo.html:176`, `app/templates/colaboradores/partials/painel_vinculo.html:177`) |
| Chat, prévia | Não: `_post` só monta template/início/modo/âncora; erro vira lista `erros`. (`app/colaboradores/services/troca_pelo_chat.py:11`, `app/colaboradores/services/troca_pelo_chat.py:58`, `app/colaboradores/services/troca_pelo_chat.py:59`) |
| Chat, execução | Não; mesma montagem. Novo vínculo ou troca para ciclo sem padrão passa a ser recusado; mesmo ciclo pode herdar. Não há argumento nessa interface para fornecer escolha. (`app/colaboradores/services/troca_pelo_chat.py:64`, `app/colaboradores/services/troca_pelo_chat.py:72`, `app/colaboradores/services/troca_pelo_chat.py:75`, `app/colaboradores/services/vinculo.py:177`) |
| Régua/realocar | Não: dict sem flag. Recusa captura ValueError; mesma restrição em novos/novos ciclos sem padrão. (`app/escala/regua_defesa.py:313`, `app/escala/regua_defesa.py:316`, `app/escala/regua_defesa.py:317`, `app/escala/regua_defesa.py:321`) |
| Comando `censo_fase_12x36` | Não: dict tem template/início/âncora; 12x36 recebe True do juiz. (`app/ponto/management/commands/censo_fase_12x36.py:419`, `app/ponto/management/commands/censo_fase_12x36.py:422`, `app/ponto/management/commands/censo_fase_12x36.py:423`, `app/escala/models.py:921`) |

Chamador direto adicional por API/importação não encontrado nas buscas estáticas; caminhos dinâmicos permanecem [não verificado]. O Django admin de vínculo é somente leitura. (`app/escala/admin.py:18`)

E:

- Mesmo valor em memória retorna antes de save/regeneração/log. Não há releitura/trava do valor persistido para validar idempotência com instância stale ou concorrência. (`app/colaboradores/services/vinculo.py:932`, `app/colaboradores/services/vinculo.py:935`)
- `autor` autenticado ganha `_ReqDoAutor.user`, chega em `evento(... ator=request.user)` e vira `LogAuditoria.usuario`. (`app/colaboradores/services/vinculo.py:19`, `app/colaboradores/services/vinculo.py:939`, `app/core/auditoria.py:24`, `app/core/observ.py:118`, `app/core/observ.py:135`)
- `autor=None`: dentro de request, ContextVar fornece usuário; fora de request, usuário fica None e `ator` identifica o processo/comando. (`app/core/observ.py:108`, `app/core/observ.py:113`, `app/core/observ.py:117`, `app/core/observ.py:123`)
- `sincronizar_feriado_admin` chama sem autor e sem motivo; não tem log próprio nessa aplicação, portanto não encontrei duplicação explícita. Ele continua podendo alterar 6x1 pelo cargo, sem resposta humana DP: o juiz None leva ao booleano do cargo. (`app/colaboradores/management/commands/sincronizar_feriado_admin.py:51`, `app/colaboradores/management/commands/sincronizar_feriado_admin.py:61`, `app/escala/models.py:934`)
- Auditoria é best-effort: falha pode ser engolida e a flag ainda confirmar. `@atomic` não garante que a trilha exista se seu escritor engoliu a falha. (`app/colaboradores/services/vinculo.py:910`, `app/core/auditoria.py:17`, `app/core/auditoria.py:25`, `app/core/observ.py:105`, `app/core/observ.py:133`, `app/core/observ.py:139`)
- Relógio oculto: `ec.save` dispara `_nucleo_post_save_escala` e `_recompute_escala`, que lê outro `timezone.localdate()` antes do `hoje` da porta; portanto não há efetivamente um único relógio no ato completo. (`app/colaboradores/services/vinculo.py:936`, `app/colaboradores/services/vinculo.py:944`, `app/ponto/signals.py:63`, `app/ponto/signals.py:72`)

## 13/11 10:47 · sessao 3e37ce6f8cdc · - · - · origem=Ronald (tmux)

D/F — achados e limites:

- **Troca de tipo fora do juiz:** `corrigir_vinculo_retroativo` grava `tipo_escala`, âncora e fim, preservando a flag anterior sem consultar regra ou escolha. Pode produzir 5x2 True ou 6x1 herdando valor de outro ciclo. `reverter_para_snapshot` também restaura tipo sem restaurar/rejulgar flag. (`app/colaboradores/services/vinculo.py:1024-1030`, `:1078-1079`.)
- **Herança sem prova de escolha humana:** `feriado_do_trecho` usa a regra determinística; quando o juiz devolve None, executa `bool(escolha)`, sem conferir ciclo de origem nem proveniência humana. Logo trecho retroativo novo 12x36 True → 6x1 nasce True sem resposta DP. O caminho principal recusaria essa mesma troca sem escolha. (`app/escala/models.py:928-934`; `app/colaboradores/services/vinculo.py:1017-1021`, `:178-190`.)

**D — passado/exportadas**

- `criar_trecho_retroativo` realmente alimenta regeneração do passado: comando cria trecho nas linhas 194–197 e chama regeneração de `piso` até `min(fim, hoje)` nas linhas 223–239; chama também suplemento futuro nas linhas 242–245. (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:194`, `:223`, `:242`.)
- As células exportadas/publicadas ficam protegidas por padrão: `_competencias_exportadas` pergunta ao juiz `competencia_fechada_para_celula`; este considera TXT entregue e holerite do colaborador com lote publicado; regeneração pula os dias dessas janelas e preserva origem humana. (`app/ponto/management/commands/gerar_celulas.py:160-188`; `app/folha/export.py:617-628`; `app/ponto/portas/celula.py:218-221`, `:284-297`.)
- Há exceção preexistente, explícita: comando aceita `--apesar-da-lavra`, passa-a à regeneração; valor não vazio esvazia `_exp`, permitindo reescrever exportada. Não confundir essa possibilidade com comportamento padrão ou execução comprovada. (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:42-44`, `:231-234`; `app/ponto/portas/celula.py:218-221`.)
- Ajuste automático só futuro: aplica exige `inicio > hoje`, regenera desde início; desfazer calcula `volta = max(inicio, hoje+1)` e regenera desde volta. Portanto esses chamadores não regeneram passado. (`app/escala/services/escala_auto_executor.py:98-108`, `:143-148`.)
- Criar/desfazer usam `feriado_do_trecho`; desfazer antes de valer apaga novo e reativa antigo, sem passar novamente pelo juiz. (`app/colaboradores/services/vinculo.py:1136-1140`, `:1149-1162`.)

**F — lote**

- Universo é **vínculo que cobre hoje**, colaborador ativo, 5x2 True, independentemente de `ativa=True`: inativo com fim cobrindo hoje pode entrar; ativo futuro fica fora. Isso não equivale automaticamente aos “16 ativos” da sonda. Lista real coincidente com os 16: **[não verificado]**. (`app/escala/models.py:937-947`; `app/escala/alimentacao.py:219-225`.)
- `--esperado` é comparado exatamente ao universo; diferença para antes de escrita. Imprime ec/col/tipo/antes; isso não prova publicação anterior no RELATO. Publicação dos 16 por colaborador e censo pré-código: **[não verificado]**. (`app/escala/management/commands/aplicar_feriado_nasce_da_escala.py:35-47`.)
- Reversão JSON completa é escrita e arquivo fechado antes da primeira mutação; modo `x` impede sobrescrever arquivo existente. Não há `fsync` nem diário de progresso/reversão executável. (`app/escala/management/commands/aplicar_feriado_nasce_da_escala.py:51-55`.)
- Há transação **por vínculo**, não por lote: `marcar` é `@transaction.atomic`; loop do comando não tem transação externa. Queda após N commits deixa N aplicados, restantes intactos e reversão contendo todos. Repetir lista original falha na comparação; reutilizar JSON falha no `open('x')`. Retomada exige conferir restante e nova lista/arquivo; considero limitação operacional, não corrupção inerente. (`app/colaboradores/services/vinculo.py:910-911`; `app/escala/management/commands/aplicar_feriado_nasce_da_escala.py:35-40`, `:51-58`.)
- A atomicidade não inclui sucesso de todos os efeitos: julgamento captura falhas; dinheiro é `on_commit` e engole exceção; E2 é outro `on_commit`, protegido por `engolir`. Portanto flag False/contador zero pode coexistir com rejulgamento, dinheiro ou encerramento ainda pendentes. Repetir `marcar` com flag já False retorna imediatamente e não repara efeitos que falharam. (`app/ponto/portas/celula.py:460-482`, `:528-540`; `app/ponto/services/fechamento.py:1798-1827`; `app/core/observ.py:85-91`; `app/colaboradores/services/vinculo.py:932-933`.)
- Lote mantém guarda de exportada: não passa `apesar_da_lavra`; usa padrão vazio da porta tanto no passado quanto no suplemento futuro. Suplemento mantém `recalculo=True`, pois `marcar` não sobrescreve esse argumento. (`app/colaboradores/services/vinculo.py:951-960`; `app/ponto/portas/celula.py:112-114`, `:218-221`, `:284-288`.)
- Futuro alcança **somente células geradas já existentes**, dentro de janela corrente; não cria 20/11 ausente. Filtra dono pelo juiz quando `substitui_dono=False`. Todos os 16 com célula20/11 False: **[não verificado]**. (`app/ponto/services/cadastro_futuro.py:26-45`; `app/ponto/management/commands/gerar_celulas.py:290-300`.)
- **Relógio fora da borda presente:** contador escolhe hoje internamente; comando o chama antes e depois; cada `marcar` lê outro hoje após salvar; descendente `_o_chao_do_dia_mudou` relê hoje para decidir passado/futuro. Lote atravessando meia-noite pode comparar universos diferentes e operar com datas diferentes por vínculo. (`app/escala/models.py:943`; `app/escala/management/commands/aplicar_feriado_nasce_da_escala.py:36`, `:57`; `app/colaboradores/services/vinculo.py:944`; `app/ponto/portas/celula.py:464-469`.)
- Não identifiquei inversão nova de trava nesses caminhos: `marcar.save()` adquire escrita no vínculo antes do suplemento travar células; suplemento usa `select_for_update` nas células. Ausência global de inversão fora do trecho auditado: **[não verificado]**. (`app/colaboradores/services/vinculo.py:935-936`, `:957`; `app/ponto/services/cadastro_futuro.py:30-32`.)

O teste RED usa fixture sintética e verifica `fatos_do_dia`, flag, célula e log; não demonstra caso EC854 real, 16 aplicados, fechamento real ou20/11. Execução GREEN da suíte: **[não verificado]**. (`app/escala/tests/test_feriado_padrao_por_escala.py:230-247`.)

## 13/11 10:47 · sessao 864c700c89c1 · - · - · origem=Ronald (tmux)

C. Para erros com um único `Pergunta #N`, sem coincidência de prefixo numérico, o separador preserva conteúdo e ordem das listas; os consumidores recebem os mesmos textos. O helper central apenas escolhe a lista e acrescenta o item original. (`app/chamados/services/validacao.py:120`; `app/chamados/services/validacao.py:124`.)

Os leitores identificados são:

- **Recusa na tela:** traduz `r['erros'][0]`, sem analisar proprietário. (`app/chamados/services/acoes_disputa.py:32`; `app/chamados/services/acoes_disputa.py:34`.)
- **Tradutor:** classifica o motivo por marcas textuais; não conhece pk nem separa perguntas. (`app/chamados/catalogo/recusa.py:77`; `app/chamados/catalogo/recusa.py:81`; `app/chamados/catalogo/recusa.py:88`.)
- **Log da recusa:** usa `erros_da_pergunta`, portanto passa pelo mesmo separador novo. (`app/chamados/services/validacao.py:515`; `app/chamados/services/validacao.py:519`; `app/chamados/services/validacao.py:142`.)
- **Log das irmãs:** concatena a lista recebida, sem reclassificar. (`app/chamados/services/validacao.py:527`; `app/chamados/services/validacao.py:532`.)
- **Aviso das irmãs na tela:** chama `aviso_das_irmas(r.get('irmas'))`; o resultado acompanha a afirmação de ponto gravado. (`app/chamados/services/acoes_disputa.py:58`; `app/chamados/services/acoes_disputa.py:61`.)
- **Envelopes:** fio entrega a mensagem ao `resposta_render`; cartão entrega a mensagem no `HX-Trigger`; ambos preservam a mensagem montada. (`app/chamados/services/fio.py:494`; `app/chamados/services/fio.py:498`; `app/core/respostas.py:85`; `app/chamados/views.py:404`; `app/chamados/views.py:407`.)

Há mudança além dos prefixos numéricos: `Pergunta #5: ... Pergunta #7` antes era próprio da #7 por substring; agora pertence à #5 e vira irmã para #7. O teste novo assume explicitamente essa mudança. Em uma única chamada antiga, `in`/`not in` já eram complementares; a duplicação ocorria entre consultas separadas para #5 e #7, não nas duas listas da mesma chamada. (`app/chamados/tests/test_bug29_30_log_validacao.py:125`; `app/chamados/tests/test_bug29_30_log_validacao.py:127`; `app/chamados/tests/test_bug29_30_log_validacao.py:129`; antes: diff fornecido, sem arquivo:linha [não verificado].)

**Ponto residual:** `aviso_das_irmas` continua extraindo **todas as menções**, não somente o dono. Para aquele erro recebido como irmã da #7, produz ids `5,7` e pode dizer “1 pergunta ... segue sem gravar (#5, #7)”, incluindo a própria #7. Reproduzi a extração em memória; existência desse texto exato em produção: [não verificado]. O aviso permanece um coletor de menções, embora o separador passe a trabalhar com proprietário único. (`app/chamados/services/validacao.py:547`; `app/chamados/services/validacao.py:549`; `app/chamados/tests/test_bug29_30_log_validacao.py:127`.)

E. Busca global por `Pergunta #`, `marca in str` e `marca not in str` não localizou outro separador de produção por substring na cópia; ausência de formas construídas dinamicamente: [não verificado]. Os dois consumidores migrados delegam ao helper único; o regex de `aviso_das_irmas` coleta IDs para apresentação, sem particionar erros. (`app/chamados/services/validacao.py:142`; `app/chamados/services/validacao.py:510`; `app/chamados/services/validacao.py:547`.)

## 13/11 10:47 · sessao ed9334c48e11 · - · - · origem=Ronald (tmux)

O fluxo de produção é: `validar_pergunta` recebe `erros_proprios` de `_materializar_uma` e soma `res['erros']`; a função pública importada de `models.py` é apenas reexportação de `materializacao.py`. `erros_da_pergunta` só é chamado nesse mesmo fluxo, nos retornos/log da recusa. (`app/chamados/services/validacao.py:484`; `app/chamados/services/validacao.py:492`; `app/chamados/services/validacao.py:493`; `app/chamados/services/validacao.py:500`; `app/chamados/services/validacao.py:519`; `app/chamados/services/validacao.py:524`; `app/chamados/models.py:4143`.)

Todos os escritores que alimentam essa lista:

| Escritor/caso | Evidência | Dono antigo → novo |
|---|---|---|
| `_ts_tipo_do_plantio`: sem data | `app/chamados/services/materializacao.py:352` | `p.pk` e eventuais prefixos numéricos/citações → só `p.pk` |
| Escape não vira batida | `app/chamados/services/materializacao.py:361` | Mesma regra |
| Default do relógio | `app/chamados/services/materializacao.py:377` | Mesma regra |
| Hora inválida, incluindo texto de exceção | `app/chamados/services/materializacao.py:421` | Mesma regra |
| `_declarar_ausencia_de_pergunta`: sem data | `app/chamados/services/materializacao.py:484` | Mesma regra |
| `_materializar_resposta_hora`: sem resposta | `app/chamados/services/materializacao.py:843` | Mesma regra |
| Vizinha do mesmo tipo/paridade | `app/chamados/services/materializacao.py:884` | Mesma regra |
| Conflito retornado por `registrar_batida`, embrulhado | `app/chamados/services/materializacao.py:942` | Mesma regra |
| `_materializar_resposta_data_hora`: parse inválido, incluindo exceção | `app/chamados/services/materializacao.py:1010` | Mesma regra |
| Conflito da âncora, embrulhado | `app/chamados/services/materializacao.py:1027` | Mesma regra |
| Gate `DATA_EM_DISPUTA Pergunta #p.pk` | `app/chamados/services/materializacao.py:1201`; `app/chamados/services/materializacao.py:1205` | Mesma regra; prefixo é reconhecido |
| `_Reprovada`: não gerou batida | `app/chamados/services/materializacao.py:1245`; `app/chamados/services/materializacao.py:1290` | Mesma regra |
| `_Reprovada`: C6 ainda considera viva | `app/chamados/services/materializacao.py:1285`; `app/chamados/services/materializacao.py:1290` | Mesma regra |
| Qualquer outra exceção dentro do `try`, embrulhada como `Pergunta #p.pk: {e}` | `app/chamados/services/materializacao.py:1291`; `app/chamados/services/materializacao.py:1292` | Mesma regra |

Todos esses erros têm o `Pergunta #p.pk` do dono como primeiro token desse formato; textos dinâmicos, conflitos e exceções entram depois dele. Os ramos recebem a mesma lista, e o coletor retorna essa lista diretamente. (`app/chamados/services/materializacao.py:1227`; `app/chamados/services/materializacao.py:1232`; `app/chamados/services/materializacao.py:1372`; `app/chamados/services/materializacao.py:1382`; `app/chamados/services/materializacao.py:1392`.)

O caso `__REUSO__Pergunta #p.pk` é escritor temporário de `erros`, porém o `finally` o remove de `erros` e o transfere para `reusos` sem o marcador; portanto ele não chega normalmente à separação de erros. Caso fosse entregue ao helper, tanto a substring antiga quanto a regex nova reconhecem `p.pk`; só a antiga também reconheceria prefixos/citações. (`app/chamados/services/materializacao.py:872`; `app/chamados/services/materializacao.py:1296`; `app/chamados/services/materializacao.py:1301`; `app/chamados/services/validacao.py:106`.)

Os textos sem `Pergunta` das recusas de competência/colisão vêm de `registrar_batida`, mas entram em `erros` envolvidos pelo prefixo do dono. `DecisaoPergunta #N` pode ser texto de exceção de outra entidade, mas o `except Exception` também o envolveria por `Pergunta #p.pk:`; uma instância real dessa exceção nesse caminho: **[não verificado]**. (`app/ponto/registro_batida.py:135`; `app/ponto/registro_batida.py:137`; `app/ponto/registro_batida.py:146`; `app/chamados/services/materializacao.py:943`; `app/chamados/services/materializacao.py:1028`; `app/chamados/services/materializacao.py:1292`; `app/chamados/models.py:3961`; `app/chamados/models.py:3965`.)

Nenhum escritor identificado perde reconhecimento pelo lookbehind. Ele só veta letra ASCII imediatamente antes de `Pergunta`; `_`, espaço e dígito são aceitos. Executei os helpers extraídos por AST em memória, sem Django, confirmando `__REUSO__Pergunta #9→9`, `DATA_EM_DISPUTA Pergunta #8→8`, `_Pergunta #7→7`, `3Pergunta #6→6`, `DecisaoPergunta #5→None`, `Pergunta #12: DecisaoPergunta #123→12`, erro de `#5` citando `#7→5` e texto sem pk→`None`. (`app/chamados/services/validacao.py:106`; `app/chamados/services/validacao.py:116`; `app/chamados/services/validacao.py:117`.)

Uma mensagem sem pk, se fornecida externamente, já não era própria de nenhuma pergunta e continua sem dono (`None`), ficando em `irmas`; a mudança real é excluir prefixos numéricos, citações posteriores e `DecisaoPergunta` isolado. Escritor de produção que entregue erro sem dono diretamente nesse fluxo não foi encontrado; ausência fora do fluxo estático auditado: **[não verificado]**. (`app/chamados/services/validacao.py:106`; `app/chamados/services/validacao.py:120`; `app/chamados/services/validacao.py:124`.)

## 13/11 09:34 · sessao 25a3a732a702 · O168 · censo · origem=Code

A triagem aponta **3 ERROR curadas pelo delta, 5 deslocamentos, 1 teste a atualizar por mudança deliberada, 3 falhas da entrega da obra e 1 defeito preexistente com causa exata da ocorrência ainda [nao verificado]**. As referências e ressalvas por falha seguem abaixo. [nao verificado]

| Falha | Veredito | Causa e delta |
|---|---|---|
| `test_resposta_direta_barra` | **CURADA-PELO-DELTA** | O novo executor usa o juiz, que exige campos ausentes em `_P`. O delta acrescenta campos, PK e isolamento da tranca. `app/ponto/tests/test_supra_juiz_executor.py:14`; `app/ponto/supra_juiz.py:559`; `app/chamados/models.py:1899`. |
| `test_resposta_na_disputa_barra` | **CURADA-PELO-DELTA** | O novo executor chama `respostas_sem_veredito()`, ausente em `_D`. O delta acrescenta a delegação ao juiz real. `app/ponto/tests/test_supra_juiz_executor.py:27`; `app/ponto/supra_juiz.py:558`. |
| `test_sem_conversa_executa` | **CURADA-PELO-DELTA** | Mesmo método ausente em `_D`, antes de julgar a pergunta sem resposta. `app/ponto/tests/test_supra_juiz_executor.py:39`; `app/ponto/supra_juiz.py:558`. |
| `test_MORDE_P_em_voo_COM_destino_nao_vivo_o_laco_sem_credor_pula` | **DO-DIFF-TESTE-A-ATUALIZAR** | O fixture conserva texto sem carimbo; a obra passou deliberadamente de carimbo **E** texto para carimbo **OU** texto. O HX reabre o pai e impede o laço sem credor. `app/chamados/tests/test_pep_reentrada.py:276`; `app/chamados/juizes.py:2035`; `app/chamados/services/disputa_emissao.py:2009`. Delta não cura. |
| `test_03_a_admin_le_gravado_e_o_aviso_das_irmas` | **PRE-EXISTENTE**, com ressalva abaixo | A porta e a apresentação envolvidas não mudaram nos hunks fornecidos. Há matching por substring suscetível ao erro fictício `#999999`; colisão efetiva nesta execução: **[nao verificado]**. `app/chamados/services/validacao.py:487`; `app/chamados/tests/test_validar_diz_o_seu.py:19`. Delta não cura. |
| `test_toda_contagem_por_status_de_pendencia_le_o_juiz_ou_esta_declarada` | **DO-DIFF-CODIGO-ERRADO** | Função nova conta diretamente por `VIVOS`, precisamente o caminho que o selo exige passar pelo juiz ou declarar. `app/chamados/juizes.py:2523`; `app/core/tests/test_tripwire_contagem_de_pendencia.py:370`. Delta não cura. |
| `test_MORDE_o_mmd_commitado_bate_com_o_codigo` | **DO-DIFF-CODIGO-ERRADO** | Artefatos gerados ficaram inconsistentes com o registro de juízes. `app/core/juizes.py:294`; `app/docs/ARQUITETURA.mmd:340`; `app/docs/MAPA.md:88`. Delta não cura. |
| `test_MORDE_uma_aresta_a_mais_no_mmd_derruba_o_selo` | **DO-DIFF-CODIGO-ERRADO** | Após restaurar o original, o teste reencontra a mesma divergência preexistente naquela entrega da obra. `app/core/tests/test_selo_diagrama_do_codigo.py:95`; `app/core/tests/test_selo_diagrama_do_codigo.py:98`. Delta não cura. |
| Selo: `ChamadoColaborador.analisar` | **DESLOCAMENTO** | Tabela: **836**; escritor: **853**. `app/chamados/tests/e4b_c12_ast.py:24`; `app/chamados/models.py:853`. Delta não cura. |
| Selo: `ChamadoColaborador.cancelar` | **DESLOCAMENTO** | Tabela: **818**; escritor: **835**. `app/chamados/tests/e4b_c12_ast.py:23`; `app/chamados/models.py:835`. Delta não cura. |
| Selo: `ChamadoColaborador.encerrar` | **DESLOCAMENTO** | Tabela: **1649**; escritor: **1666**. `app/chamados/tests/e4b_c12_ast.py:26`; `app/chamados/models.py:1666`. Delta não cura. |
| Selo: `ChamadoColaborador.encerrar_por_competencia_trancada` | **DESLOCAMENTO** | Tabela: **1669**; escritor: **1686**. `app/chamados/tests/e4b_c12_ast.py:27`; `app/chamados/models.py:1686`. Delta não cura. |
| Selo: `ChamadoColaborador.reabrir` | **DESLOCAMENTO** | Tabela: **1472**; escritor: **1489**. `app/chamados/tests/e4b_c12_ast.py:25`; `app/chamados/models.py:1489`. Delta não cura. |

**As três ERROR do executor.** O hunk responsável é `ponto/supra_juiz.py @@ -536,15 +549,25 @@`: antes bastavam `perguntas.all()` e `respondida_em`; agora o executor consulta `respostas_sem_veredito()` e `sem_veredito_entre()`. Os doubles conservaram a interface antiga. A pergunta mudou deliberadamente de “há carimbo de resposta?” para “há resposta **sem veredito**, não dispensada pela tranca?”. Resposta validada ou materializada libera; texto sem carimbo também pode segurar. `app/ponto/supra_juiz.py:552`; `app/ponto/supra_juiz.py:563`; `app/chamados/models.py:1898`; `app/docs/CORTES.json:1414`.

Os três asserts continuam corretos nos seus casos sem veredito; quem precisava atualização eram os doubles. O delta fornece os campos e a delegação exigidos por esse caminho. A verificação das funções reais extraídas por AST, com os doubles do delta, confirmou os seis casos em memória; execução Django e passagem da suíte completa após o delta: **[nao verificado]**. `app/ponto/tests/test_supra_juiz_executor.py:39`; `app/chamados/models.py:1899`; `app/ponto/supra_juiz.py:558`.

**A irmã fora de voo.** O hunk causal é `chamados/juizes.py @@ -2019,19 +2019,20 @@`, não a guarda nova de fechamento do pai. O fixture deixa ambas as perguntas com `resposta_colab='07:00'`, embora zere `respondida_em` e `validada_em`; seu comentário “sem resposta por validar” deixou de corresponder ao novo predicado. `app/chamados/tests/test_pep_reentrada.py:277`; `app/chamados/tests/test_pep_reentrada.py:278`; `app/chamados/models.py:1900`.

Com destino na P, ela entra no universo inicial, é pulada por estar em voo, mas sua disputa participa do HX. O HX consulta o juiz alterado, encontra texto sem veredito e reabre o pai. Depois, o laço sem credor pula a disputa porque o pai está vivo — **antes de chegar à irmã**. Portanto, estar fora de voo não basta para ela receber carimbo. `app/chamados/services/disputa_emissao.py:1906`; `app/chamados/services/disputa_emissao.py:1960`; `app/chamados/services/disputa_emissao.py:2003`; `app/chamados/services/disputa_emissao.py:2011`; `app/chamados/services/disputa_emissao.py:2033`.

Na base fornecida, o predicado exigia `respondida_em` não nulo e texto; essas perguntas ficavam fora dele. A obra passou propositalmente a proteger também texto sem carimbo, e há teste explícito dessa mudança. Para continuar perguntando apenas pela guarda de voo, esse fixture precisa representar silêncio também no texto; a retenção de texto sem carimbo merece permanecer coberta separadamente. `app/chamados/juizes.py:2022`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:252`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:263`.

**`test_validar_diz_o_seu`, test_03.** A pergunta do teste permanece: “a gravação da própria pergunta aparece como sucesso, enquanto o erro da irmã vira aviso separado?”. A obra não declara mudança dessa pergunta. `app/chamados/tests/test_validar_diz_o_seu.py:3`; `app/chamados/tests/test_validar_diz_o_seu.py:87`.

A mensagem observada **não prova competência realmente aprovada/exportada**: o teste injeta exatamente um erro dessa natureza, e a casca traduz o primeiro erro retornado. `app/chamados/tests/test_validar_diz_o_seu.py:19`; `app/chamados/tests/test_validar_diz_o_seu.py:47`; `app/chamados/tests/test_validar_diz_o_seu.py:85`; `app/chamados/services/acoes_disputa.py:34`.

Existe um defeito independente do diff: `marca in str(e)` considera `Pergunta #9` parte de `Pergunta #999999`. Assim, PKs **9, 99, 999, 9999 e 99999** classificariam a irmã fictícia como erro próprio, podendo produzir `nao_gravou` mesmo com materialização concluída. Outro ramo produz esse estado quando há erros e `materializada_em` continua nulo. O dado não informa qual ramo ocorreu. `app/chamados/services/validacao.py:487`; `app/chamados/services/validacao.py:490`; `app/chamados/services/validacao.py:504`.

A porta real lê **timestamp → competência do fato → `FechamentoMensal.status`**; não consulta “hoje” nessa decisão. O fixture mistura data fixa de junho com `timezone.now()`, mas atribuir a ocorrência ao relógio permanece **[nao verificado]**. O gate novo também libera imediatamente a categoria desse fixture. `app/ponto/janelas.py:84`; `app/ponto/janelas.py:87`; `app/chamados/tests/test_validar_diz_o_seu.py:33`; `app/chamados/tests/test_validar_diz_o_seu.py:38`; `app/chamados/models.py:716`.

Logo, **PRE-EXISTENTE** identifica o defeito e a cadeia não alterados; provar a mesma ocorrência na base, com o mesmo PK e estado de materialização, permanece **[nao verificado]**.

**O contador e o universo.** O hunk novo `chamados/juizes.py @@ -2505,3 +2506,18 @@` introduziu a contagem crua. Para preservar **os candidatos da trilha**, a adaptação é passar essa seleção como `_base`, sem recalcular conversa nem acrescentar outra seleção de pais. `app/chamados/juizes.py:2520`; `app/chamados/services/painel.py:153`; `app/chamados/services/painel.py:188`.

```python
return qs_na_fila(
    _base=ChamadoColaborador.objects.filter(pk__in=_ids)
).count()
```

Isso preserva os candidatos, **mas não garante o mesmo total de todos os VIVOS**: `VIVOS` inclui `registrado`; o juiz da fila exclui registrados e silenciados no futuro. Preservar literalmente esse total e usar somente `qs_na_fila` são requisitos incompatíveis no código atual. A inexistência desses estados entre os pais de produção é **[nao verificado]**. `app/chamados/catalogo/motor.py:24`; `app/chamados/services/painel.py:189`.

**Os cinco escritores do selo.** A obra inseriu **46 linhas** antes deles: três na decisão e 43 no novo método. O hunk da tabela atualizou apenas **29**, deixando todas as entradas 17 linhas atrás. O selo compara tuplas que incluem a linha; não surgiu escritor novo de estado nesses hunks. Ajustar somente essas cinco posições em memória fez o censo deixar de apontá-las. `app/chamados/models.py:686`; `app/chamados/models.py:699`; `app/chamados/models.py:742`; `app/chamados/tests/e4b_c12_ast.py:23`; `app/chamados/tests/e4b_c12_ast.py:222`.

**O diagrama exige os dois artefatos.** O registro ganhou duas perguntas, chegando a **23** na família chamado, mas os documentos ficaram em **22**. O MAPA também conserva a formulação anterior sobre conversa e omite a pergunta de respostas sem veredito. `app/core/juizes.py:294`; `app/core/juizes.py:295`; `app/docs/ARQUITETURA.mmd:340`; `app/docs/MAPA.md:88`; `app/docs/MAPA.md:180`.

Portanto, não basta regerar somente `ARQUITETURA.mmd`: o gerador compara e escreve **`ARQUITETURA.mmd` e `MAPA.md`**. A segunda falha resulta de restaurar um original já divergente, não demonstra defeito na restauração. `app/core/management/commands/gerar_diagrama.py:923`; `app/core/management/commands/gerar_diagrama.py:932`; `app/core/tests/test_selo_diagrama_do_codigo.py:95`.

## 13/11 09:33 · sessao 96e9d46f0903 · - · - · origem=Ronald (tmux)

Correção: **a falha não implica `materializada_em=None`**. A condição também entra por `proprios` (`app/chamados/services/validacao.py:490`).

O matching usa substring: `marca = 'Pergunta #%s' % perg.pk` e `marca in str(e)` (`app/chamados/services/validacao.py:487`). Assim, PK **9, 99, 999, 9999, 99999 ou 999999** casa com a irmã fictícia `#999999` (`app/chamados/tests/test_validar_diz_o_seu.py:19`), classificando-a como erro próprio mesmo após gravação correta (`app/chamados/services/validacao.py:488`, `app/chamados/services/validacao.py:504`).

**PRE-EXISTENTE**: defeito de matching em código fora dos hunks da obra; o teste apresenta fixture suscetível (`app/chamados/services/validacao.py:487`; `app/chamados/tests/test_validar_diz_o_seu.py:19`). **Colisão efetiva nesta execução: [nao verificado]**, porque o dado não informa o PK da pergunta. **Relógio como causa: [nao verificado]**. A mensagem de competência pode resultar apenas da tradução do erro injetado (`app/chamados/services/acoes_disputa.py:34`; `app/chamados/catalogo/recusa.py:35`).

_4547 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
