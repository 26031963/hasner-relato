# CODEX-ESTADO

> Ultima resposta processada: **15/11/2026 02:16** · publicado em **15/11/2026 02:15** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 15/11 02:16 · sessao 5ce9eaee8b60 · - · - · origem=Ronald (tmux)

**Pauta 1 — `test_MORDE_o_universo_le_a_celula_e_o_FIO`**

- A fixture cria D1 `discordante`, `trabalha=True`, DNA de 07h–19h, mas não fornece `ata`; o campo aceita `NULL` (`app/ponto/tests/test_o320_pauta_dos_discordantes.py:39`, `app/escala/models.py:1778`). O leitor transforma ata ausente em `{}`, mantém tipo `trabalho` e obtém zero lâmpadas (`app/escala/services/leitor_celula.py:379`, `app/escala/services/leitor_celula.py:397`, `app/escala/services/leitor_celula.py:413`).
- Sem instâncias de lâmpadas, a tarefa escreve `nao_sei=True`, razão `ata_sem_lampadas`; essa ignorância não nasceu no diff fornecido, mas o diff passa a publicá-la mesmo sem rota (`app/chamados/services/tarefa_do_dia.py:232`, `app/chamados/services/tarefa_do_dia.py:238`, `app/chamados/services/tarefa_do_dia.py:700`). A Folha lê esse envelope como fio `nao_sei`; a pauta exclui esse fio, resultando em `[]` na asserção que exige `[D1]` (`app/folha/fora_da_folha.py:87`, `app/ponto/services/pautas_dos_discordantes.py:90`, `app/ponto/tests/test_o320_pauta_dos_discordantes.py:68`). **O valor final interno da tarefa após a consulta real ao Resolver dia não aparece no traceback; atribuir toda a execução exclusivamente a esse caminho é [não verificado].**
- O corte O320 manda enviar os dias discordantes **sem fio** à pauta de DP; o próprio teste explica que SEM FIO é `fio == 'nada'`, e não ausência de qualquer pergunta/chamado (`app/ponto/services/pautas_dos_discordantes.py:4`, `app/ponto/tests/test_o320_pauta_dos_discordantes.py:96`). A regra nova separa desconhecimento de ausência conhecida de porta e impede pauta de DP para `nao_sei`, interpretado como ESTRUTURA (`app/chamados/services/tarefa_do_dia.py:697`, `app/ponto/services/pautas_dos_discordantes.py:60`).
- Portanto, o nome continua medindo duas autoridades, mas a fixture não estabelece a premissa **SEM FIO conhecido**: estabelece trabalho sem ata de lâmpadas (`app/ponto/tests/test_o320_pauta_dos_discordantes.py:39`, `app/chamados/services/tarefa_do_dia.py:238`). Cura proposta: **teste/fixture**, lavrando um caso conhecido sem porta e controlando a oferta de Resolver; se a intenção for desconhecimento, exigir exclusão e testemunho em `sem_leitura` (`app/ponto/services/pautas_dos_discordantes.py:90`).
- Volta identificável: publicação do envelope em **c5**, documentada na entrega; ramo explícito da pauta em **c6 K3** (`app/chamados/tests/e3_r3_c5_entrega.txt:54`, `app/chamados/tests/e3_r3_c6_entrega.txt:20`).

**Pauta 2 — `test_MORDE_contagem_IMPAR_e_BATIDA_e_o_motor_NAO_roda`**

- A fixture cria uma célula discordante e uma única batida; depois restaura **somente** `veredito='discordante'` por `update()` (`app/ponto/tests/test_o320_pauta_dos_discordantes.py:182`, `app/ponto/tests/test_o320_pauta_dos_discordantes.py:202`, `app/ponto/tests/test_o320_pauta_dos_discordantes.py:206`). O save da batida chama o cartório e pode alterar ata/status antes desse update (`app/ponto/signals.py:132`, `app/ponto/signals.py:85`).
- A fixture deliberadamente não cria vínculo real e simula cadastro apenas por mock de `cadastro_do_dia_carregado`; esse mock não fornece vínculo à carga da tarefa (`app/ponto/tests/test_o320_pauta_dos_discordantes.py:212`, `app/ponto/tests/test_o320_pauta_dos_discordantes.py:216`, `app/ponto/turnos.py:3053`). Sem vínculo, o teto dos marcos retorna `[]`; uma lâmpada apagada fica sem teto e a tarefa marca desconhecimento (`app/ponto/turnos.py:3065`, `app/ponto/services/cartorio.py:890`, `app/chamados/services/tarefa_do_dia.py:123`, `app/chamados/services/tarefa_do_dia.py:300`).
- Outra mudança do diff permite que a oferta real de Resolver dia publique `admin/porta_acao`; a fixture não controla essa oferta (`app/chamados/services/tarefa_do_dia.py:597`, `app/chamados/services/tarefa_do_dia.py:605`, `app/chamados/services/tarefa_do_dia.py:674`, `app/ponto/tests/test_o320_pauta_dos_discordantes.py:217`). **O traceback mostra somente `[]`; não mostra se D1 saiu por `nao_sei`, por `admin` ou por outro fio: origem interna exata [não verificado].**
- A cadeia comprovável termina antes do juiz da paridade: `apurar()` primeiro chama `dias_sem_fio()`, que só inclui fio `nada`; com lista vazia, retorna `[]` imediatamente. A montagem de `InsumosDoDono`, o pré-teste do motor e `_ins.dono()` ficam depois desse retorno (`app/ponto/services/pautas_dos_discordantes.py:87`, `app/ponto/services/pautas_dos_discordantes.py:95`, `app/ponto/management/commands/pautas_discordantes.py:71`, `app/ponto/management/commands/pautas_discordantes.py:85`, `app/ponto/management/commands/pautas_discordantes.py:100`, `app/ponto/management/commands/pautas_discordantes.py:112`).
- A regra BUG-144 continua escrita e implementada no juiz; a falha fornecida não demonstra violação dela, porque a asserção recebe um universo vazio antes dessa pergunta (`app/ponto/management/commands/e6_oraculo.py:62`, `app/ponto/tests/test_o320_pauta_dos_discordantes.py:229`, `app/ponto/management/commands/pautas_discordantes.py:72`). A regra nova LU-03 considera ação direta administrativa com tela como porta; K3 exclui desconhecimento do universo de pauta (`app/docs/CORTES.md:435`, `app/ponto/services/pautas_dos_discordantes.py:60`).
- Cura proposta: **teste/fixture**, separar a montagem da paridade do pré-filtro de alcance — ou fornecer explicitamente um dia conhecido sem fio. A identificação do ramo concreto que excluiu D1 permanece **[não verificado]**, portanto não agrupar automaticamente esta falha com “ata vazia” (`app/ponto/tests/test_o320_pauta_dos_discordantes.py:208`, `app/ponto/management/commands/pautas_discordantes.py:71`).

**Agrupamento:** a primeira integra a causa “ausência de lâmpadas antes mascarada como ausência de porta”; a segunda integra “fixture do teste de paridade não fixa mais o universo sem fio”. Ambas indicam cura de fixture, preservando testes próprios para `nao_sei` e para porta administrativa; não há, nestes dois tracebacks, prova de regressão no juiz BUG-144 (`app/chamados/services/tarefa_do_dia.py:238`, `app/chamados/services/tarefa_do_dia.py:700`, `app/ponto/management/commands/pautas_discordantes.py:71`, `app/ponto/management/commands/e6_oraculo.py:62`).

## 15/11 02:15 · sessao 8be6effeb4cc · - · - · origem=Ronald (tmux)

Os três C11 têm a **mesma falha de preparação**: `_beco()` exige `(False, None, None)` na linha 385; nenhuma das três execuções chega às asserções sobre lavra ou motor (`app/ponto/tests/test_cxr_tela_le_o_motor.py:385`, `:409`, `:422`, `:429`).

**Cadeia comum escritor → leitor → asserção**

- A fixture cria chamado `resolvido`, pergunta com `resposta_colab='17:00'` e `respondida_em` anterior ao instante da consulta; depois registra a conservação dessa resposta na E1 (`app/ponto/tests/test_cxr_tela_le_o_motor.py:359`, `:366`, `:372`).
- `linhas_da_projecao_sem_linha` produz ato `RESPOSTA` com o texto da projeção; `registrar` grava a sequência (`app/chamados/models.py:4074`, `:4086`, `:4096`).
- A carga lê decisões até `agora` e as ordena por ato; `_pela_decisao` transforma a última resposta textual em `aguarda_validacao`, ação `validar/admin` (`app/chamados/services/tarefa_do_dia.py:419`, `:421`, `:157`, `:160`).
- A âncora sai de `chamado_que_lista`: prevalece o `chamado_destino_id`; a ação recebe `porta=None` quando essa âncora está fora da fila (`app/chamados/services/fio.py:117`; `app/chamados/services/tarefa_do_dia.py:150`, `:151`).
- **Trecho causal do diff:** `admin_alcancavel` agora aceita `VALIDAR` com âncora existente mesmo fora da fila; escolhe `fio='admin'`, publica `rota='porta_acao'`, `acao='validar'`, e `porta_da_tarefa` devolve o envelope (`app/chamados/services/tarefa_do_dia.py:646`, `:648`, `:659`, `:673`, `:684`, `:700`).
- Isso explica o valor raspado `(False, 'porta_acao', {...fio:'admin', acao:'validar'...})` diante da expectativa de `_beco()` (`app/ponto/tests/test_cxr_tela_le_o_motor.py:385`; dado fornecido no traceback dessa linha).

**Cada teste, separadamente** [nao verificado]

| Teste | O que pretendia provar depois de `_beco()` | O que a falha prova | [nao verificado]
|---|---|---|
| `test_MORDE_C11_le_a_lista_lavrada_e_nao_chama_o_motor` | Dia coberto e presente na lavra → C11 com `CADASTRO`, zero chamadas ao motor e juiz consultado com lista positiva (`app/ponto/tests/test_cxr_tela_le_o_motor.py:408`, `:410`, `:413`, `:415`, `:417`). | Falha antes de produzir a lavra ou consultar a lista; comportamento posterior nesta execução **[nao verificado]** (`app/ponto/tests/test_cxr_tela_le_o_motor.py:409`, `:385`). |
| `test_MORDE_C11_lavra_que_cobre_sem_dia_e_lista_vazia_sem_motor` | Cobertura com conjunto vazio → `ESTRUTURA`, zero chamadas ao motor (`app/ponto/tests/test_cxr_tela_le_o_motor.py:419`, `:420`, `:423`, `:425`, `:426`). | Falha antes da lavra vazia; comportamento posterior nesta execução **[nao verificado]** (`app/ponto/tests/test_cxr_tela_le_o_motor.py:422`, `:385`). |
| `test_MORDE_C11_fora_da_lavra_fica_sem_resposta_e_nao_chama_o_motor` | Sem lavra → dono `None`, zero chamadas ao motor (`app/ponto/tests/test_cxr_tela_le_o_motor.py:428`, `:430`, `:431`, `:432`). | Falha antes da consulta à lista; comportamento posterior nesta execução **[nao verificado]** (`app/ponto/tests/test_cxr_tela_le_o_motor.py:429`, `:385`). |

O contrato de lavra continua escrito no código: `cxr_lavrado` distingue ausência de cobertura (`None`) de cobertura sem dias (`set()`); `InsumosDoDono` com `sem_motor=True` conserva a ausência de resposta e impede fallback para motor (`app/ponto/services/cxr_na_lista.py:165`, `:171`, `:182`; `app/ponto/management/commands/e6_oraculo.py:142`, `:166`, `:189`, `:193`). Nada no dado fornecido demonstra regressão nesse contrato; os tracebacks param antes dele (`app/ponto/tests/test_cxr_tela_le_o_motor.py:385`).

**Regra velha, regra nova e conflito escrito**

1. A fixture segue a leitura antiga de porta: pergunta respondida sob âncora terminal fica sem porta; ainda verifica o C17 antigo como `None` (`app/ponto/tests/test_cxr_tela_le_o_motor.py:364`, `:376`). O C17 continua documentado como “chamado da célula em `qs_na_fila` OU pergunta viva no app com âncora na fila” (`app/chamados/juizes.py:1667`).
2. O comentário da fixture ainda diz que resposta com linha espera **SISTEMA**; o corpo atual diz que última `RESPOSTA` textual espera **ADMIN validar**. Essa discordância é direta, independentemente da discussão sobre alcance (`app/ponto/tests/test_cxr_tela_le_o_motor.py:382`; `app/chamados/services/tarefa_do_dia.py:157`, `:160`).
3. A implementação nova segue LU-03: “ação direta do admin com tela = porta”; seu docstring interpreta âncora existente, inclusive fora da fila, como tela de validar (`app/docs/CORTES.md:435`; `app/chamados/services/tarefa_do_dia.py:622`). O teste M2 exige exatamente ação `validar` com `porta=None`, mapa de âncora e resultado `admin/porta_acao` (`app/chamados/tests/test_e3_degrau_dono_e_porta.py:540`).
4. Há texto contemporâneo contrário: LU-04 diz que pergunta em marco dispensado, fora de marco **ou porta None** não dá dono (`app/docs/CORTES.md:436`). Também permanece a regra escrita “âncora terminal é fio morto — o admin não a vê” (`app/chamados/services/fio.py:785`, `:789`). Não achei nesses sítios declaração que revogue essas restrições para a interpretação nova de LU-03 **[nao verificado]**.
5. O novo teste “caminho terminal de antes segue” não resolve essa composição: força tarefa `fio='nada'` e `porta_da_tarefa=lambda t:None`; testa o beco depois de supor ausência de porta (`app/chamados/tests/test_e3_r3_c6_sem_banco.py:283`, `:285`, `:323`).

**A fixture ainda monta o caso do nome?**

Pelo juiz publicado no hash, **não monta o beco SEM PORTA**: sua resposta textual conhecida e âncora existente passam a produzir `validar/admin/porta_acao`; o beco real rejeita qualquer envelope não `None` (`app/ponto/tests/test_cxr_tela_le_o_motor.py:366`, `:372`, `:385`; `app/chamados/services/tarefa_do_dia.py:648`, `:673`; `app/escala/services/cadastro_realidade.py:472`).

Contudo, existência de âncora **não comprova a tela desta fixture**: ela cria `DisputaSupervisao` sem `chamado`; esse campo admite `NULL`; a montagem do modal busca disputas com `chamado__isnull=False` e carrega perguntas somente das disputas selecionadas (`app/ponto/tests/test_cxr_tela_le_o_motor.py:363`; `app/chamados/models.py:1796`; `app/chamados/services/fio.py:211`, `:214`, `:247`, `:255`). A rota do modal usa essa montagem (`app/chamados/views.py:889`). A tela efetivamente renderizada não foi executada **[nao verificado]**.

