# CODEX-ESTADO

> Ultima resposta processada: **10/11/2026 00:59** · publicado em **10/11/2026 00:59** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 10/11 00:59 · sessao df47402d072f · - · - · origem=Ronald (tmux)

- **Chamador externo:** na cópia, o chamador produtivo encontrado é `aplicar_col900 --reverter`: percorre os dias e chama a porta uma vez por dia; depois só imprime o resultado e retorna, sem segundo julgamento ou cálculo explícito (`app/ponto/management/commands/aplicar_col900.py:65`, `:77`, `:79`, `:85`). A deduplicação financeira é local a uma chamada do rabo: dicionário novo em `app/ponto/portas/celula.py:450`, preenchimento em `:456`, disparo em `:458`. Portanto N reversões bem-sucedidas na mesma competência podem produzir N recálculos; não existe deduplicação do lote nesse chamador (`app/ponto/management/commands/aplicar_col900.py:65`, `:77`).

- **Rejunção:** o callback não se limita aos dias revertidos: recebe o colaborador inteiro (`app/ponto/portas/celula.py:498`); seleciona chamados vivos, de escopo DIA, com FK `celula` nula (`app/chamados/rejuncao.py:59`); resolve a data pelo catálogo e busca a célula existente (`:83`, `:88`); escreve pela porta `apontar_celula` (`:107`). Essa porta altera `celula` e `atualizado_em`, trava a ordem antes do save e preserva apontamento existente (`app/chamados/services/vinculos.py:66`, `:96`, `:98`, `:100`). Leitor do vínculo: `dia_do_fato` passa a preferir `chamado.celula.data` (`app/chamados/juizes.py:98`).

- **Falta espúria e perguntas:** o outro trecho do callback recorta os dias tocados (`app/ponto/portas/celula.py:501`); o filtro admite célula/data_turno, mas a decisão consulta `dia_do_fato`, `fatos_do_dia` e `premissa_morta` (`app/chamados/services/feriado_reavaliar.py:76`, `:30`, `:33`, `:38`). O fechamento usa `reconciliar` (`:47`), cujo escritor de estado move `status_local`, `resolvido_em`, `via_encerramento`, `atualizado_em` e urgência (`app/chamados/models.py:228`, `:244`, `:246`, `:296`). Os receivers podem resolver perguntas e julgar novamente o dia (`app/chamados/signals.py:114`, `:216`). Perguntas recebem carimbo/via e, quando há resposta textual, os carimbos de resposta/validação pela porta `carimbar`; a relavra move `veredito_lavrado`, `julgado_em`, `janela_expira_em` (`app/chamados/services/disputa_emissao.py:1860`; `app/chamados/models.py:3128`, `:3130`, `:3149`). Leitores desses estados incluem `PerguntaDisputa.estado` (`app/chamados/models.py:3043`) e o coletor que seleciona perguntas não materializadas (`app/chamados/services/disputa_emissao.py:1820`).

- **Humano não é garantia universal de “zero chamadas ao juiz”:** o rabo pula apenas o julgamento direto pelo filtro `_HUMANOS` (`app/ponto/portas/celula.py:427`, `:431`). Se o callback fechar uma falta espúria, o signal pode chamar o mesmo `julgar_celula` sem esse filtro (`app/chamados/services/feriado_reavaliar.py:47`; `app/chamados/signals.py:216`). O juiz preserva o veredito administrativo, mas pode atualizar a ata (`app/ponto/portas/celula.py:1003`, `:1006`, `:1008`). Os novos testes sem chamado espúrio não exercitam essa cascata (`app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:26`, `:99`). Dia futuro continua barrado pelo próprio cartório (`app/ponto/services/cartorio.py:1985`).

- **Atomic externo:** o rabo está depois do atomic interno (`app/ponto/portas/celula.py:785`, `:811`), mas o juiz roda sincronicamente (`:434`), enquanto dinheiro e reavaliação são agendados por `on_commit` (`app/ponto/services/fechamento.py:1824`; `app/ponto/portas/celula.py:502`). A ordem de registro é juiz → dinheiro → callback do rabo; confirmação experimental da execução em transação externa: **[não verificado]**. Não se deve dizer “já commitada” apenas porque saiu do atomic interno: existe caminho do juiz por `reconciliar`, que abre `atomic(savepoint=False)` (`app/ponto/services/cartorio.py:1499`; `app/chamados/models.py:650`). Exceção nesse bloco, depois engolida pelo rabo (`app/ponto/portas/celula.py:437`), pode comprometer a transação externa; reprodução: **[não verificado]**. A gravação específica da ata/veredito tem atomic próprio (`app/ponto/portas/celula.py:904`).

- **Callbacks anteriores ao dinheiro:** o próprio julgamento pode fechar disputa (`app/chamados/services/disputa_emissao.py:1960`), cujo signal agenda retomada de regularizações (`app/chamados/signals.py:134`, `:144`); saves de pergunta também agendam fechamento de beco (`:227`, `:238`). Logo o dinheiro da VOLTA não é necessariamente o primeiro callback registrado (`app/ponto/portas/celula.py:434`, `:458`). Existência desses dados e recálculos indiretos no caso concreto: **[não verificado]**.

- **Exceções comuns:** julgamento direto e disparo financeiro têm `try/except` separados (`app/ponto/portas/celula.py:437`, `:459`); o callback protege rejunção e falta espúria separadamente com `engolir` (`:495`, `:499`), que captura `Exception` (`app/core/observ.py:88`). O serviço financeiro também protege execução e agendamento (`app/ponto/services/fechamento.py:1818`, `:1825`). Isso sustenta best effort para exceções previstas nessas fronteiras; não sustenta garantia absoluta contra rollback de atomic externo (`app/chamados/models.py:650`).

- **Rejunção duplicada existente:** a VOLTA já emite `EventoCelula` (`app/ponto/portas/celula.py:796`), cujo consumidor também chama rejunção (`app/chamados/reconciliador.py:1364`). O novo callback acrescenta outra tentativa (`app/ponto/portas/celula.py:498`); ambas passam pelo filtro de FK nula e pela guarda da porta (`app/chamados/rejuncao.py:62`; `app/chamados/services/vinculos.py:96`). É repetição de disparo do mesmo escritor, não um segundo juiz (`app/chamados/rejuncao.py:107`).

## 10/11 00:58 · sessao 68effa817189 · - · - · origem=Ronald (tmux)

Achados financeiros, todos da cópia:

- **Competência exportada é alcançada.** `apesar_da_lavra` apenas dispensa a guarda de restauração do DNA; não é passado para o financeiro. O rabo chama `recalcular_por_evento` com recálculo habilitado por padrão; o callback chama o escritor mensal sem trava geral por exportação. Há uma exceção concreta: **09/2026 exportada continua barrada pela `GUARDA_COMPETENCIA`**, que exclui esses colaboradores do universo antes das escritas. (`app/ponto/portas/celula.py:779`, `app/ponto/portas/celula.py:811`, `app/ponto/portas/celula.py:445`, `app/ponto/services/fechamento.py:1810`, `app/ponto/services/fechamento.py:36`, `app/ponto/services/fechamento.py:558`.)

- **O motivo não acompanha a trilha específica de exportada.** A VOLTA fornece `motivo_recalculo`; o rabo encaminha como `motivo`; mas `recalcular_por_evento` não repassa `motivo_exportada` ao escritor. O aviso da competência exportada pode registrar `'(sem motivo declarado)'`; o motivo aparece somente no `info` posterior do callback. (`app/ponto/portas/celula.py:812`, `app/ponto/portas/celula.py:458`, `app/ponto/services/fechamento.py:1810`, `app/ponto/services/fechamento.py:547`, `app/ponto/services/fechamento.py:1813`.)

- **N VOLTAs em lote produzem N pedidos de recálculo**, mesmo pertencendo à mesma competência: a deduplicação é um dicionário local dentro de cada execução do rabo; a VOLTA entrega sempre `{dia}`. O callback não implementa deduplicação entre chamadas. (`app/ponto/portas/celula.py:450`, `app/ponto/portas/celula.py:456`, `app/ponto/portas/celula.py:811`, `app/ponto/services/fechamento.py:1824`.)

- **Futuro e humano: disparo financeiro não prova que o previsto volta a 240.** O rabo pula o juiz nesses casos, mas solicita dinheiro. O leitor da grade prefere o `minutos_previstos` já lavrado na ata; só usa o juiz do previsto quando o campo é `None`. Portanto uma ata preservada com 480 pode produzir novamente `DiaPago.minutos_previstos=480`, apesar do DNA restaurado. O teste desses casos espia o disparo financeiro, sem executar essa conta. (`app/ponto/portas/celula.py:427`, `app/ponto/portas/celula.py:431`, `app/ponto/portas/celula.py:445`, `app/escala/services/leitor_celula.py:379`, `app/escala/services/leitor_celula.py:399`, `app/escala/services/leitor_celula.py:411`, `app/ponto/services/fechamento.py:1060`, `app/ponto/services/dia_pago.py:387`.)

- **O financeiro reescreve a competência inteira desse colaborador**, não apenas o dia revertido. O escritor recebe `colaborador_ids=[pk]`; produz o fechamento da janela; lavra as versões `motor` e, a partir de 10/2026, `oraculo`; cada `lavrar` apaga e recria as linhas daquela versão/competência, inclusive ajuste mensal. (`app/ponto/services/fechamento.py:1810`, `app/ponto/services/fechamento.py:621`, `app/ponto/services/fechamento.py:1201`, `app/ponto/services/fechamento.py:1265`, `app/ponto/services/fechamento.py:1277`, `app/ponto/services/dia_pago.py:348`, `app/ponto/services/dia_pago.py:359`.)

Escritores e leitores:

| Campos | Escritor efetivo | Leitores concretos |
|---|---|---|
| `FechamentoMensal`: horas trabalhadas/noturnas/extras e seus splits, folga, atraso, saída antecipada, falta, intra, turnos abertos, inconsistências, DSR, banco, minutos previstos/abonados/realizados, dias previstos/incertos, motivos/causa do espelho e carimbos | Aberto: `save`; demais status: `update` com a lista de campos. (`app/ponto/services/fechamento.py:1137`, `app/ponto/services/fechamento.py:1160`.) | GET do fechamento consulta o gravado; TXT lê os fechamentos e extrai eventos; percentual usa o previsto materializado quando há carimbo. (`app/ponto/views.py:1134`, `app/folha/export.py:711`, `app/folha/export.py:722`, `app/folha/export.py:1037`.) |
| `DiaPago`: 15 campos de dia e dois de ajuste, `furo_apurado`, detalhe, versão e carimbos | `dia_pago.lavrar`, alimentado pelo mesmo resultado do fechamento; `delete` + `bulk_create`. (`app/ponto/services/dia_pago.py:34`, `app/ponto/services/dia_pago.py:43`, `app/ponto/services/fechamento.py:1201`, `app/ponto/services/dia_pago.py:345`, `app/ponto/services/dia_pago.py:359`.) | Calendário lê linhas por data e `rubricas_do_dia`; testemunhas do export leem versão `motor`. (`app/colaboradores/services/calendario.py:290`, `app/colaboradores/services/calendario.py:655`, `app/ponto/services/dia_pago.py:855`, `app/folha/porta_export.py:75`, `app/folha/porta_export.py:284`.) |

Semântica:

- **Competência usa a data do fato**, sem substituir por hoje ou pelo mês civil: callback → `competencia_do_fato` → normalização local de datetime → `competencias_do_intervalo` → `janela_atual`, cujo corte vem da empresa. (`app/ponto/services/fechamento.py:1809`, `app/ponto/services/fechamento.py:1602`, `app/ponto/services/fechamento.py:1567`, `app/ponto/services/fechamento.py:1594`, `app/ponto/services/dia_pago.py:715`, `app/ponto/janelas.py:60`.)

- **Dia dos períodos usa o mapa do motor**, não necessariamente a data civil de cada entrada: o fechamento passa `resultado.dia_da_jornada`; `_dia_de` consulta esse mapa e só depois cai em `localtime(entrada).date()`. A janela de batidas inclui as pontas para capturar turnos cruzando a competência. (`app/ponto/services/fechamento.py:1217`, `app/ponto/services/dia_pago.py:126`, `app/ponto/services/fechamento.py:624`.)

- Há documentação antiga que ainda descreve a data civil da entrada como regra universal, mas o código acima prioriza o mapa. Isso aparece no comentário do calendário e na docstring/modelo `DiaPago`; não prova um novo segundo significado introduzido pelo diff. (`app/colaboradores/services/calendario.py:288`, `app/ponto/models.py:298`, `app/ponto/models.py:319`, `app/ponto/services/dia_pago.py:128`.)

Exceções e atomicidade financeira:

- O callback financeiro engole tanto falhas de execução quanto falhas ao registrar `on_commit`; não há operação de desfazer DNA nesse caminho. (`app/ponto/services/fechamento.py:1818`, `app/ponto/services/fechamento.py:1823`.)
- A persistência de `FechamentoMensal` ocorre antes da lavratura de `DiaPago`; falhas da lavratura são engolidas. O corpo de `lavrar` não envolve seu `delete` + `bulk_create` em um `atomic`. Assim o código permite divergência parcial em falha, conforme transação/erro concreto; ocorrência em produção é **[não verificado]**. (`app/ponto/services/fechamento.py:1144`, `app/ponto/services/fechamento.py:1160`, `app/ponto/services/fechamento.py:1201`, `app/ponto/services/fechamento.py:1238`, `app/ponto/services/dia_pago.py:135`, `app/ponto/services/dia_pago.py:359`.)

Não encontrei, nesses leitores financeiros examinados, uma condição que exija “VOLTA não recalcula”: eles consomem o gravado e aceitam sua atualização pelo evento. Essa conclusão cobre os caminhos citados, não prova ausência em toda a árvore. (`app/colaboradores/services/calendario.py:290`, `app/colaboradores/services/calendario.py:655`, `app/folha/export.py:711`, `app/folha/porta_export.py:284`.)

## 10/11 00:58 · sessao 1e22f8bbbcd9 · - · - · origem=Ronald (tmux)

- **Mordida real no golden:** `_dia_pago()` usa `.get()` de `DiaPago` motor/tipo dia; falta de fixture falha, não passa vazia. A montagem exige previsto 480 antes da VOLTA; golden exige DNA restaurado, `dna_anterior=None`, ata 240 e DiaPago 240. (`app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:56`, `:77`, `:88`)

- **Futuro/humano:** `juiz.assert_not_called()` isolado seria negativo fraco, mas vem acompanhado de `dinheiro.assert_called_once_with(...)`, DNA restaurado e ata ainda 480. Portanto o desaparecimento completo do rabo reprova esses casos. Porém não demonstram resultado financeiro: mockam o dinheiro; a ata deliberadamente permanece 480. (`app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:99`, `:111`, `:117`)

- **RED “4 failures”:** compatível com três comportamentais mais o AST real, mas execução contra base e banco não demonstrada pelo material disponível **[não verificado]**. O exemplo `_FONTE_BASE` é apenas string reduzida; seu teste exige rejeição, portanto continua verde quando o verificador funciona. (`app/ponto/tests/test_reverter_regen_sem_banco.py:25`, `:81`, `:90`)

