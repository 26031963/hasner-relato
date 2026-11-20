# CODEX-ESTADO

> Ultima resposta processada: **19/11/2026 23:30** · publicado em **19/11/2026 23:31** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 19/11 23:30 · sessao 6a3bbfd9c659 · E4C-F5-CONSTROI · leitura · origem=Code

**A sonda atribui +6, +1, +6 e +2 SELECTs a sete sítios de leitura. Nenhuma das emissões listadas é ESCRITOR NOVO.** O `blame` atribui os prefetches à F5 volta A/A2 e a chamada avulsa à F5 A1. A volta 2 introduziu a leitura dos instantes pela sequência, que entrou na cópia sobre `main` pelo merge `3f1d63863`. O portão vi1 alterou testes. `atribuicao.txt:1`, `atribuicao.txt:12`, `atribuicao.txt:15`, `atribuicao.txt:26`, `commits.txt:1`, `commits.txt:2`, `commits.txt:7`, `commits.txt:8`, `commits.txt:9`, `commits.txt:10`, `app/chamados/models.py:4330`, `app/chamados/models.py:4357`.

Na tabela, as linhas de código são de `ec7134368`, salvo onde aparece `main`. A numeração de `atribuicao.txt` começa no primeiro cabeçalho de cenário. A sonda traz o prefixo do SQL e a pilha. A igualdade dos IDs entre consultas diferentes fica **[nao verificado]**.

| Cenário e linha da sonda | Emissão e chamadores em `ec7134368` | Commit da emissão | Classe e unidade de custo | O que o `main` lia e onde o lote pode ser reaproveitado |
|---|---|---|---|---|
| **1 fecha, +1** · `atribuicao.txt:2` | **A.** `prefetch_related('decisoes')` em [app/chamados/reconciliador.py:703](/home/ronald/codex_ws/ec71343686835c2e7460b5e25d4a7f3f886e0432/app/chamados/reconciliador.py:703), avaliado no `for` de `um_colaborador_por_identidade` (`app/chamados/juizes.py:1188`). Chamadores em `app/chamados/reconciliador.py:706` e `:635`. | `eaf69fe33`, **F5 A2**, lote K43. A1 liga o C6 ao compositor em `app/chamados/juizes.py:1408`. `commits.txt:2`, `commits.txt:7` | **LEITURA**, um lote de perguntas absolvíveis por chamada, +1 com 1 ou 3 fechamentos. `app/chamados/reconciliador.py:678`, `:702`, `atribuicao.txt:16` | **Substitui colunas** de instante e validação no C6. O cartório já tem decisões das perguntas com destino explícito em chamados de batida. O lote do E2 também pode conter órfãs e perguntas fora desses módulos. Reuso possível para a interseção com dados ainda atuais. Cobertura integral da emissão **[nao verificado]**. `main app/chamados/juizes.py:1389`, `app/chamados/reconciliador.py:678`, `:713`, `app/ponto/services/cartorio.py:145`, `:1026`, `app/chamados/models.py:3660` |
| **1 fecha, +1** · `atribuicao.txt:4` | **B.** `list(...prefetch_related('decisoes'))` de `_ancoradas` em [app/chamados/models.py:366](/home/ronald/codex_ws/ec71343686835c2e7460b5e25d4a7f3f886e0432/app/chamados/models.py:366). Chamadores `_plano_pergunta_viva` em `:400` e `_gate_pergunta_viva` em `:438`. | `18bf86f01`, **F5 volta A**, prefetch de `_ancoradas`. `commits.txt:8`, `app/chamados/models.py:366` | **LEITURA**, um lote por chamada do gate com ancoradas, +1 nos dois cenários de fecho. Pode crescer com o número de chamados que chegam ao gate com lote preenchido. `app/chamados/models.py:364`, `:400`, `:438`, `atribuicao.txt:18` | **Substitui colunas no C6**. O filtro de validação no `main` já consultava a sequência via SQL. O prefetch do cartório cobre destinos explícitos e o do E2 cobre as absolvíveis. Nenhum garante toda a âncora, que inclui órfãs pelo pai. O lote do próprio gate já chega a `_gravar_estado`. Eliminação integral deste SELECT por reuso **[nao verificado]**. `main app/chamados/models.py:351`, `:357`, `main app/chamados/juizes.py:1389`, `app/chamados/models.py:228`, `:361`, `:387`, `:401`, `app/chamados/reconciliador.py:703`, `app/ponto/services/cartorio.py:146` |
| **1 fecha, +1** · `atribuicao.txt:6` | **C.** Mesmo prefetch de `_ancoradas`, `app/chamados/models.py:366`. Chamador S143 em [app/chamados/services/disputa_emissao.py:1985](/home/ronald/codex_ws/ec71343686835c2e7460b5e25d4a7f3f886e0432/app/chamados/services/disputa_emissao.py:1985), sob o `lambda` de `:1920`. | `18bf86f01`, **F5 volta A**, prefetch compartilhado com B. A chamada S143 já estava na base. `commits.txt:8`, `main app/chamados/services/disputa_emissao.py:1983` | **LEITURA**, um lote de âncoras por chamada do resolver com resultado, +1 nos dois fechos. Pode crescer com o número de chamadas do S143 que encontram âncoras. `app/chamados/services/disputa_emissao.py:1985`, `atribuicao.txt:20` | **Substitui colunas no C6**, com sobreposição possível com B. O gate carrega em `app/chamados/models.py:400` e o cache é consumido em `:228`. Entre gate e S143 pode haver reaponte e gravação. O lote S128 de `app/chamados/services/disputa_emissao.py:1951` tem outro recorte e é avaliado em `:1986`, depois das âncoras. Reuso exige confirmar cobertura e frescor. Duplicação integral **[nao verificado]**. `main app/chamados/juizes.py:1389`, `app/chamados/models.py:456`, `:279`, `app/chamados/signals.py:117`, `app/chamados/services/disputa_emissao.py:1973` |
| **1 fecha, +1** · `atribuicao.txt:8` | **D.** `list(self.perguntas.all().prefetch_related('decisoes'))` de `DisputaSupervisao.fechar` em [app/chamados/models.py:2077](/home/ronald/codex_ws/ec71343686835c2e7460b5e25d4a7f3f886e0432/app/chamados/models.py:2077). Chamadores `app/chamados/reconciliador.py:663` e `app/ponto/services/cartorio.py:1026`. | `18bf86f01`, **F5 volta A**. O primeiro prefetch da sequência já existia na **F2**, `41620774bb`. `commits.txt:8`, `app/chamados/models.py:2077`, `main app/chamados/models.py:2001` | **LEITURA**, um lote por disputa que chega a esse trecho de `fechar`, +1 nos dois fechos. `app/chamados/models.py:2077`, `atribuicao.txt:22` | **Duplicada e evitável.** O `main` já carregava decisões para `respostas_sem_veredito()` e lia colunas no C6. Na cópia, `fechar()` chama `respostas_sem_veredito()` em `:2068`, que carrega a mesma coleção em `:1998` e `:2020`. As instâncias desse lote ficam no corpo da primeira leitura. Entregá-las ao C6 evita o segundo prefetch e mantém o compositor. `main app/chamados/models.py:2001`, `:2048`, `:2057`, `main app/chamados/juizes.py:1389`, `app/chamados/models.py:2018`, `:2023`, `:2077`, `:2078` |
| **1 fecha, +2** · `atribuicao.txt:10` | **E.** Prefetch aninhado `perguntas_que_resolvem__decisoes` em [app/ponto/services/cartorio.py:146](/home/ronald/codex_ws/ec71343686835c2e7460b5e25d4a7f3f886e0432/app/ponto/services/cartorio.py:146). Chamador `_julgar_colab_corpo` em `:1347`. `julgar_colab` chama o corpo em `:1015`, `:1048` e, condicionalmente, `:1113`. O par exato da sonda **[nao verificado]**. | `eaf69fe33`, **F5 A2**, lote K24. `commits.txt:2`, `app/ponto/services/cartorio.py:146` | **LEITURA**, um lote por execução do corpo que chega à carga e tem perguntas, duas emissões nos dois cenários de fecho. `app/ponto/services/cartorio.py:145`, `:1347`, `atribuicao.txt:24` | **Substitui colunas**. O `main` prefetchava as perguntas e o C6 lia seus campos. Na cópia, o primeiro lote serve aos leitores do corpo. O segundo usa o mesmo recorte por colaborador e módulos, mas a carga é local a cada passada e há carimbo/fecho entre passadas. Reuso do primeiro lote só para membros e decisões ainda atuais. Duplicação integral **[nao verificado]**. `main app/ponto/services/cartorio.py:147`, `main app/chamados/juizes.py:1389`, `app/ponto/services/cartorio.py:1026`, `:1033`, `:1048`, `:1347`, `:1417`, `:1689` |
| **1 FICA, +1** · `atribuicao.txt:13` | **F.** `list(do_fato(...).select_related(...).prefetch_related('decisoes'))` em [app/chamados/services/fio.py:1964](/home/ronald/codex_ws/ec71343686835c2e7460b5e25d4a7f3f886e0432/app/chamados/services/fio.py:1964). Chamadores `_nascer` em `app/chamados/models.py:3119` e `_sob_contrato` em `:3027`. | `eaf69fe33`, **F5 A2**, lote K17. A1 converte a eleição da canônica em `app/chamados/services/fio.py:1288`. `commits.txt:2`, `commits.txt:7` | **LEITURA**, um lote por chamada de `universo_do_fato` com candidatas, +1 com 1 ou 3 chamados que ficam. `app/chamados/services/fio.py:1963`, `atribuicao.txt:29` | **Substitui a coluna `respondida_em`** na eleição da canônica. O primeiro lote para esse recorte é carregado aqui e semeia `_viz`. A ponte anterior pode ter carregado vizinhas em `:1595` e guardado em `:1848`. Reuso sem nova derivação depende da mesma chave do `do_fato`. Lote anterior equivalente nesta execução **[nao verificado]**. `main app/chamados/services/fio.py:1283`, `:1958`, `app/chamados/services/fio.py:1288`, `:1811`, `:1968`, `:2001`, `app/chamados/models.py:3087` |
| **3 fecham, +1** · `atribuicao.txt:16` | **A**, mesmos sítio e chamadores. | `eaf69fe33`, **F5 A2**, como A. | **LEITURA**, +1 por lote, como A. | Mesma substituição de colunas e mesmo limite de cobertura de A. |
| **3 fecham, +1** · `atribuicao.txt:18` | **B**, mesmos sítio e chamadores. | `18bf86f01`, **F5 A**, como B. | **LEITURA**, +1 por lote do gate, como B. | Mesma substituição de colunas e mesmo limite de cobertura de B. |
| **3 fecham, +1** · `atribuicao.txt:20` | **C**, mesmos sítio e chamadores. | `18bf86f01`, **F5 A**, como C. | **LEITURA**, +1 por lote do resolver, como C. | Mesma substituição de colunas e mesma necessidade de confirmar frescor de C. |
| **3 fecham, +1** · `atribuicao.txt:22` | **D**, mesmos sítio e chamadores. | `18bf86f01`, **F5 A**, como D. | **LEITURA**, +1 por lote da disputa, como D. | Mesma duplicação evitável de D. `app/chamados/models.py:2020`, `:2068`, `:2077` |
| **3 fecham, +2** · `atribuicao.txt:24` | **E**, mesmos sítio e chamadores. | `eaf69fe33`, **F5 A2**, como E. | **LEITURA**, +2 por passadas do corpo, como E. | Mesma substituição de colunas e mesma condição de frescor de E. |
| **3 FICAM, +1** · `atribuicao.txt:27` | **G.** Avaliação de `list(p.decisoes.all())` em [app/chamados/models.py:4330](/home/ronald/codex_ws/ec71343686835c2e7460b5e25d4a7f3f886e0432/app/chamados/models.py:4330). Chamadores `composta` em `:4357` e `lavrar_sem_turno_aberto` em `app/chamados/reconciliador.py:367`. | `0db6afc73`, **F5 A1**, chamada corrente de `lavrar_sem_turno_aberto`. `e673e0efe`, **F5 volta 2**, introduziu a leitura no juiz, incorporada pelo merge `3f1d63863`. `commits.txt:7`, `:9`, `:10`, `app/chamados/reconciliador.py:367` | **LEITURA avulsa**, uma por pergunta persistida que chega sem cache. Nesta pilha, zero com 1 chamado que fica e +1 com 3. O código permite crescer por pergunta sem cache. `app/chamados/models.py:4326`, `:4330`, `atribuicao.txt:12`, `:27` | **Substitui a coluna `respondida_em`**. A varredura de órfãs já entrega o prefetch em `app/chamados/reconciliador.py:1575`, `:1580`. A emissão também pode chamar a porta com a pergunta recém-criada, em `app/chamados/services/disputa_emissao.py:468`, `:473`. Nesse ramo, as linhas reais retornadas por `registrar` já estão disponíveis no nascimento e o retorno é descartado. Repassá-las evitaria a leitura sem derivação paralela. O anexo termina em `lavrar_sem_turno_aberto`, então o ramo exato **[nao verificado]**. `main app/chamados/reconciliador.py:366`, `main app/chamados/juizes.py:1552`, `app/chamados/reconciliador.py:1673`, `app/chamados/models.py:3231`, `:4366`, `:4381` |
| **3 FICAM, +1** · `atribuicao.txt:29` | **F**, mesmos sítio e chamadores. | `eaf69fe33`, **F5 A2**, como F. | **LEITURA**, +1 por lote de candidatas, como F. | Mesma substituição de coluna e mesma condição de chave de F. |

| Cenário | Soma das linhas | Medição conferida |
|---|---:|---:|
| 1 chamado fecha | `1 + 1 + 1 + 1 + 2 = 6` | `236 − 230 = 6` · `atribuicao.txt:1` |
| 1 chamado FICA | `1 = 1` | `146 − 145 = 1` · `atribuicao.txt:12` |
| 3 chamados fecham | `1 + 1 + 1 + 1 + 2 = 6` | `300 − 294 = 6` · `atribuicao.txt:15` |
| 3 chamados FICAM | `1 + 1 = 2` | `221 − 219 = 2` · `atribuicao.txt:26` |

O `+7/+2/+7/+3` do item 2 do aval excede em uma consulta por cenário tanto a soma das linhas da sonda quanto a diferença medida. A origem desse descompasso **[nao verificado]**. `f5_chokepoint_passagem_1911_2325.txt:3`, `atribuicao.txt:1`, `:12`, `:15`, `:26`.

## 19/11 21:12 · sessao 6d7b3f3fd0af · E4C-F5-CONSTROI · construtor · origem=Code

A A2 acrescenta lote a **nove carregadores**. O K15 já vinha em lote, e o O261 conserva a releitura por candidata sob trava. Os dois testes de igualdade de consultas permaneceram intactos. A sintaxe dos dez arquivos e dos nove mutantes foi conferida por AST, e a contagem de `assert` e `self.assert*` ficou igual à base em todos os módulos existentes tocados. Banco, suíte e morte dos mutantes ficam para o Code e o Opus final [nao verificado]. `chamados/models.py:364`, `chamados/services/relavra_via.py:230`, `chamados/tests/test_e2_tarefa_do_dia.py:938`, `escala/tests/test_f3_beco1_folga_na_lista.py:402`.

### Carregadores e arquivos tocados

| K | Base | A2 |
|---|---|---|
| 1 | Mapa temporal em lote, instâncias sem lote. `chamados/services/tarefa_do_dia.py:390`, `:530` | `Prefetch` nas instâncias, com a junção de `absorcao_ausencia` reaproveitada no mapa temporal. `chamados/services/tarefa_do_dia.py:537`, `:545` |
| 2 | Sem lote. `chamados/juizes.py:1662` | `alimentar_portas` entrega as instâncias com decisões. `chamados/juizes.py:1663` |
| 9 | Sem lote. `chamados/reconciliador.py:1574` | Prefetch na consulta que materializa as reatáveis. `chamados/reconciliador.py:1575` |
| 10 | Sem lote. `chamados/reconciliador.py:1102` | Prefetch nas órfãs abertas. As duas varreduras com `iterator` passam blocos explícitos de 2.000. `chamados/reconciliador.py:1103`, `:1245`, `:1356` |
| 15 | Já em lote. `chamados/models.py:366` | Mesmo carregador. O selo mede **2 consultas com 1 e com 5 ancoradas**, conferindo os PKs e a forma do SQL. `chamados/tests/test_e4c_f2_leitura_do_ato.py:578`, `:584`, `:589` |
| 16 | Sem lote. `chamados/services/disputa_emissao.py:1955` | Prefetch nas pendentes do S128. `chamados/services/disputa_emissao.py:1955` |
| 17 | Sem lote. `chamados/services/fio.py:1964` | Prefetch nas sementes do universo do fato. `chamados/services/fio.py:1964` |
| 18 | Sem lote. `chamados/services/fio.py:1597` | Prefetch nos vizinhos do fato. `chamados/services/fio.py:1597` |
| 24 | Prefetch só de `perguntas_que_resolvem`. `ponto/services/cartorio.py:147` | Prefetch de `perguntas_que_resolvem__decisoes`. `ponto/services/cartorio.py:146` |
| 43 | Sem lote. `chamados/reconciliador.py:702` | Prefetch na consulta que materializa as absolvíveis. `chamados/reconciliador.py:703` |

Além dos seis módulos de produção da tabela e do selo de ancoradas, foram tocados `chamados/tests/test_o261_julgamento_desfaz_carimbo.py:187`, `chamados/tests/test_e4c_f5_voltaA_lotes.py:22` e `chamados/tests/e4c_f5_voltaA_mutantes.py:9`. O selo O261 compara `6 × Q(3) == 3 × Q(6)` e limita a **9 consultas por candidata**. O caso novo mede separadamente o SELECT de decisões por candidata e confere os PKs julgados. `chamados/tests/test_o261_julgamento_desfaz_carimbo.py:203`, `chamados/tests/test_e4c_f5_voltaA_lotes.py:162`.

### Origem das consultas extras

