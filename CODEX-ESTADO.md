# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 09:35** · publicado em **11/11/2026 09:35** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 09:35 · sessao ad850049a991 · - · - · origem=Ronald (tmux)

A sonda inline não tem arquivo local disponível; os números abaixo vêm apenas do anexo, portanto sua proveniência por `arquivo:linha` é **[nao verificado]**. Os leitores citados foram conferidos na cópia.

| Contador / saída | O que efetivamente mede / limite |
|---|---|
| `D01.colab_dia_candidato=7057` | Chaves `(colaborador_id,data_evento)` com pelo menos uma pergunta de cada posição, após exclusão de conversa/fora do fato e conferência `instancia_marco == instancia_do_motivo`. A posição vem de `app/chamados/services/declaracao_texto.py:55`. Não certifica intervalo específico; duas pausas ainda compartilham posição/motivo, limitação declarada em `:49`. |
| `D01.falta_lado=2777` | Chaves candidatas que não têm ambos os lados. Não são necessariamente pausas incompletas da célula; é ausência de perguntas em uma posição. **[nao verificado por arquivo:linha: lógica da sonda inline]** |
| `D01.multiplas_linhas_na_posicao=31` | Colab-dias com ambos os lados e cardinalidade diferente de um em pelo menos um lado. Incluídos nos 7057 e excluídos do teste de metade validada. Não conta duplicatas retratáveis ou quantidade de intervalos. **[nao verificado por arquivo:linha: sonda inline]** | [nao verificado]
| `D01.metade_validada_outra_viva=94` | Colab-dias com exatamente uma pergunta por posição e `(validada(a) ∧ C6(b)) ∨ (validada(b) ∧ C6(a))`. C6 vem de `app/chamados/juizes.py:839`; validada retorna morta em `:1325`. Não demonstra 94 pares reais da mesma pausa, nem histórico de espera, devolução ou recusa. |
| `D02.validada_sem_lastro=1181` | Perguntas com validação e `_pergunta_materializada_de_fato is False`. Esse leitor admite encerramentos por identidade e outras testemunhas, e não significa exclusivamente existência de batida nova: `app/chamados/juizes.py:647`, `:662`, `:688`, `:734`. Não prova 1181 validações inválidas. |
| `D03.carimbo_sem_lastro=4291` | Materialização timestamp presente e leitor de lastro `False`; mesma ressalva acima. |
| `D03.aplicacao.*` | Só perguntas `tipo_resposta='hora'` e texto não vazio. Retornos: aplicada 5641; aplicável 322; bloqueada_paridade 182; inválida 327; já_satisfeita 76; recusada_paridade 322. `app/chamados/services/materializacao.py:432`, `:434`, `:438`, `:442`, `:444`, `:448`, `:452`, `:457`. Não são retornos de execução da porta. |
| `D03.aplicada_sem_lastro=779` | Interseção de estado de aplicação `aplicada` e lastro `False`. O estado `aplicada` decorre do carimbo para qualquer via que não seja paridade: `app/chamados/services/materializacao.py:434`, `:438`, `:442`. Não equivale a 779 plantios malsucedidos. |
| `T11.reuso_no_slot=3700`; `D03.reuso_nao_aprovado=102` | Entre perguntas hora/texto com slot derivável, encontro do leitor de reuso; 102 encontram batida não aprovada. Uso real no escritor em `app/chamados/services/materializacao.py:859`, `:865`; UI requer aprovação em `:448`. Não demonstra 3700 reusos históricos. |
| `D05.pendencia_com_carimbo=79` | Campo de pendência não vazio e timestamp de materialização. Pendência pode coexistir com prosseguimento do plantio: `app/chamados/services/materializacao.py:845`, `:850`, `:852`. |
| `D05.recusa_com_lastro=5970` | Texto atual de recusa não vazio com lastro `True`. Não demonstra contradição ou recusa atual impeditiva, porque é texto remanescente e lastro de leitura atual. **[nao verificado por arquivo:linha: conjunção da sonda]** |
| `D05.codigo.*` | Classificação do texto atual de recusa por primeiro padrão textual correspondente; `OUTRO` é fallback. `app/chamados/catalogo/recusa.py:77`, `:81`, `:88`. Contagens: ABRE_TURNO 2; COLISAO 27; COMPETENCIA_APROVADA 41; DATA_EM_DISPUTA 33; NAO_GERA_BATIDA 53; OUTRO 5868; PARIDADE 418. Não é enumeração de atos históricos nem causas exclusivas dos 94 colab-dias. |
| `D07.vizinha_retratada=15` | Para hora/texto com slot derivável, o leitor retorna vizinha do mesmo tipo cuja `retratada_em` está preenchida. Busca anterior/posterior sem filtro de retratação: `app/chamados/services/materializacao.py:94`, `:98`, `:100`, `:104`. Não exige que a guarda bloqueie; nem que não haja reuso; não prova 15 recusas causadas por retratadas. |
| `T18.absolvivel=1` | Quantidade produzida por `perguntas_absolvidas_pela_celula` nos colaboradores alcançados pela lista de perguntas. O leitor aplica C6 e exclui dia trancado: `app/chamados/reconciliador.py:645`, `:661`, `:667`, `:672`. Não é fechamento executado ou DS fechável. |
| `T19.encerra=1` | Órfãs no universo `materializada=None`, `validada=None`, DS aberta, motivos específicos e dia presente, cujo veredito atual é ENCERRA: `app/chamados/reconciliador.py:1044`, `:1048`, `:1082`, `:1084`, `:1086`, `:1090`, `:1092`. Não é carimbo executado. |
| `T20.DS_abertas=486`; `T20.C7_true=23` | DS com `fechada_em=None`; dentre elas, C7 retorna `True`. C7 exige ausência de resposta sem veredito e fio vivo: `app/chamados/models.py:1842`, `:1848`, `:1853`. Não explica por que não fecharam nem identifica as antigas 18. |
| Caso | Existem exatamente as duas perguntas pedidas, ambas col960, segundo `caso_51522_51523 n2` e `caso.col960_confere n1`. Não foram impressos timestamps, C6, DS, data, marco ou intervalo delas. Estado e pertencimento a 94/486/23/grupos continuam **[nao verificado]**. |

**D08 não foi medido na saída disponibilizada.** Não há `D08.grupo`, `D08.grupos`, `D08.excedentes` nem baldes. As assinaturas do inline são compatíveis com a cópia: `app/chamados/services/fio.py:1225`, `:1259`, `:1905`, `:1923`. `grupos_de_colisao` começa com lista vazia e pode devolvê-la (`:1942`, `:1993`); o `Counter` inline só toca D08 dentro do laço. Assim, ausência de chaves não demonstra falha e não autoriza preencher o censo como zero. Os 486 DS abertas não são evidência dos 486 grupos antigos.

Não aparece `sonda_nao_verificada` ou `nao_verificado.*` no dado. T20, T18/T19 e caso presentes tornam indevida a afirmação de aborto no laço D08 para o inline exatamente mostrado. Qual script exato produziu a saída e por que D08 está ausente: **[nao verificado]**.

**Limite dos próprios leitores:** `_plantio_preenche_furo_na_data` engole exceção e retorna `False` (`app/chamados/services/materializacao.py:184`); o gerador de absolvíveis captura exceção e ignora pergunta (`app/chamados/reconciliador.py:669`). Portanto ausência de `nao_verificado.*` externo não certifica inexistência de falhas internas. Logs não fornecidos: **[nao verificado]**.

**Divergências C7:** explicador calcula somente `not fio_vivo` (`app/chamados/services/explicador.py:137`); relatório publica PRONTA_FECHAR se não houver vivas (`app/chamados/relatorios_disputa.py:18`, `:20`). Nenhum número de divergências foi impresso. Não convertê-lo em zero sem esclarecer a instrumentação/saída.

## 11/11 09:35 · sessao 9ad8f4f4c039 · - · - · origem=Ronald (tmux)

Não localizei no AST um predicado que receba **duas `PerguntaDisputa` e responda se pertencem à mesma pausa**. A ausência universal por reflexão, código gerado ou chamadas dinâmicas permanece **[nao verificado]**. A inspeção executada em memória percorreu 838 arquivos Python de runtime em `app/` e `bin/`, com 5.422 funções e nenhum erro de parse; essa execução não tem artefato `arquivo:linha` **[nao verificado por arquivo:linha]**.

O par dos marcos já está representado na própria célula: `CelulaDia.PARES_PAUSA_DNA` contém `('hii','hfi')` e `('hii2','hfi2')`; a entidade atribui S/E e fornece a ordem canônica. Isso identifica **marcos**, sem comparar perguntas. (`app/escala/models.py:1652`, `:1654`, `:1659`, `:1672`.)

| Leitor existente | O que responde |
|---|---|
| `EscalaColaborador.pausas_do_dia` | Todas as pausas do DNA da célula; célula existente sem marcos devolve `()`. Template somente quando falta célula. (`app/escala/models.py:1166`, `:1189`, `:1197`, `:1202`, `:1208`.) |
| `intervalo_do_dia` | Somente a primeira pausa retornada pela irmã acima. (`app/escala/models.py:1163`.) |
| `grade_da_celula` / `_celulas_da_ata` | Marcos e lâmpadas lavrados do dia; expõe tipo/hora, preservando ordem. (`app/escala/services/leitor_celula.py:114`, `:122`, `:148`, `:350`.) |
| `posicao_do_marco` | Primeiro=`inicio`, último=`fim`, intermediário S=`int_ida`, E=`int_volta`. Não identifica sozinho qual intervalo. (`app/chamados/services/declaracao_texto.py:75`, `:79`, `:83`.) |
| `_marcos_faltantes` | Lê a grade e retorna `(dia,pos,tipo,hora)` dos marcos faltantes. Não devolve índice da pausa. (`app/chamados/services/declaracao_texto.py:100`, `:121`, `:130`, `:139`.) |
| `tarefa_do_dia._posicoes` | Enumera repetições em memória: `int_ida`, `int_ida:2`, `int_volta`, `int_volta:2`. (`app/chamados/services/tarefa_do_dia.py:57`, `:65`, `:67`.) |
| `instancias_do_turno` | Publica instância, posição, tipo, hora, lâmpada e data civil a partir do dia do leitor único. (`app/chamados/services/tarefa_do_dia.py:91`, `:96`, `:101`.) |
| `_pos_da_pergunta` | Lê `instancia_marco`, com alternativa pelo motivo; não compara duas perguntas. (`app/chamados/services/tarefa_do_dia.py:106`.) |
| `pausas_dna_periodo` / `pausas_de_celulas` | Leitores em lote das pausas pelas chaves da própria entidade. (`app/escala/utils.py:27`, `:57`, `:111`, `:141`.) |

Há um impedimento concreto à persistência do índice já produzido pela tarefa: o nascimento da pergunta deriva `instancia_marco` somente do motivo e **rejeita valor diferente**, como `int_ida:2`. O próprio comentário reconhece a separação de duas pausas como pendente. (`app/chamados/models.py:3425`, `:3437`, `:3440`, `:3441`, `:3445`.) A tabela motivo→posição também documenta que duas pausas caem no mesmo motivo; `_complementar_marcos_faltantes` consome os marcos, mas descarta `_hora` ao criar a pergunta. (`app/chamados/services/declaracao_texto.py:48`, `:55`; `app/chamados/services/disputa_emissao.py:873`, `:881`.)

Os produtores/escritores relacionados são:

- `montar_dna` produz `hii/hfi` e o segundo par pelas chaves de `PARES_PAUSA_DNA`; `montar_lampadas` produz as lâmpadas com `chave`, tipo, hora e instante. (`app/ponto/management/commands/gerar_celulas.py:73`, `:81`, `:92`, `:94`, `:36`, `:60`, `:68`.)
- A porta da célula persiste geração, regeneração, edição humana e reversão. (`app/ponto/portas/celula.py:816`, `:857`, `:110`, `:256`, `:272`, `:534`, `:591`, `:604`, `:740`, `:786`.)
- `PerguntaDisputa.save` chama `_chave_de_identidade` na inserção; esta escreve a posição simples em `instancia_marco`. (`app/chamados/models.py:3413`, `:3420`, `:3425`, `:3440`.)

Portanto, existem a autoridade dos pares de marcos e leitores de posições/instâncias; a ligação persistida **pergunta→marco específico da pausa** ainda perde o índice nos sítios acima. Não se deve chamar `_posicoes` de juiz “estas duas perguntas são o mesmo par”: seu contrato é enumerar os marcos da ata. (`app/chamados/services/tarefa_do_dia.py:57`, `:67`; `app/chamados/models.py:3437`, `:3445`.)

## 11/11 09:34 · sessao b113ca5b5451 · - · - · origem=Ronald (tmux)

Após o aval, **D1–D7 não têm pergunta de lei pendente; continuam com trabalho técnico de implementação/verificação. D8 mantém a pergunta de lei sobre identidade BATIDA do acervo versus BATIDA com episódio.** O novo aval resolve especificamente a identidade da pausa pela célula; não responde à identidade de D8. (`app/chamados/services/fio.py:1274`, `app/chamados/services/fio.py:1275`; conteúdo do aval recebido: **[nao verificado por arquivo:linha]**.)

| D | Situação após aval | Lei escrita versus decisão técnica; evidência |
|---|---|---|
| **D1** | **Decisão fechada; implementação aberta.** Aplicar o par no mesmo ato, esperar a outra metade em estado derivado; identificar saída/volta do mesmo intervalo pelos marcos da célula. Sem marcos, cada pergunta segue sozinha. | O ato conjunto está em `app/docs/BACKLOG.md:1929`; estado explícito foi superado pelo derivado em `app/docs/BACKLOG.md:1930`. Fonte única de previsto: S133/L-024, `app/docs/LEIS.md:75`; testemunha lê a autoridade: `CLAUDE.md:20`; cadastro não se cura por código: L-099, `app/docs/LEIS.md:122`. O complemento específico do novo aval está apenas no texto recebido: **[nao verificado por arquivo:linha]**. Hoje a validação ainda materializa uma P (`app/chamados/services/validacao.py:461`, `:463`) e o lote itera individualmente (`app/chamados/services/materializacao.py:1375`, `:1376`). |
| **D2** | **Sem pergunta nova; harmonização técnica aberta.** Preservar diferença entre resposta humana, projeção suprida e reversão. | Escritor canônico e trilha são L-007 (`app/docs/LEIS.md:65`). Nova resposta limpa validação (`app/chamados/services/disputa_emissao.py:1468`, `:1480`); devolução também limpa, registrando DEVOLUÇÃO (`app/chamados/services/validacao.py:199`, `:205`); carimbo pode suprir resposta/validação (`app/chamados/models.py:3132`, `:3135`). Como compatibilizar essas portas é decisão técnica sob a lei escrita. |
| **D3** | **Sem pergunta nova; significado e leitores técnicos abertos.** Carimbo terminal não prova plantio. | Testemunhas leem autoridade (`CLAUDE.md:20`; `app/docs/LEIS.md:60`). Há encerramento por identidade sem criar batida (`app/chamados/services/materializacao.py:924`, `:1099`), enquanto o leitor de aplicação chama quase todo carimbo de “aplicada” (`app/chamados/services/materializacao.py:434`, `:438`, `:442`) e histórico o descreve como fato registrado (`app/chamados/services/historico.py:103`, `:104`). Adequação dessas testemunhas é técnica. |
| **D4** | **Sem pergunta nova; convergência técnica aberta.** | Um escritor canônico e exceção com trilha: L-007 (`app/docs/LEIS.md:65`; `CLAUDE.md:32`). Existem atribuição comum (`app/chamados/models.py:3101`), descarimbo em lote (`:3266`), troca isolada (`:3275`, `:3276`) e relavra (`app/chamados/services/relavra_via.py:125`, `:127`). Organizar as portas existentes sob a obrigação já escrita é técnico. |
| **D5** | **Sem pergunta nova; consumidores técnicos abertos.** Não tratar pendência/recusa como “aguarda metade”. | Estado explícito foi superado (`app/docs/BACKLOG.md:1930`); fonte e escritor únicos são lei (`CLAUDE.md:20`, `:32`). Pendência grava motivo (`app/chamados/services/materializacao.py:850`, `:851`); contestação humana também grava motivo (`app/chamados/services/disputa_emissao.py:1497`); COLISÃO pode encerrar identidade (`app/chamados/services/materializacao.py:1093`, `:1099`). Separar esses usos é técnico. |
| **D6** | **Autoridade fechada; migração e causalidade histórica abertas.** | C7 é juiz único por corte expresso (`app/docs/CORTES.md:272`), registrado em `app/core/juizes.py:279`; sua regra completa está em `app/chamados/models.py:1853`. Explicador só consulta fio vivo (`app/chamados/services/explicador.py:137`), relatório publica PRONTA_FECHAR sem vivas (`app/chamados/relatorios_disputa.py:18`, `:20`). Migrá-los é técnico. Autor automático = autor do último ato com motivo próprio é decisão técnica explícita no novo aval: **[nao verificado por arquivo:linha]**. Razão histórica de cada DS continuar aberta: **[nao verificado]**. |
| **D7** | **Lei fechada; avaliação/correção técnica aberta.** | Universo apurável exclui retratada: L-025 (`app/docs/LEIS.md:76`); papel vem do marco, não do tipo gravado: L-087 (`app/docs/LEIS.md:119`). A guarda atual lê `Batida.objects` e compara `.tipo` (`app/chamados/services/materializacao.py:94`, `:98`, `:100`, `:104`). Isso não autoriza remover a guarda sem definir/testar comportamento pela autoridade existente; solução é técnica. |
| **D8** | **Pergunta de lei ainda aberta.** Quando BATIDA do acervo sem episódio representa o mesmo fato de BATIDA com episódio, permitindo retratação entre ambas? | O código declara literalmente a lei aberta e conserva o acervo (`app/chamados/services/fio.py:1274`, `:1275`, `:1294`, `:1295`). O plano também protege mérito (`:1290`, `:1291`) e episódios distintos (`:1287`, `:1292`, `:1293`). O novo aval sobre pares da pausa não responde a essa identidade: **[nao verificado por arquivo:linha quanto ao alcance do anexo]**. |

Referências dos agregados antigos:

- **77 colab-dias**, 48 colabs e 18 na competência 11 estão registrados em `app/docs/BACKLOG.md:1882`.
- **18 DS com pode_fechar=True**, 77 pares e 848 duplicadas aparecem na descrição da raiz em `app/docs/BACKLOG.md:1883`.
- **486 grupos/848 excedentes em disputa aberta** estão registrados em `app/docs/BACKLOG.md:1887`.

Essas linhas documentam a medição anterior; não demonstram equivalência entre seu universo e o da sonda recebida. (`app/docs/BACKLOG.md:1882`, `:1883`, `:1887`; equivalência dos universos: **[nao verificado]**.)

## 11/11 09:26 · sessao d8f1db422342 · FIO-E0-V4-E4B · construtor · origem=Code

Corrigi a fixture em [test_e4b_c4_orm.py:375](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm.py:375). **GREEN ORM e morte ORM de `sem_rollback` continuam [nao verificado].**

1. **A deficiência demonstrada é a falta de protesto cobrável na fixture.** A lavra sem `codigos` grava `protestos=[]` (`ponto/portas/celula.py:1011`); o c2 recusa criar o chamado sem esse protesto (`chamados/reconciliador.py:867`). A assertion exige pergunta pendente **com destino**, portanto sua falha não demonstra ausência de qualquer pergunta (`chamados/tests/test_e4b_c4_orm.py:429`).

   O aviso é impresso em `chamados/services/declaracao_texto.py:71`, chamado pela abstenção em `chamados/services/disputa_emissao.py:911`. O aviso fornecido nomeia 21/09, dia de P90; o vizinho é 02/09 (`chamados/tests/test_e4b_c4_orm.py:21`, `chamados/tests/test_e4_backfill.py:28`). Pelo ponto alcançado na falha fornecida, as checagens de vizinho resolvido e ausência de rollback integral já haviam passado (`chamados/tests/test_e4b_c4_orm.py:426`).

2. **A fixture agora lavra `FURO_PARCIAL` e verifica protesto persistido e ata legível**, incluindo a entrada apagada (`chamados/tests/test_e4b_c4_orm.py:375`). Permanecem a precondição de persistência e a assertion-alvo de P90 (`chamados/tests/test_e4b_c4_orm.py:429`, `chamados/tests/test_e4b_c4_orm.py:434`).

3. **Defeito de produção perdendo escrita aceita: [nao verificado].** A correção ficou nos testes. O novo ensaio executa a chamada extraída da fixture, a lavra real e o corpo inteiro do c2; suas bordas simuladas não demonstram persistência ou sinais ORM ([test_e4b_c4v11_memoria.py:1](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4v11_memoria.py:1)). [nao verificado]

Diff central:

```diff
-        cel_vizinha.lavrar_veredito(via='cartorio', veredito='furo', ata=self.cel.ata)
+        cel_vizinha.lavrar_veredito(via='cartorio', veredito='furo',
+                                   codigos=['FURO_PARCIAL'], ata=self.cel.ata)
```

O diff também acrescenta as precondições em `test_e4b_c4_orm.py:377`, o novo RED com três mutantes em `test_e4b_c4v11_memoria.py:31` e sua inclusão no roteiro em [e4b_c4_verificar.py:68](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c4_verificar.py:68).

Resultados locais observados:

| Verificação | rc | Resultado / referência |
|---|---:|---|
| RED antes da correção | 1 | Assertion de destino; `test_e4b_c4v11_memoria.py:114` |
| Memória completa após alterações | 0 | 90 testes; comando abaixo | [nao verificado]
| `--selos` | 0 | 40 testes; `e4b_c4_verificar.py:238` | [nao verificado]
| `--orm-preflight` | 0 | 8 enxertos; `e4b_c4_verificar.py:78` |
| `sem_protestos` | 1 | Assertion de destino; `test_e4b_c4v11_memoria.py:114` |
| `protesto_nao_cobravel` | 1 | Mesma assertion; `test_e4b_c4v11_memoria.py:114` |
| `roteiro_sem_fixture` | 1 | Assertion de inclusão; `test_e4b_c4v11_memoria.py:52` |
| `sem_rollback`, memória | 1 | Assertion-alvo de P90; `test_e4b_c4v10_memoria.py:89` |
| 24 mutantes c4v7 | 1 cada | `AssertionError`, sem erros; catálogo em `test_e4b_c4v7_memoria.py:441` |
| GREEN / mutantes ORM | — | [nao verificado] |

