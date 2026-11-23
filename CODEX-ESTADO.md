# CODEX-ESTADO

> Ultima resposta processada: **23/11/2026 16:29** · publicado em **23/11/2026 16:29** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 23/11 16:29 · sessao 348baf761b84 · UI-EDITAR-RAIZ · censo · origem=Code

## 1. Edição da raiz, permissão e aviso

- **A rota do lápis já aceita uma raiz editável.** O GET busca o tipo pelo `tid` e só entra no fluxo “sob pai” quando está criando. O POST busca o mesmo registro e só lê `pai_id` na criação. O serviço atualiza o objeto recebido e o modelo mantém `nivel=0` quando `template_pai` é nulo. A raiz continua com o mesmo PK e sem pai. `app/escala/urls.py:25-27` · `app/escala/views_wizard.py:65-83,98-109` · `app/escala/services/cadastro_tipo.py:140-180,306-308` · `app/escala/models.py:771-775`
- **GET e POST exigem login e `editar_escalas`.** O decorador consulta `colaboradores.rbac_editar_escalas` e admite superusuário. `editavel=False` dá HTTP 400 no GET e recusa no serviço de gravação. A raiz passa pelas validações comuns do modelo e sai antes das validações de filho. O teste HTTP existente edita uma raiz e verifica que o ciclo é preservado. O teste seguinte cobre a recusa de uma raiz de sistema. `app/escala/views_wizard.py:63-70,95-109` · `app/colaboradores/mixins.py:322-346` · `app/escala/services/cadastro_tipo.py:144-150` · `app/escala/models.py:347-383` · `app/escala/tests/test_wizard_views.py:51-78`
- **O mesmo aviso vale para a raiz se houver dias barrados.** A porta pula as datas cobertas pela lavra e devolve a contagem. O juiz da lavra considera TXT entregue ou holerite publicado para o colaborador. `wizard_salvar` soma `_barrados` e acrescenta o aviso à mensagem de sucesso. Em HTMX, a mensagem vai no `HX-Trigger` de um 204 e a lista recarrega imediatamente ao receber o sucesso. A legibilidade do toast antes da recarga ficou [nao verificado]. `app/ponto/portas/celula.py:218-221,284-288,395-400` · `app/folha/export.py:597-628` · `app/escala/views_wizard.py:113-126` · `app/core/respostas.py:63-71` · `app/templates/escala/tipos_lista.html:242-246`

## 2. Campos gravados e nome exibido

- **O “Nome da escala” do wizard é `apelido`, e aparece no rótulo da raiz.** O campo visível envia `apelido`, gravado com até 12 caracteres. A raiz chama `rotulo_escala(t)`, que renderiza `nome_canonico_partes()` com o apelido, ciclo, dias e horários do próprio tipo. `nome_canonico()` é um método. `template_nome` e `descricao` entram no rótulo apenas se `nome_canonico_partes()` lançar exceção. No sucesso do drawer, a recarga consulta novamente os tipos. `app/templates/escala/wizard_tipo_escala.html:66-72` · `app/escala/services/cadastro_tipo.py:184-190` · `app/templates/escala/tipos_lista.html:48-49,242-246` · `app/core/templatetags/hasner_filters.py:64-89,119-136` · `app/escala/models.py:420-425,479-523` · `app/escala/views.py:83,114-115`
- **`template_nome` e `descricao` têm regras próprias.** O POST exige `template_nome` e o grava. No formulário de edição, ele vem oculto e só recebe o apelido se estiver vazio. `descricao` só recebe o nome como preenchimento quando está vazia. O wizard não envia `codigo`. O `save()` gera código se estiver vazio ou começar com `AUTO`. `app/templates/escala/wizard_tipo_escala.html:22,461-482` · `app/escala/views_wizard.py:18-60` · `app/escala/services/cadastro_tipo.py:148-150,184-190` · `app/escala/models.py:761-770`
- **Demais campos.** O serviço grava horários de início e fim, os dois pares de intervalo, duração do intervalo quando numérica, `noturno`, `permite_hora_extra`, extras declaradas antes/depois, calendário padrão, folgas semanais, dias de trabalho e `horarios_por_dia`. Para personalizada, fixa `ciclo_dias=7`. O modelo deriva `intervalo_modo` e `nivel`. A derivação de `minutos_jornada` no serviço roda na criação. `app/escala/services/cadastro_tipo.py:179-182,192-299` · `app/escala/models.py:771-799`

## 3. Herança, geração e DNA

- **Censo de `template_pai.` em produção.** As ocorrências literais estão em `app/escala/models.py:373` (`tipo_ciclo`, validação), `:376` (`descricao` e `tipo_ciclo`, mensagem de erro), `:380` (`nivel`, validação), `:775` (`nivel`, gravação) e `:814` (recursão de `valor_efetivo`). Esse método sobe ao pai quando o valor próprio é `None`, `''` ou `0`. A varredura encontrou apenas a definição e a chamada recursiva. Por alias, o gerador de código hierárquico lê `pai.codigo` ou chama o gerador do pai. `app/escala/models.py:745-759,806-814`
- **No nascimento sob pai**, o serviço grava o ciclo do pai e, no ramo comum, a base do pai. Para personalizada, usa `tipo_base_ref`, preenchido pelo formulário com a base do pai. Horários e pausas são gravados no próprio tipo a partir do payload. A operação de cópia também grava campos do original no novo objeto e conserva a FK ao mesmo pai. `app/escala/services/cadastro_tipo.py:152-178,205-267,397-418` · `app/templates/escala/wizard_tipo_escala.html:19`
- **Editar a raiz não muda por herança o rótulo nem as células futuras das filhas.** A lista lê o rótulo e os horários de `f`. O gerador escolhe o vínculo pela vigência, calcula se é dia de trabalho e monta o DNA a partir de `ec.tipo_escala`, com marcos e pausas desse tipo. `escala/alimentacao` resolve o vínculo pela geradora da célula e pela vigência. `app/templates/escala/tipos_lista.html:72-88` · `app/escala/models.py:281,326-345,479-523,1282,1376-1382` · `app/escala/alimentacao.py:214-216,269-286` · `app/ponto/portas/celula.py:935-953` · `app/ponto/management/commands/gerar_celulas.py:85-113`
- **O DNA gravado é lido da célula.** Sem marcos no DNA, o leitor de marcos recorre ao tipo do próprio vínculo. A edição pode reescrever DNA por regeneração explícita de um vínculo direto, se a célula for `gerada`, estiver fora da lavra e houver diferença. Há uma ressalva no trecho histórico: se um vínculo direto da raiz cobrir um dia cuja célula `gerada` pertence a outro vínculo do mesmo colaborador, o código pode trocar a geradora. O trecho futuro filtra pelo dono do dia. Existência dessa sobreposição em produção [nao verificado]. `app/escala/models.py:1128-1144,1703-1712` · `app/ponto/portas/celula.py:225-229,284-315` · `app/ponto/services/cadastro_futuro.py:43-59`

## 4. Vínculos diretos e regeneração

- **A raiz pode receber vínculo direto.** A FK aceita `TipoEscala` e a porta de vínculo busca o tipo pelo PK e o grava diretamente. Há um caso histórico documentado de vínculo corrigido para `PAI-6X1`, representado no teste por um tipo raiz. Quantidade e situação dos vínculos atuais [nao verificado]. `app/escala/models.py:871-877` · `app/colaboradores/services/vinculo.py:52-54,274-299` · `app/escala/tests/test_cobranca_orfa_regeneracao.py:13-19,122-126,153-155`
- **Toda edição chama a regeneração, inclusive a edição só de nome.** O trecho até hoje seleciona vínculos ativos com `tipo_escala=tipo` exato e começa no maior valor entre o início do vínculo e o início da competência anterior. O trecho futuro considera também vínculos encerrados desse tipo e seleciona células `gerada` já existentes depois de hoje. O nome fica fora do DNA, então a troca de nome por si só não cria diferença no DNA. `app/escala/services/cadastro_tipo.py:306-328,332-378` · `app/ponto/services/cadastro_futuro.py:19-25,43-59` · `app/ponto/management/commands/gerar_celulas.py:85-132` · `app/ponto/portas/celula.py:289-311`
- **A guarda protege as células.** O mesmo fluxo chama `invalidar_fechamento` para a janela histórica, que pode zerar `derivado_em` e atualizar `atualizado_em` de `FechamentoMensal`, sem filtro de lavra nesse `UPDATE`. `app/ponto/portas/celula.py:284-288` · `app/escala/services/cadastro_tipo.py:353-364` · `app/ponto/services/fechamento.py:1648-1656`

## 5. Selos de tela e regra de front

- **A busca literal em `app/*/tests` encontrou três arquivos.** O O74 renderiza a lista em `base.html`, recorta o cabeçalho da raiz e verifica a pílula do desenho. O smoke Chromium clica no lápis da **variante** e verifica o POST de preview e a ausência de gravação. O selo genérico de `window.hx*` varre todos os templates contra captura no carregamento. `filho-linha` teve zero ocorrências nessa busca. `app/escala/tests/test_o74_pilula_le_desenho.py:3,24-28,52-76` · `app/escala/tests/test_smoke_clique_wizard_preview.py:175-183` · `app/core/tests/test_hx_nao_se_captura_no_load.py:18,67-81`
- **Pela rota, o O122 também cobre essa tela.** Ele verifica a casca admin, o atalho “Nova escala”, seus atributos HTMX e o título. O recorte usa o prefixo `Hierarquia de tipos`. O texto atual é `clique no filho para editar`. `app/escala/tests/test_o122_etapa1_barra_em_tipos.py:48-97` · `app/templates/escala/tipos_lista.html:26`
- **A regra de smoke de clique nas duas cascas** enumera `static/js/`, service worker, bases e seus includes. A lista é uma folha da view que estende `base.html` e fica fora do grafo de includes literais das duas bases. A regra geral de selo de front pede renderização das duas cascas ou prova pelo grafo. O selo citado prova `_dialogo.html` pelo grafo, e o smoke Chromium renderiza ambas para verificar overlay e console. `CLAUDE.md:203-204` · `app/templates/escala/tipos_lista.html:1` · `app/escala/views.py:114-115` · `app/core/tests/test_selo_modal_nao_nasce_vazio.py:98-119` · `app/core/tests/test_smoke_chromium.py:172-190`

## 6. Renomear separado

A tabela de URLs de `escala` não tem rota separada de renomear `TipoEscala`. Na lista, a raiz tem o `+` para criar sob pai e a filha tem o lápis do wizard. Há um `reverse('escala:editar_tipo')` no pós-cópia sem rota correspondente na tabela. `app/escala/urls.py:12-39` · `app/templates/escala/tipos_lista.html:42-60,83-94` · `app/escala/views.py:141-151`

Hash Git, execução dos testes e smoke no navegador [nao verificado]. [nao verificado]

## 23/11 16:28 · sessao 824adfd70770 · NOTURNO-NAO-APURADO · censo · origem=Code

# NOTURNO-NAO-APURADO

Censo estático da cópia em modo somente leitura. O anexo identifica as autoridades. Hash Git da cópia, valores de produção, competências afetadas e resultado da suíte [nao verificado]. [nao verificado]

## 1. Escritores

`PeriodoCalculo` tem quatro campos noturnos, todos com default `0.0`: `minutos_noturnos_relogio`, `minutos_noturnos_legais`, `minutos_extra_50_noturna` e `minutos_extra_100_noturna`. `horas_noturnas` é uma propriedade de leitura que divide os minutos legais por 60. `app/ponto/motor_calculo_v2.py:208`, `app/ponto/motor_calculo_v2.py:214`, `app/ponto/motor_calculo_v2.py:224`, `app/ponto/motor_calculo_v2.py:259`

| Campos | Escritas encontradas |
|---|---|
| `minutos_noturnos_relogio` e `minutos_noturnos_legais` | `MotorBase._escrever_noturnas` grava o retorno de `calcular_noturnas`. Os corpos de `calcular_periodo` do 12x36, partido e comercial chamam esse escritor. `app/ponto/motor_calculo_v2.py:994`, `app/ponto/motor_calculo_v2.py:1003`, `app/ponto/motor_calculo_v2.py:2792`, `app/ponto/motor_calculo_v2.py:2896`, `app/ponto/motor_calculo_v2.py:3260` |
| `minutos_extra_50_noturna` e `minutos_extra_100_noturna` | `_aplicar_he_noturna` grava cada parcela quando o retorno correspondente é diferente de zero. É chamado dentro de `calcular_periodo` no 12x36 e comercial. No pós-processamento mensal, partido e comercial zeram as duas parcelas e chamam o mesmo escritor depois de apurar HE. No partido, os sítios são dobra, excedente diário e excedente semanal. No comercial, dobra e excedente diário. `app/ponto/motor_calculo_v2.py:1012`, `app/ponto/motor_calculo_v2.py:1019`, `app/ponto/motor_calculo_v2.py:2855`, `app/ponto/motor_calculo_v2.py:2937`, `app/ponto/motor_calculo_v2.py:3104`, `app/ponto/motor_calculo_v2.py:3160`, `app/ponto/motor_calculo_v2.py:3191`, `app/ponto/motor_calculo_v2.py:3294`, `app/ponto/motor_calculo_v2.py:3336`, `app/ponto/motor_calculo_v2.py:3361`, `app/ponto/motor_calculo_v2.py:3423` |
| Os quatro | `_dia_sem_previsto_fora_da_janela` zera por `setattr` quando incide o bloqueio total da L-097. `calcular_mes` chama esse método no laço principal, antes do bloco de crédito. `app/ponto/motor_calculo_v2.py:1422`, `app/ponto/motor_calculo_v2.py:1440`, `app/ponto/motor_calculo_v2.py:1471`, `app/ponto/motor_calculo_v2.py:2461`, `app/ponto/motor_calculo_v2.py:2511` |
| Relógio e legais, na releitura V3 | `reler` zera esses dois campos em cópia de par fechado duplicado e os reescreve para par elegível quando recebe `cct`. O módulo contém esse escritor. Os chamadores de `reler` localizados no censo estão na bateria de testes. `app/ponto/calculador/periodo.py:182`, `app/ponto/calculador/periodo.py:644`, `app/ponto/calculador/periodo.py:734`, `app/ponto/calculador/periodo.py:748`, `app/ponto/tests/test_o280_bateria_da_lei.py:4300` |

No crédito, `_pc` é calculado para cada par elegível. As duas escritas numéricas em `_abs[0]` são `minutos_trabalhados` e `minutos_fora_janela`. `app/ponto/motor_calculo_v2.py:2549`, `app/ponto/motor_calculo_v2.py:2553`, `app/ponto/motor_calculo_v2.py:2559`

## 2. Leitores

| Leitor | O que faz |
|---|---|
| `ResultadoMes` e `FechamentoMensal` | `horas_noturnas` soma legais em `periodos + periodos_ft`. `horas_extra_50_noturna` e `horas_extra_100_noturna` somam os respectivos campos em `periodos`. O fechamento arredonda os três totais a duas casas e acrescenta à HE 100 noturna a parcela dos períodos de folga promovidos a feriado. `app/ponto/motor_calculo_v2.py:321`, `app/ponto/motor_calculo_v2.py:338`, `app/ponto/services/fechamento.py:918`, `app/ponto/services/fechamento.py:921`, `app/ponto/services/fechamento.py:924`, `app/ponto/services/fechamento.py:962` |
| `DiaPago`, versão `motor` | `lavrar` soma legais e os dois subconjuntos de HE por dia, em horas. Folga com escala certa, sem escala certa e folga promovida a feriado carregam legais. A promoção a feriado também lê `minutos_extra_100_noturna`. `app/ponto/services/dia_pago.py:173`, `app/ponto/services/dia_pago.py:223`, `app/ponto/services/dia_pago.py:234`, `app/ponto/services/dia_pago.py:244`, `app/ponto/services/dia_pago.py:259`, `app/ponto/services/fechamento.py:1201`, `app/ponto/services/fechamento.py:1227` |
| Motor partido, régua `legais` | Usa `minutos_noturnos_legais - minutos_noturnos_relogio` de cada período do dia como acréscimo à base do excedente. É um leitor dos dois campos com efeito possível sobre a HE total. `app/ponto/motor_calculo_v2.py:3083`, `app/ponto/motor_calculo_v2.py:3136`, `app/ponto/motor_calculo_v2.py:3144` |
| Espelho, cartão/PDF e app | `somar_periodos` soma `p.horas_noturnas` e os minutos de relógio convertidos em horas. O espelho calcula `acrescimo_noturno` pela diferença. Na competência, `folha_manda` pode substituir o total legal pelo fechamento ou pela lavratura. Na linha diária, tela e PDF leem `DiaPago.horas_noturnas` via `rubricas_do_dia`. O PDF também consulta o campo legal do período para decidir se desenha a coluna. O app publica `total_noturnas` do resumo. `app/ponto/services/espelho.py:82`, `app/ponto/services/espelho.py:99`, `app/ponto/services/espelho.py:453`, `app/ponto/services/espelho.py:831`, `app/ponto/services/espelho.py:1306`, `app/relatorios/cartao_pela_celula.py:100`, `app/relatorios/cartao_pela_celula.py:176`, `app/relatorios/cartao_pela_celula.py:321`, `app/ponto/services/dia_pago.py:924`, `app/relatorios/pdf_espelho.py:502`, `app/relatorios/pdf_espelho.py:844`, `app/relatorios/pdf_espelho.py:911`, `app/templates/ponto/espelho.html:278`, `app/api/views.py:1431`, `app/api/views.py:1662` |
| TXT Domínio e holerite | O TXT lê `FechamentoMensal.horas_noturnas` no evento `adicional_noturno` (rubrica 25), resolve o código pelo mapa de rubricas e emite a linha. Emite HE pelos totais das faixas, sem linha própria dos subconjuntos noturnos. O módulo de holerite recebe PDF e entrega o arquivo armazenado. Valor de holerite emitido pelo Domínio [nao verificado]. `app/folha/export.py:424`, `app/folha/export.py:439`, `app/folha/export.py:730`, `app/holerite/views.py:1161`, `app/holerite/views.py:1193`, `app/holerite/views.py:1445` |
| `retratar_exportada` | Lê `FechamentoMensal` e a grade da célula. Nas linhas existentes atualiza os três campos de grade e os carimbos. Em dia sem linha cria o retrato com rubricas monetárias em zero. A linha de ajuste copia DSR e banco do fechamento. Esse caminho usa a grade e o gravado como insumo, sem consumir `PeriodoCalculo`. `app/ponto/services/dia_pago.py:451`, `app/ponto/services/dia_pago.py:461`, `app/ponto/services/dia_pago.py:469`, `app/ponto/services/dia_pago.py:488`, `app/ponto/services/dia_pago.py:504`, `app/ponto/services/dia_pago.py:538`, `app/folha/export.py:329` |
| Instrumentos fora de `tests/` | `diff_calculador` soma legais por dia no lado motor. `desvio_o68b` lê HE 100 noturna por período. `diff_extrato_parcial` lê o total legal de `ResultadoMes` e `aplicar_09_he_intra` lê os totais dos dois subconjuntos. O censo estrutural procura referências no código por AST. `app/ponto/management/commands/diff_calculador.py:260`, `app/ponto/management/commands/desvio_o68b.py:114`, `app/relatorios/management/commands/diff_extrato_parcial.py:68`, `app/ponto/management/commands/aplicar_09_he_intra.py:86`, `app/core/medida_leitor_do_dia.py:50`, `app/core/medida_leitor_do_dia.py:70` |

**Re-derivação do aberto.** O oráculo recalcula adicional e subconjuntos a partir dos segmentos fechados publicados e reutiliza as funções do motor. O `PeriodoCalculo` aberto entra como `(entrada, None, dia)` e fica fora dos segmentos fechados e das pontas de HE. O `_pc` do crédito é local ao laço. Assim, o oráculo não reconstrói o noturno desse crédito. A lavratura começa pela soma dos períodos, mas, quando o oráculo fornece a rubrica do dia, seu valor substitui a soma na versão `oraculo`. Essa substituição é o ponto de atenção da L-002. `app/ponto/motor_calculo_v2.py:2553`, `app/ponto/calculador/alimentacao.py:83`, `app/ponto/calculador/alimentacao.py:157`, `app/ponto/calculador/alimentacao.py:525`, `app/ponto/calculador/regras.py:82`, `app/ponto/calculador/regras.py:716`, `app/ponto/calculador/regras.py:824`, `app/ponto/calculador/regras.py:844`, `app/ponto/services/fechamento.py:1265`, `app/ponto/services/fechamento.py:1288`, `app/ponto/services/dia_pago.py:322`, `app/docs/LEIS.md:60`

## 3. Período com `turno_aberto=True`

**Hoje os quatro campos ficam em `0.0`, inclusive com crédito de trabalhado.** Os três construtores retornam antes do cálculo noturno quando `saida is None`. O crédito conserva a geometria aberta e escreve apenas os dois totais de minutos já citados. `app/ponto/motor_calculo_v2.py:214`, `app/ponto/motor_calculo_v2.py:224`, `app/ponto/motor_calculo_v2.py:2751`, `app/ponto/motor_calculo_v2.py:2878`, `app/ponto/motor_calculo_v2.py:3216`, `app/ponto/motor_calculo_v2.py:2559`

- **Pontualidade.** `_aberto or _nulo` zera atraso e saída antecipada de todos os períodos agrupados no dia. `Motor12x36ComEscala` também retorna antes de julgar pontualidade do aberto. Essas guardas preservam os campos noturnos. `app/ponto/motor_calculo_v2.py:1131`, `app/ponto/motor_calculo_v2.py:1156`, `app/ponto/motor_calculo_v2.py:3546`
- **Releitura e apresentação.** `reler` pode zerar a pontualidade, conta o aberto e pula suas demais escritas, preservando o noturno que recebeu. Espelho, PDF e relatório usam o indicador para data, contagem, inconsistência ou rótulo. O crivo de aprovação também registra `turno_aberto` a partir da contagem mensal. `app/ponto/calculador/periodo.py:636`, `app/ponto/calculador/periodo.py:644`, `app/ponto/motor_calculo_v2.py:373`, `app/ponto/services/espelho.py:434`, `app/ponto/services/espelho.py:671`, `app/relatorios/pdf_espelho.py:513`, `app/relatorios/pdf_espelho.py:973`, `app/relatorios/services.py:144`, `app/templates/ponto/espelho.html:194`, `app/folha/export.py:830`
- **O que pode apagar um valor transferido.** Os resets mensais do partido e comercial zerariam os **dois campos de HE noturna** de `_abs[0]`. O adicional legal e o de relógio passam por esses resets. Na versão `oraculo`, a substituição da rubrica pode descartar a contribuição do aberto na **linha do dia**, sem alterar o objeto. `app/ponto/motor_calculo_v2.py:2928`, `app/ponto/motor_calculo_v2.py:2937`, `app/ponto/motor_calculo_v2.py:3329`, `app/ponto/motor_calculo_v2.py:3336`, `app/ponto/services/dia_pago.py:322`

