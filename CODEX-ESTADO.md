# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 08:22** · publicado em **12/11/2026 08:24** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 08:22 · sessao b901d43194d7 · LASTRO-FAIXA-SEM-DEFAULT · construtor · origem=Code

Cura deixada na árvore, com censos e resposta do PDF em [LASTRO_C11.txt:6](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/LASTRO_C11.txt:6).

- 55 testes sem banco passaram; Ruff passou ([registro:243](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/LASTRO_C11.txt:243)). [nao verificado]
- RED contra c10 reproduzido; 10 mutantes falharam por asserção, com comandos registrados ([registro:209](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/LASTRO_C11.txt:209)).
- Par financeiro da competência 11: **[nao verificado]** ([limites:334](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/LASTRO_C11.txt:334)).

## 12/11 08:12 · sessao ee40839f9d72 · - · - · origem=Ronald (tmux)

Censo na base c10, somente leitura:

| Chamador direto da A2/faixa | Destino e tratamento |
|---|---|
| `ponto/status_dia.py:298`, `:304` | Leitura D4 sem `recusas=`; levanta em dia sem batida/lavra. Corrigir os dois ramos. |
| `colaboradores/services/calendario.py:507` | Tela consulta A2 sem `recusas=` e propaga (`:515`); indexa `_fatos[data]` (`:661`) e chama `_do_dia` (`:716`). Precisa transportar a recusa ao D4 (`:606`) e à palavra, preservando `KeyError` para fato ausente sem recusa. |
| `ponto/services/dia_do_colab.py:15` | Leitor da API (`api/views_mensageria.py:1970`) e ficha (`colaboradores/services/ficha.py:280`). Levanta; deve devolver rótulo da A2 sem concluir precedência. |
| `escala/services/semana_do_posto.py:177` | API chama grade (`api/views_mensageria.py:2113`) e propostas (`:2114`). Relança em `semana_do_posto.py:178`; precisa resposta nomeada sem afirmar faixa válida. |
| `ponto/services/espelho.py:653` | Já consulta com `recusas=` (`:654`); palavra recebe `erro.rotulo` (`:880`, `:341`) sem chamar `do_dia` no recusado. |
| `ferias/situacao.py:95` | Captura específica (`:96`) e devolve aviso nomeado sem concluir descanso (`:98`). |
| `ponto/services/vigia_ausencia.py:162` | Captura específica (`:163`) e conta códigos ativos (`:160`, `:168`); manter. |
| `escala/utils.py:1209` | Builder compartilhado; julgamento/grade que alimenta dinheiro. Manter recusa, sem inventar fato válido. |
| `ponto/status_dia.py:181` | Insumo da lavra, chamado pelo cartório (`cartorio.py:932`, `:1292`, `:1853`); manter recusa. |
| `ponto/services/cartorio.py:1998` | Escrita de precedência na célula (`:1999`); manter recusa. |
| `chamados/services/feriado_reavaliar.py:33` | Julga premissa antes de reconciliar (`:43`, `:94`); manter recusa. |
| `ponto/management/commands/detectar_ausencias.py:616` | Julgamento do emissor; manter recusa. |
| `ponto/services/ausencia.py:1310` | Porta converte em `AusenciaInvalida` (`:1312`); corrigir brecha de `campos` conforme pedido. |
| `ponto/precedencia.py:544`, `:565` | Veto operacional; manter recusa. Chamadores: `ausencia.py:380`, `declarar_dia.py:120`, `escala/folgas.py:310`, `:336`, `ferias/views.py:254`. |
| `folha/services/sem_furo.py:25` | Apuração de comando (`folha/management/commands/colabs_sem_furo.py:13`); nova semântica numérica da recusa não autorizada. |
| `ponto/services/faltas_de_hoje.py:61` | Apuração de comando (`ponto/management/commands/faltas_de_hoje_sem_cobranca.py:16`); não é chamada de request identificada. |
| `ponto/services/deslogados.py:166` | Apuração/lavra (`ponto/management/commands/lavrar_deslogados.py:38`, `ponto/services/pautas_dos_deslogados.py:62`). |
| `ponto/services/abriu_nao_bateu.py:123` | Apuração/lavra (`ponto/management/commands/lavrar_abriu_nao_bateu.py:30`); API lê snapshot (`api/views_mensageria.py:139`), não executa A2 no request. |

D4 chega a leitores por `ponto/services/espelho.py:170`, `api/views.py:1483`, `colaboradores/services/calendario.py:606`, `relatorios/furos.py:123` e `core/services/painel_op.py:336`. A forma unitária delega ao lote (`ponto/status_dia.py:218`).

Fechamento continua propagando nas três capturas (`ponto/services/fechamento.py:1081`, `:1089`, `:1308`); TXT chama a leitura antes de gerar (`folha/views.py:208`), e simulação chama fechamento (`folha/management/commands/simular_folha.py:123`, `:130`).

`dia_decidido.do_dia` tem dois chamadores operacionais: espelho (`ponto/services/espelho.py:343`) e calendário (`colaboradores/services/calendario.py:716`). Os `do_dia` de calculador, pergunta no app e vigia são homônimos distintos (`ponto/calculador/regras.py:576`, `chamados/services/pergunta_no_app.py:211`, `ponto/services/vigia_entrada.py:111`).

Execução HTTP/ORM e ocorrência produtiva: [nao verificado].

## 12/11 08:05 · sessao 553a6d49936c · FIO-E0-V4-E4B · construtor · origem=Code

Cura aplicada. A [entrega completa](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c14_entrega.txt:7) contém as 69 classificações R/C, escritores/leitores e comandos individuais. [nao verificado]

- GREEN: [158 testes de memória](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c14_memoria.txt:59) e [47 contratos estáticos](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c14_selos.txt:6). [nao verificado]
- [21 mutantes c14](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c14_entrega.txt:123) morderam por assertion, sem ERROR.

Suíte inteira, ORM e medição SQL do C8: **[nao verificado]**; Django está ausente e banco foi vedado. Sem commit, conforme a proibição explícita das REGRAS. [Pendências para o Code](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c14_entrega.txt:194). [nao verificado]

## 12/11 08:02 · sessao f2aeed7d1ba0 · LASTRO-FAIXA-SEM-DEFAULT · verificador · origem=Code

**REPROVA c10:** o rótulo da recusa não garante que a tela renderize: uma consulta posterior volta a levantar `TipoSemFaixa` (`app/ponto/views.py:184`, `app/ponto/services/espelho.py:170`, `app/ponto/status_dia.py:304`). O par financeiro da competência 11 exigido pelo aval não está demonstrado no dado fornecido [nao verificado].

