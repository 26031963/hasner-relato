# CODEX-ESTADO

> Ultima resposta processada: **27/11/2026 01:31** · publicado em **27/11/2026 01:34** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 27/11 01:31 · sessao d67b8bc0162f · E4C-F5-INSTANTE · construtor · origem=Code

Escrevi 12 testes em [test_e4c_f5_c4_red.py](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:1). Cada caso começa pela regra, pelo cadastro e pelo resultado esperado. A alteração ficou no arquivo novo, com zero `except` novos. [nao verificado]

A execução do módulo parou no import, antes das fixtures. O Python local devolveu `ModuleNotFoundError: No module named 'django.contrib'`, rc 1. A suíte da cópia usa PostgreSQL. Em memória, executei dois métodos do arquivo novo e oito recortes das funções reais da cópia. A saída foi `Ran 10 tests`, `FAILED (failures=10)`, rc 1, todas por `AssertionError`. As três provas de integração indicadas na tabela seguem **[nao verificado]**. `app/chamados/tests/test_e4c_f5_c4_red.py:9`, `app/config/settings/ci.py:6`

| Achado | Teste em `test_e4c_f5_c4_red.py` | Saída RED | Cura esperada | [nao verificado]
|---|---:|---|---|
| Carimbo aceita validação vigente em T2 com coluna em T1 | `:36` | `(True, None) != (False, 'par_ato_coluna')` | `chamados/models.py::DecisaoPergunta.par_ato_coluna` e `PerguntaDisputa.carimbar` devem conferir o instante vigente. `models.py:3499`, `models.py:4646` |
| Absorção aceita o mesmo par divergente | `:93` | **[nao verificado]**. O teste exige recusa por PK, trilha e ausência de `ABSORCAO`. | `chamados/services/materializacao.py::_materializar_ausencia_de_resposta` deve usar a mesma comparação por instante. `materializacao.py:1108`, `materializacao.py:1115` |
| Falha ao ler a foto vaza o carimbo em memória | `:59` | Instante, autor `7` e via `juiz_aprovou` ficaram na instância. | `chamados/models.py::PerguntaDisputa.carimbar` deve ler a foto antes de alterar a instância ou incluir essa leitura na proteção existente. `models.py:3465`, `models.py:3473`, `models.py:3490` |
| `ato_em` pendente escapa ao horizonte da tarefa | `:117` | O horizonte de 12h viu a validação pendente de 13h. | `chamados/models.py::DecisaoPergunta.leitura` e `chamados/services/tarefa_do_dia.py::_fatos_das_perguntas` devem limitar o candidato ao contexto do carimbo e respeitar o horizonte da tarefa. `models.py:4549`, `tarefa_do_dia.py:416` |
| Coluna retrodatada em disputa nova conta como estoque | `:160` | `[('b', 'respondida', False)] != [('b', 'respondida', True)]` | `chamados/models.py::DecisaoPergunta.divergencia_ato_x_coluna` deve usar a abertura posterior da disputa como prova de que a pergunta nasceu depois do marco. `models.py:1921`, `models.py:4615` |
| Porta aceita estoque no marco e depois | `:173` | `ValueError not raised` nos dois instantes. | `chamados/models.py::DecisaoPergunta._conferir_estoque` deve admitir somente o campo classificado como `b` de estoque. `models.py:4615`, `models.py:4800` |
| Porta aceita foto de campo `a` com ato vigente | `:183` | `ValueError not raised`. | `chamados/models.py::DecisaoPergunta._conferir_estoque` deve recusar a foto e preservar a autoridade do ato vigente para a reprojeção. `models.py:4611`, `models.py:4801` |
| Porta aceita dia civil de conversa como dia do fato | `:196` | `ValueError not raised`. | `chamados/models.py::DecisaoPergunta._conferir_estoque` deve excluir conversa sem competência de fato e consultar o K6 existente para as elegíveis. `models.py:3191`, `models.py:4813`, `backfill_decisoes_pergunta.py:74` |
| Payload de estoque atribui a foto ao carimbo | `:205` | `ValueError not raised`. | `chamados/models.py::DecisaoPergunta._conferir_estoque` deve recusar o motivo adulterado. `models.py:4469`, `models.py:4781`, `models.py:4785` |
| Foto histórica ganha origem de ato novo | `:215` | `'ato' != 'estoque'` no payload com o default do modelo. | `chamados/models.py::PerguntaDisputa.carimbar` e `DecisaoPergunta.registrar` devem preservar a proveniência da foto e a entrada própria da validação nova. `models.py:3473`, `models.py:3526`, `models.py:4392` |
| Entrega pode intercalar a leitura do K6 e o `INSERT` | `:234` | **[nao verificado]**. A prova usa duas conexões e a trava real de entrega. | A entrada do estoque em `chamados/models.py::DecisaoPergunta.registrar` deve serializar com `folha/export.py::travar_entrega`, respeitando Empresa antes de Colaborador. `models.py:4737`, `models.py:4742`, `folha/export.py:556` |
| Item 13 por PK com entrega real pelo K6 | `:298` | **[nao verificado]**. O teste verifica as três portas, atos, colunas, trilha e hash do mês do fato. | `chamados/services/validacao.py::validar_pergunta` e `chamados/services/materializacao.py::_materializar_uma` devem registrar o bloqueio com autor e PK. A recusa serve de controle com a trilha já emitida pela porta. `validacao.py:792`, `materializacao.py:1981`, `validacao.py:1017` |

**Achados sem RED adicional**

- Regravação retrodatada ou limpeza em pergunta já existente no marco pode produzir a mesma foto persistida que um passivo legítimo. O teste usa a abertura da disputa como evidência apenas no caso em que ela resolve a ambiguidade. As fixtures antigas que pretendem simular estoque também precisam representar uma disputa anterior ao marco se a cura usar essa evidência. `app/chamados/models.py:4600`, `app/chamados/tests/test_e4c_f5_instante_c1.py:83`
- A 0058 já recorta `entrada_em <= marco`, com teste na função real. Ordenação dos relógios no deploy e recuperação de timestamps sobrescritos seguem **[nao verificado]**. `app/chamados/migrations/0058_decisao_entrada_do_ato_historico.py:16`, `app/chamados/tests/test_e4c_f5_instante_c1.py:370`
- O censo de `RESPOSTA`, a devolução L-130, a colisão Q0.2 e a saída DRY já têm casos próprios. A contagem e a lista de PKs reais, o efeito monetário e o DIFF de frota seguem **[nao verificado]**. `app/chamados/tests/test_f2_resposta_mantem_validacao.py:20`, `app/chamados/tests/test_e4c_f5_instante_c1.py:234`, `app/chamados/tests/test_e4c_f5_instante_c1.py:340`, `app/chamados/tests/test_e4c_f5_instante_c1.py:142`, `app/docs/LEIS.md:155`
- O relógio da passagem da ausência tem um contrato próprio de horizonte, e o `max` dos atos compensadores está previsto no contrato V2. A janela de candidato foi testada. Uma mudança na data de `ABSORCAO`, o contrato geral de ato futuro e a escolha do escritor semântico da validação automática requerem conciliar os contratos citados. `app/chamados/services/materializacao.py:1047`, `app/chamados/services/materializacao.py:1100`, `app/chamados/models.py:4691`, `app/docs/BACKLOG.md:2171`, `app/docs/JUIZES.md:38`, `app/chamados/models.py:3484`
- Vigia e DRY duplicam a consolidação por PK, com a mesma regra observável no código atual. Extrair a consolidação por si só não dá um RED de comportamento. `app/chamados/services/vigia_ato_x_coluna.py:24`, `app/chamados/management/commands/estoque_ato_x_coluna.py:44`

**Comando para a sessão**

```bash
bin/suite.sh --dir /home/ronald/wt-codex-e4c-f5-instante-012038 \
  --only "chamados.tests.test_e4c_f5_c4_red"
```

Para os mutantes, após o verde, reintroduzir o comportamento da primeira coluna uma vez por caso e rodar o seletor correspondente. A aplicação e a execução dos mutantes seguem **[nao verificado]**. A certificação fica com o Code e o opus final. `app/docs/LEIS.md:147`

```bash
red() {
  bin/suite.sh --dir /home/ronald/wt-codex-e4c-f5-instante-012038 \
    --only "chamados.tests.test_e4c_f5_c4_red.$1"
}

red PortaDoCarimboTest.test_RED_carimbo_recusa_validacao_vigente_em_outro_instante
red AbsorcaoDoParTest.test_RED_absorcao_preserva_irma_com_validacao_em_instante_divergente
red PortaDoCarimboTest.test_RED_falha_da_foto_preserva_o_carimbo_em_memoria
red HorizonteDoCarimboTest.test_RED_candidato_futuro_nao_existe_no_horizonte_da_tarefa
red EstoqueDoParTest.test_RED_coluna_retrodatada_em_disputa_nova_conta_como_b_novo
red EstoqueDoParTest.test_RED_porta_estoque_recusa_instante_no_marco_ou_depois
red EstoqueDoParTest.test_RED_porta_estoque_recusa_a_com_ato_vigente
red EstoqueDoParTest.test_RED_porta_estoque_recusa_dia_civil_de_conversa
red EstoqueDoParTest.test_RED_porta_estoque_nao_grava_motivo_de_ato_do_carimbo
red EstoqueDoParTest.test_RED_foto_anterior_carimbada_hoje_conserva_origem_historica
red EstoqueConcorrenteComEntregaTest.test_RED_entrega_nao_intercala_k6_e_insert_do_estoque
red Item13PorPKTest.test_RED_item13_portas_publicas_em_entrega_vigente_com_trilha_por_pk
```

## 27/11 01:29 · sessao de4cf674062e · RAMO2-B-EM-TURNO-QUE-CRUZA · verificador · origem=Code

**REPROVADO.** A releitura v3 paga **20 min de HE50 indevida** no caso da pausa na cauda. O diff também altera o motor do Partido, que o aval (a) mandou manter intocado. A catraca c2k reproduziu as três verificações em memória. [ponto/calculador/periodo.py:745](/home/ronald/codex_ws/7f954d9211ae5382b1aae074c32c1fe4683e0fd6/app/ponto/calculador/periodo.py:745) · [app/docs/PROMPTS.md:3087](/home/ronald/codex_ws/7f954d9211ae5382b1aae074c32c1fe4683e0fd6/app/docs/PROMPTS.md:3087) · [core/tests/test_medida_turno_do_par.py:548](/home/ronald/codex_ws/7f954d9211ae5382b1aae074c32c1fe4683e0fd6/app/core/tests/test_medida_turno_do_par.py:548)

### Parcial e cauda

Valores em minutos. Nos três casos, a janela está ligada, com piso 0, teto 60, cauda pendente e dia comum. A parcial abate 30 do atraso bruto de 50 **antes** do teto. As duas pontas distam 50 e 20 dos marcos, de modo que a L-084 mantém o julgamento. A base vai até o marco, o trabalhado inclui a cauda e o teto é 30. `app/docs/LEIS.md:79–80,111,115–116` · `app/docs/PROMPTS.md:3031` · `app/ponto/motor_calculo_v2.py:1242–1307` · `app/ponto/tests/test_ramo2b_c2h_saida.py:133–161`

| Achado | Arquivo:linha | Contraexemplo e números | Procede? |
|---|---|---|---|
| **12x36 — o caso da c2j cobre só `do_dia`** | `app/ponto/tests/test_ramo2b_c2h_saida.py:133–161` · `app/ponto/calculador/regras.py:614–626,785–808,882–886` | Cadastro 19h–07h, previsto 660, pausa 00h–01h. Batidas E19h50, S00h, E01h, S07h20. Parcial 19h20–19h50. **Regra e `do_dia` executado em memória:** trabalhado 630, base 610, intra 0, atraso 20, antecipada 0, HE 0. Sem parcial, atraso 30. O teste injeta a base, a pausa e a tupla de pontualidade prontas. | **Não procede** como erro de `do_dia` nesse caso. **Procede** como lacuna do teste ponta a ponta. |
| **Comercial — o mensal troca o previsto da célula por 480** | `app/escala/utils.py:2284,2322–2328` · `app/ponto/tests/test_ramo2b_turno_que_cruza.py:62–66` · `app/ponto/motor_calculo_v2.py:3430,3555–3568,3617–3653` | Mesmo caso, em tipo Comercial diurno com célula congelada 19h–07h e pausa 00h–01h. **Regra e `do_dia`:** trabalhado 630, base 610, intra 0, atraso 20, HE 0. **Mensal:** o limite fica em 480 porque a célula cruza. `610 − 480 = 130`, sendo **120 HE50 + 10 HE100**. Executei o override com o período e o dia fornecidos. | **Procede contra L-024**, `app/docs/LEIS.md:75`. O limite fixo é herdado. A grade injeta 660 e deixa o mensal fora da prova, `app/ponto/tests/test_ramo2b_c2h_saida.py:62–77,91–113`. |
| **Partido — mudança de dinheiro fora do aval (a)** | `app/docs/PROMPTS.md:3087` · `app/ponto/motor_calculo_v2.py:1888–1905,3103–3118,3164–3167,3216–3227,3319–3326,3353–3365` | Partido real 19h–07h, vão 00h–03h, previsto 540, régua relógio. E19h50, S00h, E03h, S07h20. Parcial 19h20–19h50. **L-097(b) e caminho atual:** trabalhado 510, base 490, intra 0, atraso 20, HE 0. **Com o Partido preservado como no lado antigo do diff:** trabalhado 490, fora 20, atraso 20, HE 0. O diff também troca a base usada no cálculo mensal da intra, dobra e ganho noturno. | **Procede contra o escopo do aval (a).** Os 20 a mais de trabalhado correspondem à L-097(b). O bloqueio é aplicar a mudança ao Partido nesta entrega. |

### Achados no diff e nos leitores

| Achado | Arquivo:linha | Contraexemplo e números | Procede? |
|---|---|---|---|
| **A releitura v3 calcula HE com dois recortes incompatíveis** | `app/ponto/calculador/periodo.py:745–759,832–836,862–864` · `app/ponto/tests/test_ramo2b_c2_base_da_he.py:301–338` | Caso `DECL_60`: 19h–07h, previsto 660, pausa cadastrada 00h–01h, E19h, pausa interna julgada 07h20–07h40, S08h20. **Regra:** trabalhado 780, base 720, intra 60, HE 0. **`reler` executado:** 780, base 720, intra **40**, HE50 **20**. A segunda leitura fica só com o trabalhado recortado. A intra gravada usa a pausa do par integral e baixa o limite de 720 para 700. | **Procede contra L-031, L-032 e L-097(b)** e contra a resposta específica, `app/docs/PROMPTS.md:3071`. Regressão no caminho de ensaio, `app/ponto/calculador/periodo.py:543–548`. Efeito no gravado [nao verificado]. |
| **Pausa julgada vira trabalhado quando a base é curta** | `app/ponto/motor_calculo_v2.py:2968–2999,3449–3485` · `app/ponto/calculador/alimentacao.py:133–147` | 12x36 19h–07h, E01h10, S08h20, pausa 07h20–07h40 entregue dentro do par e dia atribuído a D. Base 350, bruto 430. **Regra:** trabalhado 410, pausa 20, HE 0. **Motor:** 430, pausa descontada 0, HE 0. No Comercial, E03h10 com o mesmo fim e a mesma pausa dá base 230 e **310 em vez de 290** trabalhados. O desconto do trabalhado integral ficou dentro do gatilho da duração **da base**. | **Procede contra L-032 e L-097(b)** na porta `calcular_periodo`, executada com a pausa julgada. Formação automática desses pares pela sequência de batidas [nao verificado]. |
| **A releitura v3 perde a parcial** | `app/ponto/calculador/periodo.py:434–435,464–477,797–825` · `app/ponto/calculador/alimentacao.py:535–566` | No 12x36 da primeira tabela, o leitor de ensaio chama o juiz de pontualidade sem a parcial. Calcula `min(50, 660 − 630) = 30`. A regra e `do_dia` dão **20**. | **Procede contra o corte do abono**, `app/docs/PROMPTS.md:3031`. O teto L-093 é respeitado. O repasse faltava no caminho de ensaio anterior ao diff. |
| **HE noturna no motor ainda termina na batida crua** | `app/ponto/motor_calculo_v2.py:458–479,1084–1094,3052–3072` · `app/ponto/calculador/alimentacao.py:106–115` · `app/ponto/calculador/regras.py:850–865` | 12x36 16h–04h, extra declarada de 60 desloca o corte para 05h, sem prorrogação pós-05h. E16h20, S05h30, previsto 720. Base 760, trabalhado 790, HE50 40. **Na base:** 04h20–05h, **40 noturnos**. **Na ponta crua do motor:** 04h50–05h30, **10 noturnos**. A mesma função, executada com as duas pontas, devolveu 40 e 10. | **Procede contra L-035 e L-097(b)**, `app/docs/LEIS.md:83,115`. O motor já lia a batida crua. O oráculo mantém o fim recortado. |
| **Export mede o dia civil, mesmo com o dia do turno disponível** | `app/folha/porta_export.py:340–362,584–591` · `app/ponto/services/dia_pago.py:109–132,174` | Competência termina em 20/11. Turno de 20/11, 23h50–07h50, pausa prevista 02h–03h, previsto 420. E00h02, S08h20 em 21/11, pausa sem gozo. Base 468, trabalhado 498, intra 60. Injetando HE indevida de 30 na testemunha, o teto da jornada é **0** e deveria sair **1** diagnóstico. A função retorna **0** porque descarta a entrada de 21/11. | **Procede contra L-085 e L-002**, `app/docs/LEIS.md:60,117`. O agrupamento civil é herdado. A extração o manteve. |
| **A troca da função muda a medição do export** | `app/folha/porta_export.py:348–362,587–591,713–723` · `app/ponto/tests/test_ramo2b_c2_base_da_he.py:717–746` | No par que a testemunha consegue agrupar, base 720, trabalhado 800, previsto 720, intra 60 e declarado 0. Para uma HE indevida de 80, o teto antigo era **80**, com **0** diagnósticos. O teto novo é **0**, com **1** diagnóstico. A entrada da testemunha continua vindo de `esp['resultado'].periodos`. Com intra 0, a mesma HE passa por essa testemunha, que só mede HE junto com intra. A pendência tem outra entrada no portão. | **Procede** a mudança do valor lido. **Não procede** atribuí-la à troca de identidade do registro AST. O alcance da prova é a testemunha HE × intra, `app/folha/porta_export.py:354–355,548,706–723`. |
| **`trab_por_dia` do Comercial virou base de HE e continua alimentando saldo** | `app/ponto/motor_calculo_v2.py:3558–3560,3655–3685` | Semana de cinco dias com célula 22h–06h, base 480 por dia e uma cauda pendente de 30. Soma trabalhada 2.430, HE 0. Pela fórmula do saldo sobre o trabalhado, `2.430 − 2.640 = −210`. Pelo mapa da base, o código calcula **−240**. | **Procede** como campo com dois significados, à luz da separação da L-097(b), `app/docs/LEIS.md:115`. O saldo já excluía esses 30 quando o trabalhado antigo vinha recortado. |
| **A tripla aceita escrita fora do tripwire** | `app/ponto/motor_calculo_v2.py:207–211,244–246,280–285,1927–1943` · `app/ponto/tests/test_ramo2b_c2_base_da_he.py:627–638,705–714` | Par 19h–08h20, trabalhado 800. O construtor gerado aceita `minutos_base_da_he=720` sozinho e produz `(720, None, None)`. O leitor numérico vê **720**. O leitor geométrico, sem fim da base, conserva o par de **800**. A porta rejeita a tripla parcial, o construtor a aceita. | **Procede contra L-007**, `app/docs/LEIS.md:65`. Chamador de produção que passe a tripla ao construtor [nao verificado]. |
| **Os dois controles do Partido deixam o cálculo que deveriam vigiar fora do teste** | `app/ponto/tests/test_ramo2b_c2h_saida.py:62–77,123–131` · `app/escala/servico_jornada.py:130–164` · `app/ponto/motor_calculo_v2.py:3184–3227` | Os controles forçam a classe com vão de 60 e chamam `calcular_periodo`. Num Partido real 19h–07h com vão cadastrado 00h–03h, batidas 19h, 00h20, 03h, 07h30, o mensal deve indenizar **20** pelo vão de 160. Um mutante que zere esses 20 deixa os dois controles intactos. | **Procede como selo sem mordida sobre o mensal**, `CLAUDE.md:28–29`. Há outros testes do caminho real do Partido, `app/ponto/tests/test_ramo2b_turno_que_cruza.py:151–164,216–225`. |

### Quem escreve e quem lê

| Campo ou valor | Escrita e leitura na cópia | Semântica que importa |
|---|---|---|
| `PeriodoCalculo.minutos_base_da_he` | 12x36, Partido e Comercial medem por par e passam a `_gravar_base_da_he`, que chama `_escrever_base_da_he`. O bloqueio de HE100 limpa e o crédito R3 agrega pela mesma porta. `_trabalhado_da_base_da_he` serve ao R3, às bases mensais do Partido e do Comercial, e à testemunha do export. `app/ponto/motor_calculo_v2.py:280–285,1612–1620,1927–1956,2747–2765,2998–2999,3113–3116,3167,3216,3320–3325,3484–3485,3560,3585–3589` · `app/folha/porta_export.py:337–351` | Minutos de relógio por par. `None` lê `minutos_trabalhados`. No R3, a escrita passa a representar a **soma de pares fechados** num portador aberto. `app/ponto/motor_calculo_v2.py:2755–2765` |
| `saida_da_base_da_he` | A janela produz o instante, `_janela_do_par` o limita ao par efetivo e a porta grava. Leem o R3, o split e a segmentação da alimentação, e a releitura v3. `app/ponto/motor_calculo_v2.py:1893–1924,1942,2751–2752` · `app/ponto/calculador/alimentacao.py:114–115,168–179` · `app/ponto/calculador/periodo.py:749–756` | Instante de fim da base, já deslocado pela extra declarada. Par todo na cauda usa a entrada como limite e tem base 0. No R3, o instante vem do último par creditado com cauda. Alcance real desse caso R3 [nao verificado]. `app/ponto/janela_he.py:192–200` · `app/ponto/motor_calculo_v2.py:1921–1923,2758–2765` |
| `ganho_noturno_da_base_da_he` | `_gravar_base_da_he` calcula legais menos relógio até o fim da base. A porta grava. `_ganho_noturno_da_base_da_he` alimenta a régua `legais` do Partido. R3 fornece zero. `app/ponto/motor_calculo_v2.py:287–293,1943–1956,2762–2765,3354–3362` | Minutos de ganho da hora reduzida. `None` usa legais menos relógio do par. Zero explícito no R3 é o valor do portador aberto. Mesmas referências. |
| Argumentos `minutos_base_da_he` e `pausa_base_da_he_min` de `do_dia` | `segmentos_do_motor` recorta pelo instante, `minutos_do_oraculo` soma segmentos e vãos, e `rubricas_por_dia` passa ambos por dia da jornada. `do_dia` usa a dupla no piso, na intra e na HE/dobra. Fechamento e `diff_calculador` consomem a saída. `app/ponto/calculador/alimentacao.py:78–88,165–179,469–474,545–548` · `app/ponto/calculador/nucleo.py:133–154` · `app/ponto/calculador/regras.py:614–626,772–808` · `app/ponto/services/fechamento.py:1291–1319` · `app/ponto/management/commands/diff_calculador.py:414–424` | Minutos **do dia**, derivados de novo a partir do instante. O núcleo mede todo vão entre segmentos do mesmo dia. Uma pausa que começa no marco dá 0 à base. Uma que termina no marco inclui o tempo anterior. A sentinela de duração zero conserva o fim do vão. A terceira derivação, em `reler`, é a que diverge em **40/20 contra 60/0**. `app/ponto/calculador/alimentacao.py:173–179` · `app/ponto/calculador/periodo.py:745–759,834–836` |
| `parciais` de `do_dia` | O DP grava as pontas pela porta de ausência. `credito_parcial` seleciona ausências pagas com período. A alimentação passa a lista do dia a `do_dia`, e o contexto a entrega ao teto do motor. O motor também carrega a lista pela mesma fonte. `app/ponto/services/ausencia.py:1039–1058,1099–1104` · `app/ponto/services/credito_parcial.py:15–32,54–70` · `app/ponto/calculador/alimentacao.py:535–566` · `app/ponto/calculador/regras.py:117–127,204–205,280–282,882–886` · `app/ponto/motor_calculo_v2.py:1326–1343` | Lista de ausências, com leitura das horas declaradas. `minutos` participa da classificação como parcial. O abate mede a cobertura da ponta em fatias `[k,k+1)` desde 00h do dia da jornada. A virada vale 1.440+. O juiz do **marco previsto** inclui as duas bordas. O juiz da **batida** usa a direção, com S no início e E no fim fora. `app/ponto/turnos.py:2622–2639,2653–2711` · `app/ponto/motor_calculo_v2.py:481–505,1242–1275` |