- **Tarefa, 36 × 30.** A fixture acrescenta duas perguntas. A leitura do código aponta três SELECTs avulsos por pergunta no percurso medido: partição do app, preparação do slot e entrada do C6. Os três chegam a `DecisaoPergunta.leitura` pelas instâncias de K1. O mapa temporal já usava lote, mas guardava as leituras separadamente. Agora a mesma carga abastece o mapa e o cache das instâncias. A atribuição dos seis SELECTs é por código e pelo total raspado, sem captura individual [nao verificado]. `chamados/tests/test_e2_tarefa_do_dia.py:921`, `:932`, `chamados/services/pergunta_no_app.py:88`, `chamados/services/carga_pergunta_viva.py:119`, `chamados/juizes.py:351`, `:1408`, `chamados/models.py:4330`, `chamados/services/tarefa_do_dia.py:537`, `:545`.
- **C11, 56 × 48.** A fixture acrescenta quatro dias com uma pergunta respondida por dia. Cada dia passa pelo C6 com a instância de K2 e pela partição com outra instância, de K1. O `all` de M1 ainda recebe o ORM e lê seus atributos diretamente. Os dois lotes cobrem as duas origens. A atribuição dos oito SELECTs é por código e pelo total raspado, sem captura individual [nao verificado]. `escala/tests/test_f3_beco1_folga_na_lista.py:104`, `:395`, `escala/services/cadastro_realidade.py:460`, `:466`, `:472`, `:502`, `chamados/juizes.py:1391`, `:1663`, `:1667`, `chamados/services/tarefa_do_dia.py:537`, `:626`.

### Testes e mutantes [nao verificado]

O RED compara apenas SELECTs cuja tabela principal é `chamados_decisaopergunta` e exige igualdade exata entre os PKs da fixture e os recebidos pelo compositor. No cartório, a validação de todas as perguntas força o `any` a percorrer a coleção inteira. `chamados/tests/test_e4c_f5_opus_models.py:25`, `chamados/tests/test_e4c_f5_voltaA_lotes.py:22`, `:28`, `:137`.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  chamados.tests.test_e4c_f5_voltaA_lotes \
  chamados.tests.test_e2_tarefa_do_dia.SeloDeterminismoERelogio.test_lote_tem_queries_constantes \
  escala.tests.test_f3_beco1_folga_na_lista.F3Beco1NaListaTest.test_MORDE_F5_queries_nao_crescem_com_os_dias \
  chamados.tests.test_o261_julgamento_desfaz_carimbo.JulgamentoDesfazCarimboSemLastroTest.test_custo_por_candidata_e_CONSTANTE \
  chamados.tests.test_e4c_f2_leitura_do_ato.AncoradasCustoTest \
  chamados.tests.test_e4c_f5_volta2_juizes.JuizesVolta2Test.test_red_8_custo_montar_chamados_fio_uma_e_cinco

PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  chamados escala.tests.test_f3_beco1_folga_na_lista \
  ponto.tests.test_selo_chokepoint_escrita
```

Cada invocação abaixo retira um único lote em memória. A morte esperada é a desigualdade entre 3 e 6 em `chamados/tests/test_e4c_f5_voltaA_lotes.py:37` [nao verificado]. `chamados/tests/e4c_f5_voltaA_mutantes.py:22`, `:51`.

```bash
muta() {
  F5_VOLTA_A_MUTANTE="$1" PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
    "chamados.tests.test_e4c_f5_voltaA_lotes.$2"
}
muta k1  TarefaEmLoteTest.test_RED_k1_tarefa
muta k2  CarregadoresEmLoteTest.test_RED_k2_portas
muta k9  CarregadoresEmLoteTest.test_RED_k9_reataveis
muta k10 CarregadoresEmLoteTest.test_RED_k10_orfas_abertas
muta k16 CarregadoresEmLoteTest.test_RED_k16_pendentes_s128
muta k17 CarregadoresEmLoteTest.test_RED_k17_fio_universo_do_fato
muta k18 CarregadoresEmLoteTest.test_RED_k18_fio_vizinhos
muta k24 CarregadoresEmLoteTest.test_RED_k24_cartorio
muta k43 CarregadoresEmLoteTest.test_RED_k43_absolviveis
```

| Mutante | Caso que deve morrer |
|---|---|
| K1, K2 | Tarefa `chamados/tests/test_e4c_f5_voltaA_lotes.py:42` e portas `:85`, respectivamente |
| K9, K10 | Reatáveis `chamados/tests/test_e4c_f5_voltaA_lotes.py:94` e órfãs abertas `:103`, respectivamente |
| K16 | Pendentes S128. `chamados/tests/test_e4c_f5_voltaA_lotes.py:110` |
| K17, K18 | Universo `chamados/tests/test_e4c_f5_voltaA_lotes.py:121` e vizinhos `:129`, respectivamente |
| K24, K43 | Cartório `chamados/tests/test_e4c_f5_voltaA_lotes.py:137` e absolvíveis `:151`, respectivamente |

As contraprovas da A1 para `isinstance`, argumento ORM cru no C6 e composição manual do fio já estão nestes casos. `chamados/tests/test_e4c_f5_selo_chamada.py:269`, `:298`, `chamados/tests/test_e4c_f5_voltaA_sitios.py:65`.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  chamados.tests.test_e4c_f5_selo_chamada.SeloPurezaTest.test_MORDE_tipo_modelo_e_consulta \
  chamados.tests.test_e4c_f5_selo_chamada.SeloChamadaTest.test_MORDE_retirar_compositor_de_cada_chamada_da_A1

CORTES_F2_MUTANTE=a1_fio_composicao_local PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  chamados.tests.test_e4c_f5_voltaA_sitios.SitiosVoltaATest.test_MORDE_fio_composicao_local
```

### Quebras previstas fora da lista

O K16 chama `prefetch_related` sobre a consulta real. O harness do S128 recompila o corpo com `Linhas`, que implementa `select_related` e carece de `prefetch_related`. Prevejo `AttributeError` nos casos que passam por `AlcanceTest.resolver`, inclusive o coletor de mutantes e os consumidores em memória abaixo. Esses testes ficaram intactos [nao verificado]. `chamados/services/disputa_emissao.py:1955`, `chamados/tests/test_e4b_classe_e_memoria.py:165`, `:190`, `:294`, `:371`.

- `chamados/tests/test_e4b_classe_e_memoria.py:213`, `:255`, `:395`, `:408`, `:416`, `:422`, `:436`, `:441`, `:448`, `:465`, `:484`, `:501`, `:527`, `:533`, `:539`, `:545`, `:551`, `:558`, `:567`, `:573`, `:586`, `:952`, `:1003`, `:1033`.
- `chamados/tests/test_e4b_c4_tranca_memoria.py:59`, `chamados/tests/test_e4b_c4v14_memoria.py:110` e `chamados/tests/test_e4b_c6_memoria.py:87`, `:120`.

### Quem escreve e quem lê

| Campo ou cache | Escrita, leitura e significado |
|---|---|
| `decisoes` | A FK declara a sequência da pergunta. Resposta, validação, devolução e reabertura usam `registrar`, que grava as linhas. `leitura` consome `p.decisoes.all()` e o compositor mantém instante e validação do ato e texto da projeção. K1 entrega a sequência integral também ao mapa temporal, cujo recorte continua em `agora`. `chamados/models.py:4188`, `:4330`, `:4357`, `:4372`, `chamados/services/disputa_emissao.py:1532`, `:1639`, `chamados/services/validacao.py:173`, `:279`, `chamados/services/tarefa_do_dia.py:393`, `:545` |
| `_prefetched_objects_cache['decisoes']` | O ORM popula o cache de cada instância carregada. O gerenciador reverso o serve a `leitura` e ao mapa de K1. `registrar` invalida a chave na instância recebida como `pergunta`. Nessas cargas a chave guarda a sequência integral. A cópia da ressuscitação tem um cache próprio, com o recorte de simulação já existente. `chamados/models.py:1585`, `:1594`, `:4326`, `:4375`, `chamados/services/tarefa_do_dia.py:537`, `:545` |

**LEI-AKITA** origem=`chamados/models.py:4317`, `:4351` · testemunha=`chamados/tests/test_e4c_f5_voltaA_lotes.py:22` · RED=3 × 6 por lote e por candidata no O261 · juízes novos=0 · padrão=juiz único puro + fonte única (compositor) + fitness function.

## 19/11 20:27 · sessao 0e74903303d8 · CADEIA-20-PASSO-1 · leitura · origem=Code

## 1. Setembro — quem marca `orfa`

**A cópia marca `Batida.orfa` em `_coletar_dados_espelho`, usando `orfas_grade` do espelho.** `montar_dias` pega a `CelulaDia` da data e chama `sobras_da_ata(cel.ata, data, empresa)`. O PDF conta as ocorrências de cada `HH:MM` em `orfas_grade` e consome uma ocorrência a cada batida marcada. [app/ponto/services/espelho.py:704](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/services/espelho.py:704) · [app/relatorios/pdf_espelho.py:595](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/relatorios/pdf_espelho.py:595)

No pai, o PDF chamava `grade_espelho_janela` com as batidas do período, montava um `set` de horas órfãs por dia e marcava toda batida cujo `HH:MM` estivesse no conjunto. A volta 3 trocou tanto a fonte, da grade remontada para `ata['orfas']`, quanto a regra para horas repetidas, de pertencimento ao conjunto para contagem de ocorrências. [Pai, app/relatorios/pdf_espelho.py:543](/home/ronald/codex_ws/7efb46e8e04a977c58923a61c53d8572b5abec83/app/relatorios/pdf_espelho.py:543) · [Pai, app/relatorios/pdf_espelho.py:605](/home/ronald/codex_ws/7efb46e8e04a977c58923a61c53d8572b5abec83/app/relatorios/pdf_espelho.py:605) · [app/escala/utils.py:2384](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/escala/utils.py:2384) · [app/escala/services/leitor_celula.py:198](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/escala/services/leitor_celula.py:198)

**Há um ramo `< 5`, mas ele só muda o texto** para `razao nao lavrada, anterior ao horizonte` quando falta razão ou estado. A sobra é devolvida com a mesma hora, e o PDF usa essa hora para marcar a batida. A escolha entre o caminho do pai e o da cópia é feita sem guarda por versão. Isso permite mudar o cartão com a ata intacta, como no agregado de setembro. O mecanismo de cada uma das 63 diferenças, incluindo quais envolveram horas repetidas, fica **[nao verificado]**. [app/escala/services/leitor_celula.py:207](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/escala/services/leitor_celula.py:207) · [app/relatorios/pdf_espelho.py:595](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/relatorios/pdf_espelho.py:595) · `cartao_diff.txt:81` · `copia_sem_cartao.txt:14` · `copia_sem_cartao.txt:24`

## 2. Outubro e novembro — horas previstas, saldo e turnos

- **`horas_previstas` e `saldo`.** O coletor soma `minutos_previstos_do_dia` para as datas retornadas por `previstas_do_periodo` e divide por 60. Para uma janela reconhecida como competência, `_folha_manda` pode sobrescrever o previsto com os minutos do fechamento lido na hora ou, na falta desse total, com os da lavratura `DiaPago`. O fechamento lido soma `minutos_previstos` dos dias de trabalho e ausência da grade, que lê `tipo_dia` e `minutos_previstos` da ata. O saldo é refeito com as horas trabalhadas menos as previstas. [app/relatorios/pdf_espelho.py:374](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/relatorios/pdf_espelho.py:374) · [app/relatorios/pdf_espelho.py:459](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/relatorios/pdf_espelho.py:459) · [app/relatorios/cartao_pela_celula.py:69](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/relatorios/cartao_pela_celula.py:69) · [app/relatorios/cartao_pela_celula.py:321](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/relatorios/cartao_pela_celula.py:321) · [app/ponto/services/fechamento.py:1046](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/services/fechamento.py:1046) · [app/folha/export.py:329](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/folha/export.py:329) · [app/escala/services/leitor_celula.py:430](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/escala/services/leitor_celula.py:430)
- **`turnos`.** É `len(turnos_do_colab(colaborador, data_ini, data_fim))`. O pareador recebe os papéis por minuto derivados da ata, via `papel_por_minuto_da_ata` e `periodos_do_dia`, que leem as lâmpadas lavradas e o DNA. Uma relavra que mude esses papéis pode mudar a contagem de turnos. [app/relatorios/pdf_espelho.py:453](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/relatorios/pdf_espelho.py:453) · [app/ponto/turnos.py:1944](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/turnos.py:1944) · [app/ponto/turnos.py:1997](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/turnos.py:1997) · [app/ponto/juiz_batida.py:149](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/juiz_batida.py:149)

O diff do pai mantém as fórmulas de `horas_previstas` e `turnos`. A projeção nova acrescenta `sobras` à grade, e o PDF usa `orfas_grade` para marcar batidas e filtrar **`turnos_abertos`**. A mudança da volta 3 que pode atingir os números perguntados é a v5 no hash, que põe as células de outubro em diante na fila de relavra. O P1 registrou 34.641 mudanças no par `ata`/`veredito` em outubro e novembro. A sonda lê os cartões só depois do P2 e guarda os totais numéricos. Para 191, 203 e 922, a fonte final do previsto e os dias que explicam a queda ficam **[nao verificado]**. Para 87, quais atas alteraram o pareamento e se isso ocorreu no P1 ficam **[nao verificado]**. [Pai, app/relatorios/pdf_espelho.py:451](/home/ronald/codex_ws/7efb46e8e04a977c58923a61c53d8572b5abec83/app/relatorios/pdf_espelho.py:451) · [app/relatorios/pdf_espelho.py:453](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/relatorios/pdf_espelho.py:453) · [app/relatorios/pdf_espelho.py:514](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/relatorios/pdf_espelho.py:514) · [app/escala/services/leitor_celula.py:451](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/escala/services/leitor_celula.py:451) · [app/ponto/services/cartorio.py:163](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/services/cartorio.py:163) · [app/ponto/services/cartorio.py:521](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/services/cartorio.py:521) · `copia_sem_cartao.txt:14` · `sonda.py:135` · `sonda.py:168` · `cartao_diff.txt:8` · `cartao_diff.txt:61` · `cartao_diff.txt:71`

## 3. P2 — impressão muda com a ata igual

O caminho é `_julgar_colab_corpo` → `impressao_insumos` → `lavrar_veredito(save=False)` → `carimbar`. O cron recalcula o hash dos insumos e, se ele divergir, a célula passa pela guarda mesmo sem `--forcar`. A lavra compara a ata nova com a anterior e só inclui `ata` nos campos a gravar quando ela difere. `carimbar` grava a impressão nova mesmo quando `ata` fica fora dessa lista. O hash inclui, entre outros insumos, status dos chamados, fio vivo e turno ainda em curso. [app/ponto/services/cartorio.py:502](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/services/cartorio.py:502) · [app/ponto/services/cartorio.py:1540](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/services/cartorio.py:1540) · [app/ponto/services/cartorio.py:2012](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/services/cartorio.py:2012) · [app/ponto/portas/celula.py:1053](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/portas/celula.py:1053) · [app/ponto/portas/celula.py:1189](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/portas/celula.py:1189)

O comando também pode rejulgar internamente com `forcar=True` as células enfileiradas pela reconciliação. A sonda registra oito julgamentos na cópia, zero no MAIN e cinco impressões finais diferentes em novembro. Como ela guarda contagens e hashes, qual insumo mudou em cada célula e se alguma passou pela reconciliação interna ficam **[nao verificado]**. [app/ponto/services/cartorio.py:1147](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/services/cartorio.py:1147) · `copia_sem_cartao.txt:18` · `copia_sem_cartao.txt:24` · `main_sem_cartao.txt:13` · `sonda.py:57` · `sonda.py:168`

## 4. Agosto — como o cron alcança ata anterior ao horizonte

`processar_cartorio` carrega todas as células da empresa com `data < hoje`. Em agosto, `versao_para` devolve `None`, então a v5 fica fora do hash. A célula antiga ainda pode entrar no julgamento comum se faltar carimbo ou veredito, ou se outro insumo mudar a impressão. Uma vez nesse caminho, `_lavrar_razoes_sobras` roda sem guarda por versão e pode acrescentar `razoes_orfas`, inclusive `[]` quando há insumos e nenhuma sobra. [app/ponto/management/commands/processar_cartorio.py:51](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/management/commands/processar_cartorio.py:51) · [app/ponto/management/commands/processar_cartorio.py:63](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/management/commands/processar_cartorio.py:63) · [app/ponto/services/cartorio.py:160](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/services/cartorio.py:160) · [app/ponto/services/cartorio.py:295](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/services/cartorio.py:295) · [app/ponto/services/cartorio.py:1552](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/services/cartorio.py:1552) · [app/ponto/services/cartorio.py:962](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/services/cartorio.py:962) · [app/ponto/services/cartorio.py:2015](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/services/cartorio.py:2015)

O P1 registra uma impressão diferente em agosto em cada árvore e uma diferença no hash conjunto de `ata` e `veredito` na cópia. Se era a mesma célula nas duas árvores, qual insumo a fez entrar na fila e qual campo mudou ficam **[nao verificado]**. `main_sem_cartao.txt:9` · `copia_sem_cartao.txt:14` · `sonda.py:60`

## 19/11 20:24 · sessao 27d6f7e885de · E4C-F5-CONSTROI · leitura · origem=Code

### 1. O argumento de `pergunta_respondida`

Quem entrega o argumento é `_pergunta_viva_calc`. A função monta `_lido = DecisaoPergunta.composta(p)`, aplica o recorte temporal e chama `pergunta_respondida(_lido._replace(respondida_em=respondida, resposta_colab=resposta))`. O argumento continua sendo um `chamados.models.LeituraComposta`, a `namedtuple` de cinco campos devolvida por `composta`. É esse tipo que faz o `isinstance(..., SimpleNamespace)` falhar. [juizes.py:1408](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/juizes.py:1408) · [juizes.py:1428](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/juizes.py:1428) · [models.py:24](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/models.py:24)