- **AST:** executei somente leitura com `PYTHONDONTWRITEBYTECODE=1 python3 -B -m unittest app.ponto.tests.test_reverter_regen_sem_banco -v`: **11 testes OK** **[não verificado por arquivo:linha; execução nesta sessão]**. Valida uma definição, uma chamada comum, ausência de escritores diretos, posição imediatamente após o `with` e argumentos exatos. (`app/ponto/tests/test_reverter_regen_sem_banco.py:54`, `:59`, `:68`, `:76`)

- **Limite concreto AST:** executei mutações apenas em memória e o verificador aceitou (a) `return True` dentro do `atomic`, tornando o rabo inalcançável; (b) corpo `_o_chao_do_dia_mudou` reduzido a `pass` **[não verificado por arquivo:linha; execução nesta sessão]**. A causa está no escopo do verificador: examina chamadas e posição sintática apenas na VOLTA, sem provar fluxo nem comportamento do helper. Os comportamentais cobrem parte dessa lacuna. (`app/ponto/tests/test_reverter_regen_sem_banco.py:54`, `:59`, `:68`)

- **Lacunas dos novos testes:** não há casos de exportada/`apesar_da_lavra`, sem DNA anterior, lote/dedup, exceções, callbacks de rejunção/falta espúria nem `atomic` externo. O patch de `on_commit` chama callbacks imediatamente e ignora a semântica real de commit, por isso não demonstra ordem sob transação externa. (`app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:20`, `:74`, `:85`, `:88`, `:111`, `:117`)

- **Escritor ata/veredito:** `ata_do_dia` produz tipo, lâmpadas, minutos e contagens; cartório chama `cel.lavrar_veredito(...save=False, ata=...)` e `carimbar` persiste os campos pela porta `salvar_e_emitir`, que grava célula e evento no mesmo `atomic`. (`app/ponto/services/cartorio.py:696`, `:704`, `:1813`, `:1833`; `app/ponto/portas/celula.py:1032`, `:1052`, `:1070`, `:899`)

- **Leitores:** grade lê ata e dá prioridade ao previsto lavrado; somente ausência dessa chave consulta `minutos_previstos_do_dia`. Transporta veredito e ata para consumidores. `chamados.juizes.dia_sem_trabalho` lê `trabalha`/`ata.tipo_dia`, com exceção parcial; export lê veredito/protestos para retenção. (`app/escala/services/leitor_celula.py:379`, `:397`, `:399`, `:411`, `:424`, `:428`; `app/chamados/juizes.py:426`; `app/folha/export.py:1206`, `:1233`, `:1245`)

- **Guardas e semântica:** rabo exclui datas futuras e `_HUMANOS=('trabalhou','faltou','justificado')`; esses são precisamente os vereditos administrativos. Mesmo excluídos, dinheiro é disparado; como grade prioriza ata, DNA restaurado não implica previsto restaurado nesses casos. O teste confirma explicitamente ata 480. (`app/ponto/portas/celula.py:426`, `:431`, `:445`, `:458`; `app/colaboradores/services/pos_vinculo.py:14`; `app/escala/models.py:1759`; `app/escala/services/leitor_celula.py:399`; `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:109`)

- **Dia civil versus turno:** `julgar_celula` rejeita futuro; hoje exige fim de turno salvo folga/teto explícito. O batch trata passado cujo turno segue aberto e conta batidas pelo dia-turno da grade, não pela data civil da batida. Portanto o filtro `dia <= hoje` do rabo não substitui todas essas guardas do juiz. (`app/ponto/services/cartorio.py:1985`, `:1993`, `:1997`, `:1356`, `:1379`)

- **Humano não significa congelamento universal da ata:** o rabo pula vereditos humanos, mas a operação `lavrar_veredito` quando chamada pelo cartório sobre `veredito_via='admin'` preserva veredito e pode atualizar ata. Portanto a promessa do teste é sobre esse gatilho direto; callbacks e outros emissores precisam ser auditados separadamente. (`app/ponto/portas/celula.py:431`, `:1002`, `:1005`, `:1008`)

## 10/11 00:54 · sessao f30997273ac9 · PAUTA-SEM-TURNO-PREFIXO-RESERVADO · verificador · origem=Code

**1. CONFORME quanto aos leitores encontrados na cópia; comparação BYTE A BYTE com a base: [nao verificado].** O histórico necessário para conferir `a4b809999 → d2230cd…` não está disponível nesta cópia; portanto, preservação dos valores anteriores e diff integral permanecem **[nao verificado]**.

Valores atuais e donos conferidos abaixo. Na coluna final, escrita e leitura referem-se ao mesmo arquivo, salvo indicação explícita.

| Constante e valor atual | Declaração | Escritor; leitor |
|---|---|---|
| `PAUTA_LIMBO_EXPORTADA = 'LIMBO EM COMPETENCIA EXPORTADA'` | `app/pautas/marcadores.py:8` | `app/chamados/services/fabrica_da_celula.py:274`; leitura `:254` |
| `PAUTA_SEM_TURNO = 'CADASTRO: PASSADO SEM TURNO PREVISTO'` | `app/pautas/marcadores.py:11` | `app/chamados/services/fabrica_da_celula.py:362`; leitura `:305` |
| `ORFA_ENTERRADA = '[ORFA-ENTERRADA]'` | `app/pautas/marcadores.py:14` | `app/chamados/management/commands/reabrir_orfas_enterradas.py:209`; leitura `:202` |
| `PASSIVO_VALIDADO = '[PASSIVO-VALIDADO]'` | `app/pautas/marcadores.py:17` | `app/chamados/services/passivo_validado.py:230`; leitura `:164` |
| `PASSIVO_VALIDADO_FORA = '[PASSIVO-VALIDADO-FORA]'` | `app/pautas/marcadores.py:20` | `app/chamados/services/passivo_validado.py:267`; leitura `:256` |
| `ARVORE_VERMELHA = 'ARVORE VERMELHA'` | `app/pautas/marcadores.py:23` | `app/core/management/commands/alarme_arvore.py:52`; leitura `:16` |
| `ESTEIRA = 'ESTEIRA'` | `app/pautas/marcadores.py:26` | `app/core/management/commands/alarme_esteira.py:56`; leitura `:15` |
| `CRON_QUEBRADO = '[CRON QUEBRADO]'` | `app/pautas/marcadores.py:29` | `app/core/services/crons_quebrados.py:72`; leitura `:54` |
| `FERIAS['pa_vencido'] = '[FERIAS/PA-VENCIDO]'` | `app/pautas/marcadores.py:33` | `app/ferias/management/commands/pautas_de_ferias.py:164`; leitura `:152` |
| `FERIAS['pa_vencendo'] = '[FERIAS/PA-VENCENDO]'` | `app/pautas/marcadores.py:34` | `app/ferias/management/commands/pautas_de_ferias.py:164`; leitura `:152` |
| `FERIAS['cadastro_pa'] = '[FERIAS/CADASTRO]'` | `app/pautas/marcadores.py:35` | `app/ferias/management/commands/pautas_de_ferias.py:164`; leitura `:152` |
| `PRE_FECHAMENTO = '[PRE-FECHAMENTO]'` | `app/pautas/marcadores.py:39` | `app/folha/management/commands/pre_fechamento.py:111`; leituras `:92`, `:98` |
| `DIA_EM_ABERTO = '[DIA EM ABERTO]'` | `app/pautas/marcadores.py:42` | `app/pautas/management/commands/pauta_dia_em_aberto.py:140`; leitura `:108` |
| `FOLGA_EXPORTADA = '[FOLGA NAO DEFINIDA - EXPORTADA]'` | `app/pautas/marcadores.py:45` | `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:135`; leitura `:126` |
| `PAGO_DEVIDO_EXPORTADA = '[PAGO X DEVIDO - EXPORTADA]'` | `app/pautas/marcadores.py:48` | `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:208`, `:212`; leituras `:243`, `:250`, `:277` |
| `REVISAO_AUSENCIA = '[A6/REVISAO-SEMANAL]'` | `app/pautas/marcadores.py:51` | `app/ponto/services/vigia_ausencia.py:239` → `app/pautas/semanal.py:278`; leituras `app/pautas/semanal.py:199`, `:228` |
| `REVISAO_SUPERVISAO = '[SUP/REVISAO-SEMANAL]'` | `app/pautas/marcadores.py:54` | `app/escala/services/fase_12x36.py:278` → `app/pautas/semanal.py:278`; leituras `app/pautas/semanal.py:199`, `:228` |
| `TEXTO_AFASTADO`; `AFASTADO = TEXTO_AFASTADO[:40]` | `app/pautas/marcadores.py:57`, `:77` | `app/ponto/services/afastado_avisa.py:76`; leitura `:63` |
| `HE_EXPORTADA = '[HE-AUTORIZADA-EM-EXPORTADA]'` | `app/pautas/marcadores.py:62` | `app/ponto/services/autorizacao_he_periodo.py:245`; leitura `:242` |
| `COBERTURA_A_CONFIRMAR = '[COBERTURA-A-CONFIRMAR]'` | `app/pautas/marcadores.py:65` | `app/ponto/services/cobertura.py:336`; leitura `:243` |
| `APP_401 = '[APP-401]'` | `app/pautas/marcadores.py:68` | `app/ponto/services/pautas_dos_deslogados.py:78`; leitura `:66` |
| `DISCORDANTE = '[DISCORDANTE]'` | `app/pautas/marcadores.py:71` | `app/ponto/services/pautas_dos_discordantes.py:180`; leituras `:98`, `:166` |
| `TRANCA = '[TRANCA]'` | `app/pautas/marcadores.py:74` | `app/chamados/services/competencia_trancada.py:235`; leitura `:221` |
| `SEM_DOCUMENTO = '[A4/SEM-DOCUMENTO]'` | `app/pautas/marcadores.py:80` | `app/ponto/services/ausencia.py:1060`; leitura `:1044` |
| `FERIAS_RECUSADA = '[A1-FERIAS/AG#'` | `app/pautas/marcadores.py:81` | `app/ponto/services/ausencia.py:1250`; leitura `:1234` |
| `ESMERIL = '[ESMERIL '` — espaço final significativo | `app/pautas/marcadores.py:84` | `app/ponto/services/pautas_do_esmeril.py:79`; formação `:16`, `:50`; leituras `:66`, `:67` |

`TEXTO_AFASTADO` contém exatamente a concatenação abaixo; seu prefixo reservado é `Bateu ponto durante afastamento. A batid` (`app/pautas/marcadores.py:57`, `:77`):

```text
Bateu ponto durante afastamento. A batida foi registrada e o dia segue explicado pela ausencia -- nao e recusa. Confira se o afastamento ainda vale: se o colaborador voltou, encerre a AUSENCIA de afastamento (a situacao sai dela, nao do cadastro); se nao voltou, oriente que nao precisa bater.
```