## 4. Retorno de `calcular_periodo(e, s, itv)` e HE noturna

- **Adicional.** Para um par fechado, os três corpos recortam as pontas e os intervalos e escrevem relógio e legais. `calcular_noturnas` calcula a janela e a prorrogação configurada, desconta os intervalos noturnos e aplica o fator da hora reduzida conforme a régua. Plantão delega ao 12x36. 12x36 com escala delega e acrescenta pontualidade. Partido noturno e intermitente herdam seus respectivos corpos. `app/ponto/motor_calculo_v2.py:983`, `app/ponto/motor_calculo_v2.py:990`, `app/ponto/motor_calculo_v2.py:2759`, `app/ponto/motor_calculo_v2.py:2792`, `app/ponto/motor_calculo_v2.py:2886`, `app/ponto/motor_calculo_v2.py:2896`, `app/ponto/motor_calculo_v2.py:3224`, `app/ponto/motor_calculo_v2.py:3260`, `app/ponto/motor_calculo_v2.py:3534`, `app/ponto/motor_calculo_v2.py:3546`, `app/ponto/motor_calculo_v2.py:3560`, `app/ponto/motor_calculo_v2.py:3569`
- **HE noturna.** 12x36 e comercial já podem preencher as duas parcelas no retorno de `_pc`. Partido devolve as duas nos defaults e apura HE no mensal. `he_noturna` usa as pontas brutas guardadas no período, as HE 50/100 já calculadas, os intervalos do motor e a prorrogação. Recorta a cauda extra e reparte o noturno proporcionalmente. O split usa essa geometria e as faixas de HE como entrada. `app/ponto/motor_calculo_v2.py:430`, `app/ponto/motor_calculo_v2.py:440`, `app/ponto/motor_calculo_v2.py:1014`, `app/ponto/motor_calculo_v2.py:2855`, `app/ponto/motor_calculo_v2.py:2896`, `app/ponto/motor_calculo_v2.py:3294`
- **Entrada do aberto na derivação mensal.** Seu trabalhado creditado já entra na soma do dia do partido e do comercial. O aberto pode receber HE total como último período do dia, por dobra, ou como último da semana no partido. O split sobre o próprio aberto retorna antes do cálculo porque `saida` é `None`. No partido com régua `legais`, transferir relógio e legais também alimentaria o termo de ganho da hora reduzida. Se esse ganho alterar a HE do dia e o portador for um período fechado, o split desse portador usa a nova HE. `app/ponto/motor_calculo_v2.py:1012`, `app/ponto/motor_calculo_v2.py:2943`, `app/ponto/motor_calculo_v2.py:3087`, `app/ponto/motor_calculo_v2.py:3097`, `app/ponto/motor_calculo_v2.py:3136`, `app/ponto/motor_calculo_v2.py:3153`, `app/ponto/motor_calculo_v2.py:3181`, `app/ponto/motor_calculo_v2.py:3328`, `app/ponto/motor_calculo_v2.py:3348`, `app/ponto/motor_calculo_v2.py:3354`, `app/ponto/motor_calculo_v2.py:3416`

## 5. Testes existentes [nao verificado]

| Caso | O que a asserção cobre |
|---|---|
| BO-B caso 1 e controles 5/6 | Caso 1 cobra 357 minutos, um aberto, pontualidade e falta em zero, igualdade com o juiz e alerta `pares fechados`. Caso 5 cobra zero trabalhado com entrada única. Caso 6 cobra 440 no dia fechado. Nenhuma asserção de noturno nesses casos. `app/ponto/tests/test_bob_dia_pago_se_contradiz.py:121`, `app/ponto/tests/test_bob_dia_pago_se_contradiz.py:131`, `app/ponto/tests/test_bob_dia_pago_se_contradiz.py:143`, `app/ponto/tests/test_bob_dia_pago_se_contradiz.py:148` |
| BO-B casos 2, 4b e 8 | Caso 2 cobra 438 minutos, zero fora da janela e CxR. Caso 4b cobra 146 trabalhados, 130 fora e 236 no juiz. Caso 8 cobra 351 no motor, 447 no juiz e contadores `(0, 1)` para crédito e pausa dentro de par pago. Nenhuma asserção de noturno. `app/ponto/tests/test_bob_dia_pago_se_contradiz.py:157`, `app/ponto/tests/test_bob_dia_pago_se_contradiz.py:204`, `app/ponto/tests/test_bob_dia_pago_se_contradiz.py:221` |
| BO-B caso 3, noturno | O pinado espera 660 trabalhados e zero fora da janela. O gêmeo cobra 360 no motor, um aberto, juiz `(300, 60, False)` e `_dia_aberto_creditado == 0`. São asserções de trabalhado, geometria e ausência de crédito, sem valor esperado de adicional ou HE noturna. `app/ponto/tests/test_bob_dia_pago_se_contradiz.py:166`, `app/ponto/tests/test_bob_dia_pago_se_contradiz.py:179`, `app/ponto/tests/test_bob_dia_pago_se_contradiz.py:183` |
| Bateria C6 e controles de zero | C6 cobra dois abertos e 300 minutos de um par fechado diurno. Os testes de zero noturno localizados cobram `noturnas_do_dia` com par sem ponta, invertido ou vazio, e `_aplicar_he_noturna` com `saida=None`. Nenhum deles fixa zero noturno em **aberto que recebeu crédito de par fechado**. `app/ponto/tests/test_o280_bateria_da_lei.py:1272`, `app/ponto/tests/test_o280_bateria_da_lei.py:3198`, `app/ponto/tests/test_s5b_regra_noturnas.py:95`, `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:181` |

**O vermelho estrutural está identificado.** A checagem em memória com o próprio `varrer` reconheceu `MotorBase.calcular_mes` como segundo escritor ao inserir uma atribuição direta de minutos legais no crédito. O selo exige que o conjunto contenha somente `MotorBase._escrever_noturnas`. Ele também reconhece `setattr` com nome literal. A suíte Django [nao verificado]. `app/ponto/tests/test_noturno_sitio_unico.py:63`, `app/ponto/tests/test_noturno_sitio_unico.py:76`, `app/ponto/tests/test_noturno_sitio_unico.py:100`, `app/ponto/tests/test_noturno_sitio_unico.py:107`

## 6. Leis e cortes

- **R3 / L-099.** O placar diz que o dia aberto “MANTEM o numero rotulado (a soma dos pares fechados)”, igual em tela, PDF, cartão, app e TXT. O aval BO-B registra R3 + L-084 e veda fabricar batida ou pagar aberto como cheio. `CORTES.md` registra R3 no placar e remete B-B24 ao dia em aberto. `DOSSIES.md` usa o R3 como molde da palavra com número rotulado para dia sem turno pareável. `app/core/placar_estrutural.py:146`, `app/docs/LEIS.md:122`, `app/docs/CORTES.md:352`, `app/docs/CORTES.md:402`, `app/docs/DOSSIES.md:140`, `app/docs/PROMPTS.md:1357`
- **L-115 / L-097 / L-084.** Depois de pausa fechada, “essa batida e a SAIDA do turno pela posicao, qualquer que seja o tipo gravado”. A última E solta sem pausa fechada segue a BUG-144. O que passa do marco é ponta de HE pela L-097, e a L-084 mantém o julgamento pelas duas pontas do dia. No crédito, a cauda só entra se a pausa começa onde termina o par já fechado. Entrada já paga e par invertido ou nulo são descartados. `app/docs/LEIS.md:115`, `app/docs/LEIS.md:116`, `app/docs/LEIS.md:153`, `app/docs/LEIS.md:434`, `app/ponto/motor_calculo_v2.py:2525`, `app/ponto/motor_calculo_v2.py:2535`, `app/ponto/motor_calculo_v2.py:2550`
- **L-073 / L-093.** O teto temporal exige instante ou fato encerrado. O teto aritmético limita `atraso + saída antecipada` do dia ao previsto menos o trabalhado. A guarda do aberto aplica zero às duas rubricas de pontualidade. `app/docs/LEIS.md:100`, `app/docs/LEIS.md:111`, `app/ponto/motor_calculo_v2.py:1106`, `app/ponto/motor_calculo_v2.py:1156`
- **L-002 / L-095 / L-110.** A testemunha lê a mesma autoridade. O dinheiro do dia é lavrado uma vez. A referência do caso esperado é a regra aplicada ao cadastro e às batidas. O juiz registrado do dinheiro do par é `MotorBase.calcular_periodo`. `app/docs/LEIS.md:60`, `app/docs/LEIS.md:113`, `app/docs/LEIS.md:136`, `app/core/juizes.py:1208`, `app/docs/CORTES.md:376`
- **L-092 / L-113 e mão única.** A L-092 registra a proteção histórica do gravado. A L-113 revoga a recusa genérica de recálculo e preserva o TXT anterior, a trilha e o DIFF prévio ao ato de frota. A guarda temporária na cópia é específica de 09/2026 nas empresas que exportaram. Para esta obra, o limite é a instrução do aval relatada no pedido. O corte de mão única inclui ponto, folha e export, e a zona inviolável exige aval explícito para o motor. `app/docs/LEIS.md:125`, `app/docs/LEIS.md:155`, `app/ponto/services/fechamento.py:36`, `app/ponto/services/fechamento.py:539`, `app/ponto/services/fechamento.py:560`, `app/docs/CORTES.md:611`, `CLAUDE.md:143`

## 7. Decisões do construtor e perguntas de lei

- **Decisão de construção.** Acumular relógio e legais dos mesmos `_pc` que passam pelos filtros atuais, na mesma iteração que acumula trabalhado e fora da janela. Fazer a escrita pelo sítio canônico, preservando o selo e o rótulo do período. `minutos_realizados` entra no fechamento e em `DiaPago` pelo mapa da grade, uma fonte distinta do crédito. `app/ponto/motor_calculo_v2.py:994`, `app/ponto/motor_calculo_v2.py:2549`, `app/ponto/tests/test_noturno_sitio_unico.py:107`, `app/ponto/services/fechamento.py:1060`, `app/ponto/services/dia_pago.py:285`
- **Pergunta de lei sobre excedente.** O aval deve mover somente o adicional ou também o excedente do partido com régua `legais`, quando o ganho da hora reduzida do crédito eleva a base? `app/ponto/motor_calculo_v2.py:3136`, `app/ponto/motor_calculo_v2.py:3144`
- **Pergunta de lei sobre os subconjuntos.** Se o aval alcança HE 50/100 noturna, qual parcela deve prevalecer depois da apuração por dia ou semana: a do `_pc` ou a correspondente à HE consolidada? As faixas do par e as do mensal são apuradas em momentos distintos, e o aberto segue sem ponta final para o split. `app/ponto/motor_calculo_v2.py:1012`, `app/ponto/motor_calculo_v2.py:2847`, `app/ponto/motor_calculo_v2.py:3153`, `app/ponto/motor_calculo_v2.py:3181`, `app/ponto/motor_calculo_v2.py:3331`, `app/ponto/motor_calculo_v2.py:3416`
- **Pergunta de escopo sobre o oráculo.** A versão `oraculo` deve incorporar o mesmo crédito nesta obra? Hoje a sua alimentação omite a geometria do `_pc`, e uma rubrica calculada por dia pode sobrepor a soma vinda do motor. `app/ponto/motor_calculo_v2.py:2553`, `app/ponto/calculador/alimentacao.py:83`, `app/ponto/services/fechamento.py:1265`, `app/ponto/services/dia_pago.py:322`
- **Decisão de verificação.** O caso que falta na bateria é um aberto que efetivamente recebe crédito de um par com noturno positivo. O caso noturno existente

[truncado em 20000 de 20426 caracteres]

## 23/11 16:28 · sessao d05e02130b88 · - · - · origem=Ronald (tmux)

**3. Herança e células**

- `template_pai` é uma FK anulável para o próprio `TipoEscala`, com relação reversa `variantes`. As leituras literais de `template_pai.` no código de produção estão em `app/escala/models.py:373` (`tipo_ciclo`, validação), `:376` (`descricao` e `tipo_ciclo`, mensagem de erro), `:380` (`nivel`, validação), `:775` (`nivel`, cálculo ao salvar) e `:814` (recursão de `valor_efetivo`). O fallback de `valor_efetivo` existe para valor próprio `None`, `''` ou `0`. Na busca por esse símbolo, as únicas ocorrências de produção foram a definição e a recursão em `app/escala/models.py:806-814`.
- A geração de código lê `pai.codigo` por alias em `app/escala/models.py:745-746`, quando o filho tem código vazio ou `AUTO` (`:761-770`). Na criação sob pai, o wizard grava o ciclo e a base no filho. A base de uma personalizada chega em `tipo_base_ref`, preenchido pelo wizard com `pai.tipo_base`. Horários e pausas vêm do payload ou dos campos do próprio tipo. `app/escala/services/cadastro_tipo.py:152-178,205-267`, `app/templates/escala/wizard_tipo_escala.html:19`. A cópia duplica campos do original e guarda a mesma FK de pai. `app/escala/services/cadastro_tipo.py:397-418`.
- O rótulo e os horários da filha vêm de `f`, e `nome_canonico_partes()` consulta campos próprios. A edição da raiz não muda o que a filha existente mostra. O cabeçalho do fluxo de **criar sob pai** consulta o rótulo e `template_nome` da própria raiz. `app/templates/escala/tipos_lista.html:72-88`, `app/core/templatetags/hasner_filters.py:64-89`, `app/escala/models.py:479-523`, `app/templates/escala/wizard_tipo_escala.html:53-56`.
- O gerador avalia a vigência e usa o `tipo_escala` do vínculo. `montar_dna` lê marcos, pausas, ciclo, jornada e extras desse tipo. A alimentação escolhe o vínculo pela geradora ou pela vigência. A edição da raiz não altera novas células cujo vínculo aponta para a filha. `app/ponto/portas/celula.py:919,942-953`, `app/ponto/management/commands/gerar_celulas.py:85-113,128-132`, `app/escala/models.py:326-345,1282,1376-1382`, `app/escala/alimentacao.py:269-286`.
- O leitor usa `CelulaDia.trabalha` e os marcos do DNA gravado. Sem marcos no DNA, consulta o tipo do próprio vínculo. A regeneração explícita do wizard seleciona vínculos diretos do tipo editado, sem percorrer `variantes`. Pode reescrever células geradas elegíveis, guardando `dna_anterior`. Para futuras existentes, preserva o dia atribuído pela juíza a outro vínculo. `app/escala/models.py:1098-1107,1128-1144`, `app/escala/services/cadastro_tipo.py:323-328,339-355,370-378`, `app/ponto/portas/celula.py:284-315`, `app/ponto/services/cadastro_futuro.py:43-59`.
- Se o mesmo colaborador tiver vínculo ativo direto na raiz sobreposto ao da filha, o caminho histórico pode substituir a geradora do dia porque valida a cobertura e compara a geradora para decidir se reescreve, sem o filtro de dono do suplemento futuro. Isso é efeito de vínculo sobreposto. A existência dessa combinação nos dados atuais é [nao verificado]. `app/escala/services/cadastro_tipo.py:339-355`, `app/ponto/portas/celula.py:227-229,284-310`, `app/ponto/services/cadastro_futuro.py:50-59`.

## 23/11 16:28 · sessao 284db5438802 · - · - · origem=Ronald (tmux)

**4. Vínculos e regeneração.** A FK permite apontar diretamente para uma raiz. O painel oferece os tipos com `is_template=True` sem filtrar pelo pai. O serviço grava no vínculo o próprio tipo selecionado pelo PK. [app/escala/models.py:871](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/escala/models.py:871) · [app/colaboradores/views.py:270](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/colaboradores/views.py:270) · [app/colaboradores/services/vinculo.py:52](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/colaboradores/services/vinculo.py:52) · [app/colaboradores/services/vinculo.py:274](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/colaboradores/services/vinculo.py:274) · [app/colaboradores/services/vinculo.py:287](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/colaboradores/services/vinculo.py:287)

Há caso histórico registrado em comentário de produção com vínculo direto em `PAI-6X1`. A fixture representa esse destino como raiz e salva esse tipo diretamente no vínculo. Quantidade e estado atuais em produção [nao verificado]. [app/escala/tests/test_cobranca_orfa_regeneracao.py:13](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/escala/tests/test_cobranca_orfa_regeneracao.py:13) · [app/escala/tests/test_cobranca_orfa_regeneracao.py:122](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/escala/tests/test_cobranca_orfa_regeneracao.py:122) · [app/escala/tests/test_cobranca_orfa_regeneracao.py:153](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/escala/tests/test_cobranca_orfa_regeneracao.py:153)

Toda edição chama a regeneração para `tipo_escala=tipo` exato. A primeira janela abrange vínculos ativos, desde a competência anterior ou o início do vínculo, o que for mais recente, até hoje. O suplemento abrange também vínculos encerrados e seleciona células futuras já existentes com `origem='gerada'`, respeitando o vínculo do dia. A porta pula os lavrados e registra os dias barrados. Uma mudança isolada de nome chama a porta, e o nome fica fora do DNA. [app/escala/services/cadastro_tipo.py:306](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/escala/services/cadastro_tipo.py:306) · [app/escala/services/cadastro_tipo.py:332](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/escala/services/cadastro_tipo.py:332) · [app/escala/services/cadastro_tipo.py:370](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/escala/services/cadastro_tipo.py:370) · [app/ponto/services/cadastro_futuro.py:22](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/ponto/services/cadastro_futuro.py:22) · [app/ponto/services/cadastro_futuro.py:43](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/ponto/services/cadastro_futuro.py:43) · [app/ponto/portas/celula.py:284](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/ponto/portas/celula.py:284) · [app/ponto/portas/celula.py:395](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/ponto/portas/celula.py:395) · [app/ponto/management/commands/gerar_celulas.py:85](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/ponto/management/commands/gerar_celulas.py:85)

**6. Renomear.** Não há rota separada no `app escala`. A lista tem `+` na raiz e lápis, cópia e exclusão nas filhas. O `reverse('escala:editar_tipo')` no retorno da cópia referencia um nome ausente da tabela de rotas. O Django Admin expõe o `ModelAdmin` genérico de `TipoEscala`. [app/escala/urls.py:12](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/escala/urls.py:12) · [app/templates/escala/tipos_lista.html:58](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/templates/escala/tipos_lista.html:58) · [app/templates/escala/tipos_lista.html:83](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/templates/escala/tipos_lista.html:83) · [app/escala/views.py:149](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/escala/views.py:149) · [app/escala/admin.py:7](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/escala/admin.py:7)

## 23/11 16:28 · sessao 0a10596158c5 · - · - · origem=Ronald (tmux)

## 5. Selos e regra de front

- A busca literal por `tipos_lista`, `wizard_tipo_editar` e `filho-linha` em `app/*/tests` encontrou três arquivos. O O74 renderiza `/escala/tipos/` na casca `base.html`, recorta o cabeçalho da raiz e verifica a pílula do desenho. O smoke do wizard clica no lápis da variante e verifica o POST de **preview**. O terceiro teste varre todos os templates para detectar captura de `window.hx*` no carregamento. `filho-linha` teve zero ocorrências nesse recorte. [test_o74_pilula_le_desenho.py](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/escala/tests/test_o74_pilula_le_desenho.py:24) · [test_o74_pilula_le_desenho.py](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/escala/tests/test_o74_pilula_le_desenho.py:52) · [test_smoke_clique_wizard_preview.py](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/escala/tests/test_smoke_clique_wizard_preview.py:175) · [test_hx_nao_se_captura_no_load.py](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/core/tests/test_hx_nao_se_captura_no_load.py:67)
- Pela URL, também há o O122, que verifica a casca, o atalho «Nova escala», os atributos HTMX e o título. O recorte do título depende da expressão `Hierarquia de tipos`. O texto atual continua com `clique no filho para editar`. O O122 mede o estilo pelo Chromium e cobra console limpo. [test_o122_etapa1_barra_em_tipos.py](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/escala/tests/test_o122_etapa1_barra_em_tipos.py:48) · [test_o122_etapa1_barra_em_tipos.py](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/escala/tests/test_o122_etapa1_barra_em_tipos.py:84) · [test_o122_etapa1_barra_em_tipos.py](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/escala/tests/test_o122_etapa1_barra_em_tipos.py:140) · [tipos_lista.html](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/templates/escala/tipos_lista.html:26)
- `FRONT SEM SMOKE NÃO SOBE` exige smoke de clique do Ronald nas duas cascas para push de `static/js/`, service worker, bases ou seus includes. `tipos_lista.html` é uma folha renderizada pela view e estende literalmente `base.html`. A travessia dos includes literais pelo método do selo confirmou que nenhuma das bases inclui essa folha. A regra geral de selo de front permite comprovar as duas cascas pelo grafo de includes. O selo citado usa o grafo para `_dialogo.html`, e o smoke Chromium abre ambas as cascas para verificar overlay e console. [CLAUDE.md](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/CLAUDE.md:203) · [tipos_lista.html](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/templates/escala/tipos_lista.html:1) · [views.py](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/escala/views.py:114) · [test_selo_modal_nao_nasce_vazio.py](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/core/tests/test_selo_modal_nao_nasce_vazio.py:98) · [test_smoke_chromium.py](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/core/tests/test_smoke_chromium.py:172)

O teste HTTP de edição existente usa uma raiz: persiste `template_nome`, preserva `tipo_ciclo` e espera 302. O teste seguinte usa raiz `editavel=False` e confirma que o POST preserva o nome. [test_wizard_views.py](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/escala/tests/test_wizard_views.py:51)

## 23/11 16:20 · sessao 283dd0d8704d · NOTURNO-NAO-APURADO · sonda · origem=Code

