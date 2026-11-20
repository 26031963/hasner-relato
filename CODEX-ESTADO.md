# CODEX-ESTADO

> Ultima resposta processada: **20/11/2026 20:07** · publicado em **20/11/2026 20:07** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 20/11 20:07 · sessao dfc7ddd27c8d · E4C-F5-CONSTROI · verificador · origem=Code

Os dois selos passaram sobre a cópia sem mutação. Os quatro mutantes novos falharam nas asserções esperadas. Encontrei falsos verdes reproduzíveis nos dois selos. Executei os visitantes reais em memória, com os metadados extraídos do código do modelo e `unittest` no lugar de `SimpleTestCase`. A suíte Django, o banco de teste e o hash do commit [nao verificado]. A cópia veio sem `.git`. `test_e4c_f5_selo_par_ato_coluna.py:479–531`, `test_e4c_f5_selo_catraca.py:299–321`, `e4c_f5_voltaE_mutantes.py:58–69`. [nao verificado]

## A. Par ato–coluna

**1. Quais sítios o censo enumera?** Cinco entradas, todas com `pareado=True`. A primeira é uma chamada ao mesmo escritor da segunda. `test_e4c_f5_selo_par_ato_coluna.py:479–496`.

| Sítio enumerado | Função e ato | Gravação de `validada_em` |
|---|---|---|
| `app/chamados/models.py:1517` | `ChamadoColaborador.reabrir` · REABERTURA | Delega ao lote em `models.py:1721`; UPDATE em `models.py:3537–3539`. |
| `app/chamados/models.py:1719` | `ChamadoColaborador.ressuscitar_perguntas` · REABERTURA | Monta `_compensa` em `models.py:1713–1718`; passa `validada_em=None` em `models.py:1721`; UPDATE em `models.py:3537–3539`. |
| `app/chamados/services/disputa_emissao.py:1664` | `reabrir_para_correcao` · REABERTURA e RECUSA | Guarda em `disputa_emissao.py:1640–1641`, conjunto em `models.py:4217`, coluna em `disputa_emissao.py:1630`, lote em `disputa_emissao.py:1683` e UPDATE em `models.py:3537–3539`. |
| `app/chamados/services/validacao.py:293` | `devolver_validacao_recusada` · DEVOLUÇÃO | Atribuição em `validacao.py:287` e `save` em `validacao.py:290`. |
| `app/chamados/services/validacao.py:370` | `devolver_paridade_bug_par_103` · DEVOLUÇÃO | Atribuição em `validacao.py:365` e `save` em `validacao.py:367`. |

Os dois `save` repassam `update_fields` ao ORM em `app/chamados/models.py:3719–3735`.

**2. Há escritor fora do censo?** Na busca por `registrar`, nomes e literais dos atos, aliases de import, montagem de listas, construtores, operações ORM, relação reversa e nome SQL da tabela, não achei outro escritor operacional desses atos. O `bulk_create` genérico recebe as linhas dos chamadores em `models.py:4428–4438`. `linhas_da_projecao_sem_linha` só produz RESPOSTA e VALIDAÇÃO, em `models.py:4313–4323`. A migração histórica `0051_decisao_pergunta.py:41,49–62` atribui REABERTURA às linhas antigas via default. O censo exclui migrações em `test_e4c_f5_selo_par_ato_coluna.py:499–507`.

Há chamadas de produção sem entrada própria no censo. A RECUSA explícita em `validacao.py:943–944` e a chamada por alias em `models.py:2405,2423` chegam ao escritor de `disputa_emissao.py:1664`. Também ficam fora as entradas de `views.py:1432`, `acoes_disputa.py:106`, `materializacao.py:1213`, `curar_perguntas_em_limbo.py:70`, `views_arquivar.py:86` e `ponto/services/cartorio.py:1721`, além da cadeia de dois saltos `models.py:1085 → :1517 → :1719`. `validacao.py:798` delega dentro do mesmo arquivo, mas o `registrar` da função chamada está sob o `if` de `validacao.py:284`. A projeção o descarta por esse ramo. `test_e4c_f5_selo_par_ato_coluna.py:297–324`.

**3. O que conta como par?**

- `save(update_fields=…)` com o campo e o mesmo receptor ou seu `.pk`, `.update(…)` com o campo e `filter(pk=…)` sintaticamente correspondente, ou lote cujo vínculo com o PK o visitante consegue reconstruir. O teste novo verifica a presença da coluna, sem verificar se o valor a limpa. `test_e4c_f5_selo_par_ato_coluna.py:47–48,128–172,234–278,489–494`. [nao verificado]
- Exige conjuntos idênticos e não vazios de nós `with transaction.atomic()` e os mesmos lados dos mesmos `if` e os mesmos `except`. Um savepoint adicional, um `atomic` só no chamador ou no decorador e condições equivalentes em `if` separados podem dar falso vermelho. Laços e outros desvios de fluxo ficam fora da comparação de ramos. `test_e4c_f5_selo_par_ato_coluna.py:86–94,486–494`.
- Projeta um salto no mesmo arquivo. Resolve `self.aux()` e `cls.aux()` na classe léxica e procura um `registrar` direto, com argumento posicional e fora de `if`/`except`. O par tem de estar no corpo que contém esse `registrar`. Import com `as` é resolvido. `self.registrar`, `cls.registrar` e alias por atribuição escapam como chamadas diretas. `test_e4c_f5_selo_par_ato_coluna.py:291–324,479–494`, `test_e4c_f5_selo_chamada.py:38–57`.
- `save()` integral, `bulk_update`, receptor com alias e queryset filtrado antes e guardado em variável podem dar falso vermelho. No outro sentido, um `filter(pk=…)` coincidente pode bastar mesmo em outro modelo ou com filtros adicionais que eliminem a linha. Reproduzi o falso verde nos dois lotes reais em memória. Acrescentar `escrever_so=[]` em `disputa_emissao.py:1683` ou usar `alvo_lido.none()` em `models.py:1721` manteve verdes os dois censos do par, embora o UPDATE fique sem linhas. `test_e4c_f5_selo_par_ato_coluna.py:148–172,234–278,489–494`, `models.py:3536–3539`. [nao verificado]

**4. Âncoras e mordida dos mutantes.** Cada âncora ocorreu exatamente uma vez, inclusive nas trocas sucessivas do primeiro mutante. `e4c_f5_voltaE_mutantes.py:26–54,63–68`.

| Mutante | Âncora na cópia e efeito | Asserção que falha |
|---|---|---|
| `d_paridade_devolucao_sem_coluna` | `validacao.py:365–367` remove a atribuição e o campo do `save`. As outras três trocas estão em `validacao.py:332,359,379` e alteram a descrição e as fotos. | `assertTrue(pareado)`, com DEVOLUÇÃO em `validacao.py:369` na fonte mutada. |
| `d_ressuscitacao_reabertura_sem_coluna` | `models.py:1721` tira `validada_em=None` do lote. | `assertTrue(pareado)` nas duas entradas, `models.py:1517,1719`. |
| `d_recusa_sem_coluna_no_lote` | `disputa_emissao.py:1630` tira `validada_em` de `campos`, usado em `:1683`. Afeta RECUSA e REABERTURA. | `assertTrue(pareado)` em `disputa_emissao.py:1664`. |
| `c_recusa_update_fields_cru` | `validacao.py:940` acrescenta `respondida_em` ao `save`, sem atribuição. | `assertEqual(vermelhos, [])`, com `update_fields(respondida_em)` em `validacao.py:940`. |

Nos três mutantes do par também falha a asserção antiga de sítio ausente do censo de escritores. A mordida exigida pelo item 78 vem da asserção nova de pareamento. `test_e4c_f5_selo_par_ato_coluna.py:520,530`, `test_e4c_f5_selo_catraca.py:316`.

**5. Os sintéticos usam o visitante real?** Sim. Os dois novos casos do par chamam `_censo_dos_atos_que_desfazem`. O da catraca chama `_julgar`, que passa a função gerada a `_vermelhos` e a `Catraca.visit`. `test_e4c_f5_selo_par_ato_coluna.py:566–575,605–612`, `test_e4c_f5_selo_catraca.py:299–321,380–392`.

## B. Catraca

**6. Qual é a regra literal?** Para cada keyword explícita `update_fields`, o visitante calcula `CAMPOS & _textos(kw.value)` e acusa cada campo ausente de `campos_do_escritor` daquela chamada. `CAMPOS` contém os nomes exatos `respondida_em` e `validada_em`. A isenção exige um `ast.Attribute(Store)` em algum ponto do corpo da função com o mesmo `ast.dump` do receptor. O item 1 do aval 1945 coincide com essa regra sob leitura sintática de “atribui” e “mesmo receptor”. A implementação também aceita um `Store` posterior ao `save`, em ramo oposto ou numa anotação sem valor. Em sentido inverso, `setattr` e atribuição por alias do objeto ficam fora da isenção. A regra se aplica a qualquer chamada com a keyword, inclusive métodos diferentes de `save`. `respostas_2011_1945.txt:2` (anexo), `test_e4c_f5_selo_catraca.py:13,56–61,83–94,192–196,271–294`.

**7. O controle passa?** Sim, na execução em memória. O controle de `:360–369` da base é hoje `test_PASSAM_escrita_foto_e_leitura_do_ato`. `test_e4c_f5_selo_catraca.py:369–378`.

**8. Quais sítios de produção usam os campos em `update_fields`?** Encontrei sete. Todos atribuem ao mesmo receptor e campo. Só `devolver_validacao_recusada` entra na lista de funções da catraca. Passou, e a catraca não acusou nenhum sítio de produção na cópia. `test_e4c_f5_selo_catraca.py:17–28,306–316`. [nao verificado]

| `save` | Função | Atribuição no mesmo receptor |
|---|---|---|
| `models.py:3400` | `PerguntaDisputa.carimbar` | `self.respondida_em` em `:3369`, `self.validada_em` em `:3372`. |
| `disputa_emissao.py:1532` | `responder_pergunta_disputa_core` | `p.respondida_em` em `:1501`, `p.validada_em` em `:1512`. |
| `disputa_emissao.py:1712` | `responder_automaticamente` | `p.respondida_em` em `:1710`. |
| `disputa_emissao.py:1734` | `carimbar_resposta_pela_trilha` | `p.respondida_em` em `:1732`. |
| `validacao.py:171` | `_carimbar_validacao` | `perg.validada_em` em `:168`. |
| `validacao.py:290` | `devolver_validacao_recusada` | `perg.validada_em` em `:287`. |
| `validacao.py:367` | `devolver_paridade_bug_par_103` | `perg.validada_em` em `:365`. |

**9. Que formas escapam?** Lista, tupla, set, variável local ligada antes à coleção conhecida, expansão `*` dentro da coleção e alternativas de expressão condicional são lidos. Escapam `+`, acréscimos via `+=`, `append` ou `extend`, valores vindos de parâmetro, módulo ou chamada sem vínculo conhecido e `**kwargs`, inclusive dicionário literal. Na produção, isso oculta os dois campos em `models.py:3365–3374,3400`, a inclusão de `validada_em` em `disputa_emissao.py:1514,1532` e a concatenação de `validacao.py:367`. `test_e4c_f5_selo_catraca.py:83–94,141–174,192–196,288–294,308–315`.

## C. Geral

**10. O veredito varia com ambiente, relógio ou banco?** `F5_VOLTA_E_MUTANTE` muda a fonte entregue aos dois selos. Os censos AST leem fontes sob `app`, sem consulta de banco ou relógio e sem leitura explícita de arquivo fora da árvore. O arquivo do par também contém testes ORM preexistentes com banco de teste. Neles, uma reabertura carimbada antes de `VALIDACAO_EM` pode deixar a validação vigente e derrubar os asserts de `:703` e `:721`. A porta usa `timezone.now()` e a leitura ordena por instante e PK. `e4c_f5_voltaE_mutantes.py:5,58–69`, `test_e4c_f5_selo_chamada.py:9`, `test_e4c_f5_selo_catraca.py:12,306–315`, `test_e4c_f5_selo_par_ato_coluna.py:499–507,682–722`, `cortes_f2_f9_apoio.py:32,37–74`, `disputa_emissao.py:1648,1664–1668`, `models.py:4363–4379`. [nao verificado]

**11. O delta toca produção ou afrouxa asserções?** O delta anexado toca apenas os três `.py` sob `app/chamados/tests`. Os identificadores dos blobs da base reconstituída e dos arquivos finais coincidem com o anexo. A completude do delta frente ao commit-base [nao verificado]. Nenhuma expressão de asserção foi removida. Estas mudaram de alcance pelo dado que recebem ou pelo ponto onde são executadas. `delta.diff` (anexo), `test_e4c_f5_selo_par_ato_coluna.py:29,435–440,520–521,645–647`, `test_e4c_f5_selo_catraca.py:306–316`, `e4c_f5_voltaE_mutantes.py:63–68`. [nao verificado]

| Asserção existente | Antes | Depois |
|---|---|---|
| Igualdade em `test_e4c_f5_selo_par_ato_coluna.py:645` | `_LIMPAM_VALIDACAO` igual ao conjunto literal dos três atos. | Igual ao conjunto das três constantes do modelo. |
| Subconjunto em `test_e4c_f5_selo_par_ato_coluna.py:647` | `ATOS_COM_FOTO` contido no conjunto literal. | Contido no conjunto derivado do modelo. |
| Âncora em `e4c_f5_voltaE_mutantes.py:67` | `fonte.count(antes) == 1` uma vez por mutante. | A mesma checagem em cada troca, na fonte resultante da troca anterior. |
| `assertEqual(erros, [])` em `test_e4c_f5_selo_par_ato_coluna.py:521` | Pareamento anterior em `_erro`. | A mesma asserção, com a nova comparação de alvos em `:435–440`. |
| `assertEqual(vermelhos, [])` em `test_e4c_f5_selo_catraca.py:316` | Fonte direta e regras anteriores do visitante. | A mesma asserção, com fonte selecionável pelo mutante e a regra de `update_fields`. |

**12. Quais classes reaparecem?** Há mordida nos quatro mutantes, com lacunas demonstradas em A3 e B9. Os julgamentos usam AST. A busca textual está na aplicação das âncoras. A expectativa dos atos passou a derivar da produção, e o próprio nome da coluna vem do metadado do modelo. O delta não introduz campo de dupla semântica. A coluna de projeção, o valor vigente da composta e a foto histórica têm papéis explícitos. `e4c_f5_voltaE_mutantes.py:66–68`, `test_e4c_f5_selo_par_ato_coluna.py:22–29,208–214,479–494,645–647`, `test_e4c_f5_selo_catraca.py:192–196,299–321`, `models.py:2659,4225–4228,4382–4399`. [nao verificado]

## ACHADOS

- **Alta — O selo do par aceita lote vazio.** Reproduzi em memória a preservação do ato com zero perguntas no UPDATE e os dois censos verdes. A comparação pode aceitar a base sem validar `escrever_so`, ou aceitar `escrever_so` sem validar o queryset efetivamente escrito. `test_e4c_f5_selo_par_ato_coluna.py:251–260`, `disputa_emissao.py:1683`, `models.py:1721,3536–3539`.
- **Média — A catraca deixa passar formas de `update_fields` usadas em produção.** Concatenação, mutação de lista e expansão de kwargs passaram nos exemplos em memória. Também passaram a atribuição posterior e a de ramo oposto. Só um dos sete sítios atuais faz parte da lista varrida. `test_e4c_f5_selo_catraca.py:17–28,83–94,192–196,271–287`, `models.py:3365–3374,3400`, `disputa_emissao.py:1514,1532`, `validacao.py:367`.
- **Média — Duas asserções antigas perderam a expectativa literal.** Uma mudança coordenada em `DEVOLUCAO` e na entrada correspondente de `_LIMPAM_VALIDACAO` pode passar onde a igualdade antiga acusaria. `test_e4c_f5_selo_par_ato_coluna.py:29,645–647`, `models.py:4205,4330`.
- **Baixa — Dois asserts ORM preexistentes dependem da ordem entre a data fixa e o relógio.** A leitura pode manter a validação quando a reabertura sai com instante anterior. `test_e4c_f5_selo_par_ato_coluna.py:690–705,713–722`, `cortes_f2_f9_apoio.py:32`, `disputa_emissao.py:1648,1664–1668`, `models.py:4363–4379`.

## 20/11 20:05 · sessao 04a20458663e · - · - · origem=Ronald (tmux)

**C10.** O seletor `F5_VOLTA_E_MUTANTE` muda o texto entregue ao AST. O par já o consultava e a catraca passou a consultá-lo. Os censos leem fontes sob `app`; a referência a `timezone.now` no visitante do par é um reconhecimento sintático. Fontes: `e4c_f5_voltaE_mutantes.py:5,58–69`, `test_e4c_f5_selo_par_ato_coluna.py:345–351,499–507`, `test_e4c_f5_selo_chamada.py:9`, `test_e4c_f5_selo_catraca.py:12,306–316`. [nao verificado]

O arquivo do par também contém testes ORM preexistentes, com banco de teste. Dois vereditos dependem indiretamente do relógio: a porta carimba a reabertura com `timezone.now()`, e a leitura ordena por `ato_em` e PK. Em memória, executei a leitura real com a fixture. Uma reabertura às 09:00 UTC de 18/10 deixa vigente a validação das 10:00 UTC. Às 10:00 com PK posterior ou às 11:00, ela a zera. Fontes: `cortes_f2_f9_apoio.py:32,37–74`, `test_e4c_f5_selo_par_ato_coluna.py:687–705,713–722`, `disputa_emissao.py:1648,1664–1668`, `models.py:4363–4379`. [nao verificado]

**C11.** O delta fornecido altera três arquivos sob `app/chamados/tests`. Reconstituí os três blobs da base em memória. Os IDs da base e do final coincidem com o anexo. Identidade do commit e completude do delta contra a base fora do anexo [nao verificado], pois a cópia não contém `.git`. [nao verificado]

- Nenhuma expressão `assert` ou `assert*` existente foi removida ou substituída. A asserção de unicidade da âncora, antes única em `e4c_f5_voltaE_mutantes.py:37` da base, agora é executada por substituição em `e4c_f5_voltaE_mutantes.py:66–68`.
- As duas asserções existentes que usam `ATOS_QUE_LIMPAM` perderam a expectativa literal. Antes, igualdade e subconjunto contra `{'devolucao', 'recusa', 'reabertura'}`. Agora, contra `{DecisaoPergunta.DEVOLUCAO, DecisaoPergunta.RECUSA, DecisaoPergunta.REABERTURA}`. Base `test_e4c_f5_selo_par_ato_coluna.py:29,498,500`, final `test_e4c_f5_selo_par_ato_coluna.py:29,645,647`.
- `assertEqual(erros, [])` e `assertEqual(vermelhos, [])` mantêm as expressões. Mudaram o pareamento dos registros e a preparação da fonte, respectivamente. Par `:465` na base, `:435–440,521` no final. Catraca `:307` na base, `:306–316` no final.

**C12.** Os julgamentos usam AST. A substituição textual aplica os mutantes antes de analisar a fonte. O oráculo dos atos passou a acompanhar o valor de produção. Renomear `DEVOLUCAO` e a entrada correspondente em `_LIMPAM_VALIDACAO` mantém verdes as duas asserções citadas. O nome da coluna vem do modelo, mas o acesso nominal e a fixture do censo ainda fixam `validada_em`. Fontes: `e4c_f5_voltaE_mutantes.py:66–68`, `test_e4c_f5_selo_catraca.py:193–196,299–315`, `test_e4c_f5_selo_par_ato_coluna.py:22,29,208–214,470–494,624–633,645–647`, `models.py:4205–4206,4330`. [nao verificado]

O delta não introduz campo de dupla semântica. A coluna da projeção e o valor vigente da composta são comparados explicitamente. A foto histórica e o sinal de manutenção da validação têm campos próprios. Fontes: `models.py:2658–2659,4225–4228,4241–4248,4382–4399`.

## 20/11 20:05 · sessao d92fd4adbc79 · - · - · origem=Ronald (tmux)

A1. O visitante real, executado em memória sobre as 829 fontes de `_fontes_de_producao`, produziu cinco entradas pareadas. A regra de inclusão está em `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:479–507`.

| Entrada | Ato | Gravação |
|---|---|---|
| `app/chamados/models.py:1517` `ChamadoColaborador.reabrir` | REABERTURA | Delega a `ressuscitar_perguntas`, com chamada do lote em `models.py:1721` e UPDATE físico em `models.py:3537–3539`. |
| `app/chamados/models.py:1719` `ChamadoColaborador.ressuscitar_perguntas` | REABERTURA | Lista `_compensa` em `models.py:1713–1718`, lote em `models.py:1721` e UPDATE em `models.py:3537–3539`. |
| `app/chamados/services/disputa_emissao.py:1664` `reabrir_para_correcao` | REABERTURA, RECUSA | Guarda em `disputa_emissao.py:1640` e conjunto em `models.py:4217`. Campos em `disputa_emissao.py:1630`, lote em `disputa_emissao.py:1683` e UPDATE em `models.py:3537–3539`. |
| `app/chamados/services/validacao.py:293` `devolver_validacao_recusada` | DEVOLUÇÃO | Atribuição em `validacao.py:287` e save em `validacao.py:290`. |
| `app/chamados/services/validacao.py:370` `devolver_paridade_bug_par_103` | DEVOLUÇÃO | Atribuição em `validacao.py:365` e save em `validacao.py:367`. |

A2. Procurei chamadas `.registrar`, nomes e literais dos atos, aliases de import, construtores, ORM e relação reversa, referências à tabela, montagem das listas e chamadas aos helpers. O único INSERT localizado é o `bulk_create` genérico de `DecisaoPergunta.registrar` em `app/chamados/models.py:4427–4438`. `linhas_da_projecao_sem_linha` só fornece RESPOSTA e VALIDAÇÃO em `models.py:4313–4323`. Há delegadores fora da lista: RECUSA explícita em `app/chamados/services/validacao.py:943–944`, REABERTURA em `models.py:2423`, `app/chamados/views.py:1432`, `app/chamados/services/acoes_disputa.py:106`, `app/chamados/services/materializacao.py:1213` e `app/chamados/management/commands/curar_perguntas_em_limbo.py:70`; ressuscitação em `app/chamados/views_arquivar.py:86`, `app/ponto/services/cartorio.py:1721` e dois saltos em `models.py:1085` → `:1517` → `:1719`. A migração histórica excluída do censo rotula as linhas antigas como REABERTURA via default em `app/chamados/migrations/0051_decisao_pergunta.py:41,49–62`.

A3. O selo aceita `save(update_fields=…)`, `update(…)` e `PerguntaDisputa.descarimbar_em_lote(…)`. Exige um conjunto não vazio e idêntico de nós `with transaction.atomic()` e os mesmos lados de `if` e mesmos `except` no escopo que contém `registrar`, além da igualdade sintática de receptor ou PK ou ligação a lote. `save()` integral, `bulk_update`, variável com queryset filtrado antes, alias de receptor, atomic de decorador/chamador e condições equivalentes escritas em `if` distintos podem acusar um par válido. `update` aceita qualquer receptor com `.filter(pk=…)` coincidente, e o teste novo testa presença do campo sem verificar o valor. A projeção usa um salto no mesmo arquivo, resolve `self.aux` e `cls.aux` para a classe léxica e só projeta `registrar` interno com posição e fora de `if`/`except`. `self.registrar`, `cls.registrar` e alias local por atribuição escapam. A resolução de import com `as` funciona. `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:47–48,86–94,134–172,234–278,291–324,479–495`; `app/chamados/tests/test_e4c_f5_selo_chamada.py:38–57`.

**Achado A3, alto.** Executei variantes em memória nas quais os atos permanecem e o lote fica vazio. Acrescentar `escrever_so=[]` em `app/chamados/services/disputa_emissao.py:1683` ou trocar `alvo_lido` por `alvo_lido.none()` em `app/chamados/models.py:1721` preserva `pareado=True` e a lista vazia de erros no censo antigo. O UPDATE efetivo é limitado pelo queryset e por `escrever_so` em `models.py:3536–3539`, mas o selo aceita coincidência da base ignorando `escrever_so` ou coincidência de `escrever_so` ignorando o queryset em `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:251–260`.

## 20/11 20:04 · sessao d8ff15d33f72 · CADEIA-20-PASSO-1 · leitura · origem=Code

# PAREI RASCUNHO — CADEIA-20-PASSO-1

O Desenho A prevê proteção para **ao menos um caminho de 16 dos 64 sítios**. A cobertura integral desses sítios, a identidade Git da cópia, o pouso da F5 e os resultados dos REDs ficam **[nao verificado]**. As linhas de código abaixo são da cópia lida. As linhas `r.txt:574–587` são da ficha 1545 anexada. `r.txt:574–587` `app/ponto/services/cartorio.py:959–1027` `app/chamados/models.py:3674–3683`

## A. Fatias por escritor

Nas três fatias, o dia é resolvido pela janela da empresa. O cadastro da empresa escreve `dia_inicio_competencia`, `ExportacaoDominio.registrar` e `invalidar` escrevem a entrega e sua invalidação, e `janela_atual` e `_K6` leem esses fatos. O retorno de `_K6` é leitura de TXT vigente. A tranca que o E2 já consulta vem de `_K7`. `app/colaboradores/views.py:515–557` `app/ponto/janelas.py:36–63` `app/folha/models.py:143–189` `app/folha/export.py:556–594` `app/chamados/services/vigia_fio.py:786–808` `app/core/juizes.py:342–343`

### 1. Cartório + relógio

**A1 e A5 — Sítios e arquivos.** **05**, nas duas lavras do corpo, e **19**, na travessia de estado que passa pelo relógio A1. A estimativa de produção é `app/ponto/services/cartorio.py`, `app/chamados/juizes.py` e o `pre_save` existente do chamado em `app/chamados/signals.py`. `app/chamados/services/fecho_runtime.py` entra se hospedar o envelope de A1. Os arquivos de alteração na **F5** são **cartório e juízes**. `models.py` é porta e leitor atingido, também na F5. A ficha manda parar se o encaixe exigir mudar o modelo ou a cláusula. A lista exata de arquivos de teste do rol V3/COBERTURA‑V1 fica [nao verificado]. `r.txt:580,583–587` `app/ponto/services/cartorio.py:1034,1343,1921` `app/chamados/models.py:216–234,302,343–384` `app/chamados/signals.py:177–186` `app/chamados/services/fecho_runtime.py:146–169` `arquivos_da_f5.txt:3,14,115`

**A2 — Unidade.** Para **05**, a unidade é a lavra de uma **célula-dia**, do exame de `_K6` antes da primeira mutação dessa célula até a porta que salva os campos e publica `EventoCelula` no mesmo `atomic`. A janela de batidas e o pareamento continuam inteiros. Dentro da unidade, colher o C6 inicial e o C6 sobre a própria célula referenciada pela pergunta, com **a ata e o par que a porta efetivamente gravaria**. A segunda passada ou a releitura existente consome a divergência na etapa em que ela fica observável. A saída da última leitura é o ponto de `PAREI` para resíduo. Para **19**, o contexto temporal abre antes de `ato()` no fecho do chamado do cartório, cobre gate e tripwire e termina para a mesma instância no `pre_save`, com restauração também em recusa ou exceção. O `atomic` da Pauta de fecho tem vida própria. `r.txt:580,583–585` `app/ponto/services/cartorio.py:1016–1049,1101–1126,1176–1204,1901–1922` `app/ponto/portas/celula.py:957–1006,1181–1202` `app/chamados/services/fecho_runtime.py:146–169` `app/chamados/models.py:216–234,361–384` `app/chamados/signals.py:180–186`

**A3 — RED de primeira lavra e controles do relógio.**

- **Primeira lavra.** Colaborador ativo, vínculo 5×2, DNA 08/12/13/17, E/S de adesão uma semana antes e zero batidas no alvo aberto. Semear `concorde` com ata sem pendências, uma cobrança viva que protesta e sua pergunta ancorada sem carimbo nem validação. Entrar pelo cron `processar_cartorio --apply`. Observar, no mesmo PK e ato, as cinco presenças consumidas de verdade, C6 **0 → 1** no par antes/lavra, a saída coerente e `emitidos` sem aumento. Na árvore atual o fio é lido antes da ata e do par novos, a pergunta sem materialização fica fora da ressurreição, e a releitura pós-fábrica só abre por aumento de `emitidos`. Em outro braço, manter uma célula entregue na margem de um turno que cruza a meia-noite. Exigir a foto persistida da entregue intacta e a célula aberta julgada com o pareamento íntegro. Mutantes de falso verde **só corrigir a foto final em outro cron**, ou **retirar os dias entregues da janela de leitura** quando o teste só compara a foto entregue. `r.txt:580,585` `app/ponto/tests/test_o261_cobrado_le_o_fio.py:243–292` `app/config/crons.py:616` `app/ponto/management/commands/processar_cartorio.py:38–65` `app/ponto/services/cartorio.py:1045–1049,1101–1114,1413–1438,1493–1547,1901–1922` `app/chamados/models.py:1542–1547` `app/chamados/juizes.py:1443–1469`
- **A1.** Entrar pela porta `julgar_celula(..., agora=10:00)` com ambiente às 10:02. Preparar uma cobrança que o cartório tenha motivo real para fechar, uma pergunta de fato ancorada em competência aberta, elegível no SQL, com célula acusatória e carimbo de identidade às 10:01, sem destino C8 válido. Esperar recusa pela cláusula existente. Num segundo caso, elegível e com fecho permitido, observar a chamada real ao C6 com 10:00 **no gate e no tripwire**, e sem esse contexto nas leituras de persistência após `pre_save`. A recusa do primeiro caso para antes do tripwire. Hoje os dois pontos chamam C6 sem o instante do ato. Validação futura é controle negativo da seleção, pois o SQL pode tirá-la antes do C6. Mutantes de falso verde **dar o instante só ao gate**, **deixá-lo vazar pelo `save`**, ou **usar o mesmo relógio nos dois lados**. O alcance ponta a ponta dessas fixtures fica [nao verificado]. `r.txt:583–585` `app/ponto/services/cartorio.py:1015,1034,1569–1581,2059–2089` `app/chamados/models.py:216–234,343–407,422–449,3539–3566` `app/chamados/juizes.py:866–898,1382–1400,1443–1457` `app/chamados/signals.py:180–186`

**A4 — Pouso próprio.** As assinaturas de entrada do E2, da fábrica e do repasse já existem. A fatia pode ser montada sem as outras duas, preservando essas chamadas e seus retornos. Sozinha, ela não fecha as escritas cruzadas dos escritores legados. O aceite de A1 e a compatibilidade com a correção de dia entregue permanecem condicionados aos pontos de D e E. `app/ponto/services/cartorio.py:1019–1049,1061–1114` `app/chamados/juizes.py:1510–1519` `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:94–131` `r.txt:580–587`

**A6 — Escritores e leitores.**