1. **PORTA e escritores**

   A criação, inclusive inativa, e a ativação explícita consultam `faixa_de` antes da escrita; a recusa usa `AusenciaInvalida(codigo='tipo_sem_faixa')` (`app/ponto/services/ausencia.py:1308`, `app/ponto/services/ausencia.py:1312`).

   Não encontrei segundo escritor operacional de `TipoAusencia` nas views, commands ou importadores pesquisados. O admin Django não registra esse modelo; os dois ramos da view passam pela porta. A migration 0052 escreve diretamente código/efeito/ativo, mas suas sementes têm faixa; 0062/0063 alteram flags de tipos existentes (`app/ponto/admin.py:11`, `app/ponto/admin.py:37`, `app/ponto/views_tipos_ausencia.py:87`, `app/ponto/views_tipos_ausencia.py:98`, `app/ponto/migrations/0052_seed_tipoausencia.py:33`, `app/ponto/precedencia.py:52`, `app/ponto/migrations/0062_fim_dos_17_tipos.py:51`, `app/ponto/migrations/0063_atestado_aceita_horas.py:20`).

   **Achado — fila, brecha da própria porta.** Ela valida `tipo.codigo` antes de aplicar indiscriminadamente `campos`. Contraexemplo reproduzido em memória: um objeto `abono` aceita `campos={'codigo':'fantasma_xyz','ativo':True}` e chama `save()` com o código desconhecido ativo. A view atual não fornece `codigo` nesse dicionário; exploração por essa tela não foi demonstrada (`app/ponto/services/ausencia.py:1310`, `app/ponto/services/ausencia.py:1314`, `app/ponto/views_tipos_ausencia.py:67`).

   A view não lê `.codigo`: entrega `str(erro)` ao feedback. Para `fantasma_xyz`, o texto é **“Tipo sem faixa de precedencia: fantasma_xyz”**; o JavaScript escreve na caixa de erro e no toast (`app/ponto/precedencia.py:100`, `app/ponto/views_tipos_ausencia.py:89`, `app/ponto/views_tipos_ausencia.py:100`, `app/core/respostas.py:65`, `app/templates/ponto/tipos_ausencia.html:73`, `app/templates/ponto/tipos_ausencia.html:86`).

   Os leitores gerais de `AusenciaInvalida.codigo` encontrados são API, formulário de ausência e comando de rederivação; eles não são os chamadores do cadastro de tipos. O novo valor tem assert específico no ensaio c10 (`app/api/views.py:1819`, `app/ponto/views.py:2806`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:172`, `app/ponto/tests/test_lastro_c10_sem_banco.py:86`).

2. **Cartório: isolamento, persistência e contadores**

   O `except` envolve **`julgar_colab` inteiro**, não cada dia nem a empresa inteira. Captura somente `TipoSemFaixa`, incrementa uma vez por colaborador recusado, loga pk/código e continua (`app/ponto/management/commands/processar_cartorio.py:64`, `app/ponto/management/commands/processar_cartorio.py:70`, `app/ponto/management/commands/processar_cartorio.py:72`).

   No caminho normal identificado, a grade do período consulta a A2 antes da primeira lavra. Um tipo desconhecido interrompe essa preparação: nenhuma ata nova daquele colaborador é escrita por esse caminho; as antigas permanecem (`app/ponto/services/cartorio.py:1168`, `app/escala/utils.py:1209`, `app/ponto/precedencia.py:419`, `app/ponto/services/cartorio.py:1288`).

   Isso **não constitui rollback por colaborador**. Se a exceção surgir depois de uma gravação, ela pode permanecer: o `finally` limpa contexto/filas, sem desfazer persistência. Há `atomic()` por salvamento de célula/evento, não envolvendo o colaborador inteiro (`app/ponto/services/cartorio.py:1093`, `app/ponto/services/cartorio.py:1116`, `app/ponto/portas/celula.py:899`, `app/ponto/portas/celula.py:904`). Nesse eventual estado parcial, as atas já salvas continuam disponíveis ao leitor da célula (`app/escala/services/leitor_celula.py:418`).

   Não há fundamento para atribuir conexão SQL quebrada à recusa normal: `TipoSemFaixa` é `ValueError` levantado pelo mapa. Continuidade com ORM e verificação de `TransactionManagementError` não foram executadas [nao verificado] (`app/ponto/precedencia.py:96`, `app/ponto/precedencia.py:108`).

   **Achado — fila, campo com dois significados.** `tipos_ativos_sem_faixa` conta colaboradores recusados no cartório e códigos ativos no vigia; o cartório sequer filtra `ativo`. Contraexemplos: um código em três colaboradores pode produzir **3/1**; um código inativo com ausência pode produzir **1/0** (`app/ponto/management/commands/processar_cartorio.py:73`, `app/ponto/services/cartorio.py:1952`, `app/ponto/services/vigia_ausencia.py:160`, `app/ponto/services/vigia_ausencia.py:168`, `app/ponto/precedencia.py:419`).

   Ambos chegam à mesma função real `faixa_de`: diretamente no vigia e pela grade/A2 no cartório. Portanto, são universos diferentes, sem evidência de segundo juiz de faixa (`app/ponto/services/vigia_ausencia.py:162`, `app/escala/utils.py:1209`, `app/ponto/precedencia.py:419`). O cartório publica seu contador no stdout; API/revisão semanal leem o contador do vigia (`app/ponto/management/commands/processar_cartorio.py:80`, `app/api/views_mensageria.py:1422`, `app/ponto/services/vigia_ausencia.py:259`).

3. **Espelho/PDF**

   A A2 recebe `recusas=`, exclui o dia recusado dos fatos e não ordena sua precedência; o compositor publica o rótulo pronto sem chamar `do_dia` para ele. O PDF reutiliza esse compositor (`app/ponto/precedencia.py:420`, `app/ponto/precedencia.py:425`, `app/ponto/services/espelho.py:339`, `app/relatorios/pdf_espelho.py:597`).

   **Achado — bloqueia, recusa tratada no miolo e relançada na tela.** Após obter o espelho, a view chama `montar_semanas`; este consulta `status_dos_dias_lavrados` sem fornecer fatos/recusas. Quando chega a um dia sem batida e sem status lavrado, consulta novamente a A2, agora sem `recusas=` (`app/ponto/views.py:158`, `app/ponto/views.py:184`, `app/ponto/services/espelho.py:170`, `app/ponto/status_dia.py:287`, `app/ponto/status_dia.py:296`, `app/ponto/status_dia.py:304`).

   **Contraexemplo:** ausência aprovada `fantasma_xyz` em dia passado sem batida e sem lavra. A primeira passagem produz “Tipo sem faixa: fantasma_xyz”; `montar_semanas` levanta novamente antes do `render`. Reproduzi essa sequência em memória com A2, D4 e `montar_semanas` reais; a execução HTTP completa permanece [nao verificado] (`app/ponto/precedencia.py:421`, `app/ponto/views.py:193`).

   **Achado — fila, paridade do PDF para dias válidos não demonstrada.** A troca de órfãs do builder por `orfas_grade` troca efetivamente a fonte: a tela normalmente lê a ata. Sem célula, esse leitor entrega órfãs vazias; o builder conserva batidas não consumidas como órfãs (`app/relatorios/pdf_espelho.py:477`, `app/ponto/services/espelho.py:522`, `app/escala/services/leitor_celula.py:394`, `app/escala/services/leitor_celula.py:420`, `app/escala/utils.py:1849`).

   **Contraexemplo de código:** builder com órfã `07:02`, ata/tela sem ela, período aberto iniciado às `07:02`. A substituição altera sua inclusão em `turnos_abertos`, depois `datas_turno_aberto`, podendo alterar dias em aberto e palavra. Nenhum tipo sem faixa é necessário; ocorrência em produção e magnitude são [nao verificado] (`app/relatorios/pdf_espelho.py:178`, `app/relatorios/pdf_espelho.py:515`, `app/relatorios/pdf_espelho.py:576`, `app/relatorios/cartao_pela_celula.py:418`, `app/relatorios/pdf_espelho.py:597`).

   Órfãs e palavra passam pela autoridade da tela, mas o PDF ainda apura/soma seu resumo e calcula saldo antes de `folha_manda`; essas derivações já existiam no recorte da base (`app/relatorios/pdf_espelho.py:418`, `app/relatorios/pdf_espelho.py:504`, `app/relatorios/pdf_espelho.py:531`, `app/ponto/tests/lastro_c9_recortes.json:27`). Paridade integral de horas/saldo/totais com a base: [nao verificado].

   Sem achado específico na remoção de `_realizado`: era helper local sem chamada no recorte fornecido da base; sua remoção isolada não explica mudança de saída (`app/ponto/tests/lastro_c9_recortes.json:27`).

4. **Fechamento e competência**

   As três capturas alteradas relançam `TipoSemFaixa`. Se ela ocorrer, interrompe o restante da chamada — empresa inteira ou subconjunto escolhido —, sem representar uma tranca formal da competência (`app/ponto/services/fechamento.py:555`, `app/ponto/services/fechamento.py:558`, `app/ponto/services/fechamento.py:1081`, `app/ponto/services/fechamento.py:1089`, `app/ponto/services/fechamento.py:1308`).

   Não há atomicidade de lote: A pode estar salvo, B pode ganhar linha `aberto` antes da falha, e C não ser processado. Isso é consequência de propagação, não uma decisão nova de pagar/abonar/zerar B (`app/ponto/services/fechamento.py:899`, `app/ponto/services/fechamento.py:1015`, `app/ponto/services/fechamento.py:1149`, `app/ponto/services/fechamento.py:1308`).

   Os chamadores relevantes têm estes destinos:

   - View e command de recalcular deixam a recusa subir (`app/ponto/views.py:1252`, `app/ponto/management/commands/recalcular_fechamento.py:118`).
   - Exportação chama leitura da empresa antes de classificar/gerar TXT; uma recusa interrompe esse fluxo (`app/folha/views.py:208`, `app/folha/views.py:212`).
   - `simular_folha` chama o fechamento antes do seu `try` posterior (`app/folha/management/commands/simular_folha.py:123`, `app/folha/management/commands/simular_folha.py:130`, `app/folha/management/commands/simular_folha.py:132`).
   - O callback por evento captura `Exception` e loga após o fato gravado: nesse chamador a recusa não chega ao usuário (`app/ponto/services/fechamento.py:1817`, `app/ponto/services/fechamento.py:1825`).
   - O cadastro declara ausência de cron ordinário de recalcular (`app/config/crons.py:1130`).

   Deixar levantar corresponde à instrução D do pedido; o isolamento por colaborador foi exigido especificamente para o cartório. A condição financeira de pouso, item 6, continua sem par medido no dado fornecido [nao verificado]. [nao verificado]

5. **Fonte única e defaults**

   Sem achado de default de **faixa** para tipo desconhecido: o mapa é consultado por indexação em `faixa_de`; A2/veto passam por essa função. O fallback `'trabalho'` da semana do posto relança expressamente `TipoSemFaixa` (`app/ponto/precedencia.py:108`, `app/ponto/precedencia.py:110`, `app/ponto/precedencia.py:419`, `app/ponto/precedencia.py:544`, `app/escala/services/semana_do_posto.py:178`).

   **Achado — fila, fallback de efeito preexistente.** `_do_banco` captura qualquer exceção e `vigente` substitui falha/resultado vazio pelo catálogo congelado; `efeito_vigente` lê esse resultado. Contraexemplo: `abono` alterado para descontar, seguido de falha na leitura, pode voltar a responder `ABONA`. Isso é fallback de efeito, não de faixa (`app/ponto/catalogo/ausencias.py:102`, `app/ponto/catalogo/ausencias.py:254`, `app/ponto/catalogo/ausencias.py:283`, `app/ponto/catalogo/ausencias.py:298`).

6. **Mordida dos testes** [nao verificado]

   Executei os três harnesses juntos: **43 testes, OK, RC 0**; seus mecanismos executam AST do fonte com alimentação substituída (`app/ponto/tests/test_lastro_c5_sem_banco.py:108`, `app/ponto/tests/test_lastro_c10_sem_banco.py:75`, `app/ponto/tests/test_lastro_c7_sem_banco.py:70`).

   Os mutantes executados ficaram vermelhos por asserção: retirar a guarda da porta, relançar no isolamento do cartório, restaurar o default `'atestado'` e retirar o rótulo. Portanto, há mordida de comportamento, além de inspeção textual (`app/ponto/tests/test_lastro_c10_sem_banco.py:31`, `app/ponto/tests/test_lastro_c10_sem_banco.py:36`, `app/ponto/tests/test_lastro_c10_sem_banco.py:43`, `app/ponto/tests/test_lastro_c10_sem_banco.py:280`, `app/ponto/tests/test_lastro_c5_sem_banco.py:50`).

   **Limite que bloqueia a conclusão de integração:** o teste B substitui `julgar_colab`; o teste PDF executa somente um prefixo com espelho mockado; o teste D injeta a exceção nas capturas, sem motor/ORM. Eles não certificam persistência, PDF completo nem o caminho posterior de `montar_semanas` que falha (`app/ponto/tests/test_lastro_c10_sem_banco.py:180`, `app/ponto/tests/test_lastro_c10_sem_banco.py:311`, `app/ponto/tests/test_lastro_c10_sem_banco.py:319`, `app/ponto/tests/test_lastro_c10_sem_banco.py:349`).

   O `test_05` cria `reciclagem` pela porta, aprova ausência e confere RD; os testes 07/08 são o par de criação/ativação recusadas. O código escolhido tem faixa e não está na semente 0052 (`app/relatorios/tests/test_rotulo_ausencia_pdf.py:52`, `app/relatorios/tests/test_rotulo_ausencia_pdf.py:64`, `app/relatorios/tests/test_rotulo_ausencia_pdf.py:73`, `app/ponto/precedencia.py:71`, `app/ponto/migrations/0052_seed_tipoausencia.py:10`). Execução ORM desses testes: [nao verificado].

   Os RED B/C também falharam com `--red-c9`, mas esse mecanismo substitui recortes congelados; não executa um checkout independente da c9 (`app/ponto/tests/test_lastro_c10_sem_banco.py:22`, `app/ponto/tests/test_lastro_c10_sem_banco.py:28`).

7. **Classes de reprovação**

   Os achados concretos são: **integração da recusa incompleta — bloqueia**; **contador com duas unidades, brecha interna da porta e fallback de efeito — fila**. Não encontrei novo juiz de faixa nem segundo escritor operacional de tipos; a consulta duplicada na tela usa a própria A2, mas perde o tratamento de recusa (`app/ponto/status_dia.py:304`, `app/ponto/management/commands/processar_cartorio.py:73`, `app/ponto/services/vigia_ausencia.py:168`, `app/ponto/services/ausencia.py:1314`, `app/ponto/catalogo/ausencias.py:283`).

   Sem achado de relógio novo no delta do PDF: `_hj` deriva do `agora` recebido. Os `except` que ainda absorvem falhas foram identificados no catálogo e no callback financeiro; as três capturas c10 do fechamento propagam a exceção (`app/relatorios/pdf_espelho.py:475`, `app/ponto/catalogo/ausencias.py:254`, `app/ponto/services/fechamento.py:1825`, `app/ponto/services/fechamento.py:1089`).

## 12/11 07:59 · sessao c2fbd7863c76 · - · - · origem=Ronald (tmux)

1. **Bloqueia — tela admin ainda pode levantar `TipoSemFaixa` antes de renderizar.** A view obtém o espelho com a recusa rotulada (`app/ponto/views.py:158`), mas depois chama `montar_semanas` (`app/ponto/views.py:184`). Esta chama `status_dos_dias_lavrados` sem fornecer fatos ou recusas (`app/ponto/services/espelho.py:170`). Ao alcançar dia sem batida e sem status lavrado, o leitor pergunta novamente à A2 pela janela inteira, sem `recusas=` (`app/ponto/status_dia.py:296`, `app/ponto/status_dia.py:304`). A A2 relança nessa modalidade (`app/ponto/precedencia.py:421`). Contraexemplo: ausência aprovada `legado_rd` em dia passado sem batida e célula sem `status_dia`; a palavra pode estar pronta no miolo, mas a view falha antes do `render` (`app/ponto/views.py:193`). O PDF não chama `montar_semanas`; chama somente o miolo (`app/relatorios/pdf_espelho.py:476`).

2. **Fila — paridade PDF sem tipo inválido não está provada; há mudança concreta de fonte.** Agora órfãs vêm da tela (`app/relatorios/pdf_espelho.py:477`), que normalmente lê a ATA (`app/ponto/services/espelho.py:522`, `app/escala/services/leitor_celula.py:420`). Antes vinham diretamente do builder, conforme o diff fornecido `[nao verificado: checkout independente da base]`. Essas listas não têm contrato de igualdade: sem célula, o leitor entrega dia sem órfãs (`app/escala/services/leitor_celula.py:390`, `app/escala/services/leitor_celula.py:394`); o builder conserva batidas não consumidas como órfãs (`app/escala/utils.py:1832`, `app/escala/utils.py:1849`).

   Contraexemplo condicional: builder classifica `07:02` como órfã e a ATA lida pela tela não; um período aberto iniciado às `07:02` passa a entrar em `_abertos_ts` (`app/relatorios/pdf_espelho.py:515`), muda `datas_turno_aberto` (`app/relatorios/pdf_espelho.py:576`) e pode mudar `dias_em_aberto`, seu badge e a palavra da linha (`app/relatorios/cartao_pela_celula.py:418`, `app/relatorios/pdf_espelho.py:750`, `app/relatorios/pdf_espelho.py:597`). Nenhum tipo sem faixa é necessário. Existência desse estado em produção e delta de horas/saldo/dinheiro: `[nao verificado]`.

3. **O selo de PDF não mede essa paridade.** Executa somente o prefixo até `_orfas_hhmm` (`app/ponto/tests/test_lastro_c10_sem_banco.py:302`, `app/ponto/tests/test_lastro_c10_sem_banco.py:319`), com `espelho_do_colab` substituído por `Mock` (`app/ponto/tests/test_lastro_c10_sem_banco.py:311`). Não executa os consumidores de órfãs, o resumo final nem a renderização; portanto o verde desse teste não certifica os efeitos descritos acima (`app/ponto/tests/test_lastro_c10_sem_banco.py:324`).

4. **Fonte única melhorou para órfãs e palavra, mas o PDF ainda calcula seu próprio resumo.** Lê os dias do espelho (`app/relatorios/pdf_espelho.py:561`), reutiliza a mesma porta de composição da palavra (`app/relatorios/pdf_espelho.py:597`) e apenas imprime `palavra_dia` (`app/relatorios/pdf_espelho.py:882`). Ainda executa `apurar_por_fatia` (`app/relatorios/pdf_espelho.py:418`), soma períodos (`app/relatorios/pdf_espelho.py:504`) e calcula saldo (`app/relatorios/pdf_espelho.py:531`) antes de `folha_manda` (`app/relatorios/pdf_espelho.py:587`). Esses caminhos já aparecem na base congelada (`app/ponto/tests/lastro_c9_recortes.json:27`); não são novos no delta.

5. **Sem achado na remoção de `_realizado`.** Era função local sem chamadas no recorte da base congelada (`app/ponto/tests/lastro_c9_recortes.json:27`); o PDF atual recebe minutos e pago pelo dict da tela (`app/ponto/services/espelho.py:792`, `app/ponto/services/espelho.py:820`). A remoção do helper, isoladamente, não muda saída; fidelidade do JSON ao commit real `[nao verificado]`.

6. **Sem relógio novo nesse delta.** `_hj` deriva do `agora` recebido e a chamada continua terminando em ontem (`app/relatorios/pdf_espelho.py:475`, `app/relatorios/pdf_espelho.py:476`). A mesma chamada foi apenas antecipada segundo o diff fornecido `[nao verificado: checkout independente da base]`.

## 12/11 07:59 · sessao 0214359824a2 · - · - · origem=Ronald (tmux)

2. O isolamento envolve **o colaborador inteiro**, na chamada única de `julgar_colab`; captura exclusivamente `TipoSemFaixa`, incrementa uma vez e registra pk/código antes de continuar (`app/ponto/management/commands/processar_cartorio.py:64`, `:70`, `:72`–`:80`).

No caminho normal desse erro, a grade inteira é montada antes de qualquer lavra (`app/ponto/services/cartorio.py:1168`); ela chama A2 (`app/escala/utils.py:1209`), que levanta em `faixa_de` (`app/ponto/precedencia.py:419`–`:422`). Portanto, nesse caminho, **nenhuma ata nova daquele colaborador é gravada**, e as anteriores permanecem; a primeira lavra/carimbo vem depois (`app/ponto/services/cartorio.py:1288`, `:1310`). Não existe rollback de colaborador: se a exceção ocorrer numa fase posterior, escritas anteriores podem permanecer; o `finally` limpa filas e contexto, mas não desfaz persistência (`app/ponto/services/cartorio.py:1093`–`:1118`).

Não há `atomic()` envolvendo o comando nem `julgar_colab`; existe atomicidade menor na porta que salva célula e evento (`app/ponto/portas/celula.py:899`–`:907`). `TipoSemFaixa` é `ValueError` puro (`app/ponto/precedencia.py:96`), levantado pela consulta ao mapa (`:105`–`:110`): esse caminho não evidencia SQL abortado ou `TransactionManagementError`. Ensaio ORM real da continuidade: **[nao verificado]**.

Os contadores são diferentes: cartório mede **colaboradores recusados na passada** (`app/ponto/services/cartorio.py:1952`–`:1954`; comando `:73`), vigia mede **códigos ativos sem faixa** (`app/ponto/services/vigia_ausencia.py:159`–`:168`). Ambos chegam à autoridade `faixa_de`: diretamente no vigia (`:162`), por grade/A2 no cartório (`app/escala/utils.py:1209`; `app/ponto/precedencia.py:419`). Leitores: resumo stdout do comando (`:75`–`:80`); API do vigia (`app/api/views_mensageria.py:1416`–`:1424`) e revisão semanal (`app/ponto/services/vigia_ausencia.py:259`–`:262`). Consumo automático do stdout em produção: **[nao verificado]**.

**Achado, fila — campo com dois significados:** `tipos_ativos_sem_faixa` designa unidades diferentes e o cartório nem exige que o cadastro esteja ativo. Contraexemplo: um código X usado por três colaboradores produz cartório=3 e vigia=1; X inativo, mas com ausência aprovada, pode produzir cartório=1 e vigia=0, porque a A2 consulta o código sem consultar `.ativo` (`app/ponto/management/commands/processar_cartorio.py:73`; `app/ponto/services/vigia_ausencia.py:160`; `app/ponto/precedencia.py:419`). Isso não introduz segundo juiz de faixa, mas cria ambiguidade semântica no nome compartilhado.

4. A recusa sobe pelas três capturas alteradas (`app/ponto/services/fechamento.py:1081`, `:1089`, `:1308`), interrompendo **todo o restante do universo daquela chamada**, que pode ser empresa inteira ou subconjunto (`:555`–`:558`, `:620`). Não é uma tranca de competência: são interrompidos o cálculo e o retorno, sem alteração específica de status por essa recusa (`:1308`–`:1318`).

**Persistência parcial concreta:** não existe atomicidade de lote. A pode ser salvo (`app/ponto/services/fechamento.py:1149`), B pode ganhar uma linha `status='aberto'` pelo `get_or_create` (`:899`–`:902`) e só depois falhar ao montar sua grade (`:1015`); C não será processado (`:1308`–`:1309`). Sem fotografia anterior para B, pode sobrar linha com defaults; com fotografia anterior, ela pode permanecer velha. Não houve regra de dinheiro nova para resolver B (`:1089`–`:1104`).

Chamadores relevantes:

- Botão de recalcular: chamada sem captura da recusa; deixa de retornar sucesso (`app/ponto/views.py:1252`–`:1253`). Resposta HTTP efetivamente observada: **[nao verificado]**.
- Gerar TXT: recalcula empresa inteira em leitura antes de classificar/aprovar; a recusa impede chegar à geração e ao carimbo (`app/folha/views.py:207`–`:212`, `:250`–`:252`).
- Comando `recalcular_fechamento --apply`: propaga antes de fotografar o depois (`app/ponto/management/commands/recalcular_fechamento.py:118`–`:122`).
- `simular_folha`, modos recalcular/lido: chamadas fora do `try` posterior; propaga (`app/folha/management/commands/simular_folha.py:121`–`:135`).
- Evento de batida/célula/HE: callback **captura qualquer Exception e loga**, preservando o fato já gravado; a recusa não sobe ao usuário por esse chamador (`app/ponto/services/fechamento.py:1814`–`:1828`).
- Regeneração em exportada: chama dentro de `transaction.atomic()`, portanto a recusa propagada desfaz o ato envolvente (`app/ponto/portas/regen_exportada.py:169`–`:180`).

Não encontrei cron ordinário de `recalcular_fechamento`; o cadastro de crons declara explicitamente que não há cron dessa operação (`app/config/crons.py:1130`–`:1137`). O comentário do callback dizendo que “o cron rega” não demonstra um agendamento real (`app/ponto/services/fechamento.py:1827`).

A propagação financeira corresponde à instrução D do pedido fornecido — deixar levantar quando seria necessário inventar regra. O item 4 do aval exige isolamento especificamente no **cartório**, não no fechamento; o item 6 exige par medido da competência 11. O dado fornecido não contém esse par: **[nao verificado]**.

## 12/11 07:59 · sessao 7bde65f716ea · - · - · origem=Ronald (tmux)

Itens 1 e 5, com achados separados de limites:

- **Fila — brecha na própria porta, anterior à c10.** `gravar_tipo_ausencia` valida o código atual e depois aplica qualquer chave de `campos` por `setattr`. Contraexemplo: `gravar_tipo_ausencia(tipo_com_codigo_abono, campos={'codigo': 'fantasma_xyz', 'ativo': True})` valida `abono` e salva `fantasma_xyz` ativo, sem faixa. A view atual não fornece `codigo` em `campos`, portanto não demonstrei exploração pelo HTTP. Evidências: `app/ponto/services/ausencia.py:1310`, `app/ponto/services/ausencia.py:1314`, `app/ponto/views_tipos_ausencia.py:67`.

- **Não encontrei segundo escritor runtime de `TipoAusencia` fora da porta.** O admin Django registra outros cinco modelos, sem `TipoAusencia`; a view de cadastro chama a porta nos dois ramos. A migration 0052 escreve diretamente código, efeito e ativo, mas seus 17 códigos estão todos no mapa. As migrations 0062 e 0063 alteram flags de tipos existentes, sem criar/ativar código novo. Evidências: `app/ponto/admin.py:11`, `app/ponto/admin.py:37`, `app/ponto/views_tipos_ausencia.py:87`, `app/ponto/views_tipos_ausencia.py:98`, `app/ponto/migrations/0052_seed_tipoausencia.py:10`, `app/ponto/migrations/0052_seed_tipoausencia.py:33`, `app/ponto/precedencia.py:52`, `app/ponto/migrations/0062_fim_dos_17_tipos.py:51`, `app/ponto/migrations/0063_atestado_aceita_horas.py:20`. Censo literal global não encontrou comando/importador/seed adicional que escreva esse modelo; caminhos dinâmicos externos à cópia: [nao verificado].

- **A recusa chega nomeada à tela.** A porta levanta `AusenciaInvalida(..., codigo='tipo_sem_faixa')`; a view lê somente `str(erro)`, não `.codigo`. `resposta_acao` envia `showToast` com tipo erro; o JavaScript escreve o texto na caixa `ta-erro` e no toast. Contraexemplo esperado: código `fantasma_xyz` exibe `Tipo sem faixa de precedencia: fantasma_xyz`. Evidências: `app/ponto/services/ausencia.py:1312`, `app/ponto/views_tipos_ausencia.py:89`, `app/ponto/views_tipos_ausencia.py:100`, `app/core/respostas.py:51`, `app/core/respostas.py:65`, `app/templates/ponto/tipos_ausencia.html:72`, `app/templates/ponto/tipos_ausencia.html:83`, `app/templates/ponto/tipos_ausencia.html:86`.

- **Leitores de `AusenciaInvalida.codigo`.** A API distingue `sobreposicao` → 409, demais códigos → 400; edição de ausência escolhe campo do formulário pelo código; um comando de rederivação registra o código. Esses consumidores não chamam `gravar_tipo_ausencia`; o novo valor `tipo_sem_faixa` tem assert específico no ensaio c10. Evidências: `app/api/views.py:1819`, `app/api/views.py:1823`, `app/ponto/views.py:2806`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:172`, `app/ponto/tests/test_lastro_c10_sem_banco.py:86`.

- **Fonte da faixa preservada.** `_FAIXA_DE_TIPO` só é acessado em `faixa_de`, que faz indexação e levanta `TipoSemFaixa` quando falta a chave. A2 e veto consultam essa função; nenhum default executável `'atestado'` substitui tipo desconhecido. `ROTULOS.get` é fallback de texto depois da classificação; `ICONE_PADRAO='atestado'` é desenho. Evidências: `app/ponto/precedencia.py:52`, `app/ponto/precedencia.py:108`, `app/ponto/precedencia.py:110`, `app/ponto/precedencia.py:419`, `app/ponto/precedencia.py:544`, `app/ponto/precedencia.py:565`, `app/ponto/catalogo/ausencias.py:119`, `app/ponto/catalogo/ausencias.py:181`.

- **Fila — fallback de efeito/cadastro sobrevive, anterior à c10.** `_do_banco` engole qualquer exceção e `vigente` troca resultado vazio/falha pelo `CATALOGO` congelado; `efeito_vigente` lê isso. Contraexemplo: `abono` configurado como `desconta`, seguido de falha de leitura do cadastro, pode voltar a responder `ABONA` do congelado. É fallback de efeito, não de faixa, e não foi introduzido pela c10. Evidências: `app/ponto/catalogo/ausencias.py:254`, `app/ponto/catalogo/ausencias.py:283`, `app/ponto/catalogo/ausencias.py:298`, `app/ponto/catalogo/ausencias.py:102`.

- **Outro fallback encontrado, sem engolir `TipoSemFaixa`.** Semana do posto usa `'trabalho'` quando não recebe faixa ou ocorre outra exceção, mas relança expressamente `TipoSemFaixa`. Portanto não demonstra sobrevivência do default para tipo sem faixa. Evidências: `app/escala/services/semana_do_posto.py:177`, `app/escala/services/semana_do_posto.py:178`, `app/escala/services/semana_do_posto.py:180`.

- **ESMERIL-SEMANTICA: mesmo nome, universos distintos.** O vigia conta códigos ativos de cadastro sem faixa; cartório conta colaboradores recusados na passada. Contraexemplo estático: um código ativo usado por três colaboradores pode resultar em vigia=1/cartório=3; código ativo sem lançamento pode resultar em vigia=1/cartório=0. Ambos consultam a mesma autoridade, portanto isso não prova segundo juiz. Evidências: `app/ponto/services/vigia_ausencia.py:155`, `app/ponto/services/vigia_ausencia.py:162`, `app/ponto/services/vigia_ausencia.py:168`, `app/ponto/services/cartorio.py:1952`, `app/ponto/management/commands/processar_cartorio.py:73`.

`TipoSemFaixa.codigo` significa identificador do tipo; `AusenciaInvalida.codigo` significa categoria da recusa. São classes distintas, e seus leitores encontrados respeitam esses sentidos. Evidências: `app/ponto/precedencia.py:101`, `app/ponto/services/ausencia.py:63`, `app/ponto/management/commands/processar_cartorio.py:74`, `app/api/views.py:1819`.

## 12/11 07:57 · sessao bbca41c9563d · BO-ROTULO-REABRIR-SO-NO-DETALHE · leitura · origem=Code

A diferença está confirmada no código: os dois detalhes incluem `Reabrir`; fio, modal e painel do turno omitem esse destino. Há também uma diferença no processamento: `detalhe.html` ignora o status enviado; `detalhe_local.html` chega ao motor. (`app/templates/chamados/detalhe.html:135`; `app/templates/chamados/detalhe_local.html:398`; `app/templates/chamados/fio_colaborador.html:268–270`; `app/templates/chamados/partials/modal_fio.html:668–670`; `app/templates/chamados/partials/painel_chamado_turno.html:76–78`; `app/chamados/views.py:292–310`; `app/chamados/services/acoes_chamado.py:313–321`.)

**1. Oferta por tela e responsável pela lista**

| Tela | Oferta de `novo_status=aberto` / “Reabrir” |
|---|---|
| `detalhe.html` | **Sim**, para `is_gestor`, quando `status_local != fechado`. Não filtra os outros estados. (`app/templates/chamados/detalhe.html:117`, `:127–135`.) |
| `detalhe_local.html` | **Sim**, para `is_gestor` com `pode_agir`, quando `status_local != fechado`. (`app/templates/chamados/detalhe_local.html:375–398`.) |
| `fio_colaborador.html` | **Não**. Oferece somente `em_analise`, `resolvido`, `fechado`; formulário condicionado a chamado não fechado, sem GLPI e com `pode_agir_map`. (`app/templates/chamados/fio_colaborador.html:254–270`.) |
| `modal_fio.html` | **Não**. Mesmos três destinos; exige chamado não fechado, sem GLPI, fora de `modo_beneficio` e com `pode_agir_map`. (`app/templates/chamados/partials/modal_fio.html:651–670`.) |
| `painel_chamado_turno.html` | **Não**. Mesmos três destinos, quando não fechado; a view exige gestor. (`app/templates/chamados/partials/painel_chamado_turno.html:65–78`; `app/chamados/views.py:347–350`.) |

Nos dois detalhes, portanto, a opção é oferecida em **`aberto`, `em_analise`, `resolvido`, `registrado`, `cancelado` e `superado`**: são todos os estados cadastrados, exceto `fechado`. (`app/chamados/models.py:28–35`; `app/templates/chamados/detalhe.html:117–135`; `app/templates/chamados/detalhe_local.html:375–398`.)

`is_gestor` significa `is_staff` ou pertencimento aos grupos `dp_rh`, `gestor`, `admin_ti`. No detalhe local, `pode_agir` consulta o departamento dono do chamado, incluindo as liberações para gestor geral e para usuários sem competências declaradas. (`app/chamados/views.py:24`, `:35–38`; `app/chamados/services/acoes_chamado.py:403–404`; `app/chamados/utils.py:32–72`.)

**Cada template enumera seus destinos.** O código compartilhado fornece a apresentação: `MOVER_PARA['aberto'] = 'Reabrir'`; `rotulo_de_movimento` retorna texto/title; `opcao_mover` gera a `<option>`. Essa tag recebe somente o destino, sem chamado, estado atual ou papel. (`app/chamados/services/verbos.py:132–146`; `app/chamados/templatetags/chamados_tags.py:67–78`.)

“Reabrir questionario”, existente no modal, é outra ação, enviada ao endpoint `reabrir_disputa_supervisao`; não é a opção `novo_status=aberto`. (`app/templates/chamados/partials/modal_fio.html:368–372`, `:665–670`.)

**2. Caminho da requisição e decisão do motor**

No **detalhe local**, para os seis estados acima, o caminho do formulário é:

1. POST para `chamados:detalhe_local` → rota `local/<chamado_id>/` → view `detalhe_chamado_local`. (`app/templates/chamados/detalhe_local.html:385`; `app/chamados/urls.py:14`; `app/chamados/views.py:359`.)
2. Guarda de departamento → `acao_resposta_livre`, que lê `novo_status` e chama `chamado.transicionar`. (`app/chamados/views.py:418–421`, `:547`; `app/chamados/services/acoes_chamado.py:287`, `:313–315`.)
3. `transicionar` obtém `get_perfil(user)` → `pode_transicionar` → `motor.decidir(estado, 'TRANSICAO_HUMANA', destino='aberto', perfil=perfil)`. (`app/chamados/models.py:119–134`.)

O motor **aceita `em_analise → aberto` para ambos os grupos de perfil**; aceita `resolvido → aberto` e `registrado → aberto` somente quando o perfil efetivo é `admin_ti`. As demais recusas estão discriminadas abaixo. (`app/chamados/catalogo/motor.py:101–105`, `:142–156`.)

Essa exceção usa o resultado de `get_perfil`, não `is_gestor`: superuser resulta em `admin_ti`; `is_staff`, sozinho, tem fallback `dp_rh`. (`app/chamados/models.py:130–132`; `app/colaboradores/mixins.py:154–178`.)

No **detalhe GLPI (`detalhe.html`)**, em qualquer dos seis estados, o formulário envia ao endpoint `followup`; a view lê apenas `conteudo`, chama `GLPIClient.adicionar_followup` e retorna o resultado da mensagem. **Não lê `novo_status` nem consulta o motor.** Em sucesso, responde “Mensagem enviada.”; em `GLPIError`, “Nao foi possivel enviar a mensagem. Tente novamente.” (`app/templates/chamados/detalhe.html:122`; `app/chamados/urls.py:12`; `app/chamados/views.py:292–310`; `app/chamados/glpi_client.py:219–230`.)

**As linhas do censo anterior continuam corretas:** `motor.py:102` exclui `aberto` dos destinos humanos de `resolvido`; `motor.py:153` aplica essa restrição para perfil diferente de `admin_ti`; a mensagem está em `:154–155`. (`app/chamados/catalogo/motor.py:102`, `:153–155`.)

**3. Pares em que a tela oferece e o motor recusa**

| Par (tela, status atual) | Perfil que recebe recusa | Mensagem |
|---|---|---|
| (`detalhe_local.html`, `aberto`) | Todos | `Chamado ja esta neste status.` (`app/chamados/catalogo/motor.py:148–149`.) |
| (`detalhe_local.html`, `resolvido`) | Diferente de `admin_ti` | `Transicao resolvido -> aberto nao permitida.` (`app/chamados/catalogo/motor.py:102`, `:153–155`.) |
| (`detalhe_local.html`, `registrado`) | Diferente de `admin_ti` | `Transicao registrado -> aberto nao permitida.` (`app/chamados/catalogo/motor.py:105`, `:153–155`.) |
| (`detalhe_local.html`, `cancelado`) | Todos | `cancelado e terminal absoluto - nao reabre (revival = re-emissao).` (`app/chamados/catalogo/motor.py:142–144`.) |
| (`detalhe_local.html`, `superado`) | Todos | `superado e terminal absoluto - nao reabre (revival = re-emissao).` (`app/chamados/catalogo/motor.py:142–144`.) |

Esses estados recebem a opção porque o template exclui apenas `fechado`; para gestor autorizado, inclui `aberto` sem consultar a matriz. (`app/templates/chamados/detalhe_local.html:375–398`.)

O motivo vira `ValueError`, é capturado sem tradução e segue como erro ao usuário. No formulário comum, há mensagem Django e redirect, e o detalhe renderiza a mensagem; em HTMX, o helper emite `showToast`. (`app/chamados/models.py:132–134`; `app/chamados/services/acoes_chamado.py:320–321`; `app/chamados/views.py:410–415`; `app/core/respostas.py:63–71`; `app/templates/chamados/detalhe_local.html:39–42`.)

`detalhe.html` não entra nessa lista de **recusas do motor**, pois seu endpoint não executa a transição. (`app/chamados/views.py:292–310`.)

**4. Origem da diferença entre telas**

A disponibilidade vem da **enumeração feita em cada template**; o mapa compartilhado centraliza rótulos, não decide quais destinos oferecer conforme estado/papel. Isso aparece diretamente nas cinco listas e na assinatura de `opcao_mover(status_local)`. (`app/templates/chamados/detalhe.html:132–135`; `app/templates/chamados/detalhe_local.html:395–398`; `app/templates/chamados/fio_colaborador.html:268–270`; `app/templates/chamados/partials/modal_fio.html:668–670`; `app/templates/chamados/partials/painel_chamado_turno.html:76–78`; `app/chamados/templatetags/chamados_tags.py:68–78`.)

A diferença também está explicitamente preservada no teste existente: `DESTINOS` inclui `aberto` apenas nos dois detalhes, e a asserção compara conjunto e ordem de cada seletor. (`app/chamados/tests/test_bo_rotulo_encerrado.py:71–79`, `:136–141`.)

## 12/11 07:56 · sessao 4d1d3242e9fe · - · - · origem=Ronald (tmux)

O caminho que chega ao motor é o de `detalhe_local.html`; o de `detalhe.html` envia apenas um followup ao GLPI, ignorando `novo_status`. [`app/templates/chamados/detalhe_local.html:385`, `app/chamados/views.py:547`, `app/chamados/services/acoes_chamado.py:315`; `app/templates/chamados/detalhe.html:122`, `app/chamados/views.py:292`, `app/chamados/views.py:304`]

- `detalhe_local.html` oferece `aberto`/“Reabrir” para `is_gestor`, com `pode_agir`, sempre que o status não é `fechado`; a lista é fixa e inclui a própria situação `aberto`, além de `em_analise`, `resolvido`, `registrado`, `cancelado` e `superado`. [`app/templates/chamados/detalhe_local.html:375`, `app/templates/chamados/detalhe_local.html:380`, `app/templates/chamados/detalhe_local.html:390`, `app/templates/chamados/detalhe_local.html:398`, `app/chamados/models.py:28`]
- `is_gestor` significa `is_staff` ou grupo `dp_rh`, `gestor`, `admin_ti`; `pode_agir` vem do controle por departamento dono do chamado. [`app/chamados/views.py:24`, `app/chamados/views.py:35`, `app/chamados/services/acoes_chamado.py:403`, `app/chamados/utils.py:68`, `app/chamados/utils.py:52`]

Caminho do clique local: `POST chamados:detalhe_local` → rota `local/<chamado_id>/` → `detalhe_chamado_local` → guarda de departamento → `acao_resposta_livre` → `ChamadoColaborador.transicionar` → `pode_transicionar` → `motor.decidir(..., 'TRANSICAO_HUMANA', destino='aberto', perfil=get_perfil(user))`. [`app/templates/chamados/detalhe_local.html:385`, `app/chamados/urls.py:14`, `app/chamados/views.py:359`, `app/chamados/views.py:420`, `app/chamados/views.py:547`, `app/chamados/services/acoes_chamado.py:313`, `app/chamados/models.py:128`, `app/chamados/models.py:119`]

| Status de origem no detalhe local | Motor com perfil diferente de `admin_ti` | Motor com perfil `admin_ti` |
|---|---|---|
| `aberto` | Recusa: `Chamado ja esta neste status.` | Mesma recusa. [`app/chamados/catalogo/motor.py:148`] |
| `em_analise` | Aceita → `aberto`. | Aceita → `aberto`. [`app/chamados/catalogo/motor.py:101`, `app/chamados/catalogo/motor.py:153`, `app/chamados/catalogo/motor.py:156`] |
| `resolvido` | Recusa: `Transicao resolvido -> aberto nao permitida.` | Aceita → `aberto`. [`app/chamados/catalogo/motor.py:102`, `app/chamados/catalogo/motor.py:153`] |
| `registrado` | Recusa: `Transicao registrado -> aberto nao permitida.` | Aceita → `aberto`. [`app/chamados/catalogo/motor.py:105`, `app/chamados/catalogo/motor.py:153`] |
| `cancelado` | Recusa: `cancelado e terminal absoluto - nao reabre (revival = re-emissao).` | Mesma recusa. [`app/chamados/catalogo/motor.py:142`] |
| `superado` | Recusa: `superado e terminal absoluto - nao reabre (revival = re-emissao).` | Mesma recusa. [`app/chamados/catalogo/motor.py:142`] |

As referências antigas continuam corretas: a linha **102** exclui `aberto` dos destinos humanos de `resolvido`; a linha **153** aplica a recusa, com exceção de `admin_ti`; a mensagem está nas linhas 154–155. [`app/chamados/catalogo/motor.py:102`, `app/chamados/catalogo/motor.py:153`]

Cada recusa vira `ValueError(motivo)`, é capturada sem tradução e entregue ao usuário com o mesmo texto: `resp('erro', str(_ve))` → `_resp` → `resposta_acao`; formulário comum recebe mensagem Django e redirect, HTMX recebe `showToast`. [`app/chamados/models.py:132`, `app/chamados/services/acoes_chamado.py:320`, `app/chamados/views.py:410`, `app/core/respostas.py:63`, `app/core/respostas.py:68`]

O perfil do motor é o resultado efetivo de `get_perfil`, não o booleano `is_gestor`: superuser resulta em `admin_ti`; depois entram competência de grupos, perfil do colaborador e fallback `is_staff → dp_rh`. [`app/chamados/models.py:130`, `app/colaboradores/mixins.py:154`]

No `detalhe.html`, o seletor também oferece `Reabrir` para gestor quando `status_local != fechado`, mas o caminho termina em `adicionar_followup`, sem consulta ao motor e sem alteração de status. A mensagem é `Mensagem enviada.` se o GLPI aceitar o followup, ou `Nao foi possivel enviar a mensagem. Tente novamente.` se ocorrer `GLPIError`. [`app/templates/chamados/detalhe.html:117`, `app/templates/chamados/detalhe.html:127`, `app/templates/chamados/detalhe.html:135`, `app/chamados/views.py:292`, `app/chamados/views.py:301`, `app/chamados/views.py:305`]

Há ainda uma diferença literal de identificador nesse caminho: o formulário passa `chamado.pk`, a rota chama esse valor de `ticket_id`, e a view o encaminha diretamente ao GLPI; o GET encontra o registro local por `ticket_id_glpi`. O efeito concreto para um registro depende dos valores dos dois identificadores. [`app/templates/chamados/detalhe.html:122`, `app/chamados/urls.py:12`, `app/chamados/views.py:304`, `app/chamados/views.py:252`; efeito em produção: **[não verificado]**]

A lista não é gerada pela matriz do motor: `MOVER_PARA['aberto']` define somente o rótulo `Reabrir`, `rotulo_de_movimento` retorna rótulo/title e `opcao_mover` produz a `<option>` sem receber chamado, status atual ou papel. Cada template escolhe explicitamente quais destinos incluir. [`app/chamados/services/verbos.py:132`, `app/chamados/services/verbos.py:140`, `app/chamados/templatetags/chamados_tags.py:68`]

## 12/11 07:56 · sessao e99ba5fece4f · - · - · origem=Ronald (tmux)

Oferta de `novo_status=aberto`:

| Tela | Condição e destino |
|---|---|
| `detalhe.html` | `status_local != fechado` e `is_gestor`; lista inclui `aberto`. `app/templates/chamados/detalhe.html:117`, `app/templates/chamados/detalhe.html:127`, `app/templates/chamados/detalhe.html:135` |
| `detalhe_local.html` | `status_local != fechado`, `is_gestor` e `pode_agir`; lista inclui `aberto`. `app/templates/chamados/detalhe_local.html:375`, `app/templates/chamados/detalhe_local.html:380`, `app/templates/chamados/detalhe_local.html:390`, `app/templates/chamados/detalhe_local.html:398` |
| `fio_colaborador.html` | Não inclui `aberto`; só `em_analise`, `resolvido`, `fechado`. Formulário exige chamado não fechado, sem GLPI e `pode_agir_map`. `app/templates/chamados/fio_colaborador.html:254`, `app/templates/chamados/fio_colaborador.html:255`, `app/templates/chamados/fio_colaborador.html:268` |
| `modal_fio.html` | Não inclui `aberto`; mesmos três destinos, sob chamado não fechado, sem GLPI, fora de `modo_beneficio` e `pode_agir_map`. `app/templates/chamados/partials/modal_fio.html:651`, `app/templates/chamados/partials/modal_fio.html:652`, `app/templates/chamados/partials/modal_fio.html:668` |
| `painel_chamado_turno.html` | Não inclui `aberto`; mesmos três destinos sob chamado não fechado. A view restringe acesso a gestor. `app/templates/chamados/partials/painel_chamado_turno.html:65`, `app/templates/chamados/partials/painel_chamado_turno.html:76`, `app/chamados/views.py:349` |

Os seis status oferecidos nos detalhes, por exclusão somente de `fechado`, são `aberto`, `em_analise`, `resolvido`, `cancelado`, `registrado`, `superado`; o universo de choices está em `app/chamados/models.py:28`.

`is_gestor` corresponde a `is_staff` ou grupo `dp_rh`, `gestor`, `admin_ti`. `app/chamados/views.py:24`, `app/chamados/views.py:35`.

`pode_agir` do detalhe local vem do contexto (`app/chamados/services/acoes_chamado.py:404`) e consulta departamento do chamado (`app/chamados/utils.py:68`), com gestor geral superuser/admin_ti (`app/chamados/utils.py:47`), compatibilidade por competências ou perfil e liberação legado sem competências (`app/chamados/utils.py:52`).

A lista de destinos é enumerada em cada template; o código comum fornece apenas rótulo/title e HTML da opção: `MOVER_PARA['aberto']='Reabrir'` em `app/chamados/services/verbos.py:136`; retorno do par em `app/chamados/services/verbos.py:146`; geração de `<option>` em `app/chamados/templatetags/chamados_tags.py:67`. A divergência das listas também é congelada no teste em `app/chamados/tests/test_bo_rotulo_encerrado.py:71` e conferida em `app/chamados/tests/test_bo_rotulo_encerrado.py:136`.

**Ressalva relevante para caminho:** `detalhe.html` posta em `chamados:followup` (`app/templates/chamados/detalhe.html:122`), cujo handler lê apenas `conteudo` (`app/chamados/views.py:292`), envia followup GLPI (`app/chamados/views.py:303`) e retorna “Mensagem enviada.” em sucesso (`app/chamados/views.py:301`, `app/chamados/views.py:310`); não lê `novo_status` nem chama motor no corpo da função (`app/chamados/views.py:273`).

O modal contém outras reaberturas: “Reabrir questionario”, para disputa existente e `pode_agir_map`, disputa aberta, em endpoint próprio (`app/templates/chamados/partials/modal_fio.html:123`, `app/templates/chamados/partials/modal_fio.html:326`, `app/templates/chamados/partials/modal_fio.html:368`); “Reabertura administrativa”, para disputa fechada, admin TI e menos de duas reaberturas (`app/templates/chamados/partials/modal_fio.html:376`, `app/templates/chamados/partials/modal_fio.html:382`, `app/templates/chamados/partials/modal_fio.html:383`, `app/templates/chamados/partials/modal_fio.html:390`). São formulários distintos do seletor `novo_status` (`app/templates/chamados/partials/modal_fio.html:665`).

## 12/11 07:53 · sessao 1ae2b1829e34 · - · - · origem=Ronald (tmux)

Censo em memória confirmou:

- Totais: pareadores 9, derivações 85; critérios A1=6/B1=82, conforme `core/tests/test_medida_turno_do_par.py:104`.
- Identidades/sítios, composição e CLASSIFICAR permanecem iguais ao registro, conferidos pelo censo de `core/medida_turno_do_par.py:448`.
- Apenas quatro entradas de linhas diferem:
  - `chamados/services/materializacao.py:171`: `[170] → [171]`.
  - `ponto/management/commands/detectar_intervalo_ausente.py:83`: `[82] → [83]`.
  - `ponto/services/triagem_batida.py:425`: `[424] → [425]`.
  - `ponto/services/triagem_batida.py:248`, `:255`, `:261`: `[247,254,260] → [248,255,261]`.

Os três arquivos têm import de `fecho_runtime` antes das ocorrências, respectivamente em `materializacao.py:43`, `detectar_intervalo_ausente.py:18`, `triagem_batida.py:6`. Retirar os imports de topo em `ponto` pode devolver suas ocorrências às linhas registradas; recalcule após a cura A16. O conteúdo histórico anterior dessas ocorrências, sem ler a base, é [nao verificado].

Execução usada: `PYTHONDONTWRITEBYTECODE=1 python3 -B -`, carregando `core/tests/test_medida_turno_do_par.py` em memória com a substituição documentada em `core/tests/test_medida_turno_do_par.py:20`, e comparando `m.censo(_app())` com os registros. Nenhum arquivo foi alterado.

## 12/11 07:53 · sessao 3b060935ddd6 · - · - · origem=Ronald (tmux)

(C) O selo não reconhece o encaminhamento por `ato=`: classifica `ato=ch.reconciliar` como “método preso a apelido” em [test_selo_reconciliar_le_o_retorno.py:57](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_selo_reconciliar_le_o_retorno.py:57).

A porta preserva o retorno: devolve `ato()` diretamente em [fecho_runtime.py:160](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/fecho_runtime.py:160), captura `resultado = ato()` em [fecho_runtime.py:165](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/fecho_runtime.py:165) e devolve esse valor em [fecho_runtime.py:169](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/fecho_runtime.py:169).

Os cinco sítios de `_julgar_colab_corpo` continuam lendo o booleano em filtros/condicional, antes de contar ou registrar fechamento: [cartorio.py:1538](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/ponto/services/cartorio.py:1538), [cartorio.py:1551](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/ponto/services/cartorio.py:1551), [cartorio.py:1572](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/ponto/services/cartorio.py:1572), [cartorio.py:1650](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/ponto/services/cartorio.py:1650), [cartorio.py:1665](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/ponto/services/cartorio.py:1665).

Correção recomendada do selo: reconhecer o atributo somente como argumento `ato=` da porta real, conferir igualdade entre receptor e primeiro argumento e verificar descarte sobre a chamada da porta. Preservar detecção de apelidos, `getattr` e portas desconhecidas. A allowlist existente deve continuar contendo apenas os dois descartes deliberados: [detectar_ausencias.py:159](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/ponto/management/commands/detectar_ausencias.py:159) e [materializacao.py:588](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/materializacao.py:588).

Mutantes recomendados: chamada da porta em statement; porta falsa; receptor diferente; remoção da leitura no chamador; `return resultado` substituído por valor constante na porta. O teste existente já exige a preservação exata de `False` em [test_e4b_c12_memoria.py:31](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c12_memoria.py:31). [nao verificado]

Nenhum arquivo foi escrito; execução dos testes e dos mutantes [não verificado]. [nao verificado]

## 12/11 07:14 · sessao 1db4d2d51bc8 · LASTRO-FAIXA-SEM-DEFAULT · construtor · origem=Code

Cura c10 escrita na cópia. A–D, funções alteradas, escritores, leitores e os dois contadores estão detalhados no [relato](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/LASTRO_C10.txt:5).

- **43 testes locais verdes**, incluindo P1–P5: [execuções](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_c10_execucoes.txt:12). [nao verificado]
- **RED/GREEN A–D e nove mutantes mortos por asserção**, com cada comando e saída integral; RED executado sobre recortes confirmados da c9: [provas](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/LASTRO_C10.txt:113).
- Teste 05 atualizado para criar `reciclagem` pela porta; pares de criar/ativar sem faixa escritos: [testes](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/relatorios/tests/test_rotulo_ausencia_pdf.py:52). [nao verificado]

**Sem commit**, conforme a proibição inicial. [Auditoria](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_c10_auditoria.txt:10). [nao verificado]

**Suite ORM e par `simular_folha` comp11: [não verificado]** — banco e Docker permaneceram proibidos. Não afirmo delta financeiro nem certificação; seguem para Code e opus. [Limites](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/LASTRO_C10.txt:254). [nao verificado]

## 12/11 03:03 · sessao 2fd88cf92d26 · AUDITORIA-DE-TRAVAS · leitura · origem=Code

| trava | a — disparos | b — defeito real confirmado | c — custo sem defeito de comportamento | d — suites inteiras / minutos | proposta | fonte |
|---|---|---|---|---|---|---|
| **1. Preverif Codex** | Total não medido; **15 voltas triadas**, 12 procedentes e 3 descartadas. | **≥2 episódios com cura**, detalhados abaixo; 12 procedentes não equivalem a 12 defeitos. | **3 consultas descartadas**; voltas adicionais causadas por elas: não medido. | Consultas triadas: **80,58 min**, sendo **13,52 min** descartadas. Suites atribuíveis: não medido. | **MUDA** a medição, conserva a revisão. Retirar perde achados comprovados; manter custa as consultas, inclusive descartadas. | `anexo:triagem.log:1–15`; `app/docs/RELATO.md:1446,322,327–328`; cruzamento C descrito no §1. |
| **2. Opus final** | Total de leituras não medido; **48 disparos B3** têm rótulo de reprovação final. | **≥3 episódios comprovados**: PAR-NAO-ADJACENTE, SELOS-CANON-I1 e E3. | **≥5 reprovações por forma/prova/documento**. | **5 suites posteriores associadas / 88,11 min**; incremento causal não medido. Duração das leituras finais: não medido. | **FICA**. Retirar perde descobertas posteriores ao preverif; manter conserva revisão e custos formais documentados. | `app/docs/RELATO.md:539,543,548–550,138,1275–1278,1415,1426`; D e S do §2. |
| **3. Disjuntor B3** | **50 disparos; 48 liberações**, sendo 22 desenho e 26 erro-bobo; **6 rearmes e 7 migrações**. | Ganho próprio do disjuntor: **não medido**; ele recebe resultados das revisões. | Os mesmos **≥5 episódios formais** do item anterior; não somar novamente. | Mesmas **5 suites / 88,11 min associados**; espera imposta pelo B3: não medida. | **MUDA** a escalada: distinguir defeito, prova/forma e desenho. Retirar perde a interrupção de ciclos; manter custa parada e liberação sem custo completo medido. | `anexo:disjuntor_14d.log`, linhas por evento; `bin/disjuntor.py:834–848,955–957,1004–1014`; §3. |
| **4. Hook Stop** | **1.006 registros BLOCK fila**, 2 BLOCK RELATO e 3 BLOCK ORCAMENTO; **237 liberações por teto**. | Não medido. | Não medido; teto não comprova falso positivo. | Suites e minutos atribuíveis: não medido. | **MUDA** a elegibilidade. Retirar perde contenção do encerramento; manter conserva cobrança incompatível com a lista e a série vigentes. | `anexo:hook_stop_por_dia.tsv:1–78`; `bin/hook_stop_fila1.py:238–252,270–287,1404,1439–1448`; §4. |
| **5. Hook PreToolUse** | É **`bin/disjuntor.py pretool`**. Invocações e negativas próprias: não medidas. | Não medido. | Não medido. | Suites/minutos: não medido. | **FICA**, com medição e conferência da instalação. Retirar perde as verificações B1/B3/B4/CENSO; manter tem custo por ferramenta e negativas não quantificados. | `.claude/settings.json:62–69`; `bin/disjuntor.py:1820–1872,1901–1907`; §5. |
| **6. ESPERANDO** | **1.839 liberações com prova + 590 registros sem prova**; não são esperas únicas. | Não medido. | **≥1 espera improdutiva documental**, de **100 min**, após morte do produtor. | **100 min documentais** nesse incidente; suites e total do período não medidos. | **MUDA** a reavaliação após o produtor terminar. Retirar perde liberação durante trabalho comprovado; manter conserva o risco de espera vencida. | `anexo:hook_stop_por_dia.tsv:1–78`; `app/docs/BACKLOG.md:1933`; `bin/hook_stop_fila1.py:789–838,1383–1390`. |
| **7. Uma vaga de construtor por repo** | Não medido: recusa por vaga ocupada ocorre antes do registro da chamada. | Colisões realmente evitadas: não medido. | Recusas desnecessárias: não medido. | Suites/minutos de serialização ou retomada: não medido. | **FICA**. Retirar perde exclusão entre construtores Codex; manter custa serialização não medida. | `bin/codex_pergunta.sh:235–260,291–293`; §7. |
| **8. Catraca de número de linha** | **≥4 disparos documentados como único vermelho de suite inteira**. | Defeitos de comportamento pegos por essa subtrava: não medido. | **≥4 episódios de deslocamento de linha**. | **4 falhas / 88,64 min**, seguidas de **4 repetições / 89,89 min**; **8 suites / 178,53 min observados**. | **MUDA** para identidade estrutural/conteúdo. Retirar posição absoluta perde aviso de deslocamento; manter conserva repetições documentadas. Economia efetiva de uma mudança: não medida. | `app/core/tests/test_medida_turno_do_par.py:539–543`; `app/docs/RELATO.md:46,547,553,560`; S do §8. |
| **9. Timeout do construtor** | **3 chamadas construtor rc=124**. | Defeito real evitado pelo encerramento: não medido. | Não medido; **2 entregas parciais preservadas** estão documentadas. | **45,27 min nas 3 chamadas**; suites e retrabalho adicional não medidos. | **MUDA** o teto mediante duração/progresso medidos. Retirar perde limite de ocupação; manter pode interromper trabalho aproveitável. | `bin/codex_pergunta.sh:83,401,419–433`; `app/docs/RELATO.md:296,519`; C do §9. |
| **10. Selo `padrao=`** | Total não medido; **2 commits só de comentário/anotação barrados** estão documentados. | Defeitos de comportamento pegos pelo campo: não medido. | **2 commits de comentário**, com **19 pares de árvores idênticas** após reescrita; outro episódio tem **3 rewords de instrumento**. | Suites/minutos atribuíveis: não medido. | **MUDA** a classificação por conteúdo, preservando a declaração. Retirar perde cobrança de termo/padrão; manter conserva reescrita formal sem mudança de árvore. | `app/docs/BACKLOG.md:1906`; `app/docs/RELATO.md:319,483`; `bin/tests/test_lei_akita.sh:122–123,307–325`. |
| **11. Suite, trava e vigias** | Negativas/esperas totais: não medidas. | Defeitos e colisões evitados no período: não medidos. | Não medido. | Construção TRAVA-SELO: **8 verificadores + 1 censo / 45,52 min**. Custo recorrente em suites/espera: não medido. Documento registra vigia e réguas atualmente em **0 suites/dia**, sem log anexo que confirme toda a janela. | **FICA** a exclusão; **MUDA** a observabilidade e a cobertura das entradas. Retirar perde serialização; manter conserva fila não medida e entradas fora da guarda de motivo. | `bin/trava_teste.sh:21–41`; `bin/suite.sh:173–175,221–226`; `app/docs/BACKLOG.md:2385,2390–2391`; C do §11. |
| **12. SUITE-SO-NO-FIM** | Execuções atribuíveis ao piloto: não medidas; foi substituído **06:28 → 06:29, “sem efeito”**. | Descobertas causadas pelo piloto: não medidas. | Suites evitáveis: não medidas. | E3: **7 inteiras / 161,44 min**; E4B-ALCANCE: **6 / 128,48 min**. **13 / 289,92 min observados**, sem atribuição causal ao piloto. | **MUDA** a avaliação: medir fases da conduta substituta. Mais inteiras custam repetição; adiá-las pode atrasar descoberta global, risco não quantificado. | `app/docs/PROMPTS.md:2086–2087`; S do §12; `bin/suite.sh:80–98,133–175`. |

Nas referências abaixo, **C[hora, obra]** identifica a linha exata de `anexo:chamadas_fim.tsv`; **S[caminho]**, a linha de `anexo:suites_14d.tsv` pelo campo `log`; **D[hora, evento/obra]**, a linha de `anexo:disjuntor_14d.log`; **G[hash]**, a linha de `anexo:gitlog_14d.txt`. São seletores de linha, sem inventar numeração. As propostas são insumo para decisão; custos compartilhados entre travas não devem ser somados novamente.

**1. Preverif Codex**

As triagens procedentes são as linhas **1–6, 8–9 e 11–14**; descartadas, **7, 10 e 15**. O escritor define uma volta como uma chamada, independentemente de quantos achados ela contém. Portanto, o anexo sustenta **12 voltas procedentes**, não uma contagem de defeitos. (`anexo:triagem.log:1–15`; `bin/disjuntor.py:870–872`.)

Há cura comprovada em **F1C4-3-EMISSORES**, volta 13, commit **`e548d8ca`**, após triagem procedente do preverif v12; e em **FIO-E0-V4-E4A-2**, preverif9, com reversão da mudança indevida do juiz e ajuste da prova nas voltas **`ea76f9f16` / `858bc0527`**. (`app/docs/RELATO.md:1446,322,327–328`; G[`ea76f9f16`], G[`858bc0527`].)

O cruzamento pela obra e pelo instante `fim` embutido na triagem soma **4.024 s procedentes + 811 s descartados = 4.835 s / 80,58 min**. As descartadas correspondem a C[10/11 02:26:14, REVERTER-DIA-SEM-RECALCULO], C[10/11 08:47:56, O169-FECHADA] e C[10/11 11:56:50, FIO-E0-V4-E4A-2]: **231 + 229 + 351 s**. Não há ligação suficiente para atribuir suites ou espera adicional a essas consultas. (`anexo:triagem.log:1–15`; respectivas linhas C.)

Para medir o total faltam classificação explícita de cada chamada como preverif, achado individual, triagem e commit da cura; `chamadas_fim.tsv` preserva papel e duração, e `triagem.log` preserva veredito por volta. (`bin/codex_pergunta.sh:253–260`; `bin/disjuntor.py:900–912`.)

**2. Opus final**

Três episódios comprovam utilidade posterior ao preverif:

- **PAR-NAO-ADJACENTE:** revisão final confirmou conflito entre pausa abaixo do piso e intervalo; cura **`d361dfee`**. (`app/docs/RELATO.md:548–550`.)
- **SELOS-CANON-I1:** comparação com célula crua deixava obra nascer sem censo; cura registrada na cópia **`ff47644a`**. (`app/docs/RELATO.md:539,543–544`.)
- **FIO-E0-V4-E3:** marcador sem horizonte alcançaria competências fora do escopo; cura **`2e0810cd0`**, pouso **`072556ec8`**. (`app/docs/RELATO.md:138`; G[`2e0810cd0`], G[`072556ec8`].)

Os **cinco episódios formais** são F1C4 v11, GRADE-FOLGA-JUIZ de 01/11, TOAST e duas reprovações MODO-COMUM. As suites posteriores associadas são:

| Linha S | segundos |
|---|---:|
| `f1c4emis/v13/suite.log` | 1.173,218 |
| `grade_juiz/vb/suite_60258b80.log` | 1.036,669 |
| `toast/suite_36768b29.log` | 1.165,072 |
| `mc/suite_2d78b6ec.log` | 961,538 |
| `mc/suite_a25e2149.log` | 950,240 |
| **Soma calculada das linhas acima** | **5.286,737 = 88,11 min** |

A classificação formal está em `app/docs/RELATO.md:1426,1415,1278,1275–1276`. A soma mede execuções associadas às curas; não demonstra economia contrafactual de remover a revisão.

Faltam início/fim, modelo efetivo e identificação das leituras finais para medir seu custo total. A configuração do agente usa **`model: inherit`**: o nome do papel sozinho não comprova execução em opus. (`.claude/agents/verificador.md:3–7`.)

**3. B3**

Os **50 disparos**, por data, são: 31/10 **6**; 01/11 **6**; 02/11 **5**; 04/11 **4**; 05/11 **7**; 06/11 **1**; 07/11 **3**; 08/11 **3**; 09/11 **2**; 10/11 **4**; 11/11 **7**; 12/11 **2**. A contagem considera exclusivamente linhas `DISJUNTOR B3`; liberações, rearmes e migrações são eventos separados. (`anexo:disjuntor_14d.log`, linhas com esses eventos e datas.)

Dois disparos explicitam **três voltas Codex procedentes**, ambos E4A-2: **10/11 02:22:34 e 11:49:24**. DATA-EVENTO-CRU apresenta inconsistência: o disparo diz revisão final, enquanto a liberação relata terceira volta Codex; origem consistente [nao verificado]. (D[10/11 02:22:34, B3 E4A-2], D[10/11 11:49:24, B3 E4A-2], D[11/11 17:31:19, B3 DATA-EVENTO-CRU], D[11/11 17:31:55, LIBERA DATA-EVENTO-CRU].)

**As 26 liberações erro-bobo não são 26 falsos positivos.** Há defeitos de implementação nesse grupo, inclusive H12H06 e OTIMIZA-3; a liberação por texto tampouco certifica cura. (D[05/11 20:48:58, LIBERA H12H06], D[05/11 00:47:21, LIBERA OTIMIZA-3]; `bin/disjuntor.py:1025–1031`.)

Para medir espera e ganho próprios faltam negativa individual, intervalo efetivamente ocioso, trabalho que continuou durante a parada e ligação à cura. O registro atual anuncia a transição e o motivo, sem esses campos. (`bin/disjuntor.py:193–201,1007–1014`.)

**4. Stop e os “937 andáveis”**

A causa está no código: `_itens_andaveis()` percorre **toda a tabela OBRAS**, excluindo itens pelo estado reconhecido por `_item_parado()`; `_andaveis_fora_do_em_curso()` somente remove o ID em curso; o recado imprime o tamanho restante. A lista fechada CONGELA e a ordem serial não participam desse cálculo. (`bin/hook_stop_fila1.py:95–97,166–180,238–252,270–287`; `app/docs/CORTES.md:446`; `app/docs/BACKLOG.md:1758`.)

Assim, um item pode continuar contado por estar aberto na tabela mesmo estando fora da vez pela lista fechada ou pela série. O ramo manda pegar o próximo da ordem, mas calcula a quantidade a partir da tabela ampla. (`bin/hook_stop_fila1.py:270–287,1391–1393`.)

**Quantas mensagens desse tipo houve: não medido.** Existem **1.006 registros da classe ampla** `BLOCK fila 1 com item aberto`; o agregado não preserva a subclasse nem o número anunciado. O episódio específico “937”, às 02:4x, não tem sua saída crua anexada [nao verificado]. (`anexo:hook_stop_por_dia.tsv:1–78`.)

Além disso, `BLOCK fila` é registrado antes de possível liberação por teto: **1.006 registros não equivalem a 1.006 decisões finais bloqueadas**. Faltam log/transcript cru, identificação da execução e decisão final para separar bloqueio útil, repetição e recado de elegibilidade incorreta. (`bin/hook_stop_fila1.py:1404,1439–1448`.)

**5. PreToolUse**

A instalação versionada aponta para **`bin/disjuntor.py pretool`**. Eventos `via=wrapper` no anexo não podem ser contabilizados como invocações desse hook: o wrapper também chama a porta CENSO diretamente. (`.claude/settings.json:62–69`; `bin/codex_pergunta.sh:229–234`.)

Há falha de instalação documentada em sessões iniciadas em `app/`, com B3 observado depois do pouso pelo Stop; isso é evidência documental, não censo completo das falhas do período. A instalação efetiva atual no host permanece [nao verificado]. (`app/docs/RELATO.md:7539,7676`.)

Para medir a–d faltam registros próprios de entrada, permissão/negação, regra responsável e início/fim. As negativas atuais registram o motivo, sem duração nem desfecho da obra. (`bin/disjuntor.py:1901–1907`.)

**6. ESPERANDO**

As **1.839 aceitações e 590 registros sem prova** não distinguem primeiras observações, tentativas repetidas e esperas únicas. Uma recusa pode decorrer de PID morto, campo inválido ou ausência de avanço observado; não comprova falso positivo. (`anexo:hook_stop_por_dia.tsv:1–78`; `bin/hook_stop_fila1.py:721–723,798–835`.)

O incidente documentado é preciso: produtor terminou, a linha deixou de ser válida e a esteira ficou **1h40** parada; ESPERANDO só era reavaliado no Stop. O log do incidente não foi anexado, portanto os **100 min** são medida documental, não reconstrução independente. (`app/docs/BACKLOG.md:1933`.)

Faltam identidade da espera, término do produtor, próxima reavaliação e retomada para medir o restante do período. A memória de mtime atual não constitui esse histórico. (`bin/hook_stop_fila1.py:681–723`.)

**7. Uma vaga por repositório**

O wrapper toma **`flock -n`** em `logs/construtor.vaga`, usando o repositório compartilhado pelas worktrees. A recusa por ocupação ocorre **antes** do trap e do registro `ini`; por isso, ausência em `chamadas_fim.tsv` não significa ausência de recusas. (`bin/codex_pergunta.sh:69–78,235–269,291–293`.)

A vaga cobre construtores Codex; sozinha não demonstra cumprimento da ordem serial entre todos os tipos de construtor. Para medir colisões evitadas e custo de ficar faltam tentativa, ocupante, recusa e retomada por obra. (`bin/codex_pergunta.sh:235–240`; `app/docs/BACKLOG.md:1758`.)

**8. Catraca de linha**

O selo pedido é **`test_CATRACA_linhas_reais_congeladas_exigem_registro_no_mesmo_commit`**. Ele compara literalmente `LINHAS_REGISTRADAS` com as posições retornadas pelo medidor; a identidade dos sítios já tem comparação separada imediatamente acima. (`app/core/tests/test_medida_turno_do_par.py:533–543`.)

| Obra; commit de registro | Suite falha → repetição verde | minutos calculados | [nao verificado]
|---|---|---:|
| E2; `93a48702` | S[`fio_e0_v4/e2_rebase/suite_v2.log`] → S[`fio_e0_v4/e2_rebase/suite_v3.log`] | 19,23 → 19,19 |
| PAR-NAO-ADJACENTE; `795cafdf` | S[`par_nao_adjacente/suite_v3.log`] → S[`par_nao_adjacente/suite_v4.log`] | 23,05 → 26,32 |
| SELOS-CANON-I1; `f0103bef` | S[`selos_canon/i1_suite_v4.log`] → S[`selos_canon/i1_suite_v5.log`] | 24,64 → 18,65 |
| UI-PAINEL; `2ca185e0e` | S[`UI-PAINEL/suite_v3.log`] → S[`UI-PAINEL/suite_v4.log`] | 21,73 → 25,74 |

Os relatos classificam esses vermelhos como deslocamentos de posição: texto preservado nos três primeiros e linhas movidas pelo encurtamento de função no UI-PAINEL. (`app/docs/RELATO.md:560,553,547,46`.)

As **quatro repetições / 89,89 min** são custo observado após esses bloqueios, não economia garantida: por exemplo, E2 também incorporou main antes da repetição. Misturas com defeitos reais em outras suites não entram nessa conta exclusiva. (`app/docs/RELATO.md:560,223,275,316`; linhas S acima.)

A proposta preserva o controle de sítios novos/alterados, retirando posição absoluta como motivo isolado de reprovação. Sua eficácia ainda é [nao verificado]; o próprio medidor declara não provar semântica, fluxo ou chamadas dinâmicas. (`app/core/medida_turno_do_par.py:18–20,32–38`.)

**9. Timeout de 900 s**

As linhas de construtor com `rc=124` são:

- C[09/11 07:47:50, FIO-E0-V4-E3]: **907 s**.
- C[10/11 00:30:33, PAUTA-SEM-TURNO-PREFIXO-RESERVADO]: **904 s**.
- C[10/11 17:12:03, BO-DISPUTA-SEM-PERGUNTA]: **905 s**.

A soma é **2.716 s / 45,27 min**. O default é configurável; construtor usa `timeout -k 30`, e a duração registrada inclui preparação/espera do wrapper. Portanto, duração acima de 900 s não demonstra sozinha falha do timeout. (`bin/codex_pergunta.sh:83,252,360–378,401`.)

E3 deixou código aproveitado no commit **`1394b61eb`**; BO-DISPUTA deixou arquivos tocados e entrega parcial. Isso comprova interrupção de trabalho aproveitável, sem comprovar que as chamadas estavam livres de defeitos. (`app/docs/RELATO.md:519,296`.)

Separadamente, as **8 recusas de limite**, somando **609 s / 10,15 min**, são `rc=75`, não timeout do construtor. (`anexo:limite.log:1–8`; C[05/11 15:05:08–15:05:46, respectivas obras]; `bin/codex_pergunta.sh:430–442`.)

**10. `padrao=` / PADRAO-NA-CURA**

O selo classifica negócio por caminho `app/`, ressalvando testes/docs; `n/a (instrumento)` não ganha análise que reconheça comentário apenas. A exceção de medida em Python é outra regra, limitada ao registro de módulos/dados. (`bin/tests/test_lei_akita.sh:122–137,316–321`.)

O caso pedido está registrado: **`11f82cfec` e `6c6689036`**, só comentário/anotação, viraram **`d75e5f2e1` e `ea1f0cab8`** alterando apenas `padrao=`; foram refeitos **17 descendentes**, com árvores iguais nos **19 pares**. A auditoria está aberta como **AUDITORIA-TRAVAS-POS-RESET-PADRAO-NA**. (`app/docs/BACKLOG.md:1906`; `app/docs/RELATO.md:319`.)

Outro episódio registra **3 rewords de instrumento**, também com árvores idênticas. Minutos e suites decorrentes não são medidos pelos anexos; faltam execução do portão, commit, instante, duração e retomada. (`app/docs/RELATO.md:483`; `bin/codex_pergunta.sh:253–260`.)

O campo verifica termo e admissibilidade; conferir se o diff realiza o padrão continua sendo leitura do verificador. (`bin/tests/test_lei_akita.sh:307–325`; `.claude/agents/verificador.md:8–15`.)

**11. Suite, trava, vigia e réguas** [nao verificado]

`bin/suite.sh` recusa inteira sem motivo válido antes do custo de montagem/docker; inteiras e recortes passam pela trava. A trava espera até **3.600 s**, retorna **75** se não obtiver a vez e mantém apenas ocupante atual; o histórico solicitado permanece pendente. (`bin/suite.sh:154,173–175,221–226`; `bin/trava_teste.sh:21–41`; `app/docs/BACKLOG.md:2391`.)

No anexo, TRAVA-SELO tem **8 chamadas verificador**, durações **432, 229, 225, 293, 266, 435, 238 e 229 s**, e **1 censo de 384 s**: **2.731 s / 45,52 min**, custo de construção do instrumento. O resumo documental “3 preverif, 1 opus” não deve substituir essa contagem observada. (C[11/11 06:40:31–08:02:16, TRAVA-SELO]; `app/docs/RELATO.md:141,154,157`.)

Para vigia/réguas, o documento registra **0 suites/dia atuais**, mas os logs citados não vieram anexados: o custo dos 14 dias permanece **não medido**. A implementação permite contar **1 inteira por chamada da régua relógio**, pela trava, e **3 por chamada da régua calendário**, fora dela. (`app/docs/BACKLOG.md:2390`; `bin/regua_relogio.sh:51–56`; `bin/regua_calendario.sh:41–46`.)

A unidade autorizada da guarda de motivo ficou restrita a **`suite.sh`**; régua e pre-push co

[truncado em 20000 de 24577 caracteres]

## 12/11 02:58 · sessao 8dc5a164915d · - · - · origem=Ronald (tmux)

Travas 1, 2, 3 e 9 — evidências para consolidação:

| Trava | Medição sustentada | Limitação / proposta |
|---|---|---|
| Preverif Codex | Anexo `triagem.log`: 15 linhas, 12 procedentes e 3 descartadas. Cruzamento com `chamadas_fim.tsv`: 80,58 min de consultas, sendo 67,07 min procedentes e 13,52 min descartadas. | Procedente conta uma **volta**, independentemente do número de achados; não prova sozinho defeito curado (`bin/disjuntor.py:870`, `bin/disjuntor.py:900`, `bin/disjuntor.py:911`). Proposta **MUDA**: conservar revisão e distinguir chamada, achado, cura e descarte; custo e economia completos não medidos. |
| Opus final | Pelo menos dois exemplos documentados de defeito confirmado após preverif: PAR-NAO-ADJACENTE, cura `d361dfee`; SELOS-CANON-I1, cura `ff47644a` na cópia, correspondente à volta 5 `f56dc9628` no gitlog fornecido (`app/docs/RELATO.md:548`, `app/docs/RELATO.md:550`, `app/docs/RELATO.md:539`, `app/docs/RELATO.md:543`). | Total de leituras e minutos não medidos: faltam registros de início/fim dos agentes finais. A definição usa `model: inherit`, portanto o nome do papel não prova modelo opus em toda chamada (`.claude/agents/verificador.md:5`). Proposta **FICA**: há falhas documentadas do preverif (`app/docs/RELATO.md:544`, `app/docs/RELATO.md:549`). |
| B3 | Contagem manual do anexo: 50 disparos, 48 liberações — 22 desenho e 26 erro-bobo —, 6 rearmes e 7 migrações. Dois disparos explicitam terceira volta Codex: E4A-2, 10/11 02:22:34 e 11:49:24. | B3 recebe reprovação ou terceira volta procedente; não diagnostica defeito (`bin/disjuntor.py:834`, `bin/disjuntor.py:955`, `bin/disjuntor.py:1007`). **ERRO-BOBO não significa sem defeito**. Proposta **MUDA**: separar confirmação de defeito, forma e pergunta de desenho; economia causal não medida. |
| Timeout construtor | Três linhas `construtor rc=124`: E3 09/11 07:47:50, 907 s; PAUTA 10/11 00:30:33, 904 s; BO-DISPUTA 10/11 17:12:03, 905 s. Soma 2.716 s = 45,27 min (`chamadas_fim.tsv`, linhas identificadas pelos horários). | Default 900 é configurável (`bin/codex_pergunta.sh:83`); execução construtor usa `timeout -k 30` (`bin/codex_pergunta.sh:401`). E3 teve entrega parcial preservada e commitada; BO-DISPUTA também teve arquivos tocados (`app/docs/RELATO.md:519`, `app/docs/RELATO.md:296`). Proposta **MUDA**: teto por trabalho medido; ganho de um novo teto não medido. |

Contagem manual dos 50 disparos B3 por data, para conferir: 31/10=6; 01/11=6; 02/11=5; 04/11=4; 05/11=7; 06/11=1; 07/11=3; 08/11=3; 09/11=2; 10/11=4; 11/11=7; 12/11=2 (`disjuntor_14d.log`, linhas `DISJUNTOR B3` nessas datas). O disparo DATA-EVENTO-CRU de 11/11 17:31:19 diz reprovação final, mas sua liberação às 17:31:55 relata terceira volta Codex; classificação consistente da origem fica [nao verificado].

Defeito real do preverif com cura explícita: F1C4-3-EMISSORES, triagem v12 procedente, volta 13 `e548d8ca`, com correção do teste nunca_bateu e documentação; isso demonstra pelo menos uma volta procedente com cura, sem transformar as 12 procedentes em 12 defeitos (`app/docs/RELATO.md:1446`). E4A-2 preverif9 confirmou criação indevida de perguntas por escritores em veredito não acusador; a errata exigiu reverter a mudança do juiz e ajustar a prova (`app/docs/RELATO.md:328`, `app/docs/RELATO.md:327`, `app/docs/RELATO.md:322`; commits `ea76f9f16` e `858bc0527` no anexo gitlog).

Há pelo menos cinco episódios documentados de reprovação por forma/prova/documento: F1C4 v11, GRADE-FOLGA-JUIZ, TOAST e duas voltas MODO-COMUM (`app/docs/RELATO.md:1426`, `app/docs/RELATO.md:1428`, `app/docs/RELATO.md:1278`, `app/docs/RELATO.md:1271`, `app/docs/RELATO.md:1276`). Cinco suites inteiras posteriores associadas somam **88,11 min**, sem comprovar que remover a trava evitaria essas execuções:

- `f1c4emis/v13/suite.log`: 1.173,218 s.
- `grade_juiz/vb/suite_60258b80.log`: 1.036,669 s.
- `toast/suite_36768b29.log`: 1.165,072 s.
- `mc/suite_2d78b6ec.log`: 961,538 s.
- `mc/suite_a25e2149.log`: 950,240 s.

Fonte dos cinco números: linhas com esses caminhos no anexo `suites_14d.tsv`. São custo associado, não incremento causal medido.

Escritores, leitores e cegueiras:

- **Chamadas**: escritor wrapper, linhas `ini` e `fim` ligadas por obra/hash/pid (`bin/codex_pergunta.sh:253`, `bin/codex_pergunta.sh:259`, `bin/codex_pergunta.sh:292`); leitor `codex_limite_estado._linhas_chamadas` (`bin/codex_limite_estado.py:49`), usado pelo gerador (`bin/gerar_codex_estado.py:41`). A duração começa antes de cópia, slots e trava da resposta, portanto inclui preparação/espera (`bin/codex_pergunta.sh:252`, `bin/codex_pergunta.sh:295`, `bin/codex_pergunta.sh:360`, `bin/codex_pergunta.sh:376`). Recusas anteriores ao `ini` ficam fora do anexo de encerradas (`bin/codex_pergunta.sh:234`, `bin/codex_pergunta.sh:240`, `bin/codex_pergunta.sh:258`).
- **Triagem**: escritor único `--triagem`, após conferir sidecar, papel e SHA (`bin/disjuntor.py:869`, `bin/disjuntor.py:892`, `bin/disjuntor.py:910`); leitor `_triagens`, consumido por `decide_b3` (`bin/disjuntor.py:921`, `bin/disjuntor.py:981`). Não registra classe, commit da cura nem custo de cada achado (`bin/disjuntor.py:911`).
- **Disjuntor**: escritor `trilha`; detalhe limitado a 300 caracteres e falha de escrita absorvida (`bin/disjuntor.py:193`, `bin/disjuntor.py:199`, `bin/disjuntor.py:200`). B3 anuncia após gravar estado (`bin/disjuntor.py:1043`); leitor do ESTADO agrega eventos e tipos de liberação (`bin/disjuntor.py:1971`, `bin/disjuntor.py:1979`, `bin/disjuntor.py:1999`). Liberação por texto válido não certifica cura (`bin/disjuntor.py:1025`, `bin/disjuntor.py:1031`).
- **Limite**: escritor wrapper (`bin/codex_pergunta.sh:434`, `bin/codex_pergunta.sh:439`); leitor conta linhas válidas (`bin/codex_limite_estado.py:27`, `bin/codex_limite_estado.py:41`). Oito recusas no anexo, 609 s = 10,15 min nas chamadas correspondentes; são limite do serviço, não timeout do construtor (`limite.log:1-8`; `chamadas_fim.tsv`, 05/11 15:05:08–15:05:46).
- **Gitlog anexo**: produtor da exportação e consumidor específico não identificados na cópia [nao verificado]. O código contém leitores de histórico como `tickets_placar.sh`, mas isso não identifica quem produziu este anexo (`bin/tickets_placar.sh:54`). Assunto truncado não prova conteúdo do patch [nao verificado].

A regra de preverif, descarte anexado, suite antes do final e retorno ao preverif após reprovação está em `CLAUDE.md:372-381`; a própria regra exige postmortem por achado e distingue falha do Codex, triagem, ausência de pergunta e falta de alcance (`CLAUDE.md:382-388`). [nao verificado]

## 12/11 02:57 · sessao 27d0677041ae · - · - · origem=Ronald (tmux)

**8 — Catraca de número de linha**

- O selo é `test_CATRACA_linhas_reais_congeladas_exigem_registro_no_mesmo_commit`: igualdade literal entre `LINHAS_REGISTRADAS` e listas `i['linhas']`; deslocamento sem alteração semântica reprova. [app/core/tests/test_medida_turno_do_par.py:539]  
- Há **pelo menos 4 suites cujo único vermelho documentado foi essa catraca**: E2 (`93a48702`, texto igual nas 149 entradas), PAR-NAO-ADJACENTE (`795cafdf`, texto idêntico), SELOS-CANON-I1 (`f0103bef`, texto idêntico) e UI-PAINEL (`2ca185e0e`, linhas movidas pelo encurtamento de `api_me`). [app/docs/RELATO.md:560; app/docs/RELATO.md:553; app/docs/RELATO.md:547; app/docs/RELATO.md:46]
- As quatro falhas somam **88,637 min** e as quatro repetições verdes **89,889 min**; **8 suites observadas / 178,526 min**, das quais **4 repetições / 89,889 min** são custo incremental documentado. Fontes anexas: `suites_14d.tsv`, E2 `e2_rebase/suite_v2,v3`; PAR `suite_v3,v4`; SELOS `i1_suite_v4,v5`; UI `suite_v3,v4`. Os números foram calculados das durações respectivas `1153.712,1151.186,1382.738,1579.099,1478.109,1118.817,1303.664,1544.245` s.
- Outros disparos misturados com defeitos reais não permitem cobrar a suite inteira exclusivamente da catraca. [app/docs/RELATO.md:223; app/docs/RELATO.md:275; app/docs/RELATO.md:316]
- **MUDA**, proposta: identidade estrutural do sítio + conteúdo normalizado no lugar da posição absoluta. Perderia aviso de mera mudança de posição; preservaria controle de sítios novos/alterados. Ganho potencial medido no recorte: evitar as quatro repetições acima; resultado da mudança é [nao verificado].
- Quem escreve o registro é a alteração do teste em cada commit; quem lê é o teste de igualdade. O medidor produz as ocorrências por AST, e o ESTADO lê o mesmo medidor. [app/core/tests/test_medida_turno_do_par.py:17; app/core/tests/test_medida_turno_do_par.py:539; bin/turno_do_par_estado.py:28; bin/gerar_estado.py:299]
- Cegueira declarada: AST lexical, sem prova semântica, fluxo, tipos ou chamadas dinâmicas; posições não dizem se houve defeito. [app/core/medida_turno_do_par.py:18; app/core/medida_turno_do_par.py:32]
- Não confundir os dois disparos de E4a1 `CLASSIFICAR` com a subtrava de linhas: são registro de candidatos informativos. [app/docs/RELATO.md:381; app/docs/RELATO.md:386; app/core/tests/test_medida_turno_do_par.py:545]

**10 — `padrao=`**

- Classifica negócio por **caminho**: `app/` exceto `/tests/` e `app/docs/`; para `n/a (instrumento)`, não examina se o diff é comentário. [bin/tests/test_lei_akita.sh:122; bin/tests/test_lei_akita.sh:316]
- Há **2 commits só de comentário/anotação** documentados como barrados: `11f82cfec` e `6c6689036`; foram reescritos para `d75e5f2e1` e `ea1f0cab8`, somente campo `padrao=`, com árvores idênticas. **19 pares** tiveram diff vazio, incluindo **17 descendentes** refeitos; são custo formal, não cura de comportamento. [app/docs/BACKLOG.md:1906; app/docs/RELATO.md:319]
- Exemplo adicional de custo formal: SELOS-CANON-I1, **3 commits** reword com árvore igual, porque tocavam `esteira_vigia.py` e o caminho os classificava negócio. [app/docs/RELATO.md:483] [nao verificado]
- Exemplo misto: PUSH-PRESO-14 traz **4 commits**, sendo **3 R1-PUBLICA** de medida e **1 merge PAUTA-08**; não classificar todos como falsos positivos. [app/docs/RELATO.md:854; app/docs/RELATO.md:851]
- Disparos completos e minutos/suites atribuíveis **não medidos**: faltam logs dos portões com instante/commit/rc e duração; `chamadas_fim` não mede reword ou espera de push. [nao verificado] [nao verificado]
- Escritor da saída `PADRAO_FALHA`, `PADRAO_JULGADOS` é o próprio selo; leitores são pre-push e laço de selos da régua. [bin/tests/test_lei_akita.sh:755; bin/pre-push.sh:38; bin/regua.sh:138]
- **MUDA**, proposta: discriminar comentário/anotação por conteúdo, preservando exigência nos commits de comportamento. Sair perde verificação de vocabulário/declaração; ficar conserva os custos documentados de reword. A semântica de “realiza o padrão” continua dependendo de leitura, porque o código verifica termo e caminhos. [bin/tests/test_lei_akita.sh:307; bin/tests/test_lei_akita.sh:324; app/docs/LEIS.md:144]

**11 — Suite, trava e vigias** [nao verificado]

- `suite.sh` recusa inteira sem motivo `pre-opus <OBRA>`/`pouso <OBRA>` antes de montagem/docker; recortes também passam pela trava. [bin/suite.sh:154; bin/suite.sh:173; bin/suite.sh:221]
- A trava usa flock, espera padrão **3600 s**, rc **75** quando não consegue; escreve somente ocupante atual `.quem`, sem histórico de duração/fila. [bin/trava_teste.sh:21; bin/trava_teste.sh:35]
- O histórico `logs/trava_teste.hist` ainda é pendência, portanto custo de espera **não medido**. [app/docs/BACKLOG.md:2391]
- Unidade autorizada da TRAVA-SELO foi **somente `suite.sh`**; régua e pre-push continuam entradas próprias. [app/docs/BACKLOG.md:2385; bin/regua.sh:165; bin/pre-push.sh:142]
- **Vigia:** documentação registra **152 suites de 19/09–26/09**, depois pausado, e **0 suites/dia / 0 min hoje**; isso é registro documental fora da janela, não validação independente dos 14 dias. Fonte host citada não veio nos anexos. [app/docs/BACKLOG.md:2390; app/docs/RELATO.md:147]
- Vigia escreve logs por rodada, estado/falhas e histórico; lê ele mesmo para confirmação e transição; `placar_code.sh` lê estado/histórico e `esteira_vigia.py` lê falhas. [bin/vigia_arvore.sh:18; bin/vigia_arvore.sh:31; bin/vigia_arvore.sh:76; bin/vigia_arvore.sh:148; bin/vigia_arvore.sh:156; bin/placar_code.sh:526; bin/placar_code.sh:534; app/core/esteira_vigia.py:1196]
- **Régua relógio:** uma inteira por chamada, pela trava. **Régua calendário:** três inteiras por chamada, **fora da trava**, com banco próprio. [bin/regua_relogio.sh:51; bin/regua_calendario.sh:41]
- Documento registra **0 chamadores atuais** e último artefato em 19/10; sem logs dos 14 dias, custo do período **não medido**. [app/docs/BACKLOG.md:2390]
- TRAVA-SELO custou documentalmente **11 voltas**, embora a linha resuma “3 preverif Codex, 1 opus” e outras linhas enumerem preverif v6–v9 e reprova/aprova. Não promover esse resumo contraditório a total auditado. [app/docs/RELATO.md:141; app/docs/RELATO.md:154; app/docs/RELATO.md:157; app/docs/RELATO.md:173]
- **FICA** flock; **MUDA** observabilidade (pedido/concessão/fim por run) e tratamento da régua calendário. Sair perde serialização; ficar custa fila sem medição e guarda apenas do chamador obediente. [bin/trava_teste.sh:35; bin/suite.sh:8; bin/regua_calendario.sh:43]

**12 — SUITE-SO-NO-FIM** [nao verificado]

- O piloto foi registrado às **06:28** e **substituído “sem efeito” às 06:29** por TRAVA-MENOS-FILA-COM-SELO. Portanto não existe ensaio causal demonstrado do piloto. [app/docs/PROMPTS.md:2086; app/docs/PROMPTS.md:2087]
- Anexo: E3 em 11/11 contém **7 inteiras**, **4 FAILED / 3 OK**, **161,441 min**; E4B-ALCANCE contém **6 inteiras**, **4 FAILED / 2 OK**, **128,479 min**. Total **13 inteiras / 289,919 min**, custo observado das obras, **não custo causal do piloto**. Fontes `suites_14d.tsv`: E3 `suite_c8b,int,int2,int3,int4,pre_opus,pre_opus2`; E4B `suite1,2,3,4,c9,c10`.
- Falhas “só-na-final”, suites evitadas e espera economizada **não medidos**: faltam fase da volta e resultados dos recortes anteriores associados ao mesmo hash; o agregado não os fornece. [nao verificado]
- Há defeito real E3 identificado no opus mesmo após suite verde: marcador sem horizonte; cura `2e0810cd0` no gitlog anexado, pouso `072556ec8`. Não atribuir essa descoberta ao piloto. [app/docs/RELATO.md:138; `gitlog_14d.txt`, linhas de `2e0810cd0` e `072556ec8`]
- **MUDA**, proposta: manter a conduta final já implementada e medir fase/hash/testes novos revelados só na inteira antes de avaliar o tripwire. Sair amplia repetição de suites; ficar pode adiar descoberta de interação global, atualmente não quantificada. [bin/suite.sh:80; bin/suite.sh:133; app/docs/PROMPTS.md:2086]

## 12/11 02:57 · sessao b13ca50a7ce7 · - · - · origem=Ronald (tmux)

Consolidação das travas 4–7, somente por leitura.

| Trava | a) disparos | b) defeito real | c) sem defeito | d) custo | proposta e fonte |
|---|---|---|---|---|---|
| 4. Stop | **1.006 registros `BLOCK fila`**, 2 `BLOCK RELATO`, 3 `BLOCK ORCAMENTO`; 237 liberações por teto; 568 registros de trabalho em curso. Não são decisões finais distintas: `BLOCK fila` antecede a eventual liberação por teto. `hook_stop_por_dia.tsv:1–78`; `bin/hook_stop_fila1.py:1404,1427–1448`. | **Não medido**: agregado não vincula bloqueio a achado/obra/cura. Falta log cru com decisão e resultado da triagem. `hook_stop_por_dia.tsv:1–78`. | **Não medido**: os 237 alarmes evidenciam o teto, sem provar ausência de defeito; os 568 eventos em curso também não provam falso positivo. `bin/hook_stop_fila1.py:1420–1448`; anexo:1–78. | Suites/minutos atribuíveis: **não medido**. Faltam identidade da chamada/suite e intervalo de espera por bloqueio. `bin/hook_stop_fila1.py:52–54`. | **MUDA**, proposta: leitura da lista autorizada e da ordem vigente; manter perde-se custo não medido de cobrança indevida; sair perde a contenção de encerramento com fila aberta. Fundamento: código conta toda tabela, não lista fechada. `bin/hook_stop_fila1.py:178–180,238–252,270–287`; `app/docs/CORTES.md:446`; `app/docs/BACKLOG.md:1758`. |
| 5. PreToolUse | Hook é **`bin/disjuntor.py pretool`**, registrado em `.claude/settings.json:62–69`. Número de invocações/negações do hook: **não medido**; anexo não traz censo de chamadas do hook, e contém eventos `via=wrapper` distintos. `bin/disjuntor.py:1802–1809,1832–1834`. | **Não medido**: falta vínculo negativa→achado confirmado→commit. `bin/disjuntor.py:1901–1907`. | **Não medido**: falta triagem das negativas. `bin/disjuntor.py:1901–1907`. | **Não medido**: faltam durações do hook e espera decorrente de cada negativa. `bin/disjuntor.py:193–199`. | **FICA**, com medição da própria porta e conferência da instalação efetiva; sair perde B1/B3/B4/CENSO, ficar custa verificação por ferramenta e negativas cujo custo não foi medido. `bin/disjuntor.py:1820–1872`; instalação da cópia não prova instalação efetiva no host [nao verificado]. |
| 6. ESPERANDO | **1.839 liberações com prova + 590 registros sem prova = 2.429 avaliações declaradas**, sem garantia de chamadas/esperas únicas. `hook_stop_por_dia.tsv:1–78`; `bin/hook_stop_fila1.py:1386–1390`. | **Não medido**: prova de vida não é prova de defeito; falta resultado posterior por espera. `bin/hook_stop_fila1.py:789–838`. | **Não medido**: 590 “sem prova” não são automaticamente falsos positivos; podem representar processo morto, campo inválido, primeira observação ou arquivo parado. `bin/hook_stop_fila1.py:816–835,721–722`. | Suites/minutos evitados ou impostos: **não medido**. Faltam identificação/duração de cada espera e contrafactual sem trava. `bin/hook_stop_fila1.py:52–54`. | **FICA**, proposta: mantém liberação quando há produtor provado; retirar perde essa saída e pode retomar cobranças de fila durante espera; ficar mantém custo de prova/recusas não medido. `bin/hook_stop_fila1.py:1383–1390`. |
| 7. Uma vaga Codex por repo | **Não medido**: recusa por vaga ocupada ocorre antes da instalação do trap e do `ini`, ficando fora de `chamadas_fim.tsv`. `bin/codex_pergunta.sh:239–240,252–269,291–293`. | **Não medido**: falta trilha das recusas e confirmação de colisão evitada. `bin/codex_pergunta.sh:183,240`. | **Não medido**: falta tentativa recusada, motivo e tarefa que poderia prosseguir. `bin/codex_pergunta.sh:183,240`. | **Não medido**: `flock -n` recusa imediatamente; tempo perdido na retomada não é registrado. `bin/codex_pergunta.sh:240`. | **FICA**, proposta: registrar tentativa/recusa antes da admissão; sair perde exclusão entre construtores Codex no mesmo repositório; ficar custa serialização/retomada não medidas. A vaga Codex não implementa por si só “uma vaga, nada em paralelo” entre todos os tipos de construtor. `bin/codex_pergunta.sh:184–185,235–240`; `app/docs/BACKLOG.md:1758`. |

