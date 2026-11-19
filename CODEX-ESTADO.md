# CODEX-ESTADO

> Ultima resposta processada: **19/11/2026 02:16** · publicado em **19/11/2026 02:17** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 19/11 02:16 · sessao 377e7316361b · ESCALA-NAO-DESCREVE · verificador · origem=Code

**BLOQUEIA — a união semântica dos pais não está comprovada.** A cópia não dispõe dos objetos Git dos pais nem da base comum; os anexos fornecem apenas os diffs da catraca e a lista de `.py` de produção trazidos sobre o comboio. Portanto, não é possível concluir que o merge automático de `app/docs/LEIS.md` e dos arquivos de produção preservou a união dos dois lados, nem enumerar com segurança todos os arquivos tocados por ambos. **[não verificado]**

1. **Catraca — NÃO BLOQUEIA.** Executei `censo('app')` em leitura e comparei, chave a chave, os dicionários construídos como no teste: 9 entradas de `pareadores` e 85 de `derivacoes_de_dia`; igualdade total e nenhum arquivo ilegível. A entrada do censo lê e analisa os arquivos de produção em [medida_turno_do_par.py:448](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/core/medida_turno_do_par.py:448); o teste define a comparação em [test_medida_turno_do_par.py:558](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/core/tests/test_medida_turno_do_par.py:558). O argumento `app` corresponde à pasta que `censo` espera; a chamada literal `censo('.')`, com diretório corrente `app`, não foi executada. **[não verificado]**

   Os dois valores alterados contra o comboio estão registrados em [test_medida_turno_do_par.py:280](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/core/tests/test_medida_turno_do_par.py:280) e [test_medida_turno_do_par.py:323](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/core/tests/test_medida_turno_do_par.py:323); os sítios são [regua_defesa.py:524](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/regua_defesa.py:524), [esmeril_espelho.py:179](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:179) e [esmeril_espelho.py:186](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:186). A igualdade foi verificada; se os números foram efetivamente gerados pelo medidor, sem digitação, é **[não verificado]**.

2. **Composição dos pais — BLOQUEIA, por falta de prova, não por defeito semântico demonstrado.** O caso mais relevante para revisão é `app/escala/services/cadastro_realidade.py`: a árvore contém a montagem da lista da ESCALA em [cadastro_realidade.py:960](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/services/cadastro_realidade.py:960) e código identificado como F6 em [cadastro_realidade.py:1076](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/services/cadastro_realidade.py:1076). Isso mostra coexistência na cópia, mas não prova quais alterações cada pai fez nem se a composição preservou integralmente sua semântica. Para esse arquivo, para `LEIS.md` e para os demais arquivos potencialmente tocados pelos dois lados, composição por arquivo: **[não verificado]**.

   A afirmação de que nenhum `.py` de produção mudou *na resolução do conflito*, e a identificação exaustiva de combinações que nenhum pai tinha, também permanecem **[não verificado]**.

3. **Testes da ESCALA — NÃO BLOQUEIA pelo que foi observado.** Em [test_escala_nao_descreve.py](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/tests/test_escala_nao_descreve.py:1), não identifiquei asserção que dependa de número de linha; há, por exemplo, asserções sobre a ordem e os valores da lista em [test_escala_nao_descreve.py:194](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/tests/test_escala_nao_descreve.py:194) e sobre a trilha do vínculo em [test_escala_nao_descreve.py:275](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/tests/test_escala_nao_descreve.py:275). Se algum comportamento desses testes foi alterado pelo comboio, e se há outros testes da ESCALA pertinentes fora desse arquivo, permanece **[não verificado]**. Os testes Django não foram executados. **[não verificado]**

4. **Escritores e leitores observáveis — NÃO BLOQUEIA quanto ao rastreamento abaixo; completude do delta, [não verificado].**

   - `medida['orfas']`: `assinaturas_do_colab` conta as órfãs e escreve a medida em [esmeril_espelho.py:119](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:119); `anomalias_da_frota` a lê e produz `orfas` e `orfas_desde` em [esmeril_espelho.py:233](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:233).
   - `proposta`: `anomalias_da_frota` a produz em [esmeril_espelho.py:241](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:241). `lavrar` entrega o retrato a `gravar_lavra`, que o persiste em `detalhe`, e `ler_lavra` o recupera em [esmeril_espelho.py:321](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:321). `lista` lê os três mapas em [cadastro_realidade.py:960](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/services/cadastro_realidade.py:960); a ação também lê a proposta em [views.py:493](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/views.py:493).
   - `carga`, `carga_desde`, `carga_texto`, `linhas` e `carga_resumo`: `lista` monta os campos em [cadastro_realidade.py:981](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/services/cadastro_realidade.py:981) e ordena e publica a lista e o resumo em [cadastro_realidade.py:1053](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/services/cadastro_realidade.py:1053). A tela os lê em [cadastro_x_realidade.html:17](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/templates/escala/cadastro_x_realidade.html:17) e o PDF em [cadastro_realidade.py:1183](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/services/cadastro_realidade.py:1183).
   - `botao_da_proposta` é calculado em [cadastro_realidade.py:830](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/services/cadastro_realidade.py:830), lido pela lista em [cadastro_realidade.py:987](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/services/cadastro_realidade.py:987) e novamente pela porta em [views.py:498](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/views.py:498). `folga_trabalhada_no_6x1` é usado tanto pela ação da assinatura quanto pelo botão em [cadastro_realidade.py:717](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/services/cadastro_realidade.py:717) e [cadastro_realidade.py:845](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/services/cadastro_realidade.py:845).
   - A view fornece `motivo` a `realocar_vinculo_para_destino` em [views.py:502](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/views.py:502); a função o passa como `confirm_txt` em [regua_defesa.py:316](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/regua_defesa.py:316); a porta o lê no veredito e o encaminha à observação da trilha em [vinculo.py:144](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/colaboradores/services/vinculo.py:144) e [vinculo.py:349](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/colaboradores/services/vinculo.py:349).

Nenhuma questão de interpretação de lei foi demonstrada pelos elementos examinados; a pendência é de evidência da composição. **[não verificado]**

BLOQUEIA: 1  
perguntas de lei: 0

## 19/11 02:09 · sessao 0e79919b9af3 · E4C-F3-LAVRA-FOTO · verificador · origem=Code

**NAO BLOQUEIA — a camada SQL fecha os cinco caminhos exercitados pelos mutantes no contexto da fixture**, somando-se à AST que já existia. Isso não prova a ausência de toda leitura possível de `DecisaoPergunta`: a garantia dinâmica só vale para as execuções capturadas, e a própria docstring declara o resíduo dos ramos não executados que a AST não reconhece. [teste:3](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:3), [teste:312](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:312)

1. **Mutantes e escapes da v2 — NAO BLOQUEIA.** O anexo `mutantes_c3.txt` registra duas falhas de `test_sql_marcos_nao_le_decisao` para cada mutante, uma com `ate` futuro e outra com `None`; não traz os detalhes das asserções. A captura testa a presença do nome da tabela de decisões no SQL. [mutantes_c3.txt:1–20, dado fornecido], [teste:316](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:316)

   | Mutante | Caminho efetivamente inserido |
   |---|---|
   | V1 | Alias de `getattr`, nome montado e consulta avaliada com `list`: corresponde ao escape dinâmico da v2. [mk.py:6, dado fornecido] |
   | V2 | Cursor em variável e nome da tabela montado: corresponde ao escape de SQL bruto da v2. [mk.py:7, dado fornecido] |
   | V3 | Itera perguntas da fixture e chama `respostas_anteriores()`, que percorre `self.decisoes.all()`: corresponde ao método de modelo apontado na v2. [mk.py:8, dado fornecido], [modelo:3793](app/chamados/models.py:3793) |
   | V4 | Chama por alias um helper de topo que avalia consulta a decisões: corresponde ao escape do fecho. [mk.py:9, dado fornecido], [mk.py:13, dado fornecido] |
   | V5 | Chama método de classe local que avalia consulta a decisões: corresponde ao outro escape do fecho. [mk.py:10, dado fornecido], [mk.py:14, dado fornecido] |

   As **cinco mutações não são todos os exemplos da v2**: a reexportação já chamada `D` e o lookup ORM montado não têm mutante específico neste anexo. A captura deve detectar SQL com a tabela nas execuções alcançadas, mas a execução dessas duas formas está **[nao verificado]**. [verif_v2.r, dado fornecido], [teste:320](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:320)

2. **Alcance da fixture e resíduo — NAO BLOQUEIA.** As duas consultas de `PerguntaDisputa` são construídas para ambos os valores de `ate`; a pergunta tem `respondida_em` definido antes das chamadas. O teste exige que alguma consulta mencione a tabela, mas não distingue individualmente as duas consultas nem prova seus resultados. [produção:201](app/chamados/veredito_lavrado.py:201), [teste:288](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:288), [teste:318](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:318)

   Ficam sem exercício demonstrado, por exemplo, uma pergunta com `materializada_em` preenchido, um marco posterior ao `ate` e entradas ou condições alternativas que desviem a execução. A docstring nomeia com honestidade a classe do resíduo — ramo não executado **e** não reconhecido pela AST — sem enumerar cada cenário. [modelo:3647](app/chamados/models.py:3647), [produção:163](app/chamados/veredito_lavrado.py:163), [teste:6](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:6)

3. **CORTE — NAO BLOQUEIA, com limite de evidência.** O teste grava uma decisão em 3/1/2020, posterior ao `respondida_em` fixado em 2/1/2020, e compara os dicionários antes e depois para os dois horizontes. Assim, morde uma implementação que faça essa decisão aumentar o máximo observado nessa fixture. Ele não afirma que o valor anterior seja exatamente `MARCO`; outro marco maior poderia mascarar a mudança. A mordida de um mutante específico que acrescente `Max(ato_em)` está **[nao verificado]**. [teste:272](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:272), [teste:298](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:298), [produção:169](app/chamados/veredito_lavrado.py:169)

4. **Relógio, fixture, subtest e Ruff — NAO BLOQUEIA.** `MARCO`, `ATO` e o horizonte são datetimes fixos e conscientes de UTC; `respondida_em` e `ato_em` não são `auto_now_add`. Os campos `Batida.criado_em` e `CelulaDia.gerada_em`, que também podem contribuir ao máximo, são `auto_now_add`; a ausência de efeitos indiretos que criem registros deles na preparação está **[nao verificado]**. [teste:272](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:272), [modelo:2635](app/chamados/models.py:2635), [modelo:4168](app/chamados/models.py:4168), [batida:79](app/ponto/models.py:79), [célula:1702](app/escala/models.py:1702)

   A atualização direta na fixture cria uma projeção com instante e texto vazio, contornando a guarda do `save`; isso é relevante para interpretar a fixture, embora a consulta de marcos leia o instante. `setUpTestData` prepara essa linha para os testes; os `subTest` registram as falhas por horizonte, como mostra o anexo dos mutantes. Aprovação da suíte sem mutação e resultado do Ruff estão **[nao verificado]**; a configuração seleciona `F`, `E9` e `B`. [teste:287](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:287), [modelo:3679](app/chamados/models.py:3679), [mutantes_c3.txt:1–20, dado fornecido], [ruff.toml:4](app/ruff.toml:4)

5. **Escritores e leitores — NAO BLOQUEIA.**

   - `PerguntaDisputa.respondida_em`: o teste escreve por `update`; em produção, a resposta humana e a automática atribuem e salvam o instante, a trilha também o escreve, e a reabertura pode zerá-lo. `marcos_do_lote` filtra e agrega esse campo; `DecisaoPergunta.linhas_da_projecao_sem_linha` também o lê para formar linhas de projeção. Esta lista identifica caminhos comprovados, não um censo exaustivo. [teste:291](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:291), [resposta humana:1527](app/chamados/services/disputa_emissao.py:1527), [automática:1683](app/chamados/services/disputa_emissao.py:1683), [trilha:1705](app/chamados/services/disputa_emissao.py:1705), [reabertura:1617](app/chamados/services/disputa_emissao.py:1617), [leitor:206](app/chamados/veredito_lavrado.py:206), [projeção:4251](app/chamados/models.py:4251)
   - `DecisaoPergunta.ato_em`: o teste fornece o instante no `create`; em produção, portas como a resposta humana e a automática fornecem o instante a `registrar`, que cria as linhas. `DecisaoPergunta.leitura` usa o campo para ordenar a sequência. A implementação atual de `marcos_do_lote` não o lê diretamente. [teste:307](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:307), [resposta humana:1532](app/chamados/services/disputa_emissao.py:1532), [automática:1686](app/chamados/services/disputa_emissao.py:1686), [registrar:4329](app/chamados/models.py:4329), [leitura:4308](app/chamados/models.py:4308), [marcos:151](app/chamados/veredito_lavrado.py:151)

A identidade Git da cópia e a correspondência completa do diff com a base são **[nao verificado]**: a consulta local informou ausência de repositório Git. O diff fornecido declara apenas a criação do teste, mas não substitui essa verificação. [diff.txt:1, dado fornecido] [nao verificado]

BLOQUEIA: 0  
perguntas de lei: 0

## 19/11 02:03 · sessao 2ee7e747fa4d · DST-L2-JZ1 · verificador · origem=Code

**AVAL-DEVOLVIDO: (1) e (3).** Há uma incompatibilidade literal entre D3 “intacto” e a retenção exigida por R7; além disso, o teto exato depende de medir o desenho conjunto, inclusive a leitura da retenção. O item (2) contém uma exceção nominal a D5 e usa uma porta existente. ([justificativa.py:266](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/services/justificativa.py:266), [reconciliador.py:439](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/chamados/reconciliador.py:439), [nota.py:75](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/chamados/services/nota.py:75); anexos fornecidos sem linhas: [não verificado])

Os anexos foram fornecidos no texto, sem numeração de linhas; suas afirmações são identificadas abaixo pelo nome e marcadas **[não verificado]** quando não confirmadas pela cópia.

### (1) Tetos — PROCEDE

1. **Premissas.** A função atual consulta por chamado: `Q(chamado_origem_id=ch.pk)`, somado ao ponteiro, e executa o filtro de pendentes; os dois fechos a chamam em laço após as transições bem-sucedidas. ([justificativa.py:262](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/services/justificativa.py:262), [cartorio.py:1016](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/services/cartorio.py:1016), [cartorio.py:1555](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/services/cartorio.py:1555))

   As 12 chaves e seus tetos atuais são:

   | Chave | Teto |
   |---|---:|
   | `registrar_batida (sem pergunta)` | 24 |
   | `registrar_batida (3 perguntas)` | 62 |
   | `_gravar_estado (sem pergunta)` | 30 |
   | `_gravar_estado (3 perguntas, CRUZANDO)` | 77 |
   | `reconciliar (gate da premissa, FECHA com dia e celula)` | 62 |
   | `reconciliar (gate da premissa, RECUSA com dia e celula)` | 14 |
   | `registrar_batida (furo vivo, 1 chamado fecha)` | **224** |
   | `registrar_batida (furo vivo, 3 chamados fecham)` | **288** |
   | `registrar_batida (furo vivo, 1 chamado FICA)` | 145 |
   | `registrar_batida (furo vivo, 3 chamados FICAM)` | 219 |
   | `registrar_batida (orfa de intervalo vetada)` | 31 |
   | `_gravar_estado (3 perguntas, NAO cruza)` | 10 |

   As duas em negrito são as chaves de fecho de FURO que o aval altera; a chave do gate `reconciliar (... FECHA ...)` também nomeia fecho e permanece entre as outras dez. ([test_selo_chokepoint_escrita.py:73](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/tests/test_selo_chokepoint_escrita.py:73), [test_selo_chokepoint_escrita.py:134](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/tests/test_selo_chokepoint_escrita.py:134), [test_selo_chokepoint_escrita.py:196](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/tests/test_selo_chokepoint_escrita.py:196), [test_selo_chokepoint_escrita.py:210](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/tests/test_selo_chokepoint_escrita.py:210))

2. **Aval vigente.** O novo aval corta expressamente a proibição anterior de mexer nesses dois tetos e troca a interface por chamado por lote. Porém, declara D3 intacto; a ressalva de R7 entra em conflito com essa declaração no caso descrito no item (3). (Anexos `aval.txt` e `brief_c2.md`: [não verificado]; [justificativa.py:266](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/services/justificativa.py:266))

3. **Literal.** O selo conta todas as queries capturadas, não apenas `SELECT`, e compara o total ao teto. A ocorrência do item (2) cria `FollowupChamado` e, portanto, sua escrita conta se ocorrer dentro da captura. Nos cenários citados na pergunta de desenho, o dado fornecido relata zero justificativas e zero `UPDATE`; assim, pela condição do novo aval, também não haveria ocorrência por rejeição nesses cenários. O `+1` final **não está demonstrado**: R7 também exige consultar a retenção existente, e o custo do desenho conjunto não foi medido nos dados fornecidos. Qualquer delta diferente do corte exige a pergunta de desenho com SQL, como o próprio aval determina. ([test_selo_chokepoint_escrita.py:287](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/tests/test_selo_chokepoint_escrita.py:287), [test_selo_chokepoint_escrita.py:428](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/tests/test_selo_chokepoint_escrita.py:428), [nota.py:102](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/chamados/services/nota.py:102); `PERGUNTA-DE-DESENHO.txt` e `aval.txt`: [não verificado])

