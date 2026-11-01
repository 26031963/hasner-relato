# CODEX-ESTADO

> Ultima resposta processada: **01/11/2026 17:51** · publicado em **01/11/2026 17:52** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 01/11 17:51 · sessao 06c9e96978f9 · O261-E-F2 · verificador · origem=Code

**A cobertura é parcial:** os quatro testes podem continuar verdes sem a guarda do fecho; o MORDE do ramo não chega ao vigia; e o caso 2 usa um veredito que a porta de produção rejeita. — `app/chamados/services/disputa_emissao.py:1626`; `app/chamados/models.py:2569`; `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:171`; `app/ponto/portas/celula.py:658`.

Esta é uma análise estática da cópia. Execução dos testes, resultados dos logs citados no cabeçalho, suíte completa e correspondência Git com `7af133ad`/`cd623758`: **[não verificado]**.

Para compactar as referências, os caminhos abaixo são relativos a `app/`, salvo `CLAUDE.md`.

**1. O que os quatro testes provam** [nao verificado]

- **T1 — `test_ramo_variante…`:** chama o C6 real, `resolver_perguntas_por_chamado_resolvido` real e `censo_alcance` real. Entretanto, `_fechar` sobrescreve novamente o estado da célula depois da função de produção. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:155`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:163`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:181`.
  
  As asserções medem: C6 vivo antes; retorno dos contadores igual a `{0,0}`; `materializada_em` nulo; `via_resolucao` vazia; C6 vivo depois; exatamente uma linha daquela célula; rota `sem_porta`; detalhe `pergunta_viva_sob_chamado_terminal`; universo e dias sem porta iguais a `(2,2)`. **O índice 6 verificado é o detalhe; a causa está no índice 5 e não é verificada.** — `chamados/tests/test_o261e_f2_ramo_e_i20.py:184`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:168`; `chamados/services/vigia_fio.py:585`.

  O contador `perguntas_resolvidas=0` não prova que o ramo sem credor deixou de carimbar: `n_pq` é incrementado no primeiro ramo, com `chamado_destino`; o ramo sem credor não o incrementa. A pergunta da fixture tem destino nulo. — `chamados/services/disputa_emissao.py:1537`, `chamados/services/disputa_emissao.py:1569`, `chamados/services/disputa_emissao.py:1638`, `chamados/services/disputa_emissao.py:1661`; `chamados/tests/test_o261e_f2_ramo_e_i20.py:142`.

- **T2 — `test_MORDE_ramo…`:** chama o fecho real com `pergunta_viva` substituído por `False`; exige que tenha nascido materialização e que `_assert_ramo` lance `AssertionError`. Isso demonstra rejeição do carimbo pela primeira asserção do helper. **Não demonstra uma leitura adversarial do vigia**, porque o helper aborta antes dela. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:193`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:196`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:171`.

- **T3 — `test_caso2_dia_regularizado…`:** chama C6 real, fecho real, vigia real e auditor real por `call_command`. Verifica C6 morto inicialmente; manutenção de `('07:55', None, None)` em texto/respondida/materializada; PK fora da cicatriz; ausência da célula no alcance; totais `(1,1)`; I20 com `VIOL 1`; e presença da representação textual do PK na linha. Não verifica o retorno do fecho nem o código de saída do auditor. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:207`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:202`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:79`.

- **T4 — `test_MORDE_caso2…`:** chama o fecho real, executa uma cópia integral do auditor com o PK acrescentado à cicatriz e depois executa o comando real. Verifica âncora textual única; cópia com `OK 0`; rejeição dessa saída pelo helper que exige `VIOL 1`; e comando real contando novamente a pergunta. O `assertRaises` da cópia é satisfeito pela divergência de flag/total, antes da checagem do PK. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:222`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:204`.

Os helpers também exigem uma única atribuição AST de `_I20_CICATRIZ` e uma única linha I20 reconhecível na saída. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:65`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:72`.

**Mutantes concretos, sem aplicá-los**

| Mutante | Alteração de produção de 1–3 linhas |
|---|---|
| **F — desligar guarda do fecho** | Trocar `if pergunta_viva(q):` por `if False:`. — `chamados/services/disputa_emissao.py:1626` |
| **P — desligar recusa da porta** | Trocar `if viva:` por `if False:`. — `chamados/models.py:2569` |
| **Z — silenciar I20** | Substituir o queryset passado a `add('I20',…)` por `PD.objects.none()`. — `chamados/management/commands/auditar_invariantes_chamados.py:1101` |
| **C — remover exclusão da cicatriz** | Remover `.exclude(pk__in=_I20_CICATRIZ)`. — `chamados/management/commands/auditar_invariantes_chamados.py:1103` |
| **R — remover filtro do instante** | Trocar `PD.objects.filter(respondida_em__isnull=True)` por `PD.objects.all()`, preservando as exclusões. — `chamados/management/commands/auditar_invariantes_chamados.py:1102` |

Veredictos **previstos por leitura estática**, supondo a fixture isolada, PK fora da cicatriz e execução-base verde; execução efetiva: **[não verificado]**. Os pontos de observação são T1/T2/T3/T4 acima. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:181`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:188`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:207`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:222`.

| Mutante | T1 ramo | T2 MORDE ramo | T3 caso 2 | T4 MORDE I20 |
|---|---:|---:|---:|---:|
| F | Verde | Verde | Verde | Verde | [nao verificado]
| P | Verde | Verde | Vermelho | Vermelho | [nao verificado]
| F + P | Vermelho | Verde | Vermelho | Vermelho | [nao verificado]
| Z | Verde | Verde | Vermelho | Vermelho | [nao verificado]
| C | Verde | Verde | Verde | Vermelho | [nao verificado]
| R | Verde | Verde | Verde | Verde | [nao verificado]

A redundância explica F e P: sem F, a porta aplica provisoriamente o carimbo e `respondida_em`, consulta C6 e desfaz; sem P, T1 continua pulando antes da porta. T3 alcança a porta e denuncia a remoção de sua recusa. — `chamados/services/disputa_emissao.py:1626`; `chamados/models.py:2555`, `chamados/models.py:2565`, `chamados/models.py:2569`; `chamados/tests/test_o261e_f2_ramo_e_i20.py:214`.

R sobrevive porque não existe controle negativo com texto **e** `respondida_em` preenchido; a fixture oferece apenas o caso positivo do meio-estado. Portanto os testes não prendem integralmente o predicado do I20. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:143`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:214`; `chamados/management/commands/auditar_invariantes_chamados.py:1102`.

**2. Quem escreve e quem lê**

| Campo | Escrita de produção | Leituras de produção relevantes |
|---|---|---|
| `PerguntaDisputa.resposta_colab` | Resposta pelo core, gravação automática e limpeza na reabertura. — `chamados/services/disputa_emissao.py:1247`, `chamados/services/disputa_emissao.py:1393`, `chamados/services/disputa_emissao.py:1358` | C6, respostas sem veredito, materializador e I20. — `chamados/juizes.py:1047`; `chamados/models.py:1767`; `chamados/services/materializacao.py:763`; `chamados/management/commands/auditar_invariantes_chamados.py:1101` |
| `PerguntaDisputa.respondida_em` | Escritores do par acima; recuperação pela trilha; `carimbar` quando solicitado e com texto; limpeza na ressuscitação. — `chamados/services/disputa_emissao.py:1408`; `chamados/models.py:2556`, `chamados/models.py:1488` | C6, `.estado`, respostas sem veredito e I20. — `chamados/juizes.py:1047`; `chamados/models.py:2477`, `chamados/models.py:1769`; `chamados/management/commands/auditar_invariantes_chamados.py:1102` |
| `PerguntaDisputa.materializada_em` | `_escrever_carimbo`, persistido pelas portas `carimbar`, `carimbar_identidade`, `descarimbar` e `descarimbar_em_lote`. — `chamados/models.py:2527`, `chamados/models.py:2533`, `chamados/models.py:2588`, `chamados/models.py:2609`, `chamados/models.py:2619` | C6, seleção do fecho, respostas sem veredito e `.estado`. — `chamados/juizes.py:1071`, `chamados/juizes.py:1077`; `chamados/services/disputa_emissao.py:1620`; `chamados/models.py:1768`, `chamados/models.py:2480` |
| `PerguntaDisputa.via_resolucao` | Portas do carimbo; `encerrar_por_fato_no_espelho` possui operação explícita que troca apenas a via. — `chamados/models.py:2527`, `chamados/models.py:2629` | C6 e seleção/trilha do fecho. — `chamados/juizes.py:1042`, `chamados/juizes.py:1071`; `chamados/services/disputa_emissao.py:1628` |
| `CelulaDia.veredito` | `CelulaDia.lavrar_veredito` delega à porta da célula. — `escala/models.py:1787`; `ponto/portas/celula.py:770` | C6 e retenção da folha, que fornece o universo do vigia. — `chamados/juizes.py:1081`; `folha/export.py:1215`; `chamados/services/vigia_fio.py:545` |
| `CelulaDia.veredito_via` | Mesma porta de lavra. — `ponto/portas/celula.py:771` | C6 distingue decisão do admin; a própria lavra preserva sua precedência. — `chamados/juizes.py:1045`; `ponto/portas/celula.py:738` |
| `CelulaDia.ata` | Porta de lavra; chave `precedencia` por `carimbar_precedencia`. — `ponto/portas/celula.py:768`, `ponto/portas/celula.py:811` | Fato do marco, concordância com a luz e marcos pendentes usados pelo C6. — `chamados/juizes.py:887`, `chamados/juizes.py:1008`, `chamados/juizes.py:1094` |
| `DisputaSupervisao.fechada_em` | `fechar`, `reabrir_admin`, `reabrir_sistema`. — `chamados/models.py:1845`, `chamados/models.py:2047`, `chamados/models.py:2071` | Seleção/fechamento do serviço, badge e signal de materialização. — `chamados/services/disputa_emissao.py:1608`, `chamados/services/disputa_emissao.py:1658`; `chamados/models.py:1721`; `chamados/signals.py:130` |
| `ChamadoColaborador.status_local` | Transições por `_gravar_estado`; nascimento por `abrir`/`criar`. — `chamados/models.py:195`, `chamados/models.py:222`, `chamados/models.py:897`, `chamados/models.py:1046` | Credores do fecho, vigia e signal de rejulgamento da célula. — `chamados/services/disputa_emissao.py:1613`, `chamados/services/disputa_emissao.py:1616`; `chamados/services/vigia_fio.py:569`, `chamados/services/vigia_fio.py:578`; `chamados/signals.py:169` |

Há três distinções importantes na montagem:

- **Meio-estado injetado:** o teste escreve só `resposta_colab` por `QuerySet.update`; o core normal grava texto e instante juntos, e a reabertura limpa ambos. Isso é uma entrada adversarial para o tripwire, mas não prova um caminho atual que a produza. O auditor documenta cicatrizes históricas; ocorrência atual em produção: **[não verificado]**. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:143`; `chamados/services/disputa_emissao.py:1247`, `chamados/services/disputa_emissao.py:1358`; `chamados/management/commands/auditar_invariantes_chamados.py:1089`.

- **Estado exato inválido no caso 2:** `_cenario('ok', ACESA)` acaba gravando `veredito='ok', veredito_via='cartorio'` diretamente. `ok` não pertence a `VEREDITOS_CARTORIO`; a porta lança `ValueError`. A lavra sem códigos produz `concorde`. Portanto esse estado exato **não sai da porta canônica**. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:209`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:152`; `escala/models.py:1719`, `escala/models.py:1596`; `ponto/portas/celula.py:658`.

- **Resultado da célula sobrescrito:** `_fechar` chama produção e depois restaura veredito/ata por `update`, sem passar pela lavra que coordena instante, protestos, status e evento. O vigia mede essa célula imposta pelo teste, sem comprovar o resultado integral deixado pela função de produção. O chamado também nasce já resolvido, sem testar sua transição; não há asserção direta posterior de `status_local` ou `fechada_em`. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:135`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:152`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:157`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:185`; `ponto/portas/celula.py:770`.

**3. Testemunha e universo do auditor**

O `mock.patch` cobre somente `_fechar`. Já terminou durante o `refresh_from_db`, a asserção de materialização e o `assertRaises`. Contudo, `_assert_ramo` encontra o carimbo e falha em sua primeira asserção, antes do C6 real e antes de `censo_alcance`. **Nenhuma leitura do vigia acontece nesse MORDE, sob ou depois do patch.** — `chamados/tests/test_o261e_f2_ramo_e_i20.py:193`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:195`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:171`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:174`.

A cópia textual executa a mesma implementação de `Command.handle`, com os mesmos defaults relevantes: `amostra=5`, `html=False`. O conjunto candidato do I20 é global: perguntas com instante nulo e texto diferente de `''`, sem filtro por empresa, data, disputa ou materialização. **O conjunto final difere intencionalmente pela exclusão adicional de um PK.** `add` conta tudo e limita apenas a amostra. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:90`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:228`; `chamados/management/commands/auditar_invariantes_chamados.py:549`, `chamados/management/commands/auditar_invariantes_chamados.py:559`, `chamados/management/commands/auditar_invariantes_chamados.py:1101`.

A cópia chama `handle` diretamente; o controle chama `call_command`. Logo, a consulta I20 é comparável, mas a cópia não exercita toda a infraestrutura de despacho do comando. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:85`.

**4. Relógio, empresas, PK e formato**

- **Relógio:** há datas literais, `hoje` explícito para o vigia e `freeze_time` na classe. Fecho e lavra leem `now`; I20 não tem predicado temporal. Não há evidência suficiente para acusar um vazamento concreto de relógio. Cobertura efetiva do `setUpTestData` e de todas as dependências pelo decorador: **[não verificado]**. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:43`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:96`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:163`; `chamados/services/disputa_emissao.py:1623`; `ponto/portas/celula.py:772`; `chamados/management/commands/auditar_invariantes_chamados.py:1101`.

- **`empresas_visiveis`: sim, depende.** Passar `[self.emp]` faz interseção com as visíveis; não substitui essa fonte. A empresa criada satisfaz o filtro atual de ativa e CNPJ fora do prefixo ocultado. O carregador da lista repete a interseção. — `chamados/services/vigia_fio.py:535`, `chamados/services/vigia_fio.py:210`; `colaboradores/queries.py:85`; `chamados/tests/test_o261e_f2_ramo_e_i20.py:104`.

- **PK/sequência:** o PK é automático, sem reserva explícita fora da cicatriz. T3 detecta colisão por `assertNotIn`; T4 detecta ao exigir que o comando real conte no final. Assim, colisão pode causar vermelho sem regressão semântica; não produz o verde esperado silenciosamente. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:141`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:215`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:245`; `chamados/management/commands/auditar_invariantes_chamados.py:1098`.

- **Formato:** o offset fixo `68` corresponde ao formato atual. O comando trunca a descrição em 58 caracteres, não a linha nem a amostra de PKs. Linha ausente, duplicada ou `ERR` provoca falha; mudança de layout também pode quebrar o parser. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:69`; `chamados/management/commands/auditar_invariantes_chamados.py:1833`.

- **Identidade fraca:** `assertIn(str(p.pk), ln)` busca substring na linha inteira; não verifica o PK como elemento da amostra e aceita coincidências parciais. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:205`.

**5. Juiz, escritor ou derivação próprios; selos da casa**

Não há predicado novo de vitalidade ou retenção: o teste lê C6, chama o censo real e seleciona sua linha por célula. Os helpers AST e de saída interpretam configuração/protocolo. Há, porém, escrita própria da fixture sobre campos de estado e reposição da célula depois do fecho. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:59`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:69`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:143`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:152`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:163`.

**Não cabe afirmar que os selos estáticos reprovariam esse arquivo:** os censos de escritores e os contratos de par resposta, porta da célula e carimbo excluem testes. As escritas seriam problemáticas como produção, mas essa exclusão impede atribuir-lhes um vermelho automático neste teste. — `core/censo_escritas.py:66`; `chamados/tests/test_contract_par_resposta.py:54`; `ponto/tests/test_selo_veredito_so_pela_porta.py:173`; `chamados/tests/test_o261e_f2_porta_carimbo.py:208`.

A insuficiência sustentada pelas regras da casa é precisa: a mordida denuncia um carimbo, mas não executa a leitura adversarial do vigia; e nenhuma das quatro provas prende isoladamente a guarda do fecho. Isso limita a alegação de proteção da costura, diante da exigência de função real e caso que morde. — `CLAUDE.md:318`; `chamados/tests/test_o261e_f2_ramo_e_i20.py:171`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:198`; `chamados/services/disputa_emissao.py:1626`; `chamados/models.py:2569`.

## 01/11 17:49 · sessao bcdff4d29137 · - · - · origem=Ronald (tmux)

Q3 — **mock fora da testemunha, mas vigia não é alcançado no MORDE**. O patch cobre somente `_fechar` (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:193`); já terminou quando ocorre `refresh_from_db` e a asserção de materialização (`:195`, `:196`). `_assert_ramo` começa recarregando o objeto e exigindo `materializada_em is None` (`:170`, `:171`), condição incompatível com a asserção imediatamente anterior do MORDE (`:196`). Assim, o `assertRaises` é satisfeito em `:171`; não chega ao C6 de `:173`, ao censo de `:174`, nem às suas asserções `:175-179`. O MORDE demonstra que o selo rejeita o carimbo indevido, mas não executa o vigia nesse caminho (`:196-198`).

A cópia do auditor mede **o mesmo universo I20**, com apenas a cicatriz ampliada: copia o módulo inteiro, exige uma ocorrência da âncora e modifica essa ocorrência (`:228-239`); ambos usam `amostra=5`, `html=False` (`:86`, `:90`; `app/chamados/management/commands/auditar_invariantes_chamados.py:549-550`). O I20 real usa todas as `PerguntaDisputa` com `respondida_em` nulo e resposta diferente de `''`, excluindo a tupla; não filtra empresa, dia, estado de disputa ou materialização (`auditar_invariantes_chamados.py:1101-1103`). `add` conta o queryset inteiro e limita somente a amostra (`:559-566`). A via da cópia chama `handle` diretamente, portanto não testa a infraestrutura completa de `call_command`, mas a consulta I20 executada é a mesma (`teste:85-90`).

Q4 — O cenário usa datas fixas e passa `hoje` explicitamente ao censo (`teste:43-47`, `:163`); a classe é decorada com `freeze_time` (`:96`). Há leituras de relógio no fecho e carimbo (`app/chamados/services/disputa_emissao.py:1623`; `app/chamados/models.py:2550`), no auditor (`auditar_invariantes_chamados.py:635`, `:1589`) e na lavra feita por `setUpTestData` (`teste:119`, `:125`; `app/ponto/portas/celula.py:772`). A implementação de freezegun não está na cópia; a cobertura efetiva de `setUpClass/setUpTestData` pelo decorador não foi demonstrada nesta leitura **[não verificado]**. Não há evidência suficiente para acusar concretamente um relógio fora do freeze; o pin disponível é `freezegun==1.5.5` (`app/requirements.txt:43`).

Existe dependência de `empresas_visiveis` mesmo passando `empresas=[self.emp]`: o censo obtém as visíveis e depois faz interseção (`app/chamados/services/vigia_fio.py:535-538`); o carregador da lista repete essa política (`:210-215`). A fixture satisfaz o filtro atual, pois cria empresa ativa e CNPJ fora do prefixo ocultado (`teste:104-105`; `app/colaboradores/queries.py:85-87`).

O pk não é escolhido fora da cicatriz: nasce por `objects.create` (`teste:141-142`). O teste normal detecta colisão e falha explicitamente (`:215`); no MORDE, a chamada final ao comando real também exigirá contagem 1 e falhará se o caso já estiver na cicatriz (`:245`, `:204`). Portanto é dependência de sequência, protegida contra falso verde por falha explícita, mas sem isolamento da sequência no próprio arquivo (`:97-99`, `:141-142`, `:215`). [nao verificado]

