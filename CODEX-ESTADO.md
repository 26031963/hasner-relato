# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 01:34** · publicado em **11/11/2026 01:36** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 01:34 · sessao f3f0a41e4168 · FIO-E0-V4-E3 · sonda · origem=Code

**`lei: PARE` antes da cura:** `col348` tem **61 células `trabalha=False` e nenhuma `True`**, condição expressamente incluída no aval. Isso não prova escala cadastrada errada: o código do ciclo intermitente retorna `False` por desenho. `col574` sai como **CADASTRO**, pois o dado informa `situacao="desligado"`. [não verificado — COLAB col348/col574 e aval fornecidos]; `app/escala/models.py:820-823`.

**Outro impedimento técnico encontrado:** trocar a folga para `concorde` perde a garantia específica de rejulgamento quando chegar a adesão: o gancho atual depende da **palavra** `nunca_bateu`. `app/ponto/services/cartorio.py:1366-1375`.

Os valores da sombra abaixo são transcritos do dado fornecido, sem confirmação independente; por isso cada linha recebe `[não verificado]`. As referências `app/...:linha` são da cópia consultada. [não verificado — origem do dado fornecido]

**1. Censo dos 15**

`ativo:null` não significa inativo: o script usa `getattr(c, 'ativo', None)`, enquanto o cadastro declara `situacao`. O modelo não declara campo `ativo` para o colaborador. `app/colaboradores/models.py:283-380`; [não verificado — script fornecido].

**Zero batidas vivas implica adesão `None` pelo cálculo atual do cartório**, que procura a primeira batida com `retratada_em IS NULL`. Para quem tem batidas vivas, o censo permite afirmar existência de adesão, mas não sua data. `app/ponto/services/cartorio.py:1165-1173`.

| Colab | Situação / ativo | Admissão | Demissão | Isento | Batidas total/vivas → adesão | Fonte |
|---|---|---|---|---|---|---|
| col144 | ativo / null | 2023-04-19 | None | false | 0/0 → None | [não verificado] |
| col209 | ativo / null | 2018-06-16 | None | false | 0/0 → None | [não verificado] |
| col237 | ativo / null | 2024-02-05 | None | false | 82/82 → existe; data não medida | [não verificado] |
| col268 | ativo / null | 2024-08-21 | None | false | 0/0 → None | [não verificado] |
| col312 | ativo / null | 2025-02-13 | None | false | 0/0 → None | [não verificado] |
| col348 | ativo / null | 2025-05-10 | None | false | 62/58 → existe; data não medida | [não verificado] |
| col427 | ativo / null | 2025-10-14 | None | false | 0/0 → None | [não verificado] |
| col574 | desligado / null | 2026-03-12 | 2027-07-06 | false | 0/0 → None | [não verificado] |
| col650 | ativo / null | 2025-09-26 | None | false | 0/0 → None | [não verificado] |
| col969 | ativo / null | 2024-02-07 | None | false | 0/0 → None | [não verificado] |
| col1005 | ativo / null | 2026-10-27 | None | false | 0/0 → None | [não verificado] |
| col1009 | ativo / null | 2026-11-01 | None | false | 0/0 → None | [não verificado] |
| col1018 | ativo / null | 2026-11-04 | None | false | 0/0 → None | [não verificado] |
| col1019 | ativo / null | 2026-11-03 | None | false | 0/0 → None | [não verificado] |
| col1027 | ativo / null | 2026-11-04 | None | false | 0/0 → None | [não verificado] |

Nos vínculos, `∞` representa o `"None"` fornecido. A coluna final identifica **cobertura por vigência**, não comprova qual vínculo a célula nomeia: o script não entrega `escala_geradora_id`. O juiz prioriza a geradora e pode devolver `None` se ela estiver fora da vigência, mesmo existindo outro candidato. `app/escala/alimentacao.py:269-286`.

| Colab | Vínculos: pk; tipo; início→fim; ativa; posto | Cobre todas as datas X1 por vigência | Fonte |
|---|---|---|---|
| col144 | 118; 12x36 07–19; 2026-04-01→2026-06-23; false; 22<br>844; 12x36 flexível (só horas); 2026-06-24→∞; true; 22 | **844** | [não verificado] |
| col209 | 178; 12x36 13–21; 2026-07-21→∞; true; 422 | **178** | [não verificado] |
| col237 | 204; 12x36 07–19; 2026-07-21→∞; true; 487 | **204** | [não verificado] |
| col268 | 234; 12x36 07:30–17:30; 2026-07-21→∞; true; 508 | **234** | [não verificado] |
| col312 | 274; 12x36 flexível (só horas); 2026-07-21→∞; true; 437 | **274** | [não verificado] |
| col348 | 306; mrv sdf · intermitente · 06:30–18:30; 2026-07-21→∞; true; 467 | **306** | [não verificado] |
| col427 | 366; 12x36 19–07; 2026-07-21→∞; true; 448 | **366** | [não verificado] |
| col574 | 489; 6x1 Seg/Ter/Qua/Qui/Sex/Dom 07–15:20; 2024-01-01→2027-07-06; false; 1 | **489**, embora inativo | [não verificado] |
| col650 | 695; 12x36 07:30–17:30; 2026-07-21→∞; true; 449<br>1075; mesmo tipo; **2026-08-26→2026-07-20**; false; 449 | **695**; 1075 tem intervalo invertido | [não verificado] |
| col969 | 1367; 5x2 Seg–Sex 08–17; 2026-09-21→2026-10-17; false; 655<br>1434; TESTE12X36 · 12x36 07–19; 2026-10-18→∞; true; 655 | **1367**: X1 é 26–27/09 e 03–04/10 | [não verificado] |
| col1005 | 1471; 6x1 08:30–17:30; 2026-10-27→∞; true; 657 | **1471** | [não verificado] |
| col1009 | 1501; 6x1prochet · 6x1 Seg–Sab 07–16; 2026-11-01→∞; true; 454 | **1501** | [não verificado] |
| col1018 | 1517; 6x1 Seg–Sab 07–16; 2026-11-04→∞; true; 649 | **1517** | [não verificado] |
| col1019 | 1518; 12x36 06–18; 2026-11-03→∞; true; 454 | **1518** | [não verificado] |
| col1027 | 1527; 12x36 06–18; 2026-11-04→∞; true; 649 | **1527** | [não verificado] |

Um vínculo inativo **com data final** pode cobrir suas datas históricas; `ativa=false` isoladamente não significa SEM VÍNCULO. `app/escala/alimentacao.py:189-216`.

Na próxima tabela, cada distribuição é `veredito:quantidade`; `NB=nunca_bateu`. Os números abrangem **21/09–20/11**, não apenas X1. A retenção usa `HOJE=10/11` no script. [não verificado — script fornecido]

| Colab | Células | `trabalha=False` × veredito | `trabalha=True` × veredito | TRABALHO retido com NB | X1 | Fonte |
|---|---:|---|---|---:|---:|---|
| col144 | 61 | None:6; NB:25 | None:5; NB:25 | 25 | 25 | [não verificado] |
| col209 | 61 | None:6; NB:25 | None:5; NB:25 | 25 | 25 | [não verificado] |
| col237 | 61 | None:5; concorde:24; fato_em_ausencia:1 | None:6; cobrado:1; concorde:24 | 0 | 1 | [não verificado] |
| col268 | 61 | None:6; NB:25 | None:5; NB:25 | 25 | 25 | [não verificado] |
| col312 | 61 | None:6; NB:25 | None:5; NB:25 | 25 | 25 | [não verificado] |
| col348 | 61 | None:11; concorde:41; furo:9 | **nenhuma** | 0 | 4 | [não verificado] |
| col427 | 61 | None:6; NB:25 | None:5; concorde:6; NB:19 | 19 | 24 | [não verificado] |
| col574 | 61 | None:1; NB:7 | None:10; NB:43 | 43 | 7 | [não verificado] |
| col650 | 61 | None:5; NB:25 | None:6; NB:25 | 25 | 25 | [não verificado] |
| col969 | 61 | None:21; NB:4 | None:24; NB:12 | 12 | 4 | [não verificado] |
| col1005 | 25 | None:2; NB:1 | None:9; NB:13 | 13 | 1 | [não verificado] |
| col1009 | 20 | None:2; concorde:2; NB:1 | None:9; concorde:4; NB:2 | 2 | 1 | [não verificado] |
| col1018 | 17 | None:2; NB:1 | None:9; NB:5 | 5 | 1 | [não verificado] |
| col1019 | 18 | None:6; NB:3 | None:5; NB:4 | 4 | 3 | [não verificado] |
| col1027 | 17 | None:5; NB:3 | None:6; NB:3 | 3 | 3 | [não verificado] |

Somam **226 dias de TRABALHO retidos com NB**, antes das exclusões cadastrais. Não são 226 dias X1. [não verificado — soma dos COLAB fornecidos]

**2. Classificação pela condição do aval**

“Entra” abaixo significa que **os campos disponíveis não mostram impedimento cadastral**, sujeito à lacuna da geradora e à identificação de teste. A parada global por `col348` continua valendo. [não verificado — aplicação do aval ao dado fornecido] [nao verificado]

| Colab | Classificação | Critério e campo | Fonte |
|---|---|---|---|
| col144 | **(iii) entra** | `situacao=ativo`, adesão None; vínculo 844 cobre X1; 30 células True, 25 trabalhos NB retidos | [não verificado] |
| col209 | **(iii) entra** | ativo, adesão None; vínculo 178; 30 True, 25 trabalhos NB retidos | [não verificado] |
| col237 | **(iii) sem exclusão cadastral demonstrada**, mas sem alvo NB atual demonstrado | ativo, vínculo 204; `batidas_vivas=82`; **nenhuma célula NB** na janela | [não verificado] |
| col268 | **(iii) entra** | ativo, adesão None; vínculo 234; 30 True, 25 trabalhos NB retidos | [não verificado] |
| col312 | **(iii) entra** | ativo, adesão None; vínculo 274; 30 True, 25 trabalhos NB retidos | [não verificado] |
| col348 | **(ii) PARE** | `trab_x_veredito` contém somente False; tipo informado intermitente | [não verificado] |
| col427 | **(iii) entra** | ativo, adesão None; vínculo 366; 30 True, 19 trabalhos NB retidos | [não verificado] |
| col574 | **(i) CADASTRO — fora da cura** | `situacao=desligado`; a demissão futura informada não autoriza ignorar esse campo | [não verificado] |
| col650 | **(iii) entra pelo vínculo 695** | ativo, adesão None; 31 True, 25 trabalhos NB retidos; vínculo 1075 invertido merece lista cadastral separada | [não verificado] |
| col969 | **TESTE não comprovado; classificação pendente** | `tipo` contém TESTE12X36, empresa 1/posto 655; porém o vínculo que cobre X1 é o 1367, 5x2 | [não verificado] | [nao verificado]
| col1005 | **(iii) entra** | ativo, adesão None; vínculo 1471; 22 True, 13 trabalhos NB retidos | [não verificado] |
| col1009 | **(iii) entra** | ativo, adesão None; vínculo 1501; 15 True, 2 trabalhos NB retidos | [não verificado] |
| col1018 | **(iii) entra** | ativo, adesão None; vínculo 1517; 14 True, 5 trabalhos NB retidos | [não verificado] |
| col1019 | **(iii) entra** | ativo, adesão None; vínculo 1518; 9 True, 4 trabalhos NB retidos | [não verificado] |
| col1027 | **(iii) entra** | ativo, adesão None; vínculo 1527; 9 True, 3 trabalhos NB retidos | [não verificado] |

**Não há SEM VÍNCULO demonstrado pelos intervalos fornecidos**; a autoridade efetiva por célula permanece não medida pela ausência da geradora no relatório. [não verificado — vínculos fornecidos]; `app/escala/alimentacao.py:269-286`.

Sobre TESTE: [nao verificado]

- O colaborador não tem flag `teste`; `situacao` admite ativo, afastado, férias e desligado. `app/colaboradores/models.py:283-380`.
- Existe critério de **empresa de diagnóstico na UI**: CNPJ iniciado por `11222333`. Isso não é uma classificação universal usada pelo export. `app/colaboradores/queries.py:78-87`.
- O raspado fornece apenas IDs de empresa/posto, sem CNPJ, nomes ou praça; portanto **empresa 1/posto 655 não comprovam teste**. [não verificado — campos fornecidos] [nao verificado]
- `TESTE12X36` sugere cadastro de teste **[inferência]**, mas é rótulo de escala; apelido não define sua regra. `app/escala/models.py:111-114`; `app/escala/models.py:420-428`; [não verificado — tipo de col969 fornecido].
- “Flexível (só horas)” em col144/col312 não comprova isenção: o juiz de isenção lê exclusivamente `isento_ponto`. `app/escala/models.py:425-428`; `app/core/constants.py:13-18`.

**3. X1 com ausência**

O resultado fornecido é **1 dia: col237, 2026-11-07**; os outros 14 colaboradores têm zero. A data é identificável porque esse colaborador tem somente essa data em `DADOS`. [não verificado — COLAB col237 e script fornecidos]

Isso demonstra somente **`CelulaDia.ausencia_id` preenchido** em uma célula X1. O script não consulta aprovação, tipo, período nem integralidade da ausência; portanto “ausência aprovada sobreposta” está **[não verificado]**. O campo é uma FK lavrada na célula. `app/escala/models.py:1792-1798`; [não verificado — expressão `x1_com_ausencia` do script].

**4. Leitores e efeito de NB → `concorde`**

A mudança sai do conjunto acusatório porque NB pertence a `VEREDITOS_ACUSAM` e `concorde` não pertence. Os efeitos abaixo são condicionais à execução de cada leitor; não provam que perguntas/chamados já tenham sido alterados na sombra. `app/escala/models.py:1767-1768`; [não verificado — execução na sombra].

| Leitor | Efeito para a folga que passar a `concorde` | Arquivo:linha |
|---|---|---|
| Retenção do export, `motivos_retencao_celula` | O dia deixa de reter por `espelho_nunca_bateu`; também não entra no ramo de veredito ausente | `app/folha/export.py:1233-1256` |
| Vigia de isenção por pausa | Sai do universo acusatório contado como isento pela pausa | `app/folha/services/vigia_pausa.py:155-157` |
| Lista C5, `colabs_nunca_bateu` | O dia sai da lista; o colaborador permanece se tiver outros dias NB elegíveis | `app/escala/services/cadastro_realidade.py:181-200` |
| Universo de adesão | O dia deixa de alimentar `dias/prev/fur`; trabalhos NB restantes continuam no universo | `app/chamados/services/adesao.py:32-40` |
| Predicado `dia_nunca_bateu_sem_adesao` | Passa a retornar falso | `app/chamados/services/fabrica_da_celula.py:87-89` |
| `motivos_a_perguntar` | Sai do retorno exclusivo NB; folga continua sem pergunta por `dia_sem_trabalho`, e `concorde` também não acusa | `app/chamados/services/fabrica_da_celula.py:105-121` |
| `perguntar_pela_celula` | Cai a saída antecipada NB; folga não passa na condição de cadastro sem turno | `app/chamados/services/fabrica_da_celula.py:183-202` |
| `motivo_fora_da_fabrica` | Some a causa `celula_nunca_bateu`; nos demais ramos pode aparecer `pergunta_do_chamado_ja_encerrada` | `app/chamados/services/disputa_emissao.py:758-773` |
| `_resposta_concorda_com_a_luz` | Pode reconhecer concordância, depois das demais verificações de resposta/batida | `app/chamados/juizes.py:1319-1321` |
| `_pergunta_viva_calc` — C6 | Some a sustentação pelo veredito acusador; a pergunta pode deixar de ser viva, respeitadas as outras precedências | `app/chamados/juizes.py:1374-1385` |
| `PerguntaDisputa.q_vivas` | Sai de `_celula_acusa` e pode entrar em `_celula_absolve`; resposta e ata provisória ainda interferem | `app/chamados/models.py:3340-3358` |
| Candidatas a perguntas absolvidas | Passam a poder entrar no universo; C6 e tranca ainda são consultados antes do resultado | `app/chamados/reconciliador.py:641-667` |
| `_absolvido_pela_celula` | Pode permitir retratação do chamado, sem marcos pendentes e respeitando proteção da resposta | `app/chamados/reconciliador.py:990-1029` |
| Filhos que seguram pai de limbo | Filho absolvido deixa de segurar o pai por esse caminho | `app/chamados/services/limbo_folgas.py:191-195` |
| Silêncio por ausência, `_premissa_morre` | Se a premissa do catálogo morreu, o veredito deixa de impedir o encerramento | `app/chamados/services/silencio_ausencia.py:28-41` |
| Recebimento de resposta tardia | Deixa de tentar renascer o chamado pelo ramo de célula acusadora; segue o tratamento tardio | `app/chamados/services/resposta_tardia.py:45-59` |
| Vigia de fio morto | O dia sai das células `acesas` desse universo | `app/chamados/services/vigia_fio.py:331`; `app/chamados/services/vigia_fio.py:378-379` |
| Contador de fio mudo/sem adesão | Deixa de alimentar `_dias_sem_adesao`; folga/concorde não ganha motivo de pergunta por essa troca | `app/chamados/management/commands/disparar_perguntas_competencia.py:111-121` |
| Veto de cobrança por marco | **Continua vetando por FOLGA**, antes da leitura do conjunto que contém NB | `app/ponto/marcos.py:119`; `app/ponto/marcos.py:155-162` |
| Vigia ausência × veredito | Sai do universo acusatório comparado com ausência integral aprovada | `app/ponto/services/vigia_ausencia_veredito.py:41-57` |
| Ausência sem efeito na célula | Deixa de incrementar `nunca_bateu`; ainda pode contar `sem_efeito` por lâmpada apagada | `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:36`; `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:58-63` |
| Relatório dia da frota | **Sem mudança para False**: conta folga e continua antes de testar ACUSAM | `app/relatorios/dia_da_frota.py:47-55` |
| Status da lavra / acusação da parcial | Folga simples sem batida já recebe `folga`; com ausência parcial, retirar NB pode alterar a classificação de irregularidade | `app/ponto/status_dia.py:68-70`; `app/ponto/status_dia.py:104-110`; `app/ponto/status_dia.py:139-162` |

As auditorias também leem o conjunto, sem realizar cura automaticamente:

| Auditoria | Efeito | Arquivo:linha |
|---|---|---|
| Contraprova F1t | Se o item continuar no piso indevidamente, passa a apontar célula que não acusa | `app/chamados/management/commands/auditar_invariantes_chamados.py:198` |
| Contraprova do piso | Folga já fica fora pelo `not c.trabalha` | `app/chamados/management/commands/auditar_invariantes_chamados.py:377` |
| Recorte A4 | Folga continua no piso correspondente por `not trabalha` | `app/chamados/management/commands/auditar_invariantes_chamados.py:1447` |
| C2/C3 | Pode entrar na medição de pergunta morta pela célula ou cobrança viva em célula concorde, conforme os demais filtros | `app/chamados/management/commands/auditar_invariantes_chamados.py:1601-1606` |
| Piso F1t | `concorde` deixa de satisfazer a condição acusatória | `app/chamados/management/commands/auditar_invariantes_chamados.py:1698-1713` |

Leitores derivados relevantes:

- Cadastro × Realidade formata a C5; não possui outra regra para NB. `app/escala/services/cadastro_realidade.py:204-207`; `app/escala/services/cadastro_realidade.py:878`.
- `vigia_retido_calado` usa os **colaboradores** da C5 para roteamento. Havendo trabalho NB restante, a pessoa pode continuar roteada; trocar a célula isoladamente não reescreve o retrato já lavrado. `app/folha/vigia_retido_calado.py:33-48`.
- `vigiar_fio_morto` publica o contador composto, e o pipeline o acompanha com dono CADASTRO. `app/chamados/management/commands/vigiar_fio_morto.py:182`; `app/chamados/management/commands/vigiar_fio_morto.py:241`; `app/core/management/commands/pipeline_placar.py:136`.
- O placar publica o contador de NB em dia coberto. `bin/placar_code.sh:419`.

Não confundir com outros campos homônimos: o `nunca_bateu` dos rankings de furos é calculado por batidas/previstos/furos; `subcategoria='nunca_bateu'` em pós-vínculo é leitura de chamado. `app/escala/services/furos_diarios.py:211-242`; `app/escala/services/furos_diarios.py:283`; `app/escala/services/furos_diarios.py:334`; `app/colaboradores/services/pos_vinculo.py:36`.

**O gancho HX-CARTORIO-NB-REJULGA**

**Não há garantia de que a folga convertida continue sendo rejulgada somente porque a adesão chegou.** O funcionamento atual é:

1. A primeira batida viva escolhe entre ramo sem adesão e ramo normal. `app/ponto/services/cartorio.py:1165-1173`.
2. Sem adesão, qualquer veredito preenchido pode ser considerado em dia se a impressão coincidir. `app/ponto/services/cartorio.py:1207-1216`.
3. Com adesão, a exceção é literalmente `cel.veredito != 'nunca_bateu'`: NB impede o pulo; `concorde` permite o pulo. `app/ponto/services/cartorio.py:1369-1375`.
4. A impressão não contém adesão global; contém os demais insumos do julgamento. Uma batida próxima, outro insumo alterado ou `forcar` pode provocar rejulgamento, mas adesão distante, sozinha, não o garante. `app/ponto/services/cartorio.py:443-522`; `app/ponto/services/cartorio.py:1318-1319`; `app/ponto/services/cartorio.py:1359-1375`.