Comandos reproduzíveis:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest \
  chamados.tests.test_e4b_classe_e_memoria \
  chamados.tests.test_e4b_c4_tranca_memoria \
  chamados.tests.test_e4b_c4v7_memoria \
  chamados.tests.test_e4b_c4v10_memoria \
  chamados.tests.test_e4b_c4v11_memoria \
  chamados.tests.test_e4b_c4_base \
  chamados.tests.test_e4b_c4_orm_protocolo -q

PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4b_c4_verificar --selos
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4b_c4_verificar --orm-preflight

PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4b_c4v11_memoria --mutante sem_protestos
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4b_c4v11_memoria --mutante protesto_nao_cobravel
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4b_c4v11_memoria --mutante roteiro_sem_fixture
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4b_c4v10_memoria --mutante sem_rollback
```

Para a sessão executar o ORM, primeiro o GREEN isolado, depois `sem_rollback` e o roteiro completo. A assertion esperada do mutante permanece em `chamados/tests/test_e4b_c4_orm.py:435`; os resultados abaixo são [nao verificado].

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4b_c4_orm.CercaC4ORMTest.test_RED_lote_misto_desfaz_recusado_enquanto_vizinho_pousa --noinput

E4B_C4_MUTANTE=sem_rollback PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4b_c4_orm.CercaC4ORMTest.test_RED_lote_misto_desfaz_recusado_enquanto_vizinho_pousa --noinput

PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4b_c4_verificar --orm
```

## 11/11 09:19 · sessao 492833d88911 · RAIZ-DA-DISPUTA · censo · origem=Code

**F3/F4 têm oito decisões identificadas e uma pergunta de lei: a identidade entre BATIDA do acervo sem episódio e BATIDA com episódio.** A aplicação conjunta do par, a espera pela outra metade, o estado derivado e a autoridade C7 já têm decisão registrada; suas divergências atuais são de implementação. (`app/chamados/services/fio.py:1275`; `app/docs/BACKLOG.md:1929`, `:1930`; `app/docs/CORTES.md:272`.)

Identidade Git da cópia com o hash informado: **[nao verificado]**. Estado atual de col960 e das perguntas 51522/51523: **[nao verificado; aguarda sonda]**. Os anexos F0/F1 permanecem como base, sem novo levantamento de nascimento, dona ou destino nesta entrega. **[nao verificado por arquivo:linha: referência aos anexos recebidos]**

Os números fornecidos são **77 colab-dias**, **486 grupos/848 excedentes** e **18 DS com `pode_fechar=True`**; não constituem partição por causa, transição ou decisão. A ligação dos 77 e dos 18 à obra está registrada; equivalência dos 486/848 ao universo do código e distribuição de todos esses números: **[aguarda sonda]**. (`app/docs/BACKLOG.md:1882`, `:1883`; chave dos grupos: `app/chamados/services/fio.py:1923`, `:1945`, `:1976`.)

O censo abaixo usa os seguintes prefixos. Cada arquivo da primeira coluna, combinado com cada número das demais colunas, constitui uma referência `arquivo:linha`.

| Prefixo | Caminho |
|---|---|
| C | `app/chamados/` |
| S | `app/chamados/services/` |
| Q | `app/chamados/management/commands/` |
| H | `app/templates/chamados/partials/` |
| P | `app/ponto/` |
| T | `app/chamados/tests/` |

**DENTRO** significa `C/models.py` ou `S/`, conforme o perímetro pedido; **FORA** compreende juízes, reconciliador, commands, views, templates e `bin/`. Essa classificação é por localização, não uma prova de que todo método localizado em `models.py` execute pela raiz DS. (`app/chamados/models.py:1848`, `:3103`; `app/chamados/reconciliador.py:340`.)

Os nomes reais e as distinções de estado são:

| Pedido | Nome real / natureza |
|---|---|
| respondida | Coluna `PerguntaDisputa.respondida_em`; `respondida` é **propriedade**, definida por timestamp não nulo. (`C/models.py:2448`, `:3496`, `:3498`.) |
| validada | Coluna `PerguntaDisputa.validada_em`, acompanhada de autor nas portas. (`C/models.py:2449`; `S/validacao.py:134`, `:135`.) |
| materializada | Coluna `PerguntaDisputa.materializada_em`, acompanhada de autor; seu help text fala em batida criada, mas portas também a usam para encerramento sem plantio. (`C/models.py:3393`, `:3395`; `S/materializacao.py:924`, `:1099`.) |
| via | Coluna `PerguntaDisputa.via_resolucao`. (`C/models.py:2561`.) |
| pendência | Coluna `PerguntaDisputa.pendencia_admin`; não é via de encerramento. (`C/models.py:2556`, `:2558`.) |
| recusa | Coluna `PerguntaDisputa.recusa_motivo`. (`C/models.py:2560`.) |
| pode_fechar | **Método derivado de `DisputaSupervisao`, não coluna nem property**: ausência de resposta sem veredito **e** ausência de fio vivo. (`C/models.py:1848`, `:1853`.) |
| estado | **Propriedade derivada da pergunta**; traduz validação, materialização, resposta e C6. (`C/models.py:3033`, `:3041`, `:3044`, `:3048`.) |

O AST foi executado por `python3 -B` em stdin, com revisão dos fluxos dinâmicos; não foi gravado artefato de execução. **[nao verificado por arquivo:linha quanto à execução]** A unidade das tabelas é **uma linha por `(campo, arquivo)`**, reunindo os sítios desse arquivo. Portanto as contagens abaixo são linhas das tabelas, não funções, atribuições ou registros de produção. **[nao verificado por arquivo:linha; reproduzível pelo script ao fim]**

| Campo | Linhas da tabela |
|---|---:|
| `respondida_em` | 33 |
| `validada_em` | 22 |
| `materializada_em` | 23 |
| `via_resolucao` | 21 |
| `pendencia_admin` | 6 |
| `recusa_motivo` | 12 |
| `pode_fechar`, incluindo derivações concorrentes | 11 |
| Total | 128 |

Nas tabelas, **m** é atribuição em memória; **P** é persistência associada; **W** é inserção/update ou delegação de escrita. A restauração em memória e o proxy de ressuscitação estão separados das escritas persistentes. (`C/models.py:1507`, `:1508`, `:3152`, `:3159`, `:3264`.)

O levantamento de **`respondida_em`** é:

| Arquivo / fronteira | Escritor ou memória | Leitor |
|---|---|---|
| `C/models.py` — DENTRO | nascimento W3017; ressuscitação W1566; lote W3266; carimbo m3132/P3152; **só memória** m1508,3163 | 1564,1844,3045,3048,3127,3131,3348,3498,3924,3931,3932,3933 |
| `S/disputa_emissao.py` — DENTRO | resposta m1468/P1508; correção W1630,1632; automática m1653/P1655; trilha m1674/P1676 | 1619,1670,1712,1856,1929,1946 |
| `S/acoes_disputa.py` — DENTRO | — | 95,130 |
| `S/becos.py` — DENTRO | — | 30 |
| `S/cobranca.py` — DENTRO | — | 228 |
| `S/declaracao_texto.py` — DENTRO | — | 417,419,687 |
| `S/fio.py` — DENTRO | — | 340,660 |
| `S/furo_sem_canal.py` — DENTRO | — | 80 |
| `S/historico.py` — DENTRO | — | `getattr`105 |
| `S/materializacao.py` — DENTRO | — | 669,1369 |
| `S/painel.py` — DENTRO | — | 67 |
| `S/passivo_validado.py` — DENTRO | — | snapshot dinâmico83 |
| `S/pergunta_no_app.py` — DENTRO | — | 71 |
| `S/raiox.py` — DENTRO | — | 138 |
| `S/validacao.py` — DENTRO | — | 393,576,581,924 |
| `S/verbos.py` — DENTRO | — | 309,312 |
| `S/vigia_fio.py` — DENTRO | — | 675,783 |
| `app/api/views.py` — FORA | — | 1172,1184,2568,2582,2625,2642 |
| `app/api/views_mensageria.py` — FORA | — | 569,570 |
| `C/juizes.py` — FORA | — | 829,1341,1449,1594,1601,1604,1949 |
| `Q/auditar_invariantes_chamados.py` — FORA | — | 177,592,1105,1572,1602 |
| `Q/backfill_decisoes_pergunta.py` — FORA | — | 56,60 |
| `Q/curar_perguntas_em_limbo.py` — FORA | — | 46,69 |
| `C/relatorios_disputa.py` — FORA | — | 21 |
| `C/veredito_lavrado.py` — FORA | — | 206,208 |
| `C/views.py` — FORA | — | 1280,1416 |
| `C/views_cobrar.py` — FORA | — | 169,175 |
| `P/management/commands/reconciliar_grade.py` — FORA | — | 460 |
| `P/supra_juiz.py` — FORA | — | 545,548 |
| `H/_card_atendimento.html` — FORA | — | 94,100,137,155,384,385 |
| `H/_pergunta_card.html` — FORA | — | 24,29,32 |
| `H/modal_fio.html` — FORA | — | 185,217,220 |
| `bin/censo_folha.py` — FORA | — | filtros28,29 |

O levantamento de **`validada_em`** é:

| Arquivo / fronteira | Escritor ou memória | Leitor |
|---|---|---|
| `C/models.py` — DENTRO | nascimento W3017; ressuscitação W1566; lote W3266; carimbo m3135/P3152; **só memória** m1507,3164 | 309,344,1564,1843,3041,3127,3134,3359,3924,3935,3936,3937 |
| `S/disputa_emissao.py` — DENTRO | resposta nova m1480/P1508; correção W1630,1632 | 1470,1471,1620,1917 |
| `S/validacao.py` — DENTRO | `_carimbar_validacao` m134/P137; devolução m199/P202 | 139,147,188,195,345,396,472,573,732,753 |
| `S/acoes_disputa.py` — DENTRO | — | 345 |
| `S/declaracao_texto.py` — DENTRO | — | 227 |
| `S/historico.py` — DENTRO | — | `getattr`105 |
| `S/materializacao.py` — DENTRO | — | 670,1369 |
| `S/painel.py` — DENTRO | — | 67,270,271,272,273 |
| `S/passivo_validado.py` — DENTRO | — | snapshot83 |
| `S/verbos.py` — DENTRO | — | 239,246 |
| `S/vigia_carimbo.py` — DENTRO | — | 97 |
| `C/juizes.py` — FORA | — | 829,1325,1949 |
| `Q/auditar_invariantes_chamados.py` — FORA | — | 205,1075,1572,1602,1765,1778 |
| `Q/backfill_decisoes_pergunta.py` — FORA | — | 56,60,100 |
| `Q/encerrar_perguntas_com_fato.py` — FORA | — | 31 |
| `Q/reabrir_orfas_enterradas.py` — FORA | — | 67 |
| `C/reconciliador.py` — FORA | — | 627,1048 |
| `P/services/flip_auto.py` — FORA | — | 128 |
| `P/services/procedencia_batida.py` — FORA | — | 74 |
| `H/_pergunta_card.html` — FORA | — | 20,23,42 |
| `H/modal_fio.html` — FORA | — | 228 |
| `bin/censo_folha.py` — FORA | — | 29 |

O levantamento de **`materializada_em`** é:

| Arquivo / fronteira | Escritor ou memória | Leitor |
|---|---|---|
| `C/models.py` — DENTRO | nascimento W3017; `_escrever_carimbo` m3099; portas P3152,3190,3212; lote W3264 | 1483,1843,1925,3043,3046,3056,3126,3182,3183,3284,3296,3313,3350,3352 |
| `S/disputa_emissao.py` — DENTRO | — | 226,762,1070,1089,1323,1621,1823,1917 |
| `S/fabrica_da_celula.py` — DENTRO | — | 170 |
| `S/historico.py` — DENTRO | — | `getattr`105 |
| `S/materializacao.py` — DENTRO | — | 434,532,623,627,632,671,1089,1093,1369,1418 |
| `S/painel.py` — DENTRO | — | 67 |
| `S/passivo_validado.py` — DENTRO | — | snapshot83 |
| `S/raiox.py` — DENTRO | — | 135,175 |
| `S/relavra_via.py` — DENTRO | — | 181,244 |
| `S/validacao.py` — DENTRO | — | 188,190,461,472,490 |
| `C/juizes.py` — FORA | — | 288,662,689,831,1336,1362,1368,1399,1417,1949 |
| `Q/auditar_disputas.py` — FORA | — | 69,148 |
| `Q/auditar_invariantes_chamados.py` — FORA | — | 470,538,587,905,955,1075,1156,1572,1602,1765 |
| `Q/backfill_decisoes_pergunta.py` — FORA | — | 100,325 |
| `Q/encerrar_perguntas_com_fato.py` — FORA | — | 31 |
| `Q/migrar_data_evento_dia_do_turno.py` — FORA | — | 147 |
| `Q/reabrir_orfas_enterradas.py` — FORA | — | 67,88,91,129,142 |
| `C/reconciliador.py` — FORA | — | 366,627,1048,1144,1154,1500 |
| `C/veredito_lavrado.py` — FORA | — | 202,204 |
| `C/views.py` — FORA | — | 1257 |
| `P/management/commands/reconciliar_fantasmas.py` — FORA | — | 211 |
| `P/services/procedencia_batida.py` — FORA | — | 74 |
| `P/services/veredito_celula.py` — FORA | — | 195 |

O levantamento de **`via_resolucao`** é:

| Arquivo / fronteira | Escritor ou memória | Leitor |
|---|---|---|
| `C/models.py` — DENTRO | nascimento W3017; `_escrever_carimbo` m3101; portas P3152,3190,3212; lote W3264; `encerrar_por_fato_no_espelho` m3275/P3276 | 1483,1920,2054,2060,2722,2755,3055,3126,3201,3264,3273,3284,3296,3313,3320,3350,3352,3485,3491 |
| `S/relavra_via.py` — DENTRO | **`relavrar` m125/P127** | 118,121,123,153,181,245 |
| `S/competencia_trancada.py` — DENTRO | — | 251 |
| `S/disputa_emissao.py` — DENTRO | — | 327,398,417,436,1927,1942,1945 |
| `S/explicador.py` — DENTRO | — | 95 |
| `S/fabrica_da_celula.py` — DENTRO | — | 148 |
| `S/fio.py` — DENTRO | — | 1099,1166,1243,1350,1431,1852,1888,1910,1954 |
| `S/materializacao.py` — DENTRO | — | 438,623,668,1089 |
| `S/passivo_validado.py` — DENTRO | — | 83,140,141,142,515 |
| `S/tarefa_do_dia.py` — DENTRO | — | 227,228 |
| `C/juizes.py` — FORA | — | 662,674,830,1336,1362 |
| `Q/auditar_invariantes_chamados.py` — FORA | — | 202,206,433,542,955,1710 |
| `Q/backfill_decisoes_pergunta.py` — FORA | — | 100,314,330,398,504 |
| `Q/encerrar_perguntas_com_fato.py` — FORA | — | 31,42,49,66,76 |
| `Q/migrar_data_evento_dia_do_turno.py` — FORA | — | 70,71,72 |
| `Q/reabrir_orfas_enterradas.py` — FORA | — | 67,129,142,160 |
| `Q/relavrar_via_i6.py` — FORA | — | 58,59,62,66,68,88 |
| `C/reconciliador.py` — FORA | — | 1143,1154 |
| `P/services/cartorio.py` — FORA | — | 1610 |
| `P/services/flip_auto.py` — FORA | — | 128 |
| `H/_card_atendimento.html` — FORA | — | 186 |

O levantamento de **`pendencia_admin`** é:

| Arquivo / fronteira | Escritor | Leitor |
|---|---|---|
| `C/models.py` — DENTRO | nascimento W3017; default2556 | — |
| `S/materializacao.py` — DENTRO | `_materializar_resposta_hora` m850/P851 | — |
| `S/explicador.py` — DENTRO | — | 85,90 |
| `Q/auditar_invariantes_chamados.py` — FORA | — | 1112 |
| `H/_card_atendimento.html` — FORA | — | 183 |
| `H/modal_fio.html` — FORA | — | 222,225 |

O levantamento de **`recusa_motivo`** é:

| Arquivo / fronteira | Escritor | Leitor |
|---|---|---|
| `C/models.py` — DENTRO | nascimento W3017; `carimbar_identidade` m3188/P3190 | — |
| `S/disputa_emissao.py` — DENTRO | contestação m1497/P1508 | — |
| `S/materializacao.py` — DENTRO | pendência m850/P851; recusa m1102/P1103 | refresh1089; comparação1092 |
| `S/validacao.py` — DENTRO | recusa admin m584/P590 | 147,188,192,208,225,228,582,604 |
| `S/explicador.py` — DENTRO | — | 90 |
| `S/passivo_validado.py` — DENTRO | — | snapshot83 |
| `Q/auditar_invariantes_chamados.py` — FORA | — | 535,1075,1112 |
| `P/services/flip_auto.py` — FORA | — | regex132 |
| `H/_card_atendimento.html` — FORA | — | 186 |
| `H/modal_fio.html` — FORA | — | 226 |
| `bin/sombra_mascara_pessoal.py` — FORA | `setattr`57; `bulk_update`62,64 | carregamento51; `getattr`54; comparação56 |
| `bin/sombra_selo_pessoal.py` — FORA | — | SQL77 |

A escrita externa da máscara é efetivamente da pergunta: o inventário inclui `PerguntaDisputa.recusa_motivo`; o programa exige banco `sombra`. Ela não deve ser confundida com escritor ordinário de produção, nem omitida do censo de formas de escrita. (`bin/sombra_regras_pessoais.py:85`; `bin/sombra_mascara_pessoal.py:10`, `:57`, `:62`.)

O levantamento de **`pode_fechar` e decisões concorrentes de fechamento** é:

| Arquivo / fronteira | Definição/derivação | Leitor |
|---|---|---|
| `C/models.py` — DENTRO | juiz `DisputaSupervisao.pode_fechar`1848,1853 | — |
| `S/explicador.py` — DENTRO | bool parcial137; publicação163 | uso local138,139 |
| `C/relatorios_disputa.py` — FORA | projeção concorrente `PRONTA_FECHAR`20 | C6 em18 |
| `S/becos.py` — DENTRO | — | 33,56 |
| `S/competencia_trancada.py` — DENTRO | — | 153,202 |
| `S/desenlace.py` — DENTRO | — | `dict.get`63 |
| `S/disputa_emissao.py` — DENTRO | — | 1278,1963 |
| `S/materializacao.py` — DENTRO | — | 585,1316 |
| `C/reconciliador.py` — FORA | — | 411,611,1170 |
| `P/services/entrada_sincronizada.py` — FORA | — | 58 |
| `H/modal_fio.html` — FORA | — | 435,446 |

As indireções relevantes para interpretar essas tabelas são:

| Forma | Resultado conferido |
|---|---|
| `update(**campos)` | `reabrir_para_correcao` monta resposta/validação nulas e texto vazio em1593; persiste diretamente em1632 ou entrega ao descarimbo em1630. (`S/disputa_emissao.py:1593`, `:1630`, `:1632`.) |
| `update(..., **outros)` | O descarimbo escreve materialização/autor/via e aceita campos adicionais; os chamadores atuais passam resposta/validação. A guarda veta apenas o trio do carimbo em `outros`. (`C/models.py:1566`, `:3255`, `:3266`; `S/disputa_emissao.py:1593`.) |
| `save(update_fields=lista)` | A resposta salva a lista montada em1508; carimbo salva em3152; identidade em3190; descarimbo em3212. A string na lista é mecanismo de persistência, não leitura do valor. (`S/disputa_emissao.py:1508`; `C/models.py:3152`, `:3190`, `:3212`.) |
| `bulk_update` próximo ao domínio | `gravar_lavras_em_lote` escreve somente `veredito_lavrado`, `julgado_em`, `janela_expira_em`; não escreve os seis campos acima. (`C/veredito_lavrado.py:237`, `:250`.) |
| `setattr` genérico | A porta de texto permite título/assunto/descrição; `core.reexibir` altera memória. Não são prova de escrita persistente desses estados. (`S/texto.py:18`, `:25`, `:33`; `app/core/reexibir.py:45`, `:49`.) |
| `F()` | O enumerador inclui `F` como leitor de campo; escrita dos alvos por `F()` não foi identificada nos sítios classificados. Ausência universal por expressões construídas: **[nao verificado]**. |
| SQL | A máscara/selo foram discriminados; o SQL do reconciliador aberto é advisory lock/unlock. SQL dinâmico arbitrário escrevendo os alvos: **[nao verificado]**. (`bin/sombra_selo_pessoal.py:77`; `C/reconciliador.py:1235`, `:1242`.) |
| Signals | `update_fields` é lido como metadado de disparo; o signal emite evento e agenda beco. Não é leitura do conteúdo de cada coluna. (`C/signals.py:227`, `:238`, `:255`, `:257`.) |
| Estado derivado | API serializa `p.estado`; explicador, fio e vigia também o consomem. São leitores indiretos dos campos/juízes já discriminados. (`app/api/views.py:2573`; `S/explicador.py:72`; `S/fio.py:614`; `S/vigia_fio.py:311`.) |

As migrations ficam fora das 128 linhas de runtime:

| Campo | Schema / leitura executável |
|---|---|
| resposta/validação | `C/migrations/0014_perguntadisputa.py:25`, `:26` |
| materialização | `C/migrations/0016_s84_pergunta_chamado_destino.py:23` |
| pendência | `C/migrations/0043_hx_pendencia_admin.py:15` |
| recusa | `C/migrations/0040_a27_recusa_motivo.py:15` |
| via | criação `C/migrations/0033_perguntadisputa_via_resolucao.py:15`; alterações em `0035_via_admin_decidiu.py:15`, `0036_a1_pergunta_celula.py:22`, `0037_a3_estado_honesto.py:31`, `0040_a27_recusa_motivo.py:20`, `0041_b2_via_legado_sem_lastro.py:15`, `0042_hx_via_ausencia_dia.py:15`, `0046_via_celula_absolveu.py:15`, `0048_via_competencia_trancada.py:14`, `0051_decisao_pergunta.py:110` |
| via, leitura de universo | `C/migrations/0053_pergunta_unica_por_familia.py:32` |

Completude contra reflexão, receivers de tipo desconhecido, SQL montado e todos os consumidores transitivos: **[nao verificado]**. O instrumento conserva uma fila de revisão dessas formas; não transforma ausência de literal em prova de ausência.

A matriz seguinte descreve **comportamento da cópia**, não a execução em produção. Os testes foram inspecionados; execução da suíte nesta sessão: **[nao verificado]**. “Sem teste” significa cobertura específica não identificada nesta leitura, não ausência global certificada. **[nao verificado quanto à cobertura global]** [nao verificado]

| ID / porta e entrada | Juiz consultado | Operação | Persistido | Retorno | Prova |
|---|---|---|---|---|---|
| **T01 responder**, entrada admissível | Guardas por tipo/opções; porta de descarimbo | Core de resposta | Texto, `respondida_em`, decisão RESPOSTA; eventual descarimbo | `(True,None)` | `S/disputa_emissao.py:1433`, `:1467`, `:1485`, `:1508`, `:1511`, `:1559`; `T/test_reresposta_invalida_validacao.py:67` |
| **T02 responder novamente**, tinha validação | Mesmas guardas | Invalida assinatura anterior | Nova resposta; validação/autor nulos; histórico conservado | Mesmo contrato | `S/disputa_emissao.py:1470`, `:1480`, `:1511`; `T/test_reresposta_invalida_validacao.py:55` |
| **T03 validar**, sem pergunta/resposta ou já validada; implausível sem confirmação | Filtro PK/DS; carimbos; `motivo_implausivel_validacao` | Recusa antes da escrita | Nada por esses ramos | `nao_encontrada`, `sem_resposta`, `ja_validada`, `implausivel` | `S/validacao.py:385`, `:393`, `:396`, `:404`; sem teste específico |
| **T04 validar hora admissível** | Implausibilidade; materializador individual | Valida **uma P**, antes do laço | Validação/autor e decisão VALIDAÇÃO; efeitos da unidade | `validada` ou `ja_no_espelho` | `S/validacao.py:134`, `:139`, `:435`, `:461`, `:463`, `:515`, `:517`; `T/test_o261_recusa_fala.py:135` |
| **T05 validar**, materialização recusada sem encerramento e com motivo | `_resposta_de_hora`; carimbo nulo; motivo | Devolve validação individual | Limpa validação/autor, mantém resposta, registra DEVOLUÇÃO; pode reabrir DS | `nao_gravou`, `devolvida=True`; repetição pode dar `None` interno | `S/validacao.py:188`, `:199`, `:205`, `:217`, `:473`, `:499`; `T/test_o261_recusa_fala.py:87`, `:148`, `:164` |
| **T06 validar**, recusa já encerrou identidade | Devolução lê materialização | Não desfaz validação | Encerramento permanece | Devolução `False`; resultado final depende dos erros | `S/validacao.py:190`, `:490`; `T/test_o261_recusa_fala.py:190` |
| **T07 materializar lote normal** | Fila por timestamps; unidade; C7 na cauda | Itera **uma P por vez** | Efeitos individuais; eventual fecho da DS | Dict de materializadas, reusos, pendentes, fecho e erros | `S/materializacao.py:1369`, `:1375`, `:1378`, `:1381`; `T/test_o317_passivo_pela_porta.py:188`, `:481` |
| **T08 materializar**, dias divergem sem conferência | `fio.data_do_fato_de` | Recusa nomeada | `recusa_motivo`; nenhuma batida desse ramo | `False` | `S/materializacao.py:1191`, `:1193`, `:1213`, `:1214`; sem teste específico |
| **T09 hora sem texto humano** | Guarda de anuência | Recusa antes de derivar horário | Motivo na P/fio | Ramo `None`; unidade `False` | `S/materializacao.py:829`, `:836`, `:841`, `:1227`; sem teste específico aberto |
| **T10 tipo não conferido** | `entrada_de_marco`, grade/célula | Usa tipo disponível pelo motivo e registra pendência | `pendencia_admin` + motivo; pode prosseguir | Não é recusa obrigatória | `S/materializacao.py:386`, `:395`, `:397`, `:850`, `:851`; sem teste específico |
| **T11 batida já no slot** | `_batida_existente_na_janela` | Reusa; anota origem; reconcilia | Pode alte

[truncado em 20000 de 49203 caracteres]

## 11/11 09:15 · sessao 5d9a4f9c90fa · FIO-E0-V4-E3 · construtor · origem=Code

Cura aplicada somente em `chamados/tests`; os arquivos alterados e novos estão registrados em [e3fix11_execucao.txt:132](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:132). [nao verificado]

O censo dos harnesses ficou assim:

| Executor | Como monta o namespace | Mesmo furo? |
|---|---|---|
| S2/S3 | Dicionário manual; `funcao()` compila somente a função solicitada, sem copiar globais. [helper:14](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e4_apoio.py:14) | Sim; corrigido carregando a constante e o leitor reais. [teste:147](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/test_e3_pos_suite_sem_banco.py:147) | [nao verificado]
| `test_e3_nb_impressao_memoria` | Seleção fixa por AST, já incluindo função e constante. [teste:173](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/test_e3_nb_impressao_memoria.py:173) | Não. | [nao verificado]
| `x1_mutantes --sem-banco` | Reutiliza o harness anterior. [executor:77](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/x1_mutantes.py:77) | Não. |
| `x1_mutantes`, instalação ORM | Executa definições em `modulo.__dict__`, incluindo função e constante. [executor:100](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/x1_mutantes.py:100) | Não pelo código; execução ORM [nao verificado]. |
| `e3_pos_suite_sem_banco` | Invoca o próprio S2/S3; herdava o defeito. [executor:78](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3_pos_suite_sem_banco.py:78) | Corrigido pela mesma alteração. |

Os outros testes que selecionam `_julgar_colab_corpo` por AST apenas inspecionam sua estrutura: [test_o219:217](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/test_o219_versao_da_regra.py:217), [test_e3_isento:112](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/test_e3_isento_na_impressao.py:112). [nao verificado]

Com `cobertura='aprovada'`, **a expectativa original permanece**: `pulados=1`, sem rejulgamento, preservando impressão e veredito anteriores. O caso usa 07/11/2026, dentro do horizonte; porém a cobertura integral exclui o marcador X1. A impressão está deliberadamente cega à cobertura e a chamada usa `apply_=False`. Portanto, nenhuma assertion precisou ser relaxada. [teste:127](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/test_e3_pos_suite_sem_banco.py:127), [cartorio:1229](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/services/cartorio.py:1229), [teste:169](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/test_e3_pos_suite_sem_banco.py:169). A cobertura visível continua tendo seu caso separado de rejulgamento para `concorde`. [memória:414](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/test_e3_nb_impressao_memoria.py:414) [nao verificado]

O diff da cura acrescenta estas linhas, preservando a assertion original:

```diff
+                ns['HORIZONTE_SEM_ADESAO'] = literais('ponto/services/cartorio.py')['HORIZONTE_SEM_ADESAO']
+                funcao('ponto/services/cartorio.py', 'sem_adesao_no_horizonte', ns)
+                self.assertIn('sem_adesao_no_horizonte', ns.keys(),
+                              'harness deve carregar o leitor real do cartorio')
+                self.assertIn('HORIZONTE_SEM_ADESAO', ns.keys(),
+                              'harness deve carregar a declaracao lida pelo leitor')
+                leitor = ns['sem_adesao_no_horizonte']
+                # Corte default 21: 20/10 ainda e comp10; o dia do caso (07/11) e comp11.
+                self.assertFalse(leitor(dt.date(2026, 10, 20)))
+                self.assertTrue(leitor(dia))
...
+                self.assertEqual(out['nunca_bateu'], 0)
+                self.assertEqual(cel.impressao, legado)
+                self.assertEqual(cel.veredito, 'nunca_bateu')
```

Fonte: [teste:147](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/test_e3_pos_suite_sem_banco.py:147). O novo [executor de mutantes:18](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3_namespace_mutantes.py:18) remove cada dependência ou substitui o leitor por constantes, somente em memória. [nao verificado]

Cada link abaixo contém o **comando exato**, RC e resultado:

| Execução | RC | Resultado |
|---|---:|---|
| [Suíte solicitada](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:14) | 0 | **31 OK** | [nao verificado]
| [X1 green](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:19) | 0 | 24 OK |
| [X1 sem_horizonte_x1](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:25) | 1 | Assertion; ERROR=0 |
| [X1 horizonte_na_10](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:32) | 1 | Assertion; ERROR=0 |
| [X1 literal_incondicional](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:39) | 1 | Assertion; ERROR=0 |
| [X1 sem_fonte_no_gancho](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:46) | 1 | Assertion; ERROR=0 |
| [X1 so_acusador](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:53) | 1 | Assertion; ERROR=0 |
| [X1 parcial_tira_marcador](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:60) | 1 | Assertion; ERROR=0 |
| [RED sem_leitor](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:67) | 1 | Dois subcasos por assertion |
| [sem_declaracao](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:74) | 1 | Dois subcasos por assertion |
| [leitor_constante_true](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:81) | 1 | Dois subcasos por assertion |
| [leitor_constante_false](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:88) | 1 | Dois subcasos por assertion |
| [declaracao_na_10](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:95) | 1 | Dois subcasos por assertion |
| [Mutantes S2/S3 existentes](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:102) | 1 cada | Assertion; ERROR=0 |
| [Pós-suite e selos](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:114) | 0 | 7+1+15+35 OK | [nao verificado]
| [Ruff](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:121) | 127 | Indisponível |
| [Diff check](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:129) | 0 | Sem apontamentos |

`status_aprovada_tira_marcador` exige ORM e não possui alvo sem banco. [x1_mutantes:122](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/x1_mutantes.py:122). Certificação Code/opus final: [nao verificado].

## 11/11 09:15 · sessao 0ca26e8461ed · FIO-E0-V4-E4C · censo · origem=Code

**CENSO-ANTES-DA-VAGA — FIO-E0-V4-E4C**

**Campos com retirada lógica prevista:** R = `resposta_colab`, `respondida_em`; V = `validada_em`, `validada_por`; C = `materializada_em`, `materializada_por`, `via_resolucao`; F = `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento`. A retirada depende do ramo: as exceções abaixo continuam lendo projeção. (`app/chamados/models.py:3844`; `app/chamados/models.py:3126`; `app/chamados/models.py:1880`; `app/docs/PROMPTS.md:2034`)

**Exceções:** A03, reabertura do continente; A05, somente assinatura; A06, identidade/duplicata; A17, pendência técnica; A18, recusa técnica; A20, conferência de data. `pendencia_admin`, recusa técnica e `data_conferida_em/por` entram no inventário, mas permanecem fontes nesses ramos. `recusa_motivo` exige separação entre decisão humana, advertência e resultado técnico. (`app/docs/PROMPTS.md:2034`; `app/chamados/services/validacao.py:593`; `app/chamados/services/disputa_emissao.py:1497`; `app/chamados/services/materializacao.py:855`; `app/chamados/services/materializacao.py:1107`)

**Perguntas de lei identificadas: D04 e D05, abaixo.** Não atribuí automaticamente o zero do censo de escritores ao censo de leitores: a lei exige a regra para a nova resposta, não equivalência presumida com o código anterior. (`app/docs/LEIS.md:136`)

Entrega no corpo da resposta; nenhuma gravação de relatório, teste, importação da aplicação, banco ou log produtivo realizada. Evidência persistida dessa execução: **[nao verificado]**. Identidade Git do hash informado, pai da E3 e diff entre eles: **[nao verificado]** — `git log` nesta raiz retornou “not a git repository”. [nao verificado]

O script ao fim foi executado por `python3 -B`, via stdin. O levantamento combina AST Python, busca sintática de templates e candidatos JS; a revisão abaixo elimina homônimos e entradas de formulário. Reflexão irrestrita, SQL montado dinamicamente, serialização implícita e consumidores externos: **[nao verificado]**.

**Autoridades e códigos usados nas tabelas**

| Código | Pergunta e destino existente | Registro |
|---|---|---|
| **T** | Ação exigível, dono e porta: `tarefa_do_dia` | `app/chamados/services/tarefa_do_dia.py:562`; `app/core/juizes.py:291`; `app/docs/JUIZES.md:78` |
| **P** | Pergunta viva: `pergunta_viva` | `app/chamados/juizes.py:862`; `app/core/juizes.py:278` |
| **FI** | Alguma pergunta viva na coleção: `fio_vivo` | `app/chamados/juizes.py:1488`; `app/core/juizes.py:287` |
| **FE** | Disputa pode fechar: `DisputaSupervisao.pode_fechar` | `app/chamados/models.py:1848`; `app/core/juizes.py:279` |
| **E** | Efeito confirmado: juiz `_pergunta_materializada_de_fato`, pela lâmpada vigente consumida por T | `app/chamados/juizes.py:662`; `app/chamados/services/tarefa_do_dia.py:119`; `app/docs/JUIZES.md:81` |
| **TR** | Dia trancado: `dia_da_pergunta_trancado` / `dias_trancados` | `app/chamados/juizes.py:974`; `app/chamados/juizes.py:1004`; `app/core/juizes.py:292` |
| **D01** | Texto, instante, assinatura e motivo da decisão: sequência DP; juiz específico de apresentação não registrado | `app/chamados/models.py:3840`; `app/core/juizes.py:272` |
| **D02** | Estado histórico do continente: falta autoridade factual equivalente demonstrada; preservar A03 | `app/chamados/models.py:2126`; `app/docs/PROMPTS.md:2034` |
| **D03** | História/procedência do processamento: DP e efeitos, sem confundir carimbo com efeito | `app/chamados/models.py:3840`; `app/chamados/services/tarefa_do_dia.py:141` |
| **D04** | Indicadores de atividade: decisão sem juiz específico confirmado | `app/chamados/services/verbos.py:239`; `app/chamados/services/painel.py:268`; `app/core/juizes.py:272` |
| **D05** | Rótulos de fase publicados pela API/tela: equivalência com T não estabelecida | `app/chamados/models.py:3034`; `app/api/views.py:2555` |
| **D06** | Invalidação de cache/ETag: dependência técnica de decisões **e efeitos** | `app/chamados/services/painel.py:58`; `app/chamados/services/painel.py:67`; `app/chamados/veredito_lavrado.py:151` |

Apontar para um juiz existente ainda não elimina a dependência: P recebe R/V/C pela projeção temporal; FE usa `respostas_sem_veredito`; E ainda consulta C/via; T consulta projeção para detectar passivo e conserva via de identidade. Esses corpos também precisam entrar na migração, respeitando as exceções. (`app/chamados/juizes.py:1369`; `app/chamados/juizes.py:1384`; `app/chamados/models.py:1853`; `app/chamados/juizes.py:679`; `app/chamados/services/tarefa_do_dia.py:398`; `app/chamados/services/tarefa_do_dia.py:230`)

**Inventário — modelos e juízes**