O parser corresponde ao formato atual: exige exatamente uma linha I20 (`teste:71-72`); o offset `68` de `:75` coincide com flag de quatro caracteres + espaço, id de quatro + espaço e descrição de 58 (`auditar_invariantes_chamados.py:1835-1836`). A descrição é truncada a 58; a amostra de pks não é truncada nessa formatação (`:1835-1836`). Mudança no layout ou linha `ERR` torna o parser falho por exceção, sem falso verde (`teste:75`). A identificação do pk é apenas substring (`:205`), portanto é mais fraca que validar o elemento da amostra; pode coincidir com outro trecho ou pk maior (`:205`; formato em `auditor:1835-1836`). [nao verificado]

Q5 — Os helpers consultam o C6 real e o censo real; filtram o resultado por `cel.pk`, sem construir um predicado próprio de vitalidade ou retenção (`teste:163-164`, `:169-179`). `_cicatriz_i20` lê o AST e `_linha_i20` interpreta o protocolo de saída; nenhum deles decide o estado de produção (`:59-76`). A cópia mutante constitui a própria implementação do auditor com uma exclusão alterada, acompanhada pela execução real como controle (`:228-245`). [nao verificado]

Há **escritas diretas de fixture fora das portas** em resposta e estado da célula (`teste:143`, `:152-153`), inclusive reposição da ata após o fecho (`:157-158`); isso limita a prova de integração do efeito do fecho sobre a célula. Não equivale automaticamente a escritor novo de produção: o censo exclui `tests/` e arquivos `test_*.py` (`app/core/censo_escritas.py:66-82`), e o selo P13 faz o mesmo (`app/core/tests/test_contract_escritores_estado.py:189-192`). O gate de célula usa esse censo (`app/ponto/tests/test_chokepoint_celula_gate.py:49-53`). Não afirmar que esses selos reprovariam o arquivo por esses updates.

A crítica amparada pela casa é a limitação da mordida: exige-se função real e caso que denuncie a costura rompida (`CLAUDE.md:318-325`), e a testemunha deve ler a autoridade (`CLAUDE.md:20-22`, `:35-38`). A mordida do ramo efetivamente denuncia o carimbo, mas não chega a medir a costura do vigia (`teste:171`, `:174`, `:196-198`). Resultado de execução dos selos/suíte: **[não verificado]**. [nao verificado]

## 01/11 17:49 · sessao 60d6079d4737 · - · - · origem=Ronald (tmux)

Análise estática; resultados reais da suíte e de mutações executadas permanecem **[não verificado]**. [nao verificado]

**Chamadas e asserções**

- **T1, ramo**: chama `resolver_perguntas_por_chamado_resolvido` por `_fechar`; verifica C6 vivo antes, retorno `{0,0}`, ausência de materialização/via, C6 vivo depois, uma linha da célula, rota `sem_porta`, **detalhe** `pergunta_viva_sob_chamado_terminal` e totais `(2,2)`. O índice 6 é detalhe, não causa. Fontes: `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:155`, `:168`, `:181`; estrutura da linha em `app/chamados/services/vigia_fio.py:585`.
- **T2, MORDE ramo**: chama o mesmo fecho com C6 falso, exige materialização e exige que `_assert_ramo` falhe. **A falha ocorre na primeira asserção, `assertIsNone(materializada_em)`; portanto não executa C6 real nem `censo_alcance` nessa contraprova**. Fontes: teste `:168`, `:188`. [nao verificado]
- **T3, caso2**: chama C6 real, fecho real, vigia real e comando real por `call_command`. Verifica C6 morto inicialmente, tripla texto/timestamps intacta, PK fora da cicatriz, nenhuma linha do dia, totais `(1,1)`, I20 `VIOL 1` e substring do PK na linha. Não verifica retorno do fecho nem RC do auditor. Fontes: teste `:79`, `:202`, `:207`. [nao verificado]
- **T4, MORDE I20**: chama fecho real, executa cópia integral do comando com PK adicional na cicatriz e depois comando real. Verifica âncora textual única; cópia `OK 0`; rejeição pelo helper que exige `VIOL 1`; comando real `VIOL 1` e presença textual do PK. Fontes: teste `:222`, `:202`. [nao verificado]

**Matriz de mutantes de produção — inferência estática**

V = passaria; F = falharia, considerando o universo isolado esperado pela fixture. As referências dos efeitos são os testes acima, o fecho em `app/chamados/services/disputa_emissao.py:1624`, a porta em `app/chamados/models.py:2554` e I20 em `app/chamados/management/commands/auditar_invariantes_chamados.py:1098`.

| Mutante concreto | T1 | T2 | T3 | T4 |
|---|---:|---:|---:|---:|
| Fecho `:1626`: substituir `if pergunta_viva(q):` por `if False:` | V | V | V | V |
| Porta `models.py:2569`: substituir `if viva:` por `if False:` | V | V | F | F |
| Aplicar os dois acima | F | V | F | F |
| I20 `:1103`: remover `.exclude(pk__in=_I20_CICATRIZ)` | V | V | V | F |
| I20 `:1103`: acrescentar `.none()` ao queryset | V | V | F | F |
| I20 `:1102`: remover só `.filter(respondida_em__isnull=True)` | V | V | V | V |

A redundância explica a primeira linha: sem guarda do fecho, `carimbar` aplica `respondida_em`, consulta C6, desfaz e recusa; sem recusa da porta, T1 ainda pula antes. No caso2, aplicar `respondida_em` torna a resposta viva se não houver concordância verificável com a luz. Fontes: `app/chamados/models.py:2555`, `:2565`, `:2569`; `app/chamados/juizes.py:1047`, `:1059`; `app/chamados/services/disputa_emissao.py:1626`.

**Mock e universo**

- O patch termina em teste `:194`; nenhuma leitura posterior permanece mockada. Mas o MORDE ramo tampouco chega ao vigia depois: `_assert_ramo` aborta em `:171`, antes de `:173–174`. Fontes: teste `:193`, `:195`, `:197`. [nao verificado]
- A cópia I20 executa `Command.handle(amostra=5, html=False)`, usa os mesmos modelos/banco e reproduz todo o módulo; só muda a tupla. Seu universo candidato é o global de perguntas com `respondida_em NULL` e texto diferente de `''`, sem filtro por empresa/data/disputa/materialização. O universo **após a exclusão** muda intencionalmente por um PK. Fontes: teste `:88`, `:228`, `:231`, `:236`; auditor `:552`, `:570`, `:1101`. [nao verificado]

**Relógio, PK e parsing**

- Há freeze na classe e `hoje` explícito para o vigia; o fecho usa `timezone.now` dentro da chamada e I20 não filtra por relógio. Não identifiquei relógio externo determinando I20 nesses trechos; cobertura de toda dependência transitiva é **[não verificado]**. Fontes: teste `:47`, `:96`, `:163`; fecho `:1623`; auditor `:1101`. [nao verificado]
- Existe dependência de `empresas_visiveis`: `empresas=[self.emp]` é interseção, não substituição. A fixture satisfaz o filtro atual porque cria empresa ativa e CNPJ fora do prefixo de diagnóstico. Fontes: `app/chamados/services/vigia_fio.py:534`, `:210`; teste `:104`; `app/colaboradores/queries.py:78`.
- Não reserva PK nem reinicia sequência explicitamente; T3 detecta colisão com cicatriz por `assertNotIn`, T4 detecta só ao exigir contagem real final. Isso pode produzir vermelho por identidade histórica, sem regressão semântica. Fontes: teste `:97`, `:141`, `:215`, `:245`; auditor `:1098`. [nao verificado]
- O parser exige exatamente uma linha I20 e usa offset fixo 68; corresponde ao formato atual. O comando trunca **descrição** a 58, não linha/amostra. Alteração no formato pode quebrar o teste; linha ausente/ERR não passa silenciosamente. Fontes: teste `:56`, `:69`; auditor `:1833`. [nao verificado]
- A identificação do PK usa substring em toda a linha, não parsing da lista de amostra; pode coincidir parcialmente com outro número. Fontes: teste `:202`; auditor `:1835`. [nao verificado]

## 01/11 17:49 · sessao 61e68756283b · - · - · origem=Ronald (tmux)

Q2 — fontes e limites encontrados:

| Campo | Escritor de produção | Leitor de produção relevante |
|---|---|---|
| `PerguntaDisputa.resposta_colab` | `responder_pergunta_disputa_core`, que grava texto e instante juntos (`app/chamados/services/disputa_emissao.py:1247`); `responder_automaticamente`, também em par (`app/chamados/services/disputa_emissao.py:1393`); reabertura limpa ambos (`app/chamados/services/disputa_emissao.py:1358`). | C6 (`app/chamados/juizes.py:1047`), respostas sem veredito (`app/chamados/models.py:1767`), I20 (`app/chamados/management/commands/auditar_invariantes_chamados.py:1101`), materializador (`app/chamados/services/materializacao.py:763`). |
| `respondida_em` | Os escritores acima; cura pela trilha (`app/chamados/services/disputa_emissao.py:1408`); porta `carimbar`, quando há texto e `resposta` solicitada (`app/chamados/models.py:2556`); ressuscitação limpa por porta de lote, selecionando texto vazio (`app/chamados/models.py:1423`, `app/chamados/models.py:1488`). | C6 (`app/chamados/juizes.py:1047`), `.estado` (`app/chamados/models.py:2477`), `.respondida` (`app/chamados/models.py:2783`), respostas sem veredito (`app/chamados/models.py:1769`), I20 (`app/chamados/management/commands/auditar_invariantes_chamados.py:1102`). |
| `materializada_em` | Atribuição central em `_escrever_carimbo` (`app/chamados/models.py:2527`), persistida por `carimbar`, `carimbar_identidade`, `descarimbar` e `descarimbar_em_lote` (`app/chamados/models.py:2533`, `app/chamados/models.py:2588`, `app/chamados/models.py:2609`, `app/chamados/models.py:2619`). | C6 (`app/chamados/juizes.py:1071`, `app/chamados/juizes.py:1077`, `app/chamados/juizes.py:1107`), seleção do fecho (`app/chamados/services/disputa_emissao.py:1620`), respostas sem veredito (`app/chamados/models.py:1768`), I9 (`app/chamados/management/commands/auditar_invariantes_chamados.py:953`). |
| `via_resolucao` | As portas do carimbo acima; exceção explícita que apenas troca a via em `encerrar_por_fato_no_espelho` (`app/chamados/models.py:2629`). | C6 (`app/chamados/juizes.py:1042`, `app/chamados/juizes.py:1071`), PMF (`app/chamados/juizes.py:624`), I9 (`app/chamados/management/commands/auditar_invariantes_chamados.py:954`). |
| `CelulaDia.veredito`, `veredito_via` | `CelulaDia.lavrar_veredito` delega (`app/escala/models.py:1787`) à porta que escreve ambos e `veredito_em`, status, protestos e evento (`app/ponto/portas/celula.py:709`, `app/ponto/portas/celula.py:770`). | C6 (`app/chamados/juizes.py:1045`, `app/chamados/juizes.py:1081`), retenção da folha (`app/folha/export.py:1215`), vigia pelo universo de `fora_da_folha` (`app/chamados/services/vigia_fio.py:545`). |
| `CelulaDia.ata` | Mesma porta de lavra (`app/ponto/portas/celula.py:726`, `app/ponto/portas/celula.py:768`); chave `precedencia` pertence a `carimbar_precedencia` (`app/ponto/portas/celula.py:811`). | Luz lida pelo C6 (`app/chamados/juizes.py:1008`); tratamento de marcos pendentes pelo C6 (`app/chamados/juizes.py:1094`). |
| `DisputaSupervisao.fechada_em` | `fechar` (`app/chamados/models.py:1845`), `reabrir_admin` (`app/chamados/models.py:2047`), `reabrir_sistema` (`app/chamados/models.py:2071`). | Seleção e fechamento do serviço (`app/chamados/services/disputa_emissao.py:1608`, `app/chamados/services/disputa_emissao.py:1658`), badge (`app/chamados/models.py:1721`), signal (`app/chamados/signals.py:28`), API (`app/api/views.py:1008`). |
| `ChamadoColaborador.status_local` | Transições por `_gravar_estado` (`app/chamados/models.py:195`, `app/chamados/models.py:222`, `app/chamados/models.py:279`); nascimento pelas portas `abrir`/`criar` (`app/chamados/models.py:897`, `app/chamados/models.py:1046`). | Sem-credor (`app/chamados/services/disputa_emissao.py:1613`, `app/chamados/services/disputa_emissao.py:1616`), C6/PMF (`app/chamados/juizes.py:721`), vigia (`app/chamados/services/vigia_fio.py:569`, `app/chamados/services/vigia_fio.py:578`), signal do fecho (`app/chamados/signals.py:81`). |

A fixture introduz deliberadamente texto sem instante por `QuerySet.update(resposta_colab='07:55')`, em vez de usar os escritores do par. Isso não demonstra um caminho atual de produção gerando esse estado; o código do I20 documenta acervo histórico com `respondida_em` nulo, que também não deve ser convertido em fato observado nesta revisão (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:143`; `app/chamados/services/disputa_emissao.py:1247`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1089`).

Achado mais forte: o caso regularizado injeta `veredito='ok'`, `veredito_via='cartorio'` diretamente no banco; a porta real rejeita esse veredito, pois `ok` não pertence a `VEREDITOS_CARTORIO`. O veredito regular sem códigos é `concorde`. Logo, o estado exato da fixture não sai dessa porta de produção (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:208`; `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:152`; `app/ponto/portas/celula.py:658`; `app/escala/models.py:1719`; `app/escala/models.py:1596`).

`_fechar` chama produção e, depois, sobrescreve a célula novamente. A testemunha mede esse estado imposto após o serviço, sem exigir que sobrevivam o veredito, a ata ou os efeitos da relavra real; o update omite `veredito_em`, status, protestos e emissão do evento da porta (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:155`; `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:152`; `app/ponto/portas/celula.py:770`).

O teste também cria o chamado já resolvido e a disputa aberta por ORM, sem provar a travessia pelas portas de estado; tampouco possui assert direto sobre `status_local` ou `fechada_em`, apenas o retorno contado do fecho no teste do ramo (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:135`; `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:139`; `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:185`).

Não dizer que “os selos da casa reprovam este teste”: os selos AST do par resposta/carimbo, da porta de célula e do carimbo excluem diretórios `tests`. As mesmas escritas em um arquivo de produção seriam alcançadas por esses contratos (`app/chamados/tests/test_contract_par_resposta.py:54`; `app/chamados/tests/test_contract_par_resposta.py:81`; `app/ponto/tests/test_selo_veredito_so_pela_porta.py:173`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:208`).

## 01/11 16:53 · sessao ee8a3724479c · O261-E-F2 · verificador · origem=Code

1. **Sim:** o resultado agregado coincide exatamente com `VIA_DO_CHAMADOR`: admin → `VIA_ADMIN_DECIDIU`; duplicata → `VIA_DUPLICATA`; paridade → `VIA_PARIDADE_RECUSADA`; recusa → `VIA_FATO_OCUPADO`. Os quatro sítios são [models.py:2498](app/chamados/models.py:2498), [fio.py:891](app/chamados/services/fio.py:891), [materializacao.py:858](app/chamados/services/materializacao.py:858) e [materializacao.py:1033](app/chamados/services/materializacao.py:1033); a comparação está em [test_o261e_f2_porta_carimbo.py:482](app/chamados/tests/test_o261e_f2_porta_carimbo.py:482).

   Nenhuma ocorrência nesses três arquivos acrescenta `<indireta>`. O `def` não é `ast.Name`; comentários não entram na AST; docstrings e strings maiores não satisfazem a igualdade exata. Uma string/docstring contendo **somente** `carimbar_identidade` contaria como `<indireta>`. O atributo da chamada direta fica excluído por `diretas`. [test_o261e_f2_porta_carimbo.py:414](app/chamados/tests/test_o261e_f2_porta_carimbo.py:414), [test_o261e_f2_porta_carimbo.py:422](app/chamados/tests/test_o261e_f2_porta_carimbo.py:422).

2. **Existe mutante de duas linhas:** substituir [materializacao.py:858](app/chamados/services/materializacao.py:858) por:

   ```python
   p.VIA_PARIDADE_RECUSADA = PerguntaDisputa.VIA_ADMIN_DECIDIU
   p.carimbar_identidade(p.VIA_PARIDADE_RECUSADA, request_user)
   ```

   O coletor continua enxergando `VIA_PARIDADE_RECUSADA`, pois verifica apenas `via.attr`; não resolve seu receptor nem seu valor. A referência à porta permanece na mesma função declarada, mantendo também o teste dos chamadores verde. [test_o261e_f2_porta_carimbo.py:420](app/chamados/tests/test_o261e_f2_porta_carimbo.py:420), [test_o261e_f2_porta_carimbo.py:443](app/chamados/tests/test_o261e_f2_porta_carimbo.py:443), [test_o261e_f2_porta_carimbo.py:473](app/chamados/tests/test_o261e_f2_porta_carimbo.py:473).

   Entretanto, o argumento passa a valer `'admin_decidiu'`, uma identidade aceita pela porta, que o encaminha à escrita e ao `save`. [models.py:2237](app/chamados/models.py:2237), [relavra_via.py:166](app/chamados/services/relavra_via.py:166), [models.py:2597](app/chamados/models.py:2597), [models.py:2602](app/chamados/models.py:2602), [models.py:2607](app/chamados/models.py:2607).

   Reexecutei os corpos dos dois testes em memória, usando `unittest.TestCase` e sobrepondo a leitura do arquivo pelo mutante: ambos passaram. Runner Django completo: [não verificado]. [test_o261e_f2_porta_carimbo.py:473](app/chamados/tests/test_o261e_f2_porta_carimbo.py:473), [test_o261e_f2_porta_carimbo.py:482](app/chamados/tests/test_o261e_f2_porta_carimbo.py:482).

3. **O teste novo lê somente fonte sintética:** constrói `src`, passa ao coletor AST e compara conjuntos; não consulta campos de banco. O teste da árvore também lê arquivos de fonte. [test_o261e_f2_porta_carimbo.py:499](app/chamados/tests/test_o261e_f2_porta_carimbo.py:499), [test_o261e_f2_porta_carimbo.py:485](app/chamados/tests/test_o261e_f2_porta_carimbo.py:485).

   No diff fornecido, nenhum leitor de produção é alterado: as mudanças ficam no coletor e no teste. O leitor C6 existente consome `via_resolucao` e `materializada_em`; a porta existente escreve esses campos com o autor. [test_o261e_f2_porta_carimbo.py:406](app/chamados/tests/test_o261e_f2_porta_carimbo.py:406), [test_o261e_f2_porta_carimbo.py:499](app/chamados/tests/test_o261e_f2_porta_carimbo.py:499), [juizes.py:1071](app/chamados/juizes.py:1071), [models.py:2527](app/chamados/models.py:2527). Efeito em registros reais: [não verificado].

## 01/11 16:49 · sessao 4b97a069739e · O261-E-F2 · verificador · origem=Code

1. **QUEM CHAMA**

As quatro chamadas encontradas fora de `tests/` e `migrations/` coincidem com `VIA_DO_CHAMADOR`, inclusive a via atribuída a cada função. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:398`)

| Local | Chamador | Primeiro argumento literal |
|---|---|---|
| `app/chamados/models.py:2498` | `PerguntaDisputa.fechar_por_admin` | `self.VIA_ADMIN_DECIDIU` |
| `app/chamados/services/fio.py:891` | `retratar_duplicata` | `PerguntaDisputa.VIA_DUPLICATA` |
| `app/chamados/services/materializacao.py:858` | `_materializar_resposta_hora` | `PerguntaDisputa.VIA_PARIDADE_RECUSADA` |
| `app/chamados/services/materializacao.py:1033` | `_carimbar_recusa` | `PerguntaDisputa.VIA_FATO_OCUPADO` |

2. **MORDE: existe mutante que passa nos dois testes.** [nao verificado]

Substitua a chamada de `app/chamados/services/materializacao.py:858` pelas três linhas abaixo, mantendo o retorno seguinte:

```python
p.carimbar_identidade(PerguntaDisputa.VIA_PARIDADE_RECUSADA, request_user)
gravar = p.carimbar_identidade
gravar(via=PerguntaDisputa.VIA_ADMIN_DECIDIU, autor=request_user)
```

A primeira chamada preserva a via esperada pelo teste. A segunda chamada usa `ast.Name`, enquanto o varredor de vias exige `ast.Attribute`; portanto, a gravação de `admin_decidiu` fica invisível para ele. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:413`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:415`)

O varredor de referências reconhece a atribuição do alias, mas ela permanece no mesmo chamador autorizado. Como o resultado vira conjunto de `(arquivo, qualname)`, essa referência adicional não altera a comparação. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:433`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:457`)

Em execução, ambas as vias são aceitas e a última chamada sobrescreve a primeira, persistindo `admin_decidiu`. (`app/chamados/models.py:2597`, `app/chamados/models.py:2602`, `app/chamados/models.py:2531`, `app/chamados/models.py:2607`)

Reexecutei os dois métodos de teste em memória, interceptando somente a leitura da fonte mutante: original e mutante passaram nas duas verificações. Usei `unittest.TestCase` no lugar de `SimpleTestCase`; execução integral pelo Django: **[não verificado]**. Métodos exercitados: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:463` e `app/chamados/tests/test_o261e_f2_porta_carimbo.py:472`.