**Achado específico dos “937 andáveis”.** A causa está comprovada: `_item_parado()` exclui fechado e formas da regex da coluna de estado; a regex conhece congelado, fila 2, aguardando, PAREI, não construir e outras formas, mas não a lista fechada CONGELA/POS-20. `_itens_andaveis()` varre todas as linhas do bloco OBRAS; `_andaveis_fora_do_em_curso()` apenas remove o ID do marcador; o recado imprime o tamanho dessa lista e manda pegar outro item. A descrição em prosa de ORDEM-POS-E4A2 e a lista CONGELA não participam desse cálculo. `bin/hook_stop_fila1.py:95–97,166–180,238–252,270–287,1391–1393`; `app/docs/BACKLOG.md:1758`; `app/docs/CORTES.md:446`.

**Frequência desse recado: não medida.** O agregado só preserva `BLOCK fila 1 com item aberto`, sem o conteúdo de `item` nem `reason`: há 1.006 registros dessa classe, limite superior para a subclasse, mas não seu total. O valor 937 e o horário específico dependem do relato do prompt [nao verificado]; falta o log cru ou transcript daquela saída. Mesmo o log cru do hook corta `item` em 60 caracteres, limitando a recuperação do número. `hook_stop_por_dia.tsv:1–78`; `bin/hook_stop_fila1.py:52–54,285–287,1404`.

