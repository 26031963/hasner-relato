# CODEX-ESTADO

> Ultima resposta processada: **17/11/2026 02:57** · publicado em **17/11/2026 02:57** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 17/11 02:57 · sessao cdb003f546de · E4C-F1-B2 · verificador · origem=Code

**Não encontrei bloqueador funcional demonstrado no delta apresentado.** A delegação elimina a cópia do predicado, e os novos testes cobrem a ausência de trilha e a guarda de contagem que o laudo apontava. Restam filas de cobertura e de medição dos efeitos indiretos. (`app/chamados/juizes.py:1591`; `app/chamados/tests/test_f1nova_b2_leitores.py:430`; `app/chamados/tests/test_f1nova_b2_leitores.py:452`; `app/chamados/tests/test_f1nova_b2_leitores.py:469`.)

A cópia não contém `.git`: identidade do HEAD e integralidade do diff **[nao verificado]**. Executei somente leitura e classificação AST em memória; execução dos testes Django, mutantes com banco e suite **[nao verificado]**. [nao verificado]

**1. Escritores, leitores e significado dos campos — sem bloqueio demonstrado; há distinções importantes.**

| Campo/valor | Quem escreve | Quem lê e o significado usado |
|---|---|---|
| Log L130: `usuario`, `valor_antes`, `valor_depois` | A porta monta a fotografia crua anterior, o estado posterior e o autor; `evento` converte `ator` em `usuario` e grava os JSONs. (`app/chamados/services/validacao.py:434`; `app/chamados/services/validacao.py:449`; `app/core/observ.py:118`; `app/core/observ.py:134`.) | Os testes interpretam essa ação especificamente. Auditoria e fotografia E4B alcançam a linha genericamente; não encontrei consumidor de negócio específico para seus JSONs. (`app/chamados/tests/test_f1nova_b2_leitores.py:438`; `app/core/views.py:705`; `app/chamados/services/foto_e4b.py:311`.) |
| `respondida_em` / `resposta_colab` | Resposta humana pelo core; resposta automática; recuperação do instante pela trilha; `carimbar`, quando existe texto; zeragem pela porta do lote, usada por reabertura, ressuscitação e devolução L130. (`app/chamados/services/disputa_emissao.py:1481`; `app/chamados/services/disputa_emissao.py:1674`; `app/chamados/services/disputa_emissao.py:1696`; `app/chamados/models.py:3251`; `app/chamados/models.py:3402`; `app/chamados/models.py:1639`; `app/chamados/services/validacao.py:440`.) | Juiz Python, faces SQL, property e estado; vitalidade; retenção; fila de validação; validar/recusar; app. Todos esses caminhos usam o par ou delegam ao juiz. (`app/chamados/juizes.py:1526`; `app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1565`; `app/chamados/models.py:3675`; `app/chamados/models.py:3164`; `app/chamados/juizes.py:1423`; `app/chamados/models.py:1933`; `app/chamados/juizes.py:1763`; `app/chamados/services/validacao.py:682`; `app/chamados/services/validacao.py:902`; `app/api/views.py:2578`.) |
| `validada_em` | Atribuição positiva em `_carimbar_validacao` e na opção `resposta='validar'` de `carimbar`. Desfazem: nova resposta, devoluções da validação/paridade, reabertura e ressuscitação. (`app/chamados/services/validacao.py:168`; `app/chamados/models.py:3255`; `app/chamados/services/disputa_emissao.py:1494`; `app/chamados/services/validacao.py:273`; `app/chamados/services/validacao.py:349`; `app/chamados/services/disputa_emissao.py:1609`; `app/chamados/models.py:1639`.) | Estado, vitalidade, retenção, materializador, portas de validação e seleção do estoque. É assinatura/processamento da validação, não substituto de “respondida”. (`app/chamados/models.py:3161`; `app/chamados/juizes.py:1405`; `app/chamados/models.py:1934`; `app/chamados/services/materializacao.py:2056`; `app/chamados/services/validacao.py:685`; `app/chamados/services/validacao.py:392`.) |
| `materializada_em` / `via_resolucao` | Carimbo em `_escrever_carimbo`, pelas portas `carimbar`, `carimbar_identidade` e `descarimbar`; lote em `descarimbar_em_lote`. Há duas portas próprias que mudam somente a via: `encerrar_por_fato_no_espelho` e `relavrar`, declaradas no selo. (`app/chamados/models.py:3218`; `app/chamados/models.py:3224`; `app/chamados/models.py:3289`; `app/chamados/models.py:3324`; `app/chamados/models.py:3402`; `app/chamados/models.py:3413`; `app/chamados/services/relavra_via.py:125`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:51`.) | Estado, C6, espelho SQL, retenção, materialização, estoque e leitores do lastro. Um carimbo não prova sozinho que nasceu batida: existem vias de identidade e resolução sem batida. (`app/chamados/models.py:3161`; `app/chamados/juizes.py:1416`; `app/chamados/juizes.py:1489`; `app/chamados/models.py:3494`; `app/chamados/models.py:1934`; `app/chamados/services/materializacao.py:2067`; `app/chamados/services/validacao.py:394`.) |
| `DecisaoPergunta` | Escrita física por `registrar`. Produtores: nascimento/ressuscitação/fechamento/retratação no modelo; resposta/reabertura/automática/recuperação pela trilha; validação/devoluções/L130; absorção; silêncio do veredito; backfills. (`app/chamados/models.py:4134`; `app/chamados/models.py:1621`; `app/chamados/models.py:2015`; `app/chamados/models.py:2147`; `app/chamados/models.py:3139`; `app/chamados/services/disputa_emissao.py:1525`; `app/chamados/services/disputa_emissao.py:1631`; `app/chamados/services/disputa_emissao.py:1678`; `app/chamados/services/disputa_emissao.py:1699`; `app/chamados/services/validacao.py:173`; `app/chamados/services/validacao.py:279`; `app/chamados/services/validacao.py:354`; `app/chamados/services/validacao.py:439`; `app/chamados/services/materializacao.py:1048`; `app/ponto/services/veredito_celula.py:195`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:324`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:141`.) | Histórico de respostas, tarefa do dia, fio, rótulo de devolução BUG-PAR, backfill e fotografia E4B. (`app/chamados/models.py:3681`; `app/chamados/services/tarefa_do_dia.py:158`; `app/chamados/services/fio.py:263`; `app/chamados/services/materializacao.py:275`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:98`; `app/chamados/services/foto_e4b.py:280`.) |

**Não é correto dizer que toda leitura desses campos pergunta a mesma coisa.** A API pode serializar o instante; a lavra calcula sua maior data; o histórico preserva um carimbo legado mesmo com texto vazio. Esses usos tratam instante e histórico, enquanto `pergunta_respondida` decide validade da resposta. (`app/api/views.py:2563`; `app/chamados/veredito_lavrado.py:206`; `app/chamados/models.py:4112`; `app/chamados/juizes.py:1552`.)

Para decidir “respondida”, o vazio é `(texto or '').strip()`: NULL, `''` e somente brancos são equivalentes, inclusive NBSP. O SQL constrói os brancos a partir de `str.isspace()`. Já a trilha L130 conserva o texto anterior cru; `DecisaoPergunta` normaliza texto falso para `''`, mas conserva brancos. Isso corresponde a preservação histórica, não a outro juiz de resposta. (`app/chamados/juizes.py:1528`; `app/chamados/juizes.py:1560`; `app/chamados/services/validacao.py:434`; `app/chamados/models.py:4115`.)

A porta de integridade recusa os dois meios-estados; o `save` verifica o par que ficará gravado, e a porta do lote também verifica as metades recebidas. Existe uma corrida concorrente expressamente declarada para saves fora da trava canônica; não é introduzida por este delta. (`app/chamados/services/fio.py:1607`; `app/chamados/models.py:3581`; `app/chamados/models.py:3591`; `app/chamados/models.py:3381`.)

**2. Três testes novos — corrigem os dois bloqueios de teste do laudo, por leitura.** [nao verificado]

| Mutação | Resultado esperado nos novos testes | [nao verificado]
|---|---|
| **(a) Apagar `evento(...)`** | Falha a exigência de exatamente uma linha por pergunta; se `_log` ficar indefinido, ocorre exceção antes. (`app/chamados/tests/test_f1nova_b2_leitores.py:438`; `app/chamados/services/validacao.py:449`.) |
| **(b) Apagar `if _log is None: raise`** | Falha o `assertRaises(TrilhaNaoGravada)` da porta direta. (`app/chamados/tests/test_f1nova_b2_leitores.py:459`; `app/chamados/services/validacao.py:455`.) |
| **(c) Apagar a guarda de contagem** | O segundo alvo falha na trilha, o comando captura e continua com uma devolvida; sem a guarda, não levanta o `CommandError` exigido e conserva a primeira alteração. (`app/chamados/management/commands/estoque_l130_instante_sem_texto.py:103`; `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:111`; `app/chamados/tests/test_f1nova_b2_leitores.py:469`.) |
| **(d) Gravar ator/antes/depois diferentes dos esperados** | As igualdades exatas falham para os dois alvos da fixture. (`app/chamados/tests/test_f1nova_b2_leitores.py:441`.) |

O patch **alcança a chamada real**: a porta importa `evento` localmente durante cada chamada, dentro do contexto do patch. O interceptador captura previamente a função real e só devolve `None` para a ação e os PKs escolhidos. (`app/chamados/services/validacao.py:423`; `app/chamados/tests/test_f1nova_b2_leitores.py:421`; `app/chamados/tests/test_f1nova_b2_leitores.py:459`; `app/chamados/tests/test_f1nova_b2_leitores.py:476`.)

**Fila — mutantes plausíveis sobreviventes:** substituir o ANTES por `resposta_colab=''` ou `via_resolucao=''` passa nos três testes novos: ambos começam nesses defaults, e a fixture só altera o instante. Também não se confere a descrição do evento. A implementação atual preserva corretamente os valores crus; a lacuna é de cobertura para brancos/NBSP e via anterior não vazia. (`app/chamados/models.py:2562`; `app/chamados/models.py:2676`; `app/chamados/tests/test_f1nova_b2_leitores.py:234`; `app/chamados/tests/test_f1nova_b2_leitores.py:438`; `app/chamados/services/validacao.py:434`; `app/chamados/services/validacao.py:450`.)

**3. `q_nao_respondida` — mesmo conjunto para todas as combinações perguntadas.**

O ramo retirado é exatamente o retorno de `q_respondida_sem_texto`; a troca é substituição por chamada, sem nova negação nem mudança de tratamento de NULL. (`app/chamados/juizes.py:1572`; `app/chamados/juizes.py:1591`.)

| `respondida_em` | Texto NULL | `''` | Só brancos, inclusive NBSP | Texto após trim |
|---|---:|---:|---:|---:|
| NULL | inclui | inclui | inclui | inclui |
| Presente | inclui | inclui | inclui | exclui |

A tabela decorre dos mesmos ramos antes/depois; NULL no texto é caso lógico previsto pela face, embora o campo atual não aceite NULL. (`app/chamados/juizes.py:1568`; `app/chamados/juizes.py:1572`; `app/chamados/juizes.py:1591`; `app/chamados/models.py:2562`.)

Os **sete chamadores de produção** encontrados usam a face em `filter()`:

- Canal: `app/chamados/canal.py:66`.
- Universo da ressuscitação: `app/chamados/models.py:1533`.
- Reabrir disputa pela view: `app/chamados/views.py:1418`.
- Validar inline: `app/chamados/views_cobrar.py:178`.
- Auditor C2: `app/chamados/management/commands/auditar_invariantes_chamados.py:1607`.
- Ação de reabertura: `app/chamados/services/acoes_disputa.py:98`.
- Recobrança: `app/chamados/services/disputa_emissao.py:1735`.

Ressuscitação e auditor encadeiam `exclude` sobre outros campos; nesses sítios não há inspeção de `Q.children`, `~q_nao_respondida()` ou dependência de `annotate`. (`app/chamados/models.py:1533`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1607`.)

**4. Coletor da catraca — nenhum RESPONDEDOR atual escondido em outro arquivo; limite futuro em fila.**

Reconstruindo em memória o coletor anterior fornecido no anexo, o conjunto anterior tem estes **24 membros**; o posterior é esse conjunto mais as três faces SQL abaixo. Sementes e fechamento transitivo são calculados em `app/chamados/tests/test_e3_catraca_respondedores.py:116`.

```text
_dias_das_perguntas_vivas
_pergunta_materializada_de_fato
_pergunta_viva_calc
_resposta_concorda_com_a_luz
_slot_esperado
alimentar_portas
classificar_resposta_validacao
competencia_do_chamado
declarou_hora
delta_da_resposta
disputa_viva_do_chamado
fio_vivo
motivo_implausivel_validacao
pergunta_respondida
pergunta_sem_lastro_ou_viva
pergunta_viva
perguntas_corrigiveis
perguntas_respondidas_a_validar
perguntas_vivas_de
porta_alcancavel_do_dia
projecao_temporal_da_pergunta
respondida_sem_texto
texto_sem_carimbo
texto_vazio
```

| Novo membro | O que faz com o texto |
|---|---|
| `q_respondida_sem_texto` | Exige instante e texto NULL ou composto somente de brancos. (`app/chamados/juizes.py:1572`.) |
| `q_respondida_com_texto` | Exige instante e algum caractere fora dos brancos. (`app/chamados/juizes.py:1580`.) |
| `q_nao_respondida` | Inclui ausência de instante ou delega ao primeiro predicado. (`app/chamados/juizes.py:1591`.) |

Executei o classificador real em memória: respondedores atuais `[]`; reconstrução anterior também `[]`. Mantendo o código atual e trocando somente o coletor, a única classificação alterada é `q_nao_respondida`, de RESPONDEDOR para PAR; nenhum outro arquivo muda de papel. (`app/chamados/tests/test_e3_catraca_respondedores.py:422`; `app/chamados/tests/test_e3_catraca_respondedores.py:385`.)

**Fila — a ampliação admite falso PAR futuro.** O coletor aceita qualquer kwarg cujo primeiro segmento seja `resposta_colab`, sem exigir consulta, e atravessa corpos diferidos por `ast.walk`. Confirmei em memória que uma função acrescentada ao juiz contendo apenas `qs.update(resposta_colab="x")` faz um consumidor `p.respondida_em is not None and função(qs)` virar PAR. Esse membro não existe na árvore atual. (`app/chamados/tests/test_e3_catraca_respondedores.py:124`; `app/chamados/tests/test_e3_catraca_respondedores.py:131`; `app/chamados/tests/test_e3_catraca_respondedores.py:151`; `app/chamados/tests/test_e3_catraca_respondedores.py:390`.)

**5. Retração, sonda e EXPORTADA — docstring corrigida; medição incompleta em fila.**

A docstring bate com o código: universo de crédito, `ignorar_tranca=True`, validação/materialização ausentes e resposta pelo juiz instante **E** texto. (`app/chamados/juizes.py:2104`; `app/chamados/juizes.py:2115`; `app/chamados/models.py:1933`.)

As diferenças CH fornecidas são compatíveis com “15 chamados, 113 perguntas a menos”: as remoções listadas somam 113, sem adição. A docstring registra essa medida; reprodução em produção **[nao verificado]**. (`app/chamados/juizes.py:2107`.)

O anexo mostra perda da retenção nos dois destinos, encerramento forçado permitido nos dois, `_retratar=True` somente no CH23072 e liberação do predicado CSF nos dois. São resultados do dado fornecido; execução independente **[nao verificado]**.

| Leitor/caminho | Efeito da mudança e limite da sonda |
|---|---|
| `retencao_da_retratacao` → `_retratar` | Perder irmãs remove uma retenção; ainda existe a retenção de justificativa alheia durante aprovação e o gate de `reconciliar`. (`app/chamados/reconciliador.py:448`; `app/chamados/reconciliador.py:453`; `app/chamados/reconciliador.py:520`.) |
| `encerrar_com_retencao` | Usa o encerramento forçado, distinto de reconciliar: não passa pelos mesmos gates de premissa/conversa. Isso permite resultados diferentes; a causa específica do `False` do CH33525 é **[nao verificado]**. (`app/chamados/reconciliador.py:498`; `app/chamados/models.py:1686`; `app/chamados/models.py:683`.) |
| Justificativa | Aprovação pode retratar a origem; ajuste retroativo percorre a cascata e força encerramentos com retenção. Enumerar justificativas relacionadas não demonstra executar essas portas nem o contexto de aprovação. (`app/ponto/services/justificativa.py:41`; `app/ponto/services/justificativa.py:46`; `app/ponto/services/justificativa.py:393`; `app/chamados/reconciliador.py:429`.) |
| Materialização `_fecha` | Sem irmãs, tenta reconciliar destino e clones. Execução desse ramo não aparece demonstrada no anexo **[nao verificado]**. (`app/chamados/services/materializacao.py:1246`; `app/chamados/services/materializacao.py:1258`.) |
| HX do pai | Pai terminal em disputa aberta deixa de ser reaberto por meias-respostas. Imprimir pais e listas não demonstra executar a reabertura. (`app/chamados/services/disputa_emissao.py:2025`; `app/chamados/services/disputa_emissao.py:2031`.) |
| CSF e gate do pai | CSF reúne perguntas da origem **e** do destino, com a mesma opção de ignorar tranca. O gate do pai lê essa lista. A sonda cobre o predicado, não o fechamento completo. (`app/ponto/supra_juiz.py:573`; `app/ponto/supra_juiz.py:588`; `app/chamados/models.py:716`.) |
| Auditor I12 | Com TM fechado e chamado vivo, irmãs levam ao piso I12r; sem elas, o chamado entra na violação I12. Esse leitor não foi medido na saída fornecida **[nao verificado]**. (`app/chamados/management/commands/auditar_invariantes_chamados.py:980`; `app/chamados/management/commands/auditar_invariantes_chamados.py:986`.) |

**Há alcance automático estrutural em EXPORTADA; execução concreta hoje não está demonstrada.** O consumidor de eventos está configurado a cada minuto e chama retratação por célula e por premissa sobre vivos do colaborador, sem cerca de competência nesses laços. O gate adicional de pergunta viva exclui expressamente competências entregues de sua proteção. Uma retratação admitida escreve chamado, pode rejeitar justificativa e aciona resolução de perguntas. (`app/config/crons.py:394`; `app/chamados/reconciliador.py:1367`; `app/chamados/reconciliador.py:1375`; `app/chamados/reconciliador.py:998`; `app/chamados/reconciliador.py:1456`; `app/chamados/models.py:364`; `app/chamados/reconciliador.py:524`.)

Isso **não prova que os dois CH anexados sejam retratados por esse cron hoje**: faltam evento pendente e aprovação dos demais juízos; a retratação por célula exige veredito que absolve, enquanto o anexo mostra `cobrado`. Execução desses casos pelo consumidor **[nao verificado]**. (`app/chamados/reconciliador.py:1346`; `app/chamados/reconciliador.py:1025`.)

O cron CSF executável usa janela corrente e ainda exige lavra/código/gates antes da retratação; os dois dias de setembro estão fora da janela informada no anexo. Instalação/execução efetiva dos crons **[nao verificado]**. (`app/config/crons.py:599`; `app/ponto/management/commands/supra_juiz.py:85`; `app/ponto/management/commands/supra_juiz.py:249`.)

A devolução direta do estoque continua recusando competência entregue, e seu comando permanece declarado como ato único. Não confundir essa proteção com uma cerca geral dos consumidores indiretos. (`app/chamados/services/validacao.py:400`; `app/config/crons.py:791`.)

**6. Classes de reprovação — resultado neste recorte.**

| Classe | Resultado |
|---|---|
| Segundo juiz / derivação paralela | A delegação remove a duplicação do ramo SQL; retenção e CSF leem o corpo compartilhado. Sem nova duplicação demonstrada. (`app/chamados/juizes.py:1591`; `app/chamados/juizes.py:2117`; `app/ponto/supra_juiz.py:573`.) |
| Segundo escritor fora da porta | L130 zera pela porta do lote e registra histórico pela porta da sequência. As duas escritas próprias de via estão declaradas no selo. (`app/chamados/services/validacao.py:439`; `app/chamados/services/validacao.py:440`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:51`.) |
| Relógio fora da borda | No comando, `hoje` nasce uma vez e segue até a porta; o segundo relógio serve ao arquivo de reversão. Sem nova violação demonstrada nesse caminho. (`app/chamados/management/commands/estoque_l130_instante_sem_texto.py:36`; `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:62`; `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:105`.) |
| Selo/RED sem mordida | Os bloqueios anteriores ganharam asserções relevantes; permanecem **em fila** os mutantes de fotografia constante e o falso PAR futuro descritos acima. (`app/chamados/tests/test_f1nova_b2_leitores.py:441`; `app/chamados/tests/test_f1nova_b2_leitores.py:460`; `app/chamados/tests/test_f1nova_b2_leitores.py:477`; `app/chamados/tests/test_e3_catraca_respondedores.py:124`.) |
| Ordem de trava | Lote trava a união antes das portas e confere FKs/elegibilidade; porta usa a ordem canônica. Sem inversão nova demonstrada no caminho do comando. (`app/chamados/management/commands/estoque_l130_instante_sem_texto.py:78`; `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:91`; `app/chamados/services/validacao.py:429`.) |
| Campo com dois significados | Histórico de carimbo legado e resposta válida são usos distintos explic

[truncado em 20000 de 20360 caracteres]

## 17/11 02:53 · sessao 732e39fefeed · - · - · origem=Ronald (tmux)

**Q2 — correções do laudo têm mordida por leitura do fluxo; execução dos testes/mutantes não verificada.** [nao verificado]

| Mutação | O que falha | Fonte |
|---|---|---|
| Remover `evento(...)` | Teste exige exatamente um log por pergunta, com ação/modelo/id; se restar `_log` indefinido, falha antes por exceção. | `app/chamados/tests/test_f1nova_b2_leitores.py:438-445`; `app/chamados/services/validacao.py:449-455` |
| Remover `if _log is None: raise` | Teste direto exige `TrilhaNaoGravada` quando evento devolve `None`. Sem a guarda, retorna `True` e deixa o ato. | `app/chamados/tests/test_f1nova_b2_leitores.py:459-467`; `app/chamados/services/validacao.py:455-458` |
| Remover guarda de contagem | O segundo alvo falha na trilha; comando captura a exceção e continua, ficando `devolvida=1`. Sem `:111-113`, o primeiro alvo fica devolvido e o comando não levanta o `CommandError` exigido. | `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:103-115`; `app/chamados/tests/test_f1nova_b2_leitores.py:469-483` |
| Ator/antes/depois errados | Igualdade exata dos três é exigida para ambos os alvos da fixture. | `app/chamados/tests/test_f1nova_b2_leitores.py:441-445` |

O patch **alcança a chamada real**: a porta faz import local de `core.observ.evento` a cada chamada (`app/chamados/services/validacao.py:423`), dentro dos contextos do patch (`app/chamados/tests/test_f1nova_b2_leitores.py:459`, `:476`). `_evento_falha_para` captura o evento real antes do patch e intercepta apenas ação + pk selecionados (`app/chamados/tests/test_f1nova_b2_leitores.py:421-427`).

**Fila — mutantes plausíveis sobreviventes nos três testes novos.** Trocar o ANTES por `resposta_colab=''` e/ou `via_resolucao=''` passa, pois ambos os campos são default `''` (`app/chamados/models.py:2562`, `:2676-2678`) e a fixture não os varia (`app/chamados/tests/test_f1nova_b2_leitores.py:234-237`). O primeiro perderia brancos/NBSP históricos; o segundo perderia via histórica não vazia. A implementação atual preserva ambos crus (`app/chamados/services/validacao.py:434-435`). Alterar/remover `descricao` também não é conferido pelos três testes (`app/chamados/tests/test_f1nova_b2_leitores.py:438-445`; evento em `app/chamados/services/validacao.py:450-451`). Isso é limitação de cobertura, sem defeito atual demonstrado.

**Atomicidade, trava e relógio — sem nova violação demonstrada no caminho do comando.** Comando abre atomic do lote, trava união inteira e confere FKs/filtro antes de chamar portas (`app/chamados/management/commands/estoque_l130_instante_sem_texto.py:73-105`); porta abre savepoint/atomic e usa trava canônica (`app/chamados/services/validacao.py:428-430`). A implementação da trava ordena Colaborador, Disputa, Chamado, Pergunta (`app/chamados/models.py:2781`, `:2790`, `:2803`, `:2807`). `hoje` nasce na borda e é repassado ao predicado/porta (`app/chamados/management/commands/estoque_l130_instante_sem_texto.py:36`, `:42`, `:99`, `:105`); a porta não chama relógio diretamente (`app/chamados/services/validacao.py:408-458`). A data do arquivo de reversão usa segundo relógio para nome/metadado, não para elegibilidade (`app/chamados/management/commands/estoque_l130_instante_sem_texto.py:62-67`).