| Campo ou valor | Quem escreve e quem lê |
|---|---|
| Janela, `agora`, `turno_encerrado` e ata candidata | Cron ou porta unitária fornece `agora`. Grade e juiz de turno calculam o encaixe e encerramento. O corpo monta a ata e lê a janela, o encerramento e os marcos no cálculo da impressão e da classificação. C6 lê a célula referenciada e a ata/par projetados no ato. `app/ponto/management/commands/processar_cartorio.py:38–65` `app/ponto/services/cartorio.py:1011–1016,1176–1204,1425–1433,1464–1469,1493–1547,2088–2090` `app/chamados/juizes.py:415–429,1453–1469` |
| Total, vivo, protesta, fio e fio no mesmo protestante | O corpo deriva os cinco de `lista`, `status_local`, marcos e C6. `classificar_dia` recebe as contagens. A impressão recebe o fio e a escolha de `cobrado` usa a interseção entre fio e protesto. O corpo põe `cods` e `rejulgar` em `out`, e o orquestrador consome essas chaves e `emitidos`. `app/ponto/services/cartorio.py:1019–1075,1101–1117,1413–1433,1493–1539,1694–1721,2021–2031` |
| `ata`, `veredito`, `veredito_via`, `veredito_em`, `protestos`, `status_dia`, `impressao`, `julgada_em`, `insumos_em`, `ausencia`, `tem_entrada`, `tem_saida` | A porta da célula prepara a lavra e o carimbo e salva juntos. A via admin participa da precedência. O próximo corpo lê o carimbo para o pulo, C6 e o E2 leem a palavra da célula, e fábrica e lastro leem os marcos da ata. `app/ponto/portas/celula.py:970–987,1042–1102,1181–1202` `app/ponto/services/cartorio.py:1438–1444,1901–1922` `app/chamados/juizes.py:1453–1469` `app/chamados/reconciliador.py:675–722` `app/chamados/services/fabrica_da_celula.py:119–148` `app/chamados/reconciliador.py:1430–1438` |
| Estado do chamado e horizonte do fecho | `_gravar_estado` escreve `status_local`, `resolvido_em`, via e demais campos da transição. Gate e tripwire leem estado e ancoradas no pós-estado em memória. C6 lê validação, resposta, materialização e via no instante fornecido. `pre_save` lê o estado anterior e os receivers e a relavra leem após a escrita. `app/chamados/models.py:209–257,279–319,343–384` `app/chamados/juizes.py:1382–1406` `app/chamados/signals.py:91–117,180–218` |

### 2. E2 + fábrica

**A1 e A5 — Sítios e arquivos.** **37** no carimbo E2, **26** no fecho da disputa e **19** no efeito sobre o pai, com os efeitos síncronos desse fecho no mesmo perímetro. **39** nos descarimbos da fábrica, inclusive o que precede a saída do limbo. **35, 36, 42 e 43** são também alcançáveis pelo nascimento/reuso da fábrica. A ficha detalha a proteção de **39**, mas a cerca de nascimento da fábrica, especialmente o **36**, fica [nao verificado]. Alterações propostas em `app/chamados/reconciliador.py` e `app/chamados/services/fabrica_da_celula.py`. O **reconciliador** está na F5. `models.py` e `fio.py` estão na F5 e são portas chamadas. `r.txt:581–582,585–587` `app/chamados/reconciliador.py:634–667` `app/chamados/services/fabrica_da_celula.py:160–174,206–231` `app/chamados/signals.py:19–46,91–117,149–165` `app/chamados/models.py:3195–3211,3348,3408` `arquivos_da_f5.txt:14–15,25`

**A2 — Unidades.** No E2, uma **candidata** por unidade, com `_K6` pelo dia dela antes de gravar, trava e releitura da porta, término no sucesso ou na recusa desse carimbo. Após o laço, cada **fecho de disputa** tem unidade própria, incluindo o pai e os receivers síncronos que o ato aciona. Na fábrica, a verificação antecede a porta de cada descarimbo atingido, inclusive os dois pontos que chamam `desfazer_carimbo_sem_lastro`. A porta já trava e relê a pergunta e a disputa. O veto de um carimbo entregue precisa deixar disponível a saída existente de Pauta DP do limbo, quando aplicável. A extensão ao nascimento e ao contêiner entra nas decisões de E. `r.txt:581–582` `app/chamados/reconciliador.py:634–667` `app/chamados/services/fio.py:1389–1459` `app/chamados/services/fabrica_da_celula.py:160–174,206–231` `app/chamados/services/relavra_via.py:215–261` `app/chamados/signals.py:19–46,91–117,149–165`

**A3 — RED de veto por candidata com ato aberto sobrevivente.**

- **E2.** No mesmo colaborador ativo com vínculo e adesão, ter um dia aberto com chamado e pergunta vivos, saída já registrada e entrada faltante, e outra pergunta elegível em célula absolutória de dia entregue e sem tranca. Registrar pela porta real a entrada que completa os marcos do dia aberto. O molde existente usa `registrar_batida` e comprova o percurso até o núcleo de fecho. Observar a sequência **lavra → carimbo aberto → tentativa de fecho adiado**, o chamado aberto chegando ao estado terminal e a pergunta entregue conservando os três campos. Variar a ordem dos PKs e incluir fecho de disputa/pai com alcance misto, observando também S143/S144. Hoje o E2 varre o colaborador, filtra por C6 e `_K7` e pode carimbar a candidata entregue sem perguntar a `_K6`. Mutante de falso verde **vetar todo o colaborador** quando o teste só olha a entregue, ou aceitar um fecho produzido depois pelo segundo receiver de batida como se fosse o fecho adiado. `app/chamados/tests/test_o261e_f1_porta_encerramento.py:384–446` `app/ponto/signals.py:131–135,206–215` `app/ponto/services/cartorio.py:1019–1049` `app/chamados/reconciliador.py:634–722` `app/chamados/signals.py:19–46,91–117,149–165`
- **Fábrica.** Pelo cron do cartório, com a fatia isolada, usar dois dias com trabalho previsto, `furo` e `S` isolada no alvo, pergunta de entrada com `VIA_SEM_TURNO_ABERTO` sem lastro e disputa aberta. Um dia com TXT vigente, outro aberto. Exigir que ambos cheguem ao ponto da guarda, só a aberta perca a tripla e que a saída do limbo preserve a Pauta devida. A fábrica atual desfaz o carimbo antes de consultar a saída do limbo. O teste existente de acesso direto até exige o descarimbo entregue. Mutantes de falso verde **guardar apenas depois da saída do limbo**, **inferir zero escrita de `ms=[]`** ou **ganhar verde porque o cartório anterior pulou a entregue**. Se a decisão de escopo incluir a fila, repetir pelo command real `consumir_eventos_celula`, que chega à fábrica sem esse pulo. `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:49–103,225–241` `app/ponto/services/cartorio.py:1061–1075` `app/chamados/services/fabrica_da_celula.py:199–231,268–301` `app/chamados/management/commands/consumir_eventos_celula.py:26–31` `app/chamados/reconciliador.py:1413–1438`

**A4 — Pouso próprio.** E2 e descarimbo podem usar as portas B2 já disponíveis, sem depender de alteração no cartório ou no emissor. O carimbo aberto precisa continuar chegando antes do fecho adiado mesmo com um veto no lote. O pouso que também se comprometer a guardar **INSERT da fábrica** precisa da guarda compartilhada ativa na unidade da fábrica. Esse alcance adicional não está fechado na ficha. `r.txt:581–585` `app/ponto/services/cartorio.py:1019–1049` `app/chamados/services/fio.py:1389–1459` `app/chamados/models.py:3205–3211,3674–3683`

**A6 — Escritores e leitores.**

| Campo ou valor | Quem escreve e quem lê |
|---|---|
| `data_evento`, `conversa`, `celula_id`, `motivo`, dono da pergunta | `_nascer` e o escritor de dia do fato fixam a identidade, as portas de reuso e costura podem atualizar a ligação e a data. E2 e fábrica leem identidade, célula e carimbos para seleção. C6 e B2 leem o dia do turno pelo mesmo acesso que distingue conversa. `app/chamados/models.py:3143–3211` `app/chamados/services/fio.py:839–858,968–976,1069–1092,1405–1425` `app/chamados/juizes.py:338–342` `app/chamados/reconciliador.py:675–722` `app/chamados/services/fabrica_da_celula.py:139–157` |
| `materializada_em`, `materializada_por`, `via_resolucao`; `validada_em`, resposta e via de identidade | E2 usa `PerguntaDisputa.carimbar` para a tripla. Fábrica usa `descarimbar` para limpá-la. Seletores, C6, C7, materialização e outbox leem esses campos. Validação e resposta continuam vindo das portas de validação, resposta e sequência. E2 e fábrica as usam para selecionar ou preservar o ato. `app/chamados/models.py:2002–2015,3293–3353,3364–3409` `app/chamados/services/validacao.py:171,276` `app/chamados/services/disputa_emissao.py:1529–1532` `app/chamados/juizes.py:1382–1451` `app/chamados/signals.py:252–259` |
| `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento`, estado do pai, motivos e retorno | C7 lê perguntas e respostas sem veredito. `DisputaSupervisao.fechar` grava os quatro campos. Defesa 1 lê o fecho e passa o pai à porta de estado. A fábrica deriva motivos da ata, vencimento e identidade; as portas de disputa/pergunta os persistem, ou a porta de Pauta escreve o destino DP. O cartório usa a lista retornada no contador, e a fila soma o retorno do E2. `app/chamados/models.py:2002–2015,2095–2104` `app/chamados/signals.py:19–46,149–165` `app/chamados/services/fabrica_da_celula.py:139–174,199–231,268–284` `app/ponto/services/cartorio.py:1071–1076` `app/chamados/reconciliador.py:1415–1416` |

### 3. Emissor + repasse com guarda no `pre_save`

**A1 e A5 — Sítios e arquivos.** **20, 19 e 22** na abertura, reabertura, espelho e `analisar`. **35, 36, 42, 43 e 60–63** no reuso, nascimento, costura e motivos do repasse. **40** fica protegido **se** o UPDATE for alcançado dentro da unidade cercada. A porta tem o vigia, mas a chamada direta ao UPDATE nos laços de repasse fica [nao verificado]. O fecho final pode atingir **26, 19 e 37** pela cascata. **23, 33 e 58** são efeitos do repasse cuja posição na unidade precisa ser resolvida. Arquivos propostos `app/ponto/management/commands/detectar_ausencias.py`, `app/chamados/services/disputa_emissao.py` e `app/chamados/signals.py`. **`disputa_emissao.py`** está na F5. `models.py` e `fio.py` também estão na lista e são portas já chamadas. `r.txt:582,585–587` `app/ponto/management/commands/detectar_ausencias.py:544–569,694–745,788–807` `app/chamados/services/disputa_emissao.py:1095–1175,1227–1237,1258–1305` `app/chamados/models.py:302,3195–3211,3477–3487` `arquivos_da_f5.txt:14,23,25`

**A2 — Unidades.** No emissor, verificar o dia do **chamado efetivamente aberto, reaberto ou espelhado**, antes da respectiva porta. No repasse, manter a trava existente **Colaborador → Disputa → Chamado → Pergunta**. Abrir a cerca antes da primeira mutação da unidade reversível, com os chamados do mesmo episódio juntos. Fechar depois da pergunta e dos efeitos daquele episódio, inclusive o `analisar` e os motivos que ele pode publicar. No **36**, o `pre_save` da pergunta em inserção, na unidade cercada do cartório, chama o vigia existente depois de `_nascer` atribuir o dia efetivo e antes do INSERT. A recusa precisa sair do bloco antes dos efeitos seguintes. O pai, o contêiner e os motivos iniciais já podem ter sido gravados nessa altura. Um savepoint iniciado só na pergunta deixa essas escritas anteriores de fora. `r.txt:582` `app/ponto/management/commands/detectar_ausencias.py:544–569,671–745` `app/chamados/services/disputa_emissao.py:1007–1073,1095–1175,1194–1237,1263–1305` `app/chamados/models.py:3205–3211,3674–3683` `app/chamados/services/fio.py:1389–1459`

**A3 — RED de pré-INSERT com origem aberta e alvo entregue.** Empresa com corte 21, colaborador ativo com vínculo 5×2 e adesão E/S anterior. Preparar dia aberto A com furo sem chamado, e um órfão vivo do mesmo colaborador no dia E entregue, com ata lavrada e falta real, sem pergunta daquele fato. Incluir outro episódio aberto que deve prosperar e, em um braço, dois chamados da mesma batida-origem. Entrar por `julgar_celula(A)` ou pelo cron do cartório. O emissor abre A e colhe os órfãos do colaborador inteiro. Testemunhar o **pré-save real** com `adding` e dia efetivo E, ausência do INSERT de E, de `analisar` e de motivos atribuídos à unidade vetada. Conferir o INSERT e a costura do episódio aberto, o estado do chamado de A e o contador. Hoje a guarda de `save` pula criação, e o receiver do outbox recebe `created` depois da escrita e retorna. Mutantes de falso verde **filtrar pelo dia do chamado antes de `_nascer`**, **inserir e só depois dar rollback**, ou **vetar o lote todo**. Acrescentar controle noturno em que a data civil da batida e o dia do turno diferem, confirmando que o nascimento proposto chegou ao dia efetivo. A fixture final com todos os portões abertos fica [nao verificado]. `app/ponto/services/cartorio.py:1783–1841,1873–1897,2021–2031,2059–2089` `app/ponto/management/commands/detectar_ausencias.py:694–745,788–807` `app/chamados/services/disputa_emissao.py:837–861,1180–1266` `app/chamados/models.py:3054–3088,3205–3223,3674–3683` `app/chamados/signals.py:252–259` `app/chamados/tests/test_plantio_noturno_mais1.py:39–76,100–105`

No mesmo RED, testar o veto do **emissor** abrindo, reabrindo e espelhando pela porta do cartório numa base sem a primeira fatia, com controle aberto e foto de PK, estado, `marcos_faltantes` e contador. Do contrário, o veto de **05** pode esconder o ramo do emissor. O cron autônomo do detector serve de controle da fronteira externa, conforme a decisão de escopo. `app/ponto/services/cartorio.py:1839–1897` `app/ponto/management/commands/detectar_ausencias.py:424–450,544–569,694–745` `app/config/crons.py:364` `r.txt:580,582,585`

**A4 — Pouso próprio.** O escopo do cartório e o vigia B2 já têm pontos de entrada na árvore. A fatia pode pousar com os consumidores existentes se conservar a distinção entre emissão contada, nenhum trabalho e recusa, e se a recusa parcial tiver o contrato de E. Hoje `REPASSE_RECUSADO` declara **“nada feito”**, enquanto `_contar_emissao` conta retorno não textual do emissor. Uma cobrança aberta de A pode sobreviver a uma recusa do repasse, e esse fato precisa continuar observável. `app/ponto/services/cartorio.py:59,991–994,1150–1153,2021–2031` `app/chamados/services/fio.py:1431–1459` `app/chamados/services/disputa_emissao.py:1036–1049,1303–1313` `app/ponto/management/commands/detectar_ausencias.py:788–807`

**A6 — Escritores e leitores.**

| Campo ou valor | Quem escreve e quem lê |
|---|---|
| `data_turno`, `marcos_faltantes`, `_marco_por`, horário previsto e atraso | Emissor escreve os defaults de a

[truncado em 20000 de 41091 caracteres]

## 20/11 20:02 · sessao 0a4110ebdf00 · - · - · origem=Ronald (tmux)

**D.** A ficha usa os juízes já registrados `_K6`, C6 e C8, os escritores atuais e uma guarda sem persistência. A guarda de INSERT deve ser ativada apenas na unidade do cartório e chamar o vigia existente depois de `_nascer` fixar o dia. Uma ativação global mudaria também regras vigentes de atos externos. [app/docs/JUIZES.md:37–42] [app/chamados/models.py:386–407,3205–3211,3674–3683] [app/chamados/services/fio.py:1389–1428] [app/docs/CORTES.md:426]

1. O encaixe A1 pode fornecer o relógio ao C6, mas a seleção SQL hoje elimina uma validação futura antes do C6. A lista selecionada serve também ao tripwire em memória e à relavra posterior ao `save`. O `pre_save` atual só captura o status anterior. Se o controle exigido depender de mudar esse filtro, aplicam-se o PAREI da ficha e a proibição de alterar a cláusula. O carimbo de identidade futuro é controle alternativo potencialmente alcançável, pois independe daquele filtro de validação. [app/chamados/models.py:216–234,309–319,343–380,386–395,3539–3566] [app/chamados/signals.py:180–186] [app/chamados/juizes.py:1382–1406,1443–1457] [ficha r.txt:583–585]
2. A passagem B5.3b e a releitura pós-emissão são os dois lugares existentes. Usá-los e reportar a divergência residual atende os limites. Redrenar a fila ou remendar no fim seria acrescentar o que o aval veda. O cálculo já retira o resíduo e publica contagem. [app/ponto/services/cartorio.py:1045–1056,1101–1126] [ficha r.txt:580,585]
3. O O44 vigente conserva o fio corrigível com o TXT intacto e a L-113 permite nascimento em competência paga. A fábrica manda limbo entregue à Pauta, conserva nascimento em `trabalha=True` e hoje tenta descarimbar antes da bifurcação. O selo F5 de exportada chama a fábrica diretamente e exige o descarimbo. Um veto global nesse escritor muda o contrato comprovadamente. Um veto restrito ao cartório conserva a passagem direta do selo e executa a escolha local do desenho A. [app/docs/LEIS.md:91,125,155] [app/docs/CORTES.md:426] [app/chamados/services/fabrica_da_celula.py:206–231] [app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:85–87,225–241] [arquivos_da_f5.txt:100]
4. A cerca do escritor de perguntas não decide sozinha o status da disputa, o do pai, seus contextos, a precedência após `julgar_colab`, as ações entregues fora da unidade nova ou o instante concorrente de publicação do TXT. `_K6` consulta a vigência e pode usar memo. O escritor da exportação insere seu registro separadamente. Garantia contra mudança concorrente de vigência entre consulta e escrita [nao verificado]. [app/chamados/reconciliador.py:649–667] [app/chamados/signals.py:19–46] [app/ponto/services/cartorio.py:2088–2096] [app/chamados/services/vigia_fio.py:786–808] [app/folha/export.py:580–592] [app/folha/models.py:173–188]
5. A catraca da cópia registra `1234/1387` e bate com as duas expressões do cartório. Se o diff deslocar essas linhas, o teste exige registro no mesmo commit, outro toque em arquivo listado pela F5. A ficha traz números de outra versão. Resultado de execução [nao verificado]. [app/ponto/services/cartorio.py:1234,1387] [app/core/tests/test_medida_turno_do_par.py:319,558–562] [arquivos_da_f5.txt:109] [nao verificado]

**E.** Quatro decisões observáveis que podem ser explicitadas no brief. Alternativas de implementação como `ContextVar` versus passagem explícita, forma do savepoint, posição do hook ou atualização da catraca não são leis. O uso de `_K6`, C6 e C8, a antecedência do savepoint, o veto B2 de dia indecidível, a exceção conversa e a sequência de travas já estão escritos. [CLAUDE.md:269–281] [app/docs/CORTES.md:558] [app/chamados/juizes.py:338–342] [app/chamados/services/fio.py:1389–1459] [app/chamados/services/disputa_emissao.py:1007–1033]

1. **Alcance das guardas nos escritores compartilhados.** Ativar somente nas unidades originadas no cartório conserva a execução direta compartilhada. Ativar também nas entradas automáticas de cron, signal ou fila altera o E2 e a fábrica que essas entradas chamam, e pode alterar efeitos da política anterior de exportada. A guarda de INSERT continua limitada à unidade do cartório pelo aval. [ficha r.txt:581–585] [app/chamados/reconciliador.py:1413–1438,1483–1493] [app/ponto/management/commands/detectar_ausencias.py:694–745,788–807] [app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:85–87,225–241]
2. **Destino de pai/disputa compartilhados entre atos admitidos e recusados.** Persistir o agregado apenas com unidades admitidas e fechar pelo universo legítimo preserva o progresso independente. Manter o agregado aberto enquanto representa a unidade que recusou preserva a pendência compartilhada. Se nenhum episódio gravar, o código hoje já pode ter criado pai/disputa e motivos, pois antecedem os nascimentos, o que exige definir se há agregado legítimo sem episódio aceito. Nenhuma alternativa autoriza escrever dia entregue. [app/chamados/services/disputa_emissao.py:1110–1175,1194–1237,1270–1305] [app/chamados/reconciliador.py:649–667] [app/chamados/signals.py:19–46]
3. **Contrato observável de sucesso parcial do repasse no emissor.** Conservar a emissão aberta já válida e publicar sucesso parcial por unidade, com veto separado, conserva o `emitidos` como retrato do que abriu. Tratar a emissão e o repasse como inseparáveis implica abortar também os efeitos da origem e reexaminar o push, hoje anterior ao repasse. O sentinela atual diz «nada feito», e `None` pode ocorrer depois de complemento e conta como emissão na outra porta. A forma do retorno é técnica, mas a promessa de efeito parcial é substantiva. [app/chamados/services/disputa_emissao.py:1036–1049,1095–1108,1303–1313] [app/ponto/management/commands/detectar_ausencias.py:775–807] [app/ponto/services/cartorio.py:2021–2031] [nao verificado]
4. **Efeito operacional do PAREI residual, se a obra pretende torná-lo comportamento de execução.** Reportar célula e etapa e travar o pouso conserva atos independentes admissíveis. Abortar a unidade afetada durante execução requer transação aberta antes da primeira escrita e muda quais atos abertos persistem. Se «PAREI da obra» for lido literalmente como condição de aceite pré-pouso, a primeira opção já está escrita e esta decisão sai da contagem. [ficha r.txt:580,585] [app/ponto/services/cartorio.py:1016–1049,1061–1126,1901–1923] [app/chamados/services/fio.py:1431–1459] [nao verificado]

**F.** A F5 é anterior por aval. Entre as três fatias, nenhum código atual obriga ordem total. A1 e a guarda de INSERT compartilham `signals.py`, o que exige conciliar edições. Se «E2 + fábrica» prometer proteger também o nascimento da fábrica no #36, depende da guarda de INSERT ativa naquela unidade ou de pousar antes uma infraestrutura equivalente comum. Se a fatia se limitar ao descarimbo #39, a dependência não existe. O E2 vem antes do fecho adiado dentro da execução, independentemente da ordem de deploy. [app/ponto/services/cartorio.py:1019–1049,1061–1073] [app/chamados/models.py:3205–3211,3674–3683] [app/chamados/services/fabrica_da_celula.py:160–174,206–220] [app/chamados/signals.py:177–186,247–259] [respostas_2011_1950.txt:5–10] [nao verificado]

## 20/11 20:00 · sessao 0dfd8d6ec562 · - · - · origem=Ronald (tmux)

- **Cartório e relógio.** RED pelo cron `processar_cartorio --apply`, com vínculo 5x2, duas batidas anteriores de adesão, zero no dia-alvo, `concorde` prévio e pergunta sem carimbo ancorada em chamado vivo. Observar F/FP realmente consumidos, C6 antes e imediatamente após a primeira lavra e coerência da saída sem depender de emissão. Hoje a leitura vem antes do par/ata da lavra, o chamado existente cala emissão e a releitura pós-emissão depende de incremento em `emitidos`. Controles de célula entregue com TXT vigente e de célula aberta preservam a janela. Um mutante que ajuste o veredito só no fim pode enganar a asserção do estado final. `app/ponto/management/commands/processar_cartorio.py:38–65` `app/ponto/tests/test_o261_cobrado_le_o_fio.py:243–292` `app/ponto/services/cartorio.py:1045–1049,1101–1114,1413–1438,1493–1511,1538–1547,1783,1901–1922` `app/folha/export.py:556–594`
- **Controle do relógio A1.** Porta unitária pública com borda 10:00, ambiente 10:02 e `VIA_ADMIN_DECIDIU` às 10:01 em pergunta ancorada de competência aberta, célula `furo` e sem destino C8 válido. Observar a tentativa real, o C6 do gate, e um controle que consegue fechar e testemunha também o tripwire e o fim do escopo antes dos receivers. A validação futura é controle separado, pois o SQL atual pode retirar a pergunta antes do C6. Relógios iguais, checar só a recusa ou só o gate deixam o mutante passar. Atingimento integral da fixture [nao verificado]. `app/ponto/services/cartorio.py:2059–2089` `app/chamados/models.py:216–234,302–319,343–380,422–449,664–701,3551–3566` `app/chamados/juizes.py:1382–1406,1443–1457` `app/chamados/signals.py:91–118,177–186`
- **E2 e fábrica.** E2 pelo julgamento público do dia aberto ou pelo registro real da última batida. O precedente usa 12x36, saída já registrada, chamado de furo com marco E e última entrada registrada pela porta. Plantar candidata entregue absolvida sem tranca no mesmo colaborador, observar o carimbo aberto antes do fecho adiado e a linha entregue íntegra. Hoje E2 percorre o colaborador e lê C6/K7 antes de escrever sem K6. O mutante que aborta o colaborador inteiro parece verde se a única asserção for a linha entregue. Fábrica com carimbo `sem_turno_aberto` sem lastro, `furo`, disputa aberta e saída isolada, pela execução pública que realmente alcança a fábrica. Observar os três campos e a trilha no mesmo PK, com controle aberto e controle da saída do limbo. Hoje descarimba antes da rota de Pauta. A cerca anterior do cartório pode tornar esse RED vazio. Na base isolada use o cron, e no conjunto use a fila somente se o alcance externo estiver aprovado. A caracterização corrente exige descarimbo no acesso direto em exportada. `app/ponto/tests/test_dst_l2_jz1_rejeita_justificativa.py:200–253` `app/ponto/services/cartorio.py:1019–1049,1061–1073` `app/chamados/reconciliador.py:634–667,675–722,1413–1438` `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:49–103,225–241` `app/chamados/services/fabrica_da_celula.py:206–231,288–301` `app/chamados/services/relavra_via.py:171–184,215–252`
- **Emissor e repasse.** Julgamento público de A aberta com furo e adesão, E entregue com ata acusatória prévia e órfão regularizável sem pergunta. A emissão de A coleta órfãos do colaborador e alcança E. Exigir testemunha do pré-save com `adding` e dia efetivamente atribuído, nenhum SQL INSERT vetado, nenhuma alteração de `analisar`/motivos/contexto da unidade vetada e persistência do aberto. Variação noturna distingue dia civil de dia do turno e exige resolvedora válida. Hoje `save` só chama vigia em atualização e o receiver atual sai em `created`. Vetar o lote inteiro, admitir pelo dia do pedido ou inserir e desfazer depois parece verde se observar só a ausência final da linha. Controle do emissor mede separadamente nascimento, reabertura/espelho, contadores e efeitos de push, pois o cron detector pula existente. `app/ponto/services/cartorio.py:1783–1841,1873–1897,2021–2031` `app/ponto/management/commands/detectar_ausencias.py:424–450,544–569,694–745,788–807` `app/chamados/services/disputa_emissao.py:1059–1073,1100–1146,1180–1266` `app/chamados/models.py:3054–3088,3205–3223,3674–3683` `app/chamados/signals.py:252–259` `app/chamados/tests/test_plantio_noturno_mais1.py:39–76,100–105`
- **Conflitos de alcance.** L-113 permite chamado em competência paga e L-124 conserva fato novo em qualquer competência. Há caracterizações de relavra do cartório entregado, preservando TXT/Gravado, e de fábrica em dia de trabalho entregue. A extensão da cerca a correção ou a chamadas externas exige delimitação. `app/docs/LEIS.md:145,155` `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:77–85,94–96,113–131` `app/chamados/tests/test_f1c4_3_emissores.py:199–218`

## 20/11 20:00 · sessao 805d943d4d4b · - · - · origem=Ronald (tmux)

No sítio **05**, as chamadas de produção identificadas para o carimbo partem do próprio cartório. Um ato externo pode chegar por `julgar_celula`, mas entra em `julgar_colab` antes do carimbo. O save de precedência após a volta é o sítio **06**. Bypass direto de 05 [nao verificado]. `app/ponto/services/cartorio.py:30,1343,1921,2088–2096` `app/ponto/portas/celula.py:1201,1216`

| Sítios | Caminhos externos à unidade proposta |
|---|---|
| **19, 26, 37, 42** | Segundo receiver de batida ou callback de filho terminal → reconciliação por colaborador → E2, órfãs e retratação. O worker também entra diretamente no E2 e em órfãs, premissa e lastro. O fecho de disputa chama o pai e S144. O save terminal do chamado chama S143. Beco pós-commit, atos humanos e materialização também chegam às mesmas portas. `app/ponto/signals.py:206–215` `app/chamados/signals.py:19–46,91–117,149–165,229–240,351–371` `app/chamados/reconciliador.py:642–667,1415–1438,1483–1493,1697–1718` `app/chamados/services/becos.py:55–91` `app/chamados/services/materializacao.py:1059,1084,1981,2043,2180` |
| **19, 20, 22, 35–36, 42–43, 60–63** | Cron detector e emissão retroativa chegam ao emissor e ao repasse. Alertas, início web de órfãos e command de perguntas chegam diretamente ao repasse. Cobrar Dia repassa após retornar do cartório unitário. O detector também fecha por afastamento, cobertura e batida. O espelho, ressalva de limbo e reconciliação de grade compartilham o escritor de contexto. `app/config/crons.py:362–364` `app/ponto/management/commands/detectar_ausencias.py:440,559,694–745,759–767,797–803,821–854` `app/ponto/management/commands/emitir_furo_retroativo.py:154` `app/ponto/management/commands/processar_alertas_turno.py:385` `app/ponto/management/commands/processar_alertas_avancados.py:159` `app/chamados/urls.py:21,32,39` `app/chamados/views.py:674,954` `app/chamados/services/fio.py:595–598` `app/chamados/services/acoes_disputa.py:214–226` `app/chamados/services/cobrar_dia.py:41–49` `app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:19–29` `app/chamados/services/pergunta_no_app.py:253–261` `app/chamados/services/disputa_emissao.py:1041–1073,1095–1099,1118–1175,1227–1237,1258–1298` `app/ponto/management/commands/reconciliar_grade.py:473` |
| **20, 35–36, 39, 42–43** | Worker e backfill chamam a fábrica diretamente. API, browser e retro de foto entram pela emissão de disputa e chegam à geração, inclusive à costura de órfã. Declaração do colaborador e do admin abre ou reusa pergunta datada. Reabertura admin e âncora chegam a 36 pela porta de conversa. `app/chamados/reconciliador.py:1435–1438` `app/chamados/services/fabrica_da_celula.py:160–174,206–219,288–301` `app/chamados/management/commands/backfill_fio_morto_e4b.py:125–143,305–318` `app/api/views.py:768` `app/api/views_core.py:762` `app/ponto/views.py:352` `app/ponto/services/triagem_batida.py:734` `app/ponto/management/commands/marcar_foto_ausente_retro.py:96` `app/chamados/services/disputa_emissao.py:448–473,676–699,1385` `app/chamados/views.py:477–480,1089–1090` `app/chamados/services/declaracao_texto.py:361–378,660–668` `app/chamados/services/acoes_disputa.py:193,263–267` |
| **39, 40, 43** | Reabrir período devolve perguntas pela via de competência. Renascimento, desarquivamento, correção HTML/API/mesa, comando de cura e callback de ausência alcançam o lote 40. Command de migração alcança 43, com veto próprio. `app/ponto/views.py:1523` `app/ponto/services/fechamento.py:1432–1448` `app/chamados/services/competencia_trancada.py:275–282` `app/chamados/models.py:981,1501–1510,1700–1705,3530–3536` `app/chamados/views_arquivar.py:86` `app/chamados/views.py:1329` `app/api/views.py:2706` `app/chamados/services/acoes_disputa.py:105–106` `app/chamados/management/commands/curar_perguntas_em_limbo.py:70` `app/ponto/signals.py:254–272` `app/chamados/services/materializacao.py:1173–1185` `app/chamados/services/disputa_emissao.py:1656` `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:309–319,416,509–522` |