### Catraca, travas e alcance

| Achado | Arquivo:linha | Contraprova | Procede? |
|---|---|---|---|
| **C2k — censo real** | `app/core/tests/test_medida_turno_do_par.py:194,250–260,288,304–313,348–350,425,548–577` · `app/core/medida_turno_do_par.py:426–444` | Rodei os três testes da c2k com `unittest` em memória. **3 passaram.** Censo de 782 arquivos: A1/A2 **6/15**, B1/B2 **78/192**, totais **9** pareadores e **81** derivações. Os sítios nomeados são **558, 937 e 2601**. O B1 da folha acompanha `he_acima_do_teto_por_dia:344`. O `.date()` que fica em `medir:391` deriva o dia do relógio capturado. `CLASSIFICAR` recebe o resultado de `B2 − B1` e o teste exige igualdade. | **Não procede** lista de exceção nova ou critério amplo no contador. A atribuição de cada mudança ao commit individual [nao verificado]. A cópia entregue não tem metadados Git [nao verificado]. |
| **`except`, fallback e asserções** | `app/ponto/motor_calculo_v2.py:280–293,1935–1943` · `app/ponto/calculador/alimentacao.py:472–474,547–548` · `app/folha/tests/test_porta_do_export.py:122–145` · `app/ponto/tests/test_contract_juiz_batida.py:382–412` | **0 `except` novos** e nenhum teste inteiro apagado nos hunks anexados. Há `raise ValueError` para dados incompletos na porta. Há fallback de ausência dos novos campos para o par integral e de chave ausente no mapa para zero. A asserção do export passou a procurar texto em duas funções. Uma chamada morta sob `if False` satisfaz o `assertIn`, e o RED de 80 chama só o helper. O AST da impressão do motor exige uma ocorrência em cada classe, com a limitação a `max` literal declarada no próprio teste. | **Procede** a lacuna de integração da testemunha. O fallback da tripla tem o contraexemplo **720/800** acima. Execução da suíte Django completa [nao verificado]. |
| **Relógio, ordem de trava e pré-requisito** | `app/folha/porta_export.py:366–391,529,549,693,1082–1083,1181–1188` · `app/folha/export.py:555–562` · `app/docs/PROMPTS.md:3065,3105` · `CLAUDE.md:421–430` | `medir` captura um instante e o repassa ao espelho, PDF e comando. Os hunks da testemunha operam sobre períodos, datas e cadastros já alimentados. A ordem escrita exige ABONO antes da RAMO2 e certificação antes do pouso. | **Não procede** leitura nova do relógio ou inversão de trava nos hunks exibidos. Pouso efetivo, certificação e ordem real dos pré-requisitos [nao verificado]. |
| **Competência exportada** | `app/ponto/services/fechamento.py:36–50,559–595,1215–1216,1822–1848` · `app/folha/views.py:214–216` · `app/docs/LEIS.md:155–156` | O evento pode atingir o escritor genérico com o motor alterado. A guarda explícita cobre 09/2026 nas empresas que a exportaram. O TXT também tem caminho de recálculo em leitura. Assim, o diff demonstra alcance potencial fora da guarda, inclusive para um caso cujo trabalhado passe de 490 a 510. | **Procede como alcance estático.** Competência exportada efetivamente tocada, antes/depois e hash do gravado [nao verificado]. |

## 27/11 01:29 · sessao fb00a5f2b756 · - · - · origem=Ronald (tmux)

**REPROVADO.** A releitura v3 dá 40 min de intra e 20 min de HE50 onde a regra dá 60 de intra e zero de HE, e o split noturno do motor usa a saída crua em vez do fim da base. `app/docs/LEIS.md:79–80,83,115`, `app/ponto/calculador/periodo.py:745–759,834–836`, `app/ponto/motor_calculo_v2.py:1084–1094`.

| Achado | Arquivo:linha | Contraexemplo | Procedência |
|---|---|---|---|
| A v3 usa a pausa da cauda para reduzir intra e criar HE | `app/ponto/calculador/periodo.py:745–759,834–836`, `app/ponto/tests/test_ramo2b_c2_base_da_he.py:301–334` | 12x36 19–07, previsto 660, pausa declarada 00–01, E19/S08:20, pausa julgada 07:20–07:40 no par. Regra dá trabalho 780, intra 60, HE0. Sonda de `reler` em memória dá 780/40/20. | Procede contra L-031/L-032/L-097. A base recortada é usada na HE e descartada para apurar intra. `app/docs/LEIS.md:79–80,115`. |
| O split noturno do motor usa a saída crua | `app/ponto/motor_calculo_v2.py:458–479,1084–1094`, `app/ponto/calculador/alimentacao.py:106–115` | 12x36 CLT 16–04, extra declarada 60, E16:20/S05:30. Sonda de `calcular_periodo` dá base 760, trabalho 790, HE50 40. Os 40 de HE até 05:00 são noturnos. Motor marca 10, oráculo ancora no marco e marca 40. | Procede contra L-035/L-097 e entre leitores. A leitura crua do motor sobrevive ao diff. `app/docs/LEIS.md:83,115`. |
| Comercial mantém previsto paralelo de 480 no caminho mensal | `app/ponto/motor_calculo_v2.py:3430,3617–3652`, `app/escala/utils.py:2284,2322–2345` | Template 5x2 Comercial diurno, célula 19–07 com pausa 00–01, E19:50/S00/E01/S07:20, parcial 19:20–19:50. Previsto 660, base 610, trabalho 630, atraso 20 e HE0 pela regra. O laço diário isolado escreve HE50 120 + HE100 10. | Procede contra a fonte única L-024. O limite incorreto já estava no código de base do diff. `app/docs/LEIS.md:75`. |
| `reler` não fornece parcial ao teto que empresta | `app/ponto/calculador/periodo.py:434–477,797–802`, `app/ponto/tests/test_ramo2b_c2h_saida.py:133–151` | Caso acima no 12x36. Regra e `do_dia` abatem 30 do atraso bruto de 50 e cobram 20. V3 de ensaio cobra 30 pelo teto `660−630`; nenhuma parcial entra. | Procede contra o corte de abater a ponta antes do teto. O repasse faltava no código-base da v3 de ensaio. `app/ponto/motor_calculo_v2.py:1242–1275,1288–1306`. |
| Pausa julgada vira trabalho quando a base fica abaixo do gatilho | `app/ponto/motor_calculo_v2.py:2968–2988,2998,3449–3471,3484` | Janela 19–07, fim 08:20, pausa interna julgada 07:20–07:40. 12x36 entra 01:10, base 350, trabalho líquido 410, API dá 430. Comercial entra 03:10, base 230, trabalho 290, API dá 310. Sondas diretas com pausa já entregue à porta. O pareamento automático desses quatro eventos [não verificado]. | Procede no contrato da porta contra L-032/L-097(b). `app/docs/LEIS.md:80,115`. |
| A testemunha usa dia civil e perde o último par no corte da competência | `app/folha/porta_export.py:340–359,585–591`, `app/ponto/motor_calculo_v2.py:3200–3224,3374–3381` | Partido dia 20, 19–07, vão 00–03, E19/S00:20, E03/S07:30 no dia 21. Os dois são do dia 20. Previsto 540, base 560, intra 20, teto HE0. Se a testemunha recebe 30 de HE indevida e intra 20 no último par, deve acusar um dia. A função descarta esse par pelo dia civil 21 e retorna zero linhas. | Procede contra L-085/L-097. O corte civil já existia em `medir`. `app/docs/LEIS.md:115,117`. |
| Os testes c2h/c2j deixam escapar as portas mensais e de alimentação | `app/ponto/tests/test_ramo2b_c2h_saida.py:62–77,91–113,123–161`, `app/ponto/motor_calculo_v2.py:3184–3224,3890–3920` | O teste parcial passa base, pausa e lista diretamente a `do_dia`. As outras grades chamam `calcular_periodo`, esvaziam `pont_por_dia` e injetam o previsto. Os dois controles Partido usam vão 60 que, se usado na fábrica com os quatro marcos, escolhe o motor contínuo 12x36. A indenização do Partido mora em `calcular_mes`. | Procede como selo sem mordida para esses caminhos. `CLAUDE.md:28–29,45–49`. |

Para o Partido de fato, 19–07 com vão 00–03, E19:50/S00 e E03/S07:20, a parcial 19:20–19:50 cobre 30 dos 50 minutos de atraso. Pela regra e pelo caminho estático da régua de relógio, previsto 540, trabalho 510, base 490, atraso 20, intra 0, HE 0. O Partido consta alterado no diff acumulado em `calcular_periodo` e `calcular_mes`. A autoria exata por commit [não verificado]. `app/ponto/motor_calculo_v2.py:3103–3118,3164–3167,3200–3224,3295–3302,3347–3381,3416`.

## 27/11 01:28 · sessao 69677b25d33b · - · - · origem=Ronald (tmux)

**REPROVADO.** A releitura v3 usa o intervalo da cauda para reduzir a indenização e combinar esse valor com a base de HE recortada. No 12x36 de 19h–07h, previsto líquido de 660 min, intervalo cadastrado 00h–01h e batidas E19h/S07h20/E07h40/S08h20, a regra dá 780 min trabalhados, 60 de intra e zero HE. Executei as funções da cópia em memória e obtive 780, 40 e 20. O caso usa a pausa entregue diretamente ao período, conforme a fixture. A divergência procede contra a L-031, L-032, L-097 e o corte da pausa na cauda. [periodo.py:745](/home/ronald/codex_ws/7f954d9211ae5382b1aae074c32c1fe4683e0fd6/app/ponto/calculador/periodo.py:745) [periodo.py:834](/home/ronald/codex_ws/7f954d9211ae5382b1aae074c32c1fe4683e0fd6/app/ponto/calculador/periodo.py:834) [PROMPTS.md:3071](/home/ronald/codex_ws/7f954d9211ae5382b1aae074c32c1fe4683e0fd6/app/docs/PROMPTS.md:3071)

- **Parcial na releitura v3.** No caso c2j E19h50/S00h/E01h/S07h20, parcial aprovada 19h20–19h50, previsto 660 e trabalho 630, a regra dá atraso 20. A releitura deixa de alimentar a parcial e limita o atraso bruto de 50 a 30. A omissão procede contra o aval do abono. O módulo declara zero leitores em produção. `app/ponto/calculador/periodo.py:8–20,434–435,474–476,797–825` · `app/docs/PROMPTS.md:3031`.
- **Exportação, dia errado.** No dia final 20/11, jornada 23h50–07h50, intervalo 02h–03h, E00h02/S08h20 de 21/11, previsto 420, base 468, trabalho 498, intra 60, uma HE indevida injetada de 30 min devia produzir um alerta pelo teto zero. Executei a testemunha em memória e obtive zero alertas pela data civil e um pelo dia do turno. Procede contra a L-085. A data civil é herdada. Com intra zero a testemunha retorna vazio ainda que haja 30 HE acima de teto zero. Esse segundo limite pertence ao teste HE×intra e não demonstra falha no contador de pendências. `app/folha/porta_export.py:344–359,548,587–591` · `app/docs/LEIS.md:117`.
- **Split da HE noturna.** No 12x36 16h–04h com 15 min de extra declarada depois, E16h/S05h30, janela ativa e prorrogação noturna desligada, a HE50 de 15 min termina às 04h15. A função real dá 15 HE noturna com a ponta da base e zero com a batida 05h30. Motor passa a batida, oráculo passa a ponta. O motor já usava o fim cru. O diff torna explícita a divergência dos leitores. `app/ponto/motor_calculo_v2.py:437–479,1084–1090,3052–3072` · `app/ponto/calculador/alimentacao.py:106–115` · `app/ponto/calculador/regras.py:845–865` · `app/docs/LEIS.md:83,115`.
- **Tripla.** A porta explícita está em `app/ponto/motor_calculo_v2.py:1927–1943` e recebe gravação em `:1945–1956`, reset em `:1613–1621` e crédito em `:2742–2765`. O construtor gerado aceita `(720, None, None)` em `:207–208,244–246`, verificado em memória. Os três chamadores de produto encontrados fornecem entrada e saída em `:2956,3095,3437`. O reset mantém o sentido de trabalho zero. O crédito aberto grava soma de pares e instante do último com cauda no mesmo campo que no par fechado indica o ponto final desse par. A ocorrência em fluxo real está [nao verificado]. O próprio teste declara ausência de fixture em `app/ponto/tests/test_ramo2b_c2_base_da_he.py:361–366`.
- **Derivação.** O motor guarda a base por par. Folha consulta o valor em `motor_calculo_v2.py:280–284` / `folha/porta_export.py:339–351`. Oráculo recalcula minutos e pausas do dia com o instante, em `ponto/calculador/alimentacao.py:168–178,469–474,545–548` e `ponto/calculador/nucleo.py:134–153`. A releitura v3 calcula de novo em `ponto/calculador/periodo.py:749–756`. Pontualidade recebe trabalho integral em `alimentacao.py:535–544` e chama a autoridade da L-093 em `regras.py:274–285,879–888`. Parcial vem de `credito_parcial.py:15–32,54–70`; limites inclusivos de marcos e direção das batidas em `turnos.py:2653–2711`, fatia semiaberta `[k,k+1)` em `motor_calculo_v2.py:481–505`.
- **Partido.** O diff contra main muda `calcular_periodo` e `calcular_mes` em `app/ponto/motor_calculo_v2.py:3094–3117,3164–3167,3213–3217,3317–3326,3356–3362`. `app/docs/PROMPTS.md:3087` registra «motor do Partido intocado». Os dois controles usam vão de 60 e `calcular_periodo` direto, em `app/ponto/tests/test_ramo2b_c2h_saida.py:64–67,121–131`. Uma quebra na intra mensal em `app/ponto/motor_calculo_v2.py:3187–3227` deixaria os controles verdes. A atribuição histórica de cada hunk a um commit está [nao verificado].
- **Competência exportada.** O helper da folha recebe `esp['resultado'].periodos` e afeta `falhas`; não muda o objeto de exportação diretamente. O motor alterado entra no recálculo por evento, que escreve em competência entregue fora da guarda especial da 09, de acordo com a L-113 e a trilha existente. Não foi verificada regravação em produção. `app/folha/porta_export.py:587–591,713–721` · `app/ponto/services/fechamento.py:36–50,559–590,1161–1215,1822–1848` · `app/docs/LEIS.md:155`.

## 27/11 01:24 · sessao e1354a6c81d9 · PAR-PELO-TURNO · construtor · origem=Code

## Item 1 — Dia por PK e carga

- **RED na base `794ab3332`.** Nos ensaios em memória, `Ran 5 tests`, `FAILED (failures=5)`, `rc 1`. Os PKs 20 e 21 recebiam dia **17/10** em `[17,21]` e **20/10** em `[20,20]`. A folha, o espelho e a fatia entregavam só `[20]` para o par de borda, que exigia `[20, 21]`. `app/ponto/tests/test_par_pelo_turno.py:195,225,636`
- **Cura na origem.** `_marca_intra_duracao` e `parear_turnos` aplicam as 26 h também à absorção de marcas. `_turnos_do_colab_puro` julga a véspera que uma entrada pode ter herdado. Folha, fatia e espelho passam a carregar pelo alcance de `janela_com_pad`. `app/ponto/turnos.py:390,1381,1844,1989,2989` · `app/ponto/services/fechamento.py:262,638,811` · `app/ponto/services/espelho.py:1032`
- **GREEN local.** O par dos PKs 21 e 22 fica em **20/10** nas três janelas do selo. O PK 20 fica no dia julgado para a órfã, **20/10** ou **19/10** conforme o DNA da véspera, com a mesma resposta nas janelas que contêm a marca. O `Motor12x36` recebeu `[20, 21]` e fechou `20/10 23:30 → 22/10 01:30`, **26 h**, no dia **20/10**. `Ran 57 tests`, `OK`, em memória. Reposicionei as marcas de dois testes de fuso nas novas bordas da janela. `app/ponto/tests/test_par_pelo_turno.py:195,225,253` · `app/ponto/tests/test_composicao_de_fatias_e_total.py:306,316`
- **Mutantes.** Os cinco mutantes unitários do item deram `rc 1` por asserção. Nos dois mutantes de carga, a sonda das funções voltou de `[20, 21]` para `[20]` em cada leitor. A integração Django está **[nao verificado]**. A chamada local parou no import com `ModuleNotFoundError: No module named 'django.test'`. **`except` novos: 0**, por comparação AST dos cinco arquivos alterados. `app/ponto/tests/test_par_pelo_turno.py:7,636,808,868,920`

## Item 5 — Laudo `col155/08-11`

Cabe **pela regra da série HJ-026**. O ensaio sintético de `17:29 → 06:31` fecha no dia **08/11**, com C9 de **782 min** contra teto de **732 min**. O selo passou na base e na cópia. O mutante `laudo_hj026_sem_c9` deu `rc 1`, com `[]` onde o selo exigia a C9. Pontas e PKs reais, papéis intermediários, matriz dos 29 casos e sombra por rubrica **[nao verificado]**. `app/docs/BACKLOG.md:745,2027` · `app/ponto/tests/test_par_pelo_turno.py:80,825`

**LEI-AKITA — padrao=Single Source of Truth.** O pareador julga turno e dia, e `janela_com_pad` fornece o alcance das cargas. `app/core/juizes.py:216` · `app/ponto/turnos.py:300,1844,3055`

| Campo ou valor | Quem escreve ou fornece | Quem lê |
|---|---|---|
| `_intra_dur`, `Turno.batidas`, `Turno.data_turno`, dia por PK | A pré-passagem carimba, `parear_turnos` compõe, `_mk` publica e `_dia_das_batidas_puro` projeta. `app/ponto/turnos.py:300,419,1381,3055` | Pós-fecho, consulta individual e espelho. `app/ponto/turnos.py:1109,3036` · `app/ponto/services/espelho.py:465` |
| Limites e lista de batidas apuráveis | `janela_com_pad` fornece as datas e `batidas_apuraveis_na_janela` consulta pelo universo de apuração. `app/ponto/turnos.py:1844,2959,2989` | Casca, lote, folha, fatia e espelho. `app/ponto/turnos.py:1882,3094` · `app/ponto/services/fechamento.py:262,638,811` · `app/ponto/services/espelho.py:1032` |
| Células e feriados da borda | Cargas de folha e espelho usam o alcance do pareador. `app/ponto/services/fechamento.py:609,619` · `app/ponto/services/espelho.py:1036,1056` | Motor da folha e do espelho. `app/ponto/services/fechamento.py:744,752` · `app/ponto/services/espelho.py:1121,1139` |

### Comandos para o Code

A partir de `app/`. O modo `base_c1v5a` monta as fontes da base em memória. A suite com `--dir` usa a pista remota, cuja execução fica **[nao verificado]**. `app/ponto/tests/test_par_pelo_turno.py:946,980` · `bin/suite.sh:319,326`

```bash
export PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci

python -m ponto.tests.test_par_pelo_turno base_c1v5a

for mutante in \
  pausa_inferida_fora_do_envelope miolo_carimbado_fora_do_envelope \
  contexto_sem_vespera_da_entrada carga_so_uma_virada \
  fatia_so_uma_virada folha_so_uma_virada espelho_so_uma_virada \
  laudo_hj026_sem_c9
do
  python -m ponto.tests.test_par_pelo_turno "$mutante"
done

../bin/suite.sh --dir /home/ronald/wt-codex-par-pelo-turno-010806 \
  --only "ponto.tests.test_par_pelo_turno ponto.tests.test_composicao_de_fatias_e_total ponto.tests.test_janela_da_autoridade_e_a_da_folha ponto.tests.test_dupla_contagem_dia20 ponto.tests.test_col129_par_separado_pelo_teto ponto.tests.test_col129_teto_vira_lista ponto.tests.test_contract_no_batida_date ponto.tests.test_contract_folha_le_a_autoridade ponto.tests.test_tripwire_b5j_universo_de_apuracao ponto.tests.test_fechamento_escritor_guardas"
```

## 27/11 01:19 · sessao e5b196ca57e5 · RAMO2-B-EM-TURNO-QUE-CRUZA · construtor · origem=Code

Ajustei os três registros da catraca em `core/tests/test_medida_turno_do_par.py`. **GREEN local: 35 de 35 testes em memória**, usando `unittest.TestCase` no lugar de `django.test.SimpleTestCase`. A suíte do Code e o Opus final estão [nao verificado]. `core/tests/test_medida_turno_do_par.py:28`, `:194`, `:251`, `:425`

As três divergências vieram da obra no merge `3b594ce1f`. No main `59600163c`, os três registros coincidiam com o censo da própria revisão. `core/medida_turno_do_par.py:336`, `:448`

| Falha | Causa | Cura | GREEN local |
|---|---|---|---|
| `test_CATRACA_linhas_reais_congeladas_exigem_registro_no_mesmo_commit` | O merge deslocou ocorrências do motor, incluindo `909 → 937` e `2521 → 2601`, e moveu a ocorrência da folha de `medir:544` no main para `he_acima_do_teto_por_dia:344`. `ponto/motor_calculo_v2.py:937`, `:2601`; `folha/porta_export.py:344` | Atualizei as linhas medidas nas duas listas e registrei a obra em um comentário de uma linha. `core/tests/test_medida_turno_do_par.py:251`, `:288`, `:304`, `:348` | Passou. `core/tests/test_medida_turno_do_par.py:565` |
| `test_CATRACA_numeros_exatos_e_registro_no_mesmo_commit` | A chamada `_p.entrada.date()` passou de `medir` para `he_acima_do_teto_por_dia`, trocando a identidade B1. `folha/porta_export.py:344`, `:587`; `core/medida_turno_do_par.py:217` | Registrei a troca de identidade. Os contadores conferem em **9 pareadores e 81 derivações de dia**. `core/tests/test_medida_turno_do_par.py:108`, `:194` | Passou. `core/tests/test_medida_turno_do_par.py:558` |
| `test_a_classificar_publicado_sem_entrar_como_criterio_amplo` | Em `medir`, a data do ato ainda é medida como B2. Com a saída da ocorrência B1, `medir` aparece em `B2_menos_B1`. `folha/porta_export.py:391`; `core/medida_turno_do_par.py:223`, `:432` | Registrei `medir` em `CLASSIFICAR['B2_menos_B1']`. `core/tests/test_medida_turno_do_par.py:425` | Passou. `core/tests/test_medida_turno_do_par.py:571` |

**`except` novos: 0.** A comparação AST encontrou o mesmo `except` antes e depois. O diff ficou restrito aos três registros e ao comentário no arquivo de teste. `core/tests/test_medida_turno_do_par.py:194`, `:251`, `:425`, `:668`

### RED e mutantes

A partir de `app/`, a função abaixo restaura em memória os valores da base `03bc04e5b`. O RED reproduz as três falhas. Cada mutante restaura um único registro e derruba somente a asserção correspondente. `core/tests/test_medida_turno_do_par.py:558`, `:565`, `:571`

```bash
catraca() {
  GIT_OPTIONAL_LOCKS=0 PYTHONDONTWRITEBYTECODE=1 python3 -B - "$1" <<'PY'
import ast
import pathlib
import subprocess
import sys
import types
import unittest

path = pathlib.Path('core/tests/test_medida_turno_do_par.py')
suite = types.ModuleType('suite_turno') [nao verificado]
suite.__file__ = str(path.resolve()) [nao verificado]
source = path.read_text().replace('from django.test import SimpleTestCase',
                                  'from unittest import TestCase as SimpleTestCase')
exec(compile(source, str(path), 'exec'), suite.__dict__) [nao verificado]
mode = sys.argv[1]
records = ('LINHAS_REGISTRADAS', 'REGISTRO', 'CLASSIFICAR')
assert mode in ('green', 'red', *records)
if mode != 'green':
    baseline = subprocess.check_output(
        ['git', 'show', f'03bc04e5b:app/{path}'], text=True)
    selected = records if mode == 'red' else (mode,)
    for node in ast.parse(baseline).body:
        if isinstance(node, ast.Assign) and isinstance(node.targets[0], ast.Name):
            name = node.targets[0].id
            if name in selected:
                setattr(suite, name, ast.literal_eval(node.value)) [nao verificado]
result = unittest.TextTestRunner(verbosity=1).run(
    unittest.defaultTestLoader.loadTestsFromModule(suite)) [nao verificado]
raise SystemExit(not result.wasSuccessful())
PY
}
```