Nas tabelas, `def → leitura` separa a definição da ocorrência consumida. Os caminhos abreviados são: **M** = `app/chamados/models.py`; **J** = `app/chamados/juizes.py`; **S/** = `app/chamados/services/`. Cada função nomeada constitui uma unidade do censo.

| Leitor | Campos / resposta atual | Destino |
|---|---|---|
| M:128 `ChamadoColaborador.transicionar` → M:190 | F; disputas relacionadas à transição | T/FE; continente D02 |
| M:201 `_gravar_estado` → M:310 | V; perguntas atingidas pela reconciliação | T + D01 |
| M:336 `_ancoradas` → M:344 | V; perguntas ancoradas pertinentes | P/T |
| M:1468 `_universo_da_ressuscitacao` → M:1484 | C/R/via; universo que pode voltar | P/E; identidade A06 |
| M:1512 `ressuscitar_perguntas` → M:1563, M:1564, M:1574 | R/V/F; conservação e seleção da volta | D01/D03/P; F=D02 |
| M:1823 `DisputaSupervisao.aberta` → M:1824 | F; continente aberto | D02; dependência A03 |
| M:1826 `respostas_sem_veredito` → M:1843 | R/V/C; impedimento de fechamento | FE, cujo corpo precisa migrar |
| M:1848 `pode_fechar` → M:1853 | Leitura indireta de R/V/C | Permanece FE, sem segundo juiz |
| M:1855 `fechar` → M:1879, M:1920 | F/R/C/via; idempotência e quitação | FE/E/D01; história D02/D03 |
| M:1971 `remover_motivo` → M:2031, M:2055 | F/R/via; retratação | DP `RETRATACAO`; A06 na identidade |
| M:2126 `reabrir_admin` → M:2138 | F; necessidade da reabertura | **A03 permanece** |
| M:2163 `reabrir_sistema` → M:2167 | F; religar continente | T para exigibilidade; D02 para continente |
| M:2182 `reabrir_por_colab` → M:2224 | F; gate de correção | T/D01; gesto do continente D02/A03 |
| M:2250 `limpar_resolucao_residual` → M:2262 | F/resolução; limpeza residual | D02/D03; retirada coordenada com E4d |
| M:2276 `DisputaSupervisao.__str__` → M:2277 | `aberta` indireta | D02 |
| M:2629 `PerguntaDisputa.trava_na_ordem` → M:2673 | F em Q/`values_list`; universo das travas | D02; não substituir ordem de trava por T |
| M:2697 `do_fato` → M:2724 | via; identidade | **A06 permanece** |
| M:2746 `q_dos_fatos` → M:2755 | via em Q; identidade | **A06 permanece** |
| M:2825 `_nascer` → M:3019 | R/V indiretos na conservação legada | D01/D03; cruza E4b |
| M:3034 `estado` → M:3041 | R/V/C; fase exibida | **D05** |
| M:3050 `fechar_por_admin` → M:3055 | C/via; idempotência | E/DP; identidade A06 |
| M:3103 `carimbar` → M:3126 | C/R/V/autores; foto e assinaturas | E/D03; **A05 somente assinatura** |
| M:3168 `carimbar_identidade` → M:3182 | C/autor; preservar carimbo | **A06 permanece** |
| M:3193 `fora_do_fato` → M:3201 | via; terminalidade da identidade | **A06 permanece** |
| M:3203 `descarimbar` → M:3206 | `fora_do_fato` indireto | **A06 no gate** |
| M:3252 `descarimbar_em_lote` → M:3265 | via; excluir identidade terminal | **A06 no gate** |
| M:3268 `encerrar_por_fato_no_espelho` → M:3273 | via/`fora_do_fato`; idempotência | E; identidade A06 |
| M:3279 `encerrar_fantasma` → M:3284 | C/via; idempotência | E/D03 |
| M:3289 `reabrir_fantasma` → M:3296 | C/via; reversibilidade | E/D03 |
| M:3301 `reabrir_por_resposta` → M:3313 | C/via; reversibilidade | DP/E |
| M:3317 `reabrir_por_competencia` → M:3320 | via; reversibilidade | TR/DP |
| M:3326 `q_vivas` → M:3348, M:3359 | R/V/C/via em Q; pré-filtro | P, conservando superset; A06 |
| M:3497 `respondida` → M:3498 | R; presença do carimbo | D01; ação por T |
| M:3519 `resposta_display` → M:3521 | R; conteúdo discriminado | D01; preservar tipo/opções |
| M:3914 `DecisaoPergunta.linhas_da_projecao_sem_linha` → M:3924 | R/V/autoria; detectar legado sem ato | **Conservação E4b**, não substituir por “nenhuma decisão” |

As condições dos índices em `PerguntaDisputa.Meta` também dependem de via; são dependências de schema, não funções adicionais no total de leitores. Permanecem no ramo A06. (`app/chamados/models.py:3485`; `app/chamados/models.py:3491`; `app/docs/PROMPTS.md:2034`)

| Leitor | Campos / pergunta respondida | Destino |
|---|---|---|
| J:282 `_retratada_com_trilha` → J:288 | C; ordem temporal da retratação | DP/D03; A06 |
| J:341 `_slot_esperado` → J:348, J:360 | R; slot esperado | D01, mantendo interpretação do juiz |
| J:662 `_pergunta_materializada_de_fato` → J:679, J:707 | C/via; efeito confirmado | Permanece E; retirar C derivado, preservar exceções |
| J:821 `_chave_memo_pv` → J:852 | R/V/C/via; chave da memoização | D06; A06 na identidade |
| J:1314 `_resposta_concorda_com_a_luz` → J:1340 | R; concordância com o fato | D01; mantém julgamento existente |
| J:1362 `projecao_temporal_da_pergunta` → J:1369 | R/V/C/via; valores no horizonte | DP por `ato_em` + E; A06 |
| J:1383 `_pergunta_viva_calc` → J:1384 | Consome a projeção temporal | P, migrando seu insumo |
| J:1476 `pergunta_sem_lastro_ou_viva` → J:1480 | C; proteção de resposta regularizada | P/E |
| J:1509 `respondida_sem_texto` → J:1512 | R; carimbo sem conteúdo | D01; conservação D03 |
| J:1655 `perguntas_corrigiveis` → J:1657 | R; universo corrigível | T/D01 |
| J:1662 `perguntas_respondidas_a_validar` → J:1664 | R/`q_vivas`; fila de validação | T/P/D01 |
| J:1678 `declarou_hora` → J:1693 | R; declaração interpretável | D01 |
| J:1830 `delta_da_resposta` → J:1840 | R; distância do marco | D01; preservar régua existente |
| J:1936 `classificar_resposta_validacao` → J:1944 | R; classe da aprovação | D01; mantém juiz existente |
| J:1957 `motivo_implausivel_validacao` → J:1966 | R; motivo de implausibilidade | D01; mantém juiz existente |
| J:1980 `irmas_respondidas_sem_consumo` → J:2013 | R/V/C; declarações ainda não consumidas | DP/E |
| J:2371 `disputa_viva_do_chamado` → J:2379 | F; relação chamado/continente | D02; ação por T/FI |

**Inventário — serviços internos**

| Arquivo e leitor | Leitura / o que responde | Destino |
|---|---|---|
| S/`tarefa_do_dia.py:164` `_tarefa_pura` → :230 | via; exclusão da identidade | **A06 permanece** |
| S/`tarefa_do_dia.py:361` `CargaDaTarefa.__init__` → :398 | R/V indiretos; passivo sem decisão | D01/D03; **gate E4b** |
| S/`tarefa_do_dia.py:449` `_carregar_vida` → :491 | P indireto; exposição administrativa | P/T, incluindo migração do insumo de P |
| S/`pergunta_no_app.py:45` `perguntas_do_app` → :50 | `q_vivas`; coleção ampla do app | P |
| S/`pergunta_no_app.py:61` `consulta_do_app` → :71 | R/F; pendentes versus histórico | T/D01; D02 no histórico do continente |
| S/`cobranca.py:221` `_calados` → :229 | R/F/`q_vivas`; quem ainda não respondeu | T/P/D01 |
| S/`janela_colab.py:49` `chamados_visiveis_colab` → :63 | `q_vivas`; visibilidade | P/T |
| S/`declaracao_texto.py:216` `_resposta_validada` → :227 | V; preservar aprovação | D01 |
| S/`declaracao_texto.py:306` `declarar_hora_colab` → :417 | R; preservar voz/instante | D01 |
| S/`declaracao_texto.py:641` `aplicar_declaracao` → :687 | R; resposta já entregue | D01/T |
| S/`validacao.py:127` `_carimbar_validacao` → :139 | R/V; conteúdo e instante do ato | D01; leitura dentro de escritor existente |
| S/`validacao.py:144` `_foto_da_validacao` → :147 | V/autor/recusa/F; fotografia | D01/D03/D02; A18 técnico |
| S/`validacao.py:163` `devolver_validacao_recusada` → :188 | R/V/C/recusa/F; devolução | DP `DEVOLUCAO`/E; A18 |
| S/`validacao.py:234` `_e_aceite_de_folga` → :236 | R; aceite de folga | D01 |
| S/`validacao.py:239` `_aceitar_a_folga` → :345 | V/autor por `refresh_from_db`; restauração | D01/D03 |
| S/`validacao.py:373` `validar_pergunta` → :393, :461 | R/V/C; guardas e resultado | T/DP/E |
| S/`validacao.py:533` `recusar_pergunta` → :573, :580 | R/V/recusa; recusabilidade e foto | DP `RECUSA`; não inferir ato de string técnica |
| S/`validacao.py:625` `conferir_data_do_fato` → :662 | conferência; idempotência | **A20 permanece** |
| S/`validacao.py:695` `validar_lote_classe_a` → :732 | V; evitar aprovação repetida | T/D01 |
| S/`validacao.py:845` `classe_do_lote` → :851 | R; classe do lote | D01 |
| S/`validacao.py:912` `fila_validar_classificada` → :924 | R; entrada na fila | T/D01 |
| S/`materializacao.py:245` `_tipo_pelo_marco` → :279 | R; tipo do plantio | D01 |
| S/`materializacao.py:332` `_ts_tipo_do_plantio` → :358 | R; instante e escape | D01; mantém ajuste temporal existente |
| S/`materializacao.py:430` `estado_aplicacao_pergunta` → :437 | R/C/via; aplicação/idempotência | DP/E; A06/A18 por ramo |
| S/`materializacao.py:467` `_declarar_ausencia_de_pergunta` → :473 | R; ausência declarada | DP + efeito Ausência |
| S/`materializacao.py:501` `_materializar_ausencia_de_resposta` → :538, :570, :590 | R/C/F; absorção | DP `ABSORCAO`/E/FE; D02 |
| S/`materializacao.py:608` `perguntas_absorvidas_por` → :630 | R/C/via; vínculo com ausência | DP `ABSORCAO` + Ausência |
| S/`materializacao.py:644` `devolver_absorvidas_da_ausencia_rejeitada` → :673 | R/V/C/autores; fotografia da volta | DP/D03 + estado da Ausência |
| S/`materializacao.py:789` `_materializar_resposta_hora` → :834 | R; declaração aplicada | D01; **A17 no resultado técnico** | [nao verificado]
| S/`materializacao.py:983` `_materializar_resposta_data_hora` → :1000 | R; interpretação textual | D01 |
| S/`materializacao.py:1088` `_carimbar_recusa` → :1094 | C/via/recusa; idempotência técnica | **A18/A06 permanecem** |
| S/`materializacao.py:1163` `_materializar_uma` → :1198 | conferência; autorização diante de dois dias | **A20 permanece** |
| S/`materializacao.py:1307` `_fechar_disputa_se_quitada` → :1321 | F/resolução; fechamento existente | FE; história D02 |
| S/`materializacao.py:1343` `materializar_perguntas_validadas_da_disputa` → :1375 | R/V/C; fila de execução | T/DP/E |
| S/`materializacao.py:1395` `materializar_pendentes_aprovadas` → :1424 | C; seleção pendente | T/E |
| S/`disputa_emissao.py:34` `colaborador_em_disputa` → :41 | F; continente aberto | D02; exigibilidade por T |
| S/`disputa_emissao.py:166` `_derivar_chamado_destino` → :228 | C; destino pertinente | P/E |
| S/`disputa_emissao.py:300` `gerar_perguntas_disputa` → :328 | via; dedup | **A06 permanece** |
| S/`disputa_emissao.py:474` `_abrir_ou_agregar_disputa` → :532 | F; continente reutilizável | D02/A03 |
| S/`disputa_emissao.py:747` `motivo_fora_da_fabrica` → :762 | C; causa de retenção | P/E |
| S/`disputa_emissao.py:945` `fecho_do_repasse` → :978 | F em Q; universo costurado | D02 |
| S/`disputa_emissao.py:1027` `criar_disputa_a_partir_de_chamados_orfaos` → :1070, :1097 | C/F; reuso | E/P/FE; D02 |
| S/`disputa_emissao.py:1302` `criar_disputa_ancora_de_chamado` → :1326 | C/F; âncora reutilizável | E/P; D02 |
| S/`disputa_emissao.py:1425` `responder_pergunta_disputa_core` → :1470, :1522 | V/autor/F; invalidação anterior e contexto | DP/D03; D02 |
| S/`disputa_emissao.py:1562` `reabrir_para_correcao` → :1619 | R/V/C/autores em `values_list`; foto anterior | DP/D03 |
| S/`disputa_emissao.py:1638` `responder_automaticamente` → :1647 | R; preservar texto existente | D01 |
| S/`disputa_emissao.py:1661` `carimbar_resposta_pela_trilha` → :1670 | R; texto sem instante | D01/D03, instante testemunhado |
| S/`disputa_emissao.py:1690` `recobrar_questionario` → :1713 | R; resposta já entregue | T/D01 |
| S/`disputa_emissao.py:1801` `_resolver_perguntas_por_chamado_resolvido` → :1825, :1856, :1927 | R/V/C/F/via; consequências do chamado | DP/P/E/FE; A06 e D02 por ramo |
| S/`fio.py:120` `_disputa_do_chamado` → :131 | F; continente apresentado | Autoridade já citada em `app/docs/JUIZES.md:40`; D02 no insumo |
| S/`fio.py:163` `montar_chamados_fio` → :187, :224, :340 | F/R/`aberta`/`q_vivas`; apresentação | P/T/D01; D02 e D05 |
| S/`fio.py:485` `processar_post_fio` → :600, :633, :660 | F/R; ramo do POST | T/D01; D02 no gesto do continente |
| S/`fio.py:1084` `reuso_sem_rechave` → :1100 | via; reuso | **A06 permanece** |
| S/`fio.py:1152` `sem_dia_na_saida` → :1167 | via; invariante de identidade | **A06 permanece** |
| S/`fio.py:1225` `canonica_do_grupo` → :1232 | R; escolha canônica | D01 + **A06** |
| S/`fio.py:1236` `merito_perdedor` → :1243 | R/via; mérito da canônica | D01 + **A06** |
| S/`fio.py:1345` `retratar_duplicata` → :1350 | via; duplicata já retratada | **A06 permanece** |
| S/`fio.py:1422` `_no_fato` → :1433 | via; universo de identidade | **A06 permanece** |
| S/`fio.py:1840` `fecho_cruza_dias` → :1853 | via; fecho da identidade | **A06 permanece** |
| S/`fio.py:1878` `perguntas_sobre_batida_retratada` → :1889 | via; diagnóstico | **A06 permanece** |
| S/`fio.py:1905` `universo_do_grupo` → :1911 | via; universo | **A06 permanece** |
| S/`fio.py:1923` `grupos_de_colisao` → :1956 | via; colisões | **A06 permanece** |
| S/`fabrica_da_celula.py:92` `motivos_a_perguntar` → :149 | via; dedup | **A06 permanece** |
| S/`fabrica_da_celula.py:160` `perguntar` → :171 | C; pergunta reaproveitável | P/E |
| S/`explicador.py:25` `_explicar_validada` → :32 | R; explicação | D01/E |
| S/`explicador.py:68` `explicar_pergunta` → :72, :85, :90, :95 | fase/R/pendência/recusa/via | T/E/D05; **A17/A18/A06** |
| S/`explicador.py:108` `por_que_nao_fecha` → :137 | `fio_vivo`; “pode fechar” | **FE**, não apenas FI |
| S/`raiox.py:51` `creditos_perguntas` → :135 | C/R em SQL/tuplas; crédito do painel | P/E/T/D01 |
| S/`raiox.py:153` `pergunta_sem_acao_possivel` → :175 | C; clique morto | E/T |
| S/`relavra_via.py:106` `relavrar` → :118 | via; necessidade da relavra | E/L; A06 fora da derivação |
| S/`relavra_via.py:142` `relavrar_dia` → :155 | via; seleção da relavra | E/L |
| S/`relavra_via.py:171` `q_carimbo_desfazivel` → :181 | C/via/F; seleção reversível | P/E; A06 e D02 |
| S/`relavra_via.py:187` `desfazer_carimbo_sem_lastro` → :244 | C/via/F; diagnóstico/foto | P/E/D03; A06 |
| S/`passivo_validado.py:131` `_balde_da_recusa` → :140 | via; classificação do passivo | E/T; **A18/A06** |
| S/`passivo_validado.py:474` `_plantar_o_dia` → :515 | via; aplicação do passivo | E/T; **A18/A06** |
| S/`historico.py:69` `_eventos_dos_chamados` → :98, :105 | `getattr(p,campo)` de R/V/C; linha do tempo | **D03**, DP e efeitos, não só último ato |
| S/`painel.py:50` `marcador_do_placar` → :67 | `Max(R/C/V)`; invalidação | **D06** |
| S/`painel.py:198` `montar_contexto_painel` → :262 | autor de V; indicador | **D04** |
| S/`painel.py:268` `_jan_val` → :270 | V por janela; atividade | **D04** |
| S/`verbos.py:218` `placar_equipe` → :239 | V/autor; ações por pessoa | **D04** |
| S/`verbos.py:253` `placar_aceite` → :311, :318 | R por `values`/dict; aceite após cobrança | **D04**, conservando definição escrita de aceite |
| S/`fila_por_causa.py:214` `_gaveta_de` → :225 | F e respostas sem veredito; gaveta | T/FE; D02 |
| S/`acoes_disputa.py:90` `acao_reabrir_disputa` → :95 | R; perguntas alcançadas | T/D01 |
| S/`acoes_disputa.py:118` `acao_regularizar_disputa` → :130 | R; regularização | T/D01/FE |
| S/`acoes_disputa.py:235` `acao_reabrir_disputa_admin` → :255 | F; escolher continente | **A03/D02** |
| S/`acoes_disputa.py:302` `acao_corrigir_resposta` → :345 | V/F/R; correção | T/D01; D02 |
| S/`acoes_chamado.py:327` `montar_contexto_detalhe` → :336 | F; detalhe | D02/T |
| S/`becos.py:25` `classificar` → :27 | F/R; resposta presa em fechada | T/FE/D01; D02 |
| S/`becos.py:36` `fechar_beco` → :56 | F e `pode_fechar` | FE; D02 |
| S/`competencia_trancada.py:65` `medir` → :113, :151 | F e respostas sem veredito | TR/FE; D02 |
| S/`competencia_trancada.py:163` `encerrar` → :196 | F; disputas alcançadas | TR/FE; D02 |
| S/`competencia_trancada.py:235` `reabrir` → :251 | via; reversibilidade | TR/DP; A06 por ram

[truncado em 20000 de 48601 caracteres]

## 11/11 09:08 · sessao f1ecbbc31623 · FIO-E0-V4-E4B · construtor · origem=Code

1. **Quem mascara a remoção é `_Desfaz`, provocando rollback do `atomic` inteiro** quando todos os dias são recusados: [backfill_fio_morto_e4b.py:349](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/management/commands/backfill_fio_morto_e4b.py:349).

2. **O lote misto precisa do rollback individual.** A recusa segue para o próximo dia; havendo um aceito, não ocorre `_Desfaz`: [backfill_fio_morto_e4b.py:327](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/management/commands/backfill_fio_morto_e4b.py:327). Acrescentei o caso ORM exigindo vizinho persistido, P90/D90/Ch12 restaurados, foto integral restaurada entre tentativas e desaparecimento dos nascimentos recusados: [test_e4b_c4_orm.py:404](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm.py:404). Cascata natural em produção: [nao verificado].

3. **Não se aplica substituir um rollback redundante.** Mantive o enxerto `sem_rollback` e apontei-o ao novo caso: [e4b_c4_verificar.py:37](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c4_verificar.py:37). A morte no ORM continua [nao verificado].

Entrega: [diff completo](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c4v10.patch:1) e [comandos/resultados](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c4v10_resultados.txt:1).

| Verificação | rc | Resultado |
|---|---:|---|
| Memória | 0 | [88 OK](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c4v10_resultados.txt:6) |
| REDs registrados | 0 | [40 OK](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c4v10_resultados.txt:10) |
| `--selos` | 0 | [40 OK](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c4v10_resultados.txt:14) |
| `--orm-preflight` | 0 | [8 enxertos válidos](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c4v10_resultados.txt:18) |
| `sem_rollback`, memória | 1 | [Uma falha na assertion-alvo](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c4v10_resultados.txt:28) |
| `rollback_roteiro_antigo` | 1 | [Assertion prevista](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c4v10_resultados.txt:38) |
| `rollback_falha_aceita` | 1 | [Cinco subtests falham, sem erros](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c4v10_resultados.txt:44) |

Para a sessão, os comandos exatos de **GREEN isolado**, **mutante isolado** e **roteiro ORM completo** estão no [registro:53](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c4v10_resultados.txt:53). Certificação ORM e opus final: [nao verificado].

## 11/11 09:08 · sessao 4aeaf4c941a5 · - · - · origem=Ronald (tmux)

Censo externo executado por AST via `python3 -B` em stdin; sem import Django, acesso ao banco ou suíte. Prova por arquivo da execução do instrumento: [nao verificado]. O recorte abaixo exclui `app/chamados/models.py`, `app/chamados/services/`, testes e migrations; atributos, argumentos ORM e strings de `getattr`/`values`/`only` foram capturados. Não prova completude contra reflexão/SQL construído. [nao verificado]

**Contagens externas do AST**, por `(arquivo, linha, forma)`:

| Campo | Ocorrências | Linhas distintas |
|---|---:|---:|
| `respondida_em` | 35 | 33 |
| `validada_em` | 19 | 18 |
| `materializada_em` | 42 | 42 |
| `via_resolucao` | 38 | 38 |
| `pendencia_admin` | 1 | 1 |
| `recusa_motivo` | 4 | 4 |
| `pode_fechar` | 4 | 4 |

Essas contagens excluem os complementos dinâmicos/templates abaixo e precisam ser apresentadas com essa unidade; sua execução não tem arquivo de log por restrição de só leitura. [nao verificado por arquivo:linha]

**Leitores externos — referências completas do estrato direto.** `R` inclui atributo/getattr, `F` seleção ORM e `V` projeção/carregamento. Cada número pertence ao arquivo da primeira coluna.

| Arquivo | Campo e linhas |
|---|---|
| `app/api/views.py` | `respondida_em` R:1172,1184,2568,2582,2625,2642 |
| `app/api/views_mensageria.py` | `respondida_em` R:569,570 |
| `app/chamados/juizes.py` | `respondida_em` R:829,1341,1449; F:1594,1601,1949; ordenação:1601 |
| `app/chamados/juizes.py` | `validada_em` R:829,1325; F:1949 |
| `app/chamados/juizes.py` | `materializada_em` R:288,662,689,831,1336,1362,1368,1399,1417; F:1949 |
| `app/chamados/juizes.py` | `via_resolucao` R:662,674,830,1336,1362 |
| `app/chamados/management/commands/auditar_disputas.py` | `materializada_em` F:69,148 |
| `app/chamados/management/commands/auditar_invariantes_chamados.py` | `respondida_em` R:177,592; F:1105,1572,1602 |
| Mesmo arquivo | `validada_em` R:205; F:1075,1572,1602,1765,1778 |
| Mesmo arquivo | `materializada_em` V:469; R:538,905; F:587,955,1075,1156,1572,1602,1765 |
| Mesmo arquivo | `via_resolucao` R:202,206,433,542,1710; F:955 |
| Mesmo arquivo | `recusa_motivo` F:535,1075,1112; `pendencia_admin` exclude:1112 |
| `app/chamados/management/commands/backfill_decisoes_pergunta.py` | `respondida_em` Q/only:56; `validada_em` Q/only:56,F:100; `materializada_em` F:100,R:325; `via_resolucao` F:100,R:314,330,V:398,504 |
| `app/chamados/management/commands/curar_perguntas_em_limbo.py` | `respondida_em` F:46,69 |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py` | `materializada_em`, `validada_em`, `via_resolucao` F:31; `via_resolucao` R:42,49,66,76 |
| `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py` | `via_resolucao` R:70,71,72; `materializada_em` R:147 |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py` | `validada_em` F:67; `materializada_em` F:67,R:88,91,129,142; `via_resolucao` F:67,R:129,142,160 |
| `app/chamados/management/commands/relavrar_via_i6.py` | `via_resolucao` R:58,59,62,66,68,88 |
| `app/chamados/reconciliador.py` | `materializada_em` R:366,1144,1154,F:627,1048,1500; `validada_em` F:627,1048; `via_resolucao` R:1143,1154; `pode_fechar()` chamadas:411,611,1170 |
| `app/chamados/relatorios_disputa.py` | `respondida_em` R:21 |
| `app/chamados/veredito_lavrado.py` | `materializada_em` F:202; `respondida_em` F:206 |
| `app/chamados/views.py` | `materializada_em` R:1257; `respondida_em` R:1280,F:1416 |
| `app/chamados/views_cobrar.py` | `respondida_em` R:169,F:175 |
| `app/ponto/management/commands/reconciliar_fantasmas.py` | `materializada_em` F:211 |
| `app/ponto/management/commands/reconciliar_grade.py` | `respondida_em` F:460 |
| `app/ponto/services/cartorio.py` | `via_resolucao` R:1610 |
| `app/ponto/services/entrada_sincronizada.py` | `pode_fechar()` chamada:58 |
| `app/ponto/services/flip_auto.py` | `via_resolucao`, `validada_em` F:128; `recusa_motivo` R:132 |
| `app/ponto/services/procedencia_batida.py` | `validada_em`, `materializada_em` R:74 |
| `app/ponto/services/veredito_celula.py` | `materializada_em` R:195 |
| `app/ponto/supra_juiz.py` | `respondida_em` R:545,548 |
| `bin/censo_folha.py` | `respondida_em` F:28,29; `validada_em` F:29 |

As linhas ORM indicam o início da chamada AST; argumentos multilinha podem estar nas linhas imediatamente seguintes. Exemplo conferido: `encerrar_perguntas_com_fato.py:31` inicia a seleção, com os filtros em :32 e :33.

**Escritor externo dinâmico obrigatório.**

| Campo | Escritor | Leitores/seleção | Escopo |
|---|---|---|---|
| `P.recusa_motivo` | `setattr(o,c,novo)` em `bin/sombra_mascara_pessoal.py:57`, persistido por `bulk_update` em :62 e :64 | `.only('pk',*campos)` :51; `getattr` :54; comparação :56 | `TEXTO` registra explicitamente `PerguntaDisputa.recusa_motivo` em `bin/sombra_regras_pessoais.py:85`; script exige banco `sombra` em `bin/sombra_mascara_pessoal.py:10` |
| `P.recusa_motivo` | — | SQL `coalesce`/regex em `bin/sombra_selo_pessoal.py:77` | Itera o mesmo inventário `R.TEXTO` em :74 |

Não encontrei atribuição literal externa a esses sete nomes no recorte AST. Ausência universal de escrita por `F()`, kwargs/reflexão/SQL construído: [nao verificado].

**Templates, separados do AST Python.**

| Arquivo | Campo | Linhas |
|---|---|---|
| `app/templates/chamados/partials/modal_fio.html` | `respondida_em` | 185,217,220 |
| Mesmo arquivo | `pendencia_admin` / display | 222,225 |
| Mesmo arquivo | `recusa_motivo` | 226 |
| Mesmo arquivo | `validada_em` | 228 |
| Mesmo arquivo | `chamado.explicacao.pode_fechar` | 435,446 |
| `app/templates/chamados/partials/_card_atendimento.html` | `respondida_em` | 94,100,137,155,384,385 |
| Mesmo arquivo | `pendencia_admin` | 183 |
| Mesmo arquivo | `via_resolucao`, display e fallback `recusa_motivo` | 186 |
| `app/templates/chamados/partials/_pergunta_card.html` | `validada_em` | 20,23,42 |
| Mesmo arquivo | `respondida_em` | 24,29,32 |

Inputs `name="recusa_motivo"` são ingresso de resposta, não leitura persistida: `modal_fio.html:61`, :295 e `_pergunta_card.html:114`.

**Migrations, separadas.**

| Campo | Definição/alteração |
|---|---|
| `respondida_em` | `app/chamados/migrations/0014_perguntadisputa.py:25` |
| `validada_em` | Mesmo arquivo :26 |
| `materializada_em` | `app/chamados/migrations/0016_s84_pergunta_chamado_destino.py:23` |
| `pendencia_admin` | `app/chamados/migrations/0043_hx_pendencia_admin.py:15` |
| `recusa_motivo` | `app/chamados/migrations/0040_a27_recusa_motivo.py:15` |
| `via_resolucao` | Criação `0033_perguntadisputa_via_resolucao.py:15`; alterações `0035_via_admin_decidiu.py:15`, `0036_a1_pergunta_celula.py:22`, `0037_a3_estado_honesto.py:31`, `0040_a27_recusa_motivo.py:20`, `0041_b2_via_legado_sem_lastro.py:15`, `0042_hx_via_ausencia_dia.py:15`, `0046_via_celula_absolveu.py:15`, `0048_via_competencia_trancada.py:14`, `0051_decisao_pergunta.py:110` |
| `via_resolucao` leitura executável | `app/chamados/migrations/0053_pergunta_unica_por_familia.py:32`, `Q` do universo do fato |

`materializada_em` de `RegularizacaoExterna` na migration `0021_regularizacaoexterna_regularizacaoitembatida_and_more.py:26` é homônimo e fica fora do censo de PerguntaDisputa.

**Divergências adicionais para a consolidação.**

1. **Relatório também re-julga C7.** `relatorios_disputa.classificar` usa C6 e devolve `PRONTA_FECHAR` quando não há vivas, sem consultar `respostas_sem_veredito`; C7 acrescenta precisamente essa guarda. (`app/chamados/relatorios_disputa.py:18`, :20; `app/chamados/models.py:1843`, :1853.) Deve compor a mesma decisão do explicador, não apenas a tela.

2. **Recusa é texto e insumo operacional.** `fila_flip_recusas` interpreta regex de `recusa_motivo` para extrair PK de Batida. A máscara transforma esse texto na sombra. Isso não demonstra defeito em produção, mas estabelece dois significados/leitores diferentes do campo. (`app/ponto/services/flip_auto.py:132`, :135; `bin/sombra_mascara_pessoal.py:57`, :62.)

3. **Validação e materialização se confundem na data apresentada de procedência.** `procedencia` usa `validada_em or materializada_em` e `validada_por or materializada_por`; seu resultado é procedência da marcação, não veredito de fechamento. (`app/ponto/services/procedencia_batida.py:72`, :74.)

4. **Respondida como proteção de conversa.** `pode_executar_csf` recusa a retratação por qualquer `respondida_em` preenchido nas perguntas da DS ou resolvedoras. Esse leitor responde “há conversa que exige decisão humana”, enquanto API/template apresentam resposta recebida e os seletores admin classificam fila. (`app/ponto/supra_juiz.py:539`, :545, :548; `app/api/views.py:2568`; `app/chamados/views_cobrar.py:169`.)

5. **`pode_fechar` não é coluna.** É método derivado da DS, definido em `models.py:1848`, retorna a conjunção de ausência de respostas sem veredito e ausência de fio vivo em :1853. O homônimo de apresentação vem do dict `explicacao` consumido pelo template em :435/:446; não confundir sua “escrita” com persistência da DS.

## 11/11 09:07 · sessao 5a294bb62c7c · - · - · origem=Ronald (tmux)

Use `E=app/chamados/services/disputa_emissao.py`, `V=app/chamados/services/validacao.py`, `MT=app/chamados/services/materializacao.py`, `T=app/chamados/tests/`. Provas abaixo foram lidas; execução dos testes [nao verificado]. [nao verificado]

| Porta / entrada | Juiz consultado | Operação / persistido | Retorno | Prova |
|---|---|---|---|---|
| Responder; resposta vazia, escolha inválida, hora fora formato ou `OUTRO_MOTIVO` em hora | Guardas locais por `tipo_resposta`/opções | Recusa antes de salvar. `E:1433`, `:1435`, `:1445`, `:1448`, `:1451`, `:1458` | `(False, mensagem)`; contrato `E:1428` | sem teste específico aberto | [nao verificado]
| Responder; primeira resposta admissível | Validação local do formato; `reabrir_por_resposta` para carimbo sem lastro | `resposta_colab`, `respondida_em`; `DecisaoPergunta.RESPOSTA` no mesmo atomic; eventual descarimbo pela porta. `E:1467`, `:1468`, `:1485`, `:1503`, `:1508`, `:1511` | `(ok, erro)`; `E:1428` | `T/test_reresposta_invalida_validacao.py:67` |
| Responder novamente; tinha validação | Mesmas guardas | Limpa `validada_em/por`, preserva histórico anterior na sequência; resposta nova recebe instante próprio. `E:1470`, `:1480`, `:1511` | Mesmo contrato | `T/test_reresposta_invalida_validacao.py:55` |
| Responder escape `FOLGA_CONTESTA` | Roteamento explícito | Grava `recusa_motivo` com a resposta; exige decisão admin, não planta nessa etapa. `E:1486`, `:1497`, `:1508` | Mesmo contrato | sem teste específico aberto | [nao verificado]
| Responder `ESCAPES_QUE_VIRAM_AUSENCIA` | Tabela do materializador / `estado_inicial` conforme comentário | Após resposta salva, chama `_materializar_ausencia_de_resposta`. `E:1538`, `:1541` | Mesmo contrato | sem teste específico aberto | [nao verificado]
| Validar; PK inválido, sem resposta, já validada | Filtro PK/DS; carimbos individuais | Sem gravação. `V:385`, `:389`, `:393`, `:396` | `nao_encontrada`, `sem_resposta`, `ja_validada`. `V:391`, `:394`, `:397` | sem teste comportamental específico aberto | [nao verificado]
| Validar; implausível sem confirmação | `motivo_implausivel_validacao` | Sem gravação. `V:404`, `:405` | `implausivel`. `V:406` | sem teste específico aberto | [nao verificado]
| Validar resposta hora admissível | Implausibilidade; unidade materializadora; depois C7 na cauda | Sob atomic/travas C→D→Ch, grava `validada_em/por` e decisão VALIDAÇÃO; roda **uma pergunta** antes do laço. `V:425`, `:431`, `:435`, `:461`, `:463`; escrita `V:134`, `:137`, `:139` | `validada` se gravou; `ja_no_espelho` se reuso. `V:515`, `:517` | `T/test_o261_recusa_fala.py:135` |
| Validar hora; unidade recusou sem encerrar e há motivo | `_resposta_de_hora`; `materializada_em is None`; motivo preenchido | Limpa **só esta** `validada_em/por`, preserva resposta, registra DEVOLUÇÃO; pode reabrir DS, salvo `sem_reabrir`. `V:188`, `:190`, `:192`, `:199`, `:205`, `:217`, `:464` | `nao_gravou`, `devolvida=True`; repetição igual não duplica trilha. `V:473`, `:499` | `T/test_o261_recusa_fala.py:87`, `:110`, `:148`, `:164` |
| Validar; recusa já encerrou identidade | Mesma devolução verifica materialização | Não desfaz validação quando `materializada_em` preenchida. `V:190` | Devolução `False`; retorno final depende erros. `V:191`, `:490` | `T/test_o261_recusa_fala.py:190` |
| Materializar lote normal | Fila por carimbos; unidade; C6/C7 | Filtra resposta+validação não nulas/materialização nula; itera **individualmente**; cauda fecha se C7 permitir. `MT:1369`, `:1375`, `:1376`, `:1378`, `:1316`, `:1333` | Dict `materializadas/reusos/pendentes/disputa_fechada/erros`. `MT:1381` | `T/test_o317_passivo_pela_porta.py:188`, `:207`, `:481`, `:502` |
| Unidade; dia diverge sem conferência | `fio.data_do_fato_de` | Não planta; grava recusa na pergunta. `MT:1191`, `:1193`, `:1213` | `False`. `MT:1214` | sem teste específico aberto | [nao verificado]
| Unidade hora sem texto humano | Guarda de anuência por `resposta_colab.strip` | Não planta; recusa escrita. `MT:829`, `:836` | Ramo `None`; unidade `False`. `MT:841`, `:1227` | referência interna a `T/test_a27_recusa_com_dono.py` em `MT:831`; teste não aberto |
| Unidade hora; marco indeterminado | `_tipo_pelo_marco` / `entrada_de_marco` / `grade_da_celula` | **Não recusa necessariamente**: mantém tipo pelo motivo e devolve pendência `tipo_nao_conferido`; escritor persiste pendência+motivo. `MT:386`, `:387`, `:395`, `:397`, `:400`, `:850`, `:851` | Continua até demais guardas. `MT:404`, `:414`, `:422` | sem teste específico aberto | [nao verificado]
| Hora; batida no slot já existe | `_batida_existente_na_janela` | Reusa, emite aviso `__REUSO__`, pode trocar `pergunta_origem` por `anotar`; reconcilia destino/clones. `MT:859`, `:865`, `:866`, `:946`, `:948`, `:974` | Ramo `True`; aviso deslocado para `reusos`. `MT:975`, `:1291`, `:1295` | `T/test_o317_passivo_pela_porta.py:207` cobre controle normal; teste específico de reuso não aberto |
| Paridade; vizinha mesmo tipo, sem furo que absolva, lâmpada `False/None` | `_vizinha_mesmo_tipo`, `_plantio_preenche_furo_na_data`, `fato_do_marco_no_espelho` | Não planta; `_carimbar_recusa` sem via terminal PARIDADE; pela validação, devolução limpa validação. `MT:875`, `:876`, `:900`, `:901`, `:913`; `V:464` | Ramo `None`; unidade `False`; validação `nao_gravou`. `MT:916`, `:1227`; `V:500` | `T/test_guarda_paridade_materializador.py:59`; `T/test_o261_paridade_respeita_a_lampada.py:174`, `:192`, `:202` |
| Paridade; mesma guarda, lâmpada exatamente `True` | Mesmo juiz da lâmpada | **Não planta**, mas `carimbar_identidade(VIA_PARIDADE_RECUSADA)` grava encerramento. `MT:901`, `:924` | Ramo `None`, unidade `False`, embora haja escrita de estado terminal. `MT:925`, `:1227` | `T/test_o261_paridade_respeita_a_lampada.py:183`; `T/test_o317_passivo_pela_porta.py:361` |
| Guarda paridade; furo do próprio dia | `_plantio_preenche_furo_na_data` lê célula/ata e `plantio_preenche_furo` | Pode atravessar a guarda. A véspera não integra a consulta atual. `MT:170`, `:181`, `:876` | Prossegue para porta Batida. `MT:927` | `T/test_o261_vespera_nao_absolve.py:76`, `:85` |
| Hora elegível sem vizinha impeditiva | `registrar_batida`, lastro `_pergunta_materializada_de_fato`, C6 em `carimbar` | Cria Batida institucional, anota origem, reconcilia destino; carimba via declaração ou via fato com `resposta='validar'`, relavra. `MT:927`, `:929`, `:930`, `:948`, `:974`, `:1233`, `:1277` | Unidade `True`. `MT:1282`, `:1299` | `T/test_guarda_paridade_materializador.py:90`; `T/test_o261_recusa_fala.py:135` |
| Batida criada/reusada, mas lastro/C6 reprova | Lastro e C6 | Exceção `_Reprovada` dentro atomic desfaz Batida/carimbo; finally grava motivo fora do savepoint. `MT:1220`, `:1233`, `:1239`, `:1277`, `:1279`, `:1283`, `:1297` | `False` e erro. `MT:1299` | `T/test_hx_materializa_depois_julga.py:30` |
| Recusa textual classificada COLISÃO | `traduzir_recusa` | `carimbar_identidade(VIA_FATO_OCUPADO)`; motivo no mesmo save; demais códigos só alteram motivo. `MT:1091`, `:1093`, `:1099`, `:1102` | `_carimbar_recusa` não possui retorno explícito. `MT:1107` | `T/test_o261_recusa_fala.py:190` prova devolução não mexe no encerrado |
| Reentrada durante plantio | `pergunta_em_voo` transacional | O materializador é escritor da P em voo; S143/S144 devem pulá-la. `MT:1141`, `:1144`, `:1146`, `:1150`, `:1153`, `:1425` | Contexto termina em commit/rollback/exceção. `MT:1147`, `:1149` | `T/test_pep_reentrada.py:109`, `:134`, `:141`, `:176`, `:193`, `:305` |

Divergências para consolidação:

1. **Unidade individual × par**. Opções atuais: pré-materializar uma pergunta na validação; depois iterar prontas individualmente; reparador também usa a unidade com fila distinta (`V:461`, `:463`, `:469`; `MT:1162`, `:1369`, `:1375`). A lei já manda par em um ato, metade espera, e estado derivado: `app/docs/BACKLOG.md:1929`, `:1930`. Não é nova pergunta de lei.

2. **`validada_em` = veredito humano × preenchimento simétrico pelo carimbo**. Validador escreve assinatura humana (`V:134`, `:135`), materializador pede `carimbar(..., resposta='validar')`, que pode suprir carimbos (`MT:1273`, `:1277`). O lookup “escritor único” não prova uma única atribuição física; desenho do escritor já decidido por L-007 (`app/docs/LEIS.md:65`). Não redefinir o campo silenciosamente.

3. **`materializada_em` = aplicação × encerramento sem Batida**. Paridade autorizada grava identidade e retorna `None` (`MT:924`, `:925`); COLISÃO também encerra sem plantio (`MT:1099`); caminho normal usa mesmo carimbo depois de julgar lastro (`MT:1233`, `:1277`). O enum já distingue vias; não exige campo novo (`MT:438`, `:439`; `app/docs/BACKLOG.md:1930`).

4. **Leitor `estado_aplicacao_pergunta` re-julga e não acompanha toda a escrita**. Se carimbada, só separa paridade e chama o restante `aplicada`, sem lastro (`MT:434`, `:438`, `:442`). Sem carimbo exige `existente.aprovada` para `ja_satisfeita`, enquanto escritor reusa qualquer Batida encontrada (`MT:448`; `:859`, `:865`). A previsão `bloqueada_paridade` não consulta lâmpada, mas escritor consulta para decidir identidade (`MT:452`, `:453`, `:454`; `:900`, `:901`, `:924`). L-002/testemunha e célula soberana já decidem quem manda (`app/docs/LEIS.md:60`, `:71`; `CLAUDE.md:167`).

5. **`pendencia_admin`/`recusa_motivo` não significam exclusivamente “espera outra metade”**. Pendência nasce também por tipo sem marco, com plantio permitido (`MT:387`, `:395`, `:397`, `:850`); recusa recebe contestação colab (`E:1497`), recusa materializador (`MT:1102`), recusa admin (`V:584`). Derivar espera só por esses campos colide com significados já existentes; estado explícito foi superado (`app/docs/BACKLOG.md:1930`).

6. **Universo cru da vizinhança × apurável**. `_vizinha_mesmo_tipo` usa `Batida.objects` sem excluir `retratada_em`, e compara `Batida.tipo` (`MT:94`, `:98`, `:100`, `:104`). L-025 e L-087 já fornecem universo/papel; não é opção de lei nova (`app/docs/LEIS.md:76`, `:119`).

Atribuição quantitativa dos 77 colab-dias, 486 grupos/848 excedentes e 18 DS a estas transições: **[aguarda sonda]**. Os documentos registram os agregados e vínculo à obra, mas não fornecem PKs ou partição pelas condições acima (`app/docs/BACKLOG.md:1882`, `:1883`). O caso 51522/51523 foi fornecido no prompt; estado efetivo das duas linhas **[nao verificado]**.

## 11/11 09:07 · sessao e58cbb9aa56f · - · - · origem=Ronald (tmux)

Recorte conferido por leitura; testes abaixo foram abertos, não executados. Resultado de execução: [nao verificado]. [nao verificado]

### Matriz — reconciliador e fechamento

| Porta / entrada | Juiz consultado | Operação / persistido | Retorno | Prova lida |
|---|---|---|---|---|
| `lavrar_sem_turno_aberto`: motivo C1, texto vazio, sem materialização, sem turno aberto naquele dia | `_tem_turno_aberto` e depois C6 `pergunta_viva`; pergunta viva barra | `p.carimbar(VIA_SEM_TURNO_ABERTO, user_sistema)`; C7 depois pode chamar `d.fechar` | `False` nas guardas/carimbo recusado; `True` após carimbo, mesmo que DS não feche | Código `app/chamados/reconciliador.py:362`, `:364`, `:366`, `:371`, `:398`, `:405`, `:411`, `:414`; testes `app/chamados/tests/test_o261_lavra_le_a_lampada.py:129`, `:148`, `:172` |
| `retratar_perguntas_absolvidas_por_celula`: P sem validação/materialização, célula julgada que não acusa | C6; tranca excluída separadamente | Atomic por P, trava C, `carimbar(VIA_CELULA_ABSOLVEU)`; acumula DS, depois C7 e fechamento | Número de carimbos; exceções individuais são logadas e laço segue | `app/chamados/reconciliador.py:584`, `:592`, `:595`, `:611`, `:617`, seleção `:627`, juiz `:661`, tranca `:667`; `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:70`, `:75`, `:84`, `:105` |
| `encerrar_orfas_pela_lampada`: motivos `orfao_14h`/`saida_sem_entrada`, DS aberta, sem carimbo/validação, com dia | `veredito_orfa`: texto vazio, lâmpada do próprio marco=True, C6 viva, teto temporal e `dia_encerrado_motivo` | `p.encerrar_fantasma(user)` e evento; para DS atingidas consulta respostas sem veredito, depois C7, chama fechar | Dict `encerradas`, `adiadas`, `disputas_barradas`, `linhas`; falha de fechamento pode entrar em `falhas` | `app/chamados/reconciliador.py:1040`, `:1048`, `:1082`, `:1084`, `:1086`, `:1088`, `:1090`, `:1128`, `:1147`, `:1163`, `:1170`, `:1175`; `app/chamados/tests/test_orfas_pelo_evento.py:96`, `:112`, `:124`, `:133`, `:150`, `:167` |
| C7, leitura | `respostas_sem_veredito()` e `fio_vivo(list(perguntas))` | Nenhuma coluna `pode_fechar`; método derivado | `not respostas_sem_veredito and not fio_vivo` | `app/chamados/models.py:1848`, `:1853`; registro `app/core/juizes.py:279`; `app/docs/CORTES.md:272` |
| Fechar normal, DS aberta, resposta sem veredito | Lista canônica: validação nula, materialização nula, timestamp ou texto de resposta; dia trancado não bloqueia | Recusa antes das escritas | `ValueError` | `app/chamados/models.py:1842`, `:1846`, `:1886`, `:1888`; `app/chamados/tests/test_c7_red_minimos.py:160`, `:377` |
| Fechar normal, DS aberta, P viva | C6 por pergunta | Recusa | `ValueError` | `app/chamados/models.py:1895`, `:1898`; `app/chamados/tests/test_c7_red_minimos.py:132` |
| Fechar normal, DS aberta, sem resposta sem veredito nem P viva | Mesmas duas guardas de C7, implementadas novamente no corpo de `fechar` | `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento`; motivo `vazia` com zero P, `veredito` com alguma P; evento | `True` | `app/chamados/models.py:1886`, `:1896`, `:1903`, `:1932`, `:1936`, `:1941`; `app/chamados/tests/test_c7_red_minimos.py:147`, `:174`, `:185` |
| Fechar `desligamento` / `admin_encerra` | Resposta sem veredito continua vetando; exceção nomeada admite silêncio vivo | Antes do save DS, `fechar_por_admin` nas vivas, decisão SILENCIO e eventos; depois quarteteto do fechamento | `True`; `admin_encerra` exige motivo | `app/chamados/models.py:1886`, `:1918`, `:1921`, `:1924`, `:1932`, `:1943`; `app/chamados/tests/test_c7_red_minimos.py:209`, `:221`, `:234`, `:261` |
| Fechar competência trancada | Tranca por leitura de C6/lista de respostas | Fecha DS com motivo específico; não carimba P | `True` pela porta | `app/chamados/models.py:1865`; `app/chamados/tests/test_c7_red_minimos.py:329`, `:361`, `:369`, `:377` |
| Fechar DS já fechada, mesma entrada / entrada diferente | Idempotência por autor, resolução, motivo | Sem nova escrita na mesma entrada | `False` / `ValueError` | `app/chamados/models.py:1879`, `:1882`, `:1883`; `app/chamados/tests/test_c7_red_minimos.py:428`, `:439` |

A leitura de respostas e de vivas acontece antes do atomic/trava interno do fechamento; o atomic começa em `app/chamados/models.py:1905`, após as leituras `:1886` e `:1895`. Efeito concorrente em produção: [nao verificado].

### Decisões candidatas

1. **Bool de fechamento com dois significados, já decidido pela lei.** O método C7 exige ausência de resposta sem veredito **e** ausência de fio vivo (`app/chamados/models.py:1853`). O explicador escreve `pode = not _fio_vivo(ps)` e publica isso sob a mesma chave `pode_fechar` (`app/chamados/services/explicador.py:137`, `:163`). Opções presentes: critério completo C7 versus critério parcial do explicador. A lei escolhe C7, sem pergunta nova: `app/docs/CORTES.md:272`, `app/docs/LEIS.md:60`, `app/core/juizes.py:279`.

2. **Carimbo não equivale a batida criada, já reconhecido pelo código.** A UI distingue `materializada_em` com `VIA_PARIDADE_RECUSADA` de aplicação (`app/chamados/services/materializacao.py:434`, `:438`, `:439`), mas todo outro carimbo retorna `aplicada` imediatamente (`:442`). Opções de leitura presentes: carimbo genérico / via específica / prova do fato. A obrigação de ler a mesma autoridade já consta da L-002 e L-061 (`app/docs/LEIS.md:60`, `:95`); a classificação de todas as vias no caso vivo depende da sonda [aguarda sonda].

3. **`validada_em` encerra vitalidade sem provar aplicação, e `estado` traduz outra pergunta.** C6 retorna morto imediatamente com validação (`app/chamados/juizes.py:1325`). `P.estado` retorna `VALIDADA` quando validada e não materializada, `RESOLVIDA` se ambas (`app/chamados/models.py:3041`, `:3043`). O mandato do par já exige aplicar ambas num ato e esperar a metade; estado explícito foi superado pelo derivado (`app/docs/BACKLOG.md:1929`, `:1930`). Não reabrir lei sobre “criar estado espera”; esse desenho já está decidido.

4. **Acervo BATIDA sem episódio versus episódio — pergunta de lei real.** `plano_de_retratacao` declara a lei aberta para permitir identidade/retratação entre as classes (`app/chamados/services/fio.py:1274`, `:1275`). Opções existentes: preservar por `sem_episodio_intocada`; retratar somente o restante aceito pelo plano (`:1294`, `:1297`). Outras proteções não são perguntas novas: mérito do admin `:1290`, múltiplos episódios `:1292`. Pergunta: “Quando a linha BATIDA sem episódio e a linha com episódio representam o mesmo fato, autorizando uma retratar a outra?” Alcance nos 486/848 [aguarda sonda].

5. **Várias caudas fecham DS; “só beco” não descreve esta cópia.** Reconciliador tem caudas C7 em `app/chamados/reconciliador.py:411`, `:611`, `:1170`. O que explica cada uma das 18 DS depende de PKs/estados e histórico, ausentes no raspado [aguarda sonda]. A lei de fechamento já foi decidida por C7 e L-007 (`app/docs/CORTES.md:272`, `app/docs/LEIS.md:65`).

Número de perguntas de lei identificadas neste recorte: **1**, a de identidade do acervo sem episódio (`app/chamados/services/fio.py:1275`). Não atribuir aos 77/486/848/18 nenhuma fração sem sonda.

### Funções para a sonda

- `fio.grupos_de_colisao(colaborador_id=None, dia_antes_de=None, fora_do_recorte=None)` devolve `(familia, chave, pks)`; chama o universo real da porta (`app/chamados/services/fio.py:1923`, `:1976`, `:1988`).
- `fio.universo_do_grupo(pks)` carrega linhas do fato, exclui conversa/vias fora do fato e chama `fecho_do_fato` (`app/chamados/services/fio.py:1905`, `:1910`, `:1912`).
- `fio.canonica_do_grupo(ps)` prefere texto não vazio e depois menor PK (`app/chamados/services/fio.py:1231`, `:1232`, `:1233`).
- `fio.plano_de_retratacao(perdedoras, canonica)` retorna `retrata` como objetos; `merito_recusado`, `episodios_unidos`, `sem_episodio_intocada` como PKs; `canonica_sem_episodio` é marcador (`app/chamados/services/fio.py:1261`, `:1277`, `:1280`, `:1290`).
- As chaves são MARCO `(colaborador_id, data_evento, motivo, instancia_marco)` e BATIDA `(batida_origem_id, motivo)`; o fecho pode dividir uma chave BATIDA por dia. Agrupar tudo apenas por colaborador-dia não reproduz os 486 grupos do juiz (`app/chamados/services/fio.py:1372`, `:1963`, `:1976`).
- Não há função canônica implementada de elegibilidade conjunta do par identificada no recorte; o mandato está no BACKLOG (`app/docs/BACKLOG.md:1929`). Um agrupamento diagnóstico colab-dia não deve ser apresentado como juiz do par [nao verificado: existência global fora do recorte].
- `pergunta_viva` memoiza só em memória (`app/chamados/juizes.py:863`, `:864`), mas seus caminhos de exceção logam via `registrar_engolido` (`:1397`, `:1409`), que usa `logging.error` (`app/core/observ.py:73`, `:79`). Para uma sonda que nada grava, usar envelope SQL `SET TRANSACTION READ ONLY` em todos os aliases consultados, sem chamar portas mutadoras, e desabilitar logging durante a leitura. Não usar `rollback` como licença para executar os materializadores; eles não são leitura.

Amostra positiva já aberta de leitores relevantes: lista de respostas `app/chamados/models.py:1843`, C7 `:1853`, C6 `app/chamados/juizes.py:1325`, explicador `app/chamados/services/explicador.py:137`, seleção reconciliador `app/chamados/reconciliador.py:629`.

Amostra positiva já aberta de escritores relevantes: fechamento `app/chamados/models.py:1932`; delegações de carimbo `app/chamados/reconciliador.py:405`, `:595`, `:1147`; decisão de silêncio `app/chamados/models.py:1924`. Escritores físicos do carimbo devem ser completados pelo outro explorer.

## 11/11 09:07 · sessao 12e26c039336 · - · - · origem=Ronald (tmux)

Inventário de interfaces conferido por AST em memória (`python3 -B`, stdin), complementado pela leitura dos templates; script não importa a aplicação. Execução produtiva e consumidores externos: [nao verificado].

| Arquivo / função | Leitura e pergunta respondida |
|---|---|
| `app/api/views.py:973 api_chamados_v2` | F em `:1025`, `:1041`: representação de disputas abertas; R em `:1170`, `:1172`: texto e instante no JSON; R em `:1184`: habilita correção. |
| `app/api/views.py:2531 api_disputa_perguntas` | R em `:2582`: subconjunto `respondidas` compatível com APK. |
| `app/api/views.py:2554 api_disputa_perguntas._item` | `estado` em `:2555`, R em `:2566`, `:2568`, `resposta_display` em `:2567`: estado, espera por admin, resposta e instante do item JSON. |
| `app/api/views.py:2589 api_disputa_responder` | R em `:2625`, `:2642`: reenvio e instante devolvido após resposta. |
| `app/api/views.py:2675 api_solicitar_correcao_disputa` | F SQL em `:2683`: correção somente em continente aberto. |
| `app/api/views_mensageria.py:554 _item_validar` | R em `:567`, `:569`, `:570`: texto, instante e dias de espera na fila IA; chamado por `api_mensageria_validar` em `:546`. |
| `app/api/foto_recebida.py:57 resolver_disputas_de_foto` | F SQL em `:62`: disputas de foto alcançadas pelo upload. `resolucao=` em `:80` é argumento de escrita, não leitor. |
| `app/chamados/views.py:359 detalhe_chamado_local` | F SQL em `:471`, `:495`, `:512`, `:530`: continente/perguntas do detalhe e cards. |
| `app/chamados/views.py:882 modal_fio_colaborador._resp_modal_full` | F SQL em `:911`: disputa carregada no modal. |
| `app/chamados/views.py:1228 responder_pergunta_disputa` | C em `:1257`: proteção contra alteração de resposta regularizada; R em `:1280`: resposta anterior/reenvio. |
| `app/chamados/views.py:1317 solicitar_correcao_disputa` | F SQL em `:1329`: gate de continente aberto. |
| `app/chamados/views.py:1372 reabrir_disputa_supervisao` | R SQL em `:1416`: perguntas pendentes que, após C6, determinam recobrança versus correção. |
| `app/chamados/views_cobrar.py:145 validar_inline` | F em `:161`, `:164`; R em `:169`, `:176`: continente, respostas disponíveis para validar e perguntas ainda aguardando colaborador. |
| `app/chamados/views_cobrar.py:275 proposta_lote` | R em `:291`: amostra JSON da proposta de validação. |
| `app/chamados/relatorios_disputa.py:11 classificar` | R em `:21`: `AGUARDA_COLAB` versus `AGUARDA_SUPERVISAO`, depois de selecionar vivas. |
| `app/chamados/relatorios_disputa.py:26 coletar` | F SQL em `:41`: universo do relatório de disputas abertas. Consumidores: `app/colaboradores/views.py:2997`, `app/chamados/management/commands/relatorio_pendencias_disputa.py:15`. |
| `app/chamados/templatetags/chamados_tags.py:107 leitura_da_resposta` | `getattr(resposta_colab)` em `:128`: evita interpretar resposta vazia; consulta o juiz de classificação em `:133`, depois produz frase em `:138`. |

| Template / unidade de render | Leituras |
|---|---|
| `app/templates/chamados/partials/modal_fio.html` | R `:185`, `:217`, `:219`, `:220`; V `:228`; A20 instante/autor `:170`, `:171`; A17 pendência `:222`, `:225`; recusa `:226`; F histórico `:379`; indiretos `disputa.aberta` `:76`, `:326`, filtro `leitura_da_resposta` `:250`. |
| `app/templates/chamados/partials/_card_atendimento.html` | R `:94`, `:100`, `:108`, `:135`, `:137`, `:142`, `:148`, `:155`, `:176`, `:177`, `:384`, `:385`; indiretos `viva` `:77`, `estado` `:183`, `resposta_display` `:100`, `:137`, `:177`, `:385`; A17 `:183`; via/recusa `:186`. A linha `:186` mistura vias derivadas e duplicata A06: classificação precisa ser por ramo. |
| `app/templates/chamados/partials/_pergunta_card.html` | V `:20`, `:23`, `:42`; R `:24`, `:29`, `:32`; `resposta_display` `:31`; filtro `leitura_da_resposta` `:43`. |
| `app/templates/chamados/partials/_lote_preview.html:24` | R: resposta na prévia de validação em lote. |
| `app/templates/chamados/fio_colaborador.html:164` e `:169` | `disputa.aberta`: cor do continente. |

Os vínculos entre render e template estão em `app/templates/chamados/detalhe_local.html:310`, `app/chamados/views.py:404`, `app/templates/chamados/meu_atendimento.html:127`, `:144`, `app/chamados/views.py:927` e `app/chamados/views_cobrar.py:362`.

Nenhum consumidor JS local dos nomes de campos legados foi encontrado na busca estática; exaustividade dinâmica e APK: [nao verificado]. O aparente `p.estado` em `app/templates/core/_copiloto.html:407` pertence às pautas carregadas por `/pautas/aba/` em `:359`, não a `PerguntaDisputa`. Inputs `name="recusa_motivo"` em `app/templates/chamados/partials/modal_fio.html:61`, `:295` e `_pergunta_card.html:114` são nomes de formulário, não leituras da projeção.

## 11/11 09:07 · sessao 896f3b04993a · - · - · origem=Ronald (tmux)

Dependências verificadas por AST de imports, sem importar a aplicação:

| Dependência | Evidência |
|---|---|
| `models ↔ juizes` | `app/chamados/models.py:153`; `app/chamados/juizes.py:11` |
| `models ↔ fio` | `app/chamados/models.py:433`; `app/chamados/services/fio.py:157` |
| `models ↔ disputa_emissao` | `app/chamados/models.py:2219`; `app/chamados/services/disputa_emissao.py:16` |
| `juizes ↔ materializacao` | `app/chamados/juizes.py:371`; `app/chamados/services/materializacao.py:155` |
| `fio ↔ vigia_fio` | `app/chamados/services/fio.py:774`; `app/chamados/services/vigia_fio.py:558` |
| `models ↔ reconciliador` | `app/chamados/models.py:977`; `app/chamados/reconciliador.py:241` |
| `declaracao_texto → validacao → disputa_emissao → declaracao_texto` | `app/chamados/services/declaracao_texto.py:648`; `app/chamados/services/validacao.py:559`; `app/chamados/services/disputa_emissao.py:859` |
| `tarefa_do_dia → juizes/models/fio/painel/lavra` | `app/chamados/services/tarefa_do_dia.py:123`, `:124`, `:125`, `:364`, `:401` |
| APIs/dashboard/cobrança → consulta central | `app/api/views.py:324`, `:1004`, `:2542`; `app/api/views_core.py:313`; `app/core/views.py:252`; `app/chamados/services/cobranca.py:47`; `app/chamados/services/aviso_pendencia.py:8` |

A amostra de 18 arquivos forma uma componente de dependências cíclicas com 17 arquivos: `models`, `juizes`, `reconciliador`, `fio`, `materializacao`, `disputa_emissao`, `declaracao_texto`, `validacao`, `pergunta_no_app`, `veredito_lavrado`, `raiox`, `explicador`, `relavra_via`, `vigia_fio`, `fabrica_da_celula`, `painel`, `verbos`. Isso impede inventar uma ordem topológica de pousos independentes apenas pelos imports; os ciclos acima são as testemunhas. Os imports locais permitem carga tardia, mas continuam sendo dependências de execução. (`app/chamados/models.py:433`; `app/chamados/services/fio.py:304`; `app/chamados/services/ex­plicador.py:69`; `app/chamados/services/raiox.py:186`; `app/chamados/services/verbos.py:9`; `app/chamados/services/painel.py:194`; `app/chamados/services/fabrica_da_celula.py:138`; `app/chamados/services/relavra_via.py:209`)

Ordem consumível pelo brief:

1. Gate da E4b medida; só depois migrar fonte. A ordem escrita é E4a → E4b → E4c → E4d, com exceções explícitas. (`app/docs/PROMPTS.md:2034`; `:2054`)
2. Núcleo de leitores/properties/querysets e a alimentação de `tarefa_do_dia`, tratado como lote coordenado quando tocar arquivos compartilhados ou ciclos acima. A consulta central ainda depende de `q_vivas`, C6 e R/F. (`app/chamados/services/pergunta_no_app.py:47`, `:50`, `:58`, `:71`)
3. Cascas/API/dashboard/cobrança e respectivos templates/JS, consumindo as interfaces estabilizadas; todas as mudanças em `app/api/views.py` pertencem ao mesmo lote de arquivo. (`app/api/views.py:324`, `:1004`, `:2542`; `app/core/views.py:252`; `app/chamados/services/cobranca.py:47`)
4. Auditorias, vigias e instrumentos que hoje observam projeções; separar conservação histórica da pergunta corrente, preservando diagnóstico da migração. (`app/chamados/management/commands/diff_e3_sombra.py:22`; `app/chamados/services/vigia_fio.py:803`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:44`)
5. E4d só após o selo E4c, preservando as exceções; parar escrita não autoriza retirar fisicamente o acervo exportado. (`app/docs/PROMPTS.md:2034`; `:2026`)

`backfill_fio_morto_e4b` e `foto_e4b` não foram encontrados pelos nomes nesta cópia; implementação, estrutura da foto, lista de arquivos atingidos e contrato dessas interfaces: **[nao verificado]**. O contrato operacional verificável limita o passivo a competências não exportadas e manda exportadas para lista do `!`. (`app/docs/PROMPTS.md:2026`)

Não há repositório Git disponível nesta raiz: `git log` retorna “not a git repository”. Portanto identidade Git de `2e9d…`, pai da obra E3 e lista exata de arquivos do diff solicitado: **[nao verificado]**. O relato local menciona `e3fix9`, uma base e outra foto, mas isso não substitui o diff deste hash. (`app/docs/relato_fio_e0_v4_e3_x1.txt:5`; `:7`)

Para o delta pós-pouso, a lista **declarada no anexo**, cuja inclusão efetiva no diff continua **[nao verificado]**, é: `app/api/views.py`, `app/chamados/services/aviso_pendencia.py`, `app/chamados/services/pergunta_no_app.py`, `app/chamados/services/tarefa_do_dia.py`, `app/chamados/services/cobranca.py`, `app/chamados/juizes.py`. As interfaces atuais existem em `app/api/views.py:1004`, `app/chamados/services/aviso_pendencia.py:8`, `app/chamados/services/pergunta_no_app.py:61`, `app/chamados/services/tarefa_do_dia.py:562`, `app/chamados/services/cobranca.py:47`, `app/chamados/juizes.py:1516`.

## 11/11 09:07 · sessao 8ce0aaba136e · BO-ADMIN-1111 · sonda · origem=Code

**As duas perguntas estão respondidas, sem validação e sem materialização. Existem ações de tela que aceitam esse estado; um caminho que conclua ambas com as mesmas horas e batidas permanece [não verificado].** O explicador devolveu `aguarda_validacao`, dono `admin`, para ambas. (`app/chamados/services/explicador.py:77`; sonda: `validada_em = None`, `materializada_em = None` e retornos de `explicar_pergunta`.)

**1. Estado gravado — tabela solicitada em (a)**

Cada valor abaixo reproduz o respectivo bloco `PerguntaDisputa pk=51522/51523` da sonda; a última coluna identifica o campo no código.

| Campo | #51522 | #51523 | Código |
|---|---|---|---|
| Colaborador / célula | col960 / 128547 | col960 / 128547 | `app/chamados/models.py:2414`, `:2433` |
| Motivo / instância | `intervalo_saida` / `int_ida` | `intervalo_volta` / `int_volta` | `app/chamados/models.py:2421`, `:2434` |
| `data_evento` | 21/10/2026 | 21/10/2026 | `app/chamados/models.py:2423` |
| `resposta_colab` | `01:00` | `02:00` | `app/chamados/models.py:2447` |
| `respondida_em` | 08/11/2026 11:04:01 | 08/11/2026 11:04:11 | `app/chamados/models.py:2448` |
| `validada_em` | `None` | `None` | `app/chamados/models.py:2449` |
| `materializada_em` | `None` | `None` | `app/chamados/models.py:3393` |
| Via — nome real: `via_resolucao` | `''` | `''` | `app/chamados/models.py:2561` |
| `pendencia_admin` | `''` | `''` | `app/chamados/models.py:2556` |
| `veredito_lavrado` | `nao` | `nao` | `app/chamados/models.py:2585` |
| `julgado_em` | 11/11/2026 08:15:10 | 11/11/2026 08:15:10 | `app/chamados/models.py:2590` |
| Disputa / destino | 5644 / 35664 | 5644 / 35664 | `app/chamados/models.py:2416`, `:3385` |

O campo equivalente pedido é realmente **`recusa_motivo`**. A sonda trouxe os seguintes textos; o formato corresponde ao erro de paridade em `app/chamados/services/materializacao.py:878`, persistido pelo `finally` em `:1296` e por `_carimbar_recusa` em `:1101`.

> #51522: `Pergunta #51522: batida vizinha #133794 (S 22/10 06:30) tem o mesmo tipo do plantio (S) - par quebrado; nao materializado (guarda de paridade).`

> #51523: `Pergunta #51523: batida vizinha #133756 (E 21/10 18:32) tem o mesmo tipo do plantio (E) - par quebrado; nao materializado (guarda de paridade).`

A disputa **5644 está aberta**, pois a sonda mostra `fechada_em = None`. Seu chamado-pai **25270** tem `status_local = 'aberto'`; o chamado-destino comum **35664** tem `status_local = 'em_analise'`, `resolvido_em = None`. (`app/chamados/models.py:1749`, `:1757`, `:52`, `:3385`; blocos correspondentes da sonda.)

O `veredito_lavrado='nao'` significa **“NAO materializada de fato”** no campo da pergunta; não é prova de decisão administrativa sobre o dia. (`app/chamados/models.py:2582`; sonda: `veredito_lavrado = 'nao'`.)

**2. Ramo do plantio**

Para ambas, a classificação sustentada pelo estado é **recusa de paridade sem carimbo terminal**, no caminho de `_materializar_resposta_hora`. O texto gravado identifica a guarda em `app/chamados/services/materializacao.py:875` e `:878`; a sonda retorna `fato_do_marco_no_espelho(p) -> False`, que, aplicado ao código desta cópia, entra em `:901`, chama `_carimbar_recusa` em `:913` e retorna em `:916`.

**Não estão carimbadas com `VIA_PARIDADE_RECUSADA`:** a sonda mostra `via_resolucao=''` e `materializada_em=None`; o carimbo dessa via ocorre somente depois daquele retorno, em `app/chamados/services/materializacao.py:924`.

O motivo completo com a vizinha fica gravado porque o `finally` chama novamente `_carimbar_recusa` com o último erro. Essa função só carimba identidade quando a tradução é `COLISAO`; para `PARIDADE`, atualiza o motivo sem preencher materialização ou via. (`app/chamados/services/materializacao.py:1296`, `:1091`, `:1093`, `:1101`; classificação `PARIDADE` em `app/chamados/catalogo/recusa.py:26`.)

A validação nula é compatível com o fluxo que desfaz a validação quando uma resposta de hora não foi materializada e tem recusa escrita, preservando a resposta do colab. **A chamada histórica, seu autor e seu instante não foram fornecidos: [não verificado].** (`app/chamados/services/validacao.py:190`, `:199`, `:200`; sonda: resposta preservada, validação nula, recusa preenchida.)

**3. Vizinhas e resultado do juiz**

| Pergunta | Vizinha indicada na recusa | Tipo | Hora local fornecida | Origem | `fato_do_marco_no_espelho` |
|---|---:|---|---|---|---|
| #51522 | #133794 | `S` | 22/10/2026 06:30:52 | `app` | `False` |
| #51523 | #133756 | `E` | 21/10/2026 18:32:07 | `app` | `False` |

Sustentação: blocos `Batida pk=133794/133756`, respectivos `recusa_motivo` e chamadas literais do juiz na sonda; seleção da vizinha em `app/chamados/services/materializacao.py:85`, `:94`, `:100`; correspondência dos motivos aos marcos em `app/chamados/juizes.py:887` e `:888`.

O juiz real lê DNA e lâmpadas da célula e retorna `all(_vistas)`. O `False` fornecido não autoriza reconstruir a ATA completa nem atribuir erro às vizinhas. (`app/chamados/juizes.py:910`, `:927`, `:933`; ATA completa e correção factual das batidas: [não verificado].)

**4. ADMIN: texto, badge, botões e portas**

A sonda devolveu literalmente:

| Pergunta | Estado / dono | Motivo | Ação |
|---|---|---|---|
| #51522 | `aguarda_validacao` / `admin` | `colaborador respondeu 01:00` | `Validar ou recusar a resposta "01:00".` |
| #51523 | `aguarda_validacao` / `admin` | `colaborador respondeu 02:00` | `Validar ou recusar a resposta "02:00".` |

Esse é o degrau `RESPONDIDA`, que retorna antes dos ramos de pendência e do juiz de materialização. (`app/chamados/models.py:3041`, `:3048`; `app/chamados/services/explicador.py:77`, `:85`, `:93`.)

No detalhe, o badge previsto é **“⚠ aguardando validacao”**, seguido da resposta e do horário de envio. No modal, os campos dados não entram nos badges “Pendencia do admin” ou “Validada”; oferecem o ramo de ações sobre a resposta. (`app/templates/chamados/partials/_pergunta_card.html:24`, `:29`; `app/templates/chamados/partials/modal_fio.html:217`, `:222`, `:228`, `:239`.)

A sonda também devolveu `pode_fechar=False` e **“Nao fecha: 30 aguardando sua validacao.”** Isso descreve a disputa inteira; os detalhes das demais pendências estão truncados, portanto [não verificado]. (`app/chamados/services/explicador.py:148`, `:162`; trecho fornecido de `por_que_nao_fecha`.)

As ações das duas devem usar o **chamado-destino 35664**, conforme a precedência de `chamado_que_lista`. A porta confere essa mesma âncora. (`app/chamados/services/fio.py:117`, `:598`, `:635`; sonda: `chamado_destino = 35664`.)

| Ação de tela | Botão e porta | Aplicação ao estado fornecido |
|---|---|---|
| **Validar** | Botão: `app/templates/chamados/partials/modal_fio.html:279`. View do modal: `app/chamados/views.py:869`; encaminhamento: `app/chamados/services/fio.py:585`. | **ACEITA tentar:** respondidas e não validadas passam nas guardas de `app/chamados/services/validacao.py:393` e `:396`. Executa o materializador em `:463`; se a guarda de paridade continuar satisfeita, recusa a gravação e devolve a validação em `:464`. |
| **Rejeitar resposta** | Botão e motivo: `app/templates/chamados/partials/modal_fio.html:290`, `:295`, `:298`. Encaminhamento: `app/chamados/services/fio.py:625`, `:637`. | **ACEITA com motivo:** ambas têm resposta e validação nula. Recusa motivo vazio, pergunta validada ou já pendente em `app/chamados/services/validacao.py:562`, `:573`, `:576`. Reabre para correção em `:593`. **Move para o colab; não materializa o marco.** |
| **Corrigir a hora pela supervisão** | Campos e botão: `app/templates/chamados/partials/modal_fio.html:300`, `:306`, `:309`, `:312`. Encaminhamento: `app/chamados/services/fio.py:639`. | **ACEITA o estado**, com HH:MM e motivo: ambas são de hora, não validadas e de disputa aberta. As recusas estão em `app/chamados/services/acoes_disputa.py:336`, `:338`, `:345`, `:347`, `:349`. Depois grava a resposta e usa a mesma validação/materialização em `:371`, `:379`; conclusão [não verificado]. |
| **Inverter tipo da vizinha** | Clique no horário → “Inverter tipo”: `app/templates/colaboradores/partials/_calendario_grade.html:121`. View: `app/ponto/views.py:3260`. | **ACEITA os campos conhecidos da batida:** ambas são E/S e não retratadas. **RECUSA** sem permissão ou com competência fechada: `app/ponto/views.py:3263`; `app/ponto/services/flip_batida.py:15`, `:21`, `:26`. Permissão, competência e necessidade factual da inversão: [não verificado]. |
| **Retratar a vizinha** | Clique no horário → motivo → “Não foi trabalho (retratar)”: `app/templates/colaboradores/partials/_calendario_grade.html:121`. View: `app/ponto/views.py:3274`. | **ACEITA o estado conhecido**, exigindo usuário autorizado e motivo cadastrado ativo; recusas em `app/ponto/services/retratar_humano.py:142`, `:145`, `:168`, `:170`. Motivo disponível, permissão e fato “não foi trabalho”: [não verificado]. |
| **Resolver dia** | “Resolver dia” → “Trabalhou no horário da escala”, falta ou justificado: `app/templates/ponto/partials/_veredito_dia.html:6`, `:21`, `:22`, `:23`. View: `app/ponto/views.py:3312`. | A **view não recusa simplesmente por pergunta respondida**, mas a **tela não oferece enquanto houver pergunta viva com resposta**: `app/colaboradores/services/calendario.py:472`, `:476`, `:738`; template: `app/templates/colaboradores/partials/_calendario_grade.html:125`. Disponibilidade efetiva: [não verificado]. O serviço ainda pode recusar dia sem trabalho, sem marcos ou sem preenchimento coerente: `app/ponto/services/veredito_celula.py:242`, `:248`, `:308`. |
| **Registrar por regularização externa** | “Adicionar batida nova”, intervalo e “Enviar ao colaborador para confirmacao”: `app/templates/chamados/regularizacao_editar.html:49`, `:57`, `:67`. Views: `app/chamados/views_regularizacao.py:42`, `:74`. | **ACEITA preparar proposta**, mas **RECUSA materializar com disputa aberta** em `app/chamados/services/regularizacao_ext.py:58`, `:61`. A sonda prova a disputa 5644 aberta; portanto não é saída de gravação enquanto esse estado permanecer. |

**Retratar não prova eliminar a recusa de paridade:** a busca da vizinha usa `Batida.objects` sem filtro por `retratada_em` ou `aprovada`. (`app/chamados/services/materializacao.py:94`, `:100`.)

**“Corrigir” também não garante mudar o ponto do plantio apenas trocando o texto:** para intervalo, havendo marco no chamado-destino, o materializador usa a hora desse marco. A sonda mostra `marcos_faltantes` com `01:00/S` e `02:00/E`. (`app/chamados/services/materializacao.py:407`, `:409`, `:411`.)

Há ainda **“Aplicar … e validar”** para declaração em texto, mas exige proposta existente; conserva a resposta já preenchida e volta à mesma validação. Existência de proposta aplicável no caso: [não verificado]. (`app/templates/chamados/partials/modal_fio.html:143`; `app/chamados/services/declaracao_texto.py:649`, `:687`, `:696`.)

**5. Recado do colab**

**O texto efetivamente visto numa tentativa não veio na sonda: [não verificado].** O código permite distinguir:

- **No questionário**, quando a pergunta está visível/viva, os campos fornecidos produzem “Resposta enviada” e **“Aguardando a supervisao conferir. Se precisar corrigir, e so responder de novo abaixo.”** (`app/templates/chamados/partials/_card_atendimento.html:77`, `:94`, `:99`, `:101`; sonda: `respondida_em` preenchida nas duas.)
- **Ao reenviar HH:MM sem tocar no relógio**, a view devolve **“Escolha a hora no relogio antes de enviar.”** Isso depende de `resposta_tocou != '1'`, dado ausente: [não verificado]. (`app/chamados/views.py:1274`, `:1278`.)
- **Na porta “Enviar hora” do chamado**, se ela selecionar estas mesmas perguntas e receber novamente `01:00`/`02:00`, a condição resposta preenchida + mesma hora devolve **“Horario ja informado; aguardando a supervisao.”** A seleção e a tentativa concretas são [não verificado]. (`app/chamados/services/declaracao_texto.py:363`, `:417`, `:418`; retorno à tela em `app/chamados/views.py:1100`.)
- O recado **“resposta anterior ja validada… supervisao precisa refazer esse horario” não é sustentado pelo estado destas duas**, pois exige `validada_em != None`, enquanto ambas têm `None`. (`app/chamados/services/declaracao_texto.py:227`, `:378`, `:412`.)

A própria visibilidade/vitalidade não fica provada apenas por `aguarda_validacao`: o estado pode retornar `RESPONDIDA` no fallback, e o juiz consulta tranca e decisão da célula, não raspadas integralmente. (`app/chamados/models.py:3048`; `app/chamados/juizes.py:1329`, `:1338`; resultado atual de `pergunta_viva`: [não verificado].)

**(b) EXISTE saída pela tela?**

**SIM, para movimentar a espera atual:** a porta **Rejeitar resposta** aceita o estado das duas com motivo e as devolve para correção; **Corrigir** aceita uma hora fundamentada e tenta gravá-la. **Concluir as duas mantendo as mesmas horas e batidas: [não verificado].** (`app/chamados/services/validacao.py:562`, `:573`, `:576`, `:593`; `app/chamados/services/acoes_disputa.py:334`, `:371`, `:379`.)

Passo a passo comprovado pelo código, para admin autorizado e com motivo factual para pedir nova confirmação:

1. Abrir o fio de **col960**, chamado **35664**, onde as perguntas se ancoram. (`app/chamados/views.py:869`; `app/chamados/services/fio.py:117`; sonda: destino 35664.)
2. Na **#51522**, preencher o motivo e clicar **“Rejeitar resposta”**; repetir na **#51523**, se ambas precisarem de nova confirmação. (`app/templates/chamados/partials/modal_fio.html:295`, `:298`; aceitação em `app/chamados/services/validacao.py:573`, `:576`.)
3. O efeito é devolver as perguntas ao colab para correção, não concluir o dia. (`app/chamados/services/validacao.py:593`, `:604`.)

Não há prova suficiente para declarar **“TRAVA SEM SAÍDA PROVADA”**, nem para prometer conclusão por inverter/retratar vizinhas ou por “Resolver dia”; faltam os fatos e condições identificados acima: [não verificado].

**(c) Texto ao admin**

> Abra o atendimento de col960, chamado #35664, e confira as respostas das perguntas #51522 e #51523. Se precisar de nova confirmação, escreva o motivo em cada pergunta e clique em “Rejeitar resposta”; ela volta ao colaborador para correção. Se já tiver confirmado outra hora, preencha a hora e o motivo em “Corrigir” e confira o resultado apresentado após o envio. (`app/chamados/services/fio.py:117`; `app/templates/chamados/partials/modal_fio.html:295`, `:298`, `:306`, `:309`, `:312`; `app/chamados/services/acoes_disputa.py:381`.)

## 11/11 09:06 · sessao e6e2d0dd97e2 · - · - · origem=Ronald (tmux)

Mapa de autoridades confirmado, com endereços atuais da cópia:

| Pergunta do leitor | Autoridade existente | Registro / consulta |
|---|---|---|
| Quais ações ainda são exigíveis, dono e porta por instância? | `tarefa_do_dia`, `app/chamados/services/tarefa_do_dia.py:562`; sequência em `_pela_decisao:116` | `app/core/juizes.py:291`; `app/docs/JUIZES.md:78` |
| A pergunta está viva? | `pergunta_viva`, `app/chamados/juizes.py:862`; corpo `_pergunta_viva_calc:1383` | `app/core/juizes.py:278`; `app/core/juizes.py:729` |
| Alguma pergunta da coleção está viva? | `fio_vivo`, `app/chamados/juizes.py:1488` | `app/core/juizes.py:287` |
| A disputa pode fechar? | `DisputaSupervisao.pode_fechar`, `app/chamados/models.py:1848` | `app/core/juizes.py:279`; `app/docs/JUIZES.md:62` |
| O efeito da pergunta existe? | `_pergunta_materializada_de_fato`, `app/chamados/juizes.py:662`, cuja lâmpada vigente é lida por C19 | `app/chamados/services/tarefa_do_dia.py:17`; `app/docs/JUIZES.md:81` |
| O dia está trancado? | `dia_da_pergunta_trancado:974` / lote `dias_trancados:1004`, em `app/chamados/juizes.py` | `app/core/juizes.py:292`; `app/docs/JUIZES.md:86` |
| Como validar a resposta? | Porta `validar_pergunta`, `app/chamados/services/validacao.py:373` | `app/docs/JUIZES.md:38`; não consta como juiz de leitura separado no bloco chamado de `app/core/juizes.py:272` |
| Qual continente apresenta o fio? | `_disputa_do_chamado`, `app/chamados/services/fio.py:120` | `app/docs/JUIZES.md:40` |
| Por onde se chega ao dia? | C17 `porta_alcancavel_do_dia`, `app/chamados/juizes.py:1606`; destino contratado C19, condicionado ao DIFF | `app/core/juizes.py:289`; `app/docs/PROMPTS.md:2007` |

**Dependências que impedem considerar os juízes já independentes das projeções:**

- C6 consome `projecao_temporal_da_pergunta`, que lê R/V/C/via: `app/chamados/juizes.py:1369`, `:1370`, `:1384`. Sua memo também usa projeções em `app/chamados/juizes.py:821`.
- O juiz do efeito ainda lê `via_resolucao/materializada_em` no ramo sem testemunha e `materializada_em` no ramo escolha: `app/chamados/juizes.py:679`, `:707`.
- C7 ainda depende de R/V/C por `respostas_sem_veredito`: `app/chamados/models.py:1842`, `:1853`.
- C19 ainda lê via para identidade (exceção A06): `app/chamados/services/tarefa_do_dia.py:230`; usa a conservação legada `linhas_da_projecao_sem_linha` na carga: `:398`; recebe C6 para exposição administrativa: `:491`. Não basta apontar o consumidor para C19 e anunciar selo 2. (`app/chamados/services/tarefa_do_dia.py:245`; `app/chamados/models.py:3924`)

**Correção importante no leitor do explicador:** `por_que_nao_fecha` decide `pode = not fio_vivo(ps)` em `app/chamados/services/explicador.py:137`; C7 registrado exige também `not respostas_sem_veredito()` em `app/chamados/models.py:1853`. Seu destino é C7, não apenas C15. Divergência efetiva de produção: [nao verificado].

**Exceções mantidas na projeção pelo aval**, também registrado em `app/docs/PROMPTS.md:2034`:

- A03 reabertura do continente: `app/chamados/models.py:2126`.
- A05 ramo assinatura: `app/chamados/models.py:3103`; não estender a exceção a todo consumidor de C.
- A06 identidade: `app/chamados/models.py:3168`; consumidor explícito C19 em `app/chamados/services/tarefa_do_dia.py:230`.
- A17 pendência técnica: `app/chamados/services/materializacao.py:855`; explicação precede efeito em `app/chamados/services/explicador.py:85`.
- A18 recusa técnica: `app/chamados/services/materializacao.py:1088`, `:1097`; não converter automaticamente a string em DP `RECUSA`.
- A20 conferência: `app/chamados/services/validacao.py:662`, `:665`; gate de plantio em `app/chamados/services/materializacao.py:1198`. A pergunta “conferência vale após mudar o par?” está expressamente diferida pelo anexo; resposta material: [nao verificado].

**DECISÕES candidatas para os leitores sem juiz específico registrado — nenhuma autoriza juiz novo:**

1. **Atividade/aceite:** decidir se o indicador conserva “uma pergunta atualmente validada/respondida” ou passa a “todos os atos do período”. Hoje `placar_equipe` conta perguntas com V corrente (`app/chamados/services/verbos.py:239`), `montar_contexto_painel._jan_val` conta V corrente (`app/chamados/services/painel.py:260`, `:268`), e `placar_aceite` declara respostas posteriores à cobrança (`app/chamados/services/verbos.py:253`). A sequência conserva atos sucessivos (`app/chamados/models.py:3840`), portanto contar DP indiscriminadamente pode alterar o significado. Juiz específico para esse indicador: [nao verificado]; o bloco chamado registrado não o contém (`app/core/juizes.py:272`).
2. **Rótulo de estado/resultado:** `PerguntaDisputa.estado` é fase por projeção, enquanto C19 é exigibilidade; não há equivalência global demonstrada. Consumidor concreto: `app/chamados/services/explicador.py:72`. Juiz específico para conservar exatamente os rótulos de fase após a retirada: [nao verificado]. A tarefa já tem razões distintas e desconhecimento (`app/chamados/services/tarefa_do_dia.py:128`, `:141`), portanto a solução pode ser apenas apresentar essas respostas, mas essa escolha precisa estar explícita no brief.
3. **Cache/ETag:** `marcador_do_placar` agrega `Max(R,C,V)` (`app/chamados/services/painel.py:66`); substituir por máximo de DP sozinho não cobre mudanças dos efeitos. Não é pergunta de lei por si: é dependência técnica a resolver com fontes canônicas já existentes, mantendo a invalidação dos consumidores (`app/chamados/services/painel.py:58`, `:80`).

Não recomendo contar automaticamente essas três DECISÕES como três perguntas de lei: a terceira é técnica; as duas primeiras podem ser resolvidas por manter os significados já documentados, mas falta verificar todos os cortes aplicáveis. Quantidade final de perguntas sem lei escrita: [nao verificado].

## 11/11 09:05 · sessao 336f1ebe2ad2 · - · - · origem=Ronald (tmux)

Evidências do item 4, somente pelo código lido:

| Ação | Template/botão | Porta e aplicação ao estado fornecido |
|---|---|---|
| Validar | `app/templates/chamados/partials/modal_fio.html:279`–284; detalhe em `_pergunta_card.html:92`–97 | **Aceita tentar**: resposta presente e validação nula passam por `app/chamados/services/validacao.py:393`–407. Executa `_materializar_uma` em `:461`–466; recusa de materialização devolve a validação, portanto **aceitar o clique não prova gravar**. |
| Rejeitar resposta | `modal_fio.html:290`–298 | **Aceita com motivo**: `validacao.py:561`–579 recusa motivo vazio, validação existente ou pergunta já pendente. Nenhuma dessas recusas corresponde às duas, desde que informado motivo. Reabre para correção em `:584`–594. **Move para o colaborador; não conclui o marco.** |
| Corrigir | `modal_fio.html:300`–312 | **Aceita hora e motivo**: `app/chamados/services/acoes_disputa.py:334`–351 exige formato HH:MM, motivo, pergunta não validada, disputa aberta e tipo `hora`; ambas correspondem aos campos fornecidos. Registra resposta em `:370`–374 e chama validação/materialização em `:379`–383. **Conclusão depende do resultado dessa porta.** |
| Regularizar disputa e fechar | `modal_fio.html:353`–359, condicionado a `todas_respondidas` em `:334` | **Não é escape das duas não validadas**: `acoes_disputa.py:139` chama materialização das validadas. A porta final recusa resposta sem veredito em `app/chamados/models.py:1886`–1894. A sonda diz `pode_fechar=False`, “30 aguardando sua validacao”. Exibição desse botão global, dependendo das demais perguntas: **[não verificado]**. |
| Resolver dia | `app/templates/ponto/partials/_veredito_dia.html:6`, `:17`, `:21` | **Não oferecido enquanto houver resposta viva**: `app/colaboradores/services/calendario.py:472`–477 marca o dia com pergunta viva e resposta; `:738`–739 torna falso `pode_ultimo_recurso`. Template só inclui o botão se `resolver_dia_oferecido` em `app/templates/colaboradores/partials/_calendario_grade.html:125`–126. A view existe em `app/ponto/views.py:3312`, mas existência da view não constitui clique disponível. |
| Registrar Batida pelo Django admin | Cadastro em `app/ponto/admin.py:12` | **Recusa inclusão/alteração/exclusão**: herda `SoLeitura`; `app/core/admin.py:59`–66 retorna `False` para as três permissões. |
| Regularização externa | `app/chamados/views_regularizacao.py:42`, `:74`, `:154` | Pode criar proposta de lote com motivo, adicionar horas e enviar ao colaborador; não há gate pelo estado destas perguntas nessas views. **Não prova conclusão**: exige confirmação/materialização e a sonda não traz lote nem seu resultado **[não verificado]**. |

Os três verbos do modal operam sob o **chamado-destino 35664**: `app/chamados/services/fio.py:116`–117 usa `chamado_destino_id` antes do chamado-container. As guardas da porta do modal conferem essa âncora em `:595`–599 para validar e `:632`–639 para rejeitar/corrigir.

O detalhe exibe **“⚠ aguardando validacao”** quando respondida e não validada (`app/templates/chamados/partials/_pergunta_card.html:23`–25), seguido da resposta e horário (`:29`–32). O explicador retorna `aguarda_validacao`, dono admin e “Validar ou recusar…” no ramo `app/chamados/services/explicador.py:77`–81, exatamente como a sonda das duas. No modal, `pendencia_admin=''` não entra no badge “Pendencia do admin” (`modal_fio.html:222`–227); validação nula também não entra em “Validada” (`:228`–229).

**Declaração em texto não oferece fuga comprovada:** `app/chamados/services/declaracao_texto.py:668` recusa somente pergunta já validada; em `:687`–696 conserva a resposta já existente e a valida pela mesma porta. Existência de followup/proposta aplicável não veio na sonda **[não verificado]**.

Conclusão delimitada: **há cliques que movem o atendimento**, especialmente Rejeitar; **não ficou provado, com os dados fornecidos, um passo a passo que conclua as duas mantendo as mesmas horas e batidas**. Alterar horas, inverter/retratar batidas ou lançar ausência requer fato que a sonda não estabelece **[não verificado]**.

## 11/11 09:04 · sessao d80e2ea10920 · - · - · origem=Ronald (tmux)

**Escada do explicador.** Para ambas, `validada_em=None`, `materializada_em=None` e `respondida_em` preenchida levam a `RESPONDIDA`; isso independe de `jul­gado_em`/`veredito_lavrado` nesta propriedade (`app/chamados/models.py:3041–3048`). O explicador retorna antes das verificações de `pendencia_admin` e materialização: `aguarda_validacao`, dono `admin`, motivo `colaborador respondeu 01:00` / `02:00` e ação `Validar ou recusar a resposta "01:00".` / `"02:00".` (`app/chamados/services/explicador.py:77–93`). São os retornos literais fornecidos pela sonda.

**Tela ADMIN, por template.**

- No detalhe, badge `⚠ aguardando validacao`, resposta e data (`app/templates/chamados/partials/_pergunta_card.html:23–32`); com `pode_agir`, oferece validar e rejeitar com motivo (`:42`, `:92–118`). O rótulo exato da validação depende de `leitura_da_resposta`: `Validar resposta`, rótulo de `fr`, ou `Validar assim mesmo` (`:43–98`). A classificação concreta não veio na sonda: **[não verificado]**.
- No modal, mostra `Resposta: 01:00/02:00` e envio; não entra nos ramos de pendência ou validada porque os respectivos campos estão vazios (`app/templates/chamados/partials/modal_fio.html:217–239`). Oferece validar (`:263–285`), rejeitar com motivo e corrigir hora com motivo, enquanto a disputa estiver aberta (`:289–315`). Dado: disputa 5644 `fechada_em=None`.
- O modal também consulta `estado_aplicacao_pergunta`; pode mostrar aviso de paridade (`app/chamados/services/fio.py:334–338`; `app/templates/chamados/partials/modal_fio.html:240–247`). O cálculo consulta novamente vizinhas e propositor (`app/chamados/services/materializacao.py:444–456`); seu retorno atual não foi raspado, portanto o aviso efetivamente presente é **[não verificado]**, apesar da recusa gravada.
- Em “detalhes do caso · trilha, sistema e celula”, o resumo literal da sonda será `Nao fecha: 30 aguardando sua validacao.` e as duas aparecem com `Sua vez:` e sua ação (`app/templates/chamados/partials/modal_fio.html:430–445`; `app/chamados/services/explicador.py:148–162`). Não atribuir as outras 28 pendências a dados não mostrados.

**Recado do COLAB: distinguir duas portas.**

1. **Questionário da pergunta.** O formulário está em `app/templates/chamados/partials/_card_atendimento.html:77–78`. Como já respondeu, mostra “Resposta enviada”, resposta/data e “Aguardando a supervisao conferir. Se precisar corrigir, e so responder de novo abaixo.” (`:94–102`); botão `Reenviar resposta` (`:154–155`). A view é `app/chamados/views.py:1228`, URL em `app/chamados/urls.py:17`.
   - Primeiro exige `pergunta_viva` (`views.py:1239–1246`).
   - A recusa “Esta resposta ja foi regularizada e virou batida oficial...” exige `materializada_em is not None` e pergunta morta (`:1257–1263`), condição que **não se aplica** aos carimbos dados.
   - HH:MM sem `resposta_tocou=1` devolve “Escolha a hora no relogio antes de enviar.” (`:1274–1279`).
   - Depois chama o core e devolve erro ou “Resposta registrada.” (`:1283–1287`). O core não veta por recusa de paridade, `recusa_motivo` ou resposta anterior; valida formato e grava nova resposta (`app/chamados/services/disputa_emissao.py:1432–1449`, `:1467–1508`).

2. **“Enviar hora” do chamado.** View `responder_chamado_colaborador`, `app/chamados/views.py:1041`; URL `app/chamados/urls.py:42`; form `app/templates/chamados/partials/_card_atendimento.html:219`, botão `:387`, área de mensagem `:389`.
   - O form só aparece para chamado aberto/em análise sem GLPI, de furo, **sem pergunta viva**, e sem regularização pela supervisão (`:203–219`). A presença desse formulário para estas perguntas é **[não verificado]**.
   - A view exige vínculo/próprio chamado (`views.py:1043–1046`). A checagem tardia não intercepta `em_analise`, pois é estado vivo (`app/chamados/services/resposta_tardia.py:35–36`; `app/chamados/catalogo/motor.py:24`).
   - `batida_ausente` está no conjunto `_FURO` (`app/chamados/catalogo/modulos.py:235–236`), passando ao ramo de declaração (`views.py:1060`).
   - Sem toque, recusa em `views.py:1085–1088`. Com toque, chama `declarar_hora_colab` (`:1090–1093`).
   - A declaração exige módulo, HH:MM, motivo válido quando fornecido, dia, ata e casamento com marco faltante (`app/chamados/services/declaracao_texto.py:320–344`). Escolhe a pergunta canônica do universo do fato (`:230–254`, `:363`).
   - A recusa “Esta batida ainda nao esta no seu espelho, mesmo com a resposta anterior ja validada...” depende de `_resposta_validada`, isto é, `validada_em is not None` (`:227`, `:378`, `:412–416`): **não é sustentada pelos registros das duas**.
   - Se a porta reusar estas mesmas perguntas e receber respectivamente `01:00` e `02:00`, cai em `respondida_em` preenchida + mesma hora e devolve **“Horario ja informado; aguardando a supervisao.”** (`:417–418`). A view devolve essa mensagem (`views.py:1100`); o JavaScript escreve “Resposta enviada” e `d.msg` (`app/templates/chamados/meu_atendimento.html:424–444`).

**Limite decisivo:** a sonda não mostra tentativa HTTP nem recado observado: **[não verificado]**. Também não mostra `CelulaDia.veredito`, `veredito_via`, tranca, ata completa ou universo canônico. `aguarda_validacao` não prova `pergunta_viva=True`, porque a propriedade retorna `RESPONDIDA` mesmo no último fallback (`models.py:3048`). O juiz de vitalidade consulta tranca/célula (`app/chamados/juizes.py:1325–1340`); sem decisão explícita admin e sem tranca, `fato=False` impede concordância com a luz (`:1290–1291`) e a resposta segue viva (`:1341–1353`).

## 11/11 09:04 · sessao b373ddb098f3 · - · - · origem=Ronald (tmux)

Ações sobre as batidas vizinhas:

| Ação | Botão/tela | Porta e aceitação |
|---|---|---|
| Inverter E/S | Clique no horário → **Inverter tipo**, em `app/templates/colaboradores/partials/_calendario_grade.html:121`. Exige `pode_veredito` e batida não classificada como órfã nessa apresentação. | `app/ponto/views.py:3260`, guarda POST + `regularizar_batida` em `:3263`. Serviço `app/ponto/services/flip_batida.py:10`: recusa retratada (`:15-16`), sem autor (`:19-20`), competência fechada (`:21-24`), tipo fora E/S (`:25-27`). As duas batidas passam nas condições conhecidas de tipo e retratação; permissão, competência e apresentação na tela **[não verificado]**. |
| Retratar | Clique no horário → escolha motivo → **Não foi trabalho (retratar)**, em `app/templates/colaboradores/partials/_calendario_grade.html:121`. | `app/ponto/views.py:3274`, guarda POST + permissão em `:3277`. Serviço `app/ponto/services/retratar_humano.py:150` exige humano/permissão (`:131-147`) e motivo cadastrado ativo (`:166-171`). Não exige estado específico das perguntas nem consulta competência nesse caminho. Motivo ativo/permissão **[não verificado]**. |
| Desfazer retratação | **↶ desfazer**, em `app/templates/colaboradores/partials/_calendario_grade.html:121`. | `app/ponto/views.py:3286`. As batidas dadas têm `retratada_em=None`, logo a porta recusa “Esta marcacao nao esta retratada”, em `app/ponto/services/retratar_humano.py:197-198`. |

**Retratar não prova retirar a recusa de paridade:** `_vizinha_mesmo_tipo` consulta `Batida.objects` sem filtro por `retratada_em` ou `aprovada`, em `app/chamados/services/materializacao.py:94-104`; `Batida` não declara manager próprio em `app/ponto/models.py:8`. A retratação grava `aprovada=False` e `retratada_em`, em `app/ponto/registro_batida.py:267-281`, mas a seleção da vizinha continua enxergando esse registro pelo código citado. Não há prova fornecida de que #133794/#133756 estejam erradas ou não representem trabalho **[não verificado]**.

**Resolver dia:** o botão é condicionado a `dia.resolver_dia_oferecido`, em `app/templates/colaboradores/partials/_calendario_grade.html:125-126`; oferece **Trabalhou no horário da escala**, **Não trabalhou (falta)** e **Justificado**, em `app/templates/ponto/partials/_veredito_dia.html:21-23`.

A tela restringe o último recurso a dia anterior à competência atual e sem pergunta viva com resposta: `app/colaboradores/services/calendario.py:472-477`, `:738-739`; o predicado ainda exige passado/status que pede decisão em `:97-104`. Assim, não basta localizar a view para afirmar que o admin consegue clicar nessa ação para o estado dado; disponibilidade efetiva **[não verificado]**.

A view `app/ponto/views.py:3312` não recusa por `respondida_em`, `via_resolucao` ou `pendencia_admin`; exige POST/permissão (`:3315`), datas/veredito válidos (`:3321-3331`) e chama o serviço (`:3340`). O serviço pode recusar ausência de trabalho previsto (`app/ponto/services/veredito_celula.py:242-243`), ausência de marcos (`:248-249`) ou nenhuma combinação coerente (`:308-310`). DNA/ata completa e resultado da simulação **[não verificado]**. Quando efetivamente bem-sucedido, fecha perguntas do dia em `:179-197` e `:345-351`, usando `PerguntaDisputa.fechar_por_admin`, em `app/chamados/models.py:3050-3064`.

**Regularização externa:** existe adicionar par/batida pelo fluxo **+ Regularizar batidas** (`app/templates/chamados/fio_colaborador.html:15`) → **Criar lote e adicionar dias** (`app/templates/chamados/regularizacao_painel.html:14-18`) → **Adicionar** com opção de intervalo (`app/templates/chamados/regularizacao_editar.html:45-58`) → **Enviar ao colaborador para confirmação** (`:63-68`). A inclusão do item aceita datas/horários válidos na view `app/chamados/views_regularizacao.py:74-131`; contudo a materialização **RECUSA** disputa aberta em `app/chamados/services/regularizacao_ext.py:58-65`. Aplicação ao dado: disputa #5644 tem `fechada_em=None`; portanto esse fluxo não materializa enquanto permanecer assim — sonda, bloco `DisputaSupervisao pk=5644`.

Não localizei uma view chamada `batida_registrar` nem botão direto equivalente no espelho; não atribuir esse nome como porta existente **[não verificado]**.

## 11/11 08:54 · sessao 00b37e6c645a · FIO-E0-V4-E3 · construtor · origem=Code

Corrigidos somente os dois arquivos autorizados: [goldens e comentário](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/test_x1_folga_sem_adesao.py:31) e [registro de linhas](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/core/tests/test_medida_turno_do_par.py:315).

```diff
--- app/ponto/tests/test_x1_folga_sem_adesao.py
+++ app/ponto/tests/test_x1_folga_sem_adesao.py
@@ -31,5 +31,5 @@
-# Hashes da funcao real da base 52a0022e, com o DNA de _criar_celula.
+# Medido com o cartorio da base 52a0022e9 (logs/FIO-E0-V4-E3/e3fix9_golden.log).
 GOLDEN_BASE = {
-    9: 'c3b1e42811726eeafe415ba8a3664e7bda8f81c8bbd7d747fb6d0dcb59001c3e',
-    10: 'c9cd4c401241f99cc08abbcb91060fd7b7f9a1243c21047382cc966e89438157',
+    9: '9814ac9bff8c08a98803213eec0f1f90630b235ab62828c68f48d53753ff5c73',
+    10: '8b9eae698d8d028a70ca3eb1048e1c56fbd7b375758630eb151c4a109d8bb569',
 }
--- app/core/tests/test_medida_turno_do_par.py
+++ app/core/tests/test_medida_turno_do_par.py
@@ -315 +315 @@
-                       ('ponto/services/cartorio.py::_julgar_colab_corpo', 'B1'): [1172, 1322],
+                       ('ponto/services/cartorio.py::_julgar_colab_corpo', 'B1'): [1191, 1344],
```

O censo pelo instrumento do selo encontrou **um único item registrado** de `cartorio.py`: `derivacoes_de_dia`, `_julgar_colab_corpo`, B1. Suas ocorrências atuais são [cartorio.py:1191](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/services/cartorio.py:1191) e [cartorio.py:1344](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/services/cartorio.py:1344). A comparação integral dos registros passou pela [catraca](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/core/tests/test_medida_turno_do_par.py:539). [nao verificado]

Comandos executados em `app/`:

```bash
# rc=0; 24 testes [nao verificado]
PYTHONDONTWRITEBYTECODE=1 python3 -B -m unittest ponto.tests.test_e3_nb_impressao_memoria -v

# rc=0; 24 testes [nao verificado]
PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.x1_mutantes --sem-banco --modo green

# Cada comando abaixo: rc=1, morto por assertion, ERROR=0
PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.x1_mutantes --sem-banco --modo mutante --mutante sem_horizonte_x1
PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.x1_mutantes --sem-banco --modo mutante --mutante horizonte_na_10
PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.x1_mutantes --sem-banco --modo mutante --mutante literal_incondicional
PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.x1_mutantes --sem-banco --modo mutante --mutante sem_fonte_no_gancho
PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.x1_mutantes --sem-banco --modo mutante --mutante so_acusador
PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.x1_mutantes --sem-banco --modo mutante --mutante parcial_tira_marcador
```

Resultados segundo o [executor sem banco](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/x1_mutantes.py:77).

RED da catraca antes da edição: **rc=1**. Após a edição: **rc=0**. Mutante restaurando o registro antigo somente em memória: **rc=1**, por assertion em [test_medida_turno_do_par.py:540](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/core/tests/test_medida_turno_do_par.py:540). Comando do mutante; para GREEN, retirar `MUTANTE=registro_antigo`:

```bash
MUTANTE=registro_antigo PYTHONDONTWRITEBYTECODE=1 python3 -B - <<'PY'
import os, pathlib, types, unittest
p = pathlib.Path('core/tests/test_medida_turno_do_par.py')
suite = types.ModuleType('suite_turno') [nao verificado]
suite.__file__ = str(p.resolve()) [nao verificado]
source = p.read_text().replace(
    'from django.test import SimpleTestCase',
    'from unittest import TestCase as SimpleTestCase')
exec(compile(source, str(p), 'exec'), suite.__dict__) [nao verificado]
if os.environ.get('MUTANTE') == 'registro_antigo':
    suite.LINHAS_REGISTRADAS['derivacoes_de_dia'][ [nao verificado]
        ('ponto/services/cartorio.py::_julgar_colab_corpo', 'B1')
    ] = [1172, 1322]
unittest.main(
    module=suite, [nao verificado]
    defaultTest='CatracaTurnoDoParTest.test_CATRACA_linhas_reais_congeladas_exigem_registro_no_mesmo_commit',
    verbosity=2)
PY
```

A busca `rg -n 'c3b1e428|c9cd4c40' .` terminou com **rc=1**, sem ocorrências restantes; `git diff --check`, **rc=0**. ORM c4/c4b após esta edição: **[nao verificado]**, reservado à sessão conforme solicitado.

## 11/11 08:50 · sessao eb804156157f · SELO-ESPERA · verificador · origem=Code

**REPROVADO para pouso:** a linha nova não garante excluir exatamente `bin/tests/`: interpreta `$RAIZ` como regex BRE e como parte da sintaxe do `sed`, permitindo conservar fixtures, excluir lançadores reais ou falhar. (`bin/selo_espera_por_processo.sh:48`)

1. **Bloqueador — exclusão inexata para caminhos especiais.** No caminho desta cópia, os dois produtores entregam prefixos compatíveis: o `grep` recebe `$RAIZ/bin`, e o Python imprime `<raiz>/<arquivo>:<linha>:<texto>`. O filtro ancorado exclui `bin/tests/`, inclusive subpastas, e preserva `bin/testsX/`. O scratch é anexado **depois**, portanto não passa pela exclusão. (`bin/selo_espera_por_processo.sh:34`, `:42`, `:45`, `:48`, `:60`; `bin/censo_codigo_py.py:48`)

   Entretanto, a expressão da linha 48 tem estes contraexemplos, reproduzidos em memória:
   
   - `RAIZ=/tmp/obra[1]`: conserva `/tmp/obra[1]/bin/tests/f.sh`; `[1]` é interpretado como classe BRE. (`bin/selo_espera_por_processo.sh:48`)
   - `RAIZ=/tmp/obra.*`: apaga `/tmp/obra.*/bin/lancadores/bin/tests/real.sh`, embora esse arquivo esteja fora do `bin/tests/` da raiz; `.*` absorve os componentes intermediários. (`bin/selo_espera_por_processo.sh:48`)
   - `RAIZ=/tmp/obra#1`: o `#` encerra prematuramente o endereço e o `sed` retorna erro; o script não verifica esse retorno e continua contando. (`bin/selo_espera_por_processo.sh:33`, `:48`, `:49`)

   Para satisfazer a exigência de exclusão exata, o prefixo precisa ser comparado literalmente ou escapado corretamente, com falha do filtro tratada. (`bin/selo_espera_por_processo.sh:48`)