Os casos pedidos ficam assim:

| Forma | Resultado |
|---|---|
| Chamada direta com via em variável local | Vira `<dinamica>`; reprova o teste de vias. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:415`) |
| Chamada direta com `via=outra_variavel` | Também vira `<dinamica>`. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:414`) |
| Keyword diferente de `via` | Sem argumento posicional, vira `<dinamica>`; a assinatura também não aceita esse outro nome. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:414`, `app/chamados/models.py:2588`) |
| Duas chamadas diretas com vias distintas | Ambas entram no conjunto; reprova. Duas com a mesma via não são contadas separadamente. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:416`) |
| Chamada direta em função aninhada nomeada | Ganha outro `qualname`; reprova os mapas. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:411`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:430`) |

3. **QUEM ESCREVE E QUEM LÊ**

A cadeia de escrita é `carimbar_identidade` → `_escrever_carimbo` → atribuição de `via_resolucao`; quem persiste é `carimbar_identidade`, no mesmo `save` dos campos de materialização. A guarda aceita qualquer uma das quatro identidades, sem vincular a via ao chamador. (`app/chamados/models.py:2597`, `app/chamados/models.py:2600`, `app/chamados/models.py:2602`, `app/chamados/models.py:2531`, `app/chamados/models.py:2607`)

Estes leitores **distinguem as quatro vias**:

| Leitor | Decisão que pode mudar |
|---|---|
| `estado_aplicacao_pergunta` | Para pergunta hora, com resposta e materializada: paridade → `recusada_paridade`; outras identidades → `aplicada`. É efeito direto do mutante acima. (`app/chamados/services/materializacao.py:417`, `app/chamados/services/materializacao.py:423`, `app/chamados/services/materializacao.py:427`) |
| `fila_flip_recusas` | Só seleciona paridade; trocar para admin retira a pergunta dessa fonte de candidatos, satisfeitas as demais condições. (`app/ponto/services/flip_auto.py:128`, `app/ponto/services/flip_auto.py:132`, `app/ponto/services/flip_auto.py:140`) |
| Cartório | O ramo que tenta reconciliar exige que todas as resolvedoras sejam duplicatas, após as guardas anteriores. (`app/ponto/services/cartorio.py:1424`, `app/ponto/services/cartorio.py:1429`) |
| Fábrica da célula | Duplicata é excluída do conjunto “já perguntado”; as outras identidades permanecem. (`app/chamados/services/fabrica_da_celula.py:103`, `app/chamados/services/fabrica_da_celula.py:108`, `app/chamados/services/fabrica_da_celula.py:111`) |
| Emissão por competência | Também exclui duplicata do conjunto “já perguntado”. (`app/chamados/management/commands/disparar_perguntas_competencia.py:124`, `app/chamados/management/commands/disparar_perguntas_competencia.py:127`) |
| `fechar_por_admin` | A guarda idempotente só reconhece `admin_decidiu` materializada; outra identidade é sobrescrita. (`app/chamados/models.py:2489`, `app/chamados/models.py:2498`) |
| `retratar_duplicata` e comando de deduplicação | Pulam a pergunta já marcada como duplicata. (`app/chamados/services/fio.py:882`, `app/chamados/management/commands/dedup_perguntas_fato.py:74`) |
| Apresentação e classificação | Mudam a frase do cartão, o motivo do explicador e o balde `via:<valor>`. (`app/templates/chamados/partials/_card_atendimento.html:186`, `app/chamados/services/explicador.py:95`, `app/chamados/services/passivo_validado.py:140`) |

O C6, `_pergunta_materializada_de_fato`, `q_vivas` e `q_carimbo_desfazivel` agrupam as quatro identidades; trocar somente entre elas não muda esses predicados. A guarda da resposta do colaborador precede o encerramento por identidade no C6. (`app/chamados/juizes.py:632`, `app/chamados/juizes.py:1047`, `app/chamados/juizes.py:1066`, `app/chamados/models.py:2708`, `app/chamados/services/relavra_via.py:179`)

Há também leituras para memo, restauração e trilha, sem decisão específica entre as quatro: `app/chamados/juizes.py:790`, `app/chamados/models.py:2552`, `app/chamados/models.py:1838`, `app/chamados/reconciliador.py:1089`, `app/chamados/services/disputa_emissao.py:1628`, `app/chamados/services/materializacao.py:614`, `app/chamados/services/materializacao.py:1023`, `app/chamados/services/passivo_validado.py:515`.

A re-lavra restringe-se às vias derivadas; o comando de encerramento por fato restringe-se a `SEM_HISTORIA`; os leitores de fantasma/sem lastro/tranca guardam por outras vias. Nenhum desses filtros distingue as quatro identidades. (`app/chamados/services/relavra_via.py:77`, `app/chamados/services/relavra_via.py:116`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:40`, `app/chamados/models.py:2645`, `app/chamados/models.py:2657`, `app/chamados/models.py:2674`, `app/chamados/models.py:2681`, `app/chamados/services/competencia_trancada.py:244`)

4. **TRIAGEM V4**

Não encontrei descarte demonstravelmente improcedente **no alcance específico declarado**. Os MORDE realmente usam fontes sintéticas, e o teste da árvore realmente compara os chamadores vivos; isso não garante cobertura de toda forma de chamada. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:463`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:491`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:509`)

O mutante encontrado é alias local de método, não importação renomeada. Assim, aquele descarte não o resolve: a referência fica autorizada pelo conjunto de chamadores, enquanto a chamada indireta escapa ao mapa de vias. A correção v5 barra a troca direta citada na triagem, mas deixa essa lacuna. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:413`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:433`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:457`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:479`)

## 01/11 16:48 · sessao 8e6feb3eb2a7 · - · - · origem=Ronald (tmux)

Mutante de duas linhas, inserido após `app/chamados/services/materializacao.py:858` e antes do retorno em `:859`:

```python
carimbar = p.carimbar_identidade
carimbar(PerguntaDisputa.VIA_ADMIN_DECIDIU, request_user)
```

A chamada original de paridade permanece. O varredor de vias exige `ast.Call.func` do tipo `ast.Attribute`, portanto não vê a segunda chamada por `ast.Name`; a referência ao método continua na mesma função já declarada e o outro teste reduz as referências a um conjunto de `(arquivo, qualname)`. Assim, ambos passam, enquanto a segunda gravação substitui a via por `admin_decidiu`. Fontes: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:413`, `:416`, `:433`, `:457`, `:472`; `app/chamados/models.py:2597`, `:2602`, `:2607`.

Executei em memória as duas funções de teste reais, mapas e varredores extraídos por AST: original e mutante deram dois testes, zero falhas, zero erros. Usei `unittest.TestCase` no lugar de `SimpleTestCase`, interceptando somente a leitura da fonte mutada; execução integral pelo Django permanece [não verificado]. [nao verificado]

Quanto aos descartes da triagem, não identifiquei um factualmente improcedente **dentro do alcance específico declarado**:

- Os seis MORDE realmente verificam fontes sintéticas; o teste da árvore realmente compara chamadores reais. A detecção de M1 não implica que detecte o mutante acima. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:463`, `:491`, `:509`, `:514`, `:533`.
- A limitação para nome montado decorre de comparar atributo/nome/string exatos. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:433`.
- A exclusão de `carimbar` do selo de chamadas internas é coerente com consultar o C6; as voltas escrevem dentro das portas autorizadas. `app/chamados/models.py:2565`, `:2609`, `:2619`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:44`, `:51`.

É improcedente apenas extrapolar esses descartes para uma garantia geral de que qualquer chamada com via indevida seria barrada: o mutante acima refuta essa garantia. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:413`, `:457`.

## 01/11 16:47 · sessao b972a23c050a · - · - · origem=Ronald (tmux)

A escrita é `PerguntaDisputa.carimbar_identidade` → `_escrever_carimbo` → `self.via_resolucao = via`; a porta persiste os três campos em `save(update_fields=campos)`. A lista aceita as quatro vias de identidade. (`app/chamados/models.py:2597`, `app/chamados/models.py:2600`, `app/chamados/models.py:2602`, `app/chamados/models.py:2531`, `app/chamados/models.py:2603`, `app/chamados/models.py:2607`, `app/chamados/services/relavra_via.py:166`)

Leitores que **mudam comportamento entre as quatro vias**:

| Leitor | Diferença |
|---|---|
| `estado_aplicacao_pergunta` | Para pergunta tipo hora com resposta e materializada, `paridade_recusada` retorna `recusada_paridade`; as demais retornam `aplicada`. (`app/chamados/services/materializacao.py:417`, `app/chamados/services/materializacao.py:423`, `app/chamados/services/materializacao.py:427`) |
| `fila_flip_recusas` | Só seleciona `paridade_recusada`, com validação recente; extrai a batida vizinha do motivo e a inclui na fila de flip, incorporada por `fila_flip`. Trocar por `admin_decidiu` remove essa fonte de candidatura, preservadas as demais condições. (`app/ponto/services/flip_auto.py:128`, `app/ponto/services/flip_auto.py:132`, `app/ponto/services/flip_auto.py:140`, `app/ponto/services/flip_auto.py:148`) |
| Cartório | Quando todas as resolvedoras são `duplicata`, entra no ramo que tenta reconciliar o chamado. Outra via não entra nesse ramo. (`app/ponto/services/cartorio.py:1424`, `app/ponto/services/cartorio.py:1429`, `app/ponto/services/cartorio.py:1430`) |
| Fábrica da célula | Exclui `duplicata` do universo que alimenta `ja`; outra identidade permanece e pode impedir nova pergunta daquele motivo. (`app/chamados/services/fabrica_da_celula.py:103`, `app/chamados/services/fabrica_da_celula.py:104`, `app/chamados/services/fabrica_da_celula.py:108`, `app/chamados/services/fabrica_da_celula.py:111`) |
| Emissão por competência | Também exclui `duplicata` do conjunto já perguntado. (`app/chamados/management/commands/disparar_perguntas_competencia.py:124`, `app/chamados/management/commands/disparar_perguntas_competencia.py:127`, `app/chamados/management/commands/disparar_perguntas_competencia.py:129`) |
| `fechar_por_admin` | A guarda idempotente só retorna `False` para `admin_decidiu` materializada; outra identidade é sobrescrita. (`app/chamados/models.py:2489`, `app/chamados/models.py:2497`, `app/chamados/models.py:2498`) |
| `retratar_duplicata` | Só retorna imediatamente para `duplicata`; outra identidade é substituída pela via de duplicata. (`app/chamados/services/fio.py:882`, `app/chamados/services/fio.py:883`, `app/chamados/services/fio.py:891`) |
| Comando de deduplicação | Remove as já `duplicata` da lista de excedentes a processar. (`app/chamados/management/commands/dedup_perguntas_fato.py:74`, `app/chamados/management/commands/dedup_perguntas_fato.py:76`) |

Leitores que alteram **classificação/apresentação**: `_balde_da_recusa` devolve o valor concreto em `via:<valor>`; o explicador mostra o display da via; o cartão distingue admin, duplicata e as duas recusas. (`app/chamados/services/passivo_validado.py:140`, `app/chamados/services/passivo_validado.py:142`, `app/chamados/services/explicador.py:95`, `app/templates/chamados/partials/_card_atendimento.html:186`)

O C6 **não distingue as quatro** no degrau por identidade; `_pergunta_materializada_de_fato` também as absolve igualmente, `q_vivas` as agrupa e `q_carimbo_desfazivel` exclui todas. A resposta do colaborador é julgada antes do degrau de identidade; logo, não afirmar “qualquer identidade sempre mata a pergunta”. (`app/chamados/juizes.py:1047`, `app/chamados/juizes.py:1055`, `app/chamados/juizes.py:1066`, `app/chamados/juizes.py:1071`, `app/chamados/juizes.py:632`, `app/chamados/juizes.py:635`, `app/chamados/models.py:2708`, `app/chamados/models.py:2713`, `app/chamados/services/relavra_via.py:166`, `app/chamados/services/relavra_via.py:179`)

A re-lavra só aceita `declaracao`/`completude_turno`, portanto não troca nenhuma das quatro identidades; `encerrar_perguntas_com_fato` também não as inclui em `SEM_HISTORIA`. (`app/chamados/services/relavra_via.py:77`, `app/chamados/services/relavra_via.py:116`, `app/chamados/services/relavra_via.py:123`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:40`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:42`)

Outros usos são memória/restauração, memo, trilha ou agrupamento por via: `app/chamados/models.py:2552`, `app/chamados/models.py:2613`, `app/chamados/models.py:2627`, `app/chamados/juizes.py:790`, `app/chamados/models.py:1838`, `app/chamados/reconciliador.py:1089`, `app/chamados/reconciliador.py:1100`, `app/chamados/services/disputa_emissao.py:1628`, `app/chamados/services/disputa_emissao.py:1643`, `app/chamados/services/materializacao.py:614`, `app/chamados/services/materializacao.py:1023`, `app/chamados/services/passivo_validado.py:64`, `app/chamados/services/passivo_validado.py:515`, `app/chamados/management/commands/auditar_invariantes_chamados.py:194`, `app/chamados/management/commands/auditar_invariantes_chamados.py:205`, `app/chamados/management/commands/auditar_invariantes_chamados.py:432`, `app/chamados/management/commands/auditar_invariantes_chamados.py:541`.

Há ainda escritores/leitores de outras vias: `encerrar_por_fato_no_espelho` escreve `fato_materializado`; as portas de órfã/fantasma/sem lastro/tranca guardam por essas vias, e seus filtros não diferenciam as quatro identidades. (`app/chamados/models.py:2634`, `app/chamados/models.py:2636`, `app/chamados/models.py:2645`, `app/chamados/models.py:2657`, `app/chamados/models.py:2674`, `app/chamados/models.py:2681`, `app/chamados/services/competencia_trancada.py:244`, `app/chamados/services/materializacao.py:572`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:67`)

## 01/11 16:44 · sessao 2b54b3c2d6c8 · O261-E-F2 · verificador · origem=Code

O patch de reabertura **não entrega o resultado prometido pelo fixture**: a primeira guarda devolve `veto_do_dia`, por ausência de vínculo de escala. Além disso, os selos novos aceitam mutantes concretos que burlam a intenção da porta. (`app/chamados/models.py:1285`; `app/ponto/services/furos_vetados.py:40`; `app/chamados/tests/test_o261_resposta_vazia.py:36`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:403`)

As referências abaixo são relativas à raiz da cópia. Para os dois anexos sem arquivo na árvore, numerei o conteúdo fornecido desde a primeira linha: `diff --git` no patch e `# O261-E-F2` na sonda. Execução Django, resultados da sonda e estado de produção: **[nao verificado]**.

1. Quem chama as seis portas.

Estes são os sítios executáveis encontrados para os métodos de `PerguntaDisputa`; chamadas repetidas dentro da mesma função aparecem separadamente pelas linhas. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:394`)

| Método | Chamador e sítio |
|---|---|
| `_escrever_carimbo` | `PerguntaDisputa.carimbar` — `app/chamados/models.py:2555` |
| | `PerguntaDisputa._desfazer_em_memoria` — `app/chamados/models.py:2582` |
| | `PerguntaDisputa.carimbar_identidade` — `app/chamados/models.py:2600`, `app/chamados/models.py:2602` |
| | `PerguntaDisputa.descarimbar` — `app/chamados/models.py:2611` |
| `_desfazer_em_memoria` | `PerguntaDisputa.carimbar`, tanto na exceção quanto na recusa — `app/chamados/models.py:2567`, `app/chamados/models.py:2570` |
| `carimbar_identidade` | `PerguntaDisputa.fechar_por_admin` — `app/chamados/models.py:2498` |
| | `retratar_duplicata` — `app/chamados/services/fio.py:891` |
| | `_materializar_resposta_hora` — `app/chamados/services/materializacao.py:858` |
| | `_carimbar_recusa` — `app/chamados/services/materializacao.py:1033` |
| `carimbar` | `PerguntaDisputa.encerrar_fantasma` — `app/chamados/models.py:2648` |
| | `lavrar_sem_turno_aberto` — `app/chamados/reconciliador.py:405` |
| | `retratar_perguntas_absolvidas_por_celula` — `app/chamados/reconciliador.py:618` |
| | `_materializar_ausencia_de_resposta` — `app/chamados/services/materializacao.py:523` |
| | `_materializar_uma` — `app/chamados/services/materializacao.py:1166` |
| | `materializar_pendentes_aprovadas` — `app/chamados/services/materializacao.py:1313` |
| | `resolver_perguntas_por_chamado_resolvido`, dois ramos — `app/chamados/services/disputa_emissao.py:1566`, `app/chamados/services/disputa_emissao.py:1638` |
| `descarimbar` | `ChamadoColaborador._universo_da_ressuscitacao`, somente na cópia em memória — `app/chamados/models.py:1439` |
| | `PerguntaDisputa.reabrir_fantasma` — `app/chamados/models.py:2659` |
| | `PerguntaDisputa.reabrir_por_resposta` — `app/chamados/models.py:2676` |
| | `PerguntaDisputa.reabrir_por_competencia` — `app/chamados/models.py:2683` |
| | `desfazer_carimbo_sem_lastro` — `app/chamados/services/relavra_via.py:231` |
| `descarimbar_em_lote` | `ChamadoColaborador.ressuscitar_perguntas` — `app/chamados/models.py:1488` |
| | `reabrir_para_correcao` — `app/chamados/services/disputa_emissao.py:1377` |

As referências descritivas às portas, sem chamada executável, estão nos comentários de declaração do modelo, na descrição do uso nos serviços, no registro de portas e no dossiê. Essas referências não acrescentam chamadores ao conjunto AST porque são comentários ou strings maiores que o nome procurado. (`app/chamados/models.py:2506`; `app/chamados/models.py:2512`; `app/chamados/models.py:2514`; `app/chamados/models.py:2516`; `app/chamados/models.py:2580`; `app/chamados/models.py:2620`; `app/chamados/services/disputa_emissao.py:1562`; `app/chamados/services/materializacao.py:857`; `app/core/portas.py:246`; `app/docs/DOSSIES.md:46`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:405`)

Há homônimos que não são essas portas: `api.credencial.carimbar`, a porta de célula importada pelo cartório e a variável local `carimbar` de competência trancada. (`app/api/views.py:23`; `app/api/views_core.py:23`; `app/ponto/services/cartorio.py:30`; `app/chamados/services/competencia_trancada.py:131`)

**Comparação com `CHAMADORES`:** os quatro chamadores de `_escrever_carimbo`, o único de `_desfazer_em_memoria` e os quatro de `carimbar_identidade` coincidem com os declarados. O dict **não fiscaliza** quem chama `carimbar`, `descarimbar` ou `descarimbar_em_lote`. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:373`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:380`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:385`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:434`)

