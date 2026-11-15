# CODEX-ESTADO

> Ultima resposta processada: **15/11/2026 10:09** · publicado em **15/11/2026 10:10** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 15/11 10:09 · sessao ae03da037804 · HE-REVOGADA-NA-PORTA · verificador · origem=Code

A correção resolve os dois bloqueios identificados: `NAO` sobre recusa revogada deixa de ser `no_op`, e autorização em lote deixa de barrar essa linha. Há, porém, duas ressalvas materiais: os testes novos não comprovam relavratura efetiva, e “DIFF gravado” no serviço não significa dinheiro persistido conferido após o commit. (`app/ponto/portas/he.py:197`, `app/ponto/portas/he.py:587`, `app/ponto/tests/test_he_revogada_na_porta.py:55`, `app/ponto/services/autorizacao_he_periodo.py:94`)

O “antes” abaixo vem do patch fornecido. Execução Django na base e no commit, identidade Git da cópia e impacto quantitativo em produção: **[nao verificado]**. O dado fornecido não traz linhas `colNN` nem valores financeiros de produção: **[nao verificado]**. [nao verificado]

**1. Escritores, leitores e ESMERIL-SEMÂNTICA**

| Campo | Quem escreve em produção | Quem consome diretamente |
|---|---|---|
| `estado` | `_gravar`, criação e atualização. (`app/ponto/portas/he.py:66`, `app/ponto/portas/he.py:75`) | Porta: antes/trilha, idempotência, retorno da ponta pequena, guarda da revogação e guarda do lote; modelo: `__str__`; motor; gestão; montador do retrato. (`app/ponto/portas/he.py:61`, `app/ponto/portas/he.py:197`, `app/ponto/portas/he.py:355`, `app/ponto/portas/he.py:395`, `app/ponto/portas/he.py:408`, `app/ponto/portas/he.py:485`, `app/ponto/portas/he.py:587`, `app/ponto/models.py:1361`, `app/ponto/motor_calculo_v2.py:1417`, `app/ponto/services/gestao_he.py:86`, `app/ponto/services/gestao_he.py:107`, `app/ponto/services/he_pendente_lavrado.py:145`) |
| `origem` | `_gravar`, criação e atualização; recebe `admin` de `decidir_he` e `sistema` da ponta pequena. (`app/ponto/portas/he.py:68`, `app/ponto/portas/he.py:76`, `app/ponto/portas/he.py:208`, `app/ponto/portas/he.py:368`) | Antes/trilhas, proteção contra revogar decisão administrativa e mensagem da guarda do lote. (`app/ponto/portas/he.py:62`, `app/ponto/portas/he.py:396`, `app/ponto/portas/he.py:408`, `app/ponto/portas/he.py:475`, `app/ponto/portas/he.py:591`) |
| `revogada_em` | Nulo na criação; `_gravar` limpa; `_marcar_revogada` preenche e persiste. (`app/ponto/models.py:1335`, `app/ponto/portas/he.py:83`, `app/ponto/portas/he.py:398`, `app/ponto/portas/he.py:400`) | Juiz `vigentes()`, antes da redecisão e trilha da revogação. O helper consulta o juiz. (`app/ponto/models.py:1267`, `app/ponto/portas/he.py:63`, `app/ponto/portas/he.py:409`, `app/ponto/portas/he.py:148`) |
| `revogada_motivo` | Vazio na criação; `_gravar` limpa; `_marcar_revogada` preenche. (`app/ponto/models.py:1340`, `app/ponto/portas/he.py:83`, `app/ponto/portas/he.py:399`) | Não encontrei consumo explícito do atributo armazenado na lógica do produto. A trilha recebe o argumento `motivo`. A ferramenta de sombra inventaria e processa esse campo, com banco explicitamente restrito à sombra. (`app/ponto/portas/he.py:409`, `bin/sombra_regras_pessoais.py:125`, `bin/sombra_mascara_pessoal.py:10`, `bin/sombra_mascara_pessoal.py:44`, `bin/sombra_selo_pessoal.py:74`) |
| `minutos_na_decisao` | `_gravar`, criação e atualização; a revogação preserva a foto. (`app/ponto/portas/he.py:67`, `app/ponto/portas/he.py:75`, `app/ponto/portas/he.py:400`) | Antes/trilhas, retornos idempotentes, retornos da revogação e agregação da gestão. (`app/ponto/portas/he.py:61`, `app/ponto/portas/he.py:203`, `app/ponto/portas/he.py:355`, `app/ponto/portas/he.py:395`, `app/ponto/portas/he.py:408`, `app/ponto/portas/he.py:477`, `app/ponto/portas/he.py:484`, `app/ponto/portas/he.py:490`, `app/ponto/portas/he.py:493`, `app/ponto/portas/he.py:494`, `app/ponto/services/gestao_he.py:108`) |
| `minutos_na_revogacao` | Nulo na criação; `_gravar` limpa; `_marcar_revogada` preenche. (`app/ponto/models.py:1343`, `app/ponto/portas/he.py:83`, `app/ponto/portas/he.py:399`) | Não encontrei consumo explícito do atributo armazenado na lógica do produto; a trilha recebe `int(minutos)`. (`app/ponto/portas/he.py:410`) |

As consultas que hidratam objetos completos também carregam os seis campos, mesmo sem consumir todos: quatro consultas cruas na porta e os mapas dos dois comandos. (`app/ponto/portas/he.py:195`, `app/ponto/portas/he.py:343`, `app/ponto/portas/he.py:470`, `app/ponto/portas/he.py:580`, `app/ponto/management/commands/lavrar_he_pendente.py:48`, `app/ponto/management/commands/recusar_ponta_pequena.py:120`)

Os demais leitores de produção recebem projeções desses valores:

- **Motor:** carrega decisões vigentes por `_DHE`; consulta autorização para dia sem previsto e para o tratamento das pontas de entrada/saída. Não usa a foto dos minutos para pagar. (`app/ponto/motor_calculo_v2.py:1417`, `app/ponto/motor_calculo_v2.py:1456`, `app/ponto/motor_calculo_v2.py:1653`, `app/ponto/motor_calculo_v2.py:1697`, `app/ponto/motor_calculo_v2.py:1739`)
- **Gestão:** `estado_por_dia` e `_decisoes` consultam vigentes; `enriquecer` publica estados, pendências e agregados; calendário, filtros, seleção de lote e contador da Central consomem essas projeções. (`app/ponto/services/gestao_he.py:87`, `app/ponto/services/gestao_he.py:105`, `app/ponto/services/gestao_he.py:141`, `app/ponto/services/gestao_he.py:249`, `app/ponto/services/gestao_he.py:278`, `app/ponto/services/gestao_he.py:281`, `app/ponto/services/gestao_he.py:312`, `app/ponto/services/gestao_he.py:332`, `app/ponto/services/gestao_he.py:405`, `app/ponto/services/gestao_he.py:475`)
- **Template:** espigas, badges, calendário, oferta de autorização e tabela de apoio leem estado/agregados publicados pela gestão. (`app/templates/ponto/gestao_he.html:109`, `app/templates/ponto/gestao_he.html:124`, `app/templates/ponto/gestao_he.html:125`, `app/templates/ponto/gestao_he.html:219`, `app/templates/ponto/gestao_he.html:220`, `app/templates/ponto/gestao_he.html:256`, `app/templates/ponto/gestao_he.html:258`, `app/templates/ponto/gestao_he.html:267`, `app/templates/ponto/gestao_he.html:273`, `app/templates/ponto/gestao_he.html:314`)
- **Folha:** `porta_export.medir` usa `estado_por_dia`; qualquer decisão vigente presente, inclusive `nao`, libera aquele dia da trava específica de HE. Ausência acima do limite entra na trava quando habilitada. (`app/folha/porta_export.py:230`, `app/folha/porta_export.py:232`, `app/folha/porta_export.py:682`, `app/folha/porta_export.py:690`)
- **Lavrador:** fornece mapa vigente ao montador, que pode revogar pela porta antes de ler `estado`. (`app/ponto/management/commands/lavrar_he_pendente.py:48`, `app/ponto/management/commands/lavrar_he_pendente.py:59`, `app/ponto/services/he_pendente_lavrado.py:143`, `app/ponto/services/he_pendente_lavrado.py:145`)
- **Comando da ponta pequena:** consulta existência de vigentes, pula essas chaves e chama a porta para as restantes. (`app/ponto/management/commands/recusar_ponta_pequena.py:120`, `app/ponto/management/commands/recusar_ponta_pequena.py:141`, `app/ponto/management/commands/recusar_ponta_pequena.py:170`)
- **Serviço de período:** delega a escrita ao lote e lê o efeito pela autoridade do motor; não consulta diretamente `DecisaoHE.objects`. (`app/ponto/services/autorizacao_he_periodo.py:94`, `app/ponto/services/autorizacao_he_periodo.py:271`, `app/ponto/services/autorizacao_he_periodo.py:277`, `app/ponto/services/autorizacao_he_periodo.py:280`)

**Semântica:** os campos conservam seus contratos: `estado` é a resposta, vigência é outra dimensão; `origem` identifica quem escreveu o estado atual; os dois campos de minutos são fotos de instantes diferentes. (`app/ponto/models.py:1267`, `app/ponto/models.py:1278`, `app/ponto/models.py:1283`, `app/ponto/models.py:1290`, `app/ponto/models.py:1345`)

Há uma ambiguidade preexistente na apresentação: todo `nao` aparece como “ciência”, inclusive recusa automática, porque a agregação não distingue origem. Além disso, `NAO` humano sobre **recusa vigente do sistema** continua `no_op`, preservando origem e minutos antigos. Portanto “com ciência” não comprova ciência humana. (`app/templates/ponto/gestao_he.html:125`, `app/templates/ponto/gestao_he.html:314`, `app/ponto/services/gestao_he.py:107`, `app/ponto/portas/he.py:197`, `app/ponto/portas/he.py:203`)

Ao reviver, os três campos da revogação são limpos; o histórico depende da trilha anterior, cuja gravação é best-effort. Esse comportamento já está no escritor. (`app/ponto/portas/he.py:83`, `app/ponto/portas/he.py:406`, `app/ponto/portas/he.py:411`)

**2. Censo de consultas fora de `.vigentes()`**

Encontrei quatro consultas executáveis de leitura, todas na porta:

| Sítio | Com `revogada_em` preenchido |
|---|---|
| `decidir_he` | Trava a linha crua; `_vigente` devolve `None`; não aplica idempotência pelo estado antigo; atualiza a mesma linha e limpa a revogação. (`app/ponto/portas/he.py:195`, `app/ponto/portas/he.py:196`, `app/ponto/portas/he.py:206`, `app/ponto/portas/he.py:83`) |
| `recusar_ponta_pequena` | Depois das guardas de minutos/limite, não considera já decidido; dry-run sinaliza recusa; apply revive a linha como recusa do sistema. (`app/ponto/portas/he.py:338`, `app/ponto/portas/he.py:343`, `app/ponto/portas/he.py:344`, `app/ponto/portas/he.py:357`, `app/ponto/portas/he.py:366`) |
| `revogar_recusa_do_sistema` | Origem administrativa retorna “decisão do admin”; origem sistema revogada retorna “já revogada”, sem escrita. (`app/ponto/portas/he.py:470`, `app/ponto/portas/he.py:475`, `app/ponto/portas/he.py:478`, `app/ponto/portas/he.py:483`) |
| `autorizar_em_lote` | Trava a linha crua; revogada não aciona a guarda de estado; segue para `decidir_he`. (`app/ponto/portas/he.py:580`, `app/ponto/portas/he.py:586`, `app/ponto/portas/he.py:587`, `app/ponto/portas/he.py:592`) |

`objects.create` é escrita. O `objects.filter` no texto `como_reverter` é instrução impressa, não consulta executada naquele comando. (`app/ponto/portas/he.py:66`, `app/ponto/management/commands/recusar_ponta_pequena.py:219`)

**3. Matriz antes → depois**

Pressupostos: autorização/permissão e motivo válidos; minutos positivos; autorização em lote acima do limite; ponta pequena dentro do limite; revogação acima do limite. As rejeições dessas entradas permanecem nas mesmas guardas. (`app/ponto/portas/he.py:175`, `app/ponto/portas/he.py:178`, `app/ponto/portas/he.py:184`, `app/ponto/portas/he.py:190`, `app/ponto/portas/he.py:335`, `app/ponto/portas/he.py:338`, `app/ponto/portas/he.py:491`, `app/ponto/portas/he.py:574`)

`=` significa mesmo resultado antes e depois; “troca” atualiza a linha existente. Referências por porta constam na última linha.

| Linha existente | `decidir NAO` | `decidir AUT` | `recusar_em_lote` | `autorizar_em_lote` | `recusar_ponta_pequena` | `revogar_recusa_do_sistema` |
|---|---|---|---|---|---|---|
| Sem linha | cria = | cria = | recusa = | autoriza = | cria sistema = | “sem decisão” = |
| Vigente admin `nao` | `no_op` = | troca = | `no_op` = | erro = | já decidido = | “decisão do admin” = |
| Vigente admin autorizado | troca = | `no_op` = | troca para `nao` = | `no_op` = | já decidido = | “decisão do admin” = |
| Vigente sistema `nao` | `no_op` = | troca = | `no_op` = | erro = | já decidido = | revoga = |
| Revogada sistema `nao` | **`no_op` → revive admin** | troca/revive = | **`no_op` → recusa contabilizada** | **erro → autoriza/revive** | revive sistema = | “já revogada” = |
| Fontes | `app/ponto/portas/he.py:195`, `:197`, `:206` | `app/ponto/portas/he.py:197`, `:206` | `app/ponto/portas/he.py:262`, `:267`, `:270` | `app/ponto/portas/he.py:580`, `:587`, `:592`, `:597` | `app/ponto/portas/he.py:343`, `:355`, `:366` | `app/ponto/portas/he.py:470`, `:475`, `:478`, `:497` |

Para linha **não revogada**, não identifiquei mudança de resultado de negócio: o juiz atual equivale precisamente à nulidade usada anteriormente nas portas do sistema. Há consulta adicional quando existe linha. (`app/ponto/models.py:1267`, `app/ponto/portas/he.py:148`)

A correção de `decidir_he` vale genericamente para qualquer linha revogada, independentemente da origem; o caminho normal de revogação, porém, só admite recusa do sistema. Existência de outras combinações em produção: **[nao verificado]**. (`app/ponto/portas/he.py:196`, `app/ponto/portas/he.py:475`, `app/ponto/portas/he.py:485`)

**4. Chamadores e prévia/confirmar**

- **View de um dia:** `NAO` sobre revogada muda de “já estava assim” para “Trocado”, com chamada ao recalculador. (`app/ponto/views.py:532`, `app/ponto/views.py:539`, `app/ponto/views.py:542`, `app/ponto/portas/he.py:227`)
- **View de recusa em lote:** esse dia passa de `no_op` para `recusados`. (`app/ponto/views.py:617`, `app/ponto/portas/he.py:262`, `app/ponto/portas/he.py:267`, `app/ponto/portas/he.py:270`)
- **Ato de período:** recusa revogada acima do limite passa de erro para autorização, tanto na prévia quanto no confirmar. (`app/ponto/services/autorizacao_he_periodo.py:277`, `app/ponto/portas/he.py:586`, `app/ponto/portas/he.py:592`)
- **Comandos:** para o mesmo estado inicial, mantêm o resultado; seus mapas já usam vigentes. A substituição por `_vigente` acrescenta leitura, preservando o predicado anterior. (`app/ponto/management/commands/lavrar_he_pendente.py:48`, `app/ponto/management/commands/lavrar_he_pendente.py:59`, `app/ponto/management/commands/recusar_ponta_pequena.py:120`, `app/ponto/management/commands/recusar_ponta_pequena.py:170`, `app/ponto/models.py:1267`)
- **Gestão:** já exibia revogada como `sem_decisao`; agora os gestos conseguem concluí-la. Após a escrita, estado e agregados vêm das decisões vigentes. A trava específica da folha também passa a reconhecer a decisão recém-gravada. (`app/ponto/services/gestao_he.py:278`, `app/ponto/services/gestao_he.py:311`, `app/folha/porta_export.py:232`, `app/folha/porta_export.py:686`)

**A igualdade entre os dois DIFFs continua protegida pelo código:** ambos executam `_ato`, medindo antes, autorizando e medindo depois; a prévia desfaz a transação; confirmar compara o dicionário exato e desfaz tudo se divergir. A revogada entra nesse mesmo caminho. (`app/ponto/services/autorizacao_he_periodo.py:271`, `app/ponto/services/autorizacao_he_periodo.py:277`, `app/ponto/services/autorizacao_he_periodo.py:280`, `app/ponto/services/autorizacao_he_periodo.py:287`, `app/ponto/services/autorizacao_he_periodo.py:321`, `app/ponto/services/autorizacao_he_periodo.py:343`, `app/ponto/services/autorizacao_he_periodo.py:346`)

A tela transporta exatamente `r['diff']`; cada medição cria autoridade/motor que consulta as decisões vigentes, sem reutilizar o mapa anterior à escrita. (`app/ponto/views.py:720`, `app/ponto/views.py:723`, `app/ponto/services/autorizacao_he_periodo.py:96`, `app/ponto/services/espelho.py:1111`, `app/ponto/motor_calculo_v2.py:1400`)

**Limite importante:** essa guarda compara o efeito no motor após gravar `DecisaoHE`. Não lê `DiaPago`/`FechamentoMensal` depois da relavratura. Esta roda por callback de commit e pode falhar sem desfazer a decisão; também pode ser barrada pela guarda de competência. Portanto, “DIFF mostrado == dinheiro efetivamente persistido” não está demonstrado por essa comparação. (`app/ponto/services/autorizacao_he_periodo.py:94`, `app/ponto/services/fechamento.py:561`, `app/ponto/services/fechamento.py:1818`, `app/ponto/services/fechamento.py:1824`)

**5. Checklist e selos**

- **Segundo juiz:** não nasce outro predicado; `_vigente` delega ao modelo. Não encontrei registro específico de HE em `core/juizes.py`; chamar esse juiz de “registrado” permanece **[nao verificado]**. (`app/ponto/portas/he.py:148`, `app/ponto/models.py:1267`, `app/core/juizes.py:7`)
- **Segundo escritor fora da porta:** não aparece no diff. Estado continua em `_gravar`; revogação em `_marcar_revogada`. (`app/ponto/portas/he.py:66`, `app/ponto/portas/he.py:84`, `app/ponto/portas/he.py:400`)
- **Relógio fora da borda:** nenhuma leitura nova. `vigentes()` testa nulidade; o relógio existente continua no escritor da revogação. (`app/ponto/models.py:1267`, `app/ponto/portas/he.py:398`)
- **Derivação paralela:** o helper não calcula minutos nem reproduz o predicado de vigência. (`app/ponto/portas/he.py:146`, `app/ponto/portas/he.py:148`)
- **Ordem de trava:** os quatro usos vêm depois de `select_for_update`; as três portas individuais têm `atomic`, e o lote tem bloco por dia. O helper acrescenta `exists()` sobre o mesmo PK, sem travar outra entidade. Quando o lote chega a `decidir_he` com linha existente, há duas consultas adicionais de vigência. Não identifiquei inversão nova de ordem. (`app/ponto/portas/he.py:148`, `app/ponto/portas/he.py:158`, `app/ponto/portas/he.py:195`, `app/ponto/portas/he.py:293`, `app/ponto/portas/he.py:343`, `app/ponto/portas/he.py:417`, `app/ponto/portas/he.py:470`, `app/ponto/portas/he.py:573`, `app/ponto/portas/he.py:580`, `app/ponto/portas/he.py:592`)
- **Campo com dois significados:** não identifiquei novo significado nos seis campos; permanecem as ressalvas de apresentação de “ciência” e de “gravado” descritas acima. (`app/ponto/models.py:1306`, `app/ponto/models.py:1314`, `app/ponto/models.py:1337`, `app/ponto/models.py:1345`, `app/templates/ponto/gestao_he.html:314`, `app/ponto/services/autorizacao_he_periodo.py:94`)

