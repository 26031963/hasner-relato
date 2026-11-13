# CODEX-ESTADO

> Ultima resposta processada: **13/11/2026 09:11** · publicado em **13/11/2026 09:13** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 13/11 09:11 · sessao a6eba0319a4e · FERIADO-ESCALA · sonda · origem=Code

```text
sonda_leitura: sonda_16.py [b1b8e75be9d0a984] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

== universo ativa=True total 535
  ciclo=12x36 base=12x36 trabalha=False : 2
  ciclo=12x36 base=12x36 trabalha=True : 332
  ciclo=5x2 base=comercial trabalha=False : 16
  ciclo=5x2 base=comercial trabalha=True : 15
  ciclo=5x2 base=turno_partido trabalha=False : 4
  ciclo=5x2 base=turno_partido trabalha=True : 1
  ciclo=6x1 base=comercial trabalha=False : 70
  ciclo=6x1 base=comercial trabalha=True : 59
  ciclo=6x1 base=turno_partido trabalha=False : 1
  ciclo=6x1 base=turno_partido trabalha=True : 23
  ciclo=intermitente base=intermitente trabalha=True : 8
  ciclo=personalizado base=comercial trabalha=False : 1
  ciclo=personalizado base=comercial trabalha=True : 3
== universo vigente(juiza) total 536
  ciclo=12x36 base=12x36 trabalha=False : 2
  ciclo=12x36 base=12x36 trabalha=True : 333
  ciclo=5x2 base=comercial trabalha=False : 16
  ciclo=5x2 base=comercial trabalha=True : 15
  ciclo=5x2 base=turno_partido trabalha=False : 4
  ciclo=5x2 base=turno_partido trabalha=True : 1
  ciclo=6x1 base=comercial trabalha=False : 70
  ciclo=6x1 base=comercial trabalha=True : 59
  ciclo=6x1 base=turno_partido trabalha=False : 1
  ciclo=6x1 base=turno_partido trabalha=True : 23
  ciclo=intermitente base=intermitente trabalha=True : 8
  ciclo=personalizado base=comercial trabalha=False : 1
  ciclo=personalizado base=comercial trabalha=True : 3
== contador existente vinculos_5x2_6x1_trabalha_feriado: {'total': 98, 'declarados': 64, 'nao_declarados': 34}
== 5x2/6x1 ativos com True: ec col te ciclo base posto posto.opera_em_feriado criado_em data_inicio data_fim padrao_juiz nasceu_apos_16_09 vigente
Traceback (most recent call last):
  File "<string>", line 30, in <module>
AttributeError: 'EscalaColaborador' object has no attribute 'criado_em'
erros 1
```

## 13/11 09:01 · sessao a880ed8aade7 · O168 · verificador · origem=Code

**REPROVA — bloqueante no selo do item F:** o novo “fecho por chamada” segue qualquer referência `ast.Name`, permitindo contar uma auxiliar que nunca é chamada. Isso cria falso positivo que o detector anterior fornecido rejeitava. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:287`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:328`.)

**A. Curas da volta 3**

| Item | Resultado e mordida |
|---|---|
| **(1) Contrato** | **Curado.** Cobra delegação no wrapper e procura `materializada_em is None` no helper. Em memória, passa no atual e falha retirando essa condição ou substituindo a delegação por `return []`. (`app/chamados/tests/test_contract_dedup_status.py:141`; `app/chamados/tests/test_contract_dedup_status.py:143`.) |
| **(2) Fixture** | **Curado o `AttributeError`.** Injeta o helper real sem bind; executei os quatro testes, todos passaram. (`app/chamados/tests/test_e4b_c4v14_memoria.py:50`; `app/chamados/tests/test_e4b_c4v14_memoria.py:121`; `app/chamados/tests/test_e4b_c4v14_memoria.py:143`.) |
| **F** | **Parcialmente curado.** O teste novo morde auxiliar chamada com leitura crua, constante de módulo e juiz preservado sem referência; também aceita chamada por apelido. Executei essas asserções em memória. Continua contando auxiliar apenas referenciada, contrariando o requisito “função não chamada não conta”. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:275`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:376`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:385`.) |

**B. Terminação e escapes**

Termina para AST finita: há finitos nomes em `defs`; cada nome entra em `vistos` antes da expansão e só é expandido uma vez, inclusive em ciclos. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:277`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:283`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:285`.)

| Caminho da leitura crua | Detector |
|---|---|
| Método de classe definida no módulo | **Escapa:** classe/método não entram em `defs`. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:277`.) |
| Lambda atribuída no módulo | **Escapa:** não é `FunctionDef`. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:277`.) |
| Lambda local ou função aninhada dentro de função alcançada | **Detecta:** percorre seus corpos, mesmo sem execução. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:311`.) |
| Função importada de outro módulo | **Não inspeciona seu corpo.** (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:305`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:277`.) |

No CSF atual, o fecho contém somente `pode_executar_csf` e `perguntas_que_seguram_csf`; obtive `raw=False`, `judge=True`. Existem chamadas aos métodos canônicos importados do juiz, sem leitura crua nesses dois corpos. (`app/ponto/supra_juiz.py:557`; `app/ponto/supra_juiz.py:558`; `app/ponto/supra_juiz.py:559`; `app/ponto/supra_juiz.py:573`.)

**C. Falso positivo e comparação**

**Import isolado não basta:** o detector exige `ast.Call`. Porém, `j(ch); return True` passa, embora o resultado do juiz seja descartado; origem do import e sombreamento também não são validados. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:326`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:330`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:334`.)

O bloqueante mais direto é este exemplo sintético, reproduzido em memória: `_le_respondida_cru=False` e `_pergunta_ao_juiz=True`, embora nenhuma chamada ao juiz ocorra na execução. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:287`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:332`.)

```python
def _isca(ch):
    return ch.sem_veredito_entre()

def pode_executar_csf(ch):
    _isca
    return True
```

Na reconstrução do algoritmo anterior fornecido, esse exemplo dava `judge=False`; agora dá `True`. Há enfraquecimento nesse formato. A falta de vínculo com o retorno já existia para chamadas por atributo. Conferência contra o commit-base Git exato: **[não verificado]**. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:287`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:332`.)

**D. Mutantes do contrato e da fixture**

O **contrato morde** a retirada de `materializada_em is None`; a **fixture não**: os quatro testes passaram também com esse mutante. Suas perguntas começam com `materializada_em=None`, sem cenário materializado que diferencie as versões. (`app/chamados/tests/test_contract_dedup_status.py:143`; `app/chamados/tests/test_e4b_classe_e_memoria.py:285`; `app/chamados/tests/test_e4b_c4v14_memoria.py:40`.)

Substituir a delegação por `return []` também deixou os quatro testes da fixture verdes; quem barrou esse mutante foi a nova asserção textual do contrato. Portanto, a fixture cura a dependência ausente, mas não certifica essas duas propriedades. (`app/chamados/tests/test_contract_dedup_status.py:141`; `app/chamados/tests/test_e4b_c4v14_memoria.py:47`; `app/chamados/tests/test_e4b_c4v14_memoria.py:50`.)

**E. Outros testes** [nao verificado]

Encontrei substituição do método por mock em `test_termo_fechar`; ela preserva a interface pública e não extrai o corpo. Outro extrator por fonte/AST/injeção não foi localizado; ausência absoluta, inclusive por nomes construídos dinamicamente: **[não verificado]**. (`app/chamados/tests/test_termo_fechar.py:28`.)

Há ainda fixture runtime incompatível fora do delta: `_D` não fornece `respostas_sem_veredito`, e `_P` não fornece `validada_em`. Reproduzi três `AttributeError` nos casos existentes; isso não é regressão nova demonstrada deste delta. (`app/ponto/tests/test_supra_juiz_executor.py:29`; `app/ponto/tests/test_supra_juiz_executor.py:16`; `app/ponto/tests/test_supra_juiz_executor.py:40`; `app/ponto/tests/test_supra_juiz_executor.py:43`; `app/ponto/tests/test_supra_juiz_executor.py:46`; `app/chamados/models.py:1899`.)

Execução Django/ORM integral, banco, logs e identidade Git do HEAD: **[não verificado]**.

## 13/11 09:00 · sessao d9cdaedcce68 · - · - · origem=Ronald (tmux)

**B. Termina, mas não é um fecho estrito por chamada.** `defs` contém finitos `FunctionDef` do módulo; cada nome é marcado antes de expandir e expandido uma vez, mesmo havendo recursão. Segue qualquer `ast.Name`, inclusive mera referência sem chamada. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:277`; `:281`; `:285`; `:287`.)

Reproduzi em memória:

| Caminho com leitura crua | `_le_respondida_cru` | Razão |
|---|---:|---|
| Método de classe definida no módulo | `False` | Classe/método ausentes de `defs`. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:277`.) |
| Lambda atribuída no módulo | `False` | Atribuição/lambda ausentes de `defs`. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:277`.) |
| Lambda dentro da função alcançada | `True` | `ast.walk` percorre o corpo. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:311`.) |
| Função aninhada na função alcançada | `True` | Corpo aninhado percorrido, mesmo sem chamada. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:286`; `:311`.) |
| Função importada de outro módulo | `False` | Seu corpo não pertence à árvore analisada. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:305`; `:277`.) |

No CSF atual, o fecho resultou apenas em `pode_executar_csf` e `perguntas_que_seguram_csf`, com `raw=False` e `judge=True`; este último chama os métodos canônicos do juiz importado. Não há nesses dois corpos método cru de classe do próprio módulo, lambda, função aninhada ou auxiliar importada crua. (`app/ponto/supra_juiz.py:552`; `:557`; `:558`; `:559`; `:573`.) Runtime de outros módulos fora desses corpos: `[não verificado]`.

**C. Import isolado não passa; chamada sem alimentar o retorno passa.** Existe exigência explícita de `ast.Call`, portanto `from chamados.models import sem_veredito_entre as j; def pode_executar_csf(ch): return True` devolveu `False`. Já `j(ch); return True` devolveu `True`; o detector não examina dependência do retorno, módulo de origem do import nem sombreamento do apelido. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:326`; `:330`; `:334`.)

Também reproduzi `judge=True` com uma auxiliar arbitrária contendo chamada ao juiz apenas **referenciada**, nunca chamada: o fecho segue nomes, não `ast.Call`. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:287`; `:328`.)

Na reconstrução do detector anterior do anexo, a chamada descartada por apelido e a auxiliar arbitrária meramente referenciada davam `False`; agora dão `True`. Portanto há novos falsos positivos nesses formatos. A falta de vínculo com o retorno já existia para chamadas por atributo: `ch.sem_veredito_entre(ch); return True` passou nas duas versões. Comparação com commit Git exato: `[não verificado]`; algoritmo anterior reproduzido do diff fornecido, implementação atual em `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:321`.

O novo selo também melhora um caso concreto: helper chamado `perguntas_que_seguram_csf`, preservado e totalmente sem referência, dava `True` antes e agora dá `False`; esse caso é cobrado pelo teste novo. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:370`; `:377`.)

## 13/11 08:59 · sessao c60966cee306 · - · - · origem=Ronald (tmux)

No recorte literal de E, não localizei outro teste que extraia por fonte/AST/injeção `respostas_sem_veredito` ou `sem_veredito_entre` além dos três arquivos do delta; ausência absoluta por nomes construídos dinamicamente: `[não verificado]`. O teste `test_termo_fechar` substitui o método com mock, sem extrair seu corpo. (`app/chamados/tests/test_termo_fechar.py:28`.)

Há, porém, outra fixture runtime incompatível na cópia, fora do delta:

- `_D` só instala `perguntas`, mas o helper exige `d.respostas_sem_veredito()`: os casos das linhas 40 e 43 levantam `AttributeError`. (`app/ponto/tests/test_supra_juiz_executor.py:27`; `app/ponto/tests/test_supra_juiz_executor.py:40`; `app/ponto/tests/test_supra_juiz_executor.py:43`; `app/ponto/supra_juiz.py:558`.)
- `_P` só instala `respondida_em`; o caso direto da linha 46 levanta `AttributeError` em `validada_em` no corpo do juiz. (`app/ponto/tests/test_supra_juiz_executor.py:14`; `app/ponto/tests/test_supra_juiz_executor.py:46`; `app/chamados/models.py:1899`.)
- O caso vazio retorna `True`. (`app/ponto/tests/test_supra_juiz_executor.py:49`; `app/ponto/supra_juiz.py:573`.)

Reproduzi esses quatro resultados em memória extraindo via AST os corpos reais de `perguntas_que_seguram_csf`, `pode_executar_csf`, `sem_veredito_entre` e as classes reais da fixture, com `dia_da_pergunta_trancado=False`; execução Django integral: `[não verificado]`. (`app/ponto/supra_juiz.py:552`; `app/ponto/supra_juiz.py:563`; `app/chamados/models.py:1889`; `app/ponto/tests/test_supra_juiz_executor.py:14`.)

Atribuir essa falha como regressão nova deste delta seria indevido: o diff fornecido altera somente os outros três testes; reprodução contra o commit-base exato: `[não verificado]`. [nao verificado]

## 13/11 08:54 · sessao e7dedf56e2fa · O168 · verificador · origem=Code

**REPROVA: a extração do predicado quebra duas verificações existentes.** As curas dos três bloqueantes anteriores estão no código, mas ficaram estas regressões:

1. `test_porta_humana_de_disputa_presta_contas` exige `materializada_em is None` no fonte de `respostas_sem_veredito`; agora essa expressão está no helper. Reproduzi a falha dessa asserção em memória. (`app/chamados/tests/test_contract_dedup_status.py:140`; `app/chamados/models.py:1886`; `app/chamados/models.py:1899`.)
2. Os quatro testes de `test_e4b_c4v14_memoria` terminam em `AttributeError: 'Objeto' object has no attribute 'sem_veredito_entre'`: a fixture injeta somente os dois métodos antigos, e o wrapper passou a depender do terceiro. Execução realizada em memória, sem ORM. (`app/chamados/tests/test_e4b_c4v14_memoria.py:47`; `app/chamados/tests/test_e4b_c4v14_memoria.py:105`; `app/chamados/models.py:1886`.)

Conferência independente do hash e do commit-base por Git, execução ORM e nova medição de produção: **[não verificado]**. A comparação abaixo usa a cópia e o diff fornecidos. [nao verificado]

**A. Bloqueantes da preverif2**

| Bloqueante anterior | Cura e mordida |
|---|---|
| **1 — critérios paralelos** | A retratação agora chama o corpo único, mantendo seu universo de crédito e explicitando `ignorar_tranca=True`. O teste novo cobra carimbo sem texto, texto sem carimbo e exclusão de validada; restaurar o antigo **E** contraria as duas primeiras asserções. Execução ORM/mutante: **[não verificado]**. (`app/chamados/juizes.py:2033`; `app/chamados/juizes.py:2035`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:259`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:263`.) |
| **2 — nota omitia destino** | Curado: o gate calcula `_pend` uma vez e usa essa mesma lista na nota. O teste do destino exige nota contendo `p3`; voltar à enumeração exclusiva da origem contraria essa asserção. Execução ORM/mutante: **[não verificado]**. (`app/chamados/models.py:719`; `app/chamados/models.py:735`; `app/chamados/models.py:739`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:248`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:250`.) |
| **3 — ponto fixo oscilante** | Curado pela união monotônica. O teste real de terminação passou na implementação atual e falhou, após dez segundos, contra uma reconstrução em memória do algoritmo escalar oscilante. Execução contra o commit antigo exato: **[não verificado]**. (`app/chamados/tests/test_o168_fechadores_fora_do_gate.py:113`; `app/chamados/tests/test_o168_fechadores_fora_do_gate.py:115`; `app/chamados/tests/test_o168_fechadores_fora_do_gate.py:256`; `app/chamados/tests/test_o168_fechadores_fora_do_gate.py:271`.) |

O cartório **não foi migrado**. Seu descarte da triagem encontra fundamento no próprio código: ele protege resposta **com texto** contra aquele bloco de ressuscitação/dedup, inclusive depois de validação; não calcula “resposta sem veredito”. Portanto, não se deve apresentá-lo como leitor do mesmo predicado. (`app/ponto/services/cartorio.py:1637`; `app/ponto/services/cartorio.py:1641`; `app/ponto/services/cartorio.py:1648`; `app/ponto/services/cartorio.py:1665`.)

**B. Preservação para os chamadores antigos**

Pelo diff fornecido, a extração preserva universo, argumento e condições:

```text
validada_em é nulo
E materializada_em é nulo
E (respondida_em preenchido OU resposta_colab.strip() preenchido)
E (ignorar_tranca OU dia não trancado)
```

O wrapper continua entregando `self.perguntas.all()` e encaminhando `ignorar_tranca`. (`app/chamados/models.py:1886`; `app/chamados/models.py:1898`.)

Permanecem na chamada padrão: `pode_fechar`, `fechar`, desligamento, ficha, reconciliador e fila por causa; competência trancada solicita explicitamente `True`. (`app/chamados/models.py:1909`; `app/chamados/models.py:1942`; `app/colaboradores/services/desligamento.py:141`; `app/colaboradores/services/ficha.py:169`; `app/chamados/reconciliador.py:1164`; `app/chamados/services/fila_por_causa.py:226`; `app/chamados/services/competencia_trancada.py:126`.)

Nenhum desses chamadores passou a fornecer outro universo ao wrapper. A tranca lê o colaborador **da disputa da pergunta** e o dia da própria pergunta; conversa fornece dia `None`. Há também consulta contextual por identidade da instância, já dentro da autoridade da tranca. A extração não troca esses insumos pelo chamado receptor. (`app/chamados/models.py:1886`; `app/chamados/juizes.py:334`; `app/chamados/juizes.py:986`; `app/chamados/juizes.py:991`.)

**C. CSF e retratação**

**Não devolvem necessariamente o mesmo resultado.** Compartilham o corpo parametrizado, mas CSF respeita a tranca e retratação a ignora; também usam universos diferentes. Essa diferença está declarada e justificada no código pela preservação dos quatro vivos que seriam soltos com a tranca e pelos dois adicionais retidos pelo **OU**. (`app/ponto/supra_juiz.py:558`; `app/ponto/supra_juiz.py:559`; `app/chamados/juizes.py:2012`; `app/chamados/juizes.py:2022`; `app/chamados/juizes.py:2035`.)

Não encontrei exigência executável de igualdade: a retratação consulta sua retenção antes de chegar ao gate; materialização e reabertura por resposta usam o mesmo leitor de irmãs. O teste novo compara explicitamente com `respostas_sem_veredito(ignorar_tranca=True)`. (`app/chamados/reconciliador.py:519`; `app/chamados/reconciliador.py:524`; `app/chamados/services/materializacao.py:768`; `app/chamados/services/disputa_emissao.py:2009`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:262`.)