2. **Contagens — coerentes com as linhas restantes, condicionadas ao filtro correto.** `n_bin` é medido após a exclusão; sem scratch, `total=n_bin` e `n_scr=nao_achado`. Com scratch, `total` é recontado após anexá-lo, e `n_scr=total-n_bin`. O limite de 30 linhas só afeta o diagnóstico; a saída final depende de `total==0`. Portanto, a aritmética permanece coerente, mas contabiliza o universo errado nos contraexemplos do item 1. (`bin/selo_espera_por_processo.sh:49`, `:57`, `:58`, `:60`, `:64`, `:65`, `:68`, `:69`, `:72`)

3. **Leitores — nenhuma dependência encontrada de ocorrências em `bin/tests/`.** A régua consome o retorno do selo, mostra seu diagnóstico e grava `FALHOU` antes do atalho “JA VERDE”; seu loop de testes é separado. (`bin/regua.sh:70`, `:72`, `:76`, `:84`, `:138`)

   O pre-push não chama diretamente esse selo nem o loop completo de testes de host; consome o carimbo/hash da régua para decidir pular o arsenal. (`bin/pre-push.sh:14`, `:15`, `:52`, `:66`, `:67`, `:70`)

   O placar extrai apenas número/`?`, transforma ausência em `?`, contabiliza `?` como 1 e soma o resultado ao `TOTAL`. O teste do placar preserva esse parser, sem exigir caminhos de fixtures. (`bin/placar_code.sh:164`, `:165`, `:170`, `:558`; `bin/tests/test_placar_conta_o_retido_do_lastro.sh:500`)

   O teste do próprio selo exige uma ocorrência sintética no scratch e zero quando resta apenas comentário. O consumidor CPUSET chama o censo genérico e aplica seu próprio filtro de `tests/`. (`bin/tests/test_selo_espera_processo_morde.sh:16`, `:17`, `:19`, `:22`, `:28`, `:37`, `:39`; `bin/tests/test_cpuset_de_teste.sh:26`, `:27`, `:30`)

   No gerador do ESTADO, os placares incorporados vêm de `placar_estrutural.py` e `espelho_verdade.py`; não há leitura direta deste contador nesse caminho. Conteúdo do ESTADO publicado: [não verificado]. (`bin/gerar_estado.py:73`, `:140`, `:387`; `bin/relato.sh:38`)

