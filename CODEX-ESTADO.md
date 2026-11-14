# CODEX-ESTADO

> Ultima resposta processada: **14/11/2026 16:01** · publicado em **14/11/2026 16:01** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 14/11 16:01 · sessao 32de8a9570c1 · - · - · origem=Ronald (tmux)

**Achado decisivo:** o `sla.vencido=True` da sonda não prova que venceu o prazo de resposta da pergunta: o SLA de tela mede horas por urgência desde a última nota do DP ou o nascimento; o C9 mede `prazo_sla <= agora`, com opção de derivar nascimento + 20 minutos. Os valores de `prazo_sla` e a associação pergunta→pai não constam da sonda. (`app/chamados/services/sla.py:17–39`; `app/chamados/catalogo/motor.py:54–72`; campos concretos da sonda: [nao verificado])

**Definições distintas e candidatos:**

| Definição | Regra / campos lidos | Escritor / leitor |
|---|---|---|
| SLA operacional de tela | `status_local in COBRANCA` e `referencia + timedelta(hours=sla_limite_horas(urgencia)) < agora`; referência = última nota `dp_rh` ou `criado_em`. Não lê resposta da pergunta nem `prazo_sla`. | Regra: `app/chamados/services/sla.py:17–39`; tabela 1/2/24/48h: `app/chamados/catalogo/motor.py:35–41`; leitores tela `app/chamados/services/painel.py:372–379`, cron `app/chamados/management/commands/alertar_chamados_sla.py:25–46`; timestamps automáticos de chamado/nota: `app/chamados/models.py:67`, `app/chamados/models.py:1721`; notas pela porta `app/chamados/services/nota.py:75–109`; subida de urgência pela porta `app/chamados/models.py:514–545`. |
| Prazo de cobrança C9 | `bool(prazo_sla and prazo_sla <= agora)`; helper retorna prazo explícito, senão `criado_em+20min`. | `app/chamados/catalogo/motor.py:45–72`; registro vivo `app/core/juizes.py:289`; lookup `app/docs/JUIZES.md:59–63`; escritores nascimento `app/chamados/services/disputa_emissao.py:581`, `:1117`, `:1352`; recobrança `:1725`; cobrança em lote `app/chamados/services/cobranca.py:170–178`; gate único `app/chamados/models.py:574–593`; leitores `app/ponto/management/commands/escalonar_chamados_supervisao.py:96`, `app/chamados/services/cobranca.py:255`, `app/chamados/services/fila.py:165–169`. |
| Prazo “da pergunta” | Não há campo prazo na pergunta; a cobrança guarda prazo no **pai da disputa**, compartilhado pelas perguntas. Cobrança em lote usa 48h por padrão ou prazo explícito; recobrança usa 20min. | Campos `app/chamados/models.py:2499–2527`; associação ao pai `app/chamados/services/cobranca.py:51–65`; 48h `app/chamados/services/cobranca.py:38`, `:165–178`; 20min `app/chamados/services/disputa_emissao.py:1692–1725`. |
| Marco previsto vencido | `quando+30min <= agora`; marco do turno encadeado atravessa meia-noite. Não é prazo para responder. | `app/ponto/services/cartorio.py:862`, `:882–893`; `app/escala/servico_jornada.py:69–94`; fábrica `app/chamados/services/fabrica_da_celula.py:35–49`; tarefa lê o teto em `app/chamados/services/tarefa_do_dia.py:82–113`. |
| Lavra vencida / stale | Lavra ausente, posterior a `agora`, janela civil local expirada ou insumo posterior; não é atraso de resposta. | Escritor `app/chamados/veredito_lavrado.py:254–275`; leitor `:213–231`; relógio civil local `:95–116`; tarefa `app/chamados/services/tarefa_do_dia.py:415–418`. |
| Silêncio / arquivo | `silenciado_ate > agora` esconde; vencimento devolve à fila. Constante de arquivo = 30 dias. | `app/chamados/models.py:87–92`, `:509–512`; `app/chamados/catalogo/motor.py:83–84`; fila `app/chamados/services/painel.py:158`. |
| Justificativa DP | Dias civis locais desde nascimento; vence quando `dias > prazo_do_tipo`. Outro domínio. | `app/ponto/catalogo/justificativas.py:21–41`; registro `app/core/juizes.py:311`. |

**Candidata do degrau:** o prazo de cobrança entregue ao C9 `catalogo/motor.py::prazo_estourou`, já registrado para a pergunta “o prazo (SLA) do chamado estourou?”. Usar `sla.vencido` da sonda substituiria essa pergunta por horas de ausência de resposta do DP; usar marco vencido substituiria por horário de batida. A escolha de qual pai/prazo corresponde à pergunta concreta precisa respeitar a associação existente, pois cobrança em lote grava nos pais compartilhados. (`app/core/juizes.py:115`, `:289`; `app/docs/JUIZES.md:63`; `app/chamados/services/sla.py:17–39`; `app/chamados/services/cobranca.py:51–65`)

**Relógio:** a configuração é `America/Sao_Paulo`, `USE_TZ=True`; SLA de tela e C9 comparam instantes, com limites distintos (`<` versus `<=`), horas versus prazo explícito/fallback em minutos; marcos têm instante aware encadeado e teto de 30min; a lavra e a justificativa usam dia civil local; identidade da pergunta da tarefa usa **dia do turno** `data_evento`. Portanto não são o mesmo relógio semântico nem a mesma unidade. (`app/config/settings/base.py:93–95`; `app/chamados/services/sla.py:30–39`; `app/chamados/catalogo/motor.py:45–72`; `app/escala/servico_jornada.py:89–93`; `app/ponto/services/cartorio.py:892`; `app/chamados/veredito_lavrado.py:104–116`; `app/ponto/catalogo/justificativas.py:34–41`; `app/chamados/services/tarefa_do_dia.py:227–230`)

**Sem resposta não deve ser reconstruído apenas de `respondida_em`:** a tarefa usa última `DecisaoPergunta` até `agora`: ausência de decisão, resposta vazia, recusa e reabertura produzem responder/colaborador; resposta com texto produz validar/admin. O carregador ordena por `ato_em,pk`, corta o horizonte e reconhece projeção sem linha como passivo. (`app/chamados/services/tarefa_do_dia.py:126–150`, `:400–413`)

**Escritores de resposta:** o core de resposta grava texto e `respondida_em` no mesmo instante da linha de decisão; resposta nova invalida validação anterior; reabertura zera resposta/carimbo por porta e registra o ato; resposta automática e recuperação pela trilha também registram decisões. (`app/chamados/services/disputa_emissao.py:1460–1485`, `:1562–1608`, `:1640–1664`, `:1668–1685`)

**Aviso e escalonamento:**

- `ja_avisado` é derivado, não campo boolean: nota de sistema cujo conteúdo contém `'SLA'`. O cron registra a nota via `anexar_ocorrencia`; passivo anterior ao corte e teto por passada podem deixar chamado vencido sem aviso. (`app/chamados/services/sla.py:42–45`; `app/chamados/management/commands/alertar_chamados_sla.py:32–46`; `app/chamados/services/nota.py:75–109`)
- `escalonado_para_dp` é recibo de push do N1: o cron compara C9, envia push e chama `marcar_escalonado_dp`; o gate escreve boolean, instante e trilha, com guarda idempotente. (`app/ponto/management/commands/escalonar_chamados_supervisao.py:93–107`; `app/chamados/models.py:595–616`)
- O escalonamento N2 pode alterar urgência para emergência, afetando o SLA operacional baseado em urgência; isso é efeito indireto no prazo operacional, não autorização para usar flags de aviso como dono do dia. (`app/ponto/management/commands/escalonar_chamados_supervisao.py:109–138`; `app/chamados/services/sla.py:26–39`)
- A tarefa não recebe aviso/escalonamento como critério no julgamento da última decisão; nesse julgamento o lado resulta do ato da pergunta. (`app/chamados/services/tarefa_do_dia.py:126–150`)
- Na sonda, as 53 linhas da classe A provam o agrupamento pelo SLA operacional informado, mas não permitem afirmar quantas satisfariam o C9 no prazo associado à pergunta; faltam prazo explícito, nascimento, última nota DP e associação pergunta→pai. [nao verificado]

## 14/11 15:39 · sessao 3eb93b59aecc · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

Corrigi o lado **TESTE** nos três casos: [nao verificado]

- **(a)** I agora encerra pela porta real, sem pergunta ancorada; depois nasce P e o reaponte para G continua exigido. Nenhuma guarda é substituída. [ORM:181](/home/ronald/wt-codex-bo-disputa-sem-pergunta-152912/app/chamados/tests/test_bo_disputa_c22_orm.py:181)
- **(b)** A fixture antiga perdia elegibilidade e retornava `None` antes de criar disputa. Agora invalida somente a data do marco, preserva tipo/hora e exige disputa produzida e fechada. [ORM:300](/home/ronald/wt-codex-bo-disputa-sem-pergunta-152912/app/chamados/tests/test_bo_disputa_c22_orm.py:300)
- **(c)** Dona/local empatavam no relógio congelado. Agora a local é efetivamente posterior e o teste verifica o pedido G na porta real, mantendo **uma trilha**. A seleção efetiva na execução anexada é [nao verificado]. [ORM:116](/home/ronald/wt-codex-bo-disputa-sem-pergunta-152912/app/chamados/tests/test_bo_disputa_c22_orm.py:116) [nao verificado]

A guarda grava `valor_depois={'chamado_destino_atual': I.pk, 'chamado_destino_pedido': G.pk}`; `models._nascer` evita duplicar essa chave. [fio:790](/home/ronald/wt-codex-bo-disputa-sem-pergunta-152912/app/chamados/services/fio.py:790), [models:2990](/home/ronald/wt-codex-bo-disputa-sem-pergunta-152912/app/chamados/models.py:2990)

Foram tocados quatro arquivos: teste ORM, suplemento em memória, mutações e entrega. Motivos, três hipóteses e comandos individuais estão no [inventário](/home/ronald/wt-codex-bo-disputa-sem-pergunta-152912/app/docs/BO_DISPUTA_C23_ENTREGA.txt:5). [nao verificado]

Em memória: **8 testes verdes**, RED da fixture base com **1 failure/0 errors**, nove mutantes mortos por assertion e seis contrastes anteriores verdes. [Resultados e comandos](/home/ronald/wt-codex-bo-disputa-sem-pergunta-152912/app/docs/BO_DISPUTA_C23_ENTREGA.txt:60) [nao verificado]

Certificação ORM: **[nao verificado]**, reservada à sessão. Ruff indisponível (`No module named ruff`); lint limpo: **[nao verificado]**. [Limites](/home/ronald/wt-codex-bo-disputa-sem-pergunta-152912/app/docs/BO_DISPUTA_C23_ENTREGA.txt:87)