`git log -S` e `blame` atribuem o assert a `e673e0efe1` (volta 2). Naquele commit, `projecao_temporal_da_pergunta` lia os instantes do ato para a instância ORM e mantinha o texto da projeção. O calc empacotava o par recortado em `SimpleNamespace(respondida_em=respondida, resposta_colab=resposta)`. O assert verificava a entrega desse par já projetado ao juiz. Naquele código, entregar a instância ORM faria `pergunta_respondida` chamar `leitura` de novo. Os asserts vizinhos verificavam uma leitura e uma chamada ao juiz por pergunta, com zero queries no cálculo após o prefetch. A volta A (`0db6afc73`) trocou o `SimpleNamespace` pelo `_replace` da leitura composta. `e673e0efe1:app/chamados/tests/test_e4c_f5_volta2_juizes.py:127` · `e673e0efe1:app/chamados/juizes.py:1389`, `:1430`, `:1559` · [teste atual:129](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/tests/test_e4c_f5_volta2_juizes.py:129)

### 2. O `1 contra 5` é do `calc`

O traceback de `red8_falhas.txt` mostra a falha em `calc.call_count >= quantidade`, na linha 163. A checagem de `leitor.call_count` vem na linha 164 e ficou sem ser executada nessa medição. `calc >= 5` verde com `leitor == 1` está **[nao verificado]**. [test_e4c_f5_volta2_juizes.py:159](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/tests/test_e4c_f5_volta2_juizes.py:159)

- **Carga.** As cinco perguntas da fixture pertencem à mesma disputa aberta e têm o mesmo chamado de destino. O fio carrega todas as perguntas das abertas com prefetch de `decisoes` e as anexa ao cartão de destino. A chamada a `pergunta_viva(p)` durante essa carga fica no ramo das disputas fechadas. [teste:119](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/tests/test_e4c_f5_volta2_juizes.py:119) · [teste:170](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/tests/test_e4c_f5_volta2_juizes.py:170) · [models.py:1979](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/models.py:1979) · [fio.py:251](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/services/fio.py:251)
- **Vida do cartão.** `tem_pergunta_viva` usa `any(...)`. A primeira pergunta da fixture dá `viva=True`: tem resposta, o teste força `_resposta_concorda_com_a_luz=False` e `_sem_acao` devolve `False` para uma pergunta sem materialização. O `any` para ali. Assim, as outras quatro pulam o **calc** nessa etapa. [fio.py:319](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/services/fio.py:319) · [models.py:3650](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/models.py:3650) · [juizes.py:1428](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/juizes.py:1428) · [raiox.py:182](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/services/raiox.py:182) · [teste:176](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/tests/test_e4c_f5_volta2_juizes.py:176)
- **Resposta de cada pergunta.** O laço de `resposta_enviada` passa pelas cinco via `espera_o_texto_do_colaborador → composta → leitura`, sem chamar o calc. O agregado `aguarda_o_colaborador` volta a ler cada pergunta para montar o recorte e chama `fio_vivo`, substituído por `False` no teste. `creditos_perguntas` também está substituído. Só o laço de `resposta_enviada` já implica pelo menos cinco chamadas a `leitura` sob o patch. [fio.py:173](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/services/fio.py:173) · [fio.py:189](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/services/fio.py:189) · [fio.py:358](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/services/fio.py:358) · [fio.py:388](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/services/fio.py:388) · [teste:179](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/tests/test_e4c_f5_volta2_juizes.py:179)
- **Patch e caches.** A property `.viva` usa o `pergunta_viva` reexportado em `models.py`. Essa função ainda resolve `_pergunta_viva_calc` no módulo `juizes`, onde o teste aplicou o patch. O memo `_pv_memo` pode pular o calc inteiro em uma nova consulta à mesma instância. Quando o calc roda, ele chama `composta` antes das guardas, e `composta` resolve `cls.leitura(p)` na classe sob patch. O prefetch fornece as linhas a `p.decisoes.all()` dentro da leitura. Nenhum desses caminhos produz cinco chamadas ao calc com uma só chamada ao leitor. [models.py:4507](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/models.py:4507) · [juizes.py:893](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/juizes.py:893) · [juizes.py:1406](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/juizes.py:1406) · [models.py:4330](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/models.py:4330) · [models.py:4357](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/models.py:4357)

### 3. Âncora de mutante ausente

Das 42 âncoras, só `rouba_respondida_do_hx` tem zero ocorrências. A busca literal ainda procura `pergunta_respondida(p) or not pergunta_viva(p, hoje=hoje)`. O sítio atual é `if pergunta_respondida(DecisaoPergunta.composta(p)) or not pergunta_viva(p, hoje=hoje):  # L-130: o juiz`. As outras 41 âncoras têm uma ocorrência cada, inclusive `pula_c6` e `perde_hoje_c6`, que procuram trechos dessa mesma linha. O `0 != 1` ocorre na checagem da âncora, antes de aplicar o mutante. O `PicklingError` vem depois, ao reportar o subteste, conforme `mutantes_mordem.txt`. [test_e4b_classe_e_memoria.py:26](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/tests/test_e4b_classe_e_memoria.py:26) · [âncora:51](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/tests/test_e4b_classe_e_memoria.py:51) · [contagem:1040](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/tests/test_e4b_classe_e_memoria.py:1040) · [disputa_emissao.py:1854](/home/ronald/codex_ws/cda2467322d281242af0ca00ea5d2771f94a759a/app/chamados/services/disputa_emissao.py:1854)

## 19/11 20:17 · sessao 1ba02fda85b9 · E4C-F5-CONSTROI · construtor · origem=Code

Implementei o helper único e os dois selos em `chamados/tests/`. A conferência estática mediu **9 módulos migrados com a mesma contagem de asserts do MAIN `eb1e49c1e`**. As expressões dos asserts e os nomes dos testes também conferem com o HEAD da cópia. Banco e suíte [nao verificado]. [Tabela e regra de contagem](chamados/tests/test_e4c_f5_duble_conforme.py:14)

### Arquivos tocados e montagem

| Módulo | Antes → depois | MAIN → cópia |
|---|---|---:|
| `e3_v8_apoio.py` | `N` para a pergunta e módulo sem `DecisaoPergunta` no HEAD :169/:199 → fábrica em :170 e classe do helper em :199. | [0 → 0](chamados/tests/e3_v8_apoio.py:170) |
| `test_e1_v10.py` | `_ln` devolvia `SimpleNamespace` no HEAD :120 → fábrica com os campos e a relação vazia para o compositor real. | [31 → 31](chamados/tests/test_e1_v10.py:121) |
| `test_e3_c8b_sem_banco.py` | O ambiente vinha direto do apoio, cujo mapa não instalava `chamados.models` → o contexto instala a classe do helper durante o ensaio. [Apoio original](chamados/tests/e3_c8b_apoio.py:150) | [86 → 86](chamados/tests/test_e3_c8b_sem_banco.py:29) |
| `test_e3_degrau_dono_e_porta.py` | Perguntas `N` e import do modelo real nos três casos do HEAD :263/:291/:343 → fábrica e classe do helper nos contextos da partição. [Consulta](chamados/tests/test_e3_degrau_dono_e_porta.py:263) [Carga](chamados/tests/test_e3_degrau_dono_e_porta.py:289) | [134 → 134](chamados/tests/test_e3_degrau_dono_e_porta.py:334) |
| `test_e3_pos_suite_sem_banco.py` | Ambiente direto do apoio no HEAD :15 e pergunta `N` no HEAD :53 → contexto compartilhado com C8b e fábrica no S1. [Contexto](chamados/tests/test_e3_pos_suite_sem_banco.py:17) | [29 → 29](chamados/tests/test_e3_pos_suite_sem_banco.py:54) |
| `test_e3_tarefa_admin_sem_banco.py` | Modelo `N` de literais e perguntas `N` no HEAD :86/:111/:200 → classe do helper, fábrica em `forma_b` e ambos na testemunha C6. [Modelo](chamados/tests/test_e3_tarefa_admin_sem_banco.py:85) [Forma B](chamados/tests/test_e3_tarefa_admin_sem_banco.py:112) | [7 → 7](chamados/tests/test_e3_tarefa_admin_sem_banco.py:184) |
| `test_e4b_c4v7_memoria.py` | A foto usava `DecisaoPergunta=modelo()` no HEAD :358 → classe compartilhada com o gerenciador vazio instalado no contexto da foto. O comando usa a montagem de `AlcanceTest`. [Foto](chamados/tests/test_e4b_c4v7_memoria.py:381) [Comando](chamados/tests/test_e4b_c4v7_memoria.py:86) | [65 → 65](chamados/tests/test_e4b_c4v7_memoria.py:358) |
| `test_e4b_classe_e_memoria.py` | `Objeto` montado à mão e `DecisaoPergunta=NS()` no HEAD :282/:334 → fábrica preservando `Objeto` e `Linhas`, com a classe do helper no módulo falso. [Pergunta](chamados/tests/test_e4b_classe_e_memoria.py:283) | [122 → 122](chamados/tests/test_e4b_classe_e_memoria.py:333) |
| `test_pergunta_viva.py` | `_p` devolvia `NS` sem `pk` no HEAD :13 → fábrica com `pk=None` e relação vazia, passando pelo compositor real. [Leitura real](chamados/models.py:4330) | [12 → 12](chamados/tests/test_pergunta_viva.py:9) |

Os dois arquivos novos são [duble_pergunta.py](chamados/tests/duble_pergunta.py:1) e [test_e4c_f5_duble_conforme.py](chamados/tests/test_e4c_f5_duble_conforme.py:1). O helper importa só a biblioteca padrão e lê os cinco valores da própria pergunta. Verifiquei o import normal, o retorno, `_replace` e `decisoes.all()` sem Django. A classe também fornece `RESPOSTA`, `VALIDACAO`, `DEVOLUCAO`, `RECUSA`, `REABERTURA`, `SILENCIO` e `ABSORCAO` para os ramos de `_pela_decisao`. O gerenciador `objects` é fornecido pela fixture da foto, que executa `.filter()`. [Helper](chamados/tests/duble_pergunta.py:3) [Ramos da tarefa](chamados/services/tarefa_do_dia.py:150) [Leitura da foto](chamados/services/foto_e4b.py:280)

`test_e3_c8c_sem_banco` recebe o helper por `forma_b` e `tarefa_em_memoria`. O R11 de `test_e3_portas_tarefa` recebe a montagem de `e3_v8_apoio`. C11, C4 tranca, C4v14 e C6 recebem a montagem compartilhada de `AlcanceTest`. [C8c](chamados/tests/test_e3_c8c_sem_banco.py:41) [R11](chamados/tests/test_e3_portas_tarefa.py:631) [C11](chamados/tests/test_e4b_c11_memoria.py:36) [C4](chamados/tests/test_e4b_c4_tranca_memoria.py:26) [C4v14](chamados/tests/test_e4b_c4v14_memoria.py:33) [C6](chamados/tests/test_e4b_c6_memoria.py:15)

### Casos especiais e previsão

- **`test_mutante_sem_patch_no_pai_cai_na_sentinela`**. A falta de `.composta` interrompia a porta antes da chamada que dispara `SENTINELA modulo real`. A montagem compartilhada corrige esse ponto. Prevejo que o mutante volte a atingir a sentinela [nao verificado]. [Caso](chamados/tests/test_e4b_classe_e_memoria.py:255) [Leitura anterior à chamada](chamados/services/disputa_emissao.py:1854) [Sentinela](chamados/tests/test_e4b_classe_e_memoria.py:224)
- **`test_mutantes_mordem_no_assert_pretendido`**. Prevejo vermelho em série. Medi **zero ocorrências** da âncora de `rouba_respondida_do_hx`, que ainda procura `pergunta_respondida(p)`. O código usa `pergunta_respondida(DecisaoPergunta.composta(p))`, e o teste exige uma ocorrência antes de aplicar o mutante. A hipótese para o `PicklingError` é o transporte do subteste falho, que referencia um caso com mocks. O traceback original está [nao verificado]. [Âncora](chamados/tests/test_e4b_classe_e_memoria.py:51) [Checagem](chamados/tests/test_e4b_classe_e_memoria.py:1038) [Mocks](chamados/tests/test_e4b_classe_e_memoria.py:288) [Transporte](core/tests/test_contract_tblib_no_paralelo.py:4)
- **`test_RED_callbacks_conhecidos_fora_do_alcance_recusam_com_nome`**. A fase de planejamento passa pela montagem corrigida antes de executar os callbacks. O `PicklingError` é compatível com o transporte de um subteste que retém a conexão e sua fila com `Mock`. A falha original e a cor após a cura ficam [nao verificado]. [Planejamento](chamados/management/commands/backfill_fio_morto_e4b.py:82) [Conexão](chamados/tests/test_e4b_c4v7_memoria.py:38) [Subteste](chamados/tests/test_e4b_c4v7_memoria.py:240)
- **Tetos A2 fora do escopo**. A sonda registra E2 `36 × 30`, F3 `56 × 48` e O261 `9 > 8`. Os asserts da cópia ainda exigem igualdade em E2 e F3 e teto de `8` no O261. A previsão continua vermelha pelos dados fornecidos, sem remedição [nao verificado]. [E2](chamados/tests/test_e2_tarefa_do_dia.py:938) [F3](escala/tests/test_f3_beco1_folga_na_lista.py:402) [O261](chamados/tests/test_o261_julgamento_desfaz_carimbo.py:205)

### Comandos para o Code

Selos de conformidade, contagem e um comando por mutante. Os três REDs passam quando a comparação rejeita o mutante em memória. [Conformidade](chamados/tests/test_e4c_f5_duble_conforme.py:38) [Mutantes](chamados/tests/test_e4c_f5_duble_conforme.py:51)

```bash
s=chamados.tests.test_e4c_f5_duble_conforme.DublePerguntaConformeTest
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test --parallel 1 "$s.test_duble_conforme_ao_modelo" "$s.test_contagem_dos_asserts_igual_ao_main"
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test --parallel 1 "$s.test_RED_mutante_sem_um_campo"
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test --parallel 1 "$s.test_RED_mutante_com_assinatura_diferente"
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test --parallel 1 "$s.test_RED_fonte_com_um_assert_a_menos"
```

Harnesses da lista, agrupados por classe onde há vários casos. `PerguntaVivaOrdemTest` seleciona os 11 casos. [Pergunta viva](chamados/tests/test_pergunta_viva.py:15) [C8b](chamados/tests/test_e3_c8b_sem_banco.py:210) [E4b](chamados/tests/test_e4b_classe_e_memoria.py:212)

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test --parallel 1 \
  chamados.tests.test_e1_v10.PlanoParticionaTest.test_RED_merito_sob_canonica_sem_episodio \
  chamados.tests.test_e3_c8b_sem_banco.AlimentacaoC6 \
  chamados.tests.test_e3_c8c_sem_banco.C8cSemBanco.test_RED_posicao_ausente_e_alimentada \
  chamados.tests.test_e3_degrau_dono_e_porta.ConsultaECarga \
  chamados.tests.test_e3_portas_tarefa.MatrizV8Test.test_v8_R11_um_instante_ate_o_app \
  chamados.tests.test_e3_pos_suite_sem_banco.PosSuiteSemBanco \
  chamados.tests.test_e3_tarefa_admin_sem_banco.LimitesDaConstrucao.test_C6_com_relacoes_e_tranca_carregadas_ainda_desce_por_pergunta \
  chamados.tests.test_e4b_c11_memoria.DiaC11Test.test_RED_conversa_e_sem_dia_nao_sao_rotuladas_exportadas \
  chamados.tests.test_e4b_c11_memoria.RelogioC11Test.test_RED_apply_ignora_recorte_argv_mas_DRY_conserva \
  chamados.tests.test_e4b_c4_tranca_memoria.TrancaMemoriaTest.test_RED_S143_retira_avulso_com_memo_do_ato_sem_aquecer_cargas \
  chamados.tests.test_e4b_c4v14_memoria.C7TrancaMemoriaTest \
  chamados.tests.test_e4b_c4v7_memoria.C4v7MemoriaTest \
  chamados.tests.test_e4b_c6_memoria.C6MemoriaTest \
  chamados.tests.test_e4b_classe_e_memoria.AlcanceTest \
  chamados.tests.test_pergunta_viva.PerguntaVivaOrdemTest