**Agrupamento e cura**

Os três pertencem à causa comum **“a fixture antes sem porta ganhou porta administrativa pela interpretação nova de LU-03”** (`app/chamados/services/tarefa_do_dia.py:646`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:385`). Classificação principal: **PERGUNTA**, pois LU-03/comentário da implementação e LU-04/regra de âncora terminal sustentam leituras diferentes (`app/docs/CORTES.md:435`, `:436`; `app/chamados/services/tarefa_do_dia.py:622`; `app/chamados/services/fio.py:789`).

- Se âncora terminal existente garante tela de validar, a cura dos três selos é **na fixture do teste**, preservando a anti-vacuidade do C11 e suas três situações de lavra; trocar apenas a expectativa para aceitar porta faria o caso deixar de percorrer o beco (`app/escala/services/cadastro_realidade.py:472`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:412`).
- Se prevalece “porta None/âncora terminal sem tela não dá dono”, a cura é **no código que publica alcance administrativo**, com prova da composição real, pois `admin_alcancavel` hoje aceita apenas a existência da âncora (`app/docs/CORTES.md:436`; `app/chamados/services/fio.py:789`; `app/chamados/services/tarefa_do_dia.py:648`).
- A volta exata c3/c4/c5/c6 que introduziu esse hunk é **[nao verificado]**; o comentário o vincula a LU-03/LU-07 (`app/chamados/services/tarefa_do_dia.py:620`).

## 15/11 02:14 · sessao 2418af21e5c7 · - · - · origem=Ronald (tmux)

Os três failures da Folha têm uma causa comum: a fixture cria `veredito='furo'` e DNA de marcos, mas não fornece `ata` com lâmpadas; o juiz distingue agora ausência **conhecida** de porta de ausência de leitura. A regra E2 já classifica trabalho com marcos vencidos e nenhuma instância como `nao_sei=True, razao='ata_sem_lampadas'`; o diff passa a publicar esse testemunho e a Folha passa a preservá-lo (`app/folha/tests/test_haiku_fora_da_folha.py:53`, `app/folha/tests/test_haiku_fora_da_folha.py:425`, `app/chamados/services/tarefa_do_dia.py:232`, `app/chamados/services/tarefa_do_dia.py:692`, `app/folha/fora_da_folha.py:84`).

**Cadeia comum, até o primeiro valor alterado.** `CelulaDia.ata` é nullable; o leitor usa `{}` quando falta ata, mantém o tipo trabalho de `trabalha=True`, e deriva as células exclusivamente da ata. Sem lâmpadas, nenhum marco com tipo/hora emerge para `instancias_do_turno`; `_tarefa_pura` responde desconhecimento. O patch de oferta `False` impede Resolver dia, mas não apaga esse desconhecimento (`app/escala/models.py:1778`, `app/escala/services/leitor_celula.py:379`, `app/escala/services/leitor_celula.py:397`, `app/escala/services/leitor_celula.py:413`, `app/escala/services/leitor_celula.py:114`, `app/chamados/services/tarefa_do_dia.py:106`, `app/chamados/services/tarefa_do_dia.py:232`, `app/chamados/services/tarefa_do_dia.py:609`).

O trecho novo determinante é `porta_da_tarefa`: antes o resultado sem rota desaparecia como `None`; agora `nao_sei=True` também publica envelope. `_fio_da_porta` interpreta envelope sem rota como `nao_sei`, e `_grupo` dá E quando não há dia com ação (`app/chamados/services/tarefa_do_dia.py:692`, `app/folha/fora_da_folha.py:84`, `app/folha/fora_da_folha.py:92`).

1. **`HaikuForaDaFolhaTest.test_o_dia_que_acusa_diz_o_veredito_e_o_que_DESTRAVA`.** O patch retorna oferta `False`; a célula tem DNA/veredito, sem ata. `o_que_segura` carrega tarefa, projeta porta, lê fio e grava o resultado em `_d['fio']`; a asserção exige `nada`, embora a cadeia entregue `nao_sei` (`app/folha/tests/test_haiku_fora_da_folha.py:53`, `app/folha/tests/test_haiku_fora_da_folha.py:75`, `app/folha/fora_da_folha.py:257`, `app/folha/fora_da_folha.py:260`, `app/folha/fora_da_folha.py:272`, `app/folha/tests/test_haiku_fora_da_folha.py:83`).

   A regra do teste é explícita: “LU-03/M4: sem pergunta, chamado E oferta, o fio e nada”. A regra da implementação também: “None = ausência conhecida de porta”; desconhecimento publica testemunho, conforme DESENHOS-DA-NOITE 2A. As duas regras não precisam conflitar: o teste deixou de montar uma negativa conhecida, porque suprimir oferta não fornece as lâmpadas que faltam (`app/folha/tests/test_haiku_fora_da_folha.py:83`, `app/chamados/services/tarefa_do_dia.py:697`, `app/chamados/tests/test_e3_r3_c6_sem_banco.py:5`, `app/chamados/services/tarefa_do_dia.py:238`).

2. **`GoldenDonoDoBloqueadoTest.test_GOLDEN_quantos_bloqueados_dependem_SO_do_admin_na_competencia`.** A fixture cobre toda a competência com células e deixa apenas um dia acusador, mas todas continuam sem ata/lâmpadas; seu patch de oferta também é `False`. `fora_da_folha` lê o fio `nao_sei` e conta grupo E; `_esperado_pela_ferramenta` preserva E como `nao_sei`; `_esperado_por_dono` aplica `DONO_DO_FIO`, que manda esse fio para `sem_leitura_sistema`. Por isso `sem_acao_disponivel` dá zero na asserção que pede um (`app/folha/tests/test_haiku_fora_da_folha.py:374`, `app/folha/tests/test_haiku_fora_da_folha.py:425`, `app/folha/fora_da_folha.py:203`, `app/folha/fora_da_folha.py:215`, `app/folha/tests/test_haiku_fora_da_folha.py:483`, `app/folha/tests/test_haiku_fora_da_folha.py:497`, `app/folha/retrato_folha.py:193`, `app/folha/tests/test_haiku_fora_da_folha.py:517`).

   A comparação ferramenta/tela vem **antes** da asserção fixa que falha; o traceback fornecido não demonstra divergência entre esses dois caminhos. O teste já incorporou LU-05, que exige balde próprio para `ninguem`, e já incorporou E na tradução; o erro restante é presumir que sua fixture produz grupo D. A regra 2A exige separar desconhecimento, e a fixture produz justamente esse caso (`app/folha/tests/test_haiku_fora_da_folha.py:511`, `app/folha/tests/test_haiku_fora_da_folha.py:515`, `app/folha/tests/test_haiku_fora_da_folha.py:485`, `app/docs/CORTES.md:437`, `app/chamados/tests/test_e3_r3_c6_sem_banco.py:6`).

3. **`GoldenDonoDoBloqueadoTest.test_MORDE_a_GOLDEN_o_dono_ANDA_quando_o_fio_do_dia_muda`.** A mesma fixture vira E. Na cadeia da tela, `montar_linhas` chama `fora_da_folha`, `_fio_cru` traduz a letra E por `FIO_DA_LETRA`, e grava `dono='nao_sei'`; `_fio_na_tela` conta o fio gravado. A asserção inicial exige um `ninguem`, antes de criar o chamado que exercitaria a mudança (`app/folha/retrato_folha.py:1192`, `app/folha/retrato_folha.py:1228`, `app/folha/retrato_folha.py:378`, `app/folha/retrato_folha.py:179`, `app/folha/tests/test_haiku_fora_da_folha.py:452`, `app/folha/tests/test_haiku_fora_da_folha.py:546`, `app/folha/tests/test_haiku_fora_da_folha.py:551`).

   O nome e docstring descrevem uma transição `nada → chamado → admin`; a fixture atual inicia em `nao_sei`. O teste conserva escrita histórica “vivo na fila TEM porta”, enquanto LU-02 exclui residuais cobrar/histórico/arquivado/None; isso merece revisar o verbo efetivo do chamado depois de reparar o início, mas esse trecho posterior **não foi alcançado** no traceback fornecido (`app/folha/tests/test_haiku_fora_da_folha.py:528`, `app/folha/tests/test_haiku_fora_da_folha.py:536`, `app/chamados/services/tarefa_do_dia.py:655`, `app/folha/tests/test_haiku_fora_da_folha.py:546`). O comportamento posterior desse chamado smoke é [nao verificado].

**Volta e cura.** A entrega c5 declara expressamente a publicação do envelope desconhecido e o grupo E/mapas da Folha. A entrega c6 declara ajuste do grupo misto; isso não explica estes casos de um único dia acusador. Identificar commits exatos sem `.git` é [nao verificado] (`app/chamados/tests/e3_r3_c5_entrega.txt:1`, `app/chamados/tests/e3_r3_c5_entrega.txt:54`, `app/chamados/tests/e3_r3_c5_entrega.txt:56`, `app/chamados/tests/e3_r3_c6_entrega.txt:16`).

**Classificação: cura no teste/fixture**, preservando a prova de ausência conhecida de porta e seu caso irmão com oferta. A fixture precisa fornecer os insumos lavrados suficientes para `nao_sei=False`; trocar simplesmente `nada` por `nao_sei` eliminaria a prova nominal do dia sem ação conhecida. Acrescentar um caso distinto de ata sem lâmpadas preservaria a regra 2A e separaria as duas situações (`app/chamados/services/tarefa_do_dia.py:238`, `app/chamados/services/tarefa_do_dia.py:697`, `app/folha/tests/test_haiku_fora_da_folha.py:83`, `app/folha/tests/test_haiku_fora_da_folha.py:88`, `app/chamados/tests/test_e3_r3_c6_sem_banco.py:6`).

## 15/11 02:14 · sessao 0d23cacef646 · BATIDA-DEDUP-RETRATADA · construtor · origem=Code

C3 entregue em [dedup_retratada_c3_entrega.txt](/home/ronald/wt-codex-batida-dedup-retratada-011759/app/ponto/tests/dedup_retratada_c3_entrega.txt:7), com cura, diff, RED/GREEN, censo e comandos.

Validação local: **31 testes verdes e 40 mutantes mortos**, com controles verdes ([evidências](/home/ronald/wt-codex-batida-dedup-retratada-011759/app/ponto/tests/dedup_retratada_c3_entrega.txt:68)). `py_compile` e Ruff passaram ([checks](/home/ronald/wt-codex-batida-dedup-retratada-011759/app/ponto/tests/dedup_retratada_c3_checks.txt:18), [Ruff](/home/ronald/wt-codex-batida-dedup-retratada-011759/app/ponto/tests/dedup_retratada_c3_checks.txt:43)). [nao verificado]

Suite, smoke real e certificação Code/opus: [nao verificado]. [nao verificado]

## 15/11 02:07 · sessao ab1e79bc6658 · JUSTIFICATIVA-PAGA-O-PERIODO · verificador · origem=Code

**Há bloqueios para liberar esta volta**, principalmente na preservação de exportadas, no contrato `None` entre fechamento e lavratura e na catraca K1. As referências estão na tabela; ocorrência e quantidade desses casos em produção: **[nao verificado]**.

| Arquivo:linha | O que está errado | Caso concreto | Gravidade |
|---|---|---|---|
| `app/ponto/services/fechamento.py:36`, `:539`, `:560`, `:1157`, `:1194` | **Exportada não está genericamente protegida.** A guarda contém somente 09/2026. Nas demais competências, detectar exportação gera aviso e permite continuar até `save/update`. A c11 muda previsto, dias, carimbo e motivos no caminho antes interrompido pelo `sum(None)`. | Competência 10 exportada, com dia justificado `sem_ata`: o recálculo pode persistir o estado novo. A afirmação da entrega “exportada intocada” não é garantida por esse escritor. | **bloqueia** |
| `app/ponto/services/fechamento.py:1073`, `:1225`; `app/ponto/services/dia_pago.py:292`; `app/folha/porta_export.py:455` | **O mesmo desconhecido tem dois significados.** Fechamento conserva abono/realizado `None`; `lavrar` transforma esses mapas em `0.0`. O comparador pula o campo cujo fechamento é `None`, ocultando a perda de informação. A conversão é herdada e continua alcançável pela c11. | Grade com P=480 e W desconhecido: fechamento tem abono/realizado desconhecidos; `DiaPago` tem `0/0`; a comparação não registra divergência nesses campos. | **bloqueia** |
| `app/ponto/services/espelho.py:1264`; `app/templates/ponto/espelho.html:35`, `:270`; `app/ponto/services/dia_pago.py:956` | **A tela não renderiza o rótulo c10.** O resumo recebe `motivo_dias_abono`, mas o template não o consulta. O texto existente depende de `lavrada=False`; qualquer `DiaPago` existente recebe `lavrada=True`. O PDF consulta o motivo em `app/relatorios/pdf_espelho.py:790`. | Dia justificado sem ata, com linha `DiaPago` existente: a tela não mostra o rótulo exigido, embora o resumo esteja marcado. | **bloqueia** |
| `app/ponto/services/espelho.py:1234`, `:1236`, `:1264`; `app/relatorios/pdf_espelho.py:333` | **O rótulo da tela usa a janela visual inteira.** O conjunto `abono_sem_apuracao` não é recortado para a competência, diferentemente dos furos em `espelho.py:1258`. O PDF alimenta seu conjunto pela janela própria. | Justificativa sem ata numa competência anterior pode tornar desconhecido o abono do resumo atual, mesmo com os dias atuais apurados. O fechamento não fornece `dias_abono` para desfazer isso (`app/relatorios/cartao_pela_celula.py:93`). | **bloqueia** |
| `app/ponto/tests/test_catraca_abono_do_motor_c11.py:68` | **K1 tem escape com nome literal estático.** Toda chave de dicionário é descartada como se fosse chave de resumo, sem examinar seu uso. | Novo leitor: `nomes={'dias_abono':'Abonos'}` seguido de `return {k: getattr(resultado,k) for k in nomes}`. Reproduzido: nenhum achado e nenhuma diferença da allowlist. Não exige nome construído em execução. | **bloqueia** |
| `app/ponto/services/fechamento.py:1073`; `app/ponto/tests/test_justificativa_c11_sem_banco.py:80`, `:90` | **Sobrevive um mutante que reabre o defeito K2.** Trocar somente `any` por `all` no abono passa os 102 testes. A fixture desconhecida só produz mapa de abono `{D: None}`, sem mistura com número. | D sem ata + D2 com abono=180: o mutante tenta somar `None` com 180, cai no `except` e apaga previsto/dias/carimbo; motivos permanecem velhos (`fechamento.py:1100`). | **bloqueia** |
| `app/ponto/services/efeito_ausencia.py:75`; `app/ponto/tests/test_justificativa_c10_sem_banco.py:145` | **Outro mutante de uma linha passa os 102 testes:** `sem_apuracao.discard(cd)` → `sem_apuracao.clear()`. A ausência inteira só é testada isoladamente. | D com ausência inteira + VIZ justificado sem ata: o mutante remove também VIZ do conjunto. Na reprodução, PDF passa de desconhecido para **2 dias/16 horas**; tela passa para 2 dias. | **bloqueia** |
| `app/ponto/services/efeito_ausencia.py:75`; `app/folha/export.py:339`; `app/escala/services/leitor_celula.py:479` | **A precedência da ausência inteira diverge entre os caminhos.** O feed remove o desconhecido, mas a grade financeira lê `periodos_justificados` cru e recoloca `sem_ata`. | Ausência inteira + justificativa aprovada + ata ausente: motor recebe dia sem rótulo; grade entrega abono/realizado `None`. O teste c10 verifica somente o feed (`test_justificativa_c10_sem_banco.py:154`). Resíduo herdado. | **fila** |
| `app/relatorios/cartao_pela_celula.py:146`, `:187`, `:208`, `:214` | **A aplicação final dos totais pode desfazer parcialmente o rótulo.** Lavratura fornece horas numéricas enquanto dias permanecem desconhecidos. Além disso, motivo posterior `None` não limpa o motivo anterior. | Resumo marcado pode terminar com `dias_abono=None` e `horas_abono=0`; ou receber dias=1/horas=8 e conservar motivo `'sem_ata'`. O badge PDF exige dias `None`, portanto motivo velho sozinho não prova badge errado (`pdf_espelho.py:790`). | **fila** |
| `app/folha/causas.py:85`; `app/ponto/services/fechamento.py:1093`; `app/ponto/tests/test_justificativa_c11_sem_banco.py:61` | **O caminho liberado pela c11 grava diagnóstico falso de zero.** O classificador usa `None or 0`; o teste substitui esse classificador por resposta constante. | Trabalho previsto=480, realizado desconhecido, sem missing/órfãs: função real devolve `realizado_zero`, com evidência “zero minuto realizado”. Reproduzido em memória. | **fila** |
| `app/ponto/services/fechamento.py:616`, `:1080`; `app/folha/export.py:1248`, `:1317` | **Há leitura de relógio fora do instante da passada.** O escritor já possui `_agora_da_passada`, mas chama retenção sem `hoje`; o leitor consulta `localdate()`. É caminho herdado, agora executado também para `sem_ata`. | Passada atravessando meia-noite pode julgar retenção com outra data. Efeito ocorrido em produção: **[nao verificado]**. | **fila** |