**Escritores, leitores e cegueiras.**

- **`logs/hook_stop_fila1.log`:** escritor `_trilha()` do Stop; registra instante, decisão, motivo truncado, item truncado e HEAD. Leitor operacional: B2, chamado pelo Stop. Cego a utilidade, falso positivo, duração e commit de cura; `BLOCK` pode preceder `ALLOW` da mesma execução. `bin/hook_stop_fila1.py:43–56,1202,1404,1448`; `bin/disjuntor.py:1263–1284`.
- **`logs/hook_stop_espera.json`:** Stop lê observação anterior, escreve mtime por caminho/inode e substitui atomicamente; primeira observação não prova avanço. É memória de prova, não histórico de espera nem resultado. `bin/hook_stop_fila1.py:681–709,721–723`.
- **Linha `ESPERANDO`:** wrapper emite no stderr para construtor; o Stop lê a linha do painel e avalia PID/arquivo. Autor de cada transcrição manual para o painel e persistência concreta no host [nao verificado]. `bin/codex_pergunta.sh:381–385`; `bin/hook_stop_fila1.py:798–836`.
- **`logs/sessao/disjuntor.log`:** escritor `trilha()` do disjuntor, com detalhe truncado em 300 caracteres; negativas PreToolUse são registradas antes da resposta e da tentativa de anotação no RELATO. Leitor `ler()` alimenta o ESTADO; também há leitura para casar CENSO. Não mede todas as invocações permitidas nem duração das negativas. `bin/disjuntor.py:193–206,1802–1809,1901–1907,1971–2008,2090`.
- **`logs/codex/chamadas.log`:** wrapper escreve `ini` e `fim`; leitores incluem `codex_limite_estado.py` e `gerar_codex_estado.py`. Cego às recusas pré-admissão, inclusive vaga ocupada: elas ocorrem antes de `INI=1`. `bin/codex_pergunta.sh:239–240,253–260,269,291–293`; `bin/codex_limite_estado.py:22,50`; `bin/gerar_codex_estado.py:41,153`.
- **`logs/construtor.vaga`:** wrapper abre append e toma lock exclusivo sem espera; o caminho deriva do git-common-dir compartilhado pelas worktrees. É objeto de exclusão, sem histórico de ocupação/recusas no próprio arquivo. `bin/codex_pergunta.sh:69–78,235–240`.

