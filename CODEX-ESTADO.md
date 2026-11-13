# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 22:21** · publicado em **12/11/2026 22:22** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 22:21 · sessao 847846c38a9e · - · - · origem=Ronald (tmux)

Censo atual: **42 leituras em 37 funções**, igual ao teto; o visitante anterior reconstruído em memória e o novo produziram listas idênticas de `(função, linha, atributo)` para todos os itens do universo. Não observei duplicação nem perda na árvore real. Resultado da execução efêmera: [nao verificado]. O universo, teto e comparação integral estão em `app/ponto/tests/test_cadastro_futuro_c5e.py:326`, `:333`, `:372`, `:531`.

Exemplos conferidos: a leitura original de `reconciliar_apos_vinculo` permanece uma (`app/colaboradores/services/pos_vinculo.py:56`); os dois carimbos de regeneração permanecem dois (`app/ponto/portas/celula.py:263`, `:308`); `julgar_celula` continua com três leituras, e a conversão externa de `_tz.now()` não acrescenta quarta (`app/ponto/services/cartorio.py:2016`, `:2037`, `:2040`; regra de conversão em `app/ponto/tests/test_cadastro_futuro_c5e.py:503`).

| Símbolo | Quem escreve | Quem lê / dependências |
|---|---|---|
| `TETO_RELOGIO` | Literal de módulo, único binding encontrado: `app/ponto/tests/test_cadastro_futuro_c5e.py:333`. | `_furos` compara por função e usa zero para função nova (`:528`, `:529`); o teste principal soma seus valores e exige igualdade integral com o censo (`:535`, `:537`). Os metatestes dependem dele indiretamente via `_furos` (`:561`, `:566`, `:576`, `:629`). |
| `TOTAL_RELOGIO` | Literal `42`, único binding encontrado: `app/ponto/tests/test_cadastro_futuro_c5e.py:372`. | Apenas o teste principal: limita soma medida e exige soma dos tetos igual ao total (`:534`, `:535`). |
| `leituras_de_relogio` | Definição da função, único binding encontrado: `app/ponto/tests/test_cadastro_futuro_c5e.py:383`. | `censo_relogio` (`:520`); selo de portas sem leitura de dia (`:543`); selos de `cadastro_futuro`, `regen_exportada` e auxiliares de vínculo sem leitura (`:546`, `:550`); metatestes de conversão/rebinding e não duplicação (`:617`, `:678`, `:681`). |

A busca em toda a cópia encontrou os três símbolos apenas nesse arquivo; não há outro selo com referência textual direta. Resultado da busca efêmera: [nao verificado]. As dependências indiretas internas estão nas chamadas de `censo_relogio` e `_furos` acima (`app/ponto/tests/test_cadastro_futuro_c5e.py:532`, `:561`, `:566`, `:576`, `:629`).

Executei `PYTHONPATH=app python3 -B -m unittest ponto.tests.test_cadastro_futuro_c5e ponto.tests.test_cadastro_futuro_sem_banco`: **66/66 OK**, sendo **23 C5E + 43 sem banco**; execução sem artefato persistido: [nao verificado]. O teste C5E que exige o censo completo é `app/ponto/tests/test_cadastro_futuro_c5e.py:531`; o harness declara ausência de banco/escrita e mutações em memória em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:3`.

## 12/11 22:19 · sessao 65804c420acf · E3-FORMA-SQL · verificador · origem=Code

REPROVA

1. **A continência de `q_vivas` continua quebrada para pendência truthy não booleana.** O Python reconhece `pendente` por truthiness; o SQL reconhece especificamente o booleano `True`. O modelo admite JSON sem tipagem interna, e a porta da lavra copia a ata sem validar essa chave. (`app/ponto/services/lastro.py:150`; `app/chamados/models.py:3403`; `app/escala/models.py:1778`; `app/ponto/portas/celula.py:995`)

   Permanece o contraexemplo da volta 2: pergunta não validada, destrancada, sem resposta nem materialização, sem via terminal; célula `concorde` pela via `cartorio`, sem fato no marco, com:

   ```json
   {"lampadas":[
     {"hora":"14:20","tipo":"S","acesa":null,"pendente":"sim"}
   ]}
   ```

   O Python reconhece provisoriedade e chega a **VIVA** pela ausência de materialização. Pela leitura do Q, o SQL não reconhece a provisoriedade, considera a célula absolvente e exclui a pergunta. Execução desse contraexemplo no PostgreSQL: **[não verificado]**. (`app/chamados/juizes.py:1452`; `app/chamados/juizes.py:1466`; `app/chamados/models.py:3408`; `app/chamados/models.py:3413`)

   O escritor canônico produz `pendente=True`; não demonstrou produzir `"sim"`. Entretanto, isso não fecha o domínio admitido pelo modelo nem cura a divergência. Nenhum teste acrescentado cobre esse estado. (`app/ponto/services/cartorio.py:915`; `app/escala/models.py:1778`; `app/chamados/tests/test_e3_forma_sql_do_juiz.py:221`)

   **A justificativa da triagem excede a sonda fornecida:** a saída integral mostra `pendente_bool_True=54`, mas não apresenta contador de pendentes não booleanos. O alegado “0 não-booleano”, sua medição e a ausência desse estado em produção são **[não verificado]**.

2. **A catraca continua permitindo uso booleano como VALOR; também permite sua entrada dentro de um declarado sem ficar vermelha.** A busca de uso de valor percorre descendentes com `ast.walk`, incluindo corpos que não serão executados. (`app/chamados/tests/test_e3_catraca_respondedores.py:209`; `app/chamados/tests/test_e3_catraca_respondedores.py:218`)

   Contraexemplo reproduzido em memória, classificado **VALOR**:

   ```python
   def novo(p):
       return p.respondida_em is not None and bool(
           lambda: p.respondida_em.isoformat()
       )
   ```

   A lambda nunca é chamada; `bool(lambda: ...)` é verdadeiro. O resultado é exatamente “há carimbo?”, sem ler seu instante. A classificação decorre de encontrar `.isoformat()` dentro da lambda. Não depende de `True or ...` nem de dobra de constante. (`app/chamados/tests/test_e3_catraca_respondedores.py:192`; `app/chamados/tests/test_e3_catraca_respondedores.py:209`; `app/chamados/tests/test_e3_catraca_respondedores.py:229`)

   Inserido em memória no início de `_slot_esperado`, como condição de um `return`, manteve **23 respondedores, cinco leituras declaradas e nenhuma declaração morta**. Os **14 testes da catraca passaram também com esse mutante**: VALOR é decidido antes de DECLARADO, e a contagem declarada só soma DECLARADO. (`app/chamados/juizes.py:341`; `app/chamados/tests/test_e3_catraca_respondedores.py:321`; `app/chamados/tests/test_e3_catraca_respondedores.py:332`; `app/chamados/tests/test_e3_catraca_respondedores.py:383`)

   Há ainda um contraexemplo ORM, também reproduzido como **VALOR**:

   ```python
   return qs.filter(
       Q(respondida_em__isnull=False)
       | Q(respondida_em__gte=d)
   ).exists()
   ```

   Para `d` válido, o segundo ramo é subconjunto do primeiro: a consulta pergunta somente se existe carimbo. O classificador encontra o lookup de ordem na cadeia e concede VALOR ao `isnull=False`, sem distinguir a disjunção. (`app/chamados/tests/test_e3_catraca_respondedores.py:292`; `app/chamados/tests/test_e3_catraca_respondedores.py:296`)

   **A contagem global também permite compensação entre declarados.** Reproduzi inserir uma leitura booleana crua em `_slot_esperado` e substituir o pré-filtro de `linhas_da_projecao_sem_linha` por `ps = list(perguntas)`: entra uma leitura DECLARADO, sai outra, permanece a segunda leitura daquele sítio. Continuam **23/5/nenhuma morta**, e os quatro testes de `ContadorRespondedoresTest` passam. O selo exige total global, sem distribuição por sítio. (`app/chamados/juizes.py:341`; `app/chamados/models.py:4010`; `app/chamados/models.py:4017`; `app/chamados/tests/test_e3_catraca_respondedores.py:67`; `app/chamados/tests/test_e3_catraca_respondedores.py:458`)

Informativos — conferência dos achados da volta 2:

| Achado | Cura no HEAD e teste que morde | [nao verificado]
|---|---|
| Pendência não booleana | **Não curado**, bloqueante 1. Permanecem truthiness versus `True`; nenhum RED correspondente foi acrescentado. (`app/ponto/services/lastro.py:150`; `app/chamados/models.py:3403`) |
| Agregado de outro queryset dentro de subconsulta | **Curado nos exemplos apresentados.** `_nos_da_cadeia` separa a cadeia recebida como argumento; `test_MORDE_classificador_volta3_subconsulta_not_e_serializacao` exige RESPONDEDOR para agregado/ordem externos e conserva VALOR na cadeia própria. (`app/chamados/tests/test_e3_catraca_respondedores.py:258`; `app/chamados/tests/test_e3_catraca_respondedores.py:506`) |
| Serialização com `not` no teste do `IfExp` | **Curada.** A guarda escolhe `orelse`; o mesmo teste compara o ramo correto com o invertido. (`app/chamados/tests/test_e3_catraca_respondedores.py:213`; `app/chamados/tests/test_e3_catraca_respondedores.py:521`; `app/chamados/tests/test_e3_catraca_respondedores.py:540`) |
| `str`/`repr`/`format`/f-string classificados como RESPONDEDOR | **Curados nos casos nomeados.** O reconhecimento está em `uso_valor`; há pares positivos e negativos no teste da volta 3. (`app/chamados/tests/test_e3_catraca_respondedores.py:195`; `app/chamados/tests/test_e3_catraca_respondedores.py:524`; `app/chamados/tests/test_e3_catraca_respondedores.py:542`) |
| Ramo morto concedendo VALOR | **Explicitamente não curado.** O comentário o descarta; os contraexemplos do bloqueante 2 mostram que a falha ultrapassa esse literal. (`app/chamados/tests/test_e3_catraca_respondedores.py:24`; `app/chamados/tests/test_e3_catraca_respondedores.py:209`) |
| `veredito=''` excluído indevidamente | **Cura preservada e cobertura ampliada.** O RED agora cruza materializada/não materializada e ausência/brancos; morde restringir a cura apenas às não materializadas. (`app/chamados/models.py:3408`; `app/chamados/tests/test_e3_forma_sql_do_juiz.py:221`) |
| Vizinho com texto não mordia a volta do bug de `''` | **Curado no selo:** novo par SEM texto exige vazio incluído e `concorde` excluído. Morde retirar a exceção de vazio e também alargar indevidamente para `concorde`. (`app/chamados/tests/test_e3_forma_sql_do_juiz.py:242`) |
| Mutante sem trim só comparava Q local | **Curado no selo:** agora patcha as duas faces reais e exige falha dos próprios testes de equivalência. (`app/chamados/tests/test_e3_forma_sql_do_juiz.py:131`; `app/chamados/tests/test_e3_forma_sql_do_juiz.py:144`) |
| U+1680 ausente da matriz | **Curado:** entrou na amostra e na cobrança de cobertura; as equivalências consultam as faces reais. (`app/chamados/tests/test_e3_forma_sql_do_juiz.py:35`; `app/chamados/tests/test_e3_forma_sql_do_juiz.py:98`; `app/chamados/tests/test_e3_forma_sql_do_juiz.py:89`; `app/chamados/tests/test_e3_forma_sql_do_juiz.py:93`) |
| Comentário ignorava leitura de resposta no degrau 6 | **Correção preservada:** reconhece materialização → slot. (`app/chamados/models.py:3393`; `app/chamados/juizes.py:1473`; `app/chamados/juizes.py:360`) |

As mordidas SQL acima foram conferidas pelo código; execução delas com banco nesta volta é **[não verificado]**. Os 14 métodos das duas classes da catraca foram executados sem banco e passaram; isso não cobre os mutantes sobreviventes apresentados. (`app/chamados/tests/test_e3_catraca_respondedores.py:408`; `app/chamados/tests/test_e3_catraca_respondedores.py:462`)

O aval item (2) está declarado assim:

| Sítio nomeado | Pergunta e autoridade declaradas | Leitura atual |
|---|---|---|
| `_slot_esperado` | Resposta já existia em `agora`? → `projecao_temporal_da_pergunta`. (`app/chamados/tests/test_e3_catraca_respondedores.py:54`) | Guarda de resposta posterior ao horizonte. (`app/chamados/juizes.py:348`) |
| `backfill_decisoes_pergunta.linhas_a_criar` | Houve ato de carimbo? → `DecisaoPergunta.linhas_da_projecao_sem_linha`. (`app/chamados/tests/test_e3_catraca_respondedores.py:57`) | Seleciona universo carimbado e delega correspondência. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:56`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:67`) |
| `DecisaoPergunta.linhas_da_projecao_sem_linha` | Houve ato sem linha correspondente? → `DecisaoPergunta`. (`app/chamados/tests/test_e3_catraca_respondedores.py:60`) | Preserva correspondência por ato e instante, inclusive texto vazio. (`app/chamados/models.py:4010`; `app/chamados/models.py:4017`; `app/chamados/models.py:4020`) |
| `curar_perguntas_em_limbo.Command.handle` | Carimbo sem trilha? → `carimbar_resposta_pela_trilha`. (`app/chamados/tests/test_e3_catraca_respondedores.py:63`) | Usa instante da nota e restringe reabertura às sem carimbo. (`app/chamados/management/commands/curar_perguntas_em_limbo.py:67`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:69`) |

**Sim**, declaração morta fica vermelha; **sim**, o novo sítio simples `is None`, fora dos quatro, sobe para 24 e fica vermelho. Há mutantes em fonte real para ambos. **Não**, isso não garante qualquer sítio booleano novo: depende do classificador, conforme bloqueante 2. (`app/chamados/tests/test_e3_catraca_respondedores.py:452`; `app/chamados/tests/test_e3_catraca_respondedores.py:479`; `app/chamados/tests/test_e3_catraca_respondedores.py:499`; `app/chamados/tests/test_e3_catraca_respondedores.py:420`)

`_define` verifica existência do endereço declarado; não prova que seu corpo responda à pergunta escrita. A meta continua sendo zero, com parada operacional em **23**. (`app/chamados/tests/test_e3_catraca_respondedores.py:387`; `app/chamados/tests/test_e3_catraca_respondedores.py:456`; `app/chamados/tests/test_e3_catraca_respondedores.py:45`)

Quanto a quem escreve e quem lê os campos, este é o mapa no HEAD; a reconstrução histórica integral de `0544c994d..d230b186f`, além do delta fornecido, é **[não verificado]**.

