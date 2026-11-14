# CODEX-ESTADO

> Ultima resposta processada: **14/11/2026 20:48** · publicado em **14/11/2026 20:48** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 14/11 20:48 · sessao 7070790a6a5f · - · - · origem=Ronald (tmux)

P5 — censo estático da cópia, sem banco/log. Frequência efetiva por requisição, flags do ambiente e duração de produção: [não verificado].

**Chamadores diretos completos encontrados por nome/import alias no Python de produção:**

- `periodos_justificados`: `efeitos_da_ausencia` (`app/ponto/services/efeito_ausencia.py:46`); overlay `_marcar_ausencias` (`app/escala/utils.py:1198`); `grade_do_fechamento` (`app/folha/export.py:339`); `montar_dias` (`app/ponto/services/espelho.py:520`).
- `efeitos_da_ausencia`: coletor PDF (`app/relatorios/pdf_espelho.py:333`); `assinaturas_do_colab`, condicionado a vínculo/tipo (`app/ponto/services/esmeril_espelho.py:155`, `app/ponto/services/esmeril_espelho.py:161`); fechamento (`app/ponto/services/fechamento.py:665`); `autoridade_do_periodo` (`app/ponto/services/espelho.py:1071`); comandos de diagnóstico `diff_espelho_alimentacao` (`app/ponto/management/commands/diff_espelho_alimentacao.py:117`) e `diff_extrato_parcial._antes` (`app/relatorios/management/commands/diff_extrato_parcial.py:41`).
- `grade_da_celula` deixou de consultá-los implicitamente; recebe mapa ou `{}` (`app/escala/services/leitor_celula.py:369`, `app/escala/services/leitor_celula.py:381`).

**Contagens estruturais, quando chega ao fim sem exceção:**

Use `E=efeitos_da_ausencia`, `P=periodos_justificados`; cada `E` inclui um `P` (`app/ponto/services/efeito_ausencia.py:46`).

| Caminho por colaborador | E | P | Prova |
|---|---:|---:|---|
| `autoridade_do_periodo` | 1 | 1 | `app/ponto/services/espelho.py:1071` |
| Recalcular fechamento, inclusive modo leitura | 1 | 2 | `app/ponto/services/fechamento.py:665`, `app/ponto/services/fechamento.py:1019`, `app/folha/export.py:339` |
| `grade_do_fechamento` isolada | 0 | 1 | `app/folha/export.py:339` |
| Builder por turno isolado | 0 | 1 | `app/escala/utils.py:1868`, `app/escala/utils.py:1198` |
| Lista de fechamento | 0 | 0 ou 1 | Só chama prontidão com fechamento+empresa: `app/ponto/services/fechamento_lista.py:85`; snapshot evita grade: `app/folha/export.py:1048`; fallback chama grade: `app/folha/export.py:1098` |
| Export TXT aceito pelo portão | 1 | 2 | Recalcula em leitura a empresa inteira antes do classificador: `app/folha/views.py:207`, `app/folha/views.py:208`, `app/folha/views.py:212` |

No recálculo, o laço é uma vez por colaborador elegível (`app/ponto/services/fechamento.py:619`); o efeito é anterior ao fatiamento e o mapa é reutilizado pelos motores (`app/ponto/services/fechamento.py:665`, `app/ponto/services/fechamento.py:755`, `app/ponto/services/fechamento.py:758`). Portanto **não multiplica por número de vínculos/fatias**, mas a mesma apuração é repetida quando diferentes leitores chamam fechamento/espelho (`app/ponto/services/fechamento.py:758`, `app/ponto/services/espelho.py:1231`, `app/relatorios/cartao_pela_celula.py:73`).

Para o espelho/tela, defina:

- `G=1` quando entra na guarda de escala/tipo e alcança a consulta financeira da grade (`app/ponto/services/espelho.py:484`, `app/ponto/services/espelho.py:520`).
- `B=1` quando também monta o builder para o trecho anterior à competência ou pela exceção `AtaAgregadaSemMarcos` (`app/ponto/services/espelho.py:533`, `app/ponto/services/espelho.py:534`, `app/ponto/services/espelho.py:540`, `app/ponto/services/espelho.py:563`).
- `F=1` quando período pedido é competência, existe fechamento gravado e o colaborador participa do recálculo; `folha_manda` chama `totais_da_folha`, que **sempre recalcula com FM existente**, sem teste de frescor (`app/relatorios/cartao_pela_celula.py:321`, `app/relatorios/cartao_pela_celula.py:323`, `app/relatorios/cartao_pela_celula.py:71`, `app/relatorios/cartao_pela_celula.py:73`, `app/ponto/services/fechamento.py:1764`).

Assim `espelho_do_colab` custa **E=1+F; P=1+G+B+2F**: autoridade inicial, grade financeira, builder eventual e folha online (`app/ponto/services/espelho.py:1231`, `app/ponto/services/espelho.py:520`, `app/ponto/services/espelho.py:534`, `app/ponto/services/espelho.py:1288`). Com `G=F=1,B=0`: **E2/P4**; com trecho histórico: **E2/P5** pelas mesmas referências. Admin e app chamam esse caminho (`app/ponto/views.py:161`, `app/api/views.py:1354`); ficha também (`app/colaboradores/services/ficha.py:199`).

O PDF acrescenta **E1/P3**, chama o espelho completo e chama novamente `folha_manda`: efeito direto (`app/relatorios/pdf_espelho.py:333`), **dois builders** (`app/relatorios/pdf_espelho.py:476`, `app/relatorios/pdf_espelho.py:544`), espelho (`app/relatorios/pdf_espelho.py:599`), folha final (`app/relatorios/pdf_espelho.py:629`). Para os dois `F=1`, `G=1,B=0`: **E4/P9**; com builder histórico interno: **E4/P10**, pelas referências anteriores. Flags de produção e exceções podem mudar a contagem; o default do builder por turno é `True` (`app/config/settings/base.py:165`).

Calendário pode pagar **E1/P1 + F*(E1/P2)** quando foi solicitado o cálculo de dias em aberto sem fornecer a lista; com lista fornecida não entra nesse ramo (`app/colaboradores/services/calendario.py:563`, `app/colaboradores/services/calendario.py:564`, `app/colaboradores/services/calendario.py:566`, `app/colaboradores/services/calendario.py:570`, `app/colaboradores/services/calendario.py:583`).

O portão que **mede** exportação é mais caro que o POST: por colaborador classificado `entra`, chama um espelho e um PDF, mais uma autoridade independente para comparar DiaPago (`app/folha/porta_export.py:469`, `app/folha/porta_export.py:470`, `app/folha/porta_export.py:476`, `app/folha/porta_export.py:496`, `app/folha/porta_export.py:597`, `app/folha/porta_export.py:78`). Há acréscimos condicionais de grade por campo divergente (`app/folha/porta_export.py:461`, `app/folha/porta_export.py:139`) e um comando agregado ao final; não fechei o custo total desse comando aninhado [não verificado] (`app/folha/porta_export.py:667`). O POST só confere o carimbo antes do recálculo, não executa essa medição (`app/folha/views.py:128`, `app/folha/views.py:129`).

**Escrita de batida continua chegando aos três.**

1. Criação chama `_fechamento_por_evento` após `create`, inclusive conflito humano (`app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:155`, `app/ponto/registro_batida.py:157`, `app/ponto/registro_batida.py:160`). A porta chama `recalcular_por_evento` (`app/ponto/registro_batida.py:177`, `app/ponto/registro_batida.py:178`), agenda callback em `on_commit` (`app/ponto/services/fechamento.py:1829`) e executa recálculo do colaborador/competência (`app/ponto/services/fechamento.py:1814`, `app/ponto/services/fechamento.py:1815`). Esse caminho custa **E1/P2** se o colaborador for processado, e cada `P` chega a `realizado_do_dia` por dia aprovado (`app/ponto/services/efeito_ausencia.py:122`, `app/ponto/services/efeito_ausencia.py:125`).

2. O próprio `post_save` julga **ontem e hoje** (`app/ponto/signals.py:108`, `app/ponto/signals.py:112`, `app/ponto/signals.py:131`, `app/ponto/signals.py:135`), via cartório (`app/ponto/signals.py:92`, `app/ponto/signals.py:93`). Pode parar por reentrada, célula inexistente/futura ou turno de hoje não encerrado (`app/ponto/signals.py:90`, `app/ponto/services/cartorio.py:2025`, `app/ponto/services/cartorio.py:2028`, `app/ponto/services/cartorio.py:2033`, `app/ponto/services/cartorio.py:2037`).

3. Cada corpo efetivamente executado do cartório monta um builder (`app/ponto/services/cartorio.py:1175`), cujo overlay chama `P` (`app/escala/utils.py:1868`, `app/escala/utils.py:1198`) e cujo laço consulta `realizado_do_dia` por dia do envelope (`app/escala/utils.py:2061`, `app/escala/utils.py:2065`). Para julgamento individual o envelope tem **dois dias** (`app/ponto/services/cartorio.py:1173`, `app/ponto/services/cartorio.py:1174`). O corpo pode repetir por reconciliação e por emissão, portanto não é teto fixo 2 chamadas por batida (`app/ponto/services/cartorio.py:996`, `app/ponto/services/cartorio.py:1017`, `app/ponto/services/cartorio.py:1021`, `app/ponto/services/cartorio.py:1073`, `app/ponto/services/cartorio.py:1086`). Se `C` é número de corpos executados, esse ramo acrescenta **P=C**, e aproximadamente **2C consultas diretas de realizado + consultas internas dos P por dia aprovado**, no builder por turno (`app/ponto/services/cartorio.py:1174`, `app/escala/utils.py:2061`, `app/ponto/services/efeito_ausencia.py:122`).

4. Retratação também agenda fechamento (`app/ponto/registro_batida.py:274`, `app/ponto/registro_batida.py:281`); `anotar.save()` aciona os receivers (`app/ponto/registro_batida.py:254`, `app/ponto/signals.py:131`); apagar aciona `post_delete` do cartório (`app/ponto/registro_batida.py:291`, `app/ponto/signals.py:138`, `app/ponto/signals.py:140`). Deduplicação/recusa saem antes da criação e da chamada explícita de fechamento (`app/ponto/registro_batida.py:114`, `app/ponto/registro_batida.py:121`, `app/ponto/registro_batida.py:150`).

**Outro custo oculto:** cada `P` sem justificativa aprovada faz apenas a consulta inicial e retorna; havendo aprovada, recarrega células, todos os vínculos e turnos com ±1 dia, e chama realizado por dia distinto (`app/ponto/services/efeito_ausencia.py:89`, `app/ponto/services/efeito_ausencia.py:95`, `app/ponto/services/efeito_ausencia.py:99`, `app/ponto/services/efeito_ausencia.py:100`, `app/ponto/services/efeito_ausencia.py:104`, `app/ponto/services/efeito_ausencia.py:122`). Embora forneça turnos, não fornece `livre`/`intervalo`, então realizado volta a consultar o vínculo do dia (`app/ponto/services/efeito_ausencia.py:125`, `app/ponto/turnos.py:781`, `app/ponto/turnos.py:788`). Medição de queries/tempo dessa repetição nesta produção: [não verificado].

## 14/11 20:48 · sessao de9b8d116772 · - · - · origem=Ronald (tmux)

P6/P8 — leitura concluída; nenhum arquivo alterado. Execução de produção, banco e logs: [não verificado].

**P8: a entrega universal do excedente à lista NÃO está implementada.** `periodos_justificados` calcula `trabalhado` e `max(0, previsto-trabalhado)`, mas não publica excedente nem chama HE; o próprio docstring declara que `conserva` não cria fila (`app/ponto/services/efeito_ausencia.py:79-83,122-129`). A lista existente recebe **pontas fora de marcos**, pergunta diferente de `trabalhado>previsto` (`app/ponto/services/espelho.py:724-752`; `app/ponto/services/he_pendente_lavrado.py:132-153`).

Contraprova disponível no próprio teste novo: a empresa da fixture nasce sem configurar HE (`app/ponto/tests/test_justificativa_paga_o_periodo.py:181`), cujo default é `janela_he_ativa=False` e `desde=None` (`app/colaboradores/models.py:58-59,117-120`); A2 declara 450 previstos, 483 trabalhados e paga zero, mas só asserta pagamento/falta/DSR, nenhuma lista (`app/ponto/tests/test_justificativa_paga_o_periodo.py:541-568`). No montador dessa configuração `_he_pendencias=[]` não entra no ramo de HE (`app/ponto/services/espelho.py:724-725`), e dia sem essa lista é descartado do retrato (`app/ponto/services/he_pendente_lavrado.py:132-134`). É uma contraprova estática; resultado executado desse cenário: [não verificado].

Caminho que existe quando as condições permitem:

1. `montar_dias` lê cadastro HE uma vez (`app/ponto/services/espelho.py:384-392`), marca pontas da grade, acrescenta relato do motor e dia sem previsto (`app/ponto/services/espelho.py:725-752`), publica `he_fora_da_janela` (`app/ponto/services/espelho.py:779`).
2. `lavrar_he_pendente` chama `apurar`, que pede `espelho_do_colab` por colaborador ativo e lê essa lista (`app/ponto/management/commands/lavrar_he_pendente.py:62`; `app/ponto/services/he_pendente_lavrado.py:92-94,132-153`). `--apply` grava `MetricaSnapshot` (`app/ponto/management/commands/lavrar_he_pendente.py:70-73`; `app/ponto/services/he_pendente_lavrado.py:177-190`).
3. Cron configurado dispara o lavrador após `lavrar_deslogados`; depois dispara recusa de ponta pequena (`app/config/crons.py:1572-1587`). Execução efetiva do cron: [não verificado].
4. Rota `gestao-he/` chama a view (`app/ponto/urls.py:17`), que lê snapshot e chama `enriquecer`, sem medir o vivo (`app/ponto/views.py:932-946`). Template desenha colabs e dias (`app/templates/ponto/gestao_he.html:92-125`).
5. Form de decisão passa minutos à porta (`app/ponto/views.py:532-534`); `decidir_he` grava pela `_gravar` e agenda recalcular (`app/ponto/portas/he.py:181-202`). A porta não faz o excedente chegar à lista; recebe a decisão sobre o que já foi mostrado (`app/ponto/portas/he.py:143-149`).

Ramos que podem deixar o excedente sem essa lista:

- HE desligada, `desde` nulo ou dia anterior à vigência: nenhuma pendência (`app/ponto/services/espelho.py:724-725`).
- Só primeira entrada e última saída dos **marcos** são pontas; células `missing` puladas, saída depende de `saida_ativa`, delta sem sinal de fora é pulado (`app/ponto/janela_he.py:194-211`).
- Extra declarada absorve a ponta ou sobra dentro do piso: pulada (`app/ponto/janela_he.py:215-217`).
- Fallback do relato só aceita causa “janela de HE”, mesmo dia, sentido conhecido, saída ativa, sentido ainda não marcado, célula existente e `missing`, mesmo marco, minutos positivos (`app/ponto/janela_he.py:267-315`). Logo não cobre toda diferença entre soma do dia e previsto.
- Universo do retrato só colaboradores `situacao='ativo'`; erro de espelho exclui colaborador e incrementa erros; dia sem data legível/fora da janela excluído (`app/ponto/services/he_pendente_lavrado.py:92-100,114-119`).
- Sem pendência de HE publicada, dia excluído (`app/ponto/services/he_pendente_lavrado.py:132-134`).
- Sem snapshot, leitor devolve `(None,None)` e enriquecimento retorna lista vazia (`app/ponto/services/he_pendente_lavrado.py:193-206`; `app/ponto/services/gestao_he.py:236-244`). Snapshot atual/ocorrência em produção: [não verificado].
- Automação padrão atualiza competência atual e apenas anterior não entregue; anteriores mais antigas e anterior exportada ficam fora desse ciclo (`app/ponto/services/he_pendente_lavrado.py:218-225`).
- Busca, praça, posto, estado e dono podem retirar dias/colabs da tela (`app/ponto/services/gestao_he.py:317-337`). Ponta pequena **não desaparece** da lista geral: recebe estado `nao`; somente filtro `dono=admin` a exclui (`app/ponto/portas/he.py:276-279`; `app/ponto/services/gestao_he.py:278-286,333-337`).
- Acesso à tela exige `ver_folha` ou `autorizar_he` e empresa visível (`app/ponto/views.py:914-924`).

**P6: censo lexical dos apagados.** Executei busca recursiva em toda cópia e adicional em produção/comandos/templates/JS, excluindo testes/docs/migrations. Zero ocorrências executáveis para cada um: `_ordem_e_sentido_dos_marcos`, `_trecho_do_marco`, `trabalhado_do_turno`, `marcos_que_nao_cruzam_o_trabalhado`, `minutos_do_periodo_justificado`, `intervalos_do_periodo_justificado`, `_minutos_sem_repetir`, `justificativas_para_revisao`, `pago_mais_trabalhado_passa_do_previsto`, `status=revisao`, `j.revisao`. O resultado negativo não possui arquivo:linha persistido; prova por artefato de censo: [não verificado]. As únicas ocorrências na cópia são documentação histórica e evidências/testes; o arquivo `evidencia_justificativa_c7_auditoria.txt` inclui saída antiga que ainda mostra definições removidas, portanto não deve ser usado como estado atual (`app/ponto/tests/evidencia_justificativa_c7_auditoria.txt:1-14`). O teste atual só certifica ausência textual dos três elementos da Central (`app/ponto/tests/test_justificativa_soma_sem_banco.py:451-454`).