Resultado **esperado por análise estática**, sem certificação de execução:

| Teste novo | Base do patch | Cópia atual | Fonte | [nao verificado]
|---|---|---|---|
| RED_1 | Falha: `no_op=True` | Passa: revive | `app/ponto/tests/test_he_revogada_na_porta.py:57`, `app/ponto/portas/he.py:197` |
| RED_1b | Falha: `(True, True)` | Passa: `(False, True)` | `app/ponto/tests/test_he_revogada_na_porta.py:78` |
| RED_1c | Falha: contabiliza `no_op` | Passa: contabiliza recusa | `app/ponto/tests/test_he_revogada_na_porta.py:89`, `app/ponto/portas/he.py:267` |
| RED_2 | Falha: guarda do lote barra | Passa: autoriza | `app/ponto/tests/test_he_revogada_na_porta.py:100`, `app/ponto/portas/he.py:587` |
| MORDE_3 | Já passa | Continua passando | `app/ponto/tests/test_he_revogada_na_porta.py:113` |
| MORDE_4 | Já passa | Continua passando | `app/ponto/tests/test_he_revogada_na_porta.py:125` |

Os dois MORDE preservam comportamento anterior; não precisam ficar vermelhos na base. MORDE_3 exige a mensagem específica da guarda, evitando passar simplesmente por falta de permissão. (`app/ponto/tests/test_he_revogada_na_porta.py:114`)

A alegação “relavrou o dia” excede a mordida dos testes novos: o recalculador está mockado; `call_count` comprova chamada. Não há caso novo de prévia/confirmar com revogada nem conferência financeira após commit. (`app/ponto/tests/test_he_revogada_na_porta.py:55`, `app/ponto/tests/test_he_revogada_na_porta.py:66`, `app/ponto/tests/test_he_revogada_na_porta.py:97`)

Quanto aos selos existentes:

- **O273:** o helper respeita o juiz, limpeza e escritores existentes. O censo textual exclui integralmente `he.py`; portanto esse selo, sozinho, não fiscaliza a política interna da porta. (`app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:426`, `app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:445`, `app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:473`)
- **O214:** permanecem os escritores e os três chamadores de `acima_do_limite`; não identifiquei contradição estática introduzida. (`app/ponto/tests/test_o214_ponta_pequena.py:296`, `app/ponto/tests/test_o214_ponta_pequena.py:313`, `app/ponto/tests/test_o214_item4_dia_do_admin.py:530`)
- **`test_tela_gestao_he*`:** não identifiquei expectativa contrariada pelo diff; resultado executado: **[nao verificado]**.
- **Atomicidade:** executei somente `python3 -B bin/atomicidade_da_porta.py app`, que lê AST. Retornou **rc=1, 35 travas, 25 cobertas, 10 violações**, nenhuma em `he.py`. O estágio de produção do shell reprova esse retorno. (`bin/atomicidade_da_porta.py:55`, `bin/atomicidade_da_porta.py:97`, `bin/atomicidade_da_porta.py:103`, `bin/tests/test_atomicidade_da_porta.sh:28`)
- As ocorrências foram em `app/chamados/models.py:2744`, `app/chamados/models.py:2753`, `app/chamados/models.py:2766`, `app/chamados/models.py:2770`, `app/ferias/services.py:485`, `app/ferias/services.py:495`, `app/fe

[truncado em 20000 de 22645 caracteres]

## 15/11 10:05 · sessao 4a7b06c0b5f1 · - · - · origem=Ronald (tmux)

Auditoria estática dos selos e checklist:

- **Juiz:** `_vigente` chama exclusivamente `DecisaoHE.objects.vigentes().filter(pk=_ja.pk).exists()`; o único predicado continua `revogada_em__isnull=True`, no modelo. Não nasce segundo predicado nem derivação de minutos. `app/ponto/portas/he.py:130`, `app/ponto/portas/he.py:148`, `app/ponto/models.py:1265`.
- **Registro:** não achei autoridade HE/DecisaoHE em `core/juizes.py`; o registro diz que suas autoridades ficam em `JUIZES[familia][pergunta]`. Distinguir “reusa o juiz já existente no modelo” de “juiz HE registrado”: a segunda afirmação é **[não verificado]**. `app/core/juizes.py:7`, `app/ponto/models.py:1265`.
- **Escritor:** o diff não acrescenta escrita; `_gravar` cria/atualiza e limpa os três campos de revogação, `_marcar_revogada` marca e deixa trilha. Os atos continuam chamando esses escritores. `app/ponto/portas/he.py:66`, `app/ponto/portas/he.py:75`, `app/ponto/portas/he.py:83`, `app/ponto/portas/he.py:206`, `app/ponto/portas/he.py:366`, `app/ponto/portas/he.py:398`, `app/ponto/portas/he.py:497`.
- **Relógio:** o helper novo não lê relógio; `vigentes()` verifica nulidade e não datas. O relógio explícito continua no escritor já existente `_marcar_revogada`, `timezone.now()`. `app/ponto/portas/he.py:146`, `app/ponto/models.py:1267`, `app/ponto/portas/he.py:398`.
- **Trava:** todos os usos novos de `_vigente` vêm depois de travar a linha. `decidir_he`, `recusar_ponta_pequena` e `revogar_recusa_do_sistema` têm `@transaction.atomic`; `autorizar_em_lote` tem bloco por item e chama novamente `decidir_he` para travar a mesma linha. A mudança adiciona uma consulta `exists()` por helper com linha existente; no lote que chega a `decidir_he`, são duas consultas adicionais sobre a mesma linha. Não aparece outra entidade travada pelo helper. `app/ponto/portas/he.py:148`, `app/ponto/portas/he.py:158`, `app/ponto/portas/he.py:195`, `app/ponto/portas/he.py:293`, `app/ponto/portas/he.py:343`, `app/ponto/portas/he.py:417`, `app/ponto/portas/he.py:470`, `app/ponto/portas/he.py:573`, `app/ponto/portas/he.py:580`, `app/ponto/portas/he.py:592`.

RED novos, **inferência estática contra a base descrita no patch; execução Django [não verificado]**:

| Teste | Base descrita | Cópia atual esperada | Evidência | [nao verificado]
|---|---|---|---|
| RED_1 | Falha: devolve `no_op=True` para `NAO` revogado | Passa: atualiza e limpa marca | `app/ponto/tests/test_he_revogada_na_porta.py:50`; `app/ponto/portas/he.py:197`; `app/ponto/portas/he.py:206` |
| RED_1b | Falha: `(True,True)` em vez de `(False,True)` | Passa: uma atualização e segundo clique idempotente | `app/ponto/tests/test_he_revogada_na_porta.py:68`; `app/ponto/portas/he.py:197` |
| RED_1c | Falha: `(recusados=0,no_op=1)` | Passa: `(1,0)` | `app/ponto/tests/test_he_revogada_na_porta.py:83`; `app/ponto/portas/he.py:262`; `app/ponto/portas/he.py:267` |
| RED_2 | Falha: guarda crua do lote devolve erro | Passa: linha revogada não barra autorização | `app/ponto/tests/test_he_revogada_na_porta.py:92`; `app/ponto/portas/he.py:586` |
| MORDE_3 | Já passa na base: recusa admin vigente barra lote | Continua passando | `app/ponto/tests/test_he_revogada_na_porta.py:106`; `app/ponto/portas/he.py:587` |
| MORDE_4 | Já passa na base: repetição vigente é `no_op` | Continua passando | `app/ponto/tests/test_he_revogada_na_porta.py:118`; `app/ponto/portas/he.py:197` |

- Os dois MORDE não serem vermelhos na base é esperado: preservam comportamento anterior. MORDE_3 confere a mensagem específica da guarda, evitando verde por falta de permissão. O helper `_admin` configura a permissão RBAC correta. `app/ponto/tests/test_he_revogada_na_porta.py:114`, `app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:66`.
- **Limitação material:** `assert _rec.call_count == 1` comprova que houve chamada, não que `DiaPago`/fechamento foi relavrado, nem qual competência mudou: o recalculador está mockado. Os testes novos tampouco exercitam serviço prévia/confirmar ou competência exportada. `app/ponto/tests/test_he_revogada_na_porta.py:55`, `app/ponto/tests/test_he_revogada_na_porta.py:66`, `app/ponto/tests/test_he_revogada_na_porta.py:97`.
- Os testes novos herdam `TestCase`; isoladamente não provam execução da porta fora de transação. Essa lacuna é coberta pelo selo existente em `TransactionTestCase`, que chama `decidir_he` e `recusar_em_lote` diretamente. `app/ponto/tests/test_he_revogada_na_porta.py:32`, `app/ponto/tests/test_atomicidade_porta_he.py:31`, `app/ponto/tests/test_atomicidade_porta_he.py:44`, `app/ponto/tests/test_atomicidade_porta_he.py:66`.

Selos existentes:

- Não encontrei expectativa nesses arquivos que o diff, por si, contrarie. O273 exige escritores únicos, limpeza de marca, ausência de estados novos e leitores por `vigentes()`; o helper satisfaz essas condições. Reproduzi somente a varredura textual de leitores de O273, sem Django: lista vazia. `app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:395`, `app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:426`, `app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:445`, `app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:460`, `app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:534`.
- O214 continua encontrando `_gravar` nos dois chamadores e continua encontrando exatamente os mesmos três chamadores de `acima_do_limite`; o diff não adiciona limites literais executáveis nem altera templates. `app/ponto/tests/test_o214_ponta_pequena.py:296`, `app/ponto/tests/test_o214_ponta_pequena.py:313`, `app/ponto/tests/test_o214_ponta_pequena.py:325`, `app/ponto/tests/test_o214_item4_dia_do_admin.py:530`.
- **O shell de atomicidade já ficaria vermelho na cópia atual por ocorrências fora do diff.** Executei apenas `python3 -B bin/atomicidade_da_porta.py app`, ferramenta que lê/analisa AST; resultado: `total=35 sob_atomic=25 arquivos=17 VIOLACAO=10`. O shell marca vermelho quando esse varredor retorna erro. `bin/atomicidade_da_porta.py:47`, `bin/atomicidade_da_porta.py:55`, `bin/atomicidade_da_porta.py:97`, `bin/tests/test_atomicidade_da_porta.sh:28`.
- As dez ocorrências reportadas foram `app/chamados/models.py:2744`, `app/chamados/models.py:2753`, `app/chamados/models.py:2766`, `app/chamados/models.py:2770`, `app/ferias/services.py:485`, `app/ferias/services.py:495`, `app/ferias/services.py:636`, `app/ferias/services.py:639`, `app/pautas/services.py:351`, `app/ponto/services/ausencia.py:1517`; nenhuma em `he.py`. São resultados do critério sintático do selo, não demonstração de falta efetiva de transação herdada de chamador. `bin/atomicidade_da_porta.py:70`, `bin/atomicidade_da_porta.py:76`.
- Não execute o próprio shell sob esta autorização: ele cria diretório/arquivos temporários e os apaga. `bin/tests/test_atomicidade_da_porta.sh:41`, `bin/tests/test_atomicidade_da_porta.sh:42`, `bin/tests/test_atomicidade_da_porta.sh:43`, `bin/tests/test_atomicidade_da_porta.sh:64`.

## 15/11 10:05 · sessao cde97262f688 · - · - · origem=Ronald (tmux)

Chamadores e efeitos, por leitura estática:

- `views.decidir_he` chama a porta por alias `_porta`; sobre recusa revogada + pedido `NAO`, passa de mensagem “já estava assim” a “Trocado”, com minutos do clique e recalcular: `app/ponto/views.py:520`, `app/ponto/views.py:532`, `app/ponto/views.py:539`, `app/ponto/views.py:542`; a nova decisão revive a linha com origem admin: `app/ponto/portas/he.py:196`, `app/ponto/portas/he.py:206`, `app/ponto/portas/he.py:75`, `app/ponto/portas/he.py:83`.
- `views.recusar_he_em_lote` passa de contar `no_op` a contar `recusados` para esse mesmo caso: `app/ponto/views.py:617`, `app/ponto/views.py:618`, `app/ponto/views.py:619`; contador da porta em `app/ponto/portas/he.py:262`, `app/ponto/portas/he.py:267`, `app/ponto/portas/he.py:270`.
- `autorizacao_he_periodo._ato` passa de erro nomeado a autorização efetiva sobre recusa revogada acima do limite: `app/ponto/services/autorizacao_he_periodo.py:277`, `app/ponto/portas/he.py:574`, `app/ponto/portas/he.py:586`, `app/ponto/portas/he.py:592`.
- `lavrar_he_pendente` e comando `recusar_ponta_pequena` mantêm resultados para as mesmas linhas; apenas sofrem consulta extra de vigência quando a porta chega a ela. Seus mapas já usam `.vigentes()`: `app/ponto/management/commands/lavrar_he_pendente.py:48`, `app/ponto/management/commands/lavrar_he_pendente.py:59`, `app/ponto/management/commands/recusar_ponta_pequena.py:120`, `app/ponto/management/commands/recusar_ponta_pequena.py:170`; predicado antigo de nulidade equivale ao juiz atual `app/ponto/models.py:1265`.
- A gestão já mostrava recusa revogada como `sem_decisao`, por consultar somente vigentes e aplicar esse default: `app/ponto/services/gestao_he.py:86`, `app/ponto/services/gestao_he.py:278`. O diff permite concluir os gestos antes bloqueados; após conclusão, estado e colunas são consultados vivos: `app/ponto/services/gestao_he.py:249`, `app/ponto/services/gestao_he.py:250`, `app/ponto/services/gestao_he.py:311`.

Prévia versus confirmar:

- Recusa revogada entra no universo do período porque `estado_por_dia` exclui revogadas; `itens_sem_decisao` recolhe os dias `sem_decisao` acima do limite: `app/ponto/services/gestao_he.py:86`, `app/ponto/services/gestao_he.py:401`, `app/ponto/services/gestao_he.py:405`; `_pedido_de_periodo` usa esse universo nos dois passos: `app/ponto/views.py:668`, `app/ponto/views.py:703`, `app/ponto/views.py:741`.
- Ambos executam `_ato`, que mede antes, autoriza pela mesma porta, mede depois e subtrai: `app/ponto/services/autorizacao_he_periodo.py:271`, `app/ponto/services/autorizacao_he_periodo.py:277`, `app/ponto/services/autorizacao_he_periodo.py:280`, `app/ponto/services/autorizacao_he_periodo.py:287`.
- A prévia desfaz tudo via rollback; confirmar compara o dicionário exato e levanta exceção dentro do atomic em divergência: `app/ponto/services/autorizacao_he_periodo.py:321`, `app/ponto/services/autorizacao_he_periodo.py:323`, `app/ponto/services/autorizacao_he_periodo.py:343`, `app/ponto/services/autorizacao_he_periodo.py:346`, `app/ponto/services/autorizacao_he_periodo.py:352`.
- Cada `_medir` pede autoridade nova; ela cria motor e o motor carrega decisões vigentes por instância: `app/ponto/services/autorizacao_he_periodo.py:94`, `app/ponto/services/autorizacao_he_periodo.py:96`, `app/ponto/services/espelho.py:1111`, `app/ponto/motor_calculo_v2.py:1400`, `app/ponto/motor_calculo_v2.py:1416`. Assim não identifiquei cache de decisão anterior que faça prévia ignorar a autorização recém-gravada dentro da transação. Confirmação executada com cenário revogado e valores reais: [não verificado].
- O valor mostrado é o mesmo `r['diff']` serializado e carregado no form: `app/ponto/views.py:720`, `app/ponto/views.py:723`, `app/templates/ponto/gestao_he_periodo_previa.html:50`, `app/templates/ponto/gestao_he_periodo_previa.html:90`.
- Atenção semântica: “gravado” nesta guarda significa resultado do motor após gravar `DecisaoHE`, não comparação com `DiaPago` depois dos callbacks de commit; `_medir` só consulta `autoridade_do_periodo`: `app/ponto/services/autorizacao_he_periodo.py:88`, `app/ponto/services/autorizacao_he_periodo.py:94`. Competência exportada é rotulada e gera pauta quando houve autorização: `app/ponto/services/autorizacao_he_periodo.py:288`, `app/ponto/services/autorizacao_he_periodo.py:299`.

Matriz: assumir autor autorizado, motivo válido, minutos positivos; para lote autorizar minutos acima do limite, para ponta pequena minutos dentro do limite, para revogar minutos acima. Guardas desses domínios em `app/ponto/portas/he.py:175`, `app/ponto/portas/he.py:178`, `app/ponto/portas/he.py:184`, `app/ponto/portas/he.py:190`, `app/ponto/portas/he.py:335`, `app/ponto/portas/he.py:338`, `app/ponto/portas/he.py:491`, `app/ponto/portas/he.py:574`.

| Linha | decidir NAO | decidir AUT | recusar lote | autorizar lote | ponta pequena | revogar |
|---|---|---|---|---|---|---|
| Sem linha | cria→igual | cria→igual | recusa→igual | autoriza→igual | cria sistema→igual | sem decisão→igual |
| Vigente admin NAO | no_op→igual | troca→igual | no_op→igual | erro→igual | já decidido→igual | admin→igual |
| Vigente admin AUT | troca→igual | no_op→igual | recusa→igual | no_op→igual | já decidido→igual | admin→igual |
| Vigente sistema NAO | no_op→igual | troca→igual | no_op→igual | erro→igual | já decidido→igual | revoga→igual |
| Revogada sistema NAO | **no_op→troca/revive admin** | troca→igual | **no_op→recusa** | **erro→troca AUT** | revive sistema→igual | já revogada→igual |

Referências da matriz: `app/ponto/portas/he.py:195`, `app/ponto/portas/he.py:197`, `app/ponto/portas/he.py:206`, `app/ponto/portas/he.py:262`, `app/ponto/portas/he.py:343`, `app/ponto/portas/he.py:355`, `app/ponto/portas/he.py:366`, `app/ponto/portas/he.py:470`, `app/ponto/portas/he.py:475`, `app/ponto/portas/he.py:478`, `app/ponto/portas/he.py:497`, `app/ponto/portas/he.py:587`, `app/ponto/portas/he.py:592`.

Selos inspecionados:

- O214 exige conjunto exato das chamadoras de `acima_do_limite`; helper novo não altera esse conjunto: `app/ponto/tests/test_o214_item4_dia_do_admin.py:530`, `app/ponto/tests/test_o214_item4_dia_do_admin.py:542`, `app/ponto/portas/he.py:148`.
- O273 censo de leitores crus exclui explicitamente `ponto/portas/he.py` e `ponto/models.py`; helper está dentro da exclusão: `app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:473`, `app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:476`.
- O273 marca escritor único também exclui a porta; diff não introduz atribuição fora dela: `app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:438`, `app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:441`.
- Não executei testes nem modifiquei arquivos. Verde/vermelho empírico: [não verificado]. [nao verificado]

## 15/11 10:05 · sessao 2c6c74fa406d · - · - · origem=Ronald (tmux)

Censo estático da cópia, sem executar testes: [nao verificado]

**Escritores de produção**

| Campo | Todos os sítios encontrados |
|---|---|
| `estado` | `_gravar`: criação `app/ponto/portas/he.py:66`, atualização `:75`, persistência `:84`. Recebe admin de `decidir_he` `:206` e sistema de `recusar_ponta_pequena` `:366`. |
| `origem` | `_gravar`: criação `app/ponto/portas/he.py:68`, atualização `:76`, persistência `:84`; origem admin passada em `:208`, sistema em `:368`. |
| `minutos_na_decisao` | `_gravar`: criação `app/ponto/portas/he.py:67`, atualização `:75`, persistência `:84`; nunca atualizado pela revogação `:398`. |
| `revogada_em` | Default nulo do modelo `app/ponto/models.py:1335`; `_gravar` limpa em `app/ponto/portas/he.py:83`; `_marcar_revogada` escreve relógio em `:398`, persiste em `:400`. |
| `revogada_motivo` | Default vazio `app/ponto/models.py:1340`; `_gravar` limpa `app/ponto/portas/he.py:83`; `_marcar_revogada` escreve `:399`, persiste `:400`. |
| `minutos_na_revogacao` | Default nulo `app/ponto/models.py:1343`; `_gravar` limpa `app/ponto/portas/he.py:83`; `_marcar_revogada` escreve `:399`, persiste `:400`. |

Não apareceu escritor de aplicação fora de `app/ponto/portas/he.py`; o suposto admin do Django mencionado em `app/ponto/models.py:1261` não está registrado no arquivo `app/ponto/admin.py:1` a `:48`. A completude contra referências dinâmicas arbitrárias é [não verificado].

**Leitores diretos dos valores**

- `estado`: antes da escrita/trilha `_gravar` `app/ponto/portas/he.py:61`; idempotência `:197`; retorno de ponta pequena `:355`; trilha da revogação `:395`, `:408`; guarda da revogação `:485`; guarda/erro do lote `:587`, `:591`; `__str__` `app/ponto/models.py:1361`; motor `app/ponto/motor_calculo_v2.py:1417`; mapa da gestão `app/ponto/services/gestao_he.py:86`; agregação da gestão `:107`, `:109`; montagem do retrato `app/ponto/services/he_pendente_lavrado.py:145`.
- `origem`: antes/trilha `_gravar` `app/ponto/portas/he.py:62`; antes/trilha revogação `:396`, `:408`; guarda de autoria da revogação `:475`; mensagem de erro do lote `:591`. Nenhum template encontrado lê origem diretamente; a gestão agrupa apenas por estado em `app/ponto/services/gestao_he.py:107`.
- `revogada_em`: antes/trilha `_gravar` `app/ponto/portas/he.py:63`; juiz `app/ponto/models.py:1267`; trilha após revogar `app/ponto/portas/he.py:409`. `_vigente` pergunta ao juiz, sem reler o atributo em Python `:148`.
- `minutos_na_decisao`: antes/trilha `_gravar` `app/ponto/portas/he.py:61`; retorno de idempotência `:203`; retorno de ponta pequena `:355`; antes/trilha revogação `:395`, `:408`; retornos de revogação `:477`, `:484`, `:490`, `:493`, foto `:494`; soma de decisão por estado na gestão `app/ponto/services/gestao_he.py:108`.
- `revogada_motivo` e `minutos_na_revogacao`: não encontrei leitor explícito do valor armazenado no produto; o log usa os argumentos `motivo` e `int(minutos)`, não os atributos recém-gravados, em `app/ponto/portas/he.py:409` e `:410`. A ferramenta de máscara inventaria `revogada_motivo` em `bin/sombra_regras_pessoais.py:125`; seu processamento genérico está em `bin/sombra_mascara_pessoal.py:44` e o selo em `bin/sombra_selo_pessoal.py:74`.

**Leitores indiretos e telas**

- Motor carrega somente vigentes por alias `_DHE`, `app/ponto/motor_calculo_v2.py:1417`; lê autorização em `:1456`, `:1653`, com efeito nos clipadores `:1697`, `:1739`. Não usa os snapshots para pagar.
- Gestão `estado_por_dia` consulta vigentes em `app/ponto/services/gestao_he.py:87`; `_decisoes` soma snapshots das vigentes em `:105`. `enriquecer` lê ambos `:249`, `:250`, publica estado por dia `:278`, contagens pendentes `:281`, `:285`, e somas/contagens de decisões `:312`.
- Gestão propaga estado ao calendário `app/ponto/services/gestao_he.py:141`; filtra estados `:332`; prepara seleção de lote somente sem decisão `:405`; Central considera ausente no mapa vigente como pendente `:475`, `:477`.
- Template `app/templates/ponto/gestao_he.html`: espiga por estado `:109`; badges `:124`, `:125`; snapshots agregados `:219`, `:220`; oferta de autorização individual `:256`; tooltip/cores/LED/risco `:258`, `:259`, `:267`, `:273`; tabela de pontas `:314`.
- Folha usa o mesmo mapa vigente em `app/folha/porta_export.py:682` a `:686`. Qualquer estado presente libera aquele dia no filtro, inclusive `nao`, em `:232`; ausência de decisão acima do limite entra na trava `:230` a `:234`.
- Comando lavrador fornece mapa vigente de objetos `app/ponto/management/commands/lavrar_he_pendente.py:48`; `apurar` consome estado `app/ponto/services/he_pendente_lavrado.py:145`, podendo antes revogar pela porta `:143`.
- Comando recusa pequena carrega existência de vigentes `app/ponto/management/commands/recusar_ponta_pequena.py:120`, pula essas chaves `:141`, chama porta nas restantes `:170`.
- Serviço de período não lê campos/ORM de `DecisaoHE`; delega lote `app/ponto/services/autorizacao_he_periodo.py:277` e mede antes/depois via autoridade do motor `:271`, `:280`, `:94`.

**Censo ORM fora de `.vigentes()`**

Só quatro consultas executáveis encontradas, todas na porta e todas travando a linha crua:

| Consulta | Com linha revogada |
|---|---|
| `app/ponto/portas/he.py:195` | Obtém/trava a linha; `_vigente` devolve `None` `:196`; não faz idempotência por estado; `_gravar(_ja,...)` revive a mesma linha e limpa revogação `:206`, `:83`. |
| `app/ponto/portas/he.py:343` | Obtém/trava; não considera já decidido `:344`; dry-run retorna sucesso sem escrita `:357`; apply revive pelo `_gravar` `:366`. |
| `app/ponto/portas/he.py:470` | Obtém/trava; para sistema revogada retorna `ja revogada` `:478`, `:483`; se origem admin retorna antes `decisao do admin` `:475`. |
| `app/ponto/portas/he.py:580` | Obtém/trava; `_vigente` devolve `None` `:586`; não barra lote por estado; segue para `decidir_he` `:592`. |

`DecisaoHE.objects.create` `app/ponto/portas/he.py:66` é escrita. `DecisaoHE.objects.filter(...)` em `app/ponto/management/commands/recusar_ponta_pequena.py:219` é texto de instrução de reversão, não consulta executada ali.

**ESMERIL-SEMÂNTICA**

- O contrato distingue estado da decisão e vigência: `NAO` é decisão `app/ponto/models.py:1283`, mas linha revogada não vale segundo `:1267`. A porta passa a respeitar isso em suas quatro políticas `app/ponto/portas/he.py:196`, `:344`, `:478`, `:586`.
- `origem` é autoria do estado atual, não último clique humano `app/ponto/models.py:1290` e `:1294`. Uma recusa **vigente do sistema**, seguida de `NAO` humano, continua `no_op`, conserva origem sistema e snapshot antigo `app/ponto/portas/he.py:197` a `:204`. Isso é preexistente, não mudança do diff.
- Consequentemente, “com ciência” na tela não prova autoria humana: template rotula todo estado `nao` assim em `app/templates/ponto/gestao_he.html:125`, `:220`, `:314`, e a agregação não separa origem em `app/ponto/services/gestao_he.py:107`. A descrição “o admin autorizou N minutos” `:96` não deve ser usada para inferir autoria dos `nao`.
- `minutos_na_decisao` continua sempre foto, nunca pendência atual `app/ponto/models.py:1278`; a gestão soma essa foto como decisão vigente `app/ponto/services/gestao_he.py:108`, enquanto os minutos vivos dos atos vêm do retrato `:388`. `minutos_na_revogacao` é a outra foto, na revogação `app/ponto/models.py:1343`.
- Na revivificação, o motivo/minutos da revogação deixam a linha atual `_gravar` `app/ponto/portas/he.py:83`; sua história está no log anterior de revogação `:406`, sujeito ao best-effort com exceção logada `:411`. O diff não cria esse comportamento.

## 15/11 09:59 · sessao 656d7497dde9 · - · - · origem=Ronald (tmux)

Há duas lacunas semânticas concretas:

1. **Escolha:** VALIDACAO não testemunha materialização. T distingue `validada_sem_efeito` (`chamados/services/tarefa_do_dia.py:169`). O materializador da escolha devolve sucesso sem Batida (`chamados/services/materializacao.py:1069`), depois carimba e lavra (`:1283`), sem registrar DP de materialização. SILENCIO testemunha fechamento humano (`chamados/models.py:1988`); ABSORCAO testemunha ausência vinculada (`chamados/services/materializacao.py:569`). Nenhum deles cobre toda escolha.

   Pergunta: **qual testemunha factual substitui C na materialização de escolha, preservando a distinção entre validação e efeito?** Pausar esse ramo da unidade E.

2. **Retratação auditada antiga:** `_retratada_com_trilha` exige C mais auditoria `retratar_pergunta_obsoleta` (`chamados/juizes.py:291`). RETRATACAO atual significa remoção do motivo, seguida de identidade (`chamados/models.py:2110`, `:2120`, `:2125`). Nenhum escritor produtivo atual desse nome de auditoria apareceu; acervo real [nao verificado].

   Pergunta: **qual fato conserva o fechamento obsoleto auditado e o distingue da remoção do motivo?** A06 pode conservar identidade aplicável; substituir universalmente por RETRATACAO + log não tem equivalência demonstrada. Pausar essa unidade fora de A06.

## 15/11 09:59 · sessao 656d7497dde9 · - · - · origem=Ronald (tmux)

Censo somente leitura; nenhum arquivo editado.

As 16 unidades no HEAD informado:

| Unidade | Linha antiga preparo → atual | Leitura atual relevante |
|---|---:|---|
| `_retratada_com_trilha` | 285? → 285 | C:291; auditoria:294-296 |
| `_slot_esperado` | 341? → 344 | R:351,363; delega ao plantio:374-376 |
| `_pergunta_materializada_de_fato` | 662 → 665 | C/VIA:682,694,710 |
| `_chave_memo_pv` | 821 → 824 | R/V/C/VIA:855-857 |
| `_resposta_concorda_com_a_luz` | 1330? → 1333 | R:1359 |
| `projecao_temporal_da_pergunta` | 1378 → 1381 | R/V/C/VIA:1388-1398 |
| `pergunta_sem_lastro_ou_viva` | 1492? → 1495 | C:1499 |
| `respondida_sem_texto` | 1525? → 1528 | R:1531 |
| `q_respondida_sem_texto` | 1548 → 1544 | R SQL:1551 |
| `q_respondida_com_texto` | 1556 → 1554 | R SQL:1559 |
| `perguntas_corrigiveis` | 1712? → 1716 | R SQL:1718 |
| `perguntas_respondidas_a_validar` | 1721? → 1723 | R ordenação:1727 |
| `declarou_hora` | 1747? → 1738 | R:1753 |
| `delta_da_resposta` | 1894? → 1890 | R:1900 |
| `classificar_resposta_validacao` | 1998? → 1996 | R:2004,2009 |
| `motivo_implausivel_validacao` | 2020? → 2017 | R:2026 |

As interrogações indicam que o preparo forneceu linha do acesso, sem linha de declaração; não apresentar essas antigas como medidas.

**Escritores/fatos, compartilhados pelas unidades D01/D03/D06:**

- R humano: `chamados/services/disputa_emissao.py:1481`, `:1482`; mesma testemunha `_agora` em DP RESPOSTA `:1525`; nova resposta limpa V `:1494`.
- R automático: `chamados/services/disputa_emissao.py:1673`, `:1674`; DP mesmo instante `:1677`.
- R reparado por trilha: `chamados/services/disputa_emissao.py:1695`; DP instante da testemunha `:1698`.
- Nascimento com R/V recebido: `chamados/models.py:3102` chama conservação e `registrar`.
- V: `chamados/services/validacao.py:159`, `:160`; DP mesmo instante e autor `:164`.
- DEVOLUÇÃO: limpa V `chamados/services/validacao.py:224`, `:225`; DP `:230`.
- REABERTURA/RECUSA: `chamados/services/disputa_emissao.py:1631` registra foto e instante `:1632-1637`; porta do lote limpa projeção `:1648`. RECUSA humana entra nessa porta pelo fluxo `chamados/services/validacao.py:626`.
- C e VIA têm atribuição única em memória: `chamados/models.py:3180-3184`; identidade usa a porta `:3265-3268`.
- SILÊNCIO: DP com instante do C em `chamados/models.py:1988-1990`.
- RETRATAÇÃO: DP em `chamados/models.py:2120-2124`, identidade em `:2125`.
- ABSORÇÃO: DP com mesmo `agora` do carimbo `chamados/services/materializacao.py:567-572`.
- Escrita física DP: `chamados/models.py:4086`, `:4096`; ordenação canônica `:4032`; conservação `:4057-4083`.
- Efeito Batida: registro `chamados/services/materializacao.py:933-939`; vínculo, inclusive reuso, `:952-956`; `timestamp`, `retratada_em`, `criado_em` são campos distintos em `ponto/models.py:23`, `:67`, `:79`.

**Quem lê os valores migrados, por unidade:**

- `_retratada_com_trilha`: E em `chamados/juizes.py:800`, `:818`, `:821`; lote em `chamados/services/carga_pergunta_viva.py:194`.
- `_slot_esperado`: E `chamados/juizes.py:755`; lote `chamados/services/carga_pergunta_viva.py:119`; explicador `chamados/services/explicador.py:29`; vigia `chamados/services/vigia_carimbo.py:110`; classe do lote `chamados/services/validacao.py:907`.
- E: P `chamados/juizes.py:1486`; lampada/lavra `chamados/veredito_lavrado.py:263`; materialização `chamados/services/materializacao.py:1239`, `:1433`; raiox `chamados/services/raiox.py:191`; explicador `chamados/services/explicador.py:93`; backfill `chamados/management/commands/backfill_veredito_lavrado.py:105`.
- Memo: `chamados/juizes.py:893`, `:905`.
- Concordância: P `chamados/juizes.py:1432`.
- Projeção temporal: P `chamados/juizes.py:1403`; slot `:350`.
- Sem lastro: corrigíveis `chamados/juizes.py:1719`; leitor de declaração `chamados/services/declaracao_texto.py:240`.
- Respondida sem texto: porta e vigia `chamados/services/fio.py:187`, `:1602`; `chamados/services/vigia_fio.py:629`; admin `chamados/juizes.py:1706`.
- Face SQL com texto: `chamados/models.py:3453-3454`; fila `chamados/juizes.py:1725`. Outros consumidores diretos da face sem texto fora da declaração: [nao verificado].
- Corrigíveis: `chamados/models.py:2282`; `chamados/services/acoes_disputa.py:96`; `chamados/views.py:1419`; template tag `chamados/templatetags/chamados_tags.py:202`.
- Fila a validar: `chamados/services/validacao.py:956`; `api/views_mensageria.py:540`.
- Declarou hora: `chamados/services/relavra_via.py:97`; `chamados/services/validacao.py:185`; `chamados/services/materializacao.py:555`.
- Delta: classificação `chamados/juizes.py:2010`; lote `chamados/services/validacao.py:899`.
- Classificação: tags `chamados/templatetags/chamados_tags.py:147`, `:174`; mensageria `api/views_mensageria.py:544`; comando `chamados/management/commands/validar_classe_a.py:63`.
- Implausível: validação `chamados/services/validacao.py:429`; tag `chamados/templatetags/chamados_tags.py:22`.
- Reexportação histórica das autoridades: `chamados/models.py:4226`.

**Esmeril semântico:**

- `ato_em` é instante do ato (`chamados/models.py:3984-3992`, `:4016`), ordenado com `pk` (`:4032`). R/V humanos usam o mesmo instante da projeção (`disputa_emissao.py:1474-1482`, `:1525`; `validacao.py:159-166`).
- Ambiente aplica `USE_TZ=True`, fuso `America/Sao_Paulo` (`config/settings/base.py:93`, `:95`). “Todo datetime está materializado em UTC em memória” não é garantido pelo campo; [nao verificado]. Não confundir representação/fuso com equivalência de instante.
- Instante do ato não é hora declarada do trabalho. Texto `data_hora` é convertido em horário local consciente (`chamados/juizes.py:363-369`). Para `hora`, a data sai da cadeia de marcos do turno (`:299-334`) e da função de plantio (`:374-376`).
- `data_evento` é dia do turno; conversa guarda dia civil e é excluída dessa interpretação (`chamados/juizes.py:337-341`). Preservar essa separação.
- C não equivale a efeito: registro, vínculo, timestamp do trabalho e instante de criação da Batida são distintos (`materializacao.py:933-956`; `ponto/models.py:23`, `:40`, `:79`). Marco D06 deve captar efeito/retratação e lavra; só o último timestamp do trabalho não basta.
- T atual está em `chamados/services/tarefa_do_dia.py:140-185`, não nas antigas linhas do preparo. Passivo em `:152-153`; devolução `:163-164`; validação sem efeito `:169-170`.

**Dois riscos concretos para a construção:**

1. `_slot_esperado` delega à F4 ainda leitora de projeção (`chamados/juizes.py:374-376`); precisa fornecer os valores do ato ao delegado sem alterar F4.
2. Guarda de escritor chama `respondida_sem_texto` com `SimpleNamespace` sem relação DP (`chamados/services/fio.py:1598-1605`). Além disso, A05 aplica R/V/C em memória antes de chamar P (`chamados/models.py:3212-3222`). Preservar essas exceções expressamente; uma leitura irrestrita de `p.decisoes` quebraria esses caminhos.

## 15/11 09:58 · sessao 9ff5783795fe · ESPINHA-NA-FRENTE · sonda · origem=Code

**NOVO: 248 dias sem porta; ANTIGO: 7 dias de fio morto.** A saída fornecida mede 248, enquanto o aval anexo menciona 247. Todos os números abaixo vêm exclusivamente dessa saída, sem arquivo numerado para conferência. **[nao verificado]**

O NOVO lista também os dias com porta ou responsável: apenas `sem_porta` incrementa `fio_morto_alcance`. [vigia_fio.py:658](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:658), [vigia_fio.py:714](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:714).

**Nenhum plano apresentado permite atribuir zeragem integral aos recortes atuais.** E4c migra leitores; E4d interrompe a projeção; E4B declara cobertura geral das classes a–e e deixa os itens 18–19 para o fim. O anexo E4c enumera arquivos por F1–F9, sem associar essas causas à zeragem. `e4c_reconfere_v2.md:7–15`; [BACKLOG.md:1968](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/docs/BACKLOG.md:1968), [BACKLOG.md:1977](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/docs/BACKLOG.md:1977).

E3-RESTO-R3 remete ao lote LU; RAIZ trata a reação conjunta de perguntas e disputas. Essas linhas também não identificam quais dias desta saída zerariam. [BACKLOG.md:768](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/docs/BACKLOG.md:768), [BACKLOG.md:1988](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/docs/BACKLOG.md:1988).

Para atribuir a cura, faltam:

- **a:** identificar a ação alcançável que faltou; o mapa registra que o PRONTO não transforma `registrado` em ação. [RELATO.md:8823](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/docs/RELATO.md:8823).
- **c:** ligar a resposta sob terminal ao ato reparador; `reabrir_credor` retorna sem ato para pergunta respondida. [disputa_emissao.py:1842](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/disputa_emissao.py:1842).
- **d:** diagnosticar a retenção e o ato correspondente; a definição da classe exige medida antes de cura. [BACKLOG.md:859](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/docs/BACKLOG.md:859).
- **e/terminal:** conferir a carga do credor e o resultado do reparo E4B; outra pergunta exportada, limbo ou recusa podem impedir a reabertura. [disputa_emissao.py:1855](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/disputa_emissao.py:1855).
- **e/sem viva e ANTIGO:** vincular cada dia ao ato executável e medir o resultado; o backfill distingue `tarefa_nada` e dias que continuam sem porta. [backfill_fio_morto_e4b.py:117](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/management/commands/backfill_fio_morto_e4b.py:117), [backfill_fio_morto_e4b.py:368](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/management/commands/backfill_fio_morto_e4b.py:368).

As contas fecham pelo dado fornecido. **[nao verificado]**

- NOVO sem porta: **212 + 11 + 10 + 7 + 2 + 4 + 2 = 248**.
- NOVO completo: **1.560 + 248 + 104 + 65 + 19 + 5 + 248 = 2.249**.
- ANTIGO morto: **6 + 1 = 7**. Universo completo de lâmpadas: **2.381 + 386 + 2 + 0 + 8 + 7 + 0 + 130 = 2.914**.
- Interseção dos **7 antigos com os 2.249 novos: 6**, todos classificados `porta_fila` no NOVO. Interseção entre os **dois contadores de mortos: 0**.
- **“Sem obra” com zeragem demonstrada: 248 NOVOS + 7 ANTIGOS = 255 dias distintos entre os alarmes.** A sonda não identifica individualmente quais antigos compõem cada causa.

Na tabela, **E/A = `exportada=True` / `exportada=False`**; todas essas contagens são do dado fornecido. **[nao verificado]** “Sem obra” significa sem zeragem atribuível demonstrada pelos planos acima.

| Universo · rota/causa/detalhe | E/A | Código classificador | Obra que zera |
|---|---:|---|---|
| NOVO · `porta_app/-/-` | 0/1.560 | [tarefa:634](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/tarefa_do_dia.py:634) | Fora do contador morto |
| NOVO · `porta_fila/-/-` | 0/248 | [tarefa:632](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/tarefa_do_dia.py:632) | Fora do contador morto |
| NOVO · `porta_pergunta/-/-` | 0/104 | [tarefa:632](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/tarefa_do_dia.py:632) | Fora do contador morto |
| NOVO · `decisao_do_dia_pendente/-/-` | 0/65 | [vigia:306](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:306) | Fora do contador morto |
| NOVO · `fio_do_admin/-/-` | 0/19 | [vigia:313](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:313) | Fora do contador morto |
| NOVO · `aguarda_decisao_de_ausencia/-/-` | 0/5 | [vigia:308](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:308) | Fora do contador morto |
| NOVO · `sem_porta/d/-` | 0/212 | [vigia:696](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:696) | **sem obra** |
| NOVO · `sem_porta/c/-` | 0/11 | [vigia:694](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:694) | **sem obra** |
| NOVO · `sem_porta/a/em_analise` | 0/10 | [vigia:683–689](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:683) | **sem obra** |
| NOVO · `sem_porta/a/aberto` | 0/7 | [vigia:683–689](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:683) | **sem obra** |
| NOVO · `sem_porta/a/registrado` | 0/2 | [vigia:683–689](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:683) | **sem obra** |
| NOVO · `sem_porta/e/pergunta_viva_sob_chamado_terminal` | 0/4 | [vigia:706](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:706) | **sem obra**; candidata E4B |
| NOVO · `sem_porta/e/sem_pergunta_viva` | 0/2 | [vigia:704](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:704) | **sem obra** |
| ANTIGO · `pergunta_morreu` | 0/6 | [vigia:449–450](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:449) | **sem obra** |
| ANTIGO · `sem_pergunta_nenhuma` | 0/1 | [vigia:449–450](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:449) | **sem obra** |

## 15/11 09:57 · sessao ccf03c37b60d · - · - · origem=Ronald (tmux)

Confirmo a leitura conservadora: **não há plano que demonstre zeragem dos recortes atuais por causa**; há obras candidatas e cobertura parcial documentada. `app/docs/RELATO.md:8820`, `app/docs/RELATO.md:8823`, `app/docs/RELATO.md:8826`, `app/docs/RELATO.md:8827`.

- **a/registrado:** o mapa anterior declara “PRONTO nao transforma registrado em acao”. `app/docs/RELATO.md:8823`.
- **c/respondida sob terminal:** o reparo de credor exclui respondidas; o código retorna sem ato quando `respondida_em` existe. `app/docs/RELATO.md:8827`; `app/chamados/services/disputa_emissao.py:1842`.
- **d:** o mapa registra a lacuna “folha retem, tarefa nada”; não demonstra cura pela E3 ou E4B. `app/docs/RELATO.md:8820`; `app/docs/RELATO.md:610`.
- **e/terminal:** E4B é candidata, mas pode recusar por limbo, outra pergunta exportada na carga ou recusa da reabertura; `exportada=False` do dia não basta. `app/chamados/services/disputa_emissao.py:1855`, `app/chamados/services/disputa_emissao.py:1860`, `app/chamados/services/disputa_emissao.py:1869`.
- **e/sem viva e ANTIGO:** falta diagnóstico que vincule cada dia ao ato reparador; o comando determina atos por tarefa e mede depois se o dia permaneceu sem porta. `app/chamados/management/commands/backfill_fio_morto_e4b.py:255`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:368`.

