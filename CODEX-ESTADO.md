# CODEX-ESTADO

> Ultima resposta processada: **15/11/2026 20:15** · publicado em **15/11/2026 20:18** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 15/11 20:15 · sessao 968c3403316b · CORTE-JUIZ-RESPONDIDA · sonda · origem=Code

universo=50359; total_erros=0; classificadas=50359; fatal=null. (`saída:1`)

Referências `saída:N`: numeração dos objetos JSON fornecidos, começando pelo resumo como linha 1.

**2. Divergências por família**

| Família | Total | Mudança ao adotar `I E T` |
|---|---:|---|
| A | 0 | Nenhuma neste universo. (`saída:2`) |
| B | 9626 | **SIM → NÃO** nos pares `I_sem_T`. (`saída:2,53–64`) |
| C | 9626 | **SIM → NÃO** nos pares `I_sem_T`. (`saída:2,65–76`) |
| D_strip | 0 | Nenhuma neste universo. (`saída:2`) |
| D_cru | 0 | Nenhuma neste universo. (`saída:2`) |
| D_SQL_sem_trim | 0 | Nenhuma neste universo. (`saída:2`) |
| A_sem_trim | 0 | Nenhuma neste universo. (`saída:2`) |

A exige instante e texto após trim; B aceita instante **ou** texto; C aceita somente instante. Esses predicados estão, respectivamente, em `app/chamados/juizes.py:1559`, `app/chamados/models.py:1907–1908` e `app/chamados/models.py:3641`.

As divergências de B e C recaem sobre os mesmos **9626** pares; não são estoques distintos a somar. (`saída:53–76`)

**3. Estoque vivo “instante sem texto”**

| Competência | Quantidade | Estado |
|---|---:|---|
| ABERTA | **2** | Vivas, não validadas, não materializadas; disputa aberta. (`saída:18`) |
| ENTREGUE | **141 = 111 + 29 + 1** | Todas vivas e não validadas; 111 não materializadas, 30 materializadas. (`saída:43,47,50`) |
| Indecidível | **0** | O único grupo de competência indecidível tem `I_T`, portanto não pertence ao estoque. (`saída:52`; quadro completo: `saída:3–52`) |

Total vivo: **143 = 2 + 141**. As contagens batem com o aval registrado: diferença **0 abertas, 0 entregues, 0 no total**. (`saída:18,43,47,50`; `app/docs/LEIS.md:151`)

Os PKs **38251/38252** constam do aval registrado, mas `saída:18` não identifica as perguntas: confirmação desses PKs **nesta execução** é **[nao verificado]**. (`app/docs/LEIS.md:151`)

**4. Pares ilegais “texto sem instante”**

**0** no quadro de estados. Não há ocorrência para discriminar por vida, validação, materialização ou competência: os grupos de `saída:3–52` somam **50359**, fechando o universo de `saída:1`, e nenhum apresenta `sem_I_T`. (`saída:1,3–52`)

## 15/11 20:14 · sessao 43d414010fcc · BUG-ENC-CELULAS-FORMA · censo · origem=Code

**1. O defeito ainda existe, mas a descrição do BACKLOG mistura dois efeitos.** `dias_encerrados` recebe `{(pk, data): célula}` e repassa o argumento original `celulas` ao pareador quando `turnos` não foi fornecido. O pareador espera `{data: célula}`. ([turnos.py:2132](/home/ronald/codex_ws/e84a167f4d704612866e392da6078adaed18f9d0/app/ponto/turnos.py:2132), `app/ponto/turnos.py:2150`, `app/ponto/turnos.py:1854`)

Na segmentação, uma chave `(pk, D)` recebida indevidamente vira `(pk, (pk, D))`; `vinculo_do_dia` procura `(pk, D)`, perde a célula e, portanto, sua geradora. Essa incompatibilidade permanece no HEAD da cópia. (`app/ponto/turnos.py:1956`, `app/ponto/turnos.py:1960`, `app/escala/alimentacao.py:269`)

O sítio antigo `cartorio.py:910` corresponde atualmente a **`:1154`**: continua passando células compostas de `d0..d1`, **sem `turnos`**. Os limites vêm das células do julgamento. ([cartorio.py:1154](/home/ronald/codex_ws/e84a167f4d704612866e392da6078adaed18f9d0/app/ponto/services/cartorio.py:1154), `app/ponto/services/cartorio.py:1131`)

A afirmação “o papel da ata não vê as células por causa da chave composta” não se sustenta: o papel percorre os objetos e entrega cada célula a `periodos_do_dia`, que lê `ata`, `dna` e `celula.data`. Há, entretanto, falta das células externas ao julgamento: o pareador lê `ini−2..fim+1`, e um mapa fornecido suprime sua carga automática. São efeitos distintos. (`app/ponto/turnos.py:1631`, `app/ponto/turnos.py:1633`, `app/ponto/juiz_batida.py:157`, `app/ponto/juiz_batida.py:175`, `app/ponto/turnos.py:1847`, `app/ponto/turnos.py:1884`)

**2. Escritores, leitores e semântica das formas.**

| Sítio | Produção/leitura e forma |
|---|---|
| `celulas_do_periodo` | Produz `{(pk,data): célula}`, com intervalo inclusivo. (`app/escala/alimentacao.py:55`, `app/escala/alimentacao.py:73`) |
| `dias_encerrados` | Recebe composto; mantém esse mapa para o encerramento, mas o repassa indevidamente ao pareador. Retorna `{data: bool}` ou motivos. (`app/ponto/turnos.py:2132`, `app/ponto/turnos.py:2134`, `app/ponto/turnos.py:2147`, `app/ponto/turnos.py:2150`) |
| `_dias_encerrados_puro` | Agrupa turnos por `data_turno`; entrega composto corretamente ao vínculo e ao fim previsto. (`app/ponto/turnos.py:2162`, `app/ponto/turnos.py:2166`) |
| `turnos_do_colab` | Espera `{data: célula}` da janela com pad. Quando carrega sozinho, converte composto para data corretamente. (`app/ponto/turnos.py:1854`, `app/ponto/turnos.py:1886`) |
| Candidatura de vínculos | Lê `celulas.values()` para obter geradoras; esse leitor independe da chave. (`app/ponto/turnos.py:1888`) |
| Segmentação | Supõe chaves data, acrescenta `pk` e consulta o vínculo diário; é onde o composto indevido perde a geradora. (`app/ponto/turnos.py:1956`, `app/ponto/turnos.py:1960`) |
| `vinculo_do_dia` | Espera composto. A geradora nomeada vence dentro da vigência, inclusive inativa; sem célula encontrada, usa os vínculos que cobrem a data. (`app/escala/alimentacao.py:269`, `app/escala/alimentacao.py:279`, `app/escala/alimentacao.py:282`, `app/escala/alimentacao.py:285`) |
| Papel da ata | Percorre células, lê seus objetos e produz mapa por instante local truncado no minuto. Não faz busca por `celulas.get(data)`. (`app/ponto/turnos.py:1631`, `app/ponto/turnos.py:1633`, `app/ponto/turnos.py:904`, `app/ponto/turnos.py:905`) |
| DNA/fim previsto | `marcos_dna_periodo` e `EscalaColaborador.marcos_do_dia` esperam composto. O primeiro produz mapa por data com os marcos. (`app/escala/utils.py:90`, `app/escala/utils.py:97`, `app/escala/models.py:1120`) |
| Cartório | Constrói composto tanto para cadastro quanto para encerramento. O cadastro direto alimenta corretamente `vinculo_do_dia`; o encerramento alcança o repasse defeituoso. (`app/ponto/services/cartorio.py:1143`, `app/ponto/services/cartorio.py:1154`, `app/ponto/supra_juiz.py:174`) |
| Singular | `dia_encerrado` delega a `dia_encerrado_motivo`; este passa `celulas` somente ao pareador. Vínculo e fim previsto não recebem essa alimentação e seguem sua leitura própria. (`app/ponto/turnos.py:2077`, `app/ponto/turnos.py:2098`, `app/ponto/turnos.py:2099`) |
| Vigia | Produz composto corretamente para `escala_vigente`, mas entrega composto indevidamente à singular, cujo destino é o pareador por data. (`app/ponto/services/vigia_de_hora.py:47`, `app/ponto/services/vigia_de_hora.py:58`, `app/ponto/turnos.py:2098`) |
| Fechamento | Cliente singular correto: constrói `{data: célula}` recortado pelo pad de cada dia. (`app/ponto/services/fechamento.py:350`, `app/ponto/services/fechamento.py:370`, `app/ponto/services/fechamento.py:377`) |
| `CargaDaJanela` | Agrupa por colaborador/data; fornece `{data}` ao pareador e composto ao encerramento, usando seus turnos já calculados. (`app/ponto/turnos.py:2983`, `app/ponto/turnos.py:3038`, `app/ponto/turnos.py:3045`) |
| Motor | Recebe composto; conserva-o para DNA e períodos, convertendo para data ao alimentar o papel. (`app/ponto/motor_calculo_v2.py:473`, `app/ponto/motor_calculo_v2.py:498`, `app/ponto/motor_calculo_v2.py:612`) |