**Escritores/leitores do log específico.** Porta monta ANTES e DEPOIS e passa autor (`app/chamados/services/validacao.py:434-454`); escritor genérico mapeia `ator` para `usuario`, `antes/depois` para JSON (`app/core/observ.py:118`, `:134-137`). O evento usa savepoint próprio e retorna `None` na falha best-effort (`app/core/observ.py:105`, `:133-139`), justificando a guarda fail-closed da porta (`app/chamados/services/validacao.py:455-457`). Pela busca literal, consumidores específicos dessa ação são os testes (`app/chamados/tests/test_f1nova_b2_leitores.py:415`, `:438-450`, `:465`, `:483`); consumidor de negócio dinâmico adicional [não verificado]. Leitores genéricos alcançam a linha: auditoria filtra por ação/modelo/usuário (`app/core/views.py:705-714`), mas template só mostra usuário/ação/descrição, sem os JSONs (`app/templates/core/log_auditoria.html:55-74`); foto E4B captura toda auditoria dos sujeitos (`app/chamados/services/foto_e4b.py:311-323`).

**DecisaoPergunta — histórico de projeção, não selo de resposta válida L-130.** A porta grava o anterior pela sequência antes de zerar (`app/chamados/services/validacao.py:436-440`). O carregador histórico aceita qualquer `respondida_em != None`, inclusive texto vazio, e normaliza texto falso para `''` (`app/chamados/models.py:4105-4119`). Isso corresponde ao propósito de preservar projeção legada (`app/chamados/models.py:4095-4104`). `registrar` é porta única, com registros imutáveis (`app/chamados/models.py:4124-4137`, `:4084-4090`). O leitor da tarefa verifica o texto da linha RESPOSTA e devolve ação de responder se vazio (`app/chamados/services/tarefa_do_dia.py:159-162`), coerente com L-130. A trilha histórica não é prova de `pergunta_respondida`.

Leitores adicionais: coleção de decisões no fio (`app/chamados/services/fio.py:251-264`), tarefa ordenada por ato/instante (`app/chamados/services/tarefa_do_dia.py:421-430`), foto E4B (`app/chamados/services/foto_e4b.py:280-281`), rótulo BUG-PAR que só procura DEVOLUCAO (`app/chamados/services/materializacao.py:275-276`), histórico de respostas (`app/chamados/models.py:3688-3697`; delegado em `app/chamados/services/disputa_emissao.py:2119-2123`). Neste último, a única RESPOSTA vazia fica como “vigente” da sequência até aparecer outra resposta ou ato com foto, portanto não aparece imediatamente na lista de anteriores; a porta L130 deliberadamente não cria REABERTURA/DEVOLUCAO (`app/chamados/services/validacao.py:417-419`).

## 17/11 02:53 · sessao 2703bf549fd7 · - · - · origem=Ronald (tmux)

Q3/Q4: não encontrei bloqueio atual; a delegação preserva o predicado e o coletor não encobre outro respondedor nesta cópia. (`app/chamados/juizes.py:1572`, `app/chamados/juizes.py:1591`, `app/chamados/tests/test_e3_catraca_respondedores.py:116`)

- A expressão anterior era `A | (B & (C | D))`; `q_respondida_sem_texto()` devolve exatamente `B & (C | D)`. Logo permanece: instante NULL inclui qualquer texto; instante presente inclui NULL, `''` e somente brancos, inclusive NBSP, e exclui texto não vazio após trim. (`app/chamados/juizes.py:1528`, `app/chamados/juizes.py:1560`, `app/chamados/juizes.py:1572`, `app/chamados/juizes.py:1591`)
- São sete chamadores de produção, todos usam `filter()`: canal:66; models:1533; views:1418; views_cobrar:178; auditar_invariantes_chamados:1607; acoes_disputa:98; disputa_emissao:1735. `models` e auditor encadeiam `exclude` sobre outros campos; nenhum inspeciona a estrutura do Q, aplica `~` nessa face ou depende de `annotate`. (`app/chamados/canal.py:66`, `app/chamados/models.py:1533`, `app/chamados/views.py:1418`, `app/chamados/views_cobrar.py:178`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1607`, `app/chamados/services/acoes_disputa.py:98`, `app/chamados/services/disputa_emissao.py:1735`)
- Antes eram 24 predicados; depois, os mesmos 24 mais as três faces SQL: `q_respondida_sem_texto`, `q_respondida_com_texto`, `q_nao_respondida`. As três decidem pela resposta; nenhum novo membro é um uso de `update`/`values` sem decisão pelo texto. (`app/chamados/tests/test_e3_catraca_respondedores.py:122`, `app/chamados/juizes.py:1572`, `app/chamados/juizes.py:1580`, `app/chamados/juizes.py:1591`)

Conjunto anterior completo, obtido pelo coletor antigo reconstruído em memória:

```text
_dias_das_perguntas_vivas
_pergunta_materializada_de_fato
_pergunta_viva_calc
_resposta_concorda_com_a_luz
_slot_esperado
alimentar_portas
classificar_resposta_validacao
competencia_do_chamado
declarou_hora
delta_da_resposta
disputa_viva_do_chamado
fio_vivo
motivo_implausivel_validacao
pergunta_respondida
pergunta_sem_lastro_ou_viva
pergunta_viva
perguntas_corrigiveis
perguntas_respondidas_a_validar
perguntas_vivas_de
porta_alcancavel_do_dia
projecao_temporal_da_pergunta
respondida_sem_texto
texto_sem_carimbo
texto_vazio
```

O conjunto posterior é esse conjunto unido às três faces SQL acima. O cálculo das sementes e do fecho está em `app/chamados/tests/test_e3_catraca_respondedores.py:116-136`.

Executei o classificador real, com `python3 -B` e `PYTHONDONTWRITEBYTECODE=1`, sobre `production_sources()` e `templates_da_arvore()`, sem Django, banco ou arquivos criados. Resultado: antes e depois, respondedores vivos vazios; mantendo o código HEAD e trocando apenas o coletor, a única mudança é `juizes.q_nao_respondida:1591`, de RESPONDEDOR para PAR. Nenhum outro arquivo muda de papel. (`app/chamados/tests/test_e3_pendentes_app.py:96`, `app/chamados/tests/test_e3_catraca_respondedores.py:403`, `app/chamados/tests/test_e3_catraca_respondedores.py:422`)

**Fila — limite futuro:** o coletor aceita qualquer keyword cujo primeiro segmento seja `resposta_colab`, inclusive `resposta_colab=` em escrita, e usa `ast.walk`, que atravessa corpos diferidos. Assim, uma futura função de escrita pode entrar indevidamente no conjunto; isso não ocorre entre os novos membros atuais. (`app/chamados/tests/test_e3_catraca_respondedores.py:124`, `app/chamados/tests/test_e3_catraca_respondedores.py:131`, `app/chamados/tests/test_e3_catraca_respondedores.py:144`)

## 17/11 02:53 · sessao 250803e680f9 · - · - · origem=Ronald (tmux)

**Pergunta 5 — a docstring está correta no predicado, mas a sonda não cobre a cadeia inteira.** `irmas_respondidas_sem_consumo` usa o universo de crédito e delega a `sem_veredito_entre(..., ignorar_tranca=True)`; esse juiz exige `validada_em/materializada_em` nulos e `pergunta_respondida` (instante E texto). `app/chamados/juizes.py:2115-2117`; `app/chamados/models.py:1926-1936`.

A frase “15 chamados, 113 perguntas a menos” corresponde às 15 linhas CH alteradas e ao resumo 111 ENTREGUE + 2 abertas do anexo; a lista das 111 está truncada, portanto sua cardinalidade não pode ser recontada integralmente. **[não verificado — anexo fornecido, sem arquivo:linha]**

| Gravidade | Achado e evidência |
|---|---|
| **fila** | A saída fornecida mede a lista de irmãs, retenção e chamadas diretas de `encerrar_com_retencao`/`_retratar` para CH23072/33525; mede também o resultado do juiz CSF. Não demonstra aprovação de justificativa, cascata real, `_fecha`, reabertura HX, executor CSF completo, I12 ou consumidor outbox. **[não verificado — limites da saída fornecida]** |
| **fila** | Perder a retenção não equivale a retratar. `retencao_da_retratacao` verifica irmãs e depois justificativa alheia no ato; `_retratar` ainda exige `ch.reconciliar`. `app/chamados/reconciliador.py:448-455`; `app/chamados/reconciliador.py:520-526`. |
| **fila** | Os caminhos de justificativa não foram executados na saída: aprovação chama `encerrar_chamado_de_origem` e `_encerrar_bloqueio_de_entrada`; ajuste retroativo percorre `_cascata_do_turno` e chama `encerrar_com_retencao`. `app/ponto/services/justificativa.py:217-218`; `app/ponto/services/justificativa.py:393-408`. A enumeração de cinco justificativas relacionadas não é execução dessas portas. **[não verificado — anexo fornecido]** |
| **fila** | `_fecha` da materialização consulta irmãs; sem elas tenta reconciliar destino e clones. Esse efeito não aparece executado na sonda. `app/chamados/services/materializacao.py:1246-1266`. |
| **fila** | HX só reabre pai terminal em disputa aberta quando ainda há irmãs; meia-resposta deixa de justificar reabertura. A sonda imprime pais/disputas/listas, mas não mostra execução desse ramo. `app/chamados/services/disputa_emissao.py:2025-2035`. |
| **fila** | CSF reúne perguntas do pai **e** do destino, com `ignorar_tranca=True`; `pode_executar_csf` nega quando a lista é não vazia. A mesma lista protege o gate do pai em `reconciliar`. `app/ponto/supra_juiz.py:573-588`; `app/chamados/models.py:716-719`. Portanto medir CSF cobre esse predicado compartilhado, mas não executa o fechamento pelo gate. |
| **fila** | I12 é outro leitor não medido: chamado de turno vivo com TM fechado vai ao piso I12r quando tem irmãs; sem irmãs vira violação I12. A contraprova ainda consulta a trilha histórica de retenção. `app/chamados/management/commands/auditar_invariantes_chamados.py:980-995`; `app/chamados/management/commands/auditar_invariantes_chamados.py:121-129`. |

**CH33525:** não atribuir seu `_retratar=False` a uma premissa específica. O encerramento forçado verifica motor e pergunta viva; a reconciliação verifica também premissa e conversa do pai. Isso explica por que os resultados podem divergir, mas a causa exata não foi exposta na sonda. `app/chamados/models.py:1686-1693`; `app/chamados/models.py:683-693`. **Causa específica de CH33525: [não verificado].**

**EXPORTADA e cron: alcance existe; execução concreta dos casos anexados não está provada.**

- O cron CSF executável usa `--executar` sem datas e, portanto, janela corrente. O comando exige também lavra `cobranca_indevida`, código, gate CSF e ausência de cobrança órfã de regeneração antes de tentar retratar. `app/config/crons.py:599`; `app/ponto/management/commands/supra_juiz.py:85-97`; `app/ponto/management/commands/supra_juiz.py:249-286`. Os dois chamados anexados são de setembro e estão fora da janela informada; não se pode dizer que esse cron os retratará hoje. **[não verificado — anexo fornecido]**
- Existe outro caminho automático: cron de um minuto → `consumir_eventos_celula` → retratação por célula e por premissa. Ele percorre todos os vivos pertinentes do colaborador sem cerca de competência entregue nesses laços. `app/config/crons.py:394`; `app/chamados/management/commands/consumir_eventos_celula.py:31`; `app/chamados/reconciliador.py:1367-1375`; `app/chamados/reconciliador.py:998-1000`; `app/chamados/reconciliador.py:1456-1467`.
- O gate de pergunta viva **exclui explicitamente competência entregue** da proteção adicional; portanto essa guarda não transforma o consumidor em proteção geral contra escrita em EXPORTADA. `app/chamados/models.py:364-365`. Uma retratação permitida escreve chamado, pode rejeitar justificativa pendente, resolve perguntas e acrescenta ocorrência; a transição também relavra perguntas de destino não validadas. `app/chamados/reconciliador.py:524-542`; `app/chamados/models.py:307-312`.
- Isso prova **alcance estrutural automático em EXPORTADA**, condicionado a evento pendente e aos demais juízos. Não prova que CH23072/33525 satisfaçam os produtores e os juízos desse consumidor hoje. Ambos mostram célula `cobrado` na saída, e a retratação por célula só aceita veredito que absolve e ausência de marcos pendentes. `app/chamados/reconciliador.py:1025-1033`. **Evento pendente e execução concreta: [não verificado].**
- O cron `reconciliar_chamados */15` é vigia, sem `--apply`; não confundir seus comentários históricos com escritor atual. `app/config/crons.py:395`; `app/chamados/management/commands/reconciliar_chamados.py:29`; `app/chamados/management/commands/reconciliar_chamados.py:71-85`.

**Pergunta 6, neste recorte:** não encontrei novo segundo juiz na retenção: retratação, materialização, HX e auditor delegam à mesma autoridade; CSF delega ao mesmo corpo de veredito. `app/chamados/reconciliador.py:448-449`; `app/chamados/services/materializacao.py:1243-1247`; `app/chamados/services/disputa_emissao.py:2024-2031`; `app/chamados/management/commands/auditar_invariantes_chamados.py:977-986`; `app/ponto/supra_juiz.py:573-574`. O achado demonstrável aqui é **quem-mais-lê/medição incompleta (fila)**; não classificaria a simples perda de retenção em entregues como violação do aval restrito de devolução de estoque. A ocorrência concreta de escrita automática indevida em entregues permanece **[não verificado]**.

## 17/11 01:36 · sessao d9fc54d941c8 · PREPARA-F2-F4-F7 · sonda · origem=Code

**F7 permanece com `lei 0` própria e `RED que morde 2`, como classificação estática.** A property continua divergindo do juiz; os prefills continuam lendo o texto cru. A execução desses REDs permanece **[nao verificado]**. (`app/chamados/models.py:3665`; `app/chamados/juizes.py:1552`; `app/templates/chamados/partials/_card_atendimento.html:114`; `app/templates/chamados/partials/_card_atendimento.html:148`; `app/templates/chamados/partials/_card_atendimento.html:154`.)

Todos os valores abaixo foram recebidos no dado raspado, sem reconferência independente: **[nao verificado]**. As referências ao código sustentam sua interpretação.

**SCRIPT 1 — projeção sem linha**

A função procura correspondência por **pergunta, ato e instante**. O recorte do comando inclui horizonte, perguntas sem dia e conversas; a projeção integral amplia essa seleção. (`app/chamados/models.py:4087`; `app/chamados/models.py:4094`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:55`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:59`.)

| Rótulo | Valor recebido | O que responde em §4/§5 |
|---|---:|---|
| `backfill_recorte_linhas_resposta` | 2992 [nao verificado] | **Confirma e dimensiona** RESPOSTAS projetadas sem linha correspondente no recorte. |
| `backfill_recorte_linhas_validacao` | 2248 [nao verificado] | **Confirma e dimensiona** VALIDAÇÕES projetadas sem correspondência no mesmo recorte. |
| `backfill_recorte_perguntas` | 3023 [nao verificado] | **Dimensiona** perguntas distintas com alguma dessas faltas. |
| `projecao_integral_linhas_resposta` | 16941 [nao verificado] | **Dimensiona** a falta de linhas RESPOSTA no universo integral. |
| `projecao_integral_linhas_validacao` | 15115 [nao verificado] | **Dimensiona** a falta de linhas VALIDAÇÃO no universo integral. |
| `projecao_integral_perguntas` | 16972 [nao verificado] | **Dimensiona** perguntas distintas afetadas no universo integral. |

Esses números preenchem a lacuna quantitativa do final da §4 e dimensionam o R1b da §5. Não estabelecem equivalência com o antigo fold nem demonstram revogação de validação humana; a função apenas propõe conservar a projeção existente. Recorte e integral não se somam. (`app/chamados/models.py:4082`; `app/chamados/models.py:4098`; `app/chamados/models.py:4102`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:67`.)

**SCRIPT 2 — divergências físicas e estados**

A property testa somente I; o juiz exige I e texto após trim. Portanto, a divergência contada corresponde a instante preenchido com texto vazio/branco. (`app/chamados/models.py:3665`; `app/chamados/juizes.py:1546`; `app/chamados/juizes.py:1552`.)

| Rótulo | Valor recebido | O que responde em §4/§5 |
|---|---:|---|
| `perguntas_lidas` | 50447 [nao verificado] | **Dimensiona** o universo percorrido pelas comparações. |
| `property_respondida_diverge_juiz` | 9626 [nao verificado] | **Confirma e dimensiona o primeiro RED** da §5 na sombra recebida. |
| `U2_fisico_ultima_devolucao_com_V` | 0 [nao verificado] | **Derruba a ocorrência atual dessa forma física no universo sondado**; não demonstra equivalência entre projeção e fold. |
| `U1_forma_materializada_sem_identidade` | 27486 [nao verificado] | **Dimensiona candidatos físicos U1**; materialização sem via de identidade ainda exige julgamento de lastro. |
| `U1_forma_juiz_lastro_false` | 4431 [nao verificado] | **Confirma e dimensiona discordância entre carimbo e juiz de lastro** dentro desses candidatos; não acrescenta automaticamente RED próprio F7. |
| `U1_falhas_juiz_lastro` | 0 [nao verificado] | **Confirma completude das chamadas externas desse juiz na sonda**; não significa ausência de diagnósticos internos. |

U2 usa a ordenação temporal da sequência. U1 exclui as vias que encerram por identidade e consulta o juiz de testemunha externa. (`app/chamados/models.py:4056`; `app/chamados/models.py:3202`; `app/chamados/juizes.py:666`.)

Os números seguintes contam retornos do **adaptador**, inclusive perguntas que podem ficar fora dos gates visuais. Não são contagens de botões apresentados nem de cliques executados. (`app/chamados/templatetags/chamados_tags.py:132`; `app/templates/chamados/partials/_pergunta_card.html:29`; `app/templates/chamados/partials/_pergunta_card.html:42`.)

| Rótulo | Valor recebido | O que responde em §4/§5 |
|---|---:|---|
| `estado_card_aplicada` | 5869 [nao verificado] | **Dimensiona** o ramo por carimbo; não certifica lastro. |
| `estado_card_aplicavel` | 430 [nao verificado] | **Dimensiona** o retorno “aplicável agora”; não prova validação ou plantio. |
| `estado_card_bloqueada_competencia` | 3 [nao verificado] | **Dimensiona** o impedimento F6 cujo gate conserva o toque de Validar. |
| `estado_card_bloqueada_competencia_entregue` | 65 [nao verificado] | **Confirma e dimensiona** o estado próprio de entregue, retirado de Validar pelos templates. |
| `estado_card_bloqueada_data` | 11 [nao verificado] | **Dimensiona** o impedimento de data em disputa. |
| `estado_card_bloqueada_paridade` | 122 [nao verificado] | **Dimensiona** o impedimento de paridade. |
| `estado_card_espera_declarado_x_gravado` | 9 [nao verificado] | **Confirma e dimensiona** a espera tratada na §4/§5. |
| `estado_card_fora_de_escopo` | 43205 [nao verificado] | **Dimensiona** retornos sem estado de aplicação; não significa perguntas sem resposta ou sem problema. |
| `estado_card_invalida` | 301 [nao verificado] | **Dimensiona** casos em que o derivador não produziu instante de plantio. |
| `estado_card_ja_satisfeita` | 75 [nao verificado] | **Dimensiona** batida aprovada já existente na janela. |
| `estado_card_recusada_fato_ocupado` | 28 [nao verificado] | **Dimensiona** recusas terminais por fato ocupado. |
| `estado_card_recusada_paridade` | 329 [nao verificado] | **Dimensiona** recusas terminais por paridade. |

As autoridades desses estados estão em `app/chamados/services/materializacao.py:840`, `app/chamados/services/materializacao.py:862`, `app/chamados/services/materializacao.py:886`, `app/chamados/services/materializacao.py:891`, `app/chamados/services/materializacao.py:895`, `app/chamados/services/materializacao.py:899`, `app/chamados/services/materializacao.py:903` e `app/chamados/services/materializacao.py:933`. Os gates distinguem entregue, espera e competência apenas fechada. (`app/templates/chamados/partials/_pergunta_card.html:44`; `app/templates/chamados/partials/_pergunta_card.html:49`; `app/templates/chamados/partials/modal_fio.html:274`.)

`aplicada` decorre do carimbo antes da consulta ao derivador; por isso seus 5869 não refutam os 4431 resultados U1 sem lastro. A interseção individual dessas contagens é **[nao verificado]**. (`app/chamados/services/materializacao.py:871`.)

O **segundo RED**, texto retirado reaparecendo em prefill, continua sustentado pelo código, mas não recebeu contador específico: a sonda não cruza texto sem I, reabertura, vitalidade e tipo de entrada. Sua ocorrência individual na sombra é **[nao verificado]**. (`app/templates/chamados/partials/_card_atendimento.html:114`; `app/templates/chamados/partials/_card_atendimento.html:148`; `app/templates/chamados/partials/_card_atendimento.html:154`.)

**SCRIPT 3 — BUG-PAR e U163**

O contador parte dos IDs fechados de U163 e separa carimbadas das atualmente sem carimbo; pertencimento não deriva do estado ou da recusa. (`app/chamados/services/universo_bug_par.py:12`; `app/chamados/services/universo_bug_par.py:33`; `app/chamados/services/materializacao.py:321`; `app/chamados/services/materializacao.py:326`.)

| Rótulo | Valor recebido | O que responde em §4/§5 |
|---|---:|---|
| `BUGPAR_universo` | 60 [nao verificado] | **Dimensiona** U163 atualmente sem carimbo; não comprova identidade com os IDs originais de U60. |
| `BUGPAR_n_aplicaveis` | 40 [nao verificado] | **Dimensiona** as atualmente aplicáveis pela testemunha. |
| `BUGPAR_resolvidas` | 0 [nao verificado] | **Dimensiona** ausência de carimbadas que deixaram de satisfazer o predicado BUG-PAR-103. |
| `BUGPAR_pendentes_aval` | 79 [nao verificado] | **Dimensiona** pendências administrativas, incluindo estados que impedem validar. |
| `BUGPAR_congelados_no_recorte` | 163 [nao verificado] | **Dimensiona** pertencimento presente no recorte; esse contador não deve zerar. |
| `BUGPAR_legitimas` | 10 [nao verificado] | **Dimensiona** retornos `bloqueada_paridade`. |
| `BUGPAR_outras` | 1 [nao verificado] | **Dimensiona** outro estado; qual estado e pergunta: **[nao verificado]**. |
| `BUGPAR_espera_declarado` | 9 [nao verificado] | **Confirma e dimensiona** a espera declarado × gravado da §4/§5. |
| `BUGPAR_bug_par_103` | 19 [nao verificado] | **Dimensiona** carimbadas absolvidas devolvíveis em competência não entregue. |
| `BUGPAR_bug_par_103_pauta_dp` | 84 [nao verificado] | **Dimensiona** carimbadas absolvidas em competência entregue, separadas para Pauta DP. |
| `BUGPAR_bug_par_103_sem_dia` | 0 [nao verificado] | **Dimensiona** ausência de dia faltante nesse subconjunto carimbado. |
| `U163_existentes` | 163 [nao verificado] | **Confirma e dimensiona** presença dos IDs da fonte fechada. |
| `U163_competencia_entregue` | 85 [nao verificado] | **Dimensiona** toda U163 em entregue pelo dia do fato; não se soma aos 84 de Pauta DP. |
| `U163_sem_dia_do_fato` | 0 [nao verificado] | **Dimensiona** ausência de dia faltante em toda U163 existente; não responde pelo universo fora163. |

As partições são as do contador real: `40+10+1+9=60`, `60+19=79` e `79+0+84+0=163`, aplicadas aos valores recebidos **[nao verificado]**. (`app/chamados/services/materializacao.py:330`; `app/chamados/services/materializacao.py:339`; `app/chamados/services/materializacao.py:351`; `app/chamados/services/materializacao.py:356`.)

**Correção da §4:** já há redação registrada para `CORTE-U163-NO-K6`: retirar a exclusão de U163 da guarda L1/L5 e manter F6 intocada. Cai a lacuna “literal não verificado”; permanece a implementação pendente na F4, pois o código ainda pula U163. Os 85 dimensionam esse complemento; os 40 aplicáveis são resultado da guarda atual, não previsão após a mudança. (`app/docs/BACKLOG.md:823`; `app/chamados/services/materializacao.py:795`; `app/chamados/services/materializacao.py:806`.)

**SCRIPT 4 — ressuscitação**