| Campo/valor | Quem escreve | Quem lê e significado |
|---|---|---|
| Par no nascimento | `_nascer`, recusando carimbo sem texto. (`app/chamados/models.py:2865`) | Juiz ilegal: carimbo presente e texto vazio após `strip`. (`app/chamados/juizes.py:1518`) |
| Par na resposta humana | `responder_pergunta_disputa_core`: valida texto, atribui texto/carimbo e registra ato. (`app/chamados/services/disputa_emissao.py:1432`; `app/chamados/services/disputa_emissao.py:1467`; `app/chamados/services/disputa_emissao.py:1511`) | C6: resposta com conteúdo; materializador: conteúdo como hora/declaração, recusando ausência de texto. (`app/chamados/juizes.py:1407`; `app/chamados/services/materializacao.py:359`; `app/chamados/services/materializacao.py:835`) |
| Par na resposta automática | `responder_automaticamente`, preservando texto existente e usando instante recebido. (`app/chamados/services/disputa_emissao.py:1649`; `app/chamados/services/disputa_emissao.py:1659`) | Mesmos leitores do par; o ato identifica resposta automática por fato entregue. (`app/chamados/services/disputa_emissao.py:1663`; `app/chamados/juizes.py:1518`) |
| Carimbo recuperado da trilha | `carimbar_resposta_pela_trilha`: texto já existe, instante vem da nota. (`app/chamados/services/disputa_emissao.py:1677`; `app/chamados/services/disputa_emissao.py:1681`) | Backfill e `DecisaoPergunta`: existência do ato histórico, não validade do conteúdo. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:56`; `app/chamados/models.py:4017`) |
| Carimbo complementado pela materialização | `PerguntaDisputa.carimbar`, exigindo texto; recusa desfaz em memória. (`app/chamados/models.py:3156`; `app/chamados/models.py:3189`) | Vitalidade e materialização efetiva. (`app/chamados/juizes.py:1407`; `app/chamados/juizes.py:1473`) |
| Limpeza do par | `descarimbar_em_lote`; reabertura e ressuscitação delegam à porta. (`app/chamados/models.py:3306`; `app/chamados/services/disputa_emissao.py:1634`; `app/chamados/models.py:1567`) | A porta valida o par resultante; o legado é fotografado antes de alterar. (`app/chamados/models.py:3291`; `app/chamados/services/disputa_emissao.py:1617`) |
| Persistência genérica do par | `save`, com tripwire. Saves alheios ao par ou preservando exatamente o passivo continuam permitidos. (`app/chamados/models.py:3472`; `app/chamados/models.py:3503`; `app/chamados/models.py:3506`) | `estado`, `respondida` e partição do app ainda significam presença do carimbo. (`app/chamados/models.py:3071`; `app/chamados/models.py:3584`; `app/chamados/services/pergunta_no_app.py:71`) |
| Par como resposta com conteúdo | Escritores acima. (`app/chamados/tests/test_e3_porta_A_escritor_do_par.py:42`) | Faces SQL, espera pelo texto e raio-X exigem carimbo + conteúdo após trim. (`app/chamados/juizes.py:1538`; `app/chamados/juizes.py:1546`; `app/chamados/services/fio.py:187`; `app/chamados/services/raiox.py:143`) |
| Carimbo como instante | Escritores acima. (`app/chamados/services/disputa_emissao.py:1468`; `app/chamados/services/disputa_emissao.py:1660`) | Projeção temporal/slot, comparação com cobrança, máximo do lote e idade da resposta. (`app/chamados/juizes.py:1380`; `app/chamados/juizes.py:348`; `app/chamados/services/verbos.py:342`; `app/chamados/veredito_lavrado.py:207`; `app/chamados/services/validacao.py:932`) |
| Texto preservado como histórico | `DecisaoPergunta.registrar` recebe a fotografia por correspondência de ato/instante; copia inclusive vazio. (`app/chamados/models.py:4029`; `app/chamados/models.py:4020`) | C19 lê decisões pelo horizonte; `ato_texto` representa texto tocado pelo ato. (`app/chamados/services/tarefa_do_dia.py:400`; `app/chamados/models.py:3939`) |
| `celula.veredito` | `lavrar_veredito`, pela porta; cartório e serviço administrativo delegam. (`app/ponto/portas/celula.py:1035`; `app/ponto/services/cartorio.py:1853`; `app/ponto/services/veredito_celula.py:352`) | C6, SQL e reconciliador distinguem ausência, acusação e absolvição; `''` agora é ausência nos três. (`app/chamados/juizes.py:1438`; `app/chamados/models.py:3408`; `app/chamados/reconciliador.py:638`) |
| `ata.lampadas.pendente` | Cartório cria `True` com `acesa=None`; porta persiste a ata. (`app/ponto/services/cartorio.py:915`; `app/ponto/portas/celula.py:1033`) | Lastro, C6 e reconciliador reconhecem provisoriedade por truthiness; SQL exige `True`. (`app/ponto/services/lastro.py:129`; `app/ponto/services/lastro.py:150`; `app/chamados/juizes.py:1452`; `app/chamados/reconciliador.py:1030`; `app/chamados/models.py:3403`) |

**Não, os leitores não supõem todos o mesmo significado.** Em `respondida_em` coexistem instante, ato histórico, rótulo de respondida e resposta com conteúdo. As diferenças temporais/históricas justificam os quatro declarados; a diferença entre app cru e espera/raio-X permanece operacional. Em `pendente`, o significado pretendido é comum, mas o domínio reconhecido diverge. (`app/chamados/juizes.py:348`; `app/chamados/models.py:4017`; `app/chamados/services/pergunta_no_app.py:71`; `app/chamados/services/fio.py:187`; `app/chamados/services/raiox.py:143`; `app/chamados/models.py:3403`)

O alargamento de `veredito=''` continua alcançando os oito leitores identificados: fila de validar, fio, app, cobrança, vigia, janela, raio-X e fechamento administrativo. Os primeiros cinco refinam por C6; janela, crédito e fechamento não fazem essa confirmação posterior. (`app/chamados/juizes.py:1703`; `app/chamados/services/fio.py:268`; `app/chamados/services/pergunta_no_app.py:58`; `app/chamados/services/cobranca.py:241`; `app/chamados/services/vigia_fio.py:764`; `app/chamados/services/janela_colab.py:63`; `app/chamados/services/raiox.py:135`; `app/ponto/services/veredito_celula.py:190`)

No fechamento, a seleção pode chegar à escrita de identidade e ao ato `SILENCIO`; portanto, esse alcance não é exclusivamente visual. A porta administrativa dispensa C6 por contrato. (`app/ponto/services/veredito_celula.py:181`; `app/ponto/services/veredito_celula.py:195`; `app/chamados/models.py:3195`; `app/chamados/models.py:3216`)

Sobre as classes que já reprovaram obras:

| Classe | Resultado nesta conferência |
|---|---|
| Segundo juiz duplicando autoridade / derivação paralela | As faces de texto usam a classe de brancos do Python; a derivação de provisoriedade continua divergente — bloqueante 1. (`app/chamados/juizes.py:1526`; `app/chamados/models.py:3403`; `app/ponto/services/lastro.py:150`) |
| Segundo escritor fora da porta | Não encontrado no alcance declarado do selo do par. São sete escritores, duas delegações e duas escritas em memória; SQL cru e `__dict__` estão fora desse alcance. (`app/chamados/tests/test_e3_porta_A_escritor_do_par.py:42`; `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:46`; `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:24`) |
| Dois donos ocultos | Q2 nomeia explicitamente “colaborador respondeu” e “FATO respondeu”; não são donos furtivos introduzidos pelo delta. (`app/core/portas.py:261`; `app/core/portas.py:264`; `app/core/portas.py:266`) |
| Relógio fora da borda | Faces novas não consultam relógio; fechamento reutiliza o instante da materialização no ato. Continência histórica continua qualificada: C6 recebe horizonte, `q_vivas()` não. (`app/chamados/juizes.py:1538`; `app/chamados/juizes.py:1546`; `app/ponto/services/veredito_celula.py:196`; `app/chamados/juizes.py:1377`; `app/chamados/models.py:3368`) |
| Selo/RED sem mordida suficiente | **Presente:** os mutantes do bloqueante 2 sobrevivem. As mordidas SQL nomeadas foram fortalecidas. (`app/chamados/tests/test_e3_catraca_respondedores.py:209`; `app/chamados/tests/test_e3_catraca_respondedores.py:458`; `app/chamados/tests/test_e3_forma_sql_do_juiz.py:131`) |
| Ordem de trava | Não identifiquei regressão nos caminhos conferidos: fechamento trava colaborador e depois disputa/chamado antes de pergunta; limpeza em lote exige colaboradores travados. (`app/ponto/services/veredito_celula.py:176`; `app/ponto/services/veredito_celula.py:185`; `app/chamados/models.py:3301`) |
| Campo com dois significados | **Presente:** carimbo cru versus resposta com conteúdo; pendência truthy versus booleano exato. (`app/chamados/models.py:3584`; `app/chamados/services/raiox.py:143`; `app/ponto/services/lastro.py:150`; `app/chamados/models.py:3403`) |

Quanto aos descartes e adiamentos da triagem:

- **Item 1 deveria continuar bloqueando.** A mudança de fila não cura a continência prometida, e o modelo/porta continuam admitindo o contraexemplo. A sonda fornecida tampouco demonstra o alegado zero não booleano. (`app/chamados/models.py:3382`; `app/escala/models.py:1778`; `app/ponto/portas/celula.py:995`; medição específica **[não verificado]**)
- **Item 2d não sustenta o descarte da classe.** Mesmo aceitando retirar o literal `True or ...` do universo pretendido, sobrevivem lambda não executada e disjunção ORM, sem dobra de constante. Isso mantém o bloqueante 2. (`app/chamados/tests/test_e3_catraca_respondedores.py:209`; `app/chamados/tests/test_e3_catraca_respondedores.py:296`)
- **Triagem item 3, sobre `veredito=''`:** os zeros fornecidos sustentam apenas ausência desse estado na foto declarada; não apagam os caminhos atingidos nem medem a alteração de brancos. Execução, atualidade e efeitos completos da sonda: **[não verificado]**. (`app/chamados/models.py:3397`; `app/chamados/models.py:3408`; `app/ponto/services/veredito_celula.py:181`)
- **Aval item (3), os 17 sítios/passivo:** não acrescento bloqueante apenas por adiá-los, enquanto permanecerem sem migração. Há leitores crus remanescentes e a própria catraca declara essa parada. Entretanto,

[truncado em 20000 de 20375 caracteres]

## 12/11 22:18 · sessao 2ce634119c25 · - · - · origem=Ronald (tmux)

| Falha/método | Classe | Evidência e cura |
|---|---|---|
| `GradePorTurnoFolgaValeEmTodaCompetenciaTest.test_MORDE_dia_da_09_com_ata_folga_sai_trabalho` | F | `escala/tests/test_grade_folga_vale_em_toda_competencia.py:62`: `_ata` grava somente `tipo_dia`. Acrescentar `orfas: []`, `n_orfas: 0`. |
| `test_MORDE_dia_fora_de_toda_versao_com_ata_folga_sai_trabalho` | F | Mesma fixture, `:62`; método `:79`. |
| `test_MORDE_simular_dia_responde_igual` | F | Mesma fixture, `:62`; método `:84`. `ponto/services/simular_dia.py:5` apenas delega ao builder. |
| `test_anti_vacuidade_folga_a_mao_do_vinculo_encerrado_segue_folga` | F | Mesma fixture, `:62`; método `:90`. |
| `NucleoInsumosDaAtaTest.test_MORDE_saiu_pro_almoco_e_nao_voltou_NAO_fechou_o_turno` | F | `escala/tests/test_selo_b5f3_lavra_batida.py:53`: ata só tem lâmpadas. Lavrar ambas as chaves. |
| `test_dia_completo_com_intervalo` | F | Fixture incompleta em `:58`. |
| `test_cross_meianoite_e_UMA_jornada` | F | Fixture incompleta em `:63`. |
| `test_tri_estado_da_ata_acesa_None_nao_vira_False` | F | Fixture incompleta em `:69`. |
| `test_folga_SEM_orfa_e_fato_folga_COM_orfa_e_duvida` | F | Literais em `:78` e `:79` só têm contador; primeiro precisa lista vazia, segundo três instantes aware pelo escritor para preservar contador literal `3`. |
| `test_marco_ausente_no_DNA_nao_se_inventa_False` | F | Fixture incompleta em `:87`. |
| `test_nunca_bateu_ata_vazia_e_zero_batida_na_vida` | N | `ponto/services/cartorio.py:840-843`: contador aceita `None`, mas `a = ata` leva a `None.get`. Preservar `None` no teste `escala/tests/test_selo_b5f3_lavra_batida.py:83`; ramo explícito `NAO_LAVRADA` retorna `(False, False)`. Base `0a747193e:app/ponto/services/cartorio.py:833` normalizava envelope e `:839` devolvia `(False, False)`; ambos correspondem a `0`. |
| `SaidaValidaViraOrfa.test_a_ata_CURADA_mantem_o_par_fechado` | F | `escala/tests/test_saida_valida_vira_orfa.py:152-154`: atribui `orfas`, omite `n_orfas`. Usar `gravar_orfas(cel.ata, d['orfas'])`. |
| `CalendarioLeOrfaDaAtaTest.test_GOLDEN_orfa_LAVRADA_NA_ATA_vem_marcada` | F + O | Fixture `colaboradores/tests/test_calendario_orfa.py:145-149` omite contador. Erro secundário: `fora_map` inicializa depois da chamada que falhou, `colaboradores/services/calendario.py:334`/`:352`; uso `:626`. Mover inicialização junto de `orfas_map`, antes do `try`, para não haver `UnboundLocalError` na recuperação. |
| `test_as_batidas_que_a_ata_casou_NAO_sao_orfas` | F + O | Mesmas origens acima; método `colaboradores/tests/test_calendario_orfa.py:171`. |
| `FuzzGrade.test_fuzz_ancorado` | O | `escala/tests/test_grade_fuzz.py:96` exige seis chaves; escritor já emite `instante` em `escala/utils.py:328`. Atualizar contrato do teste com `instante` e verificar igualdade ao instante reconstruído em `:100`; não alterar escritor. Base `0a747193e:app/escala/utils.py:326-327` tinha seis chaves. |
| `LeitorCelulaTest.test_contrato_unico_agregada_tem_as_MESMAS_CHAVES_da_lampada` | O | `escala/services/leitor_celula.py:125` inclui `instante` em lâmpadas; agregado `:173-176` omite. Acrescentar `instante: None` ao agregado, seguindo contrato explícito do teste `escala/tests/test_t41_leitor_celula.py:172-173`. |
| `test_contrato_unico_sobrevive_a_cura` | O | Mesma assimetria; teste `escala/tests/test_t41_leitor_celula.py:274-280`. |

Não encontrei classe P demonstrada nesses caminhos: o construtor em memória `escala/utils.py:1893` já usa `gravar_orfas` em `:1902-1903`; `ponto/services/simular_dia.py:5-8` delega; o cartório constrói a ata em `ponto/services/cartorio.py:739` e já lavra órfãs adiante. Os casos de grade/simulação fornecidos têm fixtures incompletas identificáveis nas linhas acima.

Nenhum arquivo foi alterado nem houve execução de banco, rede ou testes nesta exploração. [nao verificado]

## 12/11 22:17 · sessao 740decf9ea38 · - · - · origem=Ronald (tmux)

Classificação: todos os erros de chaves dos nove módulos examinados são **F**. Os nomes “alimentada”, “previsto em lote” e “régua união” não identificam, nestes tracebacks, um construtor de produção incompleto: os testes persistem atas incompletas diretamente. Evidências abaixo. [nao verificado]

| Módulo / falhas individuais | Classe | Origem e cura |
|---|---|---|
| `test_art62_nunca_acusa`: `test_atraso_de_isento_nao_acusa_e_guarda_o_delta` (:166); `test_ata_agregada_de_isento_nao_acusa` (:172) | F | `setUpTestData`, atas em `escala/tests/test_art62_nunca_acusa.py:148` e `:157`. Acrescentar `'orfas': [], 'n_orfas': 0` em ambas. |
| `test_grade_alimentada`: `test_alimentada_e_igual_a_do_banco_e_nao_consulta_celula` (:44); `test_acesa_lavrada_e_o_tri_estado_da_ata` (:57) | F | `setUpTestData` persiste ata incompleta em `escala/tests/test_grade_alimentada.py:36`. Acrescentar as duas chaves vazias. |
| `test_leitor_isento`: `test_o_dia_do_isento_existe_na_saida` (:64); `test_previsto_vale_cumprido_como_no_builder` (:69); `test_o_dia_fica_marcado_como_isencao` (:81); `test_consumidor_nao_pinta_alerta_em_dia_de_isento` (:84) | F | Fixture comum em `escala/tests/test_leitor_isento.py:47`. Acrescentar as duas chaves vazias. |
| `test_leitor_isento`: `test_batida_real_de_isento_fica_intacta` (:90) | F | Ata local em `escala/tests/test_leitor_isento.py:109`; acrescentar ambas vazias. |
| `test_leitor_isento`: `test_folga_de_isento_nao_ganha_celula` (:123) | F | Ata local em `escala/tests/test_leitor_isento.py:130`; acrescentar ambas vazias. |
| `test_leitor_regua_uniao`: `test_GOLDEN_a_uniao_junta_dias_com_marcos_diferentes` (:72); `test_o_topo_deixa_de_ser_o_placeholder_de_STRINGS` (:81); `test_JANELA_DE_UM_DIA_a_uniao_e_a_coluna_DAQUELE_dia` (:89); `test_ADITIVO_os_marcos_POR_DIA_ficam_intactos` (:106); `test_dia_sem_lampada_NAO_contribui_coluna` (:118); `test_folga_nao_inventa_coluna` (:124); `test_GOLDEN_CROSS_MEIA_NOITE_a_ordem_e_a_DO_TURNO` (:129); `test_a_CHAVE_e_a_do_builder_mas_a_ORDEM_e_a_da_ata` (:154); `test_multidia_a_ordem_segue_a_de_dias_REVERSE_CHRON` (:169); `test_idempotente_e_sem_duplicata` (:189) | F | Todas passam por `_cel`, que persiste ata incompleta em `escala/tests/test_leitor_regua_uniao.py:69`. Acrescentar ambas vazias nessa fixture. |
| `test_leitor_celula_minutos_pela_autoridade`: `test_MORDE_sem_minutos_na_ata_a_autoridade_responde_pelos_marcos` (:41) | F | Ata padrão de `_celula` em `escala/tests/test_leitor_celula_minutos_pela_autoridade.py:36`; acrescentar ambas vazias. |
| Mesmo módulo: `test_minutos_lavrados_na_ata_vencem` (:45) | F | Ata literal em `escala/tests/test_leitor_celula_minutos_pela_autoridade.py:46`; acrescentar ambas vazias. |
| Mesmo módulo: `test_celula_de_folga_preve_zero` (:49) | F | Ata literal em `escala/tests/test_leitor_celula_minutos_pela_autoridade.py:50`; acrescentar ambas vazias. |
| `test_hx_furos_diarios_celula`: `test_le_a_ata_da_celula` (:26) | F | `escala/tests/test_hx_furos_diarios_celula.py:21` declara `n_orfas=1` sem lista. Acrescentar uma órfã explícita na fixture, mantendo contagem `1`; o teste exige saída `(data, 2, 1, 1)` em `:30`. O horário real não é informado pelo dado; qualquer horário escolhido seria sintético da fixture. |
| `test_snc2_grade_previsto_em_lote`: `test_RED_o_previsto_da_grade_nao_custa_query_por_dia` (:89); `test_o_previsto_e_o_do_juiz_nao_alimentado_dia_a_dia` (:96) | F | `ATA_SEM_PREVISTO` incompleta em `escala/tests/test_snc2_grade_previsto_em_lote.py:33`, copiada em `:65`/`:76`; outra ata literal de folga em `:74`. Acrescentar ambas vazias na constante e na folga. |
| `test_selo_b5_backfill`: `test_MORDE_a_celula_carimbada_pela_F2_ENTRA_no_universo` (:63); `test_idempotente_por_CONVERGENCIA` (:73); `test_DRY_nao_escreve` (:83) | F | `_cel_lavrada_pela_F2` persiste `n_orfas=0` sem lista em `escala/tests/test_selo_b5_backfill.py:55`. Acrescentar `'orfas': []`. |
| `test_t41_leitor_celula`: `test_ata_agregada_sem_lampada_nao_quebra` (:150) | F | Ata local em `escala/tests/test_t41_leitor_celula.py:161`/`:162`; acrescentar ambas vazias. |
| Mesmo módulo: `test_contrato_unico_agregada_tem_as_MESMAS_CHAVES_da_lampada` (:166); `test_contrato_unico_sobrevive_a_cura` (:274) | O | A produção acrescentou `instante` apenas à lâmpada (`escala/services/leitor_celula.py:125`), deixando `_vazia` sem a chave (`:173`). Os testes comparam os conjuntos de chaves em `escala/tests/test_t41_leitor_celula.py:178` e `:280`. Cura proposta: `_celulas_da_ata::_vazia` declarar `'instante': None`, preservando desconhecimento temporal e os números existentes. |

As repetições “AssertionError: tripwire” de cada método têm a mesma origem da falha principal: `_valores_orfas` registra o estado ilegal antes de lançar (`escala/services/ata.py:28`, `:31`, `:49`, `:51`).

A base `0a747193e:app/escala/services/leitor_celula.py:122`/`:123` não tinha `instante` na lâmpada; `:171`/`:174` também não tinha na agregada. A divergência de formato entre esses ramos surgiu na cópia, onde só a lâmpada ganhou essa chave (`escala/services/leitor_celula.py:125`).

Não escrevi arquivos nem executei banco ou testes. Não encontrei `AGENTS.md` nos diretórios ancestrais ou em `escala/` [não verificado quanto a arquivos fora desses caminhos]. [nao verificado]

## 12/11 22:16 · sessao ce93dc9cf830 · - · - · origem=Ronald (tmux)

Conferido com `git show` em `78994522d` (B) e `69a3d1570` (C).

| Função | BASE | CÓPIA | Chamada comum |
|---|---|---|---|
| `dia_degenerado(dia)` | `app/ponto/propositor.py:34` | `app/ponto/propositor.py:36` | `fn(dia)` |
| `insumos_atribuicao(dia)` | Ausente; lógica inline em `app/ponto/management/commands/propor_reorganizacao.py:91` | `app/ponto/propositor.py:88` | Não existe isoladamente nas duas |
| `_runs_de_rajada(horas)` | `app/escala/orfa_classe.py:40` | `app/escala/orfa_classe.py:36` | Pelo chamador `classificar_orfas(dia)` |
| `classificar_orfas(dia)` | `app/escala/orfa_classe.py:53` | `app/escala/orfa_classe.py:54` | `fn(dia)` |
| `horas_reais_do_dia(dia, deslocamentos=None)` | `app/ponto/turnos.py:2646` | `app/ponto/turnos.py:2646` | `fn(dia, deslocamentos)` |
| `_entrada_espuria(periodo, batidas, terceiro)` | Terceiro `orfas_hhmm`, `app/relatorios/pdf_espelho.py:174` | Terceiro `orfas_instantes`, `app/relatorios/pdf_espelho.py:174` | Três argumentos posicionais |
| `_marcar_orfas_pdf(dia, orfas)` | Ausente; lógica inline `app/relatorios/pdf_espelho.py:609` | `app/relatorios/pdf_espelho.py:186` | Pelo coletor comum |
| `_coletar_dados_espelho(colaborador, data_ini, data_fim, _turnos_juiz=None, *, agora)` | `app/relatorios/pdf_espelho.py:187` | `app/relatorios/pdf_espelho.py:236` | `fn(c, ini, fim, agora=fixo)` |
| `_coletar_dados_espelho_mes(colaborador, mes, ano, *, agora)` | `app/relatorios/pdf_espelho.py:654` | `app/relatorios/pdf_espelho.py:694` | `fn(c, mes, ano, agora=fixo)` |
| `contexto_calendario(colaborador, GET, *, datas_em_aberto=None)` | `app/colaboradores/services/calendario.py:159` | `app/colaboradores/services/calendario.py:159` | `fn(c, GET)` |

`insumos_atribuicao` deve ser identificado como ausente na BASE; extrair ou reconstruir a lógica inline violaria a exigência. O pai comum é `propor_reorganizacao.Command._run`, assinatura em ambas `app/ponto/management/commands/propor_reorganizacao.py:25`. Opções suficientes:

```python
{
    "empresa": c.empresa_id,
    "dias": (fim - ini).days + 1,
    "colab": c.pk,
    "ref": fim.isoformat(),
    "detalhe": False,
}
```

Esse pai seleciona ativos, não isentos e exclui vínculos ativos intermitentes; chama `dia_degenerado` em todos os dias da grade e `insumos_atribuicao` somente nos dias escalados cujo template não foi declarado suspeito. Capturar `Command(stdout=io.StringIO())._run(opts)` é necessário porque o relatório pode conter nomes. Referências C: `app/ponto/management/commands/propor_reorganizacao.py:35`, `:65`, `:71`, `:83`, `:91`, `:108`, `:124`. O comando declara sombra e nenhuma escrita; seu corpo somente calcula e imprime: `app/ponto/management/commands/propor_reorganizacao.py:1`, `:109`.

`_runs_de_rajada` deve ser alcançada por `classificar_orfas`, que faz a guarda de lista vazia e só chama runs em trabalho com furo. Referências B `app/escala/orfa_classe.py:58`, `:65`, `:70`, `:74`; C `app/escala/orfa_classe.py:59`, `:68`, `:73`, `:77`. Chamador de produção de `classificar_orfas` além dos testes: [não verificado]. Na cópia, a busca local encontrou apenas consumo de `JANELA_CLUSTER_MIN` pelo código de produção em `app/ponto/turnos.py:1581`.

`horas_reais_do_dia(dia)` sozinho usa assinatura válida, mas não reproduz os argumentos de produção: o chamador real passa `deslocamentos.get(d)` no ramo de ausência parcial. O mapa vem da cadeia do turno, montado no builder e transmitido ao overlay. Referências `app/escala/utils.py:1235`, `:1238`, `:1757`, `:1854`. Para cobertura estrita sem reconstrução, executar o builder real `montar_grade_prevista_periodo_por_turno(colab, batidas, dias=..., ref=..., agora=...)`, assinatura B/C `app/escala/utils.py:1281`, e observar o resultado desse caminho. A chamada default deve ser rotulada como insuficiente para esse chamador.

O coletor PDF mensal é o pai comum adequado: chama o período real da competência e depois o coletor completo. C `app/relatorios/pdf_espelho.py:701`. Na CÓPIA, os adaptadores são executados em `app/relatorios/pdf_espelho.py:644`, `:649`; na BASE, `_entrada_espuria` em `app/relatorios/pdf_espelho.py:513`, marcação inline em `:609`. `_marcar_orfas_pdf` retorna `None`, mas modifica `Batida.orfa` em memória, portanto medir apenas o retorno tornaria a prova vazia; observar também `(b.pk, b.orfa)`. C `app/relatorios/pdf_espelho.py:186`.

O coletor devolve `resumo`, `dias`, objetos `tipo_escala` e `escala`, `anomalias_mes` e `carimbo_convencao`, em C `app/relatorios/pdf_espelho.py:683`. Serializar modelos somente por PK perderia os atributos transitórios `orfa`. O coletor não chama persistência diretamente; usa apuração, motor, espelho e folha: C `app/relatorios/pdf_espelho.py:467`, `:483`, `:638`, `:668`. Ausência de qualquer efeito externo transitivo: [não verificado]. A fábrica do motor lê `ParametroSistema` e usa um mapa fraco em memória: `app/core/regua_cct.py:443`, `:488`.

Calendário aceita:

```python
{"mes": str(mes), "ano": str(ano)}
{"modo": "competencia", "mes": str(mes), "ano": str(ano)}
{"modo": "custom", "data_ini": ini.isoformat(), "data_fim": fim.isoformat()}
```

As janelas são escolhidas em `app/colaboradores/services/calendario.py:165`, `:177`, `:198`; custom limita diferença a 366 dias em `:205`. A view repassa GET sem restringir modos; preload original usa `posto__praca`: `app/colaboradores/views.py:1946`. O drawer usa custom: `app/colaboradores/services/drawer.py:121`. O histórico efetivo de GETs de produção é [não verificado].

Congelar `timezone.now` para o mesmo instante nas duas execuções é necessário para calendário, pois lê o relógio internamente: `app/colaboradores/services/calendario.py:163`, `:250`. O retorno inclui modelos e batidas em `app/colaboradores/services/calendario.py:732`, `:749`, `:798`; novamente, preservar atributos transitórios ao medir.

## 12/11 22:16 · sessao f1db4288a56d · - · - · origem=Ronald (tmux)

Conferência somente por leitura; a cópia não tem `.git`, portanto linhas da base abaixo foram reconstruídas pelo `diff_producao.txt` fornecido, não conferidas em checkout separado. [nao verificado]

| Função | Cópia | Base reconstruída |
|---|---|---|
| `dia_degenerado(dia)` | `app/ponto/propositor.py:36` | `app/ponto/propositor.py:34` |
| `insumos_atribuicao(dia)` | `app/ponto/propositor.py:88` | Inexistente; nova no diff |
| `_runs_de_rajada(horas)` | `app/escala/orfa_classe.py:36` | `app/escala/orfa_classe.py:40` |
| `classificar_orfas(dia)` | `app/escala/orfa_classe.py:54` | `app/escala/orfa_classe.py:53` |
| `horas_reais_do_dia(dia, deslocamentos=None)` | `app/ponto/turnos.py:2646` | Mesma linha/assinatura |
| `_entrada_espuria(periodo, batidas, orfas_instantes)` | `app/relatorios/pdf_espelho.py:174` | Mesma linha; parâmetro final `orfas_hhmm` |
| `_marcar_orfas_pdf(dia, orfas)` | `app/relatorios/pdf_espelho.py:186` | Inexistente; nova no diff |
| `contexto_calendario(colaborador, GET, *, datas_em_aberto=None)` | `app/colaboradores/services/calendario.py:159` | Mesma linha/assinatura |
| `_coletar_dados_espelho(colaborador, data_ini, data_fim, _turnos_juiz=None, *, agora)` | `app/relatorios/pdf_espelho.py:236` | `app/relatorios/pdf_espelho.py:187` |
| `_coletar_dados_espelho_mes(colaborador, mes, ano, *, agora)` | `app/relatorios/pdf_espelho.py:694` | `app/relatorios/pdf_espelho.py:654` |

A chamada posicional `_entrada_espuria(periodo, batidas, orfas)` funciona nas duas assinaturas; validar com `inspect.signature(...).bind(...)`, sem capturar `TypeError` para escolher versão. `app/relatorios/pdf_espelho.py:174`

Para calendário de competência, chamada real:

```python
contexto_calendario(
    colab,
    {"modo": "competencia", "mes": str(mes), "ano": str(ano)},
)
```

O próprio calendário resolve a janela com `janela_fechamento(mes, ano, colaborador.empresa)`; modos `mes`, `semana`, `quinzena` e `custom` também existem, e o código não informa quais requisições ocorreram em produção. `app/colaboradores/services/calendario.py:165`, `app/colaboradores/services/calendario.py:175`, `app/colaboradores/services/calendario.py:177`, `app/colaboradores/services/calendario.py:198`

Para PDF, usar `_coletar_dados_espelho_mes(colab, mes, ano, agora=FIXO)` ou resolver com a função real `periodo_da_competencia(colab, mes, ano)` e chamar o coletor comum. O dono da janela delega a `folha.export.janela_competencia`. `app/relatorios/pdf_espelho.py:694`, `app/relatorios/pdf_espelho.py:703`, `app/relatorios/cartao_pela_celula.py:21`

O coletor comum percorre os adaptadores alterados em seu contexto real: `_entrada_espuria` é chamada apenas para períodos abertos, por `_contar_abertos_pdf`; `_marcar_orfas_pdf` atua nas batidas de cada dia retornado pelo espelho. Ambas as alterações mudam objetos apenas em memória nesse trecho. `app/relatorios/pdf_espelho.py:220`, `app/relatorios/pdf_espelho.py:223`, `app/relatorios/pdf_espelho.py:644`, `app/relatorios/pdf_espelho.py:649`

Para observar `horas_reais_do_dia` com argumentos de produção, instalar wrapper no atributo `ponto.turnos.horas_reais_do_dia` antes de chamar o coletor PDF. `_marcar_ausencias` importa essa função localmente e a chama com o dia da grade e `deslocamentos.get(d)`, antes de substituir células pela cobertura de ausência. Chamar a função diretamente sobre `grade_da_celula` sem deslocamentos mede outra entrada. `app/escala/utils.py:1181`, `app/escala/utils.py:1182`, `app/escala/utils.py:1235`, `app/escala/utils.py:1238`

O PDF chama `grade_espelho_janela`, que passa pelo builder e pelo overlay de ausências; isso permite observar as chamadas reais sem reconstruir a lógica de deslocamentos. `app/relatorios/pdf_espelho.py:242`, `app/relatorios/pdf_espelho.py:585`, `app/escala/utils.py:2343`, `app/escala/utils.py:1854`

O propositor de produção usa `Command._run(o)`: seleciona colaboradores ativos e não isentos por empresa, exclui vínculos ativos intermitentes, gera a grade real e chama `dia_degenerado` para cada dia. `insumos_atribuicao` só é chamada quando o dia escalou e o template não é suspeito. `app/ponto/management/commands/propor_reorganizacao.py:35`, `app/ponto/management/commands/propor_reorganizacao.py:42`, `app/ponto/management/commands/propor_reorganizacao.py:65`, `app/ponto/management/commands/propor_reorganizacao.py:71`, `app/ponto/management/commands/propor_reorganizacao.py:83`, `app/ponto/management/commands/propor_reorganizacao.py:91`

Como `insumos_atribuicao` não existe na base, medir o chamador comum `Command._run` é a aproximação real que preserva a lógica inline antiga. Capturar `stdout` em memória; o comando imprime nomes em duas saídas. `app/ponto/management/commands/propor_reorganizacao.py:25`, `app/ponto/management/commands/propor_reorganizacao.py:108`, `app/ponto/management/commands/propor_reorganizacao.py:123`

`_runs_de_rajada` tem como chamador `classificar_orfas`; a busca por todo o app não encontrou chamador de produção deste último, apenas testes. Sua unidade documentada é o dict-dia da grade. Cobri-lo com `classificar_orfas(dia)` é sonda adicional, não demonstra execução em produção. `app/escala/orfa_classe.py:54`, `app/escala/orfa_classe.py:55`, `app/escala/orfa_classe.py:77`; ausência de chamadas em produção [nao verificado]

Existem chamadas de logging nos caminhos de calendário, grade e espelho; desabilitar logging em memória evita que a sonda dispare handlers de arquivo. `app/colaboradores/services/calendario.py:400`, `app/escala/utils.py:193`, `app/ponto/services/espelho.py:570`

O calendário consulta `timezone.now()` internamente; congelar o mesmo instante nas duas execuções evita que a comparação varie com o relógio. O PDF já recebe `agora`. `app/colaboradores/services/calendario.py:163`, `app/colaboradores/services/calendario.py:250`, `app/relatorios/pdf_espelho.py:236`

## 12/11 22:16 · sessao 1ec7a97f7721 · - · - · origem=Ronald (tmux)

Não há repositório Git neste workspace; BASE foi conferida somente pela reversão dos hunks do `diff_producao.txt` fornecido, sem acesso independente ao commit `78994522d` `[nao verificado]`.

| Função | Assinatura conferida | CÓPIA | BASE reconstruída |
|---|---|---|---|
| `_celula_furada` | `(colaborador, data_iso, *, para='cobranca')` | `app/chamados/juizes.py:459` | `app/chamados/juizes.py:459` |
| `_plantio_preenche_furo_na_data` | `(colaborador, ts)` | `app/chamados/services/materializacao.py:143` | mesmo arquivo:143 |
| `_ts_tipo_do_plantio` | `(disputa, p, erros, pendencia=None, *, carga=None)` | mesmo arquivo:339 | mesmo arquivo:333 |
| `estado_aplicacao_pergunta` | `(disputa, p)` | mesmo arquivo:437 | mesmo arquivo:431 |
| `_leitura_da_celula` | `(cel)` | `app/ponto/services/cluster_espurio.py:15` | mesmo arquivo:15 |
| `dias_com_cluster` | `(colab, ec, ref, dias)` | mesmo arquivo:49 | mesmo arquivo:40 |
| `pendencias` | `(colab, hoje, dias=JANELA_DIAS)` | mesmo arquivo:162 | mesmo arquivo:153 |
| `reconciliar_colab` | `(colab, hoje, dias=JANELA_DIAS)` | mesmo arquivo:185 | mesmo arquivo:176 |
| `Command.handle` | `(self, *a, **o)` | `app/ponto/management/commands/detectar_cluster_espurio.py:65` | mesmo arquivo:65 |

Todas essas assinaturas permaneceram iguais no diff fornecido; nenhuma precisa adaptação por tentativa e erro. As linhas BASE acima são derivadas dos deslocamentos dos hunks do anexo `[nao verificado quanto ao commit Git real]`. [nao verificado]

Para `_celula_furada`, são necessários dois leitores separados: `para='cobranca'` e `para='folha'`, em todo par colaborador/data de CelulaDia. A produção distingue esses efeitos e propaga o argumento na recursão da véspera (`app/chamados/juizes.py:472`, `app/chamados/juizes.py:505`; `app/chamados/models.py:739`, `app/chamados/models.py:1253`, `app/chamados/models.py:1300`, `app/chamados/models.py:1350`). A função consulta a célula internamente, não recebe objeto pré-carregado (`app/chamados/juizes.py:487`; `app/chamados/juizes.py:403`).

**Cuidado com interpretar a matriz:** ata NULL significa zero na componente de órfãs, mas `_celula_furada` pode devolver `None`, porque ausência de `lampadas` é desconhecimento; não se deve substituir seu retorno por False (`app/chamados/juizes.py:494`, `app/chamados/juizes.py:511`).

`_plantio_preenche_furo_na_data` recebe um timestamp de proposta de plantio. Fabricar meia-noite/meio-dia por CelulaDia não representa o universo de produção. O timestamp real vem de `_ts_tipo_do_plantio`, que considera resposta, marco de intervalo e ajuste overnight (`app/chamados/services/materializacao.py:339`, `app/chamados/services/materializacao.py:415`, `app/chamados/services/materializacao.py:432`). A superfície real de leitura é `estado_aplicacao_pergunta(disputa,p)`, executável sobre toda PerguntaDisputa dos colaboradores com CelulaDia; ela chama o plantio somente quando as guardas reais chegam a ele (`app/chamados/services/materializacao.py:437`, `app/chamados/services/materializacao.py:455`, `app/chamados/services/materializacao.py:465`). Um observador transparente da função real pode registrar suas entradas/retornos enquanto essa superfície roda. Não reconstruir as guardas.

O materializador também chama o plantio, mas escreve antes e depois; não deve ser chamado na sonda (`app/chamados/services/materializacao.py:863`, `app/chamados/services/materializacao.py:888`, `app/chamados/services/materializacao.py:1234`, `app/chamados/services/materializacao.py:1241`). Os candidatos que só seriam atingidos após essas escritas não são integralmente cobertos pelo chamador de leitura `[nao verificado]`.

**`reconciliar_colab` não possui DRY**: fecha chamados e abre outros (`app/ponto/services/cluster_espurio.py:199`, `app/ponto/services/cluster_espurio.py:205`). A chamada mais próxima sem escrita é `pendencias(colab, hoje, dias)`, explicitamente read-only e usada pelo próprio reconciliador (`app/ponto/services/cluster_espurio.py:162`, `app/ponto/services/cluster_espurio.py:196`). Sua saída deve projetar `a_fechar` para IDs dos chamados; serializar instâncias com `str()` arrisca dados pessoais `[nao verificado quanto ao __str__]`.

O comando DRY é chamável com:

```python
Command(stdout=sink, stderr=sink).handle(
    apply=False, alarme=False, legado=False,
    dias=14, colab=colab.pk, esperados=None,
)
```

As opções são definidas em `app/ponto/management/commands/detectar_cluster_espurio.py:35`. O ramo DRY chama `pendencias` em `:85` e o contador de encerrados em `:94`. Entretanto, ele imprime `colab.nome` em `:87`; stdout/stderr precisam ser capturados em memória e nunca impressos diretamente. Seu retorno é `None`, então hashear somente esse retorno não mede a alteração de formatação do comando (`app/ponto/management/commands/detectar_cluster_espurio.py:87`, `app/ponto/management/commands/detectar_cluster_espurio.py:101`).

O universo padrão do comando é escala ativa sem data_fim, colaborador ativo, com uma iteração por EscalaColaborador, potencialmente repetindo colaborador (`app/ponto/management/commands/detectar_cluster_espurio.py:71`, `app/ponto/management/commands/detectar_cluster_espurio.py:78`). Para atender à frota solicitada, medir adicionalmente `pendencias` em **todos** os colaboradores com CelulaDia, sem aplicar esse filtro; a janela de abertura continua sendo `hoje - dias + 1 .. hoje`, enquanto fechamento consulta o dia próprio de todos os chamados vivos (`app/ponto/services/cluster_espurio.py:53`, `app/ponto/services/cluster_espurio.py:62`, `app/ponto/services/cluster_espurio.py:174`).

Há exceções engolidas internamente: `_celula_furada` retorna None e plantio retorna False em falhas; a contagem externa de `EXC:<Classe>` não as revela (`app/chamados/juizes.py:525`, `app/chamados/services/materializacao.py:191`). `core.observ.engolidos()` fornece contadores adicionais sem consulta a logs (`app/core/observ.py:60`), mas não cobre o except final silencioso de `_celula_furada` (`app/chamados/juizes.py:525`).

Desabilitar logging durante a sonda evita que os caminhos de erro escrevam arquivos ou exponham respostas em handlers: há logging no plantio e em `registrar_engolido` (`app/chamados/services/materializacao.py:192`; `app/core/observ.py:79`). Nenhum script/teste foi executado e nenhum arquivo foi alterado nesta investigação `[nao verificado por auditoria externa]`.

## 12/11 22:16 · sessao 088fe252e946 · - · - · origem=Ronald (tmux)

Mapa do par no HEAD da cópia:

| Campo/valor | Quem escreve | Quem lê / significado |
|---|---|---|
| `respondida_em` + `resposta_colab`, nascimento | `PerguntaDisputa._nascer` recusa carimbo sem texto antes da trava; inserção nasce pelo corpo único. (`app/chamados/models.py:2846`, `app/chamados/models.py:2865`, `app/chamados/models.py:2871`) | C6 lê carimbo **e texto após strip** como resposta; a projeção temporal apaga ambos da leitura quando o instante é posterior ao horizonte. (`app/chamados/juizes.py:1375`, `app/chamados/juizes.py:1380`, `app/chamados/juizes.py:1407`) |
| Mesmo par, resposta humana | `responder_pergunta_disputa_core`: trim/recusa vazio, um relógio, atribui o par; trava colaborador antes de save e registra ato com o mesmo instante. (`app/chamados/services/disputa_emissao.py:1432`, `app/chamados/services/disputa_emissao.py:1462`, `app/chamados/services/disputa_emissao.py:1467`, `app/chamados/services/disputa_emissao.py:1506`, `app/chamados/services/disputa_emissao.py:1511`) | Materializador lê o conteúdo como hora/escape/anuência; ausência de texto bloqueia plantio, mesmo havendo carimbo. (`app/chamados/services/materializacao.py:359`, `app/chamados/services/materializacao.py:474`, `app/chamados/services/materializacao.py:835`, `app/chamados/services/materializacao.py:1001`) |
| Mesmo par, resposta automática | `responder_automaticamente`: preserva texto existente; valida o par e grava texto/carimbo + ato usando instante recebido. (`app/chamados/services/disputa_emissao.py:1649`, `app/chamados/services/disputa_emissao.py:1657`, `app/chamados/services/disputa_emissao.py:1659`, `app/chamados/services/disputa_emissao.py:1663`) | Juiz ilegal `respondida_sem_texto`, sua porta e faces SQL interpretam o par como carimbo com/sem conteúdo após strip. (`app/chamados/juizes.py:1510`, `app/chamados/juizes.py:1518`, `app/chamados/juizes.py:1538`, `app/chamados/juizes.py:1546`, `app/chamados/services/fio.py:1602`) |
| `respondida_em`, recuperação da trilha | `carimbar_resposta_pela_trilha`: só carimba texto existente, usando instante testemunhado pela nota; grava ato correspondente. (`app/chamados/services/disputa_emissao.py:1672`, `app/chamados/services/disputa_emissao.py:1677`, `app/chamados/services/disputa_emissao.py:1681`, `app/chamados/services/disputa_emissao.py:1684`) | `_slot_esperado` pergunta se a resposta já existia no horizonte; lê texto como data/hora. (`app/chamados/juizes.py:347`, `app/chamados/juizes.py:348`, `app/chamados/juizes.py:360`) |
| `respondida_em`, complemento de materialização | `PerguntaDisputa.carimbar`: escreve quando solicitado e somente com texto; usa instante da materialização; rollback repõe valor em memória se recusado. (`app/chamados/models.py:3150`, `app/chamados/models.py:3156`, `app/chamados/models.py:3158`, `app/chamados/models.py:3189`) | `q_vivas` lê a face SQL “carimbo e texto”; degrau 6 também lê texto indiretamente pela materialização/slot. (`app/chamados/models.py:3393`, `app/chamados/models.py:3396`, `app/chamados/models.py:3412`, `app/chamados/juizes.py:1473`) |
| Par, limpeza | `descarimbar_em_lote` valida o par resultante, exige colaboradores travados e grava pelo gerente; `reabrir_para_correcao` delega par vazio depois de preservar foto da resposta; `ressuscitar_perguntas` também delega. (`app/chamados/models.py:3286`, `app/chamados/models.py:3301`, `app/chamados/models.py:3306`, `app/chamados/models.py:1567`, `app/chamados/services/disputa_emissao.py:1595`, `app/chamados/services/disputa_emissao.py:1615`, `app/chamados/services/disputa_emissao.py:1617`, `app/chamados/services/disputa_emissao.py:1634`) | Recusa/validação ainda usam carimbo cru como disponibilidade da resposta; recusa depois preserva texto anterior na trilha e reabre pela porta. (`app/chamados/services/validacao.py:395`, `app/chamados/services/validacao.py:578`, `app/chamados/services/validacao.py:588`, `app/chamados/services/validacao.py:601`) |
| Par, persistência genérica | `PerguntaDisputa.save` executa tripwire; saves de outros campos ou do mesmo par passivo continuam permitidos. (`app/chamados/models.py:3472`, `app/chamados/models.py:3503`, `app/chamados/models.py:3506`, `app/chamados/models.py:3509`) | `estado`, propriedade `respondida` e partição pendentes/histórico do app usam presença do carimbo, sem exigir texto. (`app/chamados/models.py:3071`, `app/chamados/models.py:3074`, `app/chamados/models.py:3584`, `app/chamados/services/pergunta_no_app.py:71`) |
| Par, leitores migrados/atingidos | Não escrevem o par. (`app/chamados/services/fio.py:187`, `app/chamados/services/raiox.py:143`) | `espera_o_texto_do_colaborador` e raio-X leem o par; fila global usa `q_vivas` + face “com texto” + C6; app usa `q_vivas` + C6; cobrança também exige espera/C6. (`app/chamados/services/fio.py:187`, `app/chamados/services/raiox.py:143`, `app/chamados/services/raiox.py:144`, `app/chamados/juizes.py:1700`, `app/chamados/juizes.py:1703`, `app/chamados/services/pergunta_no_app.py:50`, `app/chamados/services/pergunta_no_app.py:58`, `app/chamados/services/cobranca.py:237`, `app/chamados/services/cobranca.py:241`) |
| `respondida_em` como instante | Escritores acima. (`app/chamados/services/disputa_emissao.py:1468`, `app/chamados/services/disputa_emissao.py:1660`, `app/chamados/services/disputa_emissao.py:1681`) | Placar compara instante com cobrança e data local; fechamento agrega máximo; classificação de validação calcula idade. (`app/chamados/services/verbos.py:342`, `app/chamados/services/verbos.py:345`, `app/chamados/veredito_lavrado.py:207`, `app/chamados/veredito_lavrado.py:208`, `app/chamados/services/validacao.py:932`) |
| Par como fato histórico | `DecisaoPergunta.registrar` é a porta das linhas; preservação usa carimbo cru para correspondência `(pergunta, ato, instante)` e copia texto, inclusive vazio. (`app/chamados/models.py:4029`, `app/chamados/models.py:4010`, `app/chamados/models.py:4017`, `app/chamados/models.py:4020`) | `DecisaoPergunta` distingue projeção e fato; `ato_texto` é o texto tocado pelo ato. C19 lê passivo por essa autoridade, cortando `ato_em` pelo horizonte. (`app/chamados/models.py:3930`, `app/chamados/models.py:3939`, `app/chamados/services/tarefa_do_dia.py:400`) |

**Os sete escritores e os dois donos são explícitos:** Q1 contém `_nascer`, `carimbar`, `descarimbar_em_lote`, `save`; Q2 contém `responder_pergunta_disputa_core`, `carimbar_resposta_pela_trilha`, `responder_automaticamente`. DELEGA contém reabertura/ressuscitação; MEMORIA contém desfazer/cópia da ressuscitação. (`app/chamados/tests/test_e3_porta_A_escritor_do_par.py:42`, `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:46`, `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:47`)

O registro Q2 nomeia os donos “colaborador respondeu” e “FATO respondeu”; portanto, a multiplicidade registrada não constitui escritor furtivo descoberto nesta volta. (`app/core/portas.py:258`, `app/core/portas.py:261`, `app/core/portas.py:264`, `app/core/portas.py:266`)

**Não há significado uniforme entre leitores:** carimbo histórico, instante, rótulo de respondida e resposta com conteúdo coexistem; a diferença é operacional no app cru versus raio-X/espera pelo texto. (`app/chamados/models.py:3584`, `app/chamados/models.py:4017`, `app/chamados/services/pergunta_no_app.py:71`, `app/chamados/services/raiox.py:143`, `app/chamados/services/fio.py:187`)

Executei os sete métodos do selo do par isoladamente, por AST em memória com `python3 -B` e `SimpleTestCase` substituído por `unittest.TestCase`: passaram, incluindo mutantes de oitavo escritor e delegação que grava por fora da porta. A varredura encontrou exatamente os sete + dois DELEGA + dois MEMORIA declarados; isso vale no alcance AST que exclui SQL cru, `obj.__dict__` e o script de sombra fora de `app/`. (`app/chamados/tests/test_e3_porta_A_escritor_do_par.py:227`, `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:232`, `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:244`, `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:260`, `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:264`, `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:274`, `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:280`, `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:24`)

## 12/11 22:15 · sessao b4a6e4ce6e49 · - · - · origem=Ronald (tmux)

Conferi as duas árvores com `git -C /home/ronald/saas-hasner show <hash>:<arquivo>`; BASE e CÓPIA existem nesse repositório. [não verificado: disponibilidade futura desse caminho]

| Função | BASE 78994522d | CÓPIA 69a3d1570 |
|---|---|---|
| `_orfas_da_ata(ata)` | `app/escala/services/leitor_celula.py:180` | removida; substituída pela porta pública em `app/escala/services/leitor_celula.py:182` |
| `orfas_da_ata(celula: object) -> list[datetime] \| list[str]` | inexistente, conforme diff das árvores | `app/escala/services/leitor_celula.py:183` |
| `contagem_orfas_da_ata(celula)` | inexistente, conforme diff das árvores | `app/escala/services/leitor_celula.py:192` |
| `grade_da_celula(colab, ini, fim, *, celulas=None, escalas=None)` | `app/escala/services/leitor_celula.py:350` | `app/escala/services/leitor_celula.py:358` |
| `periodos_do_dia(celula, tipo_escala)` | `app/ponto/juiz_batida.py:149` | `app/ponto/juiz_batida.py:150` |
| `_leitura_da_celula(cel)` | `app/ponto/services/cluster_espurio.py:15` | `app/ponto/services/cluster_espurio.py:15` |
| `assinaturas_do_colab(c, ini, fim, so_escala=False)` | `app/ponto/services/esmeril_espelho.py:79` | `app/ponto/services/esmeril_espelho.py:79` |
| `janela(colab, hoje=None)` | `app/ponto/services/esmeril_espelho.py:38` | `app/ponto/services/esmeril_espelho.py:38` |
| `censo(mes, ano, empresas=None)` | `app/ponto/services/vigia_pareador_celula.py:42` | `app/ponto/services/vigia_pareador_celula.py:42` |
| `pendencias(colab, hoje, dias=JANELA_DIAS)` | `app/ponto/services/cluster_espurio.py:153` | `app/ponto/services/cluster_espurio.py:162` |
| `reconciliar_colab(colab, hoje, dias=JANELA_DIAS)` | `app/ponto/services/cluster_espurio.py:176` | `app/ponto/services/cluster_espurio.py:185` |
| `Command.handle(self, *a, **o)` do detector | `app/ponto/management/commands/detectar_cluster_espurio.py:65` | `app/ponto/management/commands/detectar_cluster_espurio.py:65` |

A chamada equivalente para órfãs é `_orfas_da_ata(cel.ata or {})` na BASE e `orfas_da_ata(cel)` na CÓPIA: o chamador real da BASE normaliza `ata` para `{}` e passa o envelope; a CÓPIA passa a célula. BASE `app/escala/services/leitor_celula.py:379`, `:420`; CÓPIA `app/escala/services/leitor_celula.py:428`. Não existe chamada real isolada de contagem na BASE; é necessário declarar essa limitação, sem fingir que existia a função nova. BASE `app/escala/services/leitor_celula.py:180`; CÓPIA `app/escala/services/leitor_celula.py:192`.

`reconciliar_colab` não oferece DRY nas duas árvores: fecha e abre chamados. A alternativa real sem escrita é `pendencias`, usada pelo próprio comando no ramo DRY. BASE `app/ponto/services/cluster_espurio.py:176`, `:189`, `:196`; CÓPIA `app/ponto/services/cluster_espurio.py:185`, `:199`, `:205`; comando `app/ponto/management/commands/detectar_cluster_espurio.py:69`, `:85`. O comando também chama `contador_cluster_encerrado_sem_lastro`; sua saída DRY contém `colab.nome`, portanto não deve ser encaminhada diretamente à saída da sonda. `app/ponto/management/commands/detectar_cluster_espurio.py:88`, `:94`.

`periodos_do_dia(cel, None)` é entrada real utilizada pela folha; o cadastro só alimenta a chave de intervalo nessa modalidade. `app/folha/export.py:1142`, `:1150`. Outro chamador real, o pareador, usa o tipo retornado por `_escalas_da_ata`; escolher sempre a geradora mudaria essa alimentação. `app/ponto/turnos.py:1538`, `:1593`, `:1633`.

Esmeril tem dois modos reais que merecem blocos separados: completo na janela de pendências dada por `janela(c, hoje)` e `so_escala=True` na competência de cada dia. `app/ponto/services/esmeril_espelho.py:206`, `:207`; `app/escala/services/escala_certa.py:38`, `:42`. Para o segundo modo, deduplicar `(colab_id, início_da_competência)` conserva o cache do chamador real. `app/escala/services/escala_certa.py:39`.

Esmeril busca suas próprias células e turnos; aceita somente o colaborador e a janela, portanto `select_related('empresa')` no colaborador é útil, mas prefetch externo de células não alimenta essas queries. `app/ponto/services/esmeril_espelho.py:79`, `:96`, `:99`. `censo` também executa queries internas e aceita competências e ids de empresas, sem parâmetro de objetos pré-carregados. `app/ponto/services/vigia_pareador_celula.py:42`, `:58`, `:70`, `:99`.

O chamador padrão de `censo` agrupa empresas pela competência que contém ontem, segundo o corte de cada empresa; esse é o universo corrente real do comando. `app/ponto/management/commands/vigia_pareador_celula.py:43`, `:48`, `:52`. O próprio `censo` consulta células da janela com um dia de cada lado e pareia turnos na janela inteira. `app/ponto/services/vigia_pareador_celula.py:64`, `:70`, `:102`.

## 12/11 22:15 · sessao f65f59e45115 · - · - · origem=Ronald (tmux)

Conferi com `git show` dos dois commits em `/home/ronald/saas-hasner`. [nao verificado]

| Função | BASE `78994522d` | CÓPIA `69a3d1570` | Assinatura |
|---|---|---|---|
| tripwire | `app/ponto/management/commands/tripwire_celulas.py:199` | mesmo arquivo `:200` | `medir_sobra_com_tipo_trocado(ini=None, fim=None)` |
| chamados | `app/chamados/juizes.py:459` | mesmo arquivo `:459` | `_celula_furada(colaborador, data_iso, *, para='cobranca')` |
| plantio | `app/chamados/services/materializacao.py:143` | mesmo arquivo `:143` | `_plantio_preenche_furo_na_data(colaborador, ts)` |
| derivação do plantio | `app/chamados/services/materializacao.py:333` | mesmo arquivo `:339` | `_ts_tipo_do_plantio(disputa, p, erros, pendencia=None, *, carga=None)` |
| aplicação para leitura | `app/chamados/services/materializacao.py:431` | mesmo arquivo `:437` | `estado_aplicacao_pergunta(disputa, p)` |
| contador novo | arquivo inexistente na BASE | `app/escala/services/ata.py:90` | `contar_nao_lavrada_passado(hoje)` |

- **Tripwire**: caminhador agregado, sem argumento célula/colab. Consulta `CelulaDia` por `data__gte=ini`, `data__lte=fim`, `trabalha=True`, com `select_related('colaborador')` — BASE `tripwire_celulas.py:256`, CÓPIA `:257`. Default resolve competência corrente por `janela_atual(date.today())` — ambas `:245`. O comando real chama sem argumentos — CÓPIA `:393`. Para frota completa, chamar uma vez `medir_sobra_com_tipo_trocado(min_data, max_data)`; retorna `(achados, universo)` — CÓPIA `:301`. Não há resultado individual por célula nessa API; não reconstruir o predicado para fabricá-lo. Pode chamar por dia distinto, usando representante `min(pk)` daquele dia e declarar essa unidade, porém continua sendo resultado agregado de todos os colabs naquele dia. A função é documentada read-only — BASE `:228`, CÓPIA `:229`.
- **`_celula_furada`**: chamar para cada `cel.colaborador`, `cel.data.isoformat()`, separadamente `para='cobranca'` e `para='folha'`. O chamador de produção passa `para` — CÓPIA `app/chamados/models.py:1300`; há usos explícitos folha em `:739` e `:1350`. A função não recebe célula; relê-a em `celula_do_dia_agora` — CÓPIA `app/chamados/juizes.py:487`, helper `:403` com query `:408`. Precarregar a célula no script não alimenta essa query. Pode recursar para véspera no ramo sem trabalho — BASE `:499`, CÓPIA `:505`. Já engole erros e devolve `None` — BASE `:519`, CÓPIA `:525`; o script só contará exceções que escaparem, portanto `None` pode esconder falha interna.
- **Plantio**: zero escrita documentada — ambas `materializacao.py:149`; consulta célula do dia local do timestamp — ambas `:157` e `:171`. Não inventar timestamps de meia-noite para cada célula: a função recebe timestamps derivados das respostas. A derivação real é `_ts_tipo_do_plantio(disputa, p, erros)` — CÓPIA `:339`; omitir `pendencia` (ela só acumula numa lista quando fornecida, `:406`). Chamar para todas as perguntas tipo hora com texto, derivar ts pela função real, e chamar `_plantio_preenche_furo_na_data(p.disputa.colaborador, ts)` quando ts válido cobre um **superset declarado**, não a seleção exata das guardas de produção. Agrupar resultados por colab/célula, preservando `p.pk` como subchave.
- O chamador mais próximo sem escrita é `estado_aplicacao_pergunta` — CÓPIA `:437`, descrito como leitura pura em `:809`. Ele devolve cedo para fora de hora/sem texto/materializada (`:444–454`), deriva ts (`:456`), e só chama plantio se houver vizinha (`:465`). Chamar esse leitor garante que se exerça a mesma seleção dele, mas não todas as chamadas do escritor.
- O outro chamador do plantio é `_materializar_resposta_hora`, que grava `p.save` antes da guarda (`materializacao.py:863`, chamada plantio `:888`); não chamar, mesmo sob rollback. A porta `_materializar_uma` também grava recusa antes de chegar ao ramo de hora (`:1225`), portanto não serve como substituto sem escrita.
- **Contador novo**: chamar a função real na cópia com a mesma data fixa da sonda: `contar_nao_lavrada_passado(hoje)`; ela faz somente `.filter(data__lt=hoje, ata__isnull=True).count()` (`ata.py:95–96`). Na BASE declarar `NAO_DISPONIVEL`, sem substituir por lógica própria; o universo bruto de estados pode contar independentemente para fechar a soma.

Risco de saída: plantio registra warning com `colab.id` e `ts` no `except` — CÓPIA `materializacao.py:191–194`; `_celula_furada` chama `registrar_engolido` em dois excepts (`juizes.py:467`, `:480`), que registra exception completa em logging (`core/observ.py:73–82`). Para cumprir saída sem dados pessoais e sem gravação em arquivos de log, desabilitar logging globalmente durante a sonda, restaurando ao fim.

## 12/11 22:15 · sessao 8259450214e8 · - · - · origem=Ronald (tmux)

Base indisponível como árvore Git: `git show 78994522d:...` falhou porque o workspace não contém repositório. As linhas da base abaixo são reconstruídas pelo `diff_producao.txt` fornecido, não conferidas numa segunda árvore `[nao verificado]`.

| Função | Cópia | Base pelo diff | Chamada |
|---|---|---|---|
| `periodos_do_dia(celula, tipo_escala)` | `app/ponto/juiz_batida.py:150` | `app/ponto/juiz_batida.py:149` | Assinatura mantida; passagem de `None` existe em produção em `app/folha/export.py:1150`. |
| `assinaturas_do_colab(c, ini, fim, so_escala=False)` | `app/ponto/services/esmeril_espelho.py:79` | `app/ponto/services/esmeril_espelho.py:79` | `assinaturas_do_colab(c, *janela(c, ref))`, como `app/ponto/services/esmeril_espelho.py:206` e `:232`. |
| `censo(mes, ano, empresas=None)` | `app/ponto/services/vigia_pareador_celula.py:42` | `app/ponto/services/vigia_pareador_celula.py:42` | `censo(mes, ano, empresas=ids)`, como `app/ponto/management/commands/vigia_pareador_celula.py:52`. |
| `medir_sobra_com_tipo_trocado(ini=None, fim=None)` | `app/ponto/management/commands/tripwire_celulas.py:200` | `app/ponto/management/commands/tripwire_celulas.py:199` | Chamada padrão de produção em `app/ponto/management/commands/tripwire_celulas.py:393`; datas explícitas aceitas na assinatura. |

`periodos_do_dia` declara leitura sem motor nem escrita (`app/ponto/juiz_batida.py:151`). Há quatro contextos reais de `tipo_escala`, portanto medir apenas `None` não cobre o intervalo cadastrado usado pelos outros chamadores (`app/ponto/juiz_batida.py:278`):

- `None`: `app/folha/export.py:1150`; validade documentada em `app/folha/export.py:1142`.
- Escala do carregador `_escalas_da_ata([cid])`: seleção em `app/ponto/turnos.py:1593`; chamada em `app/ponto/turnos.py:1633`. O carregador real faz `select_related('tipo_escala')` e ordena por colaborador, ativa e início (`app/ponto/turnos.py:1538`, `:1558`).
- `motor.escala`: `app/ponto/motor_calculo_v2.py:616`.
- Tipo do vínculo ativo: `app/escala/services/cadastro_realidade.py:256`, `:266`.

Para esmeril, usar todos os colaboradores com células e a função real `janela(c, ref)` preserva a janela do chamador: competências não entregues até ontem (`app/ponto/services/esmeril_espelho.py:38`, `:55`, `:63`). `so_escala=True` não cobre órfãs: retorna antes de A5 (`app/ponto/services/esmeril_espelho.py:102`, `:126`).

O vigia precisa do pareamento da competência inteira, não de fatias diárias (`app/ponto/services/vigia_pareador_celula.py:35`, `:102`). Ele não aceita filtro por colaborador ou célula; recebe mês, ano e empresas (`app/ponto/services/vigia_pareador_celula.py:42`). Seu retorno inclui casos e universo agregado (`app/ponto/services/vigia_pareador_celula.py:123`).

O tripwire é caminhador do gravado (`app/ponto/management/commands/tripwire_celulas.py:240`): filtra `trabalha=True`, pula menos de dois marcos e ata nula, conta o universo interno e devolve `(achados, universo)` (`app/ponto/management/commands/tripwire_celulas.py:257`, `:262`, `:265`, `:268`, `:301`). Sua assinatura não permite filtragem por colaborador/célula (`app/ponto/management/commands/tripwire_celulas.py:200`).

Duração em 4 vCPU, quantidade de colaboradores e custo de queries não podem ser determinados sem execução/telemetria `[nao verificado]`.

## 12/11 22:15 · sessao e89f0ed218fc · - · - · origem=Ronald (tmux)

1. **Pendência não booleana NÃO curada.** O HEAD conserva SQL com `contains=[{'pendente': True}]`, enquanto `marcos_pendentes` aceita qualquer valor truthy; a porta não valida o domínio interno da chave: faz `dict(ata)` e persiste a ata. O modelo continua `JSONField` livre. (`app/chamados/models.py:3403`; `app/ponto/services/lastro.py:150`; `app/ponto/portas/celula.py:995`; `app/ponto/portas/celula.py:1033`; `app/escala/models.py:1778`)

   Contraexemplo anterior permanece: `veredito='concorde'`, via cartório, ata `{"lampadas":[{"hora":"14:20","tipo":"S","acesa":null,"pendente":"sim"}]}`, pergunta sem resposta, validação, materialização ou tranca e sem fato no marco. Python reconhece provisório e retorna viva no degrau 5; SQL não reconhece `"sim"` como `True`, absolve e exclui. Execução PostgreSQL: [não verificado]. (`app/chamados/juizes.py:1391`; `app/chamados/juizes.py:1402`; `app/chamados/juizes.py:1438`; `app/chamados/juizes.py:1452`; `app/chamados/juizes.py:1466`; `app/chamados/models.py:3408`; `app/chamados/models.py:3413`)

   O escritor canônico produz `pendente=True`, portanto não demonstramos que produza `"sim"`; a divergência é do universo admitido pelo modelo. (`app/ponto/services/cartorio.py:915`; `app/escala/models.py:1778`)

   **A sonda integral fornecida não contém contador de pendente não booleano.** Contém `pendente_bool_True=54`, que não exclui outros valores; “0 não-booleano” aparece somente na narrativa da triagem. Medição específica: [não verificado].

2. **Mordidas novas SQL curam os buracos de teste nomeados, sem curar pendência.** [nao verificado]

   | Buraco anterior | Cura e mordida no HEAD |
   |---|---|
   | U+1680 fora da amostra | Adicionado a `TEXTOS` e `VAZIOS_FORA_DO_ASCII`; teste de cobertura exige presença na amostra e em `_BRANCOS`; equivalências reais consultam as duas faces. (`app/chamados/tests/test_e3_forma_sql_do_juiz.py:35`; `app/chamados/tests/test_e3_forma_sql_do_juiz.py:36`; `app/chamados/tests/test_e3_forma_sql_do_juiz.py:98`; `app/chamados/tests/test_e3_forma_sql_do_juiz.py:89`; `app/chamados/tests/test_e3_forma_sql_do_juiz.py:93`) |
   | Mutante local sem testar face de produção | `test_MORDE_a_FACE_DE_PRODUCAO_trocada_pela_forma_sem_trim` patcha as faces reais e exige que os métodos de equivalência falhem. (`app/chamados/tests/test_e3_forma_sql_do_juiz.py:131`; `app/chamados/tests/test_e3_forma_sql_do_juiz.py:144`) |
   | `veredito=''` somente sem materialização | Fixture agora aceita materialização com via legada `''` e lâmpada apagada, verifica que a via não é identidade/tranca; o RED cruza materializada `False/True` com ausência e brancos. Morde restringir a cura a não materializadas. (`app/chamados/tests/test_e3_forma_sql_do_juiz.py:196`; `app/chamados/tests/test_e3_forma_sql_do_juiz.py:206`; `app/chamados/tests/test_e3_forma_sql_do_juiz.py:209`; `app/chamados/tests/test_e3_forma_sql_do_juiz.py:214`; `app/chamados/tests/test_e3_forma_sql_do_juiz.py:221`) |
   | Vizinho vazio tinha texto e não mordia retorno do bug | Novo teste SEM texto compara `''` vivo/incluído e `concorde` morto/excluído. Morde retirar `~Q(celula__veredito='')` e morde ampliar para incluir `concorde`. (`app/chamados/tests/test_e3_forma_sql_do_juiz.py:242`; `app/chamados/models.py:3408`) |

   Bateria com banco e resultados efetivos dessas mordidas: [não verificado].

3. **Mapa de `celula.veredito`.** O vocabulário e o conjunto acusatório vêm do modelo; `None`/`''` representam ausência de julgamento para C6, `q_vivas` e reconciliador. O campo admite os dois; a porta valida vocabulários de lavra, mas pode preservar valor anterior quando a via é administrativa. (`app/escala/models.py:1751`; `app/escala/models.py:1759`; `app/escala/models.py:1767`; `app/escala/models.py:1770`; `app/chamados/juizes.py:1438`; `app/chamados/models.py:3408`; `app/chamados/reconciliador.py:638`; `app/ponto/portas/celula.py:921`; `app/ponto/portas/celula.py:924`; `app/ponto/portas/celula.py:927`)

   O escritor persistente é `ponto/portas/celula.py::lavrar_veredito`, alcançado pelo método delegador do modelo; escreve veredito/via/instante e salva com evento. O cartório e o serviço administrativo passam pela porta. Há também cópia de leitura em memória dentro da própria porta, sem persistência. (`app/ponto/portas/celula.py:974`; `app/ponto/portas/celula.py:1035`; `app/ponto/portas/celula.py:1037`; `app/ponto/portas/celula.py:1040`; `app/escala/models.py:1826`; `app/ponto/services/cartorio.py:1853`; `app/ponto/services/veredito_celula.py:352`; `app/ponto/portas/celula.py:932`; `app/ponto/portas/celula.py:947`)

   Leitores da obra: C6 julga o dia e distingue acusatório/absolvente/provisório; `q_vivas` faz pré-filtro; reconciliador filtra absolvíveis e confirma C6; `_absolvido_pela_celula` exige veredito não vazio/não acusatório e ausência de pendências. (`app/chamados/juizes.py:1438`; `app/chamados/juizes.py:1440`; `app/chamados/juizes.py:1452`; `app/chamados/models.py:3408`; `app/chamados/reconciliador.py:621`; `app/chamados/reconciliador.py:662`; `app/chamados/reconciliador.py:1028`; `app/chamados/reconciliador.py:1030`)

4. **Mapa de `ata.lampadas.pendente`.** O cartório constrói a chave como booleano `True`, com `acesa=None`, para marco ainda não vencido; a porta persiste a ata pelo mesmo ato da lavra. (`app/ponto/services/cartorio.py:907`; `app/ponto/services/cartorio.py:914`; `app/ponto/services/cartorio.py:915`; `app/ponto/portas/celula.py:991`; `app/ponto/portas/celula.py:1033`; `app/ponto/portas/celula.py:1040`)

   `marcos_apagados` usa truthiness para não chamar pendente de apagado; `marcos_pendentes` usa truthiness; C6 e reconciliador perguntam a este último; `q_vivas` exige `True` no JSON. O significado pretendido é comum — julgamento provisório —, mas o domínio reconhecido difere. (`app/ponto/services/lastro.py:129`; `app/ponto/services/lastro.py:138`; `app/ponto/services/lastro.py:150`; `app/chamados/juizes.py:1452`; `app/chamados/reconciliador.py:1030`; `app/chamados/models.py:3403`)

5. **Alargamento de `veredito=''` segue atingindo oito leitores**, incluindo três que não confirmam C6: janela de chamados visíveis, crédito/raio-X e fechamento administrativo. A janela inclui destinos diretamente; raio-X exclui materializadas com `VL_SIM` e separa carimbo/texto; fechamento entrega candidatos à porta administrativa e registra `SILENCIO`. (`app/chamados/juizes.py:1700`; `app/chamados/services/fio.py:254`; `app/chamados/services/pergunta_no_app.py:50`; `app/chamados/services/cobranca.py:237`; `app/chamados/services/vigia_fio.py:757`; `app/chamados/services/janela_colab.py:63`; `app/chamados/services/janela_colab.py:69`; `app/chamados/services/raiox.py:74`; `app/chamados/services/raiox.py:135`; `app/chamados/services/raiox.py:143`; `app/ponto/services/veredito_celula.py:181`; `app/ponto/services/veredito_celula.py:190`; `app/ponto/services/veredito_celula.py:195`)

   Os três chamadores do fechamento continuam veredito administrativo, declaração de dia e validação de folga. (`app/ponto/services/veredito_celula.py:347`; `app/ponto/services/declarar_dia.py:206`; `app/chamados/services/validacao.py:298`)

   A saída fornecida de produção com `celula_veredito_vazio=0` e `candidatas_veredito_vazio=0` sustenta ausência desse estado na foto declarada; execução e atualidade da medição: [não verificado]. Não demonstra equivalência geral nem mede efeitos da mudança de brancos nos outros caminhos. (`app/chamados/models.py:3397`; `app/chamados/models.py:3408`)

6. **Classes anteriores.** As faces de texto derivam a classe de brancos do próprio Python, sem enumeração paralela; não consultam relógio nem escrevem. A pendência mantém uma derivação paralela divergente. (`app/chamados/juizes.py:1526`; `app/chamados/juizes.py:1538`; `app/chamados/juizes.py:1546`; `app/chamados/models.py:3403`; `app/ponto/services/lastro.py:150`)

   O fechamento administrativo mantém porta e ordem declarada colaborador → disputa/chamado → pergunta; o ato `SILENCIO` reutiliza o carimbo de materialização. Não encontrei regressão de ordem nesse caminho. (`app/ponto/services/veredito_celula.py:174`; `app/ponto/services/veredito_celula.py:176`; `app/ponto/services/veredito_celula.py:185`; `app/ponto/services/veredito_celula.py:190`; `app/ponto/services/veredito_celula.py:196`)

   A continência histórica permanece qualificada: C6 projeta carimbos contra `agora`, mas `q_vivas()` não recebe horizonte. (`app/chamados/juizes.py:1368`; `app/chamados/juizes.py:1377`; `app/chamados/juizes.py:1380`; `app/chamados/models.py:3368`)

7. **Item (3), medida prévia.** O dado fornecido não inclui as contagens `NOVA_*`, igualdade de somas, comparação app/supra-juiz ou medida dos 17 sítios; esses resultados são [não verificado]. Existem ainda leitores booleanos SQL fora das duas faces e das declarações, portanto meta zero continua não cumprida no código. (`app/chamados/views.py:1416`; `app/chamados/views_cobrar.py:176`; `app/chamados/services/acoes_disputa.py:95`; `app/chamados/services/disputa_emissao.py:1720`; `app/chamados/services/materializacao.py:1376`)

   O instrumento `diff_e3_sombra` declara escopo de projeções C17/tarefa e limita o relógio do lado C17; não equivale por si à medida completa pedida para esta mudança. (`app/chamados/management/commands/diff_e3_sombra.py:26`; `app/chamados/management/commands/diff_e3_sombra.py:46`; `app/chamados/management/commands/diff_e3_sombra.py:107`)

## 12/11 22:15 · sessao ab8f1cef1a8b · BUG-CADASTRO-C5E · verificador · origem=Code

REPROVA

1. **A catraca ainda deixa uma leitura nova sobreviver e ganhou uma regressão.** O novo `visit_Assign` visita somente o valor atribuído; não percorre o alvo da atribuição. Assim, inserir antes da leitura existente de `reconciliar_apos_vinculo` o mutante abaixo acrescenta uma leitura direta que desaparece do censo. Isso contraria a promessa de “uma leitura nova = vermelho”. (`app/ponto/tests/test_cadastro_futuro_c5e.py:424`, `app/ponto/tests/test_cadastro_futuro_c5e.py:434`, `app/colaboradores/services/pos_vinculo.py:56`, `app/ponto/tests/test_cadastro_futuro_c5e.py:318`)

   ```python
   _cache = {}
   _cache[_tz.localdate()] = 1
   ```

   A contagem deveria subir de 1 para 2, fazendo falhar `test_catraca_so_encolhe` na asserção de ausência de furos; o teto continua 1. (`app/ponto/tests/test_cadastro_futuro_c5e.py:338`, `app/ponto/tests/test_cadastro_futuro_c5e.py:475`) Na execução com substituição da fonte apenas em memória, o censo permaneceu 1, `_furos=[]` e os **19 testes C5E passaram**; não há relatório persistido. [nao verificado]

   A mesma classe permanece para desempacotamento: `_ler, = (_tz.localdate,); _dia_extra = _ler()`. O detector só registra alvos `ast.Name`, enquanto esse alvo é `ast.Tuple`. (`app/ponto/tests/test_cadastro_futuro_c5e.py:428`) Também presume conversão em `_instante=None; _tz.localdate(_instante)`, porque só reconhece `None` literal no argumento. (`app/ponto/tests/test_cadastro_futuro_c5e.py:444`) Ambos conservaram 19/19 verdes em execuções em memória, sem relatório persistido. [nao verificado]

**Informativos — B1..B4 da volta 1**

| Achado | O delta cura? | Mutante e asserção que morde |
|---|---|---|
| **B1 — alias atribuído e argumentos `None`** | **Cura os exemplos exatos; não fecha a classe**, pelo bloqueante acima. A resolução de atribuição simples foi acrescentada, assim como o reconhecimento de `None` posicional e nomeado. (`app/ponto/tests/test_cadastro_futuro_c5e.py:424`, `app/ponto/tests/test_cadastro_futuro_c5e.py:444`) | `_ler_dia=_tz.localdate; _ler_dia()`, `_tz.localdate(None)` e `_tz.localtime(value=None)` passam a produzir `2 > teto 1`; a asserção que acusa é `self.assertEqual(self._furos(contagem), [])`. Há metatestes próprios dessas formas. (`app/ponto/tests/test_cadastro_futuro_c5e.py:475`, `app/ponto/tests/test_cadastro_futuro_c5e.py:521`, `app/ponto/tests/test_cadastro_futuro_c5e.py:533`, `app/ponto/tests/test_cadastro_futuro_c5e.py:542`) |
| **B2 — segundo fechamento com D+1** | **Sim.** A fixture agora contém uma inativa sobreposta, com `pk=3`, e registra o dia recebido pelo suplemento de cada vínculo. (`app/ponto/tests/test_cadastro_futuro_c5e.py:68`, `app/ponto/tests/test_cadastro_futuro_c5e.py:51`) | `c5e_segundo_fechamento_dia_seguinte` altera somente o keyword da segunda chamada. Falha `assertIn((ec.pk, HOJE), self.futuros_por_vinculo)`: chega `(3, D+1)`, falta `(3, D)`. (`app/ponto/tests/test_cadastro_futuro_sem_banco.py:34`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:42`, `app/ponto/tests/test_cadastro_futuro_c5e.py:89`) |
| **B3 — auxiliares aceitavam `None` com efeitos** | **Sim.** As três guardas foram acrescentadas e as funções entraram em `RECUSAM_NONE`. (`app/chamados/services/passivo_validado.py:316`, `app/chamados/services/passivo_validado.py:430`, `app/colaboradores/services/vinculo.py:1096`, `app/ponto/tests/test_cadastro_futuro_c5e.py:126`) | Remover cada guarda, pelo mutante `c5e_sem_guarda_none:<função>`, faz falhar as recusas esperadas em `desfazer` e `_trabalhar_o_par`. Em `reparar`, falha a regex `^reparar: hoje=None`: a recusa chega tarde, pela delegada. (`app/ponto/tests/test_cadastro_futuro_sem_banco.py:45`, `app/ponto/tests/test_cadastro_futuro_c5e.py:191`, `app/ponto/tests/test_cadastro_futuro_c5e.py:231`, `app/ponto/tests/test_cadastro_futuro_c5e.py:242`) |
| **B4 — `None`, expansão e arquivos de teste** | **Sim, para os defeitos triados.** Exige `hoje` nomeado, rejeita `None` literal e exclui `tests.py` e diretórios `tests`. (`app/ponto/tests/test_cadastro_futuro_c5e.py:263`, `app/ponto/tests/test_cadastro_futuro_c5e.py:278`) | Chamadas reconhecidas com `hoje=None`, `**{}` ou somente expansão entram em `faltam`, fazendo falhar a asserção principal `faltam == []`. Os metatestes exigem essas denúncias. Reintroduzir arquivos de teste no universo faz falhar a expectativa de zero chamadas vistas. (`app/ponto/tests/test_cadastro_futuro_c5e.py:286`, `app/ponto/tests/test_cadastro_futuro_c5e.py:294`, `app/ponto/tests/test_cadastro_futuro_c5e.py:298`, `app/ponto/tests/test_cadastro_futuro_c5e.py:311`) |