**D. DESTINO de um PAI**

A contradição textual foi corrigida: HEAD declara expressamente origem **e destino**, inclusive perguntas de outra disputa. (`app/chamados/models.py:705`.)

O teste **fabrica diretamente o vínculo** `chamado_destino=pai`; depois exercita a cascata canônica de fechamento da origem. Ele não demonstra o produtor desse vínculo. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:230`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:233`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:241`.)

Os seletores examinados excluem pais `disputa_supervisao`: derivação normal, fábrica de órfãos e HIST. (`app/chamados/services/disputa_emissao.py:198`; `app/chamados/services/disputa_emissao.py:753`; `app/chamados/services/disputa_emissao.py:1066`; `app/chamados/reconciliador.py:1587`.)

A porta genérica `reapontar_resolvedora` permitiria e gravaria um destino desse tipo, sujeito às demais guardas; não há veto geral por categoria em `resolvedora_valida`. Isso prova admissibilidade pela porta, **não um fluxo produtor concreto**. Cenário de produção que efetivamente seleciona esse pai e ocorrência na frota: **[não verificado]**. (`app/chamados/services/fio.py:908`; `app/chamados/services/fio.py:926`; `app/chamados/juizes.py:2264`; `app/chamados/juizes.py:2283`.)

**E. Catraca dos fechadores**

Termina: existem finitos nomes e duas portas possíveis; cada passada produtiva acrescenta pelo menos um par nome–porta, sem remover pares. (`app/chamados/tests/test_o168_fechadores_fora_do_gate.py:68`; `app/chamados/tests/test_o168_fechadores_fora_do_gate.py:103`; `app/chamados/tests/test_o168_fechadores_fora_do_gate.py:113`.)

Não perde os apelidos reconhecidos pela versão escalar: importações e atributos continuam reconhecidos, e a união conserva possibilidades antes sobrescritas. Cadeias, atribuições anotadas e `getattr` literal também entram. A análise global pode superestimar possibilidades entre escopos. (`app/chamados/tests/test_o168_fechadores_fora_do_gate.py:78`; `app/chamados/tests/test_o168_fechadores_fora_do_gate.py:95`; `app/chamados/tests/test_o168_fechadores_fora_do_gate.py:105`.)

O teste de duas portas tem mordida contra a não terminação: verifica a thread após `join(10)` e depois cobra o achado `usa`; não depende apenas de o algoritmo retornar. (`app/chamados/tests/test_o168_fechadores_fora_do_gate.py:268`; `app/chamados/tests/test_o168_fechadores_fora_do_gate.py:271`.)

**F. Detectores AST**

A cobertura continua limitada:

- `q.respondida_em`, mesmo com apelido na base, e `getattr(q, 'respondida_em')` são detectados. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:284`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:286`.)
- Uma auxiliar nova — local ou importada sob apelido — pode fazer a leitura crua e escapar: `_funcoes_do_csf` seleciona somente dois nomes e não segue chamadas. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:269`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:282`.)
- `CAMPO = 'respondida_em'` no módulo e `getattr(q, CAMPO)` também escapam dessa busca. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:272`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:286`.)
- Uma chamada válida ao juiz como `j(ch)`, por apelido importado, não satisfaz `_pergunta_ao_juiz`: ele exige `ast.Attribute`. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:294`.)

Reproduzi o escape em memória: `pode_executar_csf` delega à auxiliar crua enquanto `perguntas_que_seguram_csf`, preservada mas não utilizada, contém a chamada ao juiz. Resultado: `_le_respondida_cru=False` e `_pergunta_ao_juiz=True`. A catraca procura uma chamada independentemente de ela alimentar o resultado. Isso demonstra limite do selo; não demonstra leitura crua no runtime atual. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:272`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:294`; `app/ponto/supra_juiz.py:573`.)

**G. Órfãs**

Coerente com B: o teste valida a resposta antes do consumo, e o predicado exclui validadas. O assert de CSF cobra elegibilidade, **não comprova pai fechado**. (`app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:148`; `app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:163`; `app/chamados/models.py:1899`.)

Duas falhas são compatíveis com uma tentativa por evento: o consumidor exclui IDs já lidos, pode receber eventos novos na rodada seguinte e incrementa `falhas` por lote malsucedido do colaborador. O resultado ORM concreto permanece **[não verificado]**. (`app/chamados/reconciliador.py:1344`; `app/chamados/reconciliador.py:1396`; `app/chamados/reconciliador.py:1404`; `app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:157`; `app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:168`.)

**H. Escritores, leitores e significado**

| Campo/valor | Escritores e leitores |
|---|---|
| `respondida_em` | Resposta humana, automática, recuperação pela trilha e simetria do carimbo preenchem; reabertura e ressuscitação limpam. O juiz aceita carimbo **OU** texto; C6 exige ambos naquele degrau; apresentação de estado usa o carimbo. (`app/chamados/services/disputa_emissao.py:1468`; `app/chamados/services/disputa_emissao.py:1653`; `app/chamados/services/disputa_emissao.py:1674`; `app/chamados/models.py:3188`; `app/chamados/services/disputa_emissao.py:1593`; `app/chamados/models.py:1613`; `app/chamados/models.py:1900`; `app/chamados/juizes.py:1417`; `app/chamados/models.py:3101`.) |
| `resposta_colab` | Resposta humana/automática escrevem; reabertura limpa. Juiz, cartório e materializador leem com finalidades distintas: pendência, proteção do texto e plantio. (`app/chamados/services/disputa_emissao.py:1467`; `app/chamados/services/disputa_emissao.py:1652`; `app/chamados/services/disputa_emissao.py:1593`; `app/chamados/models.py:1901`; `app/ponto/services/cartorio.py:1641`; `app/chamados/services/materializacao.py:412`.) |
| `validada_em` | Validação preenche; nova resposta/devolução/reabertura limpam; simetria também pode preencher. Juiz e C6 excluem preenchidas; validação evita repetição. (`app/chamados/services/validacao.py:134`; `app/chamados/services/disputa_emissao.py:1480`; `app/chamados/services/validacao.py:199`; `app/chamados/services/disputa_emissao.py:1593`; `app/chamados/models.py:3191`; `app/chamados/models.py:1899`; `app/chamados/juizes.py:1401`; `app/chamados/services/validacao.py:396`.) |
| `materializada_em` | `_escrever_carimbo` centraliza a atribuição; portas de carimbo, descarimbo e lote a utilizam. Juiz exclui preenchidas; C6 considera também via e testemunhas; materializador seleciona pendentes. (`app/chamados/models.py:3153`; `app/chamados/models.py:3185`; `app/chamados/models.py:3264`; `app/chamados/models.py:3322`; `app/chamados/models.py:1899`; `app/chamados/juizes.py:1412`; `app/chamados/services/materializacao.py:1378`.) |
| `chamado_destino` | Nascimento/reuso e `reapontar_resolvedora` escrevem. Fio lê como âncora; CSF lê a relação reversa; irmãs aplicam crédito; consumo procura perguntas do destino resolvido. (`app/chamados/models.py:3038`; `app/chamados/models.py:3070`; `app/chamados/services/fio.py:926`; `app/chamados/services/fio.py:117`; `app/ponto/supra_juiz.py:559`; `app/chamados/juizes.py:2034`; `app/chamados/services/disputa_emissao.py:1907`.) |
| Trilha de recusa | Gate → `_registrar_recusa` → `evento` → `LogAuditoria`. Deduplicador lê última recusa/transições; contador lê qualquer recusa histórica do motivo. Auditoria permanece best-effort. (`app/chamados/models.py:723`; `app/chamados/models.py:403`; `app/core/observ.py:98`; `app/core/observ.py:134`; `app/chamados/juizes.py:2520`.) |
| Nota `retido_pela_conversa` | Gate chama a porta de nota; timeline lê `FollowupChamado`. O tipo participa da chave, enquanto a nota persiste conteúdo e `evento_chave`. (`app/chamados/models.py:735`; `app/chamados/models.py:739`; `app/chamados/services/nota.py:88`; `app/chamados/services/nota.py:100`; `app/chamados/views.py:372`.) |
| `objeto_ref` | Gate monta PKs ordenados da união origem+destino; porta incorpora o valor ao SHA256 com chamado, tipo e autor. (`app/ponto/supra_juiz.py:558`; `app/ponto/supra_juiz.py:560`; `app/chamados/models.py:739`; `app/chamados/services/nota.py:64`.) |

**Não existe significado universal “validada = humano julgou” nem “materializada = batida plantada”.** A simetria da validação é declarada; carimbos de identidade também existem. São distinções preexistentes, e o novo helper preserva o critério antigo desses campos. (`app/chamados/services/validacao.py:19`; `app/chamados/models.py:3190`; `app/chamados/models.py:3224`; `app/chamados/models.py:1899`.)

A dedup da nota conserva o mecanismo: mesmo conjunto ordenado, chamado, tipo e autor recupera a mesma nota; adicionar pergunta de destino pode produzir chave nova. Pergunta presente nas duas relações aparece uma vez pelo conjunto. (`app/ponto/supra_juiz.py:558`; `app/ponto/supra_juiz.py:559`; `app/chamados/services/nota.py:100`; `app/chamados/services/nota.py:108`; `app/chamados/models.py:1745`.)

As identidades de recusa e nota diferem: trocar perguntas mantendo motivo/status pode criar nota nova sem recusa nova. O contador também mede **vivos com recusa histórica**, não necessariamente pendência atual; a nota permanece como histórico após fechamento. (`app/chamados/models.py:405`; `app/chamados/models.py:724`; `app/chamados/models.py:739`; `app/chamados/juizes.py:2520`; `app/chamados/views.py:372`.)

**I. Classes de reprovação**

Não identifiquei nesta cura segundo juiz, segundo escritor ou derivação paralela: o wrapper delega ao corpo único, CSF compõe a lista e as escritas usam as portas existentes. Também não há nova leitura de relógio ou aquisição de trava nesses corpos alterados. (`app/chamados/models.py:1886`; `app/chamados/models.py:1895`; `app/ponto/supra_juiz.py:558`; `app/chamados/models.py:723`; `app/chamados/models.py:735`.)

Há **escape demonstrado da catraca AST por auxiliar**, além das duas regressões de testes que bloqueiam esta cópia; não cabe chamar todos os REDs de inertes, pois os casos direto, `getattr` e oscilação têm mordida específica. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:272`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:315`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:318`; `app/chamados/tests/test_o168_fechadores_fora_do_gate.py:271`; `app/chamados/tests/test_contract_dedup_status.py:140`; `app/chamados/tests/test_e4b_c4v14_memoria.py:47`.)

Pouso antes de pré-requisito, concorrência executada e ordem efetiva de implantação: **[não verificado]**. [nao verificado]

## 13/11 08:52 · sessao 8bf251387a36 · - · - · origem=Ronald (tmux)

- **D:** a contradição da docstring foi curada: HEAD diz expressamente que o gate lê origem **e destino**, inclusive de outra disputa. (`app/chamados/models.py:705-708`.) A fixture fabrica esse vínculo diretamente com `objects.create(chamado_destino=pai)`; depois percorre a cascata canônica da origem. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:228-241`.) Não demonstra o produtor desse estado. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:230-233`.)

- Não achei um seletor normal que escolha um PAI `disputa_supervisao` como destino: o derivador exclui essa categoria, o agrupador de órfãos a recusa, o HIST também a exclui; âncora exige `ancora_limbo` e declarações exigem módulo de furo. (`app/chamados/services/disputa_emissao.py:198-201`; `app/chamados/services/disputa_emissao.py:753-754`; `app/chamados/services/disputa_emissao.py:1066`; `app/chamados/services/disputa_emissao.py:1088`; `app/chamados/reconciliador.py:1587-1590`; `app/chamados/services/disputa_emissao.py:1316-1317`; `app/chamados/services/declaracao_texto.py:320-321`; `app/chamados/services/declaracao_texto.py:603-605`.) A porta genérica permitiria o vínculo no mesmo colaborador e o grava, sem proibição por categoria; isso prova possibilidade pela porta, não um fluxo de produção que o seleciona. (`app/chamados/services/fio.py:908-929`; `app/chamados/juizes.py:2264-2287`.) Ocorrência e caminho produtor concreto: **[não verificado]**. Não afirmar impossível.

- **A(2): curado no código.** `_pend` é obtido uma vez e serve à recusa e à nota; a composição une origem+destino em conjunto ordenado. (`app/chamados/models.py:719-739`; `app/ponto/supra_juiz.py:559-561`.) O teste isola origem validada, confirma retenção pelo destino e exige uma nota contendo a pergunta destino; sem a cura da lista, falharia a existência ou o conteúdo dessa nota. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:235-250`.) Execução ORM/mutante: **[não verificado]**.

- **H — `chamado_destino`:** escritores persistentes são nascimento/reuso em `_nascer` e a recostura em `reapontar_resolvedora`; `SET_NULL` do banco também pode limpá-lo. (`app/chamados/models.py:3038-3039`; `app/chamados/models.py:3070`; `app/chamados/services/fio.py:808-815`; `app/chamados/services/fio.py:926-929`; `app/chamados/models.py:3441-3447`.) Leitores incluem a âncora do fio, CSF, crédito da retratação e consumo por resolução do destino. (`app/chamados/services/fio.py:97-110`; `app/ponto/supra_juiz.py:560`; `app/chamados/juizes.py:2034`; `app/chamados/services/disputa_emissao.py:1906-1910`.)

