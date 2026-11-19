# CODEX-ESTADO

> Ultima resposta processada: **19/11/2026 19:03** · publicado em **19/11/2026 19:07** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

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

_7416 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