**Falha de instalação documentada:** a cópia relata que o PreToolUse da raiz não disparou em sessão iniciada em `app/`, permitindo que B3 só fosse observado pelo Stop após pousos; são exemplos documentais, não contagem completa de falhas dos 14 dias. `app/docs/RELATO.md:7539,7676`.

## 12/11 01:02 · sessao 85364df2b7f9 · LASTRO-FAIXA-SEM-DEFAULT · verificador · origem=Code

**REPROVO a árvore c7 por dois bloqueios:** a grade pode contornar uma recusa de A2; e o status da folga parcial pode virar `ok` no app apesar do veredito `furo`. São contraexemplos de código; ocorrência desses estados em produção: **[não verificado]**. (`app/escala/utils.py:1208`; `app/ponto/precedencia.py:409`; `app/ponto/status_dia.py:157`; `app/ponto/status_dia.py:80`.)

1. **Escritores, leitores e efeito da condição nova**

   | Chave | Quem produz → quem coloca na ata |
   |---|---|
   | `tipo_dia` | Grade normal, overlay de folga e overlay de ausência/falta → `ata_do_dia`. (`app/escala/utils.py:1810`; `app/escala/utils.py:1816`; `app/escala/utils.py:1151`; `app/escala/utils.py:1220`; `app/escala/utils.py:1229`; `app/ponto/services/cartorio.py:729`.) |
   | `ausencia_tipo` | Overlay de ausência não falta → transporte para ata. (`app/escala/utils.py:1230`; `app/ponto/services/cartorio.py:766`.) |
   | `ausencia_id` | Overlay de falta e demais ausências → transporte para ata. (`app/escala/utils.py:1222`; `app/escala/utils.py:1232`; `app/ponto/services/cartorio.py:766`.) |
   | `cobertura_parcial` | `cobre_parte_do_dia` responde; grade carimba → ata transporta. (`app/ponto/turnos.py:2600`; `app/escala/utils.py:1190`; `app/escala/utils.py:1239`; `app/ponto/services/cartorio.py:757`.) |

   A persistência continua pela porta existente; o leitor da célula transporta as chaves da ata. Não encontrei segundo escritor nesse percurso. (`app/ponto/services/cartorio.py:1288`; `app/ponto/services/cartorio.py:1849`; `app/ponto/portas/celula.py:974`; `app/escala/services/leitor_celula.py:436`.)

   **Quem lê `dia_sem_trabalho`:** `_celula_furada`, premissa do chamado, fábrica, materialização, cluster espúrio, detector de intervalo e reconciliador; pelo adaptador do supra, também classificação, cartório e tarefa do dia. (`app/chamados/juizes.py:497`; `app/chamados/models.py:1284`; `app/chamados/services/fabrica_da_celula.py:112`; `app/chamados/services/fabrica_da_celula.py:201`; `app/chamados/services/materializacao.py:174`; `app/ponto/services/cluster_espurio.py:25`; `app/ponto/management/commands/detectar_intervalo_ausente.py:51`; `app/ponto/management/commands/reconciliar_grade.py:282`; `app/ponto/supra_juiz.py:71`; `app/ponto/supra_juiz.py:495`; `app/ponto/services/cartorio.py:1251`; `app/chamados/services/tarefa_do_dia.py:207`; `app/chamados/services/tarefa_do_dia.py:462`; `app/chamados/services/tarefa_do_dia.py:474`.)

   **A condição nova preserva a cobrança da folga humana parcial produzida pela grade:** ela recebe `ausencia_tipo/id` e `cobertura_parcial`, portanto entra na exceção parcial; o marco fora do período continua faltando. Não encontrei nessa cadeia produtor de `falta+parcial` nem de `folga+parcial` sem ambas as chaves — combinações fabricadas diretamente pelo teste antigo. (`app/escala/utils.py:1219`; `app/escala/utils.py:1230`; `app/escala/utils.py:1239`; `app/escala/utils.py:1253`; `app/chamados/juizes.py:455`; `app/chamados/tests/test_emis_ausencia_parcial_cobra.py:32`; `app/chamados/tests/test_emis_ausencia_parcial_cobra.py:51`.)

   **Dia real cujo veredito ou dinheiro mudou contra `29e201d0c`: [não verificado].** O comparador disponível congela `f218…`, e P1 altera explicitamente o cadastro sintético para permitir parcial. Os mapas atuais preservam previsto, realizado limitado ao previsto e abono restante; isso não certifica dinheiro real nem todas as rubricas contra aquela base. (`app/ponto/tests/test_lastro_c5_sem_banco.py:27`; `app/ponto/tests/test_lastro_c5_sem_banco.py:598`; `app/ponto/services/dia_pago.py:392`.)

   **BLOQUEIA — leitor do status não migrado.** Contraexemplo: a configuração de P1, folga parcial de 120 minutos, 08h–10h, previsto 480 e marco das 17h ainda faltando. O supra mantém a acusação, mas o status lê prioritariamente `tipo_dia='folga'`:

   | Estado do dia | Veredito | Status c7 | Mesmo registro com `tipo_dia='ausencia'` |
   |---|---|---|---|
   | Sem batidas | `furo` | `folga` | `irregular` |
   | Realizado 180 e órfã às 16h | `furo` | `folga_trabalhada` | `irregular` |

   A construção está em `app/ponto/tests/test_lastro_c5_sem_banco.py:598`; os resultados decorrem de `app/ponto/supra_juiz.py:366`, `app/ponto/supra_juiz.py:396`, `app/ponto/status_dia.py:148`, `app/ponto/status_dia.py:157` e `app/ponto/status_dia.py:159`.

   Isso alcança a lavratura e o app: **`folga_trabalhada → ok`, enquanto `irregular → alerta`**. Contraria o próprio contrato de mostrar a acusação do restante da parcial. (`app/ponto/services/cartorio.py:1853`; `app/ponto/portas/celula.py:963`; [app/ponto/status_dia.py:80](/home/ronald/codex_ws/09abf29115766f6f068b1ab0f09b0e6fd8f08824/app/ponto/status_dia.py:80); `app/ponto/status_dia.py:135`.)