O sítio 20 tem ainda portas diretas de abertura rápida, ausência, adesão, reemissão C2, âncora, régua, alertas, par-relâmpago, turno retroativo e API. A eventual entrada no cartório por receiver se dá depois do save. `app/chamados/views.py:973` `app/ponto/services/ausencia.py:805` `app/chamados/services/adesao.py:75` `app/chamados/reconciliador.py:960–972` `app/chamados/services/disputa_emissao.py:1353` `app/escala/regua_defesa.py:51` `app/ponto/management/commands/processar_alertas_turno.py:335` `app/ponto/management/commands/detectar_par_relampago.py:368` `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151` `app/api/views.py:1985` `app/chamados/models.py:961–1030` `app/chamados/signals.py:189–218`

O sítio 36 recebe o dia efetivo antes de `save(force_insert=True)`. A cópia só vigia perguntas existentes, e a guarda proposta vale para o cartório cercado. Conversa fica fora do conjunto de fatos da cerca. `app/chamados/models.py:2963–3009,3205–3211,3674–3683` `app/chamados/services/fio.py:1389–1404,1414–1418` `r.txt:582–585`

A unidade B2 já existe em retratação, beco, resolver de S143 e no ramo histórico de reatamento. Uma unidade nova aninhada reaproveita a cerca externa e o veto pode desfazer tudo que ela abrange. O callback pode encontrar ou não `em_julgamento` conforme a fronteira transacional. A ocorrência real fica [nao verificado]. `app/chamados/reconciliador.py:562,1707` `app/chamados/services/becos.py:65–66` `app/chamados/services/disputa_emissao.py:1911–1919` `app/chamados/services/fio.py:1431–1459` `app/ponto/services/cartorio.py:991–994,1150–1153`

## 20/11 19:56 · sessao 5bc6b8895944 · E4C-F5-CONSTROI · construtor · origem=Code

**Selo (78) corrigido.** O censo passou de **5 sítios e 1 acusação** para **5 sítios e 0 acusações**. O teste alvo e o novo sintético passaram em memória. Os mutantes isolados produziram **2 falhas** na ressurreição e **1 falha** no lote usado pela RECUSA. [Censo:479](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:479) · [Asserção:530](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:530) [nao verificado]

**Arquivos tocados**

- [test_e4c_f5_selo_par_ato_coluna.py:281](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:281), visitante e teste sintético do (78). [nao verificado]
- [e4c_f5_voltaE_mutantes.py:45](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/e4c_f5_voltaE_mutantes.py:45), duas novas trocas de fonte em memória.

### (a) Sítios enumerados

| Sítio | Escopo atribuído | Ato(s) | Pareado antes → depois | UPDATE pareado |
|---|---|---|---|---|
| [chamados/models.py:1517](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:1517) | `ChamadoColaborador.reabrir` | REABERTURA | Não → sim | `descarimbar_em_lote(..., escrever_so=ids, validada_em=None)` no auxiliar. [models.py:1721](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:1721) |
| [chamados/models.py:1719](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:1719) | `ChamadoColaborador.ressuscitar_perguntas` | REABERTURA | Sim → sim | A mesma chamada à porta do lote. [models.py:1721](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:1721) |
| [chamados/services/disputa_emissao.py:1664](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/disputa_emissao.py:1664) | `reabrir_para_correcao` | REABERTURA, RECUSA | Sim → sim | `descarimbar_em_lote(qs, **campos)`, com `validada_em=None` em `campos`. [disputa_emissao.py:1630](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/disputa_emissao.py:1630) · [disputa_emissao.py:1683](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/disputa_emissao.py:1683) |
| [chamados/services/validacao.py:293](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/validacao.py:293) | `devolver_validacao_recusada` | DEVOLUCAO | Sim → sim | `perg.save(update_fields=['validada_em', 'validada_por'])`. [validacao.py:290](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/validacao.py:290) |
| [chamados/services/validacao.py:370](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/validacao.py:370) | `devolver_paridade_bug_par_103` | DEVOLUCAO | Sim → sim | `perg.save(update_fields=campos + ['validada_em', 'validada_por'])`. [validacao.py:367](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/validacao.py:367) |

**(b) Sim, o escritor já aparecia no próprio escopo.** O censo resolve `registrar(_compensa)` seguindo o `append(dict(...))` dentro do `for`. A linha fornece `pergunta_id=_q.pk` e `ato=DecisaoPergunta.REABERTURA`. A origem do `append` permite ao visitante ligar `_q` ao lote iterado. [models.py:1705](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:1705) · [models.py:1713](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:1713) · [Resolução das linhas:174](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:174) · [Resolução do lote:234](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:234)

**(c) Três chamadores diretos em produção na cópia.** O visitante procura auxiliares entre os escopos do mesmo arquivo e qualifica `self` e `cls` pela classe. [Resolução do auxiliar:297](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:297) · [Censo por arquivo:479](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:479)

- `ChamadoColaborador.reabrir` recebe a atribuição porque `self.ressuscitar_perguntas()` resolve para o método da própria classe. [chamados/models.py:1517](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:1517)
- `desarquivar_chamado` fica fora das atribuições indiretas. A chamada usa `ch` em outro arquivo. [chamados/views_arquivar.py:58](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/views_arquivar.py:58) · [chamados/views_arquivar.py:86](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/views_arquivar.py:86)
- `_julgar_colab_corpo` fica fora das atribuições indiretas. A chamada usa `x` em outro arquivo. [ponto/services/cartorio.py:1155](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/ponto/services/cartorio.py:1155) · [ponto/services/cartorio.py:1721](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/ponto/services/cartorio.py:1721)

**(d) Mesmo conjunto e mesmo `atomic` externo.** Os `ids` relidos vêm de `alvo_lido` sob a trava, com materialização presente e exclusão das vias fora do fato. A REABERTURA percorre `alvo` reconstruído com esses `ids` e a mesma exclusão. O UPDATE intersecta `alvo_lido` com `escrever_so=ids` e aplica a mesma exclusão. As duas chamadas estão no corpo do `atomic` de `ressuscitar_perguntas`. O `bulk_create` do registro também tem um `atomic` interno. [models.py:1639](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:1639) · [models.py:1646](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:1646) · [models.py:1669](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:1669) · [models.py:1705](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:1705) · [models.py:1719](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:1719) · [models.py:3536](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:3536) · [models.py:4437](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:4437)

### Antes e depois do visitante

Na base `ae40b281`, `_registros` devolvia a chamada do auxiliar como origem do ato indireto, e o censo buscava o UPDATE nas escritas do chamador (`test_e4c_f5_selo_par_ato_coluna.py:304` e `:469` na base). Agora o censo continua emitindo a linha atribuída ao chamador e carrega o escopo, a chamada a `registrar` e a origem da linha gravada. É nesse escopo que exige a mesma pergunta ou lote, o mesmo `atomic` e os mesmos ramos. [Atribuição:297](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:297) · [Pareamento e emissão:486](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:486)

O sintético mantém `Porta.chamar` e `Porta.desfazer` enumerados em todos os casos. O par completo no auxiliar passa nos dois. A retirada da coluna acusa os dois, assim como o UPDATE de outra pergunta no chamador, pois falta a escrita pareada no escopo que registra. O sintético também rejeita a escrita da mesma pergunta só no chamador, de outra pergunta no auxiliar, em outro `atomic` ou em outro ramo. Contra o visitante da base, houve **2 falhas na linha 609**, nos casos “mesma pergunta no auxiliar” e “mesma pergunta no chamador”. [Casos:595](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:595) · [Asserção:609](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:609)

### Mutantes e labels

- **`d_ressuscitacao_reabertura_sem_coluna`** remove só `validada_em=None` da chamada real à porta do lote. Em `test_atos_que_desfazem_gravam_a_coluna`, deu **2 falhas na linha 530**, uma para `reabrir` em `models.py:1517` e outra para `ressuscitar_perguntas` em `models.py:1719`. [Tabela:45](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/e4c_f5_voltaE_mutantes.py:45) · [Alvo:1721](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:1721) · [Asserção:530](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:530)
- **`d_recusa_sem_coluna_no_lote`** remove `validada_em=None` de `campos` no escritor real chamado com `ato='recusa'`. Deu **1 falha na linha 530** do mesmo teste, no sítio `disputa_emissao.py:1664`, que enumera REABERTURA e RECUSA juntas. Antes, o `c_recusa_update_fields_cru` da tabela Volta E exercitava a catraca e deixou este selo verde na execução isolada. Os mutantes `recusa_foto_do_ato` da Volta B e `recusar` da Volta D trocam leituras. [Tabela:40](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/e4c_f5_voltaE_mutantes.py:40) · [Novo mutante:50](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/e4c_f5_voltaE_mutantes.py:50) · [Alvo:1630](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/disputa_emissao.py:1630) · [RECUSA:943](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/validacao.py:943) · [Volta B:34](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/e4c_f5_voltaB_mutantes.py:34) · [Volta D:30](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/e4c_f5_voltaD_mutantes.py:30) [nao verificado]

```bash
selo=chamados.tests.test_e4c_f5_selo_par_ato_coluna.SeloParAtoColunaTest
alvo="$selo.test_atos_que_desfazem_gravam_a_coluna"

env -u F5_VOLTA_E_MUTANTE PYTHONDONTWRITEBYTECODE=1 \
  DJANGO_SETTINGS_MODULE=config.settings_test \
  python3 manage.py test "$alvo" \
  "$selo.test_MORDE_update_de_ato_indireto_no_escopo_que_grava" --noinput

PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings_test \
  F5_VOLTA_E_MUTANTE=d_ressuscitacao_reabertura_sem_coluna \
  python3 manage.py test "$alvo" --noinput

PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings_test \
  F5_VOLTA_E_MUTANTE=d_recusa_sem_coluna_no_lote \
  python3 manage.py test "$alvo" --noinput
```

**Limites.** A execução local usou `unittest` em memória, com as constantes extraídas do AST dos modelos. O Python da cópia retornou `ModuleNotFoundError` para `django.core`. Execução dos labels pelo Django, suíte, banco, logs e comportamento em produção **[nao verificado]**. A resolução estática de auxiliares cobre um salto no mesmo arquivo, pelas referências que o visitante reconhece. [manage.py:11](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/manage.py:11) · [Resolução de auxiliares:297](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:297) [nao verificado]

## 20/11 19:56 · sessao 81e83a5485e5 · FALTA-APROVADA-MOVE-O-TXT-ENTREGUE · leitura · origem=Code

As perguntas 33668, 33669 e 33686 correspondem a **dois colaboradores distintos** na amostra: PK 247 na empresa 2 e PK 121 na empresa 3. A passagem por `FALTA_DIA` retirou respectivamente 3, 3 e 1 linhas tipo 10 do TXT que o comparador montou naquele momento. A rejeição restaurou o hash integral e a contagem de linhas da primeira foto. O total na frota é [nao verificado]. `f5_6a1_v2_main.out:14,15,16`, `f5_6a1_v2_f5.out:14,15,16`, `app/folha/export.py:654`.

## 1. Da resposta à retirada das linhas

1. **A porta grava a falta no dia do fato.** A sonda chama `api_disputa_responder` autenticada como o colaborador da pergunta. A API verifica a titularidade e se a pergunta está viva, então chama `responder_pergunta_disputa_core`. O core grava a resposta e encaminha `FALTA_DIA` a `_materializar_ausencia_de_resposta` com `('falta', 'aprovada')`. Essa função chama `criar_ausencia` com o colaborador da disputa e `data=p.data_evento`. Nos três ensaios a linha de `Ausencia` era nova na foto. `app/api/views.py:2631,2646`, `app/chamados/services/disputa_emissao.py:1529,1560`, `app/chamados/services/materializacao.py:1015,1019,1095`, `app/ponto/services/ausencia.py:483,560`, `f5_6a1_v2_main.out:14,15,16`.
2. **A leitura direta da falta pode fechar a porta de homologação.** `txt_vigente_x_gravado` chama `montar_linhas_export`, que chama `classificar_export`. Depois dos crivos anteriores, `classificar_export` chama `_codigos_nao_homologados`. Esse crivo lê `datas_falta` → `datas_falta_janela` → `_datas_ausencia_tipo_janela` → `ausencias_na_janela` e `datas_no_periodo` → `dias_da_ausencia`. O dado lido é `Ausencia` do colaborador, com `status='aprovada'`, `tipo` no conjunto de descontos do catálogo vigente e `data`/`data_fim` que alcançam a janela. Se há dias de falta, há rubrica ativa `falta_diurna` e o código dela está fora do catálogo **não vazio** da empresa, o veredito é `fora / nao_homologado`. Com esse veredito o montador pula o fechamento inteiro, antes de emitir qualquer registro. `app/folha/export.py:63,71,146,169,184,258,654,714,725,974,984,994,1013`, `app/ponto/catalogo/ausencias.py:267,331`, `app/ponto/turnos.py:2802`.
3. **A escrita também pode mudar a porta do espelho.** O `save` de `Ausencia` chama `_cartorio_post_save_ausencia` → `_julgar_dias` → `julgar_celula` → `julgar_colab` → `_julgar_colab_corpo`. O cartório lê a cobertura da ausência e as batidas apuráveis, passa o estado da cobertura a `classificar_dia` e pode gravar outro `veredito`, `protestos` e `ata` por `lavrar_veredito` e `carimbar`. Por exemplo, fato em turno encerrado coberto por ausência aprovada pode produzir `FATO_EM_AUSENCIA`, que vira `fato_em_ausencia`, um veredito que acusa. Na montagem, `classificar_export` → `motivos_retencao_espelho` → `motivos_retencao_celula` → `_dias_sem_decisao` lê a célula gravada. Se restar motivo de retenção, o resultado é `fora / furo_espelho` e o montador pula o colaborador inteiro. A sonda não publicou a célula nem o `status`/`motivo` do classificador. O crivo efetivamente acionado em cada um dos três casos é [nao verificado]. `app/ponto/signals.py:85,143`, `app/ponto/services/cartorio.py:1016,1199,1385,1500,1901,1921,2059,2088`, `app/ponto/supra_juiz.py:268,392,413`, `app/escala/models.py:1619,1624,1767`, `app/ponto/portas/celula.py:957,1090,1181`, `app/folha/export.py:725,968,1176,1247,1273,1293,1301`.
4. **A emissão tem condições por fechamento e por evento.** Para um fechamento `aberto` ou `aprovado`, o colaborador precisa estar no conjunto `entra`. Aí `eventos_do_fechamento` fornece os valores de horas e o montador chama `montar_linha_10` para cada evento com valor não zero e rubrica ativa. Se houver dias de falta e rubrica `falta_diurna`, `montar_bloco_falta` emite um tipo 10 com a quantidade de dias e um tipo 11 por dia. A rejeição passa por `decidir` → `_decidir_no_ato` → `mudar_estado`, grava `rejeitada` e tira a ausência da leitura direta de faltas aprovadas. A sonda também chama explicitamente `devolver_absorvidas_da_ausencia_rejeitada`, serviço que o signal normalmente agenda para depois do commit. `app/folha/export.py:34,87,215,402,687,716,725,730,740`, `app/ponto/services/ausencia.py:975,988,1057,1084`, `app/ponto/signals.py:254,261,272`, `app/chamados/services/materializacao.py:1137`, `f5_6a1_v2_main.out:14,15,16`.

## 2. O que separa os 3 dos outros 13

Os campos que separam os grupos são `diferencas_txt.antes_para_passagem.matricula_do_caso_afetada`, `matriculas_distintas_afetadas`, `por_tipo` e a mudança de `fotos.*.hash_hoje` e `linhas_hoje`. As duas saídas dão os mesmos valores abaixo. `f5_6a1_v2_main.out:4–19`, `f5_6a1_v2_f5.out:4–19`.

| Tentativa | `ausencia.tipo` / `status_apos_passagem` | Matrícula do caso afetada / distintas | Tipo 10 retirados na passagem | `linhas_hoje` antes → passagem → rejeição |
|---|---|---|---:|---|
| 33668 `FALTA_DIA`, empresa 2, PK 247 | `falta` / `aprovada` | `true` / 1 | 3 | 429 → 426 → 429 |
| 33669 `FALTA_DIA`, empresa 2, PK 247 | `falta` / `aprovada` | `true` / 1 | 3 | 429 → 426 → 429 |
| 33686 `FALTA_DIA`, empresa 3, PK 121 | `falta` / `aprovada` | `true` / 1 | 1 | 118 → 117 → 118 |
| 5 outros `FALTA_DIA` | `falta` / `aprovada` | `false` / 0 | 0 | 429 ou 118 em todas as fotos |
| 8 `ATESTADO` | `atestado` / `aguardando_documento` | `false` / 0 | 0 | 429 ou 118 em todas as fotos |

Nos três, o hash publicado muda de `732033257f1c` para `08a1e2ab780f` na empresa 2 e de `4715b21f2af4` para `de6886947941` na empresa 3. A passagem só retira linhas tipo 10 e a rejeição só as devolve. Nos 13, todas as contagens de adição e remoção são zero nas duas etapas, e o par `hash_hoje`/`linhas_hoje` fica igual. Nos 16, `porta=ACEITA`, `nova_na_foto=true`, `status_apos_rejeicao=rejeitada`, `fechamento_mensal_status=aberto`, `divergente=true` e `foto_3_igual_a_1=true`. As fotos não registraram diferenças em `FechamentoMensal`, `Batida` do dia ou no registro vigente de `ExportacaoDominio`. A chave `TXT_do_gravado` na sonda fotografa o par calculado hoje. `f5_6a1_v2_main.out:4–19`, `f5_6a1_v2_f5.out:4–19`, `app/folha/export.py:654,673`.

Pelo código, `aguardando_documento` fica fora da leitura direta de faltas. Para `falta/aprovada`, ainda é preciso a falta alcançar a janela e mudar uma decisão que afete linhas. Na homologação, por exemplo, uma falta adicional pode deixar o resultado igual se já havia dia de desconto, se falta a rubrica, se o catálogo está vazio ou se um crivo anterior já tirou o colaborador. Se a rubrica passa e o colaborador entra, o caminho normal da falta acrescenta seu bloco. Os cinco `FALTA_DIA` sem delta têm o mesmo tipo e estado dos três com delta. O motivo individual dos cinco é [nao verificado]. `app/folha/export.py:71,725,740,866,968,994,1013`, `f5_6a1_v2_main.out:12–19`.

## 3. Qual dado cada caminho usa em competência entregue

| Caminho | Fonte e ponto de decisão |
|---|---|
| **TXT de hoje medido pela sonda** | `txt_vigente_x_gravado` passa sem `fechamentos=`. `classificar_export` e `montar_linhas_export` leem `FechamentoMensal` do banco. As horas vêm desse fechamento. A porta do espelho lê célula gravada, outros crivos consultam batidas e cadastro, e `datas_falta` lê `Ausencia` na hora. `competencia_entregue` seleciona o registro vigente para a comparação de hash. A chamada ao montador é a mesma com ou sem vigente. `app/folha/export.py:556,654,673,714,730,740,782,791,914,1247`. |
| **Botão `Gerar TXT`** | `gerar_dominio_txt` confere a porta de exportação e a existência de fechamentos gravados elegíveis. Em seguida chama `recalcular_fechamento_mes(somente_leitura=True, retornar_objeto=True)` e injeta os objetos recalculados em `classificar_export` e `montar_linhas_export`. O recálculo consulta a cobertura atual de ausências. A escolha dessa fonte é da view, inclusive se já existe TXT entregue. Se a geração prossegue, a view registra uma exportação. `app/folha/views.py:130,149,165,209,214,270,299`, `app/ponto/services/fechamento.py:539,680,1128`. |
| **Download de uma exportação histórica** | `baixar_exportacao` serve `ExportacaoDominio.conteudo` do registro solicitado. `app/folha/views.py:569,575,582`. |

## 4. Censo de leitura da frota

`classificar_export` responde **quem passa pela porta e com qual motivo**, sem montar linhas. `montar_linhas_export` responde **quais linhas saem** e aceita uma lista de fechamentos. `txt_vigente_x_gravado` e `vigia_txt.q1_vigente_x_gravado` comparam com a entrega histórica. Entre esses leitores, nenhum devolve o efeito causal de tirar uma falta do TXT de hoje. O efeito **direto** pode ser medido chamando o montador real duas vezes para um único fechamento, com a mesma foto do banco, omitindo as faltas candidatas apenas da consulta de ausências na segunda montagem. `app/folha/export.py:631,690,714,765,782`, `app/folha/services/vigia_txt.py:207,231,244,254`.

O universo abaixo é cada `(empresa, ano, mês)` que `competencia_entregue` confirma, incluindo competências com zero casos. Dentro da tripla, entram os fechamentos aptos à montagem com pelo menos uma `falta/aprovada` que intersecta a janela. A unidade é o colaborador por tripla, e o total da frota deduplica PKs entre competências. A omissão é **conjunta por colaborador e competência**, inclusive quando duas faltas sustentam o mesmo bloqueio. A sonda mede a contribuição direta das linhas de `Ausencia` com as células gravadas da foto. O efeito histórico de desfazer a lavra que o ato possa ter provocado fica [nao verificado]. `app/chamados/services/materializacao.py:1095`, `app/folha/export.py:47,71,556,687,717,1013,1247`, `app/ponto/models.py:481,485`, `app/ponto/signals.py:143`.

Em uma montagem integral, o classificador percorre os fechamentos da empresa/competência, carrega os dados em lote e faz leituras adicionais por colaborador. O montador percorre os fechamentos elegíveis, resolve eventos e falta para quem entra e guarda as linhas em memória. O comparador ainda serializa e calcula o SHA-256. Duas fotos integrais repetem esse trabalho. O corpo abaixo restringe as duas montagens a cada fechamento candidato e dispensa serializar e hashear o TXT da empresa. Tempo e volume reais na frota são [nao verificado]. `app/folha/export.py:525,542,654,712,725,782,805,994,1247`.

Corpo para um shell Django já na sombra, apresentado sem execução [nao verificado]. A guarda confere configuração, banco e schema em transação `READ ONLY`. O lançador `--rodar` do anexo faz a lavra e grava a saída. `app/config/settings/sombra.py:29,47,60`, `bin/sombra.sh:136,151,157,483,489,492`.

```python
import json
from collections import Counter
from unittest.mock import patch

from django.conf import settings
from django.db import connection, transaction
from django.utils import timezone

from chamados.services.materializacao import ESCAPES_QUE_VIRAM_AUSENCIA
from folha import export as E
from folha.models import ExportacaoDominio
from ponto.models import FechamentoMensal


def medir_fechamento(empresa, mes, ano, fech, pks_omitidos):
    leitor = E.ausencias_na_janela
    com_faltas, _ = E.montar_linhas_export(empresa, mes, ano, fechamentos=[fech])

    def leitor_sem_faltas(*args, **kwargs):
        return leitor(*args, **kwargs).exclude(pk__in=pks_omitidos)

    with patch.object(E, "ausencias_na_janela", leitor_sem_faltas):
        sem_faltas, _ = E.montar_linhas_export(empresa, mes, ano, fechamentos=[fech])

    retiradas = Counter(sem_faltas) - Counter(com_faltas)
    adicionadas = Counter(com_faltas) - Counter(sem_faltas)
    so_perde_tipo_10 = (
        bool(retiradas)
        and not adicionadas
        and all(linha.startswith("10") for linha in retiradas)
    )
    return com_faltas != sem_faltas, so_perde_tipo_10


def contagem(pks):
    return {"n": len(pks), "colaborador_pks": sorted(pks)}


def censo():
    if not (
        settings.SETTINGS_MODULE == "config.settings.sombra"
        and getattr(settings, "HASNER_SOMBRA", False) is True
        and set(settings.DATABASES) == {"default"}
        and connection.vendor == "postgresql"
        and connection.settings_dict["NAME"] == "sombra"
        and connection.settings_dict["HOST"] == "sombra_db"
        and getattr(connection, "schema_name", None) == "juliani"
        and not connection.in_atomic_block
        and connection.get_autocommit()
    ):
        raise RuntimeError("GuardaSombra")

    tipo, estado = ESCAPES_QUE_VIRAM_AUSENCIA["FALTA_DIA"]
    por_competencia = []
    frota_muda, frota_perde_10 = set(), set()
    incomparaveis = 0

    with timezone.override(settings.TIME_ZONE), transaction.atomic():
        with connection.cursor() as cursor:
            cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
            cursor.execute("SELECT pg_catalog.current_database(), pg_catalog.current_schema()")
            if cursor.fetchone() != ("sombra", "juliani"):
                raise RuntimeError("GuardaSombra")

        triplas = list(
            ExportacaoDominio.objects.order_by("empresa_id", "ano", "mes")
            .values_list("empresa_id", "ano", "mes").distinct()
        )
        for empresa_pk, ano, mes in triplas:
            vigente = E.competencia_entregue(empresa_pk, mes, ano, registro=True)
            if vigente is None:
                continue
            empresa = vigente.empresa
            base = {"empresa_pk": empresa_pk, "ano": ano, "mes": mes}
            try:
                E.codigo_no_sistema_dominio(empresa)
            except ValueError:
                incomparaveis += 1
                por_competencia.append({
                    **base, "status": "INCOMPARAVEL", "motivo": "sem_integracao_dominio",
                })
                continue

            ini, fim = E.janela_competencia(empresa, mes, ano)
            fechs = FechamentoMensal.objects.filter(
                colaborador__empresa_id=empresa_pk, ano=ano, mes=mes,
                status__in=E.STATUS_NO_TXT,
            ).select_related("colaborador__empresa").order_by("pk")

            candidatas, muda, perde_10 = set(), set(), set()
            for fech in fechs.iterator(chunk_size=200):
                pks = tuple(
                    E.ausencias_na_janela(fech.colaborador, tipo, ini, fim)
                    .filter(status=estado).order_by("pk").values_list("pk", flat=True)
                )
                if not pks:
                    continue
                pk = fech.colaborador_id
                candidatas.add(pk)
                mudou, perdeu_10 = medir_fechamento(empresa, mes, ano, fech, pks)
                if mudou:
                    muda.add(pk)
                if perdeu_10:
                    perde_10.add(pk)

            frota_muda.update(muda)
            frota_perde_10.update(perde_10)
            por_competencia.append({
                **base, "status": "COMPARADA",
                "com_falta_aprovada": contagem(candidatas),
                "muda_txt": contagem(muda),
                "so_perde_tipo_10": contagem(perde_10),
            })

    return {
        "competencias_incomparaveis": incomparaveis,
        "frota_confirmada": {
            "muda_txt": contagem(frota_muda),
            "so_perde_tipo_10": contagem(frota_perde_10),
        },
        "por_empresa_competencia": por_competencia,
    }


try:
    print(json.dumps(censo(), sort_keys=True))
except Exception as erro:
    print(json.dumps({"status": "INCONCLUSIVO", "erro": type(erro).__name__}))
    raise SystemExit(1) from None
```

## 5. O que fica em aberto

- [nao verificado] O motivo exato de retenção de cada caso, os vereditos da célula antes e depois e a configuração efetiva de `falta_diurna` e do catálogo homologado. A saída anexa mede o delta de linhas e não expõe esses insumos. `f5_6a1_v2_main.out:12–19`, `app/folha/export.py:968,974,1013`.
- [nao verificado] As rubricas e valores das linhas retiradas, se ainda havia linha com a mesma matrícula e quais eram os dias e competências dos casos. A saída publica contagens por tipo, PKs e hashes abreviados. `f5_6a1_v2_main.out:14,15,16`, `app/folha/export.py:34,246,674`.
- [nao verificado] A contagem efetiva da frota, a origem de todas as faltas existentes, o resultado do botão para esses três casos e qualquer alteração de valor pago ou de arquivo importado no Domínio. As `15.715` do resumo são perguntas candidatas da sonda original. Foram exercitadas oito perguntas em 16 tentativas. `f5_6a1_v2_main.out:3`, `f5_6a1_v2_f5.out:3`, `app/folha/views.py:130,209,270,299`.

## 20/11 19:54 · sessao 63ff5f06c7e3 · CADEIA-20-PASSO-1 · censo · origem=Code

# CADEIA-20-PASSO-1 — censo estático do desenho A

O censo encontrou **64 sítios de escrita**, dos quais **61 têm entrada externa identificada**, **27 são alcançáveis durante um callback de `on_commit`** e **33 são alcançáveis por receiver ou fila**. A ficha prevê cobertura para pelo menos um caminho de **16 sítios** e não descreve cobertura para os **48 restantes**. O cruzamento exato com a F5 dá **8 arquivos no perímetro censado**, dos quais **4 constam da estimativa de alteração da ficha**. As contagens usam os IDs das tabelas e o critério abaixo. `r.txt:574–587` `arquivos_da_f5.txt:1–119` `app/core/censo_escritas.py:6–56`

A identidade Git da cópia é **[nao verificado]**. O diretório tem o nome do hash fornecido, mas `git rev-parse HEAD` devolveu `not a git repository`. As localizações executáveis abaixo são da cópia lida. Por exemplo, nela `julgar_colab` começa na linha 959 e chama o E2 na 1027. As referências `r.txt` são da ficha anexada. `app/ponto/services/cartorio.py:959,1019–1027` `r.txt:574–587`

## 1. Escritores, entradas e cobertura

**Como reproduzir.** Rode o scanner abaixo. Para cada ocorrência, confirme o modelo, os campos e a operação persistida. Revise também `entidade=None`, que contém o `save` genérico da célula e três nascimentos por variável ou alias. Nos seis ramos que delegam a `salvar_e_emitir`, conte a chamada de cada operação, pois os campos e as entradas diferem. Conte os demais sítios pelo `save`, `INSERT`, `UPDATE`, `DELETE` ou operação ORM equivalente. Siga referências aos nomes das funções com `rg -n`, incluindo imports com alias, até `urls.py`, `handle`, `config/crons.py`, registro de signal ou consumidor. Inclua `management/` e os chamadores em `bin/`. Use o filtro de código de produção do scanner e confirme caminhos que apenas simulam ou revertem antes de atribuir uma entrada persistente. `app/core/censo_escritas.py:6–82,451–492` `app/ponto/portas/celula.py:957–965` `app/chamados/models.py:1030,1154,3209`