Portanto, o comentário de `1242-1250` corresponde ao mecanismo implementado: **depende da palavra, no ramo normal; não de uma memória persistida de que a célula passou pelo ramo sem adesão**. O teste existente cobre NB com primeira batida três dias depois; não comprova preservação para folga `concorde`. `app/ponto/services/cartorio.py:1242-1252`; `app/ponto/tests/test_cartorio_nb_readesao.py:11-23`.

A precedência já é chamada no ramo sem adesão, mas seu resultado é sobrescrito para NB quando não há cobertura integral. A porta atual resolve o literal antes do derivador em `923`, não na antiga referência `564` do aval. `app/ponto/services/cartorio.py:1237-1253`; `app/ponto/portas/celula.py:919-925`.

**5. Limites e discrepâncias do instrumento**

- **Nenhuma das 15 linhas fornecidas contém `ERRO`; o encerramento informa `n=15` e `rc_sombra=0`.** Isso não elimina limitações semânticas dos campos. [não verificado — saída fornecida]
- **`x1_dias` conta a lista estática `DADOS`**, sem conferir se cada célula continua False/NB/retida. A soma é 174, mas col237 tem 1 X1 e col348 tem 4 X1 apesar de ambos terem **zero NB em toda a janela**. Logo, o censo não confirma “174 folgas atualmente retidas por NB”. [não verificado — script e COLAB fornecidos]
- A distribuição contém **169 células False/NB na janela inteira**; esse número não é a interseção com X1. Por exemplo, col427 tem 25 False/

[truncado em 20000 de 21593 caracteres]

## 11/11 01:33 · sessao a555301a0a81 · FIO-E0-V4-E3 · construtor · origem=Code

**PARE: a cura não foi aplicada.** O C6 ainda pode consultar ausências por pergunta; não consegui garantir carga constante sem alterar sua alimentação no arquivo proibido. A testemunha executável está em [test_e3_tarefa_admin_sem_banco.py:148](/home/ronald/wt-codex-fio-e0-v4-e3-010719/app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:148). [nao verificado]

Os dois REDs falharam por assertion: receberam `fio='nada'`, esperando `admin`, sob hipótese explícita de última resposta textual E1. Não houve GREEN nem mutantes da cura. [Resultados e comandos](/home/ronald/wt-codex-fio-e0-v4-e3-010719/app/E3_TAREFA_ENTREGA.txt:181).

O [relatório entregue](/home/ronald/wt-codex-fio-e0-v4-e3-010719/app/E3_TAREFA_ENTREGA.txt:1) contém o censo, as saídas condicionais e os limites. Os testes ORM interromperam por falta de Django; seu resultado permanece **[nao verificado]**. [nao verificado]

## 11/11 01:32 · sessao 3de619f39c74 · - · - · origem=Ronald (tmux)

**Cadastro/teste.** `Colaborador` tem `situacao` e `isento_ponto`, mas não tem campo `ativo` nem marcador `teste`; `ativo:null` do script decorre do `getattr(..., None)`, não comprova inatividade. Os campos estão em `app/colaboradores/models.py:283-398`.

Existe um reconhecimento limitado de **empresa de diagnóstico** na UI: `empresas_visiveis()` exclui CNPJ começando por `11222333` e empresas inativas; o próprio código declara que exportação e lógica interna não usam esse filtro. Praça de teste/fantasma é escondida por `ativa=False`. O raspado não traz CNPJ, nomes, praça ou esses flags; `empresa=1/2/3` e `posto=...` não bastam para identificar teste. (`app/colaboradores/queries.py:78-94`)

`TESTE12X36` no col969 sugere teste **[inferência]**, mas é nome de **tipo de escala**: o apelido é apenas rótulo, e `nome_canonico()` o antepõe ao ciclo. Não prova que a pessoa seja de teste. Ademais, os quatro X1 de col969 são setembro/início de outubro, cobertos temporalmente pelo vínculo 1367; o 1434, com esse rótulo, começa em 18/10. (`app/escala/models.py:111-114`, `app/escala/models.py:420-424`; datas: anexo fornecido, sem arquivo local **[não verificado]**)

**Vínculo por dia.** Cobertura temporal exige início ≤ dia, fim nulo ou ≥ dia, e vínculo ativo **ou** com fim declarado. Portanto, `ativa=False` não elimina cobertura histórica; o vínculo 489 de col574 tem candidatura temporal nos X1 apesar do desligamento cadastral. (`app/escala/alimentacao.py:190-216`)

A autoridade efetiva é `CelulaDia.escala_geradora`: se apontar vínculo fora da vigência, a juíza devolve `None`; só sem geradora escolhe entre candidatos por vigência. O script não coleta geradora/DNA por dia, logo permite listar candidatos temporais, não certificar “qual vínculo o juiz escolheu” para cada X1. (`app/escala/alimentacao.py:269-286`, `app/escala/models.py:1692-1700`)

Pelas datas fornecidas, candidatos X1: 144→844; 209→178; 237→204; 268→234; 312→274; 348→306; 427→366; 574→489; 650→695; 969→1367; 1005→1471; 1009→1501; 1018→1517; 1019→1518; 1027→1527. O 1075 de col650 tem fim anterior ao início e não cobre nenhum dia pela regra. Datas do raspado **[não verificado]**; regra em `app/escala/alimentacao.py:214-216`.

**Condição PARE.** Col348 apresenta 61 células `trabalha=False`, nenhuma `True`, e vínculo rotulado intermitente: aciona o exemplo literal “nenhuma célula trabalha=True na janela” da condição do aval. Entretanto, isso não prova erro de escala: intermitente retorna `False` por desenho, pois não há dia previsto; o controle continua sendo entrada+saída quando trabalha. “Intermitente não pede batida” seria afirmação errada. (`app/escala/models.py:36-43`, `app/escala/models.py:816-823`; contagem/aval fornecidos **[não verificado]**)

**Adesão.** O juiz e o cartório usam a primeira batida com `retratada_em IS NULL`; nenhuma viva implica adesão `None`. O script conta o mesmo universo, mas não imprime a data da primeira viva: para col237 e col348 só demonstra adesão existente, sem data. (`app/ponto/services/adesao.py:31-45`, `app/ponto/services/cartorio.py:1165-1167`; contagens do anexo **[não verificado]**)

“Flexível (só horas)” não equivale a isenção: esse texto nasce da ausência de horário formatado; o juiz de isenção lê somente `Colaborador.isento_ponto`. (`app/escala/models.py:425-428`, `app/core/constants.py:13-18`)

**Ausência.** `x1_com_ausencia` conta células nas datas da lista de entrada cujo `ausencia_id` está preenchido. Não consulta aprovação, tipo, período, cobertura integral/parcial nem sobreposição real; o campo é uma FK lavrada na célula. Portanto, a saída demonstra **uma associação, col237 em 07/11/2026**, mas aprovação/cobertura requer **[não verificado]**. (`app/escala/models.py:1792-1798`; expressão e data no anexo fornecido **[não verificado]**)

**Outras limitações do script.**

- `x1_dias` é quantidade de datas na lista `DADOS`, não recontagem de células ainda `False/nunca_bateu` na sombra. Explica haver col237 e col348 no universo apesar de seus histogramas atuais não terem `nunca_bateu`. **[não verificado: anexo fornecido]**
- `trab_x_veredito` cobre 21/09–20/11 inteiros, não somente X1; `retidos` e trabalho retido usam essa janela, com `hoje=10/11` fixo. **[não verificado: anexo fornecido]**
- O retentor corta admissão/demissão e dispensa isento; veredito acusador pode reter sem corte por `hoje`, enquanto veredito ausente só retém dia passado. Assim, seus totais não significam “dias X1 retidos pelo ramo adesão=None”. (`app/folha/export.py:1192-1205`, `app/folha/export.py:1233-1256`)
- `retencao_chaves` é apenas a lista de chaves do primeiro detalhe retornado; `cargo_vazio` é apenas `not c.cargo`; nenhum dos dois identifica teste. **[não verificado: expressão do anexo fornecido]** [nao verificado]
- Nenhuma das quinze linhas fornecidas possui chave `ERRO`; isso não constitui nova execução/verificação na sombra. **[não verificado: saída fornecida]**

## 11/11 01:32 · sessao e6f4c8ed7d43 · - · - · origem=Ronald (tmux)

O gancho **depende da palavra `nunca_bateu`**, dentro do ramo normal, e não de uma memória persistida do ramo anterior: `app/ponto/services/cartorio.py:1369`.

- A adesão seleciona o ramo por primeira batida não retratada: `app/ponto/services/cartorio.py:1165-1173`.
- Sem adesão, a impressão é calculada com batidas/chamados vazios; qualquer veredito preenchido permite pular se a impressão coincidir: `app/ponto/services/cartorio.py:1207-1216`.
- Com adesão, o teste explícito `cel.veredito != 'nunca_bateu'` impede o pulo para NB; `concorde` satisfaz esse teste e pode ser pulado: `app/ponto/services/cartorio.py:1369-1375`.
- A impressão não inclui adesão global; inclui batidas próximas, cobertura, chamados, DNA, veto, teto, regra, fio, parcialidade e isenção: `app/ponto/services/cartorio.py:443-522`.
- Portanto, folga convertida para `concorde` **perde a garantia desse gancho**. Rejulga se a impressão mudar ou houver `forcar`; só a chegada de adesão fora da janela não assegura isso: `app/ponto/services/cartorio.py:1318-1319`, `app/ponto/services/cartorio.py:1359-1375`.
- O teste existente exercita NB com primeira batida três dias depois, fora da janela local; não demonstra preservação para `concorde`: `app/ponto/tests/test_cartorio_nb_readesao.py:11-23`.

Outros leitores nas pastas atribuídas:

| Leitor | Efeito NB → concorde em folga |
|---|---|
| `celula_veta_cobranca` | Continua vetando por `FOLGA`, pois lê `trabalha=False` antes do conjunto que contém NB. `app/ponto/marcos.py:119`, `app/ponto/marcos.py:155-162`. |
| `vigia_ausencia_veredito.censo` | Sai do universo de vereditos acusatórios e, consequentemente, das discordâncias com ausência integral aprovada. `app/ponto/services/vigia_ausencia_veredito.py:41-57`. |
| `ausencias_sem_efeito_na_celula.classificar` | Deixa de incrementar `nunca_bateu`; ainda pode incrementar `sem_efeito` se a ata tiver lâmpada apagada. `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:36`, `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:58-63`. |
| `dia_da_frota` | Sem efeito para `trabalha=False`: a folga já é contada e o laço continua antes da leitura de `VEREDITOS_ACUSAM`. `app/relatorios/dia_da_frota.py:47-55`. |
| `status_da_lavra` / `parcial_acusa_o_resto` | NB integra `_IRREGULAR` e `_ACUSA_NA_PARCIAL`; folga simples sem batida já recebe `folga`, mas uma ausência parcial pode deixar de ser classificada como irregular ao passar a concorde. `app/ponto/status_dia.py:68-70`, `app/ponto/status_dia.py:104-110`, `app/ponto/status_dia.py:139-162`. |

As referências de `config/crons.py` ao conjunto são comentários junto das chamadas aos vigias, não um novo predicado: `app/config/crons.py:2213-2217`, `app/config/crons.py:2231-2236`. O `nunca_bateu` de pós-vínculo é subcategoria de chamado, não leitura de `CelulaDia.veredito`: `app/colaboradores/services/pos_vinculo.py:36`.

## 11/11 01:32 · sessao 300317ecdcfd · - · - · origem=Ronald (tmux)

Leitores diretos executáveis encontrados em `chamados/`, `folha/`, `escala/`; efeito abaixo é leitura estática condicionada à troca da célula de `nunca_bateu` para `concorde`, sem execução nem evidência de produção. [`app/escala/models.py:1767`]

| Leitor | Efeito da troca para `concorde` |
|---|---|
| `motivos_retencao_celula` | Deixa de entrar no ramo acusador; esse dia deixa de reter por `espelho_nunca_bateu`. O ramo seguinte exige veredito `None`, portanto `concorde` também não o aciona. [`app/folha/export.py:1233`, `app/folha/export.py:1257`] |
| `_dias_isentos` | Dia deixa do universo contado como isenção por pausa; exige veredito acusador e `nao_retem_pela_pausa`. [`app/folha/services/vigia_pausa.py:155`] |
| `colabs_nunca_bateu` | Dia sai da lista C5; pessoa só sai se não restar outro dia admitido no universo. Desligados ainda passam pelo filtro do retrato da folha. [`app/escala/services/cadastro_realidade.py:181`, `app/escala/services/cadastro_realidade.py:197`] |
| `universo_adesao` | Dia sai da contagem `dias/prev/fur` usada por `sincronizar_adesao`. Trabalho `nunca_bateu` restante continua no universo. [`app/chamados/services/adesao.py:32`, `app/chamados/services/adesao.py:58`] |
| `dia_nunca_bateu_sem_adesao` | Predicado deixa de reconhecer o dia. [`app/chamados/services/fabrica_da_celula.py:87`] |
| `motivos_a_perguntar` | Cai o retorno antecipado por NB, mas folga continua sem pergunta pelo juiz `dia_sem_trabalho`; além disso `concorde` cai no retorno por não acusar. [`app/chamados/services/fabrica_da_celula.py:105`, `app/chamados/services/fabrica_da_celula.py:113`, `app/chamados/services/fabrica_da_celula.py:120`] |
| `perguntar_pela_celula` | Cai o retorno antecipado NB. Folga não entra na pauta de cadastro sem turno, pois esta exige `not dia_sem_trabalho(cel)`; motivos seguem sujeitos às guardas acima. [`app/chamados/services/fabrica_da_celula.py:183`, `app/chamados/services/fabrica_da_celula.py:202`] |
| `motivo_fora_da_fabrica` | Causa `celula_nunca_bateu` desaparece. Havendo pergunta já materializada e alcançando esse ramo, `concorde` gera `pergunta_do_chamado_ja_encerrada`; os demais critérios continuam na ordem existente. [`app/chamados/services/disputa_emissao.py:758`, `app/chamados/services/disputa_emissao.py:772`] |
| `_resposta_concorda_com_a_luz` | Se todos os demais testes de batida/resposta concordante já passaram, passa a retornar `True`, pois `concorde` está fora de ACUSAM. [`app/chamados/juizes.py:1319`, `app/chamados/juizes.py:1321`] |
| `_pergunta_viva_calc` | Some o retorno `True` do ramo acusador; pode absolver a pergunta, respeitando precedência de resposta/identidade e o teto de marcos pendentes. [`app/chamados/juizes.py:1324`, `app/chamados/juizes.py:1374`, `app/chamados/juizes.py:1385`] |
| `PerguntaDisputa.q_vivas` | Célula passa de `_celula_acusa` para `_celula_absolve`, salvo ata provisória. Resposta não vazia pode manter a candidata; é pré-filtro generoso, não igualdade com o juiz. [`app/chamados/models.py:3340`, `app/chamados/models.py:3355`, `app/chamados/models.py:3357`] |
| `qs_perguntas_absolviveis` | Perguntas do dia entram no universo candidato. `perguntas_absolvidas_pela_celula` ainda consulta C6 e tranca antes de devolver; o escritor pode carimbar/reconciliar e fechar a disputa pela porta. [`app/chamados/reconciliador.py:641`, `app/chamados/reconciliador.py:661`, `app/chamados/reconciliador.py:667`, `app/chamados/reconciliador.py:608`] |
| `_absolvido_pela_celula` | Pode devolver `(dia, cel)` se não houver marcos pendentes; permite a `retratar_chamados_absolvidos_por_celula` retratar chamado de turno pela porta existente. Resposta esperando validação continua sujeita à proteção de `_retratar`. [`app/chamados/reconciliador.py:1027`, `app/chamados/reconciliador.py:1029`, `app/chamados/reconciliador.py:1004`, `app/chamados/reconciliador.py:990`] |
| Leitor de filhos que seguram pai limbo | Filho com célula `concorde` passa pelo `continue`; deixa de segurar o pai por esse caminho. [`app/chamados/services/limbo_folgas.py:191`, `app/chamados/services/limbo_folgas.py:194`] |
| `_premissa_morre` | Se premissa do catálogo já morreu, passa a autorizar morte da cobrança por ausência; a ausência e outras guardas pertencem ao chamador. [`app/chamados/services/silencio_ausencia.py:28`, `app/chamados/services/silencio_ausencia.py:36`, `app/chamados/services/silencio_ausencia.py:41`] |
| `resposta_tardia.receber` | Não tenta renascer o chamado pela célula; resposta segue como tardia, com nota/log e eventual pauta por conteúdo. [`app/chamados/services/resposta_tardia.py:45`, `app/chamados/services/resposta_tardia.py:53`, `app/chamados/services/resposta_tardia.py:59`] |
| `vigia_fio` | Dia sai da lista `acesas`, logo sai desse universo de lâmpadas acusadoras usado no censo de fio morto. [`app/chamados/services/vigia_fio.py:331`, `app/chamados/services/vigia_fio.py:378`] |
| `disparar_perguntas_competencia.contador_fio_mudo` | Dia deixa de alimentar `_dias_sem_adesao`; folga/concorde não gera motivos pelo juiz da fábrica, portanto não vira alvo de fio mudo por essa troca. C5 informativa é relida pela função única. [`app/chamados/management/commands/disparar_perguntas_competencia.py:111`, `app/chamados/management/commands/disparar_perguntas_competencia.py:115`, `app/chamados/management/commands/disparar_perguntas_competencia.py:54`] |

Auditorias que leem ACUSAM, sem curar automaticamente:

- `_contraprova_f1t`: se item continuar indevidamente no piso F1t, agora acusa contraprova falsa porque célula não acusa. [`app/chamados/management/commands/auditar_invariantes_chamados.py:198`]
- `contraprova_piso`: para folga, permanece fora do teste de trabalho porque `not c.trabalha` já basta; trocar NB não altera esse corte. [`app/chamados/management/commands/auditar_invariantes_chamados.py:377`]
- Recorte A4: folga já vai ao piso `_a4c` por `not trabalha`; continua ali com concorde. [`app/chamados/management/commands/auditar_invariantes_chamados.py:1447`]
- C2: pergunta sem materialização/validação/resposta pode entrar no contador de morta por célula; C3 mede explicitamente cobrança viva em célula concorde. [`app/chamados/management/commands/auditar_invariantes_chamados.py:1601`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1606`]
- F1t: dia concorde deixa de satisfazer o piso de célula acusadora. [`app/chamados/management/commands/auditar_invariantes_chamados.py:1713`]

Leitores indiretos relevantes:

- `_nunca_bateu` formata C5 para Cadastro × Realidade, sem juiz adicional. [`app/escala/services/cadastro_realidade.py:204`, `app/escala/services/cadastro_realidade.py:207`, `app/escala/services/cadastro_realidade.py:878`]
- `vigia_retido_calado` usa os **colaboradores** presentes em C5 para rotear linhas sem fio; se pessoa continuar com trabalho NB, segue roteada. Esse vigia lê retratos já lavrados, portanto a troca isolada de célula não reescreve seu retrato. [`app/folha/vigia_retido_calado.py:33`, `app/folha/vigia_retido_calado.py:43`, `app/folha/vigia_retido_calado.py:48`]
- `vigiar_fio_morto` imprime/carimba o contador composto `nunca_bateu_sem_adesao`; não é leitura independente da palavra da célula. [`app/chamados/management/commands/vigiar_fio_morto.py:182`, `app/chamados/management/commands/vigiar_fio_morto.py:241`]

Não confundir `nunca_bateu` calculado nos rankings de `furos_diarios` com leitor do veredito da célula: ali os booleanos vêm de zero batida/previstos/furos, e a consulta de `subcategoria='nunca_bateu'` é de chamado. [`app/escala/services/furos_diarios.py:211`, `app/escala/services/furos_diarios.py:242`, `app/escala/services/furos_diarios.py:283`, `app/escala/services/furos_diarios.py:334`]

## 11/11 01:29 · sessao 812549853dff · FIO-E0-V4-E4B · verificador · origem=Code

**NAO LIBERA.** As curas diretas existem, mas P3/P5 não abrangem toda a cascata acionada pela reabertura; também sobreviveram mutações relevantes fora do catálogo. Referências: `app/chamados/services/disputa_emissao.py:1782`, `app/chamados/signals.py:216`, `app/ponto/services/cartorio.py:1633`, `app/chamados/tests/test_e4b_classe_e_memoria.py:538`, `app/chamados/tests/test_e4b_classe_e_memoria.py:610`.

1. **P1..P6 — existência, origem e fechamento**

   | Item | Verificação |
   |---|---|
   | **P1** | **Cura presente na porta e nos chamadores do backfill:** `hoje` chega ao C6, ao planejamento, complemento e execução. Fecha a perda de parâmetro apontada nesses caminhos. O S143 passa `None`, coerentemente com sua assinatura sem relógio. Referências: `app/chamados/services/disputa_emissao.py:1796`, `app/chamados/services/disputa_emissao.py:1812`, `app/chamados/services/disputa_emissao.py:1913`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:86`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:131`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:171`, `app/chamados/signals.py:115`. |
   | **P2** | **Fecha a duplicação para recusa já conhecida:** não chama `reabrir()`; grava somente a nota deduplicada. Não certifica recusa que apareça na segunda consulta, dentro de `reabrir()`: nesse caminho os efeitos internos continuam possíveis e a nota E4B informa apenas `porta_nao_reabriu`. Referências: `app/chamados/services/disputa_emissao.py:1837`, `app/chamados/services/disputa_emissao.py:1840`, `app/chamados/services/disputa_emissao.py:1845`, `app/chamados/models.py:1390`. |
   | **P3** | **Fecha a cascata direta do credor enumerado:** une não validadas da relavra S2 e IDs da autoridade de ressuscitação; inclui a própria pergunta, inclusive com âncora no pai. Recusa entregue e indeterminada. **Não fecha toda a cascata dos receivers nem a corrida com exportação**, detalhadas no item 5. Referências: `app/chamados/services/disputa_emissao.py:1788`, `app/chamados/services/disputa_emissao.py:1823`, `app/chamados/services/disputa_emissao.py:1826`, `app/chamados/models.py:309`, `app/chamados/signals.py:216`. |
   | **P4** | **Cura restritiva presente:** usa timestamp nulo e `texto_vazio`, recusa sem limpar/carimbar o texto; evento interrompe o resolver, backfill retira o dia dos atos e imprime LIMBO. A quantidade real continua **[nao verificado]**. Referências: `app/chamados/services/disputa_emissao.py:1828`, `app/chamados/services/disputa_emissao.py:1914`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:286`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:357`. |
   | **P5** | **Universo documentado e histórico terminal alheio retirado da antecipação.** A lista explícita segue C→D→Ch→P, mas a expansão não cobre todos os efeitos indiretos do ato. Portanto, **não fecha o achado integralmente**. Referências: `app/chamados/management/commands/backfill_fio_morto_e4b.py:8`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:197`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:208`. |
   | **P6** | **Os dois sobreviventes antigos foram cobertos:** C7 isolada e pai adicional morrem por assertion. O catálogo inteiro também morreu assim; contudo, apareceram sobreviventes adicionais sobre identidade da recusa e completude dos PKs. Referências: `app/chamados/tests/test_e4b_classe_e_memoria.py:51`, `app/chamados/tests/test_e4b_classe_e_memoria.py:243`, `app/chamados/tests/test_e4b_classe_e_memoria.py:319`, `app/chamados/tests/test_e4b_classe_e_memoria.py:708`. |

   **Destino versus pai:** a autoridade continua preferindo destino, independentemente do status; sem destino, usa o pai. A inclusão de `p` na carga fecha a fuga direta do pai sem destino. A respondida continua fora da porta E4B e segue pelo HX existente, cuja reabertura do pai não passa pela cerca nova. Referências: `app/chamados/services/fio.py:117`, `app/chamados/services/disputa_emissao.py:1812`, `app/chamados/services/disputa_emissao.py:1823`, `app/chamados/services/disputa_emissao.py:1974`.

   **Laço exportada:** a aresta direta “S143 → E4B → reabrir credor exportado” fica cortada quando a consulta já enxerga a exportação. Isso não prova fechamento global do laço diante das cascatas e da janela concorrente do item 5; ciclo completo em ORM **[nao verificado]**. Referências: `app/chamados/services/disputa_emissao.py:1826`, `app/chamados/services/disputa_emissao.py:1840`, `app/chamados/services/disputa_emissao.py:1914`.

2. **Quem escreve, quem lê e significado**

   | Campo/valor | Escritores e leitores relevantes |
   |---|---|
   | `respondida_em` / `resposta_colab` | Resposta humana, correção, resposta automática e recuperação pela trilha escrevem o par; E4B lê. C6, projeção `estado`, HX e histórico também leem. Referências: `app/chamados/services/disputa_emissao.py:1467`, `app/chamados/services/disputa_emissao.py:1593`, `app/chamados/services/disputa_emissao.py:1652`, `app/chamados/services/disputa_emissao.py:1674`, `app/chamados/services/disputa_emissao.py:1828`, `app/chamados/juizes.py:1341`, `app/chamados/models.py:3045`, `app/chamados/services/historico.py:99`. |
   | Validação, materialização e via da pergunta | Validação e portas de carimbo/descarimbo escrevem; ressuscitação pode limpar validação, resposta e materialização. C6 e a enumeração E4B leem. Referências: `app/chamados/services/validacao.py:134`, `app/chamados/models.py:3103`, `app/chamados/models.py:3252`, `app/chamados/models.py:1566`, `app/chamados/juizes.py:1325`, `app/chamados/services/disputa_emissao.py:1789`. |
   | `chamado_destino` e pai da disputa | Abertura/reuso e reaponte escrevem destino; E4B lê a âncora por `chamado_que_lista`. Raiox possui outra regra de crédito, preexistente. Referências: `app/chamados/models.py:2982`, `app/chamados/models.py:3014`, `app/chamados/services/fio.py:925`, `app/chamados/services/fio.py:117`, `app/chamados/services/raiox.py:146`. |
   | Status, `resolvido_em`, via e urgência do credor | E4B delega a `reabrir()`→`_gravar_estado`; o núcleo limpa instante/via e restaura urgência. S143, recusa administrativa e crédito leem esses estados. Referências: `app/chamados/services/disputa_emissao.py:1840`, `app/chamados/models.py:1442`, `app/chamados/models.py:248`, `app/chamados/signals.py:96`, `app/chamados/models.py:1342`, `app/chamados/services/raiox.py:146`. |
   | Fechamento da disputa | Ressuscitação pode chamar `reabrir_sistema`, que limpa fechamento/resolução e anota no pai. E4B filtra disputa aberta no evento; a trava lê também disputas da carga. Referências: `app/chamados/models.py:1574`, `app/chamados/models.py:2163`, `app/chamados/services/disputa_emissao.py:1908`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:202`. |
   | Lavra da pergunta | `_gravar_estado` relavra destinos não validados; `lavrar` escreve veredito, instante e expiração. `tarefa_do_dia` lê a lavra vigente. Referências: `app/chamados/models.py:309`, `app/chamados/veredito_lavrado.py:270`, `app/chamados/services/tarefa_do_dia.py:352`. |
   | Competência entregue | O escritor é `ExportacaoDominio.registrar`; invalidar altera vigência. E4B consulta a composição janela→K6, sem escrever TXT. Referências: `app/folha/models.py:174`, `app/folha/models.py:143`, `app/chamados/services/vigia_fio.py:822`, `app/folha/export.py:581`. |
   | `atos`, `pids`, `res` | São dados do planejamento, consumidos pela execução e impressão; não constituem novo estado de domínio persistido. Referências: `app/chamados/management/commands/backfill_fio_morto_e4b.py:85`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:121`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:284`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:353`. |

   **Protocolo dos motivos:** o escritor produz `credor_carrega_limbo:pk1,pk2[:credor_toca_exportada]` ou `credor_toca_exportada`; o retorno acrescenta `recusa:`. Dicionário e ordenação tornam os PKs únicos e crescentes. Referências: `app/chamados/services/disputa_emissao.py:1820`, `app/chamados/services/disputa_emissao.py:1824`, `app/chamados/services/disputa_emissao.py:1831`, `app/chamados/services/disputa_emissao.py:1851`.

   O backfill é o parser numérico nominal encontrado: separa o terceiro segmento por vírgula e aplica `int`. PKs fora de ordem são aceitos e a impressão normaliza com `sorted(set(...))`; `30,31` significa dois IDs. Vírgula inicial/final ou dupla produz elemento vazio e `ValueError`; no planejamento isso acontece fora do tratamento de erro por dia. O escritor atual não produz esse formato inválido. Referências: `app/chamados/management/commands/backfill_fio_morto_e4b.py:89`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:282`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:333`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:358`.

   S143 só reconhece prefixos; o backfill reconhece exportada também no sufixo combinado. Outros parsers externos à cópia: **[nao verificado]**. Referências: `app/chamados/services/disputa_emissao.py:1914`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:90`.

   **Notas:** `anexar_ocorrencia` persiste conteúdo/autoria e a chave; `tipo` e `objeto_ref` entram no hash, não são colunas próprias. A referência E4B contém pergunta, motivo completo e `resolvido_em`; a unicidade é, portanto, por esse fato, não uma nota global por credor. Histórico/fio leem genericamente as notas; SLA considera fala do DP, não a nota de sistema. Referências: `app/chamados/services/nota.py:62`, `app/chamados/services/nota.py:88`, `app/chamados/services/disputa_emissao.py:1850`, `app/chamados/models.py:1698`, `app/chamados/services/historico.py:80`, `app/chamados/services/sla.py:21`.

   **Esmeril temporal:** `hoje` é uma data; `agora`, `respondida_em` e `resolvido_em` representam instantes. `--hoje` não altera o `agora` usado pela tarefa, logo não constitui replay integral do ato. `data_evento` possui a distinção preexistente: dia do turno nas perguntas factuais e dia civil nas conversas; C6 respeita o discriminador, enquanto a cerca nova consulta diretamente essa data para competência. Equivalência de jurisdição para todas as conversas: **[nao verificado]**. Referências: `app/chamados/management/commands/backfill_fio_morto_e4b.py:258`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:83`, `app/chamados/juizes.py:332`, `app/chamados/services/disputa_emissao.py:1827`.

3. **Predicados de limbo**

   E4B coincide com `carimbar_resposta_pela_trilha`: timestamp nulo e texto não vazio após `strip()`. A autoridade existente para vazio textual é `texto_vazio`. Referências: `app/chamados/services/disputa_emissao.py:1828`, `app/chamados/services/disputa_emissao.py:1670`, `app/chamados/juizes.py:1441`.

   **Não coincide exatamente com o comando histórico:** este exclui somente `resposta_colab=''`; espaços entram no conjunto medido, mas não na cerca E4B nem no carimbo pela trilha. Além disso, aquele comando exige os PKs fixos de `MEDIDAS`, não cura genericamente qualquer limbo novo. Referências: `app/chamados/management/commands/curar_perguntas_em_limbo.py:21`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:46`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:48`.

   Isso não demonstra juiz novo introduzido pela cura: a composição inline reutiliza `texto_vazio`, conforme o brief; a divergência está no seletor legado. A interpretação “voz testemunhada versus resto de questionário” permanece nas portas do corte, não na E4B. Referências: `app/chamados/juizes.py:1441`, `app/chamados/services/disputa_emissao.py:1829`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:9`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`.

   A impressão é `LIMBO colNN DATA perguntas=PKS`; `limbo=N` conta **dias do plano**, não perguntas distintas. Quantidade real: **[nao verificado]**. Referências: `app/chamados/management/commands/backfill_fio_morto_e4b.py:357`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:365`.

4. **Idempotência**

   Para sucesso sequencial, a segunda passagem encontra credor vivo e sai antes da nota. Para recusa conhecida e fato estável, há uma nota Followup por chave e **zero nova `reabertura_recusada` em LogAuditoria**. Isso fecha o problema original de chamar repetidamente a porta recusada. Referências: `app/chamados/services/disputa_emissao.py:1818`, `app/chamados/services/disputa_emissao.py:1840`, `app/chamados/services/nota.py:95`, `app/chamados/services/nota.py:109`.

   Consultar veto/irmão por `recusa_da_reabertura()` não executa seus efeitos. Entrar em `reabrir()` pode registrar veto, auditar admin/premissa ou migrar ao irmão. Se a recusa surgir somente nessa segunda consulta, E4B não desfaz esses efeitos; esse cenário não está coberto pelo selo de recusa estável. Referências: `app/chamados/models.py:1338`, `app/chamados/models.py:1354`, `app/chamados/models.py:1391`, `app/chamados/models.py:1394`, `app/chamados/models.py:1411`, `app/chamados/tests/test_e4b_classe_e_memoria.py:538`.

   Portanto, “2× = mesmo estado, uma trilha, zero duplicata” está sustentado pelos casos sequenciais dublados, **não certificado integralmente em PostgreSQL/cascatas concorrentes [nao verificado]**. Referências: `app/chamados/tests/test_e4b_classe_e_memoria.py:516`, `app/chamados/tests/test_e4b_classe_e_memoria.py:538`.

5. **Universo, trava e reconferência**

   O universo implementado é `sem_porta` do vigia, somado ao complemento entregue/indeterminado em `[hoje−45, hoje)`; não há corte geral de 45 dias. A competência atual entra sempre na autoridade; a anterior depende de não entregue. Referências: `app/chamados/management/commands/backfill_fio_morto_e4b.py:74`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:164`, `app/chamados/services/vigia_fio.py:583`, `app/ponto/services/he_pendente_lavrado.py:220`.

   **A ordem explícita está correta:** C primeiro, depois D, Ch e P, cada conjunto por PK. **O universo antecipado é incompleto para a cascata:** expande perguntas somente das âncoras do lote; os demais chamados vivos são acrescentados sem expandir suas próprias cargas. Referências: `app/chamados/management/commands/backfill_fio_morto_e4b.py:190`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:197`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:206`, `app/chamados/models.py:2666`, `app/chamados/models.py:2675`, `app/chamados/models.py:2688`, `app/chamados/models.py:2692`.

   **Contraexemplo em memória:** acrescentando outro chamado vivo `12`, com pergunta ressuscitável `90` fora do dia e disputa fechada `90`, a função real antecipou Ch12, mas omitiu P90/D90. O teste atual cobre carga da âncora principal, não essa composição. Referências: `app/chamados/management/commands/backfill_fio_morto_e4b.py:197`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:208`, `app/chamados/tests/test_e4b_classe_e_memoria.py:692`.

   **Há caminho de escrita para a carga omitida:** reabrir salva o credor; o receiver chama o cartório; este pode ressuscitar perguntas de outro chamado vivo do dia. Havendo adiados, também chama E2 para o colaborador inteiro, cujo seletor não recorta competência/data. Assim, a cerca direta não prova “exportada nunca escrita”, nem a antecipação prova a ordem de todas as aquisições posteriores. Caminho completo em ORM: **[nao verificado]**. Referências: `app/chamados/models.py:296`, `app/chamados/signals.py:216`, `app/ponto/services/cartorio.py:1602`, `app/ponto/services/cartorio.py:1633`, `app/ponto/services/cartorio.py:970`, `app/chamados/reconciliador.py:595`, `app/chamados/reconciliador.py:627`.

   **A reconferência existe**, inclusive do plano sob trava. Escritores canônicos de nascimento/reaponte compartilham C, protegendo contra essas mudanças concorrentes. Contudo, exportação não compartilha essa trava: K6 lê sem lock e `ExportacaoDominio.registrar()` faz `create()` sem C/D/Ch/P. Existe janela entre a última consulta e `reabrir()`. Ocorrência real da corrida: **[nao verificado]**. Referências: `app/chamados/management/commands/backfill_fio_morto_e4b.py:318`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:323`, `app/chamados/models.py:2845`, `app/chamados/services/fio.py:889`, `app/folha/export.py:581`, `app/folha/models.py:182`, `app/chamados/services/disputa_emissao.py:1840`.

   Há ainda um ramo defensivo incompleto: se a porta devolver recusa nova em `_executar_dia`, ele acumula a recusa e continua os demais atos. Em memória, recusa de limbo seguida de `perguntar` chamou a fábrica. Atingibilidade desse intervalo em ORM: **[nao verificado]**. Referências: `app/chamados/management/commands/backfill_fio_morto_e4b.py:131`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:139`.

6. **Relógio**

   Não identifiquei chamada nova a `now()`/`today()` nos hunks de produção; os testes novos usam `AGORA` fixo e o B2d tocado tem `freeze_time`. Referências: `app/chamados/services/disputa_emissao.py:1796`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:258`, `app/chamados/tests/test_e4b_classe_e_memoria.py:22`, `app/chamados/tests/test_b2d_resposta_visivel.py:79`.

   Permanecem relógios preexistentes fora desse repasse: C6 do evento pode chegar a `date.today()`; a autoridade de ressuscitação consulta C6 sem `hoje`; a lavra lê `timezone.now()`. Logo, P1 corrige a chamada nova, mas não transforma a cascata inteira em replay determinístico. Referências: `app/chamados/juizes.py:577`, `app/chamados/models.py:1509`, `app/chamados/veredito_lavrado.py:271`.

7. **REDs, mutantes e Ruff**

   Executei somente os selos em memória: **51 testes OK**, abrangendo `test_e4b_classe_e_memoria`, `test_e4_expand` e `test_e4_efeitos`; os **31 mutantes catalogados morreram por `AssertionError`**, com original verde e alvo único exigidos pelo harness. Referências: `app/chamados/tests/test_e4b_classe_e_memoria.py:708`, `app/chamados/tests/test_e4_expand.py:127`, `app/chamados/tests/test_e4_efeitos.py:8`.

   Reconstruí somente em memória a pré-imagem de emissão: seu blob resultou em `d901e43c14a0d4dbe34a5e8e27e3fd8762b72fdc`, compatível com o índice antigo do patch fornecido. Os quatro REDs P1–P4 produziram **quatro falhas por assertion, zero errors**. A associação independente desse blob ao commit `345f15a87` permanece **[nao verificado]**, porque o runner depende de `git show` e esta cópia não contém repositório Git acessível. Referências dos asserts/runner: `app/chamados/tests/test_e4b_classe_e_memoria.py:493`, `app/chamados/tests/test_e4b_classe_e_memoria.py:545`, `app/chamados/tests/test_e4b_classe_e_memoria.py:558`, `app/chamados/tests/test_e4b_classe_e_memoria.py:615`, `app/chamados/tests/test_e4b_classe_e_memoria.py:736`.

   **Sobreviventes adicionais comprovados em memória:**

   - Trocar a referência da nota por somente `str(p.pk)` manteve os 36 testes verdes; perderia distinção entre motivos/encerramentos diferentes. O teste reinicia a fixture para cada motivo. Referências: `app/chamados/services/disputa_emissao.py:1850`, `app/chamados/tests/test_e4b_classe_e_memoria.py:538`.
   - Emitir somente `limbo[:1]` manteve os 36 testes verdes; omite os demais PKs da nota e da lista. Os cenários atuais verificam um PK de limbo por vez. Referências: `app/chamados/services/disputa_emissao.py:1831`, `app/chamados/tests/test_e4b_classe_e_memoria.py:610`, `app/chamados/tests/test_e4b_classe_e_memoria.py:621`.
   - Inverter a ordenação da carga também sobreviveu; falta assert de formato com múltiplos PKs. Referências: `app/chamados/services/disputa_emissao.py:1824`, `app/chamados/tests/test_e4b_classe_e_memoria.py:621`.

   Os defaults ligados ao laço preservam os asserts apresentados no patch. Os quatro blobs atuais coincidiram com os índices novos fornecidos, inclusive o teste `9a2abe712…`; a reconstrução das nove correções individuais contra uma fonte anterior ao

[truncado em 20000 de 23299 caracteres]

## 11/11 01:26 · sessao 5118d86613c6 · - · - · origem=Ronald (tmux)

P3/P5 têm cobertura direta, mas não certificam toda a cascata de `reabrir()`: a enumeração nova cobre relavra S2 e ressuscitação do credor; o `save()` desse credor também dispara o cartório. Referências: `app/chamados/services/disputa_emissao.py:1788`, `app/chamados/models.py:296`, `app/chamados/signals.py:188`.

Achados:

- **A trava não cobre todas as linhas potencialmente tocadas pela cascata.** `_travar_lote` expande somente as âncoras das perguntas do dia; acrescenta os demais chamados vivos sem expandir suas perguntas/disputas. Referências: `app/chamados/management/commands/backfill_fio_morto_e4b.py:197`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:206`.
  
  Em ensaio somente em memória, acrescentei outro chamado vivo `12`, da célula do lote, com pergunta ressuscitável `90` fora do dia, em disputa fechada `90`. A função real produziu:
  ```
  [call([7]),
   call([7], disputas=[20], chamados=[10, 11, 12], perguntas=[30])]
  ```
  Portanto, incluiu o chamado e omitiu sua pergunta/disputa. O ensaio utiliza os doubles existentes e executa a função real; o caso completo em ORM permanece [nao verificado]. Referências da fixture e da enumeração: `app/chamados/tests/test_e4b_classe_e_memoria.py:164`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:197`.

- **Há caminhos concretos no código para essa carga adicional ser tocada.** O cartório agrupa chamados dos `FURO_MODULES` pelo dia de `data_do_chamado`, seleciona os vivos e pode chamar `ressuscitar_perguntas()` de outro chamado vivo. Referências: `app/ponto/services/cartorio.py:117`, `app/ponto/services/cartorio.py:125`, `app/ponto/services/cartorio.py:1316`, `app/ponto/services/cartorio.py:1345`, `app/ponto/services/cartorio.py:1602`, `app/ponto/services/cartorio.py:1633`.

  Uma pergunta validada é morta para o fio; a autoridade de ressuscitação avalia uma cópia sem validação/carimbo e pode devolvê-la viva. Assim, o teste de fio morto não elimina esse caminho. Referências: `app/chamados/juizes.py:1325`, `app/chamados/juizes.py:1434`, `app/chamados/models.py:1506`, `app/chamados/models.py:1509`.

  Além disso, havendo adiados, o cartório chama E2 para **o colaborador inteiro**; E2 seleciona perguntas sem recorte de data/competência, carimba e pode fechar disputas. Referências: `app/ponto/services/cartorio.py:962`, `app/ponto/services/cartorio.py:970`, `app/chamados/reconciliador.py:584`, `app/chamados/reconciliador.py:595`, `app/chamados/reconciliador.py:608`, `app/chamados/reconciliador.py:627`.

  Logo, a lista explicitamente ordenada C→D→Ch→P não prova a ordem de todas as aquisições posteriores nem “exportada nunca escrita” na cascata completa. Demonstração integral concorrente/ORM: [nao verificado].

- **Existe janela entre conferir exportação e escrever.** A última leitura da cerca ocorre antes de `credor.reabrir()`; K6 consulta `ExportacaoDominio` sem trava. O escritor `ExportacaoDominio.registrar()` faz `exists()` e `create()` sem adquirir C/D/Ch/P. Referências: `app/chamados/services/disputa_emissao.py:1826`, `app/chamados/services/disputa_emissao.py:1840`, `app/folha/export.py:580`, `app/folha/models.py:174`, `app/folha/models.py:182`.

  Portanto, a trava do colaborador bloqueia os escritores canônicos concorrentes de pergunta/reaponte, mas não serializa a exportação. Referências: `app/chamados/models.py:2845`, `app/chamados/services/fio.py:889`, `app/folha/models.py:182`. Ocorrência real dessa corrida: [nao verificado].

- **A reconferência existe:** o backfill relê competência e plano sob a trava, e a porta volta a enumerar a carga. Isso fecha mudanças anteriores a essas leituras, não as cascatas posteriores descritas acima. Referências: `app/chamados/management/commands/backfill_fio_morto_e4b.py:318`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:323`, `app/chamados/services/disputa_emissao.py:1820`.

- **O teste P5 atual não cobre o contraexemplo:** exercita a carga da âncora principal e histórico terminal alheio, sem outro chamado vivo com carga própria. Referência: `app/chamados/tests/test_e4b_classe_e_memoria.py:692`.

O universo declarado corresponde à composição implementada: `sem_porta` do vigia, mais entregue/indeterminada em `[hoje−45, hoje)`; não existe corte geral de 45 dias. Referências: `app/chamados/management/commands/backfill_fio_morto_e4b.py:8`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:74`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:164`, `app/chamados/services/vigia_fio.py:583`.

**NAO LIBERA P3/P5**, pela fronteira de exportação sem serialização e pela carga indireta não abrangida pela enumeração/trava; ocorrências em produção e execução completa das cascatas em ORM permanecem [nao verificado].

## 11/11 01:25 · sessao 985f431d56cb · - · - · origem=Ronald (tmux)

P6 melhora, mas deixa lacunas:

- Executei o comando em memória dos três módulos: **51 testes OK**, com os **31 mutantes mortos por `AssertionError`**. O harness exige original verde, alvo único e mensagem esperada. `app/chamados/tests/test_e4b_classe_e_memoria.py:708`.
- Os dois sobreviventes antigos agora morrem: C7 tem rodada com `trancado=False`; pai adicional é proibido por estado e chamada. `app/chamados/tests/test_e4b_classe_e_memoria.py:319`, `app/chamados/tests/test_e4b_classe_e_memoria.py:243`.
- **Mutante adicional sobrevive:** substituir `objeto_ref` por somente `str(p.pk)` manteve os 36 testes verdes, inclusive o catálogo. Isso perderia notas de novas recusas/novos fechamentos. Os testes reiniciam a fixture para cada motivo e não verificam sua mudança preservando o histórico. `app/chamados/services/disputa_emissao.py:1850`, `app/chamados/tests/test_e4b_classe_e_memoria.py:538`.
- **Outro sobrevivente:** inverter `sorted(carga)` para ordem decrescente manteve os 36 testes verdes. Não há caso com múltiplas PKs de limbo exigindo formato e ordem estáveis. `app/chamados/services/disputa_emissao.py:1824`, `app/chamados/tests/test_e4b_classe_e_memoria.py:621`.
- O `--volta1` falhou antes dos testes porque não há repositório Git acessível; a mordida contra o objeto exato `345f15a87` permanece **[nao verificado]**. O runner depende diretamente de `git show`. `app/chamados/tests/test_e4b_classe_e_memoria.py:736`.
- Relógio dos testes novos é fixo; B2d tocado está congelado. `app/chamados/tests/test_e4b_classe_e_memoria.py:22`, `app/chamados/tests/test_e4b_classe_e_memoria.py:396`, `app/chamados/tests/test_b2d_resposta_visivel.py:79`.
- P1 não torna a cascata inteira reproduzível: enumeração chama o C6 sem `hoje`; lavra lê `timezone.now()` existente. `app/chamados/services/disputa_emissao.py:1788`, `app/chamados/models.py:1509`, `app/chamados/veredito_lavrado.py:271`.
- Os defaults vinculados ao laço preservam os asserts apresentados; a autenticação das nove alterações individuais de Ruff exige a fonte anterior separada **[nao verificado]**. `app/chamados/tests/test_e4b_classe_e_memoria.py:556`, `app/chamados/tests/test_e4b_classe_e_memoria.py:638`, `app/chamados/tests/test_e4b_classe_e_memoria.py:644`.
- **Ramo concreto da recusa nova:** `_executar_dia` registra recusa e continua os demais atos. Reproduzi em memória `recusa:credor_carrega_limbo:31` seguida de uma chamada à fábrica. Atingibilidade concorrente em ORM: **[nao verificado]**. `app/chamados/management/commands/backfill_fio_morto_e4b.py:131`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:139`.

ORM, suíte inteira, banco/log, efeitos reais e concorrência: **[nao verificado]**. [nao verificado]

## 11/11 01:25 · sessao a79ec8ad0557 · - · - · origem=Ronald (tmux)

P2/P4 e semântica, somente leitura:

- **P2 cura a recusa já conhecida:** `motivo is None and credor.reabrir()` evita entrar nos efeitos de `reabrir`; a recusa recebe nota deduplicada. A consulta é declarada e implementada como leitura, inclusive veto e irmão. Referências: `app/chamados/services/disputa_emissao.py:1837`, `app/chamados/services/disputa_emissao.py:1840`, `app/chamados/models.py:1318`, `app/chamados/models.py:1338`, `app/chamados/models.py:1354`.
- **Efeitos evitados:** veto escreve auditoria com deduplicação própria; admin/premissa escrevem `reabertura_recusada` a cada entrada; irmão pode migrar perguntas, anotar e auditar. Referências: `app/chamados/models.py:1155`, `app/chamados/models.py:1390`, `app/chamados/models.py:1394`, `app/chamados/models.py:1400`, `app/chamados/models.py:1411`, `app/chamados/models.py:1427`.
- **Recusa nova entre as duas consultas permanece distinta:** se a consulta externa passa e `reabrir()` recusa na consulta interna, os efeitos internos já podem acontecer; E4B registra `porta_nao_reabriu`, perdendo o motivo específico. Os testes substituem `reabrir` por mock e não cobrem essa mudança de resultado. Referências: `app/chamados/services/disputa_emissao.py:1837`, `app/chamados/services/disputa_emissao.py:1840`, `app/chamados/services/disputa_emissao.py:1845`, `app/chamados/models.py:1390`, `app/chamados/tests/test_e4b_classe_e_memoria.py:214`. Ocorrência concorrente concreta: [nao verificado].

**Formato e consumidores:**

- Escritor único nominal dos motivos: `credor_carrega_limbo:pk1,pk2[:credor_toca_exportada]` ou `credor_toca_exportada`; o retorno acrescenta `recusa:`. Os PKs saem únicos/crescentes porque a carga passa por dicionário e `sorted`. Referências: `app/chamados/services/disputa_emissao.py:1820`, `app/chamados/services/disputa_emissao.py:1824`, `app/chamados/services/disputa_emissao.py:1831`, `app/chamados/services/disputa_emissao.py:1838`.
- O único parser numérico de produção encontrado é o backfill: `res.split(':')[2].split(',')`, seguido de `int`. Ordem arbitrária é aceita e a impressão usa `sorted(set(...))`; vírgula separa IDs, e elemento vazio gera `ValueError`, inclusive no planejamento fora do `try` do ato. Referências: `app/chamados/management/commands/backfill_fio_morto_e4b.py:89`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:282`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:309`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:358`.
- S143 apenas testa prefixos; o backfill detecta exportada por substring, reconhecendo também o sufixo combinado. Referências: `app/chamados/services/disputa_emissao.py:1914`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:90`. Consumidores externos/reflexão: [nao verificado].

**Nota e deduplicação:**

- `tipo` e `objeto_ref` são argumentos usados no hash, não colunas persistidas; conteúdo e `evento_chave` ficam no Followup. A chave combina chamado, tipo, referência e autor; a referência E4B combina pergunta, motivo completo e `resolvido_em`. Unique parcial e tratamento de colisão implementam uma nota por chave. Referências: `app/chamados/services/nota.py:62`, `app/chamados/services/nota.py:88`, `app/chamados/services/nota.py:95`, `app/chamados/services/nota.py:109`, `app/chamados/services/disputa_emissao.py:1850`, `app/chamados/models.py:1698`.
- Conjunto limbo, motivo ou encerramento diferentes geram outra chave. A garantia é por pergunta + motivo + encerramento, não uma nota global por credor. `resolvido_em` é instante, `DateTimeField`; não há conversão nova para data. Referências: `app/chamados/services/disputa_emissao.py:1850`, `app/chamados/models.py:60`.
- Conhecida a recusa, a cura produz uma nota Followup e **zero nova linha LogAuditoria**, porque não chama `reabrir`; o selo verifica essa nota com implementação real da porta, mas unique/atomic são doubles. Referências: `app/chamados/services/disputa_emissao.py:1840`, `app/chamados/tests/test_e4b_classe_e_memoria.py:516`, `app/chamados/tests/test_e4b_classe_e_memoria.py:538`. PostgreSQL real: [nao verificado].
- Leitores genéricos: histórico lê conteúdo/criação/autoria; SLA usa somente resposta do DP, portanto a nota de sistema não reinicia prazo. Referências: `app/chamados/services/historico.py:80`, `app/chamados/services/historico.py:84`, `app/chamados/services/sla.py:17`.

**Limbo e autoridade:**

- E4B e `carimbar_resposta_pela_trilha` coincidem: timestamp nulo e texto com `.strip()` não vazio. `texto_vazio` é a autoridade existente para vazio textual. Referências: `app/chamados/services/disputa_emissao.py:1828`, `app/chamados/services/disputa_emissao.py:1670`, `app/chamados/juizes.py:1441`.
- O comando histórico diverge: exclui apenas `resposta_colab=''`. Assim, `'  '` entra no conjunto medido do comando, mas não na cerca E4B nem no carimbo. O comando também exige os PKs fixos `MEDIDAS`; não é uma porta geral de cura de qualquer limbo novo. Referências: `app/chamados/management/commands/curar_perguntas_em_limbo.py:21`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:46`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:48`.
- Não identifiquei novo juiz nessa composição inline: ela reutiliza `texto_vazio`, exatamente como o brief autorizou; a divergência é do seletor legado. A própria porta de carimbo usa o instante testemunhado pela nota, preservando o significado de instante. Referências: `app/chamados/juizes.py:1441`, `app/chamados/services/disputa_emissao.py:1663`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`.

**Lacuna adversarial comprovada:**

Mutação apenas em memória de `app/chamados/services/disputa_emissao.py:1831`, trocando `for pk in limbo` por `for pk in limbo[:1]`, **sobreviveu aos 36 testes do módulo, incluindo o harness dos 31 mutantes**: rc=0, `Ran 36 tests`, `OK`. Ela omite os demais PKs de limbo da nota e da lista do backfill; os testes P4 cobrem um único PK limbo por cenário. Referências: `app/chamados/tests/test_e4b_classe_e_memoria.py:610`, `app/chamados/tests/test_e4b_classe_e_memoria.py:621`, `app/chamados/tests/test_e4b_classe_e_memoria.py:662`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:89`. Execução realizada com `PYTHONDONTWRITEBYTECODE=1`, `FONTES_MUTADAS` e `unittest`, sem escrita de arquivos; log persistido: [nao verificado].

## 11/11 01:24 · sessao 39471fc9c9ea · FIO-E0-V4-E3 · leitura · origem=Code

**A divisão sustentada é: 3 casos (a), 2 casos (b) com limite de verificação da sequência E1, 8 casos (c) — ADMIN/DP pela ausência — e 2 casos com “sem regra” específica de encaminhamento.** As regras são GRUPO-D, HX-ADMIN/PERGUNTA-RESPONDIDA-E-MARCO-ACESO e COBERTURA-PARCIAL-PERIODO, respectivamente. (`app/docs/CORTES.md:263`, `app/chamados/juizes.py:1341`, `app/docs/CORTES.md:319`)

Nas referências abaixo, `casos_v15.out:N` significa a enésima linha `DIA` do **anexo recebido**. O JSON contém instâncias; o que está vazio são suas `acoes`. Portanto, “sem instâncias” não descreve essas saídas. (`casos_v15.out:1`, `casos_v15.out:12`)

As regras usadas na tabela são:

- **A — GRUPO-D:** “dia acusando sem pergunta e sem chamado”; responsabilidade expressa: “cura pelo emissor que le a lampada”, contando “celula acusa + 0 pergunta + 0 chamado”. A regra geral diz: “Emissao, pergunta e chamado nascem do sinal da celula”. Isso decide **o responsável pela existência do fio**, sem especificar qual pergunta resolve `FATO_EM_AUSENCIA`. (`app/docs/CORTES.md:263`, `CLAUDE.md:168`)
- **B — resposta viva:** HX-ADMIN preserva a resposta sem decisão explícita do ADMIN na célula; PERGUNTA-RESPONDIDA-E-MARCO-ACESO diz que, discordando ou sem como conferir, “segue viva para o admin decidir”. O corte SMOKE-FIO exige apresentar `Validar` quando segue viva e respondida. (`app/chamados/juizes.py:1338`, `app/chamados/juizes.py:1341`, `app/chamados/juizes.py:1350`, `app/docs/CORTES.md:337`)
- **P — parcial sem período:** “hora_ini/hora_fim [...] preenchidos pelo ADMIN na aprovacao”; sem hora, “o dia vai a lista do admin como CADASTRO”. Depois de aprovada, a edição usa a mesma porta `ausencia.py::decidir`. O cartório explicita: “parcial sem periodo vai a lista do admin, nunca chamado”. (`app/docs/CORTES.md:319`, `app/docs/CORTES.md:323`, `app/ponto/services/cartorio.py:379`)
- **S — sem regra:** as regras encontradas de `FATO_EM_AUSENCIA` definem quando acusar, mas não escolhem ator/ato para resolver o conflito residual com perguntas mortas. **Sem regra específica localizada para esse encaminhamento.** (`app/docs/CORTES.md:315`, `app/docs/CORTES.md:334`; abrangência da ausência de regra: `[nao verificado]`)

| Colab | Dia | a/b/c — quem devia agir | Regra | Arquivo:linha — dado e ramo |
|---|---|---|---|---|
| col177 | 2026-11-08 | **a — emissor**, obrigação de dar fio; espécie da emissão: sem regra específica | A — GRUPO-D | `casos_v15.out:1`; `app/docs/CORTES.md:263`; fábrica: `app/chamados/services/fabrica_da_celula.py:112` |
| col252 | 2026-09-29 | **b\* — ADMIN**, resposta viva p38162 | B — resposta viva | `casos_v15.out:2`; `app/chamados/juizes.py:1350`; descarte: `app/chamados/services/tarefa_do_dia.py:241` |
| col258 | 2026-09-28 | **c — ADMIN/DP**, declarar período da ausência | P — parcial sem período | `casos_v15.out:3`; `app/docs/CORTES.md:319`; descarte: `app/chamados/services/tarefa_do_dia.py:244` |
| col258 | 2026-10-19 | **c — ADMIN/DP**, declarar período da ausência | P | `casos_v15.out:4`; `app/docs/CORTES.md:319`; `app/chamados/services/tarefa_do_dia.py:244` |
| col259 | 2026-10-14 | **c — ADMIN/DP**, declarar período da ausência | P | `casos_v15.out:5`; `app/docs/CORTES.md:319`; `app/chamados/services/tarefa_do_dia.py:244` |
| col259 | 2026-10-15 | **c — ADMIN/DP**, declarar período da ausência | P | `casos_v15.out:6`; `app/docs/CORTES.md:319`; `app/chamados/services/tarefa_do_dia.py:244` |
| col292 | 2026-10-22 | **c — sem regra** específica que escolha ator/ato residual | S — perguntas C6 mortas | `casos_v15.out:7`; `app/docs/CORTES.md:315`; descarte: `app/chamados/services/tarefa_do_dia.py:241` |
| col292 | 2026-10-28 | **a — emissor**, obrigação de dar fio; espécie da emissão: sem regra específica | A — GRUPO-D | `casos_v15.out:8`; `app/docs/CORTES.md:263`; fábrica: `app/chamados/services/fabrica_da_celula.py:112` |
| col354 | 2026-10-21 | **c — ADMIN/DP**, declarar período da ausência | P | `casos_v15.out:9`; `app/docs/CORTES.md:319`; `app/chamados/services/tarefa_do_dia.py:244` |
| col354 | 2026-10-30 | **c — sem regra** específica que escolha ator/ato residual | S — pergunta C6 morta | `casos_v15.out:10`; `app/docs/CORTES.md:315`; descarte: `app/chamados/services/tarefa_do_dia.py:241` |
| col368 | 2026-09-21 | **c — ADMIN/DP**, declarar período da ausência | P | `casos_v15.out:11`; `app/docs/CORTES.md:319`; `app/chamados/services/tarefa_do_dia.py:244` |
| col655 | 2026-09-30 | **c — ADMIN/DP**, declarar período da ausência | P | `casos_v15.out:12`; `app/docs/CORTES.md:319`; `app/chamados/services/tarefa_do_dia.py:244` |
| col655 | 2026-10-21 | **c — ADMIN/DP**, declarar período da ausência | P | `casos_v15.out:13`; `app/docs/CORTES.md:319`; `app/chamados/services/tarefa_do_dia.py:244` |
| col841 | 2026-10-10 | **b\* — ADMIN**, resposta viva p42367 | B — resposta viva | `casos_v15.out:14`; `app/chamados/juizes.py:1350`; descarte: `app/chamados/services/tarefa_do_dia.py:241` |
| col889 | 2026-10-25 | **a — emissor**, obrigação de dar fio; espécie da emissão: sem regra específica | A — GRUPO-D | `casos_v15.out:15`; `app/docs/CORTES.md:263`; fábrica: `app/chamados/services/fabrica_da_celula.py:112` |

**Por classe:**

**A — três dias sem pergunta nem chamado.** A obrigação genérica do emissor está escrita. A função de emissão é `perguntar_pela_celula`, que consulta `motivos_a_perguntar`; esta retorna `[]` quando `dia_sem_trabalho(cel)` responde verdadeiro. O cartório possui gatilhos de chamados para `FURO_*`/limbo, mas esses ramos não recebem o protesto isolado `FATO_EM_AUSENCIA`. (`app/chamados/services/fabrica_da_celula.py:178`, `app/chamados/services/fabrica_da_celula.py:207`, `app/chamados/services/fabrica_da_celula.py:112`, `app/ponto/services/cartorio.py:1707`, `app/ponto/services/cartorio.py:1773`)

O anexo identifica `dia_sem_trabalho` como descarte **da tarefa**, nos três dias. Não traz a ata completa nem a execução histórica da fábrica: afirmar que aquele mesmo guarda efetivamente impediu cada emissão histórica é **[nao verificado]**. `trabalha=True` sozinho não afasta o guarda, pois o juiz também lê `ata.tipo_dia` e `cobertura_parcial`. (`casos_v15.out:1`, `casos_v15.out:8`, `casos_v15.out:15`, `app/chamados/juizes.py:435`)

**B — dois dias com resposta viva.** Há dever escrito de decisão pelo ADMIN; a tarefa interrompe a instância em `dia_sem_trabalho` **antes** de alcançar a pergunta e `_pela_decisao`. (`casos_v15.out:2`, `casos_v15.out:14`, `app/chamados/juizes.py:1350`, `app/chamados/services/tarefa_do_dia.py:241`, `app/chamados/services/tarefa_do_dia.py:281`)

O asterisco preserva um limite: o anexo não informa a última `DecisaoPergunta`, o passivo da projeção, a identidade completa nem a FK `PerguntaDisputa.celula`. Assim, o dever humano tem regra, mas **não está provado qual saída E1 exata essas duas perguntas produziriam**: passivo sem linha devolve indeterminação; última resposta textual devolve `validar/admin`. (`app/chamados/services/tarefa_do_dia.py:127`, `app/chamados/services/tarefa_do_dia.py:132`, `app/chamados/services/tarefa_do_dia.py:337`; valores desses campos nos dois casos: `[nao verificado]`)

**P — oito parciais.** A ação pertence ao ADMIN/DP pela ausência, e a ausência de pergunta é compatível com a regra específica. `parcial_so_lista` retorna verdadeiro para `sem_periodo`; a fábrica então retorna `[]`, “sem pergunta, sem pai, sem push”. A tarefa descarta as instâncias em `lampada=True`, razão `aceso`. (`app/docs/CORTES.md:319`, `app/ponto/services/cartorio.py:398`, `app/chamados/services/fabrica_da_celula.py:123`, `app/chamados/services/tarefa_do_dia.py:244`)

O corte não manda projetar essa edição de cadastro como tarefa de marco. O contrato de `tarefa_do_dia` é por **instância de marco**, enquanto a porta expressamente escolhida para preencher o período é `ausencia.py::decidir`. A presença concreta dos oito dias na lista, e a ausência específica que cada um abriria, são **[nao verificado]**. (`app/docs/CORTES.md:426`, `app/docs/CORTES.md:323`)

**S — dois dias com perguntas mortas.** O anexo não mostra a razão completa da morte C6; não permite concluir que se deve reabrir, ressuscitar ou emitir outra pergunta. Os degraus do C6 incluem validação, tranca, identidade e materialização antes de considerar o veredito acusador. Ator/ato residual: **sem regra específica localizada**. (`casos_v15.out:7`, `casos_v15.out:10`, `app/chamados/juizes.py:1325`, `app/chamados/juizes.py:1336`, `app/chamados/juizes.py:1362`, `app/chamados/juizes.py:1368`; regra residual: `[nao verificado]`)

`dono=ESTRUTURA` é o dono medido da divergência, não uma atribuição automática de tarefa ao ADMIN. A ação de preencher período vem do corte específico; isso não reclassifica o resultado do oráculo informado no anexo. (`app/docs/LEIS.md:122`, `app/docs/CORTES.md:319`, `casos_v15.out:3`)

**Quem escreve e quem lê os campos envolvidos nesta decisão:**

| Campo | Escrita | Leitura relevante |
|---|---|---|
| `CelulaDia.veredito` | `classificar_dia` produz os códigos; o mapa deriva `indefinida`/`fato_em_ausencia`; o cartório entrega à porta canônica `lavrar_veredito`, que atribui o campo. (`app/ponto/supra_juiz.py:388`, `app/ponto/supra_juiz.py:404`, `app/escala/models.py:1615`, `app/escala/models.py:1619`, `app/ponto/services/cartorio.py:1813`, `app/ponto/portas/celula.py:1035`) | Retenção da folha, guarda da fábrica e C6. A grade transporta o veredito; a tarefa não possui um ramo direto para esses dois vereditos. (`app/folha/export.py:1233`, `app/chamados/services/fabrica_da_celula.py:120`, `app/chamados/juizes.py:1372`, `app/escala/services/leitor_celula.py:424`, `app/chamados/services/tarefa_do_dia.py:202`) |
| `CelulaDia.trabalha` | Porta de geração: criação/atualização futura; regeneração; edição humana; reversão; tratamento de fora de vigência. (`app/ponto/portas/celula.py:854`, `app/ponto/portas/celula.py:873`, `app/ponto/portas/celula.py:272`, `app/ponto/portas/celula.py:603`, `app/ponto/portas/celula.py:788`, `app/ponto/portas/celula.py:243`) | `dia_sem_trabalho` lê junto com a ata. O leitor da célula transporta esse insumo; a tarefa pergunta ao juiz por esse transporte. (`app/chamados/juizes.py:435`, `app/escala/services/leitor_celula.py:428`, `app/ponto/supra_juiz.py:65`, `app/chamados/services/tarefa_do_dia.py:202`) |
| `CelulaDia.protestos` | Códigos de `classificar_dia`; persistência canônica em `lavrar_veredito`. (`app/ponto/supra_juiz.py:388`, `app/ponto/supra_juiz.py:404`, `app/ponto/portas/celula.py:1011`, `app/ponto/portas/celula.py:1034`) | A folha cruza protestos com os códigos não cobráveis. A tarefa trabalha com lâmpadas/perguntas/decisões, sem consultar diretamente `protestos`. (`app/folha/export.py:1245`, `app/chamados/services/tarefa_do_dia.py:163`, `app/chamados/services/tarefa_do_dia.py:224`) |
| `PerguntaDisputa.celula` | `abrir` busca a célula do dia do fato, atribui no reuso e na inserção; o rechaveamento também altera a FK. (`app/chamados/models.py:2893`, `app/chamados/models.py:2965`, `app/chamados/models.py:3014`, `app/chamados/services/fio.py:1034`) | C6 lê célula/veredito/via; C17 seleciona e agrupa pela FK. **A tarefa carrega perguntas por colaborador, `data_evento` e `conversa=False`, sem filtro pela FK.** (`app/chamados/juizes.py:1338`, `app/chamados/juizes.py:1502`, `app/chamados/services/tarefa_do_dia.py:324`) |
| `instancias` da tarefa | Projeção em memória: `instancias_do_turno` combina posição, lâmpada e teto; não é estado persistido. (`app/chamados/services/tarefa_do_dia.py:7`, `app/chamados/services/tarefa_do_dia.py:91`, `app/chamados/services/tarefa_do_dia.py:390`) | `_tarefa_pura` percorre as instâncias; `_portas_da_tarefa` reúne suas ações para produzir `fio`, `fila`, `verbos` e `rota`. (`app/chamados/services/tarefa_do_dia.py:235`, `app/chamados/services/tarefa_do_dia.py:425`) |
| Lâmpada e identidade de instância | Cartório produz as lâmpadas e lavra a ata; o leitor transporta `acesa` como `acesa_lavrada`; a tarefa transforma em `lampada`. `instancia_marco` da pergunta é derivada na inserção. (`app/ponto/services/cartorio.py:565`, `app/ponto/services/cartorio.py:704`, `app/escala/services/leitor_celula.py:137`, `app/chamados/services/tarefa_do_dia.py:84`, `app/chamados/models.py:3440`) | A tarefa usa `lampada` nos descartes e `instancia_marco` para associar perguntas. Lâmpada acesa pode ser dispensa por ausência, sem batida realizada. (`app/chamados/services/tarefa_do_dia.py:244`, `app/chamados/services/tarefa_do_dia.py:106`, `app/ponto/services/cartorio.py:558`) |

Para a classe P, os insumos adicionais têm caminho explícito: ADMIN escreve `hora_ini/hora_fim` por `decidir → _aplicar_periodo`; a grade escreve `cobertura_parcial`/`periodo_parcial`; o cartório transporta para a ata; `parcial_so_lista` lê esses carimbos. (`app/ponto/services/ausencia.py:891`, `app/ponto/services/ausencia.py:924`, `app/escala/utils.py:1230`, `app/ponto/services/cartorio.py:732`, `app/ponto/services/cartorio.py:395`)

**Compatibilidade dos dois casos (b) com o dict existente:** sim, a ação `validar`, o ator `admin` e as chaves `{acao, de, porta}` já existem. `_pela_decisao` já produz essa combinação para uma última resposta textual. Não exige chave nova. (`app/chamados/services/tarefa_do_dia.py:49`, `app/chamados/services/tarefa_do_dia.py:111`, `app/chamados/services/tarefa_do_dia.py:132`)

A representação cabe mesmo com `porta=None`: isso acontece quando a âncora está fora da fila, e o resumo ainda pode ser `fio='admin'`. **Representar a obrigação não demonstra uma porta alcançável**; nos dois registros, a âncora está resolvida e `porta=null`. (`app/chamados/services/tarefa_do_dia.py:125`, `app/chamados/services/tarefa_do_dia.py:425`, `casos_v15.out:2`, `casos_v15.out:14`)

Os três arquivos citados **ainda não consomem esse dict nesta cópia**; são fontes da tarefa ou mantêm respostas próprias. O módulo declara os respondedores anteriores vivos até a E3. (`app/chamados/services/tarefa_do_dia.py:27`)

| Arquivo | O que lê atualmente |
|---|---|
| `painel.py` | Estado do chamado (`VIVOS`/`COBRANCA`), `silenciado_ate` e relógio; o marcador lê máximos de criação/atualização/resolução dos chamados e resposta/materialização/validação das perguntas. Fornece a fila à tarefa. (`app/chamados/services/painel.py:63`, `app/chamados/services/painel.py:153`, `app/chamados/services/painel.py:188`, `app/chamados/services/tarefa_do_dia.py:363`) |
| `declaracao_texto.py` | Grade: `isento_ponto`, `dias`, `data`, `tipo_dia`, cobertura parcial, `celulas`, `marcos`, `missing`, tipo e hora. Compartilha a função de posição do marco com a tarefa. (`app/chamados/services/declaracao_texto.py:75`, `app/chamados/services/declaracao_texto.py:100`, `app/chamados/services/declaracao_texto.py:118`) |
| `chamados/juizes.py` | `alimentar_fila` lê `particionar_verbos(...)[mapa]`, PK/FK dos chamados na fila e fornece `verbo`, `fila_ids`, `fila_cel`. C17 lê perguntas por FK, C6, âncora e presença no app; produz seu próprio `{rota, fila, verbos, fio}`. Seu campo `instancias` é um mapa de colaboradores, diferente das instâncias de marco. (`app/chamados/juizes.py:1452`, `app/chamados/juizes.py:1499`, `app/chamados/juizes.py:1580`, `app/chamados/juizes.py:1588`) |

## 11/11 01:21 · sessao bdbe00fb78b9 · - · - · origem=Ronald (tmux)

Leitura somente; nenhum acesso a banco/log. As observações de produção abaixo vêm do anexo fornecido, sem arquivo local numerado. [não verificado]

**Ramos e regras**

- Nos sete `fato_em_ausencia`, `razao='dia_sem_trabalho'` identifica o descarte em `_tarefa_pura`: calcula `sem_trabalho` em `app/chamados/services/tarefa_do_dia.py:202` e interrompe cada instância em `:241-243`, antes de ler a pergunta ou decisão em `:268-283`.
- Nos oito `PARCIAL_SEM_PERIODO`, `razao='aceso'` identifica o descarte em `app/chamados/services/tarefa_do_dia.py:244-246`, também anterior à cobertura e às decisões (`:258-283`).
- `fio='nada'` resulta de nenhuma ação e `nao_sei=False`; não resulta diretamente da inexistência de pergunta: `app/chamados/services/tarefa_do_dia.py:425-434`.
- O corte específico das parciais decide **(c), ADMIN pela lista de cadastro**: literal “hora_ini/hora_fim no lancamento, preenchidos pelo ADMIN na aprovacao”; sem período, “o dia vai a lista do admin como CADASTRO ('parcial sem periodo'), nunca FATO_EM_AUSENCIA”. `app/docs/CORTES.md:319`. A edição posterior à aprovação é explicitamente pela mesma porta `ausencia.py::decidir`, com trilha. `app/docs/CORTES.md:323`.
- Para os dois dias com resposta viva (`col252/29-09`, p38162; `col841/10-10`, p42367), existe fundamento para **(b), ADMIN validar a resposta já existente**: HX-ADMIN faz resposta perder apenas para decisão explícita do admin na célula; cartório não equivale a admin (`app/chamados/juizes.py:1338-1353`). A pergunta respondida só morre por concordância com a luz se o dia não acusa (`:1279-1286`, `:1321`). O corte reforça literalmente “segue VIVA, so Validar” e “a resposta do colab so perde para decisao do admin NA CELULA”, embora no contexto de desligamento (`app/docs/CORTES.md:342`).
- **Limite:** o anexo não mostra `DecisaoPergunta`, texto da resposta, passivo da projeção nem identidade `via_resolucao`. Portanto não prova que o ramo `_pela_decisao` executaria `VALIDAR`: projeção sem linha retorna `nao_sei` em `app/chamados/services/tarefa_do_dia.py:127-135`; a carga usa a sequência e o passivo em `:337-347`. [não verificado: esses campos nos dois dias]
- Para os três `fato_em_ausencia` sem pergunta nem chamado (`col177`, `col292/28-10`, `col889`), o corte **GRUPO-D-E-A-OUTRA-METADE** nomeia expressamente o **emissor**: “dia acusando sem pergunta e sem chamado”, “cura pelo emissor que le a lampada”. `app/docs/CORTES.md:263`. A obrigação geral nasce do sinal da célula: `CLAUDE.md:167-170`.
- `trabalha=True` não implica `dia_sem_trabalho=False`: o juiz também lê `ata.tipo_dia`, exceto a ausência com `cobertura_parcial`; `app/chamados/juizes.py:426-441`; corte literal `app/docs/CORTES.md:416`. O anexo não traz o `tipo_dia` bruto. [não verificado]

**Dict existente / compatibilidade para (b)**

`validar` + `admin` já existem (`app/chamados/services/tarefa_do_dia.py:49-54`), usando a forma `{'acao','de','porta'}` (`:111-112`). `_pela_decisao` já emite essa ação para resposta textual em `:132-135`. A porta pode ser `None` se a âncora não pertence à fila (`:125-126`); mesmo sem porta inteira, a ação produz `fio='admin'` (`:425-434`). Portanto atribuir validação por instância cabe nas chaves e atores existentes; existência de uma porta alcançável para os dois dias não foi comprovada. [não verificado: porta utilizável]

O vocabulário completo de ações é `perguntar`, `responder`, `validar`, `materializar`, `rejulgar`, `decidir_ausencia`, `entregar_documento`; atores `sistema`, `colaborador`, `admin`; portas de sistema `emissor`, `cartorio`, `materializacao`. `app/chamados/services/tarefa_do_dia.py:49-54`. Não existe ação nomeada “editar período” nem “decidir conflito fato/ausência” nesse vocabulário. `:49-54`.

**Censo dos três arquivos solicitados**

Nenhum dos três é leitor do dict de `tarefa_do_dia` nesta cópia; o próprio módulo declara que os leitores ainda não migraram (`app/chamados/services/tarefa_do_dia.py:27-35`).

| Arquivo | Leitura atual relevante |
|---|---|
| `painel.py` | `ChamadoColaborador.status_local ∈ VIVOS` (`app/chamados/services/painel.py:135-155`); fila = `status_local ∈ COBRANCA`, excluindo silêncio futuro (`:188-190`); marca de perguntas lê máximos de `respondida_em`, `materializada_em`, `validada_em` (`:63-68`). É fornecedor da fila da tarefa, não consumidor do dict. |
| `declaracao_texto.py` | `grade_da_celula`, `isento_ponto`, dias por `data`, `tipo_dia`, `cobertura_parcial`, `celulas`, `marcos`, `missing`, tipo/hora; devolve faltantes (`app/chamados/services/declaracao_texto.py:100-139`). Compartilha `posicao_do_marco` com tarefa (`:75-83`) e mapa motivo/posição (`:42-57`). |
| `juizes.py` | `alimentar_fila` lê `particionar_verbos(qs_vivos())['mapa']` e `qs_na_fila().values_list('pk','celula_id')`, devolve `verbo`, `fila_ids`, `fila_cel` (`app/chamados/juizes.py:1452-1470`), fornecedor da tarefa. C17 carrega perguntas por `celula_id`, aplica C6, consulta presença no app (`:1502-1522`) e devolve suas próprias `rota`, `fila`, `verbos`, `fio` (`:1575-1589`). |

Atenção: `alimentar_portas()['instancias']` é um mapa de objetos `Colaborador`, não as instâncias de marco da tarefa. `app/chamados/juizes.py:1499-1508`.

**PerguntaDisputa.celula e instâncias**

| Campo | Escritor | Leitor |
|---|---|---|
| `PerguntaDisputa.celula` | Nascimento/reuso pela porta `PerguntaDisputa.abrir` (`app/chamados/models.py:2776-2781`); busca célula do colaborador e dia do fato em `:2893-2896`, atualiza FK do reuso em `:2965-2967`, insere em `:3014-3017`. Rechaveamento explícito também troca FK em `app/chamados/services/fio.py:1034-1043`. | C6 acessa `p.celula`, veredito e via (`app/chamados/juizes.py:1338-1340`, `:1371-1375`); C17 agrupa por FK (`:1502-1505`). A tarefa carrega perguntas por `(colaborador_id,data_evento,conversa=False)`, não por FK (`app/chamados/services/tarefa_do_dia.py:324-329`). |
| Instâncias da tarefa | Derivação pura `instancias_do_turno`: posições das lâmpadas da grade, tipo/hora e casamento com teto (`app/chamados/services/tarefa_do_dia.py:57-102`); carga em `:390-394`; não são estado persistido (`:7-9`). | `_tarefa_pura` percorre instâncias em `:235-240`; `_portas_da_tarefa` reúne suas ações em `:425`. |
| Lâmpada usada nas instâncias | A tarefa transporta `acesa_lavrada` da grade em `app/chamados/services/tarefa_do_dia.py:84-88`; a grade lê `ata.lampadas[].acesa` em `app/escala/services/leitor_celula.py:114-137`. | Triestado decide `aceso`/`lampada_nao_sabe` em `app/chamados/services/tarefa_do_dia.py:244-250`. |
| Identidade de instância da pergunta | Mapa motivo→posição em `app/chamados/services/declaracao_texto.py:42-57`; inserção deriva identidade no `save`, conforme `app/chamados/models.py:3009-3010`. | `_pos_da_pergunta` usa `p.instancia_marco` ou mapa do motivo (`app/chamados/services/tarefa_do_dia.py:106-108`), agrupa por posição em `:233-234`. |

A tarefa não lê `protestos` nem `cel.veredito` diretamente; lê a grade e as lâmpadas, o juiz de dia sem trabalho, cobertura, perguntas, decisões e lavra de materialização. `app/chamados/services/tarefa_do_dia.py:163-176`, `:202-211`, `:224-288`, `:348-352`.

## 11/11 01:21 · sessao 1a948378d99c · - · - · origem=Ronald (tmux)

Parecer de leitura:

- **Três FATO sem pergunta e sem chamado**: a responsabilidade **genérica do emissor** tem regra literal em `app/docs/CORTES.md:263`: “cura pelo emissor que le a lampada”, incluindo o contador “celula acusa + 0 pergunta + 0 chamado”. O próprio censo dessa linha inclui `fato_em_ausencia`. Essa cláusula não condiciona sua aplicação ao `lampada=False` das instâncias. **Qual pergunta/chamado e qual ação humana deve nascer para o conflito FATO_EM_AUSENCIA: sem regra específica encontrada** em LEIS/CORTES. O histórico efetivo da não emissão nesses três dias permanece [não verificado].

- **Dois FATO com resposta C6 viva**: há regra para leitura da pergunta viva e apresentação ao ADMIN: `app/docs/CORTES.md:316`, FIO-LE-O-JUIZ-6984, manda carregar “pela pergunta_viva, nao pelo estado da disputa”, sem reabrir; `app/docs/CORTES.md:337`, item 3, explicita “respondida, so falta Validar”; `app/docs/CORTES.md:342` reafirma “segue VIVA, so Validar” e que resposta só perde para decisão ADMIN na célula. A tarefa corta antes de examinar a pergunta em `app/chamados/services/tarefa_do_dia.py:241`–243. **A última DecisaoPergunta, pertencimento real pela FK e passivo da projeção não estão no anexo** [não verificado]; portanto, não identificar o ramo `_pela_decisao` como provado para essas perguntas.

- **Oito PARCIAL_SEM_PERIODO**: **c = ADMIN/DP, preencher período pela lista e porta da ausência**, com regra específica. `app/docs/CORTES.md:319`, COBERTURA-PARCIAL-PERIODO: “hora_ini/hora_fim [...] preenchidos pelo ADMIN na aprovacao”; sem hora, “o dia vai a lista do admin como CADASTRO ('parcial sem periodo')”. `app/docs/CORTES.md:323`, JUIZ-PERIODO-DA-PARCIAL(ii), exige edição posterior da aprovada pela mesma `ausencia.py::decidir`. O emissor deve calar: `app/ponto/services/cartorio.py:379`–382, “NUNCA chamado”, sem condição de fato novo; implementação retorna `True` em `:398`–399, lida pela fábrica em `app/chamados/services/fabrica_da_celula.py:123`–125. A tarefa descarta a instância acesa em `app/chamados/services/tarefa_do_dia.py:244`–246. A regra específica não manda tarefa fabricar pergunta de marco para essa classe.

- **Dois FATO com perguntas C6 mortas**: **sem regra específica de ator/ação que resolva o conflito residual** encontrada. Não inferir reabertura nem novo pedido de marco. `app/docs/CORTES.md:364`, CODEX-T16-T17-REGISTRO, manda medir “celula acusando e pergunta morta por identidade sem reparador”; não decide o ator. Motivo concreto da morte C6 nesses dois dias [não verificado]: faltam vias, materializada_em, respostas, decisões, `PerguntaDisputa.celula` e ata completa.

Distinções necessárias:

- `dono=ESTRUTURA` é dono da divergência; a lei específica pode atribuir o ato de cadastro ao ADMIN. `app/docs/LEIS.md:122` distingue donos e diz que CADASTRO/BATIDA são lista do ADMIN; não converte automaticamente ESTRUTURA em tarefa humana.
- `lampada=True` da instância não prova batida realizada: a ausência pode dispensar e acender “sem luz”, `app/ponto/services/cartorio.py:558`–562.
- Vigia causa **d** não significa dono CADASTRO: é “colab na lista, dia fora”, sem pergunta viva, `app/chamados/services/vigia_fio.py:523`–526 e `:677`–678. Causa **c** é pergunta viva respondida com âncora terminal, `:675`–676.
- `sem_porta` é resultado de alcançabilidade, não julgamento de quem devia emitir: corte literal em `app/chamados/juizes.py:1546`–1551; acesso manual ao chamado fechado e “Resolver dia” ficam fora dessa porta em `:1569`–1571.
- C6 mata validada em `app/chamados/juizes.py:1325`–1326; preserva resposta textual sem decisão ADMIN da célula em `:1341`–1353; acusações da célula dão viva em `:1372`–1375, depois dos degraus anteriores.

Porta existente para os oito parciais: escritor de hora é `_aplicar_periodo`, `app/ponto/services/ausencia.py:891`–894; `decidir` recebe os horários em `:897`–898, permite editar aprovada em `:922`–925 e grava trilha em `:929`–934.

## 11/11 01:21 · sessao 32dd6af07060 · - · - · origem=Ronald (tmux)

Leitura concluída; referências centrais:

| Classe | Regra literal e alcance | Emissão atual |
|---|---|---|
| 8 `PARCIAL_SEM_PERIODO` | “hora_ini/hora_fim [...] preenchidos pelo ADMIN”; sem hora, “lista do admin como CADASTRO”. A edição após aprovação usa `ausencia.py::decidir`. (`app/docs/CORTES.md:319`, `app/docs/CORTES.md:323`) | `parcial_so_lista` retorna `True` para `sem_periodo`; fábrica retorna `[]`, expressamente “sem pergunta, sem pai, sem push”. (`app/ponto/services/cartorio.py:370`, `app/ponto/services/cartorio.py:398`, `app/chamados/services/fabrica_da_celula.py:123`) |
| 7 `FATO_EM_AUSENCIA` | Regras encontradas determinam quando acusar: parcial só acusa com fato dentro do período; resposta `nao_sei` conserva acusação anterior. Não encontrei regra específica escolhendo ator/ato para resolver esse conflito. (`app/docs/CORTES.md:315`, `app/docs/CORTES.md:334`; escolha do ato: **sem regra** no material examinado) | Fábrica descarta `dia_sem_trabalho`; chamado automático só possui gatilhos `FURO_*`/limbo, sem gatilho exclusivo para `FATO_EM_AUSENCIA`. (`app/chamados/services/fabrica_da_celula.py:112`, `app/ponto/services/cartorio.py:1707`, `app/ponto/services/cartorio.py:1773`) |

Para as parciais, **(c), ADMIN/DP pela lista de cadastro**, tem fundamento literal; a obrigação específica de a `tarefa_do_dia` projetar esse ato não aparece nesse corte. O corte do juiz fala em ações por **instância de marco**. (`app/docs/CORTES.md:319`, `app/docs/CORTES.md:426`)

A regra geral manda emissão derivada da célula; O261 explicita fio obrigatório e responsabilidade do emissor para seu grupo D. Isso não escolhe sozinho o ato específico de resolver `FATO_EM_AUSENCIA`. (`CLAUDE.md:168`, `app/docs/CORTES.md:263`)

Escritores e transporte:

- `veredito`/`protestos`: códigos nascem em `classificar_dia` — parcial em `app/ponto/supra_juiz.py:388` ou `:441`; fato em ausência em `:404` ou `:444`. Mapa para veredito em `app/escala/models.py:1615` e `:1619`; cartório chama lavra em `app/ponto/services/cartorio.py:1813`; escritor canônico grava em `app/ponto/portas/celula.py:1034`.
- `trabalha`: criação em `app/ponto/portas/celula.py:854`, atualização futura em `:873`, regeneração em `:272`, edição humana em `:603`, reversão em `:788`.
- Período parcial: `ausencia.py::decidir` chama escritor único `_aplicar_periodo` em `app/ponto/services/ausencia.py:925`; atribuição em `:893`.
- Grade escreve `tipo_dia`, `cobertura_parcial`, `periodo_parcial` em `app/escala/utils.py:1220`, `:1230`, `:1234`; cartório transporta para ata em `app/ponto/services/cartorio.py:704` e `:732`; fábrica lê em `app/chamados/services/fabrica_da_celula.py:123`.
- `dia_sem_trabalho` lê `trabalha`, `ata.tipo_dia`, `ata.cobertura_parcial`; parcial só cala se `trabalha=False`. (`app/chamados/juizes.py:435`)

Limites: o anexo não fornece `ata.tipo_dia`, `ata.cobertura_parcial`, `ata.periodo_parcial`, registros de ausência ou execução histórica da fábrica. Atribuir o guarda efetivamente executado na emissão histórica é **[não verificado]**. O dado mostra instâncias com `lampada=True` e ações vazias; isso não demonstra marco apagado. **[não verificado: anexo fornecido sem linhas]**

## 11/11 01:19 · sessao 26d759465242 · FIO-E0-V4-E4B · construtor · origem=Code

Volta 3 escrita. [Entrega com P1–P6, censo e comandos](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/E4B_ALCANCE_ENTREGA.txt:10).

- **51 testes OK; 31 mutantes mortos; quatro REDs falham na volta 1.** [Resultados](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/E4B_ALCANCE_ENTREGA.txt:126). [nao verificado]
- C20 falhou por Django ausente. [Registro](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/E4B_ALCANCE_ENTREGA.txt:141).
- Permanece a pendência herdada da fotografia de reversão da cascata. [Detalhes](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/E4B_ALCANCE_ENTREGA.txt:384).

Backfill real não executado; ORM, suite inteira e certificação: **[nao verificado]**. [nao verificado]

## 11/11 01:07 · sessao 37bca8857cf8 · BO-DISPUTA-SEM-PERGUNTA · verificador · origem=Code

**A c20 cura a passagem comum do agrupador, mas a cura na origem continua parcial:** declaração e pendentes alcançam os escritores de destino sem consultar o índice; o replay ainda rederiva episódio quando o valor conservado é `None`. (`app/chamados/services/disputa_emissao.py:1001`, `:300`, `:505`, `:791`, `:799`; `app/chamados/models.py:2866`.)

Nas referências abreviadas, `disputa_emissao.py`, `fio.py`, `descarte_disputa.py` e `declaracao_texto.py` ficam em `app/chamados/services/`; `models.py`, em `app/chamados/`; testes, em `app/chamados/tests/`; `auditar_invariantes_chamados.py`, em `app/chamados/management/commands/`. [nao verificado]

1. **Estado dos achados da preverif4.r.**

   | Achado anterior | Estado nesta cópia |
   |---|---|
   | Exceção do gerador deixar nascimento incompleto | **Curado no núcleo:** pai, DS e perguntas participam do atomic; exceção propagada desfaz o conjunto. `disputa_emissao.py:638`, `:668`, `:685`, `:689`. |
   | Geração vazia confirmar nascimento, inclusive pela chamada vazia da grade | **Curado no núcleo:** sem pergunta local admitida, desfaz o nascimento. `disputa_emissao.py:690`, `:696`, `:698`. |
   | Canônica estrangeira deixar nova DS sem pergunta | **Curado para esse mecanismo:** núcleo, declaração e agrupador desfazem o nascimento local; permanecem as ressalvas de replay e destino abaixo. `disputa_emissao.py:696`, `:725`, `:792`, `:795`, `:1487`. |
   | API pública de declaração aceitar ausência de pergunta | **Curado:** `pergunta` é obrigatório; o ensaio sem pergunta ficou privado. `disputa_emissao.py:738`, `:762`, `:766`. |
   | Seed `orfao_14h` indevidamente conservado no lote com seed estrangeiro | **Curado para motivo novo:** começa sem seed e só o introduz se o próprio seed não foi estrangeiro. Não remove cópias preexistentes. `disputa_emissao.py:1312`, `:1469`, `:1472`. |
   | Segundo agregador na âncora | **Curado:** âncora entrega `['ancora_ausente']` ao agregador; união/save ficaram nele. `disputa_emissao.py:575`, `:581`, `:1582`, `:1613`. |
   | Luz HX sem leitor | **Curado no instrumento:** I7a/I7b usam zero linhas de pergunta e distinguem ausência de ata de silêncio estrutural. Medida atual: **[nao verificado]**. `auditar_invariantes_chamados.py:62`, `:69`, `:73`. |
   | Dona fechada como pergunta de lei aberta | **Não identifico nova pergunta de lei; implementação parcial da trilha/alarme**, conforme item 2. O agregador não salva a fechada; reabertura do sistema continua em porta própria. `disputa_emissao.py:567`; `models.py:2163`. |
   | Replay rederivar episódio pelo contexto da dona | **Parcial:** episódio positivo é conservado; `None` ainda permite rederivação. `disputa_emissao.py:504`; `models.py:2866`. |
   | Agrupador analisar chamado após replay divergente | **Curado nesse ramo:** exige destino confirmado e não recusado. `disputa_emissao.py:1498`, `:1508`, `:1510`. |
   | Dois mutantes abortarem na montagem | **Curado:** montagem fora do runner; alvos precisam falhar por assertion, sem errors. `test_bo_disputa_volta5_memoria.py:389`, `:396`, `:415`. |
   | “Paridade histórica” comparar a fonte atual consigo mesma | **Curada a alegação:** agora se chama repetibilidade e declara o limite. Comparação histórica continua **[nao verificado]**. `test_bo_disputa_volta3_memoria.py:252`, `:254`. |
   | Pendentes fora do atomic do núcleo | **Igual:** wrapper chama depois do retorno; helper abre P e depois agrega DS, sem atomic próprio. `disputa_emissao.py:836`, `:850`, `:300`, `:314`. |
   | Novos motivos admitidos em dona estrangeira não chegarem ao push correspondente | **Igual no núcleo misto:** agregação estrangeira não coleta seus `novos`; wrapper usa os `novos` da local. `disputa_emissao.py:623`, `:662`, `:665`, `:851`. | [nao verificado]
   | Push imediato poder anteceder confirmação final | **Igual no agrupador:** complementos e principal notificam dentro do atomic externo, sem `on_commit`. `disputa_emissao.py:1238`, `:1268`, `:1516`, `:1537`. | [nao verificado]
   | Nem todos os retornos `(p, criada)` serem integralmente consumidos | **Igual literalmente:** primeira tentativa da declaração conserva P e descarta o booleano em `_`; confere a dona. `disputa_emissao.py:791`, `:792`. |
   | Doubles não certificarem cartório/signals/concorrência | **Igual como limite de prova.** O teste c20 declara essas fronteiras simuladas. `test_bo_disputa_volta5l_memoria.py:6`. | [nao verificado]
   | Protegidos historicamente intocados, efeitos externos, produção e suíte inteira | **[nao verificado]**. | [nao verificado]

   A ressalva do replay é concreta: o emissor conserva `batida_origem_id=None`, mas `_nascer` executa `valor or derivador(dona)`. Se a dona aponta uma batida 222, o replay ganha esse episódio; se ela pertence a outro turno, também pode mudar o dia efetivo. A guarda pode desfazer o replay divergente, mas isso não reproduz o fato original. (`disputa_emissao.py:504`, `:725`, `:726`; `models.py:2866`, `:2875`; `fio.py:1633`.)

   Reproduzi em memória a sequência `(pedido=None, efetivo=None)` → `(pedido=None, efetivo=222)`, terminando em `replay-divergente`, sem costura confirmada. O teste F2 existente cobre somente episódio positivo 111; ORM desse contraexemplo: **[nao verificado]**. (`test_bo_disputa_volta5f_memoria.py:115`, `:122`, `:130`; `disputa_emissao.py:729`.) [nao verificado]

2. **c15/c16 — trilha, contador e saídas sem trilha.**

   **Quem escreve:** o agregador encaminha a fechada ao registrador; este fotografa motivos/fechamento, elimina motivos já presentes e agenda a gravação depois do commit. A escrita persistente passa por `core.observ.evento`. (`disputa_emissao.py:567`; `descarte_disputa.py:9`, `:10`, `:31`, `:37`; `app/core/observ.py:140`.)

   A identidade deduplicada é **dona + motivo + pergunta**, inclusive pergunta `None`; origem é procedência. O callback trava C→D em transação própria. (`descarte_disputa.py:20`, `:23`, `:25`, `:27`.)

   **Quem lê:** I23 lê todas as linhas históricas daquela ação/modelo; o comando publica o resultado e está declarado no cron. Conta **linhas de descarte**, não donas distintas, motivos atualmente pendentes ou janela temporal. (`auditar_invariantes_chamados.py:81`, `:647`, `:893`, `:1925`; `app/config/crons.py:542`.)

   Portanto, **contador e trilha têm o mesmo universo das linhas efetivamente gravadas**. Não cobrem necessariamente todos os descartes tentados ou confirmados no negócio. (`descarte_disputa.py:31`; `auditar_invariantes_chamados.py:81`.)

   Há estas saídas sem nova linha:

   - **Deliberadas:** motivo já presente; identidade já registrada; savepoint desfeito. (`descarte_disputa.py:10`, `:27`, `:35`.)
   - **Falha da auditoria:** callback ocorre depois do commit; `engolir` captura falhas, e `evento` também pode retornar `None`, ignorado pelo registrador. O descarte pode ficar confirmado sem trilha; I23 fica cego. Ocorrência em produção: **[nao verificado]**. (`descarte_disputa.py:19`, `:31`, `:37`; `app/core/observ.py:92`, `:111`, `:139`, `:145`.)
   - **Dedup antes do agregador:** suponha dona fechada com P local viva cujo motivo falta em `DS.motivos`, num chamado que não segue o caminho por ata. O helper soma o motivo em memória, mas retorna `False` pelo dedup; o complemento termina sem criação, o agrupador não chama o agregador/trilha e pode excluir o chamado e retornar. Reprodução ORM: **[nao verificado]**. (`disputa_emissao.py:1003`, `:1007`, `:1069`, `:1278`, `:1292`, `:1300`, `:1304`, `:1661`.)

   **Alarme parcial:** I23 positivo aparece como `VIOL`, mas **sozinho não provoca o `SystemExit(2)`**: essa decisão consulta somente F1/F2. O teste 5g exige o alarme F1/F2, além da linha I23; não prova alarme independente de I23. Consumidor externo específico: **[nao verificado]**. (`auditar_invariantes_chamados.py:95`, `:1942`; `test_bo_disputa_volta5g.py:101`, `:104`, `:107`.) [nao verificado]

3. **c19 — seed e caminhos sem pergunta.**

   Nova ou aberta vazia começa sem motivos; recebe `orfao_14h` ao final, salvo reuso estrangeiro **do próprio seed**. Estrangeira de outro motivo não impede o seed; motivos preexistentes são preservados. (`disputa_emissao.py:1312`, `:1313`, `:1469`; `test_bo_disputa_volta5k_memoria.py:54`, `:79`.)

   | Caminho | Resultado |
   |---|---|
   | **HX-ATA-NAO-SABE**, abstenção por `sem_celula` | Marca abstenção; com possibilidade de fechamento, conserva aberta e acende luz. `disputa_emissao.py:1088`, `:1094`, `:1383`, `:1525`. |
   | **h2**, ata disponível e nada a perguntar | Nasce e fecha vazia; zero P permite o fechamento. `disputa_emissao.py:1524`, `:1530`; `models.py:1853`, `:1903`. |
   | Nova, somente perguntas estrangeiras | Desfaz nascimento local, repete na dona e retorna `None`. `disputa_emissao.py:1487`, `:1494`, `:1513`. |
   | Aberta preexistente sem P | Não recebe esse rollback, que exige `nova`; segue ao fechamento/HX. `disputa_emissao.py:1487`, `:1524`. |
   | Exceção propagada durante o nascimento | Desfaz o atomic; não confirma nascimento vazio. `disputa_emissao.py:1238`, `:1316`. |
   | **Recusa técnica sem testemunho de ata** | Falta de data/base, marcos inválidos ou falha de leitura pode terminar zero P e fechar. Não demonstra as premissas estritas de HX ou h2. `disputa_emissao.py:1054`, `:1083`, `:1362`, `:1418`, `:1445`, `:1448`, `:1524`. |

   **Existe caminho fora das duas premissas.** Contraexemplo: chamado `batida_ausente`, aberto, `contexto_json={}`, sem origem/célula/pergunta anterior. Regularizabilidade aceita pelo mapa; não segue ata; o laço sem origem abandona-o por falta de data. A DS recebe o seed e fecha vazia **sem consultar ata nem acender luz**. (`models.py:3667`; `disputa_emissao.py:925`, `:930`, `:949`, `:955`, `:1121`, `:1337`, `:1445`, `:1469`, `:1524`.)

   Esse caminho foi reproduzido com funções produtivas via AST e infraestrutura em memória: regularizável, zero P, fechada, zero chamadas à ata/luz. Isso não certifica os efeitos ORM/signals: **[nao verificado]**. A infraestrutura de fechamento é explicitamente double. (`test_bo_disputa_volta5b_orfaos_memoria.py:77`, `:84`, `:96`.)

4. **c20 — origem e equivalência de `(motivo, dia)`.**

   | Parte | Índice | Caminho B |
   |---|---|---|
   | Fonte | `ch.contexto_json['marcos_faltantes']`. `disputa_emissao.py:1252`. | A mesma lista do chamado sobrevivente. `disputa_emissao.py:1356`. |
   | Motivo | `tipo == 'S'` → saída; qualquer outro → volta. `:1259`. | Mesma expressão. `:1359`. |
   | Dia | `date.fromisoformat(marco.get('data'))`. `:1256`. | Mesma expressão, depois envolvida por `dia_do_turno_da_celula`. `:1361`, `:1367`. |
   | Exceções | `TypeError`, `ValueError`. `:1257`. | `Exception`. `:1362`. |
   | Universo | Regularizáveis antes dos complementos. `:1250`. | Sobreviventes do filtro posterior aos complementos. `:1291`, `:1297`, `:1351`. |

   **Para JSON ordinário, mesmo marco e estado estável, motivo/dia concordam.** `DiaDoTurno` é `NewType`; o construtor usado por B não converte a data em execução. Tipo ausente/desconhecido vira “volta” em ambos. (`disputa_emissao.py:1259`, `:1359`; `app/core/dia_do_turno.py:13`; `app/ponto/turnos.py:2939`, `:2943`.)

   **Não há equivalência universal dos dois procedimentos:** diferem no tratamento de exceções e, principalmente, no universo e momento da leitura. Divergência de exceção com dado JSON persistido ordinário: **[nao verificado]**. (`disputa_emissao.py:1257`, `:1291`, `:1297`, `:1362`.)

   **Índice com chave e B sem execução correspondente:** intervalo regularizável já possui P não materializada e não segue `cobra_marcos_da_ata`. Entra no índice, pode ser complementado e depois é excluído pelo filtro; B não o percorre. O índice continua disponível e pode direcionar o complemento do chamado genérico para esse intervalo. (`disputa_emissao.py:1250`, `:1280`, `:1291`, `:1300`, `:1351`, `:1001`.)

   **B com chave ausente no índice**, mantendo estado e contexto estáveis entre as leituras: **[nao verificado]**. A implementação não fixa o resultado do predicado entre as passagens. (`disputa_emissao.py:1250`, `:1299`.)

   Havendo chave, a passagem comum substitui o destino antes de abrir e conserva-o no replay estrangeiro; sem chave, mantém o destino recebido, sem recusa adicional. Se dois intervalos oferecem a mesma chave, `setdefault` escolhe o primeiro do lote, inclusive para pedidos posteriores do segundo. (`disputa_emissao.py:1001`, `:1018`, `:1029`, `:1260`, `:1497`.)

5. **Censo de escritores de `PerguntaDisputa.chamado_destino`.**

   | Escritor persistente | Local e alcance |
   |---|---|
   | Nascimento | Construtor/inserção em `models.py:3014`, `:3017`. |
   | Reuso da canônica | Julga e atribui destino, conservando a dona da P. `models.py:2972`, `:2980`, `:2982`, `:3006`, `:3008`. |
   | `reapontar_resolvedora` | Julga, atribui e salva. `fio.py:907`, `:909`, `:925`, `:927`. |
   | Deleção da FK | Apagar o chamado produz `NULL`; não escolhe outro chamado. `models.py:3385`, `:3387`. |

   As atribuições de `fio.py:779`/`:783` são temporárias e restauradas; `models.py:366`/`:367` reassocia instância do mesmo PK. O próprio contrato registra os dois escritores produtivos e a FK. (`fio.py:807`, `:814`.)

   **Pedidos pela passagem comum:** complemento anterior ao filtro, B, ata, grupo por origem e caminho sem origem passam pelo override c20. (`disputa_emissao.py:1291`, `:1366`, `:1387`, `:1425`, `:1431`, `:1457`, `:1462`, `:1001`.)

   **Pedidos fora dela:**

   - Pendentes chamam o gerador diretamente; ele deriva destino, abre P e pode chamar a reata. O índice não chega a esse gerador. (`disputa_emissao.py:1486`, `:300`, `:412`, `:503`, `:505`, `:519`; `app/chamados/reconciliador.py:1642`, `:1655`.)
   - Declaração chama `abrir` e, condicionalmente, `reapontar_resolvedora`, sem consultar o índice. (`disputa_emissao.py:784`, `:786`, `:791`, `:799`.)

   **Sim, permanece pedido capaz de costurar no genérico uma pergunta do intervalo explícito:** P de `intervalo_saida` em B, já destinada ao chamado `intervalo`; declaração pelo chamado genérico `ch`, do mesmo colaborador/dia, casa o marco de intervalo da ata e passa `ch` como destino. A candidata estrangeira é recusada pelo helper local; `abrir` reutiliza a canônica e pode trocar seu destino para `ch`, inclusive no replay confirmado. (`declaracao_texto.py:250`, `:254`, `:341`, `:346`, `:365`; `disputa_emissao.py:791`, `:799`; `models.py:2982`, `:3006`.)

   O administrador faz o mesmo pedido. Os juízes conferem colaborador, turno e estado do fio; não elegem preferência pelo chamado com marco explícito de intervalo. (`declaracao_texto.py:650`, `:653`; `app/chamados/juizes.py:2185`, `:2208`, `:2217`; `fio.py:771`.)

   Reproduzido em memória: mesma P/dona, destino `intervalo` → destino `ch`. A porta de reuso é double; execução ORM: **[nao verificado]**. (`test_bo_disputa_volta5c_memoria.py:101`, `:104`; `disputa_emissao.py:799`.)

   Os contraexemplos pedidos **da passagem comum** passaram em memória: só `ch`, só intervalo, ambas as ordens, dona aberta/fechada e replay 2x; também outro dia e intervalo-volta. Isso não certifica o protegido ORM. (`test_bo_disputa_volta5l_memoria.py:81`, `:87`, `:109`, `:112`, `:119`, `:126`, `:136`; `test_bo_disputa_volta5b_orfaos.py:157`, `:159`.)

6. **Quem escreve, quem lê e significado dos campos.**

   | Campo | Escritores alcançáveis | Leitores que dão significado ao delta |
   |---|---|---|
   | `DS.motivos` | Agregador/nascimentos: `disputa_emissao.py:575`, `:685`, `:1337`, `:1605`; aparo/remoção/reabertura: `models.py:1966`, `:2063`, `:2159`. | União/admissão/seed/push: `disputa_emissao.py:571`, `:606`, `:1312`, `:1469`, `:1535`; auditor: `auditar_invariantes_chamados.py:50`; foto: `app/api/foto_recebida.py:61`. |
   | `DS.fechada_em` | Fechar/reabrir admin/sistema: `models.py:1932`, `:2149`, `:2173`. | Recusa/eleição/push: `disputa_emissao.py:567`, `:621`, `:645`, `:846`, `:1267`, `:1536`; receivers: `app/chamados/signals.py:28`, `:129`, `:153`; validação: `app/chamados/services/validacao.py:214`. |
   | `DS.contexto` | Agregador/nascimentos: `disputa_emissao.py:578`, `:685`, `:1337`, `:1605`; aparo/remoção: `models.py:1967`, `:2064`. | Resolução e episódio: `disputa_emissao.py:346`, `:373`; `models.py:3463`; auditor: `auditar_invariantes_chamados.py:31`. |
   | `P.disputa` | Inserção: `models.py:3014`, `:3017`; reuso preserva-a: `:3008`; deleção da DS usa CASCADE: `:2416`. | Encaminhamento: `disputa_emissao.py:613`, `:1022`; candidata: `declaracao_texto.py:254`; autorização/listagem: `fio.py:116`, `:598`. |
   | `P.chamado_destino` | Os quatro casos do item 5. `models.py:2982`, `:3014`, `:3387`; `fio.py:925`. | Complemento/filtro: `disputa_emissao.py:1280`, `:1301`; juiz: `app/chamados/juizes.py:2150`; autorização: `fio.py:598`; materialização: `app/chamados/services/materializacao.py:1076`. |
   | `P.motivo` | Inserção: `models.py:3014`, `:3017`. | Encaminhamento: `disputa_emissao.py:614`; identidade: `fio.py:1793`; candidata: `declaracao_texto.py:250`; materialização: `app/chamados/services/materializacao.py:1077`. |
   | `P.batida_origem_id` | Argumento conservado: `disputa_emissao.py:504`; nascimento/derivação: `models.py:2866`, `:2868`, `:3449`. | Identidade e dia: `models.py:2907`; `fio.py:1633`. |
   | `P.data_evento` | Porta de atribuição: `fio.py:790`, `:800`; nascimento passa por ela: `models.py:3016`. | Dedup/chave: `disputa_emissao.py:402`, `:1001`, `:1661`; juiz: `fio.py:1608`. |
   | Trilha nova | Registrador → `evento`: `descarte_disputa.py:31`; inserção: `app/core/observ.py:140`. | Dedup: `descarte_disputa.py:27`; I23: `auditar_invariantes_chamados.py:81`; apresentação: `:1925`. |

   **Semântica:** `DS.motivos` é lista de cenários; pode conter seed sem P daquele motivo. `P.motivo` participa da identidade de um fato datado/instância. Não são uma relação um a um nem uma chave global equivalente. (`models.py:1743`, `:2427`, `:3483`; `disputa_emissao.py:1469`.)

   **Dia civil versus turno:** conversa tem discriminador explícito e dia civil; pergunta datada usa o dia do fato/turno. Pendentes ignoram a data civil contextual e derivam pela batida. O problema residual é a origem rederivável no replay, não uma conversão feita por `DiaDoTurno`. (`models.py:2801`, `:2811`, `:2875`; `app/chamados/juizes.py:2188`; `disputa_emissao.py:275`, `:284`; `app/ponto/turnos.py:2943`.)

   O índice novo compara igualdade; não introduz intervalo inclusivo/exclusivo. Nos atos legados, o grupo por origem ainda classifica pela data civil antes de resolver o dia do turno. Equivalência dessas decisões para todo turno noturno: **[nao verificado]**. (`disputa_emissao.py:1001`, `:1411`, `:1412`, `:1417`.)

   Os receivers de fechamento leem o valor de `fechada_em`, sem conferir transição ou `update_fields`; salvar uma fechada não é automaticamente um no-op. O novo agregador evita esse save. (`app/chamados/signals.py:28`, `:129`, `:153`; `disputa_emissao.py:567`.)

7. **Classes de reprovação e mordida dos selos.**

   | Classe | Estado no delta/atos |
   |---|---|
   | Segundo juiz duplicando autoridade registrada | **Nenhum novo identificado no delta** para fechamento, regularizabilidade ou costura: continuam delegados. `disputa_emissao.py:966`, `:1524`; `models.py:2972`; `fio.py:907`. |
   | Segundo escritor fora da porta | **Agregação da âncora curada; nenhum novo escritor persistente de destino encontrado fora dos dois registrados/FK.** Isso não cura os pedidos divergentes que chegam a eles. `disputa_emissao.py:1613`; `fio.py:807`, `:814`; `models.py:2982`. |
   | Relógio fora da borda | **Nenhuma leitura nova identificada em c15–c20; presente nos atos legados:** contexto, seleção de passado e SLA. `disputa_emissao.py:554`, `:914`, `:679`, `:1331`. |
   | Derivação paralela de fonte única | **Presente na c20:** índice e B repetem motivo/dia, com filtros e capturas diferentes. `disputa_emissao.py:1250`, `:1256`, `:1259`, `:1359`, `:1361`. |
   | Selo/RED sem mordida | **Curado nos dois mutantes antigos e ausente nos quatro c20 exe

[truncado em 20000 de 22665 caracteres]

## 11/11 01:06 · sessao bd452f9f99a5 · FIO-E0-V4-E3 · leitura · origem=Code

**V=3, T=229, X=0; soma=232.** As causas se distribuem em **228 d, 2 c e 2 e**. (`sonda_232.out:5–236`; classificação fundamentada abaixo.)

- **V:** existe ação exigível pela lei — decisão do admin ou exposição de pergunta viva — que a tarefa omite. (`app/docs/RELATO.md:2336`; `app/docs/CORTES.md:316`)
- **T:** nenhum marco ou decisão residual demonstrado exige ação; o vigia chama de morto um dia sem tarefa exigível. (`app/docs/LEIS.md:95`; `app/chamados/juizes.py:426–441`; `app/docs/CORTES.md:406`)
- **X:** caso que não cabe nesses critérios; **nenhum neste recorte**. (`sonda_232.out:5–236`, aplicando as regras acima.)

As referências `sonda_232.out:N` numeram **o anexo colado**, contando `99 objects...` como linha 1, a linha vazia como 2 e o primeiro `DIA` como 5. Não representam leitura de um arquivo local.

A tabela principal é esta; **todas as datas são de 2026**. (`sonda_232.out:5–236`)

| Grupo | Classe / quantidade | Colaborador/dia | Fundamentação |
|---|---:|---|---|
| Respondida viva, sem validação, âncora terminal | **V / 2** | col252/09-29; col841/10-10 | Há decisão residual do admin. A causa **c** vem depois de excluir resposta vazia; a tarefa para em `dia_sem_trabalho` antes de examinar a pergunta. (`sonda_232.out:154,224`; `app/chamados/services/vigia_fio.py:673–676`; `app/chamados/services/tarefa_do_dia.py:241–243`; `app/docs/RELATO.md:2336`) |
| Pergunta grave viva em disputa fechada | **V / 1** | col237/11-07 | A lei exige apresentação ao admin pela vida da pergunta, independentemente do fechamento da disputa. O motivo grave fica fora do mapa de marcos da tarefa. (`sonda_232.out:31`; `app/docs/CORTES.md:316`; `app/chamados/services/tarefa_do_dia.py:225–226`; `app/docs/RELATO.md:1736`) |
| Sem instâncias e sem pergunta viva | **T / 201** | Lista completa na tabela seguinte | Pelo fluxo executado: dia sem trabalho, nenhuma instância elegível, nenhuma ação residual demonstrada. (`sonda_232.out:5–236`, dias discriminados abaixo; `app/chamados/services/tarefa_do_dia.py:199–210,235–243`) |
| Instâncias presentes, todas encerradas por `dia_sem_trabalho` | **T / 16** | col97/11-09; col177/11-08; col292/10-22,10-28; col348/10-24,10-31,11-07,11-08; col354/10-30; col741/11-01,11-05,11-07,11-09; col866/11-06,11-08; col889/10-25 | Não há pergunta viva nesses dias; as instâncias não geram cobrança. (`sonda_232.out:9,20,42–43,54–57,59,86–92`; `app/chamados/juizes.py:426–441`) |
| Instâncias presentes, todas `aceso`, sem pergunta viva | **T / 11** | col354/10-21; col655/09-30,10-21; col922/11-01; col996/10-26; col205/09-21; col258/09-28,10-19; col259/10-14,10-15; col368/09-21 | Os marcos estão cumpridos; não há resposta viva residual demonstrada. (`sonda_232.out:58,85,93,113,138,155–158,189,223`; `app/docs/LEIS.md:95`; `app/docs/CORTES.md:406`) |
| Pergunta não respondida viva, mas todos os marcos acesos | **T / 1** | col962/10-25 | A pergunta `intervalo_saida` não cria falta de um marco já cumprido. Aplica-se a regra específica dos marcos acesos; não existe aqui resposta pendente da exceção acima. (`sonda_232.out:102`; `app/docs/CORTES.md:406`; `app/chamados/juizes.py:1364–1375`) |

Para expandir os **201 T sem instâncias**, use os conjuntos abaixo; cada colaborador indicado corresponde a **todas** as datas da respectiva célula. (`sonda_232.out`, referências por linha na tabela.)

- **A:** 10-21, 10-23, 10-25, 10-27, 10-29, 10-31, 11-02, 11-04, 11-06, 11-08. (`sonda_232.out:10–19`)
- **B:** 09-21, 09-23, 09-25, 09-27, 09-29, 10-01, 10-03, 10-05, 10-07, 10-09, 10-11, 10-13, 10-15, 10-17, 10-19. (`sonda_232.out:123–137`)

| Colaborador(es) | Datas | Quantidade | Fonte |
|---|---|---:|---|
| col144, col209, col268, col312, col427, col969 | A | 60 | `sonda_232.out:10–19,21–30,32–41,44–53,60–69,103–112` |
| col144, col209, col268, col312 | B | 60 | `sonda_232.out:123–137,139–153,159–188` |
| col427 | B, exceto 09-21 | 14 | `sonda_232.out:190–203` |
| col650 | 10-22,10-24,10-26,10-28,10-30,11-01,11-03,11-05,11-07,11-09 | 10 | `sonda_232.out:75–84` |
| col650 | 09-22,09-24,09-26,09-28,09-30,10-02,10-04,10-06,10-08,10-10,10-12,10-14,10-16,10-18,10-20 | 15 | `sonda_232.out:208–222` |
| col969 | 09-26,09-27,10-03,10-04,10-10,10-11,10-12,10-17,10-19 | 9 | `sonda_232.out:228–236` |
| col97 | 11-01,11-03,11-05,11-07 | 4 | `sonda_232.out:5–8` |
| col574 | 10-24,10-31,11-07 | 3 | `sonda_232.out:70–72` |
| col574 | 09-26,10-03,10-10,10-17 | 4 | `sonda_232.out:204–207` |
| col639 | 11-06,11-08 | 2 | `sonda_232.out:73–74` |
| col950 | 10-24,10-28,10-29,10-31,11-01,11-02,11-07,11-08 | 8 | `sonda_232.out:94–101` |
| col950 | 10-12,10-17,10-18 | 3 | `sonda_232.out:225–227` |
| col1005 | 11-02 | 1 | `sonda_232.out:114` |
| col1009, col1018 | 11-08 | 2 | `sonda_232.out:115–116` |
| col1019 | 11-04,11-06,11-08 | 3 | `sonda_232.out:117–119` |
| col1027 | 11-05,11-07,11-09 | 3 | `sonda_232.out:120–122` |
| **Total** | | **201** | Soma das linhas acima |

**1. Entrada no universo e causas do vigia.** Todos os 232 percorrem o mesmo caminho: empresas visíveis → competências de `competencias_a_lavrar` → `dias_que_seguram` → inserção de `(colaborador, data)` em `dias`. `dias_que_seguram` seleciona colaboradores classificados fora por `furo_espelho` e consulta os dias retidos por `motivos_retencao_celula`. O fato exigido para entrar é **retenção da folha**, não pergunta viva ou marco faltante. (`app/chamados/services/vigia_fio.py:581–587`; `app/folha/fora_da_folha.py:118–137`; `app/folha/fora_da_folha.py:95–99`)

Como todos têm célula, há dois ramos possíveis de retenção: **veredito acusador**, sem passar pelas dispensas de retenção; ou **veredito nulo**, `trabalha` preenchido e dia passado fora da borda de demissão. Qual desses dois introduziu **cada dia** é **[nao verificado]**: a sonda não imprime veredito, protestos nem `trabalha`. (`sonda_232.out:5–236`; `app/folha/export.py:1233–1256`)

Depois, o vigia consulta a porta; separa hoje/futuro; consulta os quatro baldes de responsabilidade já atribuída; somente então incrementa `fio_morto_alcance`. (`app/chamados/services/vigia_fio.py:635–660,695–699`)

| Causa | Condição, na ordem efetiva | Neste recorte |
|---|---|---:|
| **a** | Chamado vivo da célula ou âncora viva fora da fila. (`app/chamados/services/vigia_fio.py:663–670`) | 0 |
| **b** | Pergunta viva respondida sem texto. (`app/chamados/services/vigia_fio.py:673–674`) | 0 |
| **c** | Pergunta viva respondida, âncora terminal; resposta vazia já foi excluída. (`app/chamados/services/vigia_fio.py:673–676`) | 2 |
| **d** | Sem pergunta viva; dia fora da lista CxR; colaborador presente nela. Célula existente e ausência de chamado vivo decorrem dos testes anteriores. (`app/chamados/services/vigia_fio.py:667–678`) | 228 |
| **e** | Restante, com detalhe nomeado. Aqui: pergunta viva sob chamado terminal. (`app/chamados/services/vigia_fio.py:679–694`) | 2 |

As quantidades dessa tabela vêm das linhas do anexo. (`sonda_232.out:5–236`)

**A causa d não afirma que o dia exige ação, nem atribui dono CADASTRO.** Afirma presença do **colaborador** na CxR e ausência daquele **dia** na lista. A lista vem de `ListaDoAdmin → cadastro_realidade.lista → dias_na_lista`; os colaboradores são extraídos separadamente das linhas por posto. Somente assinaturas que nomeiam uma `data` põem o dia na lista. (`app/chamados/services/vigia_fio.py:205–218,230–251,677–678`; `app/escala/services/cadastro_realidade.py:1044–1064`)

**2. Caminho da tarefa.** A premissa `instancias=[]` vale para **202**, não para todos os 232: são os 201 T da tabela mais col237; os outros 30 têm instâncias com ações vazias. Todos têm `fio='nada'`, fila vazia, `nao_sei=False` e razão geral vazia. (`sonda_232.out:5–236`)

Nos 202 vazios, o fluxo permite inferir `sem_trabalho=True`: caso contrário, ausência de instâncias produziria `nao_venceu` ou `ata_sem_lampadas`. As instâncias são extraídas das células da grade, exigindo tipo E/S e hora; ausência de pergunta não é o critério que decide sua existência. (`app/chamados/services/tarefa_do_dia.py:62–67,84–103,199–210,390–403`)

Nos outros 30, a tarefa encerra cada instância em `dia_sem_trabalho` (**18**) ou `aceso` (**12**), antes de examinar pergunta e decisão. Sem ações, `_portas_da_tarefa` produz `fio='nada'`, fila e verbos vazios; fila residual não cria ação. (`sonda_232.out:9,20,42–43,54–59,85–93,102,113,138,154–158,189,223–224`; `app/chamados/services/tarefa_do_dia.py:241–246,268–283,418–434`)

O fato consultado para dispensar cobrança é `dia_sem_trabalho`, alimentado pela célula/ata; para cumprir marco é a lâmpada lavrada. `marcos_pendentes=[]` apenas informa ausência de marca `pendente` na ata: sozinho não demonstra ausência de obrigação. (`app/ponto/supra_juiz.py:65–71`; `app/chamados/juizes.py:426–441`; `app/ponto/services/lastro.py:138–150`)

**3. Por que os três V e o T de col962 diferem.** Em col252 e col841, a pergunta respondida continua viva. A regra preserva a decisão do admin quando a resposta não foi absolvida pela concordância com a luz e pelo dia coerente; `dia_sem_trabalho` não extingue automaticamente essa decisão residual. A tarefa a ignora porque termina antes da pergunta. (`sonda_232.out:154,224`; `app/docs/RELATO.md:2336`; `app/chamados/juizes.py:1341–1353`; `app/chamados/services/tarefa_do_dia.py:241–243`)

Em col237, disputa fechada e âncora resolvida não substituem resolução da pergunta viva. A lei manda expô-la ao admin; a tarefa restringe-se aos motivos de marco e não alcança `batida_fora_escala_grave`. O conteúdo da decisão que o admin tomará é **[nao verificado]**. (`sonda_232.out:31`; `app/docs/CORTES.md:316`; `app/chamados/services/tarefa_do_dia.py:225–226`; `app/chamados/services/declaracao_texto.py:42–47`)

Em col962, a pergunta não foi respondida e os quatro marcos estão acesos. A regra específica dispensa cobrança com todos os marcos cumpridos. O C6 pode manter vida pelo julgamento geral da célula, pois seu encerramento direto pelo marco aceso exige também `materializada_em`; isso não transforma o marco cumprido em falta. O ramo individual que manteve esse C6 vivo é **[nao verificado]**. (`sonda_232.out:102`; `app/docs/CORTES.md:406`; `app/chamados/juizes.py:1364–1375,1388–1400`)

**4. As quatro hipóteses anteriores não explicam este recorte.** Não há rota só-fila, porta A/B ou verbos A/B observados: todas as portas são nulas e todos os chamados/âncoras impressos estão terminais. Nenhum retorno passou pelo ramo de isenção. Portanto, nenhuma das quatro hipóteses se aplica como causa demonstrada destes 232 resultados. (`sonda_232.out:5–236`; `app/chamados/juizes.py:1578–1585`; `app/chamados/services/tarefa_do_dia.py:183–189`)

A LEI-C17-UM-JUIZ escolhe a tarefa nos quatro casos anteriormente medidos; ela não declara corretas todas as saídas atuais da tarefa. O aval desta fase exige justamente decidir as divergências pela regra. (`app/docs/RELATO.md:244`; `aval_e3_troca_pelo_diff_1111.txt:4–5`)

**5. Contraexemplo por ISENÇÃO: não aparece nestes 232.** Nenhum retorno tem razão geral `isento_ponto`, obrigatória naquele ramo. Os dois casos em que `dia_sem_trabalho` esconde decisão residual são col252 e col841, classificados V; **dia sem trabalho não equivale a isenção**. Existência desse contraexemplo fora do recorte: **[nao verificado]**. (`sonda_232.out:5–236`; `app/chamados/services/tarefa_do_dia.py:183–189,202,241–243`)

**6. Não falta regra escrita para estas classificações.** Há regra para a lâmpada, para resposta residual e para pergunta viva em disputa fechada. Falta **dado**, para todos os 232, para individualizar o ramo de retenção da folha; isso não é pergunta de lei. (`app/docs/LEIS.md:95`; `app/docs/RELATO.md:2336`; `app/docs/CORTES.md:316,406`; `app/folha/export.py:1233–1256`; `sonda_232.out:5–236`)

Como **V não é vazio**, a condição “classe só T” não se verifica: pela ordem do aval, a cura na tarefa precede a troca dos leitores. (`aval_e3_troca_pelo_diff_1111.txt:5`)

## 11/11 01:04 · sessao 6585a794233b · - · - · origem=Ronald (tmux)

**Itens 2 e 3 — conclusões para consolidação.**

1. **Trilha e contador têm o mesmo universo somente depois de a gravação da auditoria ter sido bem-sucedida.** O agregador encaminha dona fechada para `registrar_descarte_em_dona_fechada`; o registrador elimina motivos já presentes, agenda `on_commit`, trava C→D e deduplica por `(dona,motivo,pergunta_id)` antes de chamar o escritor único `core.observ.evento`. Origem não participa da identidade. (`app/chamados/services/disputa_emissao.py:567`; `app/chamados/services/descarte_disputa.py:10`, `:23`, `:27`, `:31`, `:37`; `app/core/observ.py:100`, `:140`.)

   I23 lê **todas as linhas históricas** com ação/modelo correspondentes, sem filtro de estado atual, dia ou janela. Conta linhas, não donas distintas: duas perguntas/motivos da mesma dona podem produzir dois elementos iguais na amostra. O formatador publica `VIOL` se positivo, conserva a descrição integral e integra o comando executado pelo cron. (`app/chamados/management/commands/auditar_invariantes_chamados.py:77`, `:81`, `:89`, `:647`, `:893`, `:1925`; `app/config/crons.py:542`.)

   I23 positivo, isoladamente, **não provoca o `SystemExit(2)` deste comando**, que verifica somente F1/F2. Isso prova a diferença entre sinal visual e código de saída; alarme externo que leia a linha I23: **[nao verificado]**. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1942`.)

