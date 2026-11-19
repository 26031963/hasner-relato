# CODEX-ESTADO

> Ultima resposta processada: **19/11/2026 17:15** · publicado em **19/11/2026 17:16** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 19/11 17:15 · sessao eaf908cd6bf6 · E4C-F5-CONSTROI · sonda · origem=Code

- **Hoje.** `PerguntaDisputa.respondida` chama `pergunta_respondida(DecisaoPergunta.leitura(self))`. A leitura usa o instante e o texto da resposta vigente na sequência, incluindo o legado da projeção quando falta a linha correspondente. `app/chamados/models.py:3785–3791`, `app/chamados/models.py:4295–4326`
- **Novo.** A composição descrita no item 2 do 1510 preserva `leitura.respondida_em` e troca apenas `leitura.resposta_colab` por `p.resposta_colab` antes de chamar o mesmo juiz. O `RELATO` registra essa composição no fio. Na cópia disponível, `fio.py:189–192` contém o início de `montar_chamados_fio`. A implementação da sonda e a confirmação do hash pelo Git ficam [nao verificado]. `app/docs/RELATO.md:11`, `app/chamados/juizes.py:1544–1552`, `app/chamados/services/fio.py:189–192`

**Medida válida pela saída fornecida.** `ERROS=0` e `UNIVERSO=51563` é igual a `PERCORRIDAS=51563`. `DADO RASPADO:4`

Transcrição literal de `DADO RASPADO:3–9`:

```text
BANCO=sombra
UNIVERSO=51563 PERCORRIDAS=51563 ERROS=0
MUDA_TRUE_PARA_FALSE=0
MUDA_FALSE_PARA_TRUE=0
PKS_TRUE_PARA_FALSE=[]
PKS_FALSE_PARA_TRUE=[]
VEREDITO=VALIDO MUDAM=0
```

MUDAM=0

## 19/11 17:13 · sessao 8411d995dfcb · E4C-F5-CONSTROI · verificador · origem=Code

# AVAL-DEVOLVIDO — E4C-F5-CONSTROI-A-1911-1710

Revisei os arquivos da cópia em modo somente leitura. O caminho da cópia leva o hash informado e o wrapper usa esse formato para cópias de `git archive`. A identidade Git, a ref `e4c-f5-volta2` e o rebase sobre o main do dia estão [nao verificado]. Os números de execução abaixo são os do anexo raspado. `bin/codex_pergunta.sh:304–320`.

Em cada item, **1** é a premissa na cópia, **2** é o confronto com os avais, **3** é a condição literal e **4** é o alcance sobre exportada, dinheiro e `!`. `e4c_f5_constroi_a_1911_1710.txt:2`.

### FORMA — PROCEDE

1. A sequência A → B → C corresponde à seção 4 do predesenho. A cópia contém código e testes da volta anterior. O estado Git declarado está [nao verificado]. `e4c_f5_constroi_a_1911_1710.txt:3`, `chamados/tests/test_e4c_f5_volta2_juizes.py:1–3`.
2. Uma obra, uma cópia e um pouso integral respeitam 1435 e 1510. A certificação deve valer para a árvore que pousa. `e4c_f5_segue_as_tres_voltas_1911_1435.txt:5–6`, `respostas_f5_e_c7_1911_1510.txt:8`, `docs/PISTA.md:29–38`.
3. A suite anterior como condição para abrir a próxima volta fecha. A antecipação de K41 e K42 é um problema do despacho e da divisão A/B. `e4c_f5_constroi_a_1911_1710.txt:3–6`. [nao verificado]
4. A forma autoriza encadeamento e integração de código. As operações que exigem `!` continuam sujeitas à regra própria. `e4c_f5_constroi_a_1911_1710.txt:3`, `CLAUDE.md:265–268`.

### DESENHO — DEVOLVE

1. `projecao_temporal_da_pergunta`, usada pelo C6, testa `isinstance(p, PerguntaDisputa)`, importa `DecisaoPergunta` no ramo ORM, lê os instantes do ato e usa texto, materialização e via da instância. Ainda consulta `PerguntaDisputa.VIAS_FORA_DO_FATO` no corte temporal. `pergunta_respondida` faz o mesmo teste e import, lê o instante do ato no ORM e exige texto não vazio após `strip`. Nos objetos já compostos, ambos leem os atributos recebidos. `chamados/juizes.py:12–15`, `:1382–1411`, `:1533–1535`, `:1551–1564`.
2. O par aprovado por 1510 item 2 e 1620 é **instante e validação do ato, texto da projeção**. Hoje o fio monta esse par com `_replace`, a property entrega `LeituraDoAto` com o texto do ato, e `particao_do_app` passa ORM ao juiz, que faz a composição de forma implícita. `respostas_f5_e_c7_1911_1510.txt:2–3`, `e4c_f5_desenho_a_e_matriz_1911_1620.txt:2`, `chamados/services/fio.py:187–192`, `chamados/models.py:3791`, `:4319`, `chamados/services/pergunta_no_app.py:83–86`.
3. A conta **0/1** é de SELECTs da relação `decisoes`, com os escalares necessários já carregados. Sem PK, a leitura usa o passivo em memória. K28 usa `only()` e deixa `validada_em` e `validada_por_id` diferidos. O aval também precisa dizer como a fonte única alcança `sem_veredito_entre` e `estado`, que passam texto do ato ao juiz, e o placar, que passa instante da coluna por `values_list`. A pureza do juiz, sozinha, deixa essas entradas como estão. `chamados/models.py:4251–4268`, `:4295–4326`, `:2004–2006`, `:3235–3244`, `chamados/management/commands/backfill_decisoes_pergunta.py:531–533`, `chamados/services/raiox.py:135–147`.
4. Unificar a entrada de `sem_veredito_entre` alcança a guarda de fechamento, o CSF e a Pauta do DP. O efeito na frota dessas rotas está [nao verificado]. `chamados/models.py:2004–2015`, `ponto/supra_juiz.py:561–588`, `chamados/services/competencia_trancada.py:123–139`.

### DESPACHO — DEVOLVE

1. Entre os 69 K, há **três** em `chamados/models.py`: K15 em `:357`, K41 em `:1542` e K42 em `:2057`. `:3791` é um quarto sítio nominal, a property, fora da lista K. O wrapper proíbe o arquivo inteiro e inventaria o diff contra o hash recebido. `chamados/models.py:357`, `:1542`, `:2057`, `:3785–3791`, `bin/codex_pergunta.sh:121–129`, `:154–186`, `:432–442`.
2. Opus pode construir a parte nominal e o Codex pode partir de seu commit na mesma worktree. O hash de entrada do wrapper precisa ser o do commit do Opus, com `HEAD` limpo. O `rc 75` tem o desvio declarado para Opus. O `rc 3` é a recusa do inventário. `e4c_f5_constroi_a_1911_1710.txt:5`, `:15`, `CLAUDE.md:353–362`, `bin/codex_pergunta.sh:254–258`, `:432–459`.
3. O despacho põe K41 e K42 antes de A, embora sejam de B. Também deixa sem responsável as alterações em `models.py:2004` e `:3235` se a fonte única abranger os leitores do desenho. Declare o commit preparatório, o RED que o antecede e quem toca cada sítio do arquivo proibido. `e4c_f5_constroi_a_1911_1710.txt:3–8`, `chamados/models.py:1542`, `:2004–2006`, `:2057`, `:3235–3244`.
4. O despacho nominal cobre os sítios indicados em `models.py`. Motor v2, porta de célula e registro de juízes têm proibições próprias no wrapper. `e4c_f5_constroi_a_1911_1710.txt:5`, `bin/codex_pergunta.sh:121–129`.

### VOLTA A — DEVOLVE

1. A lista bate com o predesenho. São **3 L** (1, 2, 15) e **7 Q** (9, 10, 16, 17, 18, 24, 43). A fixture dos dois REDs 8 é da própria **E4C-F5, volta 2**. Ela grava texto sem instante antes de registrar o ato e cai na guarda de `save` antes das medições. `e4c_f5_constroi_a_1911_1710.txt:6`, `chamados/services/tarefa_do_dia.py:530`, `chamados/juizes.py:1668`, `chamados/models.py:357`, `chamados/tests/test_e4c_f5_volta2_juizes.py:1–3`, `:118–125`, `:143–149`, `:167–180`, `chamados/models.py:3680`, `:3712`, `chamados/services/fio.py:1718–1721`.
2. O 1510 exige RED em cada um dos três sítios, com divergência entre ato e projeção. A também passa a dividir `chamados/reconciliador.py` com C7, em funções distintas. A exceção nominal do 1335 item 2 é para **C7 × F0**. A ordem ou exceção correspondente para **C7 × F5** ficou sem registro no novo aval. `respostas_f5_e_c7_1911_1510.txt:3`, `e4c_f5_segue_as_tres_voltas_1911_1435.txt:8`, `respostas_aos_devolvidos_1911_1335.txt:3`, `chamados/reconciliador.py:528`, `:698`, `:1095`, `:1548`.
3. Reescrever só a fixture dos REDs 8 deixa testes que chamam os juízes com ORM cru e exigem a leitura do ato. A precisa distinguir a adaptação dos chamadores, necessária quando os juízes se tornam puros, da carga em lote que B e C completam. `chamados/tests/test_e4c_f5_volta2_juizes.py:37–57`, `:77–104`, `chamados/juizes.py:1389–1396`, `:1559–1564`, `e4c_f5_constroi_a_1911_1710.txt:3–6`.
4. K24 é o cruzamento declarado com a CADEIA-20 em `cartorio.py`. A carga alimenta uma guarda de resposta que pode impedir uma transição de chamado. O alcance autorizado ali é a alimentação em lote. O diff integral da CADEIA-20 está [nao verificado]. `e4c_f5_constroi_a_1911_1710.txt:7`, `:11`, `ponto/services/cartorio.py:146–147`, `:1684–1713`.

### VOLTA B — DEVOLVE

1. A conta do predesenho fecha: **(10 L + 29 Q) − (3 L + 7 Q) = 29**, com **7 L e 22 Q** em B. Os IDs são **4, 11, 23, 25–27, 29–32, 41–42, 44–50, 53–55, 61–65, 67–68**. `perguntas_vivas_de` cobre K4 e `fio_vivo` reúne seis K de L. `e4c_f5_constroi_a_1911_1710.txt:6`, `chamados/juizes.py:1512–1526`, `:2493–2494`, `chamados/models.py:1542`, `:2057`.
2. Carga em lote preservando rota e partição cabe no 1620. `e4c_f5_desenho_a_e_matriz_1911_1620.txt:2–3`, `chamados/juizes.py:1512–1526`.
3. **29** é a cobertura de B. **27** é o número de K ainda por editar em B se K41 e K42 forem concluídos no commit anterior a A. O aval precisa fixar qual dessas duas etapas termina cada K. `e4c_f5_constroi_a_1911_1710.txt:3`, `:5–6`, `chamados/models.py:1542`, `:2057`.
4. K23 lê a prévia de folha. K45 e K46 leem competência trancada, inclusive o caminho que monta a Pauta. A autorização declarada é para a carga de decisões. Qualquer mudança de resultado nesses caminhos está [nao verificado]. `folha/previa.py:28–39`, `:64–67`, `:139–146`, `chamados/services/competencia_trancada.py:119–144`, `:165–167`, `CLAUDE.md:401–417`.

### VOLTA C — PROCEDE

1. São **25 M**: **5–8, 12–14, 21, 28, 33–40, 51–52, 56–57, 59–60, 66, 69**. A sobra é a lista independente de `dedup_perguntas_fato.py:75`. Os cinco R são **3, 19, 20, 22 e 58**, respectivamente releitura O261, releitura da materialização, validação por ID, busca da irmã e devolução L-130 por ID. `e4c_f5_constroi_a_1911_1710.txt:6`, `chamados/management/commands/dedup_perguntas_fato.py:74–76`, `chamados/services/fio.py:2078`, `chamados/services/relavra_via.py:230–242`, `chamados/services/materializacao.py:284`, `:2117`, `chamados/services/validacao.py:431`, `:677`.
2. A sobra foi autorizada nominalmente em C. A divergência fica no limite escrito no item EXCEÇÃO. `e4c_f5_constroi_a_1911_1710.txt:6–7`.
3. A cobertura soma **10 + 29 + 25 = 64 K em lote**, mais **5 R = 69 K**. A sobra fica fora dos 69. A classe R preserva a leitura por item, inclusive no O261, cujo selo de custo continua obrigatório. `e4c_f5_constroi_a_1911_1710.txt:6`, `:9`, `:14`, `chamados/management/commands/dedup_perguntas_fato.py:75`, `chamados/tests/test_o261_julgamento_desfaz_carimbo.py:203–205`.
4. K14 percorre exportadas para diagnóstico com `executar=False`. O dedup tem um ramo `--apply` separado. A carga em lote é o escopo autorizado para os dois. `chamados/management/commands/backfill_fio_morto_e4b.py:153–176`, `chamados/management/commands/dedup_perguntas_fato.py:74–103`, `e4c_f5_constroi_a_1911_1710.txt:6–7`, `:13`.

### EXCEÇÃO 1435 — DEVOLVE

1. Os **59 pontos** do predesenho são **5 comuns para 10 L + 29 Q + 25 M**. Os cinco R contribuem zero pontos. `chamados/services/tarefa_do_dia.py:530`, `chamados/juizes.py:1514`, `:1517–1526`, `:1668`, `chamados/models.py:357`, `e4c_f5_constroi_a_1911_1710.txt:6–7`.
2. O 1435 veda mudanças de rota ou partição fora das letras. O 1620 limita a exceção à carga de decisões. A cláusula do 1710 mantém o mesmo limite material. `e4c_f5_segue_as_tres_voltas_1911_1435.txt:16`, `e4c_f5_desenho_a_e_matriz_1911_1620.txt:3`, `e4c_f5_constroi_a_1911_1710.txt:7`.
3. O literal **“só para os 59”** deixa de fora a sobra que C e PRONTO mandam carregar. Escreva **59 + a sobra nominal**. São **60 pontos de carga** contando a lista do dedup. `e4c_f5_constroi_a_1911_1710.txt:6–7`, `:14`, `chamados/management/commands/dedup_perguntas_fato.py:74–76`.
4. A exceção é para leitura em lote. As cercas de exportada, a rota e a partição continuam valendo. `e4c_f5_constroi_a_1911_1710.txt:7`, `:13`, `CLAUDE.md:265–268`, `:411–417`.

### RED — DEVOLVE

1. Os testes existem e medem hoje: [nao verificado]
   - **Tarefa** — igualdade do total ao construir e usar a carga, **1 × 3 perguntas**, variando também dias e colaboradores. O anexo relata **30 → 36**. `chamados/tests/test_e2_tarefa_do_dia.py:913–938`.
   - **O261** — a porta real com **1 × 3 PKs**, `Q(3) = 3 × Q(1)` e `Q(1) ≤ 8`. O anexo relata **9 > 8**. `chamados/tests/test_o261_julgamento_desfaz_carimbo.py:187–205`.
   - **C11** — igualdade do total com **1 × 5 dias**, cache do catálogo frio e cardinalidade conferida. O anexo relata **49 → 61**. `escala/tests/test_f3_beco1_folga_na_lista.py:378–402`.
   - **Batida real** — tetos **230/294** para 1/3 chamados que fecham e **145/219** para 1/3 que ficam. `_cobrar` também sela `on_commit`. O anexo relata **237/301** e **147/222**, respectivamente. `ponto/tests/test_selo_chokepoint_escrita.py:196`, `:210`, `:228`, `:235`, `:251–254`, `:287–298`, `:428–478`.
2. A igualdade 3 × 6 para os carregadores em lote atende ao 1620. O caso que morde cada um dos três leitores continua sendo exigência própria do 1510. `e4c_f5_desenho_a_e_matriz_1911_1620.txt:6`, `respostas_f5_e_c7_1911_1510.txt:3`.
3. **Igualdade bruta 3 × 6 no O261 contradiz a classe R.** A porta abre transação, trava e relê por candidata. O total deve ser comparado por candidata, `Q(3)/3 = Q(6)/6 ≤ 8`. A igualdade absoluta cabe na contagem de consultas adicionais à relação de decisões, se a implementação a separar. Dos testes nomeados, tarefa e C11 exercitam K1/K2 de A e O261 exercita K3 de R. Faltam os carregadores e os mutantes designados para B e C. `chamados/services/relavra_via.py:215–242`, `chamados/tests/test_o261_julgamento_desfaz_carimbo.py:200–205`, `chamados/services/tarefa_do_dia.py:530`, `escala/services/cadastro_realidade.py:460–466`, `e4c_f5_constroi_a_1911_1710.txt:6–9`.
4. Os testes da batida e do O261 exercitam portas escritoras com fixtures. As linhas citadas sustentam a medição de SQL, e a equivalência de exportada ou dinheiro está [nao verificado]. `ponto/tests/test_selo_chokepoint_escrita.py:416–433`, `:456–468`, `chamados/tests/test_o261_julgamento_desfaz_carimbo.py:187–205`.

### SELOS DE TETO — PROCEDE

1. `_ancoradas()` tem selo de **exatamente uma consulta**, com resultado não vazio e verificação da subquery. O261 tem o limite de **8 por candidata**. `chamados/tests/test_e4c_f2_leitura_do_ato.py:534–551`, `chamados/tests/test_o261_julgamento_desfaz_carimbo.py:203–205`.
2. O 1620 deixou os tetos intactos naquela etapa. O 1710 delimita a mudança ao custo constante por lote, com motivo e tabela, e conserva os quatro valores da batida e o teto O261. `e4c_f5_desenho_a_e_matriz_1911_1620.txt:3`, `:7`, `e4c_f5_constroi_a_1911_1710.txt:8–9`, `:14`.
3. O limite fecha como gate. Com **9 > 8** no dado raspado, falta recuperar uma consulta unitária ou registrar a pergunta de desenho. A viabilidade e os valores após a construção estão [nao verificado]. `chamados/tests/test_o261_julgamento_desfaz_carimbo.py:205`, `chamados/services/relavra_via.py:230–242`, `e4c_f5_constroi_a_1911_1710.txt:9`.
4. Medir e registrar SQL por lote cabe no escopo. Os selos atravessam transições reais nos testes, e seus resultados futuros estão [nao verificado]. `ponto/tests/test_selo_chokepoint_escrita.py:428–433`, `:464–468`.

### MEDIR — PROCEDE

1. A property atual julga o texto do ato. A fixture com resposta só no ato e projeção vazia demonstra a troca de `True` para `False` ao usar o texto da projeção com o mesmo instante. `chamados/models.py:3785–3791`, `chamados/juizes.py:1533–1535`, `:1564`, `chamados/tests/test_e4c_f2_leitura_do_ato.py:331–348`, `:490–503`.
2. A medida aplica a fonte do 1510 item 2. O critério de pouso do 1510 item 5 trata de C7, outro objeto. `respostas_f5_e_c7_1911_1510.txt:3`, `:6`, `e4c_f5_constroi_a_1911_1710.txt:10`. [nao verificado]
3. Conte mudanças no **booleano** da property. O contador existente mede desigualdade **literal** de texto, incluindo espaço e caixa. O total e os PKs da medida pedida estão [nao verificado]. `chamados/services/vigia_texto_resposta.py:8–18`, `chamados/tests/test_e4c_f5_contador_texto.py:40–53`, `e4c_f5_constroi_a_1911_1710.txt:10`.
4. A sonda declarada lê na sombra e publica só PKs. A property também entra numa porta escritora de declaração de horário, o que torna relevante o gate de pouso para divergência. `bin/sonda_leitura.sh:4–5`, `chamados/services/declaracao_texto.py:418–420`, `chamados/services/disputa_emissao.py:1439–1443`, `e4c_f5_constroi_a_1911_1710.txt:10`.

### SUITES — PROCEDE

1. O código distingue `--volta`, que seleciona obra, vizinhos e selos globais, de suite inteira com motivo `pre-opus` ou `pouso`. `bin/volta_intermediaria.sh:3–9`, `bin/suite.sh:216–223`, `:236–258`.
2. Pelo 1335 item 3, **A/B/C rodam como voltas intermediárias na pista 2**. **Pre-opus da espinha e certificação de pouso ficam na VM pela trava**. Isso é compatível com o 1710. `respostas_aos_devolvidos_1911_1335.txt:4`, `:7`, `e4c_f5_constroi_a_1911_1710.txt:11`, `bin/suite.sh:308–312`.
3. O rebase deve anteceder a certificação da árvore que pousa. O estado do passo 1, o resultado do rebase e as suites verdes estão [nao verificado]. `e4c_f5_constroi_a_1911_1710.txt:3`, `:11`, `docs/PISTA.md:29–38`.
4. O cruzamento declarado é `cartorio.py`. A regra de suite preserva o escopo de cada obra e a parada por conflito na mesma função. `ponto/services/cartorio.py:146–147`, `e4c_f5_constroi_a_1911_1710.txt:3`, `:11`, `:13`.

### VERIFICADOR — PROCEDE

1. A revisão final pelo escopo inteiro coincide com a regra de verificar o diff e os escritores e leitores atingidos. `CLAUDE.md:366–380`.
2. O verificador final complementa as verificações por volta herdadas do 1435 e a linha de evidência pré-suite. `e4c_f5_segue_as_tres_voltas_1911_1435.txt:5`, `:13`, `e4c_f5_constroi_a_1911_1710.txt:12`. [nao verificado]
3. Matriz, predesenho e aval dão o escopo. A linha publicada deve distinguir provas da construção, exigidas antes da suite, dos resultados da própria suite, do Opus e do pouso, que chegam depois. `e4c_f5_segue_as_tres_voltas_1911_1435.txt:13`, `e4c_f5_constroi_a_1911_1710.txt:12–14`, `CLAUDE.md:372–380`. [nao verificado]
4. A verificação cobre os leitores de risco e o cruzamento do cartório. A aprovação do pouso segue o rito da árvore inteira. `ponto/services/cartorio.py:1684–1713`, `CLAUDE.md:377–381`.

### PROIBIDO — DEVOLVE

1. O veto ao manager padrão tem base no selo de `_ancoradas()`. O problema concreto do veto geral aos harnesses é que o mutante de `fio.py` procura literalmente a composição `_replace` que o aval manda retirar. `chamados/models.py:343–359`, `chamados/tests/test_e4c_f2_leitura_do_ato.py:543–551`, `chamados/tests/e4c_f5_volta2_mutantes.py:32–38`, `chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:220–227`.
2. Preservar o texto da projeção, o pouso integral e a ausência de juiz novo respeita o 1510. O 1620 proíbe tocar harness para passar. O 1435 continua exigindo que o mutante com apelido morda. `respostas_f5_e_c7_1911_1510.txt:2–3`, `:8`, `e4c_f5_desenho_a_e_matriz_1911_1620.txt:7`, `e4c_f5_segue_as_tres_voltas_1911_1435.txt:11`, `:16–17`. [nao verificado]
3. Especifique quais adaptações de teste preservam a prova. As 12 famílias da matriz incluem os próprios orçamentos que o novo RED manda mudar. Além disso, o selo anterior da property exige `True` para resposta só no ato, resultado que o novo contrato muda. A exceção da fixture dos REDs 8 só resolve a criação ilegal. `chamados/tests/test_e2_tarefa_do_dia.py:913–938`, `chamados/tests/test_o261_julgamento_desfaz_carimbo.py:187–205`, `escala/tests/test_f3_beco1_folga_na_lista.py:395–402`, `chamados/tests/test_e4c_f2_leitura_do_ato.py:490–503`, `e4c_f5_constroi_a_1911_1710.txt:6`, `:8`, `:13–14`.
4. O escopo declarado autoriza leitura em lote e composição da pergunta. Motor v2, alteração de escala ou vínculo e gravado de competência exportada continuam sujeitos a `!`. Ler vínculo no C11 e ler perguntas de exportadas na auditoria são caminhos já presentes. `CLAUDE.md:265–268`, `escala/services/cadastro_realidade.py:490–503`, `chamados/management/commands/backfill_fio_morto_e4b.py:153–176`.

### PRONTO — DEVOLVE