```python
import logging
import sys
from collections import Counter, defaultdict
from datetime import date, datetime, time, timedelta
from unittest.mock import patch

sys.dont_write_bytecode = True

from django.db import connection, transaction
from django.utils import timezone
from django_tenants.utils import schema_context

from colaboradores.models import Colaborador
from ponto.janelas import janela_atual
from ponto.models import DiaPago
from ponto.services import dia_pago
from ponto.services.fechamento import recalcular_fechamento_mes
from ponto.turnos import batidas_apuraveis


INI = date(2026, 10, 1)
ATE = date(2026, 11, 22)
COLABS = (37, 113, 174, 218, 868, 382)
UM_DIA = timedelta(days=1)
H22, H05 = time(22), time(5)
ALERTA_CREDITO = "contados dos pares fechados"
ARGUMENTOS_PERIODOS = ("periodos", "ft_certa", "ft_feriado", "ft_sem_escala")
FLAGS_PERIODO = ("turno_aberto", "folga_trabalhada", "hora_extra_100")
NV = "[nao verificado]"


def alvos_por_competencia(colab):
    # C1 do anexo g3.py. Competencia em app/ponto/janelas.py:10,60.
    inicio = timezone.make_aware(datetime.combine(INI, H22))
    fim = timezone.make_aware(datetime.combine(ATE + UM_DIA, H05))
    dias_com_batida = set()
    batidas = batidas_apuraveis(colab, inicio, fim).values_list("timestamp", flat=True)
    for timestamp in batidas.iterator():
        local = timezone.localtime(timestamp)
        if local.time() >= H22:
            dias_com_batida.add(local.date())
        elif local.time() < H05:
            dias_com_batida.add(local.date() - UM_DIA)

    linhas = DiaPago.objects.filter(
        colaborador_id=colab.pk, data__range=(INI, ATE),
        tipo=DiaPago.TIPO_DIA, versao="motor",
        minutos_realizados__gt=0, horas_noturnas=0,
    ).values_list("data", "mes", "ano", "minutos_realizados", "horas_noturnas")

    alvos = defaultdict(dict)
    for dia, mes, ano, realizado, noturno_h in linhas:
        if dia in dias_com_batida:
            _, fim_comp = janela_atual(dia, colab.empresa)
            if (ano, mes) == (fim_comp.year, fim_comp.month):
                alvos[ano, mes][dia] = (realizado, noturno_h * 60)
    return alvos


def capturar_lavratura(cid, mes, ano):
    capturas = []

    def capturar(colab, mes_arg, ano_arg, *, versao, **dados):
        if versao == "motor":
            capturas.append(((colab.pk, mes_arg, ano_arg), dados))
        return 0

    # O alias e importado dentro da funcao. app/ponto/services/fechamento.py:1191,1200,1277.
    with patch.object(dia_pago, "lavrar", new=capturar):
        recalcular_fechamento_mes(
            mes, ano, colaborador_ids=[cid],
            somente_leitura=True, lavrar_dias_pagos=True,
        )

    if not capturas:
        return None
    if len(capturas) != 1 or capturas[0][0] != (cid, mes, ano):
        raise RuntimeError(f"Captura inesperada para col{cid:03d} {mes:02d}/{ano}")
    return capturas[0][1]


def hora_local(instante):
    return timezone.localtime(instante).strftime("%H:%M:%S") if instante is not None else "-"


def ramo_do_dia(periodos, realizado, noturno):
    if not periodos:
        return "sem_periodo"
    if noturno != 0:
        return "outro:periodo_com_noturno"

    positivos = [(p, credito) for _, p, credito in periodos if p.minutos_trabalhados > 0]
    if not positivos:
        return "outro:periodos_sem_trabalhado"

    creditados = [p for p, credito in positivos if p.turno_aberto and credito]
    if creditados:
        # Grade inteira e arredondamento do proprio alerta.
        # app/escala/services/leitor_celula.py:422,423
        # app/ponto/motor_calculo_v2.py:2559,2563
        if realizado is None:
            return "outro:credito_com_realizado_ausente"
        if len(positivos) == 1 and realizado > 0 and round(creditados[0].minutos_trabalhados) == realizado:
            return "credito_turno_aberto"
        return "outro:credito_nao_cobre_sozinho_o_realizado"

    if all(not p.turno_aberto and p.saida is not None for p, _ in positivos):
        return "periodo_fechado_sem_noturno"
    return "outro:trabalhado_em_aberto_sem_alerta_de_credito"


def emitir(colab_rotulo, alvos, lavra, contagem):
    por_dia = defaultdict(list)
    if lavra is not None:
        mapa = lavra["dia_da_jornada"]
        for argumento in ARGUMENTOS_PERIODOS:
            for p in lavra[argumento]:
                dia = dia_pago._dia_de(p, mapa)  # app/ponto/services/dia_pago.py:174,235,245,260
                if dia in alvos:
                    credito = any(ALERTA_CREDITO in alerta for alerta in p.alertas)
                    por_dia[dia].append((argumento, p, credito))

    for dia, (gravado, noturno_gravado) in sorted(alvos.items()):
        prefixo = f"{colab_rotulo} {dia}"
        if lavra is None:
            trabalhado = noturno = realizado_saida = NV
            ramo = "outro:lavratura_motor_nao_capturada"
        else:
            periodos = por_dia[dia]
            for argumento, p, credito in periodos:
                flags = [campo for campo in FLAGS_PERIODO if getattr(p, campo)]
                print(
                    f"PERIODO {prefixo} argumento={argumento} "
                    f"entrada={hora_local(p.entrada)} saida={hora_local(p.saida)} "
                    f"turno_aberto={p.turno_aberto} minutos_trabalhados={p.minutos_trabalhados} "
                    f"minutos_noturnos_legais={p.minutos_noturnos_legais} "
                    f"minutos_fora_janela={p.minutos_fora_janela} "
                    f"alerta_contados_dos_pares_fechados={credito} lista_de_ramos={flags}"
                )
            trabalhado = sum(p.minutos_trabalhados or 0 for _, p, _ in periodos)
            noturno = sum(p.minutos_noturnos_legais or 0 for _, p, _ in periodos)
            realizado = (lavra["realizado_por_dia"] or {}).get(dia)
            realizado_saida = NV if realizado is None else realizado
            ramo = ramo_do_dia(periodos, realizado, noturno)

        print(
            f"DIA {prefixo} DiaPago_minutos_realizados={gravado} "
            f"DiaPago_horas_noturnas_x60={noturno_gravado} "
            f"soma_trabalhados={trabalhado} soma_noturnos={noturno} "
            f"realizado_por_dia={realizado_saida} RAMO={ramo}"
        )
        contagem[ramo] += 1


def main():
    contagem = Counter(dict.fromkeys((
        "credito_turno_aberto", "periodo_fechado_sem_noturno", "sem_periodo",
    ), 0))
    nivel_anterior = logging.root.manager.disable
    logging.disable(sys.maxsize)  # app/ponto/services/fechamento.py:1305,1306
    try:
        with schema_context("juliani"), transaction.atomic(durable=True):
            with connection.cursor() as cursor:
                cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ")
                cursor.execute("SET TRANSACTION READ ONLY")
                cursor.execute(
                    "SELECT current_database(), current_schema(), "
                    "current_setting('transaction_read_only')"
                )
                if cursor.fetchone() != ("sombra", "juliani", "on"):
                    raise RuntimeError("A sonda exige sombra/juliani em READ ONLY")

            colabs = Colaborador.objects.filter(pk__in=COLABS).select_related("empresa").order_by("pk")
            for colab in colabs:
                for (ano, mes), alvos in sorted(alvos_por_competencia(colab).items()):
                    lavra = capturar_lavratura(colab.pk, mes, ano)
                    emitir("col%03d" % colab.pk, alvos, lavra, contagem)
    finally:
        logging.disable(nivel_anterior)

    for ramo, total in sorted(contagem.items()):
        print(f"TOTAL RAMO={ramo} n={total}")
    print("FIM")


main()
```

`recalcular_por_evento` chama `recalcular_fechamento_mes`, que chama `apurar_por_fatia`. A montagem passa por `get_motor_cct` e pela fábrica `motor_calculo_v2.get_motor`. Cada fatia roda `motor.calcular_mes`, e o fechamento concatena os períodos e mescla `dia_da_jornada` antes de chamar `lavrar(periodos=resultado.periodos, ...)`. A sonda captura essa chamada e lê `realizado_por_dia` do mesmo conjunto de argumentos, vindo de `por_dia_da_grade`. `app/ponto/services/fechamento.py:1810`, `:735–756`, `:279–316`, `:1060–1061`, `:1200–1227`, `app/core/regua_cct.py:428–479`, `app/ponto/motor_calculo_v2.py:3626`.

`lista_de_ramos` imprime os nomes dos campos booleanos verdadeiros do `PeriodoCalculo`. Para rotular `credito_turno_aberto`, a sonda exige que o período aberto com o alerta seja o único com trabalhado positivo e que seus minutos arredondados, na precisão do próprio alerta, coincidam com o realizado da grade. A execução em Sombra e o ramo histórico de cada dia estão [nao verificado]. `app/ponto/motor_calculo_v2.py:236–253`, `:2553–2563`, `app/escala/services/leitor_celula.py:422–423`.

A transação `READ ONLY` protege a sonda. O lançador `--rodar` desta cópia executa `lavra_de_prod` antes de iniciar o script, com `update_or_create` em Sombra, e grava a saída com `tee`. `bin/sombra.sh:136–167`, `:483–493`.

## 23/11 15:55 · sessao f539a8e990a7 · PAR-PELO-TURNO · leitura · origem=Code

**Os dois campos são gravados por `ponto.services.dia_pago.lavrar`, mas chegam de listas diferentes.** O fechamento passa `realizado_por_dia` da grade e, separadamente, os períodos apurados pelo motor. `lavrar` monta a linha com `versao='motor'` e a persiste com `bulk_create`. (`app/ponto/models.py:281,327,351`, `app/ponto/services/fechamento.py:1200–1227`, `app/ponto/services/dia_pago.py:335–360`)

## De onde vem cada número

| Campo | Escritor e origem |
|---|---|
| `minutos_realizados` | `lavrar` copia o mapa em `app/ponto/services/dia_pago.py:288–292`. O fechamento obtém o mapa com `grade_do_fechamento` e `por_dia_da_grade`, que considera dias de `trabalho` ou `ausencia` e aplica `min(realizado, previsto)` por dia. A grade lê o número já gravado em `CelulaDia.ata`. (`app/ponto/services/fechamento.py:1013–1014,1060–1064,1212–1213`, `app/folha/export.py:329–338`, `app/escala/services/leitor_celula.py:379,423`, `app/ponto/services/dia_pago.py:386–402`) |
| `horas_noturnas` | `lavrar` soma `p.minutos_noturnos_legais / 60` por dia da jornada, nos períodos normais e nos três grupos de folga/feriado que recebe. O número nasce em `MotorBase.calcular_noturnas`, que mede a interseção do **par efetivo** com a janela noturna, desconta os intervalos noturnos e aplica o fator da régua. `_escrever_noturnas` grava o resultado no `PeriodoCalculo`. (`app/ponto/services/dia_pago.py:126–132,173–179,223–265`, `app/ponto/services/fechamento.py:1203–1204,1217`, `app/ponto/motor_calculo_v2.py:82–118,969–1005,2759–2765,2792,2896,3260`) |

**Onde o pareamento diverge**

- **Grade que alimenta `minutos_realizados`.** Na configuração padrão da cópia, o cartório monta a grade por turno. O montador chama `turnos_do_colab` com as lâmpadas da montagem atual. Essa função carrega as batidas apuráveis do colaborador, sem retratadas, com dois dias de margem antes e um depois. Pareia por segmento de vínculo com `parear_turnos`. `realizado_dos_turnos` seleciona por `data_turno` e soma até `_fim_fechado`, inclusive quando o turno segue aberto. O cartório leva esse número à ata. A configuração efetiva em produção é [nao verificado]. (`app/config/settings/base.py:165`, `app/escala/utils.py:1889–1909,2052–2069,2095–2101`, `app/ponto/turnos.py:1834–1847,1867–1871,1944,1977–2000,609–622,719–771,774–793`, `app/ponto/services/cartorio.py:759,1199–1204,1898–1901,1920–1922`)
- **Motor que alimenta `horas_noturnas`.** O fechamento carrega `batidas_apuraveis`, recorta por fatia de vínculo e chama `calcular_mes`. O motor remove duplicatas e chama `montar_periodos`. `turnos_via_autoridade` chama o mesmo `parear_turnos` com os parâmetros `AUT_*` do motor. O adaptador emite `(entrada, saída ou None)` e descarta a saída órfã como período. Depois, `_periodos_pelo_marco` pode substituir os períodos do dia pelas bordas da ata, se elas passarem pelas guardas. A lista `_turnos_do_juiz` que o motor monta à parte serve ao mapa de dias e ao crédito do turno aberto. (`app/ponto/turnos.py:2896–2904`, `app/ponto/services/fechamento.py:244–262,286–291,623–633`, `app/ponto/motor_calculo_v2.py:453–531,558–576,611–645,669–688,867–895,2346–2354,2396,2511–2527`, `app/ponto/adaptador_motor.py:28–45`)

Há também o escritor `retratar_exportada`, cujo padrão é `versao='motor'`. Ele atualiza `minutos_realizados` pela mesma grade. Nas linhas existentes, a atualização deixa `horas_noturnas` com o valor anterior. Nas linhas que cria, inicializa a rubrica em zero. (`app/ponto/services/dia_pago.py:405,461–495,533–545`)

## E 19:00 → S 01:07 → E 01:2x

O trecho até `01:07` tem **367 minutos de relógio**, dos quais **187** caem entre `22:00` e `01:07`, tomando os horários fornecidos no segundo zero. O destino desses 187 minutos depende de como o motor representa a `S 01:07`. (`app/ponto/turnos.py:605–622,734–741`, `app/ponto/motor_calculo_v2.py:98–118`)

| Ramo do código | `minutos_realizados` de D | `horas_noturnas` de D |
|---|---|---|
| **`01:07` fecha um período do motor.** A `E 01:2x` fica sem saída. | O trecho fechado de 367 min entra no realizado se a geometria da grade o atribui a D. A grade desconta eventual pausa aplicável, e o escritor limita o resultado ao previsto. (`app/ponto/turnos.py:719–771,1491–1498`, `app/ponto/services/dia_pago.py:386–402`) | Com o par efetivo íntegro e sem pausa noturna descontável, grava **`187 / 52,5 ≈ 3,5619 h`**, ou **`187 / 60 ≈ 3,1167 h`** com a hora reduzida afastada. A entrada aberta contribui zero. (`app/ponto/motor_calculo_v2.py:46,214–215,983–1005,2750–2765,2792`, `app/ponto/services/dia_pago.py:173–179`) |
| **`01:07` e `01:2x` são absorvidas como pausa e o período do motor fica aberto.** | `_fim_fechado` recupera `01:07` dentro do turno aberto. O realizado parte dos mesmos 367 min, sujeito ao desconto de pausa aplicável e ao limite do previsto. (`app/ponto/turnos.py:609–622,734–771,1356–1375`, `app/ponto/services/dia_pago.py:386–402`) | **`0,0 h` desse período.** `calcular_periodo` retorna com os minutos noturnos zerados quando `saída=None`. O crédito posterior recalcula o trecho fechado, mas só transfere `minutos_trabalhados` e `minutos_fora_janela` para o período aberto. `lavrar` lê `minutos_noturnos_legais` desse período. **É aqui que o primeiro trecho pode entrar em `minutos_realizados` e ficar fora de `horas_noturnas`.** (`app/ponto/motor_calculo_v2.py:214–215,2750–2755,2877–2882,3215–3220,2541–2560`, `app/ponto/services/dia_pago.py:179`) |

**Com uma `S` posterior barrada pelo teto**, `parear_turnos` mantém a entrada corrente aberta e separa a `S` como órfã. O adaptador não cria período para ela. Se o primeiro trecho permanece fechado separadamente, vale o primeiro ramo da tabela. Se ele só existe dentro do turno aberto, vale o segundo. O teste do teto usa o tempo desde a **entrada corrente** e as bordas disponíveis para ela. Com as duas pontas a mais de três horas dos marcos, o fecho passa a usar o envelope de 26 horas. (`app/ponto/turnos.py:66–82,1083–1095,1430–1457`, `app/ponto/motor_calculo_v2.py:1931,2541–2560`, `app/ponto/adaptador_motor.py:28–44`)

**Qual ramo ocorreu e quais valores ficaram gravados para D:** [nao verificado]. O anexo aponta as funções de geometria e cálculo. A ata de D, seus marcos, o previsto, a régua e o instante da eventual `S` posterior são [nao verificado]. (`app/docs/JUIZES.md:20–29`)

## Leitores de `horas_noturnas` fora do motor

| Saída | Leitura |
|---|---|
| **Espelho diário, calendário e PDF diário** | `rubricas_do_dia` lê `DiaPago.horas_noturnas`. O espelho entrega `dia.rubricas.noturnas` ao HTML. O calendário chama o mesmo helper. O PDF lê a rubrica para a coluna `NOTURNO`. (`app/ponto/services/dia_pago.py:901–925`, `app/ponto/services/espelho.py:831`, `app/templates/ponto/espelho.html:278–279`, `app/colaboradores/services/calendario.py:688–689`, `app/relatorios/pdf_espelho.py:844,857–858,910–913`) |
| **Total do espelho, PDF e app** | `somar_periodos` lê `p.horas_noturnas`. Para o resumo da competência, `folha_manda` aplica o total do fechamento lido por `eventos_do_fechamento`, ou a soma do `DiaPago` quando a lavratura é a fonte disponível. HTML, PDF e API consomem `total_noturnas`. (`app/ponto/services/espelho.py:99,1320`, `app/relatorios/cartao_pela_celula.py:69–76,90–100,142–176,321–346`, `app/templates/ponto/espelho.html:59`, `app/relatorios/pdf_espelho.py:501–502,628,782`, `app/api/views.py:1361–1366,1431,1662`) |
| **Extrato parcial e CSV** | Soma `DiaPago.horas_noturnas` no intervalo via `soma_do_periodo`. A view entrega o valor à tela e ao CSV. (`app/ponto/services/dia_pago.py:34–35,784–789`, `app/relatorios/views.py:481–487,524,551–562`, `app/templates/relatorios/extrato_parcial.html:74,92`) |
| **Horas do mês, fechamento e painel** | O relatório mensal lê `FechamentoMensal.horas_noturnas` no CSV, no total e no HTML. A linha e o painel de fechamento leem o mesmo atributo. A apuração individual de folha também o expõe pela API de mensageria. (`app/relatorios/views.py:94,108`, `app/templates/relatorios/horas_mes.html:68`, `app/templates/ponto/_fech_linha.html:31`, `app/templates/ponto/partials/painel_fechamento.html:33`, `app/folha/services/ranking_he.py:225`, `app/api/views_mensageria.py:1661–1669`) |
| **TXT Domínio, prévia e PDF de validação** | `eventos_do_fechamento` lê `fech.horas_noturnas` como `adicional_noturno`. `montar_linhas_export` resolve a rubrica cadastrada e emite a linha. A prévia e o PDF de validação usam esse mesmo montador. O gate de homologação também lê o evento. (`app/folha/export.py:447,657,730–739,1002–1012`, `app/folha/views.py:270`, `app/folha/validacao_pdf.py:66`) |

O campo também é lido na comparação do cartão com o TXT e na comparação da soma lavrada com o total do espelho. `pago_do_dia` o consulta entre as rubricas que indicam se há valor de trabalho sustentado pela linha, e a fotografia E4B captura o campo junto com os demais campos do modelo. (`app/relatorios/cartao_pela_celula.py:249–268`, `app/folha/porta_export.py:245–274`, `app/ponto/services/dia_pago.py:861–864,890–898`, `app/chamados/services/foto_e4b.py:247–252,289–290`)

## 23/11 15:53 · sessao 61a10b6c2207 · PAR-PELO-TURNO · leitura · origem=Code