- **H — nota e dedup:** o gate escreve pela porta `anexar_ocorrencia`, como sistema e com PKs ordenados em `objeto_ref`; esse valor entra em SHA256 com chamado, tipo e autor. (`app/chamados/models.py:735-739`; `app/chamados/services/nota.py:62-65`; `app/chamados/services/nota.py:95-101`.) A porta mantém o mesmo mecanismo: unique e recuperação por `evento_chave`. (`app/chamados/models.py:1745-1747`; `app/chamados/services/nota.py:105-109`.) Mesmo conjunto repetido deduplica; acrescentar destino à lista pode gerar outra chave; a relação origem/destino não integra a identidade, apenas os PKs. (`app/ponto/supra_juiz.py:559-561`; `app/chamados/models.py:739`; `app/chamados/services/nota.py:64`.)

- **H — trilha:** gate → `_registrar_recusa` → `evento` → `LogAuditoria`; auditoria é best-effort. (`app/chamados/models.py:723-726`; `app/chamados/models.py:403-408`; `app/core/observ.py:94-98`; `app/core/observ.py:133-137`.) A recusa deduplica por motivo+status e transição posterior, enquanto a nota usa o conjunto de perguntas: mudar perguntas mantendo o status pode criar nota nova sem nova recusa. (`app/chamados/models.py:724`; `app/chamados/models.py:403-406`; `app/chamados/models.py:739`.) São granularidades diferentes.

- **H — leitores históricos:** timeline lê todos os followups; contador lê qualquer recusa histórica do motivo e filtra chamado atualmente vivo. (`app/chamados/views.py:372`; `app/templates/chamados/detalhe_local.html:112`; `app/chamados/juizes.py:2520-2523`.) Portanto o contador não prova pendência atual e pode voltar a contar um reaberto por recusa antiga; a docstring ainda o chama de fila esperando decisão. (`app/chamados/juizes.py:2515-2517`; `app/chamados/juizes.py:2520-2523`.) Nota permanece histórica mesmo quando o veredito posterior permite fechamento por cascata. (`app/chamados/views.py:372`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:169-184`.)

## 13/11 08:52 · sessao dbf201cd2e8f · - · - · origem=Ronald (tmux)

**E / A(3): curado.** O mapa só acumula pares `(nome, porta)`; há duas portas e quantidade finita de nomes, portanto cada passada produtiva acrescenta pelo menos um par e o laço termina. Importações, atributos, atribuições encadeadas/anotadas e `getattr` literal continuam reconhecidos; a união conserva as portas anteriormente reconhecidas. (`app/chamados/tests/test_o168_fechadores_fora_do_gate.py:68`; `app/chamados/tests/test_o168_fechadores_fora_do_gate.py:78`; `app/chamados/tests/test_o168_fechadores_fora_do_gate.py:95`; `app/chamados/tests/test_o168_fechadores_fora_do_gate.py:103`; `app/chamados/tests/test_o168_fechadores_fora_do_gate.py:113`.)

Executei em memória a lista fechada e quatro testes de mutantes, todos aprovados, incluindo `test_MORDE_duas_portas_no_mesmo_nome_termina`. O método real desse teste, executado contra reconstrução do mapa escalar oscilante descrito no anexo, falhou em 10,01 segundos com `ponto fixo dos apelidos nao termina`. Execução contra o commit antigo exato: **[não verificado]**. (`app/chamados/tests/test_o168_fechadores_fora_do_gate.py:171`; `app/chamados/tests/test_o168_fechadores_fora_do_gate.py:221`; `app/chamados/tests/test_o168_fechadores_fora_do_gate.py:234`; `app/chamados/tests/test_o168_fechadores_fora_do_gate.py:256`; `app/chamados/tests/test_o168_fechadores_fora_do_gate.py:274`.)

**F: cobertura incompleta, com escape reproduzido.** O detector visita somente funções cujo nome é `pode_executar_csf` ou `perguntas_que_seguram_csf`; não segue o grafo de chamadas. Assim, uma auxiliar nova pode ler `respondida_em` cru e ser chamada por `pode_executar_csf`, enquanto a antiga função da lista permanece contendo uma chamada ao juiz, embora já não seja utilizada: os dois asserts continuam passando. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:269`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:279`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:294`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:312`.)

Mutante executado em memória, resultado `_le_respondida_cru=False`, `_pergunta_ao_juiz=True`:

```python
def novo(ch):
    return ch.respondida_em is None

def perguntas_que_seguram_csf(ch):
    return ch.respostas_sem_veredito()

def pode_executar_csf(ch):
    return novo(ch)
```

O escape decorre diretamente das raízes fixas e da busca independente por qualquer chamada com aqueles nomes de atributo. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:272`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:294`.)

Outras distinções verificadas em memória:

- `q.respondida_em`, inclusive quando a base é um apelido, e `getattr(q, 'respondida_em')` são detectados. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:284`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:286`.)
- `CAMPO = 'respondida_em'` no módulo seguido de `getattr(q, CAMPO)` escapa da leitura crua: a constante está fora das duas funções visitadas. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:272`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:282`.)
- Helper importado sob apelido também escapa quando a leitura ocorre no helper; não há resolução de importações. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:269`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:283`.)
- Chamada válida ao juiz por apelido de importação, `j(ch)`, é recusada por `_pergunta_ao_juiz`, pois o detector exige `ast.Attribute`; isso é falso negativo. Comentário/docstring deixou de bastar. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:294`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:321`.)

**Classificação:** o bloqueante antigo de não terminação está curado; permanece limite demonstrado das catracas AST. Esses escapes sintéticos não provam leitura crua no código de produção atual. (`app/chamados/tests/test_o168_fechadores_fora_do_gate.py:113`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:272`; `app/ponto/supra_juiz.py:558`.)

## 13/11 08:51 · sessao d683fdfb3c73 · - · - · origem=Ronald (tmux)

B. O predicado antigo foi preservado na extração: mesmo `self.perguntas.all()`, mesmo argumento `ignorar_tranca`, mesmo filtro de validada/materializada e carimbo **OU** texto. A comparação com o baseline depende do diff fornecido; não existe `.git` nesta cópia. (`app/chamados/models.py:1886`; `app/chamados/models.py:1898`; [não verificado] para consulta independente ao commit.)

Os chamadores antigos continuam usando a chamada padrão: `pode_fechar`, `fechar`, desligamento, ficha, reconciliador e fila por causa. `competencia_trancada` continua solicitando `ignorar_tranca=True`, com justificativa própria para a pauta DP. (`app/chamados/models.py:1909`; `app/chamados/models.py:1942`; `app/colaboradores/services/desligamento.py:141`; `app/colaboradores/services/ficha.py:169`; `app/chamados/reconciliador.py:1164`; `app/chamados/services/fila_por_causa.py:226`; `app/chamados/services/competencia_trancada.py:124`.)

A tranca consulta o colaborador **da disputa da pergunta** e seu dia de turno; não consulta o chamado que recebe o universo. Conversa devolve dia `None`. Existe também cache por identidade da instância durante a consulta, mas essa leitura já pertence ao juiz da tranca. Nenhum chamador antigo passou a entregar outro universo ao wrapper. (`app/chamados/juizes.py:986`; `app/chamados/juizes.py:991`; `app/chamados/juizes.py:334`; `app/chamados/models.py:1886`.)

C. As duas aplicações não têm predicados finais idênticos: CSF usa a tranca; retratação deliberadamente a ignora. A diferença está declarada e justificada no código, inclusive com os números fornecidos na sonda: preservar quatro chamados que a tranca soltaria e segurar dois adicionais pela passagem de **E** para **OU**. (`app/ponto/supra_juiz.py:558`; `app/ponto/supra_juiz.py:559`; `app/chamados/juizes.py:2022`; `app/chamados/juizes.py:2035`.)