| Comando | Resultado local |
|---|---|
| `catraca green` | rc 0, 35 testes passaram. `core/tests/test_medida_turno_do_par.py:503` |
| `catraca red` | rc 1, três falhas de asserção. `core/tests/test_medida_turno_do_par.py:558`, `:565`, `:571` |
| `catraca LINHAS_REGISTRADAS` | rc 1, somente a asserção de linhas. `core/tests/test_medida_turno_do_par.py:565` |
| `catraca REGISTRO` | rc 1, somente a asserção de identidade. `core/tests/test_medida_turno_do_par.py:558` |
| `catraca CLASSIFICAR` | rc 1, somente a asserção de publicação. `core/tests/test_medida_turno_do_par.py:571` |

### Recorte do Code

Comando do anexo c2j, a partir da raiz da cópia. Execução [nao verificado].

```bash
PYTHONDONTWRITEBYTECODE=1 bin/suite.sh \
  --dir /home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946 \
  --only "ponto.tests.test_ramo2b_c2_base_da_he ponto.tests.test_ramo2b_c2h_saida ponto.tests.test_ramo2b_turno_que_cruza ponto.tests.test_contract_juiz_batida ponto.tests.test_janela_he_saida ponto.tests.test_o146_extra_declarada_da_escala ponto.tests.test_o280_bateria_da_lei core.tests.test_medida_turno_do_par folha.tests.test_porta_do_export"
```

**LEI-AKITA** `padrao=juiz unico` · `juizes novos=0` · `docs/LEIS.md:60` · `core/tests/test_medida_turno_do_par.py:251`

## 27/11 01:06 · sessao 42b6cdad486f · RAMO2-B-EM-TURNO-QUE-CRUZA · construtor · origem=Code

Na base `3b594ce1f`, `do_dia` já devolve o número esperado para parcial com cauda pendente. O caso mede **610 min de base da HE, 630 min trabalhados, 20 min de atraso e zero de saída antecipada, intra indenizada e HE em dia comum**. Com feriado em dobra, são **610 min de HE100**. O cálculo de produção ficou como veio no merge. [Regra escrita](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/docs/LEIS.md:79) · [L-097 emendada](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/docs/LEIS.md:115) · [Teste](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2h_saida.py:133)

`rubricas_por_dia` carrega as parciais pagas com período declarado e passa a lista do dia a `do_dia`. A base e a HE são apuradas primeiro. O abatimento é posterior, em `pontualidade_do_dia`, pelo teto emprestado do motor. A parcial cobre 30 dos 50 min de atraso bruto. Os 20 restantes passam pelo teto da L-093, de `660 − 630 = 30 min`. [Chamador](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/calculador/alimentacao.py:535) · [Base e dobra](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/calculador/regras.py:797) · [Leitor de `parciais`](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/calculador/regras.py:879) · [Abatimento](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/motor_calculo_v2.py:1270) · [Teto](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/motor_calculo_v2.py:1304)

| Item | RED | GREEN local |
|---|---|---|
| 1. Catraca | Os dois registros `[530]` divergiam do AST `[558]`. | Os dois registros pedidos conferem. Na catraca integral, **3 de 35 testes falham**. Restam 2 divergências em `pareadores` e 13 em `derivacoes_de_dia`, incluindo a troca de identidade de `medir` para `he_acima_do_teto_por_dia`. A troca também afeta `CLASSIFICAR`. [Registros](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/core/tests/test_medida_turno_do_par.py:251) · [Asserções](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/core/tests/test_medida_turno_do_par.py:547) · [Exportação](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/folha/porta_export.py:344) | [nao verificado]
| 2. Parcial com cauda | `ignora_parciais` dá **30 min de atraso** onde a regra pede **20**. | Os três cenários passam na base do merge. [Teste](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2h_saida.py:133) · [Mutação](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/ramo2b_c2h_mutantes.py:12) | [nao verificado]
| 3. Bateria de saída | Com a alimentação de `deed2fe72`, 8 subcasos falham por asserção. No cruzamento, o gozo é **0** em vez de **20 min**. | Oito casos em cada motor, 12x36 e Comercial, mais os dois controles do Partido previstos em `RAMO2-PARTIDO-2611-1812 (a)`. Com os três cenários do item 2, são **21 subcasos verdes em memória**. [Grade](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2h_saida.py:32) · [Execução e controles](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2h_saida.py:115) |
| 4. Vizinhos | As três falhas da catraca persistem após o ajuste restrito aos dois registros. [Asserções](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/core/tests/test_medida_turno_do_par.py:547) | Os selos do escritor e do contrato da batida passaram em memória, **22 testes**. Suíte Django do Code e Opus final [nao verificado]. [Escritor](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2_base_da_he.py:622) · [Contrato](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_contract_juiz_batida.py:159) | [nao verificado]

### Comandos

Medição da catraca a partir de `app/`. Resultado `pareadores [558]` e `derivacoes_de_dia [558]`. [Censo](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/core/medida_turno_do_par.py:448) · [Chamada medida](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/motor_calculo_v2.py:558)

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B - <<'PY'
from core.medida_turno_do_par import censo

medida = censo('.')
for grupo in ('pareadores', 'derivacoes_de_dia'):
    print(grupo, next(i['linhas'] for i in medida[grupo]['itens']
                      if i['chave'] == 'ponto/motor_calculo_v2.py::turnos_via_autoridade'
                      and i.get('sitio') == 'porta_parear_turnos'))
PY
```

Para a sessão rodar o recorte Django a partir da raiz da cópia. [Rota de suite da cópia](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/bin/suite.sh:26) [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 bin/suite.sh \
  --dir /home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946 \
  --only "ponto.tests.test_ramo2b_c2_base_da_he ponto.tests.test_ramo2b_c2h_saida ponto.tests.test_ramo2b_turno_que_cruza ponto.tests.test_contract_juiz_batida ponto.tests.test_janela_he_saida ponto.tests.test_o146_extra_declarada_da_escala ponto.tests.test_o280_bateria_da_lei core.tests.test_medida_turno_do_par folha.tests.test_porta_do_export"
```

Para reproduzir os mutantes a partir de `app/` no ambiente Django da sessão, defina a função abaixo. As saídas da tabela foram medidas pelo mesmo executor em `unittest`, com dublês de infraestrutura em memória e zero erros de execução. [Executor](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/ramo2b_c2h_mutantes.py:82)

```bash
ramo2() { PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.ramo2b_c2h_mutantes "$1"; }
```

| Comando | Saída local |
|---|---|
| `ramo2 green` | rc 0, 3 testes, 21 subcasos | [nao verificado]
| `ramo2 ignora_parciais` | rc 1, 2 falhas, atraso 30 em vez de 20 |
| `ramo2 teto_le_base_da_he` | rc 1, 1 falha, atraso do controle 50 em vez de 30 |
| `ramo2 abono_compoe_base_da_he` | rc 1, 1 falha, HE100 de 640 em vez de 610 |
| `ramo2 dobra_inclui_cauda` | rc 1, 1 falha, HE100 de 630 em vez de 610 |
| `ramo2 red_deed2fe72` | rc 1, 8 falhas, gozo 0 em vez de 20 no cruzamento |
| `ramo2 sem_ponta` | rc 1, 8 falhas, gozo 0 em vez de 20 no cruzamento |
| `ramo2 copy_setattr` | rc 1, 8 falhas, base 1 em vez de 700 no cruzamento |
| `ramo2 teto_max` | rc 1, 16 falhas, trabalhado 800 em vez de 760 no cruzamento |
| `ramo2 inclui_cauda` | rc 1, 5 falhas, base 760 em vez de 680 na pausa antes do marco |
| `ramo2 perde_fim_no_marco` | rc 1, 2 falhas, gozo 0 em vez de 20 na pausa que termina no marco |

### Hand-back

A contagem AST deu **0 `except` novos** nos três arquivos alterados. O diff do motor contra `138b516f2` tem **97 inserções em três blocos** e é idêntico ao diff do merge `3b594ce1f` contra a mesma referência. [Catraca](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/core/tests/test_medida_turno_do_par.py:251) · [Bateria](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2h_saida.py:123) · [Mutantes](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/ramo2b_c2h_mutantes.py:12) · [Motor](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/motor_calculo_v2.py:481)

```bash
GIT_OPTIONAL_LOCKS=0 git diff 138b516f2 -- ponto/motor_calculo_v2.py
GIT_OPTIONAL_LOCKS=0 git diff --exit-code 3b594ce1f -- ponto/motor_calculo_v2.py
```

Corpo proposto para o commit. [nao verificado]

```text
RAMO2-B c2j: cobre parcial com cauda e registra linha da catraca

OBRA: RAMO2-B-EM-TURNO-QUE-CRUZA
BASE: 3b594ce1f4bd2bfa4a2049c149ecb254643ff526

Registra porta_parear_turnos em 558 nos dois contadores.
Fixa pela regra 610 min de base, 630 trabalhados e 20 de atraso
com parcial. A grade cobre 12x36 e Comercial, com os dois controles
do Partido pelo aval RAMO2-PARTIDO-2611-1812 (a).

LEI-AKITA:
origem=docs/LEIS.md:79-80,111,115; docs/PROMPTS.md:3031,3071,3087
escrita=ponto/motor_calculo_v2.py:1926-1963
testemunha=ponto/calculador/nucleo.py:146-153, segmentos e batidas
RED=ponto/tests/test_ramo2b_c2h_saida.py:133, mutante ignora_parciais
RED da grade=ponto/tests/test_ramo2b_c2h_saida.py:115, alimentacao de deed2fe72
quem-mais-le=ponto/calculador/alimentacao.py:106-117,168-178,469-475,535-566
  (rubricas_por_dia -> ponto/calculador/regras.py:614-626,775-807,879-888);
  ponto/services/fechamento.py:1291-1300;
  ponto/management/commands/diff_calculador.py:414-424;
  ponto/calculador/periodo.py:745-759; folha/porta_export.py:339-358
padrao=juiz unico
juizes novos=0

fora do marco: PAUSA-NA-BORDA-DO-PAR, PAUSA-INTERNA-NO-PARTIDO

Catraca integral: tres falhas medidas em memoria fora dos dois
registros ajustados (linhas, identidade e CLASSIFICAR).
```

## 27/11 00:12 · sessao b7c6fd3c7578 · V6-FURO-OUTPUT · leitura · origem=Code

# V6-FURO-OUTPUT

HEAD conferido em `59600163ccc9cffb8f3ae148beede7cd7866ae86`. A leitura Bash do `.output` registrado no RELATO hoje retorna zero alvos. Confirmei em memória que três chamadas `cat` desse mesmo caminho deixam o contador vazio. [.git/refs/heads/main:1](/home/ronald/saas-hasner/.git/refs/heads/main:1) · [app/docs/RELATO.md:3041](/home/ronald/saas-hasner/app/docs/RELATO.md:3041) · [bin/disjuntor.py:2142](/home/ronald/saas-hasner/bin/disjuntor.py:2142)

## 1. Mapa do reconhecimento

| Entrada | O que conta hoje |
|---|---|
| `tail`, `cat`, `grep`, `ls` | Argumentos terminados em `.rc` ou `.log`, em qualquer diretório. Para `ls`, também conta um argumento com `logs` como componente do caminho. A análise usa o basename do comando, descarta opções e pula os redirecionamentos que reconhece. [bin/disjuntor.py:1985](/home/ronald/saas-hasner/bin/disjuntor.py:1985) · [bin/disjuntor.py:2131](/home/ronald/saas-hasner/bin/disjuntor.py:2131) · [bin/disjuntor.py:2142](/home/ronald/saas-hasner/bin/disjuntor.py:2142) |
| `head`, `wc` | Ficam fora da lista de comandos de consulta. [bin/disjuntor.py:1985](/home/ronald/saas-hasner/bin/disjuntor.py:1985) · [bin/disjuntor.py:2142](/home/ronald/saas-hasner/bin/disjuntor.py:2142) |
| `vercod`, `vercode`, `vercodex`, `python* …/vercodex.py` | Geram a chave textual `vercodex ` seguida dos argumentos, excluído `vercodex.py`. [bin/disjuntor.py:1986](/home/ronald/saas-hasner/bin/disjuntor.py:1986) · [bin/disjuntor.py:2139](/home/ronald/saas-hasner/bin/disjuntor.py:2139) |
| `ssh pista estado …` | Gera a chave textual `ssh pista estado ` seguida dos argumentos após `estado`. `ssh pista espera` e os laços de espera reconhecidos retornam lista vazia. [bin/disjuntor.py:2120](/home/ronald/saas-hasner/bin/disjuntor.py:2120) · [bin/disjuntor.py:2132](/home/ronald/saas-hasner/bin/disjuntor.py:2132) |

O **PreToolUse recebe `Read`** pelo `matcher` vazio, e a guarda geral lê `tool_input.file_path`. O **polling** roda só para `Bash` sem `agent_id`, usando `tool_input.command`. Uma mudança apenas em `_alvos_de_consulta` alcança esse ramo Bash. [.claude/settings.json:62](/home/ronald/saas-hasner/.claude/settings.json:62) · [bin/disjuntor.py:222](/home/ronald/saas-hasner/bin/disjuntor.py:222) · [bin/disjuntor.py:2217](/home/ronald/saas-hasner/bin/disjuntor.py:2217)

## 2. Chamadores, memória e identidade

- **Chamada única em produção:** `main_pretool` → `pretool` → `_pretool` → `espera_nega(cmd) or polling_nega(...)` → `_alvos_de_consulta(cmd, cwd)`. Uma negativa de `espera_nega` encerra a avaliação antes do polling. [bin/disjuntor.py:2303](/home/ronald/saas-hasner/bin/disjuntor.py:2303) · [bin/disjuntor.py:2207](/home/ronald/saas-hasner/bin/disjuntor.py:2207) · [bin/disjuntor.py:2225](/home/ronald/saas-hasner/bin/disjuntor.py:2225) · [bin/disjuntor.py:2177](/home/ronald/saas-hasner/bin/disjuntor.py:2177)
- **Memória:** `logs/sessao/disjuntor_estado.json`, sob `HOOK_STOP_RAIZ` ou a raiz derivada do script. `_com_estado` usa `flock` e grava por `os.replace`. `poll` guarda listas de timestamps por `sessão + "\t" + alvo`. A sessão vem de `session_id`, depois `transcript_path`, depois `"-"`. Com duas consultas aceitas há **menos de 600 s**, a terceira é recusada. A tentativa recusada não acrescenta timestamp. O teto da espera é **300 s**. [bin/disjuntor.py:122](/home/ronald/saas-hasner/bin/disjuntor.py:122) · [bin/disjuntor.py:340](/home/ronald/saas-hasner/bin/disjuntor.py:340) · [bin/disjuntor.py:374](/home/ronald/saas-hasner/bin/disjuntor.py:374) · [bin/disjuntor.py:1953](/home/ronald/saas-hasner/bin/disjuntor.py:1953) · [bin/disjuntor.py:1962](/home/ronald/saas-hasner/bin/disjuntor.py:1962) · [bin/disjuntor.py:2183](/home/ronald/saas-hasner/bin/disjuntor.py:2183)
- **Caminho:** usa `normpath` sobre o caminho absoluto ou sobre `join(cwd, relativo)`. Sem `cwd`, o relativo continua relativo. A função usa o `cwd` do payload, sem aplicar `cd` do comando nem resolver symlinks. Cada alvo normalizado conta uma vez por chamada. [bin/disjuntor.py:2116](/home/ronald/saas-hasner/bin/disjuntor.py:2116) · [bin/disjuntor.py:2151](/home/ronald/saas-hasner/bin/disjuntor.py:2151) · [bin/disjuntor.py:2180](/home/ronald/saas-hasner/bin/disjuntor.py:2180)

## 3. Forma do `.output`

O recorte para o aval é casar **o caminho normalizado inteiro** com `/tmp/claude-[0-9]+/[^/]+/[^/]+/tasks/[^/]+\.output` usando `fullmatch`. Isso exige UID decimal, um componente de projeto, um de sessão e o arquivo diretamente em `tasks/`. A árvore documenta um caminho real nessa forma e usa `/tmp/claude-%d` para o diretório do Code. `esteira.out`, `wrapper.out` e um `*.output` comum do repo ficam fora desse recorte. [bin/disjuntor.py:2153](/home/ronald/saas-hasner/bin/disjuntor.py:2153) · [app/docs/RELATO.md:3041](/home/ronald/saas-hasner/app/docs/RELATO.md:3041) · [bin/retencao_disco.py:638](/home/ronald/saas-hasner/bin/retencao_disco.py:638) · [fatias_agendadas/o135-teto20/esteira.sh:26](/home/ronald/saas-hasner/fatias_agendadas/o135-teto20/esteira.sh:26) · [bin/tests/prova_codex_pergunta_real.sh:85](/home/ronald/saas-hasner/bin/tests/prova_codex_pergunta_real.sh:85)

## 4. Onde nasce o RED

`bin/tests/test_disjuntor.sh` tem o helper `_pre`, mas nenhum caso de `polling_nega`. O vizinho do RED é **`polling()` em `bin/tests/test_agente_no_modelo.sh:213`**. Ele cobre primeira e segunda aceitas, terceira negada, relativo e absoluto como o mesmo alvo, bordas de 599 e 600 s, alvos e sessões distintos, `ssh pista estado`, `vercodex`, `ls logs/`, subagente e espera única. `divergencia()` também verifica o limiar e a janela, e o mutante `polling-desligado` exercita a ligação ao hook. [bin/tests/test_disjuntor.sh:103](/home/ronald/saas-hasner/bin/tests/test_disjuntor.sh:103) · [bin/tests/test_disjuntor.sh:1621](/home/ronald/saas-hasner/bin/tests/test_disjuntor.sh:1621) · [bin/tests/test_agente_no_modelo.sh:213](/home/ronald/saas-hasner/bin/tests/test_agente_no_modelo.sh:213) · [bin/tests/test_agente_no_modelo.sh:297](/home/ronald/saas-hasner/bin/tests/test_agente_no_modelo.sh:297) · [bin/tests/test_agente_no_modelo.sh:345](/home/ronald/saas-hasner/bin/tests/test_agente_no_modelo.sh:345)

## 5. Espera única

**A inclusão desse alvo no reconhecedor não recusa a espera única.** `ESPERA_UNICA` chama `rc_sem_colher.py`, que lê o RELATO e o conteúdo dos `.rc` selecionados, e usa `find` sobre `*.rc`. Seu laço já retorna zero alvos no extrator. Conferi também `logs/espera_unica.sh` em disco, fora do HEAD e ignorado pelo Git. Ele chama o mesmo juiz e usa `head` apenas nos arquivos `*.rc` devolvidos pelo `find`. Nenhuma das duas formas abre arquivo de tarefa `.output`. [bin/disjuntor.py:1967](/home/ronald/saas-hasner/bin/disjuntor.py:1967) · [bin/disjuntor.py:2120](/home/ronald/saas-hasner/bin/disjuntor.py:2120) · [bin/rc_sem_colher.py:71](/home/ronald/saas-hasner/bin/rc_sem_colher.py:71) · [bin/rc_sem_colher.py:101](/home/ronald/saas-hasner/bin/rc_sem_colher.py:101) · [.gitignore:53](/home/ronald/saas-hasner/.gitignore:53) · [logs/espera_unica.sh:5](/home/ronald/saas-hasner/logs/espera_unica.sh:5) · [logs/espera_unica.sh:9](/home/ronald/saas-hasner/logs/espera_unica.sh:9)

## 27/11 00:08 · sessao 99ce13081ea2 · CODEX-CONSTROI-MOTOR · verificador · origem=Code

**APROVADO na revisão de código da volta 10.** O achado 1 foi curado na origem. O descarte do achado 4 procede para a admissão do construtor Opus prevista em RAIAS item 3. `app/core/tests/test_model_sem_migration_pendente.py:65–70` · `app/tenants/test_model_sem_migration_pendente.py:25–39` · `bin/deploy.sh:194–196` · `CLAUDE.md:325–330`

### 1. Mordida dos REDs

O RED de campo, usado para `folha` e `tenants`, e o RED de model novo em `api` agora invocam `pergunta_sem_migration_pendente`, a mesma função do deploy. Ambos exigem `SystemExit(1)` e a operação específica na saída. `app/core/tests/test_model_sem_migration_pendente.py:56–80` · `app/tenants/test_model_sem_migration_pendente.py:25–42` · `bin/deploy.sh:194–196`

| Mutante na pergunta | REDs de campo (`folha` e `tenants`) | RED de `api` |
|---|---|---|
| Retirar só `api` das labels enviadas ao Django | Passam. Cada um cobra uma alteração no próprio app. | Reprova. O Django deixa de receber a label necessária para detectar a migration inicial de `CercaMordeApi`. `app/core/tests/test_model_sem_migration_pendente.py:24–31,40–48,79–80` · `app/tenants/test_model_sem_migration_pendente.py:25–42` |
| Retirar `--check`, mantendo `--dry-run` | Reprovam por ausência de `SystemExit`. | Reprova pela mesma asserção. `app/core/tests/test_model_sem_migration_pendente.py:48,65–70` · `app/tenants/test_model_sem_migration_pendente.py:33–39` |
| Engolir o `SystemExit` do Django | Reprovam por ausência de `SystemExit`. | Reprova pela mesma asserção. `app/core/tests/test_model_sem_migration_pendente.py:48,65–70` · `app/tenants/test_model_sem_migration_pendente.py:33–39` |

A execução em memória com dublê do Django deu esses resultados. Execução com Django real nesta cópia [nao verificado].

### 2. Achado 4 da volta 9

**Descarte procedente para a admissão.** A redação da c10 exige `wt-*` e que `ARQUIVOS:` toque pelo menos um caminho de `migrations`, `core/juizes.py`, `crons.py` ou `bin/`. A lista vigente já exclui motor e `models.py`. O `any(...)` implementa essa condição, e o selo espera admissão de `views.py` junto com um caminho da lista. `CLAUDE.md:325–330` · `bin/disjuntor.py:1961,2102–2113` · `bin/tests/test_agente_no_modelo.sh:127–143`

O escopo de escrita continua regido por CODEX item 2 e L-126. Motor e `models.py` sem migration cabem ao Codex construtor, com Opus na verificação. Escrita efetiva pelo Opus fora desse escopo [nao verificado]. `CLAUDE.md:402–406` · `app/docs/LEIS.md:147` · `aval.txt:1`

### 3. Quem escreve e quem lê

| Sítio | Escritor e leitores |
|---|---|
| **Pergunta** | `labels_locais()` produz a lista a partir do registro Django. A pergunta a entrega a `call_command`, que produz o resultado do check. Leem a pergunta `checa_sem_migration_pendente`, os REDs de campo e de `api`, e o deploy nas chamadas de decisão e diagnóstico. O Bash grava `DRIFT` pelo status e o lê na guarda. `app/core/tests/test_model_sem_migration_pendente.py:24–53,65–66` · `app/tenants/test_model_sem_migration_pendente.py:33–34` · `bin/deploy.sh:194–206` |
| **RED de campo** | `morde_campo_sem_migration` altera e restaura `max_length` em memória. O Django lê o model alterado pela pergunta comum. O teste lê o código de saída e a operação `Alter field`. Os casos são `RubricaDominio.evento` e `Cliente.nome`. `app/core/tests/test_model_sem_migration_pendente.py:56–80` · `app/tenants/test_model_sem_migration_pendente.py:41–42` |
| **RED de `api`** | O teste registra `CercaMordeApi` e o remove em `finally`, limpando o cache. O Django lê o model registrado pela pergunta comum. O teste lê o código de saída e `Create model CercaMordeApi`. `app/tenants/test_model_sem_migration_pendente.py:25–39` |

Na cópia, a única chamada Python a `call_command('makemigrations', …, '--check')` está dentro da própria pergunta. O ramo `*makemigrations*--check*` é um padrão do Docker falso. `app/core/tests/test_model_sem_migration_pendente.py:48` · `bin/tests/test_deploy_portoes_antes_do_migrate.sh:85–103`

### 4. Classes do delta

**Selo sem mordida:** curado para os três mutantes pedidos pelo conjunto dos REDs Python. O selo Bash simula o status pelo texto do comando. **Derivação paralela:** as duas invocações avulsas foram substituídas pela pergunta comum. **`except` novo e campo com dois significados:** o delta troca as chamadas e o import, preservando os `finally` e o significado dos valores testados. `delta.patch:1–34` · `app/core/tests/test_model_sem_migration_pendente.py:34–70` · `app/tenants/test_model_sem_migration_pendente.py:25–42` · `bin/tests/test_deploy_portoes_antes_do_migrate.sh:90–97`

Selos de host no hash, Opus final e prova das 03:30 são evidências da sessão [nao verificado]. `aval.txt:2`

## 27/11 00:02 · sessao cd30e8986317 · CODEX-CONSTROI-MOTOR · verificador · origem=Code