O rastreio dos contratos fica assim:

| Valor | Escritores e leitores conferidos | Contrato / resultado |
|---|---|---|
| Abono, previsto e realizado por dia | Grade recebe os minutos da justificativa em `app/folha/export.py:339`; transporta desconhecido em `app/escala/services/leitor_celula.py:479`; `por_dia_da_grade` produz os mapas em `app/ponto/services/dia_pago.py:387`. Fechamento lê em `fechamento.py:1066`; prontidão em `export.py:1110`; lavratura em `dia_pago.py:288`. | Minutos por data da grade. `sem_ata` produz abono/realizado `None`, preservando previsto (`dia_pago.py:395`). A lavratura rompe esse contrato ao converter para zero (`:292`). |
| Totais de `FechamentoMensal` | Atribuição em `app/ponto/services/fechamento.py:1073`, `:1076`, `:1096`; abstinência no `except :1107`; persistência por `save :1157` ou cópia no `update :1194`. Campos permitem `NULL` em `app/ponto/models.py:432`, `:440`, `:445`. | Com qualquer dia desconhecido, abono/realizado mensal ficam `None`; previsto/dias/carimbo podem permanecer conhecidos. Não grava o previsto como abono. |
| `DiaPago.minutos_abonados` e demais mapas | `lavrar` escreve em `app/ponto/services/dia_pago.py:292`; `retratar_exportada` cria/atualiza em `:490`, `:495` e limpa linhas fora do mapa em `:509`. Campos não são nuláveis (`app/ponto/models.py:349`). | Ambos os caminhos convertem desconhecido em zero. O problema também existe no retrato de exportada; a c11 não altera esse escritor. |
| Abono do resultado do motor | Motor acumula dias/horas em `app/ponto/motor_calculo_v2.py:2630`, `:2631`, `:2633`; composição de fatias declara os campos em `app/ponto/services/fechamento.py:99`. Tela lê em `espelho.py:1261`; PDF em `pdf_espelho.py:479`; instrumento em `ponto/management/commands/diff_espelho_alimentacao.py:28`. | Dias e horas, nunca `None` nesse ramo. Sem ata/sem batida, a ausência da chave permite pagar o previsto por `_abono_inteiro` (`motor_calculo_v2.py:2629`), conforme o aval fornecido. |
| Conjunto e motivo do rótulo | Feed escreve conjunto em `app/ponto/services/efeito_ausencia.py:56`, `:82`; `Autoridade` transporta em `espelho.py:1200`; helper escreve dias/horas `None` e motivo em `efeito_ausencia.py:96`. Tela/PDF chamam em `espelho.py:1264` e `pdf_espelho.py:483`. | O helper não calcula dinheiro. Há problemas posteriores de janela, renderização e sobrescrita descritos acima. |
| Lista de fechamento e comparador | Lista chama prontidão por linha em `app/ponto/services/fechamento_lista.py:85`; prontidão rejeita snapshot sem abono em `app/folha/export.py:1051` e devolve cego/`sem_ata` em `:1112`. Comparador pula total `None` em `app/folha/porta_export.py:455`. | Prontidão preserva desconhecido; comparador deixa de medir o campo. Custo adicional da leitura da grade em produção: **[nao verificado]**. |
| TXT Domínio e extrato | TXT monta eventos em `app/folha/export.py:733`, usando rubricas `horas_*` de `:423`; seleção consulta motivos em `:971`. Extrato lê somas de `DiaPago` em `app/relatorios/views.py:520` e decide ausência de número em `:535`. | Não encontrei conversão direta desses totais `None` para zero no TXT. Entretanto, c11 atualiza os motivos usados na seleção (`fechamento.py:1080`). O extrato não transporta o desconhecimento desses campos de minutos. |

K1 captura atributo, apelido de variável, `getattr` literal e apelido de import; sua allowlist declara contagem, papel e motivo. O escape demonstrado impede considerá-la fechada (`app/ponto/tests/test_catraca_abono_do_motor_c11.py:34`, `:74`, `:103`, `:145`).

Os testes c10 param nas atribuições e na chamada do helper; não exercitam `folha_manda` nem renderização. Os c11 extraem o bloco do escritor e substituem os classificadores vizinhos, portanto o verde não cobre a integração inteira (`app/ponto/tests/test_justificativa_c10_sem_banco.py:11`, `:47`; `app/ponto/tests/test_justificativa_c11_sem_banco.py:25`, `:59`).

Não identifiquei no delta nova fórmula para dia com ata, falta ou DSR: o filtro muda a entrega dos desconhecidos, enquanto cobertura e DSR continuam dependentes de `justificadas`. Isso não comprova os números exigidos de produção nem preservação de exportadas (`app/ponto/services/efeito_ausencia.py:55`, `:58`, `:78`; `app/ponto/motor_calculo_v2.py:2605`, `:2641`). Suite inteira, banco e medidas de produção nesta volta: **[nao verificado]**.

## 15/11 02:05 · sessao 7b78b1e3d767 · - · - · origem=Ronald (tmux)

| Arquivo:linha | Achado | Caso reproduzido em memória | Gravidade |
|---|---|---|---|
| `app/ponto/tests/test_catraca_abono_do_motor_c11.py:67` | K1 exclui toda chave de dicionário supondo que seja resumo, sem verificar o objeto ou seu uso posterior. | Novo módulo com `nomes={'dias_abono':'Abonos'}; return {k:getattr(resultado,k) for k in nomes}`: `achados=[]` e `fora_da_allowlist={}`. Leitor do motor escapa com nome literal estático; não depende dos limites de nomes construídos em runtime. | bloqueia |
| `app/ponto/services/fechamento.py:1073`; `app/ponto/tests/test_justificativa_c11_sem_banco.py:81` | Mutante de uma linha `any(...) → all(...)` sobre `_abono_dia` sobrevive aos 102 testes. A fixture sem ata só possui `_abono_dia={D:None}`, conforme a própria asserção em `:90`, deixando sem cobertura o mapa misto. | D sem ata + D2 com W=300/P=480/abono=180. Original: `(previsto,dias,incertos,abono,realizado,carimbo)=(960,2,0,None,None,AGORA)`. Mutante cai no `except` de `fechamento.py:1100`: os seis campos viram `None`, motivos/causa permanecem `VELHO`. | bloqueia |
| `app/ponto/services/efeito_ausencia.py:75`; `app/ponto/tests/test_justificativa_c10_sem_banco.py:145` | Mutante de uma linha `sem_apuracao.discard(cd) → sem_apuracao.clear()` também sobrevive aos mesmos 102 testes. A ausência inteira só é testada isoladamente em D..D (`:149`). | D com ausência inteira + VIZ justificado sem ata: original conserva `{VIZ}`, PDF/tela saem `None` com `sem_ata`; mutante esvazia o conjunto e retorna PDF `dias_abono=2, horas_abono=16.0`, tela `dias_abono=2`. | bloqueia |

Os sete módulos executados foram `test_justificativa_c11_sem_banco`, `test_catraca_abono_do_motor_c11`, `test_justificativa_c10_sem_banco`, `test_justificativa_c9_sem_banco`, `test_justificativa_c8_sem_banco`, `test_justificativa_soma_sem_banco` e `test_justificativa_periodo_v2_sem_banco`: 102 testes, zero failures/errors para cada sobrevivente. Execução somente em memória com `python3 -B`; catálogos de mutantes não foram executados nesta auditoria [nao verificado quanto ao catálogo]. [nao verificado]

K1 captura as sete formas anunciadas em `test_catraca_abono_do_motor_c11.py:145`, incluindo apelido de variável/import; `resultado.__getattribute__('dias_abono')` também foi captado como `str_arg` pela regra de `:74`. A allowlist declara contagem, papel e motivo em `:34`. `any→all` sobre realizado é morto pelo teste sem ata de `test_justificativa_c11_sem_banco.py:80` (uma falha, zero erros). [nao verificado]

## 15/11 02:05 · sessao 870959463a02 · - · - · origem=Ronald (tmux)

| arquivo:linha | O que está errado | Caso concreto | Gravidade |
|---|---|---|---|
| `app/templates/ponto/espelho.html:270`; `app/ponto/services/dia_pago.py:956`; `app/ponto/services/espelho.py:1264` | O rótulo c10 existe no resumo, mas a tela não o renderiza. Seu texto “sem apuração ainda” depende exclusivamente da inexistência de `DiaPago`; qualquer linha existente recebe `lavrada=True`. O PDF consulta o motivo correto em `app/relatorios/pdf_espelho.py:790`. | Dia justificado sem ata, com `DiaPago` existente: resumo recebe `motivo_dias_abono='sem_ata'`, mas a tela não mostra o rótulo exigido. O topo mostra outros campos em `app/templates/ponto/espelho.html:35`. | **bloqueia** |
| `app/ponto/services/espelho.py:1234`; `app/ponto/services/espelho.py:1264` | O conjunto que decide o rótulo cobre a janela visual, potencialmente anterior à competência, e não é filtrado pela competência. Apenas os furos recebem esse filtro em `app/ponto/services/espelho.py:1258`. | Dia sem ata na competência anterior faz o resumo atual receber abono desconhecido; o fechamento não repõe `dias_abono` nem limpa seu motivo em `app/relatorios/cartao_pela_celula.py:93`. | **bloqueia** |
| `app/ponto/services/dia_pago.py:395`; `app/ponto/services/dia_pago.py:292`; `app/ponto/services/fechamento.py:1225` | `None` do mesmo mapa significa desconhecido no fechamento e vira zero na lavratura, sem marca no detalhe. Reproduzido executando as funções reais por AST, sem persistência. | Grade `P=480`, abono/realizado `None` → `DiaPago` com abono `0.0`, previsto `480.0`, realizado `0.0`, detalhe `{}`. O fechamento c11 conserva abono/realizado `None` em `app/ponto/services/fechamento.py:1073`. | **bloqueia** |
| `app/relatorios/cartao_pela_celula.py:214`; `app/ponto/services/efeito_ausencia.py:96` | Uma fonte posterior conhecida não limpa o motivo c10: `None` é descartado também para o campo textual. | Reproduzido: resumo desconhecido + totais `{dias_abono:1, horas_abono:8, motivo_dias_abono:None}` termina com `1`, `8` e motivo `'sem_ata'`. O defeito está no leitor herdado, alimentado pelo novo escritor do motivo. | **fila** |
| `app/ponto/services/efeito_ausencia.py:75`; `app/folha/export.py:339`; `app/escala/services/leitor_celula.py:479` | A precedência de ausência inteira é aplicada somente no feed do motor. O caminho financeiro da grade lê os períodos crus e recoloca o desconhecido eliminado pelo feed. | Reproduzido com a fixture de `app/ponto/tests/test_justificativa_c10_sem_banco.py:148`: ausência inteira + justificativa + ata ausente → feed sem rótulo; grade e mapas de abono/realizado com `None`. O teste verifica só o primeiro lado em `app/ponto/tests/test_justificativa_c10_sem_banco.py:154`. | **fila** |

Os testes c10 executam somente atribuições e a chamada do rotulador, antes da aplicação final de totais e da renderização; portanto não cobrem os dois primeiros achados nem a retenção do motivo (`app/ponto/tests/test_justificativa_c10_sem_banco.py:47`, `app/ponto/tests/test_justificativa_c10_sem_banco.py:90`). Alcance desses casos em produção: **[nao verificado]**.

## 15/11 02:05 · sessao 8d658e89513a · - · - · origem=Ronald (tmux)