Não encontrei leitor executável exigindo igualdade entre os dois resultados: `_retratar` aplica a retenção mais restritiva antes de chegar à reconciliação do chamado. O teste compara a retratação com o juiz explicitamente chamado com `ignorar_tranca=True`. (`app/chamados/reconciliador.py:519`; `app/chamados/reconciliador.py:524`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:262`.)

G. O teste das órfãs é coerente com B: valida explicitamente a respondida e exige que CSF a libere. `falhas == 2` conta lotes do colaborador que falharam; os IDs já lidos são excluídos da próxima rodada, permitindo duas falhas com uma tentativa por evento. A execução ORM desse teste permanece [não verificado]. (`app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:148`; `app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:157`; `app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:163`; `app/chamados/reconciliador.py:1344`; `app/chamados/reconciliador.py:1396`; `app/chamados/reconciliador.py:1404`.)

**Bloqueante adicional confirmado: a extração quebra testes existentes.** [nao verificado]

- Executei em memória, sem banco nem gravação, `PYTHONDONTWRITEBYTECODE=1 python3 -B -m chamados.tests.test_e4b_c4v14_memoria`: **quatro testes, quatro erros** por `AttributeError: 'Objeto' object has no attribute 'sem_veredito_entre'`. A fixture injeta somente `respostas_sem_veredito` e `pode_fechar`; o primeiro agora precisa também do helper. (`app/chamados/tests/test_e4b_c4v14_memoria.py:47`; `app/chamados/tests/test_e4b_c4v14_memoria.py:105`; `app/chamados/models.py:1886`; `app/chamados/models.py:1909`.)
- A asserção de contrato exige literalmente `materializada_em is None` dentro de `inspect.getsource(DisputaSupervisao.respostas_sem_veredito)`. Confirmei por extração AST que agora resulta `False`: a expressão foi movida para o helper. A suíte ORM completa permanece [não verificado], mas essa asserção determinística está quebrada. (`app/chamados/tests/test_contract_dedup_status.py:140`; `app/chamados/models.py:1873`; `app/chamados/models.py:1886`; `app/chamados/models.py:1899`.)

## 13/11 08:38 · sessao b9fc8eea29c5 · O168 · sonda · origem=Code

```text
sonda_leitura: sonda_irmas.py [f45d25ff767c5a3f] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

chamados_creditados=659
perg_forma__base                                       921  amostra=[50363, 44921, 51544, 51545, 51548, 51549, 56197, 56644, 56867, 56655]
perg_forma__resp_sem_texto                             113  amostra=[33925, 28357, 28443, 29168, 29503, 29167, 29502, 29169, 32459, 32463]
perg_tranca__base                                        4  amostra=[47879, 47884, 47881, 20599]
perguntas_candidato_sem_tranca                        1034  amostra=[]
vivo_SEGURA_A_MAIS__default_tranca                       2  amostra=[23072, 33525]
vivo_SEGURA_A_MAIS__ignorar_tranca                       2  amostra=[23072, 33525]
vivo_SOLTA__default_tranca                               4  amostra=[33706, 33731, 33732, 33733]
vivo_segurado_hoje_base                                454  amostra=[2765, 5163, 5251, 6295, 6443, 7548, 10155, 10326, 12512, 13504]
vivos_creditados                                       456  amostra=[]
total_erros=0
```

## 13/11 08:37 · sessao 2677e37340bc · TIPO-NA-FONTE · verificador · origem=Code

REPROVA — **1. Bloqueante 1 parcialmente curado:** os três acessores passaram a tratar estados nomeados, mas `_orfas_da_ata`, expressamente apontado na volta 1, continua usando valores crus com fallback sem consultar `.estado` ([leitor_celula.py:192](/home/ronald/codex_ws/ed4b38773a9d8938246fdf42f88b5ecb890de8d4/app/escala/services/leitor_celula.py:192), [leitor_celula.py:193](/home/ronald/codex_ws/ed4b38773a9d8938246fdf42f88b5ecb890de8d4/app/escala/services/leitor_celula.py:193), [leitor_celula.py:194](/home/ronald/codex_ws/ed4b38773a9d8938246fdf42f88b5ecb890de8d4/app/escala/services/leitor_celula.py:194)).

**1. Estados e preservação dos valores**

**Nos três acessores perguntados, a cura funciona:** todos passam pela tabela que enumera os seis estados; estado desconhecido chama `_ilegal`. O tratamento de falsos nos LEGADO está declarado nessa tabela, preservando a operação da base (`app/escala/services/ata.py:123`, `app/escala/services/ata.py:128`, `app/escala/services/ata.py:143`, `app/escala/services/ata.py:155`, `app/escala/services/ata.py:161`).

Na tabela, `L` e `n` são os valores gravados; `B(x,d)` significa `x if x else d`.

| Estado | `horas_orfas` | `contar_orfas` | `tem_orfas` | Referência |
|---|---|---|---|---|
| `NAO_LAVRADA` | `[]` | `0` | `False` | `app/escala/services/ata.py:110`, `:123`, `:124`, `:163` |
| `LAVRADA` | `L` | `n` | `bool(L) or bool(n)` | `app/escala/services/ata.py:102`, `:135`, `:163` |
| `LEGADO_SEM_CHAVES` | `[]` | `0` | `False` | `app/escala/services/ata.py:96`, `:123`, `:124`, `:163` |
| `LEGADO_SO_CONTAGEM` | `[]` | `B(n,0)` | `bool(n)` | `app/escala/services/ata.py:98`, `:123`, `:137`, `:163` |
| `LEGADO_SO_LISTA` | `B(L,[])` | `0` | `bool(L)` | `app/escala/services/ata.py:100`, `:124`, `:137`, `:163` |
| `LEGADO_DESCASADA` | `B(L,[])` | `B(n,0)` | `bool(L) or bool(n)` | `app/escala/services/ata.py:102`, `:137`, `:163` |

**A igualdade de valor e tipo permanece também para não-lista/não-int:** por exemplo, `orfas='08:00', n_orfas='2'` devolve os próprios textos; valores falsos devolvem `[]`/`0`. Não há coerção para lista ou inteiro nesses acessores (`app/escala/services/ata.py:102`, `app/escala/services/ata.py:137`).

Para `None`, a resposta declarada é `[]/0/False`; a comparação com a expressão-base exige a normalização feita pelo leitor, pois `None` não possui `.get`. O vigia conserva essa normalização (`app/escala/services/ata.py:110`, `app/escala/services/ata.py:123`, `app/ponto/services/vigia_pareador_celula.py:74`).

Nenhum desses seis estados chega a `_ilegal`. Chegam ali ata não mapping ou estado desconhecido; o teste produz o segundo caso artificialmente (`app/escala/services/ata.py:90`, `app/escala/services/ata.py:131`, `app/escala/tests/test_tipo_item3_v5_expand.py:351`). Igualdade aqui é de **valor**, não identidade `is`: uma lista vazia LAVRADA é devolvida diretamente, enquanto `L or []` produziria outra lista (`app/escala/services/ata.py:135`).

**O resíduo do bloqueante 1 é concreto:** `_orfas_da_ata` chama `leitura_orfas`, aplica `_l.horas or []` e `_l.quantidade or 0`, e ignora o nome. Com o mesmo estado inventado usado pelo teste novo, esse leitor continua devolvendo `['08:00']`, enquanto `horas_orfas` recusa o estado. A nova proteção não alcança esse consumidor; não estou alegando divergência numérica atual (`app/escala/services/leitor_celula.py:191`, `app/escala/services/leitor_celula.py:193`, `app/escala/services/leitor_celula.py:194`, `app/escala/services/ata.py:131`, `app/escala/tests/test_tipo_item3_v5_expand.py:351`).

**2. Vigia, leitores crus e selo**

**O vigia preserva o conjunto da base nas entradas perguntadas**, inclusive sua dependência exclusiva da veracidade da contagem para decidir se examina a lista (`app/ponto/services/vigia_pareador_celula.py:74`, `app/ponto/services/vigia_pareador_celula.py:76`).

| Entrada/estado | Resultado |
|---|---|
| `None`, `{}`, não-mapping falso (`[]`, `''`, `0`, `False`) | Todos viram `{}` e devolvem `set()` (`app/ponto/services/vigia_pareador_celula.py:74`, `:76`). |
| `NAO_LAVRADA` | `None` é normalizado antes da porta; conjunto vazio (`app/ponto/services/vigia_pareador_celula.py:74`). |
| `LAVRADA` | Contagem zero: vazio; contagem positiva: conjunto de `L` (`app/ponto/services/vigia_pareador_celula.py:76`; `app/escala/services/ata.py:135`). |
| `LEGADO_SEM_CHAVES`, `LEGADO_SO_LISTA` | Contagem ausente: vazio, mesmo havendo lista (`app/escala/services/ata.py:124`; `app/ponto/services/vigia_pareador_celula.py:76`). |
| `LEGADO_SO_CONTAGEM` | Lista ausente: conjunto vazio, inclusive com contagem verdadeira (`app/escala/services/ata.py:123`; `app/ponto/services/vigia_pareador_celula.py:76`). |
| `LEGADO_DESCASADA` | Contagem falsa: vazio; verdadeira: conjunto do valor da lista, conservando também a falha de `set` para conteúdo incompatível (`app/escala/services/ata.py:137`; `app/ponto/services/vigia_pareador_celula.py:76`). |

Não-mapping **verdadeiro** não produzia conjunto na expressão-base fornecida: falhava no `.get`; agora é recusado por `_ilegal` (`app/escala/services/ata.py:90`).

Não identifiquei leitura produtiva literal crua dessas três chaves da **ATA** fora da porta. Os consumidores encontrados estão relacionados no item 4; acessos arbitrários por chave dinâmica permanecem **[não verificado]**.

`orfas` da **GRADE** é uma lista do dia construída em memória e também recomposta pelo leitor da célula; não é acesso direto à chave da ata (`app/escala/utils.py:1814`, `app/escala/utils.py:1837`, `app/escala/services/leitor_celula.py:424`). Exemplos de consumidores desse outro vocabulário: classificação, status, calendário e PDF (`app/escala/orfa_classe.py:58`, `app/ponto/status_dia.py:188`, `app/colaboradores/services/calendario.py:380`, `app/relatorios/pdf_espelho.py:477`).

**O selo morde qualquer constante literal** `n_orfas` ou `orfas_instantes` nos arquivos examinados — inclusive escrita ou texto sem relação com ata. Ele não identifica proveniência nem distingue leitura de escrita (`app/escala/tests/test_tipo_item3_v5_expand.py:361`, `app/escala/tests/test_tipo_item3_v5_expand.py:373`).

**Deixa passar:** `orfas`, chaves construídas dinamicamente, a porta e os diretórios excluídos de testes, migrations, logs e media. O mutante demonstra apenas o antigo literal `a.get('n_orfas')` do vigia (`app/escala/tests/test_tipo_item3_v5_expand.py:360`, `app/escala/tests/test_tipo_item3_v5_expand.py:371`, `app/escala/tests/test_tipo_item3_v5_expand.py:388`). O descriptor continua cobrindo atributos de instâncias; não instrumenta os payloads recebidos por `values_list` (`app/escala/services/ata_ci.py:17`, `app/escala/services/ata_ci.py:37`, `app/ponto/services/vigia_pareador_celula.py:72`).

**3. Helper e ocorrências ilegais**

**Bloqueante 3 curado no caminho dos dois testes:** cada chamada de `_ilegal` emite um registro, mesmo quando o sítio já está no `set`; o helper exige exatamente um registro antes de limpar. Duas ocorrências no mesmo sítio falham em `len(ocorrencias) == 1` (`app/escala/services/ata.py:48`, `app/escala/services/ata.py:49`, `app/escala/tests/test_tipo_item3_v5_expand.py:263`, `app/escala/tests/test_tipo_item3_v5_expand.py:269`).

`assertLogs` isolado pode aceitar várias ocorrências; quem exige uma é o helper. Zero ocorrências ilegais também reprova no helper, mesmo que outro `ERROR` tenha satisfeito o contexto (`app/escala/tests/test_tipo_item3_v5_expand.py:263`, `app/escala/tests/test_tipo_item3_v5_expand.py:264`, `app/escala/tests/test_tipo_item3_v5_expand.py:272`, `app/escala/tests/test_tipo_item3_v5_expand.py:274`).

A garantia cobre a janela capturada. Os dois testes chamam o helper imediatamente depois dela, sem chamada intermediária que produza outro ilegal (`app/escala/tests/test_tipo_item3_v5_expand.py:272`, `app/escala/tests/test_tipo_item3_v5_expand.py:274`, `app/escala/tests/test_tipo_item3_v5_expand.py:303`, `app/escala/tests/test_tipo_item3_v5_expand.py:305`).

Há **outro `clear()` incondicional**, no teste do estado inventado, sem fiscalização equivalente. É uma observação sobre a limpeza nova; o corpo visível contém uma única chamada esperada, e ocorrência adicional apagada nesse caminho está **[não verificado]** (`app/escala/tests/test_tipo_item3_v5_expand.py:351`, `app/escala/tests/test_tipo_item3_v5_expand.py:354`).

**4. Quem escreve, quem lê e significado**

| Valor | Quem escreve/produz | Quem lê |
|---|---|---|
| `ata.orfas`, `ata.n_orfas` | `gravar_orfas`: textos HH:MM e comprimento; alimentado pelo cartório com órfãs da grade (`app/escala/services/ata.py:355`, `:356`; `app/ponto/services/cartorio.py:772`). | `_valores_orfas`, seguido dos acessores ou do transporte cru de `LeituraOrfas` (`app/escala/services/ata.py:92`, `:140`, `:152`, `:158`; `app/escala/services/leitor_celula.py:192`). |
| `ata.orfas_instantes` | Mesmo escritor: ISO com fuso, paralelo às horas; remove a chave quando não há instantes disponíveis (`app/escala/services/ata.py:357`, `:358`, `:360`). | `_instantes_gravados` e `instantes_orfas`, que validam paralelismo e fuso (`app/escala/services/ata.py:168`, `:176`, `:180`, `:188`). |
| Envelope persistido | Porta de lavratura transporta, atribui e salva (`app/ponto/portas/celula.py:998`, `:1009`, `:1011`, `:1036`, `:1043`). | Vigia e censo consultam o envelope; não escrevem a entidade nesses caminhos (`app/ponto/services/vigia_pareador_celula.py:70`; `app/escala/services/ata.py:228`). |
| `ESTADOS_LEGADOS` | `leitura_orfas` incrementa **por leitura** (`app/escala/services/ata.py:114`). | Registro diagnóstico distinto do contador de células do censo (`app/escala/services/ata.py:38`, `:231`). |
| `CensoAta.estados` | Censo pergunta à mesma porta; conta exceções como ILEGAL e NULAS passadas pela função real (`app/escala/services/ata.py:231`, `:233`, `:234`, `:236`). | `legado`, `fora_da_forma` e publicação do comando (`app/escala/services/ata.py:212`, `:217`; `app/ponto/management/commands/tripwire_celulas.py:428`). |
| `universo`, NULAS hoje/futuro | Consultas ORM; hoje/futuro é `nulas - passado` (`app/escala/services/ata.py:237`, `:238`). | Publicação separa essas NULAS do débito (`app/ponto/management/commands/tripwire_celulas.py:425`, `:430`). |
| `hoje` | `_hoje()` na borda, recebido pelo `handle` (`app/ponto/management/commands/tripwire_celulas.py:357`, `:369`). | Censo e filtro `data__lt=hoje` (`app/ponto/management/commands/tripwire_celulas.py:423`; `app/escala/services/ata.py:200`). |
| `lampadas` | Cartório produz e incorpora à ata (`app/ponto/services/cartorio.py:737`, `:739`). | Vigia mantém leitura direta dessa chave ainda não migrada e usa posição, tipo e estado aceso (`app/ponto/services/vigia_pareador_celula.py:80`, `:103`, `:109`). |
| Registro de ilegal | `_ilegal` registra sítio e ocorrência (`app/escala/services/ata.py:48`, `:49`). | Helper, hook e runner (`app/escala/tests/test_tipo_item3_v5_expand.py:263`; `app/escala/services/ata_ci.py:62`; `app/escala/runner_ata_ci.py:11`). |

Os consumidores produtivos encontrados da porta são:

- Leitor da célula, chamados e materialização (`app/escala/services/leitor_celula.py:192`; `app/chamados/juizes.py:511`; `app/chamados/services/materializacao.py:182`).
- Juiz de batida, tripwire, cartório e cluster (`app/ponto/juiz_batida.py:163`; `app/ponto/management/commands/tripwire_celulas.py:267`; `app/ponto/services/cartorio.py:841`; `app/ponto/services/cluster_espurio.py:31`).
- Esmeril, cartão e vigia (`app/ponto/services/esmeril_espelho.py:126`; `app/relatorios/cartao_pela_celula.py:289`; `app/ponto/services/vigia_pareador_celula.py:76`).

**Não há significado temporal uniforme.** A grade combina órfãs associadas ao dia do turno com sobras acrescentadas pela data civil; o vigia procura pela data civil, enquanto o supra juiz declara dia-TURNO. Isso permanece anterior ao delta (`app/escala/utils.py:1761`, `app/escala/utils.py:1814`, `app/escala/utils.py:1823`, `app/ponto/services/vigia_pareador_celula.py:111`, `app/ponto/supra_juiz.py:510`).

HH:MM continua sem data/fuso no texto; o instante adicional preserva ambos. A configuração declara São Paulo, mas o fuso efetivo do processo de produção está **[não verificado]** (`app/escala/services/ata.py:75`, `app/escala/services/ata.py:358`, `app/config/settings/base.py:93`). O PDF ainda reúne HH:MM de toda a janela, defeito explicitamente preservado pelo teste anterior ao delta (`app/relatorios/pdf_espelho.py:477`, `app/escala/tests/test_tipo_item3_v5_expand.py:151`).

**5. Classes de reprovação**

| Classe | Resultado limitado ao delta |
|---|---|
| Segundo juiz registrado | Não identificado: censo chama a classificação existente; vigia conserva `turnos_do_colab` (`app/escala/services/ata.py:231`; `app/ponto/services/vigia_pareador_celula.py:99`; `app/core/juizes.py:208`). |
| Segundo escritor produtivo fora da porta | Não identificado: novos caminhos consultam e publicam. A escrita direta de ata nova é fixture de teste (`app/escala/services/ata.py:228`; `app/ponto/management/commands/tripwire_celulas.py:424`; `app/escala/tests/test_tipo_item3_v5_expand.py:252`). |
| Relógio fora da borda | O censo novo recebe o corte corretamente. O comando inteiro ainda chama helpers com `date.today()` próprio; são sítios anteriores ao delta (`app/ponto/management/commands/tripwire_celulas.py:423`, `:387`, `:398`, `:88`, `:250`). |
| Derivação paralela | Não identificada no censo: reutiliza `leitura_orfas` e `contar_nao_lavrada_passado` (`app/escala/services/ata.py:231`, `:234`). |
| Selo/RED sem mordida | Há asserções para distribuição, dívida e alarme, mas o selo tem as exclusões descritas no item 2; o teste do estado inventado cobre somente `horas_orfas` (`app/escala/tests/test_tipo_item3_v5_expand.py:275`, `:281`, `:309`, `:361`, `:353`). |
| Campo com dois significados | Conflito turno/civil permanece; não foi introduzido pelo delta (`app/escala/utils.py:1814`, `:1823`; `app/ponto/services/vigia_pareador_celula.py:111`; `app/ponto/supra_juiz.py:510`). |

Execução da suíte, mutantes e **DIFF por leitor na frota igual a zero**, exigido pelo aval: **[não verificado]**. Consistência concorrente das consultas separadas do censo: **[não verificado]** (`app/escala/services/ata.py:228`, `app/escala/services/ata.py:234`, `app/escala/services/ata.py:237`, `app/escala/services/ata.py:238`).

## 13/11 08:34 · sessao 5f793b4f6928 · - · - · origem=Ronald (tmux)

Considero `_orfas_da_ata` um **resíduo do bloqueante 1 original**, não uma regressão: a v1 citava expressamente esse leitor, e a cura dos três acessores não alcança sua política própria de valores crus com fallback. “Só delta” não comprova a cura de um sítio que permaneceu intacto (`app/escala/services/leitor_celula.py:192`; `app/escala/services/leitor_celula.py:193`; `app/escala/services/leitor_celula.py:194`; `app/escala/services/ata.py:143`; `app/escala/services/ata.py:155`; `app/escala/services/ata.py:161`).

Responderia separadamente: **os três acessores foram curados; o leitor citado na v1 continua sem tratamento pelo nome**. Não alegaria divergência numérica nesse leitor sem demonstração (`app/escala/services/leitor_celula.py:192`; `app/escala/services/leitor_celula.py:194`).

O `clear()` do teste de estado inventado merece **observação, não bloqueante demonstrado**: a limpeza é incondicional, mas o corpo visível contém uma única chamada esperada. Uma ocorrência adicional apagada por esse teste permanece [não verificado] (`app/escala/tests/test_tipo_item3_v5_expand.py:351`; `app/escala/tests/test_tipo_item3_v5_expand.py:353`; `app/escala/tests/test_tipo_item3_v5_expand.py:354`).

## 13/11 08:34 · sessao 6d04630be3db · - · - · origem=Ronald (tmux)

Somente leitura; execução e frota **[não verificado]**.

**Quem escreve/lê e significado**

| Valor | Escritor | Leitor e unidade |
|---|---|---|
| `ata.orfas`, `ata.n_orfas` | `gravar_orfas`: lista de textos HH:MM e comprimento (`app/escala/services/ata.py:354-356`); recebe `dia.get('orfas')` de `ata_do_dia` (`app/ponto/services/cartorio.py:772`). | `_valores_orfas` lê valores crus (`app/escala/services/ata.py:92`); `horas_orfas`, `contar_orfas`, `tem_orfas` os interpretam (`:140-163`). LEGADO aceita também valores fora desses tipos (`:95-105`). |
| `ata.orfas_instantes` | Mesmo escritor: ISO do `HoraComInstante` quando todas as horas carregam instante; senão remove (`app/escala/services/ata.py:357-360`). | `_instantes_gravados`/`instantes_orfas`: lista paralela de instantes com fuso (`app/escala/services/ata.py:168,176,180-190`). Nenhum consumidor produtivo externo encontrado no rastreio. |
| `HoraComInstante` | `hora_de`: HH:MM de `strftime` e instante aware (`app/escala/services/ata.py:70-78`); grade fornece batidas localizadas (`app/escala/utils.py:1326,1835,1837`). | `gravar_orfas` lê texto/instante (`app/escala/services/ata.py:355,358`). Fuso configurado São Paulo, `USE_TZ=True` (`app/config/settings/base.py:93,95`); fuso efetivo em produção **[não verificado]**. |
| Envelope persistido | Lavratura transporta pela porta (`app/ponto/portas/celula.py:998`), atribui (`:1009,1036`) e persiste (`:1011,1043`). | Delta não adiciona escritor de entidade: censo somente consulta (`app/escala/services/ata.py:228,237,238`); vigia consulta e monta estruturas locais (`app/ponto/services/vigia_pareador_celula.py:70-80`). |
| `ESTADOS_LEGADOS` | `leitura_orfas` incrementa por **leitura**, não por célula distinta (`app/escala/services/ata.py:114-115`). | Registro diagnóstico em memória; não equivale ao contador de frota produzido pelo censo (`app/escala/services/ata.py:227-238`). |
| `CensoAta.estados` | Censo classifica cada ata não NULL pela **mesma porta** (`app/escala/services/ata.py:228-233`); NULL passado usa função real (`:200,234-236`). | `legado`, `fora_da_forma`, publicação (`app/escala/services/ata.py:212,217`; `app/ponto/management/commands/tripwire_celulas.py:427-429`). Unidade: células. `NAO_LAVRADA` neste contador é explicitamente somente passado (`app/escala/services/ata.py:234-236`). |
| `universo`, NULL hoje/futuro | Total ORM e `nulas - passado` (`app/escala/services/ata.py:237-238`). | Publicação distingue hoje/futuro fora do débito (`app/ponto/management/commands/tripwire_celulas.py:425-430`). Consultas separadas, sem fotografia única demonstrada (`app/escala/services/ata.py:228,234,237,238`). |
| `hoje` | `_hoje` chama `datetime.date.today`, `handle` recebe uma vez (`app/ponto/management/commands/tripwire_celulas.py:357-369`). | Censo recebe esse corte (`:423`), dívida é `data__lt=hoje` (`app/escala/services/ata.py:200`). Fuso do processo em produção **[não verificado]**. |
| `lampadas` do vigia | `lampadas_do_dia` produz ordem/tipo/hora/acesa/luz (`app/ponto/services/cartorio.py:600,712-727`); `ata_do_dia` incorpora (`:737-739`). | Vigia conserva leitura fora do item 3 (`app/ponto/services/vigia_pareador_celula.py:80`), usa primeira/última do dia-TURNO e testa `tipo`/`acesa` (`:103-109`). |

Leitores produtivos da porta: leitor da célula (`app/escala/services/leitor_celula.py:192-194`), chamados (`app/chamados/juizes.py:511`), materialização (`app/chamados/services/materializacao.py:182`), juiz batida (`app/ponto/juiz_batida.py:163`), tripwire (`app/ponto/management/commands/tripwire_celulas.py:267`), cartório (`app/ponto/services/cartorio.py:841`), cluster (`app/ponto/services/cluster_espurio.py:31-37`), esmeril (`app/ponto/services/esmeril_espelho.py:126`), cartão (`app/relatorios/cartao_pela_celula.py:289`) e vigia migrado (`app/ponto/services/vigia_pareador_celula.py:76`).

**Não há significado temporal uniforme, mas o conflito é anterior ao delta.** A grade guarda órfãs do turno por dia de entrada (`app/escala/utils.py:1441,1761,1814`) e acrescenta sobras por data civil local (`:1326,1823,1835,1837`). Vigia pressupõe data civil (`app/ponto/services/vigia_pareador_celula.py:68,111-113`); supra juiz declara dia-TURNO (`app/ponto/supra_juiz.py:509-519`); `horas_reais_do_dia` interpreta órfã como hora da data da linha (`app/ponto/turnos.py:2647-2653,2667`). PDF ainda reúne HH:MM de toda janela (`app/relatorios/pdf_espelho.py:475-477`) e compara somente HH:MM (`:178`).

**Classes do item 5**

- **Segundo juiz registrado:** não identificado no delta: censo pergunta à porta (`app/escala/services/ata.py:231`), vigia continua perguntando a `turnos_do_colab` (`app/ponto/services/vigia_pareador_celula.py:99`), autoridade registrada do dia do turno (`app/core/juizes.py:208`).
- **Segundo escritor fora da porta:** não identificado nas mudanças produtivas; censo consulta e publica (`app/escala/services/ata.py:228-238`; `app/ponto/management/commands/tripwire_celulas.py:423-436`).
- **Relógio fora da borda:** censo novo recebe corte corretamente (`app/ponto/management/commands/tripwire_celulas.py:423`). Comando inteiro ainda lê relógios em helpers sem argumentos (`:387,398` → `:88,250`), anteriores ao delta; docstring “UMA vez” é excessiva (`:358`).
- **Derivação paralela:** não identificada no censo: reutiliza `leitura_orfas` e `contar_nao_lavrada_passado` (`app/escala/services/ata.py:231,234`).
- **Selo/RED sem mordida:** há asserções reais do mapa, soma, contador e alarme (`app/escala/tests/test_tipo_item3_v5_expand.py:275-281,307-309`); execução **[não verificado]**. AST morde constante literal de duas chaves (`:361,374-375`) e mutante antigo do vigia (`:388-389`), mas exclui `orfas` explicitamente (`:360-361`).
- **Campo com dois significados:** conflito turno/civil descrito acima existe nos consumidores, sem mudança temporal introduzida pelo delta (`app/escala/utils.py:1814,1823`; `app/ponto/services/vigia_pareador_celula.py:111`; `app/ponto/supra_juiz.py:510`).

Observação para o bloqueante 1: fora dos três helpers alterados, `_orfas_da_ata` ainda lê `LeituraOrfas` e faz `or []`/`or 0` sem consultar `.estado`; isso já estava assim antes deste delta (`app/escala/services/leitor_celula.py:192-194`).

## 13/11 08:34 · sessao 5aed1aef9e5c · - · - · origem=Ronald (tmux)

Pergunta 2, por inspeção da cópia:

- O vigia preserva o conjunto da base para `None`, `{}` e não-mapping falso: todos viram `{}` antes da porta (`app/ponto/services/vigia_pareador_celula.py:74-76`). Para mappings, contagem falsa produz `set()`; contagem verdadeira produz o conjunto das horas, conservando também os erros de `set()` quando o valor gravado não pode formar um conjunto (`app/ponto/services/vigia_pareador_celula.py:76`; `app/escala/services/ata.py:95-105,128-137,152-155`). Não-mapping verdadeiro muda de erro de `.get` na expressão-base fornecida para `EstadoIlegalDaAta` (`app/escala/services/ata.py:90-91`).

- Não encontrei leitura produtiva **literal crua** das três chaves da ATA fora da porta. Os consumidores passam pelo acessor: `app/chamados/juizes.py:511`; `app/chamados/services/materializacao.py:182`; `app/ponto/juiz_batida.py:163`; `app/ponto/services/esmeril_espelho.py:126`; `app/ponto/services/vigia_pareador_celula.py:76`; `app/ponto/services/cluster_espurio.py:31`; `app/ponto/services/cartorio.py:841`; `app/ponto/management/commands/tripwire_celulas.py:267`; `app/relatorios/cartao_pela_celula.py:289`; `app/escala/services/leitor_celula.py:192-194`. Acesso dinâmico arbitrário: **[não verificado]**.

- `orfas` da grade é outro objeto: materializado em `app/escala/utils.py:1761,1814,1835-1837` ou exposto pelo leitor em `app/escala/services/leitor_celula.py:394,424`. É consumido por classificação, presença, simulação, calendário e relatório (`app/escala/orfa_classe.py:58`; `app/folha/causas.py:43,100`; `app/ponto/status_dia.py:188`; `app/escala/services/furos_diarios.py:22`; `app/ponto/services/bordas_realizado.py:49`; `app/ponto/services/simular_dia.py:12`; `app/ponto/propositor.py:39`; `app/ponto/supra_juiz.py:379,514`; `app/ponto/turnos.py:2667`; `app/relatorios/pdf_espelho.py:477,552`; `app/colaboradores/services/calendario.py:380-381`; `app/ponto/management/commands/detectar_par_relampago.py:140`; `app/ponto/management/commands/propor_reorganizacao.py:104`).

- O selo morde **qualquer constante literal** `n_orfas` ou `orfas_instantes` nos arquivos examinados, incluindo escrita ou dado sem relação com a ATA: não analisa proveniência nem operação (`app/escala/tests/test_tipo_item3_v5_expand.py:361,369-375`). O mutante cobre especificamente o antigo `.get('n_orfas')` do vigia (`app/escala/tests/test_tipo_item3_v5_expand.py:382-389`).

- Deixa passar `orfas` cru, nomes de chave construídos dinamicamente e os caminhos excluídos: porta, testes, migrations, logs e media (`app/escala/tests/test_tipo_item3_v5_expand.py:360-375`). O descriptor não fecha a brecha de `values_list`: instrumenta atributos de instâncias (`app/escala/services/ata_ci.py:17-29,37`), enquanto o vigia recebe tuplas da consulta (`app/ponto/services/vigia_pareador_celula.py:70-73`).

Ressalva ao bloqueante 1: `_orfas_da_ata` continua usando fallback nos campos devolvidos por `LeituraOrfas`, sem consultar `.estado`; isso não é leitura crua das chaves, mas permanece relevante se o aval exige tratamento pelo nome em **todo leitor**, além dos três acessores agora alterados (`app/escala/services/leitor_celula.py:192-194`).

## 13/11 08:34 · sessao 5f793b4f6928 · - · - · origem=Ronald (tmux)

Bloqueante 3 corrigido no caminho dos dois testes: cada `_ilegal` adiciona o sítio e emite um `ERROR`; o helper exige exatamente uma ocorrência capturada antes de limpar o registro. Duas ocorrências no mesmo sítio produzem dois registros e falham antes do `clear` (`app/escala/services/ata.py:45`; `app/escala/services/ata.py:49`; `app/escala/tests/test_tipo_item3_v5_expand.py:263`; `app/escala/tests/test_tipo_item3_v5_expand.py:269`).

`assertLogs` isoladamente pode passar com várias ocorrências; quem exige exatamente uma é o helper chamado depois dos dois contextos. Zero ilegais, mesmo acompanhado por outro `ERROR`, reprova na contagem filtrada do helper; zero registros reprova no próprio contexto (`app/escala/tests/test_tipo_item3_v5_expand.py:263`; `app/escala/tests/test_tipo_item3_v5_expand.py:264`; `app/escala/tests/test_tipo_item3_v5_expand.py:272`; `app/escala/tests/test_tipo_item3_v5_expand.py:274`; `app/escala/tests/test_tipo_item3_v5_expand.py:303`; `app/escala/tests/test_tipo_item3_v5_expand.py:305`). Implementação local de `unittest.assertLogs` não inspecionada: [não verificado].

O helper ainda não mede ocorrências fora do contexto. Uma segunda ocorrência do mesmo sítio, após o contexto e antes do helper, colapsaria no `set` e seria apagada; os dois testes atuais chamam o helper imediatamente, sem operação intermediária que faça isso (`app/escala/services/ata.py:47`; `app/escala/services/ata.py:48`; `app/escala/tests/test_tipo_item3_v5_expand.py:272`; `app/escala/tests/test_tipo_item3_v5_expand.py:274`; `app/escala/tests/test_tipo_item3_v5_expand.py:303`; `app/escala/tests/test_tipo_item3_v5_expand.py:305`).

Há outro `clear()` incondicional no teste do estado inventado, sem a fiscalização de ocorrências/sítios desse helper. Isso é uma limpeza mais ampla que a expectativa declarada, embora o corpo visível faça uma única chamada esperada (`app/escala/tests/test_tipo_item3_v5_expand.py:348`; `app/escala/tests/test_tipo_item3_v5_expand.py:351`; `app/escala/tests/test_tipo_item3_v5_expand.py:354`).

No censo novo não aparece juiz duplicado: ele obtém o estado por `leitura_orfas`; captura a exceção para contar ILEGAL e mantém os efeitos de `_ilegal`. Também não aparece escritor produtivo adicional de ata nesse caminho: são consultas e agregação em memória (`app/escala/services/ata.py:228`; `app/escala/services/ata.py:231`; `app/escala/services/ata.py:232`; `app/escala/services/ata.py:238`; `app/escala/services/ata.py:48`).

O novo censo recebe `hoje` da borda e o repassa à consulta de passado. A declaração “relógio deste comando [...] UMA vez” continua falsa para o comando inteiro: dois helpers chamados sem datas executam seus próprios `date.today()`. Esses sítios já existiam segundo o delta fornecido (`app/ponto/management/commands/tripwire_celulas.py:357`; `app/ponto/management/commands/tripwire_celulas.py:369`; `app/ponto/management/commands/tripwire_celulas.py:387`; `app/ponto/management/commands/tripwire_celulas.py:398`; `app/ponto/management/commands/tripwire_celulas.py:88`; `app/ponto/management/commands/tripwire_celulas.py:250`; `app/escala/services/ata.py:200`; `app/ponto/management/commands/tripwire_celulas.py:423`).

As asserções do RED cobram distribuição, total de legado, débito e publicação do alarme; há mordida estática para os três mutantes descritos na volta 1. Execução dos testes e mutantes: [não verificado] (`app/escala/tests/test_tipo_item3_v5_expand.py:275`; `app/escala/tests/test_tipo_item3_v5_expand.py:280`; `app/escala/tests/test_tipo_item3_v5_expand.py:281`; `app/escala/tests/test_tipo_item3_v5_expand.py:307`; `app/escala/tests/test_tipo_item3_v5_expand.py:309`).

O censo não forma fotografia única: percorre atas gravadas e depois consulta NULAS passadas, total de NULAS e universo separadamente. Consistência concorrente e resultados reais de produção: [não verificado] (`app/escala/services/ata.py:228`; `app/escala/services/ata.py:234`; `app/escala/services/ata.py:237`; `app/escala/services/ata.py:238`).

## 13/11 08:29 · sessao e789de9ed990 · O168 · verificador · origem=Code

**REPROVA. Bloqueantes:**

1. **A migração ao juiz único permanece incompleta:** cartório e guarda anterior da retratação conservam critérios divergentes de `respostas_sem_veredito`. (`app/ponto/services/cartorio.py:1641`; `app/chamados/reconciliador.py:519`; `app/chamados/juizes.py:2028`; `app/chamados/models.py:1872`.)
2. **A nota não cobre todo o universo da recusa:** CSF pode reter pelo destino, mas a nota enumera somente perguntas da origem. (`diff app/ponto/supra_juiz.py:566`; `diff app/chamados/models.py:728`.)
3. **A catraca corrigida pode entrar em laço infinito:** duas atribuições de portas diferentes ao mesmo apelido fazem `mudou` permanecer verdadeiro em todas as passadas. (`diff app/chamados/tests/test_o168_fechadores_fora_do_gate.py:108`; `diff app/chamados/tests/test_o168_fechadores_fora_do_gate.py:114`.)

Abaixo, referências sem prefixo são da cópia; `diff` usa a numeração nova. Suite ORM, custos empíricos pós-diff e ocorrência dos cenários em produção: **[não verificado]**. [nao verificado]

**A. Predicado depois do diff**

Na **origem**, CSF recusa se qualquer disputa relacionada devolver `respostas_sem_veredito()` não vazia. Não exige disputa aberta. No **destino**, recusa se aquela pergunta estiver na lista sem veredito da sua disputa; outra pergunta pendente da mesma disputa, sozinha, não basta. (`diff app/ponto/supra_juiz.py:563`; `diff app/ponto/supra_juiz.py:566`.)

Não sobra leitura direta de `respondida_em` ou `validada_em` nos corpos de CSF, `_gate_conversa_do_pai` ou `_decisao_do_fechamento`. O gate chama a composição inteira, apesar da docstring afirmar que a metade destino não se aplica ali. (`diff app/ponto/supra_juiz.py:563`; `app/chamados/models.py:664`; `app/chamados/models.py:704`; `app/chamados/models.py:716`.)

“Sem veredito” significa exatamente:

```text
validada_em é nulo
E materializada_em é nulo
E (respondida_em não é nulo OU resposta_colab.strip() não é vazio)
E (ignorar_tranca OU dia não trancado)
```

Portanto, **carimbo sem texto conta; texto sem carimbo conta**. Validada, materializada ou trancada não conta na chamada padrão. “Julgada” ou “descartada” não são condições independentes: só alteram esse resultado quando refletidas nas condições acima. O predicado não consulta diretamente C6, sequência de decisões ou lavra. (`app/chamados/models.py:1856`; `app/chamados/models.py:1872`.)

É a mesma chamada padrão usada no desligamento, no reconciliador indicado e em `fechar()`. (`app/colaboradores/services/desligamento.py:141`; `app/chamados/reconciliador.py:1164`; `app/chamados/models.py:1916`.)

**B. Retratação e caminhos alternativos**

O caminho chega ao juiz A:

`retratar_premissa_morta_colab` → `_retratar` → `transicionar_com_pauta(ato=ch.reconciliar)` → `reconciliar` → `_decisao_do_fechamento` → gate → CSF → `respostas_sem_veredito`. (`app/chamados/reconciliador.py:1465`; `app/chamados/reconciliador.py:524`; `app/chamados/services/fecho_runtime.py:165`; `app/chamados/models.py:652`; `app/chamados/models.py:686`; `app/chamados/models.py:716`; `diff app/ponto/supra_juiz.py:563`.)

**Mas nem toda retratação chega lá.** Antes, `_retratar` consulta `retencao_da_retratacao`, que chama `irmas_respondidas_sem_consumo`. Esse predicado exige **carimbo E texto**, exclui validada/materializada, não exclui tranca e usa o universo de crédito destino/pai. Uma resposta trancada pode ser retida aqui embora A a exclua. (`app/chamados/reconciliador.py:519`; `app/chamados/reconciliador.py:447`; `app/chamados/juizes.py:2028`; `app/chamados/models.py:1876`.)

Existe também o caminho do cartório: `_tem_resposta_do_colab` preserva pelo **texto**, antes da reconciliação sistêmica. Resposta validada/materializada com texto continua preservada nesses ramos, enquanto CSF a libera. O achado (3) anterior, portanto, **não foi inteiramente curado**. (`app/ponto/services/cartorio.py:1637`; `app/ponto/services/cartorio.py:1648`; `app/ponto/services/cartorio.py:1665`; `app/chamados/models.py:1873`.)

**C. Itens (1) e (2) anteriores**

**(1): curado para a retenção por origem, incompleto para o gate inteiro.** Com o mesmo conjunto de perguntas de origem, a nota usa PKs ordenados como `objeto_ref`; a porta calcula `evento_chave`, e a unicidade recupera a nota existente na repetição. A recusa deduplica pelo último payload e por transições posteriores. Assim, duas passadas consecutivas do mesmo ato, sem mudança intermediária e com gravações bem-sucedidas, deixam **uma recusa e uma nota**. A auditoria continua best-effort. (`diff app/chamados/models.py:728`; `diff app/chamados/models.py:735`; `app/chamados/services/nota.py:100`; `app/chamados/services/nota.py:108`; `app/chamados/models.py:403`; `app/core/observ.py:98`.)

Se a retenção vier **somente de `perguntas_que_resolvem`**, `_pend` fica vazio e não há nota. O ensaio isolado desse ramo produziu recusa sem nota; sua ocorrência em produção é **[não verificado]**. (`diff app/ponto/supra_juiz.py:566`; `diff app/chamados/models.py:728`; `diff app/chamados/models.py:729`.)

Com `registrar=False`, o trecho novo não grava recusa nem nota; o dry-run usa essa modalidade. (`app/chamados/models.py:662`; `diff app/chamados/models.py:718`.)

A nota permanece depois do fechamento como histórico: a porta cria ou recupera, e a timeline continua lendo os followups. A instrução “feche este chamado pela ação humana” pode ficar superada pela própria cascata autorizada em (B). Além disso, “a disputa fechou” não é precondição do gate, portanto pode ser falsa já na emissão por uma reconciliação direta. (`app/chamados/services/nota.py:103`; `app/chamados/views.py:372`; `diff app/chamados/models.py:732`; `diff app/chamados/models.py:734`; `app/chamados/models.py:713`.)

**(2): o escape original foi curado**, incluindo `t = porta`, cadeia `u = t`, atribuição anotada e `getattr` literal. Porém, o ponto fixo introduziu o bloqueante novo: isto oscila indefinidamente, inclusive quando as atribuições estão em funções distintas, porque o mapa é global:

```python
t = transicionar_com_pauta
t = encerrar_com_retencao
```

Cada passada troca `t` duas vezes e termina com `mudou=True`. (`diff app/chamados/tests/test_o168_fechadores_fora_do_gate.py:93`; `diff app/chamados/tests/test_o168_fechadores_fora_do_gate.py:101`; `diff app/chamados/tests/test_o168_fechadores_fora_do_gate.py:108`.)

**D. Quem ainda pode ser segurado**

A própria disputa que fecha normalmente não pode conservar resposta pendente segundo A: `fechar()` a recusa antes de gravar o fechamento. O gate ainda pode encontrar pendência em **outra disputa do mesmo pai**, ou em pergunta que resolve esse chamado. Sua entrada exige somente PK e categoria `disputa_supervisao`; CSF não restringe as relações à disputa que disparou S88. (`app/chamados/models.py:1916`; `app/chamados/models.py:713`; `diff app/ponto/supra_juiz.py:563`; `diff app/ponto/supra_juiz.py:566`.)

O teste cria D2 diretamente enquanto D1 está aberta. Os três criadores usuais reutilizam a disputa aberta do colaborador antes de criar outra; não reproduzem esse gesto diretamente. (`diff app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:94`; `app/chamados/services/disputa_emissao.py:531`; `app/chamados/services/disputa_emissao.py:1096`; `app/chamados/services/disputa_emissao.py:1332`.)

Entretanto, **o estado é alcançável por histórico**:

1. D1 fecha e S88 resolve o pai. (`app/chamados/models.py:1966`; `app/chamados/signals.py:43`.)
2. Gestor muda o pai resolvido para `em_analise`, sem reabrir D1. (`app/chamados/services/acoes_chamado.py:319`; `app/chamados/catalogo/motor.py:102`.)
3. Sem disputa aberta, o emissor reutiliza esse pai vivo e cria D2. (`app/chamados/services/disputa_emissao.py:568`; `app/chamados/services/disputa_emissao.py:587`; `app/chamados/models.py:936`.)
4. A ação administrativa reabre D1, sem conferir D2, e cria nova pergunta; uma resposta nela pode deixar a pendência que segura o pai quando D2 fecha. (`app/chamados/services/acoes_disputa.py:255`; `app/chamados/services/acoes_disputa.py:263`; `app/chamados/models.py:2168`; `app/chamados/services/disputa_emissao.py:1468`.)

Logo, a fixture **monta diretamente um estado alcançável**, mas não testa o caminho de criação. Ocorrência em produção: **[não verificado]**.

**E. Contador, registro e corte**

`pais_segurados_pela_conversa` **lê a trilha; não rejulga**. Conta chamados atualmente vivos com **qualquer recusa histórica** daquele motivo. Não exige recusa no ciclo atual, categoria atual ou resposta ainda pendente. Um pai reaberto pode voltar à contagem pela recusa antiga. (`app/chamados/juizes.py:2519`; `app/chamados/juizes.py:2522`.)

Isso corresponde a “vivos com histórico de retenção”, não necessariamente à frota atualmente segurada por resposta sem veredito. O delta mantém o motivo da trilha, sem distinguir as recusas da regra anterior das de (B). (`diff app/chamados/models.py:720`; `app/chamados/juizes.py:2520`; `diff app/docs/JUIZES.md:92`.)

A entrada nova em `core/juizes.py` declara **predicado já existente**, com leitores anteriores; não cria seu corpo nem uma segunda implementação. Entretanto, a composição CSF muda de critério, e o registro não elimina os predicados paralelos de B. A frase antiga “a função não muda / mesmo predicado e universo” não descreve este delta. (`diff app/core/juizes.py:294`; `app/chamados/models.py:1856`; `app/colaboradores/services/desligamento.py:141`; `diff app/docs/CORTES.md:449`; `app/docs/CORTES.md:448`.)

**F. Órfãs e custo**

Liberar o pai é coerente com (B): esse teste valida a resposta antes do consumo, e A exclui validadas. Mas o diff **não demonstra “pai fechado”**: remove a asserção de pai vivo sem acrescentar uma de pai fechado. Cobra `falhas == 2`, evento pendente e uma tentativa. Identidade com o main e resultado executado: **[não verificado]**. (`app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:149`; `app/chamados/models.py:1873`; `diff app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:157`.)

“3 por PAI” só descreve o caminho mínimo de uma disputa, nenhuma resposta elegível à consulta de tranca e nenhum destino. Agora há consulta de tranca para respostas elegíveis, releitura da disputa por pergunta de destino e nova varredura da origem para a nota. Não é custo universal constante por pai. Os tetos 220/280 pós-diff permanecem **[não verificado]**. (`app/ponto/tests/test_selo_chokepoint_escrita.py:196`; `app/ponto/tests/test_selo_chokepoint_escrita.py:210`; `app/chamados/models.py:1876`; `app/chamados/juizes.py:991`; `diff app/ponto/supra_juiz.py:566`; `diff app/chamados/models.py:728`.)

**G. Mordida dos testes** [nao verificado]

`_le_respondida_cru` detecta somente `ast.Attribute` chamado `respondida_em`. Trocar o nome-base ou seu apelido não esconde `q.respondida_em`; **`getattr(q, 'respondida_em')` escapa**, assim como delegar a leitura a helper importado sob apelido. A verificação textual de `respostas_sem_veredito` também pode ser satisfeita por comentário. (`diff app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:221`; `diff app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:239`.)

O mutante clássico tem mordida funcional: volta a devolver `False` para a fixture validada e contraria a asserção do pai; uma regressão só no destino contraria a asserção correspondente. A leitura direta também reprovaria o teste AST da fonte real. O teste do mutante sintético apenas confirma o detector; não executa a suite contra a função substituída. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:56`; `diff app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:112`; `diff app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:210`; `diff app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:238`; `diff app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:242`.)