A remoção não apagou `fila_justificativas` nem sua URL (`app/ponto/views.py:1698`; `app/ponto/urls.py:38`). `?status=revisao` agora cai sem filtro de status e lista todas as justificativas: só pendente/aprovada/rejeitada têm ramos (`app/ponto/views.py:1703,1712-1718,1731-1736`). Link interno antigo não encontrado pelo censo; links externos/bookmarks: [não verificado]. Template não tem card de revisão e célula status mostra apenas status (`app/templates/ponto/fila_justificativas.html:48-58,112-113`).

Restou comentário contraditório “sem ancora = lista do admin” em `app/ponto/turnos.py:2662-2664`; implementação só registra `conserva` (`app/ponto/services/efeito_ausencia.py:116-117`).

Testes apagados pelo delta fornecido: [nao verificado]

- `MinutosDoPeriodoTest`: C1,C2,C3,C4,C5,C6, nada coberto, entrada coberta, propriedade parte+falta. Exclusão em arquivo antigo só consta do anexo, sem linhas da cópia: [não verificado]. Medir da ponta da âncora não é mais lei; pausas/noturno continuam cobertos por `test_02_duas_pausas_550` e `test_10_noturno_duas_pausas_nao_perde_a_data_do_turno` (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:214-218,343-347`).
- `CruzamentoComOTrabalhadoTest`: os sete casos da guarda de cruzamento. Exclusão só consta do anexo: [não verificado]. Retirar cobertura por sobreposição contraria agora o comportamento explicitamente testado por A1 (`app/ponto/tests/test_justificativa_soma_sem_banco.py:341-348`).
- `test_03b_uniao_de_trechos_distintos_sobrepostos_e_disjuntos`: perdeu objeto porque o pagamento é saldo único; duas âncoras continuam verificadas em `test_03_duas_ancoras_pagamento_unico_510` (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:220-227`).
- `test_fila_e_projecao_do_motivo_da_fonte` e `test_central_revisao_le_fila_uma_vez_por_colab_e_nao_rejulga`: exclusão só consta do anexo [não verificado]. **Não afirmar que eram prova correta de excedente real**: ambos usavam col788, cujo teste atual prova 59 trabalhados+361 pagos=420 previstos, portanto sem excedente (`app/ponto/tests/test_justificativa_paga_o_periodo.py:504-516`). O que falta é teste substituto da entrega do excedente real pela HE; os testes novos sobre fila só exigem remoção de símbolos (`app/ponto/tests/test_justificativa_soma_sem_banco.py:424-429,451-454`).

Não identifiquei teste removido que fosse a única prova **ainda correta sob o corte novo**; exclusividade absoluta perante toda suite não executada: [não verificado]. A lacuna comprovável é que A2 já constrói excedente real, mas termina sem verificar HE/lista (`app/ponto/tests/test_justificativa_paga_o_periodo.py:541-568`).

## 14/11 20:47 · sessao 2c6de581f85c · - · - · origem=Ronald (tmux)

Achados do motor, todos por leitura da cópia; a comparação executável com base/main ficou [não verificado].

- **Divergência concreta adicional: entrada única aberta, sem par fechado.** `periodos_justificados` calcula `trabalhado=0` e paga o previsto (`app/ponto/services/efeito_ausencia.py:125`). O feed remove esse dia de `min_justificados` (`app/ponto/services/efeito_ausencia.py:48`). Entretanto o motor inclui períodos abertos, com zero minutos, em `resultado.periodos` (`app/ponto/motor_calculo_v2.py:2499`, `app/ponto/motor_calculo_v2.py:2501`), e considera qualquer período como dia trabalhado (`app/ponto/motor_calculo_v2.py:2581`). Por isso o dia não entra em `_abono` e não recebe abono nenhum: **fonte=previsto; motor=0 minutos/0 dias**, embora não vire falta nem perca DSR (`app/ponto/motor_calculo_v2.py:2605`, `app/ponto/motor_calculo_v2.py:2611`, `app/ponto/motor_calculo_v2.py:2628`).

- **Ausência ABONA inteira com trabalho + justificativa:** a ausência remove o saldo do mapa (`app/ponto/services/efeito_ausencia.py:62`); o motor só paga abono inteiro quando o dia não consta em `datas_trabalho` (`app/ponto/motor_calculo_v2.py:2611`). Portanto, havendo período de trabalho, motor=**0 minutos/0 dias de abono**, enquanto a fonte bruta de justificativa continua retornando `max(0,P−T)` (`app/ponto/services/efeito_ausencia.py:128`). A grade financeira recebe a fonte bruta diretamente (`app/folha/export.py:340`).

Tabela do ramo real do motor, extraído por AST e executado em memória, com um dia obrigatório:

| Cenário | Minutos de abono | Dias | Fonte |
|---|---:|---:|---|
| (a) P480 sem batida | 480 | 1 | `app/ponto/motor_calculo_v2.py:2611`, `app/ponto/motor_calculo_v2.py:2630` |
| (b) P720/T600 | 120 | 0 | `app/ponto/services/efeito_ausencia.py:48`, `app/ponto/motor_calculo_v2.py:2633` |
| (c) P450/T483 | 0 | 0 | `app/ponto/services/efeito_ausencia.py:128`, `app/ponto/motor_calculo_v2.py:2633` |
| (d) P480/T420 | 60 | 0 | `app/ponto/services/efeito_ausencia.py:128`, `app/ponto/motor_calculo_v2.py:2633` |
| (e) ABONA inteira, sem trabalho/período | P | 1 | `app/ponto/services/efeito_ausencia.py:64`, `app/ponto/motor_calculo_v2.py:2630` |
| (e) ABONA inteira com período de trabalho | 0 | 0 | `app/ponto/services/efeito_ausencia.py:64`, `app/ponto/motor_calculo_v2.py:2611` |
| (f) parcial + justificativa, T>0 | max(0,P−T) | 0 | `app/ponto/services/efeito_ausencia.py:48`, `app/ponto/services/efeito_ausencia.py:58`, `app/ponto/motor_calculo_v2.py:2633` |
| (g) folga fora dos dias obrigatórios | 0 | 0 | `app/ponto/motor_calculo_v2.py:2601`, `app/ponto/motor_calculo_v2.py:2633` |
| (h) duas justificativas | mesmo saldo único | conforme trabalho/períodos | `app/ponto/services/efeito_ausencia.py:106`, `app/ponto/services/efeito_ausencia.py:122` |

Essas execuções verificam somente o ramo de falta/abono, **não motor completo, ORM, produção nem contadores semanais** [não verificado]. No ramo executado, todos os casos apresentaram `faltas=0` e `dsr_ok=True`; isso decorre de todo dia aprovado entrar em `justificadas`, independentemente de cobertura e pagamento (`app/ponto/services/efeito_ausencia.py:47`, `app/ponto/motor_calculo_v2.py:2605`).

Para P3: **não achei contraexemplo de nova falta/DSR no ramo do motor**; justificativas aprovadas sempre protegem o dia nesse cálculo (`app/ponto/services/efeito_ausencia.py:43`, `app/ponto/motor_calculo_v2.py:2607`). O override comercial também consulta `datas_justificadas` para decidir DSR (`app/ponto/motor_calculo_v2.py:3478`, `app/ponto/motor_calculo_v2.py:3490`). Isso não prova todos os outros leitores nem a comparação com base/main [não verificado].

Caminhos reais de injeção:

- Fechamento: efeitos → `datas_justificadas`/mapa → motor da fatia e motor órfão (`app/ponto/services/fechamento.py:664`, `app/ponto/services/fechamento.py:742`, `app/ponto/services/fechamento.py:755`, `app/ponto/services/fechamento.py:823`, `app/ponto/services/fechamento.py:831`).
- Espelho: mesmos efeitos → motor (`app/ponto/services/espelho.py:1071`, `app/ponto/services/espelho.py:1117`, `app/ponto/services/espelho.py:1137`).
- PDF: mesmos efeitos → motor (`app/relatorios/pdf_espelho.py:333`, `app/relatorios/pdf_espelho.py:407`).
- Esmeril: mesmos efeitos → motor (`app/ponto/services/esmeril_espelho.py:165`).
- Contagens de abono e DSR são somadas entre fatias (`app/ponto/services/fechamento.py:98`, `app/ponto/services/fechamento.py:298`).

Cuidado com resultado final: espelho chama `folha_manda`, então seus totais finais podem vir da lavratura, divergindo do resultado bruto do motor (`app/ponto/services/espelho.py:1287`). PDF preliminar acrescenta ausência parcial paga ao abono do motor (`app/relatorios/pdf_espelho.py:480`).

`dia_inteiro` tem efeito próprio no motor: zera atraso e saída antecipada, preservando trabalho; justificativas por si só não alimentam esse mapa de ausências (`app/ponto/services/efeito_ausencia.py:70`, `app/ponto/motor_calculo_v2.py:1090`, `app/ponto/motor_calculo_v2.py:1098`).

Porta de HE existente: decisão vigente é carregada por dia e a janela considera autorização (`app/ponto/motor_calculo_v2.py:1402`, `app/ponto/motor_calculo_v2.py:1658`). A chegada excedente pode ser clipada e os minutos separados (`app/ponto/janela_he.py:76`, `app/ponto/janela_he.py:102`). Provar que **todo** excedente dos casos fornecidos chega à lista do admin exige seguir o retrato/porta com dados de cadastro não fornecidos [não verificado].

## 14/11 20:47 · sessao e05797a4bd53 · - · - · origem=Ronald (tmux)

Auditoria dos leitores financeiros, somente por leitura.

- **Fonte e grade usam mapas distintos.** `efeitos_da_ausencia.min_justificados` inclui somente dias com `trabalhado > 0`; `grade_do_fechamento` e o espelho fornecem à grade todos os dias de `periodos_justificados`, inclusive dias sem trabalho. A grade só transporta o mapa quando `tipo_dia == 'trabalho'`. (`app/ponto/services/efeito_ausencia.py:48`; `app/folha/export.py:339`; `app/ponto/services/espelho.py:520`; `app/escala/services/leitor_celula.py:449`)

- **Ausência parcial tem precedência sobre a justificativa na grade.** `_marcar_ausencias` escreve `tipo_dia='ausencia'` também para parcial e, por usar `elif`, não aplica a justificativa naquele dia. `por_dia_da_grade` trata qualquer ausência como crédito `previsto - min(realizado, previsto)`, ignorando o período parcial e `min_justificados`. (`app/escala/utils.py:1207`; `app/escala/utils.py:1217`; `app/escala/utils.py:1227`; `app/escala/utils.py:1251`; `app/ponto/services/dia_pago.py:394`)

- **PDF tem soma própria adicional.** Antes de `folha_manda`, calcula `horas_abono = resultado.horas_abono + min_parcial_pagos/60`. Portanto, com previsto 480, trabalho 240, parcial paga 120 e justificativa pagando 240, o PDF de período livre informa crédito de **360 minutos**; motor e grade informam **240**. Na competência, `folha_manda` pode sobrescrever as horas para o número da grade. (`app/relatorios/pdf_espelho.py:479`; `app/relatorios/pdf_espelho.py:480`; `app/ponto/services/efeito_ausencia.py:48`; `app/ponto/services/efeito_ausencia.py:50`; `app/relatorios/cartao_pela_celula.py:321`; `app/relatorios/cartao_pela_celula.py:346`)

- **Cartão varia conforme existência de fechamento.** Havendo fechamento, lê `fechamento_lido` e devolve `horas_abono` de `minutos_abonados`, mas `_totais_do_fechamento` não devolve `dias_abono`: permanecem os dias previamente escritos pelo motor. Sem fechamento, havendo DiaPago, devolve horas da soma lavrada e dias por `dias_com_abono`. (`app/relatorios/cartao_pela_celula.py:69`; `app/relatorios/cartao_pela_celula.py:93`; `app/relatorios/cartao_pela_celula.py:118`; `app/relatorios/cartao_pela_celula.py:186`; `app/relatorios/cartao_pela_celula.py:190`; `app/relatorios/cartao_pela_celula.py:323`; `app/relatorios/cartao_pela_celula.py:342`)

- **Dias da parcial são contados como abono inteiro pela lavratura.** `dias_com_abono` aceita qualquer ata `tipo_dia='ausencia'`, sem testar `cobertura_parcial` ou realizado. Assim uma parcial com crédito positivo conta **1 dia**, enquanto a justificativa com trabalho conta **0 dias no motor**. (`app/ponto/services/dia_pago.py:700`; `app/ponto/services/dia_pago.py:703`; `app/ponto/motor_calculo_v2.py:2628`)

Tabela da **grade financeira**, assumindo ata coerente com o realizado informado:

| P2 | Crédito da grade |
|---|---:|
| a | 480 |
| b | 120 |
| c | 0 |
| d | 60 |
| e | `max(0, P − T_da_ata)` |
| f | `max(0, P − T_da_ata)` |
| g | 0 |
| h | um único saldo do dia |

A grade recebe saldo único de PJ para dias de trabalho; para ausências usa o previsto menos realizado da ata. Folga fica fora do universo da soma. (`app/folha/export.py:339`; `app/escala/services/leitor_celula.py:449`; `app/ponto/services/dia_pago.py:388`; `app/ponto/services/dia_pago.py:394`; `app/ponto/services/efeito_ausencia.py:106`; `app/ponto/services/efeito_ausencia.py:122`)

Contraexemplo **e**, previsto 480 e trabalho 240: ausência ABONA inteira retira o saldo do mapa do motor; como existe trabalho, o motor não inclui o dia em `_abono`, ficando **0 minutos/0 dias**. Grade devolve **240 minutos**; `dias_com_abono` devolve **1** se esse crédito foi lavrado. Cartão com fechamento fica **240 minutos/0 dias**; cartão apenas com lavratura fica **240 minutos/1 dia**. (`app/ponto/services/efeito_ausencia.py:62`; `app/ponto/services/efeito_ausencia.py:64`; `app/ponto/motor_calculo_v2.py:2611`; `app/ponto/motor_calculo_v2.py:2628`; `app/ponto/services/dia_pago.py:396`; `app/ponto/services/dia_pago.py:703`; `app/relatorios/cartao_pela_celula.py:118`; `app/relatorios/cartao_pela_celula.py:190`)

O espelho inicializa `dias_abono` com o resultado do motor; as horas de abono entram no resumo por `folha_manda`. Fora da competência, sua montagem não escreve `horas_abono` no resumo; o PDF livre escreve explicitamente motor mais parcial. (`app/ponto/services/espelho.py:1234`; `app/ponto/services/espelho.py:1256`; `app/ponto/services/espelho.py:1287`; `app/relatorios/cartao_pela_celula.py:321`; `app/relatorios/pdf_espelho.py:480`)

## 14/11 20:47 · sessao f710eb5486a5 · - · - · origem=Ronald (tmux)

P4: `dias_com_abono` tem regra própria de classe e cruza dados de momentos diferentes. Não é equivalente ao motor em todos os cenários. (`app/ponto/services/dia_pago.py:700-705`; `app/ponto/motor_calculo_v2.py:2611-2634`.)

**Dados efetivamente lidos e origem**