Recomendo escrever **“sem obra com zeragem demonstrada”**, evitando afirmar ausência de obra registrada: E4B cobre genericamente classes a–e, E4c migra leitores e E4d interrompe projeção. `app/docs/BACKLOG.md:1977`; `app/docs/BACKLOG.md:1968`.

Subtotal conservador: **248 NOVO + 7 ANTIGO = 255 incidências sem zeragem atribuível demonstrada** pelo material disponível. Os números atuais vêm exclusivamente do enunciado. `[nao verificado]`

## 15/11 09:56 · sessao 3cff7206f1ea · - · - · origem=Ronald (tmux)

- **E4c/E4d não têm zeragem por causa demonstrada nos anexos.** BACKLOG define E4c como migração dos leitores DIVERGE e selo; E4d como parar a projeção. O inventário F1–F9 do anexo identifica arquivos, não um reparo que zere cada classe. `app/docs/BACKLOG.md:1968`; anexo `e4c_reconfere_v2.md`, itens F1–F9.
- **E4B é a obra candidata ao passivo a–e**, explicitamente; porém a mesma linha informa apenas que itens 18–19 ficam para o fim do programa. Não demonstra que estes 248 dias desaparecerão. `app/docs/BACKLOG.md:1977`.
- **O próprio comando exige classificação adicional:** deriva atos da tarefa; sem ato executável retorna `tarefa_nada`/`nao_sei`; retira essas linhas da execução; depois mede quais continuam sem porta. A saída fornecida não contém esses atos/resultados por dia. `app/chamados/management/commands/backfill_fio_morto_e4b.py:109`; `:117`; `:262`; `:368`.
- **Classe d tem lacuna documentada:** a definição do contador não equivale ao “sair de d” do aval; exige medida antes de cura. RELATO também registra ausência de decisão na E3 para o estado retido cuja tarefa não exige nada. `app/docs/BACKLOG.md:859`; `app/docs/RELATO.md:610`.
- **E3-RESTO-R3 remete ao lote LU**, mas sua linha não relaciona LU-01..07/LU3 às causas a/c/d/e nem promete zerá-las. `app/docs/BACKLOG.md:768`.
- **RAIZ-DA-DISPUTA trata reação conjunta de raiz e filhos**, destino e órfãos, mas não contém associação dos 248/7 dias atuais a uma fatia que os zere. `app/docs/BACKLOG.md:1988`; `app/docs/GRADE-MESTRA.md:108`.

Classificação conservadora: **“sem obra com zeragem comprovada”** para os 248 NOVOS e os 7 ANTIGOS; E4B pode ser indicada como candidata, sem subtrair dias do subtotal “sem obra”. Para atribuição verificável falta plano por ramo/ato e resultado do DRY sobre os dias fornecidos. `app/docs/BACKLOG.md:1977`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:255`; `:368`.

## 15/11 09:56 · sessao 95f73f751961 · - · - · origem=Ronald (tmux)

- **NOVO:** `alcance_universo` reúne dias retidos pela Folha, por colaborador/data, nas competências enumeradas; não é o contador de mortos. `app/chamados/services/vigia_fio.py:584–600`. Apenas `sem_porta` incrementa `fio_morto_alcance`. `app/chamados/services/vigia_fio.py:714–718`.
- **Classificações NOVO:** `a` = presença de chamado vivo do dia ou âncora viva fora da fila, detalhe com status; `c` = pergunta viva respondida com destino terminal; `d` = nenhuma pergunta viva, dia fora da lista e colaborador na lista; `e/sem_pergunta_viva` e `e/pergunta_viva_sob_chamado_terminal` são residuais posteriores. `app/chamados/services/vigia_fio.py:679–707`.
- **Rotas publicadas:** `porta_fila`/`porta_pergunta` vêm da tarefa em `app/chamados/services/tarefa_do_dia.py:628–632`; `porta_app`, em `app/chamados/services/tarefa_do_dia.py:634`; o vigia apenas as lê e conta. `app/chamados/services/vigia_fio.py:658–665`.
- **Baldes com ação nomeada:** decisão pendente, ausência aberta e fio do admin são classificados respectivamente em `app/chamados/services/vigia_fio.py:306–307`, `:308–310`, `:313–314`; entram antes das causas de `sem_porta`. `app/chamados/services/vigia_fio.py:670–678`.
- **ANTIGO:** parte de chamados vivos com célula, restringe o vocabulário e exige lâmpada acesa; primeiro separa fio vivo, hoje/futuro e os baldes com dono. `app/chamados/services/vigia_fio.py:334–379`, `:412–448`. No residual, `ps` não vazio gera `pergunta_morreu`; vazio gera `sem_pergunta_nenhuma`. `app/chamados/services/vigia_fio.py:449–450`.
- A sonda fornecida permite **6 `pergunta_morreu` + 1 `sem_pergunta_nenhuma` = 7**, todos abertos; não imprime qual célula pertence a qual dessas duas causas. Atribuição individual permanece **[nao verificado]**.
- As **6 células antigas presentes no NOVO estão todas em `porta_fila` no dado fornecido**. Logo, a interseção dos 7 antigos com o universo NOVO de 2249 é **6**; com os **248 `sem_porta` é 0**, conforme as linhas fornecidas **[nao verificado]**.
- A partição antiga `com_alvo=0`, `sem_alvo=5`, `sem_pergunta_na_porta=2` também soma 7, mas é classificação distinta e não equivale às causas a–e. `app/chamados/services/vigia_fio.py:467–480`.
- Para eventual conferência do universo completo antigo: **2381 + 386 + 2 + 0 + 8 + 7 + 0 + 130 = 2914**, conforme agregados fornecidos **[nao verificado]**; o código declara a identidade em `app/chamados/services/vigia_fio.py:104–107`.
- Exportação do ANTIGO compõe a janela do dia com `competencia_entregue`; não presume o mês civil. `app/chamados/services/vigia_fio.py:843–865`.

## 15/11 09:53 · sessao 698c79408f3e · ESPINHA-NA-FRENTE · leitura · origem=Code

**“Nada” abaixo significa nenhum pedido adicional demonstrado pelo recorte descrito.** O anexo fornece alvos da E4c; o BACKLOG descreve CONTRACT e fatos da E4d, mas o diff final de cada pouso é **[nao verificado]**. `e4c_reconfere_v2.md:7–15` (anexo); `app/docs/BACKLOG.md:1968–1975`.

Cláusulas usadas: **SMOKE** = BUG 73, duas cascas; PRODUCAO-MAXIMA 5 permite smoke posterior somente quando resta TELA, com reversão pronta. **BATIDA** = kernel espera Ronald. **DINHEIRO** = aberta preaprovada com reversão anterior, hash das exportadas intacto e prova de frota posterior; gravado exportado exige `!` e REGEN-EM-EXPORTADA. **PARADA 2 / §4** = dados ESCALA/VÍNCULO, motor e apagar/voltar arquivo usado em prod exigem `!`. **CADASTRO** = regra variável precisa de cadastro pela UI. `CLAUDE.md:202–203`, `CLAUDE.md:143–144`, `CLAUDE.md:265–268`, `CLAUDE.md:406–417`, `CLAUDE.md:43–44`; `producao_maxima.txt:1` (anexo).

**Dinheiro não pode receber “exportada intacta” por presunção:** validação/materialização alcançam o fechamento da competência do fato; a cópia persiste também outros status e tem guarda específica para 09/2026. Competências e valores efetivamente atingidos por cada futura fatia: **[nao verificado]**. `app/chamados/services/validacao.py:470–472`; `app/ponto/services/fechamento.py:1809–1810`, `app/ponto/services/fechamento.py:1137–1162`, `app/ponto/services/fechamento.py:36–43`.

| fatia | pede | cláusula | arquivo:linha |
|---|---|---|---|
| **E4c F1 — juízes** | **Smoke: nada direto**, sem template/JS/view no alvo. **Batida/dinheiro:** migra predicados e fila de validação; efeito efetivo sobre esses caminhos **[nao verificado]**. **Escala/vínculo, motor, apagar arquivo, cadastro: nada descrito.** | SMOKE; BATIDA/DINHEIRO se alterar o caminho executor; PARADA 2; CADASTRO | `e4c_reconfere_v2.md:7` (anexo); `app/chamados/juizes.py:1528–1559`, `app/chamados/juizes.py:1722–1724` |
| **E4c F2 — modelos** | **Smoke: nada direto. Batida: conferir com Ronald o recorte de `_nascer`**, pertencente ao caminho de emissão alcançado pela batida; estado/seleção/apresentação isolados não demonstram esse efeito. **Dinheiro: [nao verificado]. Escala/vínculo, motor, apagar arquivo, cadastro: nada descrito.** | BATIDA; §4; PARADA 2; CADASTRO | `e4c_reconfere_v2.md:8` (anexo); `app/chamados/models.py:2922`, `app/chamados/models.py:3124–3131`; `app/chamados/services/fio.py:1161–1167` |
| **E4c F3 — validação/verbos/painel/ações** | **Smoke: nada direto. Batida: Ronald se mudar validação → materialização. Dinheiro:** validação agenda recálculo da competência **do fato**, aberta ou exportada conforme o caso; exportada gravada pede `!`. **Escala: `!` se atingir aceite de folga**, que chama `declarar_dia` → `lancar_folga`. **Motor, apagar arquivo, cadastro: nada descrito.** | BATIDA; DINHEIRO 1–2; PARADA 2 / §4; CADASTRO | `app/chamados/services/validacao.py:346–347`, `app/chamados/services/validacao.py:460–487`; `app/ponto/services/declarar_dia.py:179–193`; `app/chamados/services/materializacao.py:932–940` |
| **E4c F4 — emissão/materialização/passivo** | **Smoke: nada direto. Batida: Ronald no recorte que alterar plantio. Dinheiro:** materialização → `registrar_batida` → fechamento por evento; passivo exportado tem caminho explícito **plantio → REGEN → Pauta DP**. **`!` para executar retificação exportada e regeneração de DNA/células.** Competências atingidas **[nao verificado]**. **Motor, apagar arquivo, cadastro: nada descrito.** | BATIDA; DINHEIRO 1–2; PARADA 2 / §4; CADASTRO | `app/chamados/services/materializacao.py:932–940`, `app/chamados/services/materializacao.py:1012–1024`; `app/chamados/services/passivo_validado.py:433–449`; `app/ponto/portas/regen_exportada.py:87–104` |
| **E4c F5 — fio/reconciliador e leitores** | **Smoke: nada direto. Batida: Ronald para o recorte do reconciliador/contrato no caminho da batida. Dinheiro:** POST do fio pode chamar materialização, alcançando competência do fato; tarefa/vigia/apresentação isolados não demonstram movimento. **Escala/vínculo, motor, apagar arquivo, cadastro: nada descrito; impacto real [nao verificado].** | BATIDA; DINHEIRO 1–2; PARADA 2; CADASTRO | `app/ponto/signals.py:206–215`; `app/chamados/reconciliador.py:1429–1442`; `app/chamados/services/fio.py:698–708`, `app/chamados/services/fio.py:1165` |
| **E4c F6 — views/API/canal/tags** | **Smoke:** Meu Atendimento, detalhe local, modal do fio e prévia/resultado do lote; nas duas cascas onde compartilhados. **API:** preservar fases públicas e adicionar exigibilidade; isso não demonstra alteração da API de bater ponto. **Batida/dinheiro:** seleção do lote alcança validação → materialização/recálculo; Ronald se esse caminho mudar, `!` se mover gravado exportado. **Escala/vínculo, motor, apagar arquivo, cadastro: nada descrito.** | SMOKE; BATIDA; DINHEIRO; PARADA 2; CADASTRO | `app/chamados/views.py:550`, `app/chamados/views.py:632–633`, `app/chamados/views.py:927`; `app/chamados/views_cobrar.py:169–208`, `app/chamados/views_cobrar.py:353–386`; `app/api/urls.py:67–73`; `app/docs/BACKLOG.md:1999` |
| **E4c F7 — quatro partials** | **Smoke de Ronald:** Meu Atendimento/card, detalhe/pergunta, modal do fio e drawer do lote; **duas cascas** conforme o grafo. Pode ficar para depois **se** o diff conservar apenas apresentação/leitura, com reversão pronta. **Kernel direto: nada. Dinheiro:** nenhuma escrita demonstrada pelo recorte de apresentação; os formulários acionam portas existentes. **Escala/vínculo, motor, apagar arquivo, cadastro: nada descrito.** | SMOKE + PRODUCAO-MAXIMA 5; PARADA 2; CADASTRO | `app/templates/chamados/meu_atendimento.html:1`, `app/templates/chamados/meu_atendimento.html:127`; `app/templates/chamados/detalhe_local.html:310`; `app/templates/chamados/partials/modal_fio.html:163–177`; `app/templates/chamados/partials/_lote_preview.html:21–26`; `app/core/context_processors.py:210` |
| **E4c F8 — ponto/calendário/ficha/prévia** | **Smoke:** calendário do perfil, cabeçalho do fio e Prévia da Folha se a mudança alcançar essas saídas; template/JS futuro **[nao verificado]**. **Batida: Ronald no recorte `flip_auto`**: leitura alimenta cron com `--apply` → `flip_tipo` → `registro_batida.anotar`. **Dinheiro:** prévia é leitura; alteração monetária decorrente do flip **[nao verificado]**. **`!` se houver apply que regrave célula/ata; motor, apagar arquivo, cadastro: nada descrito.** | BATIDA; SMOKE se front; DINHEIRO se houver efeito; PARADA 2 / §4; CADASTRO | `app/colaboradores/views.py:1948–1965`; `app/chamados/views.py:918–930`; `app/folha/previa.py:28–31`; `app/ponto/services/flip_auto.py:128–140`; `app/config/crons.py:707`; `app/ponto/services/flip_batida.py:12–39`; `app/ponto/services/cartorio.py:951–956` |
| **E4c F9 — comandos** | **Smoke: nada. Migrar leitores não implica executar apply.** Se o pouso incluir `validar_classe_a --apply`, chama validação real: **Ronald para batida**, DINHEIRO na competência do fato e **`!` se alcançar gravado exportado**. Outros comandos escrevem atos/carimbos; universo/competências dos applies **[nao verificado]**. **Escala/vínculo, motor, apagar arquivo, cadastro: nada descrito.** | BATIDA; DINHEIRO; PARADA 2; CADASTRO | `app/chamados/management/commands/validar_classe_a.py:85–100`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:323–331`; `app/chamados/management/commands/encerrar_perguntas_com_fato.py:53–68`; `app/chamados/management/commands/relavrar_via_i6.py:96–102` |
| **E4d — CONTRACT** | **Arquivos do futuro corte: [nao verificado].** Conferência funcional identificável: fio e atendimento ao parar projeção. **Smoke obrigatório** depende de tocar front. **Kernel/dinheiro/escala/vínculo:** conservação dos efeitos após desligar projeção **[nao verificado]**; não cabe declarar “nada”. **Motor, apagar arquivo, cadastro: nada descrito.** | SMOKE se front; BATIDA/DINHEIRO conforme alcance; PARADA 2; CADASTRO | `app/docs/BACKLOG.md:1968`; `app/chamados/models.py:2525–2527`, `app/chamados/models.py:3982–3992`; `app/templates/chamados/partials/modal_fio.html:217–229`; `app/templates/chamados/partials/_card_atendimento.html:189–192` |
| **E4d — FATO-A03, reabertura** | **Smoke candidato:** fio, “Reabertura administrativa”; obrigatório se editar esse front. Porta atual escreve disputa/chamado/pergunta. **Batida/dinheiro diretos: nada nessa porta; alcance futuro [nao verificado]. Escala/vínculo, motor, apagar arquivo, cadastro: nada descrito.** | SMOKE se front; PARADA 2; CADASTRO | `app/docs/BACKLOG.md:1970`; `app/chamados/services/acoes_disputa.py:235–274`; `app/templates/chamados/partials/modal_fio.html:377–390` |
| **E4d — FATO-A05, assinatura** | **Tela/casca, kernel, dinheiro, competência e arquivos do pouso: [nao verificado].** O registro apenas nomeia “ramo de assinatura”; não sustenta escolher uma assinatura de escala como alvo. **Motor, apagar arquivo e cadastro novo: [nao verificado].** | SMOKE/BATIDA/DINHEIRO/PARADA 2/CADASTRO: enquadramento depende do sítio ainda não identificado | `app/docs/BACKLOG.md:1971`; [nao verificado] |
| **E4d — FATO-A06, identidade/duplicata** | **Smoke candidato:** atendimento mostrando repetida/via; obrigatório se editar front. Porta atual grava identidade/carimbo/motivo. **Batida/dinheiro diretos: nada nessa porta; alcance futuro [nao verificado]. Escala/vínculo, motor, apagar arquivo, cadastro: nada descrito.** | SMOKE se front; PARADA 2; CADASTRO | `app/docs/BACKLOG.md:1972`; `app/chamados/models.py:3251–3273`; `app/chamados/services/fio.py:1384–1401`; `app/templates/chamados/partials/_card_atendimento.html:192` |
| **E4d — FATO-A17, pendência técnica** | **Smoke candidato:** pendência no fio/card. **Batida/dinheiro:** fato está dentro do materializador; se a troca alterar prosseguimento para plantio, espera Ronald e pode alcançar fechamento da competência do fato, inclusive exportada → `!`. Delta real **[nao verificado]**. **Escala/vínculo, motor, apagar arquivo, cadastro: nada descrito.** | SMOKE se front; BATIDA; DINHEIRO; PARADA 2; CADASTRO | `app/docs/BACKLOG.md:1973`; `app/chamados/services/materializacao.py:851–857`, `app/chamados/services/materializacao.py:932–940`; `app/templates/chamados/partials/modal_fio.html:222–226`; `app/templates/chamados/partials/_card_atendimento.html:189` |
| **E4d — FATO-A18, recusa técnica** | **Smoke candidato:** recusa/motivo no fio/card. **Batida/dinheiro:** mesma fronteira materializadora; mudança na recusa pode mudar plantio e recálculo da competência do fato. Ronald para kernel; **`!` se mover gravado exportado**; impacto **[nao verificado]**. **Escala/vínculo, motor, apagar arquivo, cadastro: nada descrito.** | SMOKE se front; BATIDA; DINHEIRO; PARADA 2; CADASTRO | `app/docs/BACKLOG.md:1974`; `app/chamados/services/materializacao.py:1089–1113`, `app/chamados/services/materializacao.py:932–940`; `app/templates/chamados/partials/modal_fio.html:226`; `app/templates/chamados/partials/_card_atendimento.html:192` |
| **E4d — FATO-A20, conferência de data** | **Smoke candidato:** fio, “Datas em disputa / Conferir data”. **Batida/dinheiro:** conferência libera gate materializador → plantio → fechamento da competência do fato; Ronald para kernel, **`!` se alcançar gravado exportado**. A regra “só o par conferido” já está decidida; implementação/impacto **[nao verificado]**. **Escala/vínculo, motor, apagar arquivo, cadastro: nada descrito.** | SMOKE se front; BATIDA; DINHEIRO; PARADA 2; CADASTRO | `app/chamados/services/validacao.py:660–709`; `app/chamados/views.py:1292–1314`; `app/templates/chamados/partials/modal_fio.html:163–177`; `app/chamados/services/materializacao.py:1199–1220`; `app/docs/CORTES.md:440` |
| **Retirada física posterior — campos legados** | Está registrada **depois da série**, separada de parar projeção. **Smoke/kernel/dinheiro e arquivos da migration: [nao verificado].** Remover campo não demonstra apagar arquivo de prod; **se apagar/voltar arquivo usado, `!`**. **Cadastro novo: nada descrito.** | PARADA 2; demais cláusulas dependem do diff | `app/docs/BACKLOG.md:1969`; `CLAUDE.md:265–268` |