**H. Escritores, leitores e semântica**

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `respondida_em` | Resposta, automática, recuperação pela trilha e simetria; reabertura/ressuscitação limpam. (`app/chamados/services/disputa_emissao.py:1468`; `app/chamados/services/disputa_emissao.py:1653`; `app/chamados/services/disputa_emissao.py:1674`; `app/chamados/models.py:3162`; `app/chamados/services/disputa_emissao.py:1593`; `app/chamados/models.py:1596`.) | A aceita carimbo **OU** texto; C6 usa carimbo **E** texto naquele degrau; apresentação de estado usa carimbo. (`app/chamados/models.py:1874`; `app/chamados/juizes.py:1417`; `app/chamados/models.py:3075`.) |
| `validada_em` | Decisão de validar e simetria; nova resposta, devolução e reabertura limpam. (`app/chamados/services/validacao.py:134`; `app/chamados/models.py:3165`; `app/chamados/services/disputa_emissao.py:1480`; `app/chamados/services/validacao.py:199`; `app/chamados/services/disputa_emissao.py:1593`.) | A exclui; C6 encerra nesse degrau; validação evita repetição. (`app/chamados/models.py:1873`; `app/chamados/juizes.py:1401`; `app/chamados/services/validacao.py:396`.) |
| `status_local` | Nascimento e núcleo `_gravar_estado`; simulação altera/restaura memória. (`app/chamados/models.py:1125`; `app/chamados/models.py:228`; `app/chamados/models.py:296`; `app/chamados/models.py:356`; `app/chamados/models.py:371`.) | Motor, payload da recusa e contador; conjuntos vêm do catálogo. (`app/chamados/models.py:674`; `diff app/chamados/models.py:720`; `app/chamados/juizes.py:2522`; `app/chamados/catalogo/motor.py:24`.) |
| Trilha de recusa | Gate → `_registrar_recusa` → `evento` → `LogAuditoria`. (`diff app/chamados/models.py:719`; `app/chamados/models.py:408`; `app/core/observ.py:134`.) | Deduplicador lê última recusa/transições; contador lê qualquer ocorrência histórica. (`app/chamados/models.py:403`; `app/chamados/juizes.py:2519`.) |
| Nota `retido_pela_conversa` | Gate chama a porta única de nota. (`diff app/chamados/models.py:731`; `app/chamados/services/nota.py:107`.) | Timeline lê os followups e apresenta o conteúdo. (`app/chamados/views.py:372`; `app/templates/chamados/detalhe_local.html:112`.) |
| `objeto_ref` | Gate monta a string de PKs ordenados. (`diff app/chamados/models.py:728`; `diff app/chamados/models.py:735`.) | Porta incorpora ao hash com chamado, tipo e autor. Não persiste separadamente: persiste `evento_chave`. (`app/chamados/services/nota.py:64`; `app/chamados/services/nota.py:88`; `app/chamados/services/nota.py:100`.) |