A seleção real exige materialização, texto `NULL` ou vazio, exclusões por via, guarda do marco e C6 sobre a pergunta como ficaria após a limpeza. Portanto, não é censo de toda resposta vazia ou de todos os pares ilegais. (`app/chamados/models.py:1530`; `app/chamados/models.py:1533`; `app/chamados/models.py:1550`; `app/chamados/models.py:1553`.)

| Rótulo | Valor recebido | O que responde em §4/§5 |
|---|---:|---|
| `ressuscitacao_atual_chamados` | 730 [nao verificado] | **Dimensiona** chamados com seleção não vazia pela porta atual. |
| `ressuscitacao_atual_perguntas` | 2025 [nao verificado] | **Dimensiona** perguntas distintas que seriam selecionadas. |
| `ressuscitacao_atual_colaboradores` | 119 [nao verificado] | **Dimensiona** pessoas distintas alcançadas pela seleção. |

Isso preenche o alcance da dependência herdada na §4, sem decidir “deveria voltar ao colaborador?”. Essa pergunta continua expressamente não decidida. Não demonstra ressuscitação executada. (`app/chamados/models.py:1501`; `app/docs/LEIS.md:151`; `app/docs/CORTES.md:488`.)

**ERROR e números ausentes**

**Scripts 2/3, causa em uma linha:** marco cobrado e ATA discordam no tipo; o código registra `Exception` como diagnóstico e continua retornando o tipo do marco cobrado. (`app/chamados/services/materializacao.py:568`; `app/chamados/services/materializacao.py:571`; `app/chamados/services/materializacao.py:576`.)

`NoneType: None` decorre de `exc_info=True` nessa chamada sem exceção ativa. (`app/core/observ.py:79`; `app/chamados/services/materializacao.py:573`.)

No dado recebido, os scripts 1–4 imprimiram todos os números previstos e terminou `rc_sombra=0`: nenhum número solicitado ficou ausente por esses diagnósticos **[nao verificado]**. Não há contador dedicado de divergências de marco; `U1_falhas_juiz_lastro=0` não mede isso. (`app/core/observ.py:75`; `app/core/observ.py:77`.)

O SCRIPT 5 foi retirado deliberadamente desta volta; não apresentou falha nem número de frota **[nao verificado]**.

**Contagem final:** nenhum número acrescenta pergunta própria de lei F7 ou novo RED que morde. Mantêm-se `lei 0` própria e `RED que morde 2`; o primeiro ganhou dimensão de sombra, e o segundo continua sem dimensão de frota. A pergunta herdada de ressuscitação permanece aberta; U163 passa de lacuna documental a complemento F4 já decidido. (`app/docs/LEIS.md:151`; `app/docs/BACKLOG.md:823`; `app/chamados/models.py:3665`; `app/chamados/juizes.py:1552`; `app/templates/chamados/partials/_card_atendimento.html:148`.)

## 17/11 01:35 · sessao ab507d5ea233 · - · - · origem=Ronald (tmux)

SCRIPT 3 interpreta assim os valores do dado fornecido; confirmação independente dos valores em banco: **[nao verificado]**.

| Rótulo | Valor | Resposta às seções 4/5 |
|---|---:|---|
| `BUGPAR_universo` | 60 | Dimensiona as perguntas atuais de U163 sem carimbo de materialização. Não comprova que os IDs sejam exatamente os do conjunto original U60. (`app/chamados/services/materializacao.py:321`, `:326`, `:338`, `:351`.) |
| `BUGPAR_n_aplicaveis` | 40 | Dimensiona as que a testemunha real devolve como `aplicavel`; não comprova execução da validação nem teste RED. (`app/chamados/services/materializacao.py:339`, `:340`, `:352`.) |
| `BUGPAR_resolvidas` | 0 | Nenhuma carimbada deixou de satisfazer o predicado `paridade_absolvida_bug_par_103` neste recorte. (`app/chamados/services/materializacao.py:326`, `:327`, `:328`.) |
| `BUGPAR_pendentes_aval` | 79 | Dimensiona pendências administrativas: `universo + len(bug_par_103)` = 60+19, incluindo estados que ainda impedem validar. (`app/chamados/services/materializacao.py:314`, `:356`.) |
| `BUGPAR_congelados_no_recorte` | 163 | Dimensiona pertencimento fechado no recorte; não é um contador destinado a zerar. (`app/chamados/services/materializacao.py:315`, `:356`; `app/chamados/services/universo_bug_par.py:30`.) |
| `BUGPAR_legitimas` | 10 | Dimensiona respostas cuja testemunha retorna `bloqueada_paridade`; não significa dez leis ou dez REDs. (`app/chamados/services/materializacao.py:342`, `:343`.) |
| `BUGPAR_outras` | 1 | Dimensiona um estado diferente de aplicável/paridade/espera; o estado individual não consta do dado. (`app/chamados/services/materializacao.py:344`, `:346`, `:347`; identidade/estado: **[nao verificado]**.) |
| `BUGPAR_espera_declarado` | 9 | Confirma ocorrência da espera prevista pelo corte declarado versus gravado; dimensiona o caso RED da seção 5, sem demonstrar o HTML ou clique. (`app/chamados/services/materializacao.py:305`, `:344`, `:736`, `:741`.) |
| `BUGPAR_bug_par_103` | 19 | Dimensiona carimbadas absolvidas devolvíveis, com competência não entregue. (`app/chamados/services/materializacao.py:326`, `:330`, `:335`.) |
| `BUGPAR_bug_par_103_pauta_dp` | 84 | Dimensiona carimbadas absolvidas em competência entregue, separadas para Pauta DP. (`app/chamados/services/materializacao.py:333`, `:334`; `app/chamados/services/validacao.py:338`, `:339`.) |
| `BUGPAR_bug_par_103_sem_dia` | 0 | Nenhuma carimbada absolvida sem data do fato nesse subconjunto. (`app/chamados/services/materializacao.py:330`, `:331`, `:332`.) |
| `U163_existentes` | 163 | Dimensiona presença em banco dos IDs da fonte fechada; pertencimento não depende de recusa ou lâmpada. (`app/chamados/services/universo_bug_par.py:12`, `:20`, `:30`, `:33`.) |
| `U163_competencia_entregue` | 85 | Dimensiona toda a interseção U163 × competência entregue pelo dia do fato, maior que o subconjunto Pauta DP de 84; não soma aos 84. (`app/chamados/services/materializacao.py:776`, `:784`, `:788`, `:789`.) |
| `U163_sem_dia_do_fato` | 0 | Nenhuma pergunta existente de U163 sem dia do fato; não extrapola às perguntas fora163. (`app/chamados/services/materializacao.py:782`, `:784`.) |

As partições fecham: 40+10+1+9=60; 60+19=79; 79+0+84+0=163. São relações do contador real. (`app/chamados/services/materializacao.py:351`, `:356`.) Como as carimbadas são 19+84 e `resolvidas=0`, a diferença 85−84 situa uma entregue entre as 60 sem carimbo; sua identidade é **[nao verificado]**.

**Correção relevante da seção 4:** a cópia contém a redação registrada do `CORTE-U163-NO-K6`: retirar a exclusão das 163 de `k6_consulta_a_pergunta`, aplicar L1/L5 e manter F6 intocada; é complemento F4 com sonda antes de construir. A antiga lacuna “literal [nao verificado]” pode ser baixada. (`app/docs/BACKLOG.md:823`.) O código ainda exclui U163, portanto os 85 dimensionam esse complemento pendente, sem constituir nova pergunta de lei F7. (`app/chamados/services/materializacao.py:792`, `:795`, `:806`.)

**ERROR dos scripts 2/3, em uma linha:** divergência entre o tipo do marco cobrado e o tipo da ATA; o código registra um objeto `Exception` e segue retornando o tipo do marco cobrado, sem `raise` nesse ramo. (`app/chamados/services/materializacao.py:527`, `:568`, `:571`, `:576`.) O `NoneType: None` vem de `exc_info=True` sem exceção ativa nessa chamada. (`app/core/observ.py:79`; `app/chamados/services/materializacao.py:573`.)

Não há evidência de script abortado: os números solicitados dos scripts 1–4 e `rc_sombra=0` estão no dado; confirmação independente da execução: **[nao verificado]**. Script 5 foi explicitamente excluído, não falhou **[nao verificado]**. Falta número próprio para divergências de marco e exceções engolidas pelo adaptador: `U1_falhas_juiz_lastro=0` não mede esses eventos. (`app/core/observ.py:75`, `:77`; `app/chamados/templatetags/chamados_tags.py:135`, `:137`.)

## 17/11 01:34 · sessao 19c1683c44fa · - · - · origem=Ronald (tmux)

Somente leitura; conclusões das sondas 1 e 4:

| NUMERO recebido | Interpretação e efeito em §4/§5 | Código atual |
|---|---|---|
| `backfill_recorte_linhas_resposta = 2992` | Linhas RESPOSTA propostas no recorte do comando; confirma existência e dimensiona projeção sem correspondência na sequência. Dimensiona §4 e R1b de §5; não fecha o antigo oráculo de fold. Valor recebido: [nao verificado] | `app/chamados/management/commands/backfill_decisoes_pergunta.py:55`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:67`; `app/chamados/models.py:4098` |
| `backfill_recorte_linhas_validacao = 2248` | Mesmo recorte, linhas VALIDACAO propostas. Dimensiona a conservação necessária; não significa 2248 perguntas adicionais. Valor recebido: [nao verificado] | `app/chamados/models.py:4102` |
| `backfill_recorte_perguntas = 3023` | Perguntas distintas com ao menos uma das duas faltas. O recorte inclui horizonte L-124, perguntas sem dia e conversas. Valor recebido: [nao verificado] | `app/chamados/management/commands/backfill_decisoes_pergunta.py:55`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:59` |
| `projecao_integral_linhas_resposta = 16941` | Linhas RESPOSTA propostas sobre todas as perguntas percorridas pela sonda; dimensiona estoque integral, além do recorte. Valor recebido: [nao verificado] | `app/chamados/models.py:4091`; `app/chamados/models.py:4098` |
| `projecao_integral_linhas_validacao = 15115` | Linhas VALIDACAO propostas no universo integral. Valor recebido: [nao verificado] | `app/chamados/models.py:4102` |
| `projecao_integral_perguntas = 16972` | Perguntas distintas com alguma falta no universo integral. Recorte e integral não se somam, pois a função de domínio é a mesma e o comando restringe a seleção. Valor recebido: [nao verificado] | `app/chamados/management/commands/backfill_decisoes_pergunta.py:56`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:67`; `app/chamados/models.py:4089` |
| `ressuscitacao_atual_chamados = 730` | Chamados com lista não vazia da porta hoje; dimensiona a dependência herdada de ressuscitação em §4. Valor recebido: [nao verificado] | `app/chamados/models.py:1501`; `app/chamados/models.py:1507` |
| `ressuscitacao_atual_perguntas = 2025` | Perguntas distintas que a porta selecionaria; não demonstra que foram ressuscitadas. Valor recebido: [nao verificado] | `app/chamados/models.py:1502`; `app/chamados/models.py:1530`; `app/chamados/models.py:1557` |
| `ressuscitacao_atual_colaboradores = 119` | Pessoas distintas abrangidas pelas 2025 perguntas; dimensiona alcance da pendência herdada. Não fornece lista individual. Valor/lista: [nao verificado] | `app/chamados/models.py:1507` |

A falta de linha é definida por correspondência de **pergunta, ato e instante**, e não por inexistência de qualquer RESPOSTA/VALIDACAO. A função conserva o texto atualmente projetado; não recupera resposta antiga já sobrescrita. (`app/chamados/models.py:4082`; `app/chamados/models.py:4087`; `app/chamados/models.py:4094`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:13`.)

O universo da ressuscitação exige materialização presente e texto `NULL` ou `''`, exclui vias fora do fato, pula marco já aceso e consulta C6 sobre cópia em memória após a limpeza. Portanto, os 2025 não são censo de toda resposta vazia, nem de todos os pares ilegais da L-130. (`app/chamados/models.py:1530`; `app/chamados/models.py:1533`; `app/chamados/models.py:1535`; `app/chamados/models.py:1550`; `app/chamados/models.py:1553`; `app/chamados/models.py:1557`.)

Os números da sonda 4 **dimensionam**, sem resolver “deveria voltar ao colaborador?”. Essa lei continua expressamente não decidida; a implementação conserva a fila atual, limpa o par inteiro e registra previamente as linhas projetadas faltantes, sem usar REABERTURA para mudar o dono. (`app/docs/CORTES.md:488`; `app/docs/LEIS.md:151`; `app/chamados/models.py:1613`; `app/chamados/models.py:1622`; `app/chamados/models.py:1629`.)

Essas duas sondas, por seus predicados, não acrescentam pergunta própria de lei F7 nem novo caso de RED: R1b permanece dimensão de conservação, sem prova do antigo fold; ressuscitação continua dependência herdada. Contagem final depende da conferência das demais sondas e do código atual dos dois REDs. (`app/chamados/models.py:4087`; `app/docs/CORTES.md:488`.)

## 17/11 01:27 · sessao a1991c3094f8 · PREPARA-F2-F4-F7 · sonda · origem=Code

**Os números não mudam a entrega: lei 0 nova; RED que morde 3.** Eles dimensionam o estoque e derrubam a inferência “uma U163 entregue na fila ⇒ uma que hoje plantaria”. Não demonstram execução dos três RED. (`app/docs/RELATO.md:17`; resultados da sonda [nao verificado])

Na cópia atual, lista e contador já constam respondidos por complemento; permanece devolvida a condição completa do RED. Essa atualização vem do registro do corte, não dos números. (`app/docs/RELATO.md:5`, `app/docs/RELATO.md:8`)

Os valores abaixo são os fornecidos no **DADO RASPADO**, sem verificação independente: **[nao verificado]**. As referências sustentam sua interpretação pelo código. Uso estas abreviações, com equivalência literal `sigla:n = arquivo:linha`:

| Sigla | Arquivo |
|---|---|
| M | `app/chamados/services/materializacao.py` |
| MD | `app/chamados/models.py` |
| J | `app/chamados/juizes.py` |
| V | `app/chamados/services/validacao.py` |
| BF | `app/chamados/management/commands/backfill_decisoes_pergunta.py` |
| BP | `app/chamados/management/commands/bug_par_103.py` |
| U | `app/chamados/services/universo_bug_par.py` |

**SCRIPT 1 — lacunas e pré-backfill, seção 4.**

| Rótulo | Valor | O que responde |
|---|---:|---|
| `integral_linhas_resposta` | 16941 | **Dimensiona** lacunas RESPOSTA por correspondência de pergunta/ato/instante; não mede divergência de texto. (`MD:4094`, `MD:4098`) |
| `integral_linhas_resposta_em_validadas_nao_materializadas` | 96 | **Confirma** lacuna RESPOSTA também nesse recorte operacional. (`MD:4098`, `M:2063`) |
| `integral_linhas_validacao` | 15115 | **Dimensiona** lacunas VALIDACAO por correspondência de pergunta/ato/instante. (`MD:4102`) |
| `integral_linhas_validacao_em_validadas_nao_materializadas` | 96 | **Confirma** lacuna VALIDACAO nesse recorte; não somar às 96 anteriores como perguntas distintas. (`MD:4098`, `MD:4102`) |
| `integral_perguntas_examinadas` | 50447 | **Dimensiona** o denominador informado; seleção integral exata da sonda [nao verificado]. |
| `recorte_oficial_linhas_resposta` | 2992 | **Dimensiona** o backfill oficial, cujo recorte inclui horizonte, sem dia e conversa; difere do integral. (`BF:55`, `BF:59`, `BF:67`) |
| `recorte_oficial_linhas_validacao` | 2248 | **Dimensiona** a mesma pendência oficial para VALIDACAO. (`BF:59`, `BF:67`, `MD:4102`) |
| `recorte_oficial_nao_entregue_resposta` | 2992 | **Dimensiona:** igual ao total oficial RESPOSTA; não demonstra lacuna entregue nesse recorte. Critério exato dessa classificação [nao verificado]. |
| `recorte_oficial_nao_entregue_validacao` | 2248 | **Dimensiona:** igual ao total oficial VALIDACAO; mesma limitação. Critério exato [nao verificado]. |

**O pré-backfill não está satisfeito como “zero lacunas”.** Tampouco cabe usar o integral como quantidade autorizada do backfill oficial: o comando tem recorte próprio. (`MD:4081`, `BF:55`; valores [nao verificado])

**SCRIPT 2 — contador, seção 4 e OPUS-C4-B da seção 5.**

| Rótulo | Valor | O que responde |
|---|---:|---|
| `contador_aplicaveis` | 40 | **Dimensiona** perguntas cuja testemunha diz `aplicavel`; não prova validação ou plantio. (`M:340`) |
| `contador_bug_par_103` | 19 | **Dimensiona** carimbadas elegíveis ainda devolvíveis, com competência não entregue. (`M:326`, `M:330`, `M:336`) |
| `contador_bug_par_103_pauta_dp` | 84 | **Dimensiona** carimbadas elegíveis separadas por competência entregue. (`M:333`) |
| `contador_bug_par_103_sem_dia` | 0 | **Dimensiona** ausência de exemplar nesse balde; não derruba o RED estrutural sem dia. (`M:331`, `M:795`) |
| `contador_congelados_no_recorte` | 163 | **Confirma** presença do universo congelado no recorte; pertencimento vem do arquivo fechado. (`M:321`, `M:356`, `U:30`) |
| `contador_delta_conservacao` | 0 | **Confirma** conservação aritmética: `79 + 0 + 84 + 0 = 163`. (`M:316`) |
| `contador_espera_declarado` | 9 | **Dimensiona** a espera declarado×gravado, que impede tratar todo pendente como plantável. (`M:344`, `M:741`) |
| `contador_legitimas` | 10 | **Dimensiona** testemunhas `bloqueada_paridade`. (`M:342`) |
| `contador_n_aplicaveis` | 40 | **Confirma** o mesmo tamanho de `contador_aplicaveis`; não são outras 40. (`M:352`, `M:353`) |
| `contador_outras` | 1 | **Dimensiona** outro estado da testemunha; estado individual [nao verificado]. (`M:347`) |
| `contador_pendentes_aval` | 79 | **Confirma** `60 + 19`; mede espera do admin, não promessa de plantio. (`M:314`, `M:356`) |
| `contador_resolvidas` | 0 | **Dimensiona** o estado atual; não demonstra a transição exigida pelo OPUS-C4-B. (`M:327`) |
| `contador_universo` | 60 | **Confirma** a partição corrente `40 + 10 + 9 + 1`; não é o total congelado U163. (`M:351`) |

O OPUS-C4-B **continua sem prova da transição pela autoridade real**: a fotografia acima não executa `pendentes 1→0 / resolvidas 0→1`. O teste existente verifica essa transição, mas sua fixture instala `competencia_entregue=False`. (`app/chamados/tests/test_bug_par_volta.py:463`, `app/chamados/tests/test_bug_par_volta.py:468`, `app/chamados/tests/test_bug_par_projetado.py:51`)

**SCRIPT 2 — lista textual, seção 4 e RED da lista na seção 5.**

| Rótulo | Valor | O que responde |
|---|---:|---|
| `pauta_comando_linhas` | 1178 | **Dimensiona** a saída textual; não são 1178 U163 nem Pautas criadas. Distinção entre itens e linhas físicas da extração [nao verificado]. (`BP:85`, `BP:89`) |
| `pauta_comando_u163_distintas` | 84 | **Dimensiona** presença U163 na saída; igualdade numérica com o balde 84 não comprova identidade individual. (`BP:53`, `BP:62`) |
| `pauta_comando_u60_distintas` | 0 | **Confirma** a omissão atual de U60: primeiro percurso U103; segundo exclui U163. Sustenta a mordida da lista. (`BP:53`, `BP:72`) |

**SCRIPT 2 — U163 e montagem do RED, seções 4 e 5.**

| Rótulo | Valor | O que responde |
|---|---:|---|
| `u163_presentes` | 163 | **Confirma** presença informada do universo fechado `60 + 103`. (`U:20`, `U:30`) |
| `u163_sem_dia_do_fato` | 0 | **Dimensiona:** nenhum exemplar observado para L5; não elimina o defeito estrutural do pulo anterior à consulta do dia. (`M:795`, `M:806`, `M:809`) |
| `u163_competencia_entregue` | 85 | **Dimensiona** competência entregue pelo dia do fato; não significa 85 validáveis. (`M:776`, `M:786`) |
| `u163_entregue_estado_recusada_paridade` | 84 | **Dimensiona** recusas já carimbadas, devolvidas pela testemunha antes do derivador; não são candidatas atuais a plantio. (`M:862`, `M:868`) |
| `u163_entregue_estado_bloqueada_competencia_entregue` | 1 | **Confirma** bloqueio já existente na composição pergunta/irmã; não demonstra a falha procurada pelo novo RED. (`M:885`, `V:224`, `V:225`) |
| `u163_entregue_fm_do_fato_aberto` | 78 | **Dimensiona** FM aberto; sozinho não libera K6 nem as demais guardas. (`M:818`, `V:652`) |
| `u163_entregue_fm_do_fato_aprovado` | 7 | **Dimensiona** o restante da partição `78 + 7 = 85`; aprovação FM não substitui a autoridade K6. (`M:786`, `M:830`) |
| `u163_entregue_na_fila_real` | 1 | **Confirma** uma pergunta no filtro R/V/C do laço; pertencer à fila não equivale a poder plantar. (`M:2063`, `M:2084`) |
| `u163_entregue_fila_testemunha_aplicavel` | 0 | **Derruba** “a única entregue na fila hoje plantaria”; nenhuma aplicável é mostrada nesse recorte. (`M:885`, `M:2063`) |
| `u163_aplicavel_com_bloqueio_real` | 0 | **Dimensiona** a classe nominada; não fornece exemplar do candidato RED. Predicado completo da sonda [nao verificado]. (`V:225`) |
| `u163_aplicavel_sem_bloqueio_real` | 0 | **Dimensiona** a classe nominada; não fecha prova de plantio liberado. Predicado completo [nao verificado]. (`V:225`) |
| `u163_aplicavel_exige_confirmacao` | 0 | **Dimensiona** a classe nominada; não demonstra passagem pela confirmação. Predicado completo [nao verificado]. (`V:615`) |
| `u163_aplicavel_sem_implausivel` | 0 | **Dimensiona** a classe nominada; não demonstra passagem pelas demais guardas. Predicado completo [nao verificado]. (`V:615`) |

Os quatro últimos zeros **não significam “zero aplicáveis em toda U163”**: o contador informou 40. A relação exata entre esses filtros exige o corpo integral da sonda, ausente do texto fornecido. (`M:340`, `M:352`; filtros [nao verificado])

**SCRIPT 3 — L-130, reparo e absorção, seções 4 e 5.**

| Rótulo | Valor | O que responde |
|---|---:|---|
| `respondida_sem_texto` | 9626 | **Dimensiona** estoque com instante e texto vazio após trim. Não prova nova escrita ilegal nem que todo esse estoque deva voltar a pendente. (`J:1534`, `app/docs/LEIS.md:151`) |
| `texto_sem_carimbo` | 0 | **Derruba** hipótese de estoque observado do meio par inverso; não elimina sua guarda nem reabre a lei do reparo. (`J:1541`, `app/docs/CORTES.md:488`) |
| `absorcao_pares_retornados_pela_funcao_real` | 0 | **Dimensiona** ausência de pares retornados na execução fornecida; não prova inexistência histórica de absorção. (`M:1105`, `M:1109`) |
| `absorcao_perguntas_retornadas_pela_funcao_real` | 0 | **Dimensiona** ausência de perguntas retornadas pelo predicado atual, que exclui Batida viva. (`M:1116`, `M:1118`) |
| `absorcao_linhas_no_acervo` | 56 | **Confirma** acervo histórico informado de ABSORCAO; não equivale ao conjunto devolvível hoje. (`MD:4048`, `M:1105`) |
| `absorcao_pares_no_acervo` | 52 | **Dimensiona** pares históricos distintos informados; chave exata dessa agregação [nao verificado]. Não contradiz retorno atual zero. (`MD:4059`, `M:1109`) |

A diferença entre acervo e retorno atual **não revoga o corte**: M9b/M10 permanecem no carimbo nesta F4. (`app/docs/CORTES.md:488`)

**SCRIPT 4 — J1, seções 4 e 5.**

| Rótulo | Valor | O que responde |
|---|---:|---|
| `j1_hora_examinadas` | 7240 | **Dimensiona** o recorte `hora` comparado; seleção exata [nao verificado]. (`J:376`, `M:599`) |
| `j1_hora_divergentes` | 0 | **Confirma** paridade observada nesse recorte; derruba divergência J1 observada nele. O juiz chama o mesmo derivador. (`J:376`) |
| `j1_hora_indeterminadas` | 0 | **Confirma** ausência de indeterminação reportada nessa comparação; não certifica plantio ou fechamento. (`J:378`, `M:2084`) |