2. **Há saídas sem uma nova linha de trilha.**

   - Motivo já presente na dona: saída deliberada antes do callback; repetição da mesma identidade: dedup deliberado. Não são novos descartes registrados. (`app/chamados/services/descarte_disputa.py:10`, `:27`.)
   - Replay divergente: savepoint desfeito e nenhum novo encaminhamento confirmado; seus callbacks são cancelados. (`app/chamados/services/disputa_emissao.py:1496`, `:1501`; `app/chamados/services/descarte_disputa.py:35`.)
   - **Falha da própria trilha:** depois de o negócio confirmar, o callback pode falhar na trava/consulta; `engolir` captura. `evento` também captura sua falha de inserção e retorna `None`; o registrador ignora o retorno. Nesse caso, o descarte confirmado fica sem linha, e I23 não consegue contá-lo. O restante observável é ERROR e contador de engolidos do processo. Ocorrência em produção: **[nao verificado]**. (`app/chamados/services/descarte_disputa.py:19`, `:23`, `:31`, `:37`; `app/core/observ.py:79`, `:92`, `:111`, `:139`, `:145`.)
   - **Silêncio por dedup anterior ao agregador:** se o complemento anterior ao filtro encontra dona fechada com pergunta local viva cujo motivo falta em `DS.motivos`, o helper acrescenta o motivo em memória, mas retorna `False` pelo dedup. O complemento termina `criou=False`; o agrupador só agrega/registra quando `_cc=True`, depois exclui o chamado já coberto e pode retornar `None`. Não há incorporação nem trilha desse motivo faltante. Pressupõe esse acervo inconsistente e chamado que não segue `cobra_marcos_da_ata`; exemplo possível: categoria `intervalo_ausente`, subcategoria mapeada `batida_orfa`, módulo diferente de `batida_ausente`. Reprodução ORM: **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:1003`, `:1007`, `:1069`, `:1278`, `:1292`, `:1297`, `:1304`, `:1661`; `app/chamados/models.py:3678`; `app/chamados/juizes.py:1324`, `:1399`.)

3. **A pergunta de lei “dona FECHADA” está encerrada pelo aval fornecido; a implementação preserva a decisão e instrumenta o descarte.** O agregador não salva/reabre a dona fechada. O registro usa snapshot do fechamento encontrado e continua histórico mesmo que outra cascata reabra a dona depois. Persistência dessas cascatas nesta árvore: **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:567`; `app/chamados/services/descarte_disputa.py:9`, `:22`; aval DUAS-LEIS fornecido, sem arquivo local: **[nao verificado]**.)

