# CODEX-ESTADO

> Ultima resposta processada: **16/11/2026 02:14** · publicado em **16/11/2026 02:14** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 16/11 02:14 · sessao 63a47c44631b · LOTE7-C6 · verificador · origem=Code

1. **BLOQUEIA — `decidir` ainda invalida duas vezes num único ato.** Contraexemplo: declaração de 240 minutos em `aguardando_decisao`, aprovada com período 08:00–12:00. `mudar_estado` grava a aprovação e invalida; depois `decidir` grava as horas e invalida novamente. O “antes” do ato foi dividido em dois snapshots, contrariando o comentário “Um ato = uma invalidacao”. (`app/ponto/services/ausencia.py:966`, `app/ponto/services/ausencia.py:971`, `app/ponto/services/ausencia.py:1050`, `app/ponto/services/ausencia.py:1053`, `app/ponto/services/ausencia.py:1069`, `app/ponto/services/ausencia.py:1087`)

   Os testes separam justamente essa composição: aprovação **sem período** e alteração de período numa linha **já aprovada**. Nenhum desses dois casos detecta a duplicação acima. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:305`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:309`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:320`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:324`)

2. **BLOQUEIA — o acordador invalida competências que nenhum dos lados ocupa.** Ele transforma a união de dias de antes/depois em um intervalo contínuo, usando apenas mínimo e máximo. Contraexemplo, empresa com corte 21: mover uma falta diária aprovada de 10/09 para 10/11 também invalida outubro, embora a ausência nunca tenha ocupado essa competência. Isso mantém a divergência “contador maior que universo alterado”. (`app/ponto/services/ausencia.py:174`, `app/ponto/services/ausencia.py:175`, `app/ponto/services/ausencia.py:178`, `app/ponto/services/fechamento.py:1595`, `app/ponto/services/fechamento.py:1599`, `app/ponto/services/fechamento.py:1653`, `app/ponto/services/fechamento.py:1656`)

3. **BLOQUEIA — o selo permite escrita de insumo por uma isenção.** Se o censo não atribuir o receptor e a função importar outro modelo de `ponto.models`, a escrita é descartada. Contraexemplo aceito pela regra: função que importa `Batida`, recebe uma `Ausencia` em `a` e executa `a.save(update_fields=['status'])`. O campo é monitorado, mas a isenção posterior libera a função. (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:89`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:105`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:108`)

   Existem outras passagens: métodos de classe não entram na varredura; uma chamada do acordador dentro de `if False` satisfaz o selo; atribuir o mesmo receptor a um dict em qualquer ramo permite descartá-lo. Portanto, presença textual do acordador não prova execução nem cobertura de todos os escritores. (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:55`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:59`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:75`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:101`)

   Os mutantes fornecidos têm mordida na análise AST: retiram o acordador dos escritores reconhecidos e acrescentam cinco formas de escrita. Eles não abrangem as passagens acima. Execução Django e RED contra `f8ee770a5`: **[nao verificado]**. (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:157`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:166`)

4. **BLOQUEIA — o teste “rejulga a célula” não executa o julgamento.** Ele substitui `_julgar_dias` por mock e compara somente os IDs recebidos. Assim não executa nem a função que calcula os dias nem `cartorio.julgar_celula`. Contraexemplo: quebrar o corpo do julgador mantém esse teste passando; ele também não exige que exista uma célula julgável. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:364`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:366`, `app/ponto/signals.py:92`, `app/ponto/signals.py:93`, `app/ponto/services/cartorio.py:2027`)

5. **FILA — outras provas novas têm alcance menor que o nome.**

   - O teste de Justificativa rejeitada→pendente não conserva/recarrega a Justificativa nem verifica seu status final. Remover a atualização de Justificativa ainda satisfaz suas duas asserções: retorno `ok` e fechamento preservado. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:349`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:354`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:355`, `app/ponto/services/regularizacao.py:102`)
   - `_nada_mudou` verifica apenas agendamento não cancelado e ausência não rejeitada; não prova saldo, observações e trilhas anteriores preservados. Os testes novos exercitam cancelamento individual e massa; a captura em `corrigir_concluida_admin` não tem caso correspondente nesse arquivo. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:412`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:417`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:426`, `app/ferias/views.py:452`)
   - As fixtures principais possuem proteção contra vazio: o espelho exige quantidade igual aos dias, a rejeição exige cinco linhas, e o espião de invalidação usa `wraps` da função real. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:198`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:210`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:85`)

6. **FILA — relógio continua sendo lido dentro do acordador.** `_acordar_projecao` não recebe `hoje/agora`; lê `localdate()` internamente, inclusive antes de descobrir que os snapshots são iguais. Contraexemplo: os mesmos snapshots de afastamento aberto produzem alcance diferente conforme a data da execução. (`app/ponto/services/ausencia.py:142`, `app/ponto/services/ausencia.py:162`, `app/ponto/services/ausencia.py:165`, `app/ponto/services/ausencia.py:171`)

   Os casos derivados de `_Base` têm congelamento; `FeriasTelaTrilhaTest` herda diretamente de `TestCase`, sem esse congelamento, e seu caso de massa atravessa `datetime.now()`. Não achei chamada direta a `timezone.now()` no arquivo novo; há chamadas indiretas nos serviços/modelo exercitados. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:53`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:381`, `app/ferias/views.py:608`, `app/ponto/services/regularizacao.py:85`, `app/ponto/models.py:883`)

7. **FILA — conferência dos snapshots por escritor.** A leitura do banco protege `mudar_estado` mesmo sendo feita depois da atribuição em memória de `status`; a gravação ainda não ocorreu nesse ponto. A exceção de composição é `decidir`, descrita no achado 1. (`app/ponto/services/ausencia.py:131`, `app/ponto/services/ausencia.py:955`, `app/ponto/services/ausencia.py:966`, `app/ponto/services/ausencia.py:967`)

   | Função | Antes / última escrita / depois |
   |---|---|
   | `criar_ausencia` | Antes vazio para nascimento; `save` em `app/ponto/services/ausencia.py:545`; snapshot/acordador em `app/ponto/services/ausencia.py:580`. Retornos de linha existente saem antes, em `app/ponto/services/ausencia.py:373`, `app/ponto/services/ausencia.py:426`, `app/ponto/services/ausencia.py:439`. |
   | `editar_ausencia_registro` | Antes em `app/ponto/services/ausencia.py:630`, antes das atribuições; escrita em `app/ponto/services/ausencia.py:642`; depois em `app/ponto/services/ausencia.py:652`. O rejulgamento posterior só mexe em estados abertos, fora da folha. (`app/ponto/services/ausencia.py:658`, `app/ponto/services/ausencia.py:670`) |
   | `mudar_estado` | Antes em `app/ponto/services/ausencia.py:966`; escrita com derivações em `app/ponto/services/ausencia.py:967`; depois em `app/ponto/services/ausencia.py:971`. |
   | `decidir` | Transição já gravada/invalidação em `app/ponto/services/ausencia.py:1050`; segundo antes em `app/ponto/services/ausencia.py:1053`; carimbos/período em `app/ponto/services/ausencia.py:1069`; segundo depois em `app/ponto/services/ausencia.py:1087`. |
   | `_reabrir` | Anexo e descrição são gravados antes; mudança de status delegada em `app/ponto/services/ausencia.py:1284`; prazo posterior fica fora do insumo. (`app/ponto/services/ausencia.py:1273`, `app/ponto/services/ausencia.py:1283`, `app/ponto/services/ausencia.py:1291`) |
   | `rejulgar_status_pelo_tipo` | Só estados abertos; delega a transição e depois pode gravar prazo. (`app/ponto/services/ausencia.py:670`, `app/ponto/services/ausencia.py:675`, `app/ponto/services/ausencia.py:681`) |
   | `escalonar_aguardando_documento` | Não grava Ausencia diretamente; chama `mudar_estado` para aguardando_documento→aguardando_decisao. (`app/ponto/services/ausencia.py:1132`, `app/ponto/services/ausencia.py:1144`) |
   | `anexar_documento` | Grava anexo e delega eventual transição entre estados fora da folha. (`app/ponto/services/ausencia.py:997`, `app/ponto/services/ausencia.py:1011`, `app/ponto/services/ausencia.py:1012`) |
   | `espelhar_ferias` | Captura antes dos deletes nos dois ramos; criações passam pela porta. (`app/ponto/services/ausencia.py:1336`, `app/ponto/services/ausencia.py:1338`, `app/ponto/services/ausencia.py:1340`, `app/ponto/services/ausencia.py:1353`, `app/ponto/services/ausencia.py:1363`, `app/ponto/services/ausencia.py:1367`) |
   | `apagar_ausencias_de_ferias` | Antes em `app/ponto/services/ausencia.py:1424`; delete em `app/ponto/services/ausencia.py:1425`; depois vazio em `app/ponto/services/ausencia.py:1427`. |
   | `rejeitar_ausencias_de_ferias` | Linhas travadas; antes em `app/ponto/services/ausencia.py:1457`; saves em `app/ponto/services/ausencia.py:1462`; depois em `app/ponto/services/ausencia.py:1473`. |
   | `apagar_ausencias_em_ensaio` | Captura por colaborador em `app/ponto/services/ausencia.py:1702`; delete em `app/ponto/services/ausencia.py:1703`; depois vazio em `app/ponto/services/ausencia.py:1705`. |
   | `rederivar_fim` | Antes em `app/ponto/services/ausencia.py:1715`; save/refresh em `app/ponto/services/ausencia.py:1716`; depois em `app/ponto/services/ausencia.py:1720`. |

   Há outra chamada com múltiplas invalidações: espelhar cinco dias novos de férias chama cinco vezes `criar_ausencia`, cada uma chamando o acordador. Não existe agregação por competência nesse nascimento. (`app/ponto/services/ausencia.py:1348`, `app/ponto/services/ausencia.py:1353`, `app/ponto/services/ausencia.py:580`)

8. **FILA — o predicado de estado é o mesmo; o universo completo não é.** `_insumo_da_folha` reutiliza exatamente `_pelo_efeito(...,'folha')`, como os leitores principais do fechamento. Esse predicado exige `aprovada`. O desconto passa também por `folha.export`, cujo literal é igualmente `aprovada`; não achei caminho nesses consumidores que conte uma não aprovada. (`app/ponto/services/ausencia.py:138`, `app/ponto/services/fechamento.py:639`, `app/ponto/services/fechamento.py:683`, `app/ponto/turnos.py:2528`, `app/folha/export.py:88`, `app/ponto/services/fechamento.py:873`)

   O contrário existe no sentido de seleção: `_insumo_da_folha` recebe PKs sem recorte temporal, enquanto o fechamento recorta a janela e a cobertura desempata por data/PK. Contraexemplo: uma aprovada de setembro aparece no snapshot solicitado pelo PK, mas não participa do fechamento de novembro. Isso não é divergência do filtro de status. (`app/ponto/services/ausencia.py:138`, `app/ponto/turnos.py:2543`, `app/ponto/turnos.py:2765`, `app/ponto/turnos.py:2793`)

   **Campos adicionais efetivamente lidos:** `status`, `colaborador` e `pk` não estão em `CAMPOS_DO_INSUMO`; são representados, respectivamente, pela presença no dict, pelo argumento separado do acordador e pela chave do dict. `efeito` deriva de `tipo`. Não achei outro campo de fato faltando nos caminhos de cálculo examinados. (`app/ponto/services/ausencia.py:119`, `app/ponto/services/ausencia.py:138`, `app/ponto/services/ausencia.py:139`, `app/ponto/services/ausencia.py:142`, `app/ponto/models.py:850`)

9. **FILA — escritores e leitores de Ausencia confirmados; os significados não são uniformes em todos os consumidores.**

   | Campo | Escritores confirmados | Leitores confirmados |
   |---|---|---|
   | `status` | Nascimento: `app/ponto/services/ausencia.py:469`; transição: `app/ponto/services/ausencia.py:955`; rejeição de férias: `app/ponto/services/ausencia.py:1460`. | Propriedades/derivação: `app/ponto/models.py:720`, `app/ponto/models.py:741`, `app/ponto/models.py:882`, `app/ponto/models.py:887`; folha/cobrança: `app/ponto/turnos.py:2529`, `app/ponto/turnos.py:2531`; portas: `app/ponto/services/ausencia.py:670`, `app/ponto/services/ausencia.py:948`, `app/ponto/services/ausencia.py:1011`, `app/ponto/services/ausencia.py:1240`; consumidores: `app/chamados/services/tarefa_do_dia.py:310`, `app/chamados/services/silencio_ausencia.py:78`, `app/chamados/services/materializacao.py:662`; API: `app/api/views.py:1061`, `app/api/views.py:1869`. |
   | `tipo` | Nascimento: `app/ponto/services/ausencia.py:467`; edição: `app/ponto/services/ausencia.py:631`. | Efeito/documento/derivação: `app/ponto/models.py:729`, `app/ponto/models.py:852`, `app/ponto/models.py:887`, `app/ponto/models.py:912`; cobertura: `app/ponto/turnos.py:2546`, `app/ponto/turnos.py:2612`, `app/ponto/turnos.py:2786`; fechamento: `app/ponto/services/fechamento.py:694`; crédito: `app/ponto/services/credito_parcial.py:27`; precedência: `app/ponto/precedencia.py:127`, `app/ponto/precedencia.py:339`; apresentação: `app/ponto/services/dia_decidido.py:481`, `app/colaboradores/services/calendario.py:706`. |
   | `data` | Nascimento: `app/ponto/services/ausencia.py:467`; edição: `app/ponto/services/ausencia.py:632`. | Cobertura/vigência/sobreposição: `app/ponto/turnos.py:2543`, `app/ponto/turnos.py:2567`, `app/ponto/turnos.py:2790`, `app/ponto/turnos.py:2844`, `app/ponto/turnos.py:3170`; rateio: `app/folha/export.py:118`; crédito: `app/ponto/services/credito_parcial.py:28`; fato novo: `app/ponto/services/cartorio.py:383`; linha do tempo: `app/ponto/services/dia_decidido.py:548`, `app/ponto/services/dia_decidido.py:619`. |
   | `data_fim` | Porta/edição: `app/ponto/services/ausencia.py:468`, `app/ponto/services/ausencia.py:633`; derivação do modelo: `app/ponto/models.py:917`. | Cobertura/parcial: `app/ponto/turnos.py:2545`, `app/ponto/turnos.py:2613`, `app/ponto/turnos.py:2784`; rateio: `app/folha/export.py:117`; fato novo: `app/ponto/services/cartorio.py:384`; calendário: `app/colaboradores/services/calendario.py:708`; apresentação: `app/ponto/services/dia_decidido.py:558`, `app/ponto/services/dia_decidido.py:620`. |
   | `minutos` | Nascimento: `app/ponto/services/ausencia.py:468`; edição: `app/ponto/services/ausencia.py:641`; derivação: `app/ponto/models.py:897`. | Parcial: `app/ponto/turnos.py:2614`; desconto: `app/folha/export.py:121`; crédito: `app/ponto/services/credito_parcial.py:28`; efeito: `app/ponto/services/efeito_ausencia.py:50`; apresentação/API: `app/ponto/services/dia_decidido.py:493`, `app/ponto/services/dia_decidido.py:520`, `app/api/views.py:1870`. |
   | `hora_ini`, `hora_fim` | Atribuição em `app/ponto/services/ausencia.py:1031`, persistida por `app/ponto/services/ausencia.py:1069`. | Snapshot: `app/ponto/services/ausencia.py:139`; comparação/trilha: `app/ponto/services/ausencia.py:1048`; juiz: `app/ponto/turnos.py:2646`; grade chama esse juiz em `app/escala/utils.py:1234`; formulário: `app/templates/ponto/partials/painel_ausencia.html:314`, `app/templates/ponto/partials/painel_ausencia.html:316`. |
   | `atualizado_em` | `auto_now`: `app/ponto/models.py:824`; saves completos em `app/ponto/services/ausencia.py:545`, `app/ponto/services/ausencia.py:642`, `app/ponto/services/ausencia.py:1716`; período: `app/ponto/services/ausencia.py:1032`; rejeição de férias: `app/ponto/services/ausencia.py:1462`. | Fato novo: `app/ponto/services/cartorio.py:385`; proteção/registro de reversão: `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:123`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:154`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:189`. |

   Outros leitores confirmados de tipo/data/fim/status estão em relatório, inteligência, vigias, histórico, precedência e telas; portanto o alcance não se limita à folha. (`app/relatorios/views.py:197`, `app/relatorios/views.py:205`, `app/relatorios/views.py:277`, `app/relatorios/views.py:292`, `app/inteligencia/resumo.py:39`, `app/inteligencia/views.py:71`, `app/inteligencia/views.py:239`, `app/inteligencia/views.py:516`, `app/ponto/services/vigia_ausencia.py:92`, `app/ponto/services/vigia_ausencia_veredito.py:54`, `app/chamados/services/historico.py:132`, `app/templates/ponto/ausencias.html:150`, `app/templates/ponto/ausencias.html:177`)

   **Não afirmo que esse inventário feche todos os aliases, leitores indiretos e templates da árvore: exaustividade global [nao verificado].**

10. **FILA — há incompatibilidades concretas de significado nos leitores existentes.**

    - **Status:** a API de chamados ainda exige `Ausencia.status == 'pendente'` para oferecer anexo, embora os estados válidos sejam `aguardando_documento/aguardando_decisao/aprovada/rejeitada`. Contraexemplo: ausência aguardando documento não recebe esse bloco de anexo. (`app/api/views.py:1061`, `app/ponto/models.py:701`)
    - **Fim NULL:** cobertura prolonga somente `TIPOS_EM_ABERTO`; `houve_fato_novo` aceita NULL apenas no dia inicial; `decididos_do_periodo` também expande NULL como dia único. Contraexemplo: afastamento desde 01/11 sem fim cobre 04/11, mas esses dois últimos leitores não tratam 04/11 como parte do mesmo range. (`app/ponto/turnos.py:2786`, `app/ponto/services/cartorio.py:384`, `app/ponto/services/dia_decidido.py:558`)
    - **Carimbo de mudança:** `mudar_estado` não inclui `atualizado_em` em `update_fields`. Contraexemplo: rejeitada com `aprovada_em` antigo→aguardando→aprovada, sem novo período/carimbo fornecido; pode mudar o status sem produzir carimbo novo consultado por `houve_fato_novo`. (`app/ponto/services/ausencia.py:954`, `app/ponto/services/ausencia.py:965`, `app/ponto/models.py:882`, `app/ponto/services/cartorio.py:385`)
    - **Minutos:** a unidade é minuto; para falta de range, o valor é total do registro e o exportador divide pelos dias. O modelo calcula minutos antes de derivar um fim ausente. Contraexemplo já caracterizado pelo teste: três dias previstos de 480 minutos terminam com total 480, depois rateado pelos três dias. O teste registra esse comportamento, não sua correção. (`app/ponto/models.py:892`, `app/ponto/models.py:897`, `app/ponto/models.py:917`, `app/folha/export.py:118`, `app/folha/export.py:121`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:451`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:455`)

    Nos juízes examinados, datas são inclusivas; o período horário também é inclusivo, e fim menor que início significa madrugada seguinte do turno iniciado em `data`. Isso concorda com a validação da porta; não achei inversão dia civil/dia do turno nesse caminho específico. (`app/ponto/turnos.py:2811`, `app/ponto/turnos.py:2642`, `app/ponto/turnos.py:2653`, `app/ponto/services/ausencia.py:1018`)

11. **FILA — inventário de `derivado_em`, trilha e `Justificativa.status`.**

    | Campo/valor | Escritores | Leitores |
    |---|---|---|
    | `FechamentoMensal.derivado_em` | Default NULL: `app/ponto/models.py:464`; recalcular: `app/ponto/services/fechamento.py:1143`, `app/ponto/services/fechamento.py:1189`; restaurar: `app/ponto/services/fechamento.py:1391`; invalidar: `app/ponto/services/fechamento.py:1656`. | Filtro de invalidação: `app/ponto/services/fechamento.py:1653`; vencidas: `app/ponto/services/fechamento.py:1695`, `app/ponto/services/fechamento.py:1696`; contador: `app/ponto/services/fechamento.py:1702`; command: `app/ponto/management/commands/projecoes_vencidas.py:50`; excluído dos hashes em `app/ponto/portas/regen_exportada.py:38` e `app/ponto/management/commands/carimbo_gravado.py:30`. |
    | Evento `rejeitar`/`Ausencia` | Escritor nominal encontrado: `app/ponto/services/ausencia.py:1463`; escritor físico: `app/core/observ.py:134`. Ator/IP vêm 

[truncado em 20000 de 27337 caracteres]

## 16/11 02:09 · sessao 230d82d6cef2 · - · - · origem=Ronald (tmux)

1. **BLOQUEIA — envelope invalida competências sem mudança de insumo.** `_acordar_projecao` reduz os dias de todas as linhas/ambos lados ao mínimo/máximo e invalida o intervalo contínuo (`app/ponto/services/ausencia.py:174`, `:175`, `:178`). Contraexemplo: falta aprovada diária muda de 10/09 para 10/11; a competência intermediária de outubro é invalidada, embora a linha nunca a ocupe. `competencias_do_fato` efetivamente percorre todo o intervalo (`app/ponto/services/fechamento.py:1595`, `:1599`). Incidência em produção: [nao verificado].

2. **Predicado igual para estado; universo diferente.** `_insumo_da_folha` chama o mesmo `_pelo_efeito(...,'folha')` (`app/ponto/services/ausencia.py:138`) que os leitores principais da folha (`app/ponto/services/fechamento.py:639`, `:683`); o predicado seleciona exclusivamente aprovada (`app/ponto/turnos.py:2528`). O caminho de desconto ainda usa literal `status='aprovada'` equivalente (`app/folha/export.py:88`), chamado pelo fechamento (`app/ponto/services/fechamento.py:873`). Não achei caminho que conte ausência não aprovada nesses consumidores (`app/ponto/turnos.py:2528`, `app/folha/export.py:88`).

   O snapshot não filtra período nem resolve sobreposição; uma aprovada fora da janela ou vencida pelo desempate ainda aparece nele (`app/ponto/services/ausencia.py:138`; `app/ponto/turnos.py:2765`, `:2793`). Portanto “mesmo predicado” não prova “mesmo conjunto que contribui ao número” (`app/ponto/services/ausencia.py:138`; `app/ponto/services/fechamento.py:693`).