2. **`_coberto_inteiro_nb`: derivação paralela, sem identidade literal com outro juiz**

   Os juízes registrados já respondem cobertura para folha/cobrança; a parcialidade vem da grade. `_coberto_inteiro_nb` compõe novamente presença de cobertura e integralidade usando rótulo/chaves e o carimbo, decidindo impressão e substituição por `nunca_bateu`. **Fila**, pela dependência semântica de `tipo_dia`; não demonstrei erro produzido por essa composição em um dia válido. (`app/core/juizes.py:190`; `app/core/juizes.py:191`; `app/ponto/services/cartorio.py:1221`; `app/ponto/services/cartorio.py:1231`; `app/ponto/services/cartorio.py:1239`; `app/ponto/services/cartorio.py:1255`; `app/ponto/services/cartorio.py:1285`.)

   Não é correto chamá-lo de cópia exata de `ausencia_explica_o_dia_inteiro`: **falta aprovada integral** dá verdadeiro no predicado NB e falso naquele juiz, que exclui falta explicitamente. Também não equivale a `dia_sem_trabalho`: folga sem ausência pode ser dia sem trabalho sem representar cobertura. São perguntas distintas; essa diferença, sozinha, não prova segundo juiz indevido. (`app/ponto/turnos.py:2677`; `app/ponto/services/cartorio.py:1239`; `app/chamados/juizes.py:459`.)