**REPROVADO — volta 9, cópia `148372677`.** A c9 fecha o falso verde causado pela lista parcial no Bash e corrige o destino da obra de motor na L-126. O RED obrigatório de `api` ainda contorna a função usada pelo deploy. Selos verdes no hash, Opus final aprovado e execução real das 03:30 registrada no RELATO seguem [nao verificado]. `bin/deploy.sh:194–205` · `app/tenants/test_model_sem_migration_pendente.py:25–40` · `app/docs/LEIS.md:147` · `aval.txt:2`

## Achados da volta 8 e do Opus

| Achado | Situação na c9 e contraexemplo |
|---|---|
| **1. Schema sem label e falso verde por stdout parcial** | **Curado na origem do Bash.** A c9 passa a lista diretamente do registro Django ao `makemigrations`, no mesmo processo. O cenário da v8 em que sai `core` antes de um timeout agora recusa pelo status do comando. **Procedente no RED:** se a função nova omitir `api` ou perder `--check`, os testes que injetam drift continuam chamando `call_command` diretamente e o Docker falso continua respondendo pelo nome da função. `bin/deploy.sh:194–205` · `app/core/tests/test_model_sem_migration_pendente.py:40–48,56–70` · `app/tenants/test_model_sem_migration_pendente.py:25–40` · `bin/tests/test_deploy_portoes_antes_do_migrate.sh:90–97` |
| **2. Selos de host verdes no hash** | **Procedente, prova em aberto.** Os casos e mutantes existem no código. Um verde anterior à c9 ainda deixa a revisão atual sem prova. RED/GREEN integral do deploy, wrapper e agente no hash pedido [nao verificado]. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:244–262` · `bin/tests/test_codex_pergunta.sh:658` · `bin/tests/test_agente_no_modelo.sh:127–143,286–294` · `aval.txt:2` |
| **3. Portão na casca real** | **Procedente, prova em aberto.** O cron das 03:30 e o reload da cópia, inclusive ao desfazer, chegam ao portão quando as guardas anteriores deixam passar. Os casos 18 e 20 simulam migration existente pendente. Um deploy normal verde no falso ainda deixaria sem prova os settings efetivos e a execução das 03:30. Linha da prova no RELATO [nao verificado]. `app/config/crons.py:326–327` · `bin/molde_fatia/cadeia.sh:25–35,46–49` · `bin/deploy.sh:126–205` · `bin/tests/test_deploy_portoes_antes_do_migrate.sh:216–226` · `aval.txt:2` |
| **4. Duplo destino após duas reprovações** | **Curado na L-126.** Motor ou `models.py` fica com o construtor Codex e ganha postmortem. O repasse ao Opus vale só para o trabalho vedado ao Codex. **Procedente na admissão de escopo misto:** o disjuntor admite o brief se **qualquer** caminho casar com uma das quatro tags. Um brief com motor e migration pode passar nessa regra. O selo já espera admissão com `views.py` mais um caminho vedado. Edição efetiva pelo Opus fora do seu escopo [nao verificado]. `app/docs/LEIS.md:147` · `bin/disjuntor.py:1961,2023–2039,2102–2113` · `bin/tests/test_agente_no_modelo.sh:127–143` |
| **5. Pacote `models/` liberado** | **Curado na origem do inventário.** A regex barra o pacote e seus descendentes. O inventário também acrescenta `/` ao caminho de uma pasta nova vazia. O selo cria `folha/models/__init__.py` e tem mutante que retira a guarda. Um model declarado em `schema.py` e importado por `models.py` fica fora da regra de caminho e entra na cerca de schema se for registrado sob uma label elegível. `bin/codex_pergunta.sh:162–175,202–224` · `bin/tests/test_codex_pergunta.sh:153–162,409–412,658` · `app/core/tests/test_model_sem_migration_pendente.py:24–48` |

## `DRIFT=0` e diagnóstico

**Pelos caminhos de falha perguntados, o portão fecha.** A única atribuição de `DRIFT=0` fica no ramo bem-sucedido de `timeout -k 10 180 docker exec … shell -c`. A decisão descarta stdout e stderr e roda sem pipeline. Ausência de `core` levanta `SystemExit(1)`. Falha de import, falha do Docker ou timeout que volte com status não zero cai em `DRIFT=1`. O Bash confia integralmente no status externo. Se o processo acabar prematuramente com status zero, por exemplo por um `SystemExit(0)` antes da chamada, ou se um executor falso devolver zero, o Bash aceita. Um caminho desse tipo nos imports atuais da casca [nao verificado]. `app/manage.py:8–18` · `app/core/tests/test_model_sem_migration_pendente.py:34–48` · `bin/deploy.sh:194–201` · `bin/tests/test_deploy_portoes_antes_do_migrate.sh:90–103`

O script declara `set -u`. Num modo de publicação com `DRIFT=1`, a segunda chamada executa com `verbosity=1` e `stdout=sys.stdout`. Ela imprime `labels: …` e a saída do Django, como as operações propostas, ou `labels da casa sem core: …`. Erros do import e do Docker podem aparecer em stderr. O Bash junta os fluxos, retém as últimas 20 linhas e indenta. Essa segunda resposta fica só no diagnóstico, mesmo se voltar verde. Com `errexit` e `pipefail` juntos, o pipeline pode encerrar o script antes da mensagem de `falhou`, ainda com recusa. `--conferir` imprime `DRIFT=1`, pula a reconsulta e pode sair com rc 0 ao chegar ao fim do modo. `bin/deploy.sh:52,92,194–206,260–262` · `app/core/tests/test_model_sem_migration_pendente.py:40–48` · `app/tenants/test_model_sem_migration_pendente.py:34–40`

**Quem lê.** O cron encaminha a saída para `logs/reload_diario.log` e seu envelope registra o rc. A cadeia captura a saída em `reload_copia.out` e `reload_desfeito.out`, mostra o fim do arquivo e testa o rc na ida. O integrador captura ambos os fluxos. Ele descarta o rc do reload preliminar e testa o do deploy final antes de marcar a fatia no ar. No selo, 25 e 27 leem a mensagem de recusa em `SAIDA`. `app/config/crons.py:20,326–327,1900–1907` · `bin/cron_run.sh:11–13` · `bin/molde_fatia/cadeia.sh:32–33,48–49` · `app/core/integrador_lote.py:242–248,638–639,753–765` · `bin/tests/test_deploy_portoes_antes_do_migrate.sh:123–126,245–255`

## Universo de labels

- **`ci`** herda os 12 apps da casa de `base`, sem `api` e `tenants`. A régua seleciona testes de `api`, mas isso não instala o app no registro. **`teste_motor`** herda `saas`, instala os 14 apps da casa e é usado pelo pre-push para `tenants`. **`saas`** reúne `SHARED_APPS` e `TENANT_APPS`, com `tenants` no primeiro e `api` no segundo. `manage.py` usa `saas` como default e preserva `DJANGO_SETTINGS_MODULE` já definido. O deploy não passa `--settings`. Um ambiente com `ci` pode completar a pergunta com `core` e deixar `api` de fora. Valor efetivo em produção [nao verificado]. `app/config/settings/base.py:4–30` · `app/config/settings/ci.py:1` · `app/config/settings/saas.py:22–53` · `app/config/settings/teste_motor.py:5–13` · `bin/regua.sh:23,165–169` · `bin/pre-push.sh:198–199` · `app/manage.py:9` · `bin/deploy.sh:194–196`
- **Sem `models.py` ou fora de `BASE_DIR`.** A presença de `models.py` não participa do filtro. A função seleciona todo `AppConfig` instalado cujo `path` textual começa com `BASE_DIR/`. Um app fora desse prefixo pode ficar de fora enquanto `core` continua na lista. Um model que nunca foi importado nem registrado fica invisível mesmo com a label selecionada. `mensageria/nucleo` pertence a outro settings e tem, no deploy, um `migrate --check` separado para migrations já existentes. `app/core/tests/test_model_sem_migration_pendente.py:24–48` · `app/tenants/test_model_sem_migration_pendente.py:27–38` · `mensageria/config/settings.py:1–10` · `bin/deploy.sh:213–225`
- **`api` sem pacote `migrations`.** Com a label explícita e um model concreto registrado, a cerca espera que o Django proponha a migration inicial e saia 1. O teste registra `CercaMordeApi` sob `Meta.app_label='api'` e exige `Create model CercaMordeApi`. A asserção demonstra o contrato que o teste cobra. Sua execução com Django real neste hash e a passagem desse RED pela função nova seguem [nao verificado]. `app/core/tests/test_model_sem_migration_pendente.py:24–29,40–48` · `app/tenants/test_model_sem_migration_pendente.py:25–40`

## Mordida dos casos 25–28

| Caso | Mutante que a asserção pega |
|---|---|
| **25** | Ignorar drift no deploy normal ou consultar só depois do apply. Há migration existente pendente e o caso exige recusa, zero migrate, zero HUP e mensagem de drift. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:244–247` |
| **26** | Pular o portão em `--sem-migrate`. O caso injeta drift nesse modo e exige recusa, zero migrate e zero HUP. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:249–251` |
| **27** | Voltar ao `makemigrations --check` direto, sem a pergunta da casa. O ramo direto do falso ignora `FAKE_DRIFT_API`, então o mutante passa pelo portão e a asserção falha. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:90–97,253–255` |
| **28** | Dar verde por `No changes detected` apesar de rc 1. O falso imprime essa linha antes de falhar, e a asserção exige recusa. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:91–96,257–259` |

**Mutantes que sobrevivem.** O falso aceita qualquer linha que contenha `pergunta_sem_migration_pendente`, inclusive uma chamada quebrada que só mencione o nome. Os quatro casos injetam o mesmo rc 1, então também passam com um portão que barra exclusivamente rc 1 e dá verde em 124, 125 ou 137. Omitir só `api` **dentro da função** escapa porque o falso não executa Python e o RED de `api` usa outra chamada. Pular o drift somente em `--reload-agendado` ou `--reload-copia` também fica fora dos casos de drift. Os casos 18 e 20 ainda recusariam pelo portão posterior de migrations existentes. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:90–103,216–226,244–259` · `app/tenants/test_model_sem_migration_pendente.py:33–35` · `bin/deploy.sh:207–237`

Em leitura, conferi a sintaxe Bash e Python e executei em memória o trecho do portão com o corpo do Docker falso. O original recusou rc 1, 124, 125 e 137. Os mutantes da chamada quebrada e da recusa exclusiva de rc 1 mantiveram os resultados esperados pelos casos 25–28 no trecho. A execução integral dos selos e o vínculo criptográfico da cópia ao hash, sem metadados Git disponíveis, seguem [nao verificado]. `bin/deploy.sh:194–205` · `bin/tests/test_deploy_portoes_antes_do_migrate.sh:66–97,244–259`

## L-126, escritores, leitores e classes

**A regra vigente converge.** ZONAS INVIOLÁVEIS e PARADA item 2 exigem o `!` específico do motor. CODEX item 2 e a L-126 dão a construção autorizada ao Codex, com Opus verificador único, DIFF da competência na sombra, pista, pouso e HOM. A L-126 agora conserva esse papel após a segunda reprovação. A redação divergente permanece em `PISTA.md`: só leitura, proibição de escrever e construção de negócio pelo Opus. O cabeçalho da própria PISTA dá precedência ao CODEX consolidado. `RELATO.md:3410` documenta a leitura antiga num episódio anterior. `aval.txt:1` · `CLAUDE.md:145–146,274–277,398–406` · `app/docs/LEIS.md:147` · `app/docs/PISTA.md:8–14,80–99,112,143–152`