```

LEI-AKITA: origem=chamados/models.py:24 e :4351, testemunha=chamados/tests/test_e4c_f5_duble_conforme.py:38, RED=chamados/tests/test_e4c_f5_duble_conforme.py:51/:57/:72, padrao=Architecture Conformance Test (docs/GLOSSARIO.md:15).

## 19/11 20:07 · sessao 2cf1fbf9028e · E4C-F5-CONSTROI · verificador · origem=Code

1. **SEGUE.** `LeituraComposta` é um `collections.namedtuple` de `chamados.models`, já importado por um teste Django. `_fields` dá a ordem `validada_em`, `respondida_em`, `resposta_colab`, `materializada_em`, `via_resolucao`. [models.py:24](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/models.py:24) · [test_e4c_f5_opus_models.py:15](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/tests/test_e4c_f5_opus_models.py:15)  
   `composta` é `@classmethod` e `inspect.signature(DecisaoPergunta.composta)` dá `(p)`, sem `cls`. O selo compara com `Duble.composta` também ligada à classe. [models.py:4338](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/models.py:4338)  
   Os harnesses citados já fazem import normal de `chamados.tests.*`, caminho disponível para o helper puro. A leitura por caminho extrai corpos existentes. `e4_apoio.py` faz essa extração, e a troca de `chamados.models` ocorre nos harnesses. A lista integral da prova é [nao verificado]. [test_e3_tarefa_admin_sem_banco.py:23](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:23) · [test_e4b_classe_e_memoria.py:17](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/tests/test_e4b_classe_e_memoria.py:17) · [e3_v8_apoio.py:12](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/tests/e3_v8_apoio.py:12) · [e4_apoio.py:8](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/tests/e4_apoio.py:8) · [e3_v8_apoio.py:199](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/tests/e3_v8_apoio.py:199)

2. **SEGUE.** O 2005 autoriza a classe leve dentro do helper único. O compositor real é referência do selo. As adaptações ficam na exceção nominal do 1945 à vedação do 1710 e à lista fechada do 1830, restritas ao duble e à causa autorizada. `resposta_f5_devolvido_1945_2005.txt:3` · `resposta_f5_duble_1911_1945.txt:2` · `resposta_f5_duble_1911_1945.txt:7` · `e4c_f5_constroi_a_1911_1710.txt:13` · `respostas_f5_constroi_2_1911_1830.txt:10`

3. **SEGUE.** Comparar `_fields` em ordem e a assinatura ligada detecta a remoção de um campo declarado e a mudança da assinatura exposta. Tipos, valores, origem dos valores e comportamento de `_replace` ficam fora desse selo. A execução dos dois REDs é [nao verificado]. `resposta_f5_devolvido_1945_2005.txt:3` · [models.py:24](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/models.py:24) · [models.py:4345](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/models.py:4345)  
   Na cópia, os três consumidores de `composta` leem atributos, sem indexação posicional ou teste de identidade de `LeituraComposta`. O `isinstance` do juiz verifica `PerguntaDisputa`. O `_replace` de `fio.py` opera sobre `LeituraDoAto`, retornada por `.leitura`. O alvo do `_replace` na A1 citada no anexo é [nao verificado]. [models.py:2012](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/models.py:2012) · [models.py:3244](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/models.py:3244) · [models.py:3801](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/models.py:3801) · [juizes.py:1559](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/juizes.py:1559) · [fio.py:189](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/services/fio.py:189) · [models.py:4336](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/models.py:4336)

4. **SEGUE.** O escopo declarado é de teste. A comparação inspeciona a classe e a assinatura, sem acionar o compositor nem alcançar gravado de exportada, dinheiro ou outra ação sujeita a `!`. `resposta_f5_devolvido_1945_2005.txt:3` · [CLAUDE.md:265](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/CLAUDE.md:265) · [CLAUDE.md:406](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/CLAUDE.md:406)

## 19/11 19:50 · sessao e8d143d0a5af · E4C-F5-CONSTROI · verificador · origem=Code

# AVAL-DEVOLVIDO — RESPOSTA-F5-DUBLE-1911-1945

**A exceção resolve falhas de dublê, mas o PRONTO não fecha.** A prova registra falhas de selo, tetos e dois casos ORM cujo resultado diverge do esperado. O caso de ressuscitação F2 atinge uma regra vigente da porta. [a1_porteste.tsv:1, 3, 15, 19, 26, 29, 97–102, 114, anexo] [chamados/models.py:1534](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/models.py:1534)

A cópia tem o compositor real e os corpos dos juízes anteriores à A1. As referências à A1 vêm do diff anexo. Suite após a troca dos dublês e efeito em produção **[nao verificado]**. [chamados/juizes.py:1389](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/juizes.py:1389) · [chamados/juizes.py:1559](/home/ronald/codex_ws/18bf86f01e4fb82b07b3851c4af925c177fce364/app/chamados/juizes.py:1559) · `a1_porteste.tsv:1–114` (anexo).

| Item | Veredito | Motivo |
|---|---|---|
| **EXCEÇÃO** | **PROCEDE — TÉCNICO** | O 1945 excepciona o veto do 1710 e responde à exigência de consulta do 1735/1830 para a causa nominal. A lista literal deixa de fora `e3_v8_apoio.py`, que instala o módulo falso sem `DecisaoPergunta` para o R11. A exceção também deixa de fora os dois `NameError` do global `VIAS_FORA_DO_FATO`. `resposta_f5_duble_1911_1945.txt:2`; `e4c_f5_constroi_a_1911_1710.txt:13–14`; `respostas_f5_constroi_1911_1735.txt:9`; `respostas_f5_constroi_2_1911_1830.txt:10`; `app/chamados/tests/e3_v8_apoio.py:196–209`; `a1_porteste.tsv:15,20,70` (anexos). |
| **FONTE** | **SEGUE** | `LeituraComposta` é uma `namedtuple` de exatamente cinco campos, em ordem: `validada_em`, `respondida_em`, `resposta_colab`, `materializada_em`, `via_resolucao`. A definição é `@classmethod def composta(cls, p)`, e a assinatura ligada é `(p)`. Os dois instantes vêm de `leitura`; os três demais valores vêm da pergunta. `app/chamados/models.py:19–25,4338–4347`. |
| **MUDA 1** | **PROCEDE — TÉCNICO** | Um helper único pode abastecer os dicionários existentes sem duplicar a fábrica de dublês. Falta fechar como ele entrega a **classe real** nos harnesses que rodam sem Django/settings, inclusive quando `chamados.models` está substituído. Recompilar uma função por AST é compatível com a fábrica única. Reexecutar a declaração da `namedtuple` cria outra classe. `app/chamados/tests/e4_apoio.py:8–16`; `app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:1–10,83–98`; `app/chamados/tests/test_e4b_classe_e_memoria.py:1–4,152–161,334`. |
| **MUDA 2** | **SEGUE** | A `composta` falsa cobre a ausência do método onde ela é injetada. Nos caminhos que usam a real, a pergunta precisa de `pk`, de `decisoes.all()` se o `pk` for definido e dos campos lidos pelo legado, inclusive `validada_por_id` se houver validação projetada sem linha correspondente. Isso cobre as três faltas de interface sem alteração no compositor. A contagem de verdes segue **[nao verificado]**. `app/chamados/models.py:4261–4279,4318–4323,4345–4347`; `a1_porteste.tsv:2,6,103–113` (anexo). |
| **MUDA 3** | **SEGUE** | `test_pergunta_viva` tem uma fábrica local `_p`, sem `pk` nem `decisoes`, e chama a entrada real. Os 11 erros anexados são de `pk`. A troca da fábrica cabe na exceção e preserva as asserções de precedência. `app/chamados/tests/test_pergunta_viva.py:4–23,74–77`; `a1_porteste.tsv:103–113` (anexo). |
| **SELO (a)** | **SEGUE** | O comparável é a assinatura do método ligado **`duble.composta`** e a sequência de campos do resultado. O caso deve chamar o dublê com cinco valores distinguíveis e verificar tipo e valores. Comparar `_fields` da mesma classe consigo mesma deixaria a construção do dublê sem teste. `app/chamados/models.py:24–25,4338–4347`; `app/chamados/tests/test_e4c_f5_opus_models.py:58–68`. |
| **SELO (b)** | **PROCEDE — TÉCNICO** | Para arquivo ausente no MAIN, a contagem por módulo é **sem base**, e o selo não fecha no literal. `test_e4c_f5_selo_chamada.py` entra como novo na A1. Para módulos que já existiam, falta a contagem no MAIN **[nao verificado]**. Contar apenas o nó Python `assert` também deixaria de fora as chamadas `self.assert*` usadas pelos testes. `resposta_f5_duble_1911_1945.txt:5` (anexo); `a1_porteste.tsv:97` e `puro_porteste.tsv:10` (anexos); `app/chamados/tests/test_e4c_f5_volta2_juizes.py:133–140`. |
| **RED** | **SEGUE** | A soma do anexo fecha em **114 vermelhos em 25 módulos**. O mutante de um campo omitido é um RED separado. A prova fornecida antecede o helper e a execução desse mutante é **[nao verificado]**. `a1_por_modulo.txt:1–25`; `resposta_f5_duble_1911_1945.txt:5–6` (anexos); `app/chamados/models.py:24–25`. |
| **PROIBIDO** | **SEGUE** | A vedação a fallback no compositor e a teste de tipo **dentro dos dois juízes** é compatível com o 1830. A A1 retira os dois ramos de tipo. O `isinstance` do RED 8 está no teste, e trocá-lo continua fora da autorização de dublês do 1945. `CLAUDE.md:14–22`; `respostas_f5_constroi_2_1911_1830.txt:3,11`; `resposta_f5_duble_1911_1945.txt:7` (anexos); `app/chamados/tests/test_e4c_f5_volta2_juizes.py:140`; diff A1, hunks de `chamados/juizes.py:1389,1559`. |
| **PRONTO** | **PROCEDE — TÉCNICO e LEI** | **Técnico:** trocar dublês deixa falhas de harness fora da causa autorizada e a prova não fecha a lista nominal dos 11 da F5/tetos da A2. Os tetos E2/F3 ficam intactos pelo 1830; O261 tem a exceção nominal 8→9 do 1735. **Lei:** o F2 mostra a validada deixando de voltar pela porta de ressuscitação. O PRONTO intermediário também não substitui o PRONTO cumulativo nem a certificação após C. `a1_porteste.tsv:3,15,19,26,70,97–102,114`; `e4c_f5_constroi_a_1911_1710.txt:14`; `respostas_f5_constroi_1911_1735.txt:8`; `respostas_f5_constroi_2_1911_1830.txt:3,9,12` (anexos); `app/chamados/models.py:1534–1545,1567–1583`. |

### Onde cada harness monta os dublês

| Módulos da prova | Modelo e pergunta |
|---|---|
| `test_e4b_classe_e_memoria`, `test_e4b_c4_tranca_memoria`, `test_e4b_c4v14_memoria`, `test_e4b_c4v7_memoria`, `test_e4b_c6_memoria`, `test_e4b_c11_memoria` | **Compartilhado.** `AlcanceTest.setUp` monta `self.p` e instala `DecisaoPergunta=NS()` no módulo falso. Os demais reaproveitam `AlcanceTest`, diretamente ou pela tranca. C4v14 também injeta o **modelo real** em `sem_veredito_entre`; C4v7 tem outro modelo falso no cenário de fotografia. `app/chamados/tests/test_e4b_classe_e_memoria.py:275–287,300–335,759–764`; `app/chamados/tests/test_e4b_c4_tranca_memoria.py:25–28`; `app/chamados/tests/test_e4b_c4v14_memoria.py:32–53`; `app/chamados/tests/test_e4b_c4v7_memoria.py:85–89,342–358`; `app/chamados/tests/test_e4b_c6_memoria.py:14–21`; `app/chamados/tests/test_e4b_c11_memoria.py:35–39,74–88`. |
| `test_e3_tarefa_admin_sem_banco`, `test_e3_c8b_sem_banco`, `test_e3_c8c_sem_banco`, `test_e3_pos_suite_sem_banco` | **Compartilhado em parte.** `tarefa_em_memoria` instala `DecisaoPergunta=N(**literais(...))`, só com constantes. C8b reaproveita essa base; C8c e PosSuite usam a de C8b. As perguntas vêm de `forma_b`, de `testemunha` no apoio C8b ou de fixtures locais. `ambiente_c6` recompila C6 e slot, sem instalar outro `chamados.models`. O teste de limite do módulo admin monta seu próprio ambiente e sua própria pergunta. `app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:72–98,101–116,153–203`; `app/chamados/tests/test_e3_c8b_sem_banco.py:14–23,199–221`; `app/chamados/tests/e3_c8b_apoio.py:83–90,109–120,150–198`; `app/chamados/tests/test_e3_c8c_sem_banco.py:41–61,123–135`; `app/chamados/tests/test_e3_pos_suite_sem_banco.py:47–65,71–83`. |
| `test_e3_degrau_dono_e_porta`, `test_e3_portas_tarefa` | **Dois arranjos.** Os três casos de `ConsultaECarga` compilam a partição com perguntas locais e usam o `DecisaoPergunta` real nesses caminhos. O R11 usa `app_com_relogios`: pergunta local e módulo falso que fornece `PerguntaDisputa` e `pergunta_viva`, sem `DecisaoPergunta`. `app/chamados/tests/test_e3_degrau_dono_e_porta.py:261–274,284–319,328–344`; `app/chamados/tests/test_e3_portas_tarefa.py:631–637`; `app/chamados/tests/e3_v8_apoio.py:158–171,182–209`. |
| `test_e1_v10`, `test_pergunta_viva` | **Locais.** `_ln` e `_p` criam as perguntas em memória. Os caminhos em questão chegam ao modelo real. `_ln` fornece `pk`, sem `decisoes`; `_p` carece de ambos. `app/chamados/tests/test_e1_v10.py:118–127,143–151`; `app/chamados/tests/test_pergunta_viva.py:4–13`; `a1_porteste.tsv:2,103–113` (anexo). |
| `test_cortes_f2_f9_m4`, `test_e2_tarefa_do_dia`, `test_f2_ressuscitar_tarefa_igual`, `test_o261_julgamento_desfaz_carimbo`, `test_f3_beco1_folga_na_lista`, `test_e4c_f5_volta2_juizes` | **ORM real nos casos vermelhos.** Os REDs 8 usam `BaseCortes` e medem o ORM. O `SimpleNamespace` de `medir_calc` é a **carga**, e a asserção de tipo inspeciona o argumento entregue ao juiz. `app/chamados/tests/cortes_f2_f9_apoio.py:58–74`; `app/chamados/tests/test_cortes_f2_f9_m4.py:36–56`; `app/chamados/tests/test_e2_tarefa_do_dia.py:913–938`; `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:276–295`; `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:187–205`; `app/escala/tests/test_f3_beco1_folga_na_lista.py:378–402`; `app/chamados/tests/test_e4c_f5_volta2_juizes.py:118–180`. |
| `test_e3_mutantes_alvo`, `test_e3_volta7_sem_banco`, `test_e4b_c12_selo`, `test_e4c_f5_volta1_leem_o_ato`, `test_e4c_f5_selo_chamada` | **Selos de fonte nos casos vermelhos.** Volta7 reaproveita o selo E3; C12 censa escritores; os dois selos F5 leem AST. Nenhum desses casos monta dublê de `DecisaoPergunta` ou de pergunta para produzir o vermelho. `app/chamados/tests/test_e3_mutantes_alvo.py:15–25`; `app/chamados/tests/test_e3_volta7_sem_banco.py:96–98`; `app/chamados/tests/test_e4b_c12_selo.py:22–32`; `app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:229–251`; diff A1, novo `test_e4c_f5_selo_chamada.py`. |

### Vermelhos fora da troca de dublês

- **M4, mudança de valor.** `test_RED_queda_final_le_resposta_do_ato_com_projecao_vazia` espera `respondida`, mas recebe `pendente`. O texto está só no ato. `estado` usa o texto da projeção pelo compositor. A expectativa antiga diverge do contrato 1830 para esse leitor, e a autorização de mudar a tabela F2 não nomeia o teste M4. `a1_porteste.tsv:1`; `puro_porteste.tsv:1`; `respostas_f5_constroi_2_1911_1830.txt:6` (anexos); `app/chamados/tests/test_cortes_f2_f9_m4.py:43–56`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:500–512`; `app/chamados/models.py:3241–3253,4345–4347`.
- **F2, regressão de lei na porta.** `test_RED_7_a_foto_guarda_o_autor_da_validacao_VIGENTE_no_empate_de_instante` falha antes de conferir o autor: voltam `0`, esperado `1`. A porta zera os campos projetados da cópia para perguntar ao C6 como a pergunta ficaria **depois** da ressuscitação. A A1 recompõe essa cópia a partir dos atos ainda vigentes, recupera a validação e encerra o C6 no primeiro degrau. `a1_porteste.tsv:101` (anexo); `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:276–295`; `app/chamados/models.py:1534–1545,1555,1567–1583,4318–4347`; diff A1, hunk de `_pergunta_viva_calc`.
- **Tetos medidos.** `test_lote_tem_queries_constantes` deu **36 × 30**; `test_MORDE_F5_queries_nao_crescem_com_os_dias`, **56 × 48**; `test_custo_por_candidata_e_CONSTANTE`, **9 > 8**. F3 consta da prova por módulo e do TSV, mas está ausente de `mods_quebrados.txt`. `a1_porteste.tsv:3,102,114`; `a1_por_modulo.txt:13`; `mods_quebrados.txt:1` (anexos); `app/chamados/tests/test_e2_tarefa_do_dia.py:931–938`; `app/escala/tests/test_f3_beco1_folga_na_lista.py:395–402`; `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:203–205`.
- **Selos e globais.** `test_todo_mutante_da_E3_tem_alvo_unico` e `test_alvos` perdem a mesma âncora `inclui_respondidas`. `test_RED_um_subTest_por_sitio` acusa `_gravar_estado`: a identidade C12 inclui a linha `462` e a chamada está na `469`. O selo F5 de renomeação procura `.leitura` e deixa `anterior` sem valor quando o fio passa a `.composta`. No selo de chamada, retirar a entrada composta da projeção também invalida a origem dos campos do `_replace` no segundo juiz. Os outros dois casos são `test_foto_temporal_nao_entrega_carimbos_futuros_ao_C6` e `test_RED_harness_carrega_projecao_real_sem_alterar_pergunta`, ambos sem o global `VIAS_FORA_DO_FATO`. `a1_porteste.tsv:15,19,26,29,70,97–98`; `puro_porteste.tsv:2` (anexos); `app/chamados/tests/e3_mutantes.py:30`; `app/chamados/tests/e4b_c12_ast.py:16–20`; `app/chamados/models.py:469`; `app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:229–251`; diff A1, `_fonte_composta` e hunk de `_pergunta_viva_calc`.
- **Diagnóstico incompleto.** Os dois REDs 8 aparecem como `?`. Há uma incompatibilidade estática em `test_red_8_custo_pergunta_viva_calc_prefetch_uma_e_cinco`: a asserção exige `SimpleNamespace` e a A1 passa `LeituraComposta._replace(...)`. A exceção final dos dois REDs 8, a causa dos dois `PicklingError` e o resultado da sentinela após corrigir os dublês estão **[nao verificado]**. Já o `assertEqual` de `test_slot_hora_noturno_alimenta_grade_vinculo_e_plantio_reais` compara uma `AttributeError` armazenada no slot com a tupla esperada. É a falta de `decisoes` chegando ao assert. `a1_porteste.tsv:9,41,88–89,99–100` (anexo); `app/chamados/tests/test_e4c_f5_volta2_juizes.py:127–149,151–180`; `app/chamados/services/carga_pergunta_viva.py:117–125`; `app/chamados/tests/test_e3_c8b_sem_banco.py:406–410`.
- **Controle com corpo puro.** Os REDs **1, 2, 3, 5 e 6** da volta 2 mudam de valor na árvore só com os juízes puros. São casos com ato e projeção divergentes, e o diff A1 adapta as chamadas para entregar `D.composta(p)`. Eles não aparecem entre os vermelhos A1. `puro_porteste.tsv:3–7`; `a1_porteste.tsv:99–100` (anexos); `app/chamados/tests/test_e4c_f5_volta2_juizes.py:37–104`; diff A1, hunks desse arquivo.