**Não há significado universal “validada = humano julgou”.** O contrato distingue explicitamente o ato de validar dos carimbos de simetria. Essa distinção preexiste ao delta; a divergência material permanece nos critérios dos leitores apontados em B. (`app/chamados/services/validacao.py:19`; `app/chamados/models.py:3160`; `app/ponto/services/cartorio.py:1641`.)

**I. Classes de reprovação**

Presentes: **derivação paralela divergente**, **leitores com critérios diferentes para os mesmos campos** e **catracas com cobertura limitada**, além do novo risco de não terminação. (`app/ponto/services/cartorio.py:1641`; `app/chamados/juizes.py:2030`; `app/chamados/models.py:1874`; `diff app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:221`; `diff app/chamados/tests/test_o168_fechadores_fora_do_gate.py:108`.)

O delta usa as portas existentes para auditoria e nota; não acrescenta escrita direta de status, leitura explícita de relógio ou aquisição explícita de trava nesses trechos. Concorrência executada e ordem de pouso/pré-requisitos: **[não verificado]**. (`diff app/chamados/models.py:719`; `diff app/chamados/models.py:731`; `diff app/ponto/supra_juiz.py:563`.)

## 13/11 08:28 · sessao e180953de3ac · TIPO-NA-FONTE · verificador · origem=Code

**REPROVA.** O bloqueante principal é demonstrável: o estado LEGADO recebe nome e contador, mas os leitores continuam usando fallback sem consultar esse nome, inclusive `None → 0`. Há também leitura crua fora da porta e uma brecha no helper que limpa ilegais esperados (`app/escala/services/ata.py:106,119,130,136`; `app/ponto/services/vigia_pareador_celula.py:74`; **diff** `app/escala/tests/test_tipo_item3_v5_expand.py:259,262`).

Referências abaixo usam a numeração HEAD da cópia; **diff** indica a numeração nova fornecida, incluindo o delta. Execução da suíte, mutantes e medições da frota: **[não verificado]**. [nao verificado]

**A. Preservação dos leitores no EXPAND**

Para listas HH:MM e contagens inteiras, os hunks preservam as expressões antigas. Isso demonstra equivalência estática nesses caminhos; o DIFF por leitor na frota exigido pelo aval permanece **[não verificado]** (`app/escala/services/ata.py:119,130,136`; `app/escala/services/leitor_celula.py:193,194`).

Na tabela, `L` é a lista gravada e `n` a contagem; as demais condições do leitor continuam valendo.

| Leitor | Ata NULA | LAVRADA HH:MM completa | Sem ambas as chaves | `n != len(L)` |
|---|---|---|---|---|
| `_orfas_da_ata`, pelo leitor público | `[]` | Textos não vazios de `L` | `[]` | Usa os textos; se nenhum, `['?'] * int(n or 0)` (`app/escala/services/leitor_celula.py:193,194,383,424`). |
| `_celula_furada` | `None` pela ausência de `lampadas` | Predicado lista OU contagem, depois das guardas | Sem contribuição das órfãs | Continua lista OU contagem; não exige igualdade (`app/chamados/juizes.py:494,506,509,511,517`). |
| Materialização do plantio | Nenhuma marca órfã | Acrescenta HH:MM válidos | Nenhuma marca órfã | Usa somente `L`; ignora `n` (`app/chamados/services/materializacao.py:173,176,178,182`). |
| `periodos_do_dia` | `orfas=[]` | Strings de `L` | `[]` | Usa somente `L` (`app/ponto/juiz_batida.py:158,163`). |
| Tripwire de sobra | Ignora a célula | Entrega `L` à peneira | Entrega `None` | Entrega `L`; ignora `n` (`app/ponto/management/commands/tripwire_celulas.py:260,263,266`). |
| `insumos_da_ata`, ramo sem lâmpadas | `(False, False)` | `n` verdadeiro dá `(None, None)`; senão `(False, False)` | `(False, False)` | Decide somente por `n` (`app/ponto/services/cartorio.py:834,836,841`). |
| Cluster espúrio | `None`, salvo dia sem trabalho | Continua medindo HH:MM | `None` | Mede pela lista; ignora a divergência da contagem (`app/ponto/services/cluster_espurio.py:25,31,32,34,36`). |
| Esmeril | Nenhuma órfã | Mesmos buckets HH:MM | Nenhuma órfã | Usa somente `L` (`app/ponto/services/esmeril_espelho.py:126,127,129`). |
| Cartão pela célula | Conjunto vazio | Conjunto de strings por data | Conjunto vazio | Usa somente `L` (`app/relatorios/cartao_pela_celula.py:288,289`). |