| Dado | Origem/escritor/quando |
|---|---|
| `colab, mes, ano, versao` | Argumentos do chamador; versão padrão `'motor'`. Único chamador de produção encontrado: `_totais_da_lavratura`, que passa colaborador/competência e usa a versão padrão. (`app/ponto/services/dia_pago.py:690`; `app/relatorios/cartao_pela_celula.py:141-143`; `app/relatorios/cartao_pela_celula.py:190`.) |
| `DiaPago.data`, `tipo='dia'`, `minutos_abonados>0` | `lavrar` recebe mapas da grade por data, grava `minutos_abonados` e cria a linha diária; substitui todas as linhas da competência/versão. (`app/ponto/services/dia_pago.py:135-141`; `app/ponto/services/dia_pago.py:288-292`; `app/ponto/services/dia_pago.py:345-360`.) |
| Mapa de abono que alimenta `lavrar` | Fechamento chama `grade_do_fechamento`, deriva os mapas com `por_dia_da_grade`, soma o abono no fechamento e passa os mapas para a lavratura. (`app/ponto/services/fechamento.py:1018-1019`; `app/ponto/services/fechamento.py:1065-1069`; `app/ponto/services/fechamento.py:1205-1218`.) |
| Regra do mapa de abono | Só considera classes `trabalho` e `ausencia`. `ausencia`: previsto menos realizado com cap; `trabalho`: apenas `min_justificados` truthy. (`app/ponto/services/dia_pago.py:388-404`.) |
| Outro escritor dos três campos `minutos_*` | `retratar_exportada` usa a mesma grade, cria/atualiza os três campos, zera dias que saíram da grade e grava por bulk update/create. A guarda pode barrar o retrato. (`app/ponto/services/dia_pago.py:454-486`; `app/ponto/services/dia_pago.py:491-500`; `app/ponto/services/dia_pago.py:537-548`.) |
| `CelulaDia.ata.tipo_dia` | Builder marca ausência inteira **e parcial** como `'ausencia'`; parcial recebe `cobertura_parcial=True`, mas a classe continua ausência. (`app/escala/utils.py:1217-1231`.) |
| `CelulaDia.ata.minutos_realizados` | Builder consulta `realizado_do_dia`, declara zero só quando sem turno e guarda os minutos reais sem o cap da lavratura. (`app/escala/utils.py:2065-2079`.) |
| Escrita persistente desses dois atributos da ata | `cartorio.ata_do_dia` copia ambos; cartório monta a grade e, com `apply_`, entrega a ata à lavra e carimba. Porta atribui `cel.ata`; `carimbar` salva junto dos demais campos. (`app/ponto/services/cartorio.py:738-740`; `app/ponto/services/cartorio.py:1175`; `app/ponto/services/cartorio.py:1850-1853`; `app/ponto/services/cartorio.py:1872-1874`; `app/ponto/portas/celula.py:1090-1098`; `app/ponto/portas/celula.py:1181-1201`.) |
| Momento da lavratura `DiaPago` | Fechamento normal, ou leitura com `lavrar_dias_pagos=True` para backfill; leitura pura não entra. (`app/ponto/services/fechamento.py:1196-1205`; `app/ponto/services/fechamento.py:1242`.) |

**Divergências concretas**

1. **P2(f), parcial paga + justificativa, com trabalho e saldo positivo**: por exemplo P=720, W=600, parcial=60. O feed preserva `min_justificados[d]=120` porque parcial faz `continue`; motor conta **0 dias**, pois o dia está no mapa. Grade classifica a parcial como ausência e gera abono 120; `dias_com_abono` conta **1 dia**, ignorando `cobertura_parcial` e W=600. (`app/ponto/services/efeito_ausencia.py:48`; `app/ponto/services/efeito_ausencia.py:58-64`; `app/ponto/motor_calculo_v2.py:2628-2634`; `app/escala/utils.py:1217-1231`; `app/ponto/services/dia_pago.py:396-399`; `app/ponto/services/dia_pago.py:703-705`.)

2. **P2(e), inteira ABONA + justificativa, com trabalho e saldo positivo**: efeitos remove o dia do mapa de saldo; se W pertence a `resultado.periodos`, `datas_trabalho` contém o dia, o motor não o inclui em `_abono` e resulta **0 dias / 0 abono**. Grade ausência gera P−W e `dias_com_abono` conta **1 dia**. Exemplo P720/W600: grade/DiaPago 120 e contador 1 contra motor 0/0. (`app/ponto/services/efeito_ausencia.py:62-64`; `app/ponto/motor_calculo_v2.py:2581-2584`; `app/ponto/motor_calculo_v2.py:2611-2634`; `app/ponto/services/dia_pago.py:394-399`; `app/ponto/services/dia_pago.py:703-705`.)

3. **ATA ausente/incompleta**: DiaPago pode ter 480 abonados, mas se não existe CelulaDia, `ata` é vazia ou `minutos_realizados` está ausente, `dias_com_abono` devolve **0**, pois exige `ata` truthy e igualdade literal `==0`; não trata ausência de chave como zero. O leitor da grade, em contraste, converte ausência da chave para zero. (`app/ponto/services/dia_pago.py:700-705`; `app/escala/services/leitor_celula.py:443`.) A existência desta situação em produção é [não verificado].

4. **Mistura de snapshots**: contador filtra versão/competência apenas no DiaPago e busca a **ata atual** por data; não verifica carimbo, versão ou correspondência temporal entre os dois. Relavra da ata pode mudar a contagem sem mudança no dinheiro lavrado. (`app/ponto/services/dia_pago.py:700-705`; `app/ponto/portas/celula.py:1058-1066`; `app/ponto/portas/celula.py:1090-1098`.) Ocorrência produtiva [não verificado].

Para P2(a), supondo ata atual `trabalho/realizados=0` e crédito lavrado, conta 1; b/d contam 0 com W>0; c conta 0 por crédito zero; g conta 0 porque crédito zero e folga não entra na classe; h conta por data, não por justificativa. (`app/ponto/services/dia_pago.py:700-705`; `app/ponto/services/efeito_ausencia.py:106-128`.) Sem dados de ata/lavratura fornecidos, resultados concretos produtivos são [não verificado].

**`cartao_pela_celula` seleciona outra regra conforme a fonte**

- Com fechamento existente, usa `fechamento_lido`; as **horas** de abono vêm de `fe.minutos_abonados`, mas `_totais_do_fechamento` não escreve `dias_abono`, deixando o valor do resumo/motor. (`app/relatorios/cartao_pela_celula.py:69-76`; `app/relatorios/cartao_pela_celula.py:93-123`; `app/ponto/services/espelho.py:1256`; `app/relatorios/pdf_espelho.py:479`.)
- Sem fechamento, havendo lavratura, usa horas da soma de DiaPago e **dias da nova `dias_com_abono`**. Portanto P2(f) pode apresentar os mesmos minutos e mudar de 0 para 1 dia só pela presença/ausência do fechamento. (`app/relatorios/cartao_pela_celula.py:186-190`; `app/relatorios/cartao_pela_celula.py:321-346`.)
- Em período livre, `folha_manda` não substitui os totais por esses ramos de competência. (`app/relatorios/cartao_pela_celula.py:321-346`.)

## 14/11 20:45 · sessao 8dd5b2ceddde · E3-V10-DEGRAU-DONO-E-PORTA · censo · origem=Code

A triagem resulta em **F=26, C=23, R=2, Q=4; total=55**, conforme as linhas abaixo. A regra aplicada é porta do dono, ação administrativa com tela e consulta plural por colaborador sem porta; a classificação anterior da entrega não foi tomada como veredito. (app/chamados/services/tarefa_do_dia.py:28; app/chamados/services/tarefa_do_dia.py:41; app/chamados/tests/e3_lu2_entrega.txt:49)

A comparação histórica exata com `de702cb2f` ficou **[não verificado]**: a base não está acessível nesta cópia; o executor anexado depende de `git show`, não contém o código anterior. (app/chamados/tests/e3_degrau_mutantes.py:178) Reexecução da suíte e comportamento depois das correções propostas: **[não verificado]**.

**Os 26 ERROR — F.** M8 prevê passar `agora`; a chamada produtiva envia `agora=self.agora`, mas três lambdas dos testes recusam esse argumento. (app/chamados/tests/e3_lu2_entrega.txt:26; app/chamados/services/tarefa_do_dia.py:589)

Formas exatas referidas na tabela:

- **B:** `lambda _c, datas, *, agora=None: dict.fromkeys(datas, False)` em app/chamados/tests/test_e3_bloqueantes.py:68.
- **L:** `lambda _c, datas, *, agora=None: dict.fromkeys(datas, False)` em app/chamados/tests/test_e3_leitores_c17_chamadores.py:66.
- **E:** `_juiz(sim)` retorna `lambda _colab, datas, *, agora=None: dict.fromkeys(datas, sim)` em app/chamados/tests/test_e4_backfill.py:36; patch herdado em app/chamados/tests/test_e4_backfill.py:74.