## 15/11 09:52 · sessao 59206908181f · - · - · origem=Ronald (tmux)

F6–F9: evidências para a lista única:

| Fatia | Pedido antecipado identificado | Cláusula / prova |
|---|---|---|
| E4c F6 | Smoke de **Meu Atendimento**, **detalhe do chamado**, **modal do fio**, **validar inline/lote** e contrato dos endpoints de chamados/disputa. `views.py:632–633`, `:550`, `:927`; `views_cobrar.py:169`, `:208`, `:353–386`; `api/views.py:1159–1165`, `:2549–2563`, `:2611–2627`. A presença de `api/views.py` **não prova edição da API de batida**: `/ponto/bater/` é outra função/rota (`api/urls.py:67`), distinta de chamados/disputa (`:73`, `:92–93`). Há caminho transitivo **seleção → validar → materializar/recalcular**, portanto dinheiro e batida não podem ser descartados sem o diff: `views_cobrar.py:169–208` → `validacao.py:461–471`, `:487–494`. Competências/delta reais [nao verificado]. Novo cadastro, alteração de escala/vínculo, edição do motor ou exclusão de arquivo não estão demonstrados no recorte [nao verificado]. | BUG 73 `CLAUDE.md:202`; PARADA 2 `:265–268`; DINHEIRO `:406–417`; PROD-MÁXIMA item 5, anexo. |
| E4c F7 | Smoke de **Meu Atendimento nas duas cascas**, **pergunta no detalhe DP**, **modal do fio**, **prévia/resultado de validar lote**: `_card_atendimento.html:106`, `:114`, `:141–154`; `modal_fio.html:185`, `:217–228`; `_pergunta_card.html:20–42`; `_lote_preview.html:21–26`. Casca colaborador: `meu_atendimento.html:1`, `:127`, `:144` + `core/context_processors.py:210`; detalhe DP: `detalhe_local.html:1`, `:310`; modal servido por `chamados/views.py:927`; lote servido por `views_cobrar.py:353–386`. Os quatro alvos são templates; não demonstram edição do kernel, motor, cadastro ou dados de escala/vínculo. O clique de validação já chama a porta que materializa/recalcula (`views_cobrar.py:379–381`; `validacao.py:461–494`), portanto o smoke de escrita não deve ser confundido com simples inspeção da tela (`CLAUDE.md:205`). Delta monetário/competências [nao verificado]. | BUG 73 `CLAUDE.md:202–203`; smoke de POST `:205`; PROD-MÁXIMA item 5 admite pouso com smoke depois **somente quando for só tela**, com reversão pronta, anexo. |
| E4c F8 | Smoke das superfícies **Calendário do perfil**, **procedência no espelho/PDF**, **cabeçalho/ficha do modal do fio**, **Prévia de proposta da folha**: `colaboradores/views.py:1938`, `:1948–1965`; `ponto/services/espelho.py:755`; `relatorios/pdf_espelho.py:950`; `chamados/views.py:918`, `:930`; `modal_fio.html:13`; `folha/views.py:701–723`. **Não é fatia só de apresentação**: `flip_auto.py:128–140` lê projeção para montar fila → `:268` → cron `config/crons.py:707` (`--competencia --apply`) → `flip_automatico.py:25–31`, `:68` → `flip_batida.py:12`, `:30–39` → **`registro_batida.anotar` e recompute**. Este ramo afeta kernel/escrita de Batida e pede Ronald antes pelo item 5. Não edita `registro_batida.py` diretamente no conjunto-alvo do anexo. Janela do cron é a aberta (`flip_automatico.py:25–31`); delta/competências reais [nao verificado]. Prévia em si é leitura (`folha/previa.py:29–31`, `:139–145`). Cartório escreve campos de `CelulaDia`, mas não DNA/vínculo (`cartorio.py:951–956`); se o pouso incluir apply de relavratura desses dados de ESCALA, pede **`!`**. Edição de `motor_calculo_v2.py`, exclusão de arquivo e cadastro novo não estão demonstrados [nao verificado]. | Seção 4 `CLAUDE.md:142–153`; PARADA 2 `:265–268`; DINHEIRO `:406–417`; PROD-MÁXIMA item 5, anexo. |
| E4c F9 | **Tela direta: nada**, pois os alvos do anexo são comandos. Smoke indireto/execução de apply não decorre automaticamente de trocar os leitores; operações efetivamente incluídas no pouso [nao verificado]. **Não declarar “nada de batida/dinheiro”** se executar `validar_classe_a --apply`: `validar_classe_a.py:85–100` chama `validar_pergunta`, que materializa e recalcula (`validacao.py:461–494`). Outros applies escrevem atos/perguntas: `backfill_decisoes_pergunta.py:323–331`; `encerrar_perguntas_com_fato.py:53–68`; `relavrar_via_i6.py:96–102` → `services/relavra_via.py:106–128`; `migrar_data_evento_dia_do_turno.py:393–400`; `reconciliar_fantasmas.py:269–281`; `reconciliar_grade.py:305–316`. Backfill/migração já excluem exportadas (`backfill_decisoes_pergunta.py:302–308`; `migrar_data_evento_dia_do_turno.py:384–398`). Alterar estado de pergunta em exportada não equivale, por si, a mexer no `FechamentoMensal`; apply monetário real/competências [nao verificado]. Novo cadastro, mudança de escala/vínculo, edição do motor e exclusão de arquivo prod [nao verificado]. | DINHEIRO `CLAUDE.md:406–417`; PARADA 2 `:265–268`; kernel pelo caminho de validação e PROD-MÁXIMA item 5, anexo. |

Observações para a consolidação:

- **F8 é o caso que mais facilmente receberia “nada” indevido**: a fila de recusas abastece cron escritor de Batida pela porta `registro_batida.anotar`. `flip_auto.py:128–140`; `config/crons.py:707`; `flip_automatico.py:68`; `flip_batida.py:12`, `:30–39`.
- `cartorio.py:951–956` distingue a relavratura de veredito/ata da regeneração de DNA. O leitor R identificado pela E4c está em `cartorio.py:1637–1641`; esse ramo documenta reativação de perguntas sem tocar FechamentoMensal/batida/TXT (`:1627–1628`), mas não prova ausência de efeito de todo o arquivo ou do diff futuro.
- O cadastro exigido por “TUDO TEM CADASTRO” só nasce se a mudança introduzir regra que varie por cliente/praça/empresa (`CLAUDE.md:43–44`). Não há essa regra nova demonstrada no inventário F6–F9 do anexo; afirmar necessidade de cadastro novo seria [nao verificado].
- Excluir um arquivo só por achá-lo órfão é diferente de trocar leitores: exclusão de arquivo usado por prod pede `!` (`CLAUDE.md:265–267`). O inventário não demonstra exclusões futuras [nao verificado].

## 15/11 09:52 · sessao 479eee87c4e0 · ESPINHA-NA-FRENTE · leitura · origem=Code

**Veredito: depende do alcance efetivo dos itens 18–19. O zero atual não basta para dispensar a autorização.** A regra exige `!` para tocar o GRAVADO de competência exportada; a correspondência dos dois itens com funções e campos concretos permanece **[não verificado]**. ([CLAUDE.md:265](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/CLAUDE.md:265))

**De onde nasce a exigência**

- Regra geral, literal: “**tocar o GRAVADO de competencia EXPORTADA**” está entre os atos que exigem `!`, nunca previamente aprovados. ([CLAUDE.md:265](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/CLAUDE.md:265))
- Regra de dinheiro, literal: “**o GRAVADO (`FechamentoMensal`) nao muda por caminho generico; e `!`**”, com DIFF anterior, reversão e prova posterior. ([CLAUDE.md:411](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/CLAUDE.md:411))
- Na obra, o BACKLOG manda deixar exportadas “em lista para o `!`”; a GRADE registra explicitamente “itens 18–19; autorização das exportadas”. ([BACKLOG.md:1977](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/docs/BACKLOG.md:1977), [GRADE-MESTRA.md:717](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/docs/GRADE-MESTRA.md:717))

**O número sustenta somente a fotografia fornecida**

São **581 dias = 581 não exportados + 0 exportados + 0 indeterminados**; existem **7 competências exportadas**. Para A18/A19, a sonda informa “sem alcance chamável”, com `linhas=null` e `mudariam_campo_lido_gravado_TXT=null`: esses valores não demonstram delta zero nem ausência de caminho futuro. (*m5_sonda.out*, anexo fornecido:10, :12 e :3–9; linhas contadas desde `sonda_leitura`.)

O BACKLOG chama os itens 18–19 de “FIM DO PROGRAMA”, e o registro do aval confirma uma lista de 26 itens, mas essas referências não descrevem seus campos ou funções. Equiparar essa numeração a A18/A19 seria **[não verificado]**. ([BACKLOG.md:1977](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/docs/BACKLOG.md:1977), [PROMPTS.md:2141](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/docs/PROMPTS.md:2141))

**O alcance comprovável do código**

| Caminho | O que o código demonstra |
|---|---|
| Backfill E4B existente | `--aplicar` exige competência corrente; o ato consulta novamente se o dia está entregue e o recusa antes da execução. Portanto, sua proteção não depende apenas do censo de hoje. ([backfill_fio_morto_e4b.py:223](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/management/commands/backfill_fio_morto_e4b.py:223), [:304](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/management/commands/backfill_fio_morto_e4b.py:304)) |
| Cascatas desse backfill | A cerca recusa linha alterada em exportada, fora da competência ou de alcance indeterminado; a tentativa sofre rollback. A fotografia inclui `FechamentoMensal` e `DiaPago`; DML em tabela fora dela também causa recusa. ([foto_e4b.py:18](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/foto_e4b.py:18), [:145](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/foto_e4b.py:145), [backfill_fio_morto_e4b.py:324](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/management/commands/backfill_fio_morto_e4b.py:324)) |
| Comando executado depois | `recalcular_fechamento --apply` chama `recalcular_fechamento_mes` pela competência solicitada. O serviço tem guarda nominal para **09/2026**, não uma recusa geral de exportadas; grava FM e chama a lavra de DiaPago. Há, portanto, caminho de escrita em outras competências exportadas, independentemente dos 581 dias atuais. ([recalcular_fechamento.py:118](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/management/commands/recalcular_fechamento.py:118), [fechamento.py:36](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/services/fechamento.py:36), [:560](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/services/fechamento.py:560), [:1160](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/services/fechamento.py:1160), [:1191](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/services/fechamento.py:1191)) |

Ligar uma alteração dos itens 18–19 a esse comando posterior, ou provar que eles ficam integralmente contidos na cerca do backfill, permanece **[não verificado]**.

**Quem escreve e quem lê**

Para **cada campo que os itens 18 e 19 gravariam**, campo, escritor e leitores estão **[não verificado]**: falta a definição dos itens e a sonda devolveu `null`, não uma relação de campos. (*m5_sonda.out*, anexo:3–9.)

Os destinos financeiros comprováveis são estes; sua atribuição aos itens permanece **[não verificado]**:

| Dados | Quem escreve | Quem lê |
|---|---|---|
| Campos calculados de `FechamentoMensal` | `recalcular_fechamento_mes`, incluindo horas, faltas, DSR, minutos e causas. ([fechamento.py:1160](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/services/fechamento.py:1160)) | `montar_linhas_export` consulta o FM gravado e entrega seus valores a `eventos_do_fechamento`. ([export.py:719](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/folha/export.py:719)) |
| Linhas de `DiaPago` | `dia_pago.lavrar`, que apaga a versão do escopo e recria as linhas. ([dia_pago.py:359](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/services/dia_pago.py:359)) | Leitores por campo e influência dos itens 18–19: **[não verificado]**. |
| `ExportacaoDominio.conteudo`, `hash_sha256` e metadados | `ExportacaoDominio.registrar`. ([models.py:174](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/folha/models.py:174)) | `competencia_entregue` lê vigência; `txt_vigente_x_gravado` lê o hash. ([export.py:588](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/folha/export.py:588), [:673](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/folha/export.py:673)) |

**Depende de X:** se os itens preservarem uma exclusão efetiva de qualquer escrita em exportada, essa autorização específica não é acionada; se alcançarem o GRAVADO exportado, inclusive por execução posterior, ela continua necessária. **Com a evidência disponível, não está demonstrada a dispensa para os itens 18–19.** ([CLAUDE.md:265](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/CLAUDE.md:265); correspondência item → campo → caminho: **[não verificado]**.)

## 15/11 09:51 · sessao fe287b9b65c1 · - · - · origem=Ronald (tmux)

Não foi localizado mapa confiável dos itens **18–19 do PRONTO** para funções/campos: `app/docs/BACKLOG.md:1977` apenas os chama de “FIM DO PROGRAMA”; `app/docs/PROMPTS.md:2141` informa N=26 e repete isso. Não confundir com A18/A19 da matriz de projeções, que representam outro inventário (`app/docs/PROMPTS.md:2034`; `app/docs/BACKLOG.md:1974`).

**O backfill existente tem cerca própria, independente do número de hoje:**

- Apply só na competência corrente: `app/chamados/management/commands/backfill_fio_morto_e4b.py:223`.
- Plano retira entregue/indeterminada dos executáveis: mesmo arquivo `:256`–`:262`.
- Revalidação de entregue dentro do ato: `:303`–`:308`.
- Delta de todos os campos concretos fotografados e rollback quando cruza competência/exportada: `:319`–`:334`; `app/chamados/services/foto_e4b.py:145`.
- Foto inclui FechamentoMensal e DiaPago (`foto_e4b.py:289`–`:290`); ExportacaoDominio não está na lista de tabelas permitidas (`:18`–`:27`), portanto DML nele cairia na recusa de tabela fora da foto (`backfill_fio_morto_e4b.py:325`–`:334`).
- Callbacks desconhecidos ou capazes de escapar do recorte recusam o ato: `foto_e4b.py:30`–`:84`.