Esses números não cobrem o parse próprio de `data_hora`, nem demonstram execução das duas metades ou C6. (`J:363`, `M:214`, `M:222`, `M:2086`)

**ERROR — causa e número faltante.**

| Script | Causa em uma linha | O que ficou sem número |
|---|---|---|
| 2 | `marcos_faltantes` e ATA discordam sobre E/S; o código registra o diagnóstico e retorna `t1 or t2`. (`M:568`, `M:571`, `M:576`) | Nenhuma perda demonstrada; os contadores finais foram fornecidos. Completude contra o roteiro integral [nao verificado]. |
| 4 | Mesmo diagnóstico durante a derivação; isso difere de divergência juiz×materializador. (`M:568`, `J:376`) | Os três números J1 foram fornecidos; outros números previstos [nao verificado]. |

`NoneType: None` é compatível com `exc_info=True` fora de um `except`; não é, por si, traceback de aborto. O dado termina com `rc_sombra=0` [nao verificado]. (`app/core/observ.py:79`, `M:571`)

**Contagem mantida: lei 0 nova / RED que morde 3.** Os zeros não retiram os casos estruturais U163 entregue e U163 sem dia; a omissão U60 sustenta o caso da lista. O candidato produtivo que atravessaria todas as guardas continua sem demonstração, e o OPUS-C4-B permanece lacuna de prova. (`app/docs/RELATO.md:17`, `app/docs/RELATO.md:8`, `M:795`, `BP:72`, `app/chamados/tests/test_bug_par_projetado.py:51`; execução dos RED [nao verificado])

## 17/11 01:25 · sessao 233baf0da3f5 · - · - · origem=Ronald (tmux)

Referências atuais para SCRIPT 2, todas somente lidas:

| Número recebido | Interpretação e efeito |
|---|---|
| `contador_aplicaveis=40` | Comprimento da lista de testemunhas `aplicavel`; dimensiona o recorte, sem provar ato de validação ou plantio. `app/chamados/services/materializacao.py:340` |
| `contador_n_aplicaveis=40` | Mesmo número anterior, exposto como escalar; não somar outra vez. `app/chamados/services/materializacao.py:352` |
| `contador_legitimas=10` | Dez testemunhas `bloqueada_paridade`; dimensiona impedimentos atuais. `app/chamados/services/materializacao.py:342` |
| `contador_espera_declarado=9` | Nove testemunhas `espera_declarado_x_gravado`; dimensiona guarda que antecede validação. `app/chamados/services/materializacao.py:344`, `app/chamados/services/validacao.py:611` |
| `contador_outras=1` | Uma testemunha em outro estado; estado individual não mostrado pelo dado. `app/chamados/services/materializacao.py:347`; identidade/estado exato [nao verificado] |
| `contador_universo=60` | Soma `40+10+9+1`; universo corrente sem carimbo no recorte, não total U163. `app/chamados/services/materializacao.py:351` |
| `contador_bug_par_103=19` | Carimbadas U103 ainda elegíveis e cuja competência do fato não foi entregue. `app/chamados/services/materializacao.py:326`, `app/chamados/services/materializacao.py:336` |
| `contador_bug_par_103_pauta_dp=84` | Carimbadas elegíveis de competência entregue, separadas para Pauta DP. `app/chamados/services/materializacao.py:333` |
| `contador_bug_par_103_sem_dia=0` | Nenhuma carimbada elegível sem dia do fato nesse balde; não elimina RED estrutural sem dia. `app/chamados/services/materializacao.py:331`, `app/chamados/services/materializacao.py:795` |
| `contador_pendentes_aval=79` | `60+19`; aguardam admin. Não é 163, nem promessa de que todas plantariam hoje. `app/chamados/services/materializacao.py:356` |
| `contador_resolvidas=0` | Nenhuma carimbada saiu da condição de caso no recorte. Não demonstra transição do OPUS-C4-B. `app/chamados/services/materializacao.py:327` |
| `contador_congelados_no_recorte=163` | Pertencimento presente no recorte; U60 e U103 vêm de arquivo fechado, disjuntos, união U163. `app/chamados/services/materializacao.py:356`, `app/chamados/services/universo_bug_par.py:20`, `app/chamados/services/universo_bug_par.py:30` |
| `contador_delta_conservacao=0` | Confirma conservação aritmética: `79+0+84+0=163`. Fórmula documental: `app/chamados/services/materializacao.py:316` |
| `pauta_comando_linhas=1178` | Volume recebido da saída textual; código da sonda não disponível para conferir se conta itens ou linhas físicas [nao verificado]. Comando publica `pauta` em `app/chamados/management/commands/bug_par_103.py:85` |
| `pauta_comando_u163_distintas=84` | Dimensiona presença U163 na lista; corresponde numericamente às 84 U103 entregues. Identidade individual dessa correspondência [nao verificado]. Seleção U103: `app/chamados/management/commands/bug_par_103.py:53`, inclusão: `app/chamados/management/commands/bug_par_103.py:62` |
| `pauta_comando_u60_distintas=0` | Confirma omissão atual de U60 nessa saída: primeiro percurso U103, segundo exclui U163. `app/chamados/management/commands/bug_par_103.py:53`, `app/chamados/management/commands/bug_par_103.py:72` |
| `u163_presentes=163` | Todos os membros presentes segundo dado; cardinalidade fechada do arquivo confere com 60+103. `app/chamados/services/universo_bug_par.py:20`, `app/chamados/services/universo_bug_par.py:30` |
| `u163_sem_dia_do_fato=0` | Sem exemplar atual para RED L5; não derruba o pulo estrutural antes de consultar dia. `app/chamados/services/materializacao.py:795`, `app/chamados/services/materializacao.py:806` |
| `u163_competencia_entregue=85` | Dimensiona autoridade K6 pelo fato; não implica 85 validáveis. Autoridade real: `app/chamados/services/materializacao.py:776`, `app/chamados/services/materializacao.py:786` |
| `u163_entregue_estado_recusada_paridade=84` | Testemunha retorna recusa pelo carimbo antes do derivador; não são 84 candidatas a plantio. `app/chamados/services/materializacao.py:862`, `app/chamados/services/materializacao.py:868` |
| `u163_entregue_estado_bloqueada_competencia_entregue=1` | Um caso já bloqueia hoje pela composição pergunta/irmã; não demonstra falha da extensão U163. `app/chamados/services/materializacao.py:885`, `app/chamados/services/validacao.py:224` |
| `u163_entregue_fm_do_fato_aberto=78` | Dimensiona FM; FM aberto sozinho não libera K6 nem demais guardas. `app/chamados/services/materializacao.py:818`, `app/chamados/services/validacao.py:652` |
| `u163_entregue_fm_do_fato_aprovado=7` | Completa `78+7=85`; não substitui entregue pela autoridade K6. `app/chamados/services/materializacao.py:786`, `app/chamados/services/materializacao.py:830` |
| `u163_entregue_na_fila_real=1` | Uma satisfaz filtro R/V/C do laço. Não equivale a testemunha aplicável. `app/chamados/services/materializacao.py:2063` |
| `u163_entregue_fila_testemunha_aplicavel=0` | Derruba a inferência “uma na fila ⇒ uma hoje plantaria”; não elimina RED estrutural. `app/chamados/services/materializacao.py:2063`, `app/chamados/services/materializacao.py:885` |
| `u163_aplicavel_com_bloqueio_real=0` | Nenhum candidato aplicável com bloqueio real mostrado; não prova ausência futura. Critério exato da sonda [nao verificado]; composição atual em `app/chamados/services/validacao.py:225` |
| `u163_aplicavel_sem_bloqueio_real=0` | Nenhum candidato aplicável liberado pela autoridade real mostrado; OPUS-C4-B continua sem prova do ato real. Critério da sonda [nao verificado]; teste existente valida transição em `app/chamados/tests/test_bug_par_volta.py:464` |
| `u163_aplicavel_exige_confirmacao=0` | Nenhum candidato aplicável nessa classe mostrado; não é prova de validação. Guarda: `app/chamados/services/validacao.py:615` |
| `u163_aplicavel_sem_implausivel=0` | Nenhum candidato aplicável sem implausibilidade mostrado; não fecha montagem do RED. Guarda: `app/chamados/services/validacao.py:615` |

O caso bloqueado entre U163 deve envolver pergunta fora163 na composição pelo código: `_k6_que_barra` ignora U163, e `entrega_que_barra_a_validacao` compõe pergunta/irmã. Sem ITEM, identidade e encadeamento individual continuam [nao verificado]. `app/chamados/services/materializacao.py:795`, `app/chamados/services/materializacao.py:805`, `app/chamados/services/validacao.py:224`.

OPUS-C4-B permanece lacuna de prova: teste existente verifica `pendentes 1→0`, `resolvidas 0→1`, congelado e autoridade conservados, mas fixture instala `competencia_entregue=False`. `app/chamados/tests/test_bug_par_volta.py:463`, `app/chamados/tests/test_bug_par_volta.py:468`, `app/chamados/tests/test_bug_par_volta.py:471`, `app/chamados/tests/test_bug_par_projetado.py:51`.

Contagem anterior está documentada: **lei 0 nova; RED que morde 3**, com três pendências U163 no topo. Estes números dimensionam as pendências e afastam candidato produtivo presumido; não resolvem abrangência da lista, destino de balde ou condição normativa do RED. `app/docs/RELATO.md:17`, `app/docs/RELATO.md:19`, `app/docs/RELATO.md:6`, `app/docs/RELATO.md:8`.

## 17/11 01:25 · sessao d8e751783447 · - · - · origem=Ronald (tmux)

- **Script 1:** lacunas são por correspondência `(pergunta, ato, instante)`, não ausência de qualquer ato nem divergência de texto; 96 respostas + 96 validações não significam 192 perguntas distintas. (`app/chamados/models.py:4094`, `app/chamados/models.py:4098`, `app/chamados/models.py:4102`)
- **Recorte oficial:** horizonte L-124, perguntas sem dia e conversas; difere do integral. O comando mede R/V e não as aplica. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:3`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:55`)
- **Script 3:** 9.626 `respondida_sem_texto` dimensionam o estoque do meio par ilegal; não provam escrita nova irregular. Zero `texto_sem_carimbo` elimina estoque observado desse segundo meio par, preservando o controle. (`app/chamados/juizes.py:1534`, `app/chamados/juizes.py:1541`)
- **Absorção:** função real seleciona carimbo, via, escape no grupo e ausência de Batida viva; acervo ABSORCAO guarda vínculos históricos. Portanto 0 pares/0 perguntas retornados coexistem com 56 linhas/52 pares históricos, sem revogar o corte que mantém o leitor por carimbo. (`app/chamados/services/materializacao.py:1105`, `app/chamados/services/materializacao.py:1109`, `app/chamados/services/materializacao.py:1116`, `app/chamados/models.py:4048`, `app/docs/CORTES.md:487`)
- **Script 4:** 7.240 examinadas, 0 divergentes e 0 indeterminadas confirmam paridade observada no recorte `hora`; não comprovam paridade `data_hora`, composição das duas metades, escrita ou C6. Juiz de hora chama o mesmo derivador; `data_hora` tem parse próprio. (`app/chamados/juizes.py:363`, `app/chamados/juizes.py:376`, `app/chamados/services/materializacao.py:599`)
- **ERROR dos scripts 2/4:** conflito `marcos_faltantes`×ATA é registrado, mas prevalece `t1` e a função retorna normalmente; não é divergência entre juiz e materializador. `NoneType: None` decorre do logger com `exc_info=True` chamado fora de `except`. (`app/chamados/services/materializacao.py:568`, `app/chamados/services/materializacao.py:571`, `app/chamados/services/materializacao.py:576`, `app/core/observ.py:79`)
- O dado fornecido termina com os números J1 e `rc_sombra=0`; não demonstra script abortado nem número previsto perdido. Como o texto dos scripts da sonda não acompanha o anexo truncado, completude dos rótulos previstos: **[nao verificado]**.

## 17/11 01:22 · sessao 1739fa0bd37f · REVISAO-AVAL-O10D1711 · verificador · origem=Code

1. **Premissas: conferem, com uma referência errada.** PREPARA fechado, perguntas de lei zero, d1=d2 permitido e cruzamento dependente da lista do construtor estão registrados (`app/docs/RELATO.md:147`; `app/docs/RELATO.md:1811`). O código confirma atomic por chamada, ordenação apenas do par e retorno `'recusada'` capturado, justificando rollback externo e ordenação da composição (`app/ponto/portas/celula.py:690`; `app/ponto/portas/celula.py:711`; `app/ponto/portas/celula.py:732`). Opus resolve as vedações ao Codex (`app/docs/LEIS.md:147`). **A O10-D está em BACKLOG:2198; :2199 é RETRATO-POR-EVENTO** (`app/docs/BACKLOG.md:2198`; `app/docs/BACKLOG.md:2199`).

2. **Cortes: preservar estas condições vigentes.** Suite de não-pouso lateral vai à PISTA; VM fica para espinha, pouso e push, com exceção por indisponibilidade registrada. O item 7 não dispensa esse roteamento (`app/docs/CORTES.md:490`; `app/docs/RELATO.md:102`). A F2 pode abrir após B2 sem esperar CLASSE-D (`app/docs/RELATO.md:4`). O brief registrado da CLASSE-D trata C5 por dia/dono; LU2-07 e LU2-08 conservam questões abertas, que esta obra não resolve (`app/docs/RELATO.md:421`; `app/docs/RELATO.md:354`; `app/docs/RELATO.md:352`; `app/docs/RELATO.md:422`). Leitura integral dos briefs físicos e cruzamento deles com a futura lista O10-D: **[nao verificado]**.

3. **Literal: as condições futuras ainda não estão demonstradas.** Lista, diff da B2 contra sua base, interseções, vaga e fila atual: **[nao verificado]**. O wrapper oferece titular e `flock`, sem interface de fila ou prioridade; ceder à espinha depende do despacho (`bin/trava_teste.sh:28`; `bin/trava_teste.sh:35`; `app/docs/RELATO.md:6`). d1=d2 exige caso efetivo: hoje as duas chamadas repetem coordenadas e escrevem trabalha/folga usando DNA do vínculo próprio; aceitar datas iguais sozinho não prova troca de turnos (`app/ponto/portas/celula.py:711`; `app/ponto/portas/celula.py:622`; `app/ponto/portas/celula.py:629`). Permanecem obrigatórios quatro controles antes da primeira escrita, recusa falante, ID próprio e escala-base intocada (`app/docs/BACKLOG.md:2198`).

4. **Dinheiro: o caminho alcança recálculo; alcance real fora do declarado não está provado.** A edição agenda fechamento no commit (`app/ponto/portas/celula.py:665`; `app/ponto/portas/celula.py:496`; `app/ponto/services/fechamento.py:1824`). A guarda barra lavrada, mas o TOCTOU permanece registrado (`app/ponto/portas/celula.py:606`; `app/docs/BACKLOG.md:2197`). Competência paga também conserva exigência de `!`, mesmo sem exportação (`CLAUDE.md:404`). Rollback SQL ainda não prova reversão integral de efeitos externos: há caminho de rejulgamento síncrono até notificação imediata (`app/ponto/portas/celula.py:472`; `app/ponto/services/cartorio.py:1791`; `app/ponto/management/commands/detectar_ausencias.py:779`). Valores alterados, exportada atingida ou notificação efetivamente enviada: **[nao verificado]**.

**VEREDITO: PROCEDE (item 2: corrigir BACKLOG:2199 para BACKLOG:2198; a linha citada pertence a outra obra — app/docs/BACKLOG.md:2199).**

## 17/11 01:21 · sessao aa74e1dc86c0 · PREPARA-F3-F5-F6-F8-F9 · leitura · origem=Code

## 1 DEPENDE e premissas

**A F1 pressuposta pela F3 caiu como contrato disponível.** A BASE fornece `pergunta_respondida(p)`, que exige instante **e** texto da projeção; destina à B2 os demais leitores. Isso não fornece a antiga `leitura(p) → LeituraDosAtos`. (`app/chamados/juizes.py:1544`, `app/chamados/juizes.py:1551`, `app/chamados/juizes.py:1552`)

Identificação recebida da cópia: `fbc5c76ced33bf25943f3a5bc78ebff257d68c56`. Confirmação independente do SHA do HEAD, ancestralidade e diffs históricos: **[nao verificado]**. Banco, logs, execução dos testes e números atuais de frota: **[nao verificado]**.

As referências `brief.md` e `matriz.md` abaixo numeram o texto fornecido, começando pelo título como linha 1; correspondência com os arquivos externos originais: **[nao verificado]**. **CAIU** significa que a premissa não pode ser reutilizada para orientar esta BASE; não desmente sua cópia histórica.

**Cada ocorrência literal de `[DEPENDE-F1]`:**

| Origem | Premissa | Julgamento na BASE |
|---|---|---|
| `brief.md:57` | Guarda sem resposta passa a `leitura().resposta` | **CAIU.** Juiz disponível exige o par projetado; guarda atual testa somente instante. (`app/chamados/juizes.py:1552`, `app/chamados/services/validacao.py:594`) |
| `brief.md:58` | Validação corrente vem de `leitura().instante_validacao` | **CAIU.** A porta continua lendo V; a interface pressuposta não é o juiz pousado. (`app/chamados/services/validacao.py:597`, `app/chamados/juizes.py:1544`) |
| `brief.md:59` | Recusar consulta validação/resposta da antiga leitura | **CAIU.** Guardas atuais consultam V/R projetados. (`app/chamados/services/validacao.py:810`, `app/chamados/services/validacao.py:813`) |
| `brief.md:66` | Ações recebem aquela leitura | **CAIU.** Reabrir, regularizar e corrigir continuam consultando campos. (`app/chamados/services/acoes_disputa.py:97`, `app/chamados/services/acoes_disputa.py:132`, `app/chamados/services/acoes_disputa.py:347`) |
| `brief.md:67` | Classe, aceite de folga e idade recebem resposta/instante do fold | **CAIU.** Continuam lendo T/R. (`app/chamados/services/validacao.py:380`, `app/chamados/services/validacao.py:1094`, `app/chamados/services/validacao.py:1167`) |
| `brief.md:85` | DP RESPOSTA com projeção vazia basta para validar | **CAIU como oráculo da F1 pousada.** O juiz atual exige R/T projetados. (`app/chamados/juizes.py:1552`) |
| `brief.md:87` | Ato presente torna projeção vazia equivalente à cheia | **CAIU como contrato disponível.** Classe e ações não recebem o fold antigo. (`app/chamados/services/validacao.py:1094`, `app/chamados/services/acoes_disputa.py:355`) |
| `brief.md:129` | Esperar a F1 do contrato `leitura` resolve a dependência | **CAIU.** O contrato pousado é outro; B2 permanece explicitamente encarregada dos leitores. (`app/chamados/juizes.py:1550`, `app/chamados/juizes.py:1551`) |
| `matriz.md:9` | `_leitura_conservada`, `_leitura_da_pergunta`, `_q_resposta_do_ato` são as autoridades atuais | **CAIU.** Projeção temporal e juiz de respondida são os contratos atuais. (`app/chamados/juizes.py:1382`, `app/chamados/juizes.py:1544`) |
| `matriz.md:16` | D1 recebe a antiga leitura | **CAIU.** (`app/chamados/services/validacao.py:594`, `app/chamados/juizes.py:1552`) |
| `matriz.md:21` | A F1 determina a conservação usada pela fixture | **CAIU a dependência da F1.** O helper de correspondência existe no model, independentemente daquela API. (`app/chamados/models.py:4081`, `app/chamados/models.py:4094`) |
| `matriz.md:24` | D2 recebe validação corrente do fold | **CAIU.** (`app/chamados/services/validacao.py:597`, `app/chamados/juizes.py:1544`) |
| `matriz.md:29` | D3 recebe validação corrente do fold | **CAIU.** (`app/chamados/services/validacao.py:810`, `app/chamados/juizes.py:1544`) |
| `matriz.md:33` | D4 recebe resposta corrente do fold | **CAIU.** (`app/chamados/services/validacao.py:813`, `app/chamados/juizes.py:1552`) |
| `matriz.md:44` | D7 pode migrar para aquele juiz de validação | **CAIU o suporte pressuposto.** Os dois leitores continuam projetivos. (`app/chamados/services/validacao.py:975`, `app/chamados/services/validacao.py:996`) |
| `matriz.md:49` | D8 recebe texto do fold | **CAIU.** (`app/chamados/services/validacao.py:1094`) |
| `matriz.md:53` | D9 recebe texto do fold | **CAIU.** (`app/chamados/services/validacao.py:380`) |
| `matriz.md:57` | D10 recebe instante do fold | **CAIU.** (`app/chamados/services/validacao.py:1167`) |
| `matriz.md:67` | Leituras internas da devolução recebem `motivo_recusa`/V do fold | **CAIU.** Há refresh e leitura dos campos persistidos. (`app/chamados/services/validacao.py:262`, `app/chamados/services/validacao.py:266`, `app/chamados/services/validacao.py:269`) |
| `matriz.md:101` | D16 recebe instante do fold | **CAIU.** (`app/chamados/services/acoes_disputa.py:97`) |
| `matriz.md:103` | D17 recebe instante do fold | **CAIU.** (`app/chamados/services/acoes_disputa.py:132`) |
| `matriz.md:106` | D18 recebe V/T do fold | **CAIU.** (`app/chamados/services/acoes_disputa.py:347`, `app/chamados/services/acoes_disputa.py:355`) |
| `matriz.md:128` | Contrato fechado D1–D4/D7/D18 dispõe de `atos_pergunta.leitura` | **CAIU.** O contrato atual não fornece esse redutor. (`app/chamados/juizes.py:1544`, `app/chamados/juizes.py:1551`) |
| `matriz.md:129` | Contrato fechado D8–D10/D16–D18 dispõe daquela leitura | **CAIU.** (`app/chamados/juizes.py:1544`, `app/chamados/services/validacao.py:1094`) |
| `matriz.md:141` | Os onze degraus estão liberados pela antiga F1 | **CAIU o suporte comum.** A enumeração histórica de onze degraus não demonstra disponibilidade da interface. (`app/chamados/juizes.py:1550`, `app/chamados/juizes.py:1551`) |

**Demais premissas dos anexos:**