4. **Seed c19:** começa dos motivos existentes; somente nova/aberta vazia recebe `orfao_14h` ao final, salvo se o próprio motivo aparece em alguma estrangeira do ato. Estrangeira de outro motivo não impede o seed. O seed também pode permanecer como motivo sem pergunta específica quando há outra pergunta local. Isso é o comportamento explicitamente cobrado pelos selos K3, não exclusividade “um motivo = uma pergunta”. (`app/chamados/services/disputa_emissao.py:1312`, `:1467`; `app/chamados/tests/test_bo_disputa_volta5k_memoria.py:54`, `:79`.)

5. **Sem pergunta no agrupador:**

   | Caminho | Resultado sustentado |
   |---|---|
   | HX: `_ata_do_dia` recusa por `sem_celula` com escala vigente | `_absteve_por_ata=True`; conserva aberta e acende luz. (`app/chamados/services/disputa_emissao.py:1094`, `:1383`, `:1525`.) |
   | h2: ata disponível e nenhum fato a perguntar | Zero perguntas permite `pode_fechar`; fecha como `vazia`. (`app/chamados/services/disputa_emissao.py:1524`; `app/chamados/models.py:1848`, `:1903`.) |
   | Só estrangeiras, local nova sem P | Desfaz nascimento local e repete perguntas na dona; retorna `None`. (`app/chamados/services/disputa_emissao.py:1487`, `:1494`, `:1513`.) |
   | Local preexistente sem P | Não recebe a mesma guarda de rollback, que exige `nova`; passa ao auto-fecho/HX. (`app/chamados/services/disputa_emissao.py:1487`, `:1524`.) |
   | **Sem data/base, marcos inválidos ou erro de leitura** | Existem recusas técnicas sem testemunho de ata; podem terminar zero P e fechar, portanto não demonstram as premissas estritas HX ou h2. (`app/chamados/services/disputa_emissao.py:1054`, `:1083`, `:1356`, `:1362`, `:1413`, `:1418`, `:1445`, `:1448`, `:1451`, `:1524`.) |