**Mas o serviço compartilhado permite alcançar gravado de exportada:**

`recalcular_fechamento --apply` → `app/ponto/management/commands/recalcular_fechamento.py:118` → `recalcular_fechamento_mes` → persistência FM `app/ponto/services/fechamento.py:1144` / `:1160`, e DiaPago `:1191`–`:1201` → `app/ponto/services/dia_pago.py:359`–`:360`.

A guarda é nominal para **09/2026**, não geral: `fechamento.py:36`–`:43`, aplicada em `:560`–`:564`. O serviço declara que exportada deixou de ser trava (`:485`–`:498`) e o código só registra trilha (`:539`–`:547`). Portanto, uma execução futura noutra competência exportada pode escrever FM/DiaPago; influência dos itens 18–19 nesse caminho é **[não verificado]**. [nao verificado]

Caminho por evento também existe: `registro_batida.py:158`–`:180` → `fechamento.py:1809`–`:1824`. Porém materialização chama a porta com `fato_de_chao=False` (`materializacao.py:932`–`:939`), que consulta `competencia_fechada` (`registro_batida.py:132`–`:137`), barrando FM aprovado/exportado (`janelas.py:75`–`:89`). Essa guarda lê status do FM, não K6/ExportacaoDominio (`janelas.py:83`–`:88`).

**Exemplo de campos/escritores/leitores comprovado, somente se o item corresponder ao registro do passivo:**

`backfill_fio_morto_e4b.py:138`–`:141` chama `DecisaoPergunta.registrar(linhas_da_projecao_sem_linha(ps))`.

| Campos gravados | Quem escreve | Quem lê |
|---|---|---|
| `pergunta_id`, `ato`, `ato_em`, `ato_texto`, `ato_motivo`; na validação também `ato_por_id` | Valores montados em `app/chamados/models.py:4073`–`:4082`; persistência única `:4086`–`:4098` | `PerguntaDisputa.respostas_anteriores` lê a sequência e `ato` em `:3643`–`:3659`; tarefa lê último `ato`/`ato_texto` em `app/chamados/services/tarefa_do_dia.py:147`–`:161`; foto lê todos os campos concretos em `foto_e4b.py:247`–`:252`, `:280` |

O registro também acorda EventoCelula (`models.py:4097`–`:4098`; `:3314`–`:3331`), portanto nem esse ato deve ser tratado como inserção sem consumidores.

**Leitores do dinheiro:** `folha/export.py:782`–`:788` carrega FM gravado para classificação; rubricas leem `horas_extras_50`, extras100/feriado, folga trabalhada, noturnas e intrajornada em `:420`–`:448`. Isso comprova consumidores do gravado, mas não identifica quais campos os itens 18–19 mudariam **[não verificado]**.

Recomendação de veredito: **depende de X = definição exata dos itens 18–19 e sua vinculação a escritores/campos**. Zero atual sustenta ausência medida na fotografia fornecida; não sustenta ausência estrutural de caminho futuro.

## 15/11 09:51 · sessao 8a636440cc56 · - · - · origem=Ronald (tmux)

F1 | **nada diretamente pelo escopo leitor**; nenhum template/JS/view alvo no anexo, portanto BUG 73 não cria smoke obrigatório só por trocar o predicado. Filas de validação são alcançadas; eventual alteração de batida/dinheiro pelo diff futuro **[nao verificado]**; cadastro novo e remoção de arquivo **[nao verificado]** | BUG 73; PARADA 2 | `app/chamados/juizes.py:1528`, `app/chamados/juizes.py:1544`, `app/chamados/juizes.py:1554`, `app/chamados/juizes.py:1723`; `CLAUDE.md:202`, `CLAUDE.md:265`.

F2 | **nada diretamente para estado/seleção/apresentação**; a guarda de `_nascer` está em caminho quente de emissão, portanto conferir impacto de kernel; não há tela alvo direta nem cadastro variável novo demonstrado. Dinheiro, remoção de arquivo e diff final **[nao verificado]** | BUG 73; §4; TUDO TEM CADASTRO; PARADA 2 | `app/chamados/models.py:2903`, `app/chamados/models.py:2922`, `app/chamados/models.py:3117`, `app/chamados/models.py:3454`, `app/chamados/models.py:3640`, `app/chamados/models.py:3662`; `app/chamados/services/fio.py:1165`; `CLAUDE.md:202`, `CLAUDE.md:265`.

F3 | **esperar Ronald se o recorte mudar efeito de validação/batida ou dinheiro pago; `!` se mudar o ramo que grava folga**. Nenhum front alvo direto; validar/recusar aparece no detalhe/modal, mas smoke de clique obrigatório por BUG 73 somente por esse alcance **não está demonstrado**. Caminho real: validação → materialização → `registrar_batida`; validação também agenda fechamento na competência do fato, inclusive exportada. Cadastro novo/remoção de arquivo **[nao verificado]** | PRODUCAO-MAXIMA 5; PARADA 2; DINHEIRO 1–2; §4 | `app/chamados/services/validacao.py:346`, `app/chamados/services/validacao.py:460`, `app/chamados/services/validacao.py:470`; `app/ponto/services/declarar_dia.py:179`, `app/ponto/services/declarar_dia.py:191`; `app/chamados/services/materializacao.py:932`, `app/chamados/services/materializacao.py:1012`; `app/ponto/services/fechamento.py:1807`, `app/ponto/services/fechamento.py:1160`; `app/chamados/views.py:404`, `app/chamados/views.py:550`.

F4 | **esperar Ronald no alcance de plantio/retificação; `!` para executar o passivo em exportada com regeneração de DNA/gravado**. Nenhum front alvo direto. Caminho real: materialização planta batida; passivo planta → `regenerar_em_exportada` → Pauta DP quando mudou; alcança competência do dia do plantio. Quantas abertas/exportadas o pouso atingirá **[nao verificado]**. Cadastro novo/remoção de arquivo **[nao verificado]** | PRODUCAO-MAXIMA 5; PARADA 2; DINHEIRO 1–2; §4 | `app/chamados/services/materializacao.py:835`, `app/chamados/services/materializacao.py:932`, `app/chamados/services/materializacao.py:1012`; `app/chamados/services/passivo_validado.py:344`, `app/chamados/services/passivo_validado.py:433`, `app/chamados/services/passivo_validado.py:435`, `app/chamados/services/passivo_validado.py:437`, `app/chamados/services/passivo_validado.py:442`, `app/chamados/services/passivo_validado.py:448`.

F5 | **esperar Ronald para o recorte de kernel**: reconciliador roda por batida e fio tem guarda no caminho do chokepoint; nenhum template/JS/view alvo direto, então não há smoke obrigatório direto demonstrado por BUG 73. Dinheiro só pelo alcance comprovado de `processar_post_fio` → materialização; tarefa/Folha lê porta e não demonstra mover dinheiro por si. Cadastro novo, dado ESCALA/VÍNCULO, remoção de arquivo e impacto efetivo **[nao verificado]** | PRODUCAO-MAXIMA 5; §4; PARADA 2; DINHEIRO | `app/ponto/signals.py:206`, `app/ponto/signals.py:214`; `app/chamados/reconciliador.py:1429`, `app/chamados/reconciliador.py:1433`; `app/chamados/services/fio.py:1165`, `app/chamados/services/fio.py:701`, `app/chamados/services/fio.py:708`; `app/folha/fora_da_folha.py:63`, `app/folha/fora_da_folha.py:69`.

Cuidados para consolidar:

- **Não declarar exportada intacta em F3/F4:** o código real chama `recalcular_fechamento_mes` por evento; ele persiste aberto via `save` e demais status via `update`. Há apenas guarda específica de competência, sem veto genérico de exportada. `app/ponto/services/fechamento.py:1807`, `app/ponto/services/fechamento.py:1137`, `app/ponto/services/fechamento.py:1144`, `app/ponto/services/fechamento.py:1160`, `app/ponto/services/fechamento.py:558`.
- Telas de alcance, caso o recorte exija smoke: Central de Atendimento/lote, detalhe local, modal do fio, Meu Atendimento. A casca de Meu Atendimento é dinâmica; contexto escolhe `base.html` ou `base_app.html`. Isso é alcance funcional, não prova de que F1–F5 editam front. `app/templates/chamados/painel_gestao.html:14`, `app/chamados/views.py:550`, `app/chamados/views.py:927`, `app/chamados/views.py:632`, `app/templates/chamados/meu_atendimento.html:1`, `app/core/context_processors.py:210`.
- O anexo lista arquivos futuros, sem diff construído. Assim não há fundamento para prometer que não apagará arquivo, não tocará ramo monetário ou não exigirá cadastro; use **[nao verificado]** nesses negativos se a lista precisar ser categórica. [nao verificado]

## 15/11 09:51 · sessao 55637bbee891 · - · - · origem=Ronald (tmux)

E4d está descrita como CONTRACT para parar a projeção; os seis fatos próprios continuam registrados separadamente, “depois da E4d”, com projeção preservada até existirem. O aval fornecido pede E4d “com os fatos”; partição final e arquivos futuros não estão demonstrados. `app/docs/BACKLOG.md:1968`, `app/docs/BACKLOG.md:1970–1975`; [nao verificado].

| Fatia | Evidência para a linha consolidada |
|---|---|
| E4d CONTRACT | Parar escrita de projeção de resposta/validação exige conferir leitores do fio e atendimento: campos atuais em `app/chamados/models.py:2525–2527`, sequência em `app/chamados/models.py:3982–3992`, fio em `app/templates/chamados/partials/modal_fio.html:217–229`, atendimento em `app/templates/chamados/partials/_card_atendimento.html:189–192`. Smoke das telas é identificável; tocar template/base/JS, kernel, dinheiro, escala/vínculo, apagar arquivo e cadastro novo no diff futuro: [nao verificado]. |
| E4d FATO-A03 | Reabertura administrativa: `app/chamados/models.py:2190–2225`; casca executa a porta e cria pergunta de conversa: `app/chamados/services/acoes_disputa.py:235–274`; smoke do fio, “Reabertura administrativa”: `app/templates/chamados/partials/modal_fio.html:377–390`. Escritas demonstradas são disputa/chamado/pergunta, sem cadastro novo ou gravação direta de dinheiro/batida nessa porta. `app/chamados/services/acoes_disputa.py:259–274`. Alcance futuro [nao verificado]. |
| E4d FATO-A05 | BACKLOG só identifica “ramo de assinatura”; sítio inequívoco e casca específica não determinados: `app/docs/BACKLOG.md:1971`; [nao verificado]. Não confundir com `assinatura_proposta` do fio, que pertence à proposta de escala: `app/chamados/services/proposta_no_fio.py:96–113`. |
| E4d FATO-A06 | Identidade/duplicata escreve carimbo/via/motivo: `app/chamados/models.py:3251–3273`; dedup chama essa porta: `app/chamados/services/fio.py:1384–1401`. Smoke do atendimento exibindo “pergunta repetida”/via: `app/templates/chamados/partials/_card_atendimento.html:192`. Kernel e dinheiro não são escritos diretamente nessa porta; alcance futuro [nao verificado]. |
| E4d FATO-A17 | Pendência técnica escreve `pendencia_admin` + motivo dentro do materializador: `app/chamados/services/materializacao.py:851–857`; smoke fio e atendimento: `app/templates/chamados/partials/modal_fio.html:222–226`, `app/templates/chamados/partials/_card_atendimento.html:189`. Caminho é vizinho do plantio pelo chokepoint: `app/chamados/services/materializacao.py:932–940`; mudança efetiva no plantio/dinheiro depende do futuro diff [nao verificado]. |
| E4d FATO-A18 | Recusa técnica: `app/chamados/services/materializacao.py:1089–1113`; escreve motivo e, em colisão, identidade; anexa nota no fio. Smoke fio/atendimento: `app/templates/chamados/partials/modal_fio.html:226`, `app/templates/chamados/partials/_card_atendimento.html:192`. Integra caminho materializador; efeito futuro no plantio/dinheiro [nao verificado]. |
| E4d FATO-A20 | Conferência de data: `app/chamados/services/validacao.py:660–709`; view `app/chamados/views.py:1292–1314`; smoke “Datas em disputa / Conferir data”: `app/templates/chamados/partials/modal_fio.html:163–177`. Destrava gate materializador: `app/chamados/services/materializacao.py:1199–1220`; plantio subsequente alcança chokepoint em `app/chamados/services/materializacao.py:932–940`. Regra do par já decidida: `app/docs/CORTES.md:440`; implementação dessa decisão não demonstrada. |

**Dinheiro: cuidado material, sem inventar universo afetado.** Materializador chama `registrar_batida`; batida gravada chama fechamento por evento; fechamento recalcula a competência do fato em `on_commit`. `app/chamados/services/materializacao.py:932–940`, `app/ponto/registro_batida.py:158–180`, `app/ponto/services/fechamento.py:1798–1824`. A chamada real não filtra exportada, e o comentário explicita recalcular exportada também. `app/ponto/services/fechamento.py:1790–1793`, `app/ponto/services/fechamento.py:1809–1810`. Portanto, A17/A18/A20 e CONTRACT não podem receber “dinheiro=nada” sem provar que a migração conserva o caminho; competências, valores e frota atingidos são [nao verificado].

**Cláusulas para consolidar:** aberta pode aplicar com reversão anterior, hash das exportadas intacto e prova de frota posterior; exportada gravada exige `!` e porta REGEN. `CLAUDE.md:406–417`. Kernel de batida aguarda Ronald segundo PRODUCAO-MAXIMA item 5 fornecido; regra de chokepoint está em `CLAUDE.md:143–144`. Escala/vínculo, motor ou apagar/voltar arquivo usado por produção exigem `!`; nenhum desses atos consta do escopo resumido dos seis fatos, mas diff futuro permanece [nao verificado]. `CLAUDE.md:265–268`, `app/docs/BACKLOG.md:1970–1975`. Novo cadastro de regra variável depende de UI; nenhuma variação por cliente/praça/empresa está descrita nesses fatos. `CLAUDE.md:43–44`, `app/docs/BACKLOG.md:1970–1975`.

## 15/11 09:49 · sessao 5ac9eee1605f · E3-RESTO-R3 · construtor · origem=Code

Os 11 testes foram reescritos pela lei (b), com antes/depois e provas conservadas na [entrega](/home/ronald/wt-codex-e3-resto-r3-093851/app/chamados/tests/e3r3_c9_entrega.txt:27). [nao verificado]

Os 11 pares mutante/teste e controles GREEN→RED→GREEN estão no [script](/home/ronald/wt-codex-e3-resto-r3-093851/app/chamados/tests/e3r3_c9_mutantes.py:26). [nao verificado]

[Ruff e py_compile passaram](/home/ronald/wt-codex-e3-resto-r3-093851/app/chamados/tests/e3r3_c9_entrega.txt:221). Suíte e mutantes não foram executados; resultados funcionais: [nao verificado]. [nao verificado]

## 15/11 09:49 · sessao d533b23fcb35 · JUSTIFICATIVA-PAGA-O-PERIODO · censo · origem=Code

**A guarda atual não protege a 10/2026 por pagamento externo.** Ela exige simultaneamente a chave `(9, 2026)` e empresa exportada; o teste da 10 exige que o evento regrave o fechamento. O marco chamado `paga` consulta holerite de lote publicado, sem consultar declaração de pagamento manual. (`app/ponto/services/fechamento.py:37`, `app/ponto/services/fechamento.py:560`, `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:200`, `app/ponto/services/fechamento.py:1552`)

**1. Leitores da guarda e do contador**

| Leitor | Significado da chave e leitura por empresa |
|---|---|
| `recalcular_fechamento_mes` | Usa `(mes, ano)` da **competência de apuração**, com janela pelo corte da empresa. Consulta a guarda e exclui somente colaboradores das empresas exportadas. A chave sozinha não bloqueia empresa sem entrega. (`app/ponto/services/fechamento.py:539`, `app/ponto/services/fechamento.py:560`, `app/ponto/services/fechamento.py:562`, `app/ponto/services/fechamento.py:620`) |
| `dia_pago.retratar_exportada` | Usa a competência das linhas de `FechamentoMensal`/`DiaPago`. Exige chave guardada e empresa exportada, encontrada por `colaborador_ids=[colab.pk]`; devolve `barrado_pela_guarda=True`. (`app/ponto/services/dia_pago.py:459`, `app/ponto/services/dia_pago.py:468`, `app/ponto/services/dia_pago.py:469`) |
| `barrados_pela_guarda` | Lê `ACAO_BARRADO_GUARDA` e conta registros de `LogAuditoria` cujo texto começa com `MM/AAAA `. Não calcula janela nem filtra empresa. (`app/ponto/services/fechamento.py:47`, `app/ponto/services/fechamento.py:50`) |
| Command `recalcular_fechamento` | Depois do apply, testa a chave e imprime contador acumulado e `saida`. O recálculo pode receber empresa; o contador impresso recebe somente mês/ano. (`app/ponto/management/commands/recalcular_fechamento.py:118`, `app/ponto/management/commands/recalcular_fechamento.py:123`, `app/ponto/management/commands/recalcular_fechamento.py:126`) |
| `test_bob_b2_abono_feriado_guarda` | Cenário com corte 21 e exportação 09: exige 09 preservada, contador 1; 10 regravada, contador 0; leitura pura da 09 permitida; retrato da 09 barrado. (`app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:79`, `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:175`, `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:192`, `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:200`, `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:206`, `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:211`) |
| `test_reverter_regen_recalcula_dinheiro` | Lê o contador antes/depois da reversão de célula em 09 exportada; exige célula revertida, dinheiro preservado e contador aumentado. O contador continua sem empresa. (`app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:197`, `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:205`, `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:211`) |
| `test_justificativa_c9_sem_banco` | Injeta guarda vazia e ação fictícia para testar retrato sem ata; não caracteriza competência nem entrega real. (`app/ponto/tests/test_justificativa_c9_sem_banco.py:138`, `app/ponto/tests/test_justificativa_c9_sem_banco.py:150`) |

`ACAO_BARRADO_GUARDA='recalculo_barrado_guarda'` é escrita pelo recálculo barrado, com modelo `FechamentoMensal`, e pelo retrato barrado, com modelo `DiaPago`; seu leitor funcional é o contador acima. (`app/ponto/services/fechamento.py:44`, `app/ponto/services/fechamento.py:50`, `app/ponto/services/fechamento.py:569`, `app/ponto/services/dia_pago.py:463`)

O contador mede **registros de bloqueio**, sem somar pessoas: uma chamada pode excluir vários colaboradores e produzir uma linha, com quantidade no texto e IDs no JSON. Ele também inclui o bloqueio do retrato. (`app/ponto/services/fechamento.py:569`, `app/ponto/services/fechamento.py:570`, `app/ponto/services/fechamento.py:573`, `app/ponto/services/dia_pago.py:463`)

A competência termina no mês da chave: com corte 21, `10/2026` significa `21/09–20/10`; o corte vem de `Empresa.dia_inicio_competencia`, com default 21. O corte efetivo cadastrado de Juliani em produção é [nao verificado]. (`app/ponto/janelas.py:10`, `app/ponto/janelas.py:36`, `app/ponto/janelas.py:54`)

**2. Candidatas a responder exportada, entregue, paga, fechada ou trancada**

O registro vivo declara separadamente F6, K4, K6, K7 e K9; ele distingue expressamente fechamento por colaborador/status e tranca por empresa. (`app/core/juizes.py:76`, `app/core/juizes.py:309`, `app/core/juizes.py:326`, `app/core/juizes.py:337`, `app/core/juizes.py:338`, `app/core/juizes.py:347`)