```python
from collections import Counter, defaultdict
from copy import copy
from datetime import date, datetime, time, timedelta
from itertools import groupby
from types import SimpleNamespace

from django.db import connection, transaction
from django.utils import timezone
from django_tenants.utils import schema_context

from colaboradores.models import Colaborador
from escala.alimentacao import celulas_do_periodo, escalas_do_periodo, vinculo_do_dia
from escala.servico_jornada import eh_turno_partido_do_tipo
from escala.utils import marcos_dna_periodo
from folha.export import competencia_entregue
from ponto import motor_calculo_v2 as motor
from ponto.janelas import janela_atual
from ponto.models import DiaPago
from ponto.turnos import batidas_apuraveis, intervalos_do_turno

INI = date(2026, 10, 1)
ATE = date(2026, 11, 22)
UM_DIA = timedelta(days=1)
H22 = time(22)
H05 = time(5)

ROTULOS = {
    "C1": (
        "DiaPago dia/motor da competencia de D com minutos_realizados > 0 "
        "E horas_noturnas == 0 E ao menos uma batida apuravel em hora local "
        "em [22:00,24:00) de D ou [00:00,05:00) de D+1"
    ),
    "C2": (
        "DiaPago dia/motor da competencia de D com minutos_realizados > 0 "
        "E horas_noturnas == 0 E ao menos um turno fechado da autoridade "
        "com data_turno = D E minutos_noturnos liquidos dos intervalos > 0"
    ),
}


def dias_entre(ini, fim):
    return (ini + i * UM_DIA for i in range((fim - ini).days + 1))


def perfil_do_vinculo(ec, celulas):
    tipo = ec.tipo_escala if ec is not None else None
    classe = motor.Motor12x36
    if tipo is not None:
        if tipo.tipo_ciclo == "intermitente":
            classe = motor.MotorIntermitente
        else:
            classe = motor.MOTOR_POR_BASE[tipo.tipo_base]
            if (
                issubclass(classe, motor.MotorTurnoPartido)
                and tipo.hora_inicio is not None
                and tipo.hora_fim is not None
                and not eh_turno_partido_do_tipo(tipo)
            ):
                # Comercial e 12x36ComEscala herdam os mesmos AUT_*.
                classe = motor.MotorBase

    return SimpleNamespace(
        escala=tipo,
        _ec=ec,
        celulas_alimentadas=celulas,
        AUT_MARCOS_INTERVALO=classe.AUT_MARCOS_INTERVALO,
        AUT_INTRA_MAX_S=classe.AUT_INTRA_MAX_S,
        AUT_CONT_MAX_S=classe.AUT_CONT_MAX_S,
        DUPLICATA=classe.DUPLICATA,
    )


def noturno_por_dia(dias, vinculos, celulas, batidas_por_data, candidatos):
    resultado = {}
    for ec, grupo in groupby(dias, key=vinculos.__getitem__):
        fatia = tuple(grupo)
        alvos = candidatos.keys() & set(fatia)
        if not alvos:
            continue

        perfil = perfil_do_vinculo(ec, celulas)
        batidas = [
            copy(b)
            for d in dias_entre(fatia[0] - UM_DIA, fatia[-1] + UM_DIA)
            for b in batidas_por_data.get(d, ())
        ]
        batidas_limpas = motor.MotorBase.remover_duplicatas(perfil, batidas)

        for turno in motor.turnos_via_autoridade(perfil, batidas_limpas):
            d = turno.data_turno
            if d not in alvos or turno.aberto or turno.entrada is None or turno.saida is None:
                continue

            entrada, saida = turno.entrada.timestamp, turno.saida.timestamp
            bruto = motor.minutos_noturnos(entrada, saida, prorrogacao=True)
            pausa = motor.minutos_noturnos_dos_intervalos(
                entrada, saida, intervalos_do_turno(turno), prorrogacao=True
            )
            liquido = bruto - pausa
            if liquido > 0:
                resultado[d] = max(liquido, resultado.get(d, 0.0))
    return resultado


def marcos_da_celula(cid, d, ec, celulas):
    celula = celulas.get((cid, d))
    if celula is None or not (celula.dna or {}).get("marcos"):
        return ()
    if ec is not None:
        return ec.marcos_do_dia(d, celulas=celulas)
    return marcos_dna_periodo(cid, d, d, celulas=celulas).get(d, ())


def contar():
    dias = tuple(dias_entre(INI, ATE))
    inicio_batidas = timezone.make_aware(datetime.combine(INI - UM_DIA, time.min))
    fim_batidas = timezone.make_aware(datetime.combine(ATE + UM_DIA, time.max))
    competencias = {}
    exportadas = {}
    contadores = {codigo: Counter() for codigo in ROTULOS}
    quebras = {"marcos=sim", "marcos=nao"}
    amostras = []
    total_colabs = 0
    total_dias_pagos = 0

    colabs = (
        Colaborador.objects.select_related("empresa")
        .only("id", "empresa", "empresa__dia_inicio_competencia")
        .order_by("pk")
    )
    for colab in colabs.iterator(chunk_size=500):
        total_colabs += 1
        cid, empresa_id = colab.pk, colab.empresa_id
        quebra_empresa = f"empresa_id={empresa_id}"
        quebras.add(quebra_empresa)

        if empresa_id not in competencias:
            competencias[empresa_id] = {
                d: janela_atual(d, colab.empresa)[1] for d in dias
            }
        fim_comp = competencias[empresa_id]

        linhas = DiaPago.objects.filter(
            colaborador_id=cid, data__range=(INI, ATE),
            tipo=DiaPago.TIPO_DIA, versao="motor",
        ).values_list("data", "mes", "ano", "minutos_realizados", "horas_noturnas")
        candidatos = {}
        for d, mes, ano, realizado, noturno in linhas:
            if (mes, ano) == (fim_comp[d].month, fim_comp[d].year):
                total_dias_pagos += 1
                if realizado > 0 and noturno == 0:
                    candidatos[d] = realizado
        if not candidatos:
            continue

        batidas_por_data = defaultdict(list)
        dias_relogio = set()
        for batida in batidas_apuraveis(colab, inicio_batidas, fim_batidas):
            local = timezone.localtime(batida.timestamp)
            d, hora = local.date(), local.time()
            batidas_por_data[d].append(batida)
            if hora >= H22:
                dias_relogio.add(d)
            elif hora < H05:
                dias_relogio.add(d - UM_DIA)
        if not batidas_por_data:
            continue

        celulas = celulas_do_periodo([cid], INI, ATE)
        escalas = escalas_do_periodo(
            cid, INI, ATE, mais={c.escala_geradora_id for c in celulas.values()}
        )
        vinculos = {d: vinculo_do_dia(cid, d, escalas, celulas=celulas) for d in dias}
        pares = noturno_por_dia(dias, vinculos, celulas, batidas_por_data, candidatos)

        for d in sorted(candidatos.keys() & (dias_relogio | pares.keys())):
            ec = vinculos[d]
            tipo = ec.tipo_escala if ec is not None else None
            base = tipo.tipo_base if tipo is not None else "sem_vinculo"
            ciclo = tipo.tipo_ciclo if tipo is not None else "sem_vinculo"
            marcos = marcos_da_celula(cid, d, ec, celulas)
            toca = any(h is not None and (h >= H22 or h < H05) for h in marcos)

            mes, ano = fim_comp[d].month, fim_comp[d].year
            chave_exportacao = (empresa_id, mes, ano)
            if chave_exportacao not in exportadas:
                exportadas[chave_exportacao] = competencia_entregue(*chave_exportacao)

            chaves = (
                quebra_empresa,
                f"tipo_base={base} tipo_ciclo={ciclo}",
                f"marcos={'sim' if toca else 'nao'}",
                f"competencia={mes:02d}/{ano} exportada={'sim' if exportadas[chave_exportacao] else 'nao'}",
            )
            quebras.update(chaves)
            if d in dias_relogio:
                contadores["C1"].update(("TOTAL", *chaves))
            if d in pares:
                contadores["C2"].update(("TOTAL", *chaves))
                if len(amostras) < 40:
                    texto_marcos = ",".join(
                        h.isoformat(timespec="minutes") if h is not None else "-"
                        for h in marcos
                    ) or "-"
                    amostras.append(
                        f"AMOSTRA C2 col{cid:03d} {d} {base} {ciclo} "
                        f"{texto_marcos} {pares[d]} {candidatos[d]}"
                    )

    return total_colabs, total_dias_pagos, contadores, sorted(quebras), amostras


def main():
    with schema_context("juliani"), transaction.atomic(durable=True):
        with connection.cursor() as cursor:
            cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ")
            cursor.execute("SET TRANSACTION READ ONLY")
            cursor.execute(
                "SELECT current_database(), current_schema(), "
                "current_setting('transaction_read_only')"
            )
            if cursor.fetchone() != ("sombra", "juliani", "on"):
                raise RuntimeError("A medicao exige sombra/juliani em READ ONLY")

        colabs, dias_pagos, contadores, quebras, amostras = contar()

    print(f"UNIVERSO {INI}..{ATE} colabs={colabs} dias_colab_com_DiaPago_motor={dias_pagos}")
    for codigo, rotulo in ROTULOS.items():
        contagem = contadores[codigo]
        print(f"{codigo} ({rotulo}) total={contagem['TOTAL']}")
        for chave in quebras:
            print(f"{codigo} {chave} total={contagem[chave]}")
    for amostra in amostras:
        print(amostra)
    print("FIM")


main()
```

- `Colaborador.objects`, `empresa_id` e corte por empresa. `app/colaboradores/models.py:46`, `app/colaboradores/models.py:283`, `app/colaboradores/models.py:321`. `janela_atual` retorna a janela que contém D. O mês e o ano do fim identificam a competência. `app/ponto/janelas.py:10`, `app/ponto/janelas.py:60`.
- `DiaPago.objects` usa o filtro de tipo e versão da porta de exportação. A unicidade é por colaborador, competência, data e versão. `app/folha/porta_export.py:284`, `app/ponto/models.py:311`, `app/ponto/models.py:327`, `app/ponto/models.py:351`, `app/ponto/models.py:385`. `competencia_entregue` lê a exportação vigente por empresa, mês e ano. `app/folha/export.py:556`, `app/folha/export.py:580`.
- `batidas_apuraveis` faz a seleção e ordena por timestamp. `timezone.make_aware` e `timezone.localtime` seguem o fuso configurado. `app/ponto/turnos.py:2877`, `app/ponto/turnos.py:1980`, `app/ponto/turnos.py:45`, `app/config/settings/base.py:93`.
- `celulas_do_periodo`, `escalas_do_periodo` e `vinculo_do_dia` alimentam a quebra de `tipo_base` e `tipo_ciclo`. A quebra de marcos testa cada hora declarada na 4-tupla da célula em `[22:00,24:00)` ou `[00:00,05:00)`. A amostra imprime essa tupla na ordem `hi,hf,hii,hfi`. `app/escala/alimentacao.py:54`, `app/escala/alimentacao.py:163`, `app/escala/alimentacao.py:228`, `app/escala/models.py:117`, `app/escala/models.py:170`, `app/escala/models.py:1109`, `app/escala/utils.py:84`.
- `turnos_via_autoridade` recebe `batidas_limpas` de `MotorBase.remover_duplicatas` e os atributos do perfil. O script usa fatias contíguas pelo vínculo de D e cópias das batidas com um dia civil de contexto em cada ponta. `app/ponto/motor_calculo_v2.py:453`, `app/ponto/motor_calculo_v2.py:897`, `app/ponto/motor_calculo_v2.py:2351`, `app/ponto/motor_calculo_v2.py:3593`, `app/ponto/motor_calculo_v2.py:3640`, `app/escala/servico_jornada.py:159`, `app/ponto/turnos.py:1946`, `app/ponto/turnos.py:1984`, `app/ponto/services/fechamento.py:239`.
- `intervalos_do_turno`, `minutos_noturnos` e `minutos_noturnos_dos_intervalos` medem o líquido do par com a prorrogação padrão das duas funções. Quando mais de um par qualifica em D, a amostra mostra o maior líquido e a contagem soma um dia. A equivalência com `medir()` está [nao verificado]. `app/ponto/turnos.py:312`, `app/ponto/turnos.py:502`, `app/ponto/motor_calculo_v2.py:82`, `app/ponto/motor_calculo_v2.py:121`.
- `schema_context`, `transaction.atomic` e `connection.cursor` seguem o padrão local de transação somente leitura. O Python tem o formato esperado pelo `--rodar`. Nessa cópia, o launcher consulta a produção, importa a lavra no banco sombra e grava arquivos fora da transação do script. Execução contra o banco e contagens da frota [nao verificado]. `bin/gate_cobranca.sh:20`, `app/chamados/tests/cef_c3_sonda.py:21`, `app/config/settings/sombra.py:47`, `bin/sombra.sh:136`, `bin/sombra.sh:151`, `bin/sombra.sh:157`, `bin/sombra.sh:483`.

## 23/11 15:51 · sessao 0558da3094ac · - · - · origem=Ronald (tmux)

Para C2, `turnos_via_autoridade` aceita um `SimpleNamespace` com `escala`, `_ec`, `celulas_alimentadas` e as três constantes `AUT_*`. O perfil usa os valores de `MOTOR_POR_BASE`, a precedência do intermitente e o ajuste de geometria do partido contínuo. `MotorBase.remover_duplicatas(MotorBase, copias)` executa a limpeza sem instanciar o motor. [app/ponto/motor_calculo_v2.py:453](</home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/ponto/motor_calculo_v2.py:453>), [app/ponto/motor_calculo_v2.py:897](</home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/ponto/motor_calculo_v2.py:897>), [app/ponto/motor_calculo_v2.py:2351](</home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/ponto/motor_calculo_v2.py:2351>), [app/ponto/motor_calculo_v2.py:3593](</home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/ponto/motor_calculo_v2.py:3593>), [app/ponto/motor_calculo_v2.py:3626](</home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/ponto/motor_calculo_v2.py:3626>).

Separe por segmentos consecutivos de vínculo de D, use contexto civil de um dia em cada ponta e retenha apenas o `data_turno` do segmento. Exija entrada e saída. Subtraia os `intervalos_do_turno` com `minutos_noturnos_dos_intervalos`, usando o mesmo parâmetro `prorrogacao` no bruto e no desconto. O comportamento do `medir()` referido permanece [nao verificado]. [app/ponto/turnos.py:1946](</home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/ponto/turnos.py:1946>), [app/ponto/services/fechamento.py:239](</home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/ponto/services/fechamento.py:239>), [app/ponto/turnos.py:312](</home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/ponto/turnos.py:312>), [app/ponto/turnos.py:502](</home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/ponto/turnos.py:502>), [app/ponto/motor_calculo_v2.py:82](</home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/ponto/motor_calculo_v2.py:82>), [app/ponto/motor_calculo_v2.py:121](</home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/ponto/motor_calculo_v2.py:121>).

O wrapper invoca `lavra_de_prod` antes do Python solicitado e grava arquivos e snapshots nesse caminho. O script pode proteger suas próprias consultas com a transação solicitada. [bin/sombra.sh:144](</home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/bin/sombra.sh:144>), [bin/sombra.sh:151](</home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/bin/sombra.sh:151>), [bin/sombra.sh:157](</home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/bin/sombra.sh:157>), [bin/sombra.sh:483](</home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/bin/sombra.sh:483>).

## 23/11 15:51 · sessao 1e56280ea7e7 · - · - · origem=Ronald (tmux)

- O script pode usar `schema_context("juliani")` e `transaction.atomic(durable=True)`, emitir `SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY` antes da primeira consulta e conferir banco, schema e modo. `bin/sombra_mascara_pessoal.py:7-16`, `app/chamados/tests/cef_c3_sonda.py:22`, `app/config/settings/sombra.py:47-51`.
- O `--rodar` desta cópia importa uma lavra de produção antes de executar o script, grava arquivos e faz `update_or_create` na sombra. A restrição de somente leitura exige outra entrada para executar o mesmo Python. O launcher ainda usa `exec(open(...).read())`, então a chamada a `main()` deve ser incondicional. `bin/sombra.sh:136-166`, `bin/sombra.sh:483-493`.
- Carregue células e escalas em lote, passando os IDs das geradoras em `mais=`, e resolva `vinculo_do_dia` por colaborador e data. Para “célula tem marcos”, `marcos_dna_periodo` devolve os quatro marcos congelados. `EC.marcos_do_dia` devolve os mesmos quatro quando existe DNA e cai no template vivo na ausência. A quebra pode testar a presença de um marco pontual na faixa como interpretação explícita. `app/escala/alimentacao.py:54-78`, `app/escala/alimentacao.py:105-160`, `app/escala/alimentacao.py:228-286`, `app/escala/models.py:1109-1144`, `app/escala/utils.py:84-108`.
- A autoridade recebe `batidas_limpas` de `MotorBase.remover_duplicatas`, geometria do perfil de `get_motor`, `_ec` e `celulas_alimentadas`. Fatias contíguas com halo de um dia permitem aplicar o vínculo de D e preservar a borda à meia-noite. Copie batidas por pareamento porque a autoridade coloca marcas transitórias nos objetos. O intervalo liquidável vem de `intervalos_do_turno`. `app/ponto/motor_calculo_v2.py:453-506`, `app/ponto/motor_calculo_v2.py:897-912`, `app/ponto/motor_calculo_v2.py:2351-2354`, `app/ponto/motor_calculo_v2.py:3626-3705`, `app/ponto/services/fechamento.py:239-262`, `app/ponto/turnos.py:1517-1535`, `app/ponto/turnos.py:502-546`.
- O anexo fornecido não traz `medir()`, de modo que opção exata de prorrogação e equivalência com aquele método são [nao verificado]. Para 22h–05h estritas, passe `prorrogacao=False` às duas funções puras. `app/ponto/motor_calculo_v2.py:82-121`.

## 23/11 15:50 · sessao c066df8bb348 · - · - · origem=Ronald (tmux)

- A competência de D é o mês e ano do fim retornado por `janela_atual(D, colab.empresa)`. O recorte pode alcançar dezembro, conforme o corte de cada empresa. `app/ponto/janelas.py:24`, `app/ponto/janelas.py:36`, `app/ponto/janelas.py:60`.
- Consulte `DiaPago` por colaborador, intervalo de datas, `tipo=DiaPago.TIPO_DIA` e `versao='motor'`. Retenha somente a linha com mês e ano iguais à competência de D antes de calcular o universo. `app/folha/porta_export.py:284`, `app/ponto/models.py:311`, `app/ponto/models.py:327`, `app/ponto/models.py:351`, `app/ponto/models.py:386`.
- `competencia_entregue(empresa_id, mes, ano)` devolve o booleano de exportação vigente e aceita cache do resultado por tripla. `app/folha/export.py:556`, `app/folha/export.py:580`, `app/folha/export.py:594`.
- A autoridade acionada pela sonda lê uma borda de um dia em cada ponta e as fatias usam `data_local` para filtrar suas batidas. `app/ponto/services/espelho.py:1055`, `app/ponto/services/fechamento.py:239`.
- O pareador direto recebe o vínculo por `_ec` e o mapa por `celulas_alimentadas`. Preserve instâncias independentes de batidas quando rodar perfis sobrepostos. `app/ponto/management/commands/aplicar_09_corte_b.py:131`, `app/ponto/motor_calculo_v2.py:453`, `app/ponto/turnos.py:160`, `app/ponto/turnos.py:178`.
- O noturno líquido de relógio usa `max(0.0, minutos_noturnos(e, s, prorrogacao=p) - minutos_noturnos_dos_intervalos(e, s, intervalos, prorrogacao=p))`. A correspondência com `medir()` da sonda está [nao verificado]. `app/ponto/motor_calculo_v2.py:82`, `app/ponto/motor_calculo_v2.py:121`, `app/ponto/motor_calculo_v2.py:983`.
- Para identificar marcos da célula, `marcos_dna_periodo` retorna o DNA congelado. `EscalaColaborador.marcos_do_dia` possui fallback para template, inclusive quando há célula sem marcos. `app/escala/utils.py:84`, `app/escala/models.py:1109`, `app/escala/models.py:1135`.
- `bin/sombra.sh --rodar` localiza o script por basename em `logs/sombra/`, usa o shell Django no schema e envolve a saída em `tee`. As credenciais da sombra são configuradas no wrapper. `bin/sombra.sh:79`, `bin/sombra.sh:184`, `bin/sombra.sh:483`, `app/config/settings/sombra.py:49`.

## 23/11 15:49 · sessao 057c9d4c1879 · PAR-PELO-TURNO · sonda · origem=Code

A sonda imprimiu **5 `a`, 0 `b` e 24 `c`**. Pelas evidências do anexo e pelos critérios pedidos, a triagem fica em **27 `a`, 0 `b` e 2 `c`**. Das **22 divergências**, **13** têm par separado pelo teto no próprio dia e **9** têm entrada apurável aberta no próprio dia, sem saída final no `turno`. As outras **7** classificações são sustentadas. `app/ponto/turnos.py:183` `app/ponto/turnos.py:312` `app/ponto/motor_calculo_v2.py:983` `app/ponto/models.py:327` `app/ponto/models.py:351`

**Competência exportada:** **0 de 29 dias-colab**, abrangendo **0 dos 14 casos**, nas linhas `competencia` fornecidas. **NAO RESOLVIDO:** nenhum. Cada caso tem `RESOLVIDO n=1` com um único PK, reproduzido abaixo como `colNNN`. `app/folha/export.py:556` `app/ponto/models.py:315`

### Conferência por dia-colab

`E→S` indica os papéis no pareamento, identificados pelo PK. `Vão × teto` está em minutos, com até duas casas decimais. As órfãs vêm de `turnos_contagem` e podem incluir dias vizinhos. `DiaPago` traz `minutos_realizados / horas_noturnas` da versão `motor` em D, salvo quando D−1 está indicado. `app/ponto/turnos.py:43` `app/ponto/turnos.py:183` `app/ponto/turnos.py:1447` `app/ponto/motor_calculo_v2.py:503` `app/ponto/models.py:319` `app/ponto/models.py:327` `app/ponto/models.py:351`