3. **Atalho de `_marcar_ausencias`**

   **Não achei divergência do `tipo_dia` traduzido entre as faixas conhecidas `!= folga`.** São `ferias`, `afastamento` e `atestado`, todas superiores a feriado/folga; outra ausência superior continua traduzindo para `ausencia`. Li mapa, ordem e tradução. (`app/ponto/precedencia.py:46`; `app/ponto/precedencia.py:52`; `app/escala/utils.py:1228`.)

   **BLOQUEIA — contorna a recusa de A2.** Contraexemplo concreto: duas ausências aprovadas no mesmo dia, mesma data, `fantasma_xyz` pk1 e `atestado` pk2. Cobertura escolhe atestado pelo maior pk; c7 valida apenas esse tipo e escreve `ausencia`. A2 percorre ambas e levanta `TipoSemFaixa`. A equivalência dos rótulos conhecidos não preserva a recusa. (`app/ponto/turnos.py:2753`; `app/ponto/turnos.py:2781`; [app/escala/utils.py:1208](/home/ronald/codex_ws/09abf29115766f6f068b1ab0f09b0e6fd8f08824/app/escala/utils.py:1208); `app/escala/utils.py:1209`; `app/ponto/precedencia.py:298`; `app/ponto/precedencia.py:409`; `app/ponto/precedencia.py:105`.)

   O vigia pode denunciar o cadastro desconhecido, mas seu contador não transforma o resultado da grade em recusa daquele dia. Existência desse par em produção: **[não verificado]**. (`app/ponto/services/vigia_ausencia.py:163`; `app/ponto/services/vigia_ausencia.py:168`.)