4. **Exportada, dinheiro ou `!`.** O desenho de lote declara mudar a busca e os tetos; alcance efetivo sobre exportada, dinheiro ou `!`: **[não verificado]**. A função atual grava a justificativa, inclusive `atualizado_em`, e o aval exige censo de dinheiro antes da conclusão. ([justificativa.py:281](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/services/justificativa.py:281); `aval.txt`: [não verificado])

### (2) Selo JZ1 × D5 — NÃO PROCEDE

1. **Premissas.** O Fix A chama `_nota.anexar_ocorrencia`; a porta usa `autor_tipo='sistema'` por padrão e cria um `FollowupChamado`. O selo exige ao menos um followup de sistema e seu fixture já cria a justificativa com `chamado_origem`. ([reconciliador.py:572](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/chamados/reconciliador.py:572), [nota.py:75](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/chamados/services/nota.py:75), [nota.py:102](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/chamados/services/nota.py:102), [test_jz1_autoridade_unica.py:41](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/tests/test_jz1_autoridade_unica.py:41), [test_jz1_autoridade_unica.py:60](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/tests/test_jz1_autoridade_unica.py:60))

2. **Aval vigente.** D5 proibia followup novo no fecho; o novo aval declara uma exceção nominal quando houve rejeição. A exceção altera D5 nesse ponto e preserva o veto a followup sem rejeição e a S128 no fecho. (`brief_c2.md` e `aval.txt`: [não verificado])

3. **Literal.** A porta existente é suficiente para criar o tipo de registro que o selo conta; a passagem do teste com a implementação proposta continua **[não verificado]**. A ocorrência tem custo SQL, mas sua condição impede que seja criada nos cenários de teto descritos como sem rejeições. ([nota.py:75](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/chamados/services/nota.py:75), [test_jz1_autoridade_unica.py:60](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/tests/test_jz1_autoridade_unica.py:60); `PERGUNTA-DE-DESENHO.txt` e `aval.txt`: [não verificado])

4. **Exportada, dinheiro ou `!`.** A escrita adicional declarada é uma ocorrência no chamado; efeito fora desse escopo, inclusive sobre exportada, dinheiro ou `!`: **[não verificado]**. ([nota.py:78](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/chamados/services/nota.py:78))

### (3) R7, R8 e R9 — PROCEDE

1. **Premissas.** A retenção existente consulta irmãs respondidas sem consumo e, durante `_APROVANDO`, retém quando o ponteiro aponta outra justificativa pendente. A função nova atual não consulta essa retenção. O cartório lê `atualizado_em` da justificativa nas linhas **391–392**, não na 387, que inicia a consulta de ausência. Existe o gerador de ESTADO e existe a função do contador D4; na busca da cópia, só foram encontrados a definição do contador e referências no teste, não um leitor no gerador. ([reconciliador.py:439](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/chamados/reconciliador.py:439), [justificativa.py:262](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/services/justificativa.py:262), [cartorio.py:387](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/services/cartorio.py:387), [cartorio.py:391](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/services/cartorio.py:391), [gerar_estado.py:178](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/bin/gerar_estado.py:178), [justificativa.py:289](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/services/justificativa.py:289), [test_dst_l2_jz1_rejeita_justificativa.py:346](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/tests/test_dst_l2_jz1_rejeita_justificativa.py:346))

2. **Aval vigente.** R7 manda não rejeitar quando a retenção responde reter. No caso de `_APROVANDO` com ponteiro para outra pendente, isso contraria o literal de D3, segundo o qual a pendente apontada é rejeitada, e o item (1) diz que D3 permanece intacto. É necessária uma decisão explícita sobre essa sobreposição. ([reconciliador.py:445](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/chamados/reconciliador.py:445), [justificativa.py:266](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/services/justificativa.py:266); `aval.txt`: [não verificado])

3. **Literal.** R8 exige prova da segunda rodada sem alteração; a leitura de `atualizado_em` demonstra a razão para testá-la, não demonstra que o ciclo já está fechado. R9 exige um leitor efetivo para o contador; ele ainda não foi encontrado na cópia. R7 deve usar o mesmo predicado e seu custo precisa entrar na medição do item (1). ([cartorio.py:391](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/services/cartorio.py:391), [justificativa.py:289](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/services/justificativa.py:289); `aval.txt`: [não verificado])

4. **Exportada, dinheiro ou `!`.** Efeito dos novos testes e do leitor pretendido sobre exportada, dinheiro ou `!`: **[não verificado]**. O contador atual é uma leitura que devolve a quantidade de pares; a rejeição atual grava a justificativa. ([justificativa.py:281](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/services/justificativa.py:281), [justificativa.py:301](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/services/justificativa.py:301))

**Pergunta de lei:** quando `_APROVANDO` faz a retenção responder `justificativa_alheia`, a pendente apontada fica preservada por R7, como exceção a D3, ou é rejeitada por D3? ([reconciliador.py:452](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/chamados/reconciliador.py:452), [justificativa.py:266](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/services/justificativa.py:266); `aval.txt`: [não verificado])

AVAL-DEVOLVIDO: (1), (3)  
perguntas de lei: 1

## 19/11 02:00 · sessao 3f83936ddcb6 · E4C-F3-LAVRA-FOTO · construtor · origem=Code

Acrescentei a testemunha de comportamento e de SQL em [test_f3_lavra_foto_sem_decisao.py](/home/ronald/wt-codex-e4c-f3-lavra-foto-013922/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:271). A camada AST e seus testes permanecem intactos; a docstring registra o resíduo de cobertura ([arquivo:1](/home/ronald/wt-codex-e4c-f3-lavra-foto-013922/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:1)). [nao verificado]

A base é `django.test.TestCase`, seguindo a criação ORM de empresa, praça, posto, colaborador, disputa e pergunta do [teste vizinho](/home/ronald/wt-codex-e4c-f3-lavra-foto-013922/app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:29). O horizonte fixo UTC no ano 9998 fica além dos timestamps produzidos pelo relógio durante uma execução ordinária; o marco da pergunta e o instante do ato também são fixos ([fixture:271](/home/ronald/wt-codex-e4c-f3-lavra-foto-013922/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:271)). [nao verificado]

Os testes novos são: [nao verificado]

- `test_corte_decisao_posterior_nao_move_marcos`: compara antes e depois da inserção para os dois horizontes ([arquivo:298](/home/ronald/wt-codex-e4c-f3-lavra-foto-013922/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:298)).
- `test_sql_marcos_nao_le_decisao`: verifica a ausência da tabela da decisão e a presença da tabela da pergunta no SQL capturado ([arquivo:312](/home/ronald/wt-codex-e4c-f3-lavra-foto-013922/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:312)).
- `test_MORDE_captura_detecta_leitura_real_de_decisao`: controla o instrumento com uma consulta avaliada ([arquivo:324](/home/ronald/wt-codex-e4c-f3-lavra-foto-013922/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:324)).

Comando para a sessão executar a classe, inclusive após aplicar cada mutante do verificador:

```bash
PYTHONDONTWRITEBYTECODE=1 python manage.py test chamados.tests.test_f3_lavra_foto_sem_decisao.LavraFotoComBancoTest
```

A execução dos testes e a mordida nos mutantes são **[não verificado]**. Não executei testes, conforme solicitado; a sessão aplica os mutantes no arquivo real. [nao verificado]

## 19/11 02:00 · sessao acd426f3b1aa · CHECK-DE-TELA · sonda · origem=Code

Não consegui produzir uma sonda confiável para as cinco telas. A referência **não foi medida**, e não tenho confirmação de que a cópia corresponda ao hash informado: o diretório disponível não contém metadados Git verificáveis. [não verificado]

O impedimento concreto é que o envelope `--sombra` usa a role `leitor`, sem permissão de escrita. Uma transação com rollback não concede essa permissão: um GET que tente escrever pode falhar ou seguir outro caminho de erro, portanto não se pode tratar sua duração como a medição normal da tela. [bin/sonda_leitura.sh:4](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/bin/sonda_leitura.sh:4) [bin/sonda_leitura.sh:11](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/bin/sonda_leitura.sh:11)

O rastreio parcial encontrou:

- **Espelho:** `/ponto/espelho/` chama `espelho_ponto`, que chama `espelho_do_colab`. A rota de PDF avulso encontrada exige staff e gera o PDF por POST; não comprovei que seja a tela do colaborador pedida. Escritas indiretas do GET: [não verificado]. [app/config/urls.py:29](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/config/urls.py:29) [app/ponto/urls.py:10](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/urls.py:10) [app/ponto/views.py:160](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/views.py:160) [app/relatorios/views.py:1050](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/relatorios/views.py:1050)
- **Tarefa do dia:** existe o serviço `tarefa_do_dia(colab, dia_do_turno, agora)`, mas não comprovei a rota de tela nem o universo que ela mostra. Escritas indiretas: [não verificado]. [app/chamados/services/tarefa_do_dia.py:738](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/services/tarefa_do_dia.py:738)
- **Fila do admin:** `/chamados/` chama `painel_gestao`, que chama `montar_contexto_painel` e, na página completa, `pendentes_na_central`. Escritas indiretas do GET: [não verificado]. [app/config/urls.py:28](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/config/urls.py:28) [app/chamados/urls.py:9](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/urls.py:9) [app/chamados/views.py:721](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/views.py:721) [app/chamados/views.py:759](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/views.py:759)
- **Placar:** `/chamados/placar/` chama `placar_json`; conforme filtro e disponibilidade do resultado lavrado, lê `PlacarNavbar` ou chama `montar_contexto_painel`. O próprio código documenta escrita condicional ao relavrar perguntas desatualizadas; o estado da sombra atual é [não verificado]. [app/chamados/urls.py:24](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/urls.py:24) [app/chamados/views.py:781](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/views.py:781) [app/chamados/views.py:840](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/views.py:840)
- **Consulta do app:** `/api/me/` chama `api_me`, que consulta, entre outros leitores, `turno_aberto_de` e `projecao_do_app`. Que seja a consulta pedida na abertura e que seus leitores indiretos escrevam ou não: [não verificado]. [app/api/urls.py:64](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/api/urls.py:64) [app/api/views.py:284](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/api/views.py:284) [app/api/views.py:306](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/api/views.py:306) [app/api/views.py:315](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/api/views.py:315)

Também há um GET de atendimento do colaborador que pode registrar auditoria, mas não comprovei que ele seja a tela “tarefa do dia” solicitada. [app/chamados/views.py:559](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/views.py:559) [app/chamados/views.py:583](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/views.py:583)

Ficaram pendentes o script, a verificação completa das escritas e os universos para as duas cargas. Não tenho resultados TSV a apresentar. [não verificado]

perguntas de lei: 0

## 19/11 01:56 · sessao 40d3f8bb6fa7 · E4C-F5 · censo · origem=Code

| arquivo:linha | função | campo | receptor | tipo | classe | trecho literal curto |
|---|---|---|---|---|---|---|
| chamados/management/commands/estoque_l130_instante_sem_texto.py:60 | handle | validada_em | PerguntaDisputa | rótulo/metadado sem efeito | ii | `'validada_em': None` |
| chamados/models.py:3031 | _nascer | respondida_em | PerguntaDisputa | leitura | ii | `campos.get('respondida_em')` |
| chamados/models.py:3032 | _nascer | resposta_colab | PerguntaDisputa | leitura | ii | `campos.get('resposta_colab')` |
| chamados/models.py:3324 | carimbar | materializada_em, materializada_por, via_resolucao | PerguntaDisputa | escrita | ii | `campos = ['materializada_em', …]` |
| chamados/models.py:3329 | carimbar | respondida_em | PerguntaDisputa | escrita | ii | `campos.append('respondida_em')` |
| chamados/models.py:3333 | carimbar | validada_em, validada_por | PerguntaDisputa | escrita | ii | `campos += ['validada_em', 'validada_por']` |
| chamados/models.py:3382 | carimbar_identidade | via_resolucao, materializada_em, materializada_por | PerguntaDisputa | escrita | ii | `campos = ['via_resolucao', …]` |
| chamados/models.py:3406 | descarimbar | via_resolucao, materializada_em, materializada_por | PerguntaDisputa | escrita | ii | `campos = ['via_resolucao', …]` |
| chamados/models.py:3417 | escopo da classe | respondida_em, resposta_colab, validada_em, materializada_em, via_resolucao, data_evento | PerguntaDisputa | leitura | iii | `CAMPOS_QUE_ACORDAM` |
| chamados/models.py:3454 | descarimbar_em_lote | materializada_em, materializada_por, materializada_por_id, via_resolucao | PerguntaDisputa | leitura | ii | `set(outros) & {...}` |
| chamados/models.py:3459 | descarimbar_em_lote | respondida_em, resposta_colab | PerguntaDisputa | leitura | ii | `& set(outros)` |
| chamados/models.py:3464 | descarimbar_em_lote | respondida_em, resposta_colab | PerguntaDisputa | leitura | ii | `outros['respondida_em']` |
| chamados/models.py:3465 | descarimbar_em_lote | respondida_em | PerguntaDisputa | leitura | ii | `'respondida_em' in _par` |
| chamados/models.py:3469 | descarimbar_em_lote | respondida_em | PerguntaDisputa | leitura | ii | `outros['respondida_em']` |
| chamados/models.py:3472 | descarimbar_em_lote | resposta_colab | PerguntaDisputa | leitura | ii | `outros['resposta_colab']` |
| chamados/models.py:3557 | q_validada_pelo_ato | validada_em | PerguntaDisputa | leitura | vi | `OuterRef('validada_em')` |
| chamados/models.py:3558 | q_validada_pelo_ato | validada_em | PerguntaDisputa | leitura | vi | `ato_em__gt=OuterRef('validada_em')` |
| chamados/models.py:3690 | escopo da classe | respondida_em, resposta_colab | PerguntaDisputa | leitura | ii | `_PAR_DA_RESPOSTA` |
| chamados/services/disputa_emissao.py:1483 | responder_pergunta_disputa_core | resposta_colab, respondida_em | PerguntaDisputa | escrita | ii | `_campos = ['resposta_colab', 'respondida_em']` |
| chamados/services/disputa_emissao.py:1496 | responder_pergunta_disputa_core | validada_em, validada_por | PerguntaDisputa | escrita | ii | `_campos += ['validada_em', 'validada_por']` |
| chamados/services/disputa_emissao.py:1545 | responder_pergunta_disputa_core | validada_em, validada_por | PerguntaDisputa | rótulo/metadado sem efeito | v | `antes={'validada_em': …}` |
| chamados/services/disputa_emissao.py:1546 | responder_pergunta_disputa_core | validada_em, validada_por | PerguntaDisputa | rótulo/metadado sem efeito | v | `depois={'validada_em': None, …}` |
| chamados/services/disputa_emissao.py:1685 | responder_automaticamente | resposta_colab, respondida_em | PerguntaDisputa | escrita | ii | `p.save(update_fields=[…])` |
| chamados/services/disputa_emissao.py:1707 | carimbar_resposta_pela_trilha | respondida_em | PerguntaDisputa | escrita | ii | `update_fields=['respondida_em']` |
| chamados/services/disputa_emissao.py:1713 | carimbar_resposta_pela_trilha | respondida_em | PerguntaDisputa | rótulo/metadado sem efeito | v | `depois={'respondida_em': …}` |
| chamados/services/disputa_emissao.py:2094 | _resolver_perguntas_por_chamado_resolvido | materializada_em, via_resolucao | PerguntaDisputa | rótulo/metadado sem efeito | v | `_antes = {'materializada_em': None, …}` |
| chamados/services/foto_e4b.py:180 | nascimentos | materializada_em, respondida_em, validada_em | PerguntaDisputa | leitura | iii | `a[c] is not None and d[c] is None` |
| chamados/services/historico.py:99 | _eventos_dos_chamados | respondida_em | PerguntaDisputa | leitura | v | `('respondida_em', 'pergunta_respondida', …)` |
| chamados/services/historico.py:101 | _eventos_dos_chamados | validada_em | PerguntaDisputa | leitura | v | `('validada_em', 'pergunta_validada', …)` |
| chamados/services/historico.py:103 | _eventos_dos_chamados | materializada_em | PerguntaDisputa | leitura | v | `('materializada_em', 'pergunta_materializada', …)` |
| chamados/services/materializacao.py:1689 | _carimbar_recusa | materializada_em, via_resolucao, recusa_motivo | PerguntaDisputa | leitura | ii | `p.refresh_from_db(fields=[…])` |
| chamados/services/painel.py:67 | marcador_do_placar | respondida_em, materializada_em, validada_em | PerguntaDisputa | leitura | vi | `Max('respondida_em')` |
| chamados/services/passivo_validado.py:64 | escopo do módulo | materializada_em, materializada_por_id, via_resolucao | PerguntaDisputa | leitura | ii | `CAMPOS_REVERSAO = (…)` |
| chamados/services/passivo_validado.py:66 | escopo do módulo | respondida_em, validada_em, validada_por_id, recusa_motivo | PerguntaDisputa | leitura | ii | `'respondida_em', 'validada_em'` |
| chamados/services/passivo_validado.py:535 | _plantar_o_dia | materializada_em, via_resolucao, materializada_por | PerguntaDisputa | leitura | ii | `p.refresh_from_db(fields=[…])` |
| chamados/services/regularizacao_ext.py:201 | materializar_regularizacao_externa | status, materializada_em, materializada_por | outro modelo: RegularizacaoExterna | escrita | — | `reg.save(update_fields=[…])` |
| chamados/services/relavra_via.py:252 | desfazer_carimbo_sem_lastro | materializada_em, via_resolucao | PerguntaDisputa | rótulo/metadado sem efeito | v | `depois={'materializada_em': None, …}` |
| chamados/services/tarefa_do_dia.py:418 | VistaNoHorizonte.__getattr__ | respondida_em, validada_em | PerguntaDisputa | leitura | iii | `valor > self._agora` |
| chamados/services/validacao.py:171 | _carimbar_validacao | validada_em, validada_por | PerguntaDisputa | escrita | ii | `perg.save(update_fields=[…])` |
| chamados/services/validacao.py:262 | devolver_validacao_recusada | validada_em, validada_por, materializada_em, recusa_motivo | PerguntaDisputa | leitura | i | `perg.refresh_from_db(fields=[…])` |
| chamados/services/validacao.py:263 | devolver_validacao_recusada | resposta_colab, tipo_resposta | PerguntaDisputa | leitura | ii | `'resposta_colab', 'tipo_resposta'` |
| chamados/services/validacao.py:276 | devolver_validacao_recusada | validada_em, validada_por | PerguntaDisputa | escrita | ii | `update_fields=['validada_em', 'validada_por']` |
| chamados/services/validacao.py:300 | devolver_validacao_recusada | validada_em, validada_por_id | PerguntaDisputa | rótulo/metadado sem efeito | v | `antes={'validada_em': …}` |
| chamados/services/validacao.py:302 | devolver_validacao_recusada | validada_em, validada_por_id, recusa_motivo | PerguntaDisputa | rótulo/metadado sem efeito | v | `depois={'validada_em': None, …}` |
| chamados/services/validacao.py:351 | devolver_paridade_bug_par_103 | validada_em, validada_por | PerguntaDisputa | escrita | ii | `update_fields=campos + […]` |
| chamados/services/validacao.py:363 | devolver_paridade_bug_par_103 | materializada_em, via_resolucao, materializada_por_id | PerguntaDisputa | rótulo/metadado sem efeito | v | `depois={'materializada_em': None, …}` |
| chamados/services/validacao.py:364 | devolver_paridade_bug_par_103 | validada_em, validada_por_id, recusa_motivo | PerguntaDisputa | rótulo/metadado sem efeito | v | `'validada_em': None` |
| chamados/services/validacao.py:453 | devolver_instante_sem_texto_l130 | respondida_em, resposta_colab, via_resolucao | PerguntaDisputa | rótulo/metadado sem efeito | v | `depois={'respondida_em': None, …}` |
| chamados/services/validacao.py:576 | _aceitar_a_folga | validada_em, validada_por | PerguntaDisputa | leitura | ii | `perg.refresh_from_db(fields=[…])` |
| chamados/services/validacao.py:789 | validar_pergunta | validada_em, materializada_em | PerguntaDisputa | leitura | v | `perg.refresh_from_db(fields=[…])` |
| chamados/services/validacao.py:932 | recusar_pergunta | resposta_colab | PerguntaDisputa | leitura | v | `antes['resposta_colab']` |
| chamados/services/validacao.py:937 | recusar_pergunta | respondida_em, resposta_colab | PerguntaDisputa | rótulo/metadado sem efeito | v | `'respondida_em': None, 'resposta_colab': ''` |
| chamados/services/verbos.py:344 | placar_aceite | respondida_em | PerguntaDisputa | leitura | v | `pg['respondida_em'] > cob_por_colab[cid]` |
| chamados/services/verbos.py:347 | placar_aceite | respondida_em | PerguntaDisputa | leitura | v | `_tz.localtime(pg['respondida_em'])` |
| chamados/veredito_lavrado.py:204 | marcos_do_lote | materializada_em | PerguntaDisputa | leitura | iv | `annotate(m=Max('materializada_em'))` |
| chamados/veredito_lavrado.py:208 | marcos_do_lote | respondida_em | PerguntaDisputa | leitura | iv | `annotate(m=Max('respondida_em'))` |