**CONFORME — as derivações encontradas estão cobertas pela guarda atual**, pois continuam começando pelo reservado: cron acrescenta nome/empresa; semanal acrescenta semana; pago/devido distingue cabeça e filha; férias recusada acrescenta identificador e `]`; esmeril acrescenta ação e `]` (`app/core/services/crons_quebrados.py:54`; `app/pautas/semanal.py:199`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:243`, `:250`; `app/ponto/services/ausencia.py:1231`; `app/ponto/services/pautas_do_esmeril.py:16`; `app/pautas/services.py:45`).

Não apareceu outro prefixo de produção fora do catálogo no rastreio estático. A ausência absoluta de leitores dinâmicos ou de outras formas não detectáveis por essa busca permanece **[nao verificado]**. O selo novo fiscaliza somente keywords `texto__startswith`; não fiscaliza `contains`, `icontains` ou regex (`app/pautas/tests/test_prefixos_reservados_ast.py:155-164`).

**2. PROCEDENTE — a guarda recusa prosa além da forma reconhecida pelos leitores.** Ela aplica `strip().casefold()` e compara o prefixo bruto de todos os reservados (`app/pautas/services.py:44-45`).

| Caso | Resultado e diferença para o leitor |
|---|---|
| `ESTEIRANTE` ou `ESTEIRA de compras` | A guarda bloqueia pelos primeiros **7 caracteres**; o leitor exige `ESTEIRA <chave>:` (`app/pautas/marcadores.py:26`; `app/core/management/commands/alarme_esteira.py:15`). |
| `ARVORE VERMELHA no jardim` | Bloqueio pelos primeiros **15 caracteres**, mas aqui o próprio leitor também captura essa prosa (`app/pautas/marcadores.py:23`; `app/core/management/commands/alarme_arvore.py:16`). |
| Texto começando por `Bateu ponto durante afastamento. A batid` | Bloqueio pelos primeiros **40 caracteres**, embora o escritor emita a sentença completa; o leitor realmente usa esses 40 (`app/pautas/marcadores.py:77`; `app/ponto/services/afastado_avisa.py:63`, `:76`). |
| `[A1-FERIAS/AG#abc] relato` | A guarda bloqueia o prefixo aberto; o leitor monta identificador de agendamento seguido de `]` (`app/pautas/marcadores.py:81`; `app/ponto/services/ausencia.py:1231-1234`). |

Os leitores não fazem `casefold` explicitamente; há também leitura Python por `str.startswith`, sensível à caixa (`app/ponto/services/pautas_dos_discordantes.py:98`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:277`). Collation e comportamento efetivo do banco de produção: **[nao verificado]**.

**Correção mínima:** normalizar como o escritor (`strip`) e reservar as formas efetivamente lidas, incluindo delimitadores e sufixos necessários; retirar `casefold` onde a leitura for sensível à caixa (`app/pautas/services.py:177`; `app/core/management/commands/alarme_esteira.py:15`; `app/ponto/services/ausencia.py:1231`).

Para `ARVORE VERMELHA` e `AFASTADO`, **não existe ajuste somente na guarda que simultaneamente libere essa prosa e preserve os leitores atuais**: eles próprios classificam a prosa como identidade do sistema. É necessário estreitar também o leitor ou distinguir origem/identidade técnica do texto; compatibilidade histórica dessa alteração: **[nao verificado]** (`app/core/management/commands/alarme_arvore.py:16`; `app/ponto/services/afastado_avisa.py:63`).

Ocorrências humanas legítimas em produção: **[nao verificado]**.

**3. PROCEDENTE — existe comando humano sem guarda.** `suporte_hasner --responder ID --texto ...` aceita texto livre, verifica apenas vazio e chama `sv.responder` diretamente (`app/pautas/management/commands/suporte_hasner.py:45`, `:47`, `:127-134`). O serviço encaminha ao escritor comum, que não aplica a reserva humana (`app/pautas/services.py:177`, `:194`, `:208-214`).

**Correção mínima:** chamar `validar_texto_humano(texto)` no comando antes de responder e converter `ValidationError` em `CommandError` (`app/pautas/management/commands/suporte_hasner.py:132-138`).

Esse comando restringe o pai ao departamento de suporte; não prova, por si, falsificação de cabeça `PAUTA_SEM_TURNO`, cujo seletor exige `pai__isnull=True`. Impacto concreto em produção: **[nao verificado]** (`app/pautas/management/commands/suporte_hasner.py:122`; `app/chamados/services/fabrica_da_celula.py:304`).

**CONFORME nas rotas examinadas:** escrever e responder HTTP chamam a guarda; o URLconf de Pauta não expõe edição/encaminhamento; a API de mensageria examinada lista pautas (`app/pautas/views.py:76`, `:114`; `app/pautas/urls.py:7-14`; `app/api/views_mensageria.py:1323-1347`). Ausência universal de admin ou serializador registrado dinamicamente: **[nao verificado]**.

**4. CONFORME — catálogo folha e direção A16.** `pautas/marcadores.py` contém dados e derivações locais, sem imports de donos/models; os donos importam esse catálogo diretamente (`app/pautas/marcadores.py:1-111`; `app/ponto/services/afastado_avisa.py:11`; `app/chamados/services/fabrica_da_celula.py:239`, `:296`). Essa dependência não cria retorno pelo catálogo nem cruza a fronteira `ponto ↔ chamados` fiscalizada pelo A16 (`app/core/tests/test_contract_direcao_a16.py:99-104`, `:112-137`).

Quais donos **passaram** a importar `pautas.*`, comparados à base: **[nao verificado]**. A suíte A16 completa também permanece **[nao verificado]**; o teste novo executa somente seu detector extraído (`app/pautas/tests/test_prefixos_reservados_ast.py:236-265`).

**5. PROCEDENTE — há bloqueadores de certificação.**

- **Censo incompatível com a montagem da suíte:** executa `git ls-files`, mas a suíte monta apenas `app/` e a cópia de commit deliberadamente não contém `.git`. Correção mínima: enumerar os Python da árvore montada, preservando exclusões e sem depender do Git (`app/pautas/tests/test_prefixos_reservados_ast.py:19-25`; `bin/suite.sh:100-103`; `bin/arvore_do_push.sh:7`).

- **Selo legado do juiz ficou incompatível:** exige a definição de `PAUTA_SEM_TURNO` na fábrica e proíbe sua definição/uso nos novos locais. A reprodução isolada do corpo AST falhou nessa exigência; execução no runner Django: **[nao verificado]**. Correção mínima: admitir estritamente definição/inclusão no catálogo e import na fábrica, mantendo os usos operacionais restritos aos donos (`app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:147-150`, `:251-260`; `app/chamados/services/fabrica_da_celula.py:296`; `app/pautas/marcadores.py:11`, `:88`).

- **Os 59 mutantes não rodam automaticamente na suíte:** o executor chama o laço somente em `main`, sob `__name__ == '__main__'`; o runner normal delega à descoberta de testes. Correção mínima: expor uma asserção coletável que execute os mutantes em memória, após eliminar a dependência de Git (`app/pautas/tests/mutar_prefixos_reservados.py:85`, `:110`, `:151-152`; `app/core/runner_quarentena.py:75-76`; `app/core/tests/test_censo_escritas.py:278`).

- **Skip amplo demais:** qualquer `ImportError`, inclusive transitivo de produção, vira “Django não instalado”. Correção mínima: permitir skip somente pela ausência efetiva de Django e propagar os demais erros (`app/pautas/tests/test_prefixos_reservados.py:134-146`).

**CONFORME — os testes normais têm asserções reais**, incluindo recusa antes da escrita e contagem de cabeças/filhas quando Django carrega. A ausência de Django no construtor não torna o skip permanente: existe ramo de integração e a dependência está declarada (`app/pautas/tests/test_prefixos_reservados.py:81-98`, `:147`, `:173-200`; `app/requirements.txt:13`).

O selo AST verifica origem do **leitor**, não igualdade entre escritor e leitor; tampouco verifica os sufixos derivados. Portanto, não certifica preservação BYTE A BYTE (`app/pautas/tests/test_prefixos_reservados_ast.py:126-133`, `:155-164`). Integração Django, RED na base e execução dos 59 mutantes nesta cópia: **[nao verificado]**.

**6. Classes solicitadas.**

| Classe | Veredito |
|---|---|
| **2º juiz** | **CONFORME — não identificado nesta regra.** Escritor consulta `pauta_cadastro_existente` após a trava; contador reutiliza o seletor (`app/chamados/services/fabrica_da_celula.py:318`, `:336`, `:355-358`). |
| **2º escritor fora da porta** | **CONFORME — não identificado no rastreio.** Respostas e comando usam o escritor comum; o defeito do comando é contornar a guarda humana (`app/pautas/services.py:194`, `:208`; `app/pautas/management/commands/suporte_hasner.py:133`). |
| **Relógio fora da borda** | **CONFORME no catálogo/guarda: não.** A validação usa somente texto e catálogo; alterações históricas fora desse escopo: **[nao verificado]** (`app/pautas/services.py:38-48`; `app/pautas/marcadores.py:77`). |
| **Derivação paralela de fonte única** | **CONFORME na cópia: não identificada.** O corte de 40 caracteres nasce no catálogo e o dono importa texto e prefixo (`app/pautas/marcadores.py:77`; `app/ponto/services/afastado_avisa.py:11`). |
| **Selo/RED sem mordida** | **PROCEDENTE — sim quanto à certificação automática dos mutantes**, além dos bloqueadores de Git e contrato legado. Correções no item 5; os testes normais não são vazios (`app/pautas/tests/mutar_prefixos_reservados.py:151`; `app/pautas/tests/test_prefixos_reservados.py:81`). |
| **Campo com dois significados — ESMERIL-SEMANTICA** | **PROCEDENTE — sim, persiste.** `texto` guarda prosa e também identidade/ação interpretada pelo sistema. Fechar todas as portas humanas reduz a colisão; eliminar a dualidade exige identidade/origem técnica separada da prosa (`app/pautas/models.py:89`; `app/ponto/services/pautas_do_esmeril.py:50`, `:66-68`; `app/chamados/services/fabrica_da_celula.py:305`). |

## 10/11 00:53 · sessao 45b533905b0e · REVERTER-DIA-SEM-RECALCULO · construtor · origem=Code

Criados somente:

- [test_reverter_regen_recalcula_dinheiro.py:61](/home/ronald/wt-codex-reverter-dia-sem-recalculo-004859/app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:61): arranjo 240 → IDA 480 → VOLTA 240, com `on_commit` imediato; inclui futuro e veredito humano na mesma fixture.
- [test_reverter_regen_sem_banco.py:52](/home/ronald/wt-codex-reverter-dia-sem-recalculo-004859/app/ponto/tests/test_reverter_regen_sem_banco.py:52): contrato AST e nove mutantes locais, sem variável de ambiente.

**RED do (b) na base:** 11 testes, uma falha no [fonte real:83](/home/ronald/wt-codex-reverter-dia-sem-recalculo-004859/app/ponto/tests/test_reverter_regen_sem_banco.py:83): [nao verificado]

```text
AssertionError: VOLTA deve chamar o rabo comum uma vez e nenhum escritor direto: rabos=0; diretas=['julgar_celula']
FAILED (failures=1)
```

Comando executado:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest \
  ponto.tests.test_reverter_regen_sem_banco -v
```

Os nove mutantes foram rejeitados pelas asserções dos [casos:90](/home/ronald/wt-codex-reverter-dia-sem-recalculo-004859/app/ponto/tests/test_reverter_regen_sem_banco.py:90). Comando executado separadamente para cada um:

```bash
for caso in bloco_da_base sem_rabo rabo_duplicado juiz_paralelo \
  dinheiro_paralelo rabo_dentro_do_atomic dias_vazios \
  recalculo_desligado motivo_perdido
do
  PYTHONDONTWRITEBYTECODE=1 python3 -m unittest \
    "ponto.tests.test_reverter_regen_sem_banco.VerificadorMordeTest.test_morde_$caso" \
    -v || exit
done
```

**Mock existente:** nenhum ajuste indicado pela leitura. O [vizinho:262](/home/ronald/wt-codex-reverter-dia-sem-recalculo-004859/app/ponto/tests/test_porta_celula_dia.py:262) intercepta `ponto.services.cartorio.julgar_celula`; o [rabo comum:425](/home/ronald/wt-codex-reverter-dia-sem-recalculo-004859/app/ponto/portas/celula.py:425) importa esse mesmo alvo e conserva os argumentos esperados. Compatibilidade executada: **[não verificado]**.

Ruff retornou `All checks passed!` nos dois arquivos:

```bash
/tmp/claude-1001/-home-ronald-saas-hasner-app/707e509b-29e4-4438-9018-16733980c39a/scratchpad/ruffpkg/bin/ruff \
  check --no-cache \
  ponto/tests/test_reverter_regen_recalcula_dinheiro.py \
  ponto/tests/test_reverter_regen_sem_banco.py
```

**Sem rodar:** os três casos ORM de (a), inclusive seu RED financeiro; (b) pelo runner Django; e os vizinhos `test_porta_celula_dia` e `test_ordem_regen_julga_antes_do_dinheiro`. Django está ausente no Python consultado, e banco foi vedado nesta volta. Resultados dessas execuções: **[não verificado]**.

## 10/11 00:51 · sessao 3dce6c2db4b1 · - · - · origem=Ronald (tmux)

1. **CONFORME quanto ao catálogo atual:** censo em memória de todos os Python de produção encontrou 26 reservados e nenhum `texto__startswith` fora dele; os pontos de entrada são `app/pautas/marcadores.py:86` e `app/pautas/tests/test_prefixos_reservados_ast.py:155`. Não encontrei `texto__contains`/`texto__icontains` de produção na cópia; essa ausência resulta da busca, não tem linha própria. **Comparação BYTE A BYTE com a4b809999: [nao verificado]**, porque a cópia não contém `.git`/base acessível.

Mapa atual conferido; E = escrita; L = leitura:

| Constante; valor/declaração | E | L |
|---|---|---|
| `PAUTA_LIMBO_EXPORTADA = 'LIMBO EM COMPETENCIA EXPORTADA'`, `app/pautas/marcadores.py:8` | `app/chamados/services/fabrica_da_celula.py:274` | mesmo arquivo:254 |
| `PAUTA_SEM_TURNO = 'CADASTRO: PASSADO SEM TURNO PREVISTO'`, catálogo:11 | fábrica:362 | fábrica:305 |
| `ORFA_ENTERRADA = '[ORFA-ENTERRADA]'`, catálogo:14 | `app/chamados/management/commands/reabrir_orfas_enterradas.py:209` | mesmo:202 |
| `PASSIVO_VALIDADO = '[PASSIVO-VALIDADO]'`, catálogo:17 | `app/chamados/services/passivo_validado.py:230` | mesmo:164 |
| `PASSIVO_VALIDADO_FORA = '[PASSIVO-VALIDADO-FORA]'`, catálogo:20 | passivo:267 | passivo:256 |
| `ARVORE_VERMELHA = 'ARVORE VERMELHA'`, catálogo:23 | `app/core/management/commands/alarme_arvore.py:52` | mesmo:16 |
| `ESTEIRA = 'ESTEIRA'`, catálogo:26 | `app/core/management/commands/alarme_esteira.py:56` | mesmo:15 |
| `CRON_QUEBRADO = '[CRON QUEBRADO]'`, catálogo:29 | `app/core/services/crons_quebrados.py:72` | mesmo:54 |
| `FERIAS`: `'[FERIAS/PA-VENCIDO]'`, `'[FERIAS/PA-VENCENDO]'`, `'[FERIAS/CADASTRO]'`, catálogo:32 | `app/ferias/management/commands/pautas_de_ferias.py:164` | mesmo:152 |
| `PRE_FECHAMENTO = '[PRE-FECHAMENTO]'`, catálogo:39 | `app/folha/management/commands/pre_fechamento.py:111` | mesmo:92 e :98 |
| `DIA_EM_ABERTO = '[DIA EM ABERTO]'`, catálogo:42 | `app/pautas/management/commands/pauta_dia_em_aberto.py:140` | mesmo:108 |
| `FOLGA_EXPORTADA = '[FOLGA NAO DEFINIDA - EXPORTADA]'`, catálogo:45 | `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:135` | mesmo:126 |
| `PAGO_DEVIDO_EXPORTADA = '[PAGO X DEVIDO - EXPORTADA]'`, catálogo:48 | `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:208` e :212 | mesmo:243, :250 e :277 |
| `REVISAO_AUSENCIA = '[A6/REVISAO-SEMANAL]'`, catálogo:51 | `app/ponto/services/vigia_ausencia.py:239` → `app/pautas/semanal.py:278` | semanal:199 e :228 |
| `REVISAO_SUPERVISAO = '[SUP/REVISAO-SEMANAL]'`, catálogo:54 | `app/escala/services/fase_12x36.py:278` → semanal:278 | semanal:199 e :228 |
| `TEXTO_AFASTADO`, 293 caracteres, catálogo:57; `AFASTADO = TEXTO_AFASTADO[:40]`, catálogo:77 | `app/ponto/services/afastado_avisa.py:76` | mesmo:63 |
| `HE_EXPORTADA = '[HE-AUTORIZADA-EM-EXPORTADA]'`, catálogo:62 | `app/ponto/services/autorizacao_he_periodo.py:245` | mesmo:242 |
| `COBERTURA_A_CONFIRMAR = '[COBERTURA-A-CONFIRMAR]'`, catálogo:65 | `app/ponto/services/cobertura.py:336` | mesmo:243 |
| `APP_401 = '[APP-401]'`, catálogo:68 | `app/ponto/services/pautas_dos_deslogados.py:78` | mesmo:66 |
| `DISCORDANTE = '[DISCORDANTE]'`, catálogo:71 | `app/ponto/services/pautas_dos_discordantes.py:180` | mesmo:98 e :166 |
| `TRANCA = '[TRANCA]'`, catálogo:74 | `app/chamados/services/competencia_trancada.py:235` | mesmo:221 |
| `SEM_DOCUMENTO = '[A4/SEM-DOCUMENTO]'`, catálogo:80 | `app/ponto/services/ausencia.py:1060` | mesmo:1044 |
| `FERIAS_RECUSADA = '[A1-FERIAS/AG#'`, catálogo:81 | ausência:1250 | ausência:1231 e :1234 |
| `ESMERIL = '[ESMERIL '`, catálogo:84 | `app/ponto/services/pautas_do_esmeril.py:79` (formatação :50 e :16) | mesmo:66 e :67 |

Nas linhas abreviadas “catálogo”, trata-se de `app/pautas/marcadores.py`; “fábrica”, `app/chamados/services/fabrica_da_celula.py`; “passivo”, `app/chamados/services/passivo_validado.py`; “semanal”, `app/pautas/semanal.py`; “ausência”, `app/ponto/services/ausencia.py`.

2. **PROCEDENTE: guarda mais larga que as buscas.** `strip().casefold()` + prefixo bruto está em `app/pautas/services.py:44`. Demonstração em memória: recusa `ESTEIRANTE`, `ESTEIRA de compras`, `esteira de compras`, `arvore vermelha no jardim`, `[A1-FERIAS/AG#abc] relato`. O leitor ESTEIRA exige `'ESTEIRA <chave>:'` (`app/core/management/commands/alarme_esteira.py:15`), e férias exige pk seguido de `]` (`app/ponto/services/ausencia.py:1231`). Esses exemplos não apresentam as formas completas exigidas pelos respectivos leitores.

Os prefixos têm 7 (`ESTEIRA`), 15 (`ARVORE VERMELHA`) e 40 caracteres (`'Bateu ponto durante afastamento. A batid'`); suas declarações são `app/pautas/marcadores.py:26`, :23 e :77. A guarda recusa prosa começando pelos 40 caracteres, enquanto o escritor emite os 293 de `TEXTO_AFASTADO` (`app/ponto/services/afastado_avisa.py:76`). A existência de casos humanos reais em produção é **[nao verificado]**.

Correção mínima: fazer a guarda reproduzir o texto persistido (`strip`, `app/pautas/services.py:177`) e os predicados reais dos leitores; eliminar `casefold` onde o leitor é sensível à caixa; reservar ESTEIRA com chave e `:`, férias recusada com número e `]`. A configuração da suíte usa PostgreSQL (`app/config/settings_test.py:4`); comportamento efetivo do banco não foi exercitado **[nao verificado]**.

**ARVORE/AFASTADO exigem corrigir também o leitor**: os leitores atuais capturam a própria prosa de prefixo comum (`app/core/management/commands/alarme_arvore.py:16`, `app/ponto/services/afastado_avisa.py:63`). Só afrouxar a guarda reabre a falsificação. Pode-se estreitar ambos os lados para a sentença de sistema completa, ou separar identidade/origem de texto livre; a compatibilidade com registros históricos é **[nao verificado]**.

3. **CONFORME quanto às diferenças locais verificadas:** as derivações presentes continuam começando pelo reservado: passivo inclui colab/competência (`app/chamados/services/passivo_validado.py:159`), cron inclui nome/empresa (`app/core/services/crons_quebrados.py:54`), semanal inclui semana (`app/pautas/semanal.py:199`), pago/devido inclui chave e formas distintas de cabeça/filha (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:243`, :250). A guarda bruta cobre essas formas, com excesso; não achei leitor dessas famílias cuja forma escapasse à guarda atual.

4. **CONFORME quanto a ESMERIL reservado:** escritor deriva `'[ESMERIL %s]'` (`app/ponto/services/pautas_do_esmeril.py:16`), leitor busca `'[ESMERIL '` e extrai ação até `]` (`mesmo:66`, :67). A guarda cobre inclusive ações desconhecidas, como o leitor amplo atual (`app/pautas/services.py:45`).

## 10/11 00:51 · sessao f41df8bfb784 · - · - · origem=Ronald (tmux)

4. **CONFORME:** catálogo sem imports e sem dependência de donos/models; import isolado carregou apenas `pautas` e `pautas.marcadores` (`app/pautas/marcadores.py:1`, `app/pautas/marcadores.py:86`). Donos importam diretamente esse módulo, por exemplo `app/ponto/services/afastado_avisa.py:11`, `app/chamados/services/fabrica_da_celula.py:239`, `app/core/management/commands/alarme_esteira.py:10`. Esses imports não cruzam a fronteira ponto↔chamados fiscalizada por A16 (`app/core/tests/test_contract_direcao_a16.py:99`, `app/core/tests/test_contract_direcao_a16.py:112`). O detector extraído passou; suíte A16 completa e comparação dos imports anteriores: **[nao verificado]** (`app/pautas/tests/test_prefixos_reservados_ast.py:236`).

5. **PROCEDENTE — infraestrutura do selo incompatível com runner:** o censo executa `git ls-files` (`app/pautas/tests/test_prefixos_reservados_ast.py:20`), enquanto a suite monta apenas `app/` em `/app` (`bin/suite.sh:100`, `bin/regua.sh:167`) e a porta declara que a cópia no container não contém `.git` (`bin/arvore_do_push.sh:7`). Correção mínima: enumerar os arquivos Python da árvore montada, preservando as exclusões atuais (`app/pautas/tests/test_prefixos_reservados_ast.py:23`). Execução real no container: **[nao verificado]**. Nesta cópia, rodagem isolada resultou em 12 testes, um erro por ausência de `.git` e um skip por Django ausente; causa localizada em `app/pautas/tests/test_prefixos_reservados_ast.py:20` e `app/pautas/tests/test_prefixos_reservados.py:142`.

**PROCEDENTE — mutantes fora da suite:** `mutar_prefixos_reservados.py` é executor optativo; o laço está em `main`, chamado somente sob `__name__ == '__main__'` (`app/pautas/tests/mutar_prefixos_reservados.py:85`, `app/pautas/tests/mutar_prefixos_reservados.py:110`, `app/pautas/tests/mutar_prefixos_reservados.py:151`). Suite e régua rodam descoberta normal de labels (`bin/suite.sh:102`, `bin/regua.sh:168`); o runner só delega e aplica quarentena (`app/core/runner_quarentena.py:75`). Mesmo carregado explicitamente pelo loader unittest, o módulo fornece apenas os três testes das classes importadas (`app/pautas/tests/mutar_prefixos_reservados.py:19`), sem executar os mutantes. Correção mínima: expor execução e asserção dos mutantes num teste coletável, após retirar dependência de `.git`.

**CONFORME, com limite:** testes normais têm mordida: verificam recusa antes da escrita (`app/pautas/tests/test_prefixos_reservados.py:81`), aceitação de prosa (`app/pautas/tests/test_prefixos_reservados.py:91`) e, quando Django está disponível, status e contagem real de cabeças/filhas (`app/pautas/tests/test_prefixos_reservados.py:173`, `app/pautas/tests/test_prefixos_reservados.py:186`). Ausência de Django no construtor não torna o skip permanente: existe ramo de integração após import bem-sucedido (`app/pautas/tests/test_prefixos_reservados.py:147`), e Django está declarado em `app/requirements.txt:13`.

**PROCEDENTE — skip excessivamente amplo:** qualquer `ImportError` das dependências transitivas vira “Django não instalado” (`app/pautas/tests/test_prefixos_reservados.py:134`, `app/pautas/tests/test_prefixos_reservados.py:142`). Correção mínima: condicionar somente a ausência efetiva de Django e propagar demais falhas de importação.

**Limite do selo:** ele verifica apenas keywords `texto__startswith`, não os escritores nem contains/icontains/regex (`app/pautas/tests/test_prefixos_reservados_ast.py:155`). Aceita origem da concatenação pela esquerda sem verificar sufixos (`app/pautas/tests/test_prefixos_reservados_ast.py:126`). Probe em memória com escritor divergente e leitor usando `TRANCA` retornou zero erros; portanto igualdade escritor/leitor não é certificada por esse selo (`app/pautas/tests/test_prefixos_reservados_ast.py:159`).

6. **Selo/RED sem mordida: SIM quanto à certificação automática dos mutantes; NÃO quanto a vazio dos testes normais**, conforme distinção acima (`app/pautas/tests/mutar_prefixos_reservados.py:151`, `app/pautas/tests/test_prefixos_reservados.py:81`). **Relógio fora da borda: não identificado no catálogo/guarda/testes novos**; a guarda usa apenas texto e catálogo (`app/pautas/services.py:38`), e o catálogo contém dados (`app/pautas/marcadores.py:8`). Demais classes ficam para a consolidação dos outros rastreios.

## 10/11 00:50 · sessao d458f6a143bf · - · - · origem=Ronald (tmux)

3. **PROCEDENTE — comando manual sem guarda.** `--responder/--texto` recebe texto livre (`app/pautas/management/commands/suporte_hasner.py:45`, `:47`, `:63`), valida somente vazio (`:129`) e chama `sv.responder` (`:133`). O service delega a `escrever` (`app/pautas/services.py:208`), que aplica `strip` e persiste sem a guarda (`:177`, `:194`). Correção mínima: chamar `validar_texto_humano(texto)` nesse comando e converter `ValidationError` em `CommandError`. A restrição do comando ao próprio departamento existe (`app/pautas/management/commands/suporte_hasner.py:122`); impacto concreto sobre algum leitor em produção: **[nao verificado]**.

3. **CONFORME — rotas HTTP examinadas.** As duas rotas de texto livre chamam a guarda (`app/pautas/views.py:76`, `:114`). O conjunto de rotas de Pauta não expõe edição nem encaminhamento (`app/pautas/urls.py:7`). A API de mensageria lista pautas usando serviços de leitura (`app/api/views_mensageria.py:1323`, `:1329`, `:1340`). Ausência universal de admin/serializador dinâmico: **[nao verificado]**; nenhum registro explícito foi encontrado.

6. **Segundo escritor fora da porta: CONFORME no rastreio.** A criação encontrada está centralizada em `app/pautas/services.py:194`; respostas delegam à mesma porta (`:208`). O comando acima contorna a guarda humana, mas usa o escritor comum (`app/pautas/management/commands/suporte_hasner.py:133`).

6. **Campo com dois significados: PROCEDENTE, persistente na cópia.** `texto` armazena prosa (`app/pautas/models.py:89`) e também identidade/ação: o esmeril monta o corpo e extrai ação do prefixo (`app/ponto/services/pautas_do_esmeril.py:50`, `:66`, `:67`); o juiz de cadastro seleciona por prefixo textual (`app/chamados/services/fabrica_da_celula.py:304`, `:305`, `:318`). Correção mínima desta obra: fechar todas as portas humanas; cura estrutural exigiria identidade própria separada da prosa. Introdução desse problema no diff: **[nao verificado]**.

A resposta é o próprio modelo Pauta com `pai`, sem modelo PautaResposta separado (`app/pautas/models.py:99`). Texto humano encaminhado por `resposta_tardia` recebe cabeçalho fixo antes do conteúdo, impedindo que o usuário escolha o início (`app/chamados/services/resposta_tardia.py:62`, `:65`).

## 10/11 00:47 · sessao 557d940ccf3a · PAUTA-SEM-TURNO-PREFIXO-RESERVADO · construtor · origem=Code

Cura implementada para certificação: uma guarda em [pautas/services.py:38](/home/ronald/wt-codex-pauta-sem-turno-prefixo-reservado-003109/app/pautas/services.py:38), chamada pelas portas humanas de [escrever:76](/home/ronald/wt-codex-pauta-sem-turno-prefixo-reservado-003109/app/pautas/views.py:76) e [responder:114](/home/ronald/wt-codex-pauta-sem-turno-prefixo-reservado-003109/app/pautas/views.py:114).

1. **Direção.** Sim: `import_module` escapa do detector A16, que examina apenas `Import`/`ImportFrom` de topo entre `ponto` e `chamados` (`core/tests/test_contract_direcao_a16.py:78`). Escolhi o módulo-folha [pautas/marcadores.py:1](/home/ronald/wt-codex-pauta-sem-turno-prefixo-reservado-003109/app/pautas/marcadores.py:1): os donos importam constantes com seus aliases anteriores, e a guarda importa a mesma fonte. O detector real do A16 passou na execução isolada; a suíte A16 completa continua pendente (`pautas/tests/test_prefixos_reservados_ast.py:236`). O novo selo também recusa imports estáticos e dinâmicos no catálogo (`pautas/tests/test_prefixos_reservados_ast.py:28`).

2. **Prefixos novos.** Entram os quatro: afastamento, sem documento, férias recusadas e esmeril. Todos identificam buscas reais por `texto__startswith` (`ponto/services/afastado_avisa.py:63`, `ponto/services/ausencia.py:1044`, `ponto/services/ausencia.py:1234`, `ponto/services/pautas_do_esmeril.py:66`). Reservar `Bateu ponto durante afastamento. A batid` bloqueia também prosa humana iniciada nesses 40 caracteres; ocorrência legítima em produção é **[nao verificado]** (`pautas/marcadores.py:77`, `pautas/services.py:44`).

3. **Filhas.** Guardei também `responder`: há leitores sem filtro de pai e um leitor deliberado de filhas marcadas. Guardar somente cabeças deixaria a falsificação disponível por resposta (`ponto/services/ausencia.py:1044`, `pautas/management/commands/pauta_pago_x_devido_exportada.py:250`, `pautas/views.py:114`).

Os leitores conservam os prefixos exatos e suas derivações por semana, competência, colaborador ou chave. A normalização nova fica somente na guarda humana; o escritor comum continua aplicando seu `strip` anterior (`pautas/services.py:44`, `pautas/services.py:177`, `pautas/semanal.py:199`, `chamados/services/passivo_validado.py:159`).

Os donos abaixo foram tocados para importar a fonte única e reaproveitar as constantes nas buscas e escritas:

| Constante/família | Escritor | Leitor |
|---|---|---|
| `PAUTA_LIMBO_EXPORTADA` | `chamados/services/fabrica_da_celula.py:274` | `chamados/services/fabrica_da_celula.py:254` |
| `PAUTA_SEM_TURNO` | `chamados/services/fabrica_da_celula.py:362` | `chamados/services/fabrica_da_celula.py:305` |
| `TRANCA` | `chamados/services/competencia_trancada.py:235` | `chamados/services/competencia_trancada.py:221` |
| `ORFA_ENTERRADA` | `chamados/management/commands/reabrir_orfas_enterradas.py:209` | `chamados/management/commands/reabrir_orfas_enterradas.py:202` |
| `PASSIVO_VALIDADO` | `chamados/services/passivo_validado.py:230` | `chamados/services/passivo_validado.py:164` |
| `PASSIVO_VALIDADO_FORA` | `chamados/services/passivo_validado.py:267` | `chamados/services/passivo_validado.py:256` |
| `ARVORE_VERMELHA` | `core/management/commands/alarme_arvore.py:52` | `core/management/commands/alarme_arvore.py:16` |
| `ESTEIRA` | `core/management/commands/alarme_esteira.py:56` | `core/management/commands/alarme_esteira.py:15` |
| `CRON_QUEBRADO` | `core/services/crons_quebrados.py:72` | `core/services/crons_quebrados.py:54` |
| `FERIAS` — três marcadores | `ferias/management/commands/pautas_de_ferias.py:164` | `ferias/management/commands/pautas_de_ferias.py:152` |
| `PRE_FECHAMENTO` | `folha/management/commands/pre_fechamento.py:111` | `folha/management/commands/pre_fechamento.py:92`, `:98` |
| `DIA_EM_ABERTO` | `pautas/management/commands/pauta_dia_em_aberto.py:140` | `pautas/management/commands/pauta_dia_em_aberto.py:108` |
| `FOLGA_EXPORTADA` | `pautas/management/commands/pauta_folga_nao_definida_exportada.py:135` | `pautas/management/commands/pauta_folga_nao_definida_exportada.py:126` |
| `PAGO_DEVIDO_EXPORTADA` | `pautas/management/commands/pauta_pago_x_devido_exportada.py:208`, `:212` | `pautas/management/commands/pauta_pago_x_devido_exportada.py:243`, `:250` |
| `REVISAO_AUSENCIA` | `ponto/services/vigia_ausencia.py:239` → `pautas/semanal.py:278` | `pautas/semanal.py:199`, `:228` |
| `REVISAO_SUPERVISAO` | `escala/services/fase_12x36.py:278` → `pautas/semanal.py:278` | `pautas/semanal.py:199`, `:228` |
| `TEXTO_AFASTADO` / `AFASTADO` | `ponto/services/afastado_avisa.py:76` | `ponto/services/afastado_avisa.py:63` |
| `SEM_DOCUMENTO` | `ponto/services/ausencia.py:1060` | `ponto/services/ausencia.py:1044` |
| `FERIAS_RECUSADA` | `ponto/services/ausencia.py:1250` | `ponto/services/ausencia.py:1234` |
| `HE_EXPORTADA` | `ponto/services/autorizacao_he_periodo.py:245` | `ponto/services/autorizacao_he_periodo.py:242` |
| `COBERTURA_A_CONFIRMAR` | `ponto/services/cobertura.py:336` | `ponto/services/cobertura.py:243` |
| `ESMERIL` | `ponto/services/pautas_do_esmeril.py:79` | `ponto/services/pautas_do_esmeril.py:66`, `:67` |
| `APP_401` | `ponto/services/pautas_dos_deslogados.py:78` | `ponto/services/pautas_dos_deslogados.py:66` |
| `DISCORDANTE` | `ponto/services/pautas_dos_discordantes.py:180` | `ponto/services/pautas_dos_discordantes.py:98`, `:166` |

Completam os arquivos tocados: catálogo, service e views citados acima; [teste da porta](/home/ronald/wt-codex-pauta-sem-turno-prefixo-reservado-003109/app/pautas/tests/test_prefixos_reservados.py:73), [selo AST](/home/ronald/wt-codex-pauta-sem-turno-prefixo-reservado-003109/app/pautas/tests/test_prefixos_reservados_ast.py:167) e [executor de RED/mutantes](/home/ronald/wt-codex-pauta-sem-turno-prefixo-reservado-003109/app/pautas/tests/mutar_prefixos_reservados.py:85). [nao verificado]

**RED observado na base**, usando os corpos reais das funções com HTTP/persistência substituídos; não é prova de contagem em banco (`pautas/tests/test_prefixos_reservados.py:1`):

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m pautas.tests.mutar_prefixos_reservados \
  --base a4b809999338e30088fc00d8e885d90c1e40bcb0
```

```text
testes=3 falhas=104 erros=0 [nao verificado]
AssertionError: 200 != 400
```

**Depois**, os testes isolados e AST terminaram com `Ran 12 tests — OK (skipped=1)`; o skip declara a integração Django indisponível (`pautas/tests/test_prefixos_reservados.py:143`, `pautas/tests/test_prefixos_reservados_ast.py:167`):

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest \
  pautas.tests.test_prefixos_reservados \
  pautas.tests.test_prefixos_reservados_ast -v
```

**Mutantes:** saída observada `59 mortos; 0 sobreviventes`. Cobrem sete alterações de comportamento, retirada individual dos 26 reservados e troca individual dos 26 leitores; o executor imprime o comando de cada mutante (`pautas/tests/mutar_prefixos_reservados.py:37`, `:139`, `:147`):

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m pautas.tests.mutar_prefixos_reservados

# Exemplo individual:
PYTHONDONTWRITEBYTECODE=1 python3 -m pautas.tests.mutar_prefixos_reservados \
  --mutante sem_strip
```

Ruff nos 27 arquivos tocados devolveu `All checks passed!`, com as regras de `ruff.toml:5`; `git diff --check` também terminou sem saída.

**Não rodou:** testes Django novos, `pautas.tests` inteiro e os testes dos donos abaixo. O comando tentou iniciar, mas parou em `ModuleNotFoundError: No module named 'django'`, antes da descoberta (`manage.py:11`). Integração HTTP, contagem real de Pauta e suítes dos donos permanecem **[nao verificado]**; os testes de POST/400/zero Pauta e texto comum/200 estão em `pautas/tests/test_prefixos_reservados.py:173`.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  pautas.tests \
  chamados.tests.test_competencia_trancada \
  chamados.tests.test_juiz_pauta_cadastro_sem_turno \
  chamados.tests.test_reabrir_orfas_enterradas \
  chamados.tests.test_o317_passivo_pela_porta \
  core.tests.test_crons_quebrados \
  core.tests.test_contract_direcao_a16 \
  core.tests.test_arvore_vermelha \
  core.tests.test_esteira_sem_humano \
  ferias.test_a3_situacao_de_ferias \
  folha.tests.test_pre_fechamento \
  ponto.tests.test_afastado_nunca_bloqueia \
  ponto.tests.test_porta_ausencia_ferias \
  ponto.tests.test_aus_f6_vencidas_pelo_juiz \
  ponto.tests.test_pautas_do_esmeril \
  ponto.tests.test_lista_viva_dos_deslogados \
  ponto.tests.test_o320_pauta_dos_discordantes \
  ponto.tests.test_a6_vigia_familia_ausencia \
  ponto.tests.test_declarar_cobertura_passada \
  ponto.tests.test_o214_item2_autorizar_periodo \
  escala.tests.test_fase_12x36 \
  --settings=config.settings_test --noinput
```

Para pautas históricas, o censo permite levantar candidatos pelos prefixos da tabela. Atribuir essas pautas a humanos apenas por `autor_user` não basta: o sistema pode assinar com superuser (`pautas/services.py:103`). Existência, quantidade e autoria humana em produção: **[nao verificado]**.

## 10/11 00:35 · sessao 12a663ea2904 · FIO-E0-V4-VIGIA-FABRICA-POR-EVENTO-CRESCE · sonda · origem=Code

Segue a **saída integral fornecida no enunciado**. Nova execução nesta sessão: [nao verificado]. O wrapper executa a sonda antes de chamar o Codex e incorpora a saída raspada ao prompt (`bin/codex_pergunta.sh:311–319`, `:337–344`).

```text
sonda_leitura: sonda4.py [7da39188eea4c4ff] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

agora_local 2026-11-10T00:32:57.861744-03:00 janela 2026-10-21 2026-11-20 N_CELS 48
col144 2026-11-05 SEM_ADESAO dono=ESTRUTURA
col144 2026-11-07 SEM_ADESAO dono=ESTRUTURA
col155 2026-10-26 COM_ADESAO dono=CADASTRO
col209 2026-11-05 SEM_ADESAO dono=ESTRUTURA
col209 2026-11-07 SEM_ADESAO dono=ESTRUTURA
col209 2026-11-09 SEM_ADESAO dono=ESTRUTURA
col268 2026-11-05 SEM_ADESAO dono=ESTRUTURA
col268 2026-11-07 SEM_ADESAO dono=ESTRUTURA
col268 2026-11-09 SEM_ADESAO dono=ESTRUTURA
col298 2026-11-05 SEM_ADESAO dono=CADASTRO
col298 2026-11-06 SEM_ADESAO dono=CADASTRO
col298 2026-11-07 SEM_ADESAO dono=CADASTRO
col298 2026-11-08 SEM_ADESAO dono=CADASTRO
col298 2026-11-09 SEM_ADESAO dono=CADASTRO
col312 2026-11-05 SEM_ADESAO dono=ESTRUTURA
col312 2026-11-07 SEM_ADESAO dono=ESTRUTURA
col427 2026-11-05 SEM_ADESAO dono=ESTRUTURA
col427 2026-11-07 SEM_ADESAO dono=ESTRUTURA
col534 2026-11-05 SEM_ADESAO dono=CADASTRO
col534 2026-11-06 SEM_ADESAO dono=CADASTRO
col534 2026-11-07 SEM_ADESAO dono=CADASTRO
col534 2026-11-08 SEM_ADESAO dono=CADASTRO
col534 2026-11-09 SEM_ADESAO dono=CADASTRO
col650 2026-11-06 SEM_ADESAO dono=ESTRUTURA
col650 2026-11-08 SEM_ADESAO dono=ESTRUTURA
col982 2026-10-21 COM_ADESAO dono=CADASTRO
col1002 2026-11-05 COM_ADESAO dono=ESTRUTURA
col1002 2026-11-07 COM_ADESAO dono=ESTRUTURA
col1005 2026-11-05 SEM_ADESAO dono=CADASTRO
col1005 2026-11-06 SEM_ADESAO dono=CADASTRO
col1005 2026-11-07 SEM_ADESAO dono=CADASTRO
col1005 2026-11-08 SEM_ADESAO dono=CADASTRO
col1005 2026-11-09 SEM_ADESAO dono=CADASTRO
col1009 2026-11-07 SEM_ADESAO dono=ESTRUTURA
col1009 2026-11-09 SEM_ADESAO dono=ESTRUTURA
col1016 2026-11-05 SEM_ADESAO dono=ESTRUTURA
col1016 2026-11-07 SEM_ADESAO dono=ESTRUTURA
col1018 2026-11-05 SEM_ADESAO dono=ESTRUTURA
col1018 2026-11-06 SEM_ADESAO dono=ESTRUTURA
col1018 2026-11-07 SEM_ADESAO dono=ESTRUTURA
col1018 2026-11-09 SEM_ADESAO dono=ESTRUTURA
col1019 2026-11-05 SEM_ADESAO dono=ESTRUTURA
col1019 2026-11-07 SEM_ADESAO dono=ESTRUTURA
col1019 2026-11-09 SEM_ADESAO dono=ESTRUTURA
col1027 2026-11-04 SEM_ADESAO dono=ESTRUTURA
col1027 2026-11-06 SEM_ADESAO dono=ESTRUTURA
col1027 2026-11-08 SEM_ADESAO dono=ESTRUTURA
col1030 2026-11-09 SEM_ADESAO dono=ESTRUTURA
TABELA (ramo, dono) {('SEM_ADESAO', 'ESTRUTURA'): 29, ('COM_ADESAO', 'CADASTRO'): 2, ('SEM_ADESAO', 'CADASTRO'): 15, ('COM_ADESAO', 'ESTRUTURA'): 2}
ERROS 0
```

1. **Pela lei, sim: decide o juiz de dono**, ressalvada a competência exportada, que nunca vai ao app. A OS-19 também retira expressamente o atalho de agrupamento sob ADESAO/LIMBO como decisor de dono ([CORTES.md:326](/home/ronald/codex_ws/7b0dbff79fcf59c045dd89046e5576769f786624/app/docs/CORTES.md:326)). O juiz indicado pela L-099 é `dono_da_divergencia`; seus argumentos não incluem adesão (`app/docs/LEIS.md:122`; `app/ponto/management/commands/e6_oraculo.py:53–54`, `:98–110`).

   **Não encontrei, no caminho fábrica → app examinado, um sítio que leia esse dono para escolher app x lista.** `motivos_a_perguntar` não o consulta: usa dia sem trabalho, situação, data, teto, veredito, parcial na lista e deduplicação ([fabrica_da_celula.py:87–150](/home/ronald/codex_ws/7b0dbff79fcf59c045dd89046e5576769f786624/app/chamados/services/fabrica_da_celula.py:87)). A API lê `consulta_do_app`, cujo universo vem das perguntas vivas, sem leitura do dono L-099 (`app/api/views.py:2560–2562`; `app/chamados/services/pergunta_no_app.py:44–74`).

   Há leitores desse dono na **lista**: C9–C12 apresentam o dono e escolhem a ação por ele; `_porta_pelo_dono` distingue BATIDA, CADASTRO e ESTRUTURA (`app/escala/services/cadastro_realidade.py:906–928`, `:745–759`). O vigia também lê dono e presença do dia nessa lista para classificá-lo como `fio_do_admin`; isso classifica o contador, sem emitir pergunta (`app/chamados/services/vigia_fio.py:211–228`, `:313–314`).

2. **A entrada responde outra pergunta; o efeito também veta a pergunta no evento.** `adesao is None` significa ausência de primeira batida não retratada na vida, apurada em `app/ponto/services/cartorio.py:1165–1173`. O ramo lavra o veredito com `codigos=[]`, carimba e retorna (`app/ponto/services/cartorio.py:1253–1279`).

   Portanto, ele não é uma segunda implementação do juiz de **dono**, mas funciona como uma barreira independente à mesma saída “gerar pergunta”: não preenche `out['cods']`, e o wrapper só chama a fábrica para datas presentes nesse mapa. O ramo normal preenche o mapa (`app/ponto/services/cartorio.py:996–1008`, `:1437`). Dizer apenas “responde outra pergunta” esconderia esse efeito; chamar diretamente `motivos_a_perguntar` não reproduz essa barreira do cartório (`app/chamados/services/fabrica_da_celula.py:100–150`).

3. **Sim, existe na cópia a lista Cadastro x Realidade, assinatura C5, para “nunca bateu”.** Ela lê células lavradas com `veredito='nunca_bateu'`, desde a competência anterior até ontem; ativos entram sem a exigência de retrato usada para desligados (`app/escala/services/cadastro_realidade.py:175–200`). Essa leitura vira C5 e é incorporada à lista (`app/escala/services/cadastro_realidade.py:204–209`, `:876–879`).

   A ação exibida é “Fazer a adesao ao app ou lancar a ausencia do dia”, e há uma view que serve a lista ao usuário autorizado (`app/escala/services/cadastro_realidade.py:690–692`; [views.py:475–492](/home/ronald/codex_ws/7b0dbff79fcf59c045dd89046e5576769f786624/app/escala/views.py:475)).

A presença efetiva de cada célula fornecida no app ou nessa lista, seu veredito lavrado e sua condição de competência exportada são [nao verificado]: esses campos não aparecem no dado recebido.

## 10/11 00:34 · sessao 665dd54f0f38 · - · - · origem=Ronald (tmux)

- **Pela lei, sim, com a ressalva da exportação.** `app/docs/CORTES.md:326` determina: competência exportada nunca vai ao app; nas demais, decide o juiz de dono L-099. A mesma linha retira expressamente o atalho “agrupado sob ADESAO/LIMBO”: agrupamento não decide dono.
- A L-099 identifica o juiz em `app/docs/LEIS.md:122`: `dono_da_divergencia`, implementado em `app/ponto/management/commands/e6_oraculo.py:53`. Os retornos efetivos estão em `:98–110`; a montagem compartilhada o chama em `:182–195`. Ausência de adesão não é argumento desse juiz (`:53–54`).
- **Não identifiquei um sítio no fluxo da fábrica/app que leia esse dono para rotear genericamente SEM_ADESAO entre app e lista.** O endpoint usa `consulta_do_app` em `app/api/views.py:2560–2562`; a consulta separa pendentes/histórico por resposta e fechamento em `app/chamados/services/pergunta_no_app.py:69–74`. O universo vem de `q_vivas` e `pergunta_viva` em `:48–57`, sem consulta ao dono L-099. A fábrica `motivos_a_perguntar` também não o lê: corpo completo em `app/chamados/services/fabrica_da_celula.py:87–150`.
- **Há leitores de dono em produção, respondendo outras perguntas.** A lista Cadastro x Realidade apresenta os donos C9–C12 e escolhe as ações em `app/escala/services/cadastro_realidade.py:906–928`; `_porta_pelo_dono` envia BATIDA à regularização, CADASTRO à porta cadastral, ESTRUTURA ao sistema em `:745–759`. A view serve essa lista em `app/escala/views.py:484–492`.
- O vigia lê a lista e o dono lavrado em `app/chamados/services/vigia_fio.py:211–228`; quando o dia já está na lista com dono BATIDA/CADASTRO, classifica como `fio_do_admin` em `:313–314`. Isso classifica o contador; não é roteamento de emissão ao app (`:292–295`).
- Consequência sustentada pela cópia: a lei exige dono como autoridade, mas os fluxos de criação/consulta examinados não o usam nessa decisão (`CORTES.md:326`; `fabrica_da_celula.py:87–150`; `pergunta_no_app.py:44–74`). Presença efetiva dos 44 SEM_ADESAO na lista ou no app, e cobertura por competência exportada: **[nao verificado]** pelo dado fornecido.

## 10/11 00:34 · sessao 8b341e94a3fb · - · - · origem=Ronald (tmux)

(2) A entrada do ramo responde ao fato “há primeira batida não retratada na vida?”: consulta `Batida` sem retratação e define `adesao` pela primeira (`app/ponto/services/cartorio.py:1165–1167`). Porém o efeito não é só classificatório: ele lavra a célula com `codigos=[]` (`app/ponto/services/cartorio.py:1253`), carimba (`app/ponto/services/cartorio.py:1275`) e retorna antes dos emissores normais (`app/ponto/services/cartorio.py:1279`).

No evento, isso também barra a fábrica: `julgar_colab` só chama `perguntar_pela_celula` para datas presentes em `out['cods']` (`app/ponto/services/cartorio.py:996–1008`); o ramo sem adesão não registra essas datas, enquanto o ramo normal as registra (`app/ponto/services/cartorio.py:1437`). Portanto, a premissa é outra (adesão), mas há uma decisão independente com o mesmo efeito “não gerar pergunta no app”; não seria correto alegar que não interfere nessa pergunta. A própria documentação do ramo diz “carimba (registro) e nao emite” (`app/ponto/services/cartorio.py:1168–1173`).

`motivos_a_perguntar` não consulta adesão; suas guardas e retorno estão em `app/chamados/services/fabrica_da_celula.py:100–150`. Assim, chamá-la diretamente não equivale ao caminho do evento: a barreira sem adesão fica no cartório, antes da chamada.

(3) **Sim, existe na cópia a lista Cadastro x Realidade, assinatura C5.**

- `colabs_nunca_bateu` lê células com `veredito='nunca_bateu'`, desde o início da competência anterior até ontem (`app/escala/services/cadastro_realidade.py:175–183`); para ativos não exige confirmação no retrato da folha, que só entra para desligados (`app/escala/services/cadastro_realidade.py:185–200`).
- `_nunca_bateu` transforma isso na assinatura C5 (`app/escala/services/cadastro_realidade.py:204–209`), inserida na lista (`app/escala/services/cadastro_realidade.py:876–879`).
- C5 é rotulada como adesão, lavra do cartório (`app/escala/services/cadastro_realidade.py:78–79`), e aponta à ação “Fazer a adesao ao app ou lancar a ausencia do dia” (`app/escala/services/cadastro_realidade.py:690–692`).
- Há rota real (`app/escala/urls.py:14`), view com autorização `ver_escala` que chama a lista (`app/escala/views.py:475–492`) e template que mostra cada assinatura/dono/ação (`app/templates/escala/cadastro_x_realidade.html:28`).

A inclusão efetiva das células do dado raspado nessa tela hoje é **[nao verificado]**, pois o dado não traz veredito lavrado, resultado da lista nem renderização.

Existe também chamado de adesão para ativo com célula `nunca_bateu` (`app/chamados/services/adesao.py:32–40`), aberto na supervisão/cadastro (`app/chamados/services/adesao.py:74–86`), mas isso não substitui a evidência mais direta da lista C5.

## 10/11 00:33 · sessao 66d82483ec3b · - · - · origem=Ronald (tmux)

Achados na base, sem edições:

- Os quatro prefixos novos são identidades reais. Afastado procura `TEXTO[:40]` e escreve `TEXTO` (`ponto/services/afastado_avisa.py:65`, `:78`); ausência sem documento compartilha `marcador` entre busca e escrita (`ponto/services/ausencia.py:1038`, `:1041`, `:1060`); férias recusadas faz o mesmo com marcador por agendamento (`:1228`, `:1231`, `:1250`); esmeril escreve `[ESMERIL %s]` e lê `[ESMERIL ` (`ponto/services/pautas_do_esmeril.py:14`, `:49`, `:64`).

- O prefixo de afastado é `Bateu ponto durante afastamento. A batid`. Reservá-lo bloqueia qualquer texto humano iniciado nessa prosa; ocorrência legítima em produção é [nao verificado]. A reserva é necessária para cumprir a classe estabelecida, pois esse trecho controla a deduplicação (`ponto/services/afastado_avisa.py:62`).

- Os leitores preservam prefixos exatos, sem `strip` ou `casefold`; a normalização deve ficar apenas na guarda humana. O escritor comum já aplica `strip` (`pautas/services.py:163`), e os leitores usam diretamente os marcadores (`ponto/services/afastado_avisa.py:65`, `ponto/services/ausencia.py:1041`, `:1231`, `ponto/services/pautas_do_esmeril.py:64`).

- O selo AST precisa acompanhar os parâmetros de `semanal.py`: a busca semanal usa marcador mais semana (`pautas/semanal.py:199`), enquanto o fechamento usa marcador puro (`:228`). Os dois produtores reais passam `[A6/REVISAO-SEMANAL]` e `[SUP/REVISAO-SEMANAL]` (`ponto/services/vigia_ausencia.py:28`, `:239`; `escala/services/fase_12x36.py:237`, `:278`).

- Há bypass por respostas humanas: `views.responder` recebe texto livre (`pautas/views.py:112`) e `services.responder` grava uma Pauta filha (`pautas/services.py:194`). Muitos leitores não excluem filhas: semanal (`pautas/semanal.py:197`, `:226`), ausência (`ponto/services/ausencia.py:1039`, `:1229`), esmeril (`ponto/services/pautas_do_esmeril.py:63`) e TRANCA (`chamados/services/competencia_trancada.py:218`). `vivas()` também não exclui filhas (`pautas/models.py:57`).

- Existe leitor deliberado de filhas marcadas: `pauta_pago_x_devido_exportada` compara respostas iniciadas pela chave do sistema (`pautas/management/commands/pauta_pago_x_devido_exportada.py:248`). Portanto, acrescentar indiscriminadamente `pai__isnull=True` não resolve a classe; recomendo chamar a mesma guarda humana também em `views.responder`, preservando o escritor de sistema (`pautas/views.py:112`, `pautas/services.py:194`).

Testes relacionados: `ponto/tests/test_afastado_nunca_bloqueia.py:67`, `ponto/tests/test_porta_ausencia_ferias.py:83`, `ponto/tests/test_pautas_do_esmeril.py:27`, `ponto/tests/test_a6_vigia_familia_ausencia.py:166`, `escala/tests/test_fase_12x36.py:136`. Execução desses testes: [nao verificado].

## 10/11 00:32 · sessao e9da70ea9d32 · FIO-E0-V4-VIGIA-FABRICA-POR-EVENTO-CRESCE · sonda · origem=Code

O modo `--sonda` executa o envelope antes de chamar o agente e incorpora a saída raspada ao prompt; abaixo reproduzo integralmente o dado recebido, sem alegar nova execução. (`bin/codex_pergunta.sh:307-344`) Conteúdo de produção sem verificação independente: [nao verificado].

```text
sonda_leitura: sonda3.py [0d4b1316cd662228] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

col534 e2 situacao=ativo adm=2026-02-17 dem=None adesao=None vivas60=0 retratadas=0 ramo=SEM_ADESAO
col1005 e2 situacao=ativo adm=2026-10-27 dem=None adesao=None vivas60=0 retratadas=0 ramo=SEM_ADESAO
col268 e2 situacao=ativo adm=2024-08-21 dem=None adesao=None vivas60=0 retratadas=0 ramo=SEM_ADESAO
col209 e2 situacao=ativo adm=2018-06-16 dem=None adesao=None vivas60=0 retratadas=0 ramo=SEM_ADESAO
col298 e2 situacao=ativo adm=2024-11-28 dem=None adesao=None vivas60=0 retratadas=0 ramo=SEM_ADESAO
col650 e2 situacao=ativo adm=2025-09-26 dem=None adesao=None vivas60=0 retratadas=0 ramo=SEM_ADESAO
col427 e2 situacao=ativo adm=2025-10-14 dem=None adesao=None vivas60=0 retratadas=0 ramo=SEM_ADESAO
col1018 e2 situacao=ativo adm=2026-11-04 dem=None adesao=None vivas60=0 retratadas=0 ramo=SEM_ADESAO
col1002 e2 situacao=ativo adm=2026-10-26 dem=None adesao=2026-11-09 vivas60=2 retratadas=0 ramo=COM_ADESAO
col1027 e2 situacao=ativo adm=2026-11-04 dem=None adesao=None vivas60=0 retratadas=0 ramo=SEM_ADESAO
col1009 e2 situacao=ativo adm=2026-11-01 dem=None adesao=None vivas60=0 retratadas=0 ramo=SEM_ADESAO
col1019 e2 situacao=ativo adm=2026-11-03 dem=None adesao=None vivas60=0 retratadas=0 ramo=SEM_ADESAO
col312 e2 situacao=ativo adm=2025-02-13 dem=None adesao=None vivas60=0 retratadas=0 ramo=SEM_ADESAO
col144 e3 situacao=ativo adm=2023-04-19 dem=None adesao=None vivas60=0 retratadas=0 ramo=SEM_ADESAO
col1016 e2 situacao=ativo adm=2026-11-01 dem=None adesao=None vivas60=0 retratadas=0 ramo=SEM_ADESAO
col1030 e2 situacao=ativo adm=2026-11-09 dem=None adesao=None vivas60=0 retratadas=0 ramo=SEM_ADESAO
col982 e2 situacao=ativo adm=2026-10-07 dem=None adesao=2026-10-20 vivas60=20 retratadas=0 ramo=COM_ADESAO
col155 e3 situacao=ativo adm=2022-09-29 dem=None adesao=2026-05-25 vivas60=97 retratadas=0 ramo=COM_ADESAO
RAMO {'SEM_ADESAO': 15, 'COM_ADESAO': 3}
ERROS 0
== anexo: sonda1_resumo.txt ==
agora_local 2026-11-10T00:12:27.212274-03:00
janela 2026-10-21 2026-11-20 piso 2026-10-21
N_ITENS 139 CELULAS 48
POR_MOTIVO {'saida_sem_entrada': 45, 'orfao_14h': 48, 'intervalo_saida': 23, 'intervalo_volta': 23}
POR_DIA {'2026-10-21': 1, '2026-10-26': 1, '2026-11-04': 2, '2026-11-05': 35, '2026-11-06': 18, '2026-11-07': 38, '2026-11-08': 16, '2026-11-09': 28}
POR_EMPRESA {2: 134, 3: 5}
A 0 B 0 sem_evento_pos_dia 135 sem_evento_nenhum 0 com_evento_pendente 0
COMBO(A,B,ev_pos>0) {(False, False, False): 135, (False, False, True): 4}
TRABALHA {'True': 139} VEREDITO {'nunca_bateu': 137, 'indefinida': 1, 'cobrado': 1}
PERG_MESMO_DIA_QUALQUER>0 0
ERROS 0
 col1005 e2 2026-11-05 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 09:00:09 ultev=11-05 09:00:24
 col1005 e2 2026-11-05 intervalo_saida trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 09:00:09 ultev=11-05 09:00:24
 col1005 e2 2026-11-05 intervalo_volta trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 09:00:09 ultev=11-05 09:00:24
 col1005 e2 2026-11-05 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 09:00:09 ultev=11-05 09:00:24
 col1005 e2 2026-11-06 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-06 09:00:05 ultev=11-06 09:00:23
 col1005 e2 2026-11-06 intervalo_saida trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-06 09:00:05 ultev=11-06 09:00:23
 col1005 e2 2026-11-06 intervalo_volta trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-06 09:00:05 ultev=11-06 09:00:23
 col1005 e2 2026-11-06 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-06 09:00:05 ultev=11-06 09:00:23
```

O material recebido registra `ERROS 0` nas duas saídas e três `COM_ADESAO`: col1002, col982 e col155. [nao verificado]

1. **COM_ADESAO: sim, o ramo normal rejulgaria após cada novo vencimento, se fosse chamado para aquela célula.** A adesão vem da primeira batida não retratada da vida, independentemente de haver batida naquele dia. (`app/ponto/services/cartorio.py:1165-1173`) Com `teto_de_hoje`, os marcos vencidos são calculados para hoje e ontem; sem nenhum vencido, a célula sai como `cedo`. (`app/ponto/services/cartorio.py:1324-1344`) A quantidade de vencidos entra na impressão, portanto um novo marco vencido muda esse insumo. (`app/ponto/services/cartorio.py:1359-1365`; `app/ponto/services/cartorio.py:463-464`) Além disso, `veredito == 'nunca_bateu'` torna `_em_dia=False` mesmo com impressão igual, impedindo o pulo em `:1373`. (`app/ponto/services/cartorio.py:1366-1375`) Essa guarda provoca rejuízo dentro de uma chamada; ela não agenda outra chamada. (`app/ponto/services/cartorio.py:1314-1378`)

2. **O caminho do julgamento até a fábrica depende da presença da data em `out['cods']`, inclusive quando os códigos são `[]`.** O wrapper retira os códigos acumulados do lote para isolar o ato atual e executa o corpo. (`app/ponto/services/cartorio.py:938-959`) O ramo normal classifica e grava `out.setdefault('cods', {})[data] = list(cods)`. (`app/ponto/services/cartorio.py:1424-1437`) Com `apply_=True`, lavra ata/veredito e carimba; depois, o wrapper lê `_julgadas = out.get('cods') or {}` e chama `perguntar_pela_celula` somente para datas presentes nesse dicionário. (`app/ponto/services/cartorio.py:1810-1835`; `app/ponto/services/cartorio.py:992-1008`) A fábrica calcula vencidos e motivos e, havendo motivos após as guardas, chama `perguntar`. (`app/chamados/services/fabrica_da_celula.py:190-210`)

3. **Por que os COM_ADESAO não emitiram os marcos seguintes em produção: [nao verificado].** O raspado não apresenta `out['cods']`, os resultados das chamadas por célula nem o cruzamento dos 139 itens com os três colaboradores desse ramo. [nao verificado] A ausência de evento posterior, isoladamente, não demonstra ausência de oportunidade de rejuízo: o vigia de hora também chama `julgar_celula(..., teto_de_hoje=True)` para candidatas que passam pelas suas guardas. (`app/ponto/services/vigia_de_hora.py:114-131`) Há ainda uma entrada independente de `cods`: o consumidor chama `perguntar_por_eventos`, que relê as células das datas do lote e chama `perguntar_pela_celula`. (`app/chamados/reconciliador.py:1384-1387`; `app/chamados/services/fabrica_da_celula.py:279-293`) Execução, candidatura e resultado desses caminhos para cada COM_ADESAO: [nao verificado].

4. **SEM_ADESAO: “carimba e não emite” corresponde ao encaminhamento do corpo do cartório.** Esse ramo tem sua própria idempotência; quando aplica, mantém `nunca_bateu` sem cobertura integral, lavra com `codigos=[]`, carimba e retorna em `:1279`. (`app/ponto/services/cartorio.py:1207-1216`; `app/ponto/services/cartorio.py:1243-1279`) Ele retorna antes da gravação de `out['cods']` em `:1437`; consequentemente, não encaminha essas datas à fábrica pelo laço do wrapper. (`app/ponto/services/cartorio.py:1279`; `app/ponto/services/cartorio.py:1437`; `app/ponto/services/cartorio.py:997-1008`) A guarda de rejuízo de `:1369` pertence ao ramo normal, alcançado quando existe adesão. (`app/ponto/services/cartorio.py:1173`; `app/ponto/services/cartorio.py:1279-1280`; `app/ponto/services/cartorio.py:1369`)

5. **Sim: fábrica e vigia podem contar motivos desses SEM_ADESAO como devidos; não há guarda de adesão nesse caminho.** `nunca_bateu` integra explicitamente `VEREDITOS_ACUSAM`. (`app/escala/models.py:1767-1768`) `motivos_a_perguntar` verifica dia sem trabalho, situação ativa, futuro, vencimento, veredito, parcial na lista e deduplicação; lê os faltantes pela ata, sem consultar adesão. (`app/chamados/services/fabrica_da_celula.py:100-150`; `app/chamados/services/fabrica_da_celula.py:52-74`) `motivos_que_o_evento_deixou` retorna esses motivos, tratando separadamente a pauta do limbo exportado. (`app/chamados/services/fabrica_da_celula.py:225-236`) O vigia do `disparar` acumula cada motivo retornado nas células passadas elegíveis, imprime a contagem e gera `SystemExit(2)` no caminho de alarme quando há pendências. (`app/chamados/management/commands/disparar_perguntas_competencia.py:100-113`; `app/chamados/management/commands/disparar_perguntas_competencia.py:239-259`)

Assim, há uma divergência demonstrável entre o ramo SEM_ADESAO, que retorna sem alimentar `cods`, e o leitor do vigia, que aceita seu veredito e pode encontrar marcos devidos na ata. (`app/ponto/services/cartorio.py:1253-1279`; `app/ponto/services/cartorio.py:997-1008`; `app/chamados/services/fabrica_da_celula.py:112-150,233-236`; `app/escala/models.py:1767-1768`) Quantificar essa divergência em todos os 139 itens ou atribuir a omissão concreta dos COM_ADESAO permanece [nao verificado].

## 10/11 00:30 · sessao 0f26bcd03582 · - · - · origem=Ronald (tmux)

- **SEM_ADESAO pode entrar como devido no vigia.** `motivos_que_o_evento_deixou` chama `motivos_a_perguntar` e retorna os motivos restantes, com desvio apenas para pauta do limbo exportado (`app/chamados/services/fabrica_da_celula.py:225-236`). `nunca_bateu` integra `VEREDITOS_ACUSAM` (`app/escala/models.py:1767-1768`).

- **Não há guarda de adesão nesse caminho.** As guardas são dia sem trabalho/ativo, futuro, marcos vencidos, veredito, parcial e deduplicação (`app/chamados/services/fabrica_da_celula.py:102-150`). O teto consulta escala e vencimento por instante, com tolerância de 30 minutos (`app/ponto/services/cartorio.py:828-859`). A leitura dos faltantes usa ata, isenção, vínculo por admissão/demissão e tipo do dia; não consulta primeira batida/adesão (`app/chamados/services/declaracao_texto.py:100-143`; `app/escala/services/leitor_celula.py:365-398`).

- **O vigia do disparar contabiliza esses motivos.** Seleciona passado, exclui ontem enquanto ainda estiver na janela do vigia de hora, chama `motivos_que_o_evento_deixou` e acrescenta cada motivo em `deixou` (`app/chamados/management/commands/disparar_perguntas_competencia.py:100-113`). Imprime o total e, no caminho de alarme, lança `SystemExit(2)` se houver pendências (`app/chamados/management/commands/disparar_perguntas_competencia.py:240-259`).

- **A divergência com “carimba e não emite” está no encaminhamento.** O ramo sem adesão lavra `codigos=[]`, carimba e retorna do corpo (`app/ponto/services/cartorio.py:1253-1279`). O wrapper encaminha à fábrica apenas datas presentes em `out['cods']` (`app/ponto/services/cartorio.py:992-1008`). Assim, o corpo pode deixar ata/veredito que o leitor do vigia considera cobravel sem colocar a data no conjunto encaminhado à fábrica (`app/ponto/services/cartorio.py:1253-1279`; `app/chamados/services/fabrica_da_celula.py:112-150`).

- Os quatro motivos vêm das posições dos marcos: início/E → `saida_sem_entrada`; fim/S → `orfao_14h`; intervalo ida/S → `intervalo_saida`; intervalo volta/E → `intervalo_volta` (`app/chamados/services/declaracao_texto.py:42-46`). A fábrica limita os faltantes aos marcos vencidos (`app/chamados/services/fabrica_da_celula.py:58-74`).

O raspado mostra oito itens exemplificados para `col1005`, identificado como SEM_ADESAO, e agregados gerais; não fornece o cruzamento completo de todos os 139 itens com os dois ramos, nem o histórico de execução de cada célula. [nao verificado]

## 10/11 00:30 · sessao e5762b0ed2d9 · - · - · origem=Ronald (tmux)

1. **COM_ADESAO: sim, se o julgamento for chamado.** A adesão é a primeira batida não retratada (`app/ponto/services/cartorio.py:1165-1167`); com adesão, o ramo sem adesão não é executado (`:1173`, `:1279`). No ramo normal, `nunca_bateu` torna `_em_dia=False` mesmo com impressão igual, impedindo o pulo por idempotência (`:1366-1375`). Com `teto_de_hoje=True`, hoje e ontem usam marcos vencidos por instante; nenhum vencido implica `continue` (`:1324-1344`). A quantidade de vencidos entra na impressão (`:1359-1365`; implementação em `:463-464`). Portanto, novo vencimento muda a impressão; e o antigo `nunca_bateu` já força rejuízo independentemente dessa mudança (`:1369`). Ainda há a saída `dia is None` (`:1376-1378`).

2. **Caminho exato até a fábrica no julgamento.** `julgar_colab` retira `cods` do lote para isolar o ato (`app/ponto/services/cartorio.py:938-946`), executa o corpo (`:959`) e eventual segunda passada (`:980-984`). O corpo normal calcula `cods` e registra a data **mesmo se a lista de códigos for vazia** (`:1424-1437`). Com `apply_=True`, o wrapper lê `_julgadas = out.get('cods') or {}` e chama `perguntar_pela_celula` somente para datas presentes nesse dicionário (`:992-1008`). A ata e o veredito são lavrados antes do retorno do corpo (`:1810-1834`). No `finally`, os códigos do ato voltam ao acumulado do lote (`:1077-1084`).

3. **SEM_ADESAO: o ramo não alimenta esse caminho.** Entra em `if adesao is None` (`app/ponto/services/cartorio.py:1173`); a impressão do ramo pode produzir pulo (`:1207-1216`). Se aplica, grava `nunca_bateu` sem cobertura integral e `codigos=[]` (`:1243-1253`), carimba (`:1275-1278`) e retorna (`:1279`), antes da única gravação de `out['cods'][data]` do corpo (`:1437`). Portanto, o wrapper não chama a fábrica para essas datas por esse ato (`:997-1008`). A guarda de `nunca_bateu` em `:1369` pertence ao **ramo normal**, alcançado depois de existir adesão, não ao ramo SEM_ADESAO (`:1173`, `:1279-1280`).

4. **Há outra entrada da fábrica, independente de `cods`.** O consumidor de eventos pendentes busca lotes (`app/chamados/reconciliador.py:1343-1352`) e chama `perguntar_por_eventos` (`:1384-1387`); esta relê as células atuais das datas dos eventos e chama `perguntar_pela_celula` (`app/chamados/services/fabrica_da_celula.py:288-292`). Por isso, não cabe afirmar que SEM_ADESAO nunca pode receber pergunta: a exclusão demonstrada acima é a entrada por `cods` no julgamento. Falhas do consumidor impedem marcar os eventos como processados (`app/chamados/reconciliador.py:1388-1401`).

5. **O relógio pode chamar sem evento novo.** O código agenda `vigia_de_hora --apply` a cada dez minutos (`app/config/crons.py:365-368`). A rotina candidata células `trabalha=True` de hoje e ontem (`app/ponto/services/vigia_de_hora.py:20-23`), aplica exclusões (`:117-129`) e chama `julgar_celula(..., teto_de_hoje=True)` (`:130-131`). Portanto, “sem evento posterior” sozinho não explica por que COM_ADESAO deixou de perguntar; a execução efetiva, candidatura e resultado dessas rodadas em produção são **[nao verificado]**. Ontem sai do alcance conforme `_ontem_em_janela` (`:47-58`, `:120-121`). O batch do cartório também chama `julgar_colab` para células passadas (`app/ponto/management/commands/processar_cartorio.py:49-65`).

6. **Limite do dado raspado.** A saída fornecida não contém `out['cods']`, instantes dos vencimentos, resultado das rodadas do vigia, nem cruzamento individual entre COM_ADESAO e cada uma das 139 faltas; logo, a causa concreta da omissão dos marcos seguintes para cada COM_ADESAO permanece **[nao verificado]**. O código demonstra a possibilidade e o caminho de rejulgamento, mas não permite atribuir o caso de produção a pulo por impressão, falta de rodada, guarda da fábrica ou falha do consumidor sem essas observações **[nao verificado]**.

## 10/11 00:29 · sessao 97b44640d8a7 · FIO-E0-V4-VIGIA-FABRICA-POR-EVENTO-CRESCE · sonda · origem=Code

**ERROS 18: a sonda não permite concluir a causa em produção nem validar a divisão COM_ADESAO/SEM_ADESAO.** O bloco recebido informa esse resultado; não houve nova execução nesta sessão. [nao verificado]

O `--sonda` é executado pelo wrapper **antes** de chamar esta sessão; depois, a saída raspada entra no prompt. Abaixo devolvo integralmente o bloco recebido, incluindo o anexo, sem apresentá-lo como nova execução. (`bin/codex_pergunta.sh:311-318`, `bin/codex_pergunta.sh:338-344`; conteúdo recebido: [nao verificado])

```text
sonda_leitura: sonda2.py [abace131b830b2ec] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

ERRO 534 AttributeError 'Colaborador' object has no attribute 'ativo'
ERRO 1005 AttributeError 'Colaborador' object has no attribute 'ativo'
ERRO 268 AttributeError 'Colaborador' object has no attribute 'ativo'
ERRO 209 AttributeError 'Colaborador' object has no attribute 'ativo'
ERRO 298 AttributeError 'Colaborador' object has no attribute 'ativo'
ERRO 650 AttributeError 'Colaborador' object has no attribute 'ativo'
ERRO 427 AttributeError 'Colaborador' object has no attribute 'ativo'
ERRO 1018 AttributeError 'Colaborador' object has no attribute 'ativo'
ERRO 1002 AttributeError 'Colaborador' object has no attribute 'ativo'
ERRO 1027 AttributeError 'Colaborador' object has no attribute 'ativo'
ERRO 1009 AttributeError 'Colaborador' object has no attribute 'ativo'
ERRO 1019 AttributeError 'Colaborador' object has no attribute 'ativo'
ERRO 312 AttributeError 'Colaborador' object has no attribute 'ativo'
ERRO 144 AttributeError 'Colaborador' object has no attribute 'ativo'
ERRO 1016 AttributeError 'Colaborador' object has no attribute 'ativo'
ERRO 1030 AttributeError 'Colaborador' object has no attribute 'ativo'
ERRO 982 AttributeError 'Colaborador' object has no attribute 'ativo'
ERRO 155 AttributeError 'Colaborador' object has no attribute 'ativo'
RAMO {'SEM_ADESAO': 15, 'COM_ADESAO': 3}
ERROS 18
VEREDITO: SONDA COM ERRO -- numero nao conclui
== anexo: sonda1_resumo.txt ==
agora_local 2026-11-10T00:12:27.212274-03:00
janela 2026-10-21 2026-11-20 piso 2026-10-21
N_ITENS 139 CELULAS 48
POR_MOTIVO {'saida_sem_entrada': 45, 'orfao_14h': 48, 'intervalo_saida': 23, 'intervalo_volta': 23}
POR_DIA {'2026-10-21': 1, '2026-10-26': 1, '2026-11-04': 2, '2026-11-05': 35, '2026-11-06': 18, '2026-11-07': 38, '2026-11-08': 16, '2026-11-09': 28}
POR_EMPRESA {2: 134, 3: 5}
A 0 B 0 sem_evento_pos_dia 135 sem_evento_nenhum 0 com_evento_pendente 0
COMBO(A,B,ev_pos>0) {(False, False, False): 135, (False, False, True): 4}
TRABALHA {'True': 139} VEREDITO {'nunca_bateu': 137, 'indefinida': 1, 'cobrado': 1}
PERG_MESMO_DIA_QUALQUER>0 0
ERROS 0
 col1005 e2 2026-11-05 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 09:00:09 ultev=11-05 09:00:24
 col1005 e2 2026-11-05 intervalo_saida trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 09:00:09 ultev=11-05 09:00:24
 col1005 e2 2026-11-05 intervalo_volta trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 09:00:09 ultev=11-05 09:00:24
 col1005 e2 2026-11-05 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 09:00:09 ultev=11-05 09:00:24
 col1005 e2 2026-11-06 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-06 09:00:05 ultev=11-06 09:00:23
 col1005 e2 2026-11-06 intervalo_saida trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-06 09:00:05 ultev=11-06 09:00:23
 col1005 e2 2026-11-06 intervalo_volta trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-06 09:00:05 ultev=11-06 09:00:23
 col1005 e2 2026-11-06 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-06 09:00:05 ultev=11-06 09:00:23
```

**COM_ADESAO: sim, condicionalmente à chamada do julgamento.** No código, adesão é a data da primeira batida não retratada; havendo essa batida, o ramo `adesao is None` não é tomado. (`app/ponto/services/cartorio.py:1165-1173`)

No ramo normal com `teto_de_hoje=True`:

- Hoje, sem marco vencido, a célula é pulada; havendo vencidos, sua quantidade entra na impressão. O vencimento considera marco + 30 minutos. (`app/ponto/services/cartorio.py:1324-1328`, `app/ponto/services/cartorio.py:1359-1361`, `app/ponto/services/cartorio.py:828`, `app/ponto/services/cartorio.py:858-859`)
- Cada aumento dessa quantidade muda a impressão, pois ela incorpora `teto`. Isso impede o pulo por impressão igual no vencimento seguinte. (`app/ponto/services/cartorio.py:463-464`, `app/ponto/services/cartorio.py:1369-1375`)
- Enquanto o veredito persistido for `nunca_bateu`, `_em_dia` será falso **mesmo com impressão igual**. Essa condição impede o pulo por idempotência; depois que o veredito mudar, deixa de fazê-lo. (`app/ponto/services/cartorio.py:1366-1375`)
- Isso não garante julgamento sem invocação, nem supera a ausência do dia na grade, que também causa `continue`. (`app/ponto/services/cartorio.py:1376-1378`)

Existe gatilho pelo relógio: a configuração agenda `vigia_de_hora --apply` a cada dez minutos, e o serviço chama `julgar_celula(..., teto_de_hoje=True)` nas candidatas que passam suas guardas. Portanto, **ausência de evento posterior, isoladamente, não explica a falta dos marcos seguintes nesse ramo**. (`app/config/crons.py:367`, `app/ponto/services/vigia_de_hora.py:114-131`)

O caminho até a fábrica é:

1. O julgamento normal calcula `cods` e grava `out['cods'][data] = list(cods)`, inclusive quando a lista está vazia. (`app/ponto/services/cartorio.py:1424-1437`)
2. O wrapper isola os códigos deste ato dos códigos anteriores do lote. (`app/ponto/services/cartorio.py:938-944`)
3. Com `apply_`, lê `_julgadas = out.get('cods') or {}` e chama `perguntar_pela_celula` para cada célula cuja **data esteja nesse dicionário**; não exige códigos não vazios naquela data. (`app/ponto/services/cartorio.py:992-1008`)
4. `perguntar_pela_celula` obtém os motivos; havendo motivos e sem encaminhamento à pauta, chama `perguntar`, que cria/atualiza a disputa e gera as perguntas. (`app/chamados/services/fabrica_da_celula.py:192-210`, `app/chamados/services/fabrica_da_celula.py:161-167`)

**Por que não emitiu nos COM_ADESAO observados? [nao verificado].** O dado recebido não demonstra qual chamada ocorreu, qual guarda impediu a passagem ou quais motivos retornaram em cada ato; com `ERROS 18`, não atribuo causa a esses casos. [nao verificado]

**SEM_ADESAO: há diferença demonstrável entre o caminho do cartório e a regra de contagem.** O comentário diz “carimba e nao emite”; esse ramo lavra/carimba e retorna sem preencher `out['cods']`. Assim, esse julgamento não habilita a chamada à fábrica pelo wrapper. (`app/ponto/services/cartorio.py:1168-1173`, `app/ponto/services/cartorio.py:1253-1279`, `app/ponto/services/cartorio.py:997-1008`)

Já a fábrica e o vigia **podem considerar devidos os motivos desses dias**, dependendo das demais guardas:

- `nunca_bateu` pertence expressamente a `VEREDITOS_ACUSAM`, portanto passa pelo filtro de veredito da fábrica. (`app/escala/models.py:1767-1768`, `app/chamados/services/fabrica_da_celula.py:113-114`)
- `motivos_a_perguntar` verifica dia sem trabalho, `situacao == 'ativo'`, futuro, vencimento, veredito, parcial na lista e pergunta já existente; **não verifica adesão**. A situação ativa é consultada por `situacao`, não por atributo `ativo`. (`app/chamados/services/fabrica_da_celula.py:102-150`)
- Os motivos precisam corresponder a marcos faltantes na ata e já vencidos; sem vencidos ou sem autorização/faltantes da ata, retorna lista vazia. (`app/chamados/services/fabrica_da_celula.py:58-74`)
- `motivos_que_o_evento_deixou` chama essa mesma função, com tratamento adicional da pauta do limbo, **sem acrescentar guarda de adesão**. (`app/chamados/services/fabrica_da_celula.py:225-236`)
- O vigia do `disparar` percorre células passadas da janela, exclui ontem enquanto ainda cabe ao vigia de hora e acrescenta cada motivo retornado a `deixou`, sem guarda de adesão nesse laço. Conta motivos e células distintas; esse caminho somente conta e alarma. (`app/chamados/management/commands/disparar_perguntas_competencia.py:100-113`, `app/chamados/management/commands/disparar_perguntas_competencia.py:240-251`)

Logo, o código permite **SEM_ADESAO carimbado sem fábrica pelo julgamento, mas com motivos contados pelo vigia** quando os demais filtros permitem. Isso demonstra a possibilidade estrutural; sua incidência e participação nos números recebidos permanecem [nao verificado]. (`app/ponto/services/cartorio.py:1279`, `app/ponto/services/cartorio.py:997-1008`, `app/chamados/services/fabrica_da_celula.py:102-150`, `app/chamados/services/fabrica_da_celula.py:233-236`)

## 10/11 00:28 · sessao b9ec0c8ed08e · - · - · origem=Ronald (tmux)

Donos com testes diretamente ligados aos marcadores, em labels Django: [nao verificado]

```text
chamados.tests.test_competencia_trancada
chamados.tests.test_reabrir_orfas_enterradas
chamados.tests.test_o317_passivo_pela_porta
chamados.tests.test_e4a_pauta_cadastro_contador
chamados.tests.test_juiz_pauta_cadastro_sem_turno
chamados.tests.test_e4_c20
chamados.tests.test_f1c4_3_emissores
core.tests.test_arvore_vermelha
core.tests.test_esteira_sem_humano
core.tests.test_crons_quebrados
ferias.test_a3_situacao_de_ferias
folha.tests.test_pre_fechamento
ponto.tests.test_o214_item4_dia_do_admin
ponto.tests.test_a6_vigia_familia_ausencia
escala.tests.test_fase_12x36
ponto.tests.test_afastado_nunca_bloqueia
ponto.tests.test_afastado_na_batida
ponto.tests.test_porta_ausencia_ferias
ponto.tests.test_a4_estados_com_porta
ponto.tests.test_o214_item2_autorizar_periodo
ponto.tests.test_declarar_cobertura_passada
ponto.tests.test_pautas_do_esmeril
ponto.tests.test_lista_viva_dos_deslogados
ponto.tests.test_o320_pauta_dos_discordantes
pautas.tests.test_pauta_dia_em_aberto_classe
pautas.tests.test_f3_beco1_pauta_exportada
pautas.tests.test_pauta_pago_x_devido_exportada
```

Referências diretas e pertinentes:

| Família | Teste | [nao verificado]
|---|---|
| TRANCA | `chamados/tests/test_competencia_trancada.py:158` |
| ÓRFÃS | `chamados/tests/test_reabrir_orfas_enterradas.py:234` |
| PASSIVO | `chamados/tests/test_o317_passivo_pela_porta.py:436` |
| CADASTRO | `chamados/tests/test_e4a_pauta_cadastro_contador.py:20`, `test_juiz_pauta_cadastro_sem_turno.py:16`, `test_e4_c20.py:99` |
| LIMBO | `chamados/tests/test_f1c4_3_emissores.py:190` |
| ÁRVORE | `core/tests/test_arvore_vermelha.py:32` |
| ESTEIRA | `core/tests/test_esteira_sem_humano.py:146` |
| CRON | `core/tests/test_crons_quebrados.py:56` |
| FÉRIAS | `ferias/test_a3_situacao_de_ferias.py:620` |
| PRÉ-FECHAMENTO | `folha/tests/test_pre_fechamento.py:81` |
| DIA | `pautas/tests/test_pauta_dia_em_aberto_classe.py:95` |
| FOLGA | `pautas/tests/test_f3_beco1_pauta_exportada.py:36` |
| PAGO | `pautas/tests/test_pauta_pago_x_devido_exportada.py:30` |
| A6 semanal | `ponto/tests/test_a6_vigia_familia_ausencia.py:173` |
| SUP semanal | `escala/tests/test_fase_12x36.py:151` |
| AFASTADO | `ponto/tests/test_afastado_nunca_bloqueia.py:84`, `test_afastado_na_batida.py:94` |
| A1 FÉRIAS | `ponto/tests/test_porta_ausencia_ferias.py:59` |
| A4 DOCUMENTO | `ponto/tests/test_a4_estados_com_porta.py:157` |
| HE | `ponto/tests/test_o214_item4_dia_do_admin.py:323`, `test_o214_item2_autorizar_periodo.py:169` |
| COBERTURA | `ponto/tests/test_declarar_cobertura_passada.py:93` |
| ESMERIL | `ponto/tests/test_pautas_do_esmeril.py:27` |
| APP | `ponto/tests/test_lista_viva_dos_deslogados.py:192` |
| DISCORDANTE | `ponto/tests/test_o320_pauta_dos_discordantes.py:59` |

O teste de férias fica fora de `*/tests/*`; o teste de cron acessa o dono por `call_command('cron_quebrado', ...)`, em `core/tests/test_crons_quebrados.py:56`. Logo ambos escapam de uma seleção feita só por nomes de constantes/imports.

Comando do grep das constantes específicas executado:

```bash
git grep -n -w \
  -e PAUTA_SEM_TURNO -e PAUTA_LIMBO_EXPORTADA \
  -e MARCADOR_FORA -e MARCADOR_PAUTA -e MARCADOR_A_CONFIRMAR \
  -- '*/tests/*'
```

Também executei busca por todos os literais dos marcadores e varredura AST de imports dos donos: esta última identificou 99 módulos dependentes, mas muitos exercitam funções do dono sem relação com a família reservada, especialmente `ponto.services.ausencia`. [nao verificado]

**Conflito concreto encontrado:** `chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:23` autoriza tocar `PAUTA_SEM_TURNO` apenas no seletor/escritor da fábrica. O selo captura import/nome/atributo/literal (`:115`) e reprova consumidores fora dessa lista (`:149`), no teste `:245`. A nova importação da constante por `pautas.services` precisa de ajuste estreito desse contrato e contraprova preservando a recusa de outro consumidor.

Nenhum arquivo alterado.

## 10/11 00:27 · sessao 6e2365d60a50 · - · - · origem=Ronald (tmux)

- O vigia do `disparar` conta os motivos devolvidos por `motivos_que_o_evento_deixou`, para células passadas na janela; ontem é excluído enquanto `_ontem_em_janela` o entrega ao vigia de hora. Não verifica adesão nesse laço. `app/chamados/management/commands/disparar_perguntas_competencia.py:100-113`.

- `motivos_que_o_evento_deixou` chama `motivos_a_perguntar`; para limbo em competência entregue, desconta motivos já pautados, e nos demais casos devolve os motivos devidos diretamente. `app/chamados/services/fabrica_da_celula.py:225-236`.

- `motivos_a_perguntar` verifica dia sem trabalho, `situacao == 'ativo'`, futuro, vencidos, veredito, parcial na lista e dedup pela pergunta do fato; não tem guarda de adesão. `app/chamados/services/fabrica_da_celula.py:100-150`. `dia_sem_trabalho` lê `trabalha`, tipo da ata e cobertura parcial, sem adesão. `app/chamados/juizes.py:426-441`.

- `nunca_bateu` consta de `VEREDITOS_ACUSAM`; portanto esse veredito passa pela guarda da fábrica. Um SEM_ADESAO pode ser contado como devido quando satisfaz os demais filtros e tem motivos faltantes vencidos na ata, ainda sem pergunta do fato. Isso explica a possibilidade no código, não estabelece população ou causa real de produção. `app/escala/models.py:1767-1768`; `app/chamados/services/fabrica_da_celula.py:105-118`; `app/chamados/services/fabrica_da_celula.py:132-150`.

- O contador usa `len(deixou)`, conta células distintas e declara que o lote não escreve essas perguntas; o alarme sai com código 2 se `deixou` não estiver vazio. `app/chamados/management/commands/disparar_perguntas_competencia.py:240-251`.

- O vigia de hora candidata células `trabalha=True`, exclui desligado/isento, ontem fora de janela, nenhum marco vencido e batidas suficientes sem cobrança viva; chama `julgar_celula(...teto_de_hoje=True)` nas restantes. Não verifica adesão nesse percurso. `app/ponto/services/vigia_de_hora.py:21-23`; `app/ponto/services/vigia_de_hora.py:114-131`.

- A própria documentação alerta: rodadas perdidas além da janela de ontem não são recuperadas pelo `disparar`, que apenas conta e alarma; a pergunta depende de novo julgamento ou sinal de batida. `app/ponto/services/vigia_de_hora.py:39-43`.

## 10/11 00:27 · sessao 6de4b1b32b1f · - · - · origem=Ronald (tmux)

O caminho do código demonstra:

- `julgar_colab` retira o `cods` anterior do lote, mantendo o resultado por ato (`app/ponto/services/cartorio.py:944`). A fábrica roda com `apply_` e chama `perguntar_pela_celula` somente para datas presentes no `out['cods']` desse ato (`app/ponto/services/cartorio.py:992`, `app/ponto/services/cartorio.py:997`, `app/ponto/services/cartorio.py:1006-1008`).
- O ramo normal registra toda célula efetivamente julgada em `out['cods'][data]`, inclusive lista vazia (`app/ponto/services/cartorio.py:1436-1437`). A seleção usa a presença da chave da data, não a presença de códigos na lista (`app/ponto/services/cartorio.py:1007`).
- `adesao` é a primeira batida não retratada; ausência dela seleciona o ramo `nunca_bateu` (`app/ponto/services/cartorio.py:1165-1173`). Esse ramo carimba e retorna antes do ramo normal, sem registrar `out['cods']` (`app/ponto/services/cartorio.py:1253-1279`); portanto não habilita a fábrica do wrapper por esse ato (`app/ponto/services/cartorio.py:997-1008`).
- `motivos_a_perguntar` guarda dia sem trabalho, situação diferente de `ativo`, futuro, veredito fora dos acusadores, parcial destinada à lista e existência da pergunta do fato; não consulta adesão (`app/chamados/services/fabrica_da_celula.py:102-150`). `nunca_bateu` integra expressamente `VEREDITOS_ACUSAM` (`app/escala/models.py:1767-1768`).
- Os motivos vêm dos marcos faltantes da ata e são recortados pelos vencidos por instante (`app/chamados/services/fabrica_da_celula.py:52-74`). O vencimento usa o mesmo `cartorio.marcos_vencidos`; nenhum marco vencido resulta em nenhum motivo (`app/chamados/services/fabrica_da_celula.py:45-49`, `app/chamados/services/fabrica_da_celula.py:58-61`). A margem é 30 minutos e a comparação é `quando + timedelta(minutes=teto_min) <= agora` (`app/ponto/services/cartorio.py:828`, `app/ponto/services/cartorio.py:858-859`).
- `perguntar_pela_celula` encaminha passado sem turno à pauta de cadastro, consulta os motivos e, havendo motivos, chama `perguntar` sob push silenciado; limbo de competência entregue vai à pauta DP (`app/chamados/services/fabrica_da_celula.py:192-210`). `perguntar` cria/atualiza a disputa e gera as perguntas (`app/chamados/services/fabrica_da_celula.py:161-167`).
- `motivos_que_o_evento_deixou` usa `motivos_a_perguntar` e a exceção de pauta do limbo; não tem guarda de adesão (`app/chamados/services/fabrica_da_celula.py:225-236`). Portanto SEM_ADESAO com `nunca_bateu` pode retornar motivos devidos quando as outras guardas permitem, apesar do comentário “carimba e nao emite” do cartório (`app/ponto/services/cartorio.py:1168-1172`; `app/escala/models.py:1767-1768`; `app/chamados/services/fabrica_da_celula.py:102-150`).
- O vigia do `disparar` seleciona passado da janela, exclui ontem ainda candidato do vigia de hora, acrescenta cada motivo devolvido à lista `deixou` e imprime sua contagem; também não consulta adesão nesse universo (`app/chamados/management/commands/disparar_perguntas_competencia.py:100-113`, `app/chamados/management/commands/disparar_perguntas_competencia.py:240-243`).

Para COM_ADESAO, se a célula chega ao ramo normal com `teto_de_hoje` e há vencidos, `nunca_bateu` impede `_em_dia`; a mudança do número de vencidos também integra a impressão, fazendo o marco novo participar do rejulgamento (`app/ponto/services/cartorio.py:1324-1344`, `app/ponto/services/cartorio.py:1359-1375`). Isso depende de o chamador selecionar e invocar o julgamento da célula; causa real de não emissão na produção permanece [não verificado].

_3296 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