```sh
PYTHONPATH=app python3 -B -c '
from core.censo_escritas import varrer_arvore
alvos = {"CelulaDia", "FuroDiario", "ChamadoColaborador", "DisputaSupervisao",
         "PerguntaDisputa", "DecisaoPergunta", None}
for x in varrer_arvore("app"):
    if x["entidade"] in alvos:
        print(x)
'
```

O recorte conta as operações de negócio sobre `CelulaDia`, `FuroDiario`, o estado e a junção da cobrança, o ciclo da disputa e a identidade, o carimbo e a decisão da pergunta. Inclui os motivos e o contexto que o repasse ou a fábrica persistem como parte da abertura ou da costura. As cinco presenças da ficha são leituras calculadas. `EventoCelula` é a fila que liga os atos. A trilha de auditoria, a Pauta DP, o véu sem mudança de estado e o fechamento financeiro são efeitos adjacentes ao recorte. `app/ponto/portas/celula.py:7–21,945–965,1241–1269` `app/ponto/services/cartorio.py:1413–1433,1493–1503,1538–1539` `app/chamados/services/disputa_emissao.py:1095–1099,1263–1284` `app/chamados/services/fabrica_da_celula.py:201–219,268–284,354–373` `app/chamados/services/arquivo.py:141–173` `app/ponto/services/fechamento.py:1798–1824`

Nas tabelas, **X** significa que existe entrada iniciada fora do cartório, inclusive quando o ato externo depois entra nele. **C** significa que existe uma pilha síncrona que chega ao sítio durante um callback de `on_commit`. **E** significa que existe caminho por signal ou fila. As letras marcam possibilidade estática por sítio, e podem coexistir com a entrada síncrona do cartório. A coluna **A** distingue o trecho que a ficha cobre da cobertura das outras chegadas. `app/ponto/services/cartorio.py:959–1027,2059–2096` `app/chamados/signals.py:122–146,189–218,229–259,351–371` `r.txt:580–585`

### Entradas compartilhadas

| Entrada | Cadeia executável |
|---|---|
| **J — cartório em lote** | Cron `processar_cartorio --apply` → `handle` → `julgar_colab` → corpo, E2, fechos adiados, segunda passada, cluster, fábrica e releitura. A via D5 é `lavrar_status_competencia` → `julgar_colab(completar_status=True)` → `_completar_status`. `app/config/crons.py:616` `app/ponto/management/commands/processar_cartorio.py:65` `app/ponto/management/commands/lavrar_status_competencia.py:56` `app/ponto/services/cartorio.py:940–955,1016–1073,1101–1114` |
| **U — cartório unitário** | `julgar_celula` → `julgar_colab` → precedência após o retorno. Entram os receivers de batida, ausência, justificativa e chamado, o vigia de hora, cobrar dia, pós-vínculo, regeneração e reemissão C2. Também entram os comandos de rejulgamento e de correção. `app/ponto/services/cartorio.py:2059–2096` `app/ponto/signals.py:85–93,131–165` `app/chamados/signals.py:189–218` `app/config/crons.py:367` `app/ponto/services/vigia_de_hora.py:134` `app/chamados/views_cobrar.py:327` `app/chamados/services/cobrar_dia.py:41` `app/colaboradores/services/pos_vinculo.py:73` `app/ponto/portas/celula.py:472` `app/chamados/reconciliador.py:752,970` `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:78` `app/ponto/management/commands/curar_residuo_bug115.py:146` |
| **J/U — outras chamadas de manutenção** | O command de veredito velho, o de régua de defesa, a cura de furo sem cobrança e o script pontual também chamam a porta do cartório. `app/ponto/management/commands/celula_veredito_velho.py:144` `app/escala/management/commands/desfazer_corroboracao_regua.py:44` `app/escala/regua_defesa.py:695,728` `app/ponto/services/furos_sem_cobranca.py:128` `bin/rejulgar_o118_col221.py:24` |
| **G — geração e regeneração** | Cron `gerar_celulas` → command → portas de geração. Também chegam saves de vínculo, calendário, reparentamento de folga, serviços de alteração de vínculo ou de template e commands corretivos. `app/config/crons.py:426` `app/ponto/management/commands/gerar_celulas.py:323,349` `app/escala/signals.py:34–134,185–204` `app/core/signals.py:28–43` `app/escala/folgas.py:293–298,523–541` `app/colaboradores/services/vinculo.py:321–340,657–662` `app/escala/services/cadastro_tipo.py:323,353,376` `app/ponto/management/commands/aplicar_col900.py:234` |
| **M — emissor** | Corpo do cartório → `_abrir_chamado` ou `_espelhar_marcos`. Por fora, cron `detectar_ausencias` → `handle` e command `emitir_furo_retroativo` → `_abrir_chamado`. O próprio detector também tem os fechos por afastamento, cobertura e batida. `app/ponto/services/cartorio.py:1839,1877,1888,1897` `app/config/crons.py:364` `app/ponto/management/commands/detectar_ausencias.py:61,440,544–569,694–745,821–854` `app/ponto/management/commands/emitir_furo_retroativo.py:154` |
| **R — repasse** | M → `_abrir_chamado` → repasse; também os commands de alerta de turno e avançado, a ação web de iniciar órfãos, cobrar dia **após** o retorno do cartório e o command `perguntar_chamados_sem_pergunta`. `app/ponto/management/commands/detectar_ausencias.py:788–807` `app/config/crons.py:362–364` `app/ponto/management/commands/processar_alertas_turno.py:385` `app/ponto/management/commands/processar_alertas_avancados.py:159` `app/chamados/urls.py:21,32,39` `app/chamados/views.py:674,954` `app/chamados/services/acoes_disputa.py:226` `app/chamados/services/cobrar_dia.py:41–49` `app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:19–29` `app/chamados/services/pergunta_no_app.py:261` |
| **F — fábrica** | J/U → `perguntar_pela_celula` → `perguntar` → descarimbo, disputa e geração de perguntas. Também entram Fila e `backfill_fio_morto_e4b`. A porta de criação/agregação de disputa recebe ainda entrada de API, browser e command de foto ausente. `app/ponto/services/cartorio.py:1061–1073` `app/chamados/services/fabrica_da_celula.py:160–174,206–219,288–301` `app/chamados/management/commands/backfill_fio_morto_e4b.py:125–143,316` `app/api/views.py:768` `app/api/views_core.py:762` `app/ponto/views.py:352` `app/ponto/services/triagem_batida.py:734` `app/ponto/management/commands/marcar_foto_ausente_retro.py:96` |
| **E2 — absolvição pela célula** | J/U → E2 antes do fecho adiado. Também Batida `post_save` ou filho terminal pós-commit → `reconciliar_chamados_colab` → E2. Fila chama E2 diretamente. E2 → carimbar pergunta e, se C7 permitir, fechar disputa → receivers do pai e S144. `app/ponto/services/cartorio.py:1019–1049` `app/ponto/signals.py:206–215` `app/chamados/signals.py:19–46,149–165,351–371` `app/chamados/reconciliador.py:634–667,1416,1483` |
| **Fila — `EventoCelula`** | Cron ou command → `consumir_eventos_celula` → rejunção, E2, retratação por célula ou premissa, carimbo de órfãs, fecho por lastro e fábrica. `app/config/crons.py:394` `app/chamados/management/commands/consumir_eventos_celula.py:26–31` `app/chamados/reconciliador.py:1366–1438` |
| **H — atos humanos de pergunta e dia** | Views HTML/API de resposta e correção → portas da pergunta. Ações da mesa → validação, recusa, materialização ou abertura de conversa. View `resolver_dia` → fecha fio e lavra pela via admin. `app/chamados/views.py:1278,1307,1329` `app/api/views.py:2647,2706` `app/chamados/services/acoes_disputa.py:19,193,263–267,375–383` `app/ponto/urls.py:58` `app/ponto/views.py:3466,3494` `app/ponto/services/veredito_celula.py:190,347–352` |

**10 linhas de entradas compartilhadas.** A expansão começa por `julgar_colab`, `julgar_celula`, `_abrir_chamado`, `criar_disputa_a_partir_de_chamados_orfaos`, `perguntar_pela_celula`, `retratar_perguntas_absolvidas_por_celula` e `consumir_eventos_celula`. Os IDs abaixo remetem a essas cadeias e citam a chamada específica. `app/ponto/services/cartorio.py:959,2059` `app/chamados/reconciliador.py:599,1366` `app/chamados/services/fabrica_da_celula.py:178` `app/chamados/services/disputa_emissao.py:1041`

### 1a. Célula e projeção `FuroDiario`

| ID | Sítio e função | Entrada, flags | A |
|---|---|---|---|
| 01 | `app/ponto/portas/celula.py:1066` `lavrar_veredito`, ata/status sob veredito admin | D5 pelo cartório, `app/ponto/services/cartorio.py:940–955`. Sem flags. | A ficha não diz sobre D5. `r.txt:580,585` |
| 02 | `app/ponto/portas/celula.py:1083` `lavrar_veredito`, só status | D5 e a porta humana quando apenas o status difere. **X**. `app/ponto/services/cartorio.py:953` `app/ponto/services/veredito_celula.py:352` | A ficha não diz. `r.txt:580,585` |
| 03 | `app/ponto/portas/celula.py:1098` `lavrar_veredito`, veredito e campos associados | H → `resolver_dia` → lavra admin. **X**. `app/ponto/views.py:3494` `app/ponto/services/veredito_celula.py:352` | A ficha não diz sobre a lavra admin. `r.txt:580,585` |
| 04 | `app/ponto/portas/celula.py:1167` `desfazer_julgamento_da_futura` | G ou reversão → apaga julgamento do DNA anterior. **X E**. `app/ponto/portas/celula.py:267,314,842` `app/escala/signals.py:34–83` | A ficha não diz. `r.txt:580,585` |
| 05 | `app/ponto/portas/celula.py:1201` `carimbar`, incluindo a lavra preparada com `save=False` | J/U → carimba `nunca_bateu` ou o ramo normal. **X C E**. `app/ponto/services/cartorio.py:1321–1345,1901–1922` `app/chamados/signals.py:134–146` `app/ponto/signals.py:131–165` | **Sim** para a lavra do cartório. A ficha deixa as origens externas e os callbacks em aberto. `r.txt:580,585` |
| 06 | `app/ponto/portas/celula.py:1216` `carimbar_precedencia`, chave da ata | U → `aplicar_precedencia_na_celula` depois de voltar de `julgar_colab`. **X C E**. `app/ponto/services/cartorio.py:2046–2055,2088–2096` `app/chamados/signals.py:134–146,218` | A ficha não diz sobre esse save posterior. `r.txt:580,585` |
| 07 | `app/ponto/portas/celula.py:265` `regenerar_celulas_vinculo`, futura fora da vigência | G → ramo sem vínculo. **X E**. `app/escala/signals.py:73–81` | A ficha não diz. `r.txt:580,585` |
| 08 | `app/ponto/portas/celula.py:279` `regenerar_celulas_vinculo`, dia até hoje fora da vigência | G → atualiza `trabalha` e DNA. **X E**. `app/escala/signals.py:73–81` | A ficha não diz. `r.txt:580,585` |
| 09 | `app/ponto/portas/celula.py:293` `regenerar_celulas_vinculo`, INSERT | G, cron de geração ou signal de vínculo. **X E**. `app/ponto/management/commands/gerar_celulas.py:323` `app/escala/signals.py:73–81,125–132` | A ficha não diz. `r.txt:580,585` |
| 10 | `app/ponto/portas/celula.py:310` `regenerar_celulas_vinculo`, regrava geradora/DNA | G, inclusive alteração de âncora por signal. **X E**. `app/escala/signals.py:86–134` | A ficha não diz. `r.txt:580,585` |
| 11 | `app/ponto/portas/celula.py:638` `editar_dia`, INSERT | `trocar_plantao` chama em `app/ponto/portas/celula.py:713`. Entrada de produção anterior a essa porta **[nao verificado]**. | A ficha não diz. `r.txt:580,585` |
| 12 | `app/ponto/portas/celula.py:647` `editar_dia`, UPDATE | `trocar_plantao` chama em `app/ponto/portas/celula.py:713`. Entrada de produção anterior a essa porta **[nao verificado]**. | A ficha não diz. `r.txt:580,585` |
| 13 | `app/ponto/portas/celula.py:836` `reverter_regeneracao_dia` | Command `aplicar_col900`. **X**. `app/ponto/management/commands/aplicar_col900.py:186,198–201` | A ficha não diz. `r.txt:580,585` |
| 14 | `app/ponto/portas/celula.py:914` `gerar_celulas_periodo`, INSERT | G ou command corretivo. **X**. `app/ponto/management/commands/gerar_celulas.py:349` `app/ponto/management/commands/curar_residuo_bug115.py:138` | A ficha não diz. `r.txt:580,585` |
| 15 | `app/ponto/portas/celula.py:926` `gerar_celulas_periodo`, UPDATE de DNA | G ou command corretivo. **X**. `app/ponto/management/commands/gerar_celulas.py:349` `app/ponto/management/commands/curar_residuo_bug115.py:138` | A ficha não diz. `r.txt:580,585` |
| 16 | `app/ponto/portas/celula.py:1237` `carimbar_insumos_em_lote` | Command `backfill_b5_insumos`. **X**. `app/ponto/management/commands/backfill_b5_insumos.py:81,84` | A ficha não diz. `r.txt:580,585` |
| 17 | `app/ponto/portas/celula.py:1258` `gravar_furos`, UPSERT | Cron encadeado `apurar_furos_diarios` ou pós-vínculo/view `resolver_dia`. **X**. `app/config/crons.py:1527` `app/escala/management/commands/apurar_furos_diarios.py:13` `app/escala/services/furos_diarios.py:108` `app/colaboradores/services/pos_vinculo.py:85` `app/ponto/views.py:3505` | A ficha não diz. `r.txt:580,585` |
| 18 | `app/ponto/portas/celula.py:1268` `gravar_furos`, DELETE | Mesmas entradas do UPSERT, com recorte `apagar_em`. **X**. `app/ponto/portas/celula.py:1241–1269` `app/escala/services/furos_diarios.py:108` `app/colaboradores/services/pos_vinculo.py:85` | A ficha não diz. `r.txt:580,585` |

**18 linhas**, correspondentes a **13 sítios ORM** quando se reúne a persistência comum de 01–06 em `salvar_e_emitir`. `app/ponto/portas/celula.py:957–965,1066,1083,1098,1167,1201,1216`

### 1b. Chamado, disputa e pergunta

| ID | Sítio e função | Entrada, flags | A |
|---|---|---|---|
| 19 | `app/chamados/models.py:302` `_gravar_estado` | J/U → fechos direto e adiado, cluster, M/R, E2 → pai, Fila → lastro/retratação, H. **X C E**. `app/ponto/services/cartorio.py:1034,1070,1581,1599,1620,1699,1714` `app/ponto/services/cluster_espurio.py:190` `app/chamados/services/disputa_emissao.py:1099,1173,1192,1237,1266` `app/chamados/signals.py:43,365–371` `app/ponto/services/lastro.py:401` | **Sim** no fecho do pai no E2, na ordenação de `analisar` do repasse e no relógio A1. A ficha não diz sobre cluster, os demais ramos e as origens externas. `r.txt:581–585` |
| 20 | `app/chamados/models.py:1030` `ChamadoColaborador.abrir`, `get_or_create` | M, F e R abrem filho ou pai. J/U também abre cluster. Fila pode chegar pela fábrica e o callback de regularização pode chegar por Batida → U. **X C E**. `app/ponto/management/commands/detectar_ausencias.py:694` `app/chamados/services/disputa_emissao.py:582,1118` `app/ponto/services/cluster_espurio.py:196` `app/chamados/signals.py:134–146` `app/ponto/signals.py:131–135` | **Sim** no emissor cercado. A ficha não diz sobre cluster e as outras origens. `r.txt:582,585` |
| 21 | `app/chamados/models.py:1154` `ChamadoColaborador.criar`, INSERT | Views/API de abertura, command de alertas avançados e portas de abertura. **X**. `app/chamados/views.py:136,197` `app/chamados/services/abertura.py:47,172` `app/api/views.py:2189` `app/ponto/management/commands/processar_alertas_avancados.py:121` | A ficha não diz sobre esta porta de nascimento. `r.txt:582,585` |
| 22 | `app/chamados/services/contexto.py:104` `renovar_contexto` | M → `_espelhar_marcos`, ressalva e agrupamento de limbo. Também command `reconciliar_grade`. **X C E**. `app/ponto/management/commands/detectar_ausencias.py:559,762–767` `app/chamados/services/limbo_folgas.py:250–280` `app/ponto/management/commands/reconciliar_grade.py:473` | **Sim** para o espelho cercado. A ficha não diz sobre os outros contextos. `r.txt:582,585` |
| 23 | `app/chamados/services/contexto.py:163` `fundir_contexto` | R/F → ocorrência → funde os IDs dos órfãos ou o contexto de recorrência no pai. **X C E**. `app/chamados/services/disputa_emissao.py:600,1136,1371` `app/chamados/services/nota.py:110–123` | A ficha não diz sobre o save do contexto do pai. `r.txt:582,585` |
| 24 | `app/chamados/services/contexto.py:127` `retirar_contexto` | Command `reconciliar_grade` retira a marca `orfa_regeneracao` da cobrança. **X**. `app/ponto/management/commands/reconciliar_grade.py:454,474` | A ficha não diz. `r.txt:582,585` |
| 25 | `app/chamados/services/vinculos.py:101` `apontar_celula` | Fila ou callback de regeneração → `rejuntar_celula_dos_vivos`. Também command de carimbo por batida. **X C E**. `app/chamados/reconciliador.py:1415` `app/ponto/portas/celula.py:531–540` `app/chamados/rejuncao.py:98–107` `app/chamados/management/commands/carimbar_por_batida_coincidente.py:100` | A ficha não diz. `r.txt:582,585` |
| 26 | `app/chamados/models.py:2099` `DisputaSupervisao.fechar` | E2, R, Fila, S143, beco pós-commit e ato humano. **X C E**. `app/chamados/reconciliador.py:662,1222` `app/chamados/services/disputa_emissao.py:1298,2114–2131` `app/chamados/services/becos.py:55–66` `app/chamados/models.py:2106–2113` | **Sim** para pôr o efeito do E2 em perímetro próprio. A ficha não diz sobre o beco e os demais ramos. `r.txt:581,585` |
| 27 | `app/chamados/models.py:2324` `reabrir_admin` | H → ação de abrir conversa com motivo novo. **X**. `app/chamados/services/acoes_disputa.py:263–267` | A ficha não diz. `r.txt:582,585` |
| 28 | `app/chamados/models.py:2340` `reabrir_sistema` | Ressuscitação, F → descarimbo com reabertura habilitada, e callback da ausência rejeitada. **X C E**. `app/chamados/models.py:1700–1712` `app/chamados/services/relavra_via.py:259–261` `app/ponto/signals.py:254–272` `app/chamados/services/materializacao.py:1173–1185` | A ficha não diz sobre o save que reabre a disputa. `r.txt:581–585` |
| 29 | `app/chamados/models.py:2403` `reabrir_por_colab`, contador persistido no ato de reabrir o questionário | H, views HTML/API → correção → descarimbo em lote no mesmo atomic. **X**. `app/chamados/views.py:1329` `app/api/views.py:2706` `app/chamados/models.py:2391–2403` | A ficha não diz. `r.txt:585` |
| 30 | `app/chamados/models.py:2131` `aparar_motivo_respondido` | API de foto recebida → retira motivo já respondido por essa via. **X**. `app/api/foto_recebida.py:39–74` | A ficha não diz. `r.txt:582,585` |
| 31 | `app/chamados/models.py:2228` `remover_motivo` | Signals de vínculo/colaborador → retratação por identidade → atualiza motivos e pode fechar. **X E**. `app/escala/signals.py:154–181` `app/colaboradores/signals.py:49–82` `app/chamados/models.py:2217–2235` | A ficha não diz. `r.txt:581–585` |
| 32 | `app/chamados/services/disputa_emissao.py:601` `_abrir_ou_agregar_disputa`, INSERT da fábrica | F e as entradas API/browser/command da mesma porta. **X C E**. `app/chamados/services/fabrica_da_celula.py:172–174` `app/chamados/services/disputa_emissao.py:641,676–699` | A ficha não diz sobre o INSERT do contêiner da fábrica. `r.txt:581–582,585` |
| 33 | `app/chamados/services/disputa_emissao.py:1137` `criar_disputa_a_partir_de_chamados_orfaos`, INSERT da disputa | R → abre pai e disputa antes das perguntas. **X C E**. `app/chamados/services/disputa_emissao.py:1065–1073,1117–1142` `app/ponto/management/commands/detectar_ausencias.py

[truncado em 20000 de 46561 caracteres]

## 20/11 19:51 · sessao e89034ccc907 · E4C-F5-CONSTROI · leitura · origem=Code

## A — Item 11 · Juízes na frota

Na cópia, as assinaturas são `projecao_temporal_da_pergunta(p, agora=None)` e `pergunta_respondida(p)`. A mudança visível está na **entrada**: o C6 entrega `DecisaoPergunta.composta(p)` e, no julgamento com corte, passa ao segundo juiz o par de resposta devolvido pelo primeiro. A sonda identifica esse ponto de chamada na árvore carregada e testa a assinatura com `inspect.signature().bind()` antes de chamar. A existência de uma mudança de assinatura em `eb1e49c1e` está [nao verificado]. `app/chamados/juizes.py:1383–1403,1406–1428,1551–1560` · `app/chamados/models.py:4381–4390`

A classe é o último ato real até o horizonte, em ordem `(ato_em, pk)`, com `sem_ato` para sequência vazia. O subtotal `passivo_sem_linha` usa o leitor real da E1 e pode incluir uma pergunta com ato `silencio`. A saída também dá `passivo_sem_ato`, a interseção com sequência vazia. O corte fixo de **20/11/2026 às 00:00 −03:00** e o teto de **250 PKs por lista** são escolhas da sonda. Cruze por `(horizonte, pergunta_pk)` dentro do mesmo snapshot. Cada classe e cada combinação de valores trazem a contagem cheia e a quantidade de PKs omitidos. `app/chamados/models.py:4205–4219,4252–4253,4291–4323` · `app/chamados/services/tarefa_do_dia.py:142–155,182–183,430–434` · `app/docs/RELATO.md:397`

```python
import json
from collections import Counter, defaultdict
from contextlib import closing
from datetime import datetime
from inspect import signature, unwrap
from itertools import islice
from types import SimpleNamespace

from django.conf import settings
from django.db import connection, transaction


CORTE = datetime.fromisoformat("2026-11-20T00:00:00-03:00")
TETO_PKS = 250
LOTE = 1000
HORIZONTES = (("atual", None), ("corte", CORTE))
CAMPOS = ("validada_em", "respondida_em", "resposta_colab",
          "materializada_em", "via_resolucao")


def emitir(**dados):
    print(json.dumps(dados, sort_keys=True), flush=True)


def guarda():
    # app/config/settings/sombra.py:29,47–60; anexo e4c_f5_ab_entregue.py
    if (
        getattr(settings, "HASNER_SOMBRA", False) is not True
        or connection.vendor != "postgresql"
        or connection.settings_dict["NAME"] != "sombra"
        or getattr(connection, "schema_name", None) != "juliani"
        or connection.in_atomic_block
        or not connection.get_autocommit()
    ):
        raise RuntimeError("guarda_sombra")


def chamada(funcao, formas):
    assinatura = signature(funcao)
    for forma in formas:
        args, kwargs = forma(object(), None)
        try:
            assinatura.bind(*args, **kwargs)
        except TypeError:
            continue

        def chamar(p, agora=None, forma=forma):
            args, kwargs = forma(p, agora)
            return funcao(*args, **kwargs)

        return chamar
    raise TypeError("assinatura sem forma reconhecida")


def bloco():
    return {
        "n": 0, "pks": [], "passivo_sem_linha": 0, "passivo_sem_ato": 0,
        "sem_ato_real": 0, "por_respondida": Counter(),
        "valores": defaultdict(lambda: [0, []]),
    }


def acumular(b, pk, classe, passivo, respondida, valor, campos_diferentes):
    b["n"] += 1
    b["sem_ato_real"] += classe == "sem_ato"
    b["passivo_sem_linha"] += passivo
    b["passivo_sem_ato"] += passivo and classe == "sem_ato"
    b["por_respondida"][str(respondida).lower()] += 1
    if len(b["pks"]) < TETO_PKS:
        b["pks"].append(pk)
    grupo = b["valores"][(respondida, valor, campos_diferentes)]
    grupo[0] += 1
    if len(grupo[1]) < TETO_PKS:
        grupo[1].append(pk)


def principal():
    fase = "guarda_sombra"
    vistos = Counter()
    erros = Counter()
    blocos = defaultdict(bloco)
    try:
        guarda()
        with transaction.atomic(using=connection.alias):
            with connection.cursor() as cur:
                cur.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
                cur.execute(
                    "SELECT current_database(), current_schema(), transaction_timestamp()"
                )
                banco, esquema, instante = cur.fetchone()
            if (banco, esquema) != ("sombra", "juliani"):
                raise RuntimeError("guarda_sombra")

            fase = "contratos"
            from chamados import juizes
            from chamados.models import DecisaoPergunta as D, PerguntaDisputa as P

            # app/chamados/juizes.py:1383,1406–1428,1551; app/chamados/models.py:4381–4390
            c6 = unwrap(juizes._pergunta_viva_calc)
            composta_no_c6 = "composta" in c6.__code__.co_names
            preparar = D.composta if composta_no_c6 else lambda p: p
            projetar = chamada(juizes.projecao_temporal_da_pergunta, (
                lambda p, agora: ((p,), {"agora": agora}),
                lambda p, agora: ((p, agora), {}),
                lambda p, agora: ((), {"p": p, "agora": agora}),
            ))
            responder = chamada(juizes.pergunta_respondida, (
                lambda p, _: ((p,), {}), lambda p, _: ((), {"p": p}),
            ))

            # app/chamados/models.py:4205–4219,4252–4253,2771–2774
            atos_conhecidos = dict(D.ATOS)
            vias_conhecidas = dict(P._meta.get_field("via_resolucao").flatchoices)
            universo = (
                P.objects.using(connection.alias).order_by("pk")
                .select_related("disputa__colaborador").prefetch_related("decisoes")
            )
            emitir(
                registro="inicio", banco=banco, esquema=esquema,
                instante=instante.isoformat(), corte=CORTE.isoformat(),
                teto_pks=TETO_PKS,
                entrada="composta" if composta_no_c6 else "orm",
            )
            fase = "frota"
            with closing(universo.iterator(chunk_size=LOTE)) as linhas:
                while lote := list(islice(linhas, LOTE)):
                    # app/chamados/models.py:4291–4323
                    # app/chamados/services/tarefa_do_dia.py:430–434
                    faltantes = defaultdict(list)
                    for linha in D.linhas_da_projecao_sem_linha(lote):
                        faltantes[linha["pergunta_id"]].append(linha["ato_em"])

                    for p in lote:
                        # app/chamados/models.py:1892–1896,2626–2630
                        # app/colaboradores/models.py:321
                        emp = p.disputa.colaborador.empresa_id
                        for escopo in (None, emp):
                            vistos[escopo] += 1
                        try:
                            entrada = preparar(p)
                            atos = list(p.decisoes.all())
                            resultados = []
                            for horizonte, agora in HORIZONTES:
                                ate = [a for a in atos if agora is None or a.ato_em <= agora]
                                ultimo = max(ate, key=lambda a: (a.ato_em, a.pk), default=None)
                                classe = "sem_ato" if ultimo is None else (
                                    ultimo.ato if ultimo.ato in atos_conhecidos else "ato_fora_catalogo"
                                )
                                passivo = any(agora is None or em <= agora for em in faltantes[p.pk])

                                # app/chamados/juizes.py:1383–1403,1408–1428,1533–1535
                                proj = tuple(projetar(entrada, agora))
                                v, r, texto, m, via = proj
                                bruto = tuple(projetar(p, agora)) if composta_no_c6 else proj
                                entrada_resposta = entrada
                                if agora is not None:
                                    entrada_resposta = SimpleNamespace(**dict(zip(CAMPOS, proj)))
                                respondida = responder(entrada_resposta)
                                if type(respondida) is not bool:
                                    raise TypeError("retorno de pergunta_respondida")
                                valor = (
                                    v is not None, r is not None,
                                    not juizes.texto_vazio(texto), m is not None,
                                    "sem_via" if via in ("", None) else (
                                        via if via in vias_conhecidas else "via_fora_catalogo"
                                    ),
                                )
                                diferentes = tuple(
                                    nome for nome, a, b in zip(CAMPOS, proj, bruto) if a != b
                                )
                                resultados.append(
                                    (horizonte, classe, passivo, respondida, valor, diferentes)
                                )
                        except Exception as exc:
                            for escopo in (None, emp):
                                erros[escopo] += 1
                            emitir(registro="excecao", status="INCONCLUSIVO", empresa_pk=emp,
                                   pergunta_pk=p.pk, tipo_excecao=type(exc).__name__, n=1)
                            continue

                        for horizonte, classe, passivo, respondida, valor, diferentes in resultados:
                            for escopo in (None, emp):
                                for chave_classe in (None, classe):
                                    b = blocos[escopo, horizonte, chave_classe]
                                    acumular(b, p.pk, classe, passivo, respondida, valor, diferentes)
            transaction.set_rollback(True, using=connection.alias)
    except Exception as exc:
        emitir(registro="fim", status="INCONCLUSIVO", fase=fase,
               tipo_excecao=type(exc).__name__, excecoes=erros[None] + 1)
        return 1

    for (emp, horizonte, classe), b in blocos.items():
        valores = [
            dict(respondida=resp, projecao=list(valor), difere_da_orm=list(dif),
                 n=n, pergunta_pks=pks, pks_omitidos=n - len(pks))
            for (resp, valor, dif), (n, pks) in sorted(b["valores"].items())
        ]
        emitir(
            registro="classe" if classe is not None else "placar",
            status="INCONCLUSIVO" if erros[emp] else "CONCLUSIVO",
            empresa_pk=emp, horizonte=horizonte, classe=classe, n=b["n"],
            pergunta_pks=b["pks"], pks_omitidos=b["n"] - len(b["pks"]),
            sem_ato_real=b["sem_ato_real"], passivo_sem_linha=b["passivo_sem_linha"],
            passivo_sem_ato=b["passivo_sem_ato"], por_respondida=b["por_respondida"],
            ordem_projecao=["validada_presente", "respondida_presente", "texto_presente",
                            "materializada_presente", "via"],
            por_valor=valores,
        )
    for emp, n in vistos.items():
        emitir(registro="cobertura", empresa_pk=emp, n=n, julgadas=n - erros[emp],
               excecoes=erros[emp], status="INCONCLUSIVO" if erros[emp] else "CONCLUSIVO")
    incompleto = bool(erros[None]) or not vistos[None]
    emitir(registro="fim", status="INCONCLUSIVO" if incompleto else "CONCLUSIVO",
           n=vistos[None], julgadas=vistos[None] - erros[None], excecoes=erros[None])
    return int(incompleto)