| Caso | Colab | D | Classe impressa | Motivo impresso | Evidência do pareamento | DiaPago motor | Exportada | Conferência |
|---|---|---|---|---|---|---|---|---|
| caso01 | col129 | 2026-10-11 | c | `L-099_sem_batida_noturna_alvo` | Recusado `124339→124645`, **780,06 × 732**. D−1 fechou `123883→124025`, vão impresso [nao verificado], teto 732. Órfãs `123274, 124645, 125077`. | `0 / 0` (1414808). D−1 `492 / 8,8`. | Não · 10/2026 | **Diverge → a.** `par_separado_pelo_teto` em D. `app/ponto/turnos.py:183` |
| caso01 | col129 | 2026-10-25 | c | `L-099_sem_batida_noturna_alvo` | Recusado `137026→137305`, **780,29 × 732**. D−1 fechou `136544→136691`, 539,55 × 732. Órfãs `135921, 137305`. | `0 / 0` (2400115). D−1 `492 / 8,8`. | Não · 11/2026 | **Diverge → a.** `par_separado_pelo_teto` em D. `app/ponto/turnos.py:183` |
| caso01 | col129 | 2026-11-01 | c | `L-099_sem_batida_noturna_alvo` | Recusado `143430→143722`, **780,01 × 732**. D−1 fechou `142959→143112`, 539,77 × 732. Órfãs `142323, 143722`. | `0 / 0` (2400121). D−1 `492 / 8,8`. | Não · 11/2026 | **Diverge → a.** `par_separado_pelo_teto` em D. `app/ponto/turnos.py:183` |
| caso01 | col129 | 2026-11-02 | c | `L-099_sem_batida_noturna_alvo` | Recusados `144111→144456` em D, **779,01 × 732**, e `143430→143722` em D−1, 780,01 × 732. Órfãs `143112, 143722, 144456`. | `0 / 0` (2400122). D−1 `0 / 0`. | Não · 11/2026 | **Diverge → a.** `par_separado_pelo_teto` em D e S `143722` órfã atribuída a D. `app/ponto/turnos.py:183` `app/ponto/turnos.py:312` |
| caso01 | col129 | 2026-11-08 | c | `L-099_sem_batida_noturna_alvo` | Recusado `149397→149717`, **780,29 × 732**. D−1 fechou `148929→149080`, 540,23 × 732. Órfãs `148307, 149717`. | `0 / 0` (2400127). D−1 `492 / 8,8`. | Não · 11/2026 | **Diverge → a.** `par_separado_pelo_teto` em D. `app/ponto/turnos.py:183` |
| caso01 | col129 | 2026-11-15 | c | `L-099_sem_batida_noturna_alvo` | Recusado `155814→156153`, **779,99 × 732**. D−1 fechou `155325→155481`, 539,98 × 732. Órfãs `154676, 156153`. | `0 / 0` (2400134). D−1 `492 / 8,8`. | Não · 11/2026 | **Diverge → a.** `par_separado_pelo_teto` em D. `app/ponto/turnos.py:183` |
| caso02 | col155 | 2026-10-04 | c | `L-099_sem_batida_noturna_alvo` | Recusado `118190→118479`, **782,25 × 732**. D−1 fechou `117743→117877`, 543,34 × 732. Órfãs `117099, 118479`. | `0 / 0` (1419406). D−1 `492 / 8,8`. | Não · 10/2026 | **Diverge → a.** `par_separado_pelo_teto` em D. `app/ponto/turnos.py:183` |
| caso02 | col155 | 2026-10-18 | c | `L-099_sem_batida_noturna_alvo` | Recusado `130455→130747`, **783,99 × 732**. D−1 fechou `130003→130139`, 540,31 × 732. Órfãs `129385, 130747`. | `0 / 0` (1419420). D−1 `492 / 8,8`. | Não · 10/2026 | **Diverge → a.** `par_separado_pelo_teto` em D. `app/ponto/turnos.py:183` |
| caso02 | col155 | 2026-10-25 | c | `L-099_sem_batida_noturna_alvo` | Recusado `137024→137304`, **781,02 × 732**. D−1 fechou `136542→136683`, 538,67 × 732. Órfãs `135924, 137304`. | `0 / 0` (2450136). D−1 `492 / 8,8`. | Não · 11/2026 | **Diverge → a.** `par_separado_pelo_teto` em D. `app/ponto/turnos.py:183` |
| caso02 | col155 | 2026-11-01 | c | `L-099_sem_batida_noturna_alvo` | Recusado `143429→143724`, **781,71 × 732**. D−1 fechou `142958→143111`, 543,33 × 732. Órfãs `142326, 143724`. | `0 / 0` (2450143). D−1 `492 / 8,8`. | Não · 11/2026 | **Diverge → a.** `par_separado_pelo_teto` em D. `app/ponto/turnos.py:183` |
| caso02 | col155 | 2026-11-15 | c | `L-099_sem_batida_noturna_alvo` | Recusado `155813→156154`, **782,82 × 732**. D−1 fechou `155324→155479`, 539,11 × 732. Órfãs `154673, 156154`. | `0 / 0` (2450157). D−1 `492 / 8,8`. | Não · 11/2026 | **Diverge → a.** `par_separado_pelo_teto` em D. `app/ponto/turnos.py:183` |
| caso03 | col121 | 2026-11-03 | c | `L-099_criterios_a_b_ausentes_na_janela` | `E=145213, S=None`. Internas `S145367/E145385`. Comparação fora da fatia `143660→143828`, 312,64 × 1560, aceita. Órfã `143649`, fora da fatia. | `354 / 0` (2453972) | Não · 11/2026 | **Diverge → a.** `turno` aberto em D. Os alvos noturnos aparecem como internos e a saída final está ausente. `app/ponto/turnos.py:312` |
| caso04 | col502 | 2026-10-17 | c | `L-099_sem_batida_noturna_alvo` | Fechado `129546→129953`, **565,97 × 740**, com `129953` gravada como `E` e usada como saída. Internas `S129743/S129805`. D+1 tem comparação aceita `130188→130268`, 178,41 × 1560, e E `130283` aberta. Órfãs `[]`. | `440 / 0` (1529039) | Não · 10/2026 | **Sustentada.** Em D, `abertas_e_orfas=(False, False)` e `minutos_noturnos_relogio=0`. `app/ponto/turnos.py:312` `app/ponto/motor_calculo_v2.py:983` |
| caso05 | col868 | 2026-10-02 | c | `L-099_criterios_a_b_ausentes_na_janela` | `E=117058, S=None`. Internas `S117059/E117060`. Sem `teto_comparado` no bloco. Órfãs `[]`. | `367 / 0` (1413722) | Não · 10/2026 | **Diverge → a.** `turno` aberto em D, com alvos internos e sem saída final. `app/ponto/turnos.py:312` |
| caso06 | col174 | 2026-11-11 | c | `L-099_criterios_a_b_ausentes_na_janela` | Recusado `152378→152727`, **722 × 720** na linha `par_separado_pelo_teto`. Decimal de `teto_comparado` [nao verificado]. Internas `S152566/E152577`. Órfã `152727`. | `420 / 0` (2470743) | Não · 11/2026 | **Diverge → a.** A saída final está na lista de órfãs e o par separado está explícito. `app/ponto/turnos.py:183` |
| caso07 | col612 | 2026-11-11 | c | `L-099_criterios_a_b_ausentes_na_janela` | `E=152381, S=None`. Internas `S152546/E152562`. Comparação fora da fatia `150586→150650`, 279,61 × 1560, aceita. Órfã `150567`, fora da fatia. | `378 / 0` (2450464) | Não · 11/2026 | **Diverge → a.** `turno` aberto em D, com alvos internos e sem saída final. `app/ponto/turnos.py:312` |
| caso08 | col999 | 2026-11-09 | c | `L-099_sem_batida_noturna_alvo` | `E=150272, S=None` no vínculo 1569, `na_fatia=True`. D+1 fechou `150789→151169`, 540,18 × 780, com tipos gravados `S/E`. Órfã `149108`, fora da fatia. | `0 / 0` (2434305) | Não · 11/2026 | **Diverge → a.** `turno` do próprio D fica aberto no vínculo de D. `app/ponto/turnos.py:312` `app/escala/alimentacao.py:228` |
| caso08 | col999 | 2026-11-10 | c | `L-099_sem_batida_noturna_alvo` | Fechado `150789→151169`, **540,18 × 780**, com tipos gravados `S/E` e internas `E150858/S150916`. E `150272` aberta em D−1. D+1 fechou `151739→152239`, 551,62 × 780. Órfãs `[]`. | `470 / 0` (2434306) | Não · 11/2026 | **Sustentada.** Em D, `abertas_e_orfas=(False, False)` e `minutos_noturnos_relogio=0`. `app/ponto/turnos.py:312` `app/ponto/motor_calculo_v2.py:983` |
| caso09 | col992 | 2026-11-03 | c | `L-099_criterios_a_b_ausentes_na_janela` | Recusado `145230→145574`, **721,94 × 720**. Internas `S145380/E145394`. Comparação fora da fatia `143666→143759`, 294,74 × 1560, aceita. Órfãs `143652, 145574`. | `367 / 0` (2535747) | Não · 11/2026 | **Diverge → a.** `par_separado_pelo_teto` em D, com S `145574` órfã. `app/ponto/turnos.py:183` |
| caso10 | col490 | 2026-10-28 | c | `L-099_criterios_a_b_ausentes_na_janela` | `E=140120, S=None`. Internas `S140345/E140351`. Comparação fora da fatia `138327→138452`, 165,99 × 1560, aceita. Órfã `138319`, fora da fatia. | `495 / 0` (2452929) | Não · 11/2026 | **Diverge → a.** `turno` aberto em D, com alvos internos e sem saída final. `app/ponto/turnos.py:312` |
| caso10 | col490 | 2026-11-01 | c | `L-099_criterios_a_b_ausentes_na_janela` | `E=143507, S=None`. Internas `S143678/E143686`. Comparação fora da fatia `142287→142356`, 175,12 × 1560, aceita. Órfã `142279`, fora da fatia. | `484 / 0` (2452931) | Não · 11/2026 | **Diverge → a.** `turno` aberto em D, com alvos internos e sem saída final. `app/ponto/turnos.py:312` |
| caso11 | col788 | 2026-10-04 | a | `batida_noturna_em_E_aberta_ou_S_orfa` | `E=118383, S=None`. Comparações aceitas `117003→117074` 337,79 × 1560, `117003→117213` 422,88 × 1560, `118456→118477` 50,00 × 1560 e `118578→119321` 950,67 × 1560. Órfãs `[]`. | `0 / 0` (1406259) | Não · 10/2026 | **Sustentada.** `batida_alvo pk=118383` tem papel `E_aberta` em D. `app/ponto/turnos.py:312` |
| caso11 | col788 | 2026-10-14 | a | `batida_noturna_em_E_aberta_ou_S_orfa` | `E=127324, S=None`. Comparações vizinhas aceitas `125753→126275` 475,61 × 840 e `127416→127422` 60,77 × 1560. Órfãs `[]`. | `0 / 0` (1406268) | Não · 10/2026 | **Sustentada.** `batida_alvo pk=127324` tem papel `E_aberta` em D. `app/ponto/turnos.py:312` |
| caso11 | col788 | 2026-10-16 | a | `batida_noturna_em_E_aberta_ou_S_orfa` | `E=129246, S=None`. Comparações vizinhas aceitas `127416→127422` 60,77 × 1560 e `129342→129348` 69,43 × 1560. Órfãs `[]`. | `0 / 0` (1406270) | Não · 10/2026 | **Sustentada.** `batida_alvo pk=129246` tem papel `E_aberta` em D. `app/ponto/turnos.py:312` |
| caso11 | col788 | 2026-10-28 | c | `L-099_sem_batida_noturna_alvo` | `E=140245, S=None`. Comparações vizinhas aceitas `138680→139201` 455,64 × 840 e `140352→140360` 51,23 × 1560. Órfãs `[]`. | `0 / 0` (2542626) | Não · 11/2026 | **Diverge → a.** `turno` de D aberto com E `140245` às 21:33. O par com 22,87 min noturnos tem `data_turno=2026-10-29`. `app/ponto/turnos.py:312` `app/ponto/motor_calculo_v2.py:983` |
| caso11 | col788 | 2026-11-01 | a | `batida_noturna_em_E_aberta_ou_S_orfa` | `E=143603, S=None`. D+1 tem comparações aceitas `143682→143689` 66,49 × 1560, `143682→144086` 766,02 × 1560 e `144090→144298` 268,03 × 1560. Órfãs `[]`. | `0 / 0` (2542629) | Não · 11/2026 | **Sustentada.** `batida_alvo pk=143603` tem papel `E_aberta` em D. `app/ponto/turnos.py:312` |
| caso12 | col516 | 2026-10-30 | c | `L-099_criterios_a_b_ausentes_na_janela` | `E=142071, S=None`. Internas `S142228/E162536`. Comparação fora da fatia `140300→140413`, 371,92 × 1560, aceita. Órfã `162534`, fora da fatia. | `303 / 0` (2528760) | Não · 11/2026 | **Diverge → a.** `turno` aberto em D, com alvos internos e sem saída final. `app/ponto/turnos.py:312` |
| caso13 | col820 | 2026-10-16 | a | `batida_noturna_em_E_aberta_ou_S_orfa` | Em D, E `129248` e E `128591` abertas. Comparações vizinhas aceitas `127945→128276` 414,99 × 720 e `129345→129355` 65,04 × 1560. Órfãs `[]`. | `0 / 0` (1404220) | Não · 10/2026 | **Sustentada.** `batida_alvo pk=129248` tem papel `E_aberta` em D. `app/ponto/turnos.py:312` |
| caso14 | col467 | 2026-10-11 | c | `L-099_criterios_a_b_ausentes_na_janela` | `E=124392, S=None`. Internas `S124547/E124563`. Comparação fora da fatia `123191→123287`, 311,48 × 1560, aceita. Órfã `123173`, fora da fatia. | `330 / 0` (1415304) | Não · 10/2026 | **Diverge → a.** `turno` aberto em D, com alvos internos e sem saída final. `app/ponto/turnos.py:312` |

A leitura de `a` inclui a entrada apurável sem saída final em D, a mesma situação registrada como `E_aberta` nos cinco `a` impressos. Nas nove divergências por abertura, a causa da falta de fechamento é [nao verificado]. Para `b`, nenhum bloco combina par **do próprio D** com `minutos_noturnos_relogio > 0`, `minutos_realizados > 0` e `horas_noturnas = 0`. Nos dois `c` sustentados, o par de D é diurno. Onde D está aberto, a própria linha `turno` informa `sem_par_E_S`. `app/ponto/turnos.py:312` `app/ponto/motor_calculo_v2.py:983` `app/ponto/models.py:327` `app/ponto/models.py:351`

### Dono L-099 dos `c` impressos

**BATIDA** é o dono sugerido quando o `turno` de D tem entrada aberta e o PK aparece em `batidas_apuraveis`. Essa é a evidência local que corresponde à guarda de batida solta do juiz. O veredito do juiz em PROD é [nao verificado]. O leitor dos pares separados também consulta esse juiz para determinar o dono. `app/ponto/management/commands/e6_oraculo.py:98` `app/ponto/management/commands/e6_oraculo.py:182` `app/ponto/management/commands/e6_oraculo.py:198` `app/ponto/services/teto_na_lista.py:86`

| `c` impresso | Dono sugerido | Linha da saída que sustenta |
|---|---|---|
| caso01 · col129 · seis datas | **BATIDA** | `turno` em D com `S=None` e E `124339` (10-11), `137026` (10-25), `143430` (11-01), `144111` (11-02), `149397` (11-08) e `155814` (11-15). Cada E consta em `batidas_apuraveis`. `app/ponto/turnos.py:312` `app/ponto/turnos.py:2877` |
| caso02 · col155 · cinco datas | **BATIDA** | `turno` em D com `S=None` e E `118190` (10-04), `130455` (10-18), `137024` (10-25), `143429` (11-01) e `155813` (11-15). Cada E consta em `batidas_apuraveis`. `app/ponto/turnos.py:312` `app/ponto/turnos.py:2877` |
| caso03 · col121 · 11-03 | **BATIDA** | `turno` em D `E=145213, S=None`, com `145213` em `batidas_apuraveis`. `app/ponto/turnos.py:312` `app/ponto/turnos.py:2877` |
| caso04 · col502 · 10-17 | **não decide** | `turno` em D `129546→129953`, fechado e com `minutos_noturnos_relogio=0`. Dono [nao verificado]. `app/ponto/turnos.py:312` `app/ponto/motor_calculo_v2.py:983` |
| caso05 · col868 · 10-02 | **BATIDA** | `turno` em D `E=117058, S=None`, com `117058` em `batidas_apuraveis`. `app/ponto/turnos.py:312` `app/ponto/turnos.py:2877` |
| caso06 · col174 · 11-11 | **BATIDA** | `turno` em D `E=152378, S=None`, com `152378` em `batidas_apuraveis`. `par_separado_pelo_teto` identifica a S `152727`. `app/ponto/turnos.py:183` `app/ponto/turnos.py:312` `app/ponto/turnos.py:2877` |
| caso07 · col612 · 11-11 | **BATIDA** | `turno` em D `E=152381, S=None`, com `152381` em `batidas_apuraveis`. `app/ponto/turnos.py:312` `app/ponto/turnos.py:2877` |
| caso08 · col999 · 11-09 | **BATIDA** | `turno` do vínculo 1569 em D `E=150272, S=None, na_fatia=True`, com `150272` em `batidas_apuraveis`. `app/ponto/turnos.py:312` `app/ponto/turnos.py:2877` |
| caso08 · col999 · 11-10 | **não decide** | `turno` em D `150789→151169`, fechado e com `minutos_noturnos_relogio=0`. A E `150272` aberta tem `data_turno=2026-11-09`. Dono [nao verificado]. `app/ponto/turnos.py:312` `app/ponto/motor_calculo_v2.py:983` |
| caso09 · col992 · 11-03 | **BATIDA** | `turno` em D `E=145230, S=None`, com `145230` em `batidas_apuraveis`. `par_separado_pelo_teto` identifica a S `145574`. `app/ponto/turnos.py:183` `app/ponto/turnos.py:312` `app/ponto/turnos.py:2877` |
| caso10 · col490 · 10-28 e 11-01 | **BATIDA** | `turno` em D com `S=None` e E `140120` (10-28) ou `143507` (11-01). Ambas constam em `batidas_apuraveis` nos respectivos blocos. `app/ponto/turnos.py:312` `app/ponto/turnos.py:2877` |
| caso11 · col788 · 10-28 | **BATIDA** | `turno` em D `E=140245, S=None`, com `140245` em `batidas_apuraveis`. `app/ponto/turnos.py:312` `app/ponto/turnos.py:2877` |
| caso12 · col516 · 10-30 | **BATIDA** | `turno` em D `E=142071, S=None`, com `142071` em `batidas_apuraveis`. `app/ponto/turnos.py:312` `app/ponto/turnos.py:2877` |
| caso14 · col467 · 10-11 | **BATIDA** | `turno` em D `E=124392, S=None`, com `124392` em `batidas_apuraveis`. `app/ponto/turnos.py:312` `app/ponto/turnos.py:2877` |

Hash da cópia e implementação da seleção de alvos em `sonda.py` [nao verificado].

## 23/11 15:44 · sessao f124818f214d · PAR-PELO-TURNO · sonda · origem=Code

A saída fornecida contém **2 casos com `CAUSA c` e 14 `NAO RESOLVIDO`**. Nos dois casos com `CAUSA`, o motivo impresso é sustentado pelos turnos diurnos, mas **a classe diverge do critério literal do aval**. O `DiaPago` motor tem realizado positivo e noturno zero, o que preenche **b**. Os outros 15 dos 31 casos declarados estão [nao verificado]. Linhas `CAUSA`, `turno` e `DiaPago` da saída; `app/ponto/motor_calculo_v2.py:82`, `app/ponto/models.py:327`, `app/ponto/models.py:351`.

### Triagem por dia-colab

Os pares abaixo são os do dia `D`, identificados por pk e horário local. A coluna de evidência usa `minutos_realizados` em minutos e `horas_noturnas` em horas, ambos da versão `motor`. Linhas `turno`, `teto_comparado` e `DiaPago` da saída; `app/ponto/turnos.py:43`, `app/ponto/models.py:322`, `app/ponto/models.py:325`, `app/ponto/models.py:351`.

| Caso | colNNN (pk) | D | Classe impressa → auditada | Motivo | Evidência |
|---|---|---|---|---|---|
| caso01 | [nao verificado] | [nao verificado] | NAO RESOLVIDO | `NAO RESOLVIDO caso01 n=0` | E/S, vão × teto, órfãs, DiaPago e exportação [nao verificado] |
| caso02 | [nao verificado] | [nao verificado] | NAO RESOLVIDO | `NAO RESOLVIDO caso02 n=0` | E/S, vão × teto, órfãs, DiaPago e exportação [nao verificado] |
| caso03 | [nao verificado] | [nao verificado] | NAO RESOLVIDO | `NAO RESOLVIDO caso03 n=0` | E/S, vão × teto, órfãs, DiaPago e exportação [nao verificado] |
| caso04 | [nao verificado] | [nao verificado] | NAO RESOLVIDO | `NAO RESOLVIDO caso04 n=0` | E/S, vão × teto, órfãs, DiaPago e exportação [nao verificado] |
| caso05 | [nao verificado] | [nao verificado] | NAO RESOLVIDO | `NAO RESOLVIDO caso05 n=0` | E/S, vão × teto, órfãs, DiaPago e exportação [nao verificado] |
| caso06 | [nao verificado] | [nao verificado] | NAO RESOLVIDO | `NAO RESOLVIDO caso06 n=0` | E/S, vão × teto, órfãs, DiaPago e exportação [nao verificado] |
| caso07 | [nao verificado] | [nao verificado] | NAO RESOLVIDO | `NAO RESOLVIDO caso07 n=0` | E/S, vão × teto, órfãs, DiaPago e exportação [nao verificado] |
| caso08 | [nao verificado] | [nao verificado] | NAO RESOLVIDO | `NAO RESOLVIDO caso08 n=0` | E/S, vão × teto, órfãs, DiaPago e exportação [nao verificado] |
| caso09 | [nao verificado] | [nao verificado] | NAO RESOLVIDO | `NAO RESOLVIDO caso09 n=0` | E/S, vão × teto, órfãs, DiaPago e exportação [nao verificado] |
| caso10 | [nao verificado] | [nao verificado] | NAO RESOLVIDO | `NAO RESOLVIDO caso10 n=0` | E/S, vão × teto, órfãs, DiaPago e exportação [nao verificado] |
| caso11 | [nao verificado] | [nao verificado] | NAO RESOLVIDO | `NAO RESOLVIDO caso11 n=0` | E/S, vão × teto, órfãs, DiaPago e exportação [nao verificado] |
| caso12 | [nao verificado] | [nao verificado] | NAO RESOLVIDO | `NAO RESOLVIDO caso12 n=0` | E/S, vão × teto, órfãs, DiaPago e exportação [nao verificado] |
| caso13 | [nao verificado] | [nao verificado] | NAO RESOLVIDO | `NAO RESOLVIDO caso13 n=0` | E/S, vão × teto, órfãs, DiaPago e exportação [nao verificado] |
| caso14 | [nao verificado] | [nao verificado] | NAO RESOLVIDO | `NAO RESOLVIDO caso14 n=0` | E/S, vão × teto, órfãs, DiaPago e exportação [nao verificado] |
| col214 | col214 (214) | 2026-11-15 | **c → b · DIVERGÊNCIA** | `L-099_sem_batida_noturna_alvo` é sustentado por `b_dia_E_S=[]` e 0 min noturnos nos dois turnos. O gravado `440 min / 0 h` preenche b literal. `app/ponto/motor_calculo_v2.py:82`, `app/ponto/models.py:327`, `app/ponto/models.py:351`. | E155565 07:00:20 → S155668 12:06:40 **306,33 × 1560 min**. E155704 13:06:42 → S155775 15:20:54 **134,20 × 740 min**. Ambos aceitos; órfãs `[]`. `DiaPago` motor 2476612 **440 min / 0 h**. 11/2026 **exportada não**. Linhas `teto_comparado`, `turno`, `turnos_contagem`, `DiaPago` e `competencia`; `app/ponto/turnos.py:1447`, `app/ponto/models.py:351`, `app/folha/export.py:556`. |
| col107 | col107 (107) | 2026-11-15 | **c → b · DIVERGÊNCIA** | `L-099_sem_batida_noturna_alvo` é sustentado por `b_dia_E_S=[]` e 0 min noturnos nos dois turnos. O gravado `382 min / 0 h` preenche b literal. `app/ponto/motor_calculo_v2.py:82`, `app/ponto/models.py:327`, `app/ponto/models.py:351`. | E155585 07:05:19 → S155607 09:15:09 **129,83 × 1560 min**. E155627 10:13:25 → S155739 14:25:11 **251,76 × 750 min**, via `pos_pausa_sem_volta`. Ambos aceitos; órfãs `[]`. `DiaPago` motor 2441257 **382 min / 0 h**. 11/2026 **exportada não**. Linhas `teto_comparado`, `turno`, `turnos_contagem`, `DiaPago` e `competencia`; `app/ponto/turnos.py:1183`, `app/ponto/turnos.py:1447`, `app/ponto/models.py:351`, `app/folha/export.py:556`. |

Cada dia tem dois turnos fechados. As linhas `turno` trazem `_separada_pelo_teto=None` e `abertas_e_orfas=(False, False)`, e `evidencias_causa` traz `a_pares_recusados=[]` e `a_abertas_ou_orfas=[]`. Que os dois turnos de cada dia fossem uma única jornada separada fica [nao verificado]. A classe **b** aqui é a aplicação do critério literal. Noturno devido e omitido fica [nao verificado]. `app/ponto/turnos.py:183`, `app/ponto/turnos.py:312`, `app/ponto/motor_calculo_v2.py:82`.

### Totais

| Contagem no trecho fornecido | a | b | c | NAO RESOLVIDO |
|---|---:|---:|---:|---:|
| Impressa nas linhas `CAUSA` e `NAO RESOLVIDO` | 0 | 0 | 2 | 14 |
| Auditada pelo critério literal, com os dois `DiaPago` motor (`app/ponto/models.py:327`, `app/ponto/models.py:351`) | 0 | 2 | 0 | 14 |

- **Competência EXPORTADA:** 0 sim, 2 não. As duas linhas `competencia` mostram `status='aberto' exportada=False exportacao_pk=None`. A situação dos outros 29 casos e o total nos 31 ficam [nao verificado]. `app/folha/export.py:556`.
- **NAO RESOLVIDO:** `caso01` a `caso14`, todos com `n=0` nas respectivas linhas da saída. O status dos 15 casos ausentes fica [nao verificado].