| Função ou valor | Quem escreve e quem lê |
|---|---|
| `BASE_DIR`, registro de apps e `labels_locais()` | `base.py` define a raiz, os settings declaram os apps e o Django fornece o registro. `labels_locais()` produz a lista ordenada. Leem a função a pergunta nova, a asserção de `checa`, os REDs de campo e o RED de `api`. `app/config/settings/base.py:4–30` · `app/config/settings/saas.py:22–53` · `app/core/tests/test_model_sem_migration_pendente.py:24–66` · `app/tenants/test_model_sem_migration_pendente.py:25–35` |
| `pergunta_sem_migration_pendente`, `verbosity`, `stdout` e status | As cercas de core e tenants chamam `checa`, que chama a pergunta com os defaults. O deploy declara `PERGUNTA_MIGR` e `PERGUNTA_MIGR_V` e as entrega a `shell -c`. A pergunta passa as labels e as opções ao Django, que responde pelo resultado do check. O Bash decide e escreve `DRIFT`, lido pelo `echo` e pela guarda de recusa. `app/core/tests/test_model_sem_migration_pendente.py:34–53,73–80` · `app/tenants/test_model_sem_migration_pendente.py:15–23` · `bin/deploy.sh:194–206` |
| Fixtures e contadores do selo | `roda` fornece as flags `FAKE_*` ao processo do teste. O Docker falso lê as flags e produz status, stdout e chamadas registradas. `roda` captura `SAIDA` e `RC` e extrai `N_MIG` e `N_HUP`. As asserções leem esses valores. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:82–109,119–126,244–259` |
| Permissão de construção e trilha | `PROIBIDOS` nasce no wrapper e é lido pelo inventário na entrada, revalidação e saída. `proibidos_do_wrapper()` lê as quatro regex selecionadas por `OPUS_CONSTROI_TAGS`. `agente_nega()` as aplica ao brief. O wrapper escreve `chamadas.log` e `codex_limite_estado.py` o lê. Chamadas reais da obra e teto Opus [nao verificado]. `bin/codex_pergunta.sh:162–175,224,541,560–570,608,761,806` · `bin/disjuntor.py:1961,2023–2039,2102–2113` · `bin/codex_limite_estado.py:46–57` |

- **Segundo juiz e derivação paralela:** o Django continua sendo a autoridade de schema. A c9 elimina o recorte de stdout no Bash. A asserção em `checa` repete a leitura do mesmo registro. Já os REDs repetem a **invocação** de `makemigrations` fora da pergunta nova, o que explica a lacuna de cobertura. `app/core/tests/test_model_sem_migration_pendente.py:24–70` · `app/tenants/test_model_sem_migration_pendente.py:25–40` · `bin/deploy.sh:194–200`
- **Segundo escritor fora da porta:** o delta acrescenta consulta com `--dry-run`. As mutações de campo e do registro de models são de teste e têm limpeza em `finally`. O ponto procedente de admissão é o brief misto do Opus. `app/core/tests/test_model_sem_migration_pendente.py:48,60–68` · `app/tenants/test_model_sem_migration_pendente.py:28–38` · `bin/disjuntor.py:2102–2113`
- **Relógio, ordem, semântica e `except`:** os timeouts ficam na borda Bash e o horário continua no cron. O drift é consultado antes de migrate, restart e HUP. `DRIFT=0` tem uma única atribuição após sucesso. `DRIFT=1` agrupa divergência e falha de consulta de forma explícita. O delta não introduz `except` na pergunta. O bloqueio remanescente de **selo/RED sem mordida** está no caminho real da função, como mostram os mutantes acima. `app/config/crons.py:326–327` · `bin/deploy.sh:194–205,338–339,361–382` · `app/core/tests/test_model_sem_migration_pendente.py:34–48` · `app/tenants/test_model_sem_migration_pendente.py:33–40`

## 26/11 23:56 · sessao f9374a425f1a · O30-F3-CONSTRUIR · verificador · origem=Code

**REPROVADO — O30-F3-CONSTRUIR, volta 11.** A junção deixou uma única rota de lote. Dois achados sustentam a reprovação: a edição comum pode sobrescrever os seis campos sem passar pela gravação auditada, e a rota reunificada perdeu o filtro universal e a seleção de colaborador previstos para a F3. O caso pontual de C foi corrigido, mas o selo ainda aceita a perda da prova de persistência. D fica na lista pelo aval. `app/relatorios/urls.py:16`, `app/ponto/services/ausencia.py:311`, `app/ponto/services/ausencia.py:757`, `app/docs/BACKLOG.md:2384`, `app/relatorios/views.py:352`, `app/ponto/tests/test_atestado_f3_contrato.py:936`, `app/docs/PROMPTS.md:3103`

Conferi os 33 blobs de destino do `diff.diff` contra a cópia. Todos coincidem, e o teste removido está ausente. A identidade do commit e a ancestralidade em relação a `59600163c` ficam [nao verificado]. `diff.diff:1`, `app/ponto/urls.py:73`, `app/templates/relatorios/atestados_lote.html:47`, `bin/sombra_regras_pessoais.py:167`

### 1. Aval e junção do lote

- **Entregue no código.** A rota do main serve prévia, CSV, PDF e ZIP sobre o mesmo recorte. O PDF embute JPEG/PNG e dá link protegido para PDF, HEIC e demais formatos. A seção «Sem documento» usa a mesma lista da prévia. Os seis campos entram no formulário e nas exportações PDF/CSV. O mapa curto está registrado. `app/relatorios/views.py:382`, `app/ponto/services/atestados_lote.py:64`, `app/ponto/services/atestados_lote.py:119`, `app/ponto/services/atestados_lote.py:159`, `app/ponto/services/atestados_lote.py:203`, `app/ponto/services/atestados_lote.py:217`, `app/templates/ponto/partials/painel_ausencia.html:95`, `app/docs/RELATO.md:13589`
- **Falta parte do aval.** O backlog da F3 pede filtro universal, período, colaborador e empresa. A tela reunificada oferece empresa, período e status binário (`Aprovadas`/`Todos`). Em memória, `q` e `colaborador` não alteraram a seleção. O tipo também fica fixado em `TIPOS_MEDICOS`, embora o desenho original previsse seleção de tipo. A prévia ficou na página do relatório, onde o desenho original previa um drawer. A junção autoriza preservar a porta do main. Uma dispensa explícita desses requisitos fica [nao verificado]. `app/docs/BACKLOG.md:1445`, `app/docs/BACKLOG.md:2384`, `app/docs/RELATO.md:13620`, `app/relatorios/views.py:352`, `app/relatorios/views.py:382`, `app/templates/relatorios/atestados_lote.html:1`
- **L-111.** A busca na cópia encontrou uma view, uma rota e um template de lote. Os dois atalhos apontam para `relatorios:atestados_lote`. `lote_atestados` aparece nos testes de remoção e no mutante que o reintroduz. O POST `anexar_atestado` é a porta do formulário individual. A antiga função de anexo, sua rota e seu pendente têm lápide. `app/relatorios/views.py:346`, `app/relatorios/urls.py:16`, `app/templates/relatorios/atestados_lote.html:9`, `app/templates/ponto/ausencias.html:42`, `app/templates/relatorios/atestados.html:12`, `app/relatorios/tests/test_atestados_lote_contrato.py:86`, `app/ponto/tests/mutantes_atestado_f3.py:159`, `app/ponto/urls.py:73`, `app/ponto/views.py:2165`, `app/core/juizes.py:1098`, `app/docs/LEIS.md:137`

### 2. Achados C e D

| Achado | Resultado |
|---|---|
| **C — caso pontual corrigido, selo parcial** | A fixture usa `aguardando_documento`, relê a linha e confirma estado e ausência de arquivo. O caso aprovado envia o POST ao `action` extraído do HTML, verifica o `302`, relê `aprovada` e confere os bytes. Os três mutantes da v10 caíram por asserção. Em duas alterações adicionais em memória, o selo passou com `pendente` em outra fixture da F3 e passou quando mantive o POST e retirei as verificações de persistência. Os dois testes vizinhos com `pendente` continuam na cópia. `app/ponto/models.py:690`, `app/ponto/tests/test_atestado_f3.py:238`, `app/ponto/tests/test_atestado_f3.py:257`, `app/ponto/tests/test_atestado_f3.py:294`, `app/ponto/tests/test_atestado_f3_contrato.py:947`, `app/ponto/tests/test_atestado_f3_contrato.py:966`, `app/ponto/tests/test_anexar_ui_colab.py:34`, `app/ponto/tests/test_gate_anexo_desconta.py:23` |
| **D — dois `xfail` nomeados, caminhos da v10 preservados** | A seleção explícita deu duas falhas esperadas. Desativei os marcadores em memória e obtive um `FAIL`, zero `ERROR`, na asserção final de cada caso. No primeiro, `criar_ausencia` já retornou quando o chamador desfaz a linha, e o arquivo sobra. No segundo, a referência aparece entre `exists()` e `storage.delete()`, e o arquivo é apagado. O aval deixa ambos na lista e registra a retenção futura. Ocorrência em produção [nao verificado]. `app/ponto/tests/mutantes_atestado_f3_c14.py:98`, `app/ponto/tests/mutantes_atestado_f3_c14.py:129`, `app/ponto/tests/mutantes_atestado_f3_c14.py:131`, `app/ponto/tests/mutantes_atestado_f3_c14.py:151`, `app/ponto/services/ausencia.py:263`, `app/ponto/services/ausencia.py:668`, `app/docs/PROMPTS.md:3103` |

**c19.** O selo normaliza os espaços do texto extraído antes de conferir os seis valores. O exportador segue emitindo os mesmos dados. A execução desse selo com PDF real fica [nao verificado]. `app/relatorios/tests/test_atestados_lote_tela.py:229`, `app/ponto/services/atestados_lote.py:203`

### 3. Quem escreve e quem lê

Os seis campos têm dois pontos explícitos de persistência no serviço. `criar_ausencia` inclui os valores validados no INSERT. `_gravar_dados_atestado` relê os seis campos e a referência sob trava, faz o UPDATE e grava trilha obrigatória. Chegam ao helper `anexar_documento`, `editar_ausencia_registro` e `_reabrir`. O POST do painel passa apenas as chaves presentes. Os formulários comuns de lançar e editar ausência passam os campos que já tratavam. `app/ponto/services/ausencia.py:288`, `app/ponto/services/ausencia.py:311`, `app/ponto/services/ausencia.py:421`, `app/ponto/services/ausencia.py:525`, `app/ponto/services/ausencia.py:581`, `app/ponto/services/ausencia.py:650`, `app/ponto/services/ausencia.py:757`, `app/ponto/services/ausencia.py:1129`, `app/ponto/services/ausencia.py:1428`, `app/ponto/views_atestado.py:31`, `app/ponto/views_atestado.py:43`, `app/ponto/views.py:2550`, `app/ponto/views.py:2949`

| Campo | Migration, modelo e leitores |
|---|---|
| `atestado_medico` | Texto inicial vazio. O painel lê na linha e PDF/CSV leem a mesma coluna. A sombra aplica a regra `TEXTO`. `app/ponto/migrations/0079_ausencia_atestado_dados.py:13`, `app/ponto/models.py:787`, `app/templates/ponto/partials/painel_ausencia.html:100`, `app/ponto/services/atestados_lote.py:102`, `bin/sombra_regras_pessoais.py:112` |
| `atestado_crm` | Texto inicial vazio. Painel, PDF e CSV leem a coluna. A sombra grava vazio. `app/ponto/migrations/0079_ausencia_atestado_dados.py:15`, `app/ponto/models.py:789`, `app/templates/ponto/partials/painel_ausencia.html:103`, `app/ponto/services/atestados_lote.py:102`, `bin/sombra_regras_pessoais.py:56` |
| `atestado_crm_uf` | Texto inicial vazio. O formulário valida duas letras maiúsculas. Painel, PDF e CSV leem a coluna. A sombra grava vazio. `app/ponto/migrations/0079_ausencia_atestado_dados.py:17`, `app/ponto/models.py:790`, `app/ponto/forms.py:16`, `app/templates/ponto/partials/painel_ausencia.html:106`, `app/ponto/services/atestados_lote.py:102`, `bin/sombra_regras_pessoais.py:56` |
| `atestado_cid` | Texto inicial vazio. Painel, PDF e CSV leem a coluna. A sombra grava vazio. O admin Django omite os seis campos. `app/ponto/migrations/0079_ausencia_atestado_dados.py:19`, `app/ponto/models.py:791`, `app/templates/ponto/partials/painel_ausencia.html:109`, `app/ponto/services/atestados_lote.py:102`, `bin/sombra_regras_pessoais.py:56`, `app/ponto/admin.py:50` |
| `atestado_emissao` | Data inicial `NULL`. O painel formata para o input de data. PDF e CSV formatam a coluna como `dd/mm/aaaa`. A sombra grava `NULL`. `app/ponto/migrations/0079_ausencia_atestado_dados.py:21`, `app/ponto/models.py:793`, `app/templates/ponto/partials/painel_ausencia.html:112`, `app/ponto/services/atestados_lote.py:104`, `bin/sombra_regras_pessoais.py:80` |
| `atestado_dias` | Inteiro inicial `NULL`, aceita zero. Painel, PDF e CSV leem os dias declarados no papel. A duração do lançamento vem de `data`, `data_fim` e `dias_corridos`. A sombra grava `NULL`. `app/ponto/migrations/0079_ausencia_atestado_dados.py:23`, `app/ponto/models.py:794`, `app/ponto/forms.py:22`, `app/templates/ponto/partials/painel_ausencia.html:115`, `app/ponto/services/atestados_lote.py:203`, `app/ponto/services/dia_decidido.py:678`, `bin/sombra_regras_pessoais.py:80` |

**O escritor único efetivo falha.** Quando a edição comum omite os campos médicos, `_gravar_dados_atestado` retorna cedo e a edição faz `ausencia.save()` integral. Simulei a chamada real do serviço com uma instância antiga e um CID mais novo no registro persistido. A edição repôs o CID antigo sem trilha dos seis. `rederivar_fim` também faz `save()` integral. O censo de menções declara que deixa essa classe fora. Na sombra, a escrita deliberada é do mascarador, por `update` e `bulk_update`, e o selo lê o resultado. `app/ponto/services/ausencia.py:311`, `app/ponto/services/ausencia.py:757`, `app/ponto/services/ausencia.py:1880`, `app/ponto/models.py:932`, `app/ponto/tests/test_atestado_f3_contrato.py:176`, `bin/sombra_mascara_pessoal.py:16`, `bin/sombra_mascara_pessoal.py:44`, `bin/sombra_selo_pessoal.py:37`

| Demais valores do diff | Origem, escrita e leitura |
|---|---|
| `empresa`, `mes`, `ano`, `de`, `ate`, `status`, `formato` | GET → view. A empresa vem de `empresas_visiveis`, a janela explícita de `janela_fechamento`, e tipos médicos do catálogo vigente. A consulta lê colaborador, tipo, status, vigência e `documento_referencia` nula. A view escreve apenas a resposta e seu nome de download. `app/relatorios/views.py:352`, `app/relatorios/views.py:382`, `app/relatorios/views.py:400`, `app/colaboradores/queries.py:78`, `app/ponto/catalogo/ausencias.py:467`, `app/ponto/services/atestados_lote.py:20`, `app/ponto/services/atestados_lote.py:35`, `app/ponto/services/dia_decidido.py:624` |
| Matrícula, nome, razão social, datas, dias, tipo, rótulo, status, minutos, presença e exigência de documento, decisor e instante | Cadastro do colaborador/empresa e linha gravada de `Ausencia`. O histórico fornece dias, rótulo, status, minutos, presença, `quem` e `quando`. A exigência consulta `estado_inicial`. Prévia, CSV e PDF leem esses valores. O ZIP lê matrícula ou PK, data, tipo, nome e bytes do anexo. `app/ponto/services/dia_decidido.py:558`, `app/ponto/services/dia_decidido.py:659`, `app/ponto/services/atestados_lote.py:14`, `app/ponto/services/atestados_lote.py:71`, `app/ponto/services/atestados_lote.py:83`, `app/ponto/services/atestados_lote.py:121`, `app/ponto/services/atestados_lote.py:188`, `app/ponto/services/atestados_lote.py:247` |
| `documento`, `status`, `prazo_documento`, `descricao`, `situacao` e compensação | A criação grava documento, estado inicial, prazo e descrição. Anexo e reabertura escrevem a chave do arquivo. `mudar_estado` grava status e limpa prazo conforme a transição. A reabertura pode acrescentar descrição e renovar prazo. O INSS escreve `situacao` pela porta do colaborador. A compensação lê `_committed`, nome, storage, banco e presença de referência antes de decidir a exclusão no ramo explícito. `app/ponto/services/ausencia.py:567`, `app/ponto/services/ausencia.py:638`, `app/ponto/services/ausencia.py:1075`, `app/ponto/services/ausencia.py:1133`, `app/ponto/services/ausencia.py:1430`, `app/ponto/services/ausencia.py:250`, `app/ponto/views.py:2566` |
| Trilha, recusa e projeção | `core.observ.evento` grava ator, IP, ação, objeto, descrição e antes/depois. Para os seis campos, a trilha leva os nomes das chaves. O anexo registra o nome anterior e o novo. A recusa usa o registrador de porta crítica. A projeção compara o insumo da folha e pode invalidar o carimbo de `FechamentoMensal`. `app/core/observ.py:111`, `app/core/observ.py:136`, `app/ponto/services/ausencia.py:339`, `app/ponto/services/ausencia.py:1140`, `app/ponto/views_atestado.py:34`, `app/core/recusa_critica.py:70`, `app/ponto/services/ausencia.py:128`, `app/ponto/services/fechamento.py:1676` |

### 4. Contraexemplos e classes da régua

- **Fonte única, com divergência na filha legada.** O histórico F2 e o lote chamam `linha_do_tempo`. PDF e CSV leem os seis atributos da própria raiz selecionada, que são os mesmos exibidos ao abrir essa raiz no painel. Cobertura, exigência de documento e relógio do lote têm as autoridades indicadas. Na filha legada, porém, o painel exibe o anexo da raiz e oferece «Substituir anexo», enquanto o POST grava o arquivo na filha. O teste confirma a gravação na filha. Reabrir o painel, baixar pela rota protegida ou exportar a raiz continua mostrando o arquivo anterior. `app/ponto/views.py:2042`, `app/ponto/services/atestados_lote.py:35`, `app/ponto/services/atestados_lote.py:56`, `app/ponto/services/atestados_lote.py:102`, `app/ponto/models.py:723`, `app/core/juizes.py:264`, `app/templates/ponto/partials/painel_ausencia.html:71`, `app/templates/ponto/partials/painel_ausencia.html:89`, `app/ponto/views.py:2146`, `app/ponto/tests/test_atestado_f3.py:169`, `app/ponto/services/dia_decidido.py:628`
- **Estado e ordem de trava.** O INSS trava o colaborador antes de gravar a ausência. O UPDATE dos seis trava `Ausencia`, mas relê apenas a referência e os campos médicos. Em memória, um POST que havia carregado `aguardando_documento` antes de uma rejeição concorrente gravou `aguardando_decisao` por cima da rejeição. A trava dos seis deixa a decisão de estado apoiada na instância antiga. Uma inversão de ordem ou um deadlock em banco fica [nao verificado]. `app/ponto/views.py:2547`, `app/colaboradores/services/colaborador.py:84`, `app/ponto/services/ausencia.py:318`, `app/ponto/services/ausencia.py:1068`, `app/ponto/services/ausencia.py:1144`
- **Formato.** JPEG/PNG com decode recusado pelo tratador viram `ANEXO ILEGIVEL` e incrementam o rodapé. Se `storage.exists()` responder verdadeiro e a abertura seguinte falhar, o erro sai antes do tratador de imagem e interrompe o PDF. Arquivo de bytes desconhecidos ainda ganha link. No ZIP, um HEIC legado salvo como `.jpg` sai com os bytes originais e a extensão `.jpg`, pois o nome vem da chave gravada. O tipo real rege o link de download e a normalização de novos uploads. `app/ponto/services/atestados_lote.py:91`, `app/ponto/services/atestados_lote.py:145`, `app/ponto/services/atestados_lote.py:166`, `app/ponto/services/atestados_lote.py:208`, `app/ponto/services/atestados_lote.py:226`, `app/ponto/services/atestados_lote.py:258`, `app/ponto/services/ausencia.py:833`, `app/ponto/views.py:2153`
- **Permissão.** O superusuário passa pelo RBAC. O lote exige `ver_relatorios`, `ver_folha` e `ver_ausencias`. O POST individual exige `ver_ausencias` e `editar_ausencias`. O colaborador comum tem a API da própria ausência aberta, que recebe apenas o documento. Dentro do mesmo tenant, a empresa do setor do operador não limita essas ações: um DP com as permissões alcança outra empresa ativa e visível pelo lote, e painel, download e POST individual buscam a ausência por PK. Isolamento entre tenants e as permissões efetivas em produção [nao verificado]. `app/colaboradores/mixins.py:322`, `app/colaboradores/mixins.py:364`, `app/relatorios/views.py:20`, `app/relatorios/views.py:345`, `app/colaboradores/queries.py:78`, `app/ponto/views_atestado.py:19`, `app/ponto/views.py:2098`, `app/ponto/views.py:2144`, `app/api/views.py:3094`, `app/api/views.py:3140`
- **Competência.** Em memória, com corte 12 e data 25/09/2026, o filtro vazio devolveu 12/08–11/09. `janela_atual` devolveu 12/09–11/10. Um intervalo livre invertido também chegou à consulta, onde uma ausência longa pode satisfazer os dois limites. Os dois desvios já estavam na lista da mínima. O relógio agora é lido uma vez na view. A competência entregue pode ser consultada e exportada pelo relatório, que lê o cadastro atual. A edição apenas médica grava os campos e a trilha, sem acionar o juiz K6 ou reescrever o TXT. Estoque já exportado em produção [nao verificado]. `app/relatorios/views.py:370`, `app/ponto/turnos.py:2842`, `app/ponto/janelas.py:24`, `app/ponto/janelas.py:60`, `app/docs/RELATO.md:13666`, `app/ponto/services/ausencia.py:1129`, `app/folha/export.py:565`
- **`except` no diff inteiro.** **1 novo**, em `_foto_no_pdf`, cobrindo abertura, carregamento e transposição da foto, conforme QUATRO-PAREIS. **2 removidos**, um da antiga view de anexo e um de `_reabrir`. **Saldo −1.** O `except` de `anexar_documento` já existia e passou a circundar um bloco maior. Os dois do ZIP são contexto, e o do mutante é texto. **`bin/`: 0 novos e 0 removidos.** `app/docs/PROMPTS.md:3078`, `app/ponto/services/atestados_lote.py:145`, `app/ponto/services/atestados_lote.py:261`, `app/ponto/services/ausencia.py:1147`, `diff.diff:1`, `app/ponto/tests/mutantes_atestado_f3.py:119`, `bin/sombra_regras_pessoais.py:55`
- **Selos e pré-requisito de pouso.** Rodei 70 testes sem banco: 67 passaram, 2 `xfail` de D e 1 erro por falta de `PIL`. Nos dez mutantes da junção, os controles passaram, nove mutantes caíram por asserção e um por `ValueError` no caso de mês inválido. O registro da mínima documenta o pouso anterior e sua suíte. Suíte integral, integração HTTP real, migration aplicada e certificado de pouso deste hash [nao verificado]. `app/ponto/tests/test_atestado_f3_contrato.py:1067`, `app/ponto/tests/mutantes_atestado_f3.py:132`, `app/relatorios/tests/test_atestados_lote_contrato.py:76`, `app/ponto/services/atestados_lote.py:29`, `app/docs/RELATO.md:13683`, `CLAUDE.md:129`

**Lista do veredito.** Bloqueiam a perda de atualização dos campos auditados, a transição sobre estado obsoleto e os filtros do aval ausentes. Ficam registrados C parcial, D autorizado em lista, o anexo invisível da filha, as bordas de arquivo e período, e o alcance entre empresas dentro do tenant. A prova integral de pouso deste hash fica [nao verificado]. `app/ponto/services/ausencia.py:757`, `app/ponto/services/ausencia.py:1144`, `app/docs/BACKLOG.md:2384`, `app/ponto/tests/test_atestado_f3_contrato.py:966`, `app/docs/PROMPTS.md:3103`, `app/ponto/tests/test_atestado_f3.py:169`, `app/ponto/services/atestados_lote.py:167`, `app/docs/RELATO.md:13666`, `app/relatorios/views.py:352`

## 26/11 23:52 · sessao 8a6a096f36d6 · CODEX-CONSTROI-MOTOR · verificador · origem=Code

**REPROVADO — `a32fe226f`.** O deploy ainda pode dar `DRIFT=0` com uma lista incompleta de apps. Os selos 27 e 28 não cobrem esse caso. A L-126 ainda dá dois destinos para uma obra de motor reprovada duas vezes. O verde dos selos no hash e a execução do portão na casca real seguem [nao verificado]. [bin/deploy.sh:191](/home/ronald/codex_ws/a32fe226fc7d61dc356a7a510739fc993e6ef894/bin/deploy.sh:191) · `bin/tests/test_deploy_portoes_antes_do_migrate.sh:248` · `app/docs/LEIS.md:147`

## 1. Os cinco achados do Opus

| Achado | Cura na origem e contraexemplo |
|---|---|
| **1. Schema sem label** | **Parcial.** O registro de apps do Django agora alimenta as duas cercas e o deploy. O teste de `tenants` registra `CercaMordeApi` sob `api` e exige rc 1 com `Create model CercaMordeApi`. No deploy, se a última linha do shell for só `core`, a consulta pode sair verde com um model novo e independente em `api`. `app/core/tests/test_model_sem_migration_pendente.py:24–37` · `app/tenants/test_model_sem_migration_pendente.py:25–40` · `bin/deploy.sh:191–200` |
| **2. Selos de host sem verde no hash** | **Em aberto.** Há casos 25–28 no selo de deploy e um mutante de `p_models_pkg` no selo do wrapper. O selo do agente cobra a leitura das quatro tags e a recusa de Opus para `models.py` e motor. Um verde anterior à mudança ainda poderia ser apresentado como prova do hash atual. RED/GREEN dos 25–28, verde do wrapper e verde do agente vinculados a `a32fe226f` [nao verificado]. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:239–257` · `bin/tests/test_codex_pergunta.sh:658` · `bin/tests/test_agente_no_modelo.sh:127–143,286–288` |
| **3. Portão na produção** | **Em aberto.** O código alcança o portão no `--reload-agendado` das 03:30 e no `--reload-copia`, inclusive na chamada de desfazer a cópia. Os modos 18 e 20 do selo simulam migration pendente, e os casos 27 e 28 executam o modo normal com Docker falso. Execução do portão novo com Django e settings efetivos da casca real [nao verificado]. `app/config/crons.py:326` · `bin/molde_fatia/cadeia.sh:32,48` · `bin/deploy.sh:63–82,191–208` · `bin/tests/test_deploy_portoes_antes_do_migrate.sh:81–98,211–221,248–254` |
| **4. Aval e papel do construtor** | **Parcial.** LEIS e ZONAS INVIOLÁVEIS agora dizem que, dado o `!` da obra, Codex constrói o motor e Opus verifica. A mesma L-126 ainda manda entregar a construção ao Opus na segunda reprovação, sem ressalvar motor ou `models.py`. Numa segunda reprovação de uma obra só de motor, a lei manda trocar o construtor e o disjuntor recusa esse despacho. `app/docs/LEIS.md:147` · `CLAUDE.md:146,274–277,402–406` · `bin/disjuntor.py:1961,2102–2112` |
| **5. Pacote `models/` liberado** | **Curado no inventário de caminhos.** A regex barra descendentes de `models/`, inclusive a criação de uma pasta vazia, que o inventário registra com `/` no fim. O selo cria `folha/models/__init__.py`, exige rc 3 com o motivo e tem mutante que retira a guarda. Um model declarado num módulo de outro nome e importado por `models.py` fica fora dessa proibição de caminho. `bin/codex_pergunta.sh:162–175,202–224,231–233` · `bin/tests/test_codex_pergunta.sh:154–162,409–412,658` |

## 2. Alcance da cerca de schema

- **`ci`** herda a lista de `base` e tem 12 apps da casa instalados. `api` aparece na seleção de **testes** da régua, mas fica fora do registro de apps de `ci`. **`teste_motor`** herda `saas` e acrescenta ao universo de `ci` `api` e `tenants`. O pre-push seleciona a suíte `tenants` nesse settings, e o teste pergunta por todas as labels locais instaladas. **`saas`** é o default declarado por `manage.py` e WSGI. O deploy não fixa `--settings`, então o ambiente da casca pode determinar outro valor. Valor efetivo em produção [nao verificado]. `app/config/settings/base.py:4–30` · `app/config/settings/ci.py:1` · `bin/regua.sh:23,168–169` · `app/config/settings/teste_motor.py:5–13` · `app/config/settings/saas.py:22–53` · `bin/pre-push.sh:198–199` · `app/manage.py:9` · `app/config/wsgi.py:14` · `bin/deploy.sh:191–197`
- **Apps omitidos.** Entre os apps declarados nesses três settings, não encontrei app da casa instalado que fique fora pelo caminho. A regra compara o prefixo textual de `AppConfig.path` com `BASE_DIR/`. Um app da casa instalado com `path` fora desse prefixo fica de fora. Há um caso concreto de outro projeto da casa, `mensageria/nucleo`, instalado no settings próprio. Seu portão no deploy checa migrations **já existentes** pendentes de aplicação. `app/core/tests/test_model_sem_migration_pendente.py:24–31` · `app/config/settings/base.py:4–30` · `app/config/settings/saas.py:22–53` · `mensageria/config/settings.py:2–10` · `mensageria/nucleo/models.py:4` · `bin/deploy.sh:215–227`
- **`Meta.app_label` e `MIGRATION_MODULES`.** Se o model foi importado e a label de destino é local e instalada, a declaração em outro módulo continua no universo. O teste de `api` cobre exatamente o registro explícito. Uma label não instalada ou excluída pelo caminho fica fora da lista. Para um model gerenciado com `MIGRATION_MODULES[app] = None`, o comportamento esperado com label explícita é propor a migration inicial, conforme o comentário da cerca. Esse caso específico segue [nao verificado]. Um pacote alternativo de migrations pode deixar o estado em dia, enquanto o inventário só veda o componente de caminho literal `migrations`. `app/core/tests/test_model_sem_migration_pendente.py:24–37` · `app/tenants/test_model_sem_migration_pendente.py:25–40` · `bin/codex_pergunta.sh:162–175`
- **Abstrato, proxy e unmanaged.** A pergunta enviada é sobre estado de migrations. Uma base abstrata sem descendente concreto não tem operação própria. Um proxy ou unmanaged novo pode gerar migration de estado sem criar tabela. Um contraexemplo é acrescentar campo a um unmanaged já existente, cuja tabela externa a cerca não inspeciona. Esses casos de autodetecção seguem [nao verificado] no Django desta cópia. `app/core/tests/test_model_sem_migration_pendente.py:34–54` · `app/requirements.txt:13`

## 3. `DRIFT=0` e a mordida dos selos

| Cenário | Resultado do portão | Casos 27 e 28 |
|---|---|---|
| **Warning na última linha** | Sem ` core `, dá `1`. Com ` core `, a linha passa à próxima etapa e é dividida em argumentos. Prosa com labels inválidas tende a dar `1`; se a última linha contiver só um subconjunto válido, como `core`, pode dar `0` com drift em `api`. Como a expansão também aceita opções, uma saída que inclua uma opção de saída antecipada como `--version` pode terminar sem autodetecção [nao verificado] no Django da casca. `bin/deploy.sh:191–200` | **Nenhum dos dois** simula stdout com prosa, lista parcial ou opção. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:90–92,248–254` |
| **Timeout** | Na consulta de labels, o status do primeiro comando se perde atrás de `tail` e a atribuição segue sem teste de rc. Se já saiu `core`, um segundo comando bem-sucedido pode dar `0`. Sem stdout útil, dá `1`. Timeout no próprio `makemigrations` dá `1`. `bin/deploy.sh:52,191–200` | O 28 cobre apenas o efeito de **stdout vazio**, com rc **0** na consulta. Nenhum simula timeout ou saída antes da falha. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:90–92,252–254` |
| **Casca fora** | Se a primeira chamada falha sem stdout, dá `1`. Se a casca cai entre as chamadas e o `makemigrations` falha, dá `1`. `--conferir` pode imprimir `DRIFT=1` e ainda terminar com rc 0, por desenho. `bin/deploy.sh:191–208,262–265` | O 28 cobre ausência de labels. Nenhum simula rc do Docker indisponível nem a queda entre chamadas. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:81–98,248–254` |

O **27** morde a perda de `api` na lista limpa do stub e exige recusa, zero migrate, zero HUP e o motivo. O **28** morde a aceitação de lista vazia e exige rc não zero, zero migrate e zero HUP. Rodei em memória o trecho do Bash com respostas simuladas. Lista parcial e timeout após imprimir `core` deram `DRIFT=0`. Timeout sem stdout, casca fora sem stdout e timeout do check deram `DRIFT=1`. Conferi também a sintaxe dos scripts e dos dois módulos Python, em leitura. Execução integral dos selos, Django real e produção [nao verificado]. `bin/deploy.sh:191–202` · `bin/tests/test_deploy_portoes_antes_do_migrate.sh:65–70,90–92,248–254`

## 4. Quem escreve e quem lê

- **`labels_locais()`** deriva a lista ordenada de `settings.BASE_DIR` e do registro de `AppConfig` do Django. Chamam diretamente `checa_sem_migration_pendente`, `morde_campo_sem_migration`, o teste de `api` duas vezes e o `shell -c` do deploy. As cercas normais de core e tenants chamam o mesmo `checa`. `app/core/tests/test_model_sem_migration_pendente.py:24–37,40–50,59–64` · `app/tenants/test_model_sem_migration_pendente.py:22–43` · `bin/deploy.sh:191–192`
- **`LABELS_CASA`** é escrita uma vez pelo Bash com a última linha do stdout. Leem a variável o `case`, o `makemigrations` que decide e a impressão e reconsulta de diagnóstico. `bin/deploy.sh:191–205`
- **`p_models_pkg`** é declarada em `PROIBIDOS` e aplicada por `re.search` aos caminhos tocados. O mesmo inventário roda no pre-check, na revalidação e no fim. O disjuntor extrai as regex do wrapper, mas seleciona quatro tags, sem `p_models_pkg`. Uma obra cujo único caminho é um pacote `models/` fica recusada nas duas portas de construtor. O inventário barra descendentes, inclusive `__init__.py`, e admite um `models.py` regular dentro de `app/`, com qualquer conteúdo que as demais cercas aceitem. Também admite módulos com outros nomes e um arquivo regular chamado `models` sem barra final. `bin/codex_pergunta.sh:162–175,209–233,541,761,806,815–823` · `bin/disjuntor.py:1961,2023–2039,2102–2112`

## 5. Terceiro sítio da regra antiga

PARADA item 2, seção 4 e a cláusula nova da L-126 concordam sobre o `!` específico para tocar o motor. O despacho registra a sequência do aval, incluindo DIFF na sombra antes de pista e pouso. **`PISTA.md` conserva o sentido antigo** em três passagens: proíbe Codex de escrever, reserva código de produção ao Code e atribui construção de negócio ao Opus. O próprio cabeçalho de `PISTA.md` dá precedência ao CODEX consolidado. `RELATO.md` ainda registra a leitura antiga de “item que pede `!`” num episódio anterior. `CLAUDE.md:146,274–277,402–406` · `app/docs/LEIS.md:147` · `app/docs/PISTA.md:8–14,83–99,143–152` · `app/docs/RELATO.md:3410`

## 6. Classes de reprovação no delta

- **Selo sem mordida e valor com dois significados.** `DRIFT=0` primeiro quer dizer “a última linha contém `core`” e depois “o check saiu 0”. A distinção deixa passar uma pergunta parcial, que os selos não exercitam. A dupla atribuição do papel do Opus na L-126 é a outra ambiguidade do delta. `bin/deploy.sh:193–202` · `bin/tests/test_deploy_portoes_antes_do_migrate.sh:248–254` · `app/docs/LEIS.md:147`
- **Derivação paralela.** As três cercas compartilham a função, e o shell reinterpreta a lista com um critério mais fraco. Já existiam outros critérios para “app da casa” em `describe` e no censo de logging, com diferenças de caminho. Nenhum deles é outra cerca de migrations. `bin/deploy.sh:191–197` · `app/core/tests/test_model_sem_migration_pendente.py:24–37` · `app/core/management/commands/describe.py:15–19,32–38` · `app/ponto/tests/test_logging_de_todo_app.py:26–36`
- **Segundo juiz, segundo escritor, relógio, ordem e `except`.** O delta mantém o autodetector do Django como juiz de schema. A escrita direta nova é no registro global de models **do teste**, com limpeza em `finally`. Os timeouts ficam na borda do deploy. O portão vem antes de migrate e HUP, sem inversão de trava nem novo `except` no delta. O falso verde permite avançar sem cumprir o pré-requisito de schema. Na cadeia, a ordem reload antes da régua já existia. Pouso efetivo com esse desvio [nao verificado]. `app/core/tests/test_model_sem_migration_pendente.py:34–37` · `app/tenants/test_model_sem_migration_pendente.py:25–40` · `bin/deploy.sh:191–208,340–341,384` · `bin/molde_fatia/cadeia.sh:48–55`

## 26/11 23:45 · sessao 579083d18fc0 · CODEX-CONSTROI-MOTOR · censo · origem=Code

**Quem perguntou.** A v2 pediu contraexemplos para qualquer mudança de schema, a v4 perguntou pela cobertura de todos os apps do deploy e a v5 pediu a conferência da L-126 cláusula por cláusula. As perguntas sobre os selos cobraram mordida e ordem. A v7 ficou restrita à troca do nome na L-126 e aos seus leitores. `voltas.txt/v2/P:9`, `voltas.txt/v4/P:12`, `voltas.txt/v4/P:14`, `voltas.txt/v5/P:4`, `voltas.txt/v7/P:3`. [nao verificado]

**Quem triou.** A v2 descartou a proibição antiga do motor na L-126, e a v3 registrou a emenda. O conflito residual com `!` ficou sem tratamento explícito. A ampliação para `models/` já estava na resposta da v1 e ficou fora da triagem seguinte. `voltas.txt/v1/R:10`, `voltas.txt/v2/P:6`, `voltas.txt/v3/P:7`, `opus.md:5`.

As referências `P` e `R` contam as linhas dentro de cada bloco da transcrição colada. A cópia local ainda contém `p_models` e, no trecho do deploy, a checagem de migrations pendentes. A identidade da cópia com o HEAD final é [nao verificado]. `bin/codex_pergunta.sh:167`, `bin/deploy.sh:185`.

| Achado | Classe | Trecho literal da pergunta | Trecho literal da resposta | Motivo |
|---|---|---|---|---|
| 1. Cerca de schema sem label | **(a)** | “Liste contra-exemplos: mudanca de model que gera migration e o teste nao ve” (`voltas.txt/v2/P:9`). “O portao novo do deploy.sh pega model sem migration em TODO app instalado em prod” (`voltas.txt/v4/P:12`). | “O portão cobre o Django do `saas_core`, inclusive `tenants`, quando a casca usa `config.settings.saas`. O comando roda sem rótulo de app e sem `--settings`.” `voltas.txt/v4/R:3`. | O tema foi perguntado. A resposta tomou a lista de apps instalados como cobertura e deixou passar o caso de `api` sem pacote de migrations, que o Opus atribuiu a `ask_initial`. `app/config/settings/saas.py:48`, `opus.md:2`. |
| 2. Selos de host sem prova verde no HEAD | **(c)** | “Os selos mordem? Os casos c_motor_livre/c_models_livre e o laco novo do test_agente_no_modelo passariam com o wrapper/disjuntor do HEAD?” `voltas.txt/v1/P:6`. | “O selo de deploy VERDE foi relatado pela sessão.” `voltas.txt/v6/R:1`. | A pergunta cobrava o comportamento dos selos. Vincular os verdes ao HEAD e comprovar o RED/GREEN dos casos 25/26 exigia os registros de execução. A prova no HEAD fica [nao verificado]. `opus.md:3`, `CLAUDE.md:427`. | [nao verificado]
| 3. Portão sem execução comprovada em produção | **(c)** | “Ha caminho de publicacao que o pule (--sem-migrate, --reload-copia, --reload-agendado, --restart, --conferir, HUP das 03:30 em config/crons.py)?” `voltas.txt/v4/P:12`. | “A declaração chama `deploy.sh --reload-agendado`, passando pela mesma guarda. O crontab instalado é **[nao verificado]**.” `voltas.txt/v4/R:12`. | O código mostra as chamadas do cron e da cópia. A execução efetiva do portão em produção fica [nao verificado]. `app/config/crons.py:326`, `bin/molde_fatia/cadeia.sh:32`, `bin/molde_fatia/cadeia.sh:48`, `CLAUDE.md:418`, `opus.md:4`. |
| 4. Dois sentidos de `!` na L-126 | **(a)** | “A L-126 agora diz o aval sem omitir nem acrescentar (clausula a clausula)?” `voltas.txt/v5/P:4`. | “A ordem da fila está no bloco geral, e a condição de `PAREI` entrou na emenda.” `voltas.txt/v5/R:20`. | A resposta conferiu `PAREI` e deixou passar o efeito da proibição geral de “item que pede `!`” sobre o motor, que a PARADA 2 ainda lista entre os itens que exigem `!`. O descarte da v2 tratava da proibição antiga, posteriormente emendada. `app/docs/LEIS.md:147`, `CLAUDE.md:146`, `CLAUDE.md:274`, `CLAUDE.md:276`, `voltas.txt/v2/P:6`, `voltas.txt/v3/P:7`, `opus.md:5`. |
| 5. Remoção da guarda libera `models/` | **(b)** | “O diff faz o que o aval diz, e so isso?” `voltas.txt/v1/P:3`. | “A remoção de `p_models` libera também a antiga categoria `models/`.” `voltas.txt/v1/R:10`. | O Codex viu a ampliação. A triagem seguinte deixou o ponto de fora, o que sustenta **(b) por omissão**. Descarte formal e motivo ficam [nao verificado]. `bin/codex_pergunta.sh:167`, `voltas.txt/v2/P:4`, `voltas.txt/v2/P:6`, `opus.md:6`. |

**Contagem em cinco achados:** **(a) 2**, **(b) 1 por omissão na triagem**, **(c) 2**, **(d) 0**, **(e) 0**. `opus.md:2–6`, `CLAUDE.md:432–440`.

## 26/11 23:39 · sessao 6cccd208fc28 · O30-F3-CONSTRUIR · construtor · origem=Code

**Causa (b)**

1. `_dados_para_exportacao` preserva o `0`, e `pdf_bytes` exclui apenas `''` e `None`. [atestados_lote.py:102](/home/ronald/wt-codex-o30-f3-construir-225036/app/ponto/services/atestados_lote.py:102) · [atestados_lote.py:205](/home/ronald/wt-codex-o30-f3-construir-225036/app/ponto/services/atestados_lote.py:205)
2. O PDF põe os seis campos no mesmo parágrafo. O texto extraído em `green_c18_falha.txt` contém `Dias do\natestado: 0`. [atestados_lote.py:207](/home/ronald/wt-codex-o30-f3-construir-225036/app/ponto/services/atestados_lote.py:207)
3. `_texto` conserva a quebra, e o `assertIn` da c18 exige a sequência literal com espaço. [test_atestados_lote_tela.py:37](/home/ronald/wt-codex-o30-f3-construir-225036/app/relatorios/tests/test_atestados_lote_tela.py:37) · [test_atestados_lote_tela.py:233](/home/ronald/wt-codex-o30-f3-construir-225036/app/relatorios/tests/test_atestados_lote_tela.py:233)

**Diff** em [test_atestados_lote_tela.py:232](/home/ronald/wt-codex-o30-f3-construir-225036/app/relatorios/tests/test_atestados_lote_tela.py:232)

```diff
         texto = _texto(PdfReader(BytesIO(resposta_pdf.content)))