raise SystemExit(principal())
```

## B — Item 12 · Validar, recusar e materializar

- **Validar e recusar.** Até 20 perguntas por empresa da fila real `perguntas_respondidas_a_validar()`, por `respondida_em` e PK para desempate. A fila é o recorte da sonda. Os serviços recebem o PK e fazem suas próprias guardas. Uso o mesmo superusuário ativo de menor PK, o valor padrão de `confirmar_implausivel` e um motivo fixo de ensaio. A saída informa o PK do autor. `app/chamados/juizes.py:1770–1775` · `app/chamados/services/validacao.py:677–704,714–723,875–929`
- **Materializar.** Até 20 perguntas por empresa, por PK, no filtro inicial da porta real. A sonda chama a porta pública uma vez por disputa que entrou no recorte. O laço real continua livre para processar irmãs. A observação de `_materializar_uma` preserva seu retorno e permite distinguir `False` de uma pergunta que ficou fora da chamada individual. A porta pública também devolve o placar da disputa. `app/chamados/services/materializacao.py:1315–1341,1880–1895,2106–2174`
- **Efeitos.** A sonda intercepta FCM e web push nos pontos de envio cujo retorno é descartado pelos chamadores indicados. `on_commit` e `pg_notify` ficam sob o rollback. O outbox `EventoCelula` é um `INSERT` da mesma transação. A rota de ausência chamada pela materialização segue sem upload. Se surgir escrita por `FieldFile` ou conexão de rede Python, a sonda bloqueia a operação e marca a chamada `INCONCLUSIVO`. Ela também intercepta os handlers de log para registrar apenas tipos de exceção, pois a configuração da sombra herda handlers que escrevem arquivo. `app/chamados/services/validacao.py:961–972` · `app/chamados/services/disputa_emissao.py:1816–1848` · `app/ponto/services/fechamento.py:1798–1825` · `app/ponto/realtime.py:22–43` · `app/ponto/portas/celula.py:945–954` · `app/chamados/services/materializacao.py:988–996,1027–1035` · `app/ponto/services/ausencia.py:240–266` · `app/config/settings/sombra.py:27` · `app/config/settings/saas.py:131–183`

```python
import ast
import inspect
import json
import logging
import re
import socket
import sys
from collections import Counter, defaultdict
from textwrap import dedent
from unittest.mock import patch

from django.conf import settings
from django.contrib.auth import get_user_model
from django.db import connection, transaction
from django.db.models.fields.files import FieldFile


TETO_POR_EMPRESA = 20
MOTIVO = "Ensaio na sombra com rollback"
CODIGO = re.compile(r"[a-z][a-z0-9_]{0,63}", re.ASCII)


class EfeitoExternoBloqueado(RuntimeError):
    pass


def emitir(**dados):
    print(json.dumps(dados, sort_keys=True), flush=True)


def guarda():
    # app/config/settings/sombra.py:29,47–60; anexo e4c_f5_ab_entregue.py
    if (
        getattr(settings, "HASNER_SOMBRA", False) is not True
        or connection.vendor != "postgresql"
        or connection.settings_dict["NAME"] != "sombra"
        or getattr(connection, "schema_name", None) != "juliani"
        or connection.in_atomic_block
        or not connection.get_autocommit()
    ):
        raise RuntimeError("guarda_sombra")


def grupos(linhas):
    por_empresa = defaultdict(list)
    for empresa, disputa, pergunta in linhas:
        por_empresa[empresa].append((disputa, pergunta))
    return por_empresa


def codigo_do_estado(retorno):
    estado = retorno["estado"]
    if not isinstance(estado, str) or CODIGO.fullmatch(estado) is None:
        raise TypeError("estado fora do formato")
    if type(retorno["ok"]) is not bool:
        raise TypeError("ok fora do formato")
    return estado


def pontos_de_captura(funcoes):
    pontos = {}
    for funcao in funcoes:
        fonte, inicio = inspect.getsourcelines(funcao)
        arvore = ast.parse(dedent("".join(fonte)))
        pontos[funcao.__code__] = {
            inicio + no.body[0].lineno - 1: no.name
            for no in ast.walk(arvore)
            if isinstance(no, ast.ExceptHandler) and no.name
            and isinstance(no.type, ast.Name) and no.type.id == "Exception"
        }
    return pontos


def principal():
    fase = "guarda_sombra"
    falhas = 0
    excecoes = 0
    tentativas = 0
    try:
        guarda()
        with transaction.atomic(using=connection.alias):
            with connection.cursor() as cur:
                cur.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ WRITE")
                cur.execute(
                    "SELECT current_database(), current_schema(), transaction_timestamp()"
                )
                banco, esquema, instante = cur.fetchone()
            if (banco, esquema) != ("sombra", "juliani"):
                raise RuntimeError("guarda_sombra")

            fase = "candidatas"
            from chamados import fcm_utils, juizes, push_utils [nao verificado]
            from chamados.models import DecisaoPergunta as D, DisputaSupervisao as Disputa
            from chamados.models import PerguntaDisputa as P
            from chamados.services import materializacao as m, validacao as v
            from escala.models import EventoCelula
            from ponto.models import Ausencia, Batida

            alias = connection.alias
            perguntas = P.objects.using(alias)
            autor = (
                get_user_model().objects.using(alias)
                .filter(is_active=True, is_superuser=True).order_by("pk").first()
            )
            if autor is None:
                raise LookupError("autor de ensaio ausente")

            emitir(registro="inicio", banco=banco, esquema=esquema,
                   instante=instante.isoformat(), autor_pk=autor.pk,
                   teto_por_porta_empresa=TETO_POR_EMPRESA)

            # app/chamados/juizes.py:1770–1775
            fila = sorted(juizes.perguntas_respondidas_a_validar(),
                          key=lambda p: (p.respondida_em, p.pk))
            humanas = grupos(
                (p.disputa.colaborador.empresa_id, p.disputa_id, p.pk) for p in fila
            )

            # app/chamados/services/materializacao.py:2140–2146
            prontas = grupos(
                perguntas.filter(
                    juizes.q_respondida_com_texto() | juizes.q_respondida_sem_texto(),
                    validada_em__isnull=False, materializada_em__isnull=True,
                ).order_by("pk").values_list(
                    "disputa__colaborador__empresa_id", "disputa_id", "pk"
                )
            )

            # app/chamados/services/materializacao.py:1635–1637,2024–2029
            unidade = m._materializar_uma
            pontos = pontos_de_captura((unidade, m._materializar_resposta_data_hora))

            def ensaiar(porta, empresa, disputa_pk, pks):
                d = Disputa.objects.using(alias).get(pk=disputa_pk)
                alvo = perguntas.filter(pk__in=pks, disputa_id=disputa_pk)
                contadores = {
                    # app/chamados/services/validacao.py:161–175,938–944
                    "decisoes_disputa": D.objects.using(alias).filter(
                        pergunta__disputa_id=disputa_pk
                    ),
                    # app/chamados/services/materializacao.py:988–996,1517–1529
                    "batidas_colaborador": Batida.objects.using(alias).filter(
                        colaborador_id=d.colaborador_id
                    ),
                    "ausencias_colaborador": Ausencia.objects.using(alias).filter(
                        colaborador_id=d.colaborador_id
                    ),
                    # app/ponto/portas/celula.py:945–954
                    "eventos_colaborador": EventoCelula.objects.using(alias).filter(
                        colaborador_id=d.colaborador_id
                    ),
                }

                def contar():
                    return {nome: qs.count() for nome, qs in contadores.items()}

                antes = contar()
                durante = None
                prova_after = None
                retornos = defaultdict(list)
                tipos = Counter()
                efeitos = Counter()
                resultados = {}
                agregado = None
                escapou = None

                def observar_unidade(disputa, p, request_user, erros, reusos):
                    retorno = unidade(disputa, p, request_user, erros, reusos)
                    retornos[p.pk].append(retorno)
                    return retorno

                def rastrear(frame, evento, arg):
                    if frame.f_code not in pontos:
                        return None
                    if evento == "line":
                        nome = pontos[frame.f_code].get(frame.f_lineno)
                        exc = frame.f_locals.get(nome) if nome else None
               

[truncado em 20000 de 35026 caracteres]

## 20/11 19:49 · sessao 51f19dd66a7a · CADEIA-20-PASSO-1 · leitura · origem=Code

**Veredito para a 129134:** o código explica por que a mudança de `turno_encerrado=0` para `1` pode tirar a célula do pulo por impressão e fazê-la receber novo carimbo, mesmo com zero códigos. O anexo registra essa mudança de entrada e uma mudança na foto durante o corpo inicial. O campo que mudou na foto, a causa de cada uma das 31 chamadas e a convergência com o relógio fixo ficam **[nao verificado]**. `app/ponto/services/cartorio.py:1540`, `:1552`, `:1627`, `:2036`; `celula_129134_3rodadas.jsonl:-8`; `cruza_r3.jsonl:13`, `:16`, `:41`.

A cópia disponível não contém metadados Git para confirmar o hash. A identidade da árvore executada e os dois mapas citados em `logs/` ficam [nao verificado]. Nas referências ao JSONL da célula, `-1` é a última linha do anexo.

## 1. Escritores entre `antes_corpo_inicial` e `depois_corpo_inicial`

O comando passa ao Cartório todas as células passadas do colaborador, por data, com `apply_=True` e sem `--forcar`. A sonda põe as duas fotos em torno da chamada real ao corpo com `adiar_pela_lampada=True`. Cada foto repete a leitura com células frescas, `apply_=False` e `forcar=True`. E2, fecho **adiado**, segunda passada, cluster, fábrica e eventual releitura são chamados pelo orquestrador **depois** desse intervalo. `medida.py:117`, `:127`, `:150`, `:358`; `app/ponto/management/commands/processar_cartorio.py:29`, `:31`, `:63`, `:69`; `app/ponto/services/cartorio.py:1123`, `:1126`, `:1152`, `:1174`, `:1179`, `:1209`.

| Escritor alcançável no corpo inicial | Condição de escrita e leitura que decide |
|---|---|
| **Lavra normal e carimbo da célula** | O corpo calcula `imp`, compara com `cel.impressao`, `julgada_em` e `veredito`, e precisa encontrar o dia na grade. Com `apply_`, monta ata e status, chama `lavrar_veredito(save=False)` e passa os campos retornados a `carimbar`. A lavra compara ata, veredito, via, protestos e status e respeita a decisão do admin. O carimbo compara também `insumos_em`, ausência, entrada e saída. Se a guarda de igualdade falhar, grava os campos do carimbo e os lavrados em um `save`. `app/ponto/services/cartorio.py:1540`, `:1552`, `:1559`, `:2012`, `:2036`; `app/ponto/portas/celula.py:1045`, `:1058`, `:1060`, `:1075`, `:1090`, `:1189`, `:1201`, `:963`. |
| **Lavra e carimbo no ramo `nunca_bateu`** | A ausência de primeira batida não retratada leva a esse ramo. A guarda lê a impressão anterior, `julgada_em` e `veredito`. Se passar, com `apply_`, o corpo lê cobertura, grade, DNA, cadastro e encerramento, prepara a ata e chama a mesma porta de carimbo. Nesse ramo a classificação fica dentro de `if apply_`, portanto as fotos adicionais `foto=1` da 129134 apontam para o ramo normal da cópia. `app/ponto/services/cartorio.py:1345`, `:1370`, `:1393`, `:1399`, `:1402`, `:1410`, `:1415`, `:1435`, `:1457`; `medida.py:127`, `:174`; `cruza_r3.jsonl:14`, `:16`. |
| **Projeção em memória da ata e do veredito** | No ramo normal, depois do pulo por impressão, o corpo monta `_ata_agora` com grade, marcos, DNA e cobertura e a põe em `_tl.atas`. Depois põe em `_tl.vereditos` o par que a lavra deixaria, a partir dos códigos, do fio que protesta e da via anterior. O leitor da pergunta pode receber uma cópia da célula com esses valores antes do `save`. `app/ponto/services/cartorio.py:1556`, `:1579`, `:1614`, `:1652`, `:1660`; `app/chamados/juizes.py:424`; `app/ponto/portas/celula.py:978`, `:998`. |
| **Fechos imediatos e seus efeitos sobre o fio** | O corpo tenta fechar chamados vivos que não protestam e ficaram fora do adiamento, que cobram marcos inexistentes, ou que caem nos dois códigos de cobrança indevida. Também tenta fechar no dedup de fio morto. Lê os chamados e suas perguntas, a ata do ato, os códigos, a premissa e o estado recarregado. O gate consulta as perguntas ancoradas, competência, C6 e o destino válido. Quando autorizado, pode reapontar perguntas e gravar o novo estado do chamado. O `save` pode acionar S143, carimbar perguntas, fechar disputa e atingir o pai por sinal, além da relavra do veredito da pergunta quando o chamado cruza a fronteira de encerramento. `app/ponto/services/cartorio.py:1683`, `:1695`, `:1712`, `:1726`, `:1804`, `:1812`, `:1823`; `app/chamados/models.py:343`, `:361`, `:399`, `:444`, `:664`, `:685`, `:695`, `:302`, `:309`; `app/chamados/signals.py:19`, `:91`, `:149`; `app/chamados/services/disputa_emissao.py:1949`, `:2009`, `:2029`, `:2069`. |
| **Auto-rejeição de justificativa** | Após sucesso no fecho imediato por não protesto, lê as justificativas pendentes ligadas pela origem ou pelo ponteiro do chamado, exclui a que está em aprovação e consulta retenção. Grava `status` e `descricao` nas liberadas e o Cartório registra a ocorrência. O sinal de justificativa sai pelo `em_julgamento` nesse ato. `app/ponto/services/cartorio.py:1695`, `:1699`; `app/ponto/services/justificativa.py:279`, `:283`, `:337`, `:346`, `:353`; `app/ponto/signals.py:85`, `:161`. |
| **Ressurreição de perguntas** | Exige chamado vivo com perguntas, sem fio vivo e sem resposta do colaborador, fora do ramo só de duplicatas. A porta lê materialização, resposta, via, marco aceso e C6 sobre a pergunta como ficaria após a limpeza, usando a projeção da célula do ato. Relê a elegibilidade sob trava, registra a reabertura, limpa a projeção da pergunta e pode reabrir a disputa. `app/ponto/services/cartorio.py:1798`, `:1804`, `:1835`; `app/chamados/models.py:1542`, `:1558`, `:1567`, `:1576`, `:1627`, `:1700`, `:1710`. |
| **Emissão, espelho de marcos e repasse** | O corpo pode abrir ou reabrir cobrança se os códigos, o piso, a situação, o afastamento, o vínculo e `parcial_so_lista` autorizarem. Para furo parcial, também pode atualizar os marcos de chamado vivo. O espelho compara os marcos faltantes novos com o contexto gravado. O emissor seleciona órfãos regularizáveis do colaborador em `COBRANCA`. O repasse lê o dia, o juiz de falta, a ata e a identidade da pergunta, podendo inserir ou costurar perguntas e atualizar chamado e disputa. `app/ponto/services/cartorio.py:1889`, `:1909`, `:1919`, `:1927`, `:1953`, `:1975`, `:1990`, `:2003`; `app/ponto/management/commands/detectar_ausencias.py:554`, `:694`, `:738`, `:797`; `app/chamados/services/disputa_emissao.py:1214`, `:1223`, `:1251`, `:1258`, `:1268`, `:1284`, `:1292`. |

Para **a iteração da 129134**, a linha real tem cinco presenças zeradas e zero códigos. No ramo normal isso deixa de fora os fechos, a ressurreição, a emissão e o espelho **dessa iteração**. Com esses valores, a proposta de veredito do Cartório é `concorde`, sujeita à precedência do admin. A foto mudou após a etapa em que o corpo chama `carimbar`. O extrato não identifica qual componente da chave mudou nem o que uma escrita iniciada em outro dia do colaborador alcançou [nao verificado]. `cruza_r3.jsonl:14`, `:15`, `:16`; `medida.py:177`, `:180`; `app/ponto/services/cartorio.py:1628`, `:1662`, `:1726`, `:1739`, `:1909`, `:1975`, `:2003`, `:2036`; `app/ponto/portas/celula.py:978`; `app/escala/models.py:1624`.

O carimbo também pode inserir um evento de outbox quando grava ata ou veredito. O consumidor tem entrada própria. O comando medido chama `processar_cartorio` dentro de uma transação desfeita ao fim do braço, o que mantém os callbacks `on_commit` desse braço fora do intervalo observado. `app/ponto/portas/celula.py:941`, `:954`, `:961`; `app/chamados/management/commands/consumir_eventos_celula.py:26`; `app/chamados/signals.py:229`, `:240`, `:351`, `:371`; `medida.py:358`, `:478`, `:491`.

## 2. O que pode mudar entre rodadas

| Escritor | Dependência que pode mudar sem ato externo |
|---|---|
| **Lavra normal, carimbo e projeção em memória** | **Relógio:** `dias_encerrados` lê o `agora` do ato, o vínculo/DNA e os turnos pareados. A aberta pode expirar pelo relógio e o fim previsto pode ser atingido. Para dia passado em curso, a impressão inclui `enc:0`. Ao encerrar, essa parte sai. O mesmo resultado de encerramento vai ao classificador. **Estado anterior:** o pulo lê a impressão, o veredito e o carimbo persistidos. A impressão também lê batidas, cobertura, chamados e fio. Com a grade por turno habilitada, o pareador ainda pode ler atas persistidas do dia e da margem, inclusive escritas anteriores do próprio passe. **Instante da lavra:** `agora` vira o novo `julgada_em` se o carimbo escrever. A guarda não compara o `agora` novo com o timestamp antigo. `app/ponto/services/cartorio.py:1293`, `:1498`, `:1527`, `:1540`, `:1552`, `:1624`, `:502`, `:531`; `app/ponto/turnos.py:2015`, `:2124`, `:2157`, `:2182`; `app/escala/utils.py:1598`, `:1603`, `:2131`; `app/ponto/portas/celula.py:1189`, `:1194`. |
| **Ramo `nunca_bateu`** | Relê adesão, cobertura, DNA, grade e carimbo anterior. `turno_encerrado` entra na classificação após a guarda, mas esse ramo não passa `enc` à impressão. Uma alteração isolada dessa resposta não o tira do pulo pelo mesmo mecanismo do ramo normal. `app/ponto/services/cartorio.py:1345`, `:1370`, `:1393`, `:1399`, `:1406`, `:1415`. |
| **Fecho, relavra de pergunta, ressurreição e auto-rejeição** | Uma rodada pode deixar status, destino, materialização, via, respostas e pendências diferentes para a próxima. O gate lê o estado do banco, as ancoradas e C6, e a ressurreição lê a ata e o veredito em curso. `fio_vivo` e a leitura de C6 do gate não recebem `agora` explícito nesses pontos. C6 só recorta validação, resposta e materialização por instante quando recebe `agora`. A auto-rejeição deixa de selecionar a justificativa ao mudar `pendente` para `rejeitada`. `app/chamados/models.py:355`, `:376`, `:380`, `:685`, `:1542`, `:1567`; `app/chamados/juizes.py:1389`, `:1519`; `app/ponto/services/justificativa.py:283`, `:351`. |
| **Emissor, espelho e repasse** | `parcial_so_lista` lê o `julgada_em` tirado **antes** do ato e o `agora` da rodada. Procura fatos no intervalo, inclusive marco que venceu apagado. A rodada anterior pode avançar esse limite inferior e mudar os chamados, marcos e perguntas que o emissor ou o repasse reencontram. O `agora` também entra no cálculo de atraso e prazo do emissor. `app/ponto/services/cartorio.py:1118`, `:1919`, `:395`, `:402`, `:450`, `:467`; `app/ponto/management/commands/detectar_ausencias.py:555`, `:582`, `:594`, `:671`, `:797`; `app/chamados/services/disputa_emissao.py:1251`, `:1268`. |

Há também uma leitura de **lavra CxR** por chamada do comando. `ultimo_lavrado()` pega o snapshot mais recente por `data_ref` e `calculado_em`, sem recorte pelo `agora` congelado. O corpo usa seu conteúdo ao compor `ata.razoes_orfas`. Snapshot efetivamente lido em cada rodada, ordem efetiva do lote e participação das atas anteriores no delta da 129134 ficam [nao verificado]. `app/ponto/management/commands/processar_cartorio.py:61`, `:69`; `app/ponto/services/cxr_na_lista.py:152`, `:174`, `:182`; `app/ponto/services/cartorio.py:1093`, `:991`, `:1001`, `:2015`.

## 3. As 31 chamadas da fábrica

**Quem chama e com qual seletor.** `julgar_colab` separa `out['cods']` do lote no começo do ato. Após o corpo inicial e a eventual segunda passada, chama `perguntar_pela_celula` uma vez por célula de `cels` cuja **data** está no `cods` do ato. O ramo normal põe a data ali mesmo com `cods=[]`. Uma mudança de `enc:0` para ausência de `enc` pode assim produzir uma chamada na terceira rodada mesmo para uma célula sem protesto. Mudanças de estado que alterem os demais componentes da impressão, ou uma célula enfileirada para a segunda passada, também podem fazê-la entrar nesse seletor. A releitura pós-emissão vem depois do laço da fábrica. `app/ponto/services/cartorio.py:1108`, `:1152`, `:1170`, `:1179`, `:1222`, `:1540`, `:1627`.

**O que a fábrica seleciona depois de chamada.** Ela lê turno previsto, marcos vencidos em `agora` e ata lavrada. Para perguntar, aplica as guardas de trabalho, situação, data, veredito e `parcial_so_lista`. Subtrai os motivos já cobertos pela identidade do fato e, em paralelo, coleta carimbos elegíveis a descarimbo. Um marco que vence entre rodadas pode liberar um motivo. Uma pergunta escrita, retratada ou descarimbada pelo próprio processo pode mudar a deduplicação ou a lista de carimbos. O descarimbo ainda exige releitura sob trava e C6 favorável. Há saídas para Pauta com retorno `[]`. `app/chamados/services/fabrica_da_celula.py:45`, `:58`, `:105`, `:119`, `:139`, `:148`, `:201`, `:206`, `:214`; `app/ponto/services/cartorio.py:914`; `app/chamados/models.py:2933`; `app/chamados/services/relavra_via.py:181`, `:230`, `:242`.

**O que a contagem mostra.** O resumo registra 31 **invocações** do wrapper, zero de E2 e zero de repasse. O `n=1` de uma invocação é o comprimento da lista de motivos retornada. `n=0` ainda admite escrita interna, como descarimbo ou Pauta. As fotos `antes_fabrica` e `depois_fabrica` são do ato inteiro, então o par que contém a 129134 não identifica o PK entregue à fábrica. O dado também não identifica quais células ou predicados explicam a passagem de 5 para 31 [nao verificado]. `cruza_r3.jsonl:17`, `:18`, `:19`, `:34`, `:39`, `:40`, `:41`; `medida.py:117`, `:133`, `:221`, `:365`; `app/chamados/services/fabrica_da_celula.py:201`, `:208`, `:214`, `:220`.

## 4. Diferenças entre braços em `medida.py`

- **Relógio.** `encerrado` usa `06:27` nas três rodadas, `em_curso` usa `00:30` nas três, e `cruza` usa `00:30`, `00:30`, `06:27`. Todos em 20/11/2026, com fuso São Paulo. A sonda congela o ambiente a cada rodada, captura `self.agora` e, quando o wrapper de `julgar_colab` é alcançado, confere e repassa o instante. `medida.py:35`, `:37`, `:138`, `:341`, `:355`, `:510`.
- **Fixture e transação.** Os três braços de frota usam os mesmos nove PKs para observar e o mesmo comando para processar as empresas inteiras. As fixtures pertencem a `primeira_lavra` e `A1`, executados em envelopes próprios. Na frota, cada braço tem um `atomic` externo com rollback ao fim. As três rodadas do braço compartilham o estado transacional. `medida.py:34`, `:345`, `:350`, `:358`, `:375`, `:388`, `:420`, `:478`, `:491`, `:517`, `:521`; `app/ponto/management/commands/processar_cartorio.py:51`, `:65`.
- **Ordem.** A sonda percorre os braços na ordem declarada e as empresas sempre em `(2, 3, 4)`. O comando não fixa ordem entre colaboradores e ordena as células por data. As fotos da sonda usam `data, pk`. A ordem efetiva dos colaboradores na execução é [nao verificado]. `medida.py:36`, `:121`, `:357`, `:520`; `app/ponto/management/commands/processar_cartorio.py:51`, `:63`, `:65`.
- **Vínculo da instrumentação.** A sonda troca `cartorio.julgar_colab`, mas `processar_cartorio` importa `julgar_colab` no topo e chama o nome local. Se o comando já estava importado, o nome local conserva a função anterior ao patch. Se foi importado pela primeira vez sob o patch, conserva o método ligado àquela instância da sonda após a saída do braço. O texto anexado não mostra rebinding desse nome a cada braço. Como `entrada_passe` e `saida_passe` dependem desse wrapper, o vínculo efetivo que produziu as linhas rotuladas `cruza` fica [nao verificado]. `medida.py:133`, `:137`, `:316`, `:336`, `:478`, `:521`; `app/ponto/management/commands/processar_cartorio.py:20`, `:69`.

## 5. Classificação e linha de sonda que decide

- **O código prova o mecanismo de (b).** No ramo normal, o encerramento é uma entrada da impressão e do classificador. A passagem de em curso a encerrado já é suficiente para produzir outra impressão se os demais componentes forem os mesmos. A porta conserva o carimbo quando impressão, demais insumos e lavra coincidem. `app/ponto/services/cartorio.py:531`, `:1540`, `:1552`, `:1624`; `app/ponto/portas/celula.py:1075`, `:1189`.
- **O dado registra a mudança dessa entrada.** Na saída da rodada 2, `turno_encerrado=0`. Na entrada da 3 já vale `1`, com `mudou_foto=0`. A mudança na chave fotografada aparece depois do corpo inicial, com presenças e códigos ainda zerados, e o par em torno da fábrica tem `mudou_foto=0`. A chave só contém `impressao`, `veredito`, `julgada_em` e `ata`, além do PK. `celula_129134_3rodadas.jsonl:-8`; `cruza_r3.jsonl:13`, `:16`, `:17`, `:19`; `medida.py:180`, `:193`.
- **(c) para a classificação completa.** A comparação 2 → 3 tem entrada diferente e, portanto, não demonstra (a), que exige duas execuções com entradas iguais. Atribuir o delta exclusivamente ao relógio e afirmar que o passe convergiu, inclusive para as 31 chamadas, fica [nao verificado]. `medida.py:35`, `:39`, `:180`, `:193`; `app/ponto/services/cartorio.py:1540`, `:1552`, `:1179`.

As linhas de sonda que decidiriam:

1. **129134, rodadas 2 e 3, imediatamente antes do `if _em_dia and not forcar`.** Registrar `agora`, resultado e motivo do encerramento, sequência completa de componentes de `impressao_insumos`, impressão persistida, impressão calculada e `_em_dia`. Imediatamente **antes e depois do `carimbar` real**, registrar os valores persistidos de `impressao`, `veredito`, `julgada_em`, `ata` com as chaves alteradas, e a lista de campos retornada. Isso identifica tanto a entrada que tirou a célula do pulo quanto o campo que mudou na foto. Na entrada, registrar também se o `julgar_colab` que o comando vai chamar está ligado à instância da sonda daquele braço. `app/ponto/services/cartorio.py:502`, `:578`, `:1540`, `:1552`, `:2036`; `app/ponto/turnos.py:2182`; `app/ponto/portas/celula.py:1202`; `app/ponto/management/commands/processar_cartorio.py:69`.
2. **Fábrica, rodadas 2 e 3, imediatamente antes de testar `_cel.data in _julgadas`.** Registrar PK, data, passagem que pôs a data em `cods`, resultado do teste, impressão persistida e calculada nessa passagem e `enc`. Para as chamadas selecionadas, registrar `julgada_antes`, vencidos, motivos pela ata, motivos já cobertos e PKs de `carimbadas` **antes** das portas, e os campos de pergunta/disputa efetivamente gravados e os motivos retornados **depois**. Assim a linha atribui o salto de chamadas à seleção do Cartório e identifica separadamente a escrita que cada chamada produziu. `app/ponto/services/cartorio.py:1118`, `:1156`, `:1179`, `:1627`; `app/chamados/services/fabrica_da_celula.py:145`, `:154`, `:206`, `:220`.
3. **Convergência, saída da rodada 3 e entrada/saída de uma repetição a `06:27`.** Para a mesma célula e as demais chamadas identificadas, repetir a comparação de entradas, campos persistidos e retorno das portas com o relógio fixo. A persistência de uma mudança com as mesmas entradas demonstraria (a) no caso observado. A estabilidade nessa repetição, junto de uma diferença de entrada identificada no salto anterior, sustentaria (b) para o alcance medido. `medida.py:137`, `:145`, `:180`; `app/ponto/services/cartorio.py:1552`, `:1179`; `app/ponto/portas/celula.py:1189`.

## 20/11 19:49 · sessao 330e9ae12241 · - · - · origem=Ronald (tmux)

Dos 39 registros de receivers nos quatro apps, 27 alcançam escritores no fluxo com estado coerente e 12 ficam fora. Dos 12 registros de `on_commit`, sete alcançam e cinco ficam fora pelo mesmo critério. Os dois callbacks de isentos podem alcançar o cartório se a instância do chamado estiver desatualizada na hora do save. Nesse cenário, os totais passam a 29/10 para receivers e 9/3 para callbacks. Incidência em produção [nao verificado]. `app/chamados/services/silencio_isentos.py:24–47` `app/chamados/services/arquivo.py:165–173` `app/chamados/models.py:488–499` `app/chamados/signals.py:180–218,270–308`

Há um consumidor de fila persistente, `consumir_eventos_celula`, e um dreno explícito de callbacks para o backfill, executado dentro do savepoint. O cartório também drena duas filas locais síncronas dentro de `julgar_colab`. `app/chamados/reconciliador.py:1366–1459` `app/chamados/services/foto_e4b.py:30–84` `app/chamados/management/commands/backfill_fio_morto_e4b.py:294–335` `app/ponto/services/cartorio.py:1019–1049`

O mapa por registro, os terminais alcançados e o critério de reprodução foram enviados à raiz.

## 20/11 19:48 · sessao db239e2c8fd6 · - · - · origem=Ronald (tmux)

O censo de persistência da célula e da projeção `FuroDiario` encontrou **13 sítios ORM** em `app/ponto/portas/celula.py`, além dos caminhos de disparo descritos abaixo. A referência de produção que identifica `FuroDiario` como projeção está em `app/ponto/portas/celula.py:7–21,1241–1269`. As cinco presenças da ficha são calculadas ao ler chamados, protestos e fio. `app/ponto/services/cartorio.py:1413–1433,1493–1503,1538–1539`