| Origem | Premissa | Julgamento |
|---|---|---|
| `brief.md:10–15`; `matriz.md:3–5` | Offsets e ausência de mudanças nas cópias antigas podem ser transportados | **CAIU para esta BASE.** Validar está em `:574`, recusar em `:770`; ações ganharam linhas. Os diffs históricos permanecem **[nao verificado]**. (`app/chamados/services/validacao.py:574`, `app/chamados/services/validacao.py:770`, `app/chamados/services/acoes_disputa.py:92`) |
| `brief.md:15`; `matriz.md:16–113` | Os leitores centrais ainda leem projeção | **CONFERE.** (`app/chamados/services/validacao.py:594`, `app/chamados/services/verbos.py:264`, `app/chamados/services/painel.py:67`, `app/chamados/services/acoes_disputa.py:347`, `app/chamados/veredito_lavrado.py:207`) |
| `brief.md:16–18`; `matriz.md:37` | Recusa fora do fato já está curada | **CONFERE.** Retorna antes da escrita; casca traduz como informação. (`app/chamados/services/validacao.py:817`, `app/chamados/services/acoes_disputa.py:87`) |
| `brief.md:19–21`; `matriz.md:61,133` | A20 fica nos campos próprios | **CONFERE.** Isso não comprova implementação integral da LU-08, que limita a conferência ao par conferido: **[nao verificado]**. (`app/chamados/services/validacao.py:905`, `app/chamados/services/validacao.py:907`, `app/docs/CORTES.md:466`) |
| `brief.md:61` | F1 fornece materialização conservada por parâmetro | **CAIU.** A projeção temporal lê M atual; não recebe esse parâmetro. A permanência de M no escopo conservado continua expressa nos anexos. (`app/chamados/juizes.py:1382`, `app/chamados/juizes.py:1390`; `matriz.md:133`) |
| `brief.md:22,66`; `matriz.md:104,107,133` | POST e fechamento da disputa não são projeções da resposta | **CONFERE.** (`app/chamados/services/acoes_disputa.py:83`, `app/chamados/services/acoes_disputa.py:125`, `app/chamados/services/acoes_disputa.py:257`, `app/chamados/services/acoes_disputa.py:349`) |
| `brief.md:32–39` | A enumeração antiga esgota escritores | **CAIU.** Há lote, nascimento com `**campos`, ressuscitação e devolução BUG-PAR. (`app/chamados/models.py:1629`, `app/chamados/models.py:3124`, `app/chamados/models.py:3393`, `app/chamados/services/validacao.py:349`) |
| `brief.md:38` | `registrar` é a porta produtiva de escrita DP | **CONFERE no censo estático.** (`app/chamados/models.py:4110`, `app/chamados/models.py:4120`) |
| `brief.md:40–45`; `matriz.md:123–125` | `carimbar('validar')` pode escrever V sem VALIDACAO | **CONFERE.** O corpo atribui e salva sem registrar esse ato; absorção registra ABSORCAO. Frota: **[nao verificado]**. (`app/chamados/models.py:3245`, `app/chamados/models.py:3262`, `app/chamados/services/materializacao.py:1043`, `app/chamados/services/materializacao.py:1045`) |
| `brief.md:46` | Nenhum leitor F3 é sem janela | **CAIU como afirmação geral.** Marcador agrega o acervo; `marcos_do_lote(ate=None)` também. Isso não transforma esses leitores em contadores D04. (`app/chamados/services/painel.py:66`, `app/chamados/veredito_lavrado.py:163`) |
| `brief.md:98–101,129` | BUG-PAR ainda precisa pousar | **CAIU.** Seus corpos estão presentes, e há registro documental do pouso c4+c4c. (`app/chamados/services/validacao.py:308`, `app/chamados/services/validacao.py:526`, `app/docs/BACKLOG.md:2084`) |
| `brief.md:98–101` | BUG-PAR acrescenta espera, irmã, par, devolução e mensagem real | **CONFERE.** (`app/chamados/services/validacao.py:354`, `app/chamados/services/validacao.py:554`, `app/chamados/services/validacao.py:611`, `app/chamados/services/validacao.py:638`, `app/chamados/services/acoes_disputa.py:63`) |
| `brief.md:102` | DEVOLUCAO entra automaticamente em D04/D06 | **CAIU como descrição atual.** D04 pretendido conta **VALIDACAO**, não soma DEVOLUCAO como validação; os marcadores atuais nem consultam DP. (`app/docs/BACKLOG.md:2091`, `app/chamados/services/verbos.py:263`, `app/chamados/services/painel.py:66`, `app/chamados/veredito_lavrado.py:202`) |
| `brief.md:103–105` | Ausência de conflito das cópias F1/F2/E3/LOTE7/UI-CAMERA/RELAVRA serve como garantia atual | **CAIU como garantia reutilizável.** Diffs dessas raias: **[nao verificado]**; B2 fornecida cruza três arquivos centrais, conforme parte 6. (`app/chamados/services/validacao.py:594`, `app/chamados/services/acoes_disputa.py:97`, `app/chamados/services/verbos.py:335`) |
| `matriz.md:101` | `total_pend` conta apenas corrigíveis | **CAIU.** Conta todas as perguntas da disputa; corrigíveis são buscadas depois. (`app/chamados/services/acoes_disputa.py:97`, `app/chamados/services/acoes_disputa.py:99`) |
| `matriz.md:54,121` | Existem dois chamadores externos de `_e_aceite_de_folga` nos offsets antigos | **CAIU.** O sítio atual importa em `:247` e chama em `:259`. (`app/chamados/services/vigia_fio.py:247`, `app/chamados/services/vigia_fio.py:259`) |
| `matriz.md:19–21,79–81` | As fixtures indicadas não escrevem DP explicitamente | **CONFERE.** Usam `objects.create`; o `save` não chama o conservador. D12 também fornece texto, não somente instante. `_nascer`, outra porta, registra conservação explicitamente. (`app/chamados/tests/test_service_validar_puro.py:47`, `app/chamados/tests/test_placar_aceite.py:35`, `app/chamados/models.py:3550`, `app/chamados/models.py:3129`) |
| `matriz.md:139–141` | Os quinze NENHUM continuam sendo o censo de provas | **CAIU.** Foram encontradas provas adicionais, discriminadas na parte 3. (`app/chamados/tests/test_validar_lote.py:131`, `app/chamados/tests/test_fila_validar_lote.py:42`, `app/chamados/tests/test_guard_s116_detalhe_local.py:52`, `app/escala/tests/test_signal_ancora.py:218`) |

A contagem seguinte considera **as 25 ocorrências literais da marca**, incluindo repetições contratuais e resumo; as demais premissas foram julgadas separadamente nas linhas acima. (`brief.md:57,58,59,66,67,85,87,129`; `matriz.md:9,16,21,24,29,33,44,49,53,57,67,101,103,106,128,129,141`)

DEPENDE CAIU 25 de 25

## 2 Censo escritores e leitores

Censo estático por grep/AST, incluindo atributos, aliases, keywords e strings ORM; templates foram varridos textualmente. Completude para nomes inteiramente construídos em execução: **[nao verificado]**. **NOVO** significa ausente da enumeração do brief, sem afirmar nascimento recente.

Nesta seção, caminhos abreviados são relativos a `app/`. R=`respondida_em`; T=`resposta_colab`; V=`validada_em`; VP=`validada_por[/_id]`; C=`materializada_em`; M=`recusa_motivo`; DC=`data_conferida_*`.

**Escritores físicos e simulações:**

| Campo | Escritores encontrados |
|---|---|
| R/T | Resposta manual `chamados/services/disputa_emissao.py:1481,1482,1483`; automática `:1674,1675,1677`; reparo da trilha, somente R, `:1696,1699`; reabertura via lote `:1609,1648`. |
| R/T — **NOVO** | Ressuscitação `chamados/models.py:1629,1630`; nascimento por `**campos`, `:3124,3127`. |
| V/VP | Validação `chamados/services/validacao.py:168,169,171`; devolução `:273,274,276`; **NOVO BUG-PAR** `:349,350,351`; nova resposta limpa V/VP em `chamados/services/disputa_emissao.py:1494,1495`; reabertura em lote `:1609,1648`; ressuscitação `chamados/models.py:1629`; carimbo `:3245,3246`; nascimento `:3124`. |
| C | `_escrever_carimbo`, `chamados/models.py:3209`; persistências individuais `:3262,3299,3322`; **NOVO** lote direto `:3391,3393`; nascimento `:3124`. |
| M — **NOVO no censo** | Escape humano `chamados/services/disputa_emissao.py:1511,1513`; motivo técnico `chamados/services/materializacao.py:1332,1333`; recusa terminal `:1665,1666`; identidade `chamados/models.py:3298,3299`; recusa administrativa `chamados/services/validacao.py:827,833`; nascimento `chamados/models.py:3124`. |
| DC | Conferência `chamados/services/validacao.py:907,908,909`. |
| Memória, sem persistência por essas atribuições | Simulação da ressuscitação `chamados/models.py:1554,1555,1556`; rollback do carimbo `:3273,3274,3275`. |
| Disputa: fechamento/resolução | Fechar `chamados/models.py:2013,2015`; reabrir admin `:2230,2232`; reabrir sistema `:2254,2256`; complemento de resolução `:2346`. |
| DP | Porta produtiva `chamados/models.py:4110`, com `bulk_create` interno em `:4120`; alteração de linha existente é recusada em `:4070`. |

`regularizacao_ext.py:199` escreve C de **outro modelo**, `RegularizacaoExterna`; não é escritora de C da pergunta. (`chamados/services/regularizacao_ext.py:199`, `chamados/models.py:4152`)

**Produtores e leitores de DP:**

| Papel | Sítios |
|---|---|
| Produtores já nomeados | `chamados/models.py:1613,2005,2137,3129`; `chamados/services/disputa_emissao.py:1525,1631,1678,1699`; `chamados/services/validacao.py:173,279`; `chamados/services/materializacao.py:1045`. |
| Produtores **NOVOS** | `chamados/services/validacao.py:354`; `chamados/management/commands/backfill_fio_morto_e4b.py:141`; `chamados/management/commands/backfill_decisoes_pergunta.py:324`; `ponto/services/veredito_celula.py:195`. |
| Leitores | Histórico `chamados/models.py:3675`; correspondência `:4094`; **NOVOS** rótulo BUG-PAR `chamados/services/materializacao.py:275`, prefetch `chamados/services/fio.py:263`, tarefa `chamados/services/tarefa_do_dia.py:158,159,178,421,430`, foto `chamados/services/foto_e4b.py:280`, medidor `chamados/management/commands/backfill_decisoes_pergunta.py:99,315`, histórico HTML `templates/chamados/partials/modal_fio.html:205,210,212`. |

As chamadas ao conservador podem registrar assinaturas anteriores; não representam necessariamente um gesto humano novo. A correspondência é pergunta + espécie de ato + instante. (`chamados/models.py:4087`, `chamados/models.py:4094`, `chamados/models.py:4098`, `chamados/models.py:4102`)

**Leitores nos cinco arquivos centrais:**

| Arquivo/função | Leituras atuais |
|---|---|
| `validacao.py` — guardas | R `chamados/services/validacao.py:594,813`; V `:597,810`; C `:681,717`. |
| **NOVO** fotografia anterior | V/VP/M e fechamento da disputa em `chamados/services/validacao.py:181,182`; consumidores em `:268,294,627`. |
| Devolução recusada | Refresh dos campos `chamados/services/validacao.py:262`; C `:264`; M `:266`; V `:269`; T/M na DP `:281,282`; fechamento `:288,291,293`; fotos/trilha `:300,301,302`. |
| **NOVO** devolução BUG-PAR | C/VP/V/M `chamados/services/validacao.py:341,342,343,344`; T `:356`; valores posteriores `:363,364`. |
| **NOVO** par | V da irmã determina se haverá nova VALIDACAO em `chamados/services/validacao.py:553,554`; count de duas C em `:556`. |
| Aceite de folga | T `chamados/services/validacao.py:380`; chamador interno `:623`. |
| **NOVO no brief** lote | V em `chamados/services/validacao.py:975,996`. |
| Classe | T `chamados/services/validacao.py:1094,1096,1098,1103,1104,1105,1106,1109`. |
| Fila | R para idade `chamados/services/validacao.py:1167`. |
| Recusa: foto/motivo | T/R/M `chamados/services/validacao.py:823,824,825,843,847,848`. |
| Conferência | DC `chamados/services/validacao.py:905,907,908,909`. |
| `acoes_disputa.py` | R `chamados/services/acoes_disputa.py:97,132`; V `:347`; T `:355`; fechamento da disputa `:257,349`. POST M/resolução em `:83,125` são homônimos. |
| `verbos.py` | V/VP `chamados/services/verbos.py:264,265,266,268,270`; R `:335,336,342,345`. |
| `painel.py` | R/V/C no agregado `chamados/services/painel.py:67`; VP `:262`; V nas janelas `:270,271,272,273`; **NOVO** transporte das janelas pelo modo resumo `:530,535`. |
| `veredito_lavrado.py` | C `chamados/veredito_lavrado.py:203,204`; R `:207,208`. |

**Chamadores das funções alcançadas:**

| Função | Chamadores produtivos |
|---|---|
| `validar_pergunta` | `chamados/services/acoes_disputa.py:19,381`; **NOVOS** `api/foto_recebida.py:51`, `chamados/services/declaracao_texto.py:696`, `chamados/management/commands/validar_classe_a.py:100`. |
| `recusar_pergunta` | `chamados/services/acoes_disputa.py:82`. |
| `_carimbar_validacao` | `chamados/services/validacao.py:432,464,554,655`. |
| `devolver_validacao_recusada` | `chamados/services/validacao.py:693`. |
| **NOVO** `devolver_paridade_bug_par_103` | `chamados/management/commands/bug_par_103.py:106`. |
| `_e_aceite_de_folga` | `chamados/services/validacao.py:623`; **NOVO** `chamados/services/vigia_fio.py:259`. |
| **NOVO** `validar_lote_classe_a` | `chamados/views_cobrar.py:261,381`; `core/views_ia.py:169`. |
| `classe_do_lote` | `chamados/services/validacao.py:978,1165`; **NOVO** `api/views_mensageria.py:545`. |
| **NOVO** `fila_validar_classificada` | `chamados/views_cobrar.py:288,349,361`. |
| **NOVO** `conferir_data_do_fato` | `chamados/views.py:1312`. |
| `placar_equipe` | `chamados/views.py:1486`. |
| `placar_aceite` | `chamados/services/painel.py:298`; `chamados/views.py:1488`; `folha/views_retrato.py:515`. |
| `marcador_do_placar` / `etag_do_placar` | `chamados/services/painel.py:91`; `chamados/views.py:820`. |
| `montar_contexto_painel` | `chamados/views.py:727,852`; `core/management/commands/lavrar_badge_navbar.py:39`; `chamados/management/commands/backfill_fio_morto_e4b.py:72`. |
| `marcos_do_lote` | `chamados/services/raiox.py:38`; alias no fio `chamados/services/fio.py:281,282`; `chamados/services/tarefa_do_dia.py:434`; `chamados/management/commands/auditar_invariantes_chamados.py:1163`. |
| `acao_validar_pergunta` | `chamados/views.py:501`; `chamados/services/fio.py:655`; `chamados/views_cobrar.py:208`; `chamados/services/validacao.py:989`. |
| `acao_recusar_pergunta` | `chamados/views.py:518`; `chamados/services/fio.py:684`. |
| `acao_reabrir_disputa` | `chamados/services/fio.py:690`. |
| `acao_regularizar_disputa` | `chamados/views.py:475`. |
| `acao_corrigir_resposta` | `chamados/views.py:536`; `chamados/services/fio.py:685`. |

**Censo global adicional dos campos — NOVO na enumeração do brief.** Inclui leitura, filtro, fotografia e transporte; uma ocorrência não significa, por si, julgamento de estado.

| Arquivo | Campos e sítios |
|---|---|
| `api/views.py` | T `:1163,2561`; R `:1165,2563,2577,2620,2637`. |
| `api/views_mensageria.py` | T `:567`; R `:569,570`. |
| `chamados/canal.py` | T `:65`. |
| `chamados/juizes.py` | C `:292,683,711,858,1390,1502`; R `:352,856,1389,1423,1534,1541,1552,1572,1580,1744,1753`; T `:364,857,1360,1389,1423,1534,1541,1552,1572,1580,1779,1926,2030,2035,2052`; V `:856,1389`. |
| `chamados/models.py` | R `:1614,1924,2949,3155,3158,3237,3241,3243,3331,3370,3375,3376,3380,3382,3568,3584,3585,3665,4091,4098,4099,4100`; T 

[truncado em 20000 de 64284 caracteres]

## 17/11 01:21 · sessao 5534e82872ea · PREPARA-F3-F5-F6-F8-F9 · leitura · origem=Code

## 1 DEPENDE e premissas

**A premissa central caiu:** a F1 disponível declara `pergunta_respondida(p)`, baseada em instante **e** texto da projeção. Ela não fornece o contrato `LeituraDosAtos` pressuposto pelos anexos; o próprio código destina a migração dos leitores B/C/D à B2. (`app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1551`; `app/chamados/juizes.py:1552`)

BASE identificada pelo despacho: `fbc5c76ced33bf25943f3a5bc78ebff257d68c56`. Confirmação independente do HEAD, ancestralidade e diffs históricos: **[nao verificado]**. Resultados de banco, sondas e testes nesta entrega: **[nao verificado]**.

As referências `brief:N` e `matriz:N` contam as linhas dos anexos fornecidos, incluindo vazias, começando no título. A contagem abaixo considera **cada ocorrência literal de `[DEPENDE-...]`**; premissas sem marca são julgadas separadamente. Correspondência com os arquivos originais externos: **[nao verificado]**.

| Marca no anexo | Premissa julgada | Veredito e prova na BASE |
|---|---|---|
| brief:15 — U1 | F1 fornece texto/instante pelo ato | **CAIU.** A autoridade pousada julga o par projetado; serializer ainda lê os campos. (`app/chamados/juizes.py:1552`; `app/api/views_mensageria.py:567`; `app/api/views_mensageria.py:569`) |
| brief:16 — U2 | Mesmo contrato para chamados/v2 | **CAIU.** (`app/chamados/juizes.py:1544`; `app/api/views.py:1163`; `app/api/views.py:1165`) |
| brief:17 — U3 | Mesmo contrato para disputa/perguntas | **CAIU.** (`app/chamados/juizes.py:1544`; `app/api/views.py:2561`; `app/api/views.py:2563`) |
| brief:19 — U5 | Filtro passa a usar o antigo redutor | **CAIU.** Juiz vigente exige o par; filtro atual exige somente instante. (`app/chamados/juizes.py:1552`; `app/api/views.py:2577`) |
| brief:20 — U6 | `ja_respondida` usa o antigo redutor | **CAIU.** (`app/chamados/juizes.py:1552`; `app/api/views.py:2620`) |
| brief:22 — U8 | Amostra usa texto do ato fornecido pela F1 | **CAIU.** (`app/chamados/juizes.py:1544`; `app/chamados/views_cobrar.py:291`) |
| brief:23 — U9 | Resposta web usa o antigo redutor | **CAIU.** (`app/chamados/juizes.py:1552`; `app/chamados/views.py:1280`) |
| brief:25 — U11 | Face SQL conserva P5 pelo ato | **CAIU.** A face vigente exige instante e texto não vazio. (`app/chamados/juizes.py:1575`; `app/chamados/juizes.py:1580`) |
| brief:26 — U12 | Tag recebe leitura do ato da F1 | **CAIU.** Tag ainda testa texto projetado e pede classificação. (`app/chamados/templatetags/chamados_tags.py:166`; `app/chamados/templatetags/chamados_tags.py:171`) |
| brief:66 | `L = atos_pergunta.leitura` é autoridade disponível | **CAIU.** Contrato disponível: `pergunta_respondida`. (`app/chamados/juizes.py:1544`) |
| brief:132 | D01 é leitor mecânico sem registro de juiz | **CAIU.** O juiz de respondida está registrado. (`app/core/juizes.py:170`; `app/core/juizes.py:306`) |
| matriz:8 | F1 vigente é a interface de `be2b8d733` | **CAIU como contrato da BASE.** (`app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1551`) |
| matriz:9 — `[DEPENDE-R3]` | Ainda é necessário aguardar o pouso da R3 | **CAIU.** Há registro de R3 c17 no ar; ancestralidade do hash antigo: **[nao verificado]**. (`app/docs/BACKLOG.md:770`; `app/docs/RELATO.md:243`) |
| matriz:9 — `[DEPENDE-F2]` | Continua dependendo dos leitores de estado/display da F2 | **CONFERE como dependência de interface.** Estado/display continuam projetivos; presença histórica de `8c6577245`: **[nao verificado]**. (`app/chamados/models.py:3151`; `app/chamados/models.py:3688`) |
| matriz:32 — U1a | Texto por `L.resposta` | **CAIU.** (`app/chamados/juizes.py:1544`; `app/api/views_mensageria.py:567`) |
| matriz:33 — U1b, primeira marca | Instante por `L.instante_resposta` | **CAIU.** (`app/chamados/juizes.py:1544`; `app/api/views_mensageria.py:569`) |
| matriz:33 — U1b, segunda marca | P5 entra na fila F1 e derruba o lote | **CAIU hoje.** A fila exige instante não nulo. (`app/chamados/juizes.py:1751`; `app/chamados/juizes.py:1580`) |
| matriz:34 — U1c | P5 chega à subtração pela fila F1 | **CAIU hoje.** A subtração continua insegura para chamada direta com `None`. (`app/chamados/juizes.py:1751`; `app/api/views_mensageria.py:570`) |
| matriz:35 — U2, `[DEPENDE-F1]` | Texto/instante pelo antigo redutor | **CAIU.** (`app/chamados/juizes.py:1544`; `app/api/views.py:1163`; `app/api/views.py:1165`) |
| matriz:35 — U2, `[DEPENDE-F2]` | `resposta_display` permanece interface da F2 | **CONFERE.** A API consome essa property, ainda projetiva. (`app/api/views.py:1164`; `app/chamados/models.py:3688`) |
| matriz:36 — U3 | Texto/instante pelo antigo redutor | **CAIU.** (`app/chamados/juizes.py:1544`; `app/api/views.py:2561`; `app/api/views.py:2563`) |
| matriz:37 — U4 | Migração de T depende daquela F1 | **CAIU o vínculo com o contrato antigo; CONFERE T como autoridade existente.** (`app/chamados/juizes.py:1544`; `app/chamados/services/tarefa_do_dia.py:142`; `app/docs/CORTES.md:488`) |
| matriz:38 — U5, primeira marca | Respondidas por `L` | **CAIU.** (`app/chamados/juizes.py:1552`; `app/api/views.py:2577`) |
| matriz:38 — U5, segunda marca | P5 pode contar como respondida conservada | **CAIU.** Texto sem instante é par ilegal e não é respondida. (`app/chamados/juizes.py:1541`; `app/chamados/juizes.py:1552`; `app/docs/LEIS.md:151`) |
| matriz:39 — U6 | `ja_respondida` por `L.instante_resposta/L.resposta` | **CAIU.** A pergunta “respondida?” tem juiz único atual. (`app/chamados/juizes.py:1544`; `app/api/views.py:2620`) |
| matriz:40 — U7a, primeira marca | Seleção depende daquela interface F1 | **CAIU.** O juiz atual está disponível; o filtro segue só por instante. (`app/chamados/juizes.py:1552`; `app/chamados/views_cobrar.py:169`) |
| matriz:40 — U7a, segunda marca | P5 é resposta elegível pela F1 | **CAIU.** (`app/chamados/juizes.py:1541`; `app/chamados/juizes.py:1552`) |
| matriz:41 — U7b, primeira marca | T depende daquela interface F1 | **CAIU o vínculo antigo; CONFERE T existente.** (`app/chamados/services/tarefa_do_dia.py:142`; `app/chamados/juizes.py:1544`) |
| matriz:41 — U7b, segunda marca | Alteração do contador é consequência daquela F1 | **CAIU essa causa.** O universo cru, incluindo mortas, continua demonstrado no próprio filtro. (`app/chamados/views_cobrar.py:175`; `app/chamados/views_cobrar.py:176`) |
| matriz:42 — U8 | Texto pelo antigo redutor | **CAIU.** (`app/chamados/juizes.py:1544`; `app/chamados/views_cobrar.py:291`) |
| matriz:43 — U9a | Bool pelo antigo redutor | **CAIU.** (`app/chamados/juizes.py:1552`; `app/chamados/views.py:1280`) |
| matriz:45 — U10, primeira marca | Corrigíveis por `L` da F1 antiga | **CAIU.** Corrigíveis ainda exige instante não nulo. (`app/chamados/juizes.py:1744`) |
| matriz:45 — U10, segunda marca | P5 entra em corrigíveis e troca o escritor | **CAIU hoje.** O filtro atual exclui P5. (`app/chamados/juizes.py:1744`; `app/chamados/views.py:1420`) |
| matriz:46 — U11 | Vazio por `L.resposta` | **CAIU.** (`app/chamados/juizes.py:1544`; `app/chamados/canal.py:65`) |
| matriz:47 — U12 | Tag pelo antigo redutor | **CAIU.** (`app/chamados/juizes.py:1544`; `app/chamados/templatetags/chamados_tags.py:166`) |
| matriz:57 | Impactos P5 só podem ser medidos depois daquela F1 | **CAIU.** Os predicados atuais e as filas reais permitem medir P5 agora. (`app/chamados/juizes.py:1537`; `app/chamados/juizes.py:1742`; `app/chamados/juizes.py:1749`) |
| matriz:64 | Contrato de saída inteiro usa leitura dos atos F1 | **CAIU como interface disponível.** (`app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1551`) |
| matriz:68 | RED P5/`None` decorre do universo F1 | **CAIU essa entrada pela fila atual.** (`app/chamados/juizes.py:1580`; `app/chamados/juizes.py:1751`) |
| matriz:87 | P5 é resposta conservada válida para lista/correção | **CAIU quanto a “respondida”.** (`app/docs/LEIS.md:151`; `app/chamados/juizes.py:1552`) |
| matriz:98 | 500 no lote atual por P5 | **CAIU.** Serializer inseguro permanece, mas a fila exclui a entrada nula. (`app/api/views_mensageria.py:569`; `app/chamados/juizes.py:1751`) |

**Premissas sem marca:**