A troca nominal de dublês incide sobre fixtures. Os testes de exportada, dinheiro e `!` continuam exigindo seus resultados originais. A A1 já troca a leitura no guarda que antecede a verificação de exportada e muda o resultado do cenário ORM F2. Efeito em exportada, dinheiro ou `!` em produção **[nao verificado]**. `app/chamados/tests/test_e4b_classe_e_memoria.py:698–702,739–748`; `app/chamados/services/disputa_emissao.py:1848–1878`; `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:292–295`; diff A1, hunk de `reabrir_credor_de_pergunta_viva`.

Para o registro do PRONTO, o comparador aplicado ao vocabulário da cópia rejeita `Test Double com contract test` e aceita `Architecture Conformance Test`, termo do selo de conformidade. A execução do hook sobre um commit A1 está **[nao verificado]**. `bin/tests/test_lei_akita.sh:96–149,337–356`; `app/docs/GLOSSARIO.md:15`; `resposta_f5_duble_1911_1945.txt:8` (anexo).

## 19/11 19:34 · sessao 9d7185de4140 · CADEIA-20-PASSO-1 · sonda · origem=Code

```text
sonda_leitura: sonda.py [09aa035aee18a63b] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

HOJE 2026-11-19 ONTEM 2026-11-18 DIAS_COM_BATIDA 29858 CELULAS 51970
SEM_CELULA TOTAL 36 [(('2026-09', 2), 34), (('2026-11', 2), 2)]
ATA_NULA TOTAL 0 []
ATA_SEM_CHAVE_ORFAS TOTAL 0 []
FIM_SONDA
```

ERRO ou traceback no bloco fornecido? Não. `FIM_SONDA` apareceu? Sim, na última linha (`DADO RASPADO:1–7`).  
Reexecução [nao verificado]. A porta cria arquivos e grava log (`bin/codex_pergunta.sh:307–309`).

## 19/11 19:03 · sessao 93bc4756bb32 · E4C-F5-CONSTROI · construtor · origem=Code

**A1 implementada.** O censo AST mediu **41 chamadas na base e 41 na A1**. Das 36 identidades pendentes na base, 12 passaram a receber leitura composta. Restam **24**, descontada a exceção nominal do placar. A leitura estática dos dois juízes encontrou zero violações de pureza. Compilação em memória e `git diff --check` concluídos. Resultado da suíte, banco e mutantes ORM [nao verificado]. `chamados/tests/test_e4c_f5_selo_chamada.py:20`, `chamados/tests/test_e4c_f5_selo_chamada.py:191`, `chamados/services/raiox.py:147`.

## Arquivos tocados

- Produção: `chamados/juizes.py:1383`, `chamados/reconciliador.py:367`, `chamados/services/disputa_emissao.py:1854`, `chamados/services/fio.py:189`, `chamados/services/pergunta_no_app.py:87`, `ponto/services/cartorio.py:1690`.
- Testes novos: [test_e4c_f5_selo_chamada.py](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_chamada.py:20) e [test_e4c_f5_voltaA_sitios.py](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_voltaA_sitios.py:32). [nao verificado]
- Adaptações: `chamados/tests/test_e4c_f5_volta2_juizes.py:37`, `chamados/tests/e4c_f5_volta2_mutantes.py:33`, `chamados/tests/test_e4c_f2_leitura_do_ato.py:523`, `chamados/tests/test_f1nova_b2_leitores.py:87`.

Os juízes recebem os atributos do objeto e mantêm nome e assinatura. O corte temporal usa a constante `VIAS_FORA_DO_FATO` já definida no módulo de origem, sem consultar a classe. O comentário em cada juiz registra a leitura transitória da coluna pelos chamadores B/C que ainda entregam ORM. `chamados/juizes.py:13`, `chamados/juizes.py:1383`, `chamados/juizes.py:1549`, `chamados/models.py:2588`.

## Sítios antes e depois

`P` = `projecao_temporal_da_pergunta`. `R` = `pergunta_respondida`. `composta` = `DecisaoPergunta.composta`.

| Sítio | Antes na base `18bf86f01` | Depois na A1 |
|---|---|---|
| `_slot_esperado` | `P(p, agora)` · `chamados/juizes.py:351` | `P(composta(p), agora)` · `chamados/juizes.py:351` |
| `_pergunta_viva_calc`, corte temporal | `P(p, agora)` · `chamados/juizes.py:1411` | Uma chamada `_lido = composta(p)`, seguida de `P(_lido, agora)` · `chamados/juizes.py:1408–1409` |
| `_pergunta_viva_calc`, degrau da resposta | `R(SimpleNamespace(...))` · `chamados/juizes.py:1430` | `R(_lido._replace(respondida_em=respondida, resposta_colab=resposta))`, com os dois valores recortados da mesma leitura · `chamados/juizes.py:1428` |
| `lavrar_sem_turno_aberto` | `R(p)` · `chamados/reconciliador.py:366` | `R(composta(p))` · `chamados/reconciliador.py:367` |
| `veredito_orfa` | `R(p)` · `chamados/reconciliador.py:1133` | `R(composta(p))` · `chamados/reconciliador.py:1135` |
| `reabrir_credor_de_pergunta_viva` | `R(p)` · `chamados/services/disputa_emissao.py:1853` | `R(composta(p))` · `chamados/services/disputa_emissao.py:1854` |
| `_resolver_perguntas_por_chamado_resolvido` | `R(p)` · `chamados/services/disputa_emissao.py:2023` | `R(composta(p))` · `chamados/services/disputa_emissao.py:2025` |
| `processar_post_fio` | `R(_p)` · `chamados/services/fio.py:712` | `R(composta(_p))` · `chamados/services/fio.py:711` |
| `canonica_do_grupo` | `R(p)` · `chamados/services/fio.py:1287` | `R(composta(p))` · `chamados/services/fio.py:1287` |
| `merito_perdedor` | `R(p)` · `chamados/services/fio.py:1300` | `R(composta(p))` · `chamados/services/fio.py:1300` |
| `particao_do_app` | `R(p)` via apelido · `chamados/services/pergunta_no_app.py:86` | `R(composta(p))` via apelido · `chamados/services/pergunta_no_app.py:87` |
| `_julgar_colab_corpo._tem_resposta_do_colab` | `R(q)` · `ponto/services/cartorio.py:1689` | `R(composta(q))` · `ponto/services/cartorio.py:1690` |
| `espera_o_texto_do_colaborador` | `leitura(p)` seguida de `_replace(resposta_colab=p.resposta_colab)` · `chamados/services/fio.py:189–192` | `composta(p)` alimenta o mesmo teste de instante e o mesmo `respondida_sem_texto` · `chamados/services/fio.py:189–190` |

A partição e o fio já usavam as fontes corretas na base. Os dois casos de divergência são **GUARDAS**. A fixture grava pela porta de resposta automática e usa `update()` somente para simular o **PASSIVO**. O RED de delegação do fio exige uma chamada ao compositor, portanto a composição manual da base falha nessa asserção. `chamados/services/pergunta_no_app.py:86` na base, `chamados/juizes.py:1559` na base, `chamados/services/fio.py:189` na base, `chamados/tests/test_e4c_f5_voltaA_sitios.py:33`, `chamados/tests/test_e4c_f5_voltaA_sitios.py:65`.

A tabela F2 já veio da base com `False` para sem veredito, CSF, irmãs, estado e property, e `True` para `resposta_display` e `q_vivas`, com motivo por leitor. A prova reforçada mostra coluna com `(None, None)` e leitura temporal com validação e resposta nos instantes exatos `(T2, T1)`. `chamados/tests/test_e4c_f2_leitura_do_ato.py:500`, `chamados/tests/test_e4c_f2_leitura_do_ato.py:523`.

## Selo de chamada — 24 identidades

O ordinal conta as chamadas aos dois juízes em ordem de fonte dentro da função. A catraca compara as identidades encontradas com o conjunto declarado. A exceção é `creditos_perguntas`, `R#1`, pela tupla de `values_list`. `chamados/tests/test_e4c_f5_selo_chamada.py:20`, `chamados/tests/test_e4c_f5_selo_chamada.py:169`, `chamados/services/raiox.py:135`, `chamados/services/raiox.py:147`.

| Arquivo e linha | Função | Juiz e ordinal |
|---|---|---|
| `api/views.py:2596` | `api_disputa_perguntas` | R#1 |
| `api/views.py:2640` | `api_disputa_responder` | R#1 |
| `chamados/management/commands/auditar_invariantes_chamados.py:178` | `_contraprova_i21a` | R#1 |
| `chamados/management/commands/auditar_invariantes_chamados.py:597` | `Command.handle` | R#1 |
| `chamados/management/commands/auditar_invariantes_chamados.py:1671` | `Command.handle` | R#2 |
| `chamados/management/commands/dedup_perguntas_fato.py:76` | `Command.handle` | R#1 |
| `chamados/services/acoes_disputa.py:134` | `acao_regularizar_disputa` | R#1 |
| `chamados/services/becos.py:31` | `classificar` | R#1 |
| `chamados/services/declaracao_texto.py:418` | `declarar_hora_colab` | R#1 |
| `chamados/services/declaracao_texto.py:688` | `aplicar_declaracao` | R#1 |
| `chamados/services/materializacao.py:192` | `par_do_intervalo` | R#1 |
| `chamados/services/materializacao.py:259` | `paridade_absolvida_bug_par_103` | R#1 |
| `chamados/services/materializacao.py:290` | `_situacao_da_irma` | R#1 |
| `chamados/services/materializacao.py:885` | `estado_aplicacao_pergunta` | R#1 |
| `chamados/services/materializacao.py:1300` | `_recusa_sem_anuencia` | R#1 |
| `chamados/services/validacao.py:682` | `validar_pergunta` | R#1 |
| `chamados/services/validacao.py:902` | `recusar_pergunta` | R#1 |
| `chamados/services/vigia_fio.py:636` | `censo_alcance` | R#1 |
| `chamados/views_cobrar.py:170` | `validar_inline` | R#1 |
| `chamados/views_cobrar.py:178` | `validar_inline` | R#2 |
| `chamados/views.py:1275` | `responder_pergunta_disputa` | R#1 |
| `colaboradores/services/ficha.py:172` | `ficha_do_colab` | R#1 |
| `escala/services/cadastro_realidade.py:502` | `beco_em_lote` | P#1 |
| `folha/previa.py:142` | `_material` | R#1 |

## Comandos para o Code

Os labels abaixo cobrem os selos, os dois casos de divergência, o RED de delegação e as adaptações A1. A execução segue [nao verificado]. `chamados/tests/test_e4c_f5_selo_chamada.py:203`, `chamados/tests/test_e4c_f5_voltaA_sitios.py:45`, `chamados/tests/test_e4c_f5_volta2_juizes.py:37`, `chamados/tests/test_e4c_f2_leitura_do_ato.py:517`, `chamados/tests/test_f1nova_b2_leitores.py:87`.

```bash
export PYTHONDONTWRITEBYTECODE=1
teste() { [nao verificado]
  python3 manage.py test "$@"
}

selo=chamados.tests.test_e4c_f5_selo_chamada
sitios=chamados.tests.test_e4c_f5_voltaA_sitios.SitiosVoltaATest
v2=chamados.tests.test_e4c_f5_volta2_juizes.JuizesVolta2Test
f2=chamados.tests.test_e4c_f2_leitura_do_ato.FonteMigradaPorLeitorTest

teste "$selo" "$sitios" \ [nao verificado]
  "$v2.test_red_1_resposta_no_ato_instante_e_texto_corrente" \
  "$v2.test_red_2_reabertura_no_ato_vence_carimbo_projetado" \
  "$v2.test_red_3_validacao_no_ato_sem_carimbo_projetado" \
  "$v2.test_red_5_agora_corta_instante_do_ato_e_texto_corrente" \
  "$v2.test_red_6_respondida_instante_do_ato_texto_da_projecao" \
  "$f2.test_resposta_so_no_ato_cada_leitor_le_o_ato" \
  "$f2.test_validacao_so_no_ato_q_vivas_e_estado_leem_o_ato" \
  "$f2.test_instante_e_o_do_ato_nao_o_da_coluna" \
  chamados.tests.test_f1nova_b2_leitores.ClasseC_PropertyEEstadoTest.test_property_DELEGA_ao_juiz_nao_o_copia
```

| Mutante | Caso que deve morder |
|---|---|
| `a1_app_coluna` | `test_GUARDA_particao_do_app_instante_do_ato_texto_da_projecao` perde a ida ao histórico com instante só no ato. `chamados/tests/test_e4c_f5_voltaA_sitios.py:45` |
| `a1_app_texto_do_ato` | A mesma guarda perde a pendência com texto só na trilha. `chamados/tests/test_e4c_f5_voltaA_sitios.py:45` |
| `a1_fio_coluna` | `test_GUARDA_fio_instante_do_ato_texto_da_projecao` volta a esperar texto apesar do instante no ato e do texto corrente. `chamados/tests/test_e4c_f5_voltaA_sitios.py:55` |
| `a1_fio_texto_do_ato` | A mesma guarda deixa de esperar quando a projeção está vazia. `chamados/tests/test_e4c_f5_voltaA_sitios.py:55` |
| `a1_fio_composicao_local` | `test_MORDE_fio_composicao_local` falha na asserção de chamada única ao compositor. `chamados/tests/test_e4c_f5_voltaA_sitios.py:65` |
| Chamada crua adicional, apelido e atributo | Os casos sintéticos exigem a identidade nova e o ordinal correspondente. `chamados/tests/test_e4c_f5_selo_chamada.py:209`, `chamados/tests/test_e4c_f5_selo_chamada.py:220` |
| `_replace` com texto do ORM ou instante de outra leitura | O caso sintético exige a detecção das duas chamadas. `chamados/tests/test_e4c_f5_selo_chamada.py:232` |
| Retirada da composição em cada uma das 12 chamadas A1 | Um subteste por identidade exige que a chamada volte a cair na catraca. `chamados/tests/test_e4c_f5_selo_chamada.py:264` |
| `isinstance`, import de modelo, `.objects` e consulta | O selo de pureza assinala cada instrução sintética. `chamados/tests/test_e4c_f5_selo_chamada.py:289` |
| Fio com parâmetro renomeado e ORM entregue pelo nome `leitura` | O selo legado acusa que `respondida_sem_texto` recebeu a pergunta inteira. `chamados/tests/e4c_f5_volta2_mutantes.py:33`, `chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:220` |

Os cinco seletores ORM devem produzir falha na asserção indicada. Os casos AST e o mutante legado fazem a contraprova em memória e devem passar ao detectar a violação. Execução [nao verificado]. `chamados/tests/cortes_f2_f9_apoio.py:84`, `chamados/tests/test_e4c_f5_selo_chamada.py:209`, `chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:220`.

```bash
app="$sitios.test_GUARDA_particao_do_app_instante_do_ato_texto_da_projecao"
fio="$sitios.test_GUARDA_fio_instante_do_ato_texto_da_projecao"
CORTES_F2_MUTANTE=a1_app_coluna teste "$app" [nao verificado]
CORTES_F2_MUTANTE=a1_app_texto_do_ato teste "$app" [nao verificado]
CORTES_F2_MUTANTE=a1_fio_coluna teste "$fio" [nao verificado]
CORTES_F2_MUTANTE=a1_fio_texto_do_ato teste "$fio" [nao verificado]
CORTES_F2_MUTANTE=a1_fio_composicao_local teste "$sitios.test_MORDE_fio_composicao_local" [nao verificado]

teste "$selo.SeloChamadaTest.test_MORDE_chamada_crua_nova" [nao verificado]
teste "$selo.SeloChamadaTest.test_MORDE_apelido_e_chamada_por_atributo" [nao verificado]
teste "$selo.SeloChamadaTest.test_MORDE_replace_fora_da_projecao_do_mesmo_objeto" [nao verificado]
teste "$selo.SeloChamadaTest.test_MORDE_retirar_compositor_de_cada_chamada_da_A1" [nao verificado]
teste "$selo.SeloPurezaTest.test_MORDE_tipo_modelo_e_consulta" [nao verificado]
teste chamados.tests.test_e4c_f5_volta1_leem_o_ato.SeloTest.test_MORDE_parametro_renomeado_e_pergunta_inteira [nao verificado]
```

## Pergunta da sessão — testes congelados [nao verificado]

Previsões por leitura do código. Resultado de execução [nao verificado].