| Sítio ORM | Função, origem e cobertura A |
|---|---|
| `app/ponto/portas/celula.py:265` | `regenerar_celulas_vinculo`, recolhe julgamento/DNA de futura sem vínculo. Externo, pode ser por signal, ficha não diz. `app/escala/signals.py:34–83,86–134` `r.txt:580,585` |
| `app/ponto/portas/celula.py:279` | Mesma função, `trabalha`/DNA de dia fora da vigência até hoje. Externo, pode ser por signal, ficha não diz. `app/escala/signals.py:73` `r.txt:580,585` |
| `app/ponto/portas/celula.py:293` | Mesma função, INSERT. Externo, pode ser por signal. Cron de geração também entra na porta. Ficha não diz. `app/ponto/management/commands/gerar_celulas.py:323,347` `app/config/crons.py:426` `r.txt:580,585` |
| `app/ponto/portas/celula.py:310` | Mesma função, regrava DNA/geradora. Externo, pode ser por signal. Ficha não diz. `app/escala/signals.py:125` `r.txt:580,585` |
| `app/ponto/portas/celula.py:638` | `editar_dia`, INSERT humano. `trocar_plantao` chama à linha 713. Entrada concreta além da porta `[nao verificado]`. Ficha não diz. `r.txt:580,585` |
| `app/ponto/portas/celula.py:647` | `editar_dia`, UPDATE humano. Mesma entrada e cobertura da linha anterior. `app/ponto/portas/celula.py:713` `r.txt:580,585` |
| `app/ponto/portas/celula.py:836` | `reverter_regeneracao_dia`, fora, command `aplicar_col900`. A reversão nessa linha é síncrona, a fotografia está em callback próprio. Ficha não diz. `app/ponto/management/commands/aplicar_col900.py:186,198–201` `r.txt:580,585` |
| `app/ponto/portas/celula.py:914` | `gerar_celulas_periodo`, INSERT, comando do cron e comando corretivo. Fora, sem signal ou callback no caminho direto. Ficha não diz. `app/config/crons.py:426` `app/ponto/management/commands/gerar_celulas.py:349` `app/ponto/management/commands/curar_residuo_bug115.py:138` `r.txt:580,585` |
| `app/ponto/portas/celula.py:926` | Mesma função e entradas, UPDATE de DNA. Ficha não diz. `app/ponto/management/commands/gerar_celulas.py:349` `app/ponto/management/commands/curar_residuo_bug115.py:138` `r.txt:580,585` |
| `app/ponto/portas/celula.py:963` | `salvar_e_emitir`. Lavra normal coberta via `carimbar` em `cartorio.py:1343,1921`. Ficha não diz sobre completar status D5 (`cartorio.py:953,1291,1440`), precedência gravada depois de `julgar_colab` (`cartorio.py:2048,2088–2096`), lavra humana (`ponto/views.py:3494` → `ponto/services/veredito_celula.py:352`) e desfazer julgamento futuro (`ponto/portas/celula.py:267,314,842 → :1167`). Existem caminhos síncronos, por signal e sob callback. `app/ponto/signals.py:93,131–165,254–272` `app/chamados/signals.py:218` `r.txt:580,585` |
| `app/ponto/portas/celula.py:1237` | `carimbar_insumos_em_lote`, command `backfill_b5_insumos --apply`, fora e síncrono. Ficha não diz. `app/ponto/management/commands/backfill_b5_insumos.py:58,81,84` `r.txt:580,585` |
| `app/ponto/portas/celula.py:1258` | `gravar_furos`, UPSERT `FuroDiario`. Fora do ato do cartório: cron encadeado e pós-vínculo/`resolver_dia`. Ficha não diz. `app/config/crons.py:1527` `app/escala/management/commands/apurar_furos_diarios.py:13` `app/escala/services/furos_diarios.py:108` `app/colaboradores/services/pos_vinculo.py:85` `app/ponto/views.py:3505` `r.txt:580,585` |
| `app/ponto/portas/celula.py:1266` | `gravar_furos`, DELETE `FuroDiario`, mesmas entradas. Ficha não diz. `app/escala/services/furos_diarios.py:108` `app/colaboradores/services/pos_vinculo.py:85` `r.txt:580,585` |

Para reproduzir, rode a varredura sem importar Django e filtre as duas entidades. Audite o balde sem atribuição e acrescente `cel.save()` em `app/ponto/portas/celula.py:963`. O scanner reconhece dez sítios `CelulaDia` e dois `FuroDiario`, com o décimo primeiro da célula recuperado nessa auditoria. A fila `EventoCelula` tem quatro sítios próprios, de criação, confirmação, falha e retenção. `app/core/censo_escritas.py:43–82` `app/ponto/portas/celula.py:954,963` `app/chamados/reconciliador.py:1446,1452,1459`

```sh
PYTHONPATH=app python3 -B -c 'from core.censo_escritas import varrer_arvore; print(*(x for x in varrer_arvore("app") if x["entidade"] in {"CelulaDia", "FuroDiario", "EventoCelula", None}), sep=chr(10))'
```

Para reproduzir a expansão dos caminhos, busque referências por nome e alias de importação para cada função, mantenha comandos e `bin/`, filtre testes/migrações, siga ligações de `post_save` até o emissor e confirme as duas fases de callbacks. A lavra em lote entra pelo cron `processar_cartorio` ou seus irmãos `celula_veredito_velho` e `lavrar_status_competencia`, pelo comando de régua de defesa e pelo script pontual. A lavra unitária entra pelos cinco receivers de batida/ausência/justificativa, pelo receiver de chamado, por pós-vínculo, reemissão c2, recalcular por mudança do chão, cobrar dia, vigia de hora e comandos corretivos. `app/config/crons.py:367,616` `app/ponto/management/commands/processar_cartorio.py:65` `app/ponto/management/commands/celula_veredito_velho.py:144` `app/ponto/management/commands/lavrar_status_competencia.py:56` `app/escala/regua_defesa.py:695,728` `bin/rejulgar_o118_col221.py:24` `app/ponto/signals.py:93,131–165` `app/chamados/signals.py:218` `app/colaboradores/services/pos_vinculo.py:73` `app/chamados/reconciliador.py:752,970` `app/ponto/portas/celula.py:472` `app/chamados/services/cobrar_dia.py:41` `app/ponto/services/vigia_de_hora.py:134` `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:78` `app/ponto/management/commands/curar_residuo_bug115.py:146` `app/ponto/services/furos_sem_cobranca.py:128`

Dois caminhos síncronos do próprio cartório que a ficha não nomeia são o cluster, que pode abrir ou fechar chamados do colaborador fora do dia selecionado, e a ressuscitação do fio de um chamado que já estava vivo. `app/ponto/services/cartorio.py:1063–1073,1690–1722` `app/ponto/services/cluster_espurio.py:176–213` `r.txt:580–583`

## 20/11 19:47 · sessao e1900a3097a0 · - · - · origem=Ronald (tmux)

| Ato iniciado fora do cartório | Entrada e caminho até a escrita | Cobertura textual |
|---|---|---|
| E2 por batida | `app/ponto/signals.py:206–215` → `app/chamados/reconciliador.py:1483` → carimbo `:645` e fecho da disputa `:662`. Receiver síncrono. | A ficha descreve o E2 síncrono do cartório e deixa em aberto a origem externa e o evento. `r.txt:581,585` |
| E2 pela morte do filho | `app/chamados/signals.py:351–371` → `app/chamados/reconciliador.py:1483` → `:645,:662`. Receiver com `on_commit`. | A ficha não diz. `r.txt:585` |
| E2 e fábrica pelo outbox | `app/config/crons.py:394` → `app/chamados/management/commands/consumir_eventos_celula.py:31` → E2 `app/chamados/reconciliador.py:1416` e fábrica `:1438` → `app/chamados/services/fabrica_da_celula.py:301`. | A ficha não diz. `r.txt:585` |
| Fábrica pelo backfill | `app/chamados/management/commands/backfill_fio_morto_e4b.py:203,316` → `:_executar_dia` em `:125`, fábrica em `:143`. | A ficha não diz. `r.txt:585` |
| Abertura, reabertura e espelho do emissor | `app/config/crons.py:364` → `app/ponto/management/commands/detectar_ausencias.py:61,440`, ou `app/ponto/management/commands/emitir_furo_retroativo.py:154` → abrir `detectar_ausencias.py:694`, espelho `:745` → `app/chamados/services/contexto.py:104`. | Verificação no emissor prevista. Cobertura das entradas autônomas ficou aberta. `r.txt:582,585` |
| Repasse pelos detectores | `app/ponto/management/commands/detectar_ausencias.py:803`, `processar_alertas_turno.py:385` e `processar_alertas_avancados.py:159`, com crons declarados em `app/config/crons.py:362–364`. | A guarda do INSERT foi expressamente limitada ao escopo cercado do cartório. `r.txt:582,585` |
| Repasse por iniciar órfãos | `app/chamados/urls.py:21,39` → `app/chamados/views.py:674,954` → `app/chamados/services/fio.py:598` → `app/chamados/services/acoes_disputa.py:226`. | A guarda do INSERT foi expressamente limitada ao escopo cercado do cartório. `r.txt:582,585` |
| Repasse por cobrar dia | `app/chamados/urls.py:32` → `app/chamados/views_cobrar.py:327` → `app/chamados/services/cobrar_dia.py:49`, depois de retornar de `cartorio.julgar_celula` em `:41`. | A guarda do INSERT foi expressamente limitada ao escopo cercado do cartório. `r.txt:582,585` |
| Repasse pelo comando avulso | `app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:19–29` → `app/chamados/services/pergunta_no_app.py:261`. | A guarda do INSERT foi expressamente limitada ao escopo cercado do cartório. `r.txt:582,585` |
| Criação ou agregação de disputa pela batida e foto | API `app/api/urls.py:67` → `app/api/views.py:768`, SaaS `app/api/urls_core.py:14` → `app/api/views_core.py:762`, browser `app/ponto/urls.py:11–12` → `app/ponto/views.py:352` → `app/ponto/services/triagem_batida.py:734`, cron `app/config/crons.py:686` → `app/ponto/management/commands/marcar_foto_ausente_retro.py:96`. Porta comum `app/chamados/services/disputa_emissao.py:676–699`. | A ficha não diz para essas origens. `r.txt:582,585` |

**10 linhas de entradas externas agrupadas.** A busca inicial usa `rg -n` em `.py` de `app` e `bin`, com as funções `retratar_perguntas_absolvidas_por_celula`, `perguntar_pela_celula`, `criar_disputa_a_partir_de_chamados_orfaos`, `criar_ou_atualizar_disputa` e `_abrir_chamado`. Segue cada alias ao `handle`, view, receiver ou consumidor e conta os comandos que chegam à chamada efetiva. `app/chamados/reconciliador.py:1414–1438` `app/chamados/services/disputa_emissao.py:648–702,1041–1066` `app/ponto/management/commands/detectar_ausencias.py:61,440`

O cartório também abre e fecha chamado de cluster pela chamada em `app/ponto/services/cartorio.py:1066–1070`, com as duas mutações em `app/ponto/services/cluster_espurio.py:190,196`. O caminho avulso grava precedência na ata depois do retorno de `julgar_colab`. A ficha não diz sobre esses ramos. `app/ponto/services/cartorio.py:2048–2055,2088–2096` `r.txt:580–585`

O censo AST existente, `PYTHONPATH=app python3 -B` e `core.censo_escritas.varrer_arvore('app')`, fornece candidatos com 21 ocorrências de `PerguntaDisputa`, 19 de `DisputaSupervisao`, 16 de `ChamadoColaborador`, 10 de `CelulaDia` e 1 de `DecisaoPergunta`. O método usa atribuição heurística e deixa fora, por exemplo, os nascimentos via `cls.objects` e o `p.save(force_insert=True)` desta árvore. `app/core/censo_escritas.py:6–56,469–492` `app/chamados/models.py:1030,1154,3209`

## 20/11 19:46 · sessao 15c545c76fcc · E4C-F5-CONSTROI · construtor · origem=Code

**PAREI pelo item (78).** O selo enumerou **5 sítios e acusou 1**. A acusação é ruído do visitante, e o teste ficou vermelho conforme o aval. A catraca (06) examinou **2 chamadas com `update_fields`**, encontrou **1** com um dos campos vigiados e fez **0 acusações**. A medição foi feita sobre o AST da cópia, com os mutantes aplicados em memória. [Selo do par:505](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:505) · [Catraca:306](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_catraca.py:306) · [Tabela de mutantes:48](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/e4c_f5_voltaE_mutantes.py:48) [nao verificado]

### Lista completa da parada

| Sítio | Acusação | Classificação |
|---|---|---|
| [`chamados/models.py:1517`](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:1517), `ChamadoColaborador.reabrir` | `REABERTURA` sem UPDATE pareado de `validada_em` | **Ruído.** O visitante atribui o ato ao chamador ao seguir `self.ressuscitar_perguntas()`, mas procura o UPDATE no escopo do chamador. A função chamada registra a `REABERTURA` e passa `validada_em=None` à porta do lote no mesmo `atomic`. [Visitante:281](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:281) · [Ato:1719](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:1719) · [UPDATE:1721](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:1721) |

### Arquivos tocados e alcance

- **(78) [`test_e4c_f5_selo_par_ato_coluna.py:452`](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:452).** Antes, o censo partia das escritas da coluna. O selo vizinho partia do ato apenas para `VALIDACAO`. Agora, o censo adicional parte de `RECUSA`, `REABERTURA` e `DEVOLUCAO` e procura a coluna no `save(update_fields=...)` ou no `.update(...)` da mesma pergunta, inclusive pela porta de lote. Os atos vêm das constantes do modelo e o nome da coluna vem do campo do modelo. Os sintéticos passaram para as duas formas, incluindo a rejeição de um UPDATE em outra pergunta. [Selo vizinho:12](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_volta3_selos.py:12) · [Campo e atos:22](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:22) · [Censo novo:461](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:461) · [Sintéticos:515](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:515)
- **(06) [`test_e4c_f5_selo_catraca.py:182`](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_catraca.py:182).** Antes, a catraca varria atributos crus, `getattr`, `refresh_from_db` e projeções `only`/`values*` usadas em decisões. Agora, também resolve strings em `update_fields`, inclusive em lista, tupla ou conjunto literal ligado a um nome local. O aval das 19h45, item 1, especifica o critério de (06) do aval das 18h45. A acusação exige ausência de atribuição ao mesmo campo no mesmo receptor na função. O controle original passou com o texto e o esperado intactos. Os sintéticos passaram com atribuição correspondente e acusaram ausência de atribuição, outro campo e outro receptor. Na cópia, `validada_em` aparece no `save` de `devolver_validacao_recusada`, que atribui o campo ao mesmo `perg`. A outra chamada grava `recusa_motivo`. [Projeções:118](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_catraca.py:118) · [Resolução de textos:83](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_catraca.py:83) · [Controle:369](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_catraca.py:369) · [Sintéticos:380](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_catraca.py:380) · [Devolução:287](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/validacao.py:287) · [Recusa:940](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/validacao.py:940) [nao verificado]
- **[`e4c_f5_voltaE_mutantes.py:26`](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/e4c_f5_voltaE_mutantes.py:26).** Tabela dos dois mutantes novos, com aplicação sobre o texto em memória. [Aplicação:48](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/e4c_f5_voltaE_mutantes.py:48)

### Mutantes e comandos por label

- **`d_paridade_devolucao_sem_coluna`** remove toda menção a `validada_em` de `devolver_paridade_bug_par_103`, incluindo a atribuição, o campo do `save`, as fotos e a docstring. Preserva o ato `DEVOLUCAO`. É distinto de `d_devolucao_mantem_coluna`, que troca apenas o `save` de `devolver_validacao_recusada` por `pass`. A execução isolada acrescentou uma falha no subteste da devolução da paridade, na linha **512**, além do ruído já presente na base. [Alvo:324](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/validacao.py:324) · [Trocas:26](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/e4c_f5_voltaE_mutantes.py:26) · [Mutante anterior:61](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/e4c_f5_volta3_mutantes.py:61) · [Asserção:512](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:512)
- **`c_recusa_update_fields_cru`** acrescenta `respondida_em` ao `save(update_fields=...)` de `recusar_pergunta` sem atribuição correspondente. A execução isolada acusou `update_fields(respondida_em)` na linha **316** da catraca. [Alvo:940](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/validacao.py:940) · [Troca:40](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/e4c_f5_voltaE_mutantes.py:40) · [Asserção:316](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_catraca.py:316)

```bash
PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings_test \
  python3 manage.py test \
  chamados.tests.test_e4c_f5_selo_par_ato_coluna \
  chamados.tests.test_e4c_f5_selo_catraca --noinput

PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings_test \
  F5_VOLTA_E_MUTANTE=d_paridade_devolucao_sem_coluna \
  python3 manage.py test \
  chamados.tests.test_e4c_f5_selo_par_ato_coluna.SeloParAtoColunaTest.test_atos_que_desfazem_gravam_a_coluna --noinput

PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings_test \
  F5_VOLTA_E_MUTANTE=c_recusa_update_fields_cru \
  python3 manage.py test \
  chamados.tests.test_e4c_f5_selo_catraca.SeloCatracaTest.test_producao_sem_leituras_cruas --noinput
```

**Limites.** A catraca compara a expressão AST do receptor. Equivalência entre nomes locais distintos e `update_fields` passado por `**kwargs` ficaram fora da cobertura nova. A execução acima pelo Django, a suíte, os testes com banco e o comportamento em produção ficam **[nao verificado]**. A certificação cabe ao Code e ao opus final. [Comparação de receptores:271](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_catraca.py:271) · [Leitura da palavra-chave:193](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_catraca.py:193) [nao verificado]

## 20/11 19:21 · sessao 282f814c9fa2 · E4C-F5-CONSTROI · sonda · origem=Code