| Arquivo:linha | Erro / caso concreto | Gravidade |
|---|---|---|
| `app/ponto/services/fechamento.py:35`, `:539`, `:560`, `:1073`, `:1099`, `:1157`, `:1173` | **Exportada não está genericamente protegida.** A guarda contém somente 09/2026; outras competências exportadas recebem apenas log e seguem para `save/update`. Com dia `sem_ata`, c11 passa a persistir previsto/dias/carimbo onde antes o `except` os anulava, além de atualizar motivos/causa. Caso alcançável: competência 10 exportada, célula de trabalho justificada sem ata. Existência desse caso em produção: **[nao verificado]**. | **bloqueia** |
| `app/ponto/services/dia_pago.py:395`, `:292`; `app/folha/porta_export.py:453` | **Mesmo desconhecido vira zero e desaparece da conferência.** `por_dia_da_grade` entrega abono/realizado `None`; FM conserva `None`, mas `lavrar` grava zero. Comparador pula integralmente o campo cujo FM é `None`, sem registrar desconhecimento. Caso: P=480 e W sem ata → FM desconhecido, DiaPago=0, contador sem divergência nesse campo. | **bloqueia** |
| `app/folha/causas.py:85`, `:86`; `app/ponto/services/fechamento.py:1096`; `app/ponto/tests/test_justificativa_c11_sem_banco.py:61` | **Novo caminho ativo grava diagnóstico falso de realizado zero.** Com trabalho, P=480, W=`None`, sem missing/orfas, classificador transforma `None` em zero e devolve `realizado_zero`, evidência “zero minuto realizado”. Reproduzido executando o módulo real em memória, sem escrita. O teste substitui o classificador por resposta constante. | **fila** |
| `app/relatorios/cartao_pela_celula.py:146`, `:187`, `:208`, `:214`; `app/ponto/services/dia_pago.py:723` | **Lavratura pode desfazer parcialmente o rótulo.** Dia sem ata produz `dias_abono=None`, mas horas vêm da soma numérica de DiaPago, que já perdeu o desconhecido. `aplicar_nos_totais` pode entregar dias desconhecidos ao lado de horas numéricas. Os testes c10 excluem expressamente `folha_manda`: `app/ponto/tests/test_justificativa_c10_sem_banco.py:12`. | **fila** |

O TXT Domínio **não exporta esses três campos de minutos diretamente**: monta eventos em `app/folha/export.py:733`, definidos pelos campos `horas_*` em `:423` e `:442`. Portanto, “exporta zero silenciosamente por causa de `minutos_abonados=None`” não foi encontrado nesse emissor. Entretanto, a participação no TXT consulta motivos por `app/folha/export.py:971`; c11 passa a atualizar esses motivos em `app/ponto/services/fechamento.py:1080`.

A lista lê `prontidao` por colaborador em `app/ponto/services/fechamento_lista.py:85`; abono desconhecido cai na grade e retorna cego/`sem_ata` em `app/folha/export.py:1051` e `:1112`. O custo adicional em produção é **[nao verificado]**.

Há relógio herdado no caminho agora executado para `sem_ata`: `app/ponto/services/fechamento.py:1080` não fornece `hoje`; `app/folha/export.py:1317` encaminha `None` e `:1248` lê `localdate()`, apesar do instante único disponível em `app/ponto/services/fechamento.py:616`. Impacto concreto na virada de dia: **[nao verificado]**.

## 15/11 01:58 · sessao 556c7ef0bf1e · BATIDA-DEDUP-RETRATADA · verificador · origem=Code

**Veredito final: REPROVADO.** Há uma corrida que classifica reutilização como criação, mudança de sinalização para dupla viva e lacunas nos selos. A correção sequencial de V1, a consulta de V2 e a igualdade exigida em V3 estão presentes. (`app/ponto/services/resposta_batida.py:23`; `app/ponto/services/triagem_batida.py:664`; `app/ponto/services/triagem_batida.py:717`; `app/ponto/tests/test_tripwire_bocais_apuracao.py:37`)

Referências abaixo relativas a `app/`, exceto `CLAUDE.md`.

1. **A casca infere criação; a leitura anterior não prova quem criou.**

   O snapshot lê somente o PK existente naquele momento. A porta conhece seus ramos de reutilização/criação, mas retorna apenas `(batida, conflito)`; o helper tenta reconstruir essa informação comparando PK, timestamp, tipo e retratação. (`ponto/services/resposta_batida.py:10`; `ponto/services/resposta_batida.py:23`; `ponto/registro_batida.py:102`)

   **Se a execução chegou à porta**, os ramos sequenciais ficam assim:

   | Ramo | Resposta e efeitos |
   |---|---|
   | Viva do mesmo tipo em ±60 s, instante diferente | Porta devolve existente; timestamp diferente faz o helper responder `ja_registrada`, com campos do objeto. Pula efeitos posteriores da casca. (`ponto/registro_batida.py:113`; `ponto/services/resposta_batida.py:23`; `api/views.py:694`; `ponto/services/triagem_batida.py:724`) |
   | Viva do mesmo tipo, chave exata já vista pelo snapshot | PK igual denuncia reutilização; responde `ja_registrada`. Normalmente o dedup anterior já captura esse caso; nas APIs ele também pode ser alcançado após correção de tipo. (`ponto/services/resposta_batida.py:23`; `api/views.py:531`; `api/views.py:577`; `ponto/tests/test_dedup_retratada_c2.py:197`) |
   | Mesma chave retratada ou de outro tipo | Porta devolve a antiga, sem criar nem desfazer retratação. Helper responde `ja_registrada` com tipo, instante e sequência **da antiga**. Se houver viva temporal do tipo pedido, esse ramo temporal ganha antes da chave exata. (`ponto/registro_batida.py:113`; `ponto/registro_batida.py:120`; `ponto/services/resposta_batida.py:29`) |
   | Oposta viva, sem correspondência ao marco próprio | Nas três cascas, `autoridade_humana=False`: porta devolve `None`; casca responde `ignorada_intervalo`, antes do helper. (`ponto/registro_batida.py:145`; `ponto/registro_batida.py:150`; `api/views.py:657`; `api/views_core.py:648`; `ponto/services/triagem_batida.py:703`) |
   | Create normal, inclusive oposta admitida pelo marco próprio | Objeto nasce com timestamp/tipo pedidos, retratação NULL e PK novo; helper devolve `None`, permitindo os efeitos de nova. (`ponto/registro_batida.py:145`; `ponto/registro_batida.py:157`; `ponto/models.py:67`; `ponto/services/resposta_batida.py:23`) |
   | Create com conflito e autoridade humana | Existe na porta, mas essas três chamadas não usam autoridade humana. (`ponto/registro_batida.py:152`; `api/views.py:657`; `api/views_core.py:648`; `ponto/services/triagem_batida.py:703`) |
   | Colisão SQL no create | A unicidade declarada é colaborador/timestamp. Não há captura de `IntegrityError` nem recuperação da vencedora nesse corpo da porta ou em torno dessas chamadas. A exceção escapa desses trechos; resposta HTTP efetiva **[nao verificado]**. (`ponto/models.py:106`; `ponto/registro_batida.py:157`; `api/views.py:655`; `api/views_core.py:646`; `ponto/services/triagem_batida.py:701`) |

   **No código atual, não encontrei ramo sequencial de create normal classificado como reutilização. Encontrei o inverso em concorrência:** A lê chave ausente; B insere e confirma viva do mesmo tipo na chave; A entra na porta, que reutiliza B. Como `pk_anterior=None`, os quatro termos do helper passam e ele anuncia criação. (`ponto/services/resposta_batida.py:10`; `ponto/registro_batida.py:113`; `ponto/services/resposta_batida.py:23`)

   Reproduzi essa intercalação somente em memória, usando o corpo real da porta e das cascas: as APIs responderam `ok`; a triagem preencheu sucesso; havia uma única linha, criada pelo concorrente injetado. Rodaram foto, ordem, disputa, geofence e aviso; nas APIs, também o detector. O ensaio usa ORM/emissores dublês; ocorrência e frequência reais em produção **[nao verificado]**. (`ponto/tests/test_dedup_retratada_c2.py:80`; `ponto/tests/test_dedup_retratada_c2.py:136`; `ponto/tests/test_dedup_retratada_c2.py:156`; `api/views.py:697`; `api/views.py:705`; `ponto/services/triagem_batida.py:735`)

   Os efeitos posteriores protegidos são: ordem, detector/chamado leve, disputa, foto, geofence, autoresolve de entrada e aviso nas APIs; disputa, ordem, foto, geofence, autoresolve e contexto de sucesso na triagem. Create também dispara os efeitos da própria porta e signals; reutilização não produz um segundo SSE de criação. (`api/views.py:697`; `api/views.py:721`; `api/views.py:768`; `api/views.py:776`; `api/views.py:785`; `api/views.py:795`; `api/views.py:812`; `ponto/services/triagem_batida.py:735`; `ponto/services/triagem_batida.py:746`; `ponto/services/triagem_batida.py:752`; `ponto/services/triagem_batida.py:758`; `ponto/registro_batida.py:160`; `ponto/signals.py:42`)

   Portanto, a informação causal precisa sair da operação da porta; o snapshot não a substitui. Se o concorrente inserir retratada ou outro tipo, os testes de retratação/tipo ainda denunciam reutilização; a falha atual é a viva de mesmo tipo e chave surgida depois do snapshot. (`ponto/services/resposta_batida.py:23`; `ponto/registro_batida.py:120`)

2. **A mudança de `_bloqueio_horario_clt` altera negócio também para dupla viva.**

   Antes a função era chamada antes do dedup; agora qualquer dupla viva retorna em `664–665`, antes da chamada em `669`. O próprio teste exige zero chamadas de horário nesse controle. Isso ultrapassa a troca do predicado de retratada: modifica a sinalização de tentativas envolvendo exclusivamente batidas vivas. (`ponto/tests/dedup_retratada_c2_entrega.txt:19`; `ponto/services/triagem_batida.py:664`; `ponto/services/triagem_batida.py:669`; `ponto/tests/test_dedup_retratada_c2.py:241`)

   O “bloqueio” não recusa gravação. Quando encontra tentativa fora da janela, procura chamado em 14 horas e pode criar `batida_fora_horario`; escreve contexto, sessão e devolve `fora_horario_info`. Essa informação alimenta justificativa e `aprovada=False` enviados à porta. (`ponto/services/triagem_batida.py:394`; `ponto/services/triagem_batida.py:452`; `ponto/services/triagem_batida.py:459`; `ponto/services/triagem_batida.py:478`; `ponto/services/triagem_batida.py:486`; `ponto/services/triagem_batida.py:687`; `ponto/services/triagem_batida.py:710`)

   **Há trilha indireta:** `Chamado.criar` cria o chamado e chama `_trilha_do_nascimento`, que emite `nascimento_chamado`. A nova ordem pode suprimir chamado, nascimento/trilha e atualização de sessão que antes ocorreriam na segunda tentativa viva. (`chamados/models.py:1142`; `chamados/models.py:1143`; `chamados/models.py:1265`)

   Os leitores são a view que restaura a sessão e escolhe a tela, o template de fora de horário, o pedido de anuência e a aprovação da exceção. Essa aprovação lê tipo/hora e chama novamente a porta de batida. (`ponto/views.py:234`; `ponto/views.py:265`; `templates/ponto/triagem/fora_horario.html:15`; `templates/ponto/triagem/fora_horario.html:23`; `ponto/services/triagem_batida.py:319`; `ponto/services/triagem_batida.py:351`; `chamados/services/acoes_chamado.py:29`; `chamados/services/acoes_chamado.py:44`)

   Logo: **antes a dupla viva passava pela sinalização; agora não passa**. Autorização adicional para essa supressão de negócio **[nao verificado]**; o escopo local exige leitura literal do aval. (`ponto/tests/dedup_retratada_c2_entrega.txt:19`; `ponto/services/triagem_batida.py:664`; `CLAUDE.md:37`)

   A guarda também não garante ausência de emissão em toda recusa: sem chave exata, horário ainda roda antes de a porta recusar por oposta. O teste “sem efeitos” exclui `horario` por padrão. (`ponto/services/triagem_batida.py:669`; `ponto/services/triagem_batida.py:714`; `ponto/tests/test_dedup_retratada_c2.py:162`)

3. **Os dois payloads ricos de `ja_registrada` são congruentes; o front presente não os consome.**

   Helper e dupla viva da API têm os mesmos nove campos: `status`, `id`, `tipo`, `timestamp`, `sequencia`, `mensagem`, `sucesso`, `em_disputa`, `motivos_disputa`. Na dupla, o tipo pedido coincide com o objeto pelo filtro; no helper, vem diretamente do objeto. Ambos usam timestamp do objeto em `localtime().isoformat()` e mensagem HH:MM. (`api/views.py:531`; `api/views.py:535`; `ponto/services/resposta_batida.py:27`; `ponto/services/resposta_batida.py:29`)

   **Nesse contrato, `timestamp` é o instante do objeto devolvido**, podendo diferir do pedido. Isso já acontecia no dedup de cinco minutos e também é a fonte do timestamp no retorno `ok`; o delta não introduz uma segunda semântica nesse campo da resposta. O formato local acompanha a configuração São Paulo/`USE_TZ=True`. (`api/views.py:539`; `api/views.py:817`; `ponto/services/resposta_batida.py:33`; `config/settings/base.py:93`)

   `em_disputa=False` e motivos vazios não consultam disputa existente: são constantes tanto no retorno antigo quanto no helper. Portanto não certificam o estado atual de disputa do objeto reutilizado. (`api/views.py:543`; `ponto/services/resposta_batida.py:37`)

   Quanto aos leitores:

   | Consumidor | O que efetivamente lê |
   |---|---|
   | PWA online v2 | Usa HTTP `r.ok`, sem ler JSON, e apresenta sucesso com **tipo solicitado**. Pode continuar mostrando “Saída registrada” quando o objeto retornado é Entrada ou permanece retratado. (`templates/ponto/bater_ponto_v2.html:150`; `templates/ponto/bater_ponto_v2.html:310`; `templates/ponto/bater_ponto_v2.html:327`) |
   | Fila offline | Faz `resp.json()`, mas não usa os campos; exclui pendência pelo HTTP OK e pelo ID local `b.id`. (`static/pwa.js:158`; `static/pwa.js:162`) |
   | Template legado/triagem | Lê texto e faz `document.write`, sem interpretar o JSON; a dupla já retornava JSON mínimo e o helper acrescenta outro retorno JSON. (`templates/ponto/bater_ponto.html:533`; `templates/ponto/bater_ponto.html:539`; `ponto/services/triagem_batida.py:665`; `ponto/services/triagem_batida.py:726`) |
   | APK | Consumo dos nove campos e expectativa de timestamp **[nao verificado]**. O servidor recebe `batida_id` no upload, mas isso não prova como o cliente o obteve. (`api/views.py:840`; `api/views.py:846`) |