| Arquivo e linha | Quebra prevista |
|---|---|
| `chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:236`, `:243`, `:247` | O teste de renomeação ainda procura `.leitura(...)` no fio. Com `.composta(...)`, `anterior` fica sem valor. O caso de `:223` usa o mutante cuja âncora foi atualizada. `chamados/tests/e4c_f5_volta2_mutantes.py:33` |
| `chamados/tests/test_e4c_f5_volta2_juizes.py:122`, `:145`, `:170` | Os dois REDs 8 ainda nascem com texto sem instante e podem parar na porta antes da captura. Superado isso, `:141` ainda exige `SimpleNamespace` no C6. `chamados/models.py:3689`, `chamados/models.py:3721`, `chamados/juizes.py:1428` |
| `chamados/tests/test_pergunta_viva.py:8`, `:17` | Os dublês reais do C6 são `SimpleNamespace` sem `pk`. A leitura chamada pelo compositor acessa `p.pk`. `chamados/models.py:4318`, `chamados/juizes.py:1408` |
| `chamados/tests/test_e3_c8b_sem_banco.py:355`, `:383`, `:410`, `:430`, `chamados/tests/test_e3_c8c_sem_banco.py:136`, `chamados/tests/test_e3_pos_suite_sem_banco.py:66`, `:85`, `:99`, `:109`, `:113` | Os casos do C6 e do slot usam dublês sem a relação ou sem `composta` no módulo simulado. A chamada temporal direta de C8c também alcança o ramo que precisa de `VIAS_FORA_DO_FATO`, que o harness injeta só na classe simulada. `chamados/tests/e3_c8b_apoio.py:83`, `chamados/tests/e3_c8b_apoio.py:194`, `chamados/tests/test_e3_tarefa_admin_sem_banco.py:86` |
| `chamados/tests/test_e3_tarefa_admin_sem_banco.py:200`, `:206`, `chamados/tests/test_e3_degrau_dono_e_porta.py:271`, `:319`, `:346` | O C6 e a partição reais recebem `N(...)` sem relação `decisoes`. `chamados/models.py:4318`, `chamados/services/pergunta_no_app.py:87` |
| `chamados/tests/test_e4b_classe_e_memoria.py:303`, `:334`, `:374`, `:386` e derivados `test_e4b_c11_memoria.py:37`, `test_e4b_c4_tranca_memoria.py:27`, `test_e4b_c4v14_memoria.py:46`, `test_e4b_c4v7_memoria.py:86`, `test_e4b_c6_memoria.py:16`, todos em `chamados/tests/` | O módulo simulado fornece `DecisaoPergunta=NS()` sem `composta`. O namespace da projeção real também fornece a constante de identidade só na classe simulada. Os casos de sentinela e mutantes que executam esses mesmos caminhos podem parar antes da asserção. `chamados/tests/test_e4b_classe_e_memoria.py:254`, `chamados/tests/test_e4b_classe_e_memoria.py:1039` |
| `chamados/tests/test_e2_tarefa_do_dia.py:938`, `chamados/tests/test_o261_julgamento_desfaz_carimbo.py:204`, `escala/tests/test_f3_beco1_folga_na_lista.py:402`, `ponto/tests/test_selo_chokepoint_escrita.py:435`, `:440`, `:470`, `:475` | Tetos em risco com leituras individuais em cargas ainda sem `decisoes`. O delta da A1 permanece [nao verificado]. `chamados/services/tarefa_do_dia.py:530`, `chamados/services/relavra_via.py:236`, `chamados/models.py:4318`, `ponto/services/cartorio.py:146` |

## Escritores e leitores dos campos

| Campo | Quem escreve | Quem lê e significado |
|---|---|---|
| `validada_em` | Validação do admin grava coluna e ato. Devolução, resposta humana subsequente e reabertura podem retirar a validação. Há carimbo por absorção na porta do modelo. `chamados/services/validacao.py:161`, `:273`, `:279`, `chamados/services/disputa_emissao.py:1494`, `:1532`, `:1617`, `:1639`, `chamados/models.py:3340` | O compositor entrega a validação vigente ao juiz temporal e ao C6. `estado`, sem veredito e a face SQL também leem a validação vigente. O significado é o mesmo, com divergência possível entre ato e coluna. `chamados/models.py:2011`, `:3244`, `:3548`, `:4330`, `:4345`, `chamados/juizes.py:1391`, `:1410` |
| `respondida_em` | Resposta humana, automática e carimbo pela trilha gravam o instante. Reabertura e recusa limpam a resposta vigente. Há carimbo projetado na porta do modelo. `chamados/services/disputa_emissao.py:1528`, `:1532`, `:1617`, `:1639`, `:1683`, `:1686`, `:1705`, `:1708`, `chamados/models.py:3337` | A1 julga o instante vigente via compositor. `_slot_esperado` também mantém sua comparação com a coluna e os chamadores transitórios entregam a coluna. O campo representa o instante da resposta, mas essas leituras podem divergir sobre a vigência. `chamados/models.py:4328`, `:4334`, `:4346`, `chamados/juizes.py:353`, `:1395`, `:1557`, `chamados/services/fio.py:190` |
| `resposta_colab` | Respostas humana e automática escrevem o texto corrente, e reabertura/recusa limpam a coluna. A trilha guarda o texto que cada ato registrou. `chamados/services/disputa_emissao.py:1481`, `:1532`, `:1617`, `:1639`, `:1682`, `:1686` | Compositor, juízes e fio usam o texto corrente. `resposta_display` usa o texto da sequência e o placar usa a projeção SQL. Há significados distintos quando ato e projeção divergem. `chamados/models.py:3825`, `:4346`, `chamados/juizes.py:1392`, `:1558`, `chamados/services/fio.py:190`, `chamados/services/raiox.py:135` |
| `materializada_em` | As portas `carimbar` e `carimbar_identidade` gravam o carimbo. `descarimbar` e sua porta em lote o retiram. `chamados/models.py:3302`, `:3334`, `:3387`, `:3413`, `:3496` | Compositor, projeção temporal e C6 leem o mesmo carimbo projetado. A projeção temporal o recorta por `agora` e o C6 interpreta seu efeito junto da via. `chamados/models.py:4347`, `chamados/juizes.py:1393`, `:1399`, `:1421` |
| `via_resolucao` | As portas de carimbo e descarimbo gravam ou limpam a via. `encerrar_por_fato_no_espelho` pode trocar apenas a via. `chamados/models.py:3306`, `:3387`, `:3413`, `:3496`, `:3505` | Compositor, projeção temporal, C6 e mérito do fio leem o motivo projetado. O significado do campo é compartilhado. O corte conserva as vias de identidade fora do fato mesmo quando o carimbo fica depois do horizonte. `chamados/models.py:2588`, `:4347`, `chamados/juizes.py:1399`, `:1421`, `chamados/services/fio.py:1299` |
| `decisoes` | A FK define a relação. `DecisaoPergunta.registrar` insere as linhas e invalida o cache da instância recebida. `chamados/models.py:4176`, `:4350`, `:4360`, `:4366` | `leitura` consome a sequência ordenada, com o passivo, e `composta` usa os instantes vigentes daí. O significado compartilhado é trilha de atos. A regra de vigência pertence a `leitura`. `chamados/models.py:4318`, `:4322`, `:4327`, `:4345` |

O anexo 1735 item 7-iv informa `MUDAM=0 em 51.563` para a property, com referência a `medir_1710/r.txt`. Valor de produção [nao verificado].

LEI-AKITA origem=`chamados/juizes.py:1383` e `chamados/juizes.py:1549` + `chamados/models.py:4339`, testemunha=`chamados/tests/test_e4c_f5_selo_chamada.py:203`, RED=`chamados/tests/test_e4c_f5_voltaA_sitios.py:65`, padrão=juiz único puro + fonte única (compositor) + fitness function.

## 19/11 18:40 · sessao 29cd68d25a8d · E4C-F5-CONSTROI · sonda · origem=Code

Não executei novamente a sonda. A porta cria um arquivo temporário e grava na trilha, e a tarefa proíbe alterações em arquivos. (`bin/sonda_leitura.sh:216`, `bin/sonda_leitura.sh:217`, `bin/sonda_leitura.sh:248`, `bin/sonda_leitura.sh:249`)

**Saída literal raspada fornecida** [nao verificado]

```text
sonda_leitura: sonda.py [e54f137c79492f08] no cpuset 4-7, lendo sombra como leitor (LEITURA)
sonda_leitura: arvore=/home/ronald/wt-f5c-170736/app (copia), banco segue sombra como leitor

BANCO=sombra
UNIVERSO=51563 PERCORRIDAS=51563 ERROS=0
MUDA_RESPONDIDA=0 AMOSTRA=[]
MUDA_ESTADO=0 AMOSTRA=[]
MUDA_SEM_VEREDITO=0 AMOSTRA=[]
DISPLAY_DIF=0 CRU_DIF=0 PKS_DISPLAY=[]
VEREDITO=VALIDO MUDAM=0 DISPLAY_DIF=0
FIM_SONDA_1E
```

- `VEREDITO=VALIDO`. Sim, `ERROS=0` e `UNIVERSO==PERCORRIDAS` (`51563==51563`). [nao verificado]
- Todos os `MUDA_*` são `0`, com `AMOSTRA=[]` nos três campos. [nao verificado]
- `DISPLAY_DIF=0`, `CRU_DIF=0` e `PKS_DISPLAY=[]`. [nao verificado]

## 19/11 18:38 · sessao 7c56dccf9316 · CADEIA-20-PASSO-1 · construtor · origem=Code

**CADEIA-20-PASSO-1 · Volta 3 implementada na cópia.** A v5 começa na competência `2026-10`. O cartório usa o caminho normal de impressão e `_em_dia`. A projeção entrega hora e razão ou estado, e o `--apply` imprime o contador da empresa. `ponto/services/cartorio.py:161`, `:1334`, `:1552`, `escala/services/leitor_celula.py:193`, `ponto/management/commands/processar_cartorio.py:75`.

Conferi a sintaxe, os alvos dos mutantes e o censo estático. Os corpos de `versao_para`, `impressao_insumos` e `_regra_do_dia` conferem com a base. Suite, execução dos REDs e mutantes, smoke e medidas de produção ficam **[nao verificado]** para Code e Opus. `ponto/services/cartorio.py:280`, `:493`, `:1334`, `escala/tests/test_cadeia20_volta3_mutantes.py:116`.

A fixture antiga de espúrias do PDF monta escala e batidas em `relatorios/tests/test_cartao_marca_espuria.py:21`. A marcação agora vem da ata projetada. O resultado dessa fixture fica **[nao verificado]**. `relatorios/pdf_espelho.py:595`.

### Comandos para a sessão

Os REDs cobrem a relavra da 10 e a impressão da 09, os três estados do aval, a conferência por ocorrência e a entrada de um leitor cru. `escala/tests/test_cadeia20_volta3.py:42`, `:73`, `:142`, `:160`, `:166`, `:172`, `escala/tests/test_cadeia20_selo_orfas.py:224`.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  escala.tests.test_cadeia20_volta3 \
  escala.tests.test_cadeia20_selo_orfas \
  ponto.tests.test_o219_versao_da_regra \
  ponto.tests.test_e3_isento_na_impressao
```

Cada chamada abaixo executa um mutante. O harness exige que o RED passe com o código original e falhe na asserção indicada após a troca em memória. `escala/tests/test_cadeia20_volta3_mutantes.py:67`, `:116`.

```bash
mutante() {
  CADEIA20_V3_MUTANTE="$1" PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
    escala.tests.test_cadeia20_volta3_mutantes
}
mutante sem_v5
mutante v5_na_09
mutante estado_antigo_apagado
mutante estado_do_horizonte_errado
mutante falha_apagada
mutante rotulo_trocado
mutante espelho_descarta_sobras
mutante calendario_so_hora
mutante card_so_hora
mutante pdf_so_hora
mutante contador_perde_chave
mutante contador_ignora_ordem
mutante apply_omite_contador
mutante catraca_aceita_leitor
```

### Inventário

| Item | Mudança e localização |
|---|---|
| V3-1 | Declaração da v5 em `ponto/services/cartorio.py:161`. Ajustei os testes existentes que fixavam o teto ou o valor vivo da competência 10 em `ponto/tests/test_o219_versao_da_regra.py:79` e `ponto/tests/test_e3_isento_na_impressao.py:93`. |
| V3-2 | Mapa de rótulos em `escala/orfa_classe.py:6`. Projeção por ocorrência e entrega por dia em `escala/services/leitor_celula.py:193`, `:454`. |
| V3-3 | O serviço usa a projeção sobre as células já carregadas e entrega `orfas_grade` em `ponto/services/espelho.py:152`, `:704`, `:862`. O adaptador preserva o contrato dos demais chamadores em `escala/utils.py:1099`. Calendário e card em `templates/ponto/espelho.html:151`, `:165`, `:249`. Coleta, marcação, texto e legenda do PDF em `relatorios/pdf_espelho.py:478`, `:595`, `:874`, `:1013`. |
| V3-4 | Contador com número e PKs em `ponto/services/cartorio.py:1038`. Saída ao fim do `--apply` em `ponto/management/commands/processar_cartorio.py:75`. |
| V3-5 e V3-6 | Catraca e injeção de leitor cru em `escala/tests/test_cadeia20_selo_orfas.py:11`, `:220`, `:224`. REDs e mutantes em `escala/tests/test_cadeia20_volta3.py:42`, `:142` e `escala/tests/test_cadeia20_volta3_mutantes.py:67`. |

**Linha de base do selo — 5 Python, 0 HTML.** Os sítios contados são `colaboradores/services/calendario.py:410`, `escala/utils.py:1131`, `ponto/juiz_batida.py:161`, `ponto/services/cluster_espurio.py:31` e `relatorios/cartao_pela_celula.py:288`. Tetos em `escala/tests/test_cadeia20_selo_orfas.py:11`.

- **`ata['razoes_orfas']`** é escrita por `_lavrar_razoes_sobras` em `ponto/services/cartorio.py:968`, `:1001`, pelos dois ramos em `:1434`, `:2015`. Leem a chave a projeção, `medir_razoes_sobras` e o contador do horizonte. `escala/services/leitor_celula.py:202`, `ponto/services/cartorio.py:1013`, `:1049`.
- **`REGRA_VERSOES`** é declarada em `ponto/services/cartorio.py:161`. Leem a tupla a validação no módulo e `versao_para`. O cartório, a projeção e o contador consultam `versao_para`. `ponto/services/cartorio.py:269`, `:293`, `:1047`, `:1334`, `escala/services/leitor_celula.py:219`.

## 19/11 18:26 · sessao 42a726cd1503 · E4C-F5-CONSTROI · verificador · origem=Code

### 1c-1 — SEGUE

O ORM cru continua aceito pelo corpo puro. `pergunta_respondida` lê as colunas `p.respondida_em` e `p.resposta_colab`. A projeção temporal lê também `p.validada_em`, `p.materializada_em` e `p.via_resolucao`, e o C6 usa o par já recortado por `agora`. A cópia atual lê os **instantes** pelo ato no ramo ORM. A equivalência com o `main` de hoje fica [nao verificado]. `app/chamados/juizes.py:1389`, `app/chamados/juizes.py:1430`, `app/chamados/juizes.py:1559`, `app/chamados/models.py:2634`, `app/chamados/models.py:2748`, `app/chamados/models.py:3647`.

- **Erro versus valor.** Nenhum chamador ORM examinado exige o `isinstance` para ter os atributos que o juiz puro acessa. A regressão possível é de valor. Os REDs atuais mostram ato e coluna divergindo para resposta, reabertura e validação. `app/chamados/tests/test_e4c_f5_volta2_juizes.py:37`, `app/chamados/tests/test_e4c_f5_volta2_juizes.py:93`.
- **Harnesses.** Sim, o teste de tipo é a causa registrada. Reproduzi em memória o `NameError` quando o harness omite `PerguntaDisputa` e o `TypeError` quando injeta um `SimpleNamespace` no lugar da classe. Ambos passam desse ponto com o ramo removido. O verde das 11 famílias com o compositor ligado na A fica [nao verificado]. `app/docs/RELATO.md:11`, `app/chamados/tests/e4_apoio.py:19`, `app/chamados/tests/e3_c8b_apoio.py:83`.
- **Aval e alcance.** A passagem intermediária com leitura de coluna está declarada. A certificação única após C é compatível com o pouso único registrado para o 0815 e com o selo final do 1735. A prévia recebe o efeito de leitura tanto no juiz de resposta quanto em `fio_vivo`. Escrita em exportada, dinheiro ou `!` fora do declarado não foi identificada nesses caminhos. O cotejo integral com 1710 e 0815 e a prova da competência 11 ficam [nao verificado]. `respostas_f5_constroi_2_1911_1830.txt:3`, `respostas_f5_constroi_2_1911_1830.txt:4`, `respostas_f5_constroi_1911_1735.txt:3`, `app/docs/RELATO.md:45`, `app/folha/previa.py:139`.

### 7-iv — PROCEDE (TÉCNICO)

A tabela fecha em **cinco `False` e dois `True`**. A condição que não fecha é a prova atribuída a `:505–509`. Ali `estado` e `q_vivas` distinguem a validação do ato, mas a exclusão por `sem_veredito_entre` já ocorre pelo texto vazio. Nenhuma das três asserções distingue a fonte do instante da resposta. `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:490`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:505`, `app/chamados/models.py:2004`, `app/chamados/models.py:3235`, `app/chamados/models.py:3613`.

| Leitor | Esperado | Caminho na fixture |
|---|---|---|
| `sem_veredito_entre` | `False` | O instante do ato chega ao juiz com texto corrente vazio. O juiz rejeita o par. `app/chamados/models.py:2004`, `app/chamados/juizes.py:1564` |
| CSF | `False` | Consulta pai e destino pelo mesmo `sem_veredito_entre`, com `ignorar_tranca=True`. `app/ponto/supra_juiz.py:573` |
| Irmãs | `False` | Aplica `sem_veredito_entre` ao universo de crédito, também com `ignorar_tranca=True`. `app/chamados/juizes.py:2127` |
| `estado` | `False` | Sem validação, sem materialização e com o par rejeitado, o rótulo é `PENDENTE`. O teste compara com `RESPONDIDA`. `app/chamados/models.py:3235`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:327` |
| Property `respondida` | `False` | Delega ao juiz, que rejeita o texto corrente vazio sob o contrato novo. `app/chamados/models.py:3786`, `app/chamados/juizes.py:1564` |
| `resposta_display` | `True` | Lê `07:13` do ato e o devolve na fixture de texto com opções vazias. `app/chamados/models.py:3815`, `app/chamados/models.py:3834`, `app/chamados/tests/cortes_f2_f9_apoio.py:62` |
| `q_vivas` | `True` | Passa pela ausência de validação e entra pelo ramo largo da célula `furo`, que acusa. `app/chamados/models.py:3611`, `app/escala/models.py:1767`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:360` |