```python
# Uso na sombra com bin/sombra.sh --rodar sonda.py.
# SOMBRA_ARVORE seleciona o app da outra copia.
# bin/sombra.sh:179, bin/sombra.sh:182, bin/sombra.sh:483
#
# O --rodar chama lavra_de_prod antes do shell Python. Essa etapa le a
# producao, grava arquivos auxiliares e atualiza a sombra fora do rollback
# desta sonda. O lancador tambem grava a saida com tee.
# bin/sombra.sh:136, bin/sombra.sh:151, bin/sombra.sh:157, bin/sombra.sh:483
#
# Nomes do anexo ausentes no main, resolvidos individualmente por getattr.
# - PerguntaDisputa.RECUSA_FORA_DO_FATO. O main tem VIAS_FORA_DO_FATO e
#   carimbar devolve False nesse caso. A sonda registra a falta do nome.
#   app/chamados/models.py:2701, app/chamados/models.py:3316
# - PerguntaDisputa.RECUSA_C6. O main devolve False no ramo do C6.
#   A sonda registra a falta do nome. app/chamados/models.py:3334
# - PerguntaDisputa.RECUSA_PAR_ATO_COLUNA. O main grava o ato da irma
#   pelo ramo de sucesso e emite a recusa sem motivo. A sonda registra
#   a falta do nome. app/chamados/services/materializacao.py:1055
#
# A trilha do main traz escape_pk, ausencia_pk, via_tentada e data_evento.
# Quando falta motivo, a sonda guarda os PKs observados e marca a
# classificacao INCONCLUSIVO. A rejeicao segue o resultado da passagem.
# app/chamados/services/materializacao.py:1065
#
# A prova foto 3 == foto 1 usa hash integral e numero de linhas.
# A saida mostra 12 caracteres do hash. O delta usa multiconjuntos
# por matricula e contabiliza duplicatas. A ordem fica refletida no hash.
# O registro 11 pertence ao ultimo 10, cuja matricula ocupa [2:12].
# app/folha/export.py:34, app/folha/export.py:215, app/folha/export.py:246,
# app/folha/export.py:525, app/folha/export.py:654
#
# Rejeicao pela porta real, seguida apenas do ramo de devolucao que o
# anexo executava dentro do rollback. app/ponto/services/ausencia.py:1057,
# app/ponto/signals.py:254, app/ponto/signals.py:272
#
# Execucao no banco da sombra e compatibilidade de runtime com c8eb8eddf
# [nao verificado].

import hashlib
import io
import json
import logging
import socket
import sys
from collections import Counter, defaultdict
from contextlib import ExitStack, closing, redirect_stderr, redirect_stdout
from unittest.mock import patch

from django.conf import settings
from django.contrib.auth import get_user_model
from django.core.files.storage import FileSystemStorage
from django.db import DatabaseError, connection, transaction
from django.db.models import F, Q, TextField
from django.db.models.functions import Cast
from django.urls import reverse
from django.utils import timezone
from rest_framework.test import APIRequestFactory, force_authenticate

from api.views import api_disputa_responder
from chamados.models import PerguntaDisputa
from chamados.services import disputa_emissao, materializacao as M
from colaboradores.mixins import tem_acao
from core import observ
from folha import export as E
from folha.models import ExportacaoDominio
from ponto.models import Ausencia, Batida, FechamentoMensal
from ponto.services import ausencia as A


TETO = 8
FOTOS = ("antes", "depois_passagem", "depois_rejeicao")
NOMES_DE_RECUSA = ("RECUSA_FORA_DO_FATO", "RECUSA_C6", "RECUSA_PAR_ATO_COLUNA")
REVERSOES = {"par_ato_coluna_divergente", "sem_grupo_recuperavel"}
MOTIVOS_COMUNS = REVERSOES | {
    "fora_da_releitura", "resposta_posterior_a_passagem", "ja_carimbada",
}

# app/api/views.py:2603, app/api/views.py:2631,
# app/chamados/services/disputa_emissao.py:1439.
# A mensagem de absorcao vigente vem do anexo para c8eb8eddf [nao verificado].
RECUSAS = {
    (404, "Colaborador nao encontrado."): "sem_vinculo",
    (404, "Pergunta nao encontrada ou ja regularizada."): "nao_encontrada_ou_nao_viva",
    (400, "Resposta obrigatoria."): "resposta_vazia",
    (400, "Selecione/preencha uma resposta."): "resposta_vazia",
    (400, "Opcao invalida."): "opcao_invalida",
    (400, "Hora deve ser HH:MM."): "hora_invalida",
    (400, "A pergunta já foi resolvida pela ausência declarada."): "absorcao_vigente",
}


class Descartar(io.TextIOBase):
    def write(self, texto):
        return len(texto)


class GuardaSombra(RuntimeError):
    pass


class GuardaDeSequencias(RuntimeError):
    pass


class SemUsuarioAtivo(RuntimeError):
    pass


class SemDecisorDP(RuntimeError):
    pass


class EvidenciaInesperada(RuntimeError):
    pass


class LayoutDeExportInesperado(RuntimeError):
    pass


class RedeBloqueada(OSError):
    pass


class EscritaEmArquivoBloqueada(OSError):
    pass


def resolver_recusas(ausentes):
    motivos = set(MOTIVOS_COMUNS)
    for nome in NOMES_DE_RECUSA:
        valor = getattr(PerguntaDisputa, nome, None)
        if valor is None:
            ausentes.add(f"PerguntaDisputa.{nome}")
        else:
            motivos.add(valor)
    return motivos


def guardar_sequencias(cursor):
    # A sombra usa PostgreSQL 17. As portas podem inserir linhas com PK
    # gerado. A sonda usa RESTART dentro do atomic externo para isolar
    # os avanços de sequencia, preservando o proximo valor inicial.
    # bin/sombra_db.sh:45, app/config/settings/base.py:104,
    # app/ponto/services/ausencia.py:560, app/chamados/models.py:4338.
    # Permissao e comportamento das sequencias instaladas [nao verificado].
    try:
        cursor.execute("""
            DO $sonda$
            DECLARE
                s record;
                qualificada text;
                ultimo bigint;
                usada boolean;
                proximo numeric;
            BEGIN
                FOR s IN
                    SELECT n.nspname, c.relname, p.seqcache, p.seqincrement,
                           p.seqmin, p.seqmax, p.seqcycle
                    FROM pg_catalog.pg_sequence p
                    JOIN pg_catalog.pg_class c ON c.oid = p.seqrelid
                    JOIN pg_catalog.pg_namespace n ON n.oid = c.relnamespace
                    WHERE n.nspname IN ('juliani', 'public')
                    ORDER BY n.nspname, c.relname
                LOOP
                    qualificada := format('%I.%I', s.nspname, s.relname);
                    EXECUTE format('ALTER SEQUENCE %s CACHE %s', qualificada, s.seqcache);
                    EXECUTE 'SELECT last_value, is_called FROM ' || qualificada
                        INTO ultimo, usada;
                    proximo := ultimo::numeric;
                    IF usada THEN
                        proximo := proximo + s.seqincrement;
                    END IF;
                    IF proximo NOT BETWEEN s.seqmin AND s.seqmax THEN
                        IF NOT s.seqcycle THEN
                            CONTINUE;
                        END IF;
                        proximo := CASE WHEN s.seqincrement > 0
                                       THEN s.seqmin ELSE s.seqmax END;
                    END IF;
                    EXECUTE format('ALTER SEQUENCE %s RESTART WITH %s', qualificada, proximo);
                END LOOP;
            END;
            $sonda$;
        """)
    except DatabaseError as erro:
        raise GuardaDeSequencias() from erro


def sha(valor):
    texto = json.dumps(valor, sort_keys=True, ensure_ascii=True, separators=(",", ":"))
    return hashlib.sha256(texto.encode("ascii")).hexdigest()


def assinatura(campos):
    por_campo = {nome: sha(valor) for nome, valor in campos.items()}
    return sha(por_campo), por_campo


def fotografar_linhas(qs):
    campos = [f for f in qs.model._meta.concrete_fields if not f.primary_key]
    nomes = [f.name for f in campos]
    colunas = [Cast(F(f.attname), TextField()) for f in campos]
    return {
        pk: assinatura(dict(zip(nomes, valores)))
        for pk, *valores in qs.order_by("pk").values_list("pk", *colunas)
    }


def foto(colab, dia, mes, ano, montar):
    empresa = colab.empresa
    vigente = E.competencia_entregue(empresa.pk, mes, ano, registro=True)
    montagens = []

    def montar_observada(*args, **kwargs):
        resultado = montar(*args, **kwargs)
        linhas, _ = resultado
        montagens.append(tuple(linhas) if isinstance(linhas, (list, tuple)) else None)
        return resultado

    # Captura as linhas da propria chamada que alimenta o comparador.
    # app/folha/export.py:631, app/folha/export.py:654, app/folha/export.py:690
    if montar is None:
        txt = E.txt_vigente_x_gravado(empresa, mes, ano)
    else:
        with patch.object(E, "montar_linhas_export", montar_observada):
            txt = E.txt_vigente_x_gravado(empresa, mes, ano)

    par_txt = {chave: txt[chave] for chave in ("hash_hoje", "linhas_hoje")}
    linhas = montagens[0] if len(montagens) == 1 else None
    if linhas is not None and len(linhas) != par_txt["linhas_hoje"]:
        linhas = None

    criterio_dia = (
        Q(data=dia)
        | Q(data__lte=dia, data_fim__gte=dia)
        | Q(data__lte=dia, data_fim__isnull=True, tipo__in=Ausencia.TIPOS_EM_ABERTO)
    )
    fechamentos = FechamentoMensal.objects.filter(
        colaborador_id=colab.pk, mes=mes, ano=ano,
    )
    modelos = {
        "FechamentoMensal": fotografar_linhas(fechamentos),
        "Batida": fotografar_linhas(Batida.objects.filter(
            colaborador_id=colab.pk, timestamp__date=dia,
        )),
        "Ausencia": fotografar_linhas(Ausencia.objects.filter(
            criterio_dia, colaborador_id=colab.pk,
        )),
        "ExportacaoDominio": fotografar_linhas(ExportacaoDominio.objects.filter(
            pk=vigente.pk if vigente is not None else None,
        )),
        "TXT_do_gravado": {empresa.pk: assinatura(par_txt)},
    }
    return {
        "modelos": modelos,
        "par_txt": par_txt,
        "linhas": linhas,
        "publica": {
            "hash_hoje": par_txt["hash_hoje"][:12],
            "linhas_hoje": par_txt["linhas_hoje"],
            "divergente": txt["divergente"],
            # app/ponto/models.py:398, app/ponto/models.py:481
            "fechamento_mensal_status": fechamentos.order_by("pk").values_list(
                "status", flat=True,
            ).first(),
        },
    }


def diferencas(antes, depois):
    resultado = {}
    for modelo, a in antes["modelos"].items():
        b = depois["modelos"][modelo]
        novas = sorted(b.keys() - a.keys())
        removidas = sorted(a.keys() - b.keys())
        mudadas = [
            {"pk": pk, "campos": sorted(
                nome for nome in a[pk][1] if a[pk][1][nome] != b[pk][1][nome]
            )}
            for pk in sorted(a.keys() & b.keys()) if a[pk][0] != b[pk][0]
        ]
        resultado[modelo] = {
            "novas": {"n": len(novas), "pks": novas},
            "mudadas": {"n": len(mudadas), "linhas": mudadas},
            "removidas": {"n": len(removidas), "pks": removidas},
        }
    return resultado


def linhas_por_matricula(linhas):
    contagem = Counter()
    matricula = None
    for linha in linhas:
        registro = linha[:2]
        if registro == "10" and len(linha) == 43:
            matricula = linha[2:12]
        elif registro != "11" or len(linha) != 11 or matricula is None:
            raise LayoutDeExportInesperado()
        contagem[(registro, matricula, linha)] += 1
    return contagem


def diferencas_txt(antes, depois, matricula, status_etapa, montar):
    if status_etapa == "NAO_EXERCITADA":
        return {"status": "NAO_EXERCITADA"}
    if montar is None:
        return {"status": "INCONCLUSIVO", "motivo": "folha.export.montar_linhas_export_ausente"}
    if antes is None or depois is None:
        return {"status": "INCONCLUSIVO", "motivo": "FotoIndisponivel"}
    if antes["linhas"] is None or depois["linhas"] is None:
        return {"status": "INCONCLUSIVO", "motivo": "MontagemDoComparadorIndisponivel"}

    try:
        a = linhas_por_matricula(antes["linhas"])
        b = linhas_por_matricula(depois["linhas"])
    except Exception as erro:
        return {"status": "INCONCLUSIVO", "motivo": type(erro).__name__}

    por_tipo = {reg: {"adicionadas": 0, "removidas": 0} for reg in ("10", "11")}
    afetadas = set()
    for sentido, mudancas in (("adicionadas", b - a), ("removidas", a - b)):
        for (registro, mat, _), n in mudancas.items():
            por_tipo[registro][sentido] += n
            afetadas.add(mat)

    return {
        "status": "CONCLUSIVO",
        "por_tipo": por_tipo,
        "matriculas_distintas_afetadas": len(afetadas),
        "matricula_do_caso_afetada": matricula in afetadas,
    }


def censo(escapes):
    # Mesmo teto, filtros e ordem do anexo. O leitor da competencia esta em
    # app/chamados/services/materializacao.py:788 e os escapes em :1095.
    por_escape = dict.fromkeys(escapes, 0)
    selecionadas = {escape: [] for escape in escapes}
    universo = sem_dia = 0
    memo = {}
    qs = (
        PerguntaDisputa.objects.filter(tipo_resposta__in=("hora", "escolha"))
        .select_related("disputa__colaborador__empresa")
        .only("tipo_resposta", "opcoes", "data_evento",
              "disputa__colaborador__empresa__dia_inicio_competencia")
        .order_by(F("materializada_em").asc(nulls_first=True), "-data_evento", "pk")
    )
    with closing(qs.iterator(chunk_size=2000)) as perguntas:
        for p in perguntas:
            opcoes = p.opcoes or []
            valores = [o.get("valor") if isinstance(o, dict) else o for o in opcoes]
            oferecidos = [e for e in escapes if e in valores]
            if not oferecidos:
                continue
            empresa = p.disputa.colaborador.empresa
            chave = (empresa.pk, p.data_evento)
            if chave not in memo:
                memo[chave] = M.competencia_do_fato(p, empresa)
            comp = memo[chave]
            if comp is None:
                sem_dia += 1
                continue
            mes, ano, entregue = comp
            if not entregue:
                continue
            universo += 1
            for escape in oferecidos:
                por_escape[escape] += 1
                if len(selecionadas[escape]) < TETO:
                    selecionadas[escape].append((p.pk, escape, mes, ano))
    casos = [caso for escape in escapes for caso in selecionadas[escape]]
    return universo, sem_dia, por_escape, casos


def tipos_de_falha(falhas):
    contagem = Counter(x for x in falhas if isinstance(x, str))
    excecoes = {id(x): x for x in falhas if isinstance(x, BaseException)}
    contagem.update(type(x).__name__ for x in excecoes.values())
    return dict(contagem)


def medir_fatia(r, nome, acao, falhas):
    inicio = len(falhas)
    try:
        dados = acao()
        if transaction.get_rollback():
            falhas.append("TransacaoMarcadaParaRollback")
    except Exception as erro:
        falhas.append(erro)
    erros = tipos_de_falha(falhas[inicio:])
    del falhas[inicio:]
    r[nome] = (
        {"status": "INCONCLUSIVO", "excecoes_por_tipo": erros}
        if erros else {"status": "CONCLUSIVO", **dados}
    )


def decisor_dp():
    usuarios = get_user_model().objects.filter(is_active=True).order_by("-is_superuser", "pk")
    with closing(usuarios.iterator(chunk_size=200)) as ativos:
        for usuario in ativos:
            if tem_acao(usuario, "editar_ausencias"):
                return usuario
    raise SemDecisorDP()


def medir_caso(caso, falhas, motivos_irmas, montar, ausentes):
    pk, escape, mes, ano = caso
    r = {
        "registro": "caso", "pergunta_pk": pk, "escape": escape,
        "tentou_porta": 0, "tentou_rejeicao": 0, "ausencia": None,
        "passagem": {"status": "NAO_EXERCITADA"},
        "rejeicao": {"status": "NAO_EXERCITADA"},
    }
    fotos = {}
    ausencias = []
    recusas_irmas = defaultdict(set)
    criar_real, evento_real = A.criar_ausencia, observ.evento
    colab = dia = matricula = None

    def criar_observada(**kw):
        # app/chamados/services/materializacao.py:994,
        # app/chamados/services/materializacao.py:1011
        chamada_da_passagem = (
            sys._getframe(1).f_code is M._materializar_ausencia_de_resposta.__code__
        )
        ausencia = criar_real(**kw)
        if chamada_da_passagem:
            ausencias.append(getattr(ausencia, "pk", None))
        return ausencia

    def evento_observado(acao, modelo, objeto_id, **kw):
        if acao == M.ACAO_IRMA_VIVA_NA_AUSENCIA and modelo == "PerguntaDisputa":
            dados = kw.get("depois") or {}
            if dados.get("escape_pk") == pk:
                motivo = dados.get("motivo")
                if "motivo" not in dados:
                    ausentes.add("ACAO_IRMA_VIVA_NA_AUSENCIA.depois.motivo")
                    recusas_irmas["motivo_indisponivel"].add(objeto_id)
                elif isinstance(motivo, str) and motivo in motivos_irmas:
                    recusas_irmas[motivo].add(objeto_id)
                    if motivo in REVERSOES:
                        falhas.append("_Reprovada")
                else:
                    falhas.append("MotivoDeRecusaInesperado")
        return evento_real(acao, modelo, objeto_id, **kw)

    def passagem():
        nonlocal colab, dia, matricula
        p = PerguntaDisputa.objects.select_related(
            "disputa__colaborador__empresa", "disputa__colaborador__usuario",
        ).get(pk=pk)
        colab, dia = p.disputa.colaborador, p.data_evento
        usuario = colab.usuario
        if usuario is None or not usuario.is_active:
            raise SemUsuarioAtivo()
        r.update(colaborador_pk=colab.pk, empresa_pk=colab.empresa_id)
        # Mesma normalizacao da montagem. app/folha/export.py:728
        codigo = (colab.codigo_dominio or "").strip()
        matricula = codigo.zfill(10) if codigo else None
        fotos["antes"] = foto(colab, dia, mes, ano, montar)
        req = APIRequestFactory().post(
            reverse("api:disputa_responder", kwargs={"pk": pk}),
            {"resposta": escape},
            format="json",
        )
        force_authenticate(req, user=usuario)
        with patch.object(A, "criar_ausencia", criar_observada), \
                patch.object(observ, "evento", evento_observado):
            r["tentou_porta"] = 1
            resposta = api_disputa_responder(req, pk=pk)

        if resposta.status_code == 200 and resposta.data.get("status") == "ok":
            r["porta"] = "ACEITA"
            if len(ausencias) != 1 or ausencias[0] is None:
                raise EvidenciaInesperada()
            r["ausencia_pk"] = ausencias[0]
        else:
            motivo = RECUSAS.get((resposta.status_code, resposta.data.get("erro")))
            if motivo is None:
                raise EvidenciaInesperada()
            r.update(porta="RECUSADA", motivo_recusa=motivo)

        fotos["depois_passagem"] = foto(colab, dia, mes, ano, montar)
        if "ausencia_pk" in r:
            # A porta pode criar, devolver ou reabrir uma linha.
            # app/ponto/services/ausencia.py:392, app/ponto/services/ausencia.py:436,
            # app/ponto/services/ausencia.py:560, app/ponto/models.py:749,
            # app/ponto/models.py:809
            ausencia = Ausencia.objects.values("tipo", "status").get(pk=r["ausencia_pk"])
            r["ausencia"] = {
                "tipo": ausencia["tipo"],
                "status_apos_passagem": ausencia["status"],
                "status_apos_rejeicao": None,
                "nova_na_foto": r["ausencia_pk"] not in fotos["antes"]["modelos"]["Ausencia"],
            }
        return {"diferencas": diferencas(fotos["antes"], fotos["depois_passagem"])}

    def rejeicao():
        ausencia = Ausencia.objects.get(pk=r["ausencia_pk"])
        dp = decisor_dp()
        r["tentou_rejeicao"] = 1
        A.decidir(ausencia, "rejeitada", por=dp, decisor=dp, aprovada_em=timezone.now())
        ausencia.refresh_from_db()
        r["ausencia"]["status_apos_rejeicao"] = ausencia.status
        if ausencia.status != "rejeitada":
            raise EvidenciaInesperada()
        devolvidas = M.devolver_absorvidas_da_ausencia_rejeitada(ausencia)
        fotos["depois_rejeicao"] = foto(colab, dia, mes, ano

[truncado em 20000 de 28189 caracteres]

## 20/11 19:13 · sessao a29af3c9c491 · E4C-F5-CONSTROI · leitura · origem=Code

## E4C-F5-CONSTROI

O anexo registra mudança **nos dois campos** `hash_hoje` e `linhas_hoje`, na passagem e na rejeição, para `FALTA_DIA` nas perguntas `33668`, `33669` e `33686`. Nas outras cinco `FALTA_DIA` e nas oito `ATESTADO`, o par ficou igual. Nos 16 casos nasceu uma `Ausencia` e as fotos recortadas de `FechamentoMensal`, `Batida` e `ExportacaoDominio` ficaram iguais. O resumo traz os nomes dos campos mudados. A direção do delta, as rubricas afetadas, a volta ao hash inicial e o valor de `divergente` em cada foto ficam **[nao verificado]**. A identidade Git da cópia com o SHA informado também fica **[nao verificado]**. `medida_6a1_resumo.txt:1–18` · `sonda_6a1.py.txt:104–148`

## 1. Fontes de `montar_linhas_export`

| Fonte | Campos e efeito |
|---|---|
| **Gravado — `FechamentoMensal`** | Seleciona por `colaborador_id` ligado à empresa, `mes`, `ano` e `status` em `aberto`/`aprovado`. Os eventos leem `horas_extras_50`, `horas_extras_100`, `horas_extras_100_feriado`, `horas_folga_trabalhada`, `horas_noturnas`, `horas_intra_indenizada`, `horas_atraso` e `horas_saida_antecipada`. A HE100 normal é o total menos a parcela de feriado. `horas_trabalhadas` também decide `cadastro_zero`. `horas_trabalhadas` e `turnos_abertos` alimentam `travas_aprovacao`. `turnos_abertos` e `inconsistencias` são passados ao retentor do espelho, que ignora esses argumentos e consulta as células. `app/folha/export.py:399–452`, `:687–739`, `:830–848`, `:902–968`, `:1301–1307` |
| **Viva — `Colaborador` e `Empresa`** | `empresa_id` define a população. `codigo_dominio` decide presença de matrícula e fornece a matrícula da linha. `nome` ordena os fechamentos. `data_demissao` decide a saída por rescisão e, junto de `data_admissao`, recorta a auditoria das células. `situacao` e `isento_ponto` influem na retenção. A isenção também suprime os eventos de horas, enquanto o bloco de falta tem sua própria condição. `Empresa.dia_inicio_competencia` define a janela, e `regime_trabalhista` seleciona a régua. `app/folha/export.py:54–60`, `:417–419`, `:718–744`, `:837–875`, `:1205–1213`, `:1238–1247` · `app/ponto/models.py:481–485` · `app/ponto/janelas.py:36–57` · `app/core/regua_cct.py:352–374` · `app/core/constants.py:13–18` · `app/colaboradores/services/desligamento.py:23–32` |
| **Viva — `EmpresaIntegracao`, `RubricaDominio`, `CatalogoDominio`** | Integração por `empresa`, `sistema='dominio'` e `ativo=True`, fornecendo `codigo_no_sistema`. Rubricas ativas por `evento`, com a da empresa prevalecendo sobre a global, fornecem `codigo_fopag` e `tipo`. O catálogo da empresa fornece os `codigo` homologados. Catálogo vazio desliga a retenção por homologação. `app/folha/export.py:23–45`, `:258–260`, `:730–744`, `:984–1019` |
| **Viva — `Ausencia` e `TipoAusencia`** | `Ausencia.colaborador_id`, `tipo`, `status`, `data` e `data_fim` decidem quais datas de falta entram. O catálogo usa `TipoAusencia.ativo`, `codigo`, `efeito` e `fim` para selecionar tipos que descontam e tratar intervalos em aberto. A leitura usa cache de processo de 60 s e o catálogo em código como fallback. O bloco usa a contagem das datas e emite um registro `11` por ocorrência de data, em ordem. `Ausencia.minutos` e `FechamentoMensal.horas_falta` ficam fora dessa montagem. `datas_abonadas` existe no módulo, mas fica fora do caminho de `montar_linhas_export`. `app/folha/export.py:63–92`, `:146–221`, `:725–746` · `app/ponto/turnos.py:2802–2818` · `app/ponto/catalogo/ausencias.py:198–205`, `:240–281`, `:331–333`, `:458` |
| **Viva — `CelulaDia`, `EscalaColaborador`, `TipoEscala`** | Para `furo_espelho`, lê a célula por `colaborador_id` e `data`, seu `veredito`, `protestos`, `trabalha` e, na exceção de furo só de pausa, `ata.lampadas` e `dna.marcos`. Para saber se um dia sem célula deveria ter uma, lê os vínculos e `ativa`, `data_inicio`, `data_fim`. Para o ciclo no fim da janela e para o previsto de `cadastro_zero`, lê `CelulaDia.escala_geradora_id`, o vínculo e seu `tipo_escala`; o previsto usa `trabalha`, marcos e pausas do DNA ou, quando aplicável, ciclo, dias, horários e pausas do template. A consulta de âncora nula em vínculo ativo `12x36` alimenta `travas_aprovacao`. `app/folha/export.py:357–380`, `:805–836`, `:936–949`, `:1148–1173`, `:1247–1298` · `app/ponto/juiz_batida.py:149–176` · `app/escala/alimentacao.py:148–159`, `:205–216`, `:269–286`, `:316–331` · `app/escala/utils.py:2267–2302` · `app/escala/models.py:281–345`, `:816–867`, `:1087–1133`, `:1182–1208` |
| **Viva — `FolgaDia`, `Feriado`, `Posto`, `Municipio`** | No previsto sem célula, usa `FolgaDia.escala_colaborador_id`/`data`, a âncora do vínculo e `trabalha_em_feriado`. A consulta de feriado usa `data`, `abrangencia`, `uf` e `municipio`, com o município do posto do vínculo e seus `uf`/`nome`. `app/escala/models.py:1271–1319`, `:1356–1395` · `app/core/feriados.py:27–29`, `:40–83`, `:124–132` |
| **Viva — `AgendamentoFerias`, `PeriodoAquisitivo`, `Batida`** | `data_inicio`, `data_fim`, `status` e `periodo_aquisitivo.colaborador_id` definem férias na janela, excluindo canceladas. Cruza com `Batida.colaborador_id`, `timestamp` e `retratada_em IS NULL`. A mesma leitura de batidas apuráveis serve a `cadastro_zero`, que exige pelo menos duas batidas, zero horas trabalhadas gravadas e nenhum dia com previsto. `app/folha/export.py:482–505`, `:902–967` · `app/ferias/services.py:49–56` · `app/ponto/turnos.py:2891–2899` |
| **Viva — `AplicacaoConvencao` e `Sindicato`** | O vínculo fornece `posto.praca` e `tipo_escala.tipo_ciclo`. A régua consulta aplicação por `empresa`, `ativo`, `praca` e `sindicato`, com `cct_vigencia_inicio`/`cct_vigencia_fim` no fim da janela. A montagem consome `feriado_12x36_em_dobra` para decidir se emite a parcela de feriado no `12x36`. `app/folha/export.py:357–380`, `:420–443` · `app/core/regua_cct.py:251–275`, `:328–380` |

`ExportacaoDominio` entra **no comparador**. A vigência usa empresa/mês/ano, maior `(gerado_em, pk)` e `invalidada_em`. O comparador usa `hash_sha256` e `total_linhas` desse registro para o lado vigente. O lado `hoje` é a montagem da empresa inteira, serializada em CRLF com terminador final quando há linhas, e devolve SHA-256 e contagem. `app/folha/models.py:203–225` · `app/folha/export.py:539–553`, `:580–594`, `:655–679`

## 2. Caminhos que mudam o par com o fechamento fotografado igual

1. **Emissão direta do bloco de falta.** Uma ausência `aprovada`, de tipo que o catálogo diz que `desconta`, acrescenta as datas em que intercepta a janela. Se o colaborador continua elegível e há mapeamento `falta_diurna`, o primeiro bloco acrescenta um registro `10` e os `11` das datas. Se o bloco já existia, muda sua contagem e entram os `11` adicionais. Ao rejeitar, a linha deixa a consulta de aprovadas. A consulta acumula ocorrências de datas, inclusive quando duas ausências qualificadas cobrem o mesmo dia. `app/folha/export.py:86–92`, `:159–185`, `:215–221`, `:725–745`
2. **Homologação muda a entrada do colaborador.** A mesma falta viva ativa a checagem da rubrica `falta_diurna`. Se o catálogo de homologação tem códigos e o `codigo_fopag` dessa rubrica está fora dele, `classificar_export` pode passar a devolver `fora/nao_homologado`. Com isso, podem sumir **todas** as linhas do colaborador. Rejeitar pode liberar a entrada quando esse era o último motivo de retenção. `app/folha/export.py:714–727`, `:968–979`, `:994–1019`
3. **Rejulgamento da célula muda `furo_espelho`.** Cada `save` de `Ausencia` chama o cartório de forma síncrona. A cobertura aprovada pode transformar um furo em dia decidido ou mudar as lâmpadas da ata. Também pode surgir uma acusação, como `FATO_EM_AUSENCIA`, conforme o fato no dia. A cobertura para cobrança inclui ausências abertas e pode mudar a impressão e o julgamento, inclusive pela regra que verifica `cobertura_status is None`. Há ainda a rota indireta pelo chamado: a passagem pode encerrar destinos ou a disputa, o signal de mudança de status do chamado julga sua célula, e o cartório pode rejulgar após reconciliar chamados ou religar perguntas. Em todos esses ramos, é `veredito`/`protestos`/ata que chegam ao retentor e podem mudar a entrada do bloco inteiro. `app/ponto/signals.py:85–93`, `:115–147` · `app/ponto/services/cartorio.py:818–830`, `:1381–1443`, `:1499–1510`, `:1898–1922`, `:1044–1048` · `app/ponto/supra_juiz.py:317–329`, `:395–418`, `:507–512` · `app/chamados/services/materializacao.py:1105–1114` · `app/chamados/signals.py:19–46`, `:189–218` · `app/folha/export.py:1264–1298`
4. **Depois do commit, há caminhos adicionais fora da medida.** A reconciliação de chamados por ausência pode disparar novo julgamento. Se a disputa fechar e houver regularização externa confirmada por aplicar, outro callback pode criar, corrigir ou remover batidas. Isso pode mudar os filtros vivos de `ferias_com_batida` e `cadastro_zero`, além de disparar o cartório. Uma inclusão pela porta de batida também pode agendar recálculo mensal. A ocorrência desses ramos nos casos do anexo fica **[nao verificado]**. `app/ponto/signals.py:254–272` · `app/chamados/services/silencio_ausencia.py:94–128` · `app/chamados/signals.py:122–146`, `:189–218` · `app/chamados/services/regularizacao_ext.py:71–83`, `:122–189`, `:211–227` · `app/ponto/registro_batida.py:174–198` · `app/folha/export.py:491–504`, `:902–967`

## 3. O que distingue os casos

- **Tipo, estado e cobertura.** Na criação nova, `FALTA_DIA` pede `falta/aprovada`. `ATESTADO` pede `atestado` sem anexo e nasce em `aguardando_documento` ou `aguardando_decisao`, conforme `exige_documento` do catálogo. Assim, o atestado recém-criado fica fora da busca de faltas aprovadas, mas entra na cobertura de cobrança enquanto estiver aberto. Para o cartório, quando várias ausências cobrem o dia, vence maior `data` e depois maior `pk`. Para o bloco de falta, valem todas as ocorrências aprovadas cujo tipo desconta. Os minutos da falta podem ser derivados da jornada no `save`, mas o bloco usa datas. `app/chamados/services/materializacao.py:1027–1035`, `:1125–1131` · `app/ponto/services/ausencia.py:473–491`, `:926–944` · `app/ponto/turnos.py:2523–2532`, `:2550–2571` · `app/ponto/models.py:878–897` · `app/folha/export.py:86–92`, `:159–185`, `:215–221`
- **Entrega, fechamento e elegibilidade.** `ENTREGUE` decorre da exportação vigente da empresa. A emissão exige um `FechamentoMensal` do colaborador em `aberto` ou `aprovado` e classificação `entra`. A classificação testa rescisão, matrícula, férias com batida, `cadastro_zero`, `furo_espelho` e homologação. Um segundo dia que ainda retenha, a falta de rubrica ou um colaborador que continue fora podem deixar o par igual. Para o ramo da célula, importam também existência da célula, relógio do turno, impressão anterior, cobertura, batidas, chamados e perguntas vivas, e as exceções de protesto não cobrável ou só de pausa. `app/folha/export.py:580–594`, `:687–745`, `:866–979`, `:1264–1298` · `app/ponto/services/cartorio.py:1381–1443`, `:2062–2090`
- **O que explica cada PK fica [nao verificado].** O anexo traz empresa, escape e campos alterados. Ficam sem evidência por caso o `status` do fechamento, os vereditos, a elegibilidade, a rubrica resolvida e as linhas que entraram ou saíram. Como `linhas_hoje` mudou nos três casos, uma mera permutação das mesmas linhas é insuficiente para explicar o achado. `medida_6a1_resumo.txt:11–18` · `sonda_6a1.py.txt:125–128` · `app/folha/export.py:658–677`

## 4. Escritores alcançáveis fora da foto

| Etapa | Escritas que o código permite |
|---|---|
| **Passagem pela API** | O core salva resposta e carimbos de `PerguntaDisputa`, registra `DecisaoPergunta`, pode levantar o silêncio do chamado e anexa `FollowupChamado`. O materializador insere `Ausencia` ou, em outros casos, reaproveita/reabre uma existente, carimba irmãs, registra absorções e pode fechar chamados e `DisputaSupervisao`. Fechar o chamado pode disparar S143, relavra de perguntas e rebaixamento de urgência. Fechar a disputa pode disparar o fechamento do pai e S144, que carimba perguntas. Há trilha em `LogAuditoria`. `app/api/views.py:2624–2632`, `:2652–2658` · `app/chamados/services/disputa_emissao.py:1499–1581` · `app/ponto/services/ausencia.py:392–443`, `:559–598` · `app/chamados/services/materializacao.py:1027–1114`, `:1241–1253`, `:2207–2228` · `app/chamados/models.py:304–325`, `:2118–2126`, `:3400`, `:4438` · `app/chamados/signals.py:19–73`, `:91–119`, `:149–165` · `app/chamados/services/nota.py:103–107` · `app/core/observ.py:133–138` |
| **Passagem e rejeição, síncronas** | O cartório pode gravar `CelulaDia` e `EventoCelula`, inclusive veredito, protestos, ata, ausência e carimbos. Também pode criar ou alterar chamados, disputas e perguntas, rejeitar `Justificativa` pendente em fechos específicos e escrever `Pauta`/`ContadorPauta`, por exemplo no limbo de competência entregue ou em recusa encaminhada ao DP. O evento da pergunta também pode inserir `EventoCelula`. Nenhuma dessas tabelas tem foto própria na sonda. `app/ponto/portas/celula.py:945–965`, `:1090–1104`, `:1181–1216` · `app/ponto/services/cartorio.py:1035–1074`, `:1580–1585`, `:1876–1897` · `app/ponto/services/cluster_espurio.py:176–196` · `app/ponto/services/justificativa.py:327–353` · `app/chamados/services/fabrica_da_celula.py:160–174`, `:214–219`, `:274–284` · `app/chamados/services/fecho_runtime.py:130–141` · `app/pautas/services.py:180`, `:344–365` · `app/chamados/signals.py:252–259` · `sonda_6a1.py.txt:104–129` |
| **`FechamentoMensal`** | Tanto o nascimento quanto `decidir` podem chamar `_acordar_projecao` quando muda o insumo aprovado. O escritor zera `derivado_em` e atualiza `atualizado_em` nas competências tocadas **do mesmo colaborador**, onde `derivado_em` estava preenchido. A consulta aceita faixa de competências e independe do status de fechamento e da vigência do TXT. Em fato de um dia, esse escritor alcança a competência desse dia. Em faixa que atravessa competências, alcança linhas fora da foto. O exportador não consome esses dois carimbos. Se a linha fotografada existia e tinha `derivado_em` preenchido, a invalidação bem-sucedida apareceria no diff. Qual condição ocorreu em cada caso fica **[nao verificado]**. `app/ponto/services/ausencia.py:126–151`, `:177–195`, `:594–598`, `:1121–1124` · `app/ponto/services/fechamento.py:1573–1599`, `:1648–1665` · `app/folha/export.py:717–746` · `sonda_6a1.py.txt:92–116`, `:132–148` |
| **Rejeição exercitada** | `decidir` salva status, limpa `prazo_documento` e salva os carimbos do decisor. A chamada manual de devolução só segue se a ausência está rejeitada, o dia ficou sem outra cobertura para cobrança e há perguntas absorvidas recuperáveis. Pode limpar resposta, validação e materialização de `PerguntaDisputa`, inserir `DecisaoPergunta` e `EventoCelula`, reabrir `DisputaSupervisao` e gravar nota e auditoria. `get_user_sistema` pode criar `auth.User` se faltar. `app/ponto/services/ausencia.py:975–998`, `:1084–1124` · `app/chamados/services/materializacao.py:1151–1167`, `:1187–1222` · `app/chamados/services/disputa_emissao.py:1630–1685` · `app/chamados/models.py:2359–2366` · `app/core/utils.py:6–18` · `sonda_6a1.py.txt:293–305` |
| **Após commit ou pela tela** | O callback da ausência também reconcilia chamados e encerra os ligados à decisão, com estado, silêncio, nota e trilha. Há callbacks do beco da disputa e do filho que acorda o pai. O callback de regularização pode escrever `RegularizacaoExterna`, seus itens, `Batida` de outros dias, `TurnoMaterializado` e, quando a inclusão aciona recálculo, `FechamentoMensal` e `DiaPago`. Na tela, rejeitar individualmente percorre as ausências abertas da mesma raiz documental. Cancelar aceita também `aprovada` e grava descrição e trilha. O push web da rejeição individual pode apagar uma inscrição expirada. `app/ponto/signals.py:254–272`, `:19–50` · `app/ponto/services/ausencia.py:832–876` · `app/chamados/signals.py:122–146`, `:229–240`, `:351–371` · `app/chamados/services/regularizacao_ext.py:122–206` · `app/ponto/nucleo.py:40–59` · `app/ponto/services/fechamento.py:1144–1205`, `:1798–1824` · `app/ponto/views.py:2829–2893` · `app/chamados/push_utils.py:38–51`, `:114–118` |

No caminho síncrono rastreado, a escrita no fechamento é a invalidação acima. `ExportacaoDominio.registrar` e `invalidar` aparecem na porta do regerador, enquanto a montagem e o comparador só consultam. A foto cobre apenas o vigente encontrado naquele instante. Escrita efetiva em exportações de fora desse recorte fica **[nao verificado]**. `app/ponto/services/fechamento.py:1648–1656` · `app/folha/export.py:655–679`, `:711–746` · `app/folha/management/commands/regerar_txt_dominio.py:118–124` · `sonda_6a1.py.txt:104–128`

## 5. O que a sonda mede e onde pode errar

- **Mede o delta líquido em três instantes.** A passagem chama a view real com autenticação forçada, e a rejeição chama o serviço real mais a devolução manual. As tabelas fotografadas são comparadas por PK e por assinatura de todos os campos concretos, com `Cast` para texto. A ordem das linhas da foto não muda esse diff. Escritas intermediárias desfeitas antes da foto, tabelas omitidas, batidas de outro dia civil e exportações fora do vigente ficam invisíveis. Dia civil e dia do turno podem divergir na virada da meia-noite. A escolha é dos primeiros oito por escape na ordenação do censo, logo a proporção observada descreve a amostra. `sonda_6a1.py.txt:92–148`, `:152–188`, `:257–305` · `app/ponto/turnos.py:2902–2909`
- **O TXT mede bytes e quantidade.** A montagem herda ordenação por ano/mês/nome sem desempate explícito por PK entre nomes iguais. Uma troca só de ordem pode mudar o hash com a contagem igual. Aqui a contagem também mudou. O nome `TXT_do_gravado` na foto guarda apenas o lado `hoje`; mudanças no booleano `divergente`, retorno ao hash inicial e diferenças por matrícula ou rubrica ficam fora desse resultado. `app/ponto/models.py:481–485` · `app/folha/export.py:717–745`, `:539–553`, `:673–679` · `sonda_6a1.py.txt:125–128`, `:132–148` · `medida_6a1_resumo.txt:13–15`
- **Relógio e cache podem interferir.** A sonda fixa o fuso e deixa o relógio correr. A virada de data local pode mudar `sem_celula` na montagem, e o fim do turno pode mudar o que o cartório julga. O catálogo de ausência vive em cache de processo, compartilhado entre casos. Num snapshot de banco estável, expirar e carregar os mesmos dados é neutro; um cache inicialmente diferente do snapshot ou a entrada/saída do fallback pode alterar a leitura. O cache `_JANELA_MEMO` está no caminho de `prontidao`, fora desta montagem. Ocorrência dessas interferências fica **[nao verificado]**. `sonda_6a1.py.txt:357–358`, `:387–394` · `app/folha/export.py:1022–1067`, `:1238–1242`, `:1264–1296` · `app/ponto/services/cartorio.py:2064–2090` · `app/ponto/catalogo/ausencias.py:240–281`
- **Entre casos há rollback; entre as duas etapas do mesmo caso há continuidade.** Cada caso termina revertendo seu savepoint dentro do `REPEATABLE READ` externo. Isso isola as linhas de um caso do seguinte, inclusive quando a pergunta se repete para outro escape. Cache Python e consumo de IDs de sequência podem sobreviver ao rollback. A rejeição vê os chamados, perguntas e células deixados pela passagem do próprio caso. Os callbacks `on_commit` são descartados; a sonda invoca manualmente só a devolução das perguntas. A rejeição de `FALTA_DIA` aprovada é válida pelo serviço e corresponde ao flip do cancelamento, pois a view de rejeição seleciona apenas `ABERTAS`. `sonda_6a1.py.txt:293–316`, `:383–398` · `app/chamados/services/trava_do_colaborador.py:12–17` · `app/ponto/signals.py:254–272` · `app/ponto/services/ausencia.py:912–922` · `app/ponto/views.py:2835–2851`, `:2879–2893`
- **“Conclusivo” tem o alcance do detector.** O wrapper conta um PK devolvido por `criar_ausencia`; a prova de inserção é `Ausencia.novas` na foto. O detector recolhe exceções, logs com `exc_info` e logs de nível `ERROR` ou acima. A invalidação de fechamento captura exceção e pode emitir apenas `WARNING` sem `exc_info`; o catálogo pode

[truncado em 20000 de 22825 caracteres]

## 20/11 18:55 · sessao 51a596ddf1e6 · E4C-F5-CONSTROI · leitura · origem=Code

### `app/chamados/tests/test_main_dois_casos.py`

```python
import json
from contextlib import contextmanager
from datetime import timedelta
from unittest.mock import patch

from django.contrib.auth.models import User
from django.db import transaction
from django.test import TransactionTestCase
from freezegun import freeze_time

from chamados.models import DecisaoPergunta as D
from chamados.models import DisputaSupervisao, PerguntaDisputa as P, _OPCOES_ESCAPE_FALTA
from chamados.services import disputa_emissao as E
from chamados.services import materializacao as M
from chamados.services import validacao
from chamados.tests import cortes_f2_f9_apoio as apoio
from core.models import LogAuditoria
from escala.models import CelulaDia
from ponto.models import Ausencia
from ponto.services.ausencia import decidir
from ponto.services.cartorio import calado

# apoio_a1c_c8eb.py.txt:24–28 e app/chamados/tests/cortes_f2_f9_apoio.py:30
T1 = apoio.AGORA
T0 = T1 - timedelta(seconds=2)
T2 = T1 + timedelta(seconds=1)
T3 = T1 + timedelta(seconds=2)
DIA = T1.date()