### Dono L-099 para os dois `c` impressos

- **col214 — não decide.** `teto_comparado` para E155565/S155668 mostra `teto_do_dia=False` com marcos `12:40–21:00`. É indício de **CADASTRO**, pois o pareador usa o envelope quando ambas as pontas estão fora da régua da L-084. A inclusão de `D` em `dias_cadastro_x_realidade`, insumo do juiz L-099, fica [nao verificado]. `app/ponto/turnos.py:1435`, `app/ponto/motor_calculo_v2.py:1931`, `app/ponto/management/commands/e6_oraculo.py:162`.
- **col107 — não decide.** `teto_comparado` para E155585/S155607 mostra `teto_do_dia=False` com marcos `12:50–21:20`. É o mesmo indício de **CADASTRO**. A inclusão de `D` em `dias_cadastro_x_realidade` fica [nao verificado]. `app/ponto/turnos.py:1435`, `app/ponto/motor_calculo_v2.py:1931`, `app/ponto/management/commands/e6_oraculo.py:162`.

## 23/11 15:42 · sessao 727a1b162816 · PAR-PELO-TURNO · leitura · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 23/11 15:41 · sessao 238bcbc77caa · VIGIA-DO-TMUX · verificador · origem=Code

**VIGIA-DO-TMUX, volta 2 — P4 não realizado.** O Stop e o vigia compartilham dois predicados, mas só o Stop decide sobre ciclo, disparo ativo, expiração e agentes com transcript. Há entradas em que o Stop devolve `siga` e `recusa_clear()` devolve `''`. `bin/hook_stop_fila1.py:973–987` (diff), `bin/hook_stop_fila1.py:1012–1041,1060–1113` (cópia).

As referências marcadas **diff** usam as linhas novas dos hunks. As demais usam a cópia do main. Os contraexemplos foram conferidos em memória. Execução integral do selo, resultados dos mutantes, identidade dos commits e estado de produção **[nao verificado]**. [nao verificado]

### P1–P7

| id | procede? | arquivo:linha | consequência concreta | cura mínima |
|---|---|---|---|---|
| P1 | **parcial** | diff `bin/vigia_tmux.py:197–206,225,233–265` · diff `bin/tests/test_vigia_tmux.sh:160–184,223–225,265–269` | Cada envio fixa um `%N`, grava a marca antes da segunda captura e recaptura antes do Enter. `a` acusa troca de alvo ou ordem, `c4` acusa marca que ficou após adiamento, `j` acusa Enter com conteúdo divergente. A comparação aceita espaço adicional: `❯  /clear`, com um espaço extra após o separador do prompt, vira `/clear` pelo `.strip()`. | Remover só o separador do prompt e exigir igualdade do conteúdo restante. Acrescentar a `j` uma divergência só de espaço. |
| P2 | **parcial** | diff `bin/vigia_tmux.py:209–225` · diff `bin/tests/test_vigia_tmux.sh:234–235` | `c12` morde o falso ocioso original. Uma linha de entrada iniciada por `─` passa por borda superior. Na sequência `──── / ❯ texto / ─ detalhe / ❯ / ────` (uma linha por trecho), o parser descarta o primeiro `❯` e retorna caixa vazia. | Validar o delimitador da interface e recusar recortes ambíguos. Acrescentar essa tela ao selo. |
| P3 | **parcial** | diff `bin/vigia_tmux.py:160–190,243,251,265,311,325` · diff `bin/tests/test_vigia_tmux.sh:213–221,267–269,294–298` | `c1` e `c2` cobram uma linha após dois ticks do mesmo SHA. `c2` também cobra que o adiado possa enviar depois. Em execução normal, a chave é SHA + decisão + ação. Se o processo cair após publicar no RELATO e antes de gravar `relatados.json`, o próximo adiado ou recusado duplica a linha. | Verificar a chave e inserir sob a mesma trava da porta do RELATO. |
| P4 | **não** | diff `bin/hook_stop_fila1.py:965–987,1105` · diff `bin/vigia_tmux.py:309–315` · diff `bin/tests/test_vigia_tmux.sh:237–250` | `c13` e seu controle mordem o predicado de handoff. `c14` morde o teste de `sombra.lock`, com fixture sem disparo ativo. Nenhum compara o veredito com a transição do Stop. | Fazer Stop e vigia chamarem a mesma decisão de autorização, com o estado do ciclo e o transcript do painel. Cobrar a paridade no selo. |
| P5 | **parcial** | diff `bin/vigia_tmux.py:156–157,235,247–261,328–337` · diff `bin/tests/test_vigia_tmux.sh:48,227,252–255` | O código relê sob a trava e antes das duas teclas. `c15` e `c16` mordem as leituras por tecla. Ambos criam a pausa depois da leitura sob a trava, então passam se essa leitura for removida. | Criar `PAUSADO` no momento da aquisição da trava e cobrar saída antes da leitura do topo. |
| P6 | **parcial** | diff `bin/tests/test_vigia_tmux.sh:34–42,50–51,67–68,153–184,213,257–260,305–313` | O fake registra os alvos. `a` reprova marca tardia e resolução repetida; `c17` reprova remover o `flock`. `c1` cobra o texto do motivo até o fim da linha, mas `[0-9a-f]*` aceita até SHA vazio. O isolamento só procura os SHAs da rodada depois do teste, então uma alteração no RELATO real sem esses SHAs passa. | Cobrar a linha inteira com o SHA esperado, medir o hash do RELATO real antes e depois e registrar o RED dos dois mutantes. |
| P7 | **sim** | diff `bin/vigia_tmux.py:84–94,116,346–350` · diff `bin/tests/test_vigia_tmux.sh:143–156,262–263` | O handler fica instalado antes do parse de `TETO_CLEAR` e da importação do Stop. `c18` cobra `ValueError` no log, rc 0, saída vazia e nenhuma tecla. Recolocar o handler no fim do módulo quebra esse caso. | — |

### CLEAR e comportamento do Stop

“Libera CLEAR” abaixo é a autorização explícita do ramo `CLEAR-RAPIDO`. O fallback `AVISO` pode encerrar o turno sem essa autorização. `bin/hook_stop_fila1.py:1094–1113,1266–1270,1361–1367,1391–1405`.

| id | procede? | arquivo:linha | consequência concreta | cura mínima |
|---|---|---|---|---|
| C1 · Sem disparo ativo | **sim** | diff `bin/hook_stop_fila1.py:980–987` · `bin/hook_stop_fila1.py:1110–1133,1151–1156` | Estado ausente, sem `disparo` ou com `desfecho` preenchido deixa a recusa vazia com sombra livre. O Stop não libera por essa porta. Com gatilho novo e caminho livre, devolve `siga` para preparar CLEAR. | Exigir disparo ativo do ciclo vigente na decisão compartilhada. |
| C2 · CLEAR expirado | **sim** | diff `bin/hook_stop_fila1.py:985–987` · `bin/hook_stop_fila1.py:1061–1064,1094–1109` · `bin/orcamento_contexto.py:56` | Com handoff novo e `clear_parei_desde` há mais de 10 min, a recusa fica vazia. O Stop grava `desfecho='seguiu'` e manda retirar o `PAREI` do topo. | Aplicar a janela do Stop ao mesmo `agora`. |
| C3 · Agente sem entrega | **sim** | diff `bin/vigia_tmux.py:309` · diff `bin/hook_stop_fila1.py:973,987` · `bin/hook_stop_fila1.py:936–945,1078–1093` · `bin/disjuntor.py:465–481` | Com disparo ativo, handoff novo e agente pendente no transcript, o Stop devolve `siga`, atualiza `handoff_min` e limpa `clear_parei_desde`. O vigia chama sem transcript e pode enviar. | Resolver o transcript do painel e aplicar a guarda existente. Adiar quando a identidade da sessão for indispensável e estiver indisponível. |
| C4 · Ciclo trocado ou orçamento desligado | **sim** | diff `bin/hook_stop_fila1.py:980–987` · `bin/hook_stop_fila1.py:959–971,1003–1041` | Um marco novo pode invalidar um disparo ainda gravado no JSON. O Stop troca o ciclo antes de julgar o topo, enquanto a recusa usa o disparo antigo. Com `ORCAMENTO_CONTEXTO=0`, o Stop também sai desse ramo e a recusa ainda pode autorizar o envio. | Compartilhar seleção do ciclo e condição de desligamento. |
| C5 · Bloqueios anteriores ao orçamento | **sim** | `bin/hook_stop_fila1.py:1342–1367` · `bin/disjuntor.py:1413–1433` · diff `bin/hook_stop_fila1.py:973–987` | Uma afirmação sem prova no RELATO ou o primeiro `siga` do B4 pode bloquear o Stop antes de chegar ao CLEAR, mesmo com `recusa_clear()==''`. | Fazer a autorização do vigia respeitar os bloqueios anteriores aplicáveis. |
| C6 · Guarda vencida antes da tecla | **sim** | diff `bin/vigia_tmux.py:309–315,233–261` · `bin/disjuntor.py:416–425` | A sondagem da sombra solta a trava antes das capturas e esperas. Um DIFF pode tomar a trava nesse intervalo e o vigia ainda digitar ou enviar Enter. O Stop consultado nesse instante recusaria com disparo ativo. | Revalidar junto da tecla e manter a exclusão com a sombra até o Enter. Liberar antes de esperar o marco ou publicar no RELATO. |
| Stop · Mudança pela extração | **não** | diff `bin/hook_stop_fila1.py:965–970,1102–1108` · `bin/hook_stop_fila1.py:1061–1075` | A linha antiga e o helper avaliam o mesmo `< max(disp.get('hora') or 0, disp.get('handoff_min') or 0)`, na mesma ordem. Igualdade continua passando. Erros da avaliação continuam subindo ao mesmo tratamento. Os ramos e as gravações do Stop não mudam. | — |
| Base · APIs de ESPERANDO | **sim** | diff `bin/vigia_tmux.py:286–297` · `bin/hook_stop_fila1.py:599–626,807–831` | O anexo chama `campos_do_esperando` e `MOTIVO_PID_MORTO`. Na cópia main, o parser está dentro de `_esperando_com_prova` e `_prova_pid` usa o literal. Portar só o delta deixa esse caminho sem as APIs. A implementação herdada de `89db6b43e` está **[nao verificado]**. | Conferir no pouso o pré-requisito da volta 1 que fornece as duas APIs. |

### Escritores, leitores e classes de reprovação

| id | procede? | arquivo:linha | consequência concreta | cura mínima |
|---|---|---|---|---|
| Estado · `feitos.json` | **sim** | diff `bin/vigia_tmux.py:119–153,245–268,291–307,313–324` · diff `bin/tests/test_vigia_tmux.sh:50–51,67–68` | O vigia escreve por `_marca` e `_desmarca`, lê por `_le_feitos` e consulta o registro nas duas ações. O fake observa a marca nas capturas e teclas. O mesmo registro serve para impedir reenvio e para permitir só a retomada de um CLEAR pendente. A captura que levanta após a marca deixa `enviando-*` gravado, mesmo antes do literal. | Manter um escritor. Cobrar no selo a distinção entre falha antes da primeira tecla e envio de resultado incerto. |
| Estado · `relatados.json`, RELATO e log | **sim** | diff `bin/vigia_tmux.py:77–81,160–190,337` · diff `bin/tests/test_vigia_tmux.sh:153,158,307–313` · `bin/hook_stop_fila1.py:408–416,462–479` · `app/core/esteira_vigia.py:815–843,895–927` | `_decide` lê e grava a deduplicação, escreve em `vigia.log` e chama `no_relato`. A porta lê e regrava o RELATO. O vigia e o Stop leem dali o comando do topo pelo mesmo leitor. O selo lê log e RELATO. Topo é comando e ALARMES é trilha. O cache registra publicação anterior, mesmo se a linha sumir depois. | Usar a inserção idempotente da porta como autoridade para a deduplicação. |
| Estado · `PAUSADO` | **sim** | `bin/pausar.sh:18–28,31–49` · diff `bin/vigia_tmux.py:101,156–157,235,248,258,328–336` · diff `bin/tests/test_vigia_tmux.sh:48,227` | `pausar.sh` publica e remove o arquivo, e `--ler` lê os metadados. O vigia só testa a existência. O `touch` é escritor da fixture. A publicação da pausa pode ocorrer entre a última consulta e `send-keys`. | Cobrir a releitura sob a trava. Para exclusão estrita, serializar publicação e tecla pelo mesmo mecanismo. |
| Estado · `pane_id` | **sim** | diff `bin/vigia_tmux.py:197–206,238,247,253,261,304–307` · diff `bin/tests/test_vigia_tmux.sh:40–42,188–195` | O tmux fornece o `%N`. O vigia usa o valor local nas capturas e teclas e o grava em `feitos.json`. A decisão de retomar consulta estado e hora, e o próximo envio resolve o alvo de novo. Uma troca de painel pode mandar a retomada a outro `%N`. O fake sempre devolve `%7`. | Vincular a retomada ao painel do `/clear` e testar mudança de alvo entre as duas ações. |
| Estado · orçamento, handoff e marco | **sim** | `bin/hook_stop_fila1.py:903–910,929–933,996–1041` · diff `bin/hook_stop_fila1.py:968,981–987` · `bin/orcamento_contexto.py:152–164,352–361` · `bin/disjuntor.py:2104–2105` · `bin/handoff_sessao.sh:318–327` · `bin/marco_de_clear.py:38–61` · diff `bin/vigia_tmux.py:103,306,313–325` | O Stop grava o orçamento. Stop, vigia, `orcamento_contexto` e disjuntor o leem. `handoff_sessao.sh` grava o handoff e o Stop fornece seu mtime às duas decisões. `marco_de_clear.py` lê e grava o marco. O Stop o usa para trocar de ciclo e o vigia para soltar a retomada. | Reusar a leitura de ciclo do Stop e associar a prova de CLEAR ao evento da sessão correta. |
| Ordem de trava | **sem inversão identificada** | diff `bin/vigia_tmux.py:333–337,184,309–325` · `bin/hook_stop_fila1.py:973–986,1078` · `bin/sombra_trava.sh:21–38` · `bin/disjuntor.py:406–425` · `app/core/esteira_vigia.py:846–862` · `bin/marco_de_clear.py:38–63` | Ticks do vigia disputam `.trava`. Sob ela, o vigia sonda a trava da sombra, já solta quando entra na porta do RELATO. O Stop sonda a sombra sob `orcamento_estado.json.lock`. O escritor do marco usa `ultimo_clear.json.lock`. A lacuna demonstrada é a sondagem solta antes da tecla, em C6. | Ao fechar C6, evitar adquirir a trava do RELATO ou esperar o marco enquanto segura a trava da sombra. |
| Relógio fora da borda | **sim** | diff `bin/vigia_tmux.py:245–261,306,313–314` · `bin/marco_de_clear.py:41–57` · diff `bin/tests/test_vigia_tmux.sh:73–76,199–210` | `hora_clear` é amostrada antes de resolver o painel e das três capturas. Um CLEAR de outra origem, posterior a essa amostra e anterior ao Enter do vigia, pode satisfazer a retomada. `clear_horas` só guarda horários e `session_id` pertence ao último marco. O teste não injeta um marco concorrente antes do Enter. | Amostrar junto do Enter e exigir um evento de CLEAR correlacionado à sessão e à tentativa. |
| Segundo juiz e emissor | **sim** | diff `bin/hook_stop_fila1.py:973–987,1105` · `bin/keepalive.sh:4–9` · diff `bin/vigia_tmux.py:199,253,261,309` | `recusa_clear` é o segundo juiz de autorização. `keepalive.sh` é um emissor preexistente com alvo re-resolvido e guardas próprias, fora da trava do vigia. Execução em produção **[nao verificado]**. | Unificar a decisão de CLEAR e retirar ou serializar o emissor legado no pouso. |
| Escritores fora da porta | **sim, preexistentes** | `bin/gerar_cortes.py:117–136` · `bin/deploy_agendado.sh:31–44` · `app/core/esteira_vigia.py:834–843,922–927` · diff `bin/vigia_tmux.py:181–190` | Ambos leem e regravam o RELATO fora da trava do diretório. Podem sobrescrever um alarme recém-inserido. Com a chave já gravada em `relatados.json`, o vigia suprime a republicação. | Fazer essas regravações usarem a mesma transação do RELATO. |
| Selo · Limpeza | **sim** | diff `bin/tests/test_vigia_tmux.sh:24,248–261` | Se o selo sair antes de inicializar `SOMBRA` ou `TRAVADO`, o `trap` passa `0` ao `kill`, que sinaliza o grupo de processos. | Sinalizar só PIDs inicializados e ainda sob responsabilidade do selo. |
| `except` novo | **não, 0** | diff `bin/vigia_tmux.py:84–94` · diff `bin/hook_stop_fila1.py:965–987` · diff `bin/tests/test_vigia_tmux.sh:302–303` | O delta acrescenta `try/finally` e instala `sys.excepthook`. Acrescenta zero cláusulas `except`. O selo conta `ast.ExceptHandler` no vigia. | — |

## 23/11 15:13 · sessao 5917bcb4ab53 · PAR-PELO-TURNO · sonda · origem=Code

Na sonda, os lotes 1 (`turnos_do_colab`) e 2 (`turnos_via_autoridade`) deram o mesmo destino às batidas de referência dos quatro casos. A comparação abaixo usa os `batida_id` da saída. `app/ponto/turnos.py:1997`, `app/ponto/motor_calculo_v2.py:503`

### col129

| Batida na saída | Ramo do pareador | Teto aplicado | Resultado nos dois lotes | [nao verificado]
|---|---|---|---|
| E **155814**, 15/11 17:30 | Teto da jornada. O vão de **779,991 min** deixou a E aberta. A tentativa de fechar com a S seguinte também excedeu o teto. `app/ponto/turnos.py:1447`, `app/ponto/turnos.py:1453`, `app/ponto/turnos.py:1093` | **732 min** = 492 min entre `hi` e `hf` do DNA de **15/11** + 240 min, por `_teto_da_jornada`. `cont_max_s` entrou com 840 min. As distâncias E→`hi` e S→`hf` foram **239,749 / 48,241 min**, e a L-084 exige ambas acima de 180 min para trocar o teto. `app/ponto/turnos.py:66`, `app/ponto/turnos.py:1193`, `app/ponto/turnos.py:1430`–`1447` | **E aberta** em 15/11. `17:30` aparece em `dia['orfas']` nas duas grades. `app/ponto/turnos.py:1453`, `app/escala/utils.py:2334`, `app/escala/services/leitor_celula.py:420` |
| S **156153**, 16/11 06:30 | A mesma comparação emitiu a **S sem entrada** e gravou `_separada_pelo_teto`. `app/ponto/turnos.py:1451`–`1454` | **732 min**, calculados com o DNA de **15/11**, data da E candidata. `app/ponto/turnos.py:66`, `app/ponto/turnos.py:1430`–`1434` | **S órfã** em 16/11 no pareador. `06:30` ficou fora de `dia['orfas']` nas duas grades consultadas. `app/ponto/turnos.py:1454`, `app/escala/utils.py:2334`, `app/escala/services/leitor_celula.py:420` |
| Escala/célula | Vínculo **853**, template **198** (6×1). DNA recebido pelos dois lotes de 14 a 17/11, na ordem `(hi,hf,hii,hfi)` = **(21:30, 05:42, ∅, ∅)**. Células na grade lavrada **140135** em 15/11 e **140136** em 16/11, ambas de trabalho. `app/escala/utils.py:84`, `app/ponto/turnos.py:1997`, `app/ponto/motor_calculo_v2.py:503`, `app/escala/services/leitor_celula.py:397`, `app/escala/services/leitor_celula.py:424` | | |

### col155

| Batida na saída | Ramo do pareador | Teto aplicado | Resultado nos dois lotes | [nao verificado]
|---|---|---|---|
| E **155813**, 15/11 17:29 | Teto da jornada. O vão de **782,822 min** deixou a E aberta. A tentativa de fechar com a S seguinte também excedeu o teto. `app/ponto/turnos.py:1447`, `app/ponto/turnos.py:1453`, `app/ponto/turnos.py:1093` | **732 min** = 492 min entre `hi` e `hf` do DNA de **15/11** + 240 min, por `_teto_da_jornada`. `cont_max_s` entrou com 840 min. Distâncias E→`hi` e S→`hf` de **240,640 / 50,183 min** mantiveram o teto do dia pela L-084. `app/ponto/turnos.py:66`, `app/ponto/turnos.py:1193`, `app/ponto/turnos.py:1430`–`1447` | **E aberta** em 15/11. `17:29` aparece em `dia['orfas']` nas duas grades. `app/ponto/turnos.py:1453`, `app/escala/utils.py:2334`, `app/escala/services/leitor_celula.py:420` |
| S **156154**, 16/11 06:32 | A mesma comparação emitiu a **S sem entrada** e gravou `_separada_pelo_teto`. `app/ponto/turnos.py:1451`–`1454` | **732 min**, calculados com o DNA de **15/11**, data da E candidata. `app/ponto/turnos.py:66`, `app/ponto/turnos.py:1430`–`1434` | **S órfã** em 16/11 no pareador. `06:32` ficou fora de `dia['orfas']` nas duas grades consultadas. `app/ponto/turnos.py:1454`, `app/escala/utils.py:2334`, `app/escala/services/leitor_celula.py:420` |
| Escala/célula | Vínculo **1437**, template **198** (6×1). DNA recebido pelos dois lotes de 14 a 17/11 **(21:30, 05:42, ∅, ∅)**. Células na grade lavrada **131754** em 15/11 e **131755** em 16/11, ambas de trabalho. `app/escala/utils.py:84`, `app/ponto/turnos.py:1997`, `app/ponto/motor_calculo_v2.py:503`, `app/escala/services/leitor_celula.py:397`, `app/escala/services/leitor_celula.py:424` | | |

### col214

| Batida na saída | Ramo do pareador | Teto aplicado | Resultado nos dois lotes | [nao verificado]
|---|---|---|---|
| E **155565**, 15/11 07:00 | **Fecho normal** com a S **155668** às 12:06. `07:00` aparece em `dia['orfas']` nas duas grades. **Não responde** qual passo a colocou na grade viva, porque a sonda traz a lista final. A grade da célula lê a lista da ata. `app/ponto/turnos.py:1491`–`1493`, `app/escala/utils.py:2334`, `app/escala/services/leitor_celula.py:187`, `app/escala/services/leitor_celula.py:420` | **1.560 min**, envelope de 26 h do fecho. A L-084 substituiu os **740 min** de `_teto_da_jornada` (500 do DNA de 15/11 + 240), pois as distâncias **339,654 / 533,327 min** superaram 180 min. Vão de **306,327 min**. `cont_max_s` entrou com 840 min. `app/ponto/turnos.py:66`, `app/ponto/turnos.py:1193`, `app/ponto/turnos.py:1430`–`1447` | **Mesmo par fechado 155565→155668** em 15/11. `app/ponto/turnos.py:1997`, `app/ponto/motor_calculo_v2.py:503` |
| Escala/célula | Vínculo **182**, template **220** (6×1). DNA recebido pelos dois lotes de 14 a 17/11 **(12:40, 21:00, ∅, ∅)**. Célula de trabalho **122012** em 15/11 na grade lavrada. `app/escala/utils.py:84`, `app/ponto/turnos.py:1997`, `app/ponto/motor_calculo_v2.py:503`, `app/escala/services/leitor_celula.py:397`, `app/escala/services/leitor_celula.py:424` | | |