- **Literal da sonda.** A sonda existente compara os textos brutos. A property pode converter IDs e valores em nomes ou rótulos. Comparar sua saída com a coluna bruta pode acionar o `PAREI` mesmo com ato e projeção alinhados. A contagem pedida fica [nao verificado]. `app/chamados/services/vigia_texto_resposta.py:16`, `app/chamados/models.py:3815`, `app/chamados/models.py:3834`, `respostas_f5_constroi_2_1911_1830.txt:6`.
- **Aval e alcance.** A tabela é compatível com o contrato do 1735 e com o corte posterior ao 0815 registrado no RELATO. O item altera expectativas de leitura e mantém o display fora da F5. Não identifiquei escrita em exportada, dinheiro ou `!` fora do declarado. O cotejo integral com 1710 e 0815 fica [nao verificado]. `respostas_f5_constroi_1911_1735.txt:9`, `respostas_f5_constroi_2_1911_1830.txt:6`, `app/docs/RELATO.md:34`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:500`.

## 19/11 18:21 · sessao 5976fa43d10f · CADEIA-20-PASSO-1 · verificador · origem=Code

## AVAL-DEVOLVIDO por item — CADEIA-20-PASSO-1

O mecanismo de versão funciona sem `--forcar` e sem editar `impressao_insumos`. Ele **muda o hash gravado**. O 1815 precisa explicitar esse efeito, distinguir ausência de chave dentro e fora do horizonte e corrigir o literal de `padrao=` que o selo rejeita. `app/ponto/services/cartorio.py:518`, `app/ponto/services/cartorio.py:575`, `app/ponto/services/cartorio.py:1528`, `bin/tests/test_lei_akita.sh:337`.

A cópia traz a referência ao CONSTRÓI 1010. O anexo `aval_1010.txt:1` é o RESPOSTA-LEI 2320. O PRONTO e o PROIBIDO integrais do CONSTRÓI, o histórico Git e o estado atual da produção ficam [nao verificado]. `app/docs/PROMPTS.md:2592`, `app/docs/PROMPTS.md:2616`.

### Desenho

- **(a) SEGUE.** A volta 3 é pré-requisito do pouso. Os quatro pontos têm as sobras em horas, ainda sem a razão. O censo da catraca também precisa enxergar o calendário do HTML e a leitura do PDF por dia que marca a batida órfã. CHECK-DE-TELA (d) e smoke de três colabs [nao verificado]. `app/ponto/services/espelho.py:703`, `app/ponto/services/espelho.py:858`, `app/templates/ponto/espelho.html:151`, `app/templates/ponto/espelho.html:242`, `app/relatorios/pdf_espelho.py:477`, `app/relatorios/pdf_espelho.py:552`, `app/relatorios/pdf_espelho.py:607`, `laudo.md:2`.
- **(b) PROCEDE.** O default vigente é **2026-10 em diante**. A função retorna v4 da 10 em diante, v1 na 09 e `None` até a 08, pelo mês do fim da janela de cada empresa. Com corte 21, começa em **21/09/2026**, abrange 10, 11 e as seguintes. O seletor usa um piso fixo. `app/ponto/services/cartorio.py:161`, `app/ponto/services/cartorio.py:277`, `app/ponto/services/cartorio.py:290`, `app/ponto/janelas.py:10`, `app/ponto/janelas.py:60`.
- **(b), caminho real.** Uma `(5, '2026-10')` chega por `_regra_do_dia` a `regra:5` no hash. No ramo normal, `_em_dia` fica falso e o fluxo passa pelo `continue` até a lavra. No `nunca_bateu`, `imp0` faz o mesmo. O comando passa `forcar=False` por padrão e lavra com `--apply`. A cópia ainda tem v4. `app/ponto/services/cartorio.py:161`, `app/ponto/services/cartorio.py:518`, `app/ponto/services/cartorio.py:1310`, `app/ponto/services/cartorio.py:1375`, `app/ponto/services/cartorio.py:1410`, `app/ponto/services/cartorio.py:1517`, `app/ponto/services/cartorio.py:1528`, `app/ponto/services/cartorio.py:1988`, `app/ponto/management/commands/processar_cartorio.py:25`, `app/ponto/management/commands/processar_cartorio.py:67`.
- **(b), estado nomeado.** Chave ausente e `razoes_orfas: []` são distinguíveis. A ausência também ocorre quando faltam insumos e pode aparecer dentro do horizonte. O texto “anterior ao horizonte” exige confirmar a competência da célula. O contador existente recebe uma lista sem recorte e conta chave ausente só quando `orfas` tem elementos. `app/ponto/services/cartorio.py:959`, `app/ponto/services/cartorio.py:1001`, `app/escala/utils.py:2117`, `app/escala/utils.py:2131`.

### MEDIR na sombra

| Item | Veredito e motivo |
|---|---|
| **1** | **PROCEDE.** O universo é definível por ocorrência em `ata['orfas']`, com `n_orfas` identificando atas agregadas que só têm a contagem. Recorte pela competência da célula. O contador atual pode fechar a conservação com chave ausente ou `razao=None` e falha nomeada. Para o literal `com_razao == universo`, ambos precisam zerar, com alinhamento por célula e universo não vazio. O resultado atual é [nao verificado]. `app/ponto/services/cartorio.py:761`, `app/ponto/services/cartorio.py:1001`, `app/escala/services/leitor_celula.py:180`, `app/escala/orfa_classe.py:74`. |
| **2** | **SEGUE.** Compare rubricas por colab e competência no mesmo recorte de MAIN e CÓPIA, após o caminho real. O motor lê lâmpadas e papéis da ata, e o fechamento lê os minutos da grade para realizado e abono. Esses são sítios que podem mover valores se a relavra mudar esses insumos. O RELATO registra 618 colabs com zero delta num ensaio anterior. O zero com a volta 3, sem a flag, é [nao verificado]. `app/ponto/motor_calculo_v2.py:495`, `app/ponto/motor_calculo_v2.py:611`, `app/ponto/juiz_batida.py:149`, `app/ponto/services/fechamento.py:1013`, `app/ponto/services/fechamento.py:1060`, `app/docs/RELATO.md:16`. |
| **3** | **SEGUE.** Compare os campos persistidos após a primeira e a segunda chamada com os mesmos insumos e relógio. A entrada roda sem `--forcar` e o comando ainda pode acionar a segunda passada interna forçada das reconciliadas. O teste da obra força a chamada e compara a ata. O zero no caminho exigido é [nao verificado]. `app/ponto/services/cartorio.py:1128`, `app/ponto/portas/celula.py:1181`, `app/escala/tests/test_cadeia20_costura.py:31`, `app/escala/tests/test_cadeia20_costura.py:55`. |
| **4** | **SEGUE.** Conte células únicas que o bump fez sair da guarda, com um controle na mesma base para descontar a fila normal, e cronometre a primeira passada. O comando varre todos os dias passados da empresa e o total impresso inclui outros motivos de julgamento. Quantidade e minutos [nao verificado]. `app/ponto/management/commands/processar_cartorio.py:49`, `app/ponto/management/commands/processar_cartorio.py:63`, `app/ponto/management/commands/processar_cartorio.py:68`, `app/ponto/services/cartorio.py:1528`. |

### Mecânicos, verificador e proibido

- **(3) SEGUE.** O corte fornecido e o 1815 autorizam registrar a pergunta já cortada, hoje implementada por `escala/orfa_classe.py::razao_da_sobra`. Faltam o registro e a frase em CORTES. O selo de juiz exige a forma `corte Ronald: juiz razao_da_sobra nasce` em `CORTES.md`. `corte_razao_da_sobra.txt:1`, `cadeia_20_volta_3_e_horizonte_1911_1815.txt:7`, `app/escala/orfa_classe.py:29`, `app/core/juizes.py:1324`, `bin/tests/test_juiz_novo_tem_corte.sh:27`, `bin/tests/test_juiz_novo_tem_corte.sh:37`.
- **(4) PROCEDE.** A linha nos commits é exigência do aval. O selo pula commits sem linha, e seu validador rejeita o literal `projecao reconstruivel + horizonte (L-124)`. Conferi com o vocabulário da cópia. `Backfill / Reprocessing` é aceito e consta no ADR da L-124. Os 13 de 15 são do laudo, sem reconferência do histórico [nao verificado]. `bin/tests/test_lei_akita.sh:110`, `bin/tests/test_lei_akita.sh:337`, `bin/tests/test_lei_akita.sh:523`, `app/docs/PLANO-PADROES.md:75`, `laudo.md:5`.
- **VERIFICADOR — SEGUE.** O escopo é o das três voltas e a linha item a item antecede a suite. O laudo confirma que leitores, catraca, CHECK-DE-TELA e smoke ficaram pendentes. O checklist integral do 1010 fica [nao verificado]. `cadeia_20_volta_3_e_horizonte_1911_1815.txt:8`, `laudo.md:2`, `app/docs/BACKLOG.md:2044`.
- **PROIBIDO — PROCEDE.** “Entrar na impressão” precisa significar preservar o código do hash e deixar a versão já prevista mudar seu valor. “Rejulgar anterior” precisa significar **por efeito da obra**. A fila ainda julga por fato novo e o escritor atual roda em qualquer competência que passe pela guarda. O 2320 fornecido veda mexer no motor ou no pareador e tomar vaga ou suite da espinha. Os demais limites do CONSTRÓI e o cruzamento de diffs com F5 ou FERIADO ficam [nao verificado]. `app/docs/LEIS.md:145`, `app/ponto/services/cartorio.py:518`, `app/ponto/services/cartorio.py:1410`, `app/ponto/services/cartorio.py:1532`, `app/ponto/services/cartorio.py:1991`, `aval_1010.txt:1`, `cadeia_20_volta_3_e_horizonte_1911_1815.txt:9`.

**Exportada, dinheiro e `!`.** O teste real é K6 por empresa e competência, pela existência de TXT vigente e não invalidado. A cópia registra medições antigas com exportadas até a 09 e registra a 10 como paga fora do sistema nas empresas 2, 3 e 4. A exportação vigente de cada empresa no horizonte hoje é [nao verificado]. A relavra escreve diretamente `CelulaDia` e pode emitir `EventoCelula`. O recálculo é quem grava `FechamentoMensal`. A relavra também pode mudar veredito e protestos, que a folha lê para reter. L-113 retirou a recusa técnica de recálculo. O gravado exportado segue sob L-092 e a regra consolidada exige `!` e porta própria se houver movimento. Zero rubricas no cálculo e zero campos gravados são duas provas distintas. `app/folha/export.py:556`, `app/folha/export.py:588`, `app/ponto/services/cartorio.py:224`, `app/docs/CORTES.md:529`, `app/ponto/portas/celula.py:941`, `app/ponto/portas/celula.py:1181`, `app/folha/export.py:1273`, `app/ponto/services/fechamento.py:1137`, `app/docs/LEIS.md:125`, `app/docs/LEIS.md:155`, `CLAUDE.md:404`.

### PRONTO na cópia

| Item | Veredito e motivo |
|---|---|
| **1** | **PROCEDE.** O registro termina em v4. `app/ponto/services/cartorio.py:161`. |
| **2** | **PROCEDE.** A distinção existe no dado, mas a projeção hoje entrega horas e o template só as imprime. `app/ponto/services/cartorio.py:963`, `app/escala/services/leitor_celula.py:420`, `app/templates/ponto/espelho.html:242`. |
| **3** | **PROCEDE.** Há função de censo, sem recorte interno por horizonte e sem chamada no comando. Zero [nao verificado]. `app/ponto/services/cartorio.py:1001`, `app/ponto/management/commands/processar_cartorio.py:61`. |
| **4** | **PROCEDE.** As quatro medidas publicadas na árvore final e no caminho pedido ficam [nao verificado]. O registro anterior traz outro commit e relata 19 mudanças na segunda passada daquele ensaio. `app/docs/RELATO.md:5`, `app/docs/RELATO.md:16`, `laudo.md:3`. |
| **5** | **PROCEDE.** O juiz existe e é chamado na lavra. Permanecem pendentes o registro e o corte. `app/escala/orfa_classe.py:29`, `app/ponto/services/cartorio.py:991`, `laudo.md:4`. |
| **6** | **PROCEDE.** A publicação do PRONTO integral, item a item, antes da suite fica [nao verificado]. `cadeia_20_volta_3_e_horizonte_1911_1815.txt:8`. | [nao verificado]

**Pela metade, faltam 6 itens adicionais comprovados.** A contagem acima cobre os seis itens acrescentados pelo 1815. O total com o PRONTO integral do 1010 fica [nao verificado]. `cadeia_20_volta_3_e_horizonte_1911_1815.txt:10`, `app/docs/PROMPTS.md:2616`.

### Quem escreve e quem lê

- **`ata['razoes_orfas']`.** `_lavrar_razoes_sobras` escreve a chave nos dois ramos do cartório e `lavrar_veredito` com `carimbar` a persiste. `medir_razoes_sobras` lê a chave e tem chamadas em testes. Na cópia, zero chamador de produção do contador e zero leitor de produção da chave encontrado. `app/ponto/services/cartorio.py:959`, `app/ponto/services/cartorio.py:998`, `app/ponto/services/cartorio.py:1015`, `app/ponto/services/cartorio.py:1410`, `app/ponto/services/cartorio.py:1991`, `app/ponto/portas/celula.py:1090`, `app/ponto/portas/celula.py:1201`, `app/escala/tests/test_cadeia20_costura.py:52`, `laudo.md:2`.
- **`ata['regra']` e `versao_para`.** Zero escritor ou leitor dessa chave na ata encontrado. A versão entra como `regra:N` no hash de `CelulaDia.impressao`. Em produção, `_regra_do_dia` é o único chamador direto encontrado. Ele alimenta os dois ramos. Há leitores em testes. Chamados e a migração de identidade citam o horizonte em comentários, com piso próprio. `app/ponto/services/cartorio.py:749`, `app/ponto/services/cartorio.py:518`, `app/ponto/services/cartorio.py:1310`, `app/ponto/services/cartorio.py:1375`, `app/ponto/services/cartorio.py:1520`, `app/ponto/portas/celula.py:1195`, `app/ponto/tests/test_o219_versao_da_regra.py:85`, `app/chamados/models.py:2582`, `app/chamados/migrations/0053_pergunta_unica_por_familia.py:8`.

## 19/11 17:47 · sessao 7f0d1e40588a · E4C-F5-CONSTROI · verificador · origem=Code

# AVAL-DEVOLVIDO — E4C-F5-CONSTROI, aval 1735

**1 DESENHO — PROCEDE. 6 RED — PROCEDE em (b). 7 ADAPTAÇÕES DE TESTE — PROCEDE.** O bloqueio é a entrada em vigor dos juízes puros na A com chamadores crus em B/C, a falta de uma base definida para a catraca e de cenários nomeados para os REDs, e uma lista de adaptações que deixa de fora asserções incompatíveis com o desenho. `respostas_f5_constroi_1911_1735.txt:2–4`, `:8–11`, `chamados/juizes.py:1389–1397`, `:1559–1564`, `chamados/tests/test_f1nova_b2_leitores.py:87–97`.

Conferi a cópia somente leitura. A identidade do commit, da ref e do merge está [nao verificado]. O wrapper documenta a extração da árvore por hash com `git archive`. `bin/codex_pergunta.sh:304–320`.

## 1 DESENHO — PROCEDE

**(a) SEGUE quanto ao escopo declarado.** `:2004` monta `LeituraDoAto` e `:2006` entrega ao juiz o **texto do ato**, incluindo o passivo incorporado pela leitura. A lista exige também ausência de validação e materialização e aplica a tranca, salvo `ignorar_tranca=True`. `estado` monta a mesma leitura em `:3235` e passa o **texto do ato** ao juiz em `:3241` e `:3244`. Para decidir o ramo, `estado` ainda chama C6 com `self` em `:3240`, caminho que usa **texto da projeção** e pode reler o ato. `chamados/models.py:1998–2007`, `:3233–3244`, `:4295–4326`, `chamados/juizes.py:1389–1411`.

- **CSF e Pauta leem `sem_veredito_entre`.** A função do CSF chama a lista para o pai e para o destino, ambas com `ignorar_tranca=True`, e devolve os PKs ordenados. A medição da tranca também pede essa lista com `True`, via `respostas_sem_veredito`, e ainda chama C6 em dois pontos e `pode_fechar` no fechamento de disputas. Nenhuma dessas duas funções lê `PerguntaDisputa.estado`. A API e o vigia leem `estado`. `ponto/supra_juiz.py:552–588`, `chamados/services/competencia_trancada.py:119–144`, `:164–183`, `chamados/models.py:1979–1980`, `api/views.py:2566–2585`, `chamados/services/vigia_fio.py:256–260`.
- **Alcance de escrita.** A lista guarda `fechar`, o CSF e a retenção da retratação. Em competência trancada, `medir` monta `decidir_pelo_dp`; no ramo de aplicação, `_pauta_do_dp` lê essa lista e pode gravar a Pauta por `pautas.services.escrever`. Esses efeitos chegam pelas portas existentes. Seu resultado em produção e eventual efeito no gravado de exportada ou em dinheiro estão [nao verificado]. `chamados/models.py:2009–2015`, `:2048–2058`, `:2095–2099`, `ponto/management/commands/supra_juiz.py:257–287`, `chamados/juizes.py:2124–2129`, `chamados/reconciliador.py:439–452`, `chamados/services/competencia_trancada.py:137–139`, `:188–257`, `pautas/services.py:180–185`, `CLAUDE.md:265–268`, `:401–417`.

**(b) SEGUE com a descrição corrigida.** `values_list` produz uma tupla de cinco colunas. O juiz recebe um `SimpleNamespace` montado com `p[3]` e `p[4]`, o instante e o texto **da projeção**. `verbos.py` consome o par de conjuntos produzido pelo placar. Já existe a entrada `E4D-PROJECAO-UM-ESCRITOR` para o complemento nominal previsto no aval. O registro efetivo está [nao verificado]. `chamados/services/raiox.py:135–158`, `chamados/services/verbos.py:34`, `docs/BACKLOG.md:2023`, `respostas_f5_constroi_1911_1735.txt:3`.

**(c) PROCEDE.** A varredura AST da cópia encontrou **41 expressões diretas** dos dois juízes. A linha 15 da matriz aponta `reconciliador.py:1130`, onde se ajusta o relógio. A chamada está em `:1133`. Pela regra do arquivo do chamador, a distribuição é esta. `respostas_f5_constroi_1911_1735.txt:3–5`, `chamados/reconciliador.py:1127–1137`.

| Volta ou dono | Linhas da matriz A e chamadas na cópia |
|---|---|
| **A — 12** | 3–5 `chamados/juizes.py:351`, `:1411`, `:1430`. 14–15 `chamados/reconciliador.py:366`, `:1133`. 20–21 `chamados/services/disputa_emissao.py:1853`, `:2023`. 22–24 `chamados/services/fio.py:712`, `:1287`, `:1300`. 30 `chamados/services/pergunta_no_app.py:86`. 41 `ponto/services/cartorio.py:1689`. |
| **B — 20** | 1–2 `api/views.py:2596`, `:2640`. 16 `chamados/services/acoes_disputa.py:134`. 17 `chamados/services/becos.py:31`. 18–19 `chamados/services/declaracao_texto.py:418`, `:688`. 25–29 `chamados/services/materializacao.py:192`, `:259`, `:290`, `:885`, `:1300`. 32–33 `chamados/services/validacao.py:682`, `:902`. 34 `chamados/services/vigia_fio.py:636`. 35 `chamados/views.py:1275`. 36–37 `chamados/views_cobrar.py:170`, `:178`. 38 `colaboradores/services/ficha.py:172`. 39 `escala/services/cadastro_realidade.py:502`. 40 `folha/previa.py:142`. |
| **C — 4** | 6–8 `chamados/management/commands/auditar_invariantes_chamados.py:178`, `:597`, `:1671`. 9 `chamados/management/commands/dedup_perguntas_fato.py:76`. |
| **Opus — 4** | 10–13 `chamados/models.py:2006`, `:3241`, `:3244`, `:3791`. `:2004` e `:3235` preparam os argumentos. |
| **Exceção — 1** | 31 `chamados/services/raiox.py:147`. |

- **Os 55 indiretos são entradas, não 55 conversões.** Contei 53 expressões Python para `pergunta_viva`, incluindo a implementação da propriedade `.viva`, e os dois usos externos da propriedade. No cálculo, `:1411` recebe a pergunta e `:1430` recebe o par já recortado pelo horizonte. Memo quente pode devolver antes do cálculo. Validação, tranca e via terminal também podem sair antes de `:1430`. A matriz tem referências deslocadas no reconciliador após o merge. `chamados/models.py:3629`, `templates/chamados/partials/_card_atendimento.html:77`, `chamados/management/commands/arquivar_competencia_encerrada.py:158`, `chamados/juizes.py:887–898`, `:1398–1430`, `chamados/reconciliador.py:711`, `:808`, `:1137`, `:1645`.
- **Pureza e tipo próprio conferem com o 1710 e o 1510.** Hoje ambos fazem `isinstance` e chamam `DecisaoPergunta.leitura` no ramo ORM. A projeção temporal também depende da constante de via do modelo e lê cinco atributos. `LeituraDoAto` tem três campos e devolve o texto do ato. Remover só os ramos ORM deixaria a constante do modelo no juiz temporal. O contrato da composição precisa preservar materialização, via, o corte por `agora` e o par já recortado entregue em `:1430`. `chamados/juizes.py:1382–1430`, `:1551–1564`, `chamados/models.py:19`, `:2580`, `:2701`, `:4308–4326`, `e4c_f5_constroi_a_1911_1710.txt:4`, `respostas_f5_e_c7_1911_1510.txt:2–3`.
- **O literal da sequência A/B/C quebra a autoridade do instante.** Se a A retira a leitura do ato do corpo dos juízes, as **24 chamadas ORM** programadas para B/C passam a entregar os instantes das colunas até sua volta. Isso também alcança o temporal de C11 em `:502`, enquanto K2 e o RED de C11 estão em A. Falta dizer em que commit entram em vigor os corpos puros e a conversão de **todas** essas entradas, com a suíte verde exigida entre voltas. Além disso, a linha 40 e K23 pedem edição em `folha/previa.py`. O 0815 proíbe tocar folha e a exceção do 1710 descreve carga em lote, enquanto a linha 40 é a própria chamada ao juiz. Falta autorização nominal para a edição em folha e para eventuais mudanças de rota ou partição fora do recorte declarado. `chamados/juizes.py:1389–1397`, `:1559–1564`, `escala/services/cadastro_realidade.py:460–466`, `:502`, `escala/tests/test_f3_beco1_folga_na_lista.py:395–402`, `folha/previa.py:39–43`, `:64–67`, `:139–146`, `e4c_f5_constroi_1911_0815.txt:3`, `e4c_f5_segue_as_tres_voltas_1911_1435.txt:16`, `e4c_f5_constroi_a_1911_1710.txt:3–8`, `respostas_f5_constroi_1911_1735.txt:3–4`.

**(d) PROCEDE.** Na cópia original são **41 chamadas sem compositor, 40 fora da exceção do placar**. O commit do Opus antecede a A e já tem ordem para trocar quatro expressões. “Medida na abertura da A” deixa indefinido se a referência é o hash original ou a árvore entregue pelo Opus. Para a catraca só encolher, falta fixar a árvore de referência, impedir entrada nova mesmo quando outra sai e definir como o AST reconhece a origem de uma leitura atribuída a variável e a derivação temporal de `:1430`. O texto da exceção deve identificar o adaptador de `raiox.py:147`, que é quem entra no juiz. O 1735 dá autorização nominal para esse sítio no selo novo. O selo de leitores crus do 0815 tem outro objeto e seu veto geral a allowlist continua pertinente ali. `respostas_f5_constroi_1911_1735.txt:3–4`, `chamados/models.py:2004–2006`, `:3235–3244`, `:3791`, `chamados/juizes.py:1411–1430`, `chamados/services/raiox.py:135–147`, `e4c_f5_constroi_1911_0815.txt:3`.

**(e) PROCEDE.** Há entradas reais de leitura. `q.estado` devolve um **rótulo**. `DisputaSupervisao.sem_veredito_entre([q], ignorar_tranca=…)` devolve uma **lista**, da qual se observa se o PK aparece. Falta fixar no literal a comparação do rótulo inteiro e da presença do PK nas duas modalidades da tranca. Medir só com o padrão pode ocultar mudança para CSF, retratação e Pauta, que pedem `True`. O `MUDAM=0 em 51.563` fornecido é da property `respondida`. Os dois resultados estendidos, os PKs e a identidade da sombra estão [nao verificado]. O contador de divergência literal de texto mede outra coisa. `chamados/models.py:1982–2007`, `:3225–3244`, `:3785–3791`, `ponto/supra_juiz.py:573–574`, `chamados/juizes.py:2127–2129`, `chamados/services/competencia_trancada.py:123–139`, `chamados/services/vigia_texto_resposta.py:8–18`, `e4c_f5_constroi_a_1911_1710.txt:10`, `respostas_f5_constroi_1911_1735.txt:3`, `:9`.

**Quem escreve e quem lê os dados da composição**

| Campo ou valor | Escrita e leitura |
|---|---|
| Texto e instante da projeção | Resposta humana e automática gravam o par. A trilha pode gravar o instante sobre texto existente, e reabertura/recusa limpa o par pela porta de lote. Os juízes hoje leem o texto corrente no ramo ORM. O placar lê as duas colunas. `chamados/services/disputa_emissao.py:1481–1496`, `:1527–1534`, `:1617–1656`, `:1680–1710`, `chamados/juizes.py:1395–1397`, `:1559–1564`, `chamados/services/raiox.py:135–147`. |
| Instantes e texto do ato, mais passivo | Resposta, validação, devolução e recusa/reabertura alimentam `DecisaoPergunta.registrar`, que insere as linhas e invalida o cache recebido. `leitura` combina as linhas com o passivo, que lê `respondida_em`, `validada_em`, `validada_por_id` e texto da projeção. Os leitores afetados estão em `:2004`, `:3235` e `:3791`. `chamados/services/disputa_emissao.py:1532–1534`, `:1639–1656`, `chamados/services/validacao.py:161–175`, `:269–282`, `chamados/models.py:2004–2006`, `:3235–3244`, `:3791`, `:4251–4269`, `:4295–4347`. |
| `materializada_em` e `via_resolucao` | `_escrever_carimbo` atribui os campos. `carimbar`, `carimbar_identidade` e `descarimbar` os gravam pelas portas existentes. A projeção temporal lê ambos, e `estado` e a lista sem veredito leem a materialização. `chamados/models.py:3293–3297`, `:3320–3348`, `:3373–3408`, `:2005`, `:3236–3244`, `chamados/juizes.py:1397–1407`. |

## 6 RED — PROCEDE em (b)

**(a) SEGUE.** O teste captura o **SQL total da porta real** com 1 e 3 PKs. Hoje exige `Q(3) = 3 × Q(1)` e `Q(1) ≤ 8`. É teto **por candidata**. A porta abre transação, trava e relê por candidata, e C6 lê a relação `decisoes`. O 1735 autoriza nominalmente `Q(3)/3 = Q(6)/6 ≤ 9` para O261, inclusive diante do veto geral do 1710. A triagem relata 9 no teste anterior. O SQL da sonda e o resultado em 3 × 6 estão [nao verificado]. A fixture validada exercita o caminho de recusa. Em produção, `desfazer_carimbo_sem_lastro` lê carimbo, via e C6, e pode chamar `descarimbar` e reabrir a disputa. O aval aqui muda a métrica do teste, com `DecisaoPergunta.leitura` como fronteira da leitura do ato. `chamados/tests/test_o261_julgamento_desfaz_carimbo.py:187–205`, `chamados/services/relavra_via.py:215–262`, `chamados/models.py:3399–3409`, `:4308`, `e4c_f5_constroi_a_1911_1710.txt:8–9`, `:13`, `respostas_f5_constroi_1911_1735.txt:8–11`, `triagem.md:14`.

**(b) PROCEDE.** As **cinco funções L** existem. Três pertencem à A ou ao seu commit preparatório, `CargaDaTarefa.__init__` (K1), `alimentar_portas` (K2) e `_ancoradas` (K15). Duas pertencem à B, `perguntas_vivas_de` (K4) e `fio_vivo` (seis K). O predesenho lista **22 Q de B**, incluindo K41/K42 no commit do Opus, e **25 M de C**, mais a sobra. O 1735 pede três mutantes Q e três M, mas **não nomeia os seis IDs**, nem diz em que suíte se prova a mordida dos três L de A. `chamados/services/tarefa_do_dia.py:518–540`, `chamados/juizes.py:1512–1526`, `:1639–1672`, `chamados/models.py:343–359`, `:1542`, `:2057`, `chamados/management/commands/dedup_perguntas_fato.py:74–76`, `e4c_f5_constroi_a_1911_1710.txt:6–8`, `respostas_f5_constroi_1911_1735.txt:7–8`.

- **A igualdade total não fecha para todos os K sob a instrução de só carregar decisões em lote.** K40 faz um `.get(pk=pk)` dentro do laço antes de julgar, portanto o SQL total já cresce por pergunta mesmo com decisões previamente disponíveis. `fio_vivo` faz `any`, que pode julgar apenas a primeira pergunta nas duas medidas. A sobra faz uma carga por grupo, logo o total depende também da quantidade de grupos. Faltam a unidade de captura por cenário, a prova de que 3 e 6 perguntas foram efetivamente julgadas, e a autorização de qualquer alteração de carga além das decisões. O selo AST verifica formato de chamada. Ele não prova custo SQL nem substitui a mordida de cada caso do 1510. `chamados/management/commands/migrar_data_evento_dia_do_turno.py:448–467`, `chamados/juizes.py:1517–1526`, `chamados/management/commands/dedup_perguntas_fato.py:74–82`, `respostas_f5_e_c7_1911_1510.txt:3–4`, `e4c_f5_constroi_a_1911_1710.txt:7–9`, `respostas_f5_constroi_1911_1735.txt:3`, `:8`.
- **Escrita e alcance dos cenários.** As cargas leem `PerguntaDisputa`; o consumo das decisões ocorre em `leitura`. K14 percorre exportadas com `executar=False`. A sobra tem relatório de leitura e um ramo `--apply` que despacha ao backfill. O RED precisa identificar qual caminho mede. Prova de custo em leitura não verifica equivalência de valores exportados, dinheiro ou execução das portas escritoras [nao verificado]. `chamados/models.py:4308–4326`, `chamados/management/commands/backfill_fio_morto_e4b.py:153–175`, `chamados/management/commands/dedup_perguntas_fato.py:89–106`, `CLAUDE.md:265–268`, `:401–417`.

## 7 ADAPTAÇÕES DE TESTE — PROCEDE [nao verificado]

| Subitem | Veredito |
|---|---|
| **(i) SEGUE** | São os dois REDs 8. A fixture tenta criar o texto sem instante antes de `atos` chamar `D.registrar`. `save` a rejeita antes das capturas. O segundo RED está em `:167–180`, com o medidor em `:151–165`. A autorização é para a fixture. Contagem 1 × 5, zero SQL no cálculo com prefetch e conferência do cache fazem parte da prova. `chamados/tests/test_e4c_f5_volta2_juizes.py:27–30`, `:118–180`, `chamados/models.py:3680–3683`, `:3707–3712`, `chamados/services/fio.py:1718–1721`, `respostas_f5_constroi_1911_1735.txt:9`. |
| **(ii) SEGUE** | `:37–104` chama os juízes com ORM cru. A fixture escreve atos por `registrar` e altera a projeção por `update` explícito. Os asserts leem a tupla e o booleano, incluindo passivo, reabertura, futuro e texto em branco. O caso seguinte já usa objeto sintético e vigia que a leitura do ato jamais seja chamada para ele. `chamados/tests/test_e4c_f5_volta2_juizes.py:27–116`. |
| **(iii) SEGUE** | O mutante exige uma ocorrência literal de `_replace` e troca o argumento de `respondida_sem_texto` pela pergunta inteira renomeada para `questao`. O mutante atualizado precisa continuar chegando ao assert de **pergunta inteira** do selo 3d. Falhar na busca da âncora não prova a mordida. A transformação opera sobre texto em memória. `chamados/tests/e4c_f5_volta2_mutantes.py:1–9`, `:12–39`, `chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:187–196`, `:220–227`, `chamados/services/fio.py:187–192`, `e4c_f5_segue_as_tres_voltas_1911_1435.txt:12`. |
| **(iv) PROCEDE** | O teste escreve uma `RESPOSTA` com `ato_texto='07:13'` e atesta a projeção vazia. O assert atual exige `True` para **sete** leitores. Pelo desenho, `sem_veredito_entre`, CSF, irmãs, `estado == RESPONDIDA` e a property passam a dar `False` nessa fixture. `resposta_display` continua lendo o texto do ato, e `q_vivas` continua incluindo a pergunta pelo ramo largo da célula furada. Falta explicitar as expectativas por leitor e conservar a prova do instante e da validação pelo ato. Inverter os sete quebraria o contrato. O `MUDAM=0` da property não certifica os outros leitores. `chamados/tests/test_e4c_f2_leitura_do_ato.py:313–365`, `:490–509`, `chamados/models.py:2004–2007`, `:3235–3244`, `:3609–3616`, `:3791`, `:3815`, `chamados/juizes.py:2127–2129`, `respostas_f5_e_c7_1911_1510.txt:2–3`, `respostas_f5_constroi_1911_1735.txt:9`. |
| **(v) SEGUE** | A adaptação nominal é a do O261 descrita em 6(a). A captura lê o total de SQL da porta com PKs de fixtures validadas. `chamados/tests/test_o261_julgamento_desfaz_carimbo.py:187–205`, `respostas_f5_constroi_1911_1735.txt:8–9`. |

**O que falta na autorização de 7.** O teste B2 exige que `respondida` chame o juiz exatamente com `DecisaoPergunta.leitura(p)`. O tipo próprio exigido pelo 1735 muda justamente esse argumento. O selo de `_ancoradas()` mede **exatamente uma consulta** com retorno não vazio, enquanto a carga de decisões de K15 no mesmo caminho acrescenta um SELECT por lote. O 1710 admite remedir custo constante por lote, mas a lista fechada do 1735 só nomeia o O261 entre as adaptações de métrica. Tarefa e C11 ainda medem 1 × 3 e 1 × 5 dias. Falta dizer se o RED 3 × 6 entra como caso novo ou se os dois testes existentes podem ser adaptados. `chamados/tests/test_f1nova_b2_leitores.py:75–97`, `chamados/tests/test_e4c_f2_leitura_do_ato.py:534–551`, `chamados/models.py:343–359`, `chamados/tests/test_e2_tarefa_do_dia.py:913–938`, `escala/tests/test_f3_beco1_folga_na_lista.py:378–402`, `e4c_f5_constroi_a_1911_1710.txt:8–9`, `respostas_f5_constroi_1911_1735.txt:3`, `:9–11`.

**As 11 famílias ainda precisam passar com seus dublês atuais.** Há harness que recompila C6 com globais próprios e há stub de `chamados.models` que fornece só `ChamadoColaborador` para a rota de C11. A pureza do juiz resolve a dependência de tipo dentro dele. A compatibilidade desses chamadores com uma nova dependência de composição e o verde das famílias estão [nao verificado]. As alterações de fixture, mutante e métrica são de teste. Qualquer efeito das mudanças de produção sobre as portas de fechamento, Pauta, exportada ou dinheiro continua sujeito às cercas e às medidas correspondentes. `chamados/tests/e3_c8b_apoio.py:83–120`, `chamados/tests/test_e3_r3_c6_sem_banco.py:284–312`, `chamados/tests/test_e4b_c4v14_memoria.py:43–53`, `chamados/tests/test_e4b_classe_e_memoria.py:377–394`, `e4c_f5_constroi_1911_0815.txt:3`, `CLAUDE.md:265–268`, `:401–417`.

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

_7424 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