@contextmanager
def medida(cenario):
    fatos = dict(
        apos_passagem=None,
        antes=None,
        depois=None,
        retorno_retirada=None,
        retornos_devolucao=[],
        nao_devolvidas=None,
        sem_contagem_na_trilha=None,
    )
    try:
        yield fatos
    except Exception as erro:
        fatos["tipo_excecao"] = type(erro).__name__
        raise
    finally:
        print(
            f"MEDIDA: {cenario} | "
            + json.dumps(fatos, default=lambda valor: valor.isoformat()),
            flush=True,
        )


class MainDoisCasosTest(TransactionTestCase):
    def _preparar(self):
        # apoio_a1c_c8eb.py.txt:32–39 e app/chamados/tests/cortes_f2_f9_apoio.py:39–44
        self.relogio = self.enterContext(freeze_time(T0))
        self.enterContext(calado())
        apoio.BaseCortes.setUpTestData.__func__(type(self))
        self.usuario = User.objects.create_user(username="col01")
        self.colab.usuario = self.usuario
        self.colab.save(update_fields=["usuario"])
        self.disputa = DisputaSupervisao.objects.create(colaborador=self.colab, contexto={})

    def _escape(self, tipo="escolha"):
        # apoio_a1c_c8eb.py.txt:41–47
        p, criada = P.abrir_conversa(
            disputa=self.disputa, motivo="saida_sem_entrada", pergunta="Por que faltou?",
            tipo_resposta=tipo, opcoes=list(_OPCOES_ESCAPE_FALTA),
        )
        if not criada or p.data_evento != DIA:
            self.fail("CENARIO NAO MONTAVEL: o escape não nasceu no dia da ausência.")
        return p

    def _irma_muda(self):
        # apoio_a1c_c8eb.py.txt:73–78
        p, criada = P.abrir_conversa(
            disputa=self.disputa, motivo="intervalo_saida",
            pergunta="A que horas saiu para o intervalo?", tipo_resposta="hora",
        )
        if not criada or p.data_evento != DIA:
            self.fail("CENARIO NAO MONTAVEL: a irmã muda não nasceu no dia da ausência.")
        return p

    def _irma_com_luz(self):
        # apoio_a1c_c8eb.py.txt:49–63
        celula = CelulaDia.objects.create(
            colaborador=self.colab, data=DIA, trabalha=True, origem="gerada",
            dna={"marcos": {"hf": "19:00"}},
        )
        celula.lavrar_veredito(
            via="cartorio", veredito="concorde", agora=T0,
            ata={"lampadas": [{"tipo": "S", "hora": "19:00", "acesa": True, "luz": "19:00"}]},
        )
        p, criada = P.abrir(
            disputa=self.disputa, motivo="orfao_14h", data_evento=DIA,
            pergunta="A que horas saiu?", tipo_resposta="hora",
        )
        if not criada or p.celula_id != celula.pk:
            self.fail("CENARIO NAO MONTAVEL: a irmã de hora não nasceu vinculada à célula.")
        return p

    def _responder(self, p, texto):
        # apoio_a1c_c8eb.py.txt:80–83
        p.refresh_from_db()
        ok, erro = E.responder_pergunta_disputa_core(
            p, texto, self.usuario, ja_respondida=p.respondida_em is not None,
        )
        if not ok:
            self.fail(f"CENARIO NAO MONTAVEL: a porta de resposta recusou {texto!r} ({erro}).")

    @staticmethod
    def _trilha(p):
        # apoio_a1c_c8eb.py.txt:138–140 e app/chamados/services/materializacao.py:1067–1071
        return LogAuditoria.objects.filter(
            acao=M.ACAO_IRMA_VIVA_NA_AUSENCIA, modelo="PerguntaDisputa", objeto_id=p.pk,
        )

    def _foto_grupo(self, ausencia):
        ausencia.refresh_from_db()
        perguntas = {}
        contadas = {}
        qs = P.objects.filter(disputa_id=self.disputa.pk).order_by("pk").prefetch_related("decisoes")
        for p in qs:
            leitura = D.leitura(p)
            perguntas[p.pk] = dict(
                tipo=p.tipo_resposta,
                estado=p.estado,
                via_resolucao=p.via_resolucao,
                materializada_em=p.materializada_em,
                resposta_composta=leitura.resposta_colab,
                respondida_composta_em=leitura.respondida_em,
                validada_composta_em=leitura.validada_em,
                atos=[
                    dict(pk=ato.pk, ato=ato.ato, instante=ato.ato_em)
                    for ato in p.decisoes.all()
                ],
            )
            contadas[p.pk] = self._trilha(p).filter(valor_depois__ausencia_pk=ausencia.pk).count()
        return dict(
            ausencia_pk=ausencia.pk,
            status_ausencia=ausencia.status,
            perguntas_absorvidas_por=[p.pk for p in M.perguntas_absorvidas_por(ausencia)],
            perguntas=perguntas,
            contadas_na_trilha=contadas,
        )

    @staticmethod
    def _novas_reaberturas(antes, depois):
        atos_antes = {
            ato["pk"] for p in antes["perguntas"].values() for ato in p["atos"]
        }
        return {
            pk for pk, p in depois["perguntas"].items()
            if p["estado"] == P.PENDENTE and any(
                ato["ato"] == D.REABERTURA and ato["pk"] not in atos_antes
                for ato in p["atos"]
            )
        }

    def _medir_rejeicao(self, fatos, escape, irma, *, retirar=False):
        self.relogio.move_to(T1)
        self._responder(escape, "ATESTADO")
        try:
            ausencia = Ausencia.objects.get(colaborador=self.colab, data=DIA, tipo="atestado")
        except Ausencia.DoesNotExist:
            self.fail("CENARIO NAO MONTAVEL: a passagem não criou o atestado do dia.")

        montado = fatos["apos_passagem"] = self._foto_grupo(ausencia)
        if retirar:
            self.relogio.move_to(T2)
            fatos["retorno_retirada"] = E.reabrir_para_correcao(
                P.objects.filter(pk=escape.pk), por=self.admin, motivo="Retirada da âncora",
            )
        antes = fatos["antes"] = self._foto_grupo(ausencia)

        devolver_real = M.devolver_absorvidas_da_ausencia_rejeitada
        chamadas = fatos["retornos_devolucao"]

        def observar_devolucao(a):
            chamada = dict(ausencia_pk=a.pk, status=a.status)
            chamadas.append(chamada)
            chamada["retorno"] = devolver_real(a)
            return chamada["retorno"]

        self.relogio.move_to(T3)
        # app/ponto/signals.py:259–279
        with patch.object(M, "devolver_absorvidas_da_ausencia_rejeitada", new=observar_devolucao):
            rejeitada = decidir(ausencia, "rejeitada", por=self.admin, motivo="Teste de rejeição") [nao verificado]
        fatos["retorno_decidir"] = dict(pk=rejeitada.pk, status=rejeitada.status)
        depois = fatos["depois"] = self._foto_grupo(ausencia)

        reabertas_antes = self._novas_reaberturas(montado, antes)
        reabertas_na_rejeicao = self._novas_reaberturas(antes, depois)
        candidatas = {pk for pk, p in antes["perguntas"].items() if p["tipo"] == "hora"}
        nao_devolvidas = candidatas - reabertas_antes - reabertas_na_rejeicao
        contadas = {pk for pk, n in depois["contadas_na_trilha"].items() if n}
        fatos.update(
            reabertas_antes=sorted(reabertas_antes),
            reabertas_na_rejeicao=sorted(reabertas_na_rejeicao),
            nao_devolvidas=sorted(nao_devolvidas),
            sem_contagem_na_trilha=sorted(nao_devolvidas - contadas),
        )

        self.assertEqual(depois["status_ausencia"], "rejeitada", "[M1] A ausência ficou rejeitada.")
        self.assertTrue(
            any(c["ausencia_pk"] == ausencia.pk and c["status"] == "rejeitada"
                and "retorno" in c for c in chamadas),
            "[M2] O callback real foi chamado com esta ausência rejeitada e retornou.",
        )
        vias_esperadas = {irma.pk: P.VIA_AUSENCIA_DIA}
        if retirar:
            vias_esperadas = {irma.pk: P.VIA_DECLARACAO, escape.pk: P.VIA_AUSENCIA_DIA}
        fotos = montado["perguntas"]
        campos = ("tipo", "estado", "via_resolucao", "materializada_em")
        observado = {"perguntas": {
            pk: tuple(fotos[pk][campo] for campo in campos) for pk in vias_esperadas
        }}
        esperado = {"perguntas": {
            pk: ("hora", P.RESOLVIDA, via, T1) for pk, via in vias_esperadas.items()
        }}
        if retirar:
            observado["grupo"] = sorted(montado["perguntas_absorvidas_por"])
            esperado["grupo"] = sorted([escape.pk, irma.pk])
            observado["retorno_retirada"] = fatos["retorno_retirada"]
            esperado["retorno_retirada"] = 1
        else:
            observado["escape_tipo"] = fotos[escape.pk]["tipo"]
            esperado["escape_tipo"] = "escolha"
        self.assertEqual(observado, esperado, "[M3] A montagem divergiu da foto do anexo.")
        self.assertEqual(
            fatos["sem_contagem_na_trilha"], [],
            "[R] Há perguntas de hora sem reabertura e sem linha de trilha desta ausência.",
        )

    def test_escolha_sem_ancora_e_atestado_rejeitado(self):
        with medida("grupo escolha sem âncora") as fatos:
            self._preparar()
            escape = self._escape()
            irma = self._irma_muda()
            self._medir_rejeicao(fatos, escape, irma)

    def test_ancora_retirada_e_atestado_rejeitado(self):
        with medida("grupo âncora retirada após formação") as fatos:
            self._preparar()
            irma = self._irma_com_luz()
            ancora = self._escape("hora")
            self._responder(irma, "19:02")
            with transaction.atomic():
                P.trava_do_colaborador(self.colab.pk)
                irma.refresh_from_db()
                validacao._carimbar_validacao(irma, self.admin)
            self._medir_rejeicao(fatos, ancora, irma, retirar=True)
```

Label para rodar a partir de `app/` na cópia descartável. (`app/manage.py:18`, `app/config/settings/ci.py:6`)

```bash
python3 manage.py test chamados.tests.test_main_dois_casos --settings=config.settings.ci
```

O `[M3]` usa a foto `apos_passagem`. No cenário sem âncora, `ausencia_dia` é a via esperada da irmã muda. No cenário ancorado, é a via esperada da própria pergunta de hora que recebe `ATESTADO`. A irmã com luz tem `declaracao` na foto do anexo. (`medida_dos_dois_em_c8eb.txt:1–2`, `app/chamados/models.py:2655`, `app/chamados/models.py:2683`)

### Nome ausente → substituto

| Nome procurado | Substituto e referência |
|---|---|
| `chamados.tests.test_e4c_f5_a1c_passagem_da_ausencia` e `PassagemDaAusenciaTest` | Fixtures locais com `BaseCortes.setUpTestData` do MAIN. Import procurado em `app/chamados/tests/`. (`medida_c8eb.py.txt:20`, `apoio_a1c_c8eb.py.txt:31–39`, `app/chamados/tests/cortes_f2_f9_apoio.py:37–44`) |
| `a1c.T0`, `a1c.T1`, `a1c.T2`, `a1c.T3`, `a1c.DIA` | Constantes locais derivadas de `apoio.AGORA`, com os deslocamentos do anexo. (`apoio_a1c_c8eb.py.txt:24–28`, `app/chamados/tests/cortes_f2_f9_apoio.py:30`) |
| `PassagemDaAusenciaTest._escape`, `._irma_muda`, `._irma_da_celula`, `._irma_com_luz` | `_escape` e `_irma_muda` locais. `_irma_com_luz` local incorpora a fixture da célula com luz acesa. (`apoio_a1c_c8eb.py.txt:41–63`, `apoio_a1c_c8eb.py.txt:73–78`, `app/chamados/models.py:2963`, `app/chamados/models.py:2981`, `app/escala/models.py:1819`) |
| `PassagemDaAusenciaTest._responder`, `._passagem`, `._ausencia` | `_responder` local, avanço para `T1` com resposta `ATESTADO` e consulta direta do atestado pela mesma chave. (`apoio_a1c_c8eb.py.txt:80–104`, `apoio_a1c_c8eb.py.txt:135–136`, `app/chamados/services/disputa_emissao.py:1439`, `app/chamados/services/disputa_emissao.py:1560–1563`, `app/ponto/models.py:660`) |
| `PassagemDaAusenciaTest._trilha` | `_trilha` local sobre o mesmo `LogAuditoria`, filtrando ação, modelo, PK e `valor_depois__ausencia_pk`. O PK é passado como inteiro, conforme o campo `objeto_id`. (`apoio_a1c_c8eb.py.txt:138–140`, `app/chamados/services/materializacao.py:1067–1071`, `app/chamados/services/materializacao.py:1089`, `app/core/models.py:296–301`) |
| `DecisaoPergunta.composta` | `DecisaoPergunta.leitura` para os três campos compostos da foto. (`apoio_a1c_c8eb.py.txt:120`, `app/chamados/models.py:4295–4326`) |
| `chamados.services.vigia_ato_x_coluna.contar` | Sem substituto para o placar A/B do anexo. O módulo foi procurado em `app/chamados/services/`. A medida pedida lê `PerguntaDisputa.estado`, os atos persistidos e a trilha da ausência. (`medida_c8eb.py.txt:18`, `app/chamados/models.py:3226`, `app/chamados/models.py:4166–4168`, `app/chamados/services/materializacao.py:1067–1071`) |

### Passos sem caminho de produção localizado

- Nenhum nos dois cenários. As referências para passagem, assinatura da irmã, retirada administrativa, rejeição e callback são `app/chamados/services/disputa_emissao.py:1560–1563`, `app/chamados/services/validacao.py:161–175`, `app/chamados/services/disputa_emissao.py:1584`, `app/ponto/services/ausencia.py:1057–1088` e `app/ponto/signals.py:259–279`.
- Montagem e resultados em execução no MAIN [nao verificado].

## 20/11 18:54 · sessao 04615e8207bf · E4C-F5-CONSTROI · sonda · origem=Code

```python
"""
Sonda para bin/sonda_leitura.sh em produção, sem --sombra. O envelope
executa o Python no shell Django, schema juliani, com a role leitor e
a árvore montada em leitura. bin/sonda_leitura.sh:93, :140, :153, :235, :239.

Autoridades do funil

- Pergunta de hora: PerguntaDisputa.tipo_resposta e a entrada 'hora' do
  catálogo. É o recorte do próprio leitor da devolução.
  app/chamados/models.py:2629; app/chamados/catalogo/respostas.py:20;
  app/chamados/services/materializacao.py:1121.
- Resolvida neste funil: materializada_em preenchido, o carimbo que o leitor
  da devolução exige. O rótulo PerguntaDisputa.estado também consulta a
  validação da sequência e, conforme o caso, o C6. A sonda informa
  separadamente quantas desse recorte têm o rótulo confirmado pelas faces
  SQL e quantas ficam indeterminadas sem rodar o motor.
  app/chamados/services/materializacao.py:1121;
  app/chamados/models.py:3226, :3539, :3569; app/chamados/juizes.py:1470.
- Via: via_resolucao e as constantes VIA_AUSENCIA_DIA e VIA_DECLARACAO.
  app/chamados/models.py:2655, :2683, :2748.
- Ato e ausência ligada: DecisaoPergunta.ABSORCAO e o FK absorcao_ausencia
  da linha selecionada. A ordem da sequência é (ato_em, pk).
  app/chamados/models.py:4152, :4166, :4178, :4199, :4202.
- REJEITADA: Ausencia.status == 'rejeitada'.
  app/ponto/models.py:704, :809; app/chamados/services/materializacao.py:1154.
- Outra ausência: efeito 'cobranca', no colaborador e no dia da ausência
  ligada, como na guarda da devolução. A carga chama _pelo_efeito e
  _q_cobertura da árvore. O próprio efeito exclui a rejeitada.
  app/chamados/services/materializacao.py:1146, :1161;
  app/ponto/turnos.py:2523, :2768, :2773;
  app/chamados/services/carga_pergunta_viva.py:99.
- Competência: data_evento e empresa do colaborador da pergunta.
  _competencia chama janela_atual e usa o mês e ano do fim da janela.
  Entregue é o resultado de competencia_entregue com as exportações já
  anotadas por anotar_vigencia. Sem data_evento, o K6 fica sem consulta.
  app/chamados/models.py:2610, :2620;
  app/chamados/services/materializacao.py:788;
  app/chamados/services/passivo_validado.py:86; app/ponto/janelas.py:36, :60;
  app/folha/export.py:556; app/folha/models.py:192, :203.

LACUNA DE AUTORIDADE — ABSORÇÃO VIGENTE

A árvore tem um escritor do carimbo, escritores que o retiram ou trocam
a via e um leitor da decisão efetiva da tarefa. Um juiz único que defina
“ABSORÇÃO vigente” para esta contagem [nao verificado]. A sonda publica
as três leituras abaixo, cada uma com o funil completo.

- carimbo_do_ato: ABSORCAO cujo ato_em coincide com materializada_em na
  via atual. Se há várias linhas no mesmo instante, a sonda usa a última
  pela ordem do modelo. O escritor grava o mesmo instante no carimbo e
  no ato. Descarimbar tira instante e via. Retratar pode trocar só a via.
  app/chamados/services/materializacao.py:1047, :1059;
  app/chamados/models.py:2217, :3399, :3485, :4199.
- sequencia_sem_desfazimento: última ABSORCAO após o último ato que
  registra a retirada do carimbo ou a troca da via por retratação.
  REABERTURA e RECUSA vêm de ATOS_COM_FOTO, usado pelo escritor que
  descarimba. A REABERTURA compensadora também tira o carimbo.
  app/chamados/models.py:4164, :1694, :2217;
  app/chamados/services/disputa_emissao.py:1627, :1639, :1656.
- ato_lido_na_tarefa: ABSORCAO que _pela_decisao lê como efetiva.
  O leitor pode conservar o ato diante de respostas que preservam a
  validação, ignora a reabertura compensadora e distingue o passivo sem
  linha. Quando lê ABSORCAO, ela é a última ABSORCAO da sequência visível.
  app/chamados/services/tarefa_do_dia.py:142, :154, :158, :184, :390, :425;
  app/chamados/models.py:4220, :4237, :4295.
"""

import json


LEITURAS = (
    "carimbo_do_ato",
    "sequencia_sem_desfazimento",
    "ato_lido_na_tarefa",
)
DEGRAUS = (
    "perguntas_de_hora",
    "resolvidas",
    "nas_duas_vias",
    "com_ato_absorcao",
    "com_absorcao_vigente",
    "com_ausencia_ligada",
    "ligada_rejeitada",
    "sem_outra_ausencia_cobrindo_o_dia",
)
REJEITADA = "rejeitada"  # app/ponto/models.py:704, :809.


def ler_absorcoes(alvo, agora):
    # app/chamados/models.py:4199; app/chamados/services/tarefa_do_dia.py:390.
    linhas = (
        D.objects.filter(pergunta_id__in=alvo.order_by().values("pk"))
        .select_related("pergunta__disputa", "absorcao_ausencia")
        .only(
            "pk", "pergunta", "ato", "ato_em", "ato_texto",
            "da_ressuscitacao", "resposta_mantem_validacao",
            "absorcao_ausencia", "absorcao_ausencia__status",
            "absorcao_ausencia__colaborador", "absorcao_ausencia__data",
            "pergunta__disputa", "pergunta__disputa__chamado",
            "pergunta__chamado_destino", "pergunta__materializada_em",
            "pergunta__respondida_em", "pergunta__validada_em",
            "pergunta__validada_por", "pergunta__resposta_colab",
        )
        .order_by("pergunta_id", *D._meta.ordering)
        .iterator(chunk_size=5000)
    )
    desfazem = (*D.ATOS_COM_FOTO, D.RETRATACAO)
    leituras_absorcao = {
        "absorvida_aprovada", "absorcao_rejeitada",
        "absorcao_aguarda_documento", "absorcao_pendente",
    }  # app/chamados/services/tarefa_do_dia.py:184.

    por_pergunta = {}
    for pk, grupo in groupby(linhas, key=attrgetter("pergunta_id")):
        atos = list(grupo)
        if not any(d.ato == D.ABSORCAO for d in atos):
            continue
        p = atos[0].pergunta
        carimbo = sequencia = ultima_visivel = None
        visiveis = []
        for d in atos:
            visivel = d.ato_em <= agora and not D.es_da_ressuscitacao(d)
            if visivel:
                visiveis.append(d)
            if d.ato in desfazem:
                sequencia = None
            elif d.ato == D.ABSORCAO:
                sequencia = d
                if d.ato_em == p.materializada_em:
                    carimbo = d
                if visivel:
                    ultima_visivel = d

        tarefa = None
        if ultima_visivel is not None:
            # O passivo usa as linhas reais; a leitura usa a vista cortada.
            # app/chamados/services/tarefa_do_dia.py:402, :425, :431.
            ja = {(pk, d.ato, d.ato_em) for d in atos
                  if d.ato in (D.RESPOSTA, D.VALIDACAO)}
            passivo = (pk,) if any(
                linha["ato_em"] <= agora
                for linha in D.linhas_da_projecao_sem_linha([p], ja)
            ) else ()
            vista = SimpleNamespace(
                pk=pk, resposta_colab=p.resposta_colab,
                respondida_em=(
                    p.respondida_em
                    if p.respondida_em is not None and p.respondida_em <= agora
                    else None
                ),
                validada_em=(
                    p.validada_em
                    if p.validada_em is not None and p.validada_em <= agora
                    else None
                ),
                validada_por_id=p.validada_por_id,
                decisoes=SimpleNamespace(all=lambda: visiveis),
            )
            # Só se usa a classificação do ato. Fila, lavra e efeitos não
            # decidem o ramo ABSORCAO. app/chamados/services/tarefa_do_dia.py:152, :173, :184.
            razao, _ = _pela_decisao(
                p, visiveis, {}, frozenset(), passivo,
                leitura=D.leitura(vista),
            )
            if razao in leituras_absorcao:
                tarefa = ultima_visivel

        por_pergunta[pk] = dict(zip(LEITURAS, (carimbo, sequencia, tarefa)))
    return por_pergunta


def ler_cobertura(absorcoes):
    pares = {
        (ausencia.colaborador_id, ausencia.data)
        for leituras in absorcoes.values()
        for ato in leituras.values()
        if ato is not None
        for ausencia in (ato.absorcao_ausencia,)
        if ausencia is not None and ausencia.status == REJEITADA
    }
    if not pares:
        return set()
    dias = [dia for _, dia in pares]
    # O mesmo recorte SQL é aplicado em memória para cada par distinto. [nao verificado]
    # app/ponto/turnos.py:2523, :2773;
    # app/chamados/services/carga_pergunta_viva.py:10, :99.
    qs = _pelo_efeito(
        Ausencia.objects.filter(colaborador_id__in={cid for cid, _ in pares}),
        "cobranca",
    ).filter(_q_cobertura(min(dias), max(dias))).only(
        "colaborador", "data", "data_fim", "tipo",
    ).order_by("colaborador_id")
    por_colaborador = {
        cid: LinhasLidas(grupo)
        for cid, grupo in groupby(
            qs.iterator(chunk_size=5000), key=attrgetter("colaborador_id"),
        )
    }
    return {
        (cid, dia) for cid, dia in pares
        if cid in por_colaborador
        and por_colaborador[cid].filter(_q_cobertura(dia, dia)).exists()
    }


def ler_entregues(chaves):
    # app/chamados/services/tarefa_do_dia.py:558;
    # app/folha/models.py:203; app/folha/export.py:580.
    if not chaves:
        return {}
    registros = list(ExportacaoDominio.objects.filter(
        empresa_id__in={e for e, _, _ in chaves},
        mes__in={m for _, m, _ in chaves},
        ano__in={a for _, _, a in chaves},
    ).only("pk", "empresa", "mes", "ano", "invalidada_em").order_by())
    por_chave = defaultdict(list)
    for registro in anotar_vigencia(registros):
        por_chave[registro.empresa_id, registro.mes, registro.ano].append(registro)
    return {
        chave: competencia_entregue(*chave, exportacoes=por_chave[chave])
        for chave in chaves
    }


def nivel_com_absorcao(ato, cobertos):
    if ato is None:
        return 4
    ausencia = ato.absorcao_ausencia
    if ausencia is None:
        return 5
    if ausencia.status != REJEITADA:
        return 6
    if (ausencia.colaborador_id, ausencia.data) in cobertos:
        return 7
    return 8


def novo_funil():
    return {leitura: Counter(dict.fromkeys(DEGRAUS, 0)) for leitura in LEITURAS}


def medir():
    vias = (P.VIA_AUSENCIA_DIA, P.VIA_DECLARACAO)
    # app/chamados/catalogo/respostas.py:20;
    # app/chamados/services/materializacao.py:1121.
    base = P.objects.filter(tipo_resposta="hora")
    alvo = base.filter(materializada_em__isnull=False, via_resolucao__in=vias)
    agora = timezone.now()
    absorcoes = ler_absorcoes(alvo, agora)
    cobertos = ler_cobertura(absorcoes)

    # q_vivas é um superconjunto por contrato. Estar fora dele, ou ter
    # validação pelo ato, basta para confirmar RESOLVIDA com carimbo.
    # app/chamados/models.py:3235, :3539, :3580.
    perguntas = base.annotate(
        estado_confirmado=Case(
            When(
                Q(materializada_em__isnull=False)
                & (P.q_validada_pelo_ato() | ~P.q_vivas()),
                then=Value(True),
            ),
            default=Value(False),
            output_field=BooleanField(),
        ),
    ).order_by("pk").values_list(
        "pk", "colaborador__empresa_id",
        "colaborador__empresa__dia_inicio_competencia",
        "data_evento", "materializada_em", "via_resolucao", "estado_confirmado",
    ).iterator(chunk_size=5000)

    total = novo_funil()
    empresas = defaultdict(novo_funil)
    competencias = defaultdict(novo_funil)
    finais = {leitura: defaultdict(list) for leitura in LEITURAS}
    for pk, empresa, corte, dia, carimbo, via, confirmado in perguntas:
        mes = ano = None
        if dia is not None:
            mes, ano = _competencia(
                dia, SimpleNamespace(dia_inicio_competencia=corte),
            )
        chave = (empresa, mes, ano)
        destinos = (total, empresas[empresa], competencias[chave])
        nivel_base = 1
        if carimbo is not None:
            nivel_base = 3 if via in vias else 2
        atos = absorcoes.get(pk) if nivel_base == 3 else None

        for leitura in LEITURAS:
            nivel = nivel_base if atos is None else nivel_com_absorcao(
                atos[leitura], cobertos,
            )
            for destino in destinos:
                contagem = destino[leitura]
                contagem.update(DEGRAUS[:nivel])
                if nivel >= 2:
                    faixa = "confirmadas" if confirmado else "indeterminadas"
                    contagem["rotulo_resolvida_" + faixa] += 1
                    if nivel == len(DEGRAUS):
                        contagem["rotulo_resolvida_no_final_" + faixa] += 1
            if nivel == len(DEGRAUS):
                finais[leitura][chave].append(pk)

    entregues = ler_entregues({c for c in competencias if c[1] is not None})
    saida = []
    for empresa in sorted(empresas):
        saida.append({
            "tipo": "empresa", "empresa_pk": empresa, "funis": empresas[empresa],
        })
    for chave in sorted(competencias, key=lambda c: (c[0], c[2] or 0, c[1] or 0)):
        empresa, mes, ano = chave
        saida.append({
            "tipo": "competencia",
            "empresa_pk": empresa,
            "competencia": f"{ano:04d}-{mes:02d}" if mes is not None else None,
            "entregue": entregues.get(chave),
            "funis": competencias[chave],
        })
    for leitura, grupos in finais.items():
        pks = {True: [], False: [], None: []}
        for chave, lista in grupos.items():
            pks[entregues.get(chave)].extend(lista)
        saida.append({
            "tipo": "casos_finais", "leitura": leitura,
            "pergunta_pks_entregue": sorted(pks[True]),
            "pergunta_pks_nao_entregue": sorted(pks[False]),
            "pergunta_pks_sem_competencia": sorted(pks[None]),
        })
    saida.append({"tipo": "total", "status": "OK", "funis": total})
    return saida


try:
    from collections import Counter, defaultdict
    from itertools import groupby
    from operator import attrgetter
    from types import SimpleNamespace

    from django.db import connection, transaction
    from django.db.models import BooleanField, Case, Q, Value, When
    from django.utils import timezone

    from chamados.models import DecisaoPergunta as D, PerguntaDisputa as P
    from chamados.services.carga_pergunta_viva import LinhasLidas
    from chamados.services.passivo_validado import _competencia
    from chamados.services.tarefa_do_dia import _pela_decisao
    from folha.export import competencia_entregue
    from folha.models import ExportacaoDominio, anotar_vigencia
    from ponto.models import Ausencia
    from ponto.turnos import _pelo_efeito, _q_cobertura

    # Até 5 SELECTs de dados: decisões e perguntas (1 cada), cobertura
    # (0 ou 1), exportações (0 ou 1), anotação da vigência (0 ou 1).
    # Mais um SET TRANSACTION e o controle da transação. A contagem é de
    # consultas lógicas da sonda, à parte do bootstrap e das operações
    # de cursor do backend. app/folha/models.py:203; bin/sonda_leitura.sh:235.
    with transaction.atomic():
        with connection.cursor() as cursor:
            cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
        linhas_json = [
            json.dumps(linha, ensure_ascii=True, allow_nan=False)
            for linha in medir()
        ]
    for linha_json in linhas_json:
        print(linha_json, flush=True)
except BaseException as exc:
    print(json.dumps({
        "tipo": "total",
        "status": "INCONCLUSIVO",
        "tipo_excecao": type(exc).__name__,
    }, ensure_ascii=True), flush=True)
    raise SystemExit(1) from None


# O que a sonda não mede
#
# - O estado RESOLVIDA exato nos casos indeterminados e a materialização de
#   fato. Essas respostas podem exigir o C6 e a apuração de testemunhas.
#   Os campos rotulo_resolvida_* mostram o limite inferior e a parcela
#   indeterminada do recorte. Campo de contagem ausente nessa família vale
#   zero. app/chamados/models.py:3226; app/chamados/juizes.py:666, :1470.
# - Ligações inferidas do legado sem ato. O carimbo legado pode aparecer
#   até o degrau das vias. A reconstrução pelo escape, carimbo e batida
#   pertence a outro leitor. app/chamados/services/materializacao.py:1101, :1121.
# - Entrega quando falta o dia do fato. Nessa saída, competência e entregue
#   são null. K6 mede TXT vigente para a tripla da empresa.
#   app/chamados/services/materializacao.py:794; app/folha/export.py:556.
# - Execução e contagens de produção [nao verificado]. O anexo é um molde
#   de script e não contém resultados desta contagem [nao verificado].
#   O JSONL e o tratamento de exceções são do Python. O envelope emite
#   mensagens próprias e provisiona credencial, permissões e trilha.
#   bin/sonda_leitura.sh:126, :140, :153, :216, :221, :239, :248.
```

_7610 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