4. **A paridade exigida continua verde.** [nao verificado]

   O selo é `ContractParidadeKernelsTest.test_pares_de_mesmo_nome_identicos`: compara AST de funções de mesmo nome nos dois módulos, com whitelist atualmente vazia. Não exige identidade integral dos arquivos. (`api/tests/test_contract_paridade_kernels.py:12`; `api/tests/test_contract_paridade_kernels.py:20`; `api/tests/test_contract_paridade_kernels.py:26`; `api/tests/test_contract_paridade_kernels.py:34`)

   Reexecutei C1+C2 sem escrita: **20 testes OK**, incluindo esse contrato. O artefato entregue registra o mesmo resultado. (`ponto/tests/test_dedup_retratada_c2.py:292`; `ponto/tests/dedup_retratada_c2_green.txt:18`; `ponto/tests/dedup_retratada_c2_green.txt:27`)

   Limite: nas mutações em memória, `SOBRESCRITAS` não alimenta o `open()` do contrato de paridade. Seu verde certifica a cópia em disco, não a igualdade de cada mutante injetado. (`ponto/tests/test_dedup_retratada_c2.py:22`; `ponto/tests/test_dedup_retratada_c2.py:294`; `api/tests/test_contract_paridade_kernels.py:19`)

5. **V3 ganhou mordida nos três sítios, mas há mutantes de uma linha vivos.**

   O tripwire agora exige contagem exata: UI=2, core=2, triagem=1, helper=1. O selo C2 restaura a consulta crua em cada um dos três dedups e exige que o tripwire existente falhe; esse teste passou na reprodução. (`ponto/tests/test_tripwire_bocais_apuracao.py:21`; `ponto/tests/test_tripwire_bocais_apuracao.py:27`; `ponto/tests/test_tripwire_bocais_apuracao.py:37`; `ponto/tests/test_dedup_retratada_c2.py:271`)

   O mutante entregue “retratada tratada como nova” desliga a guarda `_resposta_existente`; ele é morto pelo caso que exige payload da retratada e ausência de efeitos. O tripwire de contagem, isoladamente, não verifica essa semântica. (`ponto/tests/test_dedup_retratada_c2.py:170`; `ponto/tests/test_dedup_retratada_c2.py:185`; `ponto/tests/test_dedup_retratada_c2.py:329`; `ponto/tests/test_tripwire_bocais_apuracao.py:30`)

   **Dois mutantes adicionais sobreviveram aos 20 testes, somente em memória:** [nao verificado]

   - Remover `and batida.retratada_em is None` da linha 24 do helper. A retratada dos testes já existe antes do snapshot, então o PK continua denunciando reutilização; a condição removida não é isolada. Isso deixa descoberta a chave retratada que surge depois do snapshot. (`ponto/services/resposta_batida.py:24`; `ponto/tests/test_dedup_retratada_c2.py:172`; `ponto/tests/test_dedup_retratada_c2.py:185`)
   - Na linha 23, trocar `batida.timestamp == timestamp` por `batida.timestamp == timestamp.replace(microsecond=0)`. Os 20 testes continuam verdes porque o instante-base tem microssegundos zero. Com `microsecond=123456`, o ensaio criou uma batida nos três sítios, respondeu `ja_registrada` e pulou foto/ordem. **Essa perda de efeitos é do mutante, não do código atual.** (`ponto/services/resposta_batida.py:23`; `ponto/tests/test_dedup_retratada_c1.py:26`; `ponto/tests/test_dedup_retratada_c2.py:206`)

   V2 foi corrigido quanto a universo e janela: triagem usa `_janela_oposto` e `batidas_apuraveis`, com limites inclusivos. Ainda reconstrói a testemunha depois da recusa, pois a porta devolve descrição, não o objeto causador; alterações concorrentes entre essas consultas continuam possíveis. (`ponto/services/triagem_batida.py:715`; `ponto/services/triagem_batida.py:717`; `ponto/turnos.py:2876`; `ponto/turnos.py:2878`; `ponto/registro_batida.py:146`)

6. **Escritores, leitores e classes estruturais.**

   | Valor lido/escrito pelo delta | Quem escreve; quem lê |
   |---|---|
   | Colaborador, PK/ID | Casca fornece colaborador; create da porta/ORM produz a linha e PK. Snapshot, helper e respostas leem identidade; chave única é colaborador/timestamp. (`api/views.py:654`; `ponto/registro_batida.py:157`; `ponto/models.py:106`; `ponto/services/resposta_batida.py:10`; `ponto/services/resposta_batida.py:31`) |
   | Timestamp pedido/persistido | APIs recebem `ts_efetivo` de `hora_do_aparelho`; triagem captura `agora`; porta grava argumento. Snapshot compara chave; helper compara instante e apresenta o do objeto. (`api/views.py:507`; `api/views_core.py:493`; `ponto/services/triagem_batida.py:626`; `ponto/registro_batida.py:158`; `ponto/services/resposta_batida.py:23`; `ponto/services/resposta_batida.py:27`) |
   | Tipo | Request fornece; APIs podem corrigir após dedup; porta persiste; flip posterior usa `anotar`. Helper compara pedido final e apresenta tipo do objeto. (`api/views.py:420`; `api/views.py:577`; `api/views.py:600`; `ponto/registro_batida.py:158`; `ponto/services/flip_batida.py:30`; `ponto/services/resposta_batida.py:24`; `ponto/services/resposta_batida.py:32`) |
   | `retratada_em` | Retratar escreve carimbo por `anotar`; desfazer escreve NULL. Autoridade filtra NULL; snapshot inclui ambas; helper lê NULL/não NULL. (`ponto/registro_batida.py:274`; `ponto/registro_batida.py:254`; `ponto/services/retratar_humano.py:207`; `ponto/turnos.py:2876`; `ponto/services/resposta_batida.py:10`; `ponto/services/resposta_batida.py:24`) |
   | Sequência | Cascas calculam pela autoridade e enviam à porta; helper apenas lê a sequência antiga do objeto reutilizado. (`api/views.py:554`; `api/views_core.py:540`; `ponto/services/triagem_batida.py:674`; `ponto/services/triagem_batida.py:706`; `ponto/services/resposta_batida.py:34`) |
   | `_pk_anterior` | Snapshot escreve variável local; helper decide classificação e triagem decide antecipadamente se chama o emissor. (`api/views.py:654`; `api/views_core.py:645`; `ponto/services/triagem_batida.py:667`; `ponto/services/triagem_batida.py:669`; `ponto/services/resposta_batida.py:23`) |
   | Payload/`_resposta_existente` | Helper monta dicionário; três cascas retornam antes dos efeitos posteriores. Leitores de front estão no item 3. (`ponto/services/resposta_batida.py:29`; `api/views.py:694`; `api/views_core.py:685`; `ponto/services/triagem_batida.py:724`) |
   | `g_op`, `op`, ID/tipo da recusa | `_janela_oposto` fornece 300/60 s; porta e casca consultam; triagem escolhe candidata viva descendente e lê ID/tipo, sem gravá-la. (`ponto/registro_batida.py:21`; `ponto/registro_batida.py:138`; `ponto/services/triagem_batida.py:716`; `ponto/services/triagem_batida.py:720`) |
   | `fora_horario_info`, justificativa, aprovação | Emissor produz informação/contexto/sessão; montagem local transforma em justificativa/aprovação; porta persiste. Leitores de anuência estão no item 2. (`ponto/services/triagem_batida.py:478`; `ponto/services/triagem_batida.py:491`; `ponto/services/triagem_batida.py:685`; `ponto/services/triagem_batida.py:710`) |
   | Inventário/regex do tripwire | Teste declara contagens/donos e lê fontes para comparar igualdade. (`ponto/tests/test_tripwire_bocais_apuracao.py:14`; `ponto/tests/test_tripwire_bocais_apuracao.py:30`; `ponto/tests/test_tripwire_bocais_apuracao.py:35`) |

   **Não sustento “segundo juiz registrado” específico:** o registro vivo de batida não declara a pergunta “foi criada neste toque”. Sustento **derivação paralela do resultado da escrita**, demonstrada pela corrida do item 1. (`core/juizes.py:1196`; `core/juizes.py:1211`; `ponto/registro_batida.py:113`; `ponto/registro_batida.py:157`; `ponto/services/resposta_batida.py:23`)

   O helper não acrescenta escritor de `Batida` nem relógio: faz SELECT, comparação e formatação. O escritor declarado continua na porta; os relógios de `_bloqueio_horario_clt` já pertencem ao corpo existente, cuja execução foi reposicionada. (`core/portas.py:103`; `ponto/services/resposta_batida.py:10`; `ponto/services/resposta_batida.py:27`; `ponto/services/triagem_batida.py:455`; `ponto/services/triagem_batida.py:469`; `ponto/tests/dedup_retratada_c2_entrega.txt:19`)

Suite inteira, opus final, smoke real nas duas cascas, medições dos 60 dias, empresas e afetados: **[nao verificado]**. A entrega também os declara pendentes; os 20 testes locais não os substituem. (`ponto/tests/dedup_retratada_c2_entrega.txt:129`; `ponto/tests/dedup_retratada_c2_entrega.txt:132`; `ponto/tests/dedup_retratada_c2_entrega.txt:137`)

## 15/11 01:58 · sessao 9aa173e69482 · E3-RESTO-R3 · sonda · origem=Code

import datetime as dt
import io
import logging
import re
import sys
from contextlib import redirect_stderr, redirect_stdout
from unittest.mock import patch

AGORA = dt.datetime.fromisoformat("2026-11-15T03:00:00+00:00")
HOJE = dt.date(2026, 11, 15)
SAIDA = sys.stdout
ERROS = 0
K6_CHAVES = False


def emitir(linha):
    print(linha, file=SAIDA, flush=True)


def erro(tipo, onde):
    global ERROS
    ERROS += 1
    emitir("ERRO %s %s" % (tipo, onde))


def local_seguro(caminho, numero):
    caminho = str(caminho).replace("\\", "/")
    if "/app/" in caminho:
        return "%s:%d" % (caminho.rsplit("/app/", 1)[1], numero)
    return "sonda:%d" % numero


def registrar_log(self, registro):
    if registro.exc_info and registro.exc_info[0]:
        tb = registro.exc_info[2]
        if tb is not None:
            while tb.tb_next is not None:
                tb = tb.tb_next
            onde = local_seguro(tb.tb_frame.f_code.co_filename, tb.tb_lineno)
        else:
            onde = local_seguro(registro.pathname, registro.lineno)
        erro(registro.exc_info[0].__name__, onde)
    elif registro.levelno >= logging.WARNING:
        erro("AvisoLogging", local_seguro(registro.pathname, registro.lineno))


def protegido(onde, funcao):
    inicio = ERROS
    saida, diagnosticos = io.StringIO(), io.StringIO()
    resultado = None
    with redirect_stdout(saida), redirect_stderr(diagnosticos):
        with patch.object(logging.Logger, "handle", registrar_log):
            try:
                resultado = funcao()
            except BaseException as exc:
                tb = exc.__traceback__
                while tb is not None and tb.tb_next is not None:
                    tb = tb.tb_next
                origem = (
                    local_seguro(tb.tb_frame.f_code.co_filename, tb.tb_lineno)
                    if tb else onde
                )
                erro(type(exc).__name__, "%s %s" % (onde, origem))
    if saida.getvalue().strip():
        erro("SaidaInesperada", "%s stdout_suprimido" % onde)
    for linha in diagnosticos.getvalue().splitlines():
        if not linha.strip():
            continue
        achado = re.fullmatch(
            r"col([0-9]+): espelho falhou \(([A-Za-z_][A-Za-z_0-9]*)\) -- fora da apuracao",
            linha.strip(),
        )
        if achado:
            erro(achado[2], "%s col%s apurar" % (onde, achado[1]))
        else:
            erro("DiagnosticoStderr", "%s stderr_suprimido" % onde)
    return ERROS == inicio, resultado


def carregar():
    from colaboradores.models import Empresa
    from escala.services.cadastro_realidade import beco_em_lote
    from folha.export import competencia_entregue
    from ponto.janelas import janela_fechamento
    from ponto.management.commands import pautas_discordantes

    return (
        Empresa, beco_em_lote, competencia_entregue,
        janela_fechamento, pautas_discordantes,
    )


def medir_k6(emp, mes, ano, ini, fim, beco):
    itens = list(beco([(emp, ini, fim)], HOJE, agora=AGORA))
    linhas = []
    primeiras_chaves = None
    for cid, assinatura in itens:
        if assinatura["codigo"] != "C11":
            raise ValueError("assinatura_invalida")
        if primeiras_chaves is None:
            chaves = sorted(assinatura)
            if not all(
                re.fullmatch(r"[A-Za-z_][A-Za-z_0-9]*", k)
                for k in chaves
            ):
                raise ValueError("chaves_invalidas")
            primeiras_chaves = " ".join(chaves)
        data = dt.date.fromisoformat(assinatura["data"]).isoformat()
        perguntas = assinatura.get("perguntas")
        n_perguntas = None if perguntas is None else len(perguntas)
        folga = assinatura.get("folga_nao_definida")
        if folga is not None and type(folga) is not bool:
            raise TypeError("folga_invalida")
        linhas.append(
            "K6I emp%d %d/%d col%d %s perguntas=%s folga=%s"
            % (emp.pk, mes, ano, int(cid), data, n_perguntas, folga)
        )
    return len(itens), primeiras_chaves, linhas


def medir_k3(emp, mes, ano, modulo):
    nativo = io.StringIO()
    comando = modulo.Command(
        stdout=nativo, stderr=sys.stderr, no_color=True
    )
    with patch.object(modulo.tz, "now", return_value=AGORA):
        comando.handle(
            mes=mes,
            ano=ano,
            empresa=[emp.pk],
            hoje=HOJE.isoformat(),
            apply=False,
        )
    linhas = [
        x for x in nativo.getvalue().splitlines() if x.strip()
    ]
    prefixos = (
        "pautas_discordantes ",
        "dia-colab de dono ESTRUTURA ",
        "dia-colab sem leitura (sistema)",
        "pautas vivas retidas",
    )
    if not linhas or not all(x.startswith(prefixos) for x in linhas):
        raise ValueError("saida_do_command_inesperada")
    return linhas