Execuções sem artefato persistido: suíte C5E **19/19**; suíte `test_cadastro_futuro_sem_banco` **43/43**; os três exemplos exatos de B1 e os quatro mutantes de B2/B3 acima foram rejeitados. [nao verificado] [nao verificado]

**Guardas novas e chamadores de produção**

As três guardas levantam **antes de qualquer consulta, escrita ou chamada a outra porta**, e nenhuma das três funções tem decorator envolvendo sua entrada. (`app/chamados/services/passivo_validado.py:289`, `app/chamados/services/passivo_validado.py:316`, `app/chamados/services/passivo_validado.py:422`, `app/chamados/services/passivo_validado.py:430`, `app/colaboradores/services/vinculo.py:1093`, `app/colaboradores/services/vinculo.py:1096`)

| Função | Primeiro trabalho posterior à guarda | Todos os chamadores de produção encontrados |
|---|---|---|
| `reparar` | Cria o placar; depois normaliza parâmetros e chama `classificar`. (`app/chamados/services/passivo_validado.py:319`, `app/chamados/services/passivo_validado.py:323`, `app/chamados/services/passivo_validado.py:327`) | Duas chamadas no comando: ensaio e execução normal. Ambas recebem `hoje` produzido por `_agora=timezone.localtime(); hoje=_agora.date()`. (`app/chamados/management/commands/materializar_passivo_validado.py:87`, `app/chamados/management/commands/materializar_passivo_validado.py:89`, `app/chamados/management/commands/materializar_passivo_validado.py:104`, `app/chamados/management/commands/materializar_passivo_validado.py:113`) |
| `_trabalhar_o_par` | Chama `_dias_do_par` e depois `_plantar_o_dia`. (`app/chamados/services/passivo_validado.py:433`, `app/chamados/services/passivo_validado.py:434`) | Única chamada em `reparar`, passando o mesmo `hoje`, já protegido pela guarda da entrada. (`app/chamados/services/passivo_validado.py:316`, `app/chamados/services/passivo_validado.py:375`, `app/chamados/services/passivo_validado.py:376`) |
| `desfazer_ajuste_do_sistema` | Compara `volta` com `novo.data_inicio`; depois consulta células, exclui/reabre ou delega ao fechamento. (`app/colaboradores/services/vinculo.py:1099`, `app/colaboradores/services/vinculo.py:1101`, `app/colaboradores/services/vinculo.py:1103`, `app/colaboradores/services/vinculo.py:1106`) | Única chamada em `escala_auto_executor.desfazer`, depois de `hoje = hoje or timezone.localdate()`. Portanto, nesse caminho, uma entrada `None` é normalizada antes da chamada. (`app/escala/services/escala_auto_executor.py:127`, `app/escala/services/escala_auto_executor.py:145`) |