linhas: 56 de 56

Totais por classe: i: 1; ii: 28; iii: 3; iv: 2; v: 18; vi: 3. Outro modelo: 1.

- **i — chamados/services/validacao.py:262:** `validar_pergunta` chama `devolver_validacao_recusada` em chamados/services/validacao.py:781. Sim: a leitura de `validada_em` participa da decisão de quando a própria porta escreve, pela condição em chamados/services/validacao.py:270 e pelo `save` em chamados/services/validacao.py:276.

perguntas de lei: 0

## 19/11 01:55 · sessao d86cb0ea8a17 · E4D · sonda · origem=Code

A limpeza L130 grava a ação `l130_instante_sem_texto_devolvida` para `PerguntaDisputa` e preserva, quando necessário, a resposta anterior como `RESPOSTA`; ela não grava `REABERTURA` nem `DEVOLUCAO`. A escolha entre as três opções de D3 permanece **[nao verificado]**. [app/chamados/services/validacao.py:414](app/chamados/services/validacao.py#L414), [app/chamados/services/validacao.py:436](app/chamados/services/validacao.py#L436), [app/chamados/services/validacao.py:449](app/chamados/services/validacao.py#L449).

**A — Escrita e leitura**

As três opções ainda exigem definir seus escritores: o código atual da L130 não escreve nenhum dos três atos propostos, e os sítios de backfill de um ato ainda não definido são **[nao verificado]**. A porta comum atual para inserir decisões é `DecisaoPergunta.registrar`; além da L130, os backfills existentes a usam para registrar linhas da projeção, e um deles também registra `SILENCIO`. [app/chamados/models.py:4329](app/chamados/models.py#L4329), [app/chamados/management/commands/backfill_fio_morto_e4b.py:141](app/chamados/management/commands/backfill_fio_morto_e4b.py#L141), [app/chamados/management/commands/backfill_decisoes_pergunta.py:324](app/chamados/management/commands/backfill_decisoes_pergunta.py#L324).

| Opção | Comportamento comprovado dos leitores atuais |
|---|---|
| **1. Novo valor de `ato`** | `DecisaoPergunta.leitura` o ignora: só `RESPOSTA` e `VALIDACAO` atribuem valores, e os conjuntos de limpeza não incluem um novo valor. A resposta anterior continuaria na leitura. `respostas_anteriores` também o ignora. A tarefa do dia, se ele for o último ato considerado, retorna `ato_desconhecido:<ato>` e nenhuma ação. [app/chamados/models.py:4277](app/chamados/models.py#L4277), [app/chamados/models.py:4317](app/chamados/models.py#L4317), [app/chamados/models.py:3800](app/chamados/models.py#L3800), [app/chamados/services/tarefa_do_dia.py:158](app/chamados/services/tarefa_do_dia.py#L158), [app/chamados/services/tarefa_do_dia.py:187](app/chamados/services/tarefa_do_dia.py#L187). |
| **2. Valor existente com marca** | A leitura atual decide pelo valor de `ato`, sem consultar uma marca L130; portanto, o efeito é o do valor escolhido. A tarefa do dia só conhece a exclusão especial da marca `da_ressuscitacao`, que está restrita a `REABERTURA`; não há comportamento comprovado para uma nova marca L130. [app/chamados/models.py:4317](app/chamados/models.py#L4317), [app/chamados/models.py:4208](app/chamados/models.py#L4208), [app/chamados/services/tarefa_do_dia.py:421](app/chamados/services/tarefa_do_dia.py#L421). |
| **3. Saneamento separado** | O escritor, o formato, a ordem em relação às decisões e o consumo pelo projetor são **[nao verificado]**. O leitor atual só monta sua sequência com `p.decisoes` e o passivo da projeção; um registro separado que não entre nessa sequência não muda essa leitura hoje. [app/chamados/models.py:4308](app/chamados/models.py#L4308). |

Na opção 2, os efeitos dos valores existentes na leitura dos três campos são estes: [app/chamados/models.py:4272](app/chamados/models.py#L4272), [app/chamados/models.py:4317](app/chamados/models.py#L4317).

| Valor candidato | Efeito atual e consequência para a limpeza |
|---|---|
| `REABERTURA` ou `RECUSA` | Limpam resposta **e validação** na leitura; a tarefa do dia os classifica como `devolvida_ao_colaborador`, com ação de responder. A tela de respostas anteriores inclui a linha de foto. A porta normal de reabertura também zera `validada_em` e `validada_por`, além do par e carimbo; a elegibilidade L130 exige `validada_em` e `materializada_em` nulos. O efeito de todos os demais campos e efeitos colaterais de chamar essa porta em vez da limpeza L130 é **[nao verificado]**. [app/chamados/models.py:4277](app/chamados/models.py#L4277), [app/chamados/services/tarefa_do_dia.py:163](app/chamados/services/tarefa_do_dia.py#L163), [app/chamados/models.py:3806](app/chamados/models.py#L3806), [app/chamados/services/disputa_emissao.py:1617](app/chamados/services/disputa_emissao.py#L1617), [app/chamados/services/validacao.py:392](app/chamados/services/validacao.py#L392). |
| `DEVOLUCAO` | Limpa a validação, mas **não** a resposta, na leitura; como último ato, a tarefa do dia pede validação pelo admin. Uma `DEVOLUCAO` com motivo específico `BUG-PAR-103` também é reconhecida pelo leitor desse selo. [app/chamados/models.py:4277](app/chamados/models.py#L4277), [app/chamados/services/tarefa_do_dia.py:165](app/chamados/services/tarefa_do_dia.py#L165), [app/chamados/services/materializacao.py:267](app/chamados/services/materializacao.py#L267). |
| `RESPOSTA` | Instala instante e texto; pode limpar validação conforme `resposta_mantem_validacao`. A tarefa distingue texto vazio de texto preenchido. Não representa, por si, a retirada da resposta. [app/chamados/models.py:4280](app/chamados/models.py#L4280), [app/chamados/models.py:4318](app/chamados/models.py#L4318), [app/chamados/services/tarefa_do_dia.py:159](app/chamados/services/tarefa_do_dia.py#L159). |
| `VALIDACAO` | Instala validação; a tarefa avalia a lavra e o efeito. Não representa a retirada da resposta. [app/chamados/models.py:4320](app/chamados/models.py#L4320), [app/chamados/services/tarefa_do_dia.py:167](app/chamados/services/tarefa_do_dia.py#L167). |
| `SILENCIO`, `RETRATACAO`, `ABSORCAO` | Não alteram os três campos da leitura. A tarefa atribui significado próprio a silêncio e absorção e trata retratação em outro ponto; `ABSORCAO` exige as duas ligações previstas na restrição do modelo. [app/chamados/models.py:4276](app/chamados/models.py#L4276), [app/chamados/services/tarefa_do_dia.py:176](app/chamados/services/tarefa_do_dia.py#L176), [app/chamados/services/tarefa_do_dia.py:260](app/chamados/services/tarefa_do_dia.py#L260), [app/chamados/models.py:4204](app/chamados/models.py#L4204). |

A face SQL da validação usa os mesmos conjuntos de limpeza e precisaria refletir uma mudança que alterasse a validação; as telas e propriedades que chamam `leitura` recebem o resultado dela. O inventário **exaustivo** dos demais leitores, selos e comandos de auditoria, bem como seu comportamento individual, permanece **[nao verificado]**. [app/chamados/models.py:3539](app/chamados/models.py#L3539), [app/chamados/models.py:3551](app/chamados/models.py#L3551), [app/chamados/models.py:4299](app/chamados/models.py#L4299).

**B — Choice e contagem**

O choice atual contém oito valores: `resposta`, `validacao`, `devolucao`, `recusa`, `reabertura`, `silencio`, `retratacao` e `absorcao`. [app/chamados/models.py:4152](app/chamados/models.py#L4152).

O número **total** de leitores com match exaustivo é **[nao verificado]**. Um ponto comprovado que precisa ser revisto para um novo ato é `_pela_decisao`: ele possui ramos para os valores que trata e devolve `ato_desconhecido` para o restante; `RETRATACAO` também é tratada antes, no chamador. Isso não prova que seja o único ponto a migrar. [app/chamados/services/tarefa_do_dia.py:142](app/chamados/services/tarefa_do_dia.py#L142), [app/chamados/services/tarefa_do_dia.py:187](app/chamados/services/tarefa_do_dia.py#L187), [app/chamados/services/tarefa_do_dia.py:259](app/chamados/services/tarefa_do_dia.py#L259).

**C — Sonda de frota**

A sonda abaixo conta perguntas distintas encontradas pelo log, por empresa atual; separa explicitamente pergunta ausente e competência sem dia. Para divergência, exige que a resposta vigente reconstruída seja uma linha `RESPOSTA` anterior ou simultânea ao último log L130 da pergunta **e** que o par reconstruído divirja do par atual da coluna. Isso mede o estado atual; não prova a divergência imediatamente após uma limpeza se houve alterações posteriores. A ordem de atos usada por `leitura` é `(ato_em, pk)`, com passivo adicional da projeção. [app/core/models.py:295](app/core/models.py#L295), [app/chamados/services/validacao.py:449](app/chamados/services/validacao.py#L449), [app/chamados/models.py:4295](app/chamados/models.py#L4295).

O juiz é `materializacao.competencia_do_fato`, o mesmo chamado pela elegibilidade L130; ele compõe a janela do dia do fato com `folha.export.competencia_entregue` e devolve `None` sem dia. Os resultados de produção são **[nao verificado]**: a sonda não foi executada. [app/chamados/services/validacao.py:400](app/chamados/services/validacao.py#L400), [app/chamados/services/materializacao.py:788](app/chamados/services/materializacao.py#L788).

```python
import collections
import os
import sys

from django.conf import settings
from django.db import connection, transaction

from chamados.models import DecisaoPergunta, PerguntaDisputa
from chamados.services.materializacao import competencia_do_fato
from core.models import LogAuditoria


ACAO = "l130_instante_sem_texto_devolvida"
MODELO = "PerguntaDisputa"


def guarda_sombra():
    if os.environ.get("DJANGO_SETTINGS_MODULE") != "config.settings.sombra":
        raise RuntimeError("settings")
    banco = settings.DATABASES["default"]
    if banco.get("NAME") != "sombra" or banco.get("HOST") != "sombra_db":
        raise RuntimeError("configuracao")
    with connection.cursor() as cursor:
        cursor.execute("SET TRANSACTION READ ONLY")
        cursor.execute("SELECT current_database(), current_schema()")
        banco_real, schema_real = cursor.fetchone()
    if banco_real != "sombra" or schema_real != "juliani":
        raise RuntimeError("conexao")


def main():
    etapa = "GUARDA_SOMBRA"
    try:
        with transaction.atomic():
            guarda_sombra()

            etapa = "LOGS"
            logs = list(
                LogAuditoria.objects.filter(acao=ACAO, modelo=MODELO)
                .order_by("objeto_id", "criado_em", "pk")
                .values_list("objeto_id", "criado_em", "pk")
            )
            ultimo_log = {}
            for pergunta_id, criado_em, log_id in logs:
                if pergunta_id is None:
                    raise RuntimeError("log_sem_objeto")
                ultimo_log[pergunta_id] = (criado_em, log_id)

            etapa = "PERGUNTAS"
            perguntas = list(
                PerguntaDisputa.objects.filter(pk__in=ultimo_log)
                .select_related("disputa__colaborador__empresa")
                .prefetch_related("decisoes")
                .order_by("pk")
            )
            por_pk = {p.pk: p for p in perguntas}
            ausentes = set(ultimo_log) - set(por_pk)

            universo = collections.Counter()
            divergencia = collections.Counter()
            competencia = collections.Counter()
            exemplos = collections.defaultdict(list)

            etapa = "CLASSIFICACAO"
            for p in perguntas:
                colab = p.disputa.colaborador
                emp = colab.empresa_id
                universo[emp] += 1

                momento_log = ultimo_log[p.pk][0]
                leitura = DecisaoPergunta.leitura(p)
                resposta_real_anterior = any(
                    d.ato == DecisaoPergunta.RESPOSTA
                    and d.ato_em == leitura.respondida_em
                    and d.ato_em <= momento_log
                    for d in p.decisoes.all()
                )
                par_lido = (leitura.respondida_em, leitura.resposta_colab or "")
                par_coluna = (p.respondida_em, p.resposta_colab or "")
                diverge = resposta_real_anterior and par_lido != par_coluna
                divergencia[(emp, "sim" if diverge else "nao")] += 1

                k6 = competencia_do_fato(p, colab.empresa)
                estado = (
                    "sem_dia" if k6 is None
                    else "entregue" if k6[2]
                    else "nao_entregue"
                )
                competencia[(emp, estado)] += 1
                exemplos[(emp, "sim" if diverge else "nao", estado)].append(
                    (p.pk, colab.pk)
                )

            etapa = "SAIDA"
            linhas = [
                "LOGS registros=%d perguntas_distintas=%d"
                % (len(logs), len(ultimo_log)),
                "PERGUNTAS_AUSENTES=%d" % len(ausentes),
            ]
            for pk in sorted(ausentes):
                linhas.append("AUSENTE pergunta_pk=%d" % pk)

            total_universo = 0
            total_div = 0
            total_comp = 0
            for emp in sorted(universo, key=str):
                n = universo[emp]
                nd = sum(divergencia[(emp, x)] for x in ("sim", "nao"))
                nc = sum(
                    competencia[(emp, x)]
                    for x in ("entregue", "nao_entregue", "sem_dia")
                )
                total_universo += n
                total_div += nd
                total_comp += nc

                linhas.append(
                    "EMPRESA pk=%s limpas=%d divergentes=%d "
                    "nao_divergentes=%d entregues=%d nao_entregues=%d sem_dia=%d"
                    % (
                        emp, n,
                        divergencia[(emp, "sim")],
                        divergencia[(emp, "nao")],
                        competencia[(emp, "entregue")],
                        competencia[(emp, "nao_entregue")],
                        competencia[(emp, "sem_dia")],
                    )
                )
                linhas.append(
                    "CONFERENCIA emp=%s divergencia_contador=%d "
                    "competencia_contador=%d universo=%d contador==universo=%s"
                    % (emp, nd, nc, n, "s" if nd == n and nc == n else "n")
                )
                if nd != n or nc != n:
                    raise RuntimeError("soma_empresa")

            for (emp, div, estado), pares in sorted(
                exemplos.items(), key=lambda item: tuple(map(str, item[0]))
            ):
                for pergunta_pk, colab_pk in pares[:2]:
                    linhas.append(
                        "EXEMPLO emp=%s divergente=%s competencia=%s "
                        "pergunta_pk=%d col%d"
                        % (emp, div, estado, pergunta_pk, colab_pk)
                    )

            total_com_ausentes = total_universo + len(ausentes)
            confere = (
                total_div == total_universo
                and total_comp == total_universo
                and total_com_ausentes == len(ultimo_log)
            )
            linhas.append(
                "CONFERENCIA_TOTAL divergencia_contador=%d "
                "competencia_contador=%d universo_existente=%d ausentes=%d "
                "universo_logs=%d contador==universo=%s"
                % (
                    total_div, total_comp, total_universo, len(ausentes),
                    len(ultimo_log), "s" if confere else "n",
                )
            )
            if not confere:
                raise RuntimeError("soma_total")

        for linha in linhas:
            print(linha)
        print("ERROS=0")
        print("FIM_SONDA_D3_L130")
        return 0
    except Exception:
        print("ERRO " + etapa)
        print("ERROS=1")
        print("FIM_SONDA_D3_L130")
        return 1


sys.exit(main())
```

perguntas de lei: 2

1. Qual representação adotar para a limpeza reconstruível: **(1)** novo ato compensador, **(2)** ato existente com marca explícita, ou **(3)** saneamento separado consumido pelo projetor? A decisão atual é **[nao verificado]**. [app/chamados/services/validacao.py:414](app/chamados/services/validacao.py#L414).
2. Como tratar a reconstrução do histórico e o estoque em competência entregue: **(a)** reconstruir automaticamente, **(b)** tratar por procedimento específico, ou **(c)** outra regra? A decisão atual é **[nao verificado]**. [app/chamados/services/validacao.py:381](app/chamados/services/validacao.py#L381).

## 19/11 01:54 · sessao 69c8d1976b61 · E4C-F3-LAVRA-FOTO · verificador · origem=Code

**BLOQUEIA:** o selo ainda permite leituras de `DecisaoPergunta` dentro de funções alcançadas no próprio módulo. O fecho segue somente chamadas cujo alvo é um nome que coincide diretamente com uma função de topo; não resolve aliases, chamadas por atributo nem métodos de classes. [teste:49](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:49)

Executei o detector isolado sobre a fonte e contramutações **em memória**: os exemplos de escape abaixo retornaram `[]`, salvo onde indicado. A execução das consultas e da suíte Django é **[não verificado]**. O detector sobre a fonte atual também retornou `[]`. [teste:41](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:41)

### Cura dos itens da volta 1

| Item | Resultado |
|---|---|
| **Helper transitivo** | **Parcial.** M7 prova a chamada direta por nome, inclusive em dois níveis; alias de função e método de classe no mesmo módulo escapam. [detector:60](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:60), [M7:217](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:217) |
| **Reexportação** | **Parcial.** M8 importa um símbolo ainda chamado `DecisaoPergunta`; o registro só reconhece esse nome original. Uma reexportação já chamada `D` escapa. [detector:68](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:68), [M8:227](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:227) |
| **`getattr`/`get_model` e nomes montados** | **Parcial.** M9 morde `getattr` chamado diretamente com expressão não constante; um alias de `getattr` com o nome montado escapa. [detector:97](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:97), [M9:233](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:233) |
| **Prefetch, string, `**dict` e segundo relacionamento** | **Parcial.** M10 e M11 mordem os literais `decisoes` e `absorcoes_feitas__isnull`; um caminho montado por concatenação escapa. Os dois nomes vêm dos relacionamentos declarados no modelo. [detector:88](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:88), [M10–M11:239](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:239), [modelo:4166](app/chamados/models.py:4166), [modelo:4176](app/chamados/models.py:4176) |
| **Caminho ORM indireto e SQL bruto** | **Parcial.** M12 morde o nome literal da tabela; SQL montado em partes escapa. M11 demonstra um caminho ORM literal, não todos os caminhos montados. [detector:92](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:92), [M12:249](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:249) |

Contramutações concretas, inseridas em `marcos_do_lote`, salvo a definição do helper. Elas retornaram `[]` no detector isolado; a validade e execução das consultas contra um banco são **[não verificado]**. [produção:151](app/chamados/veredito_lavrado.py:151), [teste:109](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:109)

```python
# Helper definido no mesmo módulo; as duas últimas linhas vão em marcos_do_lote.
def _leitor(): return __import__('chamados.models', fromlist=['DecisaoPergunta']).DecisaoPergunta.objects.all()
leitura = _leitor
leitura()
```

A forma acima também contém acesso dinâmico no helper não visitado. Uma versão com import direto no helper igualmente retornou `[]`; é essa que demonstra a falha do fecho, independentemente do acesso dinâmico. [teste:60](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:60)

```python
from chamados.tests.test_e4c_f2_leitura_do_ato import D
D.objects.all()
```

`D` é uma reexportação existente de `DecisaoPergunta`. [origem:19](app/chamados/tests/test_e4c_f2_leitura_do_ato.py:19)

```python
import chamados.models as cm
g = getattr
g(cm, 'Decisao' + 'Pergunta').objects.all()
```

```python
from chamados.models import PerguntaDisputa as PD
list(PD.objects.filter(**{'absorcoes_' + 'feitas__isnull': False}))
```

```python
from django.db import connection
c = connection.cursor()
c.execute('SELECT * FROM chamados_' + 'decisao' + 'pergunta')
```

O último exemplo escapa porque `execute` sobre a variável `c` não corresponde ao formato especial `cursor().execute`, e nenhuma das constantes contém o nome completo procurado. [teste:92](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:92)

### Formas adicionais de alcance

- **Alias de helper ou lambda que o chama: escapa e cabe no contrato do módulo.** O helper pode ser uma função de topo de `veredito_lavrado.py`; `leitura = _leitor; leitura()` não faz o detector entrar nela. Uma lambda que invoque `_leitor()` diretamente é percorrida por `ast.walk` e essa chamada direta é seguida; o escape demonstrado é o alias da função. [teste:49](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:49)
- **Método de classe definida no próprio módulo: escapa e cabe no contrato.** Uma classe `Leitor` com método `ler` que importe e consulte `DecisaoPergunta`, seguida de `Leitor().ler()` no alvo, retornou `[]`: métodos de classe não entram no mapa de funções de topo. [teste:49](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:49)
- **Método de instância de modelo em outro módulo: escapa.** Após importar `PerguntaDisputa` como `PD`, `list(PD.objects.first().respostas_anteriores())` retornou `[]`; o método existente percorre `self.decisoes.all()`. Se uma chamada a esse método deve ser proibida por este contrato limitado ao fecho *do módulo* é **pergunta de lei**. A execução com uma pergunta existente é **[não verificado]**. [modelo:3793](app/chamados/models.py:3793), [teste:62](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:62)
- **Helper em outro módulo: escapa quanto ao corpo.** O detector monta o fecho apenas com funções do AST da fonte recebida; ampliar a obrigação para o corpo importado é a mesma pergunta de lei sobre fronteira de módulo. [teste:43](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:43), [teste:49](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:49)
- **Atributo encadeado com nome literal: não escapou no exemplo `f(cm).DecisaoPergunta.objects.all()`.** O detector reconhece o atributo pelo segmento `DecisaoPergunta`, mesmo com base encadeada. Isso não prova cobertura de atributos cujo nome seja construído dinamicamente. [teste:117](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:117)

### Falso positivo e classes de reprovação

Não encontrei falso positivo no trecho atual: o detector isolado retornou lista vazia para a fonte atual. Isso não prova ausência de falsos positivos em futuras alterações; por exemplo, ele trata nomes de atributos e strings que coincidam com segmentos protegidos sem verificar o tipo do objeto ou se a string é caminho ORM. [teste:92](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:92), [teste:117](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:117)

- **Selo sem mordida:** há mordida demonstrada pela lógica e pelos mutantes declarados, mas ainda é **parcial**; os escapes internos acima constituem um único achado bloqueante consolidado. A execução da suíte M1–M12 é **[não verificado]**. [teste:217](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:217)
- **Fonte única:** os `related_name` são lidos por AST diretamente de `models.py`, em vez de serem copiados como conjunto fixo no detector. Há, porém, o literal adicional `decisoes` no teste de controle e segmentos fixos para o nome do modelo; o extrator só contempla as formas sintáticas indicadas no código. [teste:16](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:16), [teste:89](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:89), [teste:212](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:212)
- **Campo com dois significados:** não há campo de produção novo no diff fornecido; existência dessa classe no restante do sistema é **[não verificado]**. [teste:1](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:1)
- **Alvo ausente:** o detector exige exatamente uma função de topo com o nome solicitado, e há teste para ausência. [teste:44](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:44), [teste:151](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:151)

A identidade Git da cópia com o hash informado é **[não verificado]**: o diretório consultado não tem metadados Git. A correspondência do diff completo com a base também é **[não verificado]**.

BLOQUEIA: 1  
perguntas de lei: 1

## 19/11 01:52 · sessao 5f05c9931c97 · E4C-F5 · censo · origem=Code

| arquivo:linha | função | campo | receptor | classe | trecho literal curto |
|---|---|---|---|---|---|
| chamados/services/painel.py:270 | montar_contexto_painel._jan_val | validada_em | PerguntaDisputa | vi | `qs.filter(validada_em__gte=_hoje_ini)` |
| chamados/services/painel.py:271 | montar_contexto_painel._jan_val | validada_em | PerguntaDisputa | vi | `validada_em__gte=_agora_inf` |
| chamados/services/painel.py:272 | montar_contexto_painel._jan_val | validada_em | PerguntaDisputa | vi | `validada_em__gte=_agora_inf` |
| chamados/services/painel.py:273 | montar_contexto_painel._jan_val | validada_em | PerguntaDisputa | vi | `validada_em__gte=_agora_inf` |
| chamados/services/passivo_validado.py:141 | _balde_da_recusa | via_resolucao | PerguntaDisputa | v | `p.via_resolucao != antes_via` |
| chamados/services/passivo_validado.py:142 | _balde_da_recusa | via_resolucao | PerguntaDisputa | v | `return 'via:%s' % p.via_resolucao` |
| chamados/services/passivo_validado.py:530 | _plantar_o_dia | via_resolucao | PerguntaDisputa | iv | `antes_via = [], [], p.via_resolucao` |
| chamados/services/passivo_validado.py:534 | _plantar_o_dia | materializada_em | PerguntaDisputa | ii | `_sem_carimbo_antes = p.materializada_em is None` |
| chamados/services/passivo_validado.py:536 | _plantar_o_dia | materializada_em | PerguntaDisputa | ii | `_sem_carimbo_antes and p.materializada_em is not None` |
| chamados/services/raiox.py:135 | creditos_perguntas | materializada_em | PerguntaDisputa | vi | `_vivas.exclude(materializada_em__isnull=False` |
| chamados/services/raiox.py:138 | creditos_perguntas | respondida_em | PerguntaDisputa | vi | `'respondida_em', 'resposta_colab'` |
| chamados/services/raiox.py:138 | creditos_perguntas | resposta_colab | PerguntaDisputa | vi | `'respondida_em', 'resposta_colab'` |
| chamados/services/raiox.py:147 | creditos_perguntas | respondida_em | indeterminado: argumento de `SimpleNamespace`, alimentado por tupla | — | `respondida_em=p[3]` |
| chamados/services/raiox.py:147 | creditos_perguntas | resposta_colab | indeterminado: argumento de `SimpleNamespace`, alimentado por tupla | — | `resposta_colab=p[4]` |
| chamados/services/raiox.py:182 | pergunta_sem_acao_possivel | materializada_em | PerguntaDisputa | iii | `if p.materializada_em is None` |
| chamados/services/regularizacao_ext.py:220 | retomar_regularizacoes_confirmadas | materializada_em | outro modelo: RegularizacaoExterna | — | `status='confirmada', materializada_em__isnull=True` |
| chamados/services/relavra_via.py:118 | relavrar | via_resolucao | PerguntaDisputa | ii | `p.via_resolucao not in VIAS_DERIVADAS` |
| chamados/services/relavra_via.py:121 | relavrar | via_resolucao | PerguntaDisputa | ii | `nova == p.via_resolucao` |
| chamados/services/relavra_via.py:123 | relavrar | via_resolucao | PerguntaDisputa | iv | `antes = {'via_resolucao': p.via_resolucao` |
| chamados/services/relavra_via.py:155 | relavrar_dia | via_resolucao | PerguntaDisputa | vi | `via_resolucao__in=VIAS_DERIVADAS` |
| chamados/services/relavra_via.py:181 | q_carimbo_desfazivel | materializada_em | PerguntaDisputa | vi | `Q(materializada_em__isnull=False)` |
| chamados/services/relavra_via.py:181 | q_carimbo_desfazivel | via_resolucao | PerguntaDisputa | vi | `~Q(via_resolucao__in=PerguntaDisputa.VIAS_DE_IDENTIDADE)` |
| chamados/services/relavra_via.py:244 | desfazer_carimbo_sem_lastro | materializada_em | PerguntaDisputa | iv | `antes = {'materializada_em': str(p.materializada_em)` |
| chamados/services/relavra_via.py:245 | desfazer_carimbo_sem_lastro | via_resolucao | PerguntaDisputa | iv | `'via_resolucao': p.via_resolucao` |
| chamados/services/tarefa_do_dia.py:163 | _pela_decisao | validada_em | DecisaoPergunta/ato: LeituraDoAto | — | `leitura.validada_em is not None` |
| chamados/services/tarefa_do_dia.py:262 | _tarefa_pura | via_resolucao | PerguntaDisputa | iii | `p.via_resolucao in PerguntaDisputa.VIAS_FORA_DO_FATO` |
| chamados/services/tarefa_do_dia.py:263 | _tarefa_pura | via_resolucao | PerguntaDisputa | iii | `p.via_resolucao == PerguntaDisputa.VIA_MOTIVO_RETRATADO` |
| chamados/services/validacao.py:173 | _carimbar_validacao | validada_em | PerguntaDisputa | iv | `ato_em=perg.validada_em` |
| chamados/services/validacao.py:175 | _carimbar_validacao | resposta_colab | PerguntaDisputa | iv | `ato_texto=perg.resposta_colab or ''` |
| chamados/services/validacao.py:181 | _foto_da_validacao | validada_em | PerguntaDisputa | iv | `return (perg.validada_em, perg.validada_por_id` |
| chamados/services/validacao.py:264 | devolver_validacao_recusada | materializada_em | PerguntaDisputa | ii | `perg.materializada_em is not None` |
| chamados/services/validacao.py:269 | devolver_validacao_recusada | validada_em | PerguntaDisputa | ii | `if perg.validada_em is not None` |
| chamados/services/validacao.py:281 | devolver_validacao_recusada | resposta_colab | PerguntaDisputa | iv | `ato_texto=perg.resposta_colab or ''` |
| chamados/services/validacao.py:341 | devolver_paridade_bug_par_103 | materializada_em | PerguntaDisputa | iv | `antes = {'materializada_em': str(perg.materializada_em)` |
| chamados/services/validacao.py:341 | devolver_paridade_bug_par_103 | via_resolucao | PerguntaDisputa | iv | `'via_resolucao': perg.via_resolucao` |
| chamados/services/validacao.py:343 | devolver_paridade_bug_par_103 | validada_em | PerguntaDisputa | iv | `'validada_em': perg.validada_em and str(perg.validada_em)` |
| chamados/services/validacao.py:356 | devolver_paridade_bug_par_103 | resposta_colab | PerguntaDisputa | iv | `ato_texto=perg.resposta_colab or ''` |
| chamados/services/validacao.py:392 | impedimento_estoque_l130 | validada_em | PerguntaDisputa | ii | `if perg.validada_em is not None` |
| chamados/services/validacao.py:394 | impedimento_estoque_l130 | materializada_em | PerguntaDisputa | ii | `if perg.materializada_em is not None` |
| chamados/services/validacao.py:434 | devolver_instante_sem_texto_l130 | respondida_em | PerguntaDisputa | iv | `antes = {'respondida_em': str(perg.respondida_em)` |
| chamados/services/validacao.py:434 | devolver_instante_sem_texto_l130 | resposta_colab | PerguntaDisputa | iv | `'resposta_colab': perg.resposta_colab` |
| chamados/services/validacao.py:435 | devolver_instante_sem_texto_l130 | via_resolucao | PerguntaDisputa | iv | `'via_resolucao': perg.via_resolucao` |
| chamados/services/validacao.py:440 | devolver_instante_sem_texto_l130 | respondida_em | PerguntaDisputa | ii | `descarimbar_em_lote(alvo, respondida_em=None` — argumento de escrita |
| chamados/services/validacao.py:440 | devolver_instante_sem_texto_l130 | resposta_colab | PerguntaDisputa | ii | `resposta_colab=''` — argumento de escrita |
| chamados/services/validacao.py:467 | _e_aceite_de_folga | resposta_colab | PerguntaDisputa | ii | `(perg.resposta_colab or '').strip() == RESPOSTA_DE_FOLGA` |

linhas: 45 de 45

Totais por classe: i **0**; ii **11**; iii **3**; iv **15**; v **2**; vi **10**. Quatro linhas não recebem classe: uma leitura de `LeituraDoAto`, uma de outro modelo e dois argumentos de `SimpleNamespace`.

perguntas de lei: 0

## 19/11 01:52 · sessao 153b592be7b0 · E4C-F5 · censo · origem=Code

| arquivo:linha | função | campo | receptor | classe | trecho literal curto |
|---|---|---|---|---|---|
| app/chamados/services/validacao.py:640 | _validar_o_par | validada_em | PerguntaDisputa | i | `if q.validada_em is None` |
| app/chamados/services/validacao.py:643 | _validar_o_par | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=False` |
| app/chamados/services/validacao.py:685 | validar_pergunta | validada_em | PerguntaDisputa | i | `if perg.validada_em` |
| app/chamados/services/validacao.py:769 | validar_pergunta | materializada_em | PerguntaDisputa | ii | `if perg.materializada_em is None` |
| app/chamados/services/validacao.py:805 | validar_pergunta | materializada_em | PerguntaDisputa | v | `erros and perg.materializada_em is None` |
| app/chamados/services/validacao.py:898 | recusar_pergunta | validada_em | PerguntaDisputa | i | `if perg.validada_em` |
| app/chamados/services/validacao.py:912 | recusar_pergunta | resposta_colab | PerguntaDisputa | iv | `'resposta_colab': perg.resposta_colab` |
| app/chamados/services/validacao.py:913 | recusar_pergunta | respondida_em | PerguntaDisputa | iv | `'respondida_em': perg.respondida_em.isoformat()` |
| app/chamados/services/validacao.py:1064 | validar_lote_classe_a | validada_em | PerguntaDisputa | i | `if perg.validada_em` |
| app/chamados/services/validacao.py:1085 | validar_lote_classe_a | validada_em | PerguntaDisputa | v | `validadas += 1` |
| app/chamados/services/validacao.py:1183 | classe_do_lote | resposta_colab | PerguntaDisputa | ii | `resp = _sem_acento(...)` |
| app/chamados/services/validacao.py:1185 | classe_do_lote | resposta_colab | PerguntaDisputa | v | `'resp': perg.resposta_colab` |
| app/chamados/services/validacao.py:1187 | classe_do_lote | resposta_colab | PerguntaDisputa | v | `'resp': perg.resposta_colab` |
| app/chamados/services/validacao.py:1192 | classe_do_lote | resposta_colab | PerguntaDisputa | v | `'resp': perg.resposta_colab` |
| app/chamados/services/validacao.py:1193 | classe_do_lote | resposta_colab | PerguntaDisputa | v | `'resp': perg.resposta_colab` |
| app/chamados/services/validacao.py:1194 | classe_do_lote | resposta_colab | PerguntaDisputa | ii | `not _hora_legivel(perg.resposta_colab)` |
| app/chamados/services/validacao.py:1195 | classe_do_lote | resposta_colab | PerguntaDisputa | v | `'resp': perg.resposta_colab` |
| app/chamados/services/validacao.py:1198 | classe_do_lote | resposta_colab | PerguntaDisputa | v | `'resp': perg.resposta_colab` |
| app/chamados/services/validacao.py:1256 | fila_validar_classificada | respondida_em | PerguntaDisputa | v | `'idade_dias': (agora - p.respondida_em).days` |
| app/chamados/services/verbos.py:264 | placar_equipe | validada_em | PerguntaDisputa | vi | `validada_em__gte=d7` |
| app/chamados/services/verbos.py:270 | placar_equipe | validada_em | PerguntaDisputa | v | `if pq.validada_em >= hoje_ini` |
| app/chamados/services/verbos.py:337 | placar_aceite | respondida_em | PerguntaDisputa | vi | `respondida_em__gte=d7` |
| app/chamados/services/verbos.py:338 | placar_aceite | respondida_em | PerguntaDisputa | vi | `.values('disputa__colaborador_id', 'respondida_em')` |
| app/chamados/services/vigia_carimbo.py:98 | classificar | validada_em | PerguntaDisputa | vi | `validada_em__isnull=False` |
| app/chamados/services/vigia_fio.py:747 | respondida_sem_resposta | respondida_em | PerguntaDisputa | vi | `respondida_em__gte=desde` |
| app/chamados/services/vigia_fio.py:750 | respondida_sem_resposta | resposta_colab | PerguntaDisputa | vi | `values_list('resposta_colab', flat=True)` |
| app/chamados/templatetags/chamados_tags.py:139 | aplicacao_da_resposta | resposta_colab | PerguntaDisputa | v | `(perg.resposta_colab or '').strip()` |
| app/chamados/templatetags/chamados_tags.py:166 | leitura_da_resposta | resposta_colab | PerguntaDisputa | i | `if not (getattr(perg, 'resposta_colab', '') or '').strip()` |
| app/chamados/veredito_lavrado.py:203 | marcos_do_lote | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=False` |
| app/chamados/veredito_lavrado.py:207 | marcos_do_lote | respondida_em | PerguntaDisputa | vi | `respondida_em__isnull=False` |
| app/chamados/views_cobrar.py:293 | proposta_lote | resposta_colab | PerguntaDisputa | v | `'resposta': x['pergunta'].resposta_colab` |
| app/ponto/management/commands/reconciliar_fantasmas.py:213 | handle | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=True` |
| app/ponto/services/cartorio.py:1651 | _julgar_colab_corpo | via_resolucao | PerguntaDisputa | ii | `all(p.via_resolucao == _PD.VIA_DUPLICATA ...` |
| app/ponto/services/dia_do_colab.py:70 | dia_do_colab | resposta_colab | PerguntaDisputa | v | `'resposta_do_colab': p.resposta_colab or None` |
| app/ponto/services/flip_auto.py:129 | fila_flip_recusas | via_resolucao | PerguntaDisputa | vi | `via_resolucao=PerguntaDisputa.VIA_PARIDADE_RECUSADA` |
| app/ponto/services/flip_auto.py:130 | fila_flip_recusas | validada_em | PerguntaDisputa | vi | `validada_em__gte=corte` |
| app/ponto/services/procedencia_batida.py:74 | procedencia | materializada_em | PerguntaDisputa | v | `p.validada_em or p.materializada_em` |
| app/ponto/services/procedencia_batida.py:74 | procedencia | validada_em | PerguntaDisputa | v | `p.validada_em or p.materializada_em` |
| app/ponto/services/veredito_celula.py:196 | fechar_o_fio_do_dia | materializada_em | PerguntaDisputa | iv | `ato_em=p.materializada_em` |
| app/ponto/services/veredito_celula.py:197 | fechar_o_fio_do_dia | resposta_colab | PerguntaDisputa | iv | `ato_texto=p.resposta_colab or ''` |

linhas: 40 de 40  
totais por classe: i 5; ii 4; iii 0; iv 4; v 15; vi 12.

- **validacao.py:640** — chamada por `validar_pergunta` em app/chamados/services/validacao.py:771; **sim**, condiciona `_carimbar_validacao` em app/chamados/services/validacao.py:640.
- **validacao.py:685** — chamada, por exemplo, em app/chamados/services/acoes_disputa.py:19; **sim**, a saída impede o carimbo posterior em app/chamados/services/validacao.py:743.
- **validacao.py:898** — chamada em app/chamados/services/acoes_disputa.py:82; **sim**, a saída impede a reabertura em app/chamados/services/validacao.py:925.
- **validacao.py:1064** — chamada em app/chamados/views_cobrar.py:263 e :383; **sim**, impede a chamada da ação de validação em app/chamados/services/validacao.py:1078.
- **chamados_tags.py:166** — chamada em app/templates/chamados/partials/_pergunta_card.html:43 e app/templates/chamados/partials/modal_fio.html:274; decide quando uma porta escreve: **não verificado**.

perguntas de lei: 0

## 19/11 01:52 · sessao 59e12cf1219f · E4C-F5 · censo · origem=Code

| arquivo:linha | função | campo | receptor | classe | trecho literal curto |
|---|---|---|---|---|---|
| chamados/services/materializacao.py:199 | par_do_intervalo | validada_em | PerguntaDisputa | i | `irma_validada and q.validada_em is None` |
| chamados/services/materializacao.py:201 | par_do_intervalo | validada_em | PerguntaDisputa | i | `q.validada_em is None` |
| chamados/services/materializacao.py:261 | paridade_absolvida_bug_par_103 | materializada_em | PerguntaDisputa | ii | `p.materializada_em is not None` |
| chamados/services/materializacao.py:261 | paridade_absolvida_bug_par_103 | via_resolucao | PerguntaDisputa | ii | `p.via_resolucao == PerguntaDisputa.VIA_PARIDADE_RECUSADA` |
| chamados/services/materializacao.py:262 | paridade_absolvida_bug_par_103 | materializada_em | PerguntaDisputa | ii | `p.materializada_em < _corte` |
| chamados/services/materializacao.py:289 | _situacao_da_irma | materializada_em | PerguntaDisputa | v | `q.materializada_em is None` |
| chamados/services/materializacao.py:292 | _situacao_da_irma | via_resolucao | PerguntaDisputa | v | `'via:%s' % q.via_resolucao` |
| chamados/services/materializacao.py:329 | respostas_intervalo_recusadas_paridade | materializada_em | PerguntaDisputa | iii | `p.materializada_em is not None` |
| chamados/services/materializacao.py:558 | _tipo_pelo_marco | resposta_colab | PerguntaDisputa | ii | `_min(p.resposta_colab)` |
| chamados/services/materializacao.py:586 | _tipo_pelo_marco | resposta_colab | PerguntaDisputa | v | `resposta=p.resposta_colab` |
| chamados/services/materializacao.py:637 | _ts_tipo_do_plantio | resposta_colab | PerguntaDisputa | ii | `(p.resposta_colab or '').strip()` |
| chamados/services/materializacao.py:683 | _ts_tipo_do_plantio | resposta_colab | PerguntaDisputa | v | `(p.resposta_colab or '').strip()` |
| chamados/services/materializacao.py:690 | _ts_tipo_do_plantio | resposta_colab | PerguntaDisputa | ii | `_hora_fonte = p.resposta_colab` |
| chamados/services/materializacao.py:730 | _declarado_da_metade | resposta_colab | PerguntaDisputa | iii | `(p.resposta_colab or '').strip().split(':')` |
| chamados/services/materializacao.py:745 | divergencia_declarado_x_gravado | resposta_colab | PerguntaDisputa | v | `repr((p.resposta_colab or '').strip())` |
| chamados/services/materializacao.py:782 | espera_na_escrita | resposta_colab | PerguntaDisputa | iii | `not (p.resposta_colab or '').strip()` |
| chamados/services/materializacao.py:875 | estado_aplicacao_pergunta | materializada_em | PerguntaDisputa | v | `if p.materializada_em and` |
| chamados/services/materializacao.py:875 | estado_aplicacao_pergunta | via_resolucao | PerguntaDisputa | v | `getattr(p, 'via_resolucao', '')` |
| chamados/services/materializacao.py:881 | estado_aplicacao_pergunta | via_resolucao | PerguntaDisputa | v | `_VIAS_DE_RECUSA[p.via_resolucao]` |
| chamados/services/materializacao.py:887 | estado_aplicacao_pergunta | materializada_em | PerguntaDisputa | v | `if p.materializada_em:` |
| chamados/services/materializacao.py:966 | _declarar_ausencia_de_pergunta | resposta_colab | PerguntaDisputa | ii | `resp = (p.resposta_colab or '').strip()` |
| chamados/services/materializacao.py:1031 | _materializar_ausencia_de_resposta | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=True` |
| chamados/services/materializacao.py:1063 | _materializar_ausencia_de_resposta | resposta_colab | PerguntaDisputa | iv | `ato_texto=q.resposta_colab or ''` |
| chamados/services/materializacao.py:1123 | perguntas_absorvidas_por | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=False` |
| chamados/services/materializacao.py:1124 | perguntas_absorvidas_por | via_resolucao | PerguntaDisputa | vi | `via_resolucao__in=` |
| chamados/services/materializacao.py:1125 | perguntas_absorvidas_por | materializada_em | PerguntaDisputa | vi | `values_list('disputa_id', 'materializada_em')` |
| chamados/services/materializacao.py:1125 | perguntas_absorvidas_por | resposta_colab | PerguntaDisputa | vi | `filter(resposta_colab__in=escapes)` |
| chamados/services/materializacao.py:1130 | perguntas_absorvidas_por | materializada_em | PerguntaDisputa | vi | `Q(disputa_id=_disp, materializada_em=_inst)` |
| chamados/services/materializacao.py:1166 | devolver_absorvidas_da_ausencia_rejeitada | resposta_colab | PerguntaDisputa | iv | `'resposta_colab': p.resposta_colab` |
| chamados/services/materializacao.py:1166 | devolver_absorvidas_da_ausencia_rejeitada | via_resolucao | PerguntaDisputa | iv | `'via_resolucao': p.via_resolucao` |
| chamados/services/materializacao.py:1167 | devolver_absorvidas_da_ausencia_rejeitada | respondida_em | PerguntaDisputa | iv | `p.respondida_em and p.respondida_em.isoformat()` |
| chamados/services/materializacao.py:1168 | devolver_absorvidas_da_ausencia_rejeitada | validada_em | PerguntaDisputa | iv | `p.validada_em and p.validada_em.isoformat()` |
| chamados/services/materializacao.py:1169 | devolver_absorvidas_da_ausencia_rejeitada | materializada_em | PerguntaDisputa | iv | `p.materializada_em and p.materializada_em.isoformat()` |
| chamados/services/materializacao.py:1385 | _materializar_resposta_hora | resposta_colab | PerguntaDisputa | v | `p.resposta_colab` |
| chamados/services/materializacao.py:1567 | _materializar_resposta_hora | resposta_colab | PerguntaDisputa | iv | `'resposta_colab': p.resposta_colab` |
| chamados/services/materializacao.py:1593 | _materializar_resposta_data_hora | resposta_colab | PerguntaDisputa | ii | `raw = (p.resposta_colab or '').strip()` |
| chamados/services/materializacao.py:1602 | _materializar_resposta_data_hora | resposta_colab | PerguntaDisputa | v | `data_hora invalida ({p.resposta_colab})` |
| chamados/services/materializacao.py:1654 | _materializar_resposta_data_hora | resposta_colab | PerguntaDisputa | iv | `'resposta_colab': p.resposta_colab` |
| chamados/services/materializacao.py:1693 | _carimbar_recusa | materializada_em | PerguntaDisputa | ii | `p.materializada_em is None and codigo == 'COLISAO'` |
| chamados/services/materializacao.py:2106 | materializar_perguntas_validadas_da_disputa | validada_em | PerguntaDisputa | vi | `validada_em__isnull=False` |
| chamados/services/materializacao.py:2107 | materializar_perguntas_validadas_da_disputa | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=True` |
| chamados/services/materializacao.py:2120 | materializar_perguntas_validadas_da_disputa | materializada_em | PerguntaDisputa | ii | `p.materializada_em is not None` |
| chamados/services/materializacao.py:2123 | materializar_perguntas_validadas_da_disputa | validada_em | PerguntaDisputa | i | `p.validada_em is None` |
| chamados/services/materializacao.py:2168 | materializar_pendentes_aprovadas | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=True` |

linhas: 44 de 44

Totais por classe: i 3; ii 10; iii 3; iv 8; v 11; vi 9.

- :199 — chamadores em `chamados/services/validacao.py:632` e `chamados/services/materializacao.py:923,1732`; **sim**, condiciona a gravação do par quando `irma_validada=True` (`chamados/services/materializacao.py:178,182-183,199`).
- :201 — mesmos chamadores (`chamados/services/validacao.py:632`; `chamados/services/materializacao.py:923,1732`); **sim**, aciona o exame de plausibilidade que pode impedir o par (`chamados/services/materializacao.py:201-206`).
- :2123 — chamadores em `chamados/services/validacao.py:786`, `chamados/services/fio.py:718` e `chamados/services/acoes_disputa.py:143`; **sim**, impede chamar a materialização quando a pergunta relida está sem validação (`chamados/services/materializacao.py:2115-2125`).

perguntas de lei: 0

## 19/11 01:52 · sessao 9fb8bed83aa9 · E4C-F5 · censo · origem=Code

| arquivo:linha | função | campo | receptor | classe | trecho literal curto |
|---|---|---|---|---|---|
| chamados/services/disputa_emissao.py:1617 | reabrir_para_correcao | respondida_em | indeterminado: chave de `dict`, sem receptor | — | `respondida_em=None` |
| chamados/services/disputa_emissao.py:1617 | reabrir_para_correcao | resposta_colab | indeterminado: chave de `dict`, sem receptor | — | `resposta_colab=''` |
| chamados/services/disputa_emissao.py:1617 | reabrir_para_correcao | validada_em | indeterminado: chave de `dict`, sem receptor | — | `validada_em=None` |
| chamados/services/disputa_emissao.py:1643 | reabrir_para_correcao | respondida_em | PerguntaDisputa | vi | `qs.values_list(... 'respondida_em'` |
| chamados/services/disputa_emissao.py:1643 | reabrir_para_correcao | resposta_colab | PerguntaDisputa | vi | `qs.values_list('pk', 'resposta_colab'` |
| chamados/services/disputa_emissao.py:1644 | reabrir_para_correcao | validada_em | PerguntaDisputa | vi | `'validada_em', 'validada_por_id'` |
| chamados/services/disputa_emissao.py:1645 | reabrir_para_correcao | materializada_em | PerguntaDisputa | vi | `'materializada_em', 'via_resolucao'` |
| chamados/services/disputa_emissao.py:1645 | reabrir_para_correcao | via_resolucao | PerguntaDisputa | vi | `'materializada_em', 'via_resolucao'` |
| chamados/services/disputa_emissao.py:1671 | responder_automaticamente | resposta_colab | PerguntaDisputa | ii | `if (p.resposta_colab or '').strip()` |
| chamados/services/disputa_emissao.py:1701 | carimbar_resposta_pela_trilha | respondida_em | PerguntaDisputa | ii | `if p.respondida_em is not None` |
| chamados/services/disputa_emissao.py:1701 | carimbar_resposta_pela_trilha | resposta_colab | PerguntaDisputa | ii | `not (p.resposta_colab or '').strip()` |
| chamados/services/disputa_emissao.py:1709 | carimbar_resposta_pela_trilha | resposta_colab | PerguntaDisputa | iv | `ato_texto=p.resposta_colab` |
| chamados/services/disputa_emissao.py:1871 | reabrir_credor_de_pergunta_viva | respondida_em | PerguntaDisputa | ii | `q.respondida_em is None` |
| chamados/services/disputa_emissao.py:1871 | reabrir_credor_de_pergunta_viva | resposta_colab | PerguntaDisputa | ii | `not texto_vazio(q.resposta_colab)` |
| chamados/services/disputa_emissao.py:1951 | _resolver_perguntas_por_chamado_resolvido | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=True` |
| chamados/services/disputa_emissao.py:2083 | _resolver_perguntas_por_chamado_resolvido | materializada_em | PerguntaDisputa | i | `q.materializada_em is None` |
| chamados/services/disputa_emissao.py:2083 | _resolver_perguntas_por_chamado_resolvido | validada_em | PerguntaDisputa | i | `q.validada_em is None` |
| chamados/services/disputa_emissao.py:2093 | _resolver_perguntas_por_chamado_resolvido | via_resolucao | PerguntaDisputa | iv | `_via_antes = q.via_resolucao` |
| chamados/services/disputa_emissao.py:2095 | _resolver_perguntas_por_chamado_resolvido | respondida_em | PerguntaDisputa | iv | `q.respondida_em.isoformat()` |
| chamados/services/disputa_emissao.py:2100 | _resolver_perguntas_por_chamado_resolvido | resposta_colab | PerguntaDisputa | v | `_tem_resp = bool((q.resposta_colab or '').strip())` |
| chamados/services/disputa_emissao.py:2108 | _resolver_perguntas_por_chamado_resolvido | via_resolucao | PerguntaDisputa | iv | `q.via_resolucao, 'com resposta` |
| chamados/services/disputa_emissao.py:2111 | _resolver_perguntas_por_chamado_resolvido | via_resolucao | PerguntaDisputa | iv | `'via_resolucao': q.via_resolucao` |
| chamados/services/disputa_emissao.py:2112 | _resolver_perguntas_por_chamado_resolvido | respondida_em | PerguntaDisputa | iv | `'respondida_em': q.respondida_em.isoformat()` |
| chamados/services/explicador.py:55 | _explicar_validada | resposta_colab | PerguntaDisputa | v | `% (p.resposta_colab,)` |
| chamados/services/explicador.py:102 | explicar_pergunta | resposta_colab | PerguntaDisputa | v | `acao='Validar ou recusar a resposta` |
| chamados/services/explicador.py:103 | explicar_pergunta | resposta_colab | PerguntaDisputa | v | `motivo='colaborador respondeu %s'` |
| chamados/services/explicador.py:118 | explicar_pergunta | via_resolucao | PerguntaDisputa | v | `p.get_via_resolucao_display() if p.via_resolucao` |
| chamados/services/fabrica_da_celula.py:149 | motivos_a_perguntar | via_resolucao | PerguntaDisputa | vi | `.exclude(via_resolucao=PerguntaDisputa.VIA_DUPLICATA)` |
| chamados/services/fabrica_da_celula.py:171 | perguntar | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=False` |
| chamados/services/fio.py:188 | espera_o_texto_do_colaborador | respondida_em | PerguntaDisputa | i | `p.respondida_em is None or respondida_sem_texto(p)` |
| chamados/services/fio.py:378 | montar_chamados_fio | resposta_colab | PerguntaDisputa | v | `(_p.resposta_colab or '').strip()` |
| chamados/services/fio.py:1149 | reuso_sem_rechave | via_resolucao | PerguntaDisputa | vi | `.exclude(via_resolucao__in=PerguntaDisputa.VIAS_FORA_DO_FATO)` |
| chamados/services/fio.py:1216 | sem_dia_na_saida | via_resolucao | PerguntaDisputa | vi | `.exclude(via_resolucao__in=PerguntaDisputa.VIAS_FORA_DO_FATO)` |
| chamados/services/fio.py:1295 | merito_perdedor | via_resolucao | PerguntaDisputa | ii | `p.via_resolucao == PerguntaDisputa.VIA_ADMIN_DECIDIU` |
| chamados/services/fio.py:1493 | retratar_duplicata | via_resolucao | PerguntaDisputa | ii | `if p.via_resolucao == PerguntaDisputa.VIA_DUPLICATA` |
| chamados/services/fio.py:1576 | _no_fato | via_resolucao | PerguntaDisputa | vi | `.exclude(via_resolucao__in=PerguntaDisputa.VIAS_FORA_DO_FATO)` |
| chamados/services/fio.py:1709 | recusa_par_ilegal | respondida_em | indeterminado: atributo de `SimpleNamespace`, sem modelo | — | `SimpleNamespace(respondida_em=respondida_em` |
| chamados/services/fio.py:1709 | recusa_par_ilegal | resposta_colab | indeterminado: atributo de `SimpleNamespace`, sem modelo | — | `resposta_colab=resposta_colab)` |
| chamados/services/fio.py:2017 | fecho_cruza_dias | via_resolucao | PerguntaDisputa | vi | `.exclude(via_resolucao__in=PerguntaDisputa.VIAS_FORA_DO_FATO)` |
| chamados/services/fio.py:2053 | perguntas_sobre_batida_retratada | via_resolucao | PerguntaDisputa | vi | `.exclude(via_resolucao__in=PerguntaDisputa.VIAS_FORA_DO_FATO)` |
| chamados/services/fio.py:2075 | universo_do_grupo | via_resolucao | PerguntaDisputa | vi | `.exclude(via_resolucao__in=PerguntaDisputa.VIAS_FORA_DO_FATO)` |
| chamados/services/fio.py:2120 | grupos_de_colisao | via_resolucao | PerguntaDisputa | vi | `.exclude(via_resolucao__in=PerguntaDisputa.VIAS_FORA_DO_FATO)` |
| chamados/services/historico.py:109 | _eventos_dos_chamados | resposta_colab | PerguntaDisputa | v | `resp = (p.resposta_colab or '')[:14]` |
| chamados/services/materializacao.py:148 | metade_irma_do_intervalo | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=True` |

linhas: 44 de 44

Totais por classe: i: 3; ii: 7; iii: 0; iv: 6; v: 7; vi: 16. Sem classe: 5.

- **disputa_emissao.py:2083 — materializada_em:** chamada pelo wrapper em chamados/services/disputa_emissao.py:1919; **sim**, seleciona candidatas antes de `q.carimbar` (chamados/services/disputa_emissao.py:2083, chamados/services/disputa_emissao.py:2103).
- **disputa_emissao.py:2083 — validada_em:** chamada pelo mesmo wrapper em chamados/services/disputa_emissao.py:1919; **sim**, seleciona candidatas antes de `q.carimbar` (chamados/services/disputa_emissao.py:2083, chamados/services/disputa_emissao.py:2103).
- **fio.py:188 — respondida_em:** chamada em chamados/services/fio.py:175, chamados/services/fio.py:360 e chamados/services/cobranca.py:241; **não foi verificada** decisão de quando uma porta existente escreve nesses usos.

perguntas de lei: 0

## 19/11 01:51 · sessao 9e6c148debe0 · E4C-F5 · censo · origem=Code

| arquivo:linha | função | campo | receptor | classe | trecho literal curto |
|---|---|---|---|---|---|
| chamados/management/commands/curar_perguntas_em_limbo.py:32 | nota_da_resposta | resposta_colab | PerguntaDisputa | v | `p.resposta_colab or ''` |
| chamados/management/commands/curar_perguntas_em_limbo.py:47 | handle | respondida_em | PerguntaDisputa | vi | `respondida_em__isnull=True` |
| chamados/management/commands/curar_perguntas_em_limbo.py:47 | handle | resposta_colab | PerguntaDisputa | vi | `exclude(resposta_colab='')` |
| chamados/management/commands/curar_perguntas_em_limbo.py:70 | handle | respondida_em | PerguntaDisputa | vi | `respondida_em__isnull=True` |
| chamados/management/commands/encerrar_perguntas_com_fato.py:33 | handle | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=False` |
| chamados/management/commands/encerrar_perguntas_com_fato.py:34 | handle | via_resolucao | PerguntaDisputa | vi | `exclude(via_resolucao=...` |
| chamados/management/commands/encerrar_perguntas_com_fato.py:43 | handle | via_resolucao | PerguntaDisputa | ii | `p.via_resolucao in SEM_HISTORIA` |
| chamados/management/commands/encerrar_perguntas_com_fato.py:50 | handle | via_resolucao | PerguntaDisputa | v | `Counter(p.via_resolucao` |
| chamados/management/commands/encerrar_perguntas_com_fato.py:67 | handle | via_resolucao | PerguntaDisputa | iv | `antes = p.via_resolucao` |
| chamados/management/commands/encerrar_perguntas_com_fato.py:77 | handle | via_resolucao | PerguntaDisputa | iv | `depois={'via_resolucao': p.via_resolucao}` |
| chamados/management/commands/estoque_l130_instante_sem_texto.py:58 | handle | respondida_em | PerguntaDisputa | iv | `str(p.respondida_em)` |
| chamados/management/commands/estoque_l130_instante_sem_texto.py:58 | handle | resposta_colab | PerguntaDisputa | iv | `'resposta_colab': p.resposta_colab` |
| chamados/management/commands/estoque_l130_instante_sem_texto.py:59 | handle | via_resolucao | PerguntaDisputa | iv | `'via_resolucao': p.via_resolucao` |
| chamados/management/commands/migrar_data_evento_dia_do_turno.py:70 | _fonte_materializada | via_resolucao | PerguntaDisputa | ii | `p.via_resolucao in (...)` |
| chamados/management/commands/migrar_data_evento_dia_do_turno.py:71 | _fonte_materializada | via_resolucao | PerguntaDisputa | v | `'absorcao (via=%s` |
| chamados/management/commands/migrar_data_evento_dia_do_turno.py:72 | _fonte_materializada | via_resolucao | PerguntaDisputa | v | `'sem_fonte (materializada` |
| chamados/management/commands/migrar_data_evento_dia_do_turno.py:147 | classificar | materializada_em | PerguntaDisputa | iv | `'materializada' if p.materializada_em else 'viva'` |
| chamados/management/commands/relavrar_via_i6.py:58 | planejar | via_resolucao | PerguntaDisputa | ii | `p.via_resolucao not in VIAS_DERIVADAS` |
| chamados/management/commands/relavrar_via_i6.py:59 | planejar | via_resolucao | PerguntaDisputa | v | `_fora(p.via_resolucao, 'via de DECISAO` |
| chamados/management/commands/relavrar_via_i6.py:62 | planejar | via_resolucao | PerguntaDisputa | v | `_fora(p.via_resolucao, 'excluido` |
| chamados/management/commands/relavrar_via_i6.py:66 | planejar | via_resolucao | PerguntaDisputa | v | `_fora(p.via_resolucao, 'sem destino` |
| chamados/management/commands/relavrar_via_i6.py:68 | planejar | via_resolucao | PerguntaDisputa | ii | `nova != p.via_resolucao` |
| chamados/management/commands/relavrar_via_i6.py:88 | handle | via_resolucao | PerguntaDisputa | v | `pares[(p.via_resolucao, nova)]` |
| chamados/management/commands/validar_classe_a.py:63 | handle | resposta_colab | PerguntaDisputa **e** DecisaoPergunta/ato | iii | `DecisaoPergunta.leitura(q).resposta_colab != q.resposta_colab` |
| chamados/management/commands/validar_classe_a.py:83 | handle | resposta_colab | DecisaoPergunta/ato | — | `DecisaoPergunta.leitura(q).resposta_colab` |
| chamados/migrations/0053_pergunta_unica_por_familia.py:32 | _no_fato | via_resolucao | PerguntaDisputa | vi | `~Q(via_resolucao__in=VIAS_FORA_DO_FATO)` |
| chamados/models.py:1543 | _universo_da_ressuscitacao | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=False` |
| chamados/models.py:1547 | _universo_da_ressuscitacao | via_resolucao | PerguntaDisputa | vi | `via_resolucao__in=PerguntaDisputa.VIAS_FORA_DO_FATO` |
| chamados/models.py:1558 | _universo_da_ressuscitacao | respondida_em | PerguntaDisputa | ii | `p.respondida_em is None` |
| chamados/models.py:1558 | _universo_da_ressuscitacao | resposta_colab | PerguntaDisputa | ii | `not texto_vazio(p.resposta_colab)` |
| chamados/models.py:1558 | _universo_da_ressuscitacao | validada_em | PerguntaDisputa | ii | `p.validada_em is None` |
| chamados/models.py:1650 | ressuscitar_perguntas | materializada_em | PerguntaDisputa | vi | `filter(materializada_em__isnull=False)` |
| chamados/models.py:1651 | ressuscitar_perguntas | via_resolucao | PerguntaDisputa | vi | `exclude(via_resolucao__in=...` |
| chamados/models.py:1655 | ressuscitar_perguntas | respondida_em | PerguntaDisputa | vi | `only('pk', 'respondida_em'` |
| chamados/models.py:1655 | ressuscitar_perguntas | resposta_colab | PerguntaDisputa | vi | `'validada_por_id', 'resposta_colab'` |
| chamados/models.py:1655 | ressuscitar_perguntas | validada_em | PerguntaDisputa | vi | `'respondida_em', 'validada_em'` |
| chamados/models.py:1686 | ressuscitar_perguntas | via_resolucao | PerguntaDisputa | vi | `alvo.exclude(via_resolucao__in=...` |
| chamados/models.py:1692 | ressuscitar_perguntas | validada_em | DecisaoPergunta/ato | — | `d.ato_em == _lido.validada_em` |
| chamados/models.py:1696 | ressuscitar_perguntas | respondida_em | DecisaoPergunta/ato | — | `respondida_original_em=_lido.respondida_em` |
| chamados/models.py:1696 | ressuscitar_perguntas | resposta_colab | DecisaoPergunta/ato | — | `ato_texto=_lido.resposta_colab or ''` |
| chamados/models.py:1697 | ressuscitar_perguntas | validada_em | DecisaoPergunta/ato | — | `validada_original_em=_lido.validada_em` |
| chamados/models.py:1698 | ressuscitar_perguntas | validada_em | DecisaoPergunta/ato | — | `if _lido.validada_em is not None` |
| chamados/models.py:1702 | ressuscitar_perguntas | validada_em | PerguntaDisputa | ii | `descarimbar_em_lote(... validada_em=None` |
| chamados/models.py:1703 | ressuscitar_perguntas | respondida_em | PerguntaDisputa | ii | `respondida_em=None` |
| chamados/models.py:1703 | ressuscitar_perguntas | resposta_colab | PerguntaDisputa | ii | `resposta_colab=''` |

linhas: 45 de 45

Totais por classe: i = 0; ii = 10; iii = 1; iv = 6; v = 8; vi = 14. Seis linhas têm receptor de ato e não recebem classe; a linha de `validar_classe_a.py:63` compara ato e projeção.

perguntas de lei: 0. [app/docs/CORTES.md:545](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/CORTES.md:545); [app/docs/CORTES.md:560](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/CORTES.md:560); [app/docs/LEIS.md:151](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/LEIS.md:151).

## 19/11 01:51 · sessao 70b9c08a266b · E4C-F5 · censo · origem=Code

| arquivo:linha | função | campo | receptor | classe | trecho literal curto |
|---|---|---|---|---|---|
| api/views.py:1163 | api_chamados_v2 | resposta_colab | PerguntaDisputa | v | `p.resposta_colab` |
| api/views.py:1165 | api_chamados_v2 | respondida_em | PerguntaDisputa | v | `p.respondida_em.isoformat()` |
| api/views.py:2567 | api_disputa_perguntas._item | resposta_colab | PerguntaDisputa | v | `p.resposta_colab` |
| api/views.py:2569 | api_disputa_perguntas._item | respondida_em | PerguntaDisputa | v | `p.respondida_em.isoformat()` |
| api/views.py:2645 | api_disputa_responder | respondida_em | PerguntaDisputa | v | `p.respondida_em.isoformat()` |
| api/views_mensageria.py:569 | _item_validar | resposta_colab | PerguntaDisputa | v | `p.resposta_colab` |
| api/views_mensageria.py:571 | _item_validar | respondida_em | PerguntaDisputa | v | `p.respondida_em.isoformat()` |
| api/views_mensageria.py:572 | _item_validar | respondida_em | PerguntaDisputa | v | `(agora - p.respondida_em).days` |
| chamados/juizes.py:292 | _retratada_com_trilha | materializada_em | PerguntaDisputa | iii | `p.materializada_em is None` |
| chamados/juizes.py:352 | _slot_esperado | respondida_em | PerguntaDisputa | iv | `respondida is None and p.respondida_em is not None` |
| chamados/juizes.py:364 | _slot_esperado | resposta_colab | PerguntaDisputa | iv | `(p.resposta_colab or '').strip()` |
| chamados/juizes.py:683 | _pergunta_materializada_de_fato | materializada_em | PerguntaDisputa | iii | `p.materializada_em is not None` |
| chamados/juizes.py:683 | _pergunta_materializada_de_fato | via_resolucao | PerguntaDisputa | iii | `p.via_resolucao in VIAS_SEM_TESTEMUNHA` |
| chamados/juizes.py:695 | _pergunta_materializada_de_fato | via_resolucao | PerguntaDisputa | iii | `p.via_resolucao in _ABSOLVEM_SEMPRE` |
| chamados/juizes.py:711 | _pergunta_materializada_de_fato | materializada_em | PerguntaDisputa | iii | `return p.materializada_em is not None` |
| chamados/juizes.py:856 | _chave_memo_pv | respondida_em | PerguntaDisputa | iii | `getattr(p, 'respondida_em', None)` |
| chamados/juizes.py:856 | _chave_memo_pv | validada_em | PerguntaDisputa | iii | `getattr(p, 'validada_em', None)` |
| chamados/juizes.py:857 | _chave_memo_pv | resposta_colab | PerguntaDisputa | iii | `getattr(p, 'resposta_colab', None)` |
| chamados/juizes.py:857 | _chave_memo_pv | via_resolucao | PerguntaDisputa | iii | `getattr(p, 'via_resolucao', None)` |
| chamados/juizes.py:858 | _chave_memo_pv | materializada_em | PerguntaDisputa | iii | `getattr(p, 'materializada_em', None)` |
| chamados/juizes.py:1360 | _resposta_concorda_com_a_luz | resposta_colab | PerguntaDisputa | iv | `(p.resposta_colab or '').strip()` |
| chamados/juizes.py:1389 | projecao_temporal_da_pergunta | respondida_em | PerguntaDisputa | i | `respondida = ... p.respondida_em` |
| chamados/juizes.py:1389 | projecao_temporal_da_pergunta | resposta_colab | PerguntaDisputa | i | `resposta = ... p.resposta_colab` |
| chamados/juizes.py:1389 | projecao_temporal_da_pergunta | validada_em | PerguntaDisputa | i | `validada = ... p.validada_em` |
| chamados/juizes.py:1390 | projecao_temporal_da_pergunta | materializada_em | PerguntaDisputa | iv | `materializada, via = p.materializada_em, p.via_resolucao` |
| chamados/juizes.py:1390 | projecao_temporal_da_pergunta | via_resolucao | PerguntaDisputa | iv | `materializada, via = p.materializada_em, p.via_resolucao` |
| chamados/juizes.py:1423 | _pergunta_viva_calc | respondida_em | indeterminado: `SimpleNamespace` projetado | — | `SimpleNamespace(respondida_em=respondida` |
| chamados/juizes.py:1423 | _pergunta_viva_calc | resposta_colab | indeterminado: `SimpleNamespace` projetado | — | `resposta_colab=resposta)` |
| chamados/juizes.py:1502 | pergunta_sem_lastro_ou_viva | materializada_em | PerguntaDisputa | ii | `p.materializada_em is None or pergunta_viva(p)` |
| chamados/juizes.py:1534 | respondida_sem_texto | respondida_em | PerguntaDisputa; também sondador | iii | `p.respondida_em is not None` |
| chamados/juizes.py:1534 | respondida_sem_texto | resposta_colab | PerguntaDisputa; também sondador | iii | `texto_vazio(p.resposta_colab)` |
| chamados/juizes.py:1541 | texto_sem_carimbo | respondida_em | PerguntaDisputa; também sondador | iii | `p.respondida_em is None` |
| chamados/juizes.py:1541 | texto_sem_carimbo | resposta_colab | PerguntaDisputa; também sondador | iii | `not texto_vazio(p.resposta_colab)` |
| chamados/juizes.py:1552 | pergunta_respondida | respondida_em | PerguntaDisputa; também LeituraDoAto | i | `p.respondida_em is not None` |
| chamados/juizes.py:1552 | pergunta_respondida | resposta_colab | PerguntaDisputa; também LeituraDoAto | i | `not texto_vazio(p.resposta_colab)` |
| chamados/juizes.py:1572 | q_respondida_sem_texto | respondida_em | PerguntaDisputa | vi | `Q(respondida_em__isnull=False)` |
| chamados/juizes.py:1572 | q_respondida_sem_texto | resposta_colab | PerguntaDisputa | vi | `Q(resposta_colab__regex=_RE_SO_BRANCOS)` |
| chamados/juizes.py:1580 | q_respondida_com_texto | respondida_em | PerguntaDisputa | vi | `Q(respondida_em__isnull=False)` |
| chamados/juizes.py:1580 | q_respondida_com_texto | resposta_colab | PerguntaDisputa | vi | `Q(resposta_colab__regex=_RE_TEM_TEXTO)` |
| chamados/juizes.py:1591 | q_nao_respondida | respondida_em | PerguntaDisputa | vi | `Q(respondida_em__isnull=True)` |
| chamados/juizes.py:1765 | perguntas_respondidas_a_validar | respondida_em | PerguntaDisputa | vi | `.order_by('respondida_em')` |
| chamados/juizes.py:1791 | declarou_hora | resposta_colab | PerguntaDisputa | iv | `(pergunta.resposta_colab or '').strip()` |
| chamados/juizes.py:1938 | delta_da_resposta | resposta_colab | PerguntaDisputa | iv | `(perg.resposta_colab or '').strip()` |
| chamados/juizes.py:2042 | classificar_resposta_validacao | resposta_colab | PerguntaDisputa | iv | `(perg.resposta_colab or '').strip().lower()` |
| chamados/juizes.py:2047 | classificar_resposta_validacao | resposta_colab | PerguntaDisputa | iv | `'resp': perg.resposta_colab` |

linhas: 45 de 45.

Totais por classe: i **5**; ii **1**; iii **14**; iv **9**; v **8**; vi **6**; sem classe **2**.

- `juizes.py:1389` — respondida_em: chamada em `chamados/juizes.py:351` e `:1404`; alimenta o julgamento de vitalidade, que é usado como guarda de porta em `api/views.py:2622`.
- `juizes.py:1389` — resposta_colab: chamada em `chamados/juizes.py:351` e `:1404`; alimenta o julgamento de vitalidade usado na guarda em `api/views.py:2622`.
- `juizes.py:1389` — validada_em: chamada em `chamados/juizes.py:351` e `:1404`; alimenta o julgamento de vitalidade usado na guarda em `api/views.py:2622`.
- `juizes.py:1552` — respondida_em: entre os chamadores, `api/views.py:2628` passa o resultado à porta em `:2635`; `chamados/services/materializacao.py:1300` condiciona a guarda de aplicação.
- `juizes.py:1552` — resposta_colab: entre os chamadores, `api/views.py:2628` passa o resultado à porta em `:2635`; `chamados/services/materializacao.py:1300` condiciona a guarda de aplicação.

perguntas de lei: **0**.

## 19/11 01:51 · sessao b473b8de65f0 · E4C-F5 · censo · origem=Code

| arquivo:linha | função | campo | receptor | classe | trecho literal curto |
|---|---|---|---|---|---|
| chamados/juizes.py:2064 | motivo_implausivel_validacao | resposta_colab | PerguntaDisputa | ii | `(perg.resposta_colab or '').strip()` |
| chamados/management/commands/auditar_disputas.py:71 | handle | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=True` |
| chamados/management/commands/auditar_disputas.py:150 | handle | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=True` |
| chamados/management/commands/auditar_invariantes_chamados.py:203 | _contraprova_f1t | via_resolucao | PerguntaDisputa | iii | `not (_p.via_resolucao or '').strip()` |
| chamados/management/commands/auditar_invariantes_chamados.py:206 | _contraprova_f1t | validada_em | DecisaoPergunta/ato | — | `DecisaoPergunta.leitura(_p).validada_em` |
| chamados/management/commands/auditar_invariantes_chamados.py:207 | _contraprova_f1t | via_resolucao | PerguntaDisputa | iii | `_p.via_resolucao in _IDENT` |
| chamados/management/commands/auditar_invariantes_chamados.py:434 | contraprova_piso | via_resolucao | PerguntaDisputa | iii | `por_via[p.via_resolucao]` |
| chamados/management/commands/auditar_invariantes_chamados.py:472 | universo_i6 | materializada_em | PerguntaDisputa | vi | `.values_list('pk', 'materializada_em')` |
| chamados/management/commands/auditar_invariantes_chamados.py:541 | _i6_pares | materializada_em | PerguntaDisputa | iii | `p.materializada_em is None or not PMF(p)` |
| chamados/management/commands/auditar_invariantes_chamados.py:545 | _i6_pares | via_resolucao | PerguntaDisputa | iii | `_i6.append((p.pk, p.via_resolucao))` |
| chamados/management/commands/auditar_invariantes_chamados.py:592 | handle | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=True` |
| chamados/management/commands/auditar_invariantes_chamados.py:911 | handle | materializada_em | PerguntaDisputa | iii | `_p8b.materializada_em is not None` |
| chamados/management/commands/auditar_invariantes_chamados.py:962 | handle | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=False` |
| chamados/management/commands/auditar_invariantes_chamados.py:962 | handle | via_resolucao | PerguntaDisputa | vi | `via_resolucao=''` |
| chamados/management/commands/auditar_invariantes_chamados.py:1084 | handle | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=True` |
| chamados/management/commands/auditar_invariantes_chamados.py:1084 | handle | validada_em | PerguntaDisputa | vi | `validada_em__isnull=False` |
| chamados/management/commands/auditar_invariantes_chamados.py:1115 | handle | respondida_em | PerguntaDisputa | vi | `filter(respondida_em__isnull=True)` |
| chamados/management/commands/auditar_invariantes_chamados.py:1116 | handle | resposta_colab | PerguntaDisputa | vi | `resposta_colab=''` |
| chamados/management/commands/auditar_invariantes_chamados.py:1167 | handle | materializada_em | PerguntaDisputa | vi | `filter(materializada_em__isnull=False)` |
| chamados/management/commands/auditar_invariantes_chamados.py:1588 | handle | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=True` |
| chamados/management/commands/auditar_invariantes_chamados.py:1617 | handle | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=True` |
| chamados/management/commands/auditar_invariantes_chamados.py:1726 | handle | via_resolucao | PerguntaDisputa | iii | `(p.via_resolucao or '').strip() and not _viva(p)` |
| chamados/management/commands/auditar_invariantes_chamados.py:1783 | handle | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=True` |
| chamados/management/commands/backfill_decisoes_pergunta.py:57 | linhas_a_criar | respondida_em | PerguntaDisputa | vi | `Q(respondida_em__isnull=False)` |
| chamados/management/commands/backfill_decisoes_pergunta.py:57 | linhas_a_criar | validada_em | PerguntaDisputa | vi | `Q(validada_em__isnull=False)` |
| chamados/management/commands/backfill_decisoes_pergunta.py:61 | linhas_a_criar | respondida_em | PerguntaDisputa | vi | `'respondida_em'` em `.only()` |
| chamados/management/commands/backfill_decisoes_pergunta.py:61 | linhas_a_criar | validada_em | PerguntaDisputa | vi | `'validada_em'` em `.only()` |
| chamados/management/commands/backfill_decisoes_pergunta.py:62 | linhas_a_criar | resposta_colab | PerguntaDisputa | vi | `'resposta_colab'` em `.only()` |
| chamados/management/commands/backfill_decisoes_pergunta.py:105 | contador_validada_projetada_sem_linha | respondida_em | PerguntaDisputa | vi | `'respondida_em'` em `.only()` |
| chamados/management/commands/backfill_decisoes_pergunta.py:105 | contador_validada_projetada_sem_linha | resposta_colab | PerguntaDisputa | vi | `'resposta_colab'` em `.only()` |
| chamados/management/commands/backfill_decisoes_pergunta.py:105 | contador_validada_projetada_sem_linha | validada_em | PerguntaDisputa | vi | `'validada_em'` em `.only()` |
| chamados/management/commands/backfill_decisoes_pergunta.py:128 | admin_decidiu_sem_ato | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=False` |
| chamados/management/commands/backfill_decisoes_pergunta.py:128 | admin_decidiu_sem_ato | via_resolucao | PerguntaDisputa | vi | `via_resolucao=PerguntaDisputa.VIA_ADMIN_DECIDIU` |
| chamados/management/commands/backfill_decisoes_pergunta.py:129 | admin_decidiu_sem_ato | validada_em | PerguntaDisputa | vi | `validada_em__isnull=True` |
| chamados/management/commands/backfill_decisoes_pergunta.py:341 | aplicar | via_resolucao | PerguntaDisputa | ii | `p.via_resolucao != PerguntaDisputa.VIA_ADMIN_DECIDIU` |
| chamados/management/commands/backfill_decisoes_pergunta.py:352 | aplicar | materializada_em | PerguntaDisputa | iv | `ato_em=p.materializada_em` |
| chamados/management/commands/backfill_decisoes_pergunta.py:353 | aplicar | resposta_colab | PerguntaDisputa | iv | `ato_texto=p.resposta_colab or ''` |
| chamados/management/commands/backfill_decisoes_pergunta.py:357 | aplicar | via_resolucao | PerguntaDisputa | iv | `via_antes = {p.pk: p.via_resolucao for p in escreve}` |
| chamados/management/commands/backfill_decisoes_pergunta.py:425 | _diff_do_grupo | via_resolucao | PerguntaDisputa | vi | `.values_list('pk', 'via_resolucao')` |
| chamados/management/commands/backfill_decisoes_pergunta.py:531 | handle | respondida_em | PerguntaDisputa | vi | `'respondida_em'` em `.only()` |
| chamados/management/commands/backfill_decisoes_pergunta.py:531 | handle | resposta_colab | PerguntaDisputa | vi | `'resposta_colab'` em `.only()` |
| chamados/management/commands/backfill_decisoes_pergunta.py:531 | handle | via_resolucao | PerguntaDisputa | vi | `'via_resolucao'` em `.only()` |
| chamados/management/commands/bug_par_103.py:67 | handle | materializada_em | PerguntaDisputa | v | `'materializada_em': str(p.materializada_em)` |
| chamados/management/commands/bug_par_103.py:67 | handle | via_resolucao | PerguntaDisputa | v | `'via_resolucao': p.via_resolucao` |
| chamados/management/commands/bug_par_103.py:69 | handle | validada_em | PerguntaDisputa | v | `p.validada_em and str(p.validada_em)` |

linhas: 45 de 45

Totais por classe: i **0**; ii **2**; iii **7**; iv **3**; v **3**; vi **29**. Uma leitura é de ato, sem classe: chamados/management/commands/auditar_invariantes_chamados.py:206.

perguntas de lei: 0

## 19/11 01:51 · sessao b7be1208f6f3 · E4C-F5 · censo · origem=Code

| arquivo:linha | função | campo | receptor | classe | trecho literal curto |
|---|---|---|---|---|---|
| chamados/models.py:2005 | sem_veredito_entre | materializada_em | PerguntaDisputa | iv | `q.materializada_em is None` |
| chamados/models.py:2005 | sem_veredito_entre | validada_em | DecisaoPergunta/ato | — | `_lido.validada_em is None` |
| chamados/models.py:2082 | fechar | via_resolucao | PerguntaDisputa | v | `_via_antes = _q.via_resolucao` |
| chamados/models.py:2088 | fechar | materializada_em | PerguntaDisputa | iv | `ato_em=_q.materializada_em` |
| chamados/models.py:2089 | fechar | resposta_colab | DecisaoPergunta/ato | — | `leitura(_q).resposta_colab` |
| chamados/models.py:2218 | remover_motivo | via_resolucao | PerguntaDisputa | vi | `exclude(via_resolucao=...)` |
| chamados/models.py:2221 | remover_motivo | resposta_colab | DecisaoPergunta/ato | — | `leitura(_q).resposta_colab` |
| chamados/models.py:2223 | remover_motivo | via_resolucao | PerguntaDisputa | v | `_q.via_resolucao or '-'` |
| chamados/models.py:2911 | do_fato | via_resolucao | PerguntaDisputa | vi | `exclude(via_resolucao__in=...)` |
| chamados/models.py:2942 | q_dos_fatos | via_resolucao | PerguntaDisputa | vi | `~models.Q(via_resolucao__in=...)` |
| chamados/models.py:3236 | estado | validada_em | DecisaoPergunta/ato | — | `_lido.validada_em is not None` |
| chamados/models.py:3238 | estado | materializada_em | PerguntaDisputa | iv | `self.materializada_em is not None` |
| chamados/models.py:3242 | estado | materializada_em | PerguntaDisputa | iv | `self.materializada_em is not None` |
| chamados/models.py:3251 | fechar_por_admin | via_resolucao | PerguntaDisputa | ii | `self.via_resolucao == self.VIA_ADMIN_DECIDIU` |
| chamados/models.py:3252 | fechar_por_admin | materializada_em | PerguntaDisputa | ii | `self.materializada_em is not None` |
| chamados/models.py:3322 | carimbar | materializada_em | PerguntaDisputa | ii | `antes = (self.materializada_em, ...` |
| chamados/models.py:3322 | carimbar | via_resolucao | PerguntaDisputa | ii | `self.via_resolucao` |
| chamados/models.py:3323 | carimbar | respondida_em | PerguntaDisputa | ii | `self.respondida_em` |
| chamados/models.py:3323 | carimbar | validada_em | PerguntaDisputa | ii | `self.validada_em` |
| chamados/models.py:3326 | carimbar | resposta_colab | PerguntaDisputa | ii | `not texto_vazio(self.resposta_colab)` |
| chamados/models.py:3327 | carimbar | respondida_em | PerguntaDisputa | ii | `self.respondida_em is None` |
| chamados/models.py:3330 | carimbar | validada_em | PerguntaDisputa | ii | `self.validada_em is None` |
| chamados/models.py:3378 | carimbar_identidade | materializada_em | PerguntaDisputa | ii | `manter_carimbo and self.materializada_em is not None` |
| chamados/models.py:3379 | carimbar_identidade | materializada_em | PerguntaDisputa | ii | `_escrever_carimbo(self.materializada_em, ...` |
| chamados/models.py:3397 | fora_do_fato | via_resolucao | PerguntaDisputa | ii | `self.via_resolucao in self.VIAS_FORA_DO_FATO` |
| chamados/models.py:3468 | descarimbar_em_lote | resposta_colab | PerguntaDisputa | vi | `qs.values_list('resposta_colab', flat=True)` |
| chamados/models.py:3471 | descarimbar_em_lote | respondida_em | PerguntaDisputa | vi | `qs.values_list('respondida_em', flat=True)` |
| chamados/models.py:3480 | descarimbar_em_lote | via_resolucao | PerguntaDisputa | vi | `exclude(via_resolucao__in=...)` |
| chamados/models.py:3486 | descarimbar_em_lote | via_resolucao | PerguntaDisputa | vi | `exclude(via_resolucao__in=...)` |
| chamados/models.py:3494 | encerrar_por_fato_no_espelho | via_resolucao | PerguntaDisputa | ii | `self.via_resolucao == self.VIA_FATO_MATERIALIZADO` |
| chamados/models.py:3505 | encerrar_fantasma | materializada_em | PerguntaDisputa | ii | `self.materializada_em is not None` |
| chamados/models.py:3505 | encerrar_fantasma | via_resolucao | PerguntaDisputa | ii | `self.via_resolucao == self.VIA_FANTASMA` |
| chamados/models.py:3526 | reabrir_por_resposta | materializada_em | PerguntaDisputa | ii | `self.materializada_em is None` |
| chamados/models.py:3526 | reabrir_por_resposta | via_resolucao | PerguntaDisputa | ii | `self.via_resolucao not in VIAS_SEM_LASTRO` |
| chamados/models.py:3533 | reabrir_por_competencia | via_resolucao | PerguntaDisputa | ii | `self.via_resolucao != self.VIA_COMPETENCIA_TRANCADA` |
| chamados/models.py:3560 | q_validada_pelo_ato | validada_em | PerguntaDisputa | vi | `Q(validada_em__isnull=False)` |
| chamados/models.py:3599 | q_vivas | materializada_em | PerguntaDisputa | vi | `Q(materializada_em__isnull=False)` |
| chamados/models.py:3599 | q_vivas | via_resolucao | PerguntaDisputa | vi | `Q(via_resolucao__in=_IDENT)` |
| chamados/models.py:3601 | q_vivas | materializada_em | PerguntaDisputa | vi | `Q(materializada_em__isnull=False)` |
| chamados/models.py:3601 | q_vivas | via_resolucao | PerguntaDisputa | vi | `Q(via_resolucao__in=cls.VIAS_QUE_VENCEM_A_RESPOSTA)` |
| chamados/models.py:3706 | _recusa_par_ilegal | respondida_em | PerguntaDisputa | vi | `values_list('respondida_em', 'resposta_colab')` |
| chamados/models.py:3706 | _recusa_par_ilegal | resposta_colab | PerguntaDisputa | vi | `values_list('respondida_em', 'resposta_colab')` |
| chamados/models.py:3707 | _recusa_par_ilegal | respondida_em | PerguntaDisputa | ii | `self.respondida_em if ... else _banco[0]` |
| chamados/models.py:3708 | _recusa_par_ilegal | resposta_colab | PerguntaDisputa | ii | `self.resposta_colab if ... else _banco[1]` |

linhas: 44 de 44

Totais por classe: i **0**; ii **20**; iii **0**; iv **4**; v **2**; vi **14**. As outras **4** leituras têm receptor DecisaoPergunta/ato e não recebem classe. [chamados/models.py:2004](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:2004), [chamados/models.py:2089](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:2089), [chamados/models.py:2221](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:2221), [chamados/models.py:3235](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:3235).

perguntas de lei: 0

## 19/11 01:51 · sessao be0f05997da0 · E4C-F5 · censo · origem=Code

| arquivo:linha | função | campo | receptor | classe | trecho literal curto |
|---|---|---|---|---|---|
| chamados/models.py:3774 | `<modulo>` | via_resolucao | PerguntaDisputa | vi | `~models.Q(via_resolucao__in=VIAS_FORA_DO_FATO)` |
| chamados/models.py:3780 | `<modulo>` | via_resolucao | PerguntaDisputa | vi | `~models.Q(via_resolucao__in=VIAS_FORA_DO_FATO)` |
| chamados/models.py:3815 | resposta_display | resposta_colab | outro modelo: LeituraDoAto | — | `DecisaoPergunta.leitura(self).resposta_colab` |
| chamados/models.py:4251 | linhas_da_projecao_sem_linha | respondida_em | PerguntaDisputa | ii | `p.respondida_em is not None` |
| chamados/models.py:4251 | linhas_da_projecao_sem_linha | validada_em | PerguntaDisputa | ii | `p.validada_em is not None` |
| chamados/models.py:4260 | linhas_da_projecao_sem_linha | respondida_em | PerguntaDisputa | ii | `p.respondida_em is not None` |
| chamados/models.py:4261 | linhas_da_projecao_sem_linha | respondida_em | PerguntaDisputa | ii | `ja.add((p.pk, cls.RESPOSTA, p.respondida_em))` |
| chamados/models.py:4262 | linhas_da_projecao_sem_linha | respondida_em | PerguntaDisputa | ii | `ato_em=p.respondida_em` |
| chamados/models.py:4263 | linhas_da_projecao_sem_linha | resposta_colab | PerguntaDisputa | ii | `ato_texto=p.resposta_colab or ''` |
| chamados/models.py:4265 | linhas_da_projecao_sem_linha | validada_em | PerguntaDisputa | ii | `p.validada_em is not None` |
| chamados/models.py:4266 | linhas_da_projecao_sem_linha | validada_em | PerguntaDisputa | ii | `ja.add((p.pk, cls.VALIDACAO, p.validada_em))` |
| chamados/models.py:4267 | linhas_da_projecao_sem_linha | validada_em | PerguntaDisputa | ii | `ato_em=p.validada_em` |
| chamados/models.py:4268 | linhas_da_projecao_sem_linha | resposta_colab | PerguntaDisputa | ii | `ato_texto=p.resposta_colab or ''` |
| chamados/reconciliador.py:368 | lavrar_sem_turno_aberto | materializada_em | PerguntaDisputa | ii | `if p.materializada_em is not None` |
| chamados/reconciliador.py:673 | qs_perguntas_absolviveis | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=True` |
| chamados/reconciliador.py:674 | qs_perguntas_absolviveis | validada_em | PerguntaDisputa | vi | `validada_em__isnull=True` |
| chamados/reconciliador.py:1093 | _orfas_abertas | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=True` |
| chamados/reconciliador.py:1093 | _orfas_abertas | validada_em | PerguntaDisputa | vi | `validada_em__isnull=True` |
| chamados/reconciliador.py:1188 | encerrar_orfas_pela_lampada | via_resolucao | PerguntaDisputa | iv | `'via_resolucao': p.via_resolucao or ''` |
| chamados/reconciliador.py:1189 | encerrar_orfas_pela_lampada | materializada_em | PerguntaDisputa | iv | `p.materializada_em.isoformat()` |
| chamados/reconciliador.py:1199 | encerrar_orfas_pela_lampada | materializada_em | PerguntaDisputa | iv | `'materializada_em': p.materializada_em.isoformat()` |
| chamados/reconciliador.py:1199 | encerrar_orfas_pela_lampada | via_resolucao | PerguntaDisputa | iv | `'via_resolucao': p.via_resolucao` |
| chamados/reconciliador.py:1549 | perguntas_orfas_reatavies | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=True` |
| chamados/services/acoes_disputa.py:349 | acao_corrigir_resposta | validada_em | PerguntaDisputa | i | `if perg.validada_em` |
| chamados/services/acoes_disputa.py:357 | acao_corrigir_resposta | resposta_colab | PerguntaDisputa | v | `antes = (perg.resposta_colab or '').strip()` |
| chamados/services/competencia_trancada.py:276 | reabrir | via_resolucao | PerguntaDisputa | vi | `via_resolucao=VIA` |
| chamados/services/declaracao_texto.py:228 | _resposta_validada | validada_em | PerguntaDisputa | i | `return perg.validada_em is not None` |
| chamados/services/declaracao_texto.py:418 | declarar_hora_colab | resposta_colab | PerguntaDisputa | ii | `(perg.resposta_colab or '') == hora` |
| chamados/services/disputa_emissao.py:228 | _derivar_chamado_destino | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=True).exists()` |
| chamados/services/disputa_emissao.py:337 | gerar_perguntas_disputa | via_resolucao | PerguntaDisputa | vi | `via_resolucao__in=PerguntaDisputa.VIAS_FORA_DO_FATO` |
| chamados/services/disputa_emissao.py:409 | gerar_perguntas_disputa | via_resolucao | PerguntaDisputa | vi | `via_resolucao__in=PerguntaDisputa.VIAS_FORA_DO_FATO` |
| chamados/services/disputa_emissao.py:428 | gerar_perguntas_disputa | via_resolucao | PerguntaDisputa | vi | `via_resolucao__in=PerguntaDisputa.VIAS_FORA_DO_FATO` |
| chamados/services/disputa_emissao.py:446 | gerar_perguntas_disputa | via_resolucao | PerguntaDisputa | vi | `via_resolucao__in=PerguntaDisputa.VIAS_FORA_DO_FATO` |
| chamados/services/disputa_emissao.py:776 | motivo_fora_da_fabrica | materializada_em | PerguntaDisputa | vi | `filter(materializada_em__isnull=False).exists()` |
| chamados/services/disputa_emissao.py:1084 | criar_disputa_a_partir_de_chamados_orfaos | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=True` |
| chamados/services/disputa_emissao.py:1104 | criar_disputa_a_partir_de_chamados_orfaos | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=True` |
| chamados/services/disputa_emissao.py:1340 | criar_disputa_ancora_de_chamado | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=True` |
| chamados/services/disputa_emissao.py:1484 | responder_pergunta_disputa_core | validada_em | PerguntaDisputa | ii | `_validada_antes = (p.validada_em, p.validada_por_id)` |
| chamados/services/disputa_emissao.py:1485 | responder_pergunta_disputa_core | validada_em | PerguntaDisputa | ii | `if p.validada_em is not None` |

linhas: 39 de 39

Totais: i = 2; ii = 14; iii = 0; iv = 4; v = 1; vi = 17; outro receptor = 1.

- `acoes_disputa.py:349`: chamada por `chamados/views.py:536` e `chamados/services/fio.py:686`; **sim**, a guarda impede a correção antes da chamada à porta escritora em `acoes_disputa.py:375`.
- `declaracao_texto.py:228`: chamada em `declaracao_texto.py:286`, `:379` e `:669`; **sim**, as duas últimas guardam as portas que chamam o escritor em `:420` e `:694`; `:286` consulta a condição para prever a recusa.

perguntas de lei: 0 — L-130 e os cortes F5 constam em `app/docs/LEIS.md:151` e `app/docs/CORTES.md:560–561`.

## 19/11 01:50 · sessao 301527d19ae3 · E4C-F3-LAVRA-FOTO · construtor · origem=Code

Atualizei o detector em [test_f3_lavra_foto_sem_decisao.py](/home/ronald/wt-codex-e4c-f3-lavra-foto-013922/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:41): ele percorre chamadas transitivas a funções do módulo, reconhece reexportações e acessos dinâmicos, e verifica caminhos ORM e SQL usando os relacionamentos lidos do modelo por AST. [teste:16](/home/ronald/wt-codex-e4c-f3-lavra-foto-013922/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:16) [nao verificado]

A sintaxe foi verificada por parse AST. **Os testes não foram executados**, conforme pedido; resultado dos controles e mordida dos mutantes: **[não verificado]**. [nao verificado]

Para executar cada mutante separadamente, no diretório `app`:

```bash
for teste in \ [nao verificado]
  test_MORDE_m7_helper_transitivo \
  test_MORDE_m8_reexportacao \
  test_MORDE_m9_getattr_dinamico \
  test_MORDE_m10_prefetch \
  test_MORDE_m11_lookup_dict \
  test_MORDE_m12_sql
do
  PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "chamados.tests.test_f3_lavra_foto_sem_decisao.LavraFotoSemDecisaoTest.$teste" [nao verificado]
done
```

Testes novos: `test_related_names_do_modelo_nao_vazios`, `test_MORDE_m7_helper_transitivo`, `test_MORDE_m8_reexportacao`, `test_MORDE_m9_getattr_dinamico`, `test_MORDE_m10_prefetch`, `test_MORDE_m11_lookup_dict`, `test_MORDE_m12_sql` e `test_controle_helper_nao_chamado_nao_conta`. [teste:212](/home/ronald/wt-codex-e4c-f3-lavra-foto-013922/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:212) [nao verificado]

## 19/11 01:50 · sessao ab0b5e747455 · - · - · origem=Ronald (tmux)

Classificação enviada ao agente principal, com receptores, classes e trechos das 20 linhas de `auditar_invariantes_chamados.py`.

## 19/11 01:50 · sessao ea3987f5fad5 · - · - · origem=Ronald (tmux)

Classificação do backfill enviada ao agente principal, com receptores, trechos e referências de linha. Nenhuma das linhas examinadas foi classificada como classe i.

_7254 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