| Origem | Julgamento |
|---|---|
| brief:3; matriz:5–7 — bases e offsets antigos orientam a leitura atual | **CAIU como referência de localização.** U12 mudou para `:166`, e o modal chama o filtro em `:274`. Igualdade histórica dos blobs: **[nao verificado]**. (`app/chamados/templatetags/chamados_tags.py:166`; `app/templates/chamados/partials/modal_fio.html:274`) |
| matriz:12–19 — assinatura, leitura conservada, P5, ordenação e classificação pelo ato | **CAIU esse conjunto contratual.** Fila ordena por `respondida_em`; classificação lê texto projetado. (`app/chamados/juizes.py:1753`; `app/chamados/juizes.py:2030`; `app/chamados/juizes.py:1552`) |
| brief:18; matriz:37,65–66 — `aguarda_admin` já pode ser descrito por uma tabela própria de estados | **CAIU.** O corte exige ler o dono declarado pela tarefa, sem tabela própria no leitor. (`app/docs/CORTES.md:488`) |
| brief:20,65,138 — F6 inteira é leitura, sem persistência | **CAIU.** U6/U9 afetam auditoria/followup; U7 chama validação; U10 escolhe recobrar ou reabrir. (`app/chamados/services/disputa_emissao.py:1546`; `app/chamados/services/disputa_emissao.py:1569`; `app/chamados/views_cobrar.py:208`; `app/chamados/views.py:1425`; `app/chamados/views.py:1436`) |
| brief:44–47 — zero atribuições físicas dos campos nos sete arquivos | **CONFERE para os campos censados; não elimina persistência delegada.** Os sítios são leituras e chamadas de portas. (`app/api/views.py:2620`; `app/api/views.py:2627`; `app/chamados/views_cobrar.py:169`; `app/chamados/views_cobrar.py:208`; `app/chamados/views.py:1280`; `app/chamados/views.py:1283`) |
| brief:29–31; matriz:48–49 — S1/S2 SOME | **CONFERE.** Delegam; o complemento também os exclui da seleção RED. (`app/chamados/relatorios_disputa.py:24`; `app/chamados/relatorios_disputa.py:26`; `app/api/views.py:1179`; `app/docs/CORTES.md:491`) |
| brief:33–35; matriz:44 — U9b inalcançável | **CONFERE por leitura:** a primeira guarda elimina a mesma pergunta morta exigida depois. (`app/chamados/views.py:1242`; `app/chamados/views.py:1243`; `app/chamados/views.py:1257`) |
| brief:34,79,99 — morta na web retorna HTTP 404 | **CAIU.** Web chama `resposta_acao` com erro/redirecionamento; API retorna 404 explícito. (`app/chamados/views.py:1244`; `app/chamados/views.py:1246`; `app/api/views.py:2619`) |
| brief:37–38 — `fechada_em` permanece D02 | **CONFERE a separação dos leitores indicados.** (`app/chamados/views_cobrar.py:161`; `app/chamados/views_cobrar.py:164`; `app/chamados/relatorios_disputa.py:46`) |
| brief:51; matriz:75–76 — contrato público conserva chaves/fases | **CONFERE a regra escrita.** Não autoriza trocar fases por exigibilidade. (`app/docs/BACKLOG.md:2091`; `app/api/views.py:2552`; `app/api/views.py:2553`) |
| brief:53–54 — partição do app ainda lê instante/disputa aberta | **CONFERE.** Propriedade da futura F5 pelo brief dela: **[nao verificado]**. (`app/chamados/services/pergunta_no_app.py:76`; `app/chamados/services/pergunta_no_app.py:81`) |
| brief:71,95–96 — U12 mostra texto “X” ou “sem texto” | **CAIU.** A tag devolve `impl/fr`; vazio devolve ambos nulos. (`app/chamados/templatetags/chamados_tags.py:167`; `app/chamados/templatetags/chamados_tags.py:175`; `app/chamados/templatetags/chamados_tags.py:176`) |
| brief:74; matriz:84–86 — estados de validação sem lavra podem ser agrupados com efeito pendente | **CAIU esse agrupamento.** T distingue sem lavra, materializar, rejulgar e encerrada; o corte manda sem dono=False. (`app/chamados/services/tarefa_do_dia.py:169`; `app/chamados/services/tarefa_do_dia.py:172`; `app/chamados/services/tarefa_do_dia.py:174`; `app/docs/CORTES.md:488`) |
| brief:77,127–128; matriz:69 — passivo conserva apresentação | **CONFERE a conservação; não confirma o número histórico.** T retorna desconhecimento, e os leitores atuais preservam campos. (`app/chamados/services/tarefa_do_dia.py:155`; `app/api/views.py:2561`; `app/api/views.py:2563`; `app/docs/BACKLOG.md:2091`) |
| brief:83–98 — divergência ato/projeção basta como oráculo R1/R3/R4/R5 | **CAIU como RED automaticamente autorizado.** L-130 julga o par atual; o redutor pressuposto não é o contrato disponível. (`app/docs/LEIS.md:151`; `app/chamados/juizes.py:1544`) |
| brief:107–114 — interseções dos hashes antigos, inclusive RELAVRA-11 | **[nao verificado] por diff histórico.** Os sítios atuais compartilhados continuam localizáveis. (`app/chamados/views_cobrar.py:169`; `app/chamados/views_cobrar.py:291`; `app/chamados/templatetags/chamados_tags.py:166`) |
| brief:111 — BUG-PAR ainda futura e acrescenta apenas filtro vizinho | **CAIU.** Registro declara pouso c4+c4c; há também adaptador de aplicação e fallback. (`app/docs/BACKLOG.md:2084`; `app/chamados/templatetags/chamados_tags.py:132`; `app/chamados/templatetags/chamados_tags.py:139`) |
| brief:116–118 — rotas F6 passam pela UI | **CONFERE no Caddyfile.** O matcher core não contém essas rotas; o fallback aponta para UI. Recarga efetiva das cascas: **[nao verificado]**. (`Caddyfile:8`; `Caddyfile:9`; `Caddyfile:10`) |
| brief:121–124 — validação sem efeito conserva `aguarda_admin` | **CONFERE a regra D05**, inclusive quando T atribui a ação ao sistema. (`app/docs/BACKLOG.md:2091`; `app/chamados/services/tarefa_do_dia.py:172`; `app/chamados/services/tarefa_do_dia.py:174`) |
| brief:129–130 — todas as antigas respostas vazias ficam invariavelmente fora do app | **CAIU como regra universal atual.** L-130 distingue cura do estoque aberto e estoque entregue; os números atuais são **[nao verificado]**. (`app/docs/LEIS.md:151`) |
| brief:143–144 — aguardar F1 antiga e rebase futuro sobre BUG-PAR | **CAIU nessa forma.** F1 B1 e BUG-PAR têm registros de pouso; B2 continua sendo a reconferência necessária dos leitores. (`app/docs/CORTES.md:488`; `app/docs/BACKLOG.md:2084`; `app/chamados/juizes.py:1551`) |
| matriz:107 — ainda pedir conferência do APK ao Fernando | **CAIU.** O complemento registra que o campo é opcional, que `dias_esperando` não é lido e manda baixar a pendência. Fonte Android independente: **[nao verificado]**. (`app/docs/CORTES.md:491`) |

**Contagem das marcas:** 11 no brief e 29 na matriz; duas dependências da interface F2 permanecem. As demais 38 caíram na forma pressuposta pelos anexos. As ocorrências e seus fundamentos estão individualizados acima. (`app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1551`; `app/chamados/models.py:3151`; `app/chamados/models.py:3688`)

DEPENDE CAIU 38 de 40

## 2 Censo escritores e leitores

Convenção: caminhos do inventário abaixo relativos a `app/`; **R**=`respondida_em`, **T**=`resposta_colab`, **V/VP**=`validada_em/validada_por`, **M/MP**=`materializada_em/materializada_por`. **NOVO** significa ausente da enumeração do brief, sem afirmar nascimento recente. Acessos inteiramente construídos dinamicamente: **[nao verificado]**.

**Escritores físicos e portas:**

| Campos | Escritor/persistência na BASE |
|---|---|
| R/T e campos iniciais — **NOVO** | Nascimento por `**campos`, `save` e conservação da projeção anterior. (`chamados/models.py:3124`; `chamados/models.py:3127`; `chamados/models.py:3129`) |
| R/T | Resposta humana: atribui, salva e registra RESPOSTA. (`chamados/services/disputa_emissao.py:1481`; `:1482`; `:1522`; `:1525`) |
| R/T — **NOVO** | Automática: atribui, salva e registra RESPOSTA. (`chamados/services/disputa_emissao.py:1674`; `:1675`; `:1677`; `:1678`) |
| R — **NOVO** | Reparo pela trilha: instante testemunhado e RESPOSTA. (`chamados/services/disputa_emissao.py:1696`; `:1698`; `:1699`) |
| V/VP | Nova resposta limpa validação anterior. (`chamados/services/disputa_emissao.py:1494`; `:1495`; `:1496`) |
| V/VP | `_carimbar_validacao`: escreve e registra VALIDACAO. (`chamados/services/validacao.py:168`; `:169`; `:171`; `:173`) |
| V/VP | Devolução recusada: limpa e registra DEVOLUCAO. (`chamados/services/validacao.py:273`; `:274`; `:276`; `:279`) |
| V/VP — **NOVO BUG-PAR** | Devolução de paridade: limpa e registra DEVOLUCAO. (`chamados/services/validacao.py:349`; `:350`; `:351`; `:354`) |
| R/V/VP — **NOVO** | `carimbar(resposta=)` pode preencher e salvar sem registrar RESPOSTA/VALIDACAO nesse corpo. (`chamados/models.py:3242`; `:3245`; `:3246`; `:3262`) |
| R/T/V/VP | Reabertura prepara zeragem e usa porta de lote. (`chamados/services/disputa_emissao.py:1609`; `:1648`) |
| R/T/V/VP — **NOVO** | Ressuscitação conserva linhas e zera o par inteiro pela porta de lote. (`chamados/models.py:1613`; `:1629`; `:1630`) |
| M/MP | `_escrever_carimbo` atribui; `carimbar`, identidade, descarimbo e lote persistem. (`chamados/models.py:3209`; `:3210`; `:3262`; `:3300`; `:3322`; `:3393`) |
| Lote — **NOVO** | `descarimbar_em_lote` também persiste campos recebidos em `outros`. (`chamados/models.py:3391`; `:3393`) |
| Só memória — **NOVO** | Simulação da ressuscitação e restauração após falha. (`chamados/models.py:1554`; `:1555`; `:1556`; `:3273`; `:3274`; `:3275`) |
| Sequência — **NOVO no censo F6** | Porta persistente `DecisaoPergunta.registrar`, com `bulk_create` interno. (`chamados/models.py:4110`; `:4120`) |

O homônimo `RegularizacaoExterna` não pertence à pergunta. (`chamados/services/regularizacao_ext.py:199`; `chamados/models.py:4152`)

**Leitores, seletores, fotos e transportes dos campos.** Os escritores acima também aparecem neste inventário. Fora dos sete módulos nomeados pelo brief, os sítios são **NOVO no censo**.