**Não encontrei omissão nem fluxo que entregue `None` a essas três funções nos chamadores acima.** O selo, sozinho, verifica sintaxe; a conclusão sobre os valores depende também desse rastreamento dos produtores. (`app/ponto/tests/test_cadastro_futuro_c5e.py:278`, `app/chamados/management/commands/materializar_passivo_validado.py:89`, `app/chamados/services/passivo_validado.py:316`, `app/escala/services/escala_auto_executor.py:127`)

**Quem escreve, quem lê e significado dos valores**

No último delta produtivo, a mudança é exclusivamente a leitura de `hoje` para recusar `None`; não acrescenta escrita de campo persistido. (`app/chamados/services/passivo_validado.py:316`, `app/chamados/services/passivo_validado.py:430`, `app/colaboradores/services/vinculo.py:1096`)

| Valor/campo relevante ao delta | Produtor/escritor → leitores |
|---|---|
| `hoje` do passivo | O comando extrai o **dia civil local** de um instante; `reparar` valida e repassa; `_trabalhar_o_par` valida e repassa à exportada; esta repassa à regeneração. (`app/chamados/management/commands/materializar_passivo_validado.py:87`, `app/chamados/management/commands/materializar_passivo_validado.py:89`, `app/chamados/services/passivo_validado.py:376`, `app/chamados/services/passivo_validado.py:445`, `app/ponto/portas/regen_exportada.py:179`) |
| `hoje` do desfazer | O executor normaliza o dia; a nova guarda o valida; o ramo de fechamento o repassa a `fechar_vigencia`, que o entrega ao suplemento. O suplemento seleciona futuro a partir de `hoje+1`. (`app/escala/services/escala_auto_executor.py:127`, `app/colaboradores/services/vinculo.py:1096`, `app/colaboradores/services/vinculo.py:1107`, `app/colaboradores/services/vinculo.py:661`, `app/ponto/services/cadastro_futuro.py:27`) |
| `volta`, `dia` e `ts` | São valores separados de `hoje`: `volta` é o dia de efeito; `dia` é o dia da célula do plantio; `ts` é instante, convertido apenas para a comparação de competência. Não ganharam significado novo no delta. (`app/escala/services/escala_auto_executor.py:143`, `app/chamados/services/passivo_validado.py:344`, `app/chamados/services/passivo_validado.py:348`, `app/chamados/services/passivo_validado.py:353`) |
| `ativa`, `data_fim`, `data_inicio`, `pk` da sobreposta | A fixture cria a inativa; o corpo real de `fechar_vigencia` escreve `ativa=False,data_fim`; as novas asserções leem o fim e o par `(pk, hoje)`. (`app/ponto/tests/test_cadastro_futuro_c5e.py:68`, `app/colaboradores/services/vinculo.py:639`, `app/ponto/tests/test_cadastro_futuro_c5e.py:87`, `app/ponto/tests/test_cadastro_futuro_c5e.py:89`) |
| `recebidos`, `futuros_por_vinculo` | Os spies escrevem os dias e os pares vínculo/dia; os testes verificam cada fechamento e a igualdade de todos os dias recebidos. São observações da fixture. (`app/ponto/tests/test_cadastro_futuro_c5e.py:50`, `app/ponto/tests/test_cadastro_futuro_c5e.py:51`, `app/ponto/tests/test_cadastro_futuro_c5e.py:57`, `app/ponto/tests/test_cadastro_futuro_c5e.py:89`, `app/ponto/tests/test_cadastro_futuro_c5e.py:91`) |
| `gravadas`, `local['plantada']`, `local['dias']` | O dublê de plantio escreve essas estruturas; os testes leem para exigir ausência de efeitos com `None` e efeitos alcançáveis com dia válido. (`app/ponto/tests/test_cadastro_futuro_c5e.py:211`, `app/ponto/tests/test_cadastro_futuro_c5e.py:233`, `app/ponto/tests/test_cadastro_futuro_c5e.py:237`, `app/ponto/tests/test_cadastro_futuro_c5e.py:245`, `app/ponto/tests/test_cadastro_futuro_c5e.py:248`) |
| `RECUSAM_NONE` | A lista passa a incluir os três auxiliares; o teste de forma a lê para exigir guarda na entrada. (`app/ponto/tests/test_cadastro_futuro_c5e.py:123`, `app/ponto/tests/test_cadastro_futuro_c5e.py:150`) |
| `fontes`, caminhos, aliases e keywords do censo | `_censo` recebe/lê fontes, exclui caminhos de teste, resolve nomes e examina o keyword `hoje`; produz `faltam/vistos`, lidos pelas asserções. Não interpreta o significado runtime do valor. (`app/ponto/tests/test_cadastro_futuro_c5e.py:255`, `app/ponto/tests/test_cadastro_futuro_c5e.py:263`, `app/ponto/tests/test_cadastro_futuro_c5e.py:267`, `app/ponto/tests/test_cadastro_futuro_c5e.py:278`, `app/ponto/tests/test_cadastro_futuro_c5e.py:283`) |
| `Visita.funcoes`, alvos e argumentos AST | O visitante copia o mapa por escopo, registra/remove aliases e consulta esse mapa nas chamadas. A travessia incompleta dos alvos é o bloqueante. (`app/ponto/tests/test_cadastro_futuro_c5e.py:403`, `app/ponto/tests/test_cadastro_futuro_c5e.py:415`, `app/ponto/tests/test_cadastro_futuro_c5e.py:424`, `app/ponto/tests/test_cadastro_futuro_c5e.py:441`) |
| AST dos mutantes | O loader lê `MUTANTE_CADASTRO`, altera o keyword do segundo fechamento ou remove uma guarda, e compila os corpos em memória. (`app/ponto/tests/test_cadastro_futuro_sem_banco.py:33`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:42`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:53`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:114`) |

Nos caminhos produtivos atingidos pelo delta, **`hoje` continua sendo dia civil local**, separado do dia de efeito, do dia da célula e do instante. Não encontrei novo campo com dois significados. (`app/chamados/management/commands/materializar_passivo_validado.py:89`, `app/escala/services/escala_auto_executor.py:143`, `app/chamados/services/passivo_validado.py:348`)

**Classes de reprovação e demais limites**

- **Selo/RED sem mordida:** presente, no bloqueante 1. A regressão fica no analisador acrescentado pelo delta. (`app/ponto/tests/test_cadastro_futuro_c5e.py:434`)
- **Segundo juiz, segundo escritor, relógio novo fora da borda, derivação paralela e mudança de ordem de trava:** não identifiquei introdução dessas classes nas alterações produtivas, compostas pelas três recusas de entrada. (`app/chamados/services/passivo_validado.py:316`, `app/chamados/services/passivo_validado.py:430`, `app/colaboradores/services/vinculo.py:1096`)
- **Pouso antes do pré-requisito:** não encontrei chamador dessas três funções quebrado pela guarda nova; permanece incompleto o pré-requisito da catraca que deve denunciar leitura nova. (`app/chamados/management/commands/materializar_passivo_validado.py:106`, `app/chamados/management/commands/materializar_passivo_validado.py:114`, `app/chamados/services/passivo_validado.py:376`, `app/escala/services/escala_auto_executor.py:145`, `app/ponto/tests/test_cadastro_futuro_c5e.py:475`)
- **Interiores adiados:** não os conto como bloqueantes nem proponho tornar `hoje` obrigatório neles; o próprio selo registra a proibição de ampliar esta cura e a obra `RELOGIO-NA-BORDA`. (`app/ponto/tests/test_cadastro_futuro_c5e.py:316`)
- **Produção:** ocorrência, volume atingido, estado do banco, logs e implantação efetiva do hash informado não são demonstrados pelo dado fornecido. [nao verificado]

## 12/11 22:13 · sessao 1424fdde1b9f · - · - · origem=Ronald (tmux)

B1 foi curado para os mutantes exatos da volta 1, mas a promessa “uma leitura nova = vermelho” continua furada: o detector só resolve atribuições cujo alvo é `ast.Name`; desempacotamento de tupla escapa. (`app/ponto/tests/test_cadastro_futuro_c5e.py:428`; `app/ponto/tests/test_cadastro_futuro_c5e.py:318`)

Mutante inserido imediatamente antes de `app/colaboradores/services/pos_vinculo.py:56`:

```python
_ler, = (_tz.localdate,)
_dia_extra = _ler()
```

A leitura existente permanece, portanto há uma chamada adicional ao mesmo relógio; o detector ignora `_ler()` porque não registra o alvo da tupla. As asserções que deveriam acusar são `_furos(contagem) == []` e igualdade do censo com o teto. (`app/colaboradores/services/pos_vinculo.py:56`; `app/ponto/tests/test_cadastro_futuro_c5e.py:428`; `app/ponto/tests/test_cadastro_futuro_c5e.py:475`; `app/ponto/tests/test_cadastro_futuro_c5e.py:479`)

Executei três mutantes mediante substituição de `Path.read_text` somente em memória; cada variante manteve **19/19 testes verdes**, `_furos=[]`: tupla acima; `_instante=None; _tz.localdate(_instante)`; alias seguido de sobrescrita dentro de `if False`. Não há relatório persistido dessas execuções. [nao verificado] [nao verificado]

O segundo escape decorre de considerar qualquer argumento diferente de `ast.Constant(None)` uma conversão, sem resolver valor de variável. O terceiro decorre de visitar incondicionalmente o ramo e apagar o alias. (`app/ponto/tests/test_cadastro_futuro_c5e.py:444`; `app/ponto/tests/test_cadastro_futuro_c5e.py:445`; `app/ponto/tests/test_cadastro_futuro_c5e.py:432`)

B2 tem agora efeito observável nos dois fechamentos: a inativa sobreposta é criada com `pk=3`, o spy registra `(pk, hoje)` e a asserção exige `(pk, HOJE)` para ambos os vínculos. O novo vínculo usa o default `pk=2`, portanto não há colisão com a sobreposta. (`app/ponto/tests/test_cadastro_futuro_c5e.py:68`; `app/ponto/tests/test_cadastro_futuro_c5e.py:51`; `app/ponto/tests/test_cadastro_futuro_c5e.py:88`; `app/ponto/tests/test_cadastro_futuro_sem_banco.py:254`)

B4 exato está corrigido: o censo exclui `tests.py` e diretórios `tests`, exige keyword `hoje` nomeado e recusa `None` literal; há metatestes para `None`, expansões e arquivos de testes. (`app/ponto/tests/test_cadastro_futuro_c5e.py:263`; `app/ponto/tests/test_cadastro_futuro_c5e.py:278`; `app/ponto/tests/test_cadastro_futuro_c5e.py:294`; `app/ponto/tests/test_cadastro_futuro_c5e.py:298`; `app/ponto/tests/test_cadastro_futuro_c5e.py:307`)

## 12/11 22:12 · sessao e3dd67bb907d · - · - · origem=Ronald (tmux)

Há uma regressão bloqueante no detector: `visit_Assign` visita somente o valor atribuído, sem visitar os alvos. Uma leitura direta em `cache[timezone.localdate()] = 1` desaparece do censo; em execução em memória, `leituras_de_relogio` devolveu `[]`. O caminho responsável é `app/ponto/tests/test_cadastro_futuro_c5e.py:424`, `app/ponto/tests/test_cadastro_futuro_c5e.py:434`. Resultado da execução sem artefato persistido: [nao verificado].

As três guardas produtivas são a primeira instrução executável, sem decorator e antes de consultas, normalização, plantio, escrita ou delegação:

- `reparar`: guarda `app/chamados/services/passivo_validado.py:316`; primeira chamada posterior `app/chamados/services/passivo_validado.py:323`.
- `_trabalhar_o_par`: guarda `app/chamados/services/passivo_validado.py:430`; iteração e plantio posteriores `app/chamados/services/passivo_validado.py:433`.
- `desfazer_ajuste_do_sistema`: guarda `app/colaboradores/services/vinculo.py:1096`; primeira consulta `app/colaboradores/services/vinculo.py:1101`; escritas `app/colaboradores/services/vinculo.py:1103`; delegação `app/colaboradores/services/vinculo.py:1106`.

O delta produtivo não introduz juiz, escritor, campo persistido, leitura de relógio ou ordem nova de locks: acrescenta somente essas três recusas. Os efeitos posteriores conservam suas portas e campos anteriores. (`app/chamados/services/passivo_validado.py:316`; `app/chamados/services/passivo_validado.py:430`; `app/colaboradores/services/vinculo.py:1096`)

`hoje` mantém significado de dia civil local nos caminhos das guardas:

- Comando lê um instante local e extrai `.date()`: `app/chamados/management/commands/materializar_passivo_validado.py:87`; `app/chamados/management/commands/materializar_passivo_validado.py:89`. Passa a `reparar` em `:104` e `:113`, que passa a `_trabalhar_o_par` em `app/chamados/services/passivo_validado.py:375`, depois à exportada em `:444`, e à célula em `app/ponto/portas/regen_exportada.py:179`.
- Executor de desfazer normaliza o dia em `app/escala/services/escala_auto_executor.py:127`, usa esse dia para prazo em `:130`, calcula separadamente `volta` em `:143` e passa `hoje` à função em `:145`. O fechamento repassa ao suplemento em `app/colaboradores/services/vinculo.py:660`, cujo futuro começa em `hoje + 1` em `app/ponto/services/cadastro_futuro.py:27`.

No passivo, o dia da célula e o instante seguem campos separados: `dia` deriva em `app/chamados/services/passivo_validado.py:344`, `ts` é convertido em `:348`, e ambos permanecem distintos na tupla de `:353`. Não há novo duplo significado de `hoje` nesse delta. (`app/chamados/services/passivo_validado.py:344`; `app/chamados/services/passivo_validado.py:348`; `app/chamados/services/passivo_validado.py:353`)

## 12/11 22:12 · sessao e16873a6fa0d · - · - · origem=Ronald (tmux)

Guardas B3 corrigidas e com mordida:

- `reparar`: guarda é primeira instrução, antes de criar placar, normalizar ou chamar `classificar`; não há decorator. (`app/chamados/services/passivo_validado.py:289`, `:316`, `:319`, `:323`, `:327`)
- `_trabalhar_o_par`: guarda precede `_dias_do_par`, plantio e porta exportada. (`app/chamados/services/passivo_validado.py:422`, `:430`, `:433`, `:434`, `:444`)
- `desfazer_ajuste_do_sistema`: guarda precede até a leitura de `novo.data_inicio`, consultas, exclusão, reabertura e fechamento. (`app/colaboradores/services/vinculo.py:1093`, `:1096`, `:1099`, `:1101`, `:1103`, `:1104`, `:1106`)

Censo dos chamadores de produção dessas três funções:

| Destino | Chamador e origem de `hoje` |
|---|---|
| `reparar` | `app/chamados/management/commands/materializar_passivo_validado.py:104` e `:113`; ambas passam `hoje` nomeado (`:106`, `:114`), produzido por `_agora=timezone.localtime()` e `_agora.date()` (`:87`, `:89`). |
| `_trabalhar_o_par` | `app/chamados/services/passivo_validado.py:375`; recebe o mesmo `hoje` (`:376`), já recusado se `None` (`:316`). |
| `desfazer_ajuste_do_sistema` | `app/escala/services/escala_auto_executor.py:145`; recebe `hoje` após `hoje = hoje or timezone.localdate()` (`:127`). |

Não há omissão nem fluxo `None` nesses chamadores identificados: o comando produz `date`, a delegação preserva o argumento guardado, e o executor normaliza antes da chamada. (`app/chamados/management/commands/materializar_passivo_validado.py:87`, `:89`, `:106`, `:114`; `app/chamados/services/passivo_validado.py:316`, `:376`; `app/escala/services/escala_auto_executor.py:127`, `:145`)

Semântica: a origem do passivo é o dia civil local extraído de um instante local, e ele chega intacto à porta exportada e desta à regeneração. (`app/chamados/management/commands/materializar_passivo_validado.py:87`, `:89`; `app/chamados/services/passivo_validado.py:376`, `:445`; `app/ponto/portas/regen_exportada.py:179`) No desfazer, `volta` é o dia de efeito, distinto de `hoje`, calculado como `max(inicio, hoje+1)`; a guarda não confunde os dois. (`app/escala/services/escala_auto_executor.py:143`, `:145`; `app/colaboradores/services/vinculo.py:1096`, `:1099`, `:1107`) Configuração local: `America/Sao_Paulo`, `USE_TZ=True`. (`app/config/settings/base.py:93`, `:95`)

Executei `python3 -B` com `PYTHONDONTWRITEBYTECODE=1`: `FormaDaPortaTest` passou 6/6; mutantes de remoção das guardas, definidos no loader, falharam individualmente. (`app/ponto/tests/test_cadastro_futuro_sem_banco.py:45`, `:53`; `app/ponto/tests/test_cadastro_futuro_c5e.py:134`) Evidência da mordida:

- Remover guarda de `desfazer_ajuste_do_sistema`: `assertRaisesRegex` falha com `ValueError not raised`. (`app/ponto/tests/test_cadastro_futuro_c5e.py:191`)
- Remover guarda de `reparar`: a delegada levanta mensagem de `_trabalhar_o_par`, e a regex `^reparar: hoje=None` falha; portanto proteção somente delegada não passa. (`app/ponto/tests/test_cadastro_futuro_c5e.py:231`; `app/chamados/services/passivo_validado.py:431`)
- Remover guarda de `_trabalhar_o_par`: `assertRaisesRegex` falha com `ValueError not raised`. (`app/ponto/tests/test_cadastro_futuro_c5e.py:242`)

Os controles com dia válido demonstram exclusão/reabertura e plantio alcançáveis nas fixtures; os testes não dependem apenas de uma recusa num caminho sem efeito. (`app/ponto/tests/test_cadastro_futuro_c5e.py:199`, `:200`, `:201`, `:202`, `:236`, `:237`, `:247`, `:248`)

## 12/11 22:11 · sessao 90d5d98a60c5 · TIPO-NA-FONTE · construtor · origem=Code

Cura escrita, mas a **igualdade universal para ISO permanece pendente**: com duas órfãs ISO, o cluster da base lança `ValueError`; a cópia mede assinatura. [Contraexemplo executado](</home/ronald/wt-codex-tipo-na-fonte-184856/app/escala/tests/tipo_item3_volta4_red.txt:41>).

A entrega está documentada com:

- [Os oito leitores: ramo NAO_LAVRADA, legado, valores e linhas da base](</home/ronald/wt-codex-tipo-na-fonte-184856/app/escala/tests/tipo_item3_volta4_relato.txt:13>).
- [Arquivo::função alterados, contador, fixtures e selos](</home/ronald/wt-codex-tipo-na-fonte-184856/app/escala/tests/tipo_item3_volta4_relato.txt:42>).
- [Cada mutante, teste que o pega e comando](</home/ronald/wt-codex-tipo-na-fonte-184856/app/escala/tests/tipo_item3_volta4_relato.txt:85>). [nao verificado]
- [O que não foi feito e por quê](</home/ronald/wt-codex-tipo-na-fonte-184856/app/escala/tests/tipo_item3_volta4_relato.txt:129>).

Dirigidos locais: **88 testes, um skip de ORM, rc=0**. [Resultado e comando](</home/ronald/wt-codex-tipo-na-fonte-184856/app/escala/tests/tipo_item3_volta4_dirigidos.txt:1>). Os dois REDs reprovaram com o acessor anterior reinstalado em memória. [Registro](</home/ronald/wt-codex-tipo-na-fonte-184856/app/escala/tests/tipo_item3_volta4_red.txt:5>). [nao verificado]

Suite canônica e DIFF de frota não executados; sem certificação de pouso. [Limites](</home/ronald/wt-codex-tipo-na-fonte-184856/app/escala/tests/tipo_item3_volta4_relato.txt:129>). [nao verificado]

## 12/11 21:56 · sessao 37e2d0869710 · - · - · origem=Ronald (tmux)

Provas históricas lidas com `git show 0a747193e:app/<arquivo>`:

| Leitor | Ata NULA | Legado HH:MM e prova na base |
|---|---|---|
| `leitor_celula._orfas_da_ata` | Grade normaliza para `{}` em `app/escala/services/leitor_celula.py:379`; órfãs `[]` | Repassa strings; fallback só para contagem antiga: `:187-190`. |
| `juiz_batida.periodos_do_dia` | `orfas=[]` | Normaliza ata e repassa strings em `app/ponto/juiz_batida.py:157-164`. |
| `cluster_espurio._leitura_da_celula` | `None` | Insumos em `app/ponto/services/cluster_espurio.py:27-32`; duas órfãs a ≤15 min e lâmpada apagada retornam `(orfas,n_missing)` em `:33-36`. |
| `esmeril_espelho.assinaturas_do_colab` | Zero contribuição A5 | Leitura/minutos/buckets em `app/ponto/services/esmeril_espelho.py:125-128`; A5 aparece com ≥3 no bucket em `:139-141`. |
| `vigia_pareador_celula.censo` | Zero células com órfã e zero colaboradores por essa informação | Leitura em `app/ponto/services/vigia_pareador_celula.py:73-80`; borda compara `HH:MM` em `:109-112`. |
| `tripwire_celulas.medir_sobra_com_tipo_trocado` | Pula célula; universo zero | Gate em `app/ponto/management/commands/tripwire_celulas.py:258-263`; HH:MM chega à peneira em `:264-265`. |
| `chamados.juizes._celula_furada` | Retorno integral **`None`**, antes de contar órfãs | Normaliza ata em `app/chamados/juizes.py:493`; ausência de lâmpadas retorna `None` em `:505-506`; órfãs/contagem entram em `:510-517`. |
| `materializacao._plantio_preenche_furo_na_data` | Sem lâmpadas pula, terminando `False` | Gate em `app/chamados/services/materializacao.py:172-176`; HH:MM soma às marcas em `:180-183`. |

Nuances para o selo:

- “Sete zero” é contribuição de órfãs, não necessariamente retorno integral. `_celula_furada(None)` retorna `None`; plantio retorna `False`; tripwire retorna `([],0)`. As linhas acima comprovam.
- ISO literal na base não tem comportamento temporal uniforme. Cluster com duas strings ISO e lâmpada apagada chega a `int(h[3:5])`, que falha em ISO `2026-...` (`app/ponto/services/cluster_espurio.py:34` da base). Uma órfã ISO evita esse ramo e retorna `()` (`:33-37`). Não afirmar igualdade universal ISO base/cópia.

Restantes adaptações necessárias para HH:MM:

- Propositor: recuperar validação `_tem_hora` e casadas por `hora` no ramo legado; base `app/ponto/propositor.py:24-31,52-55`. Hoje `dia_degenerado` exige `instante`. `insumos_atribuicao` deve conservar slots/marcas HH:MM para o ramo legado.
- Classificador: strings precisam diferença de minutos; base `app/escala/orfa_classe.py:31-32,44,74`. Hoje `_runs_de_rajada` subtrai datetimes.
- Ausência parcial: ramo string deve usar `_minutos_do_dia(str(h))`, como base `app/ponto/turnos.py:2667`; hoje exige data e subtrai datetime.
- PDF: base compara `timezone.localtime(entrada).strftime('%H:%M')` (`app/relatorios/pdf_espelho.py:178`); ramo legado deve conservar essa comparação.
- Calendário: legado deve respeitar órfãs da própria data e comparar HH:MM; base `app/colaboradores/services/calendario.py:622,625-627`. Hoje coleta instantes de todas as datas.

Todos os oito sítios atuais ainda passam pelo acessor; vigia transforma `None` em `{}` **antes** dele (`app/ponto/services/vigia_pareador_celula.py:75-76` atual), impedindo reconhecer `NAO_LAVRADA`.

## 12/11 21:54 · sessao 2f8bf08376aa · E3-FORMA-SQL · sonda · origem=Code

Saída integral fornecida, sem nova execução [nao verificado]:

```text
sonda_leitura: sonda_e3_v3.py [e3c78f262addb5bc] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

candidatas_veredito_vazio                   0  amostra=[]
celula_veredito_vazio                       0  amostra=[]
pendente_bool_True                         54  amostra=[136970, 130883, 131162, 131162, 131162, 122296, 127887, 139760, 122542, 132309]
perguntas_total                         49192  amostra=[]
total_erros=0
```

Não executei o envelope porque ele escreve arquivos, contrariando sua regra de somente leitura ([bin/sonda_leitura.sh:132](/home/ronald/codex_ws/78994522d93958fd6c687f4228ee735b1ad04f9a/bin/sonda_leitura.sh:132)).

Igualdade das somas: [nao verificado]. As quatro contagens `NOVA_*` solicitadas não constam do dado fornecido [nao verificado].

## 12/11 21:52 · sessao 3bec7d2c5253 · E3-FORMA-SQL · verificador · origem=Code

**REPROVA**, por dois bloqueantes: ainda existe estado permitido pelo modelo que quebra a continência de `q_vivas`; a catraca ainda erra nos dois sentidos entre VALOR e RESPONDEDOR. (`app/chamados/models.py:3403`; `app/ponto/services/lastro.py:150`; `app/chamados/tests/test_e3_catraca_respondedores.py:175`, `app/chamados/tests/test_e3_catraca_respondedores.py:196`)

1. **Bloqueante — pendência verdadeira para o Python pode não ser pendência para o SQL.**

   Contraexemplo: pergunta não validada, destrancada, sem resposta, sem materialização, sem via terminal; célula `veredito='concorde'`, `veredito_via='cartorio'`, com esta ata e sem fato no marco. O modelo admite esse JSON sem impor tipo booleano a `pendente`. (`app/escala/models.py:1778`; `app/chamados/juizes.py:1391`, `app/chamados/juizes.py:1402`, `app/chamados/juizes.py:1434`)

   ```python
   {"lampadas": [
       {"hora": "14:20", "tipo": "S", "acesa": None, "pendente": "sim"}
   ]}
   ```

   `marcos_pendentes` aceita `"sim"` por truthiness. O juiz considera o veredito provisório, prossegue e retorna **VIVA** porque não há materialização. O SQL procura especificamente `{'pendente': True}`; não reconhecendo a string como booleano, considera a célula absolvente e exclui a pergunta. Essa é uma dedução do Q; execução no Postgres é **[não verificado]**. (`app/ponto/services/lastro.py:148`; `app/chamados/juizes.py:1452`, `app/chamados/juizes.py:1466`; `app/chamados/models.py:3403`, `app/chamados/models.py:3408`, `app/chamados/models.py:3413`)

   O cartório canônico escreve `pendente=True`; portanto, não demonstrei que ele produza o contraexemplo. Existência desse estado em produção: **[não verificado]**. A divergência entre os dois leitores permanece demonstrável no universo solicitado, o do modelo. (`app/ponto/services/cartorio.py:915`; `app/escala/models.py:1778`)

2. **Bloqueante — as três fugas literais da catraca foram corrigidas, mas a classe continua aberta.**

   Este uso retorna exatamente “há carimbo?”, porém o classificador dá **VALOR**: `usa()` percorre também o ramo morto. (`app/chamados/tests/test_e3_catraca_respondedores.py:175`, `app/chamados/tests/test_e3_catraca_respondedores.py:182`)

   ```python
   return p.respondida_em is not None and (
       True or p.respondida_em.isoformat()
   )
   ```

   Há também uma fuga menos artificial: o agregado de **outro queryset**, dentro de uma subconsulta, concede VALOR ao filtro booleano externo. `cadeia_do_queryset` limita a subida, mas o `ast.walk` posterior invade todos os argumentos dessa cadeia. (`app/chamados/tests/test_e3_catraca_respondedores.py:204`, `app/chamados/tests/test_e3_catraca_respondedores.py:236`)

   ```python
   return qs.filter(
       respondida_em__isnull=False,
       pk__in=other.values("pk")
           .annotate(m=Max("respondida_em"))
           .filter(m__gt=d)
           .values("pk"),
   ).exists()
   ```

   No sentido inverso, esta serialização recebe **RESPONDEDOR**: o caminho `UnaryOp(Not)` retorna `DECIDE` sem reconhecer a guarda do instante no ramo correspondente. (`app/chamados/tests/test_e3_catraca_respondedores.py:196`)

   ```python
   return None if not p.respondida_em else p.respondida_em.isoformat()
   ```

   Também recebe RESPONDEDOR `p.respondida_em is not None and str(p.respondida_em)`: chamada a `str()` não está entre os usos reconhecidos como valor. Esses resultados foram reproduzidos com o classificador extraído em memória. (`app/chamados/tests/test_e3_catraca_respondedores.py:158`, `app/chamados/tests/test_e3_catraca_respondedores.py:175`)

**Curas dos achados 1, 3, 5 e 6 da volta 1:**

| Achado | O que mudou e resultado |
|---|---|
| **1 — veredito vazio** | **Curado no caso identificado.** `_celula_absolve` passou a exigir veredito diferente de `''`, alinhando-se ao `if cel.veredito`. O teste força o vazio e exige inclusão. Não resta aquele contraexemplo; resta o bloqueante 1 acima. (`app/chamados/models.py:3408`; `app/chamados/juizes.py:1438`; `app/chamados/tests/test_e3_forma_sql_do_juiz.py:181`, `app/chamados/tests/test_e3_forma_sql_do_juiz.py:188`) |
| **3 — prosa e leitores** | **Corrigido o comentário:** reconhece que o degrau 6 também lê resposta via materialização/slot. O censo dos oito leitores está abaixo; conteúdo do corpo do commit é **[não verificado]**. (`app/chamados/models.py:3393`; `app/chamados/juizes.py:1473`, `app/chamados/juizes.py:360`) |
| **5 — classificador** | **Curados os três exemplos anteriores**, pela comparação do objeto com `ast.dump`, seleção do ramo de `IfExp` e delimitação da cadeia. **Não curada a classe**, pelos contraexemplos do bloqueante 2. (`app/chamados/tests/test_e3_catraca_respondedores.py:173`, `app/chamados/tests/test_e3_catraca_respondedores.py:179`, `app/chamados/tests/test_e3_catraca_respondedores.py:204`) |
| **6 — cobertura dos selos** | **Cobertos os buracos nomeados:** acrescentados U+0085, U+2028, U+3000 e U+FEFF; U+200B permanece; acrescentado o caso `veredito=''`. Isso amplia a matriz, sem torná-la exaustiva. Resultado SQL da volta 2: **[não verificado]**. (`app/chamados/tests/test_e3_forma_sql_do_juiz.py:28`, `app/chamados/tests/test_e3_forma_sql_do_juiz.py:94`, `app/chamados/tests/test_e3_forma_sql_do_juiz.py:188`) |

**Continência, degrau a degrau:** para a leitura atual e atas com `pendente` booleano canônico, não encontrei outra exclusão indevida; o ponto que impede estender essa conclusão a todo o modelo é o bloqueante 1. (`app/chamados/juizes.py:1389`; `app/chamados/models.py:3398`; `app/ponto/services/lastro.py:150`)

| Degrau | Relação Python → SQL |
|---|---|
| **0: validada** | Ambos excluem. (`app/chamados/juizes.py:1391`; `app/chamados/models.py:3411`) |
| **0a: tranca por leitura** | O Python pode excluir mais; a ausência desse teste no SQL alarga os candidatos. (`app/chamados/juizes.py:1395`; `app/chamados/models.py:3379`) |
| **0b: via que vence resposta** | Com materialização, ambos excluem; sem materialização, essa via não basta para excluir. (`app/chamados/juizes.py:1402`; `app/chamados/models.py:3400`) |
| **1: resposta** | Carimbo **com texto após strip** ganha o ramo SQL. Sem carimbo, ou com texto vazio/branco, não ganha esse ramo. Admin/luz podem matar posteriormente no Python, produzindo candidatos excedentes no SQL. (`app/chamados/juizes.py:1407`, `app/chamados/juizes.py:1415`, `app/chamados/juizes.py:1419`; `app/chamados/juizes.py:1546`) |
| **2: identidade** | Com materialização, exclui em ambos, salvo resposta que já venceu pelo degrau anterior; sem materialização, não exclui por identidade. (`app/chamados/juizes.py:1428`; `app/chamados/models.py:3398`, `app/chamados/models.py:3413`) |
| **Fato do marco** | O Python pode matar uma pergunta materializada; o SQL conserva candidatos excedentes. (`app/chamados/juizes.py:1434`; `app/chamados/models.py:3413`) |
| **3: célula** | `''`, NULL e ausência de célula não absolvem; acusatório entra; não acusatório com `pendente=True` entra. Não acusatório definitivo mata no Python antes dos degraus inferiores. A pendência truthy não booleana quebra essa correspondência. (`app/chamados/models.py:3403`, `app/chamados/models.py:3408`, `app/chamados/models.py:3410`; `app/chamados/juizes.py:1438`, `app/chamados/juizes.py:1452`) |
| **4–6: lei, ausência de carimbo, lastro** | Nas atas canônicas, quem chega vivo a esses degraus já está coberto por `~_celula_absolve`, com ou sem materialização. O degrau 6 efetivamente pode ler resposta. (`app/chamados/juizes.py:1454`, `app/chamados/juizes.py:1466`, `app/chamados/juizes.py:1473`; `app/chamados/models.py:3413`) |

`q_vivas()` não recebe horizonte; `pergunta_viva(agora=...)` pode desconsiderar carimbos posteriores ao instante consultado. A continência histórica exige uma qualificação adicional e não fundamenta aqui outro bloqueante da cura. (`app/chamados/models.py:3368`; `app/chamados/juizes.py:1377`, `app/chamados/juizes.py:1382`, `app/chamados/juizes.py:875`)

**Quem lê a lista mais larga de células `veredito=''`:**

| Leitor direto | Mudança possível |
|---|---|
| **Fila de validar** | Aplica também a face “com texto” e refina pelo C6. Para `''`, as perguntas com texto já entravam pelo ramo da resposta; a abertura adicional sem texto não basta para entrar nessa fila. (`app/chamados/juizes.py:1700`, `app/chamados/juizes.py:1703`; `app/chamados/models.py:3412`) |
| **Fio administrativo** | Amplia candidatos de disputas fechadas e depois consulta C6. Disputas abertas já são carregadas sem esse filtro. (`app/chamados/services/fio.py:251`, `app/chamados/services/fio.py:254`, `app/chamados/services/fio.py:268`) |
| **Perguntas do app** | Amplia candidatos, depois consulta C6. A partição pendentes/histórico ainda depende do carimbo cru e de a disputa estar aberta. (`app/chamados/services/pergunta_no_app.py:50`, `app/chamados/services/pergunta_no_app.py:58`, `app/chamados/services/pergunta_no_app.py:71`) |
| **Cobrança** | Amplia candidatos de disputas abertas; exige espera pelo texto e C6 antes da contagem. (`app/chamados/services/cobranca.py:237`, `app/chamados/services/cobranca.py:241`) |
| **Vigia de disputa fechada** | Amplia candidatos; só conta os confirmados pelo C6. (`app/chamados/services/vigia_fio.py:757`, `app/chamados/services/vigia_fio.py:764`) |
| **Janela do colaborador** | **Sem C6 posterior:** destinos adicionais ampliam diretamente a janela dos chamados visíveis. (`app/chamados/services/janela_colab.py:63`, `app/chamados/services/janela_colab.py:69`) |
| **Raio-X/crédito** | **Sem C6 posterior:** exclui materializada com `VL_SIM`, depois particiona por carimbo/texto; pode ampliar créditos a responder/validar. (`app/chamados/services/raiox.py:74`, `app/chamados/services/raiox.py:135`, `app/chamados/services/raiox.py:143`) |
| **Fechamento administrativo** | **Escreve sem C6 posterior:** entrega os resultados a `fechar_por_admin` e, havendo fechamento, registra `SILENCIO`. (`app/ponto/services/veredito_celula.py:181`, `app/ponto/services/veredito_celula.py:190`, `app/ponto/services/veredito_celula.py:195`) |

A ampliação chega, portanto, à escrita de via, materialização, autor e decisão. `fechar_por_admin` usa a porta de identidade, cuja guarda é de idempotência e que não pergunta ao C6. Não é escritor novo fora da porta; também não é mudança exclusivamente visual. Fechamento indevido efetivo ou quantidade afetada em produção: **[não verificado]**. (`app/chamados/models.py:3081`, `app/chamados/models.py:3090`, `app/chamados/models.py:3194`, `app/chamados/models.py:3212`, `app/chamados/models.py:3216`; `app/ponto/services/veredito_celula.py:196`)

Esse caminho tem três chamadores de produção: veredito da célula, declaração de dia e validação de folga. (`app/ponto/services/veredito_celula.py:347`; `app/ponto/services/declarar_dia.py:206`; `app/chamados/services/validacao.py:298`)

**Os 27 sítios mantêm a classe?** Sim: reconstruindo em memória os trechos anteriores do delta e comparando os registros completos, não houve troca de classe nos fontes atuais. Isso é mais forte que o teste permanente, que exige apenas cardinalidade 27 e alguns sítios nomeados. Os nove métodos das duas classes da catraca passaram nessa execução isolada, sem banco. (`app/chamados/tests/test_e3_catraca_respondedores.py:303`, `app/chamados/tests/test_e3_catraca_respondedores.py:333`, `app/chamados/tests/test_e3_catraca_respondedores.py:350`, `app/chamados/tests/test_e3_catraca_respondedores.py:448`)

Os três retirados continuam usos legítimos de instante: resposta posterior à cobrança, máximo instante e idade da resposta. Entretanto, estabilidade do conjunto não prova acerto semântico universal: entre os classificados RESPONDEDOR permanecem usos de horizonte, preservação de legado, correspondência ato/instante e guarda da cura. Migrá-los mecanicamente para “com texto” mudaria essas perguntas. (`app/chamados/services/verbos.py:342`; `app/chamados/veredito_lavrado.py:207`; `app/chamados/services/validacao.py:932`; `app/chamados/juizes.py:348`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:56`; `app/chamados/models.py:4017`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:69`)

**Mordida dos testes:** abaixo, “passa” significa passar pelo método isolado; outro teste pode detectar o mesmo mutante. Execução da bateria SQL da volta 2: **[não verificado]**. As propriedades SQL usam o mesmo universo em `filter`, `exclude` e negação, mas esse universo é uma matriz fixa de 28 linhas. (`app/chamados/tests/test_e3_forma_sql_do_juiz.py:28`, `app/chamados/tests/test_e3_forma_sql_do_juiz.py:52`, `app/chamados/tests/test_e3_forma_sql_do_juiz.py:65`)

| Teste novo da **volta 2** | Pega | Deixa passar | [nao verificado]
|---|---|---|
| `test_amostra_cobre_os_brancos_e_os_invisiveis` | Retirar U+0085 de `_BRANCOS`. | Trocar a face Q pela forma sem trim: esse método não consulta a face. (`app/chamados/tests/test_e3_forma_sql_do_juiz.py:94`) |
| `test_RED_celula_de_veredito_VAZIO_nao_absolve_no_sql` | Remover `~Q(celula__veredito='')`. | Restringir a cura às perguntas sem materialização; todas as fixtures desse método têm materialização ausente. (`app/chamados/tests/test_e3_forma_sql_do_juiz.py:184`, `app/chamados/tests/test_e3_forma_sql_do_juiz.py:188`) |
| `test_MORDE_veredito_vazio_com_texto_e_veredito_que_absolve` | Fazer o SQL incluir também `concorde` sem resposta. | Restaurar o bug de `''`: o caso vazio desse método tem texto e entra pelo degrau 1. (`app/chamados/tests/test_e3_forma_sql_do_juiz.py:198`) |
| `test_MORDE_classificador_VALOR_exige...` | Restaurar guarda sem identidade do objeto, escopo do statement ou ramo errado do `IfExp`. São mutantes do **classificador**, que mora no selo. | As fugas por ramo morto/subconsulta e a serialização com `not` do bloqueante 2. (`app/chamados/tests/test_e3_catraca_respondedores.py:448`, `app/chamados/tests/test_e3_catraca_respondedores.py:236`) |

Para os demais testes introduzidos ou ampliados pela obra: [nao verificado]

| Teste | Pega / deixa passar | [nao verificado]
|---|---|
| `test_universo_tem_os_dois_lados` | Pega juiz constante `False`; deixa passar erro apenas na face Q. (`app/chamados/tests/test_e3_forma_sql_do_juiz.py:79`) |
| `test_face_sem_texto_igual_ao_juiz` | Pega retirar o requisito de carimbo da face; deixa passar retirar U+1680 de `_BRANCOS`, ausente da amostra. (`app/chamados/tests/test_e3_forma_sql_do_juiz.py:28`, `app/chamados/tests/test_e3_forma_sql_do_juiz.py:85`) |
| `test_face_com_texto_igual_ao_juiz` | Pega voltar ao vazio `''` sem trim; deixa passar a mesma omissão de U+1680. (`app/chamados/tests/test_e3_forma_sql_do_juiz.py:89`) |
| `test_none_no_python_e_vazio` | Pega retirar o tratamento de `None` no Python; deixa passar retirar o ramo SQL NULL. (`app/chamados/tests/test_e3_forma_sql_do_juiz.py:114`; `app/chamados/juizes.py:1538`) |
| `test_MORDE_mutante_sem_trim` | Pega mudar o juiz para não remover brancos; deixa passar uma face Q de produção errada, pois compara um Q mutante local. (`app/chamados/tests/test_e3_forma_sql_do_juiz.py:118`) |
| `test_MORDE_mutante_com_o_barra_s_do_postgres` | Pega juiz que deixe de considerar NBSP vazio; deixa passar substituir a face de produção por `\s`, pois também usa Q mutante local. (`app/chamados/tests/test_e3_forma_sql_do_juiz.py:127`) |
| `test_RED_so_brancos...` | Pega restaurar resposta viva sem trim em `q_vivas`; deixa passar retirar a cura de `veredito=''`. (`app/chamados/tests/test_e3_forma_sql_do_juiz.py:159`) |
| `test_MORDE_texto_com_brancos_em_volta...` | Pega fazer a face rejeitar texto cercado de brancos; deixa passar retirar a cura de `''`. (`app/chamados/tests/test_e3_forma_sql_do_juiz.py:166`) |
| `test_continencia_so_brancos...` | Pega remover o ramo SQL que inclui acusatório/sem célula; deixa passar o contraexemplo de pendência não booleana. (`app/chamados/tests/test_e3_forma_sql_do_juiz.py:171`) |
| `test_RED_a_validar_nao_traz_so_brancos` | Pega retirar a face “com texto” da fila; deixa passar erro de `q_vivas` restrito a veredito vazio. (`app/chamados/tests/test_e3_forma_sql_do_juiz.py:206`) |
| `test_universo_nao_e_vazio` da catraca | Pega perder a classe VALOR dos três sítios exigidos; deixa passar trocar outros sítios mantendo classes e cardinalidade. (`app/chamados/tests/test_e3_catraca_respondedores.py:346`) |
| `test_MORDE_guarda_de_VALOR...` | Pega retirar reconhecimento de comparação, aritmética ou agregado; deixa passar agregado alheio dentro de subconsulta. (`app/chamados/tests/test_e3_catraca_respondedores.py:416`, `app/chamados/tests/test_e3_catraca_respondedores.py:236`) |

Os testes de mutantes locais demonstram capacidade do instrumento; não substituem, isoladamente, mutação da face de produção. As comparações de equivalência são as que ligam o Q real ao juiz. (`app/chamados/tests/test_e3_forma_sql_do_juiz.py:85`, `app/chamados/tests/test_e3_forma_sql_do_juiz.py:89`, `app/chamados/tests/test_e3_forma_sql_do_juiz.py:121`)

**Classes anteriores e campos com dois significados:**

- **Segundo juiz / derivação paralela:** as faces de texto leem a classe de brancos do Python; não encontrei nova enumeração independente de brancos. Continua divergente o espelho da provisoriedade apontado no bloqueante 1. (`app/chamados/juizes.py:1526`, `app/chamados/juizes.py:1538`, `app/chamados/juizes.py:1546`; `app/chamados/models.py:3403`; `app/ponto/services/lastro.py:150`)
- **Segundo escritor fora da porta:** a ampliação administrativa usa a porta existente. Os escritores do par continuam sendo nascimento/save, carimbo, descarimbo em lote, resposta humana, automática e recuperação pela trilha; a obra não acrescenta escrita nos corpos das faces Q. (`app/chamados/models.py:2865`, `app/chamados/models.py:3156`, `app/chamados/models.py:3286`, `app/chamados/models.py:3472`; `app/chamados/services/disputa_emissao.py:1467`, `app/chamados/services/disputa_emissao.py:1659`, `app/chamados/services/disputa_emissao.py:1681`)
- **Relógio fora da borda:** as faces novas não consultam relógio. O fechamento administrativo já obtém o instante na porta e o reutiliza no ato `SILENCIO`. (`app/chamados/juizes.py:1538`, `app/chamados/juizes.py:1546`; `app/chamados/models.py:3090`; `app/ponto/services/veredito_celula.py:196`)
- **Quem escreve/lê `veredito=''`:** o campo permite vazio; o C6 e agora `q_vivas` o tratam como ausência de julgamento; o reconciliador também o exclui da absolvição. A porta da lavra valida vocabulários, embora possa preservar um valor anterior quando a via já é administrativa; a fixture força `''` por `update`. Origem de eventual vazio em produção: **[não verificado]**. (`app/escala/models.py:1770`; `app/chamados/juizes.py:1438`; `app/chamados/models.py:3408`; `app/chamados/reconciliador.py:640`; `app/ponto/portas/celula.py:921`, `app/ponto/portas/celula.py:927`, `app/ponto/portas/celula.py:1035`; `app/chamados/tests/test_e3_forma_sql_do_juiz.py:181`)
- **Quem escreve/lê `respondida_em`:** os escritores exigem texto nas transições novas; os leitores ainda misturam presença do carimbo, instante e resposta válida. `respondida`, rótulos de estado e partição do app usam carimbo cru; `espera_o_texto_do_colaborador` e o juiz leem o par. O passivo inalterado continua salvável por outros campos, inclusive no fechamento administrativo. (`app/chamados/services/disputa_emissao.py:1432`, `app/chamados/services/disputa_emissao.py:1657`, `app/chamados/services/disputa_emissao.py:1677`; `app/chamados/models.py:3584`, `app/chamados/models.py:3071`, `app/chamados/models.py:3503`; `app/chamados/services/pergunta_no_app.py:71`; `app/chamados/services/fio.py:187`)

Não uso os nove `isnull` nem a cura do passivo como bloqueantes desta volta. As duas falhas acima independem dessa migração. Entretanto, a inclusão de `veredito=''` já alcança o app e a escrita administrativa pelos caminhos existentes; a medida dos efeitos em produção, inclusive eventual mudança no app ou no supra-juiz, permanece **[não verificado]**. (`app/chamados/models.py:3408`; `app/chamados/services/pergunt

[truncado em 20000 de 20061 caracteres]

_4459 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