def executar():
    global K6_CHAVES
    valido, fontes = protegido("imports", carregar)
    if not valido:
        return
    Empresa, beco, entregue, janela, modulo_k3 = fontes
    valido, empresas = protegido(
        "empresas",
        lambda: list(
            Empresa.objects.filter(ativa=True).order_by("pk").only(
                "pk", "ativa", "dia_inicio_competencia"
            )
        ),
    )
    if not valido:
        return
    for emp in empresas:
        for mes, ano in ((11, 2026), (12, 2026)):
            contexto = "emp%d %d/%d" % (emp.pk, mes, ano)
            valido, periodo = protegido(
                contexto + " janela",
                lambda: janela(mes, ano, emp),
            )
            if not valido:
                continue
            ini, fim = periodo
            if mes == 12 and ini > HOJE:
                continue
            valido, fechada = protegido(
                contexto + " entregue",
                lambda: entregue(emp.pk, mes, ano),
            )
            if not valido:
                continue
            if fechada:
                emitir("COMP %s aberta=0" % contexto)
                continue

            valido, medicao = protegido(
                "K6 " + contexto,
                lambda: medir_k6(emp, mes, ano, ini, fim, beco),
            )
            if valido:
                n, chaves, linhas = medicao
                emitir("K6 %s n=%d" % (contexto, n))
                if not K6_CHAVES and chaves is not None:
                    emitir("K6K " + chaves)
                    K6_CHAVES = True
                for linha in linhas:
                    emitir(linha)

            valido, linhas = protegido(
                "K3 " + contexto,
                lambda: medir_k3(emp, mes, ano, modulo_k3),
            )
            if valido:
                for linha in linhas:
                    emitir("K3 %s %s" % (contexto, linha))
                if not any(
                    "dia-colab sem leitura (sistema)" in x
                    for x in linhas
                ):
                    emitir("K3 sem_contador_nesta_arvore")


try:
    executar()
except BaseException as exc:
    erro(type(exc).__name__, "sonda executar")
finally:
    emitir("FIM ok=%d" % (ERROS == 0))

---FIM-SCRIPT---

- Empresas ativas e ordem: app/colaboradores/models.py:40, app/colaboradores/models.py:44, app/escala/services/cadastro_realidade.py:543, app/escala/services/cadastro_realidade.py:547.
- janela_fechamento e corte por empresa: app/ponto/janelas.py:10, app/ponto/janelas.py:36, app/ponto/janelas.py:54.
- Competência aberta pelo juiz de entrega: app/escala/services/cadastro_realidade.py:528, app/ponto/services/he_pendente_lavrado.py:218, app/folha/export.py:556, app/folha/export.py:580. Dependências de leitura: app/folha/models.py:192, app/folha/models.py:203.
- beco_em_lote: app/escala/services/cadastro_realidade.py:414. Predicado de folga: app/escala/services/cadastro_realidade.py:498. Assinatura retornada, com data/perguntas e sem campo folga_nao_definida nesta cópia: app/escala/services/cadastro_realidade.py:510.
- K3 usa handle para executar o apurar e sincronizar reais e preservar a impressão de cada árvore: app/ponto/management/commands/pautas_discordantes.py:56, app/ponto/management/commands/pautas_discordantes.py:113, app/ponto/management/commands/pautas_discordantes.py:117, app/ponto/management/commands/pautas_discordantes.py:118. Relógio da borda: app/ponto/management/commands/pautas_discordantes.py:115.
- Contagens existentes nesta cópia: app/ponto/management/commands/pautas_discordantes.py:119, app/ponto/management/commands/pautas_discordantes.py:125. O handle termina sem os dois contadores novos: app/ponto/management/commands/pautas_discordantes.py:129.
- DRY: sincronizar e consultas em app/ponto/services/pautas_dos_discordantes.py:155, app/ponto/services/pautas_dos_discordantes.py:165; escritores protegidos por apply em app/ponto/services/pautas_dos_discordantes.py:177.
- Dependências da apuração auditadas: app/ponto/management/commands/e6_oraculo.py:128, app/ponto/management/commands/e6_oraculo.py:162, app/ponto/management/commands/e6_oraculo.py:182; app/ponto/services/espelho.py:1194; app/ponto/services/fechamento.py:277.
- Erros internos que exigem captura: app/ponto/management/commands/pautas_discordantes.py:101, app/colaboradores/services/calendario.py:616, app/core/regua_cct.py:442. Uso de timezone indicado no anexo: app/relatorios/cartao_pela_celula.py:412.
- Delta c6 e execução nas duas árvores: [nao verificado]; a adaptação aos contadores usa a saída real do comando, conforme o anexo fornecido.

## 15/11 01:58 · sessao b445a52b3bee · JUSTIFICATIVA-PAGA · sonda · origem=Code

import sys
sys.dont_write_bytecode = True

import io
import time
import logging
from collections import Counter
from contextlib import contextmanager, redirect_stdout, redirect_stderr

# Retorno real: (abono, previsto, realizado), mapas por data.
# app/ponto/services/dia_pago.py:364-402
# Nesta copia, a funcao nao propaga None nem le min_justificados.
# app/ponto/services/dia_pago.py:386-402
# D1 mede os mapas reais da main; impacto c11: [nao verificado].
# Execucao com banco e resultados: [nao verificado].

MES, ANO = 11, 2026
OK = True
FALHOU = object()


@contextmanager
def silencioso():
    nivel_anterior = logging.root.manager.disable
    with io.StringIO() as descarte:
        with redirect_stdout(descarte), redirect_stderr(descarte):
            logging.disable(sys.maxsize)
            try:
                yield
            finally:
                logging.disable(nivel_anterior)


def erro(exc, onde):
    global OK
    OK = False
    print("ERRO {} {}".format(type(exc).__name__, onde), flush=True)


def tentar(onde, func, *args):
    try:
        with silencioso():
            return func(*args)
    except Exception as exc:
        erro(exc, onde)
        return FALHOU


def carregar():
    global Empresa, FechamentoMensal, EscalaColaborador, Count
    global universo_da_competencia, janela_fechamento
    global grade_do_fechamento, por_dia_da_grade, prontidao

    from colaboradores.models import Empresa
    from ponto.models import FechamentoMensal
    from escala.models import EscalaColaborador
    from django.db.models import Count
    from ponto.services.fechamento import universo_da_competencia
    from ponto.janelas import janela_fechamento
    from folha.export import grade_do_fechamento, prontidao
    from ponto.services.dia_pago import por_dia_da_grade


def ler_colaborador(colab, ini, fim):
    # Mesmo leitor de grade do escritor; objeto apenas em memoria.
    # app/ponto/services/fechamento.py:1013-1014
    # app/folha/export.py:329-338
    referencia = FechamentoMensal(
        colaborador=colab, mes=MES, ano=ANO
    )
    grade = grade_do_fechamento(referencia)
    abono, previsto, realizado = por_dia_da_grade(grade)

    # Ausencia de chave nao equivale a valor None.
    # app/ponto/services/dia_pago.py:398-402
    sem_ata = {
        data
        for mapa in (abono, realizado)
        for data, valor in mapa.items()
        if valor is None
    }

    tipos_por_data = {}
    for dia in grade["dias"]:
        data = dia["data"]
        if data in tipos_por_data:
            raise ValueError("data_duplicada")
        tipos_por_data[data] = dia["tipo_dia"]

    if any(
        data is None or not ini <= data <= fim
        or data not in tipos_por_data
        for data in sem_ata
    ):
        raise ValueError("data_invalida_no_retorno")

    # Sem filtro de tipo_dia: inclui qualquer tipo presente nos mapas.
    tipos = Counter(tipos_por_data[data] for data in sorted(sem_ata))
    return len(sem_ata), tipos


def medir_d1():
    # Empresa.ativa: app/colaboradores/models.py:44
    empresas = list(Empresa.objects.filter(ativa=True).order_by("pk"))
    universo_total = []
    universo_completo = True

    for empresa in empresas:
        emp = empresa.pk
        janela = tentar(
            "D1 emp{} janela".format(emp),
            janela_fechamento, MES, ANO, empresa,
        )
        # Mesmo universo chamado pelo escritor.
        # app/ponto/services/fechamento.py:428-445
        # app/ponto/services/fechamento.py:554
        colabs = tentar(
            "D1 emp{} universo".format(emp),
            lambda: list(
                universo_da_competencia(MES, ANO, emp)
                .select_related("empresa", "posto__praca")
                .order_by("pk")
            ),
        )
        if colabs is FALHOU:
            universo_completo = False
            continue

        universo_total.extend(colabs)
        if janela is FALHOU:
            continue

        ini, fim = janela
        completo = True
        n_colabs = 0
        n_dias = 0

        for colab in colabs:
            resultado = tentar(
                "D1 emp{} col{}".format(emp, colab.pk),
                ler_colaborador, colab, ini, fim,
            )
            if resultado is FALHOU:
                completo = False
                continue

            dias, tipos = resultado
            n_colabs += int(dias > 0)
            n_dias += dias
            print(
                "D1I emp{} col{} dias_sem_ata={} tipos={!r}".format(
                    emp, colab.pk, dias, tipos
                ),
                flush=True,
            )

        if completo:
            print(
                "D1 emp{} colabs={} dias={}".format(
                    emp, n_colabs, n_dias
                ),
                flush=True,
            )

    return universo_total, universo_completo


def ler_d2():
    # Campos e nulabilidade: app/ponto/models.py:405-409,432-453
    return list(
        FechamentoMensal.objects.filter(
            previsto_em__isnull=False,
            minutos_previstos__isnull=False,
            minutos_abonados__isnull=True,
        )
        .values("colaborador__empresa_id", "mes", "ano")
        .annotate(n=Count("pk"))
        .order_by("colaborador__empresa_id", "ano", "mes")
    )


def preparar_d3(colabs):
    ids = [colab.pk for colab in colabs]
    fechamentos = {
        fech.colaborador_id: fech
        for fech in FechamentoMensal.objects.filter(
            colaborador_id__in=ids, mes=MES, ano=ANO
        ).select_related("colaborador__empresa", "colaborador__posto__empresa")
    }

    # Todas as escalas, sem filtro ativa, como na lista.
    # app/ponto/services/fechamento_lista.py:54-57
    rows = {}
    for escala in EscalaColaborador.objects.filter(
        colaborador_id__in=ids
    ).select_related("tipo_escala"):
        rows.setdefault(escala.colaborador_id, []).append(escala)
    return fechamentos, rows


def cronometrar(fech, rows):
    # Mesma chamada da lista, sem alterar o snapshot.
    # app/ponto/services/fechamento_lista.py:85-86
    inicio = time.perf_counter()
    prontidao(fech, rows=rows)
    return time.perf_counter() - inicio


def executar():
    if tentar("imports", carregar) is FALHOU:
        return

    d1 = tentar("D1", medir_d1)

    d2 = tentar("D2", ler_d2)
    if d2 is not FALHOU:
        for linha in d2:
            print(
                "D2 emp{} {}/{} n={}".format(
                    linha["colaborador__empresa_id"],
                    linha["mes"], linha["ano"], linha["n"],
                ),
                flush=True,
            )

    if d1 is FALHOU or not d1[1]:
        raise RuntimeError("universo_D3_incompleto")

    # D3: cinco primeiros PKs do universo percorrido em D1.
    amostra = sorted(d1[0], key=lambda colab: colab.pk)[:5]
    if len(amostra) < 5:
        erro(RuntimeError(), "D3 amostra_insuficiente")

    preparados = tentar("D3 preparacao", preparar_d3, amostra)
    if preparados is FALHOU:
        return
    fechamentos, rows = preparados

    for colab in amostra:
        fech = fechamentos.get(colab.pk)
        if fech is None:
            erro(RuntimeError(), "D3 col{} fechamento_ausente".format(colab.pk))
            continue
        segundos = tentar(
            "D3 col{}".format(colab.pk),
            cronometrar, fech, rows.get(colab.pk),
        )
        if segundos is not FALHOU:
            print("D3 col{} s={:.6f}".format(colab.pk, segundos), flush=True)


try:
    executar()
except Exception as exc:
    erro(exc, "execucao")
finally:
    print("FIM ok={}".format(int(OK)), flush=True)

---FIM-SCRIPT---
bin/sonda_leitura.sh:4-5,93-105,195-250
app/colaboradores/models.py:40-53,283-340
app/ponto/models.py:398-505
app/ponto/janelas.py:10-57,75-108
app/ponto/services/dia_pago.py:350-470
app/ponto/services/fechamento.py:1-90,382-660,995-1130,1518-1573
app/ponto/services/fechamento_lista.py:1-131
app/folha/export.py:315-350,1020-1220
app/escala/services/leitor_celula.py:350-451

## 15/11 01:55 · sessao d5185e3774e2 · - · - · origem=Ronald (tmux)

Caminho lido: Command.apurar(mes, ano, empresas, hoje, *, agora), seguido de sincronizar(apurado, apply=False), conforme handle em app/ponto/management/commands/pautas_discordantes.py:56, :117-118.

sincronizar só chama escritores dentro de if apply; com False faz consultas e contagens. app/ponto/services/pautas_dos_discordantes.py:155, :165-195.

Dependências auditadas sem escrita encontrada: InsumosDoDono consulta batidas, fase e cadastro; cxr chama espelho_do_colab. app/ponto/management/commands/e6_oraculo.py:128-195. O espelho monta dados, chama autoridade/motor e folha_manda. app/ponto/services/espelho.py:1194-1296. A composição por fatia opera nos resultados em memória. app/ponto/services/fechamento.py:277-321.

A cópia termina handle sem os contadores sem_leitura/retidas; cabe K3 sem_contador_nesta_arvore. app/ponto/management/commands/pautas_discordantes.py:119-129.

Erros internos exigem captura: apurar engole erro de cxr e escreve stderr, removendo o colaborador da apuração. app/ponto/management/commands/pautas_discordantes.py:101-106. calendario registra exceção e segue com oferta desconhecida. app/colaboradores/services/calendario.py:616-624. O switch CCT também registra exceção e segue. app/core/regua_cct.py:442-448. Recomendo handler de logging que imprima apenas tipo e módulo:linha, sem mensagem/traceback, além de stderr do command que marque erro. [não verificado]

## 15/11 01:55 · sessao 4b7a06773022 · - · - · origem=Ronald (tmux)

1. **Paridade continua verde.** O contrato exige AST idêntica para funções de mesmo nome, salvo whitelist, atualmente vazia (`app/api/tests/test_contract_paridade_kernels.py:4`; `:12`; `:26`; `:34`). Reexecutei C1+C2 sem escrita: **20 testes OK**, incluindo contrato real chamado por C2 (`app/ponto/tests/test_dedup_retratada_c2.py:292`; artefato concordante `app/ponto/tests/dedup_retratada_c2_green.txt:18`; `:27`; `:29`). O contrato cobre funções top-level compartilhadas por nome; não compara módulos inteiros (`app/api/tests/test_contract_paridade_kernels.py:20`; `:31`).