Com **somente contagem**, `_orfas_da_ata` conserva as interrogações, `tem_orfas` acusa e o cluster fica não medido; com **somente lista**, os leitores da lista continuam lendo-a, enquanto `contar_orfas` devolve zero (`app/escala/services/leitor_celula.py:193,194`; `app/escala/services/ata.py:119,125,130,136`).

Existe uma diferença de API isolada: `_orfas_da_ata(None)` passa da antiga expressão `.get`, que levantava erro, para `[]`. O leitor público já transforma ata NULA em `{}`, portanto essa diferença não altera aquele caminho público (`diff.txt`, hunk de `app/escala/services/leitor_celula.py:189`; HEAD `app/escala/services/leitor_celula.py:383,424`).

`HoraComInstante` mantém o texto HH:MM e herda igualdade, hash e ordenação de `str`; os consumidores continuam perguntando pelo texto. A preservação dessas operações tem testes declarados, mas sua execução está **[não verificado]** (`app/escala/services/ata.py:51,57,60,73`; `app/escala/tests/test_tipo_item3_v5_expand.py:45,50,55`).

`instantes_orfas` é uma API nova: responde `None` para NAO_LAVRADA/LEGADO; na ata completa antiga, responde `None` havendo órfãs sem a chave nova, ou `[]` quando a contagem é zero. Não há retorno dessa API na base para comparar (`app/escala/services/ata.py:144,147,149,152`).

**B. LEGADO e autoridade do censo**

Os quatro nomes `LEGADO_*` existem; o delta os reúne em `ESTADOS_DE_LEGADO`. `ESTADOS_LEGADOS` é outro objeto: um `Counter` incrementado por **leitura**, não um censo de células distintas (`app/escala/services/ata.py:28,36,112,113`; **diff** `app/escala/services/ata.py:33`).

O requisito de os leitores tratarem LEGADO **pelo nome não foi cumprido**: `horas_orfas`, `contar_orfas`, `tem_orfas` e `_orfas_da_ata` ignoram `.estado` e seguem com `or []`, `or 0` ou coerção booleana. O leitor novo de instantes consulta o estado explicitamente (`app/escala/services/ata.py:119,130,136,147`; `app/escala/services/leitor_celula.py:192,193,194`).

Para atas não NULAS, o censo pergunta à **mesma porta**, `leitura_orfas`, e usa o estado devolvido; não duplica `_estado`. O débito de NULAS passadas vem de consulta separada, pela função `contar_nao_lavrada_passado` (**diff** `app/escala/services/ata.py:203,206,209`; HEAD `app/escala/services/ata.py:173`).

**C. Contador publicado, universo e relógio** [nao verificado]

O universo é **toda `CelulaDia` no contexto corrente do banco**: atas não NULAS de qualquer data, sem filtro de empresa, situação ou origem; NULAS passadas entram no débito, NULAS de hoje/futuro ficam fora. Abrangência efetiva de tenants em produção: **[não verificado]** (**diff** `app/escala/services/ata.py:203,209,212,213`).

O **censo novo** recebe uma única data da borda: `_hoje()` usa `datetime.date.today()` e `handle` passa o resultado ao censo. A configuração declara `America/Sao_Paulo`; o fuso efetivo do processo que executa `date.today()` em produção permanece **[não verificado]** (**diff** `app/ponto/management/commands/tripwire_celulas.py:357,359,369,423`; HEAD `app/config/settings/base.py:93`).

A afirmação “relógio deste comando lido UMA vez” não vale para o **comando inteiro**: ele chama dois helpers sem datas, e ambos voltam a executar `date.today()`. Esses sítios já existiam na base (`app/ponto/management/commands/tripwire_celulas.py:87,249,381,392`; **diff** `app/ponto/management/commands/tripwire_celulas.py:357`).

Também não existe uma fotografia única do universo: atas gravadas, NULAS passadas, total de NULAS e total de células são consultas separadas. Consistência dessas consultas sob concorrência em produção: **[não verificado]** (**diff** `app/escala/services/ata.py:203,209,212,213`).

A fórmula publicada é `legado + ilegal + nao_lavrada_passado`; a saída identifica CONTRACT como pouso próprio. É o contador proposto para a saída medida, mas **nenhum resultado real `contador = 0` foi fornecido [não verificado]** (**diff** `app/escala/services/ata.py:192`; `app/ponto/management/commands/tripwire_celulas.py:425,436`).

O cron está registrado como **vigia**. O censo consulta e o comando imprime alarmes, sem escrita de entidade nessas operações; a porta, entretanto, escreve nos registros em memória e em log (`app/config/crons.py:2171`; **diff** `app/escala/services/ata.py:48,49,115,203,213`; `app/ponto/management/commands/tripwire_celulas.py:424,432`).

**D. Ilegal em produção e limpeza da fixture**

`_ilegal` funciona também em produção: adiciona `(arquivo, linha, motivo)` a `ESTADOS_ILEGAIS`, executa `logging.error` e levanta `EstadoIlegalDaAta`. Não há guarda de CI nessa função (`app/escala/services/ata.py:43,45,46,47,48`).

LEGADO somente incrementa `ESTADOS_LEGADOS`; não chama `_ilegal`. Ata não mapping chama `_ilegal`; o censo captura a exceção e conta ILEGAL, mas deixa o sítio registrado (`app/escala/services/ata.py:89,112,113`; **diff** `app/escala/services/ata.py:206,207,208`).

Esse registro provoca falha no hook ou runner quando o censo roda no mesmo processo da suíte. O registro é global ao módulo; compartilhamento com uma execução de produção separada não é demonstrado (`app/escala/services/ata_ci.py:62,65,68`; `app/escala/runner_ata_ci.py:11,16`; **[não verificado]** quanto à execução).

**O helper pode esconder outro ilegal real do mesmo sítio.** Todas as atas não mapping recusadas por `_valores_orfas` convergem para a mesma tupla; o `set` elimina duplicatas. O helper verifica um sítio esperado e depois limpa tudo: detecta outro sítio, mas não distingue a fixture de outra ocorrência no sítio esperado (`app/escala/services/ata.py:45,46,89`; **diff** `app/escala/tests/test_tipo_item3_v5_expand.py:259,260,261,262`).

**E. Mordida dos três mutantes**

Por inspeção das asserções; execução dos mutantes **[não verificado]**:

| Mutante | Teste que deve ficar vermelho | [nao verificado]
|---|---|
| Conta LEGADO como LAVRADA | `test_RED_censo_nomeia_cada_estado_e_conta_o_debito`, pelo mapa e totais; também o teste do comando, que exige `ata_fora_da_forma=7` (**diff** `app/escala/tests/test_tipo_item3_v5_expand.py:264,267,272,298`). |
| Ignora `n_orfas != len(orfas)` | Os mesmos: a fixture `orfas=[] / n_orfas=2` precisa entrar em `LEGADO_DESCASADA` (**diff** `app/escala/tests/test_tipo_item3_v5_expand.py:243,268,298`). |
| Retira `ATA-FORA-DA-FORMA>0` | `test_o_tripwire_publica_a_linha_e_acusa`, pela asserção literal do alarme (**diff** `app/escala/tests/test_tipo_item3_v5_expand.py:282,300`). |

`test_MORDE_frota_na_forma_da_zero` não cobre esses três defeitos: antes de medir, elimina todas as células exceto uma lavrada válida (**diff** `app/escala/tests/test_tipo_item3_v5_expand.py:275,277,279`).

**F. Escritores, leitores e significado**

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `ata['orfas']`, `ata['n_orfas']` | `gravar_orfas`: strings e `len`; chamado por `ata_do_dia`, com persistência pela porta de lavratura (`app/escala/services/ata.py:289,290,291`; `app/ponto/services/cartorio.py:772`; `app/ponto/portas/celula.py:998,1003,1036,1043`). | `_valores_orfas`, os consumidores da tabela A e o vigia ainda cru (`app/escala/services/ata.py:90`; `app/ponto/services/vigia_pareador_celula.py:74`). |
| `ata['orfas_instantes']` | `gravar_orfas` escreve ISO com fuso quando todas as horas carregam instante; caso contrário remove a chave (`app/escala/services/ata.py:292,293,294,295`). | `_instantes_gravados` e `instantes_orfas`; os usos explícitos apresentados são testes (`app/escala/services/ata.py:139,141,149,158`; `app/escala/tests/test_tipo_item3_v5_expand.py:79,85,92`). |
| `HoraComInstante` | `hora_de`, alimentado por instantes localizados nos builders; nasce nas horas de células e órfãs (`app/escala/services/ata.py:61,68,73,76`; `app/escala/utils.py:329,624,665,686,802,1326,1835,1837`). | `.instante` na gravação e transporte por pickle; consumidores antigos continuam usando texto (`app/escala/services/ata.py:65,293`; `app/ponto/propositor.py:26,31,55`; `app/relatorios/pdf_espelho.py:178,477`). |
| Estado de `CensoAta` | `_estado → leitura_orfas`; o censo agrega por célula e acrescenta NULAS passadas/ILEGAL (**diff** `app/escala/services/ata.py:95,112,206,208,210`). | Propriedades `legado`, `fora_da_forma` e publicação do comando (**diff** `app/escala/services/ata.py:186,192`; `app/ponto/management/commands/tripwire_celulas.py:425,429,431`). |

Outros consumidores do valor órfã na grade incluem classificação, presença, contagem, calendário e simulação: `classificar_orfas`, `classificar_causa`, `fatos_do_status`, `_furos_do_dia`, `_teve_batida_na_ata`, `dia_perfeito`, `_score_geometria`, reorganização e calendário (`app/escala/orfa_classe.py:58,74`; `app/folha/causas.py:43,100`; `app/ponto/status_dia.py:188`; `app/escala/services/furos_diarios.py:22`; `app/ponto/services/bordas_realizado.py:49`; `app/ponto/services/simular_dia.py:12`; `app/ponto/management/commands/detectar_par_relampago.py:140`; `app/ponto/management/commands/propor_reorganizacao.py:104`; `app/colaboradores/services/calendario.py:380,381`).

**Não supõem todos a mesma unidade temporal.** A grade guarda órfãs do turno por dia de entrada e também acrescenta sobras pela data civil; o vigia procura órfã pela data civil, enquanto o supra juiz declara dia-TURNO (`app/escala/utils.py:1441,1761,1814,1823,1837`; `app/ponto/services/vigia_pareador_celula.py:67,109`; `app/ponto/supra_juiz.py:510`).

`horas_reais_do_dia` trata a órfã como HH:MM da data da linha; o PDF chega a unir HH:MM de toda a janela. A chave nova preserva o instante, mas esses leitores não o usam (`app/ponto/turnos.py:2649,2667`; `app/relatorios/pdf_espelho.py:178,477`; `app/escala/services/ata.py:293`).

**G. Proibidos**

- **Fallback e `None → 0`: presentes**, inclusive dentro do acessor e de leitor migrado; a classificação prévia não substitui o tratamento pelo nome (`app/escala/services/ata.py:90,119,130`; `app/escala/services/leitor_celula.py:194`).
- **Formato antigo e leitor migrados juntos para ISO: não demonstrado.** As chaves antigas continuam HH:MM e contagem; ISO entra em chave adicional, sem migração dos consumidores da tabela A (`app/escala/services/ata.py:290,291,293`; `app/ponto/services/cluster_espurio.py:34,37`).
- **(B) estrito agora: não identificado.** LEGADO continua devolvendo os valores crus e não é convertido em exceção (`app/escala/services/ata.py:110,112,114`).

**H. Classes de reprovação**

Há **cobertura incompleta da porta/catraca**: o vigia lê por `values_list` e acessa as chaves diretamente; o descriptor instrumenta atributos de instâncias, não esse resultado da consulta (`app/ponto/services/vigia_pareador_celula.py:69,71,74`; `app/escala/services/ata_ci.py:17,26,37`).

Há **relógio fora da borda** e **campo interpretado em contextos de dia distintos**, já apontados em C/F. O defeito do PDF é explicitamente preservado pelo teste novo; portanto não constitui regressão demonstrada desta volta (`app/ponto/management/commands/tripwire_celulas.py:87,249`; `app/escala/tests/test_tipo_item3_v5_expand.py:151,159`).

Não se demonstra segundo juiz de classificação no censo: ele chama a porta. O caminho visível de escrita continua passando por `gravar_orfas` e pela lavratura. Alteração de ordem de trava ou abertura antecipada do pouso A das 12 chaves: **[não verificado]** (**diff** `app/escala/services/ata.py:206`; HEAD `app/ponto/services/cartorio.py:772`; `app/ponto/portas/celula.py:1043`).

Os **bloqueantes para aprovação** são:

1. Leitores de LEGADO sem tratamento pelo nome, mantendo os fallbacks proibidos (`app/escala/services/ata.py:119,130,136`; `app/escala/services/leitor_celula.py:194`).
2. Leitor produtivo das mesmas chaves ainda fora da porta e da instrumentação aplicada às instâncias (`app/ponto/services/vigia_pareador_celula.py:71,74`; `app/escala/services/ata_ci.py:17,26`).
3. Helper que pode apagar um ilegal adicional no mesmo sítio da fixture (**diff** `app/escala/tests/test_tipo_item3_v5_expand.py:259,262`; HEAD `app/escala/services/ata.py:45,46`).
4. Pré-requisito de DIFF por leitor na frota igual a zero antes do opus: **[não verificado]**.

## 13/11 08:27 · sessao 05475af9aa09 · - · - · origem=Ronald (tmux)

Achados da exploração, somente leitura:

- **Item (2) foi curado para o mutante original** `t = transicionar_com_pauta`, para cadeia `u=t`, atribuição anotada e `getattr(..., 'porta')` literal. O novo teste cobre esses casos. (`diff:app/chamados/tests/test_o168_fechadores_fora_do_gate.py:78`; `diff:app/chamados/tests/test_o168_fechadores_fora_do_gate.py:101`; `diff:app/chamados/tests/test_o168_fechadores_fora_do_gate.py:232`.) [nao verificado]

- **Defeito novo demonstrável da catraca:** o ponto fixo pode nunca terminar. Fonte válida:
  ```python
  from a import transicionar_com_pauta, encerrar_com_retencao
  t = transicionar_com_pauta
  t = encerrar_com_retencao
  ```
  Cada volta muda `t` duas vezes e deixa `mudou=True`. Confirmei quatro iterações em memória; não executei o laço ilimitado. O mapa também é global ao módulo: atribuições a `t` em funções diferentes produzem o mesmo conflito, pois não existe resolução de escopo. Ocorrência desse conflito na cópia atual: [não verificado]. (`diff:app/chamados/tests/test_o168_fechadores_fora_do_gate.py:93`; `diff:app/chamados/tests/test_o168_fechadores_fora_do_gate.py:102`; `diff:app/chamados/tests/test_o168_fechadores_fora_do_gate.py:108`; `diff:app/chamados/tests/test_o168_fechadores_fora_do_gate.py:114`.) [nao verificado]

- **G — detector AST limitado:** detecta `q.respondida_em` independentemente do nome-base ou do apelido usado para importar esse objeto. Não acompanha função leitora importada sob apelido, nem detecta `getattr(q, 'respondida_em')`. Confirmei em memória: atributos direto/apelidado → `True`; `getattr` → `False`. O `assertIn('respostas_sem_veredito', fonte)` é textual e pode ser satisfeito por comentário. (`diff:app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:216`; `diff:app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:221`; `diff:app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:239`.)

- **O mutante clássico tem mordida funcional:** restaurar a leitura crua produz `False` para a fixture validada e contraria diretamente a asserção de CSF no pai; a regressão apenas na metade destino contraria a asserção correspondente. Além disso, leitura direta do atributo reprovaria o teste AST da fonte real. Execução ORM desses testes: [não verificado]. (`HEAD:app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:56`; `diff:app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:112`; `diff:app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:210`; `diff:app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:238`.) [nao verificado]