### col107

| Batida na saída | Ramo do pareador | Teto aplicado | Resultado nos dois lotes | [nao verificado]
|---|---|---|---|
| E **155585**, 15/11 07:05 | **Fecho normal** com a S **155607**. `07:05` aparece em `dia['orfas']` nas duas grades. **Não responde** qual passo a colocou na grade viva, porque a sonda traz a lista final. `app/ponto/turnos.py:1491`–`1493`, `app/escala/utils.py:2334`, `app/escala/services/leitor_celula.py:420` | **1.560 min**, envelope de 26 h do fecho. A L-084 substituiu os **750 min** de `_teto_da_jornada` (510 do DNA de 15/11 + 240), pois as distâncias **344,668 / 715,161 min** superaram 180 min. Vão de **129,829 min**. `cont_max_s` entrou com 840 min. `app/ponto/turnos.py:66`, `app/ponto/turnos.py:1193`, `app/ponto/turnos.py:1430`–`1447` | **E do par fechado 155585→155607** em 15/11. `app/ponto/turnos.py:1997`, `app/ponto/motor_calculo_v2.py:503` |
| S **155607**, 15/11 09:15 | **Fecho normal**, como saída de **155585**. `09:15` aparece em `dia['orfas']` nas duas grades. **Não responde** qual passo a colocou na grade viva, porque a sonda traz a lista final. `app/ponto/turnos.py:1491`–`1493`, `app/escala/utils.py:2334`, `app/escala/services/leitor_celula.py:420` | **1.560 min**, na mesma comparação. A S estava a **324,839 min** do marco de pausa mais próximo, fora dos **90 min** do ramo de absorção. `app/ponto/turnos.py:1356`–`1365`, `app/ponto/turnos.py:1444`–`1447` | **S do mesmo par fechado** em 15/11. `app/ponto/turnos.py:1997`, `app/ponto/motor_calculo_v2.py:503` |
| Escala/célula | Vínculo **1203**, template **181** (6×1). DNA recebido pelos dois lotes de 15 a 17/11 **(12:50, 21:20, 14:40, 15:50)**. Em 14/11 o mapa veio sem DNA e o EC retornou esses mesmos marcos. Célula de trabalho **123743** em 15/11 na grade lavrada. `app/escala/utils.py:84`, `app/escala/models.py:1135`–`1144`, `app/ponto/turnos.py:1997`, `app/ponto/motor_calculo_v2.py:503`, `app/escala/services/leitor_celula.py:397`, `app/escala/services/leitor_celula.py:424` | | |

## 23/11 15:10 · sessao 48945df890d9 · PAR-PELO-TURNO · leitura · origem=Code