6. **Contraexemplo concreto fora das premissas das duas leis:** chamado categoria `batida_ausente`, status `aberto`, `contexto_json={}`, sem célula, sem pergunta materializada nem disputa anterior. O juiz real de regularizabilidade permite-o pelo mapa; `cobra_marcos_da_ata=False`; `dia_do_fato=None`; o laço sem origem abandona-o por falta de data. A DS nasce, recebe seed e fecha vazia **sem consultar ata e sem luz**. Isso não prova “COM ATA e nada a perguntar”, exigido no aval h2. (`app/chamados/services/disputa_emissao.py:923`, `:930`, `:949`, `:955`, `:1121`, `:1337`, `:1445`, `:1469`, `:1524`; `app/chamados/models.py:3667`; `app/chamados/juizes.py:87`, `:133`, `:138`.)

   Executei o agrupador e os juízes indicados via AST em memória, com mapas reais e infraestrutura double: `regularizavel=True`, `cobra_ata=False`, `dia=None`, DS com `['orfao_14h']`, zero P, fechada, zero consultas de ata/luz. ORM/signals/banco: **[nao verificado]**. A infraestrutura usada explicita doubles de trava, origem e fechamento. (`app/chamados/tests/test_bo_disputa_volta5b_orfaos_memoria.py:77`, `:84`, `:96`.)