2. **V3 curado para as restaurações cruas pedidas.** Tripwire agora cobra igualdade exata UI=2, core=2, triagem=1 e helper=1 (`app/ponto/tests/test_tripwire_bocais_apuracao.py:21`; `:23`; `:25`; `:27`; `:37`). O selo C2 restaura query crua em cada sítio e exige falha do tripwire existente (`app/ponto/tests/test_dedup_retratada_c2.py:271`). C1 também verifica origem da `dupla` e filtros crus com alias antes da porta (`app/ponto/tests/test_dedup_retratada_c1.py:194`; `:197`; `:286`). O tripwire continua regex de contagem para arquivos enumerados; não valida semântica das condições do helper (`app/ponto/tests/test_tripwire_bocais_apuracao.py:14`; `:30`; `:36`).

3. **Mutante “retratada tratada como nova” escolhido pelo construtor morre.** Seu modo `nova` troca a guarda `_resposta_existente` por `False`, portanto deixa passar todo reuso (`app/ponto/tests/test_dedup_retratada_c2.py:329`). O caso específico confere payload do objeto, ausência de criação e de efeitos (`app/ponto/tests/test_dedup_retratada_c2.py:170`; `:180`; `:185`). Artefato registra as falhas do mutante da API (`app/ponto/tests/dedup_retratada_c2_mutantes.txt:286`; `:292`).

4. **Achei e executei mutante de UMA LINHA que passa os 20 testes.** Remover `and batida.retratada_em is None` de `app/ponto/services/resposta_batida.py:24`, apenas via `SOBRESCRITAS` em memória, manteve **20 OK**. A cobertura atual sempre começa com a retratada já presente antes do snapshot, então a condição de PK continua denunciando o reuso; não isola a condição `retratada_em` (`app/ponto/tests/test_dedup_retratada_c2.py:172`; `:185`; `app/ponto/services/resposta_batida.py:23`). Esse mutante erra quando uma mesma chave retratada aparece depois do snapshot, mas antes da porta: sem o filtro removido, vira “nova” (`app/ponto/services/resposta_batida.py:10`; `:23`; `app/ponto/registro_batida.py:120`).

5. **Corrida do código atual reproduzida em memória: reutilizada viva passa como nova.** Injetei somente entre leitura e retorno de `chave_antes_do_toque`: snapshot retorna `None`, outro toque cria viva S na mesma chave, a porta retorna essa viva pelo ramo `mesma` (`app/ponto/services/resposta_batida.py:10`; `app/ponto/registro_batida.py:113`). A guarda aceita `pk != None`, timestamp/tipo iguais e retratação NULL, devolvendo `None` como se tivesse criado (`app/ponto/services/resposta_batida.py:23`).

   Resultado observado nos trechos reais: APIs responderam `status=ok`, rodaram detector, disputa, geofence, foto, ordem e aviso; triagem preencheu contexto de sucesso e rodou disputa, geofence, foto, ordem, autoresolve e aviso. Havia **uma única linha**, criada pelo concorrente injetado. O SSE observado pertence a esse create concorrente, pois o sinal exige `created=True` (`app/ponto/signals.py:40`). Não atribuir segundo SSE à casca; SQL/concorrência real permanecem **[nao verificado]**. O harness permite essa execução por código real da porta e pós-porta com ORM/emissores dublês (`app/ponto/tests/test_dedup_retratada_c2.py:80`; `:113`; `:136`; `:156`).

6. **Limite adicional do ensaio de paridade durante mutações.** `SOBRESCRITAS` alimenta `fonte`, mas o contrato de paridade usa `open(path)` diretamente: a execução de mutantes em memória das APIs não modifica o que esse contrato compara (`app/ponto/tests/test_dedup_retratada_c2.py:22`; `:292`; `app/api/tests/test_contract_paridade_kernels.py:19`). Isso não invalida o verde do delta em disco, mas impede interpretar esse teste como mordida da paridade contra cada mutante em memória.

## 15/11 01:54 · sessao 93e6ff425bc4 · - · - · origem=Ronald (tmux)

1. **Mudança de negócio além da consulta de retratada:** agora qualquer dupla **viva** nos cinco minutos retorna antes de `_bloqueio_horario_clt`; antes a função era chamada primeiro, conforme a própria entrega e o diff fornecido. Portanto uma segunda tentativa fora de horário deixa de criar sinalização/contexto/sessão, mesmo sem envolver retratada. O teste novo exige explicitamente essa supressão (`app/ponto/services/triagem_batida.py:663`; `app/ponto/services/triagem_batida.py:669`; `app/ponto/tests/dedup_retratada_c2_entrega.txt:19`; `app/ponto/tests/test_dedup_retratada_c2.py:229`; `app/ponto/tests/test_dedup_retratada_c2.py:241`).

2. **O que escreve:** `_bloqueio_horario_clt` procura chamado nos últimos 14h; sem um, chama `Chamado.criar` com contexto da tentativa. Também escreve `context['bloqueio_horario']`, sessão e devolve `fora_horario_info`. Este último alimenta `justificativa` e `aprovada=False` enviadas à porta. Seu nome não significa recusa da batida (`app/ponto/services/triagem_batida.py:452`; `app/ponto/services/triagem_batida.py:459`; `app/ponto/services/triagem_batida.py:478`; `app/ponto/services/triagem_batida.py:486`; `app/ponto/services/triagem_batida.py:491`; `app/ponto/services/triagem_batida.py:687`; `app/ponto/services/triagem_batida.py:710`).

3. **Log indireto:** `Chamado.criar` cria o objeto e chama `_trilha_do_nascimento`; esta emite `nascimento_chamado`, com estado e módulo. Assim a mudança também suprime esse nascimento/trilha quando a tentativa viva duplicada teria aberto chamado novo (`app/chamados/models.py:1142`; `app/chamados/models.py:1143`; `app/chamados/models.py:1256`; `app/chamados/models.py:1265`).

4. **Quem lê:** a view restaura o bloqueio da sessão e escolhe tela; o template apresenta horário/diferença/escala e envia tipo/hora/chamado ao pedido de anuência. O ramo de anuência registra nota, renova contexto, escala urgência e envia push. A aprovação lê tipo/hora desse contexto e registra pela porta (`app/ponto/views.py:234`; `app/ponto/views.py:265`; `app/templates/ponto/triagem/fora_horario.html:15`; `app/templates/ponto/triagem/fora_horario.html:23`; `app/ponto/services/triagem_batida.py:319`; `app/ponto/services/triagem_batida.py:351`; `app/ponto/services/triagem_batida.py:369`; `app/ponto/services/triagem_batida.py:372`; `app/chamados/services/acoes_chamado.py:29`; `app/chamados/services/acoes_chamado.py:44`).

5. **Pré-porta ainda emite em recusas:** sem chave exata anterior, `_bloqueio_horario_clt` roda antes de a porta decidir conflito oposto. A recusa posterior não desfaz chamado/sessão; o teste de “sem efeitos” exclui `horario` por padrão. Logo “zero emissão em toda reutilização/recusa” não é garantido (`app/ponto/services/triagem_batida.py:669`; `app/ponto/services/triagem_batida.py:701`; `app/ponto/services/triagem_batida.py:714`; `app/ponto/tests/test_dedup_retratada_c2.py:162`; `app/ponto/tests/test_dedup_retratada_c2.py:164`).

6. **Classes estruturais:** não há segundo escritor de `Batida` no helper: somente SELECT/comparação/payload; o escritor declarado permanece `registro_batida.py`. Há inferência paralela do resultado criação/reuso, pois a porta conhece seus próprios ramos e a casca reconstrói a resposta com snapshot. Entretanto não identifiquei juiz **registrado** específico para “criada neste toque”: o registro vivo de batida lista períodos, cálculo do par, geofence, selfie e GPS. Não equipararia isso automaticamente a clonagem de um desses juízes (`app/ponto/services/resposta_batida.py:10`; `app/ponto/services/resposta_batida.py:23`; `app/core/portas.py:103`; `app/ponto/registro_batida.py:113`; `app/ponto/registro_batida.py:120`; `app/ponto/registro_batida.py:157`; `app/core/juizes.py:1196`).

A mudança de sinalização para dupla viva merece reprovação por ampliação do comportamento previsto no item 3 do aval, que pede unificar o predicado de retratada e manter as janelas; a regra local exige escopo literal (`CLAUDE.md:37`; `app/ponto/services/triagem_batida.py:663`; `app/ponto/services/triagem_batida.py:669`). Impacto real em produção: **[nao verificado]**.

## 15/11 01:54 · sessao 26f67f1abef5 · - · - · origem=Ronald (tmux)

1. **Payload novo e dupla viva da API têm os mesmos nove campos e formato.** Ambos retornam `status`, `id`, `tipo`, `timestamp`, `sequencia`, `mensagem`, `sucesso`, `em_disputa`, `motivos_disputa`; ambos formatam o timestamp **do objeto**, com `localtime().isoformat()`, e mensagem com HH:MM. Na dupla, `tipo` solicitado coincide com o objeto pelo filtro; no helper, lê diretamente o tipo do objeto (`app/api/views.py:531`, `app/api/views.py:535`, `app/ponto/services/resposta_batida.py:27`, `app/ponto/services/resposta_batida.py:29`).

2. **O timestamp dessa resposta não ganha significado novo:** já era o instante da batida reutilizada na dupla de 5 minutos; no sucesso novo também sai `batida.timestamp`. Não equivale necessariamente ao instante solicitado. O formato local conserva a convenção declarada de UTC no banco e UTC-3 na apresentação; configuração usa São Paulo e `USE_TZ=True` (`app/api/views.py:539`, `app/api/views.py:817`, `CLAUDE.md:146`, `app/config/settings/base.py:93`).

3. **Nenhum dos campos do payload é interpretado pelo PWA online presente.** O template chama `/api/ponto/bater/`, verifica HTTP `r.ok` e chama `v2Sucesso(tipo, false)` com **tipo solicitado**, cujo texto anuncia Entrada/Saída registrada; nem lê JSON. Assim, payload corrigido não garante mensagem correta no PWA para reaproveitamento de outro tipo/retratada (`app/templates/ponto/bater_ponto_v2.html:310`, `app/templates/ponto/bater_ponto_v2.html:322`, `app/templates/ponto/bater_ponto_v2.html:327`, `app/templates/ponto/bater_ponto_v2.html:150`).

4. **Fila offline também não usa os campos.** Lê `resp.json()` para variável `data` sem consumo posterior e remove a pendência pelo HTTP `resp.ok`, usando o ID **local da pendência**, `b.id` (`app/static/pwa.js:158`, `app/static/pwa.js:159`, `app/static/pwa.js:162`).

5. **O template legado não interpreta JSON de triagem:** lê texto da resposta HTTP OK e faz `document.write`. Triagem já tinha resposta JSON mínima na dupla e passa a retornar JSON rico no reaproveitamento pós-porta (`app/templates/ponto/bater_ponto.html:533`, `app/templates/ponto/bater_ponto.html:539`, `app/ponto/services/triagem_batida.py:664`, `app/ponto/services/triagem_batida.py:724`).

6. **Contrato/consumo móvel de cada campo: [nao verificado].** Servidor do upload lê `batida_id` do request e busca batida do colaborador autenticado; isso não prova como o APK obtém aquele ID (`app/api/views.py:840`, `app/api/views.py:846`).

7. **Não há prova de duplicação de um juiz *registrado* para “foi criada neste toque”.** O registro da família batida declara período, cálculo, geofence, selfie e GPS; não essa pergunta. O helper compara identidade e campos e constrói resposta, sem selecionar tipo, criar Batida ou consultar relógio (`app/core/juizes.py:1196`, `app/core/juizes.py:1201`, `app/core/juizes.py:1205`, `app/core/juizes.py:1211`, `app/ponto/services/resposta_batida.py:10`, `app/ponto/services/resposta_batida.py:23`). A inferência de causalidade a partir do snapshot continua sendo derivação da operação de escrita, cuja correção concorrente deve ser examinada pelo responsável pelos ramos da porta (`app/ponto/services/resposta_batida.py:17`, `app/ponto/services/resposta_batida.py:23`).

## 15/11 01:51 · sessao 6c989965197c · BATIDA-DEDUP-RETRATADA · construtor · origem=Code

Curas e evidências registradas na [entrega c2](/home/ronald/wt-codex-batida-dedup-retratada-011759/app/ponto/tests/dedup_retratada_c2_entrega.txt:1).

- 20 testes locais verdes, incluindo C1 e paridade: [resultado](/home/ronald/wt-codex-batida-dedup-retratada-011759/app/ponto/tests/dedup_retratada_c2_green.txt:27). [nao verificado]
- 10 mutantes mortos: [evidências](/home/ronald/wt-codex-batida-dedup-retratada-011759/app/ponto/tests/dedup_retratada_c2_entrega.txt:86).
- Compilação e Ruff OK: [checks](/home/ronald/wt-codex-batida-dedup-retratada-011759/app/ponto/tests/dedup_retratada_c2_checks.txt:6).

Suite e certificação Code/opus: [nao verificado]. [nao verificado]

## 15/11 01:43 · sessao 13f3357d7635 · BATIDA-DEDUP-RETRATADA · leitura · origem=Code

**Há um mecanismo de arredondamento que reproduz 0,01 h: o mensal arredonda um subtotal e depois arredonda novamente ao acrescentar folga trabalhada sem escala certa. A lavra soma os valores completos e arredonda no fim. Não é arredondamento por dia.** (`app/ponto/services/fechamento.py:902–903,984–985`; `app/ponto/services/dia_pago.py:178,264,562–563`)

**Atribuir esse mecanismo a cada um dos 13 pares: [não verificado].** O anexo não traz os subtotais necessários para essa atribuição nem a implementação da sonda q4b. [não verificado]

1. **Quem escreve o mensal e como arredonda**

   O campo é `DecimalField(... decimal_places=2)`. O escritor de cálculo é `recalcular_fechamento_mes`; ele persiste por `save()` no ramo aberto e por `update()` no outro ramo. (`app/ponto/models.py:412`; `app/ponto/services/fechamento.py:448,1137–1144,1160–1162`)

   A sequência executável é:

   ```python
   # Primeiro: total dos períodos comuns + órfãos, em horas
   round(resultado.horas_trabalhadas + _min_orfaos / 60, 2)

   # Depois, se houver folga sem escala certa:
   round(float(fechamento.horas_trabalhadas) + _min_sem / 60, 2)
   ```

   Essas são as expressões de `app/ponto/services/fechamento.py:902–903,982–985`. `resultado.horas_trabalhadas` soma **minutos dos períodos e divide por 60**, sem arredondar cada período/dia nessa propriedade. (`app/ponto/motor_calculo_v2.py:294–296`)

   Portanto, são **duas casas de hora decimal**, com um arredondamento intermediário antes da parcela de folga sem escala certa. (`app/ponto/services/fechamento.py:902–903,984–985`)

   Existe também `restaurar_fechamento`: aceita o campo e atualiza o valor recebido, sem cálculo ou arredondamento explícito próprio. (`app/ponto/services/fechamento.py:1373–1380,1391–1393`)