| Forma | O selo pega ou perde? |
|---|---|
| `p.metodo(...)`, `PerguntaDisputa.metodo(p,...)`, alias obtido por `g=p.metodo` | **Pega** o atributo; não precisa resolver seu receptor. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:403`) |
| Import local do modelo com alias, seguido de `PD.metodo(...)` | **Pega** pelo atributo; o import local não cria ponto cego nessa forma. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:403`) |
| Import do próprio nome, renomeado: `from x import _escrever_carimbo as g; g(...)` | **Perde o import como referência**: `ast.alias` não é uma das três formas examinadas. A existência de um export desse método na cópia não foi demonstrada. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:403`; **[nao verificado]**) |
| Função aninhada | **Pega**, incluindo a função interna no qualname; portanto cria outra chave na whitelist. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:400`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:407`) |
| Classe aninhada | **Pega**, incluindo classe e método internos no qualname. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:400`) |
| Lambda com referência explícita | **Pega**, mas não acrescenta um componente “lambda” ao qualname. Uma chamada extra dentro de função permitida pode conservar a mesma chave. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:400`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:427`) |
| `super().metodo`, `type(self).metodo`, `self.__class__.metodo` | **Pega**, pois todos apresentam `ast.Attribute.attr` igual ao nome. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:403`) |
| `getattr(p,'_escrever_carimbo')`, `methodcaller('_escrever_carimbo',...)` | **Pega** a constante exata. Também pega `nome='_escrever_carimbo'; getattr(p,nome)`, no literal. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:405`) |
| `getattr(p,'_escrever_'+'carimbo')`, `join`, formatação ou nome vindo de fora | **Perde quando não existe atributo, nome ou constante exata**: não avalia a expressão nem acompanha seu valor. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:403`) |

O selo também descarta linhas e multiplicidade: fiscaliza um **conjunto de `(arquivo, qualname)`**. Não distingue uma chamada legítima de outra chamada indevida na mesma função, nem examina seus argumentos. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:427`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:437`)

Seu universo é menor que “toda árvore fora tests/migrations”: exclui outros diretórios, todos os arquivos não Python, `test_*.py` e fontes que falhem no parse/decodificação. Ausência universal de chamadores dinâmicos fora desse alcance: **[nao verificado]**. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:417`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:420`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:425`)

2. Cada selo morde?

Para testar **cada um dos sete testes novos**, apliquei virtualmente o mesmo mutante de produção M1, de três linhas, acrescentado a `app/chamados/reconciliador.py`; as verificações foram executadas apenas sobre AST e fontes em memória. O mutante usa a escrita interna e persiste sem consultar C6. (`app/chamados/models.py:2527`; `app/chamados/models.py:2565`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:433`)

```diff
+def carimbar_por_fora(p, u, via, agora):
+    p._escrever_carimbo(agora, u.pk, via)
+    p.save(update_fields=['materializada_em', 'materializada_por', 'via_resolucao'])
```

| Teste de `SeloQuemChamaAPorta` | Com M1 na produção | [nao verificado]
|---|---|
| `test_os_chamadores_de_cada_porta_interna_sao_exatamente_os_declarados` | **Vermelho**: encontra a nova chave fora de `CHAMADORES`. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:437`) |
| `test_MORDE_chamada_por_atributo_por_classe_por_alias_getattr_e_methodcaller` | **Verde**: verifica somente sua string `src`. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:445`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:452`) |
| `test_o_def_e_a_prosa_nao_sao_referencia` | **Verde**: verifica somente sua string `src`. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:455`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:460`) |
| `test_MORDE_service_que_escreve_o_carimbo_sem_o_c6` | **Verde**: demonstra que a amostra seria classificada como proibida; não lê o service modificado. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:469`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:472`) |
| `test_MORDE_metodo_novo_no_modelo_tambem_fica_fora` | **Verde**: examina a classe escrita na amostra. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:477`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:481`) |
| `test_MORDE_identidade_chamada_de_modulo_que_nao_julga_identidade` | **Verde**: examina a amostra `fechar_em_massa`. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:488`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:491`) |
| `test_MORDE_o_chamador_declarado_em_outro_arquivo_nao_vale` | **Verde**: examina o homônimo artificial. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:496`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:497`) |

Esses seis testes são úteis contra regressões do **varredor**; quem fiscaliza uma mutação efetiva da produção é o primeiro teste da tabela. Não são seis provas independentes de rejeição do código mutado. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:433`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:462`)

Para `test_MORDE_a_arvore_real_ainda_atribui_o_update_pelo_gerente_reverso`, o mutante concreto em `app/chamados/models.py:1488` é:

```diff
-        n = PerguntaDisputa.descarimbar_em_lote(alvo, validada_em=None, validada_por=None, respondida_em=None)
+        n = alvo.update(materializada_em=None, validada_em=None, validada_por=None, respondida_em=None)
```

**Fica vermelho**, primeiro porque desaparece a âncora literal exigida pelo teste. A segunda metade do teste, na fonte original, também demonstra que o varredor atribui o `alvo.update()` artificial a `PerguntaDisputa` pelo mapa real de relações. Portanto há mordida, mas a rejeição dessa alteração concreta ocorre antes, pela âncora. (`app/core/tests/test_censo_escritas.py:234`; `app/core/tests/test_censo_escritas.py:235`; `app/core/tests/test_censo_escritas.py:240`; `app/core/tests/test_censo_escritas.py:244`)

**Sim, há mutante plausível que passa nos oito testes pedidos, sem montagem de nome.** Em `app/chamados/services/materializacao.py:858`:

```diff
-            p.carimbar_identidade(PerguntaDisputa.VIA_PARIDADE_RECUSADA, request_user)
+            p.carimbar_identidade(PerguntaDisputa.VIA_ADMIN_DECIDIU, request_user)
```

Os oito permaneceram verdes no ensaio em memória: arquivo, função e referência continuam iguais. O juiz de paridade passa a registrar uma decisão de admin; `carimbar_identidade` aceita essa via porque ela pertence à lista de identidade. O selo não verifica essa correspondência entre chamador e via. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:388`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:403`; `app/chamados/models.py:2597`; `app/chamados/juizes.py:1066`)

Há ainda o bypass de M1 trocando sua segunda linha por:

```diff
+    getattr(p, '_escrever_' + 'carimbo')(agora, u.pk, via)
```

Com essa forma, os sete testes de chamadores permanecem verdes; o teste novo do censo também permanece verde porque sua fonte-alvo não mudou. O varredor de escritores não reconhece o `save(update_fields=...)` como atribuição dos valores do carimbo. Resultado da suíte Django completa para esses mutantes: **[nao verificado]**. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:403`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:183`; `app/core/tests/test_censo_escritas.py:233`)

3. Escritores e leitores dos campos.

| Campo | Escritores persistentes fora das seis portas |
|---|---|
| `materializada_em` | Nenhuma atribuição explícita adicional encontrada para `PerguntaDisputa`: atribuição em `_escrever_carimbo` e limpeza na porta de lote. (`app/chamados/models.py:2529`; `app/chamados/models.py:2626`) |
| `materializada_por` | Mesma concentração, usando `_id` na atribuição e FK na limpeza. Há também o efeito de exclusão de usuário, abaixo. (`app/chamados/models.py:2530`; `app/chamados/models.py:2627`) |
| `via_resolucao` | `encerrar_por_fato_no_espelho`, atribuição e save; `relavra_via.relavrar`, nova via e relavra no mesmo save. (`app/chamados/models.py:2636`; `app/chamados/models.py:2637`; `app/chamados/services/relavra_via.py:123`; `app/chamados/services/relavra_via.py:125`) |
| `respondida_em` | Resposta do colaborador; limpeza para correção; resposta automática; recuperação pela trilha. (`app/chamados/services/disputa_emissao.py:1248`; `app/chamados/services/disputa_emissao.py:1266`; `app/chamados/services/disputa_emissao.py:1358`; `app/chamados/services/disputa_emissao.py:1378`; `app/chamados/services/disputa_emissao.py:1394`; `app/chamados/services/disputa_emissao.py:1395`; `app/chamados/services/disputa_emissao.py:1410`; `app/chamados/services/disputa_emissao.py:1411`) |
| `validada_em` | `_carimbar_validacao`; `devolver_validacao_recusada`; nova resposta que desfaz validação; limpeza para correção. (`app/chamados/services/validacao.py:131`; `app/chamados/services/validacao.py:133`; `app/chamados/services/validacao.py:188`; `app/chamados/services/validacao.py:190`; `app/chamados/services/disputa_emissao.py:1260`; `app/chamados/services/disputa_emissao.py:1266`; `app/chamados/services/disputa_emissao.py:1358`; `app/chamados/services/disputa_emissao.py:1378`) |
| `validada_por` | Os mesmos quatro atos: atribuição do autor na validação e `None` nos desfazimentos. (`app/chamados/services/validacao.py:132`; `app/chamados/services/validacao.py:189`; `app/chamados/services/disputa_emissao.py:1261`; `app/chamados/services/disputa_emissao.py:1358`) |
| `resposta_colab` | Resposta do colaborador; limpeza para `''` na correção; resposta automática. (`app/chamados/services/disputa_emissao.py:1247`; `app/chamados/services/disputa_emissao.py:1266`; `app/chamados/services/disputa_emissao.py:1358`; `app/chamados/services/disputa_emissao.py:1378`; `app/chamados/services/disputa_emissao.py:1393`; `app/chamados/services/disputa_emissao.py:1395`) |

`ressuscitar_perguntas` determina a limpeza de `respondida_em`, `validada_em` e `validada_por`, mas entrega a persistência à porta de lote. As atribuições desses instantes em `_universo_da_ressuscitacao` atingem somente uma cópia em memória, sem save. (`app/chamados/models.py:1488`; `app/chamados/models.py:1427`; `app/chamados/models.py:1439`; `app/chamados/models.py:1440`; `app/chamados/models.py:1441`)

Há **dois escritores adicionais do terceiro campo do carimbo**, `via_resolucao`. Ambos são exceções explícitas de `PORTA_VIA`; portanto sua existência não viola a whitelist atual, embora impeça dizer literalmente que somente as seis portas escrevem a tupla inteira. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:35`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:51`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:238`)

`materializada_por` e `validada_por` também podem ser zerados pelo mecanismo `on_delete=SET_NULL`. Esse efeito não passa pelas seis portas, e o selo declara expressamente que não o alcança. (`app/chamados/models.py:2742`; `app/chamados/models.py:2762`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:113`)

Existe ainda o canal genérico de nascimento `PerguntaDisputa.abrir(**campos)`, que repassa os argumentos a `objects.create`. Não foi localizado chamador atual fornecendo por ele esses campos; impossibilidade de fornecimento dinâmico: **[nao verificado]**. (`app/chamados/models.py:2374`; `app/chamados/models.py:2452`)

O materializador de regularização escreve campos homônimos de **`RegularizacaoExterna`**, não de `PerguntaDisputa`; essa distinção está registrada na allowlist. (`app/chamados/services/regularizacao_ext.py:198`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:47`)

As leituras que determinam o comportamento discutido são:

| Campo | Quem lê e para quê |
|---|---|
| `materializada_em` | C6: tranca/identidade, marco e lastro; `.estado`: materialização; materializador: seleção das ainda não materializadas; raiox: consumo; procedência: instante do ato. (`app/chamados/juizes.py:1042`; `app/chamados/juizes.py:1071`; `app/chamados/juizes.py:1077`; `app/chamados/juizes.py:1107`; `app/chamados/models.py:2477`; `app/chamados/services/materializacao.py:1261`; `app/chamados/services/raiox.py:133`; `app/ponto/services/procedencia_batida.py:74`) |
| `materializada_por` | Porta: fotografia e restauração; materializador: fotografia anterior; procedência: autoria. (`app/chamados/models.py:2552`; `app/chamados/models.py:2582`; `app/chamados/services/materializacao.py:618`; `app/ponto/services/procedencia_batida.py:71`) |
| `via_resolucao` | C6: tranca/identidade; relavra: elegibilidade e mudança; materializador: paridade; apresentação: explicação ao colaborador. (`app/chamados/juizes.py:1042`; `app/chamados/juizes.py:1071`; `app/chamados/services/relavra_via.py:116`; `app/chamados/services/relavra_via.py:119`; `app/chamados/services/materializacao.py:423`; `app/templates/chamados/partials/_card_atendimento.html:186`) |
| `respondida_em` | C6: resposta exige data **e** texto; `.estado` e `.respondida`: rótulo; validação: pré-condição; raiox: partição; HX: respostas aguardando consumo; template: apresentação. (`app/chamados/juizes.py:1047`; `app/chamados/models.py:2479`; `app/chamados/models.py:2783`; `app/chamados/services/validacao.py:363`; `app/chamados/services/raiox.py:136`; `app/chamados/juizes.py:1634`; `app/templates/chamados/partials/modal_fio.html:212`) |
| `validada_em` | Primeiro degrau do C6; `.estado`; validação idempotente; seleção do materializador; seleção do fecho sem credor; resposta sem veredito do C7. (`app/chamados/juizes.py:1033`; `app/chamados/models.py:2475`; `app/chamados/services/validacao.py:366`; `app/chamados/services/materializacao.py:1260`; `app/chamados/services/disputa_emissao.py:1620`; `app/chamados/models.py:1768`) |
| `validada_por` | Fotografias de validação/reversão, atribuição em painel/verbos e procedência. (`app/chamados/services/validacao.py:139`; `app/chamados/services/materializacao.py:616`; `app/chamados/services/painel.py:260`; `app/chamados/services/verbos.py:240`; `app/ponto/services/procedencia_batida.py:72`) |
| `resposta_colab` | C6: resposta e concordância com a luz; C7: resposta sem veredito mesmo sem timestamp; materializador: hora e guarda de texto; validação; apresentação; auditoria de meio-estado. (`app/chamados/juizes.py:1047`; `app/chamados/juizes.py:1010`; `app/chamados/models.py:1770`; `app/chamados/services/materializacao.py:264`; `app/chamados/services/materializacao.py:763`; `app/chamados/services/validacao.py:806`; `app/templates/chamados/partials/modal_fio.html:214`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1101`) |

O histórico também lê os instantes dinamicamente por `getattr`, e a fotografia de passivo lê os campos para reversão; essas leituras não são escritores adicionais. (`app/chamados/services/historico.py:99`; `app/chamados/services/historico.py:105`; `app/chamados/services/passivo_validado.py:64`; `app/chamados/services/passivo_validado.py:83`)

4. Patch não commitado. [nao verificado]

**(a) O retorno

[truncado em 20000 de 32629 caracteres]

## 01/11 16:40 · sessao 30d159e9fd65 · - · - · origem=Ronald (tmux)

Auditoria concluída e enviada com todas as referências executáveis das seis portas e limites do AST. [app/chamados/tests/test_o261e_f2_porta_carimbo.py:373]

Verificação em memória, sem Django ou alterações de arquivos:

- Mutante com chamada direta externa: teste da árvore vermelho; seis testes de fontes sintéticas verdes. [app/chamados/tests/test_o261e_f2_porta_carimbo.py:433] [nao verificado]
- Mutante usando `getattr(p, '_escrever_' + 'carimbo')`: sete testes novos verdes; scanner de escritores também não o reconhece. [app/chamados/tests/test_o261e_f2_porta_carimbo.py:403] [nao verificado]
- Mutante sem ofuscação, trocando `VIA_PARIDADE_RECUSADA` por `VIA_ADMIN_DECIDIU`: **oito testes pedidos verdes**; a whitelist valida função e arquivo, sem validar a via usada. [app/chamados/services/materializacao.py:858; app/chamados/tests/test_o261e_f2_porta_carimbo.py:388]
- Mutante restaurando `alvo.update(...)` no originário: teste novo do censo vermelho pela âncora desaparecida. [app/chamados/models.py:1488; app/core/tests/test_censo_escritas.py:235]

Resultado da suíte Django completa: [nao verificado]. [nao verificado]

## 01/11 16:39 · sessao 7d4e42e59c6f · - · - · origem=Ronald (tmux)

Q3/Q4b — achados consolidados, somente leitura.

Os dois escritores adicionais de `via_resolucao` são deliberadamente permitidos pelo selo separado `PORTA_VIA`; portanto existem fora das seis portas perguntadas, mas não constituem fuga da allowlist atual. O contrato de exclusividade dos campos `materializada_em`/`materializada_por` é distinto do contrato da via. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:35`, `:44`, `:51`, `:238`)

| Campo | Escritores persistentes fora das seis portas |
|---|---|
| `materializada_em` | Nenhum escritor explícito de `PerguntaDisputa` encontrado nas formas varridas; a atribuição está em `_escrever_carimbo`, e o lote limpa pelo gerente do modelo. (`app/chamados/models.py:2529`, `:2626`) |
| `materializada_por` | Idem; atribuição de FK por `_id` em `_escrever_carimbo`, limpeza no lote. (`app/chamados/models.py:2530`, `:2627`) |
| `via_resolucao` | `PerguntaDisputa.encerrar_por_fato_no_espelho`: atribui e salva somente a via; `relavra_via.relavrar`: atribui a nova via, relavra veredito e salva. (`app/chamados/models.py:2636`, `:2637`; `app/chamados/services/relavra_via.py:123`, `:124`, `:125`) |
| `respondida_em` | `responder_pergunta_disputa_core` atribui `now()` e salva; `reabrir_para_correcao` limpa no `update`; `responder_automaticamente` atribui o instante recebido e salva; `carimbar_resposta_pela_trilha` atribui o instante testemunhado e salva. (`app/chamados/services/disputa_emissao.py:1248`, `:1266`, `:1358`, `:1378`, `:1394`, `:1395`, `:1410`, `:1411`) |
| `validada_em` | `_carimbar_validacao` atribui `now()` e salva; `devolver_validacao_recusada` limpa; resposta nova limpa validação anterior; reabertura para correção limpa em lote. (`app/chamados/services/validacao.py:131`, `:133`, `:188`, `:190`; `app/chamados/services/disputa_emissao.py:1260`, `:1266`, `:1358`, `:1378`) |
| `validada_por` | Os mesmos quatro atos de `validada_em`, com autor na validação e `None` nos desfazimentos. (`app/chamados/services/validacao.py:132`, `:133`, `:189`, `:190`; `app/chamados/services/disputa_emissao.py:1261`, `:1266`, `:1358`, `:1378`) |
| `resposta_colab` | `responder_pergunta_disputa_core` escreve a resposta; `reabrir_para_correcao` escreve `''`; `responder_automaticamente` escreve o texto recebido. (`app/chamados/services/disputa_emissao.py:1247`, `:1266`, `:1358`, `:1378`, `:1393`, `:1395`) |

`ChamadoColaborador.ressuscitar_perguntas` fornece `validada_em=None`, `validada_por=None`, `respondida_em=None` à porta de lote; não executa um segundo `update` próprio desses campos. (`app/chamados/models.py:1488`)

As atribuições `_q.validada_em=None` e `_q.respondida_em=None` são simulação em cópia de memória: o código copia a pergunta, copia `_state` e o cache das relações, aplica `descarimbar(salvar=False)` e consulta o C6. Não persiste essa `_q`. (`app/chamados/models.py:1427`, `:1430`, `:1431`, `:1439`, `:1440`, `:1441`, `:1442`)

Há também comportamento de exclusão de usuário: ambas as FKs de autoria usam `on_delete=SET_NULL`, o que pode zerar autoria fora dessas portas. O próprio varredor declara que não alcança esse mecanismo. (`app/chamados/models.py:2740`, `:2742`, `:2760`, `:2762`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:113`)