```text
api/views.py T:1163,2561; R:1165,2563,2577,2620,2637
api/views_mensageria.py T:567; R:569,570
chamados/canal.py T:65
chamados/templatetags/chamados_tags.py T:139,166

NOVO chamados/juizes.py
 M:292,683,711,858,1390,1502
 R:352,856,1389,1423,1534,1541,1552,1572,1580,1744,1753
 T:364,857,1360,1389,1423,1534,1541,1552,1572,1580,1779,1926,2030,2035,2052
 V:856,1389

NOVO chamados/models.py
 M:1531,1923,2006,3153,3156,3166,3209,3236,3238,3292,3293,3296,3320,3331,3365,3393,3411,3423,3440,3483,3485
 MP:3210,3236,3238,3293,3296,3320,3365,3393
 R:1555,1614,1629,1924,2949,3155,3158,3237,3241,3242,3243,3273,3331,3370,3375,3376,3380,3382,3568,3584,3585,3665,4091,4098,4099,4100
 T:1533,1556,1614,1630,1925,2007,2139,2950,3240,3331,3370,3375,3379,3383,3568,3584,3586,3688,3693,3697,3701,3705,3708,3709,3710,4101,4105
 V:310,344,1554,1614,1629,1923,3151,3237,3244,3245,3247,3274,3331,3496,4091,4102,4103,4104
 VP:1614,1629,3237,3246,3247,3275,4105

NOVO chamados/reconciliador.py
 M:367,630,1050,1145,1155,1505; MP:1146,1156
 T:365,1083; V:631,1050

NOVO chamados/services/acoes_disputa.py R:97,132; T:355; V:347
NOVO chamados/services/becos.py R:30
NOVO chamados/services/declaracao_texto.py R:417,419,687; T:417; V:227

NOVO chamados/services/disputa_emissao.py
 M:228,776,1084,1104,1340,1637,1930,2062,2073,2090
 R:1482,1483,1609,1635,1675,1677,1692,1696,1698,1703,1735,1843,1861,2074,2091
 T:1481,1483,1609,1635,1663,1674,1677,1692,1700,1861,2079
 V:1484,1485,1494,1496,1537,1538,1609,1636,2062
 VP:1484,1495,1496,1537,1538,1609,1636

NOVO chamados/services/explicador.py T:55,102,103
NOVO chamados/services/fabrica_da_celula.py M:171
NOVO chamados/services/fio.py R:187,706,1613; T:377,1279,1291,1613
NOVO chamados/services/foto_e4b.py M/R/V:180
NOVO chamados/services/historico.py R:99; V:101; M:103; T:109

NOVO chamados/services/materializacao.py
 M:148,261,262,289,326,862,871,1015,1107,1109,1114,1153,1652,1656,2066,2079,2127
 MP:1154; R:1151,2064
 T:546,574,625,671,678,718,733,770,869,950,1047,1109,1150,1311,1352,1534,1558,1567,1619
 V:199,201,1152,2065,2082; VP:1152

NOVO chamados/services/painel.py M/R/V:67; VP:262; V:270,271,272,273
NOVO chamados/services/passivo_validado.py M:64,534,535,536; MP:64,535; R/V/VP:66
NOVO chamados/services/pergunta_no_app.py R:81
NOVO chamados/services/raiox.py M:135,182; R/T:138,147
NOVO chamados/services/relavra_via.py M:181,244,252

NOVO chamados/services/validacao.py
 M:262,264,341,363,556,681,701,717; MP:342,363
 R:594,813,824,848,1167
 T:175,263,281,356,380,823,843,848,1094,1096,1098,1

[truncado em 20000 de 59525 caracteres]

## 17/11 01:21 · sessao 725e070a4ac0 · - · - · origem=Ronald (tmux)

- **Premissas confirmadas:** `trocar_plantao` abre `atomic` próprio, ordena cartas por `pk` e apenas as duas células da chamada; captura `_TrocaRecusada` e devolve `'recusada'`. Portanto, ordenar as quatro antes das chamadas e provocar rollback externo são exigências coerentes, ainda por implementar (`app/ponto/portas/celula.py:690`, `app/ponto/portas/celula.py:695`, `app/ponto/portas/celula.py:711`, `app/ponto/portas/celula.py:732`). A lei respondida, d1=d2 e o contrato constam no RELATO (`app/docs/RELATO.md:1811`).
- **QUEM:** opus construtor resolve a vedação ao Codex sobre `celula.py` e item que pede `!`; a regra também exige separação entre construção e certificação (`app/docs/LEIS.md:147`).
- **Referência inexata:** a obra está em `BACKLOG.md:2198`; `:2199` é RETRATO-POR-EVENTO (`app/docs/BACKLOG.md:2198`, `app/docs/BACKLOG.md:2199`).
- **d1=d2 exige implementação efetiva:** duas chamadas atuais sobre o mesmo dia repetem as duas coordenadas, escrevendo os papéis False/True e depois True/False; o DNA vem do vínculo próprio e de seus marcos. Isso não demonstra uma permuta de turnos no mesmo dia. É lacuna da implementação atual, não contradição nova do aval que permite mudar a porta (`app/ponto/portas/celula.py:711`, `app/ponto/portas/celula.py:622`, `app/ponto/portas/celula.py:629`, `app/ponto/management/commands/gerar_celulas.py:85`, `app/ponto/management/commands/gerar_celulas.py:90`).
- **Dinheiro é alcançado:** a edição chama o rabo comum, que agenda recálculo do fechamento no commit (`app/ponto/portas/celula.py:665`, `app/ponto/portas/celula.py:483`, `app/ponto/portas/celula.py:496`, `app/ponto/services/fechamento.py:1807`, `app/ponto/services/fechamento.py:1824`). Valores ou impacto real: **[nao verificado]**.
- **Exportada:** a porta barra a competência lavrada, salvo `apesar_da_lavra`; a enumeração das exportações não usa trava e o TOCTOU já está registrado na fila. O novo DIFF precisa demonstrar o alcance, e não pode presumir que a guarda tornou a composição concorrente segura (`app/ponto/portas/celula.py:606`, `app/ponto/portas/celula.py:621`, `app/ponto/management/commands/gerar_celulas.py:170`, `app/docs/BACKLOG.md:2197`). Competência exportada efetivamente atingida: **[nao verificado]**.
- **Limite material do rollback:** a edição rejulga sincronamente; o cartório pode abrir chamado e o emissor envia push imediatamente. Por dedução estática desse caminho, rollback SQL não recolhe uma notificação já enviada. É condição que o construtor precisa enfrentar para prometer rollback integral; envio real nesse cenário: **[nao verificado]** (`app/ponto/portas/celula.py:472`, `app/ponto/services/cartorio.py:1791`, `app/ponto/management/commands/detectar_ausencias.py:779`, `app/chamados/push_utils.py:40`).
- **Não interpretar como autorização para reduzir o contrato:** a obra existente exige quatro validações antes da primeira escrita, rollback integral, recusa falante, ID próprio ligando células/pedido e escala-base intocada; o item 2 do novo aval lista só três obrigações do PREPARA, mas a FONTE mantém o contrato maior (`app/docs/BACKLOG.md:2198`). Ausência dessas obrigações na construção futura: **[nao verificado]**.

## 17/11 01:21 · sessao 26402f43f3aa · REVISAO-AVAL-C3PREP · verificador · origem=Code

Referências aos anexos contam as linhas a partir de “aval Ronald”.

1. **Premissas:** a cópia registra PREPARA F2/F4/F7, entregas e sondas da volta 2 (`app/docs/RELATO.md:16–23`), além do PID 2218451 (`app/docs/RELATO.md:3`). O teto do papel sonda é 1179 s (`bin/codex_pergunta.sh:95`). Resultado independente de `revisao_c2prep`, HEAD atual do main, quantidade de tentativas e rc de cada uma: **[nao verificado]**.

2. **Colisão com o vigente:** não identificada no escopo de leitura declarado (`complemento3_prepara_1711.txt:1–6`). A precedência antiga da CLASSE-D (`app/docs/BACKLOG.md:1990`; `app/docs/RELATO.md:421`) foi expressamente dispensada para abrir F2 pelo COMPLEMENTO-1 (`app/docs/RELATO.md:4`). LU2-07 e a parte aberta da LU2-08 continuam pendentes; o PREPARA não as resolve (`app/docs/RELATO.md:352–354`, `:422`). Os cortes das fatias permanecem aplicáveis (`app/docs/CORTES.md:488`). Brief integral e cruzamentos atuais das oito remedidas: **[nao verificado]**.

3. **Literal:** o C3 substitui expressamente a espera da volta 2 e preserva o restante do C2; não há contradição nessa antecipação (`complemento3_prepara_1711.txt:2`; `complemento2_prepara_f3_f5_f6_f8_f9_1711.txt:4`). O ramo “duas vezes ou mais” depende da medição solicitada: não pode ser tratado como ocorrido nem como ausente **[nao verificado]**. Na interpretação dos rc, 124 é timeout do Codex; expiração da trava da resposta retorna 3 (`bin/codex_pergunta.sh:392`, `:425–428`, `:447`). O envelope SQL roda antes desse timeout, portanto esse rc não prova que o SQL excedeu 1179 s (`bin/codex_pergunta.sh:329`, `:425`).

4. **Exportada, dinheiro ou `!`:** nenhuma escrita nesses domínios é autorizada pelos anexos; o escopo permanece registro e leitura, sem construtor, suite, trava ou mudança do push (`complemento3_prepara_1711.txt:1`, `:6`; `complemento2_prepara_f3_f5_f6_f8_f9_1711.txt:3`, `:6`). A guarda de exportada continua expressa no corte da F2 (`app/docs/CORTES.md:488`). Execução efetiva das sondas e seus efeitos: **[nao verificado]**.

VEREDITO: CONFERE (`complemento3_prepara_1711.txt:2–6`; `app/docs/RELATO.md:4`; `bin/codex_pergunta.sh:95`).

## 17/11 01:21 · sessao a191aa5b5315 · - · - · origem=Ronald (tmux)

- **Premissas documentadas:** O10-D exige quatro pré-validações antes da primeira escrita, rollback integral, recusa falante, ID próprio e escala-base intocada; construtor opus e pouso no `!` de escala já constam em `app/docs/BACKLOG.md:2198`. PREPARA fechada, lei zero e cruzamento dependente da lista do construtor constam em `app/docs/RELATO.md:147`; LOTE7 registrado como pousado em `app/docs/RELATO.md:164`. A referência do aval a `BACKLOG:2199` erra uma linha: ali está RETRATO-POR-EVENTO (`app/docs/BACKLOG.md:2199`).

- **Pista/trava, item 7:** o corte vigente manda suite inteira/pre-opus lateral à PISTA; pista ocupada significa esperar, e VM é fallback somente com pista indisponível e causa registrada (`app/docs/CORTES.md:490`). O complemento distingue suite de não-pouso lateral na PISTA e VM para espinha/pouso/push (`app/docs/RELATO.md:102`). Portanto “pedir a trava na brecha” não autoriza escolher VM enquanto a PISTA funciona; essa leitura colidiria com esses cortes (`app/docs/CORTES.md:490`; `app/docs/RELATO.md:102`). O item pode coexistir se conservar esse roteamento; intenção literal de substituir o roteamento: [nao verificado].

- **Fila não é garantia mecânica:** `--quem` só informa titular; aquisição usa `flock`, sem interface de fila ou prioridade (`bin/trava_teste.sh:28`; `bin/trava_teste.sh:35`). O RELATO já registra que prioridade depende do despacho (`app/docs/RELATO.md:6`). Estado atual “espinha sem suite na fila” e cessão efetiva: [nao verificado].

- **CLASSE-D/LU2:** CLASSE-D R1 trata C5 por dia/dono, muda tela/PDF e admite pouso separado sem aguardar LU2-07/08 (`app/docs/RELATO.md:421`). LU2-08 conserva corte pendente para FATO_EM_AUSENCIA e migração do insumo em `InsumosDoDono.dono()` (`app/docs/RELATO.md:352`; `app/docs/RELATO.md:422`). LU2-07 conserva questão emp1 e preservação L-124 da competência 10 paga (`app/docs/RELATO.md:354`). O aval não deve ser interpretado como autorização para decidir esses pontos; cruzamento efetivo com futura lista de O10-D: [nao verificado]. Os briefs físicos referidos em `logs/` não estão disponíveis nesta cópia: [nao verificado].

- **Ordem atualizada:** não bloquear F2 simplesmente pela ordem antiga CLASSE-D → F2 de `app/docs/BACKLOG.md:1990`: o complemento já permite F2 após B2 sem aguardar CLASSE-D (`app/docs/RELATO.md:4`).

- **Verificação obrigatória permanece:** todo construtor deve passar pelo pre-verificador, triagem com descartes anexos ao opus, suite inteira verde e depois opus final (`CLAUDE.md:372`; `CLAUDE.md:376`; `CLAUDE.md:378`). O aval cita os papéis, mas execução desses portões ainda é [nao verificado]. [nao verificado]

## 17/11 01:20 · sessao dc4aa7b78cbb · - · - · origem=Ronald (tmux)

- **Teto/condição literal:** “teto do papel sonda” corresponde ao teto de **1179 s**, eventualmente rebaixado por `CODEX_TIMEOUT` (`bin/codex_pergunta.sh:95-98`). O timeout envolve o Codex desse papel (`bin/codex_pergunta.sh:425-428`); `124` é tratado como timeout (`bin/codex_pergunta.sh:447-451`). Portanto, **não há colisão literal** no item 3. Quantidade de tentativas e respectivos rc: **[nao verificado]**.
- **Precisão operacional:** a execução do envelope antecede o timeout do Codex (`bin/codex_pergunta.sh:323-329`). Assim, timeout do papel não prova demora do SQL; o aval fala do papel, portanto isso não fundamenta PROCEDE (`bin/codex_pergunta.sh:95`; `bin/codex_pergunta.sh:425`).
- **Paralelismo:** leituras independentes em paralelo estão previstas até o teto medido (`CLAUDE.md:357-365`), e o wrapper controla slots (`bin/codex_pergunta.sh:364-381`). Soltar as cinco leituras não exige aumentar esse teto.
- **Abertura F2/CLASSE-D:** o registro vigente já responde que F2 abre pelas quatro condições contra HEAD com B2 pousada, **sem esperar CLASSE-D** (`app/docs/RELATO.md:4`). A frota aparece como medida posterior de volta 2, com lei zero já registrada (`app/docs/RELATO.md:16-19`).
- **B2/suite/trava/push:** o PREPARA registrado já é sem cópia, suite ou trava (`app/docs/RELATO.md:23`). B2 consta aberta separadamente (`app/docs/RELATO.md:30`); regras próprias da VM/pista seguem em `app/docs/CORTES.md:490`. Não encontrei colisão nesses pontos; estado real dos processos/logs: **[nao verificado]**.

## 17/11 01:20 · sessao 281d67d961ba · - · - · origem=Ronald (tmux)

- **Sem colisão com CLASSE-D na abertura da F2:** a ordem antiga exige CLASSE-D antes da F2 (`app/docs/BACKLOG.md:1990`; `app/docs/RELATO.md:421`), mas o registro posterior responde expressamente: F2 abre pelas quatro condições contra HEAD com B2 pousada, **sem esperar CLASSE-D** (`app/docs/RELATO.md:4`).
- **CLASSE-D continua distinta:** R1 propõe C5 por dia e dono por `InsumosDoDono`, sem esperar código LU2-07/08 (`app/docs/RELATO.md:421`). A cópia ainda publica C5 por colaborador, sem data (`app/escala/services/cadastro_realidade.py:176`), e exclui C5 de `DONO_PELO_JUIZ` (`app/escala/services/cadastro_realidade.py:968`).
- **LU2-07/08 não foram resolvidas por esse PREPARA:** LU2-07 mantém pergunta sobre operação emp1 e retenção por assinatura datada (`app/docs/RELATO.md:354`); cron limita empresas a 2/3/4 (`app/config/crons.py:17`). LU2-08 distingue parcial sem período decidido e fato em ausência aberto (`app/docs/RELATO.md:352`); brief mantém insumo em `InsumosDoDono.dono()` e espera corte para fatos em ausência (`app/docs/RELATO.md:422`).
- **Cuidado com referência histórica:** o corte COBERTURA-PARCIAL-PERIODO citado como `CORTES.md:370` no RELATO está nesta cópia em `app/docs/CORTES.md:386`: parcial sem período vai a CADASTRO e não a FATO_EM_AUSENCIA.
- **Cruzamentos atuais das oito remedidas e brief integral CLASSE-D:** [nao verificado]. Os arquivos `logs/...` citados não estão na cópia; os registros disponíveis são os resumos acima (`app/docs/RELATO.md:421`; `app/docs/RELATO.md:422`).
- **PREPARA permanece leitura sem construção**, compatível com despacho anterior, sem cópia/suíte/trava (`app/docs/RELATO.md:23`); as matrizes já eram preparação separada da construção (`app/docs/BACKLOG.md:2063`).

## 17/11 01:19 · sessao 349b1eb9edec · PREPARA-F3-F5-F6-F8-F9 · leitura · origem=Code

## 1 DEPENDE e premissas

**A premissa central da F8 caiu:** a F1 atual oferece `pergunta_respondida(p)`, que exige instante **e** texto após trim; o próprio contrato deixa os leitores B/C/D para a B2. A antiga interface de leitura dos atos não pode orientar esta preparação. (`app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1550`; `app/chamados/juizes.py:1552`)

**A L1 antiga já foi decidida:** V legada permanece válida diante de R sintética posterior. O corte exige medir perguntas e batidas contra a árvore pousada; os números atuais continuam **[nao verificado]**. (`app/docs/CORTES.md:488`, item 8)

BASE identificada pelo pedido: `fbc5c76ced33bf25943f3a5bc78ebff257d68c56`. Confirmação independente do SHA/HEAD, ancestralidade e diferenças entre os commits históricos: **[nao verificado]**. Resultados de testes e sondas em execução: **[nao verificado]**.

`B` e `M` abaixo significam os anexos `brief.md` e `matriz.md`, numerados desde o título, incluindo linhas vazias. Correspondência dessa numeração com arquivos externos originais: **[nao verificado]**. As referências `app/...` são da BASE.

**Cada marca literal dos anexos:**

| ID | Origem | Premissa | Julgamento na BASE |
|---|---|---|---|
| B1 | B:18 | Contrato F1 da coluna U1–U9 fornece leitura de R/V por atos | **CAIU.** O contrato atual retorna booleano sobre a projeção. (`app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1552`) |
| B2 | B:31 | As oito trocas e as exceções dependem daquela interface | **CAIU como contrato de execução.** Os leitores continuam crus; sua migração B/C/D é atribuída à B2. (`app/chamados/juizes.py:1551`; `app/ponto/services/cartorio.py:1641`; `app/colaboradores/services/calendario.py:504`) |
| B3 | B:56 | O contrato não fornece a recusa técnica de U2 | **CONFERE o limite.** O juiz atual não retorna motivos; U2 lê o motivo técnico projetado. (`app/chamados/juizes.py:1552`; `app/ponto/services/flip_auto.py:132`; `app/chamados/services/materializacao.py:1665`) |
| B4 | B:61 | Estão disponíveis as faces privada, conservada e SQL descritas em B:62–64 | **CAIU.** As faces concretas atuais são `pergunta_respondida` e seu `Q` projetivo. (`app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1575`) |
| B5 | B:68 | A comparação de frota pode usar a antiga leitura conservada como juiz atual | **CAIU.** A projeção temporal e o juiz atual leem campos; a comparação histórica não mede esta BASE. (`app/chamados/juizes.py:1389`; `app/chamados/juizes.py:1552`) |
| M1 | M:8 | F1 vigente é a interface de `be2b8d733` | **CAIU.** O contrato declara F1 NOVA B1/L-130. (`app/chamados/juizes.py:1545`; `app/chamados/juizes.py:1550`) |
| M2 | M:24, primeira marca | U1 lê `autor_validacao` da leitura F1 | **CAIU.** A função atual não fornece autor. U1 continua lendo V/C diretamente. (`app/chamados/juizes.py:1552`; `app/ponto/services/procedencia_batida.py:72`) |
| M3 | M:24, segunda marca | A fixture V sem DP só passa por conservação do fold | **CAIU a exclusividade.** V projetada é lida diretamente, inclusive pelo C6. (`app/ponto/services/procedencia_batida.py:74`; `app/chamados/juizes.py:1405`) |
| M4 | M:25 | Não há autor C fornecido pelo contrato F1 | **CONFERE o limite.** O contrato atual é booleano; o fallback de autor continua projetivo. (`app/chamados/juizes.py:1552`; `app/ponto/services/procedencia_batida.py:72`) |
| M5 | M:26 | U1 recebe `instante_validacao` da leitura F1 | **CAIU.** U1 lê o campo atual; o juiz L-130 não devolve instante. (`app/ponto/services/procedencia_batida.py:74`; `app/chamados/juizes.py:1552`) |
| M6 | M:27 | U1 recebe `instante_materializacao` daquela leitura | **CAIU.** O fallback atual é o campo C. (`app/ponto/services/procedencia_batida.py:74`) |
| M7 | M:34 | U2 recebe `instante_validacao` do contrato antigo | **CAIU.** A seleção atual é SQL sobre V projetada. (`app/ponto/services/flip_auto.py:130`; `app/chamados/juizes.py:1552`) |
| M8 | M:35 | Existe `leitura.motivo_recusa` como alternativa executável atual | **CAIU a interface pressuposta.** Permanece válida a distinção entre recusa técnica e RECUSA administrativa. (`app/chamados/juizes.py:1552`; `app/chamados/services/materializacao.py:1665`; `app/chamados/services/validacao.py:836`) |
| M9 | M:42 | U3y sob F1 só preserva R pela face conservada antiga | **CAIU a explicação.** O teste usa instante e texto; o leitor atual continua projetivo. (`app/ponto/tests/test_fechar3_troca_escala_gate.py:351`; `app/ponto/services/cartorio.py:1641`) |
| M10 | M:50 | O comportamento de SILENCIO pode ser explicado pelos redutores antigos citados | **CAIU o suporte dessa interface.** A tarefa atual lê SILENCIO; sua fotografia continua projetiva. (`app/chamados/services/tarefa_do_dia.py:176`; `app/ponto/services/veredito_celula.py:197`) |
| M11 | M:56 | A face SQL pública exclui texto só-branco | **CONFERE**, agora exigindo também instante. (`app/chamados/juizes.py:1575`; `app/chamados/juizes.py:1580`) |
| M12 | M:75 | A seção seguinte descreve a F1 disponível | **CAIU.** `carga_pergunta_viva` oferece `carga_da_vida`; o juiz de resposta é o booleano L-130. (`app/chamados/services/carga_pergunta_viva.py:69`; `app/chamados/juizes.py:1544`) |
| M13 | M:89 | P1 pode ser cumprido por `LeiturasDaVida`/face conservada antiga | **CAIU a API.** O juiz único e sua face SQL atuais estão nomeados. (`app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1575`) |
| M14 | M:106 | L1 continua herdada e sem decisão | **CAIU.** O item 8 posterior responde SIM. (`app/docs/CORTES.md:488`) |
| R1 | M:10 | Calendário tem a forma funcional R3 | **CONFERE a forma**, sem certificar ancestralidade dos hashes. (`app/colaboradores/services/calendario.py:188`; `app/colaboradores/services/calendario.py:506`) |
| R2 | M:56 | Filtro R está no calendário na forma R3 | **CONFERE.** (`app/colaboradores/services/calendario.py:504`) |
| R3 | M:57, primeira marca | C6 recebe `agora`; falha da oferta fica desconhecida | **CONFERE.** (`app/colaboradores/services/calendario.py:506`; `app/colaboradores/services/calendario.py:509`) |
| R4 | M:57, segunda marca | A tarefa carrega a oferta plural do calendário | **CONFERE**, com chamada atual deslocada. (`app/chamados/services/tarefa_do_dia.py:607`; `app/chamados/services/tarefa_do_dia.py:613`) |
| R5 | M:57, terceira marca | Pin do calendário é `[273]` | **CONFERE.** (`app/core/tests/test_medida_turno_do_par.py:275`) |
| R6 | M:103 | F8 deve considerar a forma R3 do calendário | **CONFERE a dependência funcional.** A ordem histórica entre commits permanece **[nao verificado]**. (`app/colaboradores/services/calendario.py:188`; `app/colaboradores/services/calendario.py:504`) |

**Demais premissas sobre F1/BUG-PAR/outras fatias, contadas separadamente das marcas:**

| ID | Origem | Premissa | Julgamento na BASE |
|---|---|---|---|
| A1 | B:3; M:6–10 | As bases antigas e a F1 citada descrevem o contrato de partida atual | **CAIU como base operativa.** O código declara F1 NOVA B1. Identidade histórica dos blobs: **[nao verificado]**. (`app/chamados/juizes.py:1550`) |
| A2 | B:9–13,30; M:56–57 | Calendário possui `agora`, `_oferta_conhecida` e C6 temporal | **CONFERE.** (`app/colaboradores/services/calendario.py:188`; `app/colaboradores/services/calendario.py:490`; `app/colaboradores/services/calendario.py:506`; `app/colaboradores/services/calendario.py:624`) |
| A3 | B:33; M:57 | O chamador plural passou do vigia para a tarefa | **CONFERE o chamador atual.** (`app/chamados/services/tarefa_do_dia.py:613`) |
| A4 | B:20–28,39; M:22–73 | Os oito arquivos ainda contêm os leitores projetivos apontados | **CONFERE.** (`app/ponto/services/procedencia_batida.py:72`; `app/ponto/services/flip_auto.py:130`; `app/ponto/services/cartorio.py:1641`; `app/ponto/services/veredito_celula.py:197`; `app/colaboradores/services/calendario.py:504`; `app/colaboradores/services/ficha.py:171`; `app/folha/previa.py:142`; `app/ponto/services/dia_do_colab.py:70`) |
| A5 | B:55–56; M:35 | `recusa_motivo` possui somente os dois escritores enumerados | **CAIU.** Há escape do colaborador, pendência técnica e porta genérica, além dos dois sentidos descritos. (`app/chamados/services/disputa_emissao.py:1511`; `app/chamados/services/materializacao.py:1332`; `app/chamados/models.py:3298`) |
| A6 | B:24; M:49 | C de U5 é carimbo do mesmo ato de fechamento | **CONFERE.** (`app/ponto/services/veredito_celula.py:190`; `app/ponto/services/veredito_celula.py:196`) |
| A7 | B:59; M:63 | A troca isolada de U7a basta para unificar a fonte da ficha | **CAIU.** A ficha também delega à retenção, ainda I OU T. (`app/colaboradores/services/ficha.py:169`; `app/chamados/models.py:1924`; `app/chamados/models.py:1925`) |
| A8 | M:76–84 | `LeituraDosAtos`, `_leitura_conservada` e `LeiturasDaVida` são o contrato disponível | **CAIU.** O contrato concreto é juiz booleano, projeção temporal e carga de vitalidade. (`app/chamados/juizes.py:1382`; `app/chamados/juizes.py:1544`; `app/chamados/services/carga_pergunta_viva.py:69`) |
| A9 | B:63,123; M:82–84 | A face SQL atual materializa frota em Python ou usa a Subquery conservada histórica | **CAIU.** A face atual é um `Q` sobre instante e regex. (`app/chamados/juizes.py:1580`) |
| A10 | B:64,92; M:79–81 | A F8 atual necessariamente precisa de `decisoes=`/prefetch para chamar o juiz | **CAIU.** O juiz lê apenas dois campos do objeto. (`app/chamados/juizes.py:1552`) |
| A11 | B:92 | Não existe selo de queries do calendário | **CAIU como ausência geral.** Existe limite do perfil fixo; constante em N carimbadas continua **[nao verificado]**. (`app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:230`; `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:237`) |
| A12 | B:49 | `classificar_pares` chama a função `fila_flip` | **CAIU.** É `append` numa lista local; a função `fila_flip` é quem chama `classificar_pares`. (`app/ponto/services/flip_auto.py:112`; `app/ponto/services/flip_auto.py:146`) |
| A13 | B:101,104; M:36 | A BUG-PAR antiga, ainda externa, basta para descrever a dependência atual | **CAIU.** A porta vigente já contém K6 antes da devolução. Comparação com o hash antigo: **[nao verificado]**. (`app/chamados/services/validacao.py:338`; `app/chamados/services/validacao.py:339`) |
| A14 | B:104; M:36 | BUG-PAR pode retirar perguntas do universo de U2 | **CONFERE**, quando passa pelo recorte atual: descarimba, limpa V e registra DEVOLUCAO; sem dia/entregue não devolve. (`app/chamados/services/validacao.py:339`; `app/chamados/services/validacao.py:346`; `app/chamados/services/validacao.py:349`; `app/chamados/services/validacao.py:354`) |
| A15 | B:49,108; M:36 | A cadeia relevante de flip se esgota no cron | **CAIU como censo completo.** Também existe consumidor humano de `avaliar_fila`. (`app/ponto/views.py:3457`; `app/ponto/views.py:3459`) |
| A16 | B:21,23,31,115; M:92–95 | Via, motivo técnico e fallback C permanecem projetivos | **CONFERE nos corpos e nas exceções descritas.** (`app/ponto/services/flip_auto.py:129`; `app/ponto/services/flip_auto.py:132`; `app/ponto/services/cartorio.py:1650`; `app/ponto/services/procedencia_batida.py:72`; `app/ponto/services/procedencia_batida.py:74`) |
| A17 | B:48; M:23,96–97 | O teste de U1 mede uma query, mas sua fixture não carimba R/V | **CONFERE.** (`app/ponto/tests/test_procedencia_batida.py:123`; `app/ponto/tests/test_procedencia_batida.py:126`) |
| A18 | M:99 | U2 já satisfaz relógio na borda da rodada | **CAIU como comportamento presente.** A rodada lê um instante; U2 lê outro. (`app/ponto/services/flip_auto.py:126`; `app/ponto/services/flip_auto.py:263`; `app/ponto/services/flip_auto.py:268`) |
| A19 | B:102; M:102 | Os três pins identificados permanecem relevantes | **CONFERE.** (`app/core/tests/test_medida_turno_do_par.py:275`; `app/core/tests/test_medida_turno_do_par.py:319`; `app/core/tests/test_medida_turno_do_par.py:330`) |
| A20 | B:110,119,121; M:106–109,118–119 | Ainda falta responder L1 e escolher entre fold puro/conservado/backfill para liberar F8 | **CAIU.** L1 foi respondida; o juiz atual não exige linhas DP para reconhecer o par projetado. (`app/docs/CORTES.md:488`; `app/chamados/juizes.py:1552`) |

As afirmações históricas de igualdade de blobs, neutralidade de frota, quantidade de mudanças de procedência e fila de produção não são medidas desta BASE: **[nao verificado]**. A resposta sintética não revogar V é regra escrita; sua quantidade é objeto da sonda. (`app/docs/CORTES.md:488`)

Contagem: 25 marcas literais mais 20 premissas adicionais; cada linha acima conta uma vez. Os contadores textuais de M:116 não são novas dependências. Correspondência desses contadores com os arquivos externos: **[nao verificado]**.

DEPENDE CAIU 28 de 45

## 2 Censo escritores e leitores

Convenções: caminhos relativos à raiz da BASE; `T=resposta_colab`, `I=respondida_em`, `V=validada_em`, `VP=validada_por/_id`, `C=materializada_em`, `CP=materializada_por/_id`, `X=via_resolucao`, `M=recusa_motivo`. **NOVO** significa não enumerado como esse sítio no brief F8. Cobertura de nomes inteiramente construídos em execução: **[nao verificado]**.

**Escritores físicos, portas e simulações:**

| Campos | Escritores |
|---|---|
| T/I | **NOVO:** resposta humana em `app/chamados/services/disputa_emissao.py:1481,1482,1522,1525`; automática em `:1674,1675,1677,1678`; trilha preenche I em `:1696,1698,1699`. |
| T/I/V/VP | **NOVO:** ressuscitação em `app/chamados/models.py:1629,1630`; reabertura em `app/chamados/services/disputa_emissao.py:1609,1648`; nascimento genérico em `app/chamados/models.py:3124,3127,3129`. |
| I/V/VP | **NOVO:** `carimbar(resposta=)` em `app/chamados/models.py:3242,3245,3246,3262`; esse corpo não registra RESPOSTA/VALIDACAO. |
| V/VP | `app/chamados/services/validacao.py:168,169,171,173`; devolução em `:273,274,276,279`; **NOVO BUG-PAR** em `:349,350,351,354`; nova resposta limpa em `app/chamados/services/disputa_emissao.py:1494,1495`. |
| C/CP/X | `_escrever_carimbo` em `app/chamados/models.py:3209,3210,3211`; persistidores em `:3262,3300,3322`; **NOVO** lote em `:3391,3393`. |
| Somente X | **NOVO:** encerramento específico em `app/chamados/models.py:3402,3403`; relavra em `app/chamados/services/relavra_via.py:125,127`. |
| M | Escape **NOVO** em `app/chamados/services/disputa_emissao.py:1511,1522`; pendência técnica **NOVA** em `app/chamados/services/materializacao.py:1332,1333`; `_carimbar_recusa` em `:1665,1666`; colisão delega em `:1662,1663` a `app/chamados/models.py:3298,3300`; administrativa em `app/chamados/services/validacao.py:827,833,836`; nascimento por `**campos` em `app/chamados/models.py:3124`. |
| Apenas memória | Simulação em `app/chamados/models.py:1554,1555,1556`; rollback em `:3273,3274,3275`. |
| DP | Escritor físico `DecisaoPergunta.registrar`, com lote interno. (`app/chamados/models.py:4110`; `app/chamados/models.py:4120`) |

`regularizacao_ext.py` escreve outro modelo; não pertence aos escritores da pergunta. (`app/chamados/services/regularizacao_ext.py:199`; `app/chamados/models.py:4152`)

**Leitores, filtros e transportes globais.** Os escritores acima também leem campos. Cada grupo `:n,n` abaixo indica linhas do arquivo nomeado; strings de campos/cargas são inventariadas, sem equipará-las automaticamente a julgamento.

| Arquivo | Campos e linhas |
|---|---|
| **NOVO** `app/api/views.py` | T `:1163,2561`; I `:1165,2563,2577,2620,2637`. |
| **NOVO** `app/api/views_mensageria.py` | T `:567`; I `:569,570`. |
| **NOVO** `app/chamados/canal.py` | T `:65`. |
| `app/chamados/juizes.py` | C `:292,683,711,858,1390,1502`; X `:683,695,857,1390`; I `:352,856,1389,1423,1534,1541,1552,1572,1580,1744,1753`; T `:364,857,1360,1389,1423,1534,1541,1552,1572,1580,1779,1926,2030,2035,2052`; V `:856,1389`. **NOVOS** os sítios não presentes na enumeração antiga das faces. |
| `app/chamados/models.py` | C `:1531,1923,2006,3153,3156,3166,3236,3238,3292,3293,3296,3320,3331,3365,3393,3411,3423,3440,3483,3485`; CP `:3236,3238,3293,3296,3320,3365,3393`; I `:1614,1924,2949,3155,3158,3237,3241,3243,3331,3370,3375,3376,3380,3382,3568,3584,3585,3665,4091,4098,4099,4100`; T `:1533,1614,1925,2007,2139,2950,3240,3331,3370,3375,3379,3383,3568,3584,3586,3688,3693,3697,3701,3705,3708,3709,3710,4101,4105`; V `:310,344,1614,1923,3151,3237,3244,3247,3331,3496,4091,4102,4103,4104`; VP `:1614,1629,3237,3247,4105`; X `:1535,2001,2010,2011,2136,2141,2829,2860,3165,3236,3238,3296,3311,3320,3331,3365,3392,3393,3400,3403,3411,3423,3440,3447,3483,3485,3652,3658`; M `:3299`. **NOVO** além dos métodos explicitamente citados pelo brief. |
| **NOVO** `app/chamados/reconciliador.py` | C `:367,630,1050,1145,1155,1505`; CP `:1146,1156`; T `:365,1083`; V `:631,1050`; X `:1144,1155`. |
| **NOVO** `app/chamados/services/acoes_disputa.py` | M `:83`; I `:97,132`; V `:347`; T `:355`. |
| **NOVO** `app/chamados/services/becos.py` | I `:30`. |
| **NOVO** `app/chamados/services/competencia_trancada.py` | X `:276,282`. |
| **NOVO** `app/chamados/services/declaracao_texto.py` | V `:227`; I `:417,419,687`; T `:417`. |
| **NOVO no inventário F8** `app/chamados/services/disputa_emissao.py` | C `:228,776,1084,1104,1340,1637,1930,2062,2073,2090`; I `:1483,1609,1635,1677,1692,1698,1703,1735,1843,1861,2074,2091`; T `:1483,1609,1635,1663,1677,1692,1700,1861,2079`; V `:1484,1485,1496,1537,1538,1609,1636,2062`; VP `:1484,1496,1537,1538,1609,1636`; X `:337,409,428,446,1637,2072,2073,2087,2090`; M `:1513`. |
| **NOVO** `app/chamados/services/explicador.py` | T `:55,102,103`; M `:113`; X `:118`. |
| **NOVO** `app/chamados/services/fabrica_da_celula.py` | X `:149`; C `:171`. |
| **NOVO** `app/chamados/services/fio.py` | I `:187,706,1613`; T `:377,1279,1291,1613`; M `:564,1407`; X `:1147,1214,1290,1397,1480,1921,1957,1979,2024`. |
| **NOVO** `app/chamados/services/foto_e4b.py` | C/I/V `:180`. |
| **NOVO** `app/chamados/services/historico.py` | I `:99`; V `:101`; C `:103`; T `:109`. |
| `app/chamados/services/materializacao.py` | C `:148,261,262,289,326,862,871,1015,1107,1109,1114,1153,1652,1656,2066,2079,2127`; CP `:1154`; I `:1151,2064`; T `:546,574,625,671,678,718,733,770,869,950,1047,1109,1150,1311,1352,1534,1558,1567,1619`; V `:199,201,1152,2065,2082`; VP `:1152`; X `:261,292,862,868,1108,1150,1652`; M `:1333,1652,1655,1663,1666`. **NOVO** além do materializador técnico citado. |
| **NOVO** `app/chamados/services/painel.py` | C/I `:67`; V `:67,270,271,272,273`; VP `:262`. |
| **NOVO** `app/chamados/services/passivo_validado.py` | C `:64,534,535,536`; CP `:64,535`; M/I/V/VP `:66`; X `:64,140,141,142,530,535`. |
| **NOVO** `app/chamados/services/pergunta_no_app.py` | I `:81`. |
| **NOVO** `app/chamados/services/raiox.py` | C `:135,182`; I/T `:138,147`. |
| **NOVO** `app/chamados/services/relavra_via.py` | C `:181,244,252`; X `:118,121,123,127,134,136,155,181,245,252`. |
| **NOVO** `app/chamados/services/tarefa_do_dia.py` | X `:256,257`; sequência em `:421`; projeção sem linha em `:430`. |
| `app/chamados/services/validacao.py` | C `:262,264,341,363,556,681,701,717`; CP `:342,363`; I `:594,813,824,848,1167`; T `:175,263,281,356,380,823,843,848,1094,1096,1098,1103,1104,1105,1106,1109`; V `:171,173,181,262,269,276,300,302,343,351,364,489,553,597,701,810,975,996`; VP `:171,181,262,276,300,302,343,351,364,489`; X `:341,363`; M `:181,262,266,282,299,301,302,344,364,825,833,847`. **NOVO** além da recusa administrativa antiga. |
| **NOVO** `app/chamados/services/verbos.py` | I `:335,336,342,345`; V `:264,270`; VP `:264,265,266,268`. |
| **NOVO** `app/chamados/services/vigia_carimbo.py` | V `:98`. |
| **NOVO** `app/chamados/services/vigia_fio.py` | I `:636,746`; T `:749`. |
| **NOVO** `app/chamados/templatetags/chamados_tags.py` | T `:139,166`. |
| **NOVO** `app/chamados/veredito_lavrado.py` | C `:203,204`; I `:207,208`. |
| **NOVO** `app/chamados/views.py` | C `:1257`; I `:1280,1416`. |
| **NOVO** `app/chamados/views_cobrar.py` | I `:169,176`; T `:291`. |
| `app/colaboradores/services/calendario.py` | T `:504`. |
| `app/colaboradores/services/ficha.py` | T `:171`; retenção `