```python
# PAR-PELO-TURNO | eac7a67419eb54c3af01f061dde311b5bae06717
# Na rotina de ensaio da sombra, envie este texto pela entrada padrão de
# `python -B manage.py shell`, com DJANGO_SETTINGS_MODULE=config.settings.sombra.
# A rotina monta a cópia em somente leitura e usa tmpfs para os logs. bin/sombra.sh:182
# Execução e conteúdo do banco de ensaio [nao verificado].

import linecache
import logging
import sys
from datetime import date, datetime, time, timedelta

from django.db import connection, transaction
from django.utils import timezone
from django_tenants.utils import schema_context

from colaboradores.models import Colaborador
from escala import utils as grade
from escala.services.leitor_celula import grade_da_celula
from escala.servico_jornada import escala_vigente
from ponto import motor_calculo_v2 as motor
from ponto import turnos
from ponto.services.espelho import autoridade_do_periodo

T = "app/ponto/turnos.py"
M = "app/ponto/motor_calculo_v2.py"
G = "app/escala/utils.py"
INI = date(2026, 11, 14)
FIM = date(2026, 11, 17)
DIA_15 = date(2026, 11, 15)
DIA_16 = date(2026, 11, 16)
UM_DIA = timedelta(days=1)
DIAS = tuple(INI + i * UM_DIA for i in range((FIM - INI).days + 1))

# Horas de referência dadas no enunciado e em aval_v2_anexo.txt:1.
ALVOS = {
    129: ((DIA_15, "17:30"), (DIA_16, "06:30")),
    155: ((DIA_15, "17:29"), (DIA_16, "06:32")),
    214: ((DIA_15, "07:00"),),
    107: ((DIA_15, "07:05"), (DIA_15, "09:15")),
}


def emitir(cid, fato, fonte, **campos):
    dados = " ".join(f"{k}={v!r}" for k, v in campos.items())
    print(f"col{cid} {fato} | {dados} | {fonte}", flush=True)


def local(ts):
    return timezone.localtime(ts).isoformat() if ts is not None else None


def batida(b):
    if b is None:
        return None
    return (b.pk, local(b.timestamp), b.tipo, b.is_intervalo)


def horas(marcos):
    if marcos is None:
        return None
    return tuple(h.isoformat() if h is not None else None for h in marcos)


def vao(entrada, saida):
    if entrada is None or saida is None:
        return None
    return (saida.timestamp - entrada.timestamp).total_seconds() / 60


def teto(marcos, envelope_s):
    segundos, do_dia = turnos._teto_da_jornada(marcos, envelope_s)
    return {"min": segundos / 60, "do_dia": do_dia}


def papel_e_flags(b):
    return {
        "papel_ata": getattr(b, "_papel_ata", None),
        "intra_dur": bool(getattr(b, "_intra_dur", False)),
        "eco_flush": bool(getattr(b, "_eco_flush", False)),
    }


def carimbo(b):
    valor = getattr(b, "_separada_pelo_teto", None)
    if valor is None:
        return None
    entrada, marcos, segundos = valor
    return {"entrada": batida(entrada), "marcos": horas(marcos), "teto_min": segundos / 60}


def imprimir_turnos(cid, leitor, resultado, fonte):
    if not resultado:
        emitir(cid, "Turno", fonte, leitor=leitor, quantidade=0)
    for t in resultado:
        bordas = {b.pk for b in (t.entrada, t.saida) if b is not None}
        emitir(
            cid, "Turno", fonte, leitor=leitor, data_turno=t.data_turno.isoformat(),
            entrada=batida(t.entrada), saida=batida(t.saida), aberto=t.aberto,
            internas=[(batida(b), papel_e_flags(b)) for b in t.batidas if b.pk not in bordas],
            vao_E_S_min=vao(t.entrada, t.saida),
            _separada_pelo_teto=carimbo(t.saida),
        )
    pares = turnos.pares_separados_pelo_teto(resultado)
    if not pares:
        emitir(cid, "pares_separados_pelo_teto", f"{T}:183", leitor=leitor, quantidade=0)
    for p in pares:
        emitir(
            cid, "pares_separados_pelo_teto", f"{T}:183", leitor=leitor,
            data_turno=p["data_turno"].isoformat(), entrada=batida(p["entrada"]),
            saida=batida(p["saida"]), hi_hf=horas((p["hi"], p["hf"])),
            teto_min=p["teto_min"], vao_E_S_min=p["vao_min"],
        )


# Os números são os pontos de execução da cópia indicada. A checagem evita
# atribuir o nome de um ramo a uma linha deslocada em outra revisão.
PONTOS = {
    1353: "turnos.append(_mk(cur, None",
    1376: "continue",
    1401: "continue",
    1416: "continue",
    1418: "turnos.append(_mk(cur, None",
    1444: "if _de_f >",
    1447: "> _teto_fecho:",
    1453: "turnos.append(_mk(cur, None",
    1477: "b._intra_dur = True",
    1488: "_declara_degenerado",
    1496: 'turnos.append(_mk({"entrada": None',
    1498: "turnos.append(_mk(cur, None",
    1094: "i += 1",
    1184: "out.append(t)",
}


class Rastro:
    def __init__(self, cid, batidas_alvo):
        self.cid = cid
        self.alvos = {b.pk for b in batidas_alvo}
        self.lotes = []
        self.ativos = {}

    def __call__(self, frame, evento, arg):
        if evento != "call":
            return None
        codigo = frame.f_code
        pai = frame.f_back
        if codigo is turnos.parear_turnos.__code__:
            if pai.f_code is turnos._turnos_do_colab_puro.__code__:
                ctx = pai.f_locals
                leitor = "turnos_do_colab"
                ini, fim, ec = ctx["seg_ini"], ctx["seg_fim"], ctx["esc"]
                classe = None
            elif pai.f_code is motor.turnos_via_autoridade.__code__:
                instancia = pai.f_locals["motor"]
                calculo = pai.f_back
                while calculo is not None and calculo.f_code is not motor.MotorBase.calcular_mes.__code__:
                    calculo = calculo.f_back
                ctx = calculo.f_locals
                leitor = "turnos_via_autoridade"
                ini, fim, ec = ctx["data_ini"], ctx["data_fim"], ctx["escala_colaborador"]
                classe = type(instancia).__name__
            else:
                return None
            lote = {
                "leitor": leitor, "ini": ini, "fim": fim, "ec_id": getattr(ec, "pk", None),
                "classe": classe, "numero": len(self.lotes) + 1, "ramos": set(),
            }
            self.lotes.append(lote)
            self.ativos[id(frame)] = lote
            return self.pareador
        if codigo in (
            turnos._fechar_aberto_com_saida_seguinte.__code__,
            turnos._fechar_aberto_na_pausa_sem_volta.__code__,
        ) and id(pai) in self.ativos:
            return self.pos_fechamento
        return None

    def fato(self, lote, linha, nome, envolvidos, *, ramo=True, **dados):
        ids = sorted(self.alvos.intersection(b.pk for b in envolvidos if b is not None))
        if ids:
            if ramo:
                lote["ramos"].update(ids)
            emitir(self.cid, nome, f"{T}:{linha}", leitor=lote["leitor"],
                   lote=lote["numero"], batidas_alvo=ids, **dados)

    def pareador(self, frame, evento, arg):
        lote = self.ativos[id(frame)]
        l = frame.f_locals
        if evento == "return":
            self.ativos.pop(id(frame))
            if arg is not None:
                lote["resultado"] = arg
                lote["batidas"] = l["bs"]
                lote["mapa"] = l["marcos_por_dia"] or {}
                lote["borda_template"] = l["marcos_borda"]
                lote["intervalo_template"] = l["marcos_intervalo"]
                lote["papeis"] = l["papel_por_minuto"] or {}
                lote["cont_max_s"] = l["cont_max_s"]
                lote["intra_max_s"] = l["intra_max_s"]
                lote["distancias"] = {
                    b.pk: (
                        horas(l["_bm_de"](b)),
                        turnos._dist_marco(b.timestamp, l["_bm_de"](b)),
                        horas(l["_ivm_de"](b)),
                        turnos._dist_marco(b.timestamp, l["_ivm_de"](b)),
                    )
                    for b in l["bs"] if b.pk in self.alvos
                }
            return self.pareador
        if evento != "line" or frame.f_lineno not in PONTOS:
            return self.pareador

        linha = frame.f_lineno
        b, cur = l.get("b"), l.get("cur")
        e = cur["entrada"] if cur else None
        if linha in (1444, 1447):
            if linha == 1444:
                self.fato(lote, linha, "distâncias L-084", (e, b), ramo=False,
                          entrada=batida(e), saida=batida(b),
                          dist_E_hi_min=l["_de_f"], dist_S_hf_min=l["_ds_f"],
                          corte_min=motor.MotorBase.CADASTRO_NAO_DESCREVE_MIN)
            else:
                self.fato(lote, linha, "comparação efetiva E→S", (e, b), ramo=False,
                          entrada=batida(e), saida=batida(b), vao_E_S_min=vao(e, b),
                          data_do_mapa=turnos.data_local(e.timestamp).isoformat(),
                          marcos=horas(l["_m_fecho"]),
                          teto_da_jornada=teto(l["_m_fecho"], l["_envelope_fecho"]),
                          teto_efetivo_min=l["_teto_fecho"] / 60,
                          do_dia_efetivo=l["_do_dia_fecho"])
        elif linha == 1453:
            c = getattr(b, "_separada_pelo_teto", None)
            if c is not None and c[0].pk == e.pk:
                motivo = "ramo teto do dia decisivo"
            elif not l["_do_dia_fecho"]:
                motivo = "ramo envelope do fechamento"
            else:
                motivo = "ramo fechamento acima do teto sem carimbo decisivo"
            self.fato(lote, linha, motivo, (e, b), entrada=batida(e), saida=batida(b),
                      _separada_pelo_teto=carimbo(b))
        elif linha == 1353:
            self.fato(lote, linha, "ramo absorção cont_max_s encerra a tentativa", (e, b),
                      entrada=batida(e), marca=batida(b), vao_desde_E_min=vao(e, b),
                      cont_max_min=l["cont_max_s"] / 60, dist_interna_min=l["_di_fora"])
        elif linha == 1376:
            motivo = "papel da ata" if l["_pa_b"] in ("X", "Xi") else "intervalo"
            self.fato(lote, linha, f"ramo {motivo} sem borda", (b,), marca=batida(b),
                      entrada_aberta=batida(e), **papel_e_flags(b))
        elif linha == 1401:
            self.fato(lote, linha, "ramo eco de flush", (b,), marca=batida(b), entrada_aberta=batida(e))
        elif linha in (1416, 1477):
            self.fato(lote, linha, "ramo pausa deslocada", (b,), marca=batida(b),
                      entrada_aberta=batida(e))
        elif linha == 1418:
            self.fato(lote, linha, "ramo nova E deixa a E anterior aberta", (e,),
                      entrada_aberta=batida(e), nova_entrada=batida(b), papel_nova_E=l["_pa_b"])
        elif linha == 1488:
            self.fato(lote, linha, "ramo par degenerado", (e, b),
                      entrada=batida(e), saida=batida(b), vao_E_S_min=vao(e, b))
        elif linha == 1496:
            self.fato(lote, linha, "ramo S sem E aberta", (b,),
                      saida=batida(b), **papel_e_flags(b))
        elif linha == 1498:
            self.fato(lote, linha, "ramo fim da sequência com E aberta", (e,), entrada=batida(e))
        return self.pareador

    def pos_fechamento(self, frame, evento, arg):
        if evento == "line" and frame.f_lineno in (1094, 1184):
            linha = frame.f_lineno
            l = frame.f_locals
            t = l["t"]
            s = l["_sai"] if linha == 1094 else l["_last"]
            lote = self.ativos[id(frame.f_back)]
            self.fato(
                lote, linha, f"ramo teto em {frame.f_code.co_name}", (t.entrada, s),
                data_turno=t.data_turno.isoformat(), entrada=batida(t.entrada), saida=batida(s),
                vao_E_S_min=l["dur_s"] / 60,
                teto_da_jornada=teto((l["marcos_por_dia"] or {}).get(t.data_turno), l["cont_max_s"]),
            )
        return self.pos_fechamento


def relatar_lote(cid, lote, batidas_alvo):
    leitor = f'{lote["leitor"]} lote {lote["numero"]}'
    fonte = f"{M}:503" if lote["classe"] else f"{T}:1997"
    emitir(cid, "alimentação do pareador", fonte, leitor=leitor,
           janela=(lote["ini"].isoformat(), lote["fim"].isoformat()),
           vinculo_id=lote["ec_id"], motor=lote["classe"],
           cont_max_min=lote["cont_max_s"] / 60, intra_max_min=lote["intra_max_s"] / 60,
           borda_template=horas(lote["borda_template"]),
           intervalo_template=horas(lote["intervalo_template"]),
           batidas_entrada=[batida(b) for b in lote["batidas"]])

    for d in DIAS:
        marcos = lote["mapa"].get(d)
        emitir(cid, "DNA passado ao pareador e tetos por data de referência",
               f"{fonte} {T}:66 {T}:1086 {T}:1177 {T}:1434", leitor=leitor,
               data_turno_referencia=d.isoformat(), hi_hf_hii_hfi=horas(marcos),
               teto_base_fecho=teto(marcos, 26 * 3600),
               teto_pos_fecho=teto(marcos, lote["cont_max_s"]))

    papeis = lote["papeis"]
    if not papeis:
        emitir(cid, "papel_por_minuto_da_ata", f"{T}:1565 {fonte}", leitor=leitor, papel=None)
    for minuto, papel in sorted(papeis.items()):
        ts = timezone.make_aware(minuto) if timezone.is_naive(minuto) else minuto
        emitir(cid, "papel_por_minuto_da_ata", f"{T}:1565 {fonte}",
               leitor=leitor, minuto=local(ts), papel=papel)

    resultado = lote["resultado"]
    if lote["classe"]:
        imprimir_turnos(cid, leitor, resultado, f"{M}:503 {T}:43 {T}:180")
    bs = lote["batidas"]
    indices = {b.pk: i for i, b in enumerate(bs)}
    for alvo in batidas_alvo:
        i = indices.get(alvo.pk)
        if i is None:
            emitir(cid, "alvo ausente da entrada deste lote; ramo [nao verificado]",
                   fonte, leitor=leitor, alvo=batida(alvo))
            continue
        b = bs[i]
        bordas, dist_borda, marcos_intra, dist_intra = lote["distancias"][b.pk]
        e_ref = next((x for x in reversed(bs[:i]) if x.tipo == "E"), None)
        s_ref = next((x for x in bs[i + 1:] if x.tipo == "S"), None)
        destinos = []
        for t in resultado:
            if not any(x.pk == b.pk for x in t.batidas):
                continue
            if t.entrada is not None and t.entrada.pk == b.pk:
                papel = "E aberta" if t.aberto else "E de par"
            elif t.saida is not None and t.saida.pk == b.pk:
                papel = "S órfã" if t.entrada is None else "S de par"
            else:
                papel = "interna"
            destinos.append((t.data_turno.isoformat(), papel, batida(t.entrada), batida(t.saida),
                             bool(lote["ini"] <= t.data_turno <= lote["fim"])))
        emitir(
            cid, "alvo no resultado do pareador", f"{T}:326 {T}:1237 {T}:1382 {T}:1997",
            leitor=leitor, alvo=batida(b), destinos_e_data_na_fatia=destinos,
            hi_hf_efetivos=bordas, dist_borda_mais_proxima_min=dist_borda,
            marcos_intra_efetivos=marcos_intra, dist_intra_mais_proxima_min=dist_intra,
            E_gravada_anterior=batida(e_ref), S_gravada_posterior=batida(s_ref),
            vao_referencia_E_anterior_ate_alvo_min=vao(e_ref, b),
            vao_referencia_alvo_ate_S_posterior_min=vao(b, s_ref),
            **papel_e_flags(b),
        )
        if b.pk not in lote["ramos"] and (
            not destinos or any(d[1] in ("E aberta", "S órfã", "interna") for d in destinos)
        ):
            emitir(cid, "ramo [nao verificado]", fonte, leitor=leitor, alvo=batida(b))


def imprimir_grade(cid, fonte, nome, por_data, dados_grade):
    for d in (DIA_15, DIA_16):
        dia = por_data.get(d)
        if dia is None:
            emitir(cid, "dia da grade [nao verificado]", fonte, leitor=nome, data=d.isoformat())
            continue
        orfas = dia["orfas"]
        _, orfas_realizado = grade.montar_realizado_grade(dia, dados_grade["marcos"])
        emitir(cid, "dia['orfas']", fonte, leitor=nome, data=d.isoformat(),
               tipo_dia=dia.get("tipo_dia"), celula_id=dia.get("celula_id"), orfas=orfas)
        for civil, hhmm in ALVOS[cid]:
            emitir(cid, "presença na lista por HH:MM", f"{fonte} {G}:1089", leitor=nome,
                   data_grade=d.isoformat(), data_civil_alvo=civil.isoformat(), hora_alvo=hhmm,
                   em_dia_orfas=hhmm in orfas, em_montar_realizado_grade=hhmm in orfas_realizado,
                   orfa_sem_hora="?" in orfas)


def sondar(cid, agora):
    colab = Colaborador.objects.select_related("empresa").get(pk=cid)
    emitir(cid, "empresa do colaborador", "app/colaboradores/models.py:150 app/colaboradores/models.py:321",
           empresa_id=colab.empresa_id, regime_trabalhista=colab.empresa.regime_trabalhista)

    inicio = timezone.make_aware(datetime.combine(INI, time.min))
    fim = timezone.make_aware(datetime.combine(FIM, time.max))
    bs = list(turnos.batidas_apuraveis(colab, inicio, fim))
    for b in bs:
        emitir(cid, "batida apurável", f"{T}:2877 app/ponto/models.py:23",
               batida_id=b.pk, timestamp=local(b.timestamp), tipo=b.tipo,
               is_intervalo=b.is_intervalo, retratada_em=local(b.retratada_em))

    por_minuto = {}
    for b in bs:
        ts = timezone.localtime(b.timestamp)
        por_minuto.setdefault((ts.date(), ts.strftime("%H:%M")), []).append(b)
    batidas_alvo = []
    for d, hhmm in ALVOS[cid]:
        correspondentes = por_minuto.get((d, hhmm), [])
        batidas_alvo.extend(correspondentes)
        emitir(cid, "referência do caso", f"aval_v2_anexo.txt:1 {T}:2877",
               data_civil=d.isoformat(), hora=hhmm,
               apuraveis=[batida(b) for b in correspondentes])
        if not correspondentes:
            emitir(cid, "ramo [nao verificado]", f"{T}:2877",
                   data_civil=d.isoformat(), hora=hhmm, apuravel_no_minuto=False)

    dna = grade.marcos_dna_periodo(cid, INI, FIM)
    for d in DIAS:
        ec = escala_vigente(colab, d)
        marcos_ec = ec.marcos_do_dia(d) if ec else None
        emitir(cid, "marcos do dia",
               f"{G}:84 app/escala/servico_jornada.py:15 app/escala/models.py:1109",
               data=d.isoformat(), vinculo_id=getattr(ec, "pk", None),
               EC_hi_hf_hii_hfi=horas(marcos_ec), DNA_hi_hf_hii_hfi=horas(dna.get(d)))
        if d == DIA_15:
            te = ec.tipo_escala if ec else None
            emitir(cid, "template vigente em 15/11",
                   "app/escala/servico_jornada.py:15 app/escala/models.py:46 app/escala/models.py:117 app/escala/models.py:130",
                   vinculo_id=getattr(ec, "pk", None), template_id=getattr(te, "pk", None),
                   nome=(te.template_nome or te.nome_canonico()) if te else None,
                   hi_hf=horas((te.hora_inicio, te.hora_fim)) if te else None,
                   ciclo=te.tipo_ciclo if te else None, ciclo_dias=te.ciclo_dias if te else None)

    emitir(cid, "caminho de produção do motor",
           "app/ponto/services/espelho.py:1263 app/ponto/services/espelho.py:1147 "
           "app/ponto/services/espelho.py:1200 app/ponto/services/fechamento.py:286 "
           "app/ponto/services/fechamento.py:735 app/ponto/services/fechamento.py:754 "
           f"{M}:530",
           caminho="autoridade_do_periodo → get_motor_cct por fatia → calcular_mes → turnos_via_autoridade")

    rastro = Rastro(cid, batidas_alvo)
    anterior = sys.gettrace()
    sys.settrace(rastro)
    try:
        juiz = turnos.turnos_do_colab(colab, INI, FIM)
        autoridade = autoridade_do_periodo(colab, INI, FIM)
    finally:
        sys.settrace(anterior)

    imprimir_turnos(cid, "turnos_do_colab", juiz, f"{T}:1850 {T}:1997 {T}:43 {T}:180")
    for lote in rastro.lotes:
        relatar_lote(cid, lote, batidas_alvo)
    if not any(lote["classe"] for lote in rastro.lotes):
        emitir(cid, "motor sem chamada observada a turnos_via_autoridade; ramo [nao verificado]",
               f"app/ponto/services/espelho.py:1198 {M}:530")

    viva = grade.grade_espelho_janela(colab, autoridade.batidas, INI, FIM, agora=agora)
    por_data_viva = {FIM - i * UM_DIA: dia for i, dia in enumerate(viva["dias"])}
    imprimir_grade(cid, f"{G}:2334 {G}:962", "grade viva", por_data_viva, viva)

    lavrada = grade_da_celula(colab, INI, FIM)
    por_data_lavrada = {dia["dat

[truncado em 20000 de 21358 caracteres]

## 23/11 15:10 · sessao e661bebbc12f · VIGIA-DO-TMUX · verificador · origem=Code

Revisão em leitura, pelo diff anexado e pelo código da cópia. Hash Git, crontab efetivo, configuração do tmux e ocorrências em produção [nao verificado]. O selo completo [nao verificado] em execução, pois cria e apaga arquivos temporários. `bin/tests/test_vigia_tmux.sh:19-26,95-110`

### 1. VEREDITO **ok**

O bloco antigo do anexo e o novo fazem as mesmas operações, na mesma ordem. Removem as crases finais, dividem por `|`, aparam os campos e exigem uma palavra para o produtor. Aplicam os mesmos `fullmatch` para `pid=` e `arquivo=`, convertem o PID com `int`, rejeitam campo desconhecido ou repetido com o mesmo trecho de 30 caracteres e rejeitam a ausência dos dois campos. O chamador reconstrói a mesma tupla `(True, motivo, linha)`, conserva a prova de PID antes da de arquivo e contém as mesmas exceções. Os dois textos de `pid morto` também permanecem iguais. Sem contraexemplo no diff fornecido. `bin/hook_stop_fila1.py:599-628,788-849` · anexo `v.diff`, hunks `-596`, `-614`, `-780` e `-806`

O B3 usa o recorte compartilhado sobre uma leitura consistente do RELATO e reconhece `DESENHO` e `ERRO-BOBO` pelo próprio reconhecedor. Esse caminho não chama o parser de `ESPERANDO`. `bin/disjuntor.py:722-750,758-785,978,1018-1025`

### 2. VEREDITO **ACHADO**

| Estado | Quem escreve e quem lê |
|---|---|
| Topo do RELATO | A edição do painel é atribuída ao Code pelas instruções da cópia. O Disjuntor grava a faixa do censo por `no_topo`. Leem o vigia, o Stop, o B3, o handoff e o alarme de sessão, com janelas próprias para os dois últimos. Autor de cada edição em produção [nao verificado]. `CLAUDE.md:263-282` · `bin/hook_stop_fila1.py:408-484,823-839,1108-1125,1265` · `bin/vigia_tmux.py:266-274` · `bin/disjuntor.py:299-326,758-785` · `bin/handoff_sessao.sh:85-109` · `bin/alarme_sessao_ociosa.py:119-141` |
| `logs/vigia_tmux/feitos.json` | O vigia lê por `_le_feitos`, consulta pelo SHA e grava por `_marca` e `os.replace`. `quando` é o instante da última transição. `hora_clear` é o instante anterior à tentativa, embora seja usado como referência do envio. Ambos são epoch em segundos. Outro escritor efetivo [nao verificado]. `bin/vigia_tmux.py:98-119,187-199,221-255,270-274` |
| `.trava` | O vigia abre ou cria e toma `flock` exclusivo não bloqueante. O caminho usa a trava sem ler ou escrever conteúdo. Outro usuário da trava [nao verificado]. `bin/vigia_tmux.py:62,259-266` |
| `PAUSADO` | `pausar.sh` grava por renomeação, lê com `--ler` e remove com `--soltar`. O vigia só consulta existência. O selo cria seu próprio arquivo na fixture. `bin/pausar.sh:19-47` · `bin/vigia_tmux.py:63,259-261` · `bin/tests/test_vigia_tmux.sh:167` |
| `logs/sessao/ultimo_clear.json` | `marco_de_clear.py` lê o anterior e grava sob trava, chamado pelos hooks de `clear` e `compact`. O vigia lê diretamente. Stop e gerador de ESTADO passam pelo leitor do orçamento. `hora` é o epoch do último evento, inclusive `compact`. `clear_horas` guarda os últimos 20 clears sem sessão por entrada. `session_id` pertence só ao último evento. `iso` é hora local sem offset explícito. `bin/marco_de_clear.py:19-20,32-63` · `.claude/settings.json:34-59` · `bin/vigia_tmux.py:207-214` · `bin/orcamento_contexto.py:152-164,352-361` · `bin/hook_stop_fila1.py:1028-1044` · `bin/gerar_estado.py:279-284` |
| `vigia.log` | O vigia apenda por `_log`, inclusive no `excepthook`. O selo lê e cobra mensagens. Leitor operacional [nao verificado]. O log usa data completa e hora local sem offset. A entrada no RELATO usa dia/mês e hora, sem ano nem offset. Fuso efetivo do host [nao verificado]. `bin/vigia_tmux.py:80-95,170-173,187-203` · `bin/tests/test_vigia_tmux.sh:111-116,126,148,160` |
| `ALARMES DA ESTEIRA` | `esteira_vigia.no_relato` lê a seção para inserir e regrava o arquivo. Chamam a porta o vigia, a esteira, o Disjuntor, o integrador, o fabricante e o vigia da árvore. O verificador de prova do Stop e a publicação leem o RELATO inteiro. `app/core/esteira_vigia.py:815-843,895-927,1215` · `bin/vigia_tmux.py:170-184` · `bin/disjuntor.py:290-293` · `app/core/integrador_lote.py:649,743,757,768` · `bin/fabricante.sh:44,113` · `bin/esteira.sh:96` · `bin/vigia_arvore.sh:166-170` · `bin/hook_stop_fila1.py:1329-1336` · `bin/relato.sh:54-57,119` |

**Contraexemplos.** `gerar_cortes.py` e `deploy_agendado.sh` regravam o RELATO inteiro fora da trava da porta. Se um deles lê a versão anterior, o vigia insere um alarme e depois o outro gravador salva sua versão, o alarme some. Além disso, uma caixa ocupada gera `adiado` só em `vigia.log`. O retorno ocorre antes da chamada que escreveria no RELATO, contrariando o item 4 do contrato. `bin/gerar_cortes.py:114-136` · `bin/deploy_agendado.sh:31-44` · `app/core/esteira_vigia.py:839-860` · `bin/vigia_tmux.py:170-173,187-203`

### 3. VEREDITO **ACHADO**

- **Sessão.** Todos os envios deste vigia passam `=code:` e a checagem passa `=code`. O código não monta um alvo `codex-hasner`. O painel, porém, é resolvido novamente a cada chamada. Se o painel ativo de `code` mudar entre a captura, o literal e o Enter, a checagem pode observar um painel e a tecla atingir outro. A topologia real [nao verificado]. `bin/vigia_tmux.py:66-67,126-128,149-167`
- **Repetição.** Com a mesma linha normalizada, o JSON preservado e a mesma trava, `ESPERANDO` bloqueia qualquer SHA já gravado. No CLEAR, os estados pendentes permitem só a retomada. Se `feitos.json` desaparecer entre ticks, `_le_feitos` devolve `{}` e a mesma linha pode causar outro envio. Também basta tirar a crase **final** de um `ESPERANDO` já atendido para mudar o SHA e produzir a mesma frase. O leitor conserva essa crase para o hash e o parser a descarta. `bin/vigia_tmux.py:74,98-105,221-255,264-274` · `bin/hook_stop_fila1.py:490-499,793-810`
- **Turno ou caixa ocupada.** Depois da segunda captura, o vigia ainda serializa e substitui `feitos.json`. Se Ronald digitar nesse intervalo, o literal entra na caixa alterada. Depois há mais `0,3 s` antes do Enter, sem recaptura. O Enter pode submeter o texto misturado. A janela declarada como “milissegundos” omite a gravação, o segundo intervalo e a possibilidade de troca de painel. Há também falso ocioso sem corrida. Uma captura com `❯ texto da caixa`, seguida por uma linha de continuação `❯` e pela borda `────`, passa porque o leitor escolhe o último `❯`. `bin/vigia_tmux.py:20-25,108-119,126-167,187-199`

### 4. VEREDITO **ACHADO**

- **Tecla parcial.** O literal `/clear` pode entrar na caixa e o subprocesso do Enter falhar ou expirar. Com exceção, fica `enviando-clear`. Com retorno não zero, fica `falhou-clear`. Nenhum dos dois estados reabre o envio. Um erro posterior também pode ocorrer com `/clear` já submetido, por exemplo ao ler um JSON inválido do marco. Portanto, o efeito pode ser texto deixado na caixa ou `/clear` aplicado sem retomada. `bin/vigia_tmux.py:74,91-95,122-123,161-167,187-214,239-250`
- **Marca sem tecla.** Um `arquivo=` com byte NUL embutido passa pelo `\S+`. Com PID aceito e tela ociosa, `_marca` grava `enviando-esperando`. A criação do primeiro subprocesso falha com o argumento inválido e o tick seguinte abandona o mesmo SHA. O mesmo estado ocorre se o executável `tmux` desaparecer depois das capturas e antes do primeiro envio. `bin/hook_stop_fila1.py:793-810` · `bin/vigia_tmux.py:115-123,161-167,187-194,221-232`
- **Saída 0 também falha.** `VIGIA_TMUX_TETO_CLEAR=x` falha na inicialização, antes de instalar o handler. Se `vigia.log` for um diretório, o próprio handler pode levantar ao tentar registrar o erro, antes de `_exit(0)`. JSON de feitos corrompido interrompe todos os ticks que encontrarem uma linha reconhecida enquanto o arquivo permanecer assim. `bin/vigia_tmux.py:68,84-105,270-280`

### 5. VEREDITO **ACHADO**

O vigia chama a mesma `_prova_pid` do Stop. Ela lê estado e `starttime` de `/proc/<pid>/stat` antes e depois de ler `exe` e `cmdline`. Dois `starttime` diferentes dão “pid trocou”; `PermissionError` e outros erros de leitura tratados dão “pid ilegível”. O mecanismo é o de `/proc`, embora o contrato diga `kill -0`. `bin/hook_stop_fila1.py:570-574,604-631` · `bin/vigia_tmux.py:221-232`

**Contraexemplo.** O primeiro `stat` devolve `('S', 123)`, mas `readlink('/proc/4321/exe')` levanta `FileNotFoundError`. A função retorna `pid morto (4321)` sem reler o `stat`, e o vigia aceita. A exceção vale para todo o bloco de leitura. Reciclagem **antes** do primeiro `stat` também é invisível, pois o painel só fornece o PID. Se o substituto já estiver zumbi ou cair no mesmo erro de leitura, pode ser aceito como morto sem casar com o produtor. Ocorrências reais [nao verificado]. `bin/hook_stop_fila1.py:615-631,793-810` · `bin/vigia_tmux.py:221-232`

### 6. VEREDITO **ACHADO**

- **Vacuidade parcial.** `c1` aceita qualquer `morte nao provada`, inclusive `cmdline nao casa`, então pode passar sem reconhecer o produtor vivo. O caso `e` põe o comando velho na linha física 9, já atrás de uma primeira linha útil. Assim, repete a rejeição exercitada por `c10` sem medir a borda da 15ª linha. Os casos positivos exigem envios concretos. `bin/tests/test_vigia_tmux.sh:82-89,95-101,121-155,158,171-177,189-193` · `bin/vigia_tmux.py:228-231`
- **Mutante que escapa.** Trocar só o alvo de `capture-pane` para `codex-hasner` preserva as saídas esperadas. O falso não verifica o alvo da captura, as igualdades cobram os envios e o AST só confere as constantes e certas chamadas `.get`. Também escapam remover o `flock` ou a marca anterior à tecla, pois o selo roda em série e o `send-keys` falso sempre devolve zero. `bin/vigia_tmux.py:128,193,264-265` · `bin/tests/test_vigia_tmux.sh:30-51,103-113,122-155,179-187`
- **Isolamento.** A versão da cópia usa o `tmux` do `PATH` falso e repassa a raiz da fixture ao Stop e à porta do RELATO. Com `VIGIA_TMUX_BIN`, porém, o selo executa um candidato. Remover dele apenas `ev.RELATO = H.RELATO` faz a porta tentar gravar no caminho absoluto do RELATO real antes de o caso `a` detectar a falta do alarme na fixture. Um candidato que invoque `tmux` por caminho absoluto também contorna a checagem de `PATH`. Efeito no host [nao verificado]. `bin/tests/test_vigia_tmux.sh:13-17,103-108,122-129` · `bin/vigia_tmux.py:55-58,122-123,177-184` · `app/core/esteira_vigia.py:38-43,895-927`

### 7. VEREDITO **ACHADO**

- **Juízo e fonte única.** O vigia reutiliza o leitor do topo e o reconhecedor de CLEAR. O recorte compartilhado, porém, pode aceitar um `ESPERANDO` na linha 17, depois do cabeçalho e 15 linhas em branco. Também pode devolver a primeira linha após um `OSError` posterior. A autorização do CLEAR é paralela à guarda do Stop. Uma linha com o literal exigido e um painel ocioso pode disparar `/clear` enquanto o Stop recusaria a liberação por handoff velho ou DIFF/apply em curso. A leitura do marco também é paralela. Um symlink para JSON com `clear_horas` posterior é aceito pelo vigia e recusado pelo leitor do orçamento. `bin/hook_stop_fila1.py:408-416,462-484,886-887,945-962,1074-1129` · `bin/vigia_tmux.py:207-214,235-250,266-274` · `bin/orcamento_contexto.py:68-81,152-164`
- **Relógio fora da borda.** `hora_clear` é lida antes das capturas e do Enter. Se outra sessão der `/clear` entre esse instante e o envio em `code`, o marco global pode autorizar a retomada mesmo sem confirmar o clear de `code`. A comparação usa só o número, e o histórico não guarda a sessão de cada clear. `bin/vigia_tmux.py:149-167,187-194,207-214,239-250` · `bin/marco_de_clear.py:43-61`
- **Escritores e ordem de trava.** O vigia passa pela porta de ALARMES. As regravações externas do RELATO estão no item 2. Há ainda um segundo emissor de teclas na cópia, `keepalive.sh`, que usa `-t code`, flag própria e nenhuma `.trava` compartilhada. Operação simultânea em produção [nao verificado]. No vigia, a ordem observada é `.trava` → diretório do RELATO, sem caminho inverso localizado. O defeito de ordenação é consultar `PAUSADO` uma vez antes da trava. Uma pausa criada enquanto o tick espera o marco ainda permite a retomada. `bin/keepalive.sh:4-9` · `bin/pausar.sh:39-46` · `bin/vigia_tmux.py:170-204,244-274` · `app/core/esteira_vigia.py:843-862`
- **Catch-all e selo.** O teste de AST passa com zero `ExceptHandler`, mesmo com o `sys.excepthook` que contém falhas depois de efeitos. O mutante de alvo da captura do item 6 também passa pelas verificações presentes. `bin/tests/test_vigia_tmux.sh:30-51,179-187,199-202` · `bin/vigia_tmux.py:91-95,187-203,277-280`
- **Agendamento.** A fonte da cópia não contém a entrada `*/3` do vigia. O `install` usa o render de `CRONS`, portanto instalar a partir desta cópia não instala esse agendamento. Timer efetivamente instalado no host [nao verificado]. `app/config/crons.py:281-755,764-767,1895-1925,1975-1976` · `bin/crons.sh:23-27,35-40`

## 23/11 15:09 · sessao 542ac8c9a8f9 · - · - · origem=Ronald (tmux)

**6. ACHADO.** O selo tem casos positivos que comparam as teclas esperadas. O c1 aceita qualquer `morte nao provada`, inclusive `cmdline nao casa` e `ancestrais ilegiveis`, então pode passar sem confirmar a identidade do produtor vivo. O caso e põe o comando antigo na linha 9 e depois de uma linha útil. Ele cobre a precedência da primeira linha útil, como c10, e deixa a borda física de 15 linhas sem teste. `bin/tests/test_vigia_tmux.sh:82–89,95–101,121–155,158,171–177,189–193` · `bin/vigia_tmux.py:228–231` · `bin/hook_stop_fila1.py:610–630`.

- Mutantes que escapam por inspeção: retirar o `flock` ou a marcação anterior à tecla, pois o selo roda sequencialmente e o falso sempre devolve sucesso em `send-keys`. Aceitar qualquer timestamp numérico em `clear_horas` também passa, pois cada fixture fornece um marco ausente ou recente. Trocar o único argumento `texto` por `*texto.split()` deixa as mesmas strings no log falso, que recompõe argumentos com `"$*"`. `bin/vigia_tmux.py:163,193,207–214,264–265` · `bin/tests/test_vigia_tmux.sh:41–51,103–113,121–155`.
- O caminho atual usa o falso no `PATH` e redireciona tanto o leitor quanto a porta de escrita do RELATO para a fixture. O selo aceita outro `bin/`, e sua barreira não contém um mutante que invoque tmux por caminho absoluto. Remover `ev.RELATO = H.RELATO` basta para a porta tentar usar `/home/ronald/saas-hasner/app/docs/RELATO.md` antes de a asserção na fixture falhar. Efeito em produção [nao verificado]. `bin/tests/test_vigia_tmux.sh:14–15,105–108,123–129` · `bin/vigia_tmux.py:55–58,122–123,177–184` · `bin/hook_stop_fila1.py:24–32` · `app/core/esteira_vigia.py:38–43,815–843,895–927`.

**Achados complementares para 3, 4 e 7.** A janela inclui gravação em disco após a segunda captura e 0,3 s entre literal e Enter. Cada comando resolve o painel de `=code:` novamente. Uma continuação da caixa começada por `─` passa como borda. Em `CLEAR`, `hora_clear` é capturada antes da sondagem e o histórico compartilhado aceita clear de outra sessão sem comparar identidade. `bin/vigia_tmux.py:66–67,126–167,187–194,207–214,239–250` · `.claude/settings.json:34–59` · `bin/marco_de_clear.py:43–57`.

Uma falha ao iniciar o primeiro `send-keys` persiste `enviando-*` sem tecla. Um timeout no segundo pode deixar o literal sem Enter. Uma falha do próprio log impede o `_exit(0)`. `VIGIA_TMUX_TETO_CLEAR=x` levanta antes da instalação do handler. `bin/vigia_tmux.py:68,84–95,122–123,161–167,187–198,221–245,277–280`.

A fixture ociosa contém `◯ construtor-x Rodando suite`, o que o `keepalive` bloqueia e o vigia aceita. O alarme usa a captura inteira para a mesma expressão `esc to interrupt`, enquanto o vigia só considera 30 linhas. `bin/tests/test_vigia_tmux.sh:59–67,122–125` · `bin/keepalive.sh:5–8` · `bin/alarme_sessao_ociosa.py:81–86` · `bin/vigia_tmux.py:131–143`.

## 23/11 15:08 · sessao 71793dd299ef · - · - · origem=Ronald (tmux)

**1. VEREDITO ok.** O trecho antigo de `v.diff` (`@@ -806`) e `bin/hook_stop_fila1.py:793–810` executam a mesma sequência. Ambos retiram crases finais, dividem por `|`, exigem um produtor de uma palavra, casam integralmente `pid=(\d+)` e `arquivo=(\S+)`, convertem o PID em `int`, recusam campos desconhecidos ou repetidos com a mesma mensagem e recusam a ausência dos dois campos. O Stop reconstrói a mesma tupla de recusa e mantém a ordem da prova PID → arquivo e o mesmo tratamento de exceções. Os dois textos `pid morto` também são idênticos após a extração da constante. A comparação em memória com 21.960 linhas construídas a partir dos dois parsers produziu resultados iguais. O B3 usa o recorte compartilhado do topo e julga `DESENHO` e `ERRO-BOBO` em outro caminho. `bin/hook_stop_fila1.py:601,621–628,793–810,837–849` · `bin/disjuntor.py:722–750,758–785`

**5. VEREDITO ACHADO.** O vigia chama a mesma `_prova_pid` do Stop e aceita o prefixo de morte. A função extrai o `starttime` de `/proc/<pid>/stat`, compara as duas amostras e recusa mudança de processo. `PermissionError`, outros `OSError` e erros de parse tornam a prova ilegível. Já qualquer `FileNotFoundError` na leitura de `stat`, `exe` ou `cmdline` vira morte. Contraexemplo reproduzido em memória com a função da cópia: primeiro `stat` de 4321 retorna `('S', 123)`, `readlink('/proc/4321/exe')` levanta `FileNotFoundError` e o resultado é `pid morto (4321)`, antes de reler `stat` ou conferir o produtor. A reciclagem anterior à primeira leitura é indistinguível porque a linha só traz o PID. Uma troca detectada entre duas leituras completas é recusada, mas um substituto com `exe` ausente ou em `Z`/`X` pode cair no motivo de morte antes de conferir executável e argumentos. Ocorrência em produção [nao verificado]. `bin/hook_stop_fila1.py:570–574,615–631,793–809` · `bin/vigia_tmux.py:221–232`

_7958 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