`regularizacao_ext.materializar_regularizacao_externa` escreve campos homônimos em `RegularizacaoExterna`, não em `PerguntaDisputa`; a exceção está explicitada na allowlist. (`app/chamados/services/regularizacao_ext.py:198`, `:199`, `:200`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:47`, `:49`)

`PerguntaDisputa.abrir` recebe `**campos` e os repassa a `objects.create`, portanto é um canal genérico de criação capaz de receber esses campos; nenhum caller atual fornecendo os campos examinados apareceu na varredura explícita. A impossibilidade de fazê-lo dinamicamente não está provada. (`app/chamados/models.py:2374`, `:2452`, `:2453`; [nao verificado] para ausência universal de argumentos dinâmicos)

Leitores que explicam o efeito dos campos:

| Campo | Leitores e consequência |
|---|---|
| `materializada_em` | C6 lê identidade/tranca, fato do marco e lastro; `.estado` distingue validada/materializada; materializador seleciona apenas nulo; raiox exclui materialização com veredito `VL_SIM`; procedência usa a data como alternativa à validação. (`app/chamados/juizes.py:1042`, `:1071`, `:1077`, `:1107`; `app/chamados/models.py:2475`, `:2480`; `app/chamados/services/materializacao.py:1261`; `app/chamados/services/raiox.py:133`; `app/ponto/services/procedencia_batida.py:74`) |
| `materializada_por` | Foto de reversão do ato, autoria da procedência e rollback em memória da porta. (`app/chamados/services/materializacao.py:618`; `app/ponto/services/procedencia_batida.py:71`, `:72`; `app/chamados/models.py:2552`, `:2581`, `:2582`) |
| `via_resolucao` | C6 julga identidade/tranca; relavra escolhe somente vias derivadas; materializador reconhece recusa de paridade; template escolhe explicação ao colaborador. (`app/chamados/juizes.py:1042`, `:1071`; `app/chamados/services/relavra_via.py:116`, `:119`; `app/chamados/services/materializacao.py:423`; `app/templates/chamados/partials/_card_atendimento.html:186`) |
| `respondida_em` | C6 só entra no degrau da resposta se data e texto existem; `.estado` distingue pendente/respondida; validação recusa sem data; fila global exige data; raiox separa respondidas/não respondidas pela coluna; template condiciona apresentação. (`app/chamados/juizes.py:1047`, `:1284`; `app/chamados/models.py:2479`, `:2482`; `app/chamados/services/validacao.py:363`; `app/chamados/services/raiox.py:136`, `:137`, `:138`; `app/templates/chamados/partials/modal_fio.html:212`) |
| `validada_em` | Primeiro degrau do C6 mata a pergunta; validação evita repetição; materializador exige não nulo; fecho sem credor seleciona só nulo. (`app/chamados/juizes.py:1033`; `app/chamados/services/validacao.py:366`; `app/chamados/services/materializacao.py:1260`; `app/chamados/services/disputa_emissao.py:1620`) |
| `validada_por` | Foto da validação e reversão, métricas de autoria e procedência da batida. (`app/chamados/services/validacao.py:139`; `app/chamados/services/materializacao.py:616`; `app/chamados/services/painel.py:260`; `app/chamados/services/verbos.py:240`, `:244`; `app/ponto/services/procedencia_batida.py:72`) |
| `resposta_colab` | C6 exige texto com data e compara com a luz; materializador interpreta a hora e recusa texto vazio; validação classifica a resposta; telas exibem texto; auditoria detecta meio-estado. (`app/chamados/juizes.py:1047`, `:1010`; `app/chamados/services/materializacao.py:264`, `:393`, `:763`; `app/chamados/services/validacao.py:806`; `app/templates/chamados/partials/modal_fio.html:214`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1101`, `:1102`, `:1103`) |

O histórico lê dinamicamente os três instantes por `getattr(p, campo)` e usa o texto da resposta para compor os eventos; a foto de reversão de passivo também lê dinamicamente os campos, sem escrevê-los. (`app/chamados/services/historico.py:99`, `:101`, `:103`, `:105`, `:109`; `app/chamados/services/passivo_validado.py:64`, `:66`, `:83`)

Para Q4b, não encontrei escritor atual que grave `resposta_colab` **preenchida** deixando de atribuir também `respondida_em`: o core atribui o par e salva junto; automático também atribui e salva o par. A limpeza de correção deixa `resposta_colab=''` com data nula, que é outro estado. (`app/chamados/services/disputa_emissao.py:1247`, `:1248`, `:1249`, `:1266`, `:1393`, `:1394`, `:1395`, `:1358`, `:1378`)

A porta automática não valida `instante is not None`, mas o único caller de produção localizado passa `agora=timezone.now()`. Assim, a possibilidade da API interna produzir meio-estado existe se usada com `None`; sua ocorrência nos dados fornecidos e a origem histórica dos registros continuam [nao verificado]. (`app/chamados/services/disputa_emissao.py:1382`, `:1391`, `:1393`, `:1394`; `app/api/foto_recebida.py:42`, `:48`)

A frase da sonda “só dado legado” não é uma conclusão provada pelo anexo: ele não traz resultados, datas de escrita ou trilha dos registros. [nao verificado]

## 01/11 16:39 · sessao 8051cb744c18 · - · - · origem=Ronald (tmux)

4e — A sonda usa funções reais, mas não prova uma execução completa do patch: `carimbar` consulta C6 e salva somente quando morta (`app/chamados/models.py:2565`, `app/chamados/models.py:2576`); `reabrir` usa `recusa_da_reabertura` e executa efeitos reais (`app/chamados/models.py:1338`, `app/chamados/models.py:1380`). A execução/saída `SONDA_OK` não consta do dado apresentado. [nao verificado]

O `atomic` externo com exceção desfaz as escritas da conexão participante; isso é ensaio com rollback, não conexão configurada para somente leitura. A configuração sombra declara apenas `default`, herdado de saas (`app/config/settings/sombra.py:27`, `app/config/settings/saas.py:83`); trilhas usam ORM sem outra conexão (`app/core/observ.py:133`). Ausência absoluta de escritas por qualquer conexão ou dependência transitiva: [nao verificado].

Há escapes concretos do procedimento documentado:

- O comando indicado chama `lavra_de_prod` antes da sonda (`bin/sombra.sh:489`). Essa função cria diretório e arquivos (`bin/sombra.sh:137`, `bin/sombra.sh:144`, `bin/sombra.sh:151`) e lança outro processo que grava `MetricaSnapshot` por dois `update_or_create`, fora do `atomic` da sonda (`bin/sombra.sh:157`, `bin/sombra.sh:160`, `bin/sombra.sh:166`).
- O wrapper grava a saída no host usando `tee` (`bin/sombra.sh:492`). O app está montado somente para leitura, mas `/sombra` é gravável e logs/media são tmpfs (`bin/sombra.sh:189`).
- Logging não participa do rollback: exceções do cartório chegam ao logger `ponto` (`app/ponto/services/cartorio.py:1763`), configurado com arquivo (`app/config/settings/saas.py:162`, `app/config/settings/saas.py:181`). `registrar_engolido` também modifica contador em memória e emite logging (`app/core/observ.py:75`, `app/core/observ.py:79`).
- Até C6 pode engolir exceção e continuar, portanto “qualquer exceção sobe” não descreve todas as funções chamadas (`app/chamados/juizes.py:1104`, `app/chamados/juizes.py:1115`); o reconciliador também captura falhas por chamado (`app/chamados/reconciliador.py:1369`).

A medição de laço cobre `reconciliar_chamados_colab`, que chama os reconciliadores e percorre chamados vivos (`app/chamados/reconciliador.py:1348`, `app/chamados/reconciliador.py:1356`). Não cobre a rodada do consumidor do outbox (`app/chamados/reconciliador.py:1255`) nem os efeitos pendentes em `on_commit` (`app/chamados/signals.py:121`, `app/chamados/signals.py:202`, `app/chamados/signals.py:251`). O outbox encontrado é tabela ORM, não fila externa (`app/ponto/portas/celula.py:626`, `app/ponto/portas/celula.py:631`). Ausência de laço após commit e consumidores: [nao verificado].

Item 5 — O registro vivo aponta C6 para `chamados.juizes.pergunta_viva` (`app/core/juizes.py:266`), e a porta efetivamente o consulta (`app/chamados/models.py:2565`): nenhum segundo juiz é necessário para o patch proposto. A dependência problemática é a promessa de reabrir sem demonstrar que as guardas aceitam o fixture: a porta recusa antes de mudar estado (`app/chamados/models.py:1285`, `app/chamados/models.py:1338`).

Os novos testes de formas são testes sobre strings artificiais (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:445`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:469`); a fiscalização da produção é o teste que lê a árvore (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:433`). O censo novo lê fonte real, mas exige uma âncora textual específica e sua substituição (`app/core/tests/test_censo_escritas.py:233`, `app/core/tests/test_censo_escritas.py:235`, `app/core/tests/test_censo_escritas.py:244`). Não equivale a prova universal contra mutações de escrita.

## 01/11 16:18 · sessao 4f8555cb3c3e · O261-E-F2 · censo · origem=Code

Nos anexos transcritos, conto linhas desde a primeira linha de cada arquivo, incluindo linhas em branco e excluindo o marcador `== anexo ==`.

1. **(a) — Codex não viu, com o tema perguntado.**  
   **Pergunta:** “selo/RED sem mordida (o selo AST de test_o261e_f2_porta_carimbo.py morde mutante real?)” (`codex_v1.q:25`); reiterada para o diff total (`codex_v3.q:14`).  
   **Resposta/triagem sobre o bypass por chamadas permitidas:** **nenhum**. As respostas apontaram escapes nas formas de atribuição, outro defeito do selo (`codex_v1.r:150`; `codex_v1.r:152`). O selo permite `_escrever_carimbo`, cuja implementação escreve sem consultar C6; a consulta está em `carimbar` (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:44`; `app/chamados/models.py:2527`; `app/chamados/models.py:2565`).

2. **(b) — Codex viu; a triagem reduziu a nota.**  
   **Pergunta:** comportamento observável de cada chamador “quando a porta RECUSA”, incluindo o que devolve ou emite (`codex_v1.q:15`; `codex_v1.q:16`; `codex_v1.q:17`).  
   **Resposta:** para `_materializar_ausencia_de_resposta`, “Ignora o booleano. A ausência já criada/reaberta continua” (`codex_v1.r:93`).  
   **Triagem/motivo:** “Nota. O Codex mesmo diz que o conjunto segue restrito ao efetivamente carimbado” (`triagem_v3.md:9`). O núcleo — recusa ignorada e redução do conjunto absorvido — foi tratado; a formulação específica “sem trilha/log” não aparece nesse tratamento (`codex_v1.r:93`; `triagem_v3.md:9`).

3. **(b) — Os dois componentes foram vistos e encaminhados à fila.**  
   **Pergunta:** possibilidade de gravar e reler VIVA; divergência entre memória e banco; ordem de trava (`codex_v1.q:18`; `codex_v1.q:20`; `codex_v1.q:26`).  
   **Resposta:** “sem reler ou travar a pergunta” e “exceção da lavra ou do `save`, depois da aprovação, fica fora do `try` de reversão” (`codex_v1.r:109`; `codex_v1.r:126`).  
   **Triagem/motivos:** concorrência anterior ao diff e alegada mudança da ordem de trava de sete chamadores (`triagem_v1.md:9`; `triagem_v2.md:10`). Para memória, a triagem corrigiu “o atomic cobre”, reconheceu que a memória permanece carimbada e encaminhou: “Não é regressão; vai a fila” (`triagem_v3.md:11`).

4. **(b) — Relógio visto e remetido a complemento.**  
   **Pergunta:** “relogio lido fora da borda” (`codex_v1.q:24`), repetida na volta final (`codex_v3.q:13`).  
   **Resposta:** “Os novos métodos têm `now()` de fallback”; a injeção integral permanece incompleta (`codex_v1.r:144`).  
   **Triagem/motivo:** “a leitura do relogio MUDOU DE SITIO, nao nasceu”, encaminhando ao complemento RELOGIO-INJETADO (`triagem_v2.md:9`; confirmado em `triagem_v3.md:12`). O detalhe de `encerrar_fantasma` chamar sem `quando` está no código, mas não foi explicitamente nomeado na resposta citada (`app/chamados/models.py:2648`; `codex_v1.r:144`).

5. **(b) — Visto; descarte agregado, sem motivo individual registrado.**  
   **Pergunta:** “derivacao paralela de fonte unica” (`codex_v1.q:24`; `codex_v1.q:25`).  
   **Resposta:** “Permanece a separação já existente entre identidade no C6, no queryset e na lista da relavra”, citando os dois sítios do opus (`codex_v1.r:145`).  
   **Triagem:** encerra agregadamente “2o juiz, derivacao paralela e 2o escritor explicito: nenhum” (`triagem_v2.md:12`). **Classifico em (b) por esse encerramento agregado; motivo específico da triagem para manter as listas 5×4: [nao verificado].** A preexistência foi registrada pela resposta, não como justificativa individual da triagem (`codex_v1.r:145`; `triagem_v2.md:12`).

6. **(b) — Os três restos foram vistos e deixados para outro pouso.**   [nao verificado]
   **Pergunta:** “Algum chamador de `fechar_por_competencia_trancada` [...] sobrou?”, além do mandato “L-111 apaga” (`codex_v1.q:27`; `codex_v1.q:7`).  
   **Resposta:** “Restam referências instrumentais/documentais obsoletas dizendo que a porta existe”, citando exatamente os três arquivos do opus (`codex_v1.r:160`).  
   **Triagem/motivo:** “INCOMPLETO declarado. E instrumento (`bin/`) e doc, com pouso proprio (RAIAS 5)” (`triagem_v1.md:15`). [nao verificado]

7. **(a) — Codex não viu, com o tema perguntado.**  
   **Pergunta:** “selo/RED sem mordida”, reiterada para o “diff total” (`codex_v1.q:25`; `codex_v3.q:14`).  
   **Resposta/triagem sobre a perda da testemunha da classe (f): nenhum.** O teste passa a exigir uma chamada AST a `descarimbar_em_lote` e um `update()` censado diretamente, sem `[relacionado]` (`app/core/tests/test_censo_escritas.py:210`; `app/core/tests/test_censo_escritas.py:214`). É uma lacuna estrutural inspecionável no teste; classificá-la não depende de recuperar uma execução histórica de RED (`app/core/tests/test_censo_escritas.py:202`).

8. **(e) — Não perguntado quanto ao custo da dupla execução.**  
   **Pergunta mais próxima:** menciona “memo `_pv_memo`”, mas pergunta se a porta grava e a releitura resulta VIVA; não pede avaliação de custo ou quantidade de consultas ao C6 (`codex_v1.q:18`; `codex_v1.q:19`).  
   **Resposta relacionada:** descreve que `lavrar_sem_turno_aberto` “antes salvava depois do pré-C6”, sem apontar o custo da segunda avaliação (`codex_v1.r:88`).  
   **Resposta/triagem do achado de custo: nenhum.** Os dois pontos de chamada e a alteração dos campos da chave estão no código (`app/chamados/reconciliador.py:398`; `app/chamados/reconciliador.py:405`; `app/chamados/models.py:2565`; `app/chamados/juizes.py:790`; `app/chamados/juizes.py:791`).

9. **(e) — Não perguntado quanto ao vocabulário do commit/comentário.**   [nao verificado]
   **Trechos delimitadores:** as perguntas pedem escritores, leitores e classes de defeitos; nenhuma pede auditoria lexical do commit ou da palavra “fallback” em comentários (`codex_v1.q:11`; `codex_v1.q:23`; `codex_v2.q:17`; `codex_v3.q:13`).   [nao verificado]
   **Resposta/triagem deste achado: nenhum.** O “fallback” mencionado na resposta refere-se ao relógio, outro tema (`codex_v1.r:144`). A ocorrência no comentário está confirmada (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:404`); a ocorrência no corpo original do commit, além do relato do opus, é **[nao verificado]**.

| achado | classe | volta | trecho |
|---|---|---|---|
| 1 — chamadores sem selo | **(a)** | v1/v3 | Perguntado “selo/RED sem mordida”; resposta específica: nenhum (`codex_v1.q:25`; `codex_v3.q:14`). |
| 2 — recusa silenciosa na ausência | **(b)** | v1 → triagem v3 | “Ignora o booleano”; triagem: conjunto restrito ao efetivamente carimbado (`codex_v1.r:93`; `triagem_v3.md:9`). |
| 3 — trava e memória após erro | **(b)** | v1–v3 | Janela anterior; memória: “Não é regressão; vai a fila” (`triagem_v1.md:9`; `triagem_v3.md:11`). |
| 4 — relógio nas portas | **(b)** | v1–v3 | “MUDOU DE SITIO, nao nasceu”; complemento RELOGIO-INJETADO (`triagem_v2.md:9`). |
| 5 — duas listas de identidade | **(b)** | v1 → triagem v2 | Separação registrada; encerramento agregado “nenhum”; motivo individual ausente (`codex_v1.r:145`; `triagem_v2.md:12`). |
| 6 — restos de L-111 | **(b)** | v1 | “INCOMPLETO declarado”; instrumento/doc com pouso próprio (`triagem_v1.md:15`). | [nao verificado]
| 7 — testemunha da classe (f) | **(a)** | v1/v3 | Perguntado “selo/RED sem mordida”; resposta específica: nenhum (`codex_v1.q:25`; `codex_v3.q:14`). |
| 8 — custo de dois C6 | **(e)** | v1, pergunta mais próxima | Memo perguntado quanto à vivacidade, sem pergunta de custo (`codex_v1.q:18`; `codex_v1.q:19`). |
| 9 — palavra “fallback” | **(e)** | v1–v3 | Perguntas de defeitos técnicos, sem auditoria lexical (`codex_v1.q:23`; `codex_v2.q:17`; `codex_v3.q:13`). |

## 01/11 16:17 · sessao a802cb69c575 · E4-CARGA · verificador · origem=Code

A mudança corrige um repasse entre contratos incompatíveis: `dias_encerrados` recebe `{(colaborador_id, data): célula}`, enquanto `turnos_do_colab` espera `{data: célula}` com pad. O encerramento continua usando o mesmo julgamento puro. (`app/ponto/turnos.py:2130`, `app/ponto/turnos.py:1852`, `app/ponto/turnos.py:1845`, `app/ponto/turnos.py:2153`)

**Não considero demonstrada a conclusão operacional da obra:** existe exigência explícita de “RED + impacto na sombra”; execução desses testes, impacto e sequência efetiva de pouso estão **[não verificado]**. (`app/docs/BACKLOG.md:861`)

**1. Quem produz e quem consome os valores**

| Valor | Quem produz | Quem lê |
|---|---|---|
| `dias_encerrados(..., celulas=...)` | Chamador; na ausência, `celulas_do_periodo([pk], ini, fim)`, com chave composta. (`app/ponto/turnos.py:2145`, `app/escala/alimentacao.py:41`) | `_dias_encerrados_puro`: alimenta `vinculo_do_dia` e o julgamento do fim previsto/DNA. (`app/ponto/turnos.py:2170`, `app/ponto/turnos.py:2119`, `app/escala/alimentacao.py:237`) |
| `turnos_do_colab(..., celulas=...)` | Chamador; na ausência, carrega `[ini−2, fim+1]` e converte para chave `data`. (`app/ponto/turnos.py:1845`, `app/ponto/turnos.py:1882`) | Candidatura das geradoras (`mais`), papel da ata e escolha do vínculo de cada dia. A segmentação recompõe a chave `(pk,data)`. (`app/ponto/turnos.py:1885`, `app/ponto/turnos.py:1937`, `app/ponto/turnos.py:1954`) |
| Retorno sem motivos | `_dias_encerrados_puro` escreve `{data: (_m is None)}`. (`app/ponto/turnos.py:2172`) | Consumidores booleanos listados abaixo. |
| Retorno com motivos | O mesmo corpo escreve `{data: motivo ou None}`; `None` significa encerrado. (`app/ponto/turnos.py:2132`, `app/ponto/turnos.py:2172`) | Calendário; no método do lote, leitor de status. (`app/colaboradores/services/calendario.py:701`, `app/ponto/status_dia.py:196`) |

Todos os sítios de produção encontrados que passam `celulas` explicitamente às duas funções, incluindo aliases e argumento posicional:

| Chamada | Forma e janela da alimentação | Muda com o diff? |
|---|---|---|
| Cartório → `dias_encerrados` | `{(c.pk,x.data): x}` da lista `cels`; limites `d0=cels[0].data`, `d1=cels[-1].data`. A lista pode ter lacunas. (`app/ponto/services/cartorio.py:919`, `app/ponto/services/cartorio.py:942`) | **Sim**, quando a chamada ocorre: não fornece `turnos`; agora o pareador carrega suas células com pad. (`app/ponto/turnos.py:2154`) |
| Calendário → `_dias_encerrados` | `_cels_mes`, composta, carregada em `[data_ini,data_fim]`; fornece também os turnos dessa janela. (`app/colaboradores/services/calendario.py:395`, `app/colaboradores/services/calendario.py:699`) | **Não**: `turnos` fornecido evita o ramo alterado. (`app/ponto/turnos.py:2154`) |
| `dia_encerrado_motivo` → `turnos_do_colab` | Repasse posicional do próprio parâmetro; deveria receber `{data}` cobrindo `[data−2,data+1]`. (`app/ponto/turnos.py:2096`, `app/ponto/turnos.py:1852`, `app/ponto/turnos.py:1845`) | **Não**: esse repasse permanece. (`app/ponto/turnos.py:2096`) |
| Grade, antes do alinhamento → `_tdc_pre` | Converte `_cels_gr` para `{data}`; carga de `janela_com_pad(_ini_al,ref)`. (`app/escala/utils.py:1258`, `app/escala/utils.py:1260`, `app/escala/utils.py:1532`) | **Não**: chamada direta preservada. (`app/escala/utils.py:1532`) |
| Grade, depois dos overlays → `turnos_do_colab` | `{data}` do mesmo pad; dentro da montagem, cópias com ata fresca não salva; fora dela, células persistidas. (`app/escala/utils.py:1830`, `app/escala/utils.py:1833`, `app/escala/utils.py:1835`, `app/escala/utils.py:1847`) | **Não**: essa alimentação continua chegando diretamente ao pareador. (`app/escala/utils.py:1847`) |
| Emissor de turno aberto retroativo → `turnos_do_colab` | `{data}`, carregado em `[ini−2,fim+2]`, portanto um dia além do pad necessário. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:86`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88`, `app/ponto/turnos.py:1845`) | **Não**: chamada direta preservada. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88`) |