| teste | classe | arquivo:linha do sítio que decide | o que mudar | [nao verificado]
|---|---|---|---|
| `BloqueantesE3Test.test_MORDE_8_chamado_TERMINAL_segue_sem_canal` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar o dublê para B. |
| `BloqueantesE3Test.test_MORDE_8_o_vigia_nao_le_o_predicado_cru` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar o dublê para B. |
| `ChamadoresDoC17LeemATarefaTest.test_MORDE_nenhum_chamador_migrado_pergunta_ao_C17` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar o dublê para L. |
| `ChamadoresDoC17LeemATarefaTest.test_RED_dias_sem_fio_nao_inclui_dia_so_fila` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar o dublê para L. |
| `ChamadoresDoC17LeemATarefaTest.test_RED_fora_da_folha_o_dia_vai_ao_grupo_C` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar o dublê para L. |
| `ChamadoresDoC17LeemATarefaTest.test_RED_o_que_segura_chamado_da_celula_sem_tarefa_tem_fio` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar o dublê para L. |
| `ChamadoresDoC17LeemATarefaTest.test_controle_dias_sem_fio_pergunta_na_fila_tem_fio` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar o dublê para L. |
| `ChamadoresDoC17LeemATarefaTest.test_controle_fora_da_folha_pergunta_na_fila_e_grupo_B` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar o dublê para L. |
| `ChamadoresDoC17LeemATarefaTest.test_controle_o_que_segura_pergunta_na_fila_e_colab` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar o dublê para L. |
| `BackfillE4bTest.test_backfill_2x_mesmo_estado_uma_trilha` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar `_juiz` para E. |
| `BackfillE4bTest.test_backfill_dry_usa_porta_e_nao_persiste` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar `_juiz` para E. |
| `BackfillE4bTest.test_exportada_nao_muda_hash_fm_txt` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar `_juiz` para E. |
| `BackfillE4bTest.test_sem_competencia_so_lista_e_nao_chama_porta` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar `_juiz` para E. |
| `PassivoE4bTest.test_classe_c_passivo_entra_pela_porta_da_E1_e_2x_e_uma_linha` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar `_juiz` herdado para E. |
| `PassivoE4bTest.test_classe_e_viva_sob_terminal_reabre_pela_porta_com_nota` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar `_juiz` herdado para E. |
| `PassivoE4bTest.test_classes_e_universos_nao_somem_para_zerar` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar `_juiz` herdado para E. |
| `PassivoE4bTest.test_mutante_sem_porta_morde_caracterizacao_classe_e` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_Ch12_P90_D90_exportada_desfaz` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_Ch12_P90_D90_fora_da_competencia_K6_false_desfaz` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_DRY_diff_por_colaborador_le_dinheiro_da_autoridade` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_callback_desconhecido_nomeado_e_fila_revertida` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_callback_real_de_retomada_desfaz` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_escrita_EventoCelula_por_porta_aparece_no_diff` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_foto_inteira_sob_C_e_reversao_antes_do_primeiro_ato` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_lote_misto_desfaz_recusado_enquanto_vizinho_pousa` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_tabela_nao_fotografada_desfaz` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar `_juiz` herdado para E. |

**Os 29 FAIL.** Nos testes de portas, `fila` agora nomeia chamados das portas **do dono**; `verbos` continua contexto do painel. Isso fundamenta as sete inversões abaixo. (app/chamados/services/tarefa_do_dia.py:41; app/chamados/services/tarefa_do_dia.py:624; app/chamados/tests/test_e3_degrau_dono_e_porta.py:551)

| teste | classe | arquivo:linha do sítio que decide | o que mudar | [nao verificado]
|---|---|---|---|
| `PortasDaTarefaTest.test_fila_A_pergunta_sob_B` | C | app/chamados/services/tarefa_do_dia.py:669 | LU-07/M5/M7: exigir `('porta_app', (), {a.pk:'decidir', b.pk:'cobrar'}, 'colab')`. |
| `PortasDaTarefaTest.test_passivo_sem_linha_ancora_fora_da_celula_porta_pergunta` | C | app/chamados/services/tarefa_do_dia.py:669 | LU-07/M5: exigir `('porta_pergunta', (b.pk,), {b.pk:'validar'})`, conservando passivo/`nao_sei`. |
| `PortasDaTarefaTest.test_porta_fila_quando_ancora_e_da_celula` | C | app/chamados/services/tarefa_do_dia.py:669 | LU-07/M7: exigir `('porta_app', (), {ch.pk:'cobrar'}, 'colab')`. |
| `PortasDaTarefaTest.test_v8_R10_fora_do_fato_conserva_ancora` | C | app/chamados/services/tarefa_do_dia.py:669 | LU-07/M5: exigir `('porta_fila', (b.pk,), {b.pk})`, mantendo ações vazias. |
| `PortasDaTarefaTest.test_v8_R4_encerrada_ancora_externa` | C | app/chamados/services/tarefa_do_dia.py:681 | LU-07/M5: exigir `fila=(b.pk,)` e acrescentar `acao=None, ausencia=None` ao dict esperado. |
| `PortasDaTarefaTest.test_v8_R8_escritor_real_sem_celula_dia_forte` | C | app/chamados/services/tarefa_do_dia.py:681 | LU-07/M5: exigir `fila=(ch.pk,)`, `acao=None, ausencia=None`, preservando a ligação pelo dia forte. |
| `PortasDaTarefaTest.test_v8_R9_ancora_com_nao_sei` | C | app/chamados/services/tarefa_do_dia.py:681 | LU-07/M5: exigir `fila=(b.pk,)`, `acao='validar', ausencia=None`, mantendo `nao_sei=True`. |
| `F3Beco1NaListaTest.test_RED_F6_C9_lavrado_divergente_nao_tira_o_dia_CADASTRO_da_mesa` | R | app/escala/services/cadastro_realidade.py:988 | Consultar o dono atual mesmo após excluir C11: o contraexemplo deixa C9=`BATIDA` antigo e o atalho conserva esse valor, embora o dia deva continuar `CADASTRO`. |
| `Selo14JuizEmLote.test_lote_responde_o_mesmo_que_o_avulso_e_com_queries_constantes` | Q | app/chamados/services/tarefa_do_dia.py:589 | Substituir igualdade do total de queries por orçamento da base mais consultas plurais por colaborador sem porta, mantendo a paridade dos resultados. |
| `SeloDeterminismoERelogio.test_lote_tem_queries_constantes` | Q | app/chamados/services/tarefa_do_dia.py:589 | Separar custo constante da base do custo adicional do calendário por colaborador e conferir memo/releituras sem nova consulta. |
| `FioMortoCenso.test_RED_censo_dono_fora_do_fio_le_uma_instancia_do_colab` | R | app/chamados/services/vigia_fio.py:347 | Consumir o gerador lendo/semeando `c.colaborador` antes de avançar: materializá-lo antes dessa leitura deixa três células do mesmo colaborador com três instâncias. |
| `AlimentacaoUnicaDoCensoAlcance.test_RED_fora_da_folha_semeia_o_mapa_com_os_retidos_que_carregou` | Q | app/chamados/services/tarefa_do_dia.py:589; app/ponto/motor_calculo_v2.py:1240 | Preservar zero refetch por PK e alimentar/reaproveitar colaborador e memos no caminho adicional do calendário; a associação exata das duas SQL a esse sítio é **[não verificado]**. |
| `AlimentacaoUnicaDoCensoAlcance.test_RED_o_que_segura_semeia_o_mapa_com_o_colaborador_pedido` | Q | app/chamados/services/tarefa_do_dia.py:589; app/ponto/motor_calculo_v2.py:1240 | Preservar zero refetch por PK e corrigir a alimentação do caminho adicional do calendário; a associação exata das duas SQL a esse sítio é **[não verificado]**. |
| `VigiaDoFioMortoTest.test_MORDE_a_property_viva_LE_o_juiz_e_NAO_tem_regra_propria` | C | app/chamados/services/tarefa_do_dia.py:293; app/chamados/services/vigia_fio.py:368 | LU-02/07/M7: manter a delegação property/C6, mas nestes fatos sem vínculo exigir ambas sem porta, `fio_morto=2` e `fio_vivo=0`. |
| `VigiaDoFioMortoTest.test_MORDE_folga_CONTESTADA_e_validada_e_ESCALA_NAO_DESCREVE_O_DIA_e_nao_fio_morto` | C | app/chamados/services/tarefa_do_dia.py:630; app/chamados/services/vigia_fio.py:259 | LU-03/M1/M7: sem âncora, marco exigível ou oferta, incluir `cel_n` no morto e exigir `(escala_nao_descreve_o_dia,fio_morto,fio_vivo)=(1,2,0)`. |
| `VigiaDoFioMortoTest.test_MORDE_o_carimbo_guarda_os_CONTADORES_que_a_rodada_mediu` | C | app/chamados/services/tarefa_do_dia.py:637 | LU-02/07: manter igualdade carimbo/censo e trocar `fio_morto>=1` por zero nestes fatos, pois a alimentação real conserva residual `decidir` com porta. |
| `VigiaDoFioMortoTest.test_MORDE_o_fio_vivo_e_o_fio_morto_dao_CONTAS_DIFERENTES` | C | app/chamados/services/tarefa_do_dia.py:293; app/chamados/services/vigia_fio.py:368 | LU-02/07/M7: exigir ambos mortos, `fio_morto=2`, `fio_vivo=0`, pois C6 vivo sozinho não fornece instância `responder` exigível. |
| `VigiaDoFioMortoTest.test_MORDE_sem_dia_de_fio_morto_a_PORTA_nao_e_varrida` | C | app/chamados/services/tarefa_do_dia.py:293; app/chamados/services/vigia_fio.py:368 | LU-02/07/M7: nestes fatos exigir `fio_vivo=0`, `fio_morto=1`; para conservar a prova de não varredura, fabricar efetivamente uma porta positiva. |
| `VigiaDoFioMortoTest.test_as_PARTES_SOMAM_o_total_e_o_fino_e_subconjunto` | C | app/chamados/services/tarefa_do_dia.py:656 | LU-03/07/M4/M7: exigir `fio_vivo=1`, pois somente Resolver abre porta nesta mistura, preservando as igualdades de partição. |
| `JuizDaPortaAlcancavelTest.test_MORDE_membro_da_fila_que_a_particao_nao_viu_nao_abre_porta` | C | app/chamados/services/tarefa_do_dia.py:656 | LU-02 + LU-03/M4: manter que verbo `None` não dá dono residual, mas exigir Folha `admin/porta_acao/resolver_dia` pela oferta independente. |
| `JuizDaPortaAlcancavelTest.test_registrado_na_celula_nao_e_porta` | C | app/chamados/services/tarefa_do_dia.py:656 | LU-03/M4: conservar C17 histórico sem porta e exigir Folha `admin/porta_acao/resolver_dia`. |
| `FuroSemCanalTest.test_MORDE_chamado_TERMINAL_nao_e_canal` | C | app/chamados/services/tarefa_do_dia.py:656 | LU-03/M4: excluir a célula da amostra sem canal, pois o terminal não impede a porta independente de Resolver. |
| `FuroSemCanalTest.test_a_celula_sem_canal_entra_no_contador` | C | app/chamados/services/tarefa_do_dia.py:656 | LU-03/M4: exigir exclusão da amostra e presença em `com_porta_acao`, apesar da ausência de pergunta/chamado. |
| `FuroSemCanalTest.test_esperado_e_zero_e_o_contador_nao_e_juiz` | C | app/chamados/services/furo_sem_canal.py:104 | LU-03/M4: manter `esperado=0` e exigir `total=0` nesta entrada; total positivo precisa de contraprova sem oferta. |
| `FuroSemCanalTest.test_o_contador_separa_furo_de_cobrado` | C | app/chamados/services/furo_sem_canal.py:105 | LU-03/M4: exigir `por_veredito={}` nesta entrada e testar a separação furo/cobrado sobre dias efetivamente sem porta. |
| `FuroSemCanalTest.test_pergunta_aberta_crua_SEM_porta_da_tarefa_nao_e_canal` | C | app/chamados/services/tarefa_do_dia.py:656 | LU-03/M4: manter que o carimbo cru não decide canal, mas exigir exclusão da amostra quando Resolver publica porta. |
| `HaikuForaDaFolhaTest.test_o_dia_que_acusa_diz_o_veredito_e_o_que_DESTRAVA` | C | app/chamados/services/tarefa_do_dia.py:656; app/folha/fora_da_folha.py:249 | LU-03/M4: exigir fio `admin`, ação/link `resolver_dia`, destrava administrativo e grupo A. |
| `GoldenDonoDoBloqueadoTest.test_GOLDEN_quantos_bloqueados_dependem_SO_do_admin_na_competencia` | C | app/chamados/services/tarefa_do_dia.py:656 | LU-03/M4: exigir fios `{'admin':2}`, preservando os dois bloqueados no balde Admin. |
| `GoldenDonoDoBloqueadoTest.test_MORDE_a_GOLDEN_o_dono_ANDA_quando_o_fio_do_dia_muda` | C | app/chamados/services/tarefa_do_dia.py:656; app/chamados/services/tarefa_do_dia.py:637 | LU-03/07/M4: exigir a transição `admin/Resolver → chamado/porta_fila → admin/porta_fila`, mantendo paridade ferramenta/retrato. |

Nos controles do vigia, **C não significa que C6 esteja errado**: a regra nova exige conjuntamente instância `responder` e pergunta pendente no app; `_dia` fabrica célula/pergunta sem vínculo, e o marco sem teto termina em `marco_fora_do_turno`, sem essa ação. Não é erro de assinatura/forma de dublê, portanto não cabe F na definição fornecida. (app/chamados/services/tarefa_do_dia.py:43; app/chamados/tests/test_o261_vigia_fio.py:87; app/chamados/services/tarefa_do_dia.py:293)

Nos dois testes de queries constantes, **M8 promete uma chamada plural por colaborador sem porta, memoizada; não promete total de SQL constante entre quantidades diferentes de colaboradores**. A base conserva sua promessa de consultas constantes, mas o caminho novo monta calendários por colaborador, podendo ainda dividir janelas longas. (app/chamados/services/tarefa_do_dia.py:20; app/chamados/tests/e3_lu2_entrega.txt:49; app/colaboradores/services/calendario.py:128)

Nos dois N1, os chamadores já passam os colaboradores à carga; alterar apenas essa semente não explica nem corrige necessariamente as SQL observadas. O caminho adicional verificável é calendário → `folha_manda` → totais da folha → `fechamento_lido` → apuração, cujo motor pode buscar o colaborador por PK; atribuir **exatamente as duas SQL do anexo** a esse caminho permanece **[não verificado]**, pois não há pilha das queries. (app/folha/fora_da_folha.py:181; app/folha/fora_da_folha.py:239; app/colaboradores/services/calendario.py:583; app/relatorios/cartao_pela_celula.py:73; app/ponto/services/fechamento.py:1759; app/ponto/motor_calculo_v2.py:1240)

**P1 — assinatura e propagação de `agora`.**

| função/caminho | recebimento e repasse |
|---|---|
| `resolver_dia_oferecido_nas_datas(colaborador, datas, *, agora=None)` | Recebe da tarefa e repassa ao contexto. (app/chamados/services/tarefa_do_dia.py:589; app/colaboradores/services/calendario.py:117; app/colaboradores/services/calendario.py:134) |
| `contexto_calendario(colaborador, GET, *, datas_em_aberto=None, agora=None)` | Usa o instante recebido para hoje, desfazer e D4; o default lê `timezone.now()`. (app/colaboradores/services/calendario.py:159; app/colaboradores/services/calendario.py:163; app/colaboradores/services/calendario.py:250; app/colaboradores/services/calendario.py:606) |
| `censo` e `censo_alcance` → `ListaDoAdmin` | Os dois repassam o instante à lista; o construtor e o repasse seguinte já existiam segundo a entrega. (app/chamados/services/vigia_fio.py:332; app/chamados/services/vigia_fio.py:573; app/chamados/services/vigia_fio.py:194; app/chamados/tests/e3_lu2_entrega.txt:52) |
| Comando `vigiar_fio_morto` | Lê o instante e envia à lista e aos dois censos. (app/chamados/management/commands/vigiar_fio_morto.py:171; app/chamados/management/commands/vigiar_fio_morto.py:175; app/chamados/management/commands/vigiar_fio_morto.py:180) |

Há **omissões produtivas**: `dataset_furos` recebe `agora` e passa ao D4, mas o omite no calendário do card; o drawer também lê um instante próprio e o omite ao chamar o calendário. A view chama o contexto pelo default. A entrega preserva esses defaults; serem regressões introduzidas neste commit é **[não verificado]**. (app/relatorios/furos.py:103; app/relatorios/furos.py:123; app/relatorios/furos.py:160; app/colaboradores/services/drawer.py:19; app/colaboradores/services/drawer.py:121; app/colaboradores/views.py:1949; app/chamados/tests/e3_lu2_entrega.txt:51)

Há também um repasse interno incompleto: o contexto chama `_pv(_q)` sem `agora`, embora C6 o aceite; o caminho de janela viva pode então cair em `date.today()`. Portanto a propagação até a plural/contexto não certifica ausência de relógio interno em toda a cadeia. (app/colaboradores/services/calendario.py:476; app/chamados/juizes.py:862; app/chamados/juizes.py:592)

Os dublês que quebram no dado são os três de teste B/L/E. Dublê produtivo quebrado ou chamador dinâmico adicional: **[não verificado]**. (app/chamados/tests/test_e3_bloqueantes.py:68; app/chamados/tests/test_e3_leitores_c17_chamadores.py:66; app/chamados/tests/test_e4_backfill.py:36)

**P2 — `fila`, rota por rota.** O “antes” abaixo é a expectativa antiga preservada nos testes, corroborada pelo mutante que recoloca `da_celula`; equivalência exata com a fonte de `de702cb2f`: **[não verificado]**. (app/chamados/tests/e3_degrau_mutantes.py:117; app/chamados/tests/e3_degrau_mutantes.py:178)

| rota | antes evidenciado | depois pela regra |
|---|---|---|
| `porta_app` | Chamado local podia aparecer: `(a.pk,)` ou `(ch.pk,)`. (app/chamados/tests/test_e3_portas_tarefa.py:53; app/chamados/tests/test_e3_portas_tarefa.py:83) | `()`: o dono é colaborador, a porta é o app. (app/chamados/services/tarefa_do_dia.py:650; app/chamados/services/tarefa_do_dia.py:669) |
| `porta_pergunta` | Âncora externa publicava `()`. (app/chamados/tests/test_e3_portas_tarefa.py:192; app/chamados/tests/test_e3_portas_tarefa.py:299) | Inclui âncora na fila pertencente ao dono: `(b.pk,)`. (app/chamados/services/tarefa_do_dia.py:644; app/chamados/services/tarefa_do_dia.py:669) |
| `porta_fila` | Chamados locais; externa/dia forte podia resultar em `()`. (app/chamados/tests/test_e3_portas_tarefa.py:254; app/chamados/tests/test_e3_portas_tarefa.py:291; app/chamados/tests/test_e3_portas_tarefa.py:309) | Portas do dono, incluindo externa/dia forte e excluindo residual de outro dono. (app/chamados/services/tarefa_do_dia.py:618; app/chamados/services/tarefa_do_dia.py:646; app/chamados/services/tarefa_do_dia.py:669) |
| `porta_acao` | Existência/forma na base: **[não verificado]**. | Ação administrativa alcançável sem chamado na fila; `fila=()`. (app/chamados/services/tarefa_do_dia.py:655; app/chamados/services/tarefa_do_dia.py:669) |
| `None` | Valor histórico exato: **[não verificado]**. | Sem dono publicável, `fila=()` nos casos `nada/nao_sei`. (app/chamados/services/tarefa_do_dia.py:648; app/chamados/services/tarefa_do_dia.py:669) |

M5 explicita a filtragem: residual `101:cobrar` e ação administrativa por `202:validar` resultam em `porta_pergunta`, `fila=(202,)`, embora ambos permaneçam em `verbos`. (app/chamados/tests/test_e3_degrau_dono_e_porta.py:551; app/chamados/services/tarefa_do_dia.py:624)

Quem lê o conteúdo em produção: `o_que_segura` transforma `fila` em `chamado`; `diff_e3_sombra` compara e conta seus elementos. `porta_da_tarefa` apenas transporta o campo. (app/folha/fora_da_folha.py:247; app/fol

[truncado em 20000 de 22722 caracteres]

## 14/11 20:43 · sessao 1b8bfc9ca4a2 · - · - · origem=Ronald (tmux)

Para as duas falhas N1, **Q**: conservar a exigência de nenhuma busca unitária do colaborador (`app/chamados/tests/test_n1_alimentacao_unica.py:169`; `:183`) e corrigir a alimentação/reutilização no caminho que o calendário acrescentou (`app/chamados/services/tarefa_do_dia.py:589`; `app/colaboradores/services/calendario.py:583`).

O caminho compatível com o SELECT completo por PK é:

`calendario.py:583` → `cartao_pela_celula.py:323` → `cartao_pela_celula.py:73` → `fechamento.py:1759` → builder `fechamento.py:724` → `fechamento.py:286` → `motor_calculo_v2.py:2618` → `:1294` → `:1273` → **SELECT em `:1240`**.

O builder do fechamento alimenta células e ausência, mas retorna sem preencher os memos de previsto/intervalo (`app/ponto/services/fechamento.py:750`–`:752`); o builder do espelho preenche esses memos (`app/ponto/services/espelho.py:1151`–`:1158`), portanto a chamada anterior do calendário à autoridade em `app/colaboradores/services/calendario.py:570` não basta para explicar nem evitar esse segundo caminho.

Atribuir **cada uma das duas SQL fornecidas** especificamente a `motor_calculo_v2.py:1240`: **[não verificado]**, porque o anexo não contém stack das SQL. O caminho estático e a forma da query sustentam essa inferência.

M8 promete uma consulta plural por colaborador necessário e memo na carga (`app/chamados/tests/e3_lu2_entrega.txt:26`; `:49`), não custo total constante em queries; a própria entrega deixa o custo em queries sem verificação (`app/chamados/tests/e3_lu2_entrega.txt:50`).

## 14/11 20:43 · sessao 4a4316c60eb6 · JUSTIFICATIVA-PAGA-O-PERIODO · construtor · origem=Code

Cura escrita na cópia. Evidência local: **47 testes sem banco verdes**, **17/17 mutantes c7 mortos** e **17/17 mutantes v2 mortos**, com controle verde antes de cada runner. Certificação pela suite/opus e resultados ORM: **[nao verificado]**. Saídas: [testes:50](ponto/tests/evidencia_justificativa_c7_verde.txt:50), [c7:36](ponto/tests/evidencia_justificativa_c7_mutantes.txt:36), [v2:19](ponto/tests/evidencia_justificativa_c7_v2_mutantes.txt:19).

| Item | Alteração e testemunha |
|---|---|
| **A1** | Retirada a guarda geométrica anterior à fórmula. O saldo é calculado independentemente de `cobre`: `ponto/services/efeito_ausencia.py:122`. Noturno paga 60; cobertura vazia também tem testemunha: `ponto/tests/test_justificativa_soma_sem_banco.py:345,353`. |
| **A2** | Toda aprovação permanece em `justificadas`; trabalho acima do previsto mantém zero explícito no feed: `ponto/services/efeito_ausencia.py:47`. T2 verifica cobertura, dispensa e crédito zero: `ponto/tests/test_justificativa_soma_sem_banco.py:364`. |
| **A3** | Dia sem trabalho sai de `min_justificados`, permitindo ao motor pagar/contar o dia inteiro. Com trabalho, permanece o saldo: `ponto/services/efeito_ausencia.py:48`; `ponto/motor_calculo_v2.py:2628`. O contador da lavratura distingue ausência inteira de saldo em trabalho: `ponto/services/dia_pago.py:690`. A propriedade período = pagamento integral do motor está em `ponto/tests/test_justificativa_soma_sem_banco.py:406`. |
| **A4** | O predicado pelo bruto e o campo público `motivo` foram eliminados após perderem o leitor com A5. T1 exige 361 e ausência desse campo: `ponto/tests/test_justificativa_soma_sem_banco.py:424`. O `motivo` local restante explica a conservação da âncora: `ponto/services/efeito_ausencia.py:162`. |
| **A5** | Removidos filtro, projeção, apresentação e testes exclusivos da segunda lista: `ponto/views.py:1714`; `templates/ponto/fila_justificativas.html:57`; censo em `ponto/tests/evidencia_justificativa_c7_auditoria.txt:32`. |
| **A6** | O grep da base confirmou o medidor sem chamador de produção. Removidos ele, geometria/bruto tornados órfãos e testes exclusivos. Grep anterior e posterior preservados: `ponto/tests/evidencia_justificativa_c7_auditoria.txt:1,32`. |
| **A7** | `feed_entrega_bruto` altera o **feed real** e é morto pela asserção `{dia: 361}` sobre `efeitos_da_ausencia`: `ponto/tests/mutantes_justificativa_soma.py:40`; `ponto/tests/test_justificativa_soma_sem_banco.py:431`. |
| **A8** | Renomeados os testes da propriedade e do ramo isolado de DSR; corrigidos cabeçalho e contratos ORM: `ponto/tests/test_justificativa_soma_sem_banco.py:163,255`; `ponto/tests/test_justificativa_paga_o_periodo.py:1`. |
| **A9** | Retirada a chamada financeira incondicional de `grade_da_celula`. Agora os leitores financeiros fornecem o mapa: `escala/services/leitor_celula.py:369`; `folha/export.py:340`; `ponto/services/espelho.py:521`. A chamada original está preservada em `ponto/tests/evidencia_justificativa_c7_auditoria.txt:17`. Retorno aos tetos 144/218: **[nao verificado]**. |

O RED foi executado antes da alteração de produção: registrou saldo `0 != 60`, desaparecimento de T2, dia sem trabalho no mapa parcial, contador `1 != 0` e consulta financeira pelo leitor de lâmpadas. Saída integral em [evidencia_justificativa_c7_red.txt:13](ponto/tests/evidencia_justificativa_c7_red.txt:13).

Sobre falta/DSR em T2:

- **Motor:** `datas_trabalho` nasce dos períodos; falta exige ausência tanto de trabalho quanto de justificativa: `ponto/motor_calculo_v2.py:2581,2607`. O ramo comercial conserva essa proteção: `ponto/motor_calculo_v2.py:3490`.
- **Grade:** a saída continua coberta porque a sobreposição deixou de esvaziar `cobre`; o overlay dispensa o marco e transporta zero: `ponto/services/efeito_ausencia.py:165`; `escala/utils.py:1258,1277`.
- **Espelho:** recebe o resultado da autoridade e transporta suas datas de falta: `ponto/services/espelho.py:1229,1254`. A comparação dos contadores completos de DSR antes/depois está escrita para execução ORM: `ponto/tests/test_justificativa_paga_o_periodo.py:541`; resultado **[nao verificado]**.

Sobre a HE de T2: **483 trabalhados / 450 previstos, isoladamente, não garante entrada**. Com janela ligada, entram as pontas reconhecidas conforme piso, saída ativa e extra declarada: `ponto/janela_he.py:204`; `ponto/services/espelho.py:725`. Com janela desligada, `_he_pendencias` permanece vazia: `ponto/services/espelho.py:724`. O retrato ignora lista vazia e a Gestão lê esse retrato: `ponto/services/he_pendente_lavrado.py:132`; `ponto/services/gestao_he.py:236`. Encaminhamento concreto em produção: **[nao verificado]**.

O diff de produção ficou nestes arquivos e funções:

| Arquivo::função | Resumo |
|---|---|
| `ponto/services/efeito_ausencia.py::periodos_justificados/_cobertura_de_uma/efeitos_da_ausencia` | Saldo diário, cobertura preservada e separação entre saldo e abono inteiro: linhas 28,72,132. |
| `ponto/turnos.py` | Exclusão dos helpers órfãos; censo em `ponto/tests/evidencia_justificativa_c7_auditoria.txt:32`. |
| `ponto/views.py::fila_justificativas` e template | Exclusão da revisão: `ponto/views.py:1714`. |
| `ponto/services/dia_pago.py::dias_com_abono` | Contagem de dias inteiros: linha 690. |
| `escala/services/leitor_celula.py::grade_da_celula` | Recebe o mapa financeiro por argumento: linha 369. |
| `folha/export.py::grade_do_fechamento` e `ponto/services/espelho.py::montar_dias` | Fornecem o mapa à grade financeira: linhas 340 e 521. |
| Testes e runners | REDs, contratos corrigidos e mutações em memória: `ponto/tests/test_justificativa_soma_sem_banco.py:345`; `ponto/tests/mutantes_justificativa_soma.py:95`; `ponto/tests/mutantes_justificativa_periodo_v2.py:84`. |

O contrato dos campos ficou assim:

| Campo | Quem escreve | Quem lê e significado |
|---|---|---|
| `minutos` | Fonte, fórmula diária: `efeito_ausencia.py:128` | Feed: `efeito_ausencia.py:48`; overlay: `escala/utils.py:1257,1277`; grade financeira: `folha/export.py:341`, `espelho.py:522`. Minutos pagos pela justificativa. |
| `cobre` | Juiz existente e união: `ponto/turnos.py:2727`; `efeito_ausencia.py:119,121` | Overlay: `escala/utils.py:1251,1258`. Dispensa de marcos, independente do pagamento. |
| `motivo` público | Removido | Sem leitor de produção; grep: `evidencia_justificativa_c7_auditoria.txt:32`. |
| `justificadas` | Fonte: `efeito_ausencia.py:47,63` | Fechamento: `fechamento.py:666`; espelho: `espelho.py:1117`; PDF: `pdf_espelho.py:334`; motor: `motor_calculo_v2.py:2607`. Aprovação protege o dia da falta. |
| `min_justificados` | Fonte: `efeito_ausencia.py:48,64` | Motor recebe pelo fechamento/espelho/PDF: `fechamento.py:755,831`; `espelho.py:1137`; `pdf_espelho.py:407`. Contém saldo dos dias com trabalho, inclusive zero; ausência da chave permite abono inteiro. |
| `dia_inteiro` | Juiz existente: `ponto/turnos.py:2764`; fonte transporta em `efeito_ausencia.py:70` | `espelho.py:1136`, `pdf_espelho.py:406`, motor em `motor_calculo_v2.py:1090`. Ausência inteira que zera descontos de pontualidade. |
| `trabalhado` | Autoridade existente transportada pela fonte: `efeito_ausencia.py:125,127` | Feed em `efeito_ausencia.py:48`. Distingue saldo de dia sem trabalho. |

Censo adicional dos leitores de fechamento, esmeril e comandos: `ponto/tests/evidencia_justificativa_c7_auditoria.txt:36`.

Comandos executados:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest -v -k test_A ponto.tests.test_justificativa_soma_sem_banco

PYTHONDONTWRITEBYTECODE=1 python3 -m unittest ponto.tests.test_justificativa_soma_sem_banco ponto.tests.test_justificativa_periodo_v2_sem_banco -v

PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_soma --todos

PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_periodo_v2 --todos
```