## 14/11 15:20 · sessao 5573ac6b03cc · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

Implementação, inventário dos 13 arquivos, censo e comandos estão na [entrega c22](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/docs/BO_DISPUTA_C22_ENTREGA.txt:14).

- Verificação local: 83 testes legados, 7 suplementares e 35 da catraca passaram; 19 mutantes e o registro antigo morreram por assertion. [Resultados](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/docs/BO_DISPUTA_C22_ENTREGA.txt:103). [nao verificado]
- RED/GREEN ORM, incluindo P3 vermelho: **[não verificado]**. P3 foi entregue sem `expectedFailure`; o hunk protegido continua pendente da sessão. [Comando](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/docs/BO_DISPUTA_C22_ENTREGA.txt:108).
- Ruff limpo: **[não verificado]**, ferramenta ausente. [Registro](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/docs/BO_DISPUTA_C22_ENTREGA.txt:11).

Ressalva explícita: `[G]` normalizado pede I e espera zero recusas; o teste da guarda força o pedido G na entrada e exige uma trilha em duas passadas. [Detalhe](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/docs/BO_DISPUTA_C22_ENTREGA.txt:37). [nao verificado]

## 14/11 15:11 · sessao fbfa3d68b664 · E3-V10 · leitura · origem=Code

**A árvore não preserva o main por leitor:** Folha muda o fio nos quatro estados; a API conserva o fio, mas muda porta/fila/verbos em P19; o vigia troca sua própria projeção de papel e admin. (`M:app/folha/fora_da_folha.py:65–78`; `M:app/chamados/services/tarefa_do_dia.py:545–554`; `M:app/chamados/services/vigia_fio.py:648–651`; `E:app/folha/fora_da_folha.py:74–89`; `E:app/chamados/services/tarefa_do_dia.py:570–598`; `E:app/chamados/services/vigia_fio.py:661–663`)

Uso **M** para linhas do main reconstruídas do lado antigo do patch fornecido e **E** para linhas da cópia. Não atribuo ao main arquivos ausentes do patch. Identidade integral desses arquivos entre main e cópia: `[nao verificado]`.

**Valores das portas**

Notação: `PF=porta_fila`, `PP=porta_pergunta`; cada saída abaixo é **fio / rota / fila / verbos**. Essas são as chaves devolvidas pelas duas implementações. (`M:app/chamados/juizes.py:1667–1668`; `M:app/chamados/services/tarefa_do_dia.py:553–554`; `E:app/chamados/services/tarefa_do_dia.py:597–598`)

Para P13/P14, o dado admite porta na própria célula ou âncora exclusivamente externa. Portanto, não há uma localização única demonstrada: na primeira, rota/fila são `PF/(A,)`; na segunda, `PP/()`. O mapa conserva o verbo informado. Localização concreta em produção: `[nao verificado]`. (`M:app/chamados/juizes.py:1657–1668`; `M:app/chamados/services/tarefa_do_dia.py:545–554`; `E:app/chamados/services/tarefa_do_dia.py:571–598`)

| Estado | C17 main | API tarefa main | Tarefa na árvore | Evidência M / E |
|---|---|---|---|---|
| P13: responder; painel validar | **admin** / PF ou PP / `(A,)` ou `()` / `{A:validar}` | **colab** / mesma geometria / `{A:validar}` | **colab** / mesma geometria / `{A:validar}` | `M:app/chamados/juizes.py:1608–1619,1657–1668`; `M:app/chamados/services/tarefa_do_dia.py:545–554`; `E:app/chamados/services/tarefa_do_dia.py:571–598` |
| P14: validar; painel cobrar | **colab** / PF ou PP / `(A,)` ou `()` / `{A:cobrar}` | **admin** / mesma geometria / `{A:cobrar}` | **admin** / mesma geometria / `{A:cobrar}` | Mesmos trechos da linha anterior |
| P19: A residual da célula decidir; ação admin sob B externo cobrar | **colab / PF / `(A,)` / `{A:decidir,B:cobrar}`** | **admin / PP / `()` / `{B:cobrar}`** | **admin / PF / `(A,)` / `{A:decidir,B:cobrar}`** | `M:app/chamados/juizes.py:1657–1668`; `M:app/chamados/services/tarefa_do_dia.py:545–554`; `E:app/chamados/services/tarefa_do_dia.py:571–598` |
| Simultâneo do anexo: colab+admin; único verbo validar | **admin** / geometria da porta / `{A:validar}` | **colab** / mesma geometria / `{A:validar}` | **colab** / mesma geometria / `{A:validar}` | `M:app/chamados/juizes.py:1608–1619,1657–1668`; `M:app/chamados/services/tarefa_do_dia.py:545–554`; `E:app/chamados/tests/test_e3_portas_tarefa.py:507–515` |

O C17 continua na árvore com a escada `cobrar→colab`, `validar→admin`, demais→chamado. Sua resposta própria foi preservada; seus antigos leitores passaram a receber outra resposta. (`M:app/chamados/juizes.py:1608–1619,1653–1668`; `E:app/chamados/juizes.py:1650–1661,1695–1710`; `E:app/core/juizes.py:132–138`)

**Comparação por leitor**

As referências “C17”, “tarefa main” e “tarefa árvore” nesta tabela remetem às saídas completas e às linhas da tabela anterior.

| Leitor × estado | Main | Árvore | IGUAL/DIFERE |
|---|---|---|---|
| Folha/`fora_da_folha` — P13 | C17: **admin** | tarefa: **colab**; porta/fila/verbos conservados | **DIFERE fio** |
| Folha/`fora_da_folha` — P14 | C17: **colab** | tarefa: **admin**; porta/fila/verbos conservados | **DIFERE fio** |
| Folha/`fora_da_folha` — P19 | C17: **colab**, PF, `(A,)`, `{A:decidir,B:cobrar}` | **admin**, PF, `(A,)`, mesmo mapa | **DIFERE fio** |
| Folha/`fora_da_folha` — simultâneo | C17: **admin** | tarefa: **colab**; porta/fila/verbos conservados | **DIFERE fio** |
| API `tarefa_do_dia` — P13 | tarefa main: colab | colab, mesma porta/fila/verbos | **IGUAL** nesses campos |
| API `tarefa_do_dia` — P14 | tarefa main: admin | admin, mesma porta/fila/verbos | **IGUAL** nesses campos |
| API `tarefa_do_dia` — P19 | admin, **PP, `()`, `{B:cobrar}`** | admin, **PF, `(A,)`, `{A:decidir,B:cobrar}`** | **DIFERE porta/fila/verbos** |
| API `tarefa_do_dia` — simultâneo | tarefa main: colab | colab, mesma porta/fila/verbos | **IGUAL** nesses campos |

A Folha lê o fio da porta e `o_que_segura` também publica os chamados de `fila`; não publica o mapa completo de verbos. Fontes da comparação Folha: `M:app/folha/fora_da_folha.py:65–78,225–235`; `E:app/folha/fora_da_folha.py:71–89,241–252`. Fontes da comparação API: `M:app/chamados/services/tarefa_do_dia.py:545–554`; `E:app/chamados/services/tarefa_do_dia.py:570–598`.

O vigia precisa de tabela própria: **seu papel no main era a lista de verbos, não o fio C17**. (`M:app/chamados/services/vigia_fio.py:648–651`; `E:app/chamados/services/vigia_fio.py:661–663`)

| Vigia × estado | Main: papel; incremento admin | Árvore: papel; incremento admin | IGUAL/DIFERE |
|---|---|---|---|
| P13 | `validar`; `bool({'validar'} ∩ VERBOS_DO_ADMIN_main)` | `colab`; **0** | **DIFERE papel**; número main `[nao verificado]` |
| P14 | `cobrar`; `bool({'cobrar'} ∩ VERBOS_DO_ADMIN_main)` | `admin`; **1** | **DIFERE papel**; número main `[nao verificado]` |
| P19 | `cobrar,decidir`; `bool({'cobrar','decidir'} ∩ VERBOS_DO_ADMIN_main)` | `admin`; **1** | **DIFERE papel**; número main `[nao verificado]` |
| Simultâneo | `validar`; `bool({'validar'} ∩ VERBOS_DO_ADMIN_main)` | `colab`; **0** | **DIFERE papel**; número main `[nao verificado]` |

Fontes: `M:app/chamados/services/vigia_fio.py:648–651`; `E:app/chamados/services/vigia_fio.py:661–663`. A rota recebida acompanha C17 no main e tarefa na árvore, conforme a primeira tabela; em P19 ambos recebem PF. (`M:app/chamados/services/vigia_fio.py:640,649`; `E:app/chamados/services/vigia_fio.py:648,661`)

**Corrijo o limite da atualização anterior:** só está comprovado na cópia que `VERBOS_DO_ADMIN=('validar','decidir')`. O arquivo correspondente do main não veio no patch; portanto, os números main **1/0/1/1** e a igualdade numérica de P19 ficam `[nao verificado]`. (`E:app/chamados/services/verbos.py:22`)

| Demais leitores × estado | Main | Árvore | IGUAL/DIFERE |
|---|---|---|---|
| `furo_sem_canal` — P13 | Não devolve fio/porta/verbos/fila; cobertura pelos registros crus da célula | Não devolve esses campos; havendo a porta descrita, exclui a célula de `sem_canal` | **DIFERE formato**; igualdade da cobertura `[nao verificado]` |
| `furo_sem_canal` — P14 | Mesma regra crua; carimbos/FKs concretos `[nao verificado]` | Havendo a porta descrita, exclui de `sem_canal` | **DIFERE formato**; igualdade da cobertura `[nao verificado]` |
| `furo_sem_canal` — P19 | A vivo da própria célula fornece cobertura | A porta fornece cobertura | **IGUAL cobertura da célula; DIFERE formato** |
| `furo_sem_canal` — simultâneo | Mesma regra crua; carimbos/FKs concretos `[nao verificado]` | Havendo a porta descrita, exclui de `sem_canal` | **DIFERE formato**; igualdade da cobertura `[nao verificado]` |

O main testa **pergunta da célula com `respondida_em IS NULL` OU chamado da célula em VIVOS**. A árvore testa presença da porta. O main publica `com_pergunta_aberta/com_chamado_vivo`; a árvore publica `com_porta_fila/com_porta_pergunta/nao_sei`. A entrada no universo também depende de janela/veredito; totais de produção `[nao verificado]`. (`M:app/chamados/services/furo_sem_canal.py:80–86,95–96`; `E:app/chamados/services/furo_sem_canal.py:75–87,97–116`)