3. **Campos lidos pela folha.** Tipo/data/fim/minutos estão cobertos por `CAMPOS_DO_INSUMO` (`app/ponto/services/ausencia.py:119`), e são usados na cobertura, parcial e rateio (`app/ponto/turnos.py:2784`, `:2786`, `:2790`, `:2612`; `app/folha/export.py:117`, `:121`). Hora inicial/final chegam pela grade (`app/escala/utils.py:1234`; `app/ponto/turnos.py:2646`). Outros controles lidos: colaborador seleciona o universo; status decide entrada; PK desempata (`app/ponto/turnos.py:2543`, `:2528`, `:2765`). Status está representado pela presença/ausência no dict, PK é a chave, colaborador vem separado (`app/ponto/services/ausencia.py:138`, `:139`, `:142`). Não achei outro campo de fato de Ausencia lido por esses caminhos; censo global completo: [nao verificado].

4. **Semânticas verificadas.** Data/fim são inclusivos na cobertura e expansão (`app/ponto/turnos.py:2792`, `:2811`); fim NULL só prolonga `TIPOS_EM_ABERTO`, nos outros tipos vale um dia (`app/ponto/turnos.py:2803`). Hora final é inclusiva e, menor que inicial, pertence à madrugada seguinte do turno iniciado em `data` (`app/ponto/turnos.py:2642`, `:2650`, `:2653`). Minutos são duração total do registro para falta, rateada pelos dias (`app/folha/export.py:118`, `:121`), e crédito do próprio dia para parcial (`app/ponto/services/credito_parcial.py:27`, `:28`). São usos distintos explicitados pelo tipo/forma do registro (`app/ponto/turnos.py:2612`).

5. **FILA — divergência herdada no leitor de fato novo.** `houve_fato_novo` só considera ausência de fim NULL no próprio dia inicial (`app/ponto/services/cartorio.py:383`, `:384`); cobertura prolonga afastamento em aberto até o fim perguntado (`app/ponto/turnos.py:2786`, `:2787`). Contraexemplo: afastamento de 01/11 sem fim tem `atualizado_em` movido em 05/11; consulta de fato novo para dia 04/11 não o encontra pelo ramo NULL, embora a cobertura encontre (`app/ponto/services/cartorio.py:384`; `app/ponto/turnos.py:2786`). Ocorrência no dado raspado: [nao verificado].

6. **Inventário de leitores adicional, sem garantia de completude global.**
   
   - Status/tipo/data/fim: consultas de período e cobertura (`app/ponto/turnos.py:2543`, `:2566`, `:2765`); precedência (`app/ponto/precedencia.py:273`, `:336`); relatório (`app/relatorios/views.py:197`, `:205`, `:277`, `:292`); inteligência (`app/inteligencia/resumo.py:39`, `:42`, `:43`; `app/inteligencia/views.py:71`, `:239`, `:516`).
   - Minutos: parcial (`app/ponto/turnos.py:2614`), crédito (`app/ponto/services/credito_parcial.py:28`), desconto (`app/folha/export.py:121`), API (`app/api/views.py:1870`).
   - Hora_ini/fim: juiz da parcial (`app/ponto/turnos.py:2646`, `:2648`, `:2649`), chamado pela grade (`app/escala/utils.py:1234`).
   - Tela: espelho lê cobertura cobrança/folha e status aberto (`app/ponto/services/espelho.py:582`, `:588`, `:849`, `:880`); calendário lê tipo/fim (`app/colaboradores/services/calendario.py:516`, `:705`, `:708`); API lê tipo/data/status/minutos (`app/api/views.py:1865`); mensageria lê tipo/status/fim/data (`app/api/views_mensageria.py:744`).
   - Atualizado_em: escritor automático (`app/ponto/models.py:824`); leitor de fato novo (`app/ponto/services/cartorio.py:385`); comando de reversão/caracterização (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:154`, `:156`, `:189`).

7. **FechamentoMensal.derivado_em — escritores/leitores encontrados.** Escritores: recalcular ramo save (`app/ponto/services/fechamento.py:1143`), recalcular ramo update (`:1189`), restaurar (`:1391`, `:1393`), invalidar (`:1656`); default nulo do modelo (`app/ponto/models.py:464`). Leitores: filtro de invalidar (`app/ponto/services/fechamento.py:1653`), consulta de vencidas (`:1695`, `:1696`) e contador (`:1702`); command apresenta o mesmo universo (`app/ponto/management/commands/projecoes_vencidas.py:50`, `:64`), cron registra esse contador (`app/config/crons.py:1165`). Campo excluído dos hashes (`app/ponto/portas/regen_exportada.py:38`; `app/ponto/management/commands/carimbo_gravado.py:30`). Semântica consistente de carimbo da projeção nos sítios encontrados (`app/ponto/services/fechamento.py:1679`).

8. **Justificativa.status — alcance confirmado.** Escritores de decisão/reabertura: aprovação e rejeição na porta (`app/ponto/services/justificativa.py:205`, `:283`, `:383`); lote e rejeição na view (`app/ponto/views.py:1799`, `:1878`); regularização (`app/ponto/services/regularizacao.py:99`, `:106`); triagem e entrada sincronizada (`app/ponto/services/triagem_batida.py:608`; `app/ponto/services/entrada_sincronizada.py:41`). Criações adicionais: API (`app/api/views.py:1999`) e cron (`app/ponto/management/commands/processar_alertas_turno.py:394`).

   Leitores confirmados: folha (`app/ponto/services/fechamento.py:662`), grade (`app/escala/utils.py:1197`), efeito da ausência (`app/ponto/services/efeito_ausencia.py:41`), próprias portas (`app/ponto/services/justificativa.py:33`, `:143`, `:165`, `:278`), chamados (`app/chamados/models.py:138`, `:1313`; `app/chamados/reconciliador.py:453`), telas/contadores (`app/ponto/views.py:1739`, `:1740`, `:1783`; `app/chamados/views.py:599`; `app/core/views.py:205`, `:1191`), SLA (`app/ponto/management/commands/alertar_justificativas_sla.py:19`), API (`app/api/views.py:2106`). Censo completo de aliases e templates: [nao verificado].

9. **Kernel efetivamente muda.** API core importa e chama a mesma regularização (`app/api/views_core.py:1150`, `:1179`). Novo ramo: justificativa existente sem saída, antes aprovada, volta pendente e invalida a competência da entrada (`app/ponto/services/regularizacao.py:102`, `:105`, `:115`, `:116`); há SELECT de competências e UPDATE de cada fechamento fresco (`app/ponto/services/fechamento.py:1648`, `:1652`). O import de fechamento é tardio e seu topo importa stdlib, timezone, modelos e janelas (`app/ponto/services/fechamento.py:8`).

   Dias intermediários também atravessam a porta alterada (`app/ponto/services/regularizacao.py:129` → `app/ponto/services/ausencia.py:593`, `:580`): cada ausência recém-criada ganha leitura de snapshot aprovado (`app/ponto/services/ausencia.py:138`); `_acordar_projecao` importa fechamento e lê `localdate` mesmo quando ambos snapshots são vazios (`app/ponto/services/ausencia.py:160`, `:162`, `:165`). Nesse caso não há invalidação (`app/ponto/services/ausencia.py:176`). Impacto de latência/imports em execução: [nao verificado].

## 16/11 02:07 · sessao ee55d94822d2 · - · - · origem=Ronald (tmux)

1. **[fila] Selo tem lacunas demonstráveis.** Só examina funções de topo (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:55`); isenta receptor desconhecido quando a função importa outro modelo de ponto (`:108`); aceita qualquer chamada textual do acordador, inclusive em `if False` (`:75`); isenta receptor atribuído a dict em qualquer ramo (`:59`, `:101`). Contraexemplo executado em memória: `def _m(a): from ponto.models import Batida; a.status="aprovada"; a.save(update_fields=["status"])` retorna `faltam=[]`, embora possa receber Ausencia e alterar insumo (`:108`).

2. **Mutantes fornecidos mordem na análise AST.** Executei os seis métodos do selo em memória, substituindo somente a infraestrutura Django por `unittest.TestCase` e `_monitorados` pelos campos declarados; passaram, com nove escritores identificados. Remoção do acordador e os cinco escritores novos são efetivamente cobrados (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:157`, `:166`). Isso não prova execução Django nem RED contra base: **[nao verificado]**.

3. **Não achei escritor direto externo atual escapando do gate.** Censo real da árvore atribuiu Ausencia apenas à porta; os sites adicionais não atribuídos dentro dela são cobertos pelo novo selo (`app/ponto/services/ausencia.py:1338`, `:1365`, `:1462`, `:1703`). O gate descarta entidade desconhecida (`app/ponto/tests/test_chokepoint_ausencia_gate.py:33`), portanto seu “allowlist vazia” não constitui prova absoluta. `Ausencia.save` também deriva minutos e data_fim (`app/ponto/models.py:887`, `:917`), mas o censo ignora `super().save()` (`app/core/censo_escritas.py:403`). Admin está efetivamente em leitura (`app/ponto/admin.py:38`; `app/core/admin.py:59`).

4. **[bloqueia, se vendido como prova de célula] Teste “rejulga a célula” só prova chamada do signal.** Mocka `_julgar_dias`, impedindo execução de `dias_fn()` e `cartorio.julgar_celula` (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:364`; `app/ponto/signals.py:92`). A asserção só compara IDs enviados ao mock (`teste:366`). Contraprova: `_julgar_dias` quebrado ou lambda errada continua passando; célula inexistente faria o juiz retornar sem julgar (`app/ponto/services/cartorio.py:2027`).

5. **[fila] Teste negativo da Justificativa passa sem reabertura.** Cria Justificativa rejeitada sem conservar referência, chama regularização e verifica apenas `ok` e carimbo (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:349`, `:354`). Remover o bloco que atualiza Justificativa deixaria essas asserções satisfeitas (`app/ponto/services/regularizacao.py:102`). Falta afirmar `status='pendente'`.

6. **[fila] Cobertura de rollback e views é parcial.** `_nada_mudou` verifica somente agendamento não cancelado e ausência não rejeitada; não verifica saldo, observações ou trilhas (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:412`). Existem testes da view individual e do lote (`:417`, `:426`), mas nenhum da nova captura em `corrigir_concluida_admin` (`app/ferias/views.py:452`).

7. **Relógio e fixtures.** Os testes derivados de `_Base` recebem congelamento (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:53`); `FeriasTelaTrilhaTest` herda diretamente `TestCase`, sem congelamento (`:381`), e seu lote atravessa `datetime.now()` (`app/ferias/views.py:608`). Não há chamada direta a `timezone.now()` no arquivo de teste: **[nao verificado por execução Django]**. As fixtures principais não são vazias: espelho exige quantidade igual aos dias (`teste:198`), rejeição exige cinco linhas (`:210`), e espião envolve a função real com `wraps` (`:85`).

## 16/11 02:07 · sessao e20bf62cf793 · - · - · origem=Ronald (tmux)

1. **Cancelamento de férias: sem bloqueador novo encontrado nesse caminho.** O único chamador produtivo encontrado de `rejeitar_ausencias_de_ferias` é `app/ferias/services.py:683`; as três views passam pela casca `app/ferias/views.py:567`. O escritor externo abre atomic em `app/ferias/services.py:674`, trava agendamento em `:636`, período em `:639`, depois Ausências por PK, limitando a trava à própria tabela, em `app/ponto/services/ausencia.py:1452`.

2. **Signals realmente passam por linha.** Cada `save` em `app/ponto/services/ausencia.py:1462` chama o juiz registrado em `app/ponto/signals.py:143`, que executa `_julgar_dias` em `:147`; este chama `cartorio.julgar_celula` em `:93`, sob tratamento best effort em `:88`. O consumidor registrado depois, em `:275`, agenda reconciliação no `on_commit` em `:272`; portanto o teste com mock de `_julgar_dias` prova o despacho ao juiz, sem provar o resultado real da célula (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:364`).

3. **Falha da trilha desfaz o item inteiro.** `evento` usa savepoint próprio em `app/core/observ.py:133` e devolve `None` após falha em `:139`; a rejeição transforma esse retorno em exceção dentro do atomic em `app/ponto/services/ausencia.py:1470`. A exceção atravessa também o atomic externo, que já tinha escrito o saldo em `app/ferias/services.py:682`, e impede o cancelamento posterior em `:689`. Os callbacks registrados via `on_commit` em `app/ponto/signals.py:272` ficam condicionados ao commit.

4. **As três views tratam a exceção nova.** Individual mostra “Nada foi alterado...” em `app/ferias/views.py:398`; correção de concluída mostra a mesma recusa em `:452`; massa registra IDs falhos em `:623` e mostra quantidade e IDs em `:630`. Na massa, os itens bem sucedidos continuam sendo contados e gravados individualmente (`:617`, `:625`); não existe atomic envolvendo o lote inteiro nesse trecho (`:612`).

5. **No sucesso, as Ausências rejeitadas são depois apagadas.** O serviço grava agendamento cancelado em `app/ferias/services.py:689`; o override sincroniza a Ausência em `app/ferias/models.py:448`; `espelhar_ferias` apaga as próprias linhas do agendamento inativo em `app/ponto/services/ausencia.py:1333`. A trilha da rejeição permanece como registro independente com ID numérico, sem FK para Ausência (`app/core/models.py:298`).

6. **[bloqueia] Um ato `decidir` ainda pode invalidar duas vezes.** Contraexemplo: declaração `aguardando_decisao`, aprovada juntamente com período 08:00–12:00. A transição chama o acordador em `app/ponto/services/ausencia.py:971`, via `decidir` em `:1050`; depois o mesmo ato grava o período em `:1069` e chama outro acordador em `:1087`. O comentário “Um ato = uma invalidação” em `:1052` não cobre essa composição. O teste de aprovação usa ausência sem período em `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:306`; o de período começa já aprovada em `:319`.

7. **[fila, herdado] Há ciclo possível de travas no backfill com vários agendamentos do mesmo período.** O backfill trava agendamentos com JOIN do período por PK em `app/ponto/services/ausencia.py:1637`; o cancelamento trava agendamento antes de período em `app/ferias/services.py:636`. Contraexemplo de interleaving: backfill segura G1 e período P; cancelamento segura G2 e espera P; backfill avança para G2 e espera cancelamento. A existência desse arranjo em produção é [nao verificado]; o JOIN e a ordem diferente entre linhas estão nos trechos citados.

8. **Evento de trilha: escritor e leitores.** O escritor nominal do par `rejeitar`/`Ausencia` encontrado é `app/ponto/services/ausencia.py:1463`, passando pelo escritor físico em `app/core/observ.py:134`; `rejeitar` já integra o catálogo em `app/core/models.py:291`. Leitores genéricos que abrangem esse evento: auditoria em `app/core/views.py:705`; fotografia de trilhas de Ausências em `app/chamados/services/foto_e4b.py:311`; proteção de reversão contra qualquer trilha posterior da Ausência em `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:157`. Leitor especializado desse par: não achei.

9. **`atualizado_em`: significado e lacuna herdada.** O campo é `auto_now` em `app/ponto/models.py:824`; o novo rejeitador o inclui explicitamente em `app/ponto/services/ausencia.py:1462`, e o período também em `:1032`. Saves completos existentes em `:545`, `:642`, `:1716` também passam pelo campo. `mudar_estado` usa lista parcial sem esse campo em `:954`, `:965`, `:967`; a reversão protege-se lendo tanto carimbo quanto trilha em `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:154`. Entretanto `houve_fato_novo` consulta o carimbo, sem consultar essa trilha, em `app/ponto/services/cartorio.py:383`. Contraexemplo: rejeitada com `aprovada_em` antigo → aguardando → aprovada, sem período; a mudança pode não produzir nenhum carimbo novo que esse leitor consulte (`app/ponto/services/ausencia.py:965`; `app/ponto/models.py:882`).

## 16/11 02:07 · sessao 7adcd4b501d2 · ESPELHO-VERDADE-FECHO-V0 · sonda · origem=Code

[nao verificado] No dado fornecido, as seis linhas `SOMA` e as duas linhas `ESTRUTURA_DIAS_LINHAS` são `True`. Não há ocorrência de `INCOMPLETO`.

[nao verificado] Transcrição integral da saída fornecida:

```text
sonda_leitura: script.py [fe38ad6dc95a1ace] no cpuset 4-7, lendo sombra como leitor (LEITURA)
sonda_leitura: arvore=/home/ronald/v0-arvore-3580b35e0 (copia), banco segue sombra como leitor

PARTE 4 PARTES 6
[0] UNIVERSO 10/2026 624
UNIVERSO_PARTE 10/2026 96
JANELA 10/2026 2026-09-21 2026-10-20
[0] UNIVERSO 11/2026 595
UNIVERSO_PARTE 11/2026 94
JANELA 11/2026 2026-10-21 2026-11-20
ESTIMATIVA processados 25 universo 190 minutos_total 2.7 ate_40_min True ate_15_min True
ESTIMATIVA processados 50 universo 190 minutos_total 2.7 ate_40_min True ate_15_min True
ESTIMATIVA processados 75 universo 190 minutos_total 2.7 ate_40_min True ate_15_min True
COMPETENCIA 10/2026
[1] {'dias_comparados': 1234, 'bate_ate_10min': 1186, 'esp_zero_e6_trabalho': 6, 'e6_zero_esp_trabalho': 1, 'diverge_10_60': 5, 'diverge_acima_60': 36, 'dia_batida_impar': 55, 'impar_que_BATE': 21, 'impar_que_DIVERGE': 34, 'impar_dentro_da_faixa': 30, 'impar_FORA_da_faixa': 4, 'impar_no_conjunto_do_dia': 33, 'impar_SO_POR_CORTE_do_oraculo': 1, 'dia_sem_linha_no_espelho': 0, 'dia_sem_lavratura_no_espelho': 1276, 'dia_isento_ou_ausencia': 315, 'dia_sem_trabalho_ambos': 18, 'esp_desconta_janela_declarada': 37, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 96.1% de 1234 dias comparados
[3] ESTRUTURA     3 dia-colab      15.6 h    3 colabs
[3] CADASTRO     23 dia-colab     107.6 h    8 colabs
[3] BATIDA       22 dia-colab     101.2 h   17 colabs
[4] TOTAL divergentes 48
[5] colabs com divergencia: 26
ESTRUTURA_DIA|10/2026|col129|2026-10-12|esp_min=0|e6_min=779|teto_min=779|dif_min=779|classe=esp_zero_e6_trabalho|paridade=par|batidas=17:30 06:30|veredito=em_aberto
ESTRUTURA_DIA|10/2026|col369|2026-10-12|esp_min=476|e6_min=416|teto_min=476|dif_min=-60|classe=diverge_10_60|paridade=par|batidas=07:04 12:47 13:46 15:00|veredito=feriado
ESTRUTURA_DIA|10/2026|col723|2026-10-08|esp_min=440|e6_min=345|teto_min=500|dif_min=-95|classe=diverge_acima_60|paridade=par|batidas=09:00 10:44 13:20 17:20|veredito=trabalhou
ESTRUTURA_DIAS_LINHAS 10/2026 3 True
erros_no_espelho 0
PARCIAL|10/2026|ESTRUTURA|dias=3|horas=15.566319135833336|colabs=3
PARCIAL|10/2026|CADASTRO|dias=23|horas=107.60888681222221|colabs=8
PARCIAL|10/2026|BATIDA|dias=22|horas=101.15787729194444|colabs=17
PARCIAL_TOTAL|10/2026|dias=48|horas=224.333083239999986|colabs=26
ESTIMATIVA processados 100 universo 190 minutos_total 2.7 ate_40_min True ate_15_min True
ESTIMATIVA processados 125 universo 190 minutos_total 2.7 ate_40_min True ate_15_min True
ESTIMATIVA processados 150 universo 190 minutos_total 2.8 ate_40_min True ate_15_min True
ESTIMATIVA processados 175 universo 190 minutos_total 2.8 ate_40_min True ate_15_min True
COMPETENCIA 11/2026
[1] {'dias_comparados': 983, 'bate_ate_10min': 951, 'esp_zero_e6_trabalho': 7, 'e6_zero_esp_trabalho': 0, 'diverge_10_60': 0, 'diverge_acima_60': 25, 'dia_batida_impar': 50, 'impar_que_BATE': 25, 'impar_que_DIVERGE': 25, 'impar_dentro_da_faixa': 21, 'impar_FORA_da_faixa': 4, 'impar_no_conjunto_do_dia': 24, 'impar_SO_POR_CORTE_do_oraculo': 1, 'dia_sem_linha_no_espelho': 376, 'dia_sem_lavratura_no_espelho': 1229, 'dia_isento_ou_ausencia': 264, 'dia_sem_trabalho_ambos': 34, 'esp_desconta_janela_declarada': 28, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 96.7% de 983 dias comparados
[3] ESTRUTURA     1 dia-colab       9.8 h    1 colabs
[3] CADASTRO      4 dia-colab      17.1 h    3 colabs
[3] BATIDA       27 dia-colab     160.9 h   14 colabs
[4] TOTAL divergentes 32
[5] colabs com divergencia: 16
ESTRUTURA_DIA|11/2026|col909|2026-11-11|esp_min=831|e6_min=245|teto_min=374|dif_min=-586|classe=diverge_acima_60/FORA|paridade=IMPAR|batidas=07:00 11:04 13:13 23:00|veredito=em_aberto
ESTRUTURA_DIAS_LINHAS 11/2026 1 True
erros_no_espelho 0
PARCIAL|11/2026|ESTRUTURA|dias=1|horas=9.770812222222222|colabs=1
PARCIAL|11/2026|CADASTRO|dias=4|horas=17.068988648888887|colabs=3
PARCIAL|11/2026|BATIDA|dias=27|horas=160.93444128833335|colabs=14
PARCIAL_TOTAL|11/2026|dias=32|horas=187.774242159444459|colabs=16
TEMPO_SEGUNDOS 165.0
TRATADA_LOCAL app/escala/alimentacao.py:279 GeneratorExit 239769
TRATADA_LOCAL app/escala/models.py:1282 GeneratorExit 637
TRATADA_LOCAL app/escala/services/fase_12x36.py:114 GeneratorExit 1276
TRATADA_LOCAL app/escala/services/leitor_celula.py:25 ValueError 6019
TRATADA_LOCAL app/escala/services/leitor_celula.py:81 GeneratorExit 50
TRATADA_LOCAL app/escala/utils.py:1500 GeneratorExit 1
TRATADA_LOCAL app/escala/utils.py:1687 GeneratorExit 36
TRATADA_LOCAL app/escala/utils.py:1991 GeneratorExit 6962
TRATADA_LOCAL app/escala/utils.py:2033 GeneratorExit 8
TRATADA_LOCAL app/escala/utils.py:452 GeneratorExit 3
TRATADA_LOCAL app/escala/utils.py:524 GeneratorExit 3790
TRATADA_LOCAL app/escala/utils.py:525 GeneratorExit 3790
TRATADA_LOCAL app/escala/utils.py:888 GeneratorExit 334
TRATADA_LOCAL app/folha/causas.py:58 GeneratorExit 171
TRATADA_LOCAL app/ponto/juiz_batida.py:191 GeneratorExit 1226
TRATADA_LOCAL app/ponto/juiz_batida.py:75 ValueError 445946
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:1131 GeneratorExit 649
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:2604 GeneratorExit 2833
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:677 GeneratorExit 186
TRATADA_LOCAL app/ponto/services/dia_pago.py:892 GeneratorExit 12772
TRATADA_LOCAL app/ponto/services/espelho.py:473 GeneratorExit 113
TRATADA_LOCAL app/ponto/services/espelho.py:657 GeneratorExit 4030
TRATADA_LOCAL app/ponto/services/fechamento.py:1044 GeneratorExit 180
TRATADA_LOCAL app/ponto/turnos.py:1295 GeneratorExit 24689
TRATADA_LOCAL app/ponto/turnos.py:1656 GeneratorExit 3726
TRATADA_LOCAL app/ponto/turnos.py:1699 GeneratorExit 39600
TRATADA_LOCAL app/ponto/turnos.py:679 GeneratorExit 18892
SOMA 10/2026 48   <- tem de ser igual a soma das classes (48) True
SOMA 10/2026 HORAS 224.333083239999986 True
SOMA 10/2026 COLABS_UNIAO 26 True
SOMA 11/2026 32   <- tem de ser igual a soma das classes (32) True
SOMA 11/2026 HORAS 187.774242159444459 True
SOMA 11/2026 COLABS_UNIAO 16 True
rc_sombra=0
```