+        texto = ' '.join(texto.split())
         for trecho in ('Médico: Dra. Exemplo', 'CRM: 765', 'UF do CRM: PR', 'CID: Z99.8',
```

O teste ainda exige os seis pares de rótulo e valor no texto extraído. A normalização só altera espaços em branco. Suprimir o campo inteiro ou só o `0` faz a asserção de `Dias do atestado: 0` falhar. [test_atestados_lote_tela.py:231](/home/ronald/wt-codex-o30-f3-construir-225036/app/relatorios/tests/test_atestados_lote_tela.py:231) [nao verificado]

**RED e mutantes** em memória, com o trecho extraído do anexo e a asserção do arquivo. A execução integrada pelo Code fica [nao verificado]. [test_atestados_lote_tela.py:232](/home/ronald/wt-codex-o30-f3-construir-225036/app/relatorios/tests/test_atestados_lote_tela.py:232)

```bash
sonda() {
  PYTHONDONTWRITEBYTECODE=1 python3 -B - "$1" <<'PY'
import sys
from pathlib import Path
from textwrap import dedent
from unittest import TestCase

modo = sys.argv[1]
fonte = Path('relatorios/tests/test_atestados_lote_tela.py').read_text()
inicio = fonte.index("        texto = ' '.join(texto.split())")
fim = fonte.index('\n\n', inicio)
checagem = dedent(fonte[inicio:fim])
texto = ('Médico: Dra. Exemplo · CRM: 765 · UF do CRM: PR · CID: Z99.8 · '
         'Emissão: 12/08/2026 · Dias do\natestado: 0\n')
if modo == 'red':
    checagem = checagem.split('\n', 1)[1]
elif modo == 'junta_sem_espaco':
    checagem = checagem.replace("' '.join", "''.join", 1)
elif modo == 'sem_campo':
    texto = texto.replace(' · Dias do\natestado: 0', '')
elif modo == 'sem_zero':
    texto = texto.replace('atestado: 0', 'atestado:')
exec(checagem, {'self': TestCase(), 'texto': texto})
PY
}

sonda red                # rc=1, reproduz a falha da c18
sonda corrigido          # rc=0
sonda junta_sem_espaco   # rc=1
sonda sem_campo          # rc=1
sonda sem_zero           # rc=1
```

## 26/11 23:24 · sessao 3ac36de352b7 · CODEX-CONSTROI-MOTOR · verificador · origem=Code

1. **O item (3) está curado.** A única troca é o nome da HOM pelo trecho `[homologador; nome trocado pelo papel, dado pessoal]`. Os outros **825 caracteres** da citação coincidem, caractere a caractere, com o aval raspado e com o campo `frase` do JSON. Reverter só essa troca em memória reproduziu o hash abreviado do blob anterior indicado no patch. `aval.txt:1`, `delta.patch:2`, `delta.patch:9`, `delta.patch:10`, `app/docs/LEIS.md:147`, `app/docs/CORTES.json:2475`.

2. **Ainda aparece “Ronald”**, como autor do corte já citado na linha 147 e nas linhas de contexto. O delta introduz zero nomes de pessoa, e o nome da HOM saiu da linha 147. “Claude” designa o agente verificador. No ensaio em memória com cadastro sintético, o nome da HOM passava pela raspagem na versão anterior e já estava ausente na c7. `delta.patch:6`, `delta.patch:9`, `delta.patch:10`, `app/docs/LEIS.md:147`, `CLAUDE.md:402`, `bin/nome_no_texto.py:78`, `bin/nome_no_texto.py:115`.

3. **O descarte como fila procede nesta obra.** A ocorrência em `CORTES.*` está fora do hunqe da c7. O publicador inclui `LEIS.md` e `RELATO.md`, e a tabela `SEUS CORTES` leva ao `RELATO` apenas corte, hora, idade, estado e fatia. A frase integral fica no registro de cortes. A presença anterior à obra inteira no histórico Git e a publicação efetiva no remoto ficam [nao verificado]. `delta.patch:1`, `app/docs/CORTES.json:2475`, `app/docs/CORTES.md:651`, `bin/gerar_cortes.py:87`, `bin/gerar_cortes.py:105`, `bin/gerar_cortes.py:118`, `bin/relato.sh:125`, `bin/relato.sh:158`.

4. **Quem escreve e quem lê.**
   - **Origem da L-126.** Ronald dá o aval, `CORTES.json` guarda a frase e `gerar_cortes.py` lê o JSON e escreve `CORTES.md`. A autoria da edição da linha 147 fica [nao verificado]. `app/docs/CORTES.json:2475`, `bin/gerar_cortes.py:31`, `bin/gerar_cortes.py:114`, `app/docs/LEIS.md:147`.
   - **Publicação.** `relato.sh` entrega `LEIS.md` a `raspar_publico.py`, que lê, mascara, confere e escreve a cópia. `relato.sh` faz o commit e o push. O selo de publicação lê os arquivos rastreados no destino. `bin/relato.sh:125`, `bin/relato.sh:131`, `bin/relato.sh:156`, `bin/raspar_publico.py:28`, `bin/raspar_publico.py:40`, `bin/tests/test_publicacao_sem_nome.sh:63`, `bin/tests/test_publicacao_sem_nome.sh:72`.
   - **Selos.** O índice lê o vínculo com o corte e a estrutura da tabela. O pre-push chama o selo que lê `PROTEGE`, cuja célula ficou idêntica. Na cópia, o índice retornou `RED (1)` por outros cortes sem linha. Esta emenda está mapeada na linha 423. `app/docs/LEIS.md:147`, `app/docs/LEIS.md:423`, `bin/tests/test_leis_indice.sh:25`, `bin/tests/test_leis_indice.sh:49`, `bin/tests/test_leis_indice.sh:67`, `bin/pre-push.sh:16`, `bin/tests/test_lei_protege_sitio.sh:77`.

**APROVADO — (3) curado, citação preservada fora da troca declarada, zero nomes novos e `CORTES.*` em fila nesta obra.** `delta.patch:9`, `delta.patch:10`, `app/docs/LEIS.md:147`, `bin/gerar_cortes.py:87`.

## 26/11 23:05 · sessao faa9a48effd6 · PAR-PELO-TURNO · verificador · origem=Code

## Brief c1v4

Usei o código da cópia e os anexos fornecidos. Os contraexemplos abaixo foram executados em memória, com batidas e cadastro sintéticos, sem banco e sem escrever arquivos. A identidade Git da cópia, os 107 verdes, os 9 vermelhos e a execução dos 29 mutantes ficam **[nao verificado]**. O módulo contém 29 cenários de mutação e aplica um por execução. `app/ponto/tests/test_par_pelo_turno.py:633`, `:776`, `:824`

1. **Dia por PK e borda — parcial.** A carga agora vai de `ini−4` a `fim+2`, julga turnos desde `ini−2` e só então projeta os PKs. O período público é recortado pelo dia do turno. O selo compara os dois PKs na casca, no lote e na consulta individual, incluindo o corte 20→21, com DNA fixo e ata vazia. Na sonda, `E 20/10 23:30 → S 22/10 01:30`, **26 h exatas**, fechou no dia 20 e devolveu os dois PKs no dia 20 tanto em `[20,20]` como em `[22,22]`. A folha e o espelho ainda carregam só até `fim+1`, e a folha repete esse limite por fatia. Para uma competência encerrada dia 20, a saída desse par sequer chega ao motor. `app/ponto/turnos.py:55`, `:317`, `:1851`, `:1981`, `:2020`, `:3039` · `app/ponto/tests/test_par_pelo_turno.py:132`, `:155` · `app/ponto/services/fechamento.py:264`, `:646` · `app/ponto/services/espelho.py:1055`
   
   **Contraexemplo de dia por PK.** Com DNA `21:30–05:42`, sem ata nem marcos de template, usei `E 17/10 20:30`, `S 20/10 09:00`, `E 20/10 12:00`, `S 21/10 01:00`. A pré-passagem marca `09:00 → 12:00` como pausa, usando a saída posterior como prova. O laço absorve as duas marcas antes de chegar ao corte de 26 h. Em `[17/10,21/10]` os dois PKs ficam no turno **aberto de 17/10**. Em `[20/10,20/10]` esse turno sai do contexto retornado e os mesmos PKs recebem **20/10**. A entrada de 17/10 estava carregada nas duas janelas. O limite de 26 h protege o fecho do par, enquanto o mapa ainda pode receber marcas de um turno aberto com alcance maior. `app/ponto/turnos.py:389`, `:411`, `:1377`, `:1453`, `:1981`, `:2018`, `:3051`
2. **C9 perdida por reuso — curado na origem.** `_mk` grava `Turno.c9`. O leitor valida dia e as duas pontas e deduplica pela mesma chave. O teste repareia os mesmos objetos com e sem DNA, depois relê o primeiro resultado. O mutante volta a gravar e ler o carimbo na saída e o apaga no segundo pareamento. A busca na cópia encontrou o atributo antigo apenas nas referências de teste e mutação. `app/ponto/turnos.py:41`, `:165`, `:187`, `:319` · `app/ponto/tests/test_par_pelo_turno.py:248`, `:263`, `:667`
3. **C9 × L-084 — parcial.** A C9 **chama** `minutos_ate_o_marco`, `marco_de_saida` e o predicado `_cadastro_nao_descreve`. Ela repete a montagem das duas distâncias e só julga as pontas do par. O motor também julga as pontas **do dia**, incluindo a borda posicional. O selo injeta o mapa do dia, cobre dois pares isolados e morde a volta à distância circular. Na sonda com DNA `07:00–16:00` e pares `00:30→14:00` e `18:00→22:00`, o primeiro emite C9 (`810 > 780 min`). Com os dois turnos disponíveis, o motor tira o dia da janela pela L-084, usando `00:30` e `22:00`. `app/ponto/turnos.py:170` · `app/ponto/janela_he.py:41`, `:64` · `app/ponto/motor_calculo_v2.py:1664`, `:1673`, `:1696`, `:1714`, `:2059` · `app/ponto/tests/test_par_pelo_turno.py:274`, `:692`
4. **HE sem decisão — prova do caso nominal; falha na classe.** No motor da cópia, reproduzi `17:30→06:30` com previsto de 492 min e decisão vazia. Com as duas janelas ativas, deu **492 min trabalhados, 288 fora, 0 HE**. Com a geral desligada, **288 min de HE**. Com só a saída desligada, **48 min de HE**. O diff recupera a geometria e o teste chama `calcular_periodo`. A asserção de 492 min também fixa o recorte da saída do motor desta cópia. A L-097 emendada manda conservar o trabalhado e o noturno da cauda, inclusive para turno que cruza, e classificar a HE. `app/ponto/tests/test_par_pelo_turno.py:428`, `:455` · `app/ponto/janela_he.py:183`, `:199` · `app/ponto/motor_calculo_v2.py:2872`, `:2951`, `:2963` · `app/docs/LEIS.md:115` · `app/docs/PROMPTS.md:3052`
   
   **PARE — 300 min de HE sem `DecisaoHE`, com as duas janelas ativas.** No `Motor12x36`, com DNA diurno `07:00–20:00`, previsto de 780 min e `E 08/11 07:00 → S 09/11 01:00`, o par fecha com C9 (`1.080 > 1.020 min`). A janela se aplica, mas o ramo da saída após a meia-noite conserva a saída integral e devolve zero minutos fora. O motor devolve **1.080 trabalhados, 0 fora, 300 de HE**. No caso de dois pares do item 3, a dispensa da janela pela L-084 dá **270 min de HE** no primeiro par, também com as duas flags ativas e sem decisão. São resultados do motor em memória. Crédito em `DiaPago` ou TXT de produção **[nao verificado]**. `app/ponto/turnos.py:66`, `:315`, `:1453` · `app/ponto/janela_he.py:183` · `app/ponto/motor_calculo_v2.py:1714`, `:1802`, `:1847`, `:2872`, `:2951`, `:2963`
5. **Selos — a, b, c e d ganharam asserções pertinentes.** **(a)** A fixture usa `CICLO_12x36`. **(b)** A 09 exercita o escritor antes e depois de registrar a exportação e compara SQL e hashes de `FechamentoMensal`, `DiaPago`, linha da exportação e TXT. Se o veto for retirado, a asserção `processados == 0` é a primeira defesa. O selo cobre o recálculo gravador. **(c)** O teste novo compara os dois PKs em todas as janelas da matriz. **(d)** A pausa de **2 h** mata Partido `0→3 h`. A pausa de 3h30 dá o controle acima do teto de 3 h. As saídas RED/GREEN em Django e a morte de `guarda_09` ficam **[nao verificado]**. `app/escala/models.py:26` · `app/ponto/tests/test_par_pelo_turno.py:163`, `:360`, `:532`, `:593`, `:620`, `:756` · `app/ponto/services/fechamento.py:580`
6. **Os dois vermelhos — causas distintas.** **(a)** O visitante AST da catraca deu `False` no arquivo atual e `True` ao recolocar `fim.date()` em memória. A guarda compara o dia civil da saída via `data_local` e `_data_do_turno` continua publicando o dia do turno. A allowlist fica igual no diff. O runner `base` troca o módulo em memória, e a catraca lê arquivo do disco. O RED da catraca exige a montagem de fonte informada ou a injeção de fonte que o mutante `date_keying` faz. **(b)** A entrada às `20:00` fica exatamente 3 h após a `S` das `17:00` e pode ser absorvida com ela, retirando o par fechado que alimenta a C9. Às `20:01`, o teste exige separadamente o par `07:00→17:00`, a entrada aberta, o furo real e o dono `BATIDA`, seguido da ação de regularização. A regra do dono vem do e6. A perda por reuso da C9 é o defeito distinto do item 2. `app/ponto/turnos.py:290`, `:436`, `:457` · `app/ponto/tests/test_contract_no_batida_date.py:186`, `:213`, `:232` · `app/ponto/tests/test_par_pelo_turno.py:764`, `:787`, `:816` · `app/ponto/tests/test_col129_teto_vira_lista.py:226` · `app/ponto/management/commands/e6_oraculo.py:98`, `:182`, `:198`
7. **Laudo — caso pela regra, nominal incompleto.** `col155/08-11` cabe na série HJ-026. O teste explicita que `17:29→06:31` e os PKs são sintéticos, e exige par no dia 8 com C9 de **782/732 min**. A série dá cobertura à oitava chave dominical como ensaio de regra. As pontas exatas, os papéis intermediários desse dia, a matriz dos 29 casos e o antes × depois por rubrica em sombra ficam **[nao verificado]**. `app/docs/BACKLOG.md:745`, `:2027` · `app/ponto/tests/test_par_pelo_turno.py:54`, `:79` · `app/docs/LEIS.md:136`

## Autoridades, leitores e custo

- **LEI-AKITA — padrão `Single Source of Truth`.** A geometria registrada é `turnos_do_colab` e a fonte do previsto é `minutos_previstos_do_dia`. Persiste divergência de alimentação: casca e lote usam os limites padrão do pareador, enquanto a montagem dos períodos injeta a classe do motor. Um vão de 4 h produz dois turnos no padrão e um no Plantão. O motor também mantém mapa por instante, herança de até 6 h no laço mensal e consulta de previsto por `quando.date()`, inclusive no 12x36 após aplicar a janela. São caminhos que podem discordar do dia publicado por PK. `CLAUDE.md:17`, `:20`, `:359` · `app/core/juizes.py:216` · `app/docs/JUIZES.md:20`, `:29`, `:131` · `app/ponto/turnos.py:2015` · `app/ponto/motor_calculo_v2.py:530`, `:911`, `:1341`, `:2516`, `:2951`, `:3629` · `app/ponto/tests/test_par_pelo_turno.py:360`
- **Leitores do mapa.** Espelho e API delegam a `dia_das_batidas`. O painel operacional usa `CargaDaJanela.dia_das_batidas`. Disputa, fio e serviço de jornada delegam à consulta individual, que volta ao mesmo projetor por PK. Férias e vigia de plantio também leem o mapa. Espelho e API iteram suas próprias listas de marcas, e o espelho ainda carrega só até `fim+1`. O leitor de jornada com `cross_meianoite` só aceita `data_turno` e `data_turno+1`. A delegação ao projetor, portanto, convive com diferenças de carga e de alcance na apresentação. `app/ponto/services/espelho.py:465`, `:588`, `:1055` · `app/api/views.py:1362`, `:1444`, `:1459` · `app/core/services/painel_op.py:327` · `app/chamados/services/disputa_emissao.py:283` · `app/chamados/services/fio.py:1827` · `app/escala/servico_jornada.py:249`, `:509` · `app/ferias/management/commands/listar_ferias_com_batida.py:54` · `app/ponto/management/commands/vigia_plantio_overnight.py:87`
- **Universo L-025 e consultas.** `batidas_apuraveis` continua exigindo `retratada_em IS NULL`, inclusive para pendentes de aprovação. O lote chama essa autoridade. A casca conserva o filtro ORM equivalente e a consulta histórica reapresenta uma cópia em memória. Mudam as linhas candidatas, o contexto e o custo de processá-los. No lote, a leitura das batidas continua única, o DNA lê as células já carregadas e a última batida gera até uma agregação por carga. A janela maior pode acionar essa agregação pelo O95 ou a consulta condicional de geradoras faltantes. No avulso, o mapa sem `turnos=` elimina a segunda busca de batidas. Com `turnos=` fornecido, agora executa a carga e o pareamento completos antes de substituir os turnos da janela. Contagem SQL e latência em banco **[nao verificado]**. `app/docs/LEIS.md:76` · `app/ponto/turnos.py:1161`, `:1174`, `:1875`, `:1911`, `:1930`, `:2951`, `:3006`, `:3081`, `:3097`, `:3112`, `:3132` · `app/escala/alimentacao.py:148` · `app/escala/utils.py:84`
- **Quem chama em lote.** Painel operacional, status do dia, calendário, relatório de furos, API e espelho usam `CargaDaJanela`. A carga da pergunta viva herda a classe e aplica seu próprio recorte de data sobre as linhas carregadas, chamando o mesmo corpo puro. Fechamento em massa e lavra C9 percorrem colaboradores por caminhos próprios. `app/core/services/painel_op.py:327` · `app/ponto/status_dia.py:270` · `app/colaboradores/services/calendario.py:250` · `app/relatorios/furos.py:128` · `app/api/views.py:1497` · `app/ponto/services/espelho.py:171` · `app/chamados/services/carga_pergunta_viva.py:83`, `:165` · `app/ponto/services/fechamento.py:639` · `app/ponto/services/teto_na_lista.py:43`
- **Escritor, relógio e trava.** As alterações de geometria e C9 são em memória. A persistência indireta continua pela materialização existente e pela lavra de `MetricaSnapshot` com `--apply`. O `DecisaoHE.objects.create` é da fixture. O relógio da C9 vem de `timestamp`; comando e medição conservam as duas amostragens anteriores, `localdate()` na borda e `now()` em `medir`. O TXT continua em `atomic`, travando a empresa antes de abrir a porta. O selo da 09 chama o recálculo, sem percorrer a trava do TXT. O diff fornecido preserva os `except` existentes nesses caminhos. `app/ponto/turnos.py:178`, `:326` · `app/ponto/nucleo.py:47`, `:57` · `app/ponto/management/commands/lavrar_teto_do_dia.py:22`, `:27` · `app/ponto/services/teto_na_lista.py:32`, `:71`, `:91` · `app/ponto/tests/test_par_pelo_turno.py:569`, `:604`, `:622` · `app/folha/views.py:103`, `:134` · `app/folha/export.py:556`

## Dinheiro

- **09/2026.** O recálculo gravador exclui a empresa com exportação vigente pela K6. `retratar_exportada` também veta. A leitura pura pode mostrar outro cálculo da 09. Há dois caminhos existentes fora da prova do selo: o POST de TXT recalcula em leitura e, se passar pela porta, registra **nova** `ExportacaoDominio`; o `--apply` do backfill de furos colhe o motor em leitura e pode escrever `furo_apurado` e `lavrado_em` nas linhas nulas da exportada. A lavra C9 também pode registrar fatos da 09 no snapshot de auditoria. Gravação efetiva ou mudança de rubrica da 09 em produção **[nao verificado]**. `app/docs/LEIS.md:125` · `app/docs/JUIZES.md:40` · `app/ponto/services/fechamento.py:36`, `:559`, `:580`, `:1561`, `:1783` · `app/ponto/services/dia_pago.py:451`, `:668` · `app/ponto/management/commands/apurar_furo_da_exportada.py:109`, `:131`, `:195` · `app/folha/views.py:134`, `:214`, `:275`, `:304` · `app/ponto/services/teto_na_lista.py:14`, `:91`
- **Classe que já paga e pode mudar na 11: par completo de 21/10 precedido, a mais de 3 h e até 6 h, por uma saída de par recuperado de 20/10.** Na sonda, DNA `06:00–14:00` no dia 20 e `03:00–11:00` no 21. As batidas formam `20/10 06:00→22:00` e `21/10 03:00→11:00`. O primeiro par excede o teto anterior de 12 h e agora fecha. O pareador dá **21/10** ao segundo, mas o laço mensal vê um vão de **5 h**, atribui o período ao dia **20** e o exclui da comp. 11. Em memória, a 11 teve **8 h** no controle com o segundo par isolado e **0 h** com os dois pares presentes. Esse é um mecanismo de mudança fora do par recuperado. A ocorrência e o valor gravado por colaborador na frota ficam **[nao verificado]**. `app/ponto/turnos.py:1420` no lado anterior de `obra.patch` · `app/ponto/turnos.py:66`, `:315`, `:1453` · `app/ponto/motor_calculo_v2.py:2461`, `:2513`, `:2542` · `app/ponto/services/fechamento.py:264`, `:306`, `:646`

## Quem escreve e quem lê

| Campo ou valor | Escritor ou fonte | Leitores |
|---|---|---|
| `Batida.pk`, `colaborador_id`, `timestamp`, `tipo`, `origem`, `is_intervalo`, `criado_em`, `aprovada`, `retratada_em` | Modelo/ORM e porta de registro, anotação e retratação. A reapresentação só altera a cópia. `app/ponto/models.py:8`, `:23`, `:45`, `:63`, `:79` · `app/ponto/registro_batida.py:170`, `:255`, `:287` · `app/ponto/turnos.py:1930` | Filtro de apuração, casca/lote, eco e ordem cronológica, `tipo` quando a ata se abstém, projeção por PK e hora exibida. `app/ponto/turnos.py:359`, `:1217`, `:1331`, `:1388`, `:2970`, `:3053`, `:3118` · `app/api/views.py:1459` |
| Vínculo, geradora, DNA, `hi/hf/hii/hfi`, `intervalo2`, papéis `E/S/X/Xi` | `vinculo_do_dia` julga a geradora e a vigência. `montar_dna` produz o DNA e a porta da célula grava. Cartório lavra a ata e `periodos_do_dia` projeta seus papéis. `app/docs/JUIZES.md:22`, `:143` · `app/escala/alimentacao.py:228` · `app/ponto/management/commands/gerar_celulas.py:73` · `app/ponto/portas/celula.py:290`, `:641` · `app/ponto/services/cartorio.py:1908` | `marcos_do_dia`, `marcos_dna_periodo`, carregadores do pareador e motor. O pareador publica `_papel_ata`, `_intra_ata`, `_intra_dur`, `_intra_declarado` para a geometria de pausa e o adaptador. `app/escala/models.py:1109` · `app/escala/utils.py:84` · `app/ponto/turnos.py:802`, `:1320`, `:1585`, `:1636`, `:2007` · `app/ponto/motor_calculo_v2.py:494`, `:523` · `app/ponto/adaptador_motor.py:43` |
| 26 h, pad, piso, limites de classe, teto diário, previsto líquido e fato posterior | `ENVELOPE_FECHO_S`, `_PAD_FECHO`, `DURACAO_ECO_SEG`, `AUT_*`, `_teto_da_jornada`, `minutos_previstos_do_dia` e `Max(timestamp)` das apuráveis. O teto diário usa o vão bruto cadastrado mais 4 h. `app/ponto/turnos.py:55`, `:66`, `:1911`, `:3097` · `app/ponto/management/commands/detectar_par_relampago.py:13` · `app/ponto/motor_calculo_v2.py:755`, `:2976`, `:3629` · `app/escala/utils.py:2284` | Fecho principal, O68b, O95, O195, C9, carga de borda e cálculo financeiro do excedente. `app/ponto/turnos.py:117`, `:206`, `:315`, `:1090`, `:1174`, `:1453`, `:1851` · `app/ponto/motor_calculo_v2.py:2951` |
| `Turno.data_turno`, pontas, `batidas`, `aberto`, `cross_meianoite`, dia por PK e dia por instante | `_data_do_turno` julga, `_mk` publica, `recompute_turnos` persiste a projeção e `_dia_das_batidas_puro` publica o mapa. O motor publica o mapa por instante e `dia_da_jornada`. `app/docs/JUIZES.md:20`, `:26` · `app/ponto/turnos.py:233`, `:299`, `:3039` · `app/ponto/nucleo.py:47`, `:59` · `app/ponto/motor_calculo_v2.py:911`, `:2573` | Realizado, espelho/API, painel/disputa, serviço de jornada, janela de HE e lavra por dia. `app/ponto/turnos.py:778` · `app/ponto/services/espelho.py:465` · `app/api/views.py:1444` · `app/core/services/painel_op.py:330` · `app/chamados/services/disputa_emissao.py:283` · `app/escala/servico_jornada.py:256` · `app/ponto/motor_calculo_v2.py:1646` · `app/ponto/services/dia_pago.py:174` |
| C9, chave das pontas, marcos, vão/teto, competência, vínculo/template, dono, contadores e snapshot | `_c9_do_par` emite o sinal; `pares_separados_pelo_teto` projeta as pontas e os minutos. `janela_atual` e o vínculo do dia vêm dos juízes. O e6 julga o dono. `medir` monta os itens, totais, erros e janelas; `lavrar` grava os campos de `MetricaSnapshot` sob a chave histórica. `app/ponto/turnos.py:170`, `:187` · `app/docs/JUIZES.md:35`, `:119`, `:143` · `app/ponto/management/commands/e6_oraculo.py:98`, `:182` · `app/ponto/services/teto_na_lista.py:8`, `:51`, `:61`, `:75`, `:94` | Comando/cron, `ultimo_lavrado`, assinatura Cadastro × Realidade, ação pelo dono, HTML e PDF. A chave persistida reúne fotos de duas versões da geometria, e o texto atual descreve o excesso do vão em ambas. `app/config/crons.py:521` · `app/ponto/services/teto_na_lista.py:101` · `app/escala/services/cadastro_realidade.py:101`, `:425`, `:1081`, `:1319` · `app/templates/escala/cadastro_x_realidade.html:31` |
| Corte da empresa, flags/vigência/piso/teto de HE, extra declarada, `DecisaoHE`, pontas, pendências e rubricas | Cadastro pela porta de empresa e cadastro de escala. Gestão grava decisão pela porta de HE. Motor produz pontas efetivas, minutos e relato. Espelho e lavra de pendências publicam a projeção. `app/colaboradores/services/estrutura.py:18` · `app/colaboradores/models.py:58`, `:101`, `:117` · `app/escala/models.py:1221` · `app/ponto/portas/he.py:159` · `app/ponto/motor_calculo_v2.py:1757`, `:1781`, `:1865` · `app/ponto/services/espelho.py:790` · `app/ponto/services/he_pendente_lavrado.py:93`, `:183` | Juiz da janela de competência, motor, Gestão e porta da folha. Fechamento grava o mensal, `dia_pago.lavrar` grava os dias, e `ExportacaoDominio.registrar` guarda TXT e hash. `app/ponto/janelas.py:36`, `:60` · `app/ponto/motor_calculo_v2.py:1504`, `:1511`, `:1764` · `app/ponto/services/gestao_he.py:397` · `app/folha/porta_export.py:190` · `app/ponto/services/fechamento.py:1168`, `:1225` · `app/ponto/services/dia_pago.py:359` · `app/folha/models.py:174` |

**REPROVADO — (1) dia por PK ainda varia e folha/espelho perdem a segunda virada (`app/ponto/turnos.py:2018`, `:3051`; `app/ponto/services/fechamento.py:651`), (2) C9 e L-084 do dia divergem (`app/ponto/turnos.py:170`; `app/ponto/motor_calculo_v2.py:1696`), (3) janela ativa permite 300 min de HE sem decisão (`app/ponto/janela_he.py:183`; `app/ponto/motor_calculo_v2.py:2963`), (4) par já pagável pode sair da comp. 11 (`app/ponto/motor_calculo_v2.py:2516`, `:2542`), (5) laudo nominal e sombra por rubrica [nao verificado] (`app/docs/BACKLOG.md:2027`).**

## 26/11 23:04 · sessao 0012e9bc9774 · CODEX-CONSTROI-MOTOR · verificador · origem=Code

**1. Os achados (2) e (4) foram curados no escopo da v5.** No (2), as duas chamadas usam `timeout -k 10 180`: `TERM` aos 180 s e `KILL` 10 s depois se o processo local resistir. São até cerca de 380 s nas duas chamadas em sequência. Num ensaio em memória com prazos reduzidos e um processo que ignora `TERM`, o portão recusou com `rc=1` antes de migrate, restart ou HUP. `--conferir` continua podendo sair `0` com `DRIFT=1`. O selo de deploy VERDE foi relatado pela sessão. A execução integral do selo, o `PATH` de produção e o encerramento do processo dentro do container são [nao verificado]. `bin/deploy.sh:189`, `bin/deploy.sh:196`, `bin/deploy.sh:253`, `bin/deploy.sh:332`, `bin/deploy.sh:358`, `bin/deploy.sh:375`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:64`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:236`.

**2. Sim, a citação bate caractere por caractere.** Comparei o conteúdo entre aspas com o campo `frase` do JSON, retirando o prefixo `aval Ronald: <id>. `, incluindo o espaço separador. São 833 caracteres em cada lado, zero diferenças. Isso cura o (4): o pre-mortem e `DEFAULT DENY` voltaram, e as referências da cerca estão na coluna `dono (aplica)`. A referência `:646` foi preservada na citação. Na cópia, a permissão está na linha 644 e a 646 abre o ramo de leitura. `app/docs/LEIS.md:57`, `app/docs/LEIS.md:147`, `app/docs/CORTES.json:2475`, `bin/codex_pergunta.sh:644`, `bin/codex_pergunta.sh:646`.

**3. O delta afeta o publicador da LEIS.md.** A citação põe o primeiro nome do responsável pela HOM na linha que `relato.sh` entrega ao raspador para publicar. `LEIS.md` é tratada como prosa, e o filtro de token isolado nessa modalidade só mascara caixa alta. Em memória, com a linha real e um cadastro sintético contendo o mesmo primeiro nome, o token passou intacto e o verificador do raspador devolveu zero achados. O cadastro efetivo e a publicação no remoto são [nao verificado]. `app/docs/LEIS.md:147`, `bin/relato.sh:2`, `bin/relato.sh:125`, `bin/relato.sh:131`, `bin/relato.sh:158`, `bin/raspar_publico.py:28`, `bin/nome_no_texto.py:39`, `bin/nome_no_texto.py:78`, `bin/nome_no_texto.py:115`, `bin/nome_no_texto.py:139`.

- O leitor de `PROTEGE` continua recebendo a mesma célula e a mesma estrutura. Conferi que a função apontada existe na cópia. O `test_leis_indice` rodou em leitura e devolveu `RED (1)` pela lista de cortes sem linha, da qual este corte ficou fora porque está mapeado na linha 423. A igualdade integral desse RED com c5 e main foi relatada pela sessão [nao verificado]. `app/docs/LEIS.md:147`, `app/docs/LEIS.md:423`, `bin/tests/test_lei_protege_sitio.sh:49`, `bin/tests/test_lei_protege_sitio.sh:72`, `bin/codex_limite_estado.py:71`, `bin/tests/test_leis_indice.sh:25`, `bin/tests/test_leis_indice.sh:49`, `bin/tests/test_leis_indice.sh:67`.

**4. Quem escreve e quem lê**

| O que o delta toca | Quem escreve → quem lê |
|---|---|
| Prazo, comando e resultado do portão | A regra reserva alterações em `bin/` ao construtor Opus. A autoria da c6 é [nao verificado]. `deploy.sh` fornece prazo e flags. O Django na casca fornece status e saída. O deploy usa o status para definir `DRIFT`, usa `tail` e `sed` no diagnóstico e sai `1` quando recusa. O cron, a cadeia da fatia e o selo chamam o deploy. O selo captura status e saída e conta migrate e HUP. `CLAUDE.md:402`, `app/manage.py:9`, `bin/deploy.sh:92`, `bin/deploy.sh:189`, `bin/deploy.sh:196`, `app/config/crons.py:326`, `bin/molde_fatia/cadeia.sh:48`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:114`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:236`. |
| Citação e cerca na L-126 | Ronald é a fonte do aval registrado no JSON. A autoria da edição da linha é [nao verificado]. O índice lê o vínculo com o corte e a forma da linha. O pre-push lê `PROTEGE`. `relato.sh` lê a linha por meio do raspador, que escreve a cópia destinada ao repo público. Os dois testes citados e o deploy consultam o Django sobre divergência de schema. `app/docs/CORTES.json:2475`, `app/docs/LEIS.md:147`, `bin/tests/test_leis_indice.sh:25`, `bin/pre-push.sh:16`, `bin/tests/test_lei_protege_sitio.sh:49`, `bin/relato.sh:131`, `bin/raspar_publico.py:40`, `app/core/tests/test_model_sem_migration_pendente.py:41`, `app/tenants/test_model_sem_migration_pendente.py:16`, `bin/deploy.sh:189`. |
| Trilha de pronto citada na lei | O wrapper escreve `ini` e `fim` em `chamadas.log`. `codex_limite_estado.py` lê obra, papel e resultado e conta inícios por papel. O teto de um construtor Opus é regra de conduta. O cumprimento efetivo é [nao verificado]. `app/docs/LEIS.md:147`, `bin/codex_pergunta.sh:568`, `bin/codex_pergunta.sh:606`, `bin/codex_limite_estado.py:55`, `bin/codex_limite_estado.py:71`, `CLAUDE.md:396`. |

**REPROVADO — (3) a nova citação leva o primeiro nome da HOM ao caminho de publicação, e a raspagem de `LEIS.md` deixa passar esse formato.** `app/docs/LEIS.md:147`, `bin/relato.sh:131`, `bin/nome_no_texto.py:39`, `bin/nome_no_texto.py:78`.

## 26/11 23:03 · sessao dc6e366c80e6 · O30-F3-CONSTRUIR · construtor · origem=Code

**O30-F3-CONSTRUIR · c18 · base `4cca2e935bc7f541cf8b99340023d68a6e5af56f`**

A rota `relatorios:atestados_lote` gera a prévia, o CSV, o PDF e o ZIP a partir do mesmo recorte. O CSV e o PDF entregam os seis campos a quem passa pelas três permissões do main. O PDF incorpora JPEG e PNG e aponta o HEIC para o mesmo `abrir anexo` do painel. A tela mantém empresa obrigatória e visível, padrão de aprovadas, seletor de status, tabela detalhada e volta ao acumulado. O nome dos três downloads inclui empresa e datas. `relatorios/urls.py:16`, `relatorios/views.py:346`, `relatorios/views.py:382`, `templates/relatorios/atestados_lote.html:5`, `templates/relatorios/atestados_lote.html:47`, `ponto/services/atestados_lote.py:159`, `ponto/views.py:2138`.

Com as duas datas livres vazias, a view preenche a parte ausente ou inválida da competência com o mês ou ano civil de uma única leitura da data local. Preserva a parte válida e delega o corte à janela da empresa. Duas datas livres válidas continuam prevalecendo. `relatorios/views.py:370`, `ponto/services/atestados_lote.py:20`, `ponto/janelas.py:54`.

Retirei `lote_atestados`, `FiltrosLoteAtestado`, `_colaboradores_filtrados`, o teto de 20 e os testes e mutantes da estrutura retirada. Os casos de comportamento do lote passaram para `relatorios`, incluindo PDF, HEIC, seção sem documento e exportação com mais de 20 lançamentos. O POST de anexo e os testes do formulário do painel seguem na porta existente. `ponto/views_atestado.py:17`, `ponto/tests/mutantes_atestado_f3.py:136`, `ponto/tests/mutantes_atestado_f3_c14.py:20`, `relatorios/tests/test_atestados_lote_tela.py:213`, `relatorios/tests/test_atestados_lote_tela.py:298`, `ponto/tests/test_atestado_f3.py:238`.

### RED, GREEN e mutantes

Comandos a partir de `app/`. Cada chamada `m` executa um caso independente em memória. Os dois REDs abaixo também podem ser repetidos lendo a base pelo executor. `ponto/tests/mutantes_atestado_f3.py:88`, `ponto/tests/mutantes_atestado_f3.py:136`.

```bash
export PYTHONDONTWRITEBYTECODE=1 GIT_OPTIONAL_LOCKS=0
m() { python3 -B -m ponto.tests.mutantes_atestado_f3 "$@"; }

m red_c18_lote_base
python3 -B -m unittest -v relatorios.tests.test_atestados_lote_contrato

m c18_csv_sem_campos
m c18_pdf_cai_na_previa
m c18_pdf_sem_campos_e_anexos
m c18_pdf_link_relativo
m c18_download_sem_ver_ausencias
m c7_botao_cru
m c7_pdf_embute_heic

m c18_periodo_so_com_dois_ausentes
m c18_periodo_le_dois_dias
m c18_recoloca_view_orfa

m red_c18_catraca_base
m selos
m c18_extrato_com_linhas_antigas
m c18_retorno_com_linha_antiga
```

| Item | RED na base | GREEN local e mutantes |
|---|---|---|
| **1 · PDF e CSV** | Seis campos ausentes no CSV. PDF sem `application/pdf`. | Os contratos da junção passaram. Os sete mutantes de exportação, guarda, botão e HEIC deram `rc=1`. `relatorios/tests/test_atestados_lote_contrato.py:51`, `ponto/tests/mutantes_atestado_f3.py:136`, `ponto/tests/test_atestado_f3_contrato.py:1098`. |
| **2 · Competência** | Duas reproduções chegaram a `ValueError: Informe mes e ano ou as duas datas do periodo.` A tentativa pelo Django Client parou em `ModuleNotFoundError: No module named 'django.core'`. O RED e o GREEN pelo Client ficam **[nao verificado]**. | Os contratos sem banco passaram. Um mutante voltou ao `ValueError` com só um componente inválido e o outro misturou setembro de 2026 com o ano de 2027. Ambos deram `rc=1`. `relatorios/tests/test_atestados_lote_contrato.py:75`, `relatorios/tests/test_atestados_lote_tela.py:170`, `ponto/services/atestados_lote.py:30`, `manage.py:11`. |
| **3 · L-111** | O contrato encontrou os três símbolos órfãos. | O contrato passou e o mutante que recoloca a view deu `rc=1`. O selo do painel também passou e `python3 -B -m ponto.tests.mutantes_atestado_f3_c14 painel_sem_pendente` deu `rc=1`. `relatorios/tests/test_atestados_lote_contrato.py:85`, `ponto/tests/mutantes_atestado_f3.py:159`, `ponto/tests/test_atestado_f3_contrato.py:936`. |
| **4b · Catraca** | `linhas mudaram: registrar no MESMO commit`. A base já divergia em `regularizar_retorno`. | Registrei as posições medidas, `[561, 668]` para `extrato_parcial` e `[3387]` para `regularizar_retorno`. A igualdade passou, `selos` deu `14/14` e restaurar cada registro antigo isoladamente deu `rc=1`. `core/tests/test_medida_turno_do_par.py:342`, `core/tests/test_medida_turno_do_par.py:562`, `relatorios/views.py:561`, `relatorios/views.py:668`, `ponto/views.py:3387`. |
| **4c · Arquitetura** | `ARQUITETURA.mmd` ficou para a regeneração da sessão. Resultado do selo **[nao verificado]**. `core/tests/test_selo_diagrama_do_codigo.py:44`. | |

**4a · Ruff B023.** Vinculei a exceção da iteração ao argumento do callback e mantive as asserções do teste. O RED marcou `B023` na linha 667 da base. O GREEN trouxe `All checks passed!` nos Python alterados e o mutante voltou a dar `B023`, `rc=1`. O teste de rollback também passou isoladamente. `ponto/tests/test_atestado_f3_contrato.py:629`, `ponto/tests/test_atestado_f3_contrato.py:663`.

```bash
f=ponto/tests/test_atestado_f3_contrato.py
git show "4cca2e935bc7f541cf8b99340023d68a6e5af56f:app/$f" |
  ruff check --no-cache --select B023 --stdin-filename "$f" -
ruff check --no-cache --select B023 "$f"
sed 's/, erro=falha//; s/^                    raise erro$/                    raise falha/' "$f" |
  ruff check --no-cache --select B023 --stdin-filename "$f" -
```

### Quem escreve e quem lê

| Campos | Quem escreve | Quem lê |
|---|---|---|
| `atestado_medico`, `atestado_crm`, `atestado_crm_uf`, `atestado_cid`, `atestado_emissao`, `atestado_dias` | A porta de ausência na criação e em `_gravar_dados_atestado`, usado por edição, anexo e reabertura. `ponto/services/ausencia.py:329`, `ponto/services/ausencia.py:581`, `ponto/services/ausencia.py:758`, `ponto/services/ausencia.py:1130`, `ponto/services/ausencia.py:1429`. | O mesmo leitor `_dados_para_exportacao` alimenta CSV e PDF, depois da guarda da view. `relatorios/views.py:347`, `ponto/services/atestados_lote.py:102`, `ponto/services/atestados_lote.py:126`, `ponto/services/atestados_lote.py:203`. |
| `documento` | As portas de criação, anexo e reabertura. `ponto/services/ausencia.py:585`, `ponto/services/ausencia.py:1133`, `ponto/services/ausencia.py:1431`. | A linha do tempo lê a presença, o PDF e o ZIP leem o arquivo, e `servir_documento_ausencia` lê o original pelo link protegido. `ponto/services/dia_decidido.py:685`, `ponto/services/atestados_lote.py:159`, `ponto/services/atestados_lote.py:235`, `ponto/views.py:2138`. |
| `empresa`, `status`, `mes`, `ano`, `de`, `ate`, `formato` | O formulário envia os filtros, os links acrescentam `formato` e a view resolve a parte ausente da competência atual. `templates/relatorios/atestados_lote.html:9`, `templates/relatorios/atestados_lote.html:26`, `templates/relatorios/atestados_lote.html:46`, `relatorios/views.py:376`. | A view lê os parâmetros, chama o serviço da janela e escreve o tipo e o nome do download que o navegador recebe. `relatorios/views.py:352`, `relatorios/views.py:382`, `relatorios/views.py:389`, `relatorios/views.py:400`. |
| Cadastro da empresa e do colaborador, tipo médico e dados do lançamento | As portas de estrutura e de colaborador gravam os cadastros, o `save` do colaborador atribui matrícula e as portas de ausência gravam tipo, período, estado e decisão. `colaboradores/services/estrutura.py:13`, `colaboradores/services/colaborador.py:126`, `colaboradores/models.py:438`, `ponto/services/ausencia.py:575`, `ponto/services/ausencia.py:746`, `ponto/services/ausencia.py:1075`, `ponto/services/ausencia.py:1205`, `ponto/services/ausencia.py:1896`. | Visibilidade e corte vêm das consultas de empresa. Seleção, dias, situação, minutos, responsável e data da decisão vêm do catálogo e da linha do tempo. `colaboradores/queries.py:78`, `ponto/janelas.py:36`, `ponto/catalogo/ausencias.py:366`, `ponto/services/dia_decidido.py:558`, `ponto/services/dia_decidido.py:678`, `ponto/services/atestados_lote.py:128`. |

**Vizinhos para o Code.** A execução integral pelo Django, o PDF real que requer `PIL` e a certificação do Code e do Opus ficam **[nao verificado]**. `requirements.txt:13`, `requirements.txt:20`, `ponto/tests/test_atestado_f3_contrato.py:1068`.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test \
  relatorios ponto.tests.test_atestado_f3 ponto.tests.test_atestado_f3_contrato \
  core.tests.test_medida_turno_do_par --settings=config.settings.ci
```

**LEI-AKITA padrao=Single Source of Truth** — uma porta pública de leitura, uma linha do tempo para prévia e exportações e a porta de ausência como escritora dos seis campos. `relatorios/urls.py:16`, `relatorios/views.py:387`, `ponto/services/atestados_lote.py:50`, `ponto/services/ausencia.py:329`.

## 26/11 23:00 · sessao 8ad195b03b50 · CODEX-CONSTROI-MOTOR · verificador · origem=Code

### Achados da v4 e resolução pelo `PATH`

- **(1) O descarte procede no escopo do aval.** O construtor Codex só escreve em `<wt>/app`, e o inventário recusa alterações fora de `app/`. `mensageria/nucleo` fica fora desse alcance. A lacuna técnica continua para um deploy de `mensageria`: seu Django instala `nucleo`, e o script só roda `migrate --check` nessa casca antes de poder incluí-la no HUP. Uma alegação de cobertura de todos os apps de produção continuaria incorreta. `bin/codex_pergunta.sh:173`, `bin/codex_pergunta.sh:779`, `docker-compose.mensageria.yml:14`, `mensageria/config/settings.py:9`, `bin/deploy.sh:210`, `bin/deploy.sh:375`.
- **(2) Curado para falhas que retornam, com um limite de espera ainda incompleto.** O primeiro `timeout 180` transforma qualquer retorno não zero em `DRIFT=1`. Em modo de publicação, a segunda chamada é diagnóstica e termina em `falhou`, com a mensagem “sem resposta não se publica”, antes de apply, restart ou HUP. São duas esperas em sequência. Com GNU `timeout`, cada chamada envia `TERM` aos 180 s e, sem `-k`, pode continuar esperando se o processo não encerrar. A expiração real e o encerramento do comando dentro da casca são [nao verificado]. Em `--conferir`, `DRIFT=1` ainda pode sair com status `0`. `bin/deploy.sh:92`, `bin/deploy.sh:189`, `bin/deploy.sh:196`, `bin/deploy.sh:253`, `bin/deploy.sh:332`, `bin/deploy.sh:358`, `bin/deploy.sh:375`.
- **(3) Curado.** O caso 25 agora põe `FAKE_PEND_CORE=1` junto com `FAKE_DRIFT=1` no modo normal e exige recusa com motivo, zero apply e zero HUP. No recorte em memória, a ordem atual deu `rc=1 / apply=0 / HUP=0`; com o portão depois do apply, `1 / 1 / 0`; com o portão removido, `0 / 1 / 1`. Isso confirma a mordida do caso 25. Os resultados integrais dos selos e o total de dois casos vermelhos no mutante sem guarda são os relatados pela sessão, [nao verificado] em execução integral nesta auditoria. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:88`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:116`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:236`, `bin/deploy.sh:249`, `bin/deploy.sh:332`.

**`PATH` no deploy e no selo.** No deploy, o shell busca `timeout` no `PATH` do host, e `timeout` busca `docker` nesse mesmo `PATH`. O `PATH` efetivo e a disponibilidade de `timeout` no host de produção, inclusive no cron, são [nao verificado]. Se o binário faltar, o deploy trata a falha como `DRIFT=1` e recusa a publicação. No selo, `roda` coloca `$F/fakebin` na frente, e `monta` confirma que o `docker` encontrado é o fake e que ele executa. Assim, com `timeout` disponível no caminho herdado, ele chama o fake, que registra os argumentos e lê `FAKE_DRIFT`. O caso 25 sozinho aceita a recusa por `timeout` ausente. O caso 3 exige deploy bem-sucedido e detecta essa ausência. O selo de reload também antepõe `fakebin`, mas seu fake responde ao `makemigrations` pelo ramo padrão de sucesso. `bin/deploy.sh:189`, `bin/deploy.sh:196`, `app/config/crons.py:326`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:83`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:103`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:114`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:147`, `bin/tests/test_deploy_reload_gracioso.sh:53`, `bin/tests/test_deploy_reload_gracioso.sh:94`.

### (4) L-126, cláusula por cláusula

| Cláusula do aval | Conferência |
|---|---|
| ID, pre-mortem e `DEFAULT DENY` | ID e data presentes. Pre-mortem e `DEFAULT DENY cumprido` seguem omitidos na L-126. O registro literal os conserva. `app/docs/LEIS.md:147`, `app/docs/CORTES.json:2475`. |
| Wrapper libera motor e `models.py` sem migration. Continuam proibidos migrations, `bin/`, crons e `core/juizes.py` | Permissão e quatro proibições presentes. A referência literal `:646` ficou no registro do aval. Na cópia, a mensagem de permissão está na linha 644 e a linha 646 abre o ramo de leitura. `app/docs/LEIS.md:147`, `app/docs/CORTES.json:2475`, `bin/codex_pergunta.sh:644`, `bin/codex_pergunta.sh:646`. |
| Codex → Opus único, só confere e carimba → DIFF da competência na sombra → pista → pouso → HOM do [nome] | Sequência integral, com a exigência de cumprir todos os passos. `app/docs/LEIS.md:147`, `app/docs/CORTES.json:2475`. |
| Teto de um construtor Opus, só para o que segue proibido | Presente. A frase sobre Opus não herdar motor/models por reprovação explicita como a regra específica prevalece sobre o repasse geral da segunda reprovação. `app/docs/LEIS.md:147`, `app/docs/CORTES.json:2475`. |
| Aplicação imediata às três obras | ABONO c1h, ABONO-PARCIAL-TETO e E4C-F5 c2 aparecem. `app/docs/LEIS.md:147`. |
| Proibido pousar motor sem DIFF, tocar competência exportada e Codex gerar migration | As três proibições estão na linha. A de competência exportada está no bloco geral. `app/docs/LEIS.md:147`. |
| Pronto com `chamadas.log` e no máximo um subagente Opus de construção vivo | Os dois requisitos agora estão explícitos. Cumprimento efetivo [nao verificado]. `app/docs/LEIS.md:147`, `app/docs/CORTES.json:2475`. |
| Segue a fila. `PAREI` só em lei ou `!` | A ordem da fila está no bloco geral, e a condição de `PAREI` entrou na emenda. `app/docs/LEIS.md:147`, `app/docs/CORTES.json:2475`. |

A c5 curou as duas omissões operacionais apontadas no veredito da v4. A equivalência literal ainda falha pelo pre-mortem e `DEFAULT DENY` omitidos e pelas referências acrescentadas aos dois testes Django e ao portão do deploy. `app/docs/LEIS.md:147`, `app/docs/CORTES.json:2475`.

### Quem escreve e quem lê

| O que o delta toca | Escritor → leitor |
|---|---|
| Os dois arquivos em `bin/` | A regra atribui a construção em `bin/` a Opus, e o despacho reserva a vaga Opus para a obra. A autoria efetiva do c5 é [nao verificado]. O host executa o deploy. O selo lê `DEPLOY_SRC` ou o deploy da cópia e executa uma cópia em sua raiz de teste. `CLAUDE.md:402`, `app/docs/PROMPTS.md:3106`, `app/config/crons.py:326`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:57`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:68`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:114`. |
| `180`, comando, retorno, `DRIFT` e diagnóstico | O deploy fornece prazo, casca e flags. `manage.py` fornece o default de settings. O deploy lê o status de `timeout`, escreve `DRIFT=0/1` e o lê no `echo` e na guarda, junto com `SO_CONFERIR` derivado de `$1`. A segunda chamada fornece saída a `tail` e `sed`. `falhou` escreve a mensagem e sai `1`. O selo e a cadeia da fatia consomem saída e status. `app/manage.py:9`, `bin/deploy.sh:56`, `bin/deploy.sh:63`, `bin/deploy.sh:92`, `bin/deploy.sh:189`, `bin/deploy.sh:196`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:114`, `bin/molde_fatia/cadeia.sh:48`. |
| Entradas e resultados do caso 25 | O caso escreve `FAKE_PEND_CORE` e `FAKE_DRIFT`. O fake lê ambos e grava os argumentos em `docker.log`. `roda` captura `RC` e `SAIDA` e calcula `N_MIG` e `N_HUP` a partir do log. O caso lê os quatro resultados, `_erro` incrementa `FALHAS` e o final do selo lê o total. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:60`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:83`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:114`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:236`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:244`. |
| L-126 e trilha de pronto | Ronald é a fonte do aval. A emenda está em `LEIS.md`, com autoria da edição [nao verificado]. O selo do índice lê o vínculo entre `CORTES.json` e `LEIS.md` e a estrutura da linha. O wrapper escreve `ini` e `fim` em `chamadas.log`. `codex_limite_estado.py` lê obra, papel e resultado e conta inícios por papel. O teto de Opus é regra de conduta. `app/docs/CORTES.json:2475`, `app/docs/LEIS.md:147`, `app/docs/LEIS.md:423`, `bin/tests/test_leis_indice.sh:25`, `bin/tests/test_leis_indice.sh:43`, `bin/codex_pergunta.sh:568`, `bin/codex_pergunta.sh:606`, `bin/codex_limite_estado.py:55`, `bin/codex_limite_estado.py:71`, `CLAUDE.md:397`. |

Os três blobs da cópia conferem com o lado novo do patch. A identidade do commit completo é [nao verificado]. `app/docs/LEIS.md:147`, `bin/deploy.sh:189`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:236`.

**REPROVADO — (2) o portão ainda pode esperar indefinidamente se o processo resistir a `TERM`; (4) a L-126 ainda omite o pre-mortem e `DEFAULT DENY` e acrescenta referências de implementação.** `bin/deploy.sh:189`, `bin/deploy.sh:196`, `app/docs/LEIS.md:147`, `app/docs/CORTES.json:2475`.

## 26/11 22:52 · sessao a535fb3809bc · CODEX-CONSTROI-MOTOR · verificador · origem=Code

### 1. Cobertura e caminhos de publicação

O portão cobre o Django do `saas_core`, inclusive `tenants`, quando a casca usa `config.settings.saas`. O comando roda sem rótulo de app e sem `--settings`. O `manage.py` usa `saas` por padrão, preservando um `DJANGO_SETTINGS_MODULE` já definido. Nesse perfil, `INSTALLED_APPS` reúne os apps compartilhados e os de tenant, com `tenants` entre os compartilhados. O ambiente efetivo do container em produção é **[nao verificado]**. [bin/deploy.sh:189] [app/manage.py:9] [app/config/settings/saas.py:22] [app/config/settings/saas.py:53] [nao verificado]

Se “TODO app em prod” inclui `mensageria`, a cobertura é parcial. Seu Django separado instala `nucleo` e o deploy só lhe pergunta `migrate --check`, que cobre migrations ainda por aplicar. Um drift apenas nos models de `nucleo` fica fora do novo portão, inclusive no HUP conjunto. O boot de `mensageria` também tem seu próprio `migrate` no Compose. [mensageria/manage.py:4] [mensageria/config/settings.py:6] [bin/deploy.sh:189] [bin/deploy.sh:206] [bin/deploy.sh:375] [docker-compose.mensageria.yml:20] [nao verificado]

| Caminho | Efeito do portão |
|---|---|
| Normal, `--sem-migrate`, `--sem-sombra`, `--reload-copia`, `--reload-agendado` | Todos precisam passar pela mesma guarda antes do HUP. As flags afetam outros ramos do deploy. [bin/deploy.sh:56] [bin/deploy.sh:82] [bin/deploy.sh:195] [bin/deploy.sh:224] [bin/deploy.sh:375] | [nao verificado]
| `--restart` | A mesma guarda vem antes do restart e, portanto, antes do migrate de boot de `mensageria` disparado por ele. [bin/deploy.sh:195] [bin/deploy.sh:358] [docker-compose.mensageria.yml:20] | [nao verificado]
| `--conferir` | Executa a checagem, imprime `DRIFT` e pode sair `0` com `DRIFT=1`. Sai antes de apply, restart ou HUP. [bin/deploy.sh:189] [bin/deploy.sh:195] [bin/deploy.sh:253] | [nao verificado]
| Cron das 03:30 | A declaração chama `deploy.sh --reload-agendado`, passando pela mesma guarda. O crontab instalado é **[nao verificado]**. [app/config/crons.py:326] [bin/deploy.sh:195] [bin/selo_reciclagem.sh:70] | [nao verificado]

A pista ainda pode certificar sem `tenants`. O certificador cobra as labels da régua, que omitem `tenants`. O pre-push roda o teste separado e o c4 acrescenta a recusa no deploy. Recriação via Compose é um caminho de infraestrutura externo ao script. Seu uso em produção é **[nao verificado]**. [bin/pista_certifica.sh:67] [bin/pista_certifica.sh:85] [bin/regua.sh:23] [bin/pre-push.sh:198] [bin/deploy.sh:20] [bin/deploy.sh:189] [nao verificado]

### 2. Ordem e falha operacional

Dentro do `deploy.sh`, o portão vem antes dos `migrate --check` e `--plan`, do apply do core, do restart e do HUP. Guardas anteriores podem encerrar o script mais cedo. Qualquer retorno diferente de zero do primeiro `docker exec` vira `DRIFT=1`. Nos modos de publicação, o segundo `exec` só produz diagnóstico e `falhou` encerra com `1`, mesmo se essa segunda chamada voltar verde. A mensagem atribui a recusa a migration faltante também em caso de falha operacional. [bin/deploy.sh:101] [bin/deploy.sh:146] [bin/deploy.sh:189] [bin/deploy.sh:200] [bin/deploy.sh:210] [bin/deploy.sh:221] [bin/deploy.sh:331] [bin/deploy.sh:358] [bin/deploy.sh:375] [bin/deploy.sh:92] [nao verificado]

Uma casca fora ou um timeout que devolva código de erro aciona a recusa. As duas chamadas do portão estão sem limite de tempo próprio. Se uma ficar pendurada, o script fica esperando antes do apply e do reload. Em memória, retornos fictícios `125` e `124` deram `rc=1`, zero apply e zero HUP. Casca fora e timeout reais são **[nao verificado]**. [bin/deploy.sh:189] [bin/deploy.sh:196] [bin/tests/test_deploy_portoes_antes_do_migrate.sh:88] [nao verificado]

### 3. Mordida do selo

O fake registra a chamada e casa `*makemigrations*--check*` primeiro no ramo `exec`, antes dos padrões de migrate e do `*) exit 0`. O caso 25 exige recusa com motivo, zero apply e zero HUP. O 26 exige recusa, zero apply e zero HUP em `--sem-migrate`. [bin/tests/test_deploy_portoes_antes_do_migrate.sh:83] [bin/tests/test_deploy_portoes_antes_do_migrate.sh:88] [bin/tests/test_deploy_portoes_antes_do_migrate.sh:235]

**Os dois casos passam com o portão deslocado para depois do apply e antes do HUP.** Reproduzi em memória os trechos do deploy e o despacho do fake. Ambos continuaram com `rc=1`, zero apply e zero HUP. O caso 25 deixa `FAKE_PEND_CORE` no default `0` e o 26 usa um modo que recusa pendência sem aplicar. Com `FAKE_DRIFT=1` **e** `FAKE_PEND_CORE=1` no modo normal, a ordem atual fez zero apply e a deslocada fez um apply antes de recusar. O caso 3 também deixa essa lacuna, pois sua ordem só compara auth, sombra e prova de casca com o apply. [bin/tests/test_deploy_portoes_antes_do_migrate.sh:88] [bin/tests/test_deploy_portoes_antes_do_migrate.sh:116] [bin/tests/test_deploy_portoes_antes_do_migrate.sh:131] [bin/tests/test_deploy_portoes_antes_do_migrate.sh:147] [bin/tests/test_deploy_portoes_antes_do_migrate.sh:235] [bin/deploy.sh:220] [bin/deploy.sh:331] [nao verificado]

### 4. Emenda da L-126

| Cláusula do aval | Onde aparece na L-126 |
|---|---|
| Identificação do aval e pre-mortem com `DEFAULT DENY` cumprido | ID e data abrem a emenda. Pre-mortem e `DEFAULT DENY` constam do registro literal do aval. A emenda omite ambos. [app/docs/LEIS.md:147] [app/docs/CORTES.json:2475] |
| Wrapper libera motor e `models.py` sem migration. Permanecem proibidos migrations, `bin/`, crons e `core/juizes.py` | Na abertura da emenda e nos quatro caminhos enumerados na restrição ao construtor Opus. O inventário do wrapper contém os quatro. [app/docs/LEIS.md:147] [bin/codex_pergunta.sh:162] |
| Toda obra segue Codex → Opus único, só confere e carimba → DIFF da competência na sombra → pista → pouso → HOM do [nome] | A sequência está inteira na emenda, com “nenhum passo sai”. [app/docs/LEIS.md:147] [app/docs/CORTES.json:2475] | [nao verificado]
| Teto de um construtor Opus, só para o que segue proibido ao Codex | A emenda traz “teto 1 vivo” e a restrição. “NAO herda obra de motor/models por reprovacao” explicita a consequência dessas duas cláusulas diante do repasse geral na segunda reprovação. **O alcance da autorização é o mesmo.** [app/docs/LEIS.md:147] [app/docs/CORTES.json:2475] |
| Vale já para ABONO c1h, ABONO-PARCIAL-TETO e E4C-F5 c2 | As três obras estão nomeadas na emenda. [app/docs/LEIS.md:147] |
| Proibido pousar motor sem DIFF, tocar competência exportada e Codex gerar migration | Pouso sem DIFF e geração de migration estão no fim da emenda. Competência exportada já está no `PROIBIDO ao construtor` da mesma linha. [app/docs/LEIS.md:147] | [nao verificado]
| Pronto com construtor Codex em `chamadas.log` nas obras de motor e no máximo um subagente Opus de construção vivo | A emenda traz o teto, mas omite o registro exigido em `chamadas.log` como critério de pronto. Seu cumprimento efetivo é **[nao verificado]**. [app/docs/LEIS.md:147] [app/docs/CORTES.json:2475] |
| Segue a fila. `PAREI` só em lei ou `!` | A regra geral da L-126 preserva a ordem da fila. A condição de `PAREI` fica fora da emenda. [app/docs/LEIS.md:147] [app/docs/CORTES.json:2475] |

Os dois testes Django e o portão do deploy são detalhes de implementação acrescentados à emenda. A referência `:646` fica no aval literal, conforme o descarte informado. [app/docs/LEIS.md:147] [app/docs/CORTES.json:2475] [app/docs/CORTES.md:651] [nao verificado]

### 5. Quem escreve e quem lê

| Campo ou valor | Escritor → leitor |
|---|---|
| `MODO`, `SO_CONFERIR` | O chamador fornece `$1`. O deploy grava `MODO` e inicializa `SO_CONFERIR=0`, passando a `1` só com `--conferir`. Leitores de `SO_CONFERIR` são as guardas de auth/sombra, drift e mensageria, o ramo de pendência e a saída de conferência. [bin/deploy.sh:56] [bin/deploy.sh:126] [bin/deploy.sh:195] [bin/deploy.sh:216] [bin/deploy.sh:222] [bin/deploy.sh:253] | [nao verificado]
| Settings, flags, retorno e `DRIFT` | `manage.py` fornece o default de settings e o deploy fornece `--check --dry-run --noinput`. O primeiro `docker exec` entrega o status à condição. Só o deploy atribui `DRIFT=0/1`. O `echo` e a guarda leem esse valor. [app/manage.py:9] [bin/deploy.sh:189] [bin/deploy.sh:194] | [nao verificado]
| Diagnóstico e `falhou` | O segundo `exec` fornece a saída a `tail` e `sed`. O próprio script define `falhou`, que lê o motivo, imprime e sai `1`. Também o chamam as guardas de árvore, cron, auth, sombra, pendências, prova de casca, reciclagem, apply, reload e provas finais. O selo captura saída e status. A cadeia de fatia usa ambos no reload da cópia. [bin/deploy.sh:92] [bin/deploy.sh:101] [bin/deploy.sh:112] [bin/deploy.sh:147] [bin/deploy.sh:164] [bin/deploy.sh:196] [bin/deploy.sh:216] [bin/deploy.sh:313] [bin/deploy.sh:321] [bin/deploy.sh:332] [bin/deploy.sh:358] [bin/deploy.sh:375] [bin/deploy.sh:388] [bin/deploy.sh:398] [bin/deploy.sh:434] [bin/deploy.sh:441] [bin/tests/test_deploy_portoes_antes_do_migrate.sh:114] [bin/molde_fatia/cadeia.sh:48] | [nao verificado]
| `FAKE_DRIFT`, `RC`, `SAIDA`, `N_MIG`, `N_HUP`, `FALHAS` | Os casos 25/26 põem `FAKE_DRIFT=1` no ambiente de `roda`. O fake lê o valor, usa `0` por padrão e grava os argumentos em `docker.log`. `roda` captura status e saída e lê o log para contar apply e HUP. Os dois casos leem status e contagens. Só o 25 lê `SAIDA`. `_erro` incrementa `FALHAS` e o final do selo lê o total. [bin/tests/test_deploy_portoes_antes_do_migrate.sh:59] [bin/tests/test_deploy_portoes_antes_do_migrate.sh:83] [bin/tests/test_deploy_portoes_antes_do_migrate.sh:114] [bin/tests/test_deploy_portoes_antes_do_migrate.sh:235] [bin/tests/test_deploy_portoes_antes_do_migrate.sh:243] |
| `max_length`, `antes`, `saida`, `SystemExit` e operação | Models e migrations declaram `evento=50` e `nome=120`. Os dois testes fornecem app, model, campo e operação esperada. O helper lê o valor original, cria `StringIO` **antes** da mutação, soma `7` e restaura no `finally`. `makemigrations` lê os metadados e fornece saída e código. O helper exige código `1` e o texto esperado no buffer. [app/folha/models.py:47] [app/folha/migrations/0009_evento_he100_rubrica_200.py:27] [app/tenants/models.py:16] [app/tenants/migrations/0001_initial.py:21] [app/core/tests/test_model_sem_migration_pendente.py:26] [app/core/tests/test_model_sem_migration_pendente.py:45] [app/tenants/test_model_sem_migration_pendente.py:19] | [nao verificado]
| L-126 e trilha de pronto | A emenda grava a regra em `LEIS.md`. O selo do índice lê o mapeamento do ID e a estrutura da linha. O wrapper grava `ini` e `fim` em `chamadas.log`. O leitor de estado extrai obra, papel e resultado e conta os inícios por papel. O teto Opus é conduta declarada. [app/docs/LEIS.md:147] [app/docs/LEIS.md:423] [bin/tests/test_leis_indice.sh:25] [bin/tests/test_leis_indice.sh:43] [bin/codex_pergunta.sh:568] [bin/codex_pergunta.sh:606] [bin/codex_limite_estado.py:49] [bin/codex_limite_estado.py:71] [CLAUDE.md:397] |

Os quatro blobs da cópia conferem com o lado novo do patch fornecido. A identidade do commit completo, os logs relatados e a execução integral dos selos são **[nao verificado]**. [bin/deploy.sh:189] [bin/tests/test_deploy_portoes_antes_do_migrate.sh:235] [app/core/tests/test_model_sem_migration_pendente.py:28] [app/docs/LEIS.md:147] [nao verificado]

**REPROVADO — (3) os casos 25/26 passam com o portão depois do apply. (4) A L-126 omite o critério de pronto em `chamadas.log` e a condição de `PAREI`.** [bin/tests/test_deploy_portoes_antes_do_migrate.sh:235] [bin/deploy.sh:331] [app/docs/LEIS.md:147] [app/docs/CORTES.json:2475] [nao verificado]

_8530 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