| Leitor | P13 | P14 | P19 | Simultâneo | Evidência e limite |
|---|---|---|---|---|---|
| Painel | Mapa dado: `{A:validar}` | `{A:cobrar}` | `{A:decidir,B:cobrar}` | `{A:validar}` | Na árvore, fila e verbos vêm de `qs_na_fila/particionar_verbos`; não há fio/rota de dia nesse contrato. **IGUAL/DIFERE do corpo main `[nao verificado]`**: falta seu arquivo main. (`E:app/chamados/services/painel.py:188–190,212–232`) |
| Pauta `dias_sem_fio` | Árvore não inclui como sem fio | Mesmo resultado | Mesmo resultado | Mesmo resultado | Só compara fio com `FIO_SEM_PORTA`; todos os fios acima são diferentes de nada. **Main `[nao verificado]`**, arquivo ausente. (`E:app/ponto/services/pautas_dos_discordantes.py:79–84`) |
| Beco `cadastro_realidade` | Árvore não inclui como sem porta | Mesmo resultado | Mesmo resultado | Mesmo resultado | Só observa existência da porta nesse ramo. **Main `[nao verificado]`**, arquivo ausente. (`E:app/escala/services/cadastro_realidade.py:464–471`) |
| Retrato/tela da Folha | Valor agregado `[nao verificado]` | Idem | Idem | Idem | Recebe os grupos da fonte; outros dias podem determinar o grupo da pessoa. Foto efetivamente lavrada não foi fornecida. (`E:app/folha/fora_da_folha.py:92–103,197–215`; `E:app/folha/retrato_folha.py:1158–1190`) |

**Quem escreve e quem lê**

“Escrever” aqui distingue produzir o resultado em memória de persistir uma foto. A tarefa produz saída pura; a persistência do retrato ocorre no command próprio. (`E:app/chamados/services/tarefa_do_dia.py:597–618`; `E:app/folha/management/commands/lavrar_retrato_folha.py:69–78`)

| Informação | Quem produz | Quem lê nos pontos relevantes |
|---|---|---|
| Verbo do chamado | `verbo_do_chamado`; `particionar_verbos` escreve `mapa[ch_id]` | Painel e `alimentar_fila`. (`E:app/chamados/catalogo/natureza.py:41–75`; `E:app/chamados/services/verbos.py:47–52`; `E:app/chamados/services/painel.py:229–232`; `E:app/chamados/juizes.py:1570`) |
| Pertença à fila | `qs_na_fila`; `alimentar_fila` monta `fila_ids/fila_cel/fila_dia` | C17 e tarefa. (`E:app/chamados/services/painel.py:188–190`; `E:app/chamados/juizes.py:1573–1589,1699–1708`; `E:app/chamados/services/tarefa_do_dia.py:571–584`) |
| Porta/fila/verbos/fio C17 | `porta_alcancavel_do_dia`, com `fio_pelos_verbos` | Na árvore, lado antigo da sombra. (`E:app/chamados/juizes.py:1655–1661,1699–1710`; `E:app/chamados/management/commands/diff_e3_sombra.py:127`) |
| Porta/fila/verbos/fio da tarefa | `_portas_da_tarefa`; `porta_da_tarefa` apenas projeta | API, Folha, vigia, furo, beco. (`E:app/chamados/services/tarefa_do_dia.py:570–607`; `E:app/folha/fora_da_folha.py:74–89`; `E:app/chamados/services/vigia_fio.py:648–663`; `E:app/chamados/services/furo_sem_canal.py:97–115`; `E:app/escala/services/cadastro_realidade.py:469`) |
| Papel/admin do vigia | Main: verbos/interseção; árvore: fio/igualdade | Contadores e linhas de alcance. (`M:app/chamados/services/vigia_fio.py:648–651`; `E:app/chamados/services/vigia_fio.py:635–663`) |

**Menor ponto de cura de cada diferença**

| DIFERE | Menor retorno ao contrato do mesmo leitor no main | Restrição |
|---|---|---|
| Folha — quatro estados | Voltar sua porta à autoridade **C17 existente**, no limite `_porta/_fio_do_dia`; o leitor continua apenas lendo fio/fila | Não mudar globalmente o fio da tarefa: isso quebraria a API main em P13/P14/simultâneo. (`M:app/folha/fora_da_folha.py:65–78`; `M:app/chamados/services/tarefa_do_dia.py:550–554`; `E:app/folha/fora_da_folha.py:71–89`) |
| API — P19 | Em `_portas_da_tarefa`, restaurar a projeção original **selecionada pelas ações**, antes da união residual | O trecho que acrescenta A é a união com `da_celula`; dele saem a mudança de rota, fila e mapa. (`M:app/chamados/services/tarefa_do_dia.py:545–554`; `E:app/chamados/services/tarefa_do_dia.py:571–584,597–598`) |
| Vigia — quatro papéis | Restaurar a projeção main: papel é a lista ordenada de verbos | Trocar apenas o fio não reproduz o papel textual main. (`M:app/chamados/services/vigia_fio.py:648–651`; `E:app/chamados/services/vigia_fio.py:662–663`) |
| Vigia — regra administrativa | Restaurar a pergunta main: **existe verbo administrativo?** | `fio == admin` responde outra pergunta. A regra main deve continuar usando sua autoridade `VERBOS_DO_ADMIN`; seu conteúdo main falta. (`M:app/chamados/services/vigia_fio.py:650`; `E:app/chamados/services/vigia_fio.py:662`; `E:app/chamados/services/verbos.py:22`) |
| Furo — formato/cobertura | Restaurar o retorno e o teste main no próprio `medir` | Esse teste cru era **regra própria do leitor**. Restaurá-lo literalmente mantém um derivador paralelo à porta da tarefa; o dado não demonstra equivalência que permita substituí-lo sem mudar comportamento. (`M:app/chamados/services/furo_sem_canal.py:80–96`; `E:app/chamados/services/furo_sem_canal.py:97–116`) | [nao verificado]

**Não existe um único valor de `fio` que reproduza simultaneamente Folha-main e API-main nesses estados.** O registro vivo já distingue as perguntas: C17 responde alcançabilidade com verbo do painel; C19 responde ações exigíveis por instância. Preservar essas autoridades nas respectivas perguntas evita inventar regra no leitor. Unificá-las sob uma única resposta exige mudar pelo menos um contrato main. (`E:app/core/juizes.py:138,153–159`; `M:app/chamados/juizes.py:1608–1619`; `M:app/chamados/services/tarefa_do_dia.py:550–554`)

No vigia, a projeção antiga também era regra do leitor. Para ele apenas ler respostas prontas, seria necessário **mover essa mesma projeção para uma autoridade existente**, conservando a pergunta “algum verbo exige admin”; deduzi-la novamente do fio não serve. Já no furo, uma cura sem sua regra própria ou sem transferir integralmente essa regra não está demonstrada. (`M:app/chamados/services/vigia_fio.py:648–651`; `M:app/chamados/services/furo_sem_canal.py:80–86`; `E:app/core/juizes.py:138,159`)

**O que os testes fixam** [nao verificado]

| Teste | Espera na árvore | Relação com o main | [nao verificado]
|---|---|---|
| `test_e3_portas_v6_sem_banco.py:16–20` | P13: responder + validar → **colab** | Preserva tarefa-main; fixa divergência da Folha/C17-main, que dá admin. (`M:app/chamados/juizes.py:1608–1619`; `M:app/chamados/services/tarefa_do_dia.py:550–554`) |
| Mesmo arquivo `:22–26` | P14: validar + cobrar → **admin** | Preserva tarefa-main; contraria Folha/C17-main colab. Mesmas fontes main acima. |
| Mesmo arquivo `:28–42` | Ação admin/cobrar + residual decidir → **admin** | Fixa conflito de fio com C17. **Não fixa a geometria exata de P19:** põe ambos os chamados na célula e espera fila `(101,202)`. (`E:app/chamados/tests/test_e3_portas_v6_sem_banco.py:38–42`) |
| `test_e3_portas_tarefa.py:85–97` | Variante P13: A residual decidir, B externo validar, ação responder → colab | Espera tarefa, enquanto C17 dá admin para decidir+validar. (`M:app/chamados/juizes.py:1608–1619`) |
| `test_v8_R2_hipotese_acao_admin_isolada`, `test_e3_portas_tarefa.py:496–501` | Admin isolado → admin | **Não cobre simultâneo.** (`E:app/chamados/tests/test_e3_portas_tarefa.py:497–501`) |
| `test_v8_R2_validacao_e_resposta_alcancaveis_no_mesmo_dia`, mesmo arquivo `:503–515` | Simultâneo → **colab** | Mesma porta, só validar: contraria C17 admin. Portas distintas, validar+cobrar: C17 também colab; essa variante não demonstra divergência de fio. (`M:app/chamados/juizes.py:1608–1619`; `E:app/chamados/tests/test_e3_portas_tarefa.py:507–514`) |
| `test_v8_R2_alcance_admin_com_pergunta_colab`, mesmo arquivo `:203–225` | Fixture ORM consulta crédito/fila e espera validar/colab | Fixa a expectativa E3/tarefa-main; execução ORM nesta auditoria `[nao verificado]`. (`E:app/chamados/tests/test_e3_portas_tarefa.py:203–225`) |
| `test_e3_diff_sombra.py:157–164` | Validar com fio colab → `exige_admin=False` | Fixa a nova projeção do vigia; não testa sua expressão main por interseção. (`M:app/chamados/services/vigia_fio.py:648–651`) |
| `test_e3_pendentes_app.py:145–159` | Só a sombra pode chamar C17; acusa Folha voltando a ele | A catraca impede o retorno direto à fonte main; não prova equivalência das respostas. (`E:app/chamados/tests/test_e3_pendentes_app.py:145–159`) |

Os testes v6 usam corpos reais extraídos por AST com fornecedores em memória; não substituem ORM. O executor puro remove a classe ORM. Foram executados 10 testes v6 e os dois R2 puros, todos passaram; resultado de terminal sem arquivo persistido: `[nao verificado]`. (`E:app/chamados/tests/test_e3_portas_v6_sem_banco.py:1–4`; `E:app/chamados/tests/e3_portas_apoio.py:18–26`; `E:app/chamados/tests/e3_portas_regra.py:22–29`) [nao verificado]

Para fechar os pontos main ainda não verificáveis, faltam **`app/chamados/services/verbos.py`**, **`app/chamados/services/painel.py`**, **`app/ponto/services/pautas_dos_discordantes.py`** e **`app/escala/services/cadastro_realidade.py`** do main. Eles não estão nos hunks fornecidos; suas respostas integrais no main permanecem `[nao verificado]`.

## 14/11 15:09 · sessao bba7f77d5034 · - · - · origem=Ronald (tmux)

Valores reconstituídos do patch, sem executar ORM:

| Estado canônico do anexo | C17 main | tarefa main | tarefa árvore |
|---|---|---|---|
| P13 responder + validar | admin | colab | colab |
| P14 validar + cobrar | colab | admin | admin |
| P19 A residual decidir + ação admin sob B cobrar | colab; PF; fila `(A,)`; verbos `{A:decidir,B:cobrar}` | admin; PP; fila `()`; verbos `{B:cobrar}` | admin; PF; fila `(A,)`; verbos `{A:decidir,B:cobrar}` |
| Simultâneo, uma porta com validar | admin | colab | colab |