## 16/11 01:57 · sessao 42ddca326818 · LOTE3-C6 · verificador · origem=Code

Os dois achados anteriores estão fechados para a corrida descrita: a guarda INSS antecede a ausência, e a recusa HTMX de posto relê o banco. Entretanto, a nova transação deixa uma lacuna na limpeza do `documento` quando ocorre falha depois de `criar_ausencia` retornar. (`app/ponto/views.py:2463`; `app/ponto/views.py:2465`; `app/colaboradores/views.py:339`; `app/ponto/services/ausencia.py:480`.)

Parecer por leitura estática. Execução dos testes, RED/GREEN informado, implantação e ocorrências em produção: **[nao verificado]**. [nao verificado]

**1. Fechamento dos dois achados**

**Ausência INSS:** a pré-conferência trava o colaborador e consulta sua situação gravada antes de chamar `criar_ausencia`. Se encontrar desligado, levanta `DesligadoSemReadmissao` antes da primeira escrita da ausência; a resposta específica fica fora do `atomic`. (`app/colaboradores/services/colaborador.py:84`; `app/colaboradores/services/colaborador.py:94`; `app/colaboradores/services/colaborador.py:65`; `app/ponto/views.py:2465`; `app/ponto/views.py:2486`.)

Nessa recusa inicial:

| Efeito perguntado | Resultado pelo código |
|---|---|
| Ausência | Não chega ao serviço nem ao `save`. (`app/ponto/views.py:2465`; `app/ponto/views.py:2466`.) |
| `documento` definitivo | Não chega ao `FileField.save`; a normalização anterior apenas ajusta o nome do upload. (`app/ponto/services/ausencia.py:650`; `app/ponto/services/ausencia.py:461`.) |
| `LogAuditoria` da ausência | Não alcança a chamada de criação da trilha. (`app/ponto/services/ausencia.py:502`.) |
| Chamado/pergunta decorrentes do `post_save` | O sinal não é disparado, pois não houve `save` da ausência. (`app/ponto/services/ausencia.py:479`; `app/ponto/signals.py:143`.) |
| Consumidor `on_commit` da ausência | Não chega ao agendamento; o registro da trava pertence à transação desfeita. (`app/ponto/signals.py:272`; `app/chamados/services/trava_do_colaborador.py:12`.) |

**A garantia não cobre todo erro posterior.** Com documento: `a.save()` pode terminar com sucesso, o serviço retornar e o `_gravar_colab` falhar. O `atomic` externo desfaz ausência e escritas relacionadas no banco, mas não reentra no `except` que limpa o arquivo. Esse é o achado novo detalhado ao fim; ocorrência concreta em produção permanece **[nao verificado]**. (`app/ponto/services/ausencia.py:476`; `app/ponto/services/ausencia.py:480`; `app/ponto/services/ausencia.py:512`; `app/ponto/views.py:2483`.)

Também continuam possíveis respostas genéricas de erro **depois do commit**, por exemplo na consulta dos alertas: essas falhas não equivalem à recusa da guarda e não desfazem o lançamento já confirmado. (`app/ponto/views.py:2497`; `app/ponto/views.py:2499`; `app/ponto/views.py:2511`.)

**Posto:** a guarda de `salvar_inteiro` recusa antes do `save`; o histórico só é chamado depois. Na exceção HTMX, o contexto recebe o colaborador relido, e o template apresenta seu posto. O objeto alterado anteriormente em memória deixou de alimentar essa resposta. (`app/colaboradores/services/colaborador.py:158`; `app/colaboradores/views.py:329`; `app/colaboradores/views.py:331`; `app/colaboradores/views.py:339`; `app/templates/colaboradores/partials/vinculo_row.html:16`.)

Falha do histórico após gravação continua deixando o posto novo persistido; agora a linha mostra corretamente esse posto. Esse cenário já existia e é explicitamente caracterizado pelo quinto teste, não sendo uma recusa `DesligadoSemReadmissao`. (`app/colaboradores/views.py:329`; `app/colaboradores/views.py:331`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:154`.)

**2. Sequência de travas e sinais**

A sequência depende dos dias, células e chamados existentes; não há uma lista única de queries válida para todo lançamento. O encadeamento verificável é este. (`app/ponto/signals.py:92`; `app/ponto/services/cartorio.py:2027`; `app/ponto/services/cartorio.py:1000`; `app/ponto/services/cartorio.py:1029`.)

1. **Antes do novo bloco:** carga filtrada do colaborador e leitura da data do POST, sem trava da linha nessa carga. (`app/ponto/views.py:2442`; `app/ponto/views.py:2445`.)
2. **Primeira query de trava, para INSS:** `Colaborador FOR UPDATE`, pela porta global; registro do colaborador travado; depois leitura simples da situação gravada para a guarda. A chamada inicial não solicita Disputa, Chamado ou Pergunta. (`app/colaboradores/services/colaborador.py:90`; `app/chamados/models.py:2744`; `app/chamados/models.py:2746`; `app/colaboradores/services/colaborador.py:65`.)
3. **Serviço:** consultas de tipo/sobreposição/precedência e validação; construção da ausência; `a.save()` em `atomic` interno, agora um savepoint do externo. Se houver rejeitada correspondente, existe também o ramo de reabertura da linha existente. (`app/ponto/services/ausencia.py:309`; `app/ponto/services/ausencia.py:350`; `app/ponto/services/ausencia.py:368`; `app/ponto/services/ausencia.py:438`; `app/ponto/services/ausencia.py:448`; `app/ponto/services/ausencia.py:478`.)
4. **`post_save` síncrono:** cartório calcula os dias, consulta as células e julga. Pode escrever célula e `EventoCelula`, reconciliar chamados/perguntas e fabricar disputa/perguntas. Essas escritas continuam dentro da transação externa. (`app/ponto/signals.py:115`; `app/ponto/signals.py:147`; `app/ponto/services/cartorio.py:2027`; `app/ponto/services/cartorio.py:1006`; `app/ponto/services/cartorio.py:1045`; `app/ponto/portas/celula.py:962`.)
5. **Travas C/D/CH/P desses ramos:** são adquiridas pela porta global, que ordena cada chamada como Colaborador → Disputa → Chamado → Pergunta, por PK; Pergunta usa `of=('self',)`. (`app/chamados/models.py:2744`; `app/chamados/models.py:2753`; `app/chamados/models.py:2766`; `app/chamados/models.py:2770`.)
6. **Outras travas condicionais:** fechamento com entrega de recusas ao DP pode adquirir `ContadorPauta FOR UPDATE` e travas consultivas de competência. Portanto, **há `select_for_update` fora da porta global no caminho ampliado**, sobre outra entidade; isso não constitui escritor alternativo de trava C/D/CH/P. (`app/ponto/services/cartorio.py:1011`; `app/chamados/services/fecho_runtime.py:168`; `app/chamados/services/fecho_runtime.py:131`; `app/pautas/services.py:351`; `app/chamados/services/fecho_runtime.py:92`.)
7. **Após o savepoint da ausência:** criação da auditoria; guarda final de situação, que reencontra o mesmo Colaborador já travado; atribuição e `UPDATE` de `afastado`; saída do `atomic` externo. (`app/ponto/services/ausencia.py:502`; `app/ponto/views.py:2483`; `app/colaboradores/services/colaborador.py:133`; `app/colaboradores/services/colaborador.py:136`.)
8. **Depois do commit:** execução dos callbacks agendados, incluindo consumidor da ausência e, condicionalmente, callbacks decorrentes de Pergunta e Colaborador isento. Esses callbacks não executam sob a trava original já liberada. (`app/ponto/signals.py:272`; `app/chamados/signals.py:240`; `app/chamados/signals.py:281`; `app/chamados/services/trava_do_colaborador.py:12`.)

**Ressalva sobre a ordem global:** cada chamada da porta ordena suas tabelas, mas o ato inteiro pode voltar a adquirir outras linhas depois de já ter adquirido Pergunta. Exemplo: desfazer carimbo trava C/D/P; em seguida, a fábrica pode chamar a emissão de disputa, que trava o universo C/D/CH. Essa composição já existia dentro do `atomic` de `a.save()`. O c6 garante C primeiro e torna sua aquisição final reentrante; não demonstra uma aquisição única e monotônica de todo o universo D/CH/P. Deadlock novo: **[nao verificado]**. (`app/chamados/services/relavra_via.py:230`; `app/chamados/services/fabrica_da_celula.py:169`; `app/chamados/services/fabrica_da_celula.py:172`; `app/chamados/services/disputa_emissao.py:539`; `app/ponto/services/ausencia.py:478`.)

**3. O que mudou para os leitores durante o ato**

| Leitor/momento | Resultado |
|---|---|
| Cartório síncrono | Continua recebendo `instance.colaborador` antes da atribuição de `afastado`. A pré-conferência não atualiza esse objeto em memória. (`app/ponto/signals.py:147`; `app/colaboradores/services/colaborador.py:102`; `app/ponto/views.py:2483`.) |
| `afastado_na_batida` no cartório | Continua consultando primeiro a ausência por `afastado_hoje`; só depois consulta o campo como divergência. A ausência inserida está disponível à própria transação. (`app/ponto/services/cartorio.py:1351`; `app/ponto/services/afastado_avisa.py:38`; `app/ponto/services/afastado_avisa.py:41`.) |
| Banco antes do `gravar` final | A situação ainda não recebeu o `afastado` deste lançamento; a nova guarda apenas trava e lê. (`app/colaboradores/services/colaborador.py:84`; `app/ponto/views.py:2483`.) |
| Consumidor `on_commit` | Antes podia executar ao sair do `atomic` interno, antes da escrita de situação e da auditoria de criação. Agora executa depois de ambas; relê o colaborador do banco. (`app/ponto/services/ausencia.py:478`; `app/ponto/services/ausencia.py:502`; `app/ponto/signals.py:267`; `app/ponto/signals.py:272`; `app/ponto/views.py:2483`.) |

**Não-INSS também mudou:** o `atomic` externo é incondicional. Não há pré-guarda nem escrita de situação nesse ramo, mas a auditoria de criação passa a integrar a transação do nascimento e anteceder o consumidor `on_commit`. A reabertura também passa a ficar envolvida pelo novo bloco. Portanto, não cabe afirmar comportamento transacional idêntico para os demais tipos. (`app/ponto/views.py:2463`; `app/ponto/views.py:2464`; `app/ponto/services/ausencia.py:350`; `app/ponto/services/ausencia.py:502`.)

**4. Checklist das classes e dos cinco testes** [nao verificado]

| Classe | Avaliação |
|---|---|
| Segundo juiz | Não identificado: a função nova reutiliza `_guarda_da_linha`, que consulta `esta_desligado`, registrado no vivo. (`app/colaboradores/services/colaborador.py:102`; `app/colaboradores/services/colaborador.py:62`; `app/core/juizes.py:271`.) |
| Segundo escritor fora da porta | Não introduzido: situação continua em `gravar`; posto continua em `salvar_inteiro`. (`app/ponto/views.py:2483`; `app/colaboradores/views.py:329`.) |
| Relógio fora da borda | O código acrescentado não lê relógio. Permanecem leituras profundas já existentes, inclusive no cartório e em `afastado_hoje`; não houve unificação temporal nesta volta. (`app/colaboradores/services/colaborador.py:94`; `app/ponto/services/cartorio.py:2016`; `app/ponto/services/cartorio.py:2040`; `app/ponto/turnos.py:2742`.) |
| Derivação paralela | Não nasceu nova derivação de afastamento. O registro vivo já declara legítima a escrita pela porta, condicionando leitores ao vigia. (`app/core/juizes.py:455`.) |
| Pouso antes do pré-requisito | Na corrida INSS, a guarda agora precede ausência e sinais. No posto, o histórico continua posterior à gravação, com a limitação descrita acima. (`app/ponto/views.py:2465`; `app/colaboradores/views.py:331`.) |
| Campo com dois significados | O c6 não acrescenta significado a `situacao`; conserva a coexistência entre estado cadastral e afastamento derivado, já tratada pelo registro vivo e pelo detector de divergência. (`app/core/juizes.py:455`; `app/ponto/services/afastado_avisa.py:39`.) |

| Teste | Mordida e limite | [nao verificado]
|---|---|
| 1 — desligamento durante lançamento | Exige recusa dita, ausência zero e situação desligado; morde o achado original. Não verifica documento, auditoria ou callbacks. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:77`.) |
| 2 — trava antes do INSERT | Exige existência de FOR UPDATE/INSERT/UPDATE e compara trava antes do INSERT. Não compara a posição do UPDATE nem o commit. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:92`.) |
| 3 — sobreposição | **Pode passar vazio quanto à recusa específica:** a preparação já deixa uma ausência e situação ativa; uma saída antecipada diferente de 500 preserva todos os resultados exigidos. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:104`.) |
| 4 — posto recusado | Exige posto antigo no banco e HTML, ausência do novo nome e histórico zero; morde o segundo achado. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:135`.) |
| 5 — histórico falha depois | Caracteriza a gravação consumada e impede restaurar cegamente o posto antigo. Não precisa reprovar o comportamento anterior. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:154`.) |

Os dois ganchos representam janelas reais de objeto carregado antes da trava: parse da data após carregar o colaborador; retorno de `get_object_or_404` antes da porta do posto. Entretanto, usam UPDATE na mesma conexão de `TestCase`; não demonstram concorrência entre conexões, duração da trava até commit nem execução dos consumidores pós-commit. (`app/ponto/views.py:2442`; `app/ponto/views.py:2445`; `app/colaboradores/views.py:319`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:37`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:140`; `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:43`.)

**5. Quem escreve e quem lê — caminho afetado**

| Campo/valor | Escritor | Leitores relevantes |
|---|---|---|
| `Colaborador.situacao` | View solicita; `gravar` atribui/salva. Desligamento, readmissão e reversor têm seus atos/portas. (`app/ponto/views.py:2483`; `app/colaboradores/services/colaborador.py:126`; `app/colaboradores/services/desligamento.py:128`; `app/colaboradores/services/desligamento.py:241`; `app/ponto/management/commands/reverter_situacao_afastado.py:73`.) | Lista, guarda da linha gravada, consumidor, aviso de batida e badges. (`app/ponto/views.py:2405`; `app/colaboradores/services/colaborador.py:65`; `app/ponto/signals.py:267`; `app/ponto/services/afastado_avisa.py:41`; `app/colaboradores/services/situacao_exibida.py:50`.) |
| `Colaborador.posto` | View atribui; porta salva. (`app/colaboradores/views.py:327`; `app/colaboradores/services/colaborador.py:152`.) | Histórico, releitura da recusa e template. (`app/colaboradores/views.py:326`; `app/colaboradores/views.py:339`; `app/templates/colaboradores/partials/vinculo_row.html:16`.) |
| Ausência | Serviço constrói/salva; pode reabrir rejeitada pela mesma família de portas. (`app/ponto/services/ausencia.py:446`; `app/ponto/services/ausencia.py:479`; `app/ponto/services/ausencia.py:350`.) | Cartório, consumidor e autoridades de cobertura/afastamento. (`app/ponto/signals.py:143`; `app/ponto/signals.py:275`; `app/ponto/turnos.py:2719`; `app/ponto/turnos.py:2747`.) |
| Auditoria da ausência | Serviço chama `evento`; escritor material é `core.observ`. (`app/ponto/services/ausencia.py:502`; `app/core/observ.py:134`.) | Tela de auditoria e proteção de reversão por trilha posterior. (`app/core/views.py:705`; `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:152`.) |
| Registro de colaboradores travados | `trava_na_ordem` chama `_registrar_travados`, que acrescenta sentinela à fila da conexão. (`app/chamados/models.py:2746`; `app/chamados/services/trava_do_colaborador.py:37`.) | `colaboradores_travados` e selo `exigir_colaboradores_travados`. (`app/chamados/services/trava_do_colaborador.py:29`; `app/chamados/services/trava_do_colaborador.py:55`.) |

`JUIZES.md` é lookup e manda prevalecer o registro vivo: cobertura permanece em `turnos`, estado aberto em `ABERTAS`, desligamento em `esta_desligado`. O número histórico de 104 leitores não constitui contagem atual revalidada nesta análise. (`app/docs/JUIZES.md:10`; `app/docs/JUIZES.md:55`; `app/core/juizes.py:271`; `app/core/juizes.py:470`.)

Lista final de achados:

- **PROCEDENTE — documento fora da nova fronteira de rollback.** Falha posterior ao retorno de `criar_ausencia`, especialmente na gravação final do colaborador, desfaz o banco sem executar a limpeza do arquivo. A limpeza protege somente o bloco interno do save; falta cobertura da transação ampliada. (`app/ponto/services/ausencia.py:476`; `app/ponto/services/ausencia.py:480`; `app/ponto/views.py:2483`.)
- **Observação — selos incompletos:** terceiro teste admite saída vazia; segundo não prova retenção até commit; os cinco não demonstram limpeza do anexo nem efeitos pós-commit. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:77`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:92`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:104`.)
- **Observação — ordem global composta:** aquisição por chamada respeita a porta; monotonicidade de todas as aquisições da transação inteira não está demonstrada, e existem travas adicionais de contador/competência. (`app/chamados/services/relavra_via.py:230`; `app/chamados/services/disputa_emissao.py:539`; `app/chamados/services/fecho_runtime.py:131`.)
- **Observação — comentário desatualizado:** “lei em aberto” diverge da autorização já registrada no vivo. (`app/ponto/views.py:2474`; `app/core/juizes.py:455`.)
- **Observação — dos dois achados anteriores, 0 procedente remanescente na corrida descrita.** (`app/ponto/views.py:2465`; `app/colaboradores/views.py:339`.)

## 16/11 01:54 · sessao 6e6d5efb54dc · - · - · origem=Ronald (tmux)

Evidências para consolidar, por leitura estática; execução, deadlock real e estado de produção [nao verificado].

- A primeira query de trava do novo bloco INSS é `Colaborador ... FOR UPDATE`, via `conferir_situacao_antes_do_ato → _guarda_da_linha → _travar → PerguntaDisputa.trava_na_ordem`. Depois registra o sentinela e lê a situação gravada; a pré-conferência não atualiza o objeto `colab` em memória. (`app/ponto/views.py:2463`, `app/colaboradores/services/colaborador.py:84`, `app/colaboradores/services/colaborador.py:94`, `app/chamados/models.py:2744`, `app/chamados/models.py:2746`, `app/colaboradores/services/colaborador.py:65`.)

- Seguem validações de `criar_ausencia`, criação da instância e `a.save()` em savepoint; esse save dispara cartório síncrono, que recebe `instance.colaborador`. Logo, o cartório ainda recebe o objeto carregado antes da trava, e roda antes de `situacao='afastado'` ser atribuída ou gravada pela porta final. Isso já era assim no caminho anterior; a pré-guarda nova não muda esse valor em memória. (`app/ponto/services/ausencia.py:446`, `app/ponto/services/ausencia.py:478`, `app/ponto/signals.py:143`, `app/ponto/signals.py:147`, `app/ponto/views.py:2483`, `app/colaboradores/services/colaborador.py:134`.)

- O cartório seleciona dias, consulta a célula e julga. Pode escrever `CelulaDia` e `EventoCelula`; reconciliar chamados/perguntas; abrir chamado; espelhar marcos; e fabricar disputa/perguntas. Portanto não existe uma sequência fixa de SQL igual para todos os lançamentos. (`app/ponto/signals.py:115`, `app/ponto/signals.py:122`, `app/ponto/signals.py:127`, `app/ponto/services/cartorio.py:2027`, `app/ponto/services/cartorio.py:2040`, `app/ponto/portas/celula.py:962`, `app/ponto/services/cartorio.py:1006`, `app/ponto/services/cartorio.py:1011`, `app/ponto/services/cartorio.py:1045`, `app/ponto/services/cartorio.py:1791`, `app/ponto/services/cartorio.py:1840`.)

- As travas explícitas das quatro classes ficam na porta global: C por PK; D por PK; CH por PK; P por PK, usando `of=('self',)` para P. O final `_gravar_colab` volta à guarda C, mas a linha C já está retida pelo atomic externo, portanto essa aquisição é reentrante. (`app/chamados/models.py:2744`, `app/chamados/models.py:2753`, `app/chamados/models.py:2766`, `app/chamados/models.py:2770`, `app/ponto/views.py:2483`, `app/colaboradores/services/colaborador.py:133`.)

- **Ressalva sobre “ordem global”**: a pré-conferência trava somente C; não reúne antecipadamente todas as D/CH/P dos dias da ausência. O cartório percorre dias e ramos, cujas portas podem repetir aquisições. Por exemplo, `desfazer_carimbo_sem_lastro` trava C/D/P, e a fábrica pode em seguida chamar a porta da disputa, que trava o universo C/D/CH. Não é justificável declarar uma única sequência monotônica C→D→CH→P para toda a transação sem distinguir aquisição nova e reentrância. C retida serializa os escritores que seguem o contrato; um ciclo real permanece [nao verificado]. Essa composição já existia dentro do atomic de `a.save()`, antes do c6. (`app/ponto/signals.py:92`, `app/chamados/services/fabrica_da_celula.py:208`, `app/chamados/services/fabrica_da_celula.py:219`, `app/chamados/services/relavra_via.py:230`, `app/chamados/services/fabrica_da_celula.py:169`, `app/chamados/services/fabrica_da_celula.py:172`, `app/chamados/services/disputa_emissao.py:539`, `app/ponto/services/ausencia.py:478`.)

- Há também travas **implícitas** por escrita em Ausência, célula, evento, log e eventualmente Pauta/ContadorPauta. Nos arquivos percorridos não encontrei `select_for_update` executado fora da porta global para C/D/CH/P. Os `select_for_update` de `CelulaDia` e `Ausencia` em edição/troca de plantão pertencem a outras operações, não chamadas pelo fluxo de lançamento examinado. (`app/ponto/services/ausencia.py:479`, `app/ponto/portas/celula.py:963`, `app/ponto/portas/celula.py:954`, `app/core/observ.py:134`, `app/chamados/services/fabrica_da_celula.py:369`, `app/pautas/services.py:365`, `app/ponto/portas/celula.py:631`, `app/ponto/portas/celula.py:695`.)

- O consumidor da Ausência **não roda dentro do novo atomic**: o `post_save` agenda `_depois` por `on_commit`. Antes, sem transação envolvente, ele rodava ao sair do atomic interno de `a.save()`, antes do evento `criar/Ausencia` e antes da escrita de situação. Agora roda depois de ambos, ao confirmar o atomic externo. Ele relê Colaborador do banco, então no INSS feliz passa a receber `afastado`. (`app/ponto/signals.py:250`, `app/ponto/signals.py:267`, `app/ponto/signals.py:269`, `app/ponto/signals.py:272`, `app/ponto/services/ausencia.py:478`, `app/ponto/services/ausencia.py:502`, `app/ponto/views.py:2463`, `app/ponto/views.py:2483`.)

- **Também muda para não-INSS**: o atomic externo é incondicional. Seu log `criar/Ausencia` agora integra a transação do nascimento e precede o consumidor; antes, o consumidor rodava antes desse log. Não identifiquei leitor desse log de criação no consumidor percorrido: ele consulta cobertura de Ausência e veredito da célula. A trilha consultada pelo C6 é de `retratar_pergunta_obsoleta/PerguntaDisputa`, outro universo. (`app/ponto/views.py:2463`, `app/ponto/services/ausencia.py:502`, `app/ponto/signals.py:272`, `app/chamados/services/silencio_ausencia.py:66`, `app/chamados/services/silencio_ausencia.py:41`, `app/chamados/juizes.py:294`.)

- `afastado_na_batida(c)` durante o cartório continua vendo o valor antigo de `c.situacao`, mas primeiro consulta a ausência recém-inserida por `afastado_hoje`; só usa o cadastro como divergência quando o juiz não encontra cobertura de hoje. A chamada continua antes da escrita final de situação. (`app/ponto/services/cartorio.py:1351`, `app/ponto/services/afastado_avisa.py:38`, `app/ponto/services/afastado_avisa.py:41`, `app/ponto/views.py:2483`.)

- **Observação de anexo para investigação do pai:** a limpeza do arquivo cobre somente exceções que saem do bloco de `a.save()`. Depois que `criar_ausencia` retorna, uma falha no `_gravar_colab` desfaz a transação externa, mas não reentra nessa limpeza. O log é best effort e normalmente não propaga falha. Na corrida específica de desligamento, a recusa inicial antecede `criar_ausencia`, portanto não chega à gravação do documento. (`app/ponto/services/ausencia.py:476`, `app/ponto/services/ausencia.py:480`, `app/ponto/services/ausencia.py:512`, `app/ponto/views.py:2465`, `app/ponto/views.py:2483`, `app/core/observ.py:105`, `app/core/observ.py:133`.)

Nenhum defeito novo de ordem de trava demonstrado neste levantamento; a afirmação irrestrita de sequência global única merece a ressalva acima. (`app/ponto/views.py:2465`, `app/chamados/services/relavra_via.py:230`, `app/chamados/services/disputa_emissao.py:539`.)

## 16/11 01:53 · sessao a24bd99dd2b1 · ESPELHO-VERDADE-FECHO-V0 · sonda · origem=Code

Transcrição integral do dado fornecido, sem verificação independente. [nao verificado]

```text
sonda_leitura: script.py [82c88c2988af3846] no cpuset 4-7, lendo sombra como leitor (LEITURA)
sonda_leitura: arvore=/home/ronald/v0-arvore-3580b35e0 (copia), banco segue sombra como leitor

PARTE 3 PARTES 6
[0] UNIVERSO 10/2026 624
UNIVERSO_PARTE 10/2026 102
JANELA 10/2026 2026-09-21 2026-10-20
[0] UNIVERSO 11/2026 595
UNIVERSO_PARTE 11/2026 99
JANELA 11/2026 2026-10-21 2026-11-20
ESTIMATIVA processados 25 universo 201 minutos_total 3.3 ate_40_min True ate_15_min True
ESTIMATIVA processados 50 universo 201 minutos_total 3.0 ate_40_min True ate_15_min True
ESTIMATIVA processados 75 universo 201 minutos_total 2.9 ate_40_min True ate_15_min True
ESTIMATIVA processados 100 universo 201 minutos_total 2.8 ate_40_min True ate_15_min True
COMPETENCIA 10/2026
[1] {'dias_comparados': 1236, 'bate_ate_10min': 1169, 'esp_zero_e6_trabalho': 12, 'e6_zero_esp_trabalho': 6, 'diverge_10_60': 2, 'diverge_acima_60': 47, 'dia_batida_impar': 65, 'impar_que_BATE': 23, 'impar_que_DIVERGE': 42, 'impar_dentro_da_faixa': 33, 'impar_FORA_da_faixa': 9, 'impar_no_conjunto_do_dia': 42, 'impar_SO_POR_CORTE_do_oraculo': 0, 'dia_sem_linha_no_espelho': 0, 'dia_sem_lavratura_no_espelho': 1422, 'dia_isento_ou_ausencia': 342, 'dia_sem_trabalho_ambos': 22, 'esp_desconta_janela_declarada': 38, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 94.6% de 1236 dias comparados
[3] ESTRUTURA     3 dia-colab      15.2 h    3 colabs
[3] CADASTRO      7 dia-colab      35.9 h    4 colabs
[3] BATIDA       57 dia-colab     297.7 h   24 colabs
[4] TOTAL divergentes 67
[5] colabs com divergencia: 29
ESTRUTURA_DIA|10/2026|col218|2026-10-15|esp_min=105|e6_min=0|teto_min=0|dif_min=-105|classe=e6_zero_esp_trabalho|paridade=par|batidas=|veredito=em_aberto
ESTRUTURA_DIA|10/2026|col746|2026-09-24|esp_min=289|e6_min=193|teto_min=350|dif_min=-96|classe=diverge_acima_60|paridade=par|batidas=11:00 13:02 15:39 16:49|veredito=trabalhou
ESTRUTURA_DIA|10/2026|col854|2026-10-04|esp_min=718|e6_min=7|teto_min=7|dif_min=-711|classe=diverge_acima_60|paridade=par|batidas=18:54 19:01|veredito=trabalhou
ESTRUTURA_DIAS_LINHAS 10/2026 3 True
erros_no_espelho 0
PARCIAL|10/2026|ESTRUTURA|dias=3|horas=15.209666666666665|colabs=3
PARCIAL|10/2026|CADASTRO|dias=7|horas=35.88209174|colabs=4
PARCIAL|10/2026|BATIDA|dias=57|horas=297.66026132194446|colabs=24
PARCIAL_TOTAL|10/2026|dias=67|horas=348.752019728611125|colabs=29
ESTIMATIVA processados 125 universo 201 minutos_total 2.9 ate_40_min True ate_15_min True
ESTIMATIVA processados 150 universo 201 minutos_total 3.0 ate_40_min True ate_15_min True
ESTIMATIVA processados 175 universo 201 minutos_total 2.9 ate_40_min True ate_15_min True
ESTIMATIVA processados 200 universo 201 minutos_total 2.9 ate_40_min True ate_15_min True
COMPETENCIA 11/2026
[1] {'dias_comparados': 1059, 'bate_ate_10min': 1004, 'esp_zero_e6_trabalho': 8, 'e6_zero_esp_trabalho': 7, 'diverge_10_60': 2, 'diverge_acima_60': 38, 'dia_batida_impar': 51, 'impar_que_BATE': 16, 'impar_que_DIVERGE': 35, 'impar_dentro_da_faixa': 28, 'impar_FORA_da_faixa': 7, 'impar_no_conjunto_do_dia': 33, 'impar_SO_POR_CORTE_do_oraculo': 2, 'dia_sem_linha_no_espelho': 396, 'dia_sem_lavratura_no_espelho': 1334, 'dia_isento_ou_ausencia': 205, 'dia_sem_trabalho_ambos': 26, 'esp_desconta_janela_declarada': 49, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 94.8% de 1059 dias comparados
[3] ESTRUTURA     6 dia-colab      17.8 h    5 colabs
[3] CADASTRO      9 dia-colab      32.9 h    6 colabs
[3] BATIDA       40 dia-colab     194.4 h   21 colabs
[4] TOTAL divergentes 55
[5] colabs com divergencia: 27
ESTRUTURA_DIA|11/2026|col110|2026-11-12|esp_min=241|e6_min=0|teto_min=0|dif_min=-241|classe=e6_zero_esp_trabalho|paridade=par|batidas=|veredito=em_aberto
ESTRUTURA_DIA|11/2026|col200|2026-10-29|esp_min=180|e6_min=0|teto_min=0|dif_min=-180|classe=e6_zero_esp_trabalho|paridade=par|batidas=|veredito=em_aberto
ESTRUTURA_DIA|11/2026|col200|2026-11-02|esp_min=180|e6_min=0|teto_min=0|dif_min=-180|classe=e6_zero_esp_trabalho|paridade=par|batidas=|veredito=em_aberto
ESTRUTURA_DIA|11/2026|col218|2026-11-14|esp_min=103|e6_min=0|teto_min=0|dif_min=-103|classe=e6_zero_esp_trabalho|paridade=par|batidas=|veredito=em_aberto
ESTRUTURA_DIA|11/2026|col296|2026-11-02|esp_min=345|e6_min=0|teto_min=0|dif_min=-345|classe=e6_zero_esp_trabalho|paridade=par|batidas=|veredito=trabalhou
ESTRUTURA_DIA|11/2026|col884|2026-11-08|esp_min=679|e6_min=699|teto_min=739|dif_min=20|classe=diverge_10_60|paridade=par|batidas=07:42 13:05 13:45 20:01|veredito=rejeitado
ESTRUTURA_DIAS_LINHAS 11/2026 6 True
erros_no_espelho 0
PARCIAL|11/2026|ESTRUTURA|dias=6|horas=17.8227|colabs=5
PARCIAL|11/2026|CADASTRO|dias=9|horas=32.931705|colabs=6
PARCIAL|11/2026|BATIDA|dias=40|horas=194.35440025833333|colabs=21
PARCIAL_TOTAL|11/2026|dias=55|horas=245.10880525833333|colabs=27
TEMPO_SEGUNDOS 174.3
TRATADA_LOCAL app/escala/alimentacao.py:279 GeneratorExit 257813
TRATADA_LOCAL app/escala/models.py:1282 GeneratorExit 296
TRATADA_LOCAL app/escala/services/fase_12x36.py:114 GeneratorExit 1707
TRATADA_LOCAL app/escala/services/leitor_celula.py:25 ValueError 5857
TRATADA_LOCAL app/escala/services/leitor_celula.py:81 GeneratorExit 41
TRATADA_LOCAL app/escala/utils.py:1500 GeneratorExit 4
TRATADA_LOCAL app/escala/utils.py:1687 GeneratorExit 2
TRATADA_LOCAL app/escala/utils.py:1991 GeneratorExit 6880
TRATADA_LOCAL app/escala/utils.py:2033 GeneratorExit 4
TRATADA_LOCAL app/escala/utils.py:452 GeneratorExit 6
TRATADA_LOCAL app/escala/utils.py:524 GeneratorExit 4644
TRATADA_LOCAL app/escala/utils.py:525 GeneratorExit 4644
TRATADA_LOCAL app/escala/utils.py:888 GeneratorExit 602
TRATADA_LOCAL app/folha/causas.py:58 GeneratorExit 182
TRATADA_LOCAL app/ponto/juiz_batida.py:191 GeneratorExit 17
TRATADA_LOCAL app/ponto/juiz_batida.py:75 ValueError 482764
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:1131 GeneratorExit 595
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:2604 GeneratorExit 3064
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:677 GeneratorExit 174
TRATADA_LOCAL app/ponto/services/dia_pago.py:892 GeneratorExit 12978
TRATADA_LOCAL app/ponto/services/espelho.py:473 GeneratorExit 117
TRATADA_LOCAL app/ponto/services/espelho.py:657 GeneratorExit 4228
TRATADA_LOCAL app/ponto/services/fechamento.py:1044 GeneratorExit 192
TRATADA_LOCAL app/ponto/services/fechamento.py:1045 _GradeSemCelula 4
TRATADA_LOCAL app/ponto/turnos.py:1295 GeneratorExit 24546
TRATADA_LOCAL app/ponto/turnos.py:1656 GeneratorExit 4783
TRATADA_LOCAL app/ponto/turnos.py:1699 GeneratorExit 40289
TRATADA_LOCAL app/ponto/turnos.py:679 GeneratorExit 18318
SOMA 10/2026 67   <- tem de ser igual a soma das classes (67) True
SOMA 10/2026 HORAS 348.752019728611125 True
SOMA 10/2026 COLABS_UNIAO 29 True
SOMA 11/2026 55   <- tem de ser igual a soma das classes (55) True
SOMA 11/2026 HORAS 245.10880525833333 True
SOMA 11/2026 COLABS_UNIAO 27 True
rc_sombra=0
```

- **SOMA:** as seis linhas fornecidas apresentam `True`. [nao verificado]
- **ESTRUTURA_DIAS_LINHAS:** ambas apresentam `True`: `10/2026 3 True` e `11/2026 6 True`. [nao verificado]
- **INCOMPLETO:** não há ocorrência desse marcador no dado fornecido. [nao verificado]

## 16/11 01:53 · sessao 1416237d4c62 · - · - · origem=Ronald (tmux)

Mapa direcionado entregue ao agente principal; referências centrais:

| Valor | Quem escreve | Quem lê |
|---|---|---|
| `Colaborador.situacao` | `lancar_ausencia` chama a porta; `gravar` atribui e salva (`app/ponto/views.py:2483`; `app/colaboradores/services/colaborador.py:126`). Desligamento/readmissão e reversor usam portas próprias (`app/colaboradores/services/desligamento.py:128`; `app/colaboradores/services/desligamento.py:241`; `app/ponto/management/commands/reverter_situacao_afastado.py:73`). | Lista da tela (`app/ponto/views.py:2405`), guarda da linha gravada (`app/colaboradores/services/colaborador.py:65`), consumidor após commit (`app/ponto/signals.py:267`), aviso de batida (`app/ponto/services/afastado_avisa.py:38`), badges (`app/colaboradores/services/situacao_exibida.py:50`). |
| `Colaborador.posto` | View atribui; porta salva inteiro (`app/colaboradores/views.py:327`; `app/colaboradores/services/colaborador.py:152`). | Anterior para histórico (`app/colaboradores/views.py:326`); recusa relê banco e monta contexto (`app/colaboradores/views.py:339`); template mostra esse contexto (`app/templates/colaboradores/partials/vinculo_row.html:16`). |
| Ausência criada | Serviço constrói e salva (`app/ponto/services/ausencia.py:400`; `app/ponto/services/ausencia.py:446`; `app/ponto/services/ausencia.py:478`). | Cartório e consumidor (`app/ponto/signals.py:143`; `app/ponto/signals.py:275`); juiz de cobertura/afastamento (`app/ponto/turnos.py:2719`). |
| Auditoria da Ausência | Serviço chama `evento`; escritor material é `LogAuditoria.objects.create` (`app/ponto/services/ausencia.py:501`; `app/core/observ.py:133`). | Tela de auditoria (`app/core/views.py:686`); proteção de reversão por trilha posterior (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:152`). |
| Registro de colaboradores travados | Somente `trava_na_ordem` chama `_registrar_travados`, que acrescenta sentinela à fila da conexão (`app/chamados/models.py:2744`; `app/chamados/services/trava_do_colaborador.py:37`). | `colaboradores_travados`; selo `exigir_colaboradores_travados` (`app/chamados/services/trava_do_colaborador.py:29`; `app/chamados/services/trava_do_colaborador.py:55`). |

O registro vivo autoriza explicitamente a escrita de `afastado` pela porta e mantém os leitores do campo condicionados ao vigia; o comentário da view ainda fala em lei aberta (`app/core/juizes.py:455`; `app/ponto/views.py:2474`). O censo histórico dos 104 leitores está documentado, mas sua contagem atual não foi revalidada (`app/docs/TICKETS.md:220`; `app/docs/RELATO-ARQUIVO.md:29301`).

A recusa inicial de desligado ocorre antes do serviço e do armazenamento do anexo (`app/ponto/views.py:2465`; `app/ponto/views.py:2466`). Existe borda distinta: a limpeza do arquivo protege somente a falha do `a.save()` interno; falha posterior no `gravar` ou no commit externo desfaz o banco sem passar por essa limpeza (`app/ponto/services/ausencia.py:476`; `app/ponto/services/ausencia.py:480`; `app/ponto/views.py:2483`). Ocorrência real dessa falha: [nao verificado].

## 16/11 01:53 · sessao 9ef94304cda7 · - · - · origem=Ronald (tmux)

- **Achado anterior 2 fechado:** recusa de `salvar_inteiro` ocorre antes do `save`; o histórico só vem depois; na exceção HTMX a view relê o colaborador e fornece o posto relido ao template. (`app/colaboradores/services/colaborador.py:158-160`; `app/colaboradores/views.py:329-353`; `app/templates/colaboradores/partials/vinculo_row.html:16-21`.)

- **Observação — erro após gravação continua podendo deixar alteração:** falha em `registrar_mudanca` deixa o posto novo gravado, porque o `atomic` da porta terminou antes da chamada do histórico. O quinto teste caracteriza exatamente esse cenário e exige que a tela mostre o novo posto; isso não é recusa `DesligadoSemReadmissao`. (`app/colaboradores/services/colaborador.py:158-161`; `app/colaboradores/views.py:329-339`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:154-163`.)

- **Não há juiz duplicado no pré-verificador:** `conferir_situacao_antes_do_ato` apenas exige `atomic` aberto e reutiliza `_guarda_da_linha`; esta trava e chama `_recusar_volta_de_desligado`, que consulta `esta_desligado` para destino e linha gravada. A autoridade está registrada no registro vivo. (`app/colaboradores/services/colaborador.py:55-69`; `app/colaboradores/services/colaborador.py:84-102`; `app/core/juizes.py:267-271`.)

- **Observação — comentário de lei desatualizado:** o comentário preservado afirma que a legitimidade da escrita de afastado segue em aberto, mas o registro vivo já declara a escrita via `gravar` legítima e condiciona leitores do campo ao vigia de divergência; a tabela de consulta manda prevalecer o registro vivo. (`app/ponto/views.py:2474-2483`; `app/core/juizes.py:455-473`; `app/docs/JUIZES.md:10-15`.)

- **Mordida estática dos testes:** o primeiro exige recusa dita, ausência zero e situação desligado; o segundo exige presença das três classes de SQL, trava anterior ao INSERT, registro da trava e resultado feliz; o quarto exige posto antigo no banco e HTML, ausência do posto recusado no HTML e histórico zero. Esses três contêm verificações pertinentes aos dois defeitos originais. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:77-102`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:135-152`.)

- **Observação — terceiro teste pode passar vazio quanto à sobreposição:** sua preparação já deixa exatamente uma ausência e situação ativa, que são os resultados finais exigidos; a resposta só precisa ser diferente de 500. Uma saída antecipada do endpoint que não lance ausência também satisfaz essas verificações. Falta exigir a recusa específica ou evidência de que o lançamento foi tentado. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:104-114`.)

- **Observação — quinto teste é caracterização e guarda contra cura errada:** ele não precisa reprovar o código anterior, pois este já mantinha em memória o posto novo após falha do histórico. O teste impede substituir a releitura por restauração incondicional do posto antigo. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:154-163`; diff fornecido de `app/colaboradores/views.py`, hunk `@@ -333,6 +333,10 @@`.)

- **Ganchos reproduzem uma janela real de objeto velho:** no lançamento, a carga filtrada ocorre antes do parse da data e da trava; o patch desliga no parse. Na vinculação, o patch desliga depois de carregar o objeto e antes da porta. Ambos reproduzem desligamento concluído antes da primeira trava da operação. (`app/ponto/views.py:2442-2465`; `app/colaboradores/views.py:319-329`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:43-52`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:140-146`.)

- **Limite desses ganchos:** usam UPDATE direto na mesma conexão de `TestCase`; não demonstram bloqueio real entre conexões nem corrida após a aquisição da primeira trava. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:37-40`; `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:43`.)