4. **Ocorrências em `bin/tests/` — nenhuma chamada real encontrada.** As oito linhas com o literal `pgrep` são:
   
   - Comentário: `bin/tests/test_handoff_sessao.sh:30`.
   - Comentários: `bin/tests/test_trava_teste.sh:44`, `:46`, `:49`, `:73`, `:74`, `:75`.
   - Entrada de teste: `bin/tests/test_disjuntor.sh:568`; `_plc` monta JSON e `_prec` o entrega por stdin ao disjuntor, sem executar o comando contido nele. (`bin/tests/test_disjuntor.sh:519`, `:520`)

   Os nomes montados também servem a testes: busca no código do handoff; filtro e geração de fixture da trava; geração dos arquivos sintéticos do teste do selo. Não constituem espera real por processo. (`bin/tests/test_handoff_sessao.sh:24`, `:118`; `bin/tests/test_trava_teste.sh:13`, `:53`, `:78`, `:79`; `bin/tests/test_selo_espera_processo_morde.sh:11`, `:16`, `:17`, `:19`)

   A docstring “conta igual” continua verdadeira para o **censo bruto**, que ignora apenas `__pycache__`; deixou de descrever o universo final deste selo, que filtra depois. Há descompasso documental, mas a implementação compartilhada do censo permanece intacta. (`bin/censo_codigo_py.py:40`, `:41`, `:43`, `:48`; `bin/selo_espera_por_processo.sh:45`, `:48`)