Fontes: C17 main `app/chamados/juizes.py:1608–1619,1653–1668`, reconstruído do patch; árvore `app/chamados/juizes.py:1650–1661,1695–1710`. Tarefa main `app/chamados/services/tarefa_do_dia.py:538–554`, reconstruído do patch; árvore `app/chamados/services/tarefa_do_dia.py:553–598`.

Para P13/P14, porta da própria célula produz PF/fila `(A,)`; âncora exclusivamente externa produz PP/fila `()`, preservando o mapa do verbo. Não existe uma localização única da âncora nos nomes P13/P14 fornecidos; o anexo admite explicitamente ambas. Fonte: C17 main `app/chamados/juizes.py:1657–1668`; tarefa main `app/chamados/services/tarefa_do_dia.py:545–554`; árvore `app/chamados/services/tarefa_do_dia.py:571–598`.

Vigia, por dia com porta:

| Estado | papel main → árvore | exige_admin main → árvore |
|---|---|---|
| P13 | validar → colab | 1 → 0 |
| P14 | cobrar → admin | 0 → 1 |
| P19 | cobrar,decidir → admin | **1 → 1** |
| Simultâneo uma porta validar | validar → colab | 1 → 0 |

O main usa interseção com `VERBOS_DO_ADMIN`, que inclui **decidir**; a árvore usa igualdade do fio com admin. Fontes: patch main `app/chamados/services/vigia_fio.py:648–651`; árvore `app/chamados/services/vigia_fio.py:657–663`; `app/chamados/services/verbos.py:22`. Portanto não classificar todo o contador P19 como DIFERE: o papel difere, o incremento administrativo coincide.

Há outra variante de simultâneo: portas distintas `{101:validar,202:cobrar}`. Nela, C17 e tarefa retornam colab; o contador administrativo ainda muda de 1 para 0. O teste percorre ambas as variantes: `app/chamados/tests/test_e3_portas_tarefa.py:503–515`. A descrição do anexo corresponde à variante com um único validar.

`furo_sem_canal` main **não devolve fio/rota/verbos/fila**. Decide cobertura por pergunta da própria célula com `respondida_em IS NULL` OU chamado da própria célula em VIVOS: main reconstruído `app/chamados/services/furo_sem_canal.py:80–86`; suas métricas antigas estão em `:95–96`. A árvore decide pela existência de porta e publica contagens por rota: `app/chamados/services/furo_sem_canal.py:89–116`. Cobertura main de P14 com âncora exclusivamente externa não decorre do par ação/verbo: os demais registros da célula são `[nao verificado]`. O ingresso no universo também exige veredito e janela: `app/chamados/services/furo_sem_canal.py:75–87`.

Os comentários que chamam a preferência colab da tarefa de “main” preservam **a tarefa main**, mas não preservam a saída do C17 main para os leitores anteriormente ligados a ele: `app/chamados/services/tarefa_do_dia.py:589–598`; `app/chamados/tests/test_e3_portas_tarefa.py:221–225,503–515`; C17 main `app/chamados/juizes.py:1608–1619`.

## 14/11 15:09 · sessao df8530d7400e · - · - · origem=Ronald (tmux)

Mapa dos consumidores e escritores da árvore:

| Campo | Quem escreve o resultado | Quem lê |
|---|---|---|
| Verbo por chamado | `particionar_verbos`: chama `verbo_do_chamado` e monta mapa (`app/chamados/services/verbos.py:47-52`); natureza escolhe validar/cobrar/decidir (`app/chamados/catalogo/natureza.py:41-75`). | Painel (`app/chamados/services/painel.py:229-232,383`), alimentação comum (`app/chamados/juizes.py:1570`). |
| Pertença à fila | `qs_na_fila` seleciona vivos em COBRANCA e exclui silêncio vigente (`app/chamados/services/painel.py:188-190`); `alimentar_fila` monta `fila_ids`, `fila_cel`, `fila_dia` (`app/chamados/juizes.py:1573-1589`). | C17 (`app/chamados/juizes.py:1699-1708`) e tarefa (`app/chamados/services/tarefa_do_dia.py:571-584`). |
| C17: porta/fila/verbos/fio | `porta_alcancavel_do_dia`, fio pela escada `fio_pelos_verbos` (`app/chamados/juizes.py:1650-1661,1699-1710`). | Sombra, lado atual (`app/chamados/management/commands/diff_e3_sombra.py:127`). |
| Tarefa: porta/fila/verbos/fio | `_portas_da_tarefa` reúne portas, lê verbos, combina donos das ações com residuais e escreve `out` (`app/chamados/services/tarefa_do_dia.py:570-598`). | `porta_da_tarefa` apenas projeta (`app/chamados/services/tarefa_do_dia.py:607`); Folha, vigia, furo e beco leem essa projeção (`app/folha/fora_da_folha.py:74,89`; `app/chamados/services/vigia_fio.py:648,661-663`; `app/chamados/services/furo_sem_canal.py:97-115`; `app/escala/services/cadastro_realidade.py:469`). |

**Vigia merece comparação própria:** no main, segundo o patch fornecido, papel é a lista de verbos e `exige_admin` testa interseção com `VERBOS_DO_ADMIN`; na árvore, papel é fio e admin significa `fio == 'admin'` (`diff_leitores.patch`, hunk antigo `vigia_fio.py:650-653`; árvore `app/chamados/services/vigia_fio.py:661-663`). A constante atual inclui **validar e decidir** (`app/chamados/services/verbos.py:22`); sua identidade com main fora do patch é `[nao verificado]`.

| Estado | Vigia main: papel / admin | Árvore: papel / admin |
|---|---|---|
| P13, verbo validar | `validar / 1` | `colab / 0` |
| P14, verbo cobrar | `cobrar / 0` | `admin / 1` |
| P19, verbos decidir+cobrar | `cobrar,decidir / 1` | `admin / 1` |
| Simultâneo, verbo validar | `validar / 1` | `colab / 0` |

Evidências dos valores E3: `app/chamados/tests/test_e3_portas_v6_sem_banco.py:20-41`; simultâneo `app/chamados/tests/test_e3_portas_tarefa.py:225`. Evidências da projeção vigia: patch antigo acima e `app/chamados/services/vigia_fio.py:661-663`. **Devolver fio C17 ao vigia sem restaurar sua projeção antiga ainda erra P19:** C17 colab não implica `exige_admin=False`, porque o main conta decidir (`app/chamados/juizes.py:1650-1661`; `app/chamados/services/verbos.py:22`).

Outros leitores:

- **Pauta `dias_sem_fio`:** só compara fio com `FIO_SEM_PORTA`; não publica fio/porta/verbos/fila. Trocar colab por admin não altera sua inclusão nesses estados com porta (`app/ponto/services/pautas_dos_discordantes.py:79-84`).
- **Beco:** só observa presença de porta; não lê fio/rota/fila/verbos (`app/escala/services/cadastro_realidade.py:464-471`).
- **Furo:** main não devolve fio/porta/verbos/fila; o patch usa pergunta com carimbo cru nulo ou chamado vivo com FK da célula. Não deduzir canal main de C17: uma âncora externa não satisfaz o segundo teste; ação responder/validar não basta para provar `respondida_em` cru. Resultado main por fixture exige conferir seus carimbos/FKs; produção `[nao verificado]`. A árvore exclui do sem_canal qualquer célula com porta e conta rota PF/PP (`app/chamados/services/furo_sem_canal.py:97-115`).
- **Retrato Folha:** lê os grupos da fonte e monta `dono` (`app/folha/retrato_folha.py:1158-1190`); command existente persiste (`app/folha/management/commands/lavrar_retrato_folha.py:69,78`). Divergência de um dia pode propagar, mas o grupo agregado depende dos outros dias (`app/folha/fora_da_folha.py:92-103,197-215`); valor final por pessoa `[nao verificado]`.

Pontos de cura sem derivador local:

- **Folha:** delegar novamente ao C17 existente preserva sua resposta anterior; alterar globalmente fio da tarefa para coincidir com C17 muda a resposta da API cujo main já divergia, conforme o anexo fornecido (`diff_leitores.patch`, lados antigos `folha/fora_da_folha.py:65-78` e `tarefa_do_dia.py:545-554`).
- **Vigia:** restaurar a autoridade anterior **e a projeção anterior** de papel/admin; não usar fio como substituto da pergunta “algum verbo exige admin” (`diff_leitores.patch`, antigo `vigia_fio.py:643,650-653`; árvore `app/chamados/services/vigia_fio.py:648,662-663`).
- **API P19:** o menor lugar comum é `_portas_da_tarefa`, onde a união residual amplia a saída original escolhida apenas pelas ações; não corrigir no consumidor (`app/chamados/services/tarefa_do_dia.py:571-584`; main no patch `tarefa_do_dia.py:545-554`).
- Uma saída única idêntica para Folha e API não pode reproduzir dois valores diferentes do main para o mesmo estado; preservar os dois exige manter as perguntas/projeções distintas das autoridades existentes, ou declarar a incompatibilidade com a exigência de unificação. Dedução a partir dos dois corpos fornecidos no patch; a solução arquitetural aprovada `[nao verificado]`.

Para fechar referências **do main** dos leitores adicionais, faltam os corpos de `app/ponto/services/pautas_dos_discordantes.py` e `app/escala/services/cadastro_realidade.py`; o patch fornecido não os inclui. Seus comentários atuais documentam a migração (`app/ponto/services/pautas_dos_discordantes.py:51-54`; `app/escala/services/cadastro_realidade.py:419-422`), mas não provam o corpo main `[nao verificado]`.

## 14/11 15:08 · sessao 3f5e5f8708fc · - · - · origem=Ronald (tmux)

Auditoria dos testes, sem alterar arquivos. [nao verificado]