- **F — órfãs:** liberar o pai é coerente com (B), pois o caso marca a respondida como validada antes do consumo e o juiz exclui validadas. Contudo, o diff remove a asserção sobre o pai vivo e não acrescenta asserção de pai fechado. Ele cobra `falhas == 2`, evento pendente e uma tentativa. A quantidade empírica e a explicação concreta das duas passagens permanecem [não verificado]; `falhas` conta lotes de colaborador, não chamadas a `fechar`. (`HEAD:app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:149`; `HEAD:app/chamados/models.py:1873`; `diff:app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:157`; `HEAD:app/chamados/reconciliador.py:1352`; `HEAD:app/chamados/reconciliador.py:1403`.)

- **F — custo:** “3 por PAI” continua compatível com o caminho mínimo de uma disputa de origem, respostas já validadas/materializadas e nenhum destino: origem + perguntas + destinos. Não descreve universalmente o juiz atual. Respostas elegíveis consultam a tranca, que pode carregar colaborador/empresa e consultar competência/toque; a metade destino reexecuta `respostas_sem_veredito()` para cada pergunta, relendo perguntas da mesma disputa, com trabalho potencial `N × M`. A nota ainda repete a leitura de origem/perguntas após recusa. Tetos 220/280 e custos medidos após o delta: [não verificado]. (`HEAD:app/ponto/tests/test_selo_chokepoint_escrita.py:196`; `HEAD:app/ponto/tests/test_selo_chokepoint_escrita.py:210`; `HEAD:app/chamados/models.py:1872`; `HEAD:app/chamados/juizes.py:991`; `HEAD:app/chamados/juizes.py:1048`; `HEAD:app/chamados/juizes.py:1066`; `diff:app/ponto/supra_juiz.py:566`; `diff:app/chamados/models.py:728`.)

Linhas novas precisas para consolidação: `_segunda_disputa_pendente`91, criação d2 94, RED pai validado107/assert112, validação127, materializada137, RED segunda disputa149, repetição/dry-run168–172, veredito segunda174, sem conversa189, CSF destino206/assert210/assert213, detector216/221, classe AST225, teste fonte234/assert238/assert239, teste mutante241/assert242. (`diff:app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:91`.) [nao verificado]

## 13/11 08:25 · sessao ce8b6039e8c7 · - · - · origem=Ronald (tmux)

A. Depois do diff, `pode_executar_csf` pergunta:

- Origem: qualquer disputa relacionada devolve lista não vazia de `respostas_sem_veredito()`? Se sim, recusa. Não filtra disputa aberta. (`diff app/ponto/supra_juiz.py:563`)
- Destino: a pergunta que resolve o chamado pertence à lista `q.disputa.respostas_sem_veredito()`? Se sim, recusa; não basta outra pergunta pendente da mesma disputa. (`diff app/ponto/supra_juiz.py:566-568`)
- A função, `_gate_conversa_do_pai` e `_decisao_do_fechamento` não conservam leituras diretas de `respondida_em`/`validada_em`; o gate aplica a composição inteira, embora a docstring diga “só PAI”. (`app/chamados/models.py:664-697`; `app/chamados/models.py:704-716`; `diff app/ponto/supra_juiz.py:563-568`)

O predicado existente é exatamente: `validada_em is None`, `materializada_em is None`, **carimbo de resposta OU texto.strip()**, e dia não trancado, salvo `ignorar_tranca=True`. Portanto carimbo sem texto conta, texto sem carimbo conta, validação/materialização excluem e tranca exclui; “julgada”, sequência de decisões, `recusa_motivo` e `via_resolucao` isolados não são consultados. (`app/chamados/models.py:1856-1876`)

Desligamento e reconciliador usam a mesma chamada, sem alterar `ignorar_tranca`; `fechar()` também. (`app/colaboradores/services/desligamento.py:141`; `app/chamados/reconciliador.py:1164`; `app/chamados/models.py:1916`)

B. Caminho da premissa morta: `retratar_premissa_morta_colab` → `_retratar` → `transicionar_com_pauta(... ato=ch.reconciliar)` → `reconciliar` → `_decisao_do_fechamento` → `_gate_conversa_do_pai` → `pode_executar_csf` → predicado A. (`app/chamados/reconciliador.py:1459-1465`; `app/chamados/reconciliador.py:524`; `app/chamados/services/fecho_runtime.py:165`; `app/chamados/models.py:652`; `app/chamados/models.py:686`; `app/chamados/models.py:716`; `diff app/ponto/supra_juiz.py:563-568`)

**Mas existe uma guarda anterior divergente:** `_retratar` consulta `retencao_da_retratacao` antes de chegar ao gate; esta chama `irmas_respondidas_sem_consumo`. Essa função usa carimbo **E texto**, sem exclusão por tranca, no universo de crédito destino/pai. Assim uma pergunta respondida, não validada/materializada, mas trancada, pode ser retida aqui embora A a exclua; esse ramo não chega a A. (`app/chamados/reconciliador.py:519-524`; `app/chamados/reconciliador.py:447-450`; `app/chamados/juizes.py:2028-2034`; `app/chamados/models.py:1872-1876`)

Outro caminho preservado pergunta pela resposta usando **somente texto** antes de chamar o fechamento sistêmico: `_tem_resposta_do_colab` no cartório. Pergunta validada com texto continua excluída desses ramos, diferentemente de A. (`app/ponto/services/cartorio.py:1637-1651`; `app/ponto/services/cartorio.py:1661-1666`)

D. O gate pode segurar um pai quando outra disputa relacionada ainda tem resposta pendente, inclusive disputa fechada cujos campos posteriores voltaram a representar pendência; também consulta perguntas que resolvem esse chamado, ainda que pertençam a disputa com outro pai. O limite de entrada é apenas `pk` e categoria `disputa_supervisao`. (`app/chamados/models.py:713-716`; `diff app/ponto/supra_juiz.py:563-568`)

Os três criadores de produção reutilizam **qualquer disputa aberta do colaborador** antes de criar. Logo nenhum deles cria diretamente a segunda enquanto a primeira está aberta, como faz o helper do teste. (`app/chamados/services/disputa_emissao.py:531-566`; `app/chamados/services/disputa_emissao.py:1096-1103`; `app/chamados/services/disputa_emissao.py:1332-1338`; `diff app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:91-100`)

Entretanto, **o estado é alcançável por histórico e reabertura**:

1. D1 fecha, S88 resolve P. (`app/chamados/models.py:1962-1966`; `app/chamados/signals.py:43`)
2. Gestor muda P de resolvido para em análise pela resposta livre; essa matriz permite a transição e não reabre D1. (`app/chamados/services/acoes_chamado.py:319-322`; `app/chamados/services/fecho_runtime.py:175`; `app/chamados/models.py:128-199`; `app/chamados/catalogo/motor.py:102`)
3. Sem disputa aberta, o emissor reutiliza o mesmo P vivo pelo lookup categoria/módulo/status e cria D2. (`app/chamados/services/disputa_emissao.py:568-591`; `app/chamados/models.py:936-954`)
4. A ação administrativa encontra D1 fechada desse P, reabre sem conferir D2 e cria pergunta nova. Depois da resposta, há duas disputas abertas do mesmo pai. (`app/chamados/services/acoes_disputa.py:255-268`; `app/chamados/models.py:2168-2191`; `app/chamados/services/disputa_emissao.py:1467-1468`)

Portanto a fixture **fabrica diretamente um estado alcançável**, mas não testa sua criação por esse caminho; ocorrência em produção: [não verificado]. (`diff app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:91-100`)

Quanto ao destino ser um pai: a estrutura permite conversa do mesmo colaborador com esse destino, mas o derivador habitual exclui `disputa_supervisao`; não demonstrei um chamador de produção que construa tal arranjo. (`app/chamados/models.py:2824-2832`; `app/chamados/juizes.py:2263-2271`; `app/chamados/services/disputa_emissao.py:198-201`; [não verificado])

Bloqueante sustentado para consolidar: **a migração ao juiz A não cobre a guarda anterior de retratação e deixa intacto o predicado paralelo do cartório**, com divergências concretas de critério e alcance. (`app/chamados/reconciliador.py:519-524`; `app/chamados/juizes.py:2028-2034`; `app/ponto/services/cartorio.py:1641-1648`; `app/chamados/models.py:1872-1876`)

## 13/11 08:25 · sessao 7bfb0adffbf9 · - · - · origem=Ronald (tmux)

B. Os quatro nomes `LEGADO_*` entram em `ESTADOS_DE_LEGADO` e são registrados em `ESTADOS_LEGADOS`; esse último conta **leituras**, não células distintas (diff `app/escala/services/ata.py:28`, `:33`, `:115`). O censo corrige essa diferença contando cada ata não NULL pelo mesmo `leitura_orfas`, sem reclassificar seu conteúdo; NULL passado é contado por SQL separado (diff `app/escala/services/ata.py:203`, `:206`, `:209`). Contudo, os leitores não tratam LEGADO pelo nome: `horas or []`, `quantidade or 0` e `bool(horas) or bool(quantidade)` ignoram `.estado`, incluindo conversão de `None` para zero (HEAD `app/escala/services/ata.py:119`, `:130`, `:136`). Só o leitor novo de instantes verifica o estado (HEAD `app/escala/services/ata.py:147`).

C. O universo do censo é toda `CelulaDia`, sem recorte de empresa, situação, origem ou data para atas não NULL; NULL passado entra no débito e NULL hoje/futuro fica fora dele (diff `app/escala/services/ata.py:203`, `:209`, `:212`, `:213`). O corte do censo vem uma vez de `_hoje`, usando `datetime.date.today`; fuso efetivo do processo [não verificado] (diff `app/ponto/management/commands/tripwire_celulas.py:357`, `:359`, `:369`, `:423`). Entretanto, o **comando inteiro** ainda relê o relógio nos helpers chamados sem `ini/fim`: `medir_pausa_faltando` e `medir_sobra_com_tipo_trocado` (HEAD `app/ponto/management/commands/tripwire_celulas.py:87`, `:249`, `:381`, `:392`; diff novas `:88`, `:250`, `:387`, `:398`). O universo também não é uma fotografia única: há consultas separadas para atas gravadas, NULL passado, todas NULL e total (diff `app/escala/services/ata.py:203`, `:209`, `:212`, `:213`).

O contador corresponde à fórmula declarada para abrir CONTRACT — legado + ilegal + não lavrada passada — e a mensagem nomeia CONTRACT como pouso próprio; resultado real `contador=0` [não verificado] (diff `app/escala/services/ata.py:192`; diff `app/ponto/management/commands/tripwire_celulas.py:425`, `:436`). O cron está registrado como vigia (HEAD `app/config/crons.py:2171`). As rotas inspecionadas consultam banco e imprimem alarmes, mas escrevem memória e log quando passam pela porta (diff `app/escala/services/ata.py:48`, `:49`, `:115`, `:206`).

D. `_ilegal` em produção adiciona um sítio ao conjunto global, faz `logging.error` e levanta `EstadoIlegalDaAta`; não há condicionamento a CI (HEAD `app/escala/services/ata.py:43`, `:46`, `:47`, `:48`). LEGADO não chama `_ilegal`; apenas incrementa `ESTADOS_LEGADOS` (HEAD `app/escala/services/ata.py:112`). Ata não mapping chama `_ilegal`; o censo captura a exceção, conta ILEGAL e deixa o registro global para trás (diff `app/escala/services/ata.py:91`, `:206`, `:207`, `:208`). Isso provoca falha no hook/runner quando ocorre no mesmo processo da suíte; processos diferentes não compartilham esses conjuntos (HEAD `app/escala/services/ata_ci.py:62`, `:65`; `app/escala/runner_ata_ci.py:11`, `:16`).

O helper **pode esconder outro ilegal real do mesmo sítio**: o registro é um `set` de `(arquivo, linha, motivo)`, e todas as atas não mapping recusadas por `_valores_orfas` convergem no mesmo sítio; o helper verifica um sítio, não uma ocorrência/identidade de fixture, e limpa todo o conjunto (HEAD `app/escala/services/ata.py:45`, `:46`, `:89`; diff `app/escala/tests/test_tipo_item3_v5_expand.py:259`, `:260`, `:261`, `:262`). Detecta ilegais de outros sítios, mas não duplicatas no mesmo sítio (mesmas referências).

E. Mordida prevista por inspeção; execução dos mutantes [não verificado]:

| Mutante | Teste que falha | [nao verificado]
|---|---|
| LEGADO contado como LAVRADA | `test_RED_censo_nomeia_cada_estado_e_conta_o_debito`, mapa esperado; também saída `ata_fora_da_forma=7` no teste do comando (diff `app/escala/tests/test_tipo_item3_v5_expand.py:264`, `:267`, `:282`, `:298`). |
| Ignora `n_orfas != len(orfas)` | Os mesmos testes: a fixture `orfas=[]/n_orfas=2` precisa ser `LEGADO_DESCASADA` (diff `app/escala/tests/test_tipo_item3_v5_expand.py:243`, `:267`, `:298`). |
| Remove `ATA-FORA-DA-FORMA>0` | `test_o_tripwire_publica_a_linha_e_acusa`, assert literal do alarme (diff `app/escala/tests/test_tipo_item3_v5_expand.py:282`, `:300`). |

`test_MORDE_frota_na_forma_da_zero` não morde esses três: ele elimina todas as células exceto uma lavrada válida antes da medição (diff `app/escala/tests/test_tipo_item3_v5_expand.py:275`, `:277`, `:279`).

A cópia em disco contém HEAD sem o delta; os números acima identificados como **diff** correspondem ao anexo fornecido, não a código encontrado no disco (HEAD `app/escala/services/ata.py:176`; HEAD `app/ponto/management/commands/tripwire_celulas.py:363`).

## 13/11 08:25 · sessao 54e1555b97b2 · - · - · origem=Ronald (tmux)

**G — bloqueante demonstrável:** leitores de LEGADO não tratam o estado pelo nome; continuam em `horas or []`, `quantidade or 0` e coerção booleana. Nomear/contar antes não elimina o fallback proibido. `app/escala/services/ata.py` HEAD:93–114,117–136; diff:95–116,119–138. `_orfas_da_ata` também ignora `.estado` e usa `int(_l.quantidade or 0)`. `app/escala/services/leitor_celula.py`:189–194.

**G — sem prova de (B) agora:** escritor mantém `orfas` como strings HH:MM e `n_orfas=len`; o ISO entra em chave adicional. `app/escala/services/ata.py` HEAD:281–295; diff:321–335. O consumidor da chave nova é o próprio acessor; os usos encontrados são testes. `app/escala/services/ata.py` HEAD:139–164; `app/escala/tests/test_tipo_item3_v5_expand.py`:79,85,92,103,123. Assim, a troca produtiva de formato e leitor no mesmo pouso não está demonstrada [não verificado].

**H — relógio:** o censo novo recebe `hoje` da borda; porém o comando inteiro continua lendo relógio dentro de `medir_pausa_faltando` e `medir_sobra_com_tipo_trocado`, chamados sem argumentos. `app/ponto/management/commands/tripwire_celulas.py` HEAD:85–88,247–250,381,392; diff:85–88,247–250,390,401. São sítios preexistentes no diff, portanto não apresentar como regressão introduzida pelo delta [não verificado quanto à execução].

**H — campo com dois contextos de dia, já preservado:** PDF consulta uma entrada usando somente HH:MM contra conjunto que reúne órfãs de todos os dias da janela. `app/relatorios/pdf_espelho.py`:174–178,472–477. O teste novo afirma expressamente que esse defeito da base permanece; não é correção nem regressão desta volta. `app/escala/tests/test_tipo_item3_v5_expand.py`:151–159.

**H — não demonstrados:** segundo juiz, segundo escritor produtivo, alteração de ordem de trava ou abertura do pouso A das 12 chaves [não verificado]. O caminho visível continua `ata_do_dia → gravar_orfas → lavrar_veredito`; a catraca abrange três chaves. `app/ponto/services/cartorio.py`:772; `app/escala/services/ata.py` HEAD:32,281–295; `app/ponto/portas/celula.py`:998,1003,1036,1043.

**Pré-requisito de EXPAND:** os anexos não fornecem o DIFF por leitor na frota igual a zero antes do opus; seu cumprimento permanece [não verificado].

_4518 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