5. **Classes de reprovação.** A exclusão é uma regra de diretório, sem lista de arquivos individuais. O contador continua significando quantidade de linhas encontradas no universo selecionado; não identifiquei um segundo significado introduzido pelo diff. (`bin/selo_espera_por_processo.sh:48`, `:49`, `:64`, `:68`)

   A mordida permanece para lançadores `.sh`, `.py` e subpastas fora de `bin/tests/`, sob uma raiz sem os problemas do item 1; o scratch também permanece contado. Porém, o contraexemplo com `.*` permite esconder um lançador real, caracterizando perda de mordida nesse caso. O teste versionado cobre apenas scratch `.sh`; execução dos demais mutantes citados no anexo: [não verificado]. (`bin/selo_espera_por_processo.sh:42`, `:45`, `:48`, `:60`, `:72`; `bin/tests/test_selo_espera_processo_morde.sh:17`, `:19`)

6. **Fila, preexistente — confirmado o crash Python aceito.** Na linha 46, `PIPESTATUS[0]` corresponde ao `python3` imediatamente anterior. A guarda aceita tanto 0 quanto 1; um `ImportError` antes de imprimir resultados deixa zero linhas Python, oculta o traceback e segue. Se não houver outros achados, o selo termina verde. Isso decorre das linhas anteriores ao diff e fica fora da cura. (`bin/censo_codigo_py.py:34`, `:47`, `:48`; `bin/selo_espera_por_processo.sh:45`, `:46`, `:47`, `:58`, `:72`)

_3752 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