7. **Verificação executada:** 5g memória passou 10 testes e K3 passou 2 testes; resultados não certificam Django/PostgreSQL. A trilha double substitui `engolir` por `yield` e `evento` por append infalível, portanto esses verdes não cobrem a falha de auditoria descrita acima. (`app/chamados/tests/test_bo_disputa_volta5g_memoria.py:122`, `:240`; `app/chamados/tests/test_bo_disputa_volta5k_memoria.py:54`, `:98`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:290`, `:297`, `:302`.)

## 11/11 01:03 · sessao edfffd5339dd · - · - · origem=Ronald (tmux)

Minha leitura favorece **V nos dias col252/2026-09-29, col841/2026-10-10 e col237/2026-11-07; T para col962/2026-10-25**. Os demais dias sem pergunta viva, sem ações e sem marco pendente favorecem T; não medi a contagem total do anexo. [nao verificado]

- **col252 e col841: V.** A regra escrita preserva a resposta divergente ou sem hora comparável para decisão do admin; marco aceso sozinho não extingue essa pendência. Isso está explícito em `app/docs/RELATO.md:2336`, implementado em `app/chamados/juizes.py:1341`. A tarefa pula toda análise da pergunta quando `sem_trabalho`, em `app/chamados/services/tarefa_do_dia.py:241`. A sonda demonstra exatamente perguntas respondidas, não validadas, C6 vivas, embora a tarefa diga `dia_sem_trabalho`. [nao verificado — dado do prompt]

- **col237: V.** Pergunta viva em disputa fechada deve aparecer ao admin, sem reabrir e sem push; o contador só deve cair por resolução do admin (`app/docs/CORTES.md:316`; reiterado em `app/docs/RELATO.md:2223`). `batida_fora_escala_grave` é motivo fora do chamado de marco do dia, com porta própria (`app/docs/RELATO.md:1736`). A tarefa limita perguntas aos motivos de marco (`app/chamados/services/tarefa_do_dia.py:225`), explicando a cegueira. A pergunta viva fechada e o retorno vazio constam da sonda. [nao verificado — dado do prompt]

- **col962: T.** A sonda mostra quatro lâmpadas `True`, pergunta `intervalo_saida` não respondida e nenhum marco pendente. [nao verificado — dado do prompt] A regra escrita manda deixar de contar cobrança viva quando todos os marcos estão acesos neste julgamento (`app/docs/CORTES.md:406`). O C6 ainda pode devolver verdadeiro pelo veredito acusador geral (`app/chamados/juizes.py:1372`), pois a precedência do marco preenchido exige `materializada_em` (`app/chamados/juizes.py:1368`). Isso explica o conflito de juízes sem criar falta de marco já cumprido.

**Cuidado com uma falsa equivalência no teste:** `app/chamados/tests/test_e2_tarefa_do_dia.py:814` e `:868` dizem que lâmpada acesa responde antes da pergunta, inclusive respondida. Esse comentário genérico não revoga a regra explícita das respostas divergentes/sem comparação (`app/docs/RELATO.md:2336`). Logo não usaria o teste para classificar col252/841 como T.

**C17 não decide antecipadamente este recorte inteiro:** a decisão documentada abrange os quatro mecanismos nomeados e manda tarefa responder rota/fila/verbos; não diz que toda divergência futura é T (`app/docs/RELATO.md:244`). A própria tarefa declara C6 fora de seus insumos (`app/chamados/services/tarefa_do_dia.py:29`).

**Isenção:** o ramo de isento deixa razão global `isento_ponto` e retorna antes de julgar marcos (`app/chamados/services/tarefa_do_dia.py:183`). As linhas fornecidas têm razão global vazia; portanto não sustentam erro pelo ramo de isenção. [nao verificado — dado do prompt] `dia_sem_trabalho` não significa isenção: é outro predicado e outro ramo (`app/chamados/services/tarefa_do_dia.py:202`; `:241`).

Não encontrei necessidade de nova lei para esses quatro dias: há regras escritas para resposta residual, disputa fechada e marco cumprido (`app/docs/RELATO.md:2336`; `app/docs/CORTES.md:316`; `:406`). A projeção exata dessas regras sobre os demais dias depende da consolidação do anexo. [nao verificado]

## 11/11 01:03 · sessao 27bfee3f46b7 · - · - · origem=Ronald (tmux)

- `instancias=[]`, `nao_sei=False`, `razao=''` não vêm da isenção: esse ramo sempre grava `isento_ponto`; tampouco vêm de tranca, fora de vínculo, célula ausente ou ata de trabalho sem lâmpadas, que deixam razões próprias. Pelo fluxo, correspondem a `sem_trabalho=True` e nenhuma instância extraída da grade. A grade vem do leitor único, e instâncias exigem lâmpadas com tipo E/S e hora. (`app/chamados/services/tarefa_do_dia.py:62`, `:84`, `:94`, `:180`, `:183`, `:190`, `:199`, `:202`, `:235`, `:375`.)

- Com instâncias presentes, `dia_sem_trabalho` encerra cada linha antes da pergunta; `aceso` encerra antes de examinar decisões. Por isso há linhas existentes com ações vazias, além dos dias com lista de instâncias vazia. (`app/chamados/services/tarefa_do_dia.py:235`, `:241`, `:244`.)

- Sem ações, a etapa final produz `fio='nada'`, nenhuma porta, fila vazia e verbos vazios. Fila residual não cria ação. (`app/chamados/services/tarefa_do_dia.py:418`, `:425`, `:426`, `:429`, `:430`, `:431`, `:433`.)

- A tarefa carrega apenas perguntas `conversa=False`; depois aceita somente os quatro motivos do mapa de marcos. `batida_fora_escala_grave` está fora desse mapa. Decisões só são consultadas depois das guardas de dia sem trabalho, lâmpada, vencimento e cobertura. (`app/chamados/services/tarefa_do_dia.py:225`, `:241`, `:244`, `:258`, `:281`, `:323`; `app/chamados/services/declaracao_texto.py:42`.)

- Para `col252/2026-09-29` e `col841/2026-10-10`, a causa `c` comprova pergunta C6 viva, respondida e sob âncora terminal; resposta vazia teria sido causa `b` antes. A regra escrita preserva a decisão do admin quando resposta e batida discordam ou o dia não está coerente. São candidatos sólidos a **V**: a tarefa termina em `dia_sem_trabalho`, sem chegar à resposta pendente de decisão. (`app/chamados/services/vigia_fio.py:673`, `:675`; `app/chamados/juizes.py:1341`, `:1349`, `:1350`; `app/docs/PROMPTS.md:1442`.)

- Para `col962/2026-10-25`, pergunta viva sem resposta e marco aceso não demonstram falta daquele marco: a regra BUG117 diz “fato vence pergunta”. O C6 só aplica esse encerramento direto quando há `materializada_em`, enquanto a tarefa consulta a lâmpada antes. A classificação **T** por marco aceso tem suporte; eventual exigência diferente do marco precisa ser demonstrada pelo dado. (`app/chamados/juizes.py:868`, `:880`, `:1364`, `:1368`; `app/chamados/services/tarefa_do_dia.py:244`.)

- Para `col237/2026-11-07`, a tarefa omite o motivo grave por desenho do seu mapa. Há regra escrita exigindo porta para perguntas que continuem vivas pelo C6, incluindo “todo motivo do dia sem porta”; disputa fechada não resolve por si a pergunta. A natureza exata da ação exigível desse motivo não aparece no recorte: [nao verificado]. (`app/chamados/services/tarefa_do_dia.py:225`; `app/chamados/services/declaracao_texto.py:42`; `app/docs/PROMPTS.md:1652`; `app/docs/CORTES.md:316`.)

- Nenhum resultado fornecido tem `razao='isento_ponto'`; portanto nenhum desses resultados percorreu a guarda de isenção. Não há contraexemplo comprovado de erro **por isenção** neste recorte. (`app/chamados/services/tarefa_do_dia.py:183`, `:184`, `:188`, `:189`.)

- LEI-C17-UM-JUIZ decide a favor da tarefa nos quatro campos divergentes anteriores e exige DIFF antes da troca; ela não declara que toda saída atual da tarefa já satisfaz todas as regras. O contrato do juiz é responder ações ainda exigíveis após decisões e efeitos. (`app/docs/RELATO.md:244`; `app/docs/PROMPTS.md:2007`; `app/docs/CORTES.md:426`.)

## 11/11 01:02 · sessao d87f9eac0e8c · - · - · origem=Ronald (tmux)

O caminho do vigia é único para todos os dias: `censo_alcance → dias_que_seguram → classificar_export + motivos_retencao_celula(com_dias=True) → dias.setdefault((colab, data), competência)`. O vigia exige retenção da folha, e não existência de pergunta ou chamado vivo. (`app/chamados/services/vigia_fio.py:581-587`; `app/folha/fora_da_folha.py:118-137`; `app/folha/fora_da_folha.py:95-99`)

Com célula existente, há **dois ramos possíveis de entrada**: veredito acusador que não passa pelas isenções de retenção; ou célula sem veredito, `trabalha is not None`, passada e fora do dia de demissão. A sonda não expõe veredito nem `trabalha`, portanto o ramo individual entre esses dois permanece [nao verificado]. (`app/folha/export.py:1233-1256`)

Antes de contar morto, o vigia procura porta; separa hoje/futuro; e procura decisão pendente, ausência aberta, folga contestada validada ou dia na mesa do admin. Só depois escolhe causa e incrementa `fio_morto_alcance`. (`app/chamados/services/vigia_fio.py:635-660`; `app/chamados/services/vigia_fio.py:304-315`; `app/chamados/services/vigia_fio.py:695-699`)

| Causa | Condição efetiva e referência |
|---|---|
| a | Chamado vivo da célula ou âncora viva fora da fila; como a porta já falhou, é vivo sem porta alcançável. (`app/chamados/services/vigia_fio.py:663-670`) |
| b | Pergunta viva respondida sem texto. (`app/chamados/services/vigia_fio.py:671-674`) |
| c | Pergunta viva respondida, com âncora terminal; vem depois de excluir resposta vazia. (`app/chamados/services/vigia_fio.py:673-676`) |
| d | Nenhuma pergunta viva, dia ausente da lista CxR, colaborador presente nela; existência de célula e ausência de chamado vivo decorrem dos ramos anteriores. (`app/chamados/services/vigia_fio.py:667-678`) |
| e | Restante, discriminado por detalhe: sem célula, dia na lista com outro dono, sem pergunta viva, pergunta sob terminal, sem âncora, fora do app ou outra. (`app/chamados/services/vigia_fio.py:667-694`) |

A causa **d não atribui dono CADASTRO nem prova que nada é exigível**: seu rótulo efetivo é `colab_na_lista_cxr_dia_fora`, e o teste só combina presença do colaborador com ausência do dia. (`app/chamados/services/vigia_fio.py:538-539`; `app/chamados/services/vigia_fio.py:677-678`)

A “lista” vem de `ListaDoAdmin`, que chama `escala.services.cadastro_realidade.lista`, deriva os dias com `dias_na_lista` e separadamente extrai colaboradores das linhas por posto. Só assinaturas com `data` representam dias; assinatura do vínculo não prova presença daquele dia. (`app/chamados/services/vigia_fio.py:205-218`; `app/chamados/services/vigia_fio.py:230-251`; `app/escala/services/cadastro_realidade.py:1044-1064`)

Para a classificação, os dois dias de causa **c** merecem exame próprio: a lei C6 mantém resposta com texto viva para decisão do admin, salvo decisão explícita dele sobre o dia ou concordância com a luz nos termos do juiz; esse ramo precede a absolvição pelo veredito. Portanto `dia_sem_trabalho` sozinho não permite classificá-los T. (`app/chamados/juizes.py:1341-1353`; `app/chamados/juizes.py:1372-1387`)

A tarefa, entretanto, pula cada instância por `dia_sem_trabalho` ou `lampada is True` **antes** de procurar a pergunta e sua decisão. Isso explica como pode devolver nada apesar de C6 viva, sem demonstrar por si só qual autoridade está correta naquele caso. (`app/chamados/services/tarefa_do_dia.py:241-246`; `app/chamados/services/tarefa_do_dia.py:268-283`)

`marcos_pendentes=[]` informa apenas ausência de lâmpada marcada `pendente` na ata; não informa ausência de acusação, ausência de trabalho ou ausência de obrigação. (`app/ponto/services/lastro.py:138-150`)

_3588 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