| Candidata | Predicado, universo e fonte |
|---|---|
| `competencia_entregue` — K6 | **Empresa + mês + ano**: seleciona o `ExportacaoDominio` mais recente por `(gerado_em, pk)` e exige `invalidada_em is None`. Retorna bool ou registro. Responde entrega por TXT. (`app/folha/export.py:559`, `app/folha/export.py:591`, `app/folha/export.py:594`, `app/folha/models.py:203`) |
| `empresas_exportadas_no_escopo` | Projeta a pergunta anterior: empresa indicada; senão empresas dos colaboradores indicados; senão empresas ativas. Retorna IDs cujo marco `exportada` é verdadeiro. Lista vazia de colaboradores produz escopo vazio. (`app/ponto/services/fechamento.py:1509`, `app/ponto/services/fechamento.py:1521`, `app/ponto/services/fechamento.py:1528`) |
| `marcos_da_competencia()['aprovada']` | **Empresa + competência**: existe ao menos um `FechamentoMensal` de colaborador da empresa com status `aprovado` ou `exportado`. (`app/ponto/services/fechamento.py:1546`) |
| `marcos_da_competencia()['exportada']` | Chama K6; mesma pergunta de entrega por TXT. (`app/ponto/services/fechamento.py:1550`) |
| `marcos_da_competencia()['trancada']` | Chama `competencia_trancada`; mesma pergunta da tranca viva. (`app/ponto/services/fechamento.py:1551`) |
| `marcos_da_competencia()['paga']` | **Empresa + competência**: existe `Holerite` com lote publicado, atribuído pela `empresa_folha` ou, se ela for nula, pela empresa atual do colaborador. Um único documento satisfaz o `exists()`; não exige todos os colaboradores nem prova bancária. (`app/ponto/services/fechamento.py:1552`) |
| `marco_da_competencia` | Retorna o verdadeiro mais adiantado na ordem `aprovada → exportada → trancada → paga`, ou `aberta`. É projeção dos marcos. (`app/ponto/services/fechamento.py:1474`, `app/ponto/services/fechamento.py:1555`, `app/ponto/services/fechamento.py:1566`) |
| `competencia_trancada` — K4 | **Empresa + competência**: existe `PeriodoFechado` com `reaberto_em IS NULL`. Retorna bool; sem empresa retorna falso. (`app/ponto/services/fechamento.py:1486`) |
| `periodo_trancado` — K7 | Exatamente o mesmo filtro do K4, devolvendo linha ou `None`. `ativo` não participa da decisão. (`app/ponto/services/fechamento.py:1477`, `app/ponto/services/fechamento.py:1483`) |
| `janelas.competencia_fechada` — F6, “mês fechado” | **Colaborador + instante**: converte para data local, encontra competência pelo corte da empresa e procura FM `aprovado`/`exportado`. Retorna mensagem ou `''`. Não consulta TXT, holerite ou tranca. (`app/ponto/janelas.py:75`, `app/ponto/janelas.py:84`) |
| `competencia_fechada_para_celula` — K9 | **Empresa + competência + colaborador opcional**: K6 **OU** holerite do próprio colaborador com lote publicado. Sem colaborador, somente TXT conta; `fontes=True` informa as metades verdadeiras. (`app/folha/export.py:600`, `app/folha/export.py:620`) |
| `_competencias_exportadas`, `fonte_da_lavra`, `autoridade_da_lavra` | Enumeram candidatos de TXT/holerite, consultam K9 e convertem em janelas pelo corte da empresa; apresentam qual fonte intercepta o recorte. (`app/ponto/management/commands/gerar_celulas.py:135`, `app/ponto/management/commands/gerar_celulas.py:170`, `app/ponto/management/commands/gerar_celulas.py:182`, `app/ponto/management/commands/gerar_celulas.py:192`, `app/ponto/management/commands/gerar_celulas.py:217`) |
| `dia_em_competencia_trancada` | **Empresa + dia**: enumera `PeriodoFechado` sem reabertura e testa pertencimento à janela. Sem empresa/dia retorna falso; trata falhas com abertura. (`app/chamados/juizes.py:2041`, `app/chamados/juizes.py:2051`) |
| `dia_da_pergunta_trancado` / `dias_trancados` — C20 | **Colaborador + dia de turno**: tranca viva **e ausência de toque administrativo posterior naquele colaborador/dia**. Lê K7 e trilha de cobrar/resolver/desarquivar. (`app/chamados/juizes.py:974`, `app/chamados/juizes.py:1004`, `app/chamados/juizes.py:1066`, `app/chamados/juizes.py:1076`, `app/chamados/juizes.py:1083`) |
| `ChamadoColaborador._dia_em_competencia_trancada` | Excetua módulos fora da tranca, obtém a competência do chamado e consulta K4. Decide escopo do chamado. (`app/chamados/models.py:1080`) |
| `competencia_do_dia_entregue` | **Empresa + dia**: compõe `janela_atual` e K6. Sem dia retorna `None`; memo por empresa/competência. (`app/chamados/services/vigia_fio.py:827`, `app/chamados/services/vigia_fio.py:839`) |
| `autorizacao_he_periodo._exportada` | Testa se a empresa do colaborador pertence a `empresas_exportadas_no_escopo`; mesma entrega TXT. (`app/ponto/services/autorizacao_he_periodo.py:113`) |
| `tripwire_celulas._exportada` | Testa se a data cai nas janelas de `_competencias_exportadas(empresa)`. Sem colaborador, alcança somente a metade TXT do K9. (`app/ponto/management/commands/tripwire_celulas.py:318`, `app/folha/export.py:623`) |
| `ExportacaoDominio.e_vigente`, `anotar_vigencia`, `so_vigentes` | Respondem **qual geração é a mais recente** por empresa/competência; não excluem o registro por `invalidada_em`. (`app/folha/models.py:120`, `app/folha/models.py:134`, `app/folha/models.py:192`, `app/folha/models.py:203`) |
| `exportacoes_da_competencia` / `passivo_validado._esta_exportada` | O primeiro devolve o mais recente por `e_vigente`; o segundo testa `vigente is not None`. Um último TXT invalidado pode dar verdadeiro aqui e falso no K6. (`app/folha/haiku_export.py:59`, `app/folha/haiku_export.py:70`, `app/chamados/services/passivo_validado.py:116`, `app/chamados/services/passivo_validado.py:127`, `app/folha/export.py:594`) |
| `ExportacaoDominio.registrar → ja_existia` | Testa existência de **qualquer geração anterior** da tripla. Marca reexportação; não exige entrega vigente nem pagamento. (`app/folha/models.py:174`, `app/folha/models.py:178`, `app/folha/models.py:186`) |

Os chamadores das candidatas ficam assim:

- **K6 direto:** marcos, preview, comparador de TXT e K9; ranking HE; Pautas pago/devido e folga exportada; Esmeril; foto E4b; vigia do fio; tarefa do dia; competências a lavrar; contagem de dias abonados; backfill de decisões; lavratura de status. (`app/ponto/services/fechamento.py:1550`, `app/folha/views.py:354`, `app/folha/export.py:621`, `app/folha/export.py:658`, `app/folha/services/ranking_he.py:89`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:124`, `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:93`, `app/ponto/services/esmeril_espelho.py:58`, `app/chamados/services/foto_e4b.py:245`, `app/chamados/services/vigia_fio.py:846`, `app/chamados/services/tarefa_do_dia.py:442`, `app/ponto/services/he_pendente_lavrado.py:223`, `app/ponto/services/dia_pago.py:717`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:471`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:498`, `app/ponto/management/commands/lavrar_status_competencia.py:43`)
- **Marcos:** projeções `empresas_exportadas_no_escopo` e `marco_da_competencia`, pré-fechamento e reabertura de órfãs enterradas. (`app/ponto/services/fechamento.py:1528`, `app/ponto/services/fechamento.py:1568`, `app/folha/management/commands/pre_fechamento.py:74`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:95`)
- **K4/K7:** marcos, chamado, painel/tela/reabertura de fechamento, juiz das perguntas e serviço de encerramento por competência trancada. (`app/ponto/services/fechamento.py:1551`, `app/chamados/models.py:1091`, `app/ponto/views.py:1030`, `app/ponto/views.py:1129`, `app/ponto/views.py:1543`, `app/chamados/juizes.py:1066`, `app/chamados/services/competencia_trancada.py:95`)
- **F6:** registro de batida, correção de tipo e lavratura de foto de folgas. (`app/ponto/registro_batida.py:135`, `app/ponto/services/flip_batida.py:22`, `app/escala/management/commands/lavrar_foto_folgas.py:95`)
- **K9 e suas janelas:** gerador de células, apply específico, regeneração/edição/reversão de células, correção retroativa de escala, vigência impossível, tripwire e auditor de invariantes. (`app/ponto/management/commands/gerar_celulas.py:182`, `app/ponto/management/commands/gerar_celulas.py:212`, `app/ponto/management/commands/aplicar_col900.py:64`, `app/ponto/portas/celula.py:221`, `app/ponto/portas/celula.py:607`, `app/ponto/portas/celula.py:821`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:82`, `app/escala/services/vigencia_impossivel.py:42`, `app/ponto/management/commands/tripwire_celulas.py:319`, `app/chamados/management/commands/auditar_invariantes_chamados.py:372`)
- **C20:** C6, consulta em lote, respostas sem veredito, reconciliador, tarefa do dia e emissão de disputa. O juiz simples de dia trancado é chamado por `furos_sem_cobranca`; o método do chamado também é lido por `cluster_espurio`. (`app/chamados/juizes.py:1405`, `app/chamados/juizes.py:1149`, `app/chamados/models.py:1902`, `app/chamados/reconciliador.py:668`, `app/chamados/reconciliador.py:748`, `app/chamados/services/tarefa_do_dia.py:422`, `app/chamados/services/disputa_emissao.py:1970`, `app/ponto/services/furos_sem_cobranca.py:85`, `app/ponto/services/cluster_espurio.py:254`)
- **Entrega por dia:** modelos, reconciliador, fio, foto E4b, emissão de disputa, fábrica da célula, backfill E4b e rederivação H06. O passivo validado chama sua própria `_esta_exportada`. (`app/chamados/models.py:364`, `app/chamados/reconciliador.py:745`, `app/chamados/services/fio.py:777`, `app/chamados/services/fio.py:1317`, `app/chamados/services/foto_e4b.py:238`, `app/chamados/services/disputa_emissao.py:1837`, `app/chamados/services/fabrica_da_celula.py:231`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:173`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:115`, `app/chamados/services/passivo_validado.py:358`)

**As equivalências são limitadas:** K6, marco `exportada`, empresas exportadas e entrega por dia compartilham a pergunta de TXT; K4/K7 compartilham tranca. F6 consulta status individual; C20 acrescenta toque administrativo; K9 acrescenta documento individual; `paga` é existência de holerite publicado. Nenhum desses predicados consulta pagamento manual declarado. (`app/folha/export.py:591`, `app/ponto/services/fechamento.py:1528`, `app/ponto/services/fechamento.py:1483`, `app/ponto/services/fechamento.py:1493`, `app/ponto/janelas.py:86`, `app/chamados/juizes.py:1078`, `app/folha/export.py:625`, `app/ponto/services/fechamento.py:1552`)

Também há perguntas temporais: `janela_anterior` responde competência encerrada no calendário; `versao_para` e `sem_adesao_no_horizonte` respondem horizontes de regra. Não consultam pagamento. O comentário do horizonte registra a 10 paga fora, mas o corpo compara competência com `'2026-11'`. (`app/ponto/janelas.py:66`, `app/ponto/services/cartorio.py:258`, `app/ponto/services/cartorio.py:280`, `app/ponto/services/cartorio.py:291`)

**3. Cadastro de competência paga fora do sistema**

Não localizei cadastro específico para declarar **empresa + competência paga externamente**. Inexistência absoluta fora da cópia pesquisada: [nao verificado]. Nos modelos pertinentes encontrados:

| Modelo | O que registra e quem escreve |
|---|---|
| `PeriodoFechado` — mais próximo estruturalmente | Empresa, competência, autor/data da tranca e reabertura; não contém declaração de pagamento. Escrito por `trancar_periodo` e `reabrir_periodo`. A aprovação da tela pode trancar; a reabertura chama a porta e registra auditoria. (`app/ponto/models.py:920`, `app/ponto/services/fechamento.py:1413`, `app/ponto/services/fechamento.py:1445`, `app/ponto/views.py:1369`, `app/ponto/views.py:1546`, `app/ponto/views.py:1548`) |
| `ExportacaoDominio` | Geração do TXT, autor, nome, hash e conteúdo. `registrar` cria o histórico da geração. (`app/folha/models.py:75`, `app/folha/models.py:173`) |
| `FechamentoMensal` | Por colaborador: estados aberto/aprovado/exportado, aprovação e observação; não há estado `pago`. (`app/ponto/models.py:398`, `app/ponto/models.py:471`) |
| `Holerite` / `LoteHolerite` | Documento e publicação do lote; essa publicação alimenta o marco `paga`. O command de processamento carimba `publicado_em`. (`app/holerite/models.py:6`, `app/holerite/models.py:103`, `app/holerite/management/commands/processar_lote_holerite.py:123`, `app/ponto/services/fechamento.py:1552`) |

O modelo próximo, `PeriodoFechado`, não está registrado no admin de ponto mostrado nessa cópia. (`app/ponto/admin.py:1`)

Há uma diferença temporal adicional: a matriz de holerites usa mês civil, enquanto a apuração de ponto usa o corte da empresa. O registro vivo documenta essa distinção. (`app/holerite/matriz.py:51`, `app/ponto/janelas.py:10`, `app/core/juizes.py:615`)

**4. O que impede regravar a 10 e quais escritores passam pela guarda**

**Não há bloqueio da 10 por “competência paga” no recálculo atual.** A guarda contém somente 09; status aprovado também não impede esse escritor: aberto recebe `save()`, demais estados recebem `queryset.update()` dos campos calculados. O bloqueio aprovado→aprovado do model só atua no `save()`. O contrato existente exige que recálculo não consulte tranca. (`app/ponto/services/fechamento.py:37`, `app/ponto/services/fechamento.py:1150`, `app/ponto/services/fechamento.py:1173`, `app/ponto/models.py:492`, `app/ponto/tests/test_contract_juiz_fechamento.py:117`)

Isso descreve a proteção por competência; a execução concreta para Juliani, seu universo elegível e eventuais falhas de cálculo são [nao verificado]. O serviço filtra o universo e captura falhas por colaborador. (`app/ponto/services/fechamento.py:554`, `app/ponto/services/fechamento.py:1316`)

| Escritor | Escrita e passagem pela guarda |
|---|---|
| `recalcular_fechamento_mes` | Cria/salva/atualiza FM e chama lavratura de DP `motor` e `oraculo`. **Passa:** colaboradores barrados saem antes do laço. (`app/ponto/services/fechamento.py:564`, `app/ponto/services/fechamento.py:903`, `app/ponto/services/fechamento.py:1157`, `app/ponto/services/fechamento.py:1173`, `app/ponto/services/fechamento.py:1214`, `app/ponto/services/fechamento.py:1290`) |
| `dia_pago.lavrar` | Apaga e recria as linhas do colaborador/competência/versão. **Sem guarda própria**; suas chamadas funcionais estão dentro do recálculo guardado. (`app/ponto/services/dia_pago.py:135`, `app/ponto/services/dia_pago.py:359`, `app/ponto/services/fechamento.py:1214`, `app/ponto/services/fechamento.py:1290`) |
| `retratar_exportada` | Atualiza/cria campos de grade, ajuste DSR/banco e carimbos. **Tem guarda própria.** (`app/ponto/services/dia_pago.py:459`, `app/ponto/services/dia_pago.py:495`, `app/ponto/services/dia_pago.py:518`, `app/ponto/services/dia_pago.py:552`, `app/ponto/services/dia_pago.py:559`) |
| `apurar_furo_da_exportada` | Atualiza apenas `furo_apurado` e `lavrado_em` nas linhas ainda não apuradas. **Não consulta a guarda.** (`app/ponto/services/dia_pago.py:664`, `app/ponto/services/dia_pago.py:685`, `app/ponto/services/dia_pago.py:687`) |
| `aprovar_lote` / `aprovar` | Status e carimbos de aprovação, eventualmente observação. **Não consultam a guarda.** (`app/ponto/services/fechamento.py:1336`, `app/ponto/services/fechamento.py:1348`) |
| `reabrir_lote` / `reabrir` | Status aberto e limpeza da aprovação. **Não consultam a guarda.** (`app/ponto/services/fechamento.py:1357`, `app/ponto/services/fechamento.py:1362`) |
| `restaurar_fechamento` | Atualiza campos fornecidos e invalida `derivado_em` por padrão; exige motivo. **Não consulta a guarda.** (`app/ponto/services/fecham

[truncado em 20000 de 29035 caracteres]

## 15/11 09:45 · sessao 0dbb05660749 · - · - · origem=Ronald (tmux)

Censo das perguntas 2 e 3, somente código.

| Candidata | Predicado exato, universo e fonte | Quem chama |
|---|---|---|
| `competencia_entregue` (_K6) | Por `(empresa, mes, ano)`: seleciona o `ExportacaoDominio` mais recente, desempate `(gerado_em,pk)`, e exige `invalidada_em is None`; retorna bool ou o registro. É entrega por TXT, não pagamento externo. `app/folha/export.py:559,583-597`; `app/folha/models.py:203-225`. | `folha/views.py:354`; `folha/export.py:621,658`; `folha/services/ranking_he.py:89`; `ponto/services/fechamento.py:1550`; `pautas/management/commands/pauta_pago_x_devido_exportada.py:124`; `pauta_folga_nao_definida_exportada.py:93`; `ponto/services/esmeril_espelho.py:58`; `chamados/services/foto_e4b.py:245`; `vigia_fio.py:846,848`; `tarefa_do_dia.py:442`; `ponto/services/he_pendente_lavrado.py:223`; `dia_pago.py:717`; `chamados/management/commands/backfill_decisoes_pergunta.py:471,498`; `ponto/management/commands/lavrar_status_competencia.py:43`. Todos caminhos com prefixo `app/`. |
| `empresas_exportadas_no_escopo` | Projeção do mesmo K6: empresa indicada; senão empresas dos colaboradores indicados; senão empresas ativas. Retorna IDs cujo `marcos_da_competencia()['exportada']` é verdadeiro; `[]` de colaboradores é vazio. `app/ponto/services/fechamento.py:1509-1528`. | O próprio recalculador/carimbo L-113 `app/ponto/services/fechamento.py:539`; demais consumidores ficam com o outro explorador. |
| `marcos_da_competencia` | Por empresa+competência. `aprovada`: existe FM de colaborador da empresa com status aprovado/exportado. `exportada`: K6. `trancada`: K4. `paga`: existe `Holerite` na competência com **lote publicado**, atribuído pela `empresa_folha` ou, se nula, cadastro atual do colaborador. Um único holerite satisfaz o `exists()`, não exige todos pagos. `app/ponto/services/fechamento.py:1531-1557`. | Wrapper exportadas `:1528`, `marco_da_competencia :1568`; `app/folha/management/commands/pre_fechamento.py:74`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:95`. |
| `marco_da_competencia` | Retorna o marco verdadeiro mais adiantado na ordem aprovada/exportada/trancada/paga; caso contrário aberta. Não é outro predicado de pagamento. `app/ponto/services/fechamento.py:1474,1555-1557,1566-1568`. | Leitores de tela/placar: conferir censo do agente de consumidores. |
| `competencia_trancada` (_K4) / `periodo_trancado` (_K7) | Mesmíssimo filtro `PeriodoFechado(empresa_id,mes,ano,reaberto_em IS NULL)`. K4 retorna bool; K7 retorna linha/None. `ativo` não decide. Sem empresa, falso/None. `app/ponto/services/fechamento.py:1477-1493`. | K4: marcos `:1551`, `app/chamados/models.py:1091`. K7: `app/ponto/views.py:1030,1129,1543`; `app/chamados/juizes.py:1066`; `app/chamados/services/competencia_trancada.py:95`. |
| `ponto.janelas.competencia_fechada` (_F6, “mês fechado”) | Por colaborador+dia: timestamp convertido a data local; `janela_atual` com corte da empresa; procura FM daquela competência; aprovado/exportado devolve mensagem de recusa, demais devolvem `''`. Não lê TXT, tranca ou pagamento. `app/ponto/janelas.py:75-89`. | `app/ponto/registro_batida.py:135`; `app/ponto/services/flip_batida.py:22`; `app/escala/management/commands/lavrar_foto_folgas.py:95`. |
| `competencia_fechada_para_celula` (_K9) | K6 **OU** holerite do próprio colaborador na competência com lote publicado; sem colaborador só TXT conta. `fontes=True` informa exportação/holerite. `app/folha/export.py:600-631`. | `app/ponto/management/commands/gerar_celulas.py:182,212`; `app/ponto/management/commands/aplicar_col900.py:64`. |
| `_competencias_exportadas` / `fonte_da_lavra` / `autoridade_da_lavra` | Enumera pares de ED e holerites publicados, pergunta ao K9, converte à janela da empresa. As outras funções apresentam a fonte e a janela que intercepta o recorte. Não julgam pagamento. `app/ponto/management/commands/gerar_celulas.py:135-189,192-229`. | `app/ponto/portas/celula.py:221,607,613,821`; `app/colaboradores/management/commands/corrigir_escala_retroativa.py:82`; `app/escala/services/vigencia_impossivel.py:42`; `app/chamados/management/commands/auditar_invariantes_chamados.py:372,396,422,1549`; `app/ponto/management/commands/tripwire_celulas.py:319`. |
| `dia_em_competencia_trancada` | Por empresa+dia: enumera `PeriodoFechado` vivos e verifica se dia pertence à `janela_fechamento`; sem empresa/dia falso, tratamento de exceção fail-open. `app/chamados/juizes.py:2041-2059`. | `app/ponto/services/furos_sem_cobranca.py:85`. O cartório registra a retirada desse leitor em `app/ponto/services/cartorio.py:1713`. |
| `dia_da_pergunta_trancado` / `dias_trancados` (_C20) | Por colaborador+dia de turno: tranca viva da competência **e nenhum toque administrativo posterior nesse colab/dia**. Consulta `PeriodoFechado` via K7 e trilha `LogAuditoria` de cobrar/resolver/desarquivar; conversa não tem competência de fato. `app/chamados/juizes.py:974-992,1004-1008,1062-1093`. | C6 `app/chamados/juizes.py:1405`; lote `:1149`; `app/chamados/models.py:1902`; reconciliador `:668,748`; `services/tarefa_do_dia.py:422`; `services/disputa_emissao.py:1970`. |
| `ChamadoColaborador._dia_em_competencia_trancada` | Excetua módulos fora da tranca, obtém competência do chamado e pergunta K4. É escopo de renascimento de chamado. `app/chamados/models.py:1080-1091`. | `app/chamados/models.py:1056`; `app/ponto/services/cluster_espurio.py:254`. |
| `competencia_do_dia_entregue` | Composição data→`janela_atual`→K6; `None` sem dia, memo por empresa+competência. Mesma entrega TXT, com entrada por data. `app/chamados/services/vigia_fio.py:827-849`. | `app/chamados/models.py:364`; `reconciliador.py:745`; `services/fio.py:777,1317`; `foto_e4b.py:238`; `disputa_emissao.py:1837`; `vigia_fio.py:821`; `fabrica_da_celula.py:231`; `management/commands/backfill_fio_morto_e4b.py:173,255,303`; `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:115`. |
| `autorizacao_he_periodo._exportada` | Pergunta se empresa do colaborador pertence ao resultado de `empresas_exportadas_no_escopo`. Mesma entrega TXT. `app/ponto/services/autorizacao_he_periodo.py:113-118`. | `app/ponto/services/autorizacao_he_periodo.py:288`. |
| `tripwire_celulas._exportada` | Verifica data dentro das janelas retornadas por `_competencias_exportadas(empresa)`; sem colaborador, portanto só a metade TXT de K9. `app/ponto/management/commands/tripwire_celulas.py:318-322`. | Mesmo command `:346`. |
| `ExportacaoDominio.e_vigente` / `anotar_vigencia` / `so_vigentes` | “Qual registro é mais recente?” por empresa+competência. Não excluem `invalidada_em`. `app/folha/models.py:120-141,192-225`. | K6 `app/folha/export.py:591-594`; memória `app/folha/haiku_export.py:67-75`; listas/histórico e command de regeração são outros consumidores. |
| `exportacoes_da_competencia` / `passivo_validado._esta_exportada` | O primeiro devolve mais recente por `e_vigente`; o segundo testa `'vigente' is not None`. **Não são estritamente equivalentes a K6**: um último TXT invalidado sem substituto ainda aparece aqui, enquanto K6 responde falso. `app/folha/haiku_export.py:59-75`; `app/chamados/services/passivo_validado.py:116-128`; comparação K6 `app/folha/export.py:594-597`. | `_esta_exportada` chamado em `app/chamados/services/passivo_validado.py:358`. |

Registro vivo confirma F6 em `app/core/juizes.py:309` e K4/K6/K7/K9 em `app/core/juizes.py:326-347`; o próprio registro distingue “fechada por colaborador/status” de “trancada por empresa” em `app/core/juizes.py:76-85`.

Não encontrei modelo/campo específico para declarar “paga fora do sistema”; inexistência absoluta além da cópia pesquisada [nao verificado]. O modelo estruturalmente mais próximo por **empresa+competência com autor/data** é `PeriodoFechado`, que registra **tranca**, com reabertura, e não pagamento: `app/ponto/models.py:920-949`. Escrevem nele `trancar_periodo` e `reabrir_periodo`, em `app/ponto/services/fechamento.py:1413-1465`; a tela aprovar chama trancar quando há empresa e ninguém foi pulado, `app/ponto/views.py:1365-1370`; reabrir chama a porta e registra log em `app/ponto/views.py:1543-1548`. Não está registrado no admin de ponto mostrado em `app/ponto/admin.py:1-48`.

`ExportacaoDominio` guarda nome/hash/conteúdo do TXT, autor e geração, não declaração externa: `app/folha/models.py:75-86`; seu escritor `registrar` cria histórico de gerações em `app/folha/models.py:173-188`. `FechamentoMensal` guarda por colaborador status aberto/aprovado/exportado, aprovação e observação, sem status pago: `app/ponto/models.py:398-410,471-485`. `Holerite`/`LoteHolerite` permitem representar documento publicado, não pagamento manual declarado: `app/holerite/models.py:6-30,103-123`; quem carimba publicação do lote é `app/holerite/management/commands/processar_lote_holerite.py:123-130`.

A própria cópia documenta o desencontro entre pagamento externo e o marco “paga”: julho/agosto pagos à mão em `app/ponto/services/fechamento.py:500-504`, e julho com `marcos['paga']=False` apesar de paga fora em `app/ponto/management/commands/passivo_disputa_retro.py:37-38`. Para outubro há comentário “Comp <= 10 conserva [...] inclusive a 10 paga fora do sistema”, em `app/ponto/services/cartorio.py:282`; isso é corte temporal em código, não cadastro. Estado real dessas competências em produção [nao verificado].

## 15/11 09:45 · sessao cf072147f76f · - · - · origem=Ronald (tmux)

Q1 — leitores diretos encontrados:

| Leitor | Leitura e sentido de `(mes, ano)` | Empresa? |
|---|---|---|
| `recalcular_fechamento_mes` | `GUARDA_COMPETENCIA.get((mes, ano))`; usa a competência da folha, janela por corte da empresa; exclui do universo somente os colaboradores das empresas exportadas. `app/ponto/services/fechamento.py:539`, `:554`, `:560`, `:562`, `:564`; janela em `app/ponto/janelas.py:10`, `:54`. | Sim; consulta `empresas_exportadas_no_escopo`; a competência listada sozinha não barra empresa sem entrega. `app/ponto/services/fechamento.py:561`. |
| `dia_pago.retratar_exportada` | Testa presença da competência na guarda e entrega da empresa do colaborador; retorna `barrado_pela_guarda=True`. `app/ponto/services/dia_pago.py:457`, `:459`, `:467`. | Sim, via `colaborador_ids=[colab.pk]`. `app/ponto/services/dia_pago.py:460`. |
| `barrados_pela_guarda` | Lê `ACAO_BARRADO_GUARDA`; conta **linhas de LogAuditoria** cujo `descricao` começa por `MM/AAAA `. Não calcula janela, não soma colaboradores e não filtra empresa. `app/ponto/services/fechamento.py:47`, `:50`. | Não. `app/ponto/services/fechamento.py:50`. |
| command `recalcular_fechamento` | Importa guarda e contador; depois do apply imprime o acumulado da competência e `['saida']`. Mesmo com `--empresa`, passa só mês/ano ao contador. `app/ponto/management/commands/recalcular_fechamento.py:101`, `:118`, `:123`, `:126`. | O recalculo/foto podem ser por empresa; o contador impresso não é. `app/ponto/management/commands/recalcular_fechamento.py:60`, `:61`, `:126`. |
| `test_bob_b2_abono_feriado_guarda` | Usa empresa com corte 21; cria entrega 09 e fechamentos 09/10. Exige evento 09 preservado e contador 1; evento 10 regravado e contador 0; leitura 09 permitida; retrato 09 barrado. `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:79`, `:169`, `:175`, `:192`, `:200`, `:206`, `:211`. | Sim no cenário montado; contador segue global. `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:175`, `:198`. |
| `test_reverter_regen_recalcula_dinheiro` | Lê contador antes/depois da reversão de célula em 09 exportada; exige célula revertida, dinheiro preservado e contador aumentado. `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:197`, `:203`, `:205`, `:211`, `:212`. | Cenário de um colaborador/empresa; contador não recebe empresa. `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:205`. |
| `test_justificativa_c9_sem_banco` | Injeta módulo falso com `GUARDA_COMPETENCIA=set()` e ação `'guarda'` para testar retrato sem ata; não caracteriza calendário nem produção. `app/ponto/tests/test_justificativa_c9_sem_banco.py:138`, `:149`, `:153`. | Não exercita entrega real; fornecedor falso retorna conjunto vazio. `app/ponto/tests/test_justificativa_c9_sem_banco.py:151`. |

`ACAO_BARRADO_GUARDA` é usada para escrever o evento do recalculo barrado (`modelo='FechamentoMensal'`) e do retrato barrado (`modelo='DiaPago'`); seu leitor funcional é `barrados_pela_guarda`. `app/ponto/services/fechamento.py:44`, `:50`, `:569`; `app/ponto/services/dia_pago.py:463`.

O contador mede chamadas/eventos, não quantidade de pessoas: uma chamada barrando vários colaboradores gera uma linha, com quantidade no texto e IDs no JSON; o retrato gera outra linha por chamada/colaborador. `app/ponto/services/fechamento.py:569`, `:570`, `:573`; `app/ponto/services/dia_pago.py:463`.

As menções documentais ao nome estão em `app/docs/RELATO.md:1372`, `:1374`, `:1695`, `:2567`, `:4754`, `:4758`; não são leitores executáveis da variável. As ocorrências de `barrado_pela_guarda` em `eval/juiz.py:23`, `:60`, `:227` pertencem a outro veredito, descrito como motivo `numero_fora_dos_dados`. `eval/juiz.py:60`.

Q4 — 10/2026 e inventário dos escritores:

**A guarda desta cópia não impede regravação da 10:** só contém `(9, 2026)`; a aplicação exige simultaneamente chave guardada e empresa exportada. `app/ponto/services/fechamento.py:36`, `:37`, `:560`, `:561`. O teste existente exige explicitamente que evento na 10 regrave. `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:200`.

`status='aprovado'` não impede o escritor de recalculo: aberto recebe `save()`; demais estados recebem `queryset.update()` dos números calculados, mantendo os carimbos de aprovação. `app/ponto/services/fechamento.py:1150`, `:1157`, `:1159`, `:1173`. A proteção do model é apenas no `save()` de linha originalmente aprovada que continua aprovada; não alcança o `update()` do recalculo. `app/ponto/models.py:492`, `:498`, `:503`; `app/ponto/services/fechamento.py:1173`. O contrato AST exige que `recalcular_fechamento_mes` não consulte tranca. `app/ponto/tests/test_contract_juiz_fechamento.py:117`, `:123`, `:130`.

| Escritor funcional | O que grava | Passa na guarda? |
|---|---|---|
| `recalcular_fechamento_mes` | Cria FM, salva/atualiza campos calculados; chama lavratura DP `motor`, e DP `oraculo` desde 10/2026. `app/ponto/services/fechamento.py:903`, `:1157`, `:1173`, `:1214`, `:1278`, `:1290`. | Sim, exclui colaboradores antes do laço. `app/ponto/services/fechamento.py:560`, `:564`. |
| `dia_pago.lavrar` | Apaga e recria todas as linhas do colaborador/competência/versão. `app/ponto/services/dia_pago.py:135`, `:359`, `:360`. | Sem guarda própria; as duas chamadas funcionais ficam no recalculo guardado. `app/ponto/services/fechamento.py:1214`, `:1290`. |
| `dia_pago.retratar_exportada` | Atualiza/cria três campos de grade, ajuste DSR/banco e carimbos de retrato. `app/ponto/services/dia_pago.py:486`, `:495`, `:518`, `:552`, `:556`, `:559`. | Sim, guarda própria antes de ler FM. `app/ponto/services/dia_pago.py:459`, `:469`. |
| `dia_pago.apurar_furo_da_exportada` | Somente `furo_apurado` e `lavrado_em` em linhas já existentes com furo NULL. `app/ponto/services/dia_pago.py:659`, `:664`, `:685`, `:687`. | Não consulta guarda; colhe motor em leitura e escreve por porta própria. `app/ponto/management/commands/apurar_furo_da_exportada.py:111`, `:131`. |
| `aprovar_lote` / `aprovar` | Status, usuário, data e eventualmente observação. `app/ponto/services/fechamento.py:1336`, `:1342`, `:1345`, `:1348`, `:1353`. | Não. `app/ponto/services/fechamento.py:1336`. |
| `reabrir_lote` / `reabrir` | Status aberto e limpeza da aprovação. `app/ponto/services/fechamento.py:1357`, `:1359`, `:1362`, `:1367`. | Não. `app/ponto/services/fechamento.py:1357`. |
| `restaurar_fechamento` | Atualiza campos fornecidos, exige motivo, invalida `derivado_em` por padrão. `app/ponto/services/fechamento.py:1371`, `:1391`, `:1404`, `:1406`. | Não. `app/ponto/services/fechamento.py:1386`. |
| `reabrir_periodo` | Reabre PeriodoFechado e devolve FM aprovado para aberto. `app/ponto/services/fechamento.py:1445`, `:1450`, `:1462`, `:1465`. | Não. `app/ponto/services/fechamento.py:1445`. |
| `invalidar_previsto` | Zera `previsto_em` e atualiza carimbo. `app/ponto/services/fechamento.py:1620`, `:1624`, `:1626`. | Não. `app/ponto/services/fechamento.py:1620`. |
| `invalidar_fechamento` | Zera `derivado_em` e atualiza carimbo nas competências do fato. `app/ponto/services/fechamento.py:1662`, `:1665`, `:1669`. | Não. `app/ponto/services/fechamento.py:1629`. |
| `apagar_em_ensaio` | Delete de FM, exigindo banco de ensaio. `app/ponto/services/fechamento.py:1777`, `:1779`, `:1781`. | Não; tem guarda de banco de ensaio. `app/ponto/services/fechamento.py:1780`. |
| `FechamentoMensal.save` | Escrita ORM, com proteção aprovado→aprovado. `app/ponto/models.py:492`, `:498`, `:503`. | Não consulta guarda de competência. `app/ponto/models.py:492`. |
| `bin/restore_relavratura_10_2026.py` | Salva FM por `save(update_fields)`, apaga FM excedentes, apaga/recria DP motor. `bin/restore_relavratura_10_2026.py:74`, `:81`, `:83`, `:85`, `:86`. | Não passa pelo serviço guardado. `bin/restore_relavratura_10_2026.py:69`. |
| `bin/restore_o263_comp10.py` | Mesmos atos, limitados ao alvo. `bin/restore_o263_comp10.py:43`, `:50`, `:52`, `:54`, `:56`. | Não passa pelo serviço guardado. `bin/restore_o263_comp10.py:40`. |
| `bin/restore_badge_linha_09_10.py` | Apaga DP das competências fotografadas e recria snapshot. `bin/restore_badge_linha_09_10.py:60`, `:61`, `:63`. | Não passa pelo serviço guardado. `bin/restore_badge_linha_09_10.py:59`. |

Chamadores de escrita que chegam ao recalculo, portanto à guarda: botão `app/ponto/views.py:1255`; evento `app/ponto/services/fechamento.py:1823`; command normal `app/ponto/management/commands/recalcular_fechamento.py:118`; REGEN `app/ponto/portas/regen_exportada.py:184`; `app/folha/management/commands/simular_folha.py:123`; commands `aplicar_09_corte_b.py:212`, `plano_b_no_dinheiro.py:110`, `folga_que_sumiu.py:70`, `desvio_o68b.py:94`, `celula_veredito_velho.py:149`, `diff_reclassificar_partido.py:161` e `:206`, todos sob `app/ponto/management/commands/`. Também chegam os scripts `bin/aplicar_o263_comp10.py:51`, `bin/relavrar_o118_col221.py:53`, `bin/relavrar_o119_col221.py:52`, `bin/relavrar_o120_col221_09.py:74`.

`lavrar_dias_pagos` roteia empresa não exportada para `somente_leitura=True, lavrar_dias_pagos=True` e exportada para `retratar_exportada`; ambos caminhos passam por guarda de escrita. `app/ponto/management/commands/lavrar_dias_pagos.py:104`, `:106`, `:123`. O modo leitura puro não consulta exportadas para bloquear e não persiste FM/DP. `app/ponto/services/fechamento.py:539`, `:540`, `:1141`, `:1204`.

**REGEN não contorna a guarda 09 nesta cópia:** chama o recalculo e depois o retrato, e as duas funções guardam 09 exportada; não há parâmetro de bypass nessas chamadas. `app/ponto/portas/regen_exportada.py:184`, `:193`; `app/ponto/services/fechamento.py:560`; `app/ponto/services/dia_pago.py:459`.

`restaurar_fechamento` é chamado diretamente pelos commands de reversão/aplicação sem passar pela guarda do recalculo: `app/ponto/management/commands/aplicar_09_corte_b.py:220`, `:266`; `app/ponto/management/commands/aplicar_09_he_intra.py:149`; `app/ponto/management/commands/desvio_o68b.py:197`; `app/ponto/management/commands/diff_reclassificar_partido.py:224`.

A data da competência não é mês civil: com corte 21, 25/09 pertence a 10; há comentário explícito no restore que conserva `mes/ano` da linha justamente por esse motivo. `app/ponto/janelas.py:11`, `:13`; `bin/restore_badge_linha_09_10.py:44`. O corte real cadastrado de Juliani em produção permanece [nao verificado].

_5561 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