O repasse singular merece uma ressalva: o fechamento fornece `{data}` recortado pelo pad, mas o vigia fornece `{(pk,data)}` de uma única célula, que atravessa `dia_encerrado` e ainda chega incompatível ao pareador. É uma ocorrência **preexistente, não curada por este diff**. (`app/ponto/services/fechamento.py:354`, `app/ponto/services/fechamento.py:361`, `app/ponto/services/vigia_de_hora.py:57`, `app/ponto/turnos.py:2075`, `app/ponto/turnos.py:2096`)

Os leitores de produção do retorno da **função de módulo** são:

| Leitor | Uso |
|---|---|
| Cartório | Bool com default `False`; decide `_em_curso` e alimenta o classificador. (`app/ponto/services/cartorio.py:943`, `app/ponto/services/cartorio.py:1180`, `app/ponto/services/cartorio.py:1253`) |
| Calendário | Motivos; distingue dia em curso e `sem_previsto`. (`app/colaboradores/services/calendario.py:699`, `app/colaboradores/services/calendario.py:701`, `app/colaboradores/services/calendario.py:709`) |
| Comando `supra_juiz` | Bool passado ao classificador. (`app/ponto/management/commands/supra_juiz.py:121`, `app/ponto/management/commands/supra_juiz.py:232`) |
| Emissor de furo retroativo | Bool passado ao classificador. (`app/ponto/management/commands/emitir_furo_retroativo.py:62`, `app/ponto/management/commands/emitir_furo_retroativo.py:108`) |
| `par_completo_da_janela` | Bool para excluir turnos de dias não encerrados. (`app/ponto/turnos.py:2183`, `app/ponto/turnos.py:2186`) |

O **método homônimo do lote** tem leitores no status, com motivos, e no painel, com bool. Ele chama diretamente `_dias_encerrados_puro`, sem atravessar a linha alterada. (`app/ponto/status_dia.py:196`, `app/core/services/painel_op.py:334`, `app/ponto/turnos.py:3018`)

**2. Contraexemplos, custo e memória**

**Resultado antes correto que passe a errado: [não verificado].** Os novos casos fixam correções nos dois sentidos: X deve encerrar às 08h; Y deve continuar aberto nesse instante. Isso demonstra a expectativa da fixture, não a ausência universal de regressões. (`app/ponto/tests/test_bug_enc_celulas_forma.py:49`, `app/ponto/tests/test_bug_enc_celulas_forma.py:50`)

**O custo muda no cartório:** passa a existir uma consulta adicional de células por chamada efetiva de encerramento, pois o pareador agora recebe `None` e executa `celulas_do_periodo`. `_enc` memoiza o resultado por execução de `_julgar_colab_corpo`, evitando uma carga por dia. (`app/ponto/turnos.py:2154`, `app/ponto/turnos.py:1882`, `app/escala/alimentacao.py:41`, `app/ponto/services/cartorio.py:939`)

As células adicionais do pad também podem acrescentar geradoras e disparar a consulta condicional de `mais`; alterações adicionais nos caminhos preguiçosos e relevância em tempo de produção estão **[não verificado]**. O lote conserva suas cargas e seu acesso direto ao corpo puro. (`app/ponto/turnos.py:1885`, `app/escala/alimentacao.py:123`, `app/ponto/turnos.py:2957`, `app/ponto/turnos.py:3018`)

A ordem do ramo normal do cartório é:

1. Monta a grade; o pareamento final dessa montagem recebe cópias com **ata fresca em memória**. (`app/ponto/services/cartorio.py:963`, `app/escala/utils.py:1835`, `app/escala/utils.py:1847`)
2. Consulta encerramento sob demanda, por exemplo em `_em_curso`; agora esse pareador recarrega células persistidas. (`app/ponto/services/cartorio.py:1180`, `app/ponto/services/cartorio.py:942`, `app/ponto/turnos.py:1884`)
3. Depois, no apply, lavra a célula atual com `save=False` e salva por `carimbar`. (`app/ponto/services/cartorio.py:1593`, `app/ponto/services/cartorio.py:1596`, `app/ponto/services/cartorio.py:1616`, `app/ponto/portas/celula.py:807`)

Portanto, **há diferença de estado entre a ata fresca usada pela grade e a persistida usada pelo encerramento**. Entretanto, o cartório passa ao encerramento a lista original `cels`, não as cópias frescas da grade; esse diff não remove a alimentação direta dessas cópias. Divergência efetiva dry-run/apply causada pela mudança: **[não verificado]**. (`app/ponto/services/cartorio.py:942`, `app/escala/utils.py:1846`, `app/escala/utils.py:1847`)

Como `_enc` é preguiçoso, a primeira carga pode ocorrer numa iteração posterior: a ordem demonstrada é “antes da lavra **da célula atual**”, não necessariamente antes de todas as lavras da passagem. (`app/ponto/services/cartorio.py:939`, `app/ponto/services/cartorio.py:1180`, `app/ponto/services/cartorio.py:1596`)

No ramo `nunca_bateu`, a pergunta ocorre **somente dentro de `apply_`**, antes da lavra e do carimbo; o dry-run desse ramo não faz essa pergunta. Essa assimetria já está no chamador. (`app/ponto/services/cartorio.py:1040`, `app/ponto/services/cartorio.py:1063`, `app/ponto/services/cartorio.py:1075`, `app/ponto/services/cartorio.py:1097`)

**3. Os testes mordem?** [nao verificado]

A tabela descreve o que as asserções detectariam; execução dos mutantes e resultados efetivos: **[não verificado]**.

| Teste | Mutante plausível que pega | O que deixa passar | [nao verificado]
|---|---|---|
| `test_regra` | Ignorar geradora/intervalo e fechar Y; manter X aberto; retirar `mais` somente do lote. Exige estados e motivos literais. (`app/ponto/tests/test_e4_carga_paridade.py:129`, `app/ponto/tests/test_e4_carga_paridade.py:139`) | Inverter somente o retorno sem motivos; usa `motivos=True`. (`app/ponto/tests/test_e4_carga_paridade.py:107`, `app/ponto/tests/test_e4_carga_paridade.py:115`) |
| `test_paridade_carga_x_casca_em_fixture_mista` | Divergência exclusiva de um carregador, inclusive perder `mais`, batida ou identidade da saída; compara seis campos, motivos e atribuição das batidas. (`app/ponto/tests/test_e4_carga_paridade.py:55`, `app/ponto/tests/test_e4_carga_paridade.py:122`, `app/ponto/tests/test_e4_carga_paridade.py:148`) | Erro compartilhado pelos corpos puros, mantendo os dois lados iguais. (`app/ponto/turnos.py:1913`, `app/ponto/turnos.py:3009`, `app/ponto/turnos.py:3018`) |
| `test_MORDE_carga_sem_mais` | Como controle de sensibilidade, exige que a retirada simulada de `mais` alcance a carga e produza divergência em Y. (`app/ponto/tests/test_e4_carga_paridade.py:159`, `app/ponto/tests/test_e4_carga_paridade.py:162`, `app/ponto/tests/test_e4_carga_paridade.py:165`) | **Sozinho, deixa passar produção já sem `mais`**, porque continua exigindo divergência. Quem reprova essa retirada permanente são `test_regra` e `test_paridade`. (`app/ponto/tests/test_e4_carga_paridade.py:143`, `app/ponto/tests/test_e4_carga_paridade.py:148`, `app/ponto/tests/test_e4_carga_paridade.py:165`) |
| `test_RED_alimentado_igual_a_juiza_e_ao_lote` | Restaurar o repasse antigo da chave composta: constrói essa alimentação e compara com lote e singular. (`app/ponto/tests/test_bug_enc_celulas_forma.py:33`, `app/ponto/tests/test_bug_enc_celulas_forma.py:43`, `app/ponto/tests/test_bug_enc_celulas_forma.py:44`) | Erro compartilhado do julgamento; conversão bool errada; erro exclusivo do ramo com `turnos=` fornecido. (`app/ponto/tests/test_bug_enc_celulas_forma.py:33`, `app/ponto/turnos.py:2154`, `app/ponto/turnos.py:2172`) |
| `test_RED_regra_do_veredito_do_cartorio` | Repasse antigo ou perda do intervalo de Y; fixa X encerrado e Y aberto às 08h. (`app/ponto/tests/test_bug_enc_celulas_forma.py:49`, `app/ponto/tests/test_bug_enc_celulas_forma.py:50`) | Erros exclusivos de outros instantes, Z ou conversão bool: só consulta X/Y às 08h, com motivos. (`app/ponto/tests/test_bug_enc_celulas_forma.py:33`, `app/ponto/tests/test_bug_enc_celulas_forma.py:49`) |

**O patch alcança o lote:** o construtor faz import local de `escalas_do_periodo_por_colab`; `_lote` é executado dentro do patch, antes dessa resolução e chamada. (`app/ponto/turnos.py:2951`, `app/ponto/turnos.py:2963`, `app/ponto/tests/test_e4_carga_paridade.py:159`)

**A casca fica intacta:** é medida antes do patch e retorna assinaturas e mapas já materializados. Medida dentro dele, também seria mutada, pois a casca singular resolve a função plural pelo global do módulo. (`app/ponto/tests/test_e4_carga_paridade.py:55`, `app/ponto/tests/test_e4_carga_paridade.py:106`, `app/ponto/tests/test_e4_carga_paridade.py:158`, `app/escala/alimentacao.py:154`)

Há lacunas relevantes: não existe célula da fixture fora de D, logo a necessidade de **células no pad** não é testada independentemente; também não há célula fresca não salva, fluxo real do cartório, orçamento de consultas ou assert do modo booleano. (`app/ponto/tests/test_e4_carga_paridade.py:92`, `app/ponto/tests/test_e4_carga_paridade.py:107`, `app/ponto/tests/test_bug_enc_celulas_forma.py:32`, `app/ponto/tests/test_bug_enc_celulas_forma.py:33`)

**Não há dependência direta do relógio real nos testes novos:** datas/instantes são constantes, ambas as classes congelam o relógio e os julgamentos recebem `agora`. (`app/ponto/tests/test_e4_carga_paridade.py:39`, `app/ponto/tests/test_e4_carga_paridade.py:47`, `app/ponto/tests/test_e4_carga_paridade.py:100`, `app/ponto/tests/test_bug_enc_celulas_forma.py:24`, `app/ponto/tests/test_bug_enc_celulas_forma.py:33`)

**4. Classes de reprovação**

| Classe | SIM/NÃO e prova |
|---|---|
| **(a) Segundo juiz** | **NÃO introduzido.** Os registrados continuam sendo `turnos_do_colab` e `dia_encerrado`; o carregador delega ao mesmo julgamento puro. (`app/core/juizes.py:196`, `app/core/juizes.py:223`, `app/ponto/turnos.py:2153`, `app/ponto/turnos.py:2170`) |
| **(b) Segundo escritor fora da porta** | **NÃO introduzido.** A alteração troca alimentação de leitura; lavra e persistência seguem pelas portas existentes. (`app/ponto/turnos.py:2153`, `app/ponto/services/cartorio.py:1596`, `app/ponto/services/cartorio.py:1616`) |
| **(c) Relógio fora da borda** | **NÃO introduzido; SIM preexistente no sítio.** Os defaults internos de `timezone.now()` permanecem, apesar do corte de injeção. O cartório fornece `agora`. (`app/ponto/turnos.py:2140`, `app/ponto/turnos.py:2094`, `app/docs/CORTES.md:369`, `app/ponto/services/cartorio.py:942`) |
| **(d) Dois carregadores que podem divergir** | **SIM, preexistentes e mantidos.** Casca e lote carregam separadamente, mas usam os mesmos corpos. O selo novo verifica paridade na fixture; não elimina a duplicidade de composição das cargas. (`app/ponto/turnos.py:1919`, `app/ponto/turnos.py:2957`, `app/ponto/turnos.py:3009`, `app/docs/BACKLOG.md:862`) |
| **(e) Selo/RED sem mordida** | **NÃO inteiramente vazio:** há gabaritos literais e controle de sensibilidade. **SIM, cobertura incompleta:** bool, pad com células e estado não salvo escapam conforme os testes acima. RED executado: **[não verificado]**. (`app/ponto/tests/test_e4_carga_paridade.py:143`, `app/ponto/tests/test_e4_carga_paridade.py:165`, `app/ponto/tests/test_bug_enc_celulas_forma.py:33`) |
| **(f) Ordem de trava** | **NÃO alterada pela linha do diff.** Ela apenas remove o argumento de uma leitura. Existe hipótese preexistente de composição Colaborador→Disputa, registrada como não reproduzida; regressão de trava nesta obra: **[não verificado]**. (`app/ponto/turnos.py:2154`, `app/docs/BACKLOG.md:767`) |
| **(g) Pouso antes do pré-requisito** | **NÃO comprovado; cumprimento [não verificado].** Exige paridade antes da cura, cura depois do pouso E4-7, RED e impacto. A documentação declara E4-7 no ar, mas não comprova aqui a ancestralidade/execução efetiva. (`app/docs/BACKLOG.md:862`, `app/docs/BACKLOG.md:861`, `app/docs/BACKLOG.md:789`) |

**5. Leis dos sítios**

O grep encontra proteção explícita para consumo da célula/ata, teto temporal e jornada pelo dia de início; L-085 nomeia `turnos_do_colab`. O registro de juízes também reserva geometria ao pareador e marcos do dia à autoridade da célula. (`app/docs/LEIS.md:95`, `app/docs/LEIS.md:100`, `app/docs/LEIS.md:117`, `app/docs/JUIZES.md:20`, `app/docs/JUIZES.md:22`)

**Não identifiquei violação nova dessas regras pela linha alterada:** ela mantém a pergunta ao pareador e a conjunção turno vivo/fim previsto, sem criar julgamento por data. (`app/ponto/turnos.py:2154`, `app/ponto/turnos.py:2114`, `app/ponto/turnos.py:2122`)

As ressalvas são a leitura de relógio preexistente diante de `RELOGIO-INJETADO`, a cronologia exigida pela L-110 e os pré-requisitos específicos da obra. O fonte contém casos pela regra; não comprova que foram escritos/executados antes da cura nem fornece o impacto exigido. (`app/docs/CORTES.md:369`, `app/docs/LEIS.md:136`, `app/docs/BACKLOG.md:861`, `app/docs/BACKLOG.md:862`; execução e cronologia **[não verificado]**)

## 01/11 16:16 · sessao 0144ff325e9e · - · - · origem=Ronald (tmux)

- **Ordem no cartório:** `_turno_encerrado` monta e guarda o mapa uma vez, sob demanda, em `app/ponto/services/cartorio.py:939-943`. No ramo normal ele pode ser chamado em `:1180`, antes da lavra da célula atual em `:1596` (`save=False`) e do `carimbar` em `:1616`. `carimbar` chama a gravação em `app/ponto/portas/celula.py:807`; esta faz `save` e evento no mesmo `atomic`, em `:639-642`. A ressalva é o caráter preguiçoso: não afirmar que o mapa necessariamente foi calculado antes de **todas** as lavras da passagem. (`app/ponto/services/cartorio.py:937-943`, `:1136`, `:1180`)

- **Nunca bateu / dry-run:** a pergunta de encerramento em `app/ponto/services/cartorio.py:1063` está dentro do `if apply_` de `:1040`; vem antes da lavra em `:1075` e do `carimbar` em `:1097-1099`. Nesse ramo o dry-run não executa essa pergunta; é assimetria preexistente, não criada pela linha alterada. (`app/ponto/services/cartorio.py:1040`, `:1063`, `:1075`, `:1097`)

- **Fornecedores de `cels`:** todos os fornecedores externos encontrados carregam do banco: cron em `app/ponto/management/commands/processar_cartorio.py:61-65`; sombra do veredito velho dentro de `atomic` em `app/ponto/management/commands/celula_veredito_velho.py:133-145`; preenchimento de status em `app/ponto/management/commands/lavrar_status_competencia.py:48-56`; régua, restrita às células `veredito='furo'`, em `app/escala/regua_defesa.py:685-693`; unidade em `app/ponto/services/cartorio.py:1739-1754`. Internamente há segunda passada com lista ordenada em `:833` e releitura da lista original em `:898`. Nenhum desses trechos modifica célula em memória antes de passá-la ao julgamento. (mesmas referências)

- **Célula fresca ainda sem save:** há esse contrato na **grade**, que copia células, substitui sua ata em memória e alimenta diretamente `turnos_do_colab`, em `app/escala/utils.py:1829-1847`. Esse caminho não passa por `dias_encerrados`; portanto a alteração não descarta sua alimentação. No cartório, a montagem da grade ocorre em `app/ponto/services/cartorio.py:963`, mas a alimentação de encerramento continua sendo a lista original `cels` em `:942`, não `_cels_juiz` da grade. Uma divergência real dry-run/apply decorrente da nova leitura está [não verificado].

- **(a) segundo juiz: NÃO introduzido.** Os registrados são `turnos_do_colab` em `app/core/juizes.py:196` e `dia_encerrado` em `:223`; o carregador mantém `_dias_encerrados_puro`, que pergunta à mesma função pura por dia. (`app/ponto/turnos.py:2153-2155`, `:2169-2172`)

- **(b) segundo escritor: NÃO introduzido.** A alteração termina nas mesmas funções de leitura e retorna um mapa; a lavra segue pela porta existente. (`app/ponto/turnos.py:2145-2155`, `:2167-2174`; `app/ponto/services/cartorio.py:1596`, `:1616`; `app/ponto/portas/celula.py:709-713`)

- **(c) relógio fora da borda: NÃO introduzido, mas existe no sítio.** `dias_encerrados` já usa `timezone.now()` quando `agora=None`, em `app/ponto/turnos.py:2140-2141`; `dia_encerrado_motivo` também, em `:2094-2095`. O corte exige injeção de instante em juiz/grade/cartório. (`app/docs/CORTES.md:369`) O cartório injeta `agora` na chamada alterada. (`app/ponto/services/cartorio.py:942`)

- **(d) dois carregadores: SIM, preexistentes; regra paralela nova: NÃO.** Casca e lote são declarados como fornecedores do mesmo corpo em `app/ponto/turnos.py:1919-1924` e `:2159-2161`; a alteração passa a deixar o pareador carregar sua alimentação própria em `:2154`. A diferença de janela/estado entre a alimentação externa de encerramento e a carga do pareador permanece possível por desenho; ocorrência material em produção [não verificado].