[truncado em 20000 de 60460 caracteres]

## 17/11 01:19 · sessao d5206513f7f1 · PREPARA-F3-F5-F6-F8-F9 · leitura · origem=Code

## 1 DEPENDE e premissas

**A premissa central CAIU:** a F1 pousada fornece `pergunta_respondida(p)`, sobre instante **e** texto da projeção. O contrato antigo `LeituraDosAtos/leitura/_leitura_conservada` não descreve essa F1. O próprio código reserva os leitores B/C/D à B2. (`app/chamados/juizes.py:1389`; `app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1550`.)

BASE identificada no pedido: `fbc5c76ced33bf25943f3a5bc78ebff257d68c56`; confirmação independente do HEAD e da ancestralidade Git: **[nao verificado]**. Execução de testes, sondas e números atuais de produção: **[nao verificado]**.

As referências `brief.md` e `matriz.md` contam as linhas do texto anexado, incluindo vazias, desde o título; correspondência com os arquivos originais: **[nao verificado]**. A contagem abaixo usa **uma premissa por linha numerada**; ocorrências repetidas das marcas estão reunidas na mesma premissa.

| Nº | Origem | Premissa julgada | Veredito na BASE |
|---|---|---|---|
| 1 | brief:76–78; matriz:14,16–18,67,107 — `[DEPENDE-F1]` estrutural | F1 oferece `LeituraDosAtos` como contrato dos leitores | **CAIU.** O contrato pousado é o juiz booleano sobre o par projetado. (`app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1552`.) |
| 2 | matriz:19–20; brief:84–88 | O redutor F1 limpa resposta/validação conforme os atos descritos | **CAIU como interface disponível.** A projeção temporal lê os campos atuais e corta instantes futuros. (`app/chamados/juizes.py:1389`; `app/chamados/juizes.py:1391`; `app/chamados/juizes.py:1400`.) |
| 3 | brief:78,83; matriz:21–22,75,86 | Sem DP, F1 conserva por `_leitura_conservada`, inclusive P5 | **CAIU nesse formato.** A conservação disponível mede correspondência ato+instante; não constitui aquele redutor. (`app/chamados/models.py:4081`; `app/chamados/models.py:4094`; `app/chamados/models.py:4098`.) |
| 4 | brief:93–94; matriz:23–25 | As faces SQL R da F1 consultam atos | **CAIU.** Consultam instante/texto da projeção. (`app/chamados/juizes.py:1565`; `app/chamados/juizes.py:1572`; `app/chamados/juizes.py:1575`; `app/chamados/juizes.py:1580`.) |
| 5 | brief:20; matriz:26–28,35 — `[DEPENDE-F1]` #1 | `espera_o_texto` ficou metade projeção/metade ato após F1 | **CAIU.** Ambos os termos continuam projetivos. (`app/chamados/services/fio.py:187`; `app/chamados/juizes.py:1534`.) |
| 6 | matriz:29 — `[DEPENDE-F2]` | F2 já oferece `estado` pelo ato | **CAIU como suporte pousado.** `estado` continua lendo V/C/R. (`app/chamados/models.py:3151`; `app/chamados/models.py:3153`; `app/chamados/models.py:3155`.) |
| 7 | matriz:36 — `[DEPENDE-F1]` #2 | POST pode trocar R cru pela `leitura` herdada | **CAIU a interface pressuposta.** O degrau atual continua buscando instante nulo. (`app/chamados/services/fio.py:706`; `app/chamados/juizes.py:1544`.) |
| 8 | matriz:37 — `[DEPENDE-F1]`, `[DEPENDE-R3]` #3 | Canônica dispõe do texto do redutor; R3 antiga é a base obrigatória | **CAIU como dependência atual.** Canônica lê texto projetado; R3 c17 tem pouso registrado. (`app/chamados/services/fio.py:1279`; `app/docs/RELATO.md:235`.) |
| 9 | matriz:38 — `[DEPENDE-F1]` #4 | Mérito perdedor migra pelo contrato F1 descrito | **CAIU a interface.** Continua lendo `resposta_colab`. (`app/chamados/services/fio.py:1291`; `app/chamados/juizes.py:1544`.) |
| 10 | matriz:39 — `[DEPENDE-F1]` #5 | Lavrar sem turno recebe texto da `leitura` | **CAIU como suporte presente.** Guarda atual lê texto e C projetados. (`app/chamados/reconciliador.py:365`; `app/chamados/reconciliador.py:367`.) |
| 11 | matriz:40 — `[DEPENDE-F2]` #6 | Universo absolvível dispõe da face SQL V por ato | **CAIU como suporte pousado.** O filtro continua `validada_em__isnull=True`. (`app/chamados/reconciliador.py:631`; `app/chamados/models.py:3496`.) |
| 12 | matriz:41 — `[DEPENDE-F1]` #7 | Órfãs abertas podem consumir o redutor F1 antigo | **CAIU.** O universo atual filtra C/V projetados. A descrição “filtro R/C” dessa linha também caiu: não há R nesse filtro. (`app/chamados/reconciliador.py:1049`; `app/chamados/reconciliador.py:1050`.) |
| 13 | matriz:42 — `[DEPENDE-F1]` #8 | Veredito de órfã recebe texto do ato pela F1 | **CAIU a dependência satisfeita.** A guarda lê texto projetado. (`app/chamados/reconciliador.py:1083`.) |
| 14 | matriz:43 — `[DEPENDE-F1]` #9 | Fotografias da trilha podem citar campos de `LeituraDosAtos` | **CAIU o contrato pressuposto.** As fotografias atuais usam campos da pergunta. (`app/chamados/reconciliador.py:1145`; `app/chamados/reconciliador.py:1155`; `app/chamados/juizes.py:1544`.) |
| 15 | matriz:44 #10; saída:90 | C em SQL permanece na projeção | **CONFERE.** Órfãs reatáveis filtram C; a enumeração DP não tem MATERIALIZACAO. (`app/chamados/reconciliador.py:1505`; `app/chamados/models.py:4024`.) |
| 16 | matriz:45 — `[DEPENDE-F1]`, `[DEPENDE-F2]` #11 | Explicador herda resposta/estado pelo ato | **CAIU como implementação presente.** Lê `p.estado` e texto projetado. (`app/chamados/services/explicador.py:95`; `app/chamados/services/explicador.py:102`; `app/chamados/services/explicador.py:103`.) |
| 17 | brief:87,110; matriz:46,84 — `[DEPENDE-F1]`, `[DEPENDE-F2]` #12 | Validação pode ser lida pelo instante do redutor/face V disponível | **CAIU o suporte pressuposto.** A guarda atual testa V projetada. (`app/chamados/services/declaracao_texto.py:227`; `app/chamados/models.py:3496`.) |
| 18 | matriz:47 — `[DEPENDE-F1]` #13 | Já declarada/re-resposta compara a última RESPOSTA da leitura F1 | **CAIU como contrato presente.** Compara instante/texto projetados. (`app/chamados/services/declaracao_texto.py:417`; `app/chamados/services/declaracao_texto.py:419`.) |
| 19 | matriz:48 — `[DEPENDE-F1]` #14 | Aplicar declaração dispõe da resposta do redutor | **CAIU a interface.** O degrau atual decide pelo instante cru. (`app/chamados/services/declaracao_texto.py:687`.) |
| 20 | brief:33; matriz:49 #15 | Causa “c” do alcance ainda lê R cru | **CONFERE.** Não se confunde com o contador de integridade do par. (`app/chamados/services/vigia_fio.py:636`; `app/chamados/services/vigia_fio.py:737`.) |
| 21 | brief:52–54; matriz:50,89 #16 | `respondida_sem_resposta` mede o par gravado pela porta; classificação é técnica | **CONFERE a natureza documentada.** A função declara que não julga a pergunta e lê os campos escritos pela porta. Escolha técnica final da F5: **[nao verificado]**. (`app/chamados/services/vigia_fio.py:738`; `app/chamados/services/vigia_fio.py:740`; `app/chamados/services/vigia_fio.py:746`.) |
| 22 | brief:85,105–108; matriz:51,69–71,82 — `[DEPENDE-F1]` #17 | Resposta vazia deve voltar às pendentes do app | **CAIU pelo corte posterior.** Permanece fora do app; ação pertence ao admin. (`app/docs/CORTES.md:488`; `app/chamados/services/pergunta_no_app.py:81`.) |
| 23 | matriz:52 — `[DEPENDE-F1]` #18 | Beco dispõe da leitura por ato herdada | **CAIU a interface.** Visibilidade continua por instante nulo. (`app/chamados/services/becos.py:30`; `app/chamados/juizes.py:1544`.) |
| 24 | matriz:53,85 — `[DEPENDE-F1]` #19 | F1 fornece o accessor necessário ao histórico por atos | **CAIU esse suporte.** Histórico ainda gera eventos por campos; sequência DP existe separadamente. (`app/chamados/services/historico.py:98`; `app/chamados/services/historico.py:105`; `app/chamados/models.py:4056`.) |
| 25 | matriz:54; brief:87,110 — `[DEPENDE-F2]` #20 | Vigia carimbo já pode consultar V pela face F2 | **CAIU como suporte pousado.** Universo continua por V projetada. (`app/chamados/services/vigia_carimbo.py:98`; `app/chamados/models.py:3496`.) |
| 26 | brief:46,78; matriz:55 i1 | `_pela_decisao` já lê decisões | **CONFERE.** Lê o último ato para exigibilidade; isso não prova o antigo redutor F1. (`app/chamados/services/tarefa_do_dia.py:142`; `app/chamados/services/tarefa_do_dia.py:158`.) |
| 27 | brief:76; matriz:73 | Ordenação DP é `(ato_em, pk)` | **CONFERE.** (`app/chamados/models.py:4056`.) |
| 28 | brief:89,94–95; matriz:76 | C não tem ato; conserva o campo atual | **CONFERE.** Não há recuperação de instante apagado nessa projeção. (`app/chamados/models.py:4024`; `app/chamados/juizes.py:1390`; `app/chamados/juizes.py:1400`.) |
| 29 | brief:64–72 | Os arquivos F5 não são persistidores diretos de R/T/V/C | **CONFERE no censo direto.** Os declaradores chamam portas existentes; lavras usam carimbo existente. (`app/chamados/services/declaracao_texto.py:419`; `app/chamados/services/declaracao_texto.py:693`; `app/chamados/reconciliador.py:406`.) |
| 30 | brief:64–72 | O inventário antigo esgota os escritores; atos correspondentes já cobrem toda escrita | **CAIU.** Faltavam nascimento, ressuscitação e devolução BUG-PAR; `carimbar(resposta=)` pode preencher R/V sem registrar os respectivos atos nesse corpo. (`app/chamados/models.py:3124`; `app/chamados/models.py:1629`; `app/chamados/services/validacao.py:349`; `app/chamados/models.py:3242`; `app/chamados/models.py:3262`.) |
| 31 | brief:45–46; matriz:57–58 | Furo sem canal/competência trancada não têm migração R/T/V/C própria identificada | **CONFERE para os corpos examinados.** Furo lê tarefa; competência conserva via. (`app/chamados/services/furo_sem_canal.py:94`; `app/chamados/services/competencia_trancada.py:276`; `app/chamados/services/competencia_trancada.py:282`.) |
| 32 | brief:41–46; matriz:55–56,94–95 | Tarefa, cobrança, consulta e consumidores externos herdam a partição | **CONFERE.** (`app/chamados/services/tarefa_do_dia.py:530`; `app/chamados/services/cobranca.py:48`; `app/core/views.py:253`; `app/api/views.py:998`; `app/api/views.py:2538`.) |
| 33 | brief:47–50; matriz:60 | F/VIA/X permanecem fora da migração R/T/V/C descrita | **CONFERE como separação dos campos.** Há leituras próprias de via, pendência, recusa e fechamento. (`app/chamados/services/explicador.py:108`; `app/chamados/services/explicador.py:113`; `app/chamados/services/explicador.py:118`; `app/chamados/services/becos.py:27`.) |
| 34 | brief:50; matriz:60 | A guarda conservada ainda é `recusa_respondida_sem_texto`, unilateral, para E4d | **CAIU essa descrição.** Agora é `recusa_par_ilegal`, simétrica, sobre o par que será escrito. (`app/chamados/services/fio.py:1603`; `app/chamados/services/fio.py:1613`; `app/chamados/services/fio.py:1618`.) |
| 35 | brief:126,154–155; matriz:6–9,92 | R3 histórica ainda é pré-requisito não pousado | **CAIU segundo o registro local.** R3 c17 tem pouso registrado. Ancestralidade independente: **[nao verificado]**. (`app/docs/RELATO.md:235`.) |
| 36 | brief:127,154–155; matriz:6–7,92 | F1 antiga ainda é a entrega a aguardar | **CAIU.** B1 real tem pouso registrado e contrato diferente. (`app/docs/RELATO.md:174`; `app/chamados/juizes.py:1544`.) |
| 37 | brief:128; matriz:108 — resumo `[DEPENDE-F2]` | F2 antiga fornece o suporte V/estado pressuposto | **CAIU como descrição da BASE.** Permanecem campos projetados; a B2 aparece aberta. (`app/chamados/models.py:3151`; `app/chamados/models.py:3496`; `app/docs/RELATO.md:30`.) |
| 38 | brief:129; matriz:7 | BUG-PAR ainda está fora do main | **CAIU segundo registro local.** Pousou c4+c4c. (`app/docs/RELATO.md:32`.) |
| 39 | brief:129–132 | BUG-PAR cruza explicador/fio | **CONFERE**, com alcance maior: testemunha, indisponibilidade e espera declarado×gravado. (`app/chamados/services/fio.py:376`; `app/chamados/services/fio.py:384`; `app/chamados/services/explicador.py:124`; `app/chamados/services/explicador.py:132`.) |
| 40 | brief:12–15,58–62; matriz:8–10 | O censo antigo e seus offsets bastam para esta BASE | **CAIU como inventário vigente.** Há novos consumidores e mudanças nos corpos. Os diffs históricos exatos: **[nao verificado]**. (`app/chamados/services/pergunta_no_app.py:100`; `app/chamados/services/raiox.py:147`; `app/chamados/services/explicador.py:124`.) |
| 41 | brief:136–138; matriz:98–102,110–111 | A pergunta própria F5 está resolvida, ficando zero pergunta própria de lei | **CONFERE após o corte.** A pergunta geral de devolver ao colaborador permanece expressamente não decidida. (`app/docs/CORTES.md:488`.) |
| 42 | brief:150–152 | A mudança de badge/fase pode ser presumida coberta sem conferir conservação pública | **CAIU como dispensa dessa conferência.** D05 conserva fases públicas e torna exigibilidade aditiva; o corte F5 conserva resposta vazia fora do app. (`app/docs/BACKLOG.md:2091`; `app/docs/CORTES.md:488`.) |
| 43 | brief:132,154–155; matriz:92–93 | A ordem histórica representa todos os pousos ainda pendentes | **CAIU como fila atual.** R3/B1/BUG-PAR têm pouso registrado; B2 está aberta. Situação do hash RELAVRA-11 citado: **[nao verificado]**. (`app/docs/RELATO.md:235`; `app/docs/RELATO.md:174`; `app/docs/RELATO.md:32`; `app/docs/RELATO.md:30`.) |
| 44 | brief:139–146; matriz:77–78,91 | A aproximação histórica serve como medida atual pela função real | **CAIU como medição atual.** A autoridade real disponível de correspondência é outra; valores atuais e antiga comparação por fold: **[nao verificado]**. (`app/chamados/models.py:4081`; `app/chamados/juizes.py:1544`.) |
| 45 | brief:156–157 | Publicação de código exige rito de suíte/deploy e cascas juntas | **CONFERE como regra operacional escrita**, sem execução nesta leitura. (`CLAUDE.md:90`; `CLAUDE.md:91`; `CLAUDE.md:95`.) | [nao verificado]

Os hashes/diffs históricos de RELAVRA-11, LOTE7 e UI-CAMERA não foram demonstrados pela cópia. Suas afirmações históricas de interseção/ausência de interseção permanecem **[nao verificado]**; não foram convertidas em confirmação de Git.

DEPENDE CAIU 32 de 45

## 2 Censo escritores e leitores

R=`respondida_em`; T=`resposta_colab`; V=`validada_em`; C=`materializada_em`. O levantamento combinou AST de atributos, atribuições, keywords ORM, strings de campos e chamadas/aliases, com grep de templates. Cobertura de nomes inteiramente construídos em execução: **[nao verificado]**. A distinção entre projeção e sequência está documentada no modelo. (`app/chamados/models.py:4007`; `app/chamados/models.py:4011`.)

**NOVO** significa ausente da enumeração do brief, inclusive novo sítio dentro de arquivo já citado.

**Escritores e conservação**

| Campo/objeto | Escritores encontrados |
|---|---|
| R/T — humana | `responder_pergunta_disputa_core`: atribuição/persistência e RESPOSTA. (`app/chamados/services/disputa_emissao.py:1481`; `app/chamados/services/disputa_emissao.py:1483`; `app/chamados/services/disputa_emissao.py:1525`.) |
| R/T — automática | `responder_automaticamente`: conserva anterior, escreve par e RESPOSTA. (`app/chamados/services/disputa_emissao.py:1673`; `app/chamados/services/disputa_emissao.py:1674`; `app/chamados/services/disputa_emissao.py:1678`.) |
| R — trilha | `carimbar_resposta_pela_trilha`. (`app/chamados/services/disputa_emissao.py:1696`; `app/chamados/services/disputa_emissao.py:1699`.) |
| V/autor | Validação; nova resposta; devolução; **NOVO** devolução BUG-PAR. (`app/chamados/services/validacao.py:168`; `app/chamados/services/disputa_emissao.py:1494`; `app/chamados/services/validacao.py:273`; `app/chamados/services/validacao.py:349`.) |
| R/T/V por lote | Reabertura fotografa e passa zeragem à porta; **NOVO** ressuscitação conserva anterior e zera o par inteiro. (`app/chamados/services/disputa_emissao.py:1609`; `app/chamados/services/disputa_emissao.py:1631`; `app/chamados/services/disputa_emissao.py:1648`; `app/chamados/models.py:1613`; `app/chamados/models.py:1629`; `app/chamados/models.py:1630`.) |
| **NOVO** R/V por carimbo | `carimbar(resposta=)` pode preencher R/V; o corpo salva sem registrar RESPOSTA/VALIDACAO correspondentes. (`app/chamados/models.py:3240`; `app/chamados/models.py:3242`; `app/chamados/models.py:3245`; `app/chamados/models.py:3262`.) |
| C/autor/via | `_escrever_carimbo` atribui em memória; portas e lote persistem. (`app/chamados/models.py:3209`; `app/chamados/models.py:3210`; `app/chamados/models.py:3211`; `app/chamados/models.py:3262`; `app/chamados/models.py:3299`; `app/chamados/models.py:3322`; `app/chamados/models.py:3393`.) |
| **NOVO** nascimento | `_nascer` recebe `**campos`, salva e conserva projeção sem linha. (`app/chamados/models.py:3124`; `app/chamados/models.py:3127`; `app/chamados/models.py:3129`.) |
| Apenas memória | Simulação da ressuscitação e reversão do carimbo. (`app/chamados/models.py:1554`; `app/chamados/models.py:1555`; `app/chamados/models.py:1556`; `app/chamados/models.py:3273`; `app/chamados/models.py:3274`.) |
| Homônimo excluído | `regularizacao_ext.py:199` escreve outro modelo; não C da pergunta. (`app/chamados/services/regularizacao_ext.py:199`; `app/chamados/models.py:4152`.) |
| DP — persistidor | `DecisaoPergunta.registrar`, com `bulk_create` interno. (`app/chamados/models.py:4110`; `app/chamados/models.py:4120`.) |

**Produtores e leitores da sequência**

| Papel | Sítios |
|---|---|
| Produtores DP no model | Conservação/nascimento, SILENCIO e RETRATACAO. (`app/chamados/models.py:1613`; `app/chamados/models.py:2005`; `app/chamados/models.py:2137`; `app/chamados/models.py:3129`.) |
| Produtores DP nos serviços | Resposta/reabertura/automática/trilha; validação/devoluções; absorção. (`app/chamados/services/disputa_emissao.py:1525`; `app/chamados/services/disputa_emissao.py:1631`; `app/chamados/services/disputa_emissao.py:1678`; `app/chamados/services/disputa_emissao.py:1699`; `app/chamados/services/validacao.py:173`; `app/chamados/services/validacao.py:279`; **NOVO** `app/chamados/services/validacao.py:354`; `app/chamados/services/materializacao.py:1045`.) |
| Outros produtores DP | Veredito de célula e backfills. (`app/ponto/services/veredito_celula.py:195`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:324`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:141`.) |
| Leitores DP | Respostas anteriores/correspondência; prefetch; tarefa; foto; medidor. (`app/chamados/models.py:3675`; `app/chamados/models.py:4094`; `app/chamados/services/fio.py:263`; `app/chamados/services/tarefa_do_dia.py:159`; `app/chamados/services/tarefa_do_dia.py:178`; `app/chamados/services/tarefa_do_dia.py:421`; `app/chamados/services/tarefa_do_dia.py:430`; `app/chamados/services/foto_e4b.py:280`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:67`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:99`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:315`.) |
| **NOVO** leitor BUG-PAR | Rótulo consulta decisões. (`app/chamados/services/materializacao.py:275`.) |

**Censo global dos campos em produção**

Cada número nas colunas abaixo significa `arquivo da primeira coluna:linha`. Inclui leitura, escrita, filtros e transporte; os escritores estão classificados acima. Os sítios exteriores aos 14 arquivos do brief ampliam o censo; não implicam alteração automática pela F5.

| Arquivo | R | T | V | C |
|---|---|---|---|---|
| `app/chamados/services/fio.py` | 187,706,1613 | **NOVO** 377; 1279,1291,1613 | — | — |
| `app/chamados/reconciliador.py` | — | 365,1083 | 631,1050 | 367,630,1050,1145,1155,1505 |
| `app/chamados/services/explicador.py` | — | 55,102,103 | — | — |
| `app/chamados/services/declaracao_texto.py` | 417,419,687 | 417 | 227 | — |
| `app/chamados/services/vigia_fio.py` | 636,746 | 749 | — | — |
| `app/chamados/services/raiox.py` | 138,147 | 138,147 | — | 135,182 |
| `app/chamados/services/pergunta_no_app.py` | 81 | — | — | — |
| `app/chamados/services/becos.py` | 30 | — | — | — |
| `app/chamados/services/historico.py` | 99 | 109 | 101 | 103 |
| `app/chamados/services/vigia_carimbo.py` | — | — | 98 | — |
| `app/chamados/models.py` | 1555,1614,1629,1924,2949,3155,3158,3237,3241,3242,3243,3273,3331,3370,3375,3376,3380,3382,3568,3584,3585,3665,4091,4098,4099,4100 | 1533,1556,1614,1630,1925,2007,2139,2950,3240,3331,3370,3375,3379,3383,3568,3584,3586,3688,3693,3697,3701,3705,3708,3709,3710,4101,4105 | 310,344,1554,1614,1629,1923,3151,3237,3244,3245,3247,3274,3331,3496,4091,4102,4103,4104 | 1531,1923,2006,3153,3156,3166,3209,3236,3238,3292,3293,3296,3320,3331,3365,3393,3411,3423,3440,3483,3485 |
| `app/chamados/juizes.py` | 

[truncado em 20000 de 65829 caracteres]

_6302 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