2. **O que `soma_da_competencia` devolve**

   Filtra `DiaPago` por colaborador, mês, ano e versão — padrão `motor` —, retorna `None` sem lavratura e, havendo linhas, executa `Sum` de cada campo seguido de `round(float(total), 2)`. Para `horas_trabalhadas`, devolve um **float em horas, arredondado no total da competência**. (`app/ponto/services/dia_pago.py:550–563`)

   A lavra recebe minutos, divide por 60 e acumula sem arredondar as horas diárias: períodos comuns, folgas sem escala certa e órfãos entram pelos caminhos de `:178`, `:264` e `:271`; os valores seguem para `DiaPago` em `:340–347`. (`app/ponto/services/dia_pago.py:178,264,271,340–347`)

3. **Quem lê e o que apresenta**

   | Leitor | Número utilizado |
   |---|---|
   | Lista de fechamento | **Mensal gravado**, exibido com duas casas. (`app/ponto/views.py:1137–1154`; `app/ponto/services/fechamento_lista.py:80,105–107`; `app/templates/ponto/_fech_linha.html:30`) |
   | Relatório “Horas por colaborador”/CSV | **Mensal gravado**. (`app/relatorios/views.py:68–73,89–95`; `app/templates/relatorios/horas_mes.html:67`) |
   | Espelho e resumo do PDF | Preferem **fechamento derivado agora**, pela mesma fórmula mensal; se indisponível, usam **`soma_da_competencia`**. Não constituem um par fixo “mensal versus lavra”. (`app/relatorios/cartao_pela_celula.py:69–76,111,142–174,321–346`; `app/ponto/services/fechamento.py:1759–1761`) |
   | Formatação do espelho/PDF | Espelho mostra uma casa decimal; PDF converte para horas e minutos inteiros. Isso pode ocultar a diferença centesimal. (`app/templates/ponto/espelho.html:39`; `app/relatorios/pdf_espelho.py:667–678,766`) |
   | Extrato parcial | Soma **`DiaPago` por intervalo**, via `soma_do_periodo`, e mostra trabalhadas com duas casas. Sua janela precisa ser conferida antes da comparação com uma competência. (`app/relatorios/views.py:463–487,523`; `app/templates/relatorios/extrato_parcial.html:73`) |
   | TXT Domínio | O gerador recebe fechamentos **derivados em leitura**, mas a lista de eventos **não emite esse total T como rubrica de trabalhadas**; emite HE, folga, noturno, intra e atraso. (`app/folha/views.py:207–209,265–268`; `app/folha/export.py:439–461`) |
   | Comparador/vigia disponível | Lê **ambos**, mensal e soma lavrada, para produzir divergências. (`app/folha/porta_export.py:388,451–458`) |
   | Selo disponível | `selo_leitores_no_mesmo_numero` publica **contagens de divergências**; esse contador fica fora da reprovação. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:67,88–107`) |

   A identificação específica de **m2**, **“selo de silêncio”** e da sonda **SILENCIO-VENCIDO/q4b** com esses leitores permanece **[não verificado]**.

4. **A comparação arredonda igualmente?**

   No comparador localizado, a soma já chega arredondada; o mensal gravado tem duas casas. A decisão compara os `float`s **sem novo `round`**, usando `abs(mensal - soma) > 0.015`. Só depois arredonda ambos igualmente para o registro de saída. (`app/ponto/services/dia_pago.py:563`; `app/ponto/models.py:412`; `app/folha/porta_export.py:239,451–458`)

   **Logo, esse comparador não acusa uma diferença de exatamente 0,01 h.** A fórmula que fez a sonda q4b acusá-la é **[não verificado]**. (`app/folha/porta_export.py:239,455`)

5. **Mesmo cadastro e mesmas batidas podem resultar em números diferentes?**

   **Sim:** lista de fechamento versus extrato parcial, quando suas janelas contêm exatamente os mesmos dias pagos. Um lê o mensal; outro soma a lavra. (`app/ponto/views.py:1137–1154`; `app/relatorios/views.py:463–487,523`)

   Exemplo aritmético de três dias, usando as parcelas previstas no código: dois dias comuns de **482 minutos** cada e uma folga sem escala certa de **481 minutos**. (`app/ponto/services/fechamento.py:902–903,980–985`; `app/ponto/services/dia_pago.py:178,264`)

   ```text
   Mensal:
   round((482 + 482) / 60, 2) = 16,07
   round(16,07 + 481 / 60, 2) = 24,09 h

   Soma lavrada:
   round(482/60 + 482/60 + 481/60, 2) = 24,08 h

   Mensal − lavra = +0,01 h
   ```

   A reprodução decorre do **arredondamento do subtotal antes de acrescentar a última parcela**, pelas expressões de `app/ponto/services/fechamento.py:902–903,984–985`, contra o arredondamento final de `app/ponto/services/dia_pago.py:562–563`. A ocorrência dessas parcelas nos 13 casos reais permanece **[não verificado]**.

## 15/11 01:42 · sessao a186d9d5c966 · - · - · origem=Ronald (tmux)

Confirmado: há arredondamento intermediário do subtotal mensal, seguido de outro ao acrescentar folga sem escala certa; não há arredondamento diário nesse caminho. (`app/ponto/services/fechamento.py:902–903`, `app/ponto/services/fechamento.py:980–985`)

O exemplo solicitado reproduz **FM − soma = +0,01 h**:

| Peça | Minutos | Horas |
|---|---:|---:|
| Dia comum 1 | 482 | 8,033333… |
| Dia comum 2 | 482 | 8,033333… |
| Folga sem escala certa | 481 | 8,016666… |

As conversões correspondem aos caminhos de `DiaPago` em `app/ponto/services/dia_pago.py:178` e `app/ponto/services/dia_pago.py:264`.

- FM: `round((482+482)/60, 2) = 16,07`; depois `round(16,07 + 481/60, 2) = 24,09`. (`app/ponto/services/fechamento.py:902–903`, `app/ponto/services/fechamento.py:984–985`)
- Soma lavrada: `round(482/60 + 482/60 + 481/60, 2) = 24,08`. (`app/ponto/services/dia_pago.py:178`, `app/ponto/services/dia_pago.py:264`, `app/ponto/services/dia_pago.py:562–563`)

Verifiquei a conta com `python3 -B`, sem importar a aplicação e sem escrever arquivos; sua reprodução fica sustentada pelas expressões citadas acima.

Quanto a outros escritores: `restaurar_fechamento` aceita campos válidos do modelo, inclui `horas_trabalhadas` entre os possíveis, e os entrega diretamente a `.update(**_upd)`, sem recalcular nem acrescentar arredondamento explícito nessa função. (`app/ponto/services/fechamento.py:1373–1380`, `app/ponto/services/fechamento.py:1391–1393`)

A gravação ordinária persiste pelo `.save()` para status aberto ou pelo `.update(horas_trabalhadas=...)` no outro ramo. (`app/ponto/services/fechamento.py:1137–1144`, `app/ponto/services/fechamento.py:1160–1162`)

Não encontrei outra atribuição operacional explícita a esse campo fora dessas rotas na busca textual; exaustividade contra atribuições dinâmicas externas: [não verificado].

## 15/11 01:42 · sessao e3c7fb0de668 · - · - · origem=Ronald (tmux)

- O comparador disponível é `app/folha/porta_export.py::_medir_dia_pago`: chama `soma_da_competencia` em `:388`, lê cada campo do fechamento em `:451-452` e compara os valores convertidos em `float`, sem novo arredondamento, em `:455`. A soma já veio arredondada a duas casas (`app/ponto/services/dia_pago.py:559-563`).
- A tolerância é **0,015 h** (`app/folha/porta_export.py:239`). Portanto esse comparador **não acusa diferença de exatamente 0,01 h** (`app/folha/porta_export.py:455`). Só depois de decidir a divergência ele arredonda ambos os lados igualmente para o registro de saída (`app/folha/porta_export.py:456-458`).
- `linha_x_badge` também lê a soma (`app/folha/porta_export.py:267-268`), compara badge contra soma com a mesma tolerância (`:272-276`) e combina trabalhadas com folga trabalhada (`:245-248`).
- O selo concreto disponível chama `medir` e `lavrar` (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:88-92`), conta elementos das listas (`:106-107`) e imprime contagens (`:153-155`). O contador `fechamento_x_soma_dias_pagos` fica publicado, fora de `falhas` (`app/folha/porta_export.py:687-697`; rótulo explícito em `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:67`). Seus detalhes ficam em `MetricaSnapshot` (`app/folha/porta_export.py:772-782`).
- O TXT consulta o carimbo desse selo (`app/folha/views.py:128-133`); `conferir` lê `MetricaSnapshot` e testa seu valor (`app/folha/porta_export.py:807-821`). Não mostra outro total de trabalhadas nesse caminho (`app/folha/porta_export.py:800-821`).
- `e6_oraculo.dono_da_divergencia` classifica cadastro/batida/estrutura (`app/ponto/management/commands/e6_oraculo.py:98-110`); isso não demonstra a fórmula da comparação da sonda q4b. A identificação do código de **m2**, **selo de silêncio** e da **sonda específica SILENCIO-VENCIDO/q4b**, ausentes como fontes identificáveis na cópia pesquisada, permanece **[não verificado]**.

## 15/11 01:42 · sessao 54e5daabd0c0 · - · - · origem=Ronald (tmux)

Leitores encontrados, distinguindo código executável dos comentários antigos:

- **Lista de fechamento:** lê os `FechamentoMensal` salvos, passa-os a `montar_resumos`, que conserva o objeto em `r.fechamento`; a coluna mostra `horas_trabalhadas|floatformat:2`. Caminho: `app/ponto/views.py:1137-1154`, `app/ponto/services/fechamento_lista.py:80`, `app/ponto/services/fechamento_lista.py:105-107`, `app/templates/ponto/_fech_linha.html:30`.
- **Relatório “Horas por colaborador” e CSV:** usa diretamente o FM gravado; HTML com duas casas e CSV com o próprio Decimal. `app/relatorios/views.py:68-73`, `app/relatorios/views.py:89-95`, `app/templates/relatorios/horas_mes.html:67`.
- **Painel individual de fechamento:** lê apenas o `pk` do gravado, mas o número vem de `fechamento_lido`; template com duas casas. `app/ponto/views.py:993-996`, `app/templates/ponto/partials/painel_fechamento.html:32`.
- **Espelho (tela/app) e resumo do PDF:** para competência identificada, passam por `folha_manda`, que primeiro chama `totais_da_folha`. Esta exige existência de FM, mas lê **`fechamento_lido` derivado agora**, não as horas do FM salvo. Mapeia `fe.horas_trabalhadas` a `total_trabalhadas`. `app/relatorios/cartao_pela_celula.py:69-76`, `app/relatorios/cartao_pela_celula.py:111`, `app/relatorios/cartao_pela_celula.py:321-346`. `fechamento_lido` chama o mesmo recálculo com `somente_leitura=True, retornar_objeto=True`: `app/ponto/services/fechamento.py:1759-1761`.
- **Fallback do espelho/PDF:** se não obtiver totais daquele fechamento, `_totais_da_lavratura` lê `soma_da_competencia` e mapeia sua `horas_trabalhadas` a `total_trabalhadas`; escolhido por `folha_manda`. `app/relatorios/cartao_pela_celula.py:142-146`, `app/relatorios/cartao_pela_celula.py:174`, `app/relatorios/cartao_pela_celula.py:340-346`.
- **Formatação desses resumos:** espelho exibe uma casa decimal; PDF converte a horas/minutos inteiros, com `round((horas-h)*60)`. Portanto nem toda diferença centesimal será visível nesses dois formatos. `app/templates/ponto/espelho.html:39`, `app/relatorios/pdf_espelho.py:766`, `app/relatorios/pdf_espelho.py:667-678`. Entradas de tela/PDF em `app/ponto/services/espelho.py:1283-1284` e `app/relatorios/pdf_espelho.py:628`.
- **Linhas diárias:** tela usa rubricas do `DiaPago` (trabalhadas com uma casa); PDF reaproveita o dict de dias da tela, e seu “Trabalhado” usa `pago_h`, que soma trabalhadas+folga+feriado pertinente, arredondado a duas casas no serviço e depois formatado por `_h`. Não confundir esse “pago” com `horas_trabalhadas` isolada. `app/ponto/services/espelho.py:805-813`, `app/ponto/services/dia_pago.py:898`, `app/templates/ponto/espelho.html:273-274`, `app/relatorios/pdf_espelho.py:596-598`, `app/relatorios/pdf_espelho.py:901-902`, `app/relatorios/pdf_espelho.py:981`.
- **Extrato parcial:** lê soma de `DiaPago` por intervalo (`soma_do_periodo`, não diretamente `soma_da_competencia`); arredonda o total de trabalhadas em duas casas e exibe duas casas, inclusive no CSV. Atenção: janela própria do mês civil expandido por 12h e limitado a agora; não presumir igualdade de universo com competência. `app/relatorios/views.py:463-471`, `app/relatorios/views.py:481-487`, `app/relatorios/views.py:523`, `app/templates/relatorios/extrato_parcial.html:73`, `app/relatorios/views.py:560-561`.
- **TXT Domínio:** o gerador atual recalcula objetos em leitura e os injeta na montagem; não confiar no comentário antigo de que todo chamador usa gravado. `app/folha/views.py:207-209`, `app/folha/views.py:265-268`. **Nenhum dos dois totais T é emitido como rubrica de trabalhadas**: a lista de eventos contém HE50, HE100, feriado, folga, noturno, intra e atraso. `app/folha/export.py:439-461`. Cada valor emitido vira centésimos com `int(round(float(valor)*100))`. `app/folha/export.py:34-45`. O classificador/prontidão lê `fech.horas_trabalhadas` para condições/retorno, mas isso não transforma T em linha TXT. `app/folha/export.py:831`, `app/folha/export.py:902`, `app/folha/export.py:1036`.

Par concreto de leitores de **fontes diferentes**: relatório/lista que lê FM salvo versus extrato parcial que soma DiaPago, desde que comparados sobre intervalo comum; já espelho e PDF compartilham o mesmo seletor de fonte, portanto atribuir a eles uma oposição fixa “FM versus soma diária” seria incorreto. Fontes acima: `app/relatorios/views.py:68`, `app/relatorios/views.py:487`, `app/relatorios/cartao_pela_celula.py:321-346`.

_5482 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