| Teste | O que fixa na árvore | Comparação com main | [nao verificado]
|---|---|---|
| `test_e3_portas_v6_sem_banco.py:16–20` | Ação responder/colaborador + verbo validar → fio **colab** | P13: coincide com tarefa do main; contraria C17 **admin**. Referência main fornecida: `codex_v10_construtor.r`, tabela P13. |
| `test_e3_portas_v6_sem_banco.py:22–26` | Ação validar/admin + verbo cobrar → fio **admin** | P14: coincide com tarefa do main; contraria C17 **colab**. Referência main fornecida: `codex_v10_construtor.r`, tabela P14. |
| `test_e3_portas_v6_sem_banco.py:28–42` | Caso `('admin','cobrar','decidir','admin')` exige **admin**; publica os dois chamados/verbos | P19: fixa o conflito **admin** contra C17 **colab**; a geometria não é a mesma do anexo: o teste põe **ambos** em `fila_cel`, portanto espera `(101,202)`, não somente `(A,)` (`:38–42`). | [nao verificado]
| `test_e3_portas_tarefa.py:85–97` | ORM: A residual decidir; B externo GPS validar; tarefa responder sob B → **colab** | Variante de P13 com residual; contraria o fio **admin** de C17 sobre verbos decidir+validar. Fixture A/B em `:29–38`. |
| `test_e3_portas_tarefa.py:451–457` | Mesmo P13 sintético: responder+validar → **colab** | Repete a expectativa E3/tarefa do main, não C17. |
| `test_v8_R2_hipotese_acao_admin_isolada`, `test_e3_portas_tarefa.py:496–501` | Validar/admin isolado → PF/admin/validar | Não cobre simultâneo; não serve como prova de preservação de R2 (`:497`). |
| `test_v8_R2_validacao_e_resposta_alcancaveis_no_mesmo_dia`, `test_e3_portas_tarefa.py:503–515` | Simultâneo: **colab**, mantendo ações e verbos; testa porta compartilhada e portas distintas | Com mesma porta, só verbo validar: tarefa main **colab**, C17 **admin**. Com portas distintas, mapa validar+cobrar: C17 também **colab**; portanto essa segunda combinação não demonstra divergência de fio. |
| `test_v8_R2_alcance_admin_com_pergunta_colab`, `test_e3_portas_tarefa.py:221–225` | ORM: PF/validar/**colab** | Expectativa da tarefa main; diverge de C17/admin. O arranjo consulta fila/partição reais antes de afirmar resultado (`:203–219`). Execução ORM nesta auditoria: `[nao verificado]`. |
| `test_e3_diff_sombra.py:157–164` | Vigia: verbo validar, fio colab → `exige_admin=False` | Fixa precisamente a mudança do vigia E3, pois o main conta presença de verbo administrativo, conforme patch fornecido em `vigia_fio.py`, hunk `@@ -631,24 +636,31 @@`. |
| `test_e3_portas_v6_sem_banco.py:55–63` | Sem portas/residual: admin+colaborador → **colab**, projeção `None` | Trata prioridade interna sem porta; não demonstra preservação dos consumidores com porta. |
| `test_e3_portas_tarefa.py:473–479` | Ação admin/validar + residual cobrar+decidir → **colab** | É caso em que dono residual cobrar resolve a precedência; não cobre P14 isolado nem P19 residual decidir. |

Os testes v6 executam corpos reais extraídos por AST com fornecedores em memória; o próprio módulo declara que não substituem execução Django (`test_e3_portas_v6_sem_banco.py:1–4`; `e3_portas_apoio.py:18–26`). O executor puro remove a classe ORM inteira de `test_e3_portas_tarefa.py` (`e3_portas_regra.py:22–29`). [nao verificado]

Executei, com `PYTHONDONTWRITEBYTECODE=1`, somente os 10 testes v6 e os dois R2 puros: **10/10, 1/1 e 1/1 passaram**; nenhum banco acessado; resultados de terminal sem arquivo persistido: `[nao verificado]`. [nao verificado]

Há também catraca que bloqueia uma cura por simples retorno dos consumidores ao C17: somente o instrumento de sombra é permitido como chamador (`test_e3_pendentes_app.py:19–21,145–148`); um `_porta` da folha voltando ao C17 é explicitamente o mutante reprovado (`:150–159`). Essa expectativa teria de ser alinhada à regra de preservação solicitada, sem tratar a catraca atual como prova de equivalência com main.

O teste M6 preserva o **corpo do C17** e suas respostas próprias — inclusive pergunta externa validar → admin — mas não exige que os leitores migrados conservem essas respostas (`test_e3_pendentes_app.py:171–202`). [nao verificado]

## 14/11 15:07 · sessao fb10aa8e5988 · HOOKMSG · verificador · origem=Code

Referências marcadas **v3** usam a numeração nova do diff fornecido; as demais são da cópia-base.

1. **PROCEDENTE — o early-exit pode aprovar uma entrada ilegível.** Existe `git -C R`, mas seu retorno é ignorado: erro com stdout vazio → `tem_linha(False)` → `PADRAO_MSG_OK`, antes do leitor estrito. Isso permite verde sem conhecer a mensagem; ocorrência em produção: [nao verificado]. (`bin/tests/test_lei_akita.sh:89–92`, **v3**; leitor estrito: `:414–426`, **v3**.)

   A cura de argv fecha a herança isolada de `LEI_AKITA_COMMIT`: exige `--commit` e rejeita combinação com HEAD/MORDE. A régua não passa posicionais; o pre-push também não; o MORDE interno chama Python diretamente. Não apareceu mudança acidental de modo nesses caminhos. A bateria real também foi restaurada. (`bin/tests/test_lei_akita.sh:26–33`, **v3**; `bin/regua.sh:138–140`; `bin/pre-push.sh:38`, **v3**; `bin/tests/test_lei_akita.sh:979`, base; `bin/tests/test_hook_mensagem_em_toda_copia.sh:81–82`, **v3**.)

2. **PROCEDENTE — o selo ainda admite verde parcial e vermelho falso.** [nao verificado]

   **Verde parcial:** os cinco casos exercitam o script da cópia; instalado e alternativos recebem apenas “padrão inventado” e “sem linha”. Um hook que cite o script, recuse exclusivamente o texto inventado da fixture e aceite todo o restante passa, inclusive aceitando `n/a (instrumento)` em negócio. Os mutantes atuais não cobrem isso. (`bin/tests/test_hook_mensagem_em_toda_copia.sh:71–90`, `:101–115`, **v3**.)

   **Vermelho falso por relocação:** `_roda` copia somente o wrapper. Um hook válido que localize o script relativamente ao próprio `$0` perde seu destino na cópia e passa a recusar até mensagem sem linha. Existência desse wrapper em produção: [nao verificado]. (`bin/tests/test_hook_mensagem_em_toda_copia.sh:87–90`, **v3**.)

   **Vermelho falso por configuração herdada:** os plantados não fixam `core.hooksPath`; uma configuração global pode fazer seus commits ignorarem `.git/hooks`, inclusive o mutante, invalidando a mordida. Configuração global efetiva: [nao verificado]. (`bin/tests/test_hook_mensagem_em_toda_copia.sh:59–66`, `:101–109`, **v3**.)

   Quanto a efeitos colaterais: os casos usam o repo plantado, e o wrapper novo obtém dele o `TOPO`; não há escrita direta em `$wt` nesse fluxo. Porém, copiar um hook não restringe seus acessos absolutos. Escrita efetiva no repo real por algum alternativo: [nao verificado]. (`bin/tests/test_hook_mensagem_em_toda_copia.sh:35–42`, `:87–90`, **v3**; `bin/commit_msg.sh:19–29`, **v3**.)

3. **PROCEDENTE — reclassifico o descarte por “fora do escopo”; permanece preexistente, sem cura.** O integrador é chamador de `git commit` e consumidor de sua recusa: recebe falso, não interrompe nem desfaz a cópia, faz push e pode seguir ao deploy. A definição local de escopo inclui chamadores e leitores fora do diff. (`app/core/integrador_lote.py:481`, `:714–720`, `:739–753`; `CLAUDE.md:379–380`.)

   Há uma ordem ainda anterior: `rodar` solicita reload da cópia **antes** do commit; esse modo chega ao HUP. Portanto, interromper apenas o deploy posterior não fecha toda a publicação. (`app/core/integrador_lote.py:638`, `:701`; `bin/deploy.sh:82`, `:360`.)

   “Antes de religar a esteira” cobre a entrada oficial condicionada à pausa, mas a CLI admite `rodar` diretamente. A condição completa seria **antes de qualquer execução do integrador**. Uso direto em produção e alcance integral do E3-V10: [nao verificado]. A decisão fila/bloqueio cabe ao opus com o descarte anexado. (`bin/integrador.sh:19–32`; `bin/trava.sh:40–42`; `app/core/integrador_lote.py:614–638`, `:857–859`; `CLAUDE.md:376–377`.)

4. **PROCEDENTE — classes demonstradas:** selo sem mordida suficiente, pelos casos ausentes no hook efetivo; ordem de validação/publicação, pelo OK antes de validar a leitura e pelo reload anterior ao commit. (`bin/tests/test_hook_mensagem_em_toda_copia.sh:81–90`, **v3**; `bin/tests/test_lei_akita.sh:89–92`, **v3**; `app/core/integrador_lote.py:638`, `:719`.)

   **Segundo juiz, derivação paralela da regra e campo com dois significados: nada demonstrado no delta** — os dois modos chamam `julgar_commit`; o defeito apontado está no atalho de leitura anterior. (`bin/tests/test_lei_akita.sh:84–92`, `:421–443`, `:513`, `:524–529`, **v3**.)

## 14/11 15:07 · sessao ecf9ce507b74 · FRAC5-TEXTO · verificador · origem=Code

1. **PROCEDENTE — o texto proposto corresponde ao juiz atual, mas nem todas as portas o cumprem.** O juiz exige uma fração ≥14 e no máximo três; aceita `[14,2]`, sem mínimo de 5. (`app/ferias/models.py:251`, `:252`, `:267`, `:269`, `:271`; anexo `templates/ferias/agendar.html:10`)

   - **Remarcação comum:** pode transformar a única fração de 14 em 2, satisfeitas as demais guardas. A view chama `_trocar_janela`, que verifica estado/saldo e salva sem `clean()`; `save()` também não chama o juiz. (`app/ferias/views.py:723`; `app/ferias/services.py:707`, `:712`, `:716`, `:729`, `:745`; `app/ferias/models.py:444`)
   - **Histórico e correção administrativa:** dispensam deliberadamente a validação de fracionamento; a correção apresenta aviso. (`app/ferias/services.py:529`, `:547`; `app/ferias/views.py:508`, `:517`, `:531`)
   - **Aprovação:** a materialização usa `full_clean()`, mas sua recusa deixa a solicitação aprovada, com aviso e resposta `ok=True`. Portanto, aprovação não garante agendamento aceito. (`app/ferias/services.py:756`, `:759`, `:785`, `:790`; `app/ferias/views.py:799`)
   - **Cancelamento:** pode retirar a única fração ≥14 sem revalidar as restantes. (`app/ferias/services.py:652`, `:656`, `:657`, `:688`, `:689`)
   - Individual, lote e escrita pelo Haiku chegam ao mesmo juiz de criação. A API de férias recusa POST; o admin desta app registra somente `Afastamento`. (`app/ferias/services.py:578`, `:330`; `app/ferias/views.py:669`; `app/core/intake.py:34`; `app/ferias/services.py:306`; `app/api/views.py:2224`; `app/ferias/admin.py:4`)

2. **PROCEDENTE — outros textos exibidos:**

   - Outro formulário: “até 3 frações, uma ≥14 dias”. (`app/templates/ferias/_secao_colab.html:63`)
   - Erros: máximo três; pelo menos uma com mínimo 14. (`app/ferias/models.py:368`, `:376`)
   - Avisos do período: nenhuma alcança o mínimo; quantidade excede o máximo. (`app/ferias/situacao.py:231`, `:240`)
   - Proposta/correção: fração curta pede verificar as demais; esse aviso também entra na mensagem de sucesso da correção. (`app/ferias/situacao.py:341`, `:344`; `app/ferias/views.py:552`)
   - Haiku recebe e retransmite esses avisos. (`app/api/views_mensageria.py:812`, `:824`, `:847`; `mensageria/nucleo/ferramentas.py:2117`, `:2119`)

   O “menor deve ter no mínimo 14” está na linha antiga da cópia, substituída pelo diff. Outro equivalente em JS, comunicados persistidos ou respostas efetivamente geradas: **[não verificado]**. (`app/templates/ferias/agendar.html:10`)

3. **PROCEDENTE — o selo morde os casos pedidos, mas admite falsos verdes.** As referências abaixo são ao arquivo **do anexo** `ferias/test_frac5_texto_agendar.py`.

   - Texto antigo: vermelho pelas asserções da frase e de ausência de “menor”. (`ferias/test_frac5_texto_agendar.py:27`, `:28`)
   - Juiz passa a recusar `[14,2]`, mantendo o texto: vermelho. (`ferias/test_frac5_texto_agendar.py:38`, `:39`)
   - Linha com `art. 134` ausente ou duplicada: vermelho pela contagem. (`ferias/test_frac5_texto_agendar.py:18`, `:19`)
   - **Falso verde:** juiz passa a aceitar somente duas frações. O selo testa duas aceitas e quatro recusadas, mas não três aceitas. (`ferias/test_frac5_texto_agendar.py:32`, `:34`)
   - **Falso verde:** aviso inteiro dentro de comentário HTML; continua presente na fonte, embora desapareça da tela. (`ferias/test_frac5_texto_agendar.py:17`, `:18`, `:26`, `:27`)
   - **Falso verde:** acrescentar “as demais precisam ter ao menos cinco dias” escapa da busca pelas duas expressões reconhecidas. (`ferias/test_frac5_texto_agendar.py:39`)
   - **Origem:** não confere `origin.name`; outro template resolvido pelo loader pode satisfazer o selo. A view usa o mesmo nome lógico, mas a configuração admite diretório e templates de apps. Sombreamento efetivo: **[não verificado]**. (`ferias/test_frac5_texto_agendar.py:17`; `app/ferias/views.py:338`; `app/config/settings/base.py:73`, `:74`)

   Esses resultados foram conferidos pelas asserções em memória; execução real do selo pelo Django/loader: **[não verificado]**.

4. **PROCEDENTE — escritores, leitores e unidade:**

   - Escrita única: `FRACAO_MINIMA=14` e `FRACOES_MAXIMAS=3`. O primeiro é limiar da **maior** fração; o segundo limita a contagem das frações não nulas. (`app/ferias/models.py:251`, `:252`, `:267`, `:269`, `:271`)
   - Outros leitores: mensagens e pré-aviso em `situacao.py`; o anexo acrescenta comparações no selo. Há também uma ocorrência textual em teste estrutural. (`app/ferias/situacao.py:57`, `:234`, `:235`, `:242`, `:341`, `:346`; anexo `ferias/test_frac5_texto_agendar.py:13`, `:26`, `:27`; `app/ponto/tests/test_aus_espelho_1.py:43`)
   - Dias são corridos e inclusivos: diferença entre datas +1; construção do fim usa dias −1. (`app/ferias/models.py:371`, `:382`; `app/ferias/services.py:565`, `:524`)
   - Entretanto, outras frações são lidas pelo campo `dias` armazenado, e `_trocar_janela` recebe datas/duração separadamente sem conferir coerência. Uniformidade real dos registros: **[não verificado]**. (`app/ferias/models.py:372`; `app/ferias/situacao.py:122`; `app/ferias/services.py:694`, `:716`, `:729`)

5. **Classes de reprovação:**

   - **Derivação paralela de fonte única — PROCEDENTE:** pré-aviso decide por `dias < FRACAO_MINIMA`, sem consultar o conjunto; pode avisar sobre fração curta mesmo existindo outra ≥14. É aviso, não recusa. A coleta das frações também está duplicada entre porta e situação. (`app/ferias/situacao.py:341`, `:344`, `:117`; `app/ferias/models.py:362`)
   - **Selo sem mordida — PROCEDENTE parcialmente:** há mordida para regressão textual e `[14,2]`, mas faltam a fronteira de três frações, renderização e vínculo com as portas divergentes. (anexo `ferias/test_frac5_texto_agendar.py:17`, `:32`, `:34`, `:38`; `app/ferias/services.py:729`)
   - **Selo que copia valor — nada nas comparações de 3/14:** elas leem as constantes. Porém, a frase da tela permanece literal; a docstring não demonstra geração do texto pelo juiz. (anexo `ferias/test_frac5_texto_agendar.py:8`, `:26`, `:27`; anexo `templates/ferias/agendar.html:10`)

## 14/11 15:06 · sessao 645ea3ab9938 · - · - · origem=Ronald (tmux)

2. **PROCEDENTE — verde com guarda parcialmente inoperante.** A bateria de cinco casos exercita o hook plantado para `$R/bin`; o instalado e cada alternativo recebem apenas dois casos. Um hook que contenha a referência textual, recuse exclusivamente `Padrao Inventado Do Nada` e aceite o restante passa esses dois casos, inclusive os MORDE existentes, embora aceite `n/a (instrumento)` em `app/ponto`. (`bin/tests/test_hook_mensagem_em_toda_copia.sh:81`, `:86–90`, `:105`, `:107–115`, numeração do diff v3.)

2. **PROCEDENTE — vermelho falso por mudança de localização.** `_roda` copia somente o hook para outro diretório. Um alternativo válido em `$wt/.githooks`, com `exec bash "$(dirname "$0")/../bin/commit_msg.sh" "$@"`, passa no teste textual, mas perde o destino ao ser copiado para `$T/alt.*/.git/hooks`. A recusa de mensagem sem linha então acusa uma instalação válida. Existência desse wrapper em produção: **[nao verificado]**. (`bin/tests/test_hook_mensagem_em_toda_copia.sh:87`, `:90`.)

2. **PROCEDENTE — plantados herdam `core.hooksPath` global.** Os commits sintéticos não impõem seu próprio hooksPath; uma configuração global válida pode fazer o Git ignorar os hooks e mutantes gravados em `.git/hooks`, causando reprovação falsa do MORDE. Configuração global efetiva: **[nao verificado]**. (`bin/tests/test_hook_mensagem_em_toda_copia.sh:59–66`, `:101–109`, numeração do diff v3.)

A varredura apresentada não escreve diretamente em `$wt`: consulta configuração e executa os casos em repositórios plantados; o wrapper novo determina `TOPO` pelo Git daquele plantado. Executar uma cópia do hook não restringe acessos absolutos do seu conteúdo. Efeito colateral efetivo em repositório real: **[nao verificado]**, pois os corpos dos alternativos não foram fornecidos. (`bin/tests/test_hook_mensagem_em_toda_copia.sh:35–42`, `:87–90`; `bin/commit_msg.sh:19–29`, diff v3.)

## 14/11 15:05 · sessao 73a4d5d24f27 · - · - · origem=Ronald (tmux)

4. **PROCEDENTE — mapa de escritores/leitores e significado:**

- Escrita única das constantes: `app/ferias/models.py:251` (`FRACAO_MINIMA=14`) e `app/ferias/models.py:252` (`FRACOES_MAXIMAS=3`). O juiz lê a primeira como limiar da **maior** fração e a segunda como limite da quantidade de frações não nulas; não exige 14 de cada fração (`app/ferias/models.py:267-273`).
- Leitores adicionais: importação em `app/ferias/situacao.py:57`; mínimo nas mensagens em `app/ferias/situacao.py:234-235,346`; máximo na mensagem em `app/ferias/situacao.py:242`; mínimo no predicado de pré-aviso em `app/ferias/situacao.py:341`. O candidato acrescenta importação em `ferias/test_frac5_texto_agendar.py:13` e leitura no texto esperado em `ferias/test_frac5_texto_agendar.py:26-27` **do anexo**.
- A duração é de dias corridos, incluindo início e fim: `app/ferias/models.py:371,382-385`; reconstrução do fim por início + dias − 1 em `app/ferias/services.py:331,565`; histórico calcula fim − início + 1 em `app/ferias/services.py:524`; mensagens explicitam “dias corridos” em `app/ferias/situacao.py:233,345`.
- Leitura de frações antigas usa `dias` persistido, sem recalcular pelas datas: `app/ferias/situacao.py:115-122`; `app/ferias/models.py:362-374`. A fração atualmente validada tem dias recalculados e sua coerência conferida em `app/ferias/models.py:371,382-385`.
- `_trocar_janela` recebe datas e duração separadas, grava os três valores e não chama `clean`: `app/ferias/services.py:694,716,729`; seus wrappers estão em `app/ferias/services.py:736-745`. As views calculam o fim inclusivo em `app/ferias/views.py:507,718`. Coerência dos registros efetivamente persistidos: **[não verificado]**.

5. **PROCEDENTE — derivação paralela localizada:** o pré-aviso usa diretamente `int(dias) < FRACAO_MINIMA`, sem consultar o conjunto de frações nem `violacoes_de_fracionamento`; por isso uma segunda fração curta pode gerar aviso mesmo com outra ≥14. O texto manda verificar as demais e não recusa a operação (`app/ferias/situacao.py:341-346`); o aviso do período completo consulta o juiz (`app/ferias/situacao.py:223-242`).

O candidato compara o literal do template às constantes importadas; não passa a produzir o texto do template a partir do juiz (`ferias/test_frac5_texto_agendar.py:13,25-27` **do anexo**; `templates/ferias/agendar.html:10` **do anexo**).

## 14/11 15:05 · sessao 3d00bf154b5f · - · - · origem=Ronald (tmux)

Item 1 — **PROCEDENTE: remarcação normal aceita fracionamento que a criação recusa.** O formulário permite `min="1"` e chama `editar_agendamento`; a view chama `remarcar_agendamento`, que delega a `_trocar_janela`. Essa porta verifica estado e saldo, troca os dados e chama `save()`, sem `clean()`; o `save()` também não chama `clean()`. Portanto, uma única fração futura de 14 dias pode virar 2, satisfeitas as demais guardas; a criação dos mesmos 2 dias falha no `par1`. Evidências: `app/templates/ferias/_secao_colab.html:130`, `:135`; `app/ferias/views.py:723`; `app/ferias/services.py:707`, `:712`, `:716`, `:729`, `:745`; `app/ferias/models.py:374`, `:444`.

O texto proposto corresponde aos dois predicados do juiz para frações positivas: `max(fr) >= 14` e `len(fr) <= 3`; `[14,2]` passa e não existe mínimo de 5 nesse juiz. Evidências: `app/ferias/models.py:251`, `:252`, `:267`, `:269`, `:271`; texto proposto no anexo `app/templates/ferias/agendar.html:10`.

Criação individual, lote e escrita do Haiku chegam ao mesmo juiz: individual `app/ferias/views.py:314` → `app/ferias/services.py:578` → `app/ferias/views.py:259` → `app/ferias/services.py:330`; lote `app/ferias/views.py:654`, `:669`; Haiku `app/core/intake.py:34`, `:35`, `app/core/views_ia.py:684`, `app/ferias/services.py:301`, `:306`.

**PROCEDENTE: aprovação não equivale a agendamento aceito.** A solicitação é gravada como aprovada antes da tentativa de materialização. A materialização chama `full_clean()` e usa o mesmo juiz, mas a recusa vira aviso; a aprovação permanece e a view responde `ok=True`. Evidências: `app/ferias/services.py:756`, `:759`, `:785`, `:790`, `:791`; `app/ferias/views.py:799`.

As exceções deliberadas são diferentes da remarcação normal: histórico dispensa regras de fração futura e salva; correção administrativa explicitamente dispensa `full_clean()` e apresenta regra como aviso. Evidências: `app/ferias/services.py:529`, `:530`, `:547`; `app/ferias/views.py:508`, `:510`, `:513`, `:517`, `:531`.

API de férias recusa POST com 405; Django admin desta app registra somente Afastamento. Evidências: `app/api/views.py:2220`, `:2224`; `app/ferias/admin.py:2`, `:4`. Mensageria/Haiku de consulta retransmite avisos do core; evidências: `app/api/views_mensageria.py:772`, `:812`, `:824`; `mensageria/nucleo/ferramentas.py:2103`, `:2117`, `:2119`. Comportamento de PWA fora da cópia: **[não verificado]**.

Possível apoio ao item 5: o aviso hipotético não pergunta ao juiz agregado: usa diretamente `int(dias) < FRACAO_MINIMA`, enquanto a situação existente chama `violacoes_de_fracionamento`. Porém sua mensagem manda verificar as outras frações e não afirma proibição individual; é aviso conservador, não outra porta de recusa. Evidências: `app/ferias/situacao.py:227`, `:341`, `:344`, `:345`, `:346`.

## 14/11 15:05 · sessao f31cd24de3b1 · - · - · origem=Ronald (tmux)

Item 3: **PROCEDENTE, preexistente; adiamento não é cura.** `_commitar` devolve falso na recusa (`app/core/integrador_lote.py:481`); `_subir` apenas deixa de acumular a fatia, sem desfazer a cópia nem interromper (`app/core/integrador_lote.py:714-720`); faz push incondicional e, com rc zero, deploy (`app/core/integrador_lote.py:739-753`). A fatia recusada permanece na árvore publicada; só as commitadas recebem FIM (`app/core/integrador_lote.py:763-770`).

Não há fundamento para chamar isso de regressão introduzida pelo delta: o comportamento já está na cópia-base (`app/core/integrador_lote.py:719-753`). A cura HOOKMSG adiciona outra causa de recusa à mesma falha preexistente (`diff_v3.patch`, `bin/commit_msg.sh:29-31`).

A pausa protege a entrada oficial: o wrapper consulta os portões antes de montar e adquirir a árvore (`bin/integrador.sh:19-24`, `bin/integrador.sh:32`); a pausa fecha esses portões (`bin/trava.sh:40-42`, `bin/trava.sh:62-65`). Contudo, “dormente” não vale como garantia universal: a CLI admite `rodar` diretamente (`app/core/integrador_lote.py:857-859`), e `rodar` copia e recarrega sem consultar a pausa (`app/core/integrador_lote.py:614-638`). Uso direto em produção: **[não verificado]**.

Portanto, manter na fila é compatível com a pausa informada, mas a condição completa é **antes de qualquer execução do integrador, inclusive direta**. “Antes de religar a esteira” cobre o caminho oficial; não fecha todas as entradas (`bin/integrador.sh:19-32`; `app/core/integrador_lote.py:857-859`). O texto integral do aval E3-V10 e o estado atual da pausa não foram verificados independentemente: **[não verificado]**.

Classe concreta: **ordem/propagação de reprovação** — commit recusado não interrompe a publicação da árvore copiada (`app/core/integrador_lote.py:719-753`).

## 14/11 15:05 · sessao b7f716709981 · - · - · origem=Ronald (tmux)

Inventário do item 2:

- **Texto antigo ainda na cópia:** “menor deve ter no mínimo 14 dias”, em `app/templates/ferias/agendar.html:10`; é precisamente a linha substituída no diff fornecido. `app/templates/ferias/agendar.html:10`
- **Outro template:** “até 3 frações, uma ≥14 dias”; alinhado à regra atual, mas com números literais. `app/templates/ferias/_secao_colab.html:63`
- **Erros da porta:** máximo 3 frações; pelo menos uma com no mínimo 14 dias. Também são números literais. `app/ferias/models.py:368`, `app/ferias/models.py:376`
- **Aviso sobre lançamentos existentes:** nenhuma fração alcança `FRACAO_MINIMA`, explicitamente em dias corridos; ou quantidade superior a `FRACOES_MAXIMAS`. `app/ferias/situacao.py:231`, `app/ferias/situacao.py:240`
- **Aviso sobre proposta/correção:** se `int(dias) < FRACAO_MINIMA`, pede verificar as outras frações; deriva o aviso sem consultar o conjunto, mas não declara recusa. `app/ferias/situacao.py:341`, `app/ferias/situacao.py:344`
- **Mensagem na correção administrativa:** coleta esse aviso e o concatena à mensagem de sucesso. `app/ferias/views.py:516`, `app/ferias/views.py:549`, `app/ferias/views.py:552`
- **Haiku:** API entrega os avisos de situação e de proposta; ferramenta repassa os textos, sem regra própria nesses pontos. `app/api/views_mensageria.py:812`, `app/api/views_mensageria.py:824`, `app/api/views_mensageria.py:847`, `mensageria/nucleo/ferramentas.py:2103`, `mensageria/nucleo/ferramentas.py:2117`

Outro texto usuário equivalente a “menor deve ter 14”, fora da linha substituída, não foi localizado na busca textual. Ausência absoluta, respostas efetivamente geradas pelo Haiku e comunicados persistidos em banco: [não verificado].

## 14/11 15:02 · sessao 4e2677a2198a · - · - · origem=Ronald (tmux)

Use a fixture pronta `_Cenario` de [test_o261_c_viva_sem_chamado_do_dia.py:58](chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:58).

- **c2 :954:** `_cenario('orfao_14h', DIA_ABERTO)`, com `DIA_ABERTO=date(2026,10,6)`, seguido de `reemitir_chamado_do_dia(p, c, DIA_ABERTO)`. A fixture fornece escala, entrada, célula com furo e protesto; há controle positivo para esse motivo em [test_o261_c_viva_sem_chamado_do_dia.py:222](chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:222).
- **HIST :1642:** `_cenario('orfao_14h', DIA_ABERTO, viva=False, encerrado=True)`, seguido de `reatar_pergunta_orfa(p, c)`. A pergunta nasce validada; o histórico resolvido nasce por último para evitar reabertura pelos signals. [test_o261_c_viva_sem_chamado_do_dia.py:110](chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:110), [test_o261_c_viva_sem_chamado_do_dia.py:117](chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:117).
- **I terminal:** a assinatura é `I.encerrar()`, sem argumentos. Execute antes de inserir P; com P viva ancorada, o gate pode recusar ou reatar. [models.py:1605](chamados/models.py:1605), [models.py:1617](chamados/models.py:1617).

Execução desses casos: [não verificado].

## 14/11 15:01 · sessao d125622dc511 · HOOKMSG · verificador · origem=Code

As referências marcadas **diff** usam a numeração nova; **cópia** indica o código disponível.

1. **PROCEDENTE — cura incompleta na régua.** `LEI_AKITA_COMMIT` herdado **sozinho** ainda seleciona o julgamento isolado, encerrando o selo antes do universo e do MORDE; a régua chama o script sem limpar essa variável. O pre-push foi saneado. Ocorrência real desse ambiente: [nao verificado]. (diff: `bin/tests/test_lei_akita.sh:25`, `bin/tests/test_lei_akita.sh:997`, `bin/pre-push.sh:38`; cópia: `bin/regua.sh:140`.)

   A guarda não derruba o MORDE interno normal: ele chama diretamente `python3 -c`, sem passar novamente pela guarda shell; o hook também remove HEAD/MORDE antes de chamar o selo. (diff: `bin/tests/test_lei_akita.sh:1015`, `bin/commit_msg.sh:18`.)

2. **PROCEDENTE — mensagem sem linha pode ser recusada antes do retorno `None`.** `VOCAB = vocabulario(R)` executa antes de `julgar_commit`; GLOSSARIO ausente ou PLANO-PADROES/NORTE ilegível encerra o Python antes da checagem da mensagem. Portanto, um commit inicial ou uma árvore sem esses documentos pode ser recusado mesmo sem linha LEI-AKITA. (cópia: `bin/tests/test_lei_akita.sh:95`, `:101`, `:111`, `:129`; diff: `bin/tests/test_lei_akita.sh:418`.)

   Além disso, todos passam por `write-tree`/`commit-tree`; falha nesses comandos ou ausência de resposta do juiz produz recusa, independentemente da mensagem. (diff: `bin/commit_msg.sh:20`, `:26`; `bin/tests/test_lei_akita.sh:999`.)

   | Chamador | Efeito pelo código |
   |---|---|
   | `pos_push` | Mensagem fixa sem linha; passa se construção do objeto e carga do vocabulário funcionarem. (cópia: `bin/pos_push.sh:30`; diff: `bin/commit_msg.sh:26`, `bin/tests/test_lei_akita.sh:418`.) |
   | `relato` | Mensagem sem linha em outro repositório; publica os documentos na raiz. Se esse destino usar o hook novo e não tiver `app/docs/GLOSSARIO.md`, será recusado. Instalação do hook nesse destino: [nao verificado]. (cópia: `bin/relato.sh:23`, `:115`, `:126`, `:136`; `bin/tests/test_lei_akita.sh:95`.) |
   | `codex_estado` | Mensagem sem linha no destino do relato; mesma condição acima. Na recusa, encerra antes do push. (cópia: `bin/codex_estado.sh:14`, `:78`, `:83`.) |
   | Integrador | Lê `msg_commit.txt`; conteúdo concreto [nao verificado]. Sem linha, sofre a mesma dependência antecipada do vocabulário; falha de commit segue pelo caminho do item 5. (cópia: `app/core/integrador_lote.py:481`, `:719`; `bin/tests/test_lei_akita.sh:129`.) |
   | Merges de raia | Mensagem concreta [nao verificado]. Sem linha, mesma dependência; os pais são montados incluindo `MERGE_HEAD`. (cópia: `CLAUDE.md:91`; diff: `bin/commit_msg.sh:22`, `:24`, `bin/tests/test_lei_akita.sh:418`.) |

3. **PROCEDENTE — dois verdes restantes.**

   - **“Bateria 5/5” sem bateria do script real.** `$P` é plantado, mas `_bateria "$P"` nunca roda. A bateria só roda contra o mutante, cobrando uma única divergência; o instalado recebe apenas inventado/sem linha. Recusar termo válido, recusar `n/a` instrumental ou aceitar `n/a` em negócio pode permanecer verde. (diff: `bin/tests/test_hook_mensagem_em_toda_copia.sh:68`, `:80`, `:85`, `:97`, `:112`.)
   - **Hooks alternativos continuam avaliados só por texto.** `_varre_wt` usa `_delega`; o 1b executa apenas o hook do common dir. Um hook alternativo executável que cite o script em comentário e saia zero passa na varredura — exatamente a espécie de falso demonstrada pelo MORDE(c). Existência desse desvio real: [nao verificado]. (diff: `bin/tests/test_hook_mensagem_em_toda_copia.sh:23`, `:40`, `:90`, `:99`.)

   O `while read <<<"$lista"` não acrescenta uma entrada vazia nessa construção; lista vazia é tratada antes. Pasta ausente agora é vermelho deliberado, conforme a triagem; ocorrência real [nao verificado]. (diff: `bin/tests/test_hook_mensagem_em_toda_copia.sh:31`, `:33`, `:35`, `:41`.)

4. **nada.** O selo exige somente a presença de `LEI-AKITA` nesses textos; os campos acrescentados preservam esse veredito. O fabricante entrega o prompt inteiro ao modelo, enquanto `gerar.py` lê outros moldes, sem consumir o LEIAME. Efeito nas próximas mensagens geradas: [nao verificado]. (diff: `bin/tests/test_lei_akita.sh:58`; cópia: `bin/fabricante.sh:158`, `bin/molde_fatia/gerar.py:29`.)

5. **PROCEDENTE — reclassifico o descarte do integrador.** Continuar o push não demonstra que “a fatia sai sozinha”: a recusa apenas impede sua inclusão em `subiram`, sem desfazer os arquivos copiados; o deploy seguinte continua e importa a árvore viva. Assim, código da fatia recusada pode ser publicado sem o commit. Ocorrência real: [nao verificado]. (diff: `bin/commit_msg.sh:29`; cópia: `app/core/integrador_lote.py:317`, `:719`, `:739`, `:753`; `bin/deploy.sh:327`.)

   Existe ainda um reload anterior à suite e ao commit; a recusa posterior também não desfaz esse efeito. (cópia: `app/core/integrador_lote.py:638`, `:719`.)

## 14/11 15:00 · sessao 4e2677a2198a · - · - · origem=Ronald (tmux)

Sugestões para os testes ORM, sem substituir os escritores: [nao verificado]

| Chamador | Menor caminho |
|---|---|
| Gate de encerramento | Chamar `I._gate_pergunta_viva('fechado', registrar=True, plano=([p], [(p, G)]))`; a recusa da porta levanta `ValueError`. `chamados/models.py:421`, `chamados/models.py:437` |
| Reconciliador direto | Substituir somente `_derivar_chamado_destino` por `(G, D)` e chamar `reatar_pergunta_orfa(p, colab)`. `chamados/reconciliador.py:1557`, `chamados/reconciliador.py:1655` |
| Reconciliador histórico | Derivador retorna `(None, None)`; criar histórico terminal único, fora do alcance novo. Usar motivo não intervalo como contraexemplo. `chamados/reconciliador.py:1594`, `chamados/reconciliador.py:1642` |
| Reemissão c2 | G vivo com `modulo_origem='batida_ausente'` e `data_turno=D` evita gates de nascimento. A porta `abrir` e o reaponte permanecem reais. `chamados/reconciliador.py:838`, `chamados/reconciliador.py:954` |
| Reata ao LIMBO | `reatar_perguntas_ao_limbo([p.pk], G)` consulta e escreve pelas portas reais. `chamados/services/limbo_folgas.py:239`, `chamados/services/limbo_folgas.py:244` |
| Agrupamento de furo | `agrupar_furo(I, G)`; para isolar o chamador, substituir apenas `_vivas_ancoradas` por `[p]` e `reconciliar` por `False`. `chamados/services/limbo_folgas.py:279`, `chamados/services/limbo_folgas.py:282` |
| Migração do fio | `migrar_resolvedoras_vivas(I, G)`, com P única viva em I e G sem perguntas; conferir `recusadas`. `chamados/services/fio.py:2130`, `chamados/services/fio.py:2174` |
| Reversão do beco | Executar `Command._reverter` com `builtins.open` fornecendo `StringIO` do JSON; ORM e reaponte reais. `chamados/management/commands/reatar_beco_folga_ao_limbo.py:119`, `chamados/management/commands/reatar_beco_folga_ao_limbo.py:127` |

Decisão sugerida para reversão: aplicar a mesma guarda, sem exceção. LIMBO→I eleito passa; I eleito vivo→G recusa. O reaponte para `None` atualmente não consulta `_costura_nao_mata_o_fio`; protegê-lo exige chamada incondicional e suporte a destino nulo, ou declarar explicitamente esse caso fora do delta. `chamados/services/fio.py:910`

Existem fixtures úteis de migração, LIMBO e encerramento em `chamados/tests/test_e1_v5_chave_familia.py:169`, `chamados/tests/test_o261e_f1_codex_v5.py:106`, `chamados/tests/test_o261e_f1_porta_encerramento.py:46`. Execução ORM e resultados: [não verificado].

## 14/11 15:00 · sessao 9046ca1fcf8d · - · - · origem=Ronald (tmux)

**PROCEDENTE — reclassificar o descarte do integrador.** Continuar o push não isola a fatia recusada: `_commitar` falso apenas deixa de acrescentá-la a `subiram`; não chama `_desfazer` nem retira seus arquivos da árvore (`app/core/integrador_lote.py:719-720`). Os arquivos já foram copiados (`app/core/integrador_lote.py:317-324`), e o deploy posterior segue (`app/core/integrador_lote.py:739,753`), importando a árvore do disco (`bin/deploy.sh:327,360`). Portanto a fatia recusada pode ser publicada sem commit; ocorrência real [nao verificado].

Há ainda um reload anterior ao julgamento: `deploy.sh --reload-copia` roda antes da suite e dos commits (`app/core/integrador_lote.py:638-639`). A recusa posterior não desfaz esse efeito (`app/core/integrador_lote.py:719-720`).

A verificação de índice chamada pelo pre-push não fecha esse caminho: `--conferir` verifica somente existência e chamada do pre-commit, depois retorna 0 (`bin/index_vs_arvore.sh:35-58`).

## 14/11 14:59 · sessao d50aab665e87 · - · - · origem=Ronald (tmux)

Item 3 — dois PROCEDENTE:

- **Bateria 5/5 sem execução do caso real.** `_bateria` define cinco verificações, mas só é chamada contra o mutante que aceita tudo; o instalado passa por dois casos do 1b. O repo `$P` é plantado e nunca submetido a `_bateria`. Recusar termo legítimo, recusar `n/a` em `bin/` ou aceitar `n/a` em `app/ponto` pode permanecer verde, embora a saída anuncie “bateria 5/5”. Citações do diff: `bin/tests/test_hook_mensagem_em_toda_copia.sh:68`, `:80`, `:85`, `:97`, `:112`.

- **Hooks de `core.hooksPath` continuam com delegação apenas textual.** A varredura usa `_delega "$hp"`; o 1b executa exclusivamente o hook do common dir. Assim, uma worktree cujo hooksPath contenha hook executável que cite `bin/commit_msg.sh` em comentário e termine em `exit 0` passa na varredura. O MORDE(c) demonstra exatamente esse falso, mas exercita `_roda`, sem aplicar essa prova aos alvos alternativos da varredura. Citações do diff: `bin/tests/test_hook_mensagem_em_toda_copia.sh:23`, `:40`, `:90`, `:99`. Existência desse desvio na máquina real: [nao verificado].

O `while read ... <<< "$lista"` não introduz, sozinho, uma entrada vazia adicional: a substituição de comando remove os delimitadores finais e a here-string acrescenta o delimitador para a última entrada; não identifico achado nesse ponto do diff `bin/tests/test_hook_mensagem_em_toda_copia.sh:31` e `:41`. Worktree registrada sem pasta agora gera diagnóstico deliberadamente; ocorrência na máquina real: [nao verificado].

## 14/11 14:58 · sessao 51aec7f36248 · - · - · origem=Ronald (tmux)

1. **PROCEDENTE — cura incompleta do ambiente herdado na régua.** `bin/regua.sh:138-140` executa o selo sem remover `LEI_AKITA_COMMIT`. A nova guarda só recusa quando também há HEAD/MORDE; COMMIT sozinho ainda seleciona julgamento isolado e retorna antes de universo/MORDE (`bin/tests/test_lei_akita.sh:25-27,1000-1003`, linhas novas do diff). O pre-push está saneado (`bin/pre-push.sh:38`, diff); ocorrência real desse ambiente na régua [nao verificado].

A guarda não derruba o MORDE interno normal: ele chama diretamente `python3 -c`, sem reentrar na guarda shell (`bin/tests/test_lei_akita.sh:1015`, linha nova; base `:979`). A régua normal não acrescenta COMMIT/HEAD/MORDE (`bin/regua.sh:138-140`); o pre-push remove COMMIT (`bin/pre-push.sh:38`, diff); o hook remove HEAD/MORDE (`bin/commit_msg.sh:18`, diff).

4. **nada.** O selo lê os dois textos, mas exige somente a presença de `LEI-AKITA` (`bin/tests/test_lei_akita.sh:30,58-59`, linhas novas; base `:24,52-53`). Outro selo exige `registro.baixa`, fora do trecho alterado (`bin/tests/test_baixa_diferida_host.sh:34`; `bin/fabricante_prompt.md:49-58`, diff). O fabricante entrega o prompt inteiro ao modelo (`bin/fabricante.sh:158-162`); o gerador lê apenas `esteira.sh`, `rodar.sh` e `cadeia.sh` (`bin/molde_fatia/gerar.py:29-35`). O efeito nas próximas saídas do modelo [nao verificado].

_5175 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