- **Limite do segundo teste:** verifica `FOR UPDATE < INSERT`, mas não compara posição do UPDATE nem limites de commit; o registro consultado depois da requisição continua sob o `atomic` externo de `TestCase`. Assim, não prova sozinho a duração da trava até o commit da operação em produção. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:92-102`; `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:43`; `app/chamados/services/trava_do_colaborador.py:29-34`.)

Execução dos testes e RED do commit informado: **[nao verificado]**. [nao verificado]

## 16/11 01:49 · sessao 9b8a39057952 · ESPELHO-VERDADE-FECHO-V0 · sonda · origem=Code

Transcrição integral do dado fornecido [nao verificado]:

```text
sonda_leitura: script.py [23c7f47877c9a3fd] no cpuset 4-7, lendo sombra como leitor (LEITURA)
sonda_leitura: arvore=/home/ronald/v0-arvore-3580b35e0 (copia), banco segue sombra como leitor

PARTE 2 PARTES 6
[0] UNIVERSO 10/2026 624
UNIVERSO_PARTE 10/2026 109
JANELA 10/2026 2026-09-21 2026-10-20
[0] UNIVERSO 11/2026 595
UNIVERSO_PARTE 11/2026 102
JANELA 11/2026 2026-10-21 2026-11-20
ESTIMATIVA processados 25 universo 211 minutos_total 3.0 ate_40_min True ate_15_min True
ESTIMATIVA processados 50 universo 211 minutos_total 3.0 ate_40_min True ate_15_min True
ESTIMATIVA processados 75 universo 211 minutos_total 3.0 ate_40_min True ate_15_min True
ESTIMATIVA processados 100 universo 211 minutos_total 2.8 ate_40_min True ate_15_min True
COMPETENCIA 10/2026
[1] {'dias_comparados': 1268, 'bate_ate_10min': 1227, 'esp_zero_e6_trabalho': 2, 'e6_zero_esp_trabalho': 4, 'diverge_10_60': 7, 'diverge_acima_60': 28, 'dia_batida_impar': 63, 'impar_que_BATE': 33, 'impar_que_DIVERGE': 30, 'impar_dentro_da_faixa': 24, 'impar_FORA_da_faixa': 6, 'impar_no_conjunto_do_dia': 30, 'impar_SO_POR_CORTE_do_oraculo': 0, 'dia_sem_linha_no_espelho': 0, 'dia_sem_lavratura_no_espelho': 1563, 'dia_isento_ou_ausencia': 353, 'dia_sem_trabalho_ambos': 24, 'esp_desconta_janela_declarada': 62, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 96.8% de 1268 dias comparados
[3] ESTRUTURA     5 dia-colab      11.1 h    3 colabs
[3] CADASTRO      8 dia-colab      20.4 h    6 colabs
[3] BATIDA       28 dia-colab     136.0 h   18 colabs
[4] TOTAL divergentes 41
[5] colabs com divergencia: 23
ESTRUTURA_DIA|10/2026|col37|2026-10-05|esp_min=241|e6_min=0|teto_min=0|dif_min=-241|classe=e6_zero_esp_trabalho|paridade=par|batidas=|veredito=em_aberto
ESTRUTURA_DIA|10/2026|col37|2026-10-09|esp_min=241|e6_min=0|teto_min=0|dif_min=-241|classe=e6_zero_esp_trabalho|paridade=par|batidas=|veredito=em_aberto
ESTRUTURA_DIA|10/2026|col451|2026-09-23|esp_min=707|e6_min=718|teto_min=718|dif_min=11|classe=diverge_10_60|paridade=par|batidas=07:00 18:58|veredito=em_aberto
ESTRUTURA_DIA|10/2026|col451|2026-09-25|esp_min=705|e6_min=718|teto_min=718|dif_min=13|classe=diverge_10_60|paridade=par|batidas=07:02 19:00|veredito=em_aberto
ESTRUTURA_DIA|10/2026|col841|2026-10-20|esp_min=681|e6_min=522|teto_min=701|dif_min=-159|classe=diverge_acima_60|paridade=par|batidas=11:00 18:46 21:44 22:41|veredito=em_aberto
ESTRUTURA_DIAS_LINHAS 10/2026 5 True
erros_no_espelho 0
PARCIAL|10/2026|ESTRUTURA|dias=5|horas=11.084600277777778|colabs=3
PARCIAL|10/2026|CADASTRO|dias=8|horas=20.397520712222224|colabs=6
PARCIAL|10/2026|BATIDA|dias=28|horas=136.0465618775|colabs=18
PARCIAL_TOTAL|10/2026|dias=41|horas=167.528682867500002|colabs=23
ESTIMATIVA processados 125 universo 211 minutos_total 2.9 ate_40_min True ate_15_min True
ESTIMATIVA processados 150 universo 211 minutos_total 3.0 ate_40_min True ate_15_min True
ESTIMATIVA processados 175 universo 211 minutos_total 3.0 ate_40_min True ate_15_min True
ESTIMATIVA processados 200 universo 211 minutos_total 3.0 ate_40_min True ate_15_min True
COMPETENCIA 11/2026
[1] {'dias_comparados': 1067, 'bate_ate_10min': 1011, 'esp_zero_e6_trabalho': 1, 'e6_zero_esp_trabalho': 3, 'diverge_10_60': 3, 'diverge_acima_60': 49, 'dia_batida_impar': 69, 'impar_que_BATE': 20, 'impar_que_DIVERGE': 49, 'impar_dentro_da_faixa': 45, 'impar_FORA_da_faixa': 4, 'impar_no_conjunto_do_dia': 48, 'impar_SO_POR_CORTE_do_oraculo': 1, 'dia_sem_linha_no_espelho': 408, 'dia_sem_lavratura_no_espelho': 1404, 'dia_isento_ou_ausencia': 201, 'dia_sem_trabalho_ambos': 28, 'esp_desconta_janela_declarada': 54, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 94.8% de 1067 dias comparados
[3] ESTRUTURA     3 dia-colab      23.1 h    3 colabs
[3] CADASTRO     15 dia-colab      38.5 h    6 colabs
[3] BATIDA       38 dia-colab     178.3 h   21 colabs
[4] TOTAL divergentes 56
[5] colabs com divergencia: 28
ESTRUTURA_DIA|11/2026|col37|2026-11-14|esp_min=241|e6_min=0|teto_min=0|dif_min=-241|classe=e6_zero_esp_trabalho|paridade=par|batidas=|veredito=trabalhou
ESTRUTURA_DIA|11/2026|col727|2026-11-10|esp_min=307|e6_min=0|teto_min=0|dif_min=-307|classe=e6_zero_esp_trabalho|paridade=par|batidas=|veredito=em_aberto
ESTRUTURA_DIA|11/2026|col1021|2026-11-06|esp_min=841|e6_min=0|teto_min=0|dif_min=-841|classe=e6_zero_esp_trabalho/FORA|paridade=IMPAR|batidas=05:55 19:56|veredito=folga
ESTRUTURA_DIAS_LINHAS 11/2026 3 True
erros_no_espelho 0
PARCIAL|11/2026|ESTRUTURA|dias=3|horas=23.15|colabs=3
PARCIAL|11/2026|CADASTRO|dias=15|horas=38.47013154777778|colabs=6
PARCIAL|11/2026|BATIDA|dias=38|horas=178.25105356722221|colabs=21
PARCIAL_TOTAL|11/2026|dias=56|horas=239.87118511499999|colabs=28
TEMPO_SEGUNDOS 177.5
TRATADA_LOCAL app/escala/alimentacao.py:279 GeneratorExit 273276
TRATADA_LOCAL app/escala/models.py:1282 GeneratorExit 61
TRATADA_LOCAL app/escala/services/fase_12x36.py:114 GeneratorExit 1558
TRATADA_LOCAL app/escala/services/leitor_celula.py:25 ValueError 6837
TRATADA_LOCAL app/escala/services/leitor_celula.py:81 GeneratorExit 61
TRATADA_LOCAL app/escala/utils.py:1500 GeneratorExit 4
TRATADA_LOCAL app/escala/utils.py:1687 GeneratorExit 6
TRATADA_LOCAL app/escala/utils.py:1991 GeneratorExit 6781
TRATADA_LOCAL app/escala/utils.py:2033 GeneratorExit 2
TRATADA_LOCAL app/escala/utils.py:452 GeneratorExit 6
TRATADA_LOCAL app/escala/utils.py:524 GeneratorExit 4986
TRATADA_LOCAL app/escala/utils.py:525 GeneratorExit 4986
TRATADA_LOCAL app/escala/utils.py:888 GeneratorExit 593
TRATADA_LOCAL app/folha/causas.py:58 GeneratorExit 187
TRATADA_LOCAL app/ponto/juiz_batida.py:75 ValueError 495937
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:1131 GeneratorExit 618
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:2604 GeneratorExit 3000
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:677 GeneratorExit 219
TRATADA_LOCAL app/ponto/services/dia_pago.py:892 GeneratorExit 13372
TRATADA_LOCAL app/ponto/services/espelho.py:473 GeneratorExit 108
TRATADA_LOCAL app/ponto/services/espelho.py:657 GeneratorExit 3977
TRATADA_LOCAL app/ponto/services/fechamento.py:1044 GeneratorExit 200
TRATADA_LOCAL app/ponto/services/fechamento.py:1045 _GradeSemCelula 2
TRATADA_LOCAL app/ponto/turnos.py:1295 GeneratorExit 26387
TRATADA_LOCAL app/ponto/turnos.py:1656 GeneratorExit 5880
TRATADA_LOCAL app/ponto/turnos.py:1699 GeneratorExit 41086
TRATADA_LOCAL app/ponto/turnos.py:679 GeneratorExit 19816
SOMA 10/2026 41   <- tem de ser igual a soma das classes (41) True
SOMA 10/2026 HORAS 167.528682867500002 True
SOMA 10/2026 COLABS_UNIAO 23 True
SOMA 11/2026 56   <- tem de ser igual a soma das classes (56) True
SOMA 11/2026 HORAS 239.87118511499999 True
SOMA 11/2026 COLABS_UNIAO 28 True
rc_sombra=0
```

- **SOMA:** as seis linhas exibem `True` no dado fornecido. [nao verificado]
- **ESTRUTURA_DIAS_LINHAS:** ambas exibem `True`: `10/2026 5 True` e `11/2026 3 True`. [nao verificado]
- **INCOMPLETO:** não aparece na saída fornecida desta parte. [nao verificado]

## 16/11 01:45 · sessao 8fe86b15dd7e · ESPELHO-VERDADE-FECHO-V0 · sonda · origem=Code

Na parte 1 fornecida, as seis linhas SOMA e as duas ESTRUTURA_DIAS_LINHAS são `True`. Não há ocorrência de `INCOMPLETO` nesse trecho. [nao verificado]

Transcrição integral do dado fornecido, sem verificação externa. [nao verificado]

```text
sonda_leitura: script.py [4e9a5641d912b65d] no cpuset 4-7, lendo sombra como leitor (LEITURA)
sonda_leitura: arvore=/home/ronald/v0-arvore-3580b35e0 (copia), banco segue sombra como leitor

PARTE 1 PARTES 6
[0] UNIVERSO 10/2026 624
UNIVERSO_PARTE 10/2026 110
JANELA 10/2026 2026-09-21 2026-10-20
[0] UNIVERSO 11/2026 595
UNIVERSO_PARTE 11/2026 99
JANELA 11/2026 2026-10-21 2026-11-20
ESTIMATIVA processados 25 universo 209 minutos_total 2.6 ate_40_min True ate_15_min True
ESTIMATIVA processados 50 universo 209 minutos_total 2.8 ate_40_min True ate_15_min True
ESTIMATIVA processados 75 universo 209 minutos_total 2.8 ate_40_min True ate_15_min True
ESTIMATIVA processados 100 universo 209 minutos_total 2.6 ate_40_min True ate_15_min True
COMPETENCIA 10/2026
[1] {'dias_comparados': 1180, 'bate_ate_10min': 1140, 'esp_zero_e6_trabalho': 1, 'e6_zero_esp_trabalho': 3, 'diverge_10_60': 11, 'diverge_acima_60': 25, 'dia_batida_impar': 33, 'impar_que_BATE': 15, 'impar_que_DIVERGE': 18, 'impar_dentro_da_faixa': 15, 'impar_FORA_da_faixa': 3, 'impar_no_conjunto_do_dia': 18, 'impar_SO_POR_CORTE_do_oraculo': 0, 'dia_sem_linha_no_espelho': 0, 'dia_sem_lavratura_no_espelho': 1727, 'dia_isento_ou_ausencia': 316, 'dia_sem_trabalho_ambos': 34, 'esp_desconta_janela_declarada': 43, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 96.6% de 1180 dias comparados
[3] ESTRUTURA    10 dia-colab       9.1 h    3 colabs
[3] CADASTRO     10 dia-colab      38.4 h    3 colabs
[3] BATIDA       20 dia-colab     110.2 h   13 colabs
[4] TOTAL divergentes 40
[5] colabs com divergencia: 17
ESTRUTURA_DIA|10/2026|col252|2026-09-23|esp_min=548|e6_min=594|teto_min=608|dif_min=46|classe=diverge_10_60|paridade=par|batidas=06:56 12:35 12:49 17:04|veredito=trabalhou
ESTRUTURA_DIA|10/2026|col252|2026-09-24|esp_min=552|e6_min=598|teto_min=611|dif_min=46|classe=diverge_10_60|paridade=par|batidas=06:51 12:37 12:51 17:03|veredito=trabalhou
ESTRUTURA_DIA|10/2026|col252|2026-09-25|esp_min=556|e6_min=604|teto_min=616|dif_min=48|classe=diverge_10_60|paridade=par|batidas=06:51 12:37 12:50 17:07|veredito=trabalhou
ESTRUTURA_DIA|10/2026|col252|2026-09-28|esp_min=535|e6_min=581|teto_min=595|dif_min=46|classe=diverge_10_60|paridade=par|batidas=07:12 12:38 12:53 17:07|veredito=trabalhou
ESTRUTURA_DIA|10/2026|col252|2026-10-01|esp_min=545|e6_min=590|teto_min=605|dif_min=45|classe=diverge_10_60|paridade=par|batidas=07:00 12:37 12:51 17:05|veredito=trabalhou
ESTRUTURA_DIA|10/2026|col252|2026-10-02|esp_min=545|e6_min=593|teto_min=606|dif_min=48|classe=diverge_10_60|paridade=par|batidas=06:59 12:40 12:53 17:04|veredito=trabalhou
ESTRUTURA_DIA|10/2026|col252|2026-10-06|esp_min=551|e6_min=597|teto_min=611|dif_min=46|classe=diverge_10_60|paridade=par|batidas=06:58 12:38 12:52 17:09|veredito=trabalhou
ESTRUTURA_DIA|10/2026|col252|2026-10-09|esp_min=544|e6_min=592|teto_min=604|dif_min=48|classe=diverge_10_60|paridade=par|batidas=07:06 12:48 13:00 17:10|veredito=trabalhou
ESTRUTURA_DIA|10/2026|col864|2026-10-12|esp_min=509|e6_min=449|teto_min=509|dif_min=-60|classe=diverge_10_60|paridade=par|batidas=15:01 18:10 19:10 23:30|veredito=feriado
ESTRUTURA_DIA|10/2026|col876|2026-09-21|esp_min=550|e6_min=665|teto_min=722|dif_min=115|classe=diverge_acima_60|paridade=par|batidas=18:29 04:39 05:36 06:31|veredito=em_aberto
ESTRUTURA_DIAS_LINHAS 10/2026 10 True
erros_no_espelho 0
PARCIAL|10/2026|ESTRUTURA|dias=10|horas=9.116283817777775|colabs=3
PARCIAL|10/2026|CADASTRO|dias=10|horas=38.355213611111104|colabs=3
PARCIAL|10/2026|BATIDA|dias=20|horas=110.21011656222223|colabs=13
PARCIAL_TOTAL|10/2026|dias=40|horas=157.681613991111109|colabs=17
ESTIMATIVA processados 125 universo 209 minutos_total 2.6 ate_40_min True ate_15_min True
ESTIMATIVA processados 150 universo 209 minutos_total 2.7 ate_40_min True ate_15_min True
ESTIMATIVA processados 175 universo 209 minutos_total 2.8 ate_40_min True ate_15_min True
ESTIMATIVA processados 200 universo 209 minutos_total 2.7 ate_40_min True ate_15_min True
COMPETENCIA 11/2026
[1] {'dias_comparados': 1008, 'bate_ate_10min': 946, 'esp_zero_e6_trabalho': 14, 'e6_zero_esp_trabalho': 7, 'diverge_10_60': 8, 'diverge_acima_60': 33, 'dia_batida_impar': 49, 'impar_que_BATE': 18, 'impar_que_DIVERGE': 31, 'impar_dentro_da_faixa': 21, 'impar_FORA_da_faixa': 10, 'impar_no_conjunto_do_dia': 31, 'impar_SO_POR_CORTE_do_oraculo': 0, 'dia_sem_linha_no_espelho': 396, 'dia_sem_lavratura_no_espelho': 1354, 'dia_isento_ou_ausencia': 233, 'dia_sem_trabalho_ambos': 44, 'esp_desconta_janela_declarada': 34, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 93.8% de 1008 dias comparados
[3] ESTRUTURA    10 dia-colab      35.9 h    4 colabs
[3] CADASTRO     22 dia-colab     118.5 h    6 colabs
[3] BATIDA       30 dia-colab     174.6 h   15 colabs
[4] TOTAL divergentes 62
[5] colabs com divergencia: 20
ESTRUTURA_DIA|11/2026|col114|2026-11-08|esp_min=439|e6_min=429|teto_min=499|dif_min=-10|classe=diverge_10_60|paridade=par|batidas=09:59 14:50 16:00 18:18|veredito=trabalhou
ESTRUTURA_DIA|11/2026|col114|2026-11-15|esp_min=499|e6_min=440|teto_min=499|dif_min=-59|classe=diverge_10_60|paridade=par|batidas=09:58 15:05 16:05 18:17|veredito=feriado
ESTRUTURA_DIA|11/2026|col252|2026-11-09|esp_min=551|e6_min=597|teto_min=610|dif_min=46|classe=diverge_10_60|paridade=par|batidas=06:53 12:38 12:52 17:04|veredito=trabalhou
ESTRUTURA_DIA|11/2026|col252|2026-11-10|esp_min=551|e6_min=598|teto_min=611|dif_min=47|classe=diverge_10_60|paridade=par|batidas=06:56 12:39 12:52 17:07|veredito=trabalhou
ESTRUTURA_DIA|11/2026|col252|2026-11-11|esp_min=550|e6_min=598|teto_min=611|dif_min=48|classe=diverge_10_60|paridade=par|batidas=06:54 12:39 12:52 17:04|veredito=trabalhou
ESTRUTURA_DIA|11/2026|col252|2026-11-13|esp_min=551|e6_min=597|teto_min=611|dif_min=46|classe=diverge_10_60|paridade=par|batidas=06:52 12:35 12:49 17:03|veredito=trabalhou
ESTRUTURA_DIA|11/2026|col516|2026-10-23|esp_min=361|e6_min=0|teto_min=0|dif_min=-361|classe=e6_zero_esp_trabalho|paridade=par|batidas=|veredito=folga
ESTRUTURA_DIA|11/2026|col516|2026-10-28|esp_min=372|e6_min=0|teto_min=0|dif_min=-372|classe=e6_zero_esp_trabalho|paridade=par|batidas=|veredito=trabalhou
ESTRUTURA_DIA|11/2026|col996|2026-10-28|esp_min=0|e6_min=580|teto_min=580|dif_min=580|classe=esp_zero_e6_trabalho|paridade=par|batidas=21:18 06:57|veredito=folga
ESTRUTURA_DIA|11/2026|col996|2026-11-10|esp_min=443|e6_min=1026|teto_min=1116|dif_min=583|classe=diverge_acima_60|paridade=par|batidas=06:57 13:00 14:30 15:50 21:16 06:59|veredito=em_aberto
ESTRUTURA_DIAS_LINHAS 11/2026 10 True
erros_no_espelho 0
PARCIAL|11/2026|ESTRUTURA|dias=10|horas=35.86154078361111|colabs=4
PARCIAL|11/2026|CADASTRO|dias=22|horas=118.50376287222223|colabs=6
PARCIAL|11/2026|BATIDA|dias=30|horas=174.57531034805555|colabs=15
PARCIAL_TOTAL|11/2026|dias=62|horas=328.94061400388889|colabs=20
TEMPO_SEGUNDOS 164.5
TRATADA_LOCAL app/escala/alimentacao.py:279 GeneratorExit 253166
TRATADA_LOCAL app/escala/models.py:1282 GeneratorExit 98
TRATADA_LOCAL app/escala/services/fase_12x36.py:114 GeneratorExit 1130
TRATADA_LOCAL app/escala/services/leitor_celula.py:25 ValueError 6304
TRATADA_LOCAL app/escala/services/leitor_celula.py:81 GeneratorExit 52
TRATADA_LOCAL app/escala/utils.py:1500 GeneratorExit 2
TRATADA_LOCAL app/escala/utils.py:1991 GeneratorExit 6216
TRATADA_LOCAL app/escala/utils.py:2033 GeneratorExit 3
TRATADA_LOCAL app/escala/utils.py:452 GeneratorExit 4
TRATADA_LOCAL app/escala/utils.py:524 GeneratorExit 3677
TRATADA_LOCAL app/escala/utils.py:525 GeneratorExit 3677
TRATADA_LOCAL app/escala/utils.py:888 GeneratorExit 594
TRATADA_LOCAL app/folha/causas.py:58 GeneratorExit 181
TRATADA_LOCAL app/ponto/juiz_batida.py:75 ValueError 468158
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:1131 GeneratorExit 610
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:2604 GeneratorExit 2829
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:677 GeneratorExit 172
TRATADA_LOCAL app/ponto/services/dia_pago.py:892 GeneratorExit 12196
TRATADA_LOCAL app/ponto/services/espelho.py:473 GeneratorExit 130
TRATADA_LOCAL app/ponto/services/espelho.py:657 GeneratorExit 4055
TRATADA_LOCAL app/ponto/services/fechamento.py:1044 GeneratorExit 192
TRATADA_LOCAL app/ponto/services/fechamento.py:1045 _GradeSemCelula 5
TRATADA_LOCAL app/ponto/turnos.py:1295 GeneratorExit 23417
TRATADA_LOCAL app/ponto/turnos.py:1656 GeneratorExit 4604
TRATADA_LOCAL app/ponto/turnos.py:1699 GeneratorExit 37849
TRATADA_LOCAL app/ponto/turnos.py:2653 GeneratorExit 16
TRATADA_LOCAL app/ponto/turnos.py:679 GeneratorExit 16836
SOMA 10/2026 40   <- tem de ser igual a soma das classes (40) True
SOMA 10/2026 HORAS 157.681613991111109 True
SOMA 10/2026 COLABS_UNIAO 17 True
SOMA 11/2026 62   <- tem de ser igual a soma das classes (62) True
SOMA 11/2026 HORAS 328.94061400388889 True
SOMA 11/2026 COLABS_UNIAO 20 True
rc_sombra=0
```

## 16/11 01:27 · sessao ed760e71db82 · E4C-F1-NOVA-B1 · censo · origem=Code

Numeração abaixo: primeira linha do conteúdo de cada anexo = 1, contando linhas em branco.

**1. Ressuscitação / REABERTURA alterando fila — (b): viu; a triagem não respondeu.** (`codex_verificador_v1.r:109`; `opus_final.r:2`)

Pergunta literal — `codex_verificador_v1.q:11-13`:

> 1. Para cada campo que o diff le ou grava (`respondida_em`, `resposta_colab`, `validada_em`, `materializada_em`,
>    `via_resolucao`, atos DecisaoPergunta), liste QUEM ESCREVE e QUEM LE no codigo de producao (fora de tests/), e diga
>    se todos supoem o MESMO significado (instante x dia, texto vazio x so espacos, "respondida" = I e T).

O checklist também pergunta literalmente “pouso antes do pre-requisito”. (`codex_verificador_v1.q:25`) [nao verificado]

Resposta literal — `codex_verificador_v1.r:109`:

> Pouso e sombra com zero mudanças: **[não verificado]**. A nova REABERTURA é lida pela tarefa como `RESPONDER`; equivalência booleana do juiz, sozinha, não prova zero efeito de fila. [nao verificado]

**Triagem: ausente** para esse alerta; o opus registra literalmente “triagem_v1.txt nao responde”. (`triagem_v1.txt:1-67`; `opus_final.r:2`)

A execução e o resultado da medição nas 283 ressuscitáveis constituem a parcela **(c), fora do alcance**; resultado: **[não verificado]**. O risco de código já estava apontado. (`opus_final.r:2`; `codex_verificador_v1.r:3,109`)

**2. Cópias de `texto_sem_carimbo` — (a): não viu as cópias, com o tema presente na pergunta.** (`opus_final.r:3`; `codex_verificador_v1.q:23-24`; `codex_verificador_v1.r:103,106`)

Pergunta literal — `codex_verificador_v1.q:23-24`:

> 6. Classes que ja reprovaram obra nesta casa -- diga se ha cada uma no diff: 2o juiz que duplica juiz registrado;
>    2o escritor fora da porta; relogio lido fora da borda; derivacao paralela de fonte unica; selo/RED sem mordida

Resposta literal — `codex_verificador_v1.r:103`:

> O novo juiz está registrado; os dois predicados de ilegalidade têm pergunta distinta. Resta a derivação paralela apontada no achado 3.

Esse “achado 3” da resposta trata de `espera_o_texto_do_colaborador`; as quatro cópias enumeradas pelo opus não foram apontadas como duplicação. (`codex_verificador_v1.r:13,106`; `opus_final.r:3`)

**Triagem: ausente** para esse conjunto de cópias. O descarte sobre `espera_o_texto_do_colaborador` responde ao outro apontamento. (`triagem_v1.txt:1-67`, especialmente `:13-14`)

**3. Concorrência, `update_fields` posicional e contrato p × q — (b): vistos; receberam limite, descarte ou fila na triagem.** (`codex_verificador_v3.r:15,48,64`; `triagem_v1.txt:46-57`)

Para **concorrência e `update_fields`**, pergunta literal — `codex_verificador_v3.q:6-7`:

> 2. `PerguntaDisputa._recusa_par_ilegal` (chamados/models.py) agora le o par no banco dentro do atomic do save, sem trava.
>    Existe sequencia (create; only/defer; refresh_from_db com e sem fields; save com e sem update_fields; instancia

Resposta sobre concorrência — `codex_verificador_v3.r:15`:

> 2. **PROCEDE existir contraexemplo concorrente; NÃO PROCEDE considerar a leitura dentro do `atomic` suficiente.**

Triagem literal — `triagem_v1.txt:46`:

> 2. CONCORRENCIA (A le (I,'bati'), B zera e confirma, A grava so o instante -> (I,'')): PROCEDENTE como fato, NAO CURADO

A triagem declara o limite e encaminha a decisão ao opus. (`triagem_v1.txt:48-49`)

Resposta sobre a forma posicional — `codex_verificador_v3.r:48`:

> Se o Django aceitar `p.save(False, False, None, ['respondida_em'])`, banco `(None, '')` e memória `(I, 'bati')` passam pela guarda completa, mas gravam somente o instante. Aceitação posicional nessa instalação: **[não verificado]**.

Triagem literal — `triagem_v1.txt:50-51`:

> update_fields POSICIONAL (save(False,False,None,[...])): PRE-EXISTENTE (e84a167f4 ja lia kwargs), Django 5.2.14 o trata
>    como deprecado (some no 6.0). DESCARTADO para B1 com motivo; anexo ao opus.

Para **contrato p × q**, pergunta literal — `codex_verificador_v3.q:15`:

> 5. O contrato test_contract_par_resposta.py::_governada: absolve algo que devia acusar? acusa escritor legitimo?

Resposta literal — `codex_verificador_v3.r:64`:

> A guarda pode ler `p`, enquanto o update escreve outra pergunta `q`: com texto em `p` e par vazio em `q`, carimba `q` sem texto.

Triagem literal — `triagem_v1.txt:54`:

> 5. Contrato: (a) falsa absolvicao p x q (guarda le p, escreve q) PROCEDENTE no instrumento, ofensores de producao 0;

O encaminhamento à fila está explícito em `triagem_v1.txt:56`:

> volta (limite de voltas); anexo ao opus: fila CONTRATO-PAR-POLARIDADE ganha "vincular a linha lida a escrita" e

## 16/11 01:26 · sessao 945f8a1d683f · MIUDOS-NA-FILA-DE-LOTES · leitura · origem=Code

**Há 8 causas localizadas; outras 8 entradas não têm causa atual comprovada.** BO2910-ESTRUTURA/01 contém dois defeitos distintos, por isso aparece em duas linhas. Não há identidade causal comprovada entre IDs diferentes. (`app/colaboradores/views.py:325–353`; demais provas abaixo; identidade entre IDs: **[nao verificado]**.)

A numeração dos anexos começa na primeira linha do conteúdo. Em **(b)**, “n nos sítios” limita-se aos arquivos identificados; cruzamento do restante de uma obra sem manifesto completo fica **[nao verificado]**. Os inventários usados estão reproduzidos depois da tabela. (`b1_arquivos.txt:1–62`; `r3_arquivos.txt:1–203`; anexo `brief.md`, bloco “arquivos da volta”:1–18.)

| IDs | (0) Causa e sítio produtor | (a) Tamanho e motivo | (b) Cruza espinha/BUG-PAR? | (c) Dinheiro, exportada ou API? | (d) Mesma causa de outro item |
|---|---|---|---|---|---|
| **BO-ADMIN-2710/3** | Justificativa aprovada vira conjunto de datas inteiras; o parcial soma apenas Ausência, enquanto o motor abona dias sem trabalho. (`app/ponto/services/efeito_ausencia.py:40–50`; `app/ponto/services/fechamento.py:660–665`; `app/ponto/motor_calculo_v2.py:2606–2622`.) | **OBRA:** fonte e consumidores em arquivos diferentes; o escopo registrado exige motor/folha consumindo o parcial. (`app/docs/BACKLOG.md:1956`.) | **n nos sítios citados**; demais arquivos da obra **[nao verificado]**. (`b1_arquivos.txt:1–62`; `r3_arquivos.txt:1–203`; manifesto BUG-PAR:1–18.) | **s — dinheiro:** horas de falta/abono. (`app/ponto/motor_calculo_v2.py:2618–2622`.) | **[nao verificado]** |
| **HJ-017-U1** | **Causa atual [nao verificado].** O produtor histórico já tem tratamento posicional; a reconciliação da ata também está na cópia, embora o BACKLOG ainda registre pendência. (`app/ponto/turnos.py:1067–1100`; `app/escala/utils.py:1945–2040`; `app/docs/BACKLOG.md:1253`.) | — | — | — | — |
| **BO2910-ESTRUTURA/01 — empresa** | `vincular_posto` troca o posto conservando a empresa anterior; a outra porta escreve ambos. (`app/colaboradores/views.py:325–329,1388–1389`; `app/colaboradores/services/colaborador.py:73–76`.) | **MIUDO:** divergência nas atribuições de um arquivo; ambos os campos já existem e são escritos pela outra porta nesse arquivo. (`app/colaboradores/models.py:321–322`; `app/colaboradores/views.py:1388–1391`.) | **s — R3:** `app/colaboradores/views.py`. (`r3_arquivos.txt:166`.) | **s — dinheiro/TXT:** empresa seleciona os fechamentos da exportação. (`app/folha/export.py:719–722,783–788`.) | Identidade com o defeito de exceção abaixo: **[nao verificado]**. |
| **BO2910-ESTRUTURA/01 — exceção** | A exceção da vinculação retorna HTML/toast sem registrar a falha nesse ramo. (`app/colaboradores/views.py:333–353`.) | **MIUDO:** tratamento local de erro em um arquivo de produção, sem alteração de modelo ou autoridade nesse ramo. (`app/colaboradores/views.py:333–353`.) | **s — R3:** `app/colaboradores/views.py`. (`r3_arquivos.txt:166`.) | **n no ramo produtor:** consultas, renderização, cabeçalho e retorno. (`app/colaboradores/views.py:333–353`.) | Identidade com o defeito de empresa acima: **[nao verificado]**. |
| **BO2910-ESTRUTURA/02** | A retratação pela célula seleciona somente três módulos de turno e deixa texto livre fora da seleção. (`app/chamados/reconciliador.py:337–338,997–999`; vínculo com o caso: `app/docs/RELATO.md:3328`.) | **[nao verificado]:** a exclusão está localizada, mas falta demonstrar identidade/dia suficiente para a classe; sem dia, o leitor se abstém. (`app/chamados/reconciliador.py:1024–1026`.) | **n nos sítios citados**; envelope completo **[nao verificado]**. (`b1_arquivos.txt:1–62`; `r3_arquivos.txt:1–203`; manifesto BUG-PAR:1–18.) | **[nao verificado]** para o envelope completo; o ramo conhecido chama a retratação. (`app/chamados/reconciliador.py:1001–1010`.) | **[nao verificado]** |
| **BO2910-ESTRUTURA/05** | A aprovação de parcial aceita omitir as duas horas: formulário opcional, view passa `None` e a porta só valida período quando alguma hora é fornecida. (`app/templates/ponto/partials/painel_ausencia.html:325–330`; `app/ponto/views.py:2661–2676`; `app/ponto/services/ausencia.py:973–978`.) | **[nao verificado]:** três sítios participam; isso não demonstra quantos arquivos bastam. A edição específica do período já exige ambas as horas. (`app/templates/ponto/partials/painel_ausencia.html:310–316`.) | **s — R3:** `app/ponto/views.py`. (`r3_arquivos.txt:195`.) | **s — dinheiro:** a decisão invalida fechamento do intervalo. (`app/ponto/services/ausencia.py:1002–1015`.) | Ligação causal com HJ-005-U2/U4 e BO-ADMIN-2710/1: **[nao verificado]**. |
| **BO2910-ESTRUTURA/06** | A reativação restaura cadastro/acesso sem reabrir vínculo. (`app/colaboradores/services/desligamento.py:235–258`; registro do defeito: `app/docs/BACKLOG.md:1116`.) | **[nao verificado]:** o executor está localizado, mas o alcance da reabertura não está demonstrado; o próprio contrato atual manda o admin revincular. (`app/colaboradores/services/desligamento.py:236–238`.) | **n no executor citado**; envelope completo **[nao verificado]**. (`b1_arquivos.txt:1–62`; `r3_arquivos.txt:1–203`; manifesto BUG-PAR:1–18.) | **[nao verificado]** para a reabertura; o executor atual escreve situação, acesso e histórico. (`app/colaboradores/services/desligamento.py:242–257`.) | **[nao verificado]** |
| **BO2910-ESTRUTURA/08** | A regeneração troca o DNA, conserva decisão humana e exclui humanos do rejulgamento automático. (`app/ponto/portas/celula.py:297–315,460–470,1060–1067`.) | **[nao verificado]:** os produtores estão num arquivo, mas conservar o veredito humano é regra explícita; não há alcance demonstrado que permita certificar MIUDO. (`app/ponto/portas/celula.py:973–980`.) | **n no arquivo citado**; envelope completo **[nao verificado]**. (`b1_arquivos.txt:1–62`; `r3_arquivos.txt:1–203`; manifesto BUG-PAR:1–18.) | **s — dinheiro:** o caminho recalcula fechamento por competência. (`app/ponto/portas/celula.py:483–496`.) | **[nao verificado]** |
| **HJ-074** | **Produtor causal atual [nao verificado].** O campo ainda tem default `True`, mas o nascimento atual consulta o juiz de 16/09; o dado não mostra quem gravou a flag nominal. (`app/escala/models.py:896–917`; `app/colaboradores/services/vinculo.py:287–296`; nominal: `app/docs/BACKLOG.md:2632`.) | — | — | — | — |
| **HJ-005-U1** | **Causa [nao verificado].** O registro pede censo e declara que COBERTURA-PARCIAL não foi demonstrada para 01/10. (`app/docs/BACKLOG.md:832`.) | — | — | — | — |
| **HJ-012-U2** | **Causa [nao verificado].** Há conflito entre a ingestão e a classificação documental como cobrança legítima, ainda por reconciliar. (`app/docs/BACKLOG.md:1555`; `app/docs/RELATO.md:5527`.) | — | — | — | — |
| **BO-ADMIN-2710/5** | `_marcos_faltantes` descarta a célula não missing sem ler `tipo_real`; o leitor já fornece ambos, e o registro liga isso à volta de intervalo que não nasce no nominal. (`app/chamados/services/declaracao_texto.py:130–139`; `app/escala/services/leitor_celula.py:122–133`; `app/docs/RELATO.md:3908`.) | **MIUDO no leitor localizado:** um arquivo de produção já recebe os dados; existe teste desse leitor. (`app/chamados/services/declaracao_texto.py:130–139`; `app/chamados/tests/test_hx_marcos_faltantes_celula.py:42–64`.) | **n:** produtor e teste não constam dos três manifestos. (`b1_arquivos.txt:1–62`; `r3_arquivos.txt:1–203`; manifesto BUG-PAR:1–18.) | **s — dinheiro:** o nominal inclui uma hora de intrajornada indenizada no DIFF. (`app/docs/BACKLOG.md:831`.) | **[nao verificado]** |
| **HJ-005-U2** | **Produtor nosso [nao verificado].** A sonda mostra `sem_periodo`, resposta prevista quando falta alguma hora; não mostra a origem da omissão. (Anexo `quadro.md:14`; `app/ponto/turnos.py:2646–2647`.) | — | — | — | — |
| **HJ-005-U4** | **Produtor nosso [nao verificado].** Mesmo limite: `sem_periodo` não identifica a porta que originou o cadastro incompleto. (Anexo `quadro.md:14`; `app/ponto/turnos.py:2646–2647`.) | — | — | — | — |
| **BO-ADMIN-2710/1** | **Produtor atual [nao verificado].** A causa histórica era o leitor cru da cobertura; a cópia já distingue `sem_periodo`, e a sonda não demonstra defeito novo nesse leitor. (`app/docs/RELATO.md:3904`; `app/ponto/supra_juiz.py:395,453`; anexo `quadro.md:14`.) | — | — | — | — |
| **BO-ADMIN-2710/8.1** | **Defeito atual [nao verificado].** A cópia fotografa o antes e distingue edição com antes/depois de criação; a falta de juiz leitor no dado não comprova persistência do defeito escritor. (`app/colaboradores/services/vinculo.py:187–188,368–383`; `app/colaboradores/views.py:247–249`; anexo `quadro.md:14`.) | — | — | — | — |

A alegação adicional de /06, “desligamento grava fim sem conferir vigência”, não ficou comprovada nesta cópia: o desligamento chama a porta, que restringe o fechamento pela data inicial do vínculo. (`app/colaboradores/services/desligamento.py:147–149`; `app/colaboradores/services/vinculo.py:622,638–641,862–864`.)

Para /5, a ligação documental com o produtor existe; comprovar que a divergência atual se esgota nesse mecanismo permanece **[nao verificado]**, pois o dado não contém a célula crua atual. (`app/docs/RELATO.md:3908`; anexo `quadro.md:14`.)

Os arquivos em voo abaixo são os **nomes dos anexos**; conferência contra objetos Git nesta cópia: **[nao verificado]**. A notação `{a,b}` enumera exatamente os sufixos indicados.

<details>
<summary>F1 B1 — 62 arquivos (b1_arquivos.txt:1–62)</summary>

```text
app/chamados/juizes.py
app/chamados/management/commands/auditar_invariantes_chamados.py
app/chamados/models.py
app/chamados/services/{disputa_emissao,fio,raiox}.py

app/chamados/tests/
  e3_c8b_apoio.py
  e3_v8_apoio.py
  e4b_c12_ast.py
  test_a27_recusa_com_dono.py
  test_contract_par_resposta.py
  test_contract_via_sem_declaracao.py
  test_creditos_perguntas_invariantes.py
  test_defesa1_grade.py
  test_e1_v{13,15b,15c,15f}.py
  test_e1_v5_{chave_familia,portas_backfill}.py
  test_e3_10_cura_no_escritor.py
  test_e3_catraca_respondedores.py
  test_e3_item1b.py
  test_e3_porta_A_escritor_do_par.py
  test_e3_tarefa_admin_sem_banco.py
  test_e3_validar_inline.py
  test_e4_expand.py
  test_e4b_{c12_orm,c6_orm,classe_e_memoria}.py
  test_f1nova_b1_juiz_respondida.py
  test_fechar_disputa_materializada.py
  test_fio_disputa_resolvido.py
  test_h06_falta_pela_grade.py
  test_hx_{crivo_relogio,tipo_pelo_marco,via_ausencia_dia}.py
  test_materializar_carimba_respondida.py
  test_o168_{custo_da_guarda_constante,pai_com_conversa_no_gate}.py
  test_o261_vigia_fio.py
  test_o261e_f2_fila.py
  test_paridade_fio_modal.py
  test_pep_reentrada.py
  test_porta_{pergunta_identidade,reapontamento_pergunta,resposta_pergunta}.py
  test_reconciliar_pergunta_orfa.py
  test_s122.py
  test_s139.py
  test_selo_escape_nao_e_declaracao.py
  test_selo_i4b_um_chamado_por_turno.py
  test_selo_juizes_orfaos.py

app/core/juizes.py
app/core/tests/{test_censo_escritas,test_medida_turno_do_par}.py
app/docs/ARQUITETURA.mmd
app/docs/{CORTES,JUIZES,LEIS,MAPA-JUIZES,MAPA}.md
```

</details>

<details>
<summary>R3 c17 — 203 arquivos (r3_arquivos.txt:1–203)</summary>

```text
app/chamados/juizes.py
app/chamados/management/commands/
  diff_e3_sombra.py
  lavrar_furo_sem_canal.py
  reatar_beco_folga_ao_limbo.py
  vigiar_fio_morto.py
app/chamados/services/{furo_sem_canal,tarefa_do_dia,vigia_fio}.py

app/chamados/tests/
  e3_degrau_mutantes.py
  e3_lu2_{auditoria,base,censo,entrega,green,mutantes,red,regressao,suite}.txt [nao verificado]
  e3_lu2_suite.py
  e3_lu3_apoio.py
  e3_lu3_{auditoria,base_red,censo,entrega,green,identidade_red,mutantes,red,suite}.txt [nao verificado]
  e3_mutantes.py
  e3_portas_apoio.py
  e3_portas_v6_mutantes.py
  e3_r3_c11_{auditoria,c6_mutantes,c7_mutantes,entrega}.txt
  e3_r3_c11_green{1,2,3,4,5,6,7}.txt
  e3_r3_c11_mutantes.py
  e3_r3_c11_mutantes.txt
  e3_r3_c11_red{1,2,2b,3,4,4b,5,6,7}.txt
  e3_r3_c11_regressao.txt
  e3_r3_c12_{auditoria,c11_mutantes,c6_mutantes,classificacao,degrau_green,entrega,integrais,locais,matriz_green,modulos,mutantes,red_base,ruff,v6_mutantes}.txt
  e3_r3_c12_{mutantes,verificar}.py
  e3_r3_c{14,15,16,17,4}_mutantes.py
  e3_r3_c5_{a_green,a_red,auditoria,b_green,b_red,c11_green,c11_red,c4_mutantes,c4_primeira,cor_proposta_memoria,entrega}.txt
  e3_r3_c5_geometria.py
  e3_r3_c5_leitores.py
  e3_r3_c5_{leitores,leitores_primeira,memoria,memoria_primeira}.txt
  e3_r3_c5_mutantes.py
  e3_r3_c5_mutantes.txt
  e3_r3_c5_r1_{censo_bruto,green,red,simbolos}.txt
  e3_r3_c5_r2_{chamadores,green,red}.txt
  e3_r3_c5_r5.txt
  e3_r3_c5_r6_{calendario_red,green,red}.txt
  e3_r3_c6_{c4_mutantes,c5_mutantes,entrega,green,green_f821,green_final,leitores,medida_turno,mutantes,red,red_f821,red_head}.txt
  e3_r3_c6_mutantes.py
  e3_r3_c7_{compile,entrega,green,green_carga,green_final,mutantes,red,ruff,sonda_f2}.txt
  e3_r3_c7_mutantes.py
  e3_v8_apoio.py
  e3_volta7_{apoio,mutantes}.py
  e3r3_c9_mutantes.py
  test_e3_bloqueantes.py
  test_e3_c8b_sem_banco.py
  test_e3_degrau_dono_e_porta.py
  test_e3_diff_sombra.py
  test_e3_leitores_c17_chamadores.py
  test_e3_portas_tarefa.py
  test_e3_portas_v6_sem_banco.py
  test_e3_r3_c{11,12,14,15,16,17,4,5}_sem_banco.py
  test_e3_r3_c6_nome_indefinido.py
  test_e3_r3_c{6,7}_sem_banco.py
  test_e3_tarefa_admin_sem_banco.py
  test_e3_volta7_sem_banco.py
  test_e4_{backfill,passivo}.py
  test_f3_beco1_reata_limbo.py
  test_n1_{alimentacao_unica,perguntas_do_app}.py
  test_o261_vigia_fio.py
  test_o261e_f5_alcance.py
  test_o261e_f5b_juiz_porta.py
  test_o66_furo_sem_canal.py
  test_sombra_c1_resolver_em_lote.py

app/colaboradores/services/{calendario,drawer}.py
app/colaboradores/views.py
app/config/crons.py
app/core/juizes.py
app/core/tests/test_medida_turno_do_par.py
app/core/tests/testes_sem_relogio.txt
app/escala/services/cadastro_realidade.py
app/escala/tests/test_esemvivab_desligado_nunca_bateu.py
app/escala/tests/test_f3_beco1_folga_na_lista.py
app/folha/fora_da_folha.py
app/folha/management/commands/{lavrar_retrato_folha,vigiar_retido_calado}.py
app/folha/migrations/0013_retrato_lavrado_em_help_text_e3r3.py
app/folha/{models,retrato_folha}.py
app/folha/tests/
  test_haiku_fora_da_folha.py
  test_l129_fora_da_folha_so_le_a_tarefa.py
  test_lei_desligado_nao_trava.py
  test_retrato_folha_{dono_sao_tres,drawer,quadros,smoke_chromium,tela2}.py
  test_vigia_retido_calado.py
app/folha/{views_retrato,vigia_retido_calado}.py
app/ponto/management/commands/pautas_discordantes.py
app/ponto/services/pautas_dos_discordantes.py
app/ponto/tests/{test_cxr_tela_le_o_motor,test_o320_pauta_dos_discordantes}.py
app/ponto/views.py
app/relatorios/{cartao_pela_celula,furos}.py
app/relatorios/tests/{test_e4_8_furos_le_a_lavra,test_selo_ancora_do_vinculo}.py
app/relatorios/views.py
app/templates/colaboradores/calendario_partial.html
app/templates/core/_barra_gestao.html
app/templates/folha/_retrato_o_que_fazer.html
```

</details>

<details>
<summary>BUG-PAR — manifesto adicional do brief (bloco “arquivos da volta”:1–18)</summary>

```text
app/chamados/data/bug_par_163_2026_11_13.json
app/chamados/services/
  universo_bug_par.py
  materializacao.py
  validacao.py
  explicador.py
  fio.py
app/chamados/templatetags/chamados_tags.py
app/chamados/management/commands/bug_par_103.py
app/templates/chamados/partials/{_pergunta_card,modal_fio}.html
app/chamados/tests/{test_universo_bug_par,test_bug_par_projetado,test_bug_par_103,test_s139}.py
app/config/crons.py
app/core/tests/test_medida_turno_do_par.py
app/docs/{JUIZES,RELATO}.md
```

</details>

**Contagem por (a), entre as 8 causas localizadas:** **3 MIUDO**, **1 OBRA**, **4 com tamanho [nao verificado]**. As outras **8 entradas** ficaram sem produtor causal atual comprovado e não entram como causas classificadas. (Produtores e limites: `app/colaboradores/views.py:325–353`; `app/chamados/services/declaracao_texto.py:130–139`; `app/docs/BACKLOG.md:1956`; demais linhas da tabela.)

**MIUDO com (b)=n e (c)=n: 0 demonstrados.** Os dois de /01 cruzam R3; /5 envolve dinheiro. O total absoluto do universo permanece **[nao verificado]** enquanto houver causas e tamanhos não comprovados. (`r3_arquivos.txt:166`; `app/docs/BACKLOG.md:831`.)

## 16/11 01:22 · sessao a95a624efdbf · - · - · origem=Ronald (tmux)

Achado 1: **(b)** quanto ao risco estático, já apontado em `codex_verificador_v1.r:109`; triagem: **ausente** para REABERTURA alterar fila. Apenas a medida real nas 283 ressuscitáveis fica em **(c)**. Perguntas pertinentes: `codex_verificador_v1.q:11-13` (atos, escritores/leitores e significado), `:23-26` (classes de reprovação e pouso antes do pré-requisito), `:29` (escrita no estoque). [nao verificado]

Achado 3: **(b)**, com tratamento explícito pela triagem:

- Concorrência: `triagem_v1.txt:42`, `:46-49`: reconhecida como fato, mantida como limite, encaminhada ao Opus.
- Posicional: `triagem_v1.txt:50-51`: descartado para B1.
- Contrato p×q: `triagem_v1.txt:54-57`: reconhecido, não curado nessa volta e enfileirado.

Trechos literais úteis:

`codex_verificador_v3.q:6`:

> 2. `PerguntaDisputa._recusa_par_ilegal` (chamados/models.py) agora le o par no banco dentro do atomic do save, sem trava.

`codex_verificador_v3.q:15`:

> 5. O contrato test_contract_par_resposta.py::_governada: absolve algo que devia acusar? acusa escritor legitimo?

`triagem_v1.txt:46`:

> 2. CONCORRENCIA (A le (I,'bati'), B zera e confirma, A grava so o instante -> (I,'')): PROCEDENTE como fato, NAO CURADO

`triagem_v1.txt:51`:

>    como deprecado (some no 6.0). DESCARTADO para B1 com motivo; anexo ao opus.

`triagem_v1.txt:54`:

> 5. Contrato: (a) falsa absolvicao p x q (guarda le p, escreve q) PROCEDENTE no instrumento, ofensores de producao 0;

Contagem manual de `codex_verificador_v3.r`, a confirmar pelo consolidador: concorrência `:15`, ressalva posicional `:48`, falsa absolvição p×q `:64`, exemplo `:66-71`.

## 16/11 01:22 · sessao 623ed99f3d5a · - · - · origem=Ronald (tmux)

| Causa/IDs | Sítio produtor confirmado | Tamanho: limite comprovado | Cruzamento | Dinheiro/exportada/API |
|---|---|---|---|---|
| BO2910-ESTRUTURA/01 — troca de posto conserva a empresa anterior. | `app/colaboradores/views.py:325-329`; a outra porta grava posto e empresa em `:1388-1389`; `salvar_inteiro` apenas chama `save()` em `app/colaboradores/services/colaborador.py:73-76`. | Defeito localizado em um arquivo de produção; envelope suficiente para certificar **MIUDO [nao verificado]**. | **s — R3**, `app/colaboradores/views.py` no anexo `r3_arquivos.txt`; linhas produtoras `:325-329`. | **s** — empresa participa da seleção dos fechamentos exportados em `app/folha/export.py:719-722` e `:783-788`; a consequência cross-CNPJ também está documentada em `app/colaboradores/views.py:1383-1389`. |
| BO2910-ESTRUTURA/01 — a exceção da vinculação retorna HTML/toast com HTTP padrão, sem registro da falha nesse ramo. | `app/colaboradores/views.py:333-353`. | Causa local em um arquivo de produção; **MIUDO** como dimensão desse ramo, sem alteração de modelo indicada pelo próprio ramo (`app/colaboradores/views.py:333-353`); implementação/teste futuros **[nao verificado]**. | **s — R3**, mesmo arquivo no anexo `r3_arquivos.txt`; produtor `app/colaboradores/views.py:333-353`. | **n** no ramo produtor: consultas, renderização, cabeçalho/toast e retorno (`app/colaboradores/views.py:333-353`). |
| BO2910-ESTRUTURA/02 — a retratação por célula seleciona apenas três módulos de turno, deixando texto livre fora dessa seleção. | `app/chamados/reconciliador.py:337-338` e `:997-999`; classe desconhecida conserva a premissa em `app/chamados/catalogo/premissa.py:123-125`. | **[nao verificado]**: o código mostra a exclusão, mas não demonstra identidade/dia suficiente para toda a classe; o leitor retorna sem decisão quando não encontra dia (`app/chamados/reconciliador.py:1024-1026`). Não há prova de que exigiria juiz novo. | **n** para os sítios comprovados, ausentes das três listas anexas; produtor `app/chamados/reconciliador.py:337-338`. | **[nao verificado]** para o envelope completo; o ramo conhecido retrata chamado pela porta em `app/chamados/reconciliador.py:1001-1010`. |
| BO2910-ESTRUTURA/05 — a aprovação de parcial aceita omitir ambas as horas do período. | Inputs opcionais em `app/templates/ponto/partials/painel_ausencia.html:325-330`; view conserva `None` e aprova em `app/ponto/views.py:2661-2676`; porta só valida período quando ao menos uma hora vem preenchida em `app/ponto/services/ausencia.py:973-978`. | **[nao verificado]**: três sítios participam do comportamento, mas isso não prova quantos precisam mudar. A edição específica da aprovada já tem campos obrigatórios (`app/templates/ponto/partials/painel_ausencia.html:310-316`). | **s — R3** para `app/ponto/views.py`, listado no anexo `r3_arquivos.txt`; sítio `:2661-2676`. Template e serviço não constam das listas. | **s** — a decisão invalida o fechamento do intervalo em `app/ponto/services/ausencia.py:1002-1015`. |
| BO2910-ESTRUTURA/06 — reativação restaura cadastro/acesso e não reabre vínculo. | Executor inteiro em `app/colaboradores/services/desligamento.py:235-258`; a própria descrição diz que a escala não é reativada automaticamente em `:236-238`. | **[nao verificado]**: a ausência está localizada, mas não há envelope suficiente demonstrando uma alteração em só um arquivo sem decisão adicional (`app/colaboradores/services/desligamento.py:235-258`). | **n** para o executor comprovado, ausente das listas anexas; produtor `app/colaboradores/services/desligamento.py:235-258`. | **[nao verificado]** para eventual reabertura; o executor atual grava situação/acesso/histórico (`app/colaboradores/services/desligamento.py:242-257`). |
| BO2910-ESTRUTURA/08 — regeneração altera DNA enquanto decisão humana permanece e impede novo julgamento automático. | Regeneração escreve DNA sem apagar veredito em `app/ponto/portas/celula.py:297-315`; humanos são excluídos do rejulgamento em `:460-470`; a lavra do cartório conserva veredito admin em `:1060-1067`. | Sítios estão em **um arquivo de produção**; certificar **MIUDO [nao verificado]**, porque a conservação humana é regra explícita em `app/ponto/portas/celula.py:973-980`, e o envelope/lei da alteração não está demonstrado. | **n** para esse arquivo, ausente das listas anexas; produtores `app/ponto/portas/celula.py:297-315`, `:460-470`, `:1060-1067`. | **s** — o caminho de regeneração chama `recalcular_por_evento` por competência em `app/ponto/portas/celula.py:483-496`. |

A outra alegação de BO2910-ESTRUTURA/06, “desligamento grava fim sem conferir vigência”, está **[nao verificado]** como defeito atual: o desligamento chama a porta de fechamento em `app/colaboradores/services/desligamento.py:147-149`; ela delega em `app/colaboradores/services/vinculo.py:862-864`, identifica recusados em `:622` e escreve somente vínculos cujo início não supera o fim em `:638-641`.

Os dois defeitos de /01 não têm causa única demonstrada: atribuições incompletas em `app/colaboradores/views.py:325-329` e tratamento de exceção em `:333-353` são sítios distintos. Mesma causa entre os demais IDs: **[nao verificado]**. Em particular, `sem_periodo` dos nominais col42 não prova que nasceram pelo formulário opcional de /05; essa ligação é **[nao verificado]**, embora o formulário permita a omissão (`app/templates/ponto/partials/painel_ausencia.html:325-330`).

## 16/11 01:21 · sessao d983759200f5 · - · - · origem=Ronald (tmux)

| ID | Causa / sítio produtor | (a) | (b) | (c) | (d) |
|---|---|---|---|---|---|
| BO-ADMIN-2710/3 | Justificativa aprovada entra como data inteira; minutos parciais só vêm de Ausência, e o motor abona apenas dia sem trabalho. `app/ponto/services/efeito_ausencia.py:40-50`; `app/ponto/services/fechamento.py:660-665`; `app/ponto/motor_calculo_v2.py:2606-2622`. | **OBRA**: fonte e consumidor estão em arquivos distintos; documentação exige consumo pelo motor e folha. `app/docs/BACKLOG.md:1956`. | Esses sítios não constam nos manifestos fornecidos. Cruzamento adicional do delta ainda não construído: **[nao verificado]**. | **s, dinheiro**: calcula `horas_falta`/`horas_abono`; PDF soma parcial separadamente. `app/ponto/motor_calculo_v2.py:2618-2622`; `app/relatorios/pdf_espelho.py:479`. | Igualdade causal com outros IDs do universo: **[nao verificado]**. |
| HJ-017-U1 | **Sítio produtor atual [nao verificado]**. A origem histórica pela batida `E` possui tratamento em `app/ponto/turnos.py:1067-1100`; a pendência documental do terceiro leitor (`app/docs/BACKLOG.md:1253`) possui reconciliação cross-meia-noite em `app/escala/utils.py:1945-2040` e teste nominal em `app/escala/tests/test_saida_valida_cross_meia_noite.py:5-13`. | — | — | — | — |
| HJ-074 | Campo admite `trabalha_em_feriado=True` por default; esse valor impede a folga no leitor. `app/escala/models.py:896-898`; `:1390-1395`. **Sítio que efetivamente gravou o valor de col148 [nao verificado]**: a porta atual já consulta o juiz de 16/09, `app/colaboradores/services/vinculo.py:287-296`. | — | — | — | — |

Para HJ-074, caso a consolidação aceite o default como produtor histórico já comprovado pelo anexo: o envelope documental é **OBRA**, pois inclui alteração de modelo e comando próprio (`app/docs/RELATO.md:786`); **dinheiro=s**, pois a porta altera a flag, regenera células e recalcula fechamento (`app/colaboradores/services/vinculo.py:886-908`). Isso não comprova a origem individual da flag de col148. **[nao verificado]**

## 16/11 01:21 · sessao c27123f1aeb8 · - · - · origem=Ronald (tmux)

Achado 2 → **(a): tema presente na pergunta, cópias específicas não apontadas na resposta.** Base: `codex_verificador_v1.q:23-24`:

> 6. Classes que ja reprovaram obra nesta casa -- diga se ha cada uma no diff: 2o juiz que duplica juiz registrado;  
>    2o escritor fora da porta; relogio lido fora da borda; derivacao paralela de fonte unica; selo/RED sem mordida

Na resposta, a derivação apontada corresponde a `fio.py`, sem identificar as quatro cópias relacionadas pelo opus (`codex_verificador_v1.r`, tabela da pergunta 6):

> | Segundo juiz registrado | O novo juiz está registrado; os dois predicados de ilegalidade têm pergunta distinta. Resta a derivação paralela apontada no achado 3. (`app/core/juizes.py:306`; `app/chamados/juizes.py:1531`; `app/chamados/juizes.py:1537`; `app/chamados/services/fio.py:187`) |

> | Derivação paralela | **Presente:** achado 3. (`app/chamados/services/fio.py:187`) |

Essas linhas são **103 e 106**, respectivamente, se a linha “Pouso antes do pré-requisito” é 109, conforme referência expressa em `opus_final.r:2`; contagem independente do `.r` completo: **[não verificado]**. [nao verificado]

Triagem do achado específico **ausente**; os descartes relativos a I21 e `espera_o_texto_do_colaborador` tratam outros apontamentos (`triagem_v1.txt`, itens 2 e 3 da seção v1).

Na cópia, os locais correspondentes estão em `disputa_emissao.py:1860`, `curar_perguntas_em_limbo.py:46`, `auditar_invariantes_chamados.py:1105-1106` e `disputa_emissao.py:1691`, diferindo da numeração mencionada pelo opus.

## 16/11 01:21 · sessao f307ae9f5253 · - · - · origem=Ronald (tmux)

| ID | Causa / prova do produtor | Classificação sustentada |
|---|---|---|
| BO-ADMIN-2710/5 | `_marcos_faltantes` ignora `tipo_real`: descarta célula com `missing=False`, mesmo quando a batida tem tipo diferente do marco (`app/chamados/services/declaracao_texto.py:130-139`). O leitor fornece separadamente `tipo_real` e `missing=not acesa` (`app/escala/services/leitor_celula.py:122-133`). O registro liga explicitamente esse mecanismo à E23 ocupada por S00 do col247 (`app/docs/RELATO.md:3908`); o nominal “cobra saída22 com S00 presente; certo volta” também consta em `app/docs/BACKLOG.md:831`. | **MIUDO como avaliação estática do defeito nesse leitor**: decisão inteira em um produtor, que já recebe os dados necessários (`declaracao_texto.py:130-139`; `leitor_celula.py:128-133`); teste existente do mesmo leitor em `app/chamados/tests/test_hx_marcos_faltantes_celula.py:42-64`. **(b)=n** para esse arquivo e teste: ausentes dos manifestos fornecidos (anexos `b1_arquivos.txt:1-62`, `r3_arquivos.txt:1-215` e `brief.md`, bloco “arquivos da volta”; conferir numeração dos anexos ao consolidar). **(c)=s, dinheiro**: o nominal inclui 1h de intrajornada indenizada no DIFF (`app/docs/BACKLOG.md:831`). Igualdade causal com outros IDs do universo: **[nao verificado]**. |
| BO-ADMIN-2710/8.1 | Defeito atual produtor: **[nao verificado]**. A cópia já fotografa o antes (`app/colaboradores/services/vinculo.py:187-188`) e distingue `editar` com antes/depois de `criar` (`:368-383`); a view documenta a retirada da linha nula e delega à porta (`app/colaboradores/views.py:238-249`). | Não classificar (a)–(d). A falta de juiz leitor no dado não demonstra defeito atual do escritor. O histórico documental anuncia a alteração em `app/docs/RELATO.md:3557`; os testes caracterizam a troca e os valores em `app/colaboradores/tests/test_trilha_do_vinculo.py:84-97`, `:141-153` — execução nesta leitura: **[nao verificado]**. |
| HJ-005-U1 | Causa produtora: **[nao verificado]**. O registro pede censo e diz explicitamente que COBERTURA-PARCIAL não está demonstrada para 01/10 (`app/docs/BACKLOG.md:832`); a ingestão registra dono, sem fechar sua obra (`app/docs/RELATO.md:8919`, `:8929`). | Não classificar (a)–(d), nem fundir com HJ-005-U2/U4 ou BO /1. |
| HJ-012-U2 | Causa produtora: **[nao verificado]**. Há conflito entre ingestão ESTRUTURA (`app/docs/RELATO.md:8919`) e medição documental de cobrança legítima do col239 (`app/docs/RELATO.md:5527`); o complemento manda reconciliar ambos (`app/docs/BACKLOG.md:1555`). | Não classificar (a)–(d). |

Ressalva para BO /5: o dado novo informa apenas a saída de `_marcos_faltantes`; a célula crua atual não foi fornecida. A associação documental do nominal ao produtor existe (`BACKLOG.md:831`; `RELATO.md:3908`), mas comprovar que toda discrepância atual se esgota nesse único mecanismo permanece **[nao verificado]**.

## 16/11 01:15 · sessao bf44a775e492 · RESPOSTAS-BO-1611 · censo · origem=Code

**A partição v1 soma 131, mas não está demonstrada como partição exclusiva de pessoa+dia.** Os anexos individualizam 126 IDs canônicos; há sobreposições entre raízes e casos classificados como “sem dia-colab” que já têm ambos. (app/docs/RELATO.md:429; anexo estrutura18.tsv:1–18; anexo final67.tsv:2–38; anexo mapa_hj_sem_nome.tsv:1–80)

Uso a numeração dos anexos contando sua primeira linha de conteúdo como linha 1; no código, cito a numeração desta cópia. Identidade Git do HEAD: **[nao verificado]**.

**0 — Conferência da partição**

| Conferência | Resultado e prova |
|---|---|
| ESTRUTURA histórica | Os 18 estão discriminados: nove NO AR, sete com obra pendente e dois sem cobertura comprovada. (anexo resolve.tsv:2–19) |
| Triagem dos 37 | 27 SEM DIA-COLAB, quatro BATIDA, quatro CADASTRO, dois ESTRUTURA. (anexo final67.tsv:2–38) |
| Ingestão | 80 unidades = seis ESTRUTURA + três CADASTRO + cinco BATIDA + 33 sem divergência histórica + 33 indeterminadas. (app/docs/RELATO.md:8916–8921) |
| IDs individualizados | `80 + 18 + 37 − 6 − 3 = 126`: seis ESTRUTURA já pertencem à ingestão; três indeterminados reaparecem em final67. (anexo estrutura18.tsv:1–6; anexo final67.tsv:12–14; anexo mapa_hj_sem_nome.tsv:1–80) |
| Diferença para 131 | O p0 declara sete CADASTRO e seis BATIDA; o mapa individualiza três e cinco. Faltam, portanto, individualizar **quatro CADASTRO e um BATIDA** para demonstrar o total declarado. Identidades desses cinco no p0: **[nao verificado]**. (app/docs/RELATO.md:429; app/docs/RELATO.md:8917–8918) |

As sobreposições encontradas são estas:

| Pessoa+dia | Raízes/baldes envolvidos | Prova |
|---|---|---|
| col42, 08/10 e 15/10 | BO-ADMIN-2710/1 e HJ-005-U2/U4: duas raízes dentro de “não reconferido”. | app/docs/RELATO.md:3625; anexo mapa_hj_sem_nome.tsv:8,10 |
| col42, 22/10 | BO-ADMIN-2710/1 em “não reconferido”; HJ-005-U5 em BATIDA. | app/docs/RELATO.md:3625; anexo mapa_hj_sem_nome.tsv:11 |
| col247, 24/10 | BO-ADMIN-2710/5 em “não reconferido”; HJ-031-U1 em “falta informação”. | app/docs/RELATO.md:3905; anexo final67.tsv:13; anexo mapa_hj_sem_nome.tsv:51 |
| col130, 05/10 | BO-ADMIN-2710/7 em “não reconferido”; HJ-018-U1 nos 33 históricos sem divergência. | app/docs/BACKLOG.md:835; anexo mapa_hj_sem_nome.tsv:33 |
| col789, 11/10 | Nominal RT-04 da BO-ADMIN-2710/3, com obra; HJ-004-U1 nos 33 históricos sem divergência. | app/docs/BACKLOG.md:1956; anexo mapa_hj_sem_nome.tsv:5 |

Essas raízes podem denunciar sintomas diferentes, mas **não formam unidades pessoa+dia exclusivas**. Não as fundi automaticamente. (app/docs/CORTES.md:373; app/docs/RELATO.md:3905; app/docs/BACKLOG.md:1956)

Também há três classificações “SEM DIA-COLAB” materialmente incorretas:

- HJ-012-U1: col239, 2026-10-07; discordância das testemunhas. (anexo final67.tsv:12; anexo mapa_hj_sem_nome.tsv:18)
- HJ-031-U1: col247, 2026-10-24; discordância das testemunhas. (anexo final67.tsv:13; anexo mapa_hj_sem_nome.tsv:51)
- HJ-050-U2: col523, 2026-11-07; era futuro na leitura de 05/11. (anexo final67.tsv:14; anexo mapa_hj_sem_nome.tsv:72; app/docs/RELATO.md:8920)

Portanto, não certifico um quadro corrigido exclusivo de 131: faltam os cinco IDs do p0 e a definição de tratamento das sobreposições. Os 33 permanecem como medida histórica, sem reconferência nova nesta resposta. (app/docs/RELATO.md:429; app/docs/RELATO.md:8919; app/docs/RELATO.md:8924)

**1 — Os nove “não reconferidos”**

A referência nominal privada da ingestão está em `logs/hj_0511/fonte` e `logs/hj_0511/mapa.md`; esses destinos são apontados no RELATO. Linhas internas contendo o casamento por CPF/nome: **[nao verificado]**. (app/docs/RELATO.md:8894; app/docs/RELATO.md:8924)

O lookup não oferece um juiz genérico de “caso fechado”: declara essa pergunta SEM juiz. Abaixo, FECHADO refere-se ao defeito indicado, com os limites escritos. (app/docs/JUIZES.md:109)

| Caso | Pessoa e dia escritos | Juiz real e esperado para FECHADO |
|---|---|---|
| HJ-005-U2 | col42, 2026-10-08. (anexo mapa_hj_sem_nome.tsv:8) | `ponto.turnos.periodo_da_parcial_no_dia`: esperado `fora`; `sem_periodo`/`nao_sei` não conferem. Para afirmar o que a tela mostra, consultar também `ponto.status_dia.status_do_dia_lavrado`. Cobertura no lookup: JUIZES:55–56; implementação: turnos.py:2628–2655; status: JUIZES:120; status_dia.py:201. |
| HJ-005-U4 | col42, 2026-10-15. (anexo mapa_hj_sem_nome.tsv:10) | Mesmo juiz e esperado do anterior. (app/ponto/turnos.py:2628–2655; app/docs/JUIZES.md:55–56,120) |
| HJ-030-U1 | col94, 2026-10-16; pergunta 44764, disputa 6984. (anexo mapa_hj_sem_nome.tsv:50; app/docs/RELATO.md:3964) | `chamados.juizes.pergunta_viva` + `chamados.services.fio.montar_chamados_fio`: se viva, deve aparecer no fio do admin. Viva e invisível = ABERTO; morta, isoladamente, não prova desfecho. O lookup só aponta o localizador de disputa; o registro vivo nomeia C6. (app/docs/JUIZES.md:40; app/core/juizes.py:286; app/chamados/juizes.py:865; app/chamados/services/fio.py:242–275) |
| BO-ADMIN-2710/1 | col42, 2026-10-08, 15 e 22. (app/docs/RELATO.md:3625) | Mesmo juiz de cobertura parcial; esperado `fora`, sem certificar outros furos ou dinheiro. (app/docs/JUIZES.md:55–56; app/ponto/turnos.py:2632–2637) |
| BO-ADMIN-2710/2 | col789; sete dias, somente 08/10 e 26/10 explicitados. **Mover para falta informação: lista exata dos sete dias.** (app/docs/RELATO.md:3625) | Geometria: `ponto.turnos.parear_turnos`/`turnos_do_colab`, lookup JUIZES:20. O nominal exige entrada reconhecida, pontualidade e HE correspondentes; um valor escalar de FECHADO para a raiz: **[nao verificado]**. (app/docs/CORTES.md:364; app/docs/JUIZES.md:20) |
| BO-ADMIN-2710/5 | col247, 2026-10-24. (app/docs/RELATO.md:3905) | Leitor real `_marcos_faltantes`: esperado nominal `int_volta`, tipo `E`, em vez de `int_ida/S`. Isso ainda exige conferir emissão e efeito; não basta fechar chamado. Não há linha específica desse leitor no lookup. (app/chamados/services/declaracao_texto.py:86–143; app/docs/CORTES.md:373; lookup específico **[nao verificado]**) |
| BO-ADMIN-2710/6 | col42/129/789, alvos sem todos os dias. Subcaso escrito: col129, 2026-10-25, chamado 35101. **Mover a raiz para falta informação:** dias/perguntas restantes e tela/perfil do supervisor. (app/docs/RELATO.md:3906; app/docs/CORTES.md:373–374) | C6 `pergunta_viva` e fio para invisibilidade; `qs_na_fila` para a fila. O subcaso depende expressamente da identificação da tela/perfil. (app/core/juizes.py:286; app/chamados/services/fio.py:242; app/chamados/services/painel.py:158; app/docs/CORTES.md:374) |
| BO-ADMIN-2710/7 | col130, 2026-10-05, chamado 28883. (app/docs/BACKLOG.md:835) | `chamados.services.painel.qs_na_fila(...).exists()`: esperado **False** para a pendência nominal. `registrado` não pede ação. Lookup: JUIZES:59–60,82; implementação painel.py:158–190. |
| BO-ADMIN-2710/8.1 | col789; ato de edição em 2026-10-27, 14:33–15:56; vínculos 1221/1472/1473. (app/docs/CORTES.md:362) | `colaboradores.services.vinculo.executar_vinculo` é escritor: **NAO_CONFERIDO, não chamar**. Juiz leitor que certifique fidelidade histórica de ação/antes/depois: **[nao verificado]**. A cura não reescreve a trilha histórica. (app/colaboradores/services/vinculo.py:170; app/docs/RELATO.md:3579) |

**2 — Os 57 rótulos originais “falta informação”**

“De quem” abaixo identifica a **fonte do relato**; não transfere ao relator medições internas. A origem HJ/Fernando está escrita no BACKLOG; BO-0611/Mauricio no PROMPTS; BO-1311/Mauricio na própria obra. (app/docs/BACKLOG.md:2233; app/docs/PROMPTS.md:1914; app/docs/BACKLOG.md:795)

| Caso | Dado que falta ou correção do rótulo | De quem — prova da origem |
|---|---|---|
| HJ-058 | Individualizar pessoa/dia/anexo entre quatro episódios HEIC; há datas textuais, não ausência absoluta de dia. (app/docs/BACKLOG.md:2633) | Fernando — app/docs/BACKLOG.md:2233 |
| HJ-066 | Dado externo necessário **[nao verificado]**; as duas dashboards já estão identificadas. (app/docs/BACKLOG.md:2626) | Fernando — app/docs/BACKLOG.md:2233 |
| HJ-068 | Dado externo necessário **[nao verificado]**; fio lateral, Histórico e contador já estão especificados. (app/docs/BACKLOG.md:2627) | Fernando — app/docs/BACKLOG.md:2233 |
| HJ-071 | Medir internamente se `hora_ini/hora_fim` estão gravadas; telas espelho/PDF conhecidas. (app/docs/BACKLOG.md:2628) | Fernando — app/docs/BACKLOG.md:2233 |
| HJ-073 | Censo interno por causa × permissão do supervisor. (app/docs/BACKLOG.md:2629) | Fernando — app/docs/BACKLOG.md:2233 |
| HJ-075 | Print do app mostrando escala antiga; col1032 conhecido. (app/docs/BACKLOG.md:2631) | Fernando — app/docs/BACKLOG.md:2233 |
| HJ-090 | Classificação interna L-099 dos 40 dias de férias com batida; individualização nessa linha **[nao verificado]**. (app/docs/BACKLOG.md:1932) | Fernando — app/docs/BACKLOG.md:2233 |
| HJ-INGESTAO-0511/HJ-012-U1 | Pessoa/dia presentes: col239, 07/10; falta resolver discordância das testemunhas. (anexo mapa_hj_sem_nome.tsv:18) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-INGESTAO-0511/HJ-031-U1 | Pessoa/dia presentes: col247, 24/10; falta resolver discordância das testemunhas. (anexo mapa_hj_sem_nome.tsv:51) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-INGESTAO-0511/HJ-050-U2 | Pessoa/dia presentes: col523, 07/11; precisa reconferência após a leitura que o considerava futuro. (anexo mapa_hj_sem_nome.tsv:72) | Fernando — app/docs/BACKLOG.md:1883 |
| BO-0611/02 | Qual é a queixa concreta; col78 já corrigido. Dia necessário **[nao verificado]**. (app/docs/RELATO.md:1934; app/docs/PROMPTS.md:1925) | Mauricio — app/docs/PROMPTS.md:1914 |
| BO-0611/03 | Motivos das 40 ocorrências do lote. (app/docs/RELATO.md:1934) | Mauricio — app/docs/PROMPTS.md:1914 |
| BO-0611/05 | Dia do episódio; col456 conhecido. (app/docs/RELATO.md:4; app/docs/RELATO.md:1934) | Mauricio — app/docs/PROMPTS.md:1914 |
| BO-0611/06 | Lacuna atual **[nao verificado]**; quadro antigo dizia conta de teste ativa, mas foi substituído. (app/docs/RELATO.md:1996; app/docs/PROMPTS.md:1925) | Mauricio — app/docs/PROMPTS.md:1914 |
| BO-0611/08 | Dia/prova atual **[nao verificado]**; registro substitutivo já diz col617, atestado aprovado, sem divergência. (app/docs/RELATO.md:1934) | Mauricio — app/docs/PROMPTS.md:1914 |
| BO-0611/09 | Dia/chamado individual **[nao verificado]**; col1000 e falta de porta antes da resposta conhecidos. (app/docs/RELATO.md:1934) | Mauricio — app/docs/PROMPTS.md:1914 |
| BO-0611/10 | Dia e troca individual **[nao verificado]**; col399 e ausência de UI conhecidos. (app/docs/RELATO.md:1934) | Mauricio — app/docs/PROMPTS.md:1914 |
| BO-0611/14 | Dia próprio do item 14 **[nao verificado]**; o turno 21/10 foi explicitamente destinado ao item 13. (app/docs/RELATO.md:1997) | Mauricio — app/docs/PROMPTS.md:1914 |
| BO-1311-ROTEIRO/01 | Confirmar ficha↔número; lacuna individual de dia/declaração/retentor **[nao verificado]**. (app/docs/RELATO.md:685,689) | Mauricio — app/docs/BACKLOG.md:795 |
| BO-1311-ROTEIRO/02 | Confirmar ficha↔número; lacuna individual de dia/declaração/retentor **[nao verificado]**. (app/docs/RELATO.md:685,689) | Mauricio — app/docs/BACKLOG.md:795 |
| BO-1311-ROTEIRO/03 | Confirmar ficha↔número; lacuna individual de dia/declaração/retentor **[nao verificado]**. (app/docs/RELATO.md:685,689) | Mauricio — app/docs/BACKLOG.md:795 |
| BO-1311-ROTEIRO/05 | Confirmar ficha↔número; lacuna individual de dia/declaração/retentor **[nao verificado]**. (app/docs/RELATO.md:685,689) | Mauricio — app/docs/BACKLOG.md:795 |
| BO-1311-ROTEIRO/06 | Confirmar ficha↔número; lacuna individual de dia/declaração/retentor **[nao verificado]**. (app/docs/RELATO.md:685,689) | Mauricio — app/docs/BACKLOG.md:795 |
| BO-1311-ROTEIRO/07 | Confirmar ficha↔número; lacuna individual de dia/declaração/retentor **[nao verificado]**. (app/docs/RELATO.md:685,689) | Mauricio — app/docs/BACKLOG.md:795 |
| BO-1311-ROTEIRO/08 | Confirmar ficha↔número; lacuna individual de dia/declaração/retentor **[nao verificado]**. (app/docs/RELATO.md:685,689) | Mauricio — app/docs/BACKLOG.md:795 |
| BO-1311-ROTEIRO/09 | Confirmar ficha↔número; lacuna individual de dia/declaração/retentor **[nao verificado]**. (app/docs/RELATO.md:685,689) | Mauricio — app/docs/BACKLOG.md:795 |
| BO-ADMIN-2710/8.3 | Recorte dos dias afetados; pessoa/vínculo conhecidos, mecanismo já descrito. (app/docs/RELATO.md:3908) | Mauricio — app/docs/RELATO.md:3894,3898 |
| HJ-004-U2 | Dia; col789 presente. (anexo mapa_hj_sem_nome.tsv:6) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-007-U1 | Dia; col253 presente. (anexo mapa_hj_sem_nome.tsv:13) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-008-U1 | Dia; col283 presente. (anexo mapa_hj_sem_nome.tsv:14) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-017-U2 | Dia; col923 presente. (anexo mapa_hj_sem_nome.tsv:32) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-025-U1 | Dia; col270 presente. (anexo mapa_hj_sem_nome.tsv:42) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-026-U1 | Dia; col129 presente. (anexo mapa_hj_sem_nome.tsv:43) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-032-U1 | Dia; col163 presente. (anexo mapa_hj_sem_nome.tsv:52) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-033-U1 | Dia; col960 presente. (anexo mapa_hj_sem_nome.tsv:53) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-035-U1 | Dia; col949 presente. (anexo mapa_hj_sem_nome.tsv:56) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-036-U1 | Dia; col207 presente. (anexo mapa_hj_sem_nome.tsv:57) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-037-U1 | Dia; col332 presente. (anexo mapa_hj_sem_nome.tsv:58) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-038-U1 | Dia; col446 presente. (anexo mapa_hj_sem_nome.tsv:59) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-039-U1 | Dia; col89 presente. (anexo mapa_hj_sem_nome.tsv:60) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-040-U1 | Dia; col92 presente. (anexo mapa_hj_sem_nome.tsv:61) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-041-U1 | Dia; col231 presente. (anexo mapa_hj_sem_nome.tsv:62) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-042-U1 | Dia; col214 presente. (anexo mapa_hj_sem_nome.tsv:63) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-043-U1 | Dia; col235 presente. (anexo mapa_hj_sem_nome.tsv:64) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-044-U1 | Dia; col246 presente. (anexo mapa_hj_sem_nome.tsv:65) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-045-U1 | Dia; col250 presente. (anexo mapa_hj_sem_nome.tsv:66) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-046-U1 | Dia; col673 presente. (anexo mapa_hj_sem_nome.tsv:67) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-047-U1 | Dia; col194 presente. (anexo mapa_hj_sem_nome.tsv:68) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-048-U1 | Dia; col219 presente. (anexo mapa_hj_sem_nome.tsv:69) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-051-U1 | Dia; col123 presente. (anexo mapa_hj_sem_nome.tsv:73) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-052-U1 | Dia; col1023 presente. (anexo mapa_hj_sem_nome.tsv:74) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-052-U2 | Dia; col78 presente. (anexo mapa_hj_sem_nome.tsv:75) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-053-U1 | Pessoa e dia. (anexo mapa_hj_sem_nome.tsv:76) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-054-U1 | Pessoa e dia. (anexo mapa_hj_sem_nome.tsv:77) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-055-U1 | Pessoa e dia. (anexo mapa_hj_sem_nome.tsv:78) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-056-U1 | Dia; col270 presente. (anexo mapa_hj_sem_nome.tsv:79) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-056-U2 | Pessoa e dia. (anexo mapa_hj_sem_nome.tsv:80) | Fernando — app/docs/BACKLOG.md:1883 |

Agrupamento dos **57 rótulos originais**, sem fingir que todos são pedidos externos:

| Tipo | Quantidade | Lastro |
|---|---:|---|
| Dia ausente, pessoa presente | 26 | anexo mapa_hj_sem_nome.tsv:6–80 |
| Pessoa e dia ausentes | 4 | anexo mapa_hj_sem_nome.tsv:76–78,80 |
| Individualizar episódio/anexo HEIC | 1 | app/docs/BACKLOG.md:2633 |
| Demanda de tela já descrita; lacuna externa não demonstrada | 2 | app/docs/BACKLOG.md:2626–2627 |
| Medição/classificação interna | 3 | app/docs/BACKLOG.md:2628–2629,1932 |
| Print | 1 | app/docs/BACKLOG.md:2631 |
| Pessoa+dia presentes; discordância/reconferência | 3 | anexo mapa_hj_sem_nome.tsv:18,51,72 |
| Queixa/motivos do lote | 2 | app/docs/RELATO.md:1934 |
| Episódio/dia/prova individual não demonstrados | 5 | app/docs/RELATO.md:4,1934,1997 |
| Lacuna atual não determinada após substituição do quadro | 1 | app/docs/RELATO.md:1996; app/docs/PROMPTS.md:1925 |
| Correspondência e lacuna individual do roteiro não demonstradas | 8 | app/docs/RELATO.md:685,689 |
| Dias afetados pela desativação | 1 | app/docs/RELATO.md:3908 |
| **Soma** | **57** | anexo final67.tsv:2–38; anexo mapa_hj_sem_nome.tsv:1–80 |

BO-ADMIN-2710/2 e /6 acrescentam duas raízes incompletas aos pendentes; não foram inseridas artificialmente nas 57 linhas acima. (app/docs/RELATO.md:3625,3906; app/docs/CORTES.md:374)

**3 — Oito com obra e dois sem obra**

“Hoje” nesta tabela significa o estado documental da cópia; execução ou conclusão atual em produção: **[nao verificado]**.

| Caso | Obra | GRADE / BACKLOG | O que destrava |
|---|---|---|---|
| BO-ADMIN-2710/3 | JUSTIFICATIVA-PAGA-O-PERIODO | GRADE principal INTERROMPIDA; lista posterior PARADA, aguarda juiz. BACKLOG preserva dependência. (app/docs/GRADE-MESTRA.md:270,1758; app/docs/BACKLOG.md:1956) | Pouso de COMPETENCIA-PAGA-UM-JUIZ; certificar árvore/condições do opus, consumo do parcial e DIFF novo. (app/docs/GRADE-MESTRA.md:1758; app/docs/BACKLOG.md:1956) |
| HJ-017-U1 | O232 | REGISTRADA; A/B no ar, complemento da ata pendente. (app/docs/GRADE-MESTRA.md:854; app/docs/BACKLOG.md:1253) | Migrar terceiro leitor, RED das quatro batidas e impacto de frota; colisão documental com JUSTIFICATIVA. (app/docs/BACKLOG.md:1253; app/docs/GRADE-MESTRA.md:854) |
| BO2910-ESTRUTURA/01 | BO2910-ESTRUTURA | REGISTRADA, censo não fechado. (app/docs/GRADE-MESTRA.md:170,7; app/docs/BACKLOG.md:1116) | Censo da classe de duas portas de posto/empresa e exceção engolida. (app/docs/BACKLOG.md:1116) |
| BO2910-ESTRUTURA/02 | BO2910-ESTRUTURA | REGISTRADA, censo não fechado. (app/docs/GRADE-MESTRA.md:170,7; app/docs/BACKLOG.md:1116) | Censo da classe de chamado de texto livre sem leitor da célula. (app/docs/BACKLOG.md:1116) |
| BO2910-ESTRUTURA/05 | BO2910-ESTRUTURA | REGISTRADA, censo não fechado. (app/docs/GRADE-MESTRA.md:170,7; app/docs/BACKLOG.md:1116) | Censo; tela precisa exigir período na aprovação/edição da parcial. (app/docs/BACKLOG.md:1116) |
| BO2910-ESTRUTURA/06 | BO2910-ESTRUTURA | REGISTRADA, censo não fechado. (app/docs/GRADE-MESTRA.md:170,7; app/docs/BACKLOG.md:1116) | Censo da vigência no desligamento e reabertura na reativação. (app/docs/BACKLOG.md:1116) |
| BO2910-ESTRUTURA/08 | BO2910-ESTRUTURA | REGISTRADA, censo não fechado. (app/docs/GRADE-MESTRA.md:170,7; app/docs/BACKLOG.md:1116) | Censo do veredito admin que sobrevive à regeneração com marcos novos. (app/docs/BACKLOG.md:1116) |
| HJ-074 | FERIADO-ESCALA → FERIADO-USA-A-LEI-DE-1609 | GRADE EM VOO, escopo anterior superado; lista posterior PAREI 3(ii). BACKLOG PARADA no desenho G. (app/docs/GRADE-MESTRA.md:127,1798; app/docs/BACKLOG.md:729) | Corte `! FERIADO-G item 3(ii)`; construção permanece parada pela condição medida. (app/docs/GRADE-MESTRA.md:1798; app/docs/RELATO.md:48) |

| Sem obra | Descrição e complemento cabível |
|---|---|
| HJ-005-U1 | col42, 01/10: ingestão acusa irregularidade e retenção por fato em ausência/furo/indefinição. (anexo mapa_hj_sem_nome.tsv:7) Cabe **complemento de censo em BO-ADMIN-2710**, aberta “medindo”, para reconciliar dono e destino; cobertura causal por COBERTURA-PARCIAL não está demonstrada para 01/10. Há ainda destino histórico “decisão do admin” para o recorte 01/09-10, que precisa ser reconciliado. (app/docs/BACKLOG.md:832,841; app/docs/CORTES.md:373) |
| HJ-012-U2 | col239, 12/10: ingestão acusa `espelho_cobrado`. (anexo mapa_hj_sem_nome.tsv:19) Cabe **complemento de

[truncado em 20000 de 31904 caracteres]

_5889 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