Cada mutante c7 tem seu comando individual e resultado preservados em `ponto/tests/evidencia_justificativa_c7_mutantes.txt:2`.

Para a sessão executar com ORM, **não executado aqui [nao verificado]**:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  ponto.tests.test_justificativa_paga_o_periodo \
  relatorios.test_espelho_falta \
  relatorios.tests.test_s3_cartao_le_dia_pago \
  ponto.tests.test_selo_chokepoint_escrita
```

Nenhum caminho proibido aparece no delta rastreado; sintaxe e whitespace foram conferidos: `ponto/tests/evidencia_justificativa_c7_auditoria.txt:98,102,116`. Medições dos 41, dos nove colaboradores, impacto produtivo, exportadas e contagens reais de queries permanecem **[nao verificado]**. As divergências preexistentes D1/D2 e a janela HE ficaram fora da cura; delta em `ponto/tests/evidencia_justificativa_c7_auditoria.txt:81`.

`LEI-AKITA: origem=aval Ronald 14/11 19:4x (efeito_ausencia.py:76); testemunha=T1/noturno/T2 (test_justificativa_soma_sem_banco.py:185,345,364); RED=evidencia_justificativa_c7_red.txt:13; quem-mais-le=evidencia_justificativa_c7_auditoria.txt:36; juizes novos=0; padrao=single source of truth / um juiz por pergunta.`

## 14/11 20:42 · sessao dfbffc8f5424 · - · - · origem=Ronald (tmux)

Os 16 casos abaixo são **C** pela definição estrita de F: nenhuma destas falhas decorre de assinatura de dublê; as expectativas tratam C6 vivo ou ausência de chamado acionável como suficientes para decidir a porta, enquanto a lei exige a tarefa publicada e admite Resolver como ação direta. (`app/chamados/services/tarefa_do_dia.py:43`, `app/chamados/services/tarefa_do_dia.py:46`, `app/chamados/tests/e3_lu2_entrega.txt:22`)

| Teste | Classe | Sítio que decide | O que mudar | [nao verificado]
|---|---|---|---|
| `test_MORDE_o_fio_vivo_e_o_fio_morto_dao_CONTAS_DIFERENTES` | C | `app/chamados/services/tarefa_do_dia.py:293`; `app/chamados/services/vigia_fio.py:368` | LU-02/07/M7: com os fatos sem vínculo desta fixture, esperar ambos mortos, `fio_morto=2`, `fio_vivo=0`; C6 vivo sozinho não constitui instância `responder` exigível (`app/chamados/tests/test_o261_vigia_fio.py:145`, `app/chamados/tests/test_o261_vigia_fio.py:87`). |
| `test_MORDE_a_property_viva_LE_o_juiz_e_NAO_tem_regra_propria` | C | `app/chamados/services/tarefa_do_dia.py:293`; `app/chamados/services/vigia_fio.py:368` | LU-02/07/M7: conservar os asserts de delegação C6/property, mas esperar ambas as células sem porta e `fio_morto=2`, `fio_vivo=0` nos fatos atuais sem vínculo (`app/chamados/tests/test_o261_vigia_fio.py:249`). |
| `test_MORDE_folga_CONTESTADA_e_validada_e_ESCALA_NAO_DESCREVE_O_DIA_e_nao_fio_morto` | C | `app/chamados/services/tarefa_do_dia.py:293`; `app/chamados/services/tarefa_do_dia.py:630`; `app/chamados/services/vigia_fio.py:259` | LU-03/M1/M7: a resposta não validada sem âncora, vínculo ou oferta não abre porta; esperar `cel_n` no morto e contagens `(escala_nao_descreve_o_dia,fio_morto,fio_vivo)=(1,2,0)` (`app/chamados/tests/test_o261_vigia_fio.py:110`, `app/chamados/tests/test_o261_vigia_fio.py:657`, `app/chamados/tests/test_o261_vigia_fio.py:662`). |
| `test_MORDE_o_carimbo_guarda_os_CONTADORES_que_a_rodada_mediu` | C | `app/chamados/services/tarefa_do_dia.py:637`; `app/chamados/services/tarefa_do_dia.py:640` | LU-02/07: o command usa a alimentação real, cujo residual `decidir` dá porta; substituir o requisito `fio_morto>=1` por zero nesses fatos, preservando a igualdade carimbo/censo (`app/chamados/tests/test_o261_vigia_fio.py:797`, `app/chamados/tests/test_o261_vigia_fio.py:812`, `app/chamados/tests/test_o261_vigia_fio.py:831`). |
| `test_MORDE_sem_dia_de_fio_morto_a_PORTA_nao_e_varrida` | C | `app/chamados/services/tarefa_do_dia.py:293`; `app/chamados/services/vigia_fio.py:368` | LU-02/07/M7: a pergunta C6 viva sem marco exigível não garante porta; nos fatos presentes esperar `fio_vivo=0`, `fio_morto=1`, e construir um caso separado com instância responder alcançável para conservar o propósito de custo (`app/chamados/tests/test_o261_vigia_fio.py:460`). |
| `test_as_PARTES_SOMAM_o_total_e_o_fino_e_subconjunto` | C | `app/chamados/services/tarefa_do_dia.py:293`; `app/chamados/services/tarefa_do_dia.py:656` | LU-03/07/M4/M7: nesta mistura somente Resolver constitui porta, portanto esperar `fio_vivo=1`, mantendo as igualdades de partição e os demais baldes (`app/chamados/tests/test_o261_vigia_fio.py:674`, `app/chamados/tests/test_o261_vigia_fio.py:684`, `app/chamados/tests/test_o261_vigia_fio.py:686`). |
| `test_MORDE_chamado_TERMINAL_nao_e_canal` | C | `app/chamados/services/tarefa_do_dia.py:656` | LU-03/M4: terminal não cria porta residual, mas Resolver oferecido cria `admin/porta_acao/resolver_dia`; retirar a inclusão obrigatória dessa célula na amostra sem canal (`app/chamados/tests/test_o66_furo_sem_canal.py:55`, `app/chamados/tests/test_o66_furo_sem_canal.py:117`). |
| `test_a_celula_sem_canal_entra_no_contador` | C | `app/chamados/services/tarefa_do_dia.py:656` | LU-03/M4: ausência de pergunta/chamado não impede Resolver; esperar a célula fora da amostra e contabilizada em `com_porta_acao` (`app/chamados/tests/test_o66_furo_sem_canal.py:41`, `app/chamados/tests/test_o66_furo_sem_canal.py:95`). |
| `test_esperado_e_zero_e_o_contador_nao_e_juiz` | C | `app/chamados/services/furo_sem_canal.py:104` | LU-03/M4: manter `esperado=0`, mas aceitar `total=0` nesta fixture cujos quatro dias ganharam porta; exigir total positivo requer contraprova sem oferta (`app/chamados/tests/test_o66_furo_sem_canal.py:134`, `app/chamados/services/tarefa_do_dia.py:656`). |
| `test_o_contador_separa_furo_de_cobrado` | C | `app/chamados/services/furo_sem_canal.py:105` | LU-03/M4: esperar `por_veredito={}` nestes fatos com portas e reservar a separação furo/cobrado para uma entrada sem oferta (`app/chamados/tests/test_o66_furo_sem_canal.py:128`). |
| `test_pergunta_aberta_crua_SEM_porta_da_tarefa_nao_e_canal` | C | `app/chamados/services/tarefa_do_dia.py:656` | LU-03/M4: preservar que `respondida_em=None` cru não decide canal, mas esperar exclusão da amostra quando o juiz publica a porta independente de Resolver (`app/chamados/tests/test_o66_furo_sem_canal.py:98`). |
| `test_MORDE_membro_da_fila_que_a_particao_nao_viu_nao_abre_porta` | C | `app/chamados/services/tarefa_do_dia.py:637`; `app/chamados/services/tarefa_do_dia.py:656` | LU-02 + LU-03/M4: manter que verbo `None` não dá dono residual, mas substituir o fio Folha esperado por `admin`, com `porta_acao/resolver_dia` oferecido independentemente desse membro (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:292`). |
| `test_registrado_na_celula_nao_e_porta` | C | `app/chamados/services/tarefa_do_dia.py:656` | LU-03/M4: conservar o C17 histórico sem porta e esperar Folha `admin/porta_acao/resolver_dia`, pois registrado não impede a oferta (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:200`). |
| `test_o_dia_que_acusa_diz_o_veredito_e_o_que_DESTRAVA` | C | `app/chamados/services/tarefa_do_dia.py:656`; `app/folha/fora_da_folha.py:249` | LU-03/M4: substituir `nada/SEM PORTA/grupo D` por `admin`, ação `resolver_dia`, respectivo link e grupo A (`app/folha/tests/test_haiku_fora_da_folha.py:74`, `app/folha/fora_da_folha.py:99`). |
| `test_GOLDEN_quantos_bloqueados_dependem_SO_do_admin_na_competencia` | C | `app/chamados/services/tarefa_do_dia.py:656` | LU-03/M4: o furo sem chamado já tem Resolver; esperar fios `{'admin':2}`, preservando os dois bloqueados e o balde Admin (`app/folha/tests/test_haiku_fora_da_folha.py:491`). |
| `test_MORDE_a_GOLDEN_o_dono_ANDA_quando_o_fio_do_dia_muda` | C | `app/chamados/services/tarefa_do_dia.py:656`; `app/chamados/services/tarefa_do_dia.py:637` | LU-03/07/M4: atualizar a transição para `admin/Resolver → chamado/porta_fila → admin/porta_fila`, preservando paridade ferramenta/retrato e o balde Admin (`app/folha/tests/test_haiku_fora_da_folha.py:517`, `app/folha/tests/test_haiku_fora_da_folha.py:522`, `app/folha/tests/test_haiku_fora_da_folha.py:535`). |

P3: nos três casos `'nada' != 'admin'`, quem decide é a segunda passagem: tarefas sem rota são enviadas ao juiz plural (`app/chamados/services/tarefa_do_dia.py:588`); oferta verdadeira acrescenta ação `resolver_dia` e publica `admin/porta_acao` (`app/chamados/services/tarefa_do_dia.py:656`). O predicado da autoridade exige último recurso, passado e status em `falta/irregular/aberto/sem_certidao` (`app/colaboradores/services/calendario.py:94`, `app/colaboradores/services/calendario.py:103`). A Folha apenas lê o fio publicado (`app/folha/fora_da_folha.py:85`). Pela lei M4, **admin tem razão nestes fatos**, inclusive nos dois casos cujo C17 histórico continua sem porta ou cujo residual tem verbo `None` (`app/chamados/tests/e3_lu2_entrega.txt:22`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:545`).

A principal ressalva interpretativa nos quatro controles positivos do vigia é explícita: a lei pede **instância com responder + pergunta pendente no app**, não apenas `pergunta_viva=True`; o cenário sem vínculo perde a primeira condição em `marco_fora_do_turno` (`app/chamados/services/tarefa_do_dia.py:43`, `app/chamados/services/tarefa_do_dia.py:293`). Modernizar a fixture para fabricar um vínculo real preservaria o objetivo original, mas classificá-la F ampliaria a definição restrita de F dada pelo usuário. (`app/chamados/tests/test_o261_vigia_fio.py:87`)

## 14/11 20:41 · sessao 6a39842ebef1 · - · - · origem=Ronald (tmux)

Sete **C** e uma **R** neste recorte, pela definição de `fila` como portas do dono e pela preservação explícita de F6 na entrega (`app/chamados/services/tarefa_do_dia.py:41`; `app/chamados/tests/e3_lu2_entrega.txt:117`).