1. A conta **64 + 5 = 69**, com a sobra separada, fecha. As duas superfícies do smoke existem. O app tem GET autenticado. A abertura do modal da Central faz `UPDATE` nas notas ainda não lidas antes de renderizar, inclusive por GET. `e4c_f5_constroi_a_1911_1710.txt:14`, `api/urls.py:92`, `api/views.py:2536–2554`, `chamados/urls.py:39`, `chamados/views.py:868–880`, `:951–956`, `chamados/services/nota.py:126–133`.
2. O 1435 preserva o PRONTO do 0815 e acrescenta 3a–3e. Faça a lista nova cumulativa. Na cópia, o vigia ainda itera o queryset inteiro após o prefetch, sem `iterator(chunk_size=...)` exigido pelo 1510 item 3. A retirada de `divergencia_texto_resposta_universo` está coberta por teste. A triagem de `casefold` e as provas das medidas herdadas do 0815 estão [nao verificado]. `e4c_f5_segue_as_tres_voltas_1911_1435.txt:5`, `:8–13`, `:16–17`, `respostas_f5_e_c7_1911_1510.txt:4–5`, `chamados/services/vigia_texto_resposta.py:8–18`, `chamados/tests/test_e4c_f5_contador_texto.py:55–70`.
3. O checklist precisa incorporar a métrica unitária do O261, os REDs de B/C, as adaptações de testes autorizadas e o ambiente ou a conta do smoke do modal. O verde das 11 famílias, a medida da property, as suites, Opus, pouso e smoke estão [nao verificado]. `e4c_f5_constroi_a_1911_1710.txt:8–14`, `chamados/tests/test_o261_julgamento_desfaz_carimbo.py:203–205`, `chamados/tests/e4c_f5_volta2_mutantes.py:34–37`, `chamados/views.py:877–880`.
4. A escrita do smoke é no carimbo de leitura de `FollowupChamado`. Dinheiro, escala, vínculo e gravado exportado seguem sujeitos à regra de `!`. O efeito de ponta a ponta da árvore futura nesses dados está [nao veri

[truncado em 20000 de 20123 caracteres]

## 19/11 17:01 · sessao a72a5442f87c · E4C-F5-CONSTROI · leitura · origem=Code

## 1. Classes dos carregadores

Os IDs são os da seção **K** da matriz. Os caminhos abaixo partem de `app/`, exceto `CLAUDE.md` e `bin/`. Conferi os sítios na cópia. A identidade Git da cópia com `e673e0efe` e `refs/heads/e4c-f5-volta2` está [nao verificado].

Em L, contei o ponto comum quando ele pode alimentar o conjunto que o carregador entrega. Quando um prefetch externo já reúne perguntas de vários chamados, o ponto contado é a consulta externa. Essa distinção separa `fio_vivo` das cargas do supra-juiz e da emissão retroativa. `chamados/juizes.py:1517`, `ponto/management/commands/supra_juiz.py:192`, `ponto/management/commands/emitir_furo_retroativo.py:83`.

| ID K | Arquivo:linha | Classe | Função comum | Por quê |
|---:|---|:---:|---|---|
| 1 | `chamados/services/tarefa_do_dia.py:530` | L | `CargaDaTarefa.__init__` | A mesma coleção alimenta partição, slots e C6. `:540`, `:621`, `:625`, `:632`. |
| 2 | `chamados/juizes.py:1668` | L | `alimentar_portas` | Julga e distribui as mesmas instâncias para M1 e Vigia. `chamados/juizes.py:1672`, `escala/services/cadastro_realidade.py:502`, `chamados/services/vigia_fio.py:566`. |
| 3 | `chamados/services/relavra_via.py:236` | R | — | Releitura por PK após a trava de cada candidata O261. `:215`, `:230`, `:242`. |
| 4 | `chamados/juizes.py:1514` | L | `perguntas_vivas_de` | O corpo carrega a coleção usada pelo próprio C6 e pelos leitores de vivas da disputa. `chamados/services/fio.py:712`, `chamados/views_cobrar.py:170`, `:178`. |
| 5 | `chamados/management/commands/auditar_invariantes_chamados.py:1586` | M | — | Universo da contraprova I21a. `:178`, `:1592`. |
| 6 | `chamados/management/commands/auditar_invariantes_chamados.py:590` | M | — | Consulta I2. `:596`, `:597`. |
| 7 | `chamados/management/commands/auditar_invariantes_chamados.py:1642` | M | — | F1 adianta as perguntas dos chamados. `:1645`, `:1667`, `:1671`. |
| 8 | `chamados/services/fio.py:2078` | M | — | Sementes do universo de grupo usado pelo backfill e pelo censo de colisões. `chamados/services/fio.py:2144`, `chamados/management/commands/backfill_decisoes_pergunta.py:253`. |
| 9 | `chamados/reconciliador.py:1548` | Q | — | Consulta do reate, materializada pelo reconciliador. `:1569`, `:1572`. |
| 10 | `chamados/reconciliador.py:1095` | Q | — | Consulta de órfãs abertas usada no julgamento e na varredura. `:1130`, `:1182`, `:1239`. |
| 11 | `chamados/services/becos.py:85` | Q | — | O evento adianta a relação da disputa que entrega a `classificar(d)`. `:25`, `:30`, `:86`, `:87`. |
| 12 | `chamados/management/commands/backfill_fio_morto_e4b.py:89` | M | — | Plano do dia. `:90`. |
| 13 | `chamados/management/commands/backfill_fio_morto_e4b.py:134` | M | — | Releitura dos alvos para execução do dia. `:135`. |
| 14 | `chamados/management/commands/backfill_fio_morto_e4b.py:166` | M | — | Varredura da classe de exportadas. `:153`, `:171`. |
| 15 | `chamados/models.py:357` | L | `ChamadoColaborador._ancoradas` | A lista canônica abastece o gate, a relavra e S143. `chamados/models.py:341`, `:343`, `:380`, `chamados/services/disputa_emissao.py:1983`. |
| 16 | `chamados/services/disputa_emissao.py:1949` | Q | — | Consulta própria das pendentes em S128. `:1998`, `:2009`, `:2023`. |
| 17 | `chamados/services/fio.py:1962` | Q | — | Sementes consultadas pelo universo do fato na abertura e no reuso. `:1952`, `:1977`, `:2000`. |
| 18 | `chamados/services/fio.py:1594` | Q | — | O fecho e a costura consultam vizinhos por chave. `:1847`, `:1934`, `:2088`. |
| 19 | `chamados/services/materializacao.py:2117` | R | — | Releitura por PK após a seleção e a trava de cada item a materializar. `:2104`, `:2111`, `:2125`. |
| 20 | `chamados/services/validacao.py:677` | R | — | A porta valida uma pergunta por ID, também quando invocada pelo lote. `:661`, `:682`, `:1078`. |
| 21 | `chamados/services/materializacao.py:324` | M | — | Universo U163 do contador consumido pelo comando BUG-PAR-103. `:328`, `:342`, `chamados/management/commands/bug_par_103.py:81`. |
| 22 | `chamados/services/materializacao.py:284` | R | — | Busca pontual da irmã complementar da pergunta corrente. `:281`, `:290`, `:341`. |
| 23 | `folha/previa.py:64` | Q | — | Agrupa por pessoa e dia. `_material` lê resposta antes de usar `fio_vivo`. `:67`, `:142`, `:144`. |
| 24 | `ponto/services/cartorio.py:146` | Q | — | Prefetch externo para o dia, usado também na guarda de resposta e no dedup. `:147`, `:1418`, `:1693`, `:1713`. |
| 25 | `chamados/services/declaracao_texto.py:475` | Q | — | Consulta por marco na projeção de tela, quando o mapa do lote está ausente. `:470`, `:477`, `:479`. |
| 26 | `chamados/services/declaracao_texto.py:588` | Q | — | Consulta conjunta de vários fatos, antes da separação em listas por fato. `:584`, `:591`. |
| 27 | `chamados/services/declaracao_texto.py:635` | Q | — | Consulta própria das propostas, com memo por fato. `:631`, `:634`. |
| 28 | `chamados/management/commands/backfill_decisoes_pergunta.py:531` | M | — | Consulta do DRY, seguida da canônica e do plano. `:533`, `:537`. |
| 29 | `chamados/juizes.py:1768` | Q | — | Consulta da seleção de corrigíveis, antes de produzir os IDs. `:1509`, `:1770`. |
| 30 | `chamados/juizes.py:1775` | Q | — | Consulta própria da fila de respondidas a validar. `:1773`, `:1778`. |
| 31 | `chamados/juizes.py:2438` | Q | — | Consulta do chamado para extrair dias com perguntas vivas. `:2433`, `:2441`. |
| 32 | `chamados/juizes.py:2493` | L | `fio_vivo` | Entrega a lista inteira da disputa ao ponto comum. `:2494`. |
| 33 | `chamados/management/commands/auditar_invariantes_chamados.py:1723` | M | — | Universo F1t, reutilizado na contraprova. `:1726`, `:1731`. |
| 34 | `chamados/management/commands/auditar_invariantes_chamados.py:537` | M | — | Consulta I6 de pares. `:543`. |
| 35 | `chamados/management/commands/auditar_invariantes_chamados.py:860` | M | — | Consulta I8. `:863`. |
| 36 | `chamados/management/commands/auditar_invariantes_chamados.py:896` | M | — | Consulta I8b. `:899`. |
| 37 | `chamados/management/commands/auditar_invariantes_chamados.py:1781` | M | — | Consulta F2 da auditoria. `:1785`. |
| 38 | `chamados/management/commands/auditar_invariantes_chamados.py:1794` | M | — | Consulta F3 da auditoria. `:1798`. |
| 39 | `chamados/management/commands/disparar_perguntas_competencia.py:107` | M | — | Consulta do contador de fio mudo, também executado em alarme. `:109`, `:111`, `config/crons.py:1539`. |
| 40 | `chamados/management/commands/migrar_data_evento_dia_do_turno.py:466` | M | — | A medição da migração relê os PKs do conjunto em laço. `:448`, `:464`, `:467`. |
| 41 | `chamados/models.py:1542` | Q | — | Universo próprio da ressuscitação. C6 julga uma cópia da instância com os campos projetados pela porta. `:1560`, `:1572`, `:1576`. |
| 42 | `chamados/models.py:2057` | Q | — | Segunda lista de `fechar`, após a guarda de veredito da disputa. `:2048`, `:2058`. |
| 43 | `chamados/reconciliador.py:698` | Q | — | Consulta de absolvíveis, usada pelo reconciliador acionado também pelo sinal da batida. `chamados/reconciliador.py:631`, `:708`, `ponto/signals.py:215`. |
| 44 | `chamados/services/acoes_chamado.py:351` | Q | — | Lista própria do detalhe de chamado. `:352`, `:358`. |
| 45 | `chamados/services/competencia_trancada.py:119` | Q | — | Consulta por competência, alcançada também pela medição da API. `chamados/services/competencia_trancada.py:144`, `:347`, `api/views_mensageria.py:1567`. |
| 46 | `chamados/services/competencia_trancada.py:165` | Q | — | Outra consulta, por chamado, no laço da mesma medição. `:160`, `:167`. |
| 47 | `chamados/services/disputa_emissao.py:2078` | Q | — | Lista própria das disputas candidatas no resolver sem credor. `:2072`, `:2082`, `:2091`. |
| 48 | `chamados/services/fio.py:2295` | Q | — | Consulta das perguntas do destino sob a trava da migração do fio. `:2288`, `:2296`. |
| 49 | `chamados/services/fio.py:2299` | Q | — | Consulta distinta das perguntas da origem. `:2301`. |
| 50 | `chamados/services/pergunta_no_app.py:184` | Q | — | Consulta própria de `_marco_ja_perguntado`. `:185`. |
| 51 | `chamados/services/vigia_fio.py:711` | M | — | Diagnóstico de vivas em disputa fechada. `:717`, `:718`, `chamados/management/commands/vigiar_fio_morto.py:189`. |
| 52 | `chamados/services/vigia_fio.py:770` | M | — | Diagnóstico de carimbos sem lastro. `:776`, `:777`, `:814`. |
| 53 | `chamados/views.py:1412` | Q | — | Lista da requisição de reabertura da disputa. `:1413`. |
| 54 | `colaboradores/services/calendario.py:504` | Q | — | Consulta do contexto do calendário. `:507`. |
| 55 | `ponto/services/dia_do_colab.py:64` | Q | — | Consulta própria do dia do colaborador. `:71`. |
| 56 | `chamados/management/commands/estoque_l130_instante_sem_texto.py:37` | M | — | Foto inicial do estoque. `:41`, `:42`. |
| 57 | `chamados/management/commands/estoque_l130_instante_sem_texto.py:97` | M | — | Releitura do conjunto elegível sob trava. `:91`, `:99`. |
| 58 | `chamados/services/validacao.py:431` | R | — | Releitura por ID dentro da porta de devolução L-130. `:408`, `:428`, `:432`. |
| 59 | `chamados/management/commands/arquivar_competencia_encerrada.py:158` | M | — | Relação avaliada pelo comando de arquivamento. `:123`, `:148`. |
| 60 | `chamados/services/vigia_fio.py:570` | M | — | Segunda carga do censo de alcance, para o estado de todas as perguntas. `:560`, `:567`, `chamados/management/commands/vigiar_fio_morto.py:188`. |
| 61 | `chamados/services/explicador.py:160` | Q | — | Consulta do explicador quando a coleção opcional está ausente. `:162`, `:171`. |
| 62 | `api/foto_recebida.py:81` | L | `fio_vivo` | Entrega diretamente a coleção filtrada pelo motivo. `api/foto_recebida.py:80`, `chamados/juizes.py:1526`. |
| 63 | `chamados/management/commands/encerrar_perguntas_com_fato.py:84` | L | `fio_vivo` | Entrega a coleção da disputa à guarda comum. `:82`, `:85`. |
| 64 | `chamados/services/limbo_folgas.py:203` | L | `fio_vivo` | Entrega a consulta do dia e do colaborador inteira à guarda comum. `:204`, `:205`. |
| 65 | `ponto/management/commands/supra_juiz.py:192` | Q | — | O cron adianta perguntas de vários chamados antes de chamar `fio_vivo` por chamado. `:190`, `:215`, `config/crons.py:599`. |
| 66 | `ponto/management/commands/emitir_furo_retroativo.py:83` | M | — | Passe retroativo sob ordem, com prefetch externo e uma chamada de `fio_vivo` por chamado. `:98`, `config/crons.py:1414`. |
| 67 | `ponto/management/commands/reconciliar_grade.py:461` | L | `fio_vivo` | Entrega a consulta de respondidas do chamado inteira à guarda comum. `:456`, `:462`. |
| 68 | `chamados/services/disputa_emissao.py:1429` | L | `fio_vivo` | Depois dos guardas, entrega a consulta daquele motivo e dia à guarda comum. `:1430`, `:1436`. |
| 69 | `chamados/management/commands/fechar_becos_disputa.py:32` | M | — | O comando adianta as perguntas do conjunto de disputas antes de chamar `classificar(d)`. `:33`, `:36`, `:39`, `chamados/services/becos.py:30`. |
| **SOBRA** | `chamados/management/commands/dedup_perguntas_fato.py:75` | — | — | A expressão da linha 9 da matriz recebe uma lista criada aqui e lê resposta em `:76`. K8 aponta para outra consulta, `chamados/services/fio.py:2078`. Esta sobra fica fora dos 69 IDs e acrescentaria um ponto M se incluída. |

## 2. Pontos de toque

| Classe | Carregadores K | Pontos |
|---|---:|---:|
| L | 10 | 5 |
| Q | 29 | 29 consultas |
| R | 5 | 0 — uma leitura por item, por natureza |
| M | 25 | 25 consultas |
| **Total** | **69** | **59** |

Os cinco pontos L são `CargaDaTarefa.__init__` → **1** (`chamados/services/tarefa_do_dia.py:530`), `alimentar_portas` → **1** (`chamados/juizes.py:1668`), `perguntas_vivas_de` → **1** (`chamados/juizes.py:1514`), `_ancoradas` → **1** (`chamados/models.py:357`) e `fio_vivo` → **6** (`chamados/juizes.py:1517`, `:1526`). `perguntas_do_app` já adianta `decisoes`. A partição da tarefa recebe instâncias da carga K1, e K50 é outra consulta. `chamados/services/pergunta_no_app.py:52`, `:57`, `:80`, `:184`, `chamados/services/tarefa_do_dia.py:621`.

Os 59 são pontos para alimentar os **69 K**, com os IDs da tabela como base da soma. O compositor, o ajuste do teto avulso O261, as mudanças de harness e a sobra têm endereços próprios. `chamados/models.py:4295`, `chamados/services/relavra_via.py:236`, `chamados/tests/test_o261_julgamento_desfaz_carimbo.py:205`, `chamados/tests/test_e4c_f5_volta2_juizes.py:144`, `chamados/management/commands/dedup_perguntas_fato.py:75`.

## 3. As três perguntas

**(i) Sim.** `DecisaoPergunta.leitura` já usa a relação pré-carregada. Com os escalares necessários carregados, custa **0 SELECTs de decisões** com prefetch e **1 por chamada** para instância com PK e sem prefetch. Sem PK, usa o passivo em memória. O corpo ordena atos reais e passivo por instante, ordem e PK, aplica os atos que limpam resposta ou validação e devolve `LeituraDoAto`. `chamados/models.py:4251`, `:4268`, `:4295`, `:4308`, `:4326`.

A composição usada pelo C6 está em `projecao_temporal_da_pergunta`. Ela lê **instantes de resposta e validação do ato**, **texto corrente** de `p.resposta_colab` e **materialização e via** da pergunta. Depois aplica o corte por `agora`. `pergunta_respondida(ORM)` combina o instante do ato com texto corrente. A property `respondida` chama o juiz com `LeituraDoAto`, cujo texto vem hoje do ato. O fio faz a composição mista localmente. Assim, o mesmo compositor pode alimentar uma chamada avulsa com uma leitura, preservando o horizonte e a fonte de texto de cada contrato. Adotar texto corrente também na property é a mudança de resultado demonstrada pela fixture só-no-ato da volta recusada. `chamados/juizes.py:1382`, `:1391`, `:1396`, `:1407`, `:1559`, `:1564`, `chamados/models.py:3791`, `:4319`, `chamados/services/fio.py:189`, `:192`, `chamados/tests/test_e4c_f2_leitura_do_ato.py:491`, `:502`.

**(ii) Não, para os 55.** O parâmetro `carga` já encaminha relógio, tranca e testemunhas nos chamadores que o fornecem. O cálculo chama a projeção em `:1411` sem repassar dados de decisão. Na tarefa, a carga de vida recebe a coleção, mas a partição, o preparo de slots e o cálculo usam as instâncias originais. Há um mapa por PK de leituras no horizonte, usado pela tarefa, montado sobre uma sequência recortada. Para usar `carga` como veículo geral faltam alimentar as decisões na coleção ou fornecer uma leitura por instância/PK com o horizonte e os campos exigidos, ligá-la também ao preparo do slot e à partição, e propagar a carga pelos demais percursos. O mapa recortado da tarefa e a leitura que o C6 faz antes de cortar a tupla têm operações temporais diferentes no código. `chamados/juizes.py:866`, `:887`, `:893`, `:898`, `:1411`, `:1416`, `:1526`, `:1672`, `chamados/services/carga_pergunta_viva.py:69`, `:117`, `chamados/services/tarefa_do_dia.py:390`, `:398`, `:427`, `:542`, `:621`, `:628`, `:632`.

**(iii) Um manager padrão com `prefetch_related('decisoes')`** alcançaria as consultas de instâncias via `objects` e as relações reversas que usem esse manager, inclusive a tarefa, portas, ancoradas e relações de disputa. Alcançaria também buscas avulsas que hoje carregam uma pergunta, como API, web, validar, recusar e O261. Nelas, o SELECT de decisões passa para a carga de uma instância. Leitores por `values_list` e faces `Exists` continuam produzindo escalares; instâncias prontas ou oriundas de outra via dependem da carga que as produziu. App e fio já declaram prefetch, e o fio usa um `Prefetch` com queryset próprio. O efeito da sobreposição e a cobertura exata das relações reversas estão [nao verificado]. `chamados/services/tarefa_do_dia.py:530`, `chamados/juizes.py:1668`, `chamados/models.py:357`, `:2057`, `:3552`, `api/views.py:2631`, `chamados/views.py:1239`, `chamados/services/validacao.py:677`, `:894`, `chamados/services/relavra_via.py:236`, `chamados/services/raiox.py:135`, `chamados/services/pergunta_no_app.py:57`, `chamados/services/fio.py:268`.

Quem materializa perguntas sem usar o ato passa a pagar, para um queryset não vazio, **um SELECT de prefetch por avaliação**, mais transferência, instanciação e memória das decisões retornadas. Com iteração em blocos, a carga pode ocorrer por bloco. A consulta de perguntas que apenas identifica a disputa ilustra o custo. `_ancoradas()` tem um selo de **exatamente 1 consulta** e um caso com resultado não vazio, portanto o prefetch padrão acrescentaria uma consulta ao cenário desse selo. No O261, prefetch simples ainda custa uma leitura de decisões por candidata. Os demais deltas de rota estão [nao verificado]. `chamados/services/fio.py:134`, `chamados/services/vigia_fio.py:717`, `chamados/models.py:357`, `:4308`, `chamados/tests/test_e4c_f2_leitura_do_ato.py:537`, `:545`, `chamados/services/relavra_via.py:215`, `:236`, `chamados/tests/test_o261_julgamento_desfaz_carimbo.py:204`, `:205`.

## 4. Tamanho do pouso [nao verificado]

As duas contas abaixo usam os mesmos sítios de carga e incluem a composição da property e do fio examinada na volta 3. A sobra do dedup aparece na lista de arquivos, fora da contagem K. O ajuste de tamanho do bloco do vigia de texto no diff recusado é outro trabalho: a cópia já adianta `decisoes` nessa consulta. `chamados/models.py:3791`, `chamados/services/fio.py:189`, `chamados/management/commands/dedup_perguntas_fato.py:75`, `chamados/services/vigia_texto_resposta.py:11`, `:16`.

| Pouso único | Fatias com a suíte como condição de passagem | [nao verificado]
|---|---|
| União dos arquivos A, B e C listados abaixo. Dos 69 K, **64** recebem alimentação em lote e **5** continuam por item. Verificar em conjunto o compositor, os selos abaixo e os testes da property e do fio. `chamados/models.py:3791`, `:4308`, `chamados/services/fio.py:189`, `:192`. | **A → B → C.** A trata as falhas observadas antes de separar o restante por percurso. Restam **59 → 30 → 5** carregadores K por item, dos quais **54 → 25 → 0** são de lote. Verde em cada fatia [nao verificado]. `chamados/tests/test_e4c_f5_volta2_juizes.py:144`, `:169`, `chamados/tests/test_e2_tarefa_do_dia.py:938`, `escala/tests/test_f3_beco1_folga_na_lista.py:402`, `ponto/tests/test_selo_chokepoint_escrita.py:293`. |

**A — Composição, harness da volta e cargas dos orçamentos vermelhos.** K **1, 2, 9, 10, 15, 16, 17, 18, 24 e 43**. Arquivos de produção: `chamados/juizes.py:1391`, `chamados/models.py:357`, `:3791`, `chamados/reconciliador.py:698`, `:1095`, `:1548`, `chamados/services/disputa_emissao.py:1949`, `chamados/services/fio.py:189`, `:1594`, `:1962`, `chamados/services/relavra_via.py:236`, `chamados/services/tarefa_do_dia.py:530`, `ponto/services/cartorio.py:146`. Arquivos de harness do desenho com delegação `pergunta_respondida(self)`: `chamados/tests/test_e4c_f5_volta2_juizes.py:144`, `:169`, `chamados/tests/test_f1nova_b2_leitores.py:97`, `chamados/tests/test_e4c_f2_leitura_do_ato.py:502`, `chamados/tests/e4c_f5_volta2_mutantes.py:34`, `chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:223`, `:236`. O O261 precisa recuperar **1** consulta unitária segundo a sonda raspada. Para essa conta, incorporar a leitura das decisões na própria releitura pós-trava mantém o trabalho por candidata. A viabilidade e o total dessa implementação estão [nao verificado]. O resultado dos quatro caminhos da batida e a contribuição de cada carga também estão [nao verificado]. `chamados/services/relavra_via.py:219`, `:230`, `:236`, `chamados/tests/test_o261_julgamento_desfaz_carimbo.py:204`, `:205`, `ponto/signals.py:215`, `ponto/tests/test_selo_chokepoint_escrita.py:293`.

**B — Restante de L e Q.** **29** K. Arquivos: `chamados/juizes.py:1514`, `:1517`, `:1768`, `:1775`, `:2438`, `chamados/models.py:1542`, `:2057`, `chamados/views.py:1412`, `chamados/services/acoes_chamado.py:351`, `chamados/services/becos.py:85`, `chamados/services/competencia_trancada.py:119`, `:165`, `chamados/services/declaracao_texto.py:475`, `:588`, `:635`, `chamados/services/disputa_emissao.py:2078`, `chamados/services/explicador.py:160`, `chamados/services

[truncado em 20000 de 29482 caracteres]

## 19/11 16:59 · sessao e4321ca493be · CADEIA-20-PASSO-1 · verificador · origem=Code

O dado fornecido relata **19 células que mudavam na segunda lavra antes da correção e 0 depois**, em 18.333 células de 618 colaboradores, com `ERROS=0`. Conferência independente da medição e do hash da cópia [nao verificado].

### 1. Chamadores de `_julgar_colab_corpo`

**Sim.** Sem `janela`, `d0,d1` continuam sendo as datas da primeira e da última célula da lista recebida. Há três chamadas de produção no código da cópia. `app/ponto/services/cartorio.py:1240`, `:1249`

| Chamada | Lista e janela |
|---|---|
| Primeira passada | `cels` inteira, sem `janela` explícita. `app/ponto/services/cartorio.py:1099` |
| Segunda passada do B5.3b | Só a fila `rejulgar`, ordenada por data, com `forcar=True` e as pontas da `cels` original como janela. `app/ponto/services/cartorio.py:1128–1133` |
| Releitura pós-emissão O222 | `cels` inteira, sem `janela` explícita, com `forcar=False` e contadores próprios. `app/ponto/services/cartorio.py:1196–1200` |

Nos testes, há duas chamadas diretas por extração do corpo, ambas com uma célula e sem `janela`. O espião de `JanelaDoAtoNaSegundaPassadaTest` também chama o corpo real, repassando a lista e os argumentos que recebeu. `app/ponto/tests/test_e3_nb_impressao_memoria.py:173–197`, `app/chamados/tests/test_e3_pos_suite_sem_banco.py:171–178`, `app/escala/tests/test_cadeia20_lavra.py:337–346`

### 2. Ordem, colaborador e meia-noite

- **A fila `rejulgar` é ordenada dentro do cartório.** A ordem da `cels` original vem dos chamadores. O cron, os comandos de veredito velho e de status e o rejulgamento do vínculo consultam um colaborador por vez, ordenam por `data` e pulam a lista vazia. `julgar_celula` busca a célula daquele colaborador e passa `[cel]`. O script `bin/rejulgar_o118_col221.py` também filtra e ordena, mas acessa as pontas antes de testar se há células. `app/ponto/services/cartorio.py:1128–1133`, `:2164–2181`, `app/ponto/management/commands/processar_cartorio.py:61–67`, `app/ponto/management/commands/celula_veredito_velho.py:139–145`, `app/ponto/management/commands/lavrar_status_competencia.py:52–57`, `app/escala/regua_defesa.py:689–696`, `bin/rejulgar_o118_col221.py:20–25`
- **Nos chamadores acima, a última célula pertence ao colaborador do ato.** `julgar_colab` assume esse contrato. Uma lista vazia falha no acesso às pontas já na primeira passada, e uma lista misturada ou fora de ordem fornecida diretamente passa sem validação. A fila normal nasce das células iteradas pelo próprio corpo ou das que ele adiou. O `finally` descarta resíduos antes de o `out` do lote ser reutilizado. `app/ponto/services/cartorio.py:1035–1038`, `:1094–1099`, `:1125–1133`, `:1220–1225`, `:1249`, `:1472–1475`, `:1660–1680`
- **`d1` já deve continuar sendo a última data de célula do ato.** O corpo inclui D+1 na grade com `ref=d1+1` e `dias=(d1-d0).days+2`. A consulta de batidas vai de D0−1 até imediatamente antes de D1+3, e o pareador tem seu próprio dia adicional. O teto de julgamento é outra condição: os marcos atravessam a meia-noite como instantes e vencem quando `quando + margem <= agora`, ou o juízo consulta o encerramento do turno. `app/ponto/services/cartorio.py:1269–1297`, `:901–912`, `:1482–1497`, `app/ponto/turnos.py:1834–1847`, `:2117–2134`, `app/escala/servico_jornada.py:69–94`, `CLAUDE.md:188`

### 3. Equivalência de `horas_em_rajada`

**O conjunto é o mesmo.** Na versão anterior do anexo, toda corrida de tamanho ≥2 entrava no conjunto. Na versão atual, entram as duas pontas de cada par adjacente com diferença ≤15 minutos. Toda ocorrência de uma corrida de tamanho ≥2 toca pelo menos um desses pares. Conferi em memória 9.331 sequências com a implementação da cópia e o algoritmo anterior do anexo. `app/escala/orfa_classe.py:3–21`, `:48`

| Entrada | Conjunto em ambas as versões |
|---|---|
| `08:00, 08:15, 08:30` | As três horas entram, mesmo com 30 minutos entre as pontas. `app/escala/orfa_classe.py:16–21` |
| `08:00, 08:00` | `{08:00}`. A distância é zero. A razão ainda produz uma entrada por ocorrência e liga cada uma pelo instante. `app/escala/orfa_classe.py:16–21`, `:50–76`, `app/escala/tests/test_cadeia20_costura.py:85–92` |
| `23:58, 00:01` | Conjunto vazio para esse par. A ordenação linear por `HH:MM` dá diferença de 1.437 minutos. A virada pelo instante completo está declarada como passo 2. `app/escala/orfa_classe.py:8–21` |

No diff fornecido, `_minutos` era usado internamente por `_runs_de_rajada` e na ordenação em `razao_da_sobra`, que chamava `_runs_de_rajada`. Na busca estática da cópia, não encontrei import dos removidos, inclusive com `as` ou em testes. Os `_minutos` homônimos são definições próprias. O cluster hoje importa `horas_em_rajada` e usa seu valor booleano. `app/escala/orfa_classe.py:8–21`, `:48`, `app/ponto/services/cluster_espurio.py:27–36`, `app/escala/services/meio_periodo.py:36–41`, `app/ponto/services/diagnostico_escala.py:29`, `app/relatorios/management/commands/vinculo_do_dia_divergentes.py:32`

### 4. Remoção de `out`

Os dois chamadores de produção e as duas chamadas diretas de teste usam seis argumentos. Pelo diff, a remoção só mudou a assinatura e as chamadas. O corpo escreve em `ata['razoes_orfas']` e não lia `out`. O `self.out` que sobrou no teste é inicializado vazio e a asserção sobre `sobras_sem_razao` verifica esse dicionário sem entregá-lo à função. `app/ponto/services/cartorio.py:959–998`, `:1410`, `:1991`, `app/escala/tests/test_cadeia20_lavra.py:12–17`, `:29–32`, `:67–72`

### 5. Mordida de `m21` e do teste de janela [nao verificado]

**É o mesmo par teste–mutante.** `m21_segunda_passada_sem_janela_do_ato` tira só `janela=...` da chamada interna. `JanelaDoAtoNaSegundaPassadaTest` enfileira a segunda de quatro células, chama o corpo real e exige que a segunda passada receba só aquela célula. Depois exige pelo menos duas montagens e igualdade de `(dias, ref)`. Em memória, executei o ramo e o cálculo extraídos da cópia. A cópia deu `(5, 16/06)` nas duas passadas. Com `m21`, a segunda deu `(2, 14/06)`, o que derruba a asserção `janela diferente da do ato`. Suprimir a segunda passada já derrubaria a primeira asserção do teste. `app/escala/tests/test_cadeia20_mutantes.py:80–83`, `:114–138`, `app/escala/tests/test_cadeia20_lavra.py:329–356`, `app/ponto/services/cartorio.py:1128–1133`, `:1249`, `:1291–1293`

A execução dos testes ORM e a comparação dos campos gravados por esse teste [nao verificado]. A asserção do selo observa a lista de células e os argumentos da montagem da grade. `app/escala/tests/test_cadeia20_lavra.py:341–356`

### 6. Quem escreve e quem lê

| Campo | Escritor e leitores na cópia |
|---|---|
| `ata.razoes_orfas` | A grade fornece turnos, avulsas e instantes. `_lavrar_razoes_sobras` liga as ocorrências pela data da linha da grade e pela hora, consulta `razao_da_sobra` por instante e escreve a chave. O leitor direto localizado é `medir_razoes_sobras`, que confere quantidade, ordem, hora, razão e falha na ata. `app/escala/utils.py:2117–2118`, `app/ponto/services/cartorio.py:959–998`, `:1001–1032`, `app/escala/orfa_classe.py:37–76` |
| `ata`, `veredito`, `veredito_via`, `veredito_em`, `protestos` | `lavrar_veredito` prepara os campos alterados e respeita a precedência da decisão do admin. `carimbar` persiste os campos da lavra junto com os próprios. O cluster lê `orfas` e `lampadas`. A fábrica lê ata e veredito. `decisao_do_dia` lê veredito, via e instante. `furos_sem_cobranca` filtra veredito e protestos. `app/ponto/services/cartorio.py:1988–2013`, `app/ponto/portas/celula.py:1049–1098`, `:1181–1201`, `app/ponto/services/cluster_espurio.py:27–36`, `app/chamados/services/fabrica_da_celula.py:62–71`, `:119–124`, `app/ponto/services/veredito_celula.py:85–109`, `app/ponto/services/furos_sem_cobranca.py:55–63` |
| `status_dia`, `status_rotulo` | A mesma porta de veredito chama `_aplicar_status`. O leitor `status_dos_dias_lavrados` usa o par gravado quando o dia já saiu de curso. `app/ponto/portas/celula.py:1024–1029`, `:1075–1096`, `app/ponto/status_dia.py:287–289` |
| `julgada_em`, `impressao`, `ausencia`, `tem_entrada`, `tem_saida`, `insumos_em` | `carimbar` grava os seis. O cartório lê `julgada_em` e `impressao` para decidir o pulo. Fábrica e cluster usam o julgamento anterior. O vigia B5 lê os quatro campos de insumos e os coteja com cobertura e ata. O backfill também escreve esses quatro pela porta de lote. `app/ponto/portas/celula.py:1181–1201`, `:1220–1237`, `app/ponto/services/cartorio.py:1094–1098`, `:1378`, `:1528`, `app/chamados/services/fabrica_da_celula.py:123–124`, `app/ponto/services/cluster_espurio.py:164–169`, `app/ponto/management/commands/vigia_b5_ausencia.py:54–81`, `app/ponto/management/commands/backfill_b5_insumos.py:70–84` |

**Há leitores com recortes diferentes.** O cluster seleciona células por `cel.data` na janela de abertura, consulta o dia próprio do chamado no fechamento e usa as horas da ata. A fábrica ancora o teto no turno de `cel.data`. Já `_plantio_preenche_furo_na_data` seleciona a célula pelo **dia civil** do instante quando não há pergunta. Com pergunta, usa a célula dona ou `data_evento` e posiciona a batida no eixo do turno pela diferença de datas. A costura explicita por que isso importa: uma sobra civil de D+1 pode estar lavrada na célula do turno D. `app/ponto/services/cluster_espurio.py:40–49`, `:53–76`, `app/chamados/services/fabrica_da_celula.py:35–49`, `app/chamados/services/materializacao.py:433–460`, `app/escala/tests/test_cadeia20_costura.py:110–132`

### 7. Classes de reprovação

| Classe | Conferência do delta |
|---|---|
| **2º juiz** | O B5.3b volta ao mesmo corpo e ao mesmo classificador. Razão e cluster compartilham o predicado de rajada. `app/ponto/services/cartorio.py:1063–1065`, `:1099`, `:1132–1133`, `:1590–1601`, `app/escala/orfa_classe.py:48`, `app/ponto/services/cluster_espurio.py:34–36` |
| **2º escritor fora da porta** | A razão é montada no dicionário entregue à porta. `salvar_e_emitir` faz o save e inclui o evento no mesmo `atomic` quando muda ata ou veredito. A outra operação que grava na mesma ata, `carimbar_precedencia`, escreve a sua chave e a lavra a preserva. `app/ponto/services/cartorio.py:1991–2013`, `app/ponto/portas/celula.py:106`, `:942`, `:957–965`, `:1049–1059`, `:1205–1217` |
| **Relógio fora da borda** | As duas passadas recebem o mesmo `agora` e o encaminham à grade e às portas. Há uma leitura preexistente a registrar: `julgar_celula` consulta `_tz.now()` na guarda de hoje mesmo quando recebeu `agora`. `app/ponto/services/cartorio.py:1086–1099`, `:1132–1133`, `:1293`, `:1992–2013`, `:2172–2181` |
| **Derivação paralela** | A cópia consolida a vizinhança **entre duas sobras**. Os outros usos dos 15 minutos comparam batida com batida dona do marco ou com uma luz acesa, por instantes. `app/escala/orfa_classe.py:16–21`, `app/ponto/services/cluster_espurio.py:31–36`, `app/escala/utils.py:502–508`, `app/ponto/turnos.py:1729–1745` |
| **Selo sem mordida** | O cálculo em memória distingue `m21` na asserção de janela. A asserção residual sobre `self.out` é inócua. O selo que detecta a reintrodução de um parâmetro sem uso é `ParametrosLidosTest`. `app/escala/tests/test_cadeia20_mutantes.py:80–83`, `app/escala/tests/test_cadeia20_lavra.py:67–72`, `:277–305`, `:351–356` |
| **Campo com dois significados** | `d1` é data de célula, `ref` é a borda da grade e `agora` é o instante do ato. Na ata, a chave ausente indica falta de lavra e `[]` registra ausência de sobras com insumos disponíveis. `hora` guarda o rótulo `HH:MM` e pode repetir. A ligação por instante fica nos insumos, e `razao` e `falha` são campos distintos. Uma sobra atribuída a um turno e uma sobra residual sem turno podem cair em datas diferentes pelas regras da grade. `app/ponto/services/cartorio.py:963–998`, `:1007–1018`, `:1249`, `:1291–1293`, `app/escala/orfa_classe.py:50–76`, `app/escala/utils.py:1778–1785`, `:1841–1871` |

## 19/11 16:56 · sessao d7e1cea86af2 · FERIADO-POR-POSTO · verificador · origem=Code

**Item 4, volta 2.** Os dois caminhos que criam vínculo tentam gravar uma linha quando a chave nasce diferente da herdada. A gravação é *best effort*: se o insert da auditoria falhar, o vínculo pode ficar gravado sem a linha. [vinculo.py:1093](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/colaboradores/services/vinculo.py:1093) [observ.py:124](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/core/observ.py:124)

### 1. Criação e reabertura

- **Ajuste e desfazer com renascimento** passam por `_nasce_pela_marca` imediatamente após seus respectivos `create`. O ajuste compara com a chave de `ec`. O desfazer compara com a de `antigo`. Chaves iguais deixam zero linhas. [vinculo.py:1098](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/colaboradores/services/vinculo.py:1098) [vinculo.py:1114](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/colaboradores/services/vinculo.py:1114) [vinculo.py:1138](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/colaboradores/services/vinculo.py:1138)
- **Desfazer com `volta == novo.data_inicio`** pode recusar se houver célula editada. Se prosseguir, apaga o novo, atualiza apenas `data_fim=None` e `ativa=True` no PK antigo e o relê. Esse ramo pula o helper, preserva a chave gravada no antigo e pode reabrir um vínculo divergente da marca atual. [vinculo.py:1128](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/colaboradores/services/vinculo.py:1128) [feriado_do_posto.py:218](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/colaboradores/services/feriado_do_posto.py:218)

### 2. Formato e leitores

- **Mesmo formato** nos cinco campos pedidos. `acao='editar'`, `modelo='EscalaColaborador'`, `objeto_id` igual ao PK do vínculo alvo e os dois JSONs com `trabalha_em_feriado` booleano. No nascimento, `valor_antes` vem do vínculo de origem. Na porta, vem do próprio vínculo editado. A descrição do nascimento identifica o posto e o copiador. [vinculo.py:894](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/colaboradores/services/vinculo.py:894) [vinculo.py:1098](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/colaboradores/services/vinculo.py:1098)
- **Filtro pela chave.** O uso literal localizado é `_trilha_da_chave`, nos testes. Ele aceita as duas origens. Os testes R1 contam por PK de fixture. A4 compara marca e chave dos vínculos vigentes, e o plano calcula `propagar` pela chave atual. A porta retorna `len(plano['propagar'])`. Esses três números independem da nova linha. [test_feriado_por_posto.py:117](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/colaboradores/tests/test_feriado_por_posto.py:117) [test_feriado_por_posto.py:150](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/colaboradores/tests/test_feriado_por_posto.py:150) [feriado_do_posto.py:213](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/colaboradores/services/feriado_do_posto.py:213) [feriado_do_posto.py:263](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/colaboradores/services/feriado_do_posto.py:263) [feriado_do_posto.py:306](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/colaboradores/services/feriado_do_posto.py:306)
- **Filtro amplo.** A tela de auditoria exibe e conta a linha quando os filtros incluem `editar` e `EscalaColaborador`. Os testes de correção retroativa e de régua também consultam esse par por PK. O leitor com risco concreto é `vigencia_sem_trilha`: ele aceita qualquer linha desse modelo pelo PK do vínculo **ou pelo ID do colaborador**, sem exigir campos de vigência ou autor. A nova linha pode, portanto, ocultar uma vigência sem trilha própria, inclusive por colisão de IDs. Ocorrência em produção [nao verificado]. [core/views.py:698](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/core/views.py:698) [test_contract_porta_2ato.py:190](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/colaboradores/tests/test_contract_porta_2ato.py:190) [test_termometro_grava_com_trilha.py:19](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/escala/tests/test_termometro_grava_com_trilha.py:19) [vigencia_sem_trilha.py:43](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/escala/services/vigencia_sem_trilha.py:43)

### 3. Usuário, IP e selo

- No comando `manage.py`, fora de requisição, ficam `usuario=NULL`, `ip=NULL` e `ator='sistema:ajustar_escalas_auto'`. Fora de `manage.py` e sem requisição, `ator` também pode ficar nulo. Se houver requisição no contexto do middleware, `registrar_log(None, ...)` ainda pode herdar o usuário autenticado e o IP, com prioridade para `X-Forwarded-For`. [auditoria.py:17](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/core/auditoria.py:17) [observ.py:22](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/core/observ.py:22) [observ.py:108](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/core/observ.py:108)
- O modelo aceita os nulos. O teste A3 exige uma linha e os JSONs. O selo E5 aceita autoria de sistema sem IP. Já R1 e o selo da edição humana do vínculo exigem `usuario_id`. Assim, no caminho CLI, o carimbo de sistema satisfaz o padrão E5. A exigência literal de usuário da L-007, se aplicada ao nascimento, fica sem preenchimento. [core/models.py:295](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/core/models.py:295) [test_feriado_por_posto.py:139](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/colaboradores/tests/test_feriado_por_posto.py:139) [test_feriado_por_posto.py:524](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/colaboradores/tests/test_feriado_por_posto.py:524) [test_trilha_com_contexto.py:75](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/core/tests/test_trilha_com_contexto.py:75) [test_trilha_do_vinculo.py:93](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/colaboradores/tests/test_trilha_do_vinculo.py:93) [LEIS.md:65](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/docs/LEIS.md:65)

### 4. Os quatro testes mordem [nao verificado]

| Caso | Mutante que derruba o teste | [nao verificado]
|---|---|
| FECHA nos dois | Tratar a marca por valor-verdade faz `False` cair na herança de `True`. Falha no ajuste. Se a regressão estiver só no desfazer, falha na segunda asserção. [test_feriado_por_posto.py:480](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/colaboradores/tests/test_feriado_por_posto.py:480) |
| Ciclo fora e desfazer sem marca | Ignorar o predicado aplica OPERA ao destino `12x36`. Forçar `True` no desfazer sem marca quebra a cópia. Auditar toda cópia quebra a asserção de zero linhas. [test_feriado_por_posto.py:490](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/colaboradores/tests/test_feriado_por_posto.py:490) |
| Marca mudou antes de desfazer | Ler a marca antiga pelo objeto do ajuste (`novo.posto`) devolve `True` em vez do `False` lido após `antigo.refresh_from_db()`. [test_feriado_por_posto.py:505](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/colaboradores/tests/test_feriado_por_posto.py:505) |
| Chave diferente deixa trilha | Desligar o log deixa zero linhas. Trocar o payload quebra a comparação de `False → True`. [test_feriado_por_posto.py:518](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/colaboradores/tests/test_feriado_por_posto.py:518) |

Executei os quatro corpos em memória com ORM e callbacks simulados. Sobreviveram dois mutantes relevantes: retirar só a trilha do **desfazer** e forçar `False` quando falta marca, pois a fixture sem marca já herda `False`. Suíte Django com banco [nao verificado]. [test_feriado_por_posto.py:490](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/colaboradores/tests/test_feriado_por_posto.py:490) [test_feriado_por_posto.py:505](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/colaboradores/tests/test_feriado_por_posto.py:505) [test_feriado_por_posto.py:518](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/colaboradores/tests/test_feriado_por_posto.py:518)

### 5. Transação e linha órfã

- **Pelo executor**, `create` e auditoria estão no mesmo `atomic` externo. Se o `create` levantar, o helper nem começa. Se uma exceção posterior escapar do `atomic`, ambos voltam. Se falhar apenas o insert do log, o savepoint da auditoria volta e a falha é absorvida. Os copiadores chamados diretamente dependem de transação do chamador para unir fechamento, criação e trilha. [escala_auto_executor.py:101](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/escala/services/escala_auto_executor.py:101) [escala_auto_executor.py:144](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/escala/services/escala_auto_executor.py:144) [vinculo.py:1108](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/colaboradores/services/vinculo.py:1108) [observ.py:85](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/core/observ.py:85) [observ.py:124](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/core/observ.py:124)
- **O desfazer posterior pode deixar uma linha apontando para PK apagado.** O ramo de reabertura apaga `novo`, e `LogAuditoria.objeto_id` é inteiro, sem FK que acompanhe a remoção. [vinculo.py:1128](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/colaboradores/services/vinculo.py:1128) [core/models.py:295](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/core/models.py:295)

### 6. Outras classes

- **Segundo escritor.** O selo R8 passou na varredura da escrita da **marca do posto**. Para a **chave do vínculo**, `sincronizar_feriado_admin --apply` usa a porta canônica, mas fornece uma decisão só por cargo. Pode voltar a gravar `True` num vigilante de posto FECHA. O trecho retroativo é outro nascimento que copia a chave diretamente. [test_selo_marca_do_posto_um_escritor.py:123](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/colaboradores/tests/test_selo_marca_do_posto_um_escritor.py:123) [sincronizar_feriado_admin.py:42](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/colaboradores/management/commands/sincronizar_feriado_admin.py:42) [vinculo.py:976](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/colaboradores/services/vinculo.py:976)
- **Relógio fora da borda.** O executor passa `hoje` ao copiador e à regeneração explícita. O `post_save` acionado pelo `create` lê outro `timezone.localdate()` para recomputar turnos. No lote CLI, o dry e cada aplicação também podem obter datas separadamente. [escala_auto_executor.py:98](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/escala/services/escala_auto_executor.py:98) [ponto/signals.py:53](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/ponto/signals.py:53) [ajustar_escalas_auto.py:56](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/escala/management/commands/ajustar_escalas_auto.py:56) [ajustar_escalas_auto.py:93](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/escala/management/commands/ajustar_escalas_auto.py:93) [escala_auto.py:76](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/escala/services/escala_auto.py:76)
- **Derivação paralela.** Os copiadores compartilham `chave_ao_nascer`. A criação normal usa `padrao_trabalha_em_feriado`, que aceita a marca declarada sem testar cargo ou ciclo. Assim, um `12x36` num posto FECHA pode nascer `False` pela criação normal e herdar `True` pelo copiador. [feriado_do_posto.py:174](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/colaboradores/services/feriado_do_posto.py:174) [vinculo.py:287](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/colaboradores/services/vinculo.py:287) [escala/models.py:905](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/escala/models.py:905)
- **Campo com dois significados.** `valor_antes` agora representa tanto o estado anterior do próprio `objeto_id` quanto a chave herdada de outro PK. A distinção fica na descrição. `objeto_id` é corretamente o vínculo nas duas escritas novas. A interpretação histórica como ID da pessoa continua no leitor de vigência. [vinculo.py:899](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/colaboradores/services/vinculo.py:899) [vinculo.py:1100](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/colaboradores/services/vinculo.py:1100) [vigencia_sem_trilha.py:43](/home/ronald/codex_ws/21f3dbcea8fb28934ca70beef5712b48ca90d5fc/app/escala/services/vigencia_sem_trilha.py:43)

## 19/11 16:47 · sessao b1bf7522323d · FERIADO-POR-POSTO · verificador · origem=Code

**FERIADO-POR-POSTO, item 4.** Os dois pontos já criavam vínculos. O diff muda a chave gravada no nascimento, mas deixa essa mudança sem trilha própria. Também restringe a adoção da marca aos ciclos 5x2, 6x1 e semanal, condição que não aparece no literal do aval fornecido. Dos quatro testes A3, dois detectam a volta à cópia, um por ponto de criação. `app/colaboradores/services/vinculo.py:1099`, `app/colaboradores/services/vinculo.py:1123`, `app/colaboradores/services/feriado_do_posto.py:174`, `app/escala/models.py:32`, `app/colaboradores/tests/test_feriado_por_posto.py:446`, `app/colaboradores/tests/test_feriado_por_posto.py:460`.

A cópia é um arquivo extraído do HEAD, e os três arquivos alterados conferem com os identificadores do diff anexado. O histórico Git do commit e os efeitos em produção ficam **[nao verificado]**. `CLAUDE.md:344`. [nao verificado]

### 1. Escritores da chave e alcance do R8

| Escritor no código de produção | O que grava |
|---|---|
| `executar_vinculo` | `create` com `padrao_trabalha_em_feriado`. No ramo que reaproveita um vínculo, `existente.save()` persiste o objeto inteiro, incluindo a chave já carregada. `app/colaboradores/services/vinculo.py:259`, `app/colaboradores/services/vinculo.py:283`, `app/colaboradores/services/vinculo.py:287`, `app/colaboradores/services/vinculo.py:296`. |
| `marcar_trabalha_em_feriado` | Atribui e faz `save(update_fields=...)` num vínculo existente. Recebe chamadas da porta da marca e de `sincronizar_feriado_admin --apply`. `app/colaboradores/services/vinculo.py:894`, `app/colaboradores/services/feriado_do_posto.py:306`, `app/colaboradores/management/commands/sincronizar_feriado_admin.py:46`, `app/colaboradores/management/commands/sincronizar_feriado_admin.py:53`. |
| `criar_trecho_retroativo` | `create` inativo com cópia da chave do vínculo de origem. É o trecho retroativo preservado pelo diff, hoje na linha 983. `app/colaboradores/services/vinculo.py:976`, `app/colaboradores/services/vinculo.py:983`. |
| `ajustar_vinculo_pelo_sistema` | `create` com `chave_ao_nascer`, usando o **template derivado**. `app/colaboradores/services/vinculo.py:1096`, `app/colaboradores/services/vinculo.py:1099`, `app/colaboradores/services/vinculo.py:1103`. |
| `desfazer_ajuste_do_sistema` | `create` com `chave_ao_nascer`, usando o **template antigo**. O ramo que só reabre o vínculo atualiza vigência e `ativa`. `app/colaboradores/services/vinculo.py:1113`, `app/colaboradores/services/vinculo.py:1123`, `app/colaboradores/services/vinculo.py:1127`. |

O censo estático dos usos explícitos no `app` encontrou esses quatro `create` e a atribuição com `save`. O `update(**campos)` de `gravar_regua` aceitaria a chave se ela fosse passada, mas seus chamadores passam campos da régua. O admin de `EscalaColaborador` é somente leitura. A inicialização implícita é `True` no modelo e na migration de entrada do campo. `app/colaboradores/services/vinculo.py:1079`, `app/escala/regua_defesa.py:665`, `app/escala/regua_defesa.py:722`, `app/escala/regua_defesa.py:745`, `app/escala/regua_defesa.py:768`, `app/escala/admin.py:18`, `app/core/admin.py:53`, `app/escala/models.py:896`, `app/escala/migrations/0014_escalacolaborador_trabalha_em_feriado.py:13`.

Nos testes há escritas diretas adicionais. Os `create` explícitos estão em `app/chamados/tests/test_e2_celula_regenerada_reavalia.py:57` e `:112`, `app/chamados/tests/test_jv1_feriado_vira_vigia.py:56`, `app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:77`, `app/chamados/tests/test_jv1_volta4_posto_do_dia.py:38`, `app/chamados/tests/test_jv1_volta5_posto_do_dia_nos_vetos.py:41`, `app/chamados/tests/test_varredura_feriado_evento.py:46`, `app/colaboradores/tests/test_feriado_por_posto.py:95`, `app/colaboradores/tests/test_feriado_admin_sync.py:34`, `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:82`, `app/core/tests/test_contract_feriado.py:139`, `app/escala/tests/test_celula_dia.py:403`, `app/escala/tests/test_escala_models.py:268`, `app/escala/tests/test_feriado_padrao_por_escala.py:109`, `app/ponto/tests/test_a2_precedencia.py:70` e `:291`, `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:90`, `app/ponto/tests/test_e4cal_l1_d4_natureza.py:78`, `app/ponto/tests/test_furo_feriado_pelo_juiz.py:50`, `app/ponto/tests/test_precedencia_vinculo_encerrado.py:43`, `:47` e `:98`, `app/ponto/tests/test_reavaliar_feriado_pelo_juiz.py:54` e `app/ponto/tests/test_residuo_alem_da_competencia.py:35`. Os `update` diretos estão em `app/chamados/tests/test_e2_celula_regenerada_reavalia.py:76`, `app/chamados/tests/test_jv1_feriado_vira_vigia.py:162` e `:182`, `app/ponto/tests/test_o280_bateria_da_lei.py:1736` e `app/ponto/tests/test_precedencia_vinculo_encerrado.py:69`. As atribuições seguidas de `save` estão em `app/escala/tests/test_escala_models.py:277` e `app/ponto/tests/test_a2_precedencia.py:318`.

Os dois `create` continuam gravando diretamente. Eles compartilham **o predicado de elegibilidade** com a porta, via `segue_a_marca`. A porta grava por `marcar_trabalha_em_feriado`, e os nascimentos calculam o valor por `chave_ao_nascer`. O R8 está em `test_selo_marca_do_posto_um_escritor.py`. Ele percorre `vinculo.py`, mas fiscaliza **`Posto.opera_em_feriado`** e chamadas a `marcar_opera_em_feriado`. Exige um escritor, `estrutura.marcar_opera_em_feriado`, e um chamador, `aplicar_marca`. Os dois `create` de vínculo estão fora da regra que o selo verifica. `app/colaboradores/services/feriado_do_posto.py:174`, `app/colaboradores/services/feriado_do_posto.py:186`, `app/colaboradores/services/feriado_do_posto.py:209`, `app/colaboradores/services/feriado_do_posto.py:307`, `app/colaboradores/tests/test_selo_marca_do_posto_um_escritor.py:20`, `app/colaboradores/tests/test_selo_marca_do_posto_um_escritor.py:76`, `app/colaboradores/tests/test_selo_marca_do_posto_um_escritor.py:123`.

### 2. Leitores e significados

- **Chave do vínculo.** `feriado_vira_folga` combina chave `False`, ciclo elegível e feriado no lugar do vínculo. A grade a usa se o dia-base seria trabalhado. A precedência também consulta o predicado, o fechamento o usa para separar rubricas e a alimentação do calculador o passa em `folga_e_feriado`. Um `False` em 12x36, por exemplo, não faz o feriado virar folga. `app/escala/models.py:1367`, `app/escala/models.py:1389`, `app/ponto/precedencia.py:349`, `app/ponto/precedencia.py:376`, `app/ponto/services/fechamento.py:944`, `app/ponto/calculador/alimentacao.py:514`.
- **Marca do posto.** `cobra_no_dia` aplica um veto próprio no feriado de um posto marcado `False`, inclusive quando o vínculo tem chave `True` ou está fora dos ciclos elegíveis. Marca `None` deixa a cobrança seguir por esse veto. A pauta usa `None` para selecionar postos ainda sem declaração. `app/colaboradores/services/feriado_do_posto.py:60`, `app/colaboradores/services/feriado_do_posto.py:125`, `app/colaboradores/services/feriado_do_posto.py:136`, `app/ponto/management/commands/detectar_ausencias.py:616`, `app/ponto/management/commands/detectar_ausencias.py:628`.
- **Inicialização, comparação e exibição.** O cadastro comum lê a marca para dar o valor inicial. O plano e o A4 comparam chave e marca, a reaplicação relê a marca, e o comando imprime os valores. O contador antigo conta chaves `True` em 5x2/6x1 e separa postos declarados `True`. O sincronizador por cargo compara a chave com a regra de cargo. `app/escala/models.py:905`, `app/escala/models.py:920`, `app/colaboradores/services/feriado_do_posto.py:213`, `app/colaboradores/services/feriado_do_posto.py:263`, `app/colaboradores/services/feriado_do_posto.py:289`, `app/colaboradores/services/feriado_do_posto.py:313`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:84`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:101`, `app/colaboradores/management/commands/sincronizar_feriado_admin.py:46`.

Os consumidores de jornada e folha leem o valor do vínculo do dia. A dependência da chave anterior que aparece no código é de **inicialização**: o trecho retroativo copia e os dois nascimentos automáticos usam a antiga quando o helper devolve `herdada`. `app/ponto/precedencia.py:349`, `app/ponto/precedencia.py:376`, `app/ponto/services/fechamento.py:948`, `app/colaboradores/services/vinculo.py:983`, `app/colaboradores/services/vinculo.py:1103`, `app/colaboradores/services/vinculo.py:1127`, `app/colaboradores/services/feriado_do_posto.py:190`.

### 3. Predicado e universo antes/depois

Os juízes registrados respondem sobre supressão do trabalho, dobra, competência e vigência do vínculo. `segue_a_marca` extrai a elegibilidade que a propagação já aplicava e reutiliza `cargo_e_administrativo` e a constante de ciclos. O universo permanece igual para os dados do modelo: cobertura em `hoje`, posto presente, filtro opcional pelo posto, ciclo elegível e cargo fora da lista administrativa. Depois do diff, o ciclo é filtrado no ORM e rechecado em Python pela mesma constante. Marca declarada e divergência são verificadas depois, pelo A4 ou pelo plano. `app/docs/JUIZES.md:114`, `app/docs/JUIZES.md:142`, `app/core/juizes.py:310`, `app/core/juizes.py:1324`, `app/escala/alimentacao.py:219`, `app/colaboradores/services/feriado_do_posto.py:174`, `app/colaboradores/services/feriado_do_posto.py:196`, `app/colaboradores/services/feriado_do_posto.py:218`, `app/colaboradores/services/feriado_do_posto.py:263`.

**Há uma derivação paralela no nascimento.** `padrao_trabalha_em_feriado` já responde com que chave nasce um vínculo comum. Ele aceita qualquer marca declarada, em qualquer ciclo e sem examinar cargo. `chave_ao_nascer` repete a leitura e conversão da mesma marca, com condições e valor de reserva diferentes. Exemplo verificável: num posto `OPERA`, um 5x2 de supervisor nasce `True` pelo cadastro comum e nasce `False` pelo ajuste se a chave herdada era `False`. Para cargo não administrativo em 12x36, o diff também preserva a herdada mesmo com marca declarada. Esse é o recorte a mais em relação ao literal fornecido. `app/escala/models.py:905`, `app/colaboradores/services/vinculo.py:296`, `app/colaboradores/services/feriado_do_posto.py:180`, `app/colaboradores/services/feriado_do_posto.py:186`, `app/colaboradores/management/commands/sincronizar_feriado_admin.py:4`.

### 4. Trilha e L-007

`aplicar_marca` registra o antes/depois do posto quando a marca muda. A propagação e a reaplicação registram a troca da **chave** em `LogAuditoria`, por vínculo, com autor e motivo, via `marcar_trabalha_em_feriado`. `app/colaboradores/services/feriado_do_posto.py:284`, `app/colaboradores/services/feriado_do_posto.py:306`, `app/colaboradores/services/feriado_do_posto.py:313`, `app/colaboradores/services/vinculo.py:894`, `app/colaboradores/tests/test_feriado_por_posto.py:131`.

**O nascimento com chave diferente fica sem trilha da chave.** O executor grava `HistoricoVinculo` com a troca de escala e a evidência ou motivo. O conteúdo não inclui a chave anterior e a nova. `autor` é opcional e o comando chama os dois fluxos sem passá-lo. Minha leitura da L-007 é que ela alcança esse nascimento quando ele muda o previsto em feriado: é uma ação crítica de cadastro mesmo sendo `create`. O histórico atual permite investigar a troca de escala, mas deixa sem resposta qual chave mudou. `app/docs/LEIS.md:65`, `app/escala/models.py:1367`, `app/colaboradores/services/vinculo.py:1099`, `app/colaboradores/services/vinculo.py:1123`, `app/escala/services/escala_auto_executor.py:109`, `app/escala/services/escala_auto_executor.py:149`, `app/colaboradores/services/historico_vinculo.py:7`, `app/escala/management/commands/ajustar_escalas_auto.py:39`, `app/escala/management/commands/ajustar_escalas_auto.py:95`.

### 5. Relógio, vigência e competência

O diff não introduz leitura de relógio. A chave é decidida **no `create`, pelo valor presente no objeto `Posto` recebido**, sem histórico por data, releitura forçada ou trava do posto. `inicio` e `volta` datam o vínculo e `hoje` desce para o fechamento da vigência. Já existia uma leitura indireta de relógio no `post_save` do núcleo, para a janela de recomposição dos turnos. `app/colaboradores/services/feriado_do_posto.py:186`, `app/colaboradores/services/vinculo.py:1093`, `app/colaboradores/services/vinculo.py:1107`, `app/colaboradores/services/vinculo.py:618`, `app/ponto/signals.py:53`, `app/ponto/signals.py:72`.

Pelo executor, o ajuste exige `inicio > hoje`, e o desfazimento usa `max(inicio, hoje + 1 dia)`. Chamadas diretas aos dois serviços podem informar início retroativo: o `create` não testa essas datas contra `hoje` nem consulta competência paga. A guarda existente atua na **regeneração de células**, onde pula dias cobertos por exportação vigente ou holerite publicado. Assim, a guarda protege a célula e o cadastro do vínculo tem alcance distinto. Não há consulta nova a competência paga fora da guarda nem alteração do motor no diff anexado. Ocorrência retroativa ou impacto em produção **[nao verificado]**. `app/escala/services/escala_auto_executor.py:98`, `app/escala/services/escala_auto_executor.py:143`, `app/colaboradores/services/vinculo.py:1097`, `app/colaboradores/services/vinculo.py:1120`, `app/ponto/portas/celula.py:218`, `app/ponto/portas/celula.py:284`, `app/folha/export.py:597`.

### 6. Bordas do helper

| Entrada | Resultado |
|---|---|
| `posto=None` ou marca `None` | Devolve `herdada`. `app/colaboradores/services/feriado_do_posto.py:190`. |
| `tipo_escala=None` | `segue_a_marca` dá `False` e o helper devolve `herdada`. `app/colaboradores/services/feriado_do_posto.py:182`, `app/colaboradores/services/feriado_do_posto.py:191`. |
| `cargo=None` | Normaliza para vazio e fica fora da lista administrativa. Com ciclo elegível e marca declarada, devolve a marca. Nos demais casos, devolve `herdada`. `app/colaboradores/management/commands/sincronizar_feriado_admin.py:4`, `app/colaboradores/management/commands/sincronizar_feriado_admin.py:12`, `app/colaboradores/management/commands/sincronizar_feriado_admin.py:19`, `app/colaboradores/services/feriado_do_posto.py:182`, `app/colaboradores/services/feriado_do_posto.py:190`. |
| Marca declarada `False` | Devolve `False` para ciclo elegível e cargo fora da lista administrativa. Caso contrário, devolve `herdada`. `app/colaboradores/services/feriado_do_posto.py:182`, `app/colaboradores/services/feriado_do_posto.py:190`. |

`posto=None` e `tipo_escala=None` são entradas toleradas pelo helper. As duas FKs são obrigatórias no vínculo persistido. `app/escala/models.py:871`.

### 7. Mordida dos quatro testes A3 [nao verificado]

| Teste | Se o ponto correspondente voltar a copiar | [nao verificado]
|---|---|
| `ajustar_nasce_pela_marca` | **Falha.** Herda `False`, espera `True`. `app/colaboradores/tests/test_feriado_por_posto.py:446`. |
| `ajustar_admin_e_sem_marca_copiam` | **Passa.** As duas asserções esperam a chave herdada. `app/colaboradores/tests/test_feriado_por_posto.py:452`. |
| `desfazer_renasce_pela_marca` | **Falha.** Confere que nasceu outro PK e espera `True` embora a chave antiga seja `False`. Também morde se só o desfazer regredir. `app/colaboradores/tests/test_feriado_por_posto.py:460`. |
| `desfazer_admin_copia` | **Passa.** Espera a chave herdada, `False`. `app/colaboradores/tests/test_feriado_por_posto.py:471`. |

Executei as quatro asserções A3 em memória, com dublês de persistência e um mutante de cópia por vez. Caiu exatamente um teste em cada mutante. O R8 passou seus seis testes. A execução A3 com Django e o log do RED original em `HEAD~1` ficam **[nao verificado]**. `app/colaboradores/tests/test_feriado_por_posto.py:423`, `app/colaboradores/tests/test_selo_marca_do_posto_um_escritor.py:119`.

Faltam no A3 o sentido **`FECHA` sobrescreve `True`** nos dois pontos, um ciclo fora do conjunto, o desfazer sem marca e uma asserção da trilha da chave. O ramo de reabertura sem novo PK tem teste no executor, mas ele só verifica a vigência e o histórico geral. `app/colaboradores/tests/test_feriado_por_posto.py:446`, `app/colaboradores/tests/test_feriado_por_posto.py:460`, `app/colaboradores/tests/test_feriado_por_posto.py:471`, `app/escala/tests/test_escala_auto_executor.py:86`.

### 8. Outros achados

- **Marca pode mudar entre o nascimento e o início.** A porta só propaga a vínculos que cobrem `hoje`. Um vínculo automático já criado com início futuro fica fora dessa seleção. Se a marca mudar nesse intervalo, ele pode começar a vigência com a chave decidida no nascimento. O ramo do desfazer que reabre o PK antigo também preserva a chave que encontrar. Ocorrência em produção **[nao verificado]**. `app/escala/alimentacao.py:219`, `app/colaboradores/services/feriado_do_posto.py:205`, `app/colaboradores/services/feriado_do_posto.py:263`, `app/colaboradores/services/vinculo.py:1103`, `app/colaboradores/services/vinculo.py:1113`.
- **O sincronizador por cargo pode reintroduzir divergência.** Ele manda `True` para cargo fora da lista e seleciona vínculos por `ativa=True`, inclusive se o posto estiver marcado `FECHA`. Usa o mesmo escritor de atualização com outra regra para o valor. `app/colaboradores/management/commands/sincronizar_feriado_admin.py:42`, `app/colaboradores/management/commands/sincronizar_feriado_admin.py:46`, `app/colaboradores/management/commands/sincronizar_feriado_admin.py:53`.
- **Há dependência recíproca de módulos, com imports locais.** `vinculo` importa o helper dentro das duas funções, e `feriado_do_posto` importa o escritor dentro de `aplicar_marca`. A cadeia nova chega ao comando para classificar o cargo, mas a função de classificação só normaliza e consulta a lista. A leitura estática não mostrou ciclo de inicialização nesses caminhos. `app/colaboradores/services/vinculo.py:1096`, `app/colaboradores/services/vinculo.py:1122`, `app/colaboradores/services/feriado_do_posto.py:180`, `app/colaboradores/services/feriado_do_posto.py:282`, `app/colaboradores/management/commands/sincronizar_feriado_admin.py:12`.

## 19/11 16:32 · sessao 48ad968282ee · E4C-F5-CONSTROI · censo · origem=Code

## A — Matriz de produção

A cópia contém os dois testes de tipo nas linhas indicadas. A identidade Git da cópia com `e673e0efe` e `refs/heads/e4c-f5-volta2` está [nao verificado]. Os caminhos partem de `app/`. `chamados/juizes.py:1389`, `chamados/juizes.py:1559`.

**P** = `projecao_temporal_da_pergunta`. **R** = `pergunta_respondida`. **P via C6** indica que `pergunta_viva` encaminha a mesma pergunta ao cálculo, salvo acerto do memo avulso. Contei cada expressão de chamada, inclusive ramos diferentes da mesma função. Para intermediárias que selecionam, transformam ou agregam perguntas, a matriz registra a chamada ao juiz e identifica nas colunas de laço e carga os caminhos que a abastecem. `chamados/juizes.py:893`, `chamados/juizes.py:898`, `chamados/juizes.py:1411`, `chamados/juizes.py:1430`, `chamados/juizes.py:1526`.

**Proj.** significa texto corrente da projeção. P pode esvaziá-lo no corte temporal. R, quando recebe ORM, usa o instante da leitura e o texto corrente. **Ato** identifica o texto de `LeituraDoAto`, que incorpora o passivo sem linha correspondente. Com os campos escalares carregados, a leitura custa **1 SELECT por chamada** para ORM com PK sem cache de `decisoes`, **0** com o cache e **0** sem PK. Em R, `LeituraDoAto` e `SimpleNamespace` custam zero dentro do juiz. A coluna de teto indica o escopo do teste quando ele mede a rota inteira. `chamados/juizes.py:1391`, `chamados/juizes.py:1396`, `chamados/juizes.py:1401`, `chamados/juizes.py:1559`, `chamados/juizes.py:1564`, `chamados/models.py:4304`, `chamados/models.py:4308`, `chamados/models.py:4312`, `chamados/models.py:4326`.

**B** = B2b F0. **20** = CADEIA‑20 passo 1. O cruzamento usa o arquivo do chamador e o inventário fornecido em `obras_vizinhas.txt`. A conferência dos diffs das obras está [nao verificado].

| # | Juiz | Chamador | Laço/lista ou avulso | Carregador | Decisões em lote | Teto e teste | Objeto no juiz | `resposta_colab` | Cruza | [nao verificado]
|---|---|---|---|---|---|---|---|---|---|
| 1 | R `chamados/juizes.py:1551` | `api/views.py:2596` `api_disputa_perguntas` | Lista `api/views.py:2596` | Histórico de `api/views.py:2553`, queryset `chamados/services/pergunta_no_app.py:52` | Sim `chamados/services/pergunta_no_app.py:57` | GET, igualdade 1/5 pendentes `api/tests/test_f2_teto_consultas_app.py:60` | ORM `chamados/services/pergunta_no_app.py:52` | Proj. `chamados/juizes.py:1564` | — `api/views.py:2596` |
| 2 | R `chamados/juizes.py:1551` | `api/views.py:2640` `api_disputa_responder` | Avulso `api/views.py:2633` | `.first()` `api/views.py:2631` | Não `api/views.py:2631` | Sem selo `api/views.py:2640` | ORM `api/views.py:2631` | Proj. `chamados/juizes.py:1564` | — `api/views.py:2640` |
| 3 | P `chamados/juizes.py:1382` | `chamados/juizes.py:351` `_slot_esperado` | Lista de slots `chamados/services/carga_pergunta_viva.py:117`, também via fato `chamados/juizes.py:756` | Tarefa `chamados/services/tarefa_do_dia.py:530`, `:625`. Via fato, herda `p` em `chamados/juizes.py:756` | Misto. Tarefa, não nas instâncias `chamados/services/tarefa_do_dia.py:530`, `:540`. Fio, sim `chamados/services/fio.py:268` | Tarefa, igualdade 1/3 `chamados/tests/test_e2_tarefa_do_dia.py:938` | ORM `chamados/services/tarefa_do_dia.py:530` | Proj. `chamados/juizes.py:1396` | 20, B `chamados/juizes.py:351` |
| 4 | P `chamados/juizes.py:1382` | `chamados/juizes.py:1411` `_pergunta_viva_calc` | Avulso e lista, p.ex. `chamados/juizes.py:898`, `:1669` | Instância recebida. Cargas nas linhas 42–96, p.ex. `chamados/juizes.py:1668` | Misto `chamados/services/fio.py:268`, `chamados/juizes.py:1668` | 0 no cálculo com prefetch `chamados/tests/test_e4c_f5_volta2_juizes.py:134`, igualdade 1/5 `:149`. 0 no memo quente `chamados/tests/test_selo_memo_pergunta_viva.py:178` | ORM `chamados/juizes.py:1668` | Proj. `chamados/juizes.py:1396` | 20, B `chamados/juizes.py:1411` |
| 5 | R `chamados/juizes.py:1551` | `chamados/juizes.py:1430` `_pergunta_viva_calc` | Avulso e lista `chamados/juizes.py:898`, `:1669` | Par montado da tupla `chamados/juizes.py:1411`, `:1430` | N/A para o par. A origem ORM é mista `chamados/juizes.py:1430`, `chamados/services/fio.py:268`, `chamados/juizes.py:1668` | 0 no cálculo com prefetch `chamados/tests/test_e4c_f5_volta2_juizes.py:134`, tipo conferido em `:140` | `SimpleNamespace` `chamados/juizes.py:1430` | Proj. recortada `chamados/juizes.py:1396`, `:1402`, `:1430` | 20, B `chamados/juizes.py:1430` |
| 6 | R `chamados/juizes.py:1551` | `chamados/management/commands/auditar_invariantes_chamados.py:178` `_contraprova_i21a` | Lista `chamados/management/commands/auditar_invariantes_chamados.py:177` | Consulta e recorte `chamados/management/commands/auditar_invariantes_chamados.py:1586`, `:1592` | Não `chamados/management/commands/auditar_invariantes_chamados.py:1586` | Sem selo `chamados/management/commands/auditar_invariantes_chamados.py:178` | ORM `chamados/management/commands/auditar_invariantes_chamados.py:1586` | Proj. `chamados/juizes.py:1564` | — `chamados/management/commands/auditar_invariantes_chamados.py:178` |
| 7 | R `chamados/juizes.py:1551` | `chamados/management/commands/auditar_invariantes_chamados.py:597` `handle`, I2 | Lista `chamados/management/commands/auditar_invariantes_chamados.py:596` | `_i2_pend` `chamados/management/commands/auditar_invariantes_chamados.py:590` | Não `chamados/management/commands/auditar_invariantes_chamados.py:590` | Sem selo `chamados/management/commands/auditar_invariantes_chamados.py:597` | ORM `chamados/management/commands/auditar_invariantes_chamados.py:590` | Proj. `chamados/juizes.py:1564` | — `chamados/management/commands/auditar_invariantes_chamados.py:597` |
| 8 | R `chamados/juizes.py:1551` | `chamados/management/commands/auditar_invariantes_chamados.py:1671` `handle`, F1 | `for` e `any` `chamados/management/commands/auditar_invariantes_chamados.py:1666`, `:1671` | Chamados e `_ps` `chamados/management/commands/auditar_invariantes_chamados.py:1642`, `:1667` | Não. Prefetch de perguntas `chamados/management/commands/auditar_invariantes_chamados.py:1645` | Sem selo `chamados/management/commands/auditar_invariantes_chamados.py:1671` | ORM `chamados/management/commands/auditar_invariantes_chamados.py:1667` | Proj. `chamados/juizes.py:1564` | — `chamados/management/commands/auditar_invariantes_chamados.py:1671` |
| 9 | R `chamados/juizes.py:1551` | `chamados/management/commands/dedup_perguntas_fato.py:76` `handle` | Lista por grupo `chamados/management/commands/dedup_perguntas_fato.py:74`, `:76` | `universo_do_grupo` `chamados/management/commands/dedup_perguntas_fato.py:75`, `chamados/services/fio.py:2078`, `:1594` | Não `chamados/services/fio.py:2078`, `:1594` | Sem selo `chamados/management/commands/dedup_perguntas_fato.py:76` | ORM `chamados/services/fio.py:2078` | Proj. `chamados/juizes.py:1564` | — `chamados/management/commands/dedup_perguntas_fato.py:76` |
| 10 | R `chamados/juizes.py:1551` | `chamados/models.py:2006` `DisputaSupervisao.sem_veredito_entre` | Lista `chamados/models.py:2003` | Lista recebida `chamados/models.py:1999`. Disputa `:1979`, CSF `ponto/supra_juiz.py:558`, `:573`, `:574`, irmãs `chamados/juizes.py:2127` | Sim, `prefetch_related_objects` `chamados/models.py:2001` | CSF, igualdade 1/3 disputas e pendentes `chamados/tests/test_o168_custo_da_guarda_constante.py:67`, `:68`, `:76` | `LeituraDoAto` `chamados/models.py:2004`, `:4326` | Ato `chamados/models.py:4319`, `:4326` | B `chamados/models.py:2006` |
| 11 | R `chamados/juizes.py:1551` | `chamados/models.py:3241` `PerguntaDisputa.estado`, ramo viva | Um acesso. Lista no app `api/views.py:2567`, `:2585` e vigia `chamados/services/vigia_fio.py:259` | `self` `chamados/models.py:3235`. App `chamados/services/pergunta_no_app.py:52`, vigia `chamados/services/vigia_fio.py:570` | Misto `chamados/services/pergunta_no_app.py:57`, `chamados/services/vigia_fio.py:570` | GET app, igualdade 1/5 `api/tests/test_f2_teto_consultas_app.py:60` | `LeituraDoAto` `chamados/models.py:3235`, `:4326` | Ato `chamados/models.py:4319`, `:4326` | B `chamados/models.py:3241` |
| 12 | R `chamados/juizes.py:1551` | `chamados/models.py:3244` `PerguntaDisputa.estado`, ramo final | Um acesso. Lista no app `api/views.py:2567`, `:2585` e vigia `chamados/services/vigia_fio.py:259` | `self` `chamados/models.py:3235`. App `chamados/services/pergunta_no_app.py:52`, vigia `chamados/services/vigia_fio.py:570` | Misto `chamados/services/pergunta_no_app.py:57`, `chamados/services/vigia_fio.py:570` | GET app, igualdade 1/5. Exercício específico deste ramo [nao verificado] `api/tests/test_f2_teto_consultas_app.py:60` | `LeituraDoAto` `chamados/models.py:3235`, `:4326` | Ato `chamados/models.py:4319`, `:4326` | B `chamados/models.py:3244` |
| 13 | R `chamados/juizes.py:1551` | `chamados/models.py:3791` `PerguntaDisputa.respondida` | Um acesso. Modal em lista `templates/chamados/partials/modal_fio.html:158`, `:217`. Declaração avulsa `chamados/services/declaracao_texto.py:420` | Fio `chamados/services/fio.py:262`, `:291`. Declaração `chamados/services/declaracao_texto.py:251`, `:377` | Misto `chamados/services/fio.py:268`, `:1962` | Modal ≤2837 `chamados/tests/test_selo_teto_modal.py:93`, `:184` | `LeituraDoAto` `chamados/models.py:3791`, `:4326` | Ato `chamados/models.py:4319`, `:4326` | B `chamados/models.py:3791` |
| 14 | R `chamados/juizes.py:1551` | `chamados/reconciliador.py:366` `lavrar_sem_turno_aberto` | Dentro do lote de reate `chamados/reconciliador.py:1572`, `:1667` | `perguntas_orfas_reatavies` `chamados/reconciliador.py:1548`, `:1569` | Não `chamados/reconciliador.py:1548` | Sem selo `chamados/reconciliador.py:366` | ORM `chamados/reconciliador.py:1548` | Proj. `chamados/juizes.py:1564` | C7, B `chamados/reconciliador.py:366` |
| 15 | R `chamados/juizes.py:1551` | `chamados/reconciliador.py:1130` `veredito_orfa` | Laços `chamados/reconciliador.py:1182`, `:1239`, `:1350`, os dois últimos via `fantasma_pela_lampada` `:1157` | `_orfas_abertas` `chamados/reconciliador.py:1095` | Não `chamados/reconciliador.py:1095` | Sem selo `chamados/reconciliador.py:1130` | ORM `chamados/reconciliador.py:1095` | Proj. `chamados/juizes.py:1564` | C7, B `chamados/reconciliador.py:1130` |
| 16 | R `chamados/juizes.py:1551` | `chamados/services/acoes_disputa.py:134` `acao_regularizar_disputa` | `any` `chamados/services/acoes_disputa.py:134` | `_vivas_de`, `order_by` `chamados/juizes.py:1514` | Não `chamados/juizes.py:1514` | Sem selo `chamados/services/acoes_disputa.py:134` | ORM `chamados/juizes.py:1514` | Proj. `chamados/juizes.py:1564` | — `chamados/services/acoes_disputa.py:134` |
| 17 | R `chamados/juizes.py:1551` | `chamados/services/becos.py:31` `classificar` | Lista `chamados/services/becos.py:30` | `d.perguntas.all()` `chamados/services/becos.py:30`, evento `:85`, comando `chamados/management/commands/fechar_becos_disputa.py:32` | Não. Ambas as entradas adiantam só `perguntas` `chamados/services/becos.py:86`, `chamados/management/commands/fechar_becos_disputa.py:33` | Sem selo `chamados/services/becos.py:31` | ORM `chamados/services/becos.py:30` | Proj. `chamados/juizes.py:1564` | — `chamados/services/becos.py:31` |
| 18 | R `chamados/juizes.py:1551` | `chamados/services/declaracao_texto.py:418` `declarar_hora_colab` | Avulso `chamados/services/declaracao_texto.py:418` | Canônica reaproveitada ou `abrir` `chamados/services/declaracao_texto.py:251`, `:254`, `:377` | Não `chamados/services/fio.py:1962`, `chamados/services/declaracao_texto.py:377` | Sem selo `chamados/services/declaracao_texto.py:418` | ORM `chamados/services/declaracao_texto.py:377`, `chamados/services/fio.py:1962` | Proj. `chamados/juizes.py:1564` | B `chamados/services/declaracao_texto.py:418` |
| 19 | R `chamados/juizes.py:1551` | `chamados/services/declaracao_texto.py:688` `aplicar_declaracao` | Avulso `chamados/services/declaracao_texto.py:688` | Canônica reaproveitada ou `abrir` `chamados/services/declaracao_texto.py:663`, `:667` | Não `chamados/services/fio.py:1962`, `chamados/services/declaracao_texto.py:667` | Sem selo `chamados/services/declaracao_texto.py:688` | ORM `chamados/services/declaracao_texto.py:667`, `chamados/services/fio.py:1962` | Proj. `chamados/juizes.py:1564` | B `chamados/services/declaracao_texto.py:688` |
| 20 | R `chamados/juizes.py:1551` | `chamados/services/disputa_emissao.py:1853` `reabrir_credor_de_pergunta_viva` | S143 `chamados/services/disputa_emissao.py:1986`, backfill `chamados/management/commands/backfill_fio_morto_e4b.py:89`, `:134`, `:171` | Ancoradas `chamados/models.py:357`. Backfill `chamados/management/commands/backfill_fio_morto_e4b.py:89`, `:134`, `:166` | Não `chamados/models.py:357`, `chamados/management/commands/backfill_fio_morto_e4b.py:89`, `:134`, `:166` | S143, teto pareado `chamados/tests/test_e4b_c4_orm.py:499`, `:507`. Batida fecha, tetos 230/294 `ponto/tests/test_selo_chokepoint_escrita.py:196`, `:210`, `:293` | ORM `chamados/models.py:357` | Proj. `chamados/juizes.py:1564` | B `chamados/services/disputa_emissao.py:1853` |
| 21 | R `chamados/juizes.py:1551` | `chamados/services/disputa_emissao.py:2023` `_resolver_perguntas_por_chamado_resolvido` | S128 `chamados/services/disputa_emissao.py:1998` | Pendentes `chamados/services/disputa_emissao.py:1949` | Não `chamados/services/disputa_emissao.py:1949` | S143/S128, teto pareado `chamados/tests/test_e4b_c4_orm.py:499`, `:507`. Batida fecha 230/294 `ponto/tests/test_selo_chokepoint_escrita.py:196`, `:210`, `:293` | ORM `chamados/services/disputa_emissao.py:1949` | Proj. `chamados/juizes.py:1564` | B `chamados/services/disputa_emissao.py:2023` |
| 22 | R `chamados/juizes.py:1551` | `chamados/services/fio.py:712` `processar_post_fio` | `any` `chamados/services/fio.py:712` | `_vivas_de` `chamados/juizes.py:1514` | Não `chamados/juizes.py:1514` | Sem selo `chamados/services/fio.py:712` | ORM `chamados/juizes.py:1514` | Proj. `chamados/juizes.py:1564` | B `chamados/services/fio.py:712` |
| 23 | R `chamados/juizes.py:1551` | `chamados/services/fio.py:1287` `canonica_do_grupo` | Lista `chamados/services/fio.py:1286` | Fato/grupo/vizinhos `chamados/services/fio.py:1962`, `:2078`, `:1594`. Tela `chamados/services/declaracao_texto.py:475`, `:588`, `:635`. Backfill DRY `chamados/management/commands/backfill_decisoes_pergunta.py:531` | Não nas origens citadas `chamados/services/fio.py:1962`, `:2078`, `:1594`, `chamados/services/declaracao_texto.py:588`, `chamados/management/commands/backfill_decisoes_pergunta.py:531` | Sem selo específico `chamados/services/fio.py:1287` | ORM `chamados/services/fio.py:1962`, `:2078` | Proj. `chamados/juizes.py:1564` | B `chamados/services/fio.py:1287` |
| 24 | R `chamados/juizes.py:1551` | `chamados/services/fio.py:1300` `merito_perdedor` | Lista `chamados/services/fio.py:1299` | Perdedoras do plano `chamados/services/fio.py:1335`, `:1344`. Dedup `chamados/management/commands/dedup_perguntas_fato.py:75`, `:82`. Migração `chamados/services/fio.py:2295`, `:2299`, `:2326` | Não nas cargas citadas `chamados/services/fio.py:2078`, `:2295`, `:2299` | Sem selo `chamados/services/fio.py:1300` | ORM `chamados/services/fio.py:2078`, `:2299` | Proj. `chamados/juizes.py:1564` | B `chamados/services/fio.py:1300` |
| 25 | R `chamados/juizes.py:1551` | `chamados/services/materializacao.py:192` `par_do_intervalo` | Um por chamada, tela `chamados/services/fio.py:360`, validação `chamados/services/validacao.py:632`, plantio `chamados/services/materializacao.py:1732`, `:2111` | Fio `chamados/services/fio.py:262`, contador `chamados/services/materializacao.py:324`, validação `chamados/services/validacao.py:677`, releitura `chamados/services/materializacao.py:2117` | Misto `chamados/services/fio.py:268`, `chamados/services/materializacao.py:324`, `:2117`, `chamados/services/validacao.py:677` | Modal ≤2837 `chamados/tests/test_selo_teto_modal.py:93`, `:184`. Selo próprio [nao verificado] | ORM `chamados/services/fio.py:262`, `chamados/services/materializacao.py:2117` | Proj. `chamados/juizes.py:1564` | B `chamados/services/materializacao.py:192` |
| 26 | R `chamados/juizes.py:1551` | `chamados/services/materializacao.py:259` `paridade_absolvida_bug_par_103` | Contador em laço `chamados/services/materializacao.py:328`, `:330`. Porta avulsa `chamados/services/validacao.py:334` | Contador `chamados/services/materializacao.py:324`, porta `chamados/services/validacao.py:333` | Não `chamados/services/materializacao.py:324`, `chamados/services/validacao.py:333` | Sem selo `chamados/services/materializacao.py:259` | ORM `chamados/services/materializacao.py:324`, `chamados/services/validacao.py:333` | Proj. `chamados/juizes.py:1564` | B `chamados/services/materializacao.py:259` |
| 27 | R `chamados/juizes.py:1551` | `chamados/services/materializacao.py:290` `_situacao_da_irma` | No laço do contador `chamados/services/materializacao.py:328`, `:341` | Contador `chamados/services/materializacao.py:324`, busca da irmã `:284` | Não `chamados/services/materializacao.py:284`, `:324` | Sem selo `chamados/services/materializacao.py:290` | ORM da irmã `chamados/services/materializacao.py:284` | Proj. `chamados/juizes.py:1564` | B `chamados/services/materializacao.py:290` |
| 28 | R `chamados/juizes.py:1551` | `chamados/services/materializacao.py:885` `estado_aplicacao_pergunta` | Fio `chamados/services/fio.py:360`, `:381`. Contador `chamados/services/materializacao.py:328`, `:342`. Explicador `chamados/services/explicador.py:124`, `:171` | Fio `chamados/services/fio.py:262`, contador `chamados/services/materializacao.py:324`, explicador `chamados/services/explicador.py:160`, `:162` | Misto `chamados/services/fio.py:268`, `chamados/services/materializacao.py:324`, `chamados/services/explicador.py:160` | Modal ≤2837 `chamados/tests/test_selo_teto_modal.py:93`, `:184`. RED 8 do fio simula este auxiliar `chamados/tests/test_e4c_f5_volta2_juizes.py:172` | ORM `chamados/services/fio.py:262`, `chamados/services/materializacao.py:324` | Proj. `chamados/juizes.py:1564` | B `chamados/services/materializacao.py:885` |
| 29 | R `chamados/juizes.py:1551` | `chamados/services/materializacao.py:1300` `_recusa_sem_anuencia` | Três ramos `chamados/services/materializacao.py:1355`, `:1590`, `:1674`. Laço `:2111` | Seleção `chamados/services/materializacao.py:2104`, instância efetiva `:2117` | Não `chamados/services/materializacao.py:2117` | Sem selo `chamados/services/materializacao.py:1300` | ORM `chamados/services/materializacao.py:2117` | Proj. `chamados/juizes.py:1564` | B `chamados/services/materializacao.py:1300` |
| 30 | R `chamados/juizes.py:1551` | `chamados/services/pergunta_no_app.py:86` `particao_do_app` | `for` `chamados/services/pergunta_no_app.py:85` | App `chamados/services/pergunta_no_app.py:52`, `:77`. Tarefa `chamados/services/tarefa_do_dia.py:530`, `:621` | Misto `chamados/services/pergunta_no_app.py:57`, `chamados/services/tarefa_do_dia.py:530`, `:540` | App 1/5 `api/tests/test_f2_teto_consultas_app.py:60`. Tarefa 1/3 `chamados/tests/test_e2_tarefa_do_dia.py:938` | ORM `chamados/services/pergunta_no_app.py:52`, `chamados/services/tarefa_do_dia.py:530` | Proj. `chamados/juizes.py:1564` | — `chamados/services/pergunta_no_app.py:86` |
| 31 | R `chamados/juizes.py:1551` | `chamados/services/raiox.py:147` `creditos_perguntas` | `for` `chamados/services/raiox.py:146` | `values_list` `chamados/services/raiox.py:135` | N/A, tuplas `chamados/services/raiox.py:137` | Placar ≤45 `chamados/tests/test_selo_teto_placar.py:46`, `:110` | `SimpleNamespace` `chamados/services/raiox.py:147` | Proj. de `p[4]` `chamados/services/raiox.py:138`, `:147` | — `chamados/services/raiox.py:147` |
| 32 | R `chamados/juizes.py:1551` | `chamados/services/validacao.py:682` `validar_pergunta` | Avulso e lotes `chamados/services/validacao.py:1059`, `:1078`, `chamados/management/commands/validar_classe_a.py:102`, `api/foto_recebida.py:51` | Releitura por ID `chamados/services/validacao.py:677` após o conjunto `:1056` | Não `chamados/services/validacao.py:

[truncado em 20000 de 79484 caracteres]

## 19/11 16:24 · sessao 7eb7360ac6bd · FERIADO-POR-POSTO · verificador · origem=Code

# REVISAO-DO-AVAL — FERIADO-RESPOSTAS-1911-1530

Revisei a cópia fornecida em modo somente leitura e usei os anexos colados como evidência de produção. A cópia não tem metadados Git para confirmar de forma independente o HEAD `ff763e67bf1c5570eb88bf892b6788d2e500f89b` [nao verificado].

## 1 — R7 · NAO CONFERE

1. **Premissas.** No ANTES, os seis dias-vínculo que mudaram têm HE 100 de feriado igual a zero. **Cinco têm pagamento total zero**. O sexto, v1287 em 02/11, já tem 8,08 h trabalhadas, 1 h de intra e total 9,08. A sombra mostra os resultados, mas não registra o previsto, a decisão de HE, a vigência da janela de HE nem a resposta de escala certa por dia. A causa individual do zero fica [nao verificado]. O bloqueio L-097 zera o par de folga ou feriado se a janela de HE da empresa vale no dia, o previsto é zero e falta autorização. Para par classificado como folga, há exceção sem escala certa. O oráculo recebe o par colapsado e a lista de dias bloqueados do motor. `sombra_r7.out:16–37`, `ponto/motor_calculo_v2.py:1372–1395,1422–1488,2455–2461`, `ponto/calculador/alimentacao.py:449–471`.

   A dobra ordinária usa o feriado do **início da jornada**, minutos efetivamente trabalhados e `feriado_em_dobra`. A fábrica deriva o booleano de `tipo_ciclo`, com 12x36 simples por padrão e dobra no 12x36 quando há cláusula CCT aplicável. O oráculo lê o booleano do motor e reutiliza o mesmo predicado. Para folga trabalhada, há ainda a classificação de HE 100 de feriado pelo fechamento e pelo oráculo, condicionada à escala certa e a `feriado_vira_folga`. `ponto/motor_calculo_v2.py:914–934,2809–2813,3092–3103,3623–3635,3693–3704`, `core/regua_cct.py:473–479`, `ponto/calculador/nucleo.py:344–351`, `ponto/calculador/regras.py:287–299,343–351,664–695,776–789`, `ponto/services/fechamento.py:928–955`.

2. **Colisão.** O 1530 substitui a premissa e o critério de parada do R7 do 1400, preservando a validação pela regra da L-110 e a lista C1 do 1410. A referência a L-086 precisa apontar para o corte que corrigiu sua cláusula de feriado. A previsão de trabalho não é condição da dobra do feriado. `aval.txt:5`, `feriado_constroi_e_c8_mede_1911_1400.txt:20`, `feriado_complemento_1911_1410.txt:2`, `docs/LEIS.md:118,136`, `ponto/motor_calculo_v2.py:914–934`.

3. **Literal.** São **11 dias-vínculo, de 7 vínculos, em 22 linhas**. Seis dias mudam e cinco ficam iguais, cada um medido em motor e oráculo. `PAROU=12` conta as duas versões dos seis. Os cinco dias iguais são v1303 em 02/11, v1358 em 02 e 15/11 e v1425 em 02 e 15/11. Os três vínculos constam entre os **não propagados**, e seus dias não mudam de folga para trabalho na C2. O filtro permite identificar os sítios de decisão, vigência e ciclo weekday, posto, exclusão administrativa e divergência da chave. A regeneração também preserva célula editada ou lavrada e só vira trabalho onde a escala-base prevê trabalho. Qual condição explica cada caso, e quais eram ciclo, cargo, marca, origem da célula e `feriado_em_dobra` efetivos, ficam [nao verificado]. `sombra_r7.out:4–13,16–37,45`, `sombra_c2b_resumo.out:46–65`, `colaboradores/services/feriado_do_posto.py:174–189,242–244`, `escala/models.py:1365–1395`, `ponto/portas/celula.py:284–297`.

4. **Alcance.** O dinheiro de novembro mudou na sombra, inclusive em quatro `FechamentoMensal`, e o ensaio registra rollback. No v1287, a rubrica `horas_extras_100_feriado` vai de 0 a **7,77 h**, enquanto a coluna `dobra` imprime 15,54. O alcance sobre outubro pago aparece separadamente na C2. `sombra_r7.out:16–17,38–46`, `sombra_c2b_resumo.out:66–72`.

## 2 — Prévia A5 · NAO CONFERE NO LITERAL DA MEDIÇÃO

1. **Premissas.** Hoje o comando mostra marca antes/depois, chamados vivos e alvo de retratação, total de vínculos a propagar e até dez chaves antes/depois. O plano não contém dias de pagamento nem dias previstos sem batida. A lista e o formulário de postos da cópia não têm A5. `colaboradores/management/commands/marcar_feriado_do_posto.py:82–113`, `colaboradores/services/feriado_do_posto.py:207–244`, `colaboradores/views.py:699–739,851–890`, `templates/colaboradores/partials/_postos_tabela.html:1–11`.

2. **Colisão.** Acrescentar os dois lados à proposta é compatível com A5 e C3 se continuar cobrindo todo o intervalo da porta, com motivo e proposta aprovada antes do HTML. Os dois lados descritos no 1530 cobrem efeitos de OPERA. A5 do 1400 também prevê FECHA, cuja ação pode remover jornada e retratar cobrança. A prévia só fecha com o texto anterior se conservar os dias que mudam nesse sentido também. A ação por posto é a do 1300 item 3. A caixa por vínculo cancelada às 11:30 é outra ação. `aval.txt:6`, `feriado_constroi_e_c8_mede_1911_1400.txt:12`, `feriado_complemento_1911_1410.txt:4`, `ordem_e_respostas_1911_1300.txt:4`, `cancela_ui_trabalha_em_feriado_1911_1130.txt:2–5`, `colaboradores/services/feriado_do_posto.py:238–244,263–287`.

3. **Literal.** Os 19 são o universo declarado da C2. O R7 cobre seu recorte com batida, 11 dias de sete vínculos. O 1400 explicita **J=11** para reaplicar marca vigente. O 1300 menciona **11 + 8**. O código do A4 separa `opera_chave_false` e `fecha_chave_true`, e a C2 separa propagados e não propagados, com 11 e oito. Dos 11 propagados na C2, quatro tiveram `marca_muda=True`, portanto essa lista não identifica os J do 1400. A definição e a lista de K no brief ficam [nao verificado]. A C2 mostra 14 viradas na competência atual e uma na anterior, sem presença por dia ou partição J/K. Os totais de (a) e (b) nos 19, por grupo e por passado/futuro, ficam [nao verificado]. `feriado_constroi_e_c8_mede_1911_1400.txt:9,11`, `ordem_e_respostas_1911_1300.txt:4`, `colaboradores/services/feriado_do_posto.py:192–204`, `sombra_r7.out:4–5`, `sombra_c2b_resumo.out:4,24–37,46–66`.

4. **Alcance.** O dry-run atual é leitura. Confirmar pela porta escreve a chave e pode repercutir no dinheiro. O intervalo real inclui o passado desde o maior entre início do vínculo e início da janela anterior, até hoje, e as células futuras geradas já existentes que o suplemento alcança. Dia futuro previsto sem batida é previsão de cobrança. O julgamento de falta considera encerramento e cobertura. `colaboradores/management/commands/marcar_feriado_do_posto.py:89–113`, `colaboradores/services/vinculo.py:894–922`, `ponto/services/cadastro_futuro.py:19–25,43–59`, `ponto/portas/celula.py:464–472,483–496`, `ponto/services/cartorio.py:1296–1326`.

## 3 — Competência anterior paga · NAO CONFERE COMO SUFICIÊNCIA DE POUSO [nao verificado]

1. **Premissas.** `competencia_fechada_para_celula` é a autoridade K9 para a guarda da célula. Considera TXT vigente ou holerite **da pessoa em lote publicado**. A porta passa por `_competencias_exportadas`, que enumera candidatas e consulta K9. Para corte 21, em **21/11/2026** a anterior passa a ser **21/10–20/11, competência 11/2026**. A função usa o corte da empresa. O cadastro COMPETENCIA-PAGA não está integrado nesta cópia. O RELATO registra a c6 parada e o BACKLOG separa deploy do cadastro da 10 na tela. Estado atual do deploy em produção [nao verificado]. `folha/export.py:597–628`, `core/juizes.py:345–352`, `ponto/portas/celula.py:218–221,284–288`, `ponto/management/commands/gerar_celulas.py:155–188`, `ponto/janelas.py:10–33,36–72`, `docs/RELATO.md:1165,1182`, `docs/BACKLOG.md:2073`.

2. **Colisão.** Manter K9 e recusar a candidata (a) é compatível com C2 e com o corte. O corte declara **10/2026 das empresas 2, 3 e 4 paga fora do sistema**. K9 nesta cópia responde a outra pergunta e devolveu aberta para v1355. A comparação da sombra com o corte é condição de pouso, sem precisar criar consulta paralela em produção. `aval.txt:7`, `feriado_complemento_1911_1410.txt:3`, `docs/CORTES.json:1874–1879`, `folha/export.py:617–628`, `sombra_c2b_resumo.out:10,52`.

3. **Literal.** A sombra imprime as fontes de K9. A chamada exata dentro do script da sonda fica [nao verificado]. Com corte 21, a virada de calendário retira 12/10 do intervalo. Publicar o lote do holerite de outubro do col309 faria K9 barrar aquele dia. **COMPETENCIA-PAGA no ar, por si só, não fecha (iii)**. O cadastro da 10 tem de estar efetivo e a guarda tem de alcançar a competência paga só por cadastro, inclusive na enumeração de candidatas. Além disso, **zero virada de folga/trabalho não prova zero alteração de dinheiro**. A regeneração também aciona recálculo por célula criada ou DNA alterado com `trabalha` igual. Continua faltando a prova de FM e DiaPago exigida pelo 1410 C2. `sombra_c2b_resumo.out:5–23,47–71`, `ponto/janelas.py:24–33,66–72`, `folha/export.py:617–628`, `ponto/management/commands/gerar_celulas.py:170–188`, `docs/BACKLOG.md:2073`, `ponto/portas/celula.py:289–318,390–394,483–496`, `feriado_complemento_1911_1410.txt:3`.

4. **Alcance.** Em 19/11 há **1 dia em competência paga pelo corte**, v1355, col309, empresa 2, 12/10. Mudaram os hashes sem carimbo de FM e DiaPago. O FM ganhou **7 h de HE 100 de feriado e 7 h trabalhadas**. A sombra registra rollback. A condição de pouso zero está insatisfeita na evidência fornecida. `docs/CORTES.json:1874–1879`, `sombra_c2b_resumo.out:10,52,66–72`.

## 4 — Copiadores A3 · NAO CONFERE

1. **Premissas.** Na cópia, o ajuste começa em `:1093` e cria com o mesmo posto e a chave de `ec` em `:1098–1101`. O desfazer começa em `:1104`. `:1113` recusa remover vínculo gerador de célula editada. O ramo que cria copia posto, template e chave do antigo em `:1119–1122`. O terceiro sítio começa em `:976`, `criar_trecho_retroativo`, e cria trecho inativo com o mesmo posto e a chave copiada. `colaboradores/services/vinculo.py:976–983,1093–1122`.

2. **Colisão.** O 1400 A3 mandava manter a cópia quando o posto não muda. Os dois sítios mantêm o posto e o 1530 manda mudar o critério de nascimento da chave. É uma alteração literal de A3. O 1300 item 3 autoriza a propagação pela marca quando ela é aplicada ao posto. O 1410 C6 trata do ato do admin na tela. Aqui a atribuição proposta grava **dado de vínculo no `create`**. O caminho normal do ajuste via `--apply` exige `!`; o ramo `--desfazer` chama o executor antes dessa validação e não pede `!`. A 7b item 2 inclui dado de vínculo no texto do `!`. A extensão da decisão de C6 a esses atos é a questão a devolver. `aval.txt:8`, `feriado_constroi_e_c8_mede_1911_1400.txt:10`, `feriado_complemento_1911_1410.txt:6–7`, `ordem_e_respostas_1911_1300.txt:4`, `../CLAUDE.md:265–268`, `escala/management/commands/ajustar_escalas_auto.py:29–48`, `escala/services/escala_auto_executor.py:87–113,118–151`.

3. **Literal.** A função comum é `vinculos_que_seguem_a_marca(*, hoje, posto=None)`. Ela consulta **vínculos já gravados**, weekday, vigentes em `hoje` e fora do cargo administrativo. O plano compara com `novo`; A4 lê `posto.opera_em_feriado` e compara separadamente. Os dois copiadores têm acesso ao posto, cargo e template de destino, mas essa função não aceita o candidato a nascer nem devolve o valor da chave. A4 só enxerga o novo na vigência. O RED do ajuste cabe no serviço com um `tipo_derivado` fornecido. O executor normal deriva horários, intervalo ou folga e preserva `tipo_ciclo`, portanto uma **troca de ciclo** para weekday não ocorre por esse caminho. O RED do desfazer é alcançável quando `hoje >= inicio`, ainda dentro do prazo e com o par de vínculos válido. Antes disso, `volta == inicio`, o novo é apagado e o antigo reabre com a chave existente. `colaboradores/services/feriado_do_posto.py:174–204,242–244`, `escala/alimentacao.py:219–225`, `colaboradores/services/vinculo.py:1093–1122`, `escala/services/escala_auto_executor.py:61–84,127–148`, `escala/services/cadastro_tipo.py:431–446`.

4. **Alcance.** Ajuste e desfazer regeneram explicitamente desde `inicio` ou `volta`. Dias tocados chegam a `recalcular_por_evento` no commit, que pode regravar **FechamentoMensal e DiaPago**. Fechar o vínculo também regenera futuras, nesse trecho com `recalculo=False`. Cada `create` dispara ainda a invalidação de `previsto_em` e `atualizado_em` do FM **sem filtro de competência ou lavra**, além do caminho condicional de reparentar folgas e rejulgar. No desfazer que apaga o novo, o FK `SET_NULL` pode alterar a geradora de célula sem passar pela guarda K9. Ocorrências em exportada nesses atos [nao verificado]. `escala/services/escala_auto_executor.py:101–108,143–148`, `colaboradores/services/vinculo.py:652–664,1110–1122`, `escala/signals.py:137–150,185–204`, `escala/folgas.py:513–525,529–541,283–299`, `escala/models.py:1690–1695`, `ponto/portas/celula.py:483–496`, `ponto/services/fechamento.py:1143–1144,1160–1168,1191–1207,1277–1296,1607–1613,1798–1824`.

## PROIBIDO · CONFERE COMO LIMITE

1. **Premissas.** O comando ainda tem `--apply`, que chama a porta e escreve. A proibição de executá-lo em produção é de escopo do aval. `aval.txt:9`, `colaboradores/management/commands/marcar_feriado_do_posto.py:101–116`.
2. **Colisão.** A proibição preserva os limites de 1400, 1410 C6 e CANCELA-UI-1130, inclusive motor em leitura e ausência da caixa no vínculo. `aval.txt:9`, `feriado_constroi_e_c8_mede_1911_1400.txt:23–24`, `feriado_complemento_1911_1410.txt:7–8`, `cancela_ui_trabalha_em_feriado_1911_1130.txt:4–5`, `../CLAUDE.md:265–268`.
3. **Literal.** O zero é condição de pouso. O valor fornecido é um. Os avais mantêm a precedência da fila 1. `aval.txt:7,9`, `sombra_c2b_resumo.out:66`, `feriado_constroi_e_c8_mede_1911_1400.txt:24`, `feriado_complemento_1911_1410.txt:8`. [nao verificado]
4. **Alcance.** A guarda protege a regeneração ordinária da célula. A invalidação de FM por `save` do vínculo passa por outro caminho, inclusive no `save` da propagação A1. A integridade de todo o gravado da exportada não decorre só de K9. `ponto/portas/celula.py:218–221,284–288`, `colaboradores/services/vinculo.py:894–898`, `escala/signals.py:137–150`, `ponto/services/fechamento.py:1607–1613`.

## PRONTO · NAO CONFERE

1. **Premissas.** Faltam na evidência os motivos individuais dos cinco dias iguais, os totais A5 e a prova de zero no pouso. A proposta respondida, a execução dos REDs dos copiadores, os verdes e o smoke ficam [nao verificado]. `aval.txt:10`, `sombra_r7.out:16–37`, `sombra_c2b_resumo.out:46–72`, `feriado_constroi_e_c8_mede_1911_1400.txt:25`. [nao verificado]
2. **Colisão.** PRONTO acumula 1400 e 1410. Continuam valendo a prova de C2 em FM e DiaPago, o R5 de exportada intacta com hash e a aprovação prévia da proposta da tela. `aval.txt:10`, `feriado_constroi_e_c8_mede_1911_1400.txt:12,18,25`, `feriado_complemento_1911_1410.txt:3–4`.
3. **Literal.** O R5 da cópia compara `trabalha`, DNA, uma rubrica do FM e PKs de DiaPago. Isso não cobre o hash integral pedido. O selo AST verifica escritor e chamador da **marca**, e o teste A4 verifica a função comum e a convergência. A prova específica do mutante «propagação desligada → A4 > 0 e R1 vermelho» fica [nao verificado]. `colaboradores/tests/test_feriado_por_posto.py:303–325,359–377`, `colaboradores/tests/test_selo_marca_do_posto_um_escritor.py:119–162`, `feriado_constroi_e_c8_mede_1911_1400.txt:18,21–22`.
4. **Alcance.** As duas sombras registram erros internos de lavratura do oráculo apesar de `ON_COMMIT erros=0`. A exceção é capturada dentro do fechamento. Isso preserva a evidência da mudança de outubro, mas o contador externo não certifica todas as lavraturas. `sombra_r7.out:14–15`, `sombra_c2b_resumo.out:38–45,66–71`, `ponto/services/fechamento.py:1297–1301`.

PROCEDE: 1, 2, 3, 4, PRONTO

## 19/11 16:02 · sessao fda626687629 · CADEIA-20-PASSO-1 · leitura · origem=Code

# CADEIA-20-PASSO-1

**A primeira hipótese que eu testaria é o rejulgamento interno com uma janela menor.** O Cartório remonta a grade para as células enfileiradas em `rejulgar`. Uma régua diferente consegue produzir exatamente `[] / 0 / 0` → `N lâmpadas apagadas / N / N`, com as demais chaves iguais. A pré-passagem também lê atas anteriores, mas as 19 atas vazias, por si sós, não fornecem papéis ao pareador. Qual caminho ocorreu na execução [nao verificado]. `app/ponto/services/cartorio.py:1045`, `:1160`, `:1202`, `:657`, `:735`, `app/ponto/turnos.py:1631`.

O anexo registra 19 diferenças de ata, `ERROS=0`, relógios iguais e `DESFEITO=True`. Na cópia, `julgar_colab` não recebe `lavra`. A sonda anexada verifica a assinatura antes de passar esse argumento, então, com esta assinatura, `ultimo_lavrado()` é consultado pela sonda e seu retorno fica fora da chamada. A identidade da árvore executada com o hash informado e a configuração efetiva de `GRADE_POR_TURNO` [nao verificado]. `diag_main.txt:22`, `:23`, `:24`, `:25`, `:26`, `app/ponto/services/cartorio.py:959`, `app/ponto/services/cxr_na_lista.py:152`, `app/config/settings/base.py:165`.

## 1. Caminho e escritores

- **`ata_do_dia` produz as três chaves.** Recebe `dia['celulas']`, `orfas`, `minutos_realizados`, a régua da grade e `cel.dna`. `lampadas_do_dia` lê `dna['marcos']`. Se pelo menos uma hora do DNA aparece na régua, percorre **todos** os marcos do DNA e procura a coluna pela dupla `(tipo, hora)`. Fora desse ramo, só gera lâmpadas para colunas que são `dict`. Assim, sem interseção de horas e com colunas todas `None`, sai `[] / 0 / 0`. Com marcos reconhecidos e todos apagados, sai `N / N / N` para tamanho da lista, `n_celulas` e `n_missing`. `n_celulas` é sempre `len(lamp)`. Com lista não vazia, `n_missing` conta `acesa is False`. `app/ponto/services/cartorio.py:633`, `:657`, `:675`, `:699`, `:735`, `:749`, `:765`, `:781`.
- **Dois passos pós-processam a ata.** `dispensar_por_ausencia` lê cobertura aprovada integral ou `dia['justificado']`, altera as lâmpadas existentes e reconta `n_missing`. `_ata_ate_o_teto` lê os marcos pendentes, troca `False` por `None` nos atingidos e reescreve `lampadas` e `n_missing`. Nenhum dos dois aumenta a lista. A chamada descrita deixa `teto_de_hoje` no padrão `False`. `app/ponto/services/cartorio.py:819`, `:831`, `:926`, `:959`, `:1390`.
- **A escrita persistente passa pela porta da célula.** O ramo `nunca_bateu` produz a ata em `:1280` e entrega em `:1321`. O ramo normal produz uma ata em memória para as decisões do ato em `:1465` e produz a que entrega à porta em `:1899`. `lavrar_veredito` atribui a ata e `carimbar` a inclui no `save`. `carimbar_precedencia` copia as três chaves existentes e escreve `precedencia`. A grade também escreve `lampadas` numa **cópia em memória** para consultar os turnos, sem os dois contadores. `app/ponto/services/cartorio.py:1280`, `:1321`, `:1465`, `:1899`, `app/ponto/portas/celula.py:1053`, `:1064`, `:1090`, `:1199`, `:1212`, `app/escala/utils.py:1891`.

## 2. Estado escrito e relido

**O caminho de realimentação existe.** A grade carrega do banco as células da janela com margem, inclusive vizinhas. A pré-passagem entrega essas células a `turnos_do_colab`. `periodos_do_dia` lê as lâmpadas da ata e o DNA. `papel_por_minuto_da_ata` aproveita luzes atribuídas de dias que passam por suas guardas e entrega o mapa a `parear_turnos`. A grade usa o turno da pré-passagem quando há exatamente um turno fechado na data e o dia tem intervalo declarado. Isso pode mudar o alinhamento e o consumo de batidas. A consulta posterior para o realizado usa atas recém-montadas em memória dentro da janela e conserva as do banco na margem. O cache de ata do julgamento é limpo ao sair de `julgar_colab`. `app/escala/utils.py:1315`, `:1590`, `:1599`, `:1712`, `:1891`, `app/escala/alimentacao.py:73`, `app/ponto/juiz_batida.py:157`, `app/ponto/turnos.py:1631`, `:1682`, `:1944`, `:1997`, `app/ponto/services/cartorio.py:1150`.

**Esse caminho não fecha a causa das 19 células.** Uma lista vazia e uma lista toda apagada igualmente não fornecem luzes ao mapa daquele dia. Para uma sequência fixa, tanto o alinhamento por papel quanto o alinhamento por custo devolvem um `dict` por marco, inclusive `{'missing': True}`. É a projeção na régua que pode deixar todos os slots em `None`. Uma ata de outro dia pode mudar os papéis das batidas, mas o anexo não traz as atas **anteriores à primeira chamada**, os papéis nem a régua de cada montagem. A ata ou o estado específico que mudou a decisão [nao verificado]. `app/ponto/turnos.py:1631`, `app/escala/utils.py:623`, `:666`, `:1715`.

**Há uma segunda cadeia que explica a assinatura completa.** Um fechamento ou mudança no fio pode enfileirar uma célula em `rejulgar`. O corpo interno recebe só a lista enfileirada e deriva dela as pontas da janela. Se perder o vínculo de outra data que fornecia à régua a única hora em comum com o DNA congelado, e nenhuma coluna do dia sobreviver à projeção, a última escrita da primeira chamada pode ser `[] / 0 / 0`. Na segunda chamada, o chamado já encerrado pode ficar fora de `vivos`, o recorte interno pode deixar de ocorrer e a régua da janela inteira volta a produzir `N / N / N`. Exercitei em memória o produtor da ata com essa mudança de régua e reproduzi as três chaves para `N=2` e `N=4`. Rejulgamento, fechamento e troca de régua nesses três colaboradores [nao verificado]. `app/ponto/services/cartorio.py:1045`, `:1160`, `:1202`, `:1413`, `:1594`, `:1698`, `:1740`, `:657`, `:735`, `app/escala/utils.py:1324`, `:1364`, `:1715`.

## 3. Cadastro e sonda que separa as hipóteses

O anexo mostra `trabalha=True` e `origem=gerada` nas linhas alteradas. Os dias 05, 07, 09 e 11 do 769 são compatíveis com uma fase 12×36, que usa `delta % 2 == 0` quando há âncora e a regra segue pelo ramo aritmético. Com célula existente, a grade usa `cel.trabalha`. Dois ou quatro itens são compatíveis com duas pontas ou duas pontas mais uma pausa. Virada de meia-noite e ausência total de batidas são hipóteses distintas. Todas as lâmpadas apagadas demonstram ausência de batida **atribuída a esses marcos**, e a ata guarda órfãs separadamente. O ciclo, a fase, a virada e o motivo de o desvio atingir só esses três [nao verificado]. `diag_main.txt:3`, `:18`, `:21`, `app/escala/models.py:1094`, `:1289`, `:1659`, `app/escala/utils.py:330`, `app/ponto/services/cartorio.py:699`, `:754`.

A sonda teria de imprimir, para os dias afetados, seus vizinhos e dias comparáveis sem diferença:

- **Cada entrada no corpo e cada escrita**, com rodada externa, chamada inicial ou interna, motivo de `rejulgar`, `d0/d1/ref`, datas na fila, status dos chamados antes/depois e resultado da ata por escrita. Isso mostra se a primeira fotografia já é resultado de uma sobrescrita interna. `app/ponto/services/cartorio.py:1016`, `:1045`, `:1114`, `:1160`, `:1899`.
- **O desenho e a projeção do dia**, com `GRADE_POR_TURNO`, geradora e vínculo efetivo, vigência, ciclo, âncora, folga lançada, `trabalha` inclusive nos dias pares do 769, `dna.marcos`, pausas, régua de cada corpo, `_casa` e slots `None`/`missing`/casados antes e depois dos overlays. Isso separa alternância de cadastro, régua incompatível e supressão pela grade. `app/escala/alimentacao.py:269`, `app/escala/utils.py:1364`, `:1610`, `:1715`, `:1854`, `:2095`, `app/ponto/services/cartorio.py:657`.
- **Os fatos usados no pareamento**, com batidas apuráveis anonimizadas por instante e tipo, atas do dia e da margem antes da primeira escrita, papéis por minuto, `data_turno`, `aberto`, `cross_meianoite`, órfãs e cobertura/justificativa nas duas montagens. Isso separa dia sem batida, saída da véspera, mudança de papel e efeito de cobertura. `app/ponto/services/cartorio.py:1199`, `app/escala/utils.py:1198`, `:1208`, `:1590`, `:1980`, `app/ponto/turnos.py:1631`.

## 4. Sítio da cura

- **Se a sonda confirmar a troca de régua**, o sítio é `julgar_colab` → `_julgar_colab_corpo`. O conjunto de células a regravar pode continuar restrito, com a montagem da grade alimentada pela janela original do ato. Se a régua original também perder marcos válidos do DNA, o sítio desse segundo defeito é a construção e projeção da régua em `montar_grade_prevista_periodo_por_turno`. `app/ponto/services/cartorio.py:1045`, `:1160`, `:1204`, `app/escala/utils.py:1364`, `:1618`, `:1715`.
- **Se a sonda confirmar dependência do papel da ata anterior**, o sítio é a **alimentação da pré-passagem** em `escala/utils.py:1592`. A montagem pode fornecer ao pareador papéis obtidos dos insumos da própria montagem, com política explícita para a margem. Já existe nessa função o modelo de passar cópias de células com ata montada em memória. O pareador recebe o mapa como argumento. Há, portanto, sítio de correção fora de `parear_turnos` e de `motor_calculo_v2.py`. Qual intervenção resolve o caso medido [nao verificado]. `app/escala/utils.py:1590`, `:1891`, `app/ponto/turnos.py:1192`, `:1997`.

## 5. Leitores de produção e efeito de gravar `N` já na primeira chamada

| Leitor | Efeito para a transição observada |
|---|---|
| **Leitor da célula, espelho e relatórios de furos** | `n_ok_lavrado` passa de `None` para `0`, e a grade passa de zero slots para `N` marcos com `missing=True`. A diferença `n_celulas - n_missing` permanece zero, então ela sozinha não cria fato para o recorte de vínculo. Espelho e relatórios passam a ter marcos para exibir e contar quando o dia satisfaz seus filtros. `app/escala/services/leitor_celula.py:85`, `:114`, `:175`, `:385`, `:413`, `app/ponto/services/espelho.py:533`, `app/relatorios/furos.py:136`, `app/escala/services/furos_diarios.py:43`. |
| **Worklist, prévia, placar e pauta** | A worklist pode passar de “sem lâmpada” para gap em dia de trabalho. Prévia e pauta passam a ter horas a listar. O placar `sem_furo` passa a reconhecer a data se a precedência a classificar como trabalho. O relatório da frota pode nomear os marcos faltantes. `app/ponto/worklist.py:18`, `app/folha/previa.py:77`, `app/folha/services/sem_furo.py:20`, `app/pautas/services.py:432`, `app/relatorios/dia_da_frota.py:20`. |
| **Chamados, reconciliação e materialização** | `lastro` passa a localizar os marcos e pode dizer que um marco nomeado está apagado. O juiz de furo pode passar a responder `True`, conforme suas guardas. O reconciliador pode nomear faltantes sem solicitar nova lavra por lista vazia. A materialização ganha slots previstos. O filtro SQL de provisoriedade procura `pendente=True`. `app/ponto/services/lastro.py:89`, `:113`, `:128`, `:170`, `app/chamados/juizes.py:497`, `:969`, `:1358`, `app/chamados/reconciliador.py:747`, `app/chamados/services/materializacao.py:439`, `app/chamados/models.py:3604`. |
| **Turnos, motor e exceção de retenção por pausa** | Uma ata alinhada ao DNA pode ficar legível com as pontas em aberto. Todas apagadas continuam sem fornecer papéis de batida, sem entregar par ao motor e sem satisfazer a exceção de furo só da pausa. O diagnóstico de cadastro que exige entrada e saída continua a descartar o dia. Efeito efetivo em veredito, retenção ou dinheiro [nao verificado]. `app/ponto/juiz_batida.py:195`, `:258`, `app/ponto/turnos.py:1636`, `app/ponto/motor_calculo_v2.py:616`, `app/folha/export.py:1150`, `app/escala/services/cadastro_realidade.py:238`. |
| **Diagnósticos e vigias** | Dia do colaborador e diagnóstico de escala passam a mostrar marcos apagados. Vigia de entrada pode incluir o dia em `nada_acendeu`. Presença em feriado pode ganhar um dia previsto sem presença. Cluster passa a ter o requisito de lâmpada apagada, ainda condicionado às órfãs. O vigia do pareador ganha marcos para confrontar quando tem turno fechado. Esmeril e cartão consultam luz acesa e crédito de isenção, respectivamente. `app/ponto/services/dia_do_colab.py:30`, `app/ponto/services/diagnostico_escala.py:154`, `app/ponto/services/vigia_entrada.py:45`, `:80`, `:123`, `app/colaboradores/services/feriado_do_posto.py:97`, `:115`, `app/ponto/services/cluster_espurio.py:29`, `app/ponto/services/vigia_pareador_celula.py:101`, `app/ponto/services/esmeril_espelho.py:136`, `app/relatorios/cartao_pela_celula.py:289`. |

As rotinas operacionais também consultam a lista em `detectar_intervalo_ausente`, `ausencias_sem_efeito_na_celula`, `curar_residuo_bug115` e `tripwire_celulas`. O contador de folgas do `tripwire` lê `n_celulas` sob um filtro que exclui `trabalha=True`. O detector de intervalo exige pontas acesas. O Cartório relê `lampadas` para detectar marco recém-vencido e derivar `tem_entrada` e `tem_saida`. `app/ponto/management/commands/detectar_intervalo_ausente.py:64`, `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:61`, `app/ponto/management/commands/curar_residuo_bug115.py:45`, `:148`, `app/ponto/management/commands/tripwire_celulas.py:264`, `:338`, `:344`, `app/ponto/services/cartorio.py:469`, `:853`.

## 19/11 15:43 · sessao bfb920870efb · DESAFIO-DECISAOHE-SOL62 · leitura · origem=Code

# DESAFIO-DECISAOHE-SOL62

Snapshot fixado em `ba4cb6c3c33403eecacbefeeef01b64c5b441a83`. [Commit no repositório 26031963/hasner-ponto](https://github.com/26031963/hasner-ponto/commit/ba4cb6c3c33403eecacbefeeef01b64c5b441a83)

## P1 — Acessos à tabela

| Arquivo:linha | Leitura ou escrita | Entrada |
|---|---|---|
| `app/ponto/management/commands/lavrar_he_pendente.py:48` | Leitura | `objects.vigentes().filter()`, iterado para montar o mapa de decisões |
| `app/ponto/management/commands/recusar_ponta_pequena.py:120` | Leitura | `objects.vigentes().filter()`, iterado para montar o conjunto de dias decididos |
| `app/ponto/motor_calculo_v2.py:1417` | Leitura | `_DHE.objects.vigentes().filter().only()`, iterado para montar o mapa do motor |
| `app/ponto/portas/he.py:148` | Leitura | `objects.vigentes().filter(pk=...).exists()` em `_vigente` |
| `app/ponto/portas/he.py:195` | Leitura | `objects.select_for_update().filter().first()` em `decidir_he` |
| `app/ponto/portas/he.py:346` | Leitura | `objects.select_for_update().filter().first()` em `recusar_ponta_pequena` |
| `app/ponto/portas/he.py:473` | Leitura | `objects.select_for_update().filter().first()` em `revogar_recusa_do_sistema` |
| `app/ponto/portas/he.py:583` | Leitura | `objects.select_for_update().filter().first()` em `autorizar_em_lote` |
| `app/ponto/services/gestao_he.py:87` | Leitura | `objects.vigentes().filter().values()` em `estado_por_dia` |
| `app/ponto/services/gestao_he.py:105` | Leitura | `objects.vigentes().filter().values().annotate()` em `_decisoes` |
| `app/ponto/portas/he.py:66` | Escrita | `objects.create()` em `_gravar` |
| `app/ponto/portas/he.py:84` | Escrita | `save(update_fields=...)` da instância em `_gravar` |
| `app/ponto/portas/he.py:403` | Escrita | `save(update_fields=...)` da instância em `_marcar_revogada` |
| `app/ponto/models.py:1311` | Escrita implícita condicional | `on_delete=SET_NULL` em `decidida_por` na exclusão de `auth.User`. O snapshot habilita o admin em `app/config/settings/saas.py:26` e `app/config/urls.py:24`. Execução em produção [nao verificado]. |

**Totais dos 13 sítios explícitos da tabela acima** — 6 leituras por `objects.vigentes()`, 4 por `objects.select_for_update()`, 1 escrita por `objects.create()` e 2 por `save()`. Há 1 caminho implícito condicional por `SET_NULL`. A varredura encontrou 0 acessos explícitos por outro manager, `related_name`, SQL cru ou `get_model`. Os reversos estão declarados em `app/ponto/models.py:1301` e `app/ponto/models.py:1311`. O `.delete()` de `app/ponto/management/commands/recusar_ponta_pequena.py:219` é texto de reversão.

## P2 — Autoridade de vigência

`DecisaoHEQuerySet.vigentes()` é a autoridade única. O predicado é `revogada_em IS NULL`, implementado por `self.filter(revogada_em__isnull=True)`. `objects` expõe o método e permite ler todas as linhas quando ele não é chamado. `app/ponto/models.py:1260`, `app/ponto/models.py:1265`, `app/ponto/models.py:1267`, `app/ponto/models.py:1348`.

## P3 — Leitores indiretos e seus chamadores

1. `estado_por_dia` entrega o estado por colaborador e dia (`app/ponto/services/gestao_he.py:63`). Chamadores em `app/ponto/services/gestao_he.py:250`, `app/ponto/services/gestao_he.py:475` e `app/folha/porta_export.py:686`.
2. `_decisoes` agrega dias e minutos por estado (`app/ponto/services/gestao_he.py:92`). Só `enriquecer` o chama (`app/ponto/services/gestao_he.py:249`). `enriquecer` é definido em `app/ponto/services/gestao_he.py:216` e chamado em `app/ponto/services/gestao_he.py:402`, `app/ponto/views.py:807` e `app/ponto/views.py:945`. `_tira` e `_calendario` repassam o estado para a grade (`app/ponto/services/gestao_he.py:114`, `app/ponto/services/gestao_he.py:162`), chamados apenas pelo próprio `enriquecer` (`app/ponto/services/gestao_he.py:303`, `app/ponto/services/gestao_he.py:304`, `app/ponto/services/gestao_he.py:346`, `app/ponto/services/gestao_he.py:347`).
3. `itens_sem_decisao` entrega os dias pendentes (`app/ponto/services/gestao_he.py:370`) a `app/ponto/views.py:594` e `app/ponto/views.py:668`. `pendentes_na_central` entrega a contagem (`app/ponto/services/gestao_he.py:432`) a `app/chamados/views.py:762` e `app/relatorios/views.py:43`.
4. `_vigente` entrega a linha vigente ou `None` (`app/ponto/portas/he.py:130`). Chamadores em `app/ponto/portas/he.py:196`, `app/ponto/portas/he.py:347`, `app/ponto/portas/he.py:481` e `app/ponto/portas/he.py:589`.
5. `decidir_he` também devolve o estado no resultado do ato (`app/ponto/portas/he.py:159`, `app/ponto/portas/he.py:203`, `app/ponto/portas/he.py:231`). Chamadores em `app/ponto/views.py:532`, `app/ponto/portas/he.py:265` e `app/ponto/portas/he.py:595`. `recusar_ponta_pequena` devolve `ja_decidido` (`app/ponto/portas/he.py:297`, `app/ponto/portas/he.py:358`) ao comando em `app/ponto/management/commands/recusar_ponta_pequena.py:170`.
6. `_carregar_decisoes_he` alimenta o mapa do motor (`app/ponto/motor_calculo_v2.py:1397`) para o bloqueio de dia sem previsto e para `_janela_do_dia` (`app/ponto/motor_calculo_v2.py:1455`, `app/ponto/motor_calculo_v2.py:1641`). `_janela_do_dia` entrega `autorizado` (`app/ponto/motor_calculo_v2.py:1490`, `app/ponto/motor_calculo_v2.py:1653`) aos leitores em `app/ponto/motor_calculo_v2.py:1691`, `app/ponto/motor_calculo_v2.py:1733` e `app/ponto/motor_calculo_v2.py:1760`.
7. `apurar` inclui o estado no retrato (`app/ponto/services/he_pendente_lavrado.py:71`, `app/ponto/services/he_pendente_lavrado.py:149`) para `app/ponto/management/commands/lavrar_he_pendente.py:62`. `ler` devolve o retrato gravado (`app/ponto/services/he_pendente_lavrado.py:193`) a `app/ponto/management/commands/recusar_ponta_pequena.py:98`, `app/ponto/services/gestao_he.py:401`, `app/ponto/services/gestao_he.py:466`, `app/ponto/views.py:804` e `app/ponto/views.py:934`.
8. `dias_acima_do_limite_sem_decisao` entrega a lista a `medir` (`app/folha/porta_export.py:199`, `app/folha/porta_export.py:684`). `medir` é chamado por `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:187`. `estado_da_porta` entrega o carimbo a `conferir` e `abrir_a_porta` (`app/folha/porta_export.py:1334`, `app/folha/porta_export.py:820`, `app/folha/porta_export.py:1382`). `conferir` devolve a mensagem a `abrir_a_porta`, que a entrega à view de exportação (`app/folha/porta_export.py:811`, `app/folha/porta_export.py:1376`, `app/folha/porta_export.py:1387`, `app/folha/views.py:131`).

## P4 — Estado exibido sem consultar a autoridade

- **Sim.** O espelho escreve “sem decisão” para cada ponta exibida. Para as pontas da grade, `montar_dias` produz a lista pela diferença entre batida e marco, e o rótulo sai fixo no template. Uma ponta com decisão `nao` pode receber esse rótulo. `app/ponto/janela_he.py:199`, `app/ponto/janela_he.py:228`, `app/ponto/services/espelho.py:727`, `app/ponto/services/espelho.py:776`, `app/templates/ponto/espelho.html:189`, `app/ponto/models.py:1283`.
- O PDF do espelho e o calendário também exibem o efeito “não conta” para pontas marcadas pela grade. `app/relatorios/pdf_espelho.py:932`, `app/relatorios/pdf_espelho.py:941`, `app/colaboradores/services/calendario.py:421`, `app/templates/colaboradores/partials/_calendario_grade.html:127`, `app/templates/core/_relogio_riscado.html:15`.
- O selo de exportação rotula a lista bruta `he_pendente` como “ponta de HE sem decisão”. A consulta de vigência alimenta a lista distinta de dias que travam o TXT. A mensagem do clique usa essa segunda lista do carimbo, com a vigência apurada na medição. `app/folha/porta_export.py:495`, `app/folha/porta_export.py:684`, `app/folha/porta_export.py:745`, `app/folha/porta_export.py:806`, `app/folha/porta_export.py:866`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:124`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:268`.
- A tela de auditoria exibe o histórico de decisão e revogação a partir das descrições de `LogAuditoria`. `app/ponto/portas/he.py:94`, `app/ponto/portas/he.py:409`, `app/core/views.py:705`, `app/templates/core/log_auditoria.html:73`.
- Busquei os usos do modelo, dos reversos, de `vigentes`, dos mapas e retratos, e dos rótulos “sem decisão”, “com ciência” e “autorizado” em Python e templates. A tela e o PDF de Gestão de HE consultam a autoridade por `enriquecer`, e os atalhos de pendências por `pendentes_na_central`. `app/ponto/views.py:807`, `app/ponto/views.py:945`, `app/ponto/services/gestao_he.py:249`, `app/ponto/services/gestao_he.py:475`, `app/chamados/views.py:762`, `app/relatorios/views.py:43`.

## P5 — Revogada lida como vigente

- **Sim, por concorrência no lavrador.** Uma execução carrega uma recusa vigente no mapa. Outra a revoga antes de `apurar` processar o dia. A primeira recebe `revogada=False` com motivo `ja revogada`, conserva o objeto antigo e pode gravar `estado='nao'` no retrato. `app/ponto/management/commands/lavrar_he_pendente.py:48`, `app/ponto/management/commands/lavrar_he_pendente.py:59`, `app/ponto/management/commands/lavrar_he_pendente.py:71`, `app/ponto/portas/he.py:481`, `app/ponto/portas/he.py:500`, `app/ponto/services/he_pendente_lavrado.py:143`, `app/ponto/services/he_pendente_lavrado.py:189`.
- O mesmo intervalo entre consulta e uso permite ao comando de ponta pequena contar o dia como “já decidido”, ou à Gestão de HE contabilizá-lo como “com ciência” no agregado enquanto o mapa por dia já o mostra “sem decisão”. `app/ponto/management/commands/recusar_ponta_pequena.py:120`, `app/ponto/management/commands/recusar_ponta_pequena.py:141`, `app/ponto/management/commands/recusar_ponta_pequena.py:189`, `app/ponto/services/gestao_he.py:249`, `app/ponto/services/gestao_he.py:278`, `app/ponto/services/gestao_he.py:315`, `app/templates/ponto/gestao_he.html:125`.
- **Também pelo carimbo do TXT.** Na medição, uma recusa vigente tira o dia da lista de bloqueio. Se ela for revogada depois de um carimbo verde, o clique ainda pode aceitar esse carimbo no mesmo dia sem reler a decisão. `app/folha/porta_export.py:232`, `app/folha/porta_export.py:686`, `app/folha/porta_export.py:806`, `app/folha/porta_export.py:1350`, `app/folha/porta_export.py:1368`, `app/folha/porta_export.py:1382`, `app/folha/views.py:131`.

**Tempo e arquivos abertos** — cerca de 5 minutos, 1.077 arquivos de texto abertos na varredura, incluindo 773 Python e 267 HTML. Métricas da execução [nao verificado] no snapshot.

**[nao verificado]** — ocorrência em produção dos cenários de P5, SQL efetivamente emitido pelo Django ao excluir um usuário e acessos por operadores ou ferramentas externas a `app/`.

## 19/11 15:41 · sessao 8a71eb7ff630 · CADEIA-20-PASSO-1 · verificador · origem=Code

**BLOQUEIA.** O teste de consultas pode passar com uma consulta por sobra no caminho que carrega a CXR sem fotografia fornecida. A lavra ainda recebe um parâmetro sem uso, e a regra de rajada perde a distância real entre duas sobras da mesma célula em lados opostos da meia-noite. `app/escala/tests/test_cadeia20_lavra.py:230–263`, `app/ponto/services/cartorio.py:959–998`, `app/ponto/services/cartorio.py:1066–1072`, `app/escala/orfa_classe.py:8–22`, `app/escala/orfa_classe.py:49–66`

Conferi os arquivos da cópia fornecida e executei em memória apenas as funções puras citadas abaixo. A identidade Git do diretório, a execução da suíte Django e o resultado dos mutantes nesta volta ficam **[nao verificado]**. [nao verificado]

## 1. P1–P5 da triagem

| Item | Veredito | Correção, mordida e limite |
|---|---|---|
| **P1 · Contagem por célula** | **NÃO BLOQUEIA** | Compara quantidade e hora por posição em cada célula com `razoes_orfas`. O booleano exige também zero células desalinhadas. O RED tem duas sobras e duas entradas no agregado, distribuídas como `2/0` entre duas células que deveriam ter `1/1`. Voltar à soma global falha. Retirar a comparação de horas também falha, pelo caso `10:00` → `11:00`. Comparar horas ordenadas ainda passaria pelos casos mostrados, que não incluem uma permutação pura. Atas sem a chave seguem contadas à parte. `app/ponto/services/cartorio.py:1006–1031`, `app/escala/tests/test_cadeia20_lavra.py:134–161`, `app/escala/tests/test_cadeia20_mutantes.py:63–71` |
| **P2 · Parâmetros mortos** | **BLOQUEIA pelo resíduo** | `data_turno` e `fuso` saíram da assinatura e dos chamadores. Restaurar só a assinatura antiga quebra as chamadas atuais. Restaurar assinatura e chamadas juntas deixa os testes funcionais passarem com os parâmetros mortos de volta. A assinatura de produção de `_lavrar_razoes_sobras` ainda recebe `out` e o corpo inteiro dispensa esse argumento. O auxiliar de teste `julgar` também reteve `dia` sem uso. `app/escala/orfa_classe.py:30–36`, `app/ponto/services/cartorio.py:959–998`, `app/escala/tests/test_cadeia20_coletor.py:12–14`, `app/escala/tests/test_razao_da_sobra.py:25–29`, `CLAUDE.md:178` |
| **P3 · Primeira lavra** | **BLOQUEIA pelo alcance do teto** | O teste recarrega colaborador e células, retira `razoes_orfas` da memória e do banco e captura a primeira chamada de cada braço sob rollback. Isso corrige a medição da segunda passada aquecida. A preparação preserva os demais efeitos da lavra disparada pelo sinal. O teste compara dois acréscimos e admite zero chamadas a CXR. Uma lavra que virasse no-op continuaria satisfazendo esse teste isolado. A costura exige a gravação em outro teste. `app/escala/tests/test_cadeia20_lavra.py:227–263`, `app/escala/tests/test_cadeia20_costura.py:37–54` |
| **P4 · Atribuição e reconciliação na virada** | **NÃO BLOQUEIA nos dois casos pedidos** | A sobra `01:30` tem de aparecer na grade e na ata de `DATA`, com o instante de `DEPOIS`, razão de intervalo e falha nula. Transferi-la exclusivamente ao dia civil falha. Na saída cruzada, tirar `07:58` do dia civil e deixar o seu instante falha na asserção que o m15 mira. Esse segundo RED ainda passaria se a reconciliação apagasse todos os pares e todas as sobras daquele dia, pois só exige ausência de `07:58` e alinhamento do restante. O primeiro RED também admite uma cópia extra no dia civil. `app/escala/tests/test_cadeia20_costura.py:117–151`, `app/escala/utils.py:2043–2052`, `app/escala/utils.py:2084–2086`, `app/escala/tests/test_cadeia20_mutantes.py:61–62` |
| **P5 · Entrada sem razão nem falha** | **NÃO BLOQUEIA para as entradas produzidas pela lavra** | O contador próprio sobe quando `razao is None` e `falha` é falsa. O RED morde `None` e `''`, e o m17 retira o incremento. A medida aceita qualquer valor verdadeiro como falha. Conferi em memória que `' '` entra em `falhas` e deixa o contador em zero. A lavra emite um conjunto fechado de rótulos. `app/ponto/services/cartorio.py:1019–1032`, `app/ponto/services/cartorio.py:994–998`, `app/escala/orfa_classe.py:3–5`, `app/escala/orfa_classe.py:56–82`, `app/escala/tests/test_cadeia20_lavra.py:163–168`, `app/escala/tests/test_cadeia20_mutantes.py:72–74` |

## 2. Quem escreve e quem lê — NÃO BLOQUEIA

No censo da cópia, os leitores identificados do retorno são os testes abaixo. O executor de mutantes substitui a função importada pelo teste. Os dois testes homônimos fora da cadeia medem rubricas e períodos. Um leitor externo da sonda fica **[nao verificado]**. `app/escala/tests/test_cadeia20_lavra.py:141–168`, `app/escala/tests/test_cadeia20_costura.py:52–58`, `app/escala/tests/test_cadeia20_mutantes.py:117–119`, `app/ponto/tests/test_o280_bateria_da_lei.py:2874–2880`, `app/ponto/tests/test_o280_bateria_da_lei.py:5463–5472`

| Chave | Escritor | Leitores e significado |
|---|---|---|
| `celulas_desalinhadas` | `app/ponto/services/cartorio.py:1003–1005`, `app/ponto/services/cartorio.py:1015–1018` | O próprio booleano lê o total. Os testes conferem zero, duas células compensadas e uma célula sem órfã com entrada excedente. `app/ponto/services/cartorio.py:1029–1031`, `app/escala/tests/test_cadeia20_lavra.py:141–161` |
| `sem_razao_sem_falha` | `app/ponto/services/cartorio.py:1003–1005`, `app/ponto/services/cartorio.py:1019–1026` | Os testes leem zero no caso nominal e um para `None` e `''`. A costura compara o dicionário inteiro entre lavras. `app/escala/tests/test_cadeia20_lavra.py:141–145`, `app/escala/tests/test_cadeia20_lavra.py:163–168`, `app/escala/tests/test_cadeia20_costura.py:52–58` |
| `contador_igual_universo` | `app/ponto/services/cartorio.py:1029–1031` | Os leitores exercitam a soma global **e** a guarda por célula. A chave pode ser verdadeira com atas legadas sem `razoes_orfas` ou com `sem_razao_sem_falha` positivo. Nenhuma das duas condições é exigida pelo booleano. `app/ponto/services/cartorio.py:1010–1031`, `app/escala/tests/test_cadeia20_lavra.py:134–158`, `app/escala/tests/test_cadeia20_costura.py:52–58` |
| `falhas` | `app/ponto/services/cartorio.py:1020–1026` | O delta conta as falhas verdadeiras entre as entradas sem razão. O teste compara o mapa inteiro, e a costura o inclui na igualdade da medida. `app/escala/tests/test_cadeia20_lavra.py:134–145`, `app/escala/tests/test_cadeia20_costura.py:52–58` |

`razao_da_sobra` perdeu dois parâmetros e ganhou zero. O chamador de produção passa `(hora, instante)`, turnos, o resultado de `dia_sem_trabalho`, pausas e a marca CXR. O classificador consome os cinco. As chamadas diretas de teste estão no coletor e no auxiliar `julgar`, e o mutante substitui a função no módulo. A data da célula indexa os insumos e a CXR. O instante identifica a batida, inclusive em outra data civil. Não encontrei leitor da assinatura que troque essas duas referências. `app/ponto/services/cartorio.py:974–993`, `app/escala/orfa_classe.py:30–81`, `app/escala/tests/test_cadeia20_coletor.py:7–14`, `app/escala/tests/test_razao_da_sobra.py:25–29`, `app/escala/tests/test_cadeia20_mutantes.py:115–116`

## 3. Teto e meia-noite — BLOQUEIA pelo teto

- **Consultas.** Um `SELECT` adicional em cada célula exercitada faz o acréscimo crescer de 2 para 4, mantido o restante do caminho. A brecha demonstrável é `lavra=None`: o teste sempre passa o dicionário devolvido por `ultimo_lavrado()`, inclusive quando não há medida. Uma regressão que voltasse a buscar esse snapshot a cada sobra somente quando `lavra=None` passaria pelo teste. `julgar_celula` expõe justamente esse caminho por padrão. O código atual memoíza a carga. A captura ainda conta só SQL que começa por `SELECT`. `app/escala/tests/test_cadeia20_lavra.py:230–263`, `app/ponto/services/cxr_na_lista.py:152–162`, `app/ponto/services/cartorio.py:1066–1072`, `app/ponto/services/cartorio.py:2144–2149`, `app/ponto/services/cartorio.py:2173–2175`
- **Meia-noite.** O caso com três flags prova a atribuição positiva ao turno de `DATA`. A grade encadeia os marcos, inclui as flags internas por instante e põe o excedente dos dois slots `I` em `orfas` e `instantes` do mesmo dia de montagem. O teste recarrega a célula de `DATA` e exige encontrar ali a razão da sobra `01:30`. Ele deixa em aberto a ausência de duplicata em `DEPOIS` e não testa qual data foi usada para a marca CXR. `app/escala/utils.py:1410–1411`, `app/escala/utils.py:1640–1641`, `app/escala/utils.py:1662–1666`, `app/escala/utils.py:1720–1727`, `app/escala/utils.py:1752–1759`, `app/escala/utils.py:1783–1785`, `app/escala/tests/test_cadeia20_costura.py:27–35`, `app/escala/tests/test_cadeia20_costura.py:117–132`

## 4. Classes de reprovação — BLOQUEIA

- **Segundo juiz registrado e segundo escritor.** A lavra pergunta ao juiz de dia sem trabalho e usa o resultado do pareador e da grade. A reconciliação consome o par e remove a ocorrência pelo mesmo índice. Os dois ramos acrescentam a lista em memória e persistem pela mesma porta de célula, com `lavrar_veredito` seguido de `carimbar`. Não encontrei duplicação dessas autoridades ou outra escrita de `razoes_orfas` em produção. `app/core/juizes.py:215–224`, `app/chamados/juizes.py:445–460`, `app/ponto/services/cartorio.py:974–998`, `app/escala/utils.py:1935–1936`, `app/escala/utils.py:2043–2052`, `app/escala/utils.py:2084–2086`, `app/ponto/services/cartorio.py:1404–1429`, `app/ponto/services/cartorio.py:1982–2007`, `app/ponto/portas/celula.py:1181–1202`
- **Derivação paralela e campo com dois usos.** A proximidade de órfãs por minutos de `HH:MM` é calculada no classificador e também no serviço de cluster, que exige ainda lâmpada apagada. Na razão nova, `hora` serve de rótulo e de base para distância, mesmo com o instante disponível. Conferi em memória que `23:55` e `00:05` formam dois grupos unitários. Se forem as duas sobras ligadas na mesma ata, a regra deixa de atribuir `BATIDA_REPETIDA` a batidas separadas por dez minutos. O RED noturno tem só uma sobra-alvo e pode passar com esse defeito. `app/escala/orfa_classe.py:8–22`, `app/escala/orfa_classe.py:49–68`, `app/ponto/services/cluster_espurio.py:25–36`, `app/escala/tests/test_cadeia20_costura.py:117–132`
- **Relógio e testes.** A razão nova não chama relógio. No caminho avulso já existe `localdate()` independente de `agora`, além de `now()` na guarda de hoje. No corpo do cartório, o gate de afastamento chama sem data e chega a `date.today()`. São leituras fora do instante fornecido ao ato, nos caminhos que cercam a lavra. Os REDs ORM de costura e consultas congelam o relógio e passam `agora` fixo. `app/escala/orfa_classe.py:30–82`, `app/ponto/services/cartorio.py:2144–2175`, `app/ponto/services/cartorio.py:1461–1465`, `app/ponto/services/afastado_avisa.py:20`, `app/ponto/services/afastado_avisa.py:38–43`, `app/ponto/turnos.py:2741–2748`, `app/escala/tests/test_cadeia20_costura.py:20–34`, `app/escala/tests/test_cadeia20_lavra.py:226–254`
- **Ordem de trava.** A carga nova da CXR faz leitura simples. A persistência segue pelo `atomic` da porta, que salva a célula antes de emitir o evento. Não identifiquei nova aquisição explícita de trava nesse trajeto. O teto sem mordida para `lavra=None` permanece o achado do item 3. `app/ponto/services/cxr_na_lista.py:152–162`, `app/ponto/services/cartorio.py:1066–1072`, `app/ponto/services/cartorio.py:1985–2007`, `app/ponto/portas/celula.py:957–965`

## 5. Segunda relavra — NÃO BLOQUEIA por atribuição das 19 ao diff

O dado fornecido registra **19 células em cada árvore**. A identidade das células, as chaves que mudaram, os instantes dos atos e os snapshots escolhidos estão **[nao verificado]**.

- **Relógio e CXR.** `razoes_orfas` e `falha` são calculadas a partir dos insumos recebidos. A marca `escala_nao_descreve` lê o snapshot mais recente quando o chamador não fixa `lavra`, e pode mudar quando muda a publicação. O comando fixa um snapshot por execução. Se “insumos” inclui o campo persistido `insumos_em`, ele recebe explicitamente `agora` quando `carimbar` grava. `app/escala/orfa_classe.py:38–82`, `app/ponto/services/cartorio.py:988–998`, `app/ponto/services/cartorio.py:1066–1072`, `app/ponto/services/cxr_na_lista.py:152–162`, `app/ponto/services/cxr_na_lista.py:174–185`, `app/ponto/management/commands/processar_cartorio.py:59–67`, `app/ponto/portas/celula.py:1189–1201`
- **Ordem.** A lista gravada segue a ordem das sobras, e a grade ordena os pares antes de projetar as horas. Se houver mais de um turno candidato com o mesmo `pk` de batida, o classificador usa o primeiro turno encontrado. O coletor percorre segmentos por data e ordena os turnos por `data_turno`. A seleção de snapshot tem ordenação por `data_ref` e `calculado_em`, sem terceiro desempate. A ocorrência desses empates na amostra está **[nao verificado]**. `app/escala/utils.py:1867–1871`, `app/escala/orfa_classe.py:38–47`, `app/escala/orfa_classe.py:55–81`, `app/ponto/turnos.py:1961–1977`, `app/ponto/turnos.py:1997–2006`, `app/ponto/services/cxr_na_lista.py:152–156`
- **Estado criado pela primeira relavra.** Há dependência indireta concreta. A pré-passagem da grade entrega ao pareador células carregadas do banco com a ata anterior. Para o ramo com intervalo cadastrado, o turno obtido pode alimentar `_match_marcos`. Na passagem final, o pareador recebe lâmpadas e órfãs da montagem atual dentro da janela e a ata do banco no pad. Os turnos e instantes daí saem para a razão e a falha da próxima lavra. A lista anterior de `razoes_orfas` não é usada como entrada desse cálculo. Isso demonstra o caminho pelo qual uma escrita anterior pode afetar a próxima, com a ligação às 19 células **[nao verificado]**. `app/escala/utils.py:1327–1332`, `app/escala/utils.py:1598–1618`, `app/escala/utils.py:1723–1727`, `app/escala/utils.py:1917–1936`, `app/escala/utils.py:2117–2118`, `app/ponto/turnos.py:1940–1944`, `app/ponto/services/cartorio.py:959–998`

## 19/11 15:39 · sessao 94fb24c17692 · CADEIA-20-PASSO-1 · sonda · origem=Code

```python
# Base: cadeia20_v2_dinheiro.py, anexo fornecido na conversa.
# As 19 células, as chaves alteradas e o rollback em execução estão [nao verificado].
#
# Na cópia, --sombra usa a role leitor com grants de SELECT
# (bin/sonda_leitura.sh:94, :128, :178, :236). A relavra com apply_=True
# chama a porta de gravação (app/ponto/services/cartorio.py:2006,
# app/ponto/portas/celula.py:963). O envelope da cópia impede essa gravação.

import contextlib
import hashlib
import inspect
import io
import json
import logging
import sys
from datetime import date, datetime
from zoneinfo import ZoneInfo

from django.db import transaction
from django.utils import timezone
from freezegun import freeze_time

from colaboradores.models import Colaborador
from escala.models import CelulaDia
from ponto.management.commands.detectar_ausencias import Command as Emissor
from ponto.models import Ausencia
from ponto.precedencia import PRECEDENCIA, ROTULOS
from ponto.services import cartorio
from ponto.services.cxr_na_lista import ultimo_lavrado


INI = date(2026, 10, 21)
INI_LAVRA = date(2026, 10, 14)
FIM = date(2026, 11, 20)
HOJE = FIM
FUSO = ZoneInfo("America/Sao_Paulo")
AGORA = datetime(2026, 11, 20, 23, 59, tzinfo=FUSO)
AUSENTE = object()

# Vocabulário da ata e da precedência.
# app/ponto/services/cartorio.py:757, :786, :965, :2133
# app/ponto/precedencia.py:46, :66, :391
# app/ponto/turnos.py:2650
# app/ponto/models.py:661
CHAVES_CONHECIDAS = {
    (): {
        "tipo_dia", "lampadas", "minutos_previstos", "minutos_realizados",
        "sem_ancora", "n_orfas", "orfas", "n_missing", "n_celulas",
        "cobertura_parcial", "periodo_parcial", "razoes_orfas", "precedencia",
    },
    ("precedencia",): {
        "tipo_do_dia", "faixa", "rotulo", "suprimidos", "era_dia_previsto", "texto",
    },
}
TEXTOS_SEGUROS = {
    ("tipo_dia",): {"trabalho", "folga", "ausencia", "falta"},
    ("periodo_parcial",): {"sem_periodo", "dentro", "fora", "nao_sei"},
    ("precedencia", "tipo_do_dia"): set(PRECEDENCIA) | {
        codigo for codigo, _ in Ausencia.TIPO_CHOICES
    },
    ("precedencia", "faixa"): set(PRECEDENCIA),
    ("precedencia", "rotulo"): set(ROTULOS.values()) | {"Sem dados"},
}
CAMPOS_NUMERICOS = {
    ("minutos_previstos",), ("minutos_realizados",),
    ("n_orfas",), ("n_missing",), ("n_celulas",),
}
ORIGENS = {codigo for codigo, _ in CelulaDia.ORIGEM_CHOICES}


class DesfazerSonda(Exception):
    pass


def json_canonico(valor):
    return json.dumps(
        valor, sort_keys=True, ensure_ascii=True, separators=(",", ":"), default=str
    )


def selo_celulas(queryset):
    campos = [campo.attname for campo in CelulaDia._meta.concrete_fields]
    selo = hashlib.sha256()
    for linha in queryset.order_by("pk").values_list(*campos).iterator():
        selo.update(json_canonico(linha).encode("ascii"))
        selo.update(b"\n")
    return selo.digest()


def fotografia(queryset):
    return {
        pk: (cid, dia, trabalha, origem, ata)
        for pk, cid, dia, trabalha, origem, ata in queryset.order_by().values_list(
            "pk", "colaborador_id", "data", "trabalha", "origem", "ata"
        ).iterator()
    }


def tipo(valor):
    return "ausente" if valor is AUSENTE else type(valor).__name__


def escalar_publicavel(caminho, valor):
    if valor is AUSENTE:
        return "<ausente>"
    if valor is None or type(valor) is bool:
        return json_canonico(valor)
    if type(valor) in (int, float) and caminho in CAMPOS_NUMERICOS:
        return json_canonico(valor)
    if isinstance(valor, str) and len(valor) <= 40:
        if valor == "" or valor in TEXTOS_SEGUROS.get(caminho, ()):
            return json_canonico(valor)
    return "<omitido>"


def detalhe(rotulo, caminho, antes, depois):
    texto = f"{rotulo}:{tipo(antes)}->{tipo(depois)}"
    if isinstance(antes, list) or isinstance(depois, list):
        tamanho1 = len(antes) if isinstance(antes, list) else "-"
        tamanho2 = len(depois) if isinstance(depois, list) else "-"
        texto += f" len={tamanho1}->{tamanho2}"
    if any(type(valor) in (str, int, float, bool) for valor in (antes, depois)):
        valor1 = escalar_publicavel(caminho, antes)
        valor2 = escalar_publicavel(caminho, depois)
        texto += f" valor={valor1}->{valor2}"
    return texto


def chaves_diferentes(antes, depois, prefixo, aliases):
    esquerda = antes if isinstance(antes, dict) else {}
    direita = depois if isinstance(depois, dict) else {}
    mudancas = []
    for chave in sorted(esquerda.keys() | direita.keys()):
        valor1 = esquerda.get(chave, AUSENTE)
        valor2 = direita.get(chave, AUSENTE)
        if valor1 == valor2:
            continue
        caminho = prefixo + (chave,)
        if chave in CHAVES_CONHECIDAS.get(prefixo, ()):
            rotulo = chave
        else:
            rotulo = aliases.setdefault(caminho, f"<chave#{len(aliases) + 1}>")
        mudancas.append((rotulo, caminho, valor1, valor2))
    return sorted(mudancas, key=lambda mudanca: mudanca[0])


def linhas_idemp(primeira, segunda):
    linhas = []
    aliases = {}

    # Igualdade da ata conforme o anexo. Chaves desconhecidas recebem aliases
    # por execução, e valores escalares fora do vocabulário ficam ocultos.
    for pk in sorted(primeira.keys() | segunda.keys()):
        linha1 = primeira.get(pk)
        linha2 = segunda.get(pk)
        ata1 = linha1[-1] if linha1 is not None else AUSENTE
        ata2 = linha2[-1] if linha2 is not None else AUSENTE
        if ata1 == ata2:
            continue

        # trabalha admite None (app/escala/models.py:1686).
        cid, dia, trabalha, origem, _ = linha2 if linha2 is not None else linha1
        origem = origem if origem in ORIGENS else "<omitida>"
        mudancas = chaves_diferentes(ata1, ata2, (), aliases)
        chaves = [rotulo for rotulo, _, _, _ in mudancas]
        partes = [
            f"IDEMPDIFF colab_pk={cid} cel_pk={pk} data={dia.isoformat()}"
            f" trabalha={trabalha} origem={origem} chaves={json_canonico(chaves)}"
        ]
        if not (isinstance(ata1, dict) and isinstance(ata2, dict)):
            partes.append(detalhe("ata", (), ata1, ata2))

        for rotulo, caminho, valor1, valor2 in mudancas:
            partes.append(detalhe(rotulo, caminho, valor1, valor2))
            for sub, caminho_sub, sub1, sub2 in chaves_diferentes(
                valor1, valor2, caminho, aliases
            ):
                partes.append(detalhe(f"{rotulo}.{sub}", caminho_sub, sub1, sub2))
        linhas.append(" ".join(partes))

    return linhas


def relavrar_rodada(ids, lavra, aceita_lavra, etapa, erros):
    concluidos = []
    for cid in ids:
        try:
            with transaction.atomic():
                colab = Colaborador.objects.get(pk=cid)
                celulas = list(
                    CelulaDia.objects.filter(
                        colaborador=colab, data__gte=INI_LAVRA, data__lt=HOJE
                    ).order_by("data", "pk")
                )
                if celulas:
                    kwargs = dict(
                        hoje=HOJE,
                        agora=AGORA,
                        apply_=True,
                        emissor=Emissor(),
                        out=cartorio.contadores(),
                        prefix="",
                        write=lambda mensagem: None,
                        forcar=True,
                    )
                    if aceita_lavra:
                        kwargs["lavra"] = lavra
                    cartorio.julgar_colab(colab, celulas, **kwargs)
            concluidos.append(cid)
        except Exception as exc:
            erros.append((etapa, cid, type(exc).__name__))
    return concluidos


def main():
    erros = []
    linhas = []
    agora_1a = agora_2a = None
    desfeito = False
    sentinela = DesfazerSonda()
    nivel_anterior = logging.root.manager.disable

    # Os handlers podem apontar para streams já abertos
    # (app/config/settings/saas.py:131, :155). A saída da sonda usa só
    # os campos filtrados acima e as classes das exceções interceptadas.
    try:
        logging.disable(sys.maxsize)
        with (
            freeze_time(AGORA),
            timezone.override(FUSO),
            contextlib.redirect_stdout(io.StringIO()),
            contextlib.redirect_stderr(io.StringIO()),
        ):
            try:
                if not transaction.get_autocommit():
                    raise RuntimeError("A sonda precisa do atomic externo.")

                with transaction.atomic():
                    recorte = CelulaDia.objects.filter(
                        colaborador__empresa_id__in=(2, 3, 4),
                        data__gte=INI_LAVRA,
                        data__lte=FIM,
                    )
                    universo = recorte.filter(data__gte=INI)
                    ids = list(
                        universo.order_by()
                        .values_list("colaborador_id", flat=True)
                        .distinct()
                        .order_by("colaborador_id")
                    )
                    estado_original = selo_celulas(recorte)
                    lavra = ultimo_lavrado() if ids else None
                    aceita_lavra = (
                        "lavra" in inspect.signature(cartorio.julgar_colab).parameters
                    )

                    # A sonda mantém as chamadas de relavra e omite as medições
                    # do espelho e das razões (app/ponto/services/espelho.py:1243,
                    # app/ponto/services/cartorio.py:1002, :1035).
                    agora_1a = timezone.localtime()
                    concluidos = relavrar_rodada(ids, lavra, aceita_lavra, "1a", erros)
                    primeira = fotografia(universo)

                    agora_2a = timezone.localtime()
                    relavrar_rodada(concluidos, lavra, aceita_lavra, "2a", erros)
                    segunda = fotografia(universo)

                    linhas = linhas_idemp(primeira, segunda)
                    raise sentinela
            except Exception as exc:
                if exc is sentinela:
                    # DESFEITO exige a saída pelo sentinela e a restauração
                    # de todas as colunas das células no recorte de lavra.
                    # Efeitos externos à transação [nao verificado].
                    try:
                        desfeito = (
                            transaction.get_autocommit()
                            and selo_celulas(recorte) == estado_original
                        )
                    except Exception as erro:
                        erros.append(("verificar_desfeito", 0, type(erro).__name__))
                else:
                    erros.append(("sonda", 0, type(exc).__name__))
    finally:
        logging.disable(nivel_anterior)

    for linha in linhas:
        print(linha)
    print(f"IDEMP celulas_que_mudaram_na_2a={len(linhas)}")
    for etapa, cid, classe in erros:
        print(f"ERRO etapa={etapa} colab_pk={cid} tipo={classe}")
    print(f"ERROS={len(erros)}")
    print(f"AGORA_1a={agora_1a.isoformat() if agora_1a is not None else None}")
    print(f"AGORA_2a={agora_2a.isoformat() if agora_2a is not None else None}")
    print(f"DESFEITO={desfeito}")
    if erros or not desfeito:
        print("NAO_CONCLUIDO")


main()
```

**Relógio.** O encerramento do turno compara o fim previsto com `agora`. A sonda congela o relógio e passa o mesmo instante às duas rodadas. [app/ponto/services/cartorio.py:1266](/home/ronald/codex_ws/d1de46305d7d393fe3349113ad5391e1d9302f7c/app/ponto/services/cartorio.py:1266) · [app/ponto/turnos.py:2134](/home/ronald/codex_ws/d1de46305d7d393fe3349113ad5391e1d9302f7c/app/ponto/turnos.py:2134)

**Chamado criado na 1ª.** Um chamado gravado na 1ª rodada pode entrar na lista que a 2ª relê e alterar as contagens de chamados e cobranças usadas na classificação. [app/chamados/models.py:1030](/home/ronald/codex_ws/d1de46305d7d393fe3349113ad5391e1d9302f7c/app/chamados/models.py:1030) · [app/ponto/services/cartorio.py:1432](/home/ronald/codex_ws/d1de46305d7d393fe3349113ad5391e1d9302f7c/app/ponto/services/cartorio.py:1432) · [app/ponto/services/cartorio.py:1584](/home/ronald/codex_ws/d1de46305d7d393fe3349113ad5391e1d9302f7c/app/ponto/services/cartorio.py:1584)

**Ordem.** As pontas da lista de células definem a janela. Dentro dela, batidas com o mesmo `timestamp` podem chegar em outra ordem à grade, pois a consulta não desempata por PK. A ocorrência desses empates nas 19 células está [nao verificado]. [app/ponto/services/cartorio.py:1243](/home/ronald/codex_ws/d1de46305d7d393fe3349113ad5391e1d9302f7c/app/ponto/services/cartorio.py:1243) · [app/ponto/services/cartorio.py:1287](/home/ronald/codex_ws/d1de46305d7d393fe3349113ad5391e1d9302f7c/app/ponto/services/cartorio.py:1287) · [app/ponto/turnos.py:2903](/home/ronald/codex_ws/d1de46305d7d393fe3349113ad5391e1d9302f7c/app/ponto/turnos.py:2903)

## 19/11 15:31 · sessao 4302f071431b · E4C-F5 · leitura · origem=Code

A consulta por pergunta vem de `list(p.decisoes.all())` quando a instância ORM chega sem `decisoes` pré-carregado. O cálculo do passivo E4b dentro de `DecisaoPergunta.leitura` recebe `ja` e usa as linhas que essa leitura já trouxe. O anexo permite confrontar os totais dos sete testes com esses caminhos. A atribuição de cada SQL capturado a um ponto de chamada fica **[nao verificado]**. [chamados/models.py:4254](/home/ronald/codex_ws/e673e0efe1135d60e844387527d53042d651a34d/app/chamados/models.py:4254) [chamados/models.py:4308](/home/ronald/codex_ws/e673e0efe1135d60e844387527d53042d651a34d/app/chamados/models.py:4308)

Os caminhos abaixo são relativos a `app/`. Os dois `isinstance` estão nas linhas indicadas na cópia. A confirmação do hash por metadados Git ficou [nao verificado]. `chamados/juizes.py:1389,1559`.

## 1. Chamadores

### Produção — `projecao_temporal_da_pergunta`

| Chamador | Objeto e laço | Origem de `decisoes` |
|---|---|---|
| `_slot_esperado` | `p` recebido. Uma chamada por execução com `agora`, inclusive quando `agora` vem da carga. A montagem dos slots chama o auxiliar em laço. O caminho de materialização de fato também pode chamá-lo. `chamados/juizes.py:345–355,756`, `chamados/services/carga_pergunta_viva.py:117–119`. | Depende da origem do ORM. A carga da tarefa passa instâncias sem prefetch. Fio e consulta do app têm prefetch nos seus universos. `chamados/services/tarefa_do_dia.py:530–536,625`, `chamados/services/fio.py:262–269`, `chamados/services/pergunta_no_app.py:52–65`. |
| `_pergunta_viva_calc` | `p` recebido, normalmente ORM em produção. Uma chamada por cálculo do C6, inclusive quando o C6 é usado em laço. O memo avulso de `pergunta_viva` pode evitar o cálculo. `chamados/juizes.py:886–898,1411,1526`. | Misto. Fio, app e cobrança têm prefetch. A tarefa, `alimentar_portas` e a releitura de `desfazer_carimbo_sem_lastro` chegam sem ele. `chamados/services/fio.py:262–275`, `chamados/services/pergunta_no_app.py:52–65`, `chamados/services/cobranca.py:236–242`, `chamados/services/tarefa_do_dia.py:530–536,627–632`, `chamados/juizes.py:1668–1673`, `chamados/services/relavra_via.py:236–242`. |
| `beco_em_lote`, verificação M1 | ORM de `alim['vivas']`, em laço de dias e `all` sobre as perguntas de cada dia. Só chama a projeção para `a is None`. `escala/services/cadastro_realidade.py:459–474,497–502`. | Sem prefetch no queryset de `alimentar_portas`. `chamados/juizes.py:1668–1673`. |

### Produção — `pergunta_respondida`

| Chamador | Objeto e laço | Origem de `decisoes` |
|---|---|---|
| `_pergunta_viva_calc` | `SimpleNamespace` com instante e texto já projetados. Uma chamada por execução que chega ao degrau da resposta. `chamados/juizes.py:1411–1430`. | Não se aplica ao objeto passado. |
| `DisputaSupervisao.sem_veredito_entre` | `LeituraDoAto`, em compreensão sobre várias perguntas. `chamados/models.py:1999–2007`. | `prefetch_related_objects(..., 'decisoes')` sobre as instâncias ORM com PK antes de montar as leituras. `chamados/models.py:1998–2004`. |
| `PerguntaDisputa.estado` e `PerguntaDisputa.respondida` | `LeituraDoAto`, uma pergunta por acesso. `estado` tem dois locais de chamada em ramos distintos. `chamados/models.py:3233–3244,3785–3791`. | Herdado de `self`. A leitura é feita antes de passar o objeto ao juiz. `chamados/models.py:3235,3791,4308`. |
| `raiox.creditos_perguntas` | `SimpleNamespace` montado de duas posições de um `values_list`, em laço. `chamados/services/raiox.py:135–147`. | Não se aplica ao objeto passado. |
| Web `responder_pergunta_disputa` e API `api_disputa_responder` | ORM avulso em cada porta. `chamados/views.py:1274–1275`, `api/views.py:2639–2640`. | Sem prefetch nas buscas da pergunta. `chamados/views.py:1239`, `api/views.py:2631–2634`. |
| API `api_disputa_perguntas` | ORM em compreensão sobre o histórico de `consulta_do_app`. `api/views.py:2553,2584–2596`. | Sim, via `perguntas_do_app`. `chamados/services/pergunta_no_app.py:52–77`. |
| `ficha_do_colab` | ORM em compreensão sobre as perguntas. `colaboradores/services/ficha.py:166–172`. | Sim, via `perguntas_do_app`. `chamados/services/pergunta_no_app.py:52–65`. |
| `particao_do_app` | ORM em `for p in vivas`. `chamados/services/pergunta_no_app.py:80–90`. | Sim via `consulta_do_app`. Sem prefetch via `_ler_vida` da tarefa, que passa suas próprias instâncias. `chamados/services/pergunta_no_app.py:52–77`, `chamados/services/tarefa_do_dia.py:530–536,620–621`. |
| `processar_post_fio`, `acao_regularizar_disputa` e `validar_inline` | ORM em `any` ou compreensão sobre `_vivas_de(d)`. `validar_inline` tem duas compreensões. `chamados/services/fio.py:710–712`, `chamados/services/acoes_disputa.py:132–134`, `chamados/views_cobrar.py:169–178`. | Sem prefetch de decisões em `disputa.perguntas.order_by('pk')`. `chamados/juizes.py:1512–1514`. |
| `becos.classificar` | ORM em compreensão sobre `d.perguntas.all()`. `chamados/services/becos.py:25–34`. | A entrada por evento carrega `perguntas`, apenas. `chamados/services/becos.py:79–87`. |
| `canonica_do_grupo` e `merito_perdedor` | ORM em compreensão sobre o grupo ou as perdedoras. `chamados/services/fio.py:1278–1300`. | Herdado da coleção recebida. As cargas do fato, da retratação e do comando de dedup citadas aqui chegam sem prefetch de decisões. `chamados/services/fio.py:1952–1963,1999–2001,2073–2088,2319–2326`, `chamados/management/commands/dedup_perguntas_fato.py:74–82`. |
| `declarar_hora_colab` e `aplicar_declaracao` | ORM avulso. A porta reutiliza a candidata do fato ou recebe `perg` de `PerguntaDisputa.abrir`. `chamados/services/declaracao_texto.py:364–378,418,663–668,688`. | Sem prefetch nessas origens. `chamados/services/declaracao_texto.py:250–255`, `chamados/services/fio.py:1952–1963`, `chamados/models.py:3223`. |
| `reabrir_credor_de_pergunta_viva` | ORM avulso dentro da função. O resolver S143 chama a função em laço de ancoradas. `chamados/services/disputa_emissao.py:1853,1983–1990`. | Sem prefetch de decisões em `_ancoradas()`. O lote de backfill também carrega suas perguntas sem ele. `chamados/models.py:355–359`, `chamados/management/commands/backfill_fio_morto_e4b.py:86–90,133–135,175`. |
| `_resolver_perguntas_por_chamado_resolvido` | ORM em laço do S128. `chamados/services/disputa_emissao.py:1998–2023`. | Sem prefetch de decisões na carga de pendentes. `chamados/services/disputa_emissao.py:1949–1953`. |
| `validar_pergunta` e `recusar_pergunta` | ORM avulso em cada porta. `chamados/services/validacao.py:682,902`. | Sem prefetch nas releituras. `chamados/services/validacao.py:677,894–895`. |
| `par_do_intervalo` | ORM recebido, uma pergunta por chamada. Pode vir da tela, da validação ou do laço de materialização. `chamados/services/materializacao.py:175–192,923,1732,2104–2125`, `chamados/services/validacao.py:632`. | Misto. A tela pode usar a instância pré-carregada pelo fio. A validação e o laço de materialização usam instâncias sem prefetch. `chamados/services/fio.py:262–269,379–381`, `chamados/services/validacao.py:677`, `chamados/services/materializacao.py:2104–2125`. |
| `paridade_absolvida_bug_par_103` | ORM recebido, uma por chamada. Usado no laço do contador e na porta avulsa. `chamados/services/materializacao.py:250–262,328–330`, `chamados/services/validacao.py:333–334`. | Sem prefetch nessas cargas. `chamados/services/materializacao.py:324–325`, `chamados/services/validacao.py:333–334`. |
| `_situacao_da_irma` | ORM recém-buscado para a irmã, uma por chamada dentro do laço do contador. `chamados/services/materializacao.py:281–291,328–342`. | Sem prefetch na busca da irmã. `chamados/services/materializacao.py:284–285`. |
| `estado_aplicacao_pergunta` | ORM recebido. Uma por chamada, inclusive no laço do fio e no do contador. `chamados/services/materializacao.py:865–885,328–342`, `chamados/services/fio.py:360,379–381`. | Misto. Sim nas instâncias do fio. Sem prefetch nas do contador. `chamados/services/fio.py:262–269`, `chamados/services/materializacao.py:324–325`. |
| `_recusa_sem_anuencia` | ORM recebido, uma pergunta por chamada. As portas de materialização a chamam dentro do processamento por pergunta. `chamados/services/materializacao.py:1282–1300,1355,1590,1674,2104–2125`. | O laço da disputa relê a pergunta sem prefetch de decisões. `chamados/services/materializacao.py:2104–2125`. |
| `lavrar_sem_turno_aberto` e `veredito_orfa` | ORM recebido, uma por chamada. Há laços externos de reconciliação e de órfãs. `chamados/reconciliador.py:366,1130,1182–1183,1238–1240,1350–1351,1568–1574,1667`. | As cargas desses lotes chegam sem prefetch. `chamados/reconciliador.py:1094–1101,1547–1554`. |
| `vigia_fio.censo_alcance` | ORM em `any` dentro do laço por dia, sobre `alim['vivas']`. `chamados/services/vigia_fio.py:560–566,595–597,636`. | Sem prefetch em `alimentar_portas`. `chamados/juizes.py:1668–1673`. |
| Comando `dedup_perguntas_fato` | ORM em compreensão por grupo. `chamados/management/commands/dedup_perguntas_fato.py:74–77`. | Sem prefetch na carga do grupo. `chamados/management/commands/dedup_perguntas_fato.py:74–75`. |
| Comando `auditar_invariantes_chamados`, I21a, I2 e F1 | ORM em duas compreensões e no `any` sobre as perguntas do chamado, respectivamente. `chamados/management/commands/auditar_invariantes_chamados.py:178,597,1671`. | Sem prefetch de decisões nos três. F1 pré-carrega `perguntas_que_resolvem`. `chamados/management/commands/auditar_invariantes_chamados.py:590–597,1585–1598,1642–1645,1666–1671`. |
| `folha.previa._material` | ORM em `any`, chamado no laço por dia e lâmpada. `folha/previa.py:77–95,139–145`. | Sem prefetch na carga que agrupa as perguntas por dia. `folha/previa.py:63–67`. |
| Cartório, `_tem_resposta_do_colab` | ORM em `any`, no laço de chamados do dia e no dedup. `ponto/services/cartorio.py:1684–1697,1709–1714`. | A carga pré-carrega `perguntas_que_resolvem`, sem o nível de decisões. `ponto/services/cartorio.py:146–147`. |

### Testes — pontos próprios de chamada e entradas recompiladas [nao verificado]

| Chamador | Juiz, objeto, laço e prefetch |
|---|---|
| `test_e4c_f5_volta2_juizes` RED 1–6 | Projeção com ORM em `:41,49,56,64,72,74,82–85,88` e respondida com ORM em `:65,75,97,99,101,104`. São perguntas avulsas sem prefetch, criadas pela fixture ou atualizadas e recarregadas. O `for` de `:66–72` varia `agora` na mesma pergunta. `chamados/tests/test_e4c_f5_volta2_juizes.py:32–104`, `chamados/tests/cortes_f2_f9_apoio.py:58–64`. |
| `test_e4c_f5_volta2_juizes` RED 7 | Ambos recebem `SimpleNamespace` em laço de casos. Sem queryset. O teste troca `D.leitura` por um mock que levanta se for chamado. `chamados/tests/test_e4c_f5_volta2_juizes.py:106–116`. |
| `test_e4c_f5_volta2_juizes` RED 8 | `medir_calc` passa ORM com `.prefetch_related('decisoes')` ao C6 em laço e verifica que `pergunta_respondida` recebe `SimpleNamespace`. `medir_fio` observa as instâncias ORM do fio e exige o cache em todas as chamadas observadas a `leitura`. Os dois erros do anexo correspondem à fixture inicial que tenta gravar texto sem instante antes de inserir o ato. `chamados/tests/test_e4c_f5_volta2_juizes.py:127–149,151–180`, `chamados/models.py:3680,3712`, `chamados/services/fio.py:1718–1721`. |
| `test_f1nova_b1_juiz_respondida` | Respondida recebe `SimpleNamespace` em laço de pares em `:41`, ORM de `.all()` sem prefetch em compreensão em `:79`, e ORM avulso recarregado sem prefetch em `:279`. `chamados/tests/test_f1nova_b1_juiz_respondida.py:28–41,74–81,270–279`. |
| `test_f1nova_b2_leitores` | Respondida recebe ORM de fixtures em laço em `:84` e ORM de `.all()` sem prefetch em compreensão em `:145`. O teste de delegação em `:93–97` observa `LeituraDoAto` entregue pela property ao mock. `chamados/tests/test_f1nova_b2_leitores.py:75–97,136–145`. |
| `test_e4c_f2_leitura_do_ato` | Respondida recebe `D.leitura(q)`, um `LeituraDoAto` avulso. `q` vem de `.get()` sem prefetch. `chamados/tests/test_e4c_f2_leitura_do_ato.py:416–425`. |
| `test_e3_c8c_sem_banco`, foto temporal | Alias `projecao`, obtido do ambiente AST. Recebe a mesma pergunta `N` em três horizontes, sem queryset. `chamados/tests/test_e3_c8c_sem_banco.py:124–140`, `chamados/tests/e3_c8b_apoio.py:194–198`. |
| `test_e4b_classe_e_memoria` | O callback de irmãs chama `self.pergunta_respondida(self.p)` uma vez por execução. O alias `projetar` recebe o mesmo `Objeto` em chamadas avulsas e num laço de vias. A relação é `Linhas` em memória. `chamados/tests/test_e4b_classe_e_memoria.py:282–287,311–328,377–392`. |
| Bug Par, Degrau Dono e Porta, C8c posição | `juiz_respondida()` instala o corpo real sob o nome importado pela produção. Bug Par passa `NS` avulso pela guarda de anuência. Degrau e C8c passam `N` pelo laço de `particao_do_app`. Sem prefetch ORM. `chamados/tests/e4_apoio.py:19–23`, `chamados/tests/test_bug_par_sem_banco.py:164–195`, `chamados/services/materializacao.py:1299–1300,1355`, `chamados/tests/test_e3_degrau_dono_e_porta.py:290–346`, `chamados/tests/test_e3_c8c_sem_banco.py:41–64`, `chamados/services/pergunta_no_app.py:83–86`. |
| C11 sem banco, C8b, pós-suite e tarefa admin | Os harnesses instalam os corpos reais para os pontos de produção da tabela. C11 passa `N` no laço M1. C8b, pós-suite e tarefa admin passam `N` pelos caminhos de slot, C6 ou partição. Sem prefetch ORM. `chamados/tests/test_e3_r3_c5_sem_banco.py:153–194`, `chamados/tests/test_e3_r3_c6_sem_banco.py:278–313`, `chamados/tests/e3_c8b_apoio.py:83–120,150–198`, `chamados/tests/test_e3_pos_suite_sem_banco.py:53–109`, `chamados/tests/test_e3_tarefa_admin_sem_banco.py:159–206`. |
| Outros harnesses que instalam a projeção | `app_com_relogios` passa `N` pelo filtro de vivas, com uma classe falsa `Perguntas` e um `Query` de memória. Os cenários `e3_volta7_apoio` e Degrau instalam a projeção para M1. No caso sintético de Degrau, a porta do app exclui o dia antes do laço M1. `chamados/tests/e3_v8_apoio.py:168–199`, `chamados/tests/e3_volta7_apoio.py:51–74`, `chamados/tests/test_e3_degrau_dono_e_porta.py:492–520`, `escala/services/cadastro_realidade.py:469–474,502`. |

## 2. Consultas por pergunta

Contagem por chamada, com os campos usados já carregados e após a avaliação do queryset de origem. `leitura` usa `p.decisoes.all()`, e o passivo lê também campos escalares de `p`. `chamados/models.py:4251–4269,4292,4304–4313`.

| Chamada | Consultas |
|---|---|
| `DecisaoPergunta.leitura(p)` com `decisoes` pré-carregado | **0 por pergunta**. Usa o cache da relação. A consulta do prefetch é da carga do lote. `chamados/models.py:4304–4313`, `chamados/services/fio.py:262–269`. |
| `DecisaoPergunta.leitura(p)` com PK e sem prefetch | **1 por chamada**, para materializar `p.decisoes.all()`, mesmo se a sequência estiver vazia. Uma nova chamada sem cache de prefetch volta à relação. `chamados/models.py:4308–4313`. |
| `DecisaoPergunta.leitura(p)` sem PK | **0**. Começa com `reais=[]` e calcula o passivo em memória. `chamados/models.py:4308–4313`. |
| `linhas_da_projecao_sem_linha([p], ja)` com `ja` em memória, inclusive vazio | **0**, com ou sem prefetch. Só constrói e compara chaves de ato e instante. É a forma usada por `leitura`. `chamados/models.py:4251–4270,4309–4313`. |
| `linhas_da_projecao_sem_linha(perguntas)` com `ja=None` | **0** se nenhuma pergunta tem carimbo projetado de resposta ou validação. Havendo alguma, **1 consulta para o lote inteiro**, inclusive se for uma única pergunta. Esse ramo monta `cls.objects.filter(...).values_list(...)` diretamente. `chamados/models.py:4251–4257`. |

## 3. Os sete testes de orçamento [nao verificado]

Nos quatro selos da batida, o percurso comum é `registrar_batida` → `post_save` → `cartorio.julgar_celula` → `julgar_colab` → `_julgar_colab_corpo` → `fio_vivo` → C6 → projeção temporal. O cartório carrega `perguntas_que_resolvem` sem `decisoes`. Se o chamado chega à guarda de resposta do cartório, `_tem_resposta_do_colab` também passa essas instâncias ao juiz de respondida. A fonte de SQL nesses juízes é a relação sem cache. `ponto/registro_batida.py:176`, `ponto/signals.py:85–93,131–135`, `ponto/services/cartorio.py:146–147,1016,1418,1684–1714,2088`, `chamados/juizes.py:898,1391,1411,1526,1561`, `chamados/models.py:4308–4313`.

| Teste | Total do anexo e caminho de produção | [nao verificado]
|---|---|
| `test_registrar_batida_com_1_furo_vivo_que_ela_fecha` | **237 / teto 230 (+7)**. A entrada das 07h fecha o último marco, após a saída das 19h preparada fora da captura. Além do percurso comum, o fechamento passa por `transicionar_com_pauta` → `reconciliar`. Havendo fio vivo no lote adiado, roda E2 antes do fecho. Ao gravar a transição, S143 pode voltar aos dois juízes sobre ancoradas ou pendentes sem decisões pré-carregadas. `ponto/tests/test_selo_chokepoint_escrita.py:406–438`, `ponto/services/cartorio.py:1019–1049,1569–1581`, `chamados/services/fecho_runtime.py:165`, `chamados/models.py:302,355–359`, `chamados/signals.py:97–117`, `chamados/services/disputa_emissao.py:1853,1919,1949–1953,1983–2023`. |
| `test_registrar_batida_com_3_furos_vivos_que_ela_fecha` | **301 / teto 294 (+7)**. Mesmo helper com três chamados e assert de três fechos. Os laços do fecho e do resolver também recebem perguntas sem prefetch de decisões. `ponto/tests/test_selo_chokepoint_escrita.py:416–433,440–443`, `ponto/services/cartorio.py:1026–1049,1569–1581`, `chamados/services/disputa_emissao.py:1949–1953,1983–2023`, `chamados/models.py:355–359,4308`. |
| `test_registrar_batida_com_1_furo_vivo_que_fica` | **147 / teto 145 (+2)**. A entrada das 07h deixa apagado o marco de saída que o chamado cobra. O percurso comum julga o fio nas perguntas do chamado. A guarda de respondida é alcançável se houver perguntas e o fio estiver morto. As duas leituras usam ORM sem prefetch de decisões nesse percurso. `ponto/tests/test_selo_chokepoint_escrita.py:445–473`, `ponto/services/cartorio.py:146–147,1418,1684–1697`, `chamados/models.py:4308`. |
| `test_registrar_batida_com_3_furos_vivos_que_ficam` | **222 / teto 219 (+3)**. Mesmo helper com três chamados e assert de pelo menos um vivo. O laço de vivos usa o mesmo percurso, e o dedup pode chamar de novo a guarda de respondida para irmãos. `ponto/tests/test_selo_chokepoint_escrita.py:445–468,475–478`, `ponto/services/cartorio.py:146–147,1418,1684–1714`, `chamados/models.py:4308`. |
| `test_lote_tem_queries_constantes` | **36 vs. 30 (+6)**, três perguntas contra uma. `carga.tarefa` chama `_ler_vida` sobre o ORM sem prefetch. O cenário insere resposta e validação na sequência e deixa o texto projetado vazio. A partição lê a relação e encaminha a pergunta da disputa aberta como pendente. A carga de vida lê de novo na preparação de `_slot_esperado`, e o C6 lê mais uma vez antes de sair pela validação vigente. **Três leituras de `decisoes` por pergunta adicional** explicam o diferencial pela leitura do código. O passivo separado de `_fatos_das_perguntas` trabalha por lote e só consulta com carimbo projetado. `chamados/tests/test_e2_tarefa_do_dia.py:111–122,913–938`, `chamados/models.py:4251–4257,4337–4347`, `chamados/services/tarefa_do_dia.py:425–432,530–542,620–632,665`, `chamados/services/pergunta_no_app.py:83–90`, `chamados/services/carga_pergunta_viva.py:117–119`, `chamados/juizes.py:345–351,1411–1413,1559–1564`. |
| `test_custo_por_candidata_e_CONSTANTE` | **9 / teto 8 (+1)**. A captura chama `desfazer_carimbo_sem_lastro` diretamente, com PKs. A porta relê cada candidata sem prefetch e chama C6, que para na validação vigente. A fixture grava `validada_em` na projeção. O SELECT adicional identificado é `decisoes.all()`. A validação do passivo entra em memória pelo `ja` da leitura. `chamados/tests/test_o261_julgamento_desfaz_carimbo.py:75–82,187–205`, `chamados/services/relavra_via.py:215–243`, `chamados/juizes.py:893–898,1389–1392,1411–1413`, `chamados/models.py:4254–4269,4308–4313`. |
| `test_MORDE_F5_queries_nao_crescem_com_os_dias` | **61 vs. 49 (+12)**, cinco dias contra um. `_folga_nao_definida_sem_porta` → `beco_em_lote` passa (1) por `alimentar_portas` → C6, (2) pela

[truncado em 20000 de 27425 caracteres]

## 19/11 15:29 · sessao 1911406c1480 · - · - · origem=Ronald (tmux)

Encontrei 38 pontos de chamada em produção para `pergunta_respondida` e três para `projecao_temporal_da_pergunta`. As tabelas usam caminhos relativos a `app/`.

### `pergunta_respondida`

| Chamada | Objeto e laço | Origem das decisões |
|---|---|---|
| `chamados/juizes.py:1430` | `SimpleNamespace` com o par temporal, uma chamada por C6. | O par vem da projeção do próprio C6. `chamados/juizes.py:1410-1411,1428-1430` |
| `chamados/models.py:2006` | `LeituraDoAto`, em laço. | `prefetch_related_objects(..., 'decisoes')` antes do laço. `chamados/models.py:1998-2006` |
| `chamados/models.py:3241,3244` | `LeituraDoAto`, uma chamada por acesso a `estado`, em ramos alternativos. | Origem variável. API recebe ORM com decisões carregadas pelo app. `chamados/models.py:3233-3244`, `api/views.py:2553,2566-2568`, `chamados/services/pergunta_no_app.py:52-65` |
| `chamados/models.py:3791` | `LeituraDoAto`, uma por acesso à propriedade `respondida`. | Herda a carga da instância. A leitura é feita antes de entrar no juiz. `chamados/models.py:3786-3791` |
| `chamados/services/raiox.py:147` | `SimpleNamespace` de duas colunas de `values_list`, em laço. | Sem relação de decisões no argumento. `chamados/services/raiox.py:135-147` |
| `chamados/views.py:1275`, `api/views.py:2640` | ORM avulso. | Sem prefetch nas buscas. `chamados/views.py:1239`, `api/views.py:2631-2634` |
| `api/views.py:2596`, `colaboradores/services/ficha.py:172` | ORM em laço, histórico da consulta e lista viva, respectivamente. | Com prefetch do app. `api/views.py:2553,2585-2596`, `colaboradores/services/ficha.py:166-172`, `chamados/services/pergunta_no_app.py:52-77` |
| `chamados/services/fio.py:712`, `chamados/services/acoes_disputa.py:134`, `chamados/views_cobrar.py:170,178` | ORM em laço. | `_vivas_de(d)` usa `d.perguntas.order_by('pk')` sem prefetch de decisões. `chamados/juizes.py:1512-1514` |
| `chamados/services/fio.py:1287,1300` | ORM em laços do grupo e das perdedoras. | O argumento é uma coleção. Os caminhos de eleição e dedup examinados carregam o ORM sem decisões. `chamados/services/fio.py:1335-1345,1962-1963,1999-2001,2073-2088,2319-2326`, `chamados/management/commands/dedup_perguntas_fato.py:74-77` |
| `chamados/services/becos.py:31` | ORM em laço da disputa. | Evento e comando prefetcham `perguntas`. `chamados/services/becos.py:30,85-87`, `chamados/management/commands/fechar_becos_disputa.py:32-39` |
| `chamados/services/disputa_emissao.py:1853` | ORM avulso no corpo, alcançado por laços S143 e backfill. | Sem prefetch nos seletores. `chamados/services/disputa_emissao.py:1983-1990`, `chamados/models.py:355-359`, `chamados/management/commands/backfill_fio_morto_e4b.py:89-90,134-135,166-175` |
| `chamados/services/disputa_emissao.py:2023` | ORM em laço S128. | Sem prefetch. `chamados/services/disputa_emissao.py:1949-1953,1998-2023` |
| `chamados/services/declaracao_texto.py:418,688` | ORM avulso, candidata reutilizável ou pergunta aberta pela porta. | Universo do fato sem prefetch. `chamados/services/declaracao_texto.py:251-255,373-378,663-668`, `chamados/services/fio.py:1962-1963` |
| `chamados/services/pergunta_no_app.py:86` | ORM em laço de partição. | Sim na consulta do app. A carga da tarefa fornece outras instâncias, sem prefetch da relação, mesmo com leituras mantidas em mapa. `chamados/services/pergunta_no_app.py:52-77`, `chamados/services/tarefa_do_dia.py:530-542,621` |
| `chamados/services/validacao.py:682,902` | ORM avulso. | Sem prefetch no `get`. `chamados/services/validacao.py:677,894-895` |
| `chamados/services/materializacao.py:192` | ORM avulso no par do intervalo, inclusive dentro dos laços da tela e da materialização. | Com prefetch no percurso do fio, sem na validação e na materialização. `chamados/services/fio.py:262-269,360-381`, `chamados/services/materializacao.py:923,1732,2104-2125`, `chamados/services/validacao.py:632,677` |
| `chamados/services/materializacao.py:259` | ORM avulso no predicado BUG-PAR-103, usado também pelo laço do contador. | Sem prefetch no contador ou na porta. `chamados/services/materializacao.py:324-330`, `chamados/services/validacao.py:333-334` |
| `chamados/services/materializacao.py:290` | ORM avulso da irmã, chamado no laço do contador. | `first()` sem prefetch. `chamados/services/materializacao.py:284-290,328-342` |
| `chamados/services/materializacao.py:885` | ORM avulso no estado da aplicação, chamado em laços do contador e do fio. | Misto. Contador sem prefetch, fio com. `chamados/services/materializacao.py:324-342`, `chamados/services/fio.py:262-269,360-381` |
| `chamados/services/materializacao.py:1300` | ORM por pergunta nos três ramos de materialização. | A porta de validação relê a pergunta sem prefetch no laço. `chamados/services/materializacao.py:1355,1590,1674,2104-2125` |
| `chamados/services/vigia_fio.py:636` | ORM em laço por dia e pergunta. | `alim['vivas']` de `alimentar_portas`, sem prefetch. `chamados/services/vigia_fio.py:559-570,595-597`, `chamados/juizes.py:1668-1673` |
| `chamados/management/commands/dedup_perguntas_fato.py:76` | ORM em laço dentro dos grupos. | Sem prefetch no seletor dos membros. `chamados/management/commands/dedup_perguntas_fato.py:74-77` |
| `folha/previa.py:142` | ORM em `any`, chamado no laço de dias e lâmpadas. | Sem prefetch. `folha/previa.py:64-67,77-95` |
| `chamados/reconciliador.py:366` | ORM avulso no corpo, chamado no laço da reata e na emissão. | Queryset da reata sem prefetch, emissão pode passar instância recém-criada. `chamados/reconciliador.py:1547-1554,1568-1574,1667`, `chamados/services/disputa_emissao.py:452-473` |
| `chamados/reconciliador.py:1130` | ORM avulso no corpo do veredito, chamado nos laços de órfãs. | Sem prefetch. `chamados/reconciliador.py:1094-1101,1182-1183,1238-1240,1350-1351` |
| `ponto/services/cartorio.py:1689` | ORM em `any` dentro dos laços de chamados e dedup. | Só prefetch da relação `perguntas_que_resolvem`. `ponto/services/cartorio.py:146-147,1690-1696,1709-1714` |
| `chamados/management/commands/auditar_invariantes_chamados.py:178,597,1671` | ORM em laços I21, I2 e F1, respectivamente. | I21 e I2 sem prefetch. F1 só prefetch da relação `perguntas_que_resolvem`. `chamados/management/commands/auditar_invariantes_chamados.py:590-597,1585-1598,1642-1645,1666-1671` |

### `projecao_temporal_da_pergunta`

| Chamada | Objeto e laço | Origem |
|---|---|---|
| `chamados/juizes.py:351` `_slot_esperado` | ORM repassado, uma por chamada quando há `agora`. O pré-cálculo percorre todas as perguntas e a testemunha também pode chegar a esse ponto. | Carga da tarefa sem prefetch. Herda a origem nos outros leitores. `chamados/services/carga_pergunta_viva.py:117-123`, `chamados/services/tarefa_do_dia.py:530-534,625`, `chamados/juizes.py:348-351,756-757` |
| `chamados/juizes.py:1411` `_pergunta_viva_calc` | ORM repassado, uma por execução do C6, chamado por muitos laços. | Misto, com prefetch no fio e no app e sem na tarefa e em `alimentar_portas`. `chamados/juizes.py:886-898,1668-1673`, `chamados/services/fio.py:262-275`, `chamados/services/pergunta_no_app.py:52-65`, `chamados/services/tarefa_do_dia.py:530-534,625-632` |
| `escala/services/cadastro_realidade.py:502` `beco_em_lote` | ORM em `all` dentro do laço de dias, avaliado quando a pergunta não tem âncora. | Reutiliza instâncias de `alimentar_portas` sem prefetch. `escala/services/cadastro_realidade.py:459-474,497-502`, `chamados/juizes.py:1668-1673` |

## 19/11 15:28 · sessao 9128998275dc · - · - · origem=Ronald (tmux)

Os quatro testes medem a gravação síncrona da entrada das 07h. Nos cenários `fecha`, a saída das 19h já foi registrada fora da captura e o teste confirma o fechamento dos 1 ou 3 chamados. Nos cenários `fica`, a saída segue apagada e o teste exige pelo menos um chamado vivo. O anexo informa respectivamente 237/230, 301/294, 147/145 e 222/219. A captura é convertida em contagem antes da asserção, então a contribuição efetiva de cada ramo para esses totais é [nao verificado]. `ponto/tests/test_selo_chokepoint_escrita.py:406–478`, `:287–298`.

- Caminho comum. `registrar_batida` cria a batida, o `post_save` julga o dia e a véspera, e o cartório calcula o fio vivo de cada chamado vivo. O prefetch leva `perguntas_que_resolvem`, sem `decisoes`. Para cada pergunta alcançada, `fio_vivo` chama `pergunta_viva`, que no cálculo chama `projecao_temporal_da_pergunta`. No ramo de fio morto, o cartório também chama `pergunta_respondida` diretamente para verificar a resposta do colaborador. `ponto/registro_batida.py:176`, `ponto/signals.py:85–93`, `:108–112`, `:131–135`, `ponto/services/cartorio.py:146–147`, `:1016`, `:1418`, `:1684–1714`, `:2088`, `chamados/juizes.py:893–898`, `:1411`, `:1526`.
- Cenários `fecha`. O fechamento por marco chama `transicionar_com_pauta(..., ato=x.reconciliar)` imediatamente, ou após lavrar e executar E2 quando houver fio vivo. E2 consulta ORM sem `decisoes` e chama C6. O gate do fechamento também percorre as ancoradas ORM sem `decisoes` e chama C6. O `save` do fechamento aciona S143, que consulta as duas cargas sem `decisoes` e tem chamadas diretas de `pergunta_respondida`, além de C6. `ponto/services/cartorio.py:1569–1581`, `:1019–1049`, `chamados/reconciliador.py:698–708`, `chamados/models.py:302`, `:355–380`, `:393–395`, `:422–431`, `:654–667`, `:695–701`, `chamados/signals.py:97–117`, `chamados/services/disputa_emissao.py:1853`, `:1949–1953`, `:1983–1990`, `:1998–2023`.
- Cenários `fica`. A saída apagada mantém o chamado no cálculo do fio e pode alcançar a guarda de resposta ou a fábrica após o julgamento. Se houver candidatas a descarimbo, a fábrica chama a porta que recarrega cada pergunta sem `decisoes` e consulta C6. A presença dessas candidatas na execução anexada é [nao verificado]. Depois do cartório, o receiver do reconciliador executa E2, o reatamento e a retratação de premissas mortas. `ponto/tests/test_selo_chokepoint_escrita.py:445–478`, `ponto/services/cartorio.py:1071–1073`, `:1493–1496`, `:1684–1697`, `chamados/services/fabrica_da_celula.py:207–219`, `chamados/services/relavra_via.py:236–242`, `ponto/signals.py:206–215`, `chamados/reconciliador.py:1480–1489`.

A consulta que a volta 2 pode acrescentar nesses ramos é `p.decisoes.all()` em cada chamada da leitura com ORM persistido e sem prefetch. O passivo E4b recebe `ja` calculado em memória e não entra no ramo SQL. O memo do C6 pode evitar uma nova leitura quando a mesma instância chega com a mesma chave. `chamados/juizes.py:894–905`, `:1389–1391`, `:1559–1561`, `chamados/models.py:4251–4257`, `:4308–4313`.

## 19/11 15:28 · sessao 304fee693a3b · - · - · origem=Ronald (tmux)

`funcao()` recompila apenas o corpo extraído por AST e usa o dicionário de globais fornecido pelo harness. Os dois juízes resolvem `PerguntaDisputa` antes de importar `DecisaoPergunta`. Um nome ausente gera `NameError`. Uma instância `SimpleNamespace` no segundo argumento de `isinstance` gera `TypeError`. Em ambos os casos, a execução para antes da leitura do ato. [e4_apoio.py:8](/home/ronald/codex_ws/e673e0efe1135d60e844387527d53042d651a34d/app/chamados/tests/e4_apoio.py:8) [juizes.py:1389](/home/ronald/codex_ws/e673e0efe1135d60e844387527d53042d651a34d/app/chamados/juizes.py:1389) [juizes.py:1559](/home/ronald/codex_ws/e673e0efe1135d60e844387527d53042d651a34d/app/chamados/juizes.py:1559)

| Família | Dublê e origem da falha |
|---|---|
| Bug Par, Degrau Dono e Porta, C8c posição | `juiz_respondida()` fornece apenas `texto_vazio` aos globais AST. Bug Par e os dois casos de Degrau injetam o juiz extraído e passam perguntas `NS/N`. C8c posição também usa `tarefa_em_memoria`, que instala módulos falsos com `PerguntaDisputa=N(constantes)` e `DecisaoPergunta=N(literais)`. O patch em `sys.modules` não popula os globais da função AST. `chamados/tests/e4_apoio.py:19`, `test_bug_par_sem_banco.py:164-195`, `test_e3_degrau_dono_e_porta.py:290-346`, `test_e3_c8c_sem_banco.py:41-64`, `test_e3_c8b_sem_banco.py:14-23`, `test_e3_tarefa_admin_sem_banco.py:71-86`. |
| C8b, C8c foto, pós-suite | `ambiente_c6` põe nos globais dos dois juízes `PerguntaDisputa=N(**literais(...))`, com constantes e métodos de vias, e trabalha com perguntas `N`. O próprio contexto troca `chamados.juizes`. Quando `tarefa_em_memoria` coexiste, ela fornece o módulo falso de modelos acima. `chamados/tests/e3_c8b_apoio.py:83-90,109-120,150-179,194-198`, `test_e3_c8c_sem_banco.py:124-140`, `test_e3_pos_suite_sem_banco.py:53-109`. |
| C11 C5/C6 | O módulo falso de modelos tem apenas `ChamadoColaborador=N(objects=Consulta(...))`. A projeção AST recebe separadamente `PerguntaDisputa=N(VIAS_FORA_DO_FATO=())`. O C11 passa perguntas `N` do lote de vivas à projeção. `chamados/tests/test_e3_r3_c5_sem_banco.py:153-194`, `test_e3_r3_c6_sem_banco.py:278-313`, `escala/services/cadastro_realidade.py:502`. |
| Tarefa admin | Injeta nos globais AST `PerguntaDisputa=N(VIAS_QUE_VENCEM_A_RESPOSTA=(), vias_que_encerram_por_identidade=...)` e passa pergunta `N`. O patch de módulos desse teste não substitui `chamados.models`. `chamados/tests/test_e3_tarefa_admin_sem_banco.py:159-166,183-206`. |
| E4b memória | Perguntas `Objeto`, relação `decisoes=Linhas()`. Módulo falso `NS(PerguntaDisputa=self.PD, ChamadoColaborador=self.CC, DecisaoPergunta=NS(), DisputaSupervisao=NS(...))`. Os juízes recompilados têm outro `NS` nos seus globais, com constantes e métodos de vias. C11, C4 tranca, C4v14, C4v7 e C6 reutilizam o harness. C4v14 injeta as classes reais só na função AST `sem_veredito_entre`, preservando os globais dos juízes herdados. `chamados/tests/test_e4b_classe_e_memoria.py:151-161,282-313,318-335`, `test_e4b_c11_memoria.py:37,79`, `test_e4b_c4_tranca_memoria.py:27`, `test_e4b_c4v14_memoria.py:32-53`, `test_e4b_c4v7_memoria.py:85-89`, `test_e4b_c6_memoria.py:15-17`. |

O caso C8b do slot aparece como `FAIL` porque a carga guarda o `TypeError` de `_slot_esperado` em `slots[p.pk]` e o teste compara o valor com `(ts, 'S')`. [carga_pergunta_viva.py:117](/home/ronald/codex_ws/e673e0efe1135d60e844387527d53042d651a34d/app/chamados/services/carga_pergunta_viva.py:117) [juizes.py:345](/home/ronald/codex_ws/e673e0efe1135d60e844387527d53042d651a34d/app/chamados/juizes.py:345) [test_e3_c8b_sem_banco.py:406](/home/ronald/codex_ws/e673e0efe1135d60e844387527d53042d651a34d/app/chamados/tests/test_e3_c8b_sem_banco.py:406)

## 19/11 15:23 · sessao db53e16adcc8 · DST-L2-JZ1 · sonda · origem=Code

### 1. Filtro e significado de `pendente`

**Sim, para o status literal `pendente`.** O filtro exige `status_local` em `('aberto', 'em_analise', 'registrado')`, a chave estrangeira `justificativa_relacionada` preenchida e `status='pendente'` na justificativa apontada por essa chave. (`pares.py:8–11`, anexo; `app/chamados/models.py:58,75–79`; `app/chamados/catalogo/motor.py:24`; `app/ponto/models.py:159`.)

`pendente` é a única opção do modelo com significado de pendência. As outras opções declaradas são `aprovada` e `rejeitada`. A regra de fechamento de chamado também trata `aguardando_dp` e `aguardando_colab` como pendência, embora esses valores estejam fora das opções declaradas de `Justificativa.status`. Se estivessem gravados, ficariam fora da sonda. A presença desses valores no banco é [nao verificado]. (`app/ponto/models.py:130–134,159`; `app/chamados/models.py:141–147`; `pares.py:9–10`, anexo.)

A análise usa os arquivos da cópia. A correspondência com o HEAD `688696dbb32b7859fb38a141c54d5a6f456056b3` é [nao verificado], pois a cópia está sem metadados Git.

### 2. Total e pares

**37 pares.** `CONTADOR=37 UNIVERSO=37 IGUAL=True ERROS=0`. O script chama `qs.count()` sobre o mesmo `qs` depois de materializá-lo com `list(qs)`. (`pares.py:12–13,20–22`, anexo; saída anexada:6–43, numerada a partir de `=== SOMBRA`.)

Transcrição literal da saída anexada:6–42 (`pares.py:20–21`, anexo).

```text
PAR chamado=38675 justificativa=3643
PAR chamado=38693 justificativa=3649
PAR chamado=38707 justificativa=3652
PAR chamado=38868 justificativa=3665
PAR chamado=38894 justificativa=3672
PAR chamado=38905 justificativa=3675
PAR chamado=38906 justificativa=3676
PAR chamado=38907 justificativa=3677
PAR chamado=38947 justificativa=3679
PAR chamado=39041 justificativa=3682
PAR chamado=39044 justificativa=3685
PAR chamado=39045 justificativa=3686
PAR chamado=39065 justificativa=3692
PAR chamado=39067 justificativa=3694
PAR chamado=39068 justificativa=3695
PAR chamado=39077 justificativa=3696
PAR chamado=39078 justificativa=3697
PAR chamado=39085 justificativa=3699
PAR chamado=39219 justificativa=3709
PAR chamado=39221 justificativa=3710
PAR chamado=39245 justificativa=3712
PAR chamado=39254 justificativa=3713
PAR chamado=39255 justificativa=3714
PAR chamado=39398 justificativa=3717
PAR chamado=39408 justificativa=3718
PAR chamado=39552 justificativa=3723
PAR chamado=39565 justificativa=3727
PAR chamado=39566 justificativa=3728
PAR chamado=39579 justificativa=3731
PAR chamado=39583 justificativa=3733
PAR chamado=39679 justificativa=3735
PAR chamado=39718 justificativa=3736
PAR chamado=39754 justificativa=3742
PAR chamado=39755 justificativa=3743
PAR chamado=39759 justificativa=3744
PAR chamado=39760 justificativa=3745
PAR chamado=39764 justificativa=3748
```

### 3. Sinais de falha ou schema errado

A saída não indica falha de leitura nem universo vazio. Ela registra `ERROS=0`, `rc_sombra=0`, 184 chamados vivos com ponteiro e 37 pares após o filtro de status da justificativa. (`pares.py:8–19,22`, anexo; saída anexada:5–44.)

A saída não indica schema errado. O script solicita `schema_context('juliani')`, mas a saída não imprime o schema efetivo da conexão. O schema efetivamente consultado é [nao verificado]. (`pares.py:6,17–22`, anexo.)

## 19/11 15:19 · sessao 36b247af5b6e · E4C-F5-CONSTROI · construtor · origem=Code

A volta 3 usa `projecao_temporal_da_pergunta` como fonte comum. `DecisaoPergunta.leitura` fornece os instantes de resposta e validação, e `PerguntaDisputa.resposta_colab` fornece o texto corrente. [chamados/juizes.py:1389](/home/ronald/wt-codex-e4c-f5-constroi-144636/app/chamados/juizes.py:1389)

| Sítio | Antes na base informada | Depois e caso |
|---|---|---|
| App | `chamados/services/pergunta_no_app.py:86` já passava a `PerguntaDisputa` ao juiz com a leitura correta. | A chamada segue na linha 86 e o juiz passa pela fonte comum em `chamados/juizes.py:1561`. **Guarda** em `chamados/tests/test_e4c_f5_volta3_sitios.py:25`. |
| Property `respondida` | `chamados/models.py:3791` passava `DecisaoPergunta.leitura(self)` ao juiz, que recebia o texto do ato. | Passa `self` ao juiz em `chamados/models.py:3791`. O **RED** espera `False` com texto corrente em branco e texto preenchido no ato. `chamados/tests/test_e4c_f5_volta3_sitios.py:31`. |
| Fio | `chamados/services/fio.py:192` já usava os valores corretos, combinados localmente com `_replace`. | Lê a fonte comum e conserva o predicado de espera: ausência de carimbo ou `respondida_sem_texto`. O caso guarda os resultados e exige a chamada à fonte comum, que faltava na base. `chamados/services/fio.py:189`, `chamados/juizes.py:1538`, `chamados/tests/test_e4c_f5_volta3_sitios.py:37`. |

O contador usa `iterator` com prefetch de `decisoes` e lote de **500**, tamanho adotado nesta volta. O RED reduz o lote a dois, exige **2 consultas para 2 perguntas** e **4 para 6**, e confere os resultados `(1, 2)` e `(1, 6)`. A guarda de divergência e universo também está no arquivo. [Implementação](/home/ronald/wt-codex-e4c-f5-constroi-144636/app/chamados/services/vigia_texto_resposta.py:7) · [RED](/home/ronald/wt-codex-e4c-f5-constroi-144636/app/chamados/tests/test_e4c_f5_contador_texto.py:79) · [Guarda](/home/ronald/wt-codex-e4c-f5-constroi-144636/app/chamados/tests/test_e4c_f5_contador_texto.py:27)

**Arquivos tocados**

- Produção — `chamados/juizes.py:1559`, `chamados/models.py:3786`, `chamados/services/fio.py:187`, `chamados/services/vigia_texto_resposta.py:7`.
- Casos e mutantes — `chamados/tests/test_e4c_f5_volta3_sitios.py:12`, `chamados/tests/test_e4c_f5_contador_texto.py:73`, `chamados/tests/e4c_f5_volta3_mutantes.py:37`.
- Selos anteriores — `chamados/tests/test_e4c_f2_leitura_do_ato.py:500` ajusta a expectativa da property com texto só no ato. `chamados/tests/e4c_f5_volta2_mutantes.py:32` e `chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:229` adaptam o selo do fio à nova forma da chamada.

**Comandos para o Code**

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test --settings=config.settings.ci --noinput \
  chamados.tests.test_e4c_f5_volta3_sitios.SitiosVolta3Test.test_guarda_particao_do_app \
  chamados.tests.test_e4c_f5_volta3_sitios.SitiosVolta3Test.test_red_property_respondida \
  chamados.tests.test_e4c_f5_volta3_sitios.SitiosVolta3Test.test_red_fio_le_fonte_unica \
  chamados.tests.test_e4c_f5_contador_texto.ContadorTextoTest.test_red_custo_cresce_por_lote \
  chamados.tests.test_e4c_f5_contador_texto.ContadorTextoTest.test_divergencia_e_universo

PYTHONDONTWRITEBYTECODE=1 python3 manage.py test --settings=config.settings.ci --noinput \
  chamados.tests.test_e4c_f5_volta1_leem_o_ato.SeloTest \
  chamados.tests.test_e4c_f2_leitura_do_ato.FonteMigradaPorLeitorTest.test_resposta_so_no_ato_cada_leitor_le_o_ato
```

| Mutante | Caso e falha esperada |
|---|---|
| **m1** — property volta a passar a leitura do ato | `test_red_property_respondida` falha no texto corrente em branco. `chamados/tests/test_e4c_f5_volta3_sitios.py:35` |
| **m2** — fio lê diretamente o texto do ato | `test_red_fio_le_fonte_unica` falha ao esperar o texto corrente. `chamados/tests/test_e4c_f5_volta3_sitios.py:43` |
| **m3** — vigia sem `iterator` | `test_red_custo_cresce_por_lote` espera diferença 2 e recebe 0. `chamados/tests/test_e4c_f5_contador_texto.py:89` |
| **m4** — vigia com `iterator` sem prefetch | `test_red_custo_cresce_por_lote` espera diferença 2 e recebe 4, pelo padrão `1 + K`. `chamados/tests/test_e4c_f5_contador_texto.py:89` |
| **fio-base** — recompõe os valores como na base | `test_red_fio_le_fonte_unica` chega à asserção de chamadas à fonte comum com a lista vazia. `chamados/tests/test_e4c_f5_volta3_sitios.py:44` |

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4c_f5_volta3_mutantes m1
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4c_f5_volta3_mutantes m2
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4c_f5_volta3_mutantes m3
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4c_f5_volta3_mutantes m4
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4c_f5_volta3_mutantes fio-base
```

Compilei os arquivos em memória e conferi as âncoras e os labels do [executor de mutantes](/home/ronald/wt-codex-e4c-f5-constroi-144636/app/chamados/tests/e4c_f5_volta3_mutantes.py:37). Execução dos testes, consultas medidas e mortes dos mutantes [nao verificado]. [nao verificado]

LEI-AKITA: origem=chamados/services/pergunta_no_app.py:86 + chamados/models.py:3791 + chamados/services/fio.py:189, testemunha=chamados/juizes.py:1389, RED=chamados/tests/test_e4c_f5_volta3_sitios.py:25 + chamados/tests/test_e4c_f5_contador_texto.py:79, padrao=juiz unico

## 19/11 15:18 · sessao 803c24189c48 · REPLAY-PORTA-EXPORT-SOL62 · verificador · origem=Code

**Volta 3 — reprova no R8 e na fidelidade da trilha.** F fecha a liberação por carimbo órfão e por `None == None`. A exceção em `assumir` ainda sai sem persistir a falha, e `encerrar` pode registrar um estado anterior diferente daquele que o CAS venceu. [app/folha/porta_export.py:1252](/home/ronald/codex_ws/0ae4f009b9aa83326ff1bd921eee7badfec7caa5/app/folha/porta_export.py:1252) · [app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:140](/home/ronald/codex_ws/0ae4f009b9aa83326ff1bd921eee7badfec7caa5/app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:140) · [app/folha/porta_export.py:1064](/home/ronald/codex_ws/0ae4f009b9aa83326ff1bd921eee7badfec7caa5/app/folha/porta_export.py:1064)

Revisão estática, em somente leitura, sobre a cópia e o diff fornecido. Identidade Git da cópia, execução da suíte, estado do banco e linhas efetivamente emitidas em produção [nao verificado]. [nao verificado]

### 1. Quem escreve e quem lê

| Campo | Escritores | Leitores |
|---|---|---|
| Pedido `detalhe.tentativa` | `pedir` cria ou renova o UUID. `assumir` cria outro UUID para cron/manual ou adota o existente para o job com `--tentativa`. | `_ficha` leva a identidade aos CAS, `assumir` a usa na adoção, `disparar` a põe no argv, `concluir` a usa na trava da linha e na lavra, `_recusa` a põe no objeto e `estado_da_porta` compara o par. `app/folha/porta_export.py:1020,1058,1083,1099,1121,1131,1170,1260,1304` |
| Pedido `detalhe.ator` | `pedir` grava `ator.pk` na criação e na renovação. O POST passa `request.user`. A adoção preserva o campo por cópia do detalhe. Uma tentativa nova de cron/manual nasce sem esse campo. | `_recusa` lê o campo para `ator=`. `app/folha/views.py:130` · `app/folha/porta_export.py:1035,1055,1082,1099,1124,1134,1284` |
| Carimbo `detalhe.tentativa` | `lavrar` grava. O único chamador de produção encontrado é `concluir`. `lavrar_verde_para_teste`, inclusive no teste de relatórios, passa por `assumir` e `concluir`. | `estado_da_porta` lê para decidir. Há leituras de verificação nos testes. `app/folha/porta_export.py:768,784,887,1178,1260` · `app/relatorios/tests/test_cauda_trilha_do_cartao.py:109` · `app/folha/tests/test_porta_sem_beco_item2.py:90,266` |
| Motivo | `_falhar` persiste até 500 caracteres no pedido. `concluir` persiste o motivo de `resultado_de`. `_recusa` emite o motivo com a trilha e, para divergência, monta a frase com `n` quando o motivo persistido está vazio. | `ler_linha` extrai o texto. O contador do ESTADO conta por data da linha e porta. A tela de `FALHA` usa mensagem fixa e a de divergência lê os detalhes do carimbo. `app/folha/porta_export.py:823,871,933,1050,1064,1140,1179` · `app/core/recusa_critica.py:46,89,108,133` · `bin/recusa_estado.py:60` · `bin/gerar_estado.py:269` |

**Há escritores fora de `concluir` nos testes.** `PortaDoExportTest._carimbo` grava diretamente o pedido e o carimbo com `'fixture'`. Dois testes de `VoltaDoisTest` chamam `lavrar` diretamente para fabricar as entradas rejeitadas. Na cópia, o leitor que decide a liberação é `estado_da_porta`, via `conferir` e a view. [app/folha/tests/test_porta_do_export.py:33](/home/ronald/codex_ws/0ae4f009b9aa83326ff1bd921eee7badfec7caa5/app/folha/tests/test_porta_do_export.py:33) · [app/folha/tests/test_porta_sem_beco_item2.py:302](/home/ronald/codex_ws/0ae4f009b9aa83326ff1bd921eee7badfec7caa5/app/folha/tests/test_porta_sem_beco_item2.py:302) · [app/folha/porta_export.py:820](/home/ronald/codex_ws/0ae4f009b9aa83326ff1bd921eee7badfec7caa5/app/folha/porta_export.py:820) · [app/folha/views.py:130](/home/ronald/codex_ws/0ae4f009b9aa83326ff1bd921eee7badfec7caa5/app/folha/views.py:130)

### 2. Quando pode devolver `PRONTA`

- **Para o par lido**, `PRONTA` exige pedido `CONCLUIDO`, tentativa não vazia e igualdade com a tentativa do carimbo. Há uma corrida de atualidade. A leitura pode obter o pedido concluído de A, o cron instalar B `EM_CURSO` na mesma linha, e a segunda consulta ainda encontrar o carimbo de A. O par lido é coerente e a resposta pode ser `PRONTA` com B já vigente. `app/folha/porta_export.py:1128,1172,1248,1260`
- **Cron sem clique, rollout e manual têm caminho para `PRONTA`.** `assumir` cria o pedido quando falta `--tentativa`, e `concluir` grava o par. O rollout escolhe as empresas ativas em rollout e a competência corrente de cada uma. Um carimbo legado sem par exige nova medição. Não encontrei bloqueio permanente introduzido por F nesses caminhos. `app/config/crons.py:1803` · `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:43,140,155` · `app/ponto/services/gestao_he.py:424` · `app/folha/porta_export.py:1128,1172,1258,1282`

### 3. R8 e atomicidade

- **`assumir` levantou.** O `except` escreve em stdout, inclui a empresa em `falharam` e segue, se a saída da trava funcionar. Antes de existir linha, sobra zero pedido e zero recusa `porta_export`. Se havia um `PEDIDO` do clique e a adoção falhou antes do CAS, ele fica `PEDIDO` até ser assumido ou até uma passagem do vigia após o prazo de 300 s. Nesse último caso, o motivo é o do prazo, sem a exceção original. Um verde anterior pode continuar válido se a exceção ocorreu antes de substituir o pedido. O resumo que diz que cada empresa `FALHOU` com seu motivo também cobre esse ramo. `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:140,260` · `app/folha/porta_export.py:918,986,1118,1208,1248`
- **`concluir` levantou antes ou dentro do `atomic`.** Se a exceção sai do bloco transacional, a nova gravação do carimbo e a transição são revertidas juntas. Com a mesma tentativa ainda `EM_CURSO`, `encerrar` pode vencer o CAS, persistir o motivo, emitir uma recusa e seguir para B. Se o próprio `encerrar` ou a saída da trava levantar, o laço pode abortar antes de B. `app/folha/porta_export.py:984,1064,1170,1188` · `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:129,155`
- **`concluir` também pode levantar depois do `atomic`.** Se `_recusa` levantar, o par já foi gravado pelo bloco. `encerrar` aceita apenas `PEDIDO` ou `EM_CURSO`, então não corrige uma tentativa já terminal nem repõe a emissão. É possível ficar com o par terminal completo e sem linha, se a exceção ocorrer antes da emissão. A afirmação do comentário do comando de que qualquer exceção em `concluir` deixa tudo desfeito é ampla demais. `app/folha/porta_export.py:1172,1182,1188` · `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:152`

### 4. Fidelidade da linha de recusa

- **`concluir` tem antes/depois sustentados pela linha travada.** Seleciona `EM_CURSO` da mesma tentativa e grava `FALHOU` ou `CONCLUIDO`. Em `vigiar`, cada CAS de falha aceita um único estado, o mesmo que o vigia leu. Já `encerrar` aceita dois. Uma cópia lida como `PEDIDO` pode sobreviver à adoção da mesma tentativa, vencer o CAS quando o banco já está `EM_CURSO` e emitir `antes=pedido`. A ficha compara o UUID e o filtro aceita ambos os estados. `app/folha/porta_export.py:1039,1064,1121,1172,1188,1202,1214`
- **`ator=` é o PK no fluxo do clique.** O registrador recebe `request=None`, então essa linha tem `usuario=-`. A origem tem uma brecha independente. O comando aceita `--tentativa` com `--origem` manual ou cron, e a adoção sobrescreve `origem` enquanto preserva o ator do clique. A linha pode atribuir ao pedido a origem do executor. Os POSTs barrados também emitem a recusa genérica da rota, que pode incluir o username no campo `usuario`. O contador `porta_export` filtra essa outra linha pelo nome da porta. `app/folha/porta_export.py:1055,1082,1124,1304` · `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:39` · `app/folha/views.py:132` · `app/core/respostas.py:54` · `app/core/recusa_critica.py:77,119`

### 5. O que os testes novos provam [nao verificado]

- As duas classes têm `freeze_time`. Os casos do carimbo órfão e do `None == None` exercitam a porta real. O primeiro fabrica o órfão também sem UUID, deixando sem caso explícito o órfão com UUID. O teste do helper prova o par final, e uma implementação com gravação direta do mesmo par também passaria. `app/folha/tests/test_porta_sem_beco_item2.py:292,302,310,320,387`
- **M9 morde.** Exige sucessora `EM_CURSO`, nenhum carimbo e nenhuma recusa da conclusão atrasada. **M12 tem um mutante sobrevivente isoladamente.** Se `_falhar` trocar `if venceu` por `if not venceu`, o vencedor fica calado e o perdedor emite a única linha com o mesmo antes/depois/origem. O teste não exige o motivo `banco caiu`, que identifica o vencedor. `app/folha/tests/test_porta_sem_beco_item2.py:328,346` · `app/folha/porta_export.py:1064`
- O teste do relógio morde uma segunda leitura **usada para consultar o outro dia**. Uma leitura adicional cujo retorno seja descartado passa. O reclique exige um `Popen` e uma linha `porta_export`; as recusas genéricas dos POSTs ficam fora do filtro. `app/folha/tests/test_porta_sem_beco_item2.py:29,361,371`
- **`R8ConcluirTest` morde o isolamento do comando para a exceção injetada.** A usa um mock que levanta antes de executar `concluir`, B usa a implementação real. As asserções cobram o motivo de A e `PRONTA` em B. Rollback da lavra, falha após o `atomic`, quantidade de recusas e exceção em `assumir` ficam sem cobertura nesse teste. O shell alterado cobre arquivo ausente e a contagem com linhas sintéticas. `app/folha/tests/test_porta_sem_beco_item2.py:396` · `bin/tests/test_recusa_estado.sh:28,38,48`

### 6. Classes da triagem

- **Presentes.** Segundo escritor na fixture, selos com as lacunas acima e campo com dois significados em `origem`, que serve de origem do pedido e da execução. O relógio foi unificado dentro de `estado_da_porta`. Ainda há uma releitura preexistente no `IntegrityError` de `pedir`, além de `abrir_a_porta` decidir e depois chamar `conferir` em outra leitura. Se a primeira vê `PRONTA` antes da virada e a segunda vê `SEM_MEDICAO` depois dela, esse POST recusa sem disparar. `app/folha/tests/test_porta_do_export.py:42` · `app/folha/tests/test_porta_sem_beco_item2.py:346,396` · `app/folha/porta_export.py:1091,1124,1248,1282`
- **Sem segundo juiz de abertura, derivação paralela nova ou inversão de trava encontrados.** `conferir` delega a decisão. `resultado_de` e `lavrar` recebem o mesmo `r`; a escrita do resultado e do carimbo ocorre no mesmo `atomic`. O comando pega a trava de sessão antes de `concluir` pegar a linha do pedido e lavrar o carimbo. `vigiar` consulta `pg_locks` sem disputar a advisory lock. `app/folha/porta_export.py:820,993,1140,1170,1248` · `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:129`
- **Demais descartes conferidos no código.** O log vem depois da transição. A prova de vida exige PID de sessão, mesma chave e trava concedida. O retorno do comando é agregado. Os dois contadores usam a mesma regra de data da linha e o mesmo leitor de arquivo, que pula `OSError`. O limite de tempo é de `PEDIDO`; `EM_CURSO` com trava viva permanece em conferência. `app/folha/porta_export.py:1001,1064,1182,1202` · `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:257` · `app/core/recusa_critica.py:95,108,126` · `bin/recusa_estado.py:37,60`

## 19/11 15:13 · sessao 8de25d40b48b · REPLAY-PORTA-EXPORT-SOL62 · verificador · origem=Code

**VEREDITO DA VOLTA 2 — NÃO CONFERE.** A conclusão e o carimbo já são gravados juntos e protegidos pelo UUID da tentativa. Ainda assim, a leitura pode liberar com o verde da tentativa anterior enquanto a sucessora está `EM_CURSO` ou já está `FALHOU`. A recusa crítica também pode faltar depois de uma transição terminal persistida. (`app/folha/porta_export.py:1056–1061`, `app/folha/porta_export.py:1160–1175`, `app/folha/porta_export.py:1238–1258`.)

A revisão foi estática, em modo somente leitura. Hash e histórico Git da cópia, execução dos testes, dados de banco, cron instalado e logs de produção [nao verificado]. [nao verificado]

## 1. Escritores e leitores

Pedido e carimbo são duas linhas de `MetricaSnapshot`. Cada uma tem chave própria com a competência, `escopo_id` da empresa e `data_ref` do dia civil. A unicidade cobre esse conjunto. (`app/folha/porta_export.py:779–784`, `app/folha/porta_export.py:933–941`, `app/folha/porta_export.py:1072–1079`, `app/inteligencia/models.py:87–105`.)

| Campo | Quem escreve | Quem lê |
|---|---|---|
| Pedido `estado` | `pedir` cria, `assumir` cria ou substitui no cron, `_mudar_estado` faz as transições chamadas por `pedir`, `assumir`, `concluir` e `_falhar`. (`app/folha/porta_export.py:1030–1042`, `app/folha/porta_export.py:1056–1090`, `app/folha/porta_export.py:1111–1126`, `app/folha/porta_export.py:1162–1182`.) | `pedir`, `assumir`, os filtros de `_mudar_estado` e `concluir`, `vigiar`, `vigiar_abertos` e `estado_da_porta`. (`app/folha/porta_export.py:1034–1036`, `app/folha/porta_export.py:1085–1090`, `app/folha/porta_export.py:1114–1120`, `app/folha/porta_export.py:1163–1165`, `app/folha/porta_export.py:1192–1219`, `app/folha/porta_export.py:1241–1249`.) |
| Pedido `tentativa` | `pedir` e `assumir` sem UUID geram uma identidade nova. `assumir` com UUID preserva a do pedido. (`app/folha/porta_export.py:1010–1017`, `app/folha/porta_export.py:1072–1090`, `app/folha/porta_export.py:1111–1126`.) | `disparar` passa o UUID ao command. `assumir` o usa para adotar, `_ficha` e `concluir` o usam para condicionar a escrita, `_recusa` o inclui no objeto e `estado_da_porta` o compara ao carimbo. (`app/folha/porta_export.py:1015–1037`, `app/folha/porta_export.py:1045–1058`, `app/folha/porta_export.py:1111–1117`, `app/folha/porta_export.py:1160–1171`, `app/folha/porta_export.py:1247–1249`, `app/folha/porta_export.py:1292–1296`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:139–146`.) |
| Pedido `pid`, `backend_pid` | `assumir` grava `os.getpid()` e `pg_backend_pid()`. `disparar` grava `Popen.pid` na auditoria do lançamento. Durante `PEDIDO`, a linha ainda está sem os dois PIDs. (`app/folha/porta_export.py:1072–1079`, `app/folha/porta_export.py:1108–1126`, `app/folha/porta_export.py:1303–1315`.) | `vigiar` usa `backend_pid` em `medicao_viva`. Leitor operacional do `pid` do SO [nao verificado]. Os campos distinguem processo do SO e sessão do PostgreSQL. (`app/folha/porta_export.py:898–904`, `app/folha/porta_export.py:988–1001`, `app/folha/porta_export.py:1192–1196`.) |
| Pedido `resultado`, `n`, `motivo` | `concluir` grava os três via `_mudar_estado`. `_falhar`, chamado por `encerrar` e `vigiar`, grava `resultado='falhou'` e `motivo`. (`app/folha/porta_export.py:1056–1061`, `app/folha/porta_export.py:1130–1147`, `app/folha/porta_export.py:1160–1205`.) | O command e a emissão consomem o retorno de `concluir` ou o motivo recebido por `_falhar`. A porta lê `estado`, UUID e `carimbo.valor`. Leitor operacional dos três campos **persistidos** [nao verificado]. (`app/folha/porta_export.py:1056–1061`, `app/folha/porta_export.py:1170–1175`, `app/folha/porta_export.py:1241–1258`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:154–165`.) |
| Pedido `*_em` | `pedir` grava `pedido_em`, `assumir` no cron grava `em_curso_em`, `_mudar_estado` grava `<estado>_em` e atualiza `calculado_em`. O modelo também preenche `calculado_em` ao salvar. (`app/folha/porta_export.py:1030–1039`, `app/folha/porta_export.py:1072–1079`, `app/folha/porta_export.py:1121–1126`, `app/inteligencia/models.py:90–96`.) | `vigiar` lê `pedido_em` para o prazo. Leitores operacionais de `em_curso_em`, `concluido_em`, `falhou_em` e `calculado_em` para esse job [nao verificado]. (`app/folha/porta_export.py:1192–1219`.) |
| Carimbo `tentativa`, `erro_cartao_x_txt`, `valor` | `lavrar` grava os três. `valor` recebe `falhas` bruto ou `-1` se ausente. (`app/folha/porta_export.py:737`, `app/folha/porta_export.py:762–764`, `app/folha/porta_export.py:768–790`.) | `estado_da_porta` lê UUID e valor. `conferir` lê valor e listas. O teste lê `erro_cartao_x_txt` do carimbo. `resultado_de` lê esse erro do retorno de `medir`. Leitor operacional do erro já lavrado [nao verificado]. (`app/folha/porta_export.py:825–872`, `app/folha/porta_export.py:1136–1139`, `app/folha/porta_export.py:1247–1258`, `app/folha/tests/test_porta_sem_beco_item2.py:128–134`.) |

No caminho de produção localizado, a escrita dessas duas chaves fica em `pedir`, `assumir`, `_mudar_estado` e `lavrar`. Há escritas diretas em fixtures de teste. O escritor genérico de métricas exige chave registrada e a série genérica exige chave explícita. Uso dessas interfaces com as chaves da porta [nao verificado]. (`app/folha/porta_export.py:779–790`, `app/folha/porta_export.py:1030–1042`, `app/folha/porta_export.py:1072–1090`, `app/folha/porta_export.py:1121–1126`, `app/folha/tests/test_porta_do_export.py:33–42`, `app/folha/tests/test_porta_sem_beco_item2.py:49–52`, `app/inteligencia/metricas.py:36–67`.)

## 2. Quando a porta libera

- **Leitura estável.** Um `FALHOU` lido prevalece sobre verde antigo. Um `CONCLUIDO` com UUID diferente recusa. Já um carimbo zero sem pedido libera, e um par legado `CONCLUIDO` + carimbo zero sem UUID dos dois lados passa pela igualdade `None == None`. A presença dessas linhas em produção é [nao verificado]. (`app/folha/porta_export.py:875–887`, `app/folha/porta_export.py:1235–1258`.)
- **Corrida no caminho normal.** A conferência lê A como `CONCLUIDO`. O cron instala B, que entra em curso ou falha por exceção sem tocar no verde de A. A segunda consulta ainda encontra o carimbo de A, que combina com o pedido antigo em memória. `conferir` pode devolver `''` com B vigente. (`app/folha/porta_export.py:1118–1126`, `app/folha/porta_export.py:1178–1182`, `app/folha/porta_export.py:1238–1258`, `app/folha/porta_export.py:820–824`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:147–154`.)
- **Virada do dia.** O job do clique adota o `data_ref` do pedido e `concluir` usa a mesma data no carimbo. Mas `estado_da_porta` consulta `_hoje()` separadamente para pedido e carimbo. Se a primeira consulta cair antes da meia-noite e vier sem pedido, a segunda pode aceitar o verde do novo dia sem ler o pedido desse dia. `pedir` também relê o dia no `IntegrityError`. Além disso, `medir` fixa seu próprio instante e dia. Um job iniciado às 00h pode medir com a referência do dia novo e carimbar no dia do pedido, que é o anterior. (`app/folha/porta_export.py:21–23`, `app/folha/porta_export.py:313–338`, `app/folha/porta_export.py:469–476`, `app/folha/porta_export.py:1004–1007`, `app/folha/porta_export.py:1072–1089`, `app/folha/porta_export.py:1111–1117`, `app/folha/porta_export.py:1163–1171`, `app/folha/porta_export.py:1238–1249`.)
- **Escrita atrasada.** Nos caminhos normais, `encerrar` usa UPDATE condicionado ao estado e ao UUID. `concluir` trava a linha e exige `EM_CURSO` com o UUID original antes de lavrar. A tentativa atrasada que ainda carrega seu UUID perde para a sucessora. A corrida demonstrada acima está na leitura. (`app/folha/porta_export.py:1030–1042`, `app/folha/porta_export.py:1056–1061`, `app/folha/porta_export.py:1160–1182`.)

## 3. Pedido preso, perda de falha e R7

- **A checagem de sessão existe.** `pg_locks` exige trava `advisory` concedida no banco atual, as duas chaves, `objsubid=2` e o `backend_pid` gravado. Um PID reutilizado só produz falso vivo se a sessão nova também segurar a mesma trava. Falta uma identidade de nascimento da sessão. Já um worker vivo que perde a sessão pode ser declarado morto, e um processo travado que retém a sessão pode ficar `EM_CURSO` sem prazo. Em `PEDIDO`, um filho vivo que demore mais de 300 s para assumir pode ser declarado falho pelo prazo. Ocorrências em produção [nao verificado]. (`app/folha/porta_export.py:913–915`, `app/folha/porta_export.py:965–1001`, `app/folha/porta_export.py:1108–1126`, `app/folha/porta_export.py:1185–1206`.)
- **O cron ainda pode apagar o desfecho da tentativa.** `assumir` chama o vigia e em seguida faz `update_or_create` incondicional. Se o `PEDIDO` ainda estiver no prazo, ou se um clique criá-lo entre a leitura e o upsert, o cron pode substituir o UUID sem dar resultado à tentativa substituída. O filho atrasado perde a adoção. Se o vigia já gravou `FALHOU`, o upsert substitui o resultado no registro diário. O re-pedido também limpa o detalhe anterior. (`app/folha/porta_export.py:1085–1090`, `app/folha/porta_export.py:1111–1126`, `app/folha/porta_export.py:1198–1206`.)
- **Legado pode ficar aberto.** Para uma linha antiga com chave `tentativa` ausente, `_ficha` monta `None` e o UPDATE usa igualdade no JSON. O filtro por JSON `null` não seleciona a chave ausente no PostgreSQL. Isso pode bloquear a transição do vigia e o re-pedido pela tela. A ocorrência na base e o SQL executado [nao verificado]. (`app/folha/porta_export.py:1015–1017`, `app/folha/porta_export.py:1034–1042`, `app/folha/porta_export.py:1056–1058`, `app/folha/porta_export.py:1085–1090`.)
- **R7 tem observador sem tela, condicionado à execução da corrente.** O command varre todos os abertos do schema, inclusive de dias anteriores, antes de escolher os alvos. A corrente diária espera o marcador de `lavrar_cxr_do_motor`, pode sair sem executar o elo e chama o schema `juliani`. Uma morte após a varredura aguarda a próxima observação. A instalação e a regularidade dessa execução [nao verificado]. (`app/folha/porta_export.py:1209–1221`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:59–70`, `app/config/crons.py:616`, `app/config/crons.py:1793–1805`, `bin/encadeado.sh:78–104`.)

## 4. Recusa crítica e ESTADO

- **Pode haver zero.** Na corrida normal entre vigia, `encerrar` e `concluir`, só a transição terminal vencedora emite `porta_export`. A emissão vem depois do UPDATE ou do bloco atômico. Se houver morte ou falha nesse intervalo, o terminal já persistido fica fora da varredura de abertos e pode ficar sem linha. (`app/folha/porta_export.py:1034–1061`, `app/folha/porta_export.py:1162–1175`, `app/folha/porta_export.py:1215–1219`.)
- **Pode haver duas ou mais linhas físicas.** Um `DIVERGENTE` emite uma linha `porta_export`. Cada POST barrado pela mesma divergência emite também `folha:gerar_txt`, porque a view chama o envelope sem `recusas=()`. Dois POSTs dão três linhas críticas no total. O contador do export seleciona só `porta_export`. (`app/folha/porta_export.py:1045–1053`, `app/folha/porta_export.py:1172–1175`, `app/folha/porta_export.py:1271–1276`, `app/folha/views.py:130–135`, `app/folha/urls.py:4–14`, `app/core/respostas.py:41–56`, `app/core/recusa_critica.py:58–85`, `app/core/recusa_critica.py:113–139`.)
- **O ESTADO usa o leitor único.** Publica `porta do export falhou ou divergiu hoje: N`, esperado zero. Distingue arquivo principal ausente de zero, mas ignora rotados quando o principal falta. Se o principal existir e a leitura der `OSError`, o leitor pode produzir zero. Conta linhas, sem deduplicar UUID. (`bin/gerar_estado.py:265–270`, `bin/recusa_estado.py:24–34`, `bin/recusa_estado.py:60–77`, `app/core/recusa_critica.py:95–139`.)
- **O dia contado é o da emissão**, pelo prefixo `asctime`. O `dia=` no corpo é o dia da tentativa. Uma morte de ontem observada hoje conta hoje, se o dia local do host que gera o ESTADO coincidir com o do logger. O host usa `date.today()` e a aplicação declara `America/Sao_Paulo`. Alinhamento efetivo dos fusos e do arquivo em produção [nao verificado]. (`app/folha/porta_export.py:1045–1053`, `app/core/recusa_critica.py:46–50`, `app/core/recusa_critica.py:113–121`, `bin/recusa_estado.py:60–68`, `app/config/settings/saas.py:119–123`, `app/config/settings/saas.py:160–166`, `app/config/settings/base.py:92–95`.)

## 5. R8 e código de saída

**Sim para os dois casos pedidos**, se a persistência funcionar. `Exception` de `medir` e retorno classificado como `falhou` deixam a empresa em `FALHOU`, passam à seguinte e dão `SystemExit(2)` ao fim. Falhas na própria trava, em `assumir`, `encerrar`, `concluir` ou no processamento da saída ainda podem abortar a rodada. Uma falha descoberta só na varredura inicial fica fora de `falharam`, de modo que uma rodada com sucessoras verdes pode sair verde. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:59–70`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:128–165`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:190–211`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:251–267`.)

O `cartao_x_txt` continua global por competência. B chega a medir, mas uma divergência global, ou uma exceção que reaparece na varredura global ao passar por A, pode impedir B de ficar verde. Isso preserva o critério do anexo para “outra passa”. (`app/folha/porta_export.py:664–675`, `app/folha/porta_export.py:753–764`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:20–24`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:51–75`.)

## 6. Classes de reprovação e selos

- **Derivação paralela e liberação antes do pré-requisito.** `resultado_de` classifica o retorno e o pedido guarda essa classificação. `lavrar` grava o escalar bruto, e a porta interpreta seu sinal. Por exemplo, um retorno inconsistente com `falhas=0` pode gravar `FALHOU` com `carimbo.valor=0`. O pedido estável barra, mas a prova persistida fica contraditória e o caminho sem pedido aceita o escalar sozinho. A leitura dupla do dia e a corrida da seção 2 são falhas adicionais dessa costura. (`app/folha/porta_export.py:779–790`, `app/folha/porta_export.py:1130–1147`, `app/folha/porta_export.py:1162–1175`, `app/folha/porta_export.py:1238–1258`.)
- **Juiz registrado, escritor e ordem de trava.** `medir` consulta `classificar_export`, o juiz registrado. `resultado_de` valida a forma e a decomposição do retorno da medição. Não localizei duplicação de juiz registrado nem segundo escritor de produção do carimbo no caminho auditado. A ordem visível é trava de sessão, linha do pedido e carimbo. A view dispara o job fora do request e só segue para montar o TXT quando a porta devolve vazio. (`app/core/juizes.py:319–353`, `app/folha/porta_export.py:328`, `app/folha/porta_export.py:469–476`, `app/folha/porta_export.py:1162–1171`, `app/folha/porta_export.py:1265–1276`, `app/folha/porta_export.py:1292–1304`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:129–154`, `app/folha/views.py:124–146`.)
- **(a), (b) e R7.** Os testes exercitam a consulta da trava com PID gravado diferente, o `encerrar` atrasado, o bloqueio do verde velho em leitura estável, a persistência de divergência e falha e a varredura de uma linha de ontem. O teste de `concluir` atrasado deixa a sucessora já `CONCLUIDO`, então passa mesmo se cair só o filtro do UUID. O teste da meia-noite simula `Popen` e `medir`, cobre o vínculo pedido–carimbo e deixa de fora o relógio da medição e as duas consultas da porta. (`app/folha/tests/test_porta_sem_beco_item2.py:42–146`, `app/folha/tests/test_porta_sem_beco_item2.py:249–268`.)
- **(c), (d) e R8.** Os textos literais e a lista são cobrados em `conferir`. O teste chamado “clique repetido” também só chama `conferir`, portanto deixa passar a emissão extra do POST. O teste de ausência de arquivo sempre cria o arquivo, e o shell testa ausência só para o contador de Resolver Dia. R8 exercita a orquestração com `medir` simulado e exige `SystemExit` sem conferir `.code`. O teste de agregação do command simula também a persistência e a trava. (`app/folha/tests/test_porta_sem_beco_item2.py:53–57`, `app/folha/tests/test_porta_sem_beco_item2.py:158–247`, `app/folha/tests/test_porta_sem_beco_item2.py:273–289`, `bin/tests/test_recusa_estado.sh:24–47`, `app/relatorios/tests/test_selo_leitores_no_mesmo_numero.py:121–144`.)

## 7. O que segue aberto do `rev2.r`

- **(a) e R7, parcialmente.** UUID e CAS protegem o fechamento atrasado. Seguem a substituição incondicional pelo cron, o tratamento do legado e a diferença entre conferir a sessão e conferir o PID do processo. O observador sem tela existe na corrente declarada, sujeito ao gatilho. (`app/folha/porta_export.py:1015–1042`, `app/folha/porta_export.py:1108–1126`, `app/folha/porta_export.py:1160–1206`, `app/config/crons.py:1793–1805`, `bin/encadeado.sh:81–104`.)
- **(b), parcialmente.** Resultado por tentativa e vínculo do carimbo foram gravados. Seguem abertos o verde velho sob corrida, a aceitação sem UUID e a coerência do dia na borda da leitura. (`app/folha/porta_export.py:1162–1175`, `app/folha/porta_export.py:1238–1258`.)
- **(c), literal presente.** `conferir` devolve o texto de `FALHA` e mantém lista e frase de suporte em `DIVERGENTE`. No POST que encontra um `FALHOU` anterior, `abrir_a_porta` tenta reabrir antes de compor a resposta. Se a nova tentativa estiver pendente, o usuário recebe `EM_CONFERENCIA`. (`app/folha/porta_export.py:820–827`, `app/folha/porta_export.py:825–872`, `app/folha/porta_export.py:922–929`, `app/folha/porta_export.py:1271–1276`, `app/folha/views.py:130–135`.)
- **(d), parcialmente.** `erro_cartao_x_txt` e a linha do ESTADO entraram. Seguem abertas a garantia de uma linha por tentativa e a trilha terminal com ator, antes e depois. `_recusa` passa `request=None`, o emissor grava `usuario=-` e os eventos de auditoria visíveis dizem respeito ao disparo. **R8 fecha nos dois ramos de medição revisados**, com o critério global preservado. (`app/folha/porta_export.py:779–790`, `app/folha/porta_export.py:1045–1061`, `app/folha/porta_export.py:1172–1175`, `app/folha/porta_export.py:1305–1315`, `app/core/recusa_critica.py:70–85`, `bin/gerar_estado.py:267–270`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:147–165`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:251–259`.)

_7410 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