- **(f) ordem de trava: NÃO alterada pelo diff.** A linha alterada apenas remove um argumento da chamada de leitura. (`app/ponto/turnos.py:2153-2155`) Há uma hipótese preexistente de composição Colaborador → Disputa via cartório, registrada expressamente como não reproduzida; não confundir isso com prova de regressão deste diff. (`app/docs/BACKLOG.md:767`)

- **(g) pouso antes do pré-requisito:** o pré-requisito específico existe: a ficha exige “RED + impacto na sombra” e determina tocar `turnos.py` somente depois do pouso E4-7. (`app/docs/BACKLOG.md:861`) A ficha do E4-7 declara pouso em 31/10, `a62862d4`, suíte 13124 e aprovação. (`app/docs/BACKLOG.md:789`) A execução efetiva dessa sequência e do impacto da cura está [não verificado].

- **Leis:** o grep em `LEIS.md` encontra L-061 (`:95`), L-073 (`:100`), L-085, que nomeia/protege `turnos_do_colab` (`:117`), L-115 (`:146`) e L-113 (`:148`), além das auditorias `:166/:190`. A mudança preserva a pergunta por turno/instante e o dia de início, em vez de acrescentar predicado por data. (`app/ponto/turnos.py:2114-2122`, `:2153-2155`) L-002 proíbe derivação própria (`app/docs/LEIS.md:60`); o corpo segue único (`app/ponto/turnos.py:2161`, `:2170`). L-110 exige casos pela regra antes do código (`app/docs/LEIS.md:136`); L-114 distingue certificado, impacto e termômetro (`:147`). O fonte dos testes declara essa intenção, mas a cronologia real e os resultados executados são [não verificado].

## 01/11 16:15 · sessao 545be1432c25 · - · - · origem=Ronald (tmux)

Censo de produção com `celulas` explícito, incluindo aliases e argumento posicional:

| Chamada | Escritor/forma/janela | Efeito do diff |
|---|---|---|
| `dias_encerrados` no cartório | `{(c.pk, x.data): x}` da lista `cels`; janela entre primeira e última célula, podendo haver lacunas. `app/ponto/services/cartorio.py:919`, `app/ponto/services/cartorio.py:942` | Muda: sem `turnos`, agora o pareador recarrega células com pad. `app/ponto/turnos.py:2153` |
| `dias_encerrados` no calendário | `_cels_mes = celulas_do_periodo([pk], data_ini, data_fim)`, chave composta; fornece também os turnos da mesma janela. `app/colaboradores/services/calendario.py:395`, `app/colaboradores/services/calendario.py:699` | Não muda: `turnos is not None` evita chamada ao pareador nos dois códigos. `app/ponto/turnos.py:2154` |
| `turnos_do_colab` em `dia_encerrado_motivo` | Repasse posicional do parâmetro `celulas`; formato e alcance dependem do chamador, cujo contrato é `{data: CelulaDia}` com pad da janela `[data,data]`. `app/ponto/turnos.py:2090`, `app/ponto/turnos.py:2096`, `app/ponto/turnos.py:1852`, `app/ponto/turnos.py:1845` | Não muda: esta chamada não foi alterada. `app/ponto/turnos.py:2096` |
| `_tdc_pre` na montagem da grade | Converte `_cels_gr` composta para `{data: célula}`; `_cels_gr` vem de `janela_com_pad(_ini_al, ref)`. `app/escala/utils.py:1258`, `app/escala/utils.py:1260`, `app/escala/utils.py:1532` | Não muda: chamada direta ao pareador permanece. `app/escala/utils.py:1532` |
| `turnos_do_colab` após overlays da grade | `{data: célula}`; mesmo pad; fora da montagem usa célula do banco, dentro usa cópia com ata recém-montada, não salva. `app/escala/utils.py:1830`, `app/escala/utils.py:1833`, `app/escala/utils.py:1835`, `app/escala/utils.py:1846`, `app/escala/utils.py:1847` | Não muda: alimentação direta continua preservada. `app/escala/utils.py:1847` |
| Emissor de turno aberto retroativo | `{x.data: x}`, banco cobrindo `[ini−2, fim+2]`, portanto superset do pad `[ini−2, fim+1]`. `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:86`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88`, `app/ponto/turnos.py:1845` | Não muda: chamada direta permanece. `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88` |

Os seis são todos os sítios explícitos encontrados no censo AST da cópia, incluindo aliases de imports e passagem posicional; referências dinâmicas externas à cópia são `[não verificado]`.

`dias_encerrados` escreve seu resultado exclusivamente no corpo compartilhado: percorre dias, pergunta `_dia_encerrado_motivo_puro`, produz motivo quando `motivos=True`, senão `_m is None`. A alimentação composta é lida por `vinculo_do_dia` e pelo julgamento/fim previsto. `app/ponto/turnos.py:2169`, `app/ponto/turnos.py:2170`, `app/ponto/turnos.py:2172`; consulta composta em `app/escala/alimentacao.py:237`; DNA composto em `app/escala/models.py:1095`.

Leitores de produção do retorno da função de módulo:

- Cartório lê bool com default `False`; passa ao classificador e calcula `_em_curso`. `app/ponto/services/cartorio.py:943`, `app/ponto/services/cartorio.py:1180`, `app/ponto/services/cartorio.py:1253`.
- Calendário lê motivos para `_em_curso` e trata `sem_previsto`. `app/colaboradores/services/calendario.py:701`, `app/colaboradores/services/calendario.py:709`.
- `supra_juiz` recebe bool e passa ao classificador. `app/ponto/management/commands/supra_juiz.py:121`, `app/ponto/management/commands/supra_juiz.py:232`.
- `emitir_furo_retroativo` recebe bool e passa ao classificador. `app/ponto/management/commands/emitir_furo_retroativo.py:62`, `app/ponto/management/commands/emitir_furo_retroativo.py:108`.
- `par_completo_da_janela` recebe bool e exclui turnos de dias não encerrados. `app/ponto/turnos.py:2183`, `app/ponto/turnos.py:2186`.

Leitores do método homônimo de lote: status usa motivos; painel usa bool. Esse método chama diretamente o corpo compartilhado, portanto não é afetado pela alteração da casca. `app/ponto/status_dia.py:196`, `app/ponto/status_dia.py:197`, `app/core/services/painel_op.py:334`, `app/ponto/turnos.py:3018`.

Custo: o cartório passa a pagar uma consulta adicional de células por chamada efetiva de `dias_encerrados`, porque agora o pareador recebe `None` e executa `celulas_do_periodo`; essa função é um único SELECT. O cache `_enc` limita a uma carga por execução do corpo, não uma por dia. `app/ponto/turnos.py:2154`, `app/ponto/turnos.py:1882`, `app/escala/alimentacao.py:41`, `app/ponto/services/cartorio.py:939`. Células do pad podem ainda introduzir geradoras adicionais e disparar o SELECT condicional de `mais`; custo efetivo em produção `[não verificado]`. `app/ponto/turnos.py:1885`, `app/escala/alimentacao.py:123`. O lote mantém cargas próprias e corpo compartilhado, sem passar pela casca alterada. `app/ponto/turnos.py:2957`, `app/ponto/turnos.py:2963`, `app/ponto/turnos.py:3018`.

Ordem e memória no cartório:

1. Monta a grade, cujo pareamento final usa cópias com ata fresca em memória. `app/ponto/services/cartorio.py:963`, `app/escala/utils.py:1835`, `app/escala/utils.py:1847`.
2. Consulta encerramento lazy, normalmente em `_em_curso`; este novo pareamento lê células/atas do banco, anteriores à lavra corrente. `app/ponto/services/cartorio.py:1180`, `app/ponto/services/cartorio.py:942`, `app/ponto/turnos.py:1884`, `app/ponto/turnos.py:1937`.
3. Calcula/classifica; somente depois, no apply, lavra com `save=False` e salva via `carimbar`. `app/ponto/services/cartorio.py:1243`, `app/ponto/services/cartorio.py:1593`, `app/ponto/services/cartorio.py:1596`, `app/ponto/services/cartorio.py:1616`, `app/ponto/portas/celula.py:807`.

Portanto existe diferença estrutural entre a ata fresca usada pela grade e a ata persistida usada por esse pareador; resultado incorreto adicional no dado fornecido `[não verificado]`. A célula corrente ainda não foi lavrada quando encerramento é consultado, e a ata fresca da grade tampouco era repassada pelo cartório antes do diff: sua alimentação era a lista original `cels`. `app/ponto/services/cartorio.py:942`, `app/ponto/services/cartorio.py:963`, `app/ponto/services/cartorio.py:1596`.

No ramo `nunca_bateu`, a consulta de encerramento ocorre somente dentro de `apply_`, antes da lavra; dry-run desse ramo não passa por essa consulta. `app/ponto/services/cartorio.py:1040`, `app/ponto/services/cartorio.py:1063`, `app/ponto/services/cartorio.py:1075`.

## 01/11 16:15 · sessao b7e648826626 · - · - · origem=Ronald (tmux)

Análise estática; não executei testes nem mutantes: execução e resultado efetivo [não verificado]. [nao verificado]

| Teste novo | Mutante que detecta pela estrutura/asserts | Mutante que deixa passar | [nao verificado]
|---|---|---|
| `test_regra` | Remover `mais=self._geradoras` só do lote; ignorar geradora e usar vínculo ativo; erro compartilhado que fecha Y na saída de intervalo ou mantém X aberto, pois exige aberto X=False/Y=True e motivos literais nos três instantes. `app/ponto/tests/test_e4_carga_paridade.py:129-143`; `app/ponto/turnos.py:2963-2964`. | Retornar bool invertido quando `motivos=False`: só usa `motivos=True`. Também erro só em dia diferente de D ou ramo sem previsto. `app/ponto/tests/test_e4_carga_paridade.py:107`, `:115`, `:143`. |
| `test_paridade_carga_x_casca_em_fixture_mista` | Divergência de carregamento só do lote, como retirar `mais`, mudar a identidade da saída ou perder uma batida: compara todos os seis campos do turno, mapas de motivos e dia por pk nas duas janelas. `app/ponto/tests/test_e4_carga_paridade.py:51-56`, `:106-124`, `:145-148`. | Erro comum aos corpos compartilhados que mantém ambos iguais. Casca e lote chamam `_turnos_do_colab_puro` e `_dias_encerrados_puro`. `app/ponto/turnos.py:1913`, `:2153-2155`, `:3009-3019`. |
| `test_MORDE_carga_sem_mais` | É controle positivo: injeta retirada de `mais`, exige que o patch tenha sido chamado e que Y difira em turnos. `app/ponto/tests/test_e4_carga_paridade.py:155-165`. | **Sozinho, não mata uma retirada permanente de `mais` no código:** com lote já defeituoso e casca correta, a divergência exigida continua existindo. Quem mata essa retirada permanente são `test_regra` e `test_paridade`. `app/ponto/tests/test_e4_carga_paridade.py:139-148`, `:158-165`. |
| `test_RED_alimentado_igual_a_juiza_e_ao_lote` | Restaurar a linha antiga que repassa o dict composto ao pareador: constrói exatamente esse dict, compara todos os dias contra lote e D contra singular nos três instantes e duas janelas. `app/ponto/tests/test_bug_enc_celulas_forma.py:32-44`; a composição adicional da chave ocorre em `app/ponto/turnos.py:1954-1958`, enquanto o juiz busca `(colaborador_id,data)` em `app/escala/alimentacao.py:237`. | Erro compartilhado no julgamento puro, igualdade preservada; bool errado no modo sem motivos; erro apenas no ramo de `turnos=` fornecido. Todas as chamadas do teste usam `motivos=True` e nenhuma fornece `turnos`. `app/ponto/tests/test_bug_enc_celulas_forma.py:33`, `:43-44`; `app/ponto/turnos.py:2154`, `:2170-2172`. |
| `test_RED_regra_do_veredito_do_cartorio` | Restaurar repasse antigo; usar vínculo ativo no pareador; ignorar a pausa de Y: fixa X encerrado e Y `'turno_aberto_relogio'` às 08h. `app/ponto/tests/test_bug_enc_celulas_forma.py:46-50`. | Erro só às 03h/12h, só em Z, nos outros dias ou na conversão bool. O teste consulta apenas X/Y, D e `INSTANTES[1]`, através da helper com motivos. `app/ponto/tests/test_bug_enc_celulas_forma.py:33`, `:49-50`. |

O patch **alcança a carga do lote**. `_lote()` instancia `CargaDaJanela` dentro do `with`; `CargaDaJanela.__init__` faz import local de `escalas_do_periodo_por_colab` e chama a referência importada com `mais=self._geradoras`. Portanto busca o atributo já patchado de `escala.alimentacao`. `app/ponto/tests/test_e4_carga_paridade.py:111-113`, `:159-161`; `app/ponto/turnos.py:2950-2951`, `:2963-2964`.

A casca medida antes do patch fica materializada e intacta: assinatura contém datas, pks, tuplas e bools; motivos são calculados imediatamente; mapa de dias contém pk→date. Não há queryset pendente nesses resultados. `app/ponto/tests/test_e4_carga_paridade.py:51-56`, `:103-109`, `:158`; `app/ponto/turnos.py:2172-2174`, `:2925-2933`. Medir a casca *dentro* do patch também a mutaria, pois `escalas_do_periodo` resolve a função plural pelo global do módulo. `app/escala/alimentacao.py:153-154`.

Os testes novos não cobrem célula fresca ainda não salva, execução real de `cartorio`, orçamento de consultas ou ramo sem motivos: a fixture salva células, atualiza a geradora por queryset e a helper relê do banco; nenhum dos asserts mede consultas. `app/ponto/tests/test_e4_carga_paridade.py:92-96`, `:127-165`; `app/ponto/tests/test_bug_enc_celulas_forma.py:32-50`. Também não há célula no pad fora de D: todas as geradoras/células da fixture são criadas para D; assim remover só alimentação das células do pad pode passar nesta fixture, embora as batidas de D+1 existam. `app/ponto/tests/test_e4_carga_paridade.py:77-79`, `:92-94`.

Não há leitura direta do relógio real nos testes novos: D e INSTANTES são construídos de constantes; ambas as classes são congeladas; chamadas de encerramento recebem `agora` explícito. `app/ponto/tests/test_e4_carga_paridade.py:39-48`, `:100`, `:107`, `:115`; `app/ponto/tests/test_bug_enc_celulas_forma.py:24`, `:33`, `:44`. Os defaults `timezone.now()` dessas portas só rodam se `agora is None`, condição não usada nesses testes. `app/ponto/turnos.py:2094-2095`, `:2140-2141`.

## 01/11 16:14 · sessao 79df0d6c3572 · - · - · origem=Ronald (tmux)

| Achado | Classe proposta | Pergunta e resposta/triagem | Evidência da cópia |
|---|---|---|---|
| 6 | **(b)** | `codex_v1.q`, item 7: “Algum chamador [...] (codigo, template, command, string em getattr) sobrou?”; `codex_v1.r`, §7 identifica exatamente as três referências. `triagem_v1.md`, item 7: “INCOMPLETO declarado. E instrumento (`bin/`) e doc, com pouso proprio (RAIAS 5).” | `bin/esmeril_chamado_censo.py:135`; `bin/esmeril_chamado_censo_de_hoje.txt:38`; `app/docs/DOSSIES.md:46`. |
| 7 | **(a)** | `codex_v1.q`, item 6: “selo/RED sem mordida”; `codex_v3.q`, item 4 repete para o “diff total”. Resposta ou triagem sobre a perda da testemunha viva da classe (f): **nenhum**. É inspeção estrutural de teste, acessível na cópia, não depende de execução histórica de RED. | `app/core/tests/test_censo_escritas.py:202` explica a troca; `:210`–`:213` exige chamada AST; `:214`–`:217` procura genericamente `update()` censado sem `[relacionado]`. |
| 8 | **(e)**, pela leitura estrita da pergunta | Nenhuma pergunta pede custo ou repetição do C6. A mais próxima, `codex_v1.q`, item 3, menciona “memo `_pv_memo`”, mas pergunta se a releitura resulta **VIVA**, não se há duas avaliações. `codex_v1.r`, §2 descreve “antes salvava depois do pré-C6”, e §8 diz “Já exigia C6 morto antes”; não aponta o custo nem há triagem desse custo. | Primeira consulta: `app/chamados/reconciliador.py:398`; chamada da porta: `:405`; segunda consulta: `app/chamados/models.py:2565`; instante/via mudam a chave: `app/chamados/juizes.py:790`–`:791`. |
| 9 | **(e)** | Nenhuma `.q` pede auditoria de vocabulário do commit ou palavra proibida em comentário. Menções a `fallback` em `codex_v1.r`, §6, e `codex_v2.r`, §5, tratam o **relógio**, não esta ocorrência lexical. Resposta/triagem sobre este achado: **nenhum**. | Comentário: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:404` e `app/chamados/tests/test_porta_desfecho_pergunta.py:204`. Corpo do commit não disponível nesta cópia sem `.git`: **[nao verificado]**, além do relato fornecido pelo opus. |

As referências aos anexos acima usam seções/itens porque eles só foram entregues inline; o agente principal precisa aplicar sua convenção de numeração às linhas dos anexos. **[nao verificado]**

## 01/11 16:14 · sessao b71e20460e38 · - · - · origem=Ronald (tmux)

Proposta para 1–5: **a, b, b, b, b**; o #5 exige ressalva sobre ausência de motivo específico da triagem. (`CLAUDE.md:565`.)