Os outros produtores explícitos para o pareador são a montagem da grade, que entrega `{data}` com ata em memória, e o comando retroativo, que entrega `{data}` carregado de `ini−2..fim+2`. (`app/escala/utils.py:1581`, `app/escala/utils.py:1880`, `app/escala/utils.py:1898`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:86`)

**Data da chave e significado do dia são questões diferentes:** a célula representa o dia do turno; marcos podem alcançar D+1. O pareador consulta o mapa de DNA pela **data civil da batida** para medir distâncias, mas filtra e entrega resultados por **`data_turno`**; o encerramento também agrupa por esse dia. (`app/ponto/turnos.py:2940`, `app/escala/servico_jornada.py:89`, `app/ponto/turnos.py:1231`, `app/ponto/turnos.py:1239`, `app/ponto/turnos.py:1995`, `app/ponto/turnos.py:2162`)

Quanto aos dados persistidos, geração/regeneração e edição manual escrevem trabalho, geradora e DNA; a porta de lavra escreve ata, protestos, veredito, via, instante e status. (`app/ponto/portas/celula.py:289`, `app/ponto/portas/celula.py:306`, `app/ponto/portas/celula.py:638`, `app/ponto/portas/celula.py:1091`, `app/ponto/portas/celula.py:1096`)

**3. Ref, hunks e histórico.**

A cópia disponível não fornece história Git consultável. Existência dos objetos da ref, os 30 commits mencionados, aplicação limpa dos commits e equivalência por outro hash no main: **[nao verificado]**. O diff cumulativo fornecido também não permite atribuir cada hunk individualmente a `6bedd35d4` ou `a2cd7981d`: **[nao verificado]**.

| Hunk fornecido | Situação frente à cópia |
|---|---|
| Alteração de `dias_encerrados` | **Ainda casa textualmente**, deslocada duas linhas: contexto e chamada a remover continuam presentes. Não ficou obsoleto nesse sítio. Aplicação Git efetiva: **[nao verificado]**. (`app/ponto/turnos.py:2147`, `app/ponto/turnos.py:2150`) |
| Novo `test_bug_enc_celulas_forma.py` | Destino não localizado na cópia; conflito ou aplicação Git: **[nao verificado]**. |
| Novo `test_e4_carga_paridade.py` | Destino não localizado na cópia; conflito ou aplicação Git: **[nao verificado]**. |

Documentalmente, **E4-CARGA-PARIDADE é obra separada, anterior ao BUG-ENC**; ambas continuam registradas em linhas próprias. Que `78403ab2b` já esteja no main sob outro hash: **[nao verificado]**. (`app/docs/BACKLOG.md:988`, `app/docs/BACKLOG.md:989`)

Há registro posterior confirmando novamente os dois repasses compostos, plural e singular; isso confirma o recenso documental, sem identificar um commit posterior responsável. (`app/docs/RELATO.md:578`)

**4. Conjunto de arquivos, preservados os contratos atuais.**

| Arquivo | Papel no perímetro mínimo |
|---|---|
| `app/ponto/turnos.py` | Contém o repasse plural incompatível e a fronteira do pareador onde o tripwire proposto se aplica. (`app/ponto/turnos.py:2150`, `app/ponto/turnos.py:1854`, `app/ponto/turnos.py:1956`) |
| `app/ponto/services/vigia_de_hora.py` | Contém o produtor composto indevido da via singular; o hunk plural fornecido não o alcança. (`app/ponto/services/vigia_de_hora.py:58`, `app/ponto/turnos.py:2098`) |
| `app/ponto/tests/test_bug_enc_celulas_forma.py` | Testes propostos de plural, vigia e contrato; conteúdo fornecido no anexo, presença na cópia **[nao verificado]**. |
| `app/ponto/tests/test_e4_carga_paridade.py` | Dependência da fixture importada pelo teste BUG; obra separada no BACKLOG. Presença na cópia **[nao verificado]**. (`app/docs/BACKLOG.md:989`) |

**O defeito de forma não exige alteração de `cartorio.py`: ele fornece exatamente o composto documentado pela porta plural.** Tampouco foi demonstrada necessidade de alterar `chamados/*` para corrigir esse contrato. Uma exigência adicional de versionamento/alcance no cartório para esta obra é **[nao verificado]**. (`app/ponto/services/cartorio.py:1154`, `app/ponto/turnos.py:2132`)

O teste existente de C1 precisa permanecer como proteção do contrato singular por data e do DNA divergente; ele os documenta e compara explicitamente. Necessidade de editá-lo: **[nao verificado]**. (`app/ponto/tests/test_c1_dias_encerrados.py:54`, `app/ponto/tests/test_c1_dias_encerrados.py:73`)

**5. Consumidores e alcance até dinheiro/exportação.**

| Resultado | Caminho comprovado |
|---|---|
| Encerramento → julgamento | Alimenta a impressão `enc=0` e `classificar_dia(turno_encerrado=...)`. (`app/ponto/services/cartorio.py:1397`, `app/ponto/services/cartorio.py:1474`) |
| Julgamento → persistência | Cartório entrega veredito e ata à mesma lavra; a porta grava ambos e aplica o status. (`app/ponto/services/cartorio.py:1851`, `app/ponto/services/cartorio.py:1853`, `app/ponto/portas/celula.py:1091`, `app/ponto/portas/celula.py:1096`) |
| Ata/veredito → leitores | Grade reconstrói minutos, órfãs e veredito; prévia lê ata e lâmpadas. (`app/escala/services/leitor_celula.py:413`, `app/escala/services/leitor_celula.py:423`, `app/escala/services/leitor_celula.py:433`, `app/folha/previa.py:83`) |
| Ata/papel → motor | Fechamento alimenta as células; motor deriva papel e o fornece ao pareador. Outro ramo consulta diretamente `periodos_do_dia`. **Sim, `motor_calculo_v2` consome.** (`app/ponto/services/fechamento.py:750`, `app/ponto/motor_calculo_v2.py:498`, `app/ponto/motor_calculo_v2.py:506`, `app/ponto/motor_calculo_v2.py:616`) |
| Veredito → TXT | Retenção lê vereditos e protestos; `classificar_export` pode excluir pelo motivo `furo_espelho`. (`app/folha/export.py:1273`, `app/folha/export.py:1286`, `app/folha/export.py:1293`, `app/folha/export.py:968`) |
| Encerramento → furo financeiro | Fechamento filtra os furos produzidos pelo motor antes da lavra de `DiaPago`; esse cliente fornece corretamente `{data}` com pad. (`app/ponto/services/fechamento.py:377`, `app/ponto/services/fechamento.py:1115`, `app/ponto/services/fechamento.py:1293`) |

Portanto, **há consumidores até cálculo e elegibilidade do TXT**; isso não demonstra alteração monetária causada por este bug em produção. Pessoas atingidas, valores e competência exportada efetivamente alterada: **[nao verificado]**. (`app/ponto/motor_calculo_v2.py:506`, `app/folha/export.py:970`)

“Já exportada” também não constitui bloqueio absoluto no código atual: o recálculo documenta a retirada dessa recusa, preservando trilha e versão anterior do TXT. A lei aplicável inclui L-113, além de L-092 e do horizonte L-124. (`app/ponto/services/fechamento.py:485`, `app/ponto/services/fechamento.py:493`, `app/docs/LEIS.md:155`, `app/docs/LEIS.md:125`, `app/docs/LEIS.md:145`)

**6. Leis e cortes encontrados.**

| Proteção | Referência |
|---|---|
| **L-002** — uma autoridade; testemunha lê | `app/docs/LEIS.md:60` |
| **L-020** — célula e DNA congelado | `app/docs/LEIS.md:71` |
| **L-025** — universo de batidas apuráveis | `app/docs/LEIS.md:76` |
| **L-032** — intervalo cadastrado | `app/docs/LEIS.md:80` |
| **L-061** — consumidor lê célula/ata | `app/docs/LEIS.md:95` |
| **L-073** — teto por instante/fato encerrado | `app/docs/LEIS.md:100` |
| **L-085** — jornada pertence ao dia de início | `app/docs/LEIS.md:117` |
| **L-087** — papel pela ata contra DNA | `app/docs/LEIS.md:119` |
| **L-102/L-103** — sem par: apresentação e zero declarado no fechamento | `app/docs/LEIS.md:128`, `app/docs/LEIS.md:129` |
| **L-110** — teste pela regra, sem código antigo como gabarito | `app/docs/LEIS.md:136` |
| **L-113/L-124** — exportada e horizonte | `app/docs/LEIS.md:155`, `app/docs/LEIS.md:145` |

Os cortes de nascimento/correção de `dia_encerrado` exigem fato encerrado, nunca simplesmente `data < hoje`. O corte do juiz do cálculo protege o motor; o de LAUDOS-[nome] exige ata e motor lendo o mesmo turno, mas pertence a outra obra. (`app/docs/CORTES.md:55`, `app/docs/CORTES.md:62`, `app/docs/CORTES.md:63`, `app/docs/CORTES.md:405`)

Corte nominal específico de BUG-ENC-CELULAS-FORMA ou E4-CARGA-PARIDADE: **[nao verificado]**.

**7. O RED continua discriminante por leitura, com ressalva sobre o artefato raspado.**

Para a fixture lógica X/Y descrita no anexo, a chave aninhada ainda provoca seleção dos marcos do ativo. Em X, a pausa desse ativo absorve S07; em Y, o ativo sem pausa permite fechar na S00. O lote recebe a forma correta e não sofre essa perda da geradora. (`app/ponto/turnos.py:1956`, `app/ponto/turnos.py:1984`, `app/ponto/turnos.py:1356`, `app/ponto/turnos.py:1492`, `app/ponto/turnos.py:3038`)

| Caso às D+1 08h | Previsão por leitura |
|---|---|
| X, plural alimentado como cartório | Continua aberto até a expiração: `turno_aberto_relogio`; pelo vínculo correto, já estaria encerrado. (`app/ponto/turnos.py:1356`, `app/ponto/turnos.py:1171`, `app/ponto/turnos.py:2030`, `app/ponto/turnos.py:2116`) |
| Y, plural alimentado como cartório | Fecha indevidamente na S00; pelo vínculo correto, a saída é de intervalo e a aberta permanece viva até 11h. (`app/ponto/turnos.py:1984`, `app/ponto/turnos.py:1492`, `app/ponto/turnos.py:2030`) |
| Vigia X/Y | Continua alcançando o mesmo repasse incompatível pela singular. (`app/ponto/services/vigia_de_hora.py:58`, `app/ponto/turnos.py:2098`) |
| `assertRaises(TypeError)` | Continua discriminante: o contrato é documentado, mas não há validação de chaves; elas seguem até o embrulho silencioso. Exceção observada em execução: **[nao verificado]**. (`app/ponto/turnos.py:1854`, `app/ponto/turnos.py:1884`, `app/ponto/turnos.py:1956`) |

O teste C1 existente não substitui esse cenário: sua célula tem DNA divergente, sem nomear geradora, e compara o plural composto com a singular por data. (`app/ponto/tests/test_c1_dias_encerrados.py:62`, `app/ponto/tests/test_c1_dias_encerrados.py:66`, `app/ponto/tests/test_c1_dias_encerrados.py:73`)

Execução literal do anexo, importação da fixture e resultados efetivos permanecem **[nao verificado]**. Os CPFs raspados não podem ser tratados como os dados originais da fixture: o modelo exige unicidade e normaliza CPF no `save`. (`app/colaboradores/models.py:319`, `app/colaboradores/models.py:418`)

## 15/11 20:12 · sessao fb73725634cf · - · - · origem=Ronald (tmux)

O defeito permanece: `dias_encerrados` documenta mapa `{(pk,data): célula}` e o repassa sem conversão ao pareador, que documenta `{data: célula}`; a segmentação acrescenta outro `pk`, impedindo a busca composta da geradora. (`app/ponto/turnos.py:2132`, `app/ponto/turnos.py:2150`, `app/ponto/turnos.py:1854`, `app/ponto/turnos.py:1956`, `app/escala/alimentacao.py:269`)

| Produtor/leitor | Forma e semântica |
|---|---|
| `celulas_do_periodo` | Produz composto, intervalo inclusivo. (`app/escala/alimentacao.py:55`, `app/escala/alimentacao.py:73`) |
| `dias_encerrados` | Recebe composto; reaproveita-o no encerramento e passa-o incorretamente ao pareador quando `turnos=None`. (`app/ponto/turnos.py:2147`, `app/ponto/turnos.py:2150`) |
| `_dias_encerrados_puro` | Agrupa por `data_turno`; fornece composto ao vínculo e ao fim previsto. (`app/ponto/turnos.py:2162`, `app/ponto/turnos.py:2166`) |
| `turnos_do_colab` | Contrato `{data}` na janela `ini−2..fim+1`; converte corretamente quando carrega sozinho. (`app/ponto/turnos.py:1847`, `app/ponto/turnos.py:1854`, `app/ponto/turnos.py:1884`) |
| Candidatos | Lê valores/geradoras, independente da chave. (`app/ponto/turnos.py:1888`) |
| Segmentação | Espera chave data, transforma para composto e pergunta ao vínculo. (`app/ponto/turnos.py:1956`, `app/ponto/turnos.py:1960`) |
| Papel da ata | Itera objetos, entrega cada célula a `periodos_do_dia`; composto não o cega. A chave apenas endereça balde interno. (`app/ponto/turnos.py:1631`, `app/ponto/turnos.py:1633`, `app/ponto/turnos.py:1704`, `app/ponto/turnos.py:1731`) |
| Juiz do papel | Lê ata, DNA e **`celula.data`**; produz instantes dos marcos, com saída possivelmente em D+1. (`app/ponto/juiz_batida.py:157`, `app/ponto/juiz_batida.py:175`, `app/escala/servico_jornada.py:89`) |
| Leitor do papel | Busca instante local truncado no minuto, não data isolada. (`app/ponto/turnos.py:904`, `app/ponto/turnos.py:905`) |
| `vinculo_do_dia` | Composto; geradora vence dentro da vigência, inclusive inativa; sem célula, recorre ao vínculo que cobre a data. (`app/escala/alimentacao.py:269`, `app/escala/alimentacao.py:279`, `app/escala/alimentacao.py:282`, `app/escala/alimentacao.py:285`) |
| Singular | `dia_encerrado_motivo` passa `celulas` somente ao pareador; vínculo e fim previsto não recebem essa alimentação. (`app/ponto/turnos.py:2098`, `app/ponto/turnos.py:2099`) |
| Vigia | Constrói composto corretamente para `escala_vigente`, mas entrega composto incorretamente à singular. (`app/ponto/services/vigia_de_hora.py:47`, `app/ponto/services/vigia_de_hora.py:58`) |
| Fechamento | Contrato singular `{data}`, recortado para o pad de cada dia: cliente correto atual. (`app/ponto/services/fechamento.py:350`, `app/ponto/services/fechamento.py:370`, `app/ponto/services/fechamento.py:377`) |
| Carga | Agrupa células por colaborador/data; entrega `{data}` ao pareador e composto ao encerramento. (`app/ponto/turnos.py:2983`, `app/ponto/turnos.py:3038`, `app/ponto/turnos.py:3045`) |
| Motor | Recebe composto; converte para data antes do papel; mantém composto para DNA e juiz dos períodos. (`app/ponto/motor_calculo_v2.py:473`, `app/ponto/motor_calculo_v2.py:498`, `app/ponto/motor_calculo_v2.py:612`) |

No pareador, o mapa DNA é consultado pela **data civil da batida** para distâncias; a atribuição final e o encerramento usam **`data_turno`**. Os marcos do turno são ancorados no dia de entrada e encadeados, avançando o dia quando necessário. (`app/ponto/turnos.py:1231`, `app/ponto/turnos.py:1239`, `app/ponto/turnos.py:1995`, `app/ponto/turnos.py:2162`, `app/escala/servico_jornada.py:89`, `app/escala/servico_jornada.py:97`)

Inventário dos clientes explícitos atuais:

- Pareador em produção: singular e plural em `turnos.py`; montagem da grade com `{data}` e ata em memória; comando retroativo com `{data}` e intervalo `ini−2..fim+2`. (`app/ponto/turnos.py:2098`, `app/ponto/turnos.py:2150`, `app/escala/utils.py:1880`, `app/escala/utils.py:1898`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:86`)
- Singular: fechamento correto `{data}` com pad; vigia incorreto composto. (`app/ponto/services/fechamento.py:370`, `app/ponto/services/fechamento.py:377`, `app/ponto/services/vigia_de_hora.py:58`)
- Plural: cartório fornece composto e omite `turnos`. (`app/ponto/services/cartorio.py:1154`)
- Testes explícitos: C1 plural composto e singular por data; O142 repassa argumento do helper, incluindo `None`/`{}` conforme o caso; saída órfã passa `{DIA:cel}`. (`app/ponto/tests/test_c1_dias_encerrados.py:66`, `app/ponto/tests/test_c1_dias_encerrados.py:73`, `app/ponto/tests/test_o142_turno_invariante_na_janela.py:161`, `app/ponto/tests/test_o142_turno_invariante_na_janela.py:169`, `app/ponto/tests/test_o142_turno_invariante_na_janela.py:203`, `app/escala/tests/test_saida_valida_vira_orfa.py:158`)

Os dados persistidos têm escritores concretos: geração/regeneração monta DNA e grava trabalho/geradora; edição manual também grava esses campos. A lavra escreve ata, protestos, veredito, via, instante e status; `carimbar` persiste os insumos junto dos campos lavrados. (`app/ponto/portas/celula.py:289`, `app/ponto/portas/celula.py:306`, `app/ponto/portas/celula.py:638`, `app/ponto/portas/celula.py:1091`, `app/ponto/portas/celula.py:1096`, `app/ponto/portas/celula.py:1199`)

Por leitura, o RED conceitual ainda distingue X e Y: com chave composta a segmentação escolhe o ativo; em X, intervalo ativo absorve S07 e permanece aberto porque não existe batida posterior; em Y, ativo sem intervalo permite fechar na S00. Pelo vínculo correto, X fecha e Y fica aberto até fim+4h. (`app/ponto/turnos.py:1956`, `app/ponto/turnos.py:1984`, `app/ponto/turnos.py:1356`, `app/ponto/turnos.py:1171`, `app/ponto/turnos.py:1492`, `app/ponto/turnos.py:2030`, `app/ponto/turnos.py:2116`)

O tripwire de `TypeError` também permanece pertinente: a casca não valida o formato antes de carregar e executar; composto segue silenciosamente até a transformação defeituosa. (`app/ponto/turnos.py:1850`, `app/ponto/turnos.py:1884`, `app/ponto/turnos.py:1915`, `app/ponto/turnos.py:1956`)

Execução e resultados efetivos dos testes anexos: [nao verificado]. [nao verificado]

## 15/11 20:11 · sessao 99b31c068b65 · - · - · origem=Ronald (tmux)

O cartório ainda chama a porta vulnerável sem `turnos`: `_julgar_colab_corpo` extrai `d0,d1` das células em `app/ponto/services/cartorio.py:1131`; `_turno_encerrado` passa o mapa composto `{(c.pk,x.data):x}` em `app/ponto/services/cartorio.py:1154`, sem expandir o pad nem fornecer turnos. A antiga referência `:910` está deslocada; o sítio atual é `:1154`. (`app/ponto/services/cartorio.py:1151`)

O resultado chega a dois usos distintos: marca `enc=0` na impressão do dia passado ainda em curso e alimenta `classificar_dia(turno_encerrado=...)`; os códigos derivados chegam à lavra de veredito e ata. (`app/ponto/services/cartorio.py:1397`, `app/ponto/services/cartorio.py:1399`, `app/ponto/services/cartorio.py:1464`, `app/ponto/services/cartorio.py:1474`, `app/ponto/services/cartorio.py:1853`) O cadastro direto continua correto: carrega todos os vínculos e entrega mapa composto a `cadastro_do_dia`, cujo corpo chama `vinculo_do_dia` com essa alimentação. (`app/ponto/services/cartorio.py:1143`, `app/ponto/services/cartorio.py:1145`, `app/ponto/supra_juiz.py:174`)

Consumidores e alcance:

| Dado | Produção e consumo |
|---|---|
| Ata/veredito | Cartório monta ata e pede lavra conjunta; a porta atribui `cel.ata`, `protestos`, `veredito`, via e instante. (`app/ponto/services/cartorio.py:1851`, `app/ponto/services/cartorio.py:1853`, `app/ponto/portas/celula.py:1091`, `app/ponto/portas/celula.py:1092`, `app/ponto/portas/celula.py:1093`) |
| Status do dia | A porta aplica o status com o mesmo veredito; D4 lê a coluna após o encerramento. (`app/ponto/portas/celula.py:1016`, `app/ponto/portas/celula.py:1021`, `app/ponto/status_dia.py:205`, `app/ponto/status_dia.py:211`) |
| Ata → grade | O leitor reconstrói células/marcos, órfãs, minutos realizados e veredito, transportando cobertura parcial. (`app/escala/services/leitor_celula.py:413`, `app/escala/services/leitor_celula.py:420`, `app/escala/services/leitor_celula.py:423`, `app/escala/services/leitor_celula.py:424`, `app/escala/services/leitor_celula.py:433`) |
| Papel | `periodos_do_dia` lê o objeto `celula.ata`, `celula.dna` e ancora os marcos em `celula.data`; não depende de buscar a célula por chave no mapa recebido pelo papel. (`app/ponto/juiz_batida.py:157`, `app/ponto/juiz_batida.py:158`, `app/ponto/juiz_batida.py:175`) |
| Motor | Fechamento fornece `motor.celulas_alimentadas`; o motor espera composto, converte para `{data:cel}` ao alimentar o papel e passa o papel a `parear_turnos`. Outro ramo busca `cels.get((cid,dia))` e consulta `periodos_do_dia`. (`app/ponto/services/fechamento.py:750`, `app/ponto/motor_calculo_v2.py:498`, `app/ponto/motor_calculo_v2.py:500`, `app/ponto/motor_calculo_v2.py:506`, `app/ponto/motor_calculo_v2.py:612`, `app/ponto/motor_calculo_v2.py:616`) |
| Prévia | Lê `cel.ata`, percorre lâmpadas e apresenta o veredito. (`app/folha/previa.py:83`, `app/folha/previa.py:84`, `app/folha/previa.py:94`) |
| Exportação | Retenção lê `CelulaDia.veredito` e protestos; `classificar_export` coloca o colaborador fora pelo motivo `furo_espelho`. (`app/folha/export.py:1273`, `app/folha/export.py:1286`, `app/folha/export.py:1293`, `app/folha/export.py:1294`, `app/folha/export.py:968`, `app/folha/export.py:970`) |
| Furo no dinheiro | Fechamento usa `dia_encerrado` para filtrar os furos saídos do motor; aqui fornece corretamente mapa por data recortado com pad. A lista filtrada é entregue à lavra de `DiaPago`. (`app/ponto/services/fechamento.py:350`, `app/ponto/services/fechamento.py:370`, `app/ponto/services/fechamento.py:373`, `app/ponto/services/fechamento.py:377`, `app/ponto/services/fechamento.py:1115`, `app/ponto/services/fechamento.py:1293`) |

Assim, há caminhos de consumo até cálculo monetário e elegibilidade do TXT; ocorrência concreta, valores, pessoas e alteração de competência exportada por este defeito: **[nao verificado]**. O caminho do fechamento acima não reproduz a forma composta defeituosa: ele documenta e constrói `{data:cel}` com pad. (`app/ponto/services/fechamento.py:350`, `app/ponto/services/fechamento.py:371`)

A regra atual do cartório distingue alcance por horizonte: `REGRA_VERSOES` declara versões/horizontes; `versao_para` usa a competência do juiz `janela_atual` e devolve a maior versão aplicável. Ela não consulta estado exportado ou trancado. Isso é relevante para qualquer futura discussão de alcance, sem afirmar que esta obra precise alterar essa declaração. (`app/ponto/services/cartorio.py:142`, `app/ponto/services/cartorio.py:258`, `app/ponto/services/cartorio.py:262`, `app/ponto/services/cartorio.py:273`, `app/ponto/services/cartorio.py:276`)

Leis e cortes encontrados:

- **L-025**: universo de apuração em `batidas_apuraveis`; **L-032**: intervalo cadastrado; **L-061**: consumidor lê a lâmpada; **L-073**: teto por instante ou fato encerrado. (`app/docs/LEIS.md:76`, `app/docs/LEIS.md:80`, `app/docs/LEIS.md:95`, `app/docs/LEIS.md:100`)
- **L-085**: jornada pertence ao dia de início; **L-087**: papel vem da ata contra DNA, instante vem da batida. (`app/docs/LEIS.md:117`, `app/docs/LEIS.md:119`)
- **L-102/L-103**: sem turno pareado, palavra sem número na apresentação e zero declarado no fechamento. (`app/docs/LEIS.md:128`, `app/docs/LEIS.md:129`)
- **L-110**: regra é referência, código antigo não é gabarito; **L-124**: horizonte para frente. (`app/docs/LEIS.md:136`, `app/docs/LEIS.md:145`)
- Exportadas exigem ler a alteração normativa: **L-113** revoga a recusa de recálculo e pauta como pré-condição; o texto antigo de **L-092** sozinho é insuficiente. (`app/docs/LEIS.md:125`, `app/docs/LEIS.md:155`)
- `JUIZ-DIA-ENCERRADO-NASCE` e `CHAMADO-X-JUIZ-CORRIGIDO`: nome autorizado e critério corrigido para fato encerrado, nunca `data < hoje`; registro exige diff do universo dos consumidores que retratam. (`app/docs/CORTES.md:55`, `app/docs/CORTES.md:62`)
- `JUIZ-CALCULAR-PERIODO-NASCE`: declara autoridade pelo par e proíbe tocar o motor nessa fatia. (`app/docs/CORTES.md:63`)
- `LAUDOS-[nome]-2810`: um juiz do par e ata lendo o mesmo turno do motor; é corte de outra obra, não autorização nominal desta. (`app/docs/CORTES.md:405`)

Corte nominal `BUG-ENC-CELULAS-FORMA` ou `E4-CARGA-PARIDADE` em `CORTES.md`: **[nao verificado]**. Não localizei `AGENTS.md` no inventário `rg --files` da cópia; instruções adicionais fora da cópia: **[nao verificado]**.

## 15/11 20:11 · sessao 3cf37e6a7370 · - · - · origem=Ronald (tmux)

1. **História Git e atribuição dos hunks:** a cópia não contém `.git`; o comando Git retorna “not a git repository”. Aplicação limpa da ref, existência dos três commits, equivalência por patch-id/cherry-pick e commits posteriores responsáveis por mudanças: **[nao verificado]**. O diff fornecido é cumulativo; atribuir cada hunk especificamente a `6bedd35d4` ou `a2cd7981d`: **[nao verificado]**.

2. **Hunks fornecidos contra a cópia:**

| Hunk | Resultado por leitura |
|---|---|
| `turnos.py`, alteração de `dias_encerrados` | **Ainda casa textualmente**, deslocado +2 linhas: `cels`, consulta de escalas e retorno permanecem em `app/ponto/turnos.py:2147`; a linha a remover permanece em `app/ponto/turnos.py:2150`. Contexto não ficou obsoleto. Aplicação Git efetiva: **[nao verificado]**. |
| Novo `test_bug_enc_celulas_forma.py` | Arquivo não localizado na cópia; nenhum hunk preexistente para conflitar. Aplicação Git/estado na ref: **[nao verificado]**. |
| Novo `test_e4_carga_paridade.py` | Arquivo não localizado na cópia; nenhum hunk preexistente para conflitar. Aplicação Git/estado na ref: **[nao verificado]**. |

3. **E4-CARGA-PARIDADE:** documentalmente é obra separada e anterior ao BUG-ENC, ainda registrada como fila própria (`app/docs/BACKLOG.md:989`); BUG-ENC segue interrompida com a ref guardada (`app/docs/BACKLOG.md:988`). Que `78403ab2b` já tenha pousado sob outro hash: **[nao verificado]**. A anotação posterior do recenso ainda confirma as formas compostas nos dois repasses (`app/docs/RELATO.md:578`).

4. **RED lógico ainda discriminante, sem executar:**

- Plural recebe composto e o repassa ao pareador (`app/ponto/turnos.py:2132`, `app/ponto/turnos.py:2150`); o corpo embrulha cada chave novamente (`app/ponto/turnos.py:1956`). Com a fixture lógica X/Y descrita no anexo, a perda da geradora continua alcançando a escolha dos marcos (`app/ponto/turnos.py:1960`, `app/ponto/turnos.py:1984`).
- X: saída das 07h pode ser absorvida pela pausa do vínculo ativo, mantendo o turno aberto (`app/ponto/turnos.py:1356`, `app/ponto/turnos.py:1370`). Y: vínculo ativo sem pausa permite o fechamento na saída das 00h (`app/ponto/turnos.py:1421`, `app/ponto/turnos.py:1492`). O julgamento conserva o motivo `turno_aberto_relogio` enquanto a aberta estiver viva (`app/ponto/turnos.py:2116`).
- O lote converte corretamente composto → data e entrega seus próprios turnos ao encerramento (`app/ponto/turnos.py:3038`, `app/ponto/turnos.py:3045`).
- O vigia continua entregando composto à porta singular (`app/ponto/services/vigia_de_hora.py:58`), que o repassa ao pareador (`app/ponto/turnos.py:2098`). Portanto os dois casos lógicos do vigia continuam relevantes.
- O teste proposto `assertRaises(TypeError)` também distingue o HEAD: a casca documenta `{data}` mas não valida as chaves (`app/ponto/turnos.py:1854`, `app/ponto/turnos.py:1884`); o corpo simplesmente produz a chave aninhada (`app/ponto/turnos.py:1956`). Exceção efetivamente observada: **[nao verificado]**.

5. **Limite do artefato raspado:** o arquivo novo depende da fixture nova E4, ausente da cópia; executar apenas o arquivo BUG isolado não constitui o RED demonstrado **[nao verificado]**. Não interpretar os três valores literais `[cpf]` do anexo como fixture original: CPF é único e normalizado no `save` (`app/colaboradores/models.py:319`, `app/colaboradores/models.py:418`). Passagem/importação/resultado da fixture original da ref: **[nao verificado]**.

6. **Teste atual não substitui esse RED:** o existente documenta expressamente o contrato singular por data e o plural composto (`app/ponto/tests/test_c1_dias_encerrados.py:54`), cria célula com DNA divergente sem nomear geradora (`app/ponto/tests/test_c1_dias_encerrados.py:62`) e testa a alimentação plural e singular convertida (`app/ponto/tests/test_c1_dias_encerrados.py:66`, `app/ponto/tests/test_c1_dias_encerrados.py:73`). Ele não reúne, nesse caso, a fixture multivínculo/noturna X/Y do anexo (`app/ponto/tests/test_c1_dias_encerrados.py:53`).

## 15/11 20:10 · sessao ae12a531a8b9 · VINCULO-LINHA-DO-TEMPO · censo · origem=Code

**1. Escritor e portas**

Na cópia, a porta única declarada é o **módulo** `colaboradores/services/vinculo.py`, não exclusivamente `executar_vinculo`. O gate exige ausência de escritores externos e tem allowlist e pendências vazias. (`app/core/portas.py:470`; `app/escala/tests/test_chokepoint_escala_gate.py:27`; `app/escala/tests/test_chokepoint_escala_gate.py:55`)

O executor **já absorve posteriores**: fecha anteriores, fecha inativas sobrepostas e chama `absorver_vigencias_posteriores`. Portanto, “hoje fecha só o ANTERIOR; o POSTERIOR sobrevive” está defasado em relação ao código fornecido. (`app/colaboradores/services/vinculo.py:214`; `app/colaboradores/services/vinculo.py:241`; `app/colaboradores/services/vinculo.py:253`; `app/docs/BACKLOG.md:1182`)

| Porta | Caminho constatado |
|---|---|
| Wizard de fase | Preenche campos; o formulário envia para `vincular_escala`, que chama juiz/executor. (`app/colaboradores/views_fase.py:5`; `app/templates/colaboradores/partials/painel_vinculo.html:136`; `app/colaboradores/views.py:411`; `app/colaboradores/views.py:423`) |
| Cadastro de escala/painel | Mesmo formulário e executor. (`app/templates/colaboradores/partials/painel_vinculo.html:136`; `app/colaboradores/views.py:423`) |
| Admissão | **Não cria EC nem chama o executor nessa operação**: cria `Colaborador` e registra `HistoricoVinculo`. (`app/colaboradores/views.py:1184`; `app/colaboradores/services/colaborador.py:91`; `app/colaboradores/views.py:1220`) |
| Edição cadastral | Salva `Colaborador`, não EC. (`app/colaboradores/views.py:1344`) |
| Django admin de EC | Somente leitura; adição, alteração e exclusão negadas. (`app/escala/admin.py:19`; `app/core/admin.py:59`; `app/core/admin.py:62`; `app/core/admin.py:65`) |
| Desvinculação | Chama `encerrar_vinculo` do módulo. (`app/escala/views.py:267`) |
| Desligamento | Chama `fechar_vinculos_ativos` do módulo. (`app/colaboradores/services/desligamento.py:147`) |
| Troca pelo chat | Chama juiz/executor. (`app/colaboradores/services/troca_pelo_chat.py:72`; `app/colaboradores/services/troca_pelo_chat.py:75`) |
| Régua/transferência | Chama juiz/executor. (`app/escala/regua_defesa.py:316`; `app/escala/regua_defesa.py:321`) |
| Command de fase | Chama juiz/executor. (`app/ponto/management/commands/censo_fase_12x36.py:422`) |
| Command corrigir vigência | Usa reabertura, fechamento e absorção do módulo. (`app/escala/management/commands/corrigir_vinculo_vigencia.py:86`; `app/escala/management/commands/corrigir_vinculo_vigencia.py:107`; `app/escala/management/commands/corrigir_vinculo_vigencia.py:120`) |
| Command corrigir retroativa | Usa criação de trecho e correção do módulo. (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:195`; `app/colaboradores/management/commands/corrigir_escala_retroativa.py:211`) |
| Command fechar desligados | Usa encerramento do módulo. (`app/colaboradores/management/commands/fechar_vinculo_de_desligado.py:55`) |
| Recusa de aceite | Restaura pelo módulo. (`app/chamados/services/acoes_chamado.py:147`) |
| Ajuste automático/desfazer | Usa funções do módulo. (`app/escala/services/escala_auto_executor.py:94`; `app/escala/services/escala_auto_executor.py:124`) |
| Signals | Leem alterações e acionam regeneração/invalidação/revalidação/reparentamento; não escrevem os três campos investigados. (`app/escala/signals.py:25`; `app/escala/signals.py:73`; `app/escala/signals.py:125`; `app/escala/signals.py:145`; `app/escala/signals.py:174`; `app/escala/signals.py:203`) |

**Escritores de produção externos encontrados: nenhum no censo estático.** Isso não equivale a uma trava de runtime contra qualquer escrita dinâmica; o próprio detector documenta operações não atribuíveis e exclui testes/migrations. (`app/core/censo_escritas.py:43`; `app/core/censo_escritas.py:54`; `app/escala/tests/test_chokepoint_escala_gate.py:55`)

Há várias funções escritoras **dentro** do módulo:

| Escrita | Arquivo:linha |
|---|---|
| Reativar/criar no executor | `app/colaboradores/services/vinculo.py:276`, `app/colaboradores/services/vinculo.py:287` |
| Fechar `(ativa=False, data_fim)` | `app/colaboradores/services/vinculo.py:641` |
| Reabrir `(ativa=True, data_fim)` | `app/colaboradores/services/vinculo.py:730` |
| Absorver, preservando fim | `app/colaboradores/services/vinculo.py:799` |
| Encerrar por `save` | `app/colaboradores/services/vinculo.py:843` |
| Criar trecho retroativo | `app/colaboradores/services/vinculo.py:970` |
| Corrigir fim retroativo | `app/colaboradores/services/vinculo.py:982` |
| Restaurar início/fim/ativa de snapshot | `app/colaboradores/services/vinculo.py:1031` |
| Ajustar automaticamente | `app/colaboradores/services/vinculo.py:1087`, `app/colaboradores/services/vinculo.py:1089` |
| Desfazer ajuste | `app/colaboradores/services/vinculo.py:1106`, `app/colaboradores/services/vinculo.py:1110` |

A correspondência exata da cópia com o HEAD/hash informado é **[nao verificado]**. O passivo atualmente zero em produção é **[nao verificado]**; zero em 30/09 é registro histórico. (`app/docs/RELATO-ARQUIVO.md:3097`)

**2. Censo dos leitores**

As autoridades registradas são `vinculo_do_dia` e `vinculo_cobre_a_data`; a forma ORM é `q_vinculo_cobre_a_data`. A cobertura considera intervalo e `ativa or data_fim`; a escolha diária considera a geradora da célula e, sem geradora, os candidatos que cobrem a data. (`app/core/juizes.py:1314`; `app/escala/alimentacao.py:196`; `app/escala/alimentacao.py:214`; `app/escala/alimentacao.py:219`; `app/escala/alimentacao.py:269`; `app/escala/alimentacao.py:285`)

Convenção do inventário: **A** consulta essa autoridade/carregador; **B** escolhe vínculo/tipo/fase/posto por `ativa`, sem cobertura da data; **C** usa estado para outra pergunta, descrita abaixo. Chamar depois `eh_dia_trabalho` ou marcos não transforma a seleção do vínculo em A. (`app/chamados/juizes.py:1852`; `app/chamados/juizes.py:1857`; `app/escala/services/furos_diarios.py:301`; `app/escala/services/furos_diarios.py:316`)

| Sítio | Classe e leitura |
|---|---|
| `app/escala/folgas.py:74`, `app/escala/folgas.py:79` | **A** — ativa delimita universo; cobertura mensal vem do carregador em `app/escala/folgas.py:78`. |
| `app/escala/folgas.py:459` | **A** — desempata ativos entre candidatos obtidos por `q_vinculo_cobre_a_data`. (`app/escala/folgas.py:451`) |
| `app/ponto/portas/celula.py:908` | **A** — guarda de ativa depois de cobertura e junto da escolha pelo juiz. (`app/ponto/portas/celula.py:906`; `app/ponto/portas/celula.py:909`) |
| `app/ponto/nucleo.py:15` | **A** — ordena candidatos do carregador; consulta cobertura por dia em `app/ponto/nucleo.py:26`. |

Os seguintes sítios são **B**, com a escolha descrita na última coluna:

| Arquivo:linha | Escolha por ativa sem cobertura da data |
|---|---|
| `app/colaboradores/views_fase.py:45` | Tipo/âncora do EC mais recente. O início que cobre o dia contraditório é consultado separadamente em `app/colaboradores/views_fase.py:73`. |
| `app/colaboradores/views.py:48`, `app/colaboradores/views.py:226`, `app/colaboradores/views.py:268`, `app/colaboradores/views.py:336`, `app/colaboradores/views.py:355`, `app/colaboradores/views.py:394`, `app/colaboradores/views.py:927`, `app/colaboradores/views.py:2206`, `app/colaboradores/views.py:2864` | Prefetch/listas, folga, painéis, posto, agenda e disponibilidade. |
| `app/colaboradores/services/calendario.py:258` | Contexto do calendário. |
| `app/colaboradores/services/drawer.py:22` | Contexto do drawer. |
| `app/colaboradores/services/detalhe.py:73` | Contexto do detalhe. |
| `app/colaboradores/services/troca_pelo_chat.py:54` | Rótulo do vínculo anterior. |
| `app/ponto/selecao_periodo.py:278` | Jornada usada como teto do turno. |
| `app/ponto/turnos.py:2464`, `app/ponto/turnos.py:2483` | Tipo para próxima batida/intermitente/intervalo. |
| `app/ponto/turnos.py:1560` | Ordena por `ativa,data_inicio` e conserva o último; dívida explicitada em `app/ponto/turnos.py:1541`. |
| `app/ponto/worklist.py:49` | Posto da fila DP. |
| `app/ponto/views.py:2064` | Rótulo da escala. |
| `app/ponto/services/faltas_de_hoje.py:35` | EC usado para perguntar se trabalha. |
| `app/ponto/services/abriu_nao_bateu.py:194` | EC usado para procurar marco do dia. |
| `app/ponto/services/piso_intervalo.py:28` | Template para batidas do período. |
| `app/ponto/services/espelho.py:42`, `app/ponto/services/espelho.py:201` | Piso visual e resumo da escala. |
| `app/ponto/services/bordas_realizado.py:62` | Intervalo livre do tipo ativo. |
| `app/ponto/services/fechamento_lista.py:59` | Escala do resumo. |
| `app/ponto/services/triagem_batida.py:92` | “Sem escala” pela inexistência de ativa. |
| `app/ponto/services/triagem_batida.py:332` | Rótulo de anuência por tipo ativo. |
| `app/inteligencia/calculadores.py:327` | Cálculo de desvio de escala. |
| `app/relatorios/furos.py:60` | EC ativo sem fim para limbo de hoje/amanhã. |
| `app/relatorios/views.py:492` | Extrato parcial. |
| `app/relatorios/views.py:1320` | Primeiro EC com `ec.ativa`. |
| `app/relatorios/pendencias.py:74` | Tipo exibido para direção. |
| `app/core/views.py:401`, `app/core/views.py:454`, `app/core/views.py:789` | Supervisor, onboarding e painel TV. |
| `app/core/services/painel_op.py:109` | Mapa de tipo ativo. |
| `app/holerite/views.py:282` | Escala na lista de pessoas. |
| `app/escala/views_plano_folgas.py:55` | ECs da grade mensal. |
| `app/escala/views.py:17` | Minha escala. |
| `app/escala/services/escala_auto.py:172` | EC ativo sem fim mais recente para proposta. |
| `app/escala/services/fase_conflitante.py:71` | Foto/fase do próprio vínculo ativo. |
| `app/escala/services/impacto_troca.py:34` | EC do “antes” da troca. |
| `app/escala/services/meio_periodo.py:152` | Observado por tipo ativo. |
| `app/escala/services/cadastro_realidade.py:228` | Mapa de tipo ativo comparado com células do período. |
| `app/escala/services/furos_diarios.py:35`, `app/escala/services/furos_diarios.py:301` | Apuração e classificação sem escala/limbo/sem dia. |
| `app/api/views_core.py:299` | `api_me`. |
| `app/api/views.py:311`, `app/api/views.py:908` | `api_me` e `api_escala_v2`. |
| `app/chamados/juizes.py:1852` | EC para marco esperado da pergunta. |
| `app/chamados/views.py:442` | EC ativo mais recente do tipo. |
| `app/chamados/services/disputa_emissao.py:349` | Opções de escala por ativa/sem fim. |
| `app/chamados/services/desvio_escala.py:44` | EC ativo mais recente. |
| `app/folha/views_retrato.py:107` | Cadastro do retrato. |
| `app/folha/services/ranking_he.py:132` | Tipo usado no ranking. |
| `app/ferias/management/commands/alertar_retornos.py:28` | Tipo ativo. |
| `app/relatorios/management/commands/diff_extrato_parcial.py:105` | Mapa de tipo ativo. |
| `app/escala/management/commands/termometro_regua.py:35` | EC para régua. |
| `app/escala/management/commands/celulas_contra_o_template.py:28` | EC ativo comparado com células. |
| `app/ponto/management/commands/diff_espelho_alimentacao.py:94` | Caminho legado do diff. |
| `app/ponto/management/commands/tripwire_celulas.py:17` | Mapa de EC ativo comparado com células. |
| `app/ponto/management/commands/reconciliar_fantasmas.py:173` | EC do diagnóstico. |
| `app/ponto/management/commands/fechar_chamados_meio_periodo.py:60` | Tipo ativo. |
| `app/ponto/management/commands/detectar_intervalo_ausente.py:115` | Tipo ativo. |
| `app/ponto/management/commands/censo_fase_12x36.py:403` | EC ativo mais recente na operação `_curar`. |

Os seguintes sítios são **C** — não consultam o juiz nessa leitura, mas respondem outra pergunta:

| Arquivo:linha | Outro uso |
|---|---|
| `app/folha/export.py:806` | Guarda de vínculo sem âncora para exportação. |
| `app/colaboradores/services/geofence_raios.py:63` | Quantidade de vínculos ativos por posto. |
| `app/ponto/services/cluster_espurio.py:185` | Universo do reconciliador: existência de ativo sem fim. |
| `app/relatorios/views.py:830`, `app/relatorios/views.py:856` | Universo dos filtros de busca por tipo/fase. |
| `app/escala/views_plano_folgas.py:43` | Postos com vínculo ativo. |
| `app/escala/views_plano_folgas.py:109`, `app/escala/views_plano_folgas.py:176` | Guarda por PK para editar/aplicar folgas. |
| `app/escala/utils.py:2115` | Existência de âncora; função retorna zero em `app/escala/utils.py:2123`. |
| `app/escala/regua_defesa.py:259` | Universo/contagem de templates para sugerir destino. |
| `app/escala/views.py:86`, `app/escala/views.py:124`, `app/escala/views.py:165`, `app/escala/views.py:171`, `app/escala/views.py:240`, `app/escala/views.py:262`, `app/escala/views.py:392` | Listas/contagens, proteção de exclusão, não vinculados, alvo do desvínculo e aplicação de calendário; inclui consulta `ativa=False`. |
| `app/escala/services/escala_auto.py:186` | Contagem de compartilhamento do template. |
| `app/escala/services/meio_periodo.py:80` | Contagem por template. |
| `app/escala/services/furos_diarios.py:158` | Universo do ranking por join de ativa. |
| `app/escala/services/cadastro_tipo.py:339` | Universo de propagação do template; recorte temporal próprio em `app/escala/services/cadastro_tipo.py:342`. |
| `app/api/views_mensageria.py:1222` | Pendência cadastral de folga em 6x1. |
| `app/chamados/reconciliador.py:233` | Premissa “existe ativa sem âncora”. |
| `app/chamados/detector_anomalias.py:58` | Colisão por quantidade de ativas sem fim; não é censo geral de sobreposição temporal. |
| `app/chamados/services/limbo_folgas.py:65` | Quantidade de usuários do template. |
| `app/chamados/services/desvio_escala.py:106` | Outros usuários ativos do template. |
| `app/colaboradores/management/commands/tripwire_vinculo_orfao.py:45` | Estado ativo de colaborador fora da situação ativa. |
| `app/colaboradores/management/commands/fechar_vinculo_de_desligado.py:34` | Alvos de fechamento. |
| `app/colaboradores/management/commands/sincronizar_feriado_admin.py:42` | Universo de atualização cadastral. |
| `app/ponto/management/commands/detectar_par_relampago.py:336` | Universo do detector. |
| `app/ponto/management/commands/detectar_cluster_espurio.py:71` | Universo do detector. |
| `app/ponto/management/commands/tripwire_celulas.py:57` | Universo ativo e regra temporal própria, sem juiz, em `app/ponto/management/commands/tripwire_celulas.py:67`. |
| `app/ponto/management/commands/checklist_golive.py:70` | Contagem de pessoas sem join de ativa. |
| `app/ponto/management/commands/gerar_batidas_smoke_motor.py:73` | Universo smoke com limite próprio de início. |
| `app/ponto/management/commands/detectar_ausencias.py:185` | Colegas usuários do template. |
| `app/ponto/management/commands/propor_reorganizacao.py:42` | Universo ativo intermitente. |
| `app/escala/management/commands/corrigir_vinculo_vigencia.py:100`, `app/escala/management/commands/corrigir_vinculo_vigencia.py:124` | Filtros Python por ativa e início para planejar correção. |
| `app/relatorios/management/commands/vinculo_do_dia_divergentes.py:118` | Comparador temporal legado, confrontado com juiz em `app/relatorios/management/commands/vinculo_do_dia_divergentes.py:115`. |
| `app/chamados/services/carga_pergunta_viva.py:97` | Ordenação da carga para leitores C6. |
| `app/ponto/portas/regen_exportada.py:180` | Ativa decide extensão do horizonte. |
| `app/escala/admin.py:28` | Filtro administrativo por estado. |
| `app/colaboradores/services/ficha.py:94`, `app/escala/services/vigencia_sem_trilha.py:78` | Serialização de estado. |
| `bin/censo_folha.py:25` | Contagem diagnóstica de ativas; zero recebe rótulo `SEM_ESCALA` em `bin/censo_folha.py:33`. |

No próprio escritor, são **C**, como estado/identidade/alvos da operação: no-op (`app/colaboradores/services/vinculo.py:119`, `app/colaboradores/services/vinculo.py:711`); foto anterior (`app/colaboradores/services/vinculo.py:183`); alvos de fechamento (`app/colaboradores/services/vinculo.py:215`, `app/colaboradores/services/vinculo.py:242`, `app/colaboradores/services/vinculo.py:863`); EC recém-escrita/aceite (`app/colaboradores/services/vinculo.py:322`, `app/colaboradores/services/vinculo.py:405`); censo de sobreposição com comparação própria (`app/colaboradores/services/vinculo.py:523`, `app/colaboradores/services/vinculo.py:537`); absorção (`app/colaboradores/services/vinculo.py:788`); invalidação de âncoras (`app/colaboradores/services/vinculo.py:990`); fotos/trilha (`app/colaboradores/services/vinculo.py:37`, `app/colaboradores/services/vinculo.py:111`, `app/colaboradores/services/vinculo.py:497`, `app/colaboradores/services/vinculo.py:639`).

A implementação da própria autoridade/carregador é **C**, não leitor externo: candidatura, ordenação, vigência declarada, negação ORM e desempate. (`app/escala/alimentacao.py:150`; `app/escala/alimentacao.py:153`; `app/escala/alimentacao.py:196`; `app/escala/alimentacao.py:202`; `app/escala/alimentacao.py:286`)

Leituras em testes são **C**, como arranjo/asserção, nos seguintes sítios: [nao verificado]

| Área | Arquivo:linha |
|---|---|
| Colaboradores | `app/colaboradores/tests/test_vincular_escala.py:110`; `app/colaboradores/tests/test_fase_12x36_tela.py:105`; `app/colaboradores/tests/test_ficha_fonte_dos_totais.py:68`; `app/colaboradores/tests/test_bug110_rota_da_ui_vincula.py:83`; `app/colaboradores/tests/test_vincular_escala_selos.py:52`; `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:88`; `app/colaboradores/tests/test_escala_aceite.py:78` |
| O207/linha do tempo | `app/colaboradores/tests/test_o207_vinculo_inicio_declarado.py:157`, `app/colaboradores/tests/test_o207_vinculo_inicio_declarado.py:231`, `app/colaboradores/tests/test_o207_vinculo_inicio_declarado.py:266`, `app/colaboradores/tests/test_o207_vinculo_inicio_declarado.py:309`, `app/colaboradores/tests/test_o207_vinculo_inicio_declarado.py:324`; `app/colaboradores/tests/test_vinculo_linha_do_tempo.py:73`, `app/colaboradores/tests/test_vinculo_linha_do_tempo.py:125` |
| Ponto | `app/ponto/tests/test_alertas_nucleo.py:80`, `app/ponto/tests/test_alertas_nucleo.py:154`, `app/ponto/tests/test_alertas_nucleo.py:198`, `app/ponto/tests/test_alertas_nucleo.py:241`; `app/ponto/tests/test_fluxos.py:129`; `app/ponto/tests/test_e4cal_l1_d4_natureza.py:84` |
| Demais | `app/core/tests/test_haiku_cirurgico.py:121`; `app/escala/tests/test_termometro_camada3.py:102`; `app/escala/tests/test_vigencia_um_escritor.py:79`, `app/escala/tests/test_vigencia_um_escritor.py:160`; `app/escala/tests/test_e4_limbo_pergunta_a_juiza.py:41`; `app/chamados/tests/test_e4b_c12_orm.py:82` |

O censo existente de “cópias da pergunta 2” procura comparações de início/fim; ele **não é um censo de todo `ativa=True` sem data**. Portanto sua lista vazia não certifica ausência dos B acima. (`app/core/censo_pergunta2.py:10`; `app/core/censo_pergunta2.py:37`; `app/core/censo_pergunta2.py:115`)

**3. Testes/fixtures e os “19 selos”** [nao verificado]

O registro dos **19 vermelhos é de `UniqueConstraint(colaborador, condition=ativa=True)`**, incluindo vínculos adjacentes. Não demonstra 19 falhas de EXCLUDE. O relato nomeia oito testes; os outros onze e a reprodução da contagem nesta cópia são **[nao verificado]**. (`app/docs/RELATO-ARQUIVO.md:3094`; `app/docs/RELATO-ARQUIVO.md:3099`; `app/docs/RELATO-ARQUIVO.md:3102`)

| Teste nomeado | Construção e dependência | [nao verificado]
|---|---|
| `test_01_colisao_escala_multiplas_ativas` | **Sobreposição proposital**: dois ativos sem fim; exige detectar colisão. (`app/chamados/tests/test_detector_avancado.py:121`; `app/chamados/tests/test_detector_avancado.py:123`; `app/chamados/tests/test_detector_avancado.py:129`) |
| `test_MORDE_a_absorcao_deixa_trilha_nos_DOIS_lugares` | **Sobreposição proposital**: dois ativos abertos para provar absorção com histórico/auditoria. (`app/colaboradores/tests/test_vinculo_linha_do_tempo.py:95`; `app/colaboradores/tests/test_vinculo_linha_do_tempo.py:99`; `app/colaboradores/tests/test_vinculo_linha_do_tempo.py:105`) |
| `test_MORDE_o_censo_acha_a_sobreposicao_e_a_cura_a_zera` | **Sobrepos

[truncado em 20000 de 26324 caracteres]

## 15/11 20:09 · sessao aa99330323f7 · O95 · censo · origem=Code

1. **A absorção ainda existe, mas o desfecho já tem uma correção posterior na cópia.** O ramo absorve a batida quando há turno corrente, distância ao intervalo `<=90` minutos e intervalo tão próximo quanto — ou mais próximo que — a borda. Ele não procura volta `E`, carimba `_intra_dur=True`, acrescenta a batida ao turno e continua. O empate favorece o intervalo. (`app/ponto/turnos.py:1356`, `app/ponto/turnos.py:1364`, `app/ponto/turnos.py:1370`)

Há uma guarda anterior: uma batida ainda não marcada como intervalo, próxima do marco e além de `cont_max_s` desde a entrada, encerra o turno corrente **como aberto** antes dessa absorção. A comparação usa timestamps; não exige volta. (`app/ponto/turnos.py:1349`)

Entretanto, ao final, `parear_turnos` executa `_fechar_aberto_com_saida_seguinte` e depois **`_fechar_aberto_na_pausa_sem_volta`**. Portanto, a linha anexada descreve um mecanismo ainda presente, mas não descreve integralmente o comportamento final desta cópia. (`app/ponto/turnos.py:1499`)

O resgate O95 fecha na última batida do turno quando:

- o turno tem entrada e continua sem saída;
- a última batida é `S`, carimbada `_intra_dur`;
- existe batida posterior;
- a duração passa pelo piso de par e cabe no teto da jornada. (`app/ponto/turnos.py:1166`, `app/ponto/turnos.py:1170`, `app/ponto/turnos.py:1176`)

**Essa prova posterior não é “volta dentro de `intra_max_s`”.** É qualquer batida posterior conhecida: o máximo das batidas carregadas e, quando fornecido, `ultima_batida`. Sem fato posterior, conserva aberto. (`app/ponto/turnos.py:1156`, `app/ponto/turnos.py:1173`)

Existe uma diferença entre portas: `turnos_do_colab` fornece o máximo global das batidas apuráveis; `turnos_de_batidas` e `turnos_via_autoridade` não fornecem esse provedor, ficando com o universo carregado. Isso precisa entrar no DIFF por janela. (`app/ponto/turnos.py:1898`, `app/ponto/turnos.py:1992`, `app/ponto/turnos.py:1535`, `app/ponto/motor_calculo_v2.py:503`)

A comparação com `_marca_intra_duracao` exige três distinções:

| Situação | Prova realmente exigida |
|---|---|
| Sem bordas declaradas | Turno em curso, `S→E` dentro de `intra_max_s` **e `S` seguinte** dentro de `cont_max_s`. (`app/ponto/turnos.py:407`) |
| Com bordas | `S→E` dentro de `intra_max_s`; `S` longe das bordas; `E` também longe, **ou** continuidade provada pela próxima `S`. A saída posterior não é exigida universalmente: ela libera a exceção da volta próxima da borda. (`app/ponto/turnos.py:432`, `app/ponto/turnos.py:446`, `app/ponto/turnos.py:453`) |
| Ramo `S,S` | Pode marcar a primeira `S` sem volta `E`, desde que encontre outra `S` posterior, dentro do envelope, próxima de borda. (`app/ponto/turnos.py:476`) |

O comentário citado de 25/07 existe e corresponde à **exceção de continuidade**; interpretá-lo como exigência universal de saída posterior seria mais forte que o corpo. (`app/ponto/turnos.py:438`, `app/ponto/turnos.py:453`)

2. **O campo compartilhado não garante significado idêntico entre leitores.** `Turno` contém `data_turno`, objetos de batida nas bordas, lista de batidas e estado aberto. A construção calcula o dia pelo juiz; ele pode ser a véspera da data civil da entrada. (`app/ponto/turnos.py:43`, `app/ponto/turnos.py:285`, `app/ponto/turnos.py:312`)

As chamadas diretas de produção encontradas são:

| Porta | Local e participação nessa absorção |
|---|---|
| Motor | `turnos_via_autoridade`, com marcos e parâmetros `AUT_*`. (`app/ponto/motor_calculo_v2.py:503`) |
| Batidas já carregadas | `turnos_de_batidas`, com marcos do template/DNA. (`app/ponto/turnos.py:1535`) |
| Geometria por colaborador | `_turnos_do_colab_puro`, com marcos, papel da ata e última batida global. (`app/ponto/turnos.py:1992`) |
| Turno aberto atual | `_turno_aberto_puro`. (`app/ponto/turnos.py:2278`) |
| Reconciliação de fantasmas | Passa marcos de intervalo e borda. (`app/ponto/management/commands/reconciliar_fantasmas.py:99`) |
| Chamadas sem marcos de intervalo | Não ativam esse ramo específico: órfãos, semana do posto, meio período, fila por causa, diagnóstico e lastro. (`app/ponto/utils_turno_orfao.py:31`, `app/escala/services/semana_do_posto.py:120`, `app/escala/services/meio_periodo.py:168`, `app/chamados/services/fila_por_causa.py:197`, `app/ponto/services/diagnostico_escala.py:142`, `app/ponto/services/lastro.py:268`) |
| Piso de intervalo | Passa explicitamente `marcos_intervalo=None`. (`app/ponto/services/piso_intervalo.py:48`) |

O consumo nos domínios pedidos é este:

| Leitor | O que lê; diferença de significado |
|---|---|
| **Motor** | O adaptador transforma bordas em **instantes**, aberto em saída `None` com anomalia, e saída órfã somente em anomalia. Intervalos vêm da autoridade. (`app/ponto/adaptador_motor.py:30`, `app/ponto/adaptador_motor.py:37`, `app/ponto/adaptador_motor.py:44`) |
| **Motor: dia da jornada** | Mantém mapa instante→`data_turno`, mas há duas receitas: períodos com parâmetros `AUT_*` e turnos do juiz para dia/intervalo. O próprio contrato distingue essas respostas. (`app/ponto/motor_calculo_v2.py:867`, `app/ponto/motor_calculo_v2.py:884`) |
| **Núcleo** | Copia `data_turno`, entrada, saída, aberto e cruzamento para `TurnoMaterializado`. Conserva o significado, mas publica uma representação persistida. (`app/ponto/nucleo.py:47`, `app/ponto/nucleo.py:50`, `app/ponto/nucleo.py:57`) |
| **Cartório/grade** | O cartório monta a grade; ela consulta turnos por `data_turno`, reapresenta as lâmpadas da montagem ao pareador e calcula realizado pelo juiz. A ata recebe minutos realizados, lâmpadas e órfãs. **Realizado não equivale a rubrica paga.** (`app/ponto/services/cartorio.py:1175`, `app/escala/utils.py:1581`, `app/escala/utils.py:1898`, `app/escala/utils.py:2045`, `app/ponto/services/cartorio.py:736`) |
| **Espelho tela** | Cards usam ponte instante→pk→dia do juiz. Porém o contador de turnos recorta a competência pela **data civil local da entrada**, enquanto `datas_turno_aberto` recorta pelo **dia do juiz**. É uma divergência concreta na borda da competência. (`app/ponto/services/espelho.py:426`, `app/ponto/services/espelho.py:450`, `app/ponto/services/espelho.py:471`) |
| **Espelho PDF** | O contador usa `len(turnos_do_colab(...))`, geometria da janela inteira; o comentário reconhece a diferença perante os períodos do motor contados pela tela. (`app/relatorios/pdf_espelho.py:444`, `app/relatorios/pdf_espelho.py:451`) |
| **Chamados: reconciliador** | Consulta `TurnoMaterializado` com entrada e sem saída, em `{dia−1,dia}`, e aplica veto posicional. Não é leitura direta do resultado vivo nem simples uso do booleano `aberto`. (`app/chamados/reconciliador.py:96`, `app/chamados/reconciliador.py:112`) |
| **Chamados: disputa/detector** | Juiz de disputa exige turnos fechados no `data_turno` pedido; detector procura a batida por timestamp e usa `t.data_turno`. (`app/chamados/juizes.py:401`, `app/chamados/detector_anomalias.py:157`, `app/chamados/detector_anomalias.py:162`) |
| **Chamados: materialização** | Origem procura turno aberto com entrada anterior ao instante de criação do chamado; `turnos_sem_par` inclui também saída órfã. São perguntas diferentes. (`app/chamados/services/materializacao.py:124`, `app/chamados/services/materializacao.py:127`, `app/chamados/services/materializacao.py:221`) |
| **Chamados: cruzamento** | Um juiz lê `cross_meianoite` por `data_turno`; outro considera as **datas civis locais das duas bordas** para resolver órfão. Não usam a mesma definição de dia. (`app/chamados/juizes.py:2168`, `app/chamados/services/disputa_emissao.py:722`, `app/chamados/services/disputa_emissao.py:729`) |
| **Folha** | Consome `FechamentoMensal` e traduz campos em eventos/rubricas; não reinterpreta diretamente `Turno`. Na lavratura diária, usa o mapa de jornada quando disponível e data civil local da entrada quando falta resposta no mapa. (`app/folha/export.py:402`, `app/folha/export.py:714`, `app/ponto/services/dia_pago.py:126`) |

Há ainda uma leitura temporal própria no verificador da folha: agrupa períodos por `_p.entrada.date()`, sem consultar `data_turno` nesse trecho. A ocorrência produtiva de divergência nesse sítio é [nao verificado]. (`app/folha/porta_export.py:535`)

Dois cuidados adicionais para o esmeril:

- O resgate O95 transforma a batida carimbada em saída **sem apagar o carimbo**. `_pares_marcados` exclui as bordas antes de contar pausa; contar somente `_intra_dur` mudaria o significado. O vigia também exclui bordas. (`app/ponto/turnos.py:1186`, `app/ponto/turnos.py:802`, `app/ponto/turnos.py:880`, `app/ponto/management/commands/vigiar_motor_autoridade.py:78`)
- O estado “em intervalo” significa turno aberto cuja última batida é `S`; não exige volta já existente. Esse estado participa da sugestão do próximo tipo. (`app/ponto/turnos.py:2498`, `app/ponto/turnos.py:2503`, `app/ponto/turnos.py:2474`)

3. **Há outras inferências por marco, mas com provas e efeitos diferentes.**

| Classe | Prova e efeito |
|---|---|
| **Eco junto de luz acesa, inclusive bordas** | Batida dentro do vão completo da ata e a até 15 minutos de uma luz recebe `X`, ficando interna. Não exige volta própria, mas exige entrada/saída da ata e luzes sustentadas por batidas reais únicas. (`app/escala/orfa_classe.py:28`, `app/ponto/turnos.py:1655`, `app/ponto/turnos.py:1682`, `app/ponto/turnos.py:1742`, `app/ponto/turnos.py:1804`) |
| **Primeira `S` de uma sequência `S,S`** | Absorvida sem `E` de volta, com prova de saída posterior próxima de borda e dentro do envelope. (`app/ponto/turnos.py:476`) |
| **Alinhamento de batida a borda** | `_match_marcos` atribui uma batida a um marco por distância/custo no envelope; pode acender uma borda sem parceiro. Isso produz atribuição, não prova de turno fechado. (`app/escala/utils.py:229`, `app/escala/utils.py:625`, `app/escala/utils.py:641`) |
| **Reclassificação E/S pelo marco** | `tipo_por_marcos` e `decidir_tipo_estrito` inferem papel sem exigir parceiro. Não equivalem à absorção como pausa. (`app/ponto/turnos.py:2397`, `app/ponto/turnos.py:2424`) |
| **Recorte das bordas para HE** | Entrada/saída efetivas podem cortar minutos conforme janela cadastrada; trabalham sobre pontas reais e autorização, sem provar uma volta. É efeito monetário por marco. (`app/ponto/motor_calculo_v2.py:1691`, `app/ponto/motor_calculo_v2.py:1733`, `app/ponto/janela_he.py:76`, `app/ponto/janela_he.py:117`) |
| **Desconto de intervalo declarado sem par batido** | `realizado_dos_turnos` subtrai a interseção com a janela declarada quando aplicável. É presunção de descanso na valoração, distinta de absorver uma batida. (`app/ponto/turnos.py:756`, `app/ponto/turnos.py:760`) |

A tolerância de pontualidade é outra operação: calcula atraso/antecipada e não altera horas trabalhadas nessa função. Não deve ser equiparada ao raio de casamento de marcos. (`app/ponto/motor_calculo_v2.py:1951`, `app/ponto/motor_calculo_v2.py:1959`, `app/escala/utils.py:653`)

4. **A sonda deve chamar o escritor real em modo leitura nos dois cenários.** A função é:

```python
recalcular_fechamento_mes(
    mes, ano,
    empresa_id=empresa.pk,
    colaborador_ids=ids,
    somente_leitura=True,
    retornar_objeto=True,
    lavrar_dias_pagos=False,
)
```

Ela monta o motor com `get_motor_cct`, apura por fatias e retorna os próprios fechamentos destacados; não é necessário reproduzir pareamento, soma de horas ou montagem do fechamento. (`app/ponto/services/fechamento.py:448`, `app/ponto/services/fechamento.py:735`, `app/ponto/services/fechamento.py:754`, `app/ponto/services/fechamento.py:890`, `app/ponto/services/fechamento.py:1128`)

O universo deve vir de `universo_da_competencia`, por empresa e competência, incluindo quem ainda não possui fechamento. A janela é `janela_fechamento`, não mês civil. Excluir entregues por `competencia_entregue`; para exigir também competência aberta, excluir `competencia_trancada`. Esses estados são distintos no código. O intervalo histórico e a quantidade de competências disponíveis são [nao verificado]. (`app/ponto/services/fechamento.py:382`, `app/ponto/janelas.py:54`, `app/folha/export.py:556`, `app/ponto/services/fechamento.py:1473`)

Esboço de orquestração, **não executado** [nao verificado]; os contextos representam interceptações somente em memória:

```python
from contextlib import nullcontext
from unittest.mock import patch
from django.db import transaction

from ponto import turnos
from ponto.services.fechamento import (
    recalcular_fechamento_mes,
    universo_da_competencia,
    competencia_trancada,
)
from folha.export import competencia_entregue

class RollbackSonda(Exception):
    pass

def apurar(empresa, mes, ano, ids):
    objetos = recalcular_fechamento_mes(
        mes, ano,
        empresa_id=empresa.pk,
        colaborador_ids=ids,
        somente_leitura=True,
        retornar_objeto=True,
        lavrar_dias_pagos=False,
    )
    if set(objetos) != set(ids):
        raise RuntimeError("apuracao incompleta")
    return objetos

# Exemplo: isolar apenas o resgate O95 já presente nesta cópia.
def contexto_antes():
    return patch.object(
        turnos, "_fechar_aberto_na_pausa_sem_volta",
        lambda lista, *args, **kwargs: lista,
    )

def contexto_depois():
    return nullcontext()  # executa a implementação real da cópia

for empresa, mes, ano in competencias_no_escopo:
    if competencia_entregue(empresa, mes, ano):
        continue
    if competencia_trancada(empresa.pk, mes, ano):
        continue

    ids = list(universo_da_competencia(
        mes, ano, empresa.pk,
    ).values_list("pk", flat=True))

    try:
        with transaction.atomic():
            with contexto_antes():
                antes = apurar(empresa, mes, ano, ids)
                foto_antes = fotografar_sem_dados_pessoais(antes)

            with contexto_depois():
                depois = apurar(empresa, mes, ano, ids)
                foto_depois = fotografar_sem_dados_pessoais(depois)

            raise RollbackSonda()
    except RollbackSonda:
        acumular_diff(foto_antes, foto_depois)

imprimir_agregados_e_colNNN()
```

`fotografar_sem_dados_pessoais`, `acumular_diff` e `imprimir_agregados_e_colNNN` são apenas o encanamento proposto da sonda, não funções existentes da aplicação. A interceptação acima mede **o resgate atual ligado versus desligado**, não reconstrói integralmente o código histórico de setembro. [nao verificado]

A fotografia deve conter:

- Campos numéricos dos fechamentos, incluindo horas trabalhadas, folga trabalhada, HE, noturnas, intra, descontos, DSR e banco; o conjunto já usado pelo serviço está em `CAMPOS`. Não somar indiscriminadamente parcelas que se sobrepõem. (`app/ponto/management/commands/aplicar_09_corte_b.py:67`, `app/folha/export.py:424`)
- Classificação real por `classificar_export(..., fechamentos=objetos)`, retendo somente identificador `colNNN`, estado e indicadores necessários; o retorno cru contém nome e código. (`app/folha/export.py:765`, `app/folha/export.py:770`)
- Rubricas emitíveis por `montar_linhas_export(..., itens=classificacao, fechamentos=objetos)` e `agregar_export_dominio`. Para preservar atribuição por colaborador mesmo com matrícula repetida, montar por objeto, usando a classificação do lote; descartar matrícula e TXT antes da saída. (`app/folha/export.py:690`, `app/folha/export.py:714`, `app/folha/export.py:725`, `app/folha/export.py:310`)
- Eventos antes da retenção, por `eventos_do_fechamento`, para distinguir alteração de cálculo de alteração de entrada no TXT. Valores das rubricas devem conservar unidade do catálogo ou formato cru; nem todo valor é hora. (`app/folha/export.py:402`, `app/folha/export.py:241`)

Já existe `diff_frota.medir`: compara **gravado / atual / novo**, chama o recálculo real e desfaz a transação. Porém seu universo nasce dos fechamentos existentes; além disso, uma interceptação global mantida pela mutação contaminaria o “atual” da empresa seguinte. Para reutilizá-lo, limitar cada chamada a uma empresa e restaurar a interceptação fora dela. (`app/ponto/services/diff_frota.py:62`, `app/ponto/services/diff_frota.py:72`, `app/ponto/services/diff_frota.py:84`)

A saída deve separar efeito `depois−antes`, deriva `antes−gravado` e diferença `depois−gravado`, com totais e quantidades de aumentos/reduções por campo e rubrica. Essa separação já existe em `diff_frota.somar`. Ausência no retorno deve ser falha de cobertura, pois o recalculador captura erro por colaborador e continua. (`app/ponto/services/diff_frota.py:101`, `app/ponto/services/fechamento.py:1303`)

A futura execução também precisa impedir escrita de handlers de log e emissão de dados pessoais: o ramo de erro registra nome. `atomic+raise` não resolve esse canal; sua configuração efetiva aqui é [nao verificado]. (`app/ponto/services/fechamento.py:1306`)

As comparações candidatas, sem escolher a cura, são:

| Candidata | O que o DIFF mediria |
|---|---|
| **Exigir volta `E` dentro de `intra_max_s` na absorção**, proposta pelo anexo | Mudanças de fechamento, divisão de turnos, pausas, horas e rubricas; também mudanças no almoço ainda em curso. O efeito produtivo da proposta é [nao verificado]. O teste existente exige conservar aberto quando só há entrada e saída para pausa. (`app/ponto/tests/test_o95_pausa_so_absorve_quem_voltou.py:80`) |
| **Conservar absorção e resgatar após fato posterior**, implementação presente | Efeito exclusivo da pós-passagem, diferenças entre janela curta e competência, tetos e preservação do intervalo com volta. Há testes definidos para esses comportamentos; execução nesta análise é [nao verificado]. (`app/ponto/turnos.py:1106`, `app/ponto/tests/test_o95_pausa_so_absorve_quem_voltou.py:58`, `app/ponto/tests/test_o95_pausa_so_absorve_quem_voltou.py:89`, `app/ponto/tests/test_o95_pausa_so_absorve_quem_voltou.py:111`) |

5. **Para reproduzir um total mensal do col727, a função apropriada é `recalcular_fechamento_mes`, com `colaborador_ids=[727]`, no modo leitura.** `parear_turnos` reproduz geometria; sozinho não reproduz horas e rubricas do fechamento. (`app/ponto/services/fechamento.py:448`, `app/ponto/services/fechamento.py:902`, `app/ponto/adaptador_motor.py:13`)

**Os `−38,02 h` históricos não são reproduzíveis com o material fornecido.** Faltam os insumos completos e a identificação dos dois cenários efetivamente comparados. [nao verificado]

Há ainda três números que não devem ser fundidos:

- O anexo informa `135,47 → 159,49 h`: diferença aritmética de **24,02 h**; o referencial dos `38,02 h` não foi fornecido. [nao verificado]
- O comentário do teste informa `135,47 → 163,46 h`: diferença de **27,99 h**, e menciona `38,02 h em 12 turnos`. Isso comprova o conteúdo do comentário, não uma medição reproduzida. (`app/ponto/tests/test_o95_pausa_so_absorve_quem_voltou.py:7`)
- O teste do col727 verifica o fechamento em `15:00` com batida posterior; não verifica o total mensal de `38,02 h`. (`app/ponto/tests/test_o95_pausa_so_absorve_quem_voltou.py:58`)

## 15/11 20:08 · sessao 1ba0d1600695 · CORTE-JUIZ-RESPONDIDA · leitura · origem=Code

O principal desacordo está na retenção: hoje ela aceita **instante OU texto**; a fila global já exige **instante E texto**. A ressuscitação ainda limpa somente o instante. (`app/chamados/models.py:1907`; `app/chamados/juizes.py:1725`; `app/chamados/models.py:1613`.)

As **143 vivas são a previsão do aval**, não uma medição apresentada de todos os resultados que mudarão. O anexo prevê duas abertas, `38251/38252`, e 141 entregues; não fornece a distribuição dos demais estados nem dos leitores. (`aval.txt:1`, anexo fornecido; contagens efetivamente observadas: **[nao verificado]**.)

Entrega abaixo, sem construção. Correspondência desta árvore com o HEAD/hash informado, execução dos testes e execução da sonda em produção: **[nao verificado]**. Nas referências à c9, a linha indicada é do **lado novo do diff fornecido**; os arquivos integrais daquela cópia não foram abertos: **[nao verificado]**. [nao verificado]

**Convenções para (a) e (b)**

`I` = instante presente; `T` = texto não vazio após o tratamento já existente, `(texto or '').strip()`. `J` designa o juiz novo especificado no corte: `I ∧ T`. A face SQL atual `q_respondida_com_texto()` já expressa esse par, incluindo os brancos reconhecidos pelo Python. (`aval.txt:1`, anexo; `app/chamados/juizes.py:1523`; `app/chamados/juizes.py:1539`; `app/chamados/juizes.py:1559`.)

| Marca usada nas tabelas | Estado de divergência do componente “respondida” |
|---|---|
| **Δ0** | Nenhuma diferença direta: já usa `I ∧ T`. (`app/chamados/juizes.py:1559`.) |
| **ΔB** | `I ∧ ¬T` **e** `¬I ∧ T`: o OU atual dá verdadeiro; J dá falso. (`app/chamados/models.py:1907`.) |
| **ΔC** | `I ∧ ¬T`: o instante sozinho dá verdadeiro; J dá falso. (`app/chamados/models.py:3641`.) |
| **ΔD** | `¬I ∧ T`: o texto sozinho dá verdadeiro; J dá falso. (`app/chamados/services/fio.py:1271`; `aval.txt:1`, anexo.) |
| **Δcru** | Texto sem instante e texto composto somente de brancos; depende de o sítio usar verdade crua ou comparação com `''`. (`app/colaboradores/services/calendario.py:474`; `app/templates/chamados/partials/_card_atendimento.html:141`.) |
| **Δestoque** | Depois da correção autorizada do estoque, leitores de dados passam a ver instante nulo e texto vazio nos alvos efetivamente corrigidos. Validadas/materializadas ficam preservadas. (`aval.txt:1`, anexo.) |
| **Outro** | O sítio lê conteúdo, cronologia, integridade, efeito ou trilha; não se substitui esse predicado inteiro por J. Exemplos: parse de hora, fotografia anterior e detecção de par ilegal. (`app/chamados/juizes.py:1900`; `app/chamados/services/disputa_emissao.py:1632`; `app/chamados/juizes.py:1531`.) |

As marcas indicam a diferença **local**. Filtros de vitalidade, validação, materialização, tranca, prazo, identidade e agregação podem impedir uma mudança no resultado externo. Particularmente, `q_vivas()` declara continência do C6, não igualdade. (`app/chamados/models.py:1905`; `app/chamados/models.py:3436`; `app/chamados/models.py:3439`.)

**(a) Usos que migram, por classe do censo**

Os identificadores R abaixo correspondem aos usos enumerados no anexo `censo.md`. Os sítios de código relacionados foram conferidos na cópia; a existência do arquivo físico `logs/JUIZ-RESPONDIDA/censo.md` nesta árvore: **[nao verificado]**.

**Classe A — instante E texto**

| R | Sítio e finalidade | Hoje → leitura da F1; divergência |
|---|---|---|
| 13 | C6, vitalidade. (`app/chamados/juizes.py:1403`; `app/chamados/juizes.py:1420`.) | `I∧T` → J sobre a projeção temporal; **Δ0**. Preserva os demais degraus. |
| 16 | Face SQL do par. (`app/chamados/juizes.py:1554`; `app/chamados/juizes.py:1559`.) | Aproveitar como face SQL de J; **Δ0**. |
| 19 | Fila global a validar. (`app/chamados/juizes.py:1725`; `app/chamados/juizes.py:1728`.) | Já usa face SQL e confirma C6; passa a nomear a autoridade nova; **Δ0**. |
| 29 | Auditor I21. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1572`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1575`.) | `I ∧ texto!=''`, sem trim → J SQL. Difere em **instante + brancos**, dentro dos demais filtros. |
| 52 | Degrau da resposta de `q_vivas`. (`app/chamados/models.py:3454`; `app/chamados/models.py:3468`.) | Já delega à face SQL; **Δ0**. |
| 79 | Resolvedor por chamado: preservação para materialização. (`app/chamados/services/disputa_emissao.py:2000`; `app/chamados/services/disputa_emissao.py:2001`.) | Cópia de `I∧T` → J; mantém a exigência de não materializada de fato; **Δ0**. |
| 96 | Janela visível do colaborador. (`app/chamados/services/janela_colab.py:63`.) | Herda a migração de `q_vivas`; **Δ0** no par. |
| 110 | Perguntas do app. (`app/chamados/services/pergunta_no_app.py:53`; `app/chamados/services/pergunta_no_app.py:61`.) | Herda J SQL/C6; **Δ0** no par. |
| 112 | Crédito/contador da mesa, lendo tuplas. (`app/chamados/services/raiox.py:138`; `app/chamados/services/raiox.py:143`; `app/chamados/services/raiox.py:144`.) | Cópia de `I∧T` → J mediante adaptação dos valores carregados; **Δ0**. |
| 125 | Vigia de viva em disputa fechada. (`app/chamados/services/vigia_fio.py:769`.) | Herda J SQL no pré-filtro; **Δ0** no par. |
| 141 | Alimentação do fecho do fio. (`app/ponto/services/veredito_celula.py:181`.) | Herda J SQL; **Δ0** no par. |

**Classe B — instante OU texto**

O corpo comum é `¬validada ∧ ¬materializada ∧ (I∨T)`, condicionado também à tranca. A migração troca **somente** `(I∨T)` por J; seus delegados continuam perguntando ao juiz de retenção. Todos os usos abaixo têm **ΔB**, quando satisfeitos seus respectivos universos. (`app/chamados/models.py:1905`; `app/chamados/models.py:1907`; `app/chamados/models.py:1909`.)

| R | Sítio | Finalidade e destino |
|---|---|---|
| 24 | `app/chamados/juizes.py:2079` | Irmãs sem consumo/retenção → corpo comum com J. |
| 27 | `app/chamados/management/commands/auditar_invariantes_chamados.py:984` | Auditor I12 → delegado de irmãs. |
| 38 | `app/chamados/management/commands/validar_classe_a.py:54` | Fila do comando → lista sem veredito. |
| 42 | `app/chamados/models.py:1907` | **Corpo que muda**: `sem_veredito_entre` → J. |
| 43 | `app/chamados/models.py:1889` | Lista sem veredito da disputa → mesmo corpo. |
| 44 | `app/chamados/models.py:1917` | Pode fechar → mesma lista, além da ausência de fio vivo. |
| 45 | `app/chamados/models.py:1950` | Fechamento → mesma lista impeditiva. |
| 60 | `app/chamados/reconciliador.py:448` | Retenção da retratação → irmãs. |
| 62 | `app/chamados/reconciliador.py:1164` | Encerramento de órfãs → lista sem veredito. |
| 69 | `app/chamados/services/competencia_trancada.py:126` | Medida de retenção, ignorando tranca → mesmo corpo. |
| 80 | `app/chamados/services/disputa_emissao.py:2030` | Proteção/reabertura do pai → irmãs. |
| 85 | `app/chamados/services/fila_por_causa.py:226` | Gaveta da fila → lista sem veredito. |
| 104 | `app/chamados/services/materializacao.py:768` | Retenção do destino durante materialização → irmãs. |
| 133 | `app/colaboradores/services/desligamento.py:165` | Desligamento → lista sem veredito. |
| 134 | `app/colaboradores/services/ficha.py:169` | Ficha → lista sem veredito, intersectada com vivas. |
| 142 | `app/ponto/supra_juiz.py:573`; `app/ponto/supra_juiz.py:574` | Retenção do CSF nos universos pai/destino → mesmo corpo, ignorando tranca. |

Retenção deixar de ocorrer **por resposta** não prova que o fechamento passará: C7 também exige ausência de fio vivo. (`app/chamados/models.py:1917`.)

**Classe C — só instante**

Salvo as exceções históricas expressamente identificadas, o componente `I` migra para J e `¬I` para `¬J`; consultas usam a face SQL, decisões sobre instâncias usam J. A divergência local é **ΔC**. (`app/chamados/models.py:3641`; `app/chamados/juizes.py:1559`.)

| R | Sítio | Predicado atual, finalidade e destino |
|---|---|---|
| 4 | `app/api/views.py:2577` | I seleciona respondidas no histórico → J; partição do app. |
| 5 | `app/api/views.py:2620` | I informa `ja_respondida` → J; correção/reenvio. |
| 18 | `app/chamados/juizes.py:1718` | I + lastro/vitalidade → J SQL + mesmos limites; corrigíveis. |
| 25 | `app/chamados/management/commands/auditar_invariantes_chamados.py:177` | Falta de I integra contraprova I21a → falta de J. |
| 26 | `app/chamados/management/commands/auditar_invariantes_chamados.py:592` | I em disputa aberta define piso do I2 → J. |
| 30 | `app/chamados/management/commands/auditar_invariantes_chamados.py:1603` | ¬I no auditor C2 → ¬J SQL, conservando os demais filtros. |
| 49 | `app/chamados/models.py:3128`; `app/chamados/models.py:3131` | I distingue pendente/respondida → J. As precedências de fase continuam em `app/chamados/models.py:3124`. |
| 56 | `app/chamados/models.py:3641` | Property `respondida=I` → delegação a J. |
| 58 | `app/chamados/models.py:4067`; `app/chamados/models.py:4074`; `app/chamados/models.py:4077` | **Preservação histórica:** I gera ato conservado e copia texto. Não apagar fotografia legada por ela não satisfazer J; separar isso da classificação corrente. |
| 64 | `app/chamados/services/acoes_disputa.py:95` | Conta ¬I para reabertura → conta ¬J SQL. |
| 65 | `app/chamados/services/acoes_disputa.py:130` | Viva ¬I impede regularização → viva ¬J. |
| 67 | `app/chamados/services/becos.py:30` | ¬I com destino não resolvido → ¬J; diagnóstico de órfã. |
| 71 | `app/chamados/services/declaracao_texto.py:419` | I informa reenvio → J. |
| 72 | `app/chamados/services/declaracao_texto.py:687` | ¬I dispara gravação da declaração → ¬J. |
| 76 | `app/chamados/services/disputa_emissao.py:1734` | Conta ¬I para recobrança → ¬J SQL. |
| 77 | `app/chamados/services/disputa_emissao.py:1842` | I exclui porta de reabrir credor → J; conserva C6 e demais cercas. |
| 89 | `app/chamados/services/fio.py:698` | Viva ¬I impede regularização → viva ¬J. |
| 94 | `app/chamados/services/historico.py:99`; `app/chamados/services/historico.py:105`; `app/chamados/services/historico.py:106` | **Evento histórico:** instante e `desde` selecionam ocorrência. Preservar a cronologia; não transformar evento anterior em classificação corrente nem remover trilha pelo novo J. |
| 107 | `app/chamados/services/materializacao.py:1375`; `app/chamados/services/materializacao.py:1379` | I + validada + não materializada → J SQL + mesmos filtros; entrada de materialização. |
| 111 | `app/chamados/services/pergunta_no_app.py:81` | ¬I + disputa aberta define pendente → ¬J. |
| 116 | `app/chamados/services/validacao.py:418` | ¬I retorna `sem_resposta` → ¬J. |
| 117 | `app/chamados/services/validacao.py:605` | ¬I retorna `ja_pendente` → ¬J. |
| 121 | `app/chamados/services/verbos.py:335`; `app/chamados/services/verbos.py:342`; `app/chamados/services/verbos.py:345` | I recente/posterior à cobrança → J SQL + limites temporais; contador por pessoa. |
| 123 | `app/chamados/services/vigia_fio.py:694` | I sob âncora terminal define causa c → J, mantendo a precedência diagnóstica anterior. |
| 128 | `app/chamados/views.py:1280` | I informa `ja_respondida` → J. |
| 129 | `app/chamados/views.py:1416`; `app/chamados/views.py:1417` | ¬I + C6 define pendentes vivas → ¬J + C6. |
| 130 | `app/chamados/views_cobrar.py:169`; `app/chamados/views_cobrar.py:176` | Vivas I para validar / ¬I para colaborador → J / ¬J. |
| 137 | `app/ponto/management/commands/reconciliar_grade.py:461` | I pré-filtra antes de fio vivo → J SQL. |
| 143 | `bin/censo_folha.py:28`; `bin/censo_folha.py:29` | Contadores I/¬I → J SQL/negação, conservando filtros de disputa/validação. |
| 148 | `app/templates/chamados/partials/_pergunta_card.html:24`; `app/templates/chamados/partials/_pergunta_card.html:29` | I governa rótulo/caixa → property/contexto que delega a J. |
| 150 | `app/templates/chamados/partials/modal_fio.html:185`; `app/templates/chamados/partials/modal_fio.html:217` | I governa correção/caixa → property/contexto que delega a J. |

A proteção do estoque consolidado não torna todos esses leitores invariantes: a property `respondida` lê qualquer fase, e o lote de materialização seleciona explicitamente perguntas validadas. Já `estado` pode conservar o rótulo pela precedência VALIDADA/RESOLVIDA. (`app/chamados/models.py:3641`; `app/chamados/services/materializacao.py:1377`; `app/chamados/models.py:3124`.)

**Classe D — só texto**

Onde T está sendo usado para afirmar “respondida”, passa a J, com **ΔD**. Guardas de conteúdo, preservação e apresentação continuam lendo o texto para responder à sua pergunta própria. (`app/chamados/services/fio.py:1271`; `app/chamados/services/materializacao.py:835`; `app/chamados/templatetags/chamados_tags.py:142`.)

| R | Sítio | Uso atual, finalidade e destino |
|---|---|---|
| 8 | `app/chamados/canal.py:65`; `app/chamados/canal.py:72` | SQL texto=`''` procura resposta no fio. Diagnóstico de conteúdo, não J completo; ampliar para “não respondida” exigiria ¬J e alcançaria também texto sem instante/brancos. Resultado dependente do fio: **[nao verificado]**. |
| 31 | `app/chamados/management/commands/auditar_invariantes_chamados.py:1655` | `any(T)` interrompe auditor F1 → J quando a exceção significa resposta recebida; **ΔD**. |
| 34 | `app/chamados/management/commands/backfill_decisoes_pergunta.py:504`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:506` | Carrega texto para eleição canônica → alimentar também I e delegar a autoridade usada por R90; **ΔD** no critério de resposta. |
| 37 | `app/chamados/management/commands/dedup_perguntas_fato.py:75` | T dá preferência na deduplicação → J no critério “respondida”; **ΔD**. |
| 40 | `app/chamados/models.py:1533` | Texto NULL/`''` restringe universo de ressuscitação. **Seleção de alvos**, não resposta corrente. Zerar integralmente os selecionados não implica ampliar automaticamente esse universo para todo ¬J. |
| 59 | `app/chamados/reconciliador.py:365` | T impede lavratura da órfã → J no impedimento por resposta; **ΔD**. |
| 61 | `app/chamados/reconciliador.py:1083` | T impede veredito da órfã → J no impedimento por resposta; **ΔD**. |
| 74 | `app/chamados/services/disputa_emissao.py:1663` | T impede sobrescrever resposta automática. Guarda de preservação do conteúdo; não autorizar sobrescrita de texto legado apenas porque J é falso. |
| 81 | `app/chamados/services/disputa_emissao.py:2078`; `app/chamados/services/disputa_emissao.py:2086` | T rotula auditoria “com resposta” → J se o rótulo afirma respondida; **ΔD**. O carimbo permanece na porta de `app/chamados/services/disputa_emissao.py:2081`. |
| 90 | `app/chamados/services/fio.py:1271` | T prefere canônica respondida → J; **ΔD**. |
| 91 | `app/chamados/services/fio.py:1283` | T + via admin protege mérito → J para a parcela “respondida”, conservando via/admin; **ΔD**. |
| 99 | `app/chamados/services/materializacao.py:438` | T + tipo hora permite apresentar estado de aplicação → J como entrada; conserva interpretação do conteúdo; **ΔD**. |
| 105 | `app/chamados/services/materializacao.py:835`; `app/chamados/services/materializacao.py:842` | ¬T recusa plantio. Entrada por J passa a recusar também `¬I∧T`; a guarda de conteúdo continua pertinente; **ΔD** na elegibilidade. |
| 126 | `app/chamados/templatetags/chamados_tags.py:142` | ¬T devolve leitura vazia; apresentação do conteúdo. **Outro**, além de **Δestoque**. |
| 132 | `app/colaboradores/services/calendario.py:474`; `app/colaboradores/services/calendario.py:476` | SQL texto!=`''` e C6 → J SQL + C6; **Δcru**. |
| 135 | `app/colaboradores/services/ficha.py:169`; `app/colaboradores/services/ficha.py:171` | ¬T e ausência do conjunto `falou` → J/¬J junto da migração B; os dois componentes precisam permanecer coerentes. **ΔB/ΔD**. |
| 136 | `app/folha/previa.py:142` | `any(T)` como evidência de fala. Se usado para afirmar pergunta respondida, J, **ΔD**; o conteúdo anterior continua evidência histórica. |
| 138 | `app/ponto/services/cartorio.py:1641`; `app/ponto/services/cartorio.py:1648` | `any(T)` protege conversa no cartório → J no impedimento por resposta; **ΔD**. |
| 145 | `app/templates/chamados/partials/_card_atendimento.html:141`; `app/templates/chamados/partials/_card_atendimento.html:182` | Verdade crua do texto governa apresentação. Rótulo de resposta recebida usa J; mera exibição conserva conteúdo. **Δcru** no primeiro caso; **Δestoque** no segundo. |

**Classe E — outros predicados e cópias**

| R | Sítio e finalidade | Encaminhamento e divergência |
|---|---|---|
| 2 | Correção/reenvio pelo dono da espera. (`app/api/views.py:1179`.) | Delegado de `aguarda_o_colaborador`; migra sua dependência para ¬J; **Δ0** na expressão equivalente atual. |
| 9 | Slot, teto temporal e parse. (`app/chamados/juizes.py:351`; `app/chamados/juizes.py:363`.) | J na elegibilidade de resposta; teto/parse continuam próprios. Texto sem instante pode perder elegibilidade; slot final: **[nao verificado]**. |
| 10 | Chave de memo, com os dois valores. (`app/chamados/juizes.py:855`; `app/chamados/juizes.py:856`.) | **Outro**: conservar insumos de invalidação; **Δestoque** na chave. |
| 11 | Concordância da hora declarada com a luz. (`app/chamados/juizes.py:1359`; `app/chamados/juizes.py:1371`; `app/chamados/juizes.py:1376`.) | **Outro**: interpretação permanece; o degrau chamador lê J. |
| 12 | Projeção por horizonte. (`app/chamados/juizes.py:1388`; `app/chamados/juizes.py:1393`.) | **Outro**: conservar projeção temporal; J recebe o par projetado. |
| 14–15 | Integridade `I∧¬T`, Python e SQL. (`app/chamados/juizes.py:1531`; `app/chamados/juizes.py:1551`.) | **Outro**: continuar identificando essa anomalia. `¬J` também inclui a pendente legal e não substitui esse detector. |
| 17 | Exclusão da anomalia na porta C17. (`app/chamados/juizes.py:1706`.) | Não trocar mecanicamente o detector por J/¬J; alcance depende da partição. **Δestoque** elimina a anomalia nos alvos corrigidos. |
| 20 | Reconhece declaração de hora pelo conteúdo. (`app/chamados/juizes.py:1753`; `app/chamados/juizes.py:1756`.) | Conservar interpretação; quando afirma declaração respondida, entrada por J. `¬I∧T` pode deixar de qualificar. |
| 21–23 | Delta, classe e plausibilidade. (`app/chamados/juizes.py:1900`; `app/chamados/juizes.py:2004`; `app/chamados/juizes.py:2026`.) | **Outro**: conservar parsers; J governa a entrada das filas/validação. |
| 28 | Auditor I20: ¬I e texto!=`''`, com exclusões nominais. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1101`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1105`.) | Integridade, não “respondida”. Deve distinguir texto após trim de brancos; não usar ¬J sozinho. |
| 32–33 | Seleção e cópia de projeções para trilha. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:56`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:60`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:326`.) | **Outro**: preservar fotografia histórica; não excluir assinatura anterior por J falso. |
| 35 | Busca literal da resposta na nota. (`app/chamados/management/commands/curar_perguntas_em_limbo.py:32`.) | **Outro**: correspondência de conteúdo/trilha. |
| 36 | Seleção de limbo ¬I e texto!=`''`. (`app/chamados/management/commands/curar_perguntas_em_limbo.py:46`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:69`.) | Integridade/cura; distinguir `¬I∧T` de brancos. O comando de cura não é leitor de resposta corrente. |
| 41 | Carrega par anterior à ressuscitação. (`app/chamados/models.py:1610`; `app/chamados/models.py:1611`.) | **Outro**: preservar anterior no ato; escrita muda no item (2). |
| 46–47 | Texto em SILENCIO/RETRATACAO. (`app/chamados/models.py:1990`; `app/chamados/models.py:2122`.) | **Outro**: fotografia histórica permanece. |
| 48 | Guarda do nascimento. (`app/chamados/models.py:2922`; `app/chamados/models.py:2923`.) | Expandir recusa de `I∧¬T` para as duas metades ilegais; J sozinho não distingue pendente legal de par ilegal. |
| 50 | Foto e preenchimento de instante pelo carimbo. (`app/chamados/models.py:3210`; `app/chamados/models.py:3213`; 

[truncado em 20000 de 61522 caracteres]

## 15/11 20:08 · sessao 1afa94a6b5f9 · ORDEM-VIVA-1511-NOITE · verificador · origem=Code

**1(a) — PROCEDE: “os cinco estão em B3, estacionamento, !, lei ou desenho” não confere integralmente.** O topo repete essa situação, mas há registros contrários na própria cópia: BO-0611-05 teve B3 liberado e pousou; E4b também pousou. Portanto, “lista sem abertura imediatamente liberada” não equivale à enumeração literal apresentada. (`app/docs/RELATO.md:4`, `app/docs/RELATO.md:1350`, `app/docs/RELATO.md:1351`, `app/docs/BACKLOG.md:2000`)

- BO-0611-05: **PROCEDE**, pois consta **NO AR**, merge `fb9269ea9`; a linha B3 do BACKLOG está vencida. (`app/docs/RELATO.md:1350`, `app/docs/BACKLOG.md:2004`)
- BO-DISPUTA-SEM-PERGUNTA: **CONFERE**, estacionada após 25 voltas, exigindo matriz conferida e aval. (`app/docs/BACKLOG.md:2005`)
- E4b: **CONFERE quanto às pendências**, mas o código já pousou; o apply dos 24 tem dependência própria de COMPETENCIA-PAGA, e o apply das 39 exportadas continua em `!`. (`app/docs/BACKLOG.md:2000`, `app/docs/BACKLOG.md:1942`, `app/docs/RELATO.md:4`)
- E4c: **CONFERE**, F1 nova em pré-desenho, construção somente por aval novo; F2–F9 dependem do pouso da F1. (`app/docs/BACKLOG.md:1945`, `app/docs/RELATO.md:17`)
- E4d: **CONFERE quanto à dependência**, pois Contract sucede a migração dos leitores; não está demonstrada como construção liberada. (`app/docs/BACKLOG.md:1991`)

**1(b) — CONFERE a frase; PROCEDE tomar prioridade como liberação.** “Construtor na 1a vaga após E3, na frente da LASTRO” existe, mas a mesma linha declara **PARADA até corte do desenho**. O pré-desenho foi entregue; a escolha entre selo do valor e acessor tipado continua **LU2-03 ABERTA**. O corte anterior diz “4a volta NAO abre” e mantém a trava atrás de **E3, DESLIGADO, FERIADO, O168 e HJ-090**. (`app/docs/BACKLOG.md:1891`, `app/docs/RELATO.md:716`, `app/docs/RELATO.md:717`, `app/docs/RELATO.md:63`)

O censo de 12/11 registra ATA **12 chaves/95 acessos/27 arquivos**, DNA **9/41/19**. Reexecutando a ferramenta somente em leitura nesta cópia, ATA permanece **12/95/27**, mas DNA resulta **9/51/20**. São eventos reconhecidos por AST, como cota inferior, sem prova de completude; a contagem histórica de DNA não representa mais esta cópia. (`app/docs/RELATO.md:808`; cálculo em `bin/leitores_de_chave.py:817`, `bin/leitores_de_chave.py:852`; limites em `bin/leitores_de_chave.py:4`)

**1(c) — CONFERE a existência da L-119 e da citação da ref; PROCEDE atribuir à lei a retomada dessa ref.** A L-119 diz que obra interrompida por bug P7.1 volta automaticamente ao topo quando o interruptor pousa, com registro no BACKLOG e medição no ESTADO. Não nomeia BUG-ENC nem qualquer ref de rescaldo. (`app/docs/LEIS.md:140`, `app/docs/CORTES.md:319`)

A ref `refs/rescaldo/1211/wt-e4carga-155931 @a2cd7981d` está expressamente citada na linha BUG-ENC, junto da interrupção por F2 e das pendências. O rescaldo registrou as 17 paradas como interrompidas, declarando **“ordem intocada”**. A existência/resolução efetiva da ref no Git é **[nao verificado]**. (`app/docs/BACKLOG.md:988`, `app/docs/RELATO.md:851`)

**1(d) — PROCEDE: a linha ABERTA de 08/11 está vencida.** Há prova documental positiva: **“PRONTO FIO-E0-V4-E1 NO AR 22:46”**, deploy `a8a692f5`; merge7 certificada com suite e sombra; resíduos encaminhados à E1-FILA, depois também pousada e registrada NO AR. (`app/docs/BACKLOG.md:1964`, `app/docs/RELATO.md:1674`, `app/docs/RELATO.md:1706`, `app/docs/RELATO.md:1679`, `app/docs/RELATO.md:1662`)

O código contém a migration E1 e a cura de classe v15g/MATRIZ-TODAS-AS-PORTAS. Isso sustenta entrega com resíduos desdobrados, não uma obra original ainda aguardando construção. Histórico Git independente e estado atual de produção: **[nao verificado]**. (`app/chamados/migrations/0051_decisao_pergunta.py:1`, `app/chamados/models.py:2373`)

**1(e) — CONFERE a natureza de instrumento; PROCEDE o delta literal.** O contador atual é explicitamente **“medida, nao escritor nem novo juiz”**, mas já existe como `cluster_encerrado_sem_lastro` e já alimenta o vigia. (`app/ponto/services/cluster_espurio.py:88`, `app/ponto/services/cluster_espurio.py:91`, `app/ponto/management/commands/detectar_cluster_espurio.py:92`)

O pedido antigo `fechados_por_idade` ficou vencido: o RELATO explica que idade não tinha fato registrado na fonte. Também registra pouso v5, apply dos três casos e DRY posterior vazio. Logo, não está demonstrado um delta novo pendente para construir. (`app/docs/BACKLOG.md:2121`, `app/docs/RELATO.md:1439`, `app/docs/RELATO.md:1164`)

**2 — PROCEDE nos seguintes pontos de compatibilidade literal.**

- **A lista substituída já recebeu substituições posteriores.** PLANO-JANELA-POS-RESET substitui ORDEM-POS-E4A2 do E4b em diante; ORDEM-ATE-O-ZERO substitui a ordem desse plano; ESPINHA-NA-FRENTE estabelece depois as vagas R3/F1. Assim, “substitui só a lista de 10/11” não identifica integralmente a ordem posterior que será deslocada. (`app/docs/BACKLOG.md:1839`, `app/docs/BACKLOG.md:1989`, `app/docs/BACKLOG.md:2216`)
- **F1 nova:** o aval vigente CORTE-JUIZ-RESPONDIDA exige **“construcao so por aval novo citando a entrega”**. Existirem corte e pré-desenho não satisfaz essa exigência. Há colisão se a nova frase for usada como autorização automática de construção. (`app/docs/BACKLOG.md:1945`, `app/docs/RELATO.md:4`)
- **TIPO:** a mudança de ordem não encerra LU2-03 nem libera desenho/B3; a regra vigente manda parar essa obra e seguir as outras. (`app/docs/RELATO.md:63`, `CLAUDE.md:274`, `CLAUDE.md:294`)

**CONFERE** manter leituras no Codex: QUEIMA já foi encerrada por POS-REDEFINICAO-1935. **CONFERE** teto 2, desde que global: instrumento não constitui terceira vaga de construtor. (`app/docs/RELATO.md:25`, `CLAUDE.md:291`)

**3 — PROCEDE nas condições que ainda não fecham.**

- **“Não cruza arquivo com R3 c17 nem com a F1”:** R3 tem pré-desenho entregue, mas c17 ainda exige aval novo; F1 continua com pré-desenho rodando e padrão a definir. Ausência de cruzamento com as construções futuras: **[nao verificado]**. O critério vigente é arquivo em comum. (`app/docs/RELATO.md:22`, `app/docs/BACKLOG.md:1945`, `CLAUDE.md:309`)
- **“O95 DIFF de frota”:** falta identificar qual alteração será comparada. Existe candidata escrita no BACKLOG, revertida; existe outra cura O95 no código, que preserva absorção e fecha pela existência de fato posterior. Portanto, “sem cura escrita” seria incorreto, mas o delta pretendido permanece indeterminado. O motor de DIFF distingue GRAVADO/HOJE/NOVO e exige uma mutação. (`app/docs/BACKLOG.md:1426`, `app/ponto/turnos.py:1119`, `app/ponto/turnos.py:1171`, `app/ponto/turnos.py:1502`, `app/ponto/services/diff_frota.py:7`, `app/ponto/services/diff_frota.py:45`)
- **FERIADO “termina a medida”:** a medida já está entregue e encontrou ENTREGUE_FORA=1; o ramo correspondente está declarado **não construído**, com AVAL-DEVOLVIDO preservado. Não há liberação implícita dessa construção. (`app/docs/RELATO.md:7`, `app/docs/RELATO.md:8`)

**4 — CONFERE para as leituras declaradas; PROCEDE qualquer extensão a apply.**

O95 alcança dinheiro: altera fechamento de turno, e o motor consome `parear_turnos`. O DIFF pode calcular esse efeito em transação desfeita; isso não autoriza persistência. Efeito monetário atual na frota: **[nao verificado]**. (`app/ponto/turnos.py:1186`, `app/ponto/motor_calculo_v2.py:503`, `app/ponto/services/diff_frota.py:84`, `app/ponto/services/diff_frota.py:89`)

O169 somente contador é leitura; `--apply` e `--legado --apply` são caminhos distintos que escrevem. VINCULO somente censo também não executa a autorização histórica de alteração retroativa. (`app/ponto/management/commands/detectar_cluster_espurio.py:55`, `app/ponto/management/commands/detectar_cluster_espurio.py:69`, `app/ponto/management/commands/detectar_cluster_espurio.py:80`, `app/docs/BACKLOG.md:1182`)

BUG-ENC declara mudança de veredito do cartório, e F1 registra **141 entregues com `!` próprio**. A nova ordem não comprova ausência de efeitos nem substitui esses avais; exportada e vínculo/escala continuam sujeitos às restrições vigentes. (`app/docs/BACKLOG.md:988`, `app/docs/BACKLOG.md:1945`, `CLAUDE.md:265`, `CLAUDE.md:411`)

**Marcador — CONFERE: escritor manual, juiz único no hook.** O valor é escrito **à mão**; `gerar_backlog.py` preserva e republica essa janela. O medidor de interrompidas alarma e não move o marcador. (`bin/gerar_backlog.py:113`, `bin/gerar_backlog.py:119`, `bin/gerar_backlog.py:138`, `bin/interrompidas_estado.py:31`)

Quem interpreta diretamente é `hook_stop_fila1.py::_item_da_ordem_viva`. Consomem essa resposta o selo O158, handoff, medidor de interrompidas e `backlog_estado` — este pela função `_proximo_da_fila`. (`bin/hook_stop_fila1.py:209`, `bin/tests/test_hook_nao_cobra_congelado.sh:111`, `bin/handoff_sessao.sh:64`, `bin/interrompidas_estado.py:93`, `bin/backlog_estado.py:56`)

**CONFERE: o id precisa existir no bloco OBRAS e ser aceito como aberto pelo juiz.** A correspondência do id é exata; inexistente retorna `ausente`, fechado/parado retorna `parado`. O selo rejeita ambos. Não exige a palavra literal `ABERTA` nem que seja a primeira linha da tabela. (`bin/hook_stop_fila1.py:213`, `bin/hook_stop_fila1.py:223`, `bin/hook_stop_fila1.py:225`, `bin/tests/test_hook_nao_cobra_congelado.sh:82`, `bin/tests/test_hook_nao_cobra_congelado.sh:122`)

**PROCEDE quanto ao id novo nesta cópia:** o marcador ainda aponta ORDEM-POS-E4A2; não há linha `ORDEM-VIVA-1511-NOITE` no bloco OBRAS inspecionado. Apenas trocar o comentário para esse id produziria `ausente` no juiz e vermelho no selo. Isso não equivale a garantir Stop liberado, pois o hook possui outras condições. (`app/docs/BACKLOG.md:8`, bloco delimitado em `app/docs/BACKLOG.md:713` e `app/docs/BACKLOG.md:2218`; `bin/hook_stop_fila1.py:228`, `bin/tests/test_hook_nao_cobra_congelado.sh:122`, `bin/hook_stop_fila1.py:1394`)

## 15/11 20:07 · sessao fc9b4b42c98c · FIO-E0-V4-E1-ENTRADAS · leitura · origem=Code

**Veredito: ABERTA para baixa integral, com o núcleo já pousado.** A descrição “em construção na volta 15g” ficou superada pelo pouso e pelo “PRONTO / NO AR”; porém a documentação ainda mantém a obra parada e reúne complementos pendentes. (`app/docs/RELATO.md:1674–1675`; `app/docs/GRADE-MESTRA.md:106`, `:683–701`)

A cópia não contém `.git`: `git log --all --grep` não pôde ser executado. Ancestralidade no `main`, hashes completos e datas dos commits **[não verificado]**. Abaixo, hashes e datas são **registros documentais**, sem consulta a banco ou logs. [nao verificado]

**1. Rastro encontrado**

| Data registrada | Hash / etapa | Evidência |
|---|---|---|
| 06/11, aval 22:12; despacho 22:55 | Abertura em `wt-fioe1-225336`, base `19aeede1`; E1 absorve PLANTIO (i) | `app/docs/BACKLOG.md:1964`; `app/docs/RELATO.md:1835–1836` |
| 02:20–02:25; dia do commit **[não verificado]** | `c2833d85`; PLANTIO `742f6e3b` reescrito para `51ea1e37`; árvore anterior `e55c6a37` | Entrega ainda incompleta, expressamente **“NAO POUSA”**. `app/docs/RELATO.md:1813–1815` |
| 07/11, registro da v5 | `12740227` | Desenho v5 respondido; construção retomada. `app/docs/BACKLOG.md:1964`; `app/docs/RELATO.md:1781–1783` |
| 07/11 16:2x | `344a9ddd`, volta 8 | Suite 14.165 OK, mas parada por desenho; respostas posteriores sobre veto e ordem do sinal. `app/docs/BACKLOG.md:1964`; `app/docs/RELATO.md:1769–1770` |
| 08/11, volta 14b | `6695e972` | Suite 14.301 OK; opus final 2 reprovou; nada pousado nessa etapa. `app/docs/RELATO.md:1742`, `:1721` |
| 08/11 11:2x | `d41caaf6`, volta 15 | Entrega e verificação; posteriormente superada pela 15b. `app/docs/RELATO.md:1729`, `:1726` |
| 08/11, volta 15b | `21e7341c` | Suite 14.325 OK; correções adicionais encaminhadas à 15c. `app/docs/RELATO.md:1719`, `:1725` |
| 08/11, volta 15c | `746179d7`; merge `b74da998` | Suite da volta verde; merge apresentou três falhas na catraca e defeito no portão do apply. `app/docs/RELATO.md:1714–1718` |
| 08/11, volta 15c2 | `3d8ebe87`; merge `76644dc8` | Suite 14.404 OK; opus final 3 reprovou o reuso com data antiga. `app/docs/RELATO.md:1710–1713` |
| 08/11, volta 15d2 | `f16d4737`; merge `b0c4297c` | Helper único; suite 14.422 OK; opus final 4 reprovou o caso de data NULL. `app/docs/RELATO.md:1707–1708`, `:1688` |
| 08/11, volta 15e | `06efab7e`; merge `a42d5204` | Contrato de saída e sombra verde; verificador pediu nova volta. `app/docs/RELATO.md:1696–1697` |
| 08/11, volta 15f | `4ecedc95`; merge `e2f865f5` | Suite 14.474 OK; achados g1–g5 encaminhados à 15g. `app/docs/RELATO.md:1698–1700` |
| 08/11 18:1x | `297499b8`, volta 15g | RED 8/20, GREEN 313; aval **E1-MATRIZ-TODAS-AS-PORTAS**. `app/docs/RELATO.md:1701`; `app/docs/PROMPTS.md:1992` |
| 08/11, após esse aval | `5c875ee3` | **Squash de `297499b8`**, ampliado às portas P1–P7; GREEN 5.978. `app/docs/RELATO.md:1702` |
| 08/11, interrompido 19:23 | `eabbfc5a`, merge6 | **Não certificou**; suite interrompida pelo PARE-P4. `app/docs/RELATO.md:1702`, `:1704` |
| 08/11, volta 15h | `6dee2010`; merge7 `b0e948d8` | Recusa de destino de outro colaborador; suite **14.536 OK** e sombra verde. `app/docs/RELATO.md:1705–1706` |
| 08/11 21:15; deploy 22:46 | `a8a692f5` | **Pouso local**, seguido de **PRONTO E1 NO AR**. `app/docs/RELATO.md:1675`, `:1674` |
| 08/11 23:12 | `a5df8ed5` | Reescrita de mensagens: árvore declarada igual a `a8a692f5`; 19 hashes alterados. Mapeamento efetivo **[não verificado]**. `app/docs/RELATO.md:1673` |
| 09/11 00:1x–00:4x | E1-FILA: `602a7d73` → `06ee1af0` → `45cf0e53` | Voltas corretivas; última com opus APROVA e suite 14.570 OK. `app/docs/RELATO.md:1677`, `:1676`, `:1668` |
| 09/11 00:5x; no ar 01:5x | Merge de `45cf0e53`; commit `7aa48401` | Pouso e deploy da **E1-FILA**. `app/docs/RELATO.md:1679–1680`; `app/docs/BACKLOG.md:1965` |
| 09/11 03:3x | Merge de `e9bf5de2`; commit `89df8c52` | **E1-FILA-V2-2-TRAVA também pousou**, com suite 14.622 OK e opus APROVA. `app/docs/RELATO.md:1633`; `app/docs/BACKLOG.md:1972` |

As demais menções exatas no RELATO são despachos, perguntas respondidas ou referências de contexto: `app/docs/RELATO.md:1662–1664`, `:1672`, `:1686–1690`, `:1693`, `:1723`, `:1728`, `:1749`, `:1768`, `:1784`, `:1787–1788`, `:1811–1812`, `:1818–1819`, `:9028`. As menções posteriores no BACKLOG vinculam complementos à E1. (`app/docs/BACKLOG.md:2086–2087`)

Não localizei os termos solicitados em `app/docs/RELATO-ARQUIVO.md` nem em `app/docs/TICKETS.md` **[não verificado]**.

**2. Houve pouso? O que ficou fora?** [nao verificado]

**Sim, documentalmente:** E1 em `a8a692f5`, E1-FILA em `7aa48401` e V2-2 em `89df8c52`. A comprovação por grafo Git de merge no `main` permanece **[não verificado]**. (`app/docs/RELATO.md:1675`, `:1679–1680`, `:1633`)

Ficaram separados:

- **Cinco grupos exportados**, expressamente fora do apply; o aval autorizava pousar com os limpos. (`app/docs/BACKLOG.md:855`; `app/docs/RELATO.md:1731`)
- Recusas sem contador, origem sem dia, conversa/reabertura sobre retratada e duplicatas sobre retratada, em filas próprias. (`app/docs/BACKLOG.md:857–860`)
- Resíduos de leitores, relógios, travas, outras portas e custo. **V2-2-TRAVA não está inteira pendente: ela pousou.** (`app/docs/BACKLOG.md:1970–1974`)
- Complementos posteriores **BUG-PLANTIO-NOTURNO-MAIS-1** e **ABSOLVIDA-PELA-FOLGA**; o segundo consta explicitamente aberto em 12/11. (`app/docs/BACKLOG.md:2086–2087`)

Há registro de quatro migrações aplicadas por Ronald e DRY seguinte zerado; isso é prova documental, sem conferência do banco nesta auditoria. (`app/docs/RELATO.md:1682`)

**3. A implementação existe na cópia?**

| Elemento | Prova |
|---|---|
| Trava das quatro classes | `trava_na_ordem` adquire Colaborador → DisputaSupervisao → ChamadoColaborador → PerguntaDisputa, por pk. `app/chamados/models.py:2707`, `:2744`, `:2753`, `:2766`, `:2770` |
| Catraca AST | Enumera as quatro classes, fixa o dono autorizado e exige zero trava fora dele. `app/chamados/tests/test_e1_v7_catracas.py:71`, `:85`, `:387` |
| Append-only | `DecisaoPergunta`; bloqueios de update/delete/bulk_update e de sobrescrita por save. `app/chamados/models.py:3969–3979`, `:3982`, `:4046–4052` |
| Escritor e evento da célula | `registrar` insere decisões e acorda células no mesmo atomic; a emissão passa por `emitir_evento_celula`. `app/chamados/models.py:4086–4099`, `:3314–3331` |
| Lâmpada do efeito | O leitor usa `veredito_lavrado` vigente por `esta_stale`; vencido ou ausente resulta em `nao_sei`. `app/chamados/services/tarefa_do_dia.py:16–18`, `:422–425` |
| Dia do fato em ponto único | `gravar_dia_do_fato` concentra a atribuição de `data_evento`. `app/chamados/services/fio.py:829–839` |
| Backfill | Comando com `--aplicar`; registra silêncio, retrata duplicatas pela porta e desfaz o ato em caso de recusa. `app/chamados/management/commands/backfill_decisoes_pergunta.py:324`, `:331`, `:344`, `:425` |
| Identidade no banco | Migration 0053 recusa colisões e cria unicidade parcial por MARCO e BATIDA. `app/chamados/migrations/0053_pergunta_unica_por_familia.py:36–66` |

**Limite do backfill:** esse comando não aplica as linhas antigas de resposta/validação da projeção; reserva esse passo a outro ato. A E4 registra o backfill posterior. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:3–13`; `app/docs/BACKLOG.md:1991`) Resultados atuais dos testes **[não verificado]**.

**4. Decisão para a linha**

**ABERTA para baixa integral.** Há prova documental suficiente de que **o núcleo foi entregue**, mas não de quitação do agrupamento atual: a grade mantém “PARADA” e 16 complementos; há complemento explicitamente aberto. (`app/docs/RELATO.md:1674–1675`; `app/docs/GRADE-MESTRA.md:106`, `:683–701`; `app/docs/BACKLOG.md:2087`)

Também não há prova de a E1 ter sido absorvida por outra obra **[não verificado]**. A absorção documentada ocorre no sentido contrário: **E1 absorve PLANTIO (i)**. (`app/docs/RELATO.md:1835`)

O estado sustentado pela cópia é: **“núcleo pousado em 08/11; correções E1-FILA/V2-2 pousadas em 09/11; acervo exportado e complementos em linhas próprias”**. (`app/docs/RELATO.md:1675`, `:1679`, `:1633`; `app/docs/BACKLOG.md:855`; `app/docs/GRADE-MESTRA.md:683–701`)

## 15/11 20:07 · sessao ba2ae83313e6 · - · - · origem=Ronald (tmux)

Censo somente leitura. Convenção: **A** consulta autoridade de vigência/dia; **B** escolhe vínculo/tipo/fase/posto por `ativa`, sem vigência para a data; **C** outro uso, identificado abaixo. Chamar `eh_dia_trabalho` ou `marcos_do_dia` **depois de escolher EC por ativa** não torna a seleção A: exemplos `app/chamados/juizes.py:1852`/`:1857`, `app/escala/services/furos_diarios.py:301`/`:316`.

Autoridades: `tem_vigencia_declarada` implementa `ativa or data_fim` em `app/escala/alimentacao.py:196`; `vinculo_cobre_a_data` acrescenta intervalo em `:214`; SQL equivalente em `:219`; `vinculo_do_dia` lê geradora em `:269`, ou candidatos pelo juiz em `:285`, com desempate `ativa,data_inicio` em `:286`.

Inventário de consultas ORM explícitas, com prefixo `app/`:

| Arquivo | Sítios e classificação |
|---|---|
| `folha/export.py` | **C :806** — guarda sem âncora para classificar exportação; não escolhe EC do dia. |
| `folha/views_retrato.py` | **B :107** `_cadastro`. |
| `colaboradores/views_fase.py` | **B :45** — EC ativa mais recente para tipo/âncora; o início que cobre dia contraditório segue outra chamada, `inicio_do_vinculo`, em `:73`. |
| `colaboradores/views.py` | **B :48, :226, :268, :336, :355, :394, :927, :2206, :2864** — prefetch/lista, folga, painéis, posto, row, agenda e disponibilidade. |
| `colaboradores/services/calendario.py` | **B :258** `contexto_calendario`. |
| `colaboradores/services/drawer.py` | **B :22** `contexto_drawer`. |
| `colaboradores/services/detalhe.py` | **B :73** `contexto_detalhe`. |
| `colaboradores/services/troca_pelo_chat.py` | **B :54** — rótulo “de”; `julgar_vinculo` é chamado separadamente em `:58`. |
| `colaboradores/services/geofence_raios.py` | **C :63** — quantidade de vínculos ativos por posto. |
| `ponto/selecao_periodo.py` | **B :278** — jornada de escala ativa para teto do turno armazenado; ordena `data_inicio` ascendente e usa `setdefault` em `:280`. |
| `ponto/turnos.py` | **B :2464, :2483** — escala para tipo de próxima batida/intermitente/intervalo. |
| `ponto/worklist.py` | **B :49** — posto na fila DP. |
| `ponto/views.py` | **B :2064** — `_escala_rotulo_do_colab`. |
| `ponto/services/faltas_de_hoje.py` | **B :35** — escolhe EC ativa e depois pergunta trabalho em `:40`. |
| `ponto/services/abriu_nao_bateu.py` | **B :194** — escolhe EC ativa, depois marco para `dia` em `:198`. |
| `ponto/services/piso_intervalo.py` | **B :28** — usa template ativo para batidas de período. |
| `ponto/services/espelho.py` | **B :42, :201** — piso visual e resumo da escala. |
| `ponto/services/cluster_espurio.py` | **C :185** — universo de reconciliação: existência de ativo sem fim; não escolhe EC do dia. |
| `ponto/services/bordas_realizado.py` | **B :62** — usa intervalo livre do tipo do EC ativo em `:67`; grade diária é lida separadamente em `:68`. |
| `ponto/services/fechamento_lista.py` | **B :59** — resumo por tipo ativo. |
| `ponto/services/triagem_batida.py` | **B :92** — “sem escala” por existência de ativa; **B :332** — rótulo de anuência por tipo ativo. O mesmo arquivo usa `vinculo_do_dia` em outro trecho; isso não rege essas duas consultas. |
| `inteligencia/calculadores.py` | **B :327** — `_calc_escalas_desvio`. |
| `relatorios/furos.py` | **B :60** — escolhe vínculos ativos sem fim para verificar limbo de hoje/amanhã em `:67`. |
| `relatorios/views.py` | **B :492** extrato parcial; **C :830, :856** universo de filtros de busca por tipo/fase. |
| `relatorios/pendencias.py` | **B :74** — tipo ativo para direção. |
| `core/views.py` | **B :401, :454, :789** — supervisor, onboarding e painel TV. |
| `core/services/painel_op.py` | **B :109** — mapa de tipo ativo. |
| `holerite/views.py` | **B :282** — `_pessoas_da_celula`. |
| `escala/views_plano_folgas.py` | **C :43** — postos com vínculo ativo; **B :55** — ECs da grade mensal por ativa; **C :109, :176** — guarda de estado por PK para editar/aplicar folgas. |
| `escala/folgas.py` | **A :74, :79** — ativa limita universo da lavra; quem alcança mês vem de `escalas_do_periodo_por_colab` em `:78`. |
| `escala/utils.py` | **C :2115** — guarda de existência de âncora, função termina retornando zero em `:2123`. |
| `escala/regua_defesa.py` | **C :259** — universo/contagem de templates para sugerir destino, não EC do dia. |
| `escala/views.py` | **B :17** minha escala; **C :86, :124, :165, :171, :240, :262, :392** — contagens/listas, proteção de exclusão (inclui `ativa=False`), busca de ainda não vinculados, alvo do desvínculo, universo de aplicação de calendário. |
| `escala/services/escala_auto.py` | **B :172** — EC ativo sem fim mais recente por colaborador para proposta; **C :186** — contagem de compartilhamento de template. |
| `escala/services/fase_conflitante.py` | **B :71** — compara foto/fase no próprio vínculo ativo, sem selecionar vínculo do dia (`:78–80`). |
| `escala/services/impacto_troca.py` | **B :34** — EC ativo mais recente para “antes”; chama trabalho por dia em `:39`. |
| `escala/services/meio_periodo.py` | **C :80** — contagem por template; **B :152** — observado por tipo ativo. |
| `escala/services/cadastro_realidade.py` | **B :228** — mapa de tipo ativo em `:231`, usado contra células do período em `:233`. |
| `escala/services/furos_diarios.py` | **B :35** apuração; **C :158** universo do ranking por join de ativa; **B :301** classificação sem escala/limbo/sem dia. |
| `escala/services/cadastro_tipo.py` | **C :339** — universo de propagação de alteração do template; recorta início por `max(ec.data_inicio, janela_anterior)` em `:342`, não chama juiz da cobertura nesse laço. |
| `api/views_core.py` | **B :299** `api_me`. |
| `api/views.py` | **B :311, :908** — `api_me`, `api_escala_v2`. |
| `api/views_mensageria.py` | **C :1222** — pendência cadastral de folga em 6x1. |
| `chamados/reconciliador.py` | **C :233** — premissa “existe ativa sem âncora”. |
| `chamados/juizes.py` | **B :1852** — `_marco_esperado_da_grade`: data da pergunta não recorta o EC escolhido. |
| `chamados/views.py` | **B :442** — vínculo ativo mais recente do tipo. |
| `chamados/detector_anomalias.py` | **C :58** — conta colisão de ativas sem fim, `count()>1` em `:63`; não mede sobreposição temporal geral. |
| `chamados/services/limbo_folgas.py` | **C :65** — contagem de usuários de template. |
| `chamados/services/disputa_emissao.py` | **B :349** — opções de escala do colaborador por ativa/sem fim, sem data do fato. |
| `chamados/services/desvio_escala.py` | **B :44** — EC ativo mais recente; **C :106** — outros usuários ativos do template. |
| `folha/services/ranking_he.py` | **B :132** — tipo ativo para ranking. |
| `ferias/management/commands/alertar_retornos.py` | **B :28** — tipo ativo. |
| `relatorios/management/commands/diff_extrato_parcial.py` | **B :105** — mapa de tipo ativo. |
| `escala/management/commands/termometro_regua.py` | **B :35** — EC ativo para régua. |
| `escala/management/commands/celulas_contra_o_template.py` | **B :28** — mapa de vínculo ativo contra células. |

Consultas dos commands, prefixo `app/`:

| Arquivo | Sítios e classificação |
|---|---|
| `colaboradores/management/commands/tripwire_vinculo_orfao.py` | **C :45** — estado ativo de colaborador fora de situação ativa. |
| `colaboradores/management/commands/fechar_vinculo_de_desligado.py` | **C :34** — alvos de fechamento. |
| `colaboradores/management/commands/sincronizar_feriado_admin.py` | **C :42** — universo de atualização cadastral. |
| `ponto/management/commands/detectar_par_relampago.py` | **C :336** — universo ativo/sem fim para detector. |
| `ponto/management/commands/diff_espelho_alimentacao.py` | **B :94** — caminho legado do diff por ativa. |
| `ponto/management/commands/detectar_cluster_espurio.py` | **C :71** — universo ativo/sem fim para detector. |
| `ponto/management/commands/tripwire_celulas.py` | **B :17** — mapa de EC ativo contra célula; **C :57** — universo ativo, depois regra temporal própria `ec.data_inicio <= d <= teto` em `:67`, sem fim/juiz. |
| `ponto/management/commands/reconciliar_fantasmas.py` | **B :173** — EC ativo para diagnóstico. |
| `ponto/management/commands/checklist_golive.py` | **C :70** — conta pessoas sem join de ativa. |
| `ponto/management/commands/gerar_batidas_smoke_motor.py` | **C :73** — universo smoke, ativa mais `data_inicio__lte=PERIODO_FIM`, regra própria. |
| `ponto/management/commands/detectar_ausencias.py` | **C :185** — universo de colegas do template. |
| `ponto/management/commands/propor_reorganizacao.py` | **C :42** — universo ativo intermitente. |
| `ponto/management/commands/fechar_chamados_meio_periodo.py` | **B :60** — tipo ativo. |
| `ponto/management/commands/detectar_intervalo_ausente.py` | **B :115** — tipo ativo. |
| `ponto/management/commands/censo_fase_12x36.py` | **B :403** — `_curar` escolhe ativo mais recente. |
| `escala/management/commands/corrigir_vinculo_vigencia.py` | **C :100, :124** — listas Python de `x.ativa` com limites de início para planejar correção, regras da operação. |

No próprio escritor `app/colaboradores/services/vinculo.py`, todos são **C**, porque fotografam estado ou escolhem alvos da operação, em vez de responder qual vínculo cobre dia: **:119** guarda de no-op (`existente.ativa`), **:183** foto anterior, **:215** alvos ativos a fechar, **:242** inativos sobrepostos com limites próprios, **:322** EC recém-escrita por data inicial exata, **:405** EC para aceite, **:523** censo de sobreposição (compara intervalos próprios em `:537`), **:711** no-op da reabertura, **:788** absorção de inícios `>=D`, **:863** fechamento de ativos, **:990** invalidação de âncoras.

Outros critérios Python/ordenação, prefixo `app/`:

| Sítio | Classe e uso |
|---|---|
| `escala/alimentacao.py:150`, `:153`, `:196`, `:202`, `:286` | **C**, implementação da própria autoridade/carregador, não leitor externo. |
| `escala/folgas.py:459` | **A** — desempata candidato ativo depois de `q_vinculo_cobre_a_data` em `:451`. |
| `ponto/portas/celula.py:908` | **A** — guarda ativa após `vinculo_cobre_a_data` em `:906`, e consulta `vinculo_do_dia` em `:909`. |
| `ponto/turnos.py:1560` | **B** — `_escalas_da_ata`: ordena `ativa,data_inicio`, último vence, sem data; dívida explicitada em `:1541`. |
| `ponto/nucleo.py:15` | **A** como alimentação: candidatos vêm de `escalas_do_periodo`; a ordenação conserva precedência da autoridade. |
| `relatorios/views.py:1320` | **B** — retorna primeiro `ec.ativa` de `c.escalas.all()`. |
| `relatorios/management/commands/vinculo_do_dia_divergentes.py:118` | **C** — regra temporal própria legada de comparação (`:116`), contra juiz em `:115`. |
| `chamados/services/carga_pergunta_viva.py:97` | **C** — ordena vínculos da carga para leitores C6; não escolhe EC nessa linha. |
| `ponto/portas/regen_exportada.py:180` | **C** — `bool(_ec.ativa)` decide extensão de horizonte. |
| `escala/admin.py:28` | **C** — filtro administrativo por estado `ativa`. |
| `colaboradores/services/ficha.py:94` | **C** — serializa estado, sem filtro/decisão de cobertura. |
| `escala/services/vigencia_sem_trilha.py:78` | **C** — serializa estado, sem filtro/decisão de cobertura. |
| `colaboradores/services/vinculo.py:37`, `:111`, `:497`, `:639` | **C** — fotos/trilha/serialização de estado. |

Fora de `app/`: **C `bin/censo_folha.py:25`**, contagem `c.escalas.filter(ativa=True).count()`.

Leituras de ativa dentro de testes (não confundir com criação de fixtures): todas **C** como asserção/arranjo do teste, prefixo `app/`: [nao verificado]

- `colaboradores/tests/test_vincular_escala.py:110`; `test_fase_12x36_tela.py:105`; `test_ficha_fonte_dos_totais.py:68`; `test_o207_vinculo_inicio_declarado.py:157`, `:231`, `:266`, `:309`, `:324`; `test_vinculo_linha_do_tempo.py:73`, `:125`; `test_bug110_rota_da_ui_vincula.py:83`; `test_vincular_escala_selos.py:52`; `test_e4cal_l1_calendario_le_o_d4.py:88`; `test_escala_aceite.py:78`.
- `ponto/tests/test_alertas_nucleo.py:80`, `:154`, `:198`, `:241`; `test_fluxos.py:129`; `test_e4cal_l1_d4_natureza.py:84`.
- `core/tests/test_haiku_cirurgico.py:121`.
- `escala/tests/test_termometro_camada3.py:102`; `test_vigencia_um_escritor.py:79`, `:160`; `test_e4_limbo_pergunta_a_juiza.py:41`.
- `chamados/tests/test_e4b_c12_orm.py:82`.

Não executei testes nem usei banco/log; efeitos em produção e exaustividade além dos sítios encontrados por busca estática ficam [nao verificado]. [nao verificado]

## 15/11 20:06 · sessao ab3d0959c51a · - · - · origem=Ronald (tmux)

O número **19** documenta um ensaio de **UniqueConstraint por colaborador com `ativa=True`**, incluindo vínculos adjacentes; não prova 19 falhas de EXCLUDE por sobreposição. O relato nomeia somente oito testes. Identidade dos outros 11 e reprodução da contagem nesta cópia: **[nao verificado]**. (`app/docs/RELATO-ARQUIVO.md:3094`, `:3099`, `:3102`).

| Teste nomeado no relato | Construção comprovada | Propósito | [nao verificado]
|---|---|---|
| `test_01_colisao_escala_multiplas_ativas` | Dois ativos sem fim, iniciados hoje e 30 dias antes (`app/chamados/tests/test_detector_avancado.py:121`, `:123`, `:124`; helper `:79`) | Intencional: detectar colisão (`:122`, `:129`). |
| `test_MORDE_a_absorcao_deixa_trilha_nos_DOIS_lugares` | Dois ativos abertos em D e depois de D (`app/colaboradores/tests/test_vinculo_linha_do_tempo.py:95`, `:96`, `:97`; helper `:67`) | Intencional: absorção com histórico e auditoria (`:98`, `:99`, `:105`). |
| `test_MORDE_o_censo_acha_a_sobreposicao_e_a_cura_a_zera` | Mesma sobreposição (`app/colaboradores/tests/test_vinculo_linha_do_tempo.py:132`, `:133`, `:134`) | Intencional: censo antes/depois da cura (`:136`, `:139`). |
| `test_escala_vigente_vence_first` | **Adjacentes**, velha termina ontem e nova começa hoje; ambas ativas (`app/ponto/tests/test_turno_aberto_calc_janela.py:31`, `:46`, `:48`) | Intencional: escolher pela vigência (`:45`, `:51`). |
| `test_dia_sob_escala_antiga_usa_marcos_antigos` | **Adjacentes**, velha termina D−1 e nova começa D; ambas ativas (`app/ponto/tests/test_turnos_do_colab_troca_escala.py:38`, `:42`, `:43`, `:45`) | Intencional: marcos históricos (`:1`, `:51`). |
| `test_celula_da_vigencia_certa` | **Adjacentes**, antiga termina 04/08 e nova começa 05/08; ambas ativas (`app/ponto/tests/test_pareador_dna_multivig.py:81`, `:83`, `:85`, `:87`) | Intencional: célula nasce da vigência certa (`:62`, `:90`). |
| `test_dt_fim_por_te_vigente_na_data` | **Adjacentes**, anterior termina D−1 e nova começa D; ambas ativas (`app/ponto/tests/test_nucleo_dt_fim_vigencia.py:38`, `:42`, `:43`, `:45`) | Intencional: fim previsto por dia (`:1`, `:50`). |
| `test_dia_dentro_da_vigencia_do_novo_usa_o_novo` | Antigo aberto desde 01/06; novo ativo de 01/07 a 15/07: **sobreposição real** (`app/ponto/tests/test_vigencia_partida.py:29`, `:30`, `:34`, `:37`) | Intencional: seleção histórica e respeito a `data_fim` (`:1`, `:48`, `:55`). |

Outras construções atuais, sem prova de pertencerem aos 19 históricos:

- **Sobreposição entre ativos, intencional:** absorção, idempotência e fechamento do anterior (`app/colaboradores/tests/test_vinculo_linha_do_tempo.py:82`, `:108`, `:118`); fechamento/recusa, invariante e trilha de dois vínculos (`app/escala/tests/test_vigencia_um_escritor.py:72`, `:91`, `:152`).
- **Ativo aberto e futura ativa, intencional:** fixture que compara os dois juízes nas bordas (`app/escala/tests/test_e4_vinculo_cobre_a_data.py:25`, `:26`, `:29`, `:43`, `:51`).
- **Ativo e inativas com fim sobrepostos, intencional:** contador de inconsistências (`app/escala/tests/test_vinculos_inconsistentes.py:30`, `:41`, `:42`, `:43`, `:54`); desempate da ficha (`app/escala/tests/test_e4_parte4_cinco_copias.py:76`, `:80`, `:81`); desempate do tipo vigente (`app/escala/tests/test_e4_parte5_copias.py:116`, `:122`, `:126`); referência do fechamento (`app/ponto/tests/test_referencia_pergunta_a_juiza.py:29`, `:31`, `:33`, `:40`); calendário versus geradora (`app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:88`, `:91`, `:93`, `:100`).
- **Intervalos brutos sobrepostos com inativa sem fim, intencional:** seleção pela célula geradora (`app/ponto/tests/test_vinculo_do_dia_pela_celula.py:30`, `:67`, `:70`, `:93`). A inativa sem fim não é candidata pelo juiz atual (`app/escala/tests/test_e4_vinculo_cobre_a_data.py:35`, `:40`).
- **Dublês sem banco, sobreposição intencional:** domínio da geradora (`app/ponto/tests/test_cadastro_futuro_c5b.py:104`, `:122`, `:127`, `:141`); fechamento de inativa sobreposta no mesmo ato (`app/ponto/tests/test_cadastro_futuro_c5e.py:62`, `:68`, `:82`); feriado respeita geradora (`app/ponto/tests/test_cadastro_futuro_sem_banco.py:688`); fatias alternadas A/B/A (`app/ponto/tests/test_composicao_de_fatias_e_total.py:291`, `:293`, `:294`, `:295`).

Não há evidência suficiente para chamar esses casos de “fixture descuidada”; o propósito é explícito nos testes acima. Classificação dos demais testes/fixtures dinâmicos e inventário completo de todos os estados produzidos por execução: **[nao verificado]**. [nao verificado]

Quanto às constraints:

- O modelo tem unicidade de `(colaborador, data_inicio)` e somente a CHECK `ec_vigencia_fim_nunca_antes_do_inicio` na lista de constraints (`app/escala/models.py:1004`, `:1037`, `:1041`).
- A migration 0041 declara essa CHECK por `SeparateDatabaseAndState` e SQL **NOT VALID** (`app/escala/migrations/0041_ec_vigencia_not_valid.py:31`, `:35`, `:43`, `:46`).
- A migration **0042 atual** adiciona campos de HE em `TipoEscala`; não é a unique descrita no ensaio (`app/escala/migrations/0042_o146_he_extra_declarada.py:14`, `:17`, `:26`).
- O modelo afirma que a unique e seu selo ficaram no scratchpad (`app/escala/models.py:1019`, `:1036`). O SQL com `btree_gist` e EXCLUDE aparece no relato (`app/docs/RELATO-ARQUIVO.md:21721`, `:21722`, `:21725`); instalação efetiva desse SQL em produção: **[nao verificado]**.
- A rejeição de EXCLUDE com NOT VALID está registrada como medição histórica no relato; não foi reproduzida nesta investigação (`app/docs/RELATO-ARQUIVO.md:21712`, `:21714`).

## 15/11 20:06 · sessao 1a00fab08cd3 · - · - · origem=Ronald (tmux)

Funções e escopo para a sonda, com base apenas no código:

- Conta mensal real: `ponto.services.fechamento.recalcular_fechamento_mes(mes, ano, empresa_id=..., colaborador_ids=..., somente_leitura=True, retornar_objeto=True, lavrar_dias_pagos=False)`. Retorna o próprio `FechamentoMensal` destacado, evitando reconstruir o objeto a partir do recorte de campos. `app/ponto/services/fechamento.py:448`, `app/ponto/services/fechamento.py:890`, `app/ponto/services/fechamento.py:1128`
- Essa conta monta o motor por `get_motor_cct` e chama `apurar_por_fatia`; portanto, chamar somente `parear_turnos` ou somar duração bruta não reproduz o fechamento mensal. `app/ponto/services/fechamento.py:735`, `app/ponto/services/fechamento.py:754`
- Universo canônico: `universo_da_competencia(mes, ano, empresa_id)`; inclui situações diferentes de ativo e aplica a janela da empresa aos desligados. Não substituir por filtro `situacao='ativo'`. `app/ponto/services/fechamento.py:382`, `app/ponto/services/fechamento.py:428`
- Janela real: `janela_fechamento(mes, ano, empresa)`. Competência não equivale necessariamente ao mês civil. `app/ponto/janelas.py:10`, `app/ponto/janelas.py:54`
- Excluir empresas/competências entregues por `folha.export.competencia_entregue(empresa, mes, ano)`: verifica TXT vigente não invalidado. O status `aberto` do fechamento não responde essa pergunta. `app/folha/export.py:556`, `app/folha/export.py:588`

Existe instrumento real: `ponto.services.diff_frota.medir`, com três colunas: gravado, cálculo atual intacto, cálculo após mutação; usa `atomic` e `_Rollback`. `somar` distingue efeito (`novo-hoje`), deriva (`hoje-gravado`) e alteração contra gravado (`novo-gravado`). `app/ponto/services/diff_frota.py:45`, `app/ponto/services/diff_frota.py:72`, `app/ponto/services/diff_frota.py:101`

Há duas limitações a declarar se reutilizado: nasce exclusivamente dos `FechamentoMensal` existentes e ignora universo sem fechamento; e sua mutação corre por empresa numa única transação, então monkeypatch global mantido após a primeira empresa contaminaria o “hoje” das seguintes. Usar uma empresa por chamada, restaurando o patch fora dela. `app/ponto/services/diff_frota.py:62`, `app/ponto/services/diff_frota.py:76`, `app/ponto/services/diff_frota.py:84`

Esboço integral, sem segundo cálculo:

```python
# Esboço: somente memória/stdout; não executar neste trabalho.
for empresa, mes, ano in competencias_declaradas:
    if competencia_entregue(empresa, mes, ano):
        continue

    ids = list(universo_da_competencia(
        mes, ano, empresa.pk).values_list("pk", flat=True))

    try:
        with transaction.atomic():
            antes = fotografar(empresa, mes, ano, ids)

            # Contexto entra e sai antes da próxima empresa.
            # Substitui em memória a função/cura candidata REAL,
            # sem reimplementar pareamento na sonda.
            with candidata_em_memoria:
                depois = fotografar(empresa, mes, ano, ids)

            raise RollbackSonda()
    except RollbackSonda:
        registrar_delta_em_memoria(antes, depois)

imprimir_apenas_colNN_e_agregados()
```

`fotografar` deve chamar a função real acima com objetos destacados; depois chamar `classificar_export(empresa,mes,ano,fechamentos=objetos)` uma vez para o lote, guardar apenas colaborador, status, motivo e travas, descartando `nome`, `codigo_dominio` e detalhes textuais pessoais. A função retorna esses campos pessoais, logo não imprimir o retorno cru. `app/folha/export.py:765`, `app/folha/export.py:770`, `app/folha/export.py:783`

Para as rubricas efetivamente emitíveis, chamar `montar_linhas_export(..., itens=itens_classificados, fechamentos=objetos)`, que recebe a mesma injeção e não grava exportação nem aprovação. Comparar com `agregar_export_dominio`, parser existente. `app/folha/export.py:690`, `app/folha/export.py:697`, `app/folha/export.py:714`, `app/folha/export.py:310`

Para rubricas por `colNNN` sem ambiguidade de matrícula repetida, a fotografia pode chamar a montagem por objeto (`fechamentos=[f]`, classificação do lote já calculada), agregar e somar seus valores por rubrica internamente; nunca imprimir as linhas TXT, que carregam matrícula. `app/folha/export.py:716`, `app/folha/export.py:725`, `app/folha/export.py:728`, `app/folha/export.py:738`

Manter duas vistas: eventos derivados mesmo para retidos (`eventos_do_fechamento`) e rubricas realmente emitíveis após classificação. A primeira não resolve código de rubrica; a segunda inclui o gate de exportação. `app/folha/export.py:402`, `app/folha/export.py:439`, `app/folha/export.py:730`

Não rotular todo valor de rubrica como horas: o parser preserva valor cru e o próprio código registra que horas versus dias depende do catálogo. `app/folha/export.py:241`

Saída sugerida: por competência, `colNNN`, campo, antes, depois, delta; rubrica, unidade, antes/depois/delta; transições de retenção; totais, quantidade que aumentou/diminuiu e distribuição por campo/rubrica. Incluir faltantes em cada retorno como “sem resposta”, nunca zero: o recalculador captura erro por colaborador, registra e continua. O logger desse erro inclui nome, portanto a futura sonda precisa interceptar/redigir esse canal antes de emitir somente o dado raspado. `app/ponto/services/fechamento.py:1303`

Para col727, a função apta a reproduzir o **fechamento mensal atual** é `recalcular_fechamento_mes(...colaborador_ids=[727],somente_leitura=True,retornar_objeto=True)`; a fachada individual `fechamento_lido(colaborador,mes,ano)` chama exatamente essa conta. `app/ponto/services/fechamento.py:1759`

Os números históricos do anexo não são reproduzíveis com os insumos fornecidos: faltam batidas completas, competência, vínculos/células e demais fatos de cálculo; reprodução histórica na cópia atual também não está demonstrada. `[não verificado]` Há inconsistência aritmética no próprio anexo: `159,49−135,47=24,02 h`, portanto esses dois totais não demonstram `38,02 h`; o referencial desse segundo número não foi fornecido. `[não verificado]`

## 15/11 20:06 · sessao e3f972574a41 · - · - · origem=Ronald (tmux)

Censo de escritores, somente leitura:

- **Porta única declarada = módulo `app/colaboradores/services/vinculo.py`, não apenas `executar_vinculo`**: `app/core/portas.py:470`; gate com allowlist/pêndencias vazias e proibição de escrita fora da porta: `app/escala/tests/test_chokepoint_escala_gate.py:27`, `:28`, `:55`.
- **Nenhum escritor de produção de `EscalaColaborador.data_inicio/data_fim/ativa` fora desse módulo encontrado** na busca textual, busca por SQL/tabela e execução puramente estática de `core.censo_escritas.varrer_arvore`; limites do detector (atribuição e balde não atribuível) documentados em `app/core/censo_escritas.py:6`, `:43`, `:54`, `:469`. Não executei Django/testes/banco.
- **`executar_vinculo` já absorve posteriores**, contrário ao trecho antigo do backlog: fecha anteriores em `app/colaboradores/services/vinculo.py:214`, inativas sobrepostas em `:241`, absorve posteriores em `:253`, reativa existente em `:276`/`:277`/`:283`, cria em `:287`/`:291`/`:292`. Atomic: `:169`.

Portas:

| Porta | Caminho constatado |
|---|---|
| Wizard fase | Só leitura/preenchimento: `app/colaboradores/views_fase.py:5`, `:73`, `:93`; está dentro do formulário que faz POST em `vincular_escala`: `app/templates/colaboradores/partials/painel_vinculo.html:136`, `:157`; view chama juiz/executor em `app/colaboradores/views.py:411`, `:423`. |
| Cadastro de escala/painel | Mesmo formulário/mesma view acima: `app/templates/colaboradores/partials/painel_vinculo.html:136`; `app/colaboradores/views.py:423`. |
| Admissão | **Não escreve EC e não chama executor**: cria `Colaborador` em `app/colaboradores/views.py:1184`; a função só faz `Colaborador.objects.create` em `app/colaboradores/services/colaborador.py:91`; registra `HistoricoVinculo` e encerra resposta em `app/colaboradores/views.py:1220`, `:1230`. |
| Edição de cadastro | Salva `Colaborador`, não EC: `app/colaboradores/views.py:1344`, `:1345`. |
| Django admin EC | Bloqueado, não atravessa executor porque é somente leitura: `app/escala/admin.py:19`; add/change/delete negados e todos campos readonly em `app/core/admin.py:59`, `:62`, `:65`, `:68`. |
| Desvincular da escala | `encerrar_vinculo` do módulo: `app/escala/views.py:267`, `:269`. |
| Desligamento | `fechar_vinculos_ativos` do módulo: `app/colaboradores/services/desligamento.py:147`, `:148`. |
| Chat | `julgar_vinculo` e `executar_vinculo`: `app/colaboradores/services/troca_pelo_chat.py:72`, `:75`. |
| Régua/transferência | Mesma dupla: `app/escala/regua_defesa.py:316`, `:321`. |
| Command censo fase | Mesma dupla: `app/ponto/management/commands/censo_fase_12x36.py:422`, `:423`. |
| Command corrigir vigência | Usa reabrir/fechar/absorver do módulo: `app/escala/management/commands/corrigir_vinculo_vigencia.py:86`, `:107`, `:120`. |
| Command corrigir retroativa | Usa criar trecho/corrigir vínculo do módulo: `app/colaboradores/management/commands/corrigir_escala_retroativa.py:195`, `:196`, `:211`, `:212`. |
| Command fechar desligados | Usa encerrar do módulo: `app/colaboradores/management/commands/fechar_vinculo_de_desligado.py:55`, `:56`. |
| Aceite recusado | Restauração pelo módulo: `app/chamados/services/acoes_chamado.py:147`, `:149`. |
| Ajuste automático/desfazer | Funções do módulo: `app/escala/services/escala_auto_executor.py:94`, `:103`, `:124`, `:145`. |
| Signals EC | Leem estado anterior e regeneram células, invalidam previsto, revalidam disputas/reparentam folgas; nenhum escreve os três campos: `app/escala/signals.py:18`, `:25`, `:34`, `:73`, `:86`, `:125`, `:137`, `:145`, `:154`, `:174`, `:185`, `:203`. |

**Outros escritores, todos DENTRO do módulo** (útil para evitar chamar `executar_vinculo` de função única):

| Escrita | Referência |
|---|---|
| Fechamento `(ativa=False,data_fim)` | `app/colaboradores/services/vinculo.py:641` |
| Reabertura `(ativa=True,data_fim)` | `app/colaboradores/services/vinculo.py:730` |
| Absorção `ativa=False`, fim preservado | `app/colaboradores/services/vinculo.py:799` |
| Encerramento por save | `app/colaboradores/services/vinculo.py:843`, `:844`, `:845` |
| Criar trecho retroativo inativo | `app/colaboradores/services/vinculo.py:970`, `:972`, `:973` |
| Correção retroativa de fim | `app/colaboradores/services/vinculo.py:982`, `:983` |
| Restaurar snapshot, inclusive início/fim/ativa | `app/colaboradores/services/vinculo.py:1031`, `:1032` |
| Ajuste automático: fechar e criar | `app/colaboradores/services/vinculo.py:1087`, `:1089`, `:1090` |
| Desfazer: reabrir ou fechar/criar | `app/colaboradores/services/vinculo.py:1106`, `:1108`, `:1110`, `:1111`, `:1112` |

Migrations encontradas de EC são estrutura/schema, não escritores diretos adicionais dos três campos; definição original e defaults: `app/escala/migrations/0001_initial.py:36`; alterações de constraints/unique: `app/escala/migrations/0004_alter_escalacolaborador_unique_together.py:15`; CHECK vigência: `app/escala/migrations/0041_ec_vigencia_not_valid.py:23`. Testes/fixtures escrevem EC diretamente e estão fora do escopo de produção do gate por desenho: `app/core/censo_escritas.py:54`, `:66`, `:81`.

## 15/11 20:05 · sessao 055c14243405 · - · - · origem=Ronald (tmux)

Censo comprovado por código, sem banco/log:

| Leitor | Caminho real e campos | Semântica / ressalva |
|---|---|---|
| Geometria compartilhada | `Turno = (data_turno, entrada, saida, batidas, aberto, cross_meianoite)` em `app/ponto/turnos.py:43`; `_turnos_do_colab_puro` chama `parear_turnos` com marcos e papel da ata em `app/ponto/turnos.py:1992`; filtra pelo `t.data_turno` em `app/ponto/turnos.py:1995`. | `entrada`/`saida` são Batidas, seus timestamps são instantes; `data_turno` é decisão do juiz, podendo ser véspera, não sinônimo de data civil (`app/ponto/turnos.py:248`, `app/ponto/turnos.py:285`). |
| Motor | `turnos_via_autoridade` → `parear_turnos` (`app/ponto/motor_calculo_v2.py:503`); `_periodos_via_autoridade` → `periodos_de_turnos` (`app/ponto/motor_calculo_v2.py:530`). Adaptador publica `(entrada.timestamp, saida.timestamp|None)` e intervalos julgados (`app/ponto/adaptador_motor.py:37`, `app/ponto/adaptador_motor.py:44`). | Aberto vira período com saída None + anomalia; S órfã vira só anomalia (`app/ponto/adaptador_motor.py:30`, `app/ponto/adaptador_motor.py:39`). O mapa de dia liga instante de entrada a `t.data_turno` (`app/ponto/motor_calculo_v2.py:884`, `app/ponto/motor_calculo_v2.py:894`), mas há segunda receita: `_turnos_do_juiz` usa `turnos_de_batidas` e diz que parâmetros AUT do motor podem diferir (`app/ponto/motor_calculo_v2.py:867`, `app/ponto/motor_calculo_v2.py:882`). |
| Núcleo materializado | `recompute_turnos` chama `turnos_do_colab` e copia literalmente `data_turno`, `entrada`, `saida`, `aberto`, `cross_meianoite` para `TurnoMaterializado` (`app/ponto/nucleo.py:47`, `app/ponto/nucleo.py:50`). | Preserva os significados da geometria, mas é estado persistido; reconstrução faz delete + bulk_create em atomic (`app/ponto/nucleo.py:57`). |
| Cartório / grade | Cartório chama `montar_grade_prevista_periodo` (`app/ponto/services/cartorio.py:1175`); grade pergunta `turnos_do_colab` antes do alinhamento, indexa por `data_turno` (`app/escala/utils.py:1581`), e pergunta novamente com cópias de células e lâmpadas desta montagem (`app/escala/utils.py:1886`, `app/escala/utils.py:1898`). Calcula o realizado via `realizado_do_dia(... turnos=_turnos_juiz)` (`app/escala/utils.py:2045`). | Ata grava `minutos_realizados`, lâmpadas e órfãs (`app/ponto/services/cartorio.py:736`, `app/ponto/services/cartorio.py:740`). Não é o mesmo conceito de horas pagas. Dia sem turno recebe zero explicitamente na grade (`app/escala/utils.py:2058`). O cartório também usa `dias_encerrados` para fim de dia por instante/agora (`app/ponto/services/cartorio.py:1153`). |
| Espelho tela | Lê períodos do motor e `p.turno_aberto` (`app/ponto/services/espelho.py:421`); apresenta batidas e cards via `dia_das_batidas` e ponte instante→pk→dia (`app/ponto/services/espelho.py:450`, `app/ponto/services/espelho.py:458`). Também chama `turnos_do_colab` para geometria e datas de S órfã (`app/ponto/services/espelho.py:614`). | Divergência concreta: contador de turnos recorta competência por **data civil da entrada** (`app/ponto/services/espelho.py:426`), enquanto `datas_turno_aberto` recorta por **dia do juiz** (`app/ponto/services/espelho.py:471`). Dinheiro da linha vem de DiaPago, não do realizado cru (`app/ponto/services/espelho.py:406`, `app/ponto/services/espelho.py:410`). |
| Espelho PDF | Chama `turnos_do_colab` na janela inteira (`app/relatorios/pdf_espelho.py:213`); contador é `len(turnos_do_colab(...))` (`app/relatorios/pdf_espelho.py:451`). | O contador PDF conta geometria da janela inteira; a tela conta períodos do motor. Diferença está documentada no próprio sítio (`app/relatorios/pdf_espelho.py:444`). |
| Chamados: reconciliação | `turnos_abertos_de` lê **TurnoMaterializado**, com entrada presente, saída ausente e dia em `{dia-1,dia}` (`app/chamados/reconciliador.py:96`). | Não consulta geometria viva neste ponto; aplica veto por `dia_posicional_completo` (`app/chamados/reconciliador.py:112`), portanto não é consumo puro do mesmo booleano aberto. |
| Chamados: juízes / detector | Juiz de disputa pergunta `turnos_do_colab` ou lote, filtra `data_turno==dia`, exige todos fechados (`app/chamados/juizes.py:401`); detector localiza batida por timestamp em `t.batidas`, usa `t.data_turno` para folga (`app/chamados/detector_anomalias.py:157`, `app/chamados/detector_anomalias.py:162`, `app/chamados/detector_anomalias.py:166`). | Timestamp como chave no detector pode colidir entre duplicadas; mapa de apresentação deliberadamente usa pk (`app/ponto/turnos.py:2904`). Comentário do detector chama data_turno de data civil da entrada (`app/chamados/detector_anomalias.py:154`), mas implementação lê a resposta do juiz (`app/chamados/detector_anomalias.py:163`). |
| Chamados: materialização | Origem de turno procura `t.aberto`, entrada com timestamp anterior à criação do chamado (`app/chamados/services/materializacao.py:124`, `app/chamados/services/materializacao.py:127`); contador `turnos_sem_par` conta `t.aberto or t.entrada is None` (`app/chamados/services/materializacao.py:221`). | Primeira função é consulta por instante de criação; segunda é geometria por janela de dias, incluindo órfãs, não só aberto (`app/chamados/services/materializacao.py:123`, `app/chamados/services/materializacao.py:219`). |
| Lotes | `CargaDaJanela.turnos` chama o mesmo `_turnos_do_colab_puro` com cópias de batidas (`app/ponto/turnos.py:3036`); carga C6 faz o mesmo por janela pedida (`app/chamados/services/carga_pergunta_viva.py:181`). | Mesmo corpo de geometria; caches têm janela e lifetime próprios (`app/ponto/turnos.py:3028`, `app/chamados/services/carga_pergunta_viva.py:168`). |
| Folha | `eventos_do_fechamento` traduz fechamento em eventos/horas (`app/folha/export.py:402`); `montar_linhas_export` aceita fechamentos injetados ou lê persistidos e usa `classificar_export` (`app/folha/export.py:714`, `app/folha/export.py:717`). | Exportação não reinterpreta Turno. O dinheiro vem do FechamentoMensal; dia pago usa mapa `id(periodo)→dia` se presente e **data civil da entrada se ausente** (`app/ponto/services/dia_pago.py:126`, `app/ponto/services/dia_pago.py:128`, `app/ponto/services/dia_pago.py:132`). Soma por competência arredonda campos a 2 casas (`app/ponto/services/dia_pago.py:559`, `app/ponto/services/dia_pago.py:563`). |

A regra de apresentação completa é: cada batida reclamada por turno recebe `t.data_turno`; qualquer batida remanescente recebe sua `data_local`, por pk (`app/ponto/turnos.py:2949`, `app/ponto/turnos.py:2954`). Portanto “mesmo campo” não autoriza igualar `data_turno`, dia civil, instante, minuto realizado e hora paga (`app/ponto/turnos.py:43`, `app/ponto/services/espelho.py:398`, `app/ponto/services/dia_pago.py:128`).

Chamadas diretas adicionais a `parear_turnos` com possibilidade de absorção por marco: `turnos_de_batidas` (`app/ponto/turnos.py:1535`), `_turno_aberto_puro` (`app/ponto/turnos.py:2278`), reconciliação de fantasmas (`app/ponto/management/commands/reconciliar_fantasmas.py:99`). Chamadas diretas sem marcos de intervalo, portanto sem essa absorção específica: `app/ponto/utils_turno_orfao.py:31`, `app/escala/services/semana_do_posto.py:120`, `app/escala/services/meio_periodo.py:168`, `app/chamados/services/fila_por_causa.py:197`, `app/ponto/services/diagnostico_escala.py:142`, `app/ponto/services/lastro.py:268`; piso de intervalo passa explicitamente `marcos_intervalo=None` (`app/ponto/services/piso_intervalo.py:48`).

Alerta para item 1: HEAD já menciona e executa um passo O95 posterior `_fechar_aberto_na_pausa_sem_volta`; os comentários o listam como terceiro sítio de fechamento (`app/ponto/turnos.py:121`). O anexo pode estar descrevendo o ramo ainda existente, sem contemplar a reparação posterior; conferir corpo e ordem desse pós-processamento (`app/ponto/turnos.py:121`).

## 15/11 20:05 · sessao 269ddcfaaf7d · - · - · origem=Ronald (tmux)

Classes encontradas além da absorção por marco de intervalo:

| Classe | Prova exigida e efeito |
|---|---|
| **Eco de cluster junto de luz acesa, inclusive bordas de entrada/saída** | Batida real dentro do vão da ata, a ≤15 min de alguma luz, recebe papel `X`; vira interna e não abre/fecha turno. Não exige uma volta própria, mas exige ata com entrada e saída, luzes ligadas a batidas reais únicas e minuto não ambíguo. `app/ponto/turnos.py:1655`, `app/ponto/turnos.py:1682`, `app/ponto/turnos.py:1731`, `app/ponto/turnos.py:1742`, `app/ponto/turnos.py:1804`; limite em `app/escala/orfa_classe.py:28`; consumo em `app/ponto/turnos.py:1316`, `app/ponto/turnos.py:1371`. |
| **`S,S` sem volta registrada, com segunda saída perto da borda** | `_marca_intra_duracao` marca a primeira `S` como interna se ela está longe das bordas e encontra `S` posterior dentro de `cont_max_s` que casa borda a ≤90 min. Não há prova `E` de volta, mas há prova de `S` posterior; portanto não é a ausência total de continuidade da O95. `app/ponto/turnos.py:476`, `app/ponto/turnos.py:481`, `app/ponto/turnos.py:485`. |
| **Atribuição de batida a marco de borda por alinhamento** | Na rota sem papel de turno, `_match_marcos` seleciona batidas no envelope `[primeiro marco−tol, último marco+tol]` e usa DP de distância. Batida única pode acender marco sem parceiro; o cluster guard só atua havendo mais de uma. É atribuição de luz, não fabricação de batida nem prova de turno fechado. `app/escala/utils.py:625`, `app/escala/utils.py:633`, `app/escala/utils.py:641`; custo do DP em `app/escala/utils.py:229`; `tol_min=90` em `app/escala/utils.py:668`. |
| **Recorte monetário das bordas pela janela de HE** | Entrada anterior/saída posterior ao limite cadastrado podem ser substituídas pelo marco efetivo para cálculo; minutos externos ficam fora. Parte de pontas reais já informadas, sem criar presença ou volta. Autorização positiva preserva a ponta real. Entrada: `app/ponto/motor_calculo_v2.py:1691`, `app/ponto/motor_calculo_v2.py:1697`, `app/ponto/motor_calculo_v2.py:1702`; regra pura em `app/ponto/janela_he.py:92`. Saída: `app/ponto/motor_calculo_v2.py:1733`, `app/ponto/motor_calculo_v2.py:1739`; regra em `app/ponto/janela_he.py:143`. |
| **Reclassificação de E/S pelo marco** | `tipo_por_marcos` devolve o tipo do único tipo de marco dentro de 30 min; `decidir_tipo_estrito` aceita tipo oposto a ≤10 min quando o próprio está a >30 min. Ambas classificam uma batida real sem exigir parceiro. Não são absorção como intervalo, mas pertencem à classe ampla de inferência de papel pelo marco. `app/ponto/turnos.py:2397`, `app/ponto/turnos.py:2414`, `app/ponto/turnos.py:2424`, `app/ponto/turnos.py:2435`. |

**Tolerância de pontualidade não é absorção de batida:** `aplicar_tolerancia` só retorna atraso/antecipada, comparando instantes reais aos previstos; a implementação usa `TOLERANCIA_CONFORMIDADE_MIN=10`, embora a docstring diga cinco. Não altera horas trabalhadas nessa função. `app/ponto/motor_calculo_v2.py:60`, `app/ponto/motor_calculo_v2.py:1952`, `app/ponto/motor_calculo_v2.py:1959`, `app/ponto/motor_calculo_v2.py:1973`, `app/ponto/motor_calculo_v2.py:2006`. O raio `tol_min` do matcher é atribuição, explicitamente distinto do desconto em folha. `app/escala/utils.py:653`.

Para ESMERIL-SEMÂNTICA:

- `papel_por_minuto` identifica **instante local truncado ao minuto, naive**, não dia civil nem dia do turno; timestamps crus continuam na aritmética. Emissão e consulta fazem a mesma transformação. `app/ponto/turnos.py:1604`, `app/ponto/turnos.py:893`, `app/ponto/turnos.py:904`.
- `papel_por_minuto_da_ata` delimita o **vão do turno por entrada/saída da ata**, não por contagem de batidas no dia civil; evita usar meia ata e colisões entre células vizinhas. `app/ponto/turnos.py:1619`, `app/ponto/turnos.py:1682`, `app/ponto/turnos.py:1730`, `app/ponto/turnos.py:1802`.
- A janela de saída recebe **datetime do marco já resolvido**, pois um `time` não determina o dia da saída em jornada que cruza meia-noite; a entrada recebe `time` e o coloca no dia do instante de entrada. `app/ponto/janela_he.py:20`, `app/ponto/janela_he.py:94`, `app/ponto/janela_he.py:126`.
- `em_intervalo` interpreta **estado**: turno aberto cuja última batida é `S`; não usa relógio nem exige que a volta já exista. Isso explica por que uma cura na absorção precisa preservar o almoço em curso. `app/ponto/turnos.py:2498`, `app/ponto/turnos.py:2503`, `app/ponto/turnos.py:2508`.

## 15/11 20:05 · sessao 5d92ac32c131 · O169-FECHADA · leitura · origem=Code

1. **A regra de idade já saiu do fechamento.** `--dias` limita os dias consultados por `dias_com_cluster`; `pendencias` usa essa janela na abertura, mas calcula `a_fechar` por `_vivos_que_a_premissa_matou(colab)`, sem janela (`app/ponto/services/cluster_espurio.py:40-49`, `:165-173`). O fecho lê a célula do próprio dia e pergunta `premissa_morta`; sem célula/insumos, o fato é `None`, que o catálogo conserva (`app/ponto/services/cluster_espurio.py:66-77`; `app/chamados/catalogo/premissa.py:127-134`). O ato conta somente os fechamentos aceitos pela porta (`app/ponto/services/cluster_espurio.py:187-191`).

2. **`fechados_por_idade` não está implementado com esse nome; existe outra medida.** O command usa `contador_cluster_encerrado_sem_lastro`, imprime seu JSON e retorna rc 2, em `--alarme`, quando há pendências ou divergência do esperado (`app/ponto/management/commands/detectar_cluster_espurio.py:92-104`).

   Esse contador seleciona cluster em `ENCERRADOS`, via `RECONCILIAR`, e consulta `_leitura_da_celula`, sem copiar a regra da assinatura; retorna `numero`, `esperado=0`, universo e exclusões (`app/ponto/services/cluster_espurio.py:88-98`, `:107-150`). **Ele não prova fechamento por idade**: inclui também decisão administrativa via reconciliar, e o próprio serviço declara essa limitação (`app/ponto/services/cluster_espurio.py:228-230`). A trilha de fechamento registra status/via, sem motivo idade versus lastro; não há função causal demonstrada que responda “foi fechado por idade” (`app/chamados/models.py:292-295`; `app/docs/RELATO.md:1439`).

   O delta de publicação permanece identificável: o consumo/alarme já está no command, mas esse trecho não produz carimbo persistente de negócio (`app/ponto/management/commands/detectar_cluster_espurio.py:92-104`). O canal existente do vigia do fio grava `censo` e `esperado`; o leitor compara as chaves do próprio carimbo, e o gerador do ESTADO publica suas divergências (`app/chamados/management/commands/vigiar_fio_morto.py:240-263`; `bin/fio_estado.py:121-126`, `:191-194`; `bin/gerar_estado.py:260-261`). Encaixe específico aprovado do cluster nesse carimbo: **[nao verificado]**.

   Os leitores atuais são:

   - `config/crons.py` agenda `--alarme` às 06:24 e envolve comandos em `cron_run.sh` (`app/config/crons.py:598`, `:1498-1509`).
   - O runner grava o rc em `placar.jsonl`, produz carimbo de conclusão para **0 ou 2** e propaga o rc; esse carimbo contém conclusão, não o contador (`bin/cron_run.sh:11-25`, `:38`). O encadeador espera esses arquivos (`bin/encadeado.sh:81-94`).
   - `pipeline_placar` lê rc 2 como `alarmes` e exibe `ALARME`, porém essa situação não entra na lista que provoca o rc 2 do próprio pipeline (`app/core/management/commands/pipeline_placar.py:61-81`, `:124-132`, `:158-161`). Seus leitores explícitos de contadores do carimbo cobrem `fio_mudo_alvos` e `nunca_bateu_sem_adesao`, sem cluster (`:85-106`, `:135-136`).

3. **O teste existente que morde a regressão é o RED dos 15 dias:** exige ausência em `a_fechar` e chamado ainda vivo com assinatura; o helper executa `pendencias` e `reconciliar_colab` reais (`app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:56-62`, `:74-77`). A contraprova exige fechamento fora da janela quando o lastro não tem assinatura (`:80-83`).

   O mutante literal `fechar_por_idade` **já existe** e aponta para `test_vivo_de_20_dias_com_assinatura_segue_vivo_apos_o_cron`; o harness exige falha de asserção e nenhum erro de infraestrutura (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:222-225`, `:180-192`, `:286-302`). Resultado executado nesta leitura: **[nao verificado]**.

4. **B3 aberto no registro documental:** o BACKLOG diz `B3 DESENHO` e o topo do RELATO ainda declara O169-FECHADA em B3 (`app/docs/BACKLOG.md:2121`; `app/docs/RELATO.md:4`). Estado efetivo do disjuntor nesta execução: **[nao verificado]**.

   O bloqueio implementado é **próximo verificador final e próxima volta Codex verificador**, até liberação por DESENHO/ERRO-BOBO válido; dispara na primeira reprovação final ou terceira volta Codex procedente (`bin/disjuntor.py:1007-1014`, `:1025-1037`). O desenho registrado trata da responsabilidade pelo legado, via de tranca/trilha e reabertura pela porta — assunto além do contador nominal (`app/docs/RELATO.md:1399`; `app/docs/BACKLOG.md:2121`).

## 15/11 20:05 · sessao ec64280c8cd0 · - · - · origem=Ronald (tmux)

Os registros demonstram pouso do núcleo da E1, mas não encerramento integral dos complementos; a grade ainda registra E1-ENTRADAS como `PARADA (último estado escrito)` e mantém 16 complementos. (`app/docs/RELATO.md:1675`; `app/docs/GRADE-MESTRA.md:106`; `app/docs/GRADE-MESTRA.md:683`)

| Data documental | Hash | Prova |
|---|---|---|
| 08/11 18:1x | `297499b8` | Volta 15g, aval E1-MATRIZ-TODAS-AS-PORTAS, RED 8/20, GREEN 313. `app/docs/RELATO.md:1701`; data em `app/docs/PROMPTS.md:1992`. |
| 08/11, depois do aval 18:1x | `5c875ee3` | 15g ampliada, **squash de `297499b8`**, censo P1–P7. `app/docs/RELATO.md:1702`; horário exato [não verificado]. |
| 08/11, antes de 19:23 | `eabbfc5a` | Merge6 = `e2f865f5 + 5c875ee3`; **não certificou**, suite interrompida 19:23 pelo PARE-P4. `app/docs/RELATO.md:1702`; `app/docs/RELATO.md:1704`. |
| 08/11, depois de 19:23 | `6dee2010` | Volta 15h, recusa de destino de outro colaborador. `app/docs/RELATO.md:1705`; horário exato [não verificado]. |
| 08/11, depois da 15h | `b0e948d8` | Merge7 = `e2f865f5 + 6dee2010`, certificado com 14.536 testes e sombra verde. `app/docs/RELATO.md:1705`; `app/docs/RELATO.md:1706`. |
| 08/11 21:15 | `a8a692f5` | **Pouso**, local sem push naquele momento. `app/docs/RELATO.md:1675`. |
| 08/11 22:46 | `a8a692f5` | **PRONTO FIO-E0-V4-E1 NO AR**, deploy e smoke documentados. `app/docs/RELATO.md:1674`. |
| 08/11 23:12 | `a5df8ed5` | Reescrita das mensagens: árvore igual a `a8a692f5`, 19 hashes alterados. Mapa citado está em logs, não validado nesta cópia. `app/docs/RELATO.md:1673`. |
| 09/11 00:5x / 01:5x | `45cf0e53` / `7aa48401` | Pouso da **E1-FILA** por merge `--no-ff 45cf0e53`, commit `7aa48401`; no ar 01:5x. `app/docs/RELATO.md:1679`; `app/docs/RELATO.md:1680`; `app/docs/BACKLOG.md:1965`. |
| 09/11 03:3x | `e9bf5de2` / `89df8c52` | **E1-FILA-V2-2-TRAVA também pousou**, merge `--no-ff e9bf5de2`, commit `89df8c52`, deploy e smoke documentados. `app/docs/RELATO.md:1633`; `app/docs/BACKLOG.md:1972`. |

O que ficou separado:

- Cinco grupos em competência exportada ficaram fora do apply, retidos pelo aval de 08/11. `app/docs/BACKLOG.md:855`.
- RETRATADA-CONVERSA-E-REABRIR; E1-A2-VAZIO-MEDIR; E1-RECUSAS-SEM-CONTADOR; E1-DUPLICATA-SOBRE-RETRATADA continuam em filas próprias. `app/docs/BACKLOG.md:857`; `app/docs/BACKLOG.md:858`; `app/docs/BACKLOG.md:859`; `app/docs/BACKLOG.md:860`.
- E1-FILA-V2-3-A-6, OPUS6-FILA, V2-2-OUTRAS-PORTAS e V2-2-RESIDUOS continuam registrados como resíduos; **V2-2-TRAVA não deve ser descrita como inteira pendente**, pois pousou. `app/docs/BACKLOG.md:1970`; `app/docs/BACKLOG.md:1971`; `app/docs/BACKLOG.md:1972`; `app/docs/BACKLOG.md:1973`; `app/docs/BACKLOG.md:1974`.
- BUG-PLANTIO-NOTURNO-MAIS-1 e ABSOLVIDA-PELA-FOLGA foram anexados posteriormente como **complementos** da E1-ENTRADAS; o segundo está `ABERTA, medido, nao construir` em 12/11. `app/docs/BACKLOG.md:2086`; `app/docs/BACKLOG.md:2087`.
- O apply limpo está documentado: Ronald aplicou quatro migrações, dry seguinte zero, sem exportadas, com reversão anterior. Isso é prova documental da cópia, sem verificação de produção. `app/docs/RELATO.md:1682`.

Não encontrei os termos exatos solicitados em RELATO-ARQUIVO/TICKETS [não verificado]. A linha ABERTA da E1-ENTRADAS permanece literalmente no BACKLOG, apesar dos pousos posteriores documentados. `app/docs/BACKLOG.md:1964`.

## 15/11 20:05 · sessao 5c62d999d000 · - · - · origem=Ronald (tmux)

- **1(e) PROCEDE — o delta literal está vencido.** O BACKLOG ainda pede `fechados_por_idade`, mas o RELATO explica que idade não fica registrada na fonte e que o contador possível mede encerrados com assinatura viva (`app/docs/BACKLOG.md:2121`; `app/docs/RELATO.md:1439`). Esse contador já existe como `cluster_encerrado_sem_lastro`, explicitamente “medida, não escritor nem novo juiz”, e já alimenta o alarme do vigia (`app/ponto/services/cluster_espurio.py:88`, `app/ponto/services/cluster_espurio.py:91`, `app/ponto/management/commands/detectar_cluster_espurio.py:92`, `app/ponto/management/commands/detectar_cluster_espurio.py:100`). O RELATO registra pouso v5, apply dos três e DRY seguinte vazio; situação atual de produção é [nao verificado] (`app/docs/RELATO.md:1164`).

- **1(e), classificação: CONFERE no sentido funcional de medição.** O contador apenas consulta chamados/células e devolve número, esperado e universo (`app/ponto/services/cluster_espurio.py:107`, `app/ponto/services/cluster_espurio.py:126`, `app/ponto/services/cluster_espurio.py:148`). Isso não comprova que exista delta novo a construir na “vaga de instrumento”: a implementação já está na cópia (`app/ponto/management/commands/detectar_cluster_espurio.py:92`). A regra de contrato para instrumento novo permanece escrita (`CLAUDE.md:394`).

- **3, O95 PROCEDE — falta definir o objeto atual do DIFF.** A candidata do BACKLOG é impedir absorção sem uma volta `E`, consta como revertida e movia outros colaboradores sem explicação (`app/docs/BACKLOG.md:1426`). O HEAD contém outra cura O95: preserva a absorção e fecha o turno quando há fato posterior, explicando que impedir absorção quebrava onze selos (`app/ponto/turnos.py:1106`, `app/ponto/turnos.py:1119`, `app/ponto/turnos.py:1126`). O corpo implementa a condição e o pareador chama esse passo (`app/ponto/turnos.py:1171`, `app/ponto/turnos.py:1186`, `app/ponto/turnos.py:1502`). O arquivo histórico também registra O95 já no código e gravado parcialmente atualizado (`app/docs/RELATO-ARQUIVO.md:4370`). Portanto “DIFF de frota” não determina se compara candidata revertida, cura presente ou deriva do gravado; o motor exige uma `mutacao` definida e distingue GRAVADO/HOJE/NOVO (`app/ponto/services/diff_frota.py:7`, `app/ponto/services/diff_frota.py:45`, `app/ponto/services/diff_frota.py:86`).

- **4, O95: CONFERE para censo/ensaio; PROCEDE se virar apply ou cura implicitamente autorizada.** É domínio de dinheiro: a cura muda fechamento do turno e o motor chama `parear_turnos` (`app/ponto/turnos.py:1186`; `app/ponto/motor_calculo_v2.py:503`). O DIFF real calcula em `somente_leitura=True`, dentro de transação desfeita; isso não é apply persistente (`app/ponto/services/diff_frota.py:72`, `app/ponto/services/diff_frota.py:84`, `app/ponto/services/diff_frota.py:89`). Apply de competência não exportada exige reversão anterior, exportada intacta e prova de frota; mover exportada exige `!` e porta própria (`CLAUDE.md:406`, `CLAUDE.md:411`). O aval fornecido só manda leitura/DIFF e proíbe exportada [nao verificado: anexo sem arquivo:linha nesta cópia].

- **4, O169: CONFERE se restrito ao contador; apply existe e é outra ação.** `--alarme` força `aplicar=False`; o caminho manual `--apply` chama o reconciliador, e `--legado --apply --esperados` chama tratamento que escreve (`app/ponto/management/commands/detectar_cluster_espurio.py:55`, `app/ponto/management/commands/detectar_cluster_espurio.py:69`, `app/ponto/management/commands/detectar_cluster_espurio.py:80`). “Só contador” não abrange esses caminhos.

- **4, VINCULO: CONFERE para censo de leitores; não autoriza aplicar a obra histórica.** A linha mantém dependência do censo antes do tripwire e contém autorização antiga específica, diferenças em exportada e outro caso dependente de `!` (`app/docs/BACKLOG.md:1182`). Apply retroativo continua no domínio sujeito a aval; exportada segue as condições da regra DINHEIRO (`CLAUDE.md:404`, `CLAUDE.md:411`).

## 15/11 20:05 · sessao 845fff82b2b5 · - · - · origem=Ronald (tmux)

A implementação pedida existe na cópia:

| Elemento | Prova no código |
|---|---|
| Catraca das quatro classes | `app/chamados/models.py:2707` define `trava_na_ordem`; trava Colaborador em `:2744`, DisputaSupervisao em `:2753`, ChamadoColaborador em `:2766`, PerguntaDisputa em `:2770`, cada grupo ordenado por pk. |
| Catraca AST, sem allowlist | `app/chamados/tests/test_e1_v7_catracas.py:71` enumera as quatro classes; `:85` fixa o único dono autorizado; `:387` exige zero aquisição de trava fora dele; `:392` contém teste de sanidade que renomeia o dono e verifica que detecta as quatro classes. |
| Decisão append-only | `app/chamados/models.py:3982` define `DecisaoPergunta`; `:3969` bloqueia update/delete/bulk_update; `:4046` bloqueia salvar linha existente; `:4051` bloqueia apagar. FK PROTECT em `:4014`. |
| Escritor único e lâmpada/evento | `app/chamados/models.py:4086` define `registrar`; `:4095` abre atomic; `:4096` insere novas decisões e `:4097` acorda células no mesmo ato. `:3314` implementa a emissão de EventoCelula, pelo escritor único `emitir_evento_celula` em `:3331`. |
| Preservação do legado antes de sobrescrever | `app/chamados/models.py:4057` define `linhas_da_projecao_sem_linha`; `:4074` e `:4078` verificam correspondência de ato/instante antes de gerar resposta/validação faltante. |
| Backfill | `app/chamados/management/commands/backfill_decisoes_pergunta.py:425` oferece `--aplicar`; `:210` atomic; `:217` trava todos os colaboradores antes das demais classes; `:231` trava disputas/perguntas; `:324` registra silêncio e `:331` retrata duplicatas pela porta. `:344` provoca rollback integral quando há recusa. |
| Guardas de identidade no banco | `app/chamados/migrations/0053_pergunta_unica_por_familia.py:36` recusa migração com colisões; `:57` cria índice parcial de MARCO e `:62` de BATIDA. Horizonte literal `2026-09-21` em `:28`. |
| Testes de preservação e imutabilidade | `app/chamados/tests/test_e1_entradas.py:55` resposta nova conserva anterior; `:64` verifica duas linhas e instante; `:78` verifica recusa de edição/exclusão; `:185` catraca de escritor único. |

Limites relevantes:

- O backfill de respostas/validações da projeção é **somente medido pela E1**, com apply reservado a outro ato junto da troca do leitor: `app/chamados/management/commands/backfill_decisoes_pergunta.py:3-13`. Há preservação oportunista nos escritores atuais: `app/chamados/models.py:4057-4082`.
- Exportada fica fora **por grupo inteiro**, sem bloquear os grupos limpos: `app/chamados/management/commands/backfill_decisoes_pergunta.py:169-178`; CLI lista o diff em `:442` e termina com código 3 se restarem grupos exportados em `:620-628`.
- A fila E4 explicita o passo posterior: “E4b BACKFILL C3.2-C3.9 não exportada” em `app/docs/BACKLOG.md:1991`. Isso evita tratar o apply integral da projeção como entrega presumida da E1.
- A catraca AST declara limites de cobertura, incluindo SQL dinâmico, certas escolhas por min/sorted e escrita via helpers: `app/chamados/tests/test_e1_v7_catracas.py:36-45`.
- Existência dos testes foi lida; execução e resultados atuais **[não verificado]**. Aplicação efetiva do backfill em produção **[não verificado]**. [nao verificado]

_5769 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