4. **Férias: `None`, avisos e silêncio**

   O único consumidor produtivo de `_dia_de_descanso` é `_vespera`, que efetivamente trata `None` como falso e omite a data. Porém as duas entradas produtivas passam a lista: o resultado completo conserva o aviso vermelho `tipo_sem_faixa`. Nesse percurso há **recusa nomeada da conclusão**, não fundamento para concluir “não é repouso”. (`app/ferias/situacao.py:96`; `app/ferias/situacao.py:115`; `app/ferias/situacao.py:251`; `app/ferias/situacao.py:325`.)

   Os avisos são retornados/indexados, publicados pela API e preservados na remarcação, que remove somente `colisao`. Não encontrei leitor produtivo usando `avisos=None`. (`app/ferias/situacao.py:297`; `app/ferias/situacao.py:304`; `app/ferias/situacao.py:360`; `app/api/views_mensageria.py:844`; `app/api/views_mensageria.py:846`; `app/ferias/views.py:496`; `app/ferias/views.py:522`.)

   **Fila — interface permite fallback silencioso:** `_vespera(colab, inicio)` sem lista, com `TipoSemFaixa` nos dois dias, devolve `[]` e perde os avisos. A assinatura permite isso; uso produtivo atual desse caminho: **[não verificado]**. (`app/ferias/situacao.py:72`; `app/ferias/situacao.py:97`; `app/ferias/situacao.py:107`.)

   **Fila — `except` que engole permanece:** `RuntimeError` na leitura vira `False` sem aviso, comportamento exigido pelo teste novo. A view também zera avisos diante de qualquer exceção. A captura genérica de `_dia_de_descanso` já existia no delta recebido. (`app/ferias/situacao.py:100`; `app/ponto/tests/test_lastro_c7_sem_banco.py:124`; `app/ferias/views.py:499`.)

5. **Versões e horizonte**

   **Ata comp11 efetivamente lavrada sob v5 em algum ambiente: [não verificado].** Os 21 casos do harness são sintetizados e partem de regra antiga **4**. (`app/ponto/tests/test_lastro_c7_sem_banco.py:149`; `app/ponto/tests/test_lastro_c7_sem_banco.py:157`.)

   **Fila condicional:** se houver impressão gravada com regra5, voltar à regra4 muda o hash e provoca rejulgamento sem outro fato novo. A versão entra na impressão; os dois ramos só pulam quando ela coincide. Portanto “sem bump conserva FOTO” exige distinguir atas v4 de eventuais atas v5. (`app/ponto/services/cartorio.py:490`; `app/ponto/services/cartorio.py:1254`; `app/ponto/services/cartorio.py:1256`; `app/ponto/services/cartorio.py:1397`; `app/ponto/services/cartorio.py:1405`.)

   `versao_para` é lido por `_regra_do_dia`, que alimenta esses dois hashes. `HORIZONTE_DO_FATO` continua literal em **21/09/2026**, no modelo e na migration; não é recalculado automaticamente pela tupla. (`app/ponto/services/cartorio.py:1185`; `app/chamados/models.py:2403`; `app/chamados/migrations/0053_pergunta_unica_por_familia.py:28`.)

   Leem esse horizonte: constraints do modelo, guarda/constraints da migration, recorte e grupos do fio, backfill e deduplicação. Ele delimita quais perguntas entram nessas operações e garantias de unicidade. (`app/chamados/models.py:3486`; `app/chamados/models.py:3492`; `app/chamados/migrations/0053_pergunta_unica_por_familia.py:33`; `app/chamados/migrations/0053_pergunta_unica_por_familia.py:57`; `app/chamados/services/fio.py:1412`; `app/chamados/services/fio.py:1947`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:277`; `app/chamados/management/commands/dedup_perguntas_fato.py:78`.)

   Remover v5 restaura a igualdade exigida pelo teste entre o literal e o início da competência do maior horizonte declarado; não altera fisicamente o índice por si só. (`app/ponto/services/cartorio.py:132`; `app/chamados/tests/test_e1_v6_horizonte.py:36`; `app/chamados/migrations/0053_pergunta_unica_por_familia.py:57`.)

6. **O que os testes mordem** [nao verificado]

   **Mordem implementações atuais extraídas por AST**, incluindo corpos originais das noturnas e B5u/S2S3. Não são somente texto congelado. Entretanto ORM, módulos externos, entrada da grade e persistência são substituídos; não certificam a integração cadastro → batidas → banco. (`app/ponto/tests/test_lastro_c5_sem_banco.py:39`; `app/ponto/tests/test_lastro_c5_sem_banco.py:111`; `app/ponto/tests/test_lastro_c5_sem_banco.py:214`; `app/ponto/tests/test_lastro_c5_sem_banco.py:576`; `app/ponto/tests/test_lastro_c5_sem_banco.py:583`; `app/ponto/tests/test_lastro_c7_sem_banco.py:79`; `app/ponto/tests/test_lastro_c7_sem_banco.py:178`.)

   **Sim, o JSON pode divergir e o GREEN continuar. Fila — procedência sem mordida:** o carregador confere a string de revisão e devolve `ast`; não verifica `sha256_fonte`. Contraexemplo: alterar somente esse SHA não muda nenhuma asserção desse carregador. A divergência normal entre uma base antiga e a árvore atual é esperada; a lacuna é certificar que o congelado corresponde à revisão afirmada. (`app/ponto/tests/lastro_c3_fontes.json:5`; `app/ponto/tests/test_lastro_c5_sem_banco.py:33`.)

   Os recortes c6 só substituem funções na variante RED; a instalação ocorre pelo `__main__`, não pelo GREEN normal de unittest. E P1 verifica juiz/supra da parcial, mas não seu status — por isso deixa passar o primeiro bloqueio. (`app/ponto/tests/test_lastro_c7_sem_banco.py:31`; `app/ponto/tests/test_lastro_c7_sem_banco.py:204`; `app/ponto/tests/test_lastro_c5_sem_banco.py:613`; `app/ponto/tests/test_lastro_c5_sem_banco.py:623`.)

7. **Consolidação pelas classes de reprovação**

   - **Derivação paralela de fonte única — bloqueia:** item 3, grade conclui onde A2 recusaria. (`app/escala/utils.py:1209`; `app/ponto/precedencia.py:409`.)
   - **Campo com dois significados/leitor não migrado — bloqueia:** item 1, `folga` da faixa passa a ser lida como dia inteiro sem jornada no status. (`app/ponto/status_dia.py:42`; `app/ponto/status_dia.py:148`.)
   - **Segundo juiz — não demonstrei identidade indevida no NB:** a comparação com o juiz de “explica inteiro” difere legitimamente na falta; fica a derivação em fila descrita no item 2. (`app/ponto/services/cartorio.py:1239`; `app/ponto/turnos.py:2677`.)
   - **Selo sem mordida — fila:** identidade do congelado; adicionalmente, P1 não cobre o status parcial. (`app/ponto/tests/test_lastro_c5_sem_banco.py:33`; `app/ponto/tests/test_lastro_c5_sem_banco.py:623`.)
   - **`except` que engole — fila:** retorno silencioso para erros diferentes de `TipoSemFaixa`. (`app/ferias/situacao.py:100`; `app/ferias/views.py:499`.)
   - **Segundo escritor fora da porta — sem achado no percurso examinado:** montagem da ata e lavratura continuam separadas pela porta existente. (`app/ponto/services/cartorio.py:721`; `app/ponto/portas/celula.py:974`.)
   - **Relógio fora da borda — sem achado no delta examinado:** `versao_para` usa a competência da data recebida; as mudanças de cobertura e captura examinadas não introduzem leitura de relógio. (`app/ponto/services/cartorio.py:249`; `app/escala/utils.py:1208`; `app/ferias/situacao.py:96`.)

## 12/11 01:00 · sessao 9b3ed64a1791 · - · - · origem=Ronald (tmux)

**Confirmado: bloqueia — leitor do status não migrou para a folga parcial.** O status ignora `cobertura_parcial` quando `tipo_dia='folga'`, contrariando seu próprio contrato de mostrar a acusação do resto e a invariante de concordância com o veredito. (`app/ponto/status_dia.py:18`, `app/ponto/status_dia.py:135`, `app/ponto/status_dia.py:141`, `app/ponto/status_dia.py:148`, `app/ponto/status_dia.py:157`.)

Reprodução em memória, usando corpos atuais e a configuração sintética de P1: folga aprovada de 120 minutos, 08h–10h; jornada prevista de 480 minutos; marcos 08h/17h; saída ainda `missing`. (`app/ponto/tests/test_lastro_c5_sem_banco.py:598`, `app/ponto/tests/test_lastro_c5_sem_banco.py:600`, `app/ponto/tests/test_lastro_c5_sem_banco.py:606`.)

| Batidas | Supra/veredito | Status atual | Mesmo registro rotulado `ausencia` |
|---|---|---|---|
| Nenhuma | `FURO_SEM_COBRANCA` → `furo` | `folga` | `irregular` |
| 180 minutos; órfã às 16h | `FURO_PARCIAL` → `furo` | `folga_trabalhada` | `irregular` |

Os resultados decorrem dos ramos de parcial do supra, do mapa de vereditos e dos ramos prioritários de folga no status. (`app/ponto/supra_juiz.py:366`, `app/ponto/supra_juiz.py:396`; `app/escala/models.py:1612`; `app/ponto/status_dia.py:148`, `app/ponto/status_dia.py:157`, `app/ponto/status_dia.py:159`.)

O efeito alcança a lavratura: o cartório transporta os fatos do status; a porta chama `status_da_lavra` e grava o resultado junto do veredito. (`app/ponto/services/cartorio.py:1853`; `app/ponto/portas/celula.py:951`, `app/ponto/portas/celula.py:963`, `app/ponto/portas/celula.py:1017`, `app/ponto/portas/celula.py:1038`.) No app, a divergência vira **`ok` em vez de `alerta`**; o rótulo também afirma “sem jornada prevista” apesar dos 480 previstos. (`app/ponto/status_dia.py:36`, `app/ponto/status_dia.py:42`, `app/ponto/status_dia.py:80`, `app/ponto/status_dia.py:81`.)

Classificação: **leitor não migrado / campo com dois significados**, não segundo escritor. (`app/ponto/status_dia.py:117`, `app/ponto/status_dia.py:148`; `app/ponto/portas/celula.py:976`.) P1 verifica grade, ata, juiz e supra, mas não o status parcial; seu selo permanece verde diante desse defeito. (`app/ponto/tests/test_lastro_c5_sem_banco.py:588`, `app/ponto/tests/test_lastro_c5_sem_banco.py:613`, `app/ponto/tests/test_lastro_c5_sem_banco.py:623`.)

Existência dessa configuração de cadastro em produção: **[não verificado]**. A fixture altera explicitamente a capacidade parcial do tipo. (`app/ponto/tests/test_lastro_c5_sem_banco.py:598`.)

## 12/11 00:58 · sessao f7cfc522e417 · - · - · origem=Ronald (tmux)

3. **Não encontrei divergência de `tipo_dia` para faixas conhecidas `!= folga`.** Todas são `ferias`, `afastamento` ou `atestado`, acima de feriado/folga; outra ausência superior também traduz `ausencia`. Li mapa, ordem e tradução. (`app/ponto/precedencia.py:46`, `app/ponto/precedencia.py:52`, `app/escala/utils.py:1228`)

**BLOQUEIA — bypass da recusa / derivação paralela.** Contraexemplo reproduzido em memória: duas ausências aprovadas no mesmo dia, `fantasma_xyz` pk1 e `atestado` pk2. Cobertura escolhe atestado pela recência; grade c7 valida somente atestado e escreve `ausencia`; A2 percorre ambas e levanta `TipoSemFaixa`. A otimização preserva o rótulo dos conhecidos, mas elimina uma recusa que A2 produziria. (`app/ponto/turnos.py:2753`, `app/ponto/turnos.py:2781`, `app/escala/utils.py:1208`, `app/escala/utils.py:1209`, `app/ponto/precedencia.py:298`, `app/ponto/precedencia.py:409`, `app/ponto/precedencia.py:105`)

A reprodução usou o harness e seus doubles, sem ORM; existência desse cenário em produção é [não verificado]. (`app/ponto/tests/test_lastro_c5_sem_banco.py:183`, `app/ponto/tests/test_lastro_c5_sem_banco.py:243`)

4. **O consumidor booleano realmente colapsa `None` com falso:** `_vespera` não inclui aquela data nos repousos. Porém ambos os leitores produtivos passam uma lista e recebem o aviso vermelho `tipo_sem_faixa`; o resultado completo preserva a recusa nomeada. Não encontrei consumidor produtivo que use `avisos=None`. (`app/ferias/situacao.py:96`, `app/ferias/situacao.py:115`, `app/ferias/situacao.py:166`, `app/ferias/situacao.py:251`, `app/ferias/situacao.py:320`, `app/ferias/situacao.py:325`)

Os avisos saem no retorno/indexação, são publicados pela API e preservados pela remarcação, que remove somente `colisao`. Portanto não há base para interpretar a ausência de aviso `vespera` como autorização; a API inclusive manda responder com observações, nunca “pode/não pode”. (`app/ferias/situacao.py:297`, `app/ferias/situacao.py:304`, `app/ferias/situacao.py:360`, `app/api/views_mensageria.py:844`, `app/api/views_mensageria.py:846`, `app/api/views_mensageria.py:847`, `app/ferias/views.py:496`, `app/ferias/views.py:522`)

**FILA — interface permite silêncio.** `_vespera(colab, inicio)` sem lista, diante de `TipoSemFaixa` nos dois dias, retorna `[]` e perde todos os avisos. Esse caminho é permitido pelas assinaturas, mas não identifiquei leitor produtivo atual que o utilize. (`app/ferias/situacao.py:72`, `app/ferias/situacao.py:97`, `app/ferias/situacao.py:107`, `app/ferias/situacao.py:115`)

**FILA — `except` que engole permanece:** qualquer outra exceção em `fatos_do_dia` vira `False` sem aviso; por exemplo, um `RuntimeError` de leitura. O teste novo exige esse comportamento. A remarcação também transforma qualquer exceção na obtenção dos avisos em lista vazia. Ocorrência real e introdução dessas capturas pela obra: [não verificado]. (`app/ferias/situacao.py:100`, `app/ponto/tests/test_lastro_c7_sem_banco.py:124`, `app/ferias/views.py:499`)

_4091 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