1. **(a), tema presente, bypass não apontado.**  
   Pergunta: `codex_v1.q`, nº 6: “selo/RED sem mordida (o selo AST de test_o261e_f2_porta_carimbo.py morde mutante real?)”; nº 2 também pede “Para cada chamador de `carimbar` / `carimbar_identidade`...”. Respostas: **nenhum** trecho aponta o bypass mediante chamada de `_escrever_carimbo` ou a ausência de restrição dos chamadores de identidade; os achados sobre o selo tratam formas de escrita que escapam. O selo permite `_escrever_carimbo` e verifica atribuições; o método escreve sem C6, enquanto o C6 está em `carimbar`. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:44`; `app/chamados/models.py:2527`; `app/chamados/models.py:2565`.)  
   Atenção: a cura literal proposta pelo opus restringindo `_escrever_carimbo` apenas a dois chamadores excluiria também as chamadas legítimas atuais de reversão e descarimbo. Não é necessário julgar a cura para classificar o achado. (`app/chamados/models.py:2582`; `app/chamados/models.py:2611`.)

2. **(b), visto e reduzido a nota.**  
   Pergunta: `codex_v1.q`, nº 2: comportamento observável “quando a porta RECUSA”; nº 8 inclui “ausencia/declaracao”. Resposta: `codex_v1.r`, §2, linha da tabela `_materializar_ausencia_de_resposta`: **“Ignora o booleano. A ausência já criada/reaberta continua”**; `codex_v3.r`, §2: **“recusas individuais podem reduzir o conjunto carimbado”**. Triagem: `triagem_v3.md`, item 2c: **“Nota. O Codex mesmo diz que o conjunto segue restrito ao efetivamente carimbado.”** Esse é o motivo dado para não promover o tema a correção. O retorno é ignorado e a consulta das absorvidas exige carimbo não nulo. (`app/chamados/services/materializacao.py:523`; `app/chamados/services/materializacao.py:571`.) A expressão específica “sem trilha/log” não é formulada nas respostas fornecidas: **[nao verificado]**.

3. **(b), os dois componentes foram vistos.**  
   Pergunta: `codex_v1.q`, nº 3: gravação seguida de releitura VIVA; nº 4: “instancia em memoria diferente do banco”; nº 6: ordem de trava. Resposta: `codex_v1.r`, §3: **“sem reler ou travar a pergunta”**; §4: **“exceção da lavra ou do `save`, depois da aprovação, fica fora do `try` de reversão.”** Triagem: `triagem_v1.md`, item 3a, e `triagem_v2.md`, 5b: janela anterior, travar mudaria ordem de sete chamadores; `triagem_v3.md`, 3b: **“Nao e regressao; vai a fila”**, reconhecendo que o banco volta e a memória não. A guarda encerra antes de lavra/save. (`app/chamados/models.py:2564`; `app/chamados/models.py:2572`; `app/chamados/models.py:2576`.)

4. **(b), relógio visto e remetido a complemento.**  
   Pergunta: `codex_v1.q`, nº 6: **“relogio lido fora da borda”**; repetida nas voltas seguintes. Resposta: `codex_v1.r`, §6: **“Os novos métodos têm `now()` de fallback”**; `codex_v3.r`, §4: **“Presente, embora encaminhado”**. Triagem: `triagem_v2.md`, 5a: **“a leitura do relogio MUDOU DE SITIO, nao nasceu”**, remetendo ao complemento RELOGIO-INJETADO; `triagem_v3.md`, 3c, confirma o encaminhamento. Os fallbacks existem; `encerrar_fantasma` chama a porta sem `quando`. (`app/chamados/models.py:2550`; `app/chamados/models.py:2602`; `app/chamados/models.py:2648`.) O subdetalhe “ignora o agora do chamador” não foi explicitamente nomeado nas respostas: **[nao verificado]**.

5. **(b), com ressalva: descarte agregado, motivo específico ausente.**  
   Pergunta: `codex_v1.q`, nº 6: **“derivacao paralela de fonte unica”**. Resposta: `codex_v1.r`, §6, derivação paralela: **“Permanece a separação já existente entre identidade no C6, no queryset e na lista da relavra.”** O próprio trecho cita os dois sítios exatos do opus. Triagem: `triagem_v2.md`, item 6: **“2o juiz, derivacao paralela e 2o escritor explicito: nenhum.”** Isso registra o encerramento agregado do tema, mas **motivo específico da triagem para manter as listas 5×4: [nao verificado]**. Não atribuir à triagem o argumento de preexistência que aparece na resposta Codex. A divergência está confirmada: o C6 inclui `VIA_COMPETENCIA_TRANCADA`; a lista da relavra tem quatro vias. (`app/chamados/juizes.py:1066`; `app/chamados/services/relavra_via.py:166`.)

## 01/11 15:38 · sessao 8e933ad4f1a9 · O261-E-F2 · verificador · origem=Code

**Não aprovo o selo como prova de escritor único:** a árvore passa, mas formas válidas de escrita ainda escapam, e o resolvedor produz `RecursionError` em programas finitos. A falha está na resolução de chaves e no reconhecimento das chamadas. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:96, app/chamados/tests/test_o261e_f2_porta_carimbo.py:107, app/chamados/tests/test_o261e_f2_porta_carimbo.py:142)

1. **Detector executado em memória**

Executei os 12 testes da classe estrutural: zero falhas e zero erros. Na varredura real, obtive seis ocorrências para o carimbo e quatro para a via, com `fora=[]` e nenhuma porta declarada ausente; duas das seis ocorrências do carimbo pertencem à regularização externa. Isso confirma a passagem dos testes atuais, mas os contraexemplos abaixo invalidam sua suficiência. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:188, app/chamados/tests/test_o261e_f2_porta_carimbo.py:44, app/chamados/services/regularizacao_ext.py:198)

Nos mutantes, `F` abaixo representa tanto `materializada_em` quanto `via_resolucao`; ambos produziram os mesmos resultados. As escritas por chave local e `dict` posicional também foram executadas contra um receptor em memória, confirmando que entregam o campo ao método enquanto o detector retorna `[]`. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:107, app/chamados/tests/test_o261e_f2_porta_carimbo.py:149)

| Forma que escapou | Resultado | Causa no detector |
|---|---|---|
| `campo='F'; qs.update(**{campo: None})` | `[]` | A chave do literal precisa ser constante; `_resolve` não é aplicado à chave. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:114) |
| `qs.update(**dict([('F', None)]))` | `[]` | A análise de `dict` só examina argumentos nomeados, ignorando o argumento posicional. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:116) |
| `PD=PerguntaDisputa; p=PD(F=None); p.save()` | `[]` | O nome do construtor não é resolvido por alias. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:144) |
| `for campo in ['F']: setattr(p, campo, None)` | `[]` | `_locais` não coleta os valores atribuídos pelo `for`. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:70) |
| `p.__setattr__('F', None); p.save()` | `[]` | Só a chamada denominada `setattr` recebe esse tratamento. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:145) |
| `d={}; d \|= {'F': None}; qs.update(**d)` | `[]` | `_locais` não incorpora a união por `AugAssign`. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:70) |
| Dicionário definido na função externa e usado por uma função interna | `[]` | A entrada na função interna substitui o ambiente local, perdendo a variável capturada. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:127) |

Essas lacunas excedem as exclusões declaradas de `obj.__dict__` e `SET_NULL`; não encontrei com isso um escritor clandestino **existente** na produção da cópia. A existência dessas formas no acervo de produção é [não verificado]. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:85, app/chamados/tests/test_o261e_f2_porta_carimbo.py:169)

Este programa executa normalmente, mas o detector levantou `RecursionError`, tanto com `{**d}` quanto com `dict(**d)` na terceira linha. `_resolve` protege ciclos entre nomes; `_chaves_de` reinicia essa proteção ao seguir o espalhamento. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:99, app/chamados/tests/test_o261e_f2_porta_carimbo.py:113, app/chamados/tests/test_o261e_f2_porta_carimbo.py:120)

```python
def mutante(qs):
    d = {'materializada_em': None}
    d = {**d}
    qs.update(**d)
```

Também medi expansão exponencial: cadeias com duas atribuições iguais por nível produziram 256, 4.096 e 65.536 achados para 8, 12 e 16 níveis. O resolvedor percorre todas as definições, sem deduplicação ou memorização; portanto pode consumir tempo e memória desproporcionais mesmo sem ciclo. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:73, app/chamados/tests/test_o261e_f2_porta_carimbo.py:101)

2. **Escritores e leitores dos sete campos**

Na tabela, “escritor” inclui atribuição em memória e persistência delegada; os chamadores que escolhem a operação continuam nos serviços, mas a atribuição do carimbo foi centralizada no modelo. (app/chamados/models.py:2527, app/chamados/models.py:2576, app/chamados/models.py:2626)

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `materializada_em` | `_escrever_carimbo`; limpeza em lote. (app/chamados/models.py:2529, app/chamados/models.py:2627) | C6/lastro; estado, fechamento e ressuscitação; materialização, emissão, reconciliação e validação; mesa, cache e procedência. (app/chamados/juizes.py:624, app/chamados/juizes.py:1077, app/chamados/models.py:1421, app/chamados/models.py:1768, app/chamados/models.py:2477, app/chamados/services/materializacao.py:510, app/chamados/services/disputa_emissao.py:1539, app/chamados/reconciliador.py:583, app/chamados/services/validacao.py:182, app/chamados/services/raiox.py:133, app/chamados/veredito_lavrado.py:163, app/ponto/services/procedencia_batida.py:74) |
| `materializada_por` | `_escrever_carimbo`, pelo `_id`; limpeza em lote; exclusão do usuário por `SET_NULL`. (app/chamados/models.py:2530, app/chamados/models.py:2627, app/chamados/models.py:2762) | Preservação/reversão da porta; fotos de auditoria; passivo; procedência. (app/chamados/models.py:2552, app/chamados/models.py:2600, app/chamados/services/materializacao.py:618, app/chamados/reconciliador.py:1091, app/chamados/services/passivo_validado.py:83, app/ponto/services/procedencia_batida.py:72) |
| `via_resolucao` | `_escrever_carimbo`; limpeza em lote; `encerrar_por_fato_no_espelho`; `relavrar`. (app/chamados/models.py:2531, app/chamados/models.py:2627, app/chamados/models.py:2636, app/chamados/services/relavra_via.py:123) | C6/lastro, reaberturas e idempotência; emissão, absorção, deduplicação e relavra; explicação, tranca, fábrica, cartório, flip e passivo. (app/chamados/juizes.py:635, app/chamados/juizes.py:1071, app/chamados/models.py:2674, app/chamados/models.py:2681, app/chamados/services/disputa_emissao.py:1628, app/chamados/services/materializacao.py:572, app/chamados/services/fio.py:882, app/chamados/services/relavra_via.py:116, app/chamados/services/explicador.py:95, app/chamados/services/competencia_trancada.py:244, app/chamados/services/fabrica_da_celula.py:104, app/ponto/services/cartorio.py:1429, app/ponto/services/flip_auto.py:129, app/chamados/services/passivo_validado.py:141) |
| `respondida_em` | Resposta normal, automática e recuperação pela trilha; reabertura; porta do carimbo; limpeza em lote. Há também restauração e cópia em memória. (app/chamados/services/disputa_emissao.py:1248, app/chamados/services/disputa_emissao.py:1394, app/chamados/services/disputa_emissao.py:1410, app/chamados/services/disputa_emissao.py:1358, app/chamados/models.py:2558, app/chamados/models.py:1488, app/chamados/models.py:2583, app/chamados/models.py:1441) | C6, fila e corrigíveis; estado e bloqueio de fechamento; validação, materialização, emissão, fio e cache. Também API, cobrança, vigias, telas e relatórios. (app/chamados/juizes.py:1047, app/chamados/juizes.py:1277, app/chamados/juizes.py:1284, app/chamados/models.py:1769, app/chamados/models.py:2479, app/chamados/services/validacao.py:363, app/chamados/services/materializacao.py:1259, app/chamados/services/disputa_emissao.py:1559, app/chamados/services/fio.py:336, app/chamados/veredito_lavrado.py:167, app/api/views.py:1009, app/chamados/services/cobranca.py:229, app/chamados/services/vigia_fio.py:630, app/chamados/views.py:1280, app/chamados/relatorios_disputa.py:20) |
| `validada_em` | Validação/devolução; invalidação por resposta nova; reabertura; autovalidação da porta; limpeza em lote; cópia/restauração em memória. (app/chamados/services/validacao.py:131, app/chamados/services/validacao.py:188, app/chamados/services/disputa_emissao.py:1260, app/chamados/services/disputa_emissao.py:1358, app/chamados/models.py:2561, app/chamados/models.py:1488, app/chamados/models.py:1440, app/chamados/models.py:2584) | Primeiro degrau do C6; estado e fechamento; validação, materialização, emissão e reconciliação; declaração, verbos, painel, vigia, flip e procedência. (app/chamados/juizes.py:1033, app/chamados/models.py:2475, app/chamados/models.py:1768, app/chamados/services/validacao.py:366, app/chamados/services/materializacao.py:1260, app/chamados/services/disputa_emissao.py:1620, app/chamados/reconciliador.py:584, app/chamados/services/declaracao_texto.py:208, app/chamados/services/verbos.py:246, app/chamados/services/painel.py:268, app/chamados/services/vigia_carimbo.py:98, app/ponto/services/flip_auto.py:130, app/ponto/services/procedencia_batida.py:74) |
| `validada_por` | Mesmos atos de validação, invalidação, reabertura e autovalidação; restauração em memória; `SET_NULL`. (app/chamados/services/validacao.py:132, app/chamados/services/validacao.py:189, app/chamados/services/disputa_emissao.py:1261, app/chamados/services/disputa_emissao.py:1358, app/chamados/models.py:2562, app/chamados/models.py:1488, app/chamados/models.py:2585, app/chamados/models.py:2742) | Fotos de validação, resposta e reabertura; porta; auditoria da absorção; verbos, painel, passivo e procedência. (app/chamados/services/validacao.py:139, app/chamados/services/disputa_emissao.py:1250, app/chamados/services/disputa_emissao.py:1373, app/chamados/models.py:2553, app/chamados/services/materializacao.py:616, app/chamados/services/verbos.py:240, app/chamados/services/painel.py:260, app/chamados/services/passivo_validado.py:83, app/ponto/services/procedencia_batida.py:72) |
| `veredito_lavrado` | `lavrar`, persistência individual/lote e atribuição do backfill. A nova porta solicita a lavra e inclui o campo no mesmo `save`. (app/chamados/veredito_lavrado.py:227, app/chamados/veredito_lavrado.py:231, app/chamados/veredito_lavrado.py:207, app/chamados/management/commands/backfill_veredito_lavrado.py:138, app/chamados/models.py:2572) | Validade do cache, mesa, comparação da relavra, passivo, backfill e auditoria. (app/chamados/veredito_lavrado.py:179, app/chamados/services/raiox.py:134, app/chamados/services/raiox.py:182, app/chamados/services/relavra_via.py:122, app/chamados/services/passivo_validado.py:83, app/chamados/management/commands/backfill_veredito_lavrado.py:120, app/chamados/management/commands/auditar_invariantes_chamados.py:1169) |

Outros consumidores diretos de `respondida_em` incluem declaração textual, becos, furo sem canal, aviso, verbos, ações, cobrança manual, mensageria, suprajuiz e templates. (app/chamados/services/declaracao_texto.py:363, app/chamados/services/becos.py:30, app/chamados/services/furo_sem_canal.py:81, app/chamados/services/aviso_pendencia.py:13, app/chamados/services/verbos.py:311, app/chamados/acoes_disputa.py:95, app/chamados/views_cobrar.py:169, app/api/views_mensageria.py:569, app/ponto/supra_juiz.py:515, app/templates/chamados/partials/_pergunta_card.html:24)

**Sim, leitores intactos passam a receber valores diferentes:**

- **Fecho sem credor:** a recusa preserva texto sem `respondida_em`; a fila exclui a pergunta, a validação devolve `sem_resposta`, `.estado` permanece pendente e o texto bloqueia fechar a disputa. O teste foi invertido para aceitar essa preservação; os leitores não foram migrados. Incidência em produção: [não verificado]. (app/chamados/services/disputa_emissao.py:1638, app/chamados/juizes.py:1284, app/chamados/services/validacao.py:363, app/chamados/models.py:2479, app/chamados/models.py:1768, app/chamados/tests/test_o261_resposta_vazia.py:107)
- **Autor após ressuscitação:** agora `materializada_por` também fica nulo. `procedencia` passa a devolver outro `quem` quando dependia desse autor; não há migração desse leitor. A frase já podia indicar ausência de validação porque a data já era apagada. Impacto visível adicional em produção: [não verificado]. (app/chamados/models.py:1488, app/ponto/services/procedencia_batida.py:72, app/ponto/services/procedencia_batida.py:90)
- **Absorção por ausência:** recusas individuais podem reduzir o conjunto carimbado; `perguntas_absorvidas_por` continua selecionando pelo grupo de instante e via. Isso altera o conjunto devolvido, mas não demonstra erro: ele continua restrito às perguntas efetivamente carimbadas. (app/chamados/services/materializacao.py:523, app/chamados/services/materializacao.py:569, app/chamados/services/materializacao.py:573)

A lavra granular passou para depois da autovalidação, mas não demonstrei diferença de `veredito_lavrado` causada por essa ordem: o juiz da lavra não consulta `respondida_em`, `validada_em` ou `validada_por`. (app/chamados/models.py:2556, app/chamados/models.py:2574, app/chamados/veredito_lavrado.py:219, app/chamados/juizes.py:610)

3. **Justificativas das triagens**

Há duas justificativas que precisam de correção e uma conclusão de cura que os mutantes contradizem:

- **V2, item 3 — “vias derivadas só existem depois do carimbo”: não é garantia da porta.** `relavrar` aceita a via sem verificar `materializada_em`; o modelo admite data nula e não declara restrição conjunta. O padrão dos escritores atuais não prova a premissa para qualquer entrada/acervo. Acervo contrário em produção: [não verificado]. (app/chamados/services/relavra_via.py:116, app/chamados/services/relavra_via.py:123, app/chamados/models.py:2298, app/chamados/models.py:2756, app/chamados/models.py:2776)
- **V1, nota 4 — “o atomic do chamador cobre”: excessivo para reversão em memória.** A restauração só cobre exceção/recusa do C6; lavra e `save` ficam depois dessa guarda. O rollback não executa `_desfazer_em_memoria`, e o refresh posterior da recusa só recarrega três campos. Não demonstrei uma regressão nova desse problema. (app/chamados/models.py:2564, app/chamados/models.py:2572, app/chamados/models.py:2579, app/chamados/services/materializacao.py:1024)
- **V2, itens 1/6 — selo “curado”: conclusão insuficiente.** As quatro formas acrescentadas estão cobertas, mas permanecem os escapes e a recursão demonstrados no item 1 desta resposta. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:246, app/chamados/tests/test_o261e_f2_porta_carimbo.py:107)

Os demais encaminhamentos principais têm apoio, com estes limites:

- **Limbo:** “não inventa hora” está explícito; completar o instante exige testemunha da trilha. Isso sustenta encaminhar a decisão, mas não elimina a mudança funcional dos leitores descrita acima. (app/chamados/services/disputa_emissao.py:1400, app/chamados/services/disputa_emissao.py:1403, app/chamados/services/validacao.py:363)
- **Relógio para complemento:** o corte autoriza complemento da obra dona; as leituras internas continuam presentes. Encaminhado não significa resolvido. (app/docs/CORTES.md:369, app/docs/CORTES.md:380, app/chamados/models.py:2550, app/chamados/models.py:2602)
- **Concorrência para fila:** a porta continua julgando e salvando a instância recebida, sem trava própria. Existem sete funções chamadoras da porta não identidade; afirmar que qualquer solução necessariamente mudaria a ordem de trava de todas elas é [não verificado]. (app/chamados/models.py:2552, app/chamados/models.py:2576, app/chamados/models.py:2648, app/chamados/reconciliador.py:405, app/chamados/reconciliador.py:618, app/chamados/services/disputa_emissao.py:1566, app/chamados/services/materializacao.py:523, app/chamados/services/materializacao.py:1166, app/chamados/services/materializacao.py:1313)
- **Referências obsoletas:** o instrumento ainda declara a função removida; portanto o item permanece incompleto, como reconhecido na triagem. (bin/esmeril_chamado_censo.py:135, app/chamados/models.py:2501)

4. **Classes de reprovação no diff total**

| Classe | Resultado |
|---|---|
| Segundo juiz | Não identificado nos caminhos alterados: a porta chama C6 e a lavra chama o juiz de lastro existente. (app/chamados/models.py:2565, app/chamados/veredito_lavrado.py:219) |
| Segundo escritor fora da porta | Nenhum escritor explícito encontrado pela varredura atual; a exclusividade não está provada porque os mutantes escapam. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:169, app/chamados/tests/test_o261e_f2_porta_carimbo.py:149) |
| Relógio fora da borda | **Presente**, embora encaminhado: fallback nas duas portas e relógio próprio da lavra. (app/chamados/models.py:2550, app/chamados/models.py:2602, app/chamados/veredito_lavrado.py:228) |
| Derivação paralela nova | Não identificada: a lista de identidade vem da autoridade existente, e o julgamento continua delegado. (app/chamados/models.py:2524, app/chamados/models.py:2565) |
| Selo/RED sem mordida suficiente | **Presente:** os testes passam apesar dos escapes e do resolvedor vulnerável à recursão. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:188, app/chamados/tests/test_o261e_f2_porta_carimbo.py:107) |
| Ordem de trava | A porta não acrescenta trava; inversão nova ou deadlock concreto: [não verificado]. (app/chamados/models.py:2533) |
| Pouso antes do pré-requisito | Não demonstrado no plantio granular: a recusa levanta `_Reprovada` dentro do `atomic`. Pouso operacional: [não verificado]. (app/chamados/services/materializacao.py:1109, app/chamados/services/materializacao.py:1166, app/chamados/services/materializacao.py:1172) |

Há ainda uma limitação importante da aprovação: com texto e `resposta='validar'`, a porta preenche `validada_em` **antes** do C6; seu primeiro degrau então devolve morta. Nesse caminho, a nova consulta não demonstra lastro independente nem pode reprovar pela lei do dia. Isso segue a aplicação do estado completo anunciada pela porta, portanto não o classifico isoladamente como segundo juiz ou pouso indevido. (app/chamados/models.py:2541, app/chamados/models.py:2560, app/chamados/models.py:2565, app/chamados/juizes.py:1033)

_1439 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