| Teste | Classe | Sítio que decide | O que mudar | [nao verificado]
|---|---|---|---|
| `PortasDaTarefaTest.test_fila_A_pergunta_sob_B` | C | `app/chamados/services/tarefa_do_dia.py:669` | LU-07/M5: exigir `('porta_app', (), {a.pk:'decidir', b.pk:'cobrar'}, 'colab')`; a pergunta pendente no app pertence ao colaborador (`app/chamados/tests/test_e3_portas_tarefa.py:53`; contraprova atualizada `:427`). |
| `PortasDaTarefaTest.test_passivo_sem_linha_ancora_fora_da_celula_porta_pergunta` | C | `app/chamados/services/tarefa_do_dia.py:669` | LU-07/M5: exigir `('porta_pergunta', (b.pk,), {b.pk:'validar'})`, preservando passivo/`nao_sei` (`app/chamados/tests/test_e3_portas_tarefa.py:190`; contraprova `:609`). |
| `PortasDaTarefaTest.test_porta_fila_quando_ancora_e_da_celula` | C | `app/chamados/services/tarefa_do_dia.py:669` | LU-07/M7: exigir `('porta_app', (), {ch.pk:'cobrar'}, 'colab')` (`app/chamados/tests/test_e3_portas_tarefa.py:83`; lei `app/chamados/services/tarefa_do_dia.py:43`). |
| `PortasDaTarefaTest.test_v8_R10_fora_do_fato_conserva_ancora` | C | `app/chamados/services/tarefa_do_dia.py:669` | LU-07/M5: exigir `('porta_fila', (b.pk,), {b.pk})`, preservando ações vazias (`app/chamados/tests/test_e3_portas_tarefa.py:309`; contraprova `:625`). |
| `PortasDaTarefaTest.test_v8_R4_encerrada_ancora_externa` | C | `app/chamados/services/tarefa_do_dia.py:669` | LU-07/M5: exigir `fila=(b.pk,)` e incluir `acao=None, ausencia=None` na projeção esperada (`app/chamados/tests/test_e3_portas_tarefa.py:254`; contraprova `:550`; projeção `app/chamados/services/tarefa_do_dia.py:681`). |
| `PortasDaTarefaTest.test_v8_R8_escritor_real_sem_celula_dia_forte` | C | `app/chamados/services/tarefa_do_dia.py:669` | LU-07/M5: exigir `fila=(ch.pk,)`, `acao=None, ausencia=None`, preservando porta/fio do chamado ligado pelo dia forte (`app/chamados/tests/test_e3_portas_tarefa.py:291`; contraprova `:598`; ligação `app/chamados/services/tarefa_do_dia.py:618`). |
| `PortasDaTarefaTest.test_v8_R9_ancora_com_nao_sei` | C | `app/chamados/services/tarefa_do_dia.py:669` | LU-07/M5: exigir `fila=(b.pk,)`, `acao='validar', ausencia=None`, preservando `nao_sei=True` (`app/chamados/tests/test_e3_portas_tarefa.py:299`; contraprova `:609`). |
| `F3Beco1NaListaTest.test_RED_F6_C9_lavrado_divergente_nao_tira_o_dia_CADASTRO_da_mesa` | R | `app/escala/services/cadastro_realidade.py:988` | Preservar a consulta ao dono atual do dia mesmo quando LU-03 exclui C11: o exemplo deixa somente C9 lavrado `BATIDA` e o atalho de assinatura única conserva o dono antigo, contrariando `CADASTRO` exigido e prometido na entrega (`app/escala/tests/test_f3_beco1_folga_na_lista.py:287`; `app/escala/services/cadastro_realidade.py:469`; `app/chamados/tests/e3_lu2_entrega.txt:117`). |

P2: a comparação histórica exata com `de702cb2f` permanece **[não verificado]**: a cópia não contém `.git`, e o executor de base busca fontes por `git show`, sem embuti-las (`app/chamados/tests/e3_degrau_mutantes.py:178`). A evidência de antes disponível são as expectativas antigas dos testes e o mutante que repõe `da_celula` (`app/chamados/tests/e3_degrau_mutantes.py:117`).

| Rota | Antes documentado pelos testes | Depois | [nao verificado]
|---|---|---|
| `porta_app` | Chamados locais, exemplos `(a.pk,)` e `(ch.pk,)` (`app/chamados/tests/test_e3_portas_tarefa.py:53`; `:83`). | `()` porque o dono é colaborador (`app/chamados/services/tarefa_do_dia.py:669`). |
| `porta_pergunta` | `()` para âncora externa (`app/chamados/tests/test_e3_portas_tarefa.py:192`; `:299`). | Âncoras na fila pertencentes ao dono, exemplos `(b.pk,)` (`app/chamados/services/tarefa_do_dia.py:644`; `:669`). |
| `porta_fila` | Chamados da célula; âncora externa/dia forte podia publicar `()` (`app/chamados/tests/test_e3_portas_tarefa.py:254`; `:291`; `:309`). | Portas vivas do dono, incluindo externa/dia forte; exclusão de residual de outro dono (`app/chamados/services/tarefa_do_dia.py:646`; `:669`; M5 `app/chamados/tests/test_e3_degrau_dono_e_porta.py:551`). |
| `porta_acao` | Rota anterior na base: **[não verificado]**. | Ação administrativa sem chamado na fila; `fila=()` (`app/chamados/services/tarefa_do_dia.py:655`; `:669`). |
| `None` | Valor anterior exato: **[não verificado]**. | Sem porta publicada e `fila=()` nos casos sem dono (`app/chamados/services/tarefa_do_dia.py:648`; `:669`). |

Leitores efetivos de `fila`: `o_que_segura` converte a tupla em `chamado` para o dia (`app/folha/fora_da_folha.py:247`; `:254`); `diff_e3_sombra` compara a projeção e imprime sua quantidade (`app/chamados/management/commands/diff_e3_sombra.py:50`; `:143`). `porta_da_tarefa` transporta o campo (`app/chamados/services/tarefa_do_dia.py:681`).

P4: a mensagem exata nasce em `motivo_de` quando o módulo não pertence a `MOTIVOS` nem aos aposentados (`app/chamados/catalogo/modulos.py:208`; `:216`). O caminho de abertura consulta o catálogo, captura esse `KeyError` e registra `abrir_fora_da_lei`; o comentário declara que a emissão continua (`app/chamados/models.py:988`; `:994`; `:995`; `:997`). O módulo concreto e o traceback dessa ocorrência não aparecem no dado dos 55: **[não verificado]**. O relato do diff não lista esses dois arquivos entre as alterações, mas a confirmação histórica de que o código não mudou nesta obra é **[não verificado]** (`app/chamados/tests/e3_lu2_entrega.txt:129`).

## 14/11 20:41 · sessao 1b8bfc9ca4a2 · - · - · origem=Ronald (tmux)

Os **26 ERROR são F no ponto exibido**, pela propagação prevista em M8 (`app/chamados/tests/e3_lu2_entrega.txt:26`; `app/chamados/tests/test_e3_degrau_dono_e_porta.py:609`): a produção chama a plural com `agora=self.agora` em `app/chamados/services/tarefa_do_dia.py:589`, e os três dublês antigos rejeitam esse keyword (`app/chamados/tests/test_e3_bloqueantes.py:68`; `app/chamados/tests/test_e3_leitores_c17_chamadores.py:66`; `app/chamados/tests/test_e4_backfill.py:36`). Eventuais falhas após corrigir o dublê: [não verificado].

Formas exatas usadas nas linhas abaixo:

- **B:** `lambda _c, datas, *, agora=None: dict.fromkeys(datas, False)` em `app/chamados/tests/test_e3_bloqueantes.py:68`.
- **L:** `lambda _c, datas, *, agora=None: dict.fromkeys(datas, False)` em `app/chamados/tests/test_e3_leitores_c17_chamadores.py:66`.
- **E:** `_juiz(sim)` deve retornar `lambda _colab, datas, *, agora=None: dict.fromkeys(datas, sim)` em `app/chamados/tests/test_e4_backfill.py:36`; o patch herdado está em `app/chamados/tests/test_e4_backfill.py:74`.

| teste | classe | sítio que decide | o que mudar | [nao verificado]
|---|---|---|---|
| `BloqueantesE3Test.test_MORDE_8_chamado_TERMINAL_segue_sem_canal` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e3_bloqueantes.py:195` | Atualizar o dublê para B. |
| `BloqueantesE3Test.test_MORDE_8_o_vigia_nao_le_o_predicado_cru` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e3_bloqueantes.py:201` | Atualizar o dublê para B. |
| `ChamadoresDoC17LeemATarefaTest.test_MORDE_nenhum_chamador_migrado_pergunta_ao_C17` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e3_leitores_c17_chamadores.py:184` | Atualizar o dublê para L. |
| `ChamadoresDoC17LeemATarefaTest.test_RED_dias_sem_fio_nao_inclui_dia_so_fila` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e3_leitores_c17_chamadores.py:154` | Atualizar o dublê para L. |
| `ChamadoresDoC17LeemATarefaTest.test_RED_fora_da_folha_o_dia_vai_ao_grupo_C` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e3_leitores_c17_chamadores.py:135` | Atualizar o dublê para L. |
| `ChamadoresDoC17LeemATarefaTest.test_RED_o_que_segura_chamado_da_celula_sem_tarefa_tem_fio` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e3_leitores_c17_chamadores.py:124` | Atualizar o dublê para L. |
| `ChamadoresDoC17LeemATarefaTest.test_controle_dias_sem_fio_pergunta_na_fila_tem_fio` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e3_leitores_c17_chamadores.py:159` | Atualizar o dublê para L. |
| `ChamadoresDoC17LeemATarefaTest.test_controle_fora_da_folha_pergunta_na_fila_e_grupo_B` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e3_leitores_c17_chamadores.py:143` | Atualizar o dublê para L. |
| `ChamadoresDoC17LeemATarefaTest.test_controle_o_que_segura_pergunta_na_fila_e_colab` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e3_leitores_c17_chamadores.py:131` | Atualizar o dublê para L. |
| `BackfillE4bTest.test_backfill_2x_mesmo_estado_uma_trilha` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4_backfill.py:150` | Atualizar `_juiz` para E. |
| `BackfillE4bTest.test_backfill_dry_usa_porta_e_nao_persiste` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4_backfill.py:119` | Atualizar `_juiz` para E. |
| `BackfillE4bTest.test_exportada_nao_muda_hash_fm_txt` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4_backfill.py:169` | Atualizar `_juiz` para E. |
| `BackfillE4bTest.test_sem_competencia_so_lista_e_nao_chama_porta` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4_backfill.py:137` | Atualizar `_juiz` para E. |
| `PassivoE4bTest.test_classe_c_passivo_entra_pela_porta_da_E1_e_2x_e_uma_linha` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4_passivo.py:57` | Atualizar `_juiz` herdado para E. |
| `PassivoE4bTest.test_classe_e_viva_sob_terminal_reabre_pela_porta_com_nota` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4_passivo.py:72` | Atualizar `_juiz` herdado para E. |
| `PassivoE4bTest.test_classes_e_universos_nao_somem_para_zerar` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4_passivo.py:98` | Atualizar `_juiz` herdado para E. |
| `PassivoE4bTest.test_mutante_sem_porta_morde_caracterizacao_classe_e` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4_passivo.py:92` | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_Ch12_P90_D90_exportada_desfaz` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4b_c4_orm.py:446` | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_Ch12_P90_D90_fora_da_competencia_K6_false_desfaz` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4b_c4_orm.py:346` | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_DRY_diff_por_colaborador_le_dinheiro_da_autoridade` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4b_c4_orm.py:550` | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_callback_desconhecido_nomeado_e_fila_revertida` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4b_c4_orm.py:171` | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_callback_real_de_retomada_desfaz` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4b_c4_orm.py:149` | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_escrita_EventoCelula_por_porta_aparece_no_diff` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4b_c4_orm.py:202` | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_foto_inteira_sob_C_e_reversao_antes_do_primeiro_ato` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4b_c4_orm.py:509` | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_lote_misto_desfaz_recusado_enquanto_vizinho_pousa` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4b_c4_orm.py:349` | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_tabela_nao_fotografada_desfaz` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4b_c4_orm.py:189` | Atualizar `_juiz` herdado para E. |

P1:

- As assinaturas novas documentadas são `resolver_dia_oferecido_nas_datas(colaborador, datas, *, agora=None)` (`app/colaboradores/services/calendario.py:117`) e `contexto_calendario(colaborador, GET, *, datas_em_aberto=None, agora=None)` (`app/colaboradores/services/calendario.py:159`); o contrato M8 exige recebimento/reenvio e contexto sem nova leitura de relógio (`app/chamados/tests/e3_lu2_entrega.txt:26`; `app/chamados/tests/test_e3_degrau_dono_e_porta.py:575`; `app/chamados/tests/test_e3_degrau_dono_e_porta.py:613`).
- Cadeia principal correta: `CargaDaTarefa.tarefa` envia `self.agora` à plural (`app/chamados/services/tarefa_do_dia.py:589`); plural envia `agora` ao contexto (`app/colaboradores/services/calendario.py:132`); contexto usa o instante recebido, só chama `timezone.now()` quando omitido (`app/colaboradores/services/calendario.py:163`), usa o mesmo instante no desfazer (`app/colaboradores/services/calendario.py:250`) e no D4 (`app/colaboradores/services/calendario.py:606`).
- Os dois censos agora enviam o instante ao `ListaDoAdmin` (`app/chamados/services/vigia_fio.py:332`; `app/chamados/services/vigia_fio.py:573`); o construtor já aceita `agora` (`app/chamados/services/vigia_fio.py:177`) e `_carregar` repassa à lista (`app/chamados/services/vigia_fio.py:194`), distinção expressa na entrega (`app/chamados/tests/e3_lu2_entrega.txt:52`). O comando abre o relógio em `app/chamados/management/commands/vigiar_fio_morto.py:171`, envia à lista em `:175` e a ambos os censos em `:180`/`:181`.
- Os demais chamadores do contexto são view (`app/colaboradores/views.py:1949`), drawer (`app/colaboradores/services/drawer.py:121`) e relatório (`app/relatorios/furos.py:160`), todos sem `agora`; a entrega preserva defaults dos demais chamadores (`app/chamados/tests/e3_lu2_entrega.txt:51`).
- Há omissão concreta de um instante já disponível no relatório: `dataset_furos` recebe `agora` (`app/relatorios/furos.py:103`) e o envia ao D4 (`app/relatorios/furos.py:123`), mas não ao calendário do card (`app/relatorios/furos.py:160`/`:164`), cujo default lê outro relógio (`app/colaboradores/services/calendario.py:163`). Drawer também lê `agora` no início (`app/colaboradores/services/drawer.py:19`) e o omite no calendário (`app/colaboradores/services/drawer.py:121`), além de leituras próprias de data (`app/colaboradores/services/drawer.py:43`; `:111`). Serem regressões introduzidas neste commit: [não verificado sem base/diff].
- Não há dublê de produção encontrado entre as referências estáticas dos símbolos; os dublês que quebraram estão nos testes citados. Chamada/dublê dinâmico: [não verificado]. [nao verificado]

## 14/11 20:27 · sessao defe70908db2 · E3-V10-DEGRAU-DONO-E-PORTA · construtor · origem=Code

A [entrega completa]( /home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-192627/app/chamados/tests/e3_lu2_entrega.txt:1) contém diff por função, matriz M1–M8, leitores dos campos, custo por carga, remoções L-111 e classificação dos 65 métodos do recorte.

Execuções locais:

- **159 testes OK**, sem banco. [Saída](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-192627/app/chamados/tests/e3_lu2_suite.txt:167) [nao verificado]
- **45/45 mutantes mortos**, com controle verde. [Saída e comandos individuais](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-192627/app/chamados/tests/e3_lu2_mutantes.txt:1478) [nao verificado]
- **RED anterior à cura preservado**; M7 foi controle verde. [Saída original](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-192627/app/chamados/tests/e3_lu2_red.txt:1) [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_lu2_suite
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_degrau_mutantes --todos
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_degrau_mutantes --base
```

O último comando reproduz os REDs na base e retorna **1 esperado**. [Registro](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-192627/app/chamados/tests/e3_lu2_entrega.txt:8)

Suite ORM completa, frota e timeout de 30 segundos: **[não verificado]**. Certificação do Code e opus final: **[não verificado]**. [nao verificado]

## 14/11 20:22 · sessao 9138e639ec97 · JUSTIFICATIVA-PAGA · verificador · origem=Code

1. **PROCEDENTE-candidato — P2 — dinheiro: duas contas de trabalhado alteram o crédito.** `realizado_do_dia` desconta intervalo fixo não batido; o motor comercial desconta o intervalo efetivamente recebido e mede após a janela HE (`app/ponto/turnos.py:760`; `app/ponto/motor_calculo_v2.py:3236`; `app/ponto/motor_calculo_v2.py:3254`). Cenário sintético: previsto 480, escala 08–17, pausa 12–13, E08/S15 sem pausa, justificativa `outro`, sem recorte HE. A primeira conta dá **360**, portanto crédito **120**; o motor dá **420**, totalizando **540 com o crédito**, acima dos 480. É derivação paralela, transportada para `horas_trabalhadas` (`app/ponto/turnos.py:767`; `app/ponto/services/efeito_ausencia.py:145`; `app/ponto/motor_calculo_v2.py:3268`; `app/ponto/motor_calculo_v2.py:296`). Ocorrência produtiva: **[nao verificado]**.

2. **PROCEDENTE-candidato — P2 — estrutura: o dia também tem duas atribuições.** O realizado agrupa estritamente por `data_turno`; o motor lê esse dia e depois o sobrescreve quando o intervalo entre períodos é de até seis horas (`app/ponto/turnos.py:719`; `app/ponto/motor_calculo_v2.py:2408`; `app/ponto/motor_calculo_v2.py:2413`). Cenário sintético: DNA diurno 08–17, E19/S23 em 26/09 e E03/S07 em 27/09. O pareador mantém dois dias, com 240 minutos cada; a véspera já fechada não recebe o segundo turno (`app/ponto/turnos.py:1311`; `app/ponto/turnos.py:767`). No ramo-base do motor, o segundo período herda 26/09 pelo intervalo de quatro horas; essa atribuição alimenta a presença usada em falta/abono/DSR (`app/ponto/motor_calculo_v2.py:2413`; `app/ponto/motor_calculo_v2.py:2465`; `app/ponto/motor_calculo_v2.py:2583`). Impacto produtivo: **[nao verificado]**.

3. **PROCEDENTE-candidato — P2 — estrutura/texto: cobertura inteira passou a contar zero `dias_abono`.** A fonte inclui `cobre=None` no mapa; o motor continua interpretando presença nesse mapa como motivo para excluir o dia da contagem (`app/ponto/services/efeito_ausencia.py:47`; `app/ponto/motor_calculo_v2.py:2629`). Cenário: `outro`, sem batidas, previsto 480 → **8 horas de abono e zero dias** (`app/ponto/services/efeito_ausencia.py:122`; `app/ponto/services/efeito_ausencia.py:145`; `app/ponto/motor_calculo_v2.py:2630`; `app/ponto/motor_calculo_v2.py:2633`). Espelho/PDF leem esse zero; o resumo pela lavratura conta um dia com minutos abonados positivos: conceitos diferentes para `dias_abono` (`app/ponto/services/espelho.py:1253`; `app/relatorios/pdf_espelho.py:479`; `app/ponto/services/dia_pago.py:701`; `app/relatorios/cartao_pela_celula.py:190`).

4. **PROCEDENTE-candidato — P3 — texto/estrutura: o motivo denuncia excedente que não existe no trabalho.** `pago_mais_trabalhado_passa_do_previsto` testa **bruto + trabalhado**, embora o pagamento final já tenha sido substituído pelo saldo (`app/ponto/services/efeito_ausencia.py:143`; `app/ponto/services/efeito_ausencia.py:145`; `app/ponto/services/efeito_ausencia.py:146`). O próprio cenário col788 do teste tem previsto 420, trabalho 59 e crédito 361, mas recebe o motivo; a Central apresenta “Hora além do turno — revisar HE” (`app/ponto/tests/test_justificativa_soma_sem_banco.py:178`; `app/ponto/tests/test_justificativa_soma_sem_banco.py:185`; `app/templates/ponto/fila_justificativas.html:116`). Nesse cenário, o excedente de trabalho sobre o previsto é **zero**, pela mesma fixture (`app/ponto/tests/test_justificativa_soma_sem_banco.py:180`).

5. **PROCEDENTE-candidato — P3 — estrutura: `revisao` é segunda lista, sem integração com a porta HE.** O filtro monta uma projeção por motivo; esse ramo não grava decisão HE (`app/ponto/views.py:1738`; `app/ponto/views.py:1747`; `app/ponto/views.py:1753`). O caminho existente é **pontas fora dos marcos → espelho → retrato → Gestão HE**, e já recebe os dias com pontas reconhecidas (`app/ponto/janela_he.py:204`; `app/ponto/services/espelho.py:727`; `app/ponto/services/he_pendente_lavrado.py:132`; `app/ponto/services/gestao_he.py:236`).

   Cenário de duplicação: janela ativa, previsto 480, pausa de uma hora, E07:50/S17:10 contra marcos 08–17, justificativa `outro`: as pontas entram na Gestão e o motivo também entra na Central. Uma decisão “Não” é lida pela Gestão, mas não participa do predicado da Central, que permanece baseado no bruto/trabalhado (`app/ponto/janela_he.py:205`; `app/ponto/janela_he.py:207`; `app/ponto/services/gestao_he.py:278`; `app/ponto/services/efeito_ausencia.py:146`).

   **Trabalhado > previsto não garante entrada na HE:** com janela desligada, o espelho mantém a lista vazia e o retrato ignora o dia; o novo aviso não fornece encaminhamento à ação existente (`app/ponto/services/espelho.py:721`; `app/ponto/services/espelho.py:722`; `app/ponto/services/he_pendente_lavrado.py:133`; `app/ponto/views.py:1753`). Falta essa ligação com a pendência/decisão existente, cuja porta exige minutos positivos (`app/ponto/portas/he.py:138`; `app/ponto/portas/he.py:169`).

O rastreamento dos campos de **P2** ficou assim:

| Campo | Quem escreve/fornece | Quem lê |
|---|---|---|
| `minutos` | Saldo por `j.data_turno`: `app/ponto/services/efeito_ausencia.py:118`, `:145` | Feed `app/ponto/services/efeito_ausencia.py:47`; grade `app/escala/utils.py:1257`, `:1277` |
| `cobre` | Juiz, conservação e união: `app/ponto/services/efeito_ausencia.py:208`, `:129`, `:133` | Inclusão no feed `app/ponto/services/efeito_ausencia.py:46`; dispensa dos marcos `app/escala/utils.py:1251`, `:1267` |
| `motivo` | Literal do excedente: `app/ponto/services/efeito_ausencia.py:147`; motivo da âncora vira `conserva`: `:129` | Projeção `app/ponto/services/efeito_ausencia.py:153`; Central `app/ponto/views.py:1750`; apresentação `app/templates/ponto/fila_justificativas.html:116` |
| `min_justificados` | Mapa da fonte `app/ponto/services/efeito_ausencia.py:47`; ausência inteira ABONA remove a chave em `:63` | Leitor da célula `app/escala/services/leitor_celula.py:380`; fechamento `app/ponto/services/fechamento.py:667`; linha da grade → abono `app/ponto/services/dia_pago.py:399` |
| `minutos_justificados` do motor | Fechamento `app/ponto/services/fechamento.py:755`, `:831`; espelho `app/ponto/services/espelho.py:1134`; PDF `app/relatorios/pdf_espelho.py:407`; esmeril `app/ponto/services/esmeril_espelho.py:166`; comandos `app/ponto/management/commands/diff_espelho_alimentacao.py:56`, `app/relatorios/management/commands/diff_extrato_parcial.py:62` | Ramo de abono `app/ponto/motor_calculo_v2.py:2628`, com conversão minutos→horas em `:2634` |
| `dias_abono` / `horas_abono` | Motor `app/ponto/motor_calculo_v2.py:2630`, `:2633`; alternativa gravada: `app/ponto/services/dia_pago.py:701`, `app/relatorios/cartao_pela_celula.py:186` | Espelho `app/ponto/services/espelho.py:1253`; PDF `app/relatorios/pdf_espelho.py:479`, `:480`; resumo pela lavratura `app/relatorios/cartao_pela_celula.py:190` |

A unidade numérica transportada é minuto, convertido explicitamente para hora; as divergências encontradas estão na **conta do trabalhado, atribuição ao dia e significado de dia abonado** (`app/ponto/services/efeito_ausencia.py:145`; `app/ponto/motor_calculo_v2.py:2634`; achados com fontes acima). Contagens e consequências em produção: **[nao verificado]**.

## 14/11 20:22 · sessao 885796c9d60e · JUSTIFICATIVA-PAGA · verificador · origem=Code

**P5 — PROCEDENTE-candidato · estrutura: revisão sem limite temporal e com consultas por dia.** A UI carrega todas as aprovadas, sujeitas apenas aos filtros opcionais de empresa/nome; por colaborador, calcula entre a menor e a maior `data_turno`, sem janela de competência ou paginação (`app/ponto/views.py:1707`, `:1714`, `:1720`, `:1732`, `:1747`). Cenário: duas aprovadas distantes fazem carregar células e parear batidas de todo o intervalo intermediário (`app/ponto/services/efeito_ausencia.py:109`, `:114`).

A chamada custa **7 consultas fixas + condicionais + consultas por dia coberto**; no ramo com carga de DNA e célula com geradora, **8 + 2D**, excluindo a consulta inicial e os cards da UI. As sete vêm das três cargas da fonte e quatro do carregador de turnos; o DNA acrescenta uma (`app/ponto/services/efeito_ausencia.py:98`, `:109`, `:110`; `app/ponto/turnos.py:1879`, `:1900`, `:1903`, `:1955`, `:1609`, `:1925`). O **2D** ocorre porque `realizado_do_dia` busca novamente vínculo/célula e intervalo por dia, apesar da carga anterior (`app/ponto/turnos.py:781`, `:788`, `:794`; `app/escala/servico_jornada.py:46`; `app/escala/models.py:1182`). A configuração declara `gthread` e `timeout=30`; estouro efetivo em produção **[nao verificado]** (`app/gunicorn.conf.py:19`, `:21`).

**P6 — PROCEDENTE-candidato · estrutura: comparação sensível ao relógio interno.** `recalcular_fechamento_mes` não recebe `agora`, colhe `timezone.now()` e usa esse instante para filtrar furos (`app/ponto/services/fechamento.py:448`, `:616`, `:1120`). Cenário: dia sem par e sem aberta viva, fim previsto às 19h; a classificação muda entre 18:59 e 19:01, sem mudança nas batidas (`app/ponto/turnos.py:2132`, `:2140`). Atribuição aos nove colaboradores **[nao verificado]**.

Não vi relógio novo nos blocos do delta c5+c6: fonte, leitura do previsto, overlay e revisão (`app/ponto/services/efeito_ausencia.py:93`; `app/escala/services/leitor_celula.py:354`; `app/escala/utils.py:1257`; `app/ponto/views.py:1738`). Novidade dos demais relógios desde `14b61c124` **[nao verificado]**.

**P7 — uma linha por classe:**

- **Segundo juiz registrado — PROCEDENTE-candidato · estrutura:** o registro aponta `minutos_previstos_do_dia`, mas o novo helper prefere a ata; cenário sintético ata=440, marcos 12:50–21:20 sem pausa: helper=440, registrado=510. A justificativa passa a consumir esse caminho (`app/core/juizes.py:185`; `app/escala/services/leitor_celula.py:361`; `app/escala/utils.py:2288`, `:2292`; `app/ponto/services/efeito_ausencia.py:138`).
- **Segundo escritor — não vi** nos trechos lidos: fonte consulta e produz dicionários; overlay modifica grade em memória; persistência de `DiaPago` permanece em `lavrar` (`app/ponto/services/efeito_ausencia.py:98`; `app/escala/utils.py:1251`; `app/ponto/services/dia_pago.py:359`).
- **Relógio fora da borda — PROCEDENTE-candidato · estrutura:** instante interno sem parâmetro para repetir a comparação; cenário de passagem pelo fim do turno descrito em P6 (`app/ponto/services/fechamento.py:448`, `:616`; `app/ponto/turnos.py:2140`).
- **Derivação paralela — PROCEDENTE-candidato · estrutura:** permanece o medidor antigo que declara responder quanto a justificativa “paga”, sustentado por testes; previsto480, E16 sem saída, hf17: ele responde60, a fonte responde480 — classe residual vedada pela L-111 (`app/ponto/turnos.py:2769`, `:2779`; `app/ponto/services/efeito_ausencia.py:145`; `app/ponto/tests/test_justificativa_paga_o_periodo.py:49`; `app/docs/LEIS.md:137`).
- **Selo/RED sem a mordida anunciada — PROCEDENTE-candidato · estrutura:** `feed_motor_le_bruto` altera o mapa da fonte; sua testemunha executa o alimentador do PDF com fábrica mockada, sem executar o leitor do motor. Prova emissão do mapa bruto, não consumo pelo motor (`app/ponto/tests/mutantes_justificativa_soma.py:30`; `app/ponto/tests/test_justificativa_soma_sem_banco.py:248`, `:254`).
- **Campo com dois sentidos — PROCEDENTE-candidato · texto:** `motivo` nasce de **bruto + trabalhado**, mas a tela o apresenta como trabalho “além do turno”; justificativa `outro`, previsto480 e trabalho180 dentro do turno gera motivo com pagamento300, sem demonstrar HE real (`app/ponto/services/efeito_ausencia.py:122`, `:143`, `:145`, `:146`; `app/templates/ponto/fila_justificativas.html:116`).

**P8 — PROCEDENTE-candidatos · texto:**

- **DSR anunciado além do trecho executado:** `test_bloco_real_motor_abona_361_sem_falta_ou_perda_dsr` verifica `dsr_ok` num ramo isolado; não executa nem confere os contadores semanais. Uma alteração na contabilização posterior fica fora do trecho selecionado (`app/ponto/tests/test_justificativa_soma_sem_banco.py:267`, `:276`, `:277`; `app/ponto/motor_calculo_v2.py:2641`, `:2667`).
- **Propriedade com nome falso para parte do próprio domínio:** “coberto implica soma dentro do previsto” exclui da checagem pagamentos zero. Com trabalho120 e previsto100, a soma continua120; o teste aceita pela fórmula vigente (`app/ponto/tests/test_justificativa_soma_sem_banco.py:163`, `:168`, `:172`, `:174`).
- **Docstrings contradizem as asserções atuais:** o cabeçalho ainda diz C5=690, sem âncora não justifica e D1=420; os testes atuais exigem720, cobertura inteira conservada e510. D3 diz “nada se cobre”, mas exige cobertura inteira e477 pagos (`app/ponto/tests/test_justificativa_paga_o_periodo.py:17`, `:18`, `:20`, `:458`, `:484`, `:565`, `:509`, `:516`).

## 14/11 20:22 · sessao c246f3ad8879 · JUSTIFICATIVA-PAGA · verificador · origem=Code

1. **PROCEDENTE-candidato — P1(d), dinheiro: a guarda de sobreposição impede pagar o saldo devido.** Ela esvazia `cobre` antes da fórmula; depois, `periodos_justificados` só calcula o saldo quando existe cobertura. Assim, ainda é possível devolver **0 onde faltam 60 minutos**. Fontes: `app/ponto/turnos.py:2764`, `app/ponto/services/efeito_ausencia.py:218`, `app/ponto/services/efeito_ausencia.py:137`.

Os quatro casos abaixo são sintéticos, sem intervalo cadastrado; “crédito” significa minutos pagos pela justificativa. A referência é a fórmula literal registrada em `app/ponto/services/efeito_ausencia.py:83`.

| P1 | Cadastro, batidas e justificativa | REGRA em minutos | Código |
|---|---|---|---|
| **(a)** | Previsto **480**, 08h–16h; nenhuma batida; `esqueci_saida` aprovada, sem âncora nem saída informada. | Trabalho 0; crédito **480**. | `cobre=None`, crédito **480**, `conserva='sem_ancora'`. Caminho: `app/ponto/turnos.py:2694`, `app/ponto/services/efeito_ausencia.py:129`, `app/ponto/services/efeito_ausencia.py:145`. |
| **(b)** | Previsto **420**, 06h–13h; E03:54/S04:53; E06:00/S12:30/E12:45; `esqueci_saida` aprovada ancorada em E06, sem saída informada. | Trabalho **59+390=449**; crédito **0**; excedente **29** para revisão. | Crédito **0**, `cobre={'hf'}`, motivo de revisão. Soma dos turnos: `app/ponto/turnos.py:719`, `app/ponto/turnos.py:770`; saldo e motivo: `app/ponto/services/efeito_ausencia.py:145`. |
| **(c)** | Previsto **480**, 08h–16h; E08/S17; nenhuma justificativa; janela HE vigente, piso 0 e saída ativa, sem autorização. | Crédito **0**; excedente **60** para decisão do admin. | Justificativas devolvem `{}`; os **60** seguem pela pendência HE existente, independentemente de justificativa. Fontes: `app/ponto/services/efeito_ausencia.py:105`, `app/ponto/janela_he.py:206`, `app/ponto/janela_he.py:228`, `app/ponto/services/espelho.py:727`. |
| **(d)** | Previsto **480**, **22h–06h**; E18 aberta; E22/S05 do dia seguinte; `esqueci_saida` aprovada ancorada em E18, sem saída informada. | Trabalho **420**; crédito **60**, pertencente ao dia inicial. | O trecho 22h–06h cruza o trabalho 22h–05h: retorna **`cobre=frozenset(), minutos=0`** e revisão `{}`. **Divergência: −60 minutos.** Fontes: `app/escala/utils.py:346`, `app/ponto/turnos.py:2738`, `app/ponto/turnos.py:2764`, `app/ponto/services/efeito_ausencia.py:137`, `app/ponto/services/efeito_ausencia.py:153`. |

2. **PROCEDENTE-candidato — P4, estrutura: cobertura vazia apaga a condição de justificado e também desaparece da revisão.** Exemplo: previsto **480**, 08h–16h; E01:10 aberta e E10/S19 fechadas (**540 trabalhados**); justificativa aprovada ancorada em E01:10. A sobreposição produz `cobre` vazio; o dia sai de `justificadas`, perde o mapa explícito de zero e não recebe `motivo` nem `conserva`. A grade deixa eventuais marcos faltantes sem dispensa. Fontes: `app/ponto/turnos.py:2764`, `app/ponto/services/efeito_ausencia.py:46`, `app/ponto/services/efeito_ausencia.py:137`, `app/ponto/services/efeito_ausencia.py:153`, `app/escala/utils.py:1251`, `app/escala/utils.py:1258`.

A distinção entre zero, ausência de valor e mapa vazio fica assim:

| Leitor | Tratamento |
|---|---|
| `efeitos_da_ausencia` | **Zero não exclui o dia**; quem decide é `cobre`. `cobre=None` conserva cobertura inteira; `cobre` vazio exclui o dia dos dois mapas. `app/ponto/services/efeito_ausencia.py:46`. |
| `_marcar_ausencias` | Copia crédito **0** quando `cobre` é inteiro ou preenchido. Com cobertura vazia, nenhum ramo marca o dia como justificado. `app/escala/utils.py:1251`, `app/escala/utils.py:1257`, `app/escala/utils.py:1277`. |
| Motor | **`{dia: 0}` distingue-se de `{}`**: testa presença da chave e soma zero. `None` no mapa inteiro vira `{}`. Ausência da chave só aciona abono integral se o dia estiver em `_abono`. `app/ponto/motor_calculo_v2.py:2628`, `app/ponto/motor_calculo_v2.py:2634`. |
| Grade da célula | Preserva zero pela presença da data no mapa. `app/escala/services/leitor_celula.py:448`. |
| `por_dia_da_grade` | O teste de verdade reúne crédito **0**, `None` e valor ausente: não cria entrada de abono; esse trecho não decide DSR. `app/ponto/services/dia_pago.py:398`. |

**Falta/DSR adicional no caso todo trabalhado: não demonstrado.** Mesmo removido de `datas_justificadas`, o dia reconhecido em `datas_trabalho` escapa da falta e mantém o DSR pelos predicados lidos; o ramo comercial também exige ausência dos dois conjuntos. Fontes: `app/ponto/motor_calculo_v2.py:2605`, `app/ponto/motor_calculo_v2.py:2610`, `app/ponto/motor_calculo_v2.py:3490`. Ocorrência produtiva de desconto ou perda de DSR por esse caminho: **[nao verificado]**.

## 14/11 20:21 · sessao 513b12f5fdae · - · - · origem=Ronald (tmux)

Cenário sintético **executado em memória** com `parear_turnos` e `realizado_dos_turnos` reais, extraídos por AST:

- DNA diurno 08–17; intervalo 12–13.
- 26/09: E19/S23.
- 27/09: E03/S07.
- Pareador devolveu dois turnos: `data_turno=26/09` e `data_turno=27/09`; realizado: **240 minutos em cada dia** (`app/ponto/turnos.py:719`, `app/ponto/turnos.py:767-771`).

O cenário evita absorção como pausa porque o intervalo entre S23 e E03 é de quatro horas, acima do teto de três horas (`app/ponto/turnos.py:382`, `app/ponto/turnos.py:1484-1491`). A véspera já tem turno fechado e não recebe o segundo turno (`app/ponto/turnos.py:1311-1314`).

O motor começa lendo o dia do juiz, mas **sobrescreve o dia do segundo período por 26/09**, pois o gap é de quatro horas, dentro de `0..6` (`app/ponto/motor_calculo_v2.py:2408-2413`). Portanto, há derivação paralela também da atribuição ao dia: realizado separa os dois dias; o motor os reúne no primeiro (`app/ponto/turnos.py:719`, `app/ponto/motor_calculo_v2.py:2411-2414`).

Execução completa do motor e ocorrência produtiva: **[nao verificado]**.

## 14/11 20:20 · sessao 090dd8640d32 · - · - · origem=Ronald (tmux)

**P2 — PROCEDENTE-candidato, severidade estrutura/texto: `dias_abono` passou a excluir também cobertura inteira sem trabalho.** A fonte agora coloca todos os dias cobertos no mapa, inclusive `cobre=None` (`app/ponto/services/efeito_ausencia.py:47`), mas o motor ainda usa **ausência no mapa** para contar dias abonados (`app/ponto/motor_calculo_v2.py:2629`, `app/ponto/motor_calculo_v2.py:2630`). Cenário sintético: justificativa `outro` sem batidas, previsto 480 → fonte paga 480 (`app/ponto/services/efeito_ausencia.py:122`, `app/ponto/services/efeito_ausencia.py:145`); motor produz **8 horas de abono e 0 dias**, embora o campo signifique “dias previstos sem trabalho mas justificados” (`app/ponto/motor_calculo_v2.py:284`, `app/ponto/motor_calculo_v2.py:2633`). Executei somente esse ramo real por AST em memória e obtive esse resultado; ocorrência produtiva [nao verificado].

O espelho transporta esse zero (`app/ponto/services/espelho.py:1253`); PDF também (`app/relatorios/pdf_espelho.py:479`), e oculta o badge quando zero (`app/relatorios/pdf_espelho.py:787`). Já o ramo de lavratura usa outra definição de `dias_abono`: conta linhas com `minutos_abonados>0` (`app/ponto/services/dia_pago.py:701`, `app/relatorios/cartao_pela_celula.py:190`). Assim, a mesma justificativa pode contar zero pelo motor e um pela lavratura; o ramo de escolha está em `app/relatorios/cartao_pela_celula.py:341`.

Mapa solicitado, para consolidar:

| Campo | Escritor/fornecedor | Leitores |
|---|---|---|
| `minutos` | Inicializado por dia de `j.data_turno`; valor final `max(0, previsto-trabalhado)` (`app/ponto/services/efeito_ausencia.py:118`, `app/ponto/services/efeito_ausencia.py:145`). | Feed `min_justificados` (`app/ponto/services/efeito_ausencia.py:47`); overlays da grade (`app/escala/utils.py:1257`, `app/escala/utils.py:1277`). |
| `cobre` | Resultado de `_cobertura_de_uma`, conservação e união (`app/ponto/services/efeito_ausencia.py:125`, `app/ponto/services/efeito_ausencia.py:129`, `app/ponto/services/efeito_ausencia.py:133`); juiz chamado em `app/ponto/services/efeito_ausencia.py:208`. | Inclusão em justificadas/mapa (`app/ponto/services/efeito_ausencia.py:46`); dispensa de marcos (`app/escala/utils.py:1251`, `app/escala/utils.py:1258`, `app/escala/utils.py:1267`). |
| `motivo` excedente | Comparação **bruto+trabalhado**, seguida do literal (`app/ponto/services/efeito_ausencia.py:146`, `app/ponto/services/efeito_ausencia.py:147`). | Projeção revisão (`app/ponto/services/efeito_ausencia.py:153`); Central (`app/ponto/views.py:1747`, `app/ponto/views.py:1750`); texto HE (`app/templates/ponto/fila_justificativas.html:116`). |
| `min_justificados` | Mapa dia→minutos (`app/ponto/services/efeito_ausencia.py:47`); ausência inteira ABONA remove a chave (`app/ponto/services/efeito_ausencia.py:63`). Linha da grade transporta o valor (`app/escala/services/leitor_celula.py:449`). | Fechamento (`app/ponto/services/fechamento.py:667`); espelho (`app/ponto/services/espelho.py:1134`); PDF (`app/relatorios/pdf_espelho.py:407`); esmeril (`app/ponto/services/esmeril_espelho.py:166`); comandos (`app/ponto/management/commands/diff_espelho_alimentacao.py:56`, `app/relatorios/management/commands/diff_extrato_parcial.py:62`). |
| `minutos_justificados` motor | Nasce `{}` (`app/ponto/motor_calculo_v2.py:813`); alimentado pelos leitores acima e pelos dois motores do fechamento (`app/ponto/services/fechamento.py:755`, `app/ponto/services/fechamento.py:831`). | Ramo de abono usa presença da chave e converte minutos para horas dividindo por 60 (`app/ponto/motor_calculo_v2.py:2628`, `app/ponto/motor_calculo_v2.py:2634`). |
| Abono gravado | `por_dia_da_grade` soma `min_justificados` na data da linha (`app/ponto/services/dia_pago.py:391`, `app/ponto/services/dia_pago.py:399`); fechamento soma mapa em `minutos_abonados` (`app/ponto/services/fechamento.py:1066`, `app/ponto/services/fechamento.py:1067`). | Resumo converte o gravado para horas (`app/relatorios/cartao_pela_celula.py:118`, `app/relatorios/cartao_pela_celula.py:186`). |

Os transportadores examinados mantêm **minutos por chave de data** até a conversão explícita `/60`; a comparação entre a atribuição do dia e o trabalho usado pelo motor ficou com você (`app/ponto/services/efeito_ausencia.py:118`, `app/ponto/services/dia_pago.py:391`, `app/ponto/motor_calculo_v2.py:2634`).

## 14/11 20:20 · sessao d24ad1288d7e · - · - · origem=Ronald (tmux)

- **PROCEDENTE-candidato — estrutura — segundo juiz residual:** `minutos_do_periodo_justificado` continua respondendo “minutos que […] paga” pela duração do trecho, enquanto a fonte paga saldo diário. Cenário: previsto480, E16 sem S, hf17: antigo60, fonte480. A função permanece sustentada por testes, contrariando L-111. (`app/ponto/turnos.py:2769`, `app/ponto/turnos.py:2779`, `app/ponto/services/efeito_ausencia.py:145`, `app/ponto/tests/test_justificativa_paga_o_periodo.py:49`, `app/docs/LEIS.md:137`).
- **PROCEDENTE-candidato — estrutura — previsto com duas autoridades:** registro aponta `minutos_previstos_do_dia`; novo helper prefere ata e só usa o registrado como fallback. Cenário sintético: ata440, marcos12:50–21:20 sem pausa: helper440, registrado510. A prioridade da ata já existia na grade; novidade demonstrada é a extração pública e seu consumo pela justificativa. (`app/core/juizes.py:185`, `app/escala/services/leitor_celula.py:354`, `app/escala/services/leitor_celula.py:361`, `app/escala/utils.py:2288`, `app/escala/utils.py:2292`, `app/ponto/services/efeito_ausencia.py:138`).
- **PROCEDENTE-candidato — texto — excedente hipotético apresentado como HE real:** `motivo` testa bruto+trabalhado, embora o pagamento já tenha virado saldo; a UI imprime “Hora além do turno”. Cenário sintético: justificativa `outro`, previsto480 e180 trabalhados inteiramente dentro do turno: bruto480 gera motivo, pago300, sem demonstrar qualquer trabalho além do turno. (`app/ponto/services/efeito_ausencia.py:122`, `app/ponto/services/efeito_ausencia.py:143`, `app/ponto/services/efeito_ausencia.py:145`, `app/ponto/services/efeito_ausencia.py:146`, `app/templates/ponto/fila_justificativas.html:116`).
- **Segundo escritor: não vi nos trechos lidos.** Fonte consulta e devolve estruturas; overlay altera grade em memória; fechamento recebe o mapa pela derivação existente e `lavrar` persiste `DiaPago`. Censo global de escritores [nao verificado]. (`app/ponto/services/efeito_ausencia.py:98`, `app/escala/utils.py:1251`, `app/escala/utils.py:1277`, `app/ponto/services/fechamento.py:1065`, `app/ponto/services/dia_pago.py:359`).

## 14/11 20:20 · sessao 850decff23c9 · - · - · origem=Ronald (tmux)

**P5 — PROCEDENTE-candidato, severidade estrutura:** `revisao` materializa **todas as aprovadas**, com filtros opcionais de empresa/nome, sem janela de competência nem paginação; para cada colaborador pergunta pelo intervalo entre a aprovação mais antiga e a mais recente. Cenário: duas aprovadas distantes fazem carregar e parear todo o histórico intermediário. (`app/ponto/views.py:1707`, `:1714`, `:1720`, `:1732`, `:1747`; `app/ponto/services/efeito_ausencia.py:98`, `:109`, `:114`.)

**Consultas por colaborador:** não há número universal; são **7 fixas + consultas condicionais + consultas por dia coberto**. No caminho usual com DNA de até um intervalo e célula com geradora em cada dia, a projeção faz **8 + 2D**, sendo D o número de dias cobertos; isso exclui a consulta inicial e os cards da UI. (`app/ponto/services/efeito_ausencia.py:98`, `:109`, `:110`; `app/ponto/turnos.py:1879`, `:1900`, `:1903`, `:1931`, `:1955`; `app/escala/alimentacao.py:148`; `app/ponto/turnos.py:1574`, `:1609`, `:1925`, `:2003`; `app/escala/utils.py:100`.)

O termo **2D** vem de chamar `realizado_do_dia` sem fornecer intervalo/modo: para cada dia ele busca novamente a célula com geradora e depois a pausa em outra consulta, embora `periodos_justificados` já tenha carregado células e vínculos. Cenário sintético: 100 colaboradores × 20 dias cobertos ⇒ **4.800 consultas na projeção usual**, além da UI. (`app/ponto/services/efeito_ausencia.py:109`, `:110`, `:136`, `:140`; `app/ponto/turnos.py:781`, `:788`, `:794`; `app/escala/servico_jornada.py:46`; `app/escala/models.py:1163`, `:1182`.)

Há consultas adicionais condicionais: geradoras ausentes da candidatura, agregação da última batida e fallbacks em dias sem célula/previsto. (`app/escala/alimentacao.py:155`; `app/ponto/turnos.py:1914`; `app/escala/servico_jornada.py:64`.)

A configuração UI declara `gthread`, quatro threads e `timeout = 30`; duração real e estouro em produção **[nao verificado]**. (`app/gunicorn.conf.py:19`, `:20`, `:21`.)

## 14/11 20:20 · sessao 513b12f5fdae · - · - · origem=Ronald (tmux)

1. **PROCEDENTE-candidato — P3 — texto/estrutura: falso “hora além do turno”.** O motivo nasce de `bruto + trabalhado > previsto`, embora o crédito final seja `max(0, previsto - trabalhado)` (`app/ponto/services/efeito_ausencia.py:143-147`). No cenário col788 do teste, há previsto 420, par trabalhado de 59 minutos e crédito de 361; ainda assim recebe esse motivo (`app/ponto/tests/test_justificativa_soma_sem_banco.py:178-188`). A Central imprime “Hora além do turno — revisar HE” mesmo sem trabalho excedendo o previsto (`app/templates/ponto/fila_justificativas.html:116`).

2. **PROCEDENTE-candidato — P3 — estrutura: segunda lista independente da decisão HE.** A Central seleciona motivos da justificativa sem consultar decisão HE (`app/ponto/views.py:1747-1754`); a Gestão de HE consulta decisões vigentes para determinar o estado do dia (`app/ponto/services/gestao_he.py:86-89`, `app/ponto/services/gestao_he.py:278-280`). Portanto, decidir HE não retira o motivo da Central: o predicado continua dependendo de bruto e trabalhado (`app/ponto/services/efeito_ausencia.py:143-147`). É segunda projeção/lista; não há novo escritor persistente nesse ramo (`app/ponto/views.py:1738-1754`).

3. **PROCEDENTE-candidato — P3 — estrutura: aviso não equivale à porta HE.** O caminho existente recebe exclusivamente `he_fora_da_janela`; dia sem essa lista é ignorado pelo retrato (`app/ponto/services/he_pendente_lavrado.py:132-150`). O espelho popula essa lista por pontas fora dos marcos ou dia sem previsto, condicionado à janela ativa e à vigência (`app/ponto/services/espelho.py:721-749`); trabalho maior que previsto não é seu critério geral de entrada (`app/ponto/janela_he.py:194-230`). Cenário sintético: janela desligada e trabalho acima do previsto deixam `he_fora_da_janela=[]`, enquanto a justificativa pode receber motivo e aparecer na Central (`app/ponto/services/espelho.py:721-722`, `app/ponto/services/efeito_ausencia.py:143-147`). Falta ligação desse aviso à pendência/ação existente: o filtro apenas monta a lista (`app/ponto/views.py:1747-1754`), e a porta real exige minutos positivos de pendência (`app/ponto/portas/he.py:138-172`).

Quando existem pontas reconhecidas, o dia já chega pelo caminho anterior: marcação das pontas → espelho → retrato → Gestão HE (`app/ponto/janela_he.py:218-230`, `app/ponto/services/espelho.py:727-742`, `app/ponto/services/he_pendente_lavrado.py:132-150`, `app/ponto/services/gestao_he.py:236-250`). Nessa classe, a Central acrescenta outra lista (`app/ponto/views.py:1738-1754`). Alcance produtivo e contagens: **[nao verificado]**.

_5273 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
